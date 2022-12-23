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


