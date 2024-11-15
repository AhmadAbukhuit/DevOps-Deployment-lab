# Devops-Deployment-lab
This lab to deploy multi servers using Vagrant and Manage them using Ansible

# Create Vagrant File
First prepare vagrant file to start the servers :
    - ansible-control
    - Database Server (db01)
    - Two Web Server (web01,web02)
    - Loadbalancer Server (loadbalancer) 

# Set Up Ansible-Control Server
    - Set up ansible 
        $ sudo apt install ansible 
    - creat inventory file named hosts 
        grouped servers to 4 group [control,proxy,webservers,database]
    - Set up ssh 
        $ ssh-keygen
        $ ssh-copy-id localhost
        $ ssh-copy-id web01 && ssh-copy-id web02 && ssh-copy-id loadbalancer && ssh-copy-id db01
    - Set up python-simplejson module to give ansible full functionalty 
        $ ansible all -i hosts -m command -a 'sudo apt-get -y install python-simplejson'

# Set up Services using apt module 
    - update cache 
        $ ansible all -i hosts --become -m apt -a "update_cache=yes"
    - Set up web servers using apache
        $ ansible webservers -i hosts --become -m apt -a "name=apache2 state=present"
    - Set up database server using mysql 
        $ ansible database -i hosts --become -m apt -a "name=mysql-server state=present"
    - Start services we installed 
        $ ansible database -i hosts --became -m service -a "name=mysql state=started"

# Playbook in YML
    - Write a basic playbook1.yml to check that the webservers have apache2 latest version 
    - Configure the posrt.conf file in apache2 using jenga 
    - check the configuration using curl command 

# Create roles for multi services 
    - Create role for apache2
        $ ansible-galaxy init roles/apache2
    - Write the playbook2.yml for this role