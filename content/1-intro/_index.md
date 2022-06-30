+++
title = "Introduction"
weight = 1
+++

## Introduction

The LearnFest is structured into two tracks, so this is coming your way:

### Track 1 “Ansible Automation Platform Administrator”

**General Prerequisites:**
* Access to PC with Ansible (esp `ansible-navigator`)
* Credentials/tokens to download the AAP2 installer and access the Red Hat Container registry and Red Hat Automation Hub
  * This usually means an account to the Red Hat Customer Portal 
* Optional: Write access to a GitLab or GitHub repository

**Tasks:**
* Install Automation Controller 
* Write a Playbook using the `ansible.controller` collection to configure Automation Controller
* Install Private Automation Hub, sync with Red Hat Automation Hub
* Extend Automation Mesh
* Optional: Extending to multi node/cluster (Needs separate DB)


### Track 2 “Automation Content Developer”
**General Prerequisites:**
* Automation Controller and Private Automation Hub is up and running
* Access to **ansible-builder** is installed on RHEL 8 VM/instance

**Tasks:**
* Install and configure VSCode extension, learn how to use it
* Build a custom Execution Environment (EE)
* Use EE with `ansible-navigator`
* Push the EE to Private Automation Hub
* Use the EE in Automation Controller
* Change the EE
* Discuss version pinning, requirements management etc.

<!-- ## Track 3 “Advanced for Gurus”
* Automation Services Catalog
* SSO -->

**Let's go!**

