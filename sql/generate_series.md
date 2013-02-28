!SLIDE subsection
# generate_series(start,end,step)
    @@@sql
    generate_series(1, 1000, 10)

.notes It's a function that makes an incrementing table of values. Use it anywhere you're trying to generate test data.

!SLIDE
## basic step functions
    @@@sql
    SELECT generate_series(1, 3);
    SELECT generate_series(1, 10, 2);

!SLIDE 
## time series work
    @@@sql
    SELECT generate_series(
      now() - '1 week'::interval, now(),
      '1 hour'::interval);

## you can also ignore the values
    @@@sql
    SELECT random() FROM 
      generate_series(1, 3);

!SLIDE
## Further reading
http://www.postgresql.org/docs/9.2/static/functions-srf.html
