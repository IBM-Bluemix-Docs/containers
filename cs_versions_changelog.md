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



# Version changelog
{: #changelog}

View information of version changes for major, minor, and patch updates that are available for your {{site.data.keyword.containerlong}} Kubernetes clusters. Changes include updates to Kubernetes and {{site.data.keyword.cloud_notm}} Provider components.
{:shortdesc}

Unless otherwise noted in the changelogs, the {{site.data.keyword.containerlong_notm}} provider version enables Kubernetes APIs and features that are at beta. Kubernetes alpha features, which are subject to change, are disabled.

For more information about major, minor, and patch versions and preparation actions between minor versions, see [Kubernetes versions](/docs/containers?topic=containers-cs_versions).
{: tip}

For information about changes since the previous version, see the following changelogs.
- Version 1.14 [changelog](#114_changelog).
- Version 1.13 [changelog](#113_changelog).
- Version 1.12 [changelog](#112_changelog).
- [Archive](#changelog_archive) of changelogs for unsupported versions.

Some changelogs are for _worker node fix packs_, and apply only to worker nodes. You must [apply these patches](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update) to ensure security compliance for your worker nodes. These worker node fix packs can be at a higher version than the master because some build fix packs are specific to worker nodes. Other changelogs are for _master fix packs_, and apply only to the cluster master. Master fix packs might not be automatically applied. You can choose to [apply them manually](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_update). For more information about patch types, see [Update types](/docs/containers?topic=containers-cs_versions#update_types).
{: note}

</br>


## Version 1.14 changelog
{: #114_changelog}

### Changelog for worker node fix pack 1.14.4_1526, released 22 July 2019
{: #1144_1526_worker}

The following table shows the changes that are included in the worker node fix pack 1.14.4_1526.
{: shortdesc}

<table summary="Changes that were made since version 1.14.3_1525">
<caption>Changes since version 1.14.3_1525</caption>
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
<td>v1.14.3</td>
<td>v1.14.4</td>
<td>See the [Kubernetes release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/kubernetes/kubernetes/releases/tag/v1.14.4).</td>
</tr>
<tr>
<td>Ubuntu packages</td>
<td>N/A</td>
<td>N/A</td>
<td>Updated worker node images with package updates for [CVE-2019-13012 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-13012) and [CVE-2019-7307 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-7307.html).</td>
</tr>
</table>


### Changelog for master fix pack 1.14.4_1526, released 15 July 2019
{: #1144_1526}

The following table shows the changes that are included in the master fix pack 1.14.4_1526.
{: shortdesc}

<table summary="Changes that were made since version 1.14.3_1525">
<caption>Changes since version 1.14.3_1525</caption>
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
<td>Calico</td>
<td>v3.6.1</td>
<td>v3.6.4</td>
<td>See the [Calico release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://docs.projectcalico.org/v3.6/release-notes/). Update resolves [TTA-2019-001 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.projectcalico.org/security-bulletins/#TTA-2019-001).</td>
</tr>
<tr>
<td>CoreDNS configuration</td>
<td>N/A</td>
<td>N/A</td>
<td>Changed the default CoreDNS configuration from a 5 to 30 second TTL for DNS records in the `kubernetes` zone. This change aligns with the default KubeDNS configuration. Existing CoreDNS configurations are unchanged. For more information about changing your CoreDNS configuration, see [Customizing the cluster DNS provider](/docs/containers?topic=containers-cluster_dns#dns_customize).</td>
</tr>
<tr>
<td>GPU device plug-in and installer</td>
<td>5d34347</td>
<td>a7e8ece</td>
<td>Updated base image packages.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.14.3</td>
<td>v1.14.4</td>
<td>See the [Kubernetes release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/kubernetes/kubernetes/releases/tag/v1.14.4).</td>
</tr>
<tr>
<td>{{site.data.keyword.cloud_notm}} Provider</td>
<td>v1.14.3-113</td>
<td>v1.14.4-139</td>
<td>Updated to support the Kubernetes 1.14.4 release. Additionally, `calicoctl` version is updated to 3.6.4.</td>
</tr>
</table>

### Changelog for worker node fix pack 1.14.3_1525, released 8 July 2019
{: #1143_1525}

The following table shows the changes that are included in the worker node patch 1.14.3_1525.
{: shortdesc}

<table summary="Changes that were made since version 1.14.3_1524">
<caption>Changes since version 1.14.3_1524</caption>
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
<td>Ubuntu 16.04 kernel</td>
<td>4.4.0-151-generic</td>
<td>4.4.0-154-generic</td>
<td>Updated worker node images with kernel and package updates for [CVE-2019-11478 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-11478.html) and [CVE-2019-11479 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-11479.html).</td>
</tr>
<tr>
<td>Ubuntu 18.04 kernel</td>
<td>4.15.0-52-generic</td>
<td>4.15.0-54-generic</td>
<td>Updated worker node images with kernel and package updates for [CVE-2019-11478 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-11478.html) and [CVE-2019-11479 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-11479.html).</td>
</tr>
</tbody>
</table>


### Changelog for worker node fix pack 1.14.3_1524, released 24 June 2019
{: #1143_1524}

The following table shows the changes that are included in the worker node patch 1.14.3_1524.
{: shortdesc}

<table summary="Changes that were made since version 1.14.3_1523">
<caption>Changes since version 1.14.3_1523</caption>
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
<td>Ubuntu 16.04 kernel</td>
<td>4.4.0-150-generic</td>
<td>4.4.0-151-generic</td>
<td>Updated worker node images with kernel and package updates for [CVE-2019-11477 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-11477.html) and [CVE-2019-11478 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-11478.html).</td>
</tr>
<tr>
<td>Ubuntu 18.04 kernel</td>
<td>4.15.0-51-generic</td>
<td>4.15.0-52-generic</td>
<td>Updated worker node images with kernel and package updates for [CVE-2019-11477 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-11477.html) and [CVE-2019-11478 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-11478.html).</td>
</tr>
<tr>
<td>containerd</td>
<td>1.2.6</td>
<td>1.2.7</td>
<td>See the [containerd release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/containerd/containerd/releases/tag/v1.2.7).</td>
</tr>
<tr>
<td>Max pods</td>
<td>N/A</td>
<td>N/A</td>
<td>Increased the limit of maximum number of pods for worker nodes with more than 11 CPU cores to be 10 pods per core, up to a maximum of 250 pods per worker node.</td>
</tr>
</tbody>
</table>

### Changelog for 1.14.3_1523, released 17 June 2019
{: #1143_1523}

The following table shows the changes that are included in the patch 1.14.3_1523.
{: shortdesc}

<table summary="Changes that were made since version 1.14.2_1521">
<caption>Changes since version 1.14.2_1521</caption>
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
<td>GPU device plug-in and installer</td>
<td>32257d3</td>
<td>5d34347</td>
<td>Updated image for [CVE-2019-8457 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8457). Updated the GPU drivers to [430.14 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.nvidia.com/Download/driverResults.aspx/147582/).</td>
</tr>
<tr>
<td>{{site.data.keyword.cloud_notm}} File Storage plug-in</td>
<td>346</td>
<td>347</td>
<td>Updated so that the IAM API key can be either encrypted or unencrypted.</td>
</tr>
<tr>
<td>{{site.data.keyword.cloud_notm}} Provider</td>
<td>v1.14.2-100</td>
<td>v1.14.3-113</td>
<td>Updated to support the Kubernetes 1.14.3 release.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.14.2</td>
<td>v1.14.3</td>
<td>See the [Kubernetes release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/kubernetes/kubernetes/releases/tag/v1.14.3).</td>
</tr>
<tr>
<td>Kubernetes feature gates configuration</td>
<td>N/A</td>
<td>N/A</td>
<td>Added the `SupportNodePidsLimit=true` configuration to reserve process IDs (PIDs) for use by the operating system and Kubernetes components. Added the `CustomCPUCFSQuotaPeriod=true` configuration to mitigate CPU throttling problems.</td>
</tr>
<tr>
<td>Public service endpoint for Kubernetes master</td>
<td>N/A</td>
<td>N/A</td>
<td>Fixed an issue to [enable the public service endpoint](/docs/containers?topic=containers-cs_network_cluster#set-up-public-se).</td>
</tr>
<tr>
<td>Ubuntu 16.04 kernel</td>
<td>4.4.0-148-generic</td>
<td>4.4.0-150-generic</td>
<td>Updated worker node images with kernel and package updates for [CVE-2019-10906 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-10906.html?_ga=2.184456110.929090212.1560547312-1880639276.1557078470).</td>
</tr>
<tr>
<td>Ubuntu 18.04 kernel</td>
<td>4.15.0-50-generic</td>
<td>4.15.0-51-generic</td>
<td>Updated worker node images with kernel and package updates for [CVE-2019-10906 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-10906.html?_ga=2.184456110.929090212.1560547312-1880639276.1557078470).</td>
</tr>
</tbody>
</table>

### Changelog for 1.14.2_1521, released 4 June 2019
{: #1142_1521}

The following table shows the changes that are included in the patch 1.14.2_1521.
{: shortdesc}

<table summary="Changes that were made since version 1.14.1_1519">
<caption>Changes since version 1.14.1_1519</caption>
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
<td>Cluster DNS configuration</td>
<td>N/A</td>
<td>N/A</td>
<td>Fixed a bug that might leave both Kubernetes DNS and CoreDNS pods running after cluster `create` or `update` operations.</td>
</tr>
<tr>
<td>Cluster master HA configuration</td>
<td>N/A</td>
<td>N/A</td>
<td>Updated configuration to minimize intermittent master network connectivity failures during a master update.</td>
</tr>
<tr>
<td>etcd</td>
<td>v3.3.11</td>
<td>v3.3.13</td>
<td>See the [etcd release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/coreos/etcd/releases/v3.3.13).</td>
</tr>
<tr>
<td>GPU device plug-in and installer</td>
<td>55c1f66</td>
<td>32257d3</td>
<td>Updated image for [CVE-2018-10844 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10844), [CVE-2018-10845 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10845), [CVE-2018-10846 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10846), [CVE-2019-3829 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3829), [CVE-2019-3836 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3836), [CVE-2019-9893 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9893), [CVE-2019-5435 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5435), and [CVE-2019-5436 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5436).</td>
</tr>
<tr>
<td>{{site.data.keyword.cloud_notm}} Provider</td>
<td>v1.14.1-71</td>
<td>v1.14.2-100</td>
<td>Updated to support the Kubernetes 1.14.2 release.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.14.1</td>
<td>v1.14.2</td>
<td>See the [Kubernetes release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/kubernetes/kubernetes/releases/tag/v1.14.2).</td>
</tr>
<tr>
<td>Kubernetes Metrics Server</td>
<td>v0.3.1</td>
<td>v0.3.3</td>
<td>See the [Kubernetes Metrics Server release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/kubernetes-incubator/metrics-server/releases/tag/v0.3.3).</td>
</tr>
<tr>
<td>Trusted compute agent</td>
<td>13c7ef0</td>
<td>e8c6d72</td>
<td>Updated image for [CVE-2018-10844 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10844), [CVE-2018-10845 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10845), [CVE-2018-10846 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10846), [CVE-2019-3829 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3829), [CVE-2019-3836 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3836), [CVE-2019-9893 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9893), [CVE-2019-5435 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5435), and [CVE-2019-5436 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5436).</td>
</tr>
</tbody>
</table>

### Changelog for worker node fix pack 1.14.1_1519, released 20 May 2019
{: #1141_1519}

The following table shows the changes that are included in the patch 1.14.1_1519.
{: shortdesc}

<table summary="Changes that were made since version 1.14.1_1518">
<caption>Changes since version 1.14.1_1518</caption>
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
<td>Ubuntu 16.04 kernel</td>
<td>4.4.0-145-generic</td>
<td>4.4.0-148-generic</td>
<td>Updated worker node images with kernel update for [CVE-2018-12126 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12126.html), [CVE-2018-12127 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12127.html), and [CVE-2018-12130 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12130.html).</td>
</tr>
<tr>
<td>Ubuntu 18.04 kernel</td>
<td>4.15.0-47-generic</td>
<td>4.15.0-50-generic</td>
<td>Updated worker node images with kernel update for [CVE-2018-12126 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12126.html), [CVE-2018-12127 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12127.html), and [CVE-2018-12130 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12130.html).</td>
</tr>
</tbody>
</table>

### Changelog for 1.14.1_1518, released 13 May 2019
{: #1141_1518}

The following table shows the changes that are included in the patch 1.14.1_1518.
{: shortdesc}

<table summary="Changes that were made since version 1.14.1_1516">
<caption>Changes since version 1.14.1_1516</caption>
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
<td>Cluster master HA proxy</td>
<td>1.9.6-alpine</td>
<td>1.9.7-alpine</td>
<td>See the [HAProxy release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.haproxy.org/download/1.9/src/CHANGELOG). Update resolves [CVE-2019-6706 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6706).</td>
</tr>
<tr>
<td>Kubernetes configuration</td>
<td>N/A</td>
<td>N/A</td>
<td>The Kubernetes API server audit policy configuration is updated to not log the `/openapi/v2*` read-only URL. In addition, the Kubernetes controller manager configuration increased the validity duration of signed `kubelet` certificates from 1 to 3 years.</td>
</tr>
<tr>
<td>OpenVPN client configuration</td>
<td>N/A</td>
<td>N/A</td>
<td>The OpenVPN client `vpn-*` pod in the `kube-system` namespace now sets `dnsPolicy` to `Default` to prevent the pod from failing when cluster DNS is down.</td>
</tr>
<tr>
<td>Trusted compute agent</td>
<td>e7182c7</td>
<td>13c7ef0</td>
<td>Updated image for [CVE-2016-7076 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2016-7076) and [CVE-2017-1000368 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1000368).</td>
</tr>
</tbody>
</table>

### Changelog for 1.14.1_1516, released 7 May 2019
{: #1141_1516}

The following table shows the changes that are included in the patch 1.14.1_1516.
{: shortdesc}

<table summary="Changes that were made since version 1.13.5_1519">
<caption>Changes since version 1.13.5_1519</caption>
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
<td>Calico</td>
<td>v3.4.4</td>
<td>v3.6.1</td>
<td>See the [Calico release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://docs.projectcalico.org/v3.6/release-notes/).</td>
</tr>
<tr>
<td>CoreDNS</td>
<td>1.2.6</td>
<td>1.3.1</td>
<td>See the [CoreDNS release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://coredns.io/2019/01/13/coredns-1.3.1-release/). The update includes the addition of a [metrics port ![External link icon](../icons/launch-glyph.svg "External link icon")](https://coredns.io/plugins/metrics/) on the cluster DNS service. <br><br>CoreDNS is now the only supported cluster DNS provider. If you update a cluster to Kubernetes version 1.14 from an earlier version and used KubeDNS, KubeDNS is automatically migrated to CoreDNS during the cluster update. For more information or to test out CoreDNS before you update, see [Configure the cluster DNS provider](https://cloud.ibm.com/docs/containers?topic=containers-cluster_dns#cluster_dns).</td>
</tr>
<tr>
<td>GPU device plug-in and installer</td>
<td>9ff3fda</td>
<td>ed0dafc</td>
<td>Updated image for [CVE-2019-1543 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1543).</td>
</tr>
<tr>
<td>{{site.data.keyword.cloud_notm}} Provider</td>
<td>v1.13.5-107</td>
<td>v1.14.1-71</td>
<td>Updated to support the Kubernetes 1.14.1 release. Additionally, `calicoctl` version is updated to 3.6.1. Fixed updates to version 2.0 network load balancers (NLBs) with only one available worker node for the load balancer pods. Private load balancers now support running on [private edge workers nodes](/docs/containers?topic=containers-edge#edge).</td>
</tr>
<tr>
<td>IBM pod security policies</td>
<td>N/A</td>
<td>N/A</td>
<td>[IBM pod security policies](/docs/containers?topic=containers-psp#ibm_psp) are updated to support the Kubernetes [RunAsGroup ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/concepts/policy/pod-security-policy/#users-and-groups) feature.</td>
</tr>
<tr>
<td>`kubelet` configuration</td>
<td>N/A</td>
<td>N/A</td>
<td>Set the `--pod-max-pids` option to `14336` to prevent a single pod from consuming all process IDs on a worker node.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.13.5</td>
<td>v1.14.1</td>
<td>See the [Kubernetes release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/kubernetes/kubernetes/releases/tag/v1.14.1) and [Kubernetes 1.14 blog ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/blog/2019/03/25/kubernetes-1-14-release-announcement/).<br><br>The Kubernetes default role-based access control (RBAC) policies no longer grant access to [discovery and permission-checking APIs to unauthenticated users ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/reference/access-authn-authz/rbac/#discovery-roles). This change applies only to new version 1.14 clusters. If you update a cluster from a prior version, unauthenticated users still have access to the discovery and permission-checking APIs.</td>
</tr>
<tr>
<td>Kubernetes admission controllers configuration</td>
<td>N/A</td>
<td>N/A</td>
<td><ul>
<li>Added `NodeRestriction` to the `--enable-admission-plugins` option for the cluster's Kubernetes API server and configured the related cluster resources to support this security enhancement.</li>
<li>Removed `Initializers` from the `--enable-admission-plugins` option and `admissionregistration.k8s.io/v1alpha1=true` from the `--runtime-config` option for the cluster's Kubernetes API server because these APIs are no longer supported. Instead, you can use [Kubernetes admission webhooks ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/reference/access-authn-authz/extensible-admission-controllers/).</li></ul></td>
</tr>
<tr>
<td>Kubernetes DNS autoscaler</td>
<td>1.3.0</td>
<td>1.4.0</td>
<td>See the [Kubernetes DNS autoscaler release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/kubernetes-incubator/cluster-proportional-autoscaler/releases/tag/1.4.0).</td>
</tr>
<tr>
<td>Kubernetes feature gates configuration</td>
<td>N/A</td>
<td>N/A</td>
<td><ul>
  <li>Added `RuntimeClass=false` to disable selection of the container runtime configuration.</li>
  <li>Removed `ExperimentalCriticalPodAnnotation=true` because the `scheduler.alpha.kubernetes.io/critical-pod` pod annotation is no longer supported. Instead, you can use [Kubernetes pod priority ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/docs/containers?topic=containers-pod_priority#pod_priority).</li></ul></td>
</tr>
<tr>
<td>Trusted compute agent</td>
<td>e132aa4</td>
<td>e7182c7</td>
<td>Updated image for [CVE-2019-11068 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-11068).</td>
</tr>
</tbody>
</table>

<br />


## Version 1.13 changelog
{: #113_changelog}

Review the version 1.13 changelog.
{: shortdesc}

### Changelog for worker node fix pack 1.13.8_1529, released 22 July 2019
{: #1138_1529_worker}

The following table shows the changes that are included in the worker node fix pack 1.13.8_1529.
{: shortdesc}

<table summary="Changes that were made since version 1.13.7_1528">
<caption>Changes since version 1.13.7_1528</caption>
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
<td>v1.13.7</td>
<td>v1.13.8</td>
<td>See the [Kubernetes release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/kubernetes/kubernetes/releases/tag/v1.13.8).</td>
</tr>
<tr>
<td>Ubuntu packages</td>
<td>N/A</td>
<td>N/A</td>
<td>Updated worker node images with package updates for [CVE-2019-13012 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-13012) and [CVE-2019-7307 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-7307.html).</td>
</tr>
</table>


### Changelog for master fix pack 1.13.8_1529, released 15 July 2019
{: #1138_1529}

The following table shows the changes that are included in the master fix pack 1.13.8_1529.
{: shortdesc}

<table summary="Changes that were made since version 1.13.7_1528">
<caption>Changes since version 1.13.7_1528</caption>
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
<td>Calico</td>
<td>v3.4.4</td>
<td>v3.6.4</td>
<td>See the [Calico release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://docs.projectcalico.org/v3.6/release-notes/). Update resolves [TTA-2019-001 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.projectcalico.org/security-bulletins/#TTA-2019-001).</td>
</tr>
<tr>
<td>CoreDNS configuration</td>
<td>N/A</td>
<td>N/A</td>
<td>Changed the default CoreDNS configuration from a 5 to 30 second TTL for DNS records in the `kubernetes` zone. This change aligns with the default KubeDNS configuration. Existing CoreDNS configurations are unchanged. For more information about changing your CoreDNS configuration, see [Customizing the cluster DNS provider](/docs/containers?topic=containers-cluster_dns#dns_customize).</td>
</tr>
<tr>
<td>GPU device plug-in and installer</td>
<td>5d34347</td>
<td>a7e8ece</td>
<td>Updated base image packages.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.13.7</td>
<td>v1.13.8</td>
<td>See the [Kubernetes release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/kubernetes/kubernetes/releases/tag/v1.13.8).</td>
</tr>
<tr>
<td>{{site.data.keyword.cloud_notm}} Provider</td>
<td>v1.13.7-162</td>
<td>v1.13.8-188</td>
<td>Updated to support the Kubernetes 1.13.8 release. Additionally, `calicoctl` version is updated to 3.6.4.</td>
</tr>
</tbody>
</table>

### Changelog for worker node fix pack 1.13.7_1528, released 8 July 2019
{: #1137_1528}

The following table shows the changes that are included in the worker node patch 1.13.7_1528.
{: shortdesc}

<table summary="Changes that were made since version 1.13.7_1527">
<caption>Changes since version 1.13.7_1527</caption>
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
<td>Ubuntu 16.04 kernel</td>
<td>4.4.0-151-generic</td>
<td>4.4.0-154-generic</td>
<td>Updated worker node images with kernel and package updates for [CVE-2019-11478 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-11478.html) and [CVE-2019-11479 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-11479.html).</td>
</tr>
<tr>
<td>Ubuntu 18.04 kernel</td>
<td>4.15.0-52-generic</td>
<td>4.15.0-54-generic</td>
<td>Updated worker node images with kernel and package updates for [CVE-2019-11478 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-11478.html) and [CVE-2019-11479 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-11479.html).</td>
</tr>
</tbody>
</table>


### Changelog for worker node fix pack 1.13.7_1527, released 24 June 2019
{: #1137_1527}

The following table shows the changes that are included in the worker node patch 1.13.7_1527.
{: shortdesc}

<table summary="Changes that were made since version 1.13.7_1526">
<caption>Changes since version 1.13.7_1526</caption>
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
<td>Ubuntu 16.04 kernel</td>
<td>4.4.0-150-generic</td>
<td>4.4.0-151-generic</td>
<td>Updated worker node images with kernel and package updates for [CVE-2019-11477 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-11477.html) and [CVE-2019-11478 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-11478.html).</td>
</tr>
<tr>
<td>Ubuntu 18.04 kernel</td>
<td>4.15.0-51-generic</td>
<td>4.15.0-52-generic</td>
<td>Updated worker node images with kernel and package updates for [CVE-2019-11477 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-11477.html) and [CVE-2019-11478 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-11478.html).</td>
</tr>
<tr>
<td>containerd</td>
<td>1.2.6</td>
<td>1.2.7</td>
<td>See the [containerd release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/containerd/containerd/releases/tag/v1.2.7).</td>
</tr>
<tr>
<td>Max pods</td>
<td>N/A</td>
<td>N/A</td>
<td>Increased the limit of maximum number of pods for worker nodes with more than 11 CPU cores to be 10 pods per core, up to a maximum of 250 pods per worker node.</td>
</tr>
</tbody>
</table>

### Changelog for 1.13.7_1526, released 17 June 2019
{: #1137_1526}

The following table shows the changes that are included in the patch 1.13.7_1526.
{: shortdesc}

<table summary="Changes that were made since version 1.13.6_1524">
<caption>Changes since version 1.13.6_1524</caption>
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
<td>GPU device plug-in and installer</td>
<td>32257d3</td>
<td>5d34347</td>
<td>Updated image for [CVE-2019-8457 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8457). Updated the GPU drivers to [430.14 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.nvidia.com/Download/driverResults.aspx/147582/).</td>
</tr>
<tr>
<td>{{site.data.keyword.cloud_notm}} File Storage plug-in</td>
<td>346</td>
<td>347</td>
<td>Updated so that the IAM API key can be either encrypted or unencrypted.</td>
</tr>
<tr>
<td>{{site.data.keyword.cloud_notm}} Provider</td>
<td>v1.13.6-139</td>
<td>v1.13.7-162</td>
<td>Updated to support the Kubernetes 1.13.7 release.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.13.6</td>
<td>v1.13.7</td>
<td>See the [Kubernetes release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/kubernetes/kubernetes/releases/tag/v1.13.7).</td>
</tr>
<td>Public service endpoint for Kubernetes master</td>
<td>N/A</td>
<td>N/A</td>
<td>Fixed an issue to [enable the public service endpoint](/docs/containers?topic=containers-cs_network_cluster#set-up-public-se).</td>
</tr>
<tr>
<td>Ubuntu 16.04 kernel</td>
<td>4.4.0-148-generic</td>
<td>4.4.0-150-generic</td>
<td>Updated worker node images with kernel and package updates for [CVE-2019-10906 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-10906.html?_ga=2.184456110.929090212.1560547312-1880639276.1557078470).</td>
</tr>
<tr>
<td>Ubuntu 18.04 kernel</td>
<td>4.15.0-50-generic</td>
<td>4.15.0-51-generic</td>
<td>Updated worker node images with kernel and package updates for [CVE-2019-10906 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-10906.html?_ga=2.184456110.929090212.1560547312-1880639276.1557078470).</td>
</tr>
</tbody>
</table>

### Changelog for 1.13.6_1524, released 4 June 2019
{: #1136_1524}

The following table shows the changes that are included in the patch 1.13.6_1524.
{: shortdesc}

<table summary="Changes that were made since version 1.13.6_1522">
<caption>Changes since version 1.13.6_1522</caption>
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
<td>Cluster DNS configuration</td>
<td>N/A</td>
<td>N/A</td>
<td>Fixed a bug that might leave both Kubernetes DNS and CoreDNS pods running after cluster `create` or `update` operations.</td>
</tr>
<tr>
<td>Cluster master HA configuration</td>
<td>N/A</td>
<td>N/A</td>
<td>Updated configuration to minimize intermittent master network connectivity failures during a master update.</td>
</tr>
<tr>
<td>etcd</td>
<td>v3.3.11</td>
<td>v3.3.13</td>
<td>See the [etcd release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/coreos/etcd/releases/v3.3.13).</td>
</tr>
<tr>
<td>GPU device plug-in and installer</td>
<td>55c1f66</td>
<td>32257d3</td>
<td>Updated image for [CVE-2018-10844 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10844), [CVE-2018-10845 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10845), [CVE-2018-10846 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10846), [CVE-2019-3829 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3829), [CVE-2019-3836 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3836), [CVE-2019-9893 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9893), [CVE-2019-5435 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5435), and [CVE-2019-5436 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5436).</td>
</tr>
<tr>
<td>Kubernetes Metrics Server</td>
<td>v0.3.1</td>
<td>v0.3.3</td>
<td>See the [Kubernetes Metrics Server release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/kubernetes-incubator/metrics-server/releases/tag/v0.3.3).</td>
</tr>
<tr>
<td>Trusted compute agent</td>
<td>13c7ef0</td>
<td>e8c6d72</td>
<td>Updated image for [CVE-2018-10844 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10844), [CVE-2018-10845 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10845), [CVE-2018-10846 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10846), [CVE-2019-3829 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3829), [CVE-2019-3836 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3836), [CVE-2019-9893 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9893), [CVE-2019-5435 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5435), and [CVE-2019-5436 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5436).</td>
</tr>
</tbody>
</table>

### Changelog for worker node fix pack 1.13.6_1522, released 20 May 2019
{: #1136_1522}

The following table shows the changes that are included in the patch 1.13.6_1522.
{: shortdesc}

<table summary="Changes that were made since version 1.13.6_1521">
<caption>Changes since version 1.13.6_1521</caption>
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
<td>Ubuntu 16.04 kernel</td>
<td>4.4.0-145-generic</td>
<td>4.4.0-148-generic</td>
<td>Updated worker node images with kernel update for [CVE-2018-12126 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12126.html), [CVE-2018-12127 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12127.html), and [CVE-2018-12130 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12130.html).</td>
</tr>
<tr>
<td>Ubuntu 18.04 kernel</td>
<td>4.15.0-47-generic</td>
<td>4.15.0-50-generic</td>
<td>Updated worker node images with kernel update for [CVE-2018-12126 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12126.html), [CVE-2018-12127 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12127.html), and [CVE-2018-12130 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12130.html).</td>
</tr>
</tbody>
</table>

### Changelog for 1.13.6_1521, released 13 May 2019
{: #1136_1521}

The following table shows the changes that are included in the patch 1.13.6_1521.
{: shortdesc}

<table summary="Changes that were made since version 1.13.5_1519">
<caption>Changes since version 1.13.5_1519</caption>
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
<td>Cluster master HA proxy</td>
<td>1.9.6-alpine</td>
<td>1.9.7-alpine</td>
<td>See the [HAProxy release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.haproxy.org/download/1.9/src/CHANGELOG). Update resolves [CVE-2019-6706 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6706).</td>
</tr>
<tr>
<td>GPU device plug-in and installer</td>
<td>9ff3fda</td>
<td>55c1f66</td>
<td>Updated image for [CVE-2019-1543 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1543).</td>
</tr>
<tr>
<td>{{site.data.keyword.cloud_notm}} Provider</td>
<td>v1.13.5-107</td>
<td>v1.13.6-139</td>
<td>Updated to support the Kubernetes 1.13.6 release. Also, fixed the update process for version 2.0 network load balancer that have only one available worker node for the load balancer pods.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.13.5</td>
<td>v1.13.6</td>
<td>See the [Kubernetes release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/kubernetes/kubernetes/releases/tag/v1.13.6).</td>
</tr>
<tr>
<td>Kubernetes configuration</td>
<td>N/A</td>
<td>N/A</td>
<td>The Kubernetes API server audit policy configuration is updated to not log the `/openapi/v2*` read-only URL. In addition, the Kubernetes controller manager configuration increased the validity duration of signed `kubelet` certificates from 1 to 3 years.</td>
</tr>
<tr>
<td>OpenVPN client configuration</td>
<td>N/A</td>
<td>N/A</td>
<td>The OpenVPN client `vpn-*` pod in the `kube-system` namespace now sets `dnsPolicy` to `Default` to prevent the pod from failing when cluster DNS is down.</td>
</tr>
<tr>
<td>Trusted compute agent</td>
<td>e132aa4</td>
<td>13c7ef0</td>
<td>Updated image for [CVE-2016-7076 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2016-7076), [CVE-2017-1000368 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1000368), and [CVE-2019-11068 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-11068).</td>
</tr>
</tbody>
</table>

### Changelog for worker node fix pack 1.13.5_1519, released 29 April 2019
{: #1135_1519}

The following table shows the changes that are included in the worker node fix pack 1.13.5_1519.
{: shortdesc}

<table summary="Changes that were made since version 1.13.5_1518">
<caption>Changes since version 1.13.5_1518</caption>
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
<td>Ubuntu packages</td>
<td>N/A</td>
<td>N/A</td>
<td>Updates to installed Ubuntu packages.</td>
</tr>
<tr>
<td>containerd</td>
<td>1.2.5</td>
<td>1.2.6</td>
<td>See the [containerd release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/containerd/containerd/releases/tag/v1.2.6).</td>
</tr>
</tbody>
</table>

### Changelog for worker node fix pack 1.13.5_1518, released 15 April 2019
{: #1135_1518}

The following table shows the changes that are included in the worker node fix pack 1.13.5_1518.
{: shortdesc}

<table summary="Changes that were made since version 1.13.5_1517">
<caption>Changes since version 1.13.5_1517</caption>
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
<td>Ubuntu packages</td>
<td>N/A</td>
<td>N/A</td>
<td>Updates to installed Ubuntu packages including `systemd` for [CVE-2019-3842 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-3842.html).</td>
</tr>
</tbody>
</table>

### Changelog for 1.13.5_1517, released 8 April 2019
{: #1135_1517}

The following table shows the changes that are included in the patch 1.13.5_1517.
{: shortdesc}

<table summary="Changes that were made since version 1.13.4_1516">
<caption>Changes since version 1.13.4_1516</caption>
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
<td>Calico</td>
<td>v3.4.0</td>
<td>v3.4.4</td>
<td>See the [Calico release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://docs.projectcalico.org/v3.4/releases/#v344). Update resolves [CVE-2019-9946 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9946).</td>
</tr>
<tr>
<td>Cluster master HA proxy</td>
<td>1.8.12-alpine</td>
<td>1.9.6-alpine</td>
<td>See the [HAProxy release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.haproxy.org/download/1.9/src/CHANGELOG). Update resolves [CVE-2018-0732 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0732), [CVE-2018-0734 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734), [CVE-2018-0737 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0737), [CVE-2018-5407 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407), [CVE-2019-1543 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1543), and [CVE-2019-1559 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1559).</td>
</tr>
<tr>
<td>{{site.data.keyword.cloud_notm}} Provider</td>
<td>v1.13.4-86</td>
<td>v1.13.5-107</td>
<td>Updated to support the Kubernetes 1.13.5 and Calico 3.4.4 releases.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.13.4</td>
<td>v1.13.5</td>
<td>See the [Kubernetes release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/kubernetes/kubernetes/releases/tag/v1.13.5).</td>
</tr>
<tr>
<td>Trusted compute agent</td>
<td>a02f765</td>
<td>e132aa4</td>
<td>Updated image for [CVE-2017-12447 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-12447).</td>
</tr>
<tr>
<td>Ubuntu 16.04 kernel</td>
<td>4.4.0-143-generic</td>
<td>4.4.0-145-generic</td>
<td>Updated worker node images with kernel update for [CVE-2019-9213 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9213.html).</td>
</tr>
<tr>
<td>Ubuntu 18.04 kernel</td>
<td>4.15.0-46-generic</td>
<td>4.15.0-47-generic</td>
<td>Updated worker node images with kernel update for [CVE-2019-9213 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9213.html).</td>
</tr>
</tbody>
</table>

### Changelog for worker node fix pack 1.13.4_1516, released 1 April 2019
{: #1134_1516}

The following table shows the changes that are included in the worker node fix pack 1.13.4_1516.
{: shortdesc}

<table summary="Changes that were made since version 1.13.4_1515">
<caption>Changes since version 1.13.4_1515</caption>
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
<td>Worker node resource utilization</td>
<td>N/A</td>
<td>N/A</td>
<td>Increased memory reservations for the kubelet and containerd to prevent these components from running out of resources. For more information, see [Worker node resource reserves](/docs/containers?topic=containers-planning_worker_nodes#resource_limit_node).</td>
</tr>
</tbody>
</table>

### Changelog for master fix pack 1.13.4_1515, released 26 March 2019
{: #1134_1515}

The following table shows the changes that are included in the master fix pack 1.13.4_1515.
{: shortdesc}

<table summary="Changes that were made since version 1.13.4_1513">
<caption>Changes since version 1.13.4_1513</caption>
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
<td>Cluster DNS configuration</td>
<td>N/A</td>
<td>N/A</td>
<td>Fixed the update process from Kubernetes version 1.11 to prevent the update from switching the cluster DNS provider to CoreDNS. You can still [set up CoreDNS as the cluster DNS provider](/docs/containers?topic=containers-cluster_dns#set_coredns) after the update.</td>
</tr>
<tr>
<td>{{site.data.keyword.cloud_notm}} File Storage plug-in</td>
<td>345</td>
<td>346</td>
<td>Updated image for [CVE-2019-9741 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741).</td>
</tr>
<tr>
<td>Key Management Service provider</td>
<td>166</td>
<td>167</td>
<td>Fixes intermittent `context deadline exceeded` and `timeout` errors for managing Kubernetes secrets. In addition, fixes updates to the key management service that might leave existing Kubernetes secrets unencrypted. Update includes fix for [CVE-2019-9741 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741).</td>
</tr>
<tr>
<td>Load balancer and load balancer monitor for {{site.data.keyword.cloud_notm}} Provider</td>
<td>143</td>
<td>146</td>
<td>Updated image for [CVE-2019-9741 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741).</td>
</tr>
</tbody>
</table>

### Changelog for 1.13.4_1513, released 20 March 2019
{: #1134_1513}

The following table shows the changes that are included in the patch 1.13.4_1513.
{: shortdesc}

<table summary="Changes that were made since version 1.13.4_1510">
<caption>Changes since version 1.13.4_1510</caption>
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
<td>Cluster DNS configuration</td>
<td>N/A</td>
<td>N/A</td>
<td>Fixed a bug that might cause cluster master operations, such as `refresh` or `update`, to fail when the unused cluster DNS must be scaled down.</td>
</tr>
<tr>
<td>Cluster master HA proxy configuration</td>
<td>N/A</td>
<td>N/A</td>
<td>Updated configuration to better handle intermittent connection failures to the cluster master.</td>
</tr>
<tr>
<td>CoreDNS configuration</td>
<td>N/A</td>
<td>N/A</td>
<td>Updated the CoreDNS configuration to support [multiple Corefiles ![External link icon](../icons/launch-glyph.svg "External link icon")](https://coredns.io/2017/07/23/corefile-explained/) after updating the cluster Kubernetes version from 1.12.</td>
</tr>
<tr>
<td>containerd</td>
<td>1.2.4</td>
<td>1.2.5</td>
<td>See the [containerd release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/containerd/containerd/releases/tag/v1.2.5). Update includes improved fix for [CVE-2019-5736 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5736).</td>
</tr>
<tr>
<td>GPU device plug-in and installer</td>
<td>e32d51c</td>
<td>9ff3fda</td>
<td>Updated the GPU drivers to [418.43 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.nvidia.com/object/unix.html). Update includes fix for [CVE-2019-9741 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9741.html).</td>
</tr>
<tr>
<td>{{site.data.keyword.cloud_notm}} File Storage plug-in</td>
<td>344</td>
<td>345</td>
<td>Added support for [private service endpoints](/docs/containers?topic=containers-cs_network_cluster#set-up-private-se).</td>
</tr>
<tr>
<td>Kernel</td>
<td>4.4.0-141</td>
<td>4.4.0-143</td>
<td>Updated worker node images with kernel update for [CVE-2019-6133 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-6133.html).</td>
</tr>
<tr>
<td>Key Management Service provider</td>
<td>136</td>
<td>166</td>
<td>Updated image for [CVE-2018-16890 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-16890), [CVE-2019-3822 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3822), and [CVE-2019-3823 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3823).</td>
</tr>
<tr>
<td>Trusted compute agent</td>
<td>5f3d092</td>
<td>a02f765</td>
<td>Updated image for [CVE-2018-10779 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10779), [CVE-2018-12900 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-12900), [CVE-2018-17000 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-17000), [CVE-2018-19210 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-19210), [CVE-2019-6128 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6128), and [CVE-2019-7663 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-7663).</td>
</tr>
</tbody>
</table>

### Changelog for 1.13.4_1510, released 4 March 2019
{: #1134_1510}

The following table shows the changes that are included in the patch 1.13.4_1510.
{: shortdesc}

<table summary="Changes that were made since version 1.13.2_1509">
<caption>Changes since version 1.13.2_1509</caption>
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
<td>Cluster DNS provider</td>
<td>N/A</td>
<td>N/A</td>
<td>Increased Kubernetes DNS and CoreDNS pod memory limit from `170Mi` to `400Mi` in order to handle more cluster services.</td>
</tr>
<tr>
<td>GPU device plug-in and installer</td>
<td>eb3a259</td>
<td>e32d51c</td>
<td>Updated images for [CVE-2019-6454 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6454).</td>
</tr>
<tr>
<td>{{site.data.keyword.cloud_notm}} Provider</td>
<td>v1.13.2-62</td>
<td>v1.13.4-86</td>
<td>Updated to support the Kubernetes 1.13.4 release. Fixed periodic connectivity problems for version 1.0 load balancers that set `externalTrafficPolicy` to `local`. Updated load balancer version 1.0 and 2.0 events to use the latest {{site.data.keyword.cloud_notm}} documentation links.</td>
</tr>
<tr>
<td>{{site.data.keyword.cloud_notm}} File Storage plug-in</td>
<td>342</td>
<td>344</td>
<td>Changed the base operating system for the image from Fedora to Alpine. Updated image for [CVE-2019-6486 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486).</td>
</tr>
<tr>
<td>Key Management Service provider</td>
<td>122</td>
<td>136</td>
<td>Increased client timeout to {{site.data.keyword.keymanagementservicefull_notm}}. Updated image for [CVE-2019-6486 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486).</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.13.2</td>
<td>v1.13.4</td>
<td>See the [Kubernetes release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/kubernetes/kubernetes/releases/tag/v1.13.4). Update resolves [CVE-2019-6486 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486) and [CVE-2019-1002100 ![External link icon](../icons/launch-glyph.svg "External link icon")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1002100).</td>
</tr>
<tr>
<td>Kubernetes configuration</td>
<td>N/A</td>
<td>N/A</td>
<td>Added `ExperimentalCriticalPodAnnotation=true` to the `--feature-gates` option. This setting helps migrate pods from the deprecated `scheduler.alpha.kubernetes.io/critical-pod` annotation to [Kubernetes pod priority support](/docs/containers?topic=containers-pod_priority#pod_priority).</td>
</tr>
<tr>
<td>Load balancer and load balancer monitor for {{site.data.keyword.cloud_notm}} Provider</td>
<td>132</td>
<td>143</td>
<td>Updated image for [CVE-2019-6486 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486).</td>
</tr>
<tr>
<td>OpenVPN client and server</td>
<td>2.4.6-r3-IKS-13</td>
<td>2.4.6-r3-IKS-25</td>
<td>Updated image for [CVE-2019-1559 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1559).</td>
</tr>
<tr>
<td>Trusted compute agent</td>
<td>1ea5ad3</td>
<td>5f3d092</td>
<td>Updated image for [CVE-2019-6454 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6454).</td>
</tr>
</tbody>
</table>

### Changelog for worker node fix pack 1.13.2_1509, released 27 February 2019
{: #1132_1509}

The following table shows the changes that are included in the worker node fix pack 1.13.2_1509.
{: shortdesc}

<table summary="Changes that were made since version 1.13.2_1508">
<caption>Changes since version 1.13.2_1508</caption>
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
<td>Kernel</td>
<td>4.4.0-141</td>
<td>4.4.0-142</td>
<td>Updated worker node images with kernel update for [CVE-2018-19407 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-142.168/changelog).</td>
</tr>
</tbody>
</table>

### Changelog for worker node fix pack 1.13.2_1508, released 15 February 2019
{: #1132_1508}

The following table shows the changes that are included in the worker node fix pack 1.13.2_1508.
{: shortdesc}

<table summary="Changes that were made since version 1.13.2_1507">
<caption>Changes since version 1.13.2_1507</caption>
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
<td>Cluster master HA proxy configuration</td>
<td>N/A</td>
<td>N/A</td>
<td>Changed the pod configuration `spec.priorityClassName` value to `system-node-critical` and set the `spec.priority` value to `2000001000`.</td>
</tr>
<tr>
<td>containerd</td>
<td>1.2.2</td>
<td>1.2.4</td>
<td>See the [containerd release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/containerd/containerd/releases/tag/v1.2.4). Update resolves [CVE-2019-5736 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5736).</td>
</tr>
<tr>
<td>Kubernetes `kubelet` configuration</td>
<td>N/A</td>
<td>N/A</td>
<td>Enabled the `ExperimentalCriticalPodAnnotation` feature gate to prevent critical static pod eviction. Set the `event-qps` option to `0` to prevent rate limiting event creation.</td>
</tr>
</tbody>
</table>

### Changelog for 1.13.2_1507, released 5 February 2019
{: #1132_1507}

The following table shows the changes that are included in the patch 1.13.2_1507.
{: shortdesc}

<table summary="Changes that were made since version 1.12.4_1535">
<caption>Changes since version 1.12.4_1535</caption>
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
<td>Calico</td>
<td>v3.3.1</td>
<td>v3.4.0</td>
<td>See the [Calico release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://docs.projectcalico.org/v3.4/releases/#v340).</td>
</tr>
<tr>
<td>Cluster DNS provider</td>
<td>N/A</td>
<td>N/A</td>
<td>CoreDNS is now the default cluster DNS provider for new clusters. If you update an existing cluster to 1.13 that uses KubeDNS as the cluster DNS provider, KubeDNS continues to be the cluster DNS provider. However, you can choose to [use CoreDNS instead](/docs/containers?topic=containers-cluster_dns#dns_set).</td>
</tr>
<tr>
<td>containerd</td>
<td>1.1.5</td>
<td>1.2.2</td>
<td>See the [containerd release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/containerd/containerd/releases/tag/v1.2.2).</td>
</tr>
<tr>
<td>CoreDNS</td>
<td>1.2.2</td>
<td>1.2.6</td>
<td>See the [CoreDNS release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/coredns/coredns/releases/tag/v1.2.6). Additionally, the CoreDNS configuration is updated to [support multiple Corefiles ![External link icon](../icons/launch-glyph.svg "External link icon")](https://coredns.io/2017/07/23/corefile-explained/).</td>
</tr>
<tr>
<td>etcd</td>
<td>v3.3.1</td>
<td>v3.3.11</td>
<td>See the [etcd release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/coreos/etcd/releases/v3.3.11). Additionally, the supported cipher suites to etcd are now restricted to a subset with high strength encryption (128 bits or more).</td>
</tr>
<tr>
<td>GPU device plug-in and installer</td>
<td>13fdc0d</td>
<td>eb3a259</td>
<td>Updated images for [CVE-2019-3462 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3462) and [CVE-2019-6486 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486).</td>
</tr>
<tr>
<td>{{site.data.keyword.cloud_notm}} Provider</td>
<td>v1.12.4-118</td>
<td>v1.13.2-62</td>
<td>Updated to support the Kubernetes 1.13.2 release. Additionally, `calicoctl` version is updated to 3.4.0. Fixed unnecessary configuration updates to version 2.0 network load balancers on worker node status changes.</td>
</tr>
<tr>
<td>{{site.data.keyword.cloud_notm}} File Storage plug-in</td>
<td>338</td>
<td>342</td>
<td>The file storage plug-in is updated as follows:
<ul><li>Supports dynamic provisioning with [volume topology-aware scheduling](/docs/containers?topic=containers-file_storage#file-topology).</li>
<li>Ignores persistent volume claim (PVC) delete errors if the storage is already deleted.</li>
<li>Adds a failure message annotation to failed PVCs.</li>
<li>Optimizes the storage provisioner controller's leader election and resync period settings, and increases the provisioning timeout from 30 minutes to 1 hour.</li>
<li>Checks user permissions before starting the provisioning.</li>
<li>Adds a `CriticalAddonsOnly` toleration to the `ibm-file-plugin` and `ibm-storage-watcher` deployments in the `kube-system` namespace.</li></ul></td>
</tr>
<tr>
<td>Key Management Service provider</td>
<td>111</td>
<td>122</td>
<td>Added retry logic to avoid temporary failures when Kubernetes secrets are managed by {{site.data.keyword.keymanagementservicefull_notm}}.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.12.4</td>
<td>v1.13.2</td>
<td>See the [Kubernetes release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/kubernetes/kubernetes/releases/tag/v1.13.2).</td>
</tr>
<tr>
<td>Kubernetes configuration</td>
<td>N/A</td>
<td>N/A</td>
<td>The Kubernetes API server audit policy configuration is updated to include logging metadata for `cluster-admin` requests and logging the request body of workload `create`, `update`, and `patch` requests.</td>
</tr>
<tr>
<td>Kubernetes DNS autoscaler</td>
<td>1.2.0</td>
<td>1.3.0</td>
<td>See the [Kubernetes DNS autoscaler release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/kubernetes-incubator/cluster-proportional-autoscaler/releases/tag/1.3.0).</td>
</tr>
<tr>
<td>OpenVPN client</td>
<td>2.4.6-r3-IKS-8</td>
<td>2.4.6-r3-IKS-13</td>
<td>Updated image for [CVE-2018-0734 ![External link icon](../icons/launch-glyph.svg "External link icon")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734) and [CVE-2018-5407 ![External link icon](../icons/launch-glyph.svg "External link icon")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407). Added `CriticalAddonsOnly` toleration to the `vpn` deployment in the `kube-system` namespace. Additionally, the pod configuration is now obtained from a secret instead of from a configmap.</td>
</tr>
<tr>
<td>OpenVPN server</td>
<td>2.4.6-r3-IKS-8</td>
<td>2.4.6-r3-IKS-13</td>
<td>Updated image for [CVE-2018-0734 ![External link icon](../icons/launch-glyph.svg "External link icon")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734) and [CVE-2018-5407 ![External link icon](../icons/launch-glyph.svg "External link icon")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407).</td>
</tr>
<tr>
<td>systemd</td>
<td>230</td>
<td>229</td>
<td>Security patch for [CVE-2018-16864 ![External link icon](../icons/launch-glyph.svg "External link icon")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-16864).</td>
</tr>
</tbody>
</table>

<br />



## Version 1.12 changelog
{: #112_changelog}

Review the version 1.12 changelog.
{: shortdesc}

### Changelog for worker node fix pack 1.12.10_1560, released 22 July 2019
{: #11210_1560_worker}

The following table shows the changes that are included in the worker node fix pack 1.12.10_1560.
{: shortdesc}

<table summary="Changes that were made since version 1.12.9_1559">
<caption>Changes since version 1.12.9_1559</caption>
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
<td>v1.12.9</td>
<td>v1.12.10</td>
<td>See the [Kubernetes release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/kubernetes/kubernetes/releases/tag/v1.12.10).</td>
</tr>
<tr>
<td>Ubuntu packages</td>
<td>N/A</td>
<td>N/A</td>
<td>Updated worker node images with package updates for [CVE-2019-13012 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-13012) and [CVE-2019-7307 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-7307.html).</td>
</tr>
</table>


### Changelog for master fix pack 1.12.10_1560, released 15 July 2019
{: #11210_1560}

The following table shows the changes that are included in the master fix pack 1.12.10_1560.
{: shortdesc}

<table summary="Changes that were made since version 1.12.9_1559">
<caption>Changes since version 1.12.9_1559</caption>
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
<td>Calico</td>
<td>v3.3.6</td>
<td>v3.6.4</td>
<td>See the [Calico release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://docs.projectcalico.org/v3.6/release-notes/). Update resolves [TTA-2019-001 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.projectcalico.org/security-bulletins/#TTA-2019-001).
</td>
</tr>
<tr>
<td>CoreDNS configuration</td>
<td>N/A</td>
<td>N/A</td>
<td>Changed the default CoreDNS configuration from a 5 to 30 second TTL for DNS records in the `kubernetes` zone. This change aligns with the default KubeDNS configuration. Existing CoreDNS configurations are unchanged. For more information about changing your CoreDNS configuration, see [Customizing the cluster DNS provider](/docs/containers?topic=containers-cluster_dns#dns_customize).
</td>
</tr>
<tr>
<td>GPU device plug-in and installer</td>
<td>5d34347</td>
<td>a7e8ece</td>
<td>Updated base image packages.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.12.9</td>
<td>v1.12.10</td>
<td>See the [Kubernetes release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/kubernetes/kubernetes/releases/tag/v1.12.10).</td>
</tr>
<tr>
<td>{{site.data.keyword.cloud_notm}} Provider</td>
<td>v1.12.9-227</td>
<td>v1.12.10-259</td>
<td>Updated to support the Kubernetes 1.12.10 release. Additionally, `calicoctl` version is updated to 3.6.4.</td>
</tr>
</tbody>
</table>

### Changelog for worker node fix pack 1.12.9_1559, released 8 July 2019
{: #1129_1559}

The following table shows the changes that are included in the worker node patch 1.12.9_1559.
{: shortdesc}

<table summary="Changes that were made since version 1.12.9_1558">
<caption>Changes since version 1.12.9_1558</caption>
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
<td>Ubuntu 16.04 kernel</td>
<td>4.4.0-151-generic</td>
<td>4.4.0-154-generic</td>
<td>Updated worker node images with kernel and package updates for [CVE-2019-11478 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-11478.html) and [CVE-2019-11479 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-11479.html).</td>
</tr>
<tr>
<td>Ubuntu 18.04 kernel</td>
<td>4.15.0-52-generic</td>
<td>4.15.0-54-generic</td>
<td>Updated worker node images with kernel and package updates for [CVE-2019-11478 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-11478.html) and [CVE-2019-11479 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-11479.html).</td>
</tr>
</tbody>
</table>

### Changelog for worker node fix pack 1.12.9_1558, released 24 June 2019
{: #1129_1558}

The following table shows the changes that are included in the worker node patch 1.12.9_1558.
{: shortdesc}

<table summary="Changes that were made since version 1.12.9_1557">
<caption>Changes since version 1.12.9_1557</caption>
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
<td>Ubuntu 16.04 kernel</td>
<td>4.4.0-150-generic</td>
<td>4.4.0-151-generic</td>
<td>Updated worker node images with kernel and package updates for [CVE-2019-11477 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-11477.html) and [CVE-2019-11478 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-11478.html).</td>
</tr>
<tr>
<td>Ubuntu 18.04 kernel</td>
<td>4.15.0-51-generic</td>
<td>4.15.0-52-generic</td>
<td>Updated worker node images with kernel and package updates for [CVE-2019-11477 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-11477.html) and [CVE-2019-11478 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-11478.html).</td>
</tr>
<tr>
<td>containerd</td>
<td>1.2.6</td>
<td>1.2.7</td>
<td>See the [containerd release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/containerd/containerd/releases/tag/v1.2.7).</td>
</tr>
</tbody>
</table>

### Changelog for 1.12.9_1557, released 17 June 2019
{: #1129_1557}

The following table shows the changes that are included in the patch 1.12.9_1557.
{: shortdesc}

<table summary="Changes that were made since version 1.12.9_1555">
<caption>Changes since version 1.12.9_1555</caption>
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
<td>GPU device plug-in and installer</td>
<td>32257d3</td>
<td>5d34347</td>
<td>Updated image for [CVE-2019-8457 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8457). Updated the GPU drivers to [430.14 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.nvidia.com/Download/driverResults.aspx/147582/).</td>
</tr>
<tr>
<td>{{site.data.keyword.cloud_notm}} File Storage plug-in</td>
<td>346</td>
<td>347</td>
<td>Updated so that the IAM API key can be either encrypted or unencrypted.</td>
</tr>
<td>Public service endpoint for Kubernetes master</td>
<td>N/A</td>
<td>N/A</td>
<td>Fixed an issue to [enable the public service endpoint](/docs/containers?topic=containers-cs_network_cluster#set-up-public-se).</td>
</tr>
<tr>
<td>Ubuntu 16.04 kernel</td>
<td>4.4.0-148-generic</td>
<td>4.4.0-150-generic</td>
<td>Updated worker node images with kernel and package updates for [CVE-2019-10906 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-10906.html?_ga=2.184456110.929090212.1560547312-1880639276.1557078470).</td>
</tr>
<tr>
<td>Ubuntu 18.04 kernel</td>
<td>4.15.0-50-generic</td>
<td>4.15.0-51-generic</td>
<td>Updated worker node images with kernel and package updates for [CVE-2019-10906 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-10906.html?_ga=2.184456110.929090212.1560547312-1880639276.1557078470).</td>
</tr>
</tbody>
</table>

### Changelog for 1.12.9_1555, released 4 June 2019
{: #1129_1555}

The following table shows the changes that are included in the patch 1.12.9_1555.
{: shortdesc}

<table summary="Changes that were made since version 1.12.8_1553">
<caption>Changes since version 1.12.8_1553</caption>
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
<td>Cluster DNS configuration</td>
<td>N/A</td>
<td>N/A</td>
<td>Fixed a bug that might leave both Kubernetes DNS and CoreDNS pods running after cluster `create` or `update` operations.</td>
</tr>
<tr>
<td>Cluster master HA configuration</td>
<td>N/A</td>
<td>N/A</td>
<td>Updated configuration to minimize intermittent master network connectivity failures during a master update.</td>
</tr>
<tr>
<td>etcd</td>
<td>v3.3.11</td>
<td>v3.3.13</td>
<td>See the [etcd release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/coreos/etcd/releases/v3.3.13).</td>
</tr>
<tr>
<td>GPU device plug-in and installer</td>
<td>55c1f66</td>
<td>32257d3</td>
<td>Updated image for [CVE-2018-10844 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10844), [CVE-2018-10845 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10845), [CVE-2018-10846 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10846), [CVE-2019-3829 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3829), [CVE-2019-3836 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3836), [CVE-2019-9893 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9893), [CVE-2019-5435 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5435), and [CVE-2019-5436 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5436).</td>
</tr>
<tr>
<td>{{site.data.keyword.cloud_notm}} Provider</td>
<td>v1.12.8-210</td>
<td>v1.12.9-227</td>
<td>Updated to support the Kubernetes 1.12.9 release.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.12.8</td>
<td>v1.12.9</td>
<td>See the [Kubernetes release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/kubernetes/kubernetes/releases/tag/v1.12.9).</td>
</tr>
<tr>
<td>Kubernetes Metrics Server</td>
<td>v0.3.1</td>
<td>v0.3.3</td>
<td>See the [Kubernetes Metrics Server release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/kubernetes-incubator/metrics-server/releases/tag/v0.3.3).</td>
</tr>
<tr>
<td>Trusted compute agent</td>
<td>13c7ef0</td>
<td>e8c6d72</td>
<td>Updated image for [CVE-2018-10844 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10844), [CVE-2018-10845 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10845), [CVE-2018-10846 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10846), [CVE-2019-3829 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3829), [CVE-2019-3836 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3836), [CVE-2019-9893 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9893), [CVE-2019-5435 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5435), and [CVE-2019-5436 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5436).</td>
</tr>
</tbody>
</table>

### Changelog for worker node fix pack 1.12.8_1553, released 20 May 2019
{: #1128_1533}

The following table shows the changes that are included in the patch 1.12.8_1553.
{: shortdesc}

<table summary="Changes that were made since version 1.12.8_1553">
<caption>Changes since version 1.12.8_1553</caption>
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
<td>Ubuntu 16.04 kernel</td>
<td>4.4.0-145-generic</td>
<td>4.4.0-148-generic</td>
<td>Updated worker node images with kernel update for [CVE-2018-12126 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12126.html), [CVE-2018-12127 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12127.html), and [CVE-2018-12130 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12130.html).</td>
</tr>
<tr>
<td>Ubuntu 18.04 kernel</td>
<td>4.15.0-47-generic</td>
<td>4.15.0-50-generic</td>
<td>Updated worker node images with kernel update for [CVE-2018-12126 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12126.html), [CVE-2018-12127 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12127.html), and [CVE-2018-12130 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12130.html).</td>
</tr>
</tbody>
</table>

### Changelog for 1.12.8_1552, released 13 May 2019
{: #1128_1552}

The following table shows the changes that are included in the patch 1.12.8_1552.
{: shortdesc}

<table summary="Changes that were made since version 1.12.7_1550">
<caption>Changes since version 1.12.7_1550</caption>
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
<td>Cluster master HA proxy</td>
<td>1.9.6-alpine</td>
<td>1.9.7-alpine</td>
<td>See the [HAProxy release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.haproxy.org/download/1.9/src/CHANGELOG). Update resolves [CVE-2019-6706 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6706).</td>
</tr>
<tr>
<td>GPU device plug-in and installer</td>
<td>9ff3fda</td>
<td>55c1f66</td>
<td>Updated image for [CVE-2019-1543 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1543).</td>
</tr>
<tr>
<td>{{site.data.keyword.cloud_notm}} Provider</td>
<td>v1.12.7-180</td>
<td>v1.12.8-210</td>
<td>Updated to support the Kubernetes 1.12.8 release. Also, fixed the update process for version 2.0 network load balancer that have only one available worker node for the load balancer pods.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.12.7</td>
<td>v1.12.8</td>
<td>See the [Kubernetes release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/kubernetes/kubernetes/releases/tag/v1.12.8).</td>
</tr>
<tr>
<td>Kubernetes configuration</td>
<td>N/A</td>
<td>N/A</td>
<td>The Kubernetes API server audit policy configuration is updated to not log the `/openapi/v2*` read-only URL. In addition, the Kubernetes controller manager configuration increased the validity duration of signed `kubelet` certificates from 1 to 3 years.</td>
</tr>
<tr>
<td>OpenVPN client configuration</td>
<td>N/A</td>
<td>N/A</td>
<td>The OpenVPN client `vpn-*` pod in the `kube-system` namespace now sets `dnsPolicy` to `Default` to prevent the pod from failing when cluster DNS is down.</td>
</tr>
<tr>
<td>Trusted compute agent</td>
<td>e132aa4</td>
<td>13c7ef0</td>
<td>Updated image for [CVE-2016-7076 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2016-7076), [CVE-2017-1000368 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1000368), and [CVE-2019-11068 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-11068).</td>
</tr>
</tbody>
</table>

### Changelog for worker node fix pack 1.12.7_1550, released 29 April 2019
{: #1127_1550}

The following table shows the changes that are included in the worker node fix pack 1.12.7_1550.
{: shortdesc}

<table summary="Changes that were made since version 1.12.7_1549">
<caption>Changes since version 1.12.7_1549</caption>
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
<td>Ubuntu packages</td>
<td>N/A</td>
<td>N/A</td>
<td>Updates to installed Ubuntu packages.</td>
</tr>
<tr>
<td>containerd</td>
<td>1.1.6</td>
<td>1.1.7</td>
<td>See the [containerd release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/containerd/containerd/releases/tag/v1.1.7).</td>
</tr>
</tbody>
</table>


### Changelog for worker node fix pack 1.12.7_1549, released 15 April 2019
{: #1127_1549}

The following table shows the changes that are included in the worker node fix pack 1.12.7_1549.
{: shortdesc}

<table summary="Changes that were made since version 1.12.7_1548">
<caption>Changes since version 1.12.7_1548</caption>
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
<td>Ubuntu packages</td>
<td>N/A</td>
<td>N/A</td>
<td>Updates to installed Ubuntu packages including `systemd` for [CVE-2019-3842 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-3842.html).</td>
</tr>
</tbody>
</table>

### Changelog for 1.12.7_1548, released 8 April 2019
{: #1127_1548}

The following table shows the changes that are included in the patch 1.12.7_1548.
{: shortdesc}

<table summary="Changes that were made since version 1.12.6_1547">
<caption>Changes since version 1.12.6_1547</caption>
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
<td>Calico</td>
<td>v3.3.1</td>
<td>v3.3.6</td>
<td>See the [Calico release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://docs.projectcalico.org/v3.3/releases/#v336). Update resolves [CVE-2019-9946 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9946).</td>
</tr>
<tr>
<td>Cluster master HA proxy</td>
<td>1.8.12-alpine</td>
<td>1.9.6-alpine</td>
<td>See the [HAProxy release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.haproxy.org/download/1.9/src/CHANGELOG). Update resolves [CVE-2018-0732 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0732), [CVE-2018-0734 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734), [CVE-2018-0737 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0737), [CVE-2018-5407 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407), [CVE-2019-1543 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1543), and [CVE-2019-1559 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1559).</td>
</tr>
<tr>
<td>{{site.data.keyword.cloud_notm}} Provider</td>
<td>v1.12.6-157</td>
<td>v1.12.7-180</td>
<td>Updated to support the Kubernetes 1.12.7 and Calico 3.3.6 releases.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.12.6</td>
<td>v1.12.7</td>
<td>See the [Kubernetes release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/kubernetes/kubernetes/releases/tag/v1.12.7).</td>
</tr>
<tr>
<td>Trusted compute agent</td>
<td>a02f765</td>
<td>e132aa4</td>
<td>Updated image for [CVE-2017-12447 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-12447).</td>
</tr>
<tr>
<td>Ubuntu 16.04 kernel</td>
<td>4.4.0-143-generic</td>
<td>4.4.0-145-generic</td>
<td>Updated worker node images with kernel update for [CVE-2019-9213 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9213.html).</td>
</tr>
<tr>
<td>Ubuntu 18.04 kernel</td>
<td>4.15.0-46-generic</td>
<td>4.15.0-47-generic</td>
<td>Updated worker node images with kernel update for [CVE-2019-9213 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9213.html).</td>
</tr>
</tbody>
</table>

### Changelog for worker node fix pack 1.12.6_1547, released 1 April 2019
{: #1126_1547}

The following table shows the changes that are included in the worker node fix pack 1.12.6_1547.
{: shortdesc}

<table summary="Changes that were made since version 1.12.6_1546">
<caption>Changes since version 1.12.6_1546</caption>
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
<td>Worker node resource utilization</td>
<td>N/A</td>
<td>N/A</td>
<td>Increased memory reservations for the kubelet and containerd to prevent these components from running out of resources. For more information, see [Worker node resource reserves](/docs/containers?topic=containers-planning_worker_nodes#resource_limit_node).</td>
</tr>
</tbody>
</table>


### Changelog for master fix pack 1.12.6_1546, released 26 March 2019
{: #1126_1546}

The following table shows the changes that are included in the master fix pack 1.12.6_1546.
{: shortdesc}

<table summary="Changes that were made since version 1.12.6_1544">
<caption>Changes since version 1.12.6_1544</caption>
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
<td>{{site.data.keyword.cloud_notm}} File Storage plug-in</td>
<td>345</td>
<td>346</td>
<td>Updated image for [CVE-2019-9741 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741).</td>
</tr>
<tr>
<td>Key Management Service provider</td>
<td>166</td>
<td>167</td>
<td>Fixes intermittent `context deadline exceeded` and `timeout` errors for managing Kubernetes secrets. In addition, fixes updates to the key management service that might leave existing Kubernetes secrets unencrypted. Update includes fix for [CVE-2019-9741 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741).</td>
</tr>
<tr>
<td>Load balancer and load balancer monitor for {{site.data.keyword.cloud_notm}} Provider</td>
<td>143</td>
<td>146</td>
<td>Updated image for [CVE-2019-9741 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741).</td>
</tr>
</tbody>
</table>

### Changelog for 1.12.6_1544, released 20 March 2019
{: #1126_1544}

The following table shows the changes that are included in the patch 1.12.6_1544.
{: shortdesc}

<table summary="Changes that were made since version 1.12.6_1541">
<caption>Changes since version 1.12.6_1541</caption>
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
<td>Cluster DNS configuration</td>
<td>N/A</td>
<td>N/A</td>
<td>Fixed a bug that might cause cluster master operations, such as `refresh` or `update`, to fail when the unused cluster DNS must be scaled down.</td>
</tr>
<tr>
<td>Cluster master HA proxy configuration</td>
<td>N/A</td>
<td>N/A</td>
<td>Updated configuration to better handle intermittent connection failures to the cluster master.</td>
</tr>
<tr>
<td>CoreDNS configuration</td>
<td>N/A</td>
<td>N/A</td>
<td>Updated the CoreDNS configuration to support [multiple Corefiles ![External link icon](../icons/launch-glyph.svg "External link icon")](https://coredns.io/2017/07/23/corefile-explained/).</td>
</tr>
<tr>
<td>GPU device plug-in and installer</td>
<td>e32d51c</td>
<td>9ff3fda</td>
<td>Updated the GPU drivers to [418.43 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.nvidia.com/object/unix.html). Update includes fix for [CVE-2019-9741 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9741.html).</td>
</tr>
<tr>
<td>{{site.data.keyword.cloud_notm}} File Storage plug-in</td>
<td>344</td>
<td>345</td>
<td>Added support for [private service endpoints](/docs/containers?topic=containers-cs_network_cluster#set-up-private-se).</td>
</tr>
<tr>
<td>Kernel</td>
<td>4.4.0-141</td>
<td>4.4.0-143</td>
<td>Updated worker node images with kernel update for [CVE-2019-6133 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-6133.html).</td>
</tr>
<tr>
<td>Key Management Service provider</td>
<td>136</td>
<td>166</td>
<td>Updated image for [CVE-2018-16890 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-16890), [CVE-2019-3822 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3822), and [CVE-2019-3823 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3823).</td>
</tr>
<tr>
<td>Trusted compute agent</td>
<td>5f3d092</td>
<td>a02f765</td>
<td>Updated image for [CVE-2018-10779 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10779), [CVE-2018-12900 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-12900), [CVE-2018-17000 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-17000), [CVE-2018-19210 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-19210), [CVE-2019-6128 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6128), and [CVE-2019-7663 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-7663).</td>
</tr>
</tbody>
</table>

### Changelog for 1.12.6_1541, released 4 March 2019
{: #1126_1541}

The following table shows the changes that are included in the patch 1.12.6_1541.
{: shortdesc}

<table summary="Changes that were made since version 1.12.5_1540">
<caption>Changes since version 1.12.5_1540</caption>
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
<td>Cluster DNS provider</td>
<td>N/A</td>
<td>N/A</td>
<td>Increased Kubernetes DNS and CoreDNS pod memory limit from `170Mi` to `400Mi` in order to handle more cluster services.</td>
</tr>
<tr>
<td>GPU device plug-in and installer</td>
<td>eb3a259</td>
<td>e32d51c</td>
<td>Updated images for [CVE-2019-6454 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6454).</td>
</tr>
<tr>
<td>{{site.data.keyword.cloud_notm}} Provider</td>
<td>v1.12.5-137</td>
<td>v1.12.6-157</td>
<td>Updated to support the Kubernetes 1.12.6 release. Fixed periodic connectivity problems for version 1.0 load balancers that set `externalTrafficPolicy` to `local`. Updated load balancer version 1.0 and 2.0 events to use the latest {{site.data.keyword.cloud_notm}} documentation links.</td>
</tr>
<tr>
<td>{{site.data.keyword.cloud_notm}} File Storage plug-in</td>
<td>342</td>
<td>344</td>
<td>Changed the base operating system for the image from Fedora to Alpine. Updated image for [CVE-2019-6486 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486).</td>
</tr>
<tr>
<td>Key Management Service provider</td>
<td>122</td>
<td>136</td>
<td>Increased client timeout to {{site.data.keyword.keymanagementservicefull_notm}}. Updated image for [CVE-2019-6486 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486).</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.12.5</td>
<td>v1.12.6</td>
<td>See the [Kubernetes release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/kubernetes/kubernetes/releases/tag/v1.12.6). Update resolves [CVE-2019-6486 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486) and [CVE-2019-1002100 ![External link icon](../icons/launch-glyph.svg "External link icon")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1002100).</td>
</tr>
<tr>
<td>Kubernetes configuration</td>
<td>N/A</td>
<td>N/A</td>
<td>Added `ExperimentalCriticalPodAnnotation=true` to the `--feature-gates` option. This setting helps migrate pods from the deprecated `scheduler.alpha.kubernetes.io/critical-pod` annotation to [Kubernetes pod priority support](/docs/containers?topic=containers-pod_priority#pod_priority).</td>
</tr>
<tr>
<td>Load balancer and load balancer monitor for {{site.data.keyword.cloud_notm}} Provider</td>
<td>132</td>
<td>143</td>
<td>Updated image for [CVE-2019-6486 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486).</td>
</tr>
<tr>
<td>OpenVPN client and server</td>
<td>2.4.6-r3-IKS-13</td>
<td>2.4.6-r3-IKS-25</td>
<td>Updated image for [CVE-2019-1559 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1559).</td>
</tr>
<tr>
<td>Trusted compute agent</td>
<td>1ea5ad3</td>
<td>5f3d092</td>
<td>Updated image for [CVE-2019-6454 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6454).</td>
</tr>
</tbody>
</table>

### Changelog for worker node fix pack 1.12.5_1540, released 27 February 2019
{: #1125_1540}

The following table shows the changes that are included in the worker node fix pack 1.12.5_1540.
{: shortdesc}

<table summary="Changes that were made since version 1.12.5_1538">
<caption>Changes since version 1.12.5_1538</caption>
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
<td>Kernel</td>
<td>4.4.0-141</td>
<td>4.4.0-142</td>
<td>Updated worker node images with kernel update for [CVE-2018-19407 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-142.168/changelog).</td>
</tr>
</tbody>
</table>

### Changelog for worker node fix pack 1.12.5_1538, released 15 February 2019
{: #1125_1538}

The following table shows the changes that are included in the worker node fix pack 1.12.5_1538.
{: shortdesc}

<table summary="Changes that were made since version 1.12.5_1537">
<caption>Changes since version 1.12.5_1537</caption>
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
<td>Cluster master HA proxy configuration</td>
<td>N/A</td>
<td>N/A</td>
<td>Changed the pod configuration `spec.priorityClassName` value to `system-node-critical` and set the `spec.priority` value to `2000001000`.</td>
</tr>
<tr>
<td>containerd</td>
<td>1.1.5</td>
<td>1.1.6</td>
<td>See the [containerd release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/containerd/containerd/releases/tag/v1.1.6). Update resolves [CVE-2019-5736 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5736).</td>
</tr>
<tr>
<td>Kubernetes `kubelet` configuration</td>
<td>N/A</td>
<td>N/A</td>
<td>Enabled the `ExperimentalCriticalPodAnnotation` feature gate to prevent critical static pod eviction.</td>
</tr>
</tbody>
</table>

### Changelog for 1.12.5_1537, released 5 February 2019
{: #1125_1537}

The following table shows the changes that are included in the patch 1.12.5_1537.
{: shortdesc}

<table summary="Changes that were made since version 1.12.4_1535">
<caption>Changes since version 1.12.4_1535</caption>
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
<td>etcd</td>
<td>v3.3.1</td>
<td>v3.3.11</td>
<td>See the [etcd release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/coreos/etcd/releases/v3.3.11). Additionally, the supported cipher suites to etcd are now restricted to a subset with high strength encryption (128 bits or more).</td>
</tr>
<tr>
<td>GPU device plug-in and installer</td>
<td>13fdc0d</td>
<td>eb3a259</td>
<td>Updated images for [CVE-2019-3462 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3462) and [CVE-2019-6486 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486).</td>
</tr>
<tr>
<td>{{site.data.keyword.cloud_notm}} Provider</td>
<td>v1.12.4-118</td>
<td>v1.12.5-137</td>
<td>Updated to support the Kubernetes 1.12.5 release. Additionally, `calicoctl` version is updated to 3.3.1. Fixed unnecessary configuration updates to version 2.0 network load balancers on worker node status changes.</td>
</tr>
<tr>
<td>{{site.data.keyword.cloud_notm}} File Storage plug-in</td>
<td>338</td>
<td>342</td>
<td>The file storage plug-in is updated as follows:
<ul><li>Supports dynamic provisioning with [volume topology-aware scheduling](/docs/containers?topic=containers-file_storage#file-topology).</li>
<li>Ignores persistent volume claim (PVC) delete errors if the storage is already deleted.</li>
<li>Adds a failure message annotation to failed PVCs.</li>
<li>Optimizes the storage provisioner controller's leader election and resync period settings, and increases the provisioning timeout from 30 minutes to 1 hour.</li>
<li>Checks user permissions before starting the provisioning.</li></ul></td>
</tr>
<tr>
<td>Key Management Service provider</td>
<td>111</td>
<td>122</td>
<td>Added retry logic to avoid temporary failures when Kubernetes secrets are managed by {{site.data.keyword.keymanagementservicefull_notm}}.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.12.4</td>
<td>v1.12.5</td>
<td>See the [Kubernetes release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/kubernetes/kubernetes/releases/tag/v1.12.5).</td>
</tr>
<tr>
<td>Kubernetes configuration</td>
<td>N/A</td>
<td>N/A</td>
<td>The Kubernetes API server audit policy configuration is updated to include logging metadata for `cluster-admin` requests and logging the request body of workload `create`, `update`, and `patch` requests.</td>
</tr>
<tr>
<td>OpenVPN client</td>
<td>2.4.6-r3-IKS-8</td>
<td>2.4.6-r3-IKS-13</td>
<td>Updated image for [CVE-2018-0734 ![External link icon](../icons/launch-glyph.svg "External link icon")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734) and [CVE-2018-5407 ![External link icon](../icons/launch-glyph.svg "External link icon")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407). Additionally, the pod configuration is now obtained from a secret instead of from a configmap.</td>
</tr>
<tr>
<td>OpenVPN server</td>
<td>2.4.6-r3-IKS-8</td>
<td>2.4.6-r3-IKS-13</td>
<td>Updated image for [CVE-2018-0734 ![External link icon](../icons/launch-glyph.svg "External link icon")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734) and [CVE-2018-5407 ![External link icon](../icons/launch-glyph.svg "External link icon")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407).</td>
</tr>
<tr>
<td>systemd</td>
<td>230</td>
<td>229</td>
<td>Security patch for [CVE-2018-16864 ![External link icon](../icons/launch-glyph.svg "External link icon")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-16864).</td>
</tr>
</tbody>
</table>

### Changelog for worker node fix pack 1.12.4_1535, released 28 January 2019
{: #1124_1535}

The following table shows the changes that are included in the worker node fix pack 1.12.4_1535.
{: shortdesc}

<table summary="Changes that were made since version 1.12.4_1534">
<caption>Changes since version 1.12.4_1534</caption>
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
<td>Ubuntu packages</td>
<td>N/A</td>
<td>N/A</td>
<td>Updates to installed Ubuntu packages including `apt` for [CVE-2019-3462 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3462) and [USN-3863-1 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://usn.ubuntu.com/3863-1).</td>
</tr>
</tbody>
</table>


### Changelog for 1.12.4_1534, released 21 January 2019
{: #1124_1534}

The following table shows the changes that are included in the patch 1.12.3_1534.
{: shortdesc}

<table summary="Changes that were made since version 1.12.3_1533">
<caption>Changes since version 1.12.3_1533</caption>
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
<td>{{site.data.keyword.cloud_notm}} Provider</td>
<td>v1.12.3-91</td>
<td>v1.12.4-118</td>
<td>Updated to support the Kubernetes 1.12.4 release.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.12.3</td>
<td>v1.12.4</td>
<td>See the [Kubernetes release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/kubernetes/kubernetes/releases/tag/v1.12.4).</td>
</tr>
<tr>
<td>Kubernetes add-on resizer</td>
<td>1.8.1</td>
<td>1.8.4</td>
<td>See the [Kubernetes add-on resizer release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/kubernetes/autoscaler/releases/tag/addon-resizer-1.8.4).</td>
</tr>
<tr>
<td>Kubernetes dashboard</td>
<td>v1.8.3</td>
<td>v1.10.1</td>
<td>See the [Kubernetes dashboard release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/kubernetes/dashboard/releases/tag/v1.10.1). Update resolves [CVE-2018-18264 ![External link icon](../icons/launch-glyph.svg "External link icon")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-18264).</td>
</tr>
<tr>
<td>GPU installer</td>
<td>390.12</td>
<td>410.79</td>
<td>Updated the installed GPU drivers to 410.79.</td>
</tr>
</tbody>
</table>

### Changelog for worker node fix pack 1.12.3_1533, released 7 January 2019
{: #1123_1533}

The following table shows the changes that are included in the worker node fix pack 1.12.3_1533.
{: shortdesc}

<table summary="Changes that were made since version 1.12.3_1532">
<caption>Changes since version 1.12.3_1532</caption>
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
<td>Kernel</td>
<td>4.4.0-139</td>
<td>4.4.0-141</td>
<td>Updated worker node images with kernel update for [CVE-2017-5753, CVE-2018-18690 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-141.167/changelog).</td>
</tr>
</tbody>
</table>

### Changelog for worker node fix pack 1.12.3_1532, released 17 December 2018
{: #1123_1532}

The following table shows the changes that are included in the worker node fix pack 1.12.2_1532.
{: shortdesc}

<table summary="Changes that were made since version 1.12.3_1531">
<caption>Changes since version 1.12.3_1531</caption>
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
<td>Ubuntu packages</td>
<td>N/A</td>
<td>N/A</td>
<td>Updates to installed Ubuntu packages.</td>
</tr>
</tbody>
</table>


### Changelog for 1.12.3_1531, released 5 December 2018
{: #1123_1531}

The following table shows the changes that are included in the patch 1.12.3_1531.
{: shortdesc}

<table summary="Changes that were made since version 1.12.2_1530">
<caption>Changes since version 1.12.2_1530</caption>
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
<td>{{site.data.keyword.cloud_notm}} Provider</td>
<td>v1.12.2-68</td>
<td>v1.12.3-91</td>
<td>Updated to support the Kubernetes 1.12.3 release.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.12.2</td>
<td>v1.12.3</td>
<td>See the [Kubernetes release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/kubernetes/kubernetes/releases/tag/v1.12.3). Update resolves [CVE-2018-1002105 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/kubernetes/kubernetes/issues/71411).</td>
</tr>
</tbody>
</table>

### Changelog for worker node fix pack 1.12.2_1530, released 4 December 2018
{: #1122_1530}

The following table shows the changes that are included in the worker node fix pack 1.12.2_1530.
{: shortdesc}

<table summary="Changes that were made since version 1.12.2_1529">
<caption>Changes since version 1.12.2_1529</caption>
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
<td>Worker node resource utilization</td>
<td>N/A</td>
<td>N/A</td>
<td>Added dedicated cgroups for the kubelet and containerd to prevent these components from running out of resources. For more information, see [Worker node resource reserves](/docs/containers?topic=containers-planning_worker_nodes#resource_limit_node).</td>
</tr>
</tbody>
</table>



### Changelog for 1.12.2_1529, released 27 November 2018
{: #1122_1529}

The following table shows the changes that are included in patch 1.12.2_1529.
{: shortdesc}

<table summary="Changes that were made since version 1.12.2_1528">
<caption>Changes since version 1.12.2_1528</caption>
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
<td>Calico</td>
<td>v3.2.1</td>
<td>v3.3.1</td>
<td>See the [Calico release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://docs.projectcalico.org/v3.3/releases/#v331). Update resolves [Tigera Technical Advisory TTA-2018-001 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.projectcalico.org/security-bulletins/).</td>
</tr>
<tr>
<td>Cluster DNS configuration</td>
<td>N/A</td>
<td>N/A</td>
<td>Fixed a bug that could result in both Kubernetes DNS and CoreDNS pods to run after cluster creation or update operations.</td>
</tr>
<tr>
<td>containerd</td>
<td>v1.2.0</td>
<td>v1.1.5</td>
<td>See the [containerd release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/containerd/containerd/releases/tag/v1.1.5). Updated containerd to fix a deadlock that can [stop pods from terminating ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/containerd/containerd/issues/2744).</td>
</tr>
<tr>
<td>OpenVPN client and server</td>
<td>2.4.4-r1-6</td>
<td>2.4.6-r3-IKS-8</td>
<td>Updated image for [CVE-2018-0732 ![External link icon](../icons/launch-glyph.svg "External link icon")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0732) and [CVE-2018-0737 ![External link icon](../icons/launch-glyph.svg "External link icon")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0737).</td>
</tr>
</tbody>
</table>

### Changelog for worker node fix pack 1.12.2_1528, released 19 November 2018
{: #1122_1528}

The following table shows the changes that are included in the worker node fix pack 1.12.2_1528.
{: shortdesc}

<table summary="Changes that were made since version 1.12.2_1527">
<caption>Changes since version 1.12.2_1527</caption>
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
<td>Kernel</td>
<td>4.4.0-137</td>
<td>4.4.0-139</td>
<td>Updated worker node images with kernel update for [CVE-2018-7755 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-139.165/changelog).</td>
</tr>
</tbody>
</table>


### Changelog for 1.12.2_1527, released 7 November 2018
{: #1122_1527}

The following table shows the changes that are included in patch 1.12.2_1527.
{: shortdesc}

<table summary="Changes that were made since version 1.11.3_1533">
<caption>Changes since version 1.11.3_1533</caption>
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
<td>Calico configuration</td>
<td>N/A</td>
<td>N/A</td>
<td>Calico `calico-*` pods in the `kube-system` namespace now set CPU and memory resource requests for all containers.</td>
</tr>
<tr>
<td>Cluster DNS provider</td>
<td>N/A</td>
<td>N/A</td>
<td>Kubernetes DNS (KubeDNS) remains the default cluster DNS provider. However, you now have the option to [change the cluster DNS provide to CoreDNS](/docs/containers?topic=containers-cluster_dns#dns_set).</td>
</tr>
<tr>
<td>Cluster metrics provider</td>
<td>N/A</td>
<td>N/A</td>
<td>Kubernetes Metrics Server replaces Kubernetes Heapster (deprecated since Kubernetes version 1.8) as the cluster metrics provider. For action items, see [the `metrics-server` preparation action](/docs/containers?topic=containers-cs_versions#metrics-server).</td>
</tr>
<tr>
<td>containerd</td>
<td>1.1.4</td>
<td>1.2.0</td>
<td>See the [containerd release notes![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/containerd/containerd/releases/tag/v1.2.0).</td>
</tr>
<tr>
<td>CoreDNS</td>
<td>N/A</td>
<td>1.2.2</td>
<td>See the [CoreDNS release notes![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/coredns/coredns/releases/tag/v1.2.2).</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.11.3</td>
<td>v1.12.2</td>
<td>See the [Kubernetes release notes![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/kubernetes/kubernetes/releases/tag/v1.12.2).</td>
</tr>
<tr>
<td>Kubernetes configuration</td>
<td>N/A</td>
<td>N/A</td>
<td>Added three new IBM pod security policies and their associated cluster roles. For more information, see [Understanding default resources for IBM cluster management](/docs/containers?topic=containers-psp#ibm_psp).</td>
</tr>
<tr>
<td>Kubernetes Dashboard</td>
<td>v1.8.3</td>
<td>v1.10.0</td>
<td>See the [Kubernetes Dashboard release notes![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/kubernetes/dashboard/releases/tag/v1.10.0).<br><br>
If you access the dashboard via `kubectl proxy`, the **SKIP** button on the login page is removed. Instead, [use a **Token** to log in](/docs/containers?topic=containers-app#cli_dashboard). Additionally, you can now scale up the number of Kubernetes Dashboard pods by running `kubectl -n kube-system scale deploy kubernetes-dashboard --replicas=3`.</td>
</tr>
<tr>
<td>Kubernetes DNS</td>
<td>1.14.10</td>
<td>1.14.13</td>
<td>See the [Kubernetes DNS release notes![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/kubernetes/dns/releases/tag/1.14.13).</td>
</tr>
<tr>
<td>Kubernetes Metrics Server</td>
<td>N/A</td>
<td>v0.3.1</td>
<td>See the [Kubernetes Metrics Server release notes![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/kubernetes-incubator/metrics-server/releases/tag/v0.3.1).</td>
</tr>
<tr>
<td>{{site.data.keyword.cloud_notm}} Provider</td>
<td>v1.11.3-118</td>
<td>v1.12.2-68</td>
<td>Updated to support the Kubernetes 1.12 release. Additional changes include the following:
<ul><li>Load balancer pods (`ibm-cloud-provider-ip-*` in `ibm-system` namespace) now set CPU and memory resource requests.</li>
<li>The `service.kubernetes.io/ibm-load-balancer-cloud-provider-vlan` annotation is added to specify the VLAN that the load balancer service deploys to. To see available VLANs in your cluster, run `ibmcloud ks vlans --zone <zone>`.</li>
<li>A new [load balancer 2.0](/docs/containers?topic=containers-loadbalancer-about#planning_ipvs) is available as a beta.</li></ul></td>
</tr>
<tr>
<td>OpenVPN client configuration</td>
<td>N/A</td>
<td>N/A</td>
<td>OpenVPN client `vpn-* pod` in the `kube-system` namespace now sets CPU and memory resource requests.</td>
</tr>
</tbody>
</table>


## Archive
{: #changelog_archive}

Unsupported Kubernetes versions:
*  [Version 1.11](#111_changelog)
*  [Version 1.10](#110_changelog)
*  [Version 1.9](#19_changelog)
*  [Version 1.8](#18_changelog)
*  [Version 1.7](#17_changelog)

### Version 1.11 changelog (unsupported as of 20 July 2019)
{: #111_changelog}

Review the version 1.11 changelog.
{: shortdesc}

*   [Changelog for worker node fix pack 1.11.10_1564, released 8 July 2019](#11110_1564)
*   [Changelog for worker node fix pack 1.11.10_1563, released 24 June 2019](#11110_1563)
*   [Changelog for worker node fix pack 1.11.10_1562, released 17 June 2019](#11110_1562)
*   [Changelog for 1.11.10_1561, released 4 June 2019](#11110_1561)
*   [Changelog for worker node fix pack 1.11.10_1559, released 20 May 2019](#11110_1559)
*   [Changelog for 1.11.10_1558, released 13 May 2019](#11110_1558)
*   [Changelog for worker node fix pack 1.11.9_1556, released 29 April 2019](#1119_1556)
*   [Changelog for worker node fix pack 1.11.9_1555, released 15 April 2019](#1119_1555)
*   [Changelog for 1.11.9_1554, released 8 April 2019](#1119_1554)
*   [Changelog for worker node fix pack 1.11.8_1553, released 1 April 2019](#1118_1553)
*   [Changelog for master fix pack 1.11.8_1552, released 26 March 2019](#1118_1552)
*   [Changelog for 1.11.8_1550, released 20 March 2019](#1118_1550)
*   [Changelog for 1.11.8_1547, released 4 March 2019](#1118_1547)
*   [Changelog for worker node fix pack 1.11.7_1546, released 27 February 2019](#1117_1546)
*   [Changelog for worker node fix pack 1.11.7_1544, released 15 February 2019](#1117_1544)
*   [Changelog for 1.11.7_1543, released 5 February 2019](#1117_1543)
*   [Changelog for worker node fix pack 1.11.6_1541, released 28 January 2019](#1116_1541)
*   [Changelog for 1.11.6_1540, released 21 January 2019](#1116_1540)
*   [Changelog for worker node fix pack 1.11.5_1539, released 7 January 2019](#1115_1539)
*   [Changelog for worker node fix pack 1.11.5_1538, released 17 December 2018](#1115_1538)
*   [Changelog for 1.11.5_1537, released 5 December 2018](#1115_1537)
*   [Changelog for worker node fix pack 1.11.4_1536, released 4 December 2018](#1114_1536)
*   [Changelog for 1.11.4_1535, released 27 November 2018](#1114_1535)
*   [Changelog for worker node fix pack 1.11.3_1534, released 19 November 2018](#1113_1534)
*   [Changelog for 1.11.3_1533, released 7 November 2018](#1113_1533)
*   [Changelog for master fix pack 1.11.3_1531, released 1 November 2018](#1113_1531_ha-master)
*   [Changelog for worker node fix pack 1.11.3_1531, released 26 October 2018](#1113_1531)
*   [Changelog for master fix pack 1.11.3_1527, released 15 October 2018](#1113_1527)
*   [Changelog for worker node fix pack 1.11.3_1525, released 10 October 2018](#1113_1525)
*   [Changelog for 1.11.3_1524, released 2 October 2018](#1113_1524)
*   [Changelog for 1.11.3_1521, released 20 September 2018](#1113_1521)
*   [Changelog for 1.11.2_1516, released 4 September 2018](#1112_1516)
*   [Changelog for worker node fix pack 1.11.2_1514, released 23 August 2018](#1112_1514)
*   [Changelog for 1.11.2_1513, released 14 August 2018](#1112_1513)

#### Changelog for worker node fix pack 1.11.10_1564, released 8 July 2019
{: #11110_1564}

The following table shows the changes that are included in the worker node patch 1.11.10_1564.
{: shortdesc}

<table summary="Changes that were made since version 1.11.10_1563">
<caption>Changes since version 1.11.10_1563</caption>
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
<td>Ubuntu 16.04 kernel</td>
<td>4.4.0-151-generic</td>
<td>4.4.0-154-generic</td>
<td>Updated worker node images with kernel and package updates for [CVE-2019-11478 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-11478.html) and [CVE-2019-11479 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-11479.html).</td>
</tr>
<tr>
<td>Ubuntu 18.04 kernel</td>
<td>4.15.0-52-generic</td>
<td>4.15.0-54-generic</td>
<td>Updated worker node images with kernel and package updates for [CVE-2019-11478 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-11478.html) and [CVE-2019-11479 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-11479.html).</td>
</tr>
</tbody>
</table>

#### Changelog for worker node fix pack 1.11.10_1563, released 24 June 2019
{: #11110_1563}

The following table shows the changes that are included in the worker node patch 1.11.10_1563.
{: shortdesc}

<table summary="Changes that were made since version 1.11.10_1562">
<caption>Changes since version 1.11.10_1562</caption>
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
<td>Ubuntu 16.04 kernel</td>
<td>4.4.0-150-generic</td>
<td>4.4.0-151-generic</td>
<td>Updated worker node images with kernel and package updates for [CVE-2019-11477 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-11477.html) and [CVE-2019-11478 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-11478.html).</td>
</tr>
<tr>
<td>Ubuntu 18.04 kernel</td>
<td>4.15.0-51-generic</td>
<td>4.15.0-52-generic</td>
<td>Updated worker node images with kernel and package updates for [CVE-2019-11477 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-11477.html) and [CVE-2019-11478 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-11478.html).</td>
</tr>
</tbody>
</table>

#### Changelog for worker node fix pack 1.11.10_1562, released 17 June 2019
{: #11110_1562}

The following table shows the changes that are included in the worker node patch 1.11.10_1562.
{: shortdesc}

<table summary="Changes that were made since version 1.11.10_1561">
<caption>Changes since version 1.11.10_1561</caption>
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
<td>Ubuntu 16.04 kernel</td>
<td>4.4.0-148-generic</td>
<td>4.4.0-150-generic</td>
<td>Updated worker node images with kernel and package updates for [CVE-2019-10906 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-10906.html?_ga=2.184456110.929090212.1560547312-1880639276.1557078470).</td>
</tr>
<tr>
<td>Ubuntu 18.04 kernel</td>
<td>4.15.0-50-generic</td>
<td>4.15.0-51-generic</td>
<td>Updated worker node images with kernel and package updates for [CVE-2019-10906 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-10906.html?_ga=2.184456110.929090212.1560547312-1880639276.1557078470).</td>
</tr>
</tbody>
</table>

#### Changelog for 1.11.10_1561, released 4 June 2019
{: #11110_1561}

The following table shows the changes that are included in the patch 1.11.10_1561.
{: shortdesc}

<table summary="Changes that were made since version 1.11.10_1559">
<caption>Changes since version 1.11.10_1559</caption>
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
<td>Cluster master HA configuration</td>
<td>N/A</td>
<td>N/A</td>
<td>Updated configuration to minimize intermittent master network connectivity failures during a master update.</td>
</tr>
<tr>
<td>etcd</td>
<td>v3.3.11</td>
<td>v3.3.13</td>
<td>See the [etcd release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/coreos/etcd/releases/v3.3.13).</td>
</tr>
<tr>
<td>GPU device plug-in and installer</td>
<td>55c1f66</td>
<td>32257d3</td>
<td>Updated image for [CVE-2018-10844 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10844), [CVE-2018-10845 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10845), [CVE-2018-10846 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10846), [CVE-2019-3829 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3829), [CVE-2019-3836 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3836), [CVE-2019-9893 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9893), [CVE-2019-5435 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5435), and [CVE-2019-5436 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5436).</td>
</tr>
<tr>
<td>Trusted compute agent</td>
<td>13c7ef0</td>
<td>e8c6d72</td>
<td>Updated image for [CVE-2018-10844 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10844), [CVE-2018-10845 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10845), [CVE-2018-10846 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10846), [CVE-2019-3829 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3829), [CVE-2019-3836 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3836), [CVE-2019-9893 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9893), [CVE-2019-5435 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5435), and [CVE-2019-5436 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5436).</td>
</tr>
</tbody>
</table>

#### Changelog for worker node fix pack 1.11.10_1559, released 20 May 2019
{: #11110_1559}

The following table shows the changes that are included in the patch pack 1.11.10_1559.
{: shortdesc}

<table summary="Changes that were made since version 1.11.10_1558">
<caption>Changes since version 1.11.10_1558</caption>
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
<td>Ubuntu 16.04 kernel</td>
<td>4.4.0-145-generic</td>
<td>4.4.0-148-generic</td>
<td>Updated worker node images with kernel update for [CVE-2018-12126 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12126.html), [CVE-2018-12127 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12127.html), and [CVE-2018-12130 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12130.html).</td>
</tr>
<tr>
<td>Ubuntu 18.04 kernel</td>
<td>4.15.0-47-generic</td>
<td>4.15.0-50-generic</td>
<td>Updated worker node images with kernel update for [CVE-2018-12126 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12126.html), [CVE-2018-12127 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12127.html), and [CVE-2018-12130 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://people.canonical.com/~ubuntu-security/cve/2018/CVE-2018-12130.html).</td>
</tr>
</tbody>
</table>

#### Changelog for 1.11.10_1558, released 13 May 2019
{: #11110_1558}

The following table shows the changes that are included in the patch 1.11.10_1558.
{: shortdesc}

<table summary="Changes that were made since version 1.11.9_1556">
<caption>Changes since version 1.11.9_1556</caption>
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
<td>Cluster master HA proxy</td>
<td>1.9.6-alpine</td>
<td>1.9.7-alpine</td>
<td>See the [HAProxy release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.haproxy.org/download/1.9/src/CHANGELOG). Update resolves [CVE-2019-6706 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6706).</td>
</tr>
<tr>
<td>GPU device plug-in and installer</td>
<td>9ff3fda</td>
<td>55c1f66</td>
<td>Updated image for [CVE-2019-1543 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1543).</td>
</tr>
<tr>
<td>{{site.data.keyword.cloud_notm}} Provider</td>
<td>v1.11.9-241</td>
<td>v1.11.10-270</td>
<td>Updated to support the Kubernetes 1.11.10 release.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.11.9</td>
<td>v1.11.10</td>
<td>See the [Kubernetes release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/kubernetes/kubernetes/releases/tag/v1.11.10).</td>
</tr>
<tr>
<td>Kubernetes configuration</td>
<td>N/A</td>
<td>N/A</td>
<td>The Kubernetes API server audit policy configuration is updated to not log the `/openapi/v2*` read-only URL. In addition, the Kubernetes controller manager configuration increased the validity duration of signed `kubelet` certificates from 1 to 3 years.</td>
</tr>
<tr>
<td>OpenVPN client configuration</td>
<td>N/A</td>
<td>N/A</td>
<td>The OpenVPN client `vpn-*` pod in the `kube-system` namespace now sets `dnsPolicy` to `Default` to prevent the pod from failing when cluster DNS is down.</td>
</tr>
<tr>
<td>Trusted compute agent</td>
<td>e132aa4</td>
<td>13c7ef0</td>
<td>Updated image for [CVE-2016-7076 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2016-7076), [CVE-2017-1000368 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1000368), and [CVE-2019-11068 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-11068).</td>
</tr>
</tbody>
</table>

#### Changelog for worker node fix pack 1.11.9_1556, released 29 April 2019
{: #1119_1556}

The following table shows the changes that are included in the worker node fix pack 1.11.9_1556.
{: shortdesc}

<table summary="Changes that were made since version 1.11.9_1555">
<caption>Changes since version 1.11.9_1555</caption>
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
<td>Ubuntu packages</td>
<td>N/A</td>
<td>N/A</td>
<td>Updates to installed Ubuntu packages.</td>
</tr>
<tr>
<td>containerd</td>
<td>1.1.6</td>
<td>1.1.7</td>
<td>See the [containerd release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/containerd/containerd/releases/tag/v1.1.7).</td>
</tr>
</tbody>
</table>


#### Changelog for worker node fix pack 1.11.9_1555, released 15 April 2019
{: #1119_1555}

The following table shows the changes that are included in the worker node fix pack 1.11.9_1555.
{: shortdesc}

<table summary="Changes that were made since version 1.11.9_1554">
<caption>Changes since 1.11.9_1554</caption>
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
<td>Ubuntu packages</td>
<td>N/A</td>
<td>N/A</td>
<td>Updates to installed Ubuntu packages including `systemd` for [CVE-2019-3842 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-3842.html).</td>
</tr>
</tbody>
</table>

#### Changelog for 1.11.9_1554, released 8 April 2019
{: #1119_1554}

The following table shows the changes that are included in the patch 1.11.9_1554.
{: shortdesc}

<table summary="Changes that were made since version 1.11.8_1553">
<caption>Changes since version 1.11.8_1553</caption>
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
<td>Calico</td>
<td>v3.3.1</td>
<td>v3.3.6</td>
<td>See the [Calico release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://docs.projectcalico.org/v3.3/releases/#v336). Update resolves [CVE-2019-9946 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9946).</td>
</tr>
<tr>
<td>Cluster master HA proxy</td>
<td>1.8.12-alpine</td>
<td>1.9.6-alpine</td>
<td>See the [HAProxy release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.haproxy.org/download/1.9/src/CHANGELOG). Update resolves [CVE-2018-0732 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0732), [CVE-2018-0734 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734), [CVE-2018-0737 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0737), [CVE-2018-5407 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407), [CVE-2019-1543 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1543), and [CVE-2019-1559 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1559).</td>
</tr>
<tr>
<td>{{site.data.keyword.cloud_notm}} Provider</td>
<td>v1.11.8-219</td>
<td>v1.11.9-241</td>
<td>Updated to support the Kubernetes 1.11.9 and Calico 3.3.6 releases.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.11.8</td>
<td>v1.11.9</td>
<td>See the [Kubernetes release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/kubernetes/kubernetes/releases/tag/v1.11.9).</td>
</tr>
<tr>
<td>Kubernetes DNS</td>
<td>1.14.10</td>
<td>1.14.13</td>
<td>See the [Kubernetes DNS release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/kubernetes/dns/releases/tag/1.14.13).</td>
</tr>
<tr>
<td>Trusted compute agent</td>
<td>a02f765</td>
<td>e132aa4</td>
<td>Updated image for [CVE-2017-12447 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-12447).</td>
</tr>
<tr>
<td>Ubuntu 16.04 kernel</td>
<td>4.4.0-143-generic</td>
<td>4.4.0-145-generic</td>
<td>Updated worker node images with kernel update for [CVE-2019-9213 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9213.html).</td>
</tr>
<tr>
<td>Ubuntu 18.04 kernel</td>
<td>4.15.0-46-generic</td>
<td>4.15.0-47-generic</td>
<td>Updated worker node images with kernel update for [CVE-2019-9213 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9213.html).</td>
</tr>
</tbody>
</table>

#### Changelog for worker node fix pack 1.11.8_1553, released 1 April 2019
{: #1118_1553}

The following table shows the changes that are included in the worker node fix 1.11.8_1553.
{: shortdesc}

<table summary="Changes that were made since version 1.11.8_1552">
<caption>Changes since version 1.11.8_1552</caption>
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
<td>Worker node resource utilization</td>
<td>N/A</td>
<td>N/A</td>
<td>Increased memory reservations for the kubelet and containerd to prevent these components from running out of resources. For more information, see [Worker node resource reserves](/docs/containers?topic=containers-planning_worker_nodes#resource_limit_node).</td>
</tr>
</tbody>
</table>

#### Changelog for master fix pack 1.11.8_1552, released 26 March 2019
{: #1118_1552}

The following table shows the changes that are included in the master fix pack 1.11.8_1552.
{: shortdesc}

<table summary="Changes that were made since version 1.11.8_1550">
<caption>Changes since version 1.11.8_1550</caption>
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
<td>{{site.data.keyword.cloud_notm}} File Storage plug-in</td>
<td>345</td>
<td>346</td>
<td>Updated image for [CVE-2019-9741 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741).</td>
</tr>
<tr>
<td>Key Management Service provider</td>
<td>166</td>
<td>167</td>
<td>Fixes intermittent `context deadline exceeded` and `timeout` errors for managing Kubernetes secrets. In addition, fixes updates to the key management service that might leave existing Kubernetes secrets unencrypted. Update includes fix for [CVE-2019-9741 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741).</td>
</tr>
<tr>
<td>Load balancer and load balancer monitor for {{site.data.keyword.cloud_notm}} Provider</td>
<td>143</td>
<td>146</td>
<td>Updated image for [CVE-2019-9741 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741).</td>
</tr>
</tbody>
</table>

#### Changelog for 1.11.8_1550, released 20 March 2019
{: #1118_1550}

The following table shows the changes that are included in the patch 1.11.8_1550.
{: shortdesc}

<table summary="Changes that were made since version 1.11.8_1547">
<caption>Changes since version 1.11.8_1547</caption>
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
<td>Cluster master HA proxy configuration</td>
<td>N/A</td>
<td>N/A</td>
<td>Updated configuration to better handle intermittent connection failures to the cluster master.</td>
</tr>
<tr>
<td>GPU device plug-in and installer</td>
<td>e32d51c</td>
<td>9ff3fda</td>
<td>Updated the GPU drivers to [418.43 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.nvidia.com/object/unix.html). Update includes fix for [CVE-2019-9741 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9741.html).</td>
</tr>
<tr>
<td>{{site.data.keyword.cloud_notm}} File Storage plug-in</td>
<td>344</td>
<td>345</td>
<td>Added support for [private service endpoints](/docs/containers?topic=containers-cs_network_cluster#set-up-private-se).</td>
</tr>
<tr>
<td>Kernel</td>
<td>4.4.0-141</td>
<td>4.4.0-143</td>
<td>Updated worker node images with kernel update for [CVE-2019-6133 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-6133.html).</td>
</tr>
<tr>
<td>Key Management Service provider</td>
<td>136</td>
<td>166</td>
<td>Updated image for [CVE-2018-16890 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-16890), [CVE-2019-3822 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3822), and [CVE-2019-3823 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3823).</td>
</tr>
<tr>
<td>Trusted compute agent</td>
<td>5f3d092</td>
<td>a02f765</td>
<td>Updated image for [CVE-2018-10779 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10779), [CVE-2018-12900 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-12900), [CVE-2018-17000 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-17000), [CVE-2018-19210 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-19210), [CVE-2019-6128 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6128), and [CVE-2019-7663 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-7663).</td>
</tr>
</tbody>
</table>

#### Changelog for 1.11.8_1547, released 4 March 2019
{: #1118_1547}

The following table shows the changes that are included in the patch 1.11.8_1547.
{: shortdesc}

<table summary="Changes that were made since version 1.11.7_1546">
<caption>Changes since version 1.11.7_1546</caption>
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
<td>GPU device plug-in and installer</td>
<td>eb3a259</td>
<td>e32d51c</td>
<td>Updated images for [CVE-2019-6454 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6454).</td>
</tr>
<tr>
<td>{{site.data.keyword.cloud_notm}} Provider</td>
<td>v1.11.7-198</td>
<td>v1.11.8-219</td>
<td>Updated to support the Kubernetes 1.11.8 release. Fixed periodic connectivity problems for load balancers that set `externalTrafficPolicy` to `local`. Updated load balancer events to use the latest {{site.data.keyword.cloud_notm}} documentation links.</td>
</tr>
<tr>
<td>{{site.data.keyword.cloud_notm}} File Storage plug-in</td>
<td>342</td>
<td>344</td>
<td>Changed the base operating system for the image from Fedora to Alpine. Updated image for [CVE-2019-6486 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486).</td>
</tr>
<tr>
<td>Key Management Service provider</td>
<td>122</td>
<td>136</td>
<td>Increased client timeout to {{site.data.keyword.keymanagementservicefull_notm}}. Updated image for [CVE-2019-6486 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486).</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.11.7</td>
<td>v1.11.8</td>
<td>See the [Kubernetes release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/kubernetes/kubernetes/releases/tag/v1.11.8). Update resolves [CVE-2019-6486 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486) and [CVE-2019-1002100 ![External link icon](../icons/launch-glyph.svg "External link icon")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1002100).</td>
</tr>
<tr>
<td>Kubernetes configuration</td>
<td>N/A</td>
<td>N/A</td>
<td>Added `ExperimentalCriticalPodAnnotation=true` to the `--feature-gates` option. This setting helps migrate pods from the deprecated `scheduler.alpha.kubernetes.io/critical-pod` annotation to [Kubernetes pod priority support](/docs/containers?topic=containers-pod_priority#pod_priority).</td>
</tr>
<tr>
<td>Kubernetes DNS</td>
<td>N/A</td>
<td>N/A</td>
<td>Increased Kubernetes DNS pod memory limit from `170Mi` to `400Mi` in order to handle more cluster services.</td>
</tr>
<tr>
<td>Load balancer and load balancer monitor for {{site.data.keyword.cloud_notm}} Provider</td>
<td>132</td>
<td>143</td>
<td>Updated image for [CVE-2019-6486 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486).</td>
</tr>
<tr>
<td>OpenVPN client and server</td>
<td>2.4.6-r3-IKS-13</td>
<td>2.4.6-r3-IKS-25</td>
<td>Updated image for [CVE-2019-1559 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1559).</td>
</tr>
<tr>
<td>Trusted compute agent</td>
<td>1ea5ad3</td>
<td>5f3d092</td>
<td>Updated image for [CVE-2019-6454 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6454).</td>
</tr>
</tbody>
</table>

#### Changelog for worker node fix pack 1.11.7_1546, released 27 February 2019
{: #1117_1546}

The following table shows the changes that are included in the worker node fix pack 1.11.7_1546.
{: shortdesc}

<table summary="Changes that were made since version 1.11.7_1546">
<caption>Changes since version 1.11.7_1546</caption>
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
<td>Kernel</td>
<td>4.4.0-141</td>
<td>4.4.0-142</td>
<td>Updated worker node images with kernel update for [CVE-2018-19407 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-142.168/changelog).</td>
</tr>
</tbody>
</table>

#### Changelog for worker node fix pack 1.11.7_1544, released 15 February 2019
{: #1117_1544}

The following table shows the changes that are included in the worker node fix pack 1.11.7_1544.
{: shortdesc}

<table summary="Changes that were made since version 1.11.7_1543">
<caption>Changes since version 1.11.7_1543</caption>
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
<td>Cluster master HA proxy configuration</td>
<td>N/A</td>
<td>N/A</td>
<td>Changed the pod configuration `spec.priorityClassName` value to `system-node-critical` and set the `spec.priority` value to `2000001000`.</td>
</tr>
<tr>
<td>containerd</td>
<td>1.1.5</td>
<td>1.1.6</td>
<td>See the [containerd release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/containerd/containerd/releases/tag/v1.1.6). Update resolves [CVE-2019-5736 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5736).</td>
</tr>
<tr>
<td>Kubernetes `kubelet` configuration</td>
<td>N/A</td>
<td>N/A</td>
<td>Enabled the `ExperimentalCriticalPodAnnotation` feature gate to prevent critical static pod eviction.</td>
</tr>
</tbody>
</table>

#### Changelog for 1.11.7_1543, released 5 February 2019
{: #1117_1543}

The following table shows the changes that are included in the patch 1.11.7_1543.
{: shortdesc}

<table summary="Changes that were made since version 1.11.6_1541">
<caption>Changes since version 1.11.6_1541</caption>
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
<td>etcd</td>
<td>v3.3.1</td>
<td>v3.3.11</td>
<td>See the [etcd release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/coreos/etcd/releases/v3.3.11). Additionally, the supported cipher suites to etcd are now restricted to a subset with high strength encryption (128 bits or more).</td>
</tr>
<tr>
<td>GPU device plug-in and installer</td>
<td>13fdc0d</td>
<td>eb3a259</td>
<td>Updated images for [CVE-2019-3462 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3462) and [CVE-2019-6486 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486).</td>
</tr>
<tr>
<td>{{site.data.keyword.cloud_notm}} Provider</td>
<td>v1.11.6-180</td>
<td>v1.11.7-198</td>
<td>Updated to support the Kubernetes 1.11.7 release. Additionally, `calicoctl` version is updated to 3.3.1. Fixed unnecessary configuration updates to version 2.0 network load balancers on worker node status changes.</td>
</tr>
<tr>
<td>{{site.data.keyword.cloud_notm}} File Storage plug-in</td>
<td>338</td>
<td>342</td>
<td>The file storage plug-in is updated as follows:
<ul><li>Supports dynamic provisioning with [volume topology-aware scheduling](/docs/containers?topic=containers-file_storage#file-topology).</li>
<li>Ignores persistent volume claim (PVC) delete errors if the storage is already deleted.</li>
<li>Adds a failure message annotation to failed PVCs.</li>
<li>Optimizes the storage provisioner controller's leader election and resync period settings, and increases the provisioning timeout from 30 minutes to 1 hour.</li>
<li>Checks user permissions before starting the provisioning.</li></ul></td>
</tr>
<tr>
<td>Key Management Service provider</td>
<td>111</td>
<td>122</td>
<td>Added retry logic to avoid temporary failures when Kubernetes secrets are managed by {{site.data.keyword.keymanagementservicefull_notm}}.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.11.6</td>
<td>v1.11.7</td>
<td>See the [Kubernetes release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/kubernetes/kubernetes/releases/tag/v1.11.7).</td>
</tr>
<tr>
<td>Kubernetes configuration</td>
<td>N/A</td>
<td>N/A</td>
<td>The Kubernetes API server audit policy configuration is updated to include logging metadata for `cluster-admin` requests and logging the request body of workload `create`, `update`, and `patch` requests.</td>
</tr>
<tr>
<td>OpenVPN client</td>
<td>2.4.6-r3-IKS-8</td>
<td>2.4.6-r3-IKS-13</td>
<td>Updated image for [CVE-2018-0734 ![External link icon](../icons/launch-glyph.svg "External link icon")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734) and [CVE-2018-5407 ![External link icon](../icons/launch-glyph.svg "External link icon")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407). Additionally, the pod configuration is now obtained from a secret instead of from a configmap.</td>
</tr>
<tr>
<td>OpenVPN server</td>
<td>2.4.6-r3-IKS-8</td>
<td>2.4.6-r3-IKS-13</td>
<td>Updated image for [CVE-2018-0734 ![External link icon](../icons/launch-glyph.svg "External link icon")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734) and [CVE-2018-5407 ![External link icon](../icons/launch-glyph.svg "External link icon")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407).</td>
</tr>
<tr>
<td>systemd</td>
<td>230</td>
<td>229</td>
<td>Security patch for [CVE-2018-16864 ![External link icon](../icons/launch-glyph.svg "External link icon")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-16864).</td>
</tr>
</tbody>
</table>

#### Changelog for worker node fix pack 1.11.6_1541, released 28 January 2019
{: #1116_1541}

The following table shows the changes that are included in the worker node fix pack 1.11.6_1541.
{: shortdesc}

<table summary="Changes that were made since version 1.11.6_1540">
<caption>Changes since version 1.11.6_1540</caption>
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
<td>Ubuntu packages</td>
<td>N/A</td>
<td>N/A</td>
<td>Updates to installed Ubuntu packages including `apt` for [CVE-2019-3462 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3462) / [USN-3863-1 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://usn.ubuntu.com/3863-1).</td>
</tr>
</tbody>
</table>

#### Changelog for 1.11.6_1540, released 21 January 2019
{: #1116_1540}

The following table shows the changes that are included in the patch 1.11.6_1540.
{: shortdesc}

<table summary="Changes that were made since version 1.11.5_1539">
<caption>Changes since version 1.11.5_1539</caption>
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
<td>{{site.data.keyword.cloud_notm}} Provider</td>
<td>v1.11.5-152</td>
<td>v1.11.6-180</td>
<td>Updated to support the Kubernetes 1.11.6 release.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.11.5</td>
<td>v1.11.6</td>
<td>See the [Kubernetes release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/kubernetes/kubernetes/releases/tag/v1.11.6).</td>
</tr>
<tr>
<td>Kubernetes add-on resizer</td>
<td>1.8.1</td>
<td>1.8.4</td>
<td>See the [Kubernetes add-on resizer release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/kubernetes/autoscaler/releases/tag/addon-resizer-1.8.4).</td>
</tr>
<tr>
<td>Kubernetes dashboard</td>
<td>v1.8.3</td>
<td>v1.10.1</td>
<td>See the [Kubernetes dashboard release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/kubernetes/dashboard/releases/tag/v1.10.1). Update resolves [CVE-2018-18264 ![External link icon](../icons/launch-glyph.svg "External link icon")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-18264).<br><br>If you access the dashboard via `kubectl proxy`, the **SKIP** button on the login page is removed. Instead, [use a **Token** to log in](/docs/containers?topic=containers-app#cli_dashboard).</td>
</tr>
<tr>
<td>GPU installer</td>
<td>390.12</td>
<td>410.79</td>
<td>Updated the installed GPU drivers to 410.79.</td>
</tr>
</tbody>
</table>

#### Changelog for worker node fix pack 1.11.5_1539, released 7 January 2019
{: #1115_1539}

The following table shows the changes that are included in the worker node fix pack 1.11.5_1539.
{: shortdesc}

<table summary="Changes that were made since version 1.11.5_1538">
<caption>Changes since version 1.11.5_1538</caption>
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
<td>Kernel</td>
<td>4.4.0-139</td>
<td>4.4.0-141</td>
<td>Updated worker node images with kernel update for [CVE-2017-5753, CVE-2018-18690 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-141.167/changelog).</td>
</tr>
</tbody>
</table>

#### Changelog for worker node fix pack 1.11.5_1538, released 17 December 2018
{: #1115_1538}

The following table shows the changes that are included in the worker node fix pack 1.11.5_1538.
{: shortdesc}

<table summary="Changes that were made since version 1.11.5_1537">
<caption>Changes since version 1.11.5_1537</caption>
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
<td>Ubuntu packages</td>
<td>N/A</td>
<td>N/A</td>
<td>Updates to installed Ubuntu packages.</td>
</tr>
</tbody>
</table>

#### Changelog for 1.11.5_1537, released 5 December 2018
{: #1115_1537}

The following table shows the changes that are included in the patch 1.11.5_1537.
{: shortdesc}

<table summary="Changes that were made since version 1.11.4_1536">
<caption>Changes since version 1.11.4_1536</caption>
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
<td>{{site.data.keyword.cloud_notm}} Provider</td>
<td>v1.11.4-142</td>
<td>v1.11.5-152</td>
<td>Updated to support the Kubernetes 1.11.5 release.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.11.4</td>
<td>v1.11.5</td>
<td>See the [Kubernetes release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/kubernetes/kubernetes/releases/tag/v1.11.5). Update resolves [CVE-2018-1002105 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/kubernetes/kubernetes/issues/71411).</td>
</tr>
</tbody>
</table>

#### Changelog for worker node fix pack 1.11.4_1536, released 4 December 2018
{: #1114_1536}

The following table shows the changes that are included in the worker node fix pack 1.11.4_1536.
{: shortdesc}

<table summary="Changes that were made since version 1.11.4_1535">
<caption>Changes since version 1.11.4_1535</caption>
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
<td>Worker node resource utilization</td>
<td>N/A</td>
<td>N/A</td>
<td>Added dedicated cgroups for the kubelet and containerd to prevent these components from running out of resources. For more information, see [Worker node resource reserves](/docs/containers?topic=containers-planning_worker_nodes#resource_limit_node).</td>
</tr>
</tbody>
</table>

#### Changelog for 1.11.4_1535, released 27 November 2018
{: #1114_1535}

The following table shows the changes that are included in patch 1.11.4_1535.
{: shortdesc}

<table summary="Changes that were made since version 1.11.3_1534">
<caption>Changes since version 1.11.3_1534</caption>
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
<td>Calico</td>
<td>v3.2.1</td>
<td>v3.3.1</td>
<td>See the [Calico release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://docs.projectcalico.org/v3.3/releases/#v331). Update resolves [Tigera Technical Advisory TTA-2018-001 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.projectcalico.org/security-bulletins/).</td>
</tr>
<tr>
<td>containerd</td>
<td>v1.1.4</td>
<td>v1.1.5</td>
<td>See the [containerd release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/containerd/containerd/releases/tag/v1.1.5). Updated containerd to fix a deadlock that can [stop pods from terminating ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/containerd/containerd/issues/2744).</td>
</tr>
<tr>
<td>{{site.data.keyword.cloud_notm}} Provider</td>
<td>v1.11.3-127</td>
<td>v1.11.4-142</td>
<td>Updated to support the Kubernetes 1.11.4 release.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.11.3</td>
<td>v1.11.4</td>
<td>See the [Kubernetes release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/kubernetes/kubernetes/releases/tag/v1.11.4).</td>
</tr>
<tr>
<td>OpenVPN client and server</td>
<td>2.4.4-r1-6</td>
<td>2.4.6-r3-IKS-8</td>
<td>Updated image for [CVE-2018-0732 ![External link icon](../icons/launch-glyph.svg "External link icon")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0732) and [CVE-2018-0737 ![External link icon](../icons/launch-glyph.svg "External link icon")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0737).</td>
</tr>
</tbody>
</table>

#### Changelog for worker node fix pack 1.11.3_1534, released 19 November 2018
{: #1113_1534}

The following table shows the changes that are included in the worker node fix pack 1.11.3_1534.
{: shortdesc}

<table summary="Changes that were made since version 1.11.3_1533">
<caption>Changes since version 1.11.3_1533</caption>
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
<td>Kernel</td>
<td>4.4.0-137</td>
<td>4.4.0-139</td>
<td>Updated worker node images with kernel update for [CVE-2018-7755 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-139.165/changelog).</td>
</tr>
</tbody>
</table>


#### Changelog for 1.11.3_1533, released 7 November 2018
{: #1113_1533}

The following table shows the changes that are included in patch 1.11.3_1533.
{: shortdesc}

<table summary="Changes that were made since version 1.11.3_1531">
<caption>Changes since version 1.11.3_1531</caption>
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
<td>Cluster master HA update</td>
<td>N/A</td>
<td>N/A</td>
<td>Fixed the update to highly available (HA) masters for clusters that use admission webhooks such as `initializerconfigurations`, `mutatingwebhookconfigurations`, or `validatingwebhookconfigurations`. You might use these webhooks with Helm charts such as for [Container Image Security Enforcement](/docs/services/Registry?topic=registry-security_enforce#security_enforce).</td>
</tr>
<tr>
<td>{{site.data.keyword.cloud_notm}} Provider</td>
<td>v1.11.3-100</td>
<td>v1.11.3-127</td>
<td>Added the `service.kubernetes.io/ibm-load-balancer-cloud-provider-vlan` annotation to specify the VLAN that the load balancer service deploys to. To see available VLANs in your cluster, run `ibmcloud ks vlans --zone <zone>`.</td>
</tr>
<tr>
<td>TPM-enabled kernel</td>
<td>N/A</td>
<td>N/A</td>
<td>Bare metal worker nodes with TPM chips for Trusted Compute use the default Ubuntu kernel until trust is enabled. If you [enable trust](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_feature_enable) on an existing cluster, you need to [reload](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload) any existing bare metal worker nodes with TPM chips. To check if a bare metal worker node has a TPM chip, review the **Trustable** field after running the `ibmcloud ks flavors --zone` [command](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_machine_types).</td>
</tr>
</tbody>
</table>

#### Changelog for master fix pack 1.11.3_1531, released 1 November 2018
{: #1113_1531_ha-master}

The following table shows the changes that are included in the master fix pack 1.11.3_1531.
{: shortdesc}

<table summary="Changes that were made since version 1.11.3_1527">
<caption>Changes since version 1.11.3_1527</caption>
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
<td>Cluster master</td>
<td>N/A</td>
<td>N/A</td>
<td>Updated the cluster master configuration to increase high availability (HA). Clusters now have three Kubernetes master replicas that are set up with a highly available (HA) configuration, with each master deployed on separate physical hosts.</td>
</tr>
<tr>
<td>Cluster master HA proxy</td>
<td>N/A</td>
<td>1.8.12-alpine</td>
<td>Added an `ibm-master-proxy-*` pod for client-side load balancing on all worker nodes, so that each worker node client can route requests to an available HA master replica.</td>
</tr>
<tr>
<td>etcd</td>
<td>v3.2.18</td>
<td>v3.3.1</td>
<td>See the [etcd release notes![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/coreos/etcd/releases/v3.3.1).</td>
</tr>
<tr>
<td>Encrypting data in etcd</td>
<td>N/A</td>
<td>N/A</td>
<td>Previously, etcd data was stored on a master’s NFS file storage instance that is encrypted at rest. Now, etcd data is stored on the master’s local disk and backed up to {{site.data.keyword.cos_full_notm}}. Data is encrypted during transit to {{site.data.keyword.cos_full_notm}} and at rest. However, the etcd data on the master’s local disk is not encrypted. If you want your master’s local etcd data to be encrypted, [enable {{site.data.keyword.keymanagementservicelong_notm}} in your cluster](/docs/containers?topic=containers-encryption#keyprotect).</td>
</tr>
</tbody>
</table>

#### Changelog for worker node fix pack 1.11.3_1531, released 26 October 2018
{: #1113_1531}

The following table shows the changes that are included in the worker node fix pack 1.11.3_1531.
{: shortdesc}

<table summary="Changes that were made since version 1.11.3_1525">
<caption>Changes since version 1.11.3_1525</caption>
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
<td>OS interrupt handling</td>
<td>N/A</td>
<td>N/A</td>
<td>Replaced the interrupt request (IRQ) system daemon with a more performant interrupt handler.</td>
</tr>
</tbody>
</table>

#### Changelog for master fix pack 1.11.3_1527, released 15 October 2018
{: #1113_1527}

The following table shows the changes that are included in the master fix pack 1.11.3_1527.
{: shortdesc}

<table summary="Changes that were made since version 1.11.3_1524">
<caption>Changes since version 1.11.3_1524</caption>
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
<td>Calico configuration</td>
<td>N/A</td>
<td>N/A</td>
<td>Fixed `calico-node` container readiness probe to better handle node failures.</td>
</tr>
<tr>
<td>Cluster update</td>
<td>N/A</td>
<td>N/A</td>
<td>Fixed problem with updating cluster add-ons when the master is updated from an unsupported version.</td>
</tr>
</tbody>
</table>

#### Changelog for worker node fix pack 1.11.3_1525, released 10 October 2018
{: #1113_1525}

The following table shows the changes that are included in the worker node fix pack 1.11.3_1525.
{: shortdesc}

<table summary="Changes that were made since version 1.11.3_1524">
<caption>Changes since version 1.11.3_1524</caption>
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
<td>Kernel</td>
<td>4.4.0-133</td>
<td>4.4.0-137</td>
<td>Updated worker node images with kernel update for [CVE-2018-14633, CVE-2018-17182 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-137.163/changelog).</td>
</tr>
<tr>
<td>Inactive session timeout</td>
<td>N/A</td>
<td>N/A</td>
<td>Set the inactive session timeout to 5 minutes for compliance reasons.</td>
</tr>
</tbody>
</table>


#### Changelog for 1.11.3_1524, released 2 October 2018
{: #1113_1524}

The following table shows the changes that are included in patch 1.11.3_1524.
{: shortdesc}

<table summary="Changes that were made since version 1.11.3_1521">
<caption>Changes since version 1.11.3_1521</caption>
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
<td>containerd</td>
<td>1.1.3</td>
<td>1.1.4</td>
<td>See the [containerd release notes![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/containerd/containerd/releases/tag/v1.1.4).</td>
</tr>
<tr>
<td>{{site.data.keyword.cloud_notm}} Provider</td>
<td>v1.11.3-91</td>
<td>v1.11.3-100</td>
<td>Updated the documentation link in load balancer error messages.</td>
</tr>
<tr>
<td>IBM file storage classes</td>
<td>N/A</td>
<td>N/A</td>
<td>Removed duplicate `reclaimPolicy` parameter in the IBM file storage classes.<br><br>
Also, now when you update the cluster master, the default IBM file storage class remains unchanged. If you want to change the default storage class, run `kubectl patch storageclass <storageclass> -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'` and replace `<storageclass>` with the name of the storage class.</td>
</tr>
</tbody>
</table>

#### Changelog for 1.11.3_1521, released 20 September 2018
{: #1113_1521}

The following table shows the changes that are included in patch 1.11.3_1521.
{: shortdesc}

<table summary="Changes that were made since version 1.11.2_1516">
<caption>Changes since version 1.11.2_1516</caption>
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
<td>{{site.data.keyword.cloud_notm}} Provider</td>
<td>v1.11.2-71</td>
<td>v1.11.3-91</td>
<td>Updated to support Kubernetes 1.11.3 release.</td>
</tr>
<tr>
<td>IBM file storage classes</td>
<td>N/A</td>
<td>N/A</td>
<td>Removed `mountOptions` in the IBM file storage classes to use the default that is provided by the worker node.<br><br>
Also, now when you update the cluster master, the default IBM file storage class remains `ibmc-file-bronze`. If you want to change the default storage class, run `kubectl patch storageclass <storageclass> -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'` and replace `<storageclass>` with the name of the storage class.</td>
</tr>
<tr>
<td>Key Management Service Provider</td>
<td>N/A</td>
<td>N/A</td>
<td>Added the ability to use the Kubernetes key management service (KMS) provider in the cluster, to support {{site.data.keyword.keymanagementservicefull}}. When you [enable {{site.data.keyword.keymanagementserviceshort}} in your cluster](/docs/containers?topic=containers-encryption#keyprotect), all your Kubernetes secrets are encrypted.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.11.2</td>
<td>v1.11.3</td>
<td>See the [Kubernetes release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/kubernetes/kubernetes/releases/tag/v1.11.3).</td>
</tr>
<tr>
<td>Kubernetes DNS autoscaler</td>
<td>1.1.2-r2</td>
<td>1.2.0</td>
<td>See the [Kubernetes DNS autoscaler release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/kubernetes-incubator/cluster-proportional-autoscaler/releases/tag/1.2.0).</td>
</tr>
<tr>
<td>Log rotate</td>
<td>N/A</td>
<td>N/A</td>
<td>Switched to use `systemd` timers instead of `cronjobs` to prevent `logrotate` from failing on worker nodes that are not reloaded or updated within 90 days. **Note**: In all earlier versions for this minor release, the primary disk fills up after the cron job fails because the logs are not rotated. The cron job fails after the worker node is active for 90 days without being updated or reloaded. If the logs fill up the entire primary disk, the worker node enters a failed state. The worker node can be fixed by using the `ibmcloud ks worker-reload` [command](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload) or the `ibmcloud ks worker-update` [command](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update).</td>
</tr>
<tr>
<td>Root password expiration</td>
<td>N/A</td>
<td>N/A</td>
<td>Root passwords for the worker nodes expire after 90 days for compliance reasons. If your automation tooling needs to log in to the worker node as root or relies on cron jobs that run as root, you can disable the password expiration by logging into the worker node and running `chage -M -1 root`. **Note**: If you have security compliance requirements that prevent running as root or removing password expiration, do not disable the expiration. Instead, you can [update](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update) or [reload](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload) your worker nodes at least every 90 days.</td>
</tr>
<tr>
<td>Worker node runtime components (`kubelet`, `kube-proxy`, `containerd`)</td>
<td>N/A</td>
<td>N/A</td>
<td>Removed dependencies of runtime components on the primary disk. This enhancement prevents worker nodes from failing when the primary disk is filled up.</td>
</tr>
<tr>
<td>systemd</td>
<td>N/A</td>
<td>N/A</td>
<td>Periodically clean transient mount units to prevent them from becoming unbounded. This action addresses [Kubernetes issue 57345 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/kubernetes/kubernetes/issues/57345).</td>
</tr>
</tbody>
</table>

#### Changelog for 1.11.2_1516, released 4 September 2018
{: #1112_1516}

The following table shows the changes that are included in patch 1.11.2_1516.
{: shortdesc}

<table summary="Changes that were made since version 1.11.2_1514">
<caption>Changes since version 1.11.2_1514</caption>
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
<td>Calico</td>
<td>v3.1.3</td>
<td>v3.2.1</td>
<td>See the [Calico release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://docs.projectcalico.org/v3.2/releases/#v321).</td>
</tr>
<tr>
<td>containerd</td>
<td>1.1.2</td>
<td>1.1.3</td>
<td>See the [`containerd` release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/containerd/containerd/releases/tag/v1.1.3).</td>
</tr>
<tr>
<td>{{site.data.keyword.cloud_notm}} Provider</td>
<td>v1.11.2-60</td>
<td>v1.11.2-71</td>
<td>Changed the cloud provider configuration to better handle updates for load balancer services with `externalTrafficPolicy` set to `local`.</td>
</tr>
<tr>
<td>{{site.data.keyword.cloud_notm}} File Storage plug-in configuration</td>
<td>N/A</td>
<td>N/A</td>
<td>Removed the default NFS version from the mount options in the IBM-provided file storage classes. The host's operating system now negotiates the NFS version with the IBM Cloud infrastructure NFS server. To manually set a specific NFS version, or to change the NFS version of your PV that was negotiated by the host's operating system, see [Changing the default NFS version](/docs/containers?topic=containers-file_storage#nfs_version_class).</td>
</tr>
</tbody>
</table>

#### Changelog for worker node fix pack 1.11.2_1514, released 23 August 2018
{: #1112_1514}

The following table shows the changes that are included in the worker node fix pack 1.11.2_1514.
{: shortdesc}

<table summary="Changes that were made since version 1.11.2_1513">
<caption>Changes since version 1.11.2_1513</caption>
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
<td>`systemd`</td>
<td>229</td>
<td>230</td>
<td>Updated `systemd` to fix `cgroup` leak.</td>
</tr>
<tr>
<td>Kernel</td>
<td>4.4.0-127</td>
<td>4.4.0-133</td>
<td>Updated worker node images with kernel update for [CVE-2018-3620,CVE-2018-3646 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://usn.ubuntu.com/3741-1/).</td>
</tr>
</tbody>
</table>

#### Changelog for 1.11.2_1513, released 14 August 2018
{: #1112_1513}

The following table shows the changes that are included in patch 1.11.2_1513.
{: shortdesc}

<table summary="Changes that were made since version 1.10.5_1518">
<caption>Changes since version 1.10.5_1518</caption>
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
<td>containerd</td>
<td>N/A</td>
<td>1.1.2</td>
<td>`containerd` replaces Docker as the new container runtime for Kubernetes. See the [`containerd` release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/containerd/containerd/releases/tag/v1.1.2).</td>
</tr>
<tr>
<td>Docker</td>
<td>N/A</td>
<td>N/A</td>
<td>`containerd` replaces Docker as the new container runtime for Kubernetes, to enhance performance.</td>
</tr>
<tr>
<td>etcd</td>
<td>v3.2.14</td>
<td>v3.2.18</td>
<td>See the [etcd release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/coreos/etcd/releases/v3.2.18).</td>
</tr>
<tr>
<td>{{site.data.keyword.cloud_notm}} Provider</td>
<td>v1.10.5-118</td>
<td>v1.11.2-60</td>
<td>Updated to support Kubernetes 1.11 release. In addition, load balancer pods now use the new `ibm-app-cluster-critical` pod priority class.</td>
</tr>
<tr>
<td>{{site.data.keyword.cloud_notm}} File Storage plug-in</td>
<td>334</td>
<td>338</td>
<td>Updated `incubator` version to 1.8. File storage is provisioned to the specific zone that you select. You cannot update an existing (static) PV instance labels, unless you are using a multizone cluster and need to [add the region the zone labels](/docs/containers?topic=containers-kube_concepts#storage_multizone).</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.10.5</td>
<td>v1.11.2</td>
<td>See the [Kubernetes release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/kubernetes/kubernetes/releases/tag/v1.11.2).</td>
</tr>
<tr>
<td>Kubernetes configuration</td>
<td>N/A</td>
<td>N/A</td>
<td>Updated the OpenID Connect configuration for the cluster's Kubernetes API server to support {{site.data.keyword.cloud_notm}} Identity Access and Management (IAM) access groups. Added `Priority` to the `--enable-admission-plugins` option for the cluster's Kubernetes API server and configured the cluster to support pod priority. For more information, see:
<ul><li>[{{site.data.keyword.cloud_notm}} IAM access groups](/docs/containers?topic=containers-users#rbac)</li>
<li>[Configuring pod priority](/docs/containers?topic=containers-pod_priority#pod_priority)</li></ul></td>
</tr>
<tr>
<td>Kubernetes Heapster</td>
<td>v1.5.2</td>
<td>v.1.5.4</td>
<td>Increased resource limits for the `heapster-nanny` container. See the [Kubernetes Heapster release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/kubernetes/heapster/releases/tag/v1.5.4).</td>
</tr>
<tr>
<td>Logging configuration</td>
<td>N/A</td>
<td>N/A</td>
<td>The container log directory is now `/var/log/pods/` instead of the previous `/var/lib/docker/containers/`.</td>
</tr>
</tbody>
</table>

<br />



### Version 1.10 changelog (unsupported as of 16 May 2019)
{: #110_changelog}

Review the version 1.10 changelogs.
{: shortdesc}

*   [Changelog for worker node fix pack 1.10.13_1558, released 13 May 2019](#11013_1558)
*   [Changelog for worker node fix pack 1.10.13_1557, released 29 April 2019](#11013_1557)
*   [Changelog for worker node fix pack 1.10.13_1556, released 15 April 2019](#11013_1556)
*   [Changelog for 1.10.13_1555, released 8 April 2019](#11013_1555)
*   [Changelog for worker node fix pack 1.10.13_1554, released 1 April 2019](#11013_1554)
*   [Changelog for master fix pack 1.10.13_1553, released 26 March 2019](#11118_1553)
*   [Changelog for 1.10.13_1551, released 20 March 2019](#11013_1551)
*   [Changelog for 1.10.13_1548, released 4 March 2019](#11013_1548)
*   [Changelog for worker node fix pack 1.10.12_1546, released 27 February 2019](#11012_1546)
*   [Changelog for worker node fix pack 1.10.12_1544, released 15 February 2019](#11012_1544)
*   [Changelog for 1.10.12_1543, released 5 February 2019](#11012_1543)
*   [Changelog for worker node fix pack 1.10.12_1541, released 28 January 2019](#11012_1541)
*   [Changelog for 1.10.12_1540, released 21 January 2019](#11012_1540)
*   [Changelog for worker node fix pack 1.10.11_1538, released 7 January 2019](#11011_1538)
*   [Changelog for worker node fix pack 1.10.11_1537, released 17 December 2018](#11011_1537)
*   [Changelog for 1.10.11_1536, released 4 December 2018](#11011_1536)
*   [Changelog for worker node fix pack 1.10.8_1532, released 27 November 2018](#1108_1532)
*   [Changelog for worker node fix pack 1.10.8_1531, released 19 November 2018](#1108_1531)
*   [Changelog for 1.10.8_1530, released 7 November 2018](#1108_1530_ha-master)
*   [Changelog for worker node fix pack 1.10.8_1528, released 26 October 2018](#1108_1528)
*   [Changelog for worker node fix pack 1.10.8_1525, released 10 October 2018](#1108_1525)
*   [Changelog for 1.10.8_1524, released 2 October 2018](#1108_1524)
*   [Changelog for worker node fix pack 1.10.7_1521, released 20 September 2018](#1107_1521)
*   [Changelog for 1.10.7_1520, released 4 September 2018](#1107_1520)
*   [Changelog for worker node fix pack 1.10.5_1519, released 23 August 2018](#1105_1519)
*   [Changelog for worker node fix pack 1.10.5_1518, released 13 August 2018](#1105_1518)
*   [Changelog for 1.10.5_1517, released 27 July 2018](#1105_1517)
*   [Changelog for worker node fix pack 1.10.3_1514, released 3 July 2018](#1103_1514)
*   [Changelog for worker node fix pack 1.10.3_1513, released 21 June 2018](#1103_1513)
*   [Changelog for 1.10.3_1512, released 12 June 2018](#1103_1512)
*   [Changelog for worker node fix pack 1.10.1_1510, released 18 May 2018](#1101_1510)
*   [Changelog for worker node fix pack 1.10.1_1509, released 16 May 2018](#1101_1509)
*   [Changelog for 1.10.1_1508, released 01 May 2018](#1101_1508)

#### Changelog for worker node fix pack 1.10.13_1558, released 13 May 2019
{: #11013_1558}

The following table shows the changes that are included in the worker node fix pack 1.10.13_1558.
{: shortdesc}

<table summary="Changes that were made since version 1.10.13_1557">
<caption>Changes since version 1.10.13_1557</caption>
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
<td>Cluster master HA proxy</td>
<td>1.9.6-alpine</td>
<td>1.9.7-alpine</td>
<td>See the [HAProxy release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.haproxy.org/download/1.9/src/CHANGELOG). Update resolves [CVE-2019-6706 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6706).</td>
</tr>
</tbody>
</table>

#### Changelog for worker node fix pack 1.10.13_1557, released 29 April 2019
{: #11013_1557}

The following table shows the changes that are included in the worker node fix pack 1.10.13_1557.
{: shortdesc}

<table summary="Changes that were made since version 1.10.13_1556">
<caption>Changes since 1.10.13_1556</caption>
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
<td>Ubuntu packages</td>
<td>N/A</td>
<td>N/A</td>
<td>Updates to installed Ubuntu packages.</td>
</tr>
</tbody>
</table>


#### Changelog for worker node fix pack 1.10.13_1556, released 15 April 2019
{: #11013_1556}

The following table shows the changes that are included in the worker node fix pack 1.10.13_1556.
{: shortdesc}

<table summary="Changes that were made since version 1.10.13_1555">
<caption>Changes since 1.10.13_1555</caption>
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
<td>Ubuntu packages</td>
<td>N/A</td>
<td>N/A</td>
<td>Updates to installed Ubuntu packages including `systemd` for [CVE-2019-3842 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-3842.html).</td>
</tr>
</tbody>
</table>

#### Changelog for 1.10.13_1555, released 8 April 2019
{: #11013_1555}

The following table shows the changes that are included in the patch 1.10.13_1555.
{: shortdesc}

<table summary="Changes that were made since version 1.10.13_1554">
<caption>Changes since version 1.10.13_1554</caption>
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
<td>Cluster master HA proxy</td>
<td>1.8.12-alpine</td>
<td>1.9.6-alpine</td>
<td>See the [HAProxy release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.haproxy.org/download/1.9/src/CHANGELOG). Update resolves [CVE-2018-0732 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0732), [CVE-2018-0734 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734), [CVE-2018-0737 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0737), [CVE-2018-5407 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407), [CVE-2019-1543 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1543), and [CVE-2019-1559 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1559).</td>
</tr>
<tr>
<td>Kubernetes DNS</td>
<td>1.14.10</td>
<td>1.14.13</td>
<td>See the [Kubernetes DNS release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/kubernetes/dns/releases/tag/1.14.13).</td>
</tr>
<tr>
<td>Trusted compute agent</td>
<td>a02f765</td>
<td>e132aa4</td>
<td>Updated image for [CVE-2017-12447 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-12447).</td>
</tr>
<tr>
<td>Ubuntu 16.04 kernel</td>
<td>4.4.0-143-generic</td>
<td>4.4.0-145-generic</td>
<td>Updated worker node images with kernel update for [CVE-2019-9213 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9213.html).</td>
</tr>
<tr>
<td>Ubuntu 18.04 kernel</td>
<td>4.15.0-46-generic</td>
<td>4.15.0-47-generic</td>
<td>Updated worker node images with kernel update for [CVE-2019-9213 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9213.html).</td>
</tr>
</tbody>
</table>

#### Changelog for worker node fix pack 1.10.13_1554, released 1 April 2019
{: #11013_1554}

The following table shows the changes that are included in the worker node fix 1.10.13_1554.
{: shortdesc}

<table summary="Changes that were made since version 1.10.13_1553">
<caption>Changes since version 1.10.13_1553</caption>
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
<td>Worker node resource utilization</td>
<td>N/A</td>
<td>N/A</td>
<td>Increased memory reservations for the kubelet and containerd to prevent these components from running out of resources. For more information, see [Worker node resource reserves](/docs/containers?topic=containers-planning_worker_nodes#resource_limit_node).</td>
</tr>
</tbody>
</table>


#### Changelog for master fix pack 1.10.13_1553, released 26 March 2019
{: #11118_1553}

The following table shows the changes that are included in the master fix pack 1.10.13_1553.
{: shortdesc}

<table summary="Changes that were made since version 1.10.13_1551">
<caption>Changes since version 1.10.13_1551</caption>
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
<td>{{site.data.keyword.cloud_notm}} File Storage plug-in</td>
<td>345</td>
<td>346</td>
<td>Updated image for [CVE-2019-9741 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741).</td>
</tr>
<tr>
<td>Key Management Service provider</td>
<td>166</td>
<td>167</td>
<td>Fixes intermittent `context deadline exceeded` and `timeout` errors for managing Kubernetes secrets. In addition, fixes updates to the key management service that might leave existing Kubernetes secrets unencrypted. Update includes fix for [CVE-2019-9741 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741).</td>
</tr>
<tr>
<td>Load balancer and load balancer monitor for {{site.data.keyword.cloud_notm}} Provider</td>
<td>143</td>
<td>146</td>
<td>Updated image for [CVE-2019-9741 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9741).</td>
</tr>
</tbody>
</table>

#### Changelog for 1.10.13_1551, released 20 March 2019
{: #11013_1551}

The following table shows the changes that are included in the patch 1.10.13_1551.
{: shortdesc}

<table summary="Changes that were made since version 1.10.13_1548">
<caption>Changes since version 1.10.13_1548</caption>
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
<td>Cluster master HA proxy configuration</td>
<td>N/A</td>
<td>N/A</td>
<td>Updated configuration to better handle intermittent connection failures to the cluster master.</td>
</tr>
<tr>
<td>GPU device plug-in and installer</td>
<td>e32d51c</td>
<td>9ff3fda</td>
<td>Updated the GPU drivers to [418.43 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.nvidia.com/object/unix.html). Update includes fix for [CVE-2019-9741 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-9741.html).</td>
</tr>
<tr>
<td>{{site.data.keyword.cloud_notm}} File Storage plug-in</td>
<td>344</td>
<td>345</td>
<td>Added support for [private service endpoints](/docs/containers?topic=containers-cs_network_cluster#set-up-private-se).</td>
</tr>
<tr>
<td>Kernel</td>
<td>4.4.0-141</td>
<td>4.4.0-143</td>
<td>Updated worker node images with kernel update for [CVE-2019-6133 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://people.canonical.com/~ubuntu-security/cve/2019/CVE-2019-6133.html).</td>
</tr>
<tr>
<td>Key Management Service provider</td>
<td>136</td>
<td>166</td>
<td>Updated image for [CVE-2018-16890 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-16890), [CVE-2019-3822 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3822), and [CVE-2019-3823 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3823).</td>
</tr>
<tr>
<td>Trusted compute agent</td>
<td>5f3d092</td>
<td>a02f765</td>
<td>Updated image for [CVE-2018-10779 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10779), [CVE-2018-12900 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-12900), [CVE-2018-17000 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-17000), [CVE-2018-19210 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-19210), [CVE-2019-6128 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6128), and [CVE-2019-7663 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-7663).</td>
</tr>
</tbody>
</table>

#### Changelog for 1.10.13_1548, released 4 March 2019
{: #11013_1548}

The following table shows the changes that are included in the patch 1.10.13_1548.
{: shortdesc}

<table summary="Changes that were made since version 1.10.12_1546">
<caption>Changes since version 1.10.12_1546</caption>
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
<td>GPU device plug-in and installer</td>
<td>eb3a259</td>
<td>e32d51c</td>
<td>Updated images for [CVE-2019-6454 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6454).</td>
</tr>
<tr>
<td>{{site.data.keyword.cloud_notm}} Provider</td>
<td>v1.10.12-252</td>
<td>v1.10.13-288</td>
<td>Updated to support the Kubernetes 1.10.13 release. Fixed periodic connectivity problems for load balancers that set `externalTrafficPolicy` to `local`. Updated load balancer events to use the latest {{site.data.keyword.cloud_notm}} documentation links.</td>
</tr>
<tr>
<td>{{site.data.keyword.cloud_notm}} File Storage plug-in</td>
<td>342</td>
<td>344</td>
<td>Changed the base operating system for the image from Fedora to Alpine. Updated image for [CVE-2019-6486 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486).</td>
</tr>
<tr>
<td>Key Management Service provider</td>
<td>122</td>
<td>136</td>
<td>Increased client timeout to {{site.data.keyword.keymanagementservicefull_notm}}. Updated image for [CVE-2019-6486 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486).</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.10.12</td>
<td>v1.10.13</td>
<td>See the [Kubernetes release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/kubernetes/kubernetes/releases/tag/v1.10.13).</td>
</tr>
<tr>
<td>Kubernetes DNS</td>
<td>N/A</td>
<td>N/A</td>
<td>Increased Kubernetes DNS pod memory limit from `170Mi` to `400Mi` in order to handle more cluster services.</td>
</tr>
<tr>
<td>Load balancer and load balancer monitor for {{site.data.keyword.cloud_notm}} Provider</td>
<td>132</td>
<td>143</td>
<td>Updated image for [CVE-2019-6486 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486).</td>
</tr>
<tr>
<td>OpenVPN client and server</td>
<td>2.4.6-r3-IKS-13</td>
<td>2.4.6-r3-IKS-25</td>
<td>Updated image for [CVE-2019-1559 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1559).</td>
</tr>
<tr>
<td>Trusted compute agent</td>
<td>1ea5ad3</td>
<td>5f3d092</td>
<td>Updated image for [CVE-2019-6454 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6454).</td>
</tr>
</tbody>
</table>

#### Changelog for worker node fix pack 1.10.12_1546, released 27 February 2019
{: #11012_1546}

The following table shows the changes that are included in the worker node fix pack 1.10.12_1546.
{: shortdesc}

<table summary="Changes that were made since version 1.10.12_1544">
<caption>Changes since version 1.10.12_1544</caption>
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
<td>Kernel</td>
<td>4.4.0-141</td>
<td>4.4.0-142</td>
<td>Updated worker node images with kernel update for [CVE-2018-19407 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-142.168/changelog).</td>
</tr>
</tbody>
</table>

#### Changelog for worker node fix pack 1.10.12_1544, released 15 February 2019
{: #11012_1544}

The following table shows the changes that are included in the worker node fix pack 1.10.12_1544.
{: shortdesc}

<table summary="Changes that were made since version 1.10.12_1543">
<caption>Changes since version 1.10.12_1543</caption>
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
<td>Docker</td>
<td>18.06.1-ce</td>
<td>18.06.2-ce</td>
<td>See the [Docker Community Edition release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/docker/docker-ce/releases/tag/v18.06.2-ce). Update resolves [CVE-2019-5736 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5736).</td>
</tr>
<tr>
<td>Kubernetes `kubelet` configuration</td>
<td>N/A</td>
<td>N/A</td>
<td>Enabled the `ExperimentalCriticalPodAnnotation` feature gate to prevent critical static pod eviction.</td>
</tr>
</tbody>
</table>

#### Changelog for 1.10.12_1543, released 5 February 2019
{: #11012_1543}

The following table shows the changes that are included in the patch 1.10.12_1543.
{: shortdesc}

<table summary="Changes that were made since version 1.10.12_1541">
<caption>Changes since version 1.10.12_1541</caption>
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
<td>etcd</td>
<td>v3.3.1</td>
<td>v3.3.11</td>
<td>See the [etcd release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/coreos/etcd/releases/v3.3.11). Additionally, the supported cipher suites to etcd are now restricted to a subset with high strength encryption (128 bits or more).</td>
</tr>
<tr>
<td>GPU device plug-in and installer</td>
<td>13fdc0d</td>
<td>eb3a259</td>
<td>Updated images for [CVE-2019-3462 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3462) and [CVE-2019-6486 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-6486).</td>
</tr>
<tr>
<td>{{site.data.keyword.cloud_notm}} File Storage plug-in</td>
<td>338</td>
<td>342</td>
<td>The file storage plug-in is updated as follows:
<ul><li>Supports dynamic provisioning with [volume topology-aware scheduling](/docs/containers?topic=containers-file_storage#file-topology).</li>
<li>Ignores persistent volume claim (PVC) delete errors if the storage is already deleted.</li>
<li>Adds a failure message annotation to failed PVCs.</li>
<li>Optimizes the storage provisioner controller's leader election and resync period settings, and increases the provisioning timeout from 30 minutes to 1 hour.</li>
<li>Checks user permissions before starting the provisioning.</li></ul></td>
</tr>
<tr>
<td>Key Management Service provider</td>
<td>111</td>
<td>122</td>
<td>Added retry logic to avoid temporary failures when Kubernetes secrets are managed by {{site.data.keyword.keymanagementservicefull_notm}}.</td>
</tr>
<tr>
<td>Kubernetes configuration</td>
<td>N/A</td>
<td>N/A</td>
<td>The Kubernetes API server audit policy configuration is updated to include logging metadata for `cluster-admin` requests and logging the request body of workload `create`, `update`, and `patch` requests.</td>
</tr>
<tr>
<td>OpenVPN client</td>
<td>2.4.6-r3-IKS-8</td>
<td>2.4.6-r3-IKS-13</td>
<td>Updated image for [CVE-2018-0734 ![External link icon](../icons/launch-glyph.svg "External link icon")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734) and [CVE-2018-5407 ![External link icon](../icons/launch-glyph.svg "External link icon")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407). Additionally, the pod configuration is now obtained from a secret instead of from a configmap.</td>
</tr>
<tr>
<td>OpenVPN server</td>
<td>2.4.6-r3-IKS-8</td>
<td>2.4.6-r3-IKS-13</td>
<td>Updated image for [CVE-2018-0734 ![External link icon](../icons/launch-glyph.svg "External link icon")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0734) and [CVE-2018-5407 ![External link icon](../icons/launch-glyph.svg "External link icon")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-5407).</td>
</tr>
<tr>
<td>systemd</td>
<td>230</td>
<td>229</td>
<td>Security patch for [CVE-2018-16864 ![External link icon](../icons/launch-glyph.svg "External link icon")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-16864).</td>
</tr>
</tbody>
</table>

#### Changelog for worker node fix pack 1.10.12_1541, released 28 January 2019
{: #11012_1541}

The following table shows the changes that are included in the worker node fix pack 1.10.12_1541.
{: shortdesc}

<table summary="Changes that were made since version 1.10.12_1540">
<caption>Changes since version 1.10.12_1540</caption>
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
<td>Ubuntu packages</td>
<td>N/A</td>
<td>N/A</td>
<td>Updates to installed Ubuntu packages including `apt` for [CVE-2019-3462 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3462) and [USN-3863-1 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://usn.ubuntu.com/3863-1).</td>
</tr>
</tbody>
</table>

#### Changelog for 1.10.12_1540, released 21 January 2019
{: #11012_1540}

The following table shows the changes that are included in the patch 1.10.12_1540.
{: shortdesc}

<table summary="Changes that were made since version 1.10.11_1538">
<caption>Changes since version 1.10.11_1538</caption>
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
<td>{{site.data.keyword.cloud_notm}} Provider</td>
<td>v1.10.11-219</td>
<td>v1.10.12-252</td>
<td>Updated to support the Kubernetes 1.10.12 release.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.10.11</td>
<td>v1.10.12</td>
<td>See the [Kubernetes release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/kubernetes/kubernetes/releases/tag/v1.10.12).</td>
</tr>
<tr>
<td>Kubernetes add-on resizer</td>
<td>1.8.1</td>
<td>1.8.4</td>
<td>See the [Kubernetes add-on resizer release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/kubernetes/autoscaler/releases/tag/addon-resizer-1.8.4).</td>
</tr>
<tr>
<td>Kubernetes dashboard</td>
<td>v1.8.3</td>
<td>v1.10.1</td>
<td>See the [Kubernetes dashboard release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/kubernetes/dashboard/releases/tag/v1.10.1). Update resolves [CVE-2018-18264 ![External link icon](../icons/launch-glyph.svg "External link icon")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-18264).<br><br>If you access the dashboard via `kubectl proxy`, the **SKIP** button on the login page is removed. Instead, [use a **Token** to log in](/docs/containers?topic=containers-app#cli_dashboard).</td>
</tr>
<tr>
<td>GPU installer</td>
<td>390.12</td>
<td>410.79</td>
<td>Updated the installed GPU drivers to 410.79.</td>
</tr>
</tbody>
</table>

#### Changelog for worker node fix pack 1.10.11_1538, released 7 January 2019
{: #11011_1538}

The following table shows the changes that are included in the worker node fix pack 1.10.11_1538.
{: shortdesc}

<table summary="Changes that were made since version 1.10.11_1537">
<caption>Changes since version 1.10.11_1537</caption>
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
<td>Kernel</td>
<td>4.4.0-139</td>
<td>4.4.0-141</td>
<td>Updated worker node images with kernel update for [CVE-2017-5753, CVE-2018-18690 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-141.167/changelog).</td>
</tr>
</tbody>
</table>

#### Changelog for worker node fix pack 1.10.11_1537, released 17 December 2018
{: #11011_1537}

The following table shows the changes that are included in the worker node fix pack 1.10.11_1537.
{: shortdesc}

<table summary="Changes that were made since version 1.10.11_1536">
<caption>Changes since version 1.10.11_1536</caption>
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
<td>Ubuntu packages</td>
<td>N/A</td>
<td>N/A</td>
<td>Updates to installed Ubuntu packages.</td>
</tr>
</tbody>
</table>


#### Changelog for 1.10.11_1536, released 4 December 2018
{: #11011_1536}

The following table shows the changes that are included in patch 1.10.11_1536.
{: shortdesc}

<table summary="Changes that were made since version 1.10.8_1532">
<caption>Changes since version 1.10.8_1532</caption>
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
<td>Calico</td>
<td>v3.2.1</td>
<td>v3.3.1</td>
<td>See the [Calico release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://docs.projectcalico.org/v3.3/releases/#v331). Update resolves [Tigera Technical Advisory TTA-2018-001 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.projectcalico.org/security-bulletins/).</td>
</tr>
<tr>
<td>{{site.data.keyword.cloud_notm}} Provider</td>
<td>v1.10.8-197</td>
<td>v1.10.11-219</td>
<td>Updated to support the Kubernetes 1.10.11 release.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.10.8</td>
<td>v1.10.11</td>
<td>See the [Kubernetes release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/kubernetes/kubernetes/releases/tag/v1.10.11). Update resolves [CVE-2018-1002105 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/kubernetes/kubernetes/issues/71411).</td>
</tr>
<tr>
<td>OpenVPN client and server</td>
<td>2.4.4-r1-6</td>
<td>2.4.6-r3-IKS-8</td>
<td>Updated image for [CVE-2018-0732 ![External link icon](../icons/launch-glyph.svg "External link icon")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0732) and [CVE-2018-0737 ![External link icon](../icons/launch-glyph.svg "External link icon")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0737).</td>
</tr>
<tr>
<td>Worker node resource utilization</td>
<td>N/A</td>
<td>N/A</td>
<td>Added dedicated cgroups for the kubelet and docker to prevent these components from running out of resources. For more information, see [Worker node resource reserves](/docs/containers?topic=containers-planning_worker_nodes#resource_limit_node).</td>
</tr>
</tbody>
</table>

#### Changelog for worker node fix pack 1.10.8_1532, released 27 November 2018
{: #1108_1532}

The following table shows the changes that are included in the worker node fix pack 1.10.8_1532.
{: shortdesc}

<table summary="Changes that were made since version 1.10.8_1531">
<caption>Changes since version 1.10.8_1531</caption>
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
<td>Docker</td>
<td>17.06.2</td>
<td>18.06.1</td>
<td>See the [Docker release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://docs.docker.com/engine/release-notes/#18061-ce).</td>
</tr>
</tbody>
</table>

#### Changelog for worker node fix pack 1.10.8_1531, released 19 November 2018
{: #1108_1531}

The following table shows the changes that are included in the worker node fix pack 1.10.8_1531.
{: shortdesc}

<table summary="Changes that were made since version 1.10.8_1530">
<caption>Changes since version 1.10.8_1530</caption>
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
<td>Kernel</td>
<td>4.4.0-137</td>
<td>4.4.0-139</td>
<td>Updated worker node images with kernel update for [CVE-2018-7755 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-139.165/changelog).</td>
</tr>
</tbody>
</table>

#### Changelog for 1.10.8_1530, released 7 November 2018
{: #1108_1530_ha-master}

The following table shows the changes that are included in patch 1.10.8_1530.
{: shortdesc}

<table summary="Changes that were made since version 1.10.8_1528">
<caption>Changes since version 1.10.8_1528</caption>
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
<td>Cluster master</td>
<td>N/A</td>
<td>N/A</td>
<td>Updated the cluster master configuration to increase high availability (HA). Clusters now have three Kubernetes master replicas that are set up with a highly available (HA) configuration, with each master deployed on separate physical hosts. Further, if your cluster is in a multizone-capable zone, the masters are spread across zones.</td>
</tr>
<tr>
<td>Cluster master HA proxy</td>
<td>N/A</td>
<td>1.8.12-alpine</td>
<td>Added an `ibm-master-proxy-*` pod for client-side load balancing on all worker nodes, so that each worker node client can route requests to an available HA master replica.</td>
</tr>
<tr>
<td>etcd</td>
<td>v3.2.18</td>
<td>v3.3.1</td>
<td>See the [etcd release notes![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/coreos/etcd/releases/v3.3.1).</td>
</tr>
<tr>
<td>Encrypting data in etcd</td>
<td>N/A</td>
<td>N/A</td>
<td>Previously, etcd data was stored on a master’s NFS file storage instance that is encrypted at rest. Now, etcd data is stored on the master’s local disk and backed up to {{site.data.keyword.cos_full_notm}}. Data is encrypted during transit to {{site.data.keyword.cos_full_notm}} and at rest. However, the etcd data on the master’s local disk is not encrypted. If you want your master’s local etcd data to be encrypted, [enable {{site.data.keyword.keymanagementservicelong_notm}} in your cluster](/docs/containers?topic=containers-encryption#keyprotect).</td>
</tr>
<tr>
<td>{{site.data.keyword.cloud_notm}} Provider</td>
<td>v1.10.8-172</td>
<td>v1.10.8-197</td>
<td>Added the `service.kubernetes.io/ibm-load-balancer-cloud-provider-vlan` annotation to specify the VLAN that the load balancer service deploys to. To see available VLANs in your cluster, run `ibmcloud ks vlans --zone <zone>`.</td>
</tr>
<tr>
<td>TPM-enabled kernel</td>
<td>N/A</td>
<td>N/A</td>
<td>Bare metal worker nodes with TPM chips for Trusted Compute use the default Ubuntu kernel until trust is enabled. If you [enable trust](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_feature_enable) on an existing cluster, you need to [reload](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload) any existing bare metal worker nodes with TPM chips. To check if a bare metal worker node has a TPM chip, review the **Trustable** field after running the `ibmcloud ks flavors --zone` [command](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_machine_types).</td>
</tr>
</tbody>
</table>

#### Changelog for worker node fix pack 1.10.8_1528, released 26 October 2018
{: #1108_1528}

The following table shows the changes that are included in the worker node fix pack 1.10.8_1528.
{: shortdesc}

<table summary="Changes that were made since version 1.10.8_1527">
<caption>Changes since version 1.10.8_1527</caption>
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
<td>OS interrupt handling</td>
<td>N/A</td>
<td>N/A</td>
<td>Replaced the interrupt request (IRQ) system daemon with a more performant interrupt handler.</td>
</tr>
</tbody>
</table>

#### Changelog for master fix pack 1.10.8_1527, released 15 October 2018
{: #1108_1527}

The following table shows the changes that are included in the master fix pack 1.10.8_1527.
{: shortdesc}

<table summary="Changes that were made since version 1.10.8_1524">
<caption>Changes since version 1.10.8_1524</caption>
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
<td>Calico configuration</td>
<td>N/A</td>
<td>N/A</td>
<td>Fixed `calico-node` container readiness probe to better handle node failures.</td>
</tr>
<tr>
<td>Cluster update</td>
<td>N/A</td>
<td>N/A</td>
<td>Fixed problem with updating cluster add-ons when the master is updated from an unsupported version.</td>
</tr>
</tbody>
</table>

#### Changelog for worker node fix pack 1.10.8_1525, released 10 October 2018
{: #1108_1525}

The following table shows the changes that are included in the worker node fix pack 1.10.8_1525.
{: shortdesc}

<table summary="Changes that were made since version 1.10.8_1524">
<caption>Changes since version 1.10.8_1524</caption>
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
<td>Kernel</td>
<td>4.4.0-133</td>
<td>4.4.0-137</td>
<td>Updated worker node images with kernel update for [CVE-2018-14633, CVE-2018-17182 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-137.163/changelog).</td>
</tr>
<tr>
<td>Inactive session timeout</td>
<td>N/A</td>
<td>N/A</td>
<td>Set the inactive session timeout to 5 minutes for compliance reasons.</td>
</tr>
</tbody>
</table>


#### Changelog for 1.10.8_1524, released 2 October 2018
{: #1108_1524}

The following table shows the changes that are included in patch 1.10.8_1524.
{: shortdesc}

<table summary="Changes that were made since version 1.10.7_1520">
<caption>Changes since version 1.10.7_1520</caption>
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
<td>Key Management Service Provider</td>
<td>N/A</td>
<td>N/A</td>
<td>Added the ability to use the Kubernetes key management service (KMS) provider in the cluster, to support {{site.data.keyword.keymanagementservicefull}}. When you [enable {{site.data.keyword.keymanagementserviceshort}} in your cluster](/docs/containers?topic=containers-encryption#keyprotect), all your Kubernetes secrets are encrypted.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.10.7</td>
<td>v1.10.8</td>
<td>See the [Kubernetes release notes![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/kubernetes/kubernetes/releases/tag/v1.10.8).</td>
</tr>
<tr>
<td>Kubernetes DNS autoscaler</td>
<td>1.1.2-r2</td>
<td>1.2.0</td>
<td>See the [Kubernetes DNS autoscaler release notes![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/kubernetes-incubator/cluster-proportional-autoscaler/releases/tag/1.2.0).</td>
</tr>
<tr>
<td>{{site.data.keyword.cloud_notm}} Provider</td>
<td>v1.10.7-146</td>
<td>v1.10.8-172</td>
<td>Updated to support Kubernetes 1.10.8 release. Also, updated the documentation link in load balancer error messages.</td>
</tr>
<tr>
<td>IBM file storage classes</td>
<td>N/A</td>
<td>N/A</td>
<td>Removed `mountOptions` in the IBM file storage classes to use the default that is provided by the worker node. Removed duplicate `reclaimPolicy` parameter in the IBM file storage classes.<br><br>
Also, now when you update the cluster master, the default IBM file storage class remains unchanged. If you want to change the default storage class, run `kubectl patch storageclass <storageclass> -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'` and replace `<storageclass>` with the name of the storage class.</td>
</tr>
</tbody>
</table>

#### Changelog for worker node fix pack 1.10.7_1521, released 20 September 2018
{: #1107_1521}

The following table shows the changes that are included in the worker node fix pack 1.10.7_1521.
{: shortdesc}

<table summary="Changes that were made since version 1.10.7_1520">
<caption>Changes since version 1.10.7_1520</caption>
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
<td>Log rotate</td>
<td>N/A</td>
<td>N/A</td>
<td>Switched to use `systemd` timers instead of `cronjobs` to prevent `logrotate` from failing on worker nodes that are not reloaded or updated within 90 days. **Note**: In all earlier versions for this minor release, the primary disk fills up after the cron job fails because the logs are not rotated. The cron job fails after the worker node is active for 90 days without being updated or reloaded. If the logs fill up the entire primary disk, the worker node enters a failed state. The worker node can be fixed by using the `ibmcloud ks worker-reload` [command](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload) or the `ibmcloud ks worker-update` [command](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update).</td>
</tr>
<tr>
<td>Worker node runtime components (`kubelet`, `kube-proxy`, `docker`)</td>
<td>N/A</td>
<td>N/A</td>
<td>Removed dependencies of runtime components on the primary disk. This enhancement prevents worker nodes from failing when the primary disk is filled up.</td>
</tr>
<tr>
<td>Root password expiration</td>
<td>N/A</td>
<td>N/A</td>
<td>Root passwords for the worker nodes expire after 90 days for compliance reasons. If your automation tooling needs to log in to the worker node as root or relies on cron jobs that run as root, you can disable the password expiration by logging into the worker node and running `chage -M -1 root`. **Note**: If you have security compliance requirements that prevent running as root or removing password expiration, do not disable the expiration. Instead, you can [update](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update) or [reload](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload) your worker nodes at least every 90 days.</td>
</tr>
<tr>
<td>systemd</td>
<td>N/A</td>
<td>N/A</td>
<td>Periodically clean transient mount units to prevent them from becoming unbounded. This action addresses [Kubernetes issue 57345 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/kubernetes/kubernetes/issues/57345).</td>
</tr>
<tr>
<td>Docker</td>
<td>N/A</td>
<td>N/A</td>
<td>Disabled the default Docker bridge so that the `172.17.0.0/16` IP range is now used for private routes. If you rely on building Docker containers in worker nodes by executing `docker` commands on the host directly or by using a pod that mounts the Docker socket, choose from the following options.<ul><li>To ensure external network connectivity when you build the container, run `docker build . --network host`.</li>
<li>To explicitly create a network to use when you build the container, run `docker network create` and then use this network.</li></ul>
**Note**: Have dependencies on the Docker socket or Docker directly? Update to `containerd` instead of `docker` as the container runtime so that your clusters are prepared to run Kubernetes version 1.11 or later.</td>
</tr>
</tbody>
</table>

#### Changelog for 1.10.7_1520, released 4 September 2018
{: #1107_1520}

The following table shows the changes that are included in patch 1.10.7_1520.
{: shortdesc}

<table summary="Changes that were made since version 1.10.5_1519">
<caption>Changes since version 1.10.5_1519</caption>
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
<td>Calico</td>
<td>v3.1.3</td>
<td>v3.2.1</td>
<td>See the Calico [release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://docs.projectcalico.org/v3.2/releases/#v321).</td>
</tr>
<tr>
<td>{{site.data.keyword.cloud_notm}} Provider</td>
<td>v1.10.5-118</td>
<td>v1.10.7-146</td>
<td>Updated to support Kubernetes 1.10.7 release. In addition, changed the cloud provider configuration to better handle updates for load balancer services with `externalTrafficPolicy` set to `local`.</td>
</tr>
<tr>
<td>{{site.data.keyword.cloud_notm}} File Storage plug-in</td>
<td>334</td>
<td>338</td>
<td>Updated incubator version to 1.8. File storage is provisioned to the specific zone that you select. You cannot update an existing (static) PV instance's labels, unless you are using a multizone cluster and need to add the region and zone labels.<br><br> Removed the default NFS version from the mount options in the IBM-provided file storage classes. The host's operating system now negotiates the NFS version with the IBM Cloud infrastructure NFS server. To manually set a specific NFS version, or to change the NFS version of your PV that was negotiated by the host's operating system, see [Changing the default NFS version](/docs/containers?topic=containers-file_storage#nfs_version_class).</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.10.5</td>
<td>v1.10.7</td>
<td>See the Kubernetes [release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/kubernetes/kubernetes/releases/tag/v1.10.7).</td>
</tr>
<tr>
<td>Kubernetes Heapster configuration</td>
<td>N/A</td>
<td>N/A</td>
<td>Increased resource limits for the `heapster-nanny` container.</td>
</tr>
</tbody>
</table>

#### Changelog for worker node fix pack 1.10.5_1519, released 23 August 2018
{: #1105_1519}

The following table shows the changes that are included in the worker node fix pack 1.10.5_1519.
{: shortdesc}

<table summary="Changes that were made since version 1.10.5_1518">
<caption>Changes since version 1.10.5_1518</caption>
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
<td>`systemd`</td>
<td>229</td>
<td>230</td>
<td>Updated `systemd` to fix `cgroup` leak.</td>
</tr>
<tr>
<td>Kernel</td>
<td>4.4.0-127</td>
<td>4.4.0-133</td>
<td>Updated worker node images with kernel update for [CVE-2018-3620,CVE-2018-3646 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://usn.ubuntu.com/3741-1/).</td>
</tr>
</tbody>
</table>


#### Changelog for worker node fix pack 1.10.5_1518, released 13 August 2018
{: #1105_1518}

The following table shows the changes that are included in the worker node fix pack 1.10.5_1518.
{: shortdesc}

<table summary="Changes that were made since version 1.10.5_1517">
<caption>Changes since version 1.10.5_1517</caption>
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
<td>Ubuntu packages</td>
<td>N/A</td>
<td>N/A</td>
<td>Updates to installed Ubuntu packages.</td>
</tr>
</tbody>
</table>

#### Changelog for 1.10.5_1517, released 27 July 2018
{: #1105_1517}

The following table shows the changes that are included in patch 1.10.5_1517.
{: shortdesc}

<table summary="Changes that were made since version 1.10.3_1514">
<caption>Changes since version 1.10.3_1514</caption>
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
<td>Calico</td>
<td>v3.1.1</td>
<td>v3.1.3</td>
<td>See the Calico [release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://docs.projectcalico.org/v3.1/releases/#v313).</td>
</tr>
<tr>
<td>{{site.data.keyword.cloud_notm}} Provider</td>
<td>v1.10.3-85</td>
<td>v1.10.5-118</td>
<td>Updated to support Kubernetes 1.10.5 release. In addition, LoadBalancer service `create failure` events now include any portable subnet errors.</td>
</tr>
<tr>
<td>{{site.data.keyword.cloud_notm}} File Storage plug-in</td>
<td>320</td>
<td>334</td>
<td>Increased the timeout for persistent volume creation from 15 to 30 minutes. Changed the default billing type to `hourly`. Added mount options to the pre-defined storage classes. In the NFS file storage instance in your IBM Cloud infrastructure account, changed the **Notes** field to JSON format and added the Kubernetes namespace that the PV is deployed to. To support multizone clusters, added zone and region labels to persistent volumes.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.10.3</td>
<td>v1.10.5</td>
<td>See the Kubernetes [release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/kubernetes/kubernetes/releases/tag/v1.10.5).</td>
</tr>
<tr>
<td>Kernel</td>
<td>N/A</td>
<td>N/A</td>
<td>Minor improvements to worker node network settings for high performance networking workloads.</td>
</tr>
<tr>
<td>OpenVPN client</td>
<td>N/A</td>
<td>N/A</td>
<td>The OpenVPN client `vpn` deployment that runs in the `kube-system` namespace is now managed by the Kubernetes `addon-manager`.</td>
</tr>
</tbody>
</table>

#### Changelog for worker node fix pack 1.10.3_1514, released 3 July 2018
{: #1103_1514}

The following table shows the changes that are included in the worker node fix pack 1.10.3_1514.
{: shortdesc}

<table summary="Changes that were made since version 1.10.3_1513">
<caption>Changes since version 1.10.3_1513</caption>
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
<td>Kernel</td>
<td>N/A</td>
<td>N/A</td>
<td>Optimized `sysctl` for high performance networking workloads.</td>
</tr>
</tbody>
</table>


#### Changelog for worker node fix pack 1.10.3_1513, released 21 June 2018
{: #1103_1513}

The following table shows the changes that are included in the worker node fix pack 1.10.3_1513.
{: shortdesc}

<table summary="Changes that were made since version 1.10.3_1512">
<caption>Changes since version 1.10.3_1512</caption>
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
<td>Docker</td>
<td>N/A</td>
<td>N/A</td>
<td>For non-encrypted flavors, the secondary disk is cleaned by getting a fresh file system when you reload or update the worker node.</td>
</tr>
</tbody>
</table>

#### Changelog for 1.10.3_1512, released 12 June 2018
{: #1103_1512}

The following table shows the changes that are included in patch 1.10.3_1512.
{: shortdesc}

<table summary="Changes that were made since version 1.10.1_1510">
<caption>Changes since version 1.10.1_1510</caption>
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
<td>v1.10.1</td>
<td>v1.10.3</td>
<td>See the Kubernetes [release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/kubernetes/kubernetes/releases/tag/v1.10.3).</td>
</tr>
<tr>
<td>Kubernetes Configuration</td>
<td>N/A</td>
<td>N/A</td>
<td>Added `PodSecurityPolicy` to the `--enable-admission-plugins` option for the cluster's Kubernetes API server and configured the cluster to support pod security policies. For more information, see [Configuring pod security policies](/docs/containers?topic=containers-psp).</td>
</tr>
<tr>
<td>Kubelet Configuration</td>
<td>N/A</td>
<td>N/A</td>
<td>Enabled the `--authentication-token-webhook` option to support API bearer and service account tokens for authenticating to the `kubelet` HTTPS endpoint.</td>
</tr>
<tr>
<td>{{site.data.keyword.cloud_notm}} Provider</td>
<td>v1.10.1-52</td>
<td>v1.10.3-85</td>
<td>Updated to support Kubernetes 1.10.3 release.</td>
</tr>
<tr>
<td>OpenVPN client</td>
<td>N/A</td>
<td>N/A</td>
<td>Added `livenessProbe` to the OpenVPN client `vpn` deployment that runs in the `kube-system` namespace.</td>
</tr>
<tr>
<td>Kernel update</td>
<td>4.4.0-116</td>
<td>4.4.0-127</td>
<td>New worker node images with kernel update for [CVE-2018-3639 ![External link icon](../icons/launch-glyph.svg "External link icon")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-3639).</td>
</tr>
</tbody>
</table>



#### Changelog for worker node fix pack 1.10.1_1510, released 18 May 2018
{: #1101_1510}

The following table shows the changes that are included in the worker node fix pack 1.10.1_1510.
{: shortdesc}

<table summary="Changes that were made since version 1.10.1_1509">
<caption>Changes since version 1.10.1_1509</caption>
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
<td>Kubelet</td>
<td>N/A</td>
<td>N/A</td>
<td>Fix to address a bug that occurred if you used the block storage plug-in.</td>
</tr>
</tbody>
</table>

#### Changelog for worker node fix pack 1.10.1_1509, released 16 May 2018
{: #1101_1509}

The following table shows the changes that are included in the worker node fix pack 1.10.1_1509.
{: shortdesc}

<table summary="Changes that were made since version 1.10.1_1508">
<caption>Changes since version 1.10.1_1508</caption>
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
<td>Kubelet</td>
<td>N/A</td>
<td>N/A</td>
<td>The data that you store in the `kubelet` root directory is now saved on the larger, secondary disk of your worker node machine.</td>
</tr>
</tbody>
</table>

#### Changelog for 1.10.1_1508, released 01 May 2018
{: #1101_1508}

The following table shows the changes that are included in patch 1.10.1_1508.
{: shortdesc}

<table summary="Changes that were made since version 1.9.7_1510">
<caption>Changes since version 1.9.7_1510</caption>
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
<td>Calico</td>
<td>v2.6.5</td>
<td>v3.1.1</td>
<td>See the Calico [release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://docs.projectcalico.org/v3.1/releases/#v311).</td>
</tr>
<tr>
<td>Kubernetes Heapster</td>
<td>v1.5.0</td>
<td>v1.5.2</td>
<td>See the Kubernetes Heapster [release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/kubernetes/heapster/releases/tag/v1.5.2).</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.9.7</td>
<td>v1.10.1</td>
<td>See the Kubernetes [release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/kubernetes/kubernetes/releases/tag/v1.10.1).</td>
</tr>
<tr>
<td>Kubernetes Configuration</td>
<td>N/A</td>
<td>N/A</td>
<td>Added <code>StorageObjectInUseProtection</code> to the <code>--enable-admission-plugins</code> option for the cluster's Kubernetes API server.</td>
</tr>
<tr>
<td>Kubernetes DNS</td>
<td>1.14.8</td>
<td>1.14.10</td>
<td>See the Kubernetes DNS [release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/kubernetes/dns/releases/tag/1.14.10).</td>
</tr>
<tr>
<td>{{site.data.keyword.cloud_notm}} Provider</td>
<td>v1.9.7-102</td>
<td>v1.10.1-52</td>
<td>Updated to support Kubernetes 1.10 release.</td>
</tr>
<tr>
<td>GPU support</td>
<td>N/A</td>
<td>N/A</td>
<td>Support for [graphics processing unit (GPU) container workloads](/docs/containers?topic=containers-app#gpu_app) is now available for scheduling and execution. For a list of available GPU flavors, see [Hardware for worker nodes](/docs/containers?topic=containers-planning_worker_nodes#planning_worker_nodes). For more information, see the Kubernetes documentation to [Schedule GPUs ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/tasks/manage-gpus/scheduling-gpus/).</td>
</tr>
</tbody>
</table>

<br />


### Version 1.9 changelog (unsupported as of 27 December 2018)
{: #19_changelog}

Review the version 1.9 changelogs.
{: shortdesc}

*   [Changelog for worker node fix pack 1.9.11_1539, released 17 December 2018](#1911_1539)
*   [Changelog for worker node fix pack 1.9.11_1538, released 4 December 2018](#1911_1538)
*   [Changelog for worker node fix pack 1.9.11_1537, released 27 November 2018](#1911_1537)
*   [Changelog for 1.9.11_1536, released 19 November 2018](#1911_1536)
*   [Changelog for worker node fix 1.9.10_1532, released 7 November 2018](#1910_1532)
*   [Changelog for worker node fix pack 1.9.10_1531, released 26 October 2018](#1910_1531)
*   [Changelog for master fix pack 1.9.10_1530 released 15 October 2018](#1910_1530)
*   [Changelog for worker node fix pack 1.9.10_1528, released 10 October 2018](#1910_1528)
*   [Changelog for 1.9.10_1527, released 2 October 2018](#1910_1527)
*   [Changelog for worker node fix pack 1.9.10_1524, released 20 September 2018](#1910_1524)
*   [Changelog for 1.9.10_1523, released 4 September 2018](#1910_1523)
*   [Changelog for worker node fix pack 1.9.9_1522, released 23 August 2018](#199_1522)
*   [Changelog for worker node fix pack 1.9.9_1521, released 13 August 2018](#199_1521)
*   [Changelog for 1.9.9_1520, released 27 July 2018](#199_1520)
*   [Changelog for worker node fix pack 1.9.8_1517, released 3 July 2018](#198_1517)
*   [Changelog for worker node fix pack 1.9.8_1516, released 21 June 2018](#198_1516)
*   [Changelog for 1.9.8_1515, released 19 June 2018](#198_1515)
*   [Changelog for worker node fix pack 1.9.7_1513, released 11 June 2018](#197_1513)
*   [Changelog for worker node fix pack 1.9.7_1512, released 18 May 2018](#197_1512)
*   [Changelog for worker node fix pack 1.9.7_1511, released 16 May 2018](#197_1511)
*   [Changelog for 1.9.7_1510, released 30 April 2018](#197_1510)

#### Changelog for worker node fix pack 1.9.11_1539, released 17 December 2018
{: #1911_1539}

The following table shows the changes that are included in the worker node fix pack 1.9.11_1539.
{: shortdesc}

<table summary="Changes that were made since version 1.9.11_1538">
<caption>Changes since version 1.9.11_1538</caption>
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
<td>Ubuntu packages</td>
<td>N/A</td>
<td>N/A</td>
<td>Updates to installed Ubuntu packages.</td>
</tr>
</tbody>
</table>

#### Changelog for worker node fix pack 1.9.11_1538, released 4 December 2018
{: #1911_1538}

The following table shows the changes that are included in the worker node fix pack 1.9.11_1538.
{: shortdesc}

<table summary="Changes that were made since version 1.9.11_1537">
<caption>Changes since version 1.9.11_1537</caption>
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
<td>Worker node resource utilization</td>
<td>N/A</td>
<td>N/A</td>
<td>Added dedicated cgroups for the kubelet and docker to prevent these components from running out of resources. For more information, see [Worker node resource reserves](/docs/containers?topic=containers-planning_worker_nodes#resource_limit_node).</td>
</tr>
</tbody>
</table>

#### Changelog for worker node fix pack 1.9.11_1537, released 27 November 2018
{: #1911_1537}

The following table shows the changes that are included in the worker node fix pack 1.9.11_1537.
{: shortdesc}

<table summary="Changes that were made since version 1.9.11_1536">
<caption>Changes since version 1.9.11_1536</caption>
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
<td>Docker</td>
<td>17.06.2</td>
<td>18.06.1</td>
<td>See the [Docker release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://docs.docker.com/engine/release-notes/#18061-ce).</td>
</tr>
</tbody>
</table>

#### Changelog for 1.9.11_1536, released 19 November 2018
{: #1911_1536}

The following table shows the changes that are included in patch 1.9.11_1536.
{: shortdesc}

<table summary="Changes that were made since version 1.9.10_1532">
<caption>Changes since version 1.9.10_1532</caption>
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
<td>Calico</td>
<td>v2.6.5</td>
<td>v2.6.12</td>
<td>See the [Calico release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://docs.projectcalico.org/v2.6/releases/#v2612). Update resolves [Tigera Technical Advisory TTA-2018-001 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.projectcalico.org/security-bulletins/).</td>
</tr>
<tr>
<td>Kernel</td>
<td>4.4.0-137</td>
<td>4.4.0-139</td>
<td>Updated worker node images with kernel update for [CVE-2018-7755 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-139.165/changelog).</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.9.10</td>
<td>v1.9.11</td>
<td>See the [Kubernetes release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/kubernetes/kubernetes/releases/tag/v1.9.11).</td>
</tr>
<tr>
<td>{{site.data.keyword.cloud_notm}}</td>
<td>v1.9.10-219</td>
<td>v1.9.11-249</td>
<td>Updated to support the Kubernetes 1.9.11 release.</td>
</tr>
<tr>
<td>OpenVPN client and server</td>
<td>2.4.4-r2</td>
<td>2.4.6-r3-IKS-8</td>
<td>Updated image for [CVE-2018-0732 ![External link icon](../icons/launch-glyph.svg "External link icon")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0732) and [CVE-2018-0737 ![External link icon](../icons/launch-glyph.svg "External link icon")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-0737).</td>
</tr>
</tbody>
</table>

#### Changelog for worker node fix 1.9.10_1532, released 7 November 2018
{: #1910_1532}

The following table shows the changes that are included in the worker node fix pack 1.9.11_1532.
{: shortdesc}

<table summary="Changes that were made since version 1.9.10_1531">
<caption>Changes since version 1.9.10_1531</caption>
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
<td>TPM-enabled kernel</td>
<td>N/A</td>
<td>N/A</td>
<td>Bare metal worker nodes with TPM chips for Trusted Compute use the default Ubuntu kernel until trust is enabled. If you [enable trust](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_feature_enable) on an existing cluster, you need to [reload](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload) any existing bare metal worker nodes with TPM chips. To check if a bare metal worker node has a TPM chip, review the **Trustable** field after running the `ibmcloud ks flavors --zone` [command](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_machine_types).</td>
</tr>
</tbody>
</table>

#### Changelog for worker node fix pack 1.9.10_1531, released 26 October 2018
{: #1910_1531}

The following table shows the changes that are included in the worker node fix pack 1.9.10_1531.
{: shortdesc}

<table summary="Changes that were made since version 1.9.10_1530">
<caption>Changes since version 1.9.10_1530</caption>
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
<td>OS interrupt handling</td>
<td>N/A</td>
<td>N/A</td>
<td>Replaced the interrupt request (IRQ) system daemon with a more performant interrupt handler.</td>
</tr>
</tbody>
</table>

#### Changelog for master fix pack 1.9.10_1530 released 15 October 2018
{: #1910_1530}

The following table shows the changes that are included in the worker node fix pack 1.9.10_1530.
{: shortdesc}

<table summary="Changes that were made since version 1.9.10_1527">
<caption>Changes since version 1.9.10_1527</caption>
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
<td>Cluster update</td>
<td>N/A</td>
<td>N/A</td>
<td>Fixed problem with updating cluster add-ons when the master is updated from an unsupported version.</td>
</tr>
</tbody>
</table>

#### Changelog for worker node fix pack 1.9.10_1528, released 10 October 2018
{: #1910_1528}

The following table shows the changes that are included in the worker node fix pack 1.9.10_1528.
{: shortdesc}

<table summary="Changes that were made since version 1.9.10_1527">
<caption>Changes since version 1.9.10_1527</caption>
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
<td>Kernel</td>
<td>4.4.0-133</td>
<td>4.4.0-137</td>
<td>Updated worker node images with kernel update for [CVE-2018-14633, CVE-2018-17182 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://changelogs.ubuntu.com/changelogs/pool/main/l/linux/linux_4.4.0-137.163/changelog).</td>
</tr>
<tr>
<td>Inactive session timeout</td>
<td>N/A</td>
<td>N/A</td>
<td>Set the inactive session timeout to 5 minutes for compliance reasons.</td>
</tr>
</tbody>
</table>


#### Changelog for 1.9.10_1527, released 2 October 2018
{: #1910_1527}

The following table shows the changes that are included in patch 1.9.10_1527.
{: shortdesc}

<table summary="Changes that were made since version 1.9.10_1523">
<caption>Changes since version 1.9.10_1523</caption>
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
<td>{{site.data.keyword.cloud_notm}} Provider</td>
<td>v1.9.10-192</td>
<td>v1.9.10-219</td>
<td>Updated the documentation link in load balancer error messages.</td>
</tr>
<tr>
<td>IBM file storage classes</td>
<td>N/A</td>
<td>N/A</td>
<td>Removed `mountOptions` in the IBM file storage classes to use the default that is provided by the worker node. Removed duplicate `reclaimPolicy` parameter in the IBM file storage classes.<br><br>
Also, now when you update the cluster master, the default IBM file storage class remains unchanged. If you want to change the default storage class, run `kubectl patch storageclass <storageclass> -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'` and replace `<storageclass>` with the name of the storage class.</td>
</tr>
</tbody>
</table>

#### Changelog for worker node fix pack 1.9.10_1524, released 20 September 2018
{: #1910_1524}

The following table shows the changes that are included in the worker node fix pack 1.9.10_1524.
{: shortdesc}

<table summary="Changes that were made since version 1.9.10_1523">
<caption>Changes since version 1.9.10_1523</caption>
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
<td>Log rotate</td>
<td>N/A</td>
<td>N/A</td>
<td>Switched to use `systemd` timers instead of `cronjobs` to prevent `logrotate` from failing on worker nodes that are not reloaded or updated within 90 days. **Note**: In all earlier versions for this minor release, the primary disk fills up after the cron job fails because the logs are not rotated. The cron job fails after the worker node is active for 90 days without being updated or reloaded. If the logs fill up the entire primary disk, the worker node enters a failed state. The worker node can be fixed by using the `ibmcloud ks worker-reload` [command](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload) or the `ibmcloud ks worker-update` [command](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update).</td>
</tr>
<tr>
<td>Worker node runtime components (`kubelet`, `kube-proxy`, `docker`)</td>
<td>N/A</td>
<td>N/A</td>
<td>Removed dependencies of runtime components on the primary disk. This enhancement prevents worker nodes from failing when the primary disk is filled up.</td>
</tr>
<tr>
<td>Root password expiration</td>
<td>N/A</td>
<td>N/A</td>
<td>Root passwords for the worker nodes expire after 90 days for compliance reasons. If your automation tooling needs to log in to the worker node as root or relies on cron jobs that run as root, you can disable the password expiration by logging into the worker node and running `chage -M -1 root`. **Note**: If you have security compliance requirements that prevent running as root or removing password expiration, do not disable the expiration. Instead, you can [update](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update) or [reload](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload) your worker nodes at least every 90 days.</td>
</tr>
<tr>
<td>systemd</td>
<td>N/A</td>
<td>N/A</td>
<td>Periodically clean transient mount units to prevent them from becoming unbounded. This action addresses [Kubernetes issue 57345 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/kubernetes/kubernetes/issues/57345).</td>
</tr>
<tr>
<td>Docker</td>
<td>N/A</td>
<td>N/A</td>
<td>Disabled the default Docker bridge so that the `172.17.0.0/16` IP range is now used for private routes. If you rely on building Docker containers in worker nodes by executing `docker` commands on the host directly or by using a pod that mounts the Docker socket, choose from the following options.<ul><li>To ensure external network connectivity when you build the container, run `docker build . --network host`.</li>
<li>To explicitly create a network to use when you build the container, run `docker network create` and then use this network.</li></ul>
**Note**: Have dependencies on the Docker socket or Docker directly? Update to `containerd` instead of `docker` as the container runtime so that your clusters are prepared to run Kubernetes version 1.11 or later.</td>
</tr>
</tbody>
</table>

#### Changelog for 1.9.10_1523, released 4 September 2018
{: #1910_1523}

The following table shows the changes that are included in patch 1.9.10_1523.
{: shortdesc}

<table summary="Changes that were made since version 1.9.9_1522">
<caption>Changes since version 1.9.9_1522</caption>
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
<td>{{site.data.keyword.cloud_notm}} Provider</td>
<td>v1.9.9-167</td>
<td>v1.9.10-192</td>
<td>Updated to support Kubernetes 1.9.10 release. In addition, changed the cloud provider configuration to better handle updates for load balancer services with `externalTrafficPolicy` set to `local`.</td>
</tr>
<tr>
<td>{{site.data.keyword.cloud_notm}} File Storage plug-in</td>
<td>334</td>
<td>338</td>
<td>Updated incubator version to 1.8. File storage is provisioned to the specific zone that you select. You cannot update an existing (static) PV instance's labels, unless you are using a multizone cluster and need to add the region and zone labels.<br><br>Removed the default NFS version from the mount options in the IBM-provided file storage classes. The host's operating system now negotiates the NFS version with the IBM Cloud infrastructure NFS server. To manually set a specific NFS version, or to change the NFS version of your PV that was negotiated by the host's operating system, see [Changing the default NFS version](/docs/containers?topic=containers-file_storage#nfs_version_class).</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.9.9</td>
<td>v1.9.10</td>
<td>See the Kubernetes [release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/kubernetes/kubernetes/releases/tag/v1.9.10).</td>
</tr>
<tr>
<td>Kubernetes Heapster configuration</td>
<td>N/A</td>
<td>N/A</td>
<td>Increased resource limits for the `heapster-nanny` container.</td>
</tr>
</tbody>
</table>

#### Changelog for worker node fix pack 1.9.9_1522, released 23 August 2018
{: #199_1522}

The following table shows the changes that are included in the worker node fix pack 1.9.9_1522.
{: shortdesc}

<table summary="Changes that were made since version 1.9.9_1521">
<caption>Changes since version 1.9.9_1521</caption>
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
<td>`systemd`</td>
<td>229</td>
<td>230</td>
<td>Updated `systemd` to fix `cgroup` leak.</td>
</tr>
<tr>
<td>Kernel</td>
<td>4.4.0-127</td>
<td>4.4.0-133</td>
<td>Updated worker node images with kernel update for [CVE-2018-3620,CVE-2018-3646 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://usn.ubuntu.com/3741-1/).</td>
</tr>
</tbody>
</table>


#### Changelog for worker node fix pack 1.9.9_1521, released 13 August 2018
{: #199_1521}

The following table shows the changes that are included in the worker node fix pack 1.9.9_1521.
{: shortdesc}

<table summary="Changes that were made since version 1.9.9_1520">
<caption>Changes since version 1.9.9_1520</caption>
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
<td>Ubuntu packages</td>
<td>N/A</td>
<td>N/A</td>
<td>Updates to installed Ubuntu packages.</td>
</tr>
</tbody>
</table>

#### Changelog for 1.9.9_1520, released 27 July 2018
{: #199_1520}

The following table shows the changes that are included in patch 1.9.9_1520.
{: shortdesc}

<table summary="Changes that were made since version 1.9.8_1517">
<caption>Changes since version 1.9.8_1517</caption>
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
<td>{{site.data.keyword.cloud_notm}} Provider</td>
<td>v1.9.8-141</td>
<td>v1.9.9-167</td>
<td>Updated to support Kubernetes 1.9.9 release. In addition, LoadBalancer service `create failure` events now include any portable subnet errors.</td>
</tr>
<tr>
<td>{{site.data.keyword.cloud_notm}} File Storage plug-in</td>
<td>320</td>
<td>334</td>
<td>Increased the timeout for persistent volume creation from 15 to 30 minutes. Changed the default billing type to `hourly`. Added mount options to the pre-defined storage classes. In the NFS file storage instance in your IBM Cloud infrastructure account, changed the **Notes** field to JSON format and added the Kubernetes namespace that the PV is deployed to. To support multizone clusters, added zone and region labels to persistent volumes.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.9.8</td>
<td>v1.9.9</td>
<td>See the Kubernetes [release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/kubernetes/kubernetes/releases/tag/v1.9.9).</td>
</tr>
<tr>
<td>Kernel</td>
<td>N/A</td>
<td>N/A</td>
<td>Minor improvements to worker node network settings for high performance networking workloads.</td>
</tr>
<tr>
<td>OpenVPN client</td>
<td>N/A</td>
<td>N/A</td>
<td>The OpenVPN client `vpn` deployment that runs in the `kube-system` namespace is now managed by the Kubernetes `addon-manager`.</td>
</tr>
</tbody>
</table>

#### Changelog for worker node fix pack 1.9.8_1517, released 3 July 2018
{: #198_1517}

The following table shows the changes that are included in the worker node fix pack 1.9.8_1517.
{: shortdesc}

<table summary="Changes that were made since version 1.9.8_1516">
<caption>Changes since version 1.9.8_1516</caption>
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
<td>Kernel</td>
<td>N/A</td>
<td>N/A</td>
<td>Optimized `sysctl` for high performance networking workloads.</td>
</tr>
</tbody>
</table>


#### Changelog for worker node fix pack 1.9.8_1516, released 21 June 2018
{: #198_1516}

The following table shows the changes that are included in the worker node fix pack 1.9.8_1516.
{: shortdesc}

<table summary="Changes that were made since version 1.9.8_1515">
<caption>Changes since version 1.9.8_1515</caption>
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
<td>Docker</td>
<td>N/A</td>
<td>N/A</td>
<td>For non-encrypted flavors, the secondary disk is cleaned by getting a fresh file system when you reload or update the worker node.</td>
</tr>
</tbody>
</table>

#### Changelog for 1.9.8_1515, released 19 June 2018
{: #198_1515}

The following table shows the changes that are included in patch 1.9.8_1515.
{: shortdesc}

<table summary="Changes that were made since version 1.9.7_1513">
<caption>Changes since version 1.9.7_1513</caption>
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
<td>v1.9.7</td>
<td>v1.9.8</td>
<td>See the [Kubernetes release notes![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/kubernetes/kubernetes/releases/tag/v1.9.8).</td>
</tr>
<tr>
<td>Kubernetes Configuration</td>
<td>N/A</td>
<td>N/A</td>
<td>Added `PodSecurityPolicy` to the `--admission-control` option for the cluster's Kubernetes API server and configured the cluster to support pod security policies. For more information, see [Configuring pod security policies](/docs/containers?topic=containers-psp).</td>
</tr>
<tr>
<td>IBM Cloud Provider</td>
<td>v1.9.7-102</td>
<td>v1.9.8-141</td>
<td>Updated to support Kubernetes 1.9.8 release.</td>
</tr>
<tr>
<td>OpenVPN client</td>
<td>N/A</td>
<td>N/A</td>
<td>Added <code>livenessProbe</code> to the OpenVPN client <code>vpn</code> deployment that runs in the <code>kube-system</code> namespace.</td>
</tr>
</tbody>
</table>


#### Changelog for worker node fix pack 1.9.7_1513, released 11 June 2018
{: #197_1513}

The following table shows the changes that are included in the worker node fix pack 1.9.7_1513.
{: shortdesc}

<table summary="Changes that were made since version 1.9.7_1512">
<caption>Changes since version 1.9.7_1512</caption>
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
<td>Kernel update</td>
<td>4.4.0-116</td>
<td>4.4.0-127</td>
<td>New worker node images with kernel update for [CVE-2018-3639 ![External link icon](../icons/launch-glyph.svg "External link icon")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-3639).</td>
</tr>
</tbody>
</table>

#### Changelog for worker node fix pack 1.9.7_1512, released 18 May 2018
{: #197_1512}

The following table shows the changes that are included in the worker node fix pack 1.9.7_1512.
{: shortdesc}

<table summary="Changes that were made since version 1.9.7_1511">
<caption>Changes since version 1.9.7_1511</caption>
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
<td>Kubelet</td>
<td>N/A</td>
<td>N/A</td>
<td>Fix to address a bug that occurred if you used the block storage plug-in.</td>
</tr>
</tbody>
</table>

#### Changelog for worker node fix pack 1.9.7_1511, released 16 May 2018
{: #197_1511}

The following table shows the changes that are included in the worker node fix pack 1.9.7_1511.
{: shortdesc}

<table summary="Changes that were made since version 1.9.7_1510">
<caption>Changes since version 1.9.7_1510</caption>
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
<td>Kubelet</td>
<td>N/A</td>
<td>N/A</td>
<td>The data that you store in the `kubelet` root directory is now saved on the larger, secondary disk of your worker node machine.</td>
</tr>
</tbody>
</table>

#### Changelog for 1.9.7_1510, released 30 April 2018
{: #197_1510}

The following table shows the changes that are included in patch 1.9.7_1510.
{: shortdesc}

<table summary="Changes that were made since version 1.9.3_1506">
<caption>Changes since version 1.9.3_1506</caption>
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
<td>v1.9.3</td>
<td>v1.9.7	</td>
<td><p>See the [Kubernetes release notes![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/kubernetes/kubernetes/releases/tag/v1.9.7). This release addresses [CVE-2017-1002101 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002101) and [CVE-2017-1002102 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002102) vulnerabilities.</p><p><strong>Note</strong>: Now `secret`, `configMap`, `downwardAPI`, and projected volumes are mounted as read-only. Previously, apps could write data to these volumes, but the system could automatically revert the data. If your apps rely on the previous insecure behavior, modify them accordingly.</p></td>
</tr>
<tr>
<td>Kubernetes configuration</td>
<td>N/A</td>
<td>N/A</td>
<td>Added `admissionregistration.k8s.io/v1alpha1=true` to the `--runtime-config` option for the cluster's Kubernetes API server.</td>
</tr>
<tr>
<td>{{site.data.keyword.cloud_notm}} Provider</td>
<td>v1.9.3-71</td>
<td>v1.9.7-102</td>
<td>`NodePort` and `LoadBalancer` services now support [preserving the client source IP](/docs/containers?topic=containers-loadbalancer#node_affinity_tolerations) by setting `service.spec.externalTrafficPolicy` to `Local`.</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>Fix [edge node](/docs/containers?topic=containers-edge#edge) toleration setup for older clusters.</td>
</tr>
</tbody>
</table>

<br />


### Version 1.8 changelog (Unsupported)
{: #18_changelog}

Review the version 1.8 changelogs.
{: shortdesc}

*   [Changelog for worker node fix pack 1.8.15_1521, released 20 September 2018](#1815_1521)
*   [Changelog for worker node fix pack 1.8.15_1520, released 23 August 2018](#1815_1520)
*   [Changelog for worker node fix pack 1.8.15_1519, released 13 August 2018](#1815_1519)
*   [Changelog for 1.8.15_1518, released 27 July 2018](#1815_1518)
*   [Changelog for worker node fix pack 1.8.13_1516, released 3 July 2018](#1813_1516)
*   [Changelog for worker node fix pack 1.8.13_1515, released 21 June 2018](#1813_1515)
*   [Changelog 1.8.13_1514, released 19 June 2018](#1813_1514)
*   [Changelog for worker node fix pack 1.8.11_1512, released 11 June 2018](#1811_1512)
*   [Changelog for worker node fix pack 1.8.11_1511, released 18 May 2018](#1811_1511)
*   [Changelog for worker node fix pack 1.8.11_1510, released 16 May 2018](#1811_1510)
*   [Changelog for 1.8.11_1509, released 19 April 2018](#1811_1509)

#### Changelog for worker node fix pack 1.8.15_1521, released 20 September 2018
{: #1815_1521}

<table summary="Changes that were made since version 1.8.15_1520">
<caption>Changes since version 1.8.15_1520</caption>
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
<td>Log rotate</td>
<td>N/A</td>
<td>N/A</td>
<td>Switched to use `systemd` timers instead of `cronjobs` to prevent `logrotate` from failing on worker nodes that are not reloaded or updated within 90 days. **Note**: In all earlier versions for this minor release, the primary disk fills up after the cron job fails because the logs are not rotated. The cron job fails after the worker node is active for 90 days without being updated or reloaded. If the logs fill up the entire primary disk, the worker node enters a failed state. The worker node can be fixed by using the `ibmcloud ks worker-reload` [command](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload) or the `ibmcloud ks worker-update` [command](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update).</td>
</tr>
<tr>
<td>Worker node runtime components (`kubelet`, `kube-proxy`, `docker`)</td>
<td>N/A</td>
<td>N/A</td>
<td>Removed dependencies of runtime components on the primary disk. This enhancement prevents worker nodes from failing when the primary disk is filled up.</td>
</tr>
<tr>
<td>Root password expiration</td>
<td>N/A</td>
<td>N/A</td>
<td>Root passwords for the worker nodes expire after 90 days for compliance reasons. If your automation tooling needs to log in to the worker node as root or relies on cron jobs that run as root, you can disable the password expiration by logging into the worker node and running `chage -M -1 root`. **Note**: If you have security compliance requirements that prevent running as root or removing password expiration, do not disable the expiration. Instead, you can [update](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_update) or [reload](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload) your worker nodes at least every 90 days.</td>
</tr>
<tr>
<td>systemd</td>
<td>N/A</td>
<td>N/A</td>
<td>Periodically clean transient mount units to prevent them from becoming unbounded. This action addresses [Kubernetes issue 57345 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/kubernetes/kubernetes/issues/57345).</td>
</tr>
</tbody>
</table>

#### Changelog for worker node fix pack 1.8.15_1520, released 23 August 2018
{: #1815_1520}

<table summary="Changes that were made since version 1.8.15_1519">
<caption>Changes since version 1.8.15_1519</caption>
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
<td>`systemd`</td>
<td>229</td>
<td>230</td>
<td>Updated `systemd` to fix `cgroup` leak.</td>
</tr>
<tr>
<td>Kernel</td>
<td>4.4.0-127</td>
<td>4.4.0-133</td>
<td>Updated worker node images with kernel update for [CVE-2018-3620,CVE-2018-3646 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://usn.ubuntu.com/3741-1/).</td>
</tr>
</tbody>
</table>

#### Changelog for worker node fix pack 1.8.15_1519, released 13 August 2018
{: #1815_1519}

<table summary="Changes that were made since version 1.8.15_1518">
<caption>Changes since version 1.8.15_1518</caption>
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
<td>Ubuntu packages</td>
<td>N/A</td>
<td>N/A</td>
<td>Updates to installed Ubuntu packages.</td>
</tr>
</tbody>
</table>

#### Changelog for 1.8.15_1518, released 27 July 2018
{: #1815_1518}

<table summary="Changes that were made since version 1.8.13_1516">
<caption>Changes since version 1.8.13_1516</caption>
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
<td>{{site.data.keyword.cloud_notm}} Provider</td>
<td>v1.8.13-176</td>
<td>v1.8.15-204</td>
<td>Updated to support Kubernetes 1.8.15 release. In addition, LoadBalancer service `create failure` events now include any portable subnet errors.</td>
</tr>
<tr>
<td>{{site.data.keyword.cloud_notm}} File Storage plug-in</td>
<td>320</td>
<td>334</td>
<td>Increased the timeout for persistent volume creation from 15 to 30 minutes. Changed the default billing type to `hourly`. Added mount options to the pre-defined storage classes. In the NFS file storage instance in your IBM Cloud infrastructure account, changed the **Notes** field to JSON format and added the Kubernetes namespace that the PV is deployed to. To support multizone clusters, added zone and region labels to persistent volumes.</td>
</tr>
<tr>
<td>Kubernetes</td>
<td>v1.8.13</td>
<td>v1.8.15</td>
<td>See the Kubernetes [release notes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/kubernetes/kubernetes/releases/tag/v1.8.15).</td>
</tr>
<tr>
<td>Kernel</td>
<td>N/A</td>
<td>N/A</td>
<td>Minor improvements to worker node network settings for high performance networking workloads.</td>
</tr>
<tr>
<td>OpenVPN client</td>
<td>N/A</td>
<td>N/A</td>
<td>The OpenVPN client `vpn` deployment that runs in the `kube-system` namespace is now managed by the Kubernetes `addon-manager`.</td>
</tr>
</tbody>
</table>

#### Changelog for worker node fix pack 1.8.13_1516, released 3 July 2018
{: #1813_1516}

<table summary="Changes that were made since version 1.8.13_1515">
<caption>Changes since version 1.8.13_1515</caption>
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
<td>Kernel</td>
<td>N/A</td>
<td>N/A</td>
<td>Optimized `sysctl` for high performance networking workloads.</td>
</tr>
</tbody>
</table>


#### Changelog for worker node fix pack 1.8.13_1515, released 21 June 2018
{: #1813_1515}

<table summary="Changes that were made since version 1.8.13_1514">
<caption>Changes since version 1.8.13_1514</caption>
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
<td>Docker</td>
<td>N/A</td>
<td>N/A</td>
<td>For non-encrypted flavors, the secondary disk is cleaned by getting a fresh file system when you reload or update the worker node.</td>
</tr>
</tbody>
</table>

#### Changelog 1.8.13_1514, released 19 June 2018
{: #1813_1514}

<table summary="Changes that were made since version 1.8.11_1512">
<caption>Changes since version 1.8.11_1512</caption>
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
<td>v1.8.11</td>
<td>v1.8.13</td>
<td>See the [Kubernetes release notes![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/kubernetes/kubernetes/releases/tag/v1.8.13).</td>
</tr>
<tr>
<td>Kubernetes Configuration</td>
<td>N/A</td>
<td>N/A</td>
<td>Added `PodSecurityPolicy` to the `--admission-control` option for the cluster's Kubernetes API server and configured the cluster to support pod security policies. For more information, see [Configuring pod security policies](/docs/containers?topic=containers-psp).</td>
</tr>
<tr>
<td>IBM Cloud Provider</td>
<td>v1.8.11-126</td>
<td>v1.8.13-176</td>
<td>Updated to support Kubernetes 1.8.13 release.</td>
</tr>
<tr>
<td>OpenVPN client</td>
<td>N/A</td>
<td>N/A</td>
<td>Added <code>livenessProbe</code> to the OpenVPN client <code>vpn</code> deployment that runs in the <code>kube-system</code> namespace.</td>
</tr>
</tbody>
</table>


#### Changelog for worker node fix pack 1.8.11_1512, released 11 June 2018
{: #1811_1512}

<table summary="Changes that were made since version 1.8.11_1511">
<caption>Changes since version 1.8.11_1511</caption>
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
<td>Kernel update</td>
<td>4.4.0-116</td>
<td>4.4.0-127</td>
<td>New worker node images with kernel update for [CVE-2018-3639 ![External link icon](../icons/launch-glyph.svg "External link icon")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-3639).</td>
</tr>
</tbody>
</table>


#### Changelog for worker node fix pack 1.8.11_1511, released 18 May 2018
{: #1811_1511}

<table summary="Changes that were made since version 1.8.11_1510">
<caption>Changes since version 1.8.11_1510</caption>
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
<td>Kubelet</td>
<td>N/A</td>
<td>N/A</td>
<td>Fix to address a bug that occurred if you used the block storage plug-in.</td>
</tr>
</tbody>
</table>

#### Changelog for worker node fix pack 1.8.11_1510, released 16 May 2018
{: #1811_1510}

<table summary="Changes that were made since version 1.8.11_1509">
<caption>Changes since version 1.8.11_1509</caption>
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
<td>Kubelet</td>
<td>N/A</td>
<td>N/A</td>
<td>The data that you store in the `kubelet` root directory is now saved on the larger, secondary disk of your worker node machine.</td>
</tr>
</tbody>
</table>


#### Changelog for 1.8.11_1509, released 19 April 2018
{: #1811_1509}

<table summary="Changes that were made since version 1.8.8_1507">
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
<td><p>See the [Kubernetes release notes![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/kubernetes/kubernetes/releases/tag/v1.8.11). This release addresses [CVE-2017-1002101 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002101) and [CVE-2017-1002102 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002102) vulnerabilities.</p><p>Now `secret`, `configMap`, `downwardAPI`, and projected volumes are mounted as read-only. Previously, apps could write data to these volumes, but the system could automatically revert the data. If your apps rely on the previous insecure behavior, modify them accordingly.</p></td>
</tr>
<tr>
<td>Pause container image</td>
<td>3.0</td>
<td>3.1</td>
<td>Removes inherited orphaned zombie processes.</td>
</tr>
<tr>
<td>{{site.data.keyword.cloud_notm}} Provider</td>
<td>v1.8.8-86</td>
<td>v1.8.11-126</td>
<td>`NodePort` and `LoadBalancer` services now support [preserving the client source IP](/docs/containers?topic=containers-loadbalancer#node_affinity_tolerations) by setting `service.spec.externalTrafficPolicy` to `Local`.</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>Fix [edge node](/docs/containers?topic=containers-edge#edge) toleration setup for older clusters.</td>
</tr>
</tbody>
</table>

<br />


### Version 1.7 changelog (Unsupported)
{: #17_changelog}

Review the version 1.7 changelogs.
{: shortdesc}

*   [Changelog for worker node fix pack 1.7.16_1514, released 11 June 2018](#1716_1514)
*   [Changelog for worker node fix pack 1.7.16_1513, released 18 May 2018](#1716_1513)
*   [Changelog for worker node fix pack 1.7.16_1512, released 16 May 2018](#1716_1512)
*   [Changelog for 1.7.16_1511, released 19 April 2018](#1716_1511)

#### Changelog for worker node fix pack 1.7.16_1514, released 11 June 2018
{: #1716_1514}

<table summary="Changes that were made since version 1.7.16_1513">
<caption>Changes since version 1.7.16_1513</caption>
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
<td>Kernel update</td>
<td>4.4.0-116</td>
<td>4.4.0-127</td>
<td>New worker node images with kernel update for [CVE-2018-3639 ![External link icon](../icons/launch-glyph.svg "External link icon")](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-3639).</td>
</tr>
</tbody>
</table>

#### Changelog for worker node fix pack 1.7.16_1513, released 18 May 2018
{: #1716_1513}

<table summary="Changes that were made since version 1.7.16_1512">
<caption>Changes since version 1.7.16_1512</caption>
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
<td>Kubelet</td>
<td>N/A</td>
<td>N/A</td>
<td>Fix to address a bug that occurred if you used the block storage plug-in.</td>
</tr>
</tbody>
</table>

#### Changelog for worker node fix pack 1.7.16_1512, released 16 May 2018
{: #1716_1512}

<table summary="Changes that were made since version 1.7.16_1511">
<caption>Changes since version 1.7.16_1511</caption>
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
<td>Kubelet</td>
<td>N/A</td>
<td>N/A</td>
<td>The data that you store in the `kubelet` root directory is now saved on the larger, secondary disk of your worker node machine.</td>
</tr>
</tbody>
</table>

#### Changelog for 1.7.16_1511, released 19 April 2018
{: #1716_1511}

<table summary="Changes that were made since version 1.7.4_1509">
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
<td><p>See the [Kubernetes release notes![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/kubernetes/kubernetes/releases/tag/v1.7.16). This release addresses [CVE-2017-1002101 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002101) and [CVE-2017-1002102 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-1002102) vulnerabilities.</p><p>Now `secret`, `configMap`, `downwardAPI`, and projected volumes are mounted as read-only. Previously, apps could write data to these volumes, but the system could automatically revert the data. If your apps rely on the previous insecure behavior, modify them accordingly.</p></td>
</tr>
<td>{{site.data.keyword.cloud_notm}} Provider</td>
<td>v1.7.4-133</td>
<td>v1.7.16-17</td>
<td>`NodePort` and `LoadBalancer` services now support [preserving the client source IP](/docs/containers?topic=containers-loadbalancer#node_affinity_tolerations) by setting `service.spec.externalTrafficPolicy` to `Local`.</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>Fix [edge node](/docs/containers?topic=containers-edge#edge) toleration setup for older clusters.</td>
</tr>
</tbody>
</table>
