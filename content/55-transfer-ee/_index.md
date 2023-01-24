+++
title = "Transfer Execution Environment"
weight = 55
+++

## Transfer your Execution Environment to Private Automation Hub

So by now you have seen how to use a collection that is not included in one of the official EE's and in the next step how to build a custom EE with the required collection and testing it.

The next step is to get your custom EE into a container registry so it can be used in Automation Controller. An since PAH is a container registry, too, we'll use it.
### Prerequisites

* The (working) custom execution environment from the previous chapter
* Access to your Private Automation Hub to store your EE

### Tasks

* Push your EE to your Private Automation Hub
* Add additional tags or labels and push them to the registry
* Delete old versions of the EE

### Push image to PAH

Pusing an EE to the PAH registry is pretty straight forward: You just use Podman like with any other registry. The parameters you need are:

* The URL, this is the same as the PAH web UI
* Username/password, again same as for the web UI
* Disable certificate verification becasue our PAH uses a self-signed certificate

The just do the following:

* Find the name of the local image
* Tag the local image with the registry
* Push it

```bash
podman login <PAH URL> --tls-verify=false --username admin --password <YOURPASSWORD>
podman images
podman tag localhost/ee-ansible-demo:0.1.0  <PAH URL>/ee-ansible-demo:latest
podman push localhost/ee-ansible-demo:0.1.0 <PAH URL>/ee-ansible-demo --tls-verify=false
```

* Check the image is in PAH: Log into the Web UI of your automation hub and you will find the execution environment in the **Execution Environments** menu.

### Goals

* Publish an EE in your own registry
* Get familiar with basic EE management tasks

### Tips

* [Skopeo documentation](https://github.com/containers/skopeo)
* [Podman documentation](https://docs.podman.io/en/latest/)
