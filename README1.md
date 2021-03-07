## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

https://drive.google.com/file/d/1xY1ey-Si962fE9J3IX8S20z-KlHHgnP-/view?usp=sharing

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the YMAL file may be used to install only certain pieces of it, such as Filebeat.

---

This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting inbound access to the network.
- _TODO: What aspect of security do load balancers protect? What is the advantage of a jump box?_
Load Balancers protect the network from DDoS attacks by shifting the attacking traffic from the main server to a public cloud server. 
The advantage of a load balancer is heavy amounts of traffic can be distributed before the server gets overloaded and crashes.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the file systems of the VMs on the network and system metrics.
- _TODO: What does Filebeat watch for?_
Filebeat monitors the log files or a specified location and collects all log events which are fowarded to Logstash for indexing.
- _TODO: What does Metricbeat record?_
Metricbeat takes the metres and statistics of the operating system and changes to the services running on the server.

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump Box | Gateway  | 10.0.0.4   | Linux            |
| DVWA 1   |Web Server| 10.0.0.5   | Linux            |
| DVWA 2   |Web Server| 10.0.0.6   | Linux            |
| ELK      |Monitoring| 10.0.0.8   | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the jump box machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- _TODO: Add whitelisted IP addresses_
107.135.66.232
Machines within the network can only be accessed by each other.
- _TODO: Which machine did you allow to access your ELK VM? What was its IP address?_
The Jumpbox has access to the ELK VM @ 168.61.189.130
A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | Yes                 | 107.135.66.232       |
| ELK      | No                  | 10.0.0.1-254         |
| DVWA1    | No                  | 10.0.0.1-254         |
| DVWA2    | No                  | 10.0.0.1-254         |


### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- _TODO: What is the main advantage of automating configuration with Ansible?_

The main benefit of Ansible is that it automates many repitious daily tasks. This means the developer only has to create one installer for as many computers as the develop needs.

The playbook implements the following tasks:
- _TODO: In 3-5 bullets, explain the steps of the ELK installation play. E.g., install Docker; download image; etc._
- First playbook states its name after PLAY
- Then the playbook has every task listed
- First it retrieves the docker.io
- Then it installs pip3 which an installer for python 3
- Next it installs the python module for Docker
- Then it increases the amount of memory it is going to use
- Then the playbook downloads and launches the docker elk container with the published ports specified.
- Finally the Play RECAP states whether or not the playbook was successful
The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.
 

---
# install_elk.yml
- name: Configure Elk VM with Docker
  hosts: elkservers
  remote_user: elk
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

      # Use command module
    - name: Increase virtual memory
      command: sysctl -w vm.max_map_count=262144

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

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
DVWA 1 10.0.0.5
DVWA 2 10.0.0.6

We have installed the following Beats on these machines:
Filebeat
Metricbeat
Packetbeat

These Beats allow us to collect the following information from each machine:
Filebeat monitors changes to the filesystems like the Apache logs.
Metricbeat monitors changes to the system metrics like CPU/RAM usage. 
Packetbeat collects all the packets that pass through the Network Interface Card.

---
- name: Install metric beat
  hosts: webservers
  become: true
  tasks:
    # Use command module
  - name: Download metricbeat
    command: curl -L -O https://artifacts.elastic.co/downloads/beats/metricbeat/metricbeat-7.4.0-amd64.deb

    # Use command module
  - name: install metricbeat
    command: dpkg -i metricbeat-7.4.0-amd64.deb

    # Use copy module
  - name: drop in metricbeat config
    copy:
      src: /etc/ansible/files/metricbeat-config.yml
      dest: /etc/metricbeat/metricbeat.yml

    # Use command module
  - name: enable and configure docker module for metric beat
    command: metricbeat modules enable docker

    # Use command module
  - name: setup metric beat
    command: metricbeat setup

    # Use command module
  - name: start metric beat
    command: service metricbeat start

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the playbook YMAL file to Ansible Control Node.
- Update the hosts file to include which VMs to run each playbook.
- Run the playbook, and navigate to url http://40.78.24.91:5601 to check that the installation worked as expected.

_TODO: Answer the following questions to fill in the blanks:_
- _Which file is the playbook?
the file is under /etc/ansible/files/filebeat-config.yml
 Where do you copy it?_ it is copied to the /playbooks
- _Which file do you update to make Ansible run the playbook on a specific machine? 
the hosts file under [elk] place the ip of the ELK machine
How do I specify which machine to install the ELK server on versus which to install Filebeat on?_
Under hosts make sure to put the ip address of the machine you would like to install the ELK server. Thttp://40.78.24.91:5601 his is done with elk in brackets.
- _Which URL do you navigate to in order to check that the ELK server is running?
http://40.78.24.91:5601 
_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._