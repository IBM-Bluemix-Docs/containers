---

copyright:
 years: 2014, 2021
lastupdated: "2021-04-12"

keywords: kubernetes, iks, versions, update, upgrade, BOM, bill of materials, versions, patch

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
 


# Kubernetes version changelog
{: #changelog}

View information of version changes for major, minor, and patch updates that are available for your {{site.data.keyword.containerlong}} Kubernetes clusters. Changes include updates to Kubernetes and {{site.data.keyword.cloud_notm}} Provider components.
{: shortdesc}

## Overview
{: #changelog_overview}

Unless otherwise noted in the changelogs, the {{site.data.keyword.containerlong_notm}} provider version enables Kubernetes APIs and features that are at beta. Kubernetes alpha features, which are subject to change, are disabled.

For more information about major, minor, and patch versions and preparation actions between minor versions, see [Kubernetes versions](/docs/containers?topic=containers-cs_versions).
{: tip}

Check the [Security Bulletins on {{site.data.keyword.cloud_notm}} Status](https://cloud.ibm.com/status?component=containers-kubernetes&selected=security) for security vulnerabilities that affect {{site.data.keyword.containerlong_notm}}. You can filter the results to view only Kubernetes Cluster security bulletins that are relevant to {{site.data.keyword.containerlong_notm}}. Changelog entries that address other security vulnerabilities but do not also refer to an IBM security bulletin are for vulnerabilities that are not known to affect {{site.data.keyword.containerlong_notm}} in normal usage. If you run privileged containers, run commands on the workers, or execute untrusted code, then you might be at risk.

Some changelogs are for _worker node fix packs_, and apply only to worker nodes. You must [apply these patches](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update) to ensure security compliance for your worker nodes. These worker node fix packs can be at a higher version than the master because some build fix packs are specific to worker nodes. Other changelogs are for _master fix packs_, and apply only to the cluster master. Master fix packs might not be automatically applied. You can choose to [apply them manually](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_update). For more information about patch types, see [Update types](/docs/containers?topic=containers-cs_versions#update_types).
{: note}

</br>

## Version 1.20 changelog
{: #120_changelog}

Review the version 1.20 changelog.
{: shortdesc}

### Changelog for worker node fix pack 1.20.5_1535, released 12 April 2021
{: #1205_1535}

The following table shows the changes that are included in the worker node fix pack `1.20.5_1535`. Worker node patch updates can be applied by updating, reloading (in classic infrastructure), or replacing (in VPC infrastructure) the worker node.
{: shortdesc}

| Component | Previous | Current | Description |
| --------- | -------- | ------- | ----------- |
| HA proxy | 9b2dca | a3b1ff | The update addresses [CVE-2021-3449](https://nvd.nist.gov/vuln/detail/CVE-2021-3449){: external} and [CVE-2021-3450](https://nvd.nist.gov/vuln/detail/CVE-2021-3450){: external}. |
| Ubuntu 18.04 packages | N/A | N/A | Updated worker node images with package updates for [CVE-2021-22876](https://nvd.nist.gov/vuln/detail/CVE-2021-22876){: external}. |
| Ubuntu 16.04 packages | N/A | N/A | Updated worker node images with package updates for [CVE-2021-22876](https://nvd.nist.gov/vuln/detail/CVE-2021-22876){: external}.|
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.20.5_1534" caption-side="top"}

### Changelog for master fix pack 1.20.5_1533, released 30 March 2021
{: #1205_1533}

The following table shows the changes that are included in the master fix pack patch update `1.20.5_1533`. Master patch updates are applied automatically.
{: shortdesc}

| Component | Previous | Current | Description |
| --- | --- | --- | --- |
| {{site.data.keyword.at_short}} event | N/A | N/A | Now, the `containers-kubernetes.version.update` event is sent to {{site.data.keyword.at_short}} when a master fix pack update is initiated for a cluster. |
| Cluster health image | v1.2.8 | v1.2.9 | Updated image for [CVE-2020-28851](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-28851){: external}. |
| CoreDNS | 1.8.0 | 1.8.3 | See the [CoreDNS release notes](https://coredns.io/tags/notes/){: external}. |
| Gateway-enabled cluster controller | 1232 | 1322 | Updated to use `Go` version 1.15.10 and for [CVE-2021-23839](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-23839){: external}, [CVE-2021-23840](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-23840){: external}, and [CVE-2021-23841](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-23841){: external}. |
| GPU device plug-in and installler | 1c41e4b | b9ec8af | Updated image for [CVE-2021-27919](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-27919){: external}, [CVE-2020-28851](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-28851){: external}, and [CVE-2020-28852](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-28852){: external}. |
| {{site.data.keyword.cloud_notm}} Controller Manager | v1.20.4-4 | v1.20.5-1 | Updated to support the Kubernetes 1.20.5 release. Fixed a bug that prevented VPC load balancers from supporting more than 50 subnets in an account. |
| {{site.data.keyword.filestorage_full_notm}} plug-in and monitor | 388 | 389 | Updated to use `Go` version 1.15.8. |
| {{site.data.keyword.cloud_notm}} RBAC Operator | 86de2b7 | 3dd6bbc | Updated image for [CVE-2020-28851](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-28851){: external}. |
| Kubernetes | v1.20.4 | v1.20.5 | See the [Kubernetes release notes](https://github.com/kubernetes/kubernetes/releases/tag/v1.20.5){: external}. |
| Kubernetes NodeLocal DNS cache | 1.16.0 | 1.17.1 | See the [Kubernetes NodeLocal DNS cache release notes](https://github.com/kubernetes/dns/releases/tag/1.17.1){: external}. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.20.4_1531" caption-side="top"}

### Changelog for worker node fix pack 1.20.5_1534, released 29 March 2021
{: #1205_1534}

The following table shows the changes that are included in the worker node fix pack `1.20.5_1534`. Worker node patch updates can be applied by updating, reloading (in classic infrastructure), or replacing (in VPC infrastructure) the worker node.
{: shortdesc}

| Component | Previous | Current | Description |
| --------- | -------- | ------- | ----------- |
| Ubuntu 18.04 packages | 4.15.0-136-generic | 4.15.0-140-generic | Updated worker node images with kernel and package updates for [CVE-2020-27170](https://nvd.nist.gov/vuln/detail/CVE-2020-27170){: external}, [CVE-2020-27171](https://nvd.nist.gov/vuln/detail/CVE-2020-27171){: external}, [CVE-2021-27363](https://nvd.nist.gov/vuln/detail/CVE-2021-27363){: external}, [CVE-2021-27365](https://nvd.nist.gov/vuln/detail/CVE-2021-27365){: external}, [CVE-2021-28153](https://nvd.nist.gov/vuln/detail/CVE-2021-28153){: external}, and [CVE-2021-3449](https://nvd.nist.gov/vuln/detail/CVE-2021-3449){: external}. |
| Ubuntu 16.04 packages | 4.4.0-203-generic | 4.4.0-206-generic | Updated worker node images with kernel and package updates for [CVE-2021-27363](https://nvd.nist.gov/vuln/detail/CVE-2021-27363){: external}, [CVE-2021-27365](https://nvd.nist.gov/vuln/detail/CVE-2021-27365){: external}, and [CVE-2021-28153](https://nvd.nist.gov/vuln/detail/CVE-2021-28153){: external}.|
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.20.4_1532" caption-side="top"}

### Changelog for worker node fix pack 1.20.4_1532, released 12 March 2021
{: #1204_1532}

The following table shows the changes that are included in the worker node fix pack `1.20.4_1532`. Worker node patch updates can be applied by updating, reloading (in classic infrastructure), or replacing (in VPC infrastructure) the worker node.
{: shortdesc}

| Component | Previous | Current | Description |
| --------- | -------- | ------- | ----------- |
| Containerd | v1.4.3 | v1.4.4 | See the [containerd release notes](https://github.com/containerd/containerd/releases/tag/v1.4.4){: external}. The update resolves CVE-2021-21334 (see the [IBM security bulletin](https://www.ibm.com/support/pages/node/6433475){: external}). |
| Ubuntu 18.04 packages | N/A | N/A | Updated worker node image with package updates for [CVE-2021-21300](https://nvd.nist.gov/vuln/detail/CVE-2021-21300){: external}, [CVE-2021-24031](https://nvd.nist.gov/vuln/detail/CVE-2021-24031){: external}, [CVE-2021-24032](https://nvd.nist.gov/vuln/detail/CVE-2021-24032){: external}, [CVE-2021-27218](https://nvd.nist.gov/vuln/detail/CVE-2021-27218){: external}, and [CVE-2021-27219](https://nvd.nist.gov/vuln/detail/CVE-2021-27219){: external}. |
| Ubuntu 16.04 packages | N/A | N/A | Updated worker node image with package updates for{: external}, [CVE-2021-21300](https://nvd.nist.gov/vuln/detail/CVE-2021-21300){: external}, [CVE-2021-27218](https://nvd.nist.gov/vuln/detail/CVE-2021-27218){: external}, [CVE-2021-27219](https://nvd.nist.gov/vuln/detail/CVE-2021-27219){: external}, and [CVE-2021-3177](https://nvd.nist.gov/vuln/detail/CVE-2021-3177){: external}. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.20.4_1531" caption-side="top"}

### Changelog for worker node fix pack 1.20.4_1531, released 1 March 2021
{: #1204_1531}

The following table shows the changes that are included in the worker node fix pack `1.20.4_1531`. Worker node patch updates can be applied by updating, reloading (in classic infrastructure), or replacing (in VPC infrastructure) the worker node.
{: shortdesc}

| Component | Previous | Current | Description |
| --------- | -------- | ------- | ----------- |
| Kubernetes | v1.20.2 | v1.20.4 | See the [Kubernetes release notes](https://github.com/kubernetes/kubernetes/releases/tag/v1.20.4){: external}. |
| Ubuntu 18.04 packages | 4.15.0-135-generic | 4.15.0-136-generic | Updated worker node image with package updates for [CVE-2020-27619](https://nvd.nist.gov/vuln/detail/CVE-2020-27619){: external}, [CVE-2020-29372](https://nvd.nist.gov/vuln/detail/CVE-2020-29372){: external}, [CVE-2020-29374](https://nvd.nist.gov/vuln/detail/CVE-2020-29374){: external}, [CVE-2020-8625](https://nvd.nist.gov/vuln/detail/CVE-2020-8625){: external}, [CVE-2021-23840](https://nvd.nist.gov/vuln/detail/CVE-2021-23840){: external}, [CVE-2021-23841](https://nvd.nist.gov/vuln/detail/CVE-2021-23841){: external}, [CVE-2021-26937](https://nvd.nist.gov/vuln/detail/CVE-2021-26937){: external}, [CVE-2021-27212](https://nvd.nist.gov/vuln/detail/CVE-2021-27212){: external}, and [CVE-2021-3177](https://nvd.nist.gov/vuln/detail/CVE-2021-3177){: external}. |
| Ubuntu 16.04 packages | 4.4.0-201-generic | 4.4.0-203-generic | Updated worker node image with package updates for [CVE-2020-27619](https://nvd.nist.gov/vuln/detail/CVE-2020-27619){: external}, [CVE-2020-29372](https://nvd.nist.gov/vuln/detail/CVE-2020-29372){: external}, [CVE-2020-29374](https://nvd.nist.gov/vuln/detail/CVE-2020-29374){: external}, [CVE-2020-8625](https://nvd.nist.gov/vuln/detail/CVE-2020-8625){: external}, [CVE-2021-23840](https://nvd.nist.gov/vuln/detail/CVE-2021-23840){: external}, [CVE-2021-23841](https://nvd.nist.gov/vuln/detail/CVE-2021-23841){: external}, [CVE-2021-26937](https://nvd.nist.gov/vuln/detail/CVE-2021-26937){: external}, [CVE-2021-27212](https://nvd.nist.gov/vuln/detail/CVE-2021-27212){: external}, and [CVE-2021-3177](https://nvd.nist.gov/vuln/detail/CVE-2021-3177){: external}. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.20.2_1527" caption-side="top"}

### Changelog for master fix pack 1.20.4_1531, released 27 February 2021
{: #1204_1531_master}

The following table shows the changes that are included in the master fix pack patch update `1.20.4_1531`. Master patch updates are applied automatically.
{: shortdesc}

| Component | Previous | Current | Description |
| --- | --- | --- | --- |
| Calico | v3.17.2 | v3.17.3 | See the [Calico release notes](https://docs.projectcalico.org/releases){: external}. |
| {{site.data.keyword.cloud_notm}} Controller Manager | v1.20.4-2 | v1.20.4-4 | Updated to use `calicoctl` version 3.17.3. |
| Load balancer and load balancer monitor for {{site.data.keyword.cloud_notm}} Provider | 1165 | 1274 | Fixed a bug that might cause version 2.0 network load balancers (NLBs) to crash and restart on load balancer updates. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.20.4_1530." caption-side="top"}

### Changelog for master fix pack 1.20.4_1530, released 22 February 2021
{: #1204_1530}

The following table shows the changes that are included in the master fix pack patch update `1.20.4_1530`. Master patch updates are applied automatically.
{: shortdesc}

| Component | Previous | Current | Description |
| --- | --- | --- | --- |
| {{site.data.keyword.cloud_notm}} Controller Manager | v1.20.2-15 | v1.20.4-2 | Updated to support the Kubernetes 1.20.4 release and to use `Go` version 1.15.8. Updated image to implement additional IBM security controls. |
| Key Management Service provider | v2.2.4 | v2.3.0 | Updated to use `Go` version 1.15.7. Updated image to implement additional IBM security controls. |
| Kubernetes | v1.20.2 | v1.20.4 | See the [Kubernetes release notes](https://github.com/kubernetes/kubernetes/releases/tag/v1.20.4){: external}. |
| Operator Lifecycle Manager | 0.16.1-IKS-3 | 0.16.1-IKS-5 | Updated image for [CVE-2021-23839](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-23839){: external}, [CVE-2021-23840](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-23840){: external}, and [CVE-2021-23841](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-23841){: external}. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.20.2_1528." caption-side="top"}

### Changelog for 1.20.2_1528 (master) and 1.20.2_1527 (worker node), released 17 February 2021
{: #1202_1528}

The following table shows the changes that are included in the version updates for `1.20.2_1528` master fix pack and `1.20.2_1527` worker node fix pack.
{: shortdesc}

| Component | Previous | Current | Description |
| --- | --- | --- | --- |
| Calico | v3.16.5 | v3.17.2 | See the [Calico release notes](https://docs.projectcalico.org/releases){: external}. |
| Cluster health image | v1.2.6 | v1.2.8 | Updated to use `Go` version 1.15.7. Updated image to implement additional IBM security controls. |
| Gateway-enabled cluster controller | 1195 | 1232 | Updated to use `Go` version 1.15.7. |
| GPU device plug-in and installer | af5a6cb | 1c41e4b | Updated to support the Kubernetes 1.20 release. |
| IBM Calico extension | 567 | 618 | Updated to use `Go` version 1.15.7. |
| {{site.data.keyword.cloud_notm}} Controller Manager | v1.19.7-4 | v1.20.2-15 | Updated to:<ul><li>Support the Kubernetes 1.20.2 release.</li><li>Use `calicoctl` version 3.17.2.</li><li>Implement additional IBM security controls.</li><li>Address [DLA-2509-1](https://www.debian.org/lts/security/2020/dla-2509){: external}.</li><li>Make version 1.0 and 2.0 network load balancers (NLBs) to run as a non-root user by default, with privileged escalation as needed.</li></ul><p class="note">Although the Kubernetes [SCTP protocol](https://kubernetes.io/docs/concepts/services-networking/service/#sctp){: external} and [application protocol](https://kubernetes.io/docs/concepts/services-networking/service/#application-protocol){: external} features are generally available in the community release, creating load balancers that use these protocols is not supported in {{site.data.keyword.containerlong_notm}} clusters.</p> |
| {{site.data.keyword.cloud_notm}} File Storage plug-in and monitor | 385 | 388 | Improved the retry logic for provisioning persistent volume claims (PVCs). |
| {{site.data.keyword.cloud_notm}} RBAC Operator | f859228 | 86de2b7 | Updated to use `Go` version 1.15.7. |
| Key Management Service provider | v2.2.3 | v2.2.4 | Updated image to implement additional IBM security controls. |
| Kubernetes | v1.19.7 | v1.20.2 | See the [Kubernetes release notes](https://github.com/kubernetes/kubernetes/releases/tag/v1.20.2){: external}. |
| Kubernetes configuration | N/A | N/A | Updated the [feature gate configuration](/docs/containers?topic=containers-service-settings#feature-gates). |
| Kubernetes Dashboard | v2.0.5 | v2.1.0 | See the [Kubernetes Dashboard release notes](https://github.com/kubernetes/dashboard/releases/tag/v2.1.0){: external}. |
| Kubernetes Dashboard metrics scraper configuration | N/A | N/A | [Default Kubernetes network policy added](/docs/containers?topic=containers-network_policies#default_policy) to block most pods from accessing the Kubernetes Dashboard metrics. |
| Kubernetes Metrics Server | v0.3.7 | v0.4.2 | See the [Kubernetes Metrics Server release notes](https://github.com/kubernetes-sigs/metrics-server/releases/tag/v0.4.2){: external}. |
| Kubernetes NodeLocal DNS cache | 1.15.14 | 1.16.0 | See the [Kubernetes NodeLocal DNS cache release notes](https://github.com/kubernetes/dns/releases/tag/1.16.0){: external}. |
| Load balancer and load balancer monitor for {{site.data.keyword.cloud_notm}} Provider | 1078 | 1165 | Updated to run as a non-root user by default, with privileged escalation as needed. Updated to use `Go` version 1.15.7. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.19.7_1532 (master) and 1.19.7_1535 (worker node)." caption-side="top"}

## Version 1.19 changelog
{: #119_changelog}

Review the version 1.19 changelog.
{: shortdesc}

### Changelog for worker node fix pack 1.19.9_1542, released 12 April 2021
{: #1199_1542}

The following table shows the changes that are included in the worker node fix pack `1.19.9_1542`. Worker node patch updates can be applied by updating, reloading (in classic infrastructure), or replacing (in VPC infrastructure) the worker node.
{: shortdesc}

| Component | Previous | Current | Description |
| --------- | -------- | ------- | ----------- |
| HA proxy | 9b2dca | a3b1ff | The update addresses [CVE-2021-3449](https://nvd.nist.gov/vuln/detail/CVE-2021-3449){: external} and [CVE-2021-3450](https://nvd.nist.gov/vuln/detail/CVE-2021-3450){: external}. |
| Ubuntu 18.04 packages | N/A | N/A | Updated worker node images with package updates for [CVE-2021-22876](https://nvd.nist.gov/vuln/detail/CVE-2021-22876){: external}. |
| Ubuntu 16.04 packages | N/A | N/A | Updated worker node images with package updates for [CVE-2021-22876](https://nvd.nist.gov/vuln/detail/CVE-2021-22876){: external}.|
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.19.9_1541" caption-side="top"}

### Changelog for master fix pack 1.19.9_1540, released 30 March 2021
{: #1199_1540}

The following table shows the changes that are included in the master fix pack patch update `1.19.9_1540`. Master patch updates are applied automatically.
{: shortdesc}

| Component | Previous | Current | Description |
| --- | --- | --- | --- |
| {{site.data.keyword.at_short}} event | N/A | N/A | Now, the `containers-kubernetes.version.update` event is sent to {{site.data.keyword.at_short}} when a master fix pack update is initiated for a cluster. |
| Cluster health image | v1.2.8 | v1.2.9 | Updated image for [CVE-2020-28851](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-28851){: external}. |
| Gateway-enabled cluster controller | 1232 | 1322 | Updated to use `Go` version 1.15.10 and for [CVE-2021-23839](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-23839){: external}, [CVE-2021-23840](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-23840){: external}, and [CVE-2021-23841](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-23841){: external}. |
| GPU device plug-in and installer | af5a6cb | c1c6dd3 | Updated image for [CVE-2021-27919](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-27919){: external}, [CVE-2020-28851](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-28851){: external}, and [CVE-2020-28852](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-28852){: external}. |
| {{site.data.keyword.cloud_notm}} Controller Manager | v1.19.8-4 | v1.19.9-1 | Updated to support the Kubernetes 1.19.9 release. Fixed a bug that prevented VPC load balancers from supporting more than 50 subnets in an account. |
| {{site.data.keyword.filestorage_full_notm}} plug-in and monitor | 388 | 389 | Updated to use `Go` version 1.15.8. |
| {{site.data.keyword.cloud_notm}} RBAC Operator | 86de2b7 | 3dd6bbc | Updated image for [CVE-2020-28851](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-28851){: external}. |
| Kubernetes | v1.19.8 | v1.19.9 | See the [Kubernetes release notes](https://github.com/kubernetes/kubernetes/releases/tag/v1.19.9){: external}. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.19.8_1538" caption-side="top"}

### Changelog for worker node fix pack 1.19.9_1541, released 29 March 2021
{: #1199_1541}

The following table shows the changes that are included in the worker node fix pack `1.19.9_1541`. Worker node patch updates can be applied by updating, reloading (in classic infrastructure), or replacing (in VPC infrastructure) the worker node.
{: shortdesc}

| Component | Previous | Current | Description |
| --------- | -------- | ------- | ----------- |
| Ubuntu 18.04 packages | 4.15.0-136-generic | 4.15.0-140-generic | Updated worker node images with kernel and package updates for [CVE-2020-27170](https://nvd.nist.gov/vuln/detail/CVE-2020-27170){: external}, [CVE-2020-27171](https://nvd.nist.gov/vuln/detail/CVE-2020-27171){: external}, [CVE-2021-27363](https://nvd.nist.gov/vuln/detail/CVE-2021-27363){: external}, [CVE-2021-27365](https://nvd.nist.gov/vuln/detail/CVE-2021-27365){: external}, [CVE-2021-28153](https://nvd.nist.gov/vuln/detail/CVE-2021-28153){: external}, and [CVE-2021-3449](https://nvd.nist.gov/vuln/detail/CVE-2021-3449){: external}. |
| Ubuntu 16.04 packages | 4.4.0-203-generic | 4.4.0-206-generic | Updated worker node images with kernel and package updates for [CVE-2021-27363](https://nvd.nist.gov/vuln/detail/CVE-2021-27363){: external}, [CVE-2021-27365](https://nvd.nist.gov/vuln/detail/CVE-2021-27365){: external}, and [CVE-2021-28153](https://nvd.nist.gov/vuln/detail/CVE-2021-28153){: external}.|
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.19.8_1539" caption-side="top"}

### Changelog for worker node fix pack 1.19.8_1539, released 12 March 2021
{: #1198_1539}

The following table shows the changes that are included in the worker node fix pack `1.19.8_1539`. Worker node patch updates can be applied by updating, reloading (in classic infrastructure), or replacing (in VPC infrastructure) the worker node.
{: shortdesc}

| Component | Previous | Current | Description |
| --------- | -------- | ------- | ----------- |
| Containerd | v1.4.3 | v1.4.4 | See the [containerd release notes](https://github.com/containerd/containerd/releases/tag/v1.4.4){: external}. The update resolves CVE-2021-21334 (see the [IBM security bulletin](https://www.ibm.com/support/pages/node/6433475){: external}). |
| Ubuntu 18.04 packages | N/A | N/A | Updated worker node image with package updates for [CVE-2021-21300](https://nvd.nist.gov/vuln/detail/CVE-2021-21300){: external}, [CVE-2021-24031](https://nvd.nist.gov/vuln/detail/CVE-2021-24031){: external}, [CVE-2021-24032](https://nvd.nist.gov/vuln/detail/CVE-2021-24032){: external}, [CVE-2021-27218](https://nvd.nist.gov/vuln/detail/CVE-2021-27218){: external}, and [CVE-2021-27219](https://nvd.nist.gov/vuln/detail/CVE-2021-27219){: external}. |
| Ubuntu 16.04 packages | N/A | N/A | Updated worker node image with package updates for{: external}, [CVE-2021-21300](https://nvd.nist.gov/vuln/detail/CVE-2021-21300){: external}, [CVE-2021-27218](https://nvd.nist.gov/vuln/detail/CVE-2021-27218){: external}, [CVE-2021-27219](https://nvd.nist.gov/vuln/detail/CVE-2021-27219){: external}, and [CVE-2021-3177](https://nvd.nist.gov/vuln/detail/CVE-2021-3177){: external}. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.19.8_1538" caption-side="top"}

### Changelog for worker node fix pack 1.19.8_1538, released 1 March 2021
{: #1198_1538}

The following table shows the changes that are included in the worker node fix pack `1.19.8_1538`. Worker node patch updates can be applied by updating, reloading (in classic infrastructure), or replacing (in VPC infrastructure) the worker node.
{: shortdesc}

| Component | Previous | Current | Description |
| --------- | -------- | ------- | ----------- |
| Kubernetes | v1.19.7 | v1.19.8 | See the [Kubernetes release notes](https://github.com/kubernetes/kubernetes/releases/tag/v1.19.8){: external}. |
| Ubuntu 18.04 packages | 4.15.0-135-generic | 4.15.0-136-generic | Updated worker node image with package updates for [CVE-2020-27619](https://nvd.nist.gov/vuln/detail/CVE-2020-27619){: external}, [CVE-2020-29372](https://nvd.nist.gov/vuln/detail/CVE-2020-29372){: external}, [CVE-2020-29374](https://nvd.nist.gov/vuln/detail/CVE-2020-29374){: external}, [CVE-2020-8625](https://nvd.nist.gov/vuln/detail/CVE-2020-8625){: external}, [CVE-2021-23840](https://nvd.nist.gov/vuln/detail/CVE-2021-23840){: external}, [CVE-2021-23841](https://nvd.nist.gov/vuln/detail/CVE-2021-23841){: external}, [CVE-2021-26937](https://nvd.nist.gov/vuln/detail/CVE-2021-26937){: external}, [CVE-2021-27212](https://nvd.nist.gov/vuln/detail/CVE-2021-27212){: external}, and [CVE-2021-3177](https://nvd.nist.gov/vuln/detail/CVE-2021-3177){: external}. |
| Ubuntu 16.04 packages | 4.4.0-201-generic | 4.4.0-203-generic | Updated worker node image with package updates for [CVE-2020-27619](https://nvd.nist.gov/vuln/detail/CVE-2020-27619){: external}, [CVE-2020-29372](https://nvd.nist.gov/vuln/detail/CVE-2020-29372){: external}, [CVE-2020-29374](https://nvd.nist.gov/vuln/detail/CVE-2020-29374){: external}, [CVE-2020-8625](https://nvd.nist.gov/vuln/detail/CVE-2020-8625){: external}, [CVE-2021-23840](https://nvd.nist.gov/vuln/detail/CVE-2021-23840){: external}, [CVE-2021-23841](https://nvd.nist.gov/vuln/detail/CVE-2021-23841){: external}, [CVE-2021-26937](https://nvd.nist.gov/vuln/detail/CVE-2021-26937){: external}, [CVE-2021-27212](https://nvd.nist.gov/vuln/detail/CVE-2021-27212){: external}, and [CVE-2021-3177](https://nvd.nist.gov/vuln/detail/CVE-2021-3177){: external}. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.19.7_1535" caption-side="top"}

### Changelog for master fix pack 1.19.8_1538, released 27 February 2021
{: #1198_1538_master}

The following table shows the changes that are included in the master fix pack patch update `1.19.8_1538`. Master patch updates are applied automatically.
{: shortdesc}

| Component | Previous | Current | Description |
| --- | --- | --- | --- |
| Calico | v3.16.7 | v3.16.8 | See the [Calico release notes](https://docs.projectcalico.org/releases){: external}. |
| {{site.data.keyword.cloud_notm}} Controller Manager | v1.19.8-2 | v1.19.8-4 | Updated to use `calicoctl` version 3.16.8. |
| Load balancer and load balancer monitor for {{site.data.keyword.cloud_notm}} Provider | 1165 | 1274 | Fixed a bug that might cause version 2.0 network load balancers (NLBs) to crash and restart on load balancer updates. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.19.8_1537" caption-side="top"}

### Changelog for master fix pack 1.19.8_1537, released 22 February 2021
{: #1198_1537}

The following table shows the changes that are included in the master fix pack patch update `1.19.8_1537`. Master patch updates are applied automatically.
{: shortdesc}

| Component | Previous | Current | Description |
| --- | --- | --- | --- |
| Calico | v3.16.5 | v3.16.7 | See the [Calico release notes](https://docs.projectcalico.org/releases){: external}. |
| Cluster health image | v1.2.6 | v1.2.8 | Updated to use `Go` version 1.15.7. Updated image to implement additional IBM security controls. |
| Gateway-enabled cluster controller | 1195 | 1232 | Updated to use `Go` version 1.15.7. |
| IBM Calico extension | 567 | 618 | Updated to use `Go` version 1.15.7. |
| {{site.data.keyword.cloud_notm}} Controller Manager | v1.19.7-4 | v1.19.8-2 | Updated to support the Kubernetes 1.19.8 release, to use `Go` version 1.15.8, and to use `calicoctl` version 3.16.7. Updated image to implement additional IBM security controls and for [DLA-2509-1](https://www.debian.org/lts/security/2020/dla-2509){: external}. Updated version 1.0 and 2.0 network load balancers (NLBs) to run as a non-root user by default, with privileged escalation as needed. |
| {{site.data.keyword.filestorage_full_notm}} plug-in and monitor | 385 | 388 | Improved the retry logic for provisioning persistent volume claims (PVCs). |
| {{site.data.keyword.cloud_notm}} RBAC Operator | f859228 | 86de2b7 | Updated to use `Go` version 1.15.7. |
| Key Management Service provider | v2.2.3 | v2.2.5 | Updated to use `Go` version 1.15.7. Updated image to implement additional IBM security controls. |
| Kubernetes | v1.19.7 | v1.19.8 | See the [Kubernetes release notes](https://github.com/kubernetes/kubernetes/releases/tag/v1.19.8){: external}. |
| Load balancer and load balancer monitor for {{site.data.keyword.cloud_notm}} Provider | 1078 | 1165 | Updated to run as a non-root user by default, with privileged escalation as needed. Updated to use `Go` version 1.15.7. |
| Operator Lifecycle Manager | 0.16.1-IKS-3 | 0.16.1-IKS-5 | Updated image for [CVE-2021-23839](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-23839){: external}, [CVE-2021-23840](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-23840){: external}, and [CVE-2021-23841](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-23841){: external}. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.19.7_1532" caption-side="top"}

### Changelog for worker node fix pack 1.19.7_1535, released 15 February 2021
{: #1197_1535}

The following table shows the changes that are included in the worker node fix pack `1.19.7_1535`. Worker node patch updates can be applied by updating, reloading (in classic infrastructure), or replacing (in VPC infrastructure) the worker node.
{: shortdesc}

| Component | Previous | Current | Description |
| --------- | -------- | ------- | ----------- |
| Ubuntu 18.04 packages | N/A | N/A | Updated worker node image with package updates for [CVE-2020-36221](https://nvd.nist.gov/vuln/detail/CVE-2020-36221){: external}, [CVE-2020-36222](https://nvd.nist.gov/vuln/detail/CVE-2020-36222){: external}, [CVE-2020-36223](https://nvd.nist.gov/vuln/detail/CVE-2020-36223){: external}, [CVE-2020-36224](https://nvd.nist.gov/vuln/detail/CVE-2020-36224){: external}, [CVE-2020-36225](https://nvd.nist.gov/vuln/detail/CVE-2020-36225){: external}, [CVE-2020-36226](https://nvd.nist.gov/vuln/detail/CVE-2020-36226){: external}, [CVE-2020-36227](https://nvd.nist.gov/vuln/detail/CVE-2020-36227){: external}, [CVE-2020-36228](https://nvd.nist.gov/vuln/detail/CVE-2020-36228){: external}, [CVE-2020-36229](https://nvd.nist.gov/vuln/detail/CVE-2020-36229){: external}, [CVE-2020-36230](https://nvd.nist.gov/vuln/detail/CVE-2020-36230){: external}, [CVE-2021-25682](https://nvd.nist.gov/vuln/detail/CVE-2021-25682){: external}, [CVE-2021-25683](https://nvd.nist.gov/vuln/detail/CVE-2021-25683){: external}, and [CVE-2021-25684](https://nvd.nist.gov/vuln/detail/CVE-2021-25684){: external}. |
| Ubuntu 16.04 packages | N/A | N/A | Updated worker node image with package updates for [CVE-2020-36221](https://nvd.nist.gov/vuln/detail/CVE-2020-36221){: external}, [CVE-2020-36222](https://nvd.nist.gov/vuln/detail/CVE-2020-36222){: external}, [CVE-2020-36223](https://nvd.nist.gov/vuln/detail/CVE-2020-36223){: external}, [CVE-2020-36224](https://nvd.nist.gov/vuln/detail/CVE-2020-36224){: external}, [CVE-2020-36225](https://nvd.nist.gov/vuln/detail/CVE-2020-36225){: external}, [CVE-2020-36226](https://nvd.nist.gov/vuln/detail/CVE-2020-36226){: external}, [CVE-2020-36227](https://nvd.nist.gov/vuln/detail/CVE-2020-36227){: external}, [CVE-2020-36228](https://nvd.nist.gov/vuln/detail/CVE-2020-36228){: external}, [CVE-2020-36229](https://nvd.nist.gov/vuln/detail/CVE-2020-36229){: external}, [CVE-2020-36230](https://nvd.nist.gov/vuln/detail/CVE-2020-36230){: external}, [CVE-2021-25682](https://nvd.nist.gov/vuln/detail/CVE-2021-25682){: external}, [CVE-2021-25683](https://nvd.nist.gov/vuln/detail/CVE-2021-25683){: external}, and [CVE-2021-25684](https://nvd.nist.gov/vuln/detail/CVE-2021-25684){: external}. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.19.7_1534" caption-side="top"}

### Changelog for worker node fix pack 1.19.7_1534, released 3 February 2021
{: #1197_1534}

The following table shows the changes that are included in the worker node fix pack `1.19.7_1534`. Worker node patch updates can be applied by updating, reloading (in classic infrastructure), or replacing (in VPC infrastructure) the worker node.
{: shortdesc}

| Component | Previous | Current | Description |
| --------- | -------- | ------- | ----------- |
| Metadata updates | N/A | N/A | Updated the worker node version fix pack metadata for internal documentation purposes. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.19.7_1533" caption-side="top"}

### Changelog for worker node fix pack 1.19.7_1533, released 1 February 2021
{: #1197_1533}

The following table shows the changes that are included in the worker node fix pack `1.19.7_1533`. Worker node patch updates can be applied by updating, reloading (in classic infrastructure), or replacing (in VPC infrastructure) the worker node.
{: shortdesc}

| Component | Previous | Current | Description |
| --------- | -------- | ------- | ----------- |
| Ubuntu 16.04 packages | 4.4.0-200-generic | 4.4.0-201-generic | Updated worker node image with kernel and package updates for [CVE-2019-14834](https://nvd.nist.gov/vuln/detail/CVE-2019-14834){: external}, [CVE-2020-25681](https://nvd.nist.gov/vuln/detail/CVE-2020-25681){: external}, [CVE-2020-25682](https://nvd.nist.gov/vuln/detail/CVE-2020-25682){: external}, [CVE-2020-25683](https://nvd.nist.gov/vuln/detail/CVE-2020-25683){: external}, [CVE-2020-25684](https://nvd.nist.gov/vuln/detail/CVE-2020-25684){: external}, [CVE-2020-25685](https://nvd.nist.gov/vuln/detail/CVE-2020-25685){: external}, [CVE-2020-25686](https://nvd.nist.gov/vuln/detail/CVE-2020-25686){: external}, [CVE-2020-25687](https://nvd.nist.gov/vuln/detail/CVE-2020-25687){: external}, [CVE-2020-27777](https://nvd.nist.gov/vuln/detail/CVE-2020-27777){: external}, [CVE-2021-23239](https://nvd.nist.gov/vuln/detail/CVE-2021-23239){: external}, and [CVE-2021-3156](https://nvd.nist.gov/vuln/detail/CVE-2021-3156){: external}. |
| Ubuntu 18.04 packages | 4.15.0-132-generic | 4.15.0-135-generic | Updated worker node image with kernel and package updates for [CVE-2019-12761](https://nvd.nist.gov/vuln/detail/CVE-2019-12761){: external}, [CVE-2019-14834](https://nvd.nist.gov/vuln/detail/CVE-2019-14834){: external}, [CVE-2020-25681](https://nvd.nist.gov/vuln/detail/CVE-2020-25681){: external}, [CVE-2020-25682](https://nvd.nist.gov/vuln/detail/CVE-2020-25682){: external}, [CVE-2020-25683](https://nvd.nist.gov/vuln/detail/CVE-2020-25683){: external}, [CVE-2020-25684](https://nvd.nist.gov/vuln/detail/CVE-2020-25684){: external}, [CVE-2020-25685](https://nvd.nist.gov/vuln/detail/CVE-2020-25685){: external}, [CVE-2020-25686](https://nvd.nist.gov/vuln/detail/CVE-2020-25686){: external}, [CVE-2020-25687](https://nvd.nist.gov/vuln/detail/CVE-2020-25687){: external}, [CVE-2020-27777](https://nvd.nist.gov/vuln/detail/CVE-2020-27777){: external}, [CVE-2021-23239](https://nvd.nist.gov/vuln/detail/CVE-2021-23239){: external}, and [CVE-2021-3156](https://nvd.nist.gov/vuln/detail/CVE-2021-3156){: external}. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.19.7_1532" caption-side="top"}

### Changelog for master fix pack 1.19.7_1532, released 19 January 2021
{: #1197_1532_master}

The following table shows the changes that are included in the master fix pack patch update `1.19.7_1532`. Master patch updates are applied automatically.
{: shortdesc}

| Component | Previous | Current | Description |
| --- | --- | --- | --- |
| Cluster health image | v1.2.4 | v1.2.6 | Updated image to implement additional IBM security controls. |
| Gateway-enabled cluster controller | 1184 | 1195 | Updated image for [CVE-2020-1971](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-1971){: external}. |
| GPU device plug-in and installer | c26e2ae | af5a6cb | Updated image for [CVE-2021-3114](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-3114){: external}, [CVE-2021-3115](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-3115){: external}, [CVE-2020-27350](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-27350){: external}, [CVE-2020-29361](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-29361){: external}, [CVE-2020-29362](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-29362){: external}, [CVE-2020-29363](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-29363){: external}, [CVE-2020-1971](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-1971){: external}, [CVE-2020-8231](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-8231){: external}, [CVE-2020-8284](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-8284){: external}, [CVE-2020-8285](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-8285){: external}, and [CVE-2020-8286](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-8286){: external}. |
| IBM Calico extension | 556 | 567 | Updated image for [CVE-2020-1971](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-1971){: external} and [CVE-2020-24659](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-24659){: external}. |
| {{site.data.keyword.cloud_notm}} Controller Manager | v1.19.6-1 | v1.19.7-4 | Updated to support the Kubernetes 1.19.7 release and to implement additional IBM security controls. |
| {{site.data.keyword.filestorage_full_notm}} plug-in and monitor | 384 | 385 | Updated image for [CVE-2020-1971](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-1971){: external} and [CVE-2020-24659](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-24659){: external}. |
| Key Management Service provider | v2.2.2 | v2.2.3 | Fixed bug to ignore conflict errors during KMS secret reencrpytion. Updated to use `Go` version 1.15.5. Updated image to implement additional IBM security controls and for [CVE-2020-1971](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-1971){: external}. |
| Kubernetes | v1.19.6 | v1.19.7 | See the [Kubernetes release notes](https://github.com/kubernetes/kubernetes/releases/tag/v1.19.7){: external}. Updated to implement additional IBM security controls. |
| Kubernetes Dashboard | v2.0.4 | v2.0.5 | See the [Kubernetes Dashboard release notes](https://github.com/kubernetes/dashboard/releases/tag/v2.0.5){: external}. |
| Kubernetes Dashboard metrics scraper | v1.0.4 | v1.0.6 | See the [Kubernetes Dashboard metrics scraper release notes](https://github.com/kubernetes-sigs/dashboard-metrics-scraper/releases/tag/v1.0.6){: external}. |
| Load balancer and load balancer monitor for {{site.data.keyword.cloud_notm}} Provider | 1004 | 1078 | Updated image for [CVE-2020-1971](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-1971){: external}. |
| Operator Lifecycle Manager Catalog | v1.14.0 | v1.15.3 | See the [Operator Lifecycle Manager Catalog release notes](https://github.com/operator-framework/operator-registry/releases/tag/v1.15.3){: external}. |
| Operator Lifecycle Manager | 0.16.1 | 0.16.1-IKS-3 | Updated image for [CVE-2020-1971](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-1971){: external}. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.19.6_1531" caption-side="top"}

### Changelog for worker node fix pack 1.19.7_1532, released 18 January 2021
{: #1197_1532}

The following table shows the changes that are included in the worker node fix pack `1.19.7_1532`. Worker node patch updates can be applied by updating, reloading (in classic infrastructure), or replacing (in VPC infrastructure) the worker node.
{: shortdesc}

| Component | Previous | Current | Description |
| --------- | -------- | ------- | ----------- |
| Kubernetes | v1.19.5 | v1.19.7 | See the [Kubernetes release notes](https://github.com/kubernetes/kubernetes/releases/tag/v1.19.7){: external}. |
| Ubuntu 16.04 packages | 4.4.0-197-generic | 4.4.0-200-generic | Updated worker node image with kernel and package updates for [CVE-2018-20482](https://nvd.nist.gov/vuln/detail/CVE-2018-20482){: external}, [CVE-2019-9923](https://nvd.nist.gov/vuln/detail/CVE-2019-9923){: external}, [CVE-2020-28374](https://nvd.nist.gov/vuln/detail/CVE-2020-28374){: external}, [CVE-2020-29361](https://nvd.nist.gov/vuln/detail/CVE-2020-29361) external}, and [CVE-2020-29362](https://nvd.nist.gov/vuln/detail/CVE-2020-29362) external}. |
| Ubuntu 18.04 packages | 4.15.0-128-generic | 4.15.0-132-generic | Updated worker node image with kernel and package updates for{: external}, [CVE-2018-20482](https://nvd.nist.gov/vuln/detail/CVE-2018-20482){: external}, [CVE-2019-9923](https://nvd.nist.gov/vuln/detail/CVE-2019-9923){: external}, [CVE-2020-28374](https://nvd.nist.gov/vuln/detail/CVE-2020-28374){: external}, [CVE-2020-29361](https://nvd.nist.gov/vuln/detail/CVE-2020-29361){: external}, [CVE-2020-29362](https://nvd.nist.gov/vuln/detail/CVE-2020-29362){: external}, [CVE-2020-29363](https://nvd.nist.gov/vuln/detail/CVE-2020-29363){: external}, [CVE-2021-1052](https://nvd.nist.gov/vuln/detail/CVE-2021-1052){: external}, [CVE-2021-1053](https://nvd.nist.gov/vuln/detail/CVE-2021-1053){: external}, and [CVE-2019-19770](https://nvd.nist.gov/vuln/detail/CVE-2019-19770){: external}. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.19.5_1530" caption-side="top"}

### Changelog for master fix pack 1.19.6_1531, released 6 January 2021
{: #1196_1531}

The following table shows the changes that are included in the master fix pack patch update `1.19.6_1531`. Master patch updates are applied automatically.
{: shortdesc}

| Component | Previous | Current | Description |
| --- | --- | --- | --- |
| IBM Calico extension | 538 | 556 | Updated image to include the `ip` command. |
| {{site.data.keyword.cloud_notm}} Controller Manager | v1.19.5-1 | v1.19.6-1 | Updated to support the Kubernetes 1.19.6 release and to use `Go` version 1.15.5. |
| {{site.data.keyword.filestorage_full_notm}} plug-in | N/A | N/A | Updated to run with a privileged security context. |
| {{site.data.keyword.cloud_notm}} RBAC Operator | c148a8a | f859228 | Updated image for [CVE-2020-1971](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-1971){: external} and [CVE-2020-24659](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-24659){: external}. |
| Kubernetes | v1.19.5 | v1.19.6 | See the [Kubernetes release notes](https://github.com/kubernetes/kubernetes/releases/tag/v1.19.6){: external}. |
| Kubernetes `NodeLocal` DNS cache | N/A | N/A | Updated to run with a least privileged security context. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.19.5_1529" caption-side="top"}

### Changelog for worker node fix pack 1.19.5_1530, released 21 December 2020
{: #1195_1530}

The following table shows the changes that are included in the worker node fix pack `1.19.5_1530`. Worker node patch updates can be applied by updating, reloading (in classic infrastructure), or replacing (in VPC infrastructure) the worker node.
{: shortdesc}

| Component | Previous | Current | Description |
| --------- | -------- | ------- | ----------- |
| Ubuntu 16.04 packages | N/A | N/A | Updated worker node image with package updates for: [CVE-2020-1971](https://nvd.nist.gov/vuln/detail/CVE-2020-1971){: external}, [CVE-2020-27350](https://nvd.nist.gov/vuln/detail/CVE-2020-27350){: external}, [CVE-2020-27351](https://nvd.nist.gov/vuln/detail/CVE-2020-27351){: external}, [CVE-2020-8284](https://nvd.nist.gov/vuln/detail/CVE-2020-8284){: external}, [CVE-2020-8285](https://nvd.nist.gov/vuln/detail/CVE-2020-8285){: external}, and [CVE-2020-8286](https://nvd.nist.gov/vuln/detail/CVE-2020-8286){: external}. |
| Ubuntu 18.04 packages | 4.15.0-126-generic | 4.15.0-128-generic | Updated worker node image with kernel and package updates for: [CVE-2020-1971](https://nvd.nist.gov/vuln/detail/CVE-2020-1971){: external}, [CVE-2020-27350](https://nvd.nist.gov/vuln/detail/CVE-2020-27350){: external}, [CVE-2020-27351](https://nvd.nist.gov/vuln/detail/CVE-2020-27351){: external}, [CVE-2020-8284](https://nvd.nist.gov/vuln/detail/CVE-2020-8284){: external}, [CVE-2020-8285](https://nvd.nist.gov/vuln/detail/CVE-2020-8285){: external}, and [CVE-2020-8286](https://nvd.nist.gov/vuln/detail/CVE-2020-8286){: external}. |
| Kubernetes | v1.19.4 | v1.19.5 | See the [Kubernetes changelogs.](https://github.com/kubernetes/kubernetes/releases/tag/v1.19.5){: external} |
| Ephemeral storage reservations | N/A | N/A | Reserve local ephermal storage to prevent workload evictions. |
| HAProxy | db4e6d | 9b2dca | Image update for [CVE-2020-1971](https://nvd.nist.gov/vuln/detail/CVE-2020-1971){: external} and [CVE-2020-24659](https://nvd.nist.gov/vuln/detail/CVE-2020-24659){: external}. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.19.5_1529" caption-side="top"}

### Changelog for master fix pack 1.19.5_1529, released 14 December 2020
{: #1195_1529}

The following table shows the changes that are included in the master fix pack patch update `1.19.5_1529`. Master patch updates are applied automatically.
{: shortdesc}

| Component | Previous | Current | Description |
| --- | --- | --- | --- |
| Cluster health image | v1.2.3 | v1.2.4 | Updated image to implement additional IBM security controls. |
| etcd | v3.4.13 | v3.4.14 | See the [etcd release notes](https://github.com/etcd-io/etcd/releases/v3.4.14){: external}. |
| Gateway-enabled cluster controller | 1105 | 1184 | Updated to use `Go` version 1.15.5. Updated image to implement additional IBM security controls. |
| GPU device plug-in and installer | b966c41 | c26e2ae | Updated the GPU drivers to version [450.80.02](https://www.nvidia.com/download/driverResults.aspx/165294){: external}. Updated image for [CVE-2020-28367](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-28367){: external}, [CVE-2020-28366](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-28366){: external}, and [CVE-2020-28362](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-28362){: external}. Updated image to implement additional IBM security controls. |
| IBM Calico extension | 378 | 538 | Updated to use the universal base image (UBI) and to use `Go` version 1.15.5. Updated image to implement additional IBM security controls. |
| {{site.data.keyword.cloud_notm}} Controller Manager |  v1.19.4-1 | v1.19.5-1 | Updated to support the Kubernetes 1.19.5 release. Updated image to implement additional IBM security controls. Fixed a bug in VPC load balancer creation when the cluster, VPC, or subnet are in a different resource group.  |
| {{site.data.keyword.filestorage_full_notm}} monitor | 379 | 384 | Updated to use `Go` version 1.15.5 and to run as a non-root user. Updated image to implement additional IBM security controls. |
| {{site.data.keyword.filestorage_full_notm}} plug-in | 379 | 384 | Updated to use `Go` version 1.15.5 and to run with a least privileged security context. Updated image to implement additional IBM security controls. |
| {{site.data.keyword.cloud_notm}} RBAC Operator | 197bc70 | c148a8a | Updated to use `Go` version 1.15.6 and updated image to implement additional IBM security controls. |
| Key management service (KMS) provider | v2.2.1 | v2.2.2 | Updated image to implement additional IBM security controls. |
| Kubernetes | v1.19.4 | v1.19.5 | See the [Kubernetes release notes](https://github.com/kubernetes/kubernetes/releases/tag/v1.19.5){: external}. |
| Kubernetes `NodeLocal` DNS cache | N/A | N/A | Updated the `NodeLocal` DNS cache configuration to support [customizing the `node-local-dns` config map](/docs/containers?topic=containers-cluster_dns#dns_nodelocal_customize). |
| Load balancer and load balancer monitor for {{site.data.keyword.cloud_notm}} Provider | 208 | 1004 | Updated Alpine base image to version 3.12 and to use `Go` version 1.15.5. Updated image for [CVE-2020-8037](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-8037){: external} and [CVE-2020-28928](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-28928){: external}. Updated image to implement additional IBM security controls. |
| Operator Lifecycle Manager | N/A | N/A | Updated to run as a non-root user. |
| OpenVPN client | 2.4.6-r3-IKS-116 | 2.4.6-r3-IKS-301 | Updated image to implement additional IBM security controls. |
| OpenVPN server | 2.4.6-r3-IKS-222 | 2.4.6-r3-IKS-301 | Updated image to implement additional IBM security controls. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.19.4_1527" caption-side="top"}

### Changelog for worker node fix pack 1.19.4_1529, released 11 December 2020
{: #1194_1529}

The following table shows the changes that are included in the worker node fix pack `1.19.4_1529`. Worker node patch updates can be applied by updating, reloading (in classic infrastructure), or replacing (in VPC infrastructure) the worker node.
{: shortdesc}

| Component | Previous | Current | Description |
| --------- | -------- | ------- | ----------- |
| Ubuntu 18.04 bare metal kernel | 4.15.0-126-generic | 4.15.0-123-generic | **Bare metal worker nodes**: Reverted the kernel version for bare metal worker nodes while Canonical addresses issues with the previous version that prevented worker nodes from being reloaded or updated. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.19.4_1528" caption-side="top"}

### Changelog for worker node fix pack 1.19.4_1528, released 7 December 2020
{: #1194_1528}

The following table shows the changes that are included in the worker node fix pack `1.19.4_1528`. Worker node patch updates can be applied by updating, reloading (in classic infrastructure), or replacing (in VPC infrastructure) the worker node.
{: shortdesc}

| Component | Previous | Current | Description |
| --------- | -------- | ------- | ----------- |
| Containerd | v1.4.1 | v1.4.3 | See the [containerd release notes](https://github.com/containerd/containerd/releases/tag/v1.4.3){: external}. The update resolves CVE-2020–15257 (see the [IBM security bulletin](https://www.ibm.com/support/pages/node/6387878){: external}). |
| HAProxy | 1.8.26-384f42 | db4e6d | Added provenance labels for source tracking. |
| Ubuntu 18.04 packages | 4.15.0-123-generic | 4.15.0-126-generic | Updated worker node image with kernel and package updates for [CVE-2020-14351](https://nvd.nist.gov/vuln/detail/CVE-2020-14351){: external} and [CVE-2020-4788](https://nvd.nist.gov/vuln/detail/CVE-2020-4788){: external}. |
| Ubuntu 16.04 packages | 4.4.0-194-generic | 4.4.0-197-generic | Updated worker node image with kernel and package updates for [CVE-2020-0427](https://nvd.nist.gov/vuln/detail/CVE-2020-0427){: external}, [CVE-2020-12352](https://nvd.nist.gov/vuln/detail/CVE-2020-12352){: external}, [CVE-2020-14351](https://nvd.nist.gov/vuln/detail/CVE-2020-14351){: external}, [CVE-2020-25645](https://nvd.nist.gov/vuln/detail/CVE-2020-25645){: external}, and [CVE-2020-4788](https://nvd.nist.gov/vuln/detail/CVE-2020-4788){: external}. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.19.4_1527" caption-side="top"}

### Changelog for worker node fix pack 1.19.4_1527, released 23 November 2020
{: #1194_1527_worker}

The following table shows the changes that are included in the worker node fix pack `1.19.4_1527`. Worker node patch updates can be applied by updating, reloading (in classic infrastructure), or replacing (in VPC infrastructure) the worker node.
{: shortdesc}

| Component | Previous | Current | Description |
| --------- | -------- | ------- | ----------- |
| Kubernetes | v1.19.3 | v1.19.4 | See the [Kubernetes changelog](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG/CHANGELOG-1.16.md#v1194){: external}.|
| Ubuntu 18.04 packages | 4.15.0-122-generic | 4.15.0-123-generic | Updated worker node image with kernel and package updates for [CVE-2020-25692](https://nvd.nist.gov/vuln/detail/CVE-2020-25692){: external}, [CVE-2020-25709](https://nvd.nist.gov/vuln/detail/CVE-2020-25709){: external}, [CVE-2020-25710](https://nvd.nist.gov/vuln/detail/CVE-2020-25710){: external}, [CVE-2020-28196](https://nvd.nist.gov/vuln/detail/CVE-2020-28196){: external}, and [CVE-2020-8694](https://nvd.nist.gov/vuln/detail/CVE-2020-8694){: external}. |
| Ubuntu 16.04 packages | 4.4.0-193-generic | 4.4.0-194-generic | Updated worker node image with kernel and package updates for [CVE-2020-25692](https://nvd.nist.gov/vuln/detail/CVE-2020-25692){: external}, [CVE-2020-25709](https://nvd.nist.gov/vuln/detail/CVE-2020-25709){: external}, [CVE-2020-25710](https://nvd.nist.gov/vuln/detail/CVE-2020-25710){: external}, [CVE-2020-28196](https://nvd.nist.gov/vuln/detail/CVE-2020-28196){: external}, and [CVE-2020-8694](https://nvd.nist.gov/vuln/detail/CVE-2020-8694){: external}. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.19.3_1526" caption-side="top"}

### Changelog for master fix pack 1.19.4_1527, released 16 November 2020
{: #1194_1527}

The following table shows the changes that are included in the master fix pack patch update `1.19.4_1527`. Master patch updates are applied automatically.
{: shortdesc}

| Component | Previous | Current | Description |
| --- | --- | --- | --- |
| Calico | v3.16.4 | v3.16.5 | See the [Calico release notes](https://docs.projectcalico.org/releases). |
| Cluster health image | v1.2.2 | v1.2.3 | Status codes have been added to add-on health messages. Set add-on health state to `critical` and status to `unknown` when cluster health is `critical`. When a cluster has a Kubernetes key management service (KMS) provider enabled and a disabled [Key Protect](/docs/containers?topic=containers-encryption#keyprotect) key, cluster health state is now set to `critical`. Updated image for [DLA-2424-1](https://www.debian.org/lts/security/2020/dla-2424). |
| CoreDNS | 1.7.1 | 1.8.0 | See the [CoreDNS release notes](https://coredns.io/2020/10/22/coredns-1.8.0-release/). |
| GPU device plug-in and installer | 0c07674 | b966c41 | Updated image for [CVE-2019-20386](https://nvd.nist.gov/vuln/detail/CVE-2019-20386){: external}, [CVE-2019-13050](https://nvd.nist.gov/vuln/detail/CVE-2019-13050){: external}, [CVE-2020-8177](https://nvd.nist.gov/vuln/detail/CVE-2020-8177){: external}, [CVE-2019-14889](https://nvd.nist.gov/vuln/detail/CVE-2019-14889){: external}, [CVE-2020-1730](https://nvd.nist.gov/vuln/detail/CVE-2020-1730){: external}, [CVE-2020-10029](https://nvd.nist.gov/vuln/detail/CVE-2020-10029){: external}, [CVE-2020-1751](https://nvd.nist.gov/vuln/detail/CVE-2020-1751){: external}, [CVE-2020-1752](https://nvd.nist.gov/vuln/detail/CVE-2020-1752){: external}, [CVE-2019-16168](https://nvd.nist.gov/vuln/detail/CVE-2019-16168){: external}, [CVE-2019-20218](https://nvd.nist.gov/vuln/detail/CVE-2019-20218){: external}, [CVE-2019-5018](https://nvd.nist.gov/vuln/detail/CVE-2019-5018){: external}, [CVE-2020-13630](https://nvd.nist.gov/vuln/detail/CVE-2020-13630){: external}, [CVE-2020-13631](https://nvd.nist.gov/vuln/detail/CVE-2020-13631){: external}, [CVE-2020-13632](https://nvd.nist.gov/vuln/detail/CVE-2020-13632){: external}, [CVE-2020-6405](https://nvd.nist.gov/vuln/detail/CVE-2020-6405){: external}, [CVE-2020-9327](https://nvd.nist.gov/vuln/detail/CVE-2020-9327){: external}, [CVE-2019-1551](https://nvd.nist.gov/vuln/detail/CVE-2019-1551){: external}, [CVE-2019-19221](https://nvd.nist.gov/vuln/detail/CVE-2019-19221){: external}, [CVE-2019-16935](https://nvd.nist.gov/vuln/detail/CVE-2019-16935){: external}, [CVE-2019-20907](https://nvd.nist.gov/vuln/detail/CVE-2019-20907){: external}, [CVE-2020-14422](https://nvd.nist.gov/vuln/detail/CVE-2020-14422){: external}, [CVE-2020-8492](https://nvd.nist.gov/vuln/detail/CVE-2020-8492){: external}, [CVE-2019-19906](https://nvd.nist.gov/vuln/detail/CVE-2019-19906){: external}, [CVE-2019-20454](https://nvd.nist.gov/vuln/detail/CVE-2019-20454){: external}, [CVE-2019-19956](https://nvd.nist.gov/vuln/detail/CVE-2019-19956){: external}, [CVE-2019-20388](https://nvd.nist.gov/vuln/detail/CVE-2019-20388){: external}, [CVE-2020-7595](https://nvd.nist.gov/vuln/detail/CVE-2020-7595){: external}, [CVE-2019-13627](https://nvd.nist.gov/vuln/detail/CVE-2019-13627){: external}, [CVE-2018-20843](https://nvd.nist.gov/vuln/detail/CVE-2018-20843){: external}, [CVE-2019-15903](https://nvd.nist.gov/vuln/detail/CVE-2019-15903){: external}, and [CVE-2019-20387](https://nvd.nist.gov/vuln/detail/CVE-2019-20387){: external}. |
| {{site.data.keyword.cloud_notm}} Controller Manager | v1.19.3-3 | v1.19.4-1 | Updated to support the Kubernetes 1.19.4 release. Updated image for [DLA-2424-1](https://www.debian.org/lts/security/2020/dla-2424){: external}. |
| {{site.data.keyword.cloud_notm}} RBAC Operator | 31c794a | 197bc70 | Updated image for [CVE-2019-20454](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-20454){: external}, [CVE-2020-10029](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-10029){: external}, [CVE-2020-1751](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-1751){: external}, [CVE-2020-1752](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-1752){: external}, [CVE-2019-20807](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-20807){: external}, [CVE-2019-16935](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-16935){: external}, [CVE-2019-20907](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-20907){: external}, [CVE-2020-14422](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-14422){: external}, [CVE-2020-8492](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-8492){: external}, [CVE-2019-15165](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-15165){: external}, [CVE-2019-16168](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-16168){: external}, [CVE-2019-20218](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-20218){: external}, [CVE-2019-5018](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5018){: external}, [CVE-2020-13630](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-13630){: external}, [CVE-2020-13631](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-13631){: external}, [CVE-2020-13632](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-13632){: external}, [CVE-2020-6405](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-6405){: external}, [CVE-2020-9327](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-9327){: external}, [CVE-2020-8177](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-8177){: external}, [CVE-2019-13050](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-13050){: external}, [CVE-2018-20843](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-20843){: external}, [CVE-2019-15903](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-15903){: external}, [CVE-2019-14889](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-14889){: external}, [CVE-2020-1730](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-1730){: external}, [CVE-2019-1551](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1551){: external}, [CVE-2020-14382](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-14382){: external}, [CVE-2019-13627](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-13627){: external}, [CVE-2019-19956](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-19956){: external}, [CVE-2019-20388](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-20388){: external}, [CVE-2020-7595](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-7595){: external}, [CVE-2019-20386](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-20386){: external}, [CVE-2019-19906](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-19906){: external}, [CVE-2019-20387](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-20387){: external}, and [CVE-2019-19221](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-19221){: external}.  |
| Key Management Service provider | v2.1.0 | v2.2.1 | Updated to support service to service authentication and to use `Go` version 1.15.2.  Updated image for [DLA-2424-1](https://www.debian.org/lts/security/2020/dla-2424){: external}. |
| Kubernetes | v1.19.3 | v1.19.4 | See the [Kubernetes release notes](https://github.com/kubernetes/kubernetes/releases/tag/v1.19.4){: external}. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.19.3_1525" caption-side="top"}

### Changelog for worker node fix pack 1.19.3_1526, released 9 November 2020
{: #1193_1526}

The following table shows the changes that are included in the worker node fix pack `1.19.3_1526`. Worker node patch updates can be applied by updating, reloading (in classic infrastructure), or replacing (in VPC infrastructure) the worker node.
{: shortdesc}

| Component | Previous | Current | Description |
| --------- | -------- | ------- | ----------- |
|Ubuntu 18.04 packages | N/A | N/A | Updated worker node image with kernal and package updates for [CVE-2018-14036](https://nvd.nist.gov/vuln/detail/CVE-2018-14036){: external}, [CVE-2020-10543](https://nvd.nist.gov/vuln/detail/CVE-2020-10543){: external}, [CVE-2020-10878](https://nvd.nist.gov/vuln/detail/CVE-2020-10878){: external}, [CVE-2020-12723](https://nvd.nist.gov/vuln/detail/CVE-2020-12723){: external}, [CVE-2020-16126](https://nvd.nist.gov/vuln/detail/CVE-2020-16126){: external}, [CVE-2020-25659](https://nvd.nist.gov/vuln/detail/CVE-2020-25659){: external}, and [CVE-2017-18269](https://nvd.nist.gov/vuln/detail/CVE-2017-18269){: external}. |
|Ubuntu 16.04 packages | N/A | N/A | Updated worker node image with package updates for [CVE-2018-14036](https://nvd.nist.gov/vuln/detail/CVE-2018-14036){: external}, [CVE-2020-10543](https://nvd.nist.gov/vuln/detail/CVE-2020-10543){: external}, [CVE-2020-10878](https://nvd.nist.gov/vuln/detail/CVE-2020-10878){: external}, [CVE-2020-12723](https://nvd.nist.gov/vuln/detail/CVE-2020-12723){: external}, [CVE-2020-16126](https://nvd.nist.gov/vuln/detail/CVE-2020-16126){: external}, and [CVE-2020-25659](https://nvd.nist.gov/vuln/detail/CVE-2020-25659){: external}.|
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.19.3_1525" caption-side="top"}

### Changelog for worker node fix pack 1.19.3_1525, released 26 October 2020
{: #1193_1525_worker}

The following table shows the changes that are included in the worker node fix pack `1.19.3_1525`. Worker node patch updates can be applied by updating, reloading (in classic infrastructure), or replacing (in VPC infrastructure) the worker node.
{: shortdesc}

| Component | Previous | Current | Description |
| --------- | -------- | ------- | ----------- |
| Kubernetes | v1.19.2 | v1.19.3 | See the [Kubernetes release notes](https://github.com/kubernetes/kubernetes/releases/tag/v1.19.3){: external}. |
|Ubuntu 18.04 packages | 4.15.0-118-generic | 4.15.0-122-generic | Updated worker node images with kernal and package updates for [CVE-2018-10322](https://nvd.nist.gov/vuln/detail/CVE-2018-10322){: external},[CVE-2019-20807](https://nvd.nist.gov/vuln/detail/CVE-2019-20807){: external}, [CVE-2019-20916](https://nvd.nist.gov/vuln/detail/CVE-2019-20916){: external}, [CVE-2020-12351](https://nvd.nist.gov/vuln/detail/CVE-2020-12351){: external}, [CVE-2020-12352](https://nvd.nist.gov/vuln/detail/CVE-2020-12352){: external}, [CVE-2020-15999](https://nvd.nist.gov/vuln/detail/CVE-2020-15999){: external}, [CVE-2020-16119](https://nvd.nist.gov/vuln/detail/CVE-2020-16119){: external}, [CVE-2020-16120](https://nvd.nist.gov/vuln/detail/CVE-2020-16120){: external}, [CVE-2020-24490](https://nvd.nist.gov/vuln/detail/CVE-2020-24490){: external}, and [CVE-2020-26116](https://nvd.nist.gov/vuln/detail/CVE-2020-26116){: external}. |
|Ubuntu 16.04 packages | 4.4.0-190-generic | 4.4.0-193-generic | Updated worker node images with kernel and package updates for [CVE-2017-17087](https://nvd.nist.gov/vuln/detail/CVE-2017-17087){: external}, [CVE-2018-10322](https://nvd.nist.gov/vuln/detail/CVE-2018-10322){: external}, [CVE-2019-20807](https://nvd.nist.gov/vuln/detail/CVE-2019-20807){: external}, [CVE-2020-15999](https://nvd.nist.gov/vuln/detail/CVE-2020-15999){: external}, [CVE-2020-16119](https://nvd.nist.gov/vuln/detail/CVE-2020-16119){: external}, and [CVE-2020-26116](https://nvd.nist.gov/vuln/detail/CVE-2020-26116){: external}. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.19.2_1524" caption-side="top"}

### Changelog for master fix pack 1.19.3_1525, released 26 October 2020
{: #1193_1525}

The following table shows the changes that are included in the master fix pack patch update `1.19.3_1525`. Master patch updates are applied automatically.
{: shortdesc}

| Component | Previous | Current | Description |
| --- | --- | --- | --- |
| Calico | v3.16.2 | v3.16.4 | See the [Calico release notes](https://docs.projectcalico.org/releases){: external}. |
| Calico configuration | N/A | N/A | Updated the `calico-node` daemon set in the `kube-system` namespace to set the `spec.updateStrategy.rollingUpdate.maxUnavailable` parameter to `10%` for clusters with more than 50 worker nodes. |
| Cluster health image | v1.2.1 | v1.2.2 | Fixed the cluster health status for when add-on customizations are used. |
| {{site.data.keyword.cloud_notm}} Controller Manager | v1.19.2-9 | v1.19.3-3 | Updated to support the Kubernetes 1.19.3 release and to use `Go` version 1.15.2. |
| {{site.data.keyword.filestorage_full_notm}} plug-in and monitor | 378 | 379 | Updated to use the universal base image (UBI) and to use `Go` version 1.15.2. |
| Kubernetes | v1.19.2 | v1.19.3 | See the [Kubernetes release notes](https://github.com/kubernetes/kubernetes/releases/tag/v1.19.3){: external}. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.19.2_1524" caption-side="top"}

### Changelog for 1.19.2_1524, released 13 October 2020
{: #1192_1524}

The following table shows the changes that are included in the `1.19.2_1524` version update.
{: shortdesc}

| Component | Previous | Current | Description |
| --------- | -------- | ------- | ----------- |
| Calico | v3.13.4 | v3.16.2 | See the [Calico release notes](https://docs.projectcalico.org/releases){: external}. In addition, the Calico configuration was updated to use the [Kubernetes API data store driver](https://docs.projectcalico.org/getting-started/kubernetes/hardway/the-calico-datastore){: external}. |
| Cluster health image | v1.1.11 | v1.2.1 | When a cluster has a Kubernetes key management service (KMS) provider enabled and a disabled [{{site.data.keyword.keymanagementserviceshort}}](/docs/containers?topic=containers-encryption#keyprotect) key, a warning is now returned in the cluster health state. In addition, updated to use `Go` version 1.15.2. |
| containerd | 1.3.4 | 1.4.1 | See the [containerd release notes](https://github.com/containerd/containerd/releases/tag/v1.4.1){: external}. |
| CoreDNS | 1.6.9 | 1.7.1 | See the [CoreDNS release notes](https://coredns.io/2020/09/21/coredns-1.7.1-release/){: external}. In addition, the CoreDNS configuration was updated to increase the weight of scheduling CoreDNS pods to different worker nodes and zones. |
| Gateway-enabled cluster controller | 1082 | 1105 | Updated to use `Go` version 1.15.2. |
| {{site.data.keyword.cloud_notm}} Controller Manager | v1.18.9-1 | v1.19.2-9 | Updated to support the Kubernetes 1.19.2 release and to use `Go` version 1.15 and `calicoctl` version 3.16.2. Classic network load balancers (NLBs) now set `NET_RAW` security context. In addition, support was added for VPC network load balancers. |
| {{site.data.keyword.cloud_notm}} RBAC Operator | 4b47693 | 31c794a | Updated to use `Go` version 1.15.2. |
| Key Management Service provider | v2.0.4 | v2.1.0 | Updated to use the key management service (KMS) provider secret to identify when a [{{site.data.keyword.keymanagementserviceshort}}](/docs/containers?topic=containers-encryption#keyprotect) key is enabled and disabled so that encryption and decryption requests are updated accordingly. |
| Kubernetes | v1.18.9 | v1.19.2 | See the [Kubernetes release notes](https://github.com/kubernetes/kubernetes/releases/tag/v1.19.2){: external}. |
| Kubernetes configuration | N/A | N/A | Disabled the Kubernetes `SCTPSupport` and `ServiceAppProtocol` feature gates. |
| Kubernetes DNS autoscaler | 1.7.1 | 1.8.3 | See the [Kubernetes DNS autoscaler release notes](https://github.com/kubernetes-sigs/cluster-proportional-autoscaler/releases/tag/1.8.3){: external}. In addition, the Kubernetes DNS autoscaler configuration was updated to include unscheduable worker nodes in scaling calculations. |
| Kubernetes NodeLocal DNS cache | 1.15.13 | 1.15.14 | See the [Kubernetes NodeLocal DNS cache release notes](https://github.com/kubernetes/dns/releases/tag/1.15.14){: external}. |
| Load balancer and load balancer monitor for {{site.data.keyword.cloud_notm}} Provider | 223 | 234 | Updated to use `Go` version 1.15.2. |
| Operator Lifecycle Manager Catalog | v1.6.1 | v1.14.0 | See the [Operator Lifecycle Manager Catalog release notes](https://github.com/operator-framework/operator-registry/releases/tag/v1.14.0){: external}. |
| Operator Lifecycle Manager | 0.14.1-IKS-1 | 0.16.1 | See the [Operator Lifecycle Manager release notes](https://github.com/operator-framework/operator-lifecycle-manager/releases/tag/0.16.1){: external}. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.18.9_1528" caption-side="top"}

## Version 1.18 changelog
{: #118_changelog}

Review the version 1.18 changelog.
{: shortdesc}

### Changelog for worker node fix pack 1.18.17_1548, released 12 April 2021
{: #11817_1548}

The following table shows the changes that are included in the worker node fix pack `1.18.17_1548`. Worker node patch updates can be applied by updating, reloading (in classic infrastructure), or replacing (in VPC infrastructure) the worker node.
{: shortdesc}

| Component | Previous | Current | Description |
| --------- | -------- | ------- | ----------- |
| HA proxy | 9b2dca | a3b1ff | The update addresses [CVE-2021-3449](https://nvd.nist.gov/vuln/detail/CVE-2021-3449){: external} and [CVE-2021-3450](https://nvd.nist.gov/vuln/detail/CVE-2021-3450){: external}. |
| Ubuntu 18.04 packages | N/A | N/A | Updated worker node images with package updates for [CVE-2021-22876](https://nvd.nist.gov/vuln/detail/CVE-2021-22876){: external}. |
| Ubuntu 16.04 packages | N/A | N/A | Updated worker node images with package updates for [CVE-2021-22876](https://nvd.nist.gov/vuln/detail/CVE-2021-22876){: external}.|
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.18.17_1547" caption-side="top"}

### Changelog for master fix pack 1.18.17_1546, released 30 March 2021
{: #11817_1546}

The following table shows the changes that are included in the master fix pack patch update `1.18.17_1546`. Master patch updates are applied automatically.
{: shortdesc}

| Component | Previous | Current | Description |
| --- | --- | --- | --- |
| {{site.data.keyword.at_short}} event | N/A | N/A | Now, the `containers-kubernetes.version.update` event is sent to {{site.data.keyword.at_short}} when a master fix pack update is initiated for a cluster. |
| Cluster health image | v1.1.18 | v1.1.19 | Updated image for [CVE-2020-28851](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-28851){: external}. |
| Gateway-enabled cluster controller | 1232 | 1322 | Updated to use `Go` version 1.15.10 and for [CVE-2021-23839](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-23839){: external}, [CVE-2021-23840](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-23840){: external}, and [CVE-2021-23841](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-23841){: external}. |
| GPU device plug-in and installer | af5a6cb | c1c6dd3 | Updated image for [CVE-2021-27919](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-27919){: external}, [CVE-2020-28851](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-28851){: external}, and [CVE-2020-28852](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-28852){: external}. |
| {{site.data.keyword.cloud_notm}} Controller Manager | v1.18.16-2 | v1.18.17-1 | Updated to support the Kubernetes 1.18.17 release. Fixed a bug that prevented VPC load balancers from supporting more than 50 subnets in an account. |
| {{site.data.keyword.filestorage_full_notm}} plug-in and monitor | 388 | 389 | Updated to use `Go` version 1.15.8. |
| {{site.data.keyword.cloud_notm}} RBAC Operator | 86de2b7 | 3dd6bbc | Updated image for [CVE-2020-28851](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-28851){: external}. |
| Kubernetes | v1.18.16 | v1.18.17 | See the [Kubernetes release notes](https://github.com/kubernetes/kubernetes/releases/tag/v1.18.17){: external}. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.18.16_1544" caption-side="top"}

### Changelog for worker node fix pack 1.18.17_1547, released 29 March 2021
{: #11817_1547}

The following table shows the changes that are included in the worker node fix pack `1.18.17_1547`. Worker node patch updates can be applied by updating, reloading (in classic infrastructure), or replacing (in VPC infrastructure) the worker node.
{: shortdesc}

| Component | Previous | Current | Description |
| --------- | -------- | ------- | ----------- |
| Ubuntu 18.04 packages | 4.15.0-136-generic | 4.15.0-140-generic | Updated worker node images with kernel and package updates for [CVE-2020-27170](https://nvd.nist.gov/vuln/detail/CVE-2020-27170){: external}, [CVE-2020-27171](https://nvd.nist.gov/vuln/detail/CVE-2020-27171){: external}, [CVE-2021-27363](https://nvd.nist.gov/vuln/detail/CVE-2021-27363){: external}, [CVE-2021-27365](https://nvd.nist.gov/vuln/detail/CVE-2021-27365){: external}, [CVE-2021-28153](https://nvd.nist.gov/vuln/detail/CVE-2021-28153){: external}, and [CVE-2021-3449](https://nvd.nist.gov/vuln/detail/CVE-2021-3449){: external}. |
| Ubuntu 16.04 packages | 4.4.0-203-generic | 4.4.0-206-generic | Updated worker node images with kernel and package updates for [CVE-2021-27363](https://nvd.nist.gov/vuln/detail/CVE-2021-27363){: external}, [CVE-2021-27365](https://nvd.nist.gov/vuln/detail/CVE-2021-27365){: external}, and [CVE-2021-28153](https://nvd.nist.gov/vuln/detail/CVE-2021-28153){: external}.|
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.18.16_1545" caption-side="top"}

### Changelog for worker node fix pack 1.18.16_1545, released 12 March 2021
{: #11816_1545}

The following table shows the changes that are included in the worker node fix pack `1.18.16_1545`. Worker node patch updates can be applied by updating, reloading (in classic infrastructure), or replacing (in VPC infrastructure) the worker node.
{: shortdesc}

| Component | Previous | Current | Description |
| --------- | -------- | ------- | ----------- |
| Containerd | v1.3.9 | 1.3.10 | See the [containerd release notes](https://github.com/containerd/containerd/releases/tag/v1.3.10){: external}. The update resolves CVE-2021-21334 (see the [IBM security bulletin](https://www.ibm.com/support/pages/node/6433475){: external}). |
| Ubuntu 18.04 packages | N/A | N/A | Updated worker node image with package updates for [CVE-2021-21300](https://nvd.nist.gov/vuln/detail/CVE-2021-21300){: external}, [CVE-2021-24031](https://nvd.nist.gov/vuln/detail/CVE-2021-24031){: external}, [CVE-2021-24032](https://nvd.nist.gov/vuln/detail/CVE-2021-24032){: external}, [CVE-2021-27218](https://nvd.nist.gov/vuln/detail/CVE-2021-27218){: external}, and [CVE-2021-27219](https://nvd.nist.gov/vuln/detail/CVE-2021-27219){: external}. |
| Ubuntu 16.04 packages | N/A | N/A | Updated worker node image with package updates for{: external}, [CVE-2021-21300](https://nvd.nist.gov/vuln/detail/CVE-2021-21300){: external}, [CVE-2021-27218](https://nvd.nist.gov/vuln/detail/CVE-2021-27218){: external}, [CVE-2021-27219](https://nvd.nist.gov/vuln/detail/CVE-2021-27219){: external}, and [CVE-2021-3177](https://nvd.nist.gov/vuln/detail/CVE-2021-3177){: external}. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.18.16_1544" caption-side="top"}

### Changelog for worker node fix pack 1.18.16_1544, released 1 March 2021
{: #11816_1544}

The following table shows the changes that are included in the worker node fix pack `1.18.16_1544`. Worker node patch updates can be applied by updating, reloading (in classic infrastructure), or replacing (in VPC infrastructure) the worker node.
{: shortdesc}

| Component | Previous | Current | Description |
| --------- | -------- | ------- | ----------- |
| Kubernetes | v1.18.15 | v1.18.16 | See the [Kubernetes release notes](https://github.com/kubernetes/kubernetes/releases/tag/v1.18.16){: external}. |
| Ubuntu 18.04 packages | 4.15.0-135-generic | 4.15.0-136-generic | Updated worker node image with package updates for [CVE-2020-27619](https://nvd.nist.gov/vuln/detail/CVE-2020-27619){: external}, [CVE-2020-29372](https://nvd.nist.gov/vuln/detail/CVE-2020-29372){: external}, [CVE-2020-29374](https://nvd.nist.gov/vuln/detail/CVE-2020-29374){: external}, [CVE-2020-8625](https://nvd.nist.gov/vuln/detail/CVE-2020-8625){: external}, [CVE-2021-23840](https://nvd.nist.gov/vuln/detail/CVE-2021-23840){: external}, [CVE-2021-23841](https://nvd.nist.gov/vuln/detail/CVE-2021-23841){: external}, [CVE-2021-26937](https://nvd.nist.gov/vuln/detail/CVE-2021-26937){: external}, [CVE-2021-27212](https://nvd.nist.gov/vuln/detail/CVE-2021-27212){: external}, and [CVE-2021-3177](https://nvd.nist.gov/vuln/detail/CVE-2021-3177){: external}. |
| Ubuntu 16.04 packages | 4.4.0-201-generic | 4.4.0-203-generic | Updated worker node image with package updates for [CVE-2020-27619](https://nvd.nist.gov/vuln/detail/CVE-2020-27619){: external}, [CVE-2020-29372](https://nvd.nist.gov/vuln/detail/CVE-2020-29372){: external}, [CVE-2020-29374](https://nvd.nist.gov/vuln/detail/CVE-2020-29374){: external}, [CVE-2020-8625](https://nvd.nist.gov/vuln/detail/CVE-2020-8625){: external}, [CVE-2021-23840](https://nvd.nist.gov/vuln/detail/CVE-2021-23840){: external}, [CVE-2021-23841](https://nvd.nist.gov/vuln/detail/CVE-2021-23841){: external}, [CVE-2021-26937](https://nvd.nist.gov/vuln/detail/CVE-2021-26937){: external}, [CVE-2021-27212](https://nvd.nist.gov/vuln/detail/CVE-2021-27212){: external}, and [CVE-2021-3177](https://nvd.nist.gov/vuln/detail/CVE-2021-3177){: external}. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.18.15_1541" caption-side="top"}

### Changelog for master fix pack 1.18.16_1544, released 27 February 2021
{: #11816_1544_master}

The following table shows the changes that are included in the master fix pack patch update `1.18.16_1543`. Master patch updates are applied automatically.
{: shortdesc}

| Component | Previous | Current | Description |
| --- | --- | --- | --- |
| Load balancer and load balancer monitor for {{site.data.keyword.cloud_notm}} Provider | 1165 | 1274 | Fixed a bug that might cause version 2.0 network load balancers (NLBs) to crash and restart on load balancer updates. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.18.16_1543" caption-side="top"}

### Changelog for master fix pack 1.18.16_1543, released 22 February 2021
{: #11816_1543}

The following table shows the changes that are included in the master fix pack patch update `1.18.16_1543`. Master patch updates are applied automatically.
{: shortdesc}

| Component | Previous | Current | Description |
| --- | --- | --- | --- |
| Calico | v3.13.4 | v3.13.5 | See the [Calico release notes](https://docs.projectcalico.org/releases){: external}. |
| Cluster health image | v1.1.16 | v1.1.18 | Updated to use `Go` version 1.15.7. Updated image to implement additional IBM security controls. |
| Gateway-enabled cluster controller | 1195 | 1232 | Updated to use `Go` version 1.15.7. |
| IBM Calico extension | 567 | 618 | Updated to use `Go` version 1.15.7. |
| {{site.data.keyword.cloud_notm}} Controller Manager | v1.18.15-3 | v1.18.16-2 | Updated to support the Kubernetes 1.18.16 release and to use `calicoctl` version 3.13.5. Updated image to implement additional IBM security controls and for [DLA-2509-1](https://www.debian.org/lts/security/2020/dla-2509){: external}. Updated version 1.0 and 2.0 network load balancers (NLBs) to run as a non-root user by default, with privileged escalation as needed. |
| {{site.data.keyword.filestorage_full_notm}} plug-in and monitor | 385 | 388 | Improved the retry logic for provisioning persistent volume claims (PVCs). |
| {{site.data.keyword.cloud_notm}} RBAC Operator | f859228 | 86de2b7 | Updated to use `Go` version 1.15.7. |
| Key Management Service provider | v2.2.3 | v2.2.5 | Updated to use `Go` version 1.15.7. Updated image to implement additional IBM security controls. |
| Kubernetes | v1.18.15 | v1.18.16 | See the [Kubernetes release notes](https://github.com/kubernetes/kubernetes/releases/tag/v1.18.16){: external}. |
| Load balancer and load balancer monitor for {{site.data.keyword.cloud_notm}} Provider | 1078 | 1165 | Updated to run as a non-root user by default, with privileged escalation as needed. Updated to use `Go` version 1.15.7. |
| Operator Lifecycle Manager | 0.14.1-IKS-2 | 0.14.1-IKS-4 | Updated image for [CVE-2021-23839](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-23839){: external}, [CVE-2021-23840](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-23840){: external}, and [CVE-2021-23841](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-23841){: external}. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.18.15_1538" caption-side="top"}

### Changelog for worker node fix pack 1.18.15_1541, released 15 February 2021
{: #11815_1541}

The following table shows the changes that are included in the worker node fix pack `1.18.15_1541`. Worker node patch updates can be applied by updating, reloading (in classic infrastructure), or replacing (in VPC infrastructure) the worker node.
{: shortdesc}

| Component | Previous | Current | Description |
| --------- | -------- | ------- | ----------- |
| Ubuntu 18.04 packages | N/A | N/A | Updated worker node image with package updates for [CVE-2020-36221](https://nvd.nist.gov/vuln/detail/CVE-2020-36221){: external}, [CVE-2020-36222](https://nvd.nist.gov/vuln/detail/CVE-2020-36222){: external}, [CVE-2020-36223](https://nvd.nist.gov/vuln/detail/CVE-2020-36223){: external}, [CVE-2020-36224](https://nvd.nist.gov/vuln/detail/CVE-2020-36224){: external}, [CVE-2020-36225](https://nvd.nist.gov/vuln/detail/CVE-2020-36225){: external}, [CVE-2020-36226](https://nvd.nist.gov/vuln/detail/CVE-2020-36226){: external}, [CVE-2020-36227](https://nvd.nist.gov/vuln/detail/CVE-2020-36227){: external}, [CVE-2020-36228](https://nvd.nist.gov/vuln/detail/CVE-2020-36228){: external}, [CVE-2020-36229](https://nvd.nist.gov/vuln/detail/CVE-2020-36229){: external}, [CVE-2020-36230](https://nvd.nist.gov/vuln/detail/CVE-2020-36230){: external}, [CVE-2021-25682](https://nvd.nist.gov/vuln/detail/CVE-2021-25682){: external}, [CVE-2021-25683](https://nvd.nist.gov/vuln/detail/CVE-2021-25683){: external}, and [CVE-2021-25684](https://nvd.nist.gov/vuln/detail/CVE-2021-25684){: external}. |
| Ubuntu 16.04 packages | N/A | N/A | Updated worker node image with package updates for [CVE-2020-36221](https://nvd.nist.gov/vuln/detail/CVE-2020-36221){: external}, [CVE-2020-36222](https://nvd.nist.gov/vuln/detail/CVE-2020-36222){: external}, [CVE-2020-36223](https://nvd.nist.gov/vuln/detail/CVE-2020-36223){: external}, [CVE-2020-36224](https://nvd.nist.gov/vuln/detail/CVE-2020-36224){: external}, [CVE-2020-36225](https://nvd.nist.gov/vuln/detail/CVE-2020-36225){: external}, [CVE-2020-36226](https://nvd.nist.gov/vuln/detail/CVE-2020-36226){: external}, [CVE-2020-36227](https://nvd.nist.gov/vuln/detail/CVE-2020-36227){: external}, [CVE-2020-36228](https://nvd.nist.gov/vuln/detail/CVE-2020-36228){: external}, [CVE-2020-36229](https://nvd.nist.gov/vuln/detail/CVE-2020-36229){: external}, [CVE-2020-36230](https://nvd.nist.gov/vuln/detail/CVE-2020-36230){: external}, [CVE-2021-25682](https://nvd.nist.gov/vuln/detail/CVE-2021-25682){: external}, [CVE-2021-25683](https://nvd.nist.gov/vuln/detail/CVE-2021-25683){: external}, and [CVE-2021-25684](https://nvd.nist.gov/vuln/detail/CVE-2021-25684){: external}. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.18.15_1540" caption-side="top"}

### Changelog for worker node fix pack 1.18.15_1540, released 3 February 2021
{: #11815_1540}

The following table shows the changes that are included in the worker node fix pack `1.18.15_1540`. Worker node patch updates can be applied by updating, reloading (in classic infrastructure), or replacing (in VPC infrastructure) the worker node.
{: shortdesc}

| Component | Previous | Current | Description |
| --------- | -------- | ------- | ----------- |
| Metadata updates | N/A | N/A | Updated the worker node version fix pack metadata for internal documentation purposes. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.18.15_1539" caption-side="top"}

### Changelog for worker node fix pack 1.18.15_1539, released 1 February 2021
{: #11815_1539}

The following table shows the changes that are included in the worker node fix pack `1.18.15_1539`. Worker node patch updates can be applied by updating, reloading (in classic infrastructure), or replacing (in VPC infrastructure) the worker node.
{: shortdesc}

| Component | Previous | Current | Description |
| --------- | -------- | ------- | ----------- |
| Ubuntu 16.04 packages | 4.4.0-200-generic | 4.4.0-201-generic | Updated worker node image with kernel and package updates for [CVE-2019-14834](https://nvd.nist.gov/vuln/detail/CVE-2019-14834){: external}, [CVE-2020-25681](https://nvd.nist.gov/vuln/detail/CVE-2020-25681){: external}, [CVE-2020-25682](https://nvd.nist.gov/vuln/detail/CVE-2020-25682){: external}, [CVE-2020-25683](https://nvd.nist.gov/vuln/detail/CVE-2020-25683){: external}, [CVE-2020-25684](https://nvd.nist.gov/vuln/detail/CVE-2020-25684){: external}, [CVE-2020-25685](https://nvd.nist.gov/vuln/detail/CVE-2020-25685){: external}, [CVE-2020-25686](https://nvd.nist.gov/vuln/detail/CVE-2020-25686){: external}, [CVE-2020-25687](https://nvd.nist.gov/vuln/detail/CVE-2020-25687){: external}, [CVE-2020-27777](https://nvd.nist.gov/vuln/detail/CVE-2020-27777){: external}, [CVE-2021-23239](https://nvd.nist.gov/vuln/detail/CVE-2021-23239){: external}, and [CVE-2021-3156](https://nvd.nist.gov/vuln/detail/CVE-2021-3156){: external}. |
| Ubuntu 18.04 packages | 4.15.0-132-generic | 4.15.0-135-generic | Updated worker node image with kernel and package updates for [CVE-2019-12761](https://nvd.nist.gov/vuln/detail/CVE-2019-12761){: external}, [CVE-2019-14834](https://nvd.nist.gov/vuln/detail/CVE-2019-14834){: external}, [CVE-2020-25681](https://nvd.nist.gov/vuln/detail/CVE-2020-25681){: external}, [CVE-2020-25682](https://nvd.nist.gov/vuln/detail/CVE-2020-25682){: external}, [CVE-2020-25683](https://nvd.nist.gov/vuln/detail/CVE-2020-25683){: external}, [CVE-2020-25684](https://nvd.nist.gov/vuln/detail/CVE-2020-25684){: external}, [CVE-2020-25685](https://nvd.nist.gov/vuln/detail/CVE-2020-25685){: external}, [CVE-2020-25686](https://nvd.nist.gov/vuln/detail/CVE-2020-25686){: external}, [CVE-2020-25687](https://nvd.nist.gov/vuln/detail/CVE-2020-25687){: external}, [CVE-2020-27777](https://nvd.nist.gov/vuln/detail/CVE-2020-27777){: external}, [CVE-2021-23239](https://nvd.nist.gov/vuln/detail/CVE-2021-23239){: external}, and [CVE-2021-3156](https://nvd.nist.gov/vuln/detail/CVE-2021-3156){: external}. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.18.15_1538" caption-side="top"}

### Changelog for master fix pack 1.18.15_1538, released 19 January 2021
{: #11815_1538_master}

The following table shows the changes that are included in the master fix pack patch update `1.18.15_1538`. Master patch updates are applied automatically.
{: shortdesc}

| Component | Previous | Current | Description |
| --- | --- | --- | --- |
| Cluster health image | v1.1.14 | v1.1.16 | Updated image to implement additional IBM security controls. |
| Gateway-enabled cluster controller | 1184 | 1195 | Updated image for [CVE-2020-1971](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-1971){: external}. |
| GPU device plug-in and installer | c26e2ae | af5a6cb | Updated image for [CVE-2021-3114](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-3114){: external}, [CVE-2021-3115](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-3115){: external}, [CVE-2020-27350](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-27350){: external}, [CVE-2020-29361](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-29361){: external}, [CVE-2020-29362](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-29362){: external}, [CVE-2020-29363](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-29363){: external}, [CVE-2020-1971](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-1971){: external}, [CVE-2020-8231](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-8231){: external}, [CVE-2020-8284](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-8284){: external}, [CVE-2020-8285](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-8285){: external}, and [CVE-2020-8286](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-8286){: external}. |
| IBM Calico extension | 556 | 567 | Updated image for [CVE-2020-1971](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-1971){: external} and [CVE-2020-24659](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-24659){: external}. |
| {{site.data.keyword.cloud_notm}} Controller Manager | v1.18.14-1 | v1.18.15-3 | Updated to support the Kubernetes 1.18.5 release and to implement additional IBM security controls. |
| {{site.data.keyword.filestorage_full_notm}} plug-in and monitor | 384 | 385 | Updated image for [CVE-2020-1971](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-1971){: external} and [CVE-2020-24659](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-24659){: external}. |
| Key Management Service provider | v2.2.2 | v2.2.3 | Fixed bug to ignore conflict errors during KMS secret reencrpytion. Updated to use `Go` version 1.15.5. Updated image to implement additional IBM security controls and for [CVE-2020-1971](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-1971){: external}. |
| Kubernetes | v1.18.14 | v1.18.15 | See the [Kubernetes release notes](https://github.com/kubernetes/kubernetes/releases/tag/v1.18.15){: external}. Updated to implement additional IBM security controls. |
| Kubernetes Dashboard | v2.0.4 | v2.0.5 | See the [Kubernetes Dashboard release notes](https://github.com/kubernetes/dashboard/releases/tag/v2.0.5){: external}. |
| Kubernetes Dashboard metrics scraper | v1.0.4 | v1.0.6 | See the [Kubernetes Dashboard metrics scraper release notes](https://github.com/kubernetes-sigs/dashboard-metrics-scraper/releases/tag/v1.0.6){: external}. |
| Load balancer and load balancer monitor for {{site.data.keyword.cloud_notm}} Provider | 1004 | 1078 | Updated image for [CVE-2020-1971](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-1971){: external}. |
| Operator Lifecycle Manager Catalog | v1.6.1 | v1.15.3 | See the [Operator Lifecycle Manager Catalog release notes](https://github.com/operator-framework/operator-registry/releases/tag/v1.15.3){: external}. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.18.14_1537" caption-side="top"}

### Changelog for worker node fix pack 1.18.15_1538, released 18 January 2021
{: #11815_1538}

The following table shows the changes that are included in the worker node fix pack `1.18.15_1538`. Worker node patch updates can be applied by updating, reloading (in classic infrastructure), or replacing (in VPC infrastructure) the worker node.
{: shortdesc}

| Component | Previous | Current | Description |
| --------- | -------- | ------- | ----------- |
| Kubernetes | v1.18.13 | v1.18.15 | See the [Kubernetes release notes](https://github.com/kubernetes/kubernetes/releases/tag/v1.18.15){: external}. |
| Ubuntu 16.04 packages | 4.4.0-197-generic | 4.4.0-200-generic | Updated worker node image with kernel and package updates for [CVE-2018-20482](https://nvd.nist.gov/vuln/detail/CVE-2018-20482){: external}, [CVE-2019-9923](https://nvd.nist.gov/vuln/detail/CVE-2019-9923){: external}, [CVE-2020-28374](https://nvd.nist.gov/vuln/detail/CVE-2020-28374){: external}, [CVE-2020-29361](https://nvd.nist.gov/vuln/detail/CVE-2020-29361) external}, and [CVE-2020-29362](https://nvd.nist.gov/vuln/detail/CVE-2020-29362) external}. |
| Ubuntu 18.04 packages | 4.15.0-128-generic | 4.15.0-132-generic | Updated worker node image with kernel and package updates for{: external}, [CVE-2018-20482](https://nvd.nist.gov/vuln/detail/CVE-2018-20482){: external}, [CVE-2019-9923](https://nvd.nist.gov/vuln/detail/CVE-2019-9923){: external}, [CVE-2020-28374](https://nvd.nist.gov/vuln/detail/CVE-2020-28374){: external}, [CVE-2020-29361](https://nvd.nist.gov/vuln/detail/CVE-2020-29361){: external}, [CVE-2020-29362](https://nvd.nist.gov/vuln/detail/CVE-2020-29362){: external}, [CVE-2020-29363](https://nvd.nist.gov/vuln/detail/CVE-2020-29363){: external}, [CVE-2021-1052](https://nvd.nist.gov/vuln/detail/CVE-2021-1052){: external}, [CVE-2021-1053](https://nvd.nist.gov/vuln/detail/CVE-2021-1053){: external}, and [CVE-2019-19770](https://nvd.nist.gov/vuln/detail/CVE-2019-19770){: external}. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.18.13_1536" caption-side="top"}

### Changelog for master fix pack 1.18.14_1537, released 6 January 2021
{: #11814_1537}

The following table shows the changes that are included in the master fix pack patch update `1.18.14_1537`. Master patch updates are applied automatically.
{: shortdesc}

| Component | Previous | Current | Description |
| --- | --- | --- | --- |
| IBM Calico extension | 544 | 556 | Updated image to include the `ip` command. |
| {{site.data.keyword.cloud_notm}} Controller Manager | v1.18.13-1 | v1.18.14-1 | Updated to support the Kubernetes 1.18.14 release. |
| {{site.data.keyword.filestorage_full_notm}} plug-in | N/A | N/A | Updated to run with a privileged security context. |
| {{site.data.keyword.cloud_notm}} RBAC Operator | c148a8a | f859228 | Updated image for [CVE-2020-1971](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-1971){: external} and [CVE-2020-24659](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-24659){: external}. |
| Key Management Service provider | v2.0.7 | v2.2.2 | Updated the key management service (KMS) provider support as follows.<ul><li>Updated to use `Go` version 1.15.2.</li><li>Added support for [service-to-service authentication](/docs/account?topic=account-serviceauth).</li><li>Updated to use the KMS provider secret to identify when a [Key Protect](/docs/containers?topic=containers-encryption#keyprotect) key is enabled and disabled so that encryption and decryption requests are updated accordingly.</li></ul> |
| Kubernetes | v1.18.13 | v1.18.14 | See the [Kubernetes release notes](https://github.com/kubernetes/kubernetes/releases/tag/v1.18.14){: external}. |
| Kubernetes `NodeLocal` DNS cache | N/A | N/A | Updated to run with a least privileged security context. |
| Operator Lifecycle Manager | 0.14.1-IKS-1 | 0.14.1-IKS-2 | Updated image for [CVE-2020-1971](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-1971){: external} and [CVE-2020-28928](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-28928){: external}. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.18.13_1535" caption-side="top"}

### Changelog for worker node fix pack 1.18.13_1536, released 21 December 2020
{: #11813_1536}

The following table shows the changes that are included in the worker node fix pack `1.18.13_1536`. Worker node patch updates can be applied by updating, reloading (in classic infrastructure), or replacing (in VPC infrastructure) the worker node.
{: shortdesc}

| Component | Previous | Current | Description |
| --------- | -------- | ------- | ----------- |
| Ubuntu 16.04 packages | N/A | N/A | Updated worker node image with package updates for: [CVE-2020-1971](https://nvd.nist.gov/vuln/detail/CVE-2020-1971){: external}, [CVE-2020-27350](https://nvd.nist.gov/vuln/detail/CVE-2020-27350){: external}, [CVE-2020-27351](https://nvd.nist.gov/vuln/detail/CVE-2020-27351){: external}, [CVE-2020-8284](https://nvd.nist.gov/vuln/detail/CVE-2020-8284){: external}, [CVE-2020-8285](https://nvd.nist.gov/vuln/detail/CVE-2020-8285){: external}, and [CVE-2020-8286](https://nvd.nist.gov/vuln/detail/CVE-2020-8286){: external}. |
| Ubuntu 18.04 packages | 4.15.0-126-generic | 4.15.0-128-generic | Updated worker node image with kernel and package updates for: [CVE-2020-1971](https://nvd.nist.gov/vuln/detail/CVE-2020-1971){: external}, [CVE-2020-27350](https://nvd.nist.gov/vuln/detail/CVE-2020-27350){: external}, [CVE-2020-27351](https://nvd.nist.gov/vuln/detail/CVE-2020-27351){: external}, [CVE-2020-8284](https://nvd.nist.gov/vuln/detail/CVE-2020-8284){: external}, [CVE-2020-8285](https://nvd.nist.gov/vuln/detail/CVE-2020-8285){: external}, and [CVE-2020-8286](https://nvd.nist.gov/vuln/detail/CVE-2020-8286){: external}. |
| Kubernetes | v1.18.12 | v1.18.13 | See the [Kubernetes changelogs.](https://github.com/kubernetes/kubernetes/releases/tag/v1.18.13){: external} |
| Ephemeral storage reservations | N/A | N/A | Reserve local ephermal storage to prevent workload evictions. |
| HAProxy | db4e6d | 9b2dca | Image update for [CVE-2020-1971](https://nvd.nist.gov/vuln/detail/CVE-2020-1971){: external} and [CVE-2020-24659](https://nvd.nist.gov/vuln/detail/CVE-2020-24659){: external}. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.18.13_1535" caption-side="top"}

### Changelog for master fix pack 1.18.13_1535, released 14 December 2020
{: #11813_1535}

The following table shows the changes that are included in the master fix pack patch update `1.18.13_1535`. Master patch updates are applied automatically.
{: shortdesc}

| Component | Previous | Current | Description |
| --- | --- | --- | --- |
| Cluster health image | v1.1.13 | v1.1.14 | Updated image to implement additional IBM security controls. |
| CoreDNS | 1.6.9 | 1.8.0 | See the [CoreDNS release notes](https://coredns.io/2020/10/22/coredns-1.8.0-release/){: external}. Additionally, updated the CoreDNS configuration to increase the weight of scheduling CoreDNS pods to different worker nodes and zones. |
| etcd | v3.4.13 | v3.4.14 | See the [etcd release notes](https://github.com/etcd-io/etcd/releases/v3.4.14){: external}. |
| Gateway-enabled cluster controller | 1105 | 1184 | Updated to use `Go` version 1.15.5. Updated image to implement additional IBM security controls. |
| GPU device plug-in and installer | b966c41 | c26e2ae | Updated the GPU drivers to version [450.80.02](https://www.nvidia.com/download/driverResults.aspx/165294){: external}. Updated image for [CVE-2020-28367](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-28367){: external}, [CVE-2020-28366](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-28366){: external}, and [CVE-2020-28362](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-28362){: external}. Updated image to implement additional IBM security controls. |
| IBM Calico extension | 378 | 544 | Updated to use the universal base image (UBI) and to use `Go` version 1.15.5. Updated image to implement additional IBM security controls. |
| {{site.data.keyword.cloud_notm}} Controller Manager | v1.18.12-1 | v1.18.13-1 | Updated to support the Kubernetes 1.18.13 release. Updated image to implement additional IBM security controls. Fixed a bug in VPC load balancer creation when the cluster, VPC, or subnet are in a different resource group.  |
| {{site.data.keyword.filestorage_full_notm}} monitor | 379 | 384 | Updated to use `Go` version 1.15.5 and to run as a non-root user. Updated image to implement additional IBM security controls. |
| {{site.data.keyword.filestorage_full_notm}} plug-in | 379 | 384 | Updated to use `Go` version 1.15.5 and to run with a least privileged security context. Updated image to implement additional IBM security controls. |
| {{site.data.keyword.cloud_notm}} RBAC Operator | 197bc70 | c148a8a | Updated to use `Go` version 1.15.6 and updated image to implement additional IBM security controls. |
| Key management service (KMS) provider | v2.0.5 | v2.0.7 | Updated image to implement additional IBM security controls. |
| Kubernetes | v1.18.12 | v1.18.13 | See the [Kubernetes release notes](https://github.com/kubernetes/kubernetes/releases/tag/v1.18.13){: external}. |
| Kubernetes `NodeLocal` DNS cache | N/A | N/A | Updated the `NodeLocal` DNS cache configuration to support [customizing the `node-local-dns` config map](/docs/containers?topic=containers-cluster_dns#dns_nodelocal_customize). |
| Load balancer and load balancer monitor for {{site.data.keyword.cloud_notm}} Provider | 208 | 1004 | Updated Alpine base image to version 3.12 and to use `Go` version 1.15.5. Updated image for [CVE-2020-8037](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-8037){: external} and [CVE-2020-28928](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-28928){: external}. Updated image to implement additional IBM security controls. |
| Operator Lifecycle Manager | N/A | N/A | Updated to run as a non-root user. |
| OpenVPN client | 2.4.6-r3-IKS-116 | 2.4.6-r3-IKS-301 | Updated image to implement additional IBM security controls. |
| OpenVPN server | 2.4.6-r3-IKS-222 | 2.4.6-r3-IKS-301 | Updated image to implement additional IBM security controls. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.18.12_1533" caption-side="top"}

### Changelog for worker node fix pack 1.18.12_1535, released 11 December 2020
{: #11812_1535}

The following table shows the changes that are included in the worker node fix pack `1.18.12_1535`. Worker node patch updates can be applied by updating, reloading (in classic infrastructure), or replacing (in VPC infrastructure) the worker node.
{: shortdesc}

| Component | Previous | Current | Description |
| --------- | -------- | ------- | ----------- |
| Ubuntu 18.04 bare metal kernel | 4.15.0-126-generic | 4.15.0-123-generic | **Bare metal worker nodes**: Reverted the kernel version for bare metal worker nodes while Canonical addresses issues with the previous version that prevented worker nodes from being reloaded or updated. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.18.12_1534" caption-side="top"}

### Changelog for worker node fix pack 1.18.12_1534, released 7 December 2020
{: #11812_1534}

The following table shows the changes that are included in the worker node fix pack `1.18.12_1534`. Worker node patch updates can be applied by updating, reloading (in classic infrastructure), or replacing (in VPC infrastructure) the worker node.
{: shortdesc}

| Component | Previous | Current | Description |
| --------- | -------- | ------- | ----------- |
| Containerd | v1.3.4 | 1.3.9 | See the [containerd release notes](https://github.com/containerd/containerd/releases/tag/v1.3.9){: external}. The update resolves CVE-2020–15257 (see the [IBM security bulletin](https://www.ibm.com/support/pages/node/6387878){: external}). |
| HAProxy | 1.8.26-384f42 | db4e6d | Added provenance labels for source tracking. |
| Ubuntu 18.04 packages | 4.15.0-123-generic | 4.15.0-126-generic | Updated worker node image with   kernel and package updates for [CVE-2020-14351](https://nvd.nist.gov/vuln/detail/CVE-2020-14351){: external} and [CVE-2020-4788](https://nvd.nist.gov/vuln/detail/CVE-2020-4788){: external}. |
| Ubuntu 16.04 packages | 4.4.0-194-generic | 4.4.0-197-generic | Updated worker node image with kernel and package updates for [CVE-2020-0427](https://nvd.nist.gov/vuln/detail/CVE-2020-0427){: external}, [CVE-2020-12352](https://nvd.nist.gov/vuln/detail/CVE-2020-12352){: external}, [CVE-2020-14351](https://nvd.nist.gov/vuln/detail/CVE-2020-14351){: external}, [CVE-2020-25645](https://nvd.nist.gov/vuln/detail/CVE-2020-25645){: external}, and [CVE-2020-4788](https://nvd.nist.gov/vuln/detail/CVE-2020-4788){: external}. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.18.12_1533" caption-side="top"}

### Changelog for worker node fix pack 1.18.12_1533, released 23 November 2020
{: #11812_1533_worker}

The following table shows the changes that are included in the worker node fix pack `1.18.12_1533`. Worker node patch updates can be applied by updating, reloading (in classic infrastructure), or replacing (in VPC infrastructure) the worker node.
{: shortdesc}

| Component | Previous | Current | Description |
| --------- | -------- | ------- | ----------- |
| Kubernetes | v1.18.10 | v1.18.12 | See the [Kubernetes changelog](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG/CHANGELOG-1.18.md#v11812){: external}.|
| Ubuntu 18.04 packages | 4.15.0-122-generic | 4.15.0-123-generic | Updated worker node image with kernel and package updates for [CVE-2020-25692](https://nvd.nist.gov/vuln/detail/CVE-2020-25692){: external}, [CVE-2020-25709](https://nvd.nist.gov/vuln/detail/CVE-2020-25709){: external}, [CVE-2020-25710](https://nvd.nist.gov/vuln/detail/CVE-2020-25710){: external}, [CVE-2020-28196](https://nvd.nist.gov/vuln/detail/CVE-2020-28196){: external}, and [CVE-2020-8694](https://nvd.nist.gov/vuln/detail/CVE-2020-8694){: external}. |
| Ubuntu 16.04 packages | 4.4.0-193-generic | 4.4.0-194-generic | Updated worker node image with kernel and package updates for [CVE-2020-25692](https://nvd.nist.gov/vuln/detail/CVE-2020-25692){: external}, [CVE-2020-25709](https://nvd.nist.gov/vuln/detail/CVE-2020-25709){: external}, [CVE-2020-25710](https://nvd.nist.gov/vuln/detail/CVE-2020-25710){: external}, [CVE-2020-28196](https://nvd.nist.gov/vuln/detail/CVE-2020-28196){: external}, and [CVE-2020-8694](https://nvd.nist.gov/vuln/detail/CVE-2020-8694){: external}. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.18.10_1532" caption-side="top"}

### Changelog for master fix pack 1.18.12_1533, released 16 November 2020
{: #11812_1533}

The following table shows the changes that are included in the master fix pack patch update `1.18.12_1533`. Master patch updates are applied automatically.
{: shortdesc}

| Component | Previous | Current | Description |
| --- | --- | --- | --- |
| Cluster health image | v1.1.12 | v1.1.13 | Updated image for [DLA-2424-1](https://www.debian.org/lts/security/2020/dla-2424){: external}. |
| GPU device plug-in and installer | 0c07674 | b966c41 | Updated image for [CVE-2019-20386](https://nvd.nist.gov/vuln/detail/CVE-2019-20386){: external}, [CVE-2019-13050](https://nvd.nist.gov/vuln/detail/CVE-2019-13050){: external}, [CVE-2020-8177](https://nvd.nist.gov/vuln/detail/CVE-2020-8177){: external}, [CVE-2019-14889](https://nvd.nist.gov/vuln/detail/CVE-2019-14889){: external}, [CVE-2020-1730](https://nvd.nist.gov/vuln/detail/CVE-2020-1730){: external}, [CVE-2020-10029](https://nvd.nist.gov/vuln/detail/CVE-2020-10029){: external}, [CVE-2020-1751](https://nvd.nist.gov/vuln/detail/CVE-2020-1751){: external}, [CVE-2020-1752](https://nvd.nist.gov/vuln/detail/CVE-2020-1752){: external}, [CVE-2019-16168](https://nvd.nist.gov/vuln/detail/CVE-2019-16168){: external}, [CVE-2019-20218](https://nvd.nist.gov/vuln/detail/CVE-2019-20218){: external}, [CVE-2019-5018](https://nvd.nist.gov/vuln/detail/CVE-2019-5018){: external}, [CVE-2020-13630](https://nvd.nist.gov/vuln/detail/CVE-2020-13630){: external}, [CVE-2020-13631](https://nvd.nist.gov/vuln/detail/CVE-2020-13631){: external}, [CVE-2020-13632](https://nvd.nist.gov/vuln/detail/CVE-2020-13632){: external}, [CVE-2020-6405](https://nvd.nist.gov/vuln/detail/CVE-2020-6405){: external}, [CVE-2020-9327](https://nvd.nist.gov/vuln/detail/CVE-2020-9327){: external}, [CVE-2019-1551](https://nvd.nist.gov/vuln/detail/CVE-2019-1551){: external}, [CVE-2019-19221](https://nvd.nist.gov/vuln/detail/CVE-2019-19221){: external}, [CVE-2019-16935](https://nvd.nist.gov/vuln/detail/CVE-2019-16935){: external}, [CVE-2019-20907](https://nvd.nist.gov/vuln/detail/CVE-2019-20907){: external}, [CVE-2020-14422](https://nvd.nist.gov/vuln/detail/CVE-2020-14422){: external}, [CVE-2020-8492](https://nvd.nist.gov/vuln/detail/CVE-2020-8492){: external}, [CVE-2019-19906](https://nvd.nist.gov/vuln/detail/CVE-2019-19906){: external}, [CVE-2019-20454](https://nvd.nist.gov/vuln/detail/CVE-2019-20454){: external}, [CVE-2019-19956](https://nvd.nist.gov/vuln/detail/CVE-2019-19956){: external}, [CVE-2019-20388](https://nvd.nist.gov/vuln/detail/CVE-2019-20388){: external}, [CVE-2020-7595](https://nvd.nist.gov/vuln/detail/CVE-2020-7595){: external}, [CVE-2019-13627](https://nvd.nist.gov/vuln/detail/CVE-2019-13627){: external}, [CVE-2018-20843](https://nvd.nist.gov/vuln/detail/CVE-2018-20843){: external}, [CVE-2019-15903](https://nvd.nist.gov/vuln/detail/CVE-2019-15903){: external}, and [CVE-2019-20387](https://nvd.nist.gov/vuln/detail/CVE-2019-20387){: external}. |
| {{site.data.keyword.cloud_notm}} Controller Manager |v1.18.10-1 | v1.18.12-1 | Updated to support the Kubernetes 1.18.12 release. Updated image for [DLA-2424-1](https://www.debian.org/lts/security/2020/dla-2424){: external}. |
| {{site.data.keyword.filestorage_full_notm}} plug-in and monitor | 378 | 379 | Updated to use the universal base image (UBI) and to use `Go` version 1.15.2. |
| {{site.data.keyword.cloud_notm}} RBAC Operator | 31c794a | 197bc70 | Updated image for [CVE-2019-20454](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-20454){: external}, [CVE-2020-10029](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-10029){: external}, [CVE-2020-1751](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-1751){: external}, [CVE-2020-1752](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-1752){: external}, [CVE-2019-20807](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-20807){: external}, [CVE-2019-16935](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-16935){: external}, [CVE-2019-20907](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-20907){: external}, [CVE-2020-14422](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-14422){: external}, [CVE-2020-8492](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-8492){: external}, [CVE-2019-15165](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-15165){: external}, [CVE-2019-16168](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-16168){: external}, [CVE-2019-20218](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-20218){: external}, [CVE-2019-5018](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5018){: external}, [CVE-2020-13630](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-13630){: external}, [CVE-2020-13631](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-13631){: external}, [CVE-2020-13632](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-13632){: external}, [CVE-2020-6405](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-6405){: external}, [CVE-2020-9327](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-9327){: external}, [CVE-2020-8177](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-8177){: external}, [CVE-2019-13050](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-13050){: external}, [CVE-2018-20843](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-20843){: external}, [CVE-2019-15903](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-15903){: external}, [CVE-2019-14889](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-14889){: external}, [CVE-2020-1730](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-1730){: external}, [CVE-2019-1551](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1551){: external}, [CVE-2020-14382](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-14382){: external}, [CVE-2019-13627](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-13627){: external}, [CVE-2019-19956](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-19956){: external}, [CVE-2019-20388](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-20388){: external}, [CVE-2020-7595](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-7595){: external}, [CVE-2019-20386](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-20386){: external}, [CVE-2019-19906](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-19906){: external}, [CVE-2019-20387](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-20387){: external}, and [CVE-2019-19221](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-19221){: external}.  |
| Key Management Service provider | v2.0.4 | v2.0.5 | Updated image for [DLA-2424-1](https://www.debian.org/lts/security/2020/dla-2424){: external}. |
| Kubernetes | v1.18.10 | v1.18.12 | See the [Kubernetes release notes](https://github.com/kubernetes/kubernetes/releases/tag/v1.18.12){: external}. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.18.10_1531" caption-side="top"}

### Changelog for worker node fix pack 1.18.10_1532, released 9 November 2020
{: #11810_1532}

The following table shows the changes that are included in the worker node fix pack `1.18.10_1532`. Worker node patch updates can be applied by updating, reloading (in classic infrastructure), or replacing (in VPC infrastructure) the worker node.
{: shortdesc}

| Component | Previous | Current | Description |
| --------- | -------- | ------- | ----------- |
|Ubuntu 18.04 packages | N/A | N/A | Updated worker node image with kernal and package updates for [CVE-2018-14036](https://nvd.nist.gov/vuln/detail/CVE-2018-14036){: external}, [CVE-2020-10543](https://nvd.nist.gov/vuln/detail/CVE-2020-10543){: external}, [CVE-2020-10878](https://nvd.nist.gov/vuln/detail/CVE-2020-10878){: external}, [CVE-2020-12723](https://nvd.nist.gov/vuln/detail/CVE-2020-12723){: external}, [CVE-2020-16126](https://nvd.nist.gov/vuln/detail/CVE-2020-16126){: external}, [CVE-2020-25659](https://nvd.nist.gov/vuln/detail/CVE-2020-25659){: external}, and [CVE-2017-18269](https://nvd.nist.gov/vuln/detail/CVE-2017-18269){: external}. |
|Ubuntu 16.04 packages | N/A | N/A | Updated worker node image with package updates for [CVE-2018-14036](https://nvd.nist.gov/vuln/detail/CVE-2018-14036){: external}, [CVE-2020-10543](https://nvd.nist.gov/vuln/detail/CVE-2020-10543){: external}, [CVE-2020-10878](https://nvd.nist.gov/vuln/detail/CVE-2020-10878){: external}, [CVE-2020-12723](https://nvd.nist.gov/vuln/detail/CVE-2020-12723){: external}, [CVE-2020-16126](https://nvd.nist.gov/vuln/detail/CVE-2020-16126){: external}, and [CVE-2020-25659](https://nvd.nist.gov/vuln/detail/CVE-2020-25659){: external}.|
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.18.10_1531" caption-side="top"}

### Changelog for worker node fix pack 1.18.10_1531, released 26 October 2020
{: #11810_1531_worker}

The following table shows the changes that are included in the worker node fix pack `1.18.10_15313`. Worker node patch updates can be applied by updating, reloading (in classic infrastructure), or replacing (in VPC infrastructure) the worker node.
{: shortdesc}

| Component | Previous | Current | Description |
| --------- | -------- | ------- | ----------- |
| Kubernetes | v1.18.9 | v1.18.10 | See the [Kubernetes release notes](https://github.com/kubernetes/kubernetes/releases/tag/v1.18.10){: external}. |
|Ubuntu 18.04 packages | 4.15.0-118-generic | 4.15.0-122-generic | Updated worker node images with kernal and package updates for [CVE-2018-10322](https://nvd.nist.gov/vuln/detail/CVE-2018-10322){: external},[CVE-2019-20807](https://nvd.nist.gov/vuln/detail/CVE-2019-20807){: external}, [CVE-2019-20916](https://nvd.nist.gov/vuln/detail/CVE-2019-20916){: external}, [CVE-2020-12351](https://nvd.nist.gov/vuln/detail/CVE-2020-12351){: external}, [CVE-2020-12352](https://nvd.nist.gov/vuln/detail/CVE-2020-12352){: external}, [CVE-2020-15999](https://nvd.nist.gov/vuln/detail/CVE-2020-15999){: external}, [CVE-2020-16119](https://nvd.nist.gov/vuln/detail/CVE-2020-16119){: external}, [CVE-2020-16120](https://nvd.nist.gov/vuln/detail/CVE-2020-16120){: external}, [CVE-2020-24490](https://nvd.nist.gov/vuln/detail/CVE-2020-24490){: external}, and [CVE-2020-26116](https://nvd.nist.gov/vuln/detail/CVE-2020-26116){: external}. |
|Ubuntu 16.04 packages | 4.4.0-190-generic | 4.4.0-193-generic | Updated worker node images with kernel and package updates for [CVE-2017-17087](https://nvd.nist.gov/vuln/detail/CVE-2017-17087){: external}, [CVE-2018-10322](https://nvd.nist.gov/vuln/detail/CVE-2018-10322){: external}, [CVE-2019-20807](https://nvd.nist.gov/vuln/detail/CVE-2019-20807){: external}, [CVE-2020-15999](https://nvd.nist.gov/vuln/detail/CVE-2020-15999){: external}, [CVE-2020-16119](https://nvd.nist.gov/vuln/detail/CVE-2020-16119){: external}, and [CVE-2020-26116](https://nvd.nist.gov/vuln/detail/CVE-2020-26116){: external}. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.18.9_1530" caption-side="top"}

### Changelog for master fix pack 1.18.10_1531, released 26 October 2020
{: #11810_1531}

The following table shows the changes that are included in the master fix pack patch update `1.18.10_1531`. Master patch updates are applied automatically.
{: shortdesc}

| Component | Previous | Current | Description |
| --- | --- | --- | --- |
| Calico configuration | N/A | N/A | Updated the `calico-node` daemon set in the `kube-system` namespace to set the `spec.updateStrategy.rollingUpdate.maxUnavailable` parameter to `10%` for clusters with more than 50 worker nodes. |
| Cluster health image | v1.1.11 | v1.1.12 | Updated to use `Go` version 1.15.2. |
| Gateway-enabled cluster controller | 1082 | 1105 | Updated to use `Go` version 1.15.2. |
| {{site.data.keyword.cloud_notm}} Controller Manager | v1.18.9-1 | v1.18.10-1 | Updated to support the Kubernetes 1.18.10 release. |
| {{site.data.keyword.cloud_notm}} RBAC Operator | 4b47693 | 31c794a | Updated to use `Go` version 1.15.2. |
| Kubernetes | v1.18.9 | v1.18.10 | See the [Kubernetes release notes](https://github.com/kubernetes/kubernetes/releases/tag/v1.18.10){: external}. |
| Kubernetes `NodeLocal` DNS cache | 1.15.13 | 1.15.14 | See the [Kubernetes `NodeLocal` DNS cache release notes](https://github.com/kubernetes/dns/releases/tag/1.15.14){: external}. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.18.9_1528" caption-side="top"}

### Changelog for worker node fix pack 1.18.9_1530, released 12 October 2020
{: #1189_1530}

The following table shows the changes that are included in the worker node fix pack `1.18.9_1530`. Worker node patch updates can be applied by updating, reloading (in classic infrastructure), or replacing (in VPC infrastructure) the worker node.
{: shortdesc}

| Component | Previous | Current | Description |
| --------- | -------- | ------- | ----------- |
|Ubuntu 18.04 packages | N/A | N/A | Updated worker node image with package updates for [CVE-2019-8936](https://nvd.nist.gov/vuln/detail/CVE-2019-8936){: external}, [CVE-2020-26137](https://nvd.nist.gov/vuln/detail/CVE-2020-26137){: external}, and [CVE-2020-14365](https://nvd.nist.gov/vuln/detail/CVE-2020-14365){: external}.|
|Ubuntu 16.04 packages | N/A | N/A | Updated worker node image with package updates for [CVE-2020-14365](https://nvd.nist.gov/vuln/detail/CVE-2020-14365){: external} and [CVE-2020-26137](https://nvd.nist.gov/vuln/detail/CVE-2020-26137){: external}. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.18.8_1529" caption-side="top"}

### Changelog for worker node fix pack 1.18.9_1529, released 28 September 2020
{: #1189_1529}

The following table shows the changes that are included in the worker node fix pack `1.18.9_1529`. Worker node patch updates can be applied by updating, reloading (in classic infrastructure), or replacing (in VPC infrastructure) the worker node.
{: shortdesc}

| Component | Previous | Current | Description |
| --------- | -------- | ------- | ----------- |
| Kubernetes | v1.18.8	| v1.18.9 | See the [Kubernetes release notes](https://github.com/kubernetes/kubernetes/releases/tag/v1.18.9){: external}. |
| Ubuntu 18.04 packages | 4.15.0-117-generic | 4.15.0-118-generic | Updated worker node image with   kernel and package updates for [CVE-2018-1000500](https://nvd.nist.gov/vuln/detail/CVE-2018-1000500){: external}, [CVE-2018-7738](https://nvd.nist.gov/vuln/detail/CVE-2018-7738){: external}, [CVE-2019-14855](https://nvd.nist.gov/vuln/detail/CVE-2019-14855){: external}, [CVE-2019-1547](https://nvd.nist.gov/vuln/detail/CVE-2019-1547){: external}, [CVE-2019-1551](https://nvd.nist.gov/vuln/detail/CVE-2019-1551){: external}, [CVE-2019-1563](https://nvd.nist.gov/vuln/detail/CVE-2019-1563){: external}, [CVE-2020-10753](https://nvd.nist.gov/vuln/detail/CVE-2020-10753){: external}, [CVE-2020-12059](https://nvd.nist.gov/vuln/detail/CVE-2020-12059){: external}, [CVE-2020-12888](https://nvd.nist.gov/vuln/detail/CVE-2020-12888){: external}, [CVE-2020-1760](https://nvd.nist.gov/vuln/detail/CVE-2020-1760){: external}, and [CVE-2020-1968](https://nvd.nist.gov/vuln/detail/CVE-2020-1968){: external}.|
| Ubuntu 16.04 packages | 4.4.0-189-generic | 4.4.0-190-generic | Updated worker node image with kernel and package updates for [CVE-2019-20811](https://nvd.nist.gov/vuln/detail/CVE-2019-20811){: external}, [CVE-2019-9453](https://nvd.nist.gov/vuln/detail/CVE-2019-9453){: external}, [CVE-2020-0067](https://nvd.nist.gov/vuln/detail/CVE-2020-0067){: external}, and [CVE-2020-1968](https://nvd.nist.gov/vuln/detail/CVE-2020-1968){: external}. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.18.8_1527" caption-side="top"}

### Changelog for master fix pack 1.18.9_1528, released 21 September 2020
{: #1189_1528}

The following table shows the changes that are included in the master fix pack patch update `1.18.9_1528`. Master patch updates are applied automatically.
{: shortdesc}

| Component | Previous | Current | Description |
| --- | --- | --- | --- |
| Cluster health image | v1.1.9 | v1.1.11 | Updated `Go` version for [CVE-2020-16845](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-16845){: external} and [CVE-2020-24553](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-24553){: external}. |
| etcd | v3.4.10 | v3.4.13 | See the [etcd release notes](https://github.com/etcd-io/etcd/releases/v3.4.13){: external}. |
| GPU device plug-in and installer | bacb9e1 | edd26a4 | Updated `Go` version for [CVE-2020-16845](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-16845){: external} and [CVE-2020-24553](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-24553){: external}. Updated the GPU drivers to version [450.51.06](https://www.nvidia.com/download/driverResults.aspx/162630){: external}. |
| {{site.data.keyword.cloud_notm}} Controller Manager | v1.18.6-1 | v1.18.9-1 | Updated to support the Kubernetes 1.18.9 release and to use `Go` version 1.13.15. |
| {{site.data.keyword.filestorage_full_notm}} plug-in and monitor | 377 | 378 | Updated `Go` version for [CVE-2020-16845](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-16845){: external}. |
| {{site.data.keyword.cloud_notm}} RBAC Operator | d80b738 | 4b47693 | Updated `Go` version for [CVE-2020-16845](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-16845){: external} and image for [CVE-2020-14352](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-14352){: external}. |
| Key Management Service provider | v2.0.2 | v2.0.4 | Updated `Go` version for [CVE-2020-16845](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-16845){: external} and [CVE-2020-24553](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-24553){: external}. |
| Kubernetes | v1.18.8 | v1.18.9 | See the [Kubernetes release notes](https://github.com/kubernetes/kubernetes/releases/tag/v1.18.9){: external}. |
| Kubernetes Dashboard | v2.0.3 | v2.0.4 | See the [Kubernetes Dashboard release notes](https://github.com/kubernetes/dashboard/releases/tag/v2.0.4){: external}. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.18.8_1527" caption-side="top"}

### Changelog for worker node fix pack 1.18.8_1527, released 14 September 2020
{: #1188_1527}

The following table shows the changes that are included in the worker node fix pack `1.18.8_1527`. Worker node patch updates can be applied by updating, reloading (in classic infrastructure), or replacing (in VPC infrastructure) the worker node.
{: shortdesc}

| Component | Previous | Current | Description |
| --------- | -------- | ------- | ----------- |
| HAProxy | 1.8.25-384f42 | 1.8.26-561f1a | See the [HAProxy changelog](https://www.haproxy.org/download/1.8/src/CHANGELOG){: external}.|
| Ubuntu 18.04 packages | 4.15.0-112-generic | 4.15.0-117-generic | Updated worker node image with kernel and package updates for [CVE-2020-14344](https://nvd.nist.gov/vuln/detail/CVE-2020-14344){: external}, [CVE-2020-14363](https://nvd.nist.gov/vuln/detail/CVE-2020-14363){: external}, and [CVE-2020-14386](https://nvd.nist.gov/vuln/detail/CVE-2020-14386){: external}. |
| Ubuntu 16.04 packages | 4.4.0-187-generic | 4.4.0-189-generic | Updated worker node image with kernel and package updates for [CVE-2020-14344](https://nvd.nist.gov/vuln/detail/CVE-2020-14344){: external} and [CVE-2020-14363](https://nvd.nist.gov/vuln/detail/CVE-2020-14363){: external}. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.18.8_1526" caption-side="top"}

### Changelog for worker node fix pack 1.18.8_1526, released 31 August 2020
{: #1188_1526}

The following table shows the changes that are included in the worker node fix pack `1.18.8_1526`. Worker node patch updates can be applied by updating, reloading (in classic infrastructure), or replacing (in VPC infrastructure) the worker node.
{: shortdesc}

| Component | Previous | Current | Description |
| --------- | -------- | ------- | ----------- |
| Ubuntu 18.04 packages | N/A | N/A | Updated worker node image with package updates for [CVE-2020-12403](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-12403){: external}, [CVE-2020-8231](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-8231){: external}, [CVE-2020-8622](https://nvd.nist.gov/vuln/detail/CVE-2020-8622){: external}, [CVE-2020-8623](https://nvd.nist.gov/vuln/detail/CVE-2020-8623){: external}, and [CVE-2020-8624](https://nvd.nist.gov/vuln/detail/CVE-2020-8624){: external}. |
| Ubuntu 16.04 packages | 4.4.0-186-generic | 4.4.0-187-generic | Updated worker node image with kernel and package updates for [CVE-2020-8231](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-8231){: external}, [CVE-2020-8622](https://nvd.nist.gov/vuln/detail/CVE-2020-8622){: external}, and [CVE-2020-8623](https://nvd.nist.gov/vuln/detail/CVE-2020-8623){: external}. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.18.8_1525" caption-side="top"}

### Changelog for master fix pack 1.18.8_1525, released 18 August 2020
{: #1188_1525_master}

The following table shows the changes that are included in the master fix pack patch update `1.18.8_1525`. Master patch updates are applied automatically.
{: shortdesc}

| Component | Previous | Current | Description |
| --------- | -------- | ------- | ----------- |
| Cluster health image | v1.1.8 | v1.1.9 | Updated to use `Go` version 1.13.13. |
| etcd | v3.4.9 | v3.4.10 | See the [etcd release notes](https://github.com/etcd-io/etcd/releases/v3.4.10){: external}. |
| GPU device plug-in and installer | 6847df8 | f22c75e | Updated image for [CVE-2020-14039](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-14039){: external} and [CVE-2020-15586](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-15586){: external}. |
| {{site.data.keyword.filestorage_full_notm}} plug-in and monitor | 376 | 377 | Fixed a bug that prevents persistent volume claim (PVC) creation failures from being retried. |
| {{site.data.keyword.cloud_notm}} RBAC Operator | 8882606 | d80b738 | Updated image for [CVE-2020-12049](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-12049){: external} and to use `Go` version 1.13.14. |
| Key Management Service provider | v1.0.0 | v2.0.2 | Updated image for [CVE-2020-15586](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-15586){: external}. |
| Kubernetes | v1.18.6 | v1.18.8 | See the [Kubernetes release notes](https://github.com/kubernetes/kubernetes/releases/tag/v1.18.8){: external}. |
| Kubernetes add-on resizer | 1.8.7 | 1.8.11 | See the [Kubernetes add-on resizer release notes](https://github.com/kubernetes/autoscaler/releases/tag/addon-resizer-1.8.11){: external}. |
| Kubernetes configuration | N/A | N/A | The Kubernetes API server audit policy configuration is updated to include auditing of all API groups except `apiregistration.k8s.io` and `coordination.k8s.io`. |
| Kubernetes Metrics Server | v0.3.6 | v0.3.7 | See the [Kubernetes Metrics Server release notes](https://github.com/kubernetes-sigs/metrics-server/releases/tag/v0.3.7){: external}. |
| Kubernetes `NodeLocal` DNS cache configuration | N/A | N/A | Increased the pod termination grace period. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.18.6_1523" caption-side="top"}

### Changelog for worker node fix pack 1.18.8_1525, released 17 August 2020
{: #1188_1525}

The following table shows the changes that are included in the worker node fix pack `1.18.8_1525`. Worker node patch updates can be applied by updating, reloading (in classic infrastructure), or replacing (in VPC infrastructure) the worker node.
{: shortdesc}

| Component | Previous | Current | Description |
| --------- | -------- | ------- | ----------- |
| Kubernetes | v1.18.6 | v1.18.8 | See the [Kubernetes changelogs](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG/CHANGELOG-1.18.md#v1188){: external}. |
| Ubuntu 18.04 packages | N/A | N/A | Updated worker node image with package updates for [CVE-2020-11936](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-11936){: external}, [CVE-2020-12400](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-12400){: external}, [CVE-2020-12401](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-12401){: external}, [CVE-2020-15701](https://nvd.nist.gov/vuln/detail/CVE-2020-15701){: external}, [CVE-2020-15702](https://nvd.nist.gov/vuln/detail/CVE-2020-15702){: external}, [CVE-2020-15709](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-15709){: external}, and [CVE-2020-6829](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-6829){: external}. |
| Ubuntu 16.04 packages | N/A | N/A | Updated worker node image with package updates for [CVE-2020-11936](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-11936){: external}, [CVE-2020-15701](https://nvd.nist.gov/vuln/detail/CVE-2020-15701){: external}, [CVE-2020-15702](https://nvd.nist.gov/vuln/detail/CVE-2020-15702){: external}, and [CVE-2020-15709](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-15709){: external}. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.18.6_1523" caption-side="top"}

### Changelog for worker node fix pack 1.18.6_1523, released 3 August 2020
{: #1186_1523}

The following table shows the changes that are included in the worker node fix pack `1.18.6_1523`. Worker node patch updates can be applied by updating, reloading (in classic infrastructure), or replacing (in VPC infrastructure) the worker node.
{: shortdesc}

| Component | Previous | Current | Description |
| --------- | -------- | ------- | ----------- |
| Ubuntu 18.04 packages | 4.15.0-111-generic | 4.15.0-112-generic | Updated worker node images with kernel and package updates for [CVE-2019-17514](https://nvd.nist.gov/vuln/detail/CVE-2019-17514){: external}, [CVE-2019-20907](https://nvd.nist.gov/vuln/detail/CVE-2019-20907){: external}, [CVE-2019-9674](https://nvd.nist.gov/vuln/detail/CVE-2019-9674){: external}, [CVE-2020-10713](https://nvd.nist.gov/vuln/detail/CVE-2020-10713){: external}, [CVE-2020-10757](https://nvd.nist.gov/vuln/detail/CVE-2020-10757){: external}, [CVE-2020-11935](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-11935){: external}, [CVE-2020-14308](https://nvd.nist.gov/vuln/detail/CVE-2020-14308){: external}, [CVE-2020-14309](https://nvd.nist.gov/vuln/detail/CVE-2020-14309){: external}, [CVE-2020-14310](https://nvd.nist.gov/vuln/detail/CVE-2020-14310){: external}, [CVE-2020-14311](https://nvd.nist.gov/vuln/detail/CVE-2020-14311){: external}, [CVE-2020-14422](https://nvd.nist.gov/vuln/detail/CVE-2020-14422){: external}, [CVE-2020-15705](https://nvd.nist.gov/vuln/detail/CVE-2020-15705){: external}, [CVE-2020-15706](https://nvd.nist.gov/vuln/detail/CVE-2020-15706){: external}, and [CVE-2020-15707](https://nvd.nist.gov/vuln/detail/CVE-2020-15707){: external}. |
| Ubuntu 16.04 packages | 4.4.0-185-generic | 4.4.0-186-generic | Updated worker node images with package updates for [CVE-2019-12380](https://nvd.nist.gov/vuln/detail/CVE-2019-12380){: external}, [CVE-2019-17514](https://nvd.nist.gov/vuln/detail/CVE-2019-17514){: external}, [CVE-2019-20907](https://nvd.nist.gov/vuln/detail/CVE-2019-20907){: external}, [CVE-2019-9674](https://nvd.nist.gov/vuln/detail/CVE-2019-9674){: external}, [CVE-2020-10713](https://nvd.nist.gov/vuln/detail/CVE-2020-10713){: external}, [CVE-2020-11935](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-11935){: external}, [CVE-2020-14308](https://nvd.nist.gov/vuln/detail/CVE-2020-14308){: external}, [CVE-2020-14309](https://nvd.nist.gov/vuln/detail/CVE-2020-14309){: external}, [CVE-2020-14310](https://nvd.nist.gov/vuln/detail/CVE-2020-14310){: external}, [CVE-2020-14311](https://nvd.nist.gov/vuln/detail/CVE-2020-14311){: external}, [CVE-2020-14422](https://nvd.nist.gov/vuln/detail/CVE-2020-14422){: external}, [CVE-2020-15705](https://nvd.nist.gov/vuln/detail/CVE-2020-15705){: external}, [CVE-2020-15706](https://nvd.nist.gov/vuln/detail/CVE-2020-15706){: external}, and [CVE-2020-15707](https://nvd.nist.gov/vuln/detail/CVE-2020-15707){: external}. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.18.6_1520" caption-side="top"}

### Changelog for master fix pack 1.18.6_1522, released 24 July 2020
{: #1186_1522}

The following table shows the changes that are included in the master fix pack patch update `1.18.6_1522`. Master patch updates are applied automatically.
{: shortdesc}

| Component | Previous | Current | Description |
| --------- | -------- | ------- | ----------- |
| Cluster master operations | N/A | N/A | Fixed a problem that might cause pods to fail authentication to the Kubernetes API server after a cluster master operation. |
| {{site.data.keyword.filestorage_full_notm}} plug-in and monitor | 375 | 376 | Updated to use `Go` version 1.13.8. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.18.6_1521" caption-side="top"}

### Changelog for master fix pack 1.18.6_1521, released 20 July 2020
{: #1186_1521}

The following table shows the changes that are included in the master fix pack patch update `1.18.6_1521`. Master patch updates are applied automatically.
{: shortdesc}

| Component | Previous | Current | Description |
| --------- | -------- | ------- | ----------- |
| GPU device plug-in and installer | 31d4bb6 | 8c24345 | Updated image for [CVE-2017-12133](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-12133){: external}, [CVE-2017-18269](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-18269){: external}, [CVE-2018-11236](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-11236){: external}, [CVE-2018-11237](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-11237){: external}, [CVE-2018-19591](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-19591){: external}, [CVE-2018-6485](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-6485){: external}, [CVE-2019-19126](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-19126){: external}, [CVE-2019-9169](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9169){: external}, [CVE-2020-10029](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-10029){: external}, [CVE-2020-1751](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-1751){: external}, and [CVE-2020-1752](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-1752){: external}. |
| IBM Calico extension | 353 | 378 | Updated to handle any `ens` network interface. |
| {{site.data.keyword.cloud_notm}} Controller Manager | v1.18.4-1 | v1.18.6-1 |Updated to support the Kubernetes 1.18.6 release. |
| {{site.data.keyword.filestorage_full_notm}} plug-in and monitor configuration | N/A | N/A | Added a pod memory limit. |
| {{site.data.keyword.cloud_notm}} RBAC operator | 08ce50e | 8882606 | Updated image for [CVE-2020-13777](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-13777){: external} and to use `Go` version 1.13.12. |
| Kubernetes | v1.18.4 | v1.18.6 | See the [Kubernetes release notes](https://github.com/kubernetes/kubernetes/releases/tag/v1.18.6){: external}. The update resolves CVE-2020-8559 (see the [IBM security bulletin](https://www.ibm.com/support/pages/node/6249915){: external}). |
| Kubernetes configuration | N/A | N/A | The Kubernetes API server audit policy configuration is updated to include auditing the `scheduling.k8s.io` API group and the `tokenreviews` resource. |
| Kubernetes Dashboard | v2.0.1 | v2.0.3 | See the [Kubernetes Dashboard release notes](https://github.com/kubernetes/dashboard/releases/tag/v2.0.3){: external}. |
| OpenVPN server | 2.4.6-r3-IKS-131 | 2.4.6-r3-IKS-222 | Removed the deprecated `comp-lzo` compression configuration option and updated the default cipher that is used for encryption. |
| Operator Lifecycle Manager | 0.14.1 | 0.14.1-IKS-1 | Updated image for [CVE-2020-1967](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-1967){: external}. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.18.4_1518" caption-side="top"}

### Changelog for worker node fix pack 1.18.6_1520, released 20 July 2020
{: #1186_1520}

The following table shows the changes that are included in the worker node fix pack `1.18.6_1520`. Worker node patch updates can be applied by updating, reloading (in classic infrastructure), or replacing (in VPC infrastructure) the worker node.
{: shortdesc}

| Component | Previous | Current | Description |
| --------- | -------- | ------- | ----------- |
| HAProxy | 2.0.15-afe432 | 1.8.25-384f42 | See the [HAProxy changelog](https://www.haproxy.org/download/1.8/src/CHANGELOG){: external}. Fixes a connection leak that happens when HAProxy is under high load. |
| Kubernetes | v1.18.4 | v1.18.6 | See the [Kubernetes changelogs](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG/CHANGELOG-1.18.md#v1186){: external}. The update resolves CVE-2020-8557 (see the [IBM security bulletin](https://www.ibm.com/support/pages/node/6249941){: external}). |
| Ubuntu 18.04 packages | 4.15.0-109-generic | 4.15.0-111-generic | Updated worker node images with kernel and package updates for [CVE-2018-11236](https://nvd.nist.gov/vuln/detail/CVE-2018-11236){: external}, [CVE-2018-11237](https://nvd.nist.gov/vuln/detail/CVE-2018-11237){: external}, [CVE-2018-19591](https://nvd.nist.gov/vuln/detail/CVE-2018-19591){: external}, [CVE-2019-19126](https://nvd.nist.gov/vuln/detail/CVE-2019-19126){: external}, [CVE-2019-9169](https://nvd.nist.gov/vuln/detail/CVE-2019-9169){: external}, [CVE-2020-10029](https://nvd.nist.gov/vuln/detail/CVE-2020-10029){: external}, [CVE-2020-12402](https://nvd.nist.gov/vuln/detail/CVE-2020-12402){: external}, [CVE-2020-1751](https://nvd.nist.gov/vuln/detail/CVE-2020-1751){: external}, and [CVE-2020-1752](https://nvd.nist.gov/vuln/detail/CVE-2020-1752){: external}. |
| Ubuntu 16.04 packages | 4.4.0-184-generic | 4.4.0-185-generic | Updated worker node images with package updates for [CVE-2017-12133](https://nvd.nist.gov/vuln/detail/CVE-2017-12133){: external}, [CVE-2017-18269](https://nvd.nist.gov/vuln/detail/CVE-2017-18269){: external}, [CVE-2018-11236](https://nvd.nist.gov/vuln/detail/CVE-2018-11236){: external}, [CVE-2018-11237](https://nvd.nist.gov/vuln/detail/CVE-2018-11237){: external}, [CVE-2018-6485](https://nvd.nist.gov/vuln/detail/CVE-2018-6485){: external}, [CVE-2019-19126](https://nvd.nist.gov/vuln/detail/CVE-2019-19126){: external}, [CVE-2019-9169](https://nvd.nist.gov/vuln/detail/CVE-2019-9169){: external}, [CVE-2020-0543](https://nvd.nist.gov/vuln/detail/CVE-2020-0543){: external}, [CVE-2020-10029](https://nvd.nist.gov/vuln/detail/CVE-2020-10029){: external}, [CVE-2020-10711](https://nvd.nist.gov/vuln/detail/CVE-2020-10711){: external}, [CVE-2020-13143](https://nvd.nist.gov/vuln/detail/CVE-2020-13143){: external}, [CVE-2020-1751](https://nvd.nist.gov/vuln/detail/CVE-2020-1751){: external}, and [CVE-2020-1752](https://nvd.nist.gov/vuln/detail/CVE-2020-1752){: external}. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.18.4_1518" caption-side="top"}

### Changelog for worker node fix pack 1.18.4_1518, released 6 July 2020
{: #1184_1518}

The following table shows the changes that are included in the worker node fix pack `1.18.4_1518`. Worker node patch updates can be applied by updating, reloading (in classic infrastructure), or replacing (in VPC infrastructure) the worker node.
{: shortdesc}

| Component | Previous | Current | Description |
| --------- | -------- | ------- | ----------- |
| HAProxy | 1.8.25-30b675 | 2.0.15-afe432 | See the [HAProxy changelog](https://www.haproxy.org/download/2.0/src/CHANGELOG){: external}. |
| Ubuntu 18.04 packages | 4.15.0-106-generic | 4.15.0-109-generic | Updated worker node images with kernel and package updates for [CVE-2019-12380](https://nvd.nist.gov/vuln/detail/CVE-2019-12380){: external}, [CVE-2019-16089](https://nvd.nist.gov/vuln/detail/CVE-2019-16089){: external}, [CVE-2019-19036](https://nvd.nist.gov/vuln/detail/CVE-2019-19036){: external}, [CVE-2019-19039](https://nvd.nist.gov/vuln/detail/CVE-2019-19039){: external}, [CVE-2019-19318](https://nvd.nist.gov/vuln/detail/CVE-2019-19318){: external}, [CVE-2019-19642](https://nvd.nist.gov/vuln/detail/CVE-2019-19642){: external}, [CVE-2019-19813](https://nvd.nist.gov/vuln/detail/CVE-2019-19813){: external}, [CVE-2019-3689](https://nvd.nist.gov/vuln/detail/CVE-2019-3689){: external}, [CVE-2020-0543](https://nvd.nist.gov/vuln/detail/CVE-2020-0543){: external}, [CVE-2020-10711](https://nvd.nist.gov/vuln/detail/CVE-2020-10711){: external}, [CVE-2020-13143](https://nvd.nist.gov/vuln/detail/CVE-2020-13143){: external}, [CVE-2020-8177](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-8177){: external}, [CVE-2019-19377](https://nvd.nist.gov/vuln/detail/CVE-2019-19377){: external}, and [CVE-2019-19816](https://nvd.nist.gov/vuln/detail/CVE-2019-19816){: external}. |
| Ubuntu 16.04 packages |N/A | N/A| Updated worker node images with package updates for [CVE-2019-3689](https://nvd.nist.gov/vuln/detail/CVE-2019-3689){: external} and [CVE-2020-8177](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-8177){: external}. |
| Worker node `drain` automation |N/A | N/A| Fixes a race condition that can cause worker node `drain` automation to fail. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.18.4_1517" caption-side="top"}

### Changelog for 1.18.4_1517, released 22 June 2020
{: #1184_1517}

The following table shows the changes that are included in the master and worker node update `1.18.4_1517`. Master patch updates are applied automatically. Worker node patch updates can be applied by updating, reloading (in classic infrastructure), or replacing (in VPC infrastructure) the worker node. For more information, see [Update types](/docs/containers?topic=containers-cs_versions#update_types).
{: shortdesc}

| Component | Location | Previous | Current | Description |
| --------- | -------- | ------- | -------- | ----------- |
| Calico | Master | v3.13.3 | v3.13.4 | See the [Calico release notes](https://docs.projectcalico.org/releases){: external}. The master update resolves CVE-2020-13597 (see the [IBM security bulletin](https://www.ibm.com/support/pages/node/6226322){: external}). |
| Cluster health image | Master | v1.1.5 | v1.1.8 | Additional status information is included when an add-on health state is `critical`. Improved performance when handling cluster status updates. |
| Cluster master operations | Master | N/A | N/A | Cluster master operations such as `refresh` or `update` are now canceled if a broken [Kubernetes admission webhook](https://kubernetes.io/docs/reference/access-authn-authz/extensible-admission-controllers/){: external} is detected. |
| etcd | Master | v3.4.7 | v3.4.9 | See the [etcd release notes](https://github.com/etcd-io/etcd/releases/v3.4.9){: external}. |
| GPU device plug-in and installer | Master | b9a418c | 2bcf8e4 | Updated image for [CVE-2020-3810](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-3810){: external}. |
| {{site.data.keyword.cloud_notm}} Controller Manager | Master | v1.18.3-1 | v1.18.4-1 | Updated to support the Kubernetes 1.18.4 release. Updated the version 2.0 private network load balancers (NLBs) to manage Calico global network policies. Updated `calicoctl` version to 3.13.4. |
| {{site.data.keyword.filestorage_full_notm}} plug-in | Master | 373 | 375 | Fixed a bug that might cause error handling to create additional persistent volumes. |
| {{site.data.keyword.cloud_notm}} RBAC operator | Master | N/A | 08ce50e | **New!**: Added a control plane operator to synchronize [{{site.data.keyword.cloud_notm}} Identity and Access Management (IAM) service access roles](/docs/containers?topic=containers-access_reference#service) with Kubernetes role-based access control (RBAC) roles. |
| Kubernetes | Both | v1.18.3 | v1.18.4 | See the [Kubernetes release notes](https://github.com/kubernetes/kubernetes/releases/tag/v1.18.4){: external}. The master update resolves CVE-2020-8558 (see the [IBM security bulletin](https://www.ibm.com/support/pages/node/6249905){: external}). |
| Kubernetes configuration | Master | N/A | N/A | The Kubernetes API server audit policy configuration is updated to include auditing the `apiextensions.k8s.io` API group and the `persistentvolumeclaims` and `persistentvolumes` resources. Additionally, the `http2-max-streams-per-connection` option is set to `1000` to mitigate network disruption impacts on the `kubelet` connection to the API server. |
| Kubernetes Dashboard | Master | v2.0.0 | v2.0.1 | See the [Kubernetes Dashboard release notes](https://github.com/kubernetes/dashboard/releases/tag/v2.0.1){: external}. |
| Load balancer and load balancer monitor for {{site.data.keyword.cloud_notm}} Provider | Master | 211 | 223 | Improved startup performance of version 2.0 private network load balancers (NLBs). |
| Ubuntu 18.04 packages | Worker | 4.15.0-101-generic | 4.15.0-106-generic | Updated worker node images with kernel and package updates for [CVE-2018-8740](https://nvd.nist.gov/vuln/detail/CVE-2018-8740){: external}, [CVE-2019-17023](https://nvd.nist.gov/vuln/detail/CVE-2019-17023){: external}, [CVE-2020-0543](https://nvd.nist.gov/vuln/detail/CVE-2020-0543){: external}, [CVE-2020-12049](https://nvd.nist.gov/vuln/detail/CVE-2020-12049){: external}, [CVE-2020-12399](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-12399){: external}, [CVE-2020-13434](https://nvd.nist.gov/vuln/detail/CVE-2020-13434){: external}, [CVE-2020-13630](https://nvd.nist.gov/vuln/detail/CVE-2020-13630){: external}, and [CVE-2020-13632](https://nvd.nist.gov/vuln/detail/CVE-2020-13632){: external}. |
| Ubuntu 16.04 packages | Worker | 4.4.0-179-generic | 4.4.0-184-generic | Updated worker node images with package and kernel updates for [CVE-2020-12049](https://nvd.nist.gov/vuln/detail/CVE-2020-12049){: external}, [CVE-2020-0543](https://nvd.nist.gov/vuln/detail/CVE-2020-0543){: external}, [CVE-2020-12769](https://nvd.nist.gov/vuln/detail/CVE-2020-12769){: external}, [CVE-2020-1749](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-1749){: external}, [CVE-2020-13434](https://nvd.nist.gov/vuln/detail/CVE-2020-13434){: external}, [CVE-2020-13630](https://nvd.nist.gov/vuln/detail/CVE-2020-13630){: external}, and[CVE-2020-13632](https://nvd.nist.gov/vuln/detail/CVE-2020-13632){: external}. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is where the component is located, the master, worker node, or both. The third column is the previous version number of the component. The fourth column is the current version number of the component. The fifth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.18.3_1515" caption-side="top"}

### Changelog for worker node fix pack 1.18.3_1515, released 8 June 2020
{: #1183_1515}

The following table shows the changes that are included in the worker node fix pack `1.18.3_1515`. Worker node patch updates can be applied by updating, reloading (in classic infrastructure), or replacing (in VPC infrastructure) the worker node.
{: shortdesc}

| Component | Previous | Current | Description |
| --------- | -------- | ------- | ----------- |
| Ubuntu 18.04 packages | N/A | N/A | Updated worker node images with package updates for [CVE-2019-1547](https://nvd.nist.gov/vuln/detail/CVE-2019-1547){: external}, [CVE-2019-1549](https://nvd.nist.gov/vuln/detail/CVE-2019-1549){: external}, [CVE-2019-1551](https://nvd.nist.gov/vuln/detail/CVE-2019-1551){: external}, [CVE-2019-1563](https://nvd.nist.gov/vuln/detail/CVE-2019-1563){: external}, and [CVE-2020-12762](https://nvd.nist.gov/vuln/detail/CVE-2020-12762){: external}. |
| Ubuntu 16.04 packages | 4.4.0-178-generic | 4.4.0-179-generic | Updated worker node images with package and kernel updates for [CVE-2019-1547](https://nvd.nist.gov/vuln/detail/CVE-2019-1547){: external}, [CVE-2019-1551](https://nvd.nist.gov/vuln/detail/CVE-2019-1551){: external}, [CVE-2019-1563](https://nvd.nist.gov/vuln/detail/CVE-2019-1563){: external}, and [CVE-2020-12762](https://nvd.nist.gov/vuln/detail/CVE-2020-12762){: external}. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.18.3_1514" caption-side="top"}

### Changelog for 1.18.3_1514, released 26 May 2020
{: #1183_1514}

The following table shows the changes that are included in the master and worker node update `1.18.3_1514`. Master patch updates are applied automatically. Worker node patch updates can be applied by updating, reloading (in classic infrastructure), or replacing (in VPC infrastructure) the worker node. For more information, see [Update types](/docs/containers?topic=containers-cs_versions#update_types).
{: shortdesc}

| Component | Location | Previous | Current | Description |
| --------- | -------- | ------- | -------- | ----------- |
| IBM Calico extension | Master | 349 | 353 | Skips creating a Calico host endpoint when no endpoint is needed. |
| {{site.data.keyword.cloud_notm}} Controller Manager | Master | v1.18.2-3 | v1.18.3-1 | Updated to support the Kubernetes 1.18.3 release. |
| {{site.data.keyword.filestorage_full_notm}} plug-in and monitor | Master | 371 | 373 | Image updated for [CVE-2020-11655](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-11655){: external}. |
| Kubernetes | Both | v1.18.2 | v1.18.3 | See the [Kubernetes release notes](https://github.com/kubernetes/kubernetes/releases/tag/v1.18.3){: external}. |
| Kubernetes Metrics Server | Master | N/A | N/A | Increased the CPU per node for the `metrics-server` container to improve availability of the metrics server API service for large clusters. |
| Kubernetes `NodeLocal` DNS cache | Master | 1.15.12 | 1.15.13 | See the [Kubernetes `NodeLocal` DNS cache release notes](https://github.com/kubernetes/dns/releases/tag/1.15.13){: external}. Updated the `node-local-dns` daemon set to include the `prometheus.io/port` and `prometheus.io/scrape` annotations on the pods. |
| Load balancer and load balancer monitor for {{site.data.keyword.cloud_notm}} Provider | Master | 207 | 211 | Updated the version 2.0 network load balancers (NLB) to clean up unnecessary IPVS rules. |
| Ubuntu 18.04 packages | Worker | 4.15.0-99-generic | 4.15.0-101-generic | Updated worker node images with kernel and package updates for [CVE-2019-20795](https://nvd.nist.gov/vuln/detail/CVE-2019-20795){: external}, [CVE-2020-11494](https://nvd.nist.gov/vuln/detail/CVE-2020-11494){: external}, [CVE-2020-12762](https://nvd.nist.gov/vuln/detail/CVE-2020-12762){: external}, [CVE-2020-3810](https://nvd.nist.gov/vuln/detail/CVE-2020-3810){: external}, [CVE-2020-8616](https://nvd.nist.gov/vuln/detail/CVE-2020-8616){: external}, and [CVE-2020-8617](https://nvd.nist.gov/vuln/detail/CVE-2020-8617){: external}. |
| Ubuntu 16.04 packages | Worker | 4.4.0-178-generic | 4.4.0-179-generic | Updated worker node images with package and kernel updates for [CVE-2019-19060](https://nvd.nist.gov/vuln/detail/CVE-2019-19060){: external}, [CVE-2020-11494](https://nvd.nist.gov/vuln/detail/CVE-2020-11494){: external}, [CVE-2020-11608](https://nvd.nist.gov/vuln/detail/CVE-2020-11608){: external}, [CVE-2020-12762](https://nvd.nist.gov/vuln/detail/CVE-2020-12762){: external}, [CVE-2020-3810](https://nvd.nist.gov/vuln/detail/CVE-2020-3810){: external}, [CVE-2020-8616](https://nvd.nist.gov/vuln/detail/CVE-2020-8616), and [CVE-2020-8617](https://nvd.nist.gov/vuln/detail/CVE-2020-8617){: external}. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is where the component is located, the master, worker node, or both. The third column is the previous version number of the component. The fourth column is the current version number of the component. The fifth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.18.2_1512" caption-side="top"}

### Changelog for 1.18.2_1512, released 11 May 2020
{: #1182_1512}

The following table shows the changes that are included in patch update 1.18.2_1512. If you update your cluster from Kubernetes 1.17, review the [preparation actions](/docs/containers?topic=containers-cs_versions#cs_v118).
{: shortdesc}

| Component | Previous | Current | Description |
| --- | --- | --- | --- |
| Calico | v3.12.1 | v3.13.3 | See the [Calico release notes](https://docs.projectcalico.org/archive/v3.13/release-notes/){: external}. |
| Cluster health image | v1.1.1 | v1.1.4 | When cluster add-ons do not support the current cluster version, a warning is now returned in the cluster health state. |
| CoreDNS configuration | N/A | N/A | To improve cluster DNS availability, CoreDNS [pods now prefer evenly distributed scheduling](https://kubernetes.io/blog/2020/05/introducing-podtopologyspread/){: external} across worker nodes and zones. |
| etcd | v3.4.3 | v3.4.7 | See the [etcd release notes](https://github.com/etcd-io/etcd/releases/v3.4.7){: external}). |
| Gateway-enabled cluster controller | 1045 | 1082 | Updated image for [CVE-2020-1967](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-1967){: external}. |
| GPU device plug-in and installer | 8c6538f | b9a418c | Updated image for [CVE-2020-1967](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-1967){: external}. |
| IBM Calico extension | 320 | 349 | Updated image for [CVE-2020-1967](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-1967){: external}. |
| {{site.data.keyword.cloud_notm}} Controller Manager | v1.17.5-1 | v1.18.2-3 | Updated to support the Kubernetes 1.18.2 release and to use `calicoctl` version 3.13.3. Updated network load balancer (NLB) events to use the latest {{site.data.keyword.cloud_notm}} troubleshooting documentation. |
| {{site.data.keyword.filestorage_full_notm}} plug-in and monitor | 358 | 371 | Updated image for [CVE-2020-1967](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-1967){: external}. |
| Kubernetes | v1.17.5 | v1.18.2 | See the [Kubernetes release notes](https://github.com/kubernetes/kubernetes/releases/tag/v1.18.2){: external}. The master update resolves CVE-2020-8555 (see the [IBM security bulletin](https://www.ibm.com/support/pages/node/6220220){: external}). |
| Kubernetes admission controllers configuration | N/A | N/A | Added `CertificateApproval`, `CertificateSigning`, `CertificateSubjectRestriction` and `DefaultIngressClass` to the `--enable-admission-plugins` option for the cluster's [Kubernetes API server](/docs/containers?topic=containers-service-settings#kube-apiserver). |
| Kubernetes configuration | N/A | N/A | Removed `batch/v2alpha1=true` from the `--runtime-config` option for the cluster's Kubernetes API server. |
| Kubernetes Dashboard | v2.0.0-rc7 | v2.0.0 | See the [Kubernetes Dashboard release notes](https://github.com/kubernetes/dashboard/releases/tag/v2.0.0){: external}. |
| Kubernetes NodeLocal DNS cache | 1.15.8 | 1.15.12 | <ul><li>See the [Kubernetes NodeLocal DNS cache release notes](https://github.com/kubernetes/dns/releases/tag/1.15.12){: external}.</li><li>[NodeLocal DNS cache](/docs/containers?topic=containers-cluster_dns#dns_cache) is now generally available, but still disabled by default.</li><li>In Kubernetes 1.18, you might also use [zone-aware DNS](/docs/containers?topic=containers-cluster_dns#dns_zone_aware) instead of just NodeLocal DNS cache, to increase DNS performance and availability in a multizone cluster.</li></ul> |
| Load balancer and load balancer monitor for {{site.data.keyword.cloud_notm}} Provider | 177 | 207 | Improved application logging. Updated image for [CVE-2020-1967](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-1967){: external}. |
| Pause container image | 3.1 | 3.2 | See the [pause container image release notes](https://github.com/kubernetes/kubernetes/blob/master/build/pause/CHANGELOG.md){: external}. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.17.5_1523" caption-side="top"}

<br />

## Deprecated: Version 1.17 changelog
{: #117_changelog}

Review the version 1.17 changelog.
{: shortdesc}

Kubernetes version 1.17 is deprecated, with a tentative unsupported date of 30 June 2021. Update your cluster to at least [version 1.18](/docs/containers?topic=containers-cs_versions#cs_v118) as soon as possible.
{: deprecated}

### Changelog for worker node fix pack 1.17.17_1559, released 12 April 2021
{: #11717_1559}

The following table shows the changes that are included in the worker node fix pack `1.17.17_1559`. Worker node patch updates can be applied by updating, reloading (in classic infrastructure), or replacing (in VPC infrastructure) the worker node.
{: shortdesc}

| Component | Previous | Current | Description |
| --------- | -------- | ------- | ----------- |
| HA proxy | 9b2dca | a3b1ff | The update addresses [CVE-2021-3449](https://nvd.nist.gov/vuln/detail/CVE-2021-3449){: external} and [CVE-2021-3450](https://nvd.nist.gov/vuln/detail/CVE-2021-3450){: external}. |
| Ubuntu 18.04 packages | N/A | N/A | Updated worker node images with package updates for [CVE-2021-22876](https://nvd.nist.gov/vuln/detail/CVE-2021-22876){: external}. |
| Ubuntu 16.04 packages | N/A | N/A | Updated worker node images with package updates for [CVE-2021-22876](https://nvd.nist.gov/vuln/detail/CVE-2021-22876){: external}.|
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.17.17_1558" caption-side="top"}

### Changelog for master fix pack 1.17.17_1557, released 30 March 2021
{: #11717_1557}

The following table shows the changes that are included in the master fix pack patch update `1.17.17_1557`. Master patch updates are applied automatically.
{: shortdesc}

| Component | Previous | Current | Description |
| --- | --- | --- | --- |
| {{site.data.keyword.at_short}} event | N/A | N/A | Now, the `containers-kubernetes.version.update` event is sent to {{site.data.keyword.at_short}} when a master fix pack update is initiated for a cluster. |
| Cluster health image | v1.1.18 | v1.1.19 | Updated image for [CVE-2020-28851](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-28851){: external}. |
| Gateway-enabled cluster controller | 1232 | 1322 | Updated to use `Go` version 1.15.10 and for [CVE-2021-23839](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-23839){: external}, [CVE-2021-23840](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-23840){: external}, and [CVE-2021-23841](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-23841){: external}. |
| GPU device plug-in | 4d3b934f | 65e4401 | Updated image for [CVE-2021-27919](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-27919){: external}, [CVE-2020-28851](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-28851){: external}, and [CVE-2020-28852](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-28852){: external}. |
| {{site.data.keyword.filestorage_full_notm}} plug-in and monitor | 388 | 389 | Updated to use `Go` version 1.15.8. |
| Load balancer and load balancer monitor for IBM Cloud Provider | 1165 | 1274 | Fixed a bug that might cause version 2.0 network load balancers (NLBs) to crash and restart on load balancer updates. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.17.17_1555" caption-side="top"}

### Changelog for worker node fix pack 1.17.17_1558, released 29 March 2021
{: #11717_1558}

The following table shows the changes that are included in the worker node fix pack `1.17.17_1558`. Worker node patch updates can be applied by updating, reloading (in classic infrastructure), or replacing (in VPC infrastructure) the worker node.
{: shortdesc}

| Component | Previous | Current | Description |
| --------- | -------- | ------- | ----------- |
| Ubuntu 18.04 packages | 4.15.0-136-generic | 4.15.0-140-generic | Updated worker node images with kernel and package updates for [CVE-2020-27170](https://nvd.nist.gov/vuln/detail/CVE-2020-27170){: external}, [CVE-2020-27171](https://nvd.nist.gov/vuln/detail/CVE-2020-27171){: external}, [CVE-2021-27363](https://nvd.nist.gov/vuln/detail/CVE-2021-27363){: external}, [CVE-2021-27365](https://nvd.nist.gov/vuln/detail/CVE-2021-27365){: external}, [CVE-2021-28153](https://nvd.nist.gov/vuln/detail/CVE-2021-28153){: external}, and [CVE-2021-3449](https://nvd.nist.gov/vuln/detail/CVE-2021-3449){: external}. |
| Ubuntu 16.04 packages | 4.4.0-203-generic | 4.4.0-206-generic | Updated worker node images with kernel and package updates for [CVE-2021-27363](https://nvd.nist.gov/vuln/detail/CVE-2021-27363){: external}, [CVE-2021-27365](https://nvd.nist.gov/vuln/detail/CVE-2021-27365){: external}, and [CVE-2021-28153](https://nvd.nist.gov/vuln/detail/CVE-2021-28153){: external}.|
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.17.17_1556" caption-side="top"}

### Changelog for worker node fix pack 1.17.17_1556, released 12 March 2021
{: #11717_1556}

The following table shows the changes that are included in the worker node fix pack `1.17.17_1556`. Worker node patch updates can be applied by updating, reloading (in classic infrastructure), or replacing (in VPC infrastructure) the worker node.
{: shortdesc}

| Component | Previous | Current | Description |
| --------- | -------- | ------- | ----------- |
| Containerd | v1.3.9 | 1.3.10 | See the [containerd release notes](https://github.com/containerd/containerd/releases/tag/v1.3.10){: external}. The update resolves CVE-2021-21334 (see the [IBM security bulletin](https://www.ibm.com/support/pages/node/6433475){: external}). |
| Ubuntu 18.04 packages | N/A | N/A | Updated worker node image with package updates for [CVE-2021-21300](https://nvd.nist.gov/vuln/detail/CVE-2021-21300){: external}, [CVE-2021-24031](https://nvd.nist.gov/vuln/detail/CVE-2021-24031){: external}, [CVE-2021-24032](https://nvd.nist.gov/vuln/detail/CVE-2021-24032){: external}, [CVE-2021-27218](https://nvd.nist.gov/vuln/detail/CVE-2021-27218){: external}, and [CVE-2021-27219](https://nvd.nist.gov/vuln/detail/CVE-2021-27219){: external}. |
| Ubuntu 16.04 packages | N/A | N/A | Updated worker node image with package updates for{: external}, [CVE-2021-21300](https://nvd.nist.gov/vuln/detail/CVE-2021-21300){: external}, [CVE-2021-27218](https://nvd.nist.gov/vuln/detail/CVE-2021-27218){: external}, [CVE-2021-27219](https://nvd.nist.gov/vuln/detail/CVE-2021-27219){: external}, and [CVE-2021-3177](https://nvd.nist.gov/vuln/detail/CVE-2021-3177){: external}. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.17.17_1555" caption-side="top"}

### Changelog for worker node fix pack 1.17.17_1555, released 1 March 2021
{: #11717_1555_worker}

The following table shows the changes that are included in the worker node fix pack `1.17.17_1555`. Worker node patch updates can be applied by updating, reloading (in classic infrastructure), or replacing (in VPC infrastructure) the worker node.
{: shortdesc}

| Component | Previous | Current | Description |
| --------- | -------- | ------- | ----------- |
| Ubuntu 18.04 packages | 4.15.0-135-generic | 4.15.0-136-generic | Updated worker node image with package updates for [CVE-2020-27619](https://nvd.nist.gov/vuln/detail/CVE-2020-27619){: external}, [CVE-2020-29372](https://nvd.nist.gov/vuln/detail/CVE-2020-29372){: external}, [CVE-2020-29374](https://nvd.nist.gov/vuln/detail/CVE-2020-29374){: external}, [CVE-2020-8625](https://nvd.nist.gov/vuln/detail/CVE-2020-8625){: external}, [CVE-2021-23840](https://nvd.nist.gov/vuln/detail/CVE-2021-23840){: external}, [CVE-2021-23841](https://nvd.nist.gov/vuln/detail/CVE-2021-23841){: external}, [CVE-2021-26937](https://nvd.nist.gov/vuln/detail/CVE-2021-26937){: external}, [CVE-2021-27212](https://nvd.nist.gov/vuln/detail/CVE-2021-27212){: external}, and [CVE-2021-3177](https://nvd.nist.gov/vuln/detail/CVE-2021-3177){: external}. |
| Ubuntu 16.04 packages | 4.4.0-201-generic | 4.4.0-203-generic | Updated worker node image with package updates for [CVE-2020-27619](https://nvd.nist.gov/vuln/detail/CVE-2020-27619){: external}, [CVE-2020-29372](https://nvd.nist.gov/vuln/detail/CVE-2020-29372){: external}, [CVE-2020-29374](https://nvd.nist.gov/vuln/detail/CVE-2020-29374){: external}, [CVE-2020-8625](https://nvd.nist.gov/vuln/detail/CVE-2020-8625){: external}, [CVE-2021-23840](https://nvd.nist.gov/vuln/detail/CVE-2021-23840){: external}, [CVE-2021-23841](https://nvd.nist.gov/vuln/detail/CVE-2021-23841){: external}, [CVE-2021-26937](https://nvd.nist.gov/vuln/detail/CVE-2021-26937){: external}, [CVE-2021-27212](https://nvd.nist.gov/vuln/detail/CVE-2021-27212){: external}, and [CVE-2021-3177](https://nvd.nist.gov/vuln/detail/CVE-2021-3177){: external}. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.17.17_1553" caption-side="top"}

### Changelog for master fix pack 1.17.17_1555, released 22 February 2021
{: #11717_1555}

The following table shows the changes that are included in the master fix pack patch update `1.17.17_1555`. Master patch updates are applied automatically.
{: shortdesc}

| Component | Previous | Current | Description |
| --- | --- | --- | --- |
| Cluster health image | v1.1.16 | v1.1.18 | Updated to use `Go` version 1.15.7. Updated image to implement additional IBM security controls. |
| Gateway-enabled cluster controller | 1195 | 1232 | Updated to use `Go` version 1.15.7. |
| IBM Calico extension | 567 | 618 | Updated to use `Go` version 1.15.7. |
| {{site.data.keyword.cloud_notm}} Controller Manager | v1.17.17-1 | v1.17.17-6 | Updated image for for [DLA-2509-1](https://www.debian.org/lts/security/2020/dla-2509){: external}. |
| {{site.data.keyword.filestorage_full_notm}} plug-in and monitor | 385 | 388 | Improved the retry logic for provisioning persistent volume claims (PVCs). |
| Key Management Service provider | v1.0.7 | v1.0.10 | Updated image for [CVE-2020-1971](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-1971){: external} and [CVE-2020-24659](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-24659){: external}. |
| Load balancer and load balancer monitor for {{site.data.keyword.cloud_notm}} Provider | 1078 | 1165 | Updated to use `Go` version 1.15.7. |
| Operator Lifecycle Manager | 0.14.1-IKS-2 | 0.14.1-IKS-4 | Updated image for [CVE-2021-23839](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-23839){: external}, [CVE-2021-23840](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-23840){: external}, and [CVE-2021-23841](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-23841){: external}. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.17.17_1551" caption-side="top"}

### Changelog for worker node fix pack 1.17.17_1553, released 15 February 2021
{: #11717_1553}

The following table shows the changes that are included in the worker node fix pack `1.17.17_1553`. Worker node patch updates can be applied by updating, reloading (in classic infrastructure), or replacing (in VPC infrastructure) the worker node.
{: shortdesc}

| Component | Previous | Current | Description |
| --------- | -------- | ------- | ----------- |
| Ubuntu 18.04 packages | N/A | N/A | Updated worker node image with package updates for [CVE-2020-36221](https://nvd.nist.gov/vuln/detail/CVE-2020-36221){: external}, [CVE-2020-36222](https://nvd.nist.gov/vuln/detail/CVE-2020-36222){: external}, [CVE-2020-36223](https://nvd.nist.gov/vuln/detail/CVE-2020-36223){: external}, [CVE-2020-36224](https://nvd.nist.gov/vuln/detail/CVE-2020-36224){: external}, [CVE-2020-36225](https://nvd.nist.gov/vuln/detail/CVE-2020-36225){: external}, [CVE-2020-36226](https://nvd.nist.gov/vuln/detail/CVE-2020-36226){: external}, [CVE-2020-36227](https://nvd.nist.gov/vuln/detail/CVE-2020-36227){: external}, [CVE-2020-36228](https://nvd.nist.gov/vuln/detail/CVE-2020-36228){: external}, [CVE-2020-36229](https://nvd.nist.gov/vuln/detail/CVE-2020-36229){: external}, [CVE-2020-36230](https://nvd.nist.gov/vuln/detail/CVE-2020-36230){: external}, [CVE-2021-25682](https://nvd.nist.gov/vuln/detail/CVE-2021-25682){: external}, [CVE-2021-25683](https://nvd.nist.gov/vuln/detail/CVE-2021-25683){: external}, and [CVE-2021-25684](https://nvd.nist.gov/vuln/detail/CVE-2021-25684){: external}. |
| Ubuntu 16.04 packages | N/A | N/A | Updated worker node image with package updates for [CVE-2020-36221](https://nvd.nist.gov/vuln/detail/CVE-2020-36221){: external}, [CVE-2020-36222](https://nvd.nist.gov/vuln/detail/CVE-2020-36222){: external}, [CVE-2020-36223](https://nvd.nist.gov/vuln/detail/CVE-2020-36223){: external}, [CVE-2020-36224](https://nvd.nist.gov/vuln/detail/CVE-2020-36224){: external}, [CVE-2020-36225](https://nvd.nist.gov/vuln/detail/CVE-2020-36225){: external}, [CVE-2020-36226](https://nvd.nist.gov/vuln/detail/CVE-2020-36226){: external}, [CVE-2020-36227](https://nvd.nist.gov/vuln/detail/CVE-2020-36227){: external}, [CVE-2020-36228](https://nvd.nist.gov/vuln/detail/CVE-2020-36228){: external}, [CVE-2020-36229](https://nvd.nist.gov/vuln/detail/CVE-2020-36229){: external}, [CVE-2020-36230](https://nvd.nist.gov/vuln/detail/CVE-2020-36230){: external}, [CVE-2021-25682](https://nvd.nist.gov/vuln/detail/CVE-2021-25682){: external}, [CVE-2021-25683](https://nvd.nist.gov/vuln/detail/CVE-2021-25683){: external}, and [CVE-2021-25684](https://nvd.nist.gov/vuln/detail/CVE-2021-25684){: external}. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.17.17_1552" caption-side="top"}
### Changelog for worker node fix pack 1.17.17_1552, released 1 February 2021
{: #11717_1552}

The following table shows the changes that are included in the worker node fix pack `1.17.17_1552`. Worker node patch updates can be applied by updating, reloading (in classic infrastructure), or replacing (in VPC infrastructure) the worker node.
{: shortdesc}

| Component | Previous | Current | Description |
| --------- | -------- | ------- | ----------- |
| Ubuntu 16.04 packages | 4.4.0-200-generic | 4.4.0-201-generic | Updated worker node image with kernel and package updates for [CVE-2019-14834](https://nvd.nist.gov/vuln/detail/CVE-2019-14834){: external}, [CVE-2020-25681](https://nvd.nist.gov/vuln/detail/CVE-2020-25681){: external}, [CVE-2020-25682](https://nvd.nist.gov/vuln/detail/CVE-2020-25682){: external}, [CVE-2020-25683](https://nvd.nist.gov/vuln/detail/CVE-2020-25683){: external}, [CVE-2020-25684](https://nvd.nist.gov/vuln/detail/CVE-2020-25684){: external}, [CVE-2020-25685](https://nvd.nist.gov/vuln/detail/CVE-2020-25685){: external}, [CVE-2020-25686](https://nvd.nist.gov/vuln/detail/CVE-2020-25686){: external}, [CVE-2020-25687](https://nvd.nist.gov/vuln/detail/CVE-2020-25687){: external}, [CVE-2020-27777](https://nvd.nist.gov/vuln/detail/CVE-2020-27777){: external}, [CVE-2021-23239](https://nvd.nist.gov/vuln/detail/CVE-2021-23239){: external}, and [CVE-2021-3156](https://nvd.nist.gov/vuln/detail/CVE-2021-3156){: external}. |
| Ubuntu 18.04 packages | 4.15.0-132-generic | 4.15.0-135-generic | Updated worker node image with kernel and package updates for [CVE-2019-12761](https://nvd.nist.gov/vuln/detail/CVE-2019-12761){: external}, [CVE-2019-14834](https://nvd.nist.gov/vuln/detail/CVE-2019-14834){: external}, [CVE-2020-25681](https://nvd.nist.gov/vuln/detail/CVE-2020-25681){: external}, [CVE-2020-25682](https://nvd.nist.gov/vuln/detail/CVE-2020-25682){: external}, [CVE-2020-25683](https://nvd.nist.gov/vuln/detail/CVE-2020-25683){: external}, [CVE-2020-25684](https://nvd.nist.gov/vuln/detail/CVE-2020-25684){: external}, [CVE-2020-25685](https://nvd.nist.gov/vuln/detail/CVE-2020-25685){: external}, [CVE-2020-25686](https://nvd.nist.gov/vuln/detail/CVE-2020-25686){: external}, [CVE-2020-25687](https://nvd.nist.gov/vuln/detail/CVE-2020-25687){: external}, [CVE-2020-27777](https://nvd.nist.gov/vuln/detail/CVE-2020-27777){: external}, [CVE-2021-23239](https://nvd.nist.gov/vuln/detail/CVE-2021-23239){: external}, and [CVE-2021-3156](https://nvd.nist.gov/vuln/detail/CVE-2021-3156){: external}. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.17.17_1551" caption-side="top"}

### Changelog for master fix pack 1.17.17_1551, released 19 January 2021
{: #11717_1551_master}

The following table shows the changes that are included in the master fix pack patch update `1.17.17_1551`. Master patch updates are applied automatically.
{: shortdesc}

| Component | Previous | Current | Description |
| --- | --- | --- | --- |
| Cluster health image | v1.1.13 | v1.1.16 | Updated image to implement additional IBM security controls. |
| Gateway-enabled cluster controller | 1184 | 1195 | Updated image for [CVE-2020-1971](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-1971){: external}. |
| GPU device plug-in and installer | adcae42 | 4d3b934f | Updated image for [CVE-2021-3114](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-3114){: external}, [CVE-2021-3115](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-3115){: external}, [CVE-2020-27350](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-27350){: external}, [CVE-2020-29361](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-29361){: external}, [CVE-2020-29362](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-29362){: external}, [CVE-2020-29363](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-29363){: external}, [CVE-2020-1971](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-1971){: external}, [CVE-2020-8231](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-8231){: external}, [CVE-2020-8284](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-8284){: external}, [CVE-2020-8285](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-8285){: external}, and [CVE-2020-8286](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-8286){: external}. |
| IBM Calico extension | 556 | 567 | Updated image for [CVE-2020-1971](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-1971){: external} and [CVE-2020-24659](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-24659){: external}. |
| {{site.data.keyword.cloud_notm}} Controller Manager | v1.17.16-1 | v1.17.17-1 | Updated to support the Kubernetes 1.17.17 release. |
| {{site.data.keyword.filestorage_full_notm}} plug-in and monitor | 384 | 385 | Updated image for [CVE-2020-1971](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-1971){: external} and [CVE-2020-24659](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-24659){: external}. |
| Key Management Service provider | v1.0.5 | v1.0.7 | Fixed bug to ignore conflict errors during KMS secret reencrpytion. Updated to use `Go` verison 1.15.5. Updated image for [CVE-2020-1971](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-1971){: external}. |
| Kubernetes | v1.17.16 | v1.17.17 | See the [Kubernetes release notes](https://github.com/kubernetes/kubernetes/releases/tag/v1.17.17){: external}.  Updated to implement additional IBM security controls. |
| Kubernetes Dashboard | v2.0.4 | v2.0.5 | See the [Kubernetes Dashboard release notes](https://github.com/kubernetes/dashboard/releases/tag/v2.0.5){: external}. |
| Kubernetes Dashboard metrics scraper | v1.0.4 | v1.0.6 | See the [Kubernetes Dashboard metrics scraper release notes](https://github.com/kubernetes-sigs/dashboard-metrics-scraper/releases/tag/v1.0.6){: external}. |
| Load balancer and load balancer monitor for {{site.data.keyword.cloud_notm}} Provider | 1004 | 1078 | Updated image for [CVE-2020-1971](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-1971){: external}. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.17.16_1550" caption-side="top"}

### Changelog for worker node fix pack 1.17.17_1551, released 18 January 2021
{: #11717_1551}

The following table shows the changes that are included in the worker node fix pack `1.17.17_1551`. Worker node patch updates can be applied by updating, reloading (in classic infrastructure), or replacing (in VPC infrastructure) the worker node.
{: shortdesc}

| Component | Previous | Current | Description |
| --------- | -------- | ------- | ----------- |
| Kubernetes | v1.17.15 | v1.17.17 | See the [Kubernetes release notes](https://github.com/kubernetes/kubernetes/releases/tag/v1.17.17){: external}. |
| Ubuntu 16.04 packages | 4.4.0-197-generic | 4.4.0-200-generic | Updated worker node image with kernel and package updates for [CVE-2018-20482](https://nvd.nist.gov/vuln/detail/CVE-2018-20482){: external}, [CVE-2019-9923](https://nvd.nist.gov/vuln/detail/CVE-2019-9923){: external}, [CVE-2020-28374](https://nvd.nist.gov/vuln/detail/CVE-2020-28374){: external}, [CVE-2020-29361](https://nvd.nist.gov/vuln/detail/CVE-2020-29361) external}, and [CVE-2020-29362](https://nvd.nist.gov/vuln/detail/CVE-2020-29362) external}. |
| Ubuntu 18.04 packages | 4.15.0-128-generic | 4.15.0-132-generic | Updated worker node image with kernel and package updates for [CVE-2018-20482](https://nvd.nist.gov/vuln/detail/CVE-2018-20482){: external}, [CVE-2019-9923](https://nvd.nist.gov/vuln/detail/CVE-2019-9923){: external}, [CVE-2020-28374](https://nvd.nist.gov/vuln/detail/CVE-2020-28374){: external}, [CVE-2020-29361](https://nvd.nist.gov/vuln/detail/CVE-2020-29361){: external}, [CVE-2020-29362](https://nvd.nist.gov/vuln/detail/CVE-2020-29362){: external}, [CVE-2020-29363](https://nvd.nist.gov/vuln/detail/CVE-2020-29363){: external}, [CVE-2021-1052](https://nvd.nist.gov/vuln/detail/CVE-2021-1052){: external}, [CVE-2021-1053](https://nvd.nist.gov/vuln/detail/CVE-2021-1053){: external}, and [CVE-2019-19770](https://nvd.nist.gov/vuln/detail/CVE-2019-19770){: external}. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.17.15_1549" caption-side="top"}

### Changelog for master fix pack 1.17.16_1550, released 6 January 2021
{: #11716_1550}

The following table shows the changes that are included in the master fix pack patch update `1.17.16_1550`. Master patch updates are applied automatically.
{: shortdesc}

| Component | Previous | Current | Description |
| --- | --- | --- | --- |
| IBM Calico extension | 544 | 556 | Updated image to include the `ip` command. |
| {{site.data.keyword.cloud_notm}} Controller Manager | v1.17.15-1 | v1.17.16-1 | Updated to support the Kubernetes 1.17.16 release. |
| {{site.data.keyword.filestorage_full_notm}} plug-in | N/A | N/A | Updated to run with a privileged security context. |
| Kubernetes | v1.17.15 | v1.17.16 | See the [Kubernetes release notes](https://github.com/kubernetes/kubernetes/releases/tag/v1.17.16){: external}. |
| Operator Lifecycle Manager | 0.14.1-IKS-1 | 0.14.1-IKS-2 | Updated image for [CVE-2020-1971](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-1971){: external} and [CVE-2020-28928](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-28928){: external}. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.17.15_1548" caption-side="top"}
### Changelog for worker node fix pack 1.17.15_1549, released 21 December 2020
{: #11715_1549}

The following table shows the changes that are included in the worker node fix pack `1.17.15_1549`. Worker node patch updates can be applied by updating, reloading (in classic infrastructure), or replacing (in VPC infrastructure) the worker node.
{: shortdesc}

| Component | Previous | Current | Description |
| --------- | -------- | ------- | ----------- |
| Ubuntu 16.04 packages | N/A | N/A | Updated worker node image with package updates for: [CVE-2020-1971](https://nvd.nist.gov/vuln/detail/CVE-2020-1971){: external}, [CVE-2020-27350](https://nvd.nist.gov/vuln/detail/CVE-2020-27350){: external}, [CVE-2020-27351](https://nvd.nist.gov/vuln/detail/CVE-2020-27351){: external}, [CVE-2020-8284](https://nvd.nist.gov/vuln/detail/CVE-2020-8284){: external}, [CVE-2020-8285](https://nvd.nist.gov/vuln/detail/CVE-2020-8285){: external}, and [CVE-2020-8286](https://nvd.nist.gov/vuln/detail/CVE-2020-8286){: external}. |
| Ubuntu 18.04 packages | 4.15.0-126-generic | 4.15.0-128-generic | Updated worker node image with kernel and package updates for: [CVE-2020-1971](https://nvd.nist.gov/vuln/detail/CVE-2020-1971){: external}, [CVE-2020-27350](https://nvd.nist.gov/vuln/detail/CVE-2020-27350){: external}, [CVE-2020-27351](https://nvd.nist.gov/vuln/detail/CVE-2020-27351){: external}, [CVE-2020-8284](https://nvd.nist.gov/vuln/detail/CVE-2020-8284){: external}, [CVE-2020-8285](https://nvd.nist.gov/vuln/detail/CVE-2020-8285){: external}, and [CVE-2020-8286](https://nvd.nist.gov/vuln/detail/CVE-2020-8286){: external}. |
| Kubernetes | v1.17.14 | v1.17.15 | See the [Kubernetes changelogs.](https://github.com/kubernetes/kubernetes/releases/tag/v1.17.15){: external} |
| Ephemeral storage reservations | N/A | N/A | Reserve local ephermal storage to prevent workload evictions. |
| HAProxy | db4e6d | 9b2dca | Image update for [CVE-2020-1971](https://nvd.nist.gov/vuln/detail/CVE-2020-1971){: external} and [CVE-2020-24659](https://nvd.nist.gov/vuln/detail/CVE-2020-24659){: external}. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.17.15_1548" caption-side="top"}

### Changelog for master fix pack 1.17.15_1548, released 14 December 2020
{: #11715_1548}

The following table shows the changes that are included in the master fix pack patch update `1.17.15_1548`. Master patch updates are applied automatically.
{: shortdesc}

| Component | Previous | Current | Description |
| --- | --- | --- | --- |
| CoreDNS | 1.6.9 | 1.8.0 | See the [CoreDNS release notes](https://coredns.io/2020/10/22/coredns-1.8.0-release/){: external}. Additionally, updated the CoreDNS configuration to increase the weight of scheduling CoreDNS pods to different worker nodes and zones. |
| etcd | v3.4.13 | v3.4.14 | See the [etcd release notes](https://github.com/etcd-io/etcd/releases/v3.4.14){: external}. |
| Gateway-enabled cluster controller | 1105 | 1184 | Updated to use `Go` version 1.15.5. Updated image to implement additional IBM security controls. |
| GPU device plug-in and installer | c7a8cf7 | adcae42 | Updated the GPU drivers to version [450.80.02](https://www.nvidia.com/download/driverResults.aspx/165294){: external}. Updated image for [CVE-2020-28367](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-28367){: external}, [CVE-2020-28366](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-28366){: external}, and [CVE-2020-28362](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-28362){: external}. Updated image to implement additional IBM security controls. |
| IBM Calico extension | 378 | 544 | Updated to use the universal base image (UBI) and to use `Go` version 1.15.5. Updated image to implement additional IBM security controls. |
| {{site.data.keyword.cloud_notm}} Controller Manager | v1.17.14-1 | v1.17.15-1 | Updated to support the Kubernetes 1.17.15 release. Updated image to implement additional IBM security controls. |
| {{site.data.keyword.filestorage_full_notm}} monitor | 379 | 384 | Updated to use `Go` version 1.15.5 and to run as a non-root user. Updated image to implement additional IBM security controls. |
| {{site.data.keyword.filestorage_full_notm}} plug-in | 379 | 384 | Updated to use `Go` version 1.15.5 and to run with a least privileged security context. Updated image to implement additional IBM security controls. |
| Key management service (KMS) provider | v1.0.4 | v1.0.5 | Updated image to implement additional IBM security controls. |
| Kubernetes | v1.17.14 | v1.17.15 | See the [Kubernetes release notes](https://github.com/kubernetes/kubernetes/releases/tag/v1.17.15){: external}. |
| Kubernetes `NodeLocal` DNS cache | N/A | N/A | Updated the `NodeLocal` DNS cache configuration to support [customizing the `node-local-dns` config map](/docs/containers?topic=containers-cluster_dns#dns_nodelocal_customize). |
| Load balancer and load balancer monitor for {{site.data.keyword.cloud_notm}} Provider | 208 | 1004 | Updated Alpine base image to version 3.12 and to use `Go` version 1.15.5. Updated image for [CVE-2020-8037](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-8037){: external} and [CVE-2020-28928](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-28928){: external}. Updated image to implement additional IBM security controls. |
| OpenVPN client | 2.4.6-r3-IKS-116 | 2.4.6-r3-IKS-301 | Updated image to implement additional IBM security controls. |
| OpenVPN server | 2.4.6-r3-IKS-222 | 2.4.6-r3-IKS-301 | Updated image to implement additional IBM security controls. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.17.14_1545" caption-side="top"}

### Changelog for worker node fix pack 1.17.14_1548, released 11 December 2020
{: #11714_1548}

The following table shows the changes that are included in the worker node fix pack `1.17.14_1548`. Worker node patch updates can be applied by updating, reloading (in classic infrastructure), or replacing (in VPC infrastructure) the worker node.
{: shortdesc}

| Component | Previous | Current | Description |
| --------- | -------- | ------- | ----------- |
| Ubuntu 18.04 bare metal kernel | 4.15.0-126-generic | 4.15.0-123-generic | **Bare metal worker nodes**: Reverted the kernel version for bare metal worker nodes while Canonical addresses issues with the previous version that prevented worker nodes from being reloaded or updated. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.17.14_1546" caption-side="top"}

### Changelog for worker node fix pack 1.17.14_1546, released 7 December 2020
{: #11714_1546}

The following table shows the changes that are included in the worker node fix pack `1.17.14_1546`. Worker node patch updates can be applied by updating, reloading (in classic infrastructure), or replacing (in VPC infrastructure) the worker node.
{: shortdesc}

| Component | Previous | Current | Description |
| --------- | -------- | ------- | ----------- |
| Containerd | v1.3.4 | 1.3.9 | See the [containerd release notes](https://github.com/containerd/containerd/releases/tag/v1.3.9){: external}. The update resolves CVE-2020–15257 (see the [IBM security bulletin](https://www.ibm.com/support/pages/node/6387878){: external}). |
| HAProxy | 1.8.26-384f42 | db4e6d | Added provenance labels for source tracking. |
| Ubuntu 18.04 packages | 4.15.0-123-generic | 4.15.0-126-generic | Updated worker node image with   kernel and package updates for [CVE-2020-14351](https://nvd.nist.gov/vuln/detail/CVE-2020-14351){: external} and [CVE-2020-4788](https://nvd.nist.gov/vuln/detail/CVE-2020-4788){: external}. |
| Ubuntu 16.04 packages | 4.4.0-194-generic | 4.4.0-197-generic | Updated worker node image with kernel and package updates for [CVE-2020-0427](https://nvd.nist.gov/vuln/detail/CVE-2020-0427){: external}, [CVE-2020-12352](https://nvd.nist.gov/vuln/detail/CVE-2020-12352){: external}, [CVE-2020-14351](https://nvd.nist.gov/vuln/detail/CVE-2020-14351){: external}, [CVE-2020-25645](https://nvd.nist.gov/vuln/detail/CVE-2020-25645){: external}, and [CVE-2020-4788](https://nvd.nist.gov/vuln/detail/CVE-2020-4788){: external}. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.17.14_1545" caption-side="top"}

### Changelog for worker node fix pack 1.17.14_1545, released 23 November 2020
{: #11714_1545_worker}

The following table shows the changes that are included in the worker node fix pack `1.17.14_1545`. Worker node patch updates can be applied by updating, reloading (in classic infrastructure), or replacing (in VPC infrastructure) the worker node.
{: shortdesc}

| Component | Previous | Current | Description |
| --------- | -------- | ------- | ----------- |
| Kubernetes | v1.17.13 | v1.17.14 | See the [Kubernetes changelog](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG/CHANGELOG-1.17.md#v11714){: external}.|
| Ubuntu 18.04 packages | 4.15.0-122-generic | 4.15.0-123-generic | Updated worker node image with kernel and package updates for [CVE-2020-25692](https://nvd.nist.gov/vuln/detail/CVE-2020-25692){: external}, [CVE-2020-25709](https://nvd.nist.gov/vuln/detail/CVE-2020-25709){: external}, [CVE-2020-25710](https://nvd.nist.gov/vuln/detail/CVE-2020-25710){: external}, [CVE-2020-28196](https://nvd.nist.gov/vuln/detail/CVE-2020-28196){: external}, and [CVE-2020-8694](https://nvd.nist.gov/vuln/detail/CVE-2020-8694){: external}. |
| Ubuntu 16.04 packages | 4.4.0-193-generic | 4.4.0-194-generic | Updated worker node image with kernel and package updates for [CVE-2020-25692](https://nvd.nist.gov/vuln/detail/CVE-2020-25692){: external}, [CVE-2020-25709](https://nvd.nist.gov/vuln/detail/CVE-2020-25709){: external}, [CVE-2020-25710](https://nvd.nist.gov/vuln/detail/CVE-2020-25710){: external}, [CVE-2020-28196](https://nvd.nist.gov/vuln/detail/CVE-2020-28196){: external}, and [CVE-2020-8694](https://nvd.nist.gov/vuln/detail/CVE-2020-8694){: external}. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.17.13_1544" caption-side="top"}

### Changelog for master fix pack 1.17.14_1545, released 16 November 2020
{: #11714_1545}

The following table shows the changes that are included in the master fix pack patch update `1.17.14_1545`. Master patch updates are applied automatically.
{: shortdesc}

| Component | Previous | Current | Description |
| --- | --- | --- | --- |
| Cluster health image | v1.1.12 | v1.1.13 | Updated image for [DLA-2424-1](https://www.debian.org/lts/security/2020/dla-2424){: external}. |
| GPU device plug-in and installer | edd26a4 | c7a8cf7 | Updated image for [CVE-2019-20386](https://nvd.nist.gov/vuln/detail/CVE-2019-20386){: external}, [CVE-2019-13050](https://nvd.nist.gov/vuln/detail/CVE-2019-13050){: external}, [CVE-2020-8177](https://nvd.nist.gov/vuln/detail/CVE-2020-8177){: external}, [CVE-2019-14889](https://nvd.nist.gov/vuln/detail/CVE-2019-14889){: external}, [CVE-2020-1730](https://nvd.nist.gov/vuln/detail/CVE-2020-1730){: external}, [CVE-2020-10029](https://nvd.nist.gov/vuln/detail/CVE-2020-10029){: external}, [CVE-2020-1751](https://nvd.nist.gov/vuln/detail/CVE-2020-1751){: external}, [CVE-2020-1752](https://nvd.nist.gov/vuln/detail/CVE-2020-1752){: external}, [CVE-2019-16168](https://nvd.nist.gov/vuln/detail/CVE-2019-16168){: external}, [CVE-2019-20218](https://nvd.nist.gov/vuln/detail/CVE-2019-20218){: external}, [CVE-2019-5018](https://nvd.nist.gov/vuln/detail/CVE-2019-5018){: external}, [CVE-2020-13630](https://nvd.nist.gov/vuln/detail/CVE-2020-13630){: external}, [CVE-2020-13631](https://nvd.nist.gov/vuln/detail/CVE-2020-13631){: external}, [CVE-2020-13632](https://nvd.nist.gov/vuln/detail/CVE-2020-13632){: external}, [CVE-2020-6405](https://nvd.nist.gov/vuln/detail/CVE-2020-6405){: external}, [CVE-2020-9327](https://nvd.nist.gov/vuln/detail/CVE-2020-9327){: external}, [CVE-2019-1551](https://nvd.nist.gov/vuln/detail/CVE-2019-1551){: external}, [CVE-2019-19221](https://nvd.nist.gov/vuln/detail/CVE-2019-19221){: external}, [CVE-2019-16935](https://nvd.nist.gov/vuln/detail/CVE-2019-16935){: external}, [CVE-2019-20907](https://nvd.nist.gov/vuln/detail/CVE-2019-20907){: external}, [CVE-2020-14422](https://nvd.nist.gov/vuln/detail/CVE-2020-14422){: external}, [CVE-2020-8492](https://nvd.nist.gov/vuln/detail/CVE-2020-8492){: external}, [CVE-2019-19906](https://nvd.nist.gov/vuln/detail/CVE-2019-19906){: external}, [CVE-2019-20454](https://nvd.nist.gov/vuln/detail/CVE-2019-20454){: external}, [CVE-2019-19956](https://nvd.nist.gov/vuln/detail/CVE-2019-19956){: external}, [CVE-2019-20388](https://nvd.nist.gov/vuln/detail/CVE-2019-20388){: external}, [CVE-2020-7595](https://nvd.nist.gov/vuln/detail/CVE-2020-7595){: external}, [CVE-2019-13627](https://nvd.nist.gov/vuln/detail/CVE-2019-13627){: external}, [CVE-2018-20843](https://nvd.nist.gov/vuln/detail/CVE-2018-20843){: external}, [CVE-2019-15903](https://nvd.nist.gov/vuln/detail/CVE-2019-15903){: external}, and [CVE-2019-20387](https://nvd.nist.gov/vuln/detail/CVE-2019-20387){: external}. |
| {{site.data.keyword.cloud_notm}} Controller Manager | v1.17.13-1 | v1.17.14-1 | Updated to support the Kubernetes 1.17.14 release. Updated image for [DLA-2424-1](https://www.debian.org/lts/security/2020/dla-2424){: external}. |
| {{site.data.keyword.filestorage_full_notm}} plug-in and monitor | 378 | 379 | Updated to use the universal base image (UBI) and to use `Go` version 1.15.2. |
| Key Management Service provider | v1.0.3 | v1.0.4 | Updated image for [DLA-2424-1](https://www.debian.org/lts/security/2020/dla-2424){: external}. |
| Kubernetes | v1.17.13 | v1.17.14 | See the [Kubernetes release notes](https://github.com/kubernetes/kubernetes/releases/tag/v1.17.14){: external}. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.17.13_1543" caption-side="top"}

### Changelog for worker node fix pack 1.17.13_1544, released 9 November 2020
{: #11713_1544}

The following table shows the changes that are included in the worker node fix pack `1.17.13_1544`. Worker node patch updates can be applied by updating, reloading (in classic infrastructure), or replacing (in VPC infrastructure) the worker node.
{: shortdesc}

| Component | Previous | Current | Description |
| --------- | -------- | ------- | ----------- |
|Ubuntu 18.04 packages | N/A | N/A | Updated worker node image with kernal and package updates for [CVE-2018-14036](https://nvd.nist.gov/vuln/detail/CVE-2018-14036){: external}, [CVE-2020-10543](https://nvd.nist.gov/vuln/detail/CVE-2020-10543){: external}, [CVE-2020-10878](https://nvd.nist.gov/vuln/detail/CVE-2020-10878){: external}, [CVE-2020-12723](https://nvd.nist.gov/vuln/detail/CVE-2020-12723){: external}, [CVE-2020-16126](https://nvd.nist.gov/vuln/detail/CVE-2020-16126){: external}, [CVE-2020-25659](https://nvd.nist.gov/vuln/detail/CVE-2020-25659){: external}, and [CVE-2017-18269](https://nvd.nist.gov/vuln/detail/CVE-2017-18269){: external}. |
|Ubuntu 16.04 packages | N/A | N/A | Updated worker node image with package updates for [CVE-2018-14036](https://nvd.nist.gov/vuln/detail/CVE-2018-14036){: external}, [CVE-2020-10543](https://nvd.nist.gov/vuln/detail/CVE-2020-10543){: external}, [CVE-2020-10878](https://nvd.nist.gov/vuln/detail/CVE-2020-10878){: external}, [CVE-2020-12723](https://nvd.nist.gov/vuln/detail/CVE-2020-12723){: external}, [CVE-2020-16126](https://nvd.nist.gov/vuln/detail/CVE-2020-16126){: external}, and [CVE-2020-25659](https://nvd.nist.gov/vuln/detail/CVE-2020-25659){: external}.|
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.17.13_1543" caption-side="top"}

### Changelog for worker node fix pack 1.17.13_1543, released 26 October 2020
{: #11713_1543_worker}

The following table shows the changes that are included in the worker node fix pack `1.17.13_1543`. Worker node patch updates can be applied by updating, reloading (in classic infrastructure), or replacing (in VPC infrastructure) the worker node.
{: shortdesc}

| Component | Previous | Current | Description |
| --------- | -------- | ------- | ----------- |
| Kubernetes | v1.17.12 | v1.17.13 | See the [Kubernetes release notes](https://github.com/kubernetes/kubernetes/releases/tag/v1.17.13){: external}. |
|Ubuntu 18.04 packages | 4.15.0-118-generic | 4.15.0-122-generic | Updated worker node images with kernal and package updates for [CVE-2018-10322](https://nvd.nist.gov/vuln/detail/CVE-2018-10322){: external},[CVE-2019-20807](https://nvd.nist.gov/vuln/detail/CVE-2019-20807){: external}, [CVE-2019-20916](https://nvd.nist.gov/vuln/detail/CVE-2019-20916){: external}, [CVE-2020-12351](https://nvd.nist.gov/vuln/detail/CVE-2020-12351){: external}, [CVE-2020-12352](https://nvd.nist.gov/vuln/detail/CVE-2020-12352){: external}, [CVE-2020-15999](https://nvd.nist.gov/vuln/detail/CVE-2020-15999){: external}, [CVE-2020-16119](https://nvd.nist.gov/vuln/detail/CVE-2020-16119){: external}, [CVE-2020-16120](https://nvd.nist.gov/vuln/detail/CVE-2020-16120){: external}, [CVE-2020-24490](https://nvd.nist.gov/vuln/detail/CVE-2020-24490){: external}, and [CVE-2020-26116](https://nvd.nist.gov/vuln/detail/CVE-2020-26116){: external}. |
|Ubuntu 16.04 packages | 4.4.0-190-generic | 4.4.0-193-generic | Updated worker node images with kernel and package updates for [CVE-2017-17087](https://nvd.nist.gov/vuln/detail/CVE-2017-17087){: external}, [CVE-2018-10322](https://nvd.nist.gov/vuln/detail/CVE-2018-10322){: external}, [CVE-2019-20807](https://nvd.nist.gov/vuln/detail/CVE-2019-20807){: external}, [CVE-2020-15999](https://nvd.nist.gov/vuln/detail/CVE-2020-15999){: external}, [CVE-2020-16119](https://nvd.nist.gov/vuln/detail/CVE-2020-16119){: external}, and [CVE-2020-26116](https://nvd.nist.gov/vuln/detail/CVE-2020-26116){: external}. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.17.12_1542" caption-side="top"}

### Changelog for master fix pack 1.17.13_1543, released 26 October 2020
{: #11713_1543}

The following table shows the changes that are included in the master fix pack patch update `1.17.13_1543`. Master patch updates are applied automatically.
{: shortdesc}

| Component | Previous | Current | Description |
| --- | --- | --- | --- |
| Calico configuration | N/A | N/A | Updated the `calico-node` daemon set in the `kube-system` namespace to set the `spec.updateStrategy.rollingUpdate.maxUnavailable` parameter to `10%` for clusters with more than 50 worker nodes. |
| Cluster health image | v1.1.11 | v1.1.12 | Updated to use `Go` version 1.15.2. |
| Gateway-enabled cluster controller | 1082 | 1105 | Updated to use `Go` version 1.15.2. |
| {{site.data.keyword.cloud_notm}} Controller Manager | v1.17.12-1 | v1.17.13-1 | Updated to support the Kubernetes 1.17.13 release. |
| Kubernetes | v1.17.12 | v1.17.13 | See the [Kubernetes release notes](https://github.com/kubernetes/kubernetes/releases/tag/v1.17.13){: external}. |
| Kubernetes `NodeLocal` DNS cache | 1.15.13 | 1.15.14 | See the [Kubernetes `NodeLocal` DNS cache release notes](https://github.com/kubernetes/dns/releases/tag/1.15.14){: external}. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.17.12_1540" caption-side="top"}

### Changelog for worker node fix pack 1.17.12_1542, released 12 October 2020
{: #11712_1542}

The following table shows the changes that are included in the worker node fix pack `1.17.12_1542`. Worker node patch updates can be applied by updating, reloading (in classic infrastructure), or replacing (in VPC infrastructure) the worker node.
{: shortdesc}

| Component | Previous | Current | Description |
| --------- | -------- | ------- | ----------- |
|Ubuntu 18.04 packages | N/A | N/A | Updated worker node image with package updates for [CVE-2019-8936](https://nvd.nist.gov/vuln/detail/CVE-2019-8936){: external}, [CVE-2020-26137](https://nvd.nist.gov/vuln/detail/CVE-2020-26137){: external}, and [CVE-2020-14365](https://nvd.nist.gov/vuln/detail/CVE-2020-14365){: external}.|
|Ubuntu 16.04 packages | N/A | N/A | Updated worker node image with package updates for [CVE-2020-14365](https://nvd.nist.gov/vuln/detail/CVE-2020-14365){: external} and [CVE-2020-26137](https://nvd.nist.gov/vuln/detail/CVE-2020-26137){: external}. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.17.12_1541" caption-side="top"}

### Changelog for worker node fix pack 1.17.12_1541, released 28 September 2020
{: #11712_1541}

The following table shows the changes that are included in the worker node fix pack `1.17.12_1541`. Worker node patch updates can be applied by updating, reloading (in classic infrastructure), or replacing (in VPC infrastructure) the worker node.
{: shortdesc}

| Component | Previous | Current | Description |
| --------- | -------- | ------- | ----------- |
| Kubernetes | v1.17.11	| v1.17.12 | See the [Kubernetes release notes](https://github.com/kubernetes/kubernetes/releases/tag/v1.17.12){: external}. |
| Ubuntu 18.04 packages | 4.15.0-117-generic | 4.15.0-118-generic | Updated worker node image with   kernel and package updates for [CVE-2018-1000500](https://nvd.nist.gov/vuln/detail/CVE-2018-1000500){: external}, [CVE-2018-7738](https://nvd.nist.gov/vuln/detail/CVE-2018-7738){: external}, [CVE-2019-14855](https://nvd.nist.gov/vuln/detail/CVE-2019-14855){: external}, [CVE-2019-1547](https://nvd.nist.gov/vuln/detail/CVE-2019-1547){: external}, [CVE-2019-1551](https://nvd.nist.gov/vuln/detail/CVE-2019-1551){: external}, [CVE-2019-1563](https://nvd.nist.gov/vuln/detail/CVE-2019-1563){: external}, [CVE-2020-10753](https://nvd.nist.gov/vuln/detail/CVE-2020-10753){: external}, [CVE-2020-12059](https://nvd.nist.gov/vuln/detail/CVE-2020-12059){: external}, [CVE-2020-12888](https://nvd.nist.gov/vuln/detail/CVE-2020-12888){: external}, [CVE-2020-1760](https://nvd.nist.gov/vuln/detail/CVE-2020-1760){: external}, and [CVE-2020-1968](https://nvd.nist.gov/vuln/detail/CVE-2020-1968){: external}.|
| Ubuntu 16.04 packages | 4.4.0-189-generic | 4.4.0-190-generic | Updated worker node image with kernel and package updates for [CVE-2019-20811](https://nvd.nist.gov/vuln/detail/CVE-2019-20811){: external}, [CVE-2019-9453](https://nvd.nist.gov/vuln/detail/CVE-2019-9453){: external}, [CVE-2020-0067](https://nvd.nist.gov/vuln/detail/CVE-2020-0067){: external}, and [CVE-2020-1968](https://nvd.nist.gov/vuln/detail/CVE-2020-1968){: external}. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.17.11_1539" caption-side="top"}

### Changelog for master fix pack 1.17.12_1540, released 21 September 2020
{: #11712_1540}

The following table shows the changes that are included in the master fix pack patch update `1.17.12_1540`. Master patch updates are applied automatically.
{: shortdesc}

| Component | Previous | Current | Description |
| --- | --- | --- | --- |
| Cluster health image | v1.1.9 | v1.1.11 | Updated `Go` version for [CVE-2020-16845](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-16845){: external} and [CVE-2020-24553](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-24553){: external}. |
| etcd | v3.4.10 | v3.4.13 | See the [etcd release notes](https://github.com/etcd-io/etcd/releases/v3.4.13){: external}. |
| GPU device plug-in and installer | bacb9e1 | edd26a4 | Updated `Go` version for [CVE-2020-16845](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-16845){: external} and [CVE-2020-24553](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-24553){: external}. Updated the GPU drivers to version [450.51.06](https://www.nvidia.com/download/driverResults.aspx/162630){: external}. |
| {{site.data.keyword.cloud_notm}} Controller Manager | v1.17.9-1 | v1.17.12-1 | Updated to support the Kubernetes 1.17.12 release and to use `Go` version 1.13.15. |
| {{site.data.keyword.filestorage_full_notm}} plug-in and monitor | 377 | 378 | Updated `Go` version for [CVE-2020-16845](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-16845){: external}. |
| Key Management Service provider | v1.0.1 | v1.0.3 | Updated `Go` version for [CVE-2020-16845](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-16845){: external} and [CVE-2020-24553](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-24553){: external}. |
| Kubernetes | v1.17.11 | v1.17.12 | See the [Kubernetes release notes](https://github.com/kubernetes/kubernetes/releases/tag/v1.17.12){: external}. |
| Kubernetes Dashboard | v2.0.3 | v2.0.4 | See the [Kubernetes Dashboard release notes](https://github.com/kubernetes/dashboard/releases/tag/v2.0.4){: external}. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.17.11_1539" caption-side="top"}

### Changelog for worker node fix pack 1.17.11_1539, released 14 September 2020
{: #11711_1539}

The following table shows the changes that are included in the worker node fix pack `1.17.11_1539`. Worker node patch updates can be applied by updating, reloading (in classic infrastructure), or replacing (in VPC infrastructure) the worker node.
{: shortdesc}

| Component | Previous | Current | Description |
| --------- | -------- | ------- | ----------- |
| HAProxy | 1.8.25-384f42 | 1.8.26-561f1a | See the [HAProxy changelog](https://www.haproxy.org/download/1.8/src/CHANGELOG){: external}.|
| Ubuntu 18.04 packages | 4.15.0-112-generic | 4.15.0-117-generic | Updated worker node image with kernel and package updates for [CVE-2020-14344](https://nvd.nist.gov/vuln/detail/CVE-2020-14344){: external}, [CVE-2020-14363](https://nvd.nist.gov/vuln/detail/CVE-2020-14363){: external}, and [CVE-2020-14386](https://nvd.nist.gov/vuln/detail/CVE-2020-14386){: external}. |
| Ubuntu 16.04 packages | 4.4.0-187-generic | 4.4.0-189-generic | Updated worker node image with kernel and package updates for [CVE-2020-14344](https://nvd.nist.gov/vuln/detail/CVE-2020-14344){: external} and [CVE-2020-14363](https://nvd.nist.gov/vuln/detail/CVE-2020-14363){: external}. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.17.11_1538" caption-side="top"}

### Changelog for worker node fix pack 1.17.11_1538, released 31 August 2020
{: #11711_1538}

The following table shows the changes that are included in the worker node fix pack `1.17.11_1538`. Worker node patch updates can be applied by updating, reloading (in classic infrastructure), or replacing (in VPC infrastructure) the worker node.
{: shortdesc}

| Component | Previous | Current | Description |
| --------- | -------- | ------- | ----------- |
| Ubuntu 18.04 packages | N/A | N/A | Updated worker node image with package updates for [CVE-2020-12403](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-12403){: external}, [CVE-2020-8231](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-8231){: external}, [CVE-2020-8622](https://nvd.nist.gov/vuln/detail/CVE-2020-8622){: external}, [CVE-2020-8623](https://nvd.nist.gov/vuln/detail/CVE-2020-8623){: external}, and [CVE-2020-8624](https://nvd.nist.gov/vuln/detail/CVE-2020-8624){: external}. |
| Ubuntu 16.04 packages | 4.4.0-186-generic | 4.4.0-187-generic | Updated worker node image with kernel and package updates for [CVE-2020-8231](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-8231){: external}, [CVE-2020-8622](https://nvd.nist.gov/vuln/detail/CVE-2020-8622){: external}, and [CVE-2020-8623](https://nvd.nist.gov/vuln/detail/CVE-2020-8623){: external}. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.17.11_1537" caption-side="top"}

### Changelog for master fix pack 1.17.11_1537, released 18 August 2020
{: #11711_1537_master}

The following table shows the changes that are included in the master fix pack patch update `1.17.11_1537`. Master patch updates are applied automatically.
{: shortdesc}

| Component | Previous | Current | Description |
| --------- | -------- | ------- | ----------- |
| Cluster health image | v1.1.8 | v1.1.9 | Updated to use `Go` version 1.13.13. |
| etcd | v3.4.9 | v3.4.10 | See the [etcd release notes](https://github.com/etcd-io/etcd/releases/v3.4.10){: external}. |
| GPU device plug-in and installer | 8c24345 | bacb9e1 | Updated image for [CVE-2020-14039](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-14039){: external} and [CVE-2020-15586](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-15586){: external}. |
| {{site.data.keyword.filestorage_full_notm}} plug-in and monitor | 376 | 377 | Fixed a bug that prevents persistent volume claim (PVC) creation failures from being retried. |
| Key Management Service provider | v1.0.0 | v1.0.1 | Updated image for [CVE-2020-15586](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-15586){: external}. |
| Kubernetes | v1.17.9 | v1.17.11 | See the [Kubernetes release notes](https://github.com/kubernetes/kubernetes/releases/tag/v1.17.11){: external}. |
| Kubernetes add-on resizer | 1.8.5 | 1.8.11 | See the [Kubernetes add-on resizer release notes](https://github.com/kubernetes/autoscaler/releases/tag/addon-resizer-1.8.11){: external}. |
| Kubernetes configuration | N/A | N/A | The Kubernetes API server audit policy configuration is updated to include auditing of all API groups except `apiregistration.k8s.io` and `coordination.k8s.io`. |
| Kubernetes Metrics Server | v0.3.6 | v0.3.7 | See the [Kubernetes Metrics Server release notes](https://github.com/kubernetes-sigs/metrics-server/releases/tag/v0.3.7){: external}. |
| Kubernetes `NodeLocal` DNS cache configuration | N/A | N/A | Increased the pod termination grace period. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.17.9_1535" caption-side="top"}

### Changelog for worker node fix pack 1.17.11_1537, released 17 August 2020
{: #11711_1537}

The following table shows the changes that are included in the worker node fix pack `1.17.11_1537`. Worker node patch updates can be applied by updating, reloading (in classic infrastructure), or replacing (in VPC infrastructure) the worker node.
{: shortdesc}

| Component | Previous | Current | Description |
| --------- | -------- | ------- | ----------- |
| Kubernetes | v1.17.9 | v1.17.11 | See the [Kubernetes changelogs](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG/CHANGELOG-1.17.md#v11711){: external}. |
| Ubuntu 18.04 packages | N/A | N/A | Updated worker node image with package updates for [CVE-2020-11936](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-11936){: external}, [CVE-2020-12400](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-12400){: external}, [CVE-2020-12401](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-12401){: external}, [CVE-2020-15701](https://nvd.nist.gov/vuln/detail/CVE-2020-15701){: external}, [CVE-2020-15702](https://nvd.nist.gov/vuln/detail/CVE-2020-15702){: external}, [CVE-2020-15709](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-15709){: external}, and [CVE-2020-6829](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-6829){: external}. |
| Ubuntu 16.04 packages | N/A | N/A | Updated worker node image with package updates for [CVE-2020-11936](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-11936){: external}, [CVE-2020-15701](https://nvd.nist.gov/vuln/detail/CVE-2020-15701){: external}, [CVE-2020-15702](https://nvd.nist.gov/vuln/detail/CVE-2020-15702){: external}, and [CVE-2020-15709](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-15709){: external}. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.17.9_1535" caption-side="top"}

### Changelog for worker node fix pack 1.17.9_1535, released 3 August 2020
{: #1179_1535}

The following table shows the changes that are included in the worker node fix pack `1.17.9_1535`. Worker node patch updates can be applied by updating, reloading (in classic infrastructure), or replacing (in VPC infrastructure) the worker node.
{: shortdesc}

| Component | Previous | Current | Description |
| --------- | -------- | ------- | ----------- |
| Ubuntu 18.04 packages | 4.15.0-111-generic | 4.15.0-112-generic | Updated worker node images with kernel and package updates for [CVE-2019-17514](https://nvd.nist.gov/vuln/detail/CVE-2019-17514){: external}, [CVE-2019-20907](https://nvd.nist.gov/vuln/detail/CVE-2019-20907){: external}, [CVE-2019-9674](https://nvd.nist.gov/vuln/detail/CVE-2019-9674){: external}, [CVE-2020-10713](https://nvd.nist.gov/vuln/detail/CVE-2020-10713){: external}, [CVE-2020-10757](https://nvd.nist.gov/vuln/detail/CVE-2020-10757){: external}, [CVE-2020-11935](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-11935){: external}, [CVE-2020-14308](https://nvd.nist.gov/vuln/detail/CVE-2020-14308){: external}, [CVE-2020-14309](https://nvd.nist.gov/vuln/detail/CVE-2020-14309){: external}, [CVE-2020-14310](https://nvd.nist.gov/vuln/detail/CVE-2020-14310){: external}, [CVE-2020-14311](https://nvd.nist.gov/vuln/detail/CVE-2020-14311){: external}, [CVE-2020-14422](https://nvd.nist.gov/vuln/detail/CVE-2020-14422){: external}, [CVE-2020-15705](https://nvd.nist.gov/vuln/detail/CVE-2020-15705){: external}, [CVE-2020-15706](https://nvd.nist.gov/vuln/detail/CVE-2020-15706){: external}, and [CVE-2020-15707](https://nvd.nist.gov/vuln/detail/CVE-2020-15707){: external}. |
| Ubuntu 16.04 packages | 4.4.0-185-generic | 4.4.0-186-generic | Updated worker node images with package updates for [CVE-2019-12380](https://nvd.nist.gov/vuln/detail/CVE-2019-12380){: external}, [CVE-2019-17514](https://nvd.nist.gov/vuln/detail/CVE-2019-17514){: external}, [CVE-2019-20907](https://nvd.nist.gov/vuln/detail/CVE-2019-20907){: external}, [CVE-2019-9674](https://nvd.nist.gov/vuln/detail/CVE-2019-9674){: external}, [CVE-2020-10713](https://nvd.nist.gov/vuln/detail/CVE-2020-10713){: external}, [CVE-2020-11935](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-11935){: external}, [CVE-2020-14308](https://nvd.nist.gov/vuln/detail/CVE-2020-14308){: external}, [CVE-2020-14309](https://nvd.nist.gov/vuln/detail/CVE-2020-14309){: external}, [CVE-2020-14310](https://nvd.nist.gov/vuln/detail/CVE-2020-14310){: external}, [CVE-2020-14311](https://nvd.nist.gov/vuln/detail/CVE-2020-14311){: external}, [CVE-2020-14422](https://nvd.nist.gov/vuln/detail/CVE-2020-14422){: external}, [CVE-2020-15705](https://nvd.nist.gov/vuln/detail/CVE-2020-15705){: external}, [CVE-2020-15706](https://nvd.nist.gov/vuln/detail/CVE-2020-15706){: external}, and [CVE-2020-15707](https://nvd.nist.gov/vuln/detail/CVE-2020-15707){: external}. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.17.9_1532" caption-side="top"}

### Changelog for master fix pack 1.17.9_1534, released 24 July 2020
{: #1179_1534}

The following table shows the changes that are included in the master fix pack patch update `1.17.9_1534`. Master patch updates are applied automatically.
{: shortdesc}

| Component | Previous | Current | Description |
| --------- | -------- | ------- | ----------- |
| Cluster master operations | N/A | N/A | Fixed a problem that might cause pods to fail authentication to the Kubernetes API server after a cluster master operation. |
| {{site.data.keyword.filestorage_full_notm}} plug-in and monitor | 375 | 376 | Updated to use `Go` version 1.13.8. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.17.9_1533" caption-side="top"}

### Changelog for master fix pack 1.17.9_1533, released 20 July 2020
{: #1179_1533}

The following table shows the changes that are included in the master fix pack patch update `1.17.9_1533`. Master patch updates are applied automatically.
{: shortdesc}

| Component | Previous | Current | Description |
| --------- | -------- | ------- | ----------- |
| GPU device plug-in and installer | 31d4bb6 | 8c24345 | Updated image for [CVE-2017-12133](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-12133){: external}, [CVE-2017-18269](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-18269){: external}, [CVE-2018-11236](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-11236){: external}, [CVE-2018-11237](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-11237){: external}, [CVE-2018-19591](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-19591){: external}, [CVE-2018-6485](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-6485){: external}, [CVE-2019-19126](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-19126){: external}, [CVE-2019-9169](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9169){: external}, [CVE-2020-10029](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-10029){: external}, [CVE-2020-1751](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-1751){: external}, and [CVE-2020-1752](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-1752){: external}. |
| IBM Calico extension | 353 | 378 | Updated to handle any `ens` network interface. |
| {{site.data.keyword.cloud_notm}} Controller Manager | v1.17.7-1 | v1.17.9-1 | Updated to support the Kubernetes 1.17.9 release. |
| {{site.data.keyword.filestorage_full_notm}} plug-in and monitor configuration | N/A | N/A | Added a pod memory limit. |
| Kubernetes | v1.17.7 | v1.17.9 | See the [Kubernetes release notes](https://github.com/kubernetes/kubernetes/releases/tag/v1.17.9){: external}. The update resolves CVE-2020-8559 (see the [IBM security bulletin](https://www.ibm.com/support/pages/node/6249915){: external}). |
| Kubernetes configuration | N/A | N/A | The Kubernetes API server audit policy configuration is updated to include auditing the `scheduling.k8s.io` API group and the `tokenreviews` resource. |
| Kubernetes Dashboard | v2.0.1 | v2.0.3 | See the [Kubernetes Dashboard release notes](https://github.com/kubernetes/dashboard/releases/tag/v2.0.3){: external}. |
| OpenVPN server | 2.4.6-r3-IKS-131 | 2.4.6-r3-IKS-222 | Removed the deprecated `comp-lzo` compression configuration option and updated the default cipher that is used for encryption. |
| Operator Lifecycle Manager | 0.14.1 | 0.14.1-IKS-1 | Updated image for [CVE-2020-1967](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-1967){: external}. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.17.7_1530" caption-side="top"}

### Changelog for worker node fix pack 1.17.9_1532, released 20 July 2020
{: #1179_1532}

The following table shows the changes that are included in the worker node fix pack `1.17.9_1532`. Worker node patch updates can be applied by updating, reloading (in classic infrastructure), or replacing (in VPC infrastructure) the worker node.
{: shortdesc}

| Component | Previous | Current | Description |
| --------- | -------- | ------- | ----------- |
| HAProxy | 2.0.15-afe432 | 1.8.25-384f42 | See the [HAProxy changelog](https://www.haproxy.org/download/1.8/src/CHANGELOG){: external}. Fixes a connection leak that happens when HAProxy is under high load. |
| Kubernetes | v1.17.7 | v1.17.9 | See the [Kubernetes changelogs](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG/CHANGELOG-1.17.md#v1179){: external}. The update resolves CVE-2020-8557 (see the [IBM security bulletin](https://www.ibm.com/support/pages/node/6249941){: external}). |
| Ubuntu 18.04 packages | 4.15.0-109-generic | 4.15.0-111-generic | Updated worker node images with kernel and package updates for [CVE-2018-11236](https://nvd.nist.gov/vuln/detail/CVE-2018-11236){: external}, [CVE-2018-11237](https://nvd.nist.gov/vuln/detail/CVE-2018-11237){: external}, [CVE-2018-19591](https://nvd.nist.gov/vuln/detail/CVE-2018-19591){: external}, [CVE-2019-19126](https://nvd.nist.gov/vuln/detail/CVE-2019-19126){: external}, [CVE-2019-9169](https://nvd.nist.gov/vuln/detail/CVE-2019-9169){: external}, [CVE-2020-10029](https://nvd.nist.gov/vuln/detail/CVE-2020-10029){: external}, [CVE-2020-12402](https://nvd.nist.gov/vuln/detail/CVE-2020-12402){: external}, [CVE-2020-1751](https://nvd.nist.gov/vuln/detail/CVE-2020-1751){: external}, and [CVE-2020-1752](https://nvd.nist.gov/vuln/detail/CVE-2020-1752){: external}. |
| Ubuntu 16.04 packages | 4.4.0-184-generic | 4.4.0-185-generic | Updated worker node images with package updates for [CVE-2017-12133](https://nvd.nist.gov/vuln/detail/CVE-2017-12133){: external}, [CVE-2017-18269](https://nvd.nist.gov/vuln/detail/CVE-2017-18269){: external}, [CVE-2018-11236](https://nvd.nist.gov/vuln/detail/CVE-2018-11236){: external}, [CVE-2018-11237](https://nvd.nist.gov/vuln/detail/CVE-2018-11237){: external}, [CVE-2018-6485](https://nvd.nist.gov/vuln/detail/CVE-2018-6485){: external}, [CVE-2019-19126](https://nvd.nist.gov/vuln/detail/CVE-2019-19126){: external}, [CVE-2019-9169](https://nvd.nist.gov/vuln/detail/CVE-2019-9169){: external}, [CVE-2020-0543](https://nvd.nist.gov/vuln/detail/CVE-2020-0543){: external}, [CVE-2020-10029](https://nvd.nist.gov/vuln/detail/CVE-2020-10029){: external}, [CVE-2020-10711](https://nvd.nist.gov/vuln/detail/CVE-2020-10711){: external}, [CVE-2020-13143](https://nvd.nist.gov/vuln/detail/CVE-2020-13143){: external}, [CVE-2020-1751](https://nvd.nist.gov/vuln/detail/CVE-2020-1751){: external}, and [CVE-2020-1752](https://nvd.nist.gov/vuln/detail/CVE-2020-1752){: external}. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.17.7_1530" caption-side="top"}

### Changelog for worker node fix pack 1.17.7_1530, released 6 July 2020
{: #1177_1530}

The following table shows the changes that are included in the worker node fix pack `1.17.7_1530`. Worker node patch updates can be applied by updating, reloading (in classic infrastructure), or replacing (in VPC infrastructure) the worker node.
{: shortdesc}

| Component | Previous | Current | Description |
| --------- | -------- | ------- | ----------- |
| HAProxy | 1.8.25-30b675 | 2.0.15-afe432 | See the [HAProxy changelog](https://www.haproxy.org/download/2.0/src/CHANGELOG){: external}. |
| Ubuntu 18.04 packages | 4.15.0-106-generic | 4.15.0-109-generic | Updated worker node images with kernel and package updates for [CVE-2019-12380](https://nvd.nist.gov/vuln/detail/CVE-2019-12380){: external}, [CVE-2019-16089](https://nvd.nist.gov/vuln/detail/CVE-2019-16089){: external}, [CVE-2019-19036](https://nvd.nist.gov/vuln/detail/CVE-2019-19036){: external}, [CVE-2019-19039](https://nvd.nist.gov/vuln/detail/CVE-2019-19039){: external}, [CVE-2019-19318](https://nvd.nist.gov/vuln/detail/CVE-2019-19318){: external}, [CVE-2019-19642](https://nvd.nist.gov/vuln/detail/CVE-2019-19642){: external}, [CVE-2019-19813](https://nvd.nist.gov/vuln/detail/CVE-2019-19813){: external}, [CVE-2019-3689](https://nvd.nist.gov/vuln/detail/CVE-2019-3689){: external}, [CVE-2020-0543](https://nvd.nist.gov/vuln/detail/CVE-2020-0543){: external}, [CVE-2020-10711](https://nvd.nist.gov/vuln/detail/CVE-2020-10711){: external}, [CVE-2020-13143](https://nvd.nist.gov/vuln/detail/CVE-2020-13143){: external}, [CVE-2020-8177](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-8177){: external}, [CVE-2019-19377](https://nvd.nist.gov/vuln/detail/CVE-2019-19377){: external}, and [CVE-2019-19816](https://nvd.nist.gov/vuln/detail/CVE-2019-19816){: external}. |
| Ubuntu 16.04 packages | N/A|N/A | Updated worker node images with package updates for [CVE-2019-3689](https://nvd.nist.gov/vuln/detail/CVE-2019-3689){: external} and [CVE-2020-8177](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-8177){: external}. |
| Worker node `drain` automation | N/A|N/A | Fixes a race condition that can cause worker node `drain` automation to fail. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.17.7_1529" caption-side="top"}

### Changelog for 1.17.7_1529, released 22 June 2020
{: #1177_1529}

The following table shows the changes that are included in the master and worker node update `1.17.7_1529`. Master patch updates are applied automatically. Worker node patch updates can be applied by updating, reloading (in classic infrastructure), or replacing (in VPC infrastructure) the worker node. For more information, see [Update types](/docs/containers?topic=containers-cs_versions#update_types).
{: shortdesc}

| Component | Location | Previous | Current | Description |
| --------- | -------- | ------- | -------- | ----------- |
| Calico | Master | v3.12.1 | v3.12.2 | See the [Calico release notes](https://docs.projectcalico.org/releases){: external}. The master update resolves CVE-2020-13597 (see the [IBM security bulletin](https://www.ibm.com/support/pages/node/6226322){: external}). |
| Cluster health image | Master | v1.1.5 | v1.1.8 | Additional status information is included when an add-on health state is `critical`. Improved performance when handling cluster status updates. |
| Cluster master operations | Master | N/A | N/A | Cluster master operations such as `refresh` or `update` are now canceled if a broken [Kubernetes admission webhook](https://kubernetes.io/docs/reference/access-authn-authz/extensible-admission-controllers/){: external} is detected. |
| etcd | Master | v3.4.7 | v3.4.9 | See the [etcd release notes](https://github.com/etcd-io/etcd/releases/v3.4.9){: external}. |
| GPU device plug-in and installer | Master | 8b02302 | 31d4bb6 | Updated image for [CVE-2020-3810](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-3810){: external}. |
| {{site.data.keyword.cloud_notm}} Controller Manager | Master | v1.17.6-1 | v1.17.7-1 | Updated to support the Kubernetes 1.17.7 release. Updated the version 2.0 private network load balancers (NLBs) to manage Calico global network policies. Updated `calicoctl` version to 3.12.2. |
| {{site.data.keyword.filestorage_full_notm}} plug-in | Master | 373 | 375 | Fixed a bug that might cause error handling to create additional persistent volumes. |
| Kubernetes | Both | v1.17.6 | v1.17.7 | See the [Kubernetes release notes](https://github.com/kubernetes/kubernetes/releases/tag/v1.17.7){: external}. The master update resolves CVE-2020-8558 (see the [IBM security bulletin](https://www.ibm.com/support/pages/node/6249905){: external}). |
| Kubernetes configuration | Master | N/A | N/A | The Kubernetes API server audit policy configuration is updated to include auditing the `apiextensions.k8s.io` API group and the `persistentvolumeclaims` and `persistentvolumes` resources. Additionally, the `http2-max-streams-per-connection` option is set to `1000` to mitigate network disruption impacts on the `kubelet` connection to the API server. |
| Kubernetes Dashboard | Master | v2.0.0 | v2.0.1 | See the [Kubernetes Dashboard release notes](https://github.com/kubernetes/dashboard/releases/tag/v2.0.1){: external}. |
| Ubuntu 18.04 packages | Worker | 4.15.0-101-generic | 4.15.0-106-generic | Updated worker node images with kernel and package updates for [CVE-2018-8740](https://nvd.nist.gov/vuln/detail/CVE-2018-8740){: external}, [CVE-2019-17023](https://nvd.nist.gov/vuln/detail/CVE-2019-17023){: external}, [CVE-2020-0543](https://nvd.nist.gov/vuln/detail/CVE-2020-0543){: external}, [CVE-2020-12049](https://nvd.nist.gov/vuln/detail/CVE-2020-12049){: external}, [CVE-2020-12399](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-12399){: external}, [CVE-2020-13434](https://nvd.nist.gov/vuln/detail/CVE-2020-13434){: external}, [CVE-2020-13630](https://nvd.nist.gov/vuln/detail/CVE-2020-13630){: external}, and [CVE-2020-13632](https://nvd.nist.gov/vuln/detail/CVE-2020-13632){: external}. |
| Ubuntu 16.04 packages | Worker | 4.4.0-179-generic | 4.4.0-184-generic | Updated worker node images with package and kernel updates for [CVE-2020-12049](https://nvd.nist.gov/vuln/detail/CVE-2020-12049){: external}, [CVE-2020-0543](https://nvd.nist.gov/vuln/detail/CVE-2020-0543){: external}, [CVE-2020-12769](https://nvd.nist.gov/vuln/detail/CVE-2020-12769){: external}, [CVE-2020-1749](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-1749){: external}, [CVE-2020-13434](https://nvd.nist.gov/vuln/detail/CVE-2020-13434){: external}, [CVE-2020-13630](https://nvd.nist.gov/vuln/detail/CVE-2020-13630){: external}, and[CVE-2020-13632](https://nvd.nist.gov/vuln/detail/CVE-2020-13632){: external}. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is where the component is located, the master, worker node, or both. The third column is the previous version number of the component. The fourth column is the current version number of the component. The fifth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.17.6_1527" caption-side="top"}

### Changelog for worker node fix pack 1.17.6_1527, released 8 June 2020
{: #1176_1527}

The following table shows the changes that are included in the worker node fix pack `1.17.6_1527`. Worker node patch updates can be applied by updating, reloading (in classic infrastructure), or replacing (in VPC infrastructure) the worker node.
{: shortdesc}

| Component | Previous | Current | Description |
| --------- | -------- | ------- | ----------- |
| Ubuntu 18.04 packages | N/A | N/A | Updated worker node images with package updates for [CVE-2019-1547](https://nvd.nist.gov/vuln/detail/CVE-2019-1547){: external}, [CVE-2019-1549](https://nvd.nist.gov/vuln/detail/CVE-2019-1549){: external}, [CVE-2019-1551](https://nvd.nist.gov/vuln/detail/CVE-2019-1551){: external}, [CVE-2019-1563](https://nvd.nist.gov/vuln/detail/CVE-2019-1563){: external}, and [CVE-2020-12762](https://nvd.nist.gov/vuln/detail/CVE-2020-12762){: external}. |
| Ubuntu 16.04 packages | 4.4.0-178-generic | 4.4.0-179-generic | Updated worker node images with package and kernel updates for [CVE-2019-1547](https://nvd.nist.gov/vuln/detail/CVE-2019-1547){: external}, [CVE-2019-1551](https://nvd.nist.gov/vuln/detail/CVE-2019-1551){: external}, [CVE-2019-1563](https://nvd.nist.gov/vuln/detail/CVE-2019-1563){: external}, and [CVE-2020-12762](https://nvd.nist.gov/vuln/detail/CVE-2020-12762){: external}. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.17.6_1526" caption-side="top"}

### Changelog for 1.17.6_1526, released 26 May 2020
{: #1176_1526}

The following table shows the changes that are included in the master and worker node update `1.17.6_1526`. Master patch updates are applied automatically. Worker node patch updates can be applied by updating, reloading (in classic infrastructure), or replacing (in VPC infrastructure) the worker node. For more information, see [Update types](/docs/containers?topic=containers-cs_versions#update_types).
{: shortdesc}

| Component | Location | Previous | Current | Description |
| --------- | -------- | ------- | -------- | ----------- |
| Cluster health image | Master | v1.1.1 | v1.1.4 | When cluster add-ons do not support the current cluster version, a warning is now returned in the cluster health state. |
| etcd | Master | v3.4.3 | v3.4.7 | See the [etcd release notes](https://github.com/etcd-io/etcd/releases/v3.4.7){: external}. |
| Gateway-enabled cluster controller | Master | 1045 | 1082 | Updated image for [CVE-2020-1967](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-1967){: external}. |
| GPU device plug-in and installer | Master | 8c6538f | 8b02302 | Updated image for [CVE-2020-1967](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-1967){: external}. |
| IBM Calico extension | Master | 320 | 353 | Skips creating a Calico host endpoint when no endpoint is needed. Updated image for [CVE-2020-1967](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-1967){: external}. |
| {{site.data.keyword.cloud_notm}} Controller Manager | Master | v1.17.5-1 | v1.17.6-1 | Updated to support the Kubernetes 1.17.6 release. Updated network load balancer (NLB) events to use the latest {{site.data.keyword.cloud_notm}} troubleshooting documentation. |
| {{site.data.keyword.filestorage_full_notm}} plug-in and monitor | Master | 358 | 373 | Updated image for [CVE-2020-1967](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-1967){: external} and [CVE-2020-11655](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-11655){: external}. |
| Kubernetes | Both | v1.17.5 | v1.17.6 | See the [Kubernetes release notes](https://github.com/kubernetes/kubernetes/releases/tag/v1.17.6){: external}. |
| Kubernetes Dashboard | Master | v2.0.0-rc7 | v2.0.0 | See the [Kubernetes Dashboard release notes](https://github.com/kubernetes/dashboard/releases/tag/v2.0.0){: external}. |
| Kubernetes Metrics Server | Master | N/A | N/A | Increased the CPU per node for the `metrics-server` container to improve availability of the metrics server API service for large clusters. |
| Kubernetes NodeLocal DNS cache | Master | 1.15.8 | 1.15.13 | See the [Kubernetes NodeLocal DNS cache release notes](https://github.com/kubernetes/dns/releases/tag/1.15.13){: external}. Updated the `node-local-dns` daemon set to include the `prometheus.io/port` and `prometheus.io/scrape` annotations on the pods. |
| Load balancer and load balancer monitor for {{site.data.keyword.cloud_notm}} Provider | Master | 177 | 208 | Updated the version 2.0 network load balancers (NLB) to clean up unnecessary IPVS rules. Improved application logging. Updated image for [CVE-2020-1967](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-1967){: external}. |
| Ubuntu 18.04 packages | Worker | 4.15.0-99-generic | 4.15.0-101-generic | Updated worker node images with kernel and package updates for [CVE-2019-20795](https://nvd.nist.gov/vuln/detail/CVE-2019-20795){: external}, [CVE-2020-11494](https://nvd.nist.gov/vuln/detail/CVE-2020-11494){: external}, [CVE-2020-12762](https://nvd.nist.gov/vuln/detail/CVE-2020-12762){: external}, [CVE-2020-3810](https://nvd.nist.gov/vuln/detail/CVE-2020-3810){: external}, [CVE-2020-8616](https://nvd.nist.gov/vuln/detail/CVE-2020-8616){: external}, and [CVE-2020-8617](https://nvd.nist.gov/vuln/detail/CVE-2020-8617){: external}. |
| Ubuntu 16.04 packages | Worker | 4.4.0-178-generic | 4.4.0-179-generic | Updated worker node images with package and kernel updates for [CVE-2019-19060](https://nvd.nist.gov/vuln/detail/CVE-2019-19060){: external}, [CVE-2020-11494](https://nvd.nist.gov/vuln/detail/CVE-2020-11494){: external}, [CVE-2020-11608](https://nvd.nist.gov/vuln/detail/CVE-2020-11608){: external}, [CVE-2020-12762](https://nvd.nist.gov/vuln/detail/CVE-2020-12762){: external}, [CVE-2020-3810](https://nvd.nist.gov/vuln/detail/CVE-2020-3810){: external}, [CVE-2020-8616](https://nvd.nist.gov/vuln/detail/CVE-2020-8616), and [CVE-2020-8617](https://nvd.nist.gov/vuln/detail/CVE-2020-8617){: external}. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is where the component is located, the master, worker node, or both. The third column is the previous version number of the component. The fourth column is the current version number of the component. The fifth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.17.5_1524" caption-side="top"}

### Changelog for worker node fix pack 1.17.5_1524, released 11 May 2020
{: #1175_1524}

The following table shows the changes that are included in the worker node fix pack `1.17.5_1524`. Worker node patch updates can be applied by updating, reloading (in classic infrastructure), or replacing (in VPC infrastructure) the worker node.
{: shortdesc}

| Component | Previous | Current | Description |
| --------- | -------- | ------- | ----------- |
| Ubuntu 18.04 packages | 4.15.0-96-generic | 4.15.0-99-generic | Updated worker node images with kernel and package updates for [CVE-2019-19768](https://nvd.nist.gov/vuln/detail/CVE-2019-19768){: external}, [CVE-2020-11884](https://nvd.nist.gov/vuln/detail/CVE-2020-11884){: external}, and [CVE-2020-12243](https://nvd.nist.gov/vuln/detail/CVE-2020-12243){: external}. |
| Ubuntu 16.04 packages | 4.4.0-177-generic | 4.4.0-178-generic | Updated worker node images with package and kernel updates for [CVE-2019-19768](https://nvd.nist.gov/vuln/detail/CVE-2019-19768){: external} and [CVE-2020-12243](https://nvd.nist.gov/vuln/detail/CVE-2020-12243){: external}. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.17.4_1523" caption-side="top"}

### Changelog for worker node fix pack 1.17.5_1523, released 27 April 2020
{: #1175_1523}

The following table shows the changes that are included in the worker node fix pack `1.17.5_1523`. Worker node patch updates can be applied by updating, reloading (in classic infrastructure), or replacing (in VPC infrastructure) the worker node.
{: shortdesc}

| Component | Previous | Current | Description |
| --------- | -------- | ------- | ----------- |
| containerd | v1.3.3 | v1.3.4 | See the [containerd release notes](https://github.com/containerd/containerd/releases/tag/v1.3.4){: external}. |
| HA proxy | 1.8.25-30b675 | 1.8.25-adb65d | The update addresses [CVE-2020-1967](https://nvd.nist.gov/vuln/detail/CVE-2020-1967){: external}. |
| Kubernetes | v1.17.4 | v1.17.5 | See the [Kubernetes release notes](https://github.com/kubernetes/kubernetes/releases/tag/v1.17.5){: external}. |
| Ubuntu 18.04 packages | N/A | N/A | Updated worker node images with package updates for [CVE-2018-1000876](https://nvd.nist.gov/vuln/detail/CVE-2018-1000876){: external}, [CVE-2018-10372](https://nvd.nist.gov/vuln/detail/CVE-2018-10372){: external}, [CVE-2018-10373](https://nvd.nist.gov/vuln/detail/CVE-2018-10373){: external}, [CVE-2018-10534](https://nvd.nist.gov/vuln/detail/CVE-2018-10534){: external}, [CVE-2018-10535](https://nvd.nist.gov/vuln/detail/CVE-2018-10535){: external}, [CVE-2018-12641](https://nvd.nist.gov/vuln/detail/CVE-2018-12641){: external}, [CVE-2018-12697](https://nvd.nist.gov/vuln/detail/CVE-2018-12697){: external}, [CVE-2018-12698](https://nvd.nist.gov/vuln/detail/CVE-2018-12698){: external}, [CVE-2018-12699](https://nvd.nist.gov/vuln/detail/CVE-2018-12699){: external}, [CVE-2018-12700](https://nvd.nist.gov/vuln/detail/CVE-2018-12700){: external}, [CVE-2018-12934](https://nvd.nist.gov/vuln/detail/CVE-2018-12934){: external}, [CVE-2018-13033](https://nvd.nist.gov/vuln/detail/CVE-2018-13033){: external}, [CVE-2018-17358](https://nvd.nist.gov/vuln/detail/CVE-2018-17358){: external}, [CVE-2018-17359](https://nvd.nist.gov/vuln/detail/CVE-2018-17359){: external}, [CVE-2018-17360](https://nvd.nist.gov/vuln/detail/CVE-2018-17360){: external}, [CVE-2018-17794](https://nvd.nist.gov/vuln/detail/CVE-2018-17794){: external}, [CVE-2018-17985](https://nvd.nist.gov/vuln/detail/CVE-2018-17985){: external}, [CVE-2018-18309](https://nvd.nist.gov/vuln/detail/CVE-2018-18309){: external}, [CVE-2018-18483](https://nvd.nist.gov/vuln/detail/CVE-2018-18483){: external}, [CVE-2018-18484](https://nvd.nist.gov/vuln/detail/CVE-2018-18484){: external}, [CVE-2018-18605](https://nvd.nist.gov/vuln/detail/CVE-2018-18605){: external}, [CVE-2018-18606](https://nvd.nist.gov/vuln/detail/CVE-2018-18606){: external}, [CVE-2018-18607](https://nvd.nist.gov/vuln/detail/CVE-2018-18607){: external}, [CVE-2018-18700](https://nvd.nist.gov/vuln/detail/CVE-2018-18700){: external}, [CVE-2018-18701](https://nvd.nist.gov/vuln/detail/CVE-2018-18701){: external}, [CVE-2018-19931](https://nvd.nist.gov/vuln/detail/CVE-2018-19931){: external}, [CVE-2018-19932](https://nvd.nist.gov/vuln/detail/CVE-2018-19932){: external}, [CVE-2018-20002](https://nvd.nist.gov/vuln/detail/CVE-2018-20002){: external}, [CVE-2018-20623](https://nvd.nist.gov/vuln/detail/CVE-2018-20623){: external}, [CVE-2018-20651](https://nvd.nist.gov/vuln/detail/CVE-2018-20651){: external}, [CVE-2018-20671](https://nvd.nist.gov/vuln/detail/CVE-2018-20671){: external}, [CVE-2018-8945](https://nvd.nist.gov/vuln/detail/CVE-2018-8945){: external}, [CVE-2018-9138](https://nvd.nist.gov/vuln/detail/CVE-2018-9138){: external}, [CVE-2019-12972](https://nvd.nist.gov/vuln/detail/CVE-2019-12972){: external}, [CVE-2019-14250](https://nvd.nist.gov/vuln/detail/CVE-2019-14250){: external}, [CVE-2019-14444](https://nvd.nist.gov/vuln/detail/CVE-2019-14444){: external}, [CVE-2019-17450](https://nvd.nist.gov/vuln/detail/CVE-2019-17450){: external}, [CVE-2019-17451](https://nvd.nist.gov/vuln/detail/CVE-2019-17451){: external}, [CVE-2019-18348](https://nvd.nist.gov/vuln/detail/CVE-2019-18348){: external}, [CVE-2019-9070](https://nvd.nist.gov/vuln/detail/CVE-2019-9070){: external}, [CVE-2019-9071](https://nvd.nist.gov/vuln/detail/CVE-2019-9071){: external}, [CVE-2019-9073](https://nvd.nist.gov/vuln/detail/CVE-2019-9073){: external}, [CVE-2019-9074](https://nvd.nist.gov/vuln/detail/CVE-2019-9074){: external}, [CVE-2019-9075](https://nvd.nist.gov/vuln/detail/CVE-2019-9075){: external}, [CVE-2019-9077](https://nvd.nist.gov/vuln/detail/CVE-2019-9077){: external}, [CVE-2020-11008](https://nvd.nist.gov/vuln/detail/CVE-2020-11008){: external}, [CVE-2020-5260](https://nvd.nist.gov/vuln/detail/CVE-2020-5260){: external}, and [CVE-2020-8492](https://nvd.nist.gov/vuln/detail/CVE-2020-8492){: external}. |
| Ubuntu 16.04 packages | N/A | N/A | Updated worker node images with package updates for [CVE-2019-18348](https://nvd.nist.gov/vuln/detail/CVE-2019-18348){: external}, [CVE-2020-11008](https://nvd.nist.gov/vuln/detail/CVE-2020-11008){: external}, [CVE-2020-5260](https://nvd.nist.gov/vuln/detail/CVE-2020-5260){: external}, and [CVE-2020-8492](https://nvd.nist.gov/vuln/detail/CVE-2020-8492){: external}. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.17.4_1521" caption-side="top"}

### Changelog for master fix pack 1.17.5_1522, released 23 April 2020
{: #1175_1522}

The following table shows the changes that are included in the master fix pack patch update `1.17.5_1522`. Master patch updates are applied automatically.
{: shortdesc}

| Component | Previous | Current | Description |
| --------- | -------- | ------- | ----------- |
| Cluster health | N/A | v1.1.1 | Cluster health now includes more add-on status information. |
| {{site.data.keyword.cloud_notm}} Controller Manager | v1.17.4-3 | v1.17.5-1 | Updated to support the Kubernetes 1.17.5 release and to use `Go` version 1.13.9. |
| Kubernetes | v1.17.4 | v1.17.5 | See the [Kubernetes release notes](https://github.com/kubernetes/kubernetes/releases/tag/v1.17.5){: external}. The update resolves CVE-2020-8555 (see the [IBM security bulletin](https://www.ibm.com/support/pages/node/6220220){: external}). |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.17.4_1521" caption-side="top"}

### Changelog for master fix pack 1.17.4_1521, released 17 April 2020
{: #1174_1521_master}

The following table shows the changes that are included in the master fix pack patch update `1.17.4_1521`. Master patch updates are applied automatically.
{: shortdesc}

| Component | Previous | Current | Description |
| --------- | -------- | ------- | ----------- |
| Calico | v3.12.0 | v3.12.1 | See the [Calico release notes](https://docs.projectcalico.org/release-notes/){: external}. Updated to allow egress from the worker nodes via the `allow-vrrp` `GlobalNetworkPolicy`. |
| CoreDNS | 1.6.7 | 1.6.9 | See the [CoreDNS release notes](https://coredns.io/2020/03/24/coredns-1.6.9-release/){: external}. Fixed a bug during Corefile migration that might generate invalid data that makes CoreDNS pods fail. |
| GPU device plug-in and installer | 49979f5 | 8c6538f | Updated the GPU drivers to version [440.33.01](https://www.nvidia.com/download/driverResults.aspx/154570){: external}. |
| {{site.data.keyword.cloud_notm}} Controller Manager | v1.17.4-1 | v1.17.4-3 | Updated to use `calicoctl` version 3.12.1. |
| Key Management Service provider | 277 | v1.0.0 | Updated the {{site.data.keyword.keymanagementservicelong_notm}} `Go` client. |
| Kubernetes Dashboard | v2.0.0-rc5 | v2.0.0-rc7 | See the [Kubernetes Dashboard release notes](https://github.com/kubernetes/dashboard/releases/tag/v2.0.0-rc7){: external}. Added a readiness probe to the Kubernetes Dashboard configuration. |
| Kubernetes Dashboard metrics scraper | v1.0.3 | v1.0.4 | See the [Kubernetes Dashboard metrics scraper release notes](https://github.com/kubernetes-sigs/dashboard-metrics-scraper/releases/tag/v1.0.4){: external}. |
| OpenVPN client | N/A | N/A | Fixed problem that might cause the `vpn-config` secret in the `kube-system` namespace to be deleted during cluster master operations. |
| Operator Lifecycle Manager Catalog | v1.5.11 | v1.6.1 | See the [Operator Lifecycle Manager Catalog release notes](https://github.com/operator-framework/operator-registry/releases/tag/v1.6.1){: external}. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.17.4_1520" caption-side="top"}

### Changelog for worker node fix pack 1.17.4_1521, released 13 April 2020
{: #1174_1521}

The following table shows the changes that are included in the worker node fix pack `1.17.4_1521`. Worker node patch updates can be applied by updating, reloading (in classic infrastructure), or replacing (in VPC infrastructure) the worker node.
{: shortdesc}

| Component | Previous | Current | Description |
| --------- | -------- | ------- | ----------- |
| HA proxy | 1.8.23 | 1.8.25 | See the [HA proxy changelogs](https://www.haproxy.org/download/1.8/src/CHANGELOG){: external}. The update addresses [CVE-2020-11100](https://nvd.nist.gov/vuln/detail/CVE-2020-11100){: external}. |
| Ubuntu 18.04 packages | 4.15.0-91-generic | 4.15.0-96-generic | Updated worker node images with package and kernel updates for [CVE-2020-8831](https://nvd.nist.gov/vuln/detail/CVE-2020-8831){: external}, [CVE-2020-8833](https://nvd.nist.gov/vuln/detail/CVE-2020-8833){: external}, [CVE-2020-11100](https://nvd.nist.gov/vuln/detail/CVE-2020-11100){: external}, and [CVE-2020-8834](https://nvd.nist.gov/vuln/detail/CVE-2020-8834){: external}. |
| Ubuntu 16.04 packages | 4.4.0-176-generic | 4.4.0-177-generic | Updated worker node images with package and kernel updates for [CVE-2020-8831](https://nvd.nist.gov/vuln/detail/CVE-2020-8831){: external}, [CVE-2020-8833](https://nvd.nist.gov/vuln/detail/CVE-2020-8833){: external}, and [CVE-2020-8428](https://nvd.nist.gov/vuln/detail/CVE-2020-8428){: external}. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.17.4_1520" caption-side="top"}

### Changelog for worker node fix pack 1.17.4_1520, released 30 March 2020
{: #1174_1520}

The following table shows the changes that are included in the worker node fix pack `1.17.4_1520`. Worker node patch updates can be applied by updating, reloading (in classic infrastructure), or replacing (in VPC infrastructure) the worker node.
{: shortdesc}

| Component | Previous | Current | Description |
| --------- | -------- | ------- | ----------- |
| Ubuntu 18.04 packages | 4.15.0-88-generic | 4.15.0-91-generic | Updated worker node images with package and kernel updates for [CVE-2020-1700](https://nvd.nist.gov/vuln/detail/CVE-2020-1700){: external}, [CVE-2020-10531](https://nvd.nist.gov/vuln/detail/CVE-2020-10531){: external}, [CVE-2020-2732](https://nvd.nist.gov/vuln/detail/CVE-2020-2732){: external}, [CVE-2020-8832](https://nvd.nist.gov/vuln/detail/CVE-2020-8832){: external}, [CVE-2019-12387](https://nvd.nist.gov/vuln/detail/CVE-2019-12387){: external}, [CVE-2019-12855](https://nvd.nist.gov/vuln/detail/CVE-2019-12855){: external}, [CVE-2019-9512](https://nvd.nist.gov/vuln/detail/CVE-2019-9512){: external}, [CVE-2019-9514](https://nvd.nist.gov/vuln/detail/CVE-2019-9514){: external}, [CVE-2019-9515](https://nvd.nist.gov/vuln/detail/CVE-2019-9515){: external}, [CVE-2020-10108](https://nvd.nist.gov/vuln/detail/CVE-2020-10108){: external}, [CVE-2020-10109](https://nvd.nist.gov/vuln/detail/CVE-2020-10109){: external}, [CVE-2018-20786](https://nvd.nist.gov/vuln/detail/CVE-2018-20786), and [CVE-2019-20079](https://nvd.nist.gov/vuln/detail/CVE-2019-20079){: external}. |
| Ubuntu 16.04 packages |4.4.0-174-generic | 4.4.0-176-generic | Updated worker node images with package updates for [CVE-2020-10531](https://nvd.nist.gov/vuln/detail/CVE-2020-10531){: external}, [CVE-2020-2732](https://nvd.nist.gov/vuln/detail/CVE-2020-2732){: external}, [CVE-2017-11109](https://nvd.nist.gov/vuln/detail/CVE-2017-11109){: external}, [CVE-2017-6349](https://nvd.nist.gov/vuln/detail/CVE-2017-6349), and [CVE-2017-6350](https://nvd.nist.gov/vuln/detail/CVE-2017-6350){: external}.|
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.17.4_1519" caption-side="top"}

### Changelog for 1.17.4_1519, released 16 March 2020
{: #1174_1519}

The following table shows the changes that are included in the master and worker node update `1.17.4_1519`. Master patch updates are applied automatically. Worker node patch updates can be applied by updating, reloading (in classic infrastructure), or replacing (in VPC infrastructure) the worker node. For more information, see [Update types](/docs/containers?topic=containers-cs_versions#update_types).
{: shortdesc}

| Component | Location | Previous | Current | Description |
| --------- | -------- | ------- | -------- | ----------- |
| Cluster health image | Master | N/A | N/A | Cluster health status now includes links to {{site.data.keyword.cloud_notm}} documentation. |
| {{site.data.keyword.cloud_notm}} Controller Manager | Master | v1.17.3-1 | v1.17.4-1 | Updated to support the Kubernetes 1.17.4 release and to use `Go` version 1.13.8. Fixed VPC load balancer error message to use the current {{site.data.keyword.containerlong_notm}} plug-in CLI syntax. |
| Kubernetes | Both | v1.17.3 | v1.17.4 | See the [Kubernetes release notes](https://github.com/kubernetes/kubernetes/releases/tag/v1.17.4){: external}. |
| Load balancer and load balancer monitor for {{site.data.keyword.cloud_notm}} Provider | Master | 169 | 177 | Version 2.0 network load balancers (NLB) were updated to fix problems with long-lived network connections to endpoints that failed readiness probes. |
| Operator Lifecycle Manager Catalog | Master | v1.5.9 | v1.5.11 | See the [Operator Lifecycle Manager Catalog release notes](https://github.com/operator-framework/operator-registry/releases/tag/v1.5.11){: external}. |
| Ubuntu 18.04 packages | Worker | N/A | N/A | Updated worker node images with package updates for [CVE-2019-13734](https://nvd.nist.gov/vuln/detail/CVE-2019-13734){: external}, [CVE-2019-13750](https://nvd.nist.gov/vuln/detail/CVE-2019-13750){: external}, [CVE-2019-13751](https://nvd.nist.gov/vuln/detail/CVE-2019-13751){: external}, [CVE-2019-13752](https://nvd.nist.gov/vuln/detail/CVE-2019-13752){: external}, [CVE-2019-13753](https://nvd.nist.gov/vuln/detail/CVE-2019-13753){: external}, [CVE-2019-19923](https://nvd.nist.gov/vuln/detail/CVE-2019-19923){: external}, [CVE-2019-19924](https://nvd.nist.gov/vuln/detail/CVE-2019-19924){: external}, [CVE-2019-19925](https://nvd.nist.gov/vuln/detail/CVE-2019-19925){: external}, [CVE-2019-19926](https://nvd.nist.gov/vuln/detail/CVE-2019-19926){: external}, [CVE-2019-19959](https://nvd.nist.gov/vuln/detail/CVE-2019-19959){: external}, and [CVE-2019-20218](https://nvd.nist.gov/vuln/detail/CVE-2019-20218){: external}. |
| Ubuntu 16.04 packages | Worker |N/A | N/A | Updated worker node images with package updates for [CVE-2019-13734](https://nvd.nist.gov/vuln/detail/CVE-2019-13734){: external}, [CVE-2019-13750](https://nvd.nist.gov/vuln/detail/CVE-2019-13750){: external}, [CVE-2019-13751](https://nvd.nist.gov/vuln/detail/CVE-2019-13751){: external}, [CVE-2019-13752](https://nvd.nist.gov/vuln/detail/CVE-2019-13752){: external}, [CVE-2019-13753](https://nvd.nist.gov/vuln/detail/CVE-2019-13753){: external}, [CVE-2019-19926](https://nvd.nist.gov/vuln/detail/CVE-2019-19926){: external}, and [CVE-2019-20218](https://nvd.nist.gov/vuln/detail/CVE-2019-20218){: external}. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is where the component is located, the master, worker node, or both. The third column is the previous version number of the component. The fourth column is the current version number of the component. The fifth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.17.3_1518" caption-side="top"}

### Changelog for worker node fix pack 1.17.3_1518, released 2 March 2020
{: #1173_1518}

The following table shows the changes that are included in the worker node fix pack 1.17.3_1518. Worker node patch updates can be applied by updating, reloading (in classic infrastructure), or replacing (in VPC infrastructure) the worker node.
{: shortdesc}

| Component | Previous | Current | Description |
| --------- | -------- | ------- | ----------- |
| GPU worker node provisioning | N/A | N/A | Fixed bug where new GPU worker nodes could not automatically join clusters that run Kubernetes version 1.16 or 1.17. |
| Ubuntu 18.04 packages | 4.15.0-76-generic | 4.15.0-88-generic | Updated worker node images with package updates for [CVE-2019-5108](https://nvd.nist.gov/vuln/detail/CVE-2019-5108){: external}, [CVE-2019-20096](https://nvd.nist.gov/vuln/detail/CVE-2019-20096){: external}, [CVE-2019-18885](https://nvd.nist.gov/vuln/detail/CVE-2019-18885){: external}, [CVE-2019-19082](https://nvd.nist.gov/vuln/detail/CVE-2019-19082){: external}, [CVE-2019-19078](https://nvd.nist.gov/vuln/detail/CVE-2019-19078){: external}, [CVE-2019-19332](https://nvd.nist.gov/vuln/detail/CVE-2019-19332){: external}, [CVE-2016-9840](https://nvd.nist.gov/vuln/detail/CVE-2016-9840){: external}, [CVE-2016-9841](https://nvd.nist.gov/vuln/detail/CVE-2016-9841){: external}, [CVE-2016-9842](https://nvd.nist.gov/vuln/detail/CVE-2016-9842){: external}, and [CVE-2016-9843](https://nvd.nist.gov/vuln/detail/CVE-2016-9843){: external}. |
| Ubuntu 16.04 packages | 4.4.0-173-generic | 4.4.0-174-generic | Updated worker node images with kernel and package updates for [CVE-2019-20096](https://nvd.nist.gov/vuln/detail/CVE-2019-20096){: external}, [CVE-2016-9840](https://nvd.nist.gov/vuln/detail/CVE-2016-9840){: external}, [CVE-2016-9841](https://nvd.nist.gov/vuln/detail/CVE-2016-9841){: external}, [CVE-2016-9842](https://nvd.nist.gov/vuln/detail/CVE-2016-9842){: external}, and [CVE-2016-9843](https://nvd.nist.gov/vuln/detail/CVE-2016-9843){: external}.|
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.17.3_1516" caption-side="top"}

### Changelog for fix pack 1.17.3_1516, released 17 February 2020
{: #1173_1516}

The following table shows the changes that are included in the master and worker node fix pack update `1.17.3_1516`. Master patch updates are applied automatically. Worker node patch updates can be applied by updating, reloading (in classic infrastructure), or replacing (in VPC infrastructure) the worker node. For more information, see [Update types](/docs/containers?topic=containers-cs_versions#update_types).
{: shortdesc}

| Component | Location | Previous | Current | Description |
| --------- | -------- | ------- | -------- | ----------- |
| containerd | Worker | v1.3.2 | v1.3.3 | See [containerd release notes](https://github.com/containerd/containerd/releases/tag/v1.3.3){: external}. Update resolves [CVE-2019-19921](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-19921){: external}, [CVE-2019-16884](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-16884){: external}, [CVE-2020-0601](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-0601){: external}, [CVE-2020-7919](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-7919){: external}, and [CVE-2019-11253](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-11253){: external}. |
| GPU device plug-in and installer | Master | affdfe2 | 49979f5 | Image updated for [CVE-2018-16888](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-16888){: external}, [CVE-2019-20386](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-20386){: external}, [CVE-2019-3843](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3843){: external}, [CVE-2019-3844](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3844){: external}, and [CVE-2020-1712](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-1712){: external}. |
| {{site.data.keyword.cloud_notm}} Controller Manager | Master | v1.17.2-4 | v1.17.3-1 | Updated to support the Kubernetes 1.17.3 release and to use `Go` version 1.13.6. |
| Kubernetes | Both | v1.17.2 | v1.17.3 | See the [Kubernetes release notes](https://github.com/kubernetes/kubernetes/releases/tag/v1.17.3){: external}. The master update resolves CVE-2019-11254 and CVE-2020-8552 (see the [IBM security bulletin](https://www.ibm.com/support/pages/node/6203780){: external}), and the worker node update resolves CVE-2020-8551 (see the [IBM security bulletin](https://www.ibm.com/support/pages/node/6204874){: external}). |
| Kubernetes Dashboard | Master | v2.0.0-rc3 | v2.0.0-rc5 | See the [Kubernetes Dashboard release notes](https://github.com/kubernetes/dashboard/releases/tag/v2.0.0-rc5){: external}. |
| Operator Lifecycle Manager Catalog | Master | v1.5.8 | v1.5.9 | See the [Operator Lifecycle Manager Catalog release notes](https://github.com/operator-framework/operator-registry/releases/tag/v1.5.9){: external}. |
| Ubuntu 18.04 packages | Worker | N/A | N/A | Updated worker node images with package updates for [CVE-2019-19956](https://nvd.nist.gov/vuln/detail/CVE-2019-19956){: external}, [CVE-2020-7595](https://nvd.nist.gov/vuln/detail/CVE-2020-7595){: external}, [CVE-2019-18634](https://nvd.nist.gov/vuln/detail/CVE-2019-18634){: external}, [CVE-2019-3843](https://nvd.nist.gov/vuln/detail/CVE-2019-3843){: external}, [CVE-2019-3844](https://nvd.nist.gov/vuln/detail/CVE-2019-3844){: external}, [CVE-2019-20386](https://nvd.nist.gov/vuln/detail/CVE-2019-20386){: external}, and [CVE-2020-1712](https://nvd.nist.gov/vuln/detail/CVE-2020-1712){: external}. |
| Ubuntu 16.04 packages | Worker | N/A | N/A | Updated worker node images with package updates for [CVE-2019-19956](https://nvd.nist.gov/vuln/detail/CVE-2019-19956){: external}, [CVE-2020-7595](https://nvd.nist.gov/vuln/detail/CVE-2020-7595){: external}, [CVE-2019-18634](https://nvd.nist.gov/vuln/detail/CVE-2019-18634){: external}, [CVE-2018-16888](https://nvd.nist.gov/vuln/detail/CVE-2018-16888){: external}, [CVE-2019-20386](https://nvd.nist.gov/vuln/detail/CVE-2019-20386){: external}, and[CVE-2020-1712](https://nvd.nist.gov/vuln/detail/CVE-2020-1712){: external}.|
{: summary="The rows are read from left to right. The first column is the changed component. The second column is where the component is located, the master, worker node, or both. The third column is the previous version number of the component. The fourth column is the current version number of the component. The fifth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.17.2_1515" caption-side="top"}

### Changelog for 1.17.2_1515, released 10 February 2020
{: #1172_1515}

The following table shows the changes that are included in patch update 1.17.2_1515.
{: shortdesc}

| Component | Previous | Current | Description |
| --------- | -------- | ------- | ----------- |
| Calico | v3.9.5 | v3.12.0 | See the [Calico release notes](https://docs.projectcalico.org/archive/v3.12/release-notes/){: external}. |
| CoreDNS | 1.6.6 | 1.6.7 | See the [CoreDNS release notes](https://coredns.io/2020/01/28/coredns-1.6.7-release/){: external}. |
| etcd | v3.3.18 | v3.4.3 | See the [etcd release notes](https://github.com/etcd-io/etcd/releases/v3.4.3){: external}. |
| GPU device plug-in and installer | da19df3 | affdfe2 | Image updated for [CVE-2016-9840](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2016-9840){: external}, [CVE-2016-9841](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2016-9841){: external}, [CVE-2016-9842](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2016-9842){: external}, [CVE-2016-9843](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2016-9843){: external}, and [CVE-2019-5188](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5188){: external}. |
| **New**: {{site.data.keyword.cloud_notm}} Controller Manager | v1.16.5-148 | v1.17.2-4 | The {{site.data.keyword.cloud_notm}} Controller Manager component replaces the {{site.data.keyword.cloud_notm}} Provider component by moving the {{site.data.keyword.cloud_notm}} controllers from the Kubernetes [`kube-controller-manager`](https://kubernetes.io/docs/concepts/overview/components/#kube-controller-manager){: external} to the [`cloud-controller-manager`](https://kubernetes.io/docs/concepts/overview/components/#cloud-controller-manager){: external} component. The {{site.data.keyword.cloud_notm}} Controller Manager is updated to support the Kubernetes 1.17.2 release, to use `distroless/static` base image version `c6d59815`, and to use `calicoctl` version 3.12.0. |
| {{site.data.keyword.filestorage_full_notm}} plug-in and monitor | 357 | 358 | Made the `ibmc-file-gold` storage class the default storage class for new clusters only. The default storage class for existing clusters is unchanged. If you want to set your own default, see [Changing the default storage class](/docs/containers?topic=containers-kube_concepts#default_storageclass). In addition, the updated the image for [CVE-2019-5188](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5188){: external}. |
| Kubernetes | v1.16.5 | v1.17.2 | See the [Kubernetes release notes](https://github.com/kubernetes/kubernetes/releases/tag/v1.17.2){: external}. |
| Kubernetes configuration | N/A | N/A | Updated the OpenID Connect configuration for the cluster's Kubernetes API server to use the {{site.data.keyword.iamlong}} (IAM) `iam.cloud.ibm.com` endpoint. Added the `AllowInsecureBackendProxy=false` Kubernetes feature gate to prevent skipping TLS verification of kubelet during pod logs requests. |
| Kubernetes Dashboard | v2.0.0-rc2 | v2.0.0-rc3 | See the [Kubernetes Dashboard release notes](https://github.com/kubernetes/dashboard/releases/tag/v2.0.0-rc3){: external}. |
| Kubernetes Dashboard metrics scraper | v1.0.2 | v1.0.3 | See the [Kubernetes Dashboard metrics scraper release notes](https://github.com/kubernetes-sigs/dashboard-metrics-scraper/releases/tag/v1.0.3){: external}. |
| Kubernetes nodelocal DNS cache | 1.15.4 | 1.15.8 | See the [Kubernetes nodelocal DNS cache release notes](https://github.com/kubernetes/dns/releases/tag/1.15.8){: external}. Now in Kubernetes 1.17, when you [apply the label to set up node local DNS caching](/docs/containers?topic=containers-cluster_dns#dns_cache), the requests are handled immediately and you do not need to reload the worker nodes. |
| OpenVPN server | N/A | N/A | OpenVPN server is now restarted during [cluster master refresh](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_apiserver_refresh){: external}. |
| Operator Lifecycle Manager Catalog | v1.5.6 | v1.5.8 | See the [Operator Lifecycle Manager Catalog release notes](https://github.com/operator-framework/operator-registry/releases/tag/v1.5.8){: external}. |
| Operator Lifecycle Manager | 0.13.0 | 0.14.1 | See the [Operator Lifecycle Manager release notes](https://github.com/operator-framework/operator-lifecycle-manager/releases/tag/0.14.1){: external}. |
{: summary="The rows are read from left to right. The first column is the changed component. The second column is the previous version number of the component. The third column is the current version number of the component. The fourth column contains a brief description of the change made to the component."}
{: caption="Changes since version 1.16.5_1522" caption-side="top"}
