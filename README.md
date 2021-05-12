# Project-1-Cybersecurity-Bootcamp-ELK
This is the first project I completed in my cybersecurity boot-camp course. This showcases the process and files used in order to create a webserver running DVWA on a docker container configured using Ansible with all webservers being able to communicate to an ELK server.

## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![Images/Network-Diagram-ELK-Project.png]

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the playbook file may be used to install only certain pieces of it, such as Filebeat.

- pentest-elk.yml
- filebeat-playbook.yml
- metricbeat-playbook.yml
- hosts (approximate hosts file on ansible container)

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting unwanted access to the network.
- Load balancers ensure availability of a service; the jump box acts as a hub for everything in our entire virtual network.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the files and system administration.
- Filebeat watches for log files/log events.
- Metricbeat records the metrics from the OS and puts the data in files that can be accessed through logstash and elasticsearch.

The configuration details of each machine may be found below.
| Name                 | Function          | IP Address | Operating System |
|----------------------|-------------------|------------|------------------|
| Jump-Box-Provisioner | Gateway           | 10.0.0.4   | Ubuntu - Linux   |
| Web-1                | DVWA - Web Server | 10.0.0.5   | Ubuntu - Linux   |
| Web-2                | DVWA - Web Server | 10.0.0.6   | Ubuntu - Linux   |
| Web-3                | DVWA - Web Server | 10.0.0.7   | Ubuntu - Linux   |


### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump-Box-Provisioner machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- The physical host public IP address.

Machines within the network can only be accessed by the Ansible container sitting on top of Jump-Box-Provisioner.
- The host public IP address, and 10.0.0.4 (Jump Box Provisioner).

A summary of the access policies in place can be found in the table below.

| Name                 | Publicly Accessible | Allowed IP Addresses                        |
|----------------------|---------------------|---------------------------------------------|
| Jump-Box-Provisioner | Yes                 | Host Machine Public IP Address              |
| Web-1                | No                  | 10.0.0.4                                    |
| Web-2                | No                  | 10.0.0.4                                    |
| Web-3                | No                  | 10.0.0.4                                    |
| ELK                  | Yes                 | Host Machine Public IP Address and 10.0.0.4 |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because configuration of machines becomes scalable while saving on the time it takes to configure each machine individually, in addition it greatly reduces the risk of human error.

The playbook implements the following tasks:
- Install docker
- Download the image
- Make the playbook
- Configure the ansible hosts file to accommodate for your new machine’s IP by typing ‘[elk]’ on one line and ‘[new machine private IP] ansible_python_interpreter=/usr/bin/python3’ on the lines directly beneath it and run the ‘pentest-elk’ file.
- Run the playbook 


The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

-Images/docker-ps-in-elk-vm.png

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- 10.0.0.5
- 10.0.0.6
- 10.0.0.7

We have installed the following Beats on these machines:
- Filebeats
- Metricbeats

These Beats allow us to collect the following information from each machine:
- Filebeat collects log files and log events such as when someone attempts to elevate their permissions in a machine.
- Metricbeat collects data on the current condition of the OS and the services running on that OS such as the current amount of memory being used or the amount of storage left on the system as well as the amount of inbound and outbound data.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the ‘filebeat-config.yml’ file to ‘/etc/filebeat/’.
- Update the ‘filebeat-config.yml’ file to include the IP address of the ELK machine as well as the ports open for that IP. 
- Run the playbook, and navigate to ‘Verify Incoming Data’ on Kibana to check that the installation worked as expected.

- ‘pentest_elk.yml’ configures ELK itself, ‘filebeat-config.yml’, the file that should be located in ‘/etc/ansible/files’ configures filebeat on the elk machine and is obtained from, ‘filebeat-playbook.yml’ tells ansible to make filebeat run on the virtual machines in the network, this is where the ‘filebest-config.yml’ file gets copied from the ansible container into the other mahcines ‘https://gist.githubusercontent.com/slape/5cc350109583af6cbe577bbcc0710c93/raw/eca603b72586fbe148c11f9c87bf96a63cb25760/Filebeat’
- You update the ‘/etc/ansible/hosts’ file by adding the name of the host that the ansible playbook can be applied to ([elk]) and then putting ‘[new machine private IP] ansible_python_interpreter=/usr/bin/python3’ on the lines directly beneath it._
- Go to http://[your-IP]:5601/app/kibana to see if Kibana is running.

-Commands to use-

- In gateway machine -
- sudo docker ps -a
- sudo docker start [name of ansible container]
- sudo docker attach [name of ansible container]

- In ansible container -
- cd /etc/ansible/
- mkdir files (if you don’t already have one)
- curl https://gist.githubusercontent.com/slape/5cc350109583af6cbe577bbcc0710c93/raw/eca603b72586fbe148c11f9c87bf96a63cb25760/Filebeat > /etc/ansible/files/filebeat-config.yml
-nano filebeat-config.yml
- cd /etc/ansible/roles
- nano filebeat-playbook.yml
- ansible-playbook filebeat-playbook.yml
 
