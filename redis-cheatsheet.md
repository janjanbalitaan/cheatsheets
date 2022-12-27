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

## Adding an authorization
1. Open the configuration file
```bash
sudo vi /etc/redis/redis.conf
```
2. Add a password
```conf
# you may just uncomment if requirepass is already existing then just change the password 
requirepass <your password here>
```
3. Restart the Redis Server
```bash
sudo systemctl restart redis
```

## Usage in CLI
1. open the redis cli
```bash
redis-cli
```
2. [Optional] Authenticate if need
```bash
auth <password here>
```
3. Set a value for the key
```bash
set foo 1
```
4. Get the value of the key
```bash
get foo
```
5. Exit the redis cli
```bash
exit
```

## Usage in FastAPI
* Access the [github repo](https://github.com/janjanbalitaan/fastapi-with-redis-sample) for the sample