## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![Diagram](ELK.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the playbook file may be used to install only certain pieces of it, such as Filebeat.

   

This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly resilient to DoS attacks, in addition to restricting outside access to the network.

- Load balancers protect a networks availability by helping mitigate the risks of DoS attacks against a network. 

- A jump box manages access to all machines on a network and helps secure a network by restricting and controlling the flow of outside traffic into specific zones of the network it is deployed on. 

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the data and system logs.

- Filebeat collects data about the file system 
- Metricbeat collects system metrics such as uptime 

The configuration details of each machine may be found below.
 
| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump Box | Gateway  | 10.0.0.4   | Linux            |
| Web-1    | DVWA     | 10.0.0.5   | Linux            |
| Web-2    | DVWA     | 10.0.0.6   | Linux            |
| ELK      | ELK Stack| 10.2.0.4   | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the load balancer machine can accept connections from the Internet. Access to this machine is only allowed from my workstation at home, with the following white listed IP:
- 99.230.130.72 

Machines within the network can only be accessed by the Jump Box.
- The Jump Box is the only machine with access to the ELK network. The ELK NSG only allows access from the Jump Box Ip: 10.0.0.4

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | Yes                 | 99.230.130.72        |
| ELK      | No                  | 10.0.0.4             |
| Web-1    | No                  | 10.0.0.4             |
| Web-2    | No                  | 10.0.0.4             | 

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- Automated configuration allows for installing containers, software, and configuring our ELK stack on multiple VM’s without having to manually apply these changes ourselves to each VM
- While manual configuration may not be an issue with a single, or even a few VM’s, it becomes a much more time consuming task when applied to organizations with dozens (or hundreds) of machines that require configuration.

The playbook implements the following tasks:
- Installs Docker.io, Python3-pip, and Docker Module
- Increase and use more virtual memory
- Download and launch Docker container using Docker image (sebp/elk:761)and ensures that it runs on ports: 9200, 5044, and 5601
- Finally, enables Docker service to start on boot.

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![Diagram](docker-ps.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- Jump Box: 10.0.0.4
- Web-1: 10.0.0.5
- Web-2: 10.0.0.6

We have installed the following Beats on these machines:
- FileBeat
- MetricBeat
 
These Beats allow us to collect the following information from each machine:

- Filebeat monitors log files on specific locations within our virtual network, and forwards that data to Elasticsearch which we can use to track incoming traffic on our network.

- Metricbeat collects metric data, such as uptime, from operating systems and services on our virtual network and forwards it to Elasticsearch. 

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the Filebeat configuration file to the Ansible container.
- Update the configuration file to include the IP of the ELK machine on lines #1106 and #1806 of the configuration file.
- In each playbook run, a host group must be specified. This will be either ELK or Webservers. This will allow us to run playbooks on a specified machine while avoiding others.
- Run the playbook, and navigate to http://[VM.IP]:5601/app/kibana to check that the installation worked as expected.



