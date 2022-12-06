+++
title = "The VSCode extension"
weight = 20
+++

## Learn about the VSCode extension

We highly recommend making yourself familiar with VSCode and the Ansible extension. Even if you’re not planning to use it as your daily driver, it is good to know how it works to explain and show the benefit to customers.

TODO: Check this still works in VSCode Server - it's probably already installed

### Prerequisites

* You should have a Linux workstation setup

* Install `ansible`, `ansible-lint` and `podman` on your Linux workstation

* Install `ansible-navigator` for using execution environments

### Tasks

* Install VSCode [https://code.visualstudio.com/](https://code.visualstudio.com/)

* Install the Ansible extension: [https://marketplace.visualstudio.com/items?itemName=redhat.ansible](https://marketplace.visualstudio.com/items?itemName=redhat.ansible)

* Create a playbook/role/collection

* Get familiar with the VSCode extension features

* Set **Language Mode** to Ansible

  Either on the VSCode status bar or put this into `~/.config/Code/User/settings.json`:

```json
"files.associations": {
        "*.yml": "ansible",
        "*.yaml": "ansible"
    },
```

* tab completion

* syntax highlighting

* tool tips for modules: hover over a modul FQCN, `Ctrl+Click` on a modul FQCN to open documentation

* `Ctrl+Space` for module parameters, e.g. add a task calling `ansible.builtin.yum` and use `Ctrl+Space` to see all module attributes and attribute parameters

* Make syntax error and notice errors in the Problem pane (`Ctrl+Shift+m`): you might have to make sure `ansible-lint` is enabled and notice it is only checking when you save a file

### Goal

Start VSCode and get familiar with its features.

### Tips

Although VSCode of course runs on Linux, Mac and Windows, there are some extra tasks to be done to get Ansible working on Windows. It's beyond the scope of this manual to provide detailed instructions on how to setup Podman and Ansible on Windows.

### Challenge Tasks

Try to configure VSCode to use Ansible inside an execution environment.
