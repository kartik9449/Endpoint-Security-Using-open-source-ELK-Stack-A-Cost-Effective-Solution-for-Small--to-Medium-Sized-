# Endpoint-Security-Using-open-source-ELK-with-fleets
Installation of ELK with Elastic Agent Fleet to effectively integrate endpoints for malware detection rather than to rely on Beats such as Heartbeat, Metricbeat, and Filebeat which have to be configured separately.
There are several steps that companies can take to implement security using the ELK Stack:
Collect and normalise endpoint data: The first step in threat hunting is to gather data from all of the company's endpoints. This includes logs, system information, and any other relevant data. It is important to normalise this data so it can be easily searched and analysed.
Set up the ELK Stack: The next step is to set up the ELK Stack on a dedicated server or cloud platform. This involves installing Elasticsearch, Logstash, Kibana and fleets and configuring them to work together.
Configure log input and parsing: Once the ELK Stack is set up, the next step is to configure log input and parsing. This involves setting up Logstash to collect logs from the company's endpoints and parse them into a structured format that can be easily searched and analysed in Elasticsearch.
Create visualisations and dashboards: The final step is to create visualisations and dashboards in Kibana to allow the company's security team to search and analyse the data collected from the endpoints easily. This can include graphs, charts, and other visualisations that help to identify trends and potential threats. By implementing threat hunting using the ELK Stack, companies can improve their endpoint security and reduce the risk of cyber-attacks. This is particularly useful for companies that may not have the resources to invest in more expensive security solutions. By proactively searching for IOCs and responding to potential threats quickly, companies can protect their network infrastructure and prevent significant damage from cyber-attacks.
Through the network's endpoints, threat actors try to access a system. This research provides a threat detection and prevention system that is implemented in a virtual environment using open-source ELK. Finally, a lab environment will be setup, run several attack scenarios, and make sure that all possible endpoint attack paths are taken into account.

For the lab setup, ELK was used where log analysis was done using Elasticsearch, Kibana for data visualisation and Fleet elastic-agent to get the logs from our endpoint devices. All of these would be deployed on a single server, and for the two endpoints, elastic-agents were configured on them to collect all the malicious logs at the end devices and then send them to the ELK server.

Lab Setup for this research.
1.	ELK server – Hosted on Google Cloud Platform 
2.	Test machine – Hosted on Google Cloud Platform 
3.	Home windows machine – Personal laptop

Devices	Configurations
Debian server (ELK server hosted)	1.	Elasticsearch with X-pack
2.	Kibana with X-pack
3.	Elastic Fleets Agent
Ubuntu (Test) endpoint 1	Elastic-agent
Windows (Test) endpoint 2	Elastic-agent

To Install the Elk server with fleets, the first step is to host a virtual server and endpoint on any virtual machine or cloud platform. We have deployed the server on the Google Cloud Platform (GCP) for this research.
Below is the hardware requirement for the ELK server setup.
Machine type: n1-standard-4
Operating system: Ubuntu 18.04
CPU: 4
RAM: 15 GB
Storage: 300 GB
IP range: 192.168.2.0/24 and 192.168.3.0/24

In the networking console, firewall rules were created to permit incoming TCP traffic to Elasticsearch and Kibana ports 9200 and 5601 respectively. Additionally, it is suggested to open port 22 so that anyone can SSH into our virtual machines. 
After the configuration is completed successfully, two virtual machines will be visible under the compute engine section. From there, either virtual machine can be accessed via SSH for further configurations.

