Ansible Playbook for Deploying Apache Docker Container

This Ansible playbook automates the deployment of an Apache web server as a Docker container on multiple Linux hosts. Below is a breakdown of the playbook:

ansible.cfg:

Contains Ansible configuration settings.
deprecation_warnings=False: Disables deprecation warnings.
docker_deploy.yml:

Defines a playbook named "Deploy Apache Docker Container using Ansible".
Targets hosts listed under the [linux] group in hosts.ini.
Tasks include:
Installing python3-pip package if the host is Ubuntu or Debian.
Installing the docker Python library.
Installing Docker (docker.io) on Debian-based systems.
Starting the Docker service on Debian-based systems.
Pulling the latest httpd Docker image.
Creating a directory for persistent data.
Copying an index.html file to the host.
Setting up a Docker network named apache_network with a specific subnet.
Running a Docker container named apache-httpd based on the httpd image, exposing port 80 and mounting the directory as the web server's document root.
hosts.ini:

Defines the hosts where playbook tasks will be executed.
Hosts are grouped under [linux].
Each host is associated with an SSH private key file and a common user (melvin).
ssh:

Contains SSH key files (ssh_key and ssh_key.pub) used for authentication with remote hosts.
Instructions:

Setup Control Machine:

Download and store all files in a folder named ansible_playbook.
Move this folder to the control machine.
Install Ansible:

Run sudo apt install ansible to install Ansible on the control machine.
Setup Target Machine:

Update and upgrade packages:
sql
Copy code
sudo apt update
sudo apt upgrade
SSH Key Setup:

Copy SSH key to target machine for passwordless login:
css
Copy code
ssh-copy-id -i /home/control_host_username/ansible_playbook/ssh/ssh_key target_username@10.10.10.10
Replace control_host_username and target_username with appropriate values.
SSH into Target Machine:

SSH into target machine:
css
Copy code
ssh target_username@10.10.10.10
Modify sudoers File:

Run sudo visudo and add the line:
sql
Copy code
target_username ALL=(ALL) NOPASSWD: ALL
Save and exit the file.
Repeat for All Target Hosts:

Repeat steps 3-6 for all target hosts.
Modify hosts.ini:

Update hosts.ini to configure target machines' IP and username.
Modify docker_deploy.yml:

Update the path and username in the Copy index.html to host directory task.
Execute Playbook:

Navigate to the ansible_playbook folder and run:
css
Copy code
ansible-playbook -i hosts.ini docker_deploy.yml
Access HTML File:

Open http://172.168.10.1 to view the HTML file served by Apache Docker container.
Note: Ensure proper permissions and file paths are used throughout the setup process.
