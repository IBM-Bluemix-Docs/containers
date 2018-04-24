---

copyright:
  years: 2014, 2018
lastupdated: "2018-4-24"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}



# Your responsibilities by using {{site.data.keyword.containerlong_notm}}
Learn about cluster management responsibilities and terms and conditions that you have when you use {{site.data.keyword.containerlong}}.
{:shortdesc}

## Cluster management responsibilities
{: #responsibilities}

Review the responsibilities that you share with IBM to manage your clusters.
{:shortdesc}

**IBM is responsible for:**

- Deploying the master, worker nodes, and management components within the cluster, such as Ingress application load balancer, at cluster creation time
- Managing the security updates, monitoring, and recovery of the Kubernetes master for the cluster
- Monitoring the health of the worker nodes and providing automation for the update and recovery of those worker nodes
- Performing automation tasks against your infrastructure account, including adding worker nodes, removing worker nodes, and creating a default subnet
- Managing, updating, and recovering operational components within the cluster, such as the Ingress application load balancer and the storage plug-in
- Provisioning of storage volumes when requested by persistent volume claims
- Providing security settings on all worker nodes

</br>
**You are responsible for:**

- [Deploying and managing Kubernetes resources, such as pods, services, and deployments, within the cluster](cs_app.html#app_cli)
- [Leveraging the capabilities of the service and Kubernetes to ensure high availability of apps](cs_app.html#highly_available_apps)
- [Adding or removing capacity by using the CLI to add or remove worker nodes](cs_cli_reference.html#cs_worker_add)
- [Creating public and private VLANs in IBM Cloud infrastructure (SoftLayer) for network isolation of your cluster](/docs/infrastructure/vlans/getting-started.html#getting-started-with-vlans)
- [Ensuring that all worker nodes have network connectivity to the Kubernetes master URL](cs_firewall.html#firewall) <p>**Note**: If a worker node has both public and private VLANs, then network connectivity is configured. If worker nodes are set up with a private VLAN only, you must configure an alternative solution for network connectivity. For more information, see [VLAN connection for worker nodes](cs_clusters.html#worker_vlan_connection). </p>
- [Updating the master kube-apiserver when Kubernetes version updates are available](cs_cluster_update.html#master)
- [Keeping the worker nodes up-to-date by using the `bx cs worker-update` command to apply operating system updates, security patches, and Kubernetes version updates](cs_cluster_update.html#worker_node)
- [Recovering troubled worker nodes by running `kubectl` commands, such as `cordon` or `drain`, and by running `bx cs` commands, such as `reboot`, `reload`, or `delete`](cs_cli_reference.html#cs_worker_reboot)
- [Adding or removing subnets in IBM Cloud infrastructure (SoftLayer) as needed](cs_subnets.html#subnets)
- [Backing up and restoring data in persistent storage in IBM Cloud infrastructure (SoftLayer) ![External link icon](../icons/launch-glyph.svg "External link icon")](../services/RegistryImages/ibm-backup-restore/index.html)
- [Configuring health monitoring for worker nodes with Autorecovery](cs_health.html#autorecovery)

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


See [Cloud Services terms](https://console.bluemix.net/docs/overview/terms-of-use/notices.html#terms) for overall terms of use.

