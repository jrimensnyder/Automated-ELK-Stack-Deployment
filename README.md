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

## Description of the TOPOLOGY

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application (Server)

Below is the link the link diagram for the virtual secure cloud network.

![Elk Stack Deployment Network Diagram](https://github.com/jrimensnyder/Automated-ELK-Stack-Deployment/blob/main/IMAGES/Elk%20Stack%20Deployment%20Network%20Diagram.png)

There are nine major components or our Azure Virtual Cloud Network:

1) Azure Resource Group (the outer box in the diagram) - Microsoft.com defines a resource group as "a container that holds related resources for an azure solution.  The resource group can include all the resources for the solution, or only those resources that you want to manage as a group."  In our azure cloud virtual network the resource group contains all network components,, virtual machines and security configurations required to establish a dabase hosted by three virtual machines, all monitored by an ELK Stack server.

2) Azure Virtual Network (the inner box in the diagram) - This is the cloud network created through the execution of Infrastucture as Code (IaC).  IaC enabled by Azure, allows network architects to build completely function networks on the cloud without the traditional physical hardware.  Instead of purchasing and configuring load balancers, servers, firewalls, etc, we are leveraging software and microsoft servers to create a fully-functional network.

3) The Jumpbox.  4sysops.com defines a jumpbox as a controlled entry point into a network.  In our cloud network, the jumpbox establishes a single point of entry for users to Remote Desktop Protocol (RDP) or Secure Shell (SSH) into the virtual network from the outside through port 22.  Establishing access control through one single machine reduces potential points of entry for an attack.  This increases the security of your network, however, if the jumpbox goes down, you can no longer access the private network.

4) The (Network) Security Group. Microsoft.com defines the security group as a set of common rules to "filter network traffic to and from an Azure virtual network.  We will define these rules below when we discuss the access policies for our network.

5) Network Load Balancer. Aws.amazon.com defines a network load balancer as a piece of hardware or IaC (vurtual load balancer) that "automatically distributes your incoming traffic across multiple targets.  In our network, this allows traffic to flow to another virtual machione hopsting the database if one or more machines goes offline.  For example, iof VM Web-1 and VM Web-2 goes offline, a user can still access the database because the laod balancer will send the request to VM Web-3.  This is possible because all three VN Web machines are linked to the network load balancer by grouping the VMs together in the Load Balancer Backend Pool. (See right side of network diagram).  Load balancing ensures that the application will be highly accessible, in addition to restricting unauthorized access to the network through the security group firewall.

6) Docker VM Web Machines.  These are the virtual machines that host the Database and form the heart of the virtual network. 

7) The ELK-2 Server.  This is the server that monitors the traffic on our network and stores log data.  It is an open source monitoring software.  ELK is an acronym that stands for Elasticsearch, Logstash, Kibana.  These are the three components of an ELK Stack. Accoring to the creators website elastic.com, "Elasticsearch is a search and analytic engine that searches and analyzes incoming network traffic.  Logstash is a server-side data processing pipeline that ingests data from multiple sources simultaneously, transforms it, and sends it to a "stash" (in this case Elastic Search) to be evaluated. Kibana is open source software that enalbes users to visualize data with charts and graphs in Elasticsearch.  Integrating an ELK server allows us to easily monitor the vulnerable VMs for changes to the files and system configurations.

Below is a simple ELK Stack Diagram that illustrates the flow of log data from the Web Virtual Machines through the stack.

![Elk Stack Flow Diagram](https://github.com/jrimensnyder/Automated-ELK-Stack-Deployment/blob/main/IMAGES/ELK%20Stack%20Flow%20Diagram.png) 

      - Two Monitoring services installed on our ELK Server:
      
          1 - filebeat - Elastic.co defines filebeat as lightweight software that forwards and centralizes log data.  It is installed on the elk server to monitor user specified log siles or locations, collecting log events and forwarding them to Elasticsearch or Logtash for indexing.  The bottom line is filebeat looks for user defined changes to the system or its files/

          2 - metricbeat - Elastic.com defines metricbeat as lighweight software "you can install on servers tp periodically collect metric from the operating system and from services running on the server.  Metricbeat takes the metrics and statistics that it collects and ships them to the output you specify."  In our case the metrics and statistics are sent to Elasticsearch and logstash.

8) The internet. The internet enables users with proper access the medium to SSH through the jumpbox into our virtual cloud network,  This enables access to the database hosted by our virtual machines.

