---

copyright:
  years: 2014, 2018
lastupdated: "2018-4-25"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}



# Version changelog
{: #changelog}

View information of version changes for major, minor, and patch updates that are available for your {{site.data.keyword.containerlong}} Kubernetes clusters. Changes include updates to Kubernetes and {{site.data.keyword.Bluemix_notm}} Provider components.
{:shortdesc}

IBM applies patch-level updates to your master automatically, but you must [update your worker nodes](cs_cluster_update.html#worker_node). Check monthly for available updates. As updates become available, you are notified when you view information about the worker nodes, such as with the `bx cs workers <cluster>` or `bx cs worker-get <cluster> <worker>` commands.

For a summary of migration actions, see [Kubernetes versions](cs_versions.html).
{: tip}

For information about changes since the previous version, see the following changelogs.
-  Version 1.8 [changelog](#18_changelog).
-  Version 1.7 [changelog](#17_changelog).


## Version 1.8 changelog
{: #18_changelog}

Review the following changes.

### Changelog for 1.8.11_1509
{: #1811_1509}

<table summary="Changes since version 1.8.8_1507">
<caption>Changes since version 1.8.8_1507</caption>
<thead>
<tr>
<th>Component</th>
<th>Previous</th>
<th>Current</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes</td>
<td>v1.8.8</td>
<td>v1.8.11	</td>
<td>See the [Kubernetes release notes![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/kubernetes/kubernetes/releases/tag/v1.8.11).</td>
</tr>
<tr>
<td>Pause container image</td>
<td>3.0</td>
<td>3.1</td>
<td>Removes inherited orphaned zombie processes.</td>
</tr>
<tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.8.8-86</td>
<td>v1.8.11-126</td>
<td>`NodePort` and `LoadBalancer` services now support [preserving the client source IP](cs_loadbalancer.html#node_affinity_tolerations) by setting `service.spec.externalTrafficPolicy` to `Local`.</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>Fix [edge node](cs_edge.html#edge) toleration setup for older clusters.</td>
</tr>
</tbody>
</table>

## Version 1.7 changelog
{: #17_changelog}

Review the following changes.

### Changelog for 1.7.16_1511
{: #1716_1511}

<table summary="Changes since version 1.7.4_1509">
<caption>Changes since version 1.7.4_1509</caption>
<thead>
<tr>
<th>Component</th>
<th>Previous</th>
<th>Current</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes</td>
<td>v1.7.4</td>
<td>v1.7.16	</td>
<td>See the [Kubernetes release notes![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/kubernetes/kubernetes/releases/tag/v1.7.16).</td>
</tr>
<td>{{site.data.keyword.Bluemix_notm}} Provider</td>
<td>v1.7.4-133</td>
<td>v1.7.16-17</td>
<td>`NodePort` and `LoadBalancer` services now support [preserving the client source IP](cs_loadbalancer.html#node_affinity_tolerations) by setting `service.spec.externalTrafficPolicy` to `Local`.</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>Fix [edge node](cs_edge.html#edge) toleration setup for older clusters.</td>
</tr>
</tbody>
</table>

