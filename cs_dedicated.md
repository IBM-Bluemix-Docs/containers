

4.  Create a cluster with the `cluster-create` command. When you create a standard cluster, the hardware of the worker node is billed by hours of usage.

    Example:

    ```
    bx cs cluster-create --location <location> --machine-type <machine-type> --name <cluster_name> --workers <number>
    ```
    {: pre}

    <table>
    <caption>Table 2. Understanding this command's components</caption>
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
    <td>If you are creating a standard cluster, choose a machine type. The machine type specifies the virtual compute resources that are available to each worker node. Review [Comparison of lite and standard clusters for {{site.data.keyword.containershort_notm}}](cs_planning.html#cs_planning_cluster_type) for more information. For lite clusters, you do not have to define the machine type.</td>
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

For new namespaces, see the options in [Using private and public image registries with {{site.data.keyword.containershort_notm}}](cs_cluster.html#cs_apps_images). For namespaces that were set up for single and scalable groups, [use a token and create a Kubernetes secret](cs_dedicated_tokens.html#cs_dedicated_tokens) for authentication.

### Adding subnets to clusters
{: #dedicated_cluster_subnet}

Change the pool of available portable public IP addresses by adding subnets to your cluster. For more information, see [Adding subnets to clusters](cs_cluster.html#cs_cluster_subnet). Review the following differences for adding subnets to Dedicated clusters.

#### Adding additional user-managed subnets and IP addresses to your Kubernetes clusters
{: #dedicated_byoip_subnets}

Provide more of your own subnets from an on-premises network that you want to use to access {{site.data.keyword.containershort_notm}}. You can add private IP addresses from those subnets to Ingress and load balancer services in your Kubernetes cluster. User-managed subnets are configured in one of two ways depending on the format of the subnet that you want to use.

Requirements:
- User-managed subnets can be added to private VLANs only.
- The subnet prefix length limit is /24 to /30. For example, `203.0.113.0/24` specifies 253 usable private IP addresses, while `203.0.113.0/30` specifies 1 usable private IP address.
- The first IP address in the subnet must be used as the gateway for the subnet.

Before you begin: Configure the routing of network traffic into and out of your enterprise network to the {{site.data.keyword.Bluemix_dedicated_notm}} network that will use the user-managed subnet.

1. To use your own subnet, [open a support ticket](/docs/support/index.html#contacting-support) and provide the list of subnet CIDRs that you want to use.
    **Note**: The way that the ALB and load balancers are managed for on-premises and internal account connectivity differs depending on the format of the subnet CIDR. See the final step for configuration differences.

2. After {{site.data.keyword.IBM_notm}} provisions the user-managed subnets, make the subnet available to your Kubernetes cluster.

    ```
    bx cs cluster-user-subnet-add <cluster_name> <subnet_CIDR> <private_VLAN>
    ```
    {: pre}
    Replace <em>&lt;cluster_name&gt;</em> with the name or ID of your cluster, <em>&lt;subnet_CIDR&&gt;</em> with one of the subnet CIDRs that you provided in the support ticket, and <em>&lt;private_VLAN&gt;</em> with an available private VLAN ID. You can find the ID of an available private VLAN by running `bx cs vlans`.

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
  - If you used a 10.x.x.x private IP address range for the subnet, use valid IPs from that range to configure on-premises and internal account connectivity with Ingress and a load balancer. For more information, see [Configuring access to an app](cs_apps.html#cs_apps_public).
  - If you did not use a 10.x.x.x private IP address range for the subnet, use valid IPs from that range to configure on-premises connectivity with Ingress and a load balancer. For more information, see [Configuring access to an app](cs_apps.html#cs_apps_public). However, you must use an IBM Cloud infrastructure (SoftLayer) portable private subnet to configure internal account connectivity between your cluster and other Cloud Foundry-based services. You can create a portable private subnet with the [`bx cs cluster-subnet-add`](cs_cli_reference.html#cs_cluster_subnet_add) command. For this scenario, your cluster has both a user-managed subnet for on-premises connectivity and an IBM Cloud infrastructure (SoftLayer) portable private subnet for internal account connectivity.

### Other cluster configurations
{: dedicated_other}

Review the following options for other cluster configurations:
  * [Managing cluster access](cs_cluster.html#cs_cluster_user)
  * [Updating the Kubernetes master](cs_cluster.html#cs_cluster_update)
  * [Updating worker nodes](cs_cluster.html#cs_cluster_worker_update)
  * [Configuring cluster logging](cs_cluster.html#cs_logging)
  * [Configuring cluster monitoring](cs_cluster.html#cs_monitoring)
  * [Visualizing Kubernetes cluster resources](cs_cluster.html#cs_weavescope)
  * [Removing clusters](cs_cluster.html#cs_cluster_remove)

<br />


## Deploying apps in clusters
{: #dedicated_apps}

You can use Kubernetes techniques to deploy apps in {{site.data.keyword.Bluemix_dedicated_notm}} clusters and to ensure that your apps are always up and running.
{:shortdesc}

To deploy apps in clusters, you can follow the instructions for [deploying apps in {{site.data.keyword.Bluemix_notm}} public clusters](cs_apps.html#cs_apps). Review the following differences for {{site.data.keyword.Bluemix_dedicated_notm}} clusters.

### Allowing public access to apps
{: #dedicated_apps_public}

For {{site.data.keyword.Bluemix_dedicated_notm}} environments, public primary IP addresses are blocked by a firewall. To make an app publicly available, use a [LoadBalancer service](#dedicated_apps_public_load_balancer) or [Ingress](#dedicated_apps_public_ingress) instead of a NodePort service. If you require access to a LoadBalancer service or Ingress that portable public IP addresses, supply an enterprise firewall whitelist to IBM at service onboarding time.

#### Configuring access to an app by using the load balancer service type
{: #dedicated_apps_public_load_balancer}

If you want to use public IP addresses for the load balancer, ensure that an enterprise firewall whitelist was provided to IBM or [open a support ticket](/docs/support/index.html#contacting-support) to configure the firewall whitelist. Then follow the steps in [Configuring access to an app by using the load balancer service type](cs_apps.html#cs_apps_public_load_balancer).

#### Configuring public access to an app by using Ingress
{: #dedicated_apps_public_ingress}

If you want to use public IP addresses for the application load balancer, ensure that an enterprise firewall whitelist was provided to IBM or [open a support ticket](/docs/support/index.html#contacting-support) to configure the firewall whitelist. Then follow the steps in [Configuring access to an app by using Ingress](cs_apps.html#cs_apps_public_ingress).

### Creating persistent storage
{: #dedicated_apps_volume_claim}

For {{site.data.keyword.Bluemix_dedicated_notm}} environments, you must [open a support ticket](/docs/support/index.html#contacting-support). By opening a ticket, you can request a backup for your volumes, a restoration from your volumes, and other storage functions.
</staging>
