# Automated-ELK-Stack-Deployment
Creation of an automated ELK Stack on a virtual, secure cloud network.  This network was built using Microsoft Azure Cloud Service.  The ELK Stack was deployed through the Gitbash Terminal.

Below is the link the link diagram for the virtual secure cloud network.

![Elk Stack Deployment Network Diagram](https://user-images.githubusercontent.com/75230303/112245192-cd8abe80-8c26-11eb-9ff7-9ad05a68eace.png)

The files listed below and located in ELK Stack Deployment Files in this repository generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the files listed below may be used to setup only certain pieces of of the Elk stack, such as Filebeat.

install-elk.yml


![image](https://user-images.githubusercontent.com/75230303/112247423-aafaa480-8c2a-11eb-957e-640a3b5fe603.png)

![filebeat_playbook_yml](https://user-images.githubusercontent.com/75230303/112248151-da5de100-8c2b-11eb-93e6-ae3590cb316a.PNG)

This document contains the following details:

- Description of the Topology - This section provides a description of the virtual cloud network, its "hardware" (Infrastructure as code), and its security features.

- Access Policies - This section will describe the security access policies for inbound network traffic

- ELK Configuration - This section deccribes how the Elk stack was automatically configured using a series of .yml files.  It will describe:

  - Beats in Use and their functions
  - How each beat was installed
  - Machines Being Monitored
  
- How to Use the Ansible Build


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
