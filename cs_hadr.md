---

copyright:
  years: 2014, 2018
lastupdated: "2018-01-24"

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
{: #hadr}

High availability (HA) is a core discipline in an IT infrastructure to keep your apps up and running, even after a partial or full site failure. The main target of HA is to eliminate potential fault domains in an IT infrastructure, for example, by adding redundancy and setting up failover mechanisms. Simply put, high availability is the process in which a system is supposed to take over when another system stops working efficiently or at all.  For IT, this must occur with as minimal downtime as possible—so minimal that most users don’t even know there was a problem.  Data loss must also be negligible; 

HA can be achieved on different levels in your IT infrastructure and within different components of your cluster. The level of HA that is right for you depends on your business requirements and the Service Level Agreements that you have with your customers.  

## Overview of potential fault domains in {{site.data.keyword.containerlong_notm}}
{: #fault_domains}

The following image shows how you can achieve high availability for a stateful app in a single {{site.data.keyword.containerlong_notm}} region. By setting up multiple clusters within a region, you can protect your app from potential fault domains that are marked with an 'X'. Both clusters share data via the IBM Cloud Cloudant database service, so that one cluster can take over the workload without a downtime for your app in case an entire data center is not reachable. 

<img src="images/cs_cluster_ha.png" style="height: 100px; border-style: none;" height="500" align="left" alt="Overview of fault domains in a high availability cluster within an {{site.data.keyword.containershort_notm}} region."/>

<p></p>

|#|Fault domain name|Description|How do I protect against failure?|Link to docs
|---|---|---|---|---|
1|Container or pod failure|Containers and pods are, by design, short-lived and can fail unexpectedly. To make your app highly available  you  For example,  To keep your app available When you deploy a container into a cluster |<ul><li>Use deployments to |[Highly available apps](cs_app.html#highly_available_apps).|

