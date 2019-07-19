---

copyright:
  years: 2014, 2019
lastupdated: "2019-07-15"

keywords: kubernetes, iks, nginx, ingress controller

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



# Fluentd and Ingress ALB changelog
{: #cluster-add-ons-changelog}

Your {{site.data.keyword.containerlong}} cluster comes with components, such as the Fluentd and Ingress ALB components, that are updated automatically by IBM. You can also disable automatic updates for some components and manually update them separately from the master and worker nodes. Refer to the tables in the following sections for a summary of changes for each version.
{: shortdesc}

For more information about managing updates for Fluentd and Ingress ALBs, see [Updating cluster components](/docs/containers?topic=containers-update#components).

## Ingress ALBs changelog
{: #alb_changelog}

View build version changes for Ingress application load balancers (ALBs) in your {{site.data.keyword.containerlong_notm}} clusters.
{:shortdesc}

When the Ingress ALB component is updated, the `nginx-ingress` and `ingress-auth` containers in all ALB pods are updated to the latest build version. By default, automatic updates to the component are enabled, but you can disable automatic updates and manually update the component. For more information, see [Updating the Ingress application load balancer](/docs/containers?topic=containers-update#alb).

Refer to the following table for a summary of changes for each build of the Ingress ALB component.

<table summary="Overview of build changes for the Ingress application load balancer component">
<caption>Changelog for the Ingress application load balancer component</caption>
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
<td>497 / 331</td>
<td>14 July 2019</td>
<td><ul>
<li>Adds the [`upstream-keepalive-timeout`](/docs/containers?topic=containers-ingress_annotation#upstream-keepalive-timeout) to set the maximum time that a keepalive connection stays open between the ALB proxy server and the upstream server for your back-end app.</li>
<li>Adds support for the [`reuse-port`](/docs/containers?topic=containers-ingress-settings#reuse-port) directive to increase the number of ALB socket listeners from one per cluster to one per worker node.</li>
<li>Removes the redundant update of the load balancer that exposes an ALB when a port number is changed.</li>
<li>Fixes `bzip2` vulnerabilities for [CVE-2016-3189 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2016-3189) and [CVE-2019-12900 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-12900).</li>
<li>Fixes Expat vulnerabilities for [CVE-2018-20843 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-20843).</li>
</ul></td>
<td>-</td>
</tr>
<tr>
<td>477 / 331</td>
<td>24 June 2019</td>
<td>Fixes SQLite vulnerabilities for [CVE-2016-6153 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2016-6153), [CVE-2017-10989 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-10989), [CVE-2017-13685 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-13685), [CVE-2017-2518 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-2518), [CVE-2017-2519 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-2519), [CVE-2017-2520 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-2520), [CVE-2018-20346 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-20346), [CVE-2018-20505 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-20505), [CVE-2018-20506 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-20506), [CVE-2019-8457 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8457), [CVE-2019-9936 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9936), and [CVE-2019-9937 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9937).
</td>
<td>-</td>
</tr>
<tr>
<td>473 / 331</td>
<td>18 June 2019</td>
<td><ul>
<li>Fixes Vim vulnerabilities for [CVE-2019-5953 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5953) and [CVE-2019-12735 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-12735).</li>
<li>Updates the NGINX version of ALBs to 1.15.12.</li></ul>
</td>
<td>-</td>
</tr>
<tr>
<td>470 / 330</td>
<td>07 June 2019</td>
<td>Fixes Berkeley DB vulnerabilities for [CVE-2019-8457 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8457).
</td>
<td>-</td>
</tr>
<tr>
<td>470 / 329</td>
<td>06 June 2019</td>
<td>Fixes Berkeley DB vulnerabilities for [CVE-2019-8457 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8457).
</td>
<td>-</td>
</tr>
<tr>
<td>467 / 329</td>
<td>03 June 2019</td>
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
<td>Disables TLS 1.0 and 1.1 by default. If the clients that connect to your apps support TLS 1.2, no action is required. If you still have legacy clients that require TLS 1.0 or 1.1 support, manually enable the required TLS versions by following [these steps](/docs/containers?topic=containers-ingress-settings#ssl_protocols_ciphers). For more information about how to see the TLS versions that your clients use to access your apps, see this [{{site.data.keyword.cloud_notm}} Blog post](https://www.ibm.com/blogs/bluemix/2018/11/ibm-cloud-kubernetes-service-alb-update-tls-1-0-and-1-1-disabled-by-default/).</td>
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

## Fluentd for logging changelog
{: #fluentd_changelog}

View build version changes for the Fluentd component for logging in your {{site.data.keyword.containerlong_notm}} clusters.
{:shortdesc}

By default, automatic updates to the component are enabled, but you can disable automatic updates and manually update the component. For more information, see [Managing automatic updates for Fluentd](/docs/containers?topic=containers-update#logging-up).

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
<li>Updates Fluend to version 1.3.</li>
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
