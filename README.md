# ansible_class


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

