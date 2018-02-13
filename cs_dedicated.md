---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-01"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}

# Getting started with clusters in {{site.data.keyword.Bluemix_dedicated_notm}}
{: #dedicated}

If you have an {{site.data.keyword.Bluemix_dedicated}} account, you can deploy clusters in a dedicated cloud environment (`https://<my-dedicated-cloud-instance>.bluemix.net`) and connect with the preselected {{site.data.keyword.Bluemix}} services that are also running there.
{:shortdesc}

If you do not have an {{site.data.keyword.Bluemix_dedicated_notm}} account, you can [get started with {{site.data.keyword.containershort_notm}}](container_index.html#container_index) in a public {{site.data.keyword.Bluemix_notm}} account.

## About the Dedicated cloud environment
{: #dedicated_environment}

With an {{site.data.keyword.Bluemix_dedicated_notm}} account, available physical resources are dedicated to your cluster only and are not shared with clusters from other {{site.data.keyword.IBM_notm}} customers. You might choose to set up an {{site.data.keyword.Bluemix_dedicated_notm}} environment when you want isolation for your cluster and you also require such isolation for the other {{site.data.keyword.Bluemix_notm}} services that you use. If you do not have a Dedicated account, you can create clusters with dedicated hardware in {{site.data.keyword.Bluemix_notm}} public.

With {{site.data.keyword.Bluemix_dedicated_notm}}, you can create clusters from the catalog in the Dedicated console or by using the {{site.data.keyword.containershort_notm}} CLI. When you use the Dedicated console, you log in to both your Dedicated and public accounts simultaneously using your IBMid. This dual login lets you access your public clusters using your Dedicated console. When you use the CLI, you log in using your Dedicated endpoint (`api.<my-dedicated-cloud-instance>.bluemix.net.`) and target the {{site.data.keyword.containershort_notm}} API endpoint of the public region that is associated with the Dedicated environment.

The most significant differences between {{site.data.keyword.Bluemix_notm}} public and Dedicated are as follows.

*   {{site.data.keyword.IBM_notm}} owns and manages the IBM Cloud infrastructure (SoftLayer) account that the worker nodes, VLANs, and subnets are deployed into, rather than in an account that is owned by you.
*   Specifications for those VLANs and subnets are determined when the Dedicated environment is enabled, not when the cluster is created.

### Differences in cluster management between the cloud environments
{: #dedicated_env_differences}

|Area|{{site.data.keyword.Bluemix_notm}} public|{{site.data.keyword.Bluemix_dedicated_notm}}|
|--|--------------|--------------------------------|
|Cluster creation|Create a free cluster or specify the following details for a standard cluster:<ul><li>Cluster type</li><li>Name</li><li>Location</li><li>Machine type</li><li>Number of worker nodes</li><li>Public VLAN</li><li>Private VLAN</li><li>Hardware</li></ul>|Specify the following details for a standard cluster:<ul><li>Name</li><li>Kubernetes version</li><li>Machine type</li><li>Number of worker nodes</li></ul><p>**Note:** The VLANs and Hardware settings are pre-defined during the creation of the {{site.data.keyword.Bluemix_notm}} environment.</p>|
|Cluster hardware and ownership|In standard clusters, the hardware can be shared by other {{site.data.keyword.IBM_notm}} customers or dedicated to you only. The public and private VLANs are owned and managed by you in your IBM Cloud infrastructure (SoftLayer) account.|In clusters on {{site.data.keyword.Bluemix_dedicated_notm}}, the hardware is always dedicated. The public and private VLANs are owned and managed by IBM for you. Location is pre-defined for the {{site.data.keyword.Bluemix_notm}} environment.|
|Load balancer and Ingress networking|During the provisioning of standard clusters, the following actions occur automatically.<ul><li>One portable public and one portable private subnet are bound to your cluster and assigned to your IBM Cloud infrastructure (SoftLayer) account.</li><li>One portable public IP address is used for a highly available application load balancer and a unique public route is assigned in the format &lt;cluster_name&gt;.containers.mybluemix.net. You can use this route to expose multiple apps to the public. One portable private IP address is used for a private application load balancer.</li><li>Four portable public and four portable private IP addresses are assigned to the cluster that can be used to expose apps via load balancer services. Additional subnets can be requested through your IBM Cloud infrastructure (SoftLayer) account.</li></ul>|When you create your Dedicated account, you make a connectivity decision on how you want to expose and access your cluster services. If you want to use your own enterprise IP ranges (user-manged IPs), you must provide them when you [set up an {{site.data.keyword.Bluemix_dedicated_notm}} environment](/docs/dedicated/index.html#setupdedicated). <ul><li>By default, no portable public subnets are bound to clusters that you create in your Dedicated account. Instead, you have the flexibility to choose the connectivity model which best suits your enterprise.</li><li>After you create the cluster, you choose the type of subnets you want bind to and use with your cluster for either load balancer or Ingress connectivity.<ul><li>For public or private portable subnets, you can [add subnets to clusters](cs_subnets.html#subnets)</li><li>For user-managed IP addresses that you provided to IBM at Dedicated onboarding, you can [add user-managed subnets to clusters](#dedicated_byoip_subnets).</li></ul></li><li>After you bind a subnet to your cluster, the Ingress application load balancer is created. A public Ingress route is created only if you use a portable public subnet.</li></ul>|
|NodePort networking|Expose a public port on your worker node and use the public IP address of the worker node to publicly access your service in the cluster.|All public IP addresses of the workers nodes are blocked by a firewall. However, for {{site.data.keyword.Bluemix_notm}} services that are added to the cluster, the node port can be accessed via a public IP address or a private IP address.|
|Persistent storage|Use [dynamic provisioning](cs_storage.html#create) or [static provisioning](cs_storage.html#existing) of volumes.|Use [dynamic provisioning](cs_storage.html#create) of volumes. [Open a support ticket](/docs/get-support/howtogetsupport.html#getting-customer-support) to request a backup for your volumes, request a restoration from your volumes, and perform other storage functions.</li></ul>|
|Image registry URL in {{site.data.keyword.registryshort_notm}}|<ul><li>US-South and US-East: <code>registry.ng bluemix.net</code></li><li>UK-South: <code>registry.eu-gb.bluemix.net</code></li><li>EU-Central (Frankfurt): <code>registry.eu-de.bluemix.net</code></li><li>Australia (Sydney): <code>registry.au-syd.bluemix.net</code></li></ul>|<ul><li>For new namespaces, use the same region-based registries that are defined for {{site.data.keyword.Bluemix_notm}} public.</li><li>For namespaces that were set up for single and scalable containers in {{site.data.keyword.Bluemix_dedicated_notm}}, use <code>registry.&lt;dedicated_domain&gt;</code></li></ul>|
|Accessing the registry|See the options in [Using private and public image registries with {{site.data.keyword.containershort_notm}}](cs_images.html).|<ul><li>For new namespaces, see the options in [Using private and public image registries with {{site.data.keyword.containershort_notm}}](cs_images.html).</li><li>For namespaces that were set up for single and scalable groups, [use a token and create a Kubernetes secret](cs_dedicated_tokens.html#cs_dedicated_tokens) for authentication.</li></ul>|
{: caption="Feature differences between {{site.data.keyword.Bluemix_notm}} public and {{site.data.keyword.Bluemix_dedicated_notm}}" caption-side="top"}

<br />


### Service architecture
{: #dedicated_ov_architecture}

Each worker node is set up with an {{site.data.keyword.IBM_notm}}-managed Docker Engine, separate compute resources, networking, and volume service. Built-in security features provide isolation, resource management capabilities, and worker node security compliance. The worker node communicates with the master by using secure TLS certificates and openVPN connection.
{:shortdesc}

*Kubernetes architecture and networking in the {{site.data.keyword.Bluemix_dedicated_notm}}*

![{{site.data.keyword.containershort_notm}} Kubernetes architecture on {{site.data.keyword.Bluemix_dedicated_notm}}](images/cs_dedicated_arch.png)

<br />


## Setting up {{site.data.keyword.containershort_notm}} on Dedicated
{: #dedicated_setup}

Each {{site.data.keyword.Bluemix_dedicated_notm}} environment has a public, client-owned, corporate account in {{site.data.keyword.Bluemix_notm}}. In order for users in the Dedicated environment to create clusters, the administrator must add the users to this public corporate account for the Dedicated environment.

Before you begin:
  * [Set up an {{site.data.keyword.Bluemix_dedicated_notm}} environment](/docs/dedicated/index.html#setupdedicated).
  * If your local system or your corporate network controls public internet endpoints by using proxies or firewalls, you must [open required ports and IP addresses in your firewall](cs_firewall.html#firewall).
  * [Download the Cloud Foundy CLI ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/cloudfoundry/cli/releases) and [Add the IBM Cloud Admin CLI plug-in](/docs/cli/plugins/bluemix_admin/index.html#adding-the-ibm-cloud-admin-cli-plug-in).

To allow {{site.data.keyword.Bluemix_dedicated_notm}} users to access clusters:

1.  The owner of your public {{site.data.keyword.Bluemix_notm}} account must generate an API key.
    1.  Log in to the endpoint for your {{site.data.keyword.Bluemix_dedicated_notm}} instance. Enter the {{site.data.keyword.Bluemix_notm}} credentials for the public account owner and select your account when prompted.

        ```
        bx login -a api.<my-dedicated-cloud-instance>.<region>.bluemix.net
        ```
        {: pre}

        **Note:** If you have a federated ID, use `bx login -a api.<my-dedicated-cloud-instance>.<region>.bluemix.net --sso` to log in to the {{site.data.keyword.Bluemix_notm}} CLI. Enter your user name and use the provided URL in your CLI output to retrieve your one-time passcode. You know that you have a federated ID when the login fails without the `--sso` and succeeds with the `--sso` option.

    2.  Generate an API key for inviting users to the public account. Note the API key value, which the Dedicated account administrator will use in the next step.

        ```
        bx iam api-key-create <key_name> -d "Key to invite users to <dedicated_account_name>"
        ```
        {: pre}

    3.  Note the GUID of the public account organization that you want to invite users to, which the Dedicated account administrator will use in the next step.

        ```
        bx account orgs
        ```
        {: pre}

2.  The owner of your {{site.data.keyword.Bluemix_dedicated_notm}} account can invite single or multiple users to your Public account.
    1.  Log in to the endpoint for your {{site.data.keyword.Bluemix_dedicated_notm}} instance. Enter the {{site.data.keyword.Bluemix_notm}} credentials for the Dedicated account owner and select your account when prompted.

        ```
        bx login -a api.<my-dedicated-cloud-instance>.<region>.bluemix.net
        ```
        {: pre}

        **Note:** If you have a federated ID, use `bx login -a api.<my-dedicated-cloud-instance>.<region>.bluemix.net --sso` to log in to the {{site.data.keyword.Bluemix_notm}} CLI. Enter your user name and use the provided URL in your CLI output to retrieve your one-time passcode. You know that you have a federated ID when the login fails without the `--sso` and succeeds with the `--sso` option.

    2.  Invite the users to the public account.
        * To invite a single user:

            ```
            bx cf bluemix-admin invite-users-to-public -userid=<user_email> -apikey=<public_api_key> -public_org_id=<public_org_id>
            ```
            {: pre}

            Replace <em>&lt;user_IBMid&gt;</em> with the email of the user you want to invite, <em>&lt;public_api_key&gt;</em> with the API key generated in the previous step, and <em>&lt;public_org_id&gt;</em> with the GUID of the public account organization. See [Inviting a user from IBM Cloud Dedicated](/docs/cli/plugins/bluemix_admin/index.html#admin_dedicated_invite_public) for more information on this command.

        * To invite all users currently in a Dedicated account organization:

            ```
            bx cf bluemix-admin invite-users-to-public -organization=<dedicated_org_id> -apikey=<public_api_key> -public_org_id=<public_org_id>
            ```

            Replace <em>&lt;dedicated_org_id&gt;</em> with the Dedicated account organization ID, <em>&lt;public_api_key&gt;</em> with the API key generated in the previous step, and <em>&lt;public_org_id&gt;</em> with the public account organization GUID. See [Inviting a user from IBM Cloud Dedicated](/docs/cli/plugins/bluemix_admin/index.html#admin_dedicated_invite_public) for more information on this command.

    3.  If an IBMid exists for a user, the user is automatically added to the specified organization in the public account. If an IBMid does not already exist for a user, then an invitation is sent to the user's email address. Once the user accepts the invitation, an IBMid is created for the user, and the user is added to the specified organization in the public account.

    4.  Verify that the users were added to the account.

        ```
        bx cf bluemix-admin invite-users-status -apikey=<public_api_key>
        ```
        {: pre}

        Invited users that have an existing IBMid will have a status of `ACTIVE`. Invited users that did not have an existing IBMid will have a status of either `PENDING` or `ACTIVE` depending on whether or not they have accepted the invitation to the account yet.

3.  If any user needs cluster create privileges, you must grant the Administrator role to that user.

    1.  From the menu bar in the public console, click **Manage > Security > Identity and Access**, and then click **Users**.

    2.  From the row for the user that you want to assign access, select the **Actions** menu, and then click **Assign access**.

    3.  Select **Assign access to resources**.

    4.  From the **Services** list, select **IBM Cloud Container Service**.

    5.  From the **Region** list, select **All current regions** or a specific region, if you are prompted.

    6. Under **Select roles**, select Administrator.

    7. Click **Assign**.

4.  Users can now log in to the Dedicated account endpoint to start creating clusters.

    1.  Log in to the endpoint for your {{site.data.keyword.Bluemix_dedicated_notm}} instance. Enter your IBMid when prompted.

        ```
        bx login -a api.<my-dedicated-cloud-instance>.<region>.bluemix.net
        ```
        {: pre}

        **Note:** If you have a federated ID, use `bx login -a api.<my-dedicated-cloud-instance>.<region>.bluemix.net --sso` to log in to the {{site.data.keyword.Bluemix_notm}} CLI. Enter your user name and use the provided URL in your CLI output to retrieve your one-time passcode. You know that you have a federated ID when the login fails without the `--sso` and succeeds with the `--sso` option.

    2.  If you are logging in for the first time, provide your Dedicated user ID and password when prompted. This authenticates the Dedicated account and links the Dedicated and public accounts together. Every time you log in after this first time, you only use your IBMid to log in. For more information, see [Connecting a dedicated ID to your public IBMid](/docs/cli/connect_dedicated_id.html#connect_dedicated_id).

        **Note**: You must log in to both your Dedicated account and your public account in order to create clusters. If you only want to log in to your Dedicated account, use the `--no-iam` flag when logging in to the Dedicated endpoint.

    3.  To create or access clusters in the dedicated environment, you must set the region associated with that environment.

        ```
        bx cs region-set
        ```
        {: pre}

5.  If you want to unlink your accounts, you can disconnect your IBMid from your Dedicated user ID. For more information, see [Disconnect your dedicated ID from the public IBMid](/docs/cli/connect_dedicated_id.html#disconnect-your-dedicated-id-from-the-public-ibmid).

    ```
    bx iam dedicated-id-disconnect
    ```
    {: pre}

<br />


## Creating clusters
{: #dedicated_administering}

Design your {{site.data.keyword.Bluemix_dedicated_notm}} cluster setup for maximum availability and capacity.
{:shortdesc}

### Creating clusters with the GUI
{: #dedicated_creating_ui}

1.  Open your Dedicated console: `https://<my-dedicated-cloud-instance>.bluemix.net`.
2. Select the **Also log in to {{site.data.keyword.Bluemix_notm}} Public** check box and click **Log in**.
3. Follow the prompts to log in with your IBMid. If this is your first time to log in to your Dedicated account, then follow the prompts to log in to {{site.data.keyword.Bluemix_dedicated_notm}}.
4.  From the catalog, select **Containers** and click **Kubernetes cluster**.
5.  Enter a **Cluster Name**.
6.  Select a **Machine type**. The machine type defines the amount of virtual CPU and memory that is set up in each worker node. This virtual CPU and memory is available for all the containers that you deploy in your nodes.
    -   The micro machine type indicates the smallest option.
    -   A balanced machine type has an equal amount of memory that is assigned to each CPU, which optimizes performance.
7.  Choose the **Number of worker nodes** that you need. Select `3` to ensure high availability of your cluster.
8.  Click **Create Cluster**. The details for the cluster open, but the worker nodes in the cluster take a few minutes to provision. On the **Worker nodes** tab, you can see the progress of the worker node deployment. When the worker nodes are ready, the state changes to **Ready**.

### Creating clusters with the CLI
{: #dedicated_creating_cli}

1.  Install the {{site.data.keyword.Bluemix_notm}} CLI and the [{{site.data.keyword.containershort_notm}} plug-in](cs_cli_install.html#cs_cli_install).
2.  Log in to the endpoint for your {{site.data.keyword.Bluemix_dedicated_notm}} instance. Enter your {{site.data.keyword.Bluemix_notm}} credentials and select your account when prompted.

    ```
    bx login -a api.<my-dedicated-cloud-instance>.<region>.bluemix.net
    ```
    {: pre}

    **Note:** If you have a federated ID, use `bx login -a api.<my-dedicated-cloud-instance>.<region>.bluemix.net --sso` to log in to the {{site.data.keyword.Bluemix_notm}} CLI. Enter your user name and use the provided URL in your CLI output to retrieve your one-time passcode. You know that you have a federated ID when the login fails without the `--sso` and succeeds with the `--sso` option.

3.  To target a region, run `bx cs region-set`.

4.  Create a cluster with the `cluster-create` command. When you create a standard cluster, the hardware of the worker node is billed by hours of usage.

    Example:

    ```
    bx cs cluster-create --location <location> --machine-type <machine-type> --name <cluster_name> --workers <number>
    ```
    {: pre}

    <table>
    <caption>Understanding this command's components</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding this command's components</th>
    </thead>
    <tbody>
    <tr>
    <td><code>cluster-create</code></td>
    <td>The command to create a cluster in your {{site.data.keyword.Bluemix_notm}} organization.</td>
    </tr>
    <tr>
    <td><code>--location <em>&lt;location&gt;</em></code></td>
    <td>Replace &lt;location&gt; with the {{site.data.keyword.Bluemix_notm}} location ID that your Dedicated environment is configured to use.</td>
    </tr>
    <tr>
    <td><code>--machine-type <em>&lt;machine_type&gt;</em></code></td>
    <td>If you are creating a standard cluster, choose a machine type. The machine type specifies the virtual compute resources that are available to each worker node. Review [Comparison of free and standard clusters for {{site.data.keyword.containershort_notm}}](cs_why.html#cluster_types) for more information. For free clusters, you do not have to define the machine type.</td>
    </tr>
    <tr>
    <td><code>--name <em>&lt;name&gt;</em></code></td>
    <td>Replace <em>&lt;name&gt;</em> with a name for your cluster.</td>
    </tr>
    <tr>
    <td><code>--workers <em>&lt;number&gt;</em></code></td>
    <td>The number of worker nodes to include in the cluster. If the <code>--workers</code> option is not specified, one worker node is created.</td>
    </tr>
    </tbody></table>

5.  Verify that the creation of the cluster was requested.

    ```
    bx cs clusters
    ```
    {: pre}

    **Note:** It can take up to 15 minutes for the worker node machines to be ordered, and for the cluster to be set up and provisioned in your account.

    When the provisioning of your cluster is completed, the state of your cluster changes to **deployed**.

    ```
    Name         ID                                   State      Created          Workers
    my_cluster   paf97e8843e29941b49c598f516de72101   deployed   20170201162433   1
    ```
    {: screen}

6.  Check the status of the worker nodes.

    ```
    bx cs workers <cluster>
    ```
    {: pre}

    When the worker nodes are ready, the state changes to **normal** and the status is **Ready**. When the node status is **Ready**, you can then access the cluster.

    ```
    ID                                                  Public IP        Private IP     Machine Type   State      Status
    prod-dal10-pa8dfcc5223804439c87489886dbbc9c07-w1    169.47.223.113   10.171.42.93   free           normal    Ready
    ```
    {: screen}

7.  Set the cluster that you created as the context for this session. Complete these configuration steps every time that you work with your cluster.

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

8.  Access your Kubernetes dashboard with the default port 8001.
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

### Using private and public image registries
{: #dedicated_images}

For new namespaces, see the options in [Using private and public image registries with {{site.data.keyword.containershort_notm}}](cs_images.html). For namespaces that were set up for single and scalable groups, [use a token and create a Kubernetes secret](cs_dedicated_tokens.html#cs_dedicated_tokens) for authentication.

### Adding subnets to clusters
{: #dedicated_cluster_subnet}

Change the pool of available portable public IP addresses by adding subnets to your cluster. For more information, see [Adding subnets to clusters](cs_subnets.html#subnets). Review the following differences for adding subnets to Dedicated clusters.

#### Adding additional user-managed subnets and IP addresses to your Kubernetes clusters
{: #dedicated_byoip_subnets}

Provide more of your own subnets from an on-premises network that you want to use to access {{site.data.keyword.containershort_notm}}. You can add private IP addresses from those subnets to Ingress and load balancer services in your Kubernetes cluster. User-managed subnets are configured in one of two ways depending on the format of the subnet that you want to use.

Requirements:
- User-managed subnets can be added to private VLANs only.
- The subnet prefix length limit is /24 to /30. For example, `203.0.113.0/24` specifies 253 usable private IP addresses, while `203.0.113.0/30` specifies 1 usable private IP address.
- The first IP address in the subnet must be used as the gateway for the subnet.

Before you begin: Configure the routing of network traffic into and out of your enterprise network to the {{site.data.keyword.Bluemix_dedicated_notm}} network that will use the user-managed subnet.

1. To use your own subnet, [open a support ticket](/docs/get-support/howtogetsupport.html#getting-customer-support) and provide the list of subnet CIDRs that you want to use.
    **Note**: The way that the ALB and load balancers are managed for on-premises and internal account connectivity differs depending on the format of the subnet CIDR. See the final step for configuration differences.

2. After {{site.data.keyword.IBM_notm}} provisions the user-managed subnets, make the subnet available to your Kubernetes cluster.

    ```
    bx cs cluster-user-subnet-add <cluster_name> <subnet_CIDR> <private_VLAN>
    ```
    {: pre}
    Replace <em>&lt;cluster_name&gt;</em> with the name or ID of your cluster, <em>&lt;subnet_CIDR&gt;</em> with one of the subnet CIDRs that you provided in the support ticket, and <em>&lt;private_VLAN&gt;</em> with an available private VLAN ID. You can find the ID of an available private VLAN by running `bx cs vlans`.

3. Verify that the subnets were added to your cluster. The field **User-managed** for user-provided subnets is _true_.

    ```
    bx cs cluster-get --showResources <cluster_name>
    ```
    {: pre}

    ```
    VLANs
    VLAN ID   Subnet CIDR         Public       User-managed
    1555503   192.0.2.0/24        true         false
    1555505   198.51.100.0/24     false        false
    1555505   203.0.113.0/24      false        true
    ```
    {: screen}

4. To configure on-premises and internal account connectivity, choose between these options:
  - If you used a 10.x.x.x private IP address range for the subnet, use valid IPs from that range to configure on-premises and internal account connectivity with Ingress and a load balancer. For more information, see [Configuring access to an app](cs_network_planning.html#planning).
  - If you did not use a 10.x.x.x private IP address range for the subnet, use valid IPs from that range to configure on-premises connectivity with Ingress and a load balancer. For more information, see [Configuring access to an app](cs_network_planning.html#planning). However, you must use an IBM Cloud infrastructure (SoftLayer) portable private subnet to configure internal account connectivity between your cluster and other Cloud Foundry-based services. You can create a portable private subnet with the [`bx cs cluster-subnet-add`](cs_cli_reference.html#cs_cluster_subnet_add) command. For this scenario, your cluster has both a user-managed subnet for on-premises connectivity and an IBM Cloud infrastructure (SoftLayer) portable private subnet for internal account connectivity.

### Other cluster configurations
{: #dedicated_other}

Review the following options for other cluster configurations:
  * [Managing cluster access](cs_users.html#managing)
  * [Updating the Kubernetes master](cs_cluster_update.html#master)
  * [Updating worker nodes](cs_cluster_update.html#worker_node)
  * [Configuring cluster logging](cs_health.html#logging)
  * [Configuring cluster monitoring](cs_health.html#monitoring)
      * **Note**: An `ibm-monitoring` cluster exists within each {{site.data.keyword.Bluemix_dedicated_notm}} account. This cluster continuously monitors the health of the {{site.data.keyword.containerlong_notm}} in the Dedicated environment, checking the stability and connectivity of the environment. Do not remove this cluster from the environment.
  * [Visualizing Kubernetes cluster resources](cs_integrations.html#weavescope)
  * [Removing clusters](cs_clusters.html#remove)

<br />


## Deploying apps in clusters
{: #dedicated_apps}

You can use Kubernetes techniques to deploy apps in {{site.data.keyword.Bluemix_dedicated_notm}} clusters and to ensure that your apps are always up and running.
{:shortdesc}

To deploy apps in clusters, you can follow the instructions for [deploying apps in {{site.data.keyword.Bluemix_notm}} public clusters](cs_app.html#app). Review the following differences for {{site.data.keyword.Bluemix_dedicated_notm}} clusters.

### Allowing public access to apps
{: #dedicated_apps_public}

For {{site.data.keyword.Bluemix_dedicated_notm}} environments, public primary IP addresses are blocked by a firewall. To make an app publicly available, use a [LoadBalancer service](#dedicated_apps_public_load_balancer) or [Ingress](#dedicated_apps_public_ingress) instead of a NodePort service. If you require access to a LoadBalancer service or Ingress that portable public IP addresses, supply an enterprise firewall whitelist to IBM at service onboarding time.

#### Configuring access to an app by using the load balancer service type
{: #dedicated_apps_public_load_balancer}

If you want to use public IP addresses for the load balancer, ensure that an enterprise firewall whitelist was provided to IBM or [open a support ticket](/docs/get-support/howtogetsupport.html#getting-customer-support) to configure the firewall whitelist. Then follow the steps in [Configuring access to an app by using the load balancer service type](cs_loadbalancer.html#config).

#### Configuring public access to an app by using Ingress
{: #dedicated_apps_public_ingress}

If you want to use public IP addresses for the application load balancer, ensure that an enterprise firewall whitelist was provided to IBM or [open a support ticket](/docs/get-support/howtogetsupport.html#getting-customer-support) to configure the firewall whitelist. Then follow the steps in [Configuring access to an app by using Ingress](cs_ingress.html#config).

### Creating persistent storage
{: #dedicated_apps_volume_claim}

To review options for creating persistent storage, see [Persistent data storage](cs_storage.html#planning). To request a backup for your volumes, a restoration from your volumes, and other storage functions, you must [open a support ticket](/docs/get-support/howtogetsupport.html#getting-customer-support).
