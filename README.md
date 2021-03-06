## Automated ELK Stack Deployment

The files in the yml_files and config_files directories were used to configure the network depicted below.

<img src="https://github.com/Dshashek/ELK_Monitoring_Network_Diagram/blob/master/Images/network_diagram.png">

The YAML

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose load-balanced and monitored instances of DVWA, the D*mn Vulnerable Web Application.

This network was created with Azure, Microsoft's cloud computing service.  Two Virtual Networks were created.  In the first, we set up a Jump Box as well as three virtual machines behind a load balancer.  In the second, we set up a single virtual machine to be used for monitoring of the three virtual machines in the other network.  We then created a peering between the two networks. 

Load balancing is the practice of using redundant machines to run the same application and distribute connections based on current system load to ensure a high level of availability and a more responsive experience for users.  Azure Load balancers also allow inbound NAT rules which adds the ability to restrict inbound traffic to the network.

Configuration of the web servers and ELK machine was accomplished from an ansible container running in docker.  By creating a single secure access point, a jump box, from which to configure other machines on the network, we can more easily monitor connections to those machines.  By using a machine with limited or no access out to the public internet, rather than local administration on the managed machine, we significantly reduce the likelihood of compromising an account with heighted administrative privileges.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the server metrics and system files.

The configuration details of each machine may be found below.

| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump Box | Gateway  | 10.0.0.4   | Ubuntu 18.04-LTS |
| Web-1    | Webserver| 10.0.0.8   | Ubuntu 18.04-LTS |
| Web-2    | Webserver| 10.0.0.9   | Ubuntu 18.04-LTS |
| Web-3    | Webserver| 10.0.0.10  | Ubuntu 18.04-LTS |
| ELK      | Monitoring| 10.1.0.4   | Ubuntu 18.04-LTS |

### Access Policies

A summary of the access policies in place can be found in the table below.

|**Red Team NSG**| | | | |
|:-------------:|:-----------:|:--------------:|:----:|:--------:|
|Name|Ports|Protocol|Source|Destination|
|allow_ssh| 22 | TCP | Home IP | 10.0.0.4|
|http_from_home | 80 | TCP | Home IP | Virtual Network |
|**ELK Server NSG**| | | | |
|Name|Ports|Protocol|Source|Destination|
|ssh_from_jumpbox         | 22 | TCP | Home IP | 10.0.0.4|
|access_from_home         | 80 | TCP | Jump Box Public IP | 10.1.0.4 |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because we can ensure that all systems are set up consistently, and we can easily configure new machines in the future by making modifications only to the hosts configuration file in the ansible container.

The playbook implements the following tasks:
- Uninstall apache2 so it does not conflict with docker
- Install pip3 package manager to download python packages
- Install Python docker module
- Start Docker service, increase virtual memory on ELK machine
- Download and launch the ELK docker container

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

<img src="https://github.com/Dshashek/ELK_Monitoring_Network_Diagram/blob/master/Images/docker_ps.png">


### Target Machines & Beats
This ELK server is configured to monitor the following webservers:
10.0.0.8
10.0.0.9
10.0.0.10

We have installed the following Beats on these machines:

Filebeat will allow us to specify specific files to monitor and will log changes to those files.  This could be used to collect new lines from log files or to monitor changes to important system files like /etc/shadow.

Metricbeat will allow us to specify specific information about services and programs running on the monitored machines.  For instance, you could use filebeat to monitor the CPU load, or the available drive space on monitored machines.

### Using the Playbooks
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned and the machines are set up properly in Azure: 

SSH into the control node and follow the steps below:
- Create an SSH key in the ansible container and use it to update the SSH keys in Azure for the Web VMs and the ELK machine.
- SSH into each of the web machines from the control node and add the IP addresses.
- Modify the remote_user in ansible.cfg file in /etc/ansible on the jump box to the username used for the webservers and ELK machine.
- Update the Hosts file with two groups, one for ELK, one for the webservers
- Download the playbooks for the webservers, ELK, elastic beat, and metric beat onto your ansible container.
- Place the beats config files in /etc/ansible/files and update the beats configuration files to include the IP address of the ELK Server 
- Run the playbooks and navigate to <ELK machine ip>:5601/app/kibana to check that the installation worked as expected.
