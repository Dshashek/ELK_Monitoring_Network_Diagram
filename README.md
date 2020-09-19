## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

<img src="https://github.com/Dshashek/ELK_Monitoring_Network_Diagram/blob/master/Images/network_diagram.png">

The YAML
The .yml files in the yml directory were used to deploy DVWA on web servers and ELK on a separate virtual network, all in Azure.  The initial setup of machines, network security groups, the load balancer, and virtual networks were set up through the Azure GUI on an Azure trail account.

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose load-balanced and monitored instances of DVWA, the D*mn Vulnerable Web Application.

Load balancing, the practice of using redundant machines run the same application and distribute connections based on current system load to ensure a high level of availability and a more responsive experience for users.  Azure Load balancers also allow inbound NAT rules to add the ability to restrict inbound traffic to the network.

Configuration of the web servers and elk machine was accomplished from an ansible container running in docker on a jump box machine.  By creating a single secure access point, a jump box, from which to configure other machines on the network, we can more easily monitor connections to those machines.  By using a machine with limited or no access out to the public internet, rather than local administration on the managed machine we significantly reduce the likelihood of compromising an account with heighted administrative privileges.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the server metrics and system files.

Two beats were configured, filebeat to monitor changes to files on the monitored systems, and metricbeat to monitor changes to information from the operating system and services running on the monitored systems.

The configuration details of each machine may be found below.

| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump Box | Gateway  | 10.0.0.4   | Ubuntu 18.04-LTS |
| Web-1    | Webserver| 10.0.0.8   | Ubuntu 18.04-LTS |
| Web-2    | Webserver| 10.0.0.9   | Ubuntu 18.04-LTS |
| Web-3    | Webserver| 10.0.0.10  | Ubuntu 18.04-LTS |
| ELK      | Monitoring| 10.1.0.4   | Ubuntu 18.04-LTS |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

The elk machine and web servers can be reached on http from the internet but only through the public ip address of my home network.

Machines within the network can only be accessed by ssh from the Jump Box.

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Red Team NSG | No              | home ip   |
| Elk NSG      | No                    | home ip/10.0.0.4                     |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because we can ensure that all systems are set up consistently, and easily configure new machines in the future by making modifications only to the hosts configuration file in the ansible container.

The playbook implements the following tasks:
- Uninstall apache2 so it does not conflict with docker
- Install pip3 package manager to download python packages
- Install Python docker module
- Start Docker service, increase virtual memory on elk machine
- Download and launch the elk docker contianer

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

<img src="https://github.com/Dshashek/ELK_Monitoring_Network_Diagram/blob/master/Images/docker_ps.png">


### Target Machines & Beats
This ELK server is configured to monitor the following machines:
10.0.0.8
10.0.0.9
10.0.0.10

We have installed the following Beats on these machines:
metricbeat
filebeat

These Beats allow us to collect the following information from each machine:
- _TODO: In 1-2 sentences, explain what kind of data each beat collects, and provide 1 example of what you expect to see. E.g., `Winlogbeat` collects Windows logs, which we use to track user logon events, etc._

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Create an SSH key in the ansible container and use it to update the SSH keys in Azure for the Web VMs and the Elk machine.
- SSH into each of the web machines from the control node and add the IP address.
- Modify the remote_user in ansible.cfg file in /etc/ansible on the jumpbox to the user name used for the webservers and elk machine.
- Update the Hosts file with two groups, one for elk, one for the webservers
- Download the playbooks for the webservers, elk, elastic beat, and metric beat onto your ansible container.
- Place the beats config files in /etc/ansible/files and update the beats configuration files to include the IP address of the Elk Server 
- Run the playbooks, and navigate to <elk machine ip>:5601/app/kibana to check that the installation worked as expected.

_TODO: Answer the following questions to fill in the blanks:_
- _Which file is the playbook? Where do you copy it?_
- _Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on?_
- _Which URL do you navigate to in order to check that the ELK server is running?

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._
