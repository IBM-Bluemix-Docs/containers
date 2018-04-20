---

copyright:
  years: 2014, 2018
lastupdated: "2018-04-18"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}


# Updating clusters and worker nodes
{: #update}

You can install updates to keep your Kubernetes clusters up-to-date in {{site.data.keyword.containerlong}}.
{:shortdesc}

## Updating the Kubernetes master
{: #master}

Periodically, Kubernetes releases [major, minor, or patch updates](cs_versions.html#version_types). Depending on the type of update, you could be responsible for updating the Kubernetes master components.
{:shortdesc}

Updates can affect the Kubernetes API server version or other components in your Kubernetes master.  You are always responsible for keeping your worker nodes up to date. When making updates, the Kubernetes master is updated before the worker nodes.

By default, your ability to update the Kubernetes API server is limited in your Kubernetes master more than two minor versions ahead of your current version. For example, if your current Kubernetes API server version is 1.5 and you want to update to 1.8, you must first update to 1.7. You can force the update to occur, but updating more than two minor versions might cause unexpected results. If your cluster is running an unsupported Kubernetes version, you might have to force the update.

The following diagram shows the process that you can take to update your master.

![Master update best practice](/images/update-tree.png)

Figure 1. Updating Kubernetes master process diagram

**Attention**: You cannot roll back a cluster to a previous version after the update process takes place. Be sure to use a test cluster and follow the instructions to address potential issues before updating your production master.

For _major_ or _minor_ updates, complete the following steps:

1. Review the [Kubernetes changes](cs_versions.html) and make any updates marked _Update before master_.
2. Update your Kubernetes API server and associated Kubernetes master components by using the GUI or running the [CLI command](cs_cli_reference.html#cs_cluster_update). When you update the Kubernetes API server, the API server is down for about 5 - 10 minutes. During the update, you cannot access or change the cluster. However, worker nodes, apps, and resources that cluster users have deployed are not modified and continue to run.
3. Confirm that the update is complete. Review the Kubernetes API server version on the {{site.data.keyword.Bluemix_notm}} Dashboard or run `{[bxcs]} clusters`.
4. Install the version of the [`kubectl cli`](cs_cli_install.html#kubectl) that matches the Kubernetes API server version that runs in the Kubernetes master.

When the Kubernetes API server update is complete, you can update your worker nodes.

{[white-space.md]}

## Updating worker nodes
{: #worker_node}

You received a notification to update your worker nodes. What does that mean? As security updates and patches are put in place for the Kubernetes API server and other Kubernetes master components, you must be sure that your worker nodes remain in sync.
{: shortdesc}

The worker node Kubernetes version cannot be higher than the Kubernetes API server version that runs in your Kubernetes master. Before you begin, [update the Kubernetes master](#master).

<ul>**Attention**:</br>
<li>Updates to worker nodes can cause downtime for your apps and services.</li>
<li>Data is deleted if not stored outside the pod.</li>
<li>Use [replicas ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#replicas) in your deployments to reschedule pods on available nodes.</li></ul>

But what if I can't have downtime?

As part of the update process, specific nodes are going to go down for a period of time. To help avoid down time for your application, you can define unique keys in a configuration map that specifies threshold percentages for specific types of nodes during the upgrade process. By defining rules based on standard Kubernetes labels and giving a percentage of the maximum amount of nodes that are allowed to be unavailable, you can ensure that your app remains up and running. A node is considered unavailable if it has yet to complete the deploy process.

How are the keys defined?

In the data information section of the configuration map, you can define up to 10 seperate rules to run at any given time. To be upgraded, worker nodes must pass every defined rule.

The keys are defined. What now?

After you define your rules, you run the `{[bxcs]} worker-update` command. If a successful response is returned, the worker nodes are queued to be updated. However, the nodes do not undergo the update process until all of the rules are satisfied. While they're queued, the rules are checked on an interval to see if any of the nodes are able to be updated.

What if I chose to not define a configuration map?

When the configuration map is not defined, the default is used. By default, a maximum of 20% of all of your worker nodes in each cluster are unavailable during the update process.

To update your worker nodes:

1. Make any changes that are marked _Update after master_ in [Kubernetes changes](cs_versions.html).

2. Optional: Define your configuration map.
    Example:

    ```
    apiVersion: v1
    kind: ConfigMap
    metadata:
      name: ibm-cluster-update-configuration
      namespace: kube-system
    data:
     drain_timeout_seconds: "120"
     zonecheck.json: |
       {
         "MaxUnavailablePercentage": 70,
         "NodeSelectorKey": "failure-domain.beta.kubernetes.io/zone",
         "NodeSelectorValue": "dal13"
       }
     regioncheck.json: |
       {
         "MaxUnavailablePercentage": 80,
         "NodeSelectorKey": "failure-domain.beta.kubernetes.io/region",
         "NodeSelectorValue": "us-south"
       }
     defaultcheck.json: |
       {
         "MaxUnavailablePercentage": 100
       }
    ```
    {:pre}
  <table summary="The first row in the table spans both columns. The rest of the rows should be read left to right, with the parameter in column one and the description that matches in column two.">
    <thead>
      <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the components </th>
    </thead>
    <tbody>
      <tr>
        <td><code>drain_timeout_seconds</code></td>
        <td> Optional: The timeout in seconds of the drain that occurs during the worker node update. Drain sets the node to `unschedulable`, which prevents new pods from being deployed to that node. Drain also deletes pods off of the node. Accepted values are integers from 1 to 180. The default value is 30.</td>
      </tr>
      <tr>
        <td><code>zonecheck.json</code></br><code>regioncheck.json</code></td>
        <td> Examples of unique keys for which you want to set rules. The names of the keys can be anything you want them to be; the information is parsed by the configurations set within the key. For each key that you define, you can set only one value for <code>NodeSelectorKey</code> and <code>NodeSelectorValue</code>. If you want to set rules for more than one region, or location (data center), create a new key entry. </td>
      </tr>
      <tr>
        <td><code>defaultcheck.json</code></td>
        <td> As a default, if the <code>ibm-cluster-update-configuration</code> map is not defined in a valid way, only 20% of your clusters are able to be unavailable at one time. If one or more valid rules are defined without a global default, the new default is to allow 100% of the workers to be unavailable at one time. You can control this by creating a default percentage. </td>
      </tr>
      <tr>
        <td><code>MaxUnavailablePercentage</code></td>
        <td> The maximum amount of nodes that are allowed to be unavailable for a specified key, specified as a percentage. A node is unavailable when it is in the process of deploying, reloading, or provisioning. The queued worker nodes are blocked from upgrading if it exceeds any defined maximum unavailable percentages. </td>
      </tr>
      <tr>
        <td><code>NodeSelectorKey</code></td>
        <td> The type of label for which you want to set a rule for a specified key. You can set rules for the default labels provided by IBM, as well as on labels that you created. </td>
      </tr>
      <tr>
        <td><code>NodeSelectorValue</code></td>
        <td> The subset of nodes within a specified key that the rule is set to evaluate. </td>
      </tr>
    </tbody>
  </table>

    **Note**: A maximum of 10 rules can be defined. If you add more than 10 keys to one file, only a subset of the information is parsed.

3. Update your worker nodes from the GUI or by running the CLI command.
  * To update from the {{site.data.keyword.Bluemix_notm}} Dashboard, navigate to the `Worker Nodes` section of your cluster, and click `Update Worker`.
  * To get worker node IDs, run `{[bxcs]} workers <cluster_name_or_ID>`. If you select multiple worker nodes, the worker nodes are placed in a queue for update evaluation. If they are considered ready after evaluation, they will be updated according to the rules set in the configurations

    ```
    {[bxcs]} worker-update <cluster_name_or_ID> <worker_node1_ID> <worker_node2_ID>
    ```
    {: pre}

4. Optional: Verify the events that are triggered by the configuration map and any validation errors that occur by running the following command and looking at **Events**.
    ```
    kubectl describe -n kube-system cm ibm-cluster-update-configuration
    ```
    {: pre}

5. Confirm that the update is complete:
  * Review the Kubernetes version on the {{site.data.keyword.Bluemix_notm}} Dashboard or run `{[bxcs]} workers <cluster_name_or_ID>`.
  * Review the Kubernets version of the worker nodes by running `kubectl get nodes`.
  * In some cases, older clusters might list duplicate worker nodes with a **NotReady** status after an update. To remove duplicates, see [troubleshooting](cs_troubleshoot_clusters.html#cs_duplicate_nodes).

Next steps:
  - Repeat the update process with other clusters.
  - Inform developers who work in the cluster to update their `kubectl` CLI to the version of the Kubernetes master.
  - If the Kubernetes dashboard does not display utilization graphs, [delete the `kube-dashboard` pod](cs_troubleshoot_health.html#cs_dashboard_graphs).


{[white-space.md]}


## Updating machine types
{: #machine_type}

You can update the machine types that are used in worker nodes by adding new worker nodes and removing the old ones. For example, if you have virtual worker nodes on deprecated machine types with `u1c` or `b1c` in the names, create worker nodes that use machine types with `u2c` or `b2c` in the names.
{: shortdesc}

1. Note the names and locations of the worker nodes to update.
    ```
    {[bxcs]} workers <cluster_name>
    ```
    {: pre}

2. View the available machine types.
    ```
    {[bxcs]} machine-types <location>
    ```
    {: pre}

3. Add worker nodes by using the [{[bxcs]} worker-add](cs_cli_reference.html#cs_worker_add) command. Specify a machine type.

    ```
    {[bxcs]} worker-add --cluster <cluster_name> --machine-type <machine_type> --number <number_of_worker_nodes> --private-vlan <private_VLAN_ID> --public-vlan <public_VLAN_ID>
    ```
    {: pre}

4. Verify that the worker nodes are added.

    ```
    {[bxcs]} workers <cluster_name>
    ```
    {: pre}

5. When the added worker nodes are in the `Normal` state, you can remove the outdated worker node. **Note**: If you are removing a machine type that is billed monthly (such as bare metal), you are charged for the entire the month.

    ```
    {[bxcs]} worker-rm <cluster_name> <worker_node>
    ```
    {: pre}

6. Repeat these steps to upgrade other worker nodes to different machine types.


