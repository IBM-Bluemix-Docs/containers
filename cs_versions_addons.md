---

copyright:
  years: 2014, 2021
lastupdated: "2021-04-12"

keywords: kubernetes, iks, nginx, ingress controller, fluentd

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



# Ingress ALB and Fluentd version changelog
{: #cluster-add-ons-changelog}

Your {{site.data.keyword.containerlong}} cluster comes with components, such as the Fluentd and Ingress ALB components, that are updated automatically by IBM. You can also disable automatic updates for some components and manually update them separately from the master and worker nodes. Refer to the tables in the following sections for a summary of changes for each version.
{: shortdesc}

Check the [Security Bulletins on {{site.data.keyword.cloud_notm}} Status](https://cloud.ibm.com/status?component=containers-kubernetes&selected=security) for security vulnerabilities that affect {{site.data.keyword.containerlong_notm}}. You can filter the results to view only Kubernetes Cluster security bulletins that are relevant to {{site.data.keyword.containerlong_notm}}. {{site.data.keyword.containerlong_notm}}. Changelog entries that address other security vulnerabilities but do not also refer to an IBM Security Bulletin are for vulnerabilities that are not known to affect {{site.data.keyword.containerlong_notm}} in normal usage. If you run privileged containers, run commands on the workers, or execute untrusted code, then you might be at risk.

For more information about managing updates for Fluentd and Ingress ALBs, see [Updating cluster components](/docs/containers?topic=containers-update#components).

## Kubernetes Ingress image changelog
{: #kube_ingress_changelog}

View version changes for Ingress application load balancers (ALBs) that run the [community Kubernetes Ingress image](/docs/containers?topic=containers-ingress-types).
{: shortdesc}

When you create a new ALB, enable an ALB that was previously disabled, or manually update an ALB, you can specify an image version for your ALB in the `--version` flag. The latest three versions of the Kubernetes Ingress image are supported for ALBs. To list the currently supported versions, run the following command:
```
ibmcloud ks ingress alb versions
```
{: pre}

The Kubernetes Ingress version follows the format `<community_version>_<ibm_build>_iks`. The IBM build number indicates the most recent build of the Kubernetes Ingress NGINX release that {{site.data.keyword.containerlong_notm}} released. For example, the version `0.35.0_1155_iks` indicates the most recent build of the `0.35.0` Ingress NGINX version. {{site.data.keyword.containerlong_notm}} might release builds of the community image version to address vulnerabilities.

When automatic updates are enabled for ALBs, your ALBs are updated to the most recent build of the version that is marked as `default`. If you want to use a version other than the default, you must [disable automatic updates](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_autoupdate_disable). Typically, the latest version becomes the default version one month after the latest version is released by the Kubernetes community. Actual availability and release dates of versions are subject to change and depend on various factors, such as community updates, security patches, and technology changes between versions.

### Version 0.35.0 (default)
{: #0_35_0}

For the community changes for this version of the Kubernetes Ingress image, see the [community Kubernetes changelog for `ingress-nginx`](https://github.com/kubernetes/ingress-nginx/blob/master/Changelog.md#0350){: external}. Refer to the following table for a summary of changes for each build of version 0.35.0 of the Kubernetes Ingress image that {{site.data.keyword.containerlong_notm}} releases.
{: shortdesc}

|Version build|Release date|Changes|
|-------------|------------|-------|
|0.35.0_1155_iks|12 Apr 2021|Fixes OpenSSL vulnerabilities for [CVE-2021-3449](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-3449){: external} and [CVE-2021-3450](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-3450){: external}.|
| - |25 Mar 2021| In the `ibm-k8s-controller-config` configmap, sets the `server-tokens` field to `False` so that the NGINX version is not returned in response headers. |
|0.35.0_1094_iks|22 Mar 2021|Fixes `golang` vulnerabilities for [CVE-2021-3114](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-3114){: external} and [CVE-2021-3115](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-3115){: external}.|
|0.35.0_869_iks|07 Jan 2021|Fixes an Alpine vulnerability for [CVE-2020-28241](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-28241){: external}.|
|0.35.0_826_iks|17 Dec 2020|Fixes an Alpine vulnerability for [CVE-2020-1971](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-1971){: external}.|
|0.35.0_767_iks|14 Dec 2020|Updates the Go version to 1.15.5 for [CVE-2020-28362](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-28362){: external}, [CVE-2020-28367](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-28367){: external}, and [CVE-2020-28366](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-28366){: external}.|
|0.35.0_474_iks|07 Oct 2020|Fixes vulnerabilities for [CVE-2020-24977](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-24977){: external}, [CVE-2020-8169](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-8169){: external}, and [CVE-2020-8177](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-8177){: external}.|
{: summary="The rows are read from left to right. The first column is the build of the image version. The second column is the build release date. The third column contains a brief description of the change made in the version build."}
{: caption="Build changelog for version 0.35.0 of the Kubernetes Ingress image" caption-side="top"}

### Version 0.34.1
{: #0_34_1}

For the community changes for this version of the Kubernetes Ingress image, see the [community Kubernetes changelog for `ingress-nginx`](https://github.com/kubernetes/ingress-nginx/blob/master/Changelog.md#0341){: external}. Refer to the following table for a summary of changes for each build of version 0.34.1 of the Kubernetes Ingress image that {{site.data.keyword.containerlong_notm}} releases.
{: shortdesc}

|Version build|Release date|Changes|
|-------------|------------|-------|
|0.34.1_1153_iks|12 Apr 2021|Fixes OpenSSL vulnerabilities for [CVE-2021-3449](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-3449){: external} and [CVE-2021-3450](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-3450){: external}.|
| - |25 Mar 2021| In the `ibm-k8s-controller-config` configmap, sets the `server-tokens` field to `False` so that the NGINX version is not returned in response headers. |
|0.34.1_1096_iks|22 Mar 2021|Fixes `golang` vulnerabilities for [CVE-2021-3114](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-3114){: external} and [CVE-2021-3115](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-3115){: external}.|
|0.34.1_866_iks|07 Jan 2021|Fixes an Alpine vulnerability for [CVE-2020-28241](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-28241){: external}.|
|0.34.1_835_iks|17 Dec 2020|Fixes an Alpine vulnerability for [CVE-2020-1971](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-1971){: external}.|
|0.34.1_764_iks|14 Dec 2020|Updates the Go version to 1.15.5 for [CVE-2020-28362](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-28362){: external}, [CVE-2020-28367](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-28367){: external}, and [CVE-2020-28366](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-28366){: external}.|
|0.34.1_391_iks|24 Aug 2020|Fixes vulnerabilities for [CVE-2019-15847](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-15847){: external}.|
{: summary="The rows are read from left to right. The first column is the build of the image version. The second column is the build release date. The third column contains a brief description of the change made in the version build."}
{: caption="Build changelog for version 0.34.1 of the Kubernetes Ingress image" caption-side="top"}

### Version 0.33.0
{: #0_33_0}

For the community changes for this version of the Kubernetes Ingress image, see the [community changelog for `ingress-nginx`](https://github.com/kubernetes/ingress-nginx/blob/master/Changelog.md#0330){: external}. Refer to the following table for a summary of changes for each build of version 0.33.0 of the Kubernetes Ingress image that {{site.data.keyword.containerlong_notm}} releases.
{: shortdesc}

|Version build|Release date|Changes|
|-------------|------------|-------|
|0.33.0_1154_iks|12 Apr 2021|Fixes OpenSSL vulnerabilities for [CVE-2021-3449](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-3449){: external} and [CVE-2021-3450](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-3450){: external}.|
| - |25 Mar 2021| In the `ibm-k8s-controller-config` configmap, sets the `server-tokens` field to `False` so that the NGINX version is not returned in response headers. |
|0.33.0_1097_iks|22 Mar 2021|Fixes `golang` vulnerabilities for [CVE-2021-3114](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-3114){: external} and [CVE-2021-3115](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-3115){: external}.|
|0.33.0_865_iks|07 Jan 2021|Fixes an Alpine vulnerability for [CVE-2020-28241](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-28241){: external}.|
|0.33.0_834_iks|17 Dec 2020|Fixes an Alpine vulnerability for [CVE-2020-1971](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-1971){: external}.|
|0.33.0_768_iks|14 Dec 2020|Updates the Go version to 1.15.5 for [CVE-2020-28362](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-28362){: external}, [CVE-2020-28367](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-28367){: external}, and [CVE-2020-28366](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-28366){: external}.|
|0.33.0_390_iks|24 Aug 2020|Fixes vulnerabilities for [CVE-2019-15847](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-15847){: external}.|
{: summary="The rows are read from left to right. The first column is the build of the image version. The second column is the build release date. The third column contains a brief description of the change made in the version build."}
{: caption="Build changelog for version 0.33.0 of the Kubernetes Ingress image" caption-side="top"}

### Version 0.32.0
{: #0_32_0}

For the community changes for this version of the Kubernetes Ingress image, see the [community changelog for `ingress-nginx`](https://github.com/kubernetes/ingress-nginx/blob/master/Changelog.md#0320){: external}. Refer to the following table for a summary of changes for each build of version 0.32.0 of the Kubernetes Ingress image that {{site.data.keyword.containerlong_notm}} releases.
{: shortdesc}

|Version build|Release date|Changes|
|-------------|------------|-------|
| - |25 Mar 2021| In the `ibm-k8s-controller-config` configmap, sets the `server-tokens` field to `False` so that the NGINX version is not returned in response headers. |
|0.32.0_392_iks|24 Aug 2020|Fixes vulnerabilities for [CVE-2019-15847](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-15847){: external}.|
{: summary="The rows are read from left to right. The first column is the build of the image version. The second column is the build release date. The third column contains a brief description of the change made in the version build."}
{: caption="Build changelog for version 0.32.0 of the Kubernetes Ingress image" caption-side="top"}

<br />

## Deprecated: {{site.data.keyword.containerlong_notm}} Ingress image changelog
{: #alb_changelog}

View version changes for Ingress application load balancers (ALBs) that run the custom {{site.data.keyword.containerlong_notm}} Ingress image.
{: shortdesc}

As of 01 December 2020, the custom {{site.data.keyword.containerlong_notm}} Ingress image is deprecated and becomes unsupported 01 June 2021. To use the community Kubernetes implementation of Ingress, see [Setting up community Kubernetes Ingress](/docs/containers?topic=containers-ingress-types).
{: deprecated}

When the Ingress ALB component is updated, the `nginx-ingress` and `ingress-auth` containers in all ALB pods are updated to the latest image version. By default, automatic updates to the component are enabled, but you can disable automatic updates and manually update the component. For more information, see [Updating ALBs](/docs/containers?topic=containers-ingress#alb-update).

Refer to the following table for a summary of changes for each version of the {{site.data.keyword.containerlong_notm}} Ingress image.

<table summary="Version changes for the {{site.data.keyword.containerlong_notm}} Ingress image">
<caption>Changelog for the {{site.data.keyword.containerlong_notm}} Ingress image</caption>
<col width="12%">
<col width="12%">
<col width="41%">
<col width="35%">
<thead>
<tr>
<th>`nginx-ingress` / `ingress-auth` build</th>
<th>Release date</th>
<th>Non-disruptive changes</th>
<th>Disruptive changes</th>
</tr>
</thead>
<tbody>
<tr>
<td>2469 / 989</td>
<td>12 Apr 2021</td>
<td>Fixes OpenSSL vulnerabilities for [CVE-2021-3449 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-3449) and [CVE-2021-3450 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-3450).</td>
<td>-</td>
</tr>
<tr>
<td>2466 / 982</td>
<td>22 Mar 2021</td>
<td>Fixes `golang` vulnerabilities for [CVE-2021-3114 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-3114) and [CVE-2021-3115 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-3115).</td>
<td>-</td>
</tr>
<tr>
<td>2458 / 954</td>
<td>16 Mar 2021</td>
<td>Fixes `GNOME GLib` vulnerabilities for [CVE-2021-24031 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-24031) and [CVE-CVE-2021-24032 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-24032), and fixes `Zstandard` vulnerabilities for [CVE-2021-27218 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-27218) and [CVE-2021-27219 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-27219).</td>
<td>-</td>
</tr>
<tr>
<td>2452 / 954</td>
<td>09 Feb 2021</td>
<td>Updates `ca-certificates` for [USN-4719-1 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://ubuntu.com/security/notices/USN-4719-1/).</td>
<td>-</td>
</tr>
<tr>
<td>2424 / 954</td>
<td>19 Jan 2021</td>
<td>Fixes `p11-kit` vulnerabilities for [CVE-2020-29361 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-29361), [CVE-2020-29362 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-29362), and [CVE-2020-29363 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-29363).</td>
<td>-</td>
</tr>
<tr>
<td>2410 / 954</td>
<td>17 Dec 2020</td>
<td>Fixes an Alpine vulnerability for [CVE-2020-1971 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-1971).</td>
<td>-</td>
</tr>
<tr>
<td>2410 / 947</td>
<td>14 Dec 2020</td>
<td>General enhancements to code.</td>
<td>-</td>
</tr>
<tr>
<td>662 / 947</td>
<td>14 Dec 2020</td>
<td>Updates the `go` version to 1.15.5 for [CVE-2020-28362 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-28362), [CVE-2020-28367 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-28367), and [CVE-2020-28366 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-28366).</td>
<td>-</td>
</tr>
<tr>
<td>658 / 426</td>
<td>19 Nov 2020</td>
<td>Updates the `go` version for [CVE-2020-24553 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-24553).</td>
<td>-</td>
</tr>
<tr>
<td>652 / 424</td>
<td>01 Oct 2020</td>
<td>Fixes vulnerabilities for [CVE-2018-7738 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-7738), [CVE-2020-8169 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://curl.haxx.se/docs/CVE-2020-8169.html), and [CVE-2020-8177 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://curl.se/docs/CVE-2020-8177.html).</td>
<td>-</td>
</tr>
<tr>
<td>651 / 423</td>
<td>23 Sep 2020</td>
<td>Updates the `go` version to 1.13.15 for [CVE-2020-16845 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-16845).</td>
<td>-</td>
</tr>
<tr>
<td>647 / 421</td>
<td>05 Aug 2020</td>
<td>Updates the `go` version to 1.13.14 for [CVE-2020-15586 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-15586).</td>
<td>-</td>
</tr>
<tr>
<td>645 / 420</td>
<td>16 Jul 2020</td>
<td>Updates the `go` version to 1.13.7 for [CVE-2020-7919 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-7919).</td>
<td>-</td>
</tr>
<tr>
<td>642 / 413</td>
<td>24 Jun 2020</td>
<td>General enhancements to code.</td>
<td>-</td>
</tr>
<tr>
<td>637 / 413</td>
<td>18 Jun 2020</td>
<td>Fixes vulnerabilities for [CVE-2020-11080 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-11080).</td>
<td>-</td>
</tr>
<tr>
<td>637 / 401</td>
<td>04 Jun 2020</td>
<td>Fixes vulnerabilities for [CVE-2019-1547 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1547), [CVE-2019-1549 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1549), [CVE-2019-1551 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1551), [CVE-2019-1563 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1563), [USN-4376-1 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://ubuntu.com/security/notices/USN-4376-1/), and [USN-4377-1 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://ubuntu.com/security/notices/USN-4377-1/).</td>
<td>-</td>
</tr>
<tr>
<td>628 / 401</td>
<td>18 May 2020</td>
<td><ul><li>Fixes `libxml2` vulnerabilities for [CVE-2019-20388 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-20388).</li><li>Fixes `golang` vulnerabilities for [CVE-2020-9283 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-9283).</li><li>Fixes a vulnerability introduced by a `file` regression for [USN-3911-2 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://ubuntu.com/security/notices/USN-3911-2/).</li></ul></td>
<td>-</td>
</tr>
<tr>
<td>627 / 401</td>
<td>27 Apr 2020</td>
<td><ul><li>Adds the `SameSite=None` attribute to cookies to designate them for cross-site access.</li><li>Fixes OpenSSL vulnerabilities for [CVE-2020-1967 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-1967).</li></ul></td>
<td>-</td>
</tr>
<tr>
<td>627 / 394</td>
<td>16 Apr 2020</td>
<td>General enhancements to code.</td>
<td>-</td>
</tr>
<tr>
<td>627 / 390</td>
<td>25 Mar 2020</td>
<td>Changes permissions for log files to ensure that sensitive data is archived and protected.</td>
<td>-</td>
</tr>
<tr>
<td>625 / 390</td>
<td>03 Mar 2020</td>
<td>-</td>
<td>Adds secret caching for the [{{site.data.keyword.appid_short_notm}} service binding](/docs/containers?topic=containers-ingress_annotation#appid-auth) in your cluster. If you change the {{site.data.keyword.appid_short_notm}} service binding, the new {{site.data.keyword.appid_short_notm}} secret that is generated is not used by the ALB. You must restart your ALB pods to pick up the new secret. You can find the ALB pod names by running `kubectl get pods -n kube-system | grep alb` and restart the ALB pods by running `kubectl delete pod <pod> -n kube-system` for each pod.</p></td>
</tr>
<tr>
<td>625 / 373</td>
<td>30 Jan 2020</td>
<td><ul>
<li>Fixes a bug so that online certificate status protocol (OCSP) stapling works when the OCSP responder sends its own certificate in the OCSP response, which the NGINX validates.</li>
<li>Fixes SHA1 vulnerabilities in GnuTLS for [USN-4233-2 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://ubuntu.com/security/notices/USN-4233-2/).</li>
<li>Fixes `E2fsprogs` `e2fsck` vulnerabilities for [CVE-2019-5188 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5188).</li></ul>
</td>
<td>-</td>
</tr>
<tr>
<td>621 / 373</td>
<td>20 Jan 2020</td>
<td>Fixes SHA1 vulnerabilities in GnuTLS for [USN-4233-1 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://ubuntu.com/security/notices/USN-4233-1/) and error page HTTP request vulnerabilities for [CVE-2019-20372 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-20372).</td>
<td>-</td>
</tr>
<tr>
<td>615 / 373</td>
<td>06 Jan 2020</td>
<td>Fixes OpenSSL vulnerabilities for [CVE-2019-1551 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1551).</td>
<td>-</td>
</tr>
<tr>
<td>615 / 372</td>
<td>18 Dec 2019</td>
<td><ul>
<li>Fixes minor bugs for the [`app-id` annotation](/docs/containers?topic=containers-ingress_annotation#appid-auth).</li>
<li>Removes the dependency of the `ingress-auth` container on the Kubernetes API server. The container previously made a `getSecret` call to the API for every cookie generation. Now, it caches the secret data for the life of the container.</li></ul></td>
<td>-</td>
</tr>
<tr>
<td>597 / 353</td>
<td>21 Nov 2019</td>
<td><ul>
<li>Supports TLS 1.3 by default.</li>
<li>Updates the SSL cipher list to use `HIGH:!aNULL:!MD5:!CAMELLIA:!AESCCM:!ECDH+CHACHA20`. This updated cipher list is applied only to clusters that are created on or after 21 November 2019. Existing clusters continue to use the `HIGH:!aNULL:!MD5;` SSL cipher list. If you have an existing cluster, you can [update its `ssl-ciphers` list](/docs/containers?topic=containers-ingress_annotation#ssl_protocols_ciphers) to use `HIGH:!aNULL:!MD5:!CAMELLIA:!AESCCM:!ECDH+CHACHA20` instead.</li>
<li>Fixes a bug so that the [`client-max-body-size` annotation](/docs/containers?topic=containers-ingress_annotation#client-max-body-size) can now work in conjunction with the [`app-id` annotation](/docs/containers?topic=containers-ingress_annotation#appid-auth).</li>
<li>Fixes `golang` vulnerabilities for [CVE-2019-17596 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-17596).</li></ul></td>
<td>-</td>
</tr>
<tr>
<td>584 / 345</td>
<td>07 Nov 2019</td>
<td><ul>
<li>Fixes `libssh2` vulnerabilities for [CVE-2019-17498 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-17498).</li>
<li>Adds a liveness probe to the `nginx-ingress` container so that the container is restarted if it is not healthy. The ALB pod requires the `nginx-ingress` container resources to be available before the pod can be scheduled to a worker node.</li></ul></td>
<td>-</td>
</tr>
<tr>
<td>584 / 344</td>
<td>23 Oct 2019</td>
<td><ul>
<li>Adds the [`proxy-ssl-name` optional parameter to the `ssl-services` annotation](/docs/containers?topic=containers-ingress_annotation#ssl-services).</li>
<li>Fixes `golang` vulnerabilities for [CVE-2019-16276 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-16276).</li></ul></td>
<td>-</td>
</tr>
<tr>
<td>579 / 341</td>
<td>03 Oct 2019</td>
<td>Fixes `e2fsprogs` vulnerabilities for [CVE-2019-5094 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5094).</td>
<td>-</td>
</tr>
<tr>
<td>566 / 340</td>
<td>24 Sep 2019</td>
<td>In Kubernetes version 1.14 and later clusters, adds support for `apiVersion: networking.k8s.io/v1beta1` in Ingress resource YAML files. Note that `apiVersion: extensions/v1beta1` is still supported for all versions, and version 1.13 clusters must continue to use `apiVersion: extensions/v1beta1`.</td>
<td>-</td>
</tr>
<tr>
<td>552 / 340</td>
<td>12 Sep 2019</td>
<td><ul>
<li>Adds the [`proxy-ssl-verify-depth` optional parameter to the `ssl-services` annotation](/docs/containers?topic=containers-ingress_annotation#ssl-services).</li>
<li>Fixes a bug so that when traffic is routed to a host that does not exist, a default server now successfully returns a `404 Not Found` error message.</li>
</ul></td>
<td>-</td>
</tr>
<tr>
<td>524 / 340</td>
<td>05 Sep 2019</td>
<td>Fixes HTTP/2 vulnerabilities for [CVE-2019-9511 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9511) and [CVE-2019-9513 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9513).</td>
<td>-</td>
</tr>
<tr>
<td>524 / 337</td>
<td>26 Aug 2019</td>
<td><ul><li>Fixes a bug in the deployment for the readiness check for ALB pod restarts in some older images.</li>
<li>Fixes `golang` vulnerabilities for [CVE-2019-9512 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9512) and [CVE-2019-9514 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9514).</li></ul></td>
<td>Updates the deployment for Ingress ALBs. If you use edge nodes in your cluster, [redeploy your ALBs to your edge nodes (step 4)](/docs/containers?topic=containers-edge#edge_nodes).</td>
</tr>
<tr>
<td>519 / 335</td>
<td>20 Aug 2019</td>
<td>Fixes a bug so that [specifying custom ports](/docs/containers?topic=containers-ingress_annotation#custom-port) is now functional for VPC clusters.</td>
<td>-</td>
</tr>
<tr>
<td>515 / 335</td>
<td>12 Aug 2019</td>
<td>Fixes `musl libc` vulnerabilities for [CVE-2019-14697 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-14697).</td>
<td>-</td>
</tr>
<tr>
<td>515 / 334</td>
<td>30 Jul 2019</td>
<td><ul>
<li>Adds a readiness check for ALB pod restarts to prevent request loss. ALB pods are prevented from attempting to route traffic requests until all of the Ingress resource files are parsed, up to a default maximum of 5 minutes. For more information, including steps for changing the default timeout values, see [Increasing the restart readiness check time for ALB pods](/docs/containers?topic=containers-ingress#readiness-check).</li>
<li>Fixes GNU `patch` vulnerabilities for [CVE-2019-13636 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-13636) and [CVE-2019-13638 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-13638).</li>
</ul></td>
<td>-</td>
</tr>
<tr>
<td>512 / 334</td>
<td>17 Jul 2019</td>
<td><ul>
<li>Fixes `rbash` vulnerabilities for [CVE-2016-3189 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9924).</li>
<li>Removes the following apt packages from the `nginx-ingress` image: `curl`, `bash`, `vim`, `tcpdump`, and `ca-certificates`.</li></ul></td>
<td>-</td>
</tr>
<tr>
<td>497 / 334</td>
<td>14 Jul 2019</td>
<td><ul>
<li>Adds the [`upstream-keepalive-timeout`](/docs/containers?topic=containers-ingress_annotation#upstream-keepalive-timeout) to set the maximum time that a keepalive connection stays open between the ALB proxy server and the upstream server for your back-end app.</li>
<li>Adds support for the [`reuse-port`](/docs/containers?topic=containers-ingress_annotation#reuse-port) directive to increase the number of ALB socket listeners from one per cluster to one per worker node.</li>
<li>Removes the redundant update of the load balancer that exposes an ALB when a port number is changed.</li>
<li>Fixes `bzip2` vulnerabilities for [CVE-2016-3189 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2016-3189) and [CVE-2019-12900 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-12900).</li>
<li>Fixes Expat vulnerabilities for [CVE-2018-20843 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-20843).</li>
</ul></td>
<td>-</td>
</tr>
<tr>
<td>477 / 331</td>
<td>24 Jun 2019</td>
<td>Fixes SQLite vulnerabilities for [CVE-2016-6153 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2016-6153), [CVE-2017-10989 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-10989), [CVE-2017-13685 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-13685), [CVE-2017-2518 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-2518), [CVE-2017-2519 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-2519), [CVE-2017-2520 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-2520), [CVE-2018-20346 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-20346), [CVE-2018-20505 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-20505), [CVE-2018-20506 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-20506), [CVE-2019-8457 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8457), [CVE-2019-9936 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9936), and [CVE-2019-9937 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9937).
</td>
<td>-</td>
</tr>
<tr>
<td>473 / 331</td>
<td>18 Jun 2019</td>
<td><ul>
<li>Fixes Vim vulnerabilities for [CVE-2019-5953 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5953) and [CVE-2019-12735 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-12735).</li>
<li>Updates the NGINX version of ALBs to 1.15.12.</li></ul>
</td>
<td>-</td>
</tr>
<tr>
<td>470 / 330</td>
<td>07 Jun 2019</td>
<td>Fixes Berkeley DB vulnerabilities for [CVE-2019-8457 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8457).
</td>
<td>-</td>
</tr>
<tr>
<td>470 / 329</td>
<td>06 Jun 2019</td>
<td>Fixes Berkeley DB vulnerabilities for [CVE-2019-8457 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8457).
</td>
<td>-</td>
</tr>
<tr>
<td>467 / 329</td>
<td>03 Jun 2019</td>
<td>Fixes GnuTLS vulnerabilities for [CVE-2019-3829 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3829), [CVE-2019-3836 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3836), [CVE-2019-3893 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3893), [CVE-2018-10844 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10845), [CVE-2018-10845 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10844), and [CVE-2018-10846 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10846).
</td>
<td>-</td>
</tr>
<tr>
<td>462 / 329</td>
<td>28 May 2019</td>
<td>Fixes cURL vulnerabilities for [CVE-2019-5435 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5435) and [CVE-2019-5436 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5436).</td>
<td>-</td>
</tr>
<tr>
<td>457 / 329</td>
<td>23 May 2019</td>
<td>Fixes Go vulnerabilities for [CVE-2019-11841 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-11841).</td>
<td>-</td>
</tr>
<tr>
<td>423 / 329</td>
<td>13 May 2019</td>
<td>Improves performance for the integration with {{site.data.keyword.appid_full}}.</td>
<td>-</td>
</tr>
<tr>
<td>411 / 315</td>
<td>15 Apr 2019</td>
<td>Updates the value of the {{site.data.keyword.appid_full_notm}} cookie expiration so that it matches the value of the access token expiration.</td>
<td>-</td>
</tr>
<tr>
<td>411 / 306</td>
<td>22 Mar 2019</td>
<td>Updates the Go version to 1.12.1.</td>
<td>-</td>
</tr>
<tr>
<td>410 / 305</td>
<td>18 Mar 2019</td>
<td><ul>
<li>Fixes vulnerabilities for image scans.</li>
<li>Improves logging for the integration with {{site.data.keyword.appid_full_notm}}.</li>
</ul></td>
<td>-</td>
</tr>
<tr>
<td>408 / 304</td>
<td>05 Mar 2019</td>
<td>-</td>
<td>Fixes bugs in the authorization integration that is related to log out functionality, token expiration, and the `OAuth` authorization callback. These fixes are implemented only if you enabled {{site.data.keyword.appid_full_notm}} authorization by using the [`appid-auth`](/docs/containers?topic=containers-ingress_annotation#appid-auth) annotation. To implement these fixes, additional headers are added, which increases the total header size. Depending on the size of your own headers and the total size of responses, you might need to adjust any [proxy buffer annotations](/docs/containers?topic=containers-ingress_annotation#proxy-buffer) that you use.</td>
</tr>
<tr>
<td>406 / 301</td>
<td>19 Feb 2019</td>
<td><ul>
<li>Updates the Go version to 1.11.5.</li>
<li>Fixes vulnerabilities for image scans.</li>
</ul></td>
<td>-</td>
</tr>
<tr>
<td>404 / 300</td>
<td>31 Jan 2019</td>
<td>Updates the Go version to 1.11.4.</td>
<td>-</td>
</tr>
<tr>
<td>403 / 295</td>
<td>23 Jan 2019</td>
<td><ul>
<li>Updates the NGINX version of ALBs to 1.15.2.</li>
<li>IBM-provided TLS certificates are now automatically renewed 37 days before they expire instead of 7 days.</li>
<li>Adds {{site.data.keyword.appid_full_notm}} logout functionality: If the `/logout` prefix exists in an {{site.data.keyword.appid_full_notm}} path, cookies are removed and the user is sent back to the login page.</li>
<li>Adds a header to {{site.data.keyword.appid_full_notm}} requests for internal tracking purposes.</li>
<li>Updates the {{site.data.keyword.appid_short_notm}} location directive so that the `app-id` annotation can be used with the `proxy-buffers`, `proxy-buffer-size`, and `proxy-busy-buffer-size` annotations.</li>
<li>Fixes a bug so that informational logs are not labeled as errors.</li>
</ul></td>
<td>Disables TLS 1.0 and 1.1 by default. If the clients that connect to your apps support TLS 1.2, no action is required. If you still have legacy clients that require TLS 1.0 or 1.1 support, manually enable the required TLS versions by following [these steps](/docs/containers?topic=containers-ingress_annotation#ssl_protocols_ciphers). For more information about how to see the TLS versions that your clients use to access your apps, see this [{{site.data.keyword.cloud_notm}} Blog post](https://www.ibm.com/cloud/blog/ibm-cloud-kubernetes-service-alb-update-tls-1-0-and-1-1-disabled-by-default).</td>
</tr>
<tr>
<td>393 / 291</td>
<td>09 Jan 2019</td>
<td>Adds support for integration with multiple {{site.data.keyword.appid_full_notm}} instances.</td>
<td>-</td>
</tr>
<tr>
<td>393 / 282</td>
<td>29 Nov 2018</td>
<td>Improves performance for the integration with {{site.data.keyword.appid_full_notm}}.</td>
<td>-</td>
</tr>
<tr>
<td>384 / 246</td>
<td>14 Nov 2018</td>
<td>Improves logging and logout features for the integration with {{site.data.keyword.appid_full_notm}}.</td>
<td>Replaces the self-signed certificate for `*.containers.mybluemix.net` with the LetsEncrypt signed certificate that is automatically generated for and used by the cluster. The `*.containers.mybluemix.net` self-signed certificate is removed.</td>
</tr>
<tr>
<td>350 / 192</td>
<td>05 Nov 2018</td>
<td>Adds support for enabling and disabling automatic updates of the Ingress ALB component.</td>
<td>-</td>
</tr>
</tbody>
</table>

<br />

## Fluentd for logging changelog
{: #fluentd_changelog}

View image version changes for the Fluentd component for logging in your {{site.data.keyword.containerlong_notm}} clusters.
{: shortdesc}

As of 14 November 2019, a Fluentd component is created for your cluster only if you [create a logging configuration to forward logs to a syslog server](/docs/containers?topic=containers-health#configuring). If no logging configurations for syslog exist in your cluster, the Fluentd component is removed automatically. If you do not forward logs to syslog and want to ensure that the Fluentd component is removed from your cluster, [automatic updates to Fluentd must be enabled](/docs/containers?topic=containers-update#logging-up).
{: important}

Refer to the following table for a summary of changes for each build of the Fluentd component.

<table summary="Overview of build changes for the Fluentd component">
<caption>Changelog for the Fluentd component</caption>
<col width="12%">
<col width="12%">
<col width="41%">
<col width="35%">
<thead>
<tr>
<th>Fluentd build</th>
<th>Release date</th>
<th>Non-disruptive changes</th>
<th>Disruptive changes</th>
</tr>
</thead>
<tr>
<td>c7901bf0d1323806d44ce5f92bce5085f9b6c791</td>
<td>14 Nov 2019</td>
<td>-</td>
<td>The Fluentd component is created for your cluster only if you [create a logging configuration to forward logs to a syslog server](/docs/containers?topic=containers-health#configuring). If no logging configurations for syslog exist in your cluster, the Fluentd component is removed automatically. If you do not forward logs to syslog and want to ensure that the Fluentd component is removed from your cluster, [automatic updates to Fluentd must be enabled](/docs/containers?topic=containers-update#logging-up).</td>
</tr>
<tr>
<td>c7901bf0d1323806d44ce5f92bce5085f9b6c791</td>
<td>06 Nov 2019</td>
<td>Fixes `LibSass` vulnerabilities for [CVE-2018-19218 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-19218).</td>
<td>-</td>
</tr>
<tr>
<td>ee01ba3471cadbb9925269183acd724f4bf0e5bd</td>
<td>28 Oct 2019</td>
<td>Fixes Ruby vulnerabilities for [CVE-2019-15845](https://www.ruby-lang.org/en/news/2019/10/01/nul-injection-file-fnmatch-cve-2019-15845/), [CVE-2019-16201](https://www.ruby-lang.org/en/news/2019/10/01/webrick-regexp-digestauth-dos-cve-2019-16201/), [CVE-2019-16254](https://www.ruby-lang.org/en/news/2019/10/01/http-response-splitting-in-webrick-cve-2019-16254/), and [CVE-2019-16255](https://www.ruby-lang.org/en/news/2019/10/01/code-injection-shell-test-cve-2019-16255/).</td>
<td>-</td>
</tr>
<tr>
<td>58c604236080f142f35d14fe3b6c4b4484290121</td>
<td>24 Sep 2019</td>
<td>Fixes OpenSSL vulnerabilities for [CVE-2019-1547 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1547), [CVE-2019-1549 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1549), and [CVE-2019-1563 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-1563).</td>
<td>-</td>
</tr>
<tr>
<td>7c94e41a34ff1b7a56b9163471ff740a9585e053</td>
<td>18 Sep 2019</td>
<td>Updates the Kubernetes API version in the Fluentd deployment from `extensions/v1beta1` to `apps/v1`.</td>
<td>-</td>
</tr>
<tr>
<td>e7e944a8279deee0c3a8743e2fa69696ed71b6f5</td>
<td>15 Aug 2019</td>
<td>Fixes GNU binary utilities (`binutils`) vulnerabilities for [CVE-2018-6543 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-6543), [CVE-2018-6759 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-6759), [CVE-2018-6872 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-6872), [CVE-2018-7208 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-7208), [CVE-2018-7568 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-7568), [CVE-2018-7569 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-7569), [CVE-2018-7570 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-7570), [CVE-2018-7642 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-7642), [CVE-2018-7643 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-7643), and [CVE-2018-8945 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-8945).</td>
<td>-</td>
</tr>
<tr>
<td>d24b1afcc004ec9745dc3f9ef1328d3ed4b495f8</td>
<td>09 Aug 2019</td>
<td>Fixes `musl libc` vulnerabilities for [CVE-2019-14697 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-14697).</td>
<td>-</td>
</tr>
<tr>
<td>96f399cdea1c86c63a4ca4e043180f81f3559676</td>
<td>22 Jul 2019</td>
<td>Updates Alpine packages for [CVE-2019-8905 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8905), [CVE-2019-8906 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8906), and [CVE-2019-8907 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8907).</td>
<td>-</td>
</tr>
<tr>
<td>e7c10d74350dc64d4d92ba7f72bb4ff9219315d2</td>
<td>30 May 2019</td>
<td>Updates the Fluent config map to always ignore pod logs from IBM namespaces, even when the Kubernetes master is unavailable.</td>
<td>-</td>
</tr>
<tr>
<td>c16fe1602ab65db4af0a6ac008f99ca2a526e6f6</td>
<td>21 May 2019</td>
<td>Fixes a bug where worker node metrics did not display.</td>
<td>-</td>
</tr>
<tr>
<td>60fc11f7bd39d9c6cfed923c598bf6457b3f2037</td>
<td>10 May 2019</td>
<td>Updates Ruby packages for [CVE-2019-8320 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8320), [CVE-2019-8321 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8321), [CVE-2019-8322 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8322), [CVE-2019-8323 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8323), [CVE-2019-8324 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8324), and [CVE-2019-8325 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8325).</td>
<td>-</td>
</tr>
<tr>
<td>91a737f68f7d9e81b5d2223c910aaa7d7f91b76d</td>
<td>08 May 2019</td>
<td>Updates Ruby packages for [CVE-2019-8320 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8320), [CVE-2019-8321 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8321), [CVE-2019-8322 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8322), [CVE-2019-8323 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8323), [CVE-2019-8324 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8324), and [CVE-2019-8325 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8325).</td>
<td>-</td>
</tr>
<tr>
<td>d9af69e286986a05ed4a50469585b1cf978ddb1d</td>
<td>11 Apr 2019</td>
<td>Updates the cAdvisor plug-in to use TLS 1.2.</td>
<td>-</td>
</tr>
<tr>
<td>3100ddb62580a9f46ffdff7bab2ebec40b164de6</td>
<td>01 Apr 2019</td>
<td>Updates the Fluentd service account.</td>
<td>-</td>
</tr>
<tr>
<td>c85567b75bd7ad1c9428794cd63a8e239c3fd8f5</td>
<td>18 Mar 2019</td>
<td>Removes the dependency on cURL for [CVE-2019-8323 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8323).</td>
<td>-</td>
</tr>
<tr>
<td>320ffdf87de068ee2f7f34c0e7a47a111e8d457b</td>
<td>18 Feb 2019</td>
<td><ul>
<li>Updates Fluentd to version 1.3.</li>
<li>Removes Git from the Fluentd image for [CVE-2018-19486 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-19486).</li>
</ul></td>
<td>-</td>
</tr>
<tr>
<td>972865196aefd3324105087878de12c518ed579f</td>
<td>01 Jan 2019</td>
<td><ul>
<li>Enables UTF-8 encoding for the Fluentd `in_tail` plug-in.</li>
<li>Minor bug fixes.</li>
</ul></td>
<td>-</td>
</tr>
</tbody>
</table>
