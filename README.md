# TAKE-2-ELK-Stack-Deployment-Unit-13

## Automated ELK Stack Deployment

This READ ME document contains the following details:

- Network Configuration
- Description of the Topology
- Access Policies
- ELK Configuration
	- Beats in Use
	- Machines Being Monitored
- How to Use the Ansible Build



### Network Configuration

The files in this repository were used to configure the network depicted below.



<img width="656" alt="ELK Stack network config diagram copy" src="https://user-images.githubusercontent.com/80297522/110932706-ee7a2680-837f-11eb-92cc-4ba377265aa8.png">


All files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the yml files may be used to install only certain pieces of it, such as Filebeat.

The playbooks used for this deployment are as follows:

- Configure ELK VM with docker: https://github.com/MadelineXCyber/TAKE-2-ELK-Stack-Deployment-Unit-13/blob/main/Ansible/Configure%20ELK%20VM%20with%20Docker
- Configure Web VM with Docker
- FIlebeat playbook
- Metricbeat playbook

Copies of all configuration files and playbooks used in this deployment are available in the Ansible folder: https://github.com/MadelineXCyber/TAKE-2-ELK-Stack-Deployment-Unit-13/tree/main/Ansible


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.  The DVWA site allows the cybersecurity industry to develop, learn and test security tools and skills in a legal environment. 

The network topology above includes load balancing which ensures that the application will be highly available, in addition to restricting traffic flow and access to the network. 

- Load Balancer: A Load Balancer is used to harden the network by protecting its availability and adding resilience to the overall system.   By incorporating a Load Balancer into our network architecture, all incoming traffic – in our case HTTP requests – is initially routed to a single point at the Load Balancer’s external frontend, before being redistributed to our 3 internal web servers (Web-1, Web-2 and Web-3) in the backend pool.  As the purpose of Load Balancers is to manage network traffic and divide it between the backend servers based on traffic flow, this helps to ensure maximum reliability and uptime of the network and provides critical redundancy to the system.  Load Balancers also undertake network ‘health checks’ and have the ability to incorporate specific security rules, both important measures in providing additional safeguards and security to the network.

- Jump Box: This network also includes a Jump Box VM, an administration server which acts as an intermediary or SSH host to the internal network, once again by managing and controlling access to the internal network.  An additional advantage of the Jump Box is that it is an intelligent device and can also be used as a control panel to perform critical functions such as system configurations and updates.   In order to set up this particular network, the Jump Box was used to install Docker containers on our Web-VMs and then run an Ansible playbook to configure the Web-VMs with DVWA container images.  Our Jump Box VM was also used to setup and configure a VM as an ELK server (to run an ELK Stack container) using Ansible.

Incorporating an ELK server into the network allows users to easily monitor the vulnerable VMs for changes to the network activity and system logs.

This is achieved using ELK Stack, a powerful, open-source tool used to store, search, analyse and visualise many different forms of data.  ELK is an acronym for the 3 components which make up Elastic Stack - Elasticsearch, Logstash and Kibana.

- ELASTICSEARCH is a powerful tool which allows the user to store, search and analyse data.  It has the ability to handle huge volumes of data in almost real-time ie. milliseconds. 

- LOGSTASH is a data processing pipeline that collects log data from different sources, converting different log data into a uniform format if necessary.  It is used to feed data to Elasticsearch. 

- KIBANA is a tool used to visualise data indexed in Elasticsearch.  The user can generate a variety of charts, graphs, maps and metrics using Kibana’s complex dashboard.


Due to the significant amount of information potentially contained in the Elasticsearch log database, a tool known as ‘Beats’ is now available as part of the ELK Stack suite to allow collection of specific data and information.  There are 8 official Beats in total, two of which are used in this deployment – Filebeat and Metricbeat (see also ‘Target Machines & Beats’ below).

- Filebeat: Filebeat is used to monitor specific log files or locations, as specified by the user.  Filebeat collates and organises the requested data, which is then forwarded to Elasticsearch or Logstash for indexing.  Filebeat watches for changes by monitoring the file system and specific logs.  As it is specific to a particular machine, filebeat must be installed on each individual VM/server to be monitored.

- Metricbeat: Metricbeat collects and records the metrics of a machine from the operating system and services running on the server.  These metrics allow the user to assess such things as the health of a network, as well as monitoring for signs of suspicious activity, for example CPU usage and uptime.  As with filebeat, metricbeat is specific to a particular machine and must be installed on each individual VM/server which is being monitored.


