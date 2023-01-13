# PostgreSQL Barman Cheatsheet
This is a pgbarman cheatsheet with reference using [barman website](https://docs.pgbarman.org/release/3.3.0/)

## Pre-Requisites
* PostgreSQL Server

## Installation
* NOTE: Follow this steps only in the Barman Server
1. Update apt package cache
```bash
sudo apt update
```
2. Install barman
```bash
sudo apt install barman
```

## Usage

### Master PostgreSQL to Barman Server

Backing up a master database directly to barman server

1. Create private key for postgres
```bash
# run this on postgres server
ssh-keygen -t rsa
```
2. Copy the public key from postgres
```bash
# run this on postgres server
# copy the output and save it for the later use
cat ~/.ssh/id_rsa.pub
```
3. Create private key for barman
```bash
# run this on barman server
ssh-keygen -t rsa
```
4. Copy the public key from barman
```bash
# run this on barman server
# copy the output and save it for the later use
cat ~/.ssh/id_rsa.pub
```
5. Add the barman public key to postgres authorized keys
```bash
# run this on postgres server
# paste the barman public key to the file
sudo vi ~/.ssh/authorized_keys
```
6. Add the postgres public key to barman authorized keys
```bash
# run this on barman server
# paste the postgres public key to the file
sudo vi ~/.ssh/authorized_keys
```
7. Update barman global configuration
```bash
# run this on barman server
vi /etc/barman.conf
```
put these parameters in the file
```conf
;barman.conf
[barman]
barman_user = barman
configuration_files_directory = /etc/barman.d
barman_home = /var/lib/barman
log_file = /var/log/barman/barman.log
log_level = INFO
compression = gzip
reuse_backup = link ;this will allow incremental backups
backup_method = rsync
archiver = on
```
8. Create configuration file for the backup
```bash
# run this on barman server
vi /etc/barman.d/postgres.conf
```
put these parameters in the file and just update if necessary
```conf
# postgres.conf
[postgres]
description = “PostgreSQL Server”
ssh_command = ssh postgres@<ip address>
conninfo = host=<ip address> user=barman port=5432 dbname=<database name> password=<password>
archiver = on
slot_name = barman
backup_method = rsync
retention_policy_mode = auto
retention_policy = RECOVERY WINDOW OF 7 days
wal_retention_policy = main
```
9. Trust IP Address to postgreSQL server
```bash
# run this on postgres server
sudo vi /etc/postgresql/12/main/pg_hba.conf
```
add this on the file
```bash
# pg_hba.conf
host    all             all             <barman ip address>/32            trust
```
10. Update postgreSQL server configuration file
```bash
# run this on postgres server
sudo vi /etc/postgresql/12/main/postgresql.conf
```
update the listen_address parameters
```conf
# postgresql.conf
listen_addresses = '*'
```
then restart the PostgreSQL server to update configuration
```bash
# run this on postgres server
sudo systemctl restart postgresql
```
11. Setup WAL Archiving on PostgreSQL
```bash
# run this on barman server
# copy the directory for the later use
barman show-server postgres | grep streaming_wals_directory
```
12. Add archiving on postgreSQL server configuration file
```bash
# run this on postgres server
sudo vi /etc/postgresql/12/main/postgresql.conf
```
uncomment/update the parameters below
```conf
archive_mode = on
archive_command = 'test ! -f barman@<ip address>:/var/lib/barman/postgres/streaming/%f && rsync -a %p barman@<ip address>:/var/lib/barman/postgres/streaming/%f' ;change <ip address to barman ip address>
```
then restart the PostgreSQL server to update configuration
```bash
# run this on postgres server
sudo systemctl restart postgresql
```
13. Test the created backup configuration
```bash
# run this on barman server
barman list-server # check if the server is already created
barman check postgres # check whether there is an error in the configuration
# If you see this kind of error do the command below: WAL archive: FAILED (please make sure WAL shipping is setup)
barman switch-wal postgres
barman switch-xlog --force --archive postgres
```
14. Create a base backup
```bash
# run this on barman server
barman backup postgres
```
15. Check the backup
```bash
# run this on barman server
barman list-backup postgres
```
16. Automating the backup using Cron
```bash
# run this on barman server
crontab -e
```
then add this into the file
```cron
* * * * * /usr/bin/barman cron
0 4 * * * /usr/bin/barman backup postgres
```
