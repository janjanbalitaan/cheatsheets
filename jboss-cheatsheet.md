# Docker Cheatsheet
This is a JBOSS cheatsheet with reference using [digital ocean website](https://www.digitalocean.com/community/tutorials/how-to-install-java-with-apt-get-on-ubuntu-16-04)


## Installation
* Java
    1. Install Java
    ```bash
    sudo apt-get update
    sudo apt-get install openjdk-7-jre
    sudo apt-get install openjdk-7-jdk
    ```
    2. Setup the Java 7
    ```bash
    sudo update-alternatives --config java
    # then select the number of java 7 from the options
    ```
    3. Define environment variable
    ```bash
    sudo vi /etc/environment
    ```
    then append the below defails on the file
    ```text
    JAVA_HOME="/usr/lib/jvm/java-7-openjdk-amd64"
    ```

* JBOSS
    1. Download JBOSS
    ```bash
    wget http://download.jboss.org/jbossas/7.1/jboss-as-7.1.1.Final/jboss-as-7.1.1.Final.tar.gz
    ```
    2. Extract JBOSS
    ```bash
    tar xfvz jboss-as-7.1.1.Final.tar.gz
    ```
    3. Move JBOSS to the local share directory
    ```bash
    sudo mv jboss-as-7.1.1.Final /usr/local/share/jboss
    ```
    4. Change ownership of the JBOSS directory
    ```bash
    # create a new user to let the jboss run in a non-root mode
    sudo adduser newuser
    sudo chown -R newuser /usr/local/share/jboss
    ```
    5. Add jboss user
    ```bash
    # swtich to the created user
    su newuser
    cd /usr/local/share/jboss/bin
     ./add-user.sh
    ```

## Usage
1. Start JBOSS Server
```bash
cd /usr/local/share/jboss/bin
./standalone.sh -Djboss.bind.address=0.0.0.0 -Djboss.bind.address.management=0.0.0.0&
```
2. Verify if the JBOSS server is running by accessing on the browser the link: `http://server_ip_address:9990/console`
3. Stopping the JBOSS server
```bash
cd /usr/local/share/jboss/bin
./jboss-cli.sh --connect --controller=server_ip_address:9999 command=:shutdown
```