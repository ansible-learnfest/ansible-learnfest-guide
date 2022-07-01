+++
title = "Using Private Automation Hub"
weight = 30
+++

## Task 4: Using Private Automation Hub

**Prerequisites**
* Automation Controller is running & accessible
* Private Automation Hub is running & accessible

**Tasks**
* Configure Automation Controller to access your Private Automation Hub
* Add collections from Red Hat Automation Hub, Galaxy and custom ones created by you.
* Add Excution Environment images

{{% notice tip %}}
Important: You only have to do this because we didn't have installer install Controller and PAH in one go. Then the integration would have been configured for you already. For the sake of understanding we decided to let you do this manually.
{{% /notice %}}

Let's start, as the docs for this are distributed over some places we'll give some more instructions.

### Integrate PAH into your Controller

* In you PAH go to **Collections->API token management**, hit **Create Token** and copy the token. Put it somewhere, the token will change every time you have to get it this way again!
* In Controller, go to **Resources->Credentials** and **Add** three new credentials:
  * Name them **PAH community**, **PAH certified** and **Pah published**
  * All belong to the **Organisation** `default`
  * Credential Type is **Ansible Galaxy/Automation Hub API Token**
  * Look up the **Galaxy Server URL** for each in PAH: In **Collections->Repository Management** lookup the **Repo URL** for **community**, **published** and **rh-certified** and put them in respectively.   
  * You have created the API token already, paste in into the credentials

### Add content to your (empty) PAH

Most of this is well documented [here](https://access.redhat.com/documentation/en-us/red_hat_ansible_automation_platform/2.1/html-single/managing_red_hat_certified_and_ansible_galaxy_collections_in_automation_hub/index)

Sync collections from Red Hat Automation Hub
* Go to `console.redhat.com` and open **Ansible Automation Platform->Automation Hub->Collections**. Here you **could enable/disable the sync of certain collections but there is a bug as of now!** We'll give you a working URL to sync all content as a workaround.
* What you need to do is to get the autentication token and configure it in your PAH: 
  * In Red Hat Automation Hub Go to **Connect to Hub** and copy the **Offline Token**
  * In PAH go to **Collections->Repository management->Remote**
  * Edit the `rh-certified` remote: 
    * **URL** `https://console.redhat.com/api/automation-hub/content/published/
    * **Token** the token you copied from RH AH
    * Click **Save** and then hit **Sync**. This will sync all collections from Red Hat Automation Hub to your Private Automation Hub.

Sync selected comunity collections from Ansible Galaxy
* Galaxy is configured as the remote `community` out of the box
* To sync collections:
  * Create a regular requirements.yml file pointing to the collection you want, we'll use the one from the docs:
```
collections:
  # Install a collection from Ansible Galaxy.
  - name: geerlingguy.php_roles
    version: 0.9.3
    source: https://galaxy.ansible.com
```   
  * Go to Repo Management, click the **Remote** tab again
  * Edit the `community` remote
  * In **YAML requirements** upload the  `requirements.yml` file from your local machine.
  * Click **Save**
  * In the **Remote** overview tab click **Sync** for the `community` remote

Verify the sync of the collections in **Collections->Collections**, switch the repositories with the dropdown at the top. There should be a lot of content in the `Red Hat Certified` repo and one collection in the `Community` repo. 



**Goal**

**Tips**
