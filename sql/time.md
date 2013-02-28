# Time

Postgres has amazing time support. In general, you want to use TIMESTAMPTZ and not TIMESTAMP. Intervals are great. There's lots of flexibility in input formats.Postgres is extremely careful so you can see things like "2 months, 83 days" if you sum up times. Use JUSTIFY to fix that.

## Great for

everything
really, everything

## Examples

CREATE TABLE things (when timestamptz, what text);
CREATE TABLE events (
  uuid uuid default uuid_generate_v4(),
  when timestamptz default now(),
  event hstore);
SELECT now();
SELECT now() - '1 week'::interval;
SELECT 'Jan 1 2013'::date;
SELECT '1 year'::interval * random();
SELECT generate_series(now() - '1 year'::interval, now(), '1 week')
WITH days_and_months AS (
  SELECT *, date_trunc('month', generate_series) as month
  FROM generate_series(now() - '1 year'::interval, now(), '1 day')
)
SELECT month::date, count(*) from days_and_months GROUP BY month ORDER BY month;

## Further reading

http://www.postgresql.org/docs/9.2/static/datatype-datetime.html
http://www.postgresql.org/docs/9.2/static/functions-datetime.html
