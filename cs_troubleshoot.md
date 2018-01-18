---

copyright:
  years: 2014, 2018
lastupdated: "2018-01-09"

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


# Troubleshooting clusters
{: #cs_troubleshoot}

As you use {{site.data.keyword.containershort_notm}}, consider these techniques for troubleshooting and getting help. You can also check the [status of the {{site.data.keyword.Bluemix_notm}} system ![External link icon](../icons/launch-glyph.svg "External link icon")](https://developer.ibm.com/bluemix/support/#status).

You can take some general steps to ensure that your clusters are up-to-date:
- [Reboot your worker nodes](cs_cli_reference.html#cs_worker_reboot) regularly to ensure the installation of updates and security patches that IBM automatically deploys to the operating system
- Update your cluster to [the latest default version of Kubernetes](cs_versions.html) for {{site.data.keyword.containershort_notm}}

{: shortdesc}

<br />




## Debugging clusters
{: #debug_clusters}

Review the options to debug your clusters and find the root causes for failures.

1.  List your cluster and find the `State` of the cluster.

  ```
  bx cs clusters
  ```
  {: pre}

2.  Review the `State` of your cluster.

  <table summary="Every table row should be read left to right, with the cluster state in column one and a description in column two.">
    <thead>
    <th>Cluster state</th>
    <th>Description</th>
    </thead>
    <tbody>
      <tr>
        <td>Deploying</td>
        <td>The Kubernetes master is not fully deployed yet. You cannot access your cluster.</td>
       </tr>
       <tr>
        <td>Pending</td>
        <td>The Kubernetes master is deployed. The worker nodes are being provisioned and are not available in the cluster yet. You can access the cluster, but you cannot deploy apps to the cluster.</td>
      </tr>
      <tr>
        <td>Normal</td>
        <td>All worker nodes in a cluster are up and running. You can access the cluster and deploy apps to the cluster.</td>
     </tr>
     <tr>
        <td>Warning</td>
        <td>At least one worker node in the cluster is not available, but other worker nodes are available and can take over the workload.</td>
     </tr>
     <tr>
      <td>Critical</td>
      <td>The Kubernetes master cannot be reached or all worker nodes in the cluster are down.</td>
     </tr>
    </tbody>
  </table>

3.  If your cluster is in a **Warning** or **Critical** state, or is stuck in the **Pending** state for a long time, review the state of your worker nodes. If your cluster is in a **Deploying** state, wait until your cluster is fully deployed to review the health of your cluster. Clusters in a **Normal** state are considered healthy and do not require an action at the moment.

  ```
  bx cs workers <cluster_name_or_id>
  ```
  {: pre}

  <table summary="Every table row should be read left to right, with the cluster state in column one and a description in column two.">
    <thead>
    <th>Worker node state</th>
    <th>Description</th>
    </thead>
    <tbody>
      <tr>
       <td>Unknown</td>
       <td>The Kubernetes master is not reachable for one of the following reasons:<ul><li>You requested an update of your Kubernetes master. The state of the worker node cannot be retrieved during the update.</li><li>You might have an additional firewall that is protecting your worker nodes, or changed firewall settings recently. {{site.data.keyword.containershort_notm}} requires certain IP addresses and ports to be opened to allow communication from the worker node to the Kubernetes master and vice versa. For more information, see [Firewall prevents worker nodes from connecting](#cs_firewall).</li><li>The Kubernetes master is down. Contact {{site.data.keyword.Bluemix_notm}} support by opening an [{{site.data.keyword.Bluemix_notm}} support ticket](/docs/support/index.html#contacting-support).</li></ul></td>
      </tr>
      <tr>
        <td>Provisioning</td>
        <td>Your worker node is being provisioned and is not available in the cluster yet. You can monitor the provisioning process in the **Status** column of your CLI output. If your worker node is stuck in this state for a long time, and you cannot see any progress in the **Status** column, continue with the next step to see if a problem occurred during the provisioning.</td>
      </tr>
      <tr>
        <td>Provision_failed</td>
        <td>Your worker node could not be provisioned. Continue with the next step to find the details for the failure.</td>
      </tr>
      <tr>
        <td>Reloading</td>
        <td>Your worker node is being reloaded and is not available in the cluster. You can monitor the reloading process in the **Status** column of your CLI output. If your worker node is stuck in this state for a long time, and you cannot see any progress in the **Status** column, continue with the next step to see if a problem occurred during the reloading.</td>
       </tr>
       <tr>
        <td>Reloading_failed</td>
        <td>Your worker node could not be reloaded. Continue with the next step to find the details for the failure.</td>
      </tr>
      <tr>
        <td>Normal</td>
        <td>Your worker node is fully provisioned and ready to be used in the cluster.</td>
     </tr>
     <tr>
        <td>Warning</td>
        <td>Your worker node is reaching the limit for memory or disk space.</td>
     </tr>
     <tr>
      <td>Critical</td>
      <td>Your worker node ran out of disk space.</td>
     </tr>
    </tbody>
  </table>

4.  List the details for the worker node.

  ```
  bx cs worker-get [<cluster_name_or_id>] <worker_node_id>
  ```
  {: pre}

5.  Review common error messages and learn how to resolve them.

  <table>
    <thead>
    <th>Error message</th>
    <th>Description and resolution
    </thead>
    <tbody>
      <tr>
        <td>{{site.data.keyword.Bluemix_notm}} Infrastructure Exception: Your account is currently prohibited from ordering 'Computing Instances'.</td>
        <td>Your IBM Cloud infrastructure (SoftLayer) account might be restricted from ordering compute resources. Contact {{site.data.keyword.Bluemix_notm}} support by opening an [{{site.data.keyword.Bluemix_notm}} support ticket](/docs/support/index.html#contacting-support).</td>
      </tr>
      <tr>
        <td>{{site.data.keyword.Bluemix_notm}} Infrastructure Exception: Could not place order. There are insufficient resources behind router 'router_name' to fulfill the request for the following guests: 'worker_id'.</td>
        <td>The VLAN that you selected is associated with a pod in the data center that has insufficient space to provision your worker node. You can choose between the following options:<ul><li>Use a different data center to provision your worker node. Run <code>bx cs locations</code> to list available data center.<li>If you have an existing public and private VLAN pair that is associated with another pod in the data center, use this VLAN pair instead.<li>Contact {{site.data.keyword.Bluemix_notm}} support by opening an [{{site.data.keyword.Bluemix_notm}} support ticket](/docs/support/index.html#contacting-support).</ul></td>
      </tr>
      <tr>
        <td>{{site.data.keyword.Bluemix_notm}} Infrastructure Exception: Could not obtain network VLAN with id: &lt;vlan id&gt;.</td>
        <td>Your worker node could not be provisioned because the selected VLAN ID could not be found for one of the following reasons:<ul><li>You might have specified the VLAN number instead of the VLAN ID. The VLAN number is 3 or 4 digits long, whereas the VLAN ID is 7 digits long. Run <code>bx cs vlans &lt;location&gt;</code> to retrieve the VLAN ID.<li>The VLAN ID might not be associated with the IBM Cloud infrastructure (SoftLayer) account that you use. Run <code>bx cs vlans &lt;location&gt;</code> to list available VLAN IDs for your account. To change the IBM Cloud infrastructure (SoftLayer) account, see [bx cs credentials-set](cs_cli_reference.html#cs_credentials_set). </ul></td>
      </tr>
      <tr>
        <td>SoftLayer_Exception_Order_InvalidLocation: The location provided for this order is invalid. (HTTP 500)</td>
        <td>Your IBM Cloud infrastructure (SoftLayer) is not set up to order compute resources in the selected data center. Contact [{{site.data.keyword.Bluemix_notm}} support](/docs/support/index.html#contacting-support) to verify that you account is setup correctly.</td>
       </tr>
       <tr>
        <td>{{site.data.keyword.Bluemix_notm}} Infrastructure Exception: The user does not have the necessary {{site.data.keyword.Bluemix_notm}} Infrastructure permissions to add servers

        </br></br>
        {{site.data.keyword.Bluemix_notm}} Infrastructure Exception: 'Item' must be ordered with permission.</td>
        <td>You might not have the required permissions to provision a worker node from the IBM Cloud infrastructure (SoftLayer) portfolio. See [Configure access to the IBM Cloud infrastructure (SoftLayer) portfolio to create standard Kubernetes clusters](cs_infrastructure.html#unify_accounts).</td>
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

2. [Check if the containers are stuck in the ContainerCreating state](#stuck_creating_state).

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


## Unable to connect to your infrastructure account
{: #cs_credentials}

{: tsSymptoms}
When you create a new Kubernetes cluster, you receive the following message.

```
We were unable to connect to your IBM Cloud infrastructure (SoftLayer) account.
Creating a standard cluster requires that you have either a Pay-As-You-Go account
that is linked to an IBM Cloud infrastructure (SoftLayer) account term or that you have used the {{site.data.keyword.Bluemix_notm}}
Container Service CLI to set your {{site.data.keyword.Bluemix_notm}} Infrastructure API keys.
```
{: screen}

{: tsCauses}
Users with an unlinked {{site.data.keyword.Bluemix_notm}} account must create a new Pay-As-You-Go account or manually add IBM Cloud infrastructure (SoftLayer) API keys using the {{site.data.keyword.Bluemix_notm}} CLI.

{: tsResolve}
To add credentials your {{site.data.keyword.Bluemix_notm}} account:

1.  Contact your IBM Cloud infrastructure (SoftLayer) administrator to get your IBM Cloud infrastructure (SoftLayer) user name and API key.

    **Note:** The IBM Cloud infrastructure (SoftLayer) account that you use must be set up with SuperUser permissions to successfully create standard clusters.

2.  Add the credentials.

  ```
  bx cs credentials-set --infrastructure-username <username> --infrastructure-api-key <api_key>
  ```
  {: pre}

3.  Create a standard cluster.

  ```
  bx cs cluster-create --location dal10 --public-vlan my_public_vlan_id --private-vlan my_private_vlan_id --machine-type u2c.2x4 --name my_cluster --hardware shared --workers 2
  ```
  {: pre}

<br />


## Firewall prevents running CLI commands
{: #ts_firewall_clis}

{: tsSymptoms}
When you run `bx`, `kubectl`, or `calicoctl` commands from the CLI, they fail.

{: tsCauses}
You might have corporate network policies that prevent access from your local system to public endpoints via proxies or firewalls.

{: tsResolve}
[Allow TCP access for the CLI commands to work](cs_firewall.html#firewall). This task requires an [Administrator access policy](cs_users.html#access_policies). Verify your current [access policy](cs_users.html#infra_access).


## Firewall prevents cluster from connecting to resources
{: #cs_firewall}

{: tsSymptoms}
When the worker nodes are not able to connect, you might see a variety of different symptoms. You might see one of the following messages when kubectl proxy fails or you try to access a service in your cluster and the connection fails.

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

If you run kubectl exec, attach, or logs, you might see the following message.

  ```
  Error from server: error dialing backend: dial tcp XXX.XXX.XXX:10250: getsockopt: connection timed out
  ```
  {: screen}

If kubectl proxy succeeds, but the dashboard is not available, you might see the following message.

  ```
  timeout on 172.xxx.xxx.xxx
  ```
  {: screen}



{: tsCauses}
You might have an additional firewall set up or customized your existing firewall settings in your IBM Cloud infrastructure (SoftLayer) account. {{site.data.keyword.containershort_notm}} requires certain IP addresses and ports to be opened to allow communication from the worker node to the Kubernetes master and vice versa. Another reason might be that the worker nodes are stuck in a reloading loop.

{: tsResolve}
[Allow the cluster to access infrastructure resources and other services](cs_firewall.html#firewall_outbound). This task requires an [Administrator access policy](cs_users.html#access_policies). Verify your current [access policy](cs_users.html#infra_access).

<br />



## Accessing your worker node with SSH fails
{: #cs_ssh_worker}

{: tsSymptoms}
You cannot access your worker node by using a SSH connection.

{: tsCauses}
SSH via password is disabled on the worker nodes.

{: tsResolve}
Use [DaemonSets ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) for anything you must run on every node or jobs for any one-time actions you must execute.

<br />



## Binding a service to a cluster results in same name error
{: #cs_duplicate_services}

{: tsSymptoms}
When you run `bx cs cluster-service-bind <cluster_name> <namespace> <service_instance_name>` you see the following message.

```
Multiple services with the same name were found.
Run 'bx service list' to view available Bluemix service instances...
```
{: screen}

{: tsCauses}
Multiple service instances might have the same name in different regions.

{: tsResolve}
Use the service GUID instead of the service instance name in the `bx cs cluster-service-bind` command.

1. [Log in to the region that includes the service instance to bind.](cs_regions.html#bluemix_regions)

2. Get the GUID for the service instance.
  ```
  bx service show <service_instance_name> --guid
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
  bx cs cluster-service-bind <cluster_name> <namespace> <service_instance_GUID>
  ```
  {: pre}

<br />



## After updating or reloading a worker node, duplicate nodes and pods appear
{: #cs_duplicate_nodes}

{: tsSymptoms}
When you run `kubectl get nodes` you see duplicate worker nodes with the status **NotReady**. The worker nodes with **NotReady** have public IP addresses, while the worker nodes with **Ready** have private IP addresses.

{: tsCauses}
Older clusters had worker nodes listed by the cluster's public IP address. Now, worker nodes are listed by the cluster's private IP address. When you reload or update a node, the IP address is changed, but the reference to the public IP address remains.

{: tsResolve}
There are no service disruptions due to these duplicates, but you should remove the old worker node references from the API server.

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
If you delete a worker node from your cluster and then add a worker node, it is possible for the new worker node to be assigned the private IP address of the deleted worker node. Calico uses this private IP address as a tag and continues to try to reach the deleted node.

{: tsResolve}
Manually update the reference of the private IP address to point to the correct node.

1.  Confirm that you have two worker nodes with the same **Private IP** address. Note the **Private IP** and **ID** of the deleted worker.

  ```
  bx cs workers <CLUSTER_NAME>
  ```
  {: pre}

  ```
  ID                                                 Public IP       Private IP       Machine Type   State     Status
  kube-dal10-cr9b7371a7fcbe46d08e04f046d5e6d8b4-w1   192.0.2.0.12   203.0.113.144   b2c.4x16       normal    Ready
  kube-dal10-cr9b7371a7fcbe46d08e04f046d5e6d8b4-w2   192.0.2.0.16   203.0.113.144   b2c.4x16       deleted    -
  ```
  {: screen}

2.  Install the [Calico CLI](cs_network_policy.html#adding_network_policies).
3.  List the available worker nodes in Calico. Replace <path_to_file> with the local path to the Calico configuration file.

  ```
  calicoctl get nodes --config=<path_to_file>/calicoctl.cfg
  ```
  {: pre}

  ```
  NAME
  kube-dal10-cr9b7371a7fcbe46d08e04f046d5e6d8b4-w1
  kube-dal10-cr9b7371a7fcbe46d08e04f046d5e6d8b4-w2
  ```
  {: screen}

4.  Delete the duplicate worker node in Calico. Replace NODE_ID with the worker node ID.

  ```
  calicoctl delete node NODE_ID --config=<path_to_file>/calicoctl.cfg
  ```
  {: pre}

5.  Reboot the worker node that was not deleted.

  ```
  bx cs worker-reboot CLUSTER_ID NODE_ID
  ```
  {: pre}


The deleted node is no longer listed in Calico.

<br />




## Pods remain in pending state
{: #cs_pods_pending}

{: tsSymptoms}
When you run `kubectl get pods`, you can see pods that remain in a **Pending** state.

{: tsCauses}
If you just created the Kubernetes cluster, the worker nodes might still be configuring. If this cluster is an existing one, you might not have enough capacity in your cluster to deploy the pod.

{: tsResolve}
This task requires an [Administrator access policy](cs_users.html#access_policies). Verify your current [access policy](cs_users.html#infra_access).

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

4.  If you don't have enough capacity in your cluster, add another worker node to your cluster.

  ```
  bx cs worker-add <cluster name or id> 1
  ```
  {: pre}

5.  If your pods still stay in a **pending** state after the worker node is fully deployed, review the [Kubernetes documentation ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/tasks/debug-application-cluster/debug-pod-replication-controller/#my-pod-stays-pending) to further troubleshoot the pending state of your pod.

<br />




## Pods are stuck in the creating state
{: #stuck_creating_state}

{: tsSymptoms}
When you run `kubectl get pods -o wide`, you see that multiple pods that are running on the same worker node are stuck in the `ContainerCreating` state.

{: tsCauses}
The file system on the worker node is read-only.

{: tsResolve}
1. Back up any data that might be stored on the worker node or in your containers.
2. Rebuild the worker node by running the following command.

<pre class="pre"><code>bx cs worker-reload &lt;cluster_name&gt; &lt;worker_id&gt;</code></pre>

<br />




## Containers do not start
{: #containers_do_not_start}

{: tsSymptoms}
The pods deploy successfully to clusters, but the containers do not start.

{: tsCauses}
Containers might not start when the registry quota is reached.

{: tsResolve}
[Free up storage in {{site.data.keyword.registryshort_notm}}.](../services/Registry/registry_quota.html#registry_quota_freeup)

<br />




## Logs do not appear
{: #cs_no_logs}

{: tsSymptoms}
When you access the Kibana dashboard, logs do not display.

{: tsResolve}
Review the following reasons why logs are not appearing and the corresponding troubleshooting steps:

<table>
<col width="40%">
<col width="60%">
 <thead>
 <th>Why it's happening</th>
 <th>How to fix it</th>
 </thead>
 <tbody>
 <tr>
 <td>No logging configuration is set up.</td>
 <td>In order for logs to be sent, you must first create a logging configuration to forward logs to {{site.data.keyword.loganalysislong_notm}}. To create a logging configuration, see <a href="cs_health.html#log_sources_enable">Enabling log forwarding</a>.</td>
 </tr>
 <tr>
 <td>The cluster is not in a <code>Normal</code> state.</td>
 <td>To check the state of your cluster, see <a href="cs_troubleshoot.html#debug_clusters">Debugging clusters</a>.</td>
 </tr>
 <tr>
 <td>The log storage quota has been hit.</td>
 <td>To increase your log storage limits, see the <a href="/docs/services/CloudLogAnalysis/troubleshooting/error_msgs.html#error_msgs">{{site.data.keyword.loganalysislong_notm}} documentation</a>.</td>
 </tr>
 <tr>
 <td>If you specified a space at cluster creation, the account owner does not have Manager, Developer, or Auditor permissions to that space.</td>
 <td>To change access permissions for the account owner:<ol><li>To find out who the account owner for the cluster is, run <code>bx cs api-key-info &lt;cluster_name_or_id&gt;</code>.</li><li>To grant that account owner Manager, Developer, or Auditor {{site.data.keyword.containershort_notm}} access permissions to the space, see <a href="cs_users.html#managing">Managing cluster access</a>.</li><li>To refresh the logging token after permissions have been changed, run <code>bx cs logging-config-refresh &lt;cluster_name_or_id&gt;</code>.</li></ol></td>
 </tr>
 </tbody></table>

To test changes you made during troubleshooting, you can deploy Noisy, a sample pod that produces several log events, onto a worker node in your cluster.

  1. [Target your CLI](cs_cli_install.html#cs_cli_configure) to a cluster where you want to start producing logs.

  2. Create the `deploy-noisy.yaml` configuration file.

      ```
      apiVersion: v1
      kind: Pod
      metadata:
        name: noisy
      spec:
        containers:
        - name: noisy
          image: ubuntu:16.04
          command: ["/bin/sh"]
          args: ["-c", "while true; do sleep 10; echo 'Hello world!'; done"]
          imagePullPolicy: "Always"
        ```
        {: codeblock}

  3. Run the configuration file in the cluster's context.

        ```
        kubectl apply -f <filepath_to_noisy>
        ```
        {:pre}

  4. After a few minutes, you can view your logs in the Kibana dashboard. To access the Kibana dashboard, go to one of the following URLs and select the {{site.data.keyword.Bluemix_notm}} account where you created the cluster. If you specified a space at cluster creation, go to that space instead.
        - US-South and US-East: https://logging.ng.bluemix.net
        - UK-South and EU-Central: https://logging.eu-fra.bluemix.net
        - AP-South: https://logging.au-syd.bluemix.net

<br />




## Kubernetes dashboard does not display utilization graphs
{: #cs_dashboard_graphs}

{: tsSymptoms}
When you access the Kubernetes dashboard, utilization graphs do not display.

{: tsCauses}
Sometimes after a cluster update or worker node reboot the `kube-dashboard` pod does not update.

{: tsResolve}
Delete the `kube-dashboard` pod to force a restart. The pod is re-created with RBAC policies to access heapster for utilization information.

  ```
  kubectl delete pod -n kube-system $(kubectl get pod -n kube-system --selector=k8s-app=kubernetes-dashboard -o jsonpath='{.items..metadata.name}')
  ```
  {: pre}

<br />


## Cannot connect to an app via a load balancer service
{: #cs_loadbalancer_fails}

{: tsSymptoms}
You publicly exposed your app by creating a load balancer service in your cluster. When you tried to connect to your app via the public IP address of the load balancer, the connection failed or timed out.

{: tsCauses}
Your load balancer service might not be working properly for one of the following reasons:

-   The cluster is a lite cluster or a standard cluster with only one worker node.
-   The cluster is not fully deployed yet.
-   The configuration script for your load balancer service includes errors.

{: tsResolve}
To troubleshoot your load balancer service:

1.  Check that you set up a standard cluster that is fully deployed and has at least two worker nodes to assure high availability for your load balancer service.

  ```
  bx cs workers <cluster_name_or_id>
  ```
  {: pre}

    In your CLI output, make sure that the **Status** of your worker nodes displays **Ready** and that the **Machine Type** shows a machine type other than **free**.

2.  Check the accuracy of the configuration file for your load balancer service.

  ```
  apiVersion: v1
  kind: Service
  metadata:
    name: myservice
  spec:
    type: LoadBalancer
    selector:
      <selectorkey>:<selectorvalue>
    ports:
     - protocol: TCP
       port: 8080
  ```
  {: pre}

    1.  Check that you defined **LoadBalancer** as the type for your service.
    2.  Make sure that you used the same **<selectorkey>** and **<selectorvalue>** that you used in the **label/metadata** section when you deployed your app.
    3.  Check that you used the **port** that your app listens on.

3.  Check your load balancer service and review the **Events** section to find potential errors.

  ```
  kubectl describe service <myservice>
  ```
  {: pre}

    Look for the following error messages:

    <ul><li><pre class="screen"><code>Clusters with one node must use services of type NodePort</code></pre></br>To use the load balancer service, you must have a standard cluster with at least two worker nodes.</li>
    <li><pre class="screen"><code>No cloud provider IPs are available to fulfill the load balancer service request. Add a portable subnet to the cluster and try again</code></pre></br>This error message indicates that no portable public IP addresses are left to be allocated to your load balancer service. Refer to <a href="cs_subnets.html#subnets">Adding subnets to clusters</a> to find information about how to request portable public IP addresses for your cluster. After portable public IP addresses are available to the cluster, the load balancer service is automatically created.</li>
    <li><pre class="screen"><code>Requested cloud provider IP <cloud-provider-ip> is not available. The following cloud provider IPs are available: <available-cloud-provider-ips></code></pre></br>You defined a portable public IP address for your load balancer service by using the **loadBalancerIP** section, but this portable public IP address is not available in your portable public subnet. Change your load balancer service configuration script and either choose one of the available portable public IP addresses, or remove the **loadBalancerIP** section from your script so that an available portable public IP address can be allocated automatically.</li>
    <li><pre class="screen"><code>No available nodes for load balancer services</code></pre>You do not have enough worker nodes to deploy a load balancer service. One reason might be that you deployed a standard cluster with more than one worker node, but the provisioning of the worker nodes failed.</li>
    <ol><li>List available worker nodes.</br><pre class="codeblock"><code>kubectl get nodes</code></pre></li>
    <li>If at least two available worker nodes are found, list the worker node details.</br><pre class="screen"><code>bx cs worker-get [&lt;cluster_name_or_id&gt;] &lt;worker_ID&gt;</code></pre></li>
    <li>Make sure that the public and private VLAN IDs for the worker nodes that were returned by the <code>kubectl get nodes</code> and the <code>bx cs [&lt;cluster_name_or_id&gt;] worker-get</code> commands match.</li></ol></li></ul>

4.  If you are using a custom domain to connect to your load balancer service, make sure that your custom domain is mapped to the public IP address of your load balancer service.
    1.  Find the public IP address of your load balancer service.

      ```
      kubectl describe service <myservice> | grep "LoadBalancer Ingress"
      ```
      {: pre}

    2.  Check that your custom domain is mapped to the portable public IP address of your load balancer service in the Pointer record (PTR).

<br />


## Cannot connect to an app via Ingress
{: #cs_ingress_fails}

{: tsSymptoms}
You publicly exposed your app by creating an Ingress resource for your app in your cluster. When you tried to connect to your app via the public IP address or subdomain of the Ingress controller, the connection failed or timed out.

{: tsCauses}
Ingress might not be working properly for the following reasons:
<ul><ul>
<li>The cluster is not fully deployed yet.
<li>The cluster was set up as a lite cluster or as a standard cluster with only one worker node.
<li>The Ingress configuration script includes errors.
</ul></ul>

{: tsResolve}
To troubleshoot your Ingress:

1.  Check that you set up a standard cluster that is fully deployed and has at least two worker nodes to assure high availability for your Ingress controller.

  ```
  bx cs workers <cluster_name_or_id>
  ```
  {: pre}

    In your CLI output, make sure that the **Status** of your worker nodes displays **Ready** and that the **Machine Type** shows a machine type other than **free**.

2.  Retrieve the Ingress controller subdomain and public IP address, and then ping each one.

    1.  Retrieve the Ingress controller subdomain.

      ```
      bx cs cluster-get <cluster_name_or_id> | grep "Ingress subdomain"
      ```
      {: pre}

    2.  Ping the Ingress controller subdomain.

      ```
      ping <ingress_controller_subdomain>
      ```
      {: pre}

    3.  Retrieve the public IP address of your Ingress controller.

      ```
      nslookup <ingress_controller_subdomain>
      ```
      {: pre}

    4.  Ping the Ingress controller public IP address.

      ```
      ping <ingress_controller_ip>
      ```
      {: pre}

    If the CLI returns a timeout for the public IP address or subdomain of the Ingress controller, and you have set up a custom firewall that is protecting your worker nodes, you might need to open additional ports and networking groups in your [firewall](#cs_firewall).

3.  If you are using a custom domain, make sure that your custom domain is mapped to the public IP address or subdomain of the IBM-provided Ingress controller with your Domain Name Service (DNS) provider.
    1.  If you used the Ingress controller subdomain, check your Canonical Name record (CNAME).
    2.  If you used the Ingress controller public IP address, check that your custom domain is mapped to the portable public IP address in the Pointer record (PTR).
4.  Check your Ingress configuration file.

    ```
    apiVersion: extensions/v1beta1
    kind: Ingress
    metadata:
      name: myingress
    spec:
      tls:
      - hosts:
        - <ingress_subdomain>
        secretName: <ingress_tlssecret>
      rules:
      - host: <ingress_subdomain>
        http:
          paths:
          - path: /
            backend:
              serviceName: myservice
              servicePort: 80
    ```
    {: codeblock}

    1.  Check that the Ingress controller subdomain and TLS certificate are correct. To find the IBM provided subdomain and TLS certificate, run bx cs cluster-get <cluster_name_or_id>.
    2.  Make sure that your app listens on the same path that is configured in the **path** section of your Ingress. If your app is set up to listen on the root path, include **/** as your path.
5.  Check your Ingress deployment and look for potential error messages.

  ```
  kubectl describe ingress <myingress>
  ```
  {: pre}

6.  Check the logs for your Ingress controller.
    1.  Retrieve the ID of the Ingress pods that are running in your cluster.

      ```
      kubectl get pods -n kube-system | grep alb1
      ```
      {: pre}

    2.  Retrieve the logs for each Ingress pod.

      ```
      kubectl logs <ingress_pod_id> -n kube-system
      ```
      {: pre}

    3.  Look for error messages in the Ingress controller logs.

<br />



## Ingress application load balancer secret issues
{: #cs_albsecret_fails}

{: tsSymptoms}
After deploying an Ingress application load balancer secret to your cluster, the `Description` field is not updating with the secret name when you view your certificate in {{site.data.keyword.cloudcerts_full_notm}}.

When you list information about the application load balancer secret, the status says `*_failed`. For example, `create_failed`, `update_failed`, `delete_failed`.

{: tsResolve}
Review the following reasons why the application load balancer secret might fail and the corresponding troubleshooting steps:

<table>
<col width="40%">
<col width="60%">
 <thead>
 <th>Why it's happening</th>
 <th>How to fix it</th>
 </thead>
 <tbody>
 <tr>
 <td>You do not have the required access roles to download and update certificate data.</td>
 <td>Check with your account Administrator to assign you both the **Operator** and **Editor** roles for your {{site.data.keyword.cloudcerts_full_notm}} instance. For more details, see <a href="/docs/services/certificate-manager/about.html#identity-access-management">Identity and Access Management</a> for {{site.data.keyword.cloudcerts_short}}.</td>
 </tr>
 <tr>
 <td>The certificate CRN provided at time of create, update, or remove does not belong to the same account as the cluster.</td>
 <td>Check that the certificate CRN you provided is imported to an instance of the {{site.data.keyword.cloudcerts_short}} service that is deployed in the same account as your cluster.</td>
 </tr>
 <tr>
 <td>The certificate CRN provided at time of create is incorrect.</td>
 <td><ol><li>Check the accuracy of the certificate CRN string you provide.</li><li>If the certificate CRN is found to be accurate, then try to update the secret. <pre class="pre"><code>bx cs alb-cert-deploy --update --cluster &lt;cluster_name_or_id&gt; --secret-name &lt;secret_name&gt; --cert-crn &lt;certificate_CRN&gt;</code></pre></li><li>If this command results in the <code>update_failed</code> status, then remove the secret. <pre class="pre"><code>bx cs alb-cert-rm --cluster &lt;cluster_name_or_id&gt; --secret-name &lt;secret_name&gt;</code></pre></li><li>Deploy the secret again. <pre class="pre"><code>bx cs alb-cert-deploy --cluster &lt;cluster_name_or_id&gt; --secret-name &lt;secret_name&gt; --cert-crn &lt;certificate_CRN&gt;</code></pre></li></ol></td>
 </tr>
 <tr>
 <td>The certificate CRN provided at time of update is incorrect.</td>
 <td><ol><li>Check the accuracy of the certificate CRN string you provide.</li><li>If the certificate CRN is found to be accurate, then remove the secret. <pre class="pre"><code>bx cs alb-cert-rm --cluster &lt;cluster_name_or_id&gt; --secret-name &lt;secret_name&gt;</code></pre></li><li>Deploy the secret again. <pre class="pre"><code>bx cs alb-cert-deploy --cluster &lt;cluster_name_or_id&gt; --secret-name &lt;secret_name&gt; --cert-crn &lt;certificate_CRN&gt;</code></pre></li><li>Try to update the secret. <pre class="pre"><code>bx cs alb-cert-deploy --update --cluster &lt;cluster_name_or_id&gt; --secret-name &lt;secret_name&gt; --cert-crn &lt;certificate_CRN&gt;</code></pre></li></ol></td>
 </tr>
 <tr>
 <td>The {{site.data.keyword.cloudcerts_long_notm}} service is experiencing downtime.</td>
 <td>Check that your {{site.data.keyword.cloudcerts_short}} service is up and running.</td>
 </tr>
 </tbody></table>

<br />



## Cannot retrieve the ETCD URL for Calico CLI configuration
{: #cs_calico_fails}

{: tsSymptoms}
When you retrieve the `<ETCD_URL>` to [add network policies](cs_network_policy.html#adding_network_policies), you get a `calico-config not found` error message.

{: tsCauses}
Your cluster is not at [Kubernetes version 1.7](cs_versions.html) or later.

{: tsResolve}
[Update your cluster](cs_cluster_update.html#master) or retrieve the `<ETCD_URL>` with commands that are compatible with earlier versions of Kubernetes.

To retrieve the `<ETCD_URL>`, run one of the following commands:

- Linux and OS X:

    ```
    kubectl describe pod -n kube-system `kubectl get pod -n kube-system | grep calico-policy-controller | awk '{print $1}'` | grep ETCD_ENDPOINTS | awk '{print $2}'
    ```
    {: pre}

- Windows:
    <ol>
    <li> Get a list of the pods in the kube-system namespace and locate the Calico controller pod. </br><pre class="codeblock"><code>kubectl get pod -n kube-system</code></pre></br>Example:</br><pre class="screen"><code>calico-policy-controller-1674857634-k2ckm</code></pre>
    <li> View the details of the Calico controller pod.</br> <pre class="codeblock"><code>kubectl describe pod -n kube-system calico-policy-controller-&lt;ID&gt;</code></pre>
    <li> Locate the ETCD endpoints value. Example: <code>https://169.1.1.1:30001</code>
    </ol>

When you retrieve the `<ETCD_URL>`, continue with the steps as listed in (Adding network policies)[cs_network_policy.html#adding_network_policies].

<br />




## Getting help and support
{: #ts_getting_help}

Where do you start troubleshooting a container?

-   To see whether {{site.data.keyword.Bluemix_notm}} is available, [check the {{site.data.keyword.Bluemix_notm}} status page ![External link icon](../icons/launch-glyph.svg "External link icon")](https://developer.ibm.com/bluemix/support/#status).
-   Post a question in the [{{site.data.keyword.containershort_notm}} Slack. ![External link icon](../icons/launch-glyph.svg "External link icon")](https://ibm-container-service.slack.com) Tip: If you are not using an IBM ID for your {{site.data.keyword.Bluemix_notm}} account, [request an invitation](https://bxcs-slack-invite.mybluemix.net/) to this Slack.
-   Review the forums to see whether other users ran into the same issue. When you use the forums to ask a question, tag your question so that it is seen by the {{site.data.keyword.Bluemix_notm}} development teams.

    -   If you have technical questions about developing or deploying clusters or apps with {{site.data.keyword.containershort_notm}}, post your question on [Stack Overflow ![External link icon](../icons/launch-glyph.svg "External link icon")](https://stackoverflow.com/questions/tagged/ibm-cloud+containers) and tag your question with `ibm-cloud`, `kubernetes`, and `containers`.
    -   For questions about the service and getting started instructions, use the [IBM developerWorks dW Answers ![External link icon](../icons/launch-glyph.svg "External link icon")](https://developer.ibm.com/answers/topics/containers/?smartspace=bluemix) forum. Include the `ibm-cloud` and `containers` tags.
    See [Getting help](/docs/support/index.html#getting-help) for more details about using the forums.

-   Contact IBM Support. For information about opening an IBM support ticket, or about support levels and ticket severities, see [Contacting support](/docs/support/index.html#contacting-support).

{:tip}
When reporting an issue, include your cluster ID. To get your cluster ID, run `bx cs clusters`.
