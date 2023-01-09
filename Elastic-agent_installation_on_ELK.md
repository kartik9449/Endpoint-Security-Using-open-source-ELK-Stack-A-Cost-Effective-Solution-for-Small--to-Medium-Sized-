## 1. Download the elastic-agent
      curl -L -O https://artifacts.elastic.co/downloads/beats/elastic-agent/elastic-agent-7.17.8-amd64.deb

## 2. Depackage the downloaded file abd install

     sudo dpkg -i elastic-agent-7.17.8-amd64.deb

## 3. Go to the installed file and give path and token of fleet-server

     sudo elastic-agent enroll     --fleet-server-es=http://localhost:9200   --fleet-server-service-token=AAEAAWVsYXN0aWMvZmxlZXQtc2VydmVyL3Rva2VuLTE2NzE4MzU2NzAwNzk6dzhfLTBGdVJUWTZEazZrT3VuaV8tdw   --fleet-server-policy=499b5aa7-d214-5b5d-838b-3cd76469844e   --fleet-server-insecure-http

## 4. Start the service

    sudo systemctl start elastic-agent

## 5. Enable the service

    sudo systemctl enable elastic-agent

# 6. Status of service

    sudo systemctl status elastic-agent
  