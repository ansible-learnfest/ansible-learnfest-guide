+++
title = "Transfer Execution Environment"
weight = 55
+++

## Transfer your EE

### Prerequisites

* A working execution environment

* private automation hub to store your EE

### Tasks

* Publish your EE to your private automation hub

* Add additional tags or labels and push them to the registry

* Experiment with other registries, e.g. Quay (make sure to either delete your EE after you're done or make sure it’s not publicly accessible since the EE supported requires a subscription!)

* Delete old versions of the EE

### Push image to PAH

```bash
podman login hub-student.<LABID>.example.opentlc.com --tls-verify=false --username admin --password <YOURPASSWORD>
podman tag localhost/ee-ansible-demo:0.1.0  hub-student.<LABID>.example.opentlc.com/ee-ansible-demo:latest
podman push localhost/ee-ansible-demo:0.1.0 hub-student.<LABID>.example.opentlc.com/ee-ansible-demo --tls-verify=false
```

Log into the Web UI of your automation hub and you will find the execution environment in the **Execution Environments** menu.

### Goals

* Publish an EE in your own registry

* Get familiar with basic EE management tasks

### Tips

* [Skopeo documentation](https://github.com/containers/skopeo)

* [Podman documentation](https://docs.podman.io/en/latest/)
