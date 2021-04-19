---

copyright:
  years: 2014, 2021
lastupdated: "2021-04-15"

keywords: kubernetes, iks, logmet, logs, metrics, recovery, auto-recovery

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


# Logging for clusters
{: #health}

Set up logging in {{site.data.keyword.containerlong}} to help you troubleshoot issues and improve the health and performance of your Kubernetes clusters and apps.
{: shortdesc}

Continuous monitoring and logging is the key to detecting attacks on your cluster and troubleshooting issues as they arise. By continuously monitoring your cluster, you're able to better understand your cluster capacity and the availability of resources that are available to your app. With this insight, you can prepare to protect your apps against downtime.

## Choosing a logging solution
{: #logging_overview}

By default, logs are generated and written locally for all of the following {{site.data.keyword.containerlong_notm}} cluster components: worker nodes, containers, applications, persistent storage, Ingress application load balancer, Kubernetes API, and the `kube-system` namespace. Several logging solutions are available to collect, forward, and view these logs.
{: shortdesc}

You can choose your logging solution based on which cluster components you need to collect logs for. A common implementation is to choose a logging service that you prefer based on its analysis and interface capabilities, such as {{site.data.keyword.la_full_notm}} or a third-party service. You can then use {{site.data.keyword.at_full_notm}} to audit user activity in the cluster and back up cluster master logs to {{site.data.keyword.cos_full_notm}}.

<dl>
<dt>{{site.data.keyword.la_full}}</dt>
<dd>Manage pod container logs by deploying an instance of {{site.data.keyword.la_full_notm}} and configuring this instance for your cluster in {{site.data.keyword.containershort_notm}}. A logging agent collects logs with the extension `*.log` and extensionless files that are stored in the `/var/log` directory of your pod from all namespaces, including `kube-system`. The agent then forwards the logs to your {{site.data.keyword.la_full_notm}} service instance. For more information about the service, see the [{{site.data.keyword.la_full_notm}}](/docs/log-analysis?topic=log-analysis-getting-started) documentation. To enable {{site.data.keyword.la_full_notm}} in your cluster, see [Forwarding cluster and app logs to {{site.data.keyword.la_full_notm}}](#logging).</dd>

<dt>{{site.data.keyword.at_full}}</dt>
<dd>To monitor user-initiated administrative activity made in your cluster, {{site.data.keyword.containershort_notm}} automatically generates cluster management events and forwards these event logs to {{site.data.keyword.at_full_notm}}. To access these logs, [provision an instance of {{site.data.keyword.at_full_notm}}](/docs/activity-tracker?topic=activity-tracker-getting-started). For more information about the types of {{site.data.keyword.containerlong_notm}} events that you can track, see [Activity Tracker events](/docs/containers?topic=containers-at_events).</dd>

<dt>Fluentd with an external server</dt>
<dd>To collect, forward, and view logs for a cluster component, you can create a logging configuration by using Fluentd. When you create a logging configuration, the [Fluentd ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.fluentd.org/) cluster component collects logs from the paths for a specified source. Fluentd can then forward these logs to an external server that accepts a syslog protocol. To get started, see [Understanding cluster and app log forwarding to syslog](#logging).
</dd>

<dt>{{site.data.keyword.cos_full_notm}}</dt>
<dd>To collect, forward, and view logs for your cluster's Kubernetes master, you can take a snapshot of your master logs at any point in time to collect in an {{site.data.keyword.cos_full_notm}} bucket. The snapshot includes anything that is sent through the API server, such as pod scheduling, deployments, or RBAC policies. To get started, see [Collecting master logs](#collect_master).</dd>

</dl>

<br />


## Forwarding cluster and app logs to {{site.data.keyword.la_full_notm}}
{: #logging}

Forward logs to an {{site.data.keyword.la_full_notm}} instance.
{: shortdesc}

Use the {{site.data.keyword.containerlong_notm}} observability plug-in to create a logging configuration for {{site.data.keyword.la_full_notm}} in your cluster, and use this logging configuration to automatically collect and forward pod logs to {{site.data.keyword.la_full_notm}}.
{: shortdesc}

Considerations for using the {{site.data.keyword.containerlong_notm}} observability plug-in:
* You can have only one logging configuration for {{site.data.keyword.la_full_notm}} in your cluster at a time. If you want to use a different {{site.data.keyword.la_full_notm}} service instance to send logs to, use the [`ibmcloud ob logging config replace`](/docs/containers?topic=containers-observability_cli#logging_config_replace) command.
* If you created a {{site.data.keyword.la_short}} configuration in your cluster without using the {{site.data.keyword.containerlong_notm}} observability plug-in, you can use the [`ibmcloud ob logging agent discover`](/docs/containers?topic=containers-observability_cli#logging_agent_discover) command to make the configuration visible to the plug-in. Then, you can use the observability plug-in commands and functionality in the {{site.data.keyword.cloud_notm}} console to manage the configuration.

Before you begin:
- Verify that you are assigned the **Editor** platform access role and **Manager** server access role for {{site.data.keyword.la_full_notm}}.
- Verify that you are assigned the **Administrator** platform access role and the **Manager** service access role for all Kubernetes namespaces in {{site.data.keyword.containerlong_notm}} to create the logging configuration. To view a logging configuration or launch the {{site.data.keyword.la_short}} dashboard after the logging configuration is created, users must be assigned the **Viewer** platform access role and **Reader** service access role for the `ibm-observe` Kubernetes namespace in {{site.data.keyword.containerlong_notm}}.
- If you want to use the CLI to set up the logging configuration:
  - [Install the {{site.data.keyword.containerlong_notm}} observability CLI plug-in (`ibmcloud ob`)](/docs/containers?topic=containers-cs_cli_install#cs_cli_install_steps).
  - [Log in to your account. If applicable, target the appropriate resource group. Set the context for your cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

To set up a logging configuration for your cluster:

1. Create an [{{site.data.keyword.la_full_notm}} service instance](/docs/log-analysis?topic=log-analysis-provision) and note the name of the instance. The service instance must belong to the same {{site.data.keyword.cloud_notm}} account where you created your cluster, but can be in a different resource group and {{site.data.keyword.cloud_notm}} region than your cluster.
2. Set up a logging configuration for your cluster. When you create the logging configuration, a Kubernetes namespace `ibm-observe` is created and a {{site.data.keyword.la_short}} agent is deployed as a daemon set to all worker nodes in your cluster. This agent collects logs with the extension `*.log` and extensionless files that are stored in the `/var/log` directory of your pod from all namespaces, including `kube-system`. The agent then forwards the logs to the {{site.data.keyword.la_full_notm}} service.

   - **From the console:**
     1. From the [{{site.data.keyword.containerlong_notm}} console](https://cloud.ibm.com/kubernetes/clusters){: external}, select the cluster for which you want to create a {{site.data.keyword.la_short}} configuration.
     2. On the cluster **Overview** page, click **Connect**.
     3. Select the region and the {{site.data.keyword.la_full_notm}} service instance that you created earlier, and click **Connect**.

   - **From the CLI:**
     1.  Create the {{site.data.keyword.la_short}} configuration. When you create the {{site.data.keyword.la_short}} configuration, the ingestion key that was last added is retrieved automatically. If you want to use a different ingestion key, add the `--logdna-ingestion-key <ingestion_key>` option to the command.

         To use a different ingestion key after you created your logging configuration, use the [`ibmcloud ob logging config replace`](/docs/containers?topic=containers-observability_cli#logging_config_replace) command.
         {: tip}

         ```
         ibmcloud ob logging config create --cluster <cluster_name_or_ID> --instance <Log_Analysis_instance_name_or_ID>
         ```
         {: pre}

         Example output:
         ```
         Creating configuration...
         OK
         ```
         {: screen}

     2. Verify that the logging configuration was added to your cluster.
        ```
        ibmcloud ob logging config list --cluster <cluster_name_or_ID>
        ```
        {: pre}

        Example output:
        ```
        Listing configurations...

        OK
        Instance Name                Instance ID                            CRN   
        IBM Cloud Log Analysis-opm   1a111a1a-1111-11a1-a1aa-aaa11111a11a   crn:v1:prod:public:logdna:us-south:a/a11111a1aaaaa11a111aa11a1aa1111a:1a111a1a-1111-11a1-a1aa-aaa11111a11a::  
        ```
        {: screen}

3. Optional: Verify that the {{site.data.keyword.la_short}} agent was set up successfully.
   1. If you used the console to create the {{site.data.keyword.la_short}} configuration, log in to your cluster. For more information, see [Log in to your account. If applicable, target the appropriate resource group. Set the context for your cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure).

   2. Verify that the daemon set for the {{site.data.keyword.la_short}} agent was created and all instances are listed as `AVAILABLE`.
      ```
      kubectl get daemonsets -n ibm-observe
      ```
      {: pre}

      Example output:
      ```
      NAME           DESIRED   CURRENT   READY   UP-TO-DATE   AVAILABLE   NODE SELECTOR   AGE
      logdna-agent   9         9         9       9            9           <none>          14m
      ```
      {: screen}

      The number of daemon set instances that are deployed equals the number of worker nodes in your cluster.

   3. Review the configmap that was created for your {{site.data.keyword.la_short}} agent.
      ```
      kubectl describe configmap -n ibm-observe
      ```
      {: pre}

4. Access the logs for your pods from the {{site.data.keyword.la_short}} dashboard.
   1. From the [{{site.data.keyword.containerlong_notm}} console](https://cloud.ibm.com/kubernetes/clusters){: external}, select the cluster that you configured.  
   2. On the cluster **Overview** page, click **Launch**. The {{site.data.keyword.la_short}} dashboard opens.
   3. Review the pod logs that the {{site.data.keyword.la_short}} agent collected from your cluster. It might take a few minutes for your first logs to show.

5. Review how you can [search and filter logs in the {{site.data.keyword.la_short}} dashboard](/docs/log-analysis?topic=log-analysis-view_logs).

<br />

## Forwarding cluster and app logs to an external server
{: #configuring}

Configure log forwarding for {{site.data.keyword.containerlong_notm}} standard clusters to an external server.
{: shortdesc}

### Understanding log forwarding to an external server
{: #logging-external}

When you create a logging configuration for a source in your cluster to forward to an external server, a [Fluentd](https://www.fluentd.org/){: external} component is created in your cluster. Fluentd collects the logs from that source's paths and forwards the logs to an external server. The traffic from the source to the logging service on the ingestion port is encrypted.
{: shortdesc}

As of 14 November 2019, a Fluentd component is created for your cluster only if you create a logging configuration to forward logs to a syslog server. If no logging configurations for syslog exist in your cluster, the Fluentd component is removed automatically. If you do not forward logs to syslog and want to ensure that the Fluentd component is removed from your cluster, [automatic updates to Fluentd must be enabled](/docs/containers?topic=containers-update#logging-up).
{: important}

**What are the sources that I can configure log forwarding for?**

In the following image, you can see the location of the sources that you can configure logging for.

<img src="images/log_sources.png" width="600" alt="Log sources in your cluster" style="width:600px; border-style: none"/>

1. `worker`: Information that is specific to the infrastructure configuration that you have for your worker node. Worker logs are captured in syslog and contain operating system events. In `auth.log` you can find information on the authentication requests that are made to the OS.</br>**Paths**:
    * `/var/log/syslog`
    * `/var/log/auth.log`

2. `container`: Information that is logged by a running container.</br>**Paths**: Anything that is written to `STDOUT` or `STDERR`.

3. `application`: Information about events that occur at the application level. This could be a notification that an event took place such as a successful login, a warning about storage, or other operations that can be performed at the app level.</br>**Paths**: You can set the paths that your logs are forwarded to. However, in order for logs to be sent, you must use an absolute path in your logging configuration or the logs cannot be read. If your path is mounted to your worker node, it might have created a symlink. Example: If the specified path is `/usr/local/spark/work/app-0546/0/stderr` but the logs actually go to `/usr/local/spark-1.0-hadoop-1.2/work/app-0546/0/stderr`, then the logs cannot be read.

4. `storage`: Information about persistent storage that is set up in your cluster. Storage logs can help you set up problem determination dashboards and alerts as part of your DevOps pipeline and production releases. **Note**: The paths `/var/log/kubelet.log` and `/var/log/syslog` also contain storage logs, but logs from these paths are collected by the `kubernetes` and `worker` log sources.</br>**Paths**:
    * `/var/log/ibmc-s3fs.log`
    * `/var/log/ibmc-block.log`

  **Pods**:
    * `portworx-***`
    * `ibmcloud-block-storage-attacher-***`
    * `ibmcloud-block-storage-driver-***`
    * `ibmcloud-block-storage-plugin-***`
    * `ibmcloud-object-storage-plugin-***`

5. `kubernetes`: Information from the kubelet, the kube-proxy, and other Kubernetes events that happen in the kube-system namespace of the worker node.</br>**Paths**:
    * `/var/log/kubelet.log`
    * `/var/log/kube-proxy.log`
    * `/var/log/event-exporter/1..log`

6. `kube-audit`: Information about cluster-related actions that is sent to the Kubernetes API server, including the time, the user, and the affected resource.

7. `ingress`: Information about the network traffic that comes into a cluster through the Ingress ALB.</br>**Paths**:
    * `/var/log/alb/ids/*.log`
    * `/var/log/alb/ids/*.err`
    * `/var/log/alb/customerlogs/*.log`
    * `/var/log/alb/customerlogs/*.err`


**Am I responsible for keeping Fluentd updated?**

In order to change your logging or filter configurations, the Fluentd logging component must be at the latest version. By default, automatic updates to the add-on are enabled. To disable automatic updates, see [Updating cluster components: Fluentd for logging](/docs/containers?topic=containers-update#logging-up).

**Can I forward some logs, but not others, from one source in my cluster?**

Yes. For example, if you have a particularly chatty pod, you might want to prevent logs from that pod from taking up log storage space, while still allowing other pods' logs to be forwarded. To prevent logs from a specific pod from being forwarded, see [Filtering logs](#filter-logs).

<br />

### Forwarding cluster and app logs
{: #enable-forwarding}

Create a configuration for cluster and app logging. You can differentiate between the different logging options by using flags.
{: shortdesc}

The following table shows the different options that you have when you configure logging and their descriptions.

<table summary="The columns are read from left to right. The first column has the parameter of the YAML file. The second column describes the parameter.">
<caption> Understanding logging configuration options</caption>
<col width="20%">
<thead>
<th>Parameter</th>
<th>Description</th>
  </thead>
  <tbody>
    <tr>
      <td><code><em>&lt;cluster_name_or_ID&gt;</em></code></td>
      <td>The name or ID of the cluster.</td>
    </tr>
    <tr>
      <td><code><em>--logsource</em></code></td>
      <td>The source that you want to forward logs from. Accepted values are <code>container</code>, <code>application</code>, <code>worker</code>, <code>kubernetes</code>, <code>ingress</code>, and <code>storage</code>. This argument supports a comma-separated list of log sources to apply to the configuration. If you do not provide a log source, logging configurations are created for <code>container</code> and <code>ingress</code> log sources.</td>
    </tr>
    <tr>
      <td><code><em>--type syslog</em></code></td>
      <td>The value <code>syslog</code> forwards your logs to an external server.</p>
      </dd></td>
    </tr>
    <tr>
      <td><code><em>--namespace</em></code></td>
      <td>Optional: The Kubernetes namespace that you want to forward logs from. Log forwarding is not supported for the <code>ibm-system</code> and <code>kube-system</code> Kubernetes namespaces. This value is valid only for the <code>container</code> log source. If you do not specify a namespace, then all namespaces in the cluster use this configuration.</td>
    </tr>
    <tr>
      <td><code><em>--hostname</em></code></td>
      <td>Specify the hostname or IP address of the log collector service.</td>
    </tr>
    <tr>
      <td><code><em>--port</em></code></td>
      <td>The ingestion port. If you do not specify a port, then the standard port <code>9091</code> is used.
      <p>For syslog, specify the port of the log collector server. If you do not specify a port, then the standard port <code>514</code> is used.</td>
    </tr>
    <tr>
      <td><code><em>--app-containers</em></code></td>
      <td>Optional: To forward logs from apps, you can specify the name of the container that contains your app. You can specify more than one container by using a comma-separated list. If no containers are specified, logs are forwarded from all of the containers that contain the paths that you provided.</td>
    </tr>
    <tr>
      <td><code><em>--app-paths</em></code></td>
      <td>The path on a container that the apps log to. To forward logs with source type <code>application</code>, you must provide a path. To specify more than one path, use a comma-separated list. Example: <code>/var/log/myApp1/*,/var/log/myApp2/*</code></td>
    </tr>
    <tr>
      <td><code><em>--syslog-protocol</em></code></td>
      <td>When the logging type is <code>syslog</code>, the transport layer protocol. You can use the following protocols: `udp`, `tls`, or `tcp`. When forwarding to a rsyslog server with the <code>udp</code> protocol, logs that are over 1KB are truncated.</td>
    </tr>
    <tr>
      <td><code><em>--ca-cert</em></code></td>
      <td>Required: When the logging type is <code>syslog</code> and the protocol is <code>tls</code>, the Kubernetes secret name that contains the certificate authority certificate.</td>
    </tr>
    <tr>
      <td><code><em>--verify-mode</em></code></td>
      <td>When the logging type is <code>syslog</code> and the protocol is <code>tls</code>, the verification mode. Supported values are <code>verify-peer</code> and the default <code>verify-none</code>.</td>
    </tr>
    <tr>
      <td><code><em>--skip-validation</em></code></td>
      <td>Optional: Skip the validation of the org and space names when they are specified. Skipping validation decreases processing time, but an invalid logging configuration will not correctly forward logs.</td>
    </tr>
  </tbody>
</table>

### Forwarding logs to your own server over the `udp` or `tcp` protocols
{: #enable-forwarding-udp-tcp}

1. Ensure that you have the [**Editor** or **Administrator** {{site.data.keyword.cloud_notm}} IAM platform access role](/docs/containers?topic=containers-users#platform).

2. For the cluster where the log source is located: [Log in to your account. If applicable, target the appropriate resource group. Set the context for your cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

3. Set up a server that accepts a syslog protocol in 1 of 2 ways:
  * Set up and manage your own server or have a provider manage it for you. If a provider manages the server for you, get the logging endpoint from the logging provider.

  * Run syslog from a container. For example, you can use this [deployment .yaml file](https://github.com/IBM-Cloud/kube-samples/blob/master/deploy-apps-clusters/deploy-syslog-from-kube.yaml){: external} to fetch a Docker public image that runs a container in your cluster. The image publishes the port `514` on the public cluster IP address, and uses this public cluster IP address to configure the syslog host.

  You can see your logs as valid JSON by removing syslog prefixes. To do so, add the following code to the top of your <code>etc/rsyslog.conf</code> file where your rsyslog server runs: <code>$template customFormat,"%msg%\n"</br>$ActionFileDefaultTemplate customFormat</code>
  {: tip}

4. Create a log forwarding configuration. For more information about the parameters, see the [Understanding logging configuration options table](#enable-forwarding).
    ```
    ibmcloud ks logging config create --cluster <cluster_name_or_ID> --logsource <log_source> --namespace <kubernetes_namespace> --hostname <log_server_hostname_or_IP> --port <log_server_port> --type syslog --app-containers <container1,2> --app-paths <paths_to_logs> --syslog-protocol <protocol> --skip-validation
    ```
    {: pre}

</br></br>

### Forwarding logs to your own server over the `tls` protocol
{: #enable-forwarding-tls}

The following steps are general instructions. Prior to using the container in a production environment, be sure that any security requirements that you need, are met.
{: tip}

1. Ensure that you have the following [{{site.data.keyword.cloud_notm}} IAM roles](/docs/containers?topic=containers-users#platform):
    * **Editor** or **Administrator** platform access role for the cluster
    * **Writer** or **Manager** service access role for the `kube-system` namespace

2. For the cluster where the log source is located: [Log in to your account. If applicable, target the appropriate resource group. Set the context for your cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

3. Set up a server that accepts a syslog protocol in 1 of 2 ways:
  * Set up and manage your own server or have a provider manage it for you. If a provider manages the server for you, get the logging endpoint from the logging provider.

  * Run syslog from a container. For example, you can use this [deployment .yaml file](https://github.com/IBM-Cloud/kube-samples/blob/master/deploy-apps-clusters/deploy-syslog-from-kube.yaml){: external} to fetch a Docker public image that runs a container in your cluster. The image publishes the port `514` on the public cluster IP address, and uses this public cluster IP address to configure the syslog host. You need to inject the relevant certificate authority and server-side certificates and update the `syslog.conf` to enable `tls` on your server.

4. Save your certificate authority certificate to a file named `ca-cert`. It must be that exact name.

5. Create a secret in the `kube-system` namespace for the `ca-cert` file. When you create your logging configuration, use the secret name for the `--ca-cert` flag.
    ```
    kubectl -n kube-system create secret generic --from-file=ca-cert
    ```
    {: pre}

6. Create a log forwarding configuration. For more information about the parameters, see the [Understanding logging configuration options table](#enable-forwarding).
    ```
    ibmcloud ks logging config create --cluster <cluster name or id> --logsource <log source> --type syslog --syslog-protocol tls --hostname <ip address of syslog server> --port <port for syslog server, 514 is default> --ca-cert <secret name> --verify-mode <defaults to verify-none>
    ```
    {: pre}

### Filtering logs that are forwarded
{: #filter-logs}

You can choose which logs to forward to your external server by filtering out specific logs for a period of time. You can differentiate between the different filtering options by using flags.
{: shortdesc}

<table summary="The columns are read from left to right. The first column has the parameter of the YAML file. The second column describes the parameter.">
<caption>Understanding the options for log filtering</caption>
<col width="25%">
<col width="75%">
<thead>
<th>Parameter</th>
<th>Description</th>
  </thead>
  <tbody>
    <tr>
      <td><code>&lt;cluster_name_or_ID&gt;</code></td>
      <td>Required: The name or ID of the cluster that you want to filter logs for.</td>
    </tr>
    <tr>
      <td><code>&lt;log_type&gt;</code></td>
      <td>The type of logs that you want to apply the filter to. Currently <code>all</code>, <code>container</code>, and <code>host</code> are supported.</td>
    </tr>
    <tr>
      <td><code>&lt;configs&gt;</code></td>
      <td>Optional: A comma-separated list of your logging configuration IDs. If not provided, the filter is applied to all of the cluster logging configurations that are passed to the filter. You can view log configurations that match the filter by using the <code>--show-matching-configs</code> option.</td>
    </tr>
    <tr>
      <td><code>&lt;kubernetes_namespace&gt;</code></td>
      <td>Optional: The Kubernetes namespace that you want to forward logs from. This flag applies only when you are using log type <code>container</code>.</td>
    </tr>
    <tr>
      <td><code>&lt;container_name&gt;</code></td>
      <td>Optional: The name of the container from which you want to filter logs.</td>
    </tr>
    <tr>
      <td><code>&lt;logging_level&gt;</code></td>
      <td>Optional: Filters out logs that are at the specified level and less. Acceptable values in their canonical order are <code>fatal</code>, <code>error</code>, <code>warn/warning</code>, <code>info</code>, <code>debug</code>, and <code>trace</code>. As an example, if you filtered logs at the <code>info</code> level, <code>debug</code>, and <code>trace</code> are also filtered. **Note**: You can use this flag only when log messages are in JSON format and contain a level field. To display your messages in JSON, append the <code>--output json</code> flag to the command.</td>
    </tr>
    <tr>
      <td><code>&lt;message&gt;</code></td>
      <td>Optional: Filters out logs that contain a specified message that is written as a regular expression.</td>
    </tr>
    <tr>
      <td><code>&lt;filter_ID&gt;</code></td>
      <td>Optional: The ID of the log filter.</td>
    </tr>
    <tr>
      <td><code>--show-matching-configs</code></td>
      <td>Optional: Show the logging configurations that each filter applies to.</td>
    </tr>
    <tr>
      <td><code>--all</code></td>
      <td>Optional: Delete all of your log forwarding filters.</td>
    </tr>
  </tbody>
</table>

1. Create a logging filter.
  ```
  ibmcloud ks logging filter create --cluster <cluster_name_or_ID> --type <log_type> --logging-configs <configs> --namespace <kubernetes_namespace> --container <container_name> --level <logging_level> --regex-message <message>
  ```
  {: pre}

2. View the log filter that you created.

  ```
  ibmcloud ks logging filter get --cluster <cluster_name_or_ID> --id <filter_ID> --show-matching-configs
  ```
  {: pre}

3. Update the log filter that you created.
  ```
  ibmcloud ks logging filter update --cluster <cluster_name_or_ID> --id <filter_ID> --type <server_type> --logging-configs <configs> --namespace <kubernetes_namespace --container <container_name> --level <logging_level> --regex-message <message>
  ```
  {: pre}

4. Delete a log filter that you created.

  ```
  ibmcloud ks logging filter rm --cluster <cluster_name_or_ID> --id <filter_ID> [--all]
  ```
  {: pre}

### Verifying, updating, and deleting log forwarding
{: #verifying-log-forwarding}

**Verifying**

You can verify that your configuration is set up correctly in 1 of 2 ways:

* To list all of the logging configurations in a cluster:
  ```
  ibmcloud ks logging config get --cluster <cluster_name_or_ID>
  ```
  {: pre}

* To list the logging configurations for one type of log source:
  ```
  ibmcloud ks logging config get --cluster <cluster_name_or_ID> --logsource <source>
  ```
  {: pre}

**Updating**

You can update a logging configuration that you already created:
```
ibmcloud ks logging config update --cluster <cluster_name_or_ID> --id <log_config_id> --namespace <namespace> --type <server_type> --syslog-protocol <protocol> --logsource <source> --hostname <hostname_or_ingestion_URL> --port <port> --space <cluster_space> --org <cluster_org> --app-containers <container1,2> --app-paths <paths_to_logs>
```
{: pre}

**Deleting**

You can stop forwarding logs by deleting one or all of the logging configurations for a cluster:

* To delete one logging configuration:
  ```
  ibmcloud ks logging config rm --cluster <cluster_name_or_ID> --id <log_config_ID>
  ```
  {: pre}

* To delete all of the logging configurations:
  ```
  ibmcloud ks logging config rm --cluster <my_cluster> --all
  ```
  {: pre}

<br />

## Collecting master logs in an {{site.data.keyword.cos_full_notm}} bucket
{: #collect_master}

With {{site.data.keyword.containerlong_notm}}, you can take a snapshot of your master logs at any point in time to collect in an {{site.data.keyword.cos_full_notm}} bucket. The snapshot includes anything that is sent through the API server, such as pod scheduling, deployments, or RBAC policies.
{: shortdesc}

Because Kubernetes API Server logs are automatically streamed, they're also automatically deleted to make room for the new logs coming in. By keeping a snapshot of logs at a specific point in time, you can better troubleshoot issues, look into usage differences, and find patterns to help maintain more secure applications.

**Before you begin**

* [Provision an instance](/docs/cloud-object-storage/basics?topic=cloud-object-storage-gs-dev) of {{site.data.keyword.cos_short}} from the {{site.data.keyword.cloud_notm}} catalog.
* Ensure that you have the [**Administrator** {{site.data.keyword.cloud_notm}} IAM platform access role](/docs/containers?topic=containers-users#platform) for the cluster.

**Creating a snapshot**

1. Create an Object Storage bucket through the {{site.data.keyword.cloud_notm}} console by following [this getting started tutorial](/docs/cloud-object-storage?topic=cloud-object-storage-getting-started-cloud-object-storage#gs-create-buckets).

2. Generate [HMAC service credentials](/docs/cloud-object-storage/iam?topic=cloud-object-storage-service-credentials) in the bucket that you created.
  1. In the **Service Credentials** tab of the {{site.data.keyword.cos_short}} dashboard, click **New Credential**.
  2. Give the HMAC credentials the `Writer` service access role.
  3. In the **Add Inline Configuration Parameters** field, specify `{"HMAC":true}`.

3. Through the CLI, make a request for a snapshot of your master logs.

  ```
  ibmcloud ks logging collect --cluster <cluster name or ID> --cos-bucket <COS_bucket_name> --cos-endpoint <location_of_COS_bucket> --hmac-key-id <HMAC_access_key_ID> --hmac-key <HMAC_access_key>
  ```
  {: pre}

  <table summary="The columns are read from left to right. The first column has the parameter of the YAML file. The second column describes the parameter.">
  <caption>Understanding this command's components</caption>
  <col width="30%">
  <thead>
  <th>Parameter</th>
  <th>Description</th>
    </thead>
    <tbody>
      <tr>
        <td><code>-c, --cluster <em>&lt;cluster_name_or_ID&gt;</em></code></td>
        <td>The name or ID of the cluster.</td>
      </tr>
      <tr>
        <td><code>--cos-bucket <em>&lt;COS_bucket_name&gt;</em></code></td>
        <td>The name of the {{site.data.keyword.cos_short}} bucket that you want to store your logs in.</td>
      </tr>
      <tr>
        <td><code>--cos-endpoint <em>&lt;location_of_COS_bucket&gt;</em></code></td>
        <td>The regional, cross regional, or single data center {{site.data.keyword.cos_short}} endpoint for the bucket that you are storing your logs in. For available endpoints, see [Endpoints and storage locations](/docs/cloud-object-storage/basics?topic=cloud-object-storage-endpoints) in the {{site.data.keyword.cos_short}} documentation.</td>
      </tr>
      <tr>
        <td><code>--hmac-key-id <em>&lt;HMAC_access_key_ID&gt;</em></code></td>
        <td>The unique ID for your HMAC credentials for your {{site.data.keyword.cos_short}} instance.</td>
      </tr>
      <tr>
        <td><code>--hmac-key <em>&lt;HMAC_access_key&gt;</em></code></td>
        <td>The HMAC key for your {{site.data.keyword.cos_short}} instance.</td>
      </tr>
    </tbody>
  </table>

  Example command and response:

  ```
  ibmcloud ks logging collect --cluster mycluster --cos-bucket mybucket --cos-endpoint s3-api.us-geo.objectstorage.softlayer.net --hmac-key-id e2e7f5c9fo0144563c418dlhi3545m86 --hmac-key c485b9b9fo4376722f692b63743e65e1705301ab051em96j
  There is no specified log type. The default master will be used.
  Submitting log collection request for master logs for cluster mycluster...
  OK
  The log collection request was successfully submitted. To view the status of the request run ibmcloud ks logging collect-status mycluster.
  ```
  {: screen}

4. Check the status of your request. It can take some time for the snapshot to complete, but you can check to see whether your request is successfully being completed or not. You can find the name of the file that contains your master logs in the response and use the {{site.data.keyword.cloud_notm}} console to download the file.

  ```
  ibmcloud ks logging collect-status --cluster <cluster_name_or_ID>
  ```
  {: pre}

  Example output:

  ```
  ibmcloud ks logging collect-status --cluster mycluster
  Getting the status of the last log collection request for cluster mycluster...
  OK
  State     Start Time             Error   Log URLs
  success   2018-09-18 16:49 PDT   - s3-api.us-geo.objectstorage.softlayer.net/mybucket/master-0-0862ae70a9ae6c19845ba3pc0a2a6o56-1297318756.tgz
  s3-api.us-geo.objectstorage.softlayer.net/mybucket/master-1-0862ae70a9ae6c19845ba3pc0a2a6o56-1297318756.tgz
  s3-api.us-geo.objectstorage.softlayer.net/mybucket/master-2-0862ae70a9ae6c19845ba3pc0a2a6o56-1297318756.tgz
  ```
  {: screen}
