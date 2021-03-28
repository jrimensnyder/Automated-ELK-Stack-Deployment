# Automated-ELK-Stack-Deployment

Creation of an automated ELK Stack on a virtual, secure cloud network.  This network was built using Microsoft Azure Cloud Service.  The ELK Stack was deployed through the Gitbash Terminal.

This document contains the following details:

- Description of the Topology - This section provides a description of the virtual cloud network, its "hardware" (Infrastructure as code), and its security features.

- Jumpbox configuration and use of ansible docker container to provision virtual machine hosting the DVWA Web APP.

- Access Policies - This section will describe the security access policies for inbound network traffic

- ELK Configuration - This section deccribes how the Elk stack was automatically configured using a series of .yml files.  It will describe:

  - Beats in Use and their functions
  - How each beat was installed
  - Machines Being Monitored
  
- How to Use the Ansible Build - I will conduct an investigation into traffic on our virtual network through our ELK Stack Server containing Elasticsearch Logstash, and Kibana.

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

## AUTOMATED DEPLOYMENT of the ELK STACK

The files listed below and located in ELK Stack Deployment Files in this repository generate a live ELK deployment configuration on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the files listed below may be used to setup only certain pieces of of the Elk stack, such as Filebeat.  These files are located in the Elk Stack Deployment Folder at the top of the github repository.

install-elk.yml

filebeat-config.yml

filebeat-playbook.yml

metricbeat-config.yml

metricbeat-playbook.yml

---------------------------------------------------------------------------------------------------------------------------------------------------------------------------

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because it enables clourd engineers to quickly buld virtual networks using the principle of Infratructure as Code (IaC)

Ansible is a simple information technology "automation engine that automates cloud provisioning, configuration, management" and relationships between network nodes. (ansible.com).  Ansible uses the simple programming langauge YAML Ain't Markup Language (YAML).  This language is used to make simple programs called Ansible Playbooks that will configure virtual machines.

Our ELK Deployment was accomplished using five YAML programs or playbooks.  A description of each program is provided below.

Install-Elk.yml

This YAML playbook installs a docker container on the ELK-2 Virtual Machine.  Below are the tasks completed by the Install-Elk.yml.

    - Installs docker.io - this is the container platform that will be used to house the software programs used by
      the VM to operate the ELK stack.
      
    - Installs python3-pip.  This is a package management system written in python programming language used to install 
      and manage software packages. It connects the virtual machine to an online repository for public and paid for 
      private software packages.

    - Installs Doocker module.  Docker module retrieves metrics from docker containers  

    - Downloads, launches and enables on boot a docker elk container.  This is the docker container that houses the log server and management 
      web interface consisting of Elasticsearch, Logstash, and Kibana.
      
Below is a screen capture of the gitbash terminal when you run the install-elk.yml playbook.  Notice that the YAML file only uploads the container and associated files
on the Elk-2 machine with a private IP address of 10.1.0.5.  This is because the YAML file only targets IP addresses under the ELK hosts.  Please see the Ansible Host File in the Jumpbox ansible files for more details.

![Install Elk YAML File](https://github.com/jrimensnyder/Automated-ELK-Stack-Deployment/blob/main/IMAGES/Install%20ELK%20YAML%20File%20through%20Bash.PNG)

-------------------------------------------------------------------------------------------------------------------------------------------------------------------------

filebeat-playbook.yml

This YAML Playbook installs filebeat. As discussed above, filebeat is software that forwards and centralizes log data specified by the user.  
This playbook executes the following:

    - It downloads the filebeat debian file from the artifacts.elastic.co website using curl (its a GET of the debian file)
    - It installs the filebeat debian file on the elk server and virtual machines (Web-1,2,3)
    - It drops the filebeat configuration file into the approriate /etc/ file on the virtual machines.
    - It enables and configures the filebeat module and sets up the filebeat service
    - and finally, it starts the service

Below is a screen capture of the gitbash terminal when you run the filebeat-playbook.yml.

![Install Filebeat YAML File in Bash](https://github.com/jrimensnyder/Automated-ELK-Stack-Deployment/blob/main/IMAGES/Install%20Filebeat-Playbook-yml%20through%20Bash.PNG)

---------------------------------------------------------------------------------------------------------------------------------------------------------------------

metricbeat-playbook.yml

This YAML playbook installs metricbeat to the virtual machines.  Metricbeat, as explined above, is software you can install on servers tp periodically collect metric from the operating system and from services running on the server.  The YAML file executes the following tasks:

    - It downloads the metric debian file from the artifacts.elastic.co website using curl (its a GET of the debian file)
    - It installs the metricbeat debian file on the elk server and virtual machines (Web-1,2,3)
    - It drops the mertricbeat configuration file into the approriate /etc/ file on the virtual machines.
    - It enables and configures the metricbeat module and sets up the metricbeat service
    - and finally, it starts the service 

Below is a screen capture of the gitbash terminal when you run the metricbeat-playboon.yml.  Note, with both the filebeat and metricbeat YAMLs, the files are uploaded to IPs 10.0.0.9, 10.0.0.10, 10.0.0.12.  These IPs are the three web machines that host our web app.  This is because the YAML file only targets the IP address under the host file.

![Install Metricbeat YAML File in Bash](https://github.com/jrimensnyder/Automated-ELK-Stack-Deployment/blob/main/IMAGES/Install%20Metricbeat%20YAML%20through%20Bash.PNG)

------------------------------------------------------------------------------------------------------------------------------------------------------------------------

## Now that out virtual network and ELK stack monitoring is established, lets test it's functionality.

We need to vertify the ELK server is working as expected and pulling both logs and metrics from the web servers hosting the DVWA app (Web-1,2,3).
We will accomplish this by running three tests.

Test # 1 - Generate a high amount of failed SSH login attempts from the jumpbox ansible docker to the web servers and verify that Kibana is picking up this activity.

First, I will attempt to ssh into the web machines and the elk server using a vairety of incorrect usernames.  Below is a screenshot of the gitbash commands.

![Failed SSH Attempts to Webservers](https://github.com/jrimensnyder/Automated-ELK-Stack-Deployment/blob/main/IMAGES/GItbash%20Failed%20SSH%20Attempts.PNG)


Generate a high amount of CPU usage on the pen-testing machines and verify that Kibana picks up this data.


Generate a high amount of web requests to your pen-testing servers and make sure that Kibana is picking them up.


These activities will guide you though generating some data to visualize in Kibana. Each of these activity will require the following high level steps:

Use your jump-box to attack your web machines in various ways.
Use a Linux utility to stress the system of a webVM directly.
Subsequently generate traffic and logs that Kibana will collect.
View that traffic in various ways inside Kibanna.
