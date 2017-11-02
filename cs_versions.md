---

copyright:
  years: 2014, 2017
lastupdated: "2017-11-02"

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
- 1.8.2
- 1.7.4 (Default version)
- 1.5.6

The table contains updates that are likely to have impact on deployed apps when you update a cluster to a new version. Review the [Kubernetes changelog ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG.md) for a complete list of changes in Kubernetes versions.

For more information on the updating process, see [Updating clusters](cs_cluster.html#cs_cluster_update) and [Updating worker nodes](cs_cluster.html#cs_cluster_worker_update).

## Version 1.8
{: #cs_v18}

### Update before master
{: #18_before}

<table summary="Kubernetes updates for versions 1.8">
<caption>Changes to make before you update the master to Kubernetes 1.8</caption>
<thead>
<tr>
<th>Type</th>
<th>Description
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
<th>Description
</tr>
</thead>
<tbody>
<tr>
<td>Kubernetes dashboard login</td>
<td>The URL for accessing the Kubernetes dashboard in version 1.8 has changed, and the login process includes a new authentication step. See [accessing the Kubernetes dashboard](cs_cluster.html#cs_cli_dashboard) for more information.</td>
</tr>
<tr>
<tr>
<td>Kubernetes dashboard permissions</td>
<td>To force users to log in with their credentials to view cluster resources in version 1.8, remove the 1.7 ClusterRoleBinding RBAC authorization. Run `kubectl delete clusterrolebinding -n kube-system kubernetes-dashboard`.</td>
</tr>
<tr>
<td>`kubectl delete`</td>
<td>The `kubectl delete` command no longer scales down workload API objects, like pods, before the object is deleted. If you require the object to scale down, use [kubectl scale ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/user-guide/kubectl/v1.8/#scale) before you delete the object.</td>
</tr>
<tr>
<td>`kubectl run`</td>
<td>The `kubectl run` command must use multiple flags for `--env` instead of comma separated arguments. For example, run `kubectl run --env <x>=<y> --env <z>=<a>` and not `kubectl run --env <x>=<y>,<z>=<a>`.</td>
</tr>
<td>`kubectl stop`</td>
<td>The `kubectl stop` command is no longer available.</td>
</tr>
</tbody>
</table>

## Version 1.7
{: #cs_v17}

### Update before master
{: #17_before}

<table summary="Kubernetes updates for versions 1.7 and 1.6">
<caption>Changes to make before you update the master to Kubernetes 1.7</caption>
<thead>
<tr>
<th>Type</th>
<th>Description
</tr>
</thead>
<tbody>
<tr>
<td>Storage</td>
<td>Configuration scripts with `hostPath` and `mountPath` with parent directory references like `../to/dir` are not allowed. Change paths to simple absolute paths, for example, `/path/to/dir`.
<ol>
  <li>Run this command to determine whether you need to update your storage paths.</br>
  ```
  kubectl get pods --all-namespaces -o yaml | grep "\.\." && echo "Action required"
  ```
  </br>

  <li>If `Action required` is returned, modify each impacted pod to reference the absolute path before you update all of your worker nodes. If the pod is owned by another resource, such as a deployment, modify the [_PodSpec_ ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/api-reference/v1.7/#podspec-v1-core) within that resource.
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
<th>Description
</tr>
</thead>
<tbody>
<tr>
<td>kubectl</td>
<td>After you update `kubectl` CLI to the version of your cluster, these `kubectl` commands must use multiple flags instead of comma separated arguments. <ul>
 <li>`create role`
 <li>`create clusterrole`
 <li>`create rolebinding`
 <li>`create clusterrolebinding`
 <li>`create secret`
 </ul>
</br>  For example, run `kubectl create role --resource-name <x> --resource-name <y>` and not `kubectl create role --resource-name <x>,<y>`.</td>
</tr>
<tr>
<td>Pod Affinity Scheduling</td>
<td> The `scheduler.alpha.kubernetes.io/affinity` annotation is deprecated.
<ol>
  <li>Run this command for each namespace except for `ibm-system` and `kube-system` to determine whether you need to update pod affinity scheduling.</br>
  ```
  kubectl get pods -n <namespace> -o yaml | grep "scheduler.alpha.kubernetes.io/affinity" && echo "Action required"
  ```
  </br>
  <li>If `"Action required"` is returned, modify the impacted pods to use the [_PodSpec_ ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/api-reference/v1.7/#podspec-v1-core) _affinity_ field instead of the `scheduler.alpha.kubernetes.io/affinity` annotation.
</ol>
</tr>
<tr>
<td>Network Policy</td>
<td>The `net.beta.kubernetes.io/network-policy` annotation is no longer supported.
<ol>
  <li>Run this command to determine whether you need to update your network policies.</br>
  ```
  kubectl get ns -o yaml | grep "net.beta.kubernetes.io/network-policy" | grep "DefaultDeny" && echo "Action required"
  ```
  <li>If `Action required` returns, add the following network policy to each Kubernetes namespace that was listed.</br>

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

  <li> With the network policy in place, remove the `net.beta.kubernetes.io/network-policy` annotation.
  ```
  kubectl annotate ns <namespace> --overwrite "net.beta.kubernetes.io/network-policy-"
  ```
  </ol>
</tr>
<tr>
<td>Tolerations</td>
<td>The `scheduler.alpha.kubernetes.io/tolerations` annotation is no longer supported.
<ol>
  <li>Run this command for each namespace except for `ibm-system` and `kube-system` to determine whether you need to update tolerations.</br>
  ```
  kubectl get pods -n <namespace> -o yaml | grep "scheduler.alpha.kubernetes.io/tolerations" && echo "Action required"
  ```
  </br>

  <li>If `"Action required"` is returned, modify the impacted pods to use the [_PodSpec_ ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/api-reference/v1.7/#podspec-v1-core) _tolerations_ field instead of the `scheduler.alpha.kubernetes.io/tolerations` annotation
</ol>
</tr>
<tr>
<td>Taints</td>
<td>The `scheduler.alpha.kubernetes.io/taints` annotation is no longer supported.
<ol>
  <li>Run this command to determine whether you need to update taints. </br>
  ```
  kubectl get nodes -o yaml | grep "scheduler.alpha.kubernetes.io/taints" && echo "Action required"
  ```
  <li>If `"Action required"` is returned, remove the `scheduler.alpha.kubernetes.io/taints` annotation for each node that has the unsupported annotation.</br>
  `kubectl annotate nodes <node> scheduler.alpha.kubernetes.io/taints-`
  <li>When the unsupported annotation is removed, add a taint to each node. You must have `kubectl` CLI version 1.6 or later.</br>
  `kubectl taint node <node> <taint>`
  </ol>
</tr>
</tbody>
</table>