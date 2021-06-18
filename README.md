# Elk-Stack-Project
Live ELK Deployment on Azure

The files in this repository were used to configure the network.



These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment. Alternatively, select portions of the PLAYBOOK file may be used to install only certain pieces of it, such as Filebeat.

  - filebeat-playbook.yml
    /etc/ansible/roles/filebeat-playbook.yml

  ---
- name: Installing and Launch Filebeat
  hosts: webservers
  become: yes
  tasks:
    # Use command module
  - name: Download filebeat .deb file
    command: curl -L -O  https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.4.0-amd64.deb

    # Use command module
  - name: Install filebeat .deb
    command: dpkg -i filebeat-7.4.0-amd64.deb

    # Use copy module
  - name: Drop in filebeat.yml
    copy:
      src: /etc/ansible/files/filebeat-config.yml
      dest: /etc/filebeat/filebeat.yml

    # Use command module
  - name: Enable and Configure System Module
    command: filebeat modules enable system

    # Use command module
  - name: Setup filebeat
    command: filebeat setup

    # Use command module
  - name: Start filebeat service
    command: service filebeat start

    # Use systemd module
  - name: Enable service filebeat on boot
      systemd:
      name: filebeat
      enabled: yes



### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly ACCESSIBLE, in addition to restricting TRAFFIC to the network.

LOAD BALANCERS OFFERS PROTECTION FROM CERTAIN CYBER ATTACKS, MAINLY DDOS ATTACKS BY SHIFTING AND ALLOCATING TRAFFIC. 
A MAJOR ADVANTAGE OF HAVING A JUMPBOX IS THAT IT LIMITS ACCESS TO THE VNET TO JUST ONE GATEWAY. 

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the APPLICATIONS and system LOGS.
FILEBEAT MONITORS THE FILE SYSTEM FOR ANY CHANGES AND DOCUMENTS WHEN THEY ARE MADE.
METRICBEAT TAKES METRICS AND STATISTICS FROM THE SERVERS IT IS SET TO MONITOR.

The configuration details of each machine may be found below.

| NAME    | FUNCTION   | IP ADDRESS | OPERATING SYSTEM |
-----------------------------------------------------------------------
| JUMPBOX | GATEWAY    | 10.0.0.1   | LINUX            |
| TODO    | WEB1       | 10.0.0.5   | LINUX            |
| TODO    | WEB2     	 | 10.0.0.6   | LINUX            |
| TODO    | ELK-SERVER | 10.1.0.4   | LINUX            |



### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the JUMPBOX machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
MY HOME PUBLIC IP ADDRESS ---> 45.19.105.74

Machines within the network can only be accessed by JUMPBOX.
JUMPBOX IP 10.0.0.4

A summary of the access policies in place can be found in the table below.

| NAME       | PUBLICLY ACCESSIBLE | ALLOWED IP ADDRESSES |
---------------------------------------------------------------------------------------------
| JUMPBOX    | YES                 | 45.19.105.74         
| WEB1       | NO                  | 10.0.0.4             
| WEB2       | NO                  | 10.0.0.4             
| ELK-SERVER | NO                  | 10.1.0.4             




### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
  THE MAIN ADVANTAGE OF USING ANSIBLE IS THAT IT ALLOWS YOU TO PERFORM VARIES ACTIONS ON MULTIPLE SERVERS AT A SINGLE TIME

The playbook implements the following tasks:

- * CREATE NEW VM IN NEW REGION, BUT WITHIN SAME RESOURCE GROUP.
- * CREATE PEER TO PEER CONNECTION WITH DIFFERENT VNETS.
- * ADD NEW VM TO HOSTS FILE, AND CREATE ANSIBLE PLAYBOOK
- * RUN PLAYBOOK & LAUNCH CONTAINER
- * ADD SECURITY RULE TO ALLOW ELK TO ACCESS VNET





### Target Machines & Beats
This ELK server is configured to monitor the following machines:
WEB1 IP 10.0.0.5 & WEB2 10.0.0.6

We have installed the following Beats on these machines:
FILEBEAT (METRICBEAT WAS INSTALLED AND IS RUNNING BUT IS STILL NOT GATHERING DATA)

These Beats allow us to collect the following information from each machine:
FILEBEAT DOCUMENTS WHEN CHANGES ARE MADE ON EITHER WEB1 OR WEB2


### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the ANSIBLE HOSTS FILE file to /ETC/ANSIBLE.
- Update the HOSTS file to include...INTERNAL IP ADDRESS OF THE WEB SERVERS & ELK SERVERS, AS WELL AS THE REMOTE USER INFORMATION
- Run the playbook, and navigate to KIBANA to check that the installation worked as expected.





