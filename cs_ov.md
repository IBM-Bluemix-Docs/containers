---

copyright:
  years: 2014, 2018
lastupdated: "2017-12-11"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}


# About {{site.data.keyword.containerlong_notm}}
{: #cs_ov}

{{site.data.keyword.containershort}} delivers powerful tools by combining Docker and Kubernetes technologies, an intuitive user experience, and built-in security and isolation to automate the deployment, operation, scaling, and monitoring of containerized apps in a cluster of compute hosts.
{:shortdesc}


<br />


## Docker containers
{: #cs_ov_docker}

Docker is an open source project that was released by dotCloud in 2013. Built on features of existing Linux container technology (LXC), Docker became a software platform for building, testing, deploying, and scaling apps quickly. Docker packages software into standardized units, called containers, that include all of the elements that an app needs to run.
{:shortdesc}

Learn about some basic Docker concepts:

<dl>
<dt>Image</dt>
<dd>A Docker image is built from a Dockerfile, a text file that defines how to build the image and which build artifacts to include in it, such as the app, the app's configuration, and its dependencies. Images are always built from other images, making them quick to configure. Let someone else do the bulk of the work on an image and then tweak it for your use.</dd>
<dt>Registry</dt>
<dd>An image registry is a place to store, retrieve, and share Docker images. Images that are stored in a registry can either be publicly available (public registry) or accessible by a small group of users (private registry). {{site.data.keyword.containershort_notm}} offers public images, such as ibmliberty, that you can use to create your first containerized app. When it comes to enterprise applications, use a private registry like the one that is provided in {{site.data.keyword.Bluemix_notm}} to protect your images from being used by unauthorized users.
</dd>
<dt>Container</dt>
<dd>Every container is created from an image. A container is a packaged app with all of its dependencies so that the app can be moved between environments and run without changes. Unlike virtual machines, containers do not virtualize a device, its operating system, and the underlying hardware. Only the app code, run time, system tools, libraries, and settings are packaged inside the container. Containers run as isolated processes on compute hosts and share the host operating system and its hardware resources. This approach makes a container more lightweight, portable, and efficient than a virtual machine.</dd>
</dl>

### Key benefits of using containers
{: #container_benefits}

<dl>
<dt>Containers are agile</dt>
<dd>Containers simplify system administration by providing standardized environments for development and production deployments. The lightweight run time enables rapid scale-up and scale-down of deployments. Remove the complexity of managing different operating system platforms and their underlying infrastructures by using containers to help you deploy and run any app on any infrastructure quickly and reliably.</dd>
<dt>Containers are small</dt>
<dd>You can fit many containers in the amount of space that a single virtual machine requires.</dd>
<dt>Containers are portable</dt>
<dd><ul>
  <li>Reuse pieces of images to build containers. </li>
  <li>Move app code quickly from staging to production environments.</li>
  <li>Automate your processes with continuous delivery tools.</li> </ul></dd>
</dl>


<br />


## Kubernetes basics
{: #kubernetes_basics}

Kubernetes was developed by Google as part of the Borg project and handed off to the open source community in 2014. Kubernetes combines more than 15 years of Google research in running a containerized infrastructure with production work loads, open source contributions, and Docker container management tools to provide an isolated and secure app platform for managing containers that is portable, extensible, and self-healing in case of failovers.
{:shortdesc}

Learn about some basic Kubernetes concepts as shown in the following diagram.

![Deployment setup](images/cs_app_tutorial_components1.png)

<dl>
<dt>Account</dt>
<dd>Your account refers to your {{site.data.keyword.Bluemix_notm}} account.</dd>

<dt>Cluster</dt>
<dd>A Kubernetes cluster consists of one or more compute hosts that are called worker nodes. Worker nodes are managed by a Kubernetes master that centrally controls and monitors all Kubernetes resources in the cluster. So when you deploy the resources for a containerized app, the Kubernetes master decides which worker node to deploy those resources on, taking into account the deployment requirements and available capacity in the cluster. Kubernetes resources include services, deployments, and pods.</dd>

<dt>Service</dt>
<dd>A service is a Kubernetes resource that groups a set of pods and provides network connectivity to these pods without exposing the actual private IP address of each pod. You can use a service to make your app available within your cluster or to the public internet.
</dd>

<dt>Deployment</dt>
<dd>A deployment is a Kubernetes resource where you might specify information about other resources or capabilities that are required to run your app, such as services, persistent storage, or annotations. You document a deployment in a configuration YAML file, and then apply it to the cluster. The Kubernetes master configures the resources and deploys containers into pods on the worker nodes with available capacity.
</br></br>
Define update strategies for your app, including the number of pods that you want to add during a rolling update and the number of pods that can be unavailable at a time. When you perform a rolling update, the deployment checks whether the update is working and stops the rollout when failures are detected.</dd>

<dt>Pod</dt>
<dd>Every containerized app that is deployed into a cluster is deployed, run, and managed by a Kubernetes resource that is called a pod. Pods represent small deployable units in a Kubernetes cluster and are used to group the containers that must be treated as a single unit. In most cases, each container is deployed in its own pod. However, an app might require a container and other helper containers to be deployed into one pod so that those containers can be addressed by using the same private IP address.</dd>

<dt>App</dt>
<dd>An app might refer to a complete app or a component of an app. You might deploy components of an app in separate pods or separate worker nodes.
</br></br>
To learn more about Kubernetes terminology, <a href="cs_tutorials.html#cs_cluster_tutorial" target="_blank">try the tutorial</a>.</dd>

</dl>

<br />


## Benefits of using clusters
{: #cs_ov_benefits}

Clusters are deployed on compute hosts that provide native Kubernetes and {{site.data.keyword.IBM_notm}}-added capabilities.
{:shortdesc}

|Benefit|Description|
|-------|-----------|
|Single-tenant Kubernetes clusters with compute, network, and storage infrastructure isolation|<ul><li>Create your own customized infrastructure that meets the requirements of your organization.</li><li>Provision a dedicated and secured Kubernetes master, worker nodes, virtual networks, and storage by using the resources provided by IBM Cloud infrastructure (SoftLayer).</li><li>Store persistent data, share data between Kubernetes pods, and restore data when needed with the integrated and secure volume service.</li><li>Fully managed Kubernetes master that is continuously monitored and updated by {{site.data.keyword.IBM_notm}} to keep your cluster available.</li><li>Benefit from full support for all native Kubernetes APIs.</li></ul>|
|Image security compliance with Vulnerability Advisor|<ul><li>Set up your own secured Docker private image registry where images are stored and shared by all users in the organization.</li><li>Benefit from automatic scanning of images in your private {{site.data.keyword.Bluemix_notm}} registry.</li><li>Review recommendations specific to the operating system used in the image to fix potential vulnerabilities.</li></ul>|
|Automatic scaling of apps|<ul><li>Define custom policies to scale up and scale down apps based on CPU and memory consumption.</li></ul>|
|Continuous monitoring of the cluster health|<ul><li>Use the cluster dashboard to quickly see and manage the health of your cluster, worker nodes, and container deployments.</li><li>Find detailed consumption metrics by using {{site.data.keyword.monitoringlong}} and quickly expand your cluster to meet work loads.</li><li>Review logging information by using {{site.data.keyword.loganalysislong}} to see detailed cluster activities.</li></ul>|
|Automatic recovery of unhealthy containers|<ul><li>Continuous health checks on containers that are deployed on a worker node.</li><li>Automatic re-creation of containers in case of failures.</li></ul>|
|Service discovery and service management|<ul><li>Centrally register app services to make them available to other apps in your cluster without exposing them publicly.</li><li>Discover registered services without keeping track of changing IP addresses or container IDs and benefit from automatic routing to available instances.</li></ul>|
|Secure exposure of services to the public|<ul><li>Private overlay networks with full load balancer and Ingress support to make your apps publicly available and balance workloads across multiple worker nodes without keeping track of changing IP addresses inside your cluster.</li><li>Choose between a public IP address, an {{site.data.keyword.IBM_notm}} provided route, or your own custom domain to access services in your cluster from the internet.</li></ul>|
|{{site.data.keyword.Bluemix_notm}} service integration|<ul><li>Add extra capabilities to your app through the integration of {{site.data.keyword.Bluemix_notm}} services, such as Watson APIs, Blockchain, data services, or Internet of Things, and help cluster users to simplify the app development and container management process.</li></ul>|
{: caption="Table 1. Benefits of using clusters with {{site.data.keyword.containerlong_notm}}" caption-side="top"}

<br />


## Service architecture
{: #cs_ov_architecture}

Each worker node is set up with an {{site.data.keyword.IBM_notm}} managed Docker Engine, separate compute resources, networking, and volume service, as well as built-in security features that provide isolation, resource management capabilities, and worker node security compliance. The worker node communicates with the master by using secure TLS certificates and openVPN connection.
{:shortdesc}

*Figure 1. Kubernetes architecture and networking in the {{site.data.keyword.containershort_notm}}*

![{{site.data.keyword.containerlong_notm}} Kubernetes architecture](images/cs_org_ov.png)

The diagram outlines what you maintain and what IBM maintains in a cluster. For more details about these maintenance tasks, see [Cluster management responsibilities](cs_planning.html#responsibilities).

<br />


## Abuse of containers
{: #cs_terms}

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