Configuration on elk-1 server :
Prerequisites
1.	The first step is to install Java.
Run the following command to set up Java:
# sudo apt-get install default-JRE
1.	Ensure that Java is set up:
# Java -version
2.	Update the system:
# sudo apt-get update
3.	Install wget if it is not on the system:
# sudo apt-get install wget -y
Steps for Manual Elasticsearch Installation
The "heart" of the Elk stack is elasticsearch, which is in charge of indexing and storing the data delivered from various sources.
1.	Install and download the public signing key.
# wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
2.	Install the package apt-transport-https.
#sudo apt-get install apt-transport-HTTPS -y
3.	Define a directory and save it:
# echo "deb https://artifacts.elastic.co/packages/7.17/apt stable main" | Sudo tee -a /etc/apt/sources.list.d/elastic-7.17.list
4.	Install and update Elasticsearch:
# sudo apt-get update && sudo apt-get install elasticsearch && sudo apt-get install logstash && sudo apt-get install kibana
Open the Elasticsearch configuration file and specify the host on the network before starting the service
5.	To configure Elasticsearch, edit the .yml file and change the cluster name. Set the network host to 0.0.0.0 to allow Elasticsearch to be accessed from any location. All configurations are stored in this .yml file.
# sudo su
nano /etc/elasticsearch/elasticsearch.yml
change cluster name
cluster.name: demo-elk
give the cluster a descriptive name
node.name: elk-1
change network binding
network.host: 0.0.0.0
setup discovery.type as single node
discovery.type: single-node
6.	Start Elasticsearch service:
# sudo systemctl start elasticsearch
7.	Enable Elasticsearch service:
# sudo systemctl enable elasticsearch
8.	Check the health of the Elasticsearch cluster :
# curl -k -u elastic:<password> https://localhost:9200/_cluster/health?pretty ## -k to ignore SSL verification

  
  Steps for Manual Kibana Installation:
Similar to installing Elasticsearch, the Kibana user interface may be installed in the same way.
1.	Install Kibana
Since the ELK repository has already been installed, Kibana can be directly configured. If Kibana is not installed, use the following command to install it.
# sudo apt install kibana
2.	Configure Kibana :
To configure kibana go to the kibana.yml file
By default, Kibana is configured to run at localhost:5601. Edit the configuration file and change the value of server.host to an interface IP to enable external access.
# sudo nano /etc/kibana/kibana.yml
uncomment server.port
server.port: 5601
The base URL of the server needs to be corrected every time the server is started and stopped
server.publicBaseUrl: "http://localhost:5601/"
change server.host
server.host: "0.0.0.0"
Change server.name
server.name: "kibana"
uncomment elasticsearch.host
elasticsearch.hosts: ["https://localhost:9200"]
             add kibana_system user credentials
3.	Next, execute the following command to generate an enrollment token for Kibana:
# /usr/share/elasticsearch/bin/elasticsearch-create-enrollment-token -s kibana
4.	Also generate Kibana Encryption keys:
xpack.encryptedSavedObjects.encryptionKey. : Dashboards and visualisations can be encrypted using the key.
xpack.reporting.encryptionKey: Applied to encrypt reports that are saved.
xpack. security.encryptionKey. : Information about sessions is encrypted using the key.
The command below can be used to generate these keys:
# /usr/share/kibana/bin/kibana-encryption-keys generate
After generating the three keys insert them in kibana.yml file
xpack.encryptedSavedObjects.encryptionKey: 8c4e3d9338f0a9392ab55f96b7c0d57a
xpack.reporting.encryptionKey: 9a01e6935beee9ac86119f1f1f000110
xpack. security.encryptionKey: ca3d520f9d85f2e91963e655cf36345c
  5.	Start Kibana service:
# sudo systemctl start Kibana
6.	Enable Kibana service:
# sudo systemctl enable Kibana
7.	To check the status :
# sudo systemctl status Kibana
After Elasticsearch and Kibana have been properly configured, the next step is to configure X-pack security.
  Configure X-Pack Security :
To configure any service, the first step is to stop the services if they are running.
1.	Stop Kibana and Elasticsearch:
# sudo systemctl stop kibana
# sudo systemctl stop elasticsearch
2.	enable xpack in elasticsearch.yml
# sudo nano /etc/elasticsearch/elasticsearch.yml
xpack. security.enabled: true
xpack. security.http.ssl.enabled: false     # needed for elasticsearch 8.x to run on http
xpack. security.transport.ssl.enabled: false   # needed for elasticsearch 8.x to run without SSL
3.	Setup default user passwords:
To set up passwords for all users, first start the Elasticsearch service and then navigate to the bin folder to set the passwords.
The password can be generated automatically or set manually for each user.
# sudo systemctl start elasticsearch
# cd /usr/share/elasticsearch/bin
# sudo ./elasticsearch-setup-passwords auto (this will automatically generate random password for each users )
# sudo ./elasticsearch-setup-passwords interactive (using interactive mode to enter a password for each of the user manually)
In the lab setup, the interactive mode was used to save passwords manually. After generating passwords for each user, open the kibana.yml file and provide the username and password for the kibana_system user. When attempting to log in to the user interface, the system will ask for credentials and authenticate using the information provided in the .yml file
  
  Add Endpoint Security Integration
  
  Set up the Fleet server Agent on the main host (ELK-1).
