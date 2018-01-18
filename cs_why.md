---

copyright:
  years: 2014, 2018
lastupdated: "2018-01-11"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}

# Why {{site.data.keyword.containerlong_notm}}
{: #cs_ov}

{{site.data.keyword.containershort}} delivers powerful tools by combining Docker and Kubernetes technologies, an intuitive user experience, and built-in security and isolation to automate the deployment, operation, scaling, and monitoring of containerized apps in a cluster of compute hosts.
{:shortdesc}

## Benefits of using clusters
{: #benefits}

Clusters are deployed on compute hosts that provide native Kubernetes and {{site.data.keyword.IBM_notm}}-added capabilities.
{:shortdesc}

|Benefit|Description|
|-------|-----------|
|Single-tenant Kubernetes clusters with compute, network, and storage infrastructure isolation|<ul><li>Create your own customized infrastructure that meets the requirements of your organization.</li><li>Provision a dedicated and secured Kubernetes master, worker nodes, virtual networks, and storage by using the resources provided by IBM Cloud infrastructure (SoftLayer).</li><li>Store persistent data, share data between Kubernetes pods, and restore data when needed with the integrated and secure volume service.</li><li>Fully managed Kubernetes master that is continuously monitored and updated by {{site.data.keyword.IBM_notm}} to keep your cluster available.</li><li>Benefit from full support for all native Kubernetes APIs.</li></ul>|
|Image security compliance with Vulnerability Advisor|<ul><li>Set up your own secured Docker private image registry where images are stored and shared by all users in the organization.</li><li>Benefit from automatic scanning of images in your private {{site.data.keyword.Bluemix_notm}} registry.</li><li>Review recommendations specific to the operating system used in the image to fix potential vulnerabilities.</li></ul>|
|Automatic scaling of apps|<ul><li>Define custom policies to scale up and scale down apps based on CPU and memory consumption.</li></ul>|
|Continuous monitoring of the cluster health|<ul><li>Use the cluster dashboard to quickly see and manage the health of your cluster, worker nodes, and container deployments.</li><li>Find detailed consumption metrics by using {{site.data.keyword.monitoringlong}} and quickly expand your cluster to meet work loads.</li><li>Review logging information by using {{site.data.keyword.loganalysislong}} to see detailed cluster activities.</li></ul>|
|Automatic recovery of unhealthy containers|<ul><li>Continuous health checks on containers that are deployed on a worker node.</li><li>Automatic re-creation of containers if failures occur.</li></ul>|
|Service discovery and service management|<ul><li>Centrally register app services to make them available to other apps in your cluster without exposing them publicly.</li><li>Discover registered services without keeping track of IP addresses or container IDs and benefit from automatic routing to available instances.</li></ul>|
|Secure exposure of services to the public|<ul><li>Private overlay networks with full load balancer and Ingress support to make your apps publicly available and balance workloads across multiple worker nodes without keeping track of IP addresses inside your cluster.</li><li>Choose between a public IP address, an {{site.data.keyword.IBM_notm}} provided route, or your own custom domain to access services in your cluster from the internet.</li></ul>|
|{{site.data.keyword.Bluemix_notm}} service integration|<ul><li>Add extra capabilities to your app through the integration of {{site.data.keyword.Bluemix_notm}} services, such as Watson APIs, Blockchain, data services, or Internet of Things, and help cluster users to simplify the app development and container management process.</li></ul>|

<br />


## Comparison of lite and standard clusters
{: #cluster_types}

You can create lite or standard clusters. Try out lite clusters to get familiar and test a few Kubernetes capabilities, or create standard clusters to use the full capabilities of Kubernetes to deploy apps.
{:shortdesc}

|Characteristics|Lite clusters|Standard clusters|
|---------------|-------------|-----------------|
|[Available in {{site.data.keyword.Bluemix_notm}}](cs_why.html)|<img src="images/confirm.svg" width="32" alt="Feature available" style="width:32px;" />|<img src="images/confirm.svg" width="32" alt="Feature available" style="width:32px;" />|
|[In-cluster networking](cs_secure.html#in_cluster_network)|<img src="images/confirm.svg" width="32" alt="Feature available" style="width:32px;" />|<img src="images/confirm.svg" width="32" alt="Feature available" style="width:32px;" />|
|[Public network app access by a NodePort service](cs_network_planning.html#nodeport)|<img src="images/confirm.svg" width="32" alt="Feature available" style="width:32px;" />|<img src="images/confirm.svg" width="32" alt="Feature available" style="width:32px;" />|
|[User access management](cs_users.html#managing)|<img src="images/confirm.svg" width="32" alt="Feature available" style="width:32px;" />|<img src="images/confirm.svg" width="32" alt="Feature available" style="width:32px;" />|
|[{{site.data.keyword.Bluemix_notm}} service access from the cluster and apps](cs_integrations.html#adding_cluster)|<img src="images/confirm.svg" width="32" alt="Feature available" style="width:32px;" />|<img src="images/confirm.svg" width="32" alt="Feature available" style="width:32px;" />|
|[Disk space on worker node for storage](cs_storage.html#planning)|<img src="images/confirm.svg" width="32" alt="Feature available" style="width:32px;" />|<img src="images/confirm.svg" width="32" alt="Feature available" style="width:32px;" />|
|[Persistent NFS file-based storage with volumes](cs_storage.html#planning)| |<img src="images/confirm.svg" width="32" alt="Feature available" style="width:32px;" />|
|[Public or private network app access by a load balancer service](cs_network_planning.html#loadbalancer)| |<img src="images/confirm.svg" width="32" alt="Feature available" style="width:32px;" />|
|[Public network app access by an Ingress service](cs_network_planning.html#ingress)| |<img src="images/confirm.svg" width="32" alt="Feature available" style="width:32px;" />|
|[Portable public IP addresses](cs_subnets.html#manage)| |<img src="images/confirm.svg" width="32" alt="Feature available" style="width:32px;" />|
|[Logging and monitoring](cs_health.html#logging)| |<img src="images/confirm.svg" width="32" alt="Feature available" style="width:32px;" />|
|[Available in {{site.data.keyword.Bluemix_dedicated_notm}}](cs_dedicated.html#dedicated_environment)| |<img src="images/confirm.svg" width="32" alt="Feature available" style="width:32px;" />|

<br />



## Cluster management responsibilities
{: #responsibilities}

Review the responsibilities that you share with IBM to manage your clusters.
{:shortdesc}

**IBM is responsible for:**

- Deploying the master, worker nodes, and management components within the cluster, such as Ingress controller, at cluster creation time
- Managing the updates, monitoring, and recovery of the Kubernetes master for the cluster
- Monitoring the health of the worker nodes and providing automation for the update and recovery of those worker nodes
- Performing automation tasks against your infrastructure account, including adding worker nodes, removing worker nodes, and creating a default subnet
- Managing, updating, and recovering operational components within the cluster, such as the Ingress controller and the storage plug-in
- Provisioning of storage volumes when requested by persistent volume claims
- Providing security settings on all worker nodes

</br>
**You are responsible for:**

- [Deploying and managing Kubernetes resources, such as pods, services, and deployments, within the cluster](cs_app.html#app_cli)
- [Leveraging the capabilities of the service and Kubernetes to ensure high availability of apps](cs_app.html#highly_available_apps)
- [Adding or removing capacity by using the CLI to add or remove worker nodes](cs_cli_reference.html#cs_worker_add)
- [Creating public and private VLANs in IBM Cloud infrastructure (SoftLayer) for network isolation of your cluster](/docs/infrastructure/vlans/getting-started.html#getting-started-with-vlans)
- [Ensuring that all worker nodes have network connectivity to the Kubernetes master URL](cs_firewall.html#firewall) <p>**Note**: If a worker node has both public and private VLANs, then network connectivity is configured. If the worker node has a private VLAN only set-up, then a Vyatta is required to provide network connectivity.</p>
- [Updating the master kube-apiserver and worker nodes when Kubernetes major or minor version updates are available](cs_cluster_update.html#master)
- [Taking action to recover troubled worker nodes by running `kubectl` commands, such as `cordon` or `drain`, and by running `bx cs` commands, such as `reboot`, `reload`, or `delete`](cs_cli_reference.html#cs_worker_reboot)
- [Adding or removing subnets in IBM Cloud infrastructure (SoftLayer) as needed](cs_subnets.html#subnets)
- [Backing up and restoring data in persistent storage in IBM Cloud infrastructure (SoftLayer) ![External link icon](../icons/launch-glyph.svg "External link icon")](../services/RegistryImages/ibm-backup-restore/index.html)

<br />


## Abuse of containers
{: #terms}

Clients cannot misuse {{site.data.keyword.containershort_notm}}.
{:shortdesc}

Misuse includes:

*   Any illegal activity
*   Distribution or execution of malware
*   Harming {{site.data.keyword.containershort_notm}} or interfering with anyone's use of {{site.data.keyword.containershort_notm}}
*   Harming or interfering with anyone's use of any other service or system
*   Unauthorized access to any service or system
*   Unauthorized modification of any service or system
*   Violation of the rights of others

See [Cloud Services terms](/docs/navigation/notices.html#terms) for overall terms of use.
