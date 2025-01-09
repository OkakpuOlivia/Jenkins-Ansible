
Automating Jenkins Deployment Using Ansible

Overview

This outlines the process of automating Jenkins deployment on AWS EC2 instance with Ansible. By using Ansible playbooks, eliminate manual setup steps, ensuring a consistent and repeatable deployment. The playbook handles the installation of Jenkins and its dependencies. 

Prerequisites

1. AWS EC2 Instance
   
Launch an Ubuntu-based EC2 instance.
Configure a security group with the following inbound rules:
SSH (Port 22): For Ansible to execute commands remotely.
HTTP (Port 80): To access Jenkins through a web browser.
Custom TCP (Port 8080): If Jenkins is configured to use a custom port.

2. Ansible on Control Node
   
Install Ansible on local machine
bash
sudo apt update && sudo apt install -y ansible  
Confirm Ansible installation:
bash
ansible --version 

3. SSH Access to EC2

Use the private key file for SSH access to the EC2 instance.
Ensured secure permissions for the key file:
bash
chmod 400 key.pem  

4. Ansible Inventory File
Create an inventory file specifying the EC2 instance’s public IP address:
ini
[jenkins]  
<EC2_IP> ansible_user=ubuntu ansible_ssh_private_key_file=/path/to/key.pem  

5. Internet Access

Verify that the EC2 instance has outbound internet access to download required packages like Jenkins and Java.

6. Security Group Configuration

Open the necessary ports (80 or 8080) in the security group to enable Jenkins access.

7. Updated APT Package Manager  

Ensure the APT package manager is updated on the EC2 instance to avoid installation issues.

8. Access to Jenkins Repository

Use the following details to install Jenkins:
Repository Key: Jenkins Key
Repository URL: https://pkg.jenkins.io/debian-stable binary/

Deployment Steps

Step 1: Prepare the Playbook
Create a playbook (deploy-jenkins.yml) with tasks to:

Add the Jenkins repository key and URL.
Install Jenkins and its dependencies.
Start and enable the Jenkins service.

Step 2: Update the Inventory File
Add the EC2 instance details to the inventory file for Ansible.

Step 3: Test Connectivity
Verify that the Ansible control node can connect to the EC2 instance:

bash

ansible -i inventory all -m ping  
Step 4: Run the Playbook
Execute the playbook to deploy Jenkins:

bash

ansible-playbook -i inventory deploy-jenkins.yml  

Step 5: Access Jenkins
Once the playbook completes successfully:

Connect to the EC2 instance:
bash

ssh -i /path/to/key.pem ubuntu@<EC2_IP>  
Retrieve the Jenkins initial admin password:
bash

sudo cat /var/lib/jenkins/secrets/initialAdminPassword  
Access Jenkins via a web browser:
http://<EC2_IP>:8080  
Use the admin password to unlock Jenkins and complete the initial setup.







