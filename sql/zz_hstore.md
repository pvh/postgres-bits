!SLIDE subsection
# hstore
## string dictionary in a column
    @@@SQL
    SELECT ('location => "Tora Bora",
             pressure => 100,
             depth => 30,
             witnessed=>true')::hstore

.notes It's a powerful key-value store that lives in a column. It supports full, generalized indexes out of the box and it's widely supported, including native support in Rails 4. Good for metrics, sparse data-sets, migration-free schema adaptation

!SLIDE
## hstore syntax
    @@@sql
    INSERT INTO events (time, attrs) values
       now(),
       ('location => "Tora Bora", 
         pressure => 100,
         depth => 30, witnessed=>true');

!SLIDE
## look up particular values
    @@@sql
    SELECT * FROM reports 
      WHERE attrs->'project'='Condor';

!SLIDE
## add a key
    @@@sql
    UPDATE products SET attrs = 
       attrs || ('witnessed' => 'false');

!SLIDE
## search for rows with a key
    @@@sql
    SELECT * FROM reports
      WHERE attrs ? 'pressure';

## or a subdocument
    @@@sql
    SELECT * FROM reports WHERE attrs @>
     ('pressure => 100, 
       witnessed => true');

!SLIDE
## calculate statistics
.notes This query will give you the number of times each key shows up in all rows in the table.
    @@@sql
    SELECT key, count(*) FROM
      (SELECT (each(h)).key FROM reports) 
        AS stat
    GROUP BY key
    ORDER BY count DESC, key;

!SLIDE
## generalized index
.notes Allows for efficient arbitrary subdocument querying but expensive to maintain.
    @@@sql
    CREATE INDEX events_attrs
      ON events USING GIN(attrs);

!SLIDE bullets
# the catch
* string values only (cast in Ruby/SQL)
* no nesting

!SLIDE
## Further reading

[Postgres Manual: F.16. hstore](http://www.postgresql.org/docs/9.2/static/hstore.html)

[Schemaless SQL slides on hstore](http://plv8-pgconfeu.herokuapp.com/#29)

[Blog on using hstore with Rails 4 from Kevin Faustino](http://blog.remarkablelabs.com/2012/12/a-love-affair-with-postgresql-rails-4-countdown-to-2013)
