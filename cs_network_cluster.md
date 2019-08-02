---

copyright:
  years: 2014, 2019
lastupdated: "2019-07-31"

keywords: kubernetes, iks, vlan

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



# Changing service endpoints or VLAN connections
{: #cs_network_cluster}

After you initially set up your network when you [create a cluster](/docs/containers?topic=containers-clusters), you can change the service endpoints that your Kubernetes master is accessible through or change the VLAN connections for your worker nodes.
{: shortdesc}

## Setting up the private service endpoint
{: #set-up-private-se}

In clusters that run Kubernetes version 1.11 or later, enable or disable the private service endpoint for your cluster.
{: shortdesc}

The private service endpoint makes your Kubernetes master privately accessible. Your worker nodes and your authorized cluster users can communicate with the Kubernetes master over the private network. To determine whether you can enable the private service endpoint, see [Worker-to-master and user-to-master communication](/docs/containers?topic=containers-plan_clusters#internet-facing). Note that you cannot disable the private service endpoint after you enable it.

Did you create a cluster with only a private service endpoint before you enabled your account for [VRF](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud) and [service endpoints](/docs/resources?topic=resources-private-network-endpoints#getting-started)? Try [setting up the public service endpoint](#set-up-public-se) so that you can use your cluster until your support cases are processed to update your account.
{: tip}

1. Enable [VRF](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud) in your IBM Cloud infrastructure account. To check whether a VRF is already enabled, use the `ibmcloud account show` command.
2. [Enable your {{site.data.keyword.cloud_notm}} account to use service endpoints](/docs/resources?topic=resources-private-network-endpoints#getting-started).
3. Enable the private service endpoint.
   ```
   ibmcloud ks cluster-feature-enable private-service-endpoint --cluster <cluster_name_or_ID>
   ```
   {: pre}
4. Refresh the Kubernetes master API server to use the private service endpoint. You can follow the prompt in the CLI, or manually run the following command.
   ```
   ibmcloud ks apiserver-refresh --cluster <cluster_name_or_ID>
   ```
   {: pre}

5. [Create a configmap](/docs/containers?topic=containers-update#worker-up-configmap) to control the maximum number of worker nodes that can be unavailable at a time in your cluster. When you update your worker nodes, the configmap helps prevent downtime for your apps as the apps are rescheduled orderly onto available worker nodes.
6. Update all the worker nodes in your cluster to pick up the private service endpoint configuration.

   <p class="important">By issuing the update command, the worker nodes are reloaded to pick up the service endpoint configuration. If no worker update is available, you must [reload the worker nodes manually](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli). If you reload, be sure to cordon, drain, and manage the order to control the maximum number of worker nodes that are unavailable at a time.</p>
   ```
   ibmcloud ks worker-update --cluster <cluster_name_or_ID> --workers <worker1,worker2>
   ```
   {: pre}

8. If the cluster is in an environment behind a firewall:
  * [Allow your authorized cluster users to run `kubectl` commands to access the master through the private service endpoint.](/docs/containers?topic=containers-firewall#firewall_kubectl)
  * [Allow outbound network traffic to the private IPs](/docs/containers?topic=containers-firewall#firewall_outbound) for infrastructure resources and for the {{site.data.keyword.cloud_notm}} services that you plan to use.

9.  Optional: To use the private service endpoint only:
    1.  [Disable the public service endpoint](#disable-public-se).
    2.  [Set up access to the master on the private service endpoint](/docs/containers?topic=containers-clusters#access_on_prem).


<br />


## Setting up the public service endpoint
{: #set-up-public-se}

Enable or disable the public service endpoint for your cluster.
{: shortdesc}

The public service endpoint makes your Kubernetes master publicly accessible. Your worker nodes and your authorized cluster users can securely communicate with the Kubernetes master over the public network. For more information, see [Worker-to-master and user-to-master communication](/docs/containers?topic=containers-plan_clusters#internet-facing).

**Steps to enable**</br>
If you previously disabled the public endpoint, you can re-enable it.
1. Enable the public service endpoint.
   ```
   ibmcloud ks cluster-feature-enable public-service-endpoint --cluster <cluster_name_or_ID>
   ```
   {: pre}
2. Refresh the Kubernetes master API server to use the public service endpoint. You can follow the prompt in the CLI, or manually run the following command.
   ```
   ibmcloud ks apiserver-refresh --cluster <cluster_name_or_ID>
   ```
   {: pre}
3. [Create a configmap](/docs/containers?topic=containers-update#worker-up-configmap) to control the maximum number of worker nodes that can be unavailable at a time in your cluster. When you update your worker nodes, the configmap helps prevent downtime for your apps as the apps are rescheduled orderly onto available worker nodes.
4. Update all the worker nodes in your cluster to remove the public service endpoint configuration.<p class="important">By issuing the update command, the worker nodes are reloaded to pick up the service endpoint configuration. If no worker update is available, you must reload the worker nodes manually with the `ibmcloud ks worker-reload` [command](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload). If you reload, be sure to cordon, drain, and manage the order to control the maximum number of worker nodes that are unavailable at a time.</p>
   ```
   ibmcloud ks worker-update --cluster <cluster_name_or_ID> --workers <worker1,worker2>
   ```
  {: pre}
   </br>

{: #disable-public-se}
**Steps to disable**</br>
To disable the public service endpoint, you must first enable the private service endpoint so that your worker nodes can communicate with the Kubernetes master.
1. [Enable the private service endpoint](#set-up-private-se).
2. Disable the public service endpoint.
   ```
   ibmcloud ks cluster-feature-disable public-service-endpoint --cluster <cluster_name_or_ID>
   ```
   {: pre}
3. Refresh the Kubernetes master API server to remove the public service endpoint by following the CLI prompt or by manually running the following command.
   ```
   ibmcloud ks apiserver-refresh --cluster <cluster_name_or_ID>
   ```
   {: pre}
4. [Create a configmap](/docs/containers?topic=containers-update#worker-up-configmap) to control the maximum number of worker nodes that can be unavailable at a time in your cluster. When you update your worker nodes, the configmap helps prevent downtime for your apps as the apps are rescheduled orderly onto available worker nodes.
5. Update all the worker nodes in your cluster to remove the public service endpoint configuration.<p class="important">By issuing the update command, the worker nodes are reloaded to pick up the service endpoint configuration. If no worker update is available, you must reload the worker nodes manually with the `ibmcloud ks worker-reload` [command](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload). If you reload, be sure to cordon, drain, and manage the order to control the maximum number of worker nodes that are unavailable at a time.</p>
   ```
   ibmcloud ks worker-update --cluster <cluster_name_or_ID> --workers <worker1,worker2>
   ```
  {: pre}

## Switching from the public service endpoint to the private service endpoint
{: #migrate-to-private-se}

In clusters that run Kubernetes version 1.11 or later, enable worker nodes to communicate with the master over the private network instead of the public network by enabling the private service endpoint.
{: shortdesc}

All clusters that are connected to a public and a private VLAN use the public service endpoint by default. Your worker nodes and your authorized cluster users can securely communicate with the Kubernetes master over the public network. To enable worker nodes to communicate with the Kubernetes master over the private network instead of the public network, you can enable the private service endpoint. Then, you can optionally disable the public service endpoint.
* If you enable the private service endpoint and keep the public service endpoint enabled too, workers always communicate with the master over the private network, but your users can communicate with the master over either the public or private network.
* If you enable the private service endpoint but disable the public service endpoint, workers and users must communicate with the master over the private network.

Note that you cannot disable the private service endpoint after you enable it.

1. Enable [VRF](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud) in your IBM Cloud infrastructure account. To check whether a VRF is already enabled, use the `ibmcloud account show` command.
2. [Enable your {{site.data.keyword.cloud_notm}} account to use service endpoints](/docs/resources?topic=resources-private-network-endpoints#getting-started).
3. Enable the private service endpoint.
   ```
   ibmcloud ks cluster-feature-enable private-service-endpoint --cluster <cluster_name_or_ID>
   ```
   {: pre}
4. Refresh the Kubernetes master API server to use the private service endpoint by following the CLI prompt or by manually running the following command.
   ```
   ibmcloud ks apiserver-refresh --cluster <cluster_name_or_ID>
   ```
   {: pre}
5. [Create a configmap](/docs/containers?topic=containers-update#worker-up-configmap) to control the maximum number of worker nodes that can be unavailable at a time in your cluster. When you update your worker nodes, the configmap helps prevent downtime for your apps as the apps are rescheduled orderly onto available worker nodes.

6.  Update all the worker nodes in your cluster to pick up the private service endpoint configuration.

    <p class="important">By issuing the update command, the worker nodes are reloaded to pick up the service endpoint configuration. If no worker update is available, you must [reload the worker nodes manually](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli). If you reload, be sure to cordon, drain, and manage the order to control the maximum number of worker nodes that are unavailable at a time.</p>
    ```
    ibmcloud ks worker-update --cluster <cluster_name_or_ID> --workers <worker1,worker2>
    ```
    {: pre}

7.  Optional: To use the private service endpoint only:
    1.  Disable the public service endpoint.
        ```
        ibmcloud ks cluster-feature-disable public-service-endpoint --cluster <cluster_name_or_ID>
        ```
        {: pre}
    2.  [Set up access to the master on the private service endpoint](/docs/containers?topic=containers-clusters#access_on_prem).

<br />


## Changing your worker node VLAN connections
{: #change-vlans}

When you create a cluster, you choose whether to connect your worker nodes to a private and a public VLAN or to a private VLAN only. Your worker nodes are part of worker pools, which store networking metadata that includes the VLANs to use to provision future worker nodes in the pool. You might want to change your cluster's VLAN connectivity setup later, in cases such as the following.
{: shortdesc}

* The worker pool VLANs in a zone run out of capacity, and you need to provision a new VLAN for your cluster worker nodes to use.
* You have a cluster with worker nodes that are on both public and private VLANs, but you want to change to a [private-only cluster](/docs/containers?topic=containers-plan_clusters#private_clusters).
* You have a private-only cluster, but you want some worker nodes such as a worker pool of [edge nodes](/docs/containers?topic=containers-edge#edge) on the public VLAN to expose your apps on the internet.

Trying to change the service endpoint for master-worker communication instead? Check out the topics to set up [public](#set-up-public-se) and [private](#set-up-private-se) service endpoints.
{: tip}

Before you begin:
* [Log in to your account. If applicable, target the appropriate resource group. Set the context for your cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
* If your worker nodes are stand-alone (not part of a worker pool), [update them to worker pools](/docs/containers?topic=containers-update#standalone_to_workerpool).

To change the VLANs that a worker pool uses to provision worker nodes:

1. List the names of the worker pools in your cluster.
  ```
  ibmcloud ks worker-pools --cluster <cluster_name_or_ID>
  ```
  {: pre}

2. Determine the zones for one of the worker pools. In the output, look for the **Zones** field.
  ```
  ibmcloud ks worker-pool-get --cluster <cluster_name_or_ID> --worker-pool <pool_name>
  ```
  {: pre}

3. For each zone that you found in the previous step, get an available public and private VLAN that are compatible with each other.

  1. Check the available public and private VLANs that are listed under **Type** in the output.
     ```
     ibmcloud ks vlans --zone <zone>
     ```
     {: pre}

  2. Check that the public and private VLANs in the zone are compatible. To be compatible, the **Router** must have the same pod ID. In this example output, the **Router** pod IDs match: `01a` and `01a`. If one pod ID was `01a` and the other was `02a`, you cannot set these public and private VLAN IDs for your worker pool.
     ```
     ID        Name   Number   Type      Router         Supports Virtual Workers
     229xxxx          1234     private   bcr01a.dal12   true
     229xxxx          5678     public    fcr01a.dal12   true
     ```
     {: screen}

  3. If you need to order a new public or private VLAN for the zone, you can order in the [{{site.data.keyword.cloud_notm}} console](/docs/infrastructure/vlans?topic=vlans-ordering-premium-vlans#ordering-premium-vlans), or use the following command. Remember that the VLANs must be compatible, with matching **Router** pod IDs as in the previous step. If you are creating a pair of new public and private VLANs, they must be compatible with each other.
     ```
     ibmcloud sl vlan create -t [public|private] -d <zone> -r <compatible_router>
     ```
     {: pre}

  4. Note the IDs of the compatible VLANs.

4. Set up a worker pool with the new VLAN network metadata for each zone. You can create a new worker pool, or modify an existing worker pool.

  * **Create a new worker pool**: See [adding worker nodes by creating a new worker pool](/docs/containers?topic=containers-add_workers#add_pool).

  * **Modify an existing worker pool**: Set the worker pool's network metadata to use the VLAN for each zone. Worker nodes that were already created in the pool continue to use the previous VLANs, but new worker nodes in the pool use new VLAN metadata that you set.

    * Example to add both public and private VLANs, such as if you change from private-only to both private and public:
      ```
      ibmcloud ks zone-network-set --zone <zone> --cluster <cluster_name_or_ID> --worker-pools <pool_name> --private-vlan <private_vlan_id> --public-vlan <public_vlan_id>
      ```
      {: pre}

    * Example to add only a private VLAN, such as if you change from public and private VLANs to private-only when you have a [VRF-enabled account that uses service endpoints](/docs/resources?topic=resources-private-network-endpoints#getting-started):
      ```
      ibmcloud ks zone-network-set --zone <zone> --cluster <cluster_name_or_ID> --worker-pools <pool_name> --private-vlan <private_vlan_id> --public-vlan <public_vlan_id>
      ```
      {: pre}

5. Add worker nodes to the worker pool by resizing the pool.
   ```
   ibmcloud ks worker-pool-resize --cluster <cluster_name_or_ID> --worker-pool <pool_name> --size-per-zone <number_of_workers_per_zone>
   ```
   {: pre}

   If you want to remove worker nodes that use the previous network metadata, change the number of workers per zone to double the previous number of workers per zone. Later in these steps, you can cordon, drain, and remove the previous worker nodes.
  {: tip}

6. Verify that new worker nodes are created with the appropriate **Public IP** and **Private IP** in the output. For example, if you change the worker pool from a public and private VLAN to private-only, the new worker nodes have only a private IP. If you change the worker pool from private-only to both public and private VLANs, the new worker nodes have both public and private IPs.
   ```
   ibmcloud ks workers --cluster <cluster_name_or_ID> --worker-pool <pool_name>
   ```
   {: pre}

7. Optional: Remove the worker nodes with the previous network metadata from the worker pool.
  1. In the output of the previous step, note the **ID** and **Private IP** of the worker nodes that you want to remove from the worker pool.
  2. Mark the worker node as unschedulable in a process that is known as cordoning. When you cordon a worker node, you make it unavailable for future pod scheduling.
     ```
     kubectl cordon <worker_private_ip>
     ```
     {: pre}
  3. Verify that pod scheduling is disabled for your worker node.
     ```
     kubectl get nodes
     ```
     {: pre}
     Your worker node is disabled for pod scheduling if the status displays **`SchedulingDisabled`**.
  4. Force pods to be removed from your worker node and rescheduled onto remaining worker nodes in the cluster.
     ```
     kubectl drain <worker_private_ip>
     ```
     {: pre}
     This process can take a few minutes.
  5. Remove the worker node. Use the worker ID that you previously retrieved.
     ```
     ibmcloud ks worker-rm --cluster <cluster_name_or_ID> --worker <worker_name_or_ID>
     ```
     {: pre}
  6. Verify that the worker node is removed.
     ```
     ibmcloud ks workers --cluster <cluster_name_or_ID> --worker-pool <pool_name>
     ```
     {: pre}

8. Optional: You can repeat steps 2 - 7 for each worker pool in your cluster. After you complete these steps, all worker nodes in your cluster are set up with the new VLANs.

10. Optional: If you no longer need the subnets on the old VLANs, you can [remove them](/docs/containers?topic=containers-subnets#remove-subnets).
