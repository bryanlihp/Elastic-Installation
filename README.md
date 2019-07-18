# Install Elasticsearch, Logstash and Kibana (Elastic Stack) on Ubuntu 18.01
## Elastic-Installation
The easist way to install Elasticsearch on Ubuntu 18.04 is by installing the deb package from official Elasticsearch repository.
1. Update package:
  ```
  $sudo apt-update 
  $sudo apt install apt-transport-https   ** This is necessary to access a repository over HTTPS
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
## Microsoft SQL Server installation
1. Import the public repository GPG keys:
  ```
  ```
