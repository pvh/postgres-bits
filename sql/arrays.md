!SLIDE subsection
# arrays
     ARRAY['oh', 'hello', 'there']

.notes Instead of joining in tables, sometimes it's most convenient, performant, or intuitive to simply use arrays to model a list of data. An array can be multidimensional but can only hold one type.

!SLIDE
## show me the contents of my groups
    @@@ sql
    select array_agg(name) from agents 
       group by affiliation;

!SLIDE
## pick me a random value
    @@@ sql
    select (array['hi', 'there', 
               'everyone'])[random()*2 + 1]

!SLIDE
.notes This can be made blindingly fast with a GIN index.
## find a row that includes some tags
    @@@ sql
    select name, tags from agents where 
      tags @> array['arrears', 'probation']

!SLIDE
## unnest the array into rows
    @@@ sql
    select unnest(tags) as tag from agents 
       where name = 'Sterling Archer';

.notes https://dataclips.heroku.com/ocdsqenqybkpuyhsyhmectulykhf

!SLIDE
## Further reading

http://www.postgresql.org/docs/9.2/static/arrays.html
http://www.postgresql.org/docs/9.2/static/functions-array.html
