---

copyright:
  years: 2014, 2021
lastupdated: "2021-04-15"

keywords: kubernetes, iks, release notes

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
 


# Release notes
{: #iks-release}

Use the release notes to learn about the latest changes to the {{site.data.keyword.containerlong}} documentation that are grouped by month.
{: shortdesc}



Looking for {{site.data.keyword.cloud_notm}} status, platform announcements, security bulletins, or maintenance notifications? See [{{site.data.keyword.cloud_notm}} status](https://cloud.ibm.com/status?selected=status){: external}.
{: note}

## April 2021
{: #apr21}

| Date | Description |
| ---- | ----------- |
| 15 April 2021 | **Accessing VPC clusters**: Updated the steps for [accessing VPC clusters through the private cloud service endpoint](/docs/containers?topic=containers-access_cluster#vpc_private_se) by configuring the VPC VPN gateway to access the `166.8.0.0/14` subnet. |
| 12 April 2021 | <ul><li>**Worker node versions**: Worker node fix pack update changelog documentation is available for Kubernetes version [1.20.5_1535](/docs/containers?topic=containers-changelog#1205_1535), [1.19.9_1542](/docs/containers?topic=containers-changelog#1199_1542), [1.18.17_1548](/docs/containers?topic=containers-changelog#11817_1548), and [1.17.17_1559](/docs/containers?topic=containers-changelog#11717_1559).</li><li>**Ingress changelogs**: Updated the [Ingress ALB changelog](/docs/containers?topic=containers-cluster-add-ons-changelog#kube_ingress_changelog) for updates to the Kubernetes Ingress and {{site.data.keyword.containerlong_notm}} Ingress images.</li></ul>|
| 05 April 2021 | **Deprecated data centers for classic clusters**: Houston (`hou02`) and Oslo (`osl01`) are deprecated and become unsupported later this year. To prevent any interruption of service, [redeploy all of your cluster workloads](/docs/containers?topic=containers-update_app#copy_apps_cluster) to a [supported data center](/docs/containers?topic=containers-regions-and-zones#zones-mz) and remove your `osl01` or `hou02` clusters by **1 August 2021**. Before the unsupported date, you are blocked from adding new worker nodes and clusters starting on **1 July 2021**. For more information, see [Data center closures in 2021](/docs/get-support?topic=get-support-dc-closure). |
| 02 April 2021 | **Istio add-on**: [Version 1.9.2 of the Istio managed add-on](/docs/containers?topic=containers-istio-changelog#v19) is released. |
| 01 April 2021 | <ul><li>**Cluster autoscaler add-on**: [Patch update `1.0.2_249`](/docs/containers?topic=containers-ca_changelog) of the cluster autoscaler add-on is available.</li><li>**{{site.data.keyword.block_storage_is_short}} add-on**: [Patch update `3.0.0_521`](/docs/containers?topic=containers-vpc_bs_changelog) of the {{site.data.keyword.block_storage_is_short}} add-on is available.</li><li>**{{site.data.keyword.block_storage_is_short}} driver**: Added steps to install the `vpc-block-csi-driver` on unmanaged clusters. For more information, see [Storing data on {{site.data.keyword.block_storage_is_short}} for unmanaged clusters](/docs/containers?topic=containers-vpc-block-storage-driver-unmanaged).</li><li>**New! image security add-on**: In clusters that run Kubernetes version 1.18 or later, you can [install the container image security enforcement add-on](/docs/containers?topic=containers-images#portieris-image-sec) to set up the [Portieris](https://github.com/IBM/portieris){: external} project in your cluster.</li></ul> |
{: summary="The table shows release notes. Rows are to be read from the left to right, with the date in column one, the title of the feature in column two and a description in column three."}
{: caption="Documentation updates in April 2021"}

## March 2021
{: #mar21}

| Date | Description |
| ---- | ----------- |
| 30 March 2021 | <ul><li>**Add-on changelogs**: Added version changelogs for the [ALB OAuth Proxy add-on](/docs/containers?topic=containers-alb-oauth-proxy-changelog).</li><li>**Master versions**: Master fix pack update changelog documentation is available for Kubernetes version [1.20.5_1533](/docs/containers?topic=containers-changelog#1205_1533), [1.19.9_1540](/docs/containers?topic=containers-changelog#1199_1540), [1.18.17_1546](/docs/containers?topic=containers-changelog#11817_1546), and [1.17.17_1557](/docs/containers?topic=containers-changelog#11717_1557).</li></ul>|
| 29 March 2021 | **Worker node versions**: Worker node fix pack update changelog documentation is available for Kubernetes version [1.20.5_1534](/docs/containers?topic=containers-changelog#1205_1534), [1.19.9_1541](/docs/containers?topic=containers-changelog#1199_1541), [1.18.17_1547](/docs/containers?topic=containers-changelog#11817_1547), and [1.17.17_1558](/docs/containers?topic=containers-changelog#11717_1558). |
| 25 March 2021 | <ul><li>**{{site.data.keyword.cloudcerts_short}} instances**: The default {{site.data.keyword.cloudcerts_short}} instance for your cluster is now named in the format `kube-crtmgr-<cluster_ID>`.</li></ul> |
| 23 March 2021 | **Istio add-on**: [Version 1.8.4 of the Istio managed add-on](/docs/containers?topic=containers-istio-changelog#184) is released. |
| 22 March 2021 | **Ingress ALB changelog**: Updated the [Ingress ALB changelog](/docs/containers?topic=containers-cluster-add-ons-changelog#kube_ingress_changelog) for updates to the Kubernetes Ingress and {{site.data.keyword.containerlong_notm}} Ingress images. |
| 17 March 2021 | **Deprecated events to be replaced for {{site.data.keyword.at_short}}**: To align with event auditing standards across cloud, several cluster events and fields are deprecated and are replaced in 30 days on 16 April 2021 by new events and fields.<ul><li>**Deprecated events**: Some events are [deprecated](/docs/containers?topic=containers-at_events#cluster-events-old) and are no longer sent after the deprecation. Instead, [new events](/docs/containers?topic=containers-at_events#events-new) are sent.</li><li>**Deprecated fields across events**: [Some fields](/docs/containers?topic=containers-at_events#events-updated-fields) are deprecated and replaced by or updated with new values across events.</li></ul>
| 16 March 2021 | **Ingress ALB changelog**: Updated the [`nginx-ingress` build to 2458](/docs/containers?topic=containers-cluster-add-ons-changelog#alb_changelog) for the {{site.data.keyword.containerlong_notm}} Ingress image. |
| 12 March 2021 | **Worker node versions**: Worker node fix pack update changelog documentation is available for Kubernetes version [1.20.4_1532](/docs/containers?topic=containers-changelog#1204_1532), [1.19.8_1539](/docs/containers?topic=containers-changelog#1198_1539), [1.18.16_1545](/docs/containers?topic=containers-changelog#11816_1545), and [1.17.17_1556](/docs/containers?topic=containers-changelog#11717_1556).|
| 10 March 2021 | **Istio add-on**: [Version 1.7.8 of the Istio managed add-on](/docs/containers?topic=containers-istio-changelog#178) is released. |
| 09 March 2021 | **Cluster autoscaler add-on**: Version 1.0.2 of the cluster autoscaler add-on is released. For more information, see the [cluster autoscaler add-on changelog](/docs/containers?topic=containers-ca_changelog). |
| 05 March 2021 | **Trusted images**: You can now [set up trusted content for container images](/docs/containers?topic=containers-images#trusted_images) that are signed and stored in {{site.data.keyword.registrylong_notm}}. |
| 01 March 2021 | <ul><li>**CLI changelog**: Updated the {{site.data.keyword.containerlong_notm}} CLI plug-in changelog page for the [release of version 1.0.233](/docs/containers?topic=containers-cs_cli_changelog#10).</li><li>**Istio add-on**: [Version 1.8.3 of the Istio managed add-on](/docs/containers?topic=containers-istio-changelog#183) is released.</li><li>**Worker node versions**: Worker node fix pack update changelog documentation is available for Kubernetes version [1.20.4_1531](/docs/containers?topic=containers-changelog#1204_1531), [1.19.8_1538](/docs/containers?topic=containers-changelog#1198_1538), [1.18.16_1544](/docs/containers?topic=containers-changelog#11816_1544), and [1.17.17_1555](/docs/containers?topic=containers-changelog#11717_1555_worker).</li></ul> |
{: summary="The table shows release notes. Rows are to be read from the left to right, with the date in column one, the title of the feature in column two and a description in column three."}
{: caption="Documentation updates in March 2021"}

## February 2021
{: #feb21}

| Date | Description |
| ---- | ----------- |
| 27 February 2021 | **Master versions**: Master fix pack update changelog documentation is available for Kubernetes version [1.20.4_1531](/docs/containers?topic=containers-changelog#1204_1531_master), [1.19.8_1538](/docs/containers?topic=containers-changelog#1198_1538_master), and [1.18.16_1544](/docs/containers?topic=containers-changelog#11816_1544_master). |
| 26 February 2021 | <ul><li>**End of service of VPC Gen 1**: Removed steps for using VPC Gen 1 compute. You can now [create new VPC clusters on Generation 2 compute only](/docs/containers?topic=containers-clusters#clusters_vpcg2). [Move any remaining workloads from VPC Gen 1 clusters to VPC Gen 2 clusters](/docs/containers?topic=containers-vpc_migrate_tutorial) before 01 March 2021, when any remaining VPC Gen 1 worker nodes are automatically deleted.</li><li>**{{site.data.keyword.block_storage_is_short}} add-on**: Version `3.0.0` of the {{site.data.keyword.block_storage_is_short}} add-on is released. Update your clusters to use the latest version. For more information, see the [{{site.data.keyword.block_storage_is_short}} add-on changelog](/docs/containers?topic=containers-vpc_bs_changelog).</li></ul> |
| 25 February 2021 | **CLI changelog**: Updated the {{site.data.keyword.containerlong_notm}} CLI plug-in changelog page for the [release of version 1.0.231](/docs/containers?topic=containers-cs_cli_changelog#10). |
| 24 February 2021 | **Default version**: Kubernetes 1.19 is now the default cluster version. |
| 23 February 2021 | **VPE**: In VPC clusters that run Kubernetes version 1.20 or later, worker node communication to the Kubernetes master is now established over the [VPC virtual private endpoint (VPE)](/docs/containers?topic=containers-vpc-subnets#vpc_basics_vpe). |
| 22 February 2021 | **Master versions**: Master fix pack update changelog documentation is available for Kubernetes version [1.20.4_1530](/docs/containers?topic=containers-changelog#1204_1530), [1.19.8_1537](/docs/containers?topic=containers-changelog#1198_1537), [1.18.16_1543](/docs/containers?topic=containers-changelog#11816_1543), and [1.17.17_1555](/docs/containers?topic=containers-changelog#11717_1555). |
| 20 February 2021 | **Certified Kubernetes**: The Kubernetes version [1.20](/docs/containers?topic=containers-cs_versions#cs_v120) release is now certified. |
| 17 February 2021 | <ul><li>**New! Kubernetes 1.20**: You can create or [update](/docs/containers?topic=containers-cs_versions#cs_v120) your cluster to Kubernetes version 1.20. With Kubernetes 1.20, you get the latest stable enhancements from the community, as well as beta access to features such as [API server priority](/docs/containers?topic=containers-kubeapi-priority). For more information, see the [blog announcement](http://www.ibm.com/cloud/blog/announcements/kubernetes-version-120-now-available-in-ibm-cloud-kubernetes-service){: external}.</li><li>**Deprecated Kubernetes 1.17**: With the release of Kubernetes 1.20, clusters that run version 1.17 are deprecated, with a tentative unsupported date of 30 June 2021. Update your cluster to at least [version 1.18](/docs/containers?topic=containers-cs_versions#cs_v118) as soon as possible.</li></ul> |
| 15 February 2021 | <ul><li>**New! Osaka multizone region**: You can now create classic or VPC clusters in the Osaka, Japan (`jp-osa`) [location](/docs/containers?topic=containers-regions-and-zones).</li><li>**Worker node versions**: Worker node fix pack update changelog documentation is available for Kubernetes version [1.19.7_1535](/docs/containers?topic=containers-changelog#1197_1535), [1.18.15_1541](/docs/containers?topic=containers-changelog#11815_1541), and [1.17.17_1553](/docs/containers?topic=containers-changelog#11717_1553).</li></ul> |
| 12 February 2021 | **Gateway firewalls and Calico policies**: For classic clusters in Frankfurt, updated the required IP addresses and ports that you must open in a [private gateway device firewall](/docs/containers?topic=containers-firewall#firewall_private) or [Calico private network isolation policies](https://github.com/IBM-Cloud/kube-samples/tree/master/calico-policies/private-network-isolation/calico-v3/eu-de){: external}.
| 10 February 2021 | <ul><li>**Ingress ALB changelog**: Updated the [`nginx-ingress` build to 2452](/docs/containers?topic=containers-cluster-add-ons-changelog#alb_changelog) for the {{site.data.keyword.containerlong_notm}} Ingress image.</li><li>**Gateway firewalls and Calico policies**: For classic clusters in Dallas, Washington D.C., and Frankfurt, updated the required IP addresses and ports that you must open in a [public gateway firewall device](/docs/containers?topic=containers-firewall#firewall_outbound), [private gateway device firewall](/docs/containers?topic=containers-firewall#firewall_private), or [Calico network isolation policies](https://github.com/IBM-Cloud/kube-samples/tree/master/calico-policies){: external}.</li></ul> |
| 08 February 2021 | <ul><li>**CLI changelog**: Updated the {{site.data.keyword.containerlong_notm}} CLI plug-in changelog page for the [release of version 1.0.223](/docs/containers?topic=containers-cs_cli_changelog#10).</li><li>**Istio add-on**: [Version 1.7.7 of the Istio managed add-on](/docs/containers?topic=containers-istio-changelog#177) is released.</li></ul> |
| 04 February 2021 | **Worker node modifications**: Summarized the modifications that you can make to the [default worker node settings](/docs/containers?topic=containers-kernel#worker-default).|
| 03 February 2021 | **Worker node versions**: A worker node fix pack for internal documentation updates is available for Kubernetes version [1.19.7_1534](/docs/containers?topic=containers-changelog#1197_1534), and [1.18.15_1540](/docs/containers?topic=containers-changelog#11815_1540).|
| 01 February 2021 | <ul><li>**Unsupported: Kubernetes version 1.16**: Clusters that run Kubernetes version 1.16 are unsupported as of 31 January 2021. To continue receiving important security updates and support, you must [update the cluster to a supported version](/docs/containers?topic=containers-cs_versions#prep-up) immediately.</li><li>**Worker node versions**: Worker node fix pack update changelog documentation is available for Kubernetes version [1.19.7_1533](/docs/containers?topic=containers-changelog#1197_1533), [1.18.15_1539](/docs/containers?topic=containers-changelog#11815_1539), and [1.17.17_1552](/docs/containers?topic=containers-changelog#11717_1552).</li></ul> |
{: summary="The table shows release notes. Rows are to be read from the left to right, with the date in column one, the title of the feature in column two and a description in column three."}
{: caption="Documentation updates in February 2021"}

## January 2021
{: #jan21}

| Date | Description |
| ---- | ----------- |
| 27 January 2021 | <ul><li>**Reminder: VPC Gen 1 deprecation**: You can continue to use VPC Gen 1 resources only until 26 February 2021, when all service for VPC Gen 1 ends. On 01 March 2021, any remaining VPC Gen 1 worker nodes are automatically deleted. To ensure continued support, [create new VPC clusters on Generation 2 compute only](/docs/containers?topic=containers-clusters#clusters_vpcg2), and [move your workloads from existing VPC Gen 1 clusters to VPC Gen 2 clusters](/docs/containers?topic=containers-vpc_migrate_tutorial).</li><li>**Block Storage for VPC add-on**: Block Storage for VPC add-on patch update `2.0.3_471` is released. For more information, see the [Block Storage for VPC add-on changelog](/docs/containers?topic=containers-vpc_bs_changelog).</li></ul> |
| 25 January 2021 | <ul><li>**Istio add-on**: [Version 1.8.2 of the Istio managed add-on](/docs/containers?topic=containers-istio-changelog#182) is released.</li><li>**New! {{site.data.keyword.openshiftshort}} Do (`odo`) CLI tutorial**: Looking to develop apps without using `kubectl` system admin commands or YAML configuration files? Check out the [Developing on clusters with the {{site.data.keyword.openshiftshort}} Do CLI](/docs/containers?topic=containers-odo-tutorial) tutorial for a quick guide on using `odo` to package and push your apps to your cluster. You no longer need an {{site.data.keyword.openshiftshort}} cluster to use `odo`, but can use `odo` with any Kubernetes cluster.</li><li>**New! Private service endpoint allowlists**: You can now control access to your private cloud service endpoint by [creating a subnet allowlist](/docs/containers?topic=containers-access_cluster#private-se-allowlist). Only authorized requests to your cluster master that originate from subnets in the allowlist are permitted through the cluster's private cloud service endpoint.</li><li>**Private Kubernetes Ingress**: Added steps for [privately exposing apps with ALBs that run the Kubernetes Ingress image](/docs/containers?topic=containers-ingress-types#alb-comm-create-private).</li></ul> |
| 19 January 2021 | <ul><li>**Ingress ALB changelog**: Updated the [`nginx-ingress` build to 2424](/docs/containers?topic=containers-cluster-add-ons-changelog#alb_changelog) for the {{site.data.keyword.containerlong_notm}} Ingress image.</li><li>**Master versions**: Master fix pack update changelog documentation is available for Kubernetes version [1.19.7_1532](/docs/containers?topic=containers-changelog#1197_1532_master), [1.18.15_1538](/docs/containers?topic=containers-changelog#11815_1538_master), [1.17.17_1551](/docs/containers?topic=containers-changelog#11717_1551_master), and [1.16.15_1557](/docs/containers?topic=containers-changelog_archive#11615_1557_master).</li></ul> |
| 18 January 2021 | <ul><li>**Knative add-on is unsupported**: The Knative add-on is automatically removed from your cluster. Instead, use the [Knative open source project](https://knative.dev/docs/install/){: external} or [{{site.data.keyword.codeenginefull}}](/docs/codeengine?topic=codeengine-getting-started), which includes Knative's open-source capabilities.</li><li>**Worker node versions**: Worker node fix pack update changelog documentation is available for Kubernetes version [1.19.7_1532](/docs/containers?topic=containers-changelog#1197_1532), [1.18.15_1538](/docs/containers?topic=containers-changelog#11815_1538), [1.17.17_1551](/docs/containers?topic=containers-changelog#11717_1551), and [1.16.15_1557](/docs/containers?topic=containers-changelog_archive#11615_1557).</li></ul> |
| 14 January 2021 | **Cluster autoscaler**: Cluster autoscaler add-on patch update `1.0.1_210` is released. For more information, see the [Cluster autoscaler add-on changelog](/docs/containers?topic=containers-ca_changelog) |
| 12 January 2021 | <ul><li>**Ingress resources**: Added example Ingress resource definitions that are compatible with Kubernetes version 1.19. See the example YAML file in the documentation for the [community Kubernetes Ingress setup](/docs/containers?topic=containers-ingress-types#alb-comm-create) or for the [deprecated custom {{site.data.keyword.containerlong_notm}} Ingress setup](/docs/containers?topic=containers-ingress#public_inside_4).</li><li>**Kubernetes benchmarks**: Added how to [run the CIS Kubernetes benchmark tests on your own worker nodes](/docs/containers?topic=containers-cis-benchmark#cis-worker-test).</li><li>**Removal of data center support**: Updated the documentation to reflect that Melbourne (`mel01`) is no longer available as an option to create {{site.data.keyword.cloud_notm}} resources in.</li></ul>|
| 07 January 2021 | **Ingress ALB changelog**: Updated the latest [Kubernetes Ingress image build to `0.35.0_869_iks`](/docs/containers?topic=containers-cluster-add-ons-changelog#0_35_0).|
| 06 January 2021 | **Master versions**: Master fix pack update changelog documentation is available for Kubernetes version [1.19.6_1531](/docs/containers?topic=containers-changelog#1196_1531), [1.18.14_1537](/docs/containers?topic=containers-changelog#11814_1537), [1.17.16_1550](/docs/containers?topic=containers-changelog#11716_1550), and [1.16.15_1556](/docs/containers?topic=containers-changelog_archive#11615_1556).|
{: summary="The table shows release notes. Rows are to be read from the left to right, with the date in column one, the title of the feature in column two and a description in column three."}
{: caption="Documentation updates in January 2021"}

## December 2020
{: #dec20}

| Date | Description |
| ---- | ----------- |
| 21 December 2020 | <ul><li>**Gateway firewalls and Calico policies**: For classic clusters in Tokyo, updated the {{site.data.keyword.containerlong_notm}} IP addresses that you must open in a [public gateway firewall device](/docs/containers?topic=containers-firewall#firewall_outbound) or [Calico network isolation policies](https://github.com/IBM-Cloud/kube-samples/tree/master/calico-policies/public-network-isolation){: external}.</li><li>**Worker node versions**: Worker node fix pack update changelog documentation is available for Kubernetes version [1.19.5_1530](/docs/containers?topic=containers-changelog#1195_1530), [1.18.13_1536](/docs/containers?topic=containers-changelog#11813_1536), [1.17.15_1549](/docs/containers?topic=containers-changelog#11715_1549), and [1.16.15_1555](/docs/containers?topic=containers-changelog_archive#11615_1555).</li></ul> |
| 18 December 2020 | <ul><li>**CLI changelog**: Updated the {{site.data.keyword.containerlong_notm}} CLI plug-in changelog page for the [release of version 1.0.208](/docs/containers?topic=containers-cs_cli_changelog#10).</li><li>**Object Storage plug-in**: Version `2.0.6` of the Object Storage plug-in is released. For more information, see the [Object Storage plug-in changelog](/docs/containers?topic=containers-cos_plugin_changelog).</li></ul> |
| 17 December 2020 | <ul><li>**Audit documentation**: Reorganized information about the configuration and forwarding of Kubernetes API server [audit logs](/docs/containers?topic=containers-health-audit).</li><li>**Back up and restore**: Version `1.0.5` of the `ibmcloud-backup-restore` Helm chart is released. For more information, see the [Back up and restore Helm chart changelog](/docs/containers?topic=containers-backup_restore_changelog).</li><li>**Ingress ALB changelog**: Updated the latest [Kubernetes Ingress image build to `0.35.0_826_iks`](/docs/containers?topic=containers-cluster-add-ons-changelog#0_35_0). Updated the [`ingress-auth` build to 954](/docs/containers?topic=containers-cluster-add-ons-changelog#alb_changelog) for the {{site.data.keyword.containerlong_notm}} Ingress image.</li></ul> |
| 16 December 2020 | **Istio add-on**: Versions [1.8.1](/docs/containers?topic=containers-istio-changelog#181) and [1.7.6](/docs/containers?topic=containers-istio-changelog#176) of the Istio managed add-on are released. Version 1.6 is unsupported. |
| 15 December 2020 | **Cluster autoscaler**: Cluster autoscaler add-on patch update `1.0.1_205` is released. For more information, see the [Cluster autoscaler add-on changelog](/docs/containers?topic=containers-ca_changelog). |
| 14 December 2020 | <ul><li>**Ingress ALB changelog**: Updated the latest [Kubernetes Ingress image build to `0.35.0_767_iks`](/docs/containers?topic=containers-cluster-add-ons-changelog#0_35_0). Updated the [`nginx-ingress` build to 2410 and the `ingress-auth` build to 947](/docs/containers?topic=containers-cluster-add-ons-changelog#alb_changelog) for the {{site.data.keyword.containerlong_notm}} Ingress image.</li><li>**Master versions**: Master fix pack update changelog documentation is available for Kubernetes version [1.19.5_1529](/docs/containers?topic=containers-changelog#1195_1529), [1.18.13_1535](/docs/containers?topic=containers-changelog#11813_1535), [1.17.15_1548](/docs/containers?topic=containers-changelog#11715_1548), and [1.16.15_1554](/docs/containers?topic=containers-changelog_archive#11615_1554_master).</li></ul> |
| 11 December 2020 | <ul><li>**Storage add-ons**: Cluster autoscaler add-on patch update `1.0.1_195` is released. For more information, see the [Cluster autoscaler add-on changelog](/docs/containers?topic=containers-ca_changelog). {{site.data.keyword.block_storage_is_short}} add-on patch update `2.0.3_464` is released. For more information, see the [{{site.data.keyword.block_storage_is_short}} add-on changelog](/docs/containers?topic=containers-vpc_bs_changelog).</li><li>**strongSwan versions** Added information about which [strongSwan Helm chart versions](/docs/containers?topic=containers-vpn#vpn_upgrade) are supported.</li><li>**Worker node versions**: Worker node fix pack update changelog documentation is available for Kubernetes version [1.19.4_1529](/docs/containers?topic=containers-changelog#1194_1529), [1.18.12_1535](/docs/containers?topic=containers-changelog#11812_1535), [1.17.14_1548](/docs/containers?topic=containers-changelog#11714_1548), and [1.16.15_1554](/docs/containers?topic=containers-changelog_archive#11615_1554).</li></ul> |
| 09 December 2020 | <ul><li>**Accessing clusters**: Updated the steps for [accessing clusters through the private cloud service endpoint](/docs/containers?topic=containers-access_cluster#access_private_se) to use the `--endpoint private` flag in the `ibmcloud ks cluster config` command.</li><li>**CLI changelog**: Updated the {{site.data.keyword.containerlong_notm}} CLI plug-in changelog page for the [release of version 1.0.206](/docs/containers?topic=containers-cs_cli_changelog#10).</li><li>**Proxy protocol for Ingress:** In VPC clusters, you can now [enable the PROXY protocol](/docs/containers?topic=containers-comm-ingress-annotations#preserve_source_ip_vpc) for all load balancers that expose Ingress ALBs in your cluster. The PROXY protocol enables load balancers to pass client connection information that is contained in headers on the client request, including the client IP address, the proxy server IP address, and both port numbers, to ALBs.</li></ul> |
| 09 December 2020 | <ul><li>**Helm version 2 unsupported**: Removed all steps for using Helm v2, which is unsupported. [Migrate to Helm v3](https://helm.sh/docs/topics/v2_v3_migration/){: external} today for several advantages over Helm v2, such as the removal of the Helm server, Tiller.</li><li>**Istio add-on**: [Version 1.8 of the Istio managed add-on](/docs/containers?topic=containers-istio-changelog#v18) is released.</li></ul> |
| 07 December 2020 | **{{site.data.keyword.keymanagementserviceshort}} enhancements**: For clusters that run `1.18.8_1525` or later, your cluster now [integrates more features from {{site.data.keyword.keymanagementserviceshort}}](/docs/containers?topic=containers-encryption#kms-keyprotect-features) when you enable {{site.data.keyword.keymanagementserviceshort}} as the KMS provider for the cluster. When you enable this integration, a **Reader** [service-to-service authorization policy](/docs/account?topic=account-serviceauth) between {{site.data.keyword.containerlong_notm}} and {{site.data.keyword.keymanagementserviceshort}} is automatically created for your cluster, if the policy does not already exist. If you have a cluster that runs an earlier version, [update your cluster](/docs/containers?topic=containers-update) and then [reenable KMS encryption](/docs/containers?topic=containers-encryption#keyprotect) to register your cluster with {{site.data.keyword.keymanagementserviceshort}} again.<br><br>**Worker node versions**: Worker node fix pack update changelog documentation is available for Kubernetes version [1.19.4_1528](/docs/containers?topic=containers-changelog#1194_1528), [1.18.12_1534](/docs/containers?topic=containers-changelog#11812_1534), [1.17.14_1546](/docs/containers?topic=containers-changelog#11714_1546), and [1.16.15_1553](/docs/containers?topic=containers-changelog_archive#11615_1553). |
| 03 December 2020 | <ul><li>**Istio add-on**: Versions [1.6.14](/docs/containers?topic=containers-istio-changelog#1614) and [1.7.5](/docs/containers?topic=containers-istio-changelog#175) of the Istio managed add-on are released.</li><li>**Cluster autoscaler add-on**: Patch update `1.0.1_146` is released. For more information, see the [Cluster autoscaler add-on changelog](/docs/openshift?topic=openshift-ca_changelog).</li></ul> |
| 01 December 2020 | **Default Kubernetes Ingress image**: In all new {{site.data.keyword.containerlong_notm}} clusters, default application load balancers (ALBs) now run the Kubernetes Ingress image. In existing clusters, ALBs continue to run the previously supported {{site.data.keyword.containerlong_notm}} Ingress image, which is now deprecated. For more information and migration actions, see [Setting up Kubernetes Ingress](/docs/containers?topic=containers-ingress-types). |
{: summary="The table shows release notes. Rows are to be read from the left to right, with the date in column one, the title of the feature in column two and a description in column three."}
{: caption="Documentation updates in December 2020"}

## November 2020
{: #nov20}

| Date | Description |
| ---- | ----------- |
| 24 November 2020 | <img src="images/icon-classic.png" alt="Classic infrastructure provider icon" width="15" style="width:15px; border-style: none"/> **New! Reservations to reduce classic worker node costs**: Create a reservation with contracts for 1 or 3 year terms for classic worker nodes to lock in a reduced cost for the life of the contract. Typical savings range between 30-50% compared to on-demand worker node costs. Reservations can be created in the {{site.data.keyword.cloud_notm}} console for classic infrastructure only. For more information, see [Reserving instances to reduce classic worker node costs](/docs/containers?topic=containers-reservations). |
| 23 November 2020 | **Worker node versions**: Worker node fix pack update changelog documentation is available for Kubernetes version [1.19.4_1527](/docs/containers?topic=containers-changelog#1194_1527_worker), [1.18.12_1533](/docs/containers?topic=containers-changelog#11812_1533_worker), [1.17.14_1545](/docs/containers?topic=containers-changelog#11714_1545_worker), and [1.16.15_1552](/docs/containers?topic=containers-changelog_archive#11615_1552_worker). |
| 20 November 2020 | **New! Portieris for image security enforcement**: With the [open source Portieris project](https://github.com/IBM/portieris){: external}, you can set up a Kubernetes admission controller to enforce image security policies by namespace or cluster. Use Portieris instead of the [deprecated Container Image Security Enforcement Helm chart](/docs/Registry?topic=Registry-security_enforce). |
| 19 November 2020 | **Ingress ALB changelog**: Updated the [`nginx-ingress` build to 653 and the `ingress-auth` build to 425](/docs/containers?topic=containers-cluster-add-ons-changelog#alb_changelog) for the {{site.data.keyword.containerlong_notm}} Ingress image. |
| 18 November 2020 | <ul><li>**CLI changelog**: Updated the {{site.data.keyword.containerlong_notm}} CLI plug-in changelog page for the [release of version 1.0.197](/docs/containers?topic=containers-cs_cli_changelog#10).</li><li>**Knative add-on deprecation**: As of 18 November 2020 the Knative managed add-on is deprecated. On 18 December 2020 the add-on becomes unsupported and can no longer be installed, and on 18 January 2021 the add-on is automatically uninstalled from all clusters. If you use the Knative add-on, consider migrating to the [Knative open source project](https://knative.dev/docs/install/){: external} or to [{{site.data.keyword.codeenginefull}}](/docs/codeengine?topic=codeengine-getting-started), which includes the open source capabilities of Knative.</li><li>**New! {{site.data.keyword.block_storage_is_short}} changelog**: Added a [changelog](/docs/containers?topic=containers-vpc_bs_changelog) for the {{site.data.keyword.block_storage_is_short}} add-on.</li></ul>|
| 16 November 2020 | **GPU worker nodes**: Added a reference to the [NVIDIA GPU operator installation guide](https://docs.nvidia.com/datacenter/cloud-native/gpu-operator/getting-started.html#operator-install-guide){: external} in the [deploying a sample GPU workload](/docs/containers?topic=containers-deploy_app#gpu_app) topic.<br><br>**Master versions**: Master fix pack update changelog documentation is available for Kubernetes version [1.19.4_1527](/docs/containers?topic=containers-changelog#1194_1527), [1.18.12_1533](/docs/containers?topic=containers-changelog#11812_1533), [1.17.14_1545](/docs/containers?topic=containers-changelog#11714_1545), and [1.16.15_1552](/docs/containers?topic=containers-changelog_archive#11615_1552).. |
| 13 November 2020 | **{{site.data.keyword.at_full_notm}} and IAM events**: Added [IAM actions and {{site.data.keyword.cloudaccesstrailshort}} events](/docs/containers?topic=containers-api-at-iam#ks-ingress) for the Ingress secret, Ingress ALB, and NLB DNS APIs. |
| 09 November 2020 | **Worker node versions**: Worker node fix pack update changelog documentation is available for Kubernetes version [1.19.3_1526](/docs/containers?topic=containers-changelog#1193_1526), [1.18.10_1532](/docs/containers?topic=containers-changelog#11810_1532), [1.17.13_1544](/docs/containers?topic=containers-changelog#11713_1544), and [1.16.15_1551](/docs/containers?topic=containers-changelog_archive#11615_1551).|
| 05 November 2020 | <ul><li>**{{site.data.keyword.block_storage_is_short}}**: Added topics for [verifying your {{site.data.keyword.block_storage_is_short}} file system](/docs/containers?topic=containers-vpc-block#vpc-block-fs-verify), [enabling every user to customize the default PVC settings](/docs/containers?topic=containers-vpc-block#customize-with-secret), and [enforcing base64 encoding for the {{site.data.keyword.keymanagementserviceshort}} root key CRN](/docs/containers?topic=containers-vpc-block#static-secret).</li><li>**Classic-enabled VPCs**: Added steps in [Creating VPC subnets for classic access](/docs/containers?topic=containers-vpc-subnets#ca_subnet_cli) for creating a classic-enabled VPC and VPC subnets without the automatic default address prefixes.</li><li>**Gateway firewalls and Calico policies**: For classic clusters in Dallas and Frankfurt, updated the required IP addresses and ports that you must open in a [public gateway firewall device](/docs/containers?topic=containers-firewall#firewall_outbound), [private gateway device firewall](/docs/containers?topic=containers-firewall#firewall_private), or [Calico network isolation policies](https://github.com/IBM-Cloud/kube-samples/tree/master/calico-policies){: external}.</li><li>**Istio add-on**: Versions [1.6.13](/docs/containers?topic=containers-istio-changelog#1613) and [1.7.4](/docs/containers?topic=containers-istio-changelog#174) of the Istio managed add-on are released.</li></ul> |
| 02 November 2020 | <ul><li>**General availability of Kubernetes Ingress support:** The Kubernetes Ingress image, which is built on the community Kubernetes project's implementation of the NGINX Ingress controller, is now generally available for the Ingress ALBs in your cluster. To get started, see [Creating ALBs that run the Kubernetes Ingress image](/docs/containers?topic=containers-ingress-types#alb-comm-create) or [Changing existing ALBs to run Kubernetes Ingress](/docs/containers?topic=containers-ingress-types#alb-type-migration).</li><li>**Persistent storage**: Added the following topics:<ul><li>[Creating {{site.data.keyword.cos_full_notm}} service credentials](/docs/containers?topic=containers-object_storage#service_credentials)</li><li>[Adding your {{site.data.keyword.cos_full_notm}} credentials to the default storage classes](/docs/containers?topic=containers-object_storage#storage_class_custom)</li><li>[Backing up and restoring storage data](/docs/containers?topic=containers-storage_br)</li></ul></li></ul> |
{: summary="The table shows release notes. Rows are to be read from the left to right, with the date in column one, the title of the feature in column two and a description in column three."}
{: caption="Documentation updates in November 2020"}

## October 2020
{: #oct20}

| Date | Description |
| ---- | ----------- |
| 26 October 2020 | <ul><li>**Master versions**: Master fix pack update changelog documentation is available for Kubernetes version [1.19.3_1525](/docs/containers?topic=containers-changelog#1193_1525), [1.18.10_1531](/docs/containers?topic=containers-changelog#11810_1531), [1.17.13_1543](/docs/containers?topic=containers-changelog#11713_1543), and [1.16.15_1550](/docs/containers?topic=containers-changelog_archive#11615_1550).</li><li>**Worker node versions**: Worker node fix pack update changelog documentation is available for Kubernetes version [1.19.3_1525](/docs/containers?topic=containers-changelog#1193_1525_worker), [1.18.10_1531](/docs/containers?topic=containers-changelog#11810_1531_worker), [1.17.13_1543](/docs/containers?topic=containers-changelog#11713_1543_worker), and [1.16.15_1550](/docs/containers?topic=containers-changelog_archive#11615_1550_worker).</li></ul>|
| 22 October 2020 | <ul><li>**API key**: Added more information about [how the {{site.data.keyword.containerlong_notm}} API key is used](/docs/containers?topic=containers-users#api_key_about).</li><li>**Benchmark for Kubernetes 1.19**: Review the [1.5 CIS Kubernetes benchmark results](/docs/containers?topic=containers-cis-benchmark#cis-benchmark-15) for clusters that run Kubernetes version 1.19.</li><li>**Huge pages**: In clusters that run Kubernetes 1.19 or later, you can [enable Kubernetes `HugePages` scheduling on your worker nodes](/docs/containers?topic=containers-kernel#huge-pages).</li><li>**Istio add-on**: Version [1.6.12](/docs/containers?topic=containers-istio-changelog#1612) of the Istio managed add-on is released.</li></ul> |
| 16 October 2020 | <ul><li>**Gateway firewalls and Calico policies**: For classic clusters in Dallas, updated the required IP addresses and ports that you must open in a [public gateway firewall device](/docs/containers?topic=containers-firewall#firewall_outbound), [private gateway device firewall](/docs/containers?topic=containers-firewall#firewall_private), or [Calico network isolation policies](https://github.com/IBM-Cloud/kube-samples/tree/master/calico-policies){: external}.</li><li>**Ingress classes**: Added information about [specifying Ingress classes](/docs/containers?topic=containers-ingress-types#ingress-class) to apply Ingress resources to specific ALBs.</li><li>**{{site.data.keyword.cos_short}}**: Added steps to help you [decide on the object storage configuration](/docs/containers?topic=containers-object_storage#configure_cos), and added troubleshooting steps for when [app pods fail because of an `Operation not permitted` error](/docs/containers?topic=containers-cs_troubleshoot_storage#cos_operation_not_permitted)].</li></ul> |
| 13 October 2020 | <ul><li>**New! Certified Kubernetes version 1.19**: You can now create clusters that run Kubernetes version 1.19. To update an existing cluster, see the [Version 1.19 preparation actions](/docs/containers?topic=containers-cs_versions#cs_v119). The Kubernetes 1.19 release is also certified.</li><li>**Deprecated: Kubernetes version 1.16**: With the release of version 1.19, clusters that run version 1.16 are deprecated. Update your clusters to at least [version 1.17](/docs/containers?topic=containers-cs_versions#cs_v117) today.</li><li>**New! Network load balancer for VPC**: In VPC Gen 2 clusters that run Kubernetes version 1.19, you can now create a layer 4 Network Load Balancer for VPC. VPC network load balancers offer source IP preservation and increased performance through direct server return (DSR). For more information, see [About VPC load balancing in {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-vpc-lbaas#lbaas_about).</li><li>**Version changelogs**: Changelog documentation is available for Kubernetes version [1.19.2_1524](/docs/containers?topic=containers-changelog#1192_1524).</li><li>**VPC load balancer**: Added support for setting the `service.kubernetes.io/ibm-load-balancer-cloud-provider-vpc-node-selector` and `service.kubernetes.io/ibm-load-balancer-cloud-provider-vpc-subnet` annotations when you create new VPC load balancers in clusters that run Kubernetes version 1.19.</li><li>**VPC security groups**: Expanded the list of required rules based on the cluster version for default VPC security groups.</li><li>**{{site.data.keyword.cos_short}} in VPC Gen 2**: Added support for authorized IP addresses in {{site.data.keyword.cos_full_notm}} for VPC Gen 2. For more information, see [Allowing IP addresses for {{site.data.keyword.cos_full_notm}}](/docs/containers?topic=containers-object_storage#cos_auth_ip).</li></ul> |
| 12 October 2020 | **Versions**: Worker node fix pack update changelog documentation is available for Kubernetes version [1.18.9_1530](/docs/containers?topic=containers-changelog#1189_1530), [1.17.12_1542](/docs/containers?topic=containers-changelog#11712_1542), and [1.16.15_1549](/docs/containers?topic=containers-changelog_archive#11615_1549). |
| 08 October 2020 | **Ingress ALB changelog**: Updated the [Kubernetes Ingress image build to `0.35.0_474_iks`](/docs/containers?topic=containers-cluster-add-ons-changelog#0_35_0). |
| 06 October 2020 | <ul><li>**CLI changelog**: Updated the {{site.data.keyword.containerlong_notm}} CLI plug-in changelog page for the [release of version 1.0.178](/docs/containers?topic=containers-cs_cli_changelog#10).</li><li>**Ingress secret expiration synchronization**: Added a troubleshooting topic for when [Ingress secret expiration dates are out of sync or are not updated](/docs/containers?topic=containers-cs_troubleshoot_debug_ingress#sync_cert_dates).</li><li>**Istio add-on**: Versions [1.7.3](/docs/containers?topic=containers-istio-changelog#173) and [1.6.11](/docs/containers?topic=containers-istio-changelog#1611) of the Istio managed add-on are released.</li></ul> |
| 01 October 2020 | <ul><li>**Default version**: The default version for clusters is now Kubernetes 1.18.</li><li>**Ingress ALB changelog**: Updated the [`nginx-ingress` build to 652 and the `ingress-auth` build to 424](/docs/containers?topic=containers-cluster-add-ons-changelog#alb_changelog) for the {{site.data.keyword.containerlong_notm}} Ingress image.</li></ul> |
{: summary="The table shows release notes. Rows are to be read from the left to right, with the date in column one, the title of the feature in column two and a description in column three."}
{: caption="Documentation updates in October 2020"}

## September 2020
{: #sep20}

| Date | Description |
| ---- | ----------- |
| 29 September 2020 | **Gateway firewalls and Calico policies**: For classic clusters in London or Dallas, updated the required IP addresses and ports that you must open in a [public gateway firewall device](/docs/containers?topic=containers-firewall#firewall_outbound), [private gateway device firewall](/docs/containers?topic=containers-firewall#firewall_private), or [Calico network isolation policies](https://github.com/IBM-Cloud/kube-samples/tree/master/calico-policies){: external}. |
| 28 September 2020 | **Versions**: Worker node fix pack update changelog documentation is available for Kubernetes version [1.18.9_1529](/docs/containers?topic=containers-changelog#1189_1529), [1.17.12_1541](/docs/containers?topic=containers-changelog#11712_1541), and [1.16.15_1548](/docs/containers?topic=containers-changelog_archive#11615_1548). |
| 24 September 2020 | **CLI changelog**: Updated the {{site.data.keyword.containerlong_notm}} CLI plug-in changelog page for the [release of version 1.0.171](/docs/containers?topic=containers-cs_cli_changelog#10). |
| 23 September 2020 | <ul><li>**Ingress ALB changelog**: Updated the [`nginx-ingress` build to 651 and the `ingress-auth` build to 423](/docs/containers?topic=containers-cluster-add-ons-changelog#alb_changelog) for the {{site.data.keyword.containerlong_notm}} Ingress image.</li><li>**Istio add-on**: Version [1.7.2](/docs/containers?topic=containers-istio-changelog#172) of the Istio managed add-on is released.</li></ul> |
| 22 September 2020 | **Unsupported: Kubernetes version 1.15**: Clusters that run version 1.15 are unsupported. To continue receiving important security updates and support, you must [update the cluster to a supported version](/docs/containers?topic=containers-cs_versions#prep-up) immediately.|
| 21 September 2020 | **Versions**: Master fix pack update changelog documentation is available for Kubernetes version [1.18.9_1528](/docs/containers?topic=containers-changelog#1189_1528), [1.17.12_1540](/docs/containers?topic=containers-changelog#11712_1540), and [1.16.15_1547](/docs/containers?topic=containers-changelog_archive#11615_1547). |
| 14 September 2020 | <ul><li>**Versions**: Worker node fix pack update changelog documentation is available for Kubernetes version [1.18.8_1527](/docs/containers?topic=containers-changelog#1188_1527), [1.17.11_1539](/docs/containers?topic=containers-changelog#11711_1539), [1.16.14_1546](/docs/containers?topic=containers-changelog_archive#11614_1546), and [1.15.12_1552](/docs/containers?topic=containers-changelog_archive#11512_1552).</li><li>**Istio add-on**: Versions [1.7.1](/docs/containers?topic=containers-istio-changelog#171) and [1.6.9](/docs/containers?topic=containers-istio-changelog#169) of the Istio managed add-on are released.</li><li>**VPC load balancer**: In version 1.18 and later VPC clusters, added the `service.kubernetes.io/ibm-load-balancer-cloud-provider-enable-features: "proxy-protocol"` annotation to preserve the source IP address of requests to apps in your cluster.</li></ul> |
| 04 September 2020 | **New! CIS Kubernetes Benchmark**: Added information about {{site.data.keyword.containerlong_notm}} compliance with the [Center for Internet Security (CIS) Kubernetes Benchmark](/docs/containers?topic=containers-cis-benchmark) 1.5 for clusters that run Kubernetes version 1.18. |
| 03 September 2020 | **CA certificate rotation**: Added steps to [revoke existing certificate authority (CA) certificates in your cluster and issue new CA certificates](/docs/containers?topic=containers-security#cert-rotate).|
| 02 September 2020 | **Istio add-on**: [Version 1.7 of the Istio managed add-on](/docs/containers?topic=containers-istio-changelog#v17) is released. |
| 01 September 2020 | <ul><li>**Deprecation of VPC Gen 1 compute**: VPC Generation 1 is deprecated. If you did not create any VPC Gen 1 resources before 01 September 2020, you can no longer provision any VPC Gen 1 resources. If you created any VPC Gen 1 resources before 01 September 2020, you can continue to provision and use VPC Gen 1 resources until 26 February 2021, when all service for VPC Gen 1 ends and all remaining VPC Gen 1 resources are deleted. To ensure continued support, create new VPC clusters on Generation 2 compute only, and [move your workloads from existing VPC Gen 1 clusters to VPC Gen 2 clusters](/docs/containers?topic=containers-vpc_migrate_tutorial). For more information, see [About Migrating from VPC (Gen 1) to VPC (Gen 2)](/docs/vpc-on-classic?topic=vpc-on-classic-migrating-faqs).</li><li>**Istio add-on**: [Version 1.5.10 of the Istio managed add-on](/docs/containers?topic=containers-istio-changelog#1510) is released.</li></ul>|
{: summary="The table shows release notes. Rows are to be read from the left to right, with the date in column one, the title of the feature in column two and a description in column three."}
{: caption="Documentation updates in September 2020"}


## August 2020
{: #aug20}

| Date | Description |
| ---- | ----------- |
| 31 August 2020 | **Versions**: Worker node fix pack update changelog documentation is available for Kubernetes version [1.18.8_1526](/docs/containers?topic=containers-changelog#1188_1526), [1.17.11_1538](/docs/containers?topic=containers-changelog#11711_1538), [1.16.14_1545](/docs/containers?topic=containers-changelog_archive#11614_1545), and [1.15.12_1551](/docs/containers?topic=containers-changelog_archive#11512_1551). |
| 27 August 2020 | <ul><li>**Observability CLI plug-in**: Added the following commands to manage your {{site.data.keyword.la_full_notm}} and {{site.data.keyword.mon_full_notm}} configurations: [`ibmcloud ob logging agent discover`](/docs/containers?topic=containers-observability_cli#logging_agent_discover), [`ibmcloud ob logging config enable public-endpoint` or `private-endpoint`](/docs/containers?topic=containers-observability_cli#logging_config_enable), [`ibmcloud ob logging config show`](/docs/containers?topic=containers-observability_cli#logging_config_show), [`ibmcloud ob monitoring agent discover`](/docs/containers?topic=containers-observability_cli#monitoring_agent_discover), and [`ibmcloud ob monitoring config enable public-endpoint` or `private-endpoint`](/docs/containers?topic=containers-observability_cli#monitoring_config_enable).</li><li>**New! Worker node flavors**: You can create worker nodes with new [bare metal](/docs/containers?topic=containers-planning_worker_nodes#bm-table) and [bare metal SDS](/docs/containers?topic=containers-planning_worker_nodes#sds-table) flavors in the `me4c` and `mb4c` flavors. These flavors include 2nd Generation Intel® Xeon® Scalable Processors chip sets for better performance for workloads such as machine learning, AI, and IoT.</li></ul>
| 24 August 2020 | <ul><li>**CLI changelog**: Updated the {{site.data.keyword.containerlong_notm}} CLI plug-in changelog page for the [release of version 1.0.157](/docs/containers?topic=containers-cs_cli_changelog#10).</li><li>**Cluster autoscaler**: The [cluster autoscaler](/docs/containers?topic=containers-ca) is available as a managed add-on. The cluster autoscaler Helm chart is deprecated. Migrate your autoscaled worker pools to use the add-on.</li><li>**New! Community Kubernetes Ingress support:** The Ingress ALBs in your cluster can now run the Kubernetes Ingress image, which is built on the community Kubernetes project's implementation of the NGINX Ingress controller. To use the Kubernetes Ingress image, you create your Ingress resources and configmaps according to the Kubernetes Ingress format, including community Kubernetes Ingress annotations instead of custom {{site.data.keyword.containerlong_notm}} annotations. For more information about the differences between the {{site.data.keyword.containerlong_notm}} Ingress image and the Kubernetes Ingress image, see the [Comparison of the ALB image types](/docs/containers?topic=containers-ingress-types#about-alb-images). To get started, see [Creating ALBs that run the Kubernetes Ingress image](/docs/containers?topic=containers-ingress-types#alb-comm-create) or [Changing existing ALBs to run Kubernetes Ingress](/docs/containers?topic=containers-ingress-types#alb-type-migration).</li><li>**New! Default {{site.data.keyword.cloudcerts_long}} instances**: A {{site.data.keyword.cloudcerts_long_notm}} service instance is now created by default for all new and existing standard clusters. The {{site.data.keyword.cloudcerts_short}} service instance, which is named in the format `kube-crtmgr-<cluster_ID>`, stores the TLS certificate for your cluster's default Ingress subdomain. You can also upload your own TLS certificates for custom Ingress domains to this {{site.data.keyword.cloudcerts_short}} instance and use the new [`ibmcloud ks ingress secret` commands](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_ingress_secret_create) to create secrets for these certificates in your cluster. To ensure that a {{site.data.keyword.cloudcerts_short}} instance is automatically created for your new or existing cluster, [verify that the API key for the region and resource group that the cluster is created in has the correct {{site.data.keyword.cloudcerts_short}} permissions](/docs/containers?topic=containers-ingress-types#manage_certs).</li></ul> | 
| 18 August 2020 | **Versions**: Master fix pack update changelog documentation is available for Kubernetes version [1.18.8_1525](/docs/containers?topic=containers-changelog#1188_1525_master), [1.17.11_1537](/docs/containers?topic=containers-changelog#11711_1537_master), [1.16.14_1544](/docs/containers?topic=containers-changelog_archive#11614_1544_master), and [1.15.12_1550](/docs/containers?topic=containers-changelog_archive#11512_1550_master). |
| 17 August 2020 | <ul><li>**Locations**: You can create [clusters on VPC generation 2 compute infrastructure](/docs/containers?topic=containers-regions-and-zones#zones-vpc) in the Tokyo multizone region.</li><li>**Versions**: Worker node fix pack update changelog documentation is available for Kubernetes version [1.18.8_1525](/docs/containers?topic=containers-changelog#1188_1525), [1.17.11_1537](/docs/containers?topic=containers-changelog#11711_1537), [1.16.14_1544](/docs/containers?topic=containers-changelog_archive#11614_1544), and [1.15.12_1550](/docs/containers?topic=containers-changelog_archive#11512_1550).</li><li>**VPC Gen 2**: Added a [tutorial](/docs/containers?topic=containers-vpc_migrate_tutorial) for migrating VPC Gen 1 cluster resources to VPC Gen 2.</li></ul>|
| 12 August 2020 | **Istio add-on**: [Version 1.6.8](/docs/containers?topic=containers-istio-changelog#168) and [version 1.5.9](/docs/containers?topic=containers-istio-changelog#159) of the Istio managed add-on are released. |
| 06 August 2020 | **CLI changelog**: Updated the {{site.data.keyword.containerlong_notm}} CLI plug-in changelog page for the [release of version 1.0.143](/docs/containers?topic=containers-cs_cli_changelog#10). |
| 05 August 2020 | **Ingress ALB changelog**: Updated the [`nginx-ingress` image build to 647 and the `ingress-auth` image build to 421](/docs/containers?topic=containers-cluster-add-ons-changelog#alb_changelog). |
| 04 August 2020 | **Istio add-on**: [Version 1.6.7 of the Istio managed add-on](/docs/containers?topic=containers-istio-changelog#167) is released. |
| 03 August 2020 | <ul><li>**Gateway appliance firewalls**: Updated the [required IP addresses and ports](/docs/containers?topic=containers-firewall#firewall_private) that you must open in a private gateway device firewall.</li><li>**Versions**: Worker node fix pack update changelog documentation is available for Kubernetes version [1.18.6_1523](/docs/containers?topic=containers-changelog#1186_1523), [1.17.9_1535](/docs/containers?topic=containers-changelog#1179_1535), [1.16.13_1542](/docs/containers?topic=containers-changelog_archive#11613_1542), and [1.15.12_1549](/docs/containers?topic=containers-changelog_archive#11512_1549).</li></ul> |
{: summary="The table shows release notes. Rows are to be read from the left to right, with the date in column one, the title of the feature in column two and a description in column three."}
{: caption="Documentation updates in August 2020"}

## July 2020
{: #july20}

| Date | Description |
| ---- | ----------- |
| 31 July 2020 | **{{site.data.keyword.openshiftshort}} version 4.4**: The [{{site.data.keyword.openshiftshort}} version 4.4 release](/docs/containers?topic=containers-cs_versions#cs_v118) is certified for Kubernetes version 1.17. |
| 28 July 2020 | <ul><li>**Ingress ALB versions**: Added the `ibmcloud ks ingress alb versions` CLI command to show the available versions for the ALBs in your cluster and the `ibmcloud ks ingress alb update` CLI command to update or rollback ALBs to a specific version.</li><li>**UI for creating clusters**: Updated getting started and task topics for the updated process for the [Kubernetes cluster creation console](https://cloud.ibm.com/kubernetes/catalog/create){: external}.</li></ul> |
| 24 July 2020 | <ul><li>**Minimum cluster size**: Added an FAQ about [the smallest size cluster that you can make](/docs/containers?topic=containers-faqs#smallest_cluster).</li><li>**Versions**: Master fix pack update changelog documentation is available for Kubernetes version [1.18.6_1522](/docs/containers?topic=containers-changelog#1186_1522), [1.17.9_1534](/docs/containers?topic=containers-changelog#1179_1534), [1.16.13_1541](/docs/containers?topic=containers-changelog_archive#11613_1541), and [1.15.12_1548](/docs/containers?topic=containers-changelog_archive#11512_1548).</li><li>**Worker node replacement**: Added a troubleshooting topic for when [replacing a worker node does not create a worker node](/docs/containers?topic=containers-cs_troubleshoot_clusters#auto-rebalance-off).</li></ul>|
| 20 July 2020 | <ul><li>**Master versions**: Master fix pack update changelog documentation is available for Kubernetes version [1.18.6_1521](/docs/containers?topic=containers-changelog#1186_1521), [1.17.9_1533](/docs/containers?topic=containers-changelog#1179_1533), [1.16.13_1540](/docs/containers?topic=containers-changelog_archive#11613_1540), and [1.15.12_1547](/docs/containers?topic=containers-changelog_archive#11512_1547).</li><li>**Worker node versions**: Worker node fix pack update changelog documentation is available for Kubernetes version [1.18.6_1520](/docs/containers?topic=containers-changelog#1186_1520), [1.17.9_1532](/docs/containers?topic=containers-changelog#1179_1532), [1.16.13_1539](/docs/containers?topic=containers-changelog_archive#11613_1539), and [1.15.12_1546](/docs/containers?topic=containers-changelog_archive#11512_1546).</li></ul> |
| 17 July 2020 | **Istio add-on**: [Version 1.6.5 of the Istio managed add-on](/docs/containers?topic=containers-istio-changelog#165) is released. |
| 16 July 2020 | <ul><li>**Ingress ALB changelog**: Updated the [`nginx-ingress` image build to 645 and the `ingress-auth` image build to 420](/docs/containers?topic=containers-cluster-add-ons-changelog#alb_changelog).</li><li>**Istio add-on**: [Version 1.5.8 of the Istio managed add-on](/docs/containers?topic=containers-istio-changelog#158) is released.</li><li>**Istio ports**: Updated the [required IP addresses and ports](/docs/containers?topic=containers-firewall#firewall_outbound) that you must open in a public gateway device to use the managed Istio add-on.</li><li>**Pod and service subnets**: Added information about bringing your own pod and service subnets in [classic](/docs/containers?topic=containers-subnets#basics_subnets) or [VPC clusters](/docs/containers?topic=containers-vpc-subnets#vpc_basics_subnets).</li></ul> |
| 08 July 2020 | <ul><li>**Istio add-on**: [Version 1.6 of the Istio managed add-on](/docs/containers?topic=containers-istio-changelog#16) is released.</li><li>**Knative add-on**: Version 0.15.1 of the Knative managed add-on is released. If you installed the Knative add-on before, you must uninstall and reinstall the add-on to apply these changes in your cluster.</li></ul> |
| 07 July 2020 | **CLI changelog**: Updated the {{site.data.keyword.containerlong_notm}} CLI plug-in changelog page for the [release of version 1.0.118](/docs/containers?topic=containers-cs_cli_changelog#10). |
| 06 July 2020 | **Version changelogs**: Worker node fix pack update changelog documentation is available for Kubernetes version [1.18.4_1518](/docs/containers?topic=containers-changelog#1184_1518), [1.17.7_1530](/docs/containers?topic=containers-changelog#1177_1530), [1.16.11_1537](/docs/containers?topic=containers-changelog_archive#11611_1537), and [1.15.12_1544](/docs/containers?topic=containers-changelog_archive#11512_1544). |
| 02 July 2020 | **VPC load balancer**: Added support for specifying the `service.kubernetes.io/ibm-load-balancer-cloud-provider-zone` annotation in the [configuration file for VPC load balancers](/docs/containers?topic=containers-vpc-lbaas#setup_vpc_ks_vpc_lb). |
{: summary="The table shows release notes. Rows are to be read from the left to right, with the date in column one, the title of the feature in column two and a description in column three."}
{: caption="Documentation updates in July 2020"}

## June 2020
{: #june20}

| Date | Description |
| ---- | ----------- |
| 24 June 2020 | <ul><li>**Gateway appliance firewalls**: Updated the [required IP addresses and ports](/docs/containers?topic=containers-firewall#firewall_outbound) that you must open in a public gateway device.</li><li>**Ingress ALB changelog**: Updated the [`ingress-auth` image build to 413](/docs/containers?topic=containers-cluster-add-ons-changelog#alb_changelog).</li></ul> |
| 23 June 2020 | **Istio add-on**: [Version 1.5.6 of the Istio managed add-on](/docs/containers?topic=containers-istio-changelog#15) is released.|
| 22 June 2020 | **Version changelogs**: Changelog documentation is available for Kubernetes version [1.18.4_1517](/docs/containers?topic=containers-changelog#1184_1517), [1.17.7_1529](/docs/containers?topic=containers-changelog#1177_1529), [1.16.11_1536](/docs/containers?topic=containers-changelog_archive#11611_1536), and [1.15.12_1543](/docs/containers?topic=containers-changelog_archive#11512_1543).
| 18 June 2020 | **Ingress ALB changelog**: Updated the [`ingress-auth` image build to 413](/docs/containers?topic=containers-cluster-add-ons-changelog#alb_changelog).|
| 16 June 2020 | **CLI changelog**: Updated the {{site.data.keyword.containerlong_notm}} CLI plug-in changelog page for the [release of version 1.0.99](/docs/containers?topic=containers-cs_cli_changelog#10).|
| 09 June 2020 | <ul><li>**CLI changelog**: Updated the {{site.data.keyword.containerlong_notm}} CLI plug-in changelog page for the [release of version 1.0.94](/docs/containers?topic=containers-cs_cli_changelog#10).</li><li>**Permissions**: Added a [Permissions to create a cluster](/docs/containers?topic=containers-access_reference#cluster_create_permissions) reference topic, and restructured cluster creation and user access topics to refer to this reference topic.</li><li>**New! Static routes add-on**: Added information about creating static routes on your worker nodes by enabling the [static routes cluster add-on](/docs/containers?topic=containers-static-routes).</li></ul> |
| 08 June 2020 | **Version changelogs**: Worker node changelog documentation is available for Kubernetes [1.18.3_1515](/docs/containers?topic=containers-changelog#1183_1515), [1.17.6_1527](/docs/containers?topic=containers-changelog#1176_1527), [1.16.10_1534](/docs/containers?topic=containers-changelog_archive#11610_1534), and [1.15.12_1541](/docs/containers?topic=containers-changelog_archive#11512_1541). |
| 04 June 2020 | <ul><li>**Gateway appliance firewalls**: Updated the [required IP addresses and ports](/docs/containers?topic=containers-firewall#firewall_outbound) that you must open in a public gateway device firewall.</li><li>**Ingress ALB changelog**: Updated the [`nginx-ingress` image build to 637](/docs/containers?topic=containers-cluster-add-ons-changelog#alb_changelog).</li><li>**Istio mTLS**: Changed the sample [mTLS policy](/docs/containers?topic=containers-istio-mesh#mtls) to use `kind: "PeerAuthentication"`.</li><li>**VPC network security**: Expanded information about your options for using access control lists (ACLs), security groups, and other network policies to [control traffic to and from your VPC cluster](/docs/containers?topic=containers-vpc-network-policy).</li></ul>|
| 01 June 2020 | <ul><li>**Kubernetes 1.17**: [Kubernetes 1.17](/docs/containers?topic=containers-cs_versions#version_types) is now the default version.</li><li>**VPC ACLs**: Added required rules for using VPC load balancers to steps for [Creating access control lists (ACLs) to control traffic to and from your VPC cluster](/docs/containers?topic=containers-vpc-network-policy#acls).</li></ul>|
{: summary="The table shows release notes. Rows are to be read from the left to right, with the date in column one, the title of the feature in column two and a description in column three."}
{: caption="Documentation updates in June 2020"}

## May 2020
{: #may20}

| Date | Description |
| ---- | ----------- |
| 31 May 2020 | **Unsupported: Kubernetes version 1.14**: With the release of version 1.18, clusters that run version 1.14 are unsupported. To continue receiving important security updates and support, you must [update the cluster to a supported version](/docs/containers?topic=containers-cs_versions#prep-up) immediately. |
| 26 May 2020 | <ul><li>**CLI changelog**: Updated the {{site.data.keyword.containerlong_notm}} CLI plug-in changelog page for the [release of version 1.0.84](/docs/containers?topic=containers-cs_cli_changelog#10).</li><li>**Image pull secrets**: Now, the `default-icr-io` and `default-<region>-icr-io` image pull secrets in the `default` project are replaced by a single `all-icr-io` image pull secret that has credentials to all the public and private regional registry domains. Clusters that run Kubernetes 1.15 - 1.17 still have the previous `default-<region>-icr-io` image pull secrets for backwards compatibility.</li><li>**Ingress status**: Added information about [health reporting for your Ingress components](/docs/containers?topic=containers-cs_troubleshoot_debug_ingress#ingress-status).</li><li>**NodeLocal DNS cache**: Added how to customize the [NodeLocal DNS cache](/docs/containers?topic=containers-cluster_dns#dns_nodelocal_customize).<li>**Version changelogs**: Master and worker node changelog documentation is available for Kubernetes [1.18.3_1514](/docs/containers?topic=containers-changelog#1183_1514), [1.17.6_1526](/docs/containers?topic=containers-changelog#1176_1526), [1.16.10_1533](/docs/containers?topic=containers-changelog_archive#11610_1533), [1.15.12_1540](/docs/containers?topic=containers-changelog_archive#11512_1540), and [1.14.10_1555](/docs/containers?topic=containers-changelog_archive#11410_1555).</li></ul> |
| 20 May 2020 | **New! Virtual Private Cloud Generation 2**: You can now create standard Kubernetes clusters in your [Gen 2 Virtual Private Cloud (VPC)](/docs/vpc?topic=vpc-getting-started). VPC gives you the security of a private cloud environment with the dynamic scalability of a public cloud. VPC Gen 2 clusters are available for only standard, Kubernetes clusters and are not supported in free clusters.<br><br> For more information, check out the following links:<ul><li>[Overview of Classic and VPC infrastructure providers](/docs/containers?topic=containers-infrastructure_providers)</li><li>[Supported virtual machine flavors for VPC Gen 2 worker nodes](/docs/containers?topic=containers-planning_worker_nodes#vm)</li><li>[New VPC Gen 2 commands for the CLI](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli)</li><li>[VPC Gen 2 compute cluster limitations](/docs/containers?topic=containers-limitations#ks_vpc_gen2_limits)</li></ul>Ready to get started? Try out the [Creating a cluster in your VPC on generation 2 compute tutorial](/docs/containers?topic=containers-vpc_ks_tutorial).|
| 19 May 2020 | **Istio add-on**: [Version 1.5 of the Istio managed add-on](/docs/containers?topic=containers-istio-changelog#15) is released.|
| 18 May 2020 | **Ingress ALB changelog**: Updated the [`nginx-ingress` image build to 628](/docs/containers?topic=containers-cluster-add-ons-changelog#alb_changelog).|
| 14 May 2020 | **Kubernetes version 1.18**: [Kubernetes 1.18 release](/docs/containers?topic=containers-cs_versions#cs_v118) is certified.|
| 11 May 2020 | <ul><li>**New! Kubernetes version 1.18**: You can now create clusters that run Kubernetes version 1.18. To update an existing cluster, see the [Version 1.18 preparation actions](/docs/containers?topic=containers-cs_versions#cs_v118).</li><li>**Deprecated: Kubernetes version 1.15**: With the release of version 1.18, clusters that run version 1.15 are deprecated. Consider [updating to at least version 1.16](/docs/containers?topic=containers-cs_versions#cs_v116) today.</li><li>**NodeLocal DNS cache**: [NodeLocal DNS cache](/docs/containers?topic=containers-cluster_dns#dns_cache) is generally available for clusters that run Kubernetes 1.18, but still not enabled by default.</li><li>**Zone-aware DNS beta**: For multizone clusters that run Kubernetes 1.18, you can set up [zone-aware DNS](/docs/containers?topic=containers-cluster_dns#dns_zone_aware) to improve DNS performance and availability.</li><li>**Version changelogs**: Changelog documentation is available for Kubernetes version [1.18.2_1512](/docs/containers?topic=containers-changelog#1182_1512). Worker node patch updates are available for Kubernetes [1.17.5_1524](/docs/containers?topic=containers-changelog#1175_1524), [1.16.9_1531](/docs/containers?topic=containers-changelog_archive#1169_1531), [1.15.11_1538](/docs/containers?topic=containers-changelog_archive#11511_1538), and [1.14.10_1554](/docs/containers?topic=containers-changelog_archive#11410_1554).</li></ul> |
| 08 May 2020 | **CLI changelog**: Updated the {{site.data.keyword.containerlong_notm}} CLI plug-in changelog page for the [release of version 1.0.57](/docs/containers?topic=containers-cs_cli_changelog#10). |
| 06 May 2020 | **New! {{site.data.keyword.containerlong_notm}} observability plug-in**: You can now use the {{site.data.keyword.containerlong_notm}} observability plug-in to create a logging or monitoring configuration for your cluster so that you can forward cluster logs and metrics to an {{site.data.keyword.la_full_notm}} and {{site.data.keyword.mon_full_notm}} service instance. For more information, see [Forwarding cluster and app logs to {{site.data.keyword.la_full_notm}}](/docs/containers?topic=containers-health#logging) and [Viewing cluster and app metrics with {{site.data.keyword.mon_full_notm}}](/docs/containers?topic=containers-health-monitor). You can also use the command line to create the logging and monitoring configuration. For more information, see the [Observability plug-in CLI](/docs/containers?topic=containers-observability_cli) reference. |
| 04 May 2020 | <ul><li>**Gateway appliance firewalls**: Updated the [required IP addresses and ports](/docs/containers?topic=containers-firewall#firewall_outbound) that you must open in a public gateway device firewall.</li><li>**Ingress troubleshooting**:Added a [troubleshooting topic](/docs/containers?topic=containers-cs_troubleshoot_debug_ingress#alb-pod-affinity) for when ALB pods do not deploy correctly to worker nodes.</li></ul> |
{: summary="The table shows release notes. Rows are to be read from the left to right, with the date in column one, the title of the feature in column two and a description in column three."}
{: caption="Documentation updates in May 2020"}

## April 2020
{: #apr20}

| Date | Description |
| ---- | ----------- |
| 30 April 2020 | **Cluster and worker node quotas**: Now, each region in your {{site.data.keyword.cloud_notm}} account has quotas for {{site.data.keyword.containershort}} clusters and workers. You can have **100 clusters** and **500 worker nodes** across clusters per region and per [infrastructure provider](/docs/containers?topic=containers-infrastructure_providers). With quotas in place, your account is better protected from accidental requests or billing surprises. Need more clusters? No problem, just [contact IBM Support](/docs/get-support?topic=get-support-using-avatar). In the support case, include the new cluster or worker node quota limit for the region and infrastructure provider that you want. For more information, see the [Service limitations](/docs/containers?topic=containers-limitations). |
| 29 April 2020 | <ul><li>**ALB pod scaling**: Added steps for scaling up your ALB processing capabilities by [increasing the number of ALB pods replicas](/docs/containers?topic=containers-ingress#alb_replicas).</li><li>**VPC cluster architecture**: Refreshed [networking diagrams for VPC scenarios](/docs/containers?topic=containers-service-arch#architecture_vpc).</li><li>**VPC cluster master access**: Updated [accessing the master via a private cloud service endpoint](/docs/containers?topic=containers-access_cluster#vpc_private_se) for VPC clusters. </li></ul> |
| 27 April 2020 | <ul><li>**Ingress ALB changelog**: Updated the [`ingress-auth` image build to 401](/docs/containers?topic=containers-cluster-add-ons-changelog#alb_changelog).</li><li>**Version changelogs**: Worker node patch updates are available for Kubernetes [1.17.5_1523](/docs/containers?topic=containers-changelog#1175_1523), [1.16.9_1530](/docs/containers?topic=containers-changelog_archive#1169_1530), [1.15.11_1537](/docs/containers?topic=containers-changelog_archive#11511_1537), and [1.14.10_1553](/docs/containers?topic=containers-changelog_archive#11410_1553).</li></ul> |
| 23 April 2020 | **Version changelogs**: Master patch updates are available for Kubernetes [1.17.5_1522](/docs/containers?topic=containers-changelog#1175_1522), [1.16.9_1529](/docs/containers?topic=containers-changelog_archive#1169_1529), [1.15.11_1536](/docs/containers?topic=containers-changelog_archive#11511_1536), and [1.14.10_1552](/docs/containers?topic=containers-changelog_archive#11410_1552). |
| 22 April 2020 | <ul><li>**Private network connection to registry**: For accounts that have VRF and service endpoints enabled, image push and pull traffic to {{site.data.keyword.registrylong_notm}} is now on [the private network](/docs/containers?topic=containers-registry#cluster_registry_auth_private).</li></ul>|
| 17 April 2020 | **Version changelog**: Master patch updates are available for Kubernetes [1.17.4_1521](/docs/containers?topic=containers-changelog#1174_1521_master). |
| 16 April 2020 | **Ingress ALB changelog**: Updated the [`ingress-auth` image build to 394](/docs/containers?topic=containers-cluster-add-ons-changelog#alb_changelog).|
| 13 April 2020 | <ul><li>**Gateway appliance firewalls**: Updated the [required IP addresses and ports](/docs/containers?topic=containers-firewall#firewall_private) that you must open in a private gateway device firewall for {{site.data.keyword.registrylong_notm}}.</li><li>**Version changelogs**: Worker node patch updates are available for Kubernetes [1.17.4_1521](/docs/containers?topic=containers-changelog#1174_1521), [1.16.7_1528](/docs/containers?topic=containers-changelog_archive#1168_1528), [1.15.10_1535](/docs/containers?topic=containers-changelog_archive#11511_1535), and [1.14.10_1551](/docs/containers?topic=containers-changelog_archive#11410_1551).</li></ul>
| 06 April 2020 | <ul><li>**CLI changelog**: Updated the {{site.data.keyword.containerlong_notm}} CLI plug-in changelog page for the [release of version 1.0.28](/docs/containers?topic=containers-cs_cli_changelog#10).</li><li>**Kubernetes cluster context**: Added topics for [Setting the Kubernetes context for multiple clusters](/docs/containers?topic=containers-cs_cli_install#cli_config_multiple) and [Creating a temporary `kubeconfig` file](/docs/containers?topic=containers-cs_cli_install#cli_temp_kubeconfig).</li></ul> |
| 01 April 2020 | **Istio add-on**: [Version 1.4.7 of the Istio managed add-on](/docs/containers?topic=containers-istio-changelog#147) is released.|
{: summary="The table shows release notes. Rows are to be read from the left to right, with the date in column one, the title of the feature in column two and a description in column three."}
{: caption="Documentation updates in April 2020"}

## March 2020
{: #mar20}

| Date | Description |
| ---- | ----------- |
| 30 March 2020 | <ul><li>**Version changelogs**: Worker node patch updates are available for Kubernetes [1.17.3_1520](/docs/containers?topic=containers-changelog#1174_1520), [1.16.7_1527](/docs/containers?topic=containers-changelog_archive#1168_1527), [1.15.10_1534](/docs/containers?topic=containers-changelog_archive#11511_1534), and [1.14.10_1550](/docs/containers?topic=containers-changelog_archive#11410_1550).</li><li>**`gid` file storage classes**: Added `gid` file storage classes to specify a supplemental group ID that you can assign to a non-root user ID so that the non-root user can read and write to the file storage instance. For more information, see the [storage class reference](/docs/containers?topic=containers-file_storage#file_storageclass_reference). </li></ul> |
| 27 March 2020 | **Tech overview**: Added an [Overview of personal and sensitive data storage and removal options](/docs/containers?topic=containers-service-arch#ibm-data). |
| 25 March 2020 | **Ingress ALB changelog**: Updated the [`nginx-ingress` image build to 627](/docs/containers?topic=containers-cluster-add-ons-changelog#alb_changelog). |
| 24 March 2020 | <ul><li>**CLI changelog**: Updated the {{site.data.keyword.containerlong_notm}} CLI plug-in changelog page for the [release of version 1.0.15](/docs/containers?topic=containers-cs_cli_changelog#10).</li><li>**Service dependencies**: Added information about [dependencies on other {{site.data.keyword.cloud_notm}} and 3rd party services](/docs/containers?topic=containers-service-arch#dependencies-ibmcloud).</li></ul> |
| 18 March 2020 | <ul><li>**IAM issuer details**: Added a [reference topic](/docs/containers?topic=containers-access_reference#iam_issuer_users) for the IAM issuer details of RBAC users.</li></ul> |
| 16 March 2020 | <ul><li>**New! CLI 1.0**: Updated the {{site.data.keyword.containerlong_notm}} CLI plug-in changelog page for the [release of version 1.0.0](/docs/containers?topic=containers-cs_cli_changelog#10). This version contains permanent syntax and behavior changes that are not compatible with earlier versions, so before you update be sure to follow the guidance in [Using version 1.0 of the plug-in](/docs/containers?topic=containers-cs_cli_changelog#changelog_beta).</li><li>**Installing SGX drivers**: Added a topic for [installing SGX drivers and platform software on SGX-capable worker nodes](/docs/containers?topic=containers-add_workers#install-sgx).</li><li>**Sizing workloads**: Enhanced the topic with a [How do I monitor resource usage and capacity in my cluster?](/docs/containers?topic=containers-strategy#sizing_manage) FAQ.</li><li>**`sticky-cookie-services` annotation**: Added the `secure` and `httponly` parameters to the [`sticky-cookie-services` annotation](/docs/containers?topic=containers-ingress_annotation#sticky-cookie-services).</li><li>**Version changelogs**: Master and worker node patch updates are available for Kubernetes [1.17.4_1519](/docs/containers?topic=containers-changelog#1174_1519), [1.16.8_1526](/docs/containers?topic=containers-changelog_archive#1168_1526), [1.15.11_1533](/docs/containers?topic=containers-changelog_archive#11511_1533), and [1.14.10_1549](/docs/containers?topic=containers-changelog_archive#11410_1549)</li></ul> |
| 12 March 2020 | **Version update policy**: Now, you can update your cluster master version only to the next version (`n+1`). For example, if your cluster runs Kubernetes version 1.15 and you want to update to the latest version, 1.17, you must first update to 1.16.|
| 10 March 2020 | **Istio add-on**: [Version 1.4.6 of the Istio managed add-on](/docs/containers?topic=containers-istio-changelog#146) is released.|
| 09 March 2020 | <ul><li>**Add-on troubleshooting**: Added a troubleshooting page for managed add-ons, including a topic for [reviewing add-on state and statuses](/docs/containers?topic=containers-cs_troubleshoot_addons#debug_addons).</li><li>**Managing Ingress ALBs**: Added a page for [managing the lifecycle of your ALBs](/docs/containers?topic=containers-ingress), including information about creating, updating, and moving ALBs.</li><li>**Kubernetes 1.16**: [Kubernetes 1.16](/docs/containers?topic=containers-cs_versions#version_types) is now the default version.</li><li>**Updating the {{site.data.keyword.block_storage_is_short}} add-on**: Added steps to update the {{site.data.keyword.block_storage_is_short}} add-on.</li></ul> |
| 04 March 2020 | <ul><li>**CLI changelog**: Updated the {{site.data.keyword.containerlong_notm}} CLI plug-in changelog page for the [release of version 0.4.102](/docs/containers?topic=containers-cs_cli_changelog#04).</li><li>**Ingress ALB changelog**: Updated the [`ingress-auth` image build to 390](/docs/containers?topic=containers-cluster-add-ons-changelog#alb_changelog).</li></ul> |
| 02 March 2020 | **Version changelogs**: Worker node patch updates are available for Kubernetes [1.17.3_1518](/docs/containers?topic=containers-changelog#1173_1518), [1.16.7_1525](/docs/containers?topic=containers-changelog_archive#1167_1525), [1.15.10_1532](/docs/containers?topic=containers-changelog_archive#11510_1532), and [1.14.10_1548](/docs/containers?topic=containers-changelog_archive#11410_1548). |
{: summary="The table shows release notes. Rows are to be read from the left to right, with the date in column one, the title of the feature in column two and a description in column three."}
{: caption="Documentation updates in March 2020"}

## February 2020
{: #feb20}

| Date | Description |
| ---- | ----------- |
| 28 February 2020 | **VPC clusters**: You can create VPC generation 1 compute clusters in the [**Washington DC** location (`us-east` region)](/docs/containers?topic=containers-regions-and-zones#zones-vpc). |
| 22 February 2020 | **Unsupported: Kubernetes version 1.13**: With the release of version 1.17, clusters that run version 1.13 are unsupported. To continue receiving important security updates and support, you must [update the cluster to a supported version](/docs/containers?topic=containers-cs_versions#prep-up) immediately. |
| 19 February 2020 | <ul><li>**CLI changelog**: Updated the {{site.data.keyword.containerlong_notm}} CLI plug-in changelog page for the [release of version 0.4.90](/docs/containers?topic=containers-cs_cli_changelog).</li><li>**Developing and deploying apps**: You can now find expanded information on how to develop and deploy an app to your Kubernetes cluster in the following pages:<ul><li>[Planning app deployments](/docs/containers?topic=containers-plan_deploy)</li><li>[Building containers from images](/docs/containers?topic=containers-images)</li><li>[Developing Kubernetes-native apps](/docs/containers?topic=containers-app)</li><li>[Deploying Kubernetes-native apps in clusters](/docs/containers?topic=containers-deploy_app)</li><li>[Managing the app lifecycle](/docs/containers?topic=containers-update_app)</li></ul></li><li>**Learning paths**: Curated learning paths for [administrators](/docs/containers?topic=containers-learning-path-admin) and [developers](/docs/containers?topic=containers-learning-path-dev) are now available to help guide you through your {{site.data.keyword.containerlong_notm}} experience.</li><li>**Setting up image build pipelines**: You can now find expanded information on how to set up an image registry and build pipelines in the following pages:<ul><li>[Setting up an image registry](/docs/containers?topic=containers-registry)</li><li>[Setting up continuous integration and delivery](/docs/containers?topic=containers-cicd)</li></ul></li><li>**Firewall subnets**: Removed outdated [subnet IP addresses for {{site.data.keyword.registrylong_notm}}](/docs/containers?topic=containers-firewall#vyatta_firewall).</li></ul> |
| 17 February 2020 | <ul><li>**Kubernetes version 1.17**: [Kubernetes 1.17 release](/docs/containers?topic=containers-cs_versions#cs_v117) is certified.</li><li>**Version changelogs**: Master and worker node patch updates are available for Kubernetes [1.17.3_1516](/docs/containers?topic=containers-changelog#1173_1516), [1.16.7_1524](/docs/containers?topic=containers-changelog_archive#1167_1524), [1.15.10_1531](/docs/containers?topic=containers-changelog_archive#11510_1531), [1.14.10_1547](/docs/containers?topic=containers-changelog_archive#11410_1547), and [1.13.12_1550](/docs/containers?topic=containers-changelog_archive#11312_1550)</li></ul>|
| 14 February 2020 | **Istio add-on**: [Version 1.4.4 of the Istio managed add-on](/docs/containers?topic=containers-istio-changelog#144) is released. |
| 10 February 2020 | <ul><li>**New! Kubernetes version 1.17**: You can now create clusters that run Kubernetes version 1.17. To update an existing cluster, see the [Version 1.17 preparation actions](/docs/containers?topic=containers-cs_versions#cs_v117).</li><li>**Deprecated: Kubernetes version 1.14**: With the release of version 1.17, clusters that run version 1.14 are deprecated. Consider [updating to at least version 1.15](/docs/containers?topic=containers-cs_versions#cs_v115) today.</li><li>**VPC cluster creation troubleshooting**: Added [troubleshooting steps](/docs/containers?topic=containers-cs_troubleshoot#ts_no_vpc) for when no VPCs are listed when you try to create a VPC cluster in the console.</li><li>**Knative changelogs:** Check out the changes that are included in version 0.12.1 of the managed Knative add-on. If you installed the Knative add-on before, you must uninstall and reinstall the add-on to apply these changes in your cluster.</li> </ul> |
| 06 February 2020 | <ul><li>**Cluster autoscaler**: Added a [debugging guide for the cluster autoscaler](/docs/containers?topic=containers-troubleshoot_cluster_autoscaler).</li><li>**Tags**: Added how to [add {{site.data.keyword.cloud_notm}} tags to existing clusters](/docs/containers?topic=containers-add_workers#cluster_tags).</li><li>**VPC security groups**: [Modify the security group rules](/docs/containers?topic=containers-vpc-network-policy#security_groups) for VPC Gen 2 clusters to allow traffic requests that are routed to node ports on your worker nodes.</li></ul> |
| 03 February 2020 | <ul><li>**Version changelog**: Updates are available for Kubernetes worker node fix packs [1.16.5_1523](/docs/containers?topic=containers-changelog_archive#1165_1523), [1.15.8_1530](/docs/containers?topic=containers-changelog_archive#1158_1530), [1.14.10_1546](/docs/containers?topic=containers-changelog_archive#11410_1546), and [1.13.12_1549](/docs/containers?topic=containers-changelog_archive#11312_1549).</li><li>**Expanded troubleshooting**: You can now find troubleshooting steps for {{site.data.keyword.containerlong_notm}} clusters in the following pages:<ul><li>[Clusters and masters](/docs/containers?topic=containers-cs_troubleshoot)</li><li>[Worker nodes](/docs/containers?topic=containers-cs_troubleshoot_clusters)</li><li>[Cluster networking](/docs/containers?topic=containers-cs_troubleshoot_network)</li><li>[Apps and integrations](/docs/containers?topic=containers-cs_troubleshoot_app)</li><li>[Load balancers](/docs/containers?topic=containers-cs_troubleshoot_lb)</li><li>[Ingress](/docs/containers?topic=containers-cs_troubleshoot_debug_ingress)</li><li>[Persistent storage](/docs/containers?topic=containers-cs_troubleshoot_storage)</li></ul></li></ul> |
{: summary="The table shows release notes. Rows are to be read from the left to right, with the date in column one, the title of the feature in column two and a description in column three."}
{: caption="Documentation updates in February 2020"}

## January 2020
{: #jan20}

| Date | Description |
| ---- | ----------- |
| 30 January 2020 | **Ingress ALB changelog**: Updated the [`nginx-ingress` image build to 625](/docs/containers?topic=containers-cluster-add-ons-changelog#alb_changelog). |
| 27 January 2020 | <ul><li>**Istio changelog**: Added an [Istio add-on version changelog](/docs/containers?topic=containers-istio-changelog).</li><li>**Back up and restore File and Block storage**: Added steps for deploying the [<code>ibmcloud-backup-restore</code> Helm chart](/docs/containers?topic=containers-utilities#ibmcloud-backup-restore).</li></ul> |
| 22 January 2020 | **Kubernetes 1.15**: [Kubernetes 1.15](/docs/containers?topic=containers-cs_versions#version_types) is now the default version. |
| 21 January 2020 | **Default {{site.data.keyword.containerlong_notm}} settings**: Review the [default settings](/docs/containers?topic=containers-service-settings) for Kubernetes components, such as the `kube-apiserver`, `kubelet`, or `kube-proxy`. |
| 20 January 2020 | <ul><li>**Helm version 3**: Updated [Adding services by using Helm charts](/docs/containers?topic=containers-helm) to include steps for installing Helm v3 in your cluster. Migrate to Helm v3 today for several advantages over Helm v2, such as the removal of the Helm server, Tiller.</li><li>**Ingress ALB changelog**: Updated the [`nginx-ingress` image build to 621](/docs/containers?topic=containers-cluster-add-ons-changelog#alb_changelog).</li><li>**Version changelog**: Patch updates are available for Kubernetes [1.16.5_1522](/docs/containers?topic=containers-changelog_archive#1165_1522), [1.15.8_1529](/docs/containers?topic=containers-changelog_archive#1158_1529), [1.14.10_1545](/docs/containers?topic=containers-changelog_archive#11410_1545), and [1.13.12_1548](/docs/containers?topic=containers-changelog_archive#11312_1548).</li></ul> |
| 13 January 2020 | **{{site.data.keyword.blockstorageshort}}**: Added steps for [adding raw {{site.data.keyword.blockstorageshort}} to VPC worker nodes](/docs/containers?topic=containers-utilities#vpc_api_attach). |
| 06 January 2020 | **Ingress ALB changelog**: Updated the [`ingress-auth` image to build 373](/docs/containers?topic=containers-cluster-add-ons-changelog#alb_changelog).|
| 03 January 2020 | **Version changelog**: Worker node patch updates are available for Kubernetes [1.16.3_1521](/docs/containers?topic=containers-changelog_archive#1163_1521), [1.15.6_1528](/docs/containers?topic=containers-changelog_archive#1156_1528), [1.14.9_1544](/docs/containers?topic=containers-changelog_archive#1149_1544), and [1.13.12_1547](/docs/containers?topic=containers-changelog_archive#11312_1547).|
{: summary="The table shows release notes. Rows are to be read from the left to right, with the date in column one, the title of the feature in column two and a description in column three."}
{: caption="Documentation updates in January 2020"}

## December 2019
{: #dec19}

| Date | Description |
| ---- | ----------- |
| 18 December 2019 | **Ingress ALB changelog**: Updated the [`nginx-ingress` image build to 615 and the `ingress-auth` image to build 372](/docs/containers?topic=containers-cluster-add-ons-changelog#alb_changelog). |
| 17 December 2019 | <ul><li>**Version changelog**: Master patch updates are available for Kubernetes [1.16.3_1520](/docs/containers?topic=containers-changelog_archive#1163_1520), [1.15.6_1527](/docs/containers?topic=containers-changelog_archive#1156_1527), [1.14.9_1543](/docs/containers?topic=containers-changelog_archive#1149_1543), and [1.13.12_1546](/docs/containers?topic=containers-changelog_archive#11312_1546).</li><li>**Adding classic infrastructure servers to gateway-enabled clusters**: [Adding classic IBM Cloud infrastructure server instances to your cluster network](/docs/containers?topic=containers-add_workers#gateway_vsi) is now generally available for classic gateway-enabled clusters.</li><li>**Assigning access**: Updated the steps to [assign access to your clusters through the {{site.data.keyword.cloud_notm}} IAM console](/docs/containers?topic=containers-users#add_users).</li></ul>|
| 12 December 2019 | **Setting up a service mesh with Istio**: Includes the following new pages:<ul><li>[About the managed Istio add-on](/docs/containers?topic=containers-istio-about)</li><li>[Setting up Istio](/docs/containers?topic=containers-istio)</li><li>[Managing apps in the service mesh](/docs/containers?topic=containers-istio-mesh)</li><li>[Observing Istio traffic](/docs/containers?topic=containers-istio-health)</li></ul>|
| 11 December 2019 | <ul><li>**CLI changelog**: Updated the {{site.data.keyword.containerlong_notm}} CLI plug-in changelog page for the [release of version 0.4.64](/docs/containers?topic=containers-cs_cli_changelog).</li><li>**Configuring VPC subnets**: [Added information](/docs/containers?topic=containers-vpc-subnets) about configuring VPC subnets, public gateways, and network segmentation for your VPC clusters.</li><li>**Kubernetes version lifecycles**: Added information about [the release lifecycle of supported Kubernetes versions](/docs/containers?topic=containers-cs_versions#release_lifecycle).</li><li>**Managed Knative add-on**: Added information about Istio version support.</li></ul>|
| 09 December 2019 | **Version changelog**: Worker node patch updates are available for Kubernetes [1.16.3_1519](/docs/containers?topic=containers-changelog_archive#1163_1519_worker), [1.15.6_1526](/docs/containers?topic=containers-changelog_archive#1156_1526_worker), [1.14.9_1542](/docs/containers?topic=containers-changelog_archive#1149_1542_worker), and [1.13.12_1545](/docs/containers?topic=containers-changelog_archive#11312_1545_worker). |
| 04 December 2019 | <ul><li>**Exposing apps with load balancers or Ingress ALBs**: Added quick start pages to help you get up and running with [load balancers](/docs/containers?topic=containers-loadbalancer-qs) and [Ingress ALBs](/docs/containers?topic=containers-ingress#ingress-qs).</li><li>**Kubernetes web terminal for VPC clusters**: To use the Kubernetes web terminal for VPC clusters, make sure to [configure access to external endpoints](/docs/containers?topic=containers-cs_cli_install#cli_web).</li><li>**Monitoring Istio**: Added [steps](/docs/containers?topic=containers-istio-health#istio_inspect) to launch the ControlZ component inspection and Envoy sidecar dashboards for the Istio managed add-on.</li><li>**Tuning network performance**: Added [steps](/docs/containers?topic=containers-kernel#calico-portmap) for disabling the `portmap` plug-in for for the Calico container network interface (CNI).</li><li>**Use the internal KVDB in Portworx**: Automatically set up a key-value database (KVDB) during the Portworx installation to store your Portworx metadata. For more information, see [Using the Portworx KVDB](/docs/containers?topic=containers-portworx#portworx-kvdb).</li></ul>|
{: summary="The table shows release notes. Rows are to be read from the left to right, with the date in column one, the title of the feature in column two and a description in column three."}
{: caption="Documentation updates in December 2019"}

## November 2019
{: #nov19}

<table summary="The table shows release notes. Rows are to be read from the left to right, with the date in column one, the title of the feature in column two and a description in column three.">
<caption>Documentation updates in November 2019</caption>
<thead>
<th>Date</th>
<th>Description</th>
</thead>
<tbody>
<tr>
  <td>26 November 2019</td>
  <td><ul><li><strong>CLI changelog</strong>: Updated the {{site.data.keyword.containerlong_notm}} CLI plug-in changelog page for the [release of version 0.4.61](/docs/containers?topic=containers-cs_cli_changelog).</li>
  <li><strong>Cluster autoscaling for VPC clusters</strong>: You can [set up the cluster autoscaler](/docs/containers?topic=containers-ca#ca_helm) on clusters that run on the first generation of compute for Virtual Private Cloud (VPC).</li>
  <li><strong>New! Reservations and limits for PIDs</strong>: Worker nodes that run Kubernetes version 1.14 or later set [process ID (PID) reservations and limits that vary by flavor](/docs/containers?topic=containers-planning_worker_nodes#resource_limit_node), to help prevent malicious or runaway apps from consuming all available PIDs.</li>
  <li><strong>Version changelog</strong>: Worker node patch updates are available for Kubernetes [1.16.3_1518](/docs/containers?topic=containers-changelog_archive#1163_1518_worker), [1.15.6_1525](/docs/containers?topic=containers-changelog_archive#1156_1525_worker), [1.14.9_1541](/docs/containers?topic=containers-changelog_archive#1149_1541_worker), and [1.13.12_1544](/docs/containers?topic=containers-changelog_archive#11312_1544_worker).</li></ul></td>
</tr>
<tr>
<td>22 November 2019</td>
<td><ul>
<li><strong>Bring your own DNS for load balancers</strong>: Added steps for bringing your own custom domain for [NLBs](/docs/containers?topic=containers-loadbalancer_hostname#loadbalancer_hostname_dns) in classic clusters and [VPC load balancers](/docs/containers?topic=containers-vpc-lbaas#vpc_lb_dns) in VPC clusters.</li>
<li><strong>Gateway appliance firewalls</strong>: Updated the [required IP addresses and ports](/docs/containers?topic=containers-firewall#vyatta_firewall) that you must open in a public gateway device firewall.</li>
<li><strong>Ingress ALB subdomain format</strong>: [Changes are made to the Ingress subdomain](/docs/containers?topic=containers-ingress-about#ingress-resource). New clusters are assigned an Ingress subdomain in the format `<cluster_name>.<globally_unique_account_HASH>-0000.<region>.containers.appdomain.cloud` and an Ingress secret in the format `<cluster_name>.<globally_unique_account_HASH>-0000`. Any existing clusters that use the `<cluster_name>.<region>.containers.mybluemix.net` subdomain are assigned a CNAME record that maps to a `<cluster_name>.<region_or_zone>.containers.appdomain.cloud` subdomain.</li>
</ul></td>
</tr>
<tr>
<td>21 November 2019</td>
<td><ul>
<li><strong>Ingress ALB changelog</strong>: Updated the [ALB `nginx-ingress` image to 597 and the `ingress-auth` image to build 353](/docs/containers?topic=containers-cluster-add-ons-changelog#alb_changelog).</li>
<li><strong>Version changelog</strong>: Master patch updates are available for Kubernetes [1.16.3_1518](/docs/containers?topic=containers-changelog_archive#1163_1518), [1.15.6_1525](/docs/containers?topic=containers-changelog_archive#1156_1525), [1.14.9_1541](/docs/containers?topic=containers-changelog_archive#1149_1541), and [1.13.12_1544](/docs/containers?topic=containers-changelog_archive#11312_1544).</li>
</ul></td>
</tr>
<tr>
<td>19 November 2019</td>
<td><ul>
<li><strong>Istio managed add-on GA</strong>: The Istio managed add-on is generally available for Kubernetes version 1.16 clusters. In Kubernetes version 1.16 clusters, you can install the Istio add-on or [update your existing beta add-on to the latest version](/docs/containers?topic=containers-istio#istio_update).</li>
<li><strong>Fluentd component changes</strong>: The Fluentd component is created for your cluster only if you [create a logging configuration to forward logs to a syslog server](/docs/containers?topic=containers-health#configuring). If no logging configurations for syslog exist in your cluster, the Fluentd component is removed automatically. If you do not forward logs to syslog and want to ensure that the Fluentd component is removed from your cluster, [automatic updates to Fluentd must be enabled](/docs/containers?topic=containers-update#logging-up).</li>
<li><strong>Bringing your own Ingress controller in VPC clusters</strong>: Added [steps](/docs/containers?topic=containers-ingress-user_managed#user_managed_vpc) for exposing your Ingress controller by creating a VPC load balancer and subdomain.</li>
</ul></td>
</tr>
<tr>
<td>14 November 2019</td>
<td><strong>New! Diagnostics and Debug Tool add-on</strong>: The [{{site.data.keyword.containerlong_notm}} Diagnostics and Debug Tool](/docs/containers?topic=containers-cs_troubleshoot#debug_utility) is now available as a cluster add-on.</td>
</tr>
<tr>
<td>11 November 2019</td>
<td><ul>
<li><strong>Accessing your cluster</strong>: Added an [Accessing Kubernetes clusters page](/docs/containers?topic=containers-access_cluster).</li>
<li><strong>Exposing apps that are external to your cluster by using Ingress</strong>: Added information for how to use the [`proxy-external-service` Ingress annotation](/docs/containers?topic=containers-ingress#proxy-external) to include an app that is external to your cluster in Ingress application load balancing.</li>
<li><strong>Version changelog</strong>: Worker node patch updates are available for Kubernetes [1.16.2_1515](/docs/containers?topic=containers-changelog_archive#1162_1515_worker), [1.15.5_1522](/docs/containers?topic=containers-changelog_archive#1155_1522_worker), [1.14.8_1538](/docs/containers?topic=containers-changelog_archive#1148_1538_worker), and [1.13.12_1541](/docs/containers?topic=containers-changelog_archive#11312_1541_worker).</li>
</ul></td>
</tr>
<tr>
<td>07 November 2019</td>
<td><ul><li><strong>CLI changelog</strong>: Updated the {{site.data.keyword.containerlong_notm}} CLI plug-in changelog page for the [release of version 0.4.51](/docs/containers?topic=containers-cs_cli_changelog).</li>
<li><strong>Ingress ALB changelog</strong>: Updated the [ALB `ingress-auth` image to build 345](/docs/containers?topic=containers-cluster-add-ons-changelog#alb_changelog).</li></ul></td>
</tr>
<tr>
<td>05 November 2019</td>
<td><strong>New! Adding classic infrastructure servers to gateway-enabled classic clusters (Beta)</strong>: If you have non-containerized workloads on a classic IBM Cloud infrastructure virtual server or bare metal server, you can connect those workloads to the workloads in your gateway-enabled classic cluster by [adding the server instance to your cluster network](/docs/containers?topic=containers-add_workers#gateway_vsi).</td>
</tr>
<tr>
<td>04 November 2019</td>
<td><ul><li><strong>New! Kubernetes version 1.16</strong>: You can now create clusters that run Kubernetes version 1.16. To update an existing cluster, see the [Version 1.16 preparation actions](/docs/containers?topic=containers-cs_versions#cs_v116).</li>
<li><strong>Deprecated: Kubernetes version 1.13</strong>: With the release of version 1.16, clusters that run version 1.13 are deprecated. Consider [updating to 1.14](/docs/containers?topic=containers-cs_versions#cs_v114) today.</li>
<li><strong>Unsupported: Kubernetes version 1.12</strong>: With the release of version 1.16, clusters that run version 1.12 are unsupported. To continue receiving important security updates and support, you must [update the cluster to a supported version](/docs/containers?topic=containers-cs_versions#prep-up) immediately.</li></ul></td>
</tr>
<tr>
<td>01 November 2019</td>
<td><strong>New! Keep your own key (KYOK) support (beta)</strong>: You can now [enable several key management service (KMS) providers](/docs/containers?topic=containers-encryption#kms), so that you can use your own root key to encrypt the secrets in your cluster.</td>
</tr>
</tbody>
</table>

## October 2019
{: #oct19}

<table summary="The table shows release notes. Rows are to be read from the left to right, with the date in column one, the title of the feature in column two and a description in column three.">
<caption>Documentation updates in October 2019</caption>
<thead>
<th>Date</th>
<th>Description</th>
</thead>
<tbody>
<tr>
<td>28 October 2019</td>
<td><ul>
<li><strong>New! Resource groups for VPC clusters</strong>: You can now [create Kubernetes clusters](/docs/containers?topic=containers-vpc_ks_tutorial) in different resource groups than the resource group of the Virtual Private Cloud (VPC).</li>
<li><strong>Version changelogs</strong>: Worker node patch updates are available for Kubernetes [1.15.5_1521](/docs/containers?topic=containers-changelog_archive#1155_1521), [1.14.8_1537](/docs/containers?topic=containers-changelog_archive#1148_1537), [1.13.12_1540](/docs/containers?topic=containers-changelog_archive#11312_1540), [1.12.10_1570](/docs/containers?topic=containers-changelog_archive#11210_1570), and {{site.data.keyword.openshiftshort}} [3.11.153_1529_openshift](/docs/openshift?topic=openshift-openshift_changelog#311153_1529).</li></ul></td>
</tr><tr>
<td>24 October 2019</td>
  <td><ul>
    <li><strong>CLI changelog</strong>: Updated the {{site.data.keyword.containerlong_notm}} CLI plug-in changelog page for the [release of version 0.4.42](/docs/containers?topic=containers-cs_cli_changelog).</li>
    <li><strong>Scaling down file storage</strong>: Added steps for [scaling down the default file storage plug-in](/docs/containers?topic=containers-file_storage#file_scaledown_plugin) in classic clusters.</li>
    <li><strong>Ingress subdomain troubleshooting</strong>: Added troubleshooting steps for when [no Ingress subdomain exists after cluster creation](/docs/containers?topic=containers-cs_troubleshoot_debug_ingress#ingress_subdomain)</li>
  </ul></td>
</tr>
<tr>
<td>23 October 2019</td>
  <td><ul>
    <li><strong>Ingress ALB changelog</strong>: Updated the ALB [`nginx-ingress` image to build 584 and `ingress-auth` image build to 344](/docs/containers?topic=containers-cluster-add-ons-changelog#alb_changelog).</li>
    <li><strong>Ingress annotations</strong>: Added the [`proxy-ssl-verify-depth` and `proxy-ssl-name` optional parameters to the `ssl-services` annotation](/docs/containers?topic=containers-ingress_annotation#ssl-services).</li></ul></td>
</tr>
<tr>
  <td>22 October 2019</td>
  <td><strong>Version changelogs</strong>: Master patch updates are available for Kubernetes [1.15.5_1520](/docs/containers?topic=containers-changelog_archive#1155_1520), [1.14.8_1536](/docs/containers?topic=containers-changelog_archive#1148_1536), [1.13.12_1539](/docs/containers?topic=containers-changelog_archive#11312_1539), and {{site.data.keyword.openshiftshort}} [3.11.146_1528_openshift](/docs/openshift?topic=openshift-openshift_changelog#311146_1528).</td>
</tr>
<tr>
  <td>17 October 2019</td>
  <td><ul><li><strong>New! Cluster autoscaler</strong>: The cluster autoscaler is available for private network-only clusters. To get started, update to the latest Helm chart version.</li>
  <li><strong>New DevOps tutorial</strong>: Learn how to set up your own DevOps toolchain and configure continuous delivery pipeline stages to deploy your containerized app that is stored in GitHub to a cluster in {{site.data.keyword.containerlong_notm}}.</li></ul></td>
</tr>
<tr>
  <td>14 October 2019</td>
  <td><ul>
  <li><strong>Calico MTU</strong>: Added [steps](/docs/containers?topic=containers-kernel#calico-mtu) for changing the Calico plug-in maximum transmission unit (MTU) to meet the network throughput requirements of your environment.</li>
  <li><strong>CLI changelog</strong>: Updated the {{site.data.keyword.containerlong_notm}} CLI plug-in changelog page for the [release of version 0.4.38](/docs/containers?topic=containers-cs_cli_changelog).</li>
  </li>
  <li><strong>Creating DNS subdomains for VPC load balancers and private Ingress ALBs</strong>: Added steps for [registering a VPC load balancer hostname with a DNS subdomain](/docs/containers?topic=containers-vpc-lbaas#vpc_lb_dns) and for [exposing apps to a private network](/docs/containers?topic=containers-ingress#ingress_expose_vpc_private) in VPC clusters.</li>
  <li><strong>Let's Encrypt rate limits for Ingress</strong>: Added [troubleshooting steps] for when no subdomain or secret is generated for the Ingress ALB when you create or delete clusters of the same name.</li>
  <li><strong>Version changelogs</strong>: Worker node patch updates are available for Kubernetes [1.15.4_1519](/docs/containers?topic=containers-changelog_archive#1154_1519_worker), [1.14.7_1535](/docs/containers?topic=containers-changelog_archive#1147_1535_worker), [1.13.11_1538](/docs/containers?topic=containers-changelog_archive#11311_1538_worker), [1.12.10_1569](/docs/containers?topic=containers-changelog_archive#11210_1569_worker), and {{site.data.keyword.openshiftshort}} [3.11.146_1527_openshift](/docs/openshift?topic=openshift-openshift_changelog#311146_1527).</li>
  </ul></td>
</tr>
<tr>
  <td>03 October 2019</td>
  <td><ul>
  <li><strong>Ingress ALB changelog</strong>: Updated the ALB [`nginx-ingress` image to build 579 and `ingress-auth` image build to 341](/docs/containers?topic=containers-cluster-add-ons-changelog#alb_changelog).</li>
  <li><strong>DevOps toolchain</strong>: You can now use the **DevOps** tab on the cluster details page to configure your DevOps toolchain. For more information, see [Setting up a continuous delivery pipeline for a cluster](/docs/containers?topic=containers-cicd#continuous-delivery-pipeline).</li>
  <li><strong>Security for VPC clusters</strong>: Added information for how to achieve [network segmentation and privacy in VPC clusters](/docs/containers?topic=containers-security#network_segmentation_vpc).</li>
  </ul>
  </td>
</tr>
<tr>
  <td>01 October 2019</td>
  <td><ul>
    <li><strong>End of service of {{site.data.keyword.la_full_notm}} and {{site.data.keyword.monitoringlong_notm}}</strong>: Removed steps for using {{site.data.keyword.la_full_notm}} and {{site.data.keyword.monitoringlong_notm}} to work with cluster logs and metrics. You can collect logs and metrics for your cluster by setting up [{{site.data.keyword.la_full_notm}}](/docs/containers?topic=containers-health#logging) and [{{site.data.keyword.mon_full_notm}}](/docs/monitoring?topic=monitoring-kubernetes_cluster#kubernetes_cluster) instead.</li>
    <li><strong>New! Gateway-enabled classic clusters</strong>: Keep your compute workloads private and allow limited public connectivity to your classic cluster by enabling a gateway. You can enable a gateway only on standard, Kubernetes clusters during cluster creation.<br><br>
    When you enable a gateway on a classic cluster, the cluster is created with a `compute` worker pool of compute worker nodes that are connected to a private VLAN only, and a `gateway` worker pool of gateway worker nodes that are connected to public and private VLANs. Traffic into or out of the cluster is routed through the gateway worker nodes, which provide your cluster with limited public access. For more information, check out the following links:<ul>
      <li>[Using a gateway-enabled cluster](/docs/containers?topic=containers-plan_clusters#gateway)</li>
      <li>[Isolating networking workloads to edge nodes in classic gateway-enabled clusters](/docs/containers?topic=containers-edge#edge_gateway)</li>
      <li>Flow of traffic to apps when using an [NLB 1.0](/docs/containers?topic=containers-loadbalancer-about#v1_gateway), an [NLB 2.0](/docs/containers?topic=containers-loadbalancer-about#v2_gateway), or [Ingress ALBs](/docs/containers?topic=containers-ingress-about#classic-gateway)</li></ul>
    Ready to get started? [Create a standard classic cluster with a gateway in the CLI.](/docs/containers?topic=containers-clusters#gateway_cluster_cli)</li>
    <li><strong>Version changelogs</strong>: Patch updates are available for Kubernetes [1.15.4_1518](/docs/containers?topic=containers-changelog_archive#1154_1518), [1.14.7_1534](/docs/containers?topic=containers-changelog_archive#1147_1534), [1.13.11_1537](/docs/containers?topic=containers-changelog_archive#11311_1537), and [1.12.10_1568](/docs/containers?topic=containers-changelog_archive#11210_1568_worker).</li>
  </ul>
  </td>
</tr>
</tbody></table>

<br />

## September 2019
{: #sept19}

<table summary="The table shows release notes. Rows are to be read from the left to right, with the date in column one, the title of the feature in column two and a description in column three.">
<caption>Documentation updates in September 2019</caption>
<thead>
<th>Date</th>
<th>Description</th>
</thead>
<tbody>
<tr>
  <td>27 September 2019</td>
  <td><strong>{{site.data.keyword.cos_full_notm}} supported in VPC clusters</strong>: You can now provision {{site.data.keyword.cos_full_notm}} for your apps that run in a VPC cluster. For more information, see [Storing data on {{site.data.keyword.cos_full_notm}}](/docs/containers?topic=containers-object_storage).</td>
</tr>
<tr>
  <td>24 September 2019</td>
  <td><ul>
  <li><strong>CLI changelog</strong>: Updated the {{site.data.keyword.containerlong_notm}} CLI plug-in changelog page for the [release of version 0.4.31](/docs/containers?topic=containers-cs_cli_changelog).</li>
  <li><strong>Ingress ALB changelog</strong>: Updated the ALB [`nginx-ingress` image to build 566](/docs/containers?topic=containers-cluster-add-ons-changelog#alb_changelog).</li>
  <li><strong>Managing cluster network traffic for VPC clusters</strong>: Includes the following new content:<ul>
    <li>[Opening required ports and IP addresses in your firewall for VPC clusters](/docs/containers?topic=containers-vpc-firewall)</li>
    <li>[Controlling traffic with VPC ACLs and network policies](/docs/containers?topic=containers-vpc-network-policy)</li>
    <li>[Setting up VPC VPN connectivity](/docs/containers?topic=containers-vpc-vpnaas)</li></ul></li>
  <li><strong>Customizing PVC settings for VPC Block Storage</strong>: You can create a customized storage class or use a Kubernetes secret to create VPC Block Storage with the configuration that you want. For more information, see [Customizing the default settings](/docs/containers?topic=containers-vpc-block#vpc-customize-default).</li>
  </ul></td>
</tr>
<tr>
  <td>19 September 2019</td>
  <td><strong>Sending custom Ingress certificates to legacy clients</strong>: Added [steps](/docs/containers?topic=containers-ingress_annotation#default_server_cert) for ensuring that your custom certificate, instead of the default IBM-provided certificate, is sent by the Ingress ALB to legacy clients that do not support SNI.</td>
</tr>
<tr>
  <td>16 September 2019</td>
  <td><ul><li><strong>CLI changelog</strong>: Updated the {{site.data.keyword.containerlong_notm}} CLI plug-in changelog page for the [release of version 0.4.23](/docs/containers?topic=containers-cs_cli_changelog).</li>
  <li><strong>{{site.data.keyword.at_full_notm}} events</strong>: Added information about [which {{site.data.keyword.at_short}} location your events are sent to](/docs/containers?topic=containers-at_events#at-ui) based on the {{site.data.keyword.containerlong_notm}} location where the cluster is located.</li>
  <li><strong>Version changelogs</strong>: Worker node patch updates are available for Kubernetes [1.15.3_1517](/docs/containers?topic=containers-changelog_archive#1153_1517_worker), [1.14.6_1533](/docs/containers?topic=containers-changelog_archive#1146_1533_worker), [1.13.10_1536](/docs/containers?topic=containers-changelog_archive#11310_1536_worker), and [1.12.10_1567](/docs/containers?topic=containers-changelog_archive#11210_1567_worker).</li></ul>
  </td>
</tr>
<tr>
  <td>13 September 2019</td>
  <td><ul>
    <li><strong>Entitled software</strong>: If you have licensed products from your [MyIBM.com ![External link icon](../icons/launch-glyph.svg "External link icon")](https://myibm.ibm.com) container software library, you can [set up your cluster to pull images from the entitled registry](/docs/containers?topic=containers-registry#secret_entitled_software).</li>
  <li><strong>`script update` command</strong>: Added [steps for using the `script update` command](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#script_update) to prepare your automation scripts for the release of version 1.0 of the {{site.data.keyword.containerlong_notm}} plug-in.</li>
  </ul></td>
</tr>
<tr>
  <td>12 September 2019</td>
  <td><strong>Ingress ALB changelog</strong>: Updated the ALB [`nginx-ingress` image to build 552](/docs/containers?topic=containers-cluster-add-ons-changelog#alb_changelog).</td>
</tr>
<tr>
  <td>05 September 2019</td>
  <td><strong>Ingress ALB changelog</strong>: Updated the ALB [`ingress-auth` image to build 340](/docs/containers?topic=containers-cluster-add-ons-changelog#alb_changelog).</td>
</tr>
<tr>
  <td>04 September 2019</td>
  <td><ul><li><strong>CLI changelog</strong>: Updated the {{site.data.keyword.containerlong_notm}} CLI plug-in changelog page for the [release of version 0.4.3](/docs/containers?topic=containers-cs_cli_changelog).</li>
  <li><strong>IAM allowlists</strong>: If you use an IAM allowlist, you must [allow the CIDRs of the {{site.data.keyword.containerlong_notm}} control plane](/docs/containers?topic=containers-firewall#iam_allowlist) for the zones in the region where your cluster is located so that {{site.data.keyword.containerlong_notm}} can create Ingress ALBs and `LoadBalancers` in your cluster.</li></ul></td>
</tr>
<tr>
  <td>03 September 2019</td>
  <td><ul><li><strong>New! {{site.data.keyword.containerlong_notm}} plug-in version `0.4`</strong>: Updated the {{site.data.keyword.containerlong_notm}} CLI plug-in changelog page for multiple changes in the [release of version 0.4.1](/docs/containers?topic=containers-cs_cli_changelog).</li>
  <li><strong>Version changelog</strong>: Worker node patch updates are available for Kubernetes [1.15.3_1516](/docs/containers?topic=containers-changelog_archive#1153_1516_worker), [1.14.6_1532](/docs/containers?topic=containers-changelog_archive#1146_1532_worker), [1.13.10_1535](/docs/containers?topic=containers-changelog_archive#11310_1535_worker), [1.12.10_1566](/docs/containers?topic=containers-changelog_archive#11210_1566_worker), and {{site.data.keyword.openshiftshort}} [3.11.135_1523](/docs/openshift?topic=openshift-openshift_changelog#311135_1523_worker).</li></ul></td>
</tr>
</tbody></table>

<br />

## August 2019
{: #aug19}

<table summary="The table shows release notes. Rows are to be read from the left to right, with the date in column one, the title of the feature in column two and a description in column three.">
<caption>Documentation updates in August 2019</caption>
<thead>
<th>Date</th>
<th>Description</th>
</thead>
<tbody>
<tr>
  <td>29 August 2019</td>
  <td><strong>Forwarding Kubernetes API audit logs to {{site.data.keyword.la_full_notm}}</strong>: Added steps to [create an audit webhook in your cluster](/docs/containers?topic=containers-health-audit) to collect Kubernetes API audit logs from your cluster and forward them to {{site.data.keyword.la_full_notm}}.</td>
</tr>
<tr>
  <td>28 August 2019</td>
  <td><ul>
  <li><strong>CLI changelog</strong>: Updated the {{site.data.keyword.containerlong_notm}} CLI plug-in changelog page for the [release of version 0.3.112](/docs/containers?topic=containers-cs_cli_changelog).</li>
  <li><strong>Version changelogs</strong>: Updated the changelogs for [1.15.3_1515](/docs/containers?topic=containers-changelog_archive#1153_1515), [1.14.6_1531](/docs/containers?topic=containers-changelog_archive#1146_1531), [1.13.10_1534](/docs/containers?topic=containers-changelog_archive#11310_1534), and [1.12.10_1565](/docs/containers?topic=containers-changelog_archive#11210_1565) master fix pack updates.</li>
  </ul></td>
</tr>
<tr>
  <td>26 August 2019</td>
  <td><ul>
  <li><strong>Cluster autoscaler</strong>: With the latest version of the cluster autoscaler, you can [enable autoscaling for worker pools during the Helm chart installation](/docs/containers?topic=containers-ca#ca_helm) instead of modifying the config map after installation.</li>
  <li><strong>Ingress ALB changelog</strong>: Updated the ALB [`nginx-ingress` image to build 524 and `ingress-auth` image to build 337](/docs/containers?topic=containers-cluster-add-ons-changelog#alb_changelog).</li></ul></td>
</tr>
<tr>
  <td>23 August 2019</td>
  <td><ul>
  <li><strong>App networking in VPC</strong>: Updated the [Planning in-cluster and external networking for apps](/docs/containers?topic=containers-cs_network_planning) topic with information for planning app networking in a VPC cluster.</li>
  <li><strong>Istio in VPC</strong>: Updated the [managed Istio add-on](/docs/containers?topic=containers-istio) topic with information for using Istio in a VPC cluster.</li>
  <li><strong>Remove bound services from cluster</strong>: Added instructions for how to remove an {{site.data.keyword.cloud_notm}} service that you added to a cluster by using service binding. For more information, see [Removing a service from a cluster](/docs/containers?topic=containers-service-binding#unbind-service).</li></ul></td>
</tr>
<tr>
  <td>20 August 2019</td>
  <td><strong>Ingress ALB changelog</strong>: Updated the ALB [`nginx-ingress` image to build 519](/docs/containers?topic=containers-cluster-add-ons-changelog#alb_changelog) for a `custom-ports` bug fix.</td>
</tr>
<tr>
  <td>19 August 2019</td>
  <td><ul>
  <li><strong>New! Virtual Private Cloud</strong>: You can create standard Kubernetes clusters on classic infrastructure in the next generation of the {{site.data.keyword.cloud_notm}} platform, in your [Virtual Private Cloud](/docs/vpc-on-classic?topic=vpc-on-classic-about). VPC gives you the security of a private cloud environment with the dynamic scalability of a public cloud. Classic on VPC clusters are available for only standard, Kubernetes clusters and are not supported in free or {{site.data.keyword.openshiftshort}} clusters.<br><br>
  With classic clusters in VPC, {{site.data.keyword.containerlong_notm}} introduces version 2 of the API, which supports multiple infrastructure providers for your clusters. Your cluster network setup also changes, from worker nodes that use public and private VLANs and the public cloud service endpoint to worker nodes that are on a private subnet only and have the private cloud service endpoint enabled. For more information, check out the following links:<ul>
    <li>[Overview of Classic and VPC infrastructure providers](/docs/containers?topic=containers-infrastructure_providers)</li>
    <li>[About the v2 API](/docs/containers?topic=containers-cs_api_install#api_about)</li>
    <li>[Understanding network basics of VPC clusters](/docs/containers?topic=containers-plan_clusters#plan_vpc_basics)</li></ul>
  Ready to get started? Try out the [Creating a classic cluster in your VPC tutorial](/docs/containers?topic=containers-vpc_ks_tutorial).</li>
  <li><strong>Kubernetes 1.14</strong>: [Kubernetes 1.14](/docs/containers?topic=containers-cs_versions#version_types) is now the default version.</li>
  </ul>
  </td>
</tr>
<tr>
  <td>17 August 2019</td>
  <td><ul>
  <li><strong>{{site.data.keyword.at_full}}</strong>: The [{{site.data.keyword.at_full_notm}} service](/docs/containers?topic=containers-at_events) is now supported for you to view, manage, and audit user-initiated activities in your clusters.</li>
  <li><strong>Version changelogs</strong>: Updated the changelogs for [1.15.2_1514](/docs/containers?topic=containers-changelog_archive#1152_1514), [1.14.5_1530](/docs/containers?topic=containers-changelog_archive#1145_1530), [1.13.9_1533](/docs/containers?topic=containers-changelog_archive#1139_1533), and [1.12.10_1564](/docs/containers?topic=containers-changelog_archive#11210_1564) master fix pack updates.</li></ul></td>
</tr>
<tr>
  <td>15 August 2019</td>
  <td><ul>
  <li><strong>App deployments</strong>: Added steps for [copying deployments from one cluster to another](/docs/containers?topic=containers-update_app#copy_apps_cluster).</li>
  <li><strong>FAQs</strong>: Added an FAQ about [free clusters](/docs/containers?topic=containers-faqs#faq_free).</li>
  <li><strong>Istio</strong>: Added steps for [exposing Istio-managed apps with TLS termination](/docs/containers?topic=containers-istio-mesh#tls), [securing in-cluster traffic by enabling mTLS](/docs/containers?topic=containers-istio-mesh#mtls), and [Updating the Istio add-ons](/docs/containers?topic=containers-istio#istio_update).</li>
  <li><strong>Knative</strong>: Added instructions for how to use volumes to access secrets and config maps, pull images from a private registry, scale apps based on CPU usage, [change the default container port, and change the `scale-to-zero-grace-period`.</li>
  <li><strong>Version changelogs</strong>: Updated the changelogs for [1.15.2_1513](/docs/containers?topic=containers-changelog_archive#1152_1513), [1.14.5_1529](/docs/containers?topic=containers-changelog_archive#1145_1529), [1.13.9_1532](/docs/containers?topic=containers-changelog_archive#1139_1532), and [1.12.10_1563](/docs/containers?topic=containers-changelog_archive#11210_1563) master fix pack updates.</li></ul></td>
</tr>
<tr>
  <td>12 August 2019</td>
  <td><ul>
  <li><strong>CLI changelog</strong>: Updated the {{site.data.keyword.containerlong_notm}} CLI plug-in changelog page for the [release of version 0.3.103](/docs/containers?topic=containers-cs_cli_changelog).</li>
  <li><strong>Ingress ALB changelog</strong>: Updated the ALB `ingress-auth` image to build 335 for [`musl libc` vulnerabilities](/docs/containers?topic=containers-cluster-add-ons-changelog#alb_changelog).</li></ul></td>
</tr>
<tr>
  <td>05 August 2019</td>
  <td><ul>
  <li><strong>CLI changelog</strong>: Updated the {{site.data.keyword.containerlong_notm}} CLI plug-in changelog page for the [release of version 0.3.99](/docs/containers?topic=containers-cs_cli_changelog).</li>
  <li><strong>New! `NodeLocal` DNS caching (beta)</strong>: For clusters that run Kubernetes 1.15 or later, you can set up improved cluster DNS performance with [`NodeLocal` DNS caching](/docs/containers?topic=containers-cluster_dns#dns_cache).</li>
  <li><strong>New! Version 1.15</strong>: You can create community Kubernetes clusters that run version 1.15. To update from a previous version, review the [1.15 changes](/docs/containers?topic=containers-cs_versions#cs_v115).</li>
  <li><strong>Deprecated: Version 1.12</strong>: Kubernetes version 1.12 is deprecated. Review the [changes across versions](/docs/containers?topic=containers-cs_versions), and update to a more recent version.</li>
  <li><strong>Version changelogs</strong>: Updated the changelogs for [1.14.4_1527](/docs/containers?topic=containers-changelog_archive#1144_1527_worker), [1.13.8_1530](/docs/containers?topic=containers-changelog_archive#1138_1530_worker), and [1.12.10_1561](/docs/containers?topic=containers-changelog_archive#11210_1561_worker) worker node patch updates.</li></ul></td>
</tr>
</tbody></table>

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
  <td>30 July 2019</td>
  <td><ul>
  <li><strong>CLI changelog</strong>: Updated the {{site.data.keyword.containerlong_notm}} CLI plug-in changelog page for the [release of version 0.3.95](/docs/containers?topic=containers-cs_cli_changelog).</li>
  <li><strong>Ingress ALB changelog</strong>: Updated the ALB `nginx-ingress` image to build 515 for the [ALB pod readiness check](/docs/containers?topic=containers-cluster-add-ons-changelog#alb_changelog).</li>
  <li><strong>Removing subnets from a cluster</strong>: Added steps for removing subnets [in an IBM Cloud infrastructure account](/docs/containers?topic=containers-subnets#remove-subnets) from a cluster. </li>
  </ul></td>
</tr>
<tr>
  <td>23 July 2019</td>
  <td><strong>Fluentd changelog</strong>: Fixes [Alpine vulnerabilities](/docs/containers?topic=containers-cluster-add-ons-changelog#fluentd_changelog).</td>
</tr>
<tr>
  <td>22 July 2019</td>
  <td><ul>
    <li><strong>Version policy</strong>: Increased the [version deprecation](/docs/containers?topic=containers-cs_versions#version_types) period from 30 to 45 days.</li>
    <li><strong>Version changelogs</strong>: Updated the changelogs for [1.14.4_1526](/docs/containers?topic=containers-changelog_archive#1144_1526_worker), [1.13.8_1529](/docs/containers?topic=containers-changelog_archive#1138_1529_worker), and [1.12.10_1560](/docs/containers?topic=containers-changelog_archive#11210_1560_worker) worker node patch updates.</li>
    <li><strong>Version changelog</strong>: [Version 1.11](/docs/containers?topic=containers-changelog_archive#111_changelog) is unsupported.</li></ul>
  </td>
</tr>
<tr>
  <td>17 July 2019</td>
  <td><strong>Ingress ALB changelog</strong>: [Fixes `rbash` vulnerabilities](/docs/containers?topic=containers-cluster-add-ons-changelog#alb_changelog).
  </td>
</tr>
<tr>
  <td>15 July 2019</td>
  <td><ul>
  <li><strong>Cluster and worker node ID</strong>: The ID format for clusters and worker nodes is changed. Existing clusters and worker nodes keep their existing IDs. If you have automation that relies on the previous format, update it for new clusters.<ul>
  <li>**Cluster ID**: In the regex format `{a-v0-9}[7]{a-z0-9}[2]{a-v0-9}[11]`</li>
  <li>**Worker node ID**: In the format `kube-<cluster_ID>-<cluster_name_truncated>-<resource_group_truncated>-<worker_ID>`</li></ul></li>
  <li><strong>Ingress ALB changelog</strong>: Updated the [ALB `nginx-ingress` image to build 497](/docs/containers?topic=containers-cluster-add-ons-changelog#alb_changelog).</li>
  <li><strong>Troubleshooting clusters</strong>: Added [troubleshooting steps](/docs/containers?topic=containers-cs_troubleshoot_clusters#cs_totp) for when you cannot manage clusters and worker nodes because the time-based one-time passcode (TOTP) option is enabled for your account.</li>
  <li><strong>Version changelogs</strong>: Updated the changelogs for [1.14.4_1526](/docs/containers?topic=containers-changelog_archive#1144_1526), [1.13.8_1529](/docs/containers?topic=containers-changelog_archive#1138_1529), and [1.12.10_1560](/docs/containers?topic=containers-changelog_archive#11210_1560) master fix pack updates.</li></ul>
  </td>
</tr>
<tr>
  <td>08 July 2019</td>
  <td><strong>Version changelogs</strong>: Updated the changelogs for [1.14.3_1525](/docs/containers?topic=containers-changelog_archive#1143_1525), [1.13.7_1528](/docs/containers?topic=containers-changelog_archive#1137_1528), [1.12.9_1559](/docs/containers?topic=containers-changelog_archive#1129_1559), and [1.11.10_1564](/docs/containers?topic=containers-changelog_archive#11110_1564) worker node patch updates.</td>
</tr>
<tr>
  <td>02 July 2019</td>
  <td><strong>CLI changelog</strong>: Updated the {{site.data.keyword.containerlong_notm}} CLI plug-in changelog page for the [release of version 0.3.58](/docs/containers?topic=containers-cs_cli_changelog).</td>
</tr>
<tr>
  <td>01 July 2019</td>
  <td><ul>
  <li><strong>Infrastructure permissions</strong>: Updated the [classic infrastructure roles](/docs/containers?topic=containers-access_reference#infra) required for common use cases.</li>
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
  <li><strong>Service limitations</strong>: Updated the [maximum number of pods per worker node limitation](/docs/containers?topic=containers-limitations#tech_limits). For worker nodes that run Kubernetes 1.13.7_1527, 1.14.3_1524, or later and that have more than 11 CPU cores, the worker nodes can support 10 pods per core, up to a limit of 250 pods per worker node.</li>
  <li><strong>Version changelogs</strong>: Added changelogs for [1.14.3_1524](/docs/containers?topic=containers-changelog_archive#1143_1524), [1.13.7_1527](/docs/containers?topic=containers-changelog_archive#1137_1527), [1.12.9_1558](/docs/containers?topic=containers-changelog_archive#1129_1558), and [1.11.10_1563](/docs/containers?topic=containers-changelog_archive#11110_1563) worker node patch updates.</li>
  </ul></td>
</tr>
<tr>
  <td>21 June 2019</td>
  <td>
  <strong>Accessing the Kubernetes master through the private cloud service endpoint</strong>: Added steps for [editing the local Kubernetes configuration file](/docs/containers?topic=containers-access_cluster#access_private_se) when both the public and private cloud service endpoints are enabled, but you want to access the Kubernetes master through the private cloud service endpoint only.
  </td>
</tr>
<tr>
  <td>18 June 2019</td>
  <td><ul>
  <li><strong>CLI changelog</strong>: Updated the {{site.data.keyword.containerlong_notm}} CLI plug-in changelog page for the [release of versions 0.3.47 and 0.3.49](/docs/containers?topic=containers-cs_cli_changelog).</li>
  <li><strong>Ingress ALB changelog</strong>: Updated the [ALB `nginx-ingress` image to build 473 and `ingress-auth` image to build 331](/docs/containers?topic=containers-cluster-add-ons-changelog#alb_changelog).</li>
  <li><strong>Managed add-on versions</strong>: Updated the version of the Istio managed add-on to 1.1.7 and the Knative managed add-on to 0.6.0.</li>
  <li><strong>Removing persistent storage</strong>: Updated the information for how you are billed when you delete persistent storage.</li>
  <li><strong>Service bindings with private endpoint</strong>: [Added steps](/docs/containers?topic=containers-service-binding) for how to manually create service credentials with the private cloud service endpoint when you bind the service to your cluster.</li>
  <li><strong>Version changelogs</strong>: Updated the changelogs for [1.14.3_1523](/docs/containers?topic=containers-changelog_archive#1143_1523), [1.13.7_1526](/docs/containers?topic=containers-changelog_archive#1137_1526), [1.12.9_1557](/docs/containers?topic=containers-changelog_archive#1129_1557), and [1.11.10_1562](/docs/containers?topic=containers-changelog_archive#11110_1562) patch updates.</li>
  </ul></td>
</tr>
<tr>
  <td>14 June 2019</td>
  <td><ul>
  <li><strong>`kubectl` troubleshooting</strong>: Added a [troubleshooting topic](/docs/containers?topic=containers-cs_troubleshoot#kubectl_fails) for when you have a `kubectl` client version that is 2 or more versions apart from the server version or the {{site.data.keyword.openshiftshort}} version of `kubectl`, which does not work with community Kubernetes clusters.</li>
  <li><strong>Tutorials landing page</strong>: Replaced the related links page with a new tutorials landing page for all tutorials that are specific to {{site.data.keyword.containershort_notm}}.</li>
  <li><strong>Tutorial to create a cluster and deploy an app</strong>: Combined the tutorials for creating clusters and deploying apps into one comprehensive [tutorial](/docs/containers?topic=containers-cs_cluster_tutorial).</li>
  <li><strong>Using existing subnets to create a cluster</strong>: To [reuse subnets from an unneeded cluster when you create a new cluster](/docs/containers?topic=containers-subnets#subnets_custom), the subnets must be user-managed subnets that you manually added from an on-premises network. All subnets that were automatically ordered during cluster creation are immediately marked for deletion after you delete a cluster, and you cannot reuse these subnets to create a new cluster.</li>
  </ul></td>
</tr>
<tr>
  <td>12 June 2019</td>
  <td><strong>Aggregating cluster roles</strong>: Added steps for [extending users' existing permissions by aggregating cluster roles](/docs/containers?topic=containers-users#rbac_aggregate).</td>
</tr>
<tr>
  <td>07 June 2019</td>
  <td><ul>
  <li><strong>Access to the Kubernetes master through the private cloud service endpoint</strong>: Added [steps](/docs/containers?topic=containers-access_cluster#access_private_se) to expose the private cloud service endpoint through a private load balancer. After you complete these steps, your authorized cluster users can access the Kubernetes master from a VPN or {{site.data.keyword.dl_full_notm}} connection.</li>
  <li><strong>{{site.data.keyword.BluDirectLink}}</strong>: Added {{site.data.keyword.dl_full_notm}} to the [VPN connectivity](/docs/containers?topic=containers-vpn) and [hybrid cloud](/docs/containers?topic=containers-hybrid_iks_icp) pages as a way to create a direct, private connection between your remote network environments and {{site.data.keyword.containerlong_notm}} without routing over the public internet.</li>
  <li><strong>Ingress ALB changelog</strong>: Updated the [ALB `ingress-auth` image to build 330](/docs/containers?topic=containers-cluster-add-ons-changelog#alb_changelog).</li>
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
  <td><strong>CLI reference</strong>: Updated the [CLI reference page](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli) to reflect multiple changes for the [release of version 0.3.34](/docs/containers?topic=containers-cs_cli_changelog) of the {{site.data.keyword.containerlong_notm}} CLI plug-in.</td>
</tr>
<tr>
  <td>04 June 2019</td>
  <td><strong>Version changelogs</strong>: Updated the changelogs for the [1.14.2_1521](/docs/containers?topic=containers-changelog_archive#1142_1521), [1.13.6_1524](/docs/containers?topic=containers-changelog_archive#1136_1524), [1.12.9_1555](/docs/containers?topic=containers-changelog_archive#1129_1555), and [1.11.10_1561](/docs/containers?topic=containers-changelog_archive#11110_1561) patch releases.
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
  <li><strong>Troubleshooting registry</strong>: Added a [troubleshooting topic](/docs/containers?topic=containers-cs_troubleshoot#ts_image_pull_create) for when your cluster cannot pull images from {{site.data.keyword.registryfull}} due to an error during cluster creation.
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
  <li><strong>Cluster and worker states</strong>: Updated the [Logging and monitoring page](/docs/containers?topic=containers-health-monitor#states) to add reference tables about cluster and worker node states.</li>
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
  <td><strong>Version changelogs</strong>: Added [worker node fix pack changelogs](/docs/containers?topic=containers-changelog).</td>
</tr>
<tr>
  <td>16 May 2019</td>
  <td><ul>
  <li><strong>CLI reference</strong>: Updated the [CLI reference page](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli) to add COS endpoints for `logging collect` commands and to clarify that `cluster master refresh` restarts the Kubernetes master components.</li>
  <li><strong>Unsupported: Kubernetes version 1.10</strong>: [Kubernetes version 1.10](/docs/containers?topic=containers-cs_versions#cs_v114) is now unsupported.</li>
  </ul></td>
</tr>
<tr>
  <td>15 May 2019</td>
  <td><ul>
  <li><strong>Default Kubernetes version</strong>: The default Kubernetes version is now 1.13.6.</li>
  <li><strong>Service limits</strong>: Added a [service limitations topic](/docs/containers?topic=containers-limitations#tech_limits).</li>
  </ul></td>
</tr>
<tr>
  <td>13 May 2019</td>
  <td><ul>
  <li><strong>Version changelogs</strong>: Added that new patch updates are available for [1.14.1_1518](/docs/containers?topic=containers-changelog_archive#1141_1518), [1.13.6_1521](/docs/containers?topic=containers-changelog_archive#1136_1521), [1.12.8_1552](/docs/containers?topic=containers-changelog_archive#1128_1552), [1.11.10_1558](/docs/containers?topic=containers-changelog_archive#11110_1558), and [1.10.13_1558](/docs/containers?topic=containers-changelog_archive#11013_1558).</li>
  <li><strong>Worker node flavors</strong>: Removed all [virtual machine worker node flavors](/docs/containers?topic=containers-planning_worker_nodes#vm) that are 48 or more cores per [cloud status ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/status?component=containers-kubernetes&selected=status). You can still provision [bare metal worker nodes](/docs/containers?topic=containers-planning_worker_nodes#bm) with 48 or more cores.</li></ul></td>
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


