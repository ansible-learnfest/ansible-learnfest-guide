+++
title = "The VSCode extension"
weight = 20
+++

## Introduction to the VSCode extension

We highly recommend making yourself familiar with Visual Studio Code and the Ansible extension. Even if you’re not planning to use it as your daily driver, it is good to know how it works to explain and show the benefit to customers.

### Prerequisites

* You should have a Linux workstation setup

* Install `ansible-core`, `ansible-lint` and `podman` on your Linux workstation

* Install `ansible-navigator` for using execution environments

### Tasks

* Install VSCode [https://code.visualstudio.com/](https://code.visualstudio.com/)

{{% notice note %}}
If your lab was deployed from RHPDS you already have VS Code Server available and don't have to install it again.
{{% /notice %}}

* Install the Ansible extension: [https://marketplace.visualstudio.com/items?itemName=redhat.ansible](https://marketplace.visualstudio.com/items?itemName=redhat.ansible)

* To install the extension, click on the **Extensions** menu on the left navigation bar, or from the **View** menu.

* Search for "Ansible" and make sure to install the collection provided by Red Hat

{{% notice note %}}
If you use VSCode server, you might see a "reload" button which you need to click to complete the installation.
{{% /notice %}}

You can now test the extension by performing the following tasks.

* Create a playbook/role/collection

* Set **Language Mode** to Ansible

  Either on the VSCode status bar at the bottom of the window or by putting this into `~/.local/share/code-server/User/settings.json`:

```json
"files.associations": {
        "*.yml": "ansible",
        "*.yaml": "ansible"
    },
```

{{% notice warning %}}
If you see this pop-up error message, you might have to downgrade your ansible-core package as instructed below:
`Command failed: ansible-lint --offline --nocolor -f codeclimate "/home/student/rhel-workshop/1.3-playbook/apache.yml"`\
`ERROR No module named 'ansible' FATAL: ansible-lint requires a version of Ansible package >= 2.9, but none was found.`\
`Please install a compatible version using the same python interpreter.See` https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#installing-ansible-with-pip\
Then, try to downgrade your ansible-core package and reload your code-server page:\
`$ sudo dnf install ansible-core-2.12.1`
{{% /notice %}}

* tab completion

* syntax highlighting

* tool tips for modules: hover over a module FQCN, `Ctrl+Click` on a module FQCN to open its documentation

* `Ctrl+Space` for module parameters, e.g. add a task calling `ansible.builtin.yum` and use `Ctrl+Space` to see all module attributes and attribute parameters

* Make syntax error and notice errors in the Problem pane (`Ctrl+Shift+m`): you might have to make sure `ansible-lint` is enabled and notice it is only checking when you save a file

### Goals

Start VSCode, install the Ansible extension and get familiar with its features.

### Tips

Although VSCode of course runs on Linux, Mac and Windows, there are some extra tasks to be done to get Ansible working on Windows or Mac. It's beyond the scope of this manual to provide detailed instructions on how to setup Podman and Ansible on Windows or Mac.

### Challenge Tasks

Try to configure VSCode to use Ansible inside an execution environment. The EE supported is already installed on the bastion host and can be used for these experiments.
