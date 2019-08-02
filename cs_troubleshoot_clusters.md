---

copyright:
  years: 2014, 2019
lastupdated: "2019-07-31"

keywords: kubernetes, iks, ImagePullBackOff, registry, image, failed to pull image,

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
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}



# Troubleshooting clusters and worker nodes
{: #cs_troubleshoot_clusters}

As you use {{site.data.keyword.containerlong}}, consider these techniques for troubleshooting your clusters and worker nodes.
{: shortdesc}

<p class="tip">If you have a more general issue, try out [cluster debugging](/docs/containers?topic=containers-cs_troubleshoot).<br>Also, while you troubleshoot, you can use the [{{site.data.keyword.containerlong_notm}} Diagnostics and Debug Tool](/docs/containers?topic=containers-cs_troubleshoot#debug_utility) to run tests and gather pertinent information from your cluster.</p>

## Unable to create a cluster or manage worker nodes due to permission errors
{: #cs_credentials}

{: tsSymptoms}
You try to manage worker nodes for a new or an existing cluster by running one of the following commands.
* Provision workers: `ibmcloud ks cluster-create`, `ibmcloud ks worker-pool-rebalance`, or `ibmcloud ks worker-pool-resize`
* Reload workers: `ibmcloud ks worker-reload` or `ibmcloud ks worker-update`
* Reboot workers: `ibmcloud ks worker-reboot`
* Delete workers: `ibmcloud ks cluster-rm`, `ibmcloud ks worker-rm`, `ibmcloud ks worker-pool-rebalance`, or `ibmcloud ks worker-pool-resize`

However, you receive an error message similar to one of the following.

```
We were unable to connect to your IBM Cloud infrastructure account.
Creating a standard cluster requires that you have either a
Pay-As-You-Go account that is linked to an IBM Cloud infrastructure
account term or that you have used the {{site.data.keyword.containerlong_notm}}
CLI to set your {{site.data.keyword.cloud_notm}} Infrastructure API keys.
```
{: screen}

```
{{site.data.keyword.cloud_notm}} Infrastructure Exception:
'Item' must be ordered with permission.
```
{: screen}

```
Worker not found. Review {{site.data.keyword.cloud_notm}} infrastructure permissions.
```
{: screen}

```
{{site.data.keyword.cloud_notm}} Infrastructure Exception:
The user does not have the necessary {{site.data.keyword.cloud_notm}}
Infrastructure permissions to add servers
```
{: screen}

```
IAM token exchange request failed: Cannot create IMS portal token, as no IMS account is linked to the selected BSS account
```
{: screen}

```
The cluster could not be configured with the registry. Make sure that you have the Administrator role for {{site.data.keyword.registrylong_notm}}.
```
{: screen}

{: tsCauses}
The infrastructure credentials that are set for the region and resource group are missing the appropriate [infrastructure permissions](/docs/containers?topic=containers-access_reference#infra). The user's infrastructure permissions are most commonly stored as an [API key](/docs/containers?topic=containers-users#api_key) for the region and resource group. More rarely, if you use a [different {{site.data.keyword.cloud_notm}} account type](/docs/containers?topic=containers-users#understand_infra), you might have [set infrastructure credentials manually](/docs/containers?topic=containers-users#credentials). If you use a different IBM Cloud infrastructure account to provision infrastructure resources, you might also have [orphaned clusters](#orphaned) in your account.

{: tsResolve}
The account owner must set up the infrastructure account credentials properly. The credentials depend on what type of infrastructure account you are using.

Before you begin, [Log in to your account. If applicable, target the appropriate resource group. Set the context for your cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure).

1.  Identify what user credentials are used for the region and resource group's infrastructure permissions.
    1.  Check the API key for a region and resource group of the cluster.
        ```
        ibmcloud ks api-key-info --cluster <cluster_name_or_ID>
        ```
        {: pre}

        Example output:
        ```
        Getting information about the API key owner for cluster <cluster_name>...
        OK
        Name                Email   
        <user_name>         <name@email.com>
        ```
        {: screen}
    2.  Check if the infrastructure account for the region and resource group is manually set to use a different IBM Cloud infrastructure account.
        ```
        ibmcloud ks credential-get --region <us-south>
        ```
        {: pre}

        **Example output if credentials are set to use a different account**. In this case, the user's infrastructure credentials are used for the region and resource group that you targeted, even if a different user's credentials are stored in the API key that you retrieved in the previous step.
        ```
        OK
        Infrastructure credentials for user name <1234567_name@email.com> set for resource group <resource_group_name>.
        ```
        {: screen}

        **Example output if credentials are not set to use a different account**. In this case, the API key owner that you retrieved in the previous step has the infrastructure credentials that are used for the region and resource group.
        ```
        FAILED
        No credentials set for resource group <resource_group_name>.: The user credentials could not be found. (E0051)
        ```
        {: screen}
2.  Validate the infrastructure permissions that the user has.
    1.  List the suggested and required infrastructure permissions for the region and resource group.
        ```
        ibmcloud ks infra-permissions-get --region <region>
        ```
        {: pre}

        For console and CLI commands to assign these permissions, see [Classic infrastructure roles](/docs/containers?topic=containers-access_reference#infra).
        {: tip}

    2.  Make sure that the [infrastructure credentials owner for the API key or the manually-set account has the correct permissions](/docs/containers?topic=containers-users#owner_permissions).
    3.  If necessary, you can change the [API key](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_api_key_reset) or [manually-set](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_credentials_set) infrastructure credentials owner for the region and resource group.
3.  Test that the changed permissions permit authorized users to perform infrastructure operations for the cluster.
    1.  For example, you might try to a delete a worker node.
        ```
        ibmcloud ks worker-rm --cluster <cluster_name_or_ID> --worker <worker_node_ID>
        ```
        {: pre}
    2.  Check to see if the worker node is removed.
        ```
        ibmcloud ks worker-get --cluster <cluster_name_or_ID> --worker <worker_node_ID>
        ```
        {: pre}

        Example output if the worker node removal is successful. The `worker-get` operation fails because the worker node is deleted. The infrastructure permissions are correctly set up.
        ```
        FAILED
        The specified worker node could not be found. (E0011)
        ```
        {: screen}

    3.  If the worker node is not removed, review that [**State** and **Status** fields](/docs/containers?topic=containers-cs_troubleshoot#debug_worker_nodes) and the [common issues with worker nodes](/docs/containers?topic=containers-cs_troubleshoot#common_worker_nodes_issues) to continue debugging.
    4.  If you manually set credentials and still cannot see the cluster's worker nodes in your infrastructure account, you might check whether the [cluster is orphaned](#orphaned).

<br />


## Unable to create a cluster or manage worker nodes due to paid account error
{: #cs_totp}

{: tsSymptoms}
You try to manage worker nodes for a new or an existing cluster by running one of the following commands.
* Provision clusters and workers: `ibmcloud ks cluster-create`, `ibmcloud ks worker-pool-rebalance`, or `ibmcloud ks worker-pool-resize`
* Reload workers: `ibmcloud ks worker-reload` or `ibmcloud ks worker-update`
* Reboot workers: `ibmcloud ks worker-reboot`
* Delete clusters and workers: `ibmcloud ks cluster-rm`, `ibmcloud ks worker-rm`, `ibmcloud ks worker-pool-rebalance`, or `ibmcloud ks worker-pool-resize`

However, you receive an error message similar to the following.
```
Unable to connect to the IBM Cloud account. Ensure that you have a paid account.
```
{: screen}

{: tsCauses}
Your {{site.data.keyword.cloud_notm}} account uses its own automatically linked infrastructure through a Pay-as-you-Go account. However, the account administrator enabled the time-based one-time passcode (TOTP) option so that users are prompted for a time-based one-time passcode (TOTP) at login. This type of [multifactor authentication (MFA)](/docs/iam?topic=iam-types#account-based) is account-based, and affects all access to the account. TOTP MFA also affects the access that {{site.data.keyword.containerlong_notm}} requires to make calls to {{site.data.keyword.cloud_notm}} infrastructure. If TOTP is enabled for the account, you cannot create and manage clusters and worker nodes in {{site.data.keyword.containerlong_notm}}.

{: tsResolve}
The {{site.data.keyword.cloud_notm}} account owner or an account administrator must either:
* Disable TOTP for the account, and continue to use the automatically linked infrastructure credentials for {{site.data.keyword.containerlong_notm}}.
* Continue to use TOTP, but create an infrastructure API key that {{site.data.keyword.containerlong_notm}} can use to make direct calls to the {{site.data.keyword.cloud_notm}} infrastructure API.


**To disable TOTP MFA for the account:**
1. Log in to the [{{site.data.keyword.cloud_notm}} console ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/). From the menu bar, select **Manage > Access (IAM)**.
2. In the left navigation, click the **Settings** page.
3. Under **Multifactor authentication**, click **Edit**.
4. Select **None**, and click **Update**.

**To use TOTP MFA and create an infrastructure API key for {{site.data.keyword.containerlong_notm}}:**
1. From the [{{site.data.keyword.cloud_notm}} ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/) console, select **Manage** > **Access (IAM)** > **Users** and click the name of the account owner. **Note**: If you do not use the account owner's credentials, first [ensure that the user whose credentials you use has the correct permissions](/docs/containers?topic=containers-users#owner_permissions).
2. In the **API Keys** section, find or create a classic infrastructure API key.   
3. Use the infrastructure API key to set the infrastructure API credentials for {{site.data.keyword.containerlong_notm}}. Repeat this command for each region where you create clusters.
    ```
    ibmcloud ks credential-set --infrastructure-username <infrastructure_API_username> --infrastructure-api-key <infrastructure_API_authentication_key> --region <region>
    ```
    {: pre}
4. Verify that the correct credentials are set.
    ```
    ibmcloud ks credential-get --region <region>
    ```
    Example output:
    ```
    Infrastructure credentials for user name user@email.com set for resource group default.
    ```
    {: screen}
5. To ensure that existing clusters use the updated infrastructure API credentials, run `ibmcloud ks api-key-reset --region <region>` in each region where you have clusters.

<br />


## Firewall prevents running CLI commands
{: #ts_firewall_clis}

{: tsSymptoms}
When you run `ibmcloud`, `kubectl`, or `calicoctl` commands from the CLI, they fail.

{: tsCauses}
You might have corporate network policies that prevent access from your local system to public endpoints via proxies or firewalls.

{: tsResolve}
[Allow TCP access for the CLI commands to work](/docs/containers?topic=containers-firewall#firewall_bx). This task requires the [**Administrator** {{site.data.keyword.cloud_notm}} IAM platform role](/docs/containers?topic=containers-users#platform) for the cluster.


## Cannot access resources in my cluster
{: #cs_firewall}

{: tsSymptoms}
When the worker nodes in your cluster cannot communicate on the private network, you might see various different symptoms.

- Sample error message when you run `kubectl exec`, `attach`, `logs`, `proxy`, or `port-forward`:
  ```
  Error from server: error dialing backend: dial tcp XXX.XXX.XXX:10250: getsockopt: connection timed out
  ```
  {: screen}

- Sample error message when `kubectl proxy` succeeds, but the Kubernetes dashboard is not available:
  ```
  timeout on 172.xxx.xxx.xxx
  ```
  {: screen}

- Sample error message when `kubectl proxy` fails or the connection to your service fails:
  ```
  Connection refused
  ```
  {: screen}

  ```
  Connection timed out
  ```
  {: screen}

  ```
  Unable to connect to the server: net/http: TLS handshake timeout
  ```
  {: screen}


{: tsCauses}
To access resources in the cluster, your worker nodes must be able to communicate on the private network. You might have a Vyatta or another firewall set up, or customized your existing firewall settings in your IBM Cloud infrastructure account. {{site.data.keyword.containerlong_notm}} requires certain IP addresses and ports to be opened to allow communication from the worker node to the Kubernetes master and vice versa. If your worker nodes are spread across multiple zones, you must allow private network communication by enabling VLAN spanning. Communication between worker nodes might also not be possible if your worker nodes are stuck in a reloading loop.

{: tsResolve}
1. List the worker nodes in your cluster and verify that your worker nodes are not stuck in a `Reloading` state.
   ```
   ibmcloud ks workers --cluster <cluster_name_or_id>
   ```
   {: pre}

2. If you have a multizone cluster and your account is not enabled for VRF, verify that you [enabled VLAN spanning](/docs/containers?topic=containers-subnets#subnet-routing) for your account.
3. If you have a Vyatta or custom firewall settings, make sure that you [opened up the required ports](/docs/containers?topic=containers-firewall#firewall_outbound) to allow the cluster to access infrastructure resources and services.

<br />



## Unable to view or work with a cluster
{: #cs_cluster_access}

{: tsSymptoms}
* You are not able to find a cluster. When you run `ibmcloud ks clusters`, the cluster is not listed in the output.
* You are not able to work with a cluster. When you run `ibmcloud ks cluster-config` or other cluster-specific commands, the cluster is not found.


{: tsCauses}
In {{site.data.keyword.cloud_notm}}, each resource must be in a resource group. For example, cluster `mycluster` might exist in the `default` resource group. When the account owner gives you access to resources by assigning you an {{site.data.keyword.cloud_notm}} IAM platform role, the access can be to a specific resource or to the resource group. When you are given access to a specific resource, you don't have access to the resource group. In this case, you don't need to target a resource group to work with the clusters you have access to. If you target a different resource group than the group that the cluster is in, actions against that cluster can fail. Conversely, when you are given access to a resource as part of your access to a resource group, you must target a resource group to work with a cluster in that group. If you don't target your CLI session to the resource group that the cluster is in, actions against that cluster can fail.

If you cannot find or work with a cluster, you might be experiencing one of the following issues:
* You have access to the cluster and the resource group that the cluster is in, but your CLI session is not targeted to the resource group that the cluster is in.
* You have access to the cluster, but not as part of the resource group that the cluster is in. Your CLI session is targeted to this or another resource group.
* You don't have access to the cluster.

{: tsResolve}
To check your user access permissions:

1. List all of your user permissions.
    ```
    ibmcloud iam user-policies <your_user_name>
    ```
    {: pre}

2. Check if you have access to the cluster and to the resource group that the cluster is in.
    1. Look for a policy that has a **Resource Group Name** value of the cluster's resource group and a **Memo** value of `Policy applies to the resource group`. If you have this policy, you have access to the resource group. For example, this policy indicates that a user has access to the `test-rg` resource group:
        ```
        Policy ID:   3ec2c069-fc64-4916-af9e-e6f318e2a16c
        Roles:       Viewer
        Resources:
                     Resource Group ID     50c9b81c983e438b8e42b2e8eca04065
                     Resource Group Name   test-rg
                     Memo                  Policy applies to the resource group
        ```
        {: screen}
    2. Look for a policy that has a **Resource Group Name** value of the cluster's resource group, a **Service Name** value of `containers-kubernetes` or no value, and a **Memo** value of `Policy applies to the resource(s) within the resource group`. If you this policy, you have access to clusters or to all resources within the resource group. For example, this policy indicates that a user has access to clusters in the `test-rg` resource group:
        ```
        Policy ID:   e0ad889d-56ba-416c-89ae-a03f3cd8eeea
        Roles:       Administrator
        Resources:
                     Resource Group ID     a8a12accd63b437bbd6d58fb6a462ca7
                     Resource Group Name   test-rg
                     Service Name          containers-kubernetes
                     Service Instance
                     Region
                     Resource Type
                     Resource
                     Memo                  Policy applies to the resource(s) within the resource group
        ```
        {: screen}
    3. If you have both of these policies, skip to Step 4, first bullet. If you don't have the policy from Step 2a, but you do have the policy from Step 2b, skip to Step 4, second bullet. If you do not have either of these policies, continue to Step 3.

3. Check if you have access to the cluster, but not as part of access to the resource group that the cluster is in.
    1. Look for a policy that has no values besides the **Policy ID** and **Roles** fields. If you have this policy, you have access to the cluster as part of access to the entire account. For example, this policy indicates that a user has access to all resources in the account:
        ```
        Policy ID:   8898bdfd-d520-49a7-85f8-c0d382c4934e
        Roles:       Administrator, Manager
        Resources:
                     Service Name
                     Service Instance
                     Region
                     Resource Type
                     Resource
        ```
        {: screen}
    2. Look for a policy that has a **Service Name** value of `containers-kubernetes` and a **Service Instance** value of the cluster's ID. You can find a cluster ID by running `ibmcloud ks cluster-get --cluster <cluster_name>`. For example, this policy indicates that a user has access to a specific cluster:
        ```
        Policy ID:   140555ce-93ac-4fb2-b15d-6ad726795d90
        Roles:       Administrator
        Resources:
                     Service Name       containers-kubernetes
                     Service Instance   df253b6025d64944ab99ed63bb4567b6
                     Region
                     Resource Type
                     Resource
        ```
        {: screen}
    3. If you have either of these policies, skip to the second bullet point of step 4. If you do not have either of these policies, skip to the third bullet point of step 4.

4. Depending on your access policies, choose one of the following options.
    * If you have access to the cluster and to the resource group that the cluster is in:
      1. Target the resource group. **Note**: You can't work with clusters in other resource groups until you untarget this resource group.
          ```
          ibmcloud target -g <resource_group>
          ```
          {: pre}

      2. Target the cluster.
          ```
          ibmcloud ks cluster-config --cluster <cluster_name_or_ID>
          ```
          {: pre}

    * If you have access to the cluster but not to the resource group that the cluster is in:
      1. Do not target a resource group. If you already targeted a resource group, untarget it:
        ```
        ibmcloud target --unset-resource-group
        ```
        {: pre}

      2. Target the cluster.
        ```
        ibmcloud ks cluster-config --cluster <cluster_name_or_ID>
        ```
        {: pre}

    * If you do not have access to the cluster:
        1. Ask your account owner to assign an [{{site.data.keyword.cloud_notm}} IAM platform role](/docs/containers?topic=containers-users#platform) to you for that cluster.
        2. Do not target a resource group. If you already targeted a resource group, untarget it:
          ```
          ibmcloud target --unset-resource-group
          ```
          {: pre}
        3. Target the cluster.
          ```
          ibmcloud ks cluster-config --cluster <cluster_name_or_ID>
          ```
          {: pre}

<br />


## Accessing your worker node with SSH fails
{: #cs_ssh_worker}

{: tsSymptoms}
You cannot access your worker node by using an SSH connection.

{: tsCauses}
SSH by password is unavailable on the worker nodes.

{: tsResolve}
Use a Kubernetes [`DaemonSet` ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) for actions that you must run on every node, or use jobs for one-time actions that you must run.

<br />


## Bare metal instance ID is inconsistent with worker records
{: #bm_machine_id}

{: tsSymptoms}
When you use `ibmcloud ks worker` commands with your bare metal worker node, you see a message similar to the following.

```
Instance ID inconsistent with worker records
```
{: screen}

{: tsCauses}
The machine ID can become inconsistent with the {{site.data.keyword.containerlong_notm}} worker record when the machine experiences hardware issues. When IBM Cloud infrastructure resolves this issue, a component can change within the system that the service does not identify.

{: tsResolve}
For {{site.data.keyword.containerlong_notm}} to re-identify the machine, [reload the bare metal worker node](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload). **Note**: Reloading also updates the machine's [patch version](/docs/containers?topic=containers-changelog).

You can also [delete the bare metal worker node](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_rm). **Note**: Bare metal instances are billed monthly.

<br />


## Unable to modify or delete infrastructure in an orphaned cluster
{: #orphaned}

{: tsSymptoms}
You cannot perform infrastructure-related commands on your cluster, such as:
* Adding or removing worker nodes
* Reloading or rebooting worker nodes
* Resizing worker pools
* Updating your cluster

You cannot view the cluster worker nodes in your IBM Cloud infrastructure account. However, you can update and manage other clusters in the account.

Further, you verified that you have the [proper infrastructure credentials](#cs_credentials).

{: tsCauses}
The cluster might be provisioned in an IBM Cloud infrastructure account that is no longer linked to your {{site.data.keyword.containerlong_notm}} account. The cluster is orphaned. Because the resources are in a different account, you do not have the infrastructure credentials to modify the resources.

Consider the following scenario to understand how clusters might become orphaned.
1.  You have an {{site.data.keyword.cloud_notm}} Pay-As-You-Go account.
2.  You create a cluster named `Cluster1`. The worker nodes and other infrastructure resources are provisioned into the infrastructure account that comes with your Pay-As-You-Go account.
3.  Later, you find out that your team uses a legacy or shared IBM Cloud infrastructure account. You use the `ibmcloud ks credential-set` command to change the IBM Cloud infrastructure credentials to use your team account.
4.  You create another cluster named `Cluster2`. The worker nodes and other infrastructure resources are provisioned into the team infrastructure account.
5.  You notice that `Cluster1` needs a worker node update, a worker node reload, or you just want to clean it up by deleting it. However, because `Cluster1` was provisioned into a different infrastructure account, you cannot modify its infrastructure resources. `Cluster1` is orphaned.
6.  You follow the resolution steps in the following section, but do not set your infrastructure credentials back to your team account. You can delete `Cluster1`, but now `Cluster2` is orphaned.
7.  You change your infrastructure credentials back to the team account that created `Cluster2`. Now, you no longer have an orphaned cluster!

<br>

{: tsResolve}
1.  Check which infrastructure account the region that your cluster is in currently uses to provision clusters.
    1.  Log in to the [{{site.data.keyword.containerlong_notm}} clusters console ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/kubernetes/clusters).
    2.  From the table, select your cluster.
    3.  In the **Overview** tab, check for an **Infrastructure User** field. This field helps you determine if your {{site.data.keyword.containerlong_notm}} account uses a different infrastructure account than the default.
        * If you do not see the **Infrastructure User** field, you have a linked Pay-As-You-Go account that uses the same credentials for your infrastructure and platform accounts. The cluster that cannot be modified might be provisioned in a different infrastructure account.
        * If you see an **Infrastructure User** field, you use a different infrastructure account than the one that came with your Pay-As-You-Go account. These different credentials apply to all clusters within the region. The cluster that cannot be modified might be provisioned in your Pay-As-You-Go or a different infrastructure account.
2.  Check which infrastructure account was used to provision the cluster.
    1.  In the **Worker Nodes** tab, select a worker node and note its **ID**.
    2.  Open the menu ![Menu icon](../icons/icon_hamburger.svg "Menu icon") and click **Classic Infrastructure**.
    3.  From the infrastructure navigation pane, click **Devices > Device List**.
    4.  Search for the worker node ID that you previously noted.
    5.  If you do not find the worker node ID, the worker node is not provisioned into this infrastructure account. Switch to a different infrastructure account and try again.
3.  Use the `ibmcloud ks credential-set` [command](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_credentials_set) to change your infrastructure credentials to the account that the cluster worker nodes are provisioned in, which you found in the previous step.
    If you no longer have access to and cannot get the infrastructure credentials, you must open an {{site.data.keyword.cloud_notm}} support case to remove the orphaned cluster.
    {: note}
4.  [Delete the cluster](/docs/containers?topic=containers-remove).
5.  If you want, reset the infrastructure credentials to the previous account. Note that if you created clusters with a different infrastructure account than the account that you switch to, you might orphan those clusters.
    * To set credentials to a different infrastructure account, use the `ibmcloud ks credential-set` [command](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_credentials_set).
    * To use the default credentials that come with your {{site.data.keyword.cloud_notm}} Pay-As-You-Go account, use the `ibmcloud ks credential-unset --region <region>` [command](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_credentials_unset).

<br />


## `kubectl` commands do not work
{: #kubectl_fails}

{: tsSymptoms}
When you run `kubectl` commands against your cluster, your commands fail with an error message similar to the following.

```
No resources found.
Error from server (NotAcceptable): unknown (get nodes)
```
{: screen}

```
invalid object doesn't have additional properties
```
{: screen}

```
error: No Auth Provider found for name "oidc"
```
{: screen}

{: tsCauses}
You have a different version of `kubectl` than your cluster version. [Kubernetes does not support ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/setup/version-skew-policy/) `kubectl` client versions that are 2 or more versions apart from the server version (n +/- 2). You might also have the OpenShift version of `kubectl`, which does not work with community Kubernetes clusters.

To check your client `kubectl` version against the cluster server version, run `kubectl version --short`.

{: tsResolve}
[Install the version of `kubectl`](/docs/containers?topic=containers-cs_cli_install#kubectl) that matches the Kubernetes version of your cluster.

If you have multiple clusters at different Kubernetes versions or different container platforms such as OpenShift, download each `kubectl` version binary file to a separate directory. Then, you can set up an alias in your local terminal profile to point to the `kubectl` binary directory that matches the `kubectl` version of the cluster that you want to work with, or you might be able to use a tool such as `brew switch kubernetes-cli <major.minor>`.

<br />


## `kubectl` commands time out
{: #exec_logs_fail}

{: tsSymptoms}
If you run commands such as `kubectl exec`, `kubectl attach`, `kubectl proxy`, `kubectl port-forward`, or `kubectl logs`, you see the following message.

  ```
  <workerIP>:10250: getsockopt: connection timed out
  ```
  {: screen}

{: tsCauses}
The OpenVPN connection between the master node and worker nodes is not functioning properly.

{: tsResolve}
1. If you have multiple VLANs for a cluster, multiple subnets on the same VLAN, or a multizone cluster, you must enable a [Virtual Router Function (VRF)](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud) for your IBM Cloud infrastructure account so your worker nodes can communicate with each other on the private network. To enable VRF, [contact your IBM Cloud infrastructure account representative](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#how-you-can-initiate-the-conversion). To check whether a VRF is already enabled, use the `ibmcloud account show` command. If you cannot or do not want to enable VRF, enable [VLAN spanning](/docs/infrastructure/vlans?topic=vlans-vlan-spanning#vlan-spanning). To perform this action, you need the **Network > Manage Network VLAN Spanning** [infrastructure permission](/docs/containers?topic=containers-users#infra_access), or you can request the account owner to enable it. To check whether VLAN spanning is already enabled, use the `ibmcloud ks vlan-spanning-get --region <region>` [command](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlan_spanning_get).
2. Restart the OpenVPN client pod.
  ```
  kubectl delete pod -n kube-system -l app=vpn
  ```
  {: pre}
3. If you still see the same error message, then the worker node that the VPN pod is on might be unhealthy. To restart the VPN pod and reschedule it to a different worker node, [cordon, drain, and reboot the worker node](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reboot) that the VPN pod is on.

<br />


## Binding a service to a cluster results in same name error
{: #cs_duplicate_services}

{: tsSymptoms}
When you run `ibmcloud ks cluster-service-bind --cluster <cluster_name> --namespace <namespace> --service <service_instance_name>`, you see the following message.

```
Multiple services with the same name were found.
Run 'ibmcloud service list' to view available Bluemix service instances...
```
{: screen}

{: tsCauses}
Multiple service instances might have the same name in different regions.

{: tsResolve}
Use the service GUID instead of the service instance name in the `ibmcloud ks cluster-service-bind` command.

1. [Log in to the {{site.data.keyword.cloud_notm}} region that includes the service instance to bind.](/docs/containers?topic=containers-regions-and-zones#bluemix_regions)

2. Get the GUID for the service instance.
  ```
  ibmcloud service show <service_instance_name> --guid
  ```
  {: pre}

  Output:
  ```
  Invoking 'cf service <service_instance_name> --guid'...
  <service_instance_GUID>
  ```
  {: screen}
3. Bind the service to the cluster again.
  ```
  ibmcloud ks cluster-service-bind --cluster <cluster_name> --namespace <namespace> --service <service_instance_GUID>
  ```
  {: pre}

<br />


## Binding a service to a cluster results in service not found error
{: #cs_not_found_services}

{: tsSymptoms}
When you run `ibmcloud ks cluster-service-bind --cluster <cluster_name> --namespace <namespace> --service <service_instance_name>`, you see the following message.

```
Binding service to a namespace...
FAILED

The specified IBM Cloud service could not be found. If you just created the service, wait a little and then try to bind it again. To view available IBM Cloud service instances, run 'ibmcloud service list'. (E0023)
```
{: screen}

{: tsCauses}
To bind services to a cluster, you must have the Cloud Foundry developer user role for the space where the service instance is provisioned. In addition, you must have the {{site.data.keyword.cloud_notm}} IAM Editor platform access to {{site.data.keyword.containerlong}}. To access the service instance, you must be logged in to the space where the service instance is provisioned.

{: tsResolve}

**As the user:**

1. Log in to {{site.data.keyword.cloud_notm}}.
   ```
   ibmcloud login
   ```
   {: pre}

2. Target the org and the space where the service instance is provisioned.
   ```
   ibmcloud target -o <org> -s <space>
   ```
   {: pre}

3. Verify that you are in the right space by listing your service instances.
   ```
   ibmcloud service list
   ```
   {: pre}

4. Try binding the service again. If you get the same error, then contact the account administrator and verify that you have sufficient permissions to bind services (see the following account admin steps).

**As the account admin:**

1. Verify that the user who experiences this problem has [Editor permissions for {{site.data.keyword.containerlong}}](/docs/iam?topic=iam-iammanidaccser#edit_existing).

2. Verify that the user who experiences this problem has the [Cloud Foundry developer role for the space](/docs/iam?topic=iam-mngcf#update_cf_access) where the service is provisioned.

3. If the correct permissions exists, try assigning a different permission and then re-assigning the required permission.

4. Wait a few minutes, then let the user try to bind the service again.

5. If this does not resolve the problem, then the {{site.data.keyword.cloud_notm}} IAM permissions are out of sync and you cannot resolve the issue yourself. [Contact IBM support](/docs/get-support?topic=get-support-getting-customer-support) by opening a support case. Make sure to provide the cluster ID, the user ID, and the service instance ID.
   1. Retrieve the cluster ID.
      ```
      ibmcloud ks clusters
      ```
      {: pre}

   2. Retrieve the service instance ID.
      ```
      ibmcloud service show <service_name> --guid
      ```
      {: pre}


<br />


## Binding a service to a cluster results in service does not support service keys error
{: #cs_service_keys}

{: tsSymptoms}
When you run `ibmcloud ks cluster-service-bind --cluster <cluster_name> --namespace <namespace> --service <service_instance_name>`, you see the following message.

```
This service doesn't support creation of keys
```
{: screen}

{: tsCauses}
Some services in {{site.data.keyword.cloud_notm}}, such as {{site.data.keyword.keymanagementservicelong}} do not support the creation of service credentials, also referred to as service keys. Without the support of service keys, the service is not bindable to a cluster. To find a list of services that support the creation of service keys, see [Enabling external apps to use {{site.data.keyword.cloud_notm}} services](/docs/resources?topic=resources-externalapp#externalapp).

{: tsResolve}
To integrate services that do not support service keys, check if the service provides an API that you can use to access the service directly from your app. For example, if you want to use {{site.data.keyword.keymanagementservicelong}}, see the [API reference ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/apidocs/kms?language=curl).

<br />


## After a worker node updates or reloads, duplicate nodes and pods appear
{: #cs_duplicate_nodes}

{: tsSymptoms}
When you run `kubectl get nodes`, you see duplicate worker nodes with the status **`NotReady`**. The worker nodes with **`NotReady`** have public IP addresses, while the worker nodes with **`Ready`** have private IP addresses.

{: tsCauses}
Older clusters listed worker nodes by the cluster's public IP address. Now, worker nodes are listed by the cluster's private IP address. When you reload or update a node, the IP address is changed, but the reference to the public IP address remains.

{: tsResolve}
Service is not disrupted due to these duplicates, but you can remove the old worker node references from the API server.

  ```
  kubectl delete node <node_name1> <node_name2>
  ```
  {: pre}

<br />


## Accessing a pod on a new worker node fails with a timeout
{: #cs_nodes_duplicate_ip}

{: tsSymptoms}
You deleted a worker node in your cluster and then added a worker node. When you deployed a pod or Kubernetes service, the resource cannot access the newly created worker node, and the connection times out.

{: tsCauses}
If you delete a worker node from your cluster and then add a worker node, the new worker node might be assigned the private IP address of the deleted worker node. Calico uses this private IP address as a tag and continues to try to reach the deleted node.

{: tsResolve}
Manually update the reference of the private IP address to point to the correct node.

1.  Confirm that you have two worker nodes with the same **Private IP** address. Note the **Private IP** and **ID** of the deleted worker.

  ```
  ibmcloud ks workers --cluster <cluster_name_or_id>
  ```
  {: pre}

  ```
  ID                                                 Public IP       Private IP       Machine Type   State     Status   Zone   Version
  kube-dal10-cr9b7371a7fcbe46d08e04f046d5e6d8b4-w1   169.xx.xxx.xxx  10.xxx.xx.xxx    b3c.4x16       normal    Ready    dal10      1.13.8
  kube-dal10-cr9b7371a7fcbe46d08e04f046d5e6d8b4-w2   169.xx.xxx.xxx  10.xxx.xx.xxx    b3c.4x16       deleted    -       dal10      1.13.8
  ```
  {: screen}

2.  Install the [Calico CLI](/docs/containers?topic=containers-network_policies#adding_network_policies).
3.  List the available worker nodes in Calico. Replace <path_to_file> with the local path to the Calico configuration file.

  ```
  calicoctl get nodes --config=filepath/calicoctl.cfg
  ```
  {: pre}

  ```
  NAME
  kube-dal10-cr9b7371a7faaa46d08e04f046d5e6d8b4-w1
  kube-dal10-cr9b7371a7faaa46d08e04f046d5e6d8b4-w2
  ```
  {: screen}

4.  Delete the duplicate worker node in Calico. Replace NODE_ID with the worker node ID.

  ```
  calicoctl delete node NODE_ID --config=<path_to_file>/calicoctl.cfg
  ```
  {: pre}

5.  Reboot the worker node that was not deleted.

  ```
  ibmcloud ks worker-reboot --cluster <cluster_name_or_id> --worker <worker_id>
  ```
  {: pre}


The deleted node is no longer listed in Calico.

<br />




## Pods fail to deploy because of a pod security policy
{: #cs_psp}

{: tsSymptoms}
After creating a pod or running `kubectl get events` to check on a pod deployment, you see an error message similar to the following.

```
unable to validate against any pod security policy
```
{: screen}

{: tsCauses}
[The `PodSecurityPolicy` admission controller](/docs/containers?topic=containers-psp) checks the authorization of the user or service account, such as a deployment or Helm tiller, that tried to create the pod. If no pod security policy supports the user or service account, then the `PodSecurityPolicy` admission controller prevents the pods from being created.

If you deleted one of the pod security policy resources for [{{site.data.keyword.IBM_notm}} cluster management](/docs/containers?topic=containers-psp#ibm_psp), you might experience similar issues.

{: tsResolve}
Make sure that the user or service account is authorized by a pod security policy. You might need to [modify an existing policy](/docs/containers?topic=containers-psp#customize_psp).

If you deleted an {{site.data.keyword.IBM_notm}} cluster management resource, refresh the Kubernetes master to restore it.

1.  [Log in to your account. If applicable, target the appropriate resource group. Set the context for your cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
2.  Refresh the Kubernetes master to restore it.

    ```
    ibmcloud ks apiserver-refresh
    ```
    {: pre}


<br />




## Cluster remains in a pending State
{: #cs_cluster_pending}

{: tsSymptoms}
When you deploy your cluster, it remains in a pending state and doesn't start.

{: tsCauses}
If you just created the cluster, the worker nodes might still be configuring. If you already wait for a while, you might have an invalid VLAN.

{: tsResolve}

You can try one of the following solutions:
  - Check the status of your cluster by running `ibmcloud ks clusters`. Then, check to be sure that your worker nodes are deployed by running `ibmcloud ks workers --cluster <cluster_name>`.
  - Check to see whether your VLAN is valid. To be valid, a VLAN must be associated with infrastructure that can host a worker with local disk storage. You can [list your VLANs](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlans) by running `ibmcloud ks vlans --zone <zone>` if the VLAN does not show in the list, then it is not valid. Choose a different VLAN.

<br />


## Cluster create error cannot pull images from registry
{: #ts_image_pull_create}

{: tsSymptoms}
When you created a cluster, you received an error message similar to the following.


```
Your cluster cannot pull images from the IBM Cloud Container Registry 'icr.io' domains because an IAM access policy could not be created. Make sure that you have the IAM Administrator platform role to IBM Cloud Container Registry. Then, create an image pull secret with IAM credentials to the registry by running 'ibmcloud ks cluster-pull-secret-apply'.
```
{: screen}

{: tsCauses}
During cluster creation, a service ID is created for your cluster and assigned the **Reader** service access policy to {{site.data.keyword.registrylong_notm}}. Then, an API key for this service ID is generated and stored in [an image pull secret](/docs/containers?topic=containers-images#cluster_registry_auth) to authorize the cluster to pull images from {{site.data.keyword.registrylong_notm}}.

To successfully assign the **Reader** service access policy to the service ID during cluster creation, you must have the **Administrator** platform access policy to {{site.data.keyword.registrylong_notm}}.

{: tsResolve}

Steps:
1.  Make sure that the account owner gives you the **Administrator** role to {{site.data.keyword.registrylong_notm}}.
    ```
    ibmcloud iam user-policy-create <your_user_email> --service-name container-registry --roles Administrator
    ```
    {: pre}
2.  [Use the `ibmcloud ks cluster-pull-secret-apply` command](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_pull_secret_apply) to re-create an image pull secret with the appropriate registry credentials.

<br />


## Failed to pull image from registry with `ImagePullBackOff` or authorization errors
{: #ts_image_pull}

{: tsSymptoms}

When you deploy a workload that pulls an image from {{site.data.keyword.registrylong_notm}}, your pods fail with an **`ImagePullBackOff`** status.

```
kubectl get pods
```
{: pre}

```
NAME         READY     STATUS             RESTARTS   AGE
<pod_name>   0/1       ImagePullBackOff   0          2m
```
{: screen}

When you describe the pod, you see authentication errors similar to the following.

```
kubectl describe pod <pod_name>
```
{: pre}

```
Failed to pull image "<region>.icr.io/<namespace>/<image>:<tag>" ... unauthorized: authentication required
Failed to pull image "<region>.icr.io/<namespace>/<image>:<tag>" ... 401 Unauthorized
```
{: screen}

```
Failed to pull image "registry.ng.bluemix.net/<namespace>/<image>:<tag>" ... unauthorized: authentication required
Failed to pull image "registry.ng.bluemix.net/<namespace>/<image>:<tag>" ... 401 Unauthorized
```
{: screen}

{: tsCauses}
Your cluster uses an API key or token that is stored in an [image pull secret](/docs/containers?topic=containers-images#cluster_registry_auth) to authorize the cluster to pull images from {{site.data.keyword.registrylong_notm}}. By default, new clusters have image pull secrets that use API keys so that the cluster can pull images from any regional `icr.io` registry for containers that are deployed to the `default` Kubernetes namespace.

For clusters that were created before **1 July 2019**, the cluster might have an image pull secret that uses a token. Tokens grant access to {{site.data.keyword.registrylong_notm}} for only certain regional registries that use the deprecated `<region>.registry.bluemix.net` domains.

{: tsResolve}

1.  Verify that you use the correct name and tag of the image in your deployment YAML file.
    ```
    ibmcloud cr images
    ```
    {: pre}
2.  Get the pod configuration file of a failing pod, and look for the `imagePullSecrets` section.
    ```
    kubectl get pod <pod_name> -o yaml
    ```
    {: pre}

    Example output:
    ```
    ...
    imagePullSecrets:
    - name: bluemix-default-secret
    - name: bluemix-default-secret-regional
    - name: bluemix-default-secret-international
    - name: default-us-icr-io
    - name: default-uk-icr-io
    - name: default-de-icr-io
    - name: default-au-icr-io
    - name: default-jp-icr-io
    - name: default-icr-io
    ...
    ```
    {: screen}
3.  If no image pull secrets are listed, set up the image pull secret in your namespace.
    1.  Verify that the `default` namespace has `icr-io` image pull secrets for each regional registry that you want to use. If no `icr-io` secrets are listed in the namespace, [use the `ibmcloud ks cluster-pull-secret-apply --cluster <cluster_name_or_ID>` command](/docs/containers?topic=containers-images#imagePullSecret_migrate_api_key) to create the image pull secrets in the `default` namespace.
        ```
        kubectl get secrets -n default | grep "icr-io"
        ```
        {: pre}
    2.  [Copy the image pull secrets from the `default` Kubernetes namespace to the namespace where you want to deploy your workload](/docs/containers?topic=containers-images#copy_imagePullSecret).
    3.  [Add the image pull secret to the service account for this Kubernetes namespace](/docs/containers?topic=containers-images#store_imagePullSecret) so that all pods in the namespace can use the image pull secret credentials.
4.  If image pull secrets are listed in the pod, determine what type of credentials you use to access {{site.data.keyword.registrylong_notm}}.
    *   **Deprecated**: If the secret has `bluemix` in the name, you use a registry token to authenticate with the deprecated `registry.<region>.bluemix.net` domain names. Continue with [Troubleshooting image pull secrets that use tokens](#ts_image_pull_token).
    *   If the secret has `icr` in the name, you use an API key to authenticate with the `icr.io` domain names. Continue with [Troubleshooting image pull secrets that use API keys](#ts_image_pull_apikey).
    *   If you have both types of secrets, then you use both authentication methods. Going forward, use the `icr.io` domain names in your deployment YAMLs for the container image. Continue with [Troubleshooting image pull secrets that use API keys](#ts_image_pull_apikey).

<br>
<br>

**Troubleshooting image pull secrets that use API keys**</br>
{: #ts_image_pull_apikey}

If your pod configuration has an image pull secret that uses an API key, check that the API key credentials are set up correctly.
{: shortdesc}

The following steps assume that the API key stores the credentials of a service ID. If you set up your image pull secret to use an API key of an individual user, you must verify that user's {{site.data.keyword.cloud_notm}} IAM permissions and credentials.
{: note}

1.  Find the service ID that API key uses for the image pull secret by reviewing the **Description**. The service ID that is created with the cluster says `ID for <cluster_name>` and is used in the `default` Kubernetes namespace. If you created another service ID such as to access a different Kubernetes namespace or to modify {{site.data.keyword.cloud_notm}} IAM permissions, you customized the description.
    ```
    ibmcloud iam service-ids
    ```
    {: pre}

    Example output:
    ```
    UUID                Name               Created At              Last Updated            Description                                                                                                                                                                                         Locked     
    ServiceId-aa11...   <service_ID_name>  2019-02-01T19:01+0000   2019-02-01T19:01+0000   ID for <cluster_name>                                                                                                                                         false   
    ServiceId-bb22...   <service_ID_name>  2019-02-01T19:01+0000   2019-02-01T19:01+0000   Service ID for IBM Cloud Container Registry in Kubernetes cluster <cluster_name> namespace <kube_namespace>                                                                                                                                         false    
    ```
    {: screen}
2.  Verify that the service ID is assigned at least an {{site.data.keyword.cloud_notm}} IAM **Reader** [service access role policy for {{site.data.keyword.registryshort_notm}}](/docs/services/Registry?topic=registry-user#create). If the service ID does not have the **Reader** service role, [edit the IAM policies](/docs/iam?topic=iam-serviceidpolicy#access_edit). If the policies are correct, continue with the next step to see if the credentials are valid.
    ```
    ibmcloud iam service-policies <service_ID_name>
    ```
    {: pre}

    Example output:
    ```              
    Policy ID:   a111a111-b22b-333c-d4dd-e555555555e5   
    Roles:       Reader   
    Resources:                            
                  Service Name       container-registry      
                  Service Instance         
                  Region                  
                  Resource Type      namespace      
                  Resource           <registry_namespace>  
    ```
    {: screen}  
3.  Check if the image pull secret credentials are valid.
    1.  Get the image pull secret configuration. If the pod is not in the `default` namespace, include the `-n` flag.
        ```
        kubectl get secret <image_pull_secret_name> -o yaml [-n <namespace>]
        ```
        {: pre}
    2.  In the output, copy the base64 encoded value of the `.dockercfg` field.
        ```
        apiVersion: v1
        kind: Secret
        data:
          .dockercfg: eyJyZWdp...==
        ...
        ```
        {: screen}
    3.  Decode the base64 string. For example, on OS X you can run the following command.
        ```
        echo -n "<base64_string>" | base64 --decode
        ```
        {: pre}

        Example output:
        ```
        {"auths":{"<region>.icr.io":{"username":"iamapikey","password":"<password_string>","email":"<name@abc.com>","auth":"<auth_string>"}}}
        ```
        {: screen}
    4.  Compare the image pull secret regional registry domain name with the domain name that you specified in the container image. By default, new clusters have image pull secrets for each regional registry domain name for containers that run in the `default` Kubernetes namespace. However, if you modified the default settings or are using a different Kubernetes namespace, you might not have an image pull secret for the regional registry. [Copy an image pull secret](/docs/containers?topic=containers-images#copy_imagePullSecret) for the regional registry domain name.
    5.  Log in to the registry from your local machine by using the `username` and `password` from your image pull secret. If you cannot log in, you might need to fix the service ID.
        ```
        docker login -u iamapikey -p <password_string> <region>.icr.io
        ```
        {: pre}
        1.  Re-create the cluster service ID, {{site.data.keyword.cloud_notm}} IAM policies, API key, and image pull secrets for containers that run in the `default` Kubernetes namespace.
            ```
            ibmcloud ks cluster-pull-secret-apply --cluster <cluster_name_or_ID>
            ```
            {: pre}
        2.  Re-create your deployment in the `default` Kubernetes namespace. If you still see an authorization error message, repeat Steps 1-5 with the new image pull secrets. If you still cannot log in, [contact the IBM team on Slack, or open an {{site.data.keyword.cloud_notm}} Support case](#clusters_getting_help).
    6.  If the login succeeds, pull an image locally. If the command fails with an `access denied` error, the registry account is in a different {{site.data.keyword.cloud_notm}} account than the one your cluster is in. [Create an image pull secret to access images in the other account](/docs/containers?topic=containers-images#other_registry_accounts). If you can pull an image to your local machine, then your API key has the right permissions, but the API setup in your cluster is not correct. You cannot resolve this issue. [Contact the IBM team on Slack, or open an {{site.data.keyword.cloud_notm}} Support case](#clusters_getting_help).
        ```
        docker pull <region>icr.io/<namespace>/<image>:<tag>
        ```
        {: pre}

<br>
<br>

**Deprecated: Troubleshooting image pull secrets that use tokens**</br>
{: #ts_image_pull_token}

If your pod configuration has an image pull secret that uses a token, check that the token credentials are valid.
{: shortdesc}

This method of using a token to authorize cluster access to {{site.data.keyword.registrylong_notm}} for the `registry.bluemix.net` domain names is deprecated. Before tokens become unsupported, update your deployments to [use the API key method](/docs/containers?topic=containers-images#cluster_registry_auth) to authorize cluster access to the new `icr.io` registry domain names.
{: deprecated}

1.  Get the image pull secret configuration. If the pod is not in the `default` namespace, include the `-n` flag.
    ```
    kubectl get secret <image_pull_secret_name> -o yaml [-n <namespace>]
    ```
    {: pre}
2.  In the output, copy the base64 encoded value of the `.dockercfg` field.
    ```
    apiVersion: v1
    kind: Secret
    data:
      .dockercfg: eyJyZWdp...==
    ...
    ```
    {: screen}
3.  Decode the base64 string. For example, on OS X you can run the following command.
    ```
    echo -n "<base64_string>" | base64 --decode
    ```
    {: pre}

    Example output:
    ```
    {"auths":{"registry.<region>.bluemix.net":{"username":"token","password":"<password_string>","email":"<name@abc.com>","auth":"<auth_string>"}}}
    ```
    {: screen}
4.  Compare the registry domain name with the domain name that you specified in the container image. For example, if the image pull secret authorizes access to the `registry.ng.bluemix.net` domain but you specified an image that is stored in `registry.eu-de.bluemix.net`, you must [create a token to use in an image pull secret](/docs/containers?topic=containers-images#token_other_regions_accounts) for `registry.eu-de.bluemix.net`.
5.  Log in to the registry from your local machine by using the `username` and `password` from the image pull secret. If you cannot log in, the token has an issue that you cannot resolve. [Contact the IBM team on Slack, or open an {{site.data.keyword.cloud_notm}} Support case](#clusters_getting_help).
    ```
    docker login -u token -p <password_string> registry.<region>.bluemix.net
    ```
    {: pre}
6.  If the login succeeds, pull an image locally. If the command fails with an `access denied` error, the registry account is in a different {{site.data.keyword.cloud_notm}} account than the one your cluster is in. [Create an image pull secret to access images in the other account](/docs/containers?topic=containers-images#token_other_regions_accounts). If the command succeeds, [contact the IBM team on Slack, or open an {{site.data.keyword.cloud_notm}} Support case](#clusters_getting_help).
    ```
    docker pull registry.<region>.bluemix.net/<namespace>/<image>:<tag>
    ```
    {: pre}

<br />


## Pods remain in pending state
{: #cs_pods_pending}

{: tsSymptoms}
When you run `kubectl get pods`, you can see pods that remain in a **Pending** state.

{: tsCauses}
If you just created the Kubernetes cluster, the worker nodes might still be configuring.

If this cluster is an existing one:
*  You might not have enough capacity in your cluster to deploy the pod.
*  The pod might have exceeded a resource request or limit.

{: tsResolve}
This task requires the {{site.data.keyword.cloud_notm}} IAM [**Administrator** platform role](/docs/containers?topic=containers-users#platform) for the cluster and the [**Manager** service role](/docs/containers?topic=containers-users#platform) for all namespaces.

If you just created the Kubernetes cluster, run the following command and wait for the worker nodes to initialize.

```
kubectl get nodes
```
{: pre}

If this cluster is an existing one, check your cluster capacity.

1.  Set the proxy with the default port number.

  ```
  kubectl proxy
  ```
   {: pre}

2.  Open the Kubernetes dashboard.

  ```
  http://localhost:8001/ui
  ```
  {: pre}

3.  Check if you have enough capacity in your cluster to deploy your pod.

4.  If you don't have enough capacity in your cluster, resize your worker pool to add more nodes.

    1.  Review the current sizes and flavors of your worker pools to decide which one to resize.

        ```
        ibmcloud ks worker-pools
        ```
        {: pre}

    2.  Resize your worker pools to add more nodes to each zone that the pool spans.

        ```
        ibmcloud ks worker-pool-resize --worker-pool <worker_pool> --cluster <cluster_name_or_ID> --size-per-zone <workers_per_zone>
        ```
        {: pre}

5.  Optional: Check your pod resource requests.

    1.  Confirm that the `resources.requests` values are not larger than the worker node's capacity. For example, if the pod request `cpu: 4000m`, or 4 cores, but the worker node size is only 2 cores, the pod cannot be deployed.

        ```
        kubectl get pod <pod_name> -o yaml
        ```
        {: pre}

    2.  If the request exceeds the available capacity, [add a new worker pool](/docs/containers?topic=containers-add_workers#add_pool) with worker nodes that can fulfill the request.

6.  If your pods still stay in a **pending** state after the worker node is fully deployed, review the [Kubernetes documentation ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/tasks/debug-application-cluster/debug-pod-replication-controller/#my-pod-stays-pending) to further troubleshoot the pending state of your pod.

<br />


## Containers do not start
{: #containers_do_not_start}

{: tsSymptoms}
The pods deploy successfully to clusters, but the containers do not start.

{: tsCauses}
Containers might not start when the registry quota is reached.

{: tsResolve}
[Free up storage in {{site.data.keyword.registryshort_notm}}.](/docs/services/Registry?topic=registry-registry_quota#registry_quota_freeup)

<br />


## Pods repeatedly fail to restart or are unexpectedly removed
{: #pods_fail}

{: tsSymptoms}
Your pod was healthy but unexpectedly gets removed or gets stuck in a restart loop.

{: tsCauses}
Your containers might exceed their resource limits, or your pods might be replaced by higher priority pods.

{: tsResolve}
To see if a container is being killed because of a resource limit:
<ol><li>Get the name of your pod. If you used a label, you can include it to filter your results.<pre class="pre"><code>kubectl get pods --selector='app=wasliberty'</code></pre></li>
<li>Describe the pod and look for the **Restart Count**.<pre class="pre"><code>kubectl describe pod</code></pre></li>
<li>If the pod restarted many times in a short period of time, fetch its status. <pre class="pre"><code>kubectl get pod -o go-template={{range.status.containerStatuses}}{{"Container Name: "}}{{.name}}{{"\r\nLastState: "}}{{.lastState}}{{end}}</code></pre></li>
<li>Review the reason. For example, `OOM Killed` means "out of memory," indicating that the container is crashing because of a resource limit.</li>
<li>Add capacity to your cluster so that the resources can be fulfilled.</li></ol>

<br>

To see if your pod is being replaced by higher priority pods:
1.  Get the name of your pod.

    ```
    kubectl get pods
    ```
    {: pre}

2.  Describe your pod YAML.

    ```
    kubectl get pod <pod_name> -o yaml
    ```
    {: pre}

3.  Check the `priorityClassName` field.

    1.  If there is no `priorityClassName` field value, then your pod has the `globalDefault` priority class. If your cluster admin did not set a `globalDefault` priority class, then the default is zero (0), or the lowest priority. Any pod with a higher priority class can preempt, or remove, your pod.

    2.  If there is a `priorityClassName` field value, get the priority class.

        ```
        kubectl get priorityclass <priority_class_name> -o yaml
        ```
        {: pre}

    3.  Note the `value` field to check your pod's priority.

4.  List existing priority classes in the cluster.

    ```
    kubectl get priorityclasses
    ```
    {: pre}

5.  For each priority class, get the YAML file and note the `value` field.

    ```
    kubectl get priorityclass <priority_class_name> -o yaml
    ```
    {: pre}

6.  Compare your pod's priority class value with the other priority class values to see if it is higher or lower in priority.

7.  Repeat steps 1 to 3 for other pods in the cluster, to check what priority class they are using. If those other pods' priority class is higher than your pod, your pod is not provisioned unless there is enough resources for your pod and every pod with higher priority.

8.  Contact your cluster admin to add more capacity to your cluster and confirm that the right priority classes are assigned.

<br />


## Cannot install a Helm chart with updated configuration values
{: #cs_helm_install}

{: tsSymptoms}
When you try to install an updated Helm chart by running `helm install -f config.yaml --namespace=kube-system --name=<release_name> iks-charts/<chart_name>`, you get the `Error: failed to download "iks-charts/<chart_name>"` error message.

{: tsCauses}
The URL for the {{site.data.keyword.cloud_notm}} repository in your Helm instance might be incorrect.

{: tsResolve}
To troubleshoot your Helm chart:

1. List the repositories currently available in your Helm instance.

    ```
    helm repo list
    ```
    {: pre}

2. In the output, verify that the URL for the {{site.data.keyword.cloud_notm}} repository, `ibm`, is `https://icr.io/helm/iks-charts`.

    ```
    NAME    URL
    stable  https://kubernetes-charts.storage.googleapis.com
    local   http://127.0.0.1:8888/charts
    ibm     https://icr.io/helm/iks-charts
    ```
    {: screen}

    * If the URL is incorrect:

        1. Remove the {{site.data.keyword.cloud_notm}} repository.

            ```
            helm repo remove ibm
            ```
            {: pre}

        2. Add the {{site.data.keyword.cloud_notm}} repository again.

            ```
            helm repo add iks-charts  https://icr.io/helm/iks-charts
            ```
            {: pre}

    * If the URL is correct, get the latest updates from the repository.

        ```
        helm repo update
        ```
        {: pre}

3. Install the Helm chart with your updates.

    ```
    helm install -f config.yaml --namespace=kube-system --name=<release_name> iks-charts/<chart_name>
    ```
    {: pre}

<br />


## Cannot install Helm tiller or deploy containers from public images in my cluster
{: #cs_tiller_install}

{: tsSymptoms}

When you try to install Helm tiller or want to deploy images from public registries, such as DockerHub, the installation fails with an error similar to the following:

```
Failed to pull image "gcr.io/kubernetes-helm/tiller:v2.12.0": rpc error: code = Unknown desc = failed to resolve image "gcr.io/kubernetes-helm/tiller:v2.12.0": no available registry endpoint:
```
{: screen}

{: tsCauses}
You might have set up a custom firewall, specified custom Calico policies, or created a private-only cluster by using the private service endpoint that block public network connectivity to the container registry where the image is stored.

{: tsResolve}
- If you have a custom firewall or set custom Calico policies, allow outbound and inbound network traffic between your worker nodes and the container registry where the image is stored. If the image is stored in {{site.data.keyword.registryshort_notm}}, review the required ports in [Allowing the cluster to access infrastructure resources and other services](/docs/containers?topic=containers-firewall#firewall_outbound).
- If you created a private cluster by enabling the private service endpoint only, you can [enable the public service endpoint](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_feature_disable) for your cluster. If want to install Helm charts in a private cluster without opening up a public connection, you can install Helm [with Tiller](/docs/containers?topic=containers-helm#private_local_tiller) or [without Tiller](/docs/containers?topic=containers-helm#private_install_without_tiller).

<br />


## Getting help and support
{: #clusters_getting_help}

Still having issues with your cluster?
{: shortdesc}

-  In the terminal, you are notified when updates to the `ibmcloud` CLI and plug-ins are available. Be sure to keep your CLI up-to-date so that you can use all available commands and flags.
-   To see whether {{site.data.keyword.cloud_notm}} is available, [check the {{site.data.keyword.cloud_notm}} status page ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/status?selected=status).
-   Post a question in the [{{site.data.keyword.containerlong_notm}} Slack ![External link icon](../icons/launch-glyph.svg "External link icon")](https://ibm-container-service.slack.com).
    If you are not using an IBM ID for your {{site.data.keyword.cloud_notm}} account, [request an invitation](https://cloud.ibm.com/kubernetes/slack) to this Slack.
    {: tip}
-   Review the forums to see whether other users ran into the same issue. When you use the forums to ask a question, tag your question so that it is seen by the {{site.data.keyword.cloud_notm}} development teams.
    -   If you have technical questions about developing or deploying clusters or apps with {{site.data.keyword.containerlong_notm}}, post your question on [Stack Overflow ![External link icon](../icons/launch-glyph.svg "External link icon")](https://stackoverflow.com/questions/tagged/ibm-cloud+containers) and tag your question with `ibm-cloud`, `kubernetes`, and `containers`.
    -   For questions about the service and getting started instructions, use the [IBM Developer Answers ![External link icon](../icons/launch-glyph.svg "External link icon")](https://developer.ibm.com/answers/topics/containers/?smartspace=bluemix) forum. Include the `ibm-cloud` and `containers` tags.
    See [Getting help](/docs/get-support?topic=get-support-getting-customer-support#using-avatar) for more details about using the forums.
-   Contact IBM Support by opening a case. To learn about opening an IBM support case, or about support levels and case severities, see [Contacting support](/docs/get-support?topic=get-support-getting-customer-support).
When you report an issue, include your cluster ID. To get your cluster ID, run `ibmcloud ks clusters`. You can also use the [{{site.data.keyword.containerlong_notm}} Diagnostics and Debug Tool](/docs/containers?topic=containers-cs_troubleshoot#debug_utility) to gather and export pertinent information from your cluster to share with IBM Support.
{: tip}

