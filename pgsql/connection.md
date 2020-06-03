# 🔥PostgreSQL

<img alt="postgres" width="700px" src="/assets/images/postgresql.png" />

### ✳How to install PostgreSQL?

> [How to install PostgreSQL on Windows](https://www.postgresqltutorial.com/install-postgresql/)

> [How to install PostgreSQL on Ubuntu](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-postgresql-on-ubuntu-18-04)

### ✳How to allow remote connections?

<img alt="pgsql config" width="700px" src="/assets/images/config_postgres.png" />

1. Search for **pg_hba.conf** file.

```bash
find / -name "pg_hba.conf"
```

2. Add following line, save and close file.

```bash
host    all             all             0.0.0.0/0               trust
```

3. Search for **postgresql.conf** file.

```bash
find / -name "postgresql.conf"
```

4. Open and search for **listen_addresses**. Uncomment and update following value. Save and close file.

```bash
listen_addresses = '*'
```

5. Restart PostgreSQL

```bash
sudo service postgresql restart
```

[🌐 Documentation](https://docs.boundlessgeo.com/suite/1.1.1/dataadmin/pgGettingStarted/firstconnect.html#allowing-remote-connections)
