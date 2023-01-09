# The following instructions outline the process for installing and configuring ElasticSearch on a system.
# Pre-requisites 

## The system is updated using the command 

    sudo apt-get update

## If the system does not have wget installed

    sudo apt-get install wget -y

# Manual ElK Stack Installation steps

## 1. Download and install public signing key 

    wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -

## 2. Install package apt-transport-https

    ssudo apt-get install apt-transport-HTTPS -y

## 3. Save directory definitions

    echo "deb https://artifacts.elastic.co/packages/7.17/apt stable main" | Sudo tee -a /etc/apt/sources.list.d/elastic-7.17.list


## 4. Update and Install elasticsearch, logstash and Kibana

    sudo apt-get update && sudo apt-get install elasticsearch && sudo apt-get install logstash && sudo apt-get install kibana

## 5. configure elasticsearch

    sudo su
    sudo nano /etc/elasticsearch/elasticsearch.yml

    change cluster name
    cluster.name: demo-elk  

    give the cluster a descriptive name
    node.name: elk-1 

    change network binding
    network.host: 0.0.0.0  

    setup discovery.type as single node
    discovery.type: single-node

## 6. Start Elasticsearch service

    sudo systemctl start elasticsearch

## 7. Check status Elasticsearch service

    sudo systemctl status elasticsearch

## 8. validate Elasticsearch cluster health

    curl -XGET http://localhost:9200/_cluster/health?pretty

## 9. configure kibana
    
    sudo nano /etc/kibana/kibana.yml

    uncomment server.port
    server.port: 5601

    server base url however this needs to be corrected everytime you start and stop the server
    server.publicBaseUrl: "http://192.168.2.3:5601"

    change server.host
    server.host: "0.0.0.0"
    
    change server.name
    server.name: "demo-kibana"
    
    uncomment elasticsearch.host
    elasticsearch.hosts: ["http://localhost:9200"]

    elasticsearch.username: "kibana_system"
elasticsearch.password: "Kartik"
    
## 10. start Kibana service

   sudo systemctl start kibana

## 11.  status Kibana Service

    sudo systemctl status kibana
    
## 12. enable elasticsearch and kibana

    sudo systemctl enable elasticsearch
    sudo systemctl enable kibana
    