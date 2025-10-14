# Ansible

# 🧩 Ansible Short Notes for DevOps Engineers

A concise, practical, and beginner-friendly guide to **Ansible** — an open-source automation tool for **configuration management**, **application deployment**, and **IT orchestration**.  
These notes are perfect for anyone preparing for **DevOps interviews**, **hands-on labs**, or **real-world automation projects**.

---

## 🚀 Introduction to Ansible

**Ansible** is an agentless automation tool that uses **SSH** for communication with target systems.  
It follows a **declarative approach** — you define *what* you want, not *how* to do it.

### 🧠 Key Concepts

| Concept | Description |
|----------|-------------|
| **Inventory** | Lists target hosts where Ansible executes tasks. Can be static or dynamic. |
| **Playbook** | YAML file containing a series of tasks to be executed on target hosts. |
| **Task** | The smallest unit of action in a playbook (e.g., install package, start service). |
| **Module** | Pre-built units of code that perform specific actions (like `apt`, `service`, `copy`). |
| **Role** | A structured way to group tasks, handlers, files, and variables for reusability. |

---

## ⚙️ Installing Ansible (Ubuntu Example)

```bash
sudo apt-add-repository ppa:ansible/ansible
sudo apt update
sudo apt install ansible -y


## 🔑 Copying PEM Key from Local to EC2 Instance (Windows Example)

If you’re using **Windows PowerShell**, and your `.pem` key is located in your **Downloads** folder,  
use the following command to copy it securely to your EC2 instance:

```powershell
scp -i "C:\Users\<your-username>\Downloads\Ansible-master-key.pem" "C:\Users\<your-username>\Downloads\Ansible-master-key.pem" ubuntu@<EC2-PUBLIC-IP>:/home/ubuntu/
✅ Explanation:

-i → Path to your existing key (used for authentication).

The second path → Your .pem file to be copied.

The last path → Destination on your EC2 (/home/ubuntu/ is safe and writable).

Once copied, SSH into your EC2 instance and secure the key:

bash
Copy code
chmod 400 /home/ubuntu/Ansible-master-key.pem
💡 Tip: If you want to keep keys organized, create a folder on EC2:

bash
Copy code
mkdir -p /home/ubuntu/keys && mv /home/ubuntu/Ansible-master-key.pem /home/ubuntu/keys/
chmod 400 /home/ubuntu/keys/Ansible-master-key.pem
Configure Inventory File
bash
Copy code
sudo nano /etc/ansible/hosts
Example:

ini
Copy code
[servers]
server1 ansible_host=<Public-IP>
server2 ansible_host=<Public-IP>
server3 ansible_host=<Public-IP>

[all:vars]
ansible_python_interpreter=/usr/bin/python3
ansible_user=ubuntu
ansible_ssh_private_key_file=/<path-to-your-key>.pem
⚡ Ansible Ad-hoc Commands vs Modules
🔹 Ad-hoc Commands
Used for quick, one-time tasks.

bash
Copy code
ansible all -a "df -h" -u ubuntu
ansible servers -a "uptime" -u ubuntu
🔹 Modules
Reusable components to manage resources.

bash
Copy code
ansible all -m ping -u ubuntu
ansible all -m apt -a "name=nginx state=latest" -u ubuntu --become
📜 Ansible Playbooks
Playbooks define what needs to be done across multiple servers.

Example 1: Show Date
yaml
Copy code
- name: Date playbook
  hosts: servers
  tasks:
    - name: Display current date
      command: date
Example 2: Install NGINX
yaml
Copy code
- name: Install and start NGINX
  hosts: servers
  become: yes
  tasks:
    - name: Install NGINX
      apt:
        name: nginx
        state: latest
    - name: Start and enable NGINX
      service:
        name: nginx
        state: started
        enabled: yes
🧰 Playbook to Install Tools
yaml
Copy code
- name: Install Docker and AWS CLI
  hosts: servers
  become: yes
  tasks:
    - name: Install Docker
      apt:
        name: docker.io
        state: latest

    - name: Install AWS CLI (Ubuntu/Debian)
      apt:
        name: awscli
        state: latest
      when: ansible_distribution == 'Debian' or ansible_distribution == 'Ubuntu'
🧩 Conditional Playbooks
Ansible allows using conditions to execute tasks only when certain criteria are met.

Example:

yaml
Copy code
- name: Conditional task example
  hosts: servers
  tasks:
    - name: Run this only on Ubuntu
      apt:
        name: apache2
        state: latest
      when: ansible_distribution == "Ubuntu"
🙏 Thank You
Made with ❤️ for DevOps enthusiasts.
Train with passion. Automate with confidence. 🚀


Compiled by: Surendra Kamble
