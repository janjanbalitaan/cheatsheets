# REDIS Cheatsheet
This is a redis cheatsheet with reference using [REDIS website](https://redis.io/docs/getting-started/installation/install-redis-on-linux/)

## Installation
1. Update apt package cache
```bash
sudo apt update
```
2. Install Redis
```bash
sudo apt install redis
```
3. Check redis if running 
```bash
systemctl status redis
```

## Usage
1. Start the Redis
```bash
sudo systemctl start redis
```
2. Check the status of Redis
```bash
systemctl status redis
```
3. Change the status of the redis
```bash
# to change status use one of the commands below
# sudo systemctl stop redis # to stop the redis service
# sudo systemctl restart redis # to restart the redis service
```


