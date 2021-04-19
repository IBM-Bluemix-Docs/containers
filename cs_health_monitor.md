---

copyright:
  years: 2014, 2021
lastupdated: "2021-04-15"

keywords: kubernetes, iks, logmet, logs, metrics, recovery, auto-recovery

subcollection: containers

---

{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}
{:android: data-hd-operatingsystem="android"}
{:api: .ph data-hd-interface='api'}
{:apikey: data-credential-placeholder='apikey'}
{:app_key: data-hd-keyref="app_key"}
{:app_name: data-hd-keyref="app_name"}
{:app_secret: data-hd-keyref="app_secret"}
{:app_url: data-hd-keyref="app_url"}
{:authenticated-content: .authenticated-content}
{:beta: .beta}
{:c#: data-hd-programlang="c#"}
{:cli: .ph data-hd-interface='cli'}
{:codeblock: .codeblock}
{:curl: .ph data-hd-programlang='curl'}
{:deprecated: .deprecated}
{:dotnet-standard: .ph data-hd-programlang='dotnet-standard'}
{:download: .download}
{:external: target="_blank" .external}
{:faq: data-hd-content-type='faq'}
{:fuzzybunny: .ph data-hd-programlang='fuzzybunny'}
{:generic: data-hd-operatingsystem="generic"}
{:generic: data-hd-programlang="generic"}
{:gif: data-image-type='gif'}
{:go: .ph data-hd-programlang='go'}
{:help: data-hd-content-type='help'}
{:hide-dashboard: .hide-dashboard}
{:hide-in-docs: .hide-in-docs}
{:important: .important}
{:ios: data-hd-operatingsystem="ios"}
{:java: .ph data-hd-programlang='java'}
{:java: data-hd-programlang="java"}
{:javascript: .ph data-hd-programlang='javascript'}
{:javascript: data-hd-programlang="javascript"}
{:new_window: target="_blank"}
{:note .note}
{:note: .note}
{:objectc data-hd-programlang="objectc"}
{:org_name: data-hd-keyref="org_name"}
{:php: data-hd-programlang="php"}
{:pre: .pre}
{:preview: .preview}
{:python: .ph data-hd-programlang='python'}
{:python: data-hd-programlang="python"}
{:route: data-hd-keyref="route"}
{:row-headers: .row-headers}
{:ruby: .ph data-hd-programlang='ruby'}
{:ruby: data-hd-programlang="ruby"}
{:runtime: architecture="runtime"}
{:runtimeIcon: .runtimeIcon}
{:runtimeIconList: .runtimeIconList}
{:runtimeLink: .runtimeLink}
{:runtimeTitle: .runtimeTitle}
{:screen: .screen}
{:script: data-hd-video='script'}
{:service: architecture="service"}
{:service_instance_name: data-hd-keyref="service_instance_name"}
{:service_name: data-hd-keyref="service_name"}
{:shortdesc: .shortdesc}
{:space_name: data-hd-keyref="space_name"}
{:step: data-tutorial-type='step'}
{:subsection: outputclass="subsection"}
{:support: data-reuse='support'}
{:swift: .ph data-hd-programlang='swift'}
{:swift: data-hd-programlang="swift"}
{:table: .aria-labeledby="caption"}
{:term: .term}
{:tip: .tip}
{:tooling-url: data-tooling-url-placeholder='tooling-url'}
{:troubleshoot: data-hd-content-type='troubleshoot'}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:tsSymptoms: .tsSymptoms}
{:tutorial: data-hd-content-type='tutorial'}
{:ui: .ph data-hd-interface='ui'}
{:unity: .ph data-hd-programlang='unity'}
{:url: data-credential-placeholder='url'}
{:user_ID: data-hd-keyref="user_ID"}
{:vbnet: .ph data-hd-programlang='vb.net'}
{:video: .video}


