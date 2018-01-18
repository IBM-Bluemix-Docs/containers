---

copyright:
  years: 2014, 2018
lastupdated: "2018-01-15"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}

# Kubernetes versions for {{site.data.keyword.containerlong_notm}}
{: #cs_versions}

Review the Kubernetes versions that are available on {{site.data.keyword.containerlong}}.
{:shortdesc}

{{site.data.keyword.containershort_notm}} supports several versions of Kubernetes. The default version is used when you create or update a cluster, unless you specify a different version. The available Kubernetes versions are:

- 1.8.6 (Default version)
- 1.7.4
- 1.5.6


To check which version of the Kubernetes CLI that you are running locally or that your cluster is running, run the following command and check the version.

```
kubectl version  --short
```
{: pre}

Example output:

```
Client Version: 1.8.6
Server Version: 1.8.6
```
{: screen}

## Update types
{: #version_types}

Kubernetes provides these version update types:
{:shortdesc}

|Update type|Examples of version labels|Updated by|Impact
|-----|-----|-----|-----|
|Major|1.x.x|You|Operation changes for clusters, including scripts or deployments.|
|Minor|x.5.x|You|Operation changes for clusters, including scripts or deployments.|
|Patch|x.x.3|IBM and you|No changes to scripts or deployments. IBM updates masters automatically, but you apply patches to worker nodes.|
{: caption="Impacts of Kubernetes updates" caption-side="top"}

By default, you cannot update a Kubernetes master more than two minor versions ahead. For example, if your current master is version 1.5 and you want to update to 1.8, you must update to 1.7 first. You can force the update to continue, but updating more than two minor versions might cause unexpected results.
{: tip}

The following information summarizes updates that are likely to have impact on deployed apps when you update a cluster to a new version from the previous version. Review the [Kubernetes changelog ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG.md) for a complete list of changes in Kubernetes versions.

For more information on the updating process, see [Updating clusters](cs_cluster_update.html#master) and [Updating worker nodes](cs_cluster_update.html#worker_node).

## Version 1.8
{: #cs_v18}

<p><img src="images/certified_kubernetes_1x8.png" style="width:62px; height: 100px; border-style: none; padding-right: 10px;" height="100" width="62.5" align="left" alt="This badge indicates Kubernetes version 1.8 certification for IBM Cloud Container Service."/> {{site.data.keyword.containerlong_notm}} is a Certified Kubernetes product for version 1.8 under the CNCF Kubernetes Software Conformance Certification program. _Kubernetes® is a registered trademark of The Linux Foundation in the United States and other countries, and is used pursuant to a license from The Linux Foundation._</p>

Review changes you might need to make when updating from the previous Kubernetes version to 1.8.

<br/>

### Update before master
{: #18_before}

<table summary="Kubernetes updates for versions 1.8">
<caption>Changes to make before you update the master to Kubernetes 1.8</caption>
<thead>
<tr>
<th>Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td colspan='2'>No changes required before you update the master</td>
</tr>
</tbody>
</table>

### Update after master
{: #18_after}

<table summary="Kubernetes updates for versions 1.8">
<caption>Changes to make after you update the master to Kubernetes 1.8</caption>
<thead>
<tr>
<th>Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes dashboard login</td>
<td>The URL for accessing the Kubernetes dashboard in version 1.8 has changed, and the login process includes a new authentication step. See [accessing the Kubernetes dashboard](cs_app.html#cli_dashboard) for more information.</td>
</tr>
<tr>
<td>Kubernetes dashboard permissions</td>
<td>To force users to log in with their credentials to view cluster resources in version 1.8, remove the 1.7 ClusterRoleBinding RBAC authorization. Run `kubectl delete clusterrolebinding -n kube-system kubernetes-dashboard`.</td>
</tr>
<tr>
<td>`kubectl delete`</td>
<td>The `kubectl delete` command no longer scales down workload API objects, like pods, before the object is deleted. If you require the object to scale down, use [kubectl scale ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#scale) before you delete the object.</td>
</tr>
<tr>
<td>`kubectl run`</td>
<td>The `kubectl run` command must use multiple flags for `--env` instead of comma separated arguments. For example, run <code>kubectl run --env &lt;x&gt;=&lt;y&gt; --env &lt;z&gt;=&lt;a&gt;</code> and not <code>kubectl run --env &lt;x&gt;=&lt;y&gt;,&lt;z&gt;=&lt;a&gt;</code>. </td>
</tr>
<tr>
<td>`kubectl stop`</td>
<td>The `kubectl stop` command is no longer available.</td>
</tr>
</tbody>
</table>


## Version 1.7
{: #cs_v17}

<p><img src="images/certified_kubernetes_1x7.png" height="100" width="62.5" style="width:62px; height: 100px; border-style: none; padding-right: 10px;" align="left" alt="This badge indicates Kubernetes version 1.7 certification for IBM Cloud Container Service."/> {{site.data.keyword.containerlong_notm}} is a Certified Kubernetes product for version 1.7 under the CNCF Kubernetes Software Conformance Certification program.</p>

Review changes you might need to make when updating from the previous Kubernetes version to 1.7.

<br/>

### Update before master
{: #17_before}

<table summary="Kubernetes updates for versions 1.7 and 1.6">
<caption>Changes to make before you update the master to Kubernetes 1.7</caption>
<thead>
<tr>
<th>Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Storage</td>
<td>Configuration scripts with `hostPath` and `mountPath` with parent directory references like `../to/dir` are not allowed. Change paths to simple absolute paths, for example, `/path/to/dir`.
<ol>
  <li>Determine whether you need to change storage paths:</br>
  ```
  kubectl get pods --all-namespaces -o yaml | grep "\.\." && echo "Action required"
  ```
  </br>

  <li>If `Action required` is returned, change the pods to reference the absolute path before you update all of your worker nodes. If the pod is owned by another resource, such as a deployment, change the [_PodSpec_ ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/api-reference/v1.7/#podspec-v1-core) within that resource.
</ol>
</td>
</tr>
</tbody>
</table>

### Update after master
{: #17_after}

<table summary="Kubernetes updates for versions 1.7 and 1.6">
<caption>Changes to make after you update the master to Kubernetes 1.7</caption>
<thead>
<tr>
<th>Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>kubectl</td>
<td>After the `kubectl` CLI update, these `kubectl create` commands must use multiple flags instead of comma separated arguments:<ul>
 <li>`role`
 <li>`clusterrole`
 <li>`rolebinding`
 <li>`clusterrolebinding`
 <li>`secret`
 </ul>
</br>  For example, run `kubectl create role --resource-name <x> --resource-name <y>` and not `kubectl create role --resource-name <x>,<y>`.</td>
</tr>
<tr>
<td>Pod Affinity Scheduling</td>
<td> The `scheduler.alpha.kubernetes.io/affinity` annotation is deprecated.
<ol>
  <li>For each namespace except `ibm-system` and `kube-system`, determine whether you need to update pod affinity scheduling:</br>
  ```
  kubectl get pods -n <namespace> -o yaml | grep "scheduler.alpha.kubernetes.io/affinity" && echo "Action required"
  ```
  </br></li>
  <li>If `"Action required"` is returned, use the [_PodSpec_ ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/api-reference/v1.7/#podspec-v1-core) _affinity_ field instead of the `scheduler.alpha.kubernetes.io/affinity` annotation.</li>
</ol>
</tr>
<tr>
<td>Network Policy</td>
<td>The `net.beta.kubernetes.io/network-policy` annotation is no longer available.
<ol>
  <li>Determine whether you need to change policies:</br>
  ```
  kubectl get ns -o yaml | grep "net.beta.kubernetes.io/network-policy" | grep "DefaultDeny" && echo "Action required"
  ```
  <li>If `"Action required"` is returned, add the following network policy to each Kubernetes namespace that was listed:</br>

  <pre class="codeblock">
  <code>
  kubectl create -n &lt;namespace&gt; -f - &lt;&lt;EOF
  kind: NetworkPolicy
  apiVersion: networking.k8s.io/v1
  metadata:
    name: default-deny
    namespace: &lt;namespace&gt;
  spec:
    podSelector: {}
  EOF
  </code>
  </pre>

  <li> After adding the networking policy, remove the `net.beta.kubernetes.io/network-policy` annotation:
  ```
  kubectl annotate ns <namespace> --overwrite "net.beta.kubernetes.io/network-policy-"
  ```
  </li></ol>
</tr>
<tr>
<td>Tolerations</td>
<td>The `scheduler.alpha.kubernetes.io/tolerations` annotation is no longer available.
<ol>
  <li>For each namespace except `ibm-system` and `kube-system`, determine whether you need to change tolerations:</br>
  ```
  kubectl get pods -n <namespace> -o yaml | grep "scheduler.alpha.kubernetes.io/tolerations" && echo "Action required"
  ```
  </br>

  <li>If `"Action required"` is returned, use the [_PodSpec_ ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/api-reference/v1.7/#podspec-v1-core) _tolerations_ field instead of the `scheduler.alpha.kubernetes.io/tolerations` annotation
</ol>
</tr>
<tr>
<td>Taints</td>
<td>The `scheduler.alpha.kubernetes.io/taints` annotation is no longer available.
<ol>
  <li>Determine whether you need to change taints:</br>
  ```
  kubectl get nodes -o yaml | grep "scheduler.alpha.kubernetes.io/taints" && echo "Action required"
  ```
  <li>If `"Action required"` is returned, remove the `scheduler.alpha.kubernetes.io/taints` annotation for each node:</br>
  `kubectl annotate nodes <node> scheduler.alpha.kubernetes.io/taints-`
  <li>Add a taint to each node:</br>
  `kubectl taint node <node> <taint>`
  </li></ol>
</tr>
<tr>
<td>StatefulSet pod DNS</td>
<td>StatefulSet pods lose their Kubernetes DNS entries after updating the master. To restore the DNS entries, delete the StatefulSet pods. Kubernetes re-creates the pods and automatically restores the DNS entries. For more information, see the [Kubernetes issue ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/kubernetes/kubernetes/issues/48327).
</tr>
</tbody>
</table>
