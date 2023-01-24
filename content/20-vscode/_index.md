+++
title = "The VSCode extension"
weight = 20
+++

## Introduction to the VSCode extension

We highly recommend making yourself familiar with Visual Studio Code and the Ansible extension. Even if you’re not planning to use it as your daily driver, it is good to know how it works to explain and show the benefit to customers.

### Prerequisites

{{% notice note %}}
Because your lab was deployed from RHPDS you already have VS Code Server (basically VSCode in a browser) available in your lab environment. You don't have to install anything locally.
{{% /notice %}}

Make sure you can access your VS Code Server instance using the credentials provided.

### Tasks

* Make sure the extension is installed (Hint: **Extensions** menu on the left navigation bar, or from the **View** menu.)

{{% notice note %}}
You might see a "reload" button next to the extension which you need to click to complete the installation.
{{% /notice %}}

You can now test the extension by performing the following tasks.

* Create a new file and paste [this](https://github.com/ansible-learnfest/playbooks-example/blob/main/apache_install.yml) playbook. Save it with a `.yml` extension.
* Set **Language Mode** to Ansible either on the VSCode status bar at the bottom of the window or by putting this into `~/.local/share/code-server/User/settings.json`:

```json
"files.associations": {
        "*.yml": "ansible",
        "*.yaml": "ansible"
    },
```

You'll see ansible-lint complaining about some parts of the Playbook in the Problems pane. You can open the Problems pane by clicking on View -> Problems. Your job is to fix these issues using the VS Code Ansible extension features.

* tab completion
* syntax highlighting
* hover over a red issue line to see the complaint
* tool tips for modules: hover over a module FQCN, `Ctrl+Click` on a module FQCN to open its documentation
* `Ctrl+Space` for module parameters, e.g. add a task calling `ansible.builtin.yum` and use `Ctrl+Space` to see all module attributes and attribute parameters
* Make syntax error and notice errors in the Problem pane (`Ctrl+Shift+m`): you might have to make sure `ansible-lint` is enabled and notice it is only checking when you save a file

### Goals

Get familiar with the Ansible VS Code extension features.

### Tips

Although VSCode of course runs on Linux, Mac and Windows, there are some extra tasks to be done to get Ansible working on Windows or Mac. It's beyond the scope of this manual to provide detailed instructions on how to setup Podman and Ansible on Windows or Mac.

### Challenge Tasks

Try to configure VSCode to use Ansible inside an execution environment. The EE supported is already installed on the bastion host and can be used for these experiments.
