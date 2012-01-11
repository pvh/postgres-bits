!SLIDE subsection

#SQL: going beyond CRUD

!SLIDE

# the question:
## How much time did my application spend in each state?

!SLIDE

# the setup:
## A table containing state change events.

!SLIDE

#the plan:
##calculate things in the database!

!SLIDE

First, let's rummage in the bag of tricks
and make ourselves some simulated data.

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
      generate_series(...) as entered_at;

!SLIDE code

# random state names, too...
## warning: SQL arrays are 1-indexed
    @@@ sql
    SELECT (ARRAY[
      'starting', 'running', 'offline'
    ])[random() * 3 + 1];

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


## Instant* seed data!

!SLIDE

## *: for some value of instant

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

    $ SELECT events from events limit 3;
                        events                    
    ----------------------------------------------
     (1,offline,"2011-01-11 02:03:31.86156+00")
     (17,starting,"2011-01-11 04:03:31.86156+00")
     (19,offline,"2011-01-11 06:03:31.86156+00")
    (3 rows)

It's the whole record packed into a single "composite type".
You get one free when you make a table, <i>but you can make your own</i>.
[Postgres Manual Ch. 8.15](http://www.postgresql.org/docs/9.1/static/rowtypes.html)

!SLIDE code

# Extracting data

    @@@ sql
    => SELECT (e).state FROM (
        SELECT events as E
        FROM events
       ) as sub;

## () disambiguates CTs from tables

!SLIDE

# Next up: Window functions!

!SLIDE

## Window functions let you relate between rows!

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

## But what if some rows are different than others?

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
      (0,running,"2011-01-11")   | (0,running,"2011-01-12")
      (0,running,"2011-01-11")   | (0,starting,"2011-01-18")
      (0,starting,"2011-01-11")  | (0,offline,"2011-01-24")

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

Ugh. This sucks. 

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
      durations AS (...)

    SELECT (current).app_id,
           justify_hours(sum(duration))
             as running_for
    FROM durations
    WHERE (current).state = 'running'
      AND (next).state = 'offline'
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

!SLIDE

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

# honorable mentions
* \h KEYWORD
* hstore (k/v columns!)
* EXPLAIN query
* full text search

!SLIDE bullets

# RTF Postgres M
## (I did)
* Section II (esp Ch 8, 9)
* Section VII (esp Ch 44, 45)