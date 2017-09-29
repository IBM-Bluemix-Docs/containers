---

copyright:
  years: 2014, 2017
lastupdated: "2017-09-27"

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

## {{site.data.keyword.Bluemix_notm}} region API endpoints
{: #bluemix_regions}

You can organize your resources across {{site.data.keyword.Bluemix_notm}} services by using {{site.data.keyword.Bluemix_notm}} regions. For example, you can create a Kubernetes cluster using a private Docker image that is stored in your {{site.data.keyword.registryshort_notm}} of the same region.
{:shortdesc}

{{site.data.keyword.Bluemix_notm}} regions can be accessed by specifying the API endpoint when you log in. If you do not specify a region, you are automatically logged in to the region that is closest to you.

{{site.data.keyword.Bluemix_notm}} region API endpoints:
  * US South and US East: `api.ng.bluemix.net`
  * Sydney: `api.au-syd.bluemix.net`
  * Germany: `api.eu-de.bluemix.net`
  * United Kingdom: `api.eu-gb.bluemix.net`
<br>

Example commands to log in to {{site.data.keyword.Bluemix_notm}} regions:
  * US South and US East
      ```
      bx login -a api.ng.bluemix.net
      ```
      {: pre}

  * Sydney
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
  * US South: `https://us-south.containers.bluemix.net`
  * US East: `https://us-east.containers.bluemix.net`
  * UK South: `https://uk-south.containers.bluemix.net`
  * EU Central: `https://eu-central.containers.bluemix.net`
  * AP South: `https://ap-south.containers.bluemix.net`

**Note:** US East is available for use with CLI commands only.

### Logging in to a different container region
{: #container_login_endpoints}

You might want to log in to another {{site.data.keyword.containershort_notm}} region for the following reasons:
  * You created {{site.data.keyword.Bluemix_notm}} services or private Docker images in one region and want to use them with {{site.data.keyword.containershort_notm}} in another region.

  * You want to access a cluster in a region that is different from the default {{site.data.keyword.Bluemix_notm}} region you are logged in to.

</br>

Example commands to log in to a {{site.data.keyword.containershort_notm}} region:
  * US South:
    ```
    bx cs init --host https://us-south.containers.bluemix.net
    ```
    {: pre}

  * US East:
    ```
    bx cs init --host https://us-east.containers.bluemix.net
    ```
    {: pre}

  * UK South:
    ```
    bx cs init --host https://uk-south.containers.bluemix.net
    ```
    {: pre}

  * EU Central:
    ```
    bx cs init --host https://eu-central.containers.bluemix.net
    ```
    {: pre}

  * AP South:
    ```
    bx cs init --host https://ap-south.containers.bluemix.net
    ```
    {: pre}

### Creating lite clusters in container regions
{: #lite_regions}

You can only create Kubernetes lite clusters in the following regions:
  * US South
  * UK South
  * EU Central
  * AP South

### Locations in container regions
{: #locations}

Locations are data centers within a region.

  | Region | Location | City |
  |--------|----------|------|
  | US South     | dal10, dal12        | Dallas |
  | US East      | wdc06, wdc07        | Washington, DC |
  | UK South      | lon02, lon04         | London |
  | EU Central     | ams03, fra02        | Amsterdam, Frankfurt |
  | AP South     | syd01, syd02        | Sydney |

### Using container API commands
{: #container_api}

To interact with the {{site.data.keyword.containershort_notm}} API, enter the command type and append `/v1/command` to the endpoint.

Example of `GET /clusters` API in US South:
  ```
  GET https://us-south.containers.bluemix.net/v1/clusters
  ```
  {: codeblock}

</br>

To view documentation on the API commands, append `swagger-api` to the endpoint for the region you want to view. For example:
  * EU Central: https://eu-central.containers.bluemix.net/swagger-api/

