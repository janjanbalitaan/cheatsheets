# NGINX Cheatsheet
This is a NGINX cheatsheet with reference using [nginx website](http://nginx.org/en/docs)

## Installation
1. Install NGINX
```bash
sudo apt-get install nginx
```
2. Enable the firewall
```bash
sudo ufw enable
```
3. Allow NGINX applications
```bash
# to check the nginx applications use the command below
sudo ufw app list
# then allow by using the commands below
sudo ufw allow 'Nginx HTTP'
# if using https
sudo ufw allow 'Nginx HTTPS'
```

## Usage
1. Start the NGINX
```bash
sudo systemctl start nginx
```
2. Check the status of NGINX
```bash
systemctl status nginx
```
3. Change the status of the NGINX
```bash
# to change status use one of the commands below
# sudo systemctl stop nginx # to stop the nginx service
# sudo systemctl restart nginx # to restart the nginx service
# sudo systemctl reload nginx # to reload the config
# sudo systemctl disable nginx # to disable
# sudo systemctl enable nginx # to enable
```
4. Kill gracefully the NGINX
```bash
sudo kill -QUIT $( cat /usr/local/nginx/logs/nginx.pid )
```
5. Check the if nginx is up and running by using a browser with the link `http://<your_current_ip>`

## Usage as a web server
1. Create a project directory
```bash
sudo mkdir -p /var/www/sample-project.com/html
```
2. Assign ownership to the project directory
```bash
sudo chown -R $USER:$USER /var/www/sample-project.com/html
```
3. Change permission of the project directory
```bash
sudo chmod -R 755 /var/www/sample-project.com
```
4. Create a sample index file to test
```bash
sudo vi /var/www/sample-project.com/html/index.html
```
5. Create a configuration file for the project
```bash
sudo vi /etc/nginx/sites-available/sample-project.com
```
The file should like this:
```nginx
server {
    listen <port>;
    listen [::]:<port>;

    root /var/www/sample-project.com/html;
    index index.html;

    server_name sample-project.com www.sample-project.com;

    location / {
        try_files $uri $uri/ =404;
    }
}
```
6. Create a link for the configuration file to be enabled
```bash
sudo ln -s /etc/nginx/sites-available/sample-project.com /etc/nginx/sites-enabled/
```
7. Test the configuration
```bash
sudo nginx -t
```
8. Restart or reload the nginx to apply the changes
```bash
sudo systemctl restart nginx # sudo systemctl reload nginx
```
9. Access the project to see if it is up and running by using the browser and accessing the link:`http://<your_current_ip>:<port>`

## Usage as a reverse proxy
1. Create a configuration file for the reverse proxy
```bash
sudo vi /etc/nginx/sites-available/sample-reverse-proxy.conf
```
The file should like this:
```nginx
server {
    server_name <domain_name>;

    location / {
        proxy_pass http://<server_ip>:<server_port>/;
    }
}
```
2. Create a link for the configuration file to be enabled
```bash
sudo ln -s /etc/nginx/sites-available/sample-reverse-proxy.conf /etc/nginx/sites-enabled/
```
3. Test the configuration
```bash
sudo nginx -t
```
4. Restart or reload the nginx to apply the changes
```bash
sudo systemctl restart nginx # sudo systemctl reload nginx
```
5. Access the project to see if it is properly working by using the browser and accessing the link:`http://<domain_name>`

## Usage as a load balancer
1. Create a configuration file for the load balancer
```bash
sudo vi /etc/nginx/sites-available/sample-load-balancer.conf
```
The file should like this:
```nginx
upstream sample_app {
    # start: can set load balancing method here
    # if no method the default will be round robin
    # methods available: 
    # least_conn; # least connections
    # ip_hash; # ip hash
    # hash $request_uri consistent; # generic hash
    # least_time header; # least time and can be implemented in NGINX Plus only
    # end: can set load balancing method here
    server <ip_address_1>;
    server <ip_address_2>;
}

server {
    listen 80;
    # listen 443 ssl;
    server_name <domain_name>;

    # set up ssl configuration if you have any
    # ssl on;
    # ssl_certificate         /etc/nginx/ssl/example.com/server.crt;
    # ssl_certificate_key     /etc/nginx/ssl/example.com/server.key;
    # ssl_trusted_certificate /etc/nginx/ssl/example.com/ca-certs.pem;
	
    # ssl_session_cache shared:SSL:20m;
    # ssl_session_timeout 10m;
	
    # ssl_prefer_server_ciphers       on;
    # ssl_protocols                   TLSv1 TLSv1.1 TLSv1.2;
    # ssl_ciphers                     ECDH+AESGCM:DH+AESGCM:ECDH+AES256:DH+AES256:ECDH+AES128:DH+AES:ECDH+3DES:DH+3DES:RSA+AESGCM:RSA+AES:RSA+3DES:!aNULL:!MD5:!DSS;

    location / {
        proxy_pass http://sample_app;
    }
}
```
2. Create a link for the configuration file to be enabled
```bash
sudo ln -s /etc/nginx/sites-available/sample-load-balancer.conf /etc/nginx/sites-enabled/
```
3. Test the configuration
```bash
sudo nginx -t
```
4. Restart or reload the nginx to apply the changes
```bash
sudo systemctl restart nginx # sudo systemctl reload nginx
```
5. Access the project to see if it is properly working by using the browser and accessing the link:`http://<domain_name>`