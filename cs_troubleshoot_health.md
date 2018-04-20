---

copyright:
  years: 2014, 2018
lastupdated: "2018-04-18"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}


# Troubleshooting logging and monitoring
{: #cs_troubleshoot_health}

As you use {{site.data.keyword.containerlong}}, consider these techniques for troubleshooting issues with logging and monitoring.
{: shortdesc}

If you have a more general issue, try out [cluster debugging](cs_troubleshoot.html).
{: tip}

## Logs do not appear
{: #cs_no_logs}

{: tsSymptoms}
When you access the Kibana dashboard, your logs do not display.

{: tsResolve}
Review the following reasons why your cluster logs are not appearing and the corresponding troubleshooting steps:

<table>
  <col width="40%">
  <col width="60%">
  <thead>
    <tr>
      <th>Why it's happening</th>
      <th>How to fix it</th>
    </tr>
 </thead>
 <tbody>
  <tr>
    <td>No logging configuration is set up.</td>
    <td>In order for logs to be sent, you must create a logging configuration. To do so, see <a href="cs_health.html#logging">Configuring cluster logging</a>.</td>
  </tr>
  <tr>
    <td>The cluster is not in a <code>Normal</code> state.</td>
    <td>To check the state of your cluster, see <a href="cs_troubleshoot.html#debug_clusters">Debugging clusters</a>.</td>
  </tr>
  <tr>
    <td>The log storage quota has been hit.</td>
    <td>To increase your log storage limits, see the <a href="/docs/services/CloudLogAnalysis/troubleshooting/error_msgs.html">{{site.data.keyword.loganalysislong_notm}} documentation</a>.</td>
  </tr>
  <tr>
    <td>If you specified a space at cluster creation, the account owner does not have Manager, Developer, or Auditor permissions to that space.</td>
      <td>To change access permissions for the account owner:<ol><li>To find out who the account owner for the cluster is, run <code>{[bxcs]} api-key-info &lt;cluster_name_or_ID&gt;</code>.</li><li>To grant that account owner Manager, Developer, or Auditor {{site.data.keyword.containershort_notm}} access permissions to the space, see <a href="cs_users.html#managing">Managing cluster access</a>.</li><li>To refresh the logging token after permissions have been changed, run <code>{[bxcs]} logging-config-refresh &lt;cluster_name_or_ID&gt;</code>.</li></ol></td>
    </tr>
    <tr>
      <td>You have an application logging config with a symlink in your app path.</td>
      <td><p>In order for logs to be sent, you must use an absolute path in your logging configuration or the logs cannot be read. If your path is mounted to your worker node, it might have created a symlink.</p> <p>Example: If the specified path is <code>/usr/local/<b>spark</b>/work/app-0546/0/stderr</code> but the logs actually go to <code>/usr/local/<b>spark-1.0-hadoop-1.2</b>/work/app-0546/0/stderr</code>, then the logs cannot be read.</td>
    </tr>
  </tbody>
</table>

To test changes you made during troubleshooting, you can deploy *Noisy*, a sample pod that produces several log events, onto a worker node in your cluster.

  1. [Target your CLI](cs_cli_install.html#cs_cli_configure) to a cluster where you want to start producing logs.

  2. Create the `deploy-noisy.yaml` configuration file.

      ```
      apiVersion: v1
      kind: Pod
      metadata:
        name: noisy
      spec:
        containers:
        - name: noisy
          image: ubuntu:16.04
          command: ["/bin/sh"]
          args: ["-c", "while true; do sleep 10; echo 'Hello world!'; done"]
          imagePullPolicy: "Always"
        ```
        {: codeblock}

  3. Run the configuration file in the cluster's context.

        ```
        kubectl apply -f noisy.yaml
        ```
        {:pre}

  4. After a few minutes, you can view your logs in the Kibana dashboard. To access the Kibana dashboard, go to one of the following URLs and select the {{site.data.keyword.Bluemix_notm}} account where you created the cluster. If you specified a space at cluster creation, go to that space instead.
      - US-South and US-East: https://logging.ng.bluemix.net
      - UK-South: https://logging.eu-gb.bluemix.net
      - EU-Central: https://logging.eu-fra.bluemix.net
      - AP-South: https://logging.au-syd.bluemix.net

{[white-space.md]}

## Kubernetes dashboard does not display utilization graphs
{: #cs_dashboard_graphs}

{: tsSymptoms}
When you access the Kubernetes dashboard, utilization graphs do not display.

{: tsCauses}
Sometimes after a cluster update or worker node reboot the `kube-dashboard` pod does not update.

{: tsResolve}
Delete the `kube-dashboard` pod to force a restart. The pod is re-created with RBAC policies to access heapster for utilization information.

  ```
  kubectl delete pod -n kube-system $(kubectl get pod -n kube-system --selector=k8s-app=kubernetes-dashboard -o jsonpath='{.items..metadata.name}')
  ```
  {: pre}

{[white-space.md]}

## Getting help and support
{: #ts_getting_help}

{[get-help.md]}
