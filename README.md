
## PROJECT 1


## Automated ELK Stack Deployment


The files in this repository were used to configure the network depicted below.

![Cool Diagram](https://user-images.githubusercontent.com/86559022/143804332-99e30abe-56f0-4e3c-b8ca-dd01ee186f2b.PNG)


These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the _____ file may be used to install only certain pieces of it, such as Filebeat.

  - _TODO: Enter the playbook file._

- https://github.com/Byron-R-1234/Test/blob/main/pentest.txt
- https://github.com/Byron-R-1234/Test/blob/main/Host.txt
- https://github.com/Byron-R-1234/Test/blob/main/ansible.cfg.txt
- https://github.com/Byron-R-1234/Test/blob/main/elk.txt
- https://github.com/Byron-R-1234/Test/blob/main/filebeat_config.txt
- https://github.com/Byron-R-1234/Test/blob/main/filebeat_playbook.txt
- https://github.com/Byron-R-1234/Test/blob/main/metricbeat-config.txt
- https://github.com/Byron-R-1234/Test/blob/main/filebeat_playbook.txt

This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly "functional and available" , in addition to restricting "traffic" to the network.
- What aspect of security do load balancers protect? 

Load balancers reroute live traffic from one server to another if a server is under a DDoS attack or it becomes unavailable

- What is the advantage of a jump box?

Jump Box Provisioners prevents Azure Virtual Machines from being exposed via a public IP Address. This allows us to monitor and logging only using a single box. We can also determine which ip addresses are allowed to connect to it. 

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the "network" and system "logs".

- What does Filebeat watch for?

Filebeat monitors specific log files or locations, collects log events, and sends them to Elasticsearch or Logstash for indexing.

- What does Metricbeat record?

Metricbeat uses metrics and statistics that it collects and sends them to specific outputs, such as Elasticsearch or Logstash



The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name              | Function        | IP Address               | Operating System   |
|-------------------|-----------------|--------------------------|--------------------|
| Jump Box          | Gateway         | 20.191.231.182           | Linux              |
| Web-1             | UbuntuServer    | 10.0.0.8                 | Linux              |
| Web-2             | UbuntuServer    | 10.0.0.9                 | Linux              |
| ELKserver         | UbuntuServer    | 52.190.193.242/10.2.0.4  | Linux              |


### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the "Jump-Box-Provisioner" machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:

- Add whitelisted IP addresses_

Machines within the network can only be accessed by "Workstations and Jump-Box-Provisioner via SSH".

- Which machine did you allow to access your ELK VM? 

Jump-Box-Provisioner IP (20.191.231.182) : 10.2.0.4 via SSH port 22

- What was its IP address?

my public IP via port TCP 5601

A summary of the access policies in place can be found in the table below.

|        Name        |  Publicly Accessible  |          Allowed IP Addresses             |
|--------------------|-----------------------|-------------------------------------------|
| Jump Box           | Yes                   | 20.191.231.182 (Workstation IP on SSH 22) |
| Web-1              | No                    | 10.0.0.8 on SSH 22                        |
| Web-2              | No                    | 10.0.0.9 on SSH 22                        |
| ELKserver          | No                    | Workstation MY Public IP using TCP 5601   |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- _TODO: What is the main advantage of automating configuration with Ansible?_

The playbook implements the following tasks:
- In 3-5 bullets, explain the steps of the ELK installation play. E.g., install Docker; download image; etc._

- Specify a diferent group of machines
      ```yaml
        - name: Config elk VM with Docker
          hosts: elk
          become: true
          tasks:
      ```
  - Install Docker.io
      ```yaml
        - name: Install docker.io
          apt:
            update_cache: yes
            force_apt_get: yes
            name: docker.io
            state: present
      ``` 
  - Install Python-pip
      ```yaml
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
            `docker`, which is the Docker Python pip module.
      ``` 
  - Increase Virtual Memory
      ```yaml
       - name: Use more memory
         sysctl:
           name: vm.max_map_count
           value: '262144'
           state: present
           reload: yes
      ```
  - Download and Launch ELK Docker Container (image sebp/elk)
      ```yaml
       - name: Download and launch a docker elk container
         docker_container:
           name: elk
           image: sebp/elk:761
           state: started
           restart_policy: always
      ```
  - Published ports 5044, 5601 and 9200 were made available
      ```yaml
           published_ports:
             -  5601:5601
             -  9200:9200
             -  5044:5044   
      ```
      
   The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.
   
ELKserver
---------
![ELK SERVER](https://user-images.githubusercontent.com/86559022/143826412-94343ef6-3bf0-49cf-9015-7471fdc26bee.PNG)

Jump-Box-Provisioner
--------------------

![JUMP BOX PROVISIONER 111111111111111](https://user-images.githubusercontent.com/86559022/143827778-7770e7db-5a4f-4b43-9ebd-583a00a0ee30.PNG)


Web-1
-----

![web 1 screenshot](https://user-images.githubusercontent.com/86559022/143826845-dedc4e3d-e242-4c94-a2ec-5f1fae777339.PNG)


Web-2
-----

![web-2 screeeeeenshot](https://user-images.githubusercontent.com/86559022/143827102-14981b00-d275-49c1-a81c-a16de0260e82.PNG)


![TODO: Update the path with the name of your screenshot of docker ps output](Images/docker_ps_output.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- List the IP addresses of the machines you are monitoring_

- Web-1: 10.0.0.8
- Web-2: 10.0.0.9

We have installed the following Beats on these machines:

- Filebeat 
Module status: Data succesfully recieved 


- Metricbeat 
Module status: Data succesfully recieved 


These Beats allow us to collect the following information from each machine:

- Filebeat will be used to collect log files from very specific files such as Apache, Microsft Azure tools and web servers, MySQL databases.

- Metericbeat will be used to monitor VM stats, per CPU core stats, per filesystem stats, memory stats and network stats.

Docker container 
- cpu: 31.3% 
- memory: 9.1%
- disk: 88.95

Web-1 
- cpu: 1.1%
- memory 33.9%

Web-2: 
- cpu: 1.2%
- memory: 34.5%





### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

- Public IP address verification: https://www.whatismyip.com/
- update the Security Rules that uses the My Public IP address if changed 



SSH into the control node and follow the steps below:
- Copy the "yml" file to "ansible folder".
- Update the "config" file to include "remote users and ports".
- Run the playbook, and navigate to Kibana 52.190.193.242:5601 to check that the installation worked as expected.

### For ELK VM Configuration
-Run the playbook using this command: ansible-playbook /etc/ansible/install-elk.yml


## Filebeat 

- Download the filebeat playbook using this command `curl -L -O https://gist.githubusercontent.com/slape/5cc350109583af6cbe577bbcc0710c93/raw/eca603b72586fbe148c11f9c87bf96a63cb25760/Filebeat > /etc/ansible/filebeat-config.yml`

- Copy the Filebeat config file to /etc/ansible

- update the filebeat-config.yml file to include the Elk IP 
`nano /etc/ansible/filebeat-config.yml`

```bash
output.elasticsearch:
  # Boolean flag to enable or disable the output module.
  #enabled: true

  # Array of hosts to connect to.
  # Scheme and port can be left out and will be set to the default (http and 9200)
  # In case you specify and additional path, the scheme is required: http://localhost:9200/path
  # IPv6 addresses should always be defined as: https://[2001:db8::1]:9200
  hosts: ["10.2.0.4:9200"]
  username: "elastic"
  password: "changeme" # TODO: Change this to the password you set

# Starting with Beats version 6.0.0, the dashboards are loaded via the Kibana API.
# This requires a Kibana endpoint configuration.
setup.kibana:
  host: "10.2.0.4:5601" 
# TODO: Change this to the IP address of your ELK server
```

- Run the playbook with the command: `ansible-playbook filebeat-playbook.yml`

- go to the kibana website and check module 5 status


## Metricbeat 

- Download Metricbeat playbook using this command:
  - `curl -L -O https://gist.githubusercontent.com/slape/58541585cc1886d2e26cd8be557ce04c/raw/0ce2c7e744c54513616966affb5e9d96f5e12f73/metricbeat > /etc/ansible/files/metricbeat-config.yml`

copy the Metricbeat Config file to the /etc/ansible 

update the Metricbeat-config.yml file to include the Elk IP 
`nano /etc/ansible/metricbeat-config.yml`

```bash
#============================== Kibana =====================================

# Starting with Beats version 6.0.0, the dashboards are loaded via the Kibana API.
# This requires a Kibana endpoint configuration.
setup.kibana:
  host: "10.2.0.4:5601"
  
#-------------------------- Elasticsearch output ------------------------------
output.elasticsearch:
  # TODO: Change the hosts IP address to the IP address of your ELK server
  # TODO: Change password from `changem` to the password you created
  hosts: ["10.2.0.4:9200"]
  username: "elastic"
  password: "changeme"

```

- Run the playbook using this command `ansible-playbook metricbeat-playbook.yml`

- go to the kibana website and check module 5 status

## Install Filebeat onto VM's
1. Login to Kibana > Logs : Add log data > System logs > DEB > Getting started
2. Copy: curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.6.1-amd64.deb  
   (Download the Filebeat to the VM) 

## Install Metricbeat onto VM's
1. Login to Kibana > Add Metric Data > Docker Metrics > DEB > Getting Started
2. Copy: curl -L -O https://artifacts.elastic.co/downloads/beats/metricbeat/metricbeat-7.6.1-amd64.deb  
   (Download the Metricbeat to the VM) 
   
   
 Answer the following questions to fill in the blanks:_
- _Which file is the playbook? Where do you copy it?_
- _Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on?_
- _Which URL do you navigate to in order to check that the ELK server is running?

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._

|            COMMAND                                | PURPOSE                                               |
|---------------------------------------------------|-------------------------------------------------------|                         
|`ssh-keygen`                                       |  create a ssh key for setup VM's                      |
|`sudo cat .ssh/id_rsa.pub`                         |  to view the ssh public key                           |
|`ssh sysadmin@Jump-Box-Provisioner IP address`     |  to log into the Jump-Box-Provisioner                 |
| `sudo docker container list -a`                   | list all docker containers                            |
| `sudo docker start competent_dubinsky`            | start docker container competent_dubinsky             |
|`sudo docker ps -a`                                |  list all active/inactive containers                  |
|`sudo docker attach competent_dubinsky`            |  effectively sshing into competent_dubinsky           |
|`cd /etc/ansible`                                  | Change directory to the Ansible directory             |
|`ls -laA`                                          | List all file in directory (including hidden)         |
|`nano /etc/ansible/hosts`                          |  to edit the hosts file                               |
|`nano /etc/ansible/ansible.cfg`                    |  to edit the ansible.cfg file                         |
|`nano /etc/ansible/pentest.yml`                    |  to edit the My-Playbook                              |
|`ansible-playbook [location][filename]`            |  to run the playbook                                  |
|`sudo lsof /var/lib/dpkg/lock-frontend`            | unlocking a locked file                               |
|`ssh sysadmin@Web-1 IP address`                    |  to log into the Web-1 VM                             |
|`ssh sysadmin@Web-2 IP address`                    |  to log into the Web-2 VM                             |
|`ssh sysadmin@ELKserver IP address`                |  to log into the ELKserver VM                         |
|`exit`                                             | to exit out of docker containers/Jump-Box-Provisioners|
|`nano /etc/ansible/ansible.cfg`                    |  to edit the ansible.cfg file                         |
|`nano /etc/ansible/hosts`                          |  to edit the hosts file                               |
|`nano /etc/ansible/pentest.yml`                    |  to edit the My-Playbook                              |
|`ansible-playbook [location][filename]`            |  to run the playbook                                  |
|`sudo apt-get update` 				                      |  this will update all packages                        |
|`sudo apt install docker.io`				                |  install docker application		                        |
|`sudo service docker start`				                |  start the docker application                         |
|`sudo systemctl status docker`				              |  status of the docker application                     |
|`sudo systemctl start docker`                      |  start the docker service                             |
|`sudo docker pull cyberxsecurity/ansible`	        |  pull the docker container file                       |
|`sudo docker run -ti cyberxsecurity/ansible bash`  |  run and create a docker container image              |
|`ansible -m ping all`                              |  check the connection of ansible containers           |
|`curl -L -O [location of the file on the web]`     |  to download a file from the web                      |
|`dpkg -i [filename]`                               |  to install the file i.e. (filebeat & metricbeat)     |
|`http://52.190.193.242:5601//app/kibana`           | Open web browser and navigate to Kibana Logs          |
|`nano filebeat-config.yml`                         | create and edit filebeat config file                  |
|`nano filebeat-playbook.yml`                       | write YAML file to install filebeat on webservers     |
|`nano metricbeat-config.yml`                       | create metricbeat config file and edit it             |
|`nano metricbeat-playbook.yml`                     | write YAML file to install metricbeat on webservers   |  
------------------------------------------------------------------------------------------------------------

