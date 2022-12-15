+++
title = "Use the EE in controller"
weight = 60
+++

## Use the EE in controller

### Prerequisites

* Publish an EE to private automation hub

* Configure controller to access the private automation hub and pull the EE

### Tasks

### Configure EE in Controller

* Check the **Credential** Automation Hub Container Registry point to the PAH

  * **Name**: private automation hub

  * **Organization**: Default

  * **Credential Type**: Container Registry

  * **Authentication URL**:

  * **Username**: admin

  * **Password**: &lt;your secret password>

  * **Verify SSL**: disabled

{{% notice note %}}
You can find the Authentication URL on your private automation hub by navigating to **Execution Environments**, select the EE you want to use and then in the **Details**.
{{% /notice %}}

* Go to `Execution Environments` and add the new EE

  * **Name**: ee-ansible-demo

  * **Image**: hub-student.LABID.example.opentlc.com/ee-ansible-ssa/latest

  * **Pull**: Always

  * **Credential**: private automation hub

  * **Organization**: Default

* Create a job template using the newly created EE

* Test a playbook with the collection you added to the EE, e.g. deploy a new container

* There is an example playbook available in the [deploy-container.yml](https://github.com/ansible-learnfest/ee-flow) project

### Challenge tasks

* Build another EE with different content, import it into Controller and use different job templates with different EE’s

* Think about version pinning in the EE: what’s the best way to do it? How can you track versioning?

* If you use GitLab or GitHub have a look at [RenovateBot](https://renovatebot.com) which can help you to track dependencies. [https://docs.renovatebot.com/](https://docs.renovatebot.com/)

### Goal

* Get familiar with using EE's in controller and different job templates

### Tips

You can disable automation controller's feature to automatically download and install Ansible Collections by navigating to **Settings** -> **Job Settings**. Here you can disable `Enable Collection(s) Download` and `Enable Role Download`. By doing so, you force the automation controller to only use collections which are already part of the execution environment. This can be useful to make sure the collection inside the EE is not overridden by a newer version provided by your private automation hub or Ansible Galaxy. This gives you more predictable and reproducible Ansible Playbook execution.
