+++
title = "Build an execution environment"
weight = 50
+++

## Task 2: Build an execution environment

### Prerequisites

* Install ansible-builder: on RHEL this is provided by the AAP repo, on Fedora you will need “pip install ansible-builder”, it’s recommended to use a virtual environment in this case

### Tasks

* Create an execution environment with the following requirements
  * Base image is `ee-supported-rhel8`
  * Add a community collection e.g. `containers.podman`
  * Add a supported content collection e.g. `azure.azcollection`
  * Experiment with the other options, e.g. adding an RPM or Python package
  * Use `ansible-builder` to build the EE
* Configure `ansible-navigator` to use the previously created EE either by specifying it on the command line or by creating an `ansible-navigator.yml` configuration file
* Inspect your EE by using `ansible-navigator`, e.g. get the list of included collections, ansible versions etc.
* Run a playbook with your EE and verify everything works as expected
* Use environment variables to provide your Azure credentials and write a playbook using the azure.azcollection to create objects in Azure - you can get Azure credentials from RHPDS’ open environments
* you can find an example playbook in the [playbook-infra](https://github.com/ansible-learnfest/playbooks-infra) project

### Goal

* Use your execution environment to create a virtual machine in Azure
* Don't forget to remove the VM and all associated resources - you can actually just remove the `resource group` which will remove all objects created within.

### Tips

* [Ansible-builder documentation](https://access.redhat.com/documentation/en-us/red_hat_ansible_automation_platform/2.1/html/ansible_builder_guide)
* [Ansible Builder upstream documentation](https://ansible-builder.readthedocs.io/en/stable/index.html)
* [Ansible-navigator documentation](https://access.redhat.com/documentation/en-us/red_hat_ansible_automation_platform/2.1/html/ansible_navigator_creator_guide/index)
* [Ansible Builder upstream documentation](https://ansible-navigator.readthedocs.io/en/latest/)
* [List of certified collections](https://access.redhat.com/articles/3642632)
* [Simple EE example](https://gitlab.com/cjung/ansible-ee-intro)
