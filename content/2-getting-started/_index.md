+++
title = "Prepare Base Environment"
weight = 5
+++

## Setup your LearnFest Base Environment

You'll run the LearnFest tasks on RHEL instances on AWS. So before starting with AAP you have to stand up the needed instances.

{{% notice tip %}}
The guide has been tested using a blank AWS environment sandbox Red Hatters can order in RHPDS. But everything should work the same in a regular AWS account with access tho the RHEL 8 marketplace AMIs. 
{{% /notice %}}

## Deploy RHEL Instances into AWS Account

So off to the first tasks: deploy the needed RHEL instances into an AWS account. And as a warm-up you'll use Ansible for this (we have prepared the Playbook for you). If this your first encounter with Execution Environments and `ansible-navigator`; just enjoy, you'll learn more about all this later.

**Prerequisites**
* A PC or VM prepared to run `ansible-navigator` and `podman`
* Make sure you have access to an AWS account, Red Hatters please deploy Service "AWS Blank Open Environment" from RHPDS
* Make sure you have a valid `Access Key ID` and a `Secret Access Key` for your AWS account, Red Hatters will find this in the confirmation email from RHPDS

**Tasks**
* On your `ansible-navigator` machine: Clone [https://github.com/ansible-learnfest/playbooks-infra.git](https://github.com/ansible-learnfest/playbooks-infra.git) and change into the new directory.
* Follow the instructions in the repo's [README.md](https://github.com/ansible-learnfest/playbooks-infra/blob/main/README.md) file to deploy the LearnFest base environment instances
  * Update the number of instances in `group_vars/all/main.yml` to `5` (`instance_total:`)
  * Change the name prefix (`instance_name:`) of the instances if you want 
  * The Playbook will use the SSH key `~/.ssh/id_rsa.pub` by default, if this key doesn't exist or you want to use another key, change `ec2_key_pair:`

{{% notice tip %}}
How did `ansible-navigator` know which execution environment to use? Have a look at the `ansible-navigator.yml` config file... 
{{% /notice %}}

**Goals**
* **5 x t3.large** instances deployed in a new AWS VPC
* Look up the **Public IPv4 DNS** FQDN of your instances in the AWS Console.
* Being able to login via SSH into the instances using the FQDN's and to become root
    * Example: `ssh -i "~/.ssh/id_rsa"  ec2-user@ec2-3-72-65-199.eu-central-1.compute.amazonaws.com`

**Tips**
* Podman login for pulling the execution environment image is your quay.io creds with encrypted password, not the RH Customer Portal creds.
    * Instructions on how to get an employee account are [on the source](https://source.redhat.com/groups/public/customer-service/for_stakeholders/for_non_cs_associates_wiki/how_to_request_a_quay_employee_subscription) (Request a “Type 1” account)
    * Verify you can access the [execution environment](https://quay.io/repository/redhat_emp1/ee-ansible-ssa)
* Using the prepared Ansible content you can tear down all instances and the VPC and start over: `ansible-navigator run aws-infra.yml -e remove=true`




