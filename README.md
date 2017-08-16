# CC_Project
Project Test

This Read me Document will contain steps to setup a static web application

This below setup is performed on Amazon EC2 Instance 

Ubuntu -> Ansible Controller
Amazon Linux -> Node

Change ip in hosts file to test this playbook

On AWS, you will have to setup a load balancer and place nodes in the load balancer. 

Refer : http://docs.aws.amazon.com/elasticloadbalancing/latest/classic/elb-create-https-ssl-load-balancer.html

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
 - Install Nginx
 - Modify nginx.conf file via templates
 - Remove default sites
 - create default directories
 - copy index.html.j2
 - generate ssl cerificate

6. Creating handlers

 - restart nginx using notify

7. Creating templates

 - modified nginx.conf.j2 goes here 
 - routing https traffic via port 443 enabled

8. Creating Playbook
 - Run Role

