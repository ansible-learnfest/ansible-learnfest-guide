+++
title = "Prepare Environment"
weight = 10
+++

## Setup your LearnFest Environment

Students will need the following information to deploy their personal AAP installation:

- AWS credentials, either from [RHPDS Open Environments](https://demo.redhat.com) or bring your own

- RHN user name and password for log into [RHN Subscription Management](https://access.redhat.com/management)

- RHN Pool ID with an active Ansible Red Hat Automation Platform Subscription, typically this is your Employee SKU

### Create student environment

Your instructor will provide you access credentials for the Ansible LearnFest automation controller. Navigate to the Job Template "LearnFest - Create Student environment" and fill out all details.

Enter the data into the following form fields:

- Username: the user name you want to be created, you will use this to log into the LearnFest Automation Controller to manage your infrastructure

- Password: the password for your user, this will also be the password you later use to log into your environment

- AWS Access Key: the key to log into your AWS environment, this will be sent to you when your order your RHPDS Open Environment

- AWS Secret Key: the secret key for your AWS account

- RHN User Name: your username to log into [RHN Subscription Management](https://access.redhat.com/management), this will be used to register your system

- RHN Password: your password to log into [RHN Subscription Management](https://access.redhat.com/management), this will be used to register your system

- RHN Pool ID: a pool ID granting you access to the Red Hat Ansible Automation Platform repository, typically it's the pool ID of your "Employee SKU", or "Red Hat Ansible Automation Platform"

Click **Next** and verify all details. Then click **Launch**. You will be redirected to the Playbook output and you can monitor the progress.

### After the job completed

After the job has finished, you can log into the automation controller with the credentials you just specified. This Playbook created the following items:

- your personal user account with the username and password you specified

- AWS credentials and granted you full access to them

- a dynamic inventory for your AWS account you specified and granted you full access to it

- a Job Template to deploy AAP: "Deploy AAP for student"

- a Job Template to deploy Visual Studio Code Server: "Deploy VS Code Server" - see more details in the next chapter

- a Job Template to complete delete your AAP deployment: "Remove AAP for student" - only use this if your environment is utterly broken and can't be fixed, or if you want to clean up

- a Job Template to complete delete your VS Code Server: "Remove VS Code Server" - only use this if your VS Code server is utterly broken and can't be fixed, or if you want to clean up

All deployment playbooks are idempotent and can be launched again if needed, e.g. to fix a broken installation.

### Deploy Automation Platform

After your student environment was created (this should not take more than a minute), you can log in with the credentials you just provided.

You should notice that a job "Deploy AAP for student" is already running! This will take 30-45 minutes to complete!

**NOTE:** If your deployment job fails, try it again and Ansible should fix the problem automatically. Since we heavily use cloud services, it's unfortunately out of control that some services sometimes have a hick up and fail - most of the time if you try again, the Job will succeed.

Among other things, the Playbook will perform the following actions:

- create a VPC, security group, SSH keys etc. in your AWS account

- create an instance for automation controller and a second one for automation hub in your AWS account

- install Ansible Automation Platform on the controller and hub

The deploy playbook is idempotent - in case of an error you can launch it again safely.

### Optional VS Code Server

You can use your own machine to write all the code necessary for participating in the LearnFest. For this, you will need the following tools:

- VS Code or any other editor you feel comfortable with

- VS Code Ansible extension: recommended, but not required

- ansible-navigator

- ansible-builder

If you don't have a system available with these tools, you can launch the "Deploy VS Code Server" from the LearnFest automation controller. This will deploy an instance with the Web Version of VS Code pre-installed into your AWS account. `ansible-builder` and `ansible-navigator` are not already installed on VS Code Server.
