---

copyright:
  years: 2014, 2018
lastupdated: "2018-09-05"

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

You can use various external services and catalog services with a standard Kubernetes cluster in {{site.data.keyword.containerlong}}.
{:shortdesc}


## Application services
{: #application_services}
<table summary="Summary for accessibility">
<caption>Application services</caption>
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
{: #devops_services}
<table summary="Summary for accessibility">
<caption>DevOps services</caption>
<thead>
<tr>
<th>Service</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Codeship</td>
<td>You can use <a href="https://codeship.com" target="_blank">Codeship <img src="../icons/launch-glyph.svg" alt="External link icon"></a> for the continuous integration and delivery of containers. For more information, see <a href="https://www.ibm.com/blogs/bluemix/2017/10/using-codeship-pro-deploy-workloads-ibm-container-service/" target="_blank">Using Codeship Pro To Deploy Workloads to {{site.data.keyword.containerlong_notm}} <img src="../icons/launch-glyph.svg" alt="External link icon"></a>. </td>
</tr>
<tr>
<td>Helm</td>
<td> <a href="https://helm.sh" target="_blank">Helm <img src="../icons/launch-glyph.svg" alt="External link icon"></a> is a Kubernetes package manager. You can create new Helm charts or use preexisting Helm charts to define, install, and upgrade complex Kubernetes applications that run in {{site.data.keyword.containerlong_notm}} clusters. <p>For more information, see [Setting up Helm in {{site.data.keyword.containerlong_notm}}](cs_integrations.html#helm).</p></td>
</tr>
<tr>
<td>{{site.data.keyword.contdelivery_full}}</td>
<td>Automate your app builds and container deployments to Kubernetes clusters by using a toolchain. For setup information, see the blog <a href="https://developer.ibm.com/recipes/tutorials/deploy-kubernetes-pods-to-the-bluemix-container-service-using-devops-pipelines/" target="_blank">Deploy Kubernetes pods to the {{site.data.keyword.containerlong_notm}} using DevOps Pipelines <img src="../icons/launch-glyph.svg" alt="External link icon"></a>. </td>
</tr>
<tr>
<td>Istio</td>
<td><a href="https://www.ibm.com/cloud/info/istio" target="_blank">Istio <img src="../icons/launch-glyph.svg" alt="External link icon"></a> is an open source service that gives developers a way to connect, secure, manage, and monitor a network of microservices, also known a service mesh, on cloud orchestration platforms like Kubernetes. Check out the blog post about <a href="https://developer.ibm.com/dwblog/2017/istio/" target="_blank">how IBM co-founded and launched Istio<img src="../icons/launch-glyph.svg" alt="External link icon"></a> to find out more about the open-source project. To install Istio on your Kubernetes cluster in {{site.data.keyword.containerlong_notm}} and get started with a sample app, see [Tutorial: Managing microservices with Istio](cs_tutorials_istio.html#istio_tutorial).</td>
</tr>
</tbody>
</table>

<br />



## Logging and monitoring services
{: #health_services}
<table summary="Summary for accessibility">
<caption>Logging and monitoring services</caption>
<thead>
<tr>
<th>Service</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>CoScale</td>
<td>Monitor worker nodes, containers, replica sets, replication controllers, and services with <a href="https://www.coscale.com/" target="_blank">CoScale <img src="../icons/launch-glyph.svg" alt="External link icon"></a>. For more information, see <a href="https://www.ibm.com/blogs/bluemix/2017/06/monitoring-ibm-bluemix-container-service-coscale/" target="_blank">Monitoring {{site.data.keyword.containerlong_notm}} with CoScale <img src="../icons/launch-glyph.svg" alt="External link icon"></a>. </td>
</tr>
<tr>
<td>Datadog</td>
<td>Monitor your cluster and view infrastructure and application performance metrics with <a href="https://www.datadoghq.com/" target="_blank">Datadog <img src="../icons/launch-glyph.svg" alt="External link icon"></a>. For more information, see <a href="https://www.ibm.com/blogs/bluemix/2017/07/monitoring-ibm-bluemix-container-service-datadog/" target="_blank">Monitoring {{site.data.keyword.containerlong_notm}} with Datadog <img src="../icons/launch-glyph.svg" alt="External link icon"></a>. </td>
</tr>
<tr>
<td> {{site.data.keyword.cloudaccesstrailfull}}</td>
<td>Monitor the administrative activity made in your cluster by analyzing logs through Grafana. For more information about the service, see the [Activity Tracker](/docs/services/cloud-activity-tracker/index.html) documentation. For more information about the types of events that you can track, see [Activity Tracker events](/cs_at_events.html).</td>
</tr>
<tr>
<td>{{site.data.keyword.loganalysisfull}}</td>
<td>Expand your log collection, retention, and search abilities with {{site.data.keyword.loganalysisfull_notm}}. For more information, see <a href="../services/CloudLogAnalysis/containers/containers_kube_other_logs.html" target="_blank">Enabling automatic collection of cluster logs <img src="../icons/launch-glyph.svg" alt="External link icon"></a>. </td>
</tr>
<tr>
<td>{{site.data.keyword.monitoringlong}}</td>
<td>Expand your metrics collection and retention capabilities by defining rules and alerts with {{site.data.keyword.monitoringlong_notm}}. For more information, see <a href="../services/cloud-monitoring/tutorials/container_service_metrics.html" target="_blank">Analyze metrics in Grafana for an app that is deployed in a Kubernetes cluster <img src="../icons/launch-glyph.svg" alt="External link icon"></a>. </td>
</tr>
<tr>
<td>Instana</td>
<td> <a href="https://www.instana.com/" target="_blank">Instana <img src="../icons/launch-glyph.svg" alt="External link icon"></a> provides infrastructure and app performance monitoring with a GUI that automatically discovers and maps your apps. Istana captures every request to your apps, which you can use to troubleshoot and perform root cause analysis to prevent the problems from happening again. Check out the blog post about <a href="https://www.instana.com/blog/precise-visibility-applications-ibm-bluemix-container-service/" target="_blank">deploying Istana in {{site.data.keyword.containerlong_notm}} <img src="../icons/launch-glyph.svg" alt="External link icon"></a> to learn more.</td>
</tr>
<tr>
<td>Prometheus</td>
<td>Prometheus is an open source monitoring, logging, and alerting tool that was specifically designed for Kubernetes. Prometheus retrieves detailed information about the cluster, worker nodes, and deployment health based on Kubernetes logging information. CPU, memory, I/O, and network activity is collected for each container running in a cluster. You can use the collected data in custom queries or alerts to monitor performance and workloads in your cluster.

<p>To use Prometheus, follow the <a href="https://github.com/coreos/prometheus-operator/tree/master/contrib/kube-prometheus" target="_blank">CoreOS instructions <img src="../icons/launch-glyph.svg" alt="External link icon"></a>.</p>
</td>
</tr>
<tr>
<td>Sematext</td>
<td>View metrics and logs for your containerized applications by using <a href="https://sematext.com/" target="_blank">Sematext <img src="../icons/launch-glyph.svg" alt="External link icon"></a>. For more information, see <a href="https://www.ibm.com/blogs/bluemix/2017/09/monitoring-logging-ibm-bluemix-container-service-sematext/" target="_blank">Monitoring and logging for containers with Sematext <img src="../icons/launch-glyph.svg" alt="External link icon"></a>. </td>
</tr>
<tr>
<td>Sysdig</td>
<td>Capture app, container, statsd, and host metrics with a single instrumentation point by using <a href="https://sysdig.com/" target="_blank">Sysdig <img src="../icons/launch-glyph.svg" alt="External link icon"></a>. For more information, see <a href="https://www.ibm.com/blogs/bluemix/2017/08/monitoring-ibm-bluemix-container-service-sysdig-container-intelligence/" target="_blank">Monitoring {{site.data.keyword.containerlong_notm}} with Sysdig Container Intelligence <img src="../icons/launch-glyph.svg" alt="External link icon"></a>. </td>
</tr>
<tr>
<td>Weave Scope</td>
<td>Weave Scope provides a visual diagram of your resources within a Kubernetes cluster, including services, pods, containers, processes, nodes, and more. Weave Scope provides interactive metrics for CPU and memory and also provides tools to tail and exec into a container.<p>For more information, see [Visualizing Kubernetes cluster resources with Weave Scope and {{site.data.keyword.containerlong_notm}}](cs_integrations.html#weavescope).</p></li></ol>
</td>
</tr>
</tbody>
</table>

<br />



## Security services
{: #security_services}
<table summary="Summary for accessibility">
<caption>Security services</caption>
<thead>
<tr>
<th>Service</th>
<th>Description</th>
</tr>
</thead>
<tbody>
  <tr id="appid">
    <td>{{site.data.keyword.appid_full}}</td>
    <td>Add a level of security to your apps with [{{site.data.keyword.appid_short}}](/docs/services/appid/index.html#gettingstarted) by requiring users to sign in. To authenticate web or API HTTP/HTTPS requests to your app, you can integrate {{site.data.keyword.appid_short_notm}} with your Ingress service by using the [{{site.data.keyword.appid_short_notm}} authentication Ingress annotation](cs_annotations.html#appid-auth).</td>
  </tr>
<tr>
<td>Aqua Security</td>
  <td>As a supplement to <a href="/docs/services/va/va_index.html" target="_blank">Vulnerability Advisor</a>, you can use <a href="https://www.aquasec.com/" target="_blank">Aqua Security <img src="../icons/launch-glyph.svg" alt="External link icon"></a> to improve the security of container deployments by reducing what your app is allowed to do. For more information, see <a href="https://blog.aquasec.com/securing-container-deployments-on-bluemix-with-aqua-security" target="_blank">Securing container deployments on {{site.data.keyword.Bluemix_notm}} with Aqua Security <img src="../icons/launch-glyph.svg" alt="External link icon"></a>. </td>
</tr>
<tr>
<td>{{site.data.keyword.cloudcerts_full}}</td>
<td>You can use <a href="../services/certificate-manager/index.html" target="_blank">{{site.data.keyword.cloudcerts_long}} <img src="../icons/launch-glyph.svg" alt="External link icon"></a> to store and manage SSL certificates for your apps. For more information, see <a href="https://www.ibm.com/blogs/bluemix/2018/01/use-ibm-cloud-certificate-manager-ibm-cloud-container-service-deploy-custom-domain-tls-certificates/" target="_blank">Use {{site.data.keyword.cloudcerts_long_notm}} with {{site.data.keyword.containerlong_notm}} to deploy custom domain TLS Certificates <img src="../icons/launch-glyph.svg" alt="External link icon"></a>. </td>
</tr>
<tr>
  <td>{{site.data.keyword.registrylong}}</td>
  <td>Set up your own secured Docker image repository where you can safely store and share images between cluster users. For more information, see the <a href="/docs/services/Registry/index.html" target="_blank">{{site.data.keyword.registrylong}} documentation <img src="../icons/launch-glyph.svg" alt="External link icon"></a>.</td>
</tr>
<tr>
<td>NeuVector</td>
<td>Protect containers with a cloud-native firewall by using <a href="https://neuvector.com/" target="_blank">NeuVector <img src="../icons/launch-glyph.svg" alt="External link icon"></a>. For more information, see <a href="https://www.ibm.com/us-en/marketplace/neuvector-container-security" target="_blank">NeuVector Container Security <img src="../icons/launch-glyph.svg" alt="External link icon"></a>. </td>
</tr>
<tr>
<td>Twistlock</td>
<td>As a supplement to <a href="/docs/services/va/va_index.html" target="_blank">Vulnerability Advisor</a>, you can use <a href="https://www.twistlock.com/" target="_blank">Twistlock <img src="../icons/launch-glyph.svg" alt="External link icon"></a> to manage firewalls, threat protection, and incident response. For more information, see <a href="https://www.ibm.com/blogs/bluemix/2017/07/twistlock-ibm-bluemix-container-service/" target="_blank">Twistlock on {{site.data.keyword.containerlong_notm}} <img src="../icons/launch-glyph.svg" alt="External link icon"></a>. </td>
</tr>
</tbody>
</table>

<br />



## Storage services
{: #storage_services}
<table summary="Summary for accessibility">
<caption>Storage services</caption>
<thead>
<tr>
<th>Service</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
  <td>Heptio Ark</td>
  <td>You can use <a href="https://github.com/heptio/ark" target="_blank">Heptio Ark <img src="../icons/launch-glyph.svg" alt="External link icon"></a> to back up and restore cluster resources and persistent volumes. For more information, see the Heptio Ark <a href="https://github.com/heptio/ark/blob/master/docs/use-cases.md#use-cases" target="_blank">Use cases for disaster recovery and cluster migration <img src="../icons/launch-glyph.svg" alt="External link icon"></a>.</td>
</tr>
<tr>
  <td>{{site.data.keyword.cos_full}}</td>
  <td>Data that is stored with {{site.data.keyword.cos_short}} is encrypted and dispersed across multiple geographic locations, and accessed over HTTP by using a REST API. You can use the [ibm-backup-restore image](/docs/services/RegistryImages/ibm-backup-restore/index.html) to configure the service to make one-time or scheduled backups for data in your clusters. For general information about the service, see the <a href="/docs/services/cloud-object-storage/about-cos.html" target="_blank">{{site.data.keyword.cos_short}} documentation <img src="../icons/launch-glyph.svg" alt="External link icon"></a>.</td>
</tr>
  <tr>
    <td>{{site.data.keyword.cloudantfull}}</td>
    <td>{{site.data.keyword.cloudant_short_notm}} is a document-oriented DataBase as a Service (DBaaS) that stores data as documents in JSON format. The service is built for scalability, high availability, and durability. For more information, see the <a href="/docs/services/Cloudant/getting-started.html" target="_blank">{{site.data.keyword.cloudant_short_notm}} documentation <img src="../icons/launch-glyph.svg" alt="External link icon"></a>.</td>
  </tr>
  <tr>
    <td>{{site.data.keyword.composeForMongoDB_full}}</td>
    <td>{{site.data.keyword.composeForMongoDB}} delivers high availability and redundancy, automated and on-demand no-stop backups, monitoring tools, integration into alert systems, performance analysis views, and more. For more information, see the <a href="/docs/services/ComposeForMongoDB/index.html" target="_blank">{{site.data.keyword.composeForMongoDB}} documentation <img src="../icons/launch-glyph.svg" alt="External link icon"></a>.</td>
  </tr>
</tbody>
</table>


<br />



## Adding {{site.data.keyword.Bluemix_notm}} services to clusters
{: #adding_cluster}

Add {{site.data.keyword.Bluemix_notm}} services to enhance your Kubernetes cluster with extra capabilities in areas such as Watson AI, data, security, and Internet of Things (IoT).
{:shortdesc}

**Important:** You can bind only services that support service keys. To find a list with services that support service keys, see [Enabling external apps to use {{site.data.keyword.Bluemix_notm}} services](/docs/apps/reqnsi.html#accser_external). 

Before you begin, [target your CLI](cs_cli_install.html#cs_cli_configure) to your cluster.
   
To add an {{site.data.keyword.Bluemix_notm}} service to your cluster: 
1. [Create an instance of the {{site.data.keyword.Bluemix_notm}} service](/docs/apps/reqnsi.html#req_instance). </br></br>**Note:** Some {{site.data.keyword.Bluemix_notm}} services are available only in select regions. You can bind a service to your cluster only if the service is available in the same region as your cluster. In addition, if you want to create a service instance in the Washington DC zone, you must use the CLI.
   
2. Check the type of service that you created and make note of the service instance **Name**. 
   - **Cloud Foundry services:**
     ```
     ibmcloud service list
     ```
     {: pre}
   
     Example output:
     ```
     name                         service           plan    bound apps   last operation
     <cf_service_instance_name>   <service_name>    spark                create succeeded
     ```
     {: screen}
   
  - **IAM-enabled services:**
     ```
     ibmcloud resource service-instances
     ```
     {: pre}
     
     Example output: 
     ```
     Name                          Location   State    Type               Tags   
     <iam_service_instance_name>   <region>   active   service_instance      
     ```
     {: screen}
      
   You can also see the different service types in your dashboard as **Cloud Foundry Services** and **Services**.
   
3. For IAM-enabled services, create a Cloud Foundry alias so that you can bind this service to your cluster. If your service already is a Cloud Foundry service, this step is not necessary and you can continue with the next step. 
   1. Target a Cloud Foundry org and space. 
      ```
      ibmcloud target --cf
      ```
      {: pre}
   
   2. Create a Cloud Foundry alias for the service instance.
      ```
      ibmcloud resource service-alias-create <service_alias_name> --instance-name <iam_service_instance_name>
      ```
      {: pre}

   3. Verify that the service alias is created.
      ```
      ibmcloud service list
      ```
      {: pre}
      
4. Identify the cluster namespace that you want to use to add your service. Choose between the following options.
   - List existing namespaces and choose a namespace that you want to use.
     ```
     kubectl get namespaces
     ```
     {: pre}

   - Create a namespace in your cluster.
     ```
     kubectl create namespace <namespace_name>
     ```
     {: pre}

5.  Add the service to your cluster. For IAM-enabled services, make sure to use the Cloud Foundry alias that you created earlier. 
    ```
    ibmcloud ks cluster-service-bind <cluster_name_or_ID> <namespace> <service_instance_name>
    ```
    {: pre}

    When the service is successfully added to your cluster, a cluster secret is created that holds the credentials of your service instance. Secrets are automatically encrypted in etcd to protect your data. 
    
    Example output:
    ```
    ibmcloud ks cluster-service-bind mycluster mynamespace cleardb
    Binding service instance to namespace...
    OK
    Namespace:	     mynamespace
    Secret name:     binding-<service_instance_name>
    ```
    {: screen}

6.  Verify the service credentials in your Kubernetes secret. 
    1. Get the details of the secret and note the **binding** value. The **binding** value is base64 encoded and holds the credentials for your service instance in JSON format.
       ```
       kubectl get secrets binding-<service_instance_name> --namespace=<namespace> -o yaml
       ```
       {: pre}
    
       Example output:
       ```
       apiVersion: v1
       data:
         binding: <binding>
       kind: Secret
       metadata:
         annotations:
           service-instance-id: 1111aaaa-a1aa-1aa1-1a11-111aa111aa11
           service-key-id: 2b22bb2b-222b-2bb2-2b22-b22222bb2222
         creationTimestamp: 2018-08-07T20:47:14Z
         name: binding-<service_instance_name>
         namespace: <namespace>
         resourceVersion: "6145900"
         selfLink: /api/v1/namespaces/default/secrets/binding-mycloudant
         uid: 33333c33-3c33-33c3-cc33-cc33333333c
       type: Opaque
       ```
       {: screen}
   
    2. Decode the binding value. 
       ```
       echo "<binding>" | base64 -D
       ```
       {: pre}
       
       Example output: 
       ```
       {"apikey":"KL34Ys893284NGJEPFjgrioJ12NElpow","host":"98765aab-9ce1-7tr3-ba87-bfbab6e6d9d6-bluemix.cloudant.com","iam_apikey_description":"Auto generated apikey during resource-key operation for Instance - crn:v1:bluemix:public:cloudantnosqldb:us-south:a/1234g56789cfe8e6388dd2ec098:98746cw-43d7-49ce-947a-d8fe3eebb381::","iam_apikey_name":"auto-generated-apikey-1234abcde-987f-3t64-9d96-d13775ec5663","iam_role_crn":"crn:v1:bluemix:public:iam::::serviceRole:Writer","iam_serviceid_crn":"crn:v1:bluemix:public:iam-identity::a/1234567890brasge5htn2ec098::serviceid:ServiceId-12345vgh-6c4c-ytr12-af6b-467d30d6ef44","password":"jfiavhui12484fnivhuo472nvei23913c3ff","port":443,"url":"https://25c73aac-9ce1-4c24-ba98-bfbab6e6d9d6-bluemix:ugvioev823inreuiegn43donvri29989wiu9t22@25c73aac-9ce1-4c24-ba98-abdrjio123562lnsb-bluemix.cloudant.com","username":"123b45da-9ce1-4c24-ab12-rinwnwub1294-bluemix"}
       ```
       {: screen}
       
    3. Optional: Compare the service credentials that you decoded in the previous step with the service credentials that you find for your service instance in the {{site.data.keyword.Bluemix_notm}} dashboard.
    
7. Now that your service is bound to your cluster, you must configure your app to [access the service credentials in the Kubernetes secret](#adding_app). 


## Accessing service credentials from your apps
{: #adding_app}

To access an {{site.data.keyword.Bluemix_notm}} service instance from your app, you must make the service credentials that are stored in the Kubernetes secret available to your app. 
{: shortdesc}

The credentials of a service instance are base64 encoded and stored inside your secret in JSON format. To access the data in your secret, choose among the following options:
- [Mount the secret as a volume to your pod](#mount_secret)
- [Reference the secret in environment variables](#reference_secret)

Before your begin: 
- [Target your CLI](cs_cli_install.html#cs_cli_configure) to your cluster.
- [Add an {{site.data.keyword.Bluemix_notm}} service to your cluster](#adding_cluster). 

### Mounting the secret as a volume to your pod
{: #mount_secret}

When you mount the secret as a volume to your pod, a file that is named `binding` is stored in the volume mount directory. The `binding` file in JSON format includes all the information and credentials that you need to access the {{site.data.keyword.Bluemix_notm}} service.
{: shortdesc}

1.  List available secrets in your cluster and note the **name** of your secret. Look for a secret of type **Opaque**. If multiple secrets exist, contact your cluster administrator to identify the correct service secret.

    ```
    kubectl get secrets
    ```
    {: pre}

    Example output:

    ```
    NAME                              TYPE            DATA      AGE
    binding-<service_instance_name>   Opaque          1         3m

    ```
    {: screen}

2.  Create a YAML file for your Kubernetes deployment and mount the secret as a volume to your pod. 
    ```
    apiVersion: apps/v1beta1
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
          - image: registry.bluemix.net/ibmliberty:latest
            name: secret-test
            volumeMounts:
            - mountPath: <mount_path>
              name: <volume_name>
          volumes:
          - name: <volume_name>
            secret:
              defaultMode: 420
              secretName: binding-<service_instance_name>
    ```
    {: codeblock}

    <table>
    <caption>Understanding the YAML file components</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the YAML file components</th>
    </thead>
    <tbody>
    <tr>
    <td><code>volumeMounts/mountPath</code></td>
    <td>The absolute path of the directory to where the volume is mounted inside the container.</td>
    </tr>
    <tr>
    <td><code>volumeMounts/name</code></br><code>volumes/name</code></td>
    <td>The name of the volume to mount to your pod.</td>
    </tr>
    <tr>
    <td><code>secret/defaultMode</code></td>
    <td>The read and write permissions on the secret. Use `420` to set read-only permissions. </td>
    </tr>
    <tr>
    <td><code>secret/secretName</code></td>
    <td>The name of the secret that you noted in the previous step.</td>
    </tr></tbody></table>

3.  Create the pod and mount the secret as a volume.
    ```
    kubectl apply -f secret-test.yaml
    ```
    {: pre}

4.  Verify that the pod is created.
    ```
    kubectl get pods
    ```
    {: pre}

    Example CLI output:

    ```
    NAME                           READY     STATUS    RESTARTS   AGE
    secret-test-1111454598-gfx32   1/1       Running   0          1m
    ```
    {: screen}

5.  Access the service credentials. 
    1. Log in to your pod. 
       ```
       kubectl exec <pod_name> -it -- /bin/sh
       ```
       {: pre}
       
    2. Navigate to your volume mount path that you defined earlier and list the files in your volume mount path.
       ```
       cd <volume_mountpath> && ls
       ```
       {: pre}
       
       Example output: 
       ```
       binding
       ```
       {: screen}
       
       The `binding` file includes the service credentials that you stored in the Kubernetes secret. 
       
    4. View the service credentials. The credentials are stored as key value pairs in JSON format. 
       ```
       cat binding
       ```
       {: pre}
       
       Example output: 
       ```
       {"apikey":"KL34Ys893284NGJEPFjgrioJ12NElpow","host":"98765aab-9ce1-7tr3-ba87-bfbab6e6d9d6-bluemix.cloudant.com","iam_apikey_description":"Auto generated apikey during resource-key operation for Instance - crn:v1:bluemix:public:cloudantnosqldb:us-south:a/1234g56789cfe8e6388dd2ec098:98746cw-43d7-49ce-947a-d8fe3eebb381::","iam_apikey_name":"auto-generated-apikey-1234abcde-987f-3t64-9d96-d13775ec5663","iam_role_crn":"crn:v1:bluemix:public:iam::::serviceRole:Writer","iam_serviceid_crn":"crn:v1:bluemix:public:iam-identity::a/1234567890brasge5htn2ec098::serviceid:ServiceId-12345vgh-6c4c-ytr12-af6b-467d30d6ef44","password":"jfiavhui12484fnivhuo472nvei23913c3ff","port":443,"url":"https://25c73aac-9ce1-4c24-ba98-bfbab6e6d9d6-bluemix:ugvioev823inreuiegn43donvri29989wiu9t22@25c73aac-9ce1-4c24-ba98-abdrjio123562lnsb-bluemix.cloudant.com","username":"123b45da-9ce1-4c24-ab12-rinwnwub1294-bluemix"}
       ```
       {: screen}
       
    5. Configure your app to parse the JSON content and retrieve the information that you need to access your service. 


### Referencing the secret in environment variables
{: #reference_secret}

You can add the service credentials and other key value pairs from your Kubernetes secret as environment variables to your deployment.   
{: shortdesc}

1. List available secrets in your cluster and note the **name** of your secret. Look for a secret of type **Opaque**. If multiple secrets exist, contact your cluster administrator to identify the correct service secret.

    ```
    kubectl get secrets
    ```
    {: pre}

    Example output:

    ```
    NAME                              TYPE            DATA      AGE
    binding-<service_instance_name>   Opaque          1         3m
    ```
    {: screen}
    
2. Get the details of your secret to find potential key value pairs that you can reference as environment variables in your pod. The service credentials are stored in the `binding` key of your secret. 
   ```
   kubectl get secrets binding-<service_instance_name> --namespace=<namespace> -o yaml
   ```
   {: pre}
    
   Example output:
   ```
   apiVersion: v1
   data:
     binding: <binding>
   kind: Secret
   metadata:
     annotations:
       service-instance-id: 7123acde-c3ef-4ba2-8c52-439ac007fa70
       service-key-id: 9h30dh8a-023f-4cf4-9d96-d12345ec7890
     creationTimestamp: 2018-08-07T20:47:14Z
     name: binding-<service_instance_name>
     namespace: <namespace>
     resourceVersion: "6145900"
     selfLink: /api/v1/namespaces/default/secrets/binding-mycloudant
     uid: 12345a31-9a83-11e8-ba83-cd49014748f
   type: Opaque
   ```
   {: screen}

3. Create a YAML file for your Kubernetes deployment and specify an environment variable that references the `binding` key. 
   ```
   apiVersion: apps/v1beta1
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
     template:
       metadata:
         labels:
           app: secret-test
       spec:
         containers:
         - image: registry.bluemix.net/ibmliberty:latest
           name: secret-test
           env:
           - name: BINDING
             valueFrom:
               secretKeyRef:
                 name: binding-<service_instance_name>
                 key: binding
     ```
     {: codeblock}
     
     <table>
     <caption>Understanding the YAML file components</caption>
     <thead>
     <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the YAML file components</th>
     </thead>
     <tbody>
     <tr>
     <td><code>containers/env/name</code></td>
     <td>The name of your environment variable.</td>
     </tr>
     <tr>
     <td><code>env/valueFrom/secretKeyRef/name</code></td>
     <td>The name of the secret that you noted in the previous step.</td>
     </tr>
     <tr>
     <td><code>env/valueFrom/secretKeyRef/key</code></td>
     <td>The key that is part of your secret and that you want to reference in your environment variable. To reference the service credentials, you must use the <strong>binding</strong> key.  </td>
     </tr>
     </tbody></table>

4. Create the pod that references the `binding` key of your secret as an environment variable.
   ```
   kubectl apply -f secret-test.yaml
   ```
   {: pre}

5. Verify that the pod is created.
   ```
   kubectl get pods
   ```
   {: pre}

   Example CLI output:
   ```
   NAME                           READY     STATUS    RESTARTS   AGE
   secret-test-1111454598-gfx32   1/1       Running   0          1m
   ```
   {: screen}
    
6. Verify that the environment variable is set correctly. 
   1. Log in to your pod. 
      ```
      kubectl exec <pod_name> -it bash
      ```
      {: pre}
      
   2. List all environment variables in the pod. 
      ```
      env
      ```
      {: pre}
      
      Example output: 
      ```
      BINDING={"apikey":"KL34Ys893284NGJEPFjgrioJ12NElpow","host":"98765aab-9ce1-7tr3-ba87-bfbab6e6d9d6-bluemix.cloudant.com","iam_apikey_description":"Auto generated apikey during resource-key operation for Instance - crn:v1:bluemix:public:cloudantnosqldb:us-south:a/1234g56789cfe8e6388dd2ec098:98746cw-43d7-49ce-947a-d8fe3eebb381::","iam_apikey_name":"auto-generated-apikey-1234abcde-987f-3t64-9d96-d13775ec5663","iam_role_crn":"crn:v1:bluemix:public:iam::::serviceRole:Writer","iam_serviceid_crn":"crn:v1:bluemix:public:iam-identity::a/1234567890brasge5htn2ec098::serviceid:ServiceId-12345vgh-6c4c-ytr12-af6b-467d30d6ef44","password":"jfiavhui12484fnivhuo472nvei23913c3ff","port":443,"url":"https://25c73aac-9ce1-4c24-ba98-bfbab6e6d9d6-bluemix:ugvioev823inreuiegn43donvri29989wiu9t22@25c73aac-9ce1-4c24-ba98-abdrjio123562lnsb-bluemix.cloudant.com","username":"123b45da-9ce1-4c24-ab12-rinwnwub1294-bluemix"}
      ```
      {: screen}

7. Configure your app to read the environment variable and to parse the JSON content to retrieve the information that you need to access your service. 

   Example code in Python: 
   ```
   if os.environ.get('BINDING'):
        credentials = json.loads(os.environ.get('BINDING'))
   ```
   {: codeblock}

## Setting up Helm in {{site.data.keyword.containerlong_notm}}
{: #helm}

[Helm ![External link icon](../icons/launch-glyph.svg "External link icon")](https://helm.sh) is a Kubernetes package manager. You can create Helm charts or use preexisting Helm charts to define, install, and upgrade complex Kubernetes applications that run in {{site.data.keyword.containerlong_notm}} clusters.
{:shortdesc}

Before you use Helm charts with {{site.data.keyword.containerlong_notm}}, you must install and initialize a Helm instance in your cluster. You can then add the {{site.data.keyword.Bluemix_notm}} Helm repository to your Helm instance.

Before you begin, [target your CLI](cs_cli_install.html#cs_cli_configure) to the cluster where you want to use a Helm chart.

1. Install the <a href="https://docs.helm.sh/using_helm/#installing-helm" target="_blank">Helm CLI <img src="../icons/launch-glyph.svg" alt="External link icon"></a>.

2. **Important**: To maintain cluster security, create a service account for Tiller in the `kube-system` namespace and a Kubernetes RBAC cluster role binding for the `tiller-deploy` pod.

    1. In your preferred editor, create the following file and save it as `rbac-config.yaml`.
      **Note**:
        * To install Tiller with the service account and cluster role binding in the `kube-system` namespace, you must have the [`cluster-admin` role](cs_users.html#access_policies). You can choose a namespace other than `kube-system`, but all IBM Helm charts must be installed in `kube-system`. Whenever you run a `helm` command, you must use the `tiller-namespace <namespace>` flag to point to the other namespace where Tiller is installed.

      ```
      apiVersion: v1
      kind: ServiceAccount
      metadata:
        name: tiller
        namespace: kube-system
      ---
      apiVersion: rbac.authorization.k8s.io/v1
      kind: ClusterRoleBinding
      metadata:
        name: tiller
      roleRef:
        apiGroup: rbac.authorization.k8s.io
        kind: ClusterRole
        name: cluster-admin
      subjects:
        - kind: ServiceAccount
          name: tiller
          namespace: kube-system
      ```
      {: codeblock}

    2. Create the service account and cluster role binding.

        ```
        kubectl create -f rbac-config.yaml
        ```
        {: pre}

3. Initialize Helm and install `tiller` with the service account that you created.

    ```
    helm init --service-account tiller
    ```
    {: pre}

4. Verify that the `tiller-deploy` pod has a **Status** of `Running` in your cluster.

    ```
    kubectl get pods -n kube-system -l app=helm
    ```
    {: pre}

    Example output:

    ```
    NAME                            READY     STATUS    RESTARTS   AGE
    tiller-deploy-352283156-nzbcm   1/1       Running   0          2m
    ```
    {: screen}

5. Add the {{site.data.keyword.Bluemix_notm}} Helm repository to your Helm instance.

    ```
    helm repo add ibm  https://registry.bluemix.net/helm/ibm
    ```
    {: pre}

6. List the Helm charts that are currently available in the {{site.data.keyword.Bluemix_notm}} repository.

    ```
    helm search ibm
    ```
    {: pre}

7. To learn more about a chart, list its settings and default values.

    For example, to view the settings, documentation, and default values for the strongSwan IPSec VPN service Helm chart:

    ```
    helm inspect ibm/strongswan
    ```
    {: pre}


### Related Helm links
{: #helm_links}

* To use the strongSwan Helm chart, see [Setting up VPN connectivity with the strongSwan IPSec VPN service Helm chart](cs_vpn.html#vpn-setup).
* View the available Helm charts that you can use with {{site.data.keyword.Bluemix_notm}} in the [Helm Charts Catalog ![External link icon](../icons/launch-glyph.svg "External link icon")](https://console.bluemix.net/containers-kubernetes/solutions/helm-charts) GUI.
* Learn more about the Helm commands that are used to set up and manage Helm charts in the <a href="https://docs.helm.sh/helm/" target="_blank">Helm documentation <img src="../icons/launch-glyph.svg" alt="External link icon"></a>.
* Learn more about how you can [increase deployment velocity with Kubernetes Helm Charts ![External link icon](../icons/launch-glyph.svg "External link icon")](https://developer.ibm.com/recipes/tutorials/increase-deployment-velocity-with-kubernetes-helm-charts/).

## Visualizing Kubernetes cluster resources
{: #weavescope}

Weave Scope provides a visual diagram of your resources within a Kubernetes cluster, including services, pods, containers, and more. Weave Scope provides interactive metrics for CPU and memory and tools to tail and exec into a container.
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

4.  Run a port forwarding command to open the service on your computer. The next time that you access Weave Scope, you can run this port forwarding command without completing the previous configuration steps again.

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

