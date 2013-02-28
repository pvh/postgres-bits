!SLIDE subsection
# UUIDs
     @@@sql
     ALTER TABLE ADD COLUMN (my_uuid uuid);
       SELECT uuid_generate_v4();

.notes Stop using numbers as IDs. Stop it right now.
Everyone has had a database where their primary key has become a big problem. Eventually you run out of space, or get into conflicts due to data mangling, sharding, or some other kind of problem.
Just. Use. UUIDs.

!SLIDE
## install the extension
    @@@sql
    CREATE EXTENSION "uuid-ossp";

!SLIDE
## use it for your primary keys
    @@@sql
    CREATE TABLE t (
      uuid uuid PRIMARY KEY 
                DEFAULT uuid_generate_v4(), 
      name text);

!SLIDE
## Further reading
.notes https://dataclips.heroku.com/hgidwadijlyvxxudvsppuwlvwlnm

http://www.postgresql.org/docs/9.2/static/uuid-ossp.html