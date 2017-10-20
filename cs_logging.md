---

copyright:
  years: 2014, 2017
lastupdated: "2017-10-18"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:codeblock: .codeblock}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}


# Configuring cluster logging
{: #cs_logging}

Logs help you troubleshoot issues with your clusters and apps. You can update logging for your cluster by choosing where your logs are forwarded to, enabling forwarding for various cluster log types, and enabling forwarding for cluster API server audit logs.
{:shortdesc}

## Viewing logs
{: #cs_view_logs}

To view logs for clusters and containers, you can use the standard Kubernetes and Docker logging features.
{:shortdesc}

### {{site.data.keyword.loganalysislong_notm}}
{: #cs_view_logs_k8s}

For standard clusters, logs are located in the {{site.data.keyword.Bluemix_notm}} space you were logged in to when you created the Kubernetes cluster. Container logs are monitored and forwarded outside of the container. You can access logs for a container by using the Kibana dashboard. For more information about logging, see [Logging for the {{site.data.keyword.containershort_notm}}](/docs/services/CloudLogAnalysis/containers/logging_containers_ov.html#logging_containers_ov).

To access the Kibana dashboard, go to one of the following URLs and select the {{site.data.keyword.Bluemix_notm}} organization and space where you created the cluster.
- US-South and US-East: https://logging.ng.bluemix.net
- UK-South: https://logging.eu-gb.bluemix.net
- EU-Central: https://logging.eu-de.bluemix.net

### Docker logs
{: #cs_view_logs_docker}

You can leverage the built-in Docker logging capabilities to review activities on the standard STDOUT and STDERR output streams. For more information, see [Viewing container logs for a container that runs in a Kubernetes cluster](/docs/services/CloudLogAnalysis/containers/logging_containers_other_logs.html#logging_containers_collect_data).

## Updating the logging server configuration
{: #cs_update_server}

By default, the {{site.data.keyword.containerlong}} forwards cluster logs to {{site.data.keyword.loganalysislong_notm}}. You can also configure your cluster to forward logs to an external syslog server.
{:shortdesc}

**Note**: To view logs in the Sydney location, you must configure your cluster to forward logs to an external syslog server.

Before you begin:

1. Set up a server that can accept a syslog protocol. You can run a syslog server in two ways:
  * Set up and manage your own server or have a provider manage it for you. If a provider manages the server for you, get the logging endpoint from the logging provider.
  * Run syslog from a container. For example, you can use this [deployment .yaml file](https://github.com/IBM-Bluemix/kube-samples/blob/master/deploy-apps-clusters/deploy-syslog-from-kube) to fetch a Docker public image that runs a container in a Kubernetes cluster. The image publishes the port `30514` on the public cluster IP address, and uses this public cluster IP address to configure the syslog host.

2. [Target your CLI](cs_cli_install.html#cs_cli_configure) to the cluster for which you want to configure logging.

To configure your cluster to forward logs to a syslog server:

1. Update the logging configuration.

    ```
    bx cs logging-config-update <my_cluster> --hostname <log_server_hostname> --port <log_server_port> --type syslog --namespace <my_namespace>
    ```
    {: pre}

    <table>
    <caption>Table 1. Understanding this command's components</caption>
    <thead>
    <th colspan=2><img src="images/idea.png"/> Understanding this command's components</th>
    </thead>
    <tbody>
    <tr>
    <td><code>logging-config-update</code></td>
    <td>The command to update the log forwarding configuration for your cluster.</td>
    </tr>
    <tr>
    <td><code><em>&lt;my_cluster&gt;</em></code></td>
    <td>Replace <em>&lt;my_cluster&gt;</em> with the name or ID of the cluster.</td>
    </tr>
    <tr>
    <td><code>--hostname <em>&lt;log_server_hostname&gt;</em></code></td>
    <td>Replace <em>&lt;log_server_hostname&gt;</em> with the hostname or IP address of the log collector server.</td>
    </tr>
    <tr>
    <td><code>--port <em>&lt;log_collector_port&gt;</em></code></td>
    <td>Replace <em>&lt;log_server_port&gt;</em> with the port of the log collector server. If you do not specify a port, then the standard port `514` is used for syslog.</td>
    </tr>
    <tr>
    <td><code>--type syslog</code></td>
    <td>The logging type for syslog.</td>
    </tr>
    <tr>
    <td><code>--namespace <em>&lt;my_namespace&gt;</em></code></td>
    <td>Replace <em>&lt;my_namespace&gt;</em> with the name of the namespace to which you want to apply the log forwarding configuration. Log forwarding is not supported for the 'ibm-system' and 'kube-system' Kubernetes namespaces. If you do not specify a namespace, then all namespaces in the cluster use this configuration.</td>
    </tr>
    </tbody></table>

2. Verify that the log forwarding configuration was created.

    ```
    bx cs logging-config-get <my_cluster>
    ```
    {: pre}

    Example output:

    ```
    Namespace         Host             Port   Protocol   
    mykubenamespace   myhostname.com   514    syslog   
    ```
    {: screen}

To stop forwarding cluster logs to a syslog server:

1. Delete the logging configuration. Replace <em>&lt;my_cluster&gt;</em> with the name of the cluster that the logging configuration is in and <em>&lt;my_namespace&gt;</em> with the name of the namespace from which the logs are forwarded.

    ```
    bx cs logging-config-rm <my_cluster> --namespace <my_namespace>
    ```
    {: pre}

    **Note**: This action deletes only the configuration for forwarding logs to a syslog server. Logs for the namespace continue to be forwarded to {{site.data.keyword.loganalysislong_notm}}.
