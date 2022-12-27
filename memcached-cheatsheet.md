# MEMCACHED Cheatsheet
This is a memcached cheatsheet with reference using [digital ocean website](https://www.digitalocean.com/community/tutorials/how-to-install-and-secure-memcached-on-ubuntu-20-04)

## Installation
1. Update apt package cache
```bash
sudo apt update
```
2. Install Memcached
```bash
sudo apt install memcached
```
3. Install Memcached tools
```bash
sudo apt install libmemcached-tools
```
3. Check memcached if running 
```bash
systemctl status memcached
```

## Usage
1. Start the Redis
```bash
sudo systemctl start memcached
```
2. Check the status of Redis
```bash
systemctl status memcached
```
3. Change the status of the memcached
```bash
# to change status use one of the commands below
# sudo systemctl stop memcached # to stop the memcached service
# sudo systemctl restart memcached # to restart the memcached service
```

## Adding an authorization
NOTE: not all libraries are supporting authorization while doing an access with memcached
1. Install sasl2-bin package
```bash
sudo apt install sasl2-bin
```
2. Create a directory for sasl2
```bash
sudo mkdir -p /etc/sasl2
```
3. Create a memcached configuration in sasl2 directory
```bash
sudo vi /etc/sasl2/memcached.conf
```
then add the following lines
```text
log_level: 5
mech_list: plain
sasldb_path: /etc/sasl2/memcached-sasldb2
```
4. Create sasl database with user credentials
```bash
sudo saslpasswd2 -a memcached -c -f /etc/sasl2/memcached-sasldb2 <username here>
```
5. Give memcache user and group ownership for the created sasl database
```bash
sudo chown memcache:memcache /etc/sasl2/memcached-sasldb2
```
6. Enable Sasl configuration
```bash
sudo vi /etc/memcached.conf
```
then add the these line at the bottom of the file
```text
# enable sasl configuration
-S
```
7. Restart the memcache
```bash
sudo systemctl restart memcached
```

## Usage in FastAPI
* Access the [github repo](https://github.com/janjanbalitaan/fastapi-with-memcached-sample) for the sample