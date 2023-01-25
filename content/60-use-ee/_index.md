+++
title = "Use the EE in controller"
weight = 60
+++

## Use the EE in controller

You have tested running a Playbook that depends on the Podman collection with `ansible-navigator`. Now it's time to use the custom EE with the Podman collection in Automation Controller.
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
Get the image location from your PAH: In **Execution Environemnts** click the image.
{{% /notice %}}

### Bring it all together

This time we'll run the Playbook you used before, but in your custom EE! That means we shouldn't have to use a `requirements.yml` file to automatically load the needed collection because `containers.podman` is already included in the EE.

* Create a new **Project** using `https://github.com/ansible-learnfest/ee-flow.git` **with the branch `wo-requirements`**. This branch contains no `requirement.yml` to automatically add the collection. If you want to double check, have a look a the Job output and you'll notice that no collection was installed during project sync.
* Create a **Job Template** with the same settings as in the `Using Private Automation Hub` chapter, or just create a copy of it. We want to make one change in the copied job template:
* Set the **Execution Environment** to the you just created in the previous section of this lab.
* Edit your  **Workshop Inventory** and activate `node3` and remove or disable other nodes.
* Run the Job Template and check the outcome

### Challenge tasks

* Build another EE with different content, import it into Controller and use different job templates with different EE’s
* Think about version pinning in the EE: what’s the best way to do it? How can you track versioning?
* If you use GitLab or GitHub have a look at [RenovateBot](https://renovatebot.com) which can help you to track dependencies. [https://docs.renovatebot.com/](https://docs.renovatebot.com/)

### Goal

* Get familiar with using EE's in controller and different job templates

### Tips

You can disable automation controller's feature to automatically download and install Ansible Collections by navigating to **Settings** -> **Job Settings**. Here you can disable `Enable Collection(s) Download` and `Enable Role Download`. By doing so, you force the automation controller to only use collections which are already part of the execution environment. This can be useful to make sure the collection inside the EE is not overridden by a newer version provided by your private automation hub or Ansible Galaxy. This gives you more predictable and reproducible Ansible Playbook execution.
