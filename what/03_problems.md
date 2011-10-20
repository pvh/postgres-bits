!SLIDE subsection

# It's not all good news
## (There's always a catch)

!SLIDE bullets incremental

# No Superuser
* Postgres' biggest foot-gun
* Usable to break monitoring
* To delete cluster files
* Create unreplicable situations

!SLIDE bullets incremental

# No Superuser
* required for pg\_cancel\_backend() (9.2?)
* installing all extensions and PLs

!SLIDE bullets incremental

# Limited configurability
* We provide tuned postgresql.conf
* and configure pg_hba.conf
* no user access so, no logging configuration (9.2?)

!SLIDE bullets incremental

# No role creation
* In exchange, we manage roles
* Roles are a poor abstraction

!SLIDE

# Limited version deployment
We ship 9.0; when 9.1 becomes default, we won't commission new 9.0 dbs anymore.

!SLIDE

# High-availability
We do not have an offering yet.


