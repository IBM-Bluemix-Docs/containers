---

copyright:
  years: 2014, 2018
lastupdated: "2018-03-30"

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


# Troubleshooting cluster storage
{: #cs_troubleshoot_storage}

As you use {{site.data.keyword.containerlong}}, consider these techniques for troubleshooting cluster networking. Before trying these techniques, you can take some general steps to [debug your cluster and check for common issues](cs_troubleshoot.html).
{: shortdesc}

## File systems for worker nodes change to read-only
{: #readonly_nodes}

{: tsSymptoms}
{: #stuck_creating_state}
You might see one of the following symptoms:
- When you run `kubectl get pods -o wide`, you see that multiple pods that are running on the same worker node are stuck in the `ContainerCreating` state.
- When you run a `kubectl describe` command, you see the following error in the events section: `MountVolume.SetUp failed for volume ... read-only file system`.

{: tsCauses}
The file system on the worker node is read-only.

{: tsResolve}
1. Back up any data that might be stored on the worker node or in your containers.
2. For a short-term fix to the existing worker node, reload the worker node.

<pre class="pre"><code>bx cs worker-reload <cluster_name> <worker_id></code></pre>

For a long-term fix, [update the machine type by adding another worker node](cs_cluster_update.html#machine_type).

<br />


## Adding non-root user access to persistent storage fails
{: #cs_storage_nonroot}

{: tsSymptoms}
After [adding non-root user access to persistent storage](cs_storage.html#nonroot) or deploying a Helm chart with a non-root user ID specified, the user cannot write to the mounted storage.

{: tsCauses}
The deployment or Helm chart configuration specifies the [security context](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/) for the pod's `fsGroup` (group ID) and `runAsUser` (user ID). Currently, {{site.data.keyword.containershort_notm}} does not support the `fsGroup` specification, and only supports `runAsUser` set as `0` (root permissions).

{: tsResolve}
Remove the configuration's `securityContext` fields for `fsGroup` and `runAsUser` from the image, deployment or Helm chart configuration file and redeploy. If you need to change the ownership of the mount path from `nobody`, [add non-root user access](cs_storage.html#nonroot).

<br />


## Getting help and support
{: #ts_getting_help}

Still having issues with your cluster?
{: shortdesc}

-   To see whether {{site.data.keyword.Bluemix_notm}} is available, [check the {{site.data.keyword.Bluemix_notm}} status page ![External link icon](../icons/launch-glyph.svg "External link icon")](https://developer.ibm.com/bluemix/support/#status).
-   Post a question in the [{{site.data.keyword.containershort_notm}} Slack. ![External link icon](../icons/launch-glyph.svg "External link icon")](https://ibm-container-service.slack.com) Tip: If you are not using an IBM ID for your {{site.data.keyword.Bluemix_notm}} account, [request an invitation](https://bxcs-slack-invite.mybluemix.net/) to this Slack.
{: tip}
-   Review the forums to see whether other users ran into the same issue. When you use the forums to ask a question, tag your question so that it is seen by the {{site.data.keyword.Bluemix_notm}} development teams.

    -   If you have technical questions about developing or deploying clusters or apps with {{site.data.keyword.containershort_notm}}, post your question on [Stack Overflow ![External link icon](../icons/launch-glyph.svg "External link icon")](https://stackoverflow.com/questions/tagged/ibm-cloud+containers) and tag your question with `ibm-cloud`, `kubernetes`, and `containers`.
    -   For questions about the service and getting started instructions, use the [IBM developerWorks dW Answers ![External link icon](../icons/launch-glyph.svg "External link icon")](https://developer.ibm.com/answers/topics/containers/?smartspace=bluemix) forum. Include the `ibm-cloud` and `containers` tags.
    See [Getting help](/docs/get-support/howtogetsupport.html#using-avatar) for more details about using the forums.

-   Contact IBM Support by opening a ticket. For information about opening an IBM support ticket, or about support levels and ticket severities, see [Contacting support](/docs/get-support/howtogetsupport.html#getting-customer-support).

{:tip}
When reporting an issue, include your cluster ID. To get your cluster ID, run `bx cs clusters`.

