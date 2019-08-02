---

copyright:
  years: 2014, 2019
lastupdated: "2019-07-31"

keywords: kubernetes, iks

subcollection: containers

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:preview: .preview}



# CLI changelog
{: #cs_cli_changelog}

In the terminal, you are notified when updates to the `ibmcloud` CLI and plug-ins are available. Be sure to keep your CLI up-to-date so that you can use all available commands and flags.
{:shortdesc}

* **Community Kubernetes**: [Install the CLI plug-in](/docs/containers?topic=containers-cs_cli_install#cs_cli_install_steps), which uses the `ibmcloud ks` alias.
* **OpenShift**: [Install the CLI plug-in](/docs/openshift?topic=openshift-openshift-cli), which uses the `ibmcloud oc` alias.

<br>
Refer to the following table for a summary of changes for each {{site.data.keyword.containerlong_notm}} CLI plug-in version.

<table summary="Overview of version changes for the {{site.data.keyword.containerlong_notm}} CLI plug-in">
<caption>Changelog for the {{site.data.keyword.containerlong_notm}} CLI plug-in</caption>
<thead>
<tr>
<th>Version</th>
<th>Release date</th>
<th>Changes</th>
</tr>
</thead>
<tbody>
<tr>
<td>0.3.95</td>
<td>30 Jul 2019</td>
<ul>
<li>Adds the `ibmcloud oc` alias to the {{site.data.keyword.containershort_notm}} plug-in for management of Red Hat OpenShift on IBM Cloud clusters.</li>
<td>Adds the [`ibmcloud ks cluster-subnet-detach`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_subnet_detach) command to detach a public or private portable subnet in an IBM Cloud infrastructure account from a cluster.<ul>
<li>Renames the `ibmcloud ks machine-types` command to `ibmcloud ks flavors`. You can still use the `machine-types` alias.</li>
<li>In the output of `ibmcloud ks flavors (machine-types)`, indicates flavors that are supported only for {{site.data.keyword.containerlong_notm}} or only for Red Hat OpenShift on IBM Cloud.</li>
<li>In the output of `ibmcloud ks cluster-get`, changes the term `Owner` to `Creator` to reflect that the field returns information about the user that created the cluster.</li>
<li>Improves error handling for `ibmcloud ks zone-add`.</li>
<li>Updates translations of help text.</li>
</ul>
</tr>
<tr>
<td>0.3.58</td>
<td>02 Jul 2019</td>
<td><ul>
<li>Fixes a bug so that a worker pool rebalance message is not returned when the cluster autoscaler is enabled.</li>
<li>Fixes a bug to support the default OpenShift cluster version.</li>
<li>Updates help text for the `cluster-feature-enable private-service-endpoint` and `nlb-dns-monitor-configure` commands.</li>
<li>Updates translations of help text.</li>
</ul>
</tr>
<tr>
<td>0.3.49</td>
<td>18 Jun 2019</td>
<td>Updates the Go version to 1.12.6.</td>
</tr>
<tr>
<td>0.3.47</td>
<td>15 Jun 2019</td>
<td><ul>
<li>Fixes a bug so that empty tables are not returned in the output of `ibmcloud ks kube-versions`.</li>
<li>Updates the NLB DNS model so that an array of NLB IP addresses is returned by `ibmcloud ks nlb-dnss`.</li>
<li>Changes the description text for the {{site.data.keyword.containerlong_notm}} CLI plug-in.</li>
</ul></td>
</tr>
<tr>
<td>0.3.34</td>
<td>31 May 2019</td>
<td>Adds support for creating Red Hat OpenShift on IBM Cloud clusters:<ul>
<li>Adds support for OpenShift versions in the `--kube-version` flag of the `cluster-create` command. For example, to create a standard OpenShift cluster, you can pass in `--kube-version 3.11_openshift` in your `cluster-create` command.</li>
<li>Adds the `versions` command to list all supported Kubernetes and OpenShift versions.</li>
<li>Deprecates the `kube-versions` command.</li>
</ul></td>
</tr>
<tr>
<td>0.3.33</td>
<td>30 May 2019</td>
<td><ul>
<li>Adds the <code>--powershell</code> flag to the `cluster-config` command to retrieve Kubernetes environment variables in Windows PowerShell format.</li>
<li>Deprecates the `region-get`, `region-set`, and `regions` commands. For more information, see [global endpoint functionality](/docs/containers?topic=containers-regions-and-zones#endpoint).</li>
</ul></td>
</tr>
<tr>
<td>0.3.28</td>
<td>23 May 2019</td>
<td><ul>
<li>Adds the [<code>ibmcloud ks infra-permissions-get</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#infra_permissions_get) command to check whether the credentials that allow [access to the IBM Cloud infrastructure portfolio](/docs/containers?topic=containers-users#api_key) for the targeted resource group and region are missing suggested or required infrastructure permissions.</li>
<li>Adds the <code>--private-only</code> flag to the `zone-network-set` command to unset the public VLAN for the worker pool metadata. Subsequent worker nodes in that worker pool zone are connected to a private VLAN only.</li>
<li>Removes the <code>--force-update</code> flag from the `worker-update` command.</li>
<li>Adds the **VLAN ID** column to the output of the `albs` and `alb-get` commands.</li>
<li>Adds the **Multizone Metro** column to the output of the `supported-locations` command to designate zones that are multizone-capable.</li>
<li>Adds the **Master State** and **Master Health** fields to the output of the `cluster-get` command. For more information, see [Master states](/docs/containers?topic=containers-health#states_master).</li>
<li>Updates translations of help text.</li>
</ul></td>
</tr>
<tr>
<td>0.3.8</td>
<td>30 Apr 2019</td>
<td>Adds support for [global endpoint functionality](/docs/containers?topic=containers-regions-and-zones#endpoint) in version `0.3`. By default, you can now view and manage all of your {{site.data.keyword.containerlong_notm}} resources in all locations. You are not required to target a region to work with resources.</li>
<ul><li>Adds the [<code>ibmcloud ks supported-locations</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_supported-locations) command to list all locations that {{site.data.keyword.containerlong_notm}} supports.</li>
<li>Adds the <code>--locations</code> flag to the `clusters` and `zones` commands to filter resources by one or more locations.</li>
<li>Adds the <code>--region</code> flag to the `credential-set/unset/get`, `api-key-reset`, and `vlan-spanning-get` commands. To run these commands, you must specify a region in the `--region` flag.</li></ul></td>
</tr>
<tr>
<td>0.2.102</td>
<td>15 Apr 2019</td>
<td>Adds the [`ibmcloud ks nlb-dns` group of commands](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#nlb-dns) for registering and managing a host name for network load balancer (NLB) IP addresses, and the [`ibmcloud ks nlb-dns-monitor` group of commands](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_nlb-dns-monitor-configure) for creating and modifying health check monitors for NLB host names. For more information, see [Registering NLB IPs with a DNS host name](/docs/containers?topic=containers-loadbalancer_hostname#loadbalancer_hostname_dns).
</td>
</tr>
<tr>
<td>0.2.99</td>
<td>09 Apr 2019</td>
<td><ul>
<li>Updates help text.</li>
<li>Updates the Go version to 1.12.2.</li>
</ul></td>
</tr>
<tr>
<td>0.2.95</td>
<td>03 Apr 2019</td>
<td><ul>
<li>Adds versioning support for managed cluster add-ons.</li>
<ul><li>Adds the [<code>ibmcloud ks addon-versions</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_addon_versions) command.</li>
<li>Adds the <code>--version</code> flag to [ibmcloud ks cluster-addon-enable](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_addon_enable) commands.</li></ul>
<li>Updates translations of help text.</li>
<li>Updates short links to documentation in help text.</li>
<li>Fixes a bug where JSON error messages printed in an incorrect format.</li>
<li>Fixes a bug where using the silent flag (`-s`) on some commands prevented errors from printing.</li>
</ul></td>
</tr>
<tr>
<td>0.2.80</td>
<td>19 Mar 2019</td>
<td><ul>
<li>Adds support for enabling [master-to-worker communication with service endpoints](/docs/containers?topic=containers-plan_clusters#workeruser-master) in standard clusters that run Kubernetes version 1.11 or later in [VRF-enabled accounts](/docs/resources?topic=resources-private-network-endpoints#getting-started).<ul>
<li>Adds the `--private-service-endpoint` and `--public-service-endpoint` flags to the [<code>ibmcloud ks cluster-create</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_create) command.</li>
<li>Adds the **Public Service Endpoint URL** and **Private Service Endpoint URL** fields to the output of <code>ibmcloud ks cluster-get</code>.</li>
<li>Adds the [<code>ibmcloud ks cluster-feature-enable private-service-endpoint</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_feature_enable_private_service_endpoint) command.</li>
<li>Adds the [<code>ibmcloud ks cluster-feature-enable public-service-endpoint</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_feature_enable_public_service_endpoint) command.</li>
<li>Adds the [<code>ibmcloud ks cluster-feature-disable public-service-endpoint</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_feature_disable) command.</li>
</ul></li>
<li>Updates documentation and translation.</li>
<li>Updates the Go version to 1.11.6.</li>
<li>Resolves intermittent networking issues for macOS users.</li>
</ul></td>
</tr>
<tr>
<td>0.2.75</td>
<td>14 Mar 2019</td>
<td><ul><li>Hides raw HTML from error outputs.</li>
<li>Fixes typos in help text.</li>
<li>Fixes translation of help text.</li>
</ul></td>
</tr>
<tr>
<td>0.2.61</td>
<td>26 Feb 2019</td>
<td><ul>
<li>Adds the `cluster-pull-secret-apply` command, which creates an IAM service ID for the cluster, policies, API key, and image pull secrets so that containers that run in the `default` Kubernetes namespace can pull images from IBM Cloud Container Registry. For new clusters, image pull secrets that use IAM credentials are created by default. Use this command to update existing clusters or if your cluster has an image pull secret error during creation. For more information, see [the doc](/docs/containers?topic=containers-images#cluster_registry_auth).</li>
<li>Fixes a bug where `ibmcloud ks init` failures caused help output to be printed.</li>
</ul></td>
</tr>
<tr>
<td>0.2.53</td>
<td>19 Feb 2019</td>
<td><ul><li>Fixes a bug where the region was ignored for `ibmcloud ks api-key-reset`, `ibmcloud ks credential-get/set`, and `ibmcloud ks vlan-spanning-get`.</li>
<li>Improves performance for `ibmcloud ks worker-update`.</li>
<li>Adds the version of the add-on in `ibmcloud ks cluster-addon-enable` prompts.</li>
</ul></td>
</tr>
<tr>
<td>0.2.44</td>
<td>08 Feb 2019</td>
<td><ul>
<li>Adds `--skip-rbac` option to the `ibmcloud ks cluster-config` command to skip adding user Kubernetes RBAC roles based on the {{site.data.keyword.cloud_notm}} IAM service access roles to the cluster configuration. Include this option only if you [manage your own Kubernetes RBAC roles](/docs/containers?topic=containers-users#rbac). If you use [{{site.data.keyword.cloud_notm}} IAM service access roles](/docs/containers?topic=containers-access_reference#service) to manage all your RBAC users, do not include this option.</li>
<li>Updates the Go version to 1.11.5.</li>
</ul></td>
</tr>
<tr>
<td>0.2.40</td>
<td>06 Feb 2019</td>
<td><ul>
<li>Adds the [<code>ibmcloud ks cluster-addons</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_addons), [<code>ibmcloud ks cluster-addon-enable</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_addon_enable), and [<code>ibmcloud ks cluster-addon-disable</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_addon_disable) commands for working with managed cluster add-ons such as the [Istio](/docs/containers?topic=containers-istio) and [Knative](/docs/containers?topic=containers-serverless-apps-knative) managed add-ons for {{site.data.keyword.containerlong_notm}}.</li>
<li>Improves help text for {{site.data.keyword.Bluemix_dedicated_notm}} users of the <code>ibmcloud ks vlans</code> command.</li></ul></td>
</tr>
<tr>
<td>0.2.30</td>
<td>31 Jan 2019</td>
<td>Increases the default timeout value for `ibmcloud ks cluster-config` to `500s`.</td>
</tr>
<tr>
<td>0.2.19</td>
<td>16 Jan 2019</td>
<td><ul><li>Adds the `IKS_BETA_VERSION` environment variable to enable the redesigned beta version of the {{site.data.keyword.containerlong_notm}} plug-in CLI. To try out the redesigned version, see [Using the beta command structure](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_beta).</li>
<li>Increases the default timeout value for `ibmcloud ks subnets` to `60s`.</li>
<li>Minor bug and translation fixes.</li></ul></td>
</tr>
<tr>
<td>0.1.668</td>
<td>18 Dec 2018</td>
<td><ul><li>Changes the default API endpoint from <code>https://containers.bluemix.net</code> to <code>https://containers.cloud.ibm.com</code>.</li>
<li>Fixes bug so that translations are displaying properly for command help and error messages.</li>
<li>Displays command help faster.</li></ul></td>
</tr>
<tr>
<td>0.1.654</td>
<td>05 Dec 2018</td>
<td>Updates documentation and translation.</td>
</tr>
<tr>
<td>0.1.638</td>
<td>15 Nov 2018</td>
<td>
<ul><li>Adds the [<code>ibmcloud ks cluster-refresh</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_apiserver_refresh) alias to the `apiserver-refresh` command.</li>
<li>Adds the resource group name to the output of <code>ibmcloud ks cluster-get</code> and <code>ibmcloud ks clusters</code>.</li></ul>
</td>
</tr>
<tr>
<td>0.1.635</td>
<td>06 Nov 2018</td>
<td>Adds commands for managing automatic updates of the Ingress ALB cluster add-on:<ul>
<li>[<code>ibmcloud ks alb-autoupdate-disable</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_autoupdate_disable)</li>
<li>[<code>ibmcloud ks alb-autoupdate-enable</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_autoupdate_enable)</li>
<li>[<code>ibmcloud ks alb-autoupdate-get</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_autoupdate_get)</li>
<li>[<code>ibmcloud ks alb-rollback</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_rollback)</li>
<li>[<code>ibmcloud ks alb-update</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_update)</li>
</ul></td>
</tr>
<tr>
<td>0.1.621</td>
<td>30 Oct 2018</td>
<td><ul>
<li>Adds the [<code>ibmcloud ks credential-get</code> command](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_credential_get).</li>
<li>Adds support for the <code>storage</code> log source to all cluster logging commands. For more information, see <a href="/docs/containers?topic=containers-health#logging">Understanding cluster and app log forwarding</a>.</li>
<li>Adds the `--network` flag to the [<code>ibmcloud ks cluster-config</code> command](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_config), which downloads the Calico configuration file to run all Calico commands.</li>
<li>Minor bug fixes and refactoring</li></ul>
</td>
</tr>
<tr>
<td>0.1.593</td>
<td>10 Oct 2018</td>
<td><ul><li>Adds the resource group ID to the output of <code>ibmcloud ks cluster-get</code>.</li>
<li>When [{{site.data.keyword.keymanagementserviceshort}} is enabled](/docs/containers?topic=containers-encryption#keyprotect) as a key management service (KMS) provider in your cluster, adds the KMS enabled field in the output of <code>ibmcloud ks cluster-get</code>.</li></ul></td>
</tr>
<tr>
<td>0.1.591</td>
<td>02 Oct 2018</td>
<td>Adds support for [resource groups](/docs/containers?topic=containers-clusters#cluster_prepare).</td>
</tr>
<tr>
<td>0.1.590</td>
<td>01 Oct 2018</td>
<td><ul>
<li>Adds the [<code>ibmcloud ks logging-collect</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_log_collect) and [<code>ibmcloud ks logging-collect-status</code>](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_log_collect_status) commands for collecting API server logs in your cluster.</li>
<li>Adds the [<code>ibmcloud ks key-protect-enable</code> command](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_key_protect) to enable {{site.data.keyword.keymanagementserviceshort}} as a key management service (KMS) provider in your cluster.</li>
<li>Adds the <code>--skip-master-health</code> flag to the [ibmcloud ks worker-reboot](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reboot) and [ibmcloud ks worker-reload](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reboot) commands to skip the master health check before initiating the reboot or reload.</li>
<li>Renames <code>Owner Email</code> to <code>Owner</code> in the output of <code>ibmcloud ks cluster-get</code>.</li></ul></td>
</tr>
</tbody>
</table>
