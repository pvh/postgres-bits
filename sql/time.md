!SLIDE subsection
# times & dates
    @@@sql
    SELECT now() - '1 week'::interval;

.notes Postgres has amazing time support. In general, you want to use TIMESTAMPTZ and not TIMESTAMP. Intervals are great. There's lots of flexibility in input formats.Postgres is extremely careful so you can see things like "2 months, 83 days" if you sum up times. Use JUSTIFY to fix that.

!SLIDE
# use timestamptz
    @@@sql
    CREATE TABLE events (
      when timestamptz default now(),
      uuid uuid default uuid_generate_v4(),
      event hstore);

!SLIDE
# you can do arithmetic
    @@@sql
    SELECT '1 year'::interval * random();

!SLIDE
# dates are better sometimes
    @@@sql 
    SELECT 'Jan 1 2013'::date;

!SLIDE
# rounding times is useful
    @@@sql
    SELECT date_trunc('week', g)::date;

!SLIDE
# so is generating a series
    @@@sql
    SELECT generate_series(
        now() - '1 month'::interval, 
        now(),
        '1 day')
        
!SLIDE
## Further reading

http://www.postgresql.org/docs/9.2/static/datatype-datetime.html
http://www.postgresql.org/docs/9.2/static/functions-datetime.html
