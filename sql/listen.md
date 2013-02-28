# LISTEN / NOTIFY

Originally designed to help invalidate caches, LISTEN/NOTIFY is an asynchronous inter-connection messaging bus built into Postgres. It's also useful for building database queues (see queue_classic). I'm somewhat surprised nobody has used it to reduce the need for .reload calls in frameworks like ActiveRecord.

Notification is delivered asynchronously to any connections that are LISTENing on that channel, including the connection that may have caused the NOTIFY to occur if it is LISTENing on that channel.

## Great for
broadcasting events to other clients
work distribution
cache busting

## Examples

LISTEN channel;
NOTIFY channel, 'message';
SELECT pg_notify('channel', 'message');

## Further reading

http://www.postgresql.org/docs/9.2/static/sql-notify.html
http://www.postgresql.org/docs/9.2/static/sql-listen.html
https://github.com/ryandotsmith/queue_classic
