---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-12"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}


# Integrating services
{: #integrations}

You can use various external services and services in the {{site.data.keyword.Bluemix_notm}} Catalog with a standard cluster in {{site.data.keyword.containershort_notm}}.
{:shortdesc}


## Application services
<table summary="Summary for accessibility">
<caption>Table. Integration options for application services</caption>
<thead>
<tr>
<th>Service</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>{{site.data.keyword.blockchainfull}}</td>
<td>Deploy a publicly available development environment for IBM Blockchain to a Kubernetes cluster in {{site.data.keyword.containerlong_notm}}. Use this environment to develop and customize your own blockchain network to deploy apps that share an immutable ledger for recording the history of transactions. For more information, see <a href="https://ibm-blockchain.github.io" target="_blank">Develop in a cloud sandbox
IBM Blockchain Platform <img src="../icons/launch-glyph.svg" alt="External link icon"></a>. </td>
</tr>
</tbody>
</table>

<br />



## DevOps services
<table summary="Summary for accessibility">
<caption>Table. Integration options for managing DevOps</caption>
<thead>
<tr>
<th>Service</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Codeship</td>
<td>You can use <a href="https://codeship.com" target="_blank">Codeship <img src="../icons/launch-glyph.svg" alt="External link icon"></a> for the continuous integration and delivery of containers. For more information, see <a href="https://www.ibm.com/blogs/bluemix/2017/10/using-codeship-pro-deploy-workloads-ibm-container-service/" target="_blank">Using Codeship Pro To Deploy Workloads to {{site.data.keyword.containershort_notm}} <img src="../icons/launch-glyph.svg" alt="External link icon"></a>. </td>
</tr>
<tr>
<td>Helm</td>
<td> <a href="https://helm.sh/" target="_blank">Helm <img src="../icons/launch-glyph.svg" alt="External link icon"></a> is a Kubernetes package manager. Create Helm Charts to define, install, and upgrade complex Kubernetes applications running in {{site.data.keyword.containerlong_notm}} clusters. Learn more about how you can <a href="https://developer.ibm.com/recipes/tutorials/increase-deployment-velocity-with-kubernetes-helm-charts/" target="_blank">increase deployment velocity with Kubernetes Helm Charts <img src="../icons/launch-glyph.svg" alt="External link icon"></a>. </td>
</tr>
<tr>
<td>{{site.data.keyword.contdelivery_full}}</td>
<td>Automate your app builds and container deployments to Kubernetes clusters by using a toolchain. For setup information, see the blog <a href="https://developer.ibm.com/recipes/tutorials/deploy-kubernetes-pods-to-the-bluemix-container-service-using-devops-pipelines/" target="_blank">Deploy Kubernetes pods to the {{site.data.keyword.containerlong_notm}} using DevOps Pipelines <img src="../icons/launch-glyph.svg" alt="External link icon"></a>. </td>
</tr>
<tr>
<td>{{site.data.keyword.bpfull}}</td>
<td>{{site.data.keyword.bplong}} is an automation tool that uses Terraform to deploy your infrastructure as code. When you deploy your infrastructure as a single unit, you can reuse those cloud resource definitions across any number of environments. To define a Kubernetes cluster as a resource with {{site.data.keyword.bpshort}}, try creating an environment with the [container-cluster template](https://console.bluemix.net/schematics/templates/details/Cloud-Schematics%2Fcontainer-cluster). For more information about Schematics, see [About {{site.data.keyword.bplong_notm}}](/docs/services/schematics/schematics_overview.html#about).</td>
</tr>
<tr>
<td>Istio</td>
<td><a href="https://www.ibm.com/cloud/info/istio" target="_blank">Istio <img src="../icons/launch-glyph.svg" alt="External link icon"></a> is an open source service that gives developers a way to connect, secure, manage, and monitor a network of microservices, also known a service mesh, on cloud orchestration platforms like Kubernetes. Check out the blog post about <a href="https://developer.ibm.com/dwblog/2017/istio/" target="_blank">how IBM co-founded and launched Istio<img src="../icons/launch-glyph.svg" alt="External link icon"></a> to find out more about the open-source project. To install Istio on your Kubernetes cluster in {{site.data.keyword.containershort_notm}} and get started with a sample app, see [Tutorial: Managing microservices with Istio](cs_tutorials_istio.html#istio_tutorial).</td>
</tr>
</tbody>
</table>

<br />



## Logging and monitoring services
<table summary="Summary for accessibility">
<caption>Table. Integration options for managing logs and metrics</caption>
<thead>
<tr>
<th>Service</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>CoScale</td>
<td>Monitor worker nodes, containers, replica sets, replication controllers, and services with <a href="https://www.coscale.com/" target="_blank">CoScale <img src="../icons/launch-glyph.svg" alt="External link icon"></a>. For more information, see <a href="https://www.ibm.com/blogs/bluemix/2017/06/monitoring-ibm-bluemix-container-service-coscale/" target="_blank">Monitoring {{site.data.keyword.containershort_notm}} with CoScale <img src="../icons/launch-glyph.svg" alt="External link icon"></a>. </td>
</tr>
<tr>
<td>Datadog</td>
<td>Monitor your cluster and view infrastructure and application performance metrics with <a href="https://www.datadoghq.com/" target="_blank">Datadog <img src="../icons/launch-glyph.svg" alt="External link icon"></a>. For more information, see <a href="https://www.ibm.com/blogs/bluemix/2017/07/monitoring-ibm-bluemix-container-service-datadog/" target="_blank">Monitoring {{site.data.keyword.containershort_notm}} with Datadog <img src="../icons/launch-glyph.svg" alt="External link icon"></a>. </td>
</tr>
<tr>
<td>Instana</td>
<td> <a href="https://www.instana.com/" target="_blank">Instana <img src="../icons/launch-glyph.svg" alt="External link icon"></a> provides infrastructure and app performance monitoring with a GUI that automatically discovers and maps your apps. Istana captures every request to your apps, which lets you troubleshoot and perform root cause analysis to prevent the problems from happening again. Check out the blog post about <a href="https://www.instana.com/blog/precise-visibility-applications-ibm-bluemix-container-service/" target="_blank">deploying Istana in {{site.data.keyword.containershort_notm}} <img src="../icons/launch-glyph.svg" alt="External link icon"></a> to learn more.</td>
</tr>
<tr>
<td>Prometheus</td>
<td>Prometheus is an open source monitoring, logging, and alerting tool that was specifically designed for Kubernetes to retrieve detailed information about the cluster, worker nodes, and deployment health based on the Kubernetes logging information. The CPU, memory, I/O, and network activity of all running containers in a cluster are collected and can be used in custom queries or alerts to monitor performance and workloads in your cluster.

<p>To use Prometheus, follow the <a href="https://github.com/coreos/prometheus-operator/tree/master/contrib/kube-prometheus" target="_blank">the CoreOS instructions <img src="../icons/launch-glyph.svg" alt="External link icon"></a>.</p>
</td>
</tr>
<tr>
<td>Sematext</td>
<td>View metrics and logs for your containerized applications by using <a href="https://sematext.com/" target="_blank">Sematext <img src="../icons/launch-glyph.svg" alt="External link icon"></a>. For more information, see <a href="https://www.ibm.com/blogs/bluemix/2017/09/monitoring-logging-ibm-bluemix-container-service-sematext/" target="_blank">Monitoring & logging for containers with Sematext <img src="../icons/launch-glyph.svg" alt="External link icon"></a>. </td>
</tr>
<tr>
<td>Sysdig</td>
<td>Capture app, container, statsd, and host metrics with a single instrumentation point by using <a href="https://sysdig.com/" target="_blank">Sysdig <img src="../icons/launch-glyph.svg" alt="External link icon"></a>. For more information, see <a href="https://www.ibm.com/blogs/bluemix/2017/08/monitoring-ibm-bluemix-container-service-sysdig-container-intelligence/" target="_blank">Monitoring {{site.data.keyword.containershort_notm}} with Sysdig Container Intelligence <img src="../icons/launch-glyph.svg" alt="External link icon"></a>. </td>
</tr>
<tr>
<td>Weave Scope</td>
<td>Weave Scope provides a visual diagram of your resources within a Kubernetes cluster, including services, pods, containers, processes, nodes, and more. Weave Scope provides interactive metrics for CPU and memory and also provides tools to tail and exec into a container.<p>For more information, see [Visualizing Kubernetes cluster resources with Weave Scope and {{site.data.keyword.containershort_notm}}](cs_integrations.html#weavescope).</p></li></ol>
</td>
</tr>
</tbody>
</table>

<br />



## Security services
<table summary="Summary for accessibility">
<caption>Table. Integration options for managing security</caption>
<thead>
<tr>
<th>Service</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Aqua Security</td>
  <td>As a supplement to <a href="/docs/services/va/va_index.html" target="_blank">Vulnerability Advisor</a>, you can use <a href="https://www.aquasec.com/" target="_blank">Aqua Security <img src="../icons/launch-glyph.svg" alt="External link icon"></a> to improve the security of container deployments by reducing what your app is allowed to do. For more information, see <a href="https://www.ibm.com/blogs/bluemix/2017/06/protecting-container-deployments-bluemix-aqua-security/" target="_blank">Protecting container deployments on {{site.data.keyword.Bluemix_notm}} with Aqua Security <img src="../icons/launch-glyph.svg" alt="External link icon"></a>. </td>
</tr>
<tr>
<td>{{site.data.keyword.cloudcerts_full}}</td>
<td>You can use <a href="../services/certificate-manager/index.html" target="_blank">{{site.data.keyword.cloudcerts_long}} <img src="../icons/launch-glyph.svg" alt="External link icon"></a> to store and manage SSL certificates for your apps. For more information, see <a href="https://www.ibm.com/blogs/bluemix/2018/01/use-ibm-cloud-certificate-manager-ibm-cloud-container-service-deploy-custom-domain-tls-certificates/" target="_blank">Use {{site.data.keyword.cloudcerts_long_notm}} with {{site.data.keyword.containershort_notm}} to deploy custom domain TLS Certificates <img src="../icons/launch-glyph.svg" alt="External link icon"></a>. </td>
</tr>
<tr>
<td>NeuVector</td>
<td>Protect containers with a cloud-native firewall by using <a href="https://neuvector.com/" target="_blank">NeuVector <img src="../icons/launch-glyph.svg" alt="External link icon"></a>. For more information, see <a href="https://www.ibm.com/us-en/marketplace/neuvector-container-security" target="_blank">NeuVector Container Security <img src="../icons/launch-glyph.svg" alt="External link icon"></a>. </td>
</tr>
<tr>
<td>Twistlock</td>
<td>As a supplement to <a href="/docs/services/va/va_index.html" target="_blank">Vulnerability Advisor</a>, you can use <a href="https://www.twistlock.com/" target="_blank">Twistlock <img src="../icons/launch-glyph.svg" alt="External link icon"></a> to manage firewalls, threat protection, and incident response. For more information, see <a href="https://www.ibm.com/blogs/bluemix/2017/07/twistlock-ibm-bluemix-container-service/" target="_blank">Twistlock on {{site.data.keyword.containershort_notm}} <img src="../icons/launch-glyph.svg" alt="External link icon"></a>. </td>
</tr>
</tbody>
</table>

<br />



## Adding services to clusters
{: #adding_cluster}

Add an existing {{site.data.keyword.Bluemix_notm}} service instance to your cluster to enable your cluster users to access and use the {{site.data.keyword.Bluemix_notm}} service when they deploy an app to the cluster.
{:shortdesc}

Before you begin:

1. [Target your CLI](cs_cli_install.html#cs_cli_configure) to your cluster.
2. [Request an instance of the {{site.data.keyword.Bluemix_notm}} service](/docs/apps/reqnsi.html#req_instance).
   **Note:** To create an instance of a service in the Washington DC location, you must use the CLI.

**Note:**
<ul><ul>
<li>You can only add {{site.data.keyword.Bluemix_notm}} services that support service keys. If the service does not support service keys, see [Enabling external apps to use {{site.data.keyword.Bluemix_notm}} services](/docs/apps/reqnsi.html#accser_external).</li>
<li>The cluster and the worker nodes must be deployed fully before you can add a service.</li>
</ul></ul>


To add a service:
2.  List available {{site.data.keyword.Bluemix_notm}} services.

    ```
    bx service list
    ```
    {: pre}

    Example CLI output:

    ```
    name                      service           plan    bound apps   last operation
    <service_instance_name>   <service_name>    spark                create succeeded
    ```
    {: screen}

3.  Note the **name** of the service instance that you want to add to your cluster.
4.  Identify the cluster namespace that you want to use to add your service. Choose between the following options.
    -   List existing namespaces and choose a namespace that you want to use.

        ```
        kubectl get namespaces
        ```
        {: pre}

    -   Create a new namespace in your cluster.

        ```
        kubectl create namespace <namespace_name>
        ```
        {: pre}

5.  Add the service to your cluster.

    ```
    bx cs cluster-service-bind <cluster_name_or_id> <namespace> <service_instance_name>
    ```
    {: pre}

    When the service is successfully added to your cluster, a cluster secret is created that holds the credentials of your service instance. Example CLI output:

    ```
    bx cs cluster-service-bind mycluster mynamespace cleardb
    Binding service instance to namespace...
    OK
    Namespace:	mynamespace
    Secret name:     binding-<service_instance_name>
    ```
    {: screen}

6.  Verify that the secret was created in your cluster namespace.

    ```
    kubectl get secrets --namespace=<namespace>
    ```
    {: pre}


To use the service in a pod that is deployed in the cluster, cluster users can access the service credentials of the {{site.data.keyword.Bluemix_notm}} service by [mounting the Kubernetes secret as a secret volume to a pod](cs_storage.html#app_volume_mount).

<br />



## Adding services to apps
{: #adding_app}

Encrypted Kubernetes secrets are used to store {{site.data.keyword.Bluemix_notm}} service details and credentials and allow secure communication between the service and the cluster. As the cluster user, you can access this secret by mounting it as a volume to a pod.
{:shortdesc}

Before you begin, [target your CLI](cs_cli_install.html#cs_cli_configure) to your cluster. Make sure that the {{site.data.keyword.Bluemix_notm}} service that you want to use in your app was [added to the cluster](cs_integrations.html#adding_cluster) by the cluster admin.

Kubernetes secrets are a secure way to store confidential information, such as user names, passwords, or keys. Rather than exposing confidential information via environment variables or directly in the Dockerfile, secrets must be mounted as a secret volume to a pod in order to be accessible by a running container in a pod.

When you mount a secret volume to your pod, a file named binding is stored in the volume mount directory that includes all information and credentials that you need to access the {{site.data.keyword.Bluemix_notm}} service.

1.  List available secrets in your cluster namespace.

    ```
    kubectl get secrets --namespace=<my_namespace>
    ```
    {: pre}

    Example output:

    ```
    NAME                                    TYPE                                  DATA      AGE
    binding-<service_instance_name>         Opaque                                1         3m

    ```
    {: screen}

2.  Look for a secret of type **Opaque** and note the **name** of the secret. If multiple secrets exist, contact your cluster administrator to identify the correct service secret.

3.  Open your preferred editor.

4.  Create a YAML file to configure a pod that can access the service details through a secret volume. If you bound more than one service, verify that each secret is associated with the correct service.

    ```
    apiVersion: extensions/v1beta1
    kind: Deployment
    metadata:
      labels:
        app: secret-test
      name: secret-test
      namespace: <my_namespace>
    spec:
      selector:
        matchLabels:
          app: secret-test
      replicas: 1
      template:
        metadata:
          labels:
            app: secret-test
        spec:
          containers:
          - image: nginx
            name: secret-test
            volumeMounts:
            - mountPath: /opt/service-bind
              name: service-bind-volume
          volumes:
          - name: service-bind-volume
            secret:
              defaultMode: 420
              secretName: binding-<service_instance_name>
    ```
    {: codeblock}

    <table>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the YAML file components</th>
    </thead>
    <tbody>
    <tr>
    <td><code>volumeMounts/mountPath</code></td>
    <td>The name of the secret volume that you want to mount to your container.</td>
    </tr>
    <tr>
    <td><code>volumes/name</code></td>
    <td>Enter a name for the secret volume that you want to mount to your container.</td>
    </tr>
    <tr>
    <td><code>secret/defaultMode</code></td>
    <td>Set read-only permissions for the service secret.</td>
    </tr>
    <tr>
    <td><code>secret/secretName</code></td>
    <td>Enter the name of the secret that you noted earlier.</td>
    </tr></tbody></table>

5.  Create the pod and mount the secret volume.

    ```
    kubectl apply -f <yaml_path>
    ```
    {: pre}

6.  Verify that the pod is created.

    ```
    kubectl get pods --namespace=<my_namespace>
    ```
    {: pre}

    Example CLI output:

    ```
    NAME                           READY     STATUS    RESTARTS   AGE
    secret-test-1111454598-gfx32   1/1       Running   0          1m
    ```
    {: screen}

7.  Note the **NAME** of your pod.
8.  Get the details about the pod and look for the secret name.

    ```
    kubectl describe pod <pod_name>
    ```
    {: pre}

    Output:

    ```
    ...
    Volumes:
      service-bind-volume:
        Type:       Secret (a volume populated by a Secret)
        SecretName: binding-<service_instance_name>
    ...
    ```
    {: screen}

    

9.  When implementing your app, configure it to find the secret file named **binding** in the mount directory, parse the JSON content and determine the URL and service credentials to access your {{site.data.keyword.Bluemix_notm}} service.

You can now access the {{site.data.keyword.Bluemix_notm}} service details and credentials. To work with your {{site.data.keyword.Bluemix_notm}} service, make sure your app is configured to find the service secret file in the mount directory, parse the JSON content and determine the service details.

<br />



## Visualizing Kubernetes cluster resources
{: #weavescope}

Weave Scope provides a visual diagram of your resources within a Kubernetes cluster, including services, pods, containers, processes, nodes, and more. Weave Scope provides interactive metrics for CPU and memory and also provides tools to tail and exec into a container.
{:shortdesc}

Before you begin:

-   Remember not to expose your cluster information on the public internet. Complete these steps to deploy Weave Scope securely and access it from a web browser locally.
-   If you do not have one already, [create a standard cluster](cs_clusters.html#clusters_ui). Weave Scope can be CPU intensive, especially the app. Run Weave Scope with larger standard clusters, not free clusters.
-   [Target your CLI](cs_cli_install.html#cs_cli_configure) to your cluster to run `kubectl` commands.


To use Weave Scope with a cluster:
2.  Deploy one of the provided RBAC permissions configuration file in the cluster.

    To enable read/write permissions:

    ```
    kubectl apply -f "https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/weave-scope/weave-scope-rbac.yaml"
    ```
    {: pre}

    To enable read-only permissions:

    ```
    kubectl apply --namespace weave -f "https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/weave-scope/weave-scope-rbac-readonly.yaml"
    ```
    {: pre}

    Output:

    ```
    clusterrole "weave-scope-mgr" created
    clusterrolebinding "weave-scope-mgr-role-binding" created
    ```
    {: screen}

3.  Deploy the Weave Scope service, which is privately accessible by the cluster IP address.

    <pre class="pre">
    <code>kubectl apply --namespace weave -f "https://cloud.weave.works/k8s/scope.yaml?k8s-version=$(kubectl version | base64 | tr -d '&bsol;n')"</code>
    </pre>

    Output:

    ```
    serviceaccount "weave-scope" created
    deployment "weave-scope-app" created
    service "weave-scope-app" created
    daemonset "weave-scope-agent" created
    ```
    {: screen}

4.  Run a port forwarding command to bring up the service on your computer. Now that Weave Scope is configured with the cluster, to access Weave Scope next time, you can run this port forwarding command without completing the previous configuration steps again.

    ```
    kubectl port-forward -n weave "$(kubectl get -n weave pod --selector=weave-scope-component=app -o jsonpath='{.items..metadata.name}')" 4040
    ```
    {: pre}

    Output:

    ```
    Forwarding from 127.0.0.1:4040 -> 4040
    Forwarding from [::1]:4040 -> 4040
    Handling connection for 4040
    ```
    {: screen}

5.  Open your web browser to `http://localhost:4040`. Without the default components deployed, you see the following diagram. You can choose to view topology diagrams or tables of the Kubernetes resources in the cluster.

     <img src="images/weave_scope.png" alt="Example topology from Weave Scope" style="width:357px;" />


[Learn more about the Weave Scope features ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.weave.works/docs/scope/latest/features/).

<br />

