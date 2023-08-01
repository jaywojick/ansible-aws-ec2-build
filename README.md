# Ansible Amazon AWS EC2 Provision:
Tutorial to integrate Ansible provisioning with Amazon EC2 platform, using an Ubuntu 14.04 LTS Desktop and creating an Ubuntu 14.04 LTS Server.

# Install:
- Install Ansible last version:
```sh
$ sudo apt-get update
$ sudo apt-get install software-properties-common
$ sudo apt-add-repository ppa:ansible/ansible
$ sudo apt-get update
$ sudo apt-get install ansible
```
- Install boto (python lib):
```sh
$ sudo apt-get install python-pip
$ pip install -U boto
```
- Clone the repository inside /etc/ansible. Warning: it will replace your ansible files;

- Create the ssh key (~/.ssh/id_ansible) and import it to EC2. Note the ssh key name, if you use a different name, change it inside /etc/ansible/hosts;
```sh
$ ssh-keygen -t rsa -f ~/.ssh/id_ansible
```

- Change the file /etc/ansible/ec2_creation/vars/all.yml according the information about your EC2 environment, for example:
```yml
---
ec2_config:
    region: us-east-1
    zone: us-east-1a
    #Change it according the name that you put in Amazon
    keypair: <your.pem> 
    instance_type: <your-instance-type>
    #It create a Ubuntu 14.04LTS
    image: ami-6989a659
    #Your subnet
    vpc_subnet_id: <your-subnet>
    #An security group name created at Amazon
    group: [lamp]
    assign_public_ip: true
    volume_size: 8
    instance_tag_name: TagNomeApp
    remote_user: centos #change if needed
    #The path to your local ssh file
    ssh_path: ~/.ssh/<your.pem>
```

- Export your Amazon credential to environment variables;
```sh
$ export AWS_ACCESS_KEY_ID=<YOURKEYID>
$ export AWS_SECRET_ACCESS_KEY=<YOUSECRETKEY>
/usr/local/bin/aws  sts get-caller-identity
or
export AWS_ACCESS_KEY_ID=<YOURKEYID> && export AWS_SECRET_ACCESS_KEY=<YOUSECRETKEY> && /usr/local/bin/aws  sts get-caller-identity
```
- Inside /etc/ansible/ec2_creation Run the command: 
```sh
$ ansible-playbook -i hosts playbook.yml --private-key=[your ssh key file] -vvvv
or
$ ansible-playbook -i hosts playbook.yml
```
