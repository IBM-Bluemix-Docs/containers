---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-12"

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
{{site.data.keyword.Bluemix}} is hosted worldwide. A region is a geographic area that is accessed by an endpoint. Locations are data centers within the region. Services within {{site.data.keyword.Bluemix_notm}} might be available globally, or within a specific region.
{:shortdesc}

[{{site.data.keyword.Bluemix_notm}} regions](#bluemix_regions) differ from [{{site.data.keyword.containershort_notm}} regions](#container_regions).

![{{site.data.keyword.containershort_notm}} regions and data centers](/images/regions.png)

Figure 1. {{site.data.keyword.containershort_notm}} regions and data centers

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

You might want to log in to another {{site.data.keyword.containershort_notm}} region for the following reasons:
  * You created {{site.data.keyword.Bluemix_notm}} services or private Docker images in one region and want to use them with {{site.data.keyword.containershort_notm}} in another region.
  * You want to access a cluster in a region that is different from the default {{site.data.keyword.Bluemix_notm}} region you are logged in to.

</br>

To quickly switch regions, run `bx cs region-set`.

### Locations available for the container service
{: #locations}

Locations are data centers that are available within a region.

  | Region | Location | City |
  |--------|----------|------|
  | AP North | hkg02, sng01, tok02 | Hong Kong, Singapore, Tokyo |
  | AP South     | mel01, syd01, syd04        | Melbourne, Sydney |
  | EU Central     | ams03, fra02, par01        | Amsterdam, Frankfurt, Paris |
  | UK South      | lon02, lon04         | London |
  | US East      | mon01, tor01, wdc06, wdc07        | Montreal, Toronto, Washington DC |
  | US South     | dal10, dal12, dal13       | Dallas |

### Using container service API commands
{: #container_api}

To interact with the {{site.data.keyword.containershort_notm}} API, enter the command type and append `/v1/command` to the global endpoint.

Example of `GET /clusters` API:
  ```
  GET https://containers.bluemix.net/v1/clusters
  ```
  {: codeblock}

</br>

To use the API with the global endpoint, in all your requests, pass the region name in an `X-Region` header. To list available regions, run `bx cs regions`.
{: tip}

To view documentation on the API commands, view [https://containers.bluemix.net/swagger-api/](https://containers.bluemix.net/swagger-api/).
