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

* Check the **Credential** Automation Hub Container Registry points to the PAH

* Go to `Execution Environments` and configure the new EE

  * **Name**: ee-ansible-demo

  * **Image**: hub-student.LABID.example.opentlc.com/ee-ansible-ssa/latest

  * **Credential**: Automation Hub Container Registry
  
* Create registry credentials in controller.

  * Don't forget to untick `Verify SSL` 

* Create the EE in controller

* Create a job template using the newly created EE

* Test a playbook with the collection you added to the EE, e.g. deploy a new container

* There is an example playbook available in the [deploy-container.yml](https://github.com/ansible-learnfest/ee-flow) project

### Create Project

* Create a new Project to point to [https://github.com/ansible-learnfest/ee-flow.git](https://github.com/ansible-learnfest/ee-flow.git)

### Challenge tasks

* Build another EE with different content, import it into Controller and use different job templates with different EE’s

* Think about version pinning in the EE: what’s the best way to do it? How can you track versioning?

* If you use GitLab or GitHub have a look at _renovate_ which can help you to track dependencies. [https://docs.renovatebot.com/](https://docs.renovatebot.com/)

### Goal

* Get familiar with using EE's in controller and different job templates

### Tips
