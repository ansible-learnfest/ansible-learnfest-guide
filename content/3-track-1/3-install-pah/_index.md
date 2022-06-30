+++
title = "Install Private Automation Hub"
weight = 25
+++

## Task 3: Install Private Automation Hub

The next step on your way to a full AAP infrastructure is to install **Private Automation Hub (PAH)** as on-premise complement to **Red Hat Automation Hub**. PAH allows to mirror Automation Hub and Ansible Galaxy collections as well as own in-house content. And in addition PAH can act as a container registry for execution environments images.  

**Prerequisites**
* Automation Controller is running & accessible
* You can connect to your instance 2

**Tasks**
* Install Private Automation Hub on your instance number 2, the documentation is [here](https://access.redhat.com/documentation/en-us/red_hat_ansible_automation_platform/2.2/html-single/red_hat_ansible_automation_platform_installation_guide/index#single-machine-scenario) in chapter **2.3**.
* The easiest way is to SSH into to the PAH instance, download the installer bundle like you did for Controller and install PAH locally.

{{% notice tip %}}
You can run the AAP installer in different ways, of course. You could run it from a competely different machine and connect to the hosts AAP is to be installed on, you could run it on the Controller instance like you did and have it connect to the PAH instance... but for now do it the easy way so you don't have to fiddle with SSH keys and `become` settings. :)
{{% /notice %}}

* So go ahead, SSH to instance 2, become `root` and download the installer bundle again
* Take the inventory PAH example from the docs referenced above and adapt it to your needs.
* Run the installer with your customized inventory file

**Goal**
* The Playbook configured the objects needed to install and run Apache on instance 5
* Opening the default webpage in a browser works.

**Tips**
* The collection/module documentation is here in [Automation Hub](https://console.redhat.com/ansible/automation-hub). Go to **Collections** and filter for `controller`.
* If you get confused by all the naming changes (`ansible.controller` in Automation Hub, `awx.awx` upstream, lot's of references to `Tower` in the docs)... don't be, all will finally be good! (TM)
