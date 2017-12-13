---

copyright:
  years: 2014, 2017
lastupdated: "2017-12-13"

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

Hit the ground running with {{site.data.keyword.Bluemix_notm}} by deploying highly available apps in Docker containers that run in Kubernetes clusters. Containers are a standard way to package apps and all their dependencies so that you can seamlessly move the apps between environments. Unlike virtual machines, containers do not bundle the operating system. Only the app code, run time, system tools, libraries, and settings are packaged inside containers. Containers are more lightweight, portable, and efficient than virtual machines.
{:shortdesc}


Click an option to get started:

<img usemap="#home_map" border="0" class="image" id="image_ztx_crb_f1b" src="images/cs_public_dedicated_options.png" width="440" alt="Click an icon to get started quickly with {{site.data.keyword.containershort_notm}}. With {{site.data.keyword.Bluemix_dedicated_notm}}, click this icon to see your options." style="width:440px;" />
<map name="home_map" id="home_map">
<area href="#clusters" alt="Getting started with Kubernetes clusters in {{site.data.keyword.Bluemix_notm}}" title="Getting started with Kubernetes clusters in {{site.data.keyword.Bluemix_notm}}" shape="rect" coords="-7, -8, 108, 211" />
<area href="cs_cli_install.html" alt="Install the CLIs." title="Install the CLIs." shape="rect" coords="155, -1, 289, 210" />
<area href="cs_dedicated.html#dedicated_environment" alt="{{site.data.keyword.Bluemix_dedicated_notm}} cloud environment" title="{{site.data.keyword.Bluemix_notm}} cloud environment" shape="rect" coords="326, -10, 448, 218" />
</map>


## Getting started with clusters
{: #clusters}

So you want to deploy an app in container? Hold on! Start by creating a Kubernetes cluster first. Kubernetes is an orchestration tool for containers. With Kubernetes, developers can deploy highly available apps in a flash by using the power and flexibility of clusters.
{:shortdesc}

And what is a cluster? A cluster is a set of resources, worker nodes, networks, and storage devices that keep apps highly available. After you have your cluster, then you can deploy your apps in containers.

[Before you begin, you must have a Pay-As-You-Go or Subscription {{site.data.keyword.Bluemix_notm}} account to create a lite cluster.](/docs/pricing/billable.html#upgradetopayg)


To create a lite cluster:

1.  From the [**catalog** ![External link icon](../icons/launch-glyph.svg "External link icon")](https://console.bluemix.net/catalog/?category=containers), in the **Containers** category, click **Kubernetes cluster**.

2.  Enter a **Cluster Name**. The default cluster type is lite. Next time, you can create a standard cluster and define additional customizations, like how many worker nodes are in the cluster.

3.  Click **Create Cluster**. The details for the cluster open, but the worker node in the cluster takes a few minutes to provision. You can see the status of the worker node in the **Worker nodes** tab. When the status reaches `Ready`, your worker node is ready to be used.

Good work! You created your first cluster!

*   The lite cluster has one worker node with 2 CPU and 4 GB memory available for your apps to use.
*   The worker node is centrally monitored and managed by a dedicated and highly available {{site.data.keyword.IBM_notm}}-owned Kubernetes master that controls and monitors all of the Kubernetes resources in the cluster. You can focus on your worker node and the apps that are deployed in the worker node without worrying about managing this master too.
*   The resources that are required to run the cluster, such as VLANS and IP addresses, are managed in an {{site.data.keyword.IBM_notm}}-owned IBM Cloud infrastructure (SoftLayer) account. When you create a standard cluster, you manage these resources in your own IBM Cloud infrastructure (SoftLayer) account. You can learn more about these resources when you create a standard cluster.
*   **Tip:** With an {{site.data.keyword.Bluemix_notm}} lite account, you can create 1 lite cluster with 2 CPU and 4 GB RAM, and integrate it with lite services. To create more clusters, have different machine types, and use full services, [upgrade to an {{site.data.keyword.Bluemix_notm}} Pay-As-You-Go account](/docs/pricing/billable.html#upgradetopayg).


**What's next?**

When the cluster is up and running, start doing something with your cluster.

* [Install the CLIs to start working with your cluster.](cs_cli_install.html#cs_cli_install)
* [Deploy an app in your cluster.](cs_apps.html#cs_apps_cli)
* [Create a standard cluster with multiple nodes for higher availability.](cs_cluster.html#cs_cluster_ui)
* [Set up your own private registry in {{site.data.keyword.Bluemix_notm}} to store and share Docker images with other users.](/docs/services/Registry/index.html)
