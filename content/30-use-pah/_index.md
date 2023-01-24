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

* Add collections from Red Hat Automation Hub and Galaxy to PAH
* Configure Automation Controller to access your Private Automation Hub
* Use a collection from PAH in Automation Controller

Let's start, as the docs for this are distributed over some places we'll give some more instructions.

### Add and use content in your Private Automation Hub

Most of this is well documented [here](https://access.redhat.com/documentation/en-us/red_hat_ansible_automation_platform/2.3/html-single/managing_red_hat_ansible_content_collections_and_ansible_galaxy_collections_in_automation_hub/index)

#### Sync collections from Red Hat Automation Hub to PAH

* Go to `console.redhat.com` and open **Ansible Automation Platform** -> **Automation Hub** -> **Collections**. Here you can enable/disable the sync of certain collections. You will need a RHN account with "organization administrator" privileges to perform this action.

* What you need to do is to get the authentication token and configure it in your PAH:
  * In Red Hat Automation Hub go to **Connect to Hub** we will need the **Offline Token** and the **Server URL**
  * In your private automation hub go to **Collections** -> **Repository management** -> **Remote**
  * Edit the `rh-certified` remote:
    * **URL** paste the **Server URL** from the Red Hat Automation Hub
    * **Token** the token you copied from RH AH
    * Click **Save** and then hit **Sync**. This will sync all enabled and updated collections from Red Hat Automation Hub to your Private Automation Hub.
* After the sync process has finished, check the certified collections are visible on PAH.

#### Integrate your PAH in Automation Controller

Automation Controller doesn't know about your Private Automation Hub, yet. The first step is to create a new Credential pointing to your PAH:

Get access URL and Token fromm your PAH and put them down somewhere:

* In PAH, **Collections**->**Repository management**
* Get the URLs for the **local** repos **community**, **published** and **rh-certified**
* Get the access token from **Collections**->**API Token**

Then configure three new credentials in Automation Controller for the three URLs:

* **Name**: PAH {Community,Published,RH-Certified}
* **Organization**: default
* **Credential Type**: Ansible Galaxy/Automation Hub API Token
* **Galaxy Server URL**: <the respective community repo URL>
* **API Token**: <the token>

Now the access information is configured but not used by Automation Controller. This is done on the **Organization** level.

* Open the `default` **Organization** in Automation Controller and click **Edit**
* Have a look at the **Galaxy Credentials** field. There is only one credential pointing to Ansible Galaxy. This is the default.
* Remove the credential and add the one pointing to your PAH and add the three credentials you just created
* Click **Save**

Finally, you have to disable SSL Certificate verification, since we're using self signed certificates in this lab. Navigate to **Settings** -> **Job Settings**, Edit and turn on **Ignore Ansible Galaxy SSL Certificate Verification**.

{{% notice note %}}
You could add more credentials here, the order of these credentials sets precedence for the sync and lookup of the content.
{{% /notice %}}

### Test Private Automation Hub Integration

Now check that Automation Controller can actually use the content from your PAH by creating a **Job Template** that requires a collection that is not part of the included Execution Environments:

* Create a new **Project** pointing here: `https://github.com/ansible-learnfest/ee-flow.git`
  * Have a look at the content on GitHub, esp the `collections/requirements.yml` file

* Update the existing **Workshop Inventory**:
  * remove `ansible-1` from the list of **Hosts**
  * Disable `node2` and `node3`

* Create a new **Job Template**:
  * **Name**: up to you
  * **Inventory**: The `Workshop Inventory`
  * **Project**: The one you just created
  * **Execution Environment**: `Ansible Engine 2.9 execution environment`
  * **Playbook**: `deploy-container.yml`
  * Set the right **Credentials** : `Workshop Credential`
  * Check **Privilege Escalation**

* Launch the **Template**

If you followed the steps above you should **get an error**. Why? Because You pointed Automation Controller to your PAH but the collection required in the requirements.yml is not there. Check the output of the `Source Control Update` job under **Views**->**Jobs**.

So let's get the collection on PAH.

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

**Run your Job Template again.**

* It should now run and deploy an httpd container that is hosting a small website.
* Test it from the terminal in VS Code Server:

```
$ curl node1
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
