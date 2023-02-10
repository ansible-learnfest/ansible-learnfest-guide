+++
title = "Use the EE in controller"
weight = 60
+++

## Use the EE in controller

You have tested running a Playbook that depends on the Podman collection with `ansible-navigator` in your custom Execution Environment. Now it's time to use the custom EE with the Podman collection in Automation Controller.

### Prerequisites

* The custom EE was pushed and is published in PAH

### Tasks

* Check Automation Controller is configured to use EE's in PAH
* Use the custom EE in Controller

### Configure EE in Controller

* Check the **Credential** `Automation Hub Container Registry` points to PAH
  * **Name**: Automation Hub Container Registry
  * **Organization**: Default
  * **Credential Type**: Container Registry
  * **Authentication URL**:
  * **Username**: admin
  * **Password**: &lt;your secret password>

{{% notice note %}}
You can find the Authentication URL on your private automation hub by navigating to **Execution Environments**, select the EE you want to use and then in **Details**.
{{% /notice %}}

* Go to `Execution Environments` and add the new EE
  * **Name**: ee-ansible-demo
  * **Image**: \<PAH Hostname>/\<image name>:\<tag>
  * **Pull**: Always
  * **Registry Credential**: Automation Hub Container Registry
  * **Organization**: Default

{{% notice note %}}
Get the image location from your PAH: In **Execution Environments** click the image.
{{% /notice %}}

### Bring it all together

This time we'll run the Playbook you used before, but in your custom EE! That means we shouldn't have to use a `requirements.yml` file to automatically load the needed collection because `containers.podman` is already included in the EE.

* Create a new **Project** using `https://github.com/ansible-learnfest/ee-flow.git` **with the branch `wo-requirements`**. This branch contains no `requirement.yml` to automatically add the collection. If you want to double check, have a look a the Job output and you'll notice that no collection was installed during project sync.
* Create a **Job Template** with the same settings as in the `Using Private Automation Hub` chapter, or just create a copy of it. We want to make one change in the copied job template:
  * Set the **Execution Environment** to `ee-ansible-demo` you just created in the previous section of this lab.
* Edit your  **Workshop Inventory**: add `node3.<LABID>.internal` and disable `node1.<LABID>.internal`.
* Run the Job Template and check the outcome

### And finally: The Challenge

Now that you have learned how all the stages of building, testing, pushing and using a custom Execution Environment work, do it without help from the guide. Here are your tasks:

* The Playbook `enforce-selinux.yml` in `https://github.com/ansible-learnfest/playbooks-challenge.git` should be run with your custom EE against your managed nodes.
* It uses a collection that **is not part of your EE** yet.

So this is what you have to do:

* Add the needed collection to your EE definition and build a new version.
* Because the collection comes from Red Hat Automation Hub and not Ansible Galaxy, you need to create a `ansible.cfg` like this (fill in your AH token):

```
[galaxy]
server_list = automation_hub

[galaxy_server.automation_hub]
url=https://cloud.redhat.com/api/automation-hub/
auth_url=https://sso.redhat.com/auth/realms/redhat-external/protocol/openid-connect/token
token=my_ah_token
```

* Tell `ansible-builder` in the definition file to read the `ansible.cfg` file:

```
[...]
dependencies:
  galaxy: requirements.yml
  python: requirements.txt
  system: bindep.txt

ansible_config: /path/to/ansible.cfg
[...]
```

* Using `ansible-navigator` inspect the new image to contain the needed collection
* Push the new image to your PAH
* Create a **Job Template** using the **Project** with the `enforce-selinux.yml` Playbook and the new version of the EE
* Run it!

### Goal

* Get familiar with using EE's in controller and different job templates

### Tips

You can disable automation controller's feature to automatically download and install Ansible Collections by navigating to **Settings** -> **Job Settings**. Here you can disable `Enable Collection(s) Download` and `Enable Role Download`. By doing so, you force the automation controller to only use collections which are already part of the execution environment. This can be useful to make sure the collection inside the EE is not overridden by a newer version provided by your private automation hub or Ansible Galaxy. This gives you more predictable and reproducible Ansible Playbook execution.
