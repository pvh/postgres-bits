!SLIDE subsection
# LISTEN / NOTIFY
    @@@sql
    LISTEN work_available;
    NOTIFY work_available, '44924';
    SELECT pg_notify('channel', 'message');

.notes Originally designed to help invalidate caches, LISTEN/NOTIFY is an asynchronous inter-connection messaging bus built into Postgres. It's also useful for building database queues (see queue_classic). I'm somewhat surprised nobody has used it to reduce the need for .reload calls in frameworks like ActiveRecord. Notification is delivered asynchronously to any connections that are LISTENing on that channel, including the connection that may have caused the NOTIFY to occur if it is LISTENing on that channel.

!SLIDE bullets incremental
# huh?

* LISTEN on a channel
* NOTIFY messages are delivered asynchronously w/payload
* useful to fan out messages to other clients

!SLIDE
# Great for
* broadcasting events to other clients
* work distribution
* cache busting

!SLIDE
## Further reading

[Postgres Manual: NOTIFY](http://www.postgresql.org/docs/9.2/static/sql-notify.html)
[Postgres Manual: LISTEN](http://www.postgresql.org/docs/9.2/static/sql-listen.html)
[queue_classic: a postgres-based queue](https://github.com/ryandotsmith/queue_classic)

