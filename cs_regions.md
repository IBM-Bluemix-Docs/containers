---

copyright:
  years: 2014, 2018
lastupdated: "2018-4-20"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}

# Regions and locations
{{site.data.keyword.Bluemix}} is hosted worldwide. A region is a geographic area that is accessed by an endpoint. Locations are data centers within the region. Services within {{site.data.keyword.Bluemix_notm}} might be available globally, or within a specific region. When you create a Kubernetes cluster in {{site.data.keyword.containerlong}}, its resources remain in the region that you deploy the cluster to.
{:shortdesc}

[{{site.data.keyword.Bluemix_notm}} regions](#bluemix_regions) differ from [{{site.data.keyword.containershort_notm}} regions](#container_regions).

![{{site.data.keyword.containershort_notm}} regions and data centers](/images/regions.png)

{{site.data.keyword.containershort_notm}} regions and data centers

Supported {{site.data.keyword.containershort_notm}} regions:
  * AP North
  * AP South
  * EU Central
  * UK South
  * US East
  * US South



## {{site.data.keyword.Bluemix_notm}} region API endpoints
{: #bluemix_regions}

You can organize your resources across {{site.data.keyword.Bluemix_notm}} services by using {{site.data.keyword.Bluemix_notm}} regions. For example, you can create a Kubernetes cluster by using a private Docker image that is stored in your {{site.data.keyword.registryshort_notm}} of the same region.
{:shortdesc}

To check which {{site.data.keyword.Bluemix_notm}} region you are currently in, run `bx info` and review the **Region** field.

{{site.data.keyword.Bluemix_notm}} regions can be accessed by specifying the API endpoint when you log in. If you do not specify a region, you are automatically logged in to the region that is closest to you.

{{site.data.keyword.Bluemix_notm}} region API endpoints with example login commands:

  * US South and US East
      ```
      bx login -a api.ng.bluemix.net
      ```
      {: pre}

  * Sydney and AP North
      ```
      bx login -a api.au-syd.bluemix.net
      ```
      {: pre}

  * Germany
      ```
      bx login -a api.eu-de.bluemix.net
      ```
      {: pre}

  * United Kingdom
      ```
      bx login -a api.eu-gb.bluemix.net
      ```
      {: pre}



<br />


## {{site.data.keyword.containershort_notm}} region API endpoints and locations
{: #container_regions}

By using {{site.data.keyword.containershort_notm}} regions, you can create or access Kubernetes clusters in a region other than the {{site.data.keyword.Bluemix_notm}} region that you are logged in to. {{site.data.keyword.containershort_notm}} region endpoints refer specifically to the {{site.data.keyword.containershort_notm}}, not {{site.data.keyword.Bluemix_notm}} as a whole.
{:shortdesc}

You can access the {{site.data.keyword.containershort_notm}} through one global endpoint: `https://containers.bluemix.net/`.
* To check which {{site.data.keyword.containershort_notm}} region you are currently in, run `bx cs region`.
* To retrieve a list of available regions and their endpoints, run `bx cs regions`.

To use the API with the global endpoint, in all your requests, pass the region name in an `X-Region` header.
{: tip}

### Logging in to a different container service region
{: #container_login_endpoints}

You can change locations by using the {{site.data.keyword.containershort_notm}} CLI.
{:shortdesc}

You might want to log in to another {{site.data.keyword.containershort_notm}} region for the following reasons:
  * You created {{site.data.keyword.Bluemix_notm}} services or private Docker images in one region and want to use them with {{site.data.keyword.containershort_notm}} in another region.
  * You want to access a cluster in a region that is different from the default {{site.data.keyword.Bluemix_notm}} region you are logged in to.

</br>

To quickly switch regions, run `bx cs region-set`.

### Using container service API commands
{: #containers_api}

To interact with the {{site.data.keyword.containershort_notm}} API, enter the command type and append `/v1/command` to the global endpoint.
{:shortdesc}

Example of `GET /clusters` API:
  ```
  GET https://containers.bluemix.net/v1/clusters
  ```
  {: codeblock}

</br>

To use the API with the global endpoint, in all your requests, pass the region name in an `X-Region` header. To list available regions, run `bx cs regions`.
{: tip}

To view documentation on the API commands, view [https://containers.bluemix.net/swagger-api/](https://containers.bluemix.net/swagger-api/).

## Locations available in {{site.data.keyword.containershort_notm}}
{: #locations}

Locations are physical data centers that are available within an {{site.data.keyword.Bluemix_notm}} region. Regions are a conceptual tool to organize locations, and can include locations (data centers) in different countries. The following table displays the locations available by region.
{:shortdesc}

| Region | Location | City |
|--------|----------|------|
| AP North | hkg02, seo01, sng01, tok02 | Hong Kong, Seoul, Singapore, Tokyo |
| AP South     | mel01, syd01, syd04        | Melbourne, Sydney |
| EU Central     | ams03, fra02, par01        | Amsterdam, Frankfurt, Paris |
| UK South      | lon02, lon04         | London |
| US East      | mon01, tor01, wdc06, wdc07        | Montreal, Toronto, Washington DC |
| US South     | dal10, dal12, dal13, sao01       | Dallas, São Paulo |

Your cluster's resources remain in the location (data center) in which the cluster is deployed. The following image highlights the relationship of your cluster within an example region of US East:

1.  Your cluster's resources, including the master and worker nodes, are in the same location that you deployed the cluster to. When you initiate local container orchestration actions, such as `kubectl` commands, the information is exchanged between your master and worker nodes within the same location.

2.  If you set up other cluster resources, such as storage, networking, compute, or apps running in pods, the resources and their data remain in the location that you deployed your cluster to.

3.  When you initiate cluster management actions, such as using `bx cs` commands, basic information about the cluster (such as name, ID, user, the command) is routed to a regional endpoint.

![Understanding where your cluster resources reside](/images/region-cluster-resources.png)

Understanding where your cluster resources reside.

