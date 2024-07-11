>[!info] Debian only installs the latest LTS version.
>This means, it is not necessarily the latest *stable* version of postgres[^1] and you need to install it by adding the appropriate APT repo.

>[!success] Add postgress APT repo

To do automated repo[^2]:
```bash
sudo apt install -y postgresql-common  
sudo /usr/share/postgresql-common/pgdg/apt.postgresql.org.sh
```

After this, just do the normal `apt install postgresql`

>[!warning]
>You need to remove the LTS version, i.e., the preinstalled postgresql server.

To remove the LTS version (old version):
1. Purge the default one: `apt purge postgresql-15`
2. Remove unnecessary packages (`postgresql-client-15`) affected by the purge: `apt autoremove`
3. Install the latest: `apt install postgresql`

>[!danger]
>Purging the previous installation will cause the removal of all databases (unless prompted). Although there will a prompt to make sure you are aware of your actions.

>[!info] Test version of client and server
>To test,
>1. just use `psql -V` to check client version, then to test server, 
>2. create a db and log into it: `createdb testdb`, and `psql testdb`
>3. run the following command when connected to `testdb`: `select version();`
>
>![[Pasted image 20240711152313.png]]

[^1]: PostgreSQL is available in all Debian versions by default. However, Debian "snapshots" a specific version of PostgreSQL that is then supported throughout the lifetime of that Debian version.
[^2]: https://www.postgresql.org/download/linux/debian/
