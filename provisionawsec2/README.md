*Using Ansible to provision AWS EC2 instances*

    Welcome, this article shows a simple approach to use Ansible for provisioning an AWS EC2 instance.

    The following steps will be performed along the article to demonstrate the power around the integration of Ansible and AWS Cloud:
        - Create AWS IAM user
        - Install Ansible EC2 module dependencies
        - Create/Use SSH keys
        - Run Ansible to provision the EC2 instance
        - Connect to the EC2 instance via SSH

    * Create AWS user
        Open the AWS Console, search for IAM (Identity and Access Management) and follow the steps to create a user and take note of the Access Key and Secret Key that will be used by Ansible to set up the instances.

    * EC2 module dependencies
        pip install boto boto3 ansible

    * Create SSH keys to connect to the EC2 instance after provisioning
        ssh-keygen -t rsa -b 4096 -f ~/.ssh/my_aws

    * Create Ansible Vault file to store the AWS Access and Secret keys.
        cd AWS_Ansible
        ansible-vault create group_vars/all/pass.yml
        New Vault password:
        Confirm New Vault password:

        Note: Above is the secure way, where password will be prompt to use the keys for the deployment.
        However, if you don’t want to provide the password every time, an insecure approach can create the pass.yml file by specifying a hashed password file:
            openssl rand -base64 2048 > vault.pass
            ansible-vault create group_vars/all/pass.yml --vault-password-file vault.pass

        With hashed password file you must specify the vault-password-file argument when running Ansible playbook and won’t be asked for the password:
            ansible-playbook playbook.yml --vault-password-file vault.pass

        - Edit the pass.yml file and create the keys global constants
            Create the variables ec2_access_key and ec2_secret_key and set the values gathered after user creation (IAM).
            ansible-vault edit group_vars/all/pass.yml
            Vault password:
            ec2_access_key: AAAAAAAAAAAAAABBBBBBBBBBBB
            ec2_secret_key: afjdfadgf$fgajk5ragesfjgjsfdbtirhf

    * Create the instance
        ansible-playbook playbook.yml --ask-vault-pass --tags create_ec2

    * Connect to the EC2 instance via SSH
        ssh -i ~/.ssh/my_aws ubuntu@ec2-IP.us-east-1.compute.amazonaws.com
