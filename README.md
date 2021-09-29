# AndreWilliamsMainRepository
Andre Main IT Folder

## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

(https://github.com/Dredubb31/AndreWilliamsMainRepository/blob/main/Diagrams/Azure%20Diagram.drawio.png) 

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the Ansible Playbook file may be used to install only certain pieces of it, such as Filebeat.

Ansible Files <https://github.com/Dredubb31/AndreWilliamsMainRepository.git>
  - Ansible Playbook
  - Ansible Hosts
  - Ansible Configuration
  - Ansible Elk Installation and VM Configuration
  - Ansible Filebeat Playbook
  - Ansible Filebeat Configuration
  - Ansible Mtericbeat Playbook
  - Ansible Metricbeat Configuration 

Download the ansible.cfg configuration file on this website https://ansible.com/ and edit or copy Ansible Configuration to your /etc/ansible directory

For ansible.cfg edit:
cd /etc/ansible/	
nano ansible.cfg
CTRL + W > enter remote_user
change `remote_user = sysadmin`
Assign username and SSH Public Key for Web1, Web2, ELK Virtual Machine in Azure GUI

Web1 / Web2 / ELK Server > Reset Password > Reset SSH Public Key
  username: sysadmin
  SSH Key : copy id_rsa.pub from the ansible control node in .ssh/ directory. 
To get the SSH Key run this command:
~/.ssh# ssh-keygen
~/.ssh# cat id_rsa.pub

This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available and reliable, in addition to restricting access to the network.
- Load balancers protects the system from DDoS attacks by shifting attack traffic.

- The advantages of having a jumpbox are, it's the only gateway for access to your infrastructure reducing the size of any potential attack surface. Having a dedicated SSH access point also makes it easier to have an aggregated audit log of all SSH connections.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the data and system logs.

- What does Filebeat watch for? Filebeat is a lightweight shipper for forwarding and centralizing log data. Installed as an agent on your servers, Filebeat monitors the log files or locations that you specify, collects log events, and forwards them either to Elasticsearch or Logstash for indexing.

- What does Metricbeat record? Metricbeat is a lightweight shipper that you can install on your servers to periodically collect metrics from the operating system and from services running on the server. Metricbeat takes the metrics and statistics that it collects and ships them to the output that you specify, such as Elasticsearch or Logstash.

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name         | Function     | IP Address             | Operating System |
|--------------|--------------|------------------------|------------------|
| Jump Box     | Gateway      | 10.0.0.4/40.122.158.164| Linux            |
| Elk          | Elk Server   | 10.3.0.4/40.121.38.94  | Linux            |
| Web 1        | Https        | 10.0.0.7               | Linux            |
| Web 2        | Https        | 10.0.0.8               | Linux            |
| Load Balancer| Load Balancer| 13.89.57.194           | Linux            |
### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Elk Server can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- Workstation Public IP through TCP 5601

Machines within the network can only be accessed by Workstation and Jump-Box-Provisioner. Which machine did you allow to access your ELK VM? What was its IP address?

  - Jump-Box-Provisioner IP : 10.0.0.4 via SSH port 22
  - Workstation Public IP via port TCP 5601

A summary of the access policies in place can be found in the table below.

| Name          | Publicly Accessible | Allowed IP Addresses                         |
|---------------|---------------------|----------------------------------------------|
| Jump Box      | No                  | Work station Public IP using  TCP 5601Port 22|
| Elk Serv      | No                  | Work station Public IP using  TCP 5601Port 22|                                    
|  Web1         | No                  | 10.0.0.7 on SSH 22                           |
|  Web2         | No                  | 10.0.0.8 on SSH 22                           |
| Load Balancer | No                  | Work station Public IP on HTTP 80            |


### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- Ansible allows one to optimize the process in such a way to where it will not waste time repeating the same step, and it improves the overall efficiency of the system.

The playbook implements the following tasks:
- _Specify a different group of machines as well as a different remote user
  - name: Config elk VM with Docker
    hosts: elk
    remote_user: sysadmin
    become: true
    tasks:
    
Increase System Memory :
 - name: Use more memory
  sysctl:
    name: vm.max_map_count
    value: '262144'
    state: present
    reload: yes
    
Install the following services:
   `docker.io`
   `python3-pip`
   `docker`, which is the Docker Python pip module.
   
Launching and Exposing the container with these published ports:
 `5601:5601` 
 `9200:9200`
 `5044:5044`
 
The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

https://github.com/Dredubb31/AndreWilliamsMainRepository/blob/main/Diagrams/Sudo%20Docker%20PS%20Screenshot.png?raw=true![image](https://user-images.githubusercontent.com/91231328/135185171-30436b66-3214-449b-98ed-72f05482898d.png)


### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- Web 1: 10.0.0.7
- Web 2: 10.0.0.8

We have installed the following Beats on these machines:
- Elk Server
- Web 1
- Web 2
- Metricbeat
- Filebeat

These Beats allow us to collect the following information from each machine:
- Filebeat is a lightweight shipper for forwarding and centralizing log data. It monitors the log files or locations that you specify, collects log events, and forwards them to either Elasticsearch or Logstash for indexing.

- Metricbeat is a lightweight shipper that can be installed on a server to periodically collect metrics from the operating system and from services running on the server. It takes the metrics and statistics that it collects and ships them to the output that you specify, such as Elasticsearch or Logstash.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

For ELK VM Configuration:

  - Copy the Ansible ELK Installation and VM Configuration
  - Run the playbook using this command : ansible-playbook install-elk.yml

For FILEBEAT:

Download Filebeat playbook usng this command:
    - curl -L -O 
     https://github.com/Dredubb31/AndreWilliamsMainRepository.git > Ansible/Filebeat-playbook.yml

 - Copy the '/etc/ansible/files/filebeat-config.yml' file to '/etc/filebeat/filebeat-playbook.yml'
 - Update the filebeat-playbook.yml file to include installer
    - curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.6.1-amd64.deb
 - Update the filebeat-config.yml file root@c1e0a059c0b0:/etc/ansible/files# nano filebeat-config.yml

output.elasticsearch:
  #Array of hosts to connect to.
 hosts: ["10.3.0.4:9200"]
  username: "elastic"
  password: "changeme” 

 setup.kibana:
  host: "10.3.0.4:5601"
  
 - Run the playbook using this command ansible-playbook filebeat-playbook.yml and navigate to Kibana > Logs : Add log data > System logs > 5:Module Status >  
   Check data to check that the installation worked as expected.

For METRICBEAT:

  - Download Metricbeat playbook using this command:
      - curl -L -O 
          https://github.com/Dredubb31/AndreWilliamsMainRepository.git > Ansible/Metricbeat-playbook.yml
      
  - Copy the /etc/ansible/files/metricbeat file to /etc/metricbeat/metricbeat-playbook.yml
  - Update the filebeat-playbook.yml file to include installer
      - curl -L -O https://artifacts.elastic.co/downloads/beats/metricbeat/metricbeat-7.6.1-amd64.deb
  - Update the metricbeat file rename to metricbeat-config.yml

root@c1e0a059c0b0:/etc/ansible/files# nano metricbeat-config.yml

output.elasticsearch:
#Array of hosts to connect to.
hosts: ["10.3.0.4:9200"]
  username: "elastic"
  password: "changeme"

setup.kibana:
  host: "10.3.0.4:5601"
  
Run the playbook, (ansible-playbook metricbeat-playbook.yml) and navigate to Kibana > Add Metric Data > Docker Metrics > Module Status to check that the installation worked as expected.

ADDITONAL NOTES:

How to get Filebeat installer :

  1. Login to Kibana > Logs : Add log data > System logs > DEB > Getting started
  2. Copy: curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.6.1-amd64.deb
  
How to get the Metricbeat installer:

  1. Login to Kibana > Add Metric Data > Docker Metrics > DEB > Getting Started
  2. Copy: curl -L -O https://artifacts.elastic.co/downloads/beats/metricbeat/metricbeat-7.6.1-amd64.deb
  
Which file is the playbook? Where do you copy it?

 - Answer : For the ANSIBLE : We will create the my-playbook1.yml as our playbook.

See the final solution of the Ansible Playbook

 - Answer : For FILEBEAT: We will create filbeat-playbook.yml as our playbook.

See the final solution of the Filebeat Playbook

 - Answer: For METRICBEAT: We will create metricbeat-playbook.yml as our playbook.

See the final solution of the Metricbeat Playbook

Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to         install Filebeat on?

How to Download and Edit the Ansible Configuration file

- root@c1e0a059c0b0:/etc/ansible# curl -L -O https://ansible.com/  > ansible.cfg
- root@c1e0a059c0b0:/etc/ansible# nano ansible.cfg

- Press CTRL + W (to search > enter remote_user then change `remote_user = sysadmin`

Where : `sysadmin` is the remote user that has control over ansible.

How to Edit the Ansible Hosts file in this directory /etc/ansible/hosts

  #List the IP Addresses of your webservers
  #You should have at least 2 IP addresses

[webservers]
10.0.0.4 ansible_python_interpreter=/usr/bin/python3
10.0.0.7 ansible_python_interpreter=/usr/bin/python3
10.0.0.8 ansible_python_interpreter=/usr/bin/python3

#List the IP address of your ELK server
#There should only be one IP address
[elk]
10.3.0.4 ansible_python_interpreter=/usr/bin/python3

Where: [webservers] and [elk] are the group of machines and each group has 1 or more members.

How to Create the ELK Installation and VM Configuration in the /etc/ansible/ directory:

See the final solution of the Ansible ELK Installation and VM Configuration

Specify a different group of machines as well as a different remote user

      - name: Config elk VM with Docker
        hosts: elk
        remote_user: sysadmin
        become: true
        tasks:
        
Where: [elk] is the Virtual Machine hosts or the group of machine targetted for this installation and can only be done by a `sysadmin` remote_user

How to Copy the raw Filebeat Module Configuration file from web to the /etc/ansible/files directory:

  - `curl -L -O 
    https://gist.githubusercontent.com/slape/5cc350109583af6cbe577bbcc0710c93/raw/eca603b72586fbe148c11f9c87bf96a63cb25760/Filebeat > /etc/ansible/files/filebeat-config.yml'
    
    - Note : The filebeat-config.yml as our filebeat configuration file.

See the final solution of the Filebeat Config file

hosts: ["10.3.0.4:9200"]
  username: "elastic"
  password: "changeme" 

setup.kibana:
  host: "10.3.0.4:5601"
Where: hosts: ["10.3.0.4:9200"] is the ELK VM that can install Filebeat

How to Copy the raw Metricbeat Module Configuration from web to the /etc/ansible/files/ directory:

  -`curl -L -O https://gist.githubusercontent.com/slape/58541585cc1886d2e26cd8be557ce04c/raw/0ce2c7e744c54513616966affb5e9d96f5e12f73/metricbeat >   /etc/ansible/files/metricbeat-config.yml'
   - Note : the metricbeat-config.yml as our metricbeat configuration file. ```

See the final solution of the Metricbeat Config file

hosts: ["10.3.0.4:9200"]
  username: "elastic"
  password: "changeme" 

setup.kibana:
  host: "10.3.0.4:5601"
Where: hosts: ["10.3.0.4:9200"] is the ELK VM that can install Metricbeat

  - Which URL do you navigate to in order to check that the ELK server is running?
     - Test Kibana on web : http://[your.ELK-VM.External.IP]:5601/app/kibana
     - Test Kibana on localhost: sysadmin@10.3.0.4: curl localhost:5601/app/kibana

Other Linux Command List :

https://phoenixnap.com/kb/wp-content/uploads/2021/04/Docker-commands-cheat-sheet-by-PhoenixNAP-scaled.jpg
