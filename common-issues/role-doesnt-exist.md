>[!info] When creating a DB right after a clean installation of postgres

I got the following error: 
>[!error] 
>createdb: error: connection to server on socket "/var/run/postgresql/.s.PGSQL.5432" failed: FATAL:  role "root" does not exist

Based on the documentation on, "This will happen if the administrator has not created a PostgreSQL user account for you. You will need to become the operating system user under which PostgreSQL was installed to create *the first user account*". 
So, because I installed it with the default user of my linux OS, apparently I am "root". To fix this:
>[!success] I need to create a postgres user for myself.

>[!note]
>PostgreSQL user accounts are distinct from operating system user accounts.

To do so,
1. First check your username on the OS you are using: `echo $USER`. Let's say `root`
2. Login as default PostgreSQL user (`postgres`): `sudo -u postgres -i`
3. As postgres user. Add a new database user using the `createuser` command: `createuser --interactive`
4. exit `postgres` user and continue your work.

Commands mentioned:

| command    | what it does                                  |
| ---------- | --------------------------------------------- |
| `createdb` | created a db                                  |
| `dropdb`   | removes a db and all associated files with it |

References:
1. https://www.postgresql.org/docs/current/tutorial-createdb.html
2. https://stackoverflow.com/a/45800532/7606121
---
