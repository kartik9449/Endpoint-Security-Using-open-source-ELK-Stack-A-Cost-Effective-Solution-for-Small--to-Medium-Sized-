# Configure X-Pack Security 

## 1. stop kibana

    sudo systemctl stop kibana

## 2. Stop elasticsearch 

    sudo systemctl stop elasticsearch

## 3. enable xpack in elasticsearch.yml

    sudo nano /etc/elasticsearch/elasticsearch.yml

    xpack.security.enabled: true
    xpack.security.authc.api_key.enabled: true


    --------------x-pack monitoring---------

    xpack.monitoring.collection.enabled: true
    xpack.monitoring.elasticsearch.collection.enabled: true

## 4. Setup user passwords using interactive mode other option is to set using auto mode
## interactive = manually enter password for each user
## auto = automatically generates password for all users

    sudo systemctl start elasticsearch
    cd /usr/share/elasticsearch/bin
    sudo ./elasticsearch-setup-passwords interactive



## 5. Add the default username in kibana

    sudo nano /etc/kibana/kibana.yml

    elasticsearch.username: "kibana"
    elasticsearch.password: "new_password"

## Generate encryption key 

    xpack.encryptedSavedObjects.encryptionKey: 8c4e3d9338f0a9392ab55f96b7c0d57a
    xpack.reporting.encryptionKey: 9a01e6935beee9ac86119f1f1f000110
    xpack.security.encryptionKey: ca3d520f9d85f2e91963e655cf36345c


## 6. Start Kibana service

    sudo systemctl start kibana



