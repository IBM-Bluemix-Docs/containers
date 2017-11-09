---

copyright:
  years: 2014, 2017
lastupdated: "2017-11-07"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}


# Deploying apps in clusters
{: #cs_apps}

You can use Kubernetes techniques to deploy apps and to ensure your apps are up and running at all times. For example, you can perform rolling updates and rollbacks without downtime for your users.
{:shortdesc}

Deploying an app generally includes the following steps.

1.  [Install the CLIs](cs_cli_install.html#cs_cli_install).

2.  Create a configuration file for your app. [Review the best practices from Kubernetes. ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/concepts/configuration/overview/)

3.  Run the configuration file by using one of the following methods.
    -   [The Kubernetes CLI](#cs_apps_cli)
    -   The Kubernetes dashboard
        1.  [Start the Kubernetes dashboard.](#cs_cli_dashboard)
        2.  [Run the configuration file.](#cs_apps_ui)

<br />


## Launching the Kubernetes dashboard
{: #cs_cli_dashboard}

Open a Kubernetes dashboard on your local system to view information about a cluster and its worker nodes.
{:shortdesc}

Before you begin, [target your CLI](cs_cli_install.html#cs_cli_configure) to your cluster. This task requires the [Administrator access policy](cs_cluster.html#access_ov). Verify your current [access policy](cs_cluster.html#view_access).

You can use the default port or set your own port to launch the Kubernetes dashboard for a cluster.

1.  For clusters with a Kubernetes master version of 1.7.4 or earlier:

    1.  Set the proxy with the default port number.

        ```
        kubectl proxy
        ```
        {: pre}

        Output:

        ```
        Starting to serve on 127.0.0.1:8001
        ```
        {: screen}

    2.  Open the Kubernetes dashboard in a web browser.

        ```
        http://localhost:8001/ui
        ```
        {: codeblock}

2.  For clusters with a Kubernetes master version of 1.8.2 or later:

    1.  Download your credentials.

        ```
        bx cs cluster-config <cluster_name>
        ```
        {: codeblock}

    2.  View the cluster credentials that you downloaded. Use the filepath that is specified in the export in the previous step.

        For macOS or Linux:

        ```
        cat <filepath_to_cluster_credentials>
        ```
        {: codeblock}

        For Windows:

        ```
        type <filepath_to_cluster_credentials>
        ```
        {: codeblock}

    3.  Copy the token in the **id-token** field.

    4.  Set the proxy with the default port number.

        ```
        kubectl proxy
        ```
        {: pre}

        Your CLI output looks as follows:

        ```
        Starting to serve on 127.0.0.1:8001
        ```
        {: screen}

    6.  Sign in to the dashboard.

        1.  Copy this URL into your browser.

            ```
            http://localhost:8001/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy/
            ```
            {: codeblock}

        2.  In the sign on page, select the **Token** authentication method.

        3.  Then, paste the **id-token** value into the **Token** field and click **SIGN IN**.

When you are done with the Kubernetes dashboard, use `CTRL+C` to exit the `proxy` command. After you exit, the Kubernetes dashboard is no longer available. Run the `proxy` command to restart the Kubernetes dashboard.

<br />


## Creating secrets
{: #secrets}

Kubernetes secrets are a secure way to store confidential information, such as user names, passwords, or keys.


<table>
<caption>Table. Files needed to be stored in secrets by task</caption>
<thead>
<th>Task</th>
<th>Required files to store in secrets</th>
</thead>
<tbody>
<tr>
<td>Add a service to a cluster</td>
<td>None. A secret is created for you when you bind a service to a cluster.</td>
</tr>
<tr>
<td>Optional: Configure the Ingress service with TLS, if you are not using ingress-secret. <p><b>Note</b>: TLS is already enabled by default and a secret is created already for the TLS Connection.

To view the default TLS secret:
<pre>
bx cs cluster-get &gt;CLUSTER-NAME&lt; &#124; grep "Ingress secret"
</pre>
</p>
To create your own instead, complete the steps in this topic.</td>
<td>Server cert and key: <code>server.crt</code> and <code>server.key</code></td>
<tr>
<td>Create the mutual-authentication annotation.</td>
<td>CA cert: <code>ca.crt</code></td>
</tr>
</tbody>
</table>

For more information on what you can store in secrets, see the [Kubernetes documentation](https://kubernetes.io/docs/concepts/configuration/secret/).



To create a secret with a certificate:

1. Generate the certificate authority (CA) cert and key from your certificate provider. If you have your own domain, purchase an official TLS certificate for your domain. For test purposes, you can generate a self-signed certificate.

 Important: Make sure the [CN](https://support.dnsimple.com/articles/what-is-common-name/) is different for each certificate.

 The client cert and client key must be verified up to the trusted root certificate, which in this case, is the CA cert. Example:

 ```
 Client Certificate: issued by Intermediate Certificate
 Intermediate Certificate: issued by Root Certificate
 Root Certificate: issued by itself
 ```
 {: codeblock}

2. Create the certificate as a Kubernetes secret.

 ```
 kubectl create secret generic <secretName> --from-file=<cert_file>=<cert_file>
 ```
 {: pre}

 Examples:
 - TLS connection:

 ```
 kubectl create secret tls <secretName> --from-file=tls.crt=server.crt --from-file=tls.key=server.key
 ```
 {: pre}

 - Mutual authentication annotation:

 ```
 kubectl create secret generic <secretName> --from-file=ca.crt=ca.crt
 ```
 {: pre}

<br />



## Allowing public access to apps
{: #cs_apps_public}

To make an app publicly available to the internet, you must update your configuration file before you deploy the app into a cluster.
{:shortdesc}

Depending on whether you created a lite or a standard cluster, different ways exist to make your app accessible from the internet.

<dl>
<dt><a href="#cs_apps_public_nodeport" target="_blank">NodePort service</a> (lite and standard clusters)</dt>
<dd>Expose a public port on every worker node and use the public IP address of any worker node to publicly access your service in the cluster. The public IP address of the worker node is not permanent. When a worker node is removed or re-created, a new public IP address is assigned to the worker node. You can use the NodePort service for testing the public access for your app or when public access is needed for a short amount of time only. When you require a stable public IP address and more availability for your service endpoint, expose your app by using a LoadBalancer service or Ingress.</dd>
<dt><a href="#cs_apps_public_load_balancer" target="_blank">LoadBalancer service</a> (standard clusters only)</dt>
<dd>Every standard cluster is provisioned with 4 portable public and 4 portable private IP addresses that you can use to create an external TCP/ UDP load balancer for your app. You can customize your load balancer by exposing any port that your app requires. The portable public IP address that is assigned to the load balancer is permanent and does not change when a worker node is re-created in the cluster.

</br>
If you need HTTP or HTTPS load balancing for your app and want to use one public route to expose multiple apps in your cluster as services, use the built-in Ingress support with {{site.data.keyword.containershort_notm}}.</dd>
<dt><a href="#cs_apps_public_ingress" target="_blank">Ingress</a> (standard clusters only)</dt>
<dd>Expose multiple apps in your cluster by creating one external HTTP or HTTPS load balancer that uses a secured and unique public entrypoint to route incoming requests to your apps. Ingress consists of two main components, the Ingress resource and the Ingress controller. The Ingress resource defines the rules for how to route and load balance incoming requests for an app. All Ingress resources must be registered with the Ingress controller that listens for incoming HTTP or HTTPS service requests and forwards requests based on the rules defined for each Ingress resource. Use Ingress if you want to implement your own load balancer with custom routing rules and if you need SSL termination for your apps.

</dd></dl>

### Configuring public access to an app by using the NodePort service type
{: #cs_apps_public_nodeport}

Make your app available to internet access by using the public IP address of any worker node in a cluster and exposing a node port. Use this option for testing and short-term public access.

{:shortdesc}

You can expose your app as a Kubernetes NodePort service for lite or standard clusters.

For {{site.data.keyword.Bluemix_dedicated_notm}} environments, public IP addresses are blocked by a firewall. To make an app publicly available, use a [LoadBalancer service](#cs_apps_public_load_balancer) or [Ingress](#cs_apps_public_ingress) instead.

**Note:** The public IP address of a worker node is not permanent. If the worker node must be re-created, a new public IP address is assigned to the worker node. If you need a stable public IP address and more availability for your service, expose your app by using a [LoadBalancer service](#cs_apps_public_load_balancer) or [Ingress](#cs_apps_public_ingress).




1.  Define a [service ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/concepts/services-networking/service/) section in the configuration file.
2.  In the `spec` section for the service, add the NodePort type.

    ```
    spec:
      type: NodePort
    ```
    {: codeblock}

3.  Optional: In the `ports` section, add a NodePort in the 30000 - 32767 range. Do not specify a NodePort that is already in use by another service. If you are unsure which NodePorts are already in use, do not assign one. If no NodePort is assigned, a random one is assigned for you.

    ```
    ports:
      - port: 80
        nodePort: 31514
    ```
    {: codeblock}

    If you want to specify a NodePort and want to see which NodePorts are already in use, you can run the following command.

    ```
    kubectl get svc
    ```
    {: pre}

    Output:

    ```
    NAME           CLUSTER-IP     EXTERNAL-IP   PORTS          AGE
    myapp          10.10.10.83    <nodes>       80:31513/TCP   28s
    redis-master   10.10.10.160   <none>        6379/TCP       28s
    redis-slave    10.10.10.194   <none>        6379/TCP       28s
    ```
    {: screen}

4.  Save the changes.
5.  Repeat to create a service for every app.

    Example:

    ```
    apiVersion: v1
    kind: Service
    metadata:
      name: my-nodeport-service
      labels:
        run: my-demo
    spec:
      selector:
        run: my-demo
      type: NodePort
      ports:
       - protocol: TCP
         port: 8081
         # nodePort: 31514

    ```
    {: codeblock}

**What's next:**

When the app is deployed, you can use the public IP address of any worker node and the NodePort to form the public URL to access the app in a browser.

1.  Get the public IP address for a worker node in the cluster.

    ```
    bx cs workers <cluster_name>
    ```
    {: pre}

    Output:

    ```
    ID                                                Public IP   Private IP    Size     State    Status
    prod-dal10-pa215dcf5bbc0844a990fa6b0fcdbff286-w1  192.0.2.23  10.100.10.10  u1c.2x4  normal   Ready
    prod-dal10-pa215dcf5bbc0844a990fa6b0fcdbff286-w2  192.0.2.27  10.100.10.15  u1c.2x4  normal   Ready
    ```
    {: screen}

2.  If a random NodePort was assigned, find out which one was assigned.

    ```
    kubectl describe service <service_name>
    ```
    {: pre}

    Output:

    ```
    Name:                   <service_name>
    Namespace:              default
    Labels:                 run=<deployment_name>
    Selector:               run=<deployment_name>
    Type:                   NodePort
    IP:                     10.10.10.8
    Port:                   <unset> 8080/TCP
    NodePort:               <unset> 30872/TCP
    Endpoints:              172.30.171.87:8080
    Session Affinity:       None
    No events.
    ```
    {: screen}

    In this example, the NodePort is `30872`.

3.  Form the URL with one of the worker node public IP addresses and the NodePort. Example: `http://192.0.2.23:30872`

### Configuring access to an app by using the load balancer service type
{: #cs_apps_public_load_balancer}

Expose a port and use a portable IP address for the load balancer to access the app. Use a public IP address to make an app accessible on the internet, or a private IP address to make an app accessible on your private infrastructure network.

Unlike with a NodePort service, the portable IP address of the load balancer service is not dependent on the worker node that the app is deployed on. However, a Kubernetes LoadBalancer service is also a NodePort service. A LoadBalancer service makes your app available over the load balancer IP address and port and makes your app available over the service's node ports.

The portable IP address of the load balancer is assigned for you and does not change when you add or remove worker nodes. Therefore, load balancer services are more highly available than NodePort services. Users can select any port for the load balancer and are not limited to the NodePort port range. You can use load balancer services for TCP and UDP protocols.

When an {{site.data.keyword.Bluemix_dedicated_notm}} account is [enabled for clusters](cs_ov.html#setup_dedicated), you can request public subnets to be used for load balancer IP addresses. [Open a support ticket](/docs/support/index.html#contacting-support) to create the subnet, and then use the [`bx cs cluster-subnet-add`](cs_cli_reference.html#cs_cluster_subnet_add) command to add the subnet to the cluster.

**Note:** Load balancer services do not support TLS termination. If your app requires TLS termination, you can expose your app by using [Ingress](#cs_apps_public_ingress), or configure your app to manage the TLS termination.

Before you begin:

-   This feature is available for standard clusters only.
-   You must have a portable public or private IP address available to assign to the load balancer service.
-   A load balancer service with a portable private IP address still has a public node port open on every worker node. To add a network policy to prevent public traffic, see [Blocking incoming traffic](cs_security.html#cs_block_ingress).

To create a load balancer service:

1.  [Deploy your app to the cluster](#cs_apps_cli). When you deploy your app to the cluster, one or more pods are created for you that run your app in a container. Ensure that you add a label to your deployment in the metadata section of your configuration file. This label is needed to identify all pods where your app is running so that they can be included in the load balancing.
2.  Create a load balancer service for the app that you want to expose. To make your app available on the public internet or on a private network, create a Kubernetes service for your app. Configure your service to include all the pods that make up your app into the load balancing.
    1.  Create a service configuration file that is named, for example, `myloadbalancer.yaml`.
    2.  Define a load balancer service for the app that you want to expose.
        - If your cluster is on a public VLAN, a portable public IP address is used. Most clusters are on a public VLAN.
        - If your cluster is available on a private VLAN only, then a portable private IP address is used.
        - You can request a portable public or private IP address for a LoadBalancer service by adding an annotation to the configuration file.

        LoadBalancer service that uses a default IP address:

        ```
        apiVersion: v1
        kind: Service
        metadata:
          name: <myservice>
        spec:
          type: LoadBalancer
          selector:
            <selectorkey>:<selectorvalue>
          ports:
           - protocol: TCP
             port: 8080
        ```
        {: codeblock}

        LoadBalancer service that uses an annotation to specify a private or public IP address:

        ```
        apiVersion: v1
        kind: Service
        metadata:
          name: <myservice>
          annotations:
            service.kubernetes.io/ibm-load-balancer-cloud-provider-ip-type: <public_or_private>
        spec:
          type: LoadBalancer
          selector:
            <selectorkey>:<selectorvalue>
          ports:
           - protocol: TCP
             port: 8080
        ```
        {: codeblock}

        <table>
        <thead>
        <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the YAML file components</th>
        </thead>
        <tbody>
        <tr>
          <td><code>name</code></td>
          <td>Replace <em>&lt;myservice&gt;</em> with a name for your load balancer service.</td>
        </tr>
        <tr>
          <td><code>selector</code></td>
          <td>Enter the label key (<em>&lt;selectorkey&gt;</em>) and value (<em>&lt;selectorvalue&gt;</em>) pair that you want to use to target the pods where your app runs. For example, if you use the following selector <code>app: code</code>, all pods that have this label in their metadata are included in the load balancing. Enter the same label that you used when you deployed your app to the cluster. </td>
        </tr>
        <tr>
          <td><code>port</code></td>
          <td>The port that the service listens on.</td>
        </tr>
        <tr>
          <td>`service.kubernetes.io/ibm-load-balancer-cloud-provider-ip-type:`
          <td>Annotation to specify the type of LoadBalancer. The values are `private` and `public`. When creating a public LoadBalancer in clusters on public VLANs, this annotation is not required.</td>
        </tbody></table>
    3.  Optional: To use a specific portable IP address for your load balancer that is available to your cluster, you can specify that IP address by including the `loadBalancerIP` in the spec section. For more information, see the [Kubernetes documentation ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/concepts/services-networking/service/).
    4.  Optional: Configure a firewall by specifying the `loadBalancerSourceRanges` in the spec section. For more information, see the [Kubernetes documentation ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/tasks/access-application-cluster/configure-cloud-provider-firewall/).
    5.  Create the service in your cluster.

        ```
        kubectl apply -f myloadbalancer.yaml
        ```
        {: pre}

        When your load balancer service is created, a portable IP address is automatically assigned to the load balancer. If no portable IP address is available, the load balancer service cannot be created.
3.  Verify that the load balancer service was created successfully. Replace _&lt;myservice&gt;_ with the name of the load balancer service that you created in the previous step.

    ```
    kubectl describe service <myservice>
    ```
    {: pre}

    **Note:** It might take a few minutes for the load balancer service to be created properly and for the app to be available.

    Example CLI output:

    ```
    Name:                   <myservice>
    Namespace:              default
    Labels:                 <none>
    Selector:               <selectorkey>=<selectorvalue>
    Type:                   LoadBalancer
    IP:                     10.10.10.90
    LoadBalancer Ingress:   192.168.10.38
    Port:                   <unset> 8080/TCP
    NodePort:               <unset> 32040/TCP
    Endpoints:              172.30.171.87:8080
    Session Affinity:       None
    Events:
    FirstSeen	LastSeen	Count	From			SubObjectPath	Type		Reason			Message
      ---------	--------	-----	----			-------------	--------	------			-------
      10s		10s		1	{service-controller }			Normal		CreatingLoadBalancer	Creating load balancer
      10s		10s		1	{service-controller }			Normal		CreatedLoadBalancer	Created load balancer
    ```
    {: screen}

    The **LoadBalancer Ingress** IP address is the portable IP address that was assigned to your load balancer service.
4.  If you created a public load balancer, access your app from the internet.
    1.  Open your preferred web browser.
    2.  Enter the portable public IP address of the load balancer and port. In the example above, the portable public IP address `192.168.10.38` was assigned to the load balancer service.

        ```
        http://192.168.10.38:8080
        ```
        {: codeblock}




### Configuring public access to an app by using the Ingress controller
{: #cs_apps_public_ingress}

Expose multiple apps in your cluster by creating Ingress resources that are managed by the IBM-provided Ingress controller. The Ingress controller is an external HTTP or HTTPS load balancer that uses a secured and unique public entrypoint to route incoming requests to your apps inside or outside your cluster.

**Note:** Ingress is available for standard clusters only and requires at least two worker nodes in the cluster to ensure high availability. Setting up Ingress requires an [Administrator access policy](cs_cluster.html#access_ov). Verify your current [access policy](cs_cluster.html#view_access).

When you create a standard cluster, an Ingress controller is automatically created for you and assigned a portable public IP address and a public route. You can configure the Ingress controller and define individual routing rules for every app that you expose to the public. Every app that is exposed via Ingress is assigned a unique path that is appended to the public route, so that you can use a unique URL to access an app publicly in your cluster.

When an {{site.data.keyword.Bluemix_dedicated_notm}} account is [enabled for clusters](cs_ov.html#setup_dedicated), you can request public subnets to be used for Ingress controller IP addresses. Then, the Ingress controller is created and a public route is assigned. [Open a support ticket](/docs/support/index.html#contacting-support) to create the subnet, and then use the [`bx cs cluster-subnet-add`](cs_cli_reference.html#cs_cluster_subnet_add) command to add the subnet to the cluster.

You can configure the Ingress controller for the following scenarios.

-   [Use the IBM-provided domain without TLS termination](#ibm_domain)
-   [Use the IBM-provided domain with TLS termination](#ibm_domain_cert)
-   [Use a custom domain and TLS certificate to do TLS termination](#custom_domain_cert)
-   [Use the IBM-provided or a custom domain with TLS termination to access apps outside your cluster](#external_endpoint)
-   [Opening ports in the Ingress load balancer](#opening_ingress_ports)
-   [Configuring SSL protocols and SSL ciphers at the HTTP level](#ssl_protocols_ciphers)
-   [Customize your Ingress controller with annotations](cs_annotations.html)
{: #ingress_annotation}


#### Using the IBM-provided domain without TLS termination
{: #ibm_domain}

You can configure the Ingress controller as an HTTP load balancer for the apps in your cluster and use the IBM-provided domain to access your apps from the internet.

Before you begin:

-   If you do not have one already, [create a standard cluster](cs_cluster.html#cs_cluster_ui).
-   [Target your CLI](cs_cli_install.html#cs_cli_configure) to your cluster to run `kubectl` commands.

To configure the Ingress controller:

1.  [Deploy your app to the cluster](#cs_apps_cli). When you deploy your app to the cluster, one or more pods are created for you that run your app in a container. Ensure that you add a label to your deployment in the metadata section of your configuration file. This label is needed to identify all pods where your app is running, so that they can be included in the Ingress load balancing.
2.  Create a Kubernetes service for the app to expose. The Ingress controller can include your app into the Ingress load balancing only if your app is exposed via a Kubernetes service inside the cluster.
    1.  Open your preferred editor and create a service configuration file that is named, for example, `myservice.yaml`.
    2.  Define a service for the app that you want to expose to the public.

        ```
        apiVersion: v1
        kind: Service
        metadata:
          name: <myservice>
        spec:
          selector:
            <selectorkey>: <selectorvalue>
          ports:
           - protocol: TCP
             port: 8080
        ```
        {: codeblock}

        <table>
        <thead>
        <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the YAML file components</th>
        </thead>
        <tbody>
        <tr>
        <td><code>name</code></td>
        <td>Replace <em>&lt;myservice&gt;</em> with a name for your load balancer service.</td>
        </tr>
        <tr>
        <td><code>selector</code></td>
        <td>Enter the label key (<em>&lt;selectorkey&gt;</em>) and value (<em>&lt;selectorvalue&gt;</em>) pair that you want to use to target the pods where your app runs. For example, if you use the following selector <code>app: code</code>, all pods that have this label in their metadata are included in the load balancing. Enter the same label that you used when you deployed your app to the cluster. </td>
         </tr>
         <tr>
         <td><code>port</code></td>
         <td>The port that the service listens on.</td>
         </tr>
         </tbody></table>
    3.  Save your changes.
    4.  Create the service in your cluster.

        ```
        kubectl apply -f myservice.yaml
        ```
        {: pre}
    5.  Repeat these steps for every app that you want to expose to the public.
3.  Get the details for your cluster to view the IBM-provided domain. Replace _&lt;mycluster&gt;_ with the name of the cluster where the app is deployed that you want to expose to the public.

    ```
    bx cs cluster-get <mycluster>
    ```
    {: pre}

    Your CLI output looks similar to the following.

    ```
    Retrieving cluster <mycluster>...
    OK
    Name:    <mycluster>
    ID:    b9c6b00dc0aa487f97123440b4895f2d
    Created:  2017-04-26T19:47:08+0000
    State:    normal
    Master URL:  https://169.57.40.165:1931
    Ingress subdomain:  <ibmdomain>
    Ingress secret:  <ibmtlssecret>
    Workers:  3
    ```
    {: screen}

    You can see the IBM-provided domain in the **Ingress subdomain** field.
4.  Create an Ingress resource. Ingress resources define the routing rules for the Kubernetes service that you created for your app and are used by the Ingress controller to route incoming network traffic to the service. You can use one Ingress resource to define routing rules for multiple apps as long as every app is exposed via a Kubernetes service inside the cluster.
    1.  Open your preferred editor and create an Ingress configuration file that is named, for example, `myingress.yaml`.
    2.  Define an Ingress resource in your configuration file that uses the IBM-provided domain to route incoming network traffic to the service that you created earlier.

        ```
        apiVersion: extensions/v1beta1
        kind: Ingress
        metadata:
          name: <myingressname>
        spec:
          rules:
          - host: <ibmdomain>
            http:
              paths:
              - path: /<myservicepath1>
                backend:
                  serviceName: <myservice1>
                  servicePort: 80
              - path: /<myservicepath2>
                backend:
                  serviceName: <myservice2>
                  servicePort: 80
        ```
        {: codeblock}

        <table>
        <thead>
        <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the YAML file components</th>
        </thead>
        <tbody>
        <tr>
        <td><code>name</code></td>
        <td>Replace <em>&lt;myingressname&gt;</em> with a name for your Ingress resource.</td>
        </tr>
        <tr>
        <td><code>host</code></td>
        <td>Replace <em>&lt;ibmdomain&gt;</em> with the IBM-provided <strong>Ingress subdomain</strong> name from the previous step.

        </br></br>
        <strong>Note:</strong> Do not use * for your host or leave the host property empty to avoid failures during Ingress creation.</td>
        </tr>
        <tr>
        <td><code>path</code></td>
        <td>Replace <em>&lt;myservicepath1&gt;</em> with a slash or the unique path that your app is listening on, so that network traffic can be forwarded to the app.

        </br>
        For every Kubernetes service, you can define an individual path that is appended to the IBM-provided domain to create a unique path to your app, for example <code>ingress_domain/myservicepath1</code>. When you enter this route into a web browser, network traffic is routed to the Ingress controller. The Ingress controller looks up the associated service, and sends network traffic to the service and to the pods where the app is running by using the same path. The app must be set up to listen on this path in order to receive incoming network traffic.

        </br></br>
        Many apps do not listen on a specific path, but use the root path and a specific port. In this case, define the root path as <code>/</code> and do not specify an individual path for your app.
        </br>
        Examples: <ul><li>For <code>http://ingress_host_name/</code>, enter <code>/</code> as the path.</li><li>For <code>http://ingress_host_name/myservicepath</code>, enter <code>/myservicepath</code> as the path.</li></ul>
        </br>
        <strong>Tip:</strong> If you want to configure your Ingress to listen on a path that is different to the one that your app listens on, you can use the [rewrite annotation](cs_annotations.html#rewrite-path) to establish proper routing to your app.</td>
        </tr>
        <tr>
        <td><code>serviceName</code></td>
        <td>Replace <em>&lt;myservice1&gt;</em> with the name of the service that you used when you created the Kubernetes service for your app.</td>
        </tr>
        <tr>
        <td><code>servicePort</code></td>
        <td>The port that your service listens to. Use the same port that you defined when you created the Kubernetes service for your app.</td>
        </tr>
        </tbody></table>

    3.  Create the Ingress resource for your cluster.

        ```
        kubectl apply -f myingress.yaml
        ```
        {: pre}

5.  Verify that the Ingress resource was created successfully. Replace _&lt;myingressname&gt;_ with the name of the Ingress resource that you created earlier.

    ```
    kubectl describe ingress <myingressname>
    ```
    {: pre}

  **Note:** It might take a few minutes for the Ingress resource to be created and for the app to be available on the public internet.
6.  In a web browser, enter the URL of the app service to access.

    ```
    http://<ibmdomain>/<myservicepath1>
    ```
    {: codeblock}


#### Using the IBM-provided domain with TLS termination
{: #ibm_domain_cert}

You can configure the Ingress controller to manage incoming TLS connections for your apps, decrypt the network traffic by using the IBM-provided TLS certificate, and forward the unencrypted request to the apps that are exposed in your cluster.

Before you begin:

-   If you do not have one already, [create a standard cluster](cs_cluster.html#cs_cluster_ui).
-   [Target your CLI](cs_cli_install.html#cs_cli_configure) to your cluster to run `kubectl` commands.

To configure the Ingress controller:

1.  [Deploy your app to the cluster](#cs_apps_cli). Ensure that you add a label to your deployment in the metadata section of your configuration file. This label identifies all pods where your app is running, so that the pods are included in the Ingress load balancing.
2.  Create a Kubernetes service for the app to expose. The Ingress controller can include your app into the Ingress load balancing only if your app is exposed via a Kubernetes service inside the cluster.
    1.  Open your preferred editor and create a service configuration file that is named, for example, `myservice.yaml`.
    2.  Define a service for the app that you want to expose to the public.

        ```
        apiVersion: v1
        kind: Service
        metadata:
          name: <myservice>
        spec:
          selector:
            <selectorkey>: <selectorvalue>
          ports:
           - protocol: TCP
             port: 8080
        ```
        {: codeblock}

        <table>
        <thead>
        <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the YAML file components</th>
        </thead>
        <tbody>
        <tr>
        <td><code>name</code></td>
        <td>Replace <em>&lt;myservice&gt;</em> with a name for your Kubernetes service.</td>
        </tr>
        <tr>
        <td><code>selector</code></td>
        <td>Enter the label key (<em>&lt;selectorkey&gt;</em>) and value (<em>&lt;selectorvalue&gt;</em>) pair that you want to use to target the pods where your app runs. For example, if you use the following selector <code>app: code</code>, all pods that have this label in their metadata are included in the load balancing. Enter the same label that you used when you deployed your app to the cluster. </td>
         </tr>
         <tr>
         <td><code>port</code></td>
         <td>The port that the service listens on.</td>
         </tr>
         </tbody></table>

    3.  Save your changes.
    4.  Create the service in your cluster.

        ```
        kubectl apply -f myservice.yaml
        ```
        {: pre}

    5.  Repeat these steps for every app that you want to expose to the public.

3.  View the IBM-provided domain and TLS certificate. Replace _&lt;mycluster&gt;_ with the name of the cluster where the app is deployed.

    ```
    bx cs cluster-get <mycluster>
    ```
    {: pre}

    Your CLI output looks similar to the following.

    ```
    bx cs cluster-get <mycluster>
    Retrieving cluster <mycluster>...
    OK
    Name:    <mycluster>
    ID:    b9c6b00dc0aa487f97123440b4895f2d
    Created:  2017-04-26T19:47:08+0000
    State:    normal
    Master URL:  https://169.57.40.165:1931
    Ingress subdomain:  <ibmdomain>
    Ingress secret:  <ibmtlssecret>
    Workers:  3
    ```
    {: screen}

    You can see the IBM-provided domain in the **Ingress subdomain** and the IBM-provided certificate in the **Ingress secret** field.

4.  Create an Ingress resource. Ingress resources define the routing rules for the Kubernetes service that you created for your app and are used by the Ingress controller to route incoming network traffic to the service. You can use one Ingress resource to define routing rules for multiple apps as long as every app is exposed via a Kubernetes service inside the cluster.
    1.  Open your preferred editor and create an Ingress configuration file that is named, for example, `myingress.yaml`.
    2.  Define an Ingress resource in your configuration file that uses the IBM-provided domain to route incoming network traffic to your services, and the IBM-provided certificate to manage the TLS termination for you. For every service you can define an individual path that is appended to the IBM-provided domain to create a unique path to your app, for example `https://ingress_domain/myapp`. When you enter this route into a web browser, network traffic is routed to the Ingress controller. The Ingress controller looks up the associated service and sends network traffic to the service, and further to the pods where the app is running.

        **Note:** Your app must listen on the path that you defined in the Ingress resource. Otherwise, network traffic cannot be forwarded to the app. Most apps do not listen on a specific path, but use the root path and a specific port. In this case, define the root path as `/` and do not specify an individual path for your app.

        ```
        apiVersion: extensions/v1beta1
        kind: Ingress
        metadata:
          name: <myingressname>
        spec:
          tls:
          - hosts:
            - <ibmdomain>
            secretName: <ibmtlssecret>
          rules:
          - host: <ibmdomain>
            http:
              paths:
              - path: /<myservicepath1>
                backend:
                  serviceName: <myservice1>
                  servicePort: 80
              - path: /<myservicepath2>
                backend:
                  serviceName: <myservice2>
                  servicePort: 80
        ```
        {: codeblock}

        <table>
        <thead>
        <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the YAML file components</th>
        </thead>
        <tbody>
        <tr>
        <td><code>name</code></td>
        <td>Replace <em>&lt;myingressname&gt;</em> with a name for your Ingress resource.</td>
        </tr>
        <tr>
        <td><code>tls/hosts</code></td>
        <td>Replace <em>&lt;ibmdomain&gt;</em> with the IBM-provided <strong>Ingress subdomain</strong> name from the previous step. This domain is configured for TLS termination.

        </br></br>
        <strong>Note:</strong> Do not use &ast; for your host or leave the host property empty to avoid failures during Ingress creation.</td>
        </tr>
        <tr>
        <td><code>tls/secretName</code></td>
        <td>Replace <em>&lt;ibmtlssecret&gt;</em> with the IBM-provided <strong>Ingress secret</strong> name from the previous step. This certificate manages the TLS termination.
        </tr>
        <tr>
        <td><code>host</code></td>
        <td>Replace <em>&lt;ibmdomain&gt;</em> with the IBM-provided <strong>Ingress subdomain</strong> name from the previous step. This domain is configured for TLS termination.

        </br></br>
        <strong>Note:</strong> Do not use &ast; for your host or leave the host property empty to avoid failures during Ingress creation.</td>
        </tr>
        <tr>
        <td><code>path</code></td>
        <td>Replace <em>&lt;myservicepath1&gt;</em> with a slash or the unique path that your app is listening on, so that network traffic can be forwarded to the app.

        </br>
        For every Kubernetes service, you can define an individual path that is appended to the IBM-provided domain to create a unique path to your app, for example <code>ingress_domain/myservicepath1</code>. When you enter this route into a web browser, network traffic is routed to the Ingress controller. The Ingress controller looks up the associated service, and sends network traffic to the service and to the pods where the app is running by using the same path. The app must be set up to listen on this path in order to receive incoming network traffic.

        </br>
        Many apps do not listen on a specific path, but use the root path and a specific port. In this case, define the root path as <code>/</code> and do not specify an individual path for your app.

        </br>
        Examples: <ul><li>For <code>http://ingress_host_name/</code>, enter <code>/</code> as the path.</li><li>For <code>http://ingress_host_name/myservicepath</code>, enter <code>/myservicepath</code> as the path.</li></ul>
        <strong>Tip:</strong> If you want to configure your Ingress to listen on a path that is different to the one that your app listens on, you can use the [rewrite annotation](cs_annotations.html#rewrite-path) to establish proper routing to your app.</td>
        </tr>
        <tr>
        <td><code>serviceName</code></td>
        <td>Replace <em>&lt;myservice1&gt;</em> with the name of the service that you used when you created the Kubernetes service for your app.</td>
        </tr>
        <tr>
        <td><code>servicePort</code></td>
        <td>The port that your service listens to. Use the same port that you defined when you created the Kubernetes service for your app.</td>
        </tr>
        </tbody></table>

    3.  Create the Ingress resource for your cluster.

        ```
        kubectl apply -f myingress.yaml
        ```
        {: pre}

5.  Verify that the Ingress resource was created successfully. Replace _&lt;myingressname&gt;_ with the name of the Ingress resource that you created earlier.

    ```
    kubectl describe ingress <myingressname>
    ```
    {: pre}

    **Note:** It might take a few minutes for the Ingress resource to be created properly and for the app to be available on the public internet.
6.  In a web browser, enter the URL of the app service to access.

    ```
    https://<ibmdomain>/<myservicepath1>
    ```
    {: codeblock}

#### Using the Ingress controller with a custom domain and TLS certificate
{: #custom_domain_cert}

You can configure the Ingress controller to route incoming network traffic to the apps in your cluster and use your own TLS certificate to manage the TLS termination, while using your custom domain rather than the IBM-provided domain.
{:shortdesc}

Before you begin:

-   If you do not have one already, [create a standard cluster](cs_cluster.html#cs_cluster_ui).
-   [Target your CLI](cs_cli_install.html#cs_cli_configure) to your cluster to run `kubectl` commands.

To configure the Ingress controller:

1.  Create a custom domain. To create a custom domain, work with your Domain Name Service (DNS) provider to register your custom domain.
2.  Configure your domain to route incoming network traffic to the IBM Ingress controller. Choose between these options:
    -   Define an alias for your custom domain by specifying the IBM-provided domain as a Canonical Name record (CNAME). To find the IBM-provided Ingress domain, run `bx cs cluster-get <mycluster>` and look for the **Ingress subdomain** field.
    -   Map your custom domain to the portable public IP address of the IBM-provided Ingress controller by adding the IP address as a record. To find the portable public IP address of the Ingress controller:
        1.  Run `bx cs cluster-get <mycluster>` and look for the **Ingress subdomain** field.
        2.  Run `nslookup <Ingress subdomain>`.
3.  Create a TLS certificate and key for your domain that is encoded in PEM format.
4.  Store your TLS certificate and key in a Kubernetes secret.
    1.  Open your preferred editor and create a Kubernetes secret configuration file that is named, for example, `mysecret.yaml`.
    2.  Define a secret that uses your TLS certificate and key.

        ```
        apiVersion: v1
        kind: Secret
        metadata:
          name: <mytlssecret>
        type: Opaque
        data:
          tls.crt: <tlscert>
          tls.key: <tlskey>
        ```
        {: codeblock}

        <table>
        <thead>
        <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the YAML file components</th>
        </thead>
        <tbody>
        <tr>
        <td><code>name</code></td>
        <td>Replace <em>&lt;mytlssecret&gt;</em> with a name for your Kubernetes secret.</td>
        </tr>
        <tr>
        <td><code>tls.cert</code></td>
        <td>Replace <em>&lt;tlscert&gt;</em> with your custom TLS certificate that is encoded in base64 format.</td>
         </tr>
         <td><code>tls.key</code></td>
         <td>Replace <em>&lt;tlskey&gt;</em> with your custom TLS key that is encoded in base64 format.</td>
         </tbody></table>

    3.  Save your configuration file.
    4.  Create the TLS secret for your cluster.

        ```
        kubectl apply -f mysecret.yaml
        ```
        {: pre}

5.  [Deploy your app to the cluster](#cs_apps_cli). When you deploy your app to the cluster, one or more pods are created for you that run your app in a container. Enure that you add a label to your deployment in the metadata section of your configuration file. This label is needed to identify all pods where your app is running, so that they can be included in the Ingress load balancing.

6.  Create a Kubernetes service for the app to expose. The Ingress controller can include your app into the Ingress load balancing only if your app is exposed via a Kubernetes service inside the cluster.

    1.  Open your preferred editor and create a service configuration file that is named, for example, `myservice.yaml`.
    2.  Define a service for the app that you want to expose to the public.

        ```
        apiVersion: v1
        kind: Service
        metadata:
          name: <myservice>
        spec:
          selector:
            <selectorkey>: <selectorvalue>
          ports:
           - protocol: TCP
             port: 8080
        ```
       {: codeblock}

        <table>
        <thead>
        <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the YAML file components</th>
        </thead>
        <tbody>
        <tr>
        <td><code>name</code></td>
        <td>Replace <em>&lt;myservice1&gt;</em> with a name for your Kubernetes service.</td>
        </tr>
        <tr>
        <td><code>selector</code></td>
        <td>Enter the label key (<em>&lt;selectorkey&gt;</em>) and value (<em>&lt;selectorvalue&gt;</em>) pair that you want to use to target the pods where your app runs. For example, if you use the following selector <code>app: code</code>, all pods that have this label in their metadata are included in the load balancing. Enter the same label that you used when you deployed your app to the cluster. </td>
         </tr>
         <td><code>port</code></td>
         <td>The port that the service listens on.</td>
         </tbody></table>

    3.  Save your changes.
    4.  Create the service in your cluster.

        ```
        kubectl apply -f myservice.yaml
        ```
        {: pre}

    5.  Repeat these steps for every app that you want to expose to the public.
7.  Create an Ingress resource. Ingress resources define the routing rules for the Kubernetes service that you created for your app and are used by the Ingress controller to route incoming network traffic to the service. You can use one Ingress resource to define routing rules for multiple apps as long as every app is exposed via a Kubernetes service inside the cluster.
    1.  Open your preferred editor and create an Ingress configuration file that is named, for example, `myingress.yaml`.
    2.  Define an Ingress resource in your configuration file that uses your custom domain to route incoming network traffic to your services, and your custom certificate to manage the TLS termination. For every service you can define an individual path that is appended to your custom domain to create a unique path to your app, for example `https://mydomain/myapp`. When you enter this route into a web browser, network traffic is routed to the Ingress controller. The Ingress controller looks up the associated service and sends network traffic to the service, and further to the pods where the app is running.

        **Note:** It is important that the app listens on the path that you defined in the Ingress resource. Otherwise, network traffic cannot be forwarded to the app. Most apps do not listen on a specific path, but use the root path and a specific port. In this case, define the root path as `/` and do not specify an individual path for your app.

        ```
        apiVersion: extensions/v1beta1
        kind: Ingress
        metadata:
          name: <myingressname>
        spec:
          tls:
          - hosts:
            - <mycustomdomain>
            secretName: <mytlssecret>
          rules:
          - host: <mycustomdomain>
            http:
              paths:
              - path: /<myservicepath1>
                backend:
                  serviceName: <myservice1>
                  servicePort: 80
              - path: /<myservicepath2>
                backend:
                  serviceName: <myservice2>
                  servicePort: 80
        ```
        {: codeblock}

        <table>
        <thead>
        <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the YAML file components</th>
        </thead>
        <tbody>
        <tr>
        <td><code>name</code></td>
        <td>Replace <em>&lt;myingressname&gt;</em> with a name for your Ingress resource.</td>
        </tr>
        <tr>
        <td><code>tls/hosts</code></td>
        <td>Replace <em>&lt;mycustomdomain&gt;</em> with our custom domain that you want to configure for TLS termination.

        </br></br>
        <strong>Note:</strong> Do not use &ast; for your host or leave the host property empty to avoid failures during Ingress creation.</td>
        </tr>
        <tr>
        <td><code>tls/secretName</code></td>
        <td>Replace <em>&lt;mytlssecret&gt;</em> with the name of the secret that you created earlier and that holds your custom TLS certificate and key to manage the TLS termination for your custom domain.
        </tr>
        <tr>
        <td><code>host</code></td>
        <td>Replace <em>&lt;mycustomdomain&gt;</em> with our custom domain that you want to configure for TLS termination.

        </br></br>
        <strong>Note:</strong> Do not use &ast; for your host or leave the host property empty to avoid failures during Ingress creation.
        </td>
        </tr>
        <tr>
        <td><code>path</code></td>
        <td>Replace <em>&lt;myservicepath1&gt;</em> with a slash or the unique path that your app is listening on, so that network traffic can be forwarded to the app.

        </br>
        For every Kubernetes service, you can define an individual path that is appended to the IBM-provided domain to create a unique path to your app, for example <code>ingress_domain/myservicepath1</code>. When you enter this route into a web browser, network traffic is routed to the Ingress controller. The Ingress controller looks up the associated service, and sends network traffic to the service and to the pods where the app is running by using the same path. The app must be set up to listen on this path in order to receive incoming network traffic.

        </br>
        Many apps do not listen on a specific path, but use the root path and a specific port. In this case, define the root path as <code>/</code> and do not specify an individual path for your app.

        </br></br>
        Examples: <ul><li>For <code>https://mycustomdomain/</code>, enter <code>/</code> as the path.</li><li>For <code>https://mycustomdomain/myservicepath</code>, enter <code>/myservicepath</code> as the path.</li></ul>
        <strong>Tip:</strong> If you want to configure your Ingress to listen on a path that is different to the one that your app listens on, you can use the [rewrite annotation](cs_annotations.html#rewrite-path) to establish proper routing to your app.
        </td>
        </tr>
        <tr>
        <td><code>serviceName</code></td>
        <td>Replace <em>&lt;myservice1&gt;</em> with the name of the service that you used when you created the Kubernetes service for your app.</td>
        </tr>
        <tr>
        <td><code>servicePort</code></td>
        <td>The port that your service listens to. Use the same port that you defined when you created the Kubernetes service for your app.</td>
        </tr>
        </tbody></table>

    3.  Save your changes.
    4.  Create the Ingress resource for your cluster.

        ```
        kubectl apply -f myingress.yaml
        ```
        {: pre}

8.  Verify that the Ingress resource was created successfully. Replace _&lt;myingressname&gt;_ with the name of the Ingress resource that you created earlier.

    ```
    kubectl describe ingress <myingressname>
    ```
    {: pre}

    **Note:** It might take a few minutes for the Ingress resource to be created properly and for the app to be available on the public internet.

9.  Access your app from the internet.
    1.  Open your preferred web browser.
    2.  Enter the URL of the app service that you want to access.

        ```
        https://<mycustomdomain>/<myservicepath1>
        ```
        {: codeblock}


#### Configuring the Ingress controller to route network traffic to apps outside the cluster
{: #external_endpoint}

You can configure the Ingress controller for apps that are located outside of the cluster to be included into the cluster load balancing. Incoming requests on the IBM-provided or your custom domain are forwarded automatically to the external app.

Before you begin:

-   If you do not have one already, [create a standard cluster](cs_cluster.html#cs_cluster_ui).
-   [Target your CLI](cs_cli_install.html#cs_cli_configure) to your cluster to run `kubectl` commands.
-   Ensure that the external app that you want to include into the cluster load balancing can be accessed by using a public IP address.

You can configure the Ingress controller to route incoming network traffic on the IBM-provided domain to apps that are located outside your cluster. If you want to use a custom domain and TLS certificate instead, replace the IBM-provided domain and TLS certificate with your [custom domain and TLS certificate](#custom_domain_cert).

1.  Configure a Kubernetes endpoint that defines the external location of the app that you want to include into the cluster load balancing.
    1.  Open your preferred editor and create an endpoint configuration file that is named, for example, `myexternalendpoint.yaml`.
    2.  Define your external endpoint. Include all public IP addresses and ports that you can use to access your external app.

        ```
        kind: Endpoints
        apiVersion: v1
        metadata:
          name: <myendpointname>
        subsets:
          - addresses:
              - ip: <externalIP1>
              - ip: <externalIP2>
            ports:
              - port: <externalport>
        ```
        {: codeblock}

        <table>
        <thead>
        <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the YAML file components</th>
        </thead>
        <tbody>
        <tr>
        <td><code>name</code></td>
        <td>Replace <em>&lt;myendpointname&gt;</em> with the name for your Kubernetes endpoint.</td>
        </tr>
        <tr>
        <td><code>ip</code></td>
        <td>Replace <em>&lt;externalIP&gt;</em> with the public IP addresses to connect to your external app.</td>
         </tr>
         <td><code>port</code></td>
         <td>Replace <em>&lt;externalport&gt;</em> with the port that your external app listens to.</td>
         </tbody></table>

    3.  Save your changes.
    4.  Create the Kubernetes endpoint for your cluster.

        ```
        kubectl apply -f myexternalendpoint.yaml
        ```
        {: pre}

2.  Create a Kubernetes service for your cluster and configure it to forward incoming requests to the external endpoint that you created earlier.
    1.  Open your preferred editor and create a service configuration file that is named, for example, `myexternalservice.yaml`.
    2.  Define the service.

        ```
        apiVersion: v1
        kind: Service
        metadata:
          name: <myexternalservice>
          labels:
              name: <myendpointname>
        spec:
          ports:
           - protocol: TCP
             port: 8080
        ```
        {: codeblock}

        <table>
        <thead>
        <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the YAML file components</th>
        </thead>
        <tbody>
        <tr>
        <td><code>metadata/name</code></td>
        <td>Replace <em>&lt;myexternalservice&gt;</em> with the name for your Kubernetes service.</td>
        </tr>
        <tr>
        <td><code>labels/name</code></td>
        <td>Replace <em>&lt;myendpointname&gt;</em> with the name of the Kubernetes endpoint that you created earlier.</td>
        </tr>
        <tr>
        <td><code>port</code></td>
        <td>The port that the service listens on.</td>
        </tr></tbody></table>

    3.  Save your changes.
    4.  Create the Kubernetes service for your cluster.

        ```
        kubectl apply -f myexternalservice.yaml
        ```
        {: pre}

3.  View the IBM-provided domain and TLS certificate. Replace _&lt;mycluster&gt;_ with the name of the cluster where the app is deployed.

    ```
    bx cs cluster-get <mycluster>
    ```
    {: pre}

    Your CLI output looks similar to the following.

    ```
    Retrieving cluster <mycluster>...
    OK
    Name:    <mycluster>
    ID:    b9c6b00dc0aa487f97123440b4895f2d
    Created:  2017-04-26T19:47:08+0000
    State:    normal
    Master URL:  https://169.57.40.165:1931
    Ingress subdomain:  <ibmdomain>
    Ingress secret:  <ibmtlssecret>
    Workers:  3
    ```
    {: screen}

    You can see the IBM-provided domain in the **Ingress subdomain** and the IBM-provided certificate in the **Ingress secret** field.

4.  Create an Ingress resource. Ingress resources define the routing rules for the Kubernetes service that you created for your app and are used by the Ingress controller to route incoming network traffic to the service. You can use one Ingress resource to define routing rules for multiple external apps as long as every app is exposed with its external endpoint via a Kubernetes service inside the cluster.
    1.  Open your preferred editor and create an Ingress configuration file that is named, for example, `myexternalingress.yaml`.
    2.  Define an Ingress resource in your configuration file that uses the IBM-provided domain and TLS certificate to route incoming network traffic to your external app by using the external endpoint that you defined earlier. For every service you can define an individual path that is appended to the IBM-provided or custom domain to create a unique path to your app, for example `https://ingress_domain/myapp`. When you enter this route into a web browser, network traffic is routed to the Ingress controller. The Ingress controller looks up the associated service and sends network traffic to the service, and further to the external app.

        **Note:** It is important that the app listens on the path that you defined in the Ingress resource. Otherwise, network traffic cannot be forwarded to the app. Most apps do not listen on a specific path, but use the root path and a specific port. In this case, define the root path as / and do not specify an individual path for your app.

        ```
        apiVersion: extensions/v1beta1
        kind: Ingress
        metadata:
          name: <myingressname>
        spec:
          tls:
          - hosts:
            - <ibmdomain>
            secretName: <ibmtlssecret>
          rules:
          - host: <ibmdomain>
            http:
              paths:
              - path: /<myexternalservicepath1>
                backend:
                  serviceName: <myservice1>
                  servicePort: 80
              - path: /<myexternalservicepath2>
                backend:
                  serviceName: <myexternalservice2>
                  servicePort: 80
        ```
        {: codeblock}

        <table>
        <thead>
        <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the YAML file components</th>
        </thead>
        <tbody>
        <tr>
        <td><code>name</code></td>
        <td>Replace <em>&lt;myingressname&gt;</em> with the name for the Ingress resource.</td>
        </tr>
        <tr>
        <td><code>tls/hosts</code></td>
        <td>Replace <em>&lt;ibmdomain&gt;</em> with the IBM-provided <strong>Ingress subdomain</strong> name from the previous step. This domain is configured for TLS termination.

        </br></br>
        <strong>Note:</strong> Do not use &ast; for your host or leave the host property empty to avoid failures during Ingress creation.</td>
        </tr>
        <tr>
        <td><code>tls/secretName</code></td>
        <td>Replace <em>&lt;ibmtlssecret&gt;</em> with the IBM-provided <strong>Ingress secret</strong> from the previous step. This certificate manages the TLS termination.</td>
        </tr>
        <tr>
        <td><code>rules/host</code></td>
        <td>Replace <em>&lt;ibmdomain&gt;</em> with the IBM-provided <strong>Ingress subdomain</strong> name from the previous step. This domain is configured for TLS termination.

        </br></br>
        <strong>Note:</strong> Do not use &ast; for your host or leave the host property empty to avoid failures during Ingress creation.</td>
        </tr>
        <tr>
        <td><code>path</code></td>
        <td>Replace <em>&lt;myexternalservicepath&gt;</em> with a slash or the unique path that your external app is listening on, so that network traffic can be forwarded to the app.

        </br>
        For every Kubernetes service, you can define an individual path that is appended to your domain to create a unique path to your app, for example <code>https://ibmdomain/myservicepath1</code>. When you enter this route into a web browser, network traffic is routed to the Ingress controller. The Ingress controller looks up the associated service, and sends network traffic to the external app by using the same path. The app must be set up to listen on this path in order to receive incoming network traffic.

        </br></br>
        Many apps do not listen on a specific path, but use the root path and a specific port. In this case, define the root path as <code>/</code> and do not specify an individual path for your app.

        </br></br>
        <strong>Tip:</strong> If you want to configure your Ingress to listen on a path that is different to the one that your app listens on, you can use the [rewrite annotation](cs_annotations.html#rewrite-path) to establish proper routing to your app.</td>
        </tr>
        <tr>
        <td><code>serviceName</code></td>
        <td>Replace <em>&lt;myexternalservice&gt;</em> with the name of the service that you used when you created the Kubernetes service for your external app.</td>
        </tr>
        <tr>
        <td><code>servicePort</code></td>
        <td>The port that your service listens to.</td>
        </tr>
        </tbody></table>

    3.  Save your changes.
    4.  Create the Ingress resource for your cluster.

        ```
        kubectl apply -f myexternalingress.yaml
        ```
        {: pre}

5.  Verify that the Ingress resource was created successfully. Replace _&lt;myingressname&gt;_ with the name of the Ingress resource that you created earlier.

    ```
    kubectl describe ingress <myingressname>
    ```
    {: pre}

    **Note:** It might take a few minutes for the Ingress resource to be created properly and for the app to be available on the public internet.

6.  Access your external app.
    1.  Open your preferred web browser.
    2.  Enter the URL to access your external app.

        ```
        https://<ibmdomain>/<myexternalservicepath>
        ```
        {: codeblock}



#### Opening ports in the Ingress load balancer
{: #opening_ingress_ports}

By default, only ports 80 and 443 are exposed in the Ingress load balancer. To expose other ports, you can edit the ibm-cloud-provider-ingress-cm config map resource.

1.  Create a local version of the configuration file for the ibm-cloud-provider-ingress-cm config map resource. Add a <code>data</code> section and specify public ports 80, 443, and any other ports you want to add to the config map file separated by a semi-colon (;).

 Note: When specifying the ports, 80 and 443 must also be included to keep those ports open. Any port not specified is closed.

 ```
 apiVersion: v1
 data:
   public-ports: "80;443;<port3>"
 kind: ConfigMap
 metadata:
   name: ibm-cloud-provider-ingress-cm
   namespace: kube-system
 ```
 {: codeblock}

 Example:
 ```
 apiVersion: v1
 data:
   public-ports: "80;443;9443"
 kind: ConfigMap
 metadata:
   name: ibm-cloud-provider-ingress-cm
   namespace: kube-system
 ```

2. Apply the configuration file.

 ```
 kubectl apply -f <path/to/configmap.yaml>
 ```
 {: pre}

3. Verify that the configuration file was applied.

 ```
 kubectl describe cm ibm-cloud-provider-ingress-cm -n kube-system
 ```
 {: pre}

 Output:
 ```
 Name:        ibm-cloud-provider-ingress-cm
 Namespace:    kube-system
 Labels:        <none>
 Annotations:    <none>

 Data
 ====

  public-ports: "80;443;<port3>"
 ```
 {: codeblock}

For more information about config map resources, see the [Kubernetes documentation](https://kubernetes-v1-4.github.io/docs/user-guide/configmap/).



#### Configuring SSL protocols and SSL ciphers at the HTTP level
{: #ssl_protocols_ciphers}

Enable SSL protocols and ciphers at the global HTTP level by editing the `ibm-cloud-provider-ingress-cm` config map.

By default, the following values are used for ssl-protocols and ssl-ciphers:

```
ssl-protocols : "TLSv1 TLSv1.1 TLSv1.2"
ssl-ciphers : "HIGH:!aNULL:!MD5"
```
{: codeblock}

For more information about these parameters, see the NGINX documentation for [ssl-protocols ![External link icon](../icons/launch-glyph.svg "External link icon")](http://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_protocols) and [ssl-ciphers ![External link icon](../icons/launch-glyph.svg "External link icon")](http://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_ciphers).

To change the default values:
1. Create a local version of the configuration file for the ibm-cloud-provider-ingress-cm config map resource.

 ```
 apiVersion: v1
 data:
   ssl-protocols: "TLSv1 TLSv1.1 TLSv1.2"
   ssl-ciphers: "HIGH:!aNULL:!MD5"
 kind: ConfigMap
 metadata:
   name: ibm-cloud-provider-ingress-cm
   namespace: kube-system
 ```
 {: codeblock}

2. Apply the configuration file.

 ```
 kubectl apply -f <path/to/configmap.yaml>
 ```
 {: pre}

3. Verify that the configuration file is applied.

 ```
 kubectl describe cm ibm-cloud-provider-ingress-cm -n kube-system
 ```
 {: pre}

 Output:
 ```
 Name:        ibm-cloud-provider-ingress-cm
 Namespace:    kube-system
 Labels:        <none>
 Annotations:    <none>

 Data
 ====

  ssl-protocols: "TLSv1 TLSv1.1 TLSv1.2"
  ssl-ciphers: "HIGH:!aNULL:!MD5"
 ```
 {: screen}


## Managing IP addresses and subnets
{: #cs_cluster_ip_subnet}

You can use portable public and private subnets and IP addresses to expose apps in your cluster and make them accessible from the internet or on a private network.
{:shortdesc}

In {{site.data.keyword.containershort_notm}}, you can add stable, portable IPs for Kubernetes services by adding network subnets to the cluster. When you create a standard cluster, {{site.data.keyword.containershort_notm}} automatically provisions a portable public subnet, 5 portable public, and 5 portable private IP addresses. Portable IP addresses are static and do not change when a worker node, or even the cluster, is removed.

 Two of the portable IP addresses, one public and one private, is used for the [Ingress controller](#cs_apps_public_ingress) that you can use to expose multiple apps in your cluster by using a public route. 4 portable public and 4 private IP addresses can be used to expose apps by [creating a load balancer service](#cs_apps_public_load_balancer).

**Note:** Portable public IP addresses are charged on a monthly basis. If you choose to remove portable public IP addresses after your cluster is provisioned, you still have to pay the monthly charge, even if you used them only for a short amount of time.



1.  Create a Kubernetes service configuration file that is named `myservice.yaml` and define a service of type `LoadBalancer` with a dummy load balancer IP address. The following example uses the IP address 1.1.1.1 as the load balancer IP address.

    ```
    apiVersion: v1
    kind: Service
    metadata:
      labels:
        run: myservice
      name: myservice
      namespace: default
    spec:
      ports:
      - port: 80
        protocol: TCP
        targetPort: 80
      selector:
        run: myservice
      sessionAffinity: None
      type: LoadBalancer
      loadBalancerIP: 1.1.1.1
    ```
    {: codeblock}

2.  Create the service in your cluster.

    ```
    kubectl apply -f myservice.yaml
    ```
    {: pre}

3.  Inspect the service.

    ```
    kubectl describe service myservice
    ```
    {: pre}

    **Note:** The creation of this service fails because the Kubernetes master cannot find the specified load balancer IP address in the Kubernetes config map. When you run this command, you can see the error message and the list of available public IP addresses for the cluster.

    ```
    Error on cloud load balancer a8bfa26552e8511e7bee4324285f6a4a for service default/myservice with UID 8bfa2655-2e85-11e7-bee4-324285f6a4af: Requested cloud provider IP 1.1.1.1 is not available. The following cloud provider IPs are available: <list_of_IP_addresses>
    ```
    {: screen}




1.  Create a Kubernetes service configuration file that is named `myservice.yaml` and define a service of type `LoadBalancer` with a dummy load balancer IP address. The following example uses the IP address 1.1.1.1 as the load balancer IP address.

    ```
    apiVersion: v1
    kind: Service
    metadata:
      labels:
        run: myservice
      name: myservice
      namespace: default
    spec:
      ports:
      - port: 80
        protocol: TCP
        targetPort: 80
      selector:
        run: myservice
      sessionAffinity: None
      type: LoadBalancer
      loadBalancerIP: 1.1.1.1
    ```
    {: codeblock}

2.  Create the service in your cluster.

    ```
    kubectl apply -f myservice.yaml
    ```
    {: pre}

3.  Inspect the service.

    ```
    kubectl describe service myservice
    ```
    {: pre}

    **Note:** The creation of this service fails because the Kubernetes master cannot find the specified load balancer IP address in the Kubernetes config map. When you run this command, you can see the error message and the list of available public IP addresses for the cluster.

    ```
    Error on cloud load balancer a8bfa26552e8511e7bee4324285f6a4a for service default/myservice with UID 8bfa2655-2e85-11e7-bee4-324285f6a4af: Requested cloud provider IP 1.1.1.1 is not available. The following cloud provider IPs are available: <list_of_IP_addresses>
    ```
    {: screen}

</staging>

### Freeing up used IP addresses
{: #freeup_ip}

You can free up a used portable IP address by deleting the load balancer service that is using the portable IP address.

[Before you begin, set the context for the cluster you want to use.](cs_cli_install.html#cs_cli_configure)

1.  List available services in your cluster.

    ```
    kubectl get services
    ```
    {: pre}

2.  Remove the load balancer service that uses a public or private IP address.

    ```
    kubectl delete service <myservice>
    ```
    {: pre}

<br />


## Deploying apps with the GUI
{: #cs_apps_ui}

When you deploy an app to your cluster by using the Kubernetes dashboard, a deployment resource is automatically created that creates, updates, and manages the pods in your cluster.
{:shortdesc}

Before you begin:

-   Install the required [CLIs](cs_cli_install.html#cs_cli_install).
-   [Target your CLI](cs_cli_install.html#cs_cli_configure) to your cluster.

To deploy your app:

1.  [Open the Kubernetes dashboard](#cs_cli_dashboard).
2.  From the Kubernetes dashboard, click **+ Create**.
3.  Select **Specify app details below** to enter the app details on the GUI or **Upload a YAML or JSON file** to upload your app [configuration file ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/tasks/inject-data-application/define-environment-variable-container/). Use [this example YAML file ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/IBM-Bluemix/kube-samples/blob/master/deploy-apps-clusters/deploy-ibmliberty.yaml) to deploy a container from the **ibmliberty** image in the US-South region.
4.  In the Kubernetes dashboard, click **Deployments** to verify that the deployment was created.
5.  If you made your app publicly available by using a node port service, a load balancer service, or Ingress, verify that you can access the app.

<br />


## Deploying apps with the CLI
{: #cs_apps_cli}

After a cluster is created, you can deploy an app into that cluster by using the Kubernetes CLI.
{:shortdesc}

Before you begin:

-   Install the required [CLIs](cs_cli_install.html#cs_cli_install).
-   [Target your CLI](cs_cli_install.html#cs_cli_configure) to your cluster.

To deploy your app:

1.  Create a configuration file based on [Kubernetes best practices ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/concepts/configuration/overview/). Generally, a configuration file contains configuration details for each of the resources you are creating in Kubernetes. Your script might include one or more of the following sections:

    -   [Deployment ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/): Defines the creation of pods and replica sets. A pod includes an individual containerized app and replica sets control multiple instances of pods.

    -   [Service ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/concepts/services-networking/service/): Provides front-end access to pods by using a worker node or load balancer public IP address, or a public Ingress route.

    -   [Ingress ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/concepts/services-networking/ingress/): Specifies a type of load balancer that provides routes to access your app publicly.

2.  Run the configuration file in a cluster's context.

    ```
    kubectl apply -f deployment_script_location
    ```
    {: pre}

3.  If you made your app publicly available by using a node port service, a load balancer service, or Ingress, verify that you can access the app.

<br />





## Scaling apps
{: #cs_apps_scaling}

<!--Horizontal auto-scaling is not working at the moment due to a port issue with heapster. The dev team is working on a fix. We pulled out this content from the public docs. It is only visible in staging right now.-->

Deploy cloud applications that respond to changes in demand for your applications and that use resources only when needed. Autoscaling automatically increases or decreases the number of instances of your apps based on CPU.
{:shortdesc}

Before you begin, [target your CLI](cs_cli_install.html#cs_cli_configure) to your cluster.

**Note:** Are you looking for information about scaling Cloud Foundry applications? Check out [IBM Auto-Scaling for {{site.data.keyword.Bluemix_notm}}](/docs/services/Auto-Scaling/index.html).

With Kubernetes, you can enable [Horizontal Pod Autoscaling ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/) to scale your apps based on CPU.

1.  Deploy your app to your cluster from the CLI. When you deploy your app, you must request CPU.

    ```
    kubectl run <name> --image=<image> --requests=cpu=<cpu> --expose --port=<port_number>
    ```
    {: pre}

    <table>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding this command&apos;s components</th>
    </thead>
    <tbody>
    <tr>
    <td><code>--image</code></td>
    <td>The application that you want to deploy.</td>
    </tr>
    <tr>
    <td><code>--request=cpu</code></td>
    <td>The required CPU for the container, which is specified in milli-cores. As an example, <code>--requests=200m</code>.</td>
    </tr>
    <tr>
    <td><code>--expose</code></td>
    <td>When true, creates an external service.</td>
    </tr>
    <tr>
    <td><code>--port</code></td>
    <td>The port where your app is available externally.</td>
    </tr></tbody></table>

    **Note:** For more complex deployments, you might need to create a [configuration file](#cs_apps_cli).
2.  Create a Horizontal Pod Autoscaler and define your policy. For more information about working with the `kubetcl autoscale` command, see [the Kubernetes documentation ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/user-guide/kubectl/v1.5/#autoscale).

    ```
    kubectl autoscale deployment <deployment_name> --cpu-percent=<percentage> --min=<min_value> --max=<max_value>
    ```
    {: pre}

    <table>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding this command&apos;s components</th>
    </thead>
    <tbody>
    <tr>
    <td><code>--cpu-percent</code></td>
    <td>The average CPU utilization that is maintained by the Horizontal Pod Autoscaler, which is specified as a percentage.</td>
    </tr>
    <tr>
    <td><code>--min</code></td>
    <td>The minimum number of deployed pods that are used to maintain the specified CPU utilization percentage.</td>
    </tr>
    <tr>
    <td><code>--max</code></td>
    <td>The maximum number of deployed pods that are used to maintain the specified CPU utilization percentage.</td>
    </tr>
    </tbody></table>

<br />


## Managing rolling deployments
{: #cs_apps_rolling}

You can manage the rollout of your changes in an automated and controlled fashion. If your rollout isn't going according to plan, you can roll back your deployment to the previous revision.
{:shortdesc}

Before you begin, create a [deployment](#cs_apps_cli).

1.  [Roll out ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/user-guide/kubectl/v1.5/#rollout) a change. For example, you might want to change the image that you used in your initial deployment.

    1.  Get the deployment name.

        ```
        kubectl get deployments
        ```
        {: pre}

    2.  Get the pod name.

        ```
        kubectl get pods
        ```
        {: pre}

    3.  Get the name of the container that is running in the pod.

        ```
        kubectl describe pod <pod_name>
        ```
        {: pre}

    4.  Set the new image for the deployment to use.

        ```
        kubectl set image deployment/<deployment_name><container_name>=<image_name>
        ```
        {: pre}

    When you run the commands, the change is immediately applied and logged in the roll-out history.

2.  Check the status of your deployment.

    ```
    kubectl rollout status deployments/<deployment_name>
    ```
    {: pre}

3.  Roll back a change.
    1.  View the roll-out history for the deployment and identify the revision number of your last deployment.

        ```
        kubectl rollout history deployment/<deployment_name>
        ```
        {: pre}

        **Tip:** To see the details for a specific revision, include the revision number.

        ```
        kubectl rollout history deployment/<deployment_name> --revision=<number>
        ```
        {: pre}

    2.  Roll back to the previous version, or specify a revision. To roll back to the previous version, use the following command.

        ```
        kubectl rollout undo deployment/<depoyment_name> --to-revision=<number>
        ```
        {: pre}

<br />


## Adding {{site.data.keyword.Bluemix_notm}} services
{: #cs_apps_service}

Encrypted Kubernetes secrets are used to store {{site.data.keyword.Bluemix_notm}} service details and credentials and allow secure communication between the service and the cluster. As the cluster user, you can access this secret by mounting it as a volume to a pod.
{:shortdesc}

Before you begin, [target your CLI](cs_cli_install.html#cs_cli_configure) to your cluster. Make sure that the {{site.data.keyword.Bluemix_notm}} service that you want to use in your app was [added to the cluster](cs_cluster.html#cs_cluster_service) by the cluster admin.

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

4.  Create a YAML file to configure a pod that can access the service details through a secret volume.

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


## Creating persistent storage
{: #cs_apps_volume_claim}

Create a persistent volume claim (pvc) to provision NFS file storage for your cluster. Then, mount this claim to a pod to ensure that data is available even if the pod crashes or shuts down.
{:shortdesc}

The NFS file storage that backs the persistent volume is clustered by IBM in order to provide high availability for your data.


When an {{site.data.keyword.Bluemix_dedicated_notm}} account is [enabled for clusters](cs_ov.html#setup_dedicated), instead of using this task, you must [open a support ticket](/docs/support/index.html#contacting-support). By opening a ticket, you can request a backup for your volumes, a restoration from your volumes, and other storage functions.


1.  Review the available storage classes. {{site.data.keyword.containerlong}} provides eight pre-defined storage classes so that the cluster admin does not have to create any storage classes. The `ibmc-file-bronze` storage class is the same as the `default` storage class.

    ```
    kubectl get storageclasses
    ```
    {: pre}

    ```
    $ kubectl get storageclasses
    NAME                         TYPE
    default                      ibm.io/ibmc-file
    ibmc-file-bronze (default)   ibm.io/ibmc-file
    ibmc-file-custom             ibm.io/ibmc-file
    ibmc-file-gold               ibm.io/ibmc-file
    ibmc-file-retain-bronze      ibm.io/ibmc-file
    ibmc-file-retain-custom      ibm.io/ibmc-file
    ibmc-file-retain-gold        ibm.io/ibmc-file
    ibmc-file-retain-silver      ibm.io/ibmc-file
    ibmc-file-silver             ibm.io/ibmc-file
    ```
    {: screen}

2.  Decide if you want to save your data and the NFS file share after you delete the pvc. If you want to keep your data, then choose a `retain` storage class. If you want the data and your file share to be deleted when you delete the pvc, choose a storage class without `retain`.

3.  Review the IOPS of a storage class and the available storage sizes.
    - The bronze, silver, and gold storage classes use Endurance storage and have a single defined IOPS per GB for each class. The total IOPS depends on the size of the storage. For example, 1000Gi pvc at 4 IOPS per GB has a total of 4000 IOPS.

    ```
    kubectl describe storageclasses ibmc-file-silver
    ```
    {: pre}

    The **parameters** field provides the IOPS per GB associated with the storage class and the available sizes in gigabytes.

    ```
    Parameters:	iopsPerGB=4,sizeRange=20Gi,40Gi,80Gi,100Gi,250Gi,500Gi,1000Gi,2000Gi,4000Gi,8000Gi,12000Gi
    ```
    {: screen}

    - The custom storage classes use [Performance storage ![External link icon](../icons/launch-glyph.svg "External link icon")](https://knowledgelayer.softlayer.com/topic/performance-storage) and have discrete options for total IOPS and size.

    ```
    kubectl describe storageclasses ibmc-file-retain-custom
    ```
    {: pre}

    The **parameters** field provides the IOPS associated with the storage class and the available sizes in gigabytes. For example, a 40Gi pvc can select IOPS that is a multiple of 100 that is in the range of 100 - 2000 IOPS.

    ```
    Parameters:	Note=IOPS value must be a multiple of 100,reclaimPolicy=Retain,sizeIOPSRange=20Gi:[100-1000],40Gi:[100-2000],80Gi:[100-4000],100Gi:[100-6000],1000Gi[100-6000],2000Gi:[200-6000],4000Gi:[300-6000],8000Gi:[500-6000],12000Gi:[1000-6000]
    ```
    {: screen}

4.  Create a configuration file to define your persistent volume claim and save the configuration as a `.yaml` file.

    Example for bronze, silver, gold classes:

    ```
    apiVersion: v1
    kind: PersistentVolumeClaim
    metadata:
      name: <pvc_name>
      annotations:
        volume.beta.kubernetes.io/storage-class: "ibmc-file-silver"
    spec:
      accessModes:
        - ReadWriteMany
      resources:
        requests:
          storage: 20Gi
    ```
    {: codeblock}

    Example for custom classes:

    ```
    apiVersion: v1
    kind: PersistentVolumeClaim
    metadata:
      name: <pvc_name>
      annotations:
        volume.beta.kubernetes.io/storage-class: "ibmc-file-retain-custom"
    spec:
      accessModes:
        - ReadWriteMany
      resources:
        requests:
          storage: 40Gi
          iops: "500"
    ```
    {: codeblock}

    <table>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the YAML file components</th>
    </thead>
    <tbody>
    <tr>
    <td><code>metadata/name</code></td>
    <td>Enter the name of the persistent volume claim.</td>
    </tr>
    <tr>
    <td><code>metadata/annotations</code></td>
    <td>Specify the storage class for the persistent volume:
      <ul>
      <li>ibmc-file-bronze / ibmc-file-retain-bronze : 2 IOPS per GB.</li>
      <li>ibmc-file-silver / ibmc-file-retain-silver: 4 IOPS per GB.</li>
      <li>ibmc-file-gold / ibmc-file-retain-gold: 10 IOPS per GB.</li>
      <li>ibmc-file-custom / ibmc-file-retain-custom: Multiple values of IOPS available.

    </li> If you do not specify a storage class, the persistent volume is created with the bronze storage class.</td>
    </tr>
    <tr>
    <td><code>spec/accessModes</code>
    <code>resources/requests/storage</code></td>
    <td>If you choose a size other than one that is listed, the size is rounded up. If you select a size larger than the largest size, then the size is rounded down.</td>
    </tr>
    <tr>
    <td><code>spec/accessModes</code>
    <code>resources/requests/iops</code></td>
    <td>This option is for ibmc-file-custom / ibmc-file-retain-custom only. Specify the total IOPS for the storage. Run `kubectl describe storageclasses ibmc-file-custom` to see all options. If you choose an IOPS other than one that is listed, the IOPS is rounded up.</td>
    </tr>
    </tbody></table>

5.  Create the persistent volume claim.

    ```
    kubectl apply -f <local_file_path>
    ```
    {: pre}

6.  Verify that your persistent volume claim is created and bound to the persistent volume. This process can take a few minutes.

    ```
    kubectl describe pvc <pvc_name>
    ```
    {: pre}

    Your output looks similar to the following.

    ```
    Name:		<pvc_name>
    Namespace:	default
    StorageClass:	""
    Status:		Bound
    Volume:		pvc-0d787071-3a67-11e7-aafc-eef80dd2dea2
    Labels:		<none>
    Capacity:	20Gi
    Access Modes:	RWX
    Events:
      FirstSeen	LastSeen	Count	From								SubObjectPath	Type		Reason			Message
      ---------	--------	-----	----								-------------	--------	------			-------
      3m		3m		1	{ibm.io/ibmc-file 31898035-3011-11e7-a6a4-7a08779efd33 }			Normal		Provisioning		External provisioner is provisioning volume for claim "default/my-persistent-volume-claim"
      3m		1m		10	{persistentvolume-controller }							Normal		ExternalProvisioning	cannot find provisioner "ibm.io/ibmc-file", expecting that a volume for the claim is provisioned either manually or via external software
      1m		1m		1	{ibm.io/ibmc-file 31898035-3011-11e7-a6a4-7a08779efd33 }			Normal		ProvisioningSucceeded	Successfully provisioned volume pvc-0d787071-3a67-11e7-aafc-eef80dd2dea2

    ```
    {: screen}

6.  {: #cs_apps_volume_mount}To mount the persistent volume claim to your pod, create a configuration file. Save the configuration as a `.yaml` file.

    ```
    apiVersion: v1
    kind: Pod
    metadata:
     name: <pod_name>
    spec:
     containers:
     - image: nginx
       name: mycontainer
       volumeMounts:
       - mountPath: /volumemount
         name: myvol
     volumes:
     - name: myvol
       persistentVolumeClaim:
         claimName: mypvc
    ```
    {: codeblock}

    <table>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the YAML file components</th>
    </thead>
    <tbody>
    <tr>
    <td><code>metadata/name</code></td>
    <td>The name of the pod.</td>
    </tr>
    <tr>
    <td><code>volumeMounts/mountPath</code></td>
    <td>The absolute path of the directory to where the volume is mounted inside the container.</td>
    </tr>
    <tr>
    <td><code>volumeMounts/name</code></td>
    <td>The name of the volume that you mount to your container.</td>
    </tr>
    <tr>
    <td><code>volumes/name</code></td>
    <td>The name of the volume that you mount to your container. Typically this name is the same as <code>volumeMounts/name</code>.</td>
    </tr>
    <tr>
    <td><code>volumes/name/persistentVolumeClaim</code></td>
    <td>The name of the persistent volume claim that you want to use as your volume. When you mount the volume to the pod, Kubernetes identifies the persistent volume that is bound to the persistent volume claim and enables the user to read from and write to the persistent volume.</td>
    </tr>
    </tbody></table>

8.  Create the pod and mount the persistent volume claim to your pod.

    ```
    kubectl apply -f <local_yaml_path>
    ```
    {: pre}

9.  Verify that the volume is successfully mounted to your pod.

    ```
    kubectl describe pod <pod_name>
    ```
    {: pre}

    The mount point is in the **Volume Mounts** field and the volume is in the **Volumes** field.

    ```
     Volume Mounts:
          /var/run/secrets/kubernetes.io/serviceaccount from default-token-tqp61 (ro)
          /volumemount from myvol (rw)
    ...
    Volumes:
      myvol:
        Type:	PersistentVolumeClaim (a reference to a PersistentVolumeClaim in the same namespace)
        ClaimName:	mypvc
        ReadOnly:	false

    ```
    {: screen}

<br />


## Adding non-root user access to persistent storage
{: #cs_apps_volumes_nonroot}

Non-root users do not have write permission on the volume mount path for NFS-backed storage. To grant write permission, you must edit the Dockerfile of the image to create a directory on the mount path with the correct permission.
{:shortdesc}

Before you begin, [target your CLI](cs_cli_install.html#cs_cli_configure) to your cluster.

If you are designing an app with a non-root user that requires write permission to the volume, you must add the following processes to your Dockerfile and entrypoint script:

-   Create a non-root user.
-   Temporarily add the user to the root group.
-   Create a directory in the volume mount path with the correct user permissions.

For {{site.data.keyword.containershort_notm}}, the default owner of the volume mount path is the owner `nobody`. With NFS storage, if the owner does not exist locally in the pod, then the `nobody` user is created. The volumes are set up to recognize the root user in the container, which for some apps, is the only user inside a container. However, many apps specify a non-root user other than `nobody` that writes to the container mount path.

1.  Create a Dockerfile in a local directory. This example Dockerfile is creating a non-root user named `myguest`.

    ```
    FROM registry.<region>.bluemix.net/ibmnode:latest

    # Create group and user with GID & UID 1010.
    # In this case your are creating a group and user named myguest.
    # The GUID and UID 1010 is unlikely to create a conflict with any existing user GUIDs or UIDs in the image.
    # The GUID and UID must be between 0 and 65536. Otherwise, container creation fails.
    RUN groupadd --gid 1010 myguest
    RUN useradd --uid 1010 --gid 1010 -m --shell /bin/bash myguest

    ENV MY_USER=myguest

    COPY entrypoint.sh /sbin/entrypoint.sh
    RUN chmod 755 /sbin/entrypoint.sh

    EXPOSE 22
    ENTRYPOINT ["/sbin/entrypoint.sh"]
    ```
    {: codeblock}

2.  Create the entrypoint script in the same local folder as the Dockerfile. This example entrypoint script is specifying `/mnt/myvol` as the volume mount path.

    ```
    #!/bin/bash
    set -e

    # This is the mount point for the shared volume.
    # By default the mount point is owned by the root user.
    MOUNTPATH="/mnt/myvol"
    MY_USER=${MY_USER:-"myguest"}

    # This function creates a subdirectory that is owned by
    # the non-root user under the shared volume mount path.
    create_data_dir() {
      #Add the non-root user to primary group of root user.
      usermod -aG root $MY_USER

      # Provide read-write-execute permission to the group for the shared volume mount path.
      chmod 775 $MOUNTPATH

      # Create a directory under the shared path owned by non-root user myguest.
      su -c "mkdir -p ${MOUNTPATH}/mydata" -l $MY_USER
      su -c "chmod 700 ${MOUNTPATH}/mydata" -l $MY_USER
      ls -al ${MOUNTPATH}

      # For security, remove the non-root user from root user group.
      deluser $MY_USER root

      # Change the shared volume mount path back to its original read-write-execute permission.
      chmod 755 $MOUNTPATH
      echo "Created Data directory..."
    }

    create_data_dir

    # This command creates a long-running process for the purpose of this example.
    tail -F /dev/null
    ```
    {: codeblock}

3.  Log in to {{site.data.keyword.registryshort_notm}}.

    ```
    bx cr login
    ```
    {: pre}

4.  Build the image locally. Remember to replace _&lt;my_namespace&gt;_ with the namespace to your private images registry. Run `bx cr namespace-get` if you need to find your namespace.

    ```
    docker build -t registry.<region>.bluemix.net/<my_namespace>/nonroot .
    ```
    {: pre}

5.  Push the image to your namespace in {{site.data.keyword.registryshort_notm}}.

    ```
    docker push registry.<region>.bluemix.net/<my_namespace>/nonroot
    ```
    {: pre}

6.  Create a persistent volume claim by creating a configuration `.yaml` file. This example uses a lower performance storage class. Run `kubectl get storageclasses` to see available storage classes.

    ```
    apiVersion: v1
    kind: PersistentVolumeClaim
    metadata:
      name: mypvc
      annotations:
        volume.beta.kubernetes.io/storage-class: "ibmc-file-bronze"
    spec:
      accessModes:
        - ReadWriteMany
      resources:
        requests:
          storage: 20Gi
    ```
    {: codeblock}

7.  Create the persistent volume claim.

    ```
    kubectl apply -f <local_file_path>
    ```
    {: pre}

8.  Create a configuration file to mount the volume and run the pod from the nonroot image. The volume mount path `/mnt/myvol` matches the mount path that is specified in the Dockerfile. Save the configuration as a `.yaml` file.

    ```
    apiVersion: v1
    kind: Pod
    metadata:
     name: mypod
    spec:
     containers:
     - image: registry.<region>.bluemix.net/<my_namespace>/nonroot
       name: mycontainer
       volumeMounts:
       - mountPath: /mnt/myvol
         name: myvol
     volumes:
     - name: myvol
       persistentVolumeClaim:
         claimName: mypvc
    ```
    {: codeblock}

9.  Create the pod and mount the persistent volume claim to your pod.

    ```
    kubectl apply -f <local_yaml_path>
    ```
    {: pre}

10. Verify that the volume is successfully mounted to your pod.

    ```
    kubectl describe pod mypod
    ```
    {: pre}

    The mount point is listed in the **Volume Mounts** field and the volume is listed in the **Volumes** field.

    ```
     Volume Mounts:
          /var/run/secrets/kubernetes.io/serviceaccount from default-token-tqp61 (ro)
          /mnt/myvol from myvol (rw)
    ...
    Volumes:
      myvol:
        Type:	PersistentVolumeClaim (a reference to a PersistentVolumeClaim in the same namespace)
        ClaimName:	mypvc
        ReadOnly:	false

    ```
    {: screen}

11. Log in to the pod after the pod is running.

    ```
    kubectl exec -it mypod /bin/bash
    ```
    {: pre}

12. View permissions of your volume mount path.

    ```
    ls -al /mnt/myvol/
    ```
    {: pre}

    ```
    root@instance-006ff76b:/# ls -al /mnt/myvol/
    total 12
    drwxr-xr-x 3 root    root    4096 Jul 13 19:03 .
    drwxr-xr-x 3 root    root    4096 Jul 13 19:03 ..
    drwx------ 2 myguest myguest 4096 Jul 13 19:03 mydata
    ```
    {: screen}

    This output shows that root has read, write, and execute permissions on the volume mount path `mnt/myvol/`, but the non-root myguest user has permission to read and write to the `mnt/myvol/mydata` folder. Because of these updated permissions, the non-root user can now write data to the persistent volume.


