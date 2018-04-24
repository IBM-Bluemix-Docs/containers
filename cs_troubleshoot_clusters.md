---

copyright:
  years: 2014, 2018
lastupdated: "2018-4-24"

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



# Troubleshooting clusters and worker nodes
{: #cs_troubleshoot_clusters}

As you use {{site.data.keyword.containerlong}}, consider these techniques for troubleshooting your clusters and worker nodes.
{: shortdesc}

If you have a more general issue, try out [cluster debugging](cs_troubleshoot.html).
{: tip}

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


## After updating or reloading a worker node, applications receive RBAC DENY errors
{: #cs_rbac_deny}

{: tsSymptoms}
After updating to Kubernetes version 1.7, applications receive `RBAC DENY` errors.

{: tsCauses}
As of [Kubernetes version 1.7](cs_versions.html#cs_v17), applications that run in the `default` namespace no longer have cluster administrator privileges to the Kubernetes API for enhanced security.

If your app runs in the `default` namespace, uses the `default ServiceAccount`, and accesses the Kubernetes API, it is affected by this Kubernetes change. For more information, see [the Kubernetes documentation![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/admin/authorization/rbac/#upgrading-from-15).

{: tsResolve}
Before you begin, [target your CLI](cs_cli_install.html#cs_cli_configure) to your cluster.

1.  **Temporary action**: As you update your app RBAC policies, you might want to revert temporarily to the previous `ClusterRoleBinding` for the `default ServiceAccount` in the `default` namespace.

    1.  Copy the following `.yaml` file.

        ```yaml
        kind: ClusterRoleBinding
        apiVersion: rbac.authorization.k8s.io/v1beta1
        metadata:
         name: admin-binding-nonResourceURLSs-default
        subjects:
          - kind: ServiceAccount
            name: default
            namespace: default
        roleRef:
         kind: ClusterRole
         name: admin-role-nonResourceURLSs
         apiGroup: rbac.authorization.k8s.io
        ---
        kind: ClusterRoleBinding
        apiVersion: rbac.authorization.k8s.io/v1beta1
        metadata:
         name: admin-binding-resourceURLSs-default
        subjects:
          - kind: ServiceAccount
            name: default
            namespace: default
        roleRef:
         kind: ClusterRole
         name: admin-role-resourceURLSs
         apiGroup: rbac.authorization.k8s.io
        ```

    2.  Apply the `.yaml` files to your cluster.

        ```
        kubectl apply -f FILENAME
        ```
        {: pre}

2.  [Create RBAC authorization resources![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/admin/authorization/rbac/#api-overview) to update the `ClusterRoleBinding` admin access.

3.  If you created a temporary cluster role binding, remove it.

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
  ID                                                 Public IP       Private IP       Machine Type   State     Status   Location   Version
  kube-dal10-cr9b7371a7fcbe46d08e04f046d5e6d8b4-w1   169.xx.xxx.xxx  10.xxx.xx.xxx    b2c.4x16       normal    Ready    dal10      1.8.11
  kube-dal10-cr9b7371a7fcbe46d08e04f046d5e6d8b4-w2   169.xx.xxx.xxx  10.xxx.xx.xxx    b2c.4x16       deleted    -       dal10      1.8.11
  ```
  {: screen}

2.  Install the [Calico CLI](cs_network_policy.html#adding_network_policies).
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
  bx cs worker-reboot CLUSTER_ID NODE_ID
  ```
  {: pre}


The deleted node is no longer listed in Calico.

<br />


## Cluster remains in a pending State
{: #cs_cluster_pending}

{: tsSymptoms}
When you deploy your cluster, it remains in a pending state and doesn't start.

{: tsCauses}
If you just created the cluster, the worker nodes might still be configuring. If you have been waiting for awhile, you may have an invalid VLAN.

{: tsResolve}

You can try one of the following solutions:
  - Check the status of your cluster by running `bx cs clusters`. Then check to be sure that your worker nodes are deployed by running `bx cs workers <cluster_name>`.
  - Check to see if your VLAN is valid. To be valid, a VLAN must be associated with infrastructure that can host a worker with local disk storage. You can [list your VLANs](/docs/containers/cs_cli_reference.html#cs_vlans) by running `bx cs vlans LOCATION` if the VLAN does not show in the list, then it is not valid. Choose a different VLAN.

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
  bx cs worker-add <cluster_name_or_ID> 1
  ```
  {: pre}

5.  If your pods still stay in a **pending** state after the worker node is fully deployed, review the [Kubernetes documentation ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/tasks/debug-application-cluster/debug-pod-replication-controller/#my-pod-stays-pending) to further troubleshoot the pending state of your pod.

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



## Cannot install a Helm chart with updated configuration values
{: #cs_helm_install}

{: tsSymptoms}
When you try to install an updated Helm chart by running `helm install -f config.yaml --namespace=kube-system --name=<release_name> ibm/<chart_name>`, you get the `Error: failed to download "ibm/<chart_name>"` error message.

{: tsCauses}
The URL for the {{site.data.keyword.Bluemix_notm}} repository in your Helm instance might be incorrect.

{: tsResolve}
To troubleshoot your Helm chart:

1. List the repositories currently available in your Helm instance.

    ```
    helm repo list
    ```
    {: pre}

2. In the output, verify that the URL for the {{site.data.keyword.Bluemix_notm}} repository, `ibm`, is `https://registry.bluemix.net/helm/ibm`.

    ```
    NAME    URL
    stable  https://kubernetes-charts.storage.googleapis.com
    local   http://127.0.0.1:8888/charts
    ibm     https://registry.bluemix.net/helm/ibm
    ```
    {: screen}

    * If the URL is incorrect:

        1. Remove the {{site.data.keyword.Bluemix_notm}} repository.

            ```
            helm repo remove ibm
            ```
            {: pre}

        2. Add the {{site.data.keyword.Bluemix_notm}} repository again.

            ```
            helm repo add ibm  https://registry.bluemix.net/helm/ibm
            ```
            {: pre}

    * If the URL is correct, get the latest updates from the repository.

        ```
        helm repo update
        ```
        {: pre}

3. Install the Helm chart with your updates.

    ```
    helm install -f config.yaml --namespace=kube-system --name=<release_name> ibm/<chart_name>
    ```
    {: pre}


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


