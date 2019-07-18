# Install Elasticsearch, Logstash and Kibana (Elastic Stack) on Ubuntu 18.01
## Prerequisites
- Ubuntu 18.04 server
  - OS: Ubuntu 18.04
  - RAM: 4GB
  - CPU: 2
## Java 8
Required by Elasticsearch and Logstash. Note that Java 9 is not supported.
  ```
  $sudo apt install openjdk-8-jdk
  ```
  Verify the Java version by running:
  ```
  $java -version
  ```
  The output should look like:
  ```
  openjdk version "1.8.0_191"
  OpenJDK Runtime Environment (build 1.8.0_191-8u191-b12-2ubuntu0.18.04.1-b12)
  OpenJDK 64-Bit Server VM (build 25.191-b12, mixed mode)
  ```
## Nginx
Required as a reverse proxy for Kibana.
Nginx is one of the most popular web servers in the world and is responsible for hosting some of the largest and highest-traffic sites on the internet. It is more resource-friendly than Apache in most cases and can be used as a web server or reverse proxy.
1. Installing Nginx
Nginx is available in Ubuntu's defaul repository,it is possible to install it from these repositories using the apt packaging system.
  ```
  $sudo apt update
  $sudo apt install nginx
  ```
2. Adjust the Firewall
Before testing Nginx, the firewall software needs to be adjusted to allow access to the service. Nginx registers itself as a service with ufw upon installation, making it straightforward to allow Nginx access.
List the application configurations that ufw knows how to work with by typing:
  ```
  $sudo ufw app list
  ```
output:
  ```
  Available applications:
    Nginx Full
    Nginx HTTP
    Nginx HTTPS
    OpenSSH
  ```
There are three options avaliable for Nginx
  - Nginx Full: This profile opens both port 80 (normal, unencrypted web traffic) and port 443 (TLS/SSL encrypted traffic)
  - Nginx HTTP: This profile opens only port 80 (normal, unencrypted web traffic)
  - Nginx HTTPS: This profile opens only port 443 (TLS/SSL encrypted traffic)
Allow trafix on port 80:
  ```
  $sudo ufw allow 'Nginx HTTP'
  $sudo ufw status
  ```
3.Check the web server
  ```
  $systemctl status nginx
  $ip addr show eth0 | grep inet | awk '{ print $2; }' | sed 's/\/.*$//'
  $curl -4 icanhazip.com
  ```
4. Managing the Nginx Process
Basic management commands:
  ```
  $sudo systemctl stop nginx
  $sudo systemctl start nginx
  $sudo systemctl restart nginx
  $sudo systemctl reload nginx
  $sudo systemctl disable nginx
  $sudo systemctl enable nginx
  ```
5. More Information
Refer to https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-ubuntu-18-04

## Elastic-Installation
Elasticsearch is a distributed RESTful search engine.The easist way to install Elasticsearch on Ubuntu 18.04 is by installing the deb package from official Elasticsearch repository.
1. Update package:
  apt-transport-https is necessary to access a repository over HTTPS
  ```
  $sudo apt-update 
  $sudo apt install apt-transport-https
  ```
2. Add Elasticsearch repository
  Import the repository's GPG using the following wget command:
  ```
  $wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
  ```
  Next, add the Elasticsearch repository to the system:
  ```
  $sudo sh -c 'echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" > /etc/apt/sources.list.d/elastic-7.x.list'
  ```
3. Update package and install
  Once the repository is enabled, update the apt package list and install the Elasticsearch engine:
  ```
  $sudo apt update
  $sudo apt install elasticsearch
  
  ```
4. Enable the service and run
  Elasticsearch service will not start automatically. Use the following command to enable and run the service:
  ```
  $sudo systemctl enable elasticsearch.service
  $sudo systemctl start elasticsearch.service
  ```
  To verify that ElasticSearch is running, using the following command:
  ```
  $curl -X GET "localhost:9200/"
  ```
  Config Elasticsearch by editing file /etc/elasticsearch/elasticsearch.yml
  ```
  $sudo nano /etc/elasticsearch/elasticsearch.yml
  ```
  Note that when setting network.host to 0.0.0.0 (production use), cluster.initial_master_nodes must be set accordingly.
  
## Kibana
Kibana is an Elasticsearch web interface for searching and visualizing logs.
According to the official documentation, you should install Kibana only after installing Elasticsearch. Installing in this order ensures that the components each product depends on are correctly in place.
Because you've already added the Elastic package source in the previous step, you can just install the remaining components of the Elastic Stack using apt:
  ```
  $sudo apt install kibana
  ```
Then enable and start the Kibana service:
  ```
  $sudo systemctl enable kibana
  $sudo systemctl start kibana
  ```

## Logstash
Logstash is the data processing component of the Elastic Stack which sends incoming data to Elasticsearch.
## Beats
## Microsoft SQL Server installation
1. Import the public repository GPG keys:
  ```
  $wget -qO- https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
  ```
2. Register the Microsoft SQL Server Ubuntu repository
  ```
  $sudo add-apt-repository "$(wget -qO- https://packages.microsoft.com/config/ubuntu/16.04/mssql-server-2017.list)"
  ```
3. Install SQL Server package
  ```
  $sudo apt-get update
  $sudo apt-get install -y mssql-server
  ``` 
4.Setup SQL Server 
  ```
  $sudo /opt/mssql/bin/mssql-conf setup
  ```
  Verify that SQL Server is running:
  ```
  $systemctl status mssql-server --no-pager
  ```
5.Firewall
Enabling port SQL Server TCP port 1434 on the firewall is required for connecting remotely:
  ```
  $ufw enable 1433
  ```
In addition to TCP 1433 for SQL Server, you may need to enable TCP 1434 for the Dedicated Administrator Connection (DAC). These are default ports.


