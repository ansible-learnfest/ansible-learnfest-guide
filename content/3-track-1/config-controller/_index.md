+++
title = "Configure Automation Controller"
weight = 20
+++

## Task 2: Configure Automation Controller 
Okay, Automation Controller is up and running but pretty empty. In this task you add some automation content to it. To make it not too easy and resemble more real life situations, you will use **Ansible to configure Automation Controller objects**.

**Prerequisites**
* Automation Controller is running & accessible
* You can run `ansible-navigator` on a laptop/VM.


**Tasks**
* Create a Playbook using the `ansible.controller` collection to create these objects in your Automation Controller:
  * **Inventory**
  * **Hosts** in the inventory, with your instance number 5 only
  * **Machine credentials** to connect to your managed host
  * **Project** to pull Ansible content from
  * **Template** to install Apache on the managed hosts 
* If you haven't done this before it might sound daunting, but fear not: [Here](https://ansible-labs-crew.github.io/ansible-controller-advanced/3-awx-collection-intro/) is a chapter from one of our labs, you just have to migrate it to your infrastructure.

{{% notice tip %}}
BTW you don't have to install the `ansible.controller` collection because it's already baked in into the EE we used before to deploy instances.
{{% /notice %}}


Okay...
* On your system create a new directory next to the one with the cloned git repo from deploying the AWS instances. 
* In the new directory create a Playbook resembling the one from our lab. Change:
  * Replace all `awx.awx.*` module calls with `ansible.controller.*` (we are using the supported collection here)
  * Change the `loop` in the creating hosts section to only contain the public FQDN of your instance number 5
  * Make sure `ssh_key_data:` points to the SSH key you use to connect to the instances 

{{% notice tip %}}
You have seen how `ansible-navigator` knew which Execution Environment image to use, right? But this `ansible-navigator.yml` config magic only works when you run `ansible-navigator` in the same directory. You can specify the EE and other parameters on the commandline but it's easier to track what you've done to copy the `ansible-navigator.yml` file over to your new directory and change it.
{{% /notice %}}

* Configure credentials for the modules to talk to your Automation Controller:
  * Set the environment variables **CONTROLLER_HOST**, **CONTROLLER_USERNAME**, **CONTROLLER_PASSWORD** with the values of your Automation Controller and set **CONTROLLER_VERIFY_SSL=false**
  * **Important**: Add the env vars to the `ansible-navigator.yml` file so they are available in the running container!
* Run the Playbook using `ansible-navigator`
  * Hint: Use `-m stdout` to mimic the output of `ansible-playbook`


**Goal**
* The Playbook configured the objects needed to install and run Apache on instance 5
* Opening the default webpage in a browser works.

**Tips**
* The collection/module documentation is here in [Automation Hub](https://console.redhat.com/ansible/automation-hub). Go to **Collections** and filter for `controller`.
* If you get confused by all the naming changes (`ansible.controller` in Automation Hub, `awx.awx` upstream, lot's of references to `Tower` in the docs)... don't be, all will finally be good! (TM)
