## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

See images

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the YML file may be used to install only certain pieces of it, such as Filebeat.

⦁	ELK Install
⦁	Metricbeat playbook
⦁	Filebeat playbook


This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly avaiable, in addition to restricting access to the network.
What aspect of security do load balancers protect? What is the advantage of a jump box?Load balancers destribute network or application traffic across several different servers this protects against denial of service attacks.  It also helps with intrusion preventions of the applications because of the restricted access.  The jump box creates a more secure way to monitor the environment because the users is connecting from a single point.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the Data and system Logs.
-What does Filebeat watch for?Filebeat monitors the log files or locations that you specify, collects log events, and forwards them either to Elasticsearch or Logstash for indexing.
TODO: What does Metricbeat record?Metricbeat takes the metrics and statistics that it collects and ships them to the output that you specify, such as Elasticsearch or Logstash. Metricbeat helps you monitor your servers by collecting metrics from the system and services running on the server, such as: Apache.
The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name     | Function   | IP Address | Operating System |
|----------|------------|------------|------------------|
| Jump Box | Gateway    | 10.0.0.6   | Linux            |
| Web-1    | Server     | 10.0.0.4   | Linux            |
| Web-2    | Server     | 10.0.0.5   | Linux            |
| ElkVM    | Log Server | 10.1.0.4   | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the jumpbox machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
20.120.91.80

Machines within the network can only be accessed by Jumpbox.
Which machine did you allow to access your ELK VM? What was its IP address?
Jumpbox 10.0.0.6

A summary of the access policies in place can be found in the table below.

| Name          | Publicly Accessible | Allowed IP Address |
|---------------|---------------------|--------------------|
| Jump Box      | Yes                 | 20.120.91.80       |
| Load Balancer | Yes                 |                    |
| Web-1         | No                  |                    |
| Web-2         | No                  |                    |
| Elk           | Yes                 |                    |
### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
What is the main advantage of automating configuration with Ansible?
 It makes  the process automated.  If you needed to add more machines it is an easier process that you do not have to start from scratch and configure each machine individually.

The playbook implements the following tasks:
⦁	Install docker.io
⦁	Install PIP
⦁	Install docker python module
⦁	Download and Install a Docker elk container
⦁	run command to increase the memory
 
# Use apt module
    - name: Install docker.io
      apt:
        update_cache: yes
        force_apt_get: yes
        name: docker.io
        state: present

      # Use apt module
    - name: Install python3-pip
      apt:
        force_apt_get: yes
        name: python3-pip
        state: present

      # Use pip module (It will default to pip3)
    - name: Install Docker module
      pip:
        name: docker
        state: present

      # Use command module
    - name: Increase virtual memory
      command: sysctl -w vm.max_map_count=262144

      # Use sysctl module
    - name: Use more memory
      sysctl:
        name: vm.max_map_count
        value: "262144"
        state: present
        reload: yes

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

See Images

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
List the IP addresses of the machines you are monitoring
⦁	Web-1  10.0.0.4
⦁	Web-2  10.0.0.5

We have installed the following Beats on these machines:
Specify which Beats you successfully installed
⦁	FileBeat
⦁	MetricBeat

These Beats allow us to collect the following information from each machine:
In 1-2 sentences, explain what kind of data each beat collects, and provide 1 example of what you expect to see. E.g., `Winlogbeat` collects Windows logs, which we use to track user logon events, etc.
⦁	Filebeat monitors the log files or locations that you specify, collects log events, and forwards them either to Elasticsearch or Logstash for indexing.
⦁	Metricbeat helps you monitor your servers by collecting metrics from the system and services running on the server, such as: Apache.


### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 
 
SSH into the control node and follow the steps below:
- Copy the configuration file from Ansible container to /etc/ansible/roles.
- Update the /etc/ansible/hosts file to include to include webservers and ELKServer IP addresses.
- Run the playbook, and navigate to the Elk server's Kibana webpage using the load balancer's public IP address through the webservers  to check that the installation worked as expected.


- _Which file is the playbook? Where do you copy it?_Filebeat configuration, you copy it from /etc/ansible/files/filebeat-config.yml and copy it to etc/filebeat/filebeat.yml
- _Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on?_  You update the filebeat-config.yml and then change the hosts file with where you want it installed ElkServer of Filebeat.
- _Which URL do you navigate to in order to check that the ELK server is running?
http://[ElkVMIP}:5600/app/kibana

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._
