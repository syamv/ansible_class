# ansible_class


Installation::
yum install https://releases.ansible.com/ansible/rpm/release/epel-7-x86_64/ansible-2.4.2.0-1.el7.ans.noarch.rpm
ansible --version

--------------------
https://www.techinformant.in/devops-ansible-quick-introduction-static-dynamic-inventories/
https://devopsmates.com/ansible-installation-and-configuration-on-redhatcentos-7/
https://www.techinformant.in/epel-repository/
https://aws.amazon.com/blogs/apn/getting-started-with-ansible-and-dynamic-amazon-ec2-inventory-management/
https://developer.github.com/v3/guides/using-ssh-agent-forwarding/
https://www.youtube.com/watch?v=Hr8I7pxtG9U&t=381s
http://www.tothenew.com/blog/launching-and-configuring-an-aws-ec2-instance-using-ansible/
https://github.com/ricardofontanelli/ansible-aws-ec2-provision

--------------------------------------------
sudo su
yum install ansible
--------------
$ sudo apt-get update
$ sudo apt-get install software-properties-common
$ sudo apt-add-repository ppa:ansible/ansible
$ sudo apt-get update
$ sudo apt-get install ansible
-----------------------
=====================

Server_Name	IP_Add			SSH
webserver	192.168.33.101		Available
appsever	192.168.33.102		Available
dbserver	192.168.33.103		UnAvailable


Ansible Server Side
1) create user: adduser -d /home/ansible -m ansible
2) create password: passwd ansible
3) switch user to ansible: su - ansible
4) create ssh key for ansible user: ssh-keygen -t rsa
5) add the clien mechine ipaddress or hostname in the /etc/hosts & /etc/hostname files to access via ssh connection on ansible server.
   vi /etc/hosts
	   192.168.33.101 webserver
	   192.168.33.102 appsever
	   192.168.33.103 dbserver
	   
   vi /etc/hostname
	   192.168.33.101 webserver
	   192.168.33.102 appsever
	   192.168.33.103 dbserver
   
5) Before going to 6th step: perform the above 1, 2, 3 steps in the client mechine.
6) once above mentione steps are finished : get ready to copy the "public key ( id_rsa.pub) " of ansible user to client mechine using the below command.
cat ~/.ssh/id_rsa.pub | ssh user@ipaddress "mkdir -p ~/.ssh && cat >>  ~/.ssh/authorized_keys"

7) Client mechine Side
login to clien mechine and follow the steps 
complete the 1,2,3 steps of server side 
	go to : cd /home/ansible
	chmod 700 .ssh
	chown ansible:ansible .ssh
	cd .ssh
	chmod 600 authorized_keys
	chown ansible:ansible
	
8) Now go to Server mechine, try to login via ssh
ssh username@ipaddress (or) ssh hostname
9) All the asnible user in "sudo /etc/sudoers file to get the root access from play books 

chmod +w /etc/sudoers && echo "ansible ALL=(ALL)       NOPASSWD: ALL" >> /etc/sudoers && chmod -w /etc/sudoers

=============================
disable IP tables
ipdables -F
service iptables stop
chkconfig ipdables off
Disable selinux 
vi /etc/sysconfig/selinux
=============================
/etc/yum.repos.d
yum install epel-release
ansible-doc -l 
ansible-doc module_name
===================
Q) How to run the playbook specific to one host?
	ansible-playbook playbook_name --limit host_name
Q) How to run the playbook on failed servers?
	ansible-playbook playbook.yml --limit @/folder_name/playbook.retry
Q) How to run the playboook in localhost
	ansible-playbook -i "localhost," -c local varable.yml

=================================================================
https://www.tecmint.com/how-to-enable-epel-repository-for-rhel-centos-6-5/


yum --enablerepo=epel info ansible
yum install ansible --skip-broken
================================================================
Ansble Ad-Hoc commands are used for ondemand.
Any playbook have four componetns
1) Setup
---
key: value
- hosts: Server_Name
  remote_user: user_name ( user has to be a root permissions so we need to add the user in /etc/sudoers file )
  become: yes
  
2) Varaiables
	a) vars
	vars:
		 key: value
Example: 
	vars:
		 pkg: git
	tasks:
	  - name: Install git
		yum: name={{pkg}} state=installed
		 
		 
	b) vars_files
Create file with varsfile.yml and define the variables 
--------------Ex: varsfile.yml
---
pkg: httpd
:wq!
--------------playbook.yml
---
  vars_files:
		- varsfile.yml
	tasks:
		  - name: Install git
			yum: name={{pkg}} state=installed


	c) vars_prompt
	d) --extra-vars
3) tasks
4) Handlers

==========================

/etc/ansible/host
private_key_path=pem
host_key_checking=false
remote_user=ansible

/etc/ansible/ec2.py
==========================
ignore_errors: true ===== is to skip the failed tasks
ansible host_name -m setup | more ==== to gather all the facts in the ansible. (All varibales)
tags==== are only to run the particular tasks using the --tags from command line which was defined in playbook withe define tag name.
ansible-playbook tags.yml --tags "install"
ansible-playbook tags.yml --tags "copy"
ansible-playbook tags.yml --skip-tags "install" to skip the tag and run the rest of play.
ansible-playbook tags.yml --tags "install,copy"
tags:
   - always ==== means this task having tag as always it runs always
 ============ start-at-task "namme of the task"
 
 ------------------:   
 --syntax-check == is used to check the sytax of playbook
 setup == is used to get the facts like, Network and Platform information of the all hosts
 with_items == is used for loop conditions, doing same activity for multiple times like for loop
 ----------------------
 when == is connection base module like if......
 when: ansible_os_family == 'Redhat'
-----------------------
templete === module is used to substitute the varibales that are their in the source file.
copy == only used to copy
command == used to work with unix commands but not SHELL environment or variables.
script == this module is used to copy the script and exeutes on remote server
raw === N/W devices, routers devices use this module [ raw: copy module also called ]

***NOTE: command, script, raw modules are not idempotency

=======================================================================================================================

static inventory :::::::::::: details of remote systems information ::: ip/hostname (/etc/ansible/hosts )
/etc/ansible/hosts

ansible.cfg --- 1. /etc/ansible/hosts
hosts  2.look for hosts/remote info  ----  ip/hostname
roles

ssh key
public -- remote


Groups
hosts/id
On Primisis/ own data center/ VM, Vagrant.....

Ansible Control Mechine
Ansible :::::::: Dynamic inventory ::::: AWS 
-----------------
ansible 2.4
aws cli
python
boto3

aws configure
access key
security key
region
file format


/etc/ansible/  ---  
ansible.cfg  ---- 1./etc/ansible/ec2.py  (dynamic inventory file)
ec2.init --- 3.initilazation 
ec2.py  --- 2.dynamic inventory file
roles

Connection

Ansible  to AWS  ::::::  password less

AWS --- user --- Access Key, Security KEy, Region, AWS.pem ( public key---- ssh-keygen -t rsa)
aws cli --- AWS Command Line interface

ansible.cfg
inventory = /etc/ansible/ec2.py
private_key_file = /home/ansible/dynamic_inventory_aws/ansible_class/AwsKey_USNV.pem

