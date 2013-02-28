# UUIDs

Stop using numbers as IDs. Stop it right now.

Everyone has had a database where their primary key has become a big problem. Eventually you run out of space, or get into conflicts due to data mangling, sharding, or some other kind of problem.

Just. Use. UUIDs.

## Great for

tables
foreign keys

## Examples

CREATE EXTENSION "uuid-ossp";
CREATE TABLE t (uuid uuid PRIMARY KEY DEFAULT uuid_generate_v4(), name text);
https://dataclips.heroku.com/hgidwadijlyvxxudvsppuwlvwlnm

## Further reading

http://www.postgresql.org/docs/9.2/static/uuid-ossp.html