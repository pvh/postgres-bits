# WITH

I do not believe it is possible to comprehend complex queries as a general rule without the use of WITH. You should always use WITH. My usual workflow is to build queries incrementally pushing each new step up into a WITH expression as I go.

# Great for

Building readable analytics queries.
Avoiding temporary tables, views, or other stateful artifacts.
Recursive insanity.
Any time a human has to read a query.

## Examples

WITH 
  subquery AS ( SELECT 1 ),
  other_query AS ( SELECT * FROM subquery )
SELECT * FROM subquery, other_query;

https://dataclips.heroku.com/efbiveiwdjrwrfpykhtomnehgkgn

## Details

http://www.postgresql.org/docs/9.2/static/queries-with.html