# Monitoring cluster health
{: #health-monitor}

Set up monitoring in {{site.data.keyword.containerlong}} to help you troubleshoot issues and improve the health and performance of your Kubernetes clusters and apps.
{: shortdesc}

Continuous monitoring and logging is the key to detecting attacks on your cluster and troubleshooting issues as they arise. By continuously monitoring your cluster, you're able to better understand your cluster capacity and the availability of resources that are available to your app. With this insight, you can prepare to protect your apps against downtime.

## Choosing a monitoring solution
{: #view_metrics}

Metrics help you monitor the health and performance of your clusters. You can use the standard Kubernetes and container runtime features to monitor the health of your clusters and apps.
{: shortdesc}

Every Kubernetes master is continuously monitored by IBM. {{site.data.keyword.containerlong_notm}} automatically scans every node where the Kubernetes master is deployed for vulnerabilities that are found in Kubernetes and OS-specific security fixes. If vulnerabilities are found, {{site.data.keyword.containerlong_notm}} automatically applies fixes and resolves vulnerabilities on behalf of the user to ensure master node protection. You are responsible for monitoring and analyzing the logs for the rest of your cluster components.

To avoid conflicts when using metrics services, be sure that clusters across resource groups and regions have unique names.
{: tip}

<dl>
  <dt>{{site.data.keyword.mon_full}}</dt>
  <dd>Gain operational visibility into the performance and health of your apps and your cluster by deploying a {{site.data.keyword.mon_short}} agent to your worker nodes. The agent collects pod and cluster metrics, and sends these metrics to {{site.data.keyword.mon_full_notm}}. For more information about {{site.data.keyword.mon_full_notm}}, see the [service documentation](/docs/monitoring?topic=monitoring-getting-started). To set up the {{site.data.keyword.mon_short}} agent in your cluster, see [Viewing cluster and app metrics with {{site.data.keyword.mon_full_notm}}](#monitoring).</dd>

  <dt>Kubernetes dashboard</dt>
  <dd>The Kubernetes dashboard is an administrative web interface where you can review the health of your worker nodes, find Kubernetes resources, deploy containerized apps, and troubleshoot apps with logging and monitoring information. For more information about how to access your Kubernetes dashboard, see [Launching the Kubernetes dashboard for {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-deploy_app#cli_dashboard).</dd>

</dl>

<br />

## Forwarding cluster and app metrics to {{site.data.keyword.mon_full_notm}}
{: #monitoring}

Use the {{site.data.keyword.containerlong_notm}} observability plug-in to create a monitoring configuration for {{site.data.keyword.mon_full_notm}} in your cluster, and use this monitoring configuration to automatically collect and forward metrics to {{site.data.keyword.mon_full_notm}}.
{: shortdesc}

With {{site.data.keyword.mon_full_notm}}, you can collects cluster and pod metrics, such as the CPU and memory usage of your worker nodes, incoming and outgoing HTTP traffic for your pods, and data about several infrastructure components. In addition, the agent can collect custom application metrics by using either a Prometheus-compatible scraper or a StatsD facade.

Considerations for using the {{site.data.keyword.containerlong_notm}} observability plug-in:
* You can have only one monitoring configuration for {{site.data.keyword.mon_full_notm}} in your cluster at a time. If you want to use a different {{site.data.keyword.mon_full_notm}} service instance to send metrics to, use the [`ibmcloud ob monitoring config replace`](/docs/containers?topic=containers-observability_cli#monitoring_config_replace) command.
* If you created a {{site.data.keyword.mon_short}} configuration in your cluster without using the {{site.data.keyword.containerlong_notm}} observability plug-in, you can use the [`ibmcloud ob monitoring agent discover`](/docs/containers?topic=containers-observability_cli#monitoring_agent_discover) command to make the configuration visible to the plug-in. Then, you can use the observability plug-in commands and functionality in the {{site.data.keyword.cloud_notm}} console to manage the configuration.

Before you begin:
- Verify that you are assigned the **Editor** platform access role and **Manager** server access role for {{site.data.keyword.mon_full_notm}}.
- Verify that you are assigned the **Administrator** platform access role and the **Manager** service access role for all Kubernetes namespaces in {{site.data.keyword.containerlong_notm}} to create the monitoring configuration. To view a monitoring configuration or launch the {{site.data.keyword.mon_short}} dashboard after the monitoring configuration is created, users must be assigned the **Viewer** platform access role and **Reader** service access role for the `ibm-observe` Kubernetes namespace in {{site.data.keyword.containerlong_notm}}.
- If you want to use the CLI to set up the monitoring configuration:
  - [Install the {{site.data.keyword.containerlong_notm}} observability CLI plug-in (`ibmcloud ob`)](/docs/containers?topic=containers-cs_cli_install#cs_cli_install_steps).
  - [Log in to your account. If applicable, target the appropriate resource group. Set the context for your cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

To set up a monitoring configuration for your cluster:

1. Create an [{{site.data.keyword.mon_full_notm}} service instance](/docs/monitoring?topic=monitoring-provision) and note the name of the instance. The service instance must belong to the same {{site.data.keyword.cloud_notm}} account where you created your cluster, but can be in a different resource group and {{site.data.keyword.cloud_notm}} region than your cluster.
2. Set up a monitoring configuration for your cluster. When you create the monitoring configuration, a Kubernetes namespace `ibm-observe` is created and a {{site.data.keyword.mon_short}} agent is deployed as a Kubernetes daemon set to all worker nodes in your cluster. This agent collects cluster and pod metrics, such as the worker node CPU and memory usage, or the amount incoming and outgoing network traffic to your pods.

   - **From the console: **
     1. From the [{{site.data.keyword.containerlong_notm}} console](https://cloud.ibm.com/kubernetes/clusters){: external}, select the cluster for which you want to create a {{site.data.keyword.mon_short}} configuration.
     2. On the cluster **Overview** page, click **Connect**.
     3. Select the region and the {{site.data.keyword.mon_full_notm}} service instance that you created earlier, and click **Connect**.

   - **From the CLI:**
     1. Create the {{site.data.keyword.mon_short}} configuration. When you create the {{site.data.keyword.mon_short}} configuration, the access key that was last added is retrieved automatically. If you want to use a different access key, add the `--sysdig-access-key <access_key>` option to the command.

        To use a different service access key after you created the monitoring configuration, use the [`ibmcloud ob monitoring config replace`](/docs/containers?topic=containers-observability_cli#monitoring_config_replace) command.
        {: tip}

        ```
        ibmcloud ob monitoring config create --cluster <cluster_name_or_ID> --instance <Monitoring_instance_name_or_ID>
        ```
        {: pre}

        Example output:
        ```
        Creating configuration...
        OK
        ```
        {: screen}

     2. Verify that the monitoring configuration was added to your cluster.
        ```
        ibmcloud ob monitoring config list --cluster <cluster_name_or_ID>
        ```
        {: pre}

        Example output:
        ```
        Listing configurations...

        OK
        Instance Name                Instance ID                            CRN   
        IBM Cloud Monitoring-aaa     1a111a1a-1111-11a1-a1aa-aaa11111a11a   crn:v1:prod:public:sysdig:us-south:a/a11111a1aaaaa11a111aa11a1aa1111a:1a111a1a-1111-11a1-a1aa-aaa11111a11a::  
        ```
        {: screen}

3. Optional: Verify that the {{site.data.keyword.mon_short}} agent was set up successfully.
   1. If you used the console to create the {{site.data.keyword.mon_short}} configuration, log in to your cluster. For more information, see [Log in to your account. If applicable, target the appropriate resource group. Set the context for your cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
   2. Verify that the daemon set for the {{site.data.keyword.mon_short}} agent was created and all instances are listed as `AVAILABLE`.
      ```
      kubectl get daemon sets -n ibm-observe
      ```
      {: pre}

      Example output:
      ```
      NAME           DESIRED   CURRENT   READY   UP-TO-DATE   AVAILABLE   NODE SELECTOR   AGE
      sysdig-agent   9         9         9       9            9           <none>          14m
      ```
      {: screen}

      The number of daemon set instances that are deployed equals the number of worker nodes in your cluster.

   3. Review the configmap that was created for your {{site.data.keyword.mon_short}} agent.
      ```
      kubectl describe configmap -n ibm-observe
      ```
      {: pre}

4. Access the metrics for your pods and cluster from the {{site.data.keyword.mon_short}} dashboard.
   1. From the [{{site.data.keyword.containerlong_notm}} console](https://cloud.ibm.com/kubernetes/clusters){: external}, select the cluster that you configured.
   2. On the cluster **Overview** page, click **Launch**. The {{site.data.keyword.mon_short}} dashboard opens.
   3. Review the pod and cluster metrics that the {{site.data.keyword.mon_short}} agent collected from your cluster. It might take a few minutes for your first metrics to show.

5. Review how you can work with the [{{site.data.keyword.mon_short}} dashboard](/docs/monitoring?topic=monitoring-panels) to further analyze your metrics.

## Viewing cluster states
{: #states}

Review the state of a Kubernetes cluster to get information about the availability and capacity of the cluster, and potential problems that might occur.
{: shortdesc}

To view information about a specific cluster, such as its zones, service endpoint URLs, Ingress subdomain, version, and owner, use the `ibmcloud ks cluster get --cluster <cluster_name_or_ID>` [command](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_get). Include the `--show-resources` flag to view more cluster resources such as add-ons for storage pods or subnet VLANs for public and private IPs.

You can review information about the overall cluster, the IBM-managed master, and your worker nodes. To troubleshoot your cluster and worker nodes, see [Troubleshooting clusters](/docs/containers?topic=containers-cs_troubleshoot#debug_clusters).

### Cluster states
{: #states_cluster}

You can view the current cluster state by running the `ibmcloud ks cluster ls` command and locating the **State** field.
{: shortdesc}
    <table summary="Every table row should be read left to right, with the cluster state in column one and a description in column two.">
    <caption>Cluster states</caption>
       <thead>
       <th>Cluster state</th>
       <th>Description</th>
       </thead>
       <tbody>
    <tr>
       <td>`Aborted`</td>
       <td>The deletion of the cluster is requested by the user before the Kubernetes master is deployed. After the deletion of the cluster is completed, the cluster is removed from your dashboard. If your cluster is stuck in this state for a long time, open an [{{site.data.keyword.cloud_notm}} support case](/docs/containers?topic=containers-get-help).</td>
       </tr>
     <tr>
         <td>`Critical`</td>
         <td>The Kubernetes master cannot be reached or all worker nodes in the cluster are down. If you enabled {{site.data.keyword.keymanagementservicelong_notm}} in your cluster, the {{site.data.keyword.keymanagementserviceshort}} container might fail to encrypt or decrypt your cluster secrets. If so, you can view an error with more information when you run `kubectl get secrets`.</td>
        </tr>
       <tr>
         <td>`Delete failed`</td>
         <td>The Kubernetes master or at least one worker node cannot be deleted. List worker nodes by running `ibmcloud ks worker ls --cluster <cluster_name_or_ID>`. If worker nodes are listed, see [Unable to create or delete worker nodes](/docs/containers?topic=containers-cs_troubleshoot_clusters#infra_errors). If no workers are listed, open an [{{site.data.keyword.cloud_notm}} support case](/docs/containers?topic=containers-get-help).</td>
       </tr>
       <tr>
         <td>`Deleted`</td>
         <td>The cluster is deleted but not yet removed from your dashboard. If your cluster is stuck in this state for a long time, open an [{{site.data.keyword.cloud_notm}} support case](/docs/containers?topic=containers-get-help). </td>
       </tr>
       <tr>
       <td>`Deleting`</td>
       <td>The cluster is being deleted and cluster infrastructure is being dismantled. You cannot access the cluster.  </td>
       </tr>
       <tr>
         <td>`Deploy failed`</td>
         <td>The deployment of the Kubernetes master could not be completed. You cannot resolve this state. Contact IBM Cloud support by opening an [{{site.data.keyword.cloud_notm}} support case](/docs/containers?topic=containers-get-help).</td>
       </tr>
         <tr>
           <td>`Deploying`</td>
           <td>The Kubernetes master is not fully deployed yet. You cannot access your cluster. Wait until your cluster is fully deployed to review the health of your cluster.</td>
          </tr>
          <tr>
           <td>`Normal`</td>
           <td>All worker nodes in a cluster are up and running. You can access the cluster and deploy apps to the cluster. This state is considered healthy and does not require an action from you.<p class="note">Although the worker nodes might be normal, other infrastructure resources, such as [networking](/docs/containers?topic=containers-cs_troubleshoot_network) and [storage](/docs/containers?topic=containers-cs_troubleshoot_storage), might still need attention. If you just created the cluster, some parts of the cluster that are used by other services such as Ingress secrets or registry image pull secrets, might still be in process.</p></td>
        </tr>
          <tr>
           <td>`Pending`</td>
           <td>The Kubernetes master is deployed. The worker nodes are being provisioned and are not available in the cluster yet. You can access the cluster, but you cannot deploy apps to the cluster.  </td>
         </tr>
       <tr>
         <td>`Requested`</td>
         <td>A request to create the cluster and order the infrastructure for the Kubernetes master and worker nodes is sent. When the deployment of the cluster starts, the cluster state changes to <code>Deploying</code>. If your cluster is stuck in the <code>Requested</code> state for a long time, open an [{{site.data.keyword.cloud_notm}} support case](/docs/containers?topic=containers-get-help). </td>
       </tr>
       <tr>
         <td>`Updating`</td>
         <td>The Kubernetes API server that runs in your Kubernetes master is being updated to a new Kubernetes API version. During the update, you cannot access or change the cluster. Worker nodes, apps, and resources that the user deployed are not modified and continue to run. Wait for the update to complete to review the health of your cluster. </td>
       </tr>
       <tr>
        <td>`Unsupported`</td>
        <td>The [Kubernetes version](/docs/containers?topic=containers-cs_versions#cs_versions) that the cluster runs is no longer supported. Your cluster's health is no longer actively monitored or reported. Additionally, you cannot add or reload worker nodes. To continue receiving important security updates and support, you must update your cluster. Review the [version update preparation actions](/docs/containers?topic=containers-cs_versions#prep-up), then [update your cluster](/docs/containers?topic=containers-update#update) to a supported Kubernetes version.</td>
       </tr>
        <tr>
           <td>`Warning`</td>
           <td><ul><li>At least one worker node in the cluster is not available, but other worker nodes are available and can take over the workload. Try to [reload](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload) the unavailable worker nodes.</li>
           <li>Your cluster has zero worker nodes, such as if you created a cluster without any worker nodes or manually removed all the worker nodes from the cluster. [Resize your worker pool](/docs/containers?topic=containers-add_workers#resize_pool) to add worker nodes to recover from a `Warning` state, and then [update the Calico node entries for your worker nodes](/docs/containers?topic=containers-cs_troubleshoot_clusters#zero_nodes_calico_failure).</li>
           <li>A control plane operation for your cluster failed. View the cluster in the console or run `ibmcloud ks cluster get --cluster <cluster_name_or_ID>` to [check the **Master Status** for further debugging](/docs/containers?topic=containers-cs_troubleshoot#debug_master).</li></ul></td>
        </tr>
       </tbody>
     </table>



### Master states
{: #states_master}

Your {{site.data.keyword.containerlong_notm}} includes an IBM-managed master with highly available replicas, automatic security patch updates applied for you, and automation in place to recover in case of an incident. You can check the health, status, and state of the cluster master by running `ibmcloud ks cluster get --cluster <cluster_name_or_ID>`.
{: shortdesc}

**Master Health**

The **Master Health** reflects the state of master components and notifies you if something needs your attention. The health might be one of the following:
*   `error`: The master is not operational. IBM is automatically notified and takes action to resolve this issue. You can continue monitoring the health until the master is `normal`. You can also [open an {{site.data.keyword.cloud_notm}} support case](/docs/containers?topic=containers-get-help).
*   `normal`: The master is operational and healthy. No action is required.
*   `unavailable`: The master might not be accessible, which means some actions such as resizing a worker pool are temporarily unavailable. IBM is automatically notified and takes action to resolve this issue. You can continue monitoring the health until the master is `normal`.
*   `unsupported`: The master runs an unsupported version of Kubernetes. You must [update your cluster](/docs/containers?topic=containers-update) to return the master to `normal` health.

**Master Status and State**

The **Master Status** provides details of what operation from the master state is in progress. The status includes a timestamp of how long the master has been in the same state, such as `Ready (1 month ago)`. The **Master State** reflects the lifecycle of possible operations that can be performed on the master, such as deploying, updating, and deleting. Each state is described in the following table.

|Master state|Description|
|--- |--- |
|`deployed`|The master is successfully deployed. Check the status to verify that the master is `Ready` or to see if an update is available.|
|`deploying`|The master is currently deploying. Wait for the state to become `deployed` before working with your cluster, such as adding worker nodes.|
|`deploy_failed`|The master failed to deploy. IBM Support is notified and works to resolve the issue. Check the **Master Status** field for more information, or wait for the state to become `deployed`.|
|`deleting`|The master is currently deleting because you deleted the cluster. You cannot undo a deletion. After the cluster is deleted, you can no longer check the master state because the cluster is completely removed.|
|`delete_failed`|The master failed to delete. IBM Support is notified and works to resolve the issue. You cannot resolve the issue by trying to delete the cluster again. Instead, check the **Master Status** field for more information, or wait for the cluster to delete. You can also [open an {{site.data.keyword.cloud_notm}} support case](/docs/containers?topic=containers-get-help).|
|`updating`|The master is updating its Kubernetes version. The update might be a patch update that is automatically applied, or a minor or major version that you applied by updating the cluster. During the update, your highly available master can continue processing requests, and your app workloads and worker nodes continue to run. After the master update is complete, you can [update your worker nodes](/docs/containers?topic=containers-update#worker_node).</br></br>If the update is unsuccessful, the master returns to a `deployed` state and continues running the previous version. IBM Support is notified and works to resolve the issue. You can check if the update failed in the **Master Status** field.|
|`update_cancelled`|The master update is canceled because the cluster was not in a healthy state at the time of the update. Your master remains in this state until your cluster is healthy and you manually update the master. To update the master, use the `ibmcloud ks cluster master update` [command](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_update).If you do not want to update the master to the default `major.minor` version during the update, include the `--version` flag and specify the latest patch version that is available for the `major.minor` version that you want, such as `1.19.9`. To list available versions, run `ibmcloud ks versions`.|
|`update_failed`|The master update failed. IBM Support is notified and works to resolve the issue. You can continue to monitor the health of the master until the master reaches a normal state. If the master remains in this state for more than 1 day, [open an {{site.data.keyword.cloud_notm}} support case](/docs/containers?topic=containers-get-help). IBM Support might identify other issues in your cluster that you must fix before the master can be updated.|
{: caption="Master states"}
{: summary="Table rows read from left to right, with the master state in column one and a description in column two."}



### Worker node states
{: #states_workers}

You can view the current worker node state by running the `ibmcloud ks worker ls --cluster <cluster_name_or_ID>` command and locating the **State** and **Status** fields.
{: shortdesc}
    <table summary="Every table row should be read left to right, with the cluster state in column one and a description in column two.">
    <caption>Worker node states</caption>
      <thead>
      <th>Worker node state</th>
      <th>Description</th>
      </thead>
      <tbody>
    <tr>
        <td>`Critical`</td>
        <td>A worker node can go into a Critical state for many reasons: <ul><li>You initiated a reboot for your worker node without cordoning and draining your worker node. Rebooting a worker node can cause data corruption in <code>containerd</code>, <code>kubelet</code>, <code>kube-proxy</code>, and <code>calico</code>. </li>
        <li>The pods that are deployed to your worker node do not use proper resource limits for [memory](https://kubernetes.io/docs/tasks/configure-pod-container/assign-memory-resource/){: external} and [CPU](https://kubernetes.io/docs/tasks/configure-pod-container/assign-cpu-resource/){: external}. If you set none or excessive resource limits, pods can consume all available resources, leaving no resources for other pods to run on this worker node. This overcommitment of workload causes the worker node to fail. <ol><li>List the pods that run on your worker node and review the CPU and memory usage, requests and limits. <pre class="codeblock"><code>kubectl describe node &lt;worker_private_IP&gt;</code></pre></li><li>For pods that consume a lot of memory and CPU resources, check if you set proper resource limits for memory and CPU. <pre class="codeblock"><code>kubectl get pods &lt;pod_name&gt; -n &lt;namespace&gt; -o json</code></pre></li><li>Optional: Remove the resource-intensive pods to free up compute resources on your worker node. <pre class="codeblock"><code>kubectl delete pod &lt;pod_name&gt;</code></pre><pre class="codeblock"><code>kubectl delete deployment &lt;deployment_name&gt;</code></pre></li></ol></li>
        <li><code>containerd</code>, <code>kubelet</code>, or <code>calico</code> went into an unrecoverable state after it ran hundreds or thousands of containers over time. </li>
        <li>You set up a Virtual Router Appliance for your worker node that went down and cut off the communication between your worker node and the Kubernetes master. </li><li> Current networking issues in {{site.data.keyword.containerlong_notm}} or IBM Cloud infrastructure that causes the communication between your worker node and the Kubernetes master to fail.</li>
        <li>Your worker node ran out of capacity. Check the <strong>Status</strong> of the worker node to see whether it shows <strong>Out of disk</strong> or <strong>Out of memory</strong>. If your worker node is out of capacity, consider to either reduce the workload on your worker node or add a worker node to your cluster to help load balance the workload.</li>
        <li>The device was powered off from the [{{site.data.keyword.cloud_notm}} console resource list ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/resources). Open the resource list and find your worker node ID in the **Devices** list. In the action menu, click **Power On**.</li></ul>
        In many cases, [reloading](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload) your worker node can solve the problem. When you reload your worker node, the latest [patch version](/docs/containers?topic=containers-cs_versions#version_types) is applied to your worker node. The major and minor version is not changed. Before you reload your worker node, make sure to cordon and drain your worker node to ensure that the existing pods are terminated gracefully and rescheduled onto remaining worker nodes. </br></br> If reloading the worker node does not resolve the issue, go to the next step to continue troubleshooting your worker node.<p class="tip">You can [configure health checks for your worker node and enable Autorecovery](/docs/containers?topic=containers-health-monitor#autorecovery). If Autorecovery detects an unhealthy worker node based on the configured checks, Autorecovery triggers a corrective action like rebooting a VPC worker node or reloading the operating system on a classic worker node. For more information about how Autorecovery works, see the [Autorecovery blog ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/cloud/blog/autorecovery-utilizes-consistent-hashing-high-availability).</p>
        </td>
       </tr>
       <tr>
       <td>`Deleting`</td>
       <td>You requested to delete the worker node, possibly as part of resizing a worker pool or autoscaling the cluster. Other operations cannot be issued against the worker node while the worker node deletes. You cannot reverse the deletion process. When the deletion process completes, you are no longer billed for the worker nodes.</td>
       </tr>
       <tr>
       <td>`Deleted`</td>
       <td>Your worker node is deleted, and no longer is listed in the cluster or billed. This state cannot be undone. Any data that was stored only on the worker node, such as container images, are also deleted.</td>
       </tr>
       <tr>
       <td>`Deployed`</td>
       <td>Updates are successfully deployed to your worker node. After updates are deployed, {{site.data.keyword.containerlong_notm}} starts a health check on the worker node. After the health check is successful, the worker node goes into a <code>Normal</code> state. Worker nodes in a <code>Deployed</code> state usually are ready to receive workloads, which you can check by running <code>kubectl get nodes</code> and confirming that the state shows <code>Normal</code>. </td>
       </tr>
        <tr>
          <td>`Deploying`</td>
          <td>When you update the Kubernetes version of your worker node, your worker node is redeployed to install the updates. If you reload or reboot your worker node, the worker node is redeployed to automatically install the latest patch version. If your worker node is stuck in this state for a long time, check whether a problem occurred during the deployment. </td>
       </tr>
       <tr>
         <td>`Deploy_failed`</td>
         <td>Your worker node could not be deployed. List the details for the worker node to find the details for the failure by running `ibmcloud ks worker get --cluster <cluster_name_or_id> --worker <worker_node_id>`.</td>
       </tr>
          <tr>
          <td>`Normal`</td>
          <td>Your worker node is fully provisioned and ready to be used in the cluster. This state is considered healthy and does not require an action from the user. **Note**: Although the worker nodes might be normal, other infrastructure resources, such as [networking](/docs/containers?topic=containers-cs_troubleshoot_network) and [storage](/docs/containers?topic=containers-cs_troubleshoot_storage), might still need attention.</td>
       </tr>
     <tr>
          <td>`Provisioned`</td>
          <td>Your worker node completed provisioning and is part of the cluster. Billing for the worker node begins. The worker node state soon reports a regular health state and status, such as `normal` and `ready`.</td>
        </tr>
     <tr>
     <tr>
          <td>`Provisioning`</td>
          <td>Your worker node is being provisioned and is not available in the cluster yet. You can monitor the provisioning process in the <strong>Status</strong> column of your CLI output. If your worker node is stuck in this state for a long time, check whether a problem occurred during the provisioning.</td>
        </tr>
     <tr>
          <td>`Provision pending`</td>
          <td>Another process is completing before the worker node provisioning process starts. You can monitor the other process that must complete first in the <strong>Status</strong> column of your CLI output. For example, in VPC clusters, the `Pending security group creation` indicates that the security group for your worker nodes is creating first before the worker nodes can be provisioned. If your worker node is stuck in this state for a long time, check whether a problem occurred during the other process.</td>
     </tr>
        <tr>
          <td>`Provision_failed`</td>
          <td>Your worker node could not be provisioned. List the details for the worker node to find the details for the failure by running `ibmcloud ks worker get --cluster <cluster_name_or_id> --worker <worker_node_id>`.</td>
        </tr>
     <tr>
          <td>`Reloading`</td>
          <td>Your worker node is being reloaded and is not available in the cluster. You can monitor the reloading process in the <strong>Status</strong> column of your CLI output. If your worker node is stuck in this state for a long time, check whether a problem occurred during the reloading.</td>
         </tr>
         <tr>
          <td>`Reloading_failed`</td>
          <td>Your worker node could not be reloaded. List the details for the worker node to find the details for the failure by running `ibmcloud ks worker get --cluster <cluster_name_or_id> --worker <worker_node_id>`.</td>
        </tr>
        <tr>
          <td>`Reload_pending`</td>
          <td>A request to reload or to update the Kubernetes version of your worker node is sent. When the worker node is being reloaded, the state changes to <code>Reloading</code>. </td>
        </tr>
        <tr>
         <td>`Unknown`</td>
         <td>The Kubernetes master is not reachable for one of the following reasons:<ul><li>You requested an update of your Kubernetes master. The state of the worker node cannot be retrieved during the update. If the worker node remains in this state for an extended period of time even after the Kubernetes master is successfully updated, try to [reload](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload) the worker node.</li><li>You might have another firewall that is protecting your worker nodes, or changed firewall settings recently. {{site.data.keyword.containerlong_notm}} requires certain IP addresses and ports to be opened to allow communication from the worker node to the Kubernetes master and vice versa. For more information, see [Firewall prevents worker nodes from connecting](/docs/containers?topic=containers-firewall#vyatta_firewall).</li><li>The Kubernetes master is down. Contact {{site.data.keyword.cloud_notm}} support by opening an [{{site.data.keyword.cloud_notm}} support case](/docs/containers?topic=containers-get-help).</li></ul></td>
    </tr>
       <tr>
          <td>`Warning`</td>
          <td>Your worker node is reaching the limit for memory or disk space. You can either reduce work load on your worker node or add a worker node to your cluster to help load balance the work load.</td>
    </tr>
      </tbody>
    </table>



## Monitoring worker node health in with Autorecovery
{: #autorecovery}

The Autorecovery system uses various checks to query worker node health status. If Autorecovery detects an unhealthy worker node based on the configured checks, Autorecovery triggers a corrective action like rebooting a VPC worker node or reloading the operating system in a classic worker node. Only one worker node undergoes a corrective action at a time. The worker node must successfully complete the corrective action before any other worker node undergoes a corrective action. For more information, see this [Autorecovery blog post](https://www.ibm.com/cloud/blog/autorecovery-utilizes-consistent-hashing-high-availability){: external}.
{: shortdesc}

Autorecovery requires at least one healthy worker node to function properly. Configure Autorecovery with active checks only in clusters with two or more worker nodes.
{: note}

Before you begin:
- Ensure that you have the following [{{site.data.keyword.cloud_notm}} IAM roles](/docs/containers?topic=containers-users#platform):
    - **Administrator** platform access role for the cluster
    - **Writer** or **Manager** service access role for the `kube-system` namespace
- [Log in to your account. If applicable, target the appropriate resource group. Set the context for your cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

To configure Autorecovery:

1.  [Follow the instructions](/docs/containers?topic=containers-helm#install_v3) to install the Helm version 3 client on your local machine.

2. Create a configuration map file that defines your checks in JSON format. For example, the following YAML file defines three checks: an HTTP check and two Kubernetes API server checks. Refer to the tables following the example YAML file for information about the three kinds of checks and information about the individual components of the checks.<p class="tip">*Define each check as a unique key in the `data` section of the configuration map.</p>

   ```yaml
   kind: ConfigMap
   apiVersion: v1
   metadata:
     name: ibm-worker-recovery-checks
     namespace: kube-system
   data:
     checknode.json: |
       {
         "Check":"KUBEAPI",
         "Resource":"NODE",
         "FailureThreshold":3,
         "CorrectiveAction":"RELOAD",
         "CooloffSeconds":1800,
         "IntervalSeconds":180,
         "TimeoutSeconds":10,
         "Enabled":true
       }
     checkpod.json: |
       {
         "Check":"KUBEAPI",
         "Resource":"POD",
         "PodFailureThresholdPercent":50,
         "FailureThreshold":3,
         "CorrectiveAction":"RELOAD",
         "CooloffSeconds":1800,
         "IntervalSeconds":180,
         "TimeoutSeconds":10,
         "Enabled":true
       }
     checkhttp.json: |
       {
         "Check":"HTTP",
         "FailureThreshold":3,
         "CorrectiveAction":"REBOOT",
         "CooloffSeconds":1800,
         "IntervalSeconds":180,
         "TimeoutSeconds":10,
         "Port":80,
         "ExpectedStatus":200,
         "Route":"/myhealth",
         "Enabled":false
       }
   ```
   {:codeblock}

   <table summary="The columns are read from left to right. The first column has the parameter of the YAML file. The second column describes the parameter.">
   <caption>Understanding the configmap components</caption>
   <thead>
   <col width="25%">
   <th>Component</th>
   <th>Description</th>
   </thead>
   <tbody>
   <tr>
   <td><code>name</code></td>
   <td>The configuration name <code>ibm-worker-recovery-checks</code> is a constant and cannot be changed.</td>
   </tr>
   <tr>
   <td><code>namespace</code></td>
   <td>The <code>kube-system</code> namespace is a constant and cannot be changed.</td>
   </tr>
   <tr>
   <td><code>checknode.json</code></td>
   <td>Defines a Kubernetes API node check that checks whether each worker node is in the <code>Ready</code> state. The check for a specific worker node counts as a failure if the worker node is not in the <code>Ready</code> state. The check in the example YAML runs every 3 minutes. If it fails three consecutive times, the worker node is reloaded. This action is equivalent to running <code>ibmcloud ks worker reload</code>.<br></br>The node check is enabled until you set the <b>Enabled</b> field to <code>false</code> or remove the check.<p class="note">Reloading is supported only for worker nodes on classic infrastructure.</p></td>
   </tr>
   <tr>
   <td><code>checkpod.json</code></td>
   <td>
   Defines a Kubernetes API pod check that checks the total percentage of <code>NotReady</code> pods on a worker node based on the total pods that are assigned to that worker node. The check for a specific worker node counts as a failure if the total percentage of <code>NotReady</code> pods is greater than the defined <code>PodFailureThresholdPercent</code>. The check in the example YAML runs every 3 minutes. If it fails three consecutive times, the worker node is reloaded. This action is equivalent to running <code>ibmcloud ks worker reload</code>. For example, the default <code>PodFailureThresholdPercent</code> is 50%. If the percentage of <code>NotReady</code> pods is greater than 50% three consecutive times, the worker node is reloaded. <br></br>By default, pods in all namespaces are checked. To restrict the check to only pods in a specified namespace, add the <code>Namespace</code> field to the check. The pod check is enabled until you set the <b>Enabled</b> field to <code>false</code> or remove the check.<p class="note">Reloading is supported only for worker nodes on classic infrastructure.</p>
   </td>
   </tr>
   <tr>
   <td><code>checkhttp.json</code></td>
   <td>Defines an HTTP check that checks if an HTTP server that runs on your worker node is healthy. To use this check, you must deploy an HTTP server on every worker node in your cluster by using a [daemon set ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/). You must implement a health check that is available at the <code>/myhealth</code> path and that can verify whether your HTTP server is healthy. You can define other paths by changing the <code>Route</code> parameter. If the HTTP server is healthy, you must return the HTTP response code that is defined in <code>ExpectedStatus</code>. The HTTP server must be configured to listen on the private IP address of the worker node. You can find the private IP address by running <code>kubectl get nodes</code>.<br></br>
   For example, consider two nodes in a cluster that have the private IP addresses 10.10.10.1 and 10.10.10.2. In this example, two routes are checked for a 200 HTTP response: <code>http://10.10.10.1:80/myhealth</code> and <code>http://10.10.10.2:80/myhealth</code>.
   The check in the example YAML runs every 3 minutes. If it fails three consecutive times, the worker node is rebooted. This action is equivalent to running <code>ibmcloud ks worker reboot</code>.<br></br>The HTTP check is disabled until you set the <b>Enabled</b> field to <code>true</code>.</td>
   </tr>
   </tbody>
   </table>

   <table summary="The columns are read from left to right. The first column has the component of the check. The second column describes the component.">
   <caption>Understanding the individual components of checks</caption>
   <thead>
   <col width="25%">
   <th>Component</th>
   <th>Description</th>
   </thead>
   <tbody>
   <tr>
   <td><code>Check</code></td>
   <td>Enter the type of check that you want Autorecovery to use. <ul><li><code>HTTP</code>: Autorecovery calls HTTP servers that run on each node to determine whether the nodes are running properly.</li><li><code>KUBEAPI</code>: Autorecovery calls the Kubernetes API server and reads the health status data reported by the worker nodes.</li></ul></td>
   </tr>
   <tr>
   <td><code>Resource</code></td>
   <td>When the check type is <code>KUBEAPI</code>, enter the type of resource that you want Autorecovery to check. Accepted values are <code>NODE</code> or <code>POD</code>.</td>
   </tr>
   <tr>
   <td><code>FailureThreshold</code></td>
   <td>Enter the threshold for the number of consecutive failed checks. When this threshold is met, Autorecovery triggers the specified corrective action. For example, if the value is 3 and Autorecovery fails a configured check three consecutive times, Autorecovery triggers the corrective action that is associated with the check.</td>
   </tr>
   <tr>
   <td><code>PodFailureThresholdPercent</code></td>
   <td>When the resource type is <code>POD</code>, enter the threshold for the percentage of pods on a worker node that can be in a [<strong><code>NotReady</code></strong> ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/#define-readiness-probes) state. This percentage is based on the total number of pods that are scheduled to a worker node. When a check determines that the percentage of unhealthy pods is greater than the threshold, the check counts as one failure.</td>
   </tr>
   <tr>
   <td><code>CorrectiveAction</code></td>
   <td>Enter the action to run when the failure threshold is met. A corrective action runs only while no other workers are being repaired and when this worker node is not in a cool-off period from a previous action. <ul><li><code>REBOOT</code>: Reboots the worker node.</li><li><code>RELOAD</code>: Reloads all of the necessary configurations for the worker node from a clean OS.</li></ul></td>
   </tr>
   <tr>
   <td><code>CooloffSeconds</code></td>
   <td>Enter the number of seconds Autorecovery must wait to issue another corrective action for a node that was already issued a corrective action. The cool off period starts at the time a corrective action is issued.</td>
   </tr>
   <tr>
   <td><code>IntervalSeconds</code></td>
   <td>Enter the number of seconds in between consecutive checks. For example, if the value is 180, Autorecovery runs the check on each node every 3 minutes.</td>
   </tr>
   <tr>
   <td><code>TimeoutSeconds</code></td>
   <td>Enter the maximum number of seconds that a check call to the database takes before Autorecovery terminates the call operation. The value for <code>TimeoutSeconds</code> must be less than the value for <code>IntervalSeconds</code>.</td>
   </tr>
   <tr>
   <td><code>Port</code></td>
   <td>When the check type is <code>HTTP</code>, enter the port that the HTTP server must bind to on the worker nodes. This port must be exposed on the IP of every worker node in the cluster. Autorecovery requires a constant port number across all nodes for checking servers. Use [daemon sets ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) when you deploy a custom server into a cluster.</td>
   </tr>
   <tr>
   <td><code>ExpectedStatus</code></td>
   <td>When the check type is <code>HTTP</code>, enter the HTTP server status that you expect to be returned from the check. For example, a value of 200 indicates that you expect an <code>OK</code> response from the server.</td>
   </tr>
   <tr>
   <td><code>Route</code></td>
   <td>When the check type is <code>HTTP</code>, enter the path that is requested from the HTTP server. This value is typically the metrics path for the server that runs on all of the worker nodes.</td>
   </tr>
   <tr>
   <td><code>Enabled</code></td>
   <td>Enter <code>true</code> to enable the check or <code>false</code> to disable the check.</td>
   </tr>
   <tr>
   <td><code>Namespace</code></td>
   <td> Optional: To restrict <code>checkpod.json</code> to checking only pods in one namespace, add the <code>Namespace</code> field and enter the namespace.</td>
   </tr>
   </tbody>
   </table>

3. Create the configuration map in your cluster.
    ```
    kubectl apply -f ibm-worker-recovery-checks.yaml
    ```
    {: pre}

4. Verify that you created the configuration map with the name `ibm-worker-recovery-checks` in the `kube-system` namespace with the proper checks.
    ```
    kubectl -n kube-system get cm ibm-worker-recovery-checks -o yaml
    ```
    {: pre}

5. Deploy Autorecovery into your cluster by installing the `ibm-worker-recovery` Helm chart.
    ```
    helm install ibm-worker-recovery iks-charts/ibm-worker-recovery --namespace kube-system
    ```
    {: pre}

6. After a few minutes, you can check the `Events` section in the output of the following command to see activity on the Autorecovery deployment.
    ```
    kubectl -n kube-system describe deployment ibm-worker-recovery
    ```
    {: pre}

7. If you do not see activity on the Autorecovery deployment, you can check the Helm deployment by running the tests that are included in the Autorecovery chart definition.
    ```
    helm test ibm-worker-recovery -n kube-system
    ```
    {: pre}
