+++
title = "Extend Automation Mesh"
weight = 35
+++

## Task 5: Extend Automation Mesh

Even a single-node Automation Controller installation contains an Automation Mesh, in this case it's living on the Controller node itself. In this case the node type is called "hybrid", although you can install node types "control" but then you need execution nodes again. 

Go and check this on your Controller under **Topology View**. This section is about extending the Automation Mesh with a second Execution Node.

**Prerequisites**
* Automation Controller is running & accessible
* You have run example Ansible content against an managed node

**Tasks**
### Install Execution Node
* You need to enable the AAP installer on your Controller node to connect to one of your AWS instances via SSH. This means either 
  * to copy the private key you are using to connect to your instances to the Controller instance 
  * or to create a new SSH key on the Controller and copy the public key to the `authorized_keys` file on the machine you want to use as Execution Node.
* For the fun of it you need to do a small AWS exercise: Open port `27199` on your Controller and your Execution Node by adding it to their respective **Security Groups** in the AWS Console
  * This is the `receptor` port Automation Mesh is communicating on and not open on the default AWS instance deployments.
* To install the Execution Node, edit the inventory file on your Controller by adding these lines:

```
[automationcontroller:vars]
peers=execution_nodes

[execution_nodes]
<execution node> ansible_user=ec2-user ansible_become=yes
```
* Run the installer, wait for successful completion

### Use the Execution Node

Right now you have execution nodes, one is the Controller as a hybrid node, the other is the new Execution node. Now use the node by configuring the Controller to execute the jobs for your managed node on the new Execution node.

* Add a new **Instance Group**
* Add the Execution Node to the **Instance Node**
* You can configure on **Organization, Inventory** and **Template** level to use a specific Execution Node. Do this for the **Template** installing PHP or the **Inventory** with the managed node. Or play around, it's a LearnFest after all.
* Launch the **Template** and make sure it run on the Execution Node.

**Goal**
* The installer installs the execution node without errors
* The new node appears in the Controller web UI under **Topology View**, you should now have one Hybrid Node and one Execution Node. 
* You can launch a **Template** using the Execution Node to run it.

**Tips**
* The Automation Mesh documentation is [here](https://access.redhat.com/documentation/en-us/red_hat_ansible_automation_platform/2.1/html-single/red_hat_ansible_automation_platform_automation_mesh_guide/)
