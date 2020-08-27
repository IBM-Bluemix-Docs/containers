---

copyright:
  years: 2014, 2020
lastupdated: "2020-08-24"

keywords: kubernetes, iks, logmet, logs, metrics

subcollection: containers

---

{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}
{:android: data-hd-operatingsystem="android"}
{:apikey: data-credential-placeholder='apikey'}
{:app_key: data-hd-keyref="app_key"}
{:app_name: data-hd-keyref="app_name"}
{:app_secret: data-hd-keyref="app_secret"}
{:app_url: data-hd-keyref="app_url"}
{:authenticated-content: .authenticated-content}
{:beta: .beta}
{:c#: data-hd-programlang="c#"}
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
{:java: #java .ph data-hd-programlang='java'}
{:java: .ph data-hd-programlang='java'}
{:java: data-hd-programlang="java"}
{:javascript: .ph data-hd-programlang='javascript'}
{:javascript: data-hd-programlang="javascript"}
{:new_window: target="_blank"}
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
{:swift: #swift .ph data-hd-programlang='swift'}
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
{:unity: .ph data-hd-programlang='unity'}
{:url: data-credential-placeholder='url'}
{:user_ID: data-hd-keyref="user_ID"}
{:vb.net: .ph data-hd-programlang='vb.net'}
{:video: .video}




# Logging and monitoring cluster health
{: #health}

Set up logging and monitoring in {{site.data.keyword.containerlong}} to help you troubleshoot issues and improve the health and performance of your Kubernetes clusters and apps.
{: shortdesc}

Continuous monitoring and logging is the key to detecting attacks on your cluster and troubleshooting issues as they arise. By continuously monitoring your cluster, you're able to better understand your cluster capacity and the availability of resources that are available to your app. With this insight, you can prepare to protect your apps against downtime.

## Choosing a logging solution
{: #logging_overview}

By default, logs are generated and written locally for all of the following {{site.data.keyword.containerlong_notm}} cluster components: worker nodes, containers, applications, persistent storage, Ingress application load balancer, Kubernetes API, and the `kube-system` namespace. Several logging solutions are available to collect, forward, and view these logs.
{: shortdesc}

You can choose your logging solution based on which cluster components you need to collect logs for. A common implementation is to choose a logging service that you prefer based on its analysis and interface capabilities, such as {{site.data.keyword.la_full_notm}} or a third-party service. You can then use {{site.data.keyword.at_full_notm}} to audit user activity in the cluster and back up cluster master logs to {{site.data.keyword.cos_full_notm}}.

<dl>
<dt>{{site.data.keyword.la_full}}</dt>
<dd>Manage pod container logs by deploying an instance of {{site.data.keyword.la_full_notm}} and configuring this instance for your cluster in {{site.data.keyword.containershort_notm}}. A logging agent collects logs with the extension `*.log` and extensionless files that are stored in the `/var/log` directory of your pod from all namespaces, including `kube-system`. The agent then forwards the logs to your {{site.data.keyword.la_full_notm}} service instance. For more information about the service, see the [{{site.data.keyword.la_full_notm}}](/docs/Log-Analysis-with-LogDNA?topic=Log-Analysis-with-LogDNA-getting-started) documentation. To enable {{site.data.keyword.la_full_notm}} in your cluster, see [Creating a logging configuration to forward cluster and app logs to {{site.data.keyword.la_full_notm}}](#app_logdna).</dd>

<dt>{{site.data.keyword.at_full}}</dt>
<dd>To monitor user-initiated administrative activity made in your cluster, {{site.data.keyword.containershort_notm}} automatically generates cluster management events and forwards these event logs to {{site.data.keyword.at_full_notm}}. To access these logs, [provision an instance of {{site.data.keyword.at_full_notm}}](/docs/Activity-Tracker-with-LogDNA?topic=Activity-Tracker-with-LogDNA-getting-started). For more information about the types of {{site.data.keyword.containerlong_notm}} events that you can track, see [Activity Tracker events](/docs/containers?topic=containers-at_events).</dd>

<dt>Fluentd with an external server</dt>
<dd>To collect, forward, and view logs for a cluster component, you can create a logging configuration by using Fluentd. When you create a logging configuration, the [Fluentd ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.fluentd.org/) cluster component collects logs from the paths for a specified source. Fluentd can then forward these logs to an external server that accepts a syslog protocol. To get started, see [Understanding cluster and app log forwarding to syslog](#logging).
</dd>

<dt>{{site.data.keyword.cos_full_notm}}</dt>
<dd>To collect, forward, and view logs for your cluster's Kubernetes master, you can take a snapshot of your master logs at any point in time to collect in an {{site.data.keyword.cos_full_notm}} bucket. The snapshot includes anything that is sent through the API server, such as pod scheduling, deployments, or RBAC policies. To get started, see [Collecting master logs](#collect_master).</dd>

</dl>

<br />



## Forwarding cluster, app, and Kubernetes API audit logs to {{site.data.keyword.la_full_notm}}
{: #logdna}

Manage logs by deploying LogDNA as a third-party service to your cluster.
{: shortdesc}


### Creating a logging configuration to forward cluster and app logs to {{site.data.keyword.la_full_notm}}
{: #app_logdna}

Use the {{site.data.keyword.containerlong_notm}} observability plug-in to create a logging configuration for {{site.data.keyword.la_full_notm}} in your cluster, and use this logging configuration to automatically collect and forward pod logs to {{site.data.keyword.la_full_notm}}.
{: shortdesc}

You can have only one logging configuration for {{site.data.keyword.la_full_notm}} in your cluster at a time. If you want to use a different {{site.data.keyword.la_full_notm}} service instance to send logs to, first remove any existing logging configuration and then follow the steps to create your new one.
{: note}



Before you begin:
- Verify that you are assigned the **Editor** platform role and **Manager** server access role for {{site.data.keyword.la_full_notm}}.
- Verify that you are assigned the **Administrator** platform role and the **Manager** service access role for all Kubernetes namespaces in {{site.data.keyword.containerlong_notm}} to create the logging configuration. To view a logging configuration or launch the LogDNA dashboard after the logging configuration is created, users must be assigned the **Viewer** platform role and **Reader** service access role for the `ibm-observe` Kubernetes namespace in {{site.data.keyword.containerlong_notm}}.
- If you want to use the CLI to set up the logging configuration:
  - [Install the {{site.data.keyword.containerlong_notm}} observability CLI plug-in (`ibmcloud ob`)](/docs/containers?topic=containers-cs_cli_install#cs_cli_install_steps).
  - [Log in to your account. If applicable, target the appropriate resource group. Set the context for your cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

To set up a logging configuration for your cluster:

1. Create an [{{site.data.keyword.la_full_notm}} service instance](/docs/Log-Analysis-with-LogDNA?topic=Log-Analysis-with-LogDNA-provision) and note the name of the instance. The service instance must belong to the same {{site.data.keyword.cloud_notm}} account where you created your cluster, but can be in a different resource group and {{site.data.keyword.cloud_notm}} region than your cluster.
2. Set up a logging configuration for your cluster. When you create the logging configuration, a Kubernetes namespace `ibm-observe` is created and a LogDNA agent is deployed as a daemonset to all worker nodes in your cluster. This agent collects logs with the extension `*.log` and extensionless files that are stored in the `/var/log` directory of your pod from all namespaces, including `kube-system`. The agent then forwards the logs to the {{site.data.keyword.la_full_notm}} service.

   - **From the console:**
     1. From the [{{site.data.keyword.containerlong_notm}} console](https://cloud.ibm.com/kubernetes/clusters){: external}, select the cluster for which you want to create a LogDNA logging configuration.
     2. On the cluster **Overview** page, click **Connect**.
     3. Select the region and the {{site.data.keyword.la_full_notm}} service instance that you created earlier, and click **Connect**.

   - **From the CLI:**
     1.  Create the LogDNA logging configuration. When you create the LogDNA logging configuration, the ingestion key that was last added is retrieved automatically. If you want to use a different ingestion key, add the `--logdna-ingestion-key <ingestion_key>` option to the command.

         ```
         ibmcloud ob logging config create --cluster <cluster_name_or_ID> --instance <LogDNA_instance_name_or_ID>
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
        Instance Name                            Instance ID                            CRN   
        IBM Cloud Log Analysis with LogDNA-opm   1a111a1a-1111-11a1-a1aa-aaa11111a11a   crn:v1:prod:public:logdna:us-south:a/a11111a1aaaaa11a111aa11a1aa1111a:1a111a1a-1111-11a1-a1aa-aaa11111a11a::  
        ```
        {: screen}

3. Optional: Verify that the LogDNA agent was set up successfully.
   1. If you used the console to create the LogDNA logging configuration, log in to your cluster. For more information, see [Log in to your account. If applicable, target the appropriate resource group. Set the context for your cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure).

   2. Verify that the daemonset for the LogDNA agent was created and all instances are listed as `AVAILABLE`.
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

      The number of daemonset instances that are deployed equals the number of worker nodes in your cluster.

   3. Review the configmap that was created for your LogDNA agent.
      ```
      kubectl describe configmap -n ibm-observe
      ```
      {: pre}

4. Access the logs for your pods from the LogDNA dashboard.
   1. From the [{{site.data.keyword.containerlong_notm}} console](https://cloud.ibm.com/kubernetes/clusters){: external}, select the cluster that you configured.  
   2. On the cluster **Overview** page, click **Launch**. The LogDNA dashboard opens.
   3. Review the pod logs that the LogDNA agent collected from your cluster. It might take a few minutes for your first logs to show.

5. Review how you can [search and filter logs in the LogDNA dashboard](/docs/Log-Analysis-with-LogDNA?topic=Log-Analysis-with-LogDNA-view_logs). 


### Forwarding Kubernetes API audit logs
{: #webhook_logdna}

To monitor user-initiated administrative activity made in your cluster, {{site.data.keyword.containershort_notm}} automatically generates cluster management events and automatically forwards these event logs to {{site.data.keyword.at_full_notm}}. In addition to the cluster management events, you can also collect and forward any events that are passed through your Kubernetes API server to {{site.data.keyword.la_full_notm}}. To set up your cluster to forward audit logs to {{site.data.keyword.la_full_notm}}, you can create a Kubernetes audit system by using the provided image and deployment.
{: shortdesc}

The Kubernetes audit system in your cluster consists of an audit webhook, a log collection service and webserver app, and a logging agent. The webhook collects the Kubernetes API server events from your cluster master. The log collection service is a Kubernetes `ClusterIP` service that is created from an image from the public {{site.data.keyword.cloud_notm}} registry. This service exposes a simple `node.js` HTTP webserver app that is exposed only on the private network. The webserver app parses the log data from the audit webhook and creates each log as a unique JSON line. Finally, the logging agent forwards the logs from the webserver app to {{site.data.keyword.la_full_notm}}, where you can view the logs.

To see how the audit webhook collects logs, check out the {{site.data.keyword.containerlong_notm}} [`kube-audit` policy](https://github.com/IBM-Cloud/kube-samples/blob/master/kube-audit/kube-audit-policy.yaml){: external}.

You cannot modify the default `kube-audit` policy or apply your own custom policy.
{: note}

For more information about Kubernetes audit logs, see the <a href="https://kubernetes.io/docs/tasks/debug-application-cluster/audit/" target="blank">auditing topic <img src="../icons/launch-glyph.svg" alt="External link icon"></a> in the Kubernetes documentation.

**Before you begin**:

* You must have the following permissions:
  * [**Administrator** {{site.data.keyword.cloud_notm}} IAM platform role](/docs/containers?topic=containers-users#platform) for the {{site.data.keyword.containerlong_notm}} cluster.
  * [**Administrator** {{site.data.keyword.cloud_notm}} IAM platform role](/docs/account?topic=account-userroles) for {{site.data.keyword.la_full_notm}}.
* For the cluster that you want to collect API server audit logs from: [Log in to your account. If applicable, target the appropriate resource group. Set the context for your cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
* Keep in mind that only one audit webhook can be created in a cluster. You can set up an audit webhook to forward logs to {{site.data.keyword.la_full_notm}} or to forward logs to an external syslog server, but not both.

**To forward Kubernetes API audit logs to {{site.data.keyword.la_full_notm}}:**

1. Target the global container registry for public {{site.data.keyword.cloud_notm}} images.
  ```
  ibmcloud cr region-set global
  ```
  {: pre}

2. Optional: For more information about the `kube-audit` image, inspect `icr.io/ibm/ibmcloud-kube-audit-to-logdna`.
  ```
  ibmcloud cr image-inspect icr.io/ibm/ibmcloud-kube-audit-to-logdna
  ```
  {: pre}

3. Create a configuration file that is named `ibmcloud-kube-audit.yaml`. This configuration file creates a log collection service and a deployment that pulls the `icr.io/ibm/ibmcloud-kube-audit-to-logdna` image to create a log collection container.
  ```yaml
  apiVersion: v1
  kind: List
  metadata:
    name: ibmcloud-kube-audit
  items:
    - apiVersion: apps/v1
      kind: Deployment
      metadata:
        name: ibmcloud-kube-audit
        labels:
          app: ibmcloud-kube-audit
      spec:
        replicas: 1
        selector:
          matchLabels:
            app: ibmcloud-kube-audit
        template:
          metadata:
            labels:
              app: ibmcloud-kube-audit
          spec:
            containers:
              - name: ibmcloud-kube-audit
                image: 'icr.io/ibm/ibmcloud-kube-audit-to-logdna:latest'
                ports:
                  - containerPort: 3000
    - apiVersion: v1
      kind: Service
      metadata:
        name: ibmcloud-kube-audit-service
        labels:
          app: ibmcloud-kube-audit
      spec:
        selector:
          app: ibmcloud-kube-audit
        ports:
          - protocol: TCP
            port: 80
            targetPort: 3000
        type: ClusterIP
  ```
  {: codeblock}

4. Create the deployment in the `default` namespace of your cluster.
  ```
  kubectl create -f ibmcloud-kube-audit.yaml
  ```
  {: pre}

5. Verify that the `ibmcloud-kube-audit-service` pod has a **STATUS** of `Running`.
  ```
  kubectl get pods -l app=ibmcloud-kube-audit
  ```
  {: pre}

  Example output:
  ```
  NAME                                             READY   STATUS             RESTARTS   AGE
  ibmcloud-kube-audit-c75cb84c5-qtzqd              1/1     Running   0          21s
  ```
  {: screen}

6. Verify that the `ibmcloud-kube-audit-service` service is deployed in your cluster. In the output, note the **CLUSTER_IP**.
  ```
  kubectl get svc -l app=ibmcloud-kube-audit
  ```
  {: pre}

  Example output:
  ```
  NAME                          TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
  ibmcloud-kube-audit-service   ClusterIP   172.21.xxx.xxx   <none>        80/TCP           1m
  ```
  {: screen}

7. Create the audit webhook to collect Kubernetes API server event logs. Add the `http://` prefix to the **CLUSTER_IP**.
  ```
  ibmcloud ks cluster master audit-webhook set --cluster <cluster_name_or_ID> --remote-server http://172.21.xxx.xxx
  ```
  {: pre}

8. Verify that the audit webhook is created in your cluster.
  ```
  ibmcloud ks cluster master audit-webhook get --cluster <cluster_name_or_ID>
  ```
  {: pre}

  Example output:
  ```
  OK
  Server:			http://172.21.xxx.xxx
  ```
  {: screen}

9. Apply the webhook to your Kubernetes API server by refreshing the cluster master. It might take several minutes for the master to refresh.
  ```
  ibmcloud ks cluster master refresh --cluster <cluster_name_or_ID>
  ```
  {: pre}

10. While the master refreshes, [provision an instance of {{site.data.keyword.la_full_notm}} and deploy a logging agent to every worker node in your cluster](/docs/Log-Analysis-with-LogDNA?topic=Log-Analysis-with-LogDNA-kube#kube). The logging agent is required to forward logs from inside your cluster to the {{site.data.keyword.la_full_notm}} service. If you already set up logging agents in your cluster, you can skip this step.

11. After the master refresh completes and the logging agents are running on your worker nodes, you can [view your Kubernetes API audit logs in {{site.data.keyword.la_full_notm}}](/docs/Log-Analysis-with-LogDNA?topic=Log-Analysis-with-LogDNA-kube#kube_step4).

<br />


## Forwarding cluster, app, and Kubernetes API audit logs to an external server
{: #configuring}

Configure log forwarding for {{site.data.keyword.containerlong_notm}} standard clusters to an external server.
{: shortdesc}

### Understanding log forwarding to an external server
{: #logging}

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
      <td>Required: When the logging type is <code>syslog</code> and the protocol is <code>tls</code>, the Kubernetes secret name that contains the Certificate Authority certificate.</td>
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

**Forwarding logs to your own server over the `udp` or `tcp` protocols**

1. Ensure that you have the [**Editor** or **Administrator** {{site.data.keyword.cloud_notm}} IAM platform role](/docs/containers?topic=containers-users#platform).

2. For the cluster where the log source is located: [Log in to your account. If applicable, target the appropriate resource group. Set the context for your cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

3. Set up a server that accepts a syslog protocol in 1 of 2 ways:
  * Set up and manage your own server or have a provider manage it for you. If a provider manages the server for you, get the logging endpoint from the logging provider.

  * Run syslog from a container. For example, you can use this [deployment .yaml file](https://github.com/IBM-Cloud/kube-samples/blob/master/deploy-apps-clusters/deploy-syslog-from-kube.yaml){: external} to fetch a Docker public image that runs a container in your cluster. The image publishes the port `514` on the public cluster IP address, and uses this public cluster IP address to configure the syslog host.

  You can see your logs as valid JSON by removing syslog prefixes. To do so, add the following code to the top of your <code>etc/rsyslog.conf</code> file where your rsyslog server runs: <code>$template customFormat,"%msg%\n"</br>$ActionFileDefaultTemplate customFormat</code>
  {: tip}

4. Create a log forwarding configuration.
    ```
    ibmcloud ks logging config create --cluster <cluster_name_or_ID> --logsource <log_source> --namespace <kubernetes_namespace> --hostname <log_server_hostname_or_IP> --port <log_server_port> --type syslog --app-containers <container1,2> --app-paths <paths_to_logs> --syslog-protocol <protocol> --skip-validation
    ```
    {: pre}

</br></br>

**Forwarding logs to your own server over the `tls` protocol**

The following steps are general instructions. Prior to using the container in a production environment, be sure that any security requirements that you need, are met.
{: tip}

1. Ensure that you have the following [{{site.data.keyword.cloud_notm}} IAM roles](/docs/containers?topic=containers-users#platform):
    * **Editor** or **Administrator** platform role for the cluster
    * **Writer** or **Manager** service role for the `kube-system` namespace

2. For the cluster where the log source is located: [Log in to your account. If applicable, target the appropriate resource group. Set the context for your cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

3. Set up a server that accepts a syslog protocol in 1 of 2 ways:
  * Set up and manage your own server or have a provider manage it for you. If a provider manages the server for you, get the logging endpoint from the logging provider.

  * Run syslog from a container. For example, you can use this [deployment .yaml file](https://github.com/IBM-Cloud/kube-samples/blob/master/deploy-apps-clusters/deploy-syslog-from-kube.yaml){: external} to fetch a Docker public image that runs a container in your cluster. The image publishes the port `514` on the public cluster IP address, and uses this public cluster IP address to configure the syslog host. You need to inject the relevant Certificate Authority and server-side certificates and update the `syslog.conf` to enable `tls` on your server.

4. Save your Certificate Authority certificate to a file named `ca-cert`. It must be that exact name.

5. Create a secret in the `kube-system` namespace for the `ca-cert` file. When you create your logging configuration, use the secret name for the `--ca-cert` flag.
    ```
    kubectl -n kube-system create secret generic --from-file=ca-cert
    ```
    {: pre}

6. Create a log forwarding configuration.
    ```
    ibmcloud ks logging config create --cluster <cluster name or id> --logsource <log source> --type syslog --syslog-protocol tls --hostname <ip address of syslog server> --port <port for syslog server, 514 is default> --ca-cert <secret name> --verify-mode <defaults to verify-none>
    ```
    {: pre}

### Forwarding Kubernetes API audit logs
{: #audit_enable}

To audit any events that are passed through your Kubernetes API server, you can create a configuration to forward events to your external server.
{: shortdesc}

For more information about Kubernetes audit logs, see the <a href="https://kubernetes.io/docs/tasks/debug-application-cluster/audit/" target="blank">auditing topic <img src="../icons/launch-glyph.svg" alt="External link icon"></a> in the Kubernetes documentation.

* [Filters](#filter-logs) are not supported.
* Keep in mind that only one audit webhook can be created in a cluster. You can set up an audit webhook to forward logs to {{site.data.keyword.at_full_notm}} or to forward logs to an external syslog server, but not both.
* You must have the [**Administrator** {{site.data.keyword.cloud_notm}} IAM platform role](/docs/containers?topic=containers-users#platform) for the cluster.

**Before you begin**

1. Set up a remote logging server where you can forward the logs. For example, you can [use Logstash with Kubernetes](https://kubernetes.io/docs/tasks/debug-application-cluster/audit/#use-logstash-to-collect-and-distribute-audit-events-from-webhook-backend){: external} to collect audit events.

2. For the cluster that you want to collect API server audit logs from: [Log in to your account. If applicable, target the appropriate resource group. Set the context for your cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

To forward Kubernetes API audit logs:

1. Set up the webhook. If you do not provide any information in the flags, a default configuration is used.

    ```
    ibmcloud ks cluster master audit-webhook set --cluster <cluster_name_or_ID> --remote-server <server_URL_or_IP> --ca-cert <CA_cert_path> --client-cert <client_cert_path> --client-key <client_key_path>
    ```
    {: pre}

  <table summary="The columns are read from left to right. The first column has the parameter of the YAML file. The second column describes the parameter.">
  <caption>Understanding this command's components</caption>
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
        <td><code><em>&lt;server_URL&gt;</em></code></td>
        <td>The URL or IP address for the remote logging service that you want to send logs to. Certificates are ignored if you provide an unsecure server URL.</td>
      </tr>
      <tr>
        <td><code><em>&lt;CA_cert_path&gt;</em></code></td>
        <td>The file path for the CA certificate that is used to verify the remote logging service.</td>
      </tr>
      <tr>
        <td><code><em>&lt;client_cert_path&gt;</em></code></td>
        <td>The file path for the client certificate that is used to authenticate against the remote logging service.</td>
      </tr>
      <tr>
        <td><code><em>&lt;client_key_path&gt;</em></code></td>
        <td>The file path for the corresponding client key that is used to connect to the remote logging service.</td>
      </tr>
    </tbody>
  </table>

2. Verify that log forwarding was enabled by viewing the URL for the remote logging service.

    ```
    ibmcloud ks cluster master audit-webhook get --cluster <cluster_name_or_ID>
    ```
    {: pre}

    Example output:
    ```
    OK
    Server:			https://8.8.8.8
    ```
    {: screen}

3. Apply the configuration update by restarting the Kubernetes master.

    ```
    ibmcloud ks cluster master refresh --cluster <cluster_name_or_ID>
    ```
    {: pre}

4. Optional: If you want to stop forwarding audit logs, you can disable your configuration.
    1. For the cluster that you want to stop collecting API server audit logs from: [Log in to your account. If applicable, target the appropriate resource group. Set the context for your cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
    2. Disable the webhook back-end configuration for the cluster's API server.

        ```
        ibmcloud ks cluster master audit-webhook unset --cluster <cluster_name_or_ID>
        ```
        {: pre}

    3. Apply the configuration update by restarting the Kubernetes master.

        ```
        ibmcloud ks cluster master refresh --cluster <cluster_name_or_ID>
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

**Verifying**</br>
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

**Updating**</br>
You can update a logging configuration that you already created:
```
ibmcloud ks logging config update --cluster <cluster_name_or_ID> --id <log_config_id> --namespace <namespace> --type <server_type> --syslog-protocol <protocol> --logsource <source> --hostname <hostname_or_ingestion_URL> --port <port> --space <cluster_space> --org <cluster_org> --app-containers <container1,2> --app-paths <paths_to_logs>
```
{: pre}

**Deleting**</br>
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
* Ensure that you have the [**Administrator** {{site.data.keyword.cloud_notm}} IAM platform role](/docs/containers?topic=containers-users#platform) for the cluster.

**Creating a snapshot**

1. Create an Object Storage bucket through the {{site.data.keyword.cloud_notm}} console by following [this getting started tutorial](/docs/cloud-object-storage?topic=cloud-object-storage-getting-started-cloud-object-storage#gs-create-buckets).

2. Generate [HMAC service credentials](/docs/cloud-object-storage/iam?topic=cloud-object-storage-service-credentials) in the bucket that you created.
  1. In the **Service Credentials** tab of the {{site.data.keyword.cos_short}} dashboard, click **New Credential**.
  2. Give the HMAC credentials the `Writer` service role.
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

<br />


## Choosing a monitoring solution
{: #view_metrics}

Metrics help you monitor the health and performance of your clusters. You can use the standard Kubernetes and container runtime features to monitor the health of your clusters and apps.
{: shortdesc}

Every Kubernetes master is continuously monitored by IBM. {{site.data.keyword.containerlong_notm}} automatically scans every node where the Kubernetes master is deployed for vulnerabilities that are found in Kubernetes and OS-specific security fixes. If vulnerabilities are found, {{site.data.keyword.containerlong_notm}} automatically applies fixes and resolves vulnerabilities on behalf of the user to ensure master node protection. You are responsible for monitoring and analyzing the logs for the rest of your cluster components.

To avoid conflicts when using metrics services, be sure that clusters across resource groups and regions have unique names.
{: tip}

<dl>
  <dt>{{site.data.keyword.mon_full}}</dt>
  <dd>Gain operational visibility into the performance and health of your apps and your cluster by deploying a Sysdig agent to your worker nodes. The agent collects pod and cluster metrics, and sends these metrics to {{site.data.keyword.mon_full_notm}}. For more information about {{site.data.keyword.mon_full_notm}}, see the [service documentation](/docs/Monitoring-with-Sysdig?topic=Monitoring-with-Sysdig-getting-started). To set up the Sysdig agent in your cluster, see [Viewing cluster and app metrics with {{site.data.keyword.mon_full_notm}}](#sysdig).</dd>

  <dt>Kubernetes dashboard</dt>
  <dd>The Kubernetes dashboard is an administrative web interface where you can review the health of your worker nodes, find Kubernetes resources, deploy containerized apps, and troubleshoot apps with logging and monitoring information. For more information about how to access your Kubernetes dashboard, see [Launching the Kubernetes dashboard for {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-deploy_app#cli_dashboard).</dd>

</dl>

<br />


## Creating a monitoring configuration to forward cluster and app metrics to {{site.data.keyword.mon_full_notm}}
{: #sysdig}

Use the {{site.data.keyword.containerlong_notm}} observability plug-in to create a monitoring configuration for {{site.data.keyword.mon_full_notm}} in your cluster, and use this monitoring configuration to automatically collect and forward metrics to {{site.data.keyword.mon_full_notm}}.
{: shortdesc}

With {{site.data.keyword.mon_full_notm}}, you can collects cluster and pod metrics, such as the CPU and memory usage of your worker nodes, incoming and outgoing HTTP traffic for your pods, and data about several infrastructure components. In addition, the agent can collect custom application metrics by using either a Prometheus-compatible scraper or a StatsD facade.

You can have only one monitoring configuration for {{site.data.keyword.mon_full_notm}} in your cluster at a time. If you want to use a different {{site.data.keyword.mon_full_notm}} service instance to send metrics to, first remove any existing monitoring configuration and then follow the steps to create your new one.
{: note}


Before you begin:
- Verify that you are assigned the **Editor** platform role and **Manager** server access role for {{site.data.keyword.mon_full_notm}}.
- Verify that you are assigned the **Administrator** platform role and the **Manager** service access role for all Kubernetes namespaces in {{site.data.keyword.containerlong_notm}} to create the monitoring configuration. To view a monitoring configuration or launch the Sysdig dashboard after the monitoring configuration is created, users must be assigned the **Viewer** platform role and **Reader** service access role for the `ibm-observe` Kubernetes namespace in {{site.data.keyword.containerlong_notm}}.
- If you want to use the CLI to set up the monitoring configuration:
  - [Install the {{site.data.keyword.containerlong_notm}} observability CLI plug-in (`ibmcloud ob`)](/docs/containers?topic=containers-cs_cli_install#cs_cli_install_steps).
  - [Log in to your account. If applicable, target the appropriate resource group. Set the context for your cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

To set up a monitoring configuration for your cluster:

1. Create an [{{site.data.keyword.mon_full_notm}} service instance](/docs/Monitoring-with-Sysdig?topic=Monitoring-with-Sysdig-provision) and note the name of the instance. The service instance must belong to the same {{site.data.keyword.cloud_notm}} account where you created your cluster, but can be in a different resource group and {{site.data.keyword.cloud_notm}} region than your cluster.
2. Set up a monitoring configuration for your cluster. When you create the monitoring configuration, a Kubernetes namespace `ibm-observe` is created and a Sysdig agent is deployed as a Kubernetes daemonset to all worker nodes in your cluster. This agent collects cluster and pod metrics, such as the worker node CPU and memory usage, or the amount incoming and outgoing network traffic to your pods.

   - **From the console: **
     1. From the [{{site.data.keyword.containerlong_notm}} console](https://cloud.ibm.com/kubernetes/clusters){: external}, select the cluster for which you want to create a Sysdig monitoring configuration.
     2. On the cluster **Overview** page, click **Connect**.
     3. Select the region and the {{site.data.keyword.mon_full_notm}} service instance that you created earlier, and click **Connect**.

   - **From the CLI: **
     1. Create the Sysdig monitoring configuration. When you create the Sysdig monitoring configuration, the access key that was last added is retrieved automatically. If you want to use a different access key, add the `--sysdig-access-key <access_key>` option to the command.

        ```
        ibmcloud ob monitoring config create --cluster <cluster_name_or_ID> --instance <Sysdig_instance_name_or_ID>
        ```
        {: pre}

        Example output:
        ```
        Creating configuration...
        OK
        ```
        {: screen}

     2. Verify that the monitoring configuration was added to your cluster.
        ```
        ibmcloud ob monitoring config list --cluster <cluster_name_or_ID>
        ```
        {: pre}

        Example output:
        ```
        Listing configurations...

        OK
        Instance Name                            Instance ID                            CRN   
        IBM Cloud Monitornig with Sysdig-aaa     1a111a1a-1111-11a1-a1aa-aaa11111a11a   crn:v1:prod:public:sysdig:us-south:a/a11111a1aaaaa11a111aa11a1aa1111a:1a111a1a-1111-11a1-a1aa-aaa11111a11a::  
        ```
        {: screen}

3. Optional: Verify that the Sysdig agent was set up successfully.
   1. If you used the console to create the Sysdig monitoring configuration, log in to your cluster. For more information, see [Log in to your account. If applicable, target the appropriate resource group. Set the context for your cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
   2. Verify that the daemonset for the Sysdig agent was created and all instances are listed as `AVAILABLE`.
      ```
      kubectl get daemonsets -n ibm-observe
      ```
      {: pre}

      Example output:
      ```
      NAME           DESIRED   CURRENT   READY   UP-TO-DATE   AVAILABLE   NODE SELECTOR   AGE
      sysdig-agent   9         9         9       9            9           <none>          14m
      ```
      {: screen}

      The number of daemonset instances that are deployed equals the number of worker nodes in your cluster.

   3. Review the configmap that was created for your Sysdig agent.
      ```
      kubectl describe configmap -n ibm-observe
      ```
      {: pre}

4. Access the metrics for your pods and cluster from the Sysdig dashboard.
   1. From the [{{site.data.keyword.containerlong_notm}} console](https://cloud.ibm.com/kubernetes/clusters){: external}, select the cluster that you configured.
   2. On the cluster **Overview** page, click **Launch**. The Sysdig dashboard opens.
   3. Review the pod and cluster metrics that the Sysdig agent collected from your cluster. It might take a few minutes for your first metrics to show.

5. Review how you can work with the [Sysdig dashboard](/docs/Monitoring-with-Sysdig?topic=Monitoring-with-Sysdig-dashboards#dashboards) to further analyze your metrics.


## Viewing cluster states
{: #states}

Review the state of a Kubernetes cluster to get information about the availability and capacity of the cluster, and potential problems that might occur.
{: shortdesc}

To view information about a specific cluster, such as its zones, service endpoint URLs, Ingress subdomain, version, and owner, use the `ibmcloud ks cluster get --cluster <cluster_name_or_ID>` [command](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_get). Include the `--show-resources` flag to view more cluster resources such as add-ons for storage pods or subnet VLANs for public and private IPs.

You can review information about the overall cluster, the IBM-managed master, and your worker nodes. To troubleshoot your cluster and worker nodes, see [Troubleshooting clusters](/docs/containers?topic=containers-cs_troubleshoot#debug_clusters).

### Cluster states
{: #states_cluster}

You can view the current cluster state by running the `ibmcloud ks cluster ls` command and locating the **State** field.
{: shortdesc}
    <table summary="Every table row should be read left to right, with the cluster state in column one and a description in column two.">
    <caption>Cluster states</caption>
       <thead>
       <th>Cluster state</th>
       <th>Description</th>
       </thead>
       <tbody>
    <tr>
       <td>`Aborted`</td>
       <td>The deletion of the cluster is requested by the user before the Kubernetes master is deployed. After the deletion of the cluster is completed, the cluster is removed from your dashboard. If your cluster is stuck in this state for a long time, open an [{{site.data.keyword.cloud_notm}} support case](/docs/containers?topic=containers-get-help).</td>
       </tr>
     <tr>
         <td>`Critical`</td>
         <td>The Kubernetes master cannot be reached or all worker nodes in the cluster are down. If you enabled {{site.data.keyword.keymanagementservicelong_notm}} in your cluster, the {{site.data.keyword.keymanagementserviceshort}} container might fail to encrypt or decrypt your cluster secrets. If so, you can view an error with more information when you run `kubectl get secrets`.</td>
        </tr>
       <tr>
         <td>`Delete failed`</td>
         <td>The Kubernetes master or at least one worker node cannot be deleted.  </td>
       </tr>
       <tr>
         <td>`Deleted`</td>
         <td>The cluster is deleted but not yet removed from your dashboard. If your cluster is stuck in this state for a long time, open an [{{site.data.keyword.cloud_notm}} support case](/docs/containers?topic=containers-get-help). </td>
       </tr>
       <tr>
       <td>`Deleting`</td>
       <td>The cluster is being deleted and cluster infrastructure is being dismantled. You cannot access the cluster.  </td>
       </tr>
       <tr>
         <td>`Deploy failed`</td>
         <td>The deployment of the Kubernetes master could not be completed. You cannot resolve this state. Contact IBM Cloud support by opening an [{{site.data.keyword.cloud_notm}} support case](/docs/containers?topic=containers-get-help).</td>
       </tr>
         <tr>
           <td>`Deploying`</td>
           <td>The Kubernetes master is not fully deployed yet. You cannot access your cluster. Wait until your cluster is fully deployed to review the health of your cluster.</td>
          </tr>
          <tr>
           <td>`Normal`</td>
           <td>All worker nodes in a cluster are up and running. You can access the cluster and deploy apps to the cluster. This state is considered healthy and does not require an action from you.<p class="note">Although the worker nodes might be normal, other infrastructure resources, such as [networking](/docs/containers?topic=containers-cs_troubleshoot_network) and [storage](/docs/containers?topic=containers-cs_troubleshoot_storage), might still need attention. If you just created the cluster, some parts of the cluster that are used by other services such as Ingress secrets or registry image pull secrets, might still be in process.</p></td>
        </tr>
          <tr>
           <td>`Pending`</td>
           <td>The Kubernetes master is deployed. The worker nodes are being provisioned and are not available in the cluster yet. You can access the cluster, but you cannot deploy apps to the cluster.  </td>
         </tr>
       <tr>
         <td>`Requested`</td>
         <td>A request to create the cluster and order the infrastructure for the Kubernetes master and worker nodes is sent. When the deployment of the cluster starts, the cluster state changes to <code>Deploying</code>. If your cluster is stuck in the <code>Requested</code> state for a long time, open an [{{site.data.keyword.cloud_notm}} support case](/docs/containers?topic=containers-get-help). </td>
       </tr>
       <tr>
         <td>`Updating`</td>
         <td>The Kubernetes API server that runs in your Kubernetes master is being updated to a new Kubernetes API version. During the update, you cannot access or change the cluster. Worker nodes, apps, and resources that the user deployed are not modified and continue to run. Wait for the update to complete to review the health of your cluster. </td>
       </tr>
       <tr>
        <td>`Unsupported`</td>
        <td>The [Kubernetes version](/docs/containers?topic=containers-cs_versions#cs_versions) that the cluster runs is no longer supported. Your cluster's health is no longer actively monitored or reported. Additionally, you cannot add or reload worker nodes. To continue receiving important security updates and support, you must update your cluster. Review the [version update preparation actions](/docs/containers?topic=containers-cs_versions#prep-up), then [update your cluster](/docs/containers?topic=containers-update#update) to a supported Kubernetes version.</td>
       </tr>
        <tr>
           <td>`Warning`</td>
           <td><ul><li>At least one worker node in the cluster is not available, but other worker nodes are available and can take over the workload. Try to [reload](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload) the unavailable worker nodes.</li>
           <li>Your cluster has zero worker nodes, such as if you created a cluster without any worker nodes or manually removed all the worker nodes from the cluster. [Resize your worker pool](/docs/containers?topic=containers-add_workers#resize_pool) to add worker nodes to recover from a `Warning` state.</li>
           <li>A control plane operation for your cluster failed. View the cluster in the console or run `ibmcloud ks cluster get --cluster <cluster_name_or_ID>` to [check the **Master Status** for further debugging](/docs/containers?topic=containers-cs_troubleshoot#debug_master).</li></ul></td>
        </tr>
       </tbody>
     </table>




### Master states
{: #states_master}

Your {{site.data.keyword.containerlong_notm}} includes an IBM-managed master with highly available replicas, automatic security patch updates applied for you, and automation in place to recover in case of an incident. You can check the health, status, and state of the cluster master by running `ibmcloud ks cluster get --cluster <cluster_name_or_ID>`.
{: shortdesc}

**Master Health**<br>
The **Master Health** reflects the state of master components and notifies you if something needs your attention. The health might be one of the following:
*   `error`: The master is not operational. IBM is automatically notified and takes action to resolve this issue. You can continue monitoring the health until the master is `normal`. You can also [open an {{site.data.keyword.cloud_notm}} support case](/docs/containers?topic=containers-get-help).
*   `normal`: The master is operational and healthy. No action is required.
*   `unavailable`: The master might not be accessible, which means some actions such as resizing a worker pool are temporarily unavailable. IBM is automatically notified and takes action to resolve this issue. You can continue monitoring the health until the master is `normal`.
*   `unsupported`: The master runs an unsupported version of Kubernetes. You must [update your cluster](/docs/containers?topic=containers-update) to return the master to `normal` health.

**Master Status and State**<br>
The **Master Status** provides details of what operation from the master state is in progress. The status includes a timestamp of how long the master has been in the same state, such as `Ready (1 month ago)`. The **Master State** reflects the lifecycle of possible operations that can be performed on the master, such as deploying, updating, and deleting. Each state is described in the following table.

|Master state|Description|
|--- |--- |
|`deployed`|The master is successfully deployed. Check the status to verify that the master is `Ready` or to see if an update is available.|
|`deploying`|The master is currently deploying. Wait for the state to become `deployed` before working with your cluster, such as adding worker nodes.|
|`deploy_failed`|The master failed to deploy. IBM Support is notified and works to resolve the issue. Check the **Master Status** field for more information, or wait for the state to become `deployed`.|
|`deleting`|The master is currently deleting because you deleted the cluster. You cannot undo a deletion. After the cluster is deleted, you can no longer check the master state because the cluster is completely removed.|
|`delete_failed`|The master failed to delete. IBM Support is notified and works to resolve the issue. You cannot resolve the issue by trying to delete the cluster again. Instead, check the **Master Status** field for more information, or wait for the cluster to delete. You can also [open an {{site.data.keyword.cloud_notm}} support case](/docs/containers?topic=containers-get-help).|
|`updating`|The master is updating its Kubernetes version. The update might be a patch update that is automatically applied, or a minor or major version that you applied by updating the cluster. During the update, your highly available master can continue processing requests, and your app workloads and worker nodes continue to run. After the master update is complete, you can [update your worker nodes](/docs/containers?topic=containers-update#worker_node).</br></br>If the update is unsuccessful, the master returns to a `deployed` state and continues running the previous version. IBM Support is notified and works to resolve the issue. You can check if the update failed in the **Master Status** field.|
|`update_cancelled`|The master update is canceled because the cluster was not in a healthy state at the time of the update. Your master remains in this state until your cluster is healthy and you manually update the master. To update the master, use the `ibmcloud ks cluster master update` [command](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_update).If you do not want to update the master to the default `major.minor` version during the update, include the `--version` flag and specify the latest patch version that is available for the `major.minor` version that you want, such as `1.17.11`. To list available versions, run `ibmcloud ks versions`.|
|`update_failed`|The master update failed. IBM Support is notified and works to resolve the issue. You can continue to monitor the health of the master until the master reaches a normal state. If the master remains in this state for more than 1 day, [open an {{site.data.keyword.cloud_notm}} support case](/docs/containers?topic=containers-get-help). IBM Support might identify other issues in your cluster that you must fix before the master can be updated.|
{: caption="Master states"}
{: summary="Table rows read from left to right, with the master state in column one and a description in column two."}




### Worker node states
{: #states_workers}

You can view the current worker node state by running the `ibmcloud ks worker ls --cluster <cluster_name_or_ID>` command and locating the **State** and **Status** fields.
{: shortdesc}
    <table summary="Every table row should be read left to right, with the cluster state in column one and a description in column two.">
    <caption>Worker node states</caption>
      <thead>
      <th>Worker node state</th>
      <th>Description</th>
      </thead>
      <tbody>
    <tr>
        <td>`Critical`</td>
        <td>A worker node can go into a Critical state for many reasons: <ul><li>You initiated a reboot for your worker node without cordoning and draining your worker node. Rebooting a worker node can cause data corruption in <code>containerd</code>, <code>kubelet</code>, <code>kube-proxy</code>, and <code>calico</code>. </li>
        <li>The pods that are deployed to your worker node do not use proper resource limits for [memory](https://kubernetes.io/docs/tasks/configure-pod-container/assign-memory-resource/){: external} and [CPU](https://kubernetes.io/docs/tasks/configure-pod-container/assign-cpu-resource/){: external}. If you set none or excessive resource limits, pods can consume all available resources, leaving no resources for other pods to run on this worker node. This overcommitment of workload causes the worker node to fail. <ol><li>List the pods that run on your worker node and review the CPU and memory usage, requests and limits. <pre class="codeblock"><code>kubectl describe node &lt;worker_private_IP&gt;</code></pre></li><li>For pods that consume a lot of memory and CPU resources, check if you set proper resource limits for memory and CPU. <pre class="codeblock"><code>kubectl get pods &lt;pod_name&gt; -n &lt;namespace&gt; -o json</code></pre></li><li>Optional: Remove the resource-intensive pods to free up compute resources on your worker node. <pre class="codeblock"><code>kubectl delete pod &lt;pod_name&gt;</code></pre><pre class="codeblock"><code>kubectl delete deployment &lt;deployment_name&gt;</code></pre></li></ol></li>
        <li><code>containerd</code>, <code>kubelet</code>, or <code>calico</code> went into an unrecoverable state after it ran hundreds or thousands of containers over time. </li>
        <li>You set up a Virtual Router Appliance for your worker node that went down and cut off the communication between your worker node and the Kubernetes master. </li><li> Current networking issues in {{site.data.keyword.containerlong_notm}} or IBM Cloud infrastructure that causes the communication between your worker node and the Kubernetes master to fail.</li>
        <li>Your worker node ran out of capacity. Check the <strong>Status</strong> of the worker node to see whether it shows <strong>Out of disk</strong> or <strong>Out of memory</strong>. If your worker node is out of capacity, consider to either reduce the workload on your worker node or add a worker node to your cluster to help load balance the workload.</li>
        <li>The device was powered off from the [{{site.data.keyword.cloud_notm}} console resource list ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/resources). Open the resource list and find your worker node ID in the **Devices** list. In the action menu, click **Power On**.</li></ul>
        In many cases, [reloading](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload) your worker node can solve the problem. When you reload your worker node, the latest [patch version](/docs/containers?topic=containers-cs_versions#version_types) is applied to your worker node. The major and minor version is not changed. Before you reload your worker node, make sure to cordon and drain your worker node to ensure that the existing pods are terminated gracefully and rescheduled onto remaining worker nodes. </br></br> If reloading the worker node does not resolve the issue, go to the next step to continue troubleshooting your worker node.<p class="tip">In classic clusters, you can [configure health checks for your worker node and enable Autorecovery](/docs/containers?topic=containers-health#autorecovery). If Autorecovery detects an unhealthy worker node based on the configured checks, Autorecovery triggers a corrective action like an OS reload on the worker node. For more information about how Autorecovery works, see the [Autorecovery blog ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/cloud/blog/autorecovery-utilizes-consistent-hashing-high-availability).</p>
        </td>
       </tr>
       <tr>
       <td>`Deployed`</td>
       <td>Updates are successfully deployed to your worker node. After updates are deployed, {{site.data.keyword.containerlong_notm}} starts a health check on the worker node. After the health check is successful, the worker node goes into a <code>Normal</code> state. Worker nodes in a <code>Deployed</code> state usually are ready to receive workloads, which you can check by running <code>kubectl get nodes</code> and confirming that the state shows <code>Normal</code>. </td>
       </tr>
        <tr>
          <td>`Deploying`</td>
          <td>When you update the Kubernetes version of your worker node, your worker node is redeployed to install the updates. If you reload or reboot your worker node, the worker node is redeployed to automatically install the latest patch version. If your worker node is stuck in this state for a long time, continue with the next step to see whether a problem occurred during the deployment. </td>
       </tr>
       <tr>
         <td>`Deploy_failed`</td>
         <td>Your worker node could not be deployed. List the details for the worker node to find the details for the failure by running `ibmcloud ks worker get --cluster <cluster_name_or_id> --worker <worker_node_id>`.</td>
       </tr>
          <tr>
          <td>`Normal`</td>
          <td>Your worker node is fully provisioned and ready to be used in the cluster. This state is considered healthy and does not require an action from the user. **Note**: Although the worker nodes might be normal, other infrastructure resources, such as [networking](/docs/containers?topic=containers-cs_troubleshoot_network) and [storage](/docs/containers?topic=containers-cs_troubleshoot_storage), might still need attention.</td>
       </tr>
     <tr>
          <td>`Provisioning`</td>
          <td>Your worker node is being provisioned and is not available in the cluster yet. You can monitor the provisioning process in the <strong>Status</strong> column of your CLI output. If your worker node is stuck in this state for a long time, continue with the next step to see whether a problem occurred during the provisioning.</td>
        </tr>
        <tr>
          <td>`Provision_failed`</td>
          <td>Your worker node could not be provisioned. List the details for the worker node to find the details for the failure by running `ibmcloud ks worker get --cluster <cluster_name_or_id> --worker <worker_node_id>`.</td>
        </tr>
     <tr>
          <td>`Reloading`</td>
          <td>Your worker node is being reloaded and is not available in the cluster. You can monitor the reloading process in the <strong>Status</strong> column of your CLI output. If your worker node is stuck in this state for a long time, continue with the next step to see whether a problem occurred during the reloading.</td>
         </tr>
         <tr>
          <td>`Reloading_failed`</td>
          <td>Your worker node could not be reloaded. List the details for the worker node to find the details for the failure by running `ibmcloud ks worker get --cluster <cluster_name_or_id> --worker <worker_node_id>`.</td>
        </tr>
        <tr>
          <td>`Reload_pending`</td>
          <td>A request to reload or to update the Kubernetes version of your worker node is sent. When the worker node is being reloaded, the state changes to <code>Reloading</code>. </td>
        </tr>
        <tr>
         <td>`Unknown`</td>
         <td>The Kubernetes master is not reachable for one of the following reasons:<ul><li>You requested an update of your Kubernetes master. The state of the worker node cannot be retrieved during the update. If the worker node remains in this state for an extended period of time even after the Kubernetes master is successfully updated, try to [reload](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload) the worker node.</li><li>You might have another firewall that is protecting your worker nodes, or changed firewall settings recently. {{site.data.keyword.containerlong_notm}} requires certain IP addresses and ports to be opened to allow communication from the worker node to the Kubernetes master and vice versa. For more information, see [Firewall prevents worker nodes from connecting](/docs/containers?topic=containers-firewall#vyatta_firewall).</li><li>The Kubernetes master is down. Contact {{site.data.keyword.cloud_notm}} support by opening an [{{site.data.keyword.cloud_notm}} support case](/docs/containers?topic=containers-get-help).</li></ul></td>
    </tr>
       <tr>
          <td>`Warning`</td>
          <td>Your worker node is reaching the limit for memory or disk space. You can either reduce work load on your worker node or add a worker node to your cluster to help load balance the work load.</td>
    </tr>
      </tbody>
    </table>




## Configuring health monitoring for worker nodes in classic clusters with Autorecovery
{: #autorecovery}

The Autorecovery system uses various checks to query worker node health status. If Autorecovery detects an unhealthy worker node based on the configured checks, Autorecovery triggers a corrective action like an OS reload on the worker node. Only one worker node undergoes a corrective action at a time. The worker node must successfully complete the corrective action before any other worker node undergoes a corrective action. For more information, see this [Autorecovery blog post](https://www.ibm.com/cloud/blog/autorecovery-utilizes-consistent-hashing-high-availability){: external}.
{: shortdesc}

<p class="note">Autorecovery requires at least one healthy node to function properly. Configure Autorecovery with active checks only in clusters with two or more worker nodes.</br></br><img src="images/icon-classic.png" alt="Classic infrastructure provider icon" width="15" style="width:15px; border-style: none"/> Autorecovery is supported only for classic clusters, and is not supported for VPC clusters.</p>

Before you begin:
- Ensure that you have the following [{{site.data.keyword.cloud_notm}} IAM roles](/docs/containers?topic=containers-users#platform):
    - **Administrator** platform role for the cluster
    - **Writer** or **Manager** service role for the `kube-system` namespace
- [Log in to your account. If applicable, target the appropriate resource group. Set the context for your cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

To configure Autorecovery:

1.  [Follow the instructions](/docs/containers?topic=containers-helm#install_v3) to install the Helm version 3 client on your local machine.

2. Create a configuration map file that defines your checks in JSON format. For example, the following YAML file defines three checks: an HTTP check and two Kubernetes API server checks. Refer to the tables following the example YAML file for information about the three kinds of checks and information about the individual components of the checks.<p class="tip">*Define each check as a unique key in the `data` section of the configuration map.</p>

   ```yaml
   kind: ConfigMap
   apiVersion: v1
   metadata:
     name: ibm-worker-recovery-checks
     namespace: kube-system
   data:
     checknode.json: |
       {
         "Check":"KUBEAPI",
         "Resource":"NODE",
         "FailureThreshold":3,
         "CorrectiveAction":"RELOAD",
         "CooloffSeconds":1800,
         "IntervalSeconds":180,
         "TimeoutSeconds":10,
         "Enabled":true
       }
     checkpod.json: |
       {
         "Check":"KUBEAPI",
         "Resource":"POD",
         "PodFailureThresholdPercent":50,
         "FailureThreshold":3,
         "CorrectiveAction":"RELOAD",
         "CooloffSeconds":1800,
         "IntervalSeconds":180,
         "TimeoutSeconds":10,
         "Enabled":true
       }
     checkhttp.json: |
       {
         "Check":"HTTP",
         "FailureThreshold":3,
         "CorrectiveAction":"REBOOT",
         "CooloffSeconds":1800,
         "IntervalSeconds":180,
         "TimeoutSeconds":10,
         "Port":80,
         "ExpectedStatus":200,
         "Route":"/myhealth",
         "Enabled":false
       }
   ```
   {:codeblock}

   <table summary="The columns are read from left to right. The first column has the parameter of the YAML file. The second column describes the parameter.">
   <caption>Understanding the configmap components</caption>
   <thead>
   <col width="25%">
   <th>Component</th>
   <th>Description</th>
   </thead>
   <tbody>
   <tr>
   <td><code>name</code></td>
   <td>The configuration name <code>ibm-worker-recovery-checks</code> is a constant and cannot be changed.</td>
   </tr>
   <tr>
   <td><code>namespace</code></td>
   <td>The <code>kube-system</code> namespace is a constant and cannot be changed.</td>
   </tr>
   <tr>
   <td><code>checknode.json</code></td>
   <td>Defines a Kubernetes API node check that checks whether each worker node is in the <code>Ready</code> state. The check for a specific worker node counts as a failure if the worker node is not in the <code>Ready</code> state. The check in the example YAML runs every 3 minutes. If it fails three consecutive times, the worker node is reloaded. This action is equivalent to running <code>ibmcloud ks worker reload</code>.<br></br>The node check is enabled until you set the <b>Enabled</b> field to <code>false</code> or remove the check.</td>
   </tr>
   <tr>
   <td><code>checkpod.json</code></td>
   <td>
   Defines a Kubernetes API pod check that checks the total percentage of <code>NotReady</code> pods on a worker node based on the total pods that are assigned to that worker node. The check for a specific worker node counts as a failure if the total percentage of <code>NotReady</code> pods is greater than the defined <code>PodFailureThresholdPercent</code>. The check in the example YAML runs every 3 minutes. If it fails three consecutive times, the worker node is reloaded. This action is equivalent to running <code>ibmcloud ks worker reload</code>. For example, the default <code>PodFailureThresholdPercent</code> is 50%. If the percentage of <code>NotReady</code> pods is greater than 50% three consecutive times, the worker node is reloaded. <br></br>By default, pods in all namespaces are checked. To restrict the check to only pods in a specified namespace, add the <code>Namespace</code> field to the check. The pod check is enabled until you set the <b>Enabled</b> field to <code>false</code> or remove the check.
   </td>
   </tr>
   <tr>
   <td><code>checkhttp.json</code></td>
   <td>Defines an HTTP check that checks if an HTTP server that runs on your worker node is healthy. To use this check, you must deploy an HTTP server on every worker node in your cluster by using a [daemon set ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/). You must implement a health check that is available at the <code>/myhealth</code> path and that can verify whether your HTTP server is healthy. You can define other paths by changing the <code>Route</code> parameter. If the HTTP server is healthy, you must return the HTTP response code that is defined in <code>ExpectedStatus</code>. The HTTP server must be configured to listen on the private IP address of the worker node. You can find the private IP address by running <code>kubectl get nodes</code>.<br></br>
   For example, consider two nodes in a cluster that have the private IP addresses 10.10.10.1 and 10.10.10.2. In this example, two routes are checked for a 200 HTTP response: <code>http://10.10.10.1:80/myhealth</code> and <code>http://10.10.10.2:80/myhealth</code>.
   The check in the example YAML runs every 3 minutes. If it fails three consecutive times, the worker node is rebooted. This action is equivalent to running <code>ibmcloud ks worker reboot</code>.<br></br>The HTTP check is disabled until you set the <b>Enabled</b> field to <code>true</code>.</td>
   </tr>
   </tbody>
   </table>

   <table summary="The columns are read from left to right. The first column has the component of the check. The second column describes the component."
   <caption>Understanding the individual components of checks</caption>
   <thead>
   <col width="25%">
   <th>Component</th>
   <th>Description</th>
   </thead>
   <tbody>
   <tr>
   <td><code>Check</code></td>
   <td>Enter the type of check that you want Autorecovery to use. <ul><li><code>HTTP</code>: Autorecovery calls HTTP servers that run on each node to determine whether the nodes are running properly.</li><li><code>KUBEAPI</code>: Autorecovery calls the Kubernetes API server and reads the health status data reported by the worker nodes.</li></ul></td>
   </tr>
   <tr>
   <td><code>Resource</code></td>
   <td>When the check type is <code>KUBEAPI</code>, enter the type of resource that you want Autorecovery to check. Accepted values are <code>NODE</code> or <code>POD</code>.</td>
   </tr>
   <tr>
   <td><code>FailureThreshold</code></td>
   <td>Enter the threshold for the number of consecutive failed checks. When this threshold is met, Autorecovery triggers the specified corrective action. For example, if the value is 3 and Autorecovery fails a configured check three consecutive times, Autorecovery triggers the corrective action that is associated with the check.</td>
   </tr>
   <tr>
   <td><code>PodFailureThresholdPercent</code></td>
   <td>When the resource type is <code>POD</code>, enter the threshold for the percentage of pods on a worker node that can be in a [<strong><code>NotReady</code></strong> ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/#define-readiness-probes) state. This percentage is based on the total number of pods that are scheduled to a worker node. When a check determines that the percentage of unhealthy pods is greater than the threshold, the check counts as one failure.</td>
   </tr>
   <tr>
   <td><code>CorrectiveAction</code></td>
   <td>Enter the action to run when the failure threshold is met. A corrective action runs only while no other workers are being repaired and when this worker node is not in a cool-off period from a previous action. <ul><li><code>REBOOT</code>: Reboots the worker node.</li><li><code>RELOAD</code>: Reloads all of the necessary configurations for the worker node from a clean OS.</li></ul></td>
   </tr>
   <tr>
   <td><code>CooloffSeconds</code></td>
   <td>Enter the number of seconds Autorecovery must wait to issue another corrective action for a node that was already issued a corrective action. The cool off period starts at the time a corrective action is issued.</td>
   </tr>
   <tr>
   <td><code>IntervalSeconds</code></td>
   <td>Enter the number of seconds in between consecutive checks. For example, if the value is 180, Autorecovery runs the check on each node every 3 minutes.</td>
   </tr>
   <tr>
   <td><code>TimeoutSeconds</code></td>
   <td>Enter the maximum number of seconds that a check call to the database takes before Autorecovery terminates the call operation. The value for <code>TimeoutSeconds</code> must be less than the value for <code>IntervalSeconds</code>.</td>
   </tr>
   <tr>
   <td><code>Port</code></td>
   <td>When the check type is <code>HTTP</code>, enter the port that the HTTP server must bind to on the worker nodes. This port must be exposed on the IP of every worker node in the cluster. Autorecovery requires a constant port number across all nodes for checking servers. Use [daemon sets ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) when you deploy a custom server into a cluster.</td>
   </tr>
   <tr>
   <td><code>ExpectedStatus</code></td>
   <td>When the check type is <code>HTTP</code>, enter the HTTP server status that you expect to be returned from the check. For example, a value of 200 indicates that you expect an <code>OK</code> response from the server.</td>
   </tr>
   <tr>
   <td><code>Route</code></td>
   <td>When the check type is <code>HTTP</code>, enter the path that is requested from the HTTP server. This value is typically the metrics path for the server that runs on all of the worker nodes.</td>
   </tr>
   <tr>
   <td><code>Enabled</code></td>
   <td>Enter <code>true</code> to enable the check or <code>false</code> to disable the check.</td>
   </tr>
   <tr>
   <td><code>Namespace</code></td>
   <td> Optional: To restrict <code>checkpod.json</code> to checking only pods in one namespace, add the <code>Namespace</code> field and enter the namespace.</td>
   </tr>
   </tbody>
   </table>

3. Create the configuration map in your cluster.
    ```
    kubectl apply -f ibm-worker-recovery-checks.yaml
    ```
    {: pre}

4. Verify that you created the configuration map with the name `ibm-worker-recovery-checks` in the `kube-system` namespace with the proper checks.
    ```
    kubectl -n kube-system get cm ibm-worker-recovery-checks -o yaml
    ```
    {: pre}

5. Deploy Autorecovery into your cluster by installing the `ibm-worker-recovery` Helm chart.
    ```
    helm install ibm-worker-recovery iks-charts/ibm-worker-recovery --namespace kube-system
    ```
    {: pre}

6. After a few minutes, you can check the `Events` section in the output of the following command to see activity on the Autorecovery deployment.
    ```
    kubectl -n kube-system describe deployment ibm-worker-recovery
    ```
    {: pre}

7. If you do not see activity on the Autorecovery deployment, you can check the Helm deployment by running the tests that are included in the Autorecovery chart definition.
    ```
    helm test ibm-worker-recovery -n kube-system
    ```
    {: pre}
