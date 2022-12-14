+++
title = "Build an execution environment"
weight = 50
+++

## Build an execution environment

Execution environments are Linux container images which are built on top of the [Red Hat Universal Base Images](https://www.redhat.com/en/blog/introducing-red-hat-universal-base-image) (UBI) and additionally contain:

* ansible-core

* Python and its dependencies

* Ansible collections and their dependencies

* optionally additional software packages like RPMs

This makes the execution of an ansible playbook more scalable, reliable and predictable since the combination of Playbook and execution environment should always deliver the same results.

### Prerequisites

* Install ansible-builder: on RHEL this is provided by the AAP repo, on Fedora you will need “pip install ansible-builder”, it’s recommended to use a virtual environment in this case.

### Tasks

#### Checkout repo

Clone the repo to your VS Code terminal

```bash
git clone https://github.com/ansible-learnfest/ee-flow.git
```

#### Login to registry.redhat.io

```bash
podman login registry.redhat.io
```

#### Build Execution Environment

* In the repo change to the `ansible-builder` directory

* Run `ansible-builder` to create the new EE as in the example below:

```bash
ansible-builder build -f ee-ansible-demo.yml -t ee-ansible-demo:0.1.0 -v 3
```


* Experiment with the other options, e.g. adding an RPM or Python package

#### Use the Execution Environment

* Configure `ansible-navigator` to use the previously created EE either by specifying it on the command line or by creating an `ansible-navigator.yml` configuration file.

* Inspect your EE by using `ansible-navigator`, e.g. to get the list of included collections, ansible versions etc.

* Run a playbook with your EE and verify everything works as expected.

  * Write a playbook using the `containers.podman` to create containers or use the previously used example on [ee-flow](https://github.com/ansible-learnfest/ee-flow.git) project without the `requirements.yml` file.
 
  * What do you notice

{{% notice note %}}
In a production environment we typically recommend to use the `ee-minimal-rhel8` as a base image and only add the collections we specifically need. To make this lab not too complex, we decided to use the EE supported as a base image though.
{{% /notice %}}

### Goals

* Build an EE adding at least one collection

* Use your execution environment to create a container.

### Tips

* [Ansible-builder documentation](https://access.redhat.com/documentation/en-us/red_hat_ansible_automation_platform/2.1/html/ansible_builder_guide)

* [Ansible Builder upstream documentation](https://ansible-builder.readthedocs.io/en/stable/index.html)

* [Ansible-navigator documentation](https://access.redhat.com/documentation/en-us/red_hat_ansible_automation_platform/2.3/html-single/ansible_navigator_creator_guide/index)

* [Ansible Builder upstream documentation](https://ansible-navigator.readthedocs.io/en/latest/)

* [List of certified collections](https://access.redhat.com/articles/3642632)

* [Simple EE example](https://gitlab.com/cjung/ansible-ee-intro)
