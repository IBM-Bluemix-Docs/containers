---

copyright:
  years: 2014, 2018
lastupdated: "2018-01-11"

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

## Updating the Kubernetes master
{: #master}


Kubernetes periodically updates [major, minor, and patch versions](cs_versions.html#version_types), which impacts your clusters. Updating a cluster is a two-step process. First, you must update the Kubernetes master, and then you can update each of the worker nodes.

By default, you cannot update a Kubernetes master more than two minor versions ahead. For example, if your current master is version 1.5 and you want to update to 1.8, you must update to 1.7 first. You can force the update to continue, but updating more than two minor versions might cause unexpected results.

**Attention**: Follow the update instructions and use a test cluster to address potential app outages and interruptions during the update. You cannot roll back a cluster to a previous version.

When making a _major_ or _minor_ update, complete the following steps.

1. Review the [Kubernetes changes](cs_versions.html) and make any updates marked _Update before master_.
2. Update your Kubernetes master by using the GUI or running the [CLI command](cs_cli_reference.html#cs_cluster_update). When you update the Kubernetes master, the master is down for about 5 - 10 minutes. During the update, you cannot access or change the cluster. However, worker nodes, apps, and resources that cluster users have deployed are not modified and continue to run.
3. Confirm that the update is complete. Review the Kubernetes version on the {{site.data.keyword.Bluemix_notm}} Dashboard or run `bx cs clusters`.

When the Kubernetes master update is complete, you can update your worker nodes.


<br />


## Updating worker nodes
{: #worker_node}


While IBM automatically applies patches to the Kubernetes master, you must explicitly update worker nodes for major and minor updates. The worker node version cannot be higher than the Kubernetes master.

**Attention**: Updates to worker nodes can cause downtime for your apps and services:
- Data is deleted if not stored outside the pod.
- Use [replicas ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#replicas) in your deployments to reschedule pods on available nodes.

Updating production-level clusters:
- To help avoid downtime for your apps, the update process prevents pods from being scheduled on the worker node during the update. See [`kubectl drain` ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#drain) for more information.
- Use a test cluster to validate that your workloads and the delivery process are not impacted by the update. You cannot roll back worker nodes to a previous version.
- Production-level clusters should have capacity to survive a worker node failure. If your cluster does not, add a worker node before updating the cluster.
- A rolling update is performed when multiple worker nodes are requested to upgrade. A maximum of 20 percent of the total amount of worker nodes in a cluster can be upgraded simultaneously. The upgrade process waits for a worker node to complete the upgrade before another worker starts upgrading.


1. Install the version of the [`kubectl cli` ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/tasks/tools/install-kubectl/) that matches the Kubernetes version of the Kubernetes master.

2. Make any changes that are marked _Update after master_ in [Kubernetes changes](cs_versions.html).

3. Update your worker nodes:
  * To update from the {{site.data.keyword.Bluemix_notm}} Dashboard, navigate to the `Worker Nodes` section of your cluster, and click `Update Worker`.
  * To get worker node IDs, run `bx cs workers <cluster_name_or_id>`. If you select multiple worker nodes, the worker nodes are updated one at a time.

    ```
    bx cs worker-update <cluster_name_or_id> <worker_node_id1> <worker_node_id2>
    ```
    {: pre}

4. Confirm that the update is complete:
  * Review the Kubernetes version on the {{site.data.keyword.Bluemix_notm}} Dashboard or run `bx cs workers <cluster_name_or_id>`.
  * Review the Kubernets version of the worker nodes by running `kubectl get nodes`.
  * In some cases, older clusters might list duplicate worker nodes with a **NotReady** status after an update. To remove duplicates, see [troubleshooting](cs_troubleshoot.html#cs_duplicate_nodes).

After you complete the update:
  - Repeat the update process with other clusters.
  - Inform developers who work in the cluster to update their `kubectl` CLI to the version of the Kubernetes master.
  - If the Kubernetes dashboard does not display utilization graphs, [delete the `kube-dashboard` pod](cs_troubleshoot.html#cs_dashboard_graphs).



<br />

