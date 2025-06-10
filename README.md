Ansible EC2 Automation Project
Overview
This project demonstrates how to use Ansible to automate infrastructure tasks on AWS EC2 instances. One EC2 instance is set up as the Ansible master, managing three other EC2 servers (targets). Tasks such as connectivity checks, system information gathering, and Nginx installation are performed using Ansible ad-hoc commands and playbooks.

Architecture
1 Master EC2 Instance: Runs Ansible and controls the other servers.

3 Target EC2 Servers: Managed by the master via SSH using a shared key pair.

All instances use the same key pair for authentication and run Ubuntu.

Setup Steps
1. Install Ansible on the Master Server
bash
Copy
Edit
sudo apt-add-repository ppa:ansible/ansible
sudo apt update
sudo apt install ansible
2. Configure Ansible Inventory
Edit the inventory file:

bash
Copy
Edit
sudo vim /etc/ansible/hosts
Add a group and define your servers using their public IP addresses:

ini
Copy
Edit
[servers]
server_1 ansible_host=18.206.22.91
server_2 ansible_host=19.198.21.87
server_3 ansible_host=100.26.29.41

[servers:vars]
ansible_ssh_private_key_file=/home/ubuntu/keys/ansible-master-key-demo.pem
ansible_python_interpreter=/usr/bin/python3
ansible_user=ubuntu
3. Transfer SSH Key to Master
Copy the key from your local machine to the master EC2 instance:

bash
Copy
Edit
scp -i "ansible-master-key-demo.pem" ansible-master-key-demo.pem ubuntu@<MASTER_PUBLIC_DNS>:/home/ubuntu/keys
This allows the master server to SSH into the other servers.

4. Connectivity Check
Test the connection to all servers:

bash
Copy
Edit
ansible servers -m ping
Check RAM on all servers:

bash
Copy
Edit
ansible servers -a "free -h"
Playbooks
1. Install Nginx
Create a playbook file named nginx-playbook.yml:

yaml
Copy
Edit
---
- name: nginx-install and run
  hosts: servers
  become: yes

  tasks:
    - name: Install Nginx
      apt:
        name: nginx
        state: latest
Run the playbook:

bash
Copy
Edit
ansible-playbook nginx-playbook.yml
2. Date Check (Additional Task)
A sample playbook to fetch date info (in date-playbook.yml):

yaml
Copy
Edit
---
- name: Check current date on servers
  hosts: servers
  become: no

  tasks:
    - name: Get current date
      command: date
Run it using:

bash
Copy
Edit
ansible-playbook date-playbook.yml
Summary
In this project:

You set up Ansible on a master EC2 instance.

Configured the inventory to connect securely to target servers using SSH.

Verified connectivity and performed system checks with ad-hoc Ansible commands.

Installed and managed Nginx using an Ansible playbook.

Created additional playbooks such as checking the current date.

Files Included
nginx-playbook.yml

date-playbook.yml

README.md
