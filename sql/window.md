# "Window" Functions

It's like a souped-up GROUP BY without the grouping.

Often times you need to relate two rows from the same table in ways
that have to do with ordering or grouping their contents. This is a
huge pain to implement correctly in application code and inefficient.

## Great for

invoices
site analytics
state machines
any time you need to relate multiple rows

## Examples

SELECT lead(status, 1) OVER ( PARTITION BY agent_uuid ORDER BY when ) FROM agent_status
https://dataclips.heroku.com/dsjnolxgeksipvyhujkgsyhymrmv

## Further reading

http://www.postgresql.org/docs/9.1/static/tutorial-window.html
http://www.postgresql.org/docs/9.1/static/functions-window.html
http://www.postgresql.org/docs/9.1/static/sql-expressions.html#SYNTAX-WINDOW-FUNCTIONS
