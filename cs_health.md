---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-02"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}


# Logging and monitoring clusters
{: #health}

Configure cluster logging and monitoring to help you troubleshoot issues with your clusters and apps and monitor the health and performance of your clusters.
{:shortdesc}

## Configuring cluster logging
{: #logging}

You can send logs to a specific location for processing or long-term storage. On a Kubernetes cluster in {{site.data.keyword.containershort_notm}}, you can enable log forwarding for your cluster and choose where your logs are forwarded. **Note**: Log forwarding is supported only for standard clusters.
{:shortdesc}

You can forward logs for log sources such as containers, applications, worker nodes, Kubernetes clusters, and Ingress controllers. Review the following table for information about each log source.

|Log source|Characteristics|Log paths|
|----------|---------------|-----|
|`container`|Logs for your container that runs in a Kubernetes cluster.|-|
|`application`|Logs for your own application that runs in a Kubernetes cluster.|`/var/log/apps/**/*.log`, `/var/log/apps/**/*.err`|
|`worker`|Logs for virtual machine worker nodes within a Kubernetes cluster.|`/var/log/syslog`, `/var/log/auth.log`|
|`kubernetes`|Logs for the Kubernetes system component.|`/var/log/kubelet.log`, `/var/log/kube-proxy.log`|
|`ingress`|Logs for an application load balancer, managed by the Ingress controller, that manages network traffic coming into a Kubernetes cluster.|`/var/log/alb/ids/*.log`, `/var/log/alb/ids/*.err`, `/var/log/alb/customerlogs/*.log`, `/var/log/alb/customerlogs/*.err`|
{: caption="Log source characteristics" caption-side="top"}

## Enabling log forwarding
{: #log_sources_enable}

You can forward logs to {{site.data.keyword.loganalysislong_notm}} or to an external syslog server. If you want to forward logs from one log source to both log collector servers, then you must create two logging configurations. **Note**: To forward logs for applications, see [Enabling log forwarding for applications](#apps_enable).
{:shortdesc}

Before you begin:

1. If you want to forward logs to an external syslog server, you can set up a server that accepts a syslog protocol in two ways:
  * Set up and manage your own server or have a provider manage it for you. If a provider manages the server for you, get the logging endpoint from the logging provider.
  * Run syslog from a container. For example, you can use this [deployment .yaml file ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/IBM-Bluemix/kube-samples/blob/master/deploy-apps-clusters/deploy-syslog-from-kube.yaml) to fetch a Docker public image that runs a container in a Kubernetes cluster. The image publishes the port `514` on the public cluster IP address, and uses this public cluster IP address to configure the syslog host.

2. [Target your CLI](cs_cli_install.html#cs_cli_configure) to the cluster where the log source is located.

To enable log forwarding for a container, worker node, Kubernetes system component, application, or Ingress application load balancer:

1. Create a log forwarding configuration.

  * To forward logs to {{site.data.keyword.loganalysislong_notm}}:

    ```
    bx cs logging-config-create <my_cluster> --logsource <my_log_source> --namespace <kubernetes_namespace> --hostname <ingestion_URL> --port <ingestion_port> --space <cluster_space> --org <cluster_org> --type ibm
    ```
    {: pre}

    <table>
    <caption>Understanding this command's components</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding this command's components</th>
    </thead>
    <tbody>
    <tr>
    <td><code>logging-config-create</code></td>
    <td>The command to create an {{site.data.keyword.loganalysislong_notm}} log forwarding configuration.</td>
    </tr>
    <tr>
    <td><code><em>&lt;my_cluster&gt;</em></code></td>
    <td>Replace <em>&lt;my_cluster&gt;</em> with the name or ID of the cluster.</td>
    </tr>
    <tr>
    <td><code>--logsource <em>&lt;my_log_source&gt;</em></code></td>
    <td>Replace <em>&lt;my_log_source&gt;</em> with the log source. Accepted values are <code>container</code>, <code>application</code>, <code>worker</code>, <code>kubernetes</code>, and <code>ingress</code>.</td>
    </tr>
    <tr>
    <td><code><em>&lt;kubernetes_namespace&gt;</em></code></td>
    <td>Replace <em>&lt;kubernetes_namespace&gt;</em> with the Kubernetes namespace that you want to forward logs from. Log forwarding is not supported for the <code>ibm-system</code> and <code>kube-system</code> Kubernetes namespaces. This value is valid only for the container log source and is optional. If you do not specify a namespace, then all namespaces in the cluster use this configuration.</td>
    </tr>
    <tr>
    <td><code>--hostname <em>&lt;ingestion_URL&gt;</em></code></td>
    <td>Replace <em>&lt;ingestion_URL&gt;</em> with the {{site.data.keyword.loganalysisshort_notm}} ingestion URL. You can find the list of available ingestion URLs [here](/docs/services/CloudLogAnalysis/log_ingestion.html#log_ingestion_urls). If you do not specify an ingestion URL, the endpoint for the region where your cluster was created is used.</td>
    </tr>
    <tr>
    <td><code>--port <em>&lt;ingestion_port&gt;</em></code></td>
    <td>Replace <em>&lt;ingestion_port&gt;</em> with the ingestion port. If you do not specify a port, then the standard port <code>9091</code> is used.</td>
    </tr>
    <tr>
    <td><code>--space <em>&lt;cluster_space&gt;</em></code></td>
    <td>Replace <em>&lt;cluster_space&gt;</em> with the name of the Cloud Foundry space that you want to send logs to. If you do not specify a space, logs are sent to the account level.</td>
    </tr>
    <tr>
    <td><code>--org <em>&lt;cluster_org&gt;</em></code></td>
    <td>Replace <em>&lt;cluster_org&gt;</em> with the name of the Cloud Foundry org that the space is in. This value is required if you specified a space.</td>
    </tr>
    <tr>
    <td><code>--type ibm</code></td>
    <td>The log type for sending logs to {{site.data.keyword.loganalysisshort_notm}}.</td>
    </tr>
    </tbody></table>

  * To forward logs to an external syslog server:

    ```
    bx cs logging-config-create <my_cluster> --logsource <my_log_source> --namespace <kubernetes_namespace> --hostname <log_server_hostname_or_IP> --port <log_server_port> --type syslog
    ```
    {: pre}

    <table>
    <caption>Understanding this command's components</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding this command's components</th>
    </thead>
    <tbody>
    <tr>
    <td><code>logging-config-create</code></td>
    <td>The command to create a syslog log forwarding configuration.</td>
    </tr>
    <tr>
    <td><code><em>&lt;my_cluster&gt;</em></code></td>
    <td>Replace <em>&lt;my_cluster&gt;</em> with the name or ID of the cluster.</td>
    </tr>
    <tr>
    <td><code>--logsource <em>&lt;my_log_source&gt;</em></code></td>
    <td>Replace <em>&lt;my_log_source&gt;</em> with the log source. Accepted values are <code>container</code>, <code>application</code>, <code>worker</code>, <code>kubernetes</code>, and <code>ingress</code>.</td>
    </tr>
    <tr>
    <td><code><em>&lt;kubernetes_namespace&gt;</em></code></td>
    <td>Replace <em>&lt;kubernetes_namespace&gt;</em> with the Kubernetes namespace that you want to forward logs from. Log forwarding is not supported for the <code>ibm-system</code> and <code>kube-system</code> Kubernetes namespaces. This value is valid only for the container log source and is optional. If you do not specify a namespace, then all namespaces in the cluster use this configuration.</td>
    </tr>
    <tr>
    <td><code>--hostname <em>&lt;log_server_hostname_or_IP&gt;</em></code></td>
    <td>Replace <em>&lt;log_server_hostname&gt;</em> with the hostname or IP address of the log collector service.</td>
    </tr>
    <tr>
    <td><code>--port <em>&lt;log_server_port&gt;</em></code></td>
    <td>Replace <em>&lt;log_server_port&gt;</em> with the port of the log collector server. If you do not specify a port, then the standard port <code>514</code> is used.</td>
    </tr>
    <tr>
    <td><code>--type syslog</code></td>
    <td>The log type for sending logs to an external syslog server.</td>
    </tr>
    </tbody></table>

2. Verify that the log forwarding configuration was created.

    * To list all the logging configurations in the cluster:
      ```
      bx cs logging-config-get <my_cluster>
      ```
      {: pre}

      Example output:

      ```
      Id                                    Source       Namespace     Host                          Port   Org      Space      Protocol     Paths
      f4bc77c0-ee7d-422d-aabf-a4e6b977264e  kubernetes   -             172.30.162.138                5514   -        -          syslog       /var/log/kubelet.log,/var/log/kube-proxy.log
      5bd9c609-13c8-4c48-9d6e-3a6664c825a9  application  -             ingest.logging.ng.bluemix.net 9091   my_org   my_space   ibm          /var/log/apps/**/*.log,/var/log/apps/**/*.err
      8a284f1a-451c-4c48-b1b4-a4e6b977264e  containers   my-namespace  myhostname.common             5514   -        -          syslog       -
      ```
      {: screen}

    * To list logging configurations for one type of log source:
      ```
      bx cs logging-config-get <my_cluster> --logsource worker
      ```
      {: pre}

      Example output:

      ```
      Id                                    Source    Namespace   Host                            Port   Org    Space     Protocol    Paths
      f4bc77c0-ee7d-422d-aabf-a4e6b977264e  worker    -           ingest.logging.ng.bluemix.net   9091   -      -         ibm         /var/log/syslog,/var/log/auth.log
      5bd9c609-13c8-4c48-9d6e-3a6664c825a9  worker    -           172.30.162.138                  5514   -      -         syslog      /var/log/syslog,/var/log/auth.log
      ```
      {: screen}


### Enabling log forwarding for applications
{: #apps_enable}

Logs from applications must be constrained to a specific directory on the host node. You can do this by mounting a host path volume to your containers with a mount path. This mount path serves as the directory on your containers where application logs are sent. The predefined host path directory, `/var/log/apps`, is automatically created when you create the volume mount.

Review the following aspects of application log forwarding:
* Logs are read recursively from the /var/log/apps path. This means that you can put application logs in subdirectories of the /var/log/apps path.
* Only application log files with `.log` or `.err` file extensions are forwarded.
* When you enable log forwarding for the first time, application logs are tailed instead of being read from head. This means that the contents of any logs already present before application logging was enabled are not read. The logs are read from the point that logging was enabled. However, after the first time that log forwarding is enabled, logs are always picked up from where they last left off.
* When you mount the `/var/log/apps` host path volume to containers, the containers all write to this same directory. This means that if your containers are writing to the same file name, the containers will write to the exact same file on the host. If this is not your intention, you can prevent your containers from overwriting the same log files by naming the log files from each container differently.
* Because all containers write to the same file name, do not use this method to forward application logs for ReplicaSets. Instead, you can write logs from the application to STDOUT and STDERR, which are picked up as container logs. To forward application logs written to STDOUT and STDERR, follow the steps in [Enabling log forwarding](cs_health.html#log_sources_enable).

Before you start, [target your CLI](cs_cli_install.html#cs_cli_configure) to the cluster where the log source is located.

1. Open the `.yaml` configuration file for the application's pod.

2. Add the following `volumeMounts` and `volumes` to the configuration file:

    ```
    apiVersion: v1
    kind: Pod
    metadata:
      name: <pod_name>
    containers:
    - name: fluentd
      image: "<your_registry_image>"
      volumeMounts:
        # Docker paths
          - mountPath: /var/log/my-app-log-files-directory
            name: app-log
    volumes:
    - name: app-log
      hostPath:
        path: /var/log/apps
    ```
    {: codeblock}

2. Mount the volume to the pod.

    ```
    kubectl apply -f <local_yaml_path>
    ```
    {:pre}

3. To create a log forwarding configuration, follow the steps in [Enabling log forwarding](cs_health.html#log_sources_enable).

<br />


## Updating the log forwarding configuration
{: #log_sources_update}

You can update a logging configuration for a container, application, worker node, Kubernetes system component, or Ingress application load balancer.
{: shortdesc}

Before you begin:

1. If you are changing the log collector server to syslog, you can set up a server that accepts a syslog protocol in two ways:
  * Set up and manage your own server or have a provider manage it for you. If a provider manages the server for you, get the logging endpoint from the logging provider.
  * Run syslog from a container. For example, you can use this [deployment .yaml file ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/IBM-Bluemix/kube-samples/blob/master/deploy-apps-clusters/deploy-syslog-from-kube.yaml) to fetch a Docker public image that runs a container in a Kubernetes cluster. The image publishes the port `514` on the public cluster IP address, and uses this public cluster IP address to configure the syslog host.

2. [Target your CLI](cs_cli_install.html#cs_cli_configure) to the cluster where the log source is located.

To change the details of a logging configuration:

1. Update the logging configuration.

    ```
    bx cs logging-config-update <my_cluster> --id <log_config_id> --logsource <my_log_source> --hostname <log_server_hostname_or_IP> --port <log_server_port> --space <cluster_space> --org <cluster_org> --type <logging_type>
    ```
    {: pre}

    <table>
    <caption>Understanding this command's components</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding this command's components</th>
    </thead>
    <tbody>
    <tr>
    <td><code>logging-config-update</code></td>
    <td>The command to update the log forwarding configuration for your log source.</td>
    </tr>
    <tr>
    <td><code><em>&lt;my_cluster&gt;</em></code></td>
    <td>Replace <em>&lt;my_cluster&gt;</em> with the name or ID of the cluster.</td>
    </tr>
    <tr>
    <td><code>--id <em>&lt;log_config_id&gt;</em></code></td>
    <td>Replace <em>&lt;log_config_id&gt;</em> with the ID of the log source configuration.</td>
    </tr>
    <tr>
    <td><code>--logsource <em>&lt;my_log_source&gt;</em></code></td>
    <td>Replace <em>&lt;my_log_source&gt;</em> with the log source. Accepted values are <code>container</code>, <code>application</code>, <code>worker</code>, <code>kubernetes</code>, and <code>ingress</code>.</td>
    </tr>
    <tr>
    <td><code>--hostname <em>&lt;log_server_hostname_or_IP&gt;</em></code></td>
    <td>When the logging type is <code>syslog</code>, replace <em>&lt;log_server_hostname_or_IP&gt;</em> with the hostname or IP address of the log collector service. When the logging type is <code>ibm</code>, replace <em>&lt;log_server_hostname&gt;</em> with the {{site.data.keyword.loganalysislong_notm}} ingestion URL. You can find the list of available ingestion URLs [here](/docs/services/CloudLogAnalysis/log_ingestion.html#log_ingestion_urls). If you do not specify an ingestion URL, the endpoint for the region where your cluster was created is used.</td>
    </tr>
    <tr>
    <td><code>--port <em>&lt;log_collector_port&gt;</em></code></td>
    <td>Replace <em>&lt;log_server_port&gt;</em> with the port of the log collector server. If you do not specify a port, then the standard port <code>514</code> is used for <code>syslog</code> and <code>9091</code> is used for <code>ibm</code>.</td>
    </tr>
    <tr>
    <td><code>--space <em>&lt;cluster_space&gt;</em></code></td>
    <td>Replace <em>&lt;cluster_space&gt;</em> with the name of the Cloud Foundry space that you want to send logs to. This value is valid only for log type <code>ibm</code> and is optional. If you do not specify a space, logs are sent to the account level.</td>
    </tr>
    <tr>
    <td><code>--org <em>&lt;cluster_org&gt;</em></code></td>
    <td>Replace <em>&lt;cluster_org&gt;</em> with the name of the Cloud Foundry org that the space is in. This value is valid only for log type <code>ibm</code> and is required if you specified a space.</td>
    </tr>
    <tr>
    <td><code>--type <em>&lt;logging_type&gt;</em></code></td>
    <td>Replace <em>&lt;logging_type&gt;</em> with the new log forwarding protocol you want to use. Currently, <code>syslog</code> and <code>ibm</code> are supported.</td>
    </tr>
    </tbody></table>

2. Verify that the log forwarding configuration was updated.

    * To list all the logging configurations in the cluster:

      ```
      bx cs logging-config-get <my_cluster>
      ```
      {: pre}

      Example output:

      ```
      Id                                    Source       Namespace     Host                          Port   Org      Space      Protocol     Paths
      f4bc77c0-ee7d-422d-aabf-a4e6b977264e  kubernetes   -             172.30.162.138                5514   -        -          syslog       /var/log/kubelet.log,/var/log/kube-proxy.log
      5bd9c609-13c8-4c48-9d6e-3a6664c825a9  application  -             ingest.logging.ng.bluemix.net 9091   my_org   my_space   ibm          /var/log/apps/**/*.log,/var/log/apps/**/*.err
      8a284f1a-451c-4c48-b1b4-a4e6b977264e  containers   my-namespace  myhostname.common             5514   -        -          syslog       -
      ```
      {: screen}

    * To list logging configurations for one type of log source:

      ```
      bx cs logging-config-get <my_cluster> --logsource worker
      ```
      {: pre}

      Example output:

      ```
      Id                                    Source    Namespace   Host                            Port   Org    Space     Protocol    Paths
      f4bc77c0-ee7d-422d-aabf-a4e6b977264e  worker    -           ingest.logging.ng.bluemix.net   9091   -      -         ibm         /var/log/syslog,/var/log/auth.log
      5bd9c609-13c8-4c48-9d6e-3a6664c825a9  worker    -           172.30.162.138                  5514   -      -         syslog      /var/log/syslog,/var/log/auth.log
      ```

      {: screen}

<br />


## Stopping log forwarding
{: #log_sources_delete}

You can stop forwarding logs by deleting the logging configuration.

Before you begin, [target your CLI](cs_cli_install.html#cs_cli_configure) to your cluster where the log source is located.

1. Delete the logging configuration.

    ```
    bx cs logging-config-rm <my_cluster> --id <log_config_id>
    ```
    {: pre}
    Replace <em>&lt;my_cluster&gt;</em> with the name of the cluster that the logging configuration is in and <em>&lt;log_config_id&gt;</em> with the ID of the log source configuration.

<br />


## Configuring log forwarding for Kubernetes API audit logs
{: #app_forward}

Kubernetes API audit logs capture any calls to the Kubernetes API server from your cluster. To start collecting Kubernetes API audit logs, you can configure the Kubernetes API server to set up a webhook backend for your cluster. This webhook backend enables logs to be sent to a remote server. For more information about Kubernetes audit logs, see the <a href="https://kubernetes.io/docs/tasks/debug-application-cluster/audit/" target="_blank">auditing topic <img src="../icons/launch-glyph.svg" alt="External link icon"></a> in the Kubernetes documentation.

**Note**:
* Forwarding for Kubernetes API audit logs is only supported for Kubernetes version 1.7 and newer.
* Currently, a default audit policy is used for all clusters with this logging configuration.

### Enabling Kubernetes API audit log forwarding
{: #audit_enable}

Before you begin:

1. Set up a remote logging server where you can forward the logs. For example, you can [use Logstash with Kubernetes ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/tasks/debug-application-cluster/audit/#use-logstash-to-collect-and-distribute-audit-events-from-webhook-backend) to collect audit events.

2. [Target your CLI](cs_cli_install.html#cs_cli_configure) to the cluster that you want to collect API server audit logs from.

To forward Kubernetes API audit logs:

1. Set the webhook backend for the API server configuration. A webhook configuration is created based on the information you provide in this command's flags. If you do not provide any information in the flags, a default webhook configuration is used.

    ```
    bx cs apiserver-config-set audit-webhook <my_cluster> --remoteServer <server_URL_or_IP> --caCert <CA_cert_path> --clientCert <client_cert_path> --clientKey <client_key_path>
    ```
    {: pre}

    <table>
    <caption>Understanding this command's components</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding this command's components</th>
    </thead>
    <tbody>
    <tr>
    <td><code>apiserver-config-set</code></td>
    <td>The command to set an option for the cluster's Kubernetes API server configuration.</td>
    </tr>
    <tr>
    <td><code>audit-webhook</code></td>
    <td>The subcommand to set the audit webhook configuration for the cluster's Kubernetes API server.</td>
    </tr>
    <tr>
    <td><code><em>&lt;my_cluster&gt;</em></code></td>
    <td>Replace <em>&lt;my_cluster&gt;</em> with the name or ID of the cluster.</td>
    </tr>
    <tr>
    <td><code>--remoteServer <em>&lt;server_URL&gt;</em></code></td>
    <td>Replace <em>&lt;server_URL&gt;</em> with the URL or IP address for the remote logging service you want to send logs to. If you provide an insecure serverURL, any certificates are ignored.</td>
    </tr>
    <tr>
    <td><code>--caCert <em>&lt;CA_cert_path&gt;</em></code></td>
    <td>Replace <em>&lt;CA_cert_path&gt;</em> with the filepath for the CA certificate that is used to verify the remote logging service.</td>
    </tr>
    <tr>
    <td><code>--clientCert <em>&lt;client_cert_path&gt;</em></code></td>
    <td>Replace <em>&lt;client_cert_path&gt;</em> with the filepath for the client certificate that is used to authenticate against the remote logging service.</td>
    </tr>
    <tr>
    <td><code>--clientKey <em>&lt;client_key_path&gt;</em></code></td>
    <td>Replace <em>&lt;client_key_path&gt;</em> with the filepath for the corresponding client key that is used to connect to the remote logging service.</td>
    </tr>
    </tbody></table>

2. Verify that the log forwarding was enabled by viewing the URL for the remote logging service.

    ```
    bx cs apiserver-config-get audit-webhook <my_cluster>
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
    bx cs apiserver-refresh <my_cluster>
    ```
    {: pre}

### Stopping Kubernetes API audit log forwarding
{: #audit_delete}

You can stop forwarding audit logs by disabling the webhook backend configuration for the cluster's API server.

Before you begin, [target your CLI](cs_cli_install.html#cs_cli_configure) to the cluster that you want to stop collecting API server audit logs from.

1. Disable the webhook backend configuration for the cluster's API server.

    ```
    bx cs apiserver-config-unset audit-webhook <my_cluster>
    ```
    {: pre}

2. Apply the configuration update by restarting the Kubernetes master.

    ```
    bx cs apiserver-refresh <my_cluster>
    ```
    {: pre}

<br />


## Viewing logs
{: #view_logs}

To view logs for clusters and containers, you can use the standard Kubernetes and Docker logging features.
{:shortdesc}

### {{site.data.keyword.loganalysislong_notm}}
{: #view_logs_k8s}

For standard clusters, logs are located in the {{site.data.keyword.Bluemix_notm}} account you were logged in to when you created the Kubernetes cluster. If you specified an {{site.data.keyword.Bluemix_notm}} space when you created the cluster or when you created the logging configuration, then logs are located in that space. Logs are monitored and forwarded outside of the container. You can access logs for a container using the Kibana dashboard. For more information about logging, see [Logging for the {{site.data.keyword.containershort_notm}}](/docs/services/CloudLogAnalysis/containers/containers_kubernetes.html#containers_kubernetes).

**Note**: If you specified a space when you created the cluster or the logging configuration, then the account owner needs Manager, Developer, or Auditor permissions to that space to view logs. For more information about changing {{site.data.keyword.containershort_notm}} access policies and permissions, see [Managing cluster access](cs_users.html#managing). Once permissions are changed, it can take up to 24 hours for logs to start appearing.

To access the Kibana dashboard, go to one of the following URLs and select the {{site.data.keyword.Bluemix_notm}} account or space where you created the cluster.
- US-South and US-East: https://logging.ng.bluemix.net
- UK-South and EU-Central: https://logging.eu-fra.bluemix.net
- AP-South: https://logging.au-syd.bluemix.net

For more information about viewing logs, see [Navigating to Kibana from a web browser](/docs/services/CloudLogAnalysis/kibana/launch.html#launch_Kibana_from_browser).

### Docker logs
{: #view_logs_docker}

You can leverage the built-in Docker logging capabilities to review activities on the standard STDOUT and STDERR output streams. For more information, see [Viewing container logs for a container that runs in a Kubernetes cluster](/docs/services/CloudLogAnalysis/containers/containers_kubernetes.html#containers_kubernetes).

<br />


## Configuring cluster monitoring
{: #monitoring}

Metrics help you monitor the health and performance of your clusters. You can configure health monitoring for worker nodes to automatically detect and correct any workers that enter a degraded or nonoperational state. **Note**: Monitoring is supported only for standard clusters.
{:shortdesc}

## Viewing metrics
{: #view_metrics}

You can use the standard Kubernetes and Docker features to monitor the health of your clusters and apps.
{:shortdesc}

<dl>
<dt>Cluster details page in {{site.data.keyword.Bluemix_notm}}</dt>
<dd>{{site.data.keyword.containershort_notm}} provides information about the health and capacity of your cluster and the usage of your cluster resources. You can use this GUI to scale out your cluster, work with your persistent storage, and add more capabilities to your cluster through {{site.data.keyword.Bluemix_notm}} service binding. To view the cluster details page, go to your **{{site.data.keyword.Bluemix_notm}} Dashboard** and select a cluster.</dd>
<dt>Kubernetes dashboard</dt>
<dd>The Kubernetes dashboard is an administrative web interface where you can review the health of your worker nodes, find Kubernetes resources, deploy containerized apps, and troubleshoot apps using logging and monitoring information. For more information about how to access your Kubernetes dashboard, see [Launching the Kubernetes dashboard for {{site.data.keyword.containershort_notm}}](cs_app.html#cli_dashboard).</dd>
<dt>{{site.data.keyword.monitoringlong_notm}}</dt>
<dd>Metrics for standard clusters are located in the {{site.data.keyword.Bluemix_notm}} account that was logged in to when the Kubernetes cluster was created. If you specified an {{site.data.keyword.Bluemix_notm}} space when you created the cluster, then metrics are located in that space. Container metrics are collected automatically for all containers that are deployed in a cluster. These metrics are sent and are made available through Grafana. For more information about metrics, see [Monitoring for the {{site.data.keyword.containershort_notm}}](/docs/services/cloud-monitoring/containers/monitoring_containers_ov.html#monitoring_bmx_containers_ov).<p>To access the Grafana dashboard, go to one of the following URLs and select the {{site.data.keyword.Bluemix_notm}} account or space where you created the cluster.<ul><li>US-South and US-East: https://metrics.ng.bluemix.net</li><li>UK-South: https://metrics.eu-gb.bluemix.net</li></ul></p></dd></dl>

### Other health monitoring tools
{: #health_tools}

You can configure other tools for more monitoring capabilities.
<dl>
<dt>Prometheus</dt>
<dd>Prometheus is an open source monitoring, logging, and alerting tool that was designed for Kubernetes. The tool retrieves detailed information about the cluster, worker nodes, and deployment health based on the Kubernetes logging information. For setup information, see [Integrating services with {{site.data.keyword.containershort_notm}}](cs_integrations.html#integrations).</dd>
</dl>

<br />


## Configuring health monitoring for worker nodes with Autorecovery
{: #autorecovery}

The {{site.data.keyword.containerlong_notm}} Autorecovery system can be deployed into existing clusters of Kubernetes version 1.7 or later. The Autorecovery system uses various checks to query worker node health status. If Autorecovery detects an unhealthy worker node based on the configured checks, Autorecovery triggers a corrective action like an OS reload on the worker node. Only one worker node undergoes a corrective action at a time. The worker node must successfully complete the corrective action before any other worker node undergoes a corrective action. For more information, see this [Autorecovery blog post ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/blogs/bluemix/2017/12/autorecovery-utilizes-consistent-hashing-high-availability/).
**NOTE**: Autorecovery requires at least one healthy node to function properly. Configure Autorecovery with active checks only in clusters with two or more worker nodes.

Before you begin, [target your CLI](cs_cli_install.html#cs_cli_configure) to the cluster where you want to check worker node statuses.

1. Create a configuration map file that defines your checks in JSON format. For example, the following YAML file defines three checks: an HTTP check and two Kubernetes API server checks. **Note**: Each check needs to be defined as a unique key in the data section of the config map.

    ```
    kind: ConfigMap
    apiVersion: v1
    metadata:
      name: ibm-worker-recovery-checks
      namespace: kube-system
    data:
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
    ```
    {:codeblock}


<table summary="Understanding the Config Map Components">
<caption>Understanding the config map components</caption>
<thead>
<th colspan=2><img src="images/idea.png"/>Understanding the config map components</th>
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
<td><code>checkhttp.json</code></td>
<td>Defines an HTTP check that checks that an HTTP server is running on every node's IP address on port 80 and returns a 200 response at path <code>/myhealth</code>. You can find the IP address for a node by running <code>kubectl get nodes</code>.
For example, consider two nodes in a cluster that have IP adresses of 10.10.10.1 and 10.10.10.2. In this example, two routes are checked for 200 OK responses: <code>http://10.10.10.1:80/myhealth</code> and <code>http://10.10.10.2:80/myhealth</code>.
The check in the above example YAML runs every 3 minutes. If it fails 3 consecutive times, the node is rebooted. This action is equivalent to running <code>bx cs worker-reboot</code>. The HTTP check is disabled until you set the <b>Enabled</b> field to <code>true</code>.</td>
</tr>
<tr>
<td><code>checknode.json</code></td>
<td>Defines a Kubernetes API node check that checks whether each node is in the <code>Ready</code> state. The check for a specific node counts as a failure if the node is not in the <code>Ready</code> state.
The check in the above example YAML runs every 3 minutes. If it fails 3 consecutive times, the node is reloaded. This action is equivalent to running <code>bx cs worker-reload</code>. The node check is enabled until you set the <b>Enabled</b> field to <code>false</code> or remove the check.</td>
</tr>
<tr>
<td><code>checkpod.json</code></td>
<td>Defines a Kubernetes API pod check that checks the total percentage of <code>NotReady</code> pods on a node based on the total pods assigned to that node. The check for a specific node counts as a failure if the total percentage of <code>NotReady</code> pods is greater than the defined <code>PodFailureThresholdPercent</code>.
The check in the above example YAML runs every 3 minutes. If it fails 3 consecutive times, the node is reloaded. This action is equivalent to running <code>bx cs worker-reload</code>. The pod check is enabled until you set the <b>Enabled</b> field to <code>false</code> or remove the check.</td>
</tr>
</tbody>
</table>


<table summary="Understanding the Components of Individual Rules">
<caption>Understanding the components of individual rules</caption>
<thead>
<th colspan=2><img src="images/idea.png"/>Understanding the individual rule components </th>
</thead>
<tbody>
<tr>
<td><code>Check</code></td>
<td>Enter the type of check that you want Autorecovery to use. <ul><li><code>HTTP</code>: Autorecovery calls HTTP servers that run on each node to determine whether the nodes are running properly.</li><li><code>KUBEAPI</code>: Autorecovery calls the Kubernetes API server and reads the health status data reported by the worker nodes.</li></ul></td>
</tr>
<tr>
<td><code>Resource</code></td>
<td>When the check type is <code>KUBEAPI</code>, enter the type of resource that you want Autorecovery to check. Accepted values are <code>NODE</code> or <code>PODS</code>.</td>
</tr>
<tr>
<td><code>FailureThreshold</code></td>
<td>Enter the threshold for the number of consecutive failed checks. When this threshold is met, Autorecovery triggers the specified corrective action. For example, if the value is 3 and Autorecovery fails a configured check three consecutive times, Autorecovery triggers the corrective action that is associated with the check.</td>
</tr>
<tr>
<td><code>PodFailureThresholdPercent</code></td>
<td>When the resource type is <code>PODS</code>, enter the threshold for the percentage of pods on a worker node that can be in a [NotReady ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/#define-readiness-probes) state. This percentage is based on the total number of pods that are scheduled to a worker node. When a check determines that the percentage of unhealthy pods is greater than the threshold, the check counts as one failure.</td>
</tr>
<tr>
<td><code>CorrectiveAction</code></td>
<td>Enter the action to run when the failure threshold is met. A corrective action runs only while no other workers are being repaired and when this worker node is not in a cool-off period from a previous action. <ul><li><code>REBOOT</code>: Reboots the worker node.</li><li><code>RELOAD</code>: Reloads all of the necessary configurations for the worker node from a clean OS.</li></ul></td>
</tr>
<tr>
<td><code>CooloffSeconds</code></td>
<td>Enter the number of seconds Autorecovery must wait to issue another corrective action for a node that was already issued a corrective action. The cooloff period starts at the time a corrective action is issued.</td>
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
<td>When the check type is <code>HTTP</code>, enter the port that the HTTP server must bind to on the worker nodes. This port must be exposed on the IP of every worker node in the cluster. Autorecovery requires a constant port number across all nodes for checking servers. Use [DaemonSets ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/)when deploying a custom server into a cluster.</td>
</tr>
<tr>
<td><code>ExpectedStatus</code></td>
<td>When the check type is <code>HTTP</code>, enter the HTTP server status that you expect to be returned from the check. For example, a value of 200 indicates that you expect an <code>OK</code> response from the server.</td>
</tr>
<tr>
<td><code>Route</code></td>
<td>When the check type is <code>HTTP</code>, enter the path that is requested from the HTTP server. This value is typically the metrics path for the server that is running on all of the worker nodes.</td>
</tr>
<tr>
<td><code>Enabled</code></td>
<td>Enter <code>true</code> to enable the check or <code>false</code> to disable the check.</td>
</tr>
</tbody>
</table>

2. Create the configuration map in your cluster.

    ```
    kubectl apply -f <my_file.yaml>
    ```
    {: pre}

3. Verify that you created the configuration map with the name `ibm-worker-recovery-checks` in the `kube-system` namespace with the proper checks.

    ```
    kubectl -n kube-system get cm ibm-worker-recovery-checks -o yaml
    ```
    {: pre}

4. Ensure that you created a Docker pull secret with the name `international-registry-docker-secret` in the `kube-system` namespace. Autorecovery is hosted in {{site.data.keyword.registryshort_notm}}'s global Docker registry. If you have not created a Docker registry secret that contains valid credentials for the global registry, create one to run the Autorecovery system.

    1. Install the {{site.data.keyword.registryshort_notm}} plug-in.

        ```
        bx plugin install container-registry -r Bluemix
        ```
        {: pre}

    2. Target the global registry.

        ```
        bx cr region-set global
        ```
        {: pre}

    3. Create a registry token for the global registry.

        ```
        bx cr token-add --non-expiring --description globalRegistryToken
        ```
        {: pre}

    4. Set the `GLOBAL_REGISTRY_TOKEN` environment variable to the token you created.

        ```
        GLOBAL_REGISTRY_TOKEN=$(bx cr token-get $(bx cr tokens | grep globalRegistryToken | awk '{print $1}') -q)
        ```
        {: pre}

    5. Set the `DOCKER_EMAIL` environment variable to the current user. Your email address is needed only to run the `kubectl` command in the next step.

        ```
        DOCKER_EMAIL=$(bx target | grep "User" | awk '{print $2}')
        ```
        {: pre}

    6. Create the Docker pull secret.

        ```
        kubectl -n kube-system create secret docker-registry international-registry-docker-secret --docker-username=token --docker-password="$GLOBAL_REGISTRY_TOKEN" --docker-server=registry.bluemix.net --docker-email="$DOCKER_EMAIL"
        ```
        {: pre}

5. Deploy Autorecovery into your cluster by applying this YAML file.

   ```
   kubectl apply -f https://raw.githubusercontent.com/IBM-Bluemix/kube-samples/master/ibm-worker-recovery/ibm-worker-recovery.yml
   ```
   {: pre}

6. After a few minutes, you can check the `Events` section in the output of the following command to see activity on the Autorecovery deployment.

    ```
    kubectl -n kube-system describe deployment ibm-worker-recovery
    ```
    {: pre}
