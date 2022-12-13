# Docker Cheatsheet
This is a Docker cheatsheet with reference using [docker website](https://docs.docker.com/engine/install/ubuntu/)

## Installation
If you have an older version of docker, might need to uninstall
```bash
sudo apt-get remove docker docker-engine docker.io containerd runc
```

### Setup the repository
1. Update and install packages needed
```bash
sudo apt-get update
sudo apt install apt-transport-https ca-certificates curl software-properties-common
```
2. Add Docker's official GPG key
```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```
3. Setup the repository
```bash
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable"
apt-cache policy docker-ce
```

### Install Docker
1. Install Docker Engine and Docker Compose
```bash
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin
```
2. Verify installation
```bash
sudo docker run hello-world
```

## Usage in FastAPI
1. Create an Image file
```Dockerfile
FROM python:3.8.1
# setup a working directory
WORKDIR /code
# copy the requirements to be installed
COPY ./requirements.txt /code/requirements.txt
# install the requirements
RUN pip install --no-cache-dir --upgrade -r /code/requirements.txt
# copy important files to the working directory
COPY ./api /code/api
COPY ./models /code/models
COPY ./utilities /code/utilities
COPY ./app.py /code/app.py
COPY ./.env /code/.env
# expose the port where the application will be running
EXPOSE 8000
# run the command
CMD ["uvicorn", "app:app", "--host", "0.0.0.0", "--port", "8000"]
# if using nginx use the cmd below
# CMD ["uvicorn", "app:app", "--proxy-headers", "--host", "0.0.0.0", "--port", "8000"]
```
2. Build the image
```bash
sudo docker build -t <docker-image-name> .
```
3. Run the docker
```bash
# this will allow to connect to the local database in the host
sudo docker run -d --name <container-name> --network="host" <image-name>
```