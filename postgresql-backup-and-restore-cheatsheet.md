# PostgreSQL Backup and Restore Cheatsheet
This is a PostgreSQL Backup and Restore cheatsheet with reference using [postgresql website](https://www.postgresql.org/docs/current/backup.html)

## Pre-Requisites
* PostgreSQL Server

## Usage

### Backing up Local Postgres
1. Login to the postgres user
```bash
sudo su - postgres
```
2. Use pg_dump to backup a database
```bash
# single database
pg_dump <db_name> > <backup_name>.bak
# backing up large database
# pg_dump <db_name> | gzip > <backup_name>.gz
```
3. [Optional] For multiple database backup
```bash
# multiple database
pg_dumpall > <backup_name>.bak
```

### Restoring Locally
1. Login to the postgres user
```bash
sudo su - postgres
```
2. Restore
```bash
# using psql
psql -f <backup_name>.bak
# if restoring for specific database
# psql -d <db_name> -f <backup_name>.bak
```
3. [Optional] Restore using pg_restore
```bash
# only applicable for single database backup
pg_restore -d <db_name> -f <backup_name>.bak
```

### Backing up Remote Posgres
1. Update the neccessary details and run the command
```bash
pg_dump -U <db_user> -W -h <remote_host> -p <remote_port> <db_name> > <backup_name>.bak
```
3. [Optional] For multiple database backup
```bash
# multiple database
pg_dumpall -U <db_user> -W -h <remote_host> -p <remote_port> > <backup_name>.bak
# backing up large database
# pg_dumpall -U <db_user> -W -h <remote_host> -p <remote_port> | gzip > <backup_name>.gz
```

### Restoring Remotely
1. Update the neccessary details and run the command
```bash
# using psql
psql -U <db_user> -W -h <remote_host> -p <remote_port> -f <backup_name>.bak
# if restoring for specific database
# psql -U <db_user> -W -h <remote_host> -p <remote_port> -d <db_name> -f <backup_name>.bak
```
2. [Optional] Restore using pg_restore
```bash
# only applicable for single database backup
pg_restore -U <db_user> -W -h <remote_host> -p <remote_port> -d <db_name> -f <backup_name>.bak
```