Our final network topology consists of a Jump Box VM, 3 Web Servers and an ELK-VM.  The configuration details of each machine may be found below. 

| Name        | Function                                          | IP Address                                            | Operating System     |
|-------------|---------------------------------------------------|-------------------------------------------------------|----------------------|
| Jump Box VM | Gateway <br>Intelligence <br>Ansible control node | Public: 52.187.237.72 <br>Private: 10.1.0.4           | Linux (Ubuntu 18.04) |
| Web-1       | Internal web server <br>DVWA container            | Public: Load balancer public IP <br>Private: 10.1.0.5 | Linux (Ubuntu 18.04) |
| Web-2       | Internal web server <br>DVWA container            | Public: Load balancer public IP <br>Private: 10.1.0.6 | Linux (Ubuntu 18.04) |
| Web-3       | Internal web server <br>DVWA container            | Public: Load balancer public IP <br>Private: 10.1.0.7 | Linux (Ubuntu 18.04) |
| ELK-VM      | Log server <br>ELK Stack container                | Public: 40.87.108.196 <br>Private: 10.0.0.4           | Linux (Ubuntu 18.04) |




### Access Policies


The machines on the internal network are not exposed to the public Internet.

Only the Jump Box VM and Load Balancer machines can accept connections from the Internet.  Access to these machines is only allowed from the following IP address:
- My local host machine with public (*dynamic) IP: 175.32.150.11

Machines within the network can only be accessed by the Jump Box.
- The Jump Box VM can access the ELK VM through the internal network.
- My local host machine can access the ELK VM using its external IP.

A summary of the access policies in place can be found in the table below.

| Name        | Publicly Accessible | Allowed IP Addresses                                                                                                |
|-------------|---------------------|---------------------------------------------------------------------------------------------------------------------|
| Jump Box VM | Yes                 | My host machine: IP 175.32.150.11 <br>Web-1: 10.1.0.5 <br>Web-2: 10.1.0.6 <br>Web-3: 10.1.0.7 <br>ELK-VM: 10.0.0.4  |
| Web-1       | No                  | Jump Box VM: 10.1.0.4<br>Load Balancer: 13.72.251.150                                                               |
| Web-2       | No                  | Jump Box VM: 10.1.0.4<br>Load Balancer: 13.72.251.150                                                               |
| Web-3       | No                  | Jump Box VM: 10.1.0.4<br>Load Balancer: 13.72.251.150                                                               |
| ELK-VM      | Yes                 | My host machine: 175.32.150.11<br>Web-1: 10.1.0.5 <br>Web-2: 10.1.0.6 <br>Web-3: 10.1.0.7 <br>Jump Box VM: 10.1.0.4 |




### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- Automated configuration streamlines and simplifies network and system configurations as it allows us to execute complex and multiple commands/scripts in one command.
- Automated configuration allows us to configure multiple servers/machines identically, and simultaneously.
- There is less room for human error using automation.  This is particularly important when configuring multiple machines which require identical configuration.
- An automated process is much easier to use and less time consuming than configuration through a manual process, which generally requires configuration one machine at a time.

We used the following playbook to configure the ELK machine: 

<img width="676" alt="elk_config_image" src="https://user-images.githubusercontent.com/80297522/110934032-99d7ab00-8381-11eb-82eb-ff1caba0b542.png">


The playbook used implements the following tasks:
- Install the docker package, docker.io, python3-pip (the package-management system written in Python which is used to install and manage software packages) and docker.
- Configure the target machine to use more virtual memory when running the ELK container
- Install the docker module using python3-pip.
- Download and launch the docker ELK container, sebp/elk:761.  The image should be start using three specific port mappings: 5601:5601, 9200:9200 and 5044:5044.
- Use the systemd module to configure automatic restart of the docker service when the machine reboots.

The following screenshot displays the result of running docker ps after successfully configuring the ELK instance

<img width="1119" alt="sudo_docker_ps" src="https://user-images.githubusercontent.com/80297522/110933137-7b24e480-8380-11eb-91a0-e19ac2581037.png">



### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- Web-1: 10.1.0.5
- Web-2: 10.1.0.6
- Web-3: 10.1.0.7

We have installed the following Beats on these machines:
- Filebeat
- Metricbeat

These Beats allow us to collect the following information from each machine:

- Filebeat: Filebeat is used to monitor specific log files or locations, as specified by the user.   Filebeat collates and organises this data, which is then forwarded to Elasticsearch or Logstash for indexing.  Filebeat watches for changes in data by monitoring the file system and specific logs – see sample of system log activity below.  As it is specific to a particular machine, Filebeat must be installed on each individual VM/server to be monitored.

<img width="1355" alt="filebeat_image" src="https://user-images.githubusercontent.com/80297522/110933249-a1e31b00-8380-11eb-891e-3b879a865bf9.png">


- Metricbeat: Metricbeat collects and records the metrics of a machine from the operating system and services running on the server, for example CPU and memory usage and container information (see below). These metrics allow the user to assess such things as the health of a network, as well as monitoring for signs of suspicious activity.  As with Filebeat, Metricbeat is specific to a particular machine and must be installed on each individual VM/server which is being monitored.



<img width="1366" alt="metricbeat_image" src="https://user-images.githubusercontent.com/80297522/110933289-ac9db000-8380-11eb-914d-af51df1e6569.png">



### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned:

SSH into the control node and follow the steps below:

- Copy the filebeat-playbook.yml file to the /etc/ansible/roles folder.

- Update the filebeat-config.yml file to include the ELK-VM IP details at lines 1106 and 1806, as follows:
	- 	Configure Elasticsearch output at line 1106: hosts: ["10.0.0.4:9200"]
	- 	Kibana endpoint configuration at line 1806: host: "10.0.0.4:5601"

- Run the playbook: filebeat-playbook.yml.

<img width="863" alt="filebeat_playbook_image" src="https://user-images.githubusercontent.com/80297522/110933350-be7f5300-8380-11eb-8830-3cfdb9e69b9c.png">


Navigate to the Filebeat installation page on the ELK server GUI using the ELK-VM public IP (http://: 40.87.108.196/app/kibana) to check that the installation worked as expected.

Take a screenshot of the result. 

<img width="1402" alt="kibana-homepage" src="https://user-images.githubusercontent.com/80297522/110933378-cb03ab80-8380-11eb-8f80-273a4b92d100.png">




### Bonus

As a Bonus, provide the specific commands the user will need to run to download the playbook, update the files, etc.


See: https://github.com/MadelineXCyber/TAKE-2-ELK-Stack-Deployment-Unit-13/blob/main/Images/Bonus%20question%20-%20Unit%2013%20ELK%20Stack%20Deployment.pdf



### ADDITIONAL MATERIAL


### Diagrams

Additional diagrams featuring each of the networks in Azure can be found by following these links:

- Azure Network Watcher Topology - Red Team Resource Group.png 

<img width="1343" alt="Azure Network Watcher Topology - Red Team Resource Group" src="https://user-images.githubusercontent.com/80297522/110933687-32b9f680-8381-11eb-9c5e-933343074ae5.png">


- Azure Network Watcher Topology - Red Team VNet.png

<img width="1115" alt="Azure Network Watcher Topology - Red Team VNet" src="https://user-images.githubusercontent.com/80297522/110933704-3a799b00-8381-11eb-8eea-1c73583067fd.png">

 

- Azure Network Watcher Topology - ELK-VM VNet.png

<img width="511" alt="Azure Network Watcher Topology - ELK-VM VNet" src="https://user-images.githubusercontent.com/80297522/110933725-3f3e4f00-8381-11eb-82a4-b750af9a242b.png">



### LINUX COMMANDS


### Linux Commands: All commands used to create the ELK Stack deployment

[Linux commands - ELK Stack deployment.pdf](https://github.com/MadelineXCyber/TAKE-2-ELK-Stack-Deployment-Unit-13/blob/main/Linux/Linux%20commands%20-%20ELK%20Stack%20deployment.pdf)



### Linux Commands: WK 3 Lucky Duck

https://github.com/MadelineXCyber/TAKE-2-ELK-Stack-Deployment-Unit-13/blob/main/Linux/LINUX%20COMMANDS:%20WK%203%20Lucky%20Duck



### Linux Commands: WK 4 Linux Systems Administration

https://github.com/MadelineXCyber/TAKE-2-ELK-Stack-Deployment-Unit-13/blob/main/Linux/LINUX%20COMMANDS:%20WK%204%20Linux%20Systems%20Admin



### Linux Commands: WK 5 Archiving and Logging Data

https://github.com/MadelineXCyber/TAKE-2-ELK-Stack-Deployment-Unit-13/blob/main/Linux/LINUX%20COMMANDS:%20WK%205%20Archiving%20and%20Loggi

