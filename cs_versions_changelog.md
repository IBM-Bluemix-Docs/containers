.1509
- Supported: 1.7.4.1511

IBM applies patch-level updates to your master automatically, but you must [update your worker nodes](cs_cluster_update.html#worker_nodes). Check monthly for available updates. As patch updates become available, you are notified when you view information about the worker nodes, such as with the `bx cs workers <cluster>` or `bx cs worker-get <cluster> <worker>` commands.

For information on other types of updates, see [Major and minor Kubernetes versions](cs_versions.html).
{: tip}

The following information summarizes the updates from the previous patch version.
-  Version 1.9 [patch changelog](#19_changelog).
-  Version 1.8 [patch changelog](#18_changelog).
-  Version 1.7 [patch changelog](#17_changelog).

## Version 1.9 patch changelog
{: #19_changelog}

Coming soon!

## Version 1.8 patch changelog
{: #18_changelog}

Review the following changes before making patch [updates to your Kubernetes 1.8 cluster](cs_cluster_update.html).

### Changelog for 1.8.11_1509
{: #1811_1509}

<table summary="Changes since version 1.8.8_1507">
<caption>Changes since version 1.8.8_1507</caption>
<thead>
<tr>
<th>Component</th>
<th>Previous version</th>
<th>Current version</th>
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
<td>{{site.data.keyword_Bluemix_notm}} Provider</td>
<td>v1.8.8-86</td>
<td>v1.8.11-126</td>
<td>`NodePort` and `LoadBalancer` services now support [preserving the client source IP](cs_app.html#node_affinity_tolerations) by `setting service.spec.externalTrafficPolicy` to `Local`.</td>
</tr>
</tbody>
</table>

## Version 1.7 patch changelog
{: #17_changelog}

Review the following changes before making patch [updates to your Kubernetes 1.7 cluster](cs_cluster_update.html).

### Changelog for 1.7.16_1511
{: #1716_1511}

<table summary="Changes since version 1.7.4_1509">
<caption>Changes since version 1.7.4_1509</caption>
<thead>
<tr>
<th>Component</th>
<th>Previous version</th>
<th>Current version</th>
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
<td>{{site.data.keyword_Bluemix_notm}} Provider</td>
<td>v1.7.4-133</td>
<td>v1.7.16-17</td>
<td>`NodePort` and `LoadBalancer` services now support [preserving the client source IP](cs_app.html#node_affinity_tolerations) by `setting service.spec.externalTrafficPolicy` to `Local`.</td>
</tr>
<tr>
<td>Fix [edge node](cs_edge.html#edge) toleration setup for older clusters.</td>
</tr>
</tbody>
</table>

</staging>
