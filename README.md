# ansible

Assignment1: (use ansible playbooks)
1. Setup ansible cluster with 3 nodes
2. on slave1 install java
3. On slave2 install mysql

---
- name: Installing java on Slave1
  hosts: slave1
  become: true
  tasks:
    - name: install java on slave1
      apt: name=openjdk-17-jdk state=latest
- name: Installing mysql on slave2
  hosts: slave2
  become: true
  tasks:
    - name: install mysql on slave2
      apt: name=mysql-server state=latest
	  
	  
	  
Assignment2:
1. Create a script which can add text " this text has been added by custom script" to /tmp/1.txt
2. Run this script using ansible on all the hosts.

script.sh
echo "This text has been added by custom script" > /tmp/1.txt
---
- name: executing script file inside slave server
  hosts: slave1, slave2
  become: true
  tasks:
    - name: executing script file
      script: script.sh
	  
Assignment3:
1. Create 2 ansible roles
2. Install Apache2 on slave1 using one role and NGINX on slave2 using the other role
3. Above should be implemented using different ansible roles.


create a role inside /etc/ansible -- sudo ansible-galaxy init apache2-role
ls
cd apache-role
ls
cd tasks
ls
sudo nano install.yml
---
- name: Install Apache2
  apt:
    name: apache2
    state: latest
sudo nano main.yml
---
# tasks file for apache-role
- include_tasks: install.yml

cd /etc/ansible
sudo nano play3.yml
---
- name: executing role inside slave1
  hosts: slave1
  become: true
  roles:
    - apache-role

create a role inside /etc/ansible -- sudo ansible-galaxy init nginx-role
ls
cd nginx-role
ls
cd tasks
ls
sudo nano nginx.yml
---
- name: Install nginx
  apt:
    name: nginx
    state: latest
	
sudo nano main.yml
---
# tasks file for apache-role
- include_tasks: nginx.yml

cd /etc/ansible
sudo nano play3.yml
---
- name: executing role inside slave2
  hosts: slave2
  become: true
  roles:
    - nginx-role

Assignment4:
1.use the previous deployment of ansible cluster
2.Configure the files folder in the role with index.html which should be replaced with original index.html

All of the above should happen only on Slave which has nginx installed using the role.

cd nginx-role
cd files
sudo nano copy.yml
" hello world get lost"

cd ..
cd tasks
sudo nano copy.yml
---
- name: replacing nginx-dashboard
  copy: src=/etc/ansible/roles/nginx-role/files/index.html dest=/var/www/html/
 
sudo nano main.yml
---
# tasks file for nginx-role
- include_tasks: copy.yml

cd /etc/ansible

sudo nano play4.yml
---
- name: replacing index file on salve2
  hosts: slave2
  become: true
  roles:
    - nginx-role
	
	
Assigment5:
1. Create a new deployment of ansible cluster of 5 nodes
2. label 2 nodes as test and other as prod
3. Install Java on test nodes
4. Install mysql server on prod nodes

Use ansible roles for the above and group the hosts under test and prod

sudo ansible-galaxy init java
sudo ansible-galaxy init mysql

---
- name: install java on test nodes
  hosts: test
  become: true
  roles:
    - java
- name: install java on prod nodes
  hosts: prod
  become: true
  roles:
    - prod
