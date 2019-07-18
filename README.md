# Install Elasticsearch, Logstash and Kibana (Elastic Stack) on Ubuntu 18.01
## Prerequisites
- Ubuntu 18.04 server
  Set up by following our Initial Server Setup Guide for Ubuntu 18.04, including a non-root user with sudo privileges and a firewall configured with ufw. The amount of CPU, RAM, and storage that your Elastic Stack server will require depends on the volume of logs that you intend to gather. For this tutorial, we will be using a VPS with the following specifications for our Elastic Stack server:
  - OS: Ubuntu 18.04
  - RAM: 4GB
  - CPU: 2
- Java 8 — which is required by Elasticsearch and Logstash — installed on your server. Note that Java 9 is not supported. To install this, follow the "Installing the Oracle JDK" section of our guide on how to install Java 8 on Ubuntu 18.04.
- Nginx installed on your server, which we will configure later in this guide as a reverse proxy for Kibana. Follow our guide on How to Install Nginx on Ubuntu 18.04 to set this up.
## Elastic-Installation
Elasticsearch is a distributed RESTful search engine.The easist way to install Elasticsearch on Ubuntu 18.04 is by installing the deb package from official Elasticsearch repository.
1. Update package:
  apt-transport-https is necessary to access a repository over HTTPS
  ```
  $sudo apt-update 
  $sudo apt install apt-transport-https
  ```
2. Install OpenJDK 8:
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
3. Add Elasticsearch repository
  Import the repository's GPG using the following wget command:
  ```
  $wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
  ```
  Next, add the Elasticsearch repository to the system:
  ```
  $sudo sh -c 'echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" > /etc/apt/sources.list.d/elastic-7.x.list'
  ```
4. Update package and install
  Once the repository is enabled, update the apt package list and install the Elasticsearch engine:
  ```
  $sudo apt update
  $sudo apt install elasticsearch
  
  ```
5. Enable the service and run
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
## Logstash
Logstash is the data processing component of the Elastic Stack which sends incoming data to Elasticsearch.

## Kibana
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
5.


