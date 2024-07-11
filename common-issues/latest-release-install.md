>[!info] Debian only installs the latest LTS version.
>This means, it is not necessarily the latest *stable* version of postgres[^1] and you need to install it by adding the appropriate APT repo.

>[!success] Add postgress APT repo

To do automated repo[^2]:
```bash
sudo apt install -y postgresql-common  
sudo /usr/share/postgresql-common/pgdg/apt.postgresql.org.sh
```

After this, just do the normal `apt install postgresql`

>[!note]
>No need to uninstall Debian LTS version

[^1]: PostgreSQL is available in all Debian versions by default. However, Debian "snapshots" a specific version of PostgreSQL that is then supported throughout the lifetime of that Debian version.
[^2]: https://www.postgresql.org/download/linux/debian/
