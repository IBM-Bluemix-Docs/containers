---

copyright:
  years: 2014, 2017
lastupdated: "2017-09-08"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:codeblock: .codeblock}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip} 
{:download: .download}


# Setting up clusters
{: #cs_cluster}

Design your cluster setup for maximum availability and capacity.
{:shortdesc}

Before you begin, review the options for [highly available cluster configurations](cs_planning.html#cs_planning_cluster_config).

![Stages of high availability for a cluster](images/cs_cluster_ha_roadmap.png)](https://console.bluemix.net/docs/api/content/containers/images/cs_cluster_ha_roadmap.png)

## Creating clusters with the GUI
{: #cs_cluster_ui}

A Kubernetes cluster is a set of worker nodes that are organized into a network. The purpose of the cluster is to define a set of resources, nodes, networks, and storage devices that keep applications highly available. Before you can deploy an app, you must create a cluster and set the definitions for the worker nodes in that cluster.
{:shortdesc}

For {{site.data.keyword.Bluemix_notm}} Dedicated users, see [Creating Kubernetes clusters from the GUI in {{site.data.keyword.Bluemix_notm}} Dedicated (Closed Beta)](#creating_ui_dedicated) instead.

To create a cluster:
1.  From the catalog, select **Containers** and click **Kubernetes cluster**.

2.  For the **Cluster type**, select **Standard**. With a standard cluster, you get features like multiple worker nodes for a highly available environment.
3.  Enter a **Cluster Name**.
4.  Select a {{site.data.keyword.Bluemix_notm}} **Location** in which to deploy your cluster. The locations that are available to you depend on the {{site.data.keyword.Bluemix_notm}} region that you are logged in to. Select the region that is physically closest to you for best performance. When you select a location that is located outside your country, keep in mind that you might require legal authorization before data can be physically stored in a foreign country. The {{site.data.keyword.Bluemix_notm}} region determines the container registry that you can use and the {{site.data.keyword.Bluemix_notm}} services that are available to you.
5.  Select a **Machine type**. The machine type defines the amount of virtual CPU and memory that is set up in each worker node and that is available for all the containers that you deploy in your nodes.
    -   The micro machine type indicates the smallest option.
    -   A balanced machine type has an equal amount of memory that is assigned to each CPU, which optimizes performance.
6.  Choose the **Number of worker nodes** that you need. Select 3 for higher availability of your cluster.
7.  Select a **Private VLAN** from your {{site.data.keyword.BluSoftlayer_full}} account. A private VLAN is used to communicate between worker nodes. You can use the same private VLAN for multiple clusters.
8. Select a **Public VLAN** from your {{site.data.keyword.BluSoftlayer_notm}} account. A public VLAN is used to communicate between worker nodes and the IBM-managed Kubernetes master. You can use the same public VLAN for multiple clusters. If you choose not to select a Public VLAN, you must configure an alternative solution.
9. For **Hardware**, choose **Dedicated** or **Shared**. **Shared** is a sufficient option for most situations.
    -   **Dedicated**: Ensure complete isolation of your physical resources from other IBM customers.
    -   **Shared**: Allow IBM to store your physical resources on the same hardware as other IBM customers.
10. Click **Create Cluster**. The details for the cluster open, but the worker nodes in the cluster take a few minutes to provision. In the **Worker nodes** tab, you can see the progress of the worker node deployment. When the worker nodes are ready, the state changes to **Ready**.

    **Note:** Every worker node is assigned a unique worker node ID and domain name that must not be manually changed after the cluster is created. Changing the ID or domain name prevents the Kubernetes master from managing your cluster.


**What's next?**

When the cluster is up and running, you can check out the following tasks:

-   [Install the CLIs to start working with your cluster.](cs_cli_install.html#cs_cli_install)
-   [Deploy an app in your cluster.](cs_apps.html#cs_apps_cli)
-   [Set up your own private registry in {{site.data.keyword.Bluemix_notm}} to store and share Docker images with other users.](/docs/services/Registry/index.html)

### Creating clusters with the GUI in {{site.data.keyword.Bluemix_notm}} Dedicated (Closed Beta)
{: #creating_ui_dedicated}

1.  Log in to {{site.data.keyword.Bluemix_notm}} Public console ([https://console.bluemix.net ![External link icon](../icons/launch-glyph.svg "External link icon")](https://console.bluemix.net)) with your IBMID.
2.  From the account menu, select your {{site.data.keyword.Bluemix_notm}} Dedicated account. The console is updated with the services and information for your {{site.data.keyword.Bluemix_notm}} Dedicated instance.
3.  From the catalog, select **Containers** and click **Kubernetes cluster**.
4.  Enter a **Cluster Name**.
5.  Select a **Machine type**. The machine type defines the amount of virtual CPU and memory that is set up in each worker node and that is available for all the containers that you deploy in your nodes.
    -   The micro machine type indicates the smallest option.
    -   A balanced machine type has an equal amount of memory assigned to each CPU, which optimizes performance.
6.  Choose the **Number of worker nodes** that you need. Select 3 to ensure high availability of your cluster.
7.  Click **Create Cluster**. The details for the cluster open, but the worker nodes in the cluster take a few minutes to provision. In the **Worker nodes** tab, you can see the progress of the worker node deployment. When the worker nodes are ready, the state changes to **Ready**.

**What's next?**

When the cluster is up and running, you can check out the following tasks:

-   [Install the CLIs to start working with your cluster.](cs_cli_install.html#cs_cli_install)
-   [Deploy an app in your cluster.](cs_apps.html#cs_apps_cli)
-   [Set up your own private registry in {{site.data.keyword.Bluemix_notm}} to store and share Docker images with other users.](/docs/services/Registry/index.html)

## Creating clusters with the CLI
{: #cs_cluster_cli}

A cluster is a set of worker nodes that are organized into a network. The purpose of the cluster is to define a set of resources, nodes, networks, and storage devices that keep applications highly available. Before you can deploy an app, you must create a cluster and set the definitions for the worker nodes in that cluster.
{:shortdesc}

For {{site.data.keyword.Bluemix_notm}} Dedicated users, see [Creating Kubernetes clusters from the CLI in {{site.data.keyword.Bluemix_notm}} Dedicated (Closed Beta)](#creating_cli_dedicated) instead.

To create a cluster:
1.  Install the {{site.data.keyword.Bluemix_notm}} CLI and the [{{site.data.keyword.containershort_notm}} plug-in](cs_cli_install.html#cs_cli_install).
2.  Log in to the {{site.data.keyword.Bluemix_notm}} CLI. Enter your {{site.data.keyword.Bluemix_notm}} credentials when prompted.

    ```
    bx login
    ```
    {: pre}

      To specify a specific {{site.data.keyword.Bluemix_notm}} region, include the API endpoint. If you have private Docker images that are stored in the container registry of a specific {{site.data.keyword.Bluemix_notm}} region, or {{site.data.keyword.Bluemix_notm}} services instances that you already created, log in to this region to access your images and {{site.data.keyword.Bluemix_notm}} services.

      The {{site.data.keyword.Bluemix_notm}} region that you log in to also determines the region where you can create your Kubernetes clusters, including the available data centers. If you do not specify a region, you are automatically logged in to the region that is closest to you.

       -  US South

           ```
           bx login -a api.ng.bluemix.net
           ```
           {: pre}
     
       -  Sydney

           ```
           bx login -a api.au-syd.bluemix.net
           ```
           {: pre}

       -  Germany

           ```
           bx login -a api.eu-de.bluemix.net
           ```
           {: pre}

       -  United Kingdom

           ```
           bx login -a api.eu-gb.bluemix.net
           ```
           {: pre}

      **Note:** If you have a federated ID, use `bx login --sso` to log in to the {{site.data.keyword.Bluemix_notm}} CLI. Enter your user name and use the provided URL in your CLI output to retrieve your one-time passcode. You know you have a federated ID when the login fails without the `--sso` and succeeds with the `--sso` option.

3.  If you are assigned to multiple {{site.data.keyword.Bluemix_notm}} accounts, organizations and spaces, select the account where you want to create your Kubernetes cluster. Clusters are specific to an account and an organization, but are independent from a {{site.data.keyword.Bluemix_notm}} space. Therefore, if you have access to multiple spaces in your organization, you can select any space from the list.
4.  Optional: If you want to create or access Kubernetes clusters in a region other than the {{site.data.keyword.Bluemix_notm}} region that you selected earlier, specify this region. For example, you might want to log in to another {{site.data.keyword.containershort_notm}} region for the following reasons:

    -   You created {{site.data.keyword.Bluemix_notm}} services or private Docker images in one region and want to use them with {{site.data.keyword.containershort_notm}} in another region.
    -   You want to access a cluster in a region that is different from the default {{site.data.keyword.Bluemix_notm}} region you are logged in to.
    
    Choose between the following API endpoints:

    -   US-South:

        ```
        bx cs init --host https://us-south.containers.bluemix.net
        ```
        {: pre}

    -   UK-South:

        ```
        bx cs init --host https://uk-south.containers.bluemix.net
        ```
        {: pre}

    -   EU-Central:

        ```
        bx cs init --host https://eu-central.containers.bluemix.net
        ```
        {: pre}

    -   AP-South:

        ```
        bx cs init --host https://ap-south.containers.bluemix.net
        ```
        {: pre}
    
6.  Create a cluster.
    1.  Review the locations that are available. The locations that are shown depend on the {{site.data.keyword.containershort_notm}} region that you are logged in.

        ```
        bx cs locations
        ```
        {: pre}

        Your CLI output looks similar to the following:

        -   US-South:

            ```
            dal10
            dal12
            ```
            {: screen}

        -   UK-South:

            ```
            lon02
            lon04
            ```
            {: screen}

        -   EU-Central:

            ```
            ams03
            fra02
            ```
            {: screen}

        -   AP-South

            ```
            syd01
            syd02
            ```
            {: screen}

    2.  Choose a location and review the machine types available in that location. The machine type specifies the virtual compute resources that are available to each worker node.

        ```
        bx cs machine-types <location>
        ```
        {: pre}

        ```
        Getting machine types list...
        OK
        Machine Types
        Name         Cores   Memory   Network Speed   OS             Storage   Server Type   
        u1c.2x4      2       4GB      1000Mbps        UBUNTU_16_64   100GB     virtual   
        b1c.4x16     4       16GB     1000Mbps        UBUNTU_16_64   100GB     virtual   
        b1c.16x64    16      64GB     1000Mbps        UBUNTU_16_64   100GB     virtual   
        b1c.32x128   32      128GB    1000Mbps        UBUNTU_16_64   100GB     virtual   
        b1c.56x242   56      242GB    1000Mbps        UBUNTU_16_64   100GB     virtual 
        ```
        {: screen}

    3.  Check to see if a public and private VLAN already exists in the {{site.data.keyword.BluSoftlayer_notm}} for this account.

        ```
        bx cs vlans <location>
        ```
        {: pre}

        ```
        ID        Name                Number   Type      Router  
        1519999   vlan   1355     private   bcr02a.dal10  
        1519898   vlan   1357     private   bcr02a.dal10 
        1518787   vlan   1252     public   fcr02a.dal10 
        1518888   vlan   1254     public    fcr02a.dal10 
        ```
        {: screen}

        If a public and private VLAN already exists, note the matching routers. Private VLAN routers always begin with `bcr` (back-end router) and public VLAN routers always begin with `fcr` (front-end router). The number and letter combination after those prefixes must match to use those VLANs when creating a cluster. In the example output, any of the private VLANs can be used with any of public VLANs because the routers all include `02a.dal10`.

    4.  Run the `cluster-create` command. You can choose between a lite cluster, which includes one worker node set up with 2vCPU and 4GB memory, or a standard cluster, which can include as many worker nodes as you choose in your {{site.data.keyword.BluSoftlayer_notm}} account. When you create a standard cluster, by default, the hardware of the worker node is shared by multiple IBM customers and billed by hours of usage. </br>Example for a standard cluster:

        ```
        bx cs cluster-create --location dal10 --public-vlan <public_vlan_id> --private-vlan <private_vlan_id> --machine-type u1c.2x4 --workers 3 --name <cluster_name>
        ```
        {: pre}

        Example for a lite cluster:

        ```
        bx cs cluster-create --name my_cluster
        ```
        {: pre}

        <table>
        <caption>Table 1. Understanding this command's components</caption>
        <thead>
        <th colspan=2><img src="images/idea.png"/> Understanding this command's components</th>
        </thead>
        <tbody>
        <tr>
        <td><code>cluster-create</code></td>
        <td>The command to create a cluster in your {{site.data.keyword.Bluemix_notm}} organization.</td> 
        </tr>
        <tr>
        <td><code>--location <em>&lt;location&gt;</em></code></td>
        <td>Replace <em>&lt;location&gt;</em> with the {{site.data.keyword.Bluemix_notm}} location ID where you want to create your cluster. The locations that are available to you depend on the {{site.data.keyword.containershort_notm}} region you are logged in to. Available locations are:<ul><li>US-South<ul><li>dal10 [Dallas]</li><li>dal12 [Dallas]</li></ul></li><li>UK-South<ul><li>lon02 [London]</li><li>lon04 [London]</li></ul></li><li>EU-Central<ul><li>ams03 [Amsterdam]</li><li>ra02 [Frankfurt]</li></ul></li><li>AP-South<ul><li>syd01 [Sydney]</li><li>syd04 [Sydney]</li></ul></li></ul></td> 
        </tr>
        <tr>
        <td><code>--machine-type <em>&lt;machine_type&gt;</em></code></td>
        <td>If you are creating a standard cluster, choose a machine type. The machine type specifies the virtual compute resources that are available to each worker node. Review [Comparison of lite and standard clusters for {{site.data.keyword.containershort_notm}}](cs_planning.html#cs_planning_cluster_type) for more information. For lite clusters, you do not have to define the machine type.</td> 
        </tr>
        <tr>
        <td><code>--public-vlan <em>&lt;public_vlan_id&gt;</em></code></td>
        <td><ul><li>For lite clusters, you do not have to define a public VLAN. Your lite cluster is automatically connected to a public VLAN that is owned by IBM.</li><li>For a standard cluster, if you already have a public VLAN set up in your {{site.data.keyword.BluSoftlayer_notm}} account for that location, enter the ID of the public VLAN. Otherwise, you do not have to specify this option because {{site.data.keyword.containershort_notm}} automatically creates a public VLAN for you. <br/><br/><strong>Note</strong>: The public and private VLANs that you specify with the create command must match. Private VLAN routers always begin with <code>bcr</code> (back-end router) and public VLAN routers always begin with <code>fcr</code> (front-end router). The number and letter combination after those prefixes must match to use those VLANs when creating a cluster. Do not use public and private VLANs that do not match to create a cluster.</li></ul></td> 
        </tr>
        <tr>
        <td><code>--private-vlan <em>&lt;private_vlan_id&gt;</em></code></td>
        <td><ul><li>For lite clusters, you do not have to define a private VLAN. Your lite cluster is automatically connected to a private VLAN that is owned by IBM.</li><li>For a standard cluster, if you already have a private VLAN set up in your {{site.data.keyword.BluSoftlayer_notm}} account for that location, enter the ID of the private VLAN. Otherwise, you do not have to specify this option because {{site.data.keyword.containershort_notm}} automatically creates a private VLAN for you. <br/><br/><strong>Note</strong>: The public and private VLANs that you specify with the create command must match. Private VLAN routers always begin with <code>bcr</code> (back-end router) and public VLAN routers always begin with <code>fcr</code> (front-end router). The number and letter combination after those prefixes must match to use those VLANs when creating a cluster. Do not use public and private VLANs that do not match to create a cluster.</li></ul></td> 
        </tr>
        <tr>
        <td><code>--name <em>&lt;name&gt;</em></code></td>
        <td>Replace <em>&lt;name&gt;</em> with a name for your cluster.</td> 
        </tr>
        <tr>
        <td><code>--workers <em>&lt;number&gt;</em></code></td>
        <td>The number of worker nodes to include in the cluster. If the <code>--workers</code> option is not specified, 1 worker node is created.</td> 
        </tr>
        </tbody></table>

7.  Verify that the creation of the cluster was requested.

    ```
    bx cs clusters
    ```
    {: pre}

    **Note:** It can take up to 15 minutes for the worker node machines to be ordered, and for the cluster to be set up and provisioned in your account.

    When the provisioning of your cluster is completed, the status of your cluster changes to **deployed**.

    ```
    Name         ID                                   State      Created          Workers   
    my_cluster   paf97e8843e29941b49c598f516de72101   deployed   20170201162433   1   
    ```
    {: screen}

8.  Check the status of the worker nodes.

    ```
    bx cs workers <cluster>
    ```
    {: pre}

    When the worker nodes are ready, the state changes to **normal** and the status is **Ready**. When the node status is **Ready**, you can then access the cluster.

    **Note:** Every worker node is assigned a unique worker node ID and domain name that must not be manually changed after the cluster is created. Changing the ID or domain name prevents the Kubernetes master from managing your cluster.

    ```
    ID                                                  Public IP        Private IP     Machine Type   State      Status  
    prod-dal10-pa8dfcc5223804439c87489886dbbc9c07-w1   169.47.223.113   10.171.42.93   free           normal    Ready
    ```
    {: screen}

9. Set the cluster you created as the context for this session. Complete these configuration steps every time that you work with your cluster.
    1.  Get the command to set the environment variable and download the Kubernetes configuration files.

        ```
        bx cs cluster-config <cluster_name_or_id>
        ```
        {: pre}

        When the download of the configuration files is finished, a command is displayed that you can use to set the path to the local Kubernetes configuration file as an environment variable.

        Example for OS X:

        ```
        export KUBECONFIG=/Users/<user_name>/.bluemix/plugins/container-service/clusters/<cluster_name>/kube-config-prod-dal10-<cluster_name>.yml
        ```
        {: screen}

    2.  Copy and paste the command that is displayed in your terminal to set the `KUBECONFIG` environment variable.
    3.  Verify that the `KUBECONFIG` environment variable is set properly.

        Example for OS X:

        ```
        echo $KUBECONFIG
        ```
        {: pre}

        Output:

        ```
        /Users/<user_name>/.bluemix/plugins/container-service/clusters/<cluster_name>/kube-config-prod-dal10-<cluster_name>.yml
        
        ```
        {: screen}

10. Launch your Kubernetes dashboard with the default port 8001.
    1.  Set the proxy with the default port number.

        ```
        kubectl proxy
        ```
        {: pre}

        ```
        Starting to serve on 127.0.0.1:8001
        ```
        {: screen}

    2.  Open the following URL in a web browser to see the Kubernetes dashboard.

        ```
        http://localhost:8001/ui
        ```
        {: codeblock}


**What's next?**

-   [Deploy an app in your cluster.](cs_apps.html#cs_apps_cli)
-   [Manage your cluster with the `kubectl` command line. ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/user-guide/kubectl/)
-   [Set up your own private registry in {{site.data.keyword.Bluemix_notm}} to store and share Docker images with other users.](/docs/services/Registry/index.html)

### Creating clusters with the CLI in {{site.data.keyword.Bluemix_notm}} Dedicated (Closed Beta)
{: #creating_cli_dedicated}

1.  Install the {{site.data.keyword.Bluemix_notm}} CLI and the [{{site.data.keyword.containershort_notm}} plug-in](cs_cli_install.html#cs_cli_install).
2.  Log in to the public endpoint for {{site.data.keyword.containershort_notm}}. Enter your {{site.data.keyword.Bluemix_notm}} credentials and select the {{site.data.keyword.Bluemix_notm}} Dedicated account when prompted.

    ```
    bx login -a api.<region>.bluemix.net
    ```
    {: pre}

    **Note:** If you have a federated ID, use `bx login --sso` to log in to the {{site.data.keyword.Bluemix_notm}} CLI. Enter your user name and use the provided URL in your CLI output to retrieve your one-time passcode. You know you have a federated ID when the login fails without the `--sso` and succeeds with the `--sso` option.

3.  Create a cluster with the `cluster-create` command. When you create a standard cluster, the hardware of the worker node is billed by hours of usage.

    Example

    ```
    bx cs cluster-create --machine-type <machine-type> --workers <number> --name <cluster_name>
    ```
    {: pre}
    
    <table>
    <caption>Table 2. Understanding this command's components</caption>
    <thead>
    <th colspan=2><img src="images/idea.png"/> Understanding this command's components</th>
    </thead>
    <tbody>
    <tr>
    <td><code>cluster-create</code></td>
    <td>The command to create a cluster in your {{site.data.keyword.Bluemix_notm}} organization.</td> 
    </tr>
    <tr>
    <td><code>--location <em>&lt;location&gt;</em></code></td>
    <td>Replace &lt;location&gt; with the {{site.data.keyword.Bluemix_notm}} location ID where you want to create your cluster. The locations that are available to you depend on the {{site.data.keyword.containershort_notm}} region you are logged in to. Available locations are:<ul><li>US-South<ul><li>dal10 [Dallas]</li><li>dal12 [Dallas]</li></ul></li><li>UK-South<ul><li>lon02 [London]</li><li>lon04 [London]</li></ul></li><li>EU-Central<ul><li>ams03 [Amsterdam]</li><li>ra02 [Frankfurt]</li></ul></li><li>AP-South<ul><li>syd01 [Sydney]</li><li>syd04 [Sydney]</li></ul></li></ul></td> 
    </tr>
    <tr>
    <td><code>--machine-type <em>&lt;machine_type&gt;</em></code></td>
    <td>If you are creating a standard cluster, choose a machine type. The machine type specifies the virtual compute resources that are available to each worker node. Review [Comparison of lite and standard clusters for {{site.data.keyword.containershort_notm}}](cs_planning.html#cs_planning_cluster_type) for more information. For lite clusters, you do not have to define the machine type.</td> 
    </tr>
    <tr>
    <td><code>--name <em>&lt;name&gt;</em></code></td>
    <td>Replace <em>&lt;name&gt;</em> with a name for your cluster.</td> 
    </tr>
    <tr>
    <td><code>--workers <em>&lt;number&gt;</em></code></td>
    <td>The number of worker nodes to include in the cluster. If the <code>--workers</code> option is not specified, 1 worker node is created.</td> 
    </tr>
    </tbody></table>

4.  Verify that the creation of the cluster was requested.

    ```
    bx cs clusters
    ```
    {: pre}

    **Note:** It can take up to 15 minutes for the worker node machines to be ordered, and for the cluster to be set up and provisioned in your account.

    When the provisioning of your cluster is completed, the status of your cluster changes to **deployed**.

    ```
    Name         ID                                   State      Created          Workers   
    my_cluster   paf97e8843e29941b49c598f516de72101   deployed   20170201162433   1   
    ```
    {: screen}

5.  Check the status of the worker nodes.

    ```
    bx cs workers <cluster>
    ```
    {: pre}

    When the worker nodes are ready, the state changes to **normal** and the status is **Ready**. When the node status is **Ready**, you can then access the cluster.

    ```
    ID                                                  Public IP        Private IP     Machine Type   State      Status  
    prod-dal10-pa8dfcc5223804439c87489886dbbc9c07-w1   169.47.223.113   10.171.42.93   free           normal    Ready
    ```
    {: screen}

6.  Set the cluster you created as the context for this session. Complete these configuration steps every time that you work with your cluster.

    1.  Get the command to set the environment variable and download the Kubernetes configuration files.

        ```
        bx cs cluster-config <cluster_name_or_id>
        ```
        {: pre}

        When the download of the configuration files is finished, a command is displayed that you can use to set the path to the local Kubernetes configuration file as an environment variable.

        Example for OS X:

        ```
        export KUBECONFIG=/Users/<user_name>/.bluemix/plugins/container-service/clusters/<cluster_name>/kube-config-prod-dal10-<cluster_name>.yml
        ```
        {: screen}

    2.  Copy and paste the command that is displayed in your terminal to set the `KUBECONFIG` environment variable.
    3.  Verify that the `KUBECONFIG` environment variable is set properly.

        Example for OS X:

        ```
        echo $KUBECONFIG
        ```
        {: pre}

        Output:

        ```
        /Users/<user_name>/.bluemix/plugins/container-service/clusters/<cluster_name>/kube-config-prod-dal10-<cluster_name>.yml
        
        ```
        {: screen}

7.  Access your Kubernetes dashboard with the default port 8001.
    1.  Set the proxy with the default port number.

        ```
        kubectl proxy
        ```
        {: pre}

        ```
        Starting to serve on 127.0.0.1:8001
        ```
        {: screen}

    2.  Open the following URL in a web browser in order to see the Kubernetes dashboard.

        ```
        http://localhost:8001/ui
        ```
        {: codeblock}


**What's next?**

-   [Deploy an app in your cluster.](cs_apps.html#cs_apps_cli)
-   [Manage your cluster with the `kubectl` command line. ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/user-guide/kubectl/)
-   [Set up your own private registry in {{site.data.keyword.Bluemix_notm}} to store and share Docker images with other users.](/docs/services/Registry/index.html)

## Using private and public image registries
{: #cs_apps_images}

A Docker image is the basis for every container that you create. An image is created from a Dockerfile, which is a file that contains instructions to build the image. A Dockerfile might reference build artifacts in its instructions that are stored separately, such as an app, the app's configuration, and its dependencies. Images are typically stored in a registry that can either be accessible by the public (public registry) or set up with limited access for a small group of users (private registry).
{:shortdesc}

Review the following options to find information about how to set up an image registry and how to use an image from the registry.

-   [Accessing a namespace in {{site.data.keyword.registryshort_notm}} to work with IBM-provided images and your own private Docker images](#bx_registry_default).
-   [Accessing public images from Docker Hub](#dockerhub).
-   [Accessing private images that are stored in other private registries](#private_registry).

### Accessing a namespace in {{site.data.keyword.registryshort_notm}} to work with IBM-provided images and your own private Docker images
{: #bx_registry_default}

You can deploy containers to your cluster from an IBM-provided public image or a private image that is stored in your namespace in {{site.data.keyword.registryshort_notm}}.

Before you begin:

1. [Set up a namespace in {{site.data.keyword.registryshort_notm}} on {{site.data.keyword.Bluemix_notm}} Public or {{site.data.keyword.Bluemix_notm}} Dedicated and push images to this namespace](/docs/services/Registry/registry_setup_cli_namespace.html#registry_namespace_add).
2. [Create a cluster](#cs_cluster_cli).
3. [Target your CLI to your cluster](cs_cli_install.html#cs_cli_configure).

When you create a cluster, a non-expiring registry token is automatically created for the cluster. This token is used to authorize read-only access to any of your namespaces that you set up in {{site.data.keyword.registryshort_notm}} so that you can work with IBM-provided public and your own private Docker images. Tokens must be stored in a Kubernetes `imagePullSecret` so that they are accessible to a Kubernetes cluster when you deploy a containerized app. When your cluster is created, {{site.data.keyword.containershort_notm}} automatically stores this token in a Kubernetes `imagePullSecret`. The `imagePullSecret` is added to the default Kubernetes namespace, the default list of secrets in the ServiceAccount for that namespace, and the kube-system namespace.

**Note:** By using this initial setup, you can deploy containers from any image that is available in a namespace in your {{site.data.keyword.Bluemix_notm}} account into the **default** namespace of your cluster. If you want to deploy a container into other namespaces of your cluster, or if you want to use an image that is stored in another {{site.data.keyword.Bluemix_notm}} region or in another {{site.data.keyword.Bluemix_notm}} account, you must [create your own imagePullSecret for your cluster](#bx_registry_other).

To deploy a container into the **default** namespace of your cluster, create a deployment configuration script.

1.  Open your preferred editor and create a deployment configuration script that is named <em>mydeployment.yaml</em>.
2.  Define the deployment and the image that you want to use from your namespace in {{site.data.keyword.registryshort_notm}}.

    To use a private image from a namespace in {{site.data.keyword.registryshort_notm}}:

    ```
    apiVersion: extensions/v1beta1
    kind: Deployment
    metadata:
      name: ibmliberty-deployment
    spec:
      replicas: 3
      template:
        metadata:
          labels:
            app: ibmliberty
        spec:
          containers:
          - name: ibmliberty
            image: registry.<region>.bluemix.net/<namespace>/<my_image>:<tag>
    ```
    {: codeblock}

    **Tip:** To retrieve your namespace information, run `bx cr namespace-list`.

3.  Create the deployment in your cluster.

    ```
    kubectl apply -f mydeployment.yaml
    ```
    {: pre}

    **Tip:** You can also deploy an existing configuration script, such as one of the IBM-provided public images. This example uses the **ibmliberty** image in the US-South region.

    ```
    kubectl apply -f https://raw.githubusercontent.com/IBM-{{site.data.keyword.Bluemix_notm}}/kube-samples/master/deploy-apps-clusters/deploy-ibmliberty.yaml
    ```
    {: pre}

### Deploying images to other Kubernetes namespaces or accessing images in other {{site.data.keyword.Bluemix_notm}} regions and accounts
{: #bx_registry_other}

You can deploy containers to other Kubernetes namespaces, use images that are stored in other {{site.data.keyword.Bluemix_notm}} regions or accounts, or use images that are stored in {{site.data.keyword.Bluemix_notm}} Dedicated by creating your own imagePullSecret.

Before you begin:

1.  [Set up a namespace in {{site.data.keyword.registryshort_notm}} on {{site.data.keyword.Bluemix_notm}} Public or {{site.data.keyword.Bluemix_notm}} Dedicated and push images to this namespace](/docs/services/Registry/registry_setup_cli_namespace.html#registry_namespace_add).
2.  [Create a cluster](#cs_cluster_cli).
3.  [Target your CLI to your cluster](cs_cli_install.html#cs_cli_configure).

To create your own imagePullSecret:

**Note:** ImagePullSecrets are valid only for the Kubernetes namespaces that they were created for. Repeat these steps for every namespace where you want to deploy containers from a private image.

1.  If you do not have a token yet, [create a token for the registry that you want to access.](/docs/services/Registry/registry_tokens.html#registry_tokens_create)
2.  List available tokens in your {{site.data.keyword.Bluemix_notm}} account.

    ```
    bx cr token-list
    ```
    {: pre}

3.  Note down the token ID that you want to use.
4.  Retrieve the value for your token. Replace <em>&lt;token_id&gt;</em> with the ID of the token that you retrieved in the previous step.

    ```
    bx cr token-get <token_id>
    ```
    {: pre}

    Your token value is displayed in the **Token** field of your CLI output.

5.  Create the Kubernetes secret to store your token information.

    ```
    kubectl --namespace <kubernetes_namespace> create secret docker-registry <secret_name>  --docker-server=<registry_url> --docker-username=token --docker-password=<token_value> --docker-email=<docker_email>
    ```
    {: pre}
    
    <table>
    <caption>Table 3. Understanding this command's components</caption>
    <thead>
    <th colspan=2><img src="images/idea.png"/> Understanding this command's components</th>
    </thead>
    <tbody>
    <tr>
    <td><code>--namespace <em>&lt;kubernetes_namespace&gt;</em></code></td>
    <td>Required. The Kubernetes namespace of your cluster where you want to use the secret and deploy containers to. Run <code>kubectl get namespaces</code> to list all namespaces in your cluster.</td> 
    </tr>
    <tr>
    <td><code><em>&lt;secret_name&gt;</em></code></td>
    <td>Required. The name that you want to use for your imagePullSecret.</td> 
    </tr>
    <tr>
    <td><code>--docker-server <em>&lt;registry_url&gt;</em></code></td>
    <td>Required. The URL to the image registry where your namespace is set up.<ul><li>For namespaces that are set up in US-South registry.ng.bluemix.net</li><li>For namespaces that are set up in UK-South registry.eu-gb.bluemix.net</li><li>For namespaces that are set up in EU-Central (Frankfurt) registry.eu-de.bluemix.net</li><li>For namespaces that are set up in Australia (Sydney) registry.au-syd.bluemix.net</li><li>For namespaces that are set up in {{site.data.keyword.Bluemix_notm}} Dedicated registry.<em>&lt;dedicated_domain&gt;</em></li></ul></td> 
    </tr>
    <tr>
    <td><code>--docker-username <em>&lt;docker_username&gt;</em></code></td>
    <td>Required. The user name to log in to your private registry.</td> 
    </tr>
    <tr>
    <td><code>--docker-password <em>&lt;token_value&gt;</em></code></td>
    <td>Required. The value of your registry token that you retrieved earlier.</td> 
    </tr>
    <tr>
    <td><code>--docker-email <em>&lt;docker-email&gt;</em></code></td>
    <td>Required. If you have one, enter your Docker email address. If you do not have one, enter a fictional email address, as for example a@b.c. This email is mandatory to create a Kubernetes secret, but is not used after creation.</td> 
    </tr>
    </tbody></table>

6.  Verify that the secret was created successfully. Replace <em>&lt;kubernetes_namespace&gt;</em> with the name of the namespace where you created the imagePullSecret.

    ```
    kubectl get secrets --namespace <kubernetes_namespace>
    ```
    {: pre}

7.  Create a pod that references the imagePullSecret.
    1.  Open your preferred editor and create a pod configuration script that is named mypod.yaml.
    2.  Define the pod and the imagePullSecret that you want to use to access the private {{site.data.keyword.Bluemix_notm}} registry. To use a private image from a namespace:

        ```
        apiVersion: v1
        kind: Pod
        metadata:
          name: <pod_name>
        spec:
          containers:
            - name: <container_name>
              image: registry.<region>.bluemix.net/<my_namespace>/<my_image>:<tag>  
          imagePullSecrets:
            - name: <secret_name>
        ```
        {: codeblock}

        <table>
        <caption>Table 4. Understanding the YAML file components</caption>
        <thead>
        <th colspan=2><img src="images/idea.png"/> Understanding the YAML file components</th>
        </thead>
        <tbody>
        <tr>
        <td><code><em>&lt;container_name&gt;</em></code></td>
        <td>The name of the container that you want to deploy to your cluster.</td> 
        </tr>
        <tr>
        <td><code><em>&lt;secret_name&gt;</em></code></td>
        <td>The namespace where your image is stored. To list available namespaces, run `bx cr namespace-list`.</td> 
        </tr>
        <tr>
        <td><code><em>&lt;my_namespace&gt;</em></code></td>
        <td>The namespace where your image is stored. To list available namespaces, run `bx cr namespace-list`.</td> 
        </tr>
        <tr>
        <td><code><em>&lt;my_image&gt;</em></code></td>
        <td>The name of the image that you want to use. To list available images in a {{site.data.keyword.Bluemix_notm}} account, run `bx cr image-list`.</td> 
        </tr>
        <tr>
        <td><code><em>&lt;tag&gt;</em></code></td>
        <td>The version of the image that you want to use. If no tag is specified, the image that is tagged <strong>latest</strong> is used by default.</td> 
        </tr>
        <tr>
        <td><code><em>&lt;secret_name&gt;</em></code></td>
        <td>The name of the imagePullSecret that you created earlier.</td> 
        </tr>
        </tbody></table>

   3.  Save your changes.
   4.  Create the deployment in your cluster.

        ```
        kubectl apply -f mypod.yaml
        ```
        {: pre}


### Accessing public images from Docker Hub
{: #dockerhub}

You can use any public image that is stored in Docker Hub to deploy a container to your cluster without any additional configuration. Create a deployment configuration script file or deploy an existing one.

Before you begin:

1.  [Create a cluster](#cs_cluster_cli).
2.  [Target your CLI to your cluster](cs_cli_install.html#cs_cli_configure).

Create a deployment configuration script.

1.  Open your preferred editor and create a deployment configuration script that is named mydeployment.yaml.
2.  Define the deployment and the public image from Docker Hub that you want to use. The following configuration script uses the public NGINX image that is available on Docker Hub.

    ```
    apiVersion: extensions/v1beta1
    kind: Deployment
    metadata:
      name: nginx-deployment
    spec:
      replicas: 3
      template:
        metadata:
          labels:
            app: nginx
        spec:
          containers:
          - name: nginx
            image: nginx
    ```
    {: codeblock}

3.  Create the deployment in your cluster.

    ```
    kubectl apply -f mydeployment.yaml
    ```
    {: pre}

    **Tip:** Alternatively, deploy an existing configuration script. The following example uses the same public NGINX image, but applies it directly to your cluster.

    ```
    kubectl apply -f https://raw.githubusercontent.com/IBM-{{site.data.keyword.Bluemix_notm}}/kube-samples/master/deploy-apps-clusters/deploy-nginx.yaml
    ```
    {: pre}


### Accessing private images that are stored in other private registries
{: #private_registry}

If you already have a private registry that you want to use, you must store the registry credentials in a Kubernetes imagePullSecret and reference this secret in your configuration script.

Before you begin:

1.  [Create a cluster](#cs_cluster_cli).
2.  [Target your CLI to your cluster](cs_cli_install.html#cs_cli_configure).

To create an imagePullSecret, follow these steps.

**Note:** ImagePullSecrets are valid for the Kubernetes namespaces they were created for. Repeat these steps for every namespace where you want to deploy containers from an image in a private {{site.data.keyword.Bluemix_notm}} registry.

1.  Create the Kubernetes secret to store your private registry credentials.

    ```
    kubectl --namespace <kubernetes_namespace> create secret docker-registry <secret_name>  --docker-server=<registry_url> --docker-username=<docker_username> --docker-password=<docker_password> --docker-email=<docker_email>
    ```
    {: pre}
    
    <table>
    <caption>Table 5. Understanding this command's components</caption>
    <thead>
    <th colspan=2><img src="images/idea.png"/> Understanding this command's components</th>
    </thead>
    <tbody>
    <tr>
    <td><code>--namespace <em>&lt;kubernetes_namespace&gt;</em></code></td>
    <td>Required. The Kubernetes namespace of your cluster where you want to use the secret and deploy containers to. Run <code>kubectl get namespaces</code> to list all namespaces in your cluster.</td> 
    </tr>
    <tr>
    <td><code><em>&lt;secret_name&gt;</em></code></td>
    <td>Required. The name that you want to use for your imagePullSecret.</td> 
    </tr>
    <tr>
    <td><code>--docker-server <em>&lt;registry_url&gt;</em></code></td>
    <td>Required. The URL to the registry where your private images are stored.</td> 
    </tr>
    <tr>
    <td><code>--docker-username <em>&lt;docker_username&gt;</em></code></td>
    <td>Required. The user name to log in to your private registry.</td> 
    </tr>
    <tr>
    <td><code>--docker-password <em>&lt;token_value&gt;</em></code></td>
    <td>Required. The value of your registry token that you retrieved earlier.</td> 
    </tr>
    <tr>
    <td><code>--docker-email <em>&lt;docker-email&gt;</em></code></td>
    <td>Required. If you have one, enter your Docker email address. If you do not have one, enter a fictional email address, as for example a@b.c. This email is mandatory to create a Kubernetes secret, but is not used after creation.</td> 
    </tr>
    </tbody></table>

2.  Verify that the secret was created successfully. Replace <em>&lt;kubernetes_namespace&gt;</em> with the name of the namespace where you created the imagePullSecret.

    ```
    kubectl get secrets --namespace <kubernetes_namespace>
    ```
    {: pre}

3.  Create a pod that references the imagePullSecret.
    1.  Open your preferred editor and create a pod configuration script that is named mypod.yaml.
    2.  Define the pod and the imagePullSecret that you want to use to access the private {{site.data.keyword.Bluemix_notm}} registry. To use a private image from your private registry:

        ```
        apiVersion: v1
        kind: Pod
        metadata:
          name: <pod_name>
        spec:
          containers:
            - name: <container_name>
              image: <my_image>:<tag>  
          imagePullSecrets:
            - name: <secret_name>
        ```
        {: codeblock}

        <table>
        <caption>Table 6. Understanding the YAML file components</caption>
        <thead>
        <th colspan=2><img src="images/idea.png"/> Understanding the YAML file components</th>
        </thead>
        <tbody>
        <tr>
        <td><code><em>&lt;pod_name&gt;</em></code></td>
        <td>The name of the pod that you want to create.</td> 
        </tr>
        <tr>
        <td><code><em>&lt;container_name&gt;</em></code></td>
        <td>The name of the container that you want to deploy to your cluster.</td> 
        </tr>
        <tr>
        <td><code><em>&lt;my_image&gt;</em></code></td>
        <td>The full path to the image in your private registry that you want to use.</td> 
        </tr>
        <tr>
        <td><code><em>&lt;tag&gt;</em></code></td>
        <td>The version of the image that you want to use. If no tag is specified, the image that is tagged <strong>latest</strong> is used by default.</td> 
        </tr>
        <tr>
        <td><code><em>&lt;secret_name&gt;</em></code></td>
        <td>The name of the imagePullSecret that you created earlier.</td> 
        </tr>
        </tbody></table>

  3.  Save your changes.
  4.  Create the deployment in your cluster.

        ```
        kubectl apply -f mypod.yaml
        ```
        {: pre}


## Adding {{site.data.keyword.Bluemix_notm}} services to clusters
{: #cs_cluster_service}

Add an existing {{site.data.keyword.Bluemix_notm}} service instance to your cluster to enable your cluster users to access and use the {{site.data.keyword.Bluemix_notm}} service when they deploy an app to the cluster.
{:shortdesc}

Before you begin:

-   [Target your CLI](cs_cli_install.html#cs_cli_configure) to your cluster.
-   [Request an instance of the {{site.data.keyword.Bluemix_notm}} service](/docs/services/reqnsi.html#req_instance) in your space.
-   For {{site.data.keyword.Bluemix_notm}} Dedicated users, see [Adding {{site.data.keyword.Bluemix_notm}} services to clusters in {{site.data.keyword.Bluemix_notm}} Dedicated (Closed Beta)](#binding_dedicated) instead.

**Note:** 
- You can only add {{site.data.keyword.Bluemix_notm}} services that support service keys. If the service does not support service keys, see [Enabling external apps to use {{site.data.keyword.Bluemix_notm}} services](/docs/services/reqnsi.html#req_instance).
- The cluster and the worker nodes must be deployed fully before you can add a service.

To add a service:
2.  List all existing services in your {{site.data.keyword.Bluemix_notm}} space.

    ```
    bx service list
    ```
    {: pre}

    Example CLI output:

    ```
    name                      service           plan    bound apps   last operation   
    <service_instance_name>   <service_name>    spark                create succeeded
    ```
    {: screen}

3.  Note the **name** of the service instance that you want to add to your cluster.
4.  Identify the cluster namespace that you want to use to add your service. Choose between the following options.
    -   List existing namespaces and choose a namespace that you want to use.

        ```
        kubectl get namespaces
        ```
        {: pre}

    -   Create a new namespace in your cluster.

        ```
        kubectl create namespace <namespace_name>
        ```
        {: pre}

5.  Add the service to your cluster.

    ```
    bx cs cluster-service-bind <cluster_name_or_id> <namespace> <service_instance_name>
    ```
    {: pre}

    When the service is successfully added to your cluster, a cluster secret is created that holds the credentials of your service instance. Example CLI output:

    ```
    bx cs cluster-service-bind mycluster mynamespace cleardb 
    Binding service instance to namespace...
    OK
    Namespace: mynamespace
    Secret name:     binding-<service_instance_name>
    ```
    {: screen}

6.  Verify that the secret was created in your cluster namespace.

    ```
    kubectl get secrets --namespace=<namespace>
    ```
    {: pre}


To use the service in a pod that is deployed in the cluster, cluster users can access the service credentials of the {{site.data.keyword.Bluemix_notm}} service by [mounting the Kubernetes secret as a secret volume to a pod](cs_apps.html#cs_apps_service).

### Adding {{site.data.keyword.Bluemix_notm}} services to clusters in {{site.data.keyword.Bluemix_notm}} Dedicated (Closed Beta)
{: #binding_dedicated}

Before you begin, [request an instance of the {{site.data.keyword.Bluemix_notm}} service](/docs/services/reqnsi.html#req_instance) in your space to add to your cluster.

**Note**: The cluster and the worker nodes must be deployed fully before you can add a service.

1.  Log in to the {{site.data.keyword.Bluemix_notm}} Dedicated environment where the service instance was created.

    ```
    bx login -a api.<dedicated_domain>
    ```
    {: pre}

2.  List all existing services in your {{site.data.keyword.Bluemix_notm}} space.

    ```
    bx service list
    ```
    {: pre}

    Example CLI output:

    ```
    name                      service           plan    bound apps   last operation   
    <service_instance_name>   <service_name>    spark                create succeeded
    ```
    {: screen}

3.  Create a service credentials key that contains the confidential information about the service, such as the user name, password, and URL.

    ```
    bx service key-create <service_name> <service_key_name>
    ```
    {: pre}

4.  Use the service credentials key to create a JSON file on your computer that includes the confidential information about the service.

    ```
    bx service key-show <service_name> <service_key_name>| sed -n '/{/,/}/'p >> /filepath/<dedicated-service-key>.json
    ```
    {: pre}

5.  Log in to the public endpoint for {{site.data.keyword.containershort_notm}} and target your CLI to the cluster in your {{site.data.keyword.Bluemix_notm}} Dedicated environment.
    1.  Log in to the account by using the public endpoint for {{site.data.keyword.containershort_notm}}. Enter your {{site.data.keyword.Bluemix_notm}} credentials and select the {{site.data.keyword.Bluemix_notm}} Dedicated account when prompted.

        ```
        bx login -a api.ng.bluemix.net
        ```
        {: pre}

        **Note:** If you have a federated ID, use `bx login --sso` to log in to the {{site.data.keyword.Bluemix_notm}} CLI. Enter your user name and use the provided URL in your CLI output to retrieve your one-time passcode. You know you have a federated ID when the login fails without the `--sso` and succeeds with the `--sso` option.

    2.  Get a list of available clusters and identify the name of the cluster to target in your CLI.

        ```
        bx cs clusters
        ```
        {: pre}

    3.  Get the command to set the environment variable and download the Kubernetes configuration files.

        ```
        bx cs cluster-config <cluster_name_or_id>
        ```
        {: pre}

        When the download of the configuration files is finished, a command is displayed that you can use to set the path to the local Kubernetes configuration file as an environment variable.

        Example for OS X:

        ```
        export KUBECONFIG=/Users/<user_name>/.bluemix/plugins/container-service/clusters/<cluster_name>/kube-config-prod-dal10-<cluster_name>.yml
        ```
        {: screen}

    4.  Copy and paste the command that is displayed in your terminal to set the `KUBECONFIG` environment variable.
6.  Create a Kubernetes secret from the service credentials JSON file.

    ```
    kubectl create secret generic <secret_name> --from-file=/filepath/<dedicated-service-key>.json
    ```
    {: pre}

7.  Repeat these steps for each {{site.data.keyword.Bluemix_notm}} service you want to use.

The {{site.data.keyword.Bluemix_notm}} service is bound to the cluster and can be used by any pods that are deployed in that cluster. To use the service in a pod, cluster users can [mount the Kubernetes secret as a secret volume to the pod](cs_apps.html#cs_apps_service) to access the service credentials for the {{site.data.keyword.Bluemix_notm}} service.


## Managing cluster access
{: #cs_cluster_user}

You can grant access to your cluster to other users, so that they can access the cluster, manage the cluster, and deploy apps to the cluster.
{:shortdesc}

Every user that works with {{site.data.keyword.containershort_notm}} must be assigned a service-specific user role in Identity and Access Management that determines what actions this user can perform. Identity and Access Management differentiates between the following access permissions.

-   {{site.data.keyword.containershort_notm}} access policies

    Access policies determine the cluster management actions that you can perform on a cluster, such as creating or removing clusters, and adding or removing extra worker nodes.

<!-- If you want to prevent a user from deploying apps to a cluster or creating other Kubernetes resources, you must create RBAC policies for the cluster. -->

-   Cloud Foundry roles

    Every user must be assigned a Cloud Foundry user role. This role determines the actions that the user can perform on the {{site.data.keyword.Bluemix_notm}} account, such as inviting other users, or viewing the quota usage. To review the permissions of each role, see [Cloud Foundry roles](/docs/iam/users_roles.html#cfroles).

-   RBAC roles

    Every user who is assigned an {{site.data.keyword.containershort_notm}} access policy is automatically assigned an RBAC role. RBAC roles determine the actions that you can perform on Kubernetes resources inside the cluster. RBAC roles are set up for the default namespace only. The cluster administrator can add RBAC roles for other namespaces in the cluster. See [Using RBAC Authorization ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/admin/authorization/rbac/#api-overview) in the Kubernetes documentation for more information.


Choose between the following actions to proceed:

-   [View required access policies and permissions to work with clusters](#access_ov).
-   [View your current access policy](#view_access).
-   [Change the access policy of an existing user](#change_access).
-   [Add additional users to the {{site.data.keyword.Bluemix_notm}} account](#add_users).

### Overview of required {{site.data.keyword.containershort_notm}} access policies and permissions
{: #access_ov}

Review the access policies and permissions that you can grant to users in your {{site.data.keyword.Bluemix_notm}} account.

|Access policy|Cluster Management Permissions|Kubernetes Resource Permissions|
|-------------|------------------------------|-------------------------------|
|<ul><li>Role: Administrator</li><li>Service Instances: all current service instances</li></ul>|<ul><li>Create a lite or standard cluster</li><li>Set credentials for a {{site.data.keyword.Bluemix_notm}} account to access the {{site.data.keyword.BluSoftlayer_notm}} portfolio</li><li>Remove a cluster</li><li>Assign and change {{site.data.keyword.containershort_notm}} access policies for other existing users in this account.</li></ul><br/>This role inherits permissions from the Editor, Operator, and Viewer roles for all clusters in this account.|<ul><li>RBAC Role: cluster-admin</li><li>Read/write access to resources in every namespace</li><li>Create roles within a namespace</li></ul>|
|<ul><li>Role: Administrator</li><li>Service Instances: a specific cluster ID</li></ul>|<ul><li>Remove a specific cluster.</li></ul><br/>This role inherits permissions from the Editor, Operator, and Viewer roles for the selected cluster.|<ul><li>RBAC Role: cluster-admin</li><li>Read/write access to resources in every namespace</li><li>Create roles within a namespace</li><li>Access Kubernetes dashboard</li></ul>|
|<ul><li>Role: Operator</li><li>Service Instances: all current service instances/ a specific cluster ID</li></ul>|<ul><li>Add additional worker nodes to a cluster</li><li>Remove worker nodes from a cluster</li><li>Reboot a worker node</li><li>Reload a worker node</li><li>Add a subnet to a cluster</li></ul>|<ul><li>RBAC Role: admin</li><li>Read/write access to resources inside the default namespace but not to the namespace itself</li><li>Create roles within a namespace</li></ul>|
|<ul><li>Role: Editor</li><li>Service Instances: all current service instances a specific cluster ID</li></ul>|<ul><li>Bind a {{site.data.keyword.Bluemix_notm}} service to a cluster.</li><li>Unbind a {{site.data.keyword.Bluemix_notm}} service to a cluster.</li><li>Create a webhook.</li></ul><br/>Use this role for your app developers.|<ul><li>RBAC Role: edit</li><li>Read/write access to resources inside the default namespace</li></ul>|
|<ul><li>Role: Viewer</li><li>Service Instances: all current service instances/ a specific cluster ID</li></ul>|<ul><li>List a cluster</li><li>View details for a cluster</li></ul>|<ul><li>RBAC Role: view</li><li>Read access to resources inside the default namespace</li><li>No read access to Kubernetes secrets</li></ul>|
|<ul><li>Cloud Foundry organization role: Manager</li></ul>|<ul><li>Add additional users to a {{site.data.keyword.Bluemix_notm}} account</li></ul>||
|<ul><li>Cloud Foundry space role: Developer</li></ul>|<ul><li>Create {{site.data.keyword.Bluemix_notm}} service instances/li><li>Bind {{site.data.keyword.Bluemix_notm}} service instances to clusters</li></ul>||
{: caption="Table 7. Overview of required IBM Bluemix Container Service access policies and permissions" caption-side="top"}

### Verifying your {{site.data.keyword.containershort_notm}} access policy
{: #view_access}

You can review and verify your assigned access policy for {{site.data.keyword.containershort_notm}}. The access policy determines the cluster management actions that you can perform.

1.  Select the {{site.data.keyword.Bluemix_notm}} account where you want to verify your {{site.data.keyword.containershort_notm}} access policy.
2.  From the menu bar, click **Manage** > **Security** > **Identity and Access**. The **Users** window displays a list of users with their email addresses and current status for the selected account.
3.  Select the user for whom you want to check the access policy.
4.  In the **Service Policies** section, review the access policy for the user. To find detailed information about the actions that you can perform with this role, see [Overview of required {{site.data.keyword.containershort_notm}} access policies and permissions](#access_ov).
5.  Optional: [Change your current access policy](#change_access).

    **Note:** Only users with an assigned Administrator service policy for all resources in {{site.data.keyword.containershort_notm}} can change the access policy for an existing user. To add further users to a {{site.data.keyword.Bluemix_notm}} account, you must have the Manager Cloud Foundry role for the account. To find the ID of the {{site.data.keyword.Bluemix_notm}} account owner, run `bx iam accounts` and look for the **Owner User ID**.


### Changing the {{site.data.keyword.containershort_notm}} access policy for an existing user
{: #change_access}

You can change the access policy for an existing user to grant cluster management permissions for a cluster in your {{site.data.keyword.Bluemix_notm}} account.

Before you begin, [verify that you have been assigned the Administrator access policy](#view_access) for all resources in {{site.data.keyword.containershort_notm}}.

1.  Select the {{site.data.keyword.Bluemix_notm}} account where you want to change the {{site.data.keyword.containershort_notm}} access policy for an existing user.
2.  From the menu bar, click **Manage** > **Security** > **Identity and Access**. The **Users** window displays a list of users with their email addresses and current status for the selected account.
3.  Find the user for whom you want to change the access policy. If you do not find the user you are looking for, [invite this user to the {{site.data.keyword.Bluemix_notm}} account](#add_users).
4.  From the **Actions** tab, click **Assign policy**.
5.  From the **Service** drop-down list, select **{{site.data.keyword.containershort_notm}}**.
6.  From the **Roles** drop-down list, select the access policy that you want to assign.Selecting a role without any limitations on a specific region or cluster automatically applies this access policy to all clusters that were created in this account. If you want to limit the access to a certain cluster or region, select them from the **Service instance** and **Region** drop-down list. To find a list of supported actions per access policy, see [Overview of required {{site.data.keyword.containershort_notm}} access policies and permissions](#access_ov). To find the ID of a specific cluster, run `bx cs clusters`.
7.  Click **Assign Policy** to save your changes.

### Adding users to a {{site.data.keyword.Bluemix_notm}} account
{: #add_users}

You can add additional users to a {{site.data.keyword.Bluemix_notm}} account to grant access to your clusters.

Before you begin, verify that you have been assigned the Manager Cloud Foundry role for a {{site.data.keyword.Bluemix_notm}} account.

1.  Select the {{site.data.keyword.Bluemix_notm}} account where you want to add users.
2.  From the menu bar, click **Manage** > **Security** > **Identity and Access**. The Users window displays a list of users with their email addresses and current status for the selected account.
3.  Click **Invite users**.
4.  In **Email address or existing IBMid**, enter the email address of the user that you want to add to the {{site.data.keyword.Bluemix_notm}} account.
5.  In the **Access** section, expand **Identity and Access enabled services**.
6.  From the **Services** drop-down list, select **{{site.data.keyword.containershort_notm}}**.
7.  From the **Roles** drop-down list, select the access policy that you want to assign. Selecting a role without any limitations on a specific region or cluster automatically applies this access policy to all clusters that were created in this account. If you want to limit the access to a certain cluster or region, select them from the **Service instance** and **Region** drop-down list. To find a list of supported actions per access policy, see [Overview of required {{site.data.keyword.containershort_notm}} access policies and permissions](#access_ov). To find the ID of a specific cluster, run `bx cs clusters`.
8.  Expand the **Cloud Foundry access** section and select the {{site.data.keyword.Bluemix_notm}} organization from the **Organization** drop-down list to which you want to add the user.
9.  From the **Space Roles** drop-down list, select any role. Kubernetes clusters are independent from {{site.data.keyword.Bluemix_notm}} spaces. To allow this user to add additional users to a {{site.data.keyword.Bluemix_notm}} account, you must assign the user a Cloud Foundry **Org Role**. However, you can assign Cloud Foundry org roles in a later step only.
10. Click **Invite users**.
11. Optional: From the **Users** overview, in the **Actions** tab, select **Manage User**.
12. Optional: In the **Cloud Foundry roles** section, find the Cloud Foundry organization role that was granted to the user that you added in the previous steps.
13. Optional: From the **Actions** tab, select **Edit Organization Role**.
14. Optional: From the **Organization Roles** drop-down list, select **Manager**.
15. Optional: Click **Save Role**.



## Adding subnets to clusters
{: #cs_cluster_subnet}

Change the pool of available portable public IP addresses by adding subnets to your cluster.
{:shortdesc}

In {{site.data.keyword.containershort_notm}}, you can add stable, portable IPs for Kubernetes services by adding network subnets to the cluster. When you create a standard cluster, {{site.data.keyword.containershort_notm}} automatically provisions a portable public subnet and 5 IP addresses. Portable public IP addresses are static and do not change when a worker node, or even the cluster, is removed.

One of the portable public IP addresses is used for the [Ingress controller](cs_apps.html#cs_apps_public_ingress) that you can use to expose multiple apps in your cluster by using a public route. The remaining 4 portable public IP addresses can be used to expose single apps to the public by [creating a load balancer service](cs_apps.html#cs_apps_public_load_balancer).

**Note:** Portable public IP addresses are charged on a monthly basis. If you choose to remove portable public IP addresses after your cluster is provisioned, you still have to pay the monthly charge, even if you used them only for a short amount of time.

### Requesting additional subnets for your cluster
{: #add_subnet}

You can add stable, portable public IPs to the cluster by assigning subnets to the cluster.

For {{site.data.keyword.Bluemix_notm}} Dedicated users, instead of using this task, you must [open a support ticket](/docs/support/index.html#contacting-support) to create the subnet, and then use the [`bx cs cluster-subnet-add`](cs_cli_reference.html#cs_cluster_subnet_add) command to add the subnet to the cluster.

Before you begin, make sure that you can access the {{site.data.keyword.BluSoftlayer_notm}} portfolio through the {{site.data.keyword.Bluemix_notm}} GUI. To access the portfolio, you must set up or use an existing {{site.data.keyword.Bluemix_notm}} Pay-As-You-Go account.

1.  From the catalog, in the **Infrastructure** section, select **Network**.
2.  Select **Subnet/IPs** and click **Create**.
3.  From the **Select the type of subnet to add to this account** drop-down menu, select **Portable Public**.
4.  Select the number of IP addresses that you want to add from your portable subnet.

    **Note:** When you add portable public IP addresses for your subnet, 3 IP addresses are used to establish cluster-internal networking, so that you cannot use them for your Ingress controller or to create a load balancer service. For example, if you request 8 portable public IP addresses, you can use 5 of them to expose your apps to the public.

5.  Select the public VLAN where you want to route the portable public IP addresses to. You must select the public VLAN that an existing worker node is connected to. Review the public VLAN for a worker node.

    ```
    bx cs worker-get <worker_id>
    ```
    {: pre}

6.  Complete the questionnaire and click **Place order**.

    **Note:** Portable public IP addresses are charged on a monthly. If you choose to remove portable public IP addresses after you created them, you still must pay the monthly charge, even if you used them only part of the month.
<!-- removed conref to test bx login -->
7.  After the subnet is provisioned, make the subnet available to your Kubernetes cluster.
    1.  From the Infrastructure dashboard, select the subnet that you created and note the ID of the subnet.
    2.  Log in to the {{site.data.keyword.Bluemix_notm}} CLI.

        ```
        bx login
        ```
        {: pre}

        To specify a specific {{site.data.keyword.Bluemix_notm}} region, choose one of the following API endpoints:

       -  US South

           ```
           bx login -a api.ng.bluemix.net
           ```
           {: pre}
     
       -  Sydney

           ```
           bx login -a api.au-syd.bluemix.net
           ```
           {: pre}

       -  Germany

           ```
           bx login -a api.eu-de.bluemix.net
           ```
           {: pre}

       -  United Kingdom

           ```
           bx login -a api.eu-gb.bluemix.net
           ```
           {: pre}

    3.  List all clusters in your account and note the ID of the cluster where you want to make your subnet available.

        ```
        bx cs clusters
        ```
        {: pre}

    4.  Add the subnet to your cluster. When you make a subnet available to a cluster, a Kubernetes config map is created for you that includes all available portable public IP addresses that you can use. If no Ingress controller exists for your cluster, one portable public IP address is automatically used to create the Ingress controller. All other portable public IP addresses can be used to create load balancer services for your apps.

        ```
        bx cs cluster-subnet-add <cluster name or id> <subnet id>
        ```
        {: pre}

8.  Verify that the subnet was successfully added to your cluster. The cluster id is listed in the **Bound Cluster** column.

    ```
    bx cs subnets
    ```
    {: pre}

### Adding custom and existing subnets to Kubernetes clusters
{: #custom_subnet}

You can add existing portable public subnets to your Kubernetes cluster.

Before you begin, [target your CLI](cs_cli_install.html#cs_cli_configure) to your cluster.

If you have an existing subnet in your {{site.data.keyword.BluSoftlayer_notm}} portfolio with custom firewall rules or available IP addresses that you want to use, create a cluster with no subnet and make your existing subnet available to the cluster when the cluster provisions.

1.  Identify the subnet to use. Note the ID of the subnet and the VLAN ID. In this example, the subnet ID is 807861 and the VLAN ID is 1901230.

    ```
    bx cs subnets
    ```
    {: pre}

    ```
    Getting subnet list...
    OK
    ID        Network                                      Gateway                                   VLAN ID   Type      Bound Cluster   
    553242    203.0.113.0/24                               10.87.15.00                               1565280   private      
    807861    192.0.2.0/24                                 10.121.167.180                            1901230   public      
    
    ```
    {: screen}

2.  Confirm the location that the VLAN is located. In this example, the location is dal10.

    ```
    bx cs vlans dal10
    ```
    {: pre}

    ```
    Getting VLAN list...
    OK
    ID        Name                  Number   Type      Router   
    1900403   vlan                    1391     private   bcr01a.dal10   
    1901230   vlan                    1180     public   fcr02a.dal10 
    ```
    {: screen}

3.  Create a cluster by using the location and VLAN ID that you identified. Include the `--no-subnet` flag to prevent a new portable public IP subnet from being created automatically.

    ```
    bx cs cluster-create --location dal10 --machine-type u1c.2x4 --no-subnet --public-vlan 1901230 --private-vlan 1900403 --workers 3 --name my_cluster 
    ```
    {: pre}

4.  Verify that the creation of the cluster was requested.

    ```
    bx cs clusters
    ```
    {: pre}

    **Note:** It can take up to 15 minutes for the worker node machines to be ordered, and for the cluster to be set up and provisioned in your account.

    When the provisioning of your cluster is completed, the status of your cluster changes to **deployed**.

    ```
    Name         ID                                   State      Created          Workers   
    my_cluster   paf97e8843e29941b49c598f516de72101   deployed   20170201162433   3   
    ```
    {: screen}

5.  Check the status of the worker nodes.

    ```
    bx cs workers <cluster>
    ```
    {: pre}

    When the worker nodes are ready, the state changes to **normal** and the status is **Ready**. When the node status is **Ready**, you can then access the cluster.

    ```
    ID                                                  Public IP        Private IP     Machine Type   State      Status  
    prod-dal10-pa8dfcc5223804439c87489886dbbc9c07-w1   169.47.223.113   10.171.42.93   free           normal    Ready
    ```
    {: screen}

6.  Add the subnet to your cluster by specifying the subnet ID. When you make a subnet available to a cluster, a Kubernetes config map is created for you that includes all available portable public IP addresses that you can use. If no Ingress controller already exists for your cluster, one portable public IP address is automatically used to create the Ingress controller. All other portable public IP addresses can be used to create load balancer services for your apps.

    ```
    bx cs cluster-subnet-add mycluster 807861
    ```
    {: pre}


## Using existing NFS file shares in clusters
{: #cs_cluster_volume_create}

If you already have existing NFS file shares in your {{site.data.keyword.BluSoftlayer_notm}} account that you want to use with Kubernetes, you can do so by creating persistent volumes on your existing NFS file share. A persistent volume is a piece of actual hardware that serves as a Kubernetes cluster resource and can be consumed by the cluster user.
{:shortdesc}

Before you begin, make sure that you have an existing NFS file share that you can use to create your persistent volume.

[![Create persistent volumes and persistent volume claims](images/cs_cluster_pv_pvc.png)](https://console.bluemix.net/docs/api/content/containers/images/cs_cluster_pv_pvc.png)

Kubernetes differentiates between persistent volumes that represent the actual hardware and persistent volume claims that are requests for storage usually initiated by the cluster user. When you want to enable existing NFS file shares to be used with Kubernetes, you must create persistent volumes with a certain size and access mode and create a persistent volume claim that matches the persistent volume specification. If persistent volume and persistent volume claim match, they are bound to each other. Only bound persistent volume claims can be used by the cluster user to mount the volume to a pod. This process is referred to as static provisioning of persistent storage.

**Note:** Static provisioning of persistent storage only applies to existing NFS file shares. If you do not have existing NFS file shares, cluster users can use the [dynamic provisioning](cs_apps.html#cs_apps_volume_claim) process to add persistent volumes.

To create a persistent volume and matching persistent volume claim, follow these steps.

1.  In your {{site.data.keyword.BluSoftlayer_notm}} account, look up the ID and path of the NFS file share where you want to create your persistent volume object.
    1.  Log in to your {{site.data.keyword.BluSoftlayer_notm}} account.
    2.  Click **Storage**.
    3.  Click **File Storage** and note the ID and path of the NFS file share that you want to use.
2.  Open your preferred editor.
3.  Create a storage configuration script for your persistent volume.

    ```
    apiVersion: v1
    kind: PersistentVolume
    metadata:
     name: mypv
    spec:
     capacity:
       storage: "20Gi"
     accessModes:
       - ReadWriteMany
     nfs:
       server: "nfslon0410b-fz.service.softlayer.com"
       path: "/IBM01SEV8491247_0908"
    ```
    {: codeblock}

    <table>
    <caption>Table 8. Understanding the YAML file components</caption>
    <thead>
    <th colspan=2><img src="images/idea.png"/> Understanding the YAML file components</th>
    </thead>
    <tbody>
    <tr>
    <td><code>name</code></td>
    <td>Enter the name of the persistent volume object that you want to create.</td> 
    </tr>
    <tr>
    <td><code>storage</code></td>
    <td>Enter the storage size of the existing NFS file share. The storage size must be written in gigabytes, for example, 20Gi (20 GB) or 1000Gi (1 TB), and the size must match the size of the existing file share.</td> 
    </tr>
    <tr>
    <td><code>accessMode</code></td>
    <td>Access modes define the way that the persistent volume claim can be mounted to a worker node.<ul><li>ReadWriteOnce (RWO): The persistent volume can be mounted to pods in a single worker node only. Pods that are mounted to this persistent volume can read from and write to the volume.</li><li>ReadOnlyMany (ROX): The persistent volume can be mounted to pods that are hosted on multiple worker nodes. Pods that are mounted to this persistent volume can only read from the volume.</li><li>ReadWriteMany (RWX): This persistent volume can be mounted to pods that are hosted on multiple worker nodes. Pods that are mounted to this persistent volume can read from and write to the volume.</li></ul></td> 
    </tr>
    <tr>
    <td><code>server</code></td>
    <td>Enter the NFS file share server ID.</td> 
    </tr>
    <tr>
    <td><code>path</code></td>
    <td>Enter the path to the NFS file share where you want to create the persistent volume object.</td> 
    </tr>
    </tbody></table>

4.  Create the persistent volume object in your cluster.

    ```
    kubectl apply -f <yaml_path>
    ```
    {: pre}

    Example

    ```
    kubectl apply -f deploy/kube-config/pv.yaml
    ```
    {: pre}

5.  Verify that the persistent volume is created.

    ```
    kubectl get pv
    ```
    {: pre}

6.  Create another configuration script to create your persistent volume claim. In order for the persistent volume claim to match the persistent volume object that you created earlier, you must choose the same value for `storage` and `accessMode`. The `storage-class` field must be empty. If any of these fields do not match the persistent volume, then a new persistent volume is created automatically instead.

    ```
    kind: PersistentVolumeClaim
    apiVersion: v1
    metadata:
     name: mypvc
     annotations:
       volume.beta.kubernetes.io/storage-class: ""
    spec:
     accessModes:
       - ReadWriteMany
     resources:
       requests:
         storage: "20Gi"
    ```
    {: codeblock}

7.  Create your persistent volume claim.

    ```
    kubectl apply -f deploy/kube-config/mypvc.yaml
    ```
    {: pre}

8.  Verify that your persistent volume claim is created and bound to the persistent volume object. This process can take a few minutes.

    ```
    kubectl describe pvc mypvc
    ```
    {: pre}

    Your output looks similar to the following.

    ```
    Name: mypvc
    Namespace: default
    StorageClass: ""
    Status: Bound
    Volume: pvc-0d787071-3a67-11e7-aafc-eef80dd2dea2
    Labels: <none>
    Capacity: 20Gi
    Access Modes: RWX
    Events:
      FirstSeen LastSeen Count From        SubObjectPath Type Reason Message
      --------- -------- ----- ----        ------------- -------- ------ -------
      3m 3m 1 {ibm.io/ibmc-file 31898035-3011-11e7-a6a4-7a08779efd33 } Normal Provisioning External provisioner is provisioning volume for claim "default/my-persistent-volume-claim"
      3m 1m  10 {persistentvolume-controller } Normal ExternalProvisioning cannot find provisioner "ibm.io/ibmc-file", expecting that a volume for the claim is provisioned either manually or via external software
      1m 1m 1 {ibm.io/ibmc-file 31898035-3011-11e7-a6a4-7a08779efd33 } Normal ProvisioningSucceeded Successfully provisioned volume pvc-0d787071-3a67-11e7-aafc-eef80dd2dea2
    ```
    {: screen}


You successfully created a persistent volume object and bound it to a persistent volume claim. Cluster users can now [mount the persistent volume claim](cs_apps.html#cs_apps_volume_mount) to their pod and start reading from and writing to the persistent volume object.

## Visualizing Kubernetes cluster resources
{: #cs_weavescope}

Weave Scope provides a visual diagram of your resources within a Kubernetes cluster, including services, pods, containers, processes, nodes, and more. Weave Scope provides interactive metrics for CPU and memory and also provides tools to tail and exec into a container.
{:shortdesc}

Before you begin:

-   Remember not to expose your cluster information on the public internet. Complete these steps to deploy Weave Scope securely and access it from a web browser locally.
-   If you do not have one already, [create a standard cluster](#cs_cluster_ui). Weave Scope can be CPU heavy, especially the app. Run Weave Scope with larger standard clusters, not lite clusters.
-   [Target your CLI](cs_cli_install.html#cs_cli_configure) to your cluster to run `kubectl` commands.


To use Weave Scope with a cluster:
2.  Deploy one of the provided RBAC permissions configuration file in the cluster.

    To enable read/write permissions:

    ```
    kubectl apply -f "https://raw.githubusercontent.com/IBM-{{site.data.keyword.Bluemix_notm}}/kube-samples/master/weave-scope/weave-scope-rbac.yaml"
    ```
    {: pre}

    To enable read-only permissions:

    ```
    kubectl apply -f "https://raw.githubusercontent.com/IBM-{{site.data.keyword.Bluemix_notm}}/kube-samples/master/weave-scope/weave-scope-rbac-readonly.yaml"
    ```
    {: pre}

    Output:

    ```
    clusterrole "weave-scope-mgr" created
    clusterrolebinding "weave-scope-mgr-role-binding" created
    ```
    {: screen}

3.  Deploy the Weave Scope service, which is privately accessible by the cluster IP address.

    <pre class="pre">
    <code>kubectl apply --namespace kube-system -f "https://cloud.weave.works/k8s/scope.yaml?k8s-version=$(kubectl version | base64 | tr -d '&bsol;n')"</code>
    </pre>

    Output:

    ```
    serviceaccount "weave-scope" created
    deployment "weave-scope-app" created
    service "weave-scope-app" created
    daemonset "weave-scope-agent" created
    ```
    {: screen}

4.  Run a port forwarding command to bring up the service on your computer. Now that Weave Scope is configured with the cluster, to access Weave Scope next time, you can run this port forwarding command without completing the previous configuration steps again.

    ```
    kubectl port-forward -n kube-system "$(kubectl get -n kube-system pod --selector=weave-scope-component=app -o jsonpath='{.items..metadata.name}')" 4040
    ```
    {: pre}

    Output:

    ```
    Forwarding from 127.0.0.1:4040 -> 4040
    Forwarding from [::1]:4040 -> 4040
    Handling connection for 4040
    ```
    {: screen}

5.  Open your web browser to `http://localhost:4040`. Choose to view topology diagrams or tables of the Kubernetes resources in the cluster.

     <img src="images/weave_scope.png" alt="Example topology from Weave Scope" style="width:357px;" /> 


[Learn more about the Weave Scope features ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.weave.works/docs/scope/latest/features/).

## Removing clusters
{: #cs_cluster_remove}

When you are finished with a cluster, you can remove it so that the cluster is no longer consuming resources.
{:shortdesc}

Lite and standard clusters created with a standard or {{site.data.keyword.Bluemix_notm}} Pay-As-You-Go account must be removed manually by the user when they are not needed anymore. Lite clusters created with a free trial account are automatically removed after the free trial period ends.

When you delete a cluster, you are also deleting resources on the cluster, including containers, pods, bound services, and secrets. If you do not delete your storage when you delete your cluster, you can delete your storage through the {{site.data.keyword.BluSoftlayer_notm}} dashboard in the {{site.data.keyword.Bluemix_notm}} GUI. Due to the monthly billing cycle, a persistent volume claim cannot be deleted on the last day of a month. If you delete the persistent volume claim on the last day of the month, the deletion remains pending until the beginning of the next month.

**Warning:** No backups are created of your cluster or your data in your persistent storage. Deleting a cluster is permanent and cannot be undone.

-   From the {{site.data.keyword.Bluemix_notm}} GUI
    1.  Select your cluster and click **Delete** from the **More actions...** menu.
-   From the {{site.data.keyword.Bluemix_notm}} CLI
    1.  List the available clusters.

        ```
        bx cs clusters
        ```
        {: pre}

    2.  Delete the cluster.

        ```
        bx cs cluster-rm my_cluster
        ```
        {: pre}

    3.  Follow the prompts and choose whether to delete cluster resources.

When you remove a cluster, the portable public and private subnets are not removed automatically. Subnets are used to assign portable public IP addresses to load balancer services or your Ingress controller. You can choose to manually delete subnets or reuse them in a new cluster.
