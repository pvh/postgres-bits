# Arrays

Instead of joining in tables, sometimes it's most convenient, performant, or intuitive to simply use arrays to model a list of data. An array can be multidimensional but can only hold one type.

## Great for

array_agg() in GROUP BY clauses
using as an adhoc table in a query
storing tags in an indexed way

## Examples

select array_agg(name) from agents group by affiliation;
select (array['hi', 'there', 'everyone'])[random()*2 + 1]
select name from agents where tags @> array['arrears', 'probation'];
https://dataclips.heroku.com/ocdsqenqybkpuyhsyhmectulykhf

## Further reading

http://www.postgresql.org/docs/9.2/static/arrays.html
http://www.postgresql.org/docs/9.2/static/functions-array.html
