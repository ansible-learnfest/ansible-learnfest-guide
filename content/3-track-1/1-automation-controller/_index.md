+++
title = "Install Automation Controller"
weight = 15
+++

## Task 1: Install Automation Controller 

You should now have access to your AWS instances and you should be able to SSH into them. It’s time to deploy Ansible Automation Platform 2.2! We’ll start with installing Automation Controller on your instance 1.

**Prerequisites**
* Able to SSH into your instances and to become root
* A Red Hat Customer Portal account with an AAP subscription

**Tasks**
* SSH into your instance number 1 to run the installer there.
* Become `root`: `sudo -i` 
* Download the **Ansible Automation Platform 2.2.0 Setup Bundle** installer from Customer Portal to your instance. 
    * The link to the download location can also be found in the installation docs. 
    * Tip: Copy the download link and use `curl` with single quotes around the link and the `-o <filename>` option to save to file. 

**In general follow the installation documentation to install Automation Controller with a database on the same node.** 

* Extract installer tarball, change into the directory. 
* Create the most simple `inventory` file, do not install privat automation hub or anything else yet. Backup the default `inventory` file before. 
    * There is an example for a basic inventory file for our simple installation scenario in chapter 2 of the installation documentation (see tips below). 
    * **Attention: you can’t use “localhost” in the inventory file  anymore, change it to the FQDN of the instance**.
    * You have to change all `<...>` values in the template 
* Hints:
    * You can skip the docs section “Attaching your Red Hat Ansible Automation Platform subscription“ because we are using the bundle installer and the AWS RHEL instances are already attached to the default RHEL 8 repos.
* Run the platform installer with `./setup.sh`
* When Automation Controller has been installed: 
    * Login and attach an AAP Subscription using your Customer Portal User/Password
    * Allow Analytics
    * Accept 
* There you have it, your Automation Controller is up and running.

**Goal**
* Being able to log in to the web UI with the password you provided in the inventory file.

**Tips**
* [Ansible Automation Platform 2.2 Installation Guide](https://access.redhat.com/documentation/en-us/red_hat_ansible_automation_platform/2.2/html-single/red_hat_ansible_automation_platform_installation_guide/index)
* There is a [link](https://access.redhat.com/RegistryAuthentication#creating-registry-service-accounts-6) to instructions for creating a registry account in the docs (“Creating Registry Service Accounts guide”)
* There are instructions for obtaining the Red Hat Ansible Automation Platform installer in the docs (“Choosing and obtaining a Red Hat Ansible Automation Platform installer”)