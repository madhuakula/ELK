# Installing ELK-2.0 on Ubuntu 14.04

---

- In this tutorial, we will install `elasticsearch 2.2.x`, `logstash 2.2.x` and `kibana 4.4.x` on `Ubuntu 14.04`. 
- For more detailed information about Elasticsearch, Logstash and Kibana **[click here](ELK-Overview.md)**.

- Here is the details support metrics for ELK stack **[https://www.elastic.co/support/matrix](https://www.elastic.co/support/matrix)**


### Prerequisites

To complete this tutorial, we need below system requirements

- Operating System : `Ubuntu 14.04`
- RAM : `2GB`
- CPU : `2`
- Working `Internet` Connection


#### Installing Java 9

Elasticsearch and logstash requires Java, So for more details about the version find the support matrix above.

- Before doing all the changes we will change user to `root`

```
sudo su
```
- We will add the Oracle Java PPA to repository

```
add-apt-repository -y ppa:webupd8team/java
```
- Update the apt repository

```
apt-get update -y
```
- Install the Oracle Java Installer

```
apt-get -y install oracle-java9-installer
```
- Check the Java installation by executing below command

```
java -version
```

#### Installing Elasticsearch 2.2.x

- Go to the home directory by executing the below command

```
cd
```
- Execute the below command to download the elasticsearch Debian package file locally. You can download the version which you need by looking here. [https://www.elastic.co/downloads/elasticsearch](https://www.elastic.co/downloads/elasticsearch)

```
wget https://download.elasticsearch.org/elasticsearch/release/org/elasticsearch/distribution/deb/elasticsearch/2.2.0/elasticsearch-2.2.0.deb
```
- Install the package by executing the below command

```
dpkg -i elasticsearch-2.2.0.deb
```
- Please do changes to the elasticsearch.yml file by following the below instructions
	- Go and edit the elasticsearch.yml for cluster name, node name, etc.
	- It's good practice to give the cluster name, node name and it should be unique
	- The basic security protection to restrict other users to elasticsearch is by providing `network.host` details
	- By uncommenting the `bootstrap.mlockall` elasticsearch node not to swap it's memory.

- Edit the file and un comment the below details

```
vi /etc/elasticsearch/elasticsearch.yml
```
- Un comment the below lines and give the detail as below.

```
 cluster.name: elasticsearch_testing-prod
 node.name: "testing_log_prod"
 bootstrap.mlockall: true
 network.host: 127.0.0.1
```
- Then start the elasticsearch service by running the below command

```
service elasticsearch start
```
- Run the below command to start the elasticsearch process on boot

```
update-rc.d elasticsearch defaults 95 10
```
- Run the below `CURL` command to check the elasticsearch service

```
curl -XGET localhost:9200
```
- Now you are successfully able to install the elasticsearch

#### Installing elasticsearch plugins

- There are bunch of plugins available for elasticsearch like head, big desk, HQ, etc.
- We install basic `elasticsearch head` plugin
- Navigate to the elasticsearch installation directory

```
cd /usr/share/elasticsearch
```
- Run the below command to install elasticsearch head plugin, we can install other plugins similarly.

```
./bin/plugin install mobz/elasticsearch-head
```
- Now to check the plugin output, restart the elasticsearch server and browse through the below URL

```
http://localhost:9200/_plugin/head
```

#### Installing apache2 for sample logs

- Installing apache2 web server for getting basic sample logs to parse using logstash
- Run the following command to install `apache2`

```
apt-get install apache2 -y
```
- Start the apache service by running the below command.

```
service apache2 start
```
- Browse the local host to see the apache server running or not.

```
http://localhost
```

#### Installing logstash 2.2.x

- Download the logstash debian package by running the below command

```
wget https://download.elastic.co/logstash/logstash/packages/debian/logstash_2.2.0-1_all.deb
```
- Install the logstash package by running the below command.

```
dpkg -i logstash_2.2.0-1_all.deb
```
- Now logstash has been installed and it's time to write basic logstash config for testing
- Here we are using apache2 logs for parsing. Please follow the below commands to make simple logstash config
- Create and edit the logstash file by running the below command

```
vi /etc/logstash/conf.d/logstash.conf
```
- Paste the below code and save the file by executing `:wq`

```
input {
    file {
        path => '/var/log/apache2/access.log'
    start_position => "beginning"
    }
}

filter {
    grok {
        match => { "message" => "%{COMBINEDAPACHELOG}" }
    }
}

output {
  stdout { codec => rubydebug }
  elasticsearch {
    hosts => ["localhost:9200"]
  }
}
```
- Now you can do basic configuration test by running the below command

```
/opt/logstash/bin/logstash --configtest --config /etc/logstash/conf.d/logstash.conf

(or)

service logstash configtest
```
- Run the logstash service and add to the startup services

```
service logstash start
```
```
update-rc.d elasticsearch defaults 95 10
```

- Now run the below command to forward the logstash logs

```
/opt/logstash/bin/logstash -f /etc/logstash/conf.d/logstash.conf
```
- Check the log parsing output to elasticsearch by browsing to the elastic head plugin

```
http://localhost:9200/_plugin/head
```

### Installing Kibana 4.4.x

- Run the below command to add the key

```
wget -qO - https://packages.elastic.co/GPG-KEY-elasticsearch | apt-key add -
```
- Add the kibana repo to the list

```
echo "deb http://packages.elastic.co/kibana/4.4/debian stable main" | tee -a /etc/apt/sources.list
```
- Update and install the cabin

```
apt-get update && apt-get install kibana
```
- Add the kibana to startup service

```
update-rc.d kibana defaults 95 10
```
- Start the kibana service by running below command

```
service kibana start
```
- Please check the kibana dashboard by browsing to the below URL

```
http://localhost:5601
``` 
- select the index of elasticsearch and submit, then you can see the logs which are coming from elasticsearch.

---
**Now we are successfully able to install ELK-2.0, Please let me know if any issues.**
