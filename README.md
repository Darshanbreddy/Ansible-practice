# Ansible EC2 Automation Project 🚀

This project uses Ansible to automate tasks on AWS EC2 instances. A single Ansible **master** EC2 instance manages three **target** EC2 servers via SSH using a shared key pair.

---

## 🏗️ Architecture

- **Ansible Master (EC2)**: Installs and runs Ansible
- **3 EC2 Target Servers**: Configured and controlled via SSH
- **Shared SSH Key Pair**: `ansible-master-key-demo.pem`
- **Ubuntu OS**: All instances run Ubuntu

---

## ✅ Prerequisites

- AWS EC2 instances:
  - 1 Master
  - 3 Targets
- Ubuntu installed on all
- SSH key pair (`.pem` file)
- Internet access for package installs

---

## ⚙️ Setup & Configuration

### 1. Install Ansible on the Master

bash
sudo apt-add-repository ppa:ansible/ansible
sudo apt update
sudo apt install ansible -y
From your local machine, run:

bash
scp -i "ansible-master-key-demo.pem" \
  ansible-master-key-demo.pem \
  ubuntu@<MASTER_PUBLIC_DNS>:/home/ubuntu/keys/
Edit /etc/ansible/hosts on the master and add:
[servers]
server_1 ansible_host=18.206.22.91
server_2 ansible_host=19.198.21.87
server_3 ansible_host=100.26.29.41

[servers:vars]
ansible_user=ubuntu
ansible_ssh_private_key_file=/home/ubuntu/keys/ansible-master-key-demo.pem
ansible_python_interpreter=/usr/bin/python3


Test Connectivity & Fetch System Info
ansible servers -m ping
ansible servers -a "free -h"


All YAML playbooks are in the root directory for easy use
