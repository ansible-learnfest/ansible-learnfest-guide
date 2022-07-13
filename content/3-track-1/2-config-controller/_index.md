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
  * **Hosts** in the inventory, assuming you didn't change it this should only be your instance number 5
  * **Machine credentials** to connect to your managed host
  * **Project** to pull Ansible content from
  * **Template** to install Apache on the managed hosts 
* If you haven't done this before it might sound daunting, but fear not: You can find a complete template below in the **Tips** section.

{{% notice tip %}}
BTW you don't have to install the `ansible.controller` collection because it's already baked into the EE we used before to deploy instances.
{{% /notice %}}


Okay...
* On your system create a new directory next to the one with the cloned git repo from deploying the AWS instances. 
* In the new directory create a Playbook resembling the one from our lab. 
  * **Note**: We are using the supported collection `ansible.controller` here, upstream is called `awx.awx`.
  * Change the `loop` in the creating hosts section to only contain the public FQDN of your Controller instance.
  * Make sure `ssh_key_data:` points to the SSH key you use to connect to the instances 

{{% notice tip %}}
You have seen how `ansible-navigator` knew which Execution Environment image to use, right? But this only works if you have the needed configuration in `ansible-navigator.yml`living in the same directory you run `ansible-navigator` from. You can specify the EE and other parameters on the commandline but it's easier to track what you've done in the config file. As a start **copy the `ansible-navigator.yml` file over** to your new directory and adapt it if needed.
{{% /notice %}}

* Configure credentials for the `ansible.controller` collection modules to be able to talk to your Automation Controller:
  * Set the environment variables **CONTROLLER_HOST**, **CONTROLLER_USERNAME**, **CONTROLLER_PASSWORD** with the values of your Automation Controller and set **CONTROLLER_VERIFY_SSL=false**
  * **Important: Add the env vars above to the `ansible-navigator.yml` file**
    * This way they are available in the running container!
    * **Add only the name of the variable under the env vars already in the file!**
* Run the Playbook using `ansible-navigator`
  * Hint: Use `-m stdout` to mimic the output of `ansible-playbook`


**Goal**
* The Playbook configured the objects needed
* The **Template** run installed and started Apache on your managed node, should be instance 5
* Opening the default webpage on the managed node in a browser works.

**Tips**
* Here is the template for the Playbook:

<details><summary><b>Click here for Template</b></summary>
<hr/>
<p>

```
---
- name: Configure automation controller
  hosts: localhost
  become: false
  gather_facts: false
  tasks:
  - name: Create an inventory
    ansible.controller.inventory:
      name: Learnfest Inventory
      organization: Default
  - name: Add hosts to inventory
    ansible.controller.host:
      name: "{{  item }}"
      inventory: Learnfest Inventory
      state: present
    loop:
      - <instance-5>
  - name: Machine Credentials
    ansible.controller.credential:
      name: Learnfest Credentials
      credential_type: Machine
      organization: Default
      inputs:
        username: ec2-user
        ssh_key_data: "{{ lookup('file', '~/.ssh/<GUID>key.pem' ) }}"
  - name: Learnfest Project
    ansible.controller.project:
      name: Learnfest Project
      organization: Default
      state: present
      scm_update_on_launch: True
      scm_delete_on_update: True
      scm_type: git
      scm_url: https://github.com/ansible-learnfest/playbooks-example.git
  - name: Learnfest Job Template
    ansible.controller.job_template:
      name: Install Apache
      organization: Default
      state: present
      inventory: Learnfest Inventory
      become_enabled: True
      playbook: apache_install.yml
      project: Learnfest Project
      credential: Learnfest Credentials
```

</p>
<hr/>
</details>

* The collection/module documentation is here in [Automation Hub](https://console.redhat.com/ansible/automation-hub). Go to **Collections** and filter for `controller`.
* If you would like to know what collections are in a certain Execution Environment, run `ansible-navigator collections` against the EE in question.
* If you get confused by all the naming changes (`ansible.controller` in Automation Hub, `awx.awx` upstream, lot's of references to `Tower` in the docs)... don't be, all will finally be good! (TM)

