+++
title = "AAP 2 Full Stack Installation"
weight = 40
+++

## Task 6: Start over with Full-Stack Installation

Until now you installed and configured an AAP 2.2 environment component by component. In real life you would have the installer deploy all components in one go, so you would get an environment with integrated components consisting of:

* Automation Controller
* Private Automation Hub
* Automation Services Catalog (Tech Preview)
* Red Hat SSO for central authentication (required for Catalog, optional for other components)

So in this section you'll build the whole thing.

**Prerequisites**
* Delete your AWS environment

`ansible-navigator run aws-infra.yml -m stdout -e remove=true`

* and start over with new instances:

`ansible-navigator run aws-infra.yml -m stdout`

* Open the instances security groups so we don't have to tweak filter rules for now. **Disclaimer: Only for lab use, not in real life!**
* In the AWS console for each of your instances
  * Go to **Instance Details -> Security -> Click "Security Group"**
  * Add an inbound rule **All traffic** to **0.0.0.0/0**
  * Save

**Tasks**

* SSH to instance 1, **as root** create a new SSH key pair (`ssh-keygen`)and add the new public key to the `authorized_keys` file of **ec2-user** on the other three instances.
  * The AAP installer on instance 1 needs to SSH into the other instances!
* Download/unpack the **Ansible Automation Platform 2.2.0 Setup Bundle** on instance 1 again.
* Register the SSO host (instance 4) and enable Java repo (yeah, RH SSO is Java-based!)
  * `subscription-manager register`
  * `subscription-manager repos --enable=jb-eap-7.3-for-rhel-8-x86_64-rpms`
* Create an inventory file to configure Installer:
  * Use the template below, fill in missing values
* The inventory should:
  * install Controller on instance 1
  * install the other three AAP components from instance 1 on the other instances: **instance 2 - PAH**, **instance 3 - Catalog**, **instance 4 - SSO**

{{% notice tip %}}
Ansible basics: Disable host key checking, either by running `export ANSIBLE_HOST_KEY_CHECKING=False; ./setup.sh` or by putting
```
[defaults]
host_key_checking = False
```
into ./ansible.cfg
{{% /notice %}}

* Run the installer
* Get a coffee. Or two.

**Goal**
* Access to the web UIs on all four instances:
  * Automation Controller
  * Private Automation Hub
  * Automation Services Catalog
  * RH SSO Admin UI (Port 8443)

**Tips**

Example inventory file:

```
[automationcontroller]
<instance-1> ansible_connection=local

[automationcontroller:vars]
peers=execution_nodes

[execution_nodes]

[automationhub]
<instance-2> ansible_user=ec2-user ansible_become=true

[automationcatalog]
<instance-3> ansible_user=ec2-user ansible_become=true

[database]

[sso]
<instance-4> ansible_user=ec2-user ansible_become=true

[all:vars]
admin_password='<password>'
pg_host=''
pg_port=5432
pg_database='awx'
pg_username='awx'
pg_password='<password>'
pg_sslmode='prefer'  # set to 'verify-full' for client-side enforced SSL

registry_url='registry.redhat.io'
registry_username='<registry user>'
registry_password='<registry token or password'

receptor_listener_port=27199

automationhub_admin_password='<password>'
automationhub_pg_host='instance-1'
automationhub_pg_port=5432
automationhub_pg_database='automationhub'
automationhub_pg_username='automationhub'
automationhub_pg_password='<password>'
automationhub_pg_sslmode='prefer'

automationcatalog_pg_host='instance-1'
automationcatalog_pg_port=5432
automationcatalog_pg_database='automationservicescatalog'
automationcatalog_pg_username='automationservicescatalog'
automationcatalog_pg_password='<password>'
automationcatalog_controller_verify_ssl=False

sso_keystore_password='<password>'
sso_console_admin_password='<password>'
```
