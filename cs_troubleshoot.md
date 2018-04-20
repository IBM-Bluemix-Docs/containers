---

copyright:
  years: 2014, 2018
lastupdated: "2018-4-20"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}


# Debugging your cluster
{: #cs_troubleshoot}

As you use {{site.data.keyword.containerlong}}, consider these techniques for general troubleshooting and debugging your clusters. You can also check the [status of the {{site.data.keyword.Bluemix_notm}} system ![External link icon](../icons/launch-glyph.svg "External link icon")](https://developer.ibm.com/bluemix/support/#status).
{: shortdesc}

You can take some general steps to ensure that your clusters are up-to-date:
- Check on a monthly basis for available security and operating system patches to [update your worker nodes](cs_cli_reference.html#cs_worker_update).
- [Update your cluster](cs_cli_reference.html#cs_cluster_update) to the latest default [version of Kubernetes](cs_versions.html) for {{site.data.keyword.containershort_notm}}

## Debugging clusters
{: #debug_clusters}

Review the options to debug your clusters and find the root causes for failures.

1.  List your cluster and find the `State` of the cluster.

  ```
  bx cs clusters
  ```
  {: pre}

2.  Review the `State` of your cluster. If your cluster is in a **Critical**, **Delete failed**, or **Warning** state, or is stuck in the **Pending** state for a long time, start [debugging the worker nodes](#debug_worker_nodes).

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
       <td>All worker nodes in a cluster are up and running. You can access the cluster and deploy apps to the cluster. This state is considered healthy and does not require an action from you. **Note**: Although the worker nodes might be normal, other infrastructure resources, such as [networking](cs_troubleshoot_network.html) and [storage](cs_troubleshoot_storage.html), might still need attention.</td>
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


## Debugging worker nodes
{: #debug_worker_nodes}

Review the options to debug your worker nodes and find the root causes for failures.


1.  If your cluster is in a **Critical**, **Delete failed**, or **Warning** state, or is stuck in the **Pending** state for a long time, review the state of your worker nodes.

  ```
  bx cs workers <cluster_name_or_id>
  ```
  {: pre}

2.  Review the `State` and `Status` field for every worker node in your CLI output.

  <table summary="Every table row should be read left to right, with the cluster state in column one and a description in column two.">
    <thead>
    <th>Worker node state</th>
    <th>Description</th>
    </thead>
    <tbody>
  <tr>
      <td>Critical</td>
      <td>A worker node can go into a Critical state for many reasons. The most common reasons are the following: <ul><li>You initiated a reboot for your worker node without cordoning and draining your worker node. Rebooting a worker node can cause data corruption in <code>docker</code>, <code>kubelet</code>, <code>kube-proxy</code>, and <code>calico</code>. </li><li>The pods that are deployed to your worker node do not use resource limits for [memory ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/tasks/configure-pod-container/assign-memory-resource/) and [CPU ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/tasks/configure-pod-container/assign-cpu-resource/). Without resource limits, pods can consume all available resources, leaving no resources for other pods to run on this worker node. This overcommitment of workload causes the worker node to fail. </li><li><code>Docker</code>, <code>kubelet</code> or <code>calico</code> went into an unrecoverable state after running hundreds or thousands of containers over time. </li><li>You set up a Vyatta for your worker node that went down and cut off the communication between your worker node and the Kubernetes master. </li><li> Current networking issues in {{site.data.keyword.containershort_notm}} or IBM Cloud infrastructure (SoftLayer) that causes the communication between your worker node and the Kubernetes master to fail.</li><li>Your worker node ran out of capacity. Check the <strong>Status</strong> of the worker node to see if it shows <strong>Out of disk</strong> or <strong>Out of memory</strong>. If your worker node is out of capacity, consider to either reduce the workload on your worker node or add a worker node to your cluster to help load balance the workload.</li></ul> In many cases, [reloading](cs_cli_reference.html#cs_worker_reload) your worker node can solve the problem. When you reload your worker node, the latest [patch version](cs_versions.html#version_types) is applied to your worker node. The major and minor version is not changed. Before you reload your worker node, make sure to cordon and drain your worker node to ensure that existing pods are terminated gracefully and rescheduled onto remaining worker nodes. </br></br> If reloading the worker node does not resolve the issue, go to the next step to continue troubleshooting your worker node. </br></br><strong>Tip:</strong> You can [configure health checks for your worker node and enable Autorecovery](cs_health.html#autorecovery). If Autorecovery detects an unhealthy worker node based on the configured checks, Autorecovery triggers a corrective action like an OS reload on the worker node. For more information about how Autorecovery works, see the [Autorecovery blog ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/blogs/bluemix/2017/12/autorecovery-utilizes-consistent-hashing-high-availability/).
      </td>
     </tr>
      <tr>
        <td>Deploying</td>
        <td>When you update the Kubernetes version of your worker node, your worker node is redeployed to install the updates. If your worker node is stuck in this state for a long time, continue with the next step to see if a problem occurred during the deployment. </td>
     </tr>
        <tr>
        <td>Normal</td>
        <td>Your worker node is fully provisioned and ready to be used in the cluster. This state is considered healthy and does not require an action from the user. **Note**: Although the worker nodes might be normal, other infrastructure resources, such as [networking](cs_troubleshoot_network.html) and [storage](cs_troubleshoot_storage.html), might still need attention.</td>
     </tr>
   <tr>
        <td>Provisioning</td>
        <td>Your worker node is being provisioned and is not available in the cluster yet. You can monitor the provisioning process in the <strong>Status</strong> column of your CLI output. If your worker node is stuck in this state for a long time, and you cannot see any progress in the <strong>Status</strong> column, continue with the next step to see if a problem occurred during the provisioning.</td>
      </tr>
      <tr>
        <td>Provision_failed</td>
        <td>Your worker node could not be provisioned. Continue with the next step to find the details for the failure.</td>
      </tr>
   <tr>
        <td>Reloading</td>
        <td>Your worker node is being reloaded and is not available in the cluster. You can monitor the reloading process in the <strong>Status</strong> column of your CLI output. If your worker node is stuck in this state for a long time, and you cannot see any progress in the <strong>Status</strong> column, continue with the next step to see if a problem occurred during the reloading.</td>
       </tr>
       <tr>
        <td>Reloading_failed </td>
        <td>Your worker node could not be reloaded. Continue with the next step to find the details for the failure.</td>
      </tr>
      <tr>
        <td>Reload_pending </td>
        <td>A request to reload or to update the Kubernetes version of your worker node is sent. When the worker node is being reloaded, the state changes to <code>Reloading</code>. </td>
      </tr>
      <tr>
       <td>Unknown</td>
       <td>The Kubernetes master is not reachable for one of the following reasons:<ul><li>You requested an update of your Kubernetes master. The state of the worker node cannot be retrieved during the update.</li><li>You might have an additional firewall that is protecting your worker nodes, or changed firewall settings recently. {{site.data.keyword.containershort_notm}} requires certain IP addresses and ports to be opened to allow communication from the worker node to the Kubernetes master and vice versa. For more information, see [Firewall prevents worker nodes from connecting](cs_troubleshoot_clusters.html#cs_firewall).</li><li>The Kubernetes master is down. Contact {{site.data.keyword.Bluemix_notm}} support by opening an [{{site.data.keyword.Bluemix_notm}} support ticket](#ts_getting_help).</li></ul></td>
  </tr>
     <tr>
        <td>Warning</td>
        <td>Your worker node is reaching the limit for memory or disk space. You can either reduce work load on your worker node or add a worker node to your cluster to help load balance the work load.</td>
  </tr>
    </tbody>
  </table>

5.  List the details for the worker node. If the details include an error message, review the list of [common error messages for worker nodes](#common_worker_nodes_issues) to learn how to resolve the problem.

   ```
   bx cs worker-get <worker_id>
   ```
   {: pre}

  ```
  bx cs worker-get [<cluster_name_or_id>] <worker_node_id>
  ```
  {: pre}

<br />


## Common issues with worker nodes
{: #common_worker_nodes_issues}

Review common error messages and learn how to resolve them.

  <table>
    <thead>
    <th>Error message</th>
    <th>Description and resolution
    </thead>
    <tbody>
      <tr>
        <td>{{site.data.keyword.Bluemix_notm}} Infrastructure Exception: Your account is currently prohibited from ordering 'Computing Instances'.</td>
        <td>Your IBM Cloud infrastructure (SoftLayer) account might be restricted from ordering compute resources. Contact {{site.data.keyword.Bluemix_notm}} support by opening an [{{site.data.keyword.Bluemix_notm}} support ticket](#ts_getting_help).</td>
      </tr>
      <tr>
        <td>{{site.data.keyword.Bluemix_notm}} Infrastructure Exception: Could not place order. There are insufficient resources behind router 'router_name' to fulfill the request for the following guests: 'worker_id'.</td>
        <td>The VLAN that you selected is associated with a pod in the data center that has insufficient space to provision your worker node. You can choose between the following options:<ul><li>Use a different data center to provision your worker node. Run <code>bx cs locations</code> to list available data center.<li>If you have an existing public and private VLAN pair that is associated with another pod in the data center, use this VLAN pair instead.<li>Contact {{site.data.keyword.Bluemix_notm}} support by opening an [{{site.data.keyword.Bluemix_notm}} support ticket](#ts_getting_help).</ul></td>
      </tr>
      <tr>
        <td>{{site.data.keyword.Bluemix_notm}} Infrastructure Exception: Could not obtain network VLAN with id: &lt;vlan id&gt;.</td>
        <td>Your worker node could not be provisioned because the selected VLAN ID could not be found for one of the following reasons:<ul><li>You might have specified the VLAN number instead of the VLAN ID. The VLAN number is 3 or 4 digits long, whereas the VLAN ID is 7 digits long. Run <code>bx cs vlans &lt;location&gt;</code> to retrieve the VLAN ID.<li>The VLAN ID might not be associated with the IBM Cloud infrastructure (SoftLayer) account that you use. Run <code>bx cs vlans &lt;location&gt;</code> to list available VLAN IDs for your account. To change the IBM Cloud infrastructure (SoftLayer) account, see [bx cs credentials-set](cs_cli_reference.html#cs_credentials_set). </ul></td>
      </tr>
      <tr>
        <td>SoftLayer_Exception_Order_InvalidLocation: The location provided for this order is invalid. (HTTP 500)</td>
        <td>Your IBM Cloud infrastructure (SoftLayer) is not set up to order compute resources in the selected data center. Contact [{{site.data.keyword.Bluemix_notm}} support](#ts_getting_help) to verify that you account is setup correctly.</td>
       </tr>
       <tr>
        <td>{{site.data.keyword.Bluemix_notm}} Infrastructure Exception: The user does not have the necessary {{site.data.keyword.Bluemix_notm}} Infrastructure permissions to add servers
        </br></br>
        {{site.data.keyword.Bluemix_notm}} Infrastructure Exception: 'Item' must be ordered with permission.</td>
        <td>You might not have the required permissions to provision a worker node from the IBM Cloud infrastructure (SoftLayer) portfolio. See [Configure access to the IBM Cloud infrastructure (SoftLayer) portfolio to create standard Kubernetes clusters](cs_infrastructure.html#unify_accounts).</td>
      </tr>
      <tr>
       <td>Worker unable to talk to {{site.data.keyword.containershort_notm}} servers. Please verify your firewall setup is allowing traffic from this worker.
       <td><ul><li>If you have a firewall, [configure your firewall settings to allow outgoing traffic to the appropriate ports and IP addresses](cs_firewall.html#firewall_outbound).</li><li>Check if your cluster does not have a public IP by running `bx cs workers <mycluster>`. If there is no public IP listed, then your cluster only has private VLANs.<ul><li>If you want the cluster to have only private VLANs, make sure that you have set up your [VLAN connection](cs_clusters.html#worker_vlan_connection) and your [firewall](cs_firewall.html#firewall_outbound).</li><li>If you want the cluster to have a public IP, [add new worker nodes](cs_cli_reference.html#cs_worker_add) with both public and private VLANs.</li></ul></li></ul></td>
     </tr>
      <tr>
  <td>Cannot create IMS portal token, as no IMS account is linked to the selected BSS account</br></br>Provided user not found or active</br></br>SoftLayer_Exception_User_Customer_InvalidUserStatus: User account is currently cancel_pending.</br></br>Waiting for machine to be visible to the user</td>
  <td>The owner of the API key that is used to access the IBM Cloud infrastructure (SoftLayer) portolio does not have the required permissions to perform the action, or might be pending deletion.</br></br><strong>As the user</strong>, follow these steps: <ol><li>If you have access to multiple accounts, make sure that you are logged in to the account where you want to work with {{site.data.keyword.containerlong_notm}}. </li><li>Run <code>bx cs api-key-info</code> to view the current API key owner that is used to access the IBM Cloud infrastructure (SoftLayer) portolio. </li><li>Run <code>bx account list</code> to view the owner of the {{site.data.keyword.Bluemix_notm}} account that you currently use. </li><li>Contact the owner of the {{site.data.keyword.Bluemix_notm}} account and report that the API key owner that you retrieved earlier has insufficient permissions in IBM Cloud infrastructure (SoftLayer) or might be pending to be deleted. </li></ol></br><strong>As the account owner</strong>, follow these steps: <ol><li>Review the [required permissions in IBM Cloud infrastructure (SoftLayer)](cs_users.html#managing) to perform the action that previously failed. </li><li>Fix the permissions of the API key owner or create a new API key by using the [<code>bx cs api-key-reset</code>](cs_cli_reference.html#cs_api_key_reset) command. </li><li>If you or another account admin manually set IBM Cloud infrastructure (SoftLayer) credentials in your account, run [<code>bx cs credentials-unset</code>](cs_cli_reference.html#cs_credentials_unset) to remove the credentials from your account.</li></ol></td>
  </tr>
    </tbody>
  </table>



<br />




## Debugging app deployments
{: #debug_apps}

Review the options that you have to debug your app deployments and find the root causes for failures.

1. Look for abnormalities in the service or deployment resources by running the `describe` command.

 Example:
 <pre class="pre"><code>kubectl describe service &lt;service_name&gt; </code></pre>

2. [Check if the containers are stuck in the ContainerCreating state](cs_troubleshoot_storage.html#stuck_creating_state).

3. Check if the cluster is in the `Critical` state. If the cluster is in a `Critical` state, check the firewall rules and verify that the master can communicate with the worker nodes.

4. Verify that the service is listening on the correct port.
   1. Get the name of a pod.
     <pre class="pre"><code>kubectl get pods</code></pre>
   2. Log in to a container.
     <pre class="pre"><code>kubectl exec -it &lt;pod_name&gt; -- /bin/bash</code></pre>
   3. Curl the app from within the container. If the port is not accessible, the service might not be listening on the correct port or the app might have issues. Update the configuration file for the service with the correct port and redeploy or investigate potential issues with the app.
     <pre class="pre"><code>curl localhost: &lt;port&gt;</code></pre>

5. Verify that the service is linked correctly to the pods.
   1. Get the name of a pod.
     <pre class="pre"><code>kubectl get pods</code></pre>
   2. Log in to a container.
     <pre class="pre"><code>kubectl exec -it &lt;pod_name&gt; -- /bin/bash</code></pre>
   3. Curl the cluster IP address and port of the service. If the IP address and port are not accessible, look at the endpoints for the service. If there are no endpoints, then the selector for the service does not match the pods. If there are endpoints, then look at the target port field on the service and make sure that the target port is the same as what is being used for the pods.
     <pre class="pre"><code>curl &lt;cluster_IP&gt;:&lt;port&gt;</code></pre>

6. For Ingress services, verify that the service is accessible from within the cluster.
   1. Get the name of a pod.
     <pre class="pre"><code>kubectl get pods</code></pre>
   2. Log in to a container.
     <pre class="pre"><code>kubectl exec -it &lt;pod_name&gt; -- /bin/bash</code></pre>
   2. Curl the URL specified for the Ingress service. If the URL is not accessible, check for a firewall issue between the cluster and the external endpoint. 
     <pre class="pre"><code>curl &lt;host_name&gt;.&lt;domain&gt;</code></pre>

<br />


## Getting help and support
{: #ts_getting_help}

Still having issues with your cluster?
{: shortdesc}

-   To see whether {{site.data.keyword.Bluemix_notm}} is available, [check the {{site.data.keyword.Bluemix_notm}} status page ![External link icon](../icons/launch-glyph.svg "External link icon")](https://developer.ibm.com/bluemix/support/#status).
-   Post a question in the [{{site.data.keyword.containershort_notm}} Slack. ![External link icon](../icons/launch-glyph.svg "External link icon")](https://ibm-container-service.slack.com)
    If you are not using an IBM ID for your {{site.data.keyword.Bluemix_notm}} account, [request an invitation](https://bxcs-slack-invite.mybluemix.net/) to this Slack.
    {: tip}
-   Review the forums to see whether other users ran into the same issue. When you use the forums to ask a question, tag your question so that it is seen by the {{site.data.keyword.Bluemix_notm}} development teams.

    -   If you have technical questions about developing or deploying clusters or apps with {{site.data.keyword.containershort_notm}}, post your question on [Stack Overflow ![External link icon](../icons/launch-glyph.svg "External link icon")](https://stackoverflow.com/questions/tagged/ibm-cloud+containers) and tag your question with `ibm-cloud`, `kubernetes`, and `containers`.
    -   For questions about the service and getting started instructions, use the [IBM developerWorks dW Answers ![External link icon](../icons/launch-glyph.svg "External link icon")](https://developer.ibm.com/answers/topics/containers/?smartspace=bluemix) forum. Include the `ibm-cloud` and `containers` tags.
    See [Getting help](/docs/get-support/howtogetsupport.html#using-avatar) for more details about using the forums.

-   Contact IBM Support by opening a ticket. For information about opening an IBM support ticket, or about support levels and ticket severities, see [Contacting support](/docs/get-support/howtogetsupport.html#getting-customer-support).

{:tip}
When reporting an issue, include your cluster ID. To get your cluster ID, run `bx cs clusters`.


