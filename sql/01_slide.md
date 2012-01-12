!SLIDE subsection

# SQL sandbox

!SLIDE

# the question:
## How can I reason about the progress of a 
## state machine which logs its state changes?

!SLIDE

# the setup:
## A table containing state change events.

!SLIDE

#the plan:
##calculate things in the database!

!SLIDE

All of the queries on slides ahead are designed to let you copy/paste them into any psql console to follow along at home.

!SLIDE code

# 2, 4, 6, 8...

    @@@ sql
    SELECT generate_series(1, 10, 2);

!SLIDE code

# Let's use times instead
    @@@ sql
    SELECT generate_series(
      now() - '1 week'::interval,
      now(),
      '4 hour'::interval);

!SLIDE code

# add some random app_ids...
    @@@ sql
    SELECT 
      floor(random() * 20) as app_id,
      generate_series(1, 5) as entered_at;

!SLIDE code

# random state names, too...
## warning: SQL arrays are 1-indexed
    @@@ sql
    SELECT (ARRAY[
      'starting', 'running', 'offline'
    ])[floor(random() * 3) + 1];

[Postgresql Manual, 8.14](http://www.postgresql.org/docs/current/static/arrays.html)

!SLIDE

# Put it all together and we have...

!SLIDE

    @@@ sql
     SELECT
       floor(random() * 20) as app_id, 
       (ARRAY[
         'starting', 'running', 'offline'
       ])[floor(random() * 3) + 1] as state,
       generate_series(
         now() - '1 year'::interval, 
         now(), 
         '2 hour'::interval) as entered_at;
    
!SLIDE

# Shove that in a table

!SLIDE

    @@@ sql
    CREATE TABLE events AS
     SELECT
       floor(random() * 20) as app_id, 
       (ARRAY[
         'starting', 'running', 'offline'
       ])[floor(random() * 3) + 1] as state,
       generate_series(
         now() - '1 year'::interval, 
         now(), 
         '2 hour'::interval) as entered_at;

[Postgres Manual, SQL Commands Reference: CREATE TABLE AS](http://www.postgresql.org/docs/current/static/sql-createtableas.html)

!SLIDE

## Are you scared yet?

!SLIDE center

![Kitteh](kitteh.jpg)

!SLIDE

## Grab your hats,
## here we go again.

!SLIDE

## Data's in, let's get down to querying

!SLIDE code commandline incremental

# Here's something new

    $ SELECT events from events;
                        events                    
    ----------------------------------------------
     (1,offline,"2011-01-11 02:03:31.86156+00")
     (17,starting,"2011-01-11 04:03:31.86156+00")
     (19,offline,"2011-01-11 06:03:31.86156+00")

It's the whole record packed into a single "composite type".
You get one free when you make a table, <i>but you can make your own</i>.
[Postgres Manual, 8.15](http://www.postgresql.org/docs/current/static/rowtypes.html)

!SLIDE code

# Extracting data

    @@@ sql
    => SELECT (e).state FROM (
        SELECT events as e
        FROM events
       ) as sub;

## () disambiguates CTs from tables

!SLIDE

# Next up: Window functions!

!SLIDE

## Window functions let you relate between rows!
(I do not know how I ever lived without this.)

!SLIDE code

## If we have

    @@@ sql
    => SELECT data
       FROM generate_series(1, 4) as data;
     data 
    ------
        1
        2
        3
        4
    (4 rows)

!SLIDE code

## get the next row with

    @@@ sql
    => SELECT data, 
         lead(data, 1) OVER (ORDER BY data)
       FROM generate_series(1, 4) as data;
     data | lead 
    ------+------
        0 |    1
        1 |    2
        2 |    3 
        3 |    4
        4 |     
    (5 rows)
!SLIDE

## But what if we need to split these rows into groups?

!SLIDE code

    @@@ sql
    => SELECT data, 
         lead(data, 1) OVER (
           PARTITION BY (data > 2)
           ORDER BY data)
       FROM generate_series(0, 4) as data;
     data | lead 
    ------+------
        0 |    1
        1 |    2
        2 |     
        3 |    4
        4 |     
    (5 rows)

!SLIDE

# We're going in!

!SLIDE code commandline incremental

# Select all the transitions
  
    $ SELECT                                               \
       (events) as current,                                \
       lead((events), 1) OVER                              \
         (PARTITION BY app_id ORDER BY entered_at) as next \
       FROM events;
                current          |        next               
     ----------------------------+----------------------------
      (0,starting,"2011-01-11")  | (0,running,"2011-01-12")
      (0,running,"2011-01-12")   | (0,offline,"2011-01-18")
      (0,offline,"2011-01-18")   | (0,running,"2011-01-24")

!SLIDE code

## Calculate the duration via a subselect.

    @@@ sql
    SELECT
      (next).entered_at - 
        (current).entered_at as duration 
    FROM (
      SELECT
      (events) as current,
      lead((events), 1) over (
        partition by app_id 
        order by entered_at) as next
      FROM events
    ) as nested;

!SLIDE

Ugh. This is getting ugly.

!SLIDE

Let's use a WITH-expression to make things more readable.

!SLIDE code

    @@@ sql
    WITH
    transitions AS (
      SELECT
      (events) as current,
      lead((events), 1) over (
        partition by app_id 
        order by entered_at) as next
      FROM events
    )

    SELECT *, 
      (next).entered_at - 
        (current).entered_at as duration
    FROM transitions;

!SLIDE

Great, now how long did each app spend in the running state on occasions where it went offline next?

!SLIDE code

    @@@ sql
    WITH
      transitions AS (...),
      du rations AS (
        SELECT *, 
          (next).entered_at - 
            (current).entered_at as duration
        FROM transitions;
      ),
      interesting_transitions AS (
        SELECT * FROM durations
          WHERE (current).state = 'running'
            AND (next).state = 'offline')

    SELECT (current).app_id,
           justify_hours(sum(duration))
             as running_for
    FROM interesting_transitions
    GROUP BY (current).app_id;

!SLIDE code

    @@@ sql
     app_id |   running_for   
    --------+-------------------
          0 | 108 days 10:00:00
          1 | 129 days 08:00:00
          2 | 116 days
          3 | 135 days 02:00:00
    (20 rows)

!SLIDE center

![Cloud](mushroom-cloud.jpg)

!SLIDE

Okay, brain melted yet?

!SLIDE

# RECAP

!SLIDE bullets

# creating datasets
* generate_series(from, to, step)
* CREATE TABLE AS
* floor(random() * 500) for ids

!SLIDE bullets

# querying datasets
* WITH expressions
* window functions
* composite types (row types)

!SLIDE bullets

# data types
* ARRAY[1,2,3]
* casting via value::type
* interval

!SLIDE bullets

# left as an exercise
* \h KEYWORD
* EXPLAIN query
* hstore (k/v columns!)
* full text search

!SLIDE bullets

# fun catalog tables
* pg\_stat\_activity
* pg\_locks
* pg\_stat\_database

!SLIDE bullets

# RTF Postgres M
### (I did)
* Data Types, Ch. 8
* Functions and Operators, Ch. 9
* Overview of PostgreSQL Internals, Ch. 44
* System Catalogs, Ch. 45

!SLIDE

Not bad for a big hash.

!SLIDE

PS: 

Don't develop against SQLite and deploy on PostgreSQL;

that's like testing on Ruby 1.8.6 and deploying on Ruby 1.9.1.
