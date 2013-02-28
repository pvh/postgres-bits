# Ranges

Ranges are awesome, especially for time and space applications. You could do a whole talk on ranges. You can create so-called exclusion constraints which can protect against overlapping reservations.

## Great for

Reservations
Calendaring
Spatial applications

## Examples

SELECT tstzrange(now(), infinity);
SELECT daterange('Jan 1 2013', 'Feb 1 2013') @> 'Jan 15 2013'::date;
https://dataclips.heroku.com/xsfcgdnzcjqabukvdfuibebsxnwp

## Further reading

http://www.postgresql.org/docs/devel/static/rangetypes.html
http://www.postgresql.org/docs/9.2/static/functions-range.html