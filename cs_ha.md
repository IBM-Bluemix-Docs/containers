---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-06"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}


# High availability for {{site.data.keyword.containerlong_notm}}
{: #ha}

High availability is a core discipline in an IT infrastructure to keep your apps up and running, even after a partial or full site failure. The main purpose of high availability is to eliminate potential points of failures in an IT infrastructure. For example, you can prepare for the failure of one system by adding redundancy and setting up failover mechanisms. 

You can achieve high availability on different levels in your IT infrastructure and within different components of your cluster. The level of availability that is right for you depends on several factors, such as your business requirements, the Service Level Agreements that you have with your customers, and the money that you want to spend. 

## Overview of potential points of failure in {{site.data.keyword.containerlong_notm}}
{: #fault_domains}

The {{site.data.keyword.containerlong_notm}} architecture and infrastructure is designed to ensure reliability, low processing latency and a maximum uptime of the service. However, failures can happen. Depending on the service that you host in {{site.data.keyword.Bluemix_notm}}, you might not be able to tolerate failures, even if failures last for only a few minutes. 

{{site.data.keyword.containershort_notm}} provides several approaches to add more availability to your cluster by adding redundancy and anti-affinity. Review the following image to learn about potential points of failure and how to eliminate them. 

<img src="images/cs_failure_ov.png" alt="Overview of fault domains in a high availability cluster within an {{site.data.keyword.containershort_notm}} region." width="250" style="width:250px; border-style: none"/>

<table summary="The table shows points of failure in {{site.data.keyword.containershort_notm}}. Rows are to be read from the left to right, with the number of the point of failure in column one, the title of the point of failure in column two, a description in column three, and a link to the documentation in column four.">
<col width="3%">
<col width="10%">
<col width="70%">
<col width="17%">
  <thead>
  <th>#</th>
  <th>Point of failure</th>
  <th>Description</th>
  <th>Link to docs</th>
  </thead>
  <tbody>
    <tr>
      <td>1</td>
      <td>Container or pod failure</td>
      <td>Containers and pods are, by design, short-lived and can fail unexpectedly. For example, a container or pod might crash if an error occurs in your app. To make your app highly available, you must ensure that you have enough instances of your app to handle the workload plus additional instances in case of a failure. Ideally, these instances are distributed across multiple worker nodes to protect your app from a worker node failure.</td>
      <td>[Deploying highly available apps.](cs_app.html#highly_available_apps)</td>
  </tr>
  <tr>
    <td>2</td>
    <td>Worker node failure</td>
    <td>A worker node is a VM that runs on top of a physical hardware. Worker node failures include hardware outages, such as power, cooling, or networking, and issues on the VM itself. You can account for a worker node failure by setting up multiple worker nodes in your cluster. <br/><br/><strong>Note:</strong> Worker nodes in one location are not guaranteed to be on separate physical compute hosts. For example, you might have a cluster with 3 worker nodes, but all 3 worker nodes were created on the same physical compute host in the IBM location. If this physical compute host goes down, all your worker nodes are down. To protect against this failure, you must set up a second cluster in another location.</td>
    <td>[Creating clusters with multiple worker nodes.](cs_cli_reference.html#cs_cluster_create)</td>
  </tr>
  <tr>
    <td>3</td>
    <td>Cluster failure</td>
    <td>The Kubernetes master is the main component that keeps your cluster up and running. The master stores all cluster data in the etcd database that serves as the single point of truth for your cluster. A cluster failure occurs when the master cannot be reached due to a networking failure or when data in your etcd database get corrupted. You can create multiple clusters in one location to protect your apps from a Kubernetes master or etcd failure. To load balance between the clusters, you must set up an external load balancer. <br/><br/><strong>Note:</strong> Setting up multiple clusters in one location does not guarantee that your worker nodes are deployed on separate physical compute hosts. To protect against this failure, you must set up a second cluster in another location.</td>
    <td>[Setting up highly available clusters.](cs_clusters.html#planning_clusters)</td>
  </tr>
  <tr>
    <td>4</td>
    <td>Location failure</td>
    <td>A location failure affects all physical compute hosts and NFS storage. Failures include power, cooling, networking, or storage outages, and natural disasters, like flooding, earthquakes, and hurricanes. To protect against a location failure, you must have clusters in two different locations that are load balanced by an external load balancer.</td>
    <td>[Setting up highly available clusters.](cs_clusters.html#planning_clusters)</td>
  </tr>
  <tr>
    <td>5</td>
    <td>Region failure</td>
    <td>Every region is set up with a highly available load balancer that is accessible from the region-specific API endpoint. The load balancer routes incoming and outgoing requests to clusters in the regional locations. The likelihood of a full regional failure is low. However, if you want to account for this failure, you can set up multiple clusters in different regions and connect them by using an external load balancer. In case an entire region fails, the cluster in the other region can take over the work load. <br/><br/><strong>Note:</strong> A multi-region cluster requires several Cloud resources, and depending on your app, can be complex and expensive. Check if you need a multi-region setup or if you can accomodate a potential service disruption. If you want to set up a multi-region cluster, make sure that your app and the data can be hosted in another region, and that your app can handle global data replication.</td>
    <td>[Setting up highly available clusters.](cs_clusters.html#planning_clusters)</td>
  </tr>
  <tr>
    <td>6a, 6b</td>
    <td>Storage failure</td>
    <td>In a stateful app, data plays an important role to keep your app up and running. You want to make sure that your data is highly available so that you can recover from a potential failure. In {{site.data.keyword.containershort_notm}} you can choose from several options to persist your data. For example, you can provision NFS storage by using Kubernetes native persistent volumes, or store your data by using an {{site.data.keyword.Bluemix_notm}} database service.</td>
    <td>[Planning highly available data.](cs_storage.html#planning)</td>
  </tr>
  </tbody>
  </table>
