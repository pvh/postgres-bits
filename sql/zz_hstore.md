# HStore

It's a powerful key-value store that lives in a column. It supports full, generalized indexes out of the box and it's widely supported, including native support in Rails 4.

## Great for

metrics
one-off attributes
sparse data-sets
migration-free schema adaptation
comparing tables

## Examples

SELECT 'temperature => 3, pressure => 100, depth => 30, witnessed=>true'::hstore
SELECT 'temperature => 3, pressure => 100, depth => 30, witnessed=>true'::hstore
       @> 'witnessed=>true'
SELECT skeys('temperature => 3, pressure => 100, depth => 30, witnessed=>true'::hstore);
SELECT 'temperature => 3, pressure => 100, depth => 30, witnessed=>true'::hstore ? 'witnessed';
SELECT key, count(*) FROM
  (SELECT (each(h)).key FROM reports) AS stat
  GROUP BY key
  ORDER BY count DESC, key;

# Further reading

http://www.postgresql.org/docs/9.2/static/hstore.html
http://blog.remarkablelabs.com/2012/12/a-love-affair-with-postgresql-rails-4-countdown-to-2013
