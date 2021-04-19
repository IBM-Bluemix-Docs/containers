---

copyright:
  years: 2014, 2021
lastupdated: "2021-04-15"

keywords: kubernetes, iks, helm

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
 

# Adding services by using managed add-ons
{: #managed-addons}

Quickly add extra capabilities and open-source technologies to your cluster with managed add-ons.
{: shortdesc}

**What are managed add-ons?**

Managed {{site.data.keyword.containerlong_notm}} add-ons are an easy way to enhance your cluster with extra capabilities and open-source capabilities, such as Istio, Kubernetes web terminal, {{site.data.keyword.block_storage_is_short}}, or the Cluster Autoscaler. The version of the driver, plug-in, or open-source tool that you add to your cluster is tested by IBM and approved to be used in {{site.data.keyword.containerlong_notm}}.

The managed add-ons that you can install in your cluster depend on the type of cluster, the container platform, and the infrastructure provider that you choose.
{: note}

**How does the billing and support work for managed add-ons?**

Managed add-ons are fully integrated into the {{site.data.keyword.cloud_notm}} support organization. If you have a question or an issue with using the managed add-ons, you can use one of the {{site.data.keyword.containerlong_notm}} support channels. For more information, see [Getting help and support](/docs/containers?topic=containers-get-help).

If the tool that you add to your cluster incurs costs, these costs are automatically integrated and listed as part of your {{site.data.keyword.containerlong_notm}} billing. The billing cycle is determined by {{site.data.keyword.cloud_notm}} depending on when you enabled the add-on in your cluster.

**What limitations do I need to account for?**

If you installed an admission controller that blocks unsigned images, such as [Portieris](/docs/openshift?topic=openshift-images#portieris-image-sec), you cannot enable managed add-ons in your cluster.

## Adding managed add-ons
{: #adding-managed-add-ons}

To enable a managed add-on in your cluster from the CLI, use the [`ibmcloud ks cluster addon enable` command](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_addon_enable). To enable a managed add-on in your cluster in the [Kubernetes clusters console](https://cloud.ibm.com/kubernetes/clusters){: external}, use the **Add-ons** pane of the cluster details page. When you enable the managed add-on, a supported version of the tool, including all Kubernetes resources are automatically installed in your cluster. Refer to the documentation of each managed add-on to find the prerequisites that your cluster must meet to install the managed add-on.

For more information about the prerequisites for each add-on, see:
- [ALB OAuth Proxy](/docs/containers?topic=containers-comm-ingress-annotations#app-id)
- [{{site.data.keyword.block_storage_is_short}}](/docs/containers?topic=containers-vpc-block)
- [Cluster Autoscaler](/docs/openshift?topic=openshift-ca)
- [Diagnostics and Debug Tool](/docs/containers?topic=containers-cs_troubleshoot#debug_utility)
- [Istio](/docs/containers?topic=containers-istio)
- [Kubernetes web terminal](/docs/containers?topic=containers-cs_cli_install#cli_web)
- [Static routes](/docs/containers?topic=containers-static-routes)

## Updating managed add-ons
{: #updating-managed-add-ons}

The versions of each managed add-on are tested by {{site.data.keyword.cloud_notm}} and approved for use in {{site.data.keyword.containerlong_notm}}. To update the components of an add-on to the most recent version supported by {{site.data.keyword.containerlong_notm}}, use the following steps.
{: shortdesc}

1. Check for update instructions that are specific to your managed add-on. If you do not find update instructions, continue with the next step.<p class="important">The Istio add-on requires specific update steps, and some Istio add-on versions include breaking changes. Ensure that you follow the steps in [Updating the Istio add-on](/docs/containers?topic=containers-istio#istio_update)) to update your Istio add-on.</p>
2. If your add-on does not have specific update instructions, select the cluster where you installed managed add-ons from your [cluster dashboard](https://cloud.ibm.com/kubernetes/clusters).
3. Select the **Add-ons** tab.
4. From the actions menu, select **Update** to start updating the managed add-on. When the update is installed, the latest version of the managed add-on is listed on the cluster add-on page.

<br />

## Reviewing add-on states and statuses
{: #debug_addons_review}

You can check the health state and status of a cluster add-on by running the following command:
```
ibmcloud ks cluster addon ls -c <cluster_name_or_ID>
```
{: pre}

Example output:
```
Name            Version   Health State   Health Status
debug-tool      2.0.0     normal         Addon Ready
istio           1.4       -              Enabling
kube-terminal   1.0.0     normal         Addon Ready
```
{: screen}

The **Health State** reflects the lifecycle of the add-on components. The **Health Status** provides details of what add-on operation is in progress. Each state and status is described in the following tables.

|Add-on health state|Description|
|--- |--- |
|`critical`|The add-on is not ready to be used for one of the following reasons:<ul><li>Some or all of the add-on components are unhealthy.</li><li>The add-on failed to deploy.</li><li>The add-on may be at an unsupported version.</li></ul>Check the **Health Status** field for more information.|
|`normal`|The add-on is successfully deployed. Check the status to verify that the add-on is `Ready` or to see if an update is available.|
|`pending`|Some or all components of the add-on are currently deploying. Wait for the state to become `normal` before working with your add-on.|
|`updating`|The add-on is updating and is not ready to be used. Check the **Health Status** field for the version that the add-on is updating to.|
|`warning`|The add-on might not function properly due to cluster limitations. Check the **Health Status** field for more information.|
{: caption="Add-on health states"}
{: summary="Table rows read from left to right, with the add-on state in column one and a description in column two."}

</br>

|Status code|Add-on health status|Description|
|--- |--- |--- |
|H1500|`Addon Ready`|The add-on is successfully deployed and is healthy.|
|H1501, H1502, H1503|`Addon Not Ready`|Some or all of the add-on components are unhealthy. Check whether all add-on component pods are running. For example, for the Istio add-on, check whether all pods in the `istio-system` namespace are `Running` by running `kubectl get pods -n istio-system`.|
|H1504, H1505, H1506, H1507, H1508|`Failure determining health status.`|The add-on health cannot be determined. [Open a support case](/docs/get-support?topic=get-support-using-avatar). In the description, include the error code from the health status.|
|H1509|`Addon Unsupported`|The add-on runs an unsupported version, or the add-on version is unsupported for your cluster version. [Update your add-on to the latest version](/docs/containers?topic=containers-managed-addons#updating-managed-add-ons), or see specific update steps for [Istio](/docs/containers?topic=containers-istio#istio_update).|
|H1510|`Cluster resources low, not enough workers in Ready state.`|The add-on is not ready to be used for one of the following reasons:<ul><li>The cluster does not meet the size criteria for the add-on. For example, check the size requirements for [Istio](/docs/containers?topic=containers-istio#istio_install).</li><li>Worker nodes in your cluster are not in a `Normal` state. [Review the worker nodes' state and status](/docs/containers?topic=containers-cs_troubleshoot_clusters#debug_worker_nodes).</li></ul>|
|-|`Enabling`|The add-on is currently deploying to the cluster. Note that the add-on might take up to 15 minutes to install.|
|H1511|`Istio data plane components may need to be updated.`| When the Istio control plane is updated, the Istio data plane components are not updated automatically. Whenever the Istio add-on is updated to a new patch or minor version, you must [manually update your data plane components](/docs/containers?topic=containers-istio#update_client_sidecar), including the `istioctl` client and the Istio sidecars for your app.|
|H1512|`Addon daemonset may not be available on all Ready nodes.`|For the static route add-on: The static route operator `DaemonSet` is not available on any worker nodes, which prevents you from applying static route resources. Your worker nodes cannot run the static route operator `DaemonSet` for the following reasons:<ul><li>One or more worker nodes reached their [resource limits](/docs/containers?topic=containers-cs_troubleshoot_clusters#debug_worker_nodes).</li><li>One or more worker nodes are running the [maximum number of pods per worker node](/docs/containers?topic=containers-limitations#classic_limits).</li></ul>|
{: caption="Add-on health statuses"}
{: summary="Table rows read from left to right, with the add-on status in column one and a description in column two."}

Still having trouble with the Istio add-on? Check out these [troubleshooting topics](/docs/containers?topic=containers-istio#istio-ts) to debug your Istio deployment.
{: tip}




