+++
title = "Transfer Execution Environment"
weight = 55
+++

## Transfer your Execution Environment to private automation hub

By now you have learned how to use a collection that is not included in one of the official EE's and in the next step how to build a custom EE with the required collection and testing it.

The next step is to get your custom EE into a container registry so it can be used in automation controller. And since PAH provides a container registry, too, we'll use it.

### Prerequisites

* The (working) custom execution environment from the previous chapter
* Access to your private automation hub to store your EE

### Tasks

* Push your EE to your private automation hub
* Add additional tags or labels and push them to the registry
* Delete old versions of the EE

### Push image to PAH

Pushing an EE to the PAH registry is pretty straight forward: You just use Podman like with any other registry. The parameters you need are:

* The registry, this is the same as the hostname of your private automation hub
* Username/password, again same as for the web UI

Then just do the following:

* Find the name of the local image
* Tag the local image with the registry
* Push it

```bash
podman login <PAH hostname>
podman images
podman tag localhost/ee-ansible-demo:0.1.0  <PAH hostname>/ee-ansible-demo:latest
podman push <PAH hostname>/ee-ansible-demo
```

* Check the image is in PAH: Log into the Web UI of your automation hub and you should find the execution environment in the **Execution Environments** menu.

### Goals

* Publish an EE in your own registry
* Get familiar with basic EE management tasks

### Tips

* [Skopeo documentation](https://github.com/containers/skopeo)
* [Podman documentation](https://docs.podman.io/en/latest/)
