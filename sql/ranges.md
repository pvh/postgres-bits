!SLIDE subsection
# ranges
    @@@sql
    daterange(
      '["Jan 1 2013", "Jan 15 2013")'
    ) @> 'Jan 10 2013'::date

.notes Ranges are awesome, especially for time and space applications. You could do a whole talk on ranges. You can create so-called exclusion constraints which can protect against overlapping reservations.

!SLIDE
## containment (`@>`)
    @@@sql
    SELECT 
      daterange('Jan 1 2013', 'Feb 1 2013') 
      @> 
      'Jan 15 2013'::date;

!SLIDE
## overlap (`&&`)
    @@@sql
    SELECT numrange(3,7) && numrange(4,12);

!SLIDE
## union (`+`)
    @@@sql
    numrange(5,15) + numrange(10,20)
    => '[5, 20)'

!SLIDE
## exclusion constraint
    @@@sql
    ALTER TABLE gear_reservations ADD
      EXCLUDE USING gist 
        (reserved WITH =, period WITH &&);

!SLIDE
## Great for

* simplifying crazy range queries
* calendaring and reservation systems

.notes https://dataclips.heroku.com/xsfcgdnzcjqabukvdfuibebsxnwp

!SLIDE
## Further reading

http://www.postgresql.org/docs/devel/static/rangetypes.html
http://www.postgresql.org/docs/9.2/static/functions-range.html