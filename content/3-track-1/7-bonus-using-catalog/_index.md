+++
title = "Bonus Track: Automation Catalog"
weight = 50
+++

## Bonus Track: Using Automation Services Catalog

With a full-stack AAP 2.2 setup running along, you could start playing around with the brand new **Automation Services Catalog**. This is the new self-service portal replacing the hosted offering (and somehow one functionality of dear, old CloudForms).

Catalog depends on RH SSO for Authentication, for PAH and Controller this is optional.

{{% notice warning %}}
Remember it's early days; Catalog is in Tech Preview!
{{% /notice %}}


**Prerequisites**

* A full AAP 2.2 installation (Controller, PAH, Catalog, RH SSO)
* Access to all web UIs

**Tasks**

* Start with Automation Services Catalog 
  * Log in with the new user
  * Check under **Platforms** **Automation Controller** status is `Available`, click the circular `Refresh` icon.
  * Create a new **Portfolio** and access it
  * To add a create a new **Product**, click **Add** and set **Filter by Platform** to `Automation Controller`
  * Check the `Demo Job Template` tile and click **Add** 
  * Now you can order the Product, try it and check the Job is run in Controller.

**Play from here on**

* If you want to use PAH within an SSO-enabled setup, it now needs an SSO user, too. Create one or give the `hubadmin` role to your `catalog-user`.
* Set an approval for a **Product**, you need a user with SSO approver role.
* Create a **Template** with a **Survey** in Controller and use this in a new **Product** in Catalog.

**Goal**

* See what Catalog can do right now... :-)

**Tips**

