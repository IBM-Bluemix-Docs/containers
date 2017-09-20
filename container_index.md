---

copyright:
  years: 2014, 2017
lastupdated: "2017-08-14"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}


# Getting started with {{site.data.keyword.containerlong_notm}}
{: #container_index}

Manage highly available apps inside Docker containers and Kubernetes clusters on the {{site.data.keyword.IBM}} cloud. A container is a standard way to package an app and all its dependencies so that the app can be moved between environments and run without changes. Unlike virtual machines, containers do not bundle the operating system. Only the app code, run time, system tools, libraries, and settings are packaged inside the container, which makes a container more lightweight, portable, and efficient than a virtual machine.

Click an option to get started:

<img usemap="#home_map" border="0" class="image" id="image_ztx_crb_f1b" src="images/cs_public_dedicated_options.png" width="440" alt="With Bluemix Public, you can create Kubernetes clusters or migrate single and scalable container groups to clusters. With Bluemix Dedicated, click this icon to see your options." style="width:440px;" />
<map name="home_map" id="home_map">
<area href="#clusters" alt="Getting started with Kubernetes clusters in Bluemix" title="Getting started with Kubernetes clusters in Bluemix" shape="rect" coords="-7, -8, 108, 211" />
<area href="cs_classic.html#cs_classic" alt="Running single and scalable containers in IBM Bluemix Container Service" title="Running single and scalable containers in IBM Bluemix Container Service" shape="rect" coords="155, -1, 289, 210" />
<area href="cs_ov.html#dedicated_environment" alt="Bluemix Dedicated cloud environment" title="Bluemix Dedicated cloud environment" shape="rect" coords="326, -10, 448, 218" />
</map>


## Getting started with clusters
{: #clusters}

Kubernetes is an orchestration tool for scheduling app containers onto a cluster of compute machines. With Kubernetes, developers can rapidly develop highly available applications by using the power and flexibility of containers.
{:shortdesc}

Before you can deploy an app by using Kubernetes, start by creating a cluster. A cluster is a set of worker nodes that are organized into a network. The purpose of the cluster is to define a set of resources, nodes, networks, and storage devices that keep applications highly available.

To create a lite cluster:

1.  From the [**catalog** ![External link icon](../icons/launch-glyph.svg "External link icon")](https://console.bluemix.net/catalog/?category=containers), in the **Containers** category, click **Kubernetes cluster**.

2.  Enter the cluster details. The default cluster type is lite, so you have just a few fields to customize. Next time, you can create a standard cluster and define additional customizations, like how many worker nodes are in the cluster.
    1.  Enter a **Cluster Name**.
    2.  Select a **Location** in which to deploy your cluster. The locations that are available to you depend on the region that you are logged in to. Select the region that is physically closest to you for best performance.

    Available locations are:

    <ul><li>US-South<ul><li>dal10 [Dallas]</li><li>dal12 [Dallas]</li></ul></li><li>UK-South<ul><li>lon02 [London]</li><li>lon04 [London]</li></ul></li><li>EU-Central<ul><li>ams03 [Amsterdam]</li><li>ra02 [Frankfurt]</li></ul></li><li>AP-South<ul><li>syd01 [Sydney]</li><li>syd04 [Sydney]</li></ul></li></ul>

3.  Click **Create Cluster**. The details for the cluster open, but the worker node in the cluster takes a few minutes to provision. You can see the status of the worker node in the **Worker nodes** tab. When the status reaches `Ready`, your worker node is ready to be used.

Good work! You created your first cluster!

*   The lite cluster has one worker node with 2 CPU and 4 GB memory available for your apps to use.
*   The worker node is centrally monitored and managed by a dedicated and highly available {{site.data.keyword.IBM_notm}}-owned Kubernetes master that controls and monitors all of the Kubernetes resources in the cluster. You can focus on your worker node and the apps that are deployed in the worker node without worrying about managing this master too.
*   The resources that are required to run the cluster, such as VLANS and IP addresses, are managed in an {{site.data.keyword.IBM_notm}}-owned {{site.data.keyword.BluSoftlayer_full}} account. When you create a standard cluster, you manage these resources in your own {{site.data.keyword.BluSoftlayer_notm}} account. You can learn more about these resources when you create a standard cluster.
*   **Tip:** Lite clusters that are created with a {{site.data.keyword.Bluemix_notm}} free trial account are automatically removed after the free trial period ends, unless you [upgrade to a {{site.data.keyword.Bluemix_notm}} Pay-As-You-Go account](/docs/pricing/billable.html#upgradetopayg).


**What's next?**

When the cluster is up and running, you can check out the following tasks.

* [Install the CLIs to start working with your cluster.](cs_cli_install.html#cs_cli_install)
* [Deploy an app in your cluster.](cs_apps.html#cs_apps_cli)
* [Create a standard cluster with multiple nodes for higher availability.](cs_cluster.html#cs_cluster_ui)
* [Set up your own private registry in {{site.data.keyword.Bluemix_notm}} to store and share Docker images with other users.](/docs/services/Registry/index.html)


## Getting started with clusters in {{site.data.keyword.Bluemix_notm}} Dedicated (Closed Beta)
{: #dedicated}

Kubernetes is an orchestration tool for scheduling app containers onto a cluster of compute machines. With Kubernetes, developers can rapidly develop highly available applications by using the power and flexibility of containers in their {{site.data.keyword.Bluemix_notm}} Dedicated instance.
{:shortdesc}

Before you begin, [set up your {{site.data.keyword.Bluemix_notm}} Dedicated environment to use clusters](cs_ov.html#setup_dedicated). Then, you can create a cluster. A cluster is a set of worker nodes that are organized into a network. The purpose of the cluster is to define a set of resources, nodes, networks, and storage devices that keep applications highly available. After you have a cluster, then you can deploy your app into the cluster.

**Tip:** If your organization does not have a {{site.data.keyword.Bluemix_notm}} Dedicated environment already, you might not need one. [Try a dedicated, standard cluster in the {{site.data.keyword.Bluemix_notm}} Public environment first.](cs_cluster.html#cs_cluster_ui)

To deploy a cluster in {{site.data.keyword.Bluemix_notm}} Dedicated:

1.  Log in to the {{site.data.keyword.Bluemix_notm}} Public console ([https://console.bluemix.net ![External link icon](../icons/launch-glyph.svg "External link icon")](https://console.bluemix.net/catalog/?category=containers)) with your IBMid. Though you must request a cluster from {{site.data.keyword.Bluemix_notm}} Public, you are deploying it into your {{site.data.keyword.Bluemix_notm}} Dedicated account.
2.  If you have multiple accounts, from the account menu, select a {{site.data.keyword.Bluemix_notm}} account.
3.  From the catalog, in the **Containers** category, click **Kubernetes cluster**.
4.  Enter the cluster details.
    1.  Enter a **Cluster Name**.
    2.  Select a **Kubernetes version** to use in the worker nodes. 
    3.  Select a **Machine type**. The machine type defines the amount of virtual CPU and memory that is set up in each worker node and that is available for all the containers that you deploy in your nodes.
    4.  Choose the **Number of worker nodes** that you need. Select 3 for higher availability of your cluster.

    The cluster type, location, public VLAN, private VLAN, and hardware fields are defined during the creation process of the {{site.data.keyword.Bluemix_notm}} Dedicated account, so you cannot adjust those values.
5.  Click **Create Cluster**. The details for the cluster open, but the worker nodes in the cluster take a few minutes to provision. You can see the status of the worker nodes in the **Worker nodes** tab. When the status reaches `Ready`, your worker nodes are ready to be used.

    The worker nodes are centrally monitored and managed by a dedicated and highly available {{site.data.keyword.IBM_notm}}-owned Kubernetes master that controls and monitors all of the Kubernetes resources in the cluster. You can focus on your worker nodes and the apps that are deployed in the worker nodes without worrying about managing this master too.

Good work! You created your first cluster!


**What's next?**

When the cluster is up and running, you can check out the following tasks.

* [Install the CLIs to start working with your cluster.](cs_cli_install.html#cs_cli_install)
* [Deploy an app to your cluster.](cs_apps.html#cs_apps_cli)
* [Add {{site.data.keyword.Bluemix_notm}} services to your cluster.](cs_cluster.html#binding_dedicated)
* [Learn about the differences between clusters in {{site.data.keyword.Bluemix_notm}} Dedicated and Public.](cs_ov.html#env_differences)
