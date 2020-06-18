# vSphere


## Product overview
### High-Level information

vSphere is a VMware product dedicated to managing an (usually) on-premise infrastructure. From physical machines running [VMware ESXi](https://en.wikipedia.org/wiki/VMware_ESXi) that are called ESXi Hosts, users can spin up or migrate Virtual Machines from one host to another.

vSphere is an integrated solution and provides an easy managing interface over concepts like data storage, or computing resource. 

### Terminology
This section details some of vSphere-specific elements. This section does not intend to be an extensive list, but rather a place for those unfamiliar with the product to have the basics required to understand how the Datadog integration works.

- vSphere - The complete suite of tools and technologies detailed in this article.
- vCenter server - The main machine which controls ESXi hosts and provides both a web UI and an API to control the vSphere environment.
- vCSA (vCenter Server Appliance) - A specific kind of vCenter where the software runs in a dedicated Linux machine (more recent). By opposition, the legacy vCenter is installed on an existing Windows Machine.
- ESXi host - The physical machine controlled by vCenter where the ESXi (bare-metal) virtualizer is installed. The host boots a minimal OS that is able to run Virtual Machines.
- VM - What anyone using vSphere actually needs in the end, instances that can run applications and code. Note: Datadog monitors both ESXi hosts and VMs and it calls them both "host" (they are in the host map).
- Attributes/tags - It is possible to add attributes and tags to any vSphere ressource, note that those two are now very similar with "attributes" being the deprecated thing to use.
- Datacenter - A set of resources grouped together. A single vCenter server can handle multiple datacenter.
- Datastore - A virtual vSphere concept to represent data storing capabilities. It can be a NFS server that ESXi hosts have read/write access to, it can be a mounted disk on the host and more. Datastores are often shared between multiple hosts. This allows Virtual Machines to be migrated from one host to another.
- Cluster - A logical grouping of computational resources, you can add multiple ESXi hosts in your cluster and then you can create VM in the cluster (and not on a specific host, vSphere will take care of placing your VM in one of the ESXi host and migrating it when needed).
- Photon OS - An open-source mimimal Linux distribution and used by both ESXi and vCSA as a base.

## The integration

### Overall logic
The Datadog vSphere integration runs from a single agent and pulls all the information from a single vCenter endpoint. Because the agent cannot run directly on Photon OS, it is usually require that the agent runs within a dedicated VM inside the vSphere infrastructure.
-----
