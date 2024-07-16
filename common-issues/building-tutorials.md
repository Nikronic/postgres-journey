The official documentation works with examples from the tutorials provided in the source code of the PostgreSQL, and users need to download and build them.

To do so,
1. download the tutorials from https://www.postgresql.org/ftp/source/
2. build the tutorials by doing `cd src/tutorials && build`

>[!error] `complex.c:10:10: fatal error: postgres.h: No such file or directory`[^1]
>If you encounter the aforementioned error, you need to either uses the local binaries or install system wide packages. Assuming you have installed PostgreSQL version `XX` (e.g., `16`):
>
>`apt install postgresql-server-dev-XX`

[^1]: https://stackoverflow.com/q/56724622/7606121
