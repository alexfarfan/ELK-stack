## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

https://drive.google.com/file/d/1xY1ey-Si962fE9J3IX8S20z-KlHHgnP-/view?usp=sharing

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the .yml file may be used to install only certain pieces of it, such as Filebeat.

   install-elk.yml

This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly stable, in addition to restricting access to the network.
- _TODO: What aspect of security do load balancers protect? What is the advantage of a jump box?_
Load Balancers protect the network from DDoS attacks and attacks using many logins or heavy traffic. The jump box lets the user configure many machines at the same time.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the database and system logs.
- _TODO: What does Filebeat watch for?_Filebeat collects metrics from the operating system and services on the server.
- _TODO: What does Metricbeat record?_ Metric beat records the metricss such as uptime.

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump Box | Gateway  | 10.0.0.1   | Linux            |
| TODO     |          |            |                  |
| TODO     |          |            |                  |
| TODO     |          |            |                  |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the jumpbox provisioner machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- _TODO: Add whitelisted IP addresses_
107.135.66.232

Machines within the network can only be accessed by jumpbox VM.
- _TODO: Which machine did you allow to access your ELK VM? What was its IP address?_ Jumpbox VM 10.0.0.7

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | Yes/No              | 10.0.0.1 10.0.0.2    |
|          |                     |                      |
|          |                     |                      |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- _TODO: What is the main advantage of automating configuration with Ansible?_
It becomes easy to configure multiple machines and fix configuration errors quickly.

The playbook implements the following tasks:
- _TODO: In 3-5 bullets, explain the steps of the ELK installation play. E.g., install Docker; download image; etc._
- First the playbook installs docker.io
- Next it installs pip3 and the Docker python module
- Then the playbook makes the system use more memory
- Then it downloads and launches the elk container

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![TODO: Update the path with the name of your screenshot of docker ps output](Images/docker_ps_output.png)
ELKansible.PNG

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- _TODO: List the IP addresses of the machines you are monitoring_
10.1.0.4

We have installed the following Beats on these machines:
- _TODO: Specify which Beats you successfully installed_
Filebeat

These Beats allow us to collect the following information from each machine:
- _TODO: In 1-2 sentences, explain what kind of data each beat collects, and provide 1 example of what you expect to see. E.g., `Winlogbeat` collects Windows logs, which we use to track user logon events, etc._

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the configuration file to the ansible container.
- Update the filebeat-config.yml file to include...
the ip address of your ELK machine under hosts at line #1106
and line #1806 put the ip address of the ELK Machine
- Run the playbook, and navigate to Filebeat installation page on ELK server GUI to check that the installation worked as expected.

_TODO: Answer the following questions to fill in the blanks:_

- _Which file is the playbook? Where do you copy it?_
the file is filebeat-playbook.yml and it is copied to the ansible container.
- _Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on?_
You need to update the hosts file and the ansible configuration file. 
Using the machine's ip address, you can specify which machine gets the ELK server install versus which one gets the filebeat.

- _Which URL do you navigate to in order to check that the ELK server is running?
http://[your.VM.IP]:5601/app/kibana. 


_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._

curl https://gist.githubusercontent.com/slape/5cc350109583af6cbe577bbcc0710c93/raw/eca603b72586fbe148c11f9c87bf96a63cb25760/Filebeat > /etc/ansible/files/filebeat-config.yml