# DBLink

Ever wanted to join a table across two databases? Yeah, you can do that. Unfortunately, there isn't a good way to do that and keep the credentials private, so it needs to be used in a trusted environment. Unfortunately type determination happens at parsing time, so you need to describe the types yourself in the query.

## Great for

recovering data
migrating data
joining across databases

## Examples

CREATE EXTENSION dblink;
SELECT dblink_connect('postgres://')
CREATE TABLE AS SELECT * FROM dblink('select * from agents') as 
  agents(uuid uuid, name text, birthday date, affliation text);

## Further reading

http://www.postgresql.org/docs/9.2/static/dblink.html
http://www.postgresql.org/docs/9.2/static/contrib-dblink-function.html