9) Local workstation.  This is where is all begins.  The local machine access the internet to initiate remote access into the private network.

-------------------------------------------------------------------------------------------------------------------------------------------------------------

## Virtual Machine Configuration

The configuration details of each machine may be found below.

| Name         | Function              | IP Address    | Operating System |
|--------------|-----------------------|---------------|------------------|
| Jumpbox      | Gateway               | 52.177.217.63 | Linux Ubuntu     |
| ELK-2        | ELK Stack Host        | 10.1.0.5      | Linux Ubuntu     |
| Web-1        | DVWA Application Host | 10.0.0.9      | Linux Ubuntu     |
| Web-2        | DVWA Application Host | 10.0.0.10     | Linux Ununtu     |
| Web-3        | DVWA Application Host | 10.0.0.12     | Linux Ubuntu     |
| Loadbalancer | Distributes Traffic   | 40.87.99.10   | N/A              |

-------------------------------------------------------------------------------------------------------------------------------------------------------------

## Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the jumpbox machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- 100.15.241.202 - Public IP address of my (James Rimensnyder's Desktop Computer) - The firewall allows SSH access (port 22) from this IP address and HTTP traffic (port 80).

IMPORTANT NOTE - All access rules are inbound rules.  The chart below represents which machines can be accessed port 80 (HTTP) and which can be accessed through port 22 (SSH).

- Once Again - Only the Jumpbox can be accessed via port 22 from IP 100.15.241.202

| Names   | Public Accessible | Allowed IP Address                                                    |
|---------|-------------------|-----------------------------------------------------------------------|
| Jumpbox | Yes               | (SSH) 100.15.241.202, (HTTP) 10.1.0.5, 10.0.0.9, 10.0.0.10, 10.0.0.12 |
| ELK-2   | No                | (SSH) 10.0.0.4, (HTTP)10.0.0.9, 10.0.0.10, 10.0.0.12                  |
| Web-1   | No                | (SSH) 10.0.0.4, (HTTP) 10.1.0.5, 10.0.0.9, 10.0.0.10, 10.0.0.12       |
| Web-2   | No                | (SSH) 10.0.0.4, (HTTP) 10.1.0.5, 10.0.0.9, 10.0.0.10, 10.0.0.12       |
| Web-3   | No                | (SSH) 10.0.0.4, (HTTP) 10.1.0.5, 10.0.0.9, 10.0.0.10, 10.0.0.12       |

Below is a screen shot of the Security Group for the VM network.  This Azure Network Security Group (Security_Group_Rimy) has four key rules that generate the access configurations listed above.

Rule #1 - Port 80 - This rule establishes HTTP access from my personal desktop IP 100.15.241.202 to the virtual network.  This includes the Jumpbox, ELK-2, Web-1, Web-2, amnd Web-3.

Rule #2 - Elk-SSH - This rule allows SSH (port 22) access from the Jumpbox's public IP address (10.0.0.4) to ELK-2

Rule #3 - SSH-from-Jump - This rule allows access through port 22 from the Jumpbox to the the virtual machines.

![Security Group Rimy](https://github.com/jrimensnyder/Automated-ELK-Stack-Deployment/blob/main/IMAGES/Security%20Group%20Rimy.PNG)


Machines within the network can only be accessed by the ansible docker risiding on the Jumpbox VM.

-The ELK-2 VM can only be accessed by the ansible docker on the Jumpbox VM.  It is accessed through the jumpbox public IP address 10.0.0.4.  This is accomplished through the ansible docker condescending_napier located on the Jumpbox VM.

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------

AUTOMATED DEPLOYMENT of the ELK STACK

The files listed below and located in ELK Stack Deployment Files in this repository generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the files listed below may be used to setup only certain pieces of of the Elk stack, such as Filebeat.

install-elk.yml

filebeat-config.yml

filebeat-playbook.yml

metricbeat-config-yml

metricbeat-playbook.yml




### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- _TODO: What is the main advantage of automating configuration with Ansible?_

The playbook implements the following tasks:
- _TODO: In 3-5 bullets, explain the steps of the ELK installation
