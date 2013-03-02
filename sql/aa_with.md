!SLIDE subsection

# WITH
    @@@sql
    WITH a AS ( SELECT 'a' as a )
      SELECT * FROM a
.notes I do not believe it is possible to comprehend complex queries as a general rule without the use of WITH. You should always use WITH. My usual workflow is to build queries incrementally pushing each new step up into a WITH expression as I go.

!SLIDE
.notes please, please don't ever show someone a query that contains a subselect which isn't in a WITH clause. it's just not worth it.

# WITH changed my life.

!SLIDE
## with gives queries a narrative
    @@@sql
    WITH
      prepared_data AS ( ... )
    SELECT data, count(data), 
           min(data), max(data)
    FROM prepared_data
    GROUP BY data;

!SLIDE
## with expressions are chainable
    @@@sql
    WITH 
      subquery AS ( SELECT 1 ),
      other_query AS ( 
        SELECT * FROM subquery )
    SELECT * FROM subquery, other_query;

!SLIDE
## even recursive
    @@@sql
    WITH RECURSIVE t(n) AS (
      VALUES (1)
    UNION ALL
      SELECT n+1 FROM t WHERE n < 100
    )
    SELECT sum(n) FROM t;

!SLIDE

# caveat:
## WITH expressions are optimization boundaries

!SLIDE
## Further reading
[Example Dataclip](https://dataclips.heroku.com/efbiveiwdjrwrfpykhtomnehgkgn)

[Postgres Manual: 7.8. WITH Queries](http://www.postgresql.org/docs/9.2/static/queries-with.html)
