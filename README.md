# AndreWilliamsMainRepository
Andre Main IT Folder

## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

(https://github.com/Dredubb31/AndreWilliamsMainRepository/blob/main/Diagrams/Azure%20Diagram.drawio.png) 

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the Ansible Playbook file may be used to install only certain pieces of it, such as Filebeat.

  - firstplaybook.yaml
  - filebeat-playbook.yml
  - metricbeat-playbook.yml

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

| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump Box | Gateway  | 10.0.0.4   | Linux            |
| Elk Serv | Logging  | 10.3.0.4   | Linux            |
| Web 1    | Https    | 10.0.0.7   | Linux            |
| Web 2    | Https    | 10.0.0.8   | Linux            |
| Load     |
| Balancer |
### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Elk Server can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- Workstation Public IP through TCP 5601

Machines within the network can only be accessed by Workstation and Jump-Box-Provisioner. Which machine did you allow to access your ELK VM? What was its IP address?

Jump-Box-Provisioner IP : 10.0.0.4 via SSH port 22
Workstation Public IP via port TCP 5601

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | No                  | Workstation Public IP via port TCP 5601  
           |
| Elk Serv | No                  | 10.3.0.4             |
|  Web1    | No                  | 10.0.0.7             |
|  Web2    | No                  | 10.0.0.8             |
| Load Bal


### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- Ansible allows one to optimize the process in such a way to where it will not waste time repeating the same step, and it improves the overall efficiency of the system.

The playbook implements the following tasks:
- _TODO: In 3-5 bullets, explain the steps of the ELK installation play. E.g., install Docker; download image; etc._
- ...
- ...

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

(Ansible/Sudo Docker PS Screenshot.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- Web 1 & 2 @ 13.89.57.194

We have installed the following Beats on these machines:
- Metricbeat
- Filebeat

These Beats allow us to collect the following information from each machine:
- Filebeat is a lightweight shipper for forwarding and centralizing log data. It monitors the log files or locations that you specify, collects log events, and forwards them to either Elasticsearch or Logstash for indexing.

- Metricbeat is a lightweight shipper that can be installed on a server to periodically collect metrics from the operating system and from services running on the server. It takes the metrics and statistics that it collects and ships them to the output that you specify, such as Elasticsearch or Logstash.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the firstplaybook.yaml file to installelk.yaml.
- Update the installelk.yaml file to include
---
- name: Configure Elk VM with Docker
  hosts: elk
  remote_user: azadmin
  become: true
  tasks:
    # Use apt module
    - name: Install docker.io
      apt:
        update_cache: yes
        name: docker.io
        state: present

      # Use apt module
    - name: Install pip3
      apt:
        force_apt_get: yes
        name: python3-pip
        state: present

      # Use pip module
    - name: Install Docker python module
      pip:
        name: docker
        state: present

      # Use sysctl module
    - name: Use more memory
      sysctl:
        name: vm.max_map_count
        value: "262144"
        state: present
        reload: yes

      # Use docker_container module
    - name: download and launch a docker elk container
      docker_container:
        name: elk
        image: sebp/elk:761
        state: started
        restart_policy: always
        published_ports:
          - 5601:5601
          - 9200:9200
          - 5044:5044

      # Use systemd module
    - name: Enable service docker on boot
      systemd:
        name: docker
        enabled: yes
- Run the playbook, and navigate to ssh elkadmin@<vmipaddress>  and sudo docker ps to check that the installation worked as expected.

_TODO: Answer the following questions to fill in the blanks:_
- _Which file is the playbook? [firstplaybook.yaml] Where do you copy it? nano firstplaybook.yaml
- _Which file do you update to make Ansible run the playbook on a specific machine? cd /etc/ansible/hosts. How do I specify which machine to install the ELK server on versus which to install Filebeat on?_
- _Which URL do you navigate to in order to check that the ELK server is running? http://40.121.38.94:5601/app/kibana#/home

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._
