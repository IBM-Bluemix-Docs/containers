---

copyright:
  years: 2014, 2018
lastupdated: "2018-04-02"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}


# Setting up clusters
{: #clusters}

Design your Kubernetes cluster setup for maximum container availability and capacity with {{site.data.keyword.containerlong}}.
{:shortdesc}

## Cluster configuration planning
{: #planning_clusters}

Use standard clusters to increase app availability.
{:shortdesc}

Your users are less likely to experience downtime when you distribute your setup across multiple worker nodes and clusters. Built-in capabilities, like load balancing and isolation, increase resiliency against potential failures with hosts, networks, or apps.

Review these potential cluster setups that are ordered with increasing degrees of availability:

![Stages of high availability for a cluster](images/cs_cluster_ha_roadmap.png)

1.  One cluster with multiple worker nodes
2.  Two clusters that run in different locations in the same region, each with multiple worker nodes
3.  Two clusters that run in different regions, each with multiple worker nodes

Increase the availability of your cluster with these techniques:

<dl>
<dt>Spread apps across worker nodes</dt>
<dd>Allow developers to spread their apps in containers across multiple worker nodes per cluster. An app instance in each of three worker nodes allow for the downtime of one worker node without interrupting the usage of the app. You can specify how many worker nodes to include when you create a cluster from the [{{site.data.keyword.Bluemix_notm}} GUI](cs_clusters.html#clusters_ui) or the [CLI](cs_clusters.html#clusters_cli). Kubernetes limits the maximum number of worker nodes that you can have in a cluster, so keep in mind the [worker node and pod quotas ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/admin/cluster-large/).
<pre class="codeblock">
<code>bx cs cluster-create --location &lt;dal10&gt; --workers 3 --public-vlan &lt;my_public_vlan_id&gt; --private-vlan &lt;my_private_vlan_id&gt; --machine-type &lt;u2c.2x4&gt; --name &lt;my_cluster&gt;</code>
</pre>
</dd>
<dt>Spread apps across clusters</dt>
<dd>Create multiple clusters, each with multiple worker nodes. If an outage occurs with one cluster, users can still access an app that is also deployed in another cluster.
<p>Cluster 1:</p>
<pre class="codeblock">
<code>bx cs cluster-create --location &lt;dal10&gt; --workers 3 --public-vlan &lt;my_public_vlan_id&gt; --private-vlan &lt;my_private_vlan_id&gt; --machine-type &lt;u2c.2x4&gt; --name &lt;my_cluster1&gt;</code>
</pre>
<p>Cluster 2:</p>
<pre class="codeblock">
<code>bx cs cluster-create --location &lt;dal12&gt; --workers 3 --public-vlan &lt;my_public_vlan_id&gt; --private-vlan &lt;my_private_vlan_id&gt; --machine-type &lt;u2c.2x4&gt;  --name &lt;my_cluster2&gt;</code>
</pre>
</dd>
<dt>Spread apps across clusters in different regions</dt>
<dd>When you spread apps across clusters in different regions, you can allow load balancing to occur based on the region the user is in. If the cluster, hardware, or even an entire location in one region goes down, traffic is routed to the container that is deployed in another location.
<p><strong>Important:</strong> After you configure a custom domain, you can use these commands to create the clusters.</p>
<p>Location 1:</p>
<pre class="codeblock">
<code>bx cs cluster-create --location &lt;dal10&gt; --workers 3 --public-vlan &lt;my_public_vlan_id&gt; --private-vlan &lt;my_private_vlan_id&gt; --machine-type &lt;u2c.2x4&gt; --name &lt;my_cluster1&gt;</code>
</pre>
<p>Location 2:</p>
<pre class="codeblock">
<code>bx cs cluster-create --location &lt;ams03&gt; --workers 3 --public-vlan &lt;my_public_vlan_id&gt; --private-vlan &lt;my_private_vlan_id&gt; --machine-type &lt;u2c.2x4&gt; --name &lt;my_cluster2&gt;</code>
</pre>
</dd>
</dl>

<br />





## Worker node configuration planning
{: #planning_worker_nodes}

A Kubernetes cluster consists of worker nodes and is centrally monitored and managed by the Kubernetes master. Cluster admins decide how to set up the cluster of worker nodes to ensure that cluster users have all the resources to deploy and run apps in the cluster.
{:shortdesc}

When you create a standard cluster, worker nodes  are ordered in IBM Cloud infrastructure (SoftLayer) on your behalf and added to the default worker node pool in your cluster. Every worker node is assigned a unique worker node ID and domain name that must not be changed after the cluster is created.

You can choose between virtual or physical (bare metal) servers. Depending on the level of hardware isolation that you choose, virtual worker nodes can be set up as shared or dedicated nodes. You can also choose whether you want worker nodes to connect to a public VLAN and private VLAN, or only to a private VLAN. Every worker node is provisioned with a specific machine type that determines the number of vCPUs, memory, and disk space that are available to the containers that are deployed to the worker node. Kubernetes limits the maximum number of worker nodes that you can have in a cluster. Review [worker node and pod quotas ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/admin/cluster-large/) for more information.





### Hardware for worker nodes
{: #shared_dedicated_node}

When you create a standard cluster in {{site.data.keyword.Bluemix_notm}}, you choose to provision your worker nodes as physical machines (bare metal) or as virtual machines that run on physical hardware. When you create a free cluster, your worker node is automatically provisioned as a virtual, shared node in the IBM Cloud infrastructure (SoftLayer) account.
{:shortdesc}

![Hardware options for worker nodes in a standard cluster](images/cs_clusters_hardware.png)

<dl>
<dt>Physical machines (bare metal)</dt>
<dd>You can provision your worker node as a single-tenant physical server, also referred to as bare metal. Bare metal gives you direct access to the physical resources on the machine, such as the memory or CPU. This setup eliminates the virtual machine hypervisor that allocates physical resources to virtual machines that run on the host. Instead, all of a bare metal machine's resources are dedicated exclusively to the worker, so you don't need to worry about "noisy neighbors" sharing resources or slowing down performance.
<p><strong>Monthly billing</strong>: Bare metal servers are more expensive than virtual servers, and are best suited for high-performance apps that need more resources and host control. Bare metal servers are billed monthly. If you cancel a bare metal server before the end of the month, you are charged through the end of that month. Ordering and canceling bare metal servers is a manual process through your IBM Cloud infrastructure (SoftLayer) account. It can take more than one business day to complete.</p>
<p><strong>Option to enable Trusted Compute</strong>: Enable Trusted Compute to verify your worker nodes against tampering. If you don't enable trust during cluster creation but want to later, you can use the `bx cs feature-enable` [command](cs_cli_reference.html#cs_cluster_feature_enable). After you enable trust, you cannot disable it later. You can make a new cluster without trust. For more information about how trust works during the node startup process, see [{{site.data.keyword.containershort_notm}} with Trusted Compute](cs_secure.html#trusted_compute). Trusted Compute is available on clusters that run Kubernetes version 1.9 or later and have certain bare metal machine types. When you run the `bx cs machine-types <location>` [command](cs_cli_reference.html#cs_machine_types), you can see which machines support trust by reviewing the `Trustable` field.</p>
<p><strong>Bare metal machine type groups</strong>: Bare metal machine types come in groups that have different compute resources that you can choose from to meet your app's needs. Physical machine types have more local storage than virtual, and some have RAID to back up local data. To learn about the different types of bare metal offerings, see the `bx cs machine-type` [command](cs_cli_reference.html#cs_machine_types).
<ul><li>`mb1c.4x32`: If you don't need RAM- or data-intensive resources, choose this type for a balanced configuration of physical machine resources for your worker nodes. Balanced with 4 cores, 32GB Memory, 1TB SATA Primary Disk, 2TB SATA Secondary Disk, 10Gbps Bonded Network.</li>
<li>`mb1c.16x64`: If you don't need RAM- or data-intensive resources, choose this type for a balanced configuration of physical machine resources for your worker nodes. Balanced with 16 cores, 64GB Memory, 1TB SATA Primary Disk, 1.7TB SSD Secondary Disk, 10Gbps Bonded Network.</li>
<li>`mr1c.28x512`: Choose this type to maximize the RAM available to your worker nodes. RAM intensive with 28 cores, 512GB Memory, 1TB SATA Primary Disk, 1.7TB SSD Secondary Disk, 10Gbps Bonded Network.</li>
<li>`md1c.16x64.4x4tb`: Choose this type if your worker nodes require a significant amount of local disk storage, including RAID to back up the data stored locally on the machine. The 1TB primary storage disks are configured for RAID1, and the 4TB secondary storage disks are configured for RAID10. Data intensive with 28 cores, 512GB Memory, 2x1TB RAID1 Primary Disk, 4x4TB SATA RAID10 Secondary Disk, 10Gbps Bonded Network.</li>
<li>`md1c.28x512.4x4tb`: Choose this type if your worker nodes require a significant amount of local disk storage, including RAID to back up the data stored locally on the machine. The 1TB primary storage disks are configured for RAID1, and the 4TB secondary storage disks are configured for RAID10. Data intensive with 16 cores, 64GB Memory, 2x1TB RAID1 Primary Disk, 4x4TB SATA RAID10 Secondary Disk, 10Gbps Bonded Network.</li>

</ul></p></dd>
<dt>Virtual machines</dt>
<dd>When you create a standard virtual cluster, you must choose whether you want the underlying hardware to be shared by multiple {{site.data.keyword.IBM_notm}} customers (multi tenancy) or to be dedicated to you only (single tenancy).
<p>In a multi-tenant set up, physical resources, such as CPU and memory, are shared across all virtual machines that are deployed to the same physical hardware. To ensure that every virtual machine can run independently, a virtual machine monitor, also referred to as the hypervisor, segments the physical resources into isolated entities and allocates them as dedicated resources to a virtual machine (hypervisor isolation).</p>
<p>In a single-tenant set up, all physical resources are dedicated to you only. You can deploy multiple worker nodes as virtual machines on the same physical host. Similar to the multi-tenant set up, the hypervisor assures that every worker node gets its share of the available physical resources.</p>
<p>Shared nodes are usually less costly than dedicated nodes because the costs for the underlying hardware are shared among multiple customers. However, when you decide between shared and dedicated nodes, you might want to check with your legal department to discuss the level of infrastructure isolation and compliance that your app environment requires.</p>
<p><strong>Virtual `u2c` or `b2c` machine types</strong>: These machines use local disk instead of storage area networking (SAN) for reliability. Reliability benefits include higher throughput when serializing bytes to the local disk and reduced file system degradation due to network failures. These machine types contain 25GB primary local disk storage for the OS file system, and 100GB secondary local disk storage for `/var/lib/docker`, the directory that all the container data is written to.</p>
<p><strong>Deprecated `u1c` or `b1c` machine types</strong>: To start using `u2c` and `b2c` machine types, [update the machine types by adding worker nodes](cs_cluster_update.html#machine_type).</p></dd>
</dl>


Available physical and virtual machines types vary by the location in which you deploy the cluster. For more information, see the `bx cs machine-type` [command](cs_cli_reference.html#cs_machine_types). You can deploy clusters by using the [console UI](#clusters_ui) or the [CLI](#clusters_cli).

### VLAN connection for worker nodes
{: #worker_vlan_connection}

When you create a cluster, every cluster is automatically connected to a VLAN from your IBM Cloud infrastructure (SoftLayer) account.
{:shortdesc}

A VLAN configures a group of worker nodes and pods as if they were attached to the same physical wire. The private VLAN determines the private IP address that is assigned to a worker node during cluster creation, and the public VLAN determines the public IP address that is assigned to a worker node during cluster creation.

For free clusters, the cluster's worker nodes are connected to an IBM-owned public VLAN and private VLAN by default during cluster creation. For standard clusters, you must connect your worker nodes to a private VLAN. You can either connect your worker nodes to both a public VLAN and the private VLAN, or to the private VLAN only. If you want to connect your worker nodes to a private VLAN only, you can designate the ID of an existing private VLAN during cluster creation or [create a private VLAN](/docs/cli/reference/softlayer/index.html#sl_vlan_create). If worker nodes are set up with a private VLAN only, you must configure an alternative solution for network connectivity. For more information, see [VLAN connection for worker nodes](#worker_vlan_connection).

**Note**: If you have multiple VLANs for a cluster or multiple subnets on the same VLAN, you must turn on VLAN spanning so that your worker nodes can communicate with each other on the private network. For instructions, see [Enable or disable VLAN spanning](/docs/infrastructure/vlans/vlan-spanning.html#enable-or-disable-vlan-spanning).

### Worker node memory limits
{: #resource_limit_node}

{{site.data.keyword.containershort_notm}} sets a memory limit on each worker node. When pods that are running on the worker node exceed this memory limit, the pods are removed. In Kubernetes, this limit is called a [hard eviction threshold ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/tasks/administer-cluster/out-of-resource/#hard-eviction-thresholds).
{:shortdesc}

If your pods are removed frequently, add more worker nodes to your cluster or set [resource limits ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/#resource-requests-and-limits-of-pod-and-container) on your pods.

Each machine type has a different memory capacity. When there is less memory available on the worker node than the mininum threshold that is allowed, Kubernetes immediately removes the pod. The pod reschedules onto another worker node if a worker node is available.

|Worker node memory capacity|Minumum memory threshold of a worker node|
|---------------------------|------------|
|4 GB  | 256 MB |
|16 GB | 1024 MB |
|64 GB | 4096 MB |
|128 GB| 4096 MB |
|242 GB| 4096 MB |

To review how much memory is used on your worker node, run [kubectl top node ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#top).

### Autorecovery for your worker nodes
`Docker`, `kubelet`, `kube-proxy`, and `calico` are critical components that must be functional to have a healthy Kubernetes worker node. Over time these components can break and may leave your worker node in a nonfunctional state. Nonfunctional worker nodes decrease total capacity of the cluster and can result in downtime for your app.

You can [configure health checks for your worker node and enable Autorecovery](cs_health.html#autorecovery). If Autorecovery detects an unhealthy worker node based on the configured checks, Autorecovery triggers a corrective action like an OS reload on the worker node. For more information about how Autorecovery works, see the [Autorecovery blog ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/blogs/bluemix/2017/12/autorecovery-utilizes-consistent-hashing-high-availability/).

<br />



## Creating clusters with the GUI
{: #clusters_ui}

The purpose of the Kubernetes cluster is to define a set of resources, nodes, networks, and storage devices that keep apps highly available. Before you can deploy an app, you must create a cluster and set the definitions for the worker nodes in that cluster.
{:shortdesc}

Before you begin, you must have a Pay-As-You-Go or Subscription [{{site.data.keyword.Bluemix_notm}} account](https://console.bluemix.net/registration/). You can create 1 free cluster to try out some of the capabilities, or create standard clusters for fully-customizable clusters with your choice of hardware isolation.

To create a cluster:

1. In the catalog, select **Kubernetes Cluster**.

2. Select a region in which to deploy your cluster.

3. Select a type of cluster plan. You can choose either **Free** or **Standard**. With a standard cluster you have access to features like multiple worker nodes for a highly available environment.

4.  Configure your cluster details.

    1. **Free and Standard**: Give your cluster a name. The name must start with a letter, can contain letters, numbers, and -, and must be 35 characters or fewer. Note that the {{site.data.keyword.IBM_notm}}-assigned Ingress subdomain is derived from the cluster name. The cluster name and Ingress subdomain together form the fully qualified domain name, which must be unique within a region and have 63 characters or fewer. To meet these requirements, the cluster name might be truncated or the subdomain might be assigned random character values.

    2. **Standard**: Choose a version of Kubernetes, and select a location in which to deploy your cluster. For the best performance, select the location that is physically closest to you. Keep in mind that you might require legal authorization before data can be physically stored in a foreign country if you select a location that is outside your country.

    3. **Standard**: Select a type of hardware isolation. Virtual is billed hourly and bare metal is billed monthly.

        - **Virtual - Dedicated**: Your worker nodes are hosted on infrastructure that is devoted to your account. Your physical resources are completely isolated.

        - **Virtual - Shared**: Infrastructure resources, such as the hypervisor and physical hardware, are shared across you and other IBM customers, but each worker node is accessible only by you. Although this option is less expensive and sufficient in most cases, you might want to verify your performance and infrastructure requirements with your company policies.

        - **Bare Metal**: Billed monthly, bare metal servers are provisioned by manual interaction with IBM Cloud infrastructure (SoftLayer), and can take more than one business day to complete. Bare metal is best suited for high-performance applications that need more resources and host control. For clusters that run Kubernetes version 1.9 or later, you can also choose to enable [Trusted Compute](cs_secure.html#trusted_compute) to verify your worker nodes against tampering. If you don't enable trust during cluster creation but want to later, you can use the `bx cs feature-enable` [command](cs_cli_reference.html#cs_cluster_feature_enable). After you enable trust, you cannot disable it later.

        Be sure that you want to provision a bare metal machine. Because it is billed monthly, if you cancel it immediately after an order by mistake, you are still charged the full month.
        {:tip}

    4.  **Standard**: Select a type of machine and specify the number of worker nodes that you need. The machine type defines the amount of virtual CPU, memory, and disk space that is set up in each worker node and made available to the containers. Available bare metal and virtual machines types vary by the location in which you deploy the cluster. For more information, see the documentation for the `bx cs machine-type` [command](cs_cli_reference.html#cs_machine_types). After you create your cluster, you can add different machine types by adding a new worker node to the cluster.

    5. **Standard**: Select a public VLAN (optional) and private VLAN (required) from your IBM Cloud infrastructure (SoftLayer) account. Both VLANs communicate between worker nodes but the public VLAN also communicates with the IBM-managed Kubernetes master. You can use the same VLAN for multiple clusters.
        **Note**: If worker nodes are set up with a private VLAN only, you must configure an alternative solution for network connectivity. For more information, see [VLAN connection for worker nodes](#worker_vlan_connection).

    6. By default, **Encrypt local disk** is selected. If you choose to clear the check box, then the host's Docker data is not encrypted.[Learn more about the encryption](cs_secure.html#encrypted_disks).

4. Click **Create cluster**. You can see the progress of the worker node deployment in the **Worker nodes** tab. When the deploy is done, you can see that your cluster is ready in the **Overview** tab.
    **Note:** Every worker node is assigned a unique worker node ID and domain name that must not be manually changed after the cluster is created. Changing the ID or domain name prevents the Kubernetes master from managing your cluster.

**What's next?**

When the cluster is up and running, you can check out the following tasks:


-   [Install the CLIs to start working with your cluster.](cs_cli_install.html#cs_cli_install)
-   [Deploy an app in your cluster.](cs_app.html#app_cli)
-   [Set up your own private registry in {{site.data.keyword.Bluemix_notm}} to store and share Docker images with other users.](/docs/services/Registry/index.html)
- If you have multiple VLANs for a cluster or multiple subnets on the same VLAN, you must [turn on VLAN spanning](/docs/infrastructure/vlans/vlan-spanning.html#enable-or-disable-vlan-spanning) so that your worker nodes can communicate with each other on the private network.
- If you have a firewall, you might need to [open the required ports](cs_firewall.html#firewall) to use `bx`, `kubectl`, or `calicotl` commands, to allow outbound traffic from your cluster, or to allow inbound traffic for networking services.

<br />


## Creating clusters with the CLI
{: #clusters_cli}

The purpose of the Kubernetes cluster is to define a set of resources, nodes, networks, and storage devices that keep apps highly available. Before you can deploy an app, you must create a cluster and set the definitions for the worker nodes in that cluster.
{:shortdesc}

Before you begin:
- You must have a Pay-As-You-Go or Subscription [{{site.data.keyword.Bluemix_notm}} account](https://console.bluemix.net/registration/). You can create 1 free cluster to try out some of the capabilities, or create standard clusters for fully-customizable clusters with your choice of hardware isolation.
- [Make sure you have the minimum required permissions in IBM Cloud infrastructure (SoftLayer) to provision a standard cluster](cs_users.html#infra_access).

To create a cluster:

1.  Install the {{site.data.keyword.Bluemix_notm}} CLI and the [{{site.data.keyword.containershort_notm}} plug-in](cs_cli_install.html#cs_cli_install).

2.  Log in to the {{site.data.keyword.Bluemix_notm}} CLI. Enter your {{site.data.keyword.Bluemix_notm}} credentials when prompted.

    ```
    bx login
    ```
    {: pre}

    **Note:** If you have a federated ID, use `bx login --sso` to log in to the {{site.data.keyword.Bluemix_notm}} CLI. Enter your user name and use the provided URL in your CLI output to retrieve your one-time passcode. You know you have a federated ID when the login fails without the `--sso` and succeeds with the `--sso` option.

3. If you have multiple {{site.data.keyword.Bluemix_notm}} accounts, select the account where you want to create your Kubernetes cluster.

4.  If you want to create or access Kubernetes clusters in a region other than the {{site.data.keyword.Bluemix_notm}} region that you selected earlier, run `bx cs region-set`.

6.  Create a cluster.

    1.  **Standard clusters**: Review the locations that are available. The locations that are shown depend on the {{site.data.keyword.containershort_notm}} region that you are logged in.

        ```
        bx cs locations
        ```
        {: pre}

        Your CLI output matches the [locations for the container region](cs_regions.html#locations).

    2.  **Standard clusters**: Choose a location and review the machine types available in that location. The machine type specifies the virtual or physical compute hosts that are available to each worker node.

        -  View the **Server Type** field to choose virtual or physical (bare metal) machines.
        -  **Virtual**: Billed hourly, virtual machines are provisioned on shared or dedicated hardware.
        -  **Physical**: Billed monthly, bare metal servers are provisioned by manual interaction with IBM Cloud infrastructure (SoftLayer), and can take more than one business day to complete. Bare metal is best suited for high-performance applications that need more resources and host control.
        - **Physical machines with Trusted Compute**: For bare metal clusters that run Kubernetes version 1.9 or later, you can also choose to enable [Trusted Compute](cs_secure.html#trusted_compute) to verify your bare metal worker nodes against tampering. If you don't enable trust during cluster creation but want to later, you can use the `bx cs feature-enable` [command](cs_cli_reference.html#cs_cluster_feature_enable). After you enable trust, you cannot disable it later.
        -  **Machine types**: To decide what machine type to deploy, review the core, memory, and storage combinations or consult the `bx cs machine-type` [command documentation](cs_cli_reference.html#cs_machine_types). After you create your cluster, you can add different physical or virtual machine types by using the `bx cs worker-add` [command](cs_cli_reference.html#cs_worker_add).

           Be sure that you want to provision a bare metal machine. Because it is billed monthly, if you cancel it immediately after an order by  mistake, you are still charged the full month.
           {:tip}

        ```
        bx cs machine-types <location>
        ```
        {: pre}

    3.  **Standard clusters**: Check to see if a public and private VLAN already exists in the IBM Cloud infrastructure (SoftLayer) for this account.

        ```
        bx cs vlans <location>
        ```
        {: pre}

        ```
        ID        Name   Number   Type      Router
        1519999   vlan   1355     private   bcr02a.dal10
        1519898   vlan   1357     private   bcr02a.dal10
        1518787   vlan   1252     public    fcr02a.dal10
        1518888   vlan   1254     public    fcr02a.dal10
        ```
        {: screen}

        If a public and private VLAN already exist, note the matching routers. Private VLAN routers always begin with <code>bcr</code> (back-end router) and public VLAN routers always begin with <code>fcr</code> (front-end router). When creating a cluster and specifying the public and private VLANs, the number and letter combination after those prefixes must match. In the example output, any of the private VLANs can be used with any of public VLANs because the routers all include `02a.dal10`.

        You must connect your worker nodes to a private VLAN, and optionally, you can connect your worker nodes to a public VLAN. **Note**: If worker nodes are set up with a private VLAN only, you must configure an alternative solution for network connectivity. For more information, see [VLAN connection for worker nodes](#worker_vlan_connection).

    4.  **Free and standard clusters**: Run the `cluster-create` command. You can choose between a free cluster, which includes one worker node set up with 2vCPU and 4GB memory, or a standard cluster, which can include as many worker nodes as you choose in your IBM Cloud infrastructure (SoftLayer) account. When you create a standard cluster, by default, the worker node disks are encrypted, its hardware is shared by multiple IBM customers, and it is billed by hours of usage. </br>Example for a standard cluster. Specify the cluster's options:

        ```
        bx cs cluster-create --location dal10 --machine-type u2c.2x4 --hardware <shared_or_dedicated> --public-vlan <public_vlan_id> --private-vlan <private_vlan_id> --workers 3 --name <cluster_name> --kube-version <major.minor.patch> [--disable-disk-encrypt] [--trusted]
        ```
        {: pre}

        Example for a free cluster. Specify the cluster name:

        ```
        bx cs cluster-create --name my_cluster
        ```
        {: pre}

        <table>
        <caption>Table. Understanding <code>bx cs cluster-create</code> command components</caption>
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
        <td>**Standard clusters**: Replace <em>&lt;location&gt;</em> with the {{site.data.keyword.Bluemix_notm}} location ID where you want to create your cluster. [Available locations](cs_regions.html#locations) depend on the {{site.data.keyword.containershort_notm}} region you are logged in to.</td>
        </tr>
        <tr>
        <td><code>--machine-type <em>&lt;machine_type&gt;</em></code></td>
        <td>**Standard clusters**: Choose a machine type. You can deploy your worker nodes as virtual machines on shared or dedicated hardware, or as physical machines on bare metal. Available physical and virtual machines types vary by the location in which you deploy the cluster. For more information, see the documentation for the `bx cs machine-type` [command](cs_cli_reference.html#cs_machine_types). For free clusters, you do not have to define the machine type.</td>
        </tr>
        <tr>
        <td><code>--hardware <em>&lt;shared_or_dedicated&gt;</em></code></td>
        <td>**Standard clusters, virtual-only**: The level of hardware isolation for your worker node. Use dedicated to have available physical resources dedicated to you only, or shared to allow physical resources to be shared with other IBM customers. The default is shared. This value is optional for standard clusters and is not available for free clusters.</td>
        </tr>
        <tr>
        <td><code>--public-vlan <em>&lt;public_vlan_id&gt;</em></code></td>
        <td><ul>
          <li>**Free clusters**: You do not have to define a public VLAN. Your free cluster is automatically connected to a public VLAN that is owned by IBM.</li>
          <li>**Standard clusters**: If you already have a public VLAN set up in your IBM Cloud infrastructure (SoftLayer) account for that location, enter the ID of the public VLAN. If you want to connect your worker nodes to a private VLAN only, do not specify this option. **Note**: If worker nodes are set up with a private VLAN only, you must configure an alternative solution for network connectivity. For more information, see [VLAN connection for worker nodes](#worker_vlan_connection).<br/><br/>
          <strong>Note</strong>: Private VLAN routers always begin with <code>bcr</code> (back-end router) and public VLAN routers always begin with <code>fcr</code> (front-end router). When creating a cluster and specifying the public and private VLANs, the number and letter combination after those prefixes must match.</li>
        </ul></td>
        </tr>
        <tr>
        <td><code>--private-vlan <em>&lt;private_vlan_id&gt;</em></code></td>
        <td><ul><li>**Free clusters**: You do not have to define a private VLAN. Your free cluster is automatically connected to a private VLAN that is owned by IBM.</li><li>**Standard clusters**: If you already have a private VLAN set up in your IBM Cloud infrastructure (SoftLayer) account for that location, enter the ID of the private VLAN. If you do not have a private VLAN in your account, do not specify this option. {{site.data.keyword.containershort_notm}} automatically creates a private VLAN for you.<br/><br/><strong>Note</strong>: Private VLAN routers always begin with <code>bcr</code> (back-end router) and public VLAN routers always begin with <code>fcr</code> (front-end router). When creating a cluster and specifying the public and private VLANs, the number and letter combination after those prefixes must match.</li></ul></td>
        </tr>
        <tr>
        <td><code>--name <em>&lt;name&gt;</em></code></td>
        <td>**Free and standard clusters**: Replace <em>&lt;name&gt;</em> with a name for your cluster. The name must start with a letter, can contain letters, numbers, and -, and must be 35 characters or fewer. Note that the {{site.data.keyword.IBM_notm}}-assigned Ingress subdomain is derived from the cluster name. The cluster name and Ingress subdomain together form the fully qualified domain name, which must be unique within a region and have 63 characters or fewer. To meet these requirements, the cluster name might be truncated or the subdomain might be assigned random character values.</td>
        </tr>
        <tr>
        <td><code>--workers <em>&lt;number&gt;</em></code></td>
        <td>**Standard clusters**: The number of worker nodes to include in the cluster. If the <code>--workers</code> option is not specified, 1 worker node is created.</td>
        </tr>
        <tr>
          <td><code>--kube-version <em>&lt;major.minor.patch&gt;</em></code></td>
          <td>**Standard clusters**: The Kubernetes version for the cluster master node. This value is optional. Unless specified, the cluster is created with the default of supported Kubernetes versions. To see available versions, run <code>bx cs kube-versions</code>.</td>
        </tr>
        <tr>
        <td><code>--disable-disk-encrypt</code></td>
        <td>**Free and standard clusters**: Worker nodes feature disk encryption by default; [learn more](cs_secure.html#encrypted_disks). If you want to disable encryption, include this option.</td>
        </tr>
        <tr>
        <td><code>--trusted</code></td>
        <td>**Standard bare metal clusters**: Enable [Trusted Compute](cs_secure.html#trusted_compute) to verify your bare metal worker nodes against tampering. If you don't enable trust during cluster creation but want to later, you can use the `bx cs feature-enable` [command](cs_cli_reference.html#cs_cluster_feature_enable). After you enable trust, you cannot disable it later.</td>
        </tr>
        </tbody></table>

7.  Verify that the creation of the cluster was requested.

    ```
    bx cs clusters
    ```
    {: pre}

    **Note:** For virtual machines, it can take a few minutes for the worker node machines to be ordered, and for the cluster to be set up and provisioned in your account. Bare metal physical machines are provisioned by manual interaction with IBM Cloud infrastructure (SoftLayer), and can take more than one business day to complete.

    When the provisioning of your cluster is completed, the status of your cluster changes to **deployed**.

    ```
    Name         ID                                   State      Created          Workers   Location   Version
    my_cluster   paf97e8843e29941b49c598f516de72101   deployed   20170201162433   1         mil01      1.8.8
    ```
    {: screen}

8.  Check the status of the worker nodes.

    ```
    bx cs workers <cluster>
    ```
    {: pre}

    When the worker nodes are ready, the state changes to **normal** and the status is **Ready**. When the node status is **Ready**, you can then access the cluster.

    **Note:** Every worker node is assigned a unique worker node ID and domain name that must not be changed manually after the cluster is created. Changing the ID or domain name prevents the Kubernetes master from managing your cluster.

    ```
    ID                                                 Public IP       Private IP       Machine Type   State    Status   Location   Version
    kube-mil01-paf97e8843e29941b49c598f516de72101-w1   169.47.223.113  10.171.42.93    free           normal   Ready    mil01      1.8.8
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

10. Launch your Kubernetes dashboard with the default port `8001`.
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


-   [Deploy an app in your cluster.](cs_app.html#app_cli)
-   [Manage your cluster with the `kubectl` command line. ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/user-guide/kubectl/)
-   [Set up your own private registry in {{site.data.keyword.Bluemix_notm}} to store and share Docker images with other users.](/docs/services/Registry/index.html)
- If you have multiple VLANs for a cluster or multiple subnets on the same VLAN, you must [turn on VLAN spanning](/docs/infrastructure/vlans/vlan-spanning.html#enable-or-disable-vlan-spanning) so that your worker nodes can communicate with each other on the private network.
- If you have a firewall, you might need to [open the required ports](cs_firewall.html#firewall) to use `bx`, `kubectl`, or `calicotl` commands, to allow outbound traffic from your cluster, or to allow inbound traffic for networking services.

<br />






## Viewing cluster states
{: #states}

Review the state of a Kubernetes cluster to get information about the availability and capacity of the cluster, and potential problems that might have occurred.
{:shortdesc}

To view information about a specific cluster, such as its location, master URL, Ingress subdomain, version, workers, owner, and monitoring dashboard, use the `bx cs cluster-get <mycluster>` [command](cs_cli_reference.html#cs_cluster_get). Include the `--showResources` flag to view more cluster resources such as add-ons for storage pods or subnet VLANs for public and private IPs.

You can view the current cluster state by running the `bx cs clusters` command and locating the **State** field. To troubleshoot your cluster and worker nodes, see [Troubleshooting clusters](cs_troubleshoot.html#debug_clusters).

<table summary="Every table row should be read left to right, with the cluster state in column one and a description in column two.">
   <thead>
   <th>Cluster state</th>
   <th>Description</th>
   </thead>
   <tbody>
<tr>
   <td>Aborted</td>
   <td>The deletion of the cluster is requested by the user before the Kubernetes master is deployed. After the deletion of the cluster is completed, the cluster is removed from your dashboard. If your cluster is stuck in this state for a long time, open an [{{site.data.keyword.Bluemix_notm}} support ticket](cs_troubleshoot.html#ts_getting_help).</td>
   </tr>
 <tr>
     <td>Critical</td>
     <td>The Kubernetes master cannot be reached or all worker nodes in the cluster are down. </td>
    </tr>
   <tr>
     <td>Delete failed</td>
     <td>The Kubernetes master or at least one worker node cannot be deleted.  </td>
   </tr>
   <tr>
     <td>Deleted</td>
     <td>The cluster is deleted but not yet removed from your dashboard. If your cluster is stuck in this state for a long time, open an [{{site.data.keyword.Bluemix_notm}} support ticket](cs_troubleshoot.html#ts_getting_help). </td>
   </tr>
   <tr>
   <td>Deleting</td>
   <td>The cluster is being deleted and cluster infrastructure is being dismantled. You cannot access the cluster.  </td>
   </tr>
   <tr>
     <td>Deploy failed</td>
     <td>The deployment of the Kubernetes master could not be completed. You cannot resolve this state. Contact IBM Cloud support by opening an [{{site.data.keyword.Bluemix_notm}} support ticket](cs_troubleshoot.html#ts_getting_help).</td>
   </tr>
     <tr>
       <td>Deploying</td>
       <td>The Kubernetes master is not fully deployed yet. You cannot access your cluster. Wait until your cluster is fully deployed to review the health of your cluster.</td>
      </tr>
      <tr>
       <td>Normal</td>
       <td>All worker nodes in a cluster are up and running. You can access the cluster and deploy apps to the cluster. This state is considered healthy and does not require an action from you.</td>
    </tr>
      <tr>
       <td>Pending</td>
       <td>The Kubernetes master is deployed. The worker nodes are being provisioned and are not available in the cluster yet. You can access the cluster, but you cannot deploy apps to the cluster.  </td>
     </tr>
   <tr>
     <td>Requested</td>
     <td>A request to create the cluster and order the infrastructure for the Kubernetes master and worker nodes is sent. When the deployment of the cluster starts, the cluster state changes to <code>Deploying</code>. If your cluster is stuck in the <code>Requested</code> state for a long time, open an [{{site.data.keyword.Bluemix_notm}} support ticket](cs_troubleshoot.html#ts_getting_help). </td>
   </tr>
   <tr>
     <td>Updating</td>
     <td>The Kubernetes API server that runs in your Kubernetes master is being updated to a new Kubernetes API version. During the update you cannot access or change the cluster. Worker nodes, apps, and resources that have been deployed by the user are not modified and continue to run. Wait for the update to complete to review the health of your cluster. </td>
   </tr>
    <tr>
       <td>Warning</td>
       <td>At least one worker node in the cluster is not available, but other worker nodes are available and can take over the workload. </td>
    </tr>
   </tbody>
 </table>

<br />


## Removing clusters
{: #remove}

When you are finished with a cluster, you can remove it so that the cluster is no longer consuming resources.
{:shortdesc}

Free and standard clusters created with a Pay-As-You-Go account must be removed manually by the user when they are not needed anymore.

When you delete a cluster, you are also deleting resources on the cluster, including containers, pods, bound services, and secrets. If you do not delete your storage when you delete your cluster, you can delete your storage through the IBM Cloud infrastructure (SoftLayer) dashboard in the {{site.data.keyword.Bluemix_notm}} GUI. Due to the monthly billing cycle, a persistent volume claim cannot be deleted on the last day of a month. If you delete the persistent volume claim on the last day of the month, the deletion remains pending until the beginning of the next month.

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

When you remove a cluster, you can choose to remove the portable subnets and persistent storage associated with it:
- Subnets are used to assign portable public IP addresses to load balancer services or your Ingress application load balancer. If you keep them, you can [reuse them in a new cluster](cs_subnets.html#custom) or manually delete them later from your IBM Cloud infrastructure (SoftLayer) portfolio.
- If you created a persistent volume claim by using an [existing file share](cs_storage.html#existing), then you cannot delete the file share when you delete the cluster. You must manually delete the file share later from your IBM Cloud infrastructure (SoftLayer) portfolio.
- Persistent storage provides high availability for your data. If you delete it, you cannot recover your data.

**Tip**: After it is no longer listed in the available clusters list, you can reuse the name of a removed cluster.

  ```
  bx cs clusters
  ```
  {: pre}
