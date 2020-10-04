# Ansible
Personal projects with ansible.

This is tested on Mac book as a controller and Ubuntu 18 on aws as a node

## Quick introduction to ansible
* It will be faster and easier to understand if ever worked with bash script which executes remotely.
* Works over ssh
* Dependent on python
* Reads JSON or Yaml files for the configuration
* Execution process involves-
    * Creating a python script that will have the configuraiton in YAML format
    * Sent that script via ssh to the manage nodes
    * Execute the remote python script.
    * Remove the temporarily created configuration scripts.

## Install ansible on Mac
    $ brew install ansible  # Ref https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#installing-ansible-on-macos

## Configure the Nodes to be managed. I would recommend to use the .ssh/config
    Sample from my configuration
    Host lamp
    Hostname ec2-XX-XX-XX-XX-compute-1.amazonaws.com
    User root
    IdentityFile PATHOFTHEKEY
    StrictHostKeyChecking no
    UserKnownHostsFile /dev/null

    To enable root in ubuntu18 for AWS. Login to the server switch to root user removes the line before your key in the Authorisation file.

## To install the respective service run the following.

    git clone git@github.com:srivastava-ami/ansible.git
    cd ansible/lampstack  # lampstack is Linux Apache Mysql PHP
    ansible -i inventory.ini vasnode -m raw -a "apt update && apt install python -y"  #For the first time only as ansible is dependent on python
    ansible-playbook lamp.yaml -i inventory.ini

You will not be able to use the wordpress unless LAMP stack is available. To deploy WordPress run followin.

    cd ansible/wordpress
    vim ./vars/default.yml  # Set the Mysql root password and mysql password.
    ansible-playbook playbook.yml -i inventory.ini

Note: Make sure the version of python on the remote server should be python3, All of my testing is based on that only. You can manually update after installing the python by following.

    cd /usr/bin/
    sudo unlink python
    sudo ln -s python3.6 python
    ls -l python*

Following is the structure.


    [lampstack]
    |
    |____lamp.yaml
    |____index.html
    |____inventory.ini


    [wordpress]
    |
    |____vars
    | |____default.yml
    |____roles
    | |____wordpress
    | | |____tasks
    | | | |____main.yml
    | |____php
    | | |____tasks
    | | | |____main.yml
    | |____firewall
    | | |____tasks
    | |____mysql
    | | |____tasks
    | | | |____main.yml
    | |____apache
    | | |____tasks
    | | | |____main.yml
    |____files
    | |____wp-config.php.j2
    | |____apache.conf.j2
    |____playbook.yml
    |____inventory.ini
