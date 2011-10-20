!SLIDE 
# A quick demo

!SLIDE code commandline incremental
# Get a database

    $ heroku addons:add heroku-postgresql
    -----> Adding heroku-postgresql to myapp... done
           Attached as HEROKU_POSTGRESQL_CHARCOAL
           The database should be available in 3-5 minutes
           Use `heroku pg:wait` to track status

!SLIDE code commandline incremental
# ... connect to it

    $ heroku pg:psql CHARCOAL
    Connecting to HEROKU_POSTGRESQL_CHARCOAL... done
    psql (9.0.4, server 9.0.5)
    SSL connection (cipher: DHE-RSA-AES256-SHA, bits: 256)
    Type "help" for help.

    dx1s0triskofm4=>

!SLIDE code commandline incremental
# Set up replication...

    $ heroku addons:add heroku-postgresql --follow CHARCOAL
    -----> Adding heroku-postgresql to myapp... done
           Attached as HEROKU_POSTGRESQL_WHITE
           Follower will become available for read-only queries
           Use `heroku pg:wait` to track status

!SLIDE code commandline incremental

# ... and you're done.
    $ heroku pg:psql WHITE
    Connecting to HEROKU_POSTGRESQL_WHITE... done
    psql (9.0.4, server 9.0.5)
    SSL connection (cipher: DHE-RSA-AES256-SHA, bits: 256)
    Type "help" for help.

    dx1s0triskofm4=> SELECT pg_is_in_recovery();
     pg_is_in_recovery 
    -------------------
     t
    (1 row)

!SLIDE code commandline incremental

# Take a backup...

    $ heroku pgbackups:capture CHARCOAL

    HEROKU_POSTGRESQL_CHARCOAL  ----backup--->  b001

    Capturing... done
    Storing... done

!SLIDE code commandline incremental

# ... or daily backups...

    $ heroku addons:upgrade pgbackups:daily
    -----> Upgrading pgbackups:daily to new-on-rdio... done
           Plan upgraded

!SLIDE code commandline incremental

# and restore a backup.

    $ heroku pgbackups:restore CHARCOAL --confirm myapp

    CHARCOAL  <---restore---  b001 (most recent)
                              HEROKU_POSTGRESQL_CHARCOAL
                              2011/10/19 06:37.31
                              1.2KB

    Retrieving... done
    Restoring... done

!SLIDE

# Or use the [website](http://postgres.heroku.com)
