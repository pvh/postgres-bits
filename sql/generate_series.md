# generate_series

It's a function that makes an incrementing table of values. Use it anywhere you're trying to generate test data.

## Great for

generating test data
creating sequences of dates or numbers to group against

## Examples

SELECT generate_series(1, 3);
SELECT generate_series(1, 10, 2);
SELECT generate_series(now() - '1 week'::interval, now(), '1 hour'::interval)
SELECT random() FROM generate_series(1, 5);

## Further reading
http://www.postgresql.org/docs/9.2/static/functions-srf.html
