---

copyright:
  years: 2014, 2019
lastupdated: "2019-07-15"

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


# Release notes
{: #iks-release}

Use the release notes to learn about the latest changes to the {{site.data.keyword.containerlong}} documentation that are grouped by month.
{:shortdesc}

## July 2019
{: #jul19}

<table summary="The table shows release notes. Rows are to be read from the left to right, with the date in column one, the title of the feature in column two and a description in column three.">
<caption>{{site.data.keyword.containerlong_notm}} documentation updates in July 2019</caption>
<thead>
<th>Date</th>
<th>Description</th>
</thead>
<tbody>
<tr>
  <td>15 July 2019</td>
  <td><ul>
  <li><strong>Cluster and worker node ID</strong>: The ID format for clusters and worker nodes is changed. Existing clusters and worker nodes keep their existing IDs. If you have automation that relies on the previous format, update it for new clusters.<ul>
  <li>**Cluster ID**: In the regex format `{a-v0-9}[7]{a-z0-9}[2]{a-v0-9}[11]`</li>
  <li>**Worker node ID**: In the format `kube-<cluster_ID>-<cluster_name_truncated>-<resource_group_truncated>-<worker_ID>`</li></ul></li>
  <li><strong>Ingress ALB changelog</strong>: Updated the [ALB `nginx-ingress` image to build 497](/docs/containers?topic=containers-cluster-add-ons-changelog#alb_changelog).</li>
  <li><strong>Troubleshooting clusters</strong>: Added [troubleshooting steps](/docs/containers?topic=containers-cs_troubleshoot_clusters#cs_totp) for when you cannot manage clusters and worker nodes because the time-based one-time passcode (TOTP) option is enabled for your account.</li>
  <li><strong>Version changelogs</strong>: Updated the changelogs for [1.14.4_1526](/docs/containers?topic=containers-changelog#1144_1526), [1.13.8_1529](/docs/containers?topic=containers-changelog#1138_1529), and [1.12.10_1560](/docs/containers?topic=containers-changelog#11210_1560) master fix pack updates.</li></ul>
  </td>
</tr>
<tr>
  <td>08 July 2019</td>
  <td><ul>
  <li><strong>App networking</strong>: You can now find information about app networking with NLBs and Ingress ALBs in the following pages:
    <ul><li>[Basic and DSR load balancing with network load balancers (NLB)](/docs/containers?topic=containers-cs_sitemap#sitemap-nlb)</li>
    <li>[HTTPS load balancing with Ingress application load balancers (ALB)](/docs/containers?topic=containers-cs_sitemap#sitemap-ingress)</li></ul>
  </li>
  <li><strong>Version changelogs</strong>: Updated the changelogs for [1.14.3_1525](/docs/containers?topic=containers-changelog#1143_1525), [1.13.7_1528](/docs/containers?topic=containers-changelog#1137_1528), [1.12.9_1559](/docs/containers?topic=containers-changelog#1129_1559), and [1.11.10_1564](/docs/containers?topic=containers-changelog#11110_1564) worker node patch updates.</li></ul>
  </td>
</tr>
<tr>
  <td>02 July 2019</td>
  <td><strong>CLI changelog</strong>: Updated the {{site.data.keyword.containerlong_notm}} CLI plug-in changelog page for the [release of version 0.3.58](/docs/containers?topic=containers-cs_cli_changelog).</td>
</tr>
<tr>
  <td>01 July 2019</td>
  <td><ul>
  <li><strong>Infrastructure permissions</strong>: Updated the [classic infrastructure roles](/docs/containers?topic=containers-access_reference#infra) required for common use cases.</li>
  <li><strong>OpenShift FAQs</strong>: Expanded the [FAQs](/docs/containers?topic=containers-faqs#container_platforms) to include information about OpenShift clusters.</li>
  <li><strong>Securing Istio-managed apps with {{site.data.keyword.appid_short_notm}}</strong>: Added information about the [App Identity and Access adapter](/docs/containers?topic=containers-istio#app-id).</li>
  <li><strong>strongSwan VPN service</strong>: If you install strongSwan in a multizone cluster and use the `enableSingleSourceIP=true` setting, you can now [set `local.subnet` to the `%zoneSubnet` variable and use the `local.zoneSubnet` to specify an IP address as a /32 subnet for each zone of the cluster](/docs/containers?topic=containers-vpn#strongswan_4).</li>
  </ul></td>
</tr>
</tbody></table>


## June 2019
{: #jun19}

<table summary="The table shows release notes. Rows are to be read from the left to right, with the date in column one, the title of the feature in column two and a description in column three.">
<caption>{{site.data.keyword.containerlong_notm}} documentation updates in June 2019</caption>
<thead>
<th>Date</th>
<th>Description</th>
</thead>
<tbody>
<tr>
  <td>24 June 2019</td>
  <td><ul>
  <li><strong>Calico network policies</strong>: Added a set of [public Calico policies](/docs/containers?topic=containers-network_policies#isolate_workers_public) and expanded the set of [private Calico policies](/docs/containers?topic=containers-network_policies#isolate_workers) to protect your cluster on public and private networks.</li>
  <li><strong>Ingress ALB changelog</strong>: Updated the [ALB `nginx-ingress` image to build 477](/docs/containers?topic=containers-cluster-add-ons-changelog#alb_changelog).</li>
  <li><strong>Service limitations</strong>: Updated the [maximum number of pods per worker node limitation](/docs/containers?topic=containers-ibm-cloud-kubernetes-service-technology#tech_limits). For worker nodes that run Kubernetes 1.13.7_1527, 1.14.3_1524, or later and that have more than 11 CPU cores, the worker nodes can support 10 pods per core, up to a limit of 250 pods per worker node.</li>
  <li><strong>Version changelogs</strong>: Added changelogs for [1.14.3_1524](/docs/containers?topic=containers-changelog#1143_1524), [1.13.7_1527](/docs/containers?topic=containers-changelog#1137_1527), [1.12.9_1558](/docs/containers?topic=containers-changelog#1129_1558), and [1.11.10_1563](/docs/containers?topic=containers-changelog#11110_1563) worker node patch updates.</li>
  </ul></td>
</tr>
<tr>
  <td>21 June 2019</td>
  <td><ul>
  <li><strong>Accessing OpenShift clusters</strong>: Added steps for [automating access to an OpenShift cluster by using an OpenShift login token](/docs/containers?topic=containers-cs_cli_install#openshift_cluster_login).</li>
  <li><strong>Accessing the Kubernetes master through the private service endpoint</strong>: Added steps for [editing the local Kubernetes configuration file](/docs/containers?topic=containers-clusters#access_on_prem) when both the public and private service endpoints are enabled, but you want to access the Kubernetes master through the private service endpoint only.</li>
  <li><strong>Troubleshooting OpenShift clusters</strong>: Added a [troubleshooting section](/docs/containers?topic=containers-openshift_tutorial#openshift_troubleshoot) to the
  Creating a Red Hat OpenShift on IBM Cloud cluster (beta) tutorial.</li>
  </ul></td>
</tr>
<tr>
  <td>18 June 2019</td>
  <td><ul>
  <li><strong>CLI changelog</strong>: Updated the {{site.data.keyword.containerlong_notm}} CLI plug-in changelog page for the [release of versions 0.3.47 and 0.3.49](/docs/containers?topic=containers-cs_cli_changelog).</li>
  <li><strong>Ingress ALB changelog</strong>: Updated the [ALB `nginx-ingress` image to build 473 and `ingress-auth` image to build 331](/docs/containers?topic=containers-cluster-add-ons-changelog#alb_changelog).</li>
  <li><strong>Managed add-on versions</strong>: Updated the version of the Istio managed add-on to 1.1.7 and the Knative managed add-on to 0.6.0.</li>
  <li><strong>Removing persistent storage</strong>: Updated the information for how you are billed when you [delete persistent storage](/docs/containers?topic=containers-cleanup).</li>
  <li><strong>Service bindings with private endpoint</strong>: [Added steps](/docs/containers?topic=containers-service-binding) for how to manually create service credentials with the private service endpoint when you bind the service to your cluster.</li>
  <li><strong>Version changelogs</strong>: Updated the changelogs for [1.14.3_1523](/docs/containers?topic=containers-changelog#1143_1523), [1.13.7_1526](/docs/containers?topic=containers-changelog#1137_1526), [1.12.9_1557](/docs/containers?topic=containers-changelog#1129_1557), and [1.11.10_1562](/docs/containers?topic=containers-changelog#11110_1562) patch updates.</li>
  </ul></td>
</tr>
<tr>
  <td>14 June 2019</td>
  <td><ul>
  <li><strong>`kubectl` troubleshooting</strong>: Added a [troubleshooting topic](/docs/containers?topic=containers-cs_troubleshoot_clusters#kubectl_fails) for when you have a `kubectl` client version that is 2 or more versions apart from the server version or the OpenShift version of `kubectl`, which does not work with native Kubernetes clusters.</li>
  <li><strong>Tutorials landing page</strong>: Replaced the related links page with a new [tutorials landing page](/docs/containers?topic=containers-tutorials-ov) for all tutorials that are specific to {{site.data.keyword.containershort_notm}}.</li>
  <li><strong>Tutorial to create a cluster and deploy an app</strong>: Combined the tutorials for creating clusters and deploying apps into one comprehensive [tutorial](/docs/containers?topic=containers-cs_cluster_tutorial).</li>
  <li><strong>Using existing subnets to create a cluster</strong>: To [reuse subnets from an unneeded cluster when you create a new cluster](/docs/containers?topic=containers-subnets#subnets_custom), the subnets must be user-managed subnets that you manually added from an on-premises network. All subnets that were automatically ordered during cluster creation are immediately deleted after you delete a cluster, and you cannot reuse these subnets to create a new cluster.</li>
  </ul></td>
</tr>
<tr>
  <td>12 June 2019</td>
  <td><strong>Aggregating cluster roles</strong>: Added steps for [extending users' existing permissions by aggregating cluster roles](/docs/containers?topic=containers-users#rbac_aggregate).</td>
</tr>
<tr>
  <td>07 June 2019</td>
  <td><ul>
  <li><strong>Access to the Kubernetes master through the private service endpoint</strong>: Added [steps](/docs/containers?topic=containers-clusters#access_on_prem) to expose the private service endpoint through a private load balancer. After you complete these steps, your authorized cluster users can access the Kubernetes master from a VPN or {{site.data.keyword.cloud_notm}} Direct Link connection.</li>
  <li><strong>{{site.data.keyword.BluDirectLink}}</strong>: Added {{site.data.keyword.cloud_notm}} Direct Link to the [VPN connectivity](/docs/containers?topic=containers-vpn) and [hybrid cloud](/docs/containers?topic=containers-hybrid_iks_icp) pages as a way to create a direct, private connection between your remote network environments and {{site.data.keyword.containerlong_notm}} without routing over the public internet.</li>
  <li><strong>Ingress ALB changelog</strong>: Updated the [ALB `ingress-auth` image to build 330](/docs/containers?topic=containers-cluster-add-ons-changelog#alb_changelog).</li>
  <li><strong>OpenShift beta</strong>: [Added a lesson](/docs/containers?topic=containers-openshift_tutorial#openshift_logdna_sysdig) about how to modify app deployments for privileged security context constraints for {{site.data.keyword.la_full_notm}} and {{site.data.keyword.mon_full_notm}} add-ons.</li>
  </ul></td>
</tr>
<tr>
  <td>06 June 2019</td>
  <td><ul>
  <li><strong>Fluentd changelog</strong>: Added a [Fluentd version changelog](/docs/containers?topic=containers-cluster-add-ons-changelog#fluentd_changelog).</li>
  <li><strong>Ingress ALB changelog</strong>: Updated the [ALB `nginx-ingress` image to build 470](/docs/containers?topic=containers-cluster-add-ons-changelog#alb_changelog).</li>
  </ul></td>
</tr>
<tr>
  <td>05 June 2019</td>
  <td><ul>
  <li><strong>CLI reference</strong>: Updated the [CLI reference page](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli) to reflect multiple changes for the [release of version 0.3.34](/docs/containers?topic=containers-cs_cli_changelog) of the {{site.data.keyword.containerlong_notm}} CLI plug-in.</li>
  <li><strong>New! Red Hat OpenShift on IBM Cloud clusters (beta)</strong>: With the Red Hat OpenShift on IBM Cloud beta, you can create {{site.data.keyword.containerlong_notm}} clusters with worker nodes that come installed with the OpenShift container orchestration platform software. You get all the advantages of managed {{site.data.keyword.containerlong_notm}} for your cluster infrastructure environment, along with the [OpenShift tooling and catalog ![External link icon](../icons/launch-glyph.svg "External link icon")](https://docs.openshift.com/container-platform/3.11/welcome/index.html) that runs on Red Hat Enterprise Linux for your app deployments. To get started, see [Tutorial: Creating a Red Hat OpenShift on IBM Cloud cluster (beta)](/docs/containers?topic=containers-openshift_tutorial).</li>
  </ul></td>
</tr>
<tr>
  <td>04 June 2019</td>
  <td><ul>
  <li><strong>Version changelogs</strong>: Updated the changelogs for the [1.14.2_1521](/docs/containers?topic=containers-changelog#1142_1521), [1.13.6_1524](/docs/containers?topic=containers-changelog#1136_1524), [1.12.9_1555](/docs/containers?topic=containers-changelog#1129_1555), and [1.11.10_1561](/docs/containers?topic=containers-changelog#11110_1561) patch releases.</li></ul>
  </td>
</tr>
<tr>
  <td>03 June 2019</td>
  <td><ul>
  <li><strong>Bringing your own Ingress controller</strong>: Updated the [steps](/docs/containers?topic=containers-ingress-user_managed) to reflect changes to the default community controller and to require a health check for controller IP addresses in multizone clusters.</li>
  <li><strong>{{site.data.keyword.cos_full_notm}}</strong>: Updated the [steps](/docs/containers?topic=containers-object_storage#install_cos) to install the {{site.data.keyword.cos_full_notm}} plug-in with or without the Helm server, Tiller.</li>
  <li><strong>Ingress ALB changelog</strong>: Updated the [ALB `nginx-ingress` image to build 467](/docs/containers?topic=containers-cluster-add-ons-changelog#alb_changelog).</li>
  <li><strong>Kustomize</strong>: Added an example of [reusing Kubernetes configuration files across multiple environments with Kustomize](/docs/containers?topic=containers-app#kustomize).</li>
  <li><strong>Razee</strong>: Added [Razee ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/razee-io/Razee) to the supported integrations to visualize deployment information in the cluster and to automate the deployment of Kubernetes resources. </li></ul>
  </td>
</tr>
</tbody></table>


## May 2019
{: #may19}

<table summary="The table shows release notes. Rows are to be read from the left to right, with the date in column one, the title of the feature in column two and a description in column three.">
<caption>{{site.data.keyword.containerlong_notm}} documentation updates in May 2019</caption>
<thead>
<th>Date</th>
<th>Description</th>
</thead>
<tbody>
<tr>
  <td>30 May 2019</td>
  <td><ul>
  <li><strong>CLI reference</strong>: Updated the [CLI reference page](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli) to reflect multiple changes for the [release of version 0.3.33](/docs/containers?topic=containers-cs_cli_changelog) of the {{site.data.keyword.containerlong_notm}} CLI plug-in.</li>
  <li><strong>Troubleshooting storage</strong>: <ul>
  <li>Added a file and block storage troubleshooting topic for [PVC pending states](/docs/containers?topic=containers-cs_troubleshoot_storage#file_pvc_pending).</li>
  <li>Added a block storage troubleshooting topic for when [an app cannot access or write to PVC](/docs/containers?topic=containers-cs_troubleshoot_storage#block_app_failures).</li>
  </ul></li>
  </ul></td>
</tr>
<tr>
  <td>28 May 2019</td>
  <td><ul>
  <li><strong>Cluster add-ons changelog</strong>: Updated the [ALB `nginx-ingress` image to build 462](/docs/containers?topic=containers-cluster-add-ons-changelog).</li>
  <li><strong>Troubleshooting registry</strong>: Added a [troubleshooting topic](/docs/containers?topic=containers-cs_troubleshoot_clusters#ts_image_pull_create) for when your cluster cannot pull images from {{site.data.keyword.registryfull}} due to an error during cluster creation.
  </li>
  <li><strong>Troubleshooting storage</strong>: <ul>
  <li>Added a topic for [debugging persistent storage failures](/docs/containers?topic=containers-cs_troubleshoot_storage#debug_storage).</li>
  <li>Added a troubleshooting topic for [PVC creation failures due to missing permissions](/docs/containers?topic=containers-cs_troubleshoot_storage#missing_permissions).</li>
  </ul></li>
  </ul></td>
</tr>
<tr>
  <td>23 May 2019</td>
  <td><ul>
  <li><strong>CLI reference</strong>: Updated the [CLI reference page](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli) to reflect multiple changes for the [release of version 0.3.28](/docs/containers?topic=containers-cs_cli_changelog) of the {{site.data.keyword.containerlong_notm}} CLI plug-in.</li>
  <li><strong>Cluster add-ons changelog</strong>: Updated the [ALB `nginx-ingress` image to build 457](/docs/containers?topic=containers-cluster-add-ons-changelog).</li>
  <li><strong>Cluster and worker states</strong>: Updated the [Logging and monitoring page](/docs/containers?topic=containers-health#states) to add reference tables about cluster and worker node states.</li>
  <li><strong>Cluster planning and creation</strong>: You can now find information about cluster planning, creation, and removal and network planning in the following pages:
    <ul><li>[Planning your cluster network setup](/docs/containers?topic=containers-plan_clusters)</li>
    <li>[Planning your cluster for high availability](/docs/containers?topic=containers-ha_clusters)</li>
    <li>[Planning your worker node setup](/docs/containers?topic=containers-planning_worker_nodes)</li>
    <li>[Creating clusters](/docs/containers?topic=containers-clusters)</li>
    <li>[Adding worker nodes and zones to clusters](/docs/containers?topic=containers-add_workers)</li>
    <li>[Removing clusters](/docs/containers?topic=containers-remove)</li>
    <li>[Changing service endpoints or VLAN connections](/docs/containers?topic=containers-cs_network_cluster)</li></ul>
  </li>
  <li><strong>Cluster version updates</strong>: Updated the [unsupported versions policy](/docs/containers?topic=containers-cs_versions) to note that you cannot update clusters if the master version is three or more versions behind the oldest supported version. You can view if the cluster is **unsupported** by reviewing the **State** field when you list clusters.</li>
  <li><strong>Istio</strong>: Updated the [Istio page](/docs/containers?topic=containers-istio) to remove the limitation that Istio does not work in clusters that are connected to a private VLAN only. Added a step to the [Updating managed add-ons topic](/docs/containers?topic=containers-managed-addons#updating-managed-add-ons) to re-create Istio gateways that use TLS sections after the update of the Istio managed add-on is complete.</li>
  <li><strong>Popular topics</strong>: Replaced the popular topics with this release notes page for new features and updates that are specific to {{site.data.keyword.containershort_notm}}. For the latest information on {{site.data.keyword.cloud_notm}} products, check out the [Announcements](https://www.ibm.com/cloud/blog/announcements).</li>
  </ul></td>
</tr>
<tr>
  <td>20 May 2019</td>
  <td><ul>
  <li><strong>Version changelogs</strong>: Added [worker node fix pack changelogs](/docs/containers?topic=containers-changelog).</li>
  </ul></td>
</tr>
<tr>
  <td>16 May 2019</td>
  <td><ul>
  <li><strong>CLI reference</strong>: Updated the [CLI reference page](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli) to add COS endpoints for `logging-collect` commands and to clarify that `apiserver-refresh` restarts the Kubernetes master components.</li>
  <li><strong>Unsupported: Kubernetes version 1.10</strong>: [Kubernetes version 1.10](/docs/containers?topic=containers-cs_versions#cs_v114) is now unsupported.</li>
  </ul></td>
</tr>
<tr>
  <td>15 May 2019</td>
  <td><ul>
  <li><strong>Default Kubernetes version</strong>: The default Kubernetes version is now 1.13.6.</li>
  <li><strong>Service limits</strong>: Added a [service limitations topic](/docs/containers?topic=containers-ibm-cloud-kubernetes-service-technology#tech_limits).</li>
  </ul></td>
</tr>
<tr>
  <td>13 May 2019</td>
  <td><ul>
  <li><strong>Version changelogs</strong>: Added that new patch updates are available for [1.14.1_1518](/docs/containers?topic=containers-changelog#1141_1518), [1.13.6_1521](/docs/containers?topic=containers-changelog#1136_1521), [1.12.8_1552](/docs/containers?topic=containers-changelog#1128_1552), [1.11.10_1558](/docs/containers?topic=containers-changelog#11110_1558), and [1.10.13_1558](/docs/containers?topic=containers-changelog#11013_1558).</li>
  <li><strong>Worker node flavors</strong>: Removed all [virtual machine worker node flavors](/docs/containers?topic=containers-planning_worker_nodes#vm) that are 48 or more cores per [cloud status ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/status?component=containers-kubernetes&selected=status). You can still provision [bare metal worker nodes](/docs/containers?topic=containers-plan_clusters#bm) with 48 or more cores.</li></ul></td>
</tr>
<tr>
  <td>08 May 2019</td>
  <td><ul>
  <li><strong>API</strong>: Added a link to the [global API swagger docs ![External link icon](../icons/launch-glyph.svg "External link icon")](https://containers.cloud.ibm.com/global/swagger-global-api/#/).</li>
  <li><strong>Cloud Object Storage</strong>: [Added a troubleshooting guide for Cloud Object Storage](/docs/containers?topic=containers-cs_troubleshoot_storage#cos_pvc_pending) in your {{site.data.keyword.containerlong_notm}} clusters.</li>
  <li><strong>Kubernetes strategy</strong>: Added a topic about [What knowledge and technical skills are good to have before I move my apps to {{site.data.keyword.containerlong_notm}}?](/docs/containers?topic=containers-strategy#knowledge).</li>
  <li><strong>Kubernetes version 1.14</strong>: Added that the [Kubernetes 1.14 release](/docs/containers?topic=containers-cs_versions#cs_v114) is certified.</li>
  <li><strong>Reference topics</strong>: Updated information for various service binding, `logging`, and `nlb` operations in the [user access](/docs/containers?topic=containers-access_reference) and [CLI](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli) reference pages.</li></ul></td>
</tr>
<tr>
  <td>07 May 2019</td>
  <td><ul>
  <li><strong>Cluster DNS provider</strong>: [Explained the benefits of CoreDNS](/docs/containers?topic=containers-cluster_dns) now that clusters that run Kubernetes 1.14 and later support only CoreDNS.</li>
  <li><strong>Edge nodes</strong>: Added private load balancer support for [edge nodes](/docs/containers?topic=containers-edge).</li>
  <li><strong>Free clusters</strong>: Clarified where [free clusters](/docs/containers?topic=containers-regions-and-zones#regions_free) are supported.</li>
  <li><strong>New! Integrations</strong>: Added and restructure information about [{{site.data.keyword.cloud_notm}} services and third-party integrations](/docs/containers?topic=containers-ibm-3rd-party-integrations), [popular integrations](/docs/containers?topic=containers-supported_integrations), and [partnerships](/docs/containers?topic=containers-service-partners).</li>
  <li><strong>New! Kubernetes version 1.14</strong>: Create or update your clusters to [Kubernetes 1.14](/docs/containers?topic=containers-cs_versions#cs_v114).</li>
  <li><strong>Deprecated Kubernetes version 1.11</strong>: [Update](/docs/containers?topic=containers-update) any clusters that run [Kubernetes 1.11](/docs/containers?topic=containers-cs_versions#cs_v111) before they become unsupported.</li>
  <li><strong>Permissions</strong>: Added an FAQ, [What access policies do I give my cluster users?](/docs/containers?topic=containers-faqs#faq_access)</li>
  <li><strong>Worker pools</strong>: Added instructions for how to [apply labels to existing worker pools](/docs/containers?topic=containers-add_workers#worker_pool_labels).</li>
  <li><strong>Reference topics</strong>: To support new features such as Kubernetes 1.14, [changelog](/docs/containers?topic=containers-changelog#changelog) reference pages are updated.</li></ul></td>
</tr>
<tr>
  <td>01 May 2019</td>
  <td><strong>Assigning infrastructure access</strong>: Revised the [steps to assign IAM permissions for opening support cases](/docs/containers?topic=containers-users#infra_access).</td>
</tr>
</tbody></table>