In this configuration, the Fleet server is installed on the same node as the ELK stack.
Before adding fleet policy or agents, a fleet server host and Elasticsearch host must be added to the Kibana interface using the host name or IP address in the fleet settings. To do this, follow the steps below:
1.	Go to the Management menu in the Kibana UI > Fleet
2.	In the upper right > Fleet settings > Add Fleet Server hosts & change Elasticsearch hosts
3.	Server hosts - http://<public IP >:8220
The URLs that the agents will use to connect to a Fleet Server should be specified. For enrolment purposes, Fleet displays the first specified URL if there are multiple URLs. Port 8220 is the default port for Fleet Server.
4.	Elasticsearch hosts - http://<public IP >:9200
Indicate the Elasticsearch URLs to which agents should submit data. Port 9200 is what Elasticsearch uses by default.
  
  Fleet server configuration on elk-1
1.	installing the Elastic agent for the Fleet server on my ELK stack node;
# curl -L -O https://artifacts.elastic.co/downloads/beats/elastic-agent/elastic-agent-7.17.8-amd64.deb
2.	Extract the elastic agent
# sudo dpkg -i elastic-agent-7.17.8-arm64.deb
3.	Run the installation by going to the extracted Elastic agent directory.
# sudo elastic-agent enroll   \
--fleet-server-es=http:// <public_IP>:8220 \
--fleet-server-service-token=AAEAAWVsYXN0aWMvZmxlZXQtc2VydmVyL3Rva2VuLTE2NzE4MzU2NzAwNzk6dzhfLTBGdVJUWTZEazZrT3VuaV8tdw \
--fleet-server-policy=499b5aa7-d214-5b5d-838b-3cd76469844e \
--fleet-server-insecure-HTTP
Confirm the installation path and whether to start the agent as a service when the installation command has been completed, then select Y.
  Once the installation is completed, it will display a message as successfully enrolled the elastic agent.
4.	After installation, the elastic agent must be started and enabled
# sudo systemctl start elastic-agent
# sudo systemctl enable elastic-agent
Now Verify that the Fleet server is connected by returning to the Kibana UI.

  
  Installation of elastic fleet agents on ubuntu :
The same commands that were used to install the elastic agent on elk-1 should be followed to install it on other servers
Commands for Ubuntu:
# sudo apt-get update
# curl -L -O https://artifacts.elastic.co/downloads/beats/elastic-agent/elastic-agent-7.17.8-amd64.deb
# sudo dpkg -i elastic-agent-7.17.8-amd64.deb
# sudo elastic-agent enroll --url=http://<public IP>:8220 --enrollment-token=AAEAAWVsYXN0aWMvZmxlZXQtc2VydmVyL3Rva2VuLTE2NzIxODgxNjI1OTE6LVlxNEpfd0FTS2VUTmVGdHl5bkkwZw --insecure
# sudo systemctl start elastic-agent
# sudo systemctl status elastic-agent

  
  
  Installation of elastic fleet agents on Windows:
1.	Installing the Elastic agent to a windows laptop using PowerShell ( configure as admin)
# wget https://artifacts.elastic.co/downloads/beats/elastic-agent/elastic-agent-7.17.8-windows-x86_64.zip -OutFile elastic-agent-7.17.8-windows-x86_64.zip
2.	Extract the elastic agent
# >> Expand-Archive .\elastic-agent-7.17.8-windows-x86_64.zip
3.	Run the installation using the below command:
#.\elastic-agent.exe install -f --url=http://<public IP>:8220 --enrollment-token=SElSdVA0VUIwcWxUa1lyZzRmYnU6bEIwVk5MLVRTX3lQZV9ac0hNT1BtQQ== --insecure
  
  
  
  Here are the general steps to follow to download a malware sample on an Ubuntu endpoint and check for the logs on an ELK server with Elastic Agent:
•	Install Elastic Agent on the Ubuntu endpoint.
•	Download the malware sample on the endpoint.
•	Configure Elastic Agent to send logs to the ELK server.
•	Check the ELK server for logs generated by the endpoint.
•	Open a terminal on the Ubuntu endpoint and search for the directory where the malware sample should be downloaded
•	Use a command like wget or curl to download the malware sample from a trusted source. For example: wget https://www.example.com/malware.exe








