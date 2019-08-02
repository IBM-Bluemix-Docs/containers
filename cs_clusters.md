---

copyright:
  years: 2014, 2019
lastupdated: "2019-07-31"

keywords: kubernetes, iks, clusters, worker nodes, worker pools, delete

subcollection: containers

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:preview: .preview}
{:gif: data-image-type='gif'}



# Creating clusters
{: #clusters}

Create a Kubernetes cluster in {{site.data.keyword.containerlong}}.
{: shortdesc}

Still getting started? Try out the [creating a Kubernetes cluster tutorial](/docs/containers?topic=containers-cs_cluster_tutorial#cs_cluster_tutorial). Not sure which cluster setup to choose? See [Planning your cluster network setup](/docs/containers?topic=containers-plan_clusters).
{: tip}

Have you created a cluster before and are just looking for quick example commands? Try these examples.
*  **Free cluster**:
   ```
   ibmcloud ks cluster-create --name my_cluster
   ```
   {: pre}
*  **Standard cluster, shared virtual machine**:
   ```
   ibmcloud ks cluster-create --name my_cluster --zone dal10 --machine-type b3c.4x16 --hardware shared --workers 3 --public-vlan <public_VLAN_ID> --private-vlan <private_VLAN_ID>
   ```
   {: pre}
*  **Standard cluster, bare metal**:
   ```
   ibmcloud ks cluster-create --name my_cluster --zone dal10 --machine-type mb2c.4x32 --hardware dedicated --workers 3 --public-vlan <public_VLAN_ID> --private-vlan <private_VLAN_ID>
   ```
   {: pre}
*  **Standard cluster, virtual machine with public and private service endpoints in a VRF-enabled account**:
   ```
   ibmcloud ks cluster-create --name my_cluster --zone dal10 --machine-type b3c.4x16 --hardware shared --workers 3 --public-service-endpoint --private-service-endpoint --public-vlan <public_VLAN_ID> --private-vlan <private_VLAN_ID>
   ```
   {: pre}
*  **Standard cluster that uses private VLANs and the private service endpoint only**:
   ```
   ibmcloud ks cluster-create --name my_cluster --zone dal10 --machine-type b3c.4x16 --hardware shared --workers 3 --private-service-endpoint --private-vlan <private_VLAN_ID> --private-only
   ```
   {: pre}
*   **For a multizone cluster, after you created the cluster in a [multizone metro](/docs/containers?topic=containers-regions-and-zones#zones), [add zones](/docs/containers?topic=containers-add_workers#add_zone)**:
    ```
    ibmcloud ks zone-add --zone <zone> --cluster <cluster_name_or_ID> --worker-pools <pool1_name,pool2_name,...> --private-vlan <private_VLAN_ID> --public-vlan <public_VLAN_ID>
    ```
    {: pre}

<br />



## Prepare to create clusters at the account level
{: #cluster_prepare}

Prepare your {{site.data.keyword.cloud_notm}} account for {{site.data.keyword.containerlong_notm}}. These are preparations that, after the account administrator makes them, you might not need to change each time that you create a cluster. However, each time that you create a cluster, you still want to verify that the current account-level state is what you need it to be.
{: shortdesc}

1. [Create or upgrade your account to a billable account ({{site.data.keyword.cloud_notm}} Pay-As-You-Go or Subscription)](https://cloud.ibm.com/registration/).

2. [Set up an {{site.data.keyword.containerlong_notm}} API key](/docs/containers?topic=containers-users#api_key) in the regions that you want to create clusters. Assign the API key with the appropriate permissions to create clusters:
  * **Super User** role or the [minimum required permissions](/docs/containers?topic=containers-access_reference#infra) for classic infrastructure.
  * **Administrator** platform management role for {{site.data.keyword.containershort_notm}} at the account level.
  * **Administrator** platform management role for Container Registry at the account level. If your account predates 4 October 2018, you need to [enable {{site.data.keyword.cloud_notm}} IAM policies for {{site.data.keyword.registryshort_notm}}](/docs/services/Registry?topic=registry-user#existing_users). With IAM policies, you can control access to resources such as registry namespaces.

  Are you the account owner? You already have the necessary permissions! When you create a cluster, the API key for that region and resource group is set with your credentials.
  {: tip}

3. Verify that you have the **Administrator** platform role for {{site.data.keyword.containerlong_notm}}. To allow your cluster to pull images from the private registry, you also need the **Administrator** platform role for {{site.data.keyword.registrylong_notm}}.
  1. From the [{{site.data.keyword.cloud_notm}} console ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/) menu bar, click **Manage > Access (IAM)**.
  2. Click the **Users** page, and then from the table, select yourself.
  3. From the **Access policies** tab, confirm that your **Role** is **Administrator**. You can be the **Administrator** for all the resources in the account, or at least for {{site.data.keyword.containershort_notm}}. **Note**: If you have the **Administrator** role for {{site.data.keyword.containershort_notm}} in only one resource group or region instead of the entire account, you must have at least the **Viewer** role at the account level to see the account's VLANs.
  <p class="tip">Make sure that your account administrator does not assign you the **Administrator** platform role at the same time as a service role. You must assign platform and service roles separately.</p>

4. If your account uses multiple resource groups, figure out your account's strategy for [managing resource groups](/docs/containers?topic=containers-users#resource_groups).
  * The cluster is created in the resource group that you target when you log in to {{site.data.keyword.cloud_notm}}. If you do not target a resource group, the default resource group is automatically targeted.
  * If you want to create a cluster in a different resource group than the default, you need at least the **Viewer** role for the resource group. If you do not have any role for the resource group but are still an **Administrator** for the service within the resource group, your cluster is created in the default resource group.
  * You cannot change a cluster's resource group. Furthermore, if you need to use the `ibmcloud ks cluster-service-bind` [command](/docs/containers-cli-plugin?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_service_bind) to [integrate with an {{site.data.keyword.cloud_notm}} service](/docs/containers?topic=containers-service-binding#bind-services), that service must be in the same resource group as the cluster. Services that do not use resource groups like {{site.data.keyword.registrylong_notm}} or that do not need service binding like {{site.data.keyword.la_full_notm}} work even if the cluster is in a different resource group.
  * If you plan to use [{{site.data.keyword.monitoringlong_notm}} for metrics](/docs/containers?topic=containers-health#view_metrics), plan to give your cluster a name that is unique across all resource groups and regions in your account to avoid metrics naming conflicts.
  * Free clusters are created in the `default` resource group.

5. **Standard clusters**: Plan your cluster [network setup](/docs/containers?topic=containers-plan_clusters) so that your cluster meets the needs of your workloads and environment. Then, set up your IBM Cloud infrastructure networking to allow worker-to-master and user-to-master communication:
  * To use the private service endpoint only or the public and private service endpoints (run internet-facing workloads or extend your on-premises data center):
    1. Enable [VRF](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud) in your IBM Cloud infrastructure account. To check whether a VRF is already enabled, use the `ibmcloud account show` command.
    2. [Enable your {{site.data.keyword.cloud_notm}} account to use service endpoints](/docs/resources?topic=resources-private-network-endpoints#getting-started).
    <p class="note">The Kubernetes master is accessible through the private service endpoint if authorized cluster users are in your {{site.data.keyword.cloud_notm}} private network or are connected to the private network through a [VPN connection](/docs/infrastructure/iaas-vpn?topic=VPN-getting-started) or [{{site.data.keyword.cloud_notm}} Direct Link](/docs/infrastructure/direct-link?topic=direct-link-get-started-with-ibm-cloud-direct-link). However, communication with the Kubernetes master over the private service endpoint must go through the <code>166.X.X.X</code> IP address range, which is not routable from a VPN connection or through {{site.data.keyword.cloud_notm}} Direct Link. You can expose the private service endpoint of the master for your cluster users by using a private network load balancer (NLB). The private NLB exposes the private service endpoint of the master as an internal <code>10.X.X.X</code> IP address range that users can access with the VPN or {{site.data.keyword.cloud_notm}} Direct Link connection. If you enable only the private service endpoint, you can use the Kubernetes dashboard or temporarily enable the public service endpoint to create the private NLB. For more information, see [Accessing clusters through the private service endpoint](/docs/containers?topic=containers-clusters#access_on_prem).</p>

  * To use the public service endpoint only (run internet-facing workloads):
    1. Enable [VLAN spanning](/docs/infrastructure/vlans?topic=vlans-vlan-spanning#vlan-spanning) for your IBM Cloud infrastructure account so your worker nodes can communicate with each other on the private network. To perform this action, you need the **Network > Manage Network VLAN Spanning** [infrastructure permission](/docs/containers?topic=containers-users#infra_access), or you can request the account owner to enable it. To check whether VLAN spanning is already enabled, use the `ibmcloud ks vlan-spanning-get --region <region>` [command](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlan_spanning_get).
  * To use a gateway device (extend your on-premises data center):
    1. Enable [VLAN spanning](/docs/infrastructure/vlans?topic=vlans-vlan-spanning#vlan-spanning) for your IBM Cloud infrastructure account so your worker nodes can communicate with each other on the private network. To perform this action, you need the **Network > Manage Network VLAN Spanning** [infrastructure permission](/docs/containers?topic=containers-users#infra_access), or you can request the account owner to enable it. To check whether VLAN spanning is already enabled, use the `ibmcloud ks vlan-spanning-get --region <region>` [command](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlan_spanning_get).
    2. Configure a gateway device. For example, you might choose to set up a [Virtual Router Appliance](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-about-the-vra) or a [Fortigate Security Appliance](/docs/services/vmwaresolutions/services?topic=vmware-solutions-fsa_considerations) to act as your firewall to allow necessary traffic and block unwanted traffic.
    3. [Open up the required private IP addresses and ports](/docs/containers?topic=containers-firewall#firewall_outbound) for each region so that the master and the worker nodes can communicate and for the {{site.data.keyword.cloud_notm}} services that you plan to use.

<br />


## Prepare to create clusters at the cluster level
{: #prepare_cluster_level}

After you set up your account to create clusters, prepare the setup of your cluster. These are preparations that impact your cluster each time that you create a cluster.
{: shortdesc}

1. Decide between a [free or standard cluster](/docs/containers?topic=containers-cs_ov#cluster_types). You can create one free cluster to try out some of the capabilities for 30 days, or create fully-customizable standard clusters with your choice of hardware isolation. Create a standard cluster to get more benefits and control over your cluster performance.

2. For standard clusters, plan your cluster setup.
  * Decide whether to create a [single zone](/docs/containers?topic=containers-ha_clusters#single_zone) or [multizone](/docs/containers?topic=containers-ha_clusters#multizone) cluster. Note that multizone clusters are available in select locations only.
  * Choose what type of [hardware and isolation](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes) you want for your cluster's worker nodes, including the decision between virtual or bare metal machines.

3. For standard clusters, you can [estimate the cost](/docs/billing-usage?topic=billing-usage-cost#cost) in the {{site.data.keyword.cloud_notm}} console. For more information about charges that might not be included in the estimator, see [Pricing and billing](/docs/containers?topic=containers-faqs#charges).

4. If you create the cluster in an environment behind a firewall, such as for clusters that extend your on-premises data center, [allow outbound network traffic to the public and private IPs](/docs/containers?topic=containers-firewall#firewall_outbound) for the {{site.data.keyword.cloud_notm}} services that you plan to use.

<br />


## Creating a free cluster
{: #clusters_free}

You can use your one free cluster to become familiar with how {{site.data.keyword.containerlong_notm}} works. With free clusters, you can learn the terminology, complete a tutorial, and get your bearings before you take the leap to production-level standard clusters. Don't worry, you still get a free cluster even if you have a billable account.
{: shortdesc}

Free clusters include one worker node set up with two vCPU and four GB memory and have a life span of 30 days. After that time, the cluster expires and the cluster and its data are deleted. The deleted data is not backed up by {{site.data.keyword.cloud_notm}} and cannot be restored. Be sure to back up any important data.
{: note}

### Creating a free cluster in the console
{: #clusters_ui_free}

1. In the [catalog ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/catalog?category=containers), select **{{site.data.keyword.containershort_notm}}** to create a cluster.
2. Select the **Free** cluster plan.
3. Select a geography in which to deploy your cluster.
4. Select a metro location in the geography. Your cluster is created in a zone within this metro.
5. Give your cluster a name. The name must start with a letter, can contain letters, numbers, and hyphen (-), and must be 35 characters or fewer. Use a name that is unique across regions.
6. Click **Create cluster**. By default, a worker pool with one worker node is created. You can see the progress of the worker node deployment in the **Worker nodes** tab. When the deployment is done, you can see that your cluster is ready in the **Overview** tab. Note that even if the cluster is ready, some parts of the cluster that are used by other services, such as registry image pull secrets, might still be in process.

  Every worker node is assigned a unique worker node ID and domain name that must not be changed manually after the cluster is created. Changing the ID or domain name prevents the Kubernetes master from managing your cluster.
  {: important}
7. After your cluster is created, you can [begin working with your cluster by configuring your CLI session](#access_internet).

### Creating a free cluster in the CLI
{: #clusters_cli_free}

Before you begin, install the {{site.data.keyword.cloud_notm}} CLI and the [{{site.data.keyword.containerlong_notm}} plug-in](/docs/containers?topic=containers-cs_cli_install#cs_cli_install).

1. Log in to the {{site.data.keyword.cloud_notm}} CLI.
  1. Log in and enter your {{site.data.keyword.cloud_notm}} credentials when prompted.
     ```
     ibmcloud login
     ```
     {: pre}

     If you have a federated ID, use `ibmcloud login --sso` to log in to the {{site.data.keyword.cloud_notm}} CLI. Enter your user name and use the provided URL in your CLI output to retrieve your one-time passcode. You know that you have a federated ID when the login fails without the `--sso` and succeeds with the `--sso` option.
     {: tip}

  2. If you have multiple {{site.data.keyword.cloud_notm}} accounts, select the account where you want to create your Kubernetes cluster.

  3. To create the free cluster in a specific region, you must target that region. You can create a free cluster in `ap-south`, `eu-central`, `uk-south`, or `us-south`. The cluster is created in a zone within that region.
     ```
     ibmcloud ks region-set
     ```
     {: pre}

2. Create a cluster.
  ```
  ibmcloud ks cluster-create --name my_cluster
  ```
  {: pre}

3. **Free clusters in the London metro only**: Currently, you must target the EU Central regional API to work with your cluster.
  ```
  ibmcloud ks init --host https://eu-gb.containers.cloud.ibm.com
  ```
  {: pre}

3. Verify that the creation of the cluster was requested. It can take a few minutes for the worker node machine to be ordered.
    ```
    ibmcloud ks clusters
    ```
    {: pre}

    When the provisioning of your cluster is completed, the status of your cluster changes to **deployed**.
    ```
    Name         ID                                   State      Created          Workers   Zone       Version     Resource Group Name
    my_cluster   paf97e8843e29941b49c598f516de72101   deployed   20170201162433   1         mil01      1.13.8      Default
    ```
    {: screen}

4. Check the status of the worker node.
    ```
    ibmcloud ks workers --cluster <cluster_name_or_ID>
    ```
    {: pre}

    When the worker node is ready, the state changes to **normal** and the status is **Ready**. When the node status is **Ready**, you can then access the cluster. Note that even if the cluster is ready, some parts of the cluster that are used by other services, such as Ingress secrets or registry image pull secrets, might still be in process.
    ```
    ID                                                 Public IP       Private IP      Machine Type   State    Status   Zone        Version     Resource Group Name
    kube-mil01-paf97e8843e29941b49c598f516de72101-w1   169.xx.xxx.xxx  10.xxx.xx.xxx   free           normal   Ready    mil01       1.13.8      Default
    ```
    {: screen}

    Every worker node is assigned a unique worker node ID and domain name that must not be changed manually after the cluster is created. Changing the ID or domain name prevents the Kubernetes master from managing your cluster.
    {: important}

5. After your cluster is created, you can [begin working with your cluster by configuring your CLI session](#access_internet).

<br />


## Creating a standard cluster
{: #clusters_standard}

Use the {{site.data.keyword.cloud_notm}} CLI or the {{site.data.keyword.cloud_notm}} console to create a fully-customizable standard cluster with your choice of hardware isolation and access to features like multiple worker nodes for a highly available environment.
{: shortdesc}

### Creating a standard cluster in the console
{: #clusters_ui}

1. In the [catalog ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/catalog?category=containers), select **{{site.data.keyword.containershort_notm}}** to create a cluster.

2. Select a resource group in which to create your cluster.
    * A cluster can be created in only one resource group, and after the cluster is created, you can't change its resource group.
    * To create clusters in a resource group other than the default, you must have at least the [**Viewer** role](/docs/containers?topic=containers-users#platform) for the resource group.

3. Select a geography in which to deploy your cluster.

4. Give your cluster a unique name. The name must start with a letter, can contain letters, numbers, and hyphen (-), and must be 35 characters or fewer. Use a name that is unique across regions. The cluster name and the region in which the cluster is deployed form the fully qualified domain name for the Ingress subdomain. To ensure that the Ingress subdomain is unique within a region, the cluster name might be truncated and appended with a random value within the Ingress domain name.
 **Note**: Changing the unique ID or domain name that is assigned during creation blocks the Kubernetes master from managing your cluster.

5. Select **Single zone** or **Multizone** availability. In a multizone cluster, the master node is deployed in a multizone-capable zone and your cluster's resources are spread across multiple zones.

6. Enter your metro and zone details.
  * Multizone clusters:
    1. Select a metro location. For the best performance, select the metro location that is physically closest to you. Your choices might be limited by geography.
    2. Select the specific zones in which you want to host your cluster. You must select at least one zone but you can select as many as you would like. If you select more than one zone, the worker nodes are spread across the zones that you choose which gives you higher availability. If you select only one zone, you can [add zones to your cluster](/docs/containers?topic=containers-add_workers#add_zone) after it is created.
  * Single zone clusters: Select a zone in which you want to host your cluster. For the best performance, select the zone that is physically closest to you. Your choices might be limited by geography.

7. For each zone, choose VLANs.
  * To create a cluster in which you can run internet-facing workloads:
    1. Select a public VLAN and a private VLAN from your IBM Cloud infrastructure account for each zone. Worker nodes communicate with each other by using the private VLAN, and can communicate with the Kubernetes master by using the public or the private VLAN. If you do not have a public or private VLAN in this zone, a public and a private VLAN is automatically created for you. You can use the same VLAN for multiple clusters.
  * To create a cluster that extends your on-premises data center on the private network only, that extends your on-premises data center with the option of adding limited public access later, or that extends your on-premises data center and provides limited public access through a gateway device:
    1. Select a private VLAN from your IBM Cloud infrastructure account for each zone. Worker nodes communicate with each other by using the private VLAN. If you do not have a private VLAN in a zone, a private VLAN is automatically created for you. You can use the same VLAN for multiple clusters.
    2. For the public VLAN, select **None**.

8. For **Master service endpoint**, choose how your Kubernetes master and worker nodes communicate.
  * To create a cluster in which you can run internet-facing workloads:
    * If VRF and service endpoints are enabled in your {{site.data.keyword.cloud_notm}} account, select **Both private & public endpoints**.
    * If you cannot or do not want to enable VRF, select **Public endpoint only**.
  * To create a cluster that extends your on-premises data center only, or a cluster that extends your on-premises data center and provides limited public access with edge worker nodes, select **Both private & public endpoints** or **Private endpoint only**. Ensure that you have enabled VRF and service endpoints in your {{site.data.keyword.cloud_notm}} account. Note that if you enable the private service endpoint only, you must [expose the master endpoint through a private network load balancer](#access_on_prem) so that users can access the master through a VPN or {{site.data.keyword.BluDirectLink}} connection.
  * To create a cluster that extends your on-premises data center and provides limited public access with a gateway device, select **Public endpoint only**.

9. Configure your default worker pool. Worker pools are groups of worker nodes that share the same configuration. You can always add more worker pools to your cluster later.
  1. Choose the Kubernetes API server version for the cluster master node and worker nodes.
  2. Filter the worker flavors by selecting a machine type. Virtual is billed hourly and bare metal is billed monthly.
    - **Bare metal**: Billed monthly, bare metal servers are provisioned manually by IBM Cloud infrastructure after you order, and can take more than one business day to complete. Bare metal is best suited for high-performance applications that need more resources and host control. Be sure that you want to provision a bare metal machine. Because it is billed monthly, if you cancel it immediately after an order by mistake, you are still charged the full month.
    - **Virtual - shared**: Infrastructure resources, such as the hypervisor and physical hardware, are shared across you and other IBM customers, but each worker node is accessible only by you. Although this option is less expensive and sufficient in most cases, you might want to verify your performance and infrastructure requirements with your company policies.
    - **Virtual - dedicated**: Your worker nodes are hosted on infrastructure that is devoted to your account. Your physical resources are completely isolated.
  3. Select a flavor. The flavor defines the amount of virtual CPU, memory, and disk space that is set up in each worker node and made available to the containers. Available bare metal and virtual machines types vary by the zone in which you deploy the cluster. After you create your cluster, you can add different flavors by adding a worker or pool to the cluster.
  4. Specify the number of worker nodes that you need in the cluster. The number of workers that you enter is replicated across the number of zones that you selected. This means that if you have two zones and select three worker nodes, six nodes are provisioned, and three nodes live in each zone.

10. Click **Create cluster**. A worker pool is created with the number of workers that you specified. You can see the progress of the worker node deployment in the **Worker nodes** tab.
    *   When the deployment is done, you can see that your cluster is ready in the **Overview** tab. Note that even if the cluster is ready, some parts of the cluster that are used by other services, such as Ingress secrets or registry image pull secrets, might still be in process.
    *   Every worker node is assigned a unique worker node ID and domain name that must not be changed manually after the cluster is created. Changing the ID or domain name prevents the Kubernetes master from managing your cluster.<p class="tip">Is your cluster not in a **deployed** state? Check out the [Debugging clusters](/docs/containers?topic=containers-cs_troubleshoot) guide for help. For example, if your cluster is provisioned in an account that is protected by a firewall gateway device, you must [configure your firewall settings to allow outgoing traffic to the appropriate ports and IP addresses](/docs/containers?topic=containers-firewall#firewall_outbound).</p>

11. After your cluster is created, you can [begin working with your cluster by configuring your CLI session](#access_cluster).

### Creating a standard cluster in the CLI
{: #clusters_cli_steps}

Before you begin, install the {{site.data.keyword.cloud_notm}} CLI and the [{{site.data.keyword.containerlong_notm}} plug-in](/docs/containers?topic=containers-cs_cli_install#cs_cli_install).

1. Log in to the {{site.data.keyword.cloud_notm}} CLI.
  1. Log in and enter your {{site.data.keyword.cloud_notm}} credentials when prompted.
     ```
     ibmcloud login
     ```
     {: pre}

     If you have a federated ID, use `ibmcloud login --sso` to log in to the {{site.data.keyword.cloud_notm}} CLI. Enter your user name and use the provided URL in your CLI output to retrieve your one-time passcode. You know that you have a federated ID when the login fails without the `--sso` and succeeds with the `--sso` option.
     {: tip}

  2. If you have multiple {{site.data.keyword.cloud_notm}} accounts, select the account where you want to create your Kubernetes cluster.

  3. To create clusters in a resource group other than default, target that resource group.

      * A cluster can be created in only one resource group, and after the cluster is created, you can't change its resource group.
      * You must have at least the [**Viewer** role](/docs/containers?topic=containers-users#platform) for the resource group.
      ```
      ibmcloud target -g <resource_group_name>
      ```
      {: pre}

3. Review the zones that are available. In the output of the following command, zones have a **Location Type** of `dc`. To span your cluster across zones, you must create the cluster in a [multizone-capable zone](/docs/containers?topic=containers-regions-and-zones#zones). Multizone-capable zones have a metro value in the **Multizone Metro** column.
    ```
    ibmcloud ks supported-locations
    ```
    {: pre}
    When you select a zone that is located outside your country, keep in mind that you might require legal authorization before data can be physically stored in a foreign country.
    {: note}

4. Review the worker node flavors that are available in that zone. The worker flavor specifies the virtual or physical compute hosts that are available to each worker node.
  - **Virtual**: Billed hourly, virtual machines are provisioned on shared or dedicated hardware.
  - **Physical**: Billed monthly, bare metal servers are provisioned manually by IBM Cloud infrastructure after you order, and can take more than one business day to complete. Bare metal is best suited for high-performance applications that need more resources and host control.
  - **flavors**: To decide what flavor to deploy, review the core, memory, and storage combinations of the [available worker node hardware](/docs/containers?topic=containers-planning_worker_nodes#shared_dedicated_node). After you create your cluster, you can add different physical or virtual flavors by [adding a worker pool](/docs/containers?topic=containers-add_workers#add_pool).

     Be sure that you want to provision a bare metal machine. Because it is billed monthly, if you cancel it immediately after an order by mistake, you are still charged the full month.
     {:tip}

  ```
  ibmcloud ks flavors --zone <zone>
  ```
  {: pre}

4. Check to see whether VLANs for the zone already exist in the IBM Cloud infrastructure for this account.
  ```
  ibmcloud ks vlans --zone <zone>
  ```
  {: pre}

  Example output:
  ```
  ID        Name   Number   Type      Router
  1519999   vlan   1355     private   bcr02a.dal10
  1519898   vlan   1357     private   bcr02a.dal10
  1518787   vlan   1252     public    fcr02a.dal10
  1518888   vlan   1254     public    fcr02a.dal10
  ```
  {: screen}
  * To create a cluster in which you can run internet-facing workloads, check to see whether a public and private VLAN exist. If a public and private VLAN already exist, note the matching routers. Private VLAN routers always begin with <code>bcr</code> (back-end router) and public VLAN routers always begin with <code>fcr</code> (front-end router). When you create a cluster and specify the public and private VLANs, the number and letter combination after those prefixes must match. In the example output, any of the private VLANs can be used with any of public VLANs because the routers all include `02a.dal10`.
  * To create a cluster that extends your on-premises data center on the private network only, that extends your on-premises data center with the option of adding limited public access later through edge worker nodes, or that extends your on-premises data center and provides limited public access through a gateway device, check to see whether a private VLAN exists. If you have a private VLAN, note the ID.

5. Run the `cluster-create` command. By default, the worker node disks are AES 256-bit encrypted and the cluster is billed by hours of usage.
  * To create a cluster in which you can run internet-facing workloads:
    ```
    ibmcloud ks cluster-create --zone <zone> --machine-type <flavor> --hardware <shared_or_dedicated> --public-vlan <public_VLAN_ID> --private-vlan <private_VLAN_ID> --workers <number> --name <cluster_name> --kube-version <major.minor.patch> [--private-service-endpoint] [--public-service-endpoint] [--disable-disk-encrypt]
    ```
    {: pre}
  * To create a cluster that extends your on-premises data center on the private network, with the option of adding limited public access later through edge worker nodes:
    ```
    ibmcloud ks cluster-create --zone <zone> --machine-type <flavor> --hardware <shared_or_dedicated> --private-vlan <private_VLAN_ID> --private-only --workers <number> --name <cluster_name> --kube-version <major.minor.patch> --private-service-endpoint [--public-service-endpoint] [--disable-disk-encrypt]
    ```
    {: pre}
  * To create a cluster that extends your on-premises data center and provides limited public access through a gateway device:
    ```
    ibmcloud ks cluster-create --zone <zone> --machine-type <flavor> --hardware <shared_or_dedicated> --private-vlan <private_VLAN_ID> --private-only --workers <number> --name <cluster_name> --kube-version <major.minor.patch> --public-service-endpoint [--disable-disk-encrypt]
    ```
    {: pre}

    <table>
    <caption>cluster-create components</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding this command's components</th>
    </thead>
    <tbody>
    <tr>
    <td><code>cluster-create</code></td>
    <td>The command to create a cluster in your {{site.data.keyword.cloud_notm}} organization.</td>
    </tr>
    <tr>
    <td><code>--zone <em>&lt;zone&gt;</em></code></td>
    <td>Specify the {{site.data.keyword.cloud_notm}} zone ID where you want to create your cluster that you chose in step 4.</td>
    </tr>
    <tr>
    <td><code>--machine-type <em>&lt;machine_type&gt;</em></code></td>
    <td>Specify the flavor that you chose in step 5.</td>
    </tr>
    <tr>
    <td><code>--hardware <em>&lt;shared_or_dedicated&gt;</em></code></td>
    <td>Specify with the level of hardware isolation for your worker node. Use dedicated to have available physical resources dedicated to you only, or shared to allow physical resources to be shared with other IBM customers. The default is shared. This value is optional for VM standard clusters. For bare metal flavors, specify `dedicated`.</td>
    </tr>
    <tr>
    <td><code>--public-vlan <em>&lt;public_vlan_id&gt;</em></code></td>
    <td>If you already have a public VLAN set up in your IBM Cloud infrastructure account for that zone, enter the ID of the public VLAN that you found in step 4.<p>Private VLAN routers always begin with <code>bcr</code> (back-end router) and public VLAN routers always begin with <code>fcr</code> (front-end router). When you create a cluster and specify the public and private VLANs, the number and letter combination after those prefixes must match.</p></td>
    </tr>
    <tr>
    <td><code>--private-vlan <em>&lt;private_vlan_id&gt;</em></code></td>
    <td>If you already have a private VLAN set up in your IBM Cloud infrastructure account for that zone, enter the ID of the private VLAN that you found in step 4. If you do not have a private VLAN in your account, do not specify this option. {{site.data.keyword.containerlong_notm}} automatically creates a private VLAN for you.<p>Private VLAN routers always begin with <code>bcr</code> (back-end router) and public VLAN routers always begin with <code>fcr</code> (front-end router). When you create a cluster and specify the public and private VLANs, the number and letter combination after those prefixes must match.</p></td>
    </tr>
    <tr>
    <td><code>--private-only</code></td>
    <td>Create the cluster with private VLANs only. If you include this option, do not include the <code>--public-vlan</code> option.</td>
    </tr>
    <tr>
    <td><code>--name <em>&lt;name&gt;</em></code></td>
    <td>Specify a name for your cluster. The name must start with a letter, can contain letters, numbers, and hyphen (-), and must be 35 characters or fewer. Use a name that is unique across regions. The cluster name and the region in which the cluster is deployed form the fully qualified domain name for the Ingress subdomain. To ensure that the Ingress subdomain is unique within a region, the cluster name might be truncated and appended with a random value within the Ingress domain name.
</td>
    </tr>
    <tr>
    <td><code>--workers <em>&lt;number&gt;</em></code></td>
    <td>Specify the number of worker nodes to include in the cluster. If the <code>--workers</code> option is not specified, one worker node is created.</td>
    </tr>
    <tr>
    <td><code>--kube-version <em>&lt;major.minor.patch&gt;</em></code></td>
    <td>The Kubernetes version for the cluster master node. This value is optional. When the version is not specified, the cluster is created with the default of supported Kubernetes versions. To see available versions, run <code>ibmcloud ks versions</code>.
</td>
    </tr>
    <tr>
    <td><code>--private-service-endpoint</code></td>
    <td>**In [VRF-enabled accounts](/docs/resources?topic=resources-private-network-endpoints#getting-started)**: Enable the private service endpoint so that your Kubernetes master and the worker nodes can communicate over the private VLAN. In addition, you can choose to enable the public service endpoint by using the `--public-service-endpoint` flag to access your cluster over the internet. If you enable the private service endpoint only, you must be connected to the private VLAN to communicate with your Kubernetes master. After you enable a private service endpoint, you cannot later disable it.<br><br>After you create the cluster, you can get the endpoint by running `ibmcloud ks cluster-get --cluster <cluster_name_or_ID>`.</td>
    </tr>
    <tr>
    <td><code>--public-service-endpoint</code></td>
    <td>Enable the public service endpoint so that your Kubernetes master can be accessed over the public network, for example to run `kubectl` commands from your terminal, and so that your Kubernetes master and the worker nodes can communicate over the public VLAN. You can later disable the public service endpoint if you want a private-only cluster.<br><br>After you create the cluster, you can get the endpoint by running `ibmcloud ks cluster-get --cluster <cluster_name_or_ID>`.</td>
    </tr>
    <tr>
    <td><code>--disable-disk-encrypt</code></td>
    <td>Worker nodes feature AES 256-bit [disk encryption](/docs/containers?topic=containers-security#encrypted_disk) by default. If you want to disable encryption, include this option.</td>
    </tr>
    </tbody></table>

6. Verify that the creation of the cluster was requested. For virtual machines, it can take a few minutes for the worker node machines to be ordered, and for the cluster to be set up and provisioned in your account. Bare metal physical machines are provisioned by manual interaction with IBM Cloud infrastructure, and can take more than one business day to complete.
    ```
    ibmcloud ks clusters
    ```
    {: pre}

    When the provisioning of your cluster is completed, the status of your cluster changes to **deployed**.
    ```
    Name         ID                                   State      Created          Workers   Zone       Version     Resource Group Name
    my_cluster   paf97e8843e29941b49c598f516de72101   deployed   20170201162433   1         mil01      1.13.8      Default
    ```
    {: screen}

    Is your cluster not in a **deployed** state? Check out the [Debugging clusters](/docs/containers?topic=containers-cs_troubleshoot) guide for help. For example, if your cluster is provisioned in an account that is protected by a firewall gateway device, you must [configure your firewall settings to allow outgoing traffic to the appropriate ports and IP addresses](/docs/containers?topic=containers-firewall#firewall_outbound).
    {: tip}

7. Check the status of the worker nodes.
    ```
    ibmcloud ks workers --cluster <cluster_name_or_ID>
    ```
    {: pre}

    When the worker nodes are ready, the state changes to **normal** and the status is **Ready**. When the node status is **Ready**, you can then access the cluster. Note that even if the cluster is ready, some parts of the cluster that are used by other services, such as Ingress secrets or registry image pull secrets, might still be in process. Note that if you created your cluster with a private VLAN only, no **Public IP** addresses are assigned to your worker nodes.

    ```
    ID                                                 Public IP       Private IP      Machine Type   State    Status   Zone        Version     Resource Group Name
    kube-mil01-paf97e8843e29941b49c598f516de72101-w1   169.xx.xxx.xxx  10.xxx.xx.xxx   standard       normal   Ready    mil01       1.13.8      Default
    ```
    {: screen}

    Every worker node is assigned a unique worker node ID and domain name that must not be changed manually after the cluster is created. Changing the ID or domain name prevents the Kubernetes master from managing your cluster.
    {: important}

8. After your cluster is created, you can [begin working with your cluster by configuring your CLI session](#access_cluster).

<br />


## Accessing your cluster
{: #access_cluster}

After your cluster is created, you can begin working with your cluster by configuring your CLI session.
{: shortdesc}

### Accessing clusters through the public service endpoint
{: #access_internet}

To work with your cluster, set the cluster that you created as the context for a CLI session to run `kubectl` commands.
{: shortdesc}

If you want to use the {{site.data.keyword.cloud_notm}} console instead, you can run CLI commands directly from your web browser in the [Kubernetes Terminal](/docs/containers?topic=containers-cs_cli_install#cli_web).
{: tip}

1. If your network is protected by a company firewall, allow access to the {{site.data.keyword.cloud_notm}} and {{site.data.keyword.containerlong_notm}} API endpoints and ports.
  1. [Allow access to the public endpoints for the `ibmcloud` API and the `ibmcloud ks` API in your firewall](/docs/containers?topic=containers-firewall#firewall_bx).
  2. [Allow your authorized cluster users to run `kubectl` commands](/docs/containers?topic=containers-firewall#firewall_kubectl) to access the master through the public only, private only, or public and private service endpoints.
  3. [Allow your authorized cluster users to run `calicotl` commands](/docs/containers?topic=containers-firewall#firewall_calicoctl) to manage Calico network policies in your cluster.

2. Set the cluster that you created as the context for this session. Complete these configuration steps every time that you work with your cluster.
  1. Get the command to set the environment variable and download the Kubernetes configuration files.
      ```
      ibmcloud ks cluster-config --cluster <cluster_name_or_ID>
      ```
      {: pre}
      When the download of the configuration files is finished, a command is displayed that you can use to set the path to the local Kubernetes configuration file as an environment variable.

      Example for OS X:
      ```
      export KUBECONFIG=/Users/<user_name>/.bluemix/plugins/kubernetes-service/clusters/mycluster/kube-config-prod-dal10-mycluster.yml
      ```
      {: screen}
  2. Copy and paste the command that is displayed in your terminal to set the `KUBECONFIG` environment variable.
  3. Verify that the `KUBECONFIG` environment variable is set properly.
      Example for OS X:
      ```
      echo $KUBECONFIG
      ```
      {: pre}

      Output:
      ```
      /Users/<user_name>/.bluemix/plugins/kubernetes-service/clusters/mycluster/kube-config-prod-dal10-mycluster.yml
      ```
      {: screen}

3. Launch your Kubernetes dashboard with the default port `8001`.
  1. Set the proxy with the default port number.
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

### Accessing clusters through the private service endpoint
{: #access_on_prem}

The Kubernetes master is accessible through the private service endpoint if authorized cluster users are in your {{site.data.keyword.cloud_notm}} private network or are connected to the private network through a [VPN connection](/docs/infrastructure/iaas-vpn?topic=VPN-getting-started) or [{{site.data.keyword.cloud_notm}} Direct Link](/docs/infrastructure/direct-link?topic=direct-link-get-started-with-ibm-cloud-direct-link). However, communication with the Kubernetes master over the private service endpoint must go through the <code>166.X.X.X</code> IP address range, which is not routable from a VPN connection or through {{site.data.keyword.cloud_notm}} Direct Link. You can expose the private service endpoint of the master for your cluster users by using a private network load balancer (NLB). The private NLB exposes the private service endpoint of the master as an internal <code>10.X.X.X</code> IP address range that users can access with the VPN or {{site.data.keyword.cloud_notm}} Direct Link connection. If you enable only the private service endpoint, you can use the Kubernetes dashboard or temporarily enable the public service endpoint to create the private NLB.
{: shortdesc}

1. If your network is protected by a company firewall, allow access to the {{site.data.keyword.cloud_notm}} and {{site.data.keyword.containerlong_notm}} API endpoints and ports.
  1. [Allow access to the public endpoints for the `ibmcloud` API and the `ibmcloud ks` API in your firewall](/docs/containers?topic=containers-firewall#firewall_bx).
  2. [Allow your authorized cluster users to run `kubectl` commands](/docs/containers?topic=containers-firewall#firewall_kubectl). Note that you cannot test the connection to your cluster in step 6 until you expose the private service endpoint of the master to the cluster by using a private NLB.

2. Get the private service endpoint URL and port for your cluster.
  ```
  ibmcloud ks cluster-get --cluster <cluster_name_or_ID>
  ```
  {: pre}

  In this example output, the **Private Service Endpoint URL** is `https://c1.private.us-east.containers.cloud.ibm.com:25073`.
  ```
  Name:                           setest
  ID:                             b8dcc56743394fd19c9f3db7b990e5e3
  State:                          normal
  Created:                        2019-04-25T16:03:34+0000
  Location:                       wdc04
  Master URL:                     https://c1.private.us-east.containers.cloud.ibm.com:25073
  Public Service Endpoint URL:    -
  Private Service Endpoint URL:   https://c1.private.us-east.containers.cloud.ibm.com:25073
  Master Location:                Washington D.C.
  ...
  ```
  {: screen}

3. Create a YAML file that is named `kube-api-via-nlb.yaml`. This YAML creates a private `LoadBalancer` service and exposes the private service endpoint through that NLB. Replace `<private_service_endpoint_port>` with the port you found in the previous step.
  ```
  apiVersion: v1
  kind: Service
  metadata:
    name: kube-api-via-nlb
    annotations:
      service.kubernetes.io/ibm-load-balancer-cloud-provider-ip-type: private
    namespace: default
  spec:
    type: LoadBalancer
    ports:
    - protocol: TCP
      port: <private_service_endpoint_port>
      targetPort: <private_service_endpoint_port>
  ---
  kind: Endpoints
  apiVersion: v1
  metadata:
    name: kube-api-via-nlb
  subsets:
    - addresses:
        - ip: 172.20.0.1
      ports:
        - port: 2040
  ```
  {: codeblock}

4. To create the private NLB, you must be connected to the cluster master. Because you cannot yet connect through the private service endpoint from a VPN or {{site.data.keyword.cloud_notm}} Direct Link, you must connect to the cluster master and create the NLB by using the public service endpoint or Kubernetes dashboard.
  * If you enabled the private service endpoint only, you can use the Kubernetes dashboard to create the NLB. The dashboard automatically routes all requests to the private service endpoint of the master.
    1.  Log in to the [{{site.data.keyword.cloud_notm}} console](https://cloud.ibm.com/).
    2.  From the menu bar, select the account that you want to use.
    3.  From the menu ![Menu icon](../icons/icon_hamburger.svg "Menu icon"), click **Kubernetes**.
    4.  On the **Clusters** page, click the cluster that you want to access.
    5.  From the cluster detail page, click the **Kubernetes Dashboard**.
    6.  Click **+ Create**.
    7.  Select **Create from file**, upload the `kube-api-via-nlb.yaml` file, and click **Upload**.
    8.  In the **Overview** page, verify that the `kube-api-via-nlb` service is created. In the **External endpoints** column, note the `10.x.x.x` address. This IP address exposes the private service endpoint for the Kubernetes master on the port that you specified in your YAML file.

  * If you also enabled the public service endpoint, you already have access to the master.
    1. Get the command to set the environment variable and download the Kubernetes configuration files.
        ```
        ibmcloud ks cluster-config --cluster <cluster_name_or_ID>
        ```
        {: pre}
        When the download of the configuration files is finished, a command is displayed that you can use to set the path to the local Kubernetes configuration file as an environment variable.

        Example for OS X:
        ```
        export KUBECONFIG=/Users/<user_name>/.bluemix/plugins/kubernetes-service/clusters/mycluster/kube-config-prod-dal10-mycluster.yml
        ```
        {: screen}
    2. Copy and paste the command that is displayed in your terminal to set the `KUBECONFIG` environment variable.
    3. Create the NLB and endpoint.
      ```
      kubectl apply -f kube-api-via-nlb.yaml
      ```
      {: pre}
    4. Verify that the `kube-api-via-nlb` NLB is created. In the output, note the `10.x.x.x` **EXTERNAL-IP** address. This IP address exposes the private service endpoint for the Kubernetes master on the port that you specified in your YAML file.
      ```
      kubectl get svc -o wide
      ```
      {: pre}

      In this example output, the IP address for the private service endpoint of the Kubernetes master is `10.186.92.42`.
      ```
      NAME                     TYPE           CLUSTER-IP       EXTERNAL-IP      PORT(S)          AGE   SELECTOR
      kube-api-via-nlb         LoadBalancer   172.21.150.118   10.186.92.42     443:32235/TCP    10m   <none>
      ...
      ```
      {: screen}

  <p class="note">If you want to connect to the master by using the [strongSwan VPN service](/docs/containers?topic=containers-vpn#vpn-setup), note the `172.21.x.x` **Cluster IP** to use in the next step instead. Because the strongSwan VPN pod runs inside your cluster, it can access the NLB by using the IP address of the internal cluster IP service. In your `config.yaml` file for the strongSwan Helm chart, ensure that the Kubernetes service subnet CIDR, `172.21.0.0/16`, is listed in the `local.subnet` setting.</p>

5. On the client machines where you or your users run `kubectl` commands, add the NLB IP address and the private service endpoint URL to the `/etc/hosts` file. Do not include any ports in the IP address and URL and do not include `https://` in the URL.
  * For OSX and Linux users:
    ```
    sudo nano /etc/hosts
    ```
    {: pre}
  * For Windows users:
    ```
    notepad C:\Windows\System32\drivers\etc\hosts
    ```
    {: pre}

    Depending on your local machine permissions, you might need to run Notepad as an administrator to edit the hosts file.
    {: tip}

  Example text to add:
  ```
  10.186.92.42      c1.private.us-east.containers.cloud.ibm.com
  ```
  {: codeblock}

6. Verify that you are connected to the private network through a VPN or {{site.data.keyword.cloud_notm}} Direct Link connection.

7. Get the command to set the environment variable and download the Kubernetes configuration files.
    ```
    ibmcloud ks cluster-config --cluster <cluster_name_or_ID>
    ```
    {: pre}
    When the download of the configuration files is finished, a command is displayed that you can use to set the path to the local Kubernetes configuration file as an environment variable.

    Example for OS X:
    ```
    export KUBECONFIG=/Users/<user_name>/.bluemix/plugins/kubernetes-service/clusters/mycluster/kube-config-prod-dal10-mycluster.yml
    ```
    {: screen}

8. Optional: If you have both the public and private service endpoints enabled, update your local Kubernetes configuration file to use the private service endpoint. By default, the public service endpoint is downloaded to the configuration file.
  1. Navigate to the `kubeconfig` directory and open the file.
    ```
    cd /Users/<user_name>/.bluemix/plugins/kubernetes-service/clusters/<cluster_name> && nano touch kube-config-prod-dal10-mycluster.yml
    ```
    {: pre}
  2. Edit your Kubernetes configuration file to add the word `private` to the service endpoint URL. For example, in the Kubernetes configuration file `kube-config-prod-dal10-mycluster.yml`, the server field might look like `server: https://c1.us-east.containers.cloud.ibm.com:30426`. You can change this URL to the private service endpoint URL by changing the server field to `server: https://c1.private.us-east.containers.cloud.ibm.com:30426`.
  3. Repeat these steps each time that you run `ibmcloud ks cluster-config`.

9. Copy and paste the command that is displayed in your terminal to set the `KUBECONFIG` environment variable.

10. Verify that the `kubectl` commands run properly with your cluster through the private service endpoint by checking the Kubernetes CLI server version.
  ```
  kubectl version --short
  ```
  {: pre}

  Example output:
  ```
  Client Version: v1.13.8
  Server Version: v1.13.8
  ```
  {: screen}

<br />


## Next steps
{: #next_steps}

When the cluster is up and running, you can check out the following tasks:
- If you created the cluster in a multizone capable zone, [spread worker nodes by adding a zone to your cluster](/docs/containers?topic=containers-add_workers).
- [Deploy an app in your cluster.](/docs/containers?topic=containers-app#app_cli)
- [Set up your own private registry in {{site.data.keyword.cloud_notm}} to store and share Docker images with other users.](/docs/services/Registry?topic=registry-getting-started)
- [Set up the cluster autoscaler](/docs/containers?topic=containers-ca#ca) to automatically add or remove worker nodes from your worker pools based on your workload resource requests.
- Control who can create pods in your cluster with [pod security policies](/docs/containers?topic=containers-psp).
- Enable the [Istio](/docs/containers?topic=containers-istio) and [Knative](/docs/containers?topic=containers-serverless-apps-knative) managed add-ons to extend your cluster capabilities.

Then, you can check out the following network configuration steps for your cluster setup:

### Run internet-facing app workloads in a cluster
{: #next_steps_internet}

* Isolate networking workloads to [edge worker nodes](/docs/containers?topic=containers-edge).
* Expose your apps with [public networking services](/docs/containers?topic=containers-cs_network_planning#public_access).
* Control public traffic to the network services that expose your apps by creating [Calico pre-DNAT policies](/docs/containers?topic=containers-network_policies#block_ingress), such as whitelist and blacklist policies.
* Connect your cluster with services in private networks outside of your {{site.data.keyword.cloud_notm}} account by setting up a [strongSwan IPSec VPN service](/docs/containers?topic=containers-vpn).

### Extend your on-premises data center to a cluster and allow limited public access using edge nodes and Calico network policies
{: #next_steps_calico}

* Connect your cluster with services in private networks outside of your {{site.data.keyword.cloud_notm}} account by setting up [{{site.data.keyword.cloud_notm}} Direct Link](/docs/infrastructure/direct-link?topic=direct-link-get-started-with-ibm-cloud-direct-link) or the [strongSwan IPSec VPN service](/docs/containers?topic=containers-vpn#vpn-setup). {{site.data.keyword.cloud_notm}} Direct Link allows communication between apps and services in your cluster and an on-premises network over the private network, while strongSwan allows communication through an encrypted VPN tunnel over the public network.
* Isolate public networking workloads by creating an [edge worker pool](/docs/containers?topic=containers-edge) of worker nodes that are connected to public and private VLANs.
* Expose your apps with [private networking services](/docs/containers?topic=containers-cs_network_planning#private_access).
* Create Calico host network policies to isolate your cluster on the [public network](/docs/containers?topic=containers-network_policies#isolate_workers_public) and on the [private network](/docs/containers?topic=containers-network_policies#isolate_workers).

### Extend your on-premises data center to a cluster and allow limited public access using a gateway device
{: #next_steps_gateway}

* If you also configure your gateway firewall for the private network, you must [allow communication between worker nodes and let your cluster access infrastructure resources over the private network](/docs/containers?topic=containers-firewall#firewall_private).
* To securely connect your worker nodes and apps to private networks outside of your {{site.data.keyword.cloud_notm}} account, set up an IPSec VPN endpoint on your gateway device. Then, [configure the strongSwan IPSec VPN service](/docs/containers?topic=containers-vpn#vpn-setup) in your cluster to use the VPN endpoint on your gateway or [set up VPN connectivity directly with VRA](/docs/containers?topic=containers-vpn#vyatta).
* Expose your apps with [private networking services](/docs/containers?topic=containers-cs_network_planning#private_access).
* [Open up the required ports and IP addresses](/docs/containers?topic=containers-firewall#firewall_inbound) in your gateway device firewall to permit inbound traffic to networking services.

### Extend your on-premises data center to a cluster on the private network only
{: #next_steps_extend}

* If you have a firewall on the private network, [allow communication between worker nodes and let your cluster access infrastructure resources over the private network](/docs/containers?topic=containers-firewall#firewall_private).
* Connect your cluster with services in private networks outside of your {{site.data.keyword.cloud_notm}} account by setting up [{{site.data.keyword.cloud_notm}} Direct Link](/docs/infrastructure/direct-link?topic=direct-link-get-started-with-ibm-cloud-direct-link).
* Expose your apps on the private network with [private networking services](/docs/containers?topic=containers-cs_network_planning#private_access).
