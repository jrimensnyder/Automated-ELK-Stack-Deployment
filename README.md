# Automated-ELK-Stack-Deployment
Creation of an automated ELK Stack on a virtual, secure cloud network.  This network was built using Microsoft Azure Cloud Service.  The ELK Stack was deployed through the Gitbash Terminal.

This document contains the following details:

- Description of the Topology - This section provides a description of the virtual cloud network, its "hardware" (Infrastructure as code), and its security features.

- Access Policies - This section will describe the security access policies for inbound network traffic

- ELK Configuration - This section deccribes how the Elk stack was automatically configured using a series of .yml files.  It will describe:

  - Beats in Use and their functions
  - How each beat was installed
  - Machines Being Monitored
  
- How to Use the Ansible Build

------------------------------------------------------------------------

TOPOLOGY

Below is the link the link diagram for the virtual secure cloud network.

   ![Elk Stack Deployment Network Diagram](https://user-images.githubusercontent.com/75230303/112245192-cd8abe80-8c26-11eb-9ff7-9ad05a68eace.png)

There are eight major components of the Azure Virtual Cloud Network:

1) Azure Resource Group (the outer box in the diagram) - Microsoft.com defines a resource group as "a container that holds related resources for an azure solution.  The resource group can include all the resources for the solution, or only those resources that you want to manage as a group."  In our azure cloud virtual network the resource group contains all network components,, virtual machines and security configurations required to establish a dabase hosted by three virtual machines, all monitored by an ELK Stack server.

2) Azure Virtual Network (the inner box in the diagram) - This is the cloud network created through the execution of Infrastucture as Code (IaC).  IaC enabled by Azure, allows network architects to build completely function networks on the cloud without the traditional physical hardware.  Instead of purchasing and configuring load balancers, servers, firewalls, etc, we are leveraging software and microsoft servers to create a fully-functional network.

3) The Jumpbox.  4sysops.com defines a jumpbox as a controlled entry point into a network.  In our cloud network, the jumpbox establishes a single point of entry for users to Remote Desktop Protocol (RDP) or Secure Shell (SSH) into the virtual network from the outside through port 22.  Establishing access control through one single machine reduces potential points of entry for an attack.  This increases the security of your network, however, if the jumpbox goes down, you can no longer access the private network.

4) The (Network) Security Group. Microsoft.com defines the security group as a set of common rules to "filter network traffic to and from an Azure virtual network.  We will define these rules below when we discuss the access policies for our network.

5) Network Load Balancer. Aws.amazon.com defines a network load balancer as a piece of hardware or IaC (vurtual load balancer) that "automatically distributes your incoming traffic across multiple targets.  In our network, this allows traffic to flow to another virtual machione hopsting the database if one or more machines goes offline.  For example, iof VM Web-1 and VM Web-2 goes offline, a user can still access the database because the laod balancer will send the request to VM Web-3.  This is possible because all three VN Web machines are linked to the network load balancer by grouping the VMs together in the Load Balancer Backend Pool. (See right side of network diagram)

6) Docker VM Web Machines.  These are the virtual machines that host the Database and form the heart of the virtual network. 

7) The ELK-2 Server.  This is the server that monitors the traffic on our network and stores log data.  It is an open source monitoring software.  ELK is an acronym that stands for Elasticsearch, Logstash, Kibana.  

The files listed below and located in ELK Stack Deployment Files in this repository generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the files listed below may be used to setup only certain pieces of of the Elk stack, such as Filebeat.

install-elk.yml

filebeat-config.yml

filebeat-playbook.yml

metricbeat-config-yml

metricbeat-playbook.yml


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly _____, in addition to restricting _____ to the network.
- _TODO: What aspect of security do load balancers protect? What is the advantage of a jump box?_

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the _____ and system _____.
- _TODO: What does Filebeat watch for?_
- _TODO: What does Metricbeat record?_

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

Only the _____ machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- _TODO: Add whitelisted IP addresses_

Machines within the network can only be accessed by _____.
- _TODO: Which machine did you allow to access your ELK VM? What was its IP address?_

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | Yes/No              | 10.0.0.1 10.0.0.2    |
|          |                     |                      |
|          |                     |                      |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- _TODO: What is the main advantage of automating configuration with Ansible?_

The playbook implements the following tasks:
- _TODO: In 3-5 bullets, explain the steps of the ELK installation
