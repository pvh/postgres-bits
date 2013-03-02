!SLIDE subsection
# window functions
    @@@sql
    SELECT lead(status, 1) OVER 
      ( PARTITION BY agent_uuid 
        ORDER BY when ) 
    FROM agent_status

.notes It's like a souped-up GROUP BY without the grouping.
Often times you need to relate two rows from the same table in ways
that have to do with ordering or grouping their contents. This is a
huge pain to implement correctly in application code and inefficient.

!SLIDE
## determining event duration
    @@@sql
    SELECT when - lag(when, 1) OVER 
        ( PARTITION BY agent_uuid 
          ORDER BY when )
      AS duration
    FROM metrics;

!SLIDE
## find the sixth decile
    @@@sql
    WITH decile as (
      SELECT *, 
         ntile(10) OVER ( ORDER BY score )
    ) SELECT * FROM decile where ntile = 6;

!SLIDE
## tracking state changes
    @@@sql
    SELECT (agent_statuses) as current, 
           lead((agent_statuses), 1) OVER 
             ( PARTITION BY agent_uuid 
               ORDER BY when )
              as next
    FROM agent_statuses; 
    
!SLIDE
## Further reading
[Example dataclip](https://dataclips.heroku.com/dsjnolxgeksipvyhujkgsyhymrmv)

[Postgres Manual: 3.5. Window Functions](http://www.postgresql.org/docs/9.1/static/tutorial-window.html)

[Postgres Manual: 9.19. Window Functions](http://www.postgresql.org/docs/9.1/static/functions-window.html)

[Postgres Manual: 4.2.8. Window Function Calls](http://www.postgresql.org/docs/9.1/static/sql-expressions.html#SYNTAX-WINDOW-FUNCTIONS)

bonus: `$ heroku pg:mandelbrot` (requires [pg-extras](http://github.com/heroku/pg-extras))
