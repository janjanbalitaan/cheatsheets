# PostgreSQL Replication Cheatsheet
This is a postgresql replication cheatsheet with reference using [digital ocean website](https://www.digitalocean.com/community/tutorials/how-to-set-up-physical-streaming-replication-with-postgresql-12-on-ubuntu-20-04)

## Requirements
* Primary node ubuntu server
* Replication node ubuntu server

## Installation
* NOTE: Follow these steps for both server
1. Update apt package cache
```bash
sudo apt update
```
2. Install PostgreSQL
```bash
sudo apt install postgresql postgresql-contrib -y
```
3. Enable PostgreSQL 
```bash
sudo systemctl enable postgresql
```

## Usage

### Configure Primary Node
1. Login to psql using postgres user
```bash
sudo -u postgres psql
```
2. Create a user with replication privileges
```psql
CREATE ROLE replication_user WITH REPLICATION LOGIN PASSWORD 'P@ssw0rd';
```
then quit the psql
```psql
\q
```
3. Update postgresql configuration
```bash
sudo vi /etc/postgresql/12/main/postgresql.conf
```
then find and uncomment/update these parameters
```conf
listen_address = '<change this to primary node ip address>'

wal_level = logical

wal_log_hints = on
```
4. Update pg_hba configuration
```bash
sudo vi /etc/postgresql/12/main/pg_hba.conf
```
then add the line below to the file
```conf
host  replication   replication_user  <change_this_to_replication_node_ip_addres>/24   md5
```
5. Upon saving the changes on the configuration files, restart the postgresql server
```bash
sudo systemctl restart postgresql
```

### Configure Replication Node
1. Stop PostgreSQL server
```bash
sudo systemctl stop postgresql
```
2. Remove all files to start on a clean slate
```bash
sudo rm -rv /var/lib/postgresql/12/main/
```
3. Copy data from Primary Node
```bash
sudo pg_basebackup -h <primary_node_ip_address> -U replication_user -X stream -C -S replication_1 -v -R -W -D /var/lib/postgresql/12/main/
```
4. Grant ownership to the directory
```bash
sudo chown postgres -R /var/lib/postgresql/12/main/
```
5. Start the PostgreSQL server
```bash
sudo systemctl start postgresql
```


### Testing the Replication
Verify pg_stat_replication
```bash
sudo -u postgres psql
```
then query and verify if the replication node ip address is streaming
```psql
select client_addr, state from pg_stat_replication;
```
if the replication node is streaming all the operations on the primary node should be replicated on the replication node
