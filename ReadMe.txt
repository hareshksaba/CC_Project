This Read me Document will contain steps to setup a static web application

This below setup is performed on Amazon EC2 Instance 

Ubuntu -> Ansible Controller
Amazon Linux -> Node

On AWS, you will have to setup a load balancer and place nodes in the load balancer. 

Health check can be setup from the AWS UI Console to periodically check for the index.html file in the node(nginx server) and a threshold of 3 or 5 failures based on your setup




1. Setup Ansible controller

-- Install Ansible

sudo apt-get install software-properties-common
sudo apt-add-repository ppa:ansible/ansible
sudo apt-get update
sudo apt-get install ansible

-- Generate rsa key

2. Setup Node

-- Make sure python is installed.
-- Copy is_rsa.pub file from controller to node /.ssh/authorized_key

3. Test Ansible Setup.

-- Create a project folder(Comcast_Project)
-- Create an inventory file (hosts)
-- example(web1 ansible_ssh_host=your_ip_goes_here)

-- Test if setup is correct by running below command
	ansible all -i hosts -m ping
	This should result in "ping":"pong"

4. Create Folder Structure for Roles / Playbook on Ansible controller.

-- Create a folder nginx
-- inside nginx create 6 subfolders
	-files
	-handlers
	-meta
	-templates
	-tasks
	-vars


5. Creating tasks

-- under roles/nginx/tasks 

Create a main.yaml file

Paste the following code

---

- hosts: webservers
  
  vars: 
   - MyMessage:  "Haresh Comcast Project"
  
  tasks:
   - name: Nginx Webserver setup
     yum: name=nginx state=installed update_cache=true
     notify:
      - start nginx
   

   - name: copy index.html
     template: src=index.html.j2 dest=/usr/share/nginx/html/index.html
     notify:
      - restart nginx
   
   - name: nginx listen on port 8081
     lineinfile: dest=/etc/nginx/nginx.conf regexp="^%80\default%" line="8081 default" state=present
     notify:
      - restart nginx

...

6. Creating handlers

Under roles/nginx/handlers

Paste the following in main.yml

---
- name: start ngnix
  service: name=nginx state=started

- name: restart nginx
  service: name=nginx state=restarted

7. Creating templates

Under roles/nginx/files

create a file index.html.j2 and plaste the following

<html>          
  <head>
    <title>Hello World</title>
  </head>
  <body>
    <h1>!Hello World!</h1>
  </body>
</html>


8. Creating Playbook

create playbook deploy-webserver.yaml in the Home folder of this project
---
- hosts: hosts
  roles:
    - role: nginx


