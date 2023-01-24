+++
title = "Using Private Automation Hub"
weight = 30
+++

## Using Private Automation Hub

### Prerequisites

* Make sure you can log into to your:

  * Automation Controller
  * Private Automation Hub (PAH)

* Using the access information/credentials provided.

### Tasks

* Configure Automation Controller to access your Private Automation Hub
* Add collections from Red Hat Automation Hub and Galaxy to PAH
* Use a collection from PAH in Automation Controller

Let's start, as the docs for this are distributed over some places we'll give some more instructions.

#### Integrate your PAH in Automation Controller

In this lab your automation controller is already configured to fetch collections from the private automation hub. We want to additionally also disable direct retrieval of collections from Ansible Galaxy.

* Open the `default` **Organization** in Automation Controller and click **Edit**
* Have a look at the **Galaxy Credentials** field. There is only one credential pointing to Ansible Galaxy. This is the default.
* Make sure the credentials for your private automation hub are enabled, and **disable** the "Ansible Galaxy" credential.
* Click **Save**

{{% notice note %}}
You could add more credentials here, the order of these credentials sets precedence for the sync and lookup of the content.
{{% /notice %}}

### Setup demo project

Before we configure content synchronization, we want to add a demo project:

* Create a new **Project** pointing here: `https://github.com/ansible-learnfest/ee-flow.git`

* Have a look at the content on GitHub, esp the `collections/requirements.yml` file

You will notice this project will fail to sync (Click on the **Jobs** menu on the left) with an error "ERROR! Failed to resolve the requested dependencies map. Could not satisfy the following requirements: containers.podman". This is because the `requirements.yml` file lists a dependency for a collection which is not yet available on your controller.

To solve this issue, we have to configure your private automation hub to sync the necessary sources, and to configure your automation controller to use the content from your private automation hub. We did already configure automation controller, but we haven't synced any content yet.

### Add and use content in your Private Automation Hub

Most of this is well documented [here](https://access.redhat.com/documentation/en-us/red_hat_ansible_automation_platform/2.3/html-single/managing_red_hat_ansible_content_collections_and_ansible_galaxy_collections_in_automation_hub/index)

#### Sync collections from Red Hat Automation Hub to PAH

* Go to `console.redhat.com` and open **Ansible Automation Platform** -> **Automation Hub** -> **Collections**. Here you can enable/disable the sync of certain collections. You will need a RHN account with "organization administrator" privileges to perform this action.

* What you need to do is to get the authentication token and configure it in your PAH:
  * In Red Hat Automation Hub go to **Connect to Hub** we will need the **Offline Token** and the **Server URL**
  * In your private automation hub go to **Collections** -> **Repository management** -> **Remote**
  * Edit the `rh-certified` remote:
    * **URL** paste and override the **Server URL** from the Red Hat Automation Hub
    * **Token** the token you copied from RH AH
    * Click **Save** and then hit **Sync**. This will sync all enabled and updated collections from Red Hat Automation Hub to your Private Automation Hub.
* After the sync process has finished, check the certified collections are visible on PAH.

#### Sync selected community collections from Ansible Galaxy to PAH

Galaxy is configured as the remote `community` out of the box. Follow the instructions to configure the synchronization.

* Create a regular requirements.yml file pointing to the `containers.podman` collection:

```yaml
collections:
  # Install a collection from Ansible Galaxy.
  - name: containers.podman
```

* Go to Repo Management, click the **Remote** tab again
* Edit the `community` remote
* In **YAML requirements** upload the  `requirements.yml` file from your local machine.
* Click **Save**
* In the **Remote** overview tab click **Sync** for the `community` remote

Verify the sync of the collections in **Collections** -> **Collections**, switch the repository filter with the dropdown at the top. There should be a lot of content in the `Red Hat Certified` repo and one collection in the `Community` repo. The 'published' filter will not find anything, since we haven't uploaded any collections we created ourselves.

### Test Private Automation Hub Integration

Now check that Automation Controller can actually use the content from your PAH. Let's first sync our project again and the error message should disappear.

Before we can test our Playbook, we have to create an inventory. Create an inventory called **Workshop Inventory** and populate it with the servers listed in `/etc/ansible/hosts` in the "managed_nodes" section:

```bash
$ cat /etc/ansible/hosts
[output truncated]

[managed_nodes]
node1.<LABID>.internal
node2.<LABID>.internal
node3.<LABID>.internal
```

We also have to create machine credentials for these servers. Let's create machine credentials called **Workshop Credentials**. You can use your private key and username "ec2-user":

```bash
cat .ssh/<LABID>key.pem
```

For a proper end to end test, let's create a **Job Template** that requires a collection that is not part of the included Execution Environments:

* Sync the project you created earlier again and check it runs successfully. You should notice that the task which install collection from the `requirements.yml` is succeeding.

* Create a new **Job Template**:
  * **Name**: up to you
  * **Inventory**: The `Workshop Inventory`
  * **Project**: The one you just created
  * **Execution Environment**: `Ansible Engine 2.9 execution environment`
  * **Playbook**: `deploy-container.yml`
  * Set the right **Credentials** : `Workshop Credential`
  * Check **Privilege Escalation**

* Launch the **Template**

It should now run and deploy an httpd container that is hosting a small website. Test it from the terminal in VS Code Server:

```bash
curl node1
```

So recap what happened:

* You created a **Template** that runs a Playbook that has a requirement on a certain Collection which is **not part of the Execution Environments included in Controller**.
* Your **Organization** (`default`) is configured in a way it can only download Collections from your Private Automation Hub
* The Collection did exist on your PAH

{{% notice note %}}
As this collection is not part of the Execution Environment the Playbook uses, how did it work?
In this case is it was dynamically "added" to the Execution Environment at runtime. This behavior did already exist in Ansible Tower 3.8, and it still does work in automation controller. This means, you only have to build your own execution environment if your collection has additional Python or package dependencies. You can double check by looking at the details of the "source control update" job of your project and click on the "fetch galaxy collections from collections/requirements" task.
{{% /notice %}}

### Goals

Be able to manually configure private automation hub to synchronize content from Red Hat's automation hub and Ansible Galaxy.

You should also better understand that, although it might be beneficial to create custom execution environments, it's not always necessary and automation controller can still load and install collections during runtime.

### Tips

* Check the list of [certified collections](https://access.redhat.com/articles/3642632) in the Red Hat Customer Portal
