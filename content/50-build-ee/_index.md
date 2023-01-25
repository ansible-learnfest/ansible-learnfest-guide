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

Install `ansible-builder` in your lab environment: on RHEL this is provided by the AAP repo, on Fedora you will need `pip install ansible-builder`, it’s recommended to use a virtual environment in this case.

{{% notice note %}}
`ansible-builder` is a high-level tool for building AAP Execution Environments that abstracts away a lot of the intricacies of container image building. Under the hood it uses `podman`, of course.
{{% /notice %}}

```bash
# Only if you're not on RHEL
dnf -y install python3-virtualenv
virtualenv ansible-builder
. ansible-builder/bin/activate
pip install -U pip ansible-builder
# If you are on RHEL and the AAP repo is enabled
sudo yum install ansible-builder
```

### Tasks

In the previous chapter you have learned how you can still use collections not contained in an Execution Environment in a Playbook. But in many cases you'll start building custom EE's at some point containing collections you use frequently in your Ansible content.

In this chapter you'll do exactly this: build a custom Execution Environment.
#### Checkout repo

We have prepared a repository with the needed content to build the EE image. Go and clone the repo to your VS Code terminal:

```bash
git clone https://github.com/ansible-learnfest/ee-flow.git
```

#### Login to registry.redhat.io

As the base image will be pulled from the Red hat container registry, you have to login with your personal Red Hat login credentials (the one you use on [access.redhat.com](https://access.redhat.com)) in the VS Code terminal:

```bash
podman login registry.redhat.io
```

#### Build Execution Environment

* Change to the `ee-flow/ansible-builder/` directory
* Examine the three files that describe your build (these are all in the default state listing nothing so far):
  * `bindep.txt` lists rpm that need to be installed into the EE
  * `requirements.txt` is for installing additional Python dependencies
  * `requirements.yml` might be the most important by listing the Collections which should go into the EE.

{{% notice note %}}
The three files describing the content and possible additional build steps are pulled together in a `.yml` build file, `ee-ansible-demo.yml` in our case. Again feel free to examine the file.
{{% /notice %}}

* We need the `containers.podman` collection in the EE, change the `requirements.yml` to include it (hint: use the content of the file you uploaded to PAH in the previous chapter).
* That's all. Run `ansible-builder` to create the new EE as in the example below (make sure you are in `ee-flow/ansible-builder/`):

```bash
ansible-builder build -f ee-ansible-demo.yml -t ee-ansible-demo:0.1.0 -v 3
```

{{% notice note %}}
We're using the `-v 3` flags to get more detailed output from `ansible-builder` - by default the tool is very quiet. Also note how we give a name to the image together with a tag.
{{% /notice %}}

* After the build has finished, check the image is there:

```bash
podman images
```

As said `podman` is used to actually build the image, the `Containerfile` needed is created by `ansible-builder`. Please take the time to locate it and have a look at it.

#### Use the Execution Environment

Before we push a custom EE to a registry and use it in Automation Controller we want to make sure it provides what we need to run our Playbooks with all dependencies. Basically check if it works... :-)

For this we run a Playbook in the runtime environment the EE provides. Because `ansible-playbook` can't do this, we need to use the second new tool on the block, `ansible-navigator`:

* Configure `ansible-navigator` to use the previously created EE either by
  * specifying it on the command line
  * or by creating an `ansible-navigator.yml` configuration file.
* To specify the EE image on the command line, look for `Specify the name of the execution environment image` in the output of `ansible-navigator --help`.
* Or if you prefer to make the necessary changes in the (already existing) `~/.ansible-navigator.yaml` file:

```yaml
---
ansible-navigator:
  execution-environment:
    image: ee-ansible-demo:0.1.0
```

* After pointing to our new EE, run `ansible-navigator` and start to explore its features. Navigating is straight forward, try the following:
  * Get a list of included collections with `:collections` on the start page. `ESC` always takes you back one step in the menu structure.
  * Inspect the image with `:images`
  * Just look around and remember `ESC` is your friend when you got lost in menus items...
* Exit `ansible-navigator`

Now we finally want to run a Playbook to test the new EE. The demo Playbook you used before in Automation Controller is part of the GitHub repo `ee-flow` you checked out already. So we just need an inventory to go with it.

* Copy the inventory so we can modify it

```bash
cp /etc/ansible/hosts lab_inventory.ini
```

* Edit the inventory file `~/ee-flow/ansible-builder/lab_inventory.ini` and remove or comment out all nodes except 'node2'. The result should look something like this (note all other lines are removed!):

```ini
[managed_nodes]
#node1.<LABID>.internal
node2.<LABID>.internal
#node3.<LABID>.internal
```

* Start `ansible-navigator` and run the Playbook:
  * `:run ~/ee-flow/deploy-container.yml -i ~/ee-flow/ansible-builder/lab_inventory.ini`
  * While it's running look at the task execution by hitting a line number.
  * After the run has finished, leave `ansible-navigator` with multiple `ESC`
  * Check the deployment worked:

```bash
curl node2
```

There is a command line parameter to `ansible-navigator` that makes the run and output mimic `ansible-playbook`, that is not jumping into the TUI interface, give it a try:

```bash
ansible-navigator run ~/ee-flow/deploy-container.yml -i ~/ee-flow/ansible-builder/lab_inventory.ini -m stdout
```

### Goals

* Build an EE adding at least one collection.
* Use `ansible-navigator` to inspect and run a Playbook in a custom EE.

### Tips

* [Ansible-builder documentation](https://access.redhat.com/documentation/en-us/red_hat_ansible_automation_platform/2.3/html/ansible_builder_guide)
* [Ansible Builder upstream documentation](https://ansible-builder.readthedocs.io/en/stable/index.html)
* [Ansible-navigator documentation](https://access.redhat.com/documentation/en-us/red_hat_ansible_automation_platform/2.3/html-single/ansible_navigator_creator_guide/index)
* [Ansible Builder upstream documentation](https://ansible-navigator.readthedocs.io/en/latest/)
* [List of certified collections](https://access.redhat.com/articles/3642632)
* [Simple EE example](https://gitlab.com/cjung/ansible-ee-intro)
