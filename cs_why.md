---

copyright:
  years: 2014, 2018
lastupdated: "2018-04-11"

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

{{site.data.keyword.containerlong}} delivers powerful tools by combining Docker and Kubernetes technologies, an intuitive user experience, and built-in security and isolation to automate the deployment, operation, scaling, and monitoring of containerized apps in a cluster of compute hosts.
{:shortdesc}

## Benefits of using the service
{: #benefits}

Clusters are deployed on compute hosts that provide native Kubernetes and {{site.data.keyword.IBM_notm}}-specific capabilities.
{:shortdesc}

|Benefit|Description|
|-------|-----------|
|Single-tenant Kubernetes clusters with compute, network, and storage infrastructure isolation|<ul><li>Create your own customized infrastructure that meets the requirements of your organization.</li><li>Provision a dedicated and secured Kubernetes master, worker nodes, virtual networks, and storage by using the resources provided by IBM Cloud infrastructure (SoftLayer).</li><li>Fully managed Kubernetes master that is continuously monitored and updated by {{site.data.keyword.IBM_notm}} to keep your cluster available.</li><li>Option to provision worker nodes as bare metal servers with Trusted Compute.</li><li>Store persistent data, share data between Kubernetes pods, and restore data when needed with the integrated and secure volume service.</li><li>Benefit from full support for all native Kubernetes APIs.</li></ul>|
|Image security compliance with Vulnerability Advisor|<ul><li>Set up your own secured Docker private image registry where images are stored and shared by all users in the organization.</li><li>Benefit from automatic scanning of images in your private {{site.data.keyword.Bluemix_notm}} registry.</li><li>Review recommendations specific to the operating system used in the image to fix potential vulnerabilities.</li></ul>|
|Continuous monitoring of the cluster health|<ul><li>Use the cluster dashboard to quickly see and manage the health of your cluster, worker nodes, and container deployments.</li><li>Find detailed consumption metrics by using {{site.data.keyword.monitoringlong}} and quickly expand your cluster to meet work loads.</li><li>Review logging information by using {{site.data.keyword.loganalysislong}} to see detailed cluster activities.</li></ul>|
|Secure exposure of apps to the public|<ul><li>Choose between a public IP address, an {{site.data.keyword.IBM_notm}} provided route, or your own custom domain to access services in your cluster from the internet.</li></ul>|
|{{site.data.keyword.Bluemix_notm}} service integration|<ul><li>Add extra capabilities to your app through the integration of {{site.data.keyword.Bluemix_notm}} services, such as Watson APIs, Blockchain, data services, or Internet of Things.</li></ul>|



<br />


## Comparison of running the service in {{site.data.keyword.Bluemix_notm}} Public, Dedicated, Private, and a hybrid setup
{: #differentiation}

You can run {{site.data.keyword.containershort_notm}} in {{site.data.keyword.Bluemix_notm}} Public or Dedicated, in {{site.data.keyword.Bluemix_notm}} Private, or in a hybrid setup.
{:shortdesc}

Review the following information for the differences between these {{site.data.keyword.containershort_notm}} setups.

<table>
<col width="22%">
<col width="78%">
 <thead>
 <th>{{site.data.keyword.containershort_notm}} setup</th>
 <th>Description</th>
 </thead>
 <tbody>
 <tr>
 <td>{{site.data.keyword.Bluemix_notm}} Public
 </td>
 <td>With {{site.data.keyword.Bluemix_notm}} Public on [shared or dedicated hardware or bare metal machines](cs_clusters.html#shared_dedicated_node), you can host your apps in clusters on the cloud by using {{site.data.keyword.containershort_notm}}. {{site.data.keyword.containershort_notm}} on {{site.data.keyword.Bluemix_notm}} Public delivers powerful tools by combining Docker and Kubernetes technologies, an intuitive user experience, and built-in security and isolation to automate the deployment, operation, scaling, and monitoring of containerized apps in a cluster of compute hosts.<br><br>For more information, see [{{site.data.keyword.containershort_notm}} technology](cs_tech.html#ibm-cloud-container-service-technology).
 </td>
 </tr>
 <tr>
 <td>{{site.data.keyword.Bluemix_notm}} Dedicated
 </td>
 <td>{{site.data.keyword.Bluemix_notm}} Dedicated offers the same Container Service capabilities on the cloud as {{site.data.keyword.Bluemix_notm}} Public. However, with an {{site.data.keyword.Bluemix_notm}} Dedicated account, available [physical resources are dedicated to your cluster only](cs_clusters.html#shared_dedicated_node) and are not shared with clusters from other {{site.data.keyword.IBM_notm}} customers. You might choose to set up an {{site.data.keyword.Bluemix_notm}} Dedicated environment when you require isolation for your cluster and the other {{site.data.keyword.Bluemix_notm}} services that you use.<br><br>For more information, see [Getting started with clusters in {{site.data.keyword.Bluemix_notm}} Dedicated](cs_dedicated.html#dedicated).
 </td>
 </tr>
 <tr>
 <td>{{site.data.keyword.Bluemix_notm}} Private
 </td>
 <td>{{site.data.keyword.Bluemix_notm}} Private is an application platform that can be installed locally on your own machines. You might choose to use Container Service in {{site.data.keyword.Bluemix_notm}} Private when you need to develop and manage on-prem, containerized apps in your own controlled and secure environment behind a firewall. <br><br>For more information, see [{{site.data.keyword.Bluemix_notm}} Private product information ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/cloud-computing/products/ibm-cloud-private/) and [documentation ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_1.2.0/kc_welcome_containers.html).
 </td>
 </tr>
 <tr>
 <td>Hybrid setup
 </td>
 <td>Hybrid is the combination use of services that run in {{site.data.keyword.Bluemix_notm}} Public or Dedicated and other services that run on-premise, such as an app in {{site.data.keyword.Bluemix_notm}} Private. Examples for a hybrid setup: <ul><li>Provisioning a cluster with Container Service in {{site.data.keyword.Bluemix_notm}} Public but connecting that cluster to an on-prem database.</li><li>Provisioning a cluster with Container Service in {{site.data.keyword.Bluemix_notm}} Private and deploying an app into that cluster. However, this app might use a {{site.data.keyword.ibmwatson}} service such as {{site.data.keyword.toneanalyzershort}} in {{site.data.keyword.Bluemix_notm}} Public.</li></ul><br>To enable communication between services running in {{site.data.keyword.Bluemix_notm}} Public/Dedicated and services running on-prem, you must [set up a VPN connection](cs_vpn.html).
 </td>
 </tr>
 </tbody>
</table>

<br />


## Comparison of free and standard clusters
{: #cluster_types}

You can create one free cluster or any number of standard clusters. Try out free clusters to get familiar and test a few Kubernetes capabilities, or create standard clusters to use the full capabilities of Kubernetes to deploy apps.
{:shortdesc}

|Characteristics|Free clusters|Standard clusters|
|---------------|-------------|-----------------|
|[In-cluster networking](cs_secure.html#in_cluster_network)|<img src="images/confirm.svg" width="32" alt="Feature available" style="width:32px;" />|<img src="images/confirm.svg" width="32" alt="Feature available" style="width:32px;" />|
|[Public network app access by a NodePort service to a non-stable IP address](cs_nodeport.html)|<img src="images/confirm.svg" width="32" alt="Feature available" style="width:32px;" />|<img src="images/confirm.svg" width="32" alt="Feature available" style="width:32px;" />|
|[User access management](cs_users.html#managing)|<img src="images/confirm.svg" width="32" alt="Feature available" style="width:32px;" />|<img src="images/confirm.svg" width="32" alt="Feature available" style="width:32px;" />|
|[{{site.data.keyword.Bluemix_notm}} service access from the cluster and apps](cs_integrations.html#adding_cluster)|<img src="images/confirm.svg" width="32" alt="Feature available" style="width:32px;" />|<img src="images/confirm.svg" width="32" alt="Feature available" style="width:32px;" />|
|[Disk space on worker node for non-persistent storage](cs_storage.html#planning)|<img src="images/confirm.svg" width="32" alt="Feature available" style="width:32px;" />|<img src="images/confirm.svg" width="32" alt="Feature available" style="width:32px;" />|
|[Persistent NFS file-based storage with volumes](cs_storage.html#planning)| |<img src="images/confirm.svg" width="32" alt="Feature available" style="width:32px;" />|
|[Public or private network app access by a load balancer service to a stable IP address](cs_loadbalancer.html#planning)| |<img src="images/confirm.svg" width="32" alt="Feature available" style="width:32px;" />|
|[Public network app access by an Ingress service to a stable IP address and customizable URL](cs_ingress.html#planning)| |<img src="images/confirm.svg" width="32" alt="Feature available" style="width:32px;" />|
|[Portable public IP addresses](cs_subnets.html#manage)| |<img src="images/confirm.svg" width="32" alt="Feature available" style="width:32px;" />|
|[Logging and monitoring](cs_health.html#logging)| |<img src="images/confirm.svg" width="32" alt="Feature available" style="width:32px;" />|
|[Option to provision your worker nodes on physical (bare metal) servers](cs_clusters.html#shared_dedicated_node) | |<img src="images/confirm.svg" width="32" alt="Feature available" style="width:32px;" />|
|[Option to provision bare metal workers with Trusted Compute](cs_clusters.html#shared_dedicated_node) | |<img src="images/confirm.svg" width="32" alt="Feature available" style="width:32px;" />|
|[Available in {{site.data.keyword.Bluemix_dedicated_notm}}](cs_dedicated.html#dedicated_environment)| |<img src="images/confirm.svg" width="32" alt="Feature available" style="width:32px;" />|

<br />




{: #responsibilities}
**Note**: Looking for your responsibilities and container terms when you use the service?

{: #terms}
See [{{site.data.keyword.containershort_notm}} responsibilities](cs_responsibilities.html).
