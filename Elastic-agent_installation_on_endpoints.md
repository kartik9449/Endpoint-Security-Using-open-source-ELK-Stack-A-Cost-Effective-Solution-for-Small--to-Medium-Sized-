## For ubuntu

## 1. Download the elastic-agent
      curl -L -O https://artifacts.elastic.co/downloads/beats/elastic-agent/elastic-agent-7.17.8-amd64.deb

## 2. Depackage the downloaded file abd install

     sudo dpkg -i elastic-agent-7.17.8-amd64.deb

## 3. Install and give URL and token of fleet-server

    sudo ./elastic-agent install --url=http://34.27.172.83:8220 --enrollment-token=SElSdVA0VUIwcWxUa1lyZzRmYnU6bEIwVk5MLVRTX3lQZV9ac0hNT1BtQQ==
   
## 4. Start the service

    sudo systemctl start elastic-agent

## 5. Enable the service

    sudo systemctl enable elastic-agent

# 6. Status of service

    sudo systemctl status elastic-agent
  
  ## -----------------------------------------------------------------------------------------


## For Windows

## 1. Download the elastic-agent 

    wget https://artifacts.elastic.co/downloads/beats/elastic-agent/elastic-agent-7.17.8-windows-x86_64.zip -OutFile elastic-agent-7.17.8-windows-x86_64.zip


## 2. Unzip the downloaded file and install

    >> Expand-Archive .\elastic-agent-7.17.8-windows-x86_64.zip

## 3. Install and give URL and token of fleet-server
     .\elastic-agent.exe install -f --url=http://34.27.172.83:8220 --enrollment-token=SElSdVA0VUIwcWxUa1lyZzRmYnU6bEIwVk5MLVRTX3lQZV9ac0hNT1BtQQ== --insecure