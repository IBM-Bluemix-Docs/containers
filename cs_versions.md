---

copyright:
  years: 2014, 2018
lastupdated: "2018-04-17"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}

# Major and minor Kubernetes versions
{: #cs_versions}

{{site.data.keyword.containerlong}} concurrently supports multiple versions of Kubernetes. When a latest version (n) is released, versions up to 2 behind (n-2) are supported. Versions more than 2 behind the latest (n-3) are first deprecated and then unsupported.
{:shortdesc}

The current supported Kubernetes versions are:

- Latest: 1.9.3
- Default: 1.8.8
- Supported: 1.7.4

**Deprecated Versions**: When clusters are running on a deprecated Kubernetes, you have 30 days to review and update to a supported Kubernetes version before the version becomes unsupported. During the deprecation period, you can run limited commands in your clusters to add workers, reload workers, and update the cluster. You cannot create new clusters in the deprecated version.

**Unsupported Versions**: If you are running clusters on a Kubernetes version that is not supported, [review potential impacts](#version_types) for updates and then immediately [update your cluster](cs_cluster_update.html#update) to continue receiving important security updates and support.

To check the server version, run the following command.

```
kubectl version  --short | grep -i server
```
{: pre}

Example output:

```
Server Version: 1.8.8
```
{: screen}


## Update types
{: #version_types}

Your Kubernetes cluster has three types of updates: major, minor, and patch.
{:shortdesc}

|Update type|Examples of version labels|Updated by|Impact
|-----|-----|-----|-----|
|Major|1.x.x|You|Operation changes for clusters, including scripts or deployments.|
|Minor|x.9.x|You|Operation changes for clusters, including scripts or deployments.|
|Patch|x.x.4_1510|IBM and you|Kubernetes patches, as well as other {{site.data.keyword.Bluemix_notm}} Provider component updates such as security and operating system patches. IBM updates masters automatically, but you apply patches to worker nodes.|
{: caption="Impacts of Kubernetes updates" caption-side="top"}

As updates become available, you are notified when you view information about the worker nodes, such as with the `bx cs workers <cluster>` or `bx cs worker-get <cluster> <worker>` commands.
-  **Major and minor updates**: First [update your master node](cs_cluster_update.html#master) and then [update the worker nodes](cs_cluster_update.html#worker_node). 
   - By default, you cannot update a Kubernetes master more than two minor versions ahead. For example, if your current master is version 1.5 and you want to update to 1.8, you must update to 1.7 first. You can force the update to continue, but updating more than two minor versions might cause unexpected results.
   - If you use a `kubectl` CLI version that does match at least the `major.minor` version of your clusters, you might experience unexpected results. Make sure to keep your Kubernetes cluster and [CLI versions](cs_cli_install.html#kubectl) up-to-date.
-  **Patch updates**: Check monthly to see if an update is available, and use the `bx cs worker-update` [command](cs_cli_reference.html#cs_worker_update) to apply these security and operating system patches.

The following information summarizes updates that are likely to have impact on deployed apps when you update a cluster to a new version from the previous version. Review the [Kubernetes changelog ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG.md) for a complete list of changes in Kubernetes versions.
-  Version 1.9 [minor update](#cs_v19).
-  Version 1.8 [minor update](#cs_v18).
-  Version 1.7 [minor update](#cs_v17).
-  [Archive](#k8s_version_archive) of deprecated or unsupported versions.


## Version 1.9
{: #cs_v19}

<p><img src="images/certified_kubernetes_1x9.png" style="padding-right: 10px;" align="left" alt="This badge indicates Kubernetes version 1.9 certification for IBM Cloud Container Service."/> {{site.data.keyword.containerlong_notm}} is a Certified Kubernetes product for version 1.9 under the CNCF Kubernetes Software Conformance Certification program. _Kubernetes® is a registered trademark of The Linux Foundation in the United States and other countries, and is used pursuant to a license from The Linux Foundation._</p>

Review changes that you might need to make when you are updating from the previous Kubernetes version to 1.9.

<br/>

### Update before master
{: #19_before}

<table summary="Kubernetes updates for version 1.9">
<caption>Changes to make before you update the master to Kubernetes 1.9</caption>
<thead>
<tr>
<th>Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Webhook admission API</td>
<td>The admission API, which is used when the API server calls admission control webhooks, is moved from <code>admission.v1alpha1</code> to <code>admission.v1beta1</code>. <em>You must delete any existing webhooks before you upgrade your cluster</em>, and update the webhook configuration files to use the latest API. This change is not backward compatible.</td>
</tr>
</tbody>
</table>

### Update after master
{: #19_after}

<table summary="Kubernetes updates for version 1.9">
<caption>Changes to make after you update the master to Kubernetes 1.9</caption>
<thead>
<tr>
<th>Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>`kubectl` output</td>
<td>Now, when you use the `kubectl` command to specify `-o custom-columns` and the column is not found in the object, you see an output of `<none>`.<br>
Previously, the operation failed and you saw the error message `xxx is not found`. If your scripts rely on the previous behavior, update them.</td>
</tr>
<tr>
<td>`kubectl patch`</td>
<td>Now, when no changes are made to the resource that is patched, the `kubectl patch` command fails with `exit code 1`. If your scripts rely on the previous behavior, update them.</td>
</tr>
<tr>
<td>Kubernetes dashboard permissions</td>
<td>Users are required to log in to the Kubernetes dashboard with their credentials to view cluster resources. The default Kubernetes dashboard `ClusterRoleBinding` RBAC authorization is removed. For instructions, see [Launching the Kubernetes dashboard](cs_app.html#cli_dashboard).</td>
</tr>
<tr>
<td>Taints and tolerations</td>
<td>The `node.alpha.kubernetes.io/notReady` and `node.alpha.kubernetes.io/unreachable` taints were changed to `node.kubernetes.io/not-ready` and `node.kubernetes.io/unreachable` respectively.<br>
Although the taints are updated automatically, you must manually update the tolerations for these taints. For each namespace except `ibm-system` and `kube-system`, determine whether you need to change tolerations:<br>
<ul><li><code>kubectl get pods -n &lt;namespace&gt; -o yaml | grep "node.alpha.kubernetes.io/notReady" && echo "Action required"</code></li><li>
<code>kubectl get pods -n &lt;namespace&gt; -o yaml | grep "node.alpha.kubernetes.io/unreachable" && echo "Action required"</code></li></ul><br>
If `Action required` is returned, modify the pod tolerations accordingly.</td>
</tr>
<tr>
<td>Webhook admission API</td>
<td>If you deleted existing webhooks before you updated the cluster, create new webhooks.</td>
</tr>
</tbody>
</table>

<br />


## Version 1.8
{: #cs_v18}

<p><img src="images/certified_kubernetes_1x8.png" style="padding-right: 10px;" align="left" alt="This badge indicates Kubernetes version 1.8 certification for IBM Cloud Container Service."/> {{site.data.keyword.containerlong_notm}} is a Certified Kubernetes product for version 1.8 under the CNCF Kubernetes Software Conformance Certification program. _Kubernetes® is a registered trademark of The Linux Foundation in the United States and other countries, and is used pursuant to a license from The Linux Foundation._</p>

Review changes that you might need to make when you are updating from the previous Kubernetes version to 1.8.

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
<td>None</td>
<td>No changes are required before you update the master</td>
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
<td>The URL for accessing the Kubernetes dashboard in version 1.8 changed, and the login process includes a new authentication step. See [accessing the Kubernetes dashboard](cs_app.html#cli_dashboard) for more information.</td>
</tr>
<tr>
<td>Kubernetes dashboard permissions</td>
<td>To force users to log in with their credentials to view cluster resources in version 1.8, remove the 1.7 ClusterRoleBinding RBAC authorization. Run `kubectl delete clusterrolebinding -n kube-system kubernetes-dashboard`.</td>
</tr>
<tr>
<td>`kubectl delete`</td>
<td>The `kubectl delete` command no longer scales down workload API objects, like pods, before the object is deleted. If you require the object to scale down, use [`kubectl scale` ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#scale) before you delete the object.</td>
</tr>
<tr>
<td>`kubectl run`</td>
<td>The `kubectl run` command must use multiple flags for `--env` instead of comma-separated arguments. For example, run <code>kubectl run --env &lt;x&gt;=&lt;y&gt; --env &lt;z&gt;=&lt;a&gt;</code> and not <code>kubectl run --env &lt;x&gt;=&lt;y&gt;,&lt;z&gt;=&lt;a&gt;</code>. </td>
</tr>
<tr>
<td>`kubectl stop`</td>
<td>The `kubectl stop` command is no longer available.</td>
</tr>
</tbody>
</table>

<br />


## Version 1.7
{: #cs_v17}

<p><img src="images/certified_kubernetes_1x7.png" style="padding-right: 10px;" align="left" alt="This badge indicates Kubernetes version 1.7 certification for IBM Cloud Container Service."/> {{site.data.keyword.containerlong_notm}} is a Certified Kubernetes product for version 1.7 under the CNCF Kubernetes Software Conformance Certification program.</p>

Review changes that you might need to make when you are updating from the previous Kubernetes version to 1.7.

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
<td>Deployment `apiVersion`</td>
<td>After you update the cluster from Kubernetes 1.5, use `apps/v1beta1` for the `apiVersion` field in new `Deployment` YAML files. Continue to use `extensions/v1beta1` for other resources, such as `Ingress`.</td>
</tr>
<tr>
<td>kubectl</td>
<td>After the `kubectl` CLI update, these `kubectl create` commands must use multiple flags instead of comma-separated arguments:<ul>
 <li>`role`
 <li>`clusterrole`
 <li>`rolebinding`
 <li>`clusterrolebinding`
 <li>`secret`
 </ul>
</br>  For example, run `kubectl create role --resource-name <x> --resource-name <y>` and not `kubectl create role --resource-name <x>,<y>`.</td>
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
</td></tr>
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
</td></tr>
<tr>
<td>RBAC for `default` `ServiceAccount`</td>
<td><p>The administrator `ClusterRoleBinding` for the `default` `ServiceAccount` in the `default` namespace is removed to improve cluster security. Applications that run in the `default` namespace no longer have cluster administrator privileges to the Kubernetes API, and might encounter `RBAC DENY` permission errors. Check your app and its `.yaml` file to see whether it runs in the `default` namespace, uses the `default ServiceAccount`, and accesses the Kubernetes API.</p>
<p>If your applications rely on these privileges, [create RBAC authorization resources![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/admin/authorization/rbac/#api-overview) for your apps.</p>
  <p>As you update your app RBAC policies, you might want to revert temporarily to the previous `default`. Copy, save, and apply the following files with the `kubectl apply -f FILENAME` command. <strong>Note</strong>: Revert to give yourself time to update all your application resources, and not as a long-term solution.</p>

<p><pre class="codeblock">
<code>
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
</code>
</pre></p>
</td>
</tr>
<tr>
<td>StatefulSet pod DNS</td>
<td>StatefulSet pods lose their Kubernetes DNS entries after updating the master. To restore the DNS entries, delete the StatefulSet pods. Kubernetes re-creates the pods and automatically restores the DNS entries. For more information, see the [Kubernetes issue ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/kubernetes/kubernetes/issues/48327).</td>
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
</td></tr>
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
</td></tr>
</tbody>
</table>

<br />


## Archive
{: #k8s_version_archive}

### Version 1.5 (Unsupported)
{: #cs_v1-5}

As of 4 April 2018, {{site.data.keyword.containershort_notm}} clusters that run [Kubernetes version 1.5](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG-1.5.md) are unsupported. Version 1.5 clusters cannot receive security updates or support unless they are updated to the next most recent version ([Kubernetes 1.7](#cs_v17)).

[Review potential impact](cs_versions.html#cs_versions) of each Kubernetes version update, and then [update your clusters](cs_cluster_update.html#update) immediately. Note that you update from one version to the next most recent, such as 1.5 to 1.7 or 1.8 to 1.9.
