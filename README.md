# UCD_Project_1


The files in this repository were used to configure the network depicted below.

Images/Project_1.png

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. 
Alternatively, select portions of the YAML file may be used to install only certain pieces of it, such as Filebeat.

---
- name: Configure Elk VM with Docker
 hosts: elkserver
 remote_user: sysadmin
 become: true
 tasks:

   - name: Install docker.io
     apt:
       update_cache: yes
       name: docker.io
       state: present

   - name: Install pip3
     apt:
       force_apt_get: yes
       name: python3-pip
       state: present

   - name: Install Docker python module
     pip:
       name: docker
       state: present

   - name: Increase virtual memory
     command: sysctl -w vm.max_map_count=262144

   - name: Use more memory
     sysctl:
       name: vm.max_map_count
       value: "262144"
       state: present
       reload: yes

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


This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly optimized, in addition to restricting traffic to the network.
- Load Balancers protect the availability of data, as it allows the system to more easily distribute traffic and makes the system more resistent to DoS attacks.
- Jump Boxes reduce the attack surface because only one machine is available to the outside internet. One machine is much less likely to be an issue than 
- a larger amount of machines.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the Jumpbox and system network.
-# _TODO: What does Filebeat watch for?_ NA, we were not able to complete this part of the lab as it was not available
-# _TODO: What does Metricbeat record?_  NA, we were not able to complete this part of the lab as it was not available

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump Box | Gateway  | 10.0.0.4   | Linux            |
| Web-1    | Server   | 10.0.0.5   | Linux            |
| Web-2    | Server   | 10.0.0.6   | Linux            |
| ELK      | Listener | 10.1.0.4   | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the JumpBox machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- 73.66.81.140 #whitelisted IP

Machines within the network can only be accessed by Jumpbox.
- JumpBox VM. Internal IP = 10.0.0.4

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | Yes                 | 73.66.81.140         |
| Web-1    | No                  | 10.0.0.4             |
| Web-2    | No                  | 10.0.0.4             |
| ELK      | No                  | 10.0.0.4,73.66.81.140|

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- It is repeatable and easy to bug test. If another server needs the same setup, only a few variables need changing.
- If there are issues, only the automation file needs to be changed, not the entire system.

The playbook implements the following tasks:
- Install Docker
- Pull container
- Install container
- Configure container on host machine



### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- 10.0.0.5
- 10.0.0.6

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the playbook file to Ansible.
- Update the .yml file to include webservers and elk
- Run the playbook, and navigate to webservers and elk to check that the installation worked as expected.

- _Which file is the playbook? Where do you copy it?_
-   pentest.yml. Ansible container
- _Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on?_
-   pentest.yml
- _Which URL do you navigate to in order to check that the ELK server is running?
- http://<dynamic_elk_ip>:5601/app/kibana
