+++
title = "Use the EE in controller"
weight = 60
+++

## Use the EE in controller

### Prerequisites

* Publish an EE to private automation hub
* Configure controller to access the private automation hub and pull the EE

### Tasks

## Configure EE in Controller
- Check the **Credential** Automation Hub Container Registry points to the PAH
- Go to `Execution Environments`
- Configure the new EE
  - **Name**: ee-ansible-demo
  - **Image**: pah.<LABID>.<SUBDOMAIN>.opentlc.com/ee-ansible-demo:latest
  - **Credential**: Automation Hub Container Registry

* Create registry credentials in controller
* Create the EE in controller
* Create a job template using the EE
* Test a playbook with the collection you added to the EE, e.g. perform an action in Azure
* There is an example playbook available in the [playbook-infra](https://github.com/ansible-learnfest/playbooks-infra) project

## Create Project
- Create a new Project to point to https://github.com/ansible-learnfest/ee-flow.git

### Challenge tasks

* Build another EE with different content, import it into Controller and use different job templates with different EE’s
* Think about version pinning in the EE: what’s the best way to do it? How can you track versioning?
* If you use GitLab or GitHub have a look at _renovate_ which can help you to track dependencies. [https://docs.renovatebot.com/](https://docs.renovatebot.com/)

### Goal

* Get familiar with using EE's in controller and different job templates
