---

copyright:
  years: 2014, 2017
lastupdated: "2017-11-17"

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

![{{site.data.keyword.containershort_notm}} regions and datacenters](/images/regions.png)

Figure 1. {{site.data.keyword.containershort_notm}} regions and datacenters

Supported {{site.data.keyword.containershort_notm}} regions:
  * AP North
  * AP South
  * EU Central
  * UK South
  * US East
  * US South

You can create Kubernetes lite clusters in the following regions:
  * AP South
  * EU Central
  * UK South
  * US South

  **Note**: If you are not a paid customer, you are unable to create lite clusters in the US South region.


## {{site.data.keyword.Bluemix_notm}} region API endpoints
{: #bluemix_regions}

You can organize your resources across {{site.data.keyword.Bluemix_notm}} services by using {{site.data.keyword.Bluemix_notm}} regions. For example, you can create a Kubernetes cluster by using a private Docker image that is stored in your {{site.data.keyword.registryshort_notm}} of the same region.
{:shortdesc}

To check which {{site.data.keyword.Bluemix_notm}} region you are currently in, run `bx info` and review the **Region** field.

{{site.data.keyword.Bluemix_notm}} regions can be accessed by specifying the API endpoint when you log in. If you do not specify a region, you are automatically logged in to the region that is closest to you.

{{site.data.keyword.Bluemix_notm}} region API endpoints with example log in commands:

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

{{site.data.keyword.containershort_notm}} region API endpoints:
  * AP North: `https://ap-north.containers.bluemix.net`
  * AP South: `https://ap-south.containers.bluemix.net`
  * EU Central: `https://eu-central.containers.bluemix.net`
  * UK South: `https://uk-south.containers.bluemix.net`
  * US East: `https://us-east.containers.bluemix.net`
  * US South: `https://us-south.containers.bluemix.net`

To check which {{site.data.keyword.containershort_notm}} region you are currently in, run `bx cs region`.

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
  | AP North | hkg02, tok02 | Hong Kong, Tokyo |
  | AP South     | mel01, syd01, syd04        | Melbourne, Sydney |
  | EU Central     | ams03, fra02, par01        | Amsterdam, Frankfurt, Paris |
  | UK South      | lon02, lon04         | London |
  | US East      | tor01, wdc06, wdc07        | Toronto, Washington, DC |
  | US South     | dal10, dal12, dal13       | Dallas |

### Using container service API commands
{: #container_api}

To interact with the {{site.data.keyword.containershort_notm}} API, enter the command type and append `/v1/command` to the endpoint.

Example of `GET /clusters` API in US South:
  ```
  GET https://us-south.containers.bluemix.net/v1/clusters
  ```
  {: codeblock}

</br>

To view documentation on the API commands, append `swagger-api` to the endpoint for the region to view.
  * AP North: https://ap-north.containers.bluemix.net/swagger-api/
  * AP South: https://ap-south.containers.bluemix.net/swagger-api/
  * EU Central: https://eu-central.containers.bluemix.net/swagger-api/
  * UK South: https://uk-south.containers.bluemix.net/swagger-api/
  * US East: https://us-east.containers.bluemix.net/swagger-api/
  * US South: https://us-south.containers.bluemix.net/swagger-api/
