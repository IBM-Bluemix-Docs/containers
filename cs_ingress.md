---

copyright:
  years: 2014, 2018
lastupdated: "2018-4-25"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}




# Exposing apps with Ingress
{: #ingress}

Expose multiple apps in your Kubernetes cluster by creating Ingress resources that are managed by the IBM-provided application load balancer in {{site.data.keyword.containerlong}}.
{:shortdesc}

## Managing network traffic by using Ingress
{: #planning}

Ingress is a Kubernetes service that balances network traffic workloads in your cluster by forwarding public or private requests to your apps. You can use Ingress to expose multiple app services to the public or to a private network by using a unique public or private route.
{:shortdesc}



Ingress consists of two components:
<dl>
<dt>Application load balancer</dt>
<dd>The application load balancer (ALB) is an external load balancer that listens for incoming HTTP, HTTPS, TCP, or UDP service requests and forwards requests to the appropriate app pod. When you create a standard cluster, {{site.data.keyword.containershort_notm}} automatically creates a highly available ALB for your cluster and assigns a unique public route to it. The public route is linked to a portable public IP address that is provisioned into your IBM Cloud infrastructure (SoftLayer) account during cluster creation. A default private ALB is also automatically created, but is not automatically enabled.</dd>
<dt>Ingress resource</dt>
<dd>To expose an app by using Ingress, you must create a Kubernetes service for your app and register this service with the ALB by defining an Ingress resource. The Ingress resource is a Kubernetes resource that defines the rules for how to route incoming requests for an app. The Ingress resource also specifies the path to your app service, which is appended to the public route to form a unique app URL such as `mycluster.us-south.containers.mybluemix.net/myapp`.</dd>
</dl>

The following diagram shows how Ingress directs communication from the internet to an app:

<img src="images/cs_ingress.png" width="550" alt="Expose an app in {{site.data.keyword.containershort_notm}} by using Ingress" style="width:550px; border-style: none"/>

1. A user sends a request to your app by accessing your app's URL. This URL is the public URL for your exposed app with the Ingress resource path appended to it, such as `mycluster.us-south.containers.mybluemix.net/myapp`.

2. A DNS system service that acts as the global load balancer resolves the URL to the portable public IP address of the default public ALB in the cluster, and the request is routed to the Kubernetes ALB service for the app.

3. The Kubernetes service routes the request to the ALB.

4. The ALB checks if a routing rule for the `myapp` path in the cluster exists. If a matching rule is found, the request is forwarded according to the rules that you defined in the Ingress resource to the pod where the app is deployed. If multiple app instances are deployed in the cluster, the ALB load balances the requests between the app pods.



## Planning your Ingress configuration

### Prerequisites
{: #config_prereqs}

Before getting started with Ingress, review the following prerequisites.
{:shortdesc}

**Prerequisites for all Ingress configurations:**
- Ingress is available for standard clusters only and requires at least two worker nodes in the cluster to ensure high availability and that periodic updates are applied.
- Setting up Ingress requires an [Administrator access policy](cs_users.html#access_policies). Verify your current [access policy](cs_users.html#infra_access).



### Exposing apps in the same or in different namespaces
{: #multiple_namespaces}

The number of Ingress resources that you must create depends on whether you want to expose apps in a single namespace or in multiple namespaces within a cluster.
{:shortdesc}

If your apps are all exposed by Kubernetes services in the same cluster namespace, only one Ingress resource is required to define routing rules for the apps.

If your apps are exposed by Kubernetes services in different namespaces in one cluster, you must create one resource per namespace to define rules for the apps that are exposed there. To register multiple Ingress resources with the Ingress ALB, you must use a wildcard domain. When a wildcard domain such as `*.mycluster.us-south.containers.mybluemix.net` is registered, you can specify a different subdomain in each Ingress resource because multiple subdomains all resolve to the same host.

For example, consider the following scenario:
* You have two versions of the same app, `dev` and `stage`, for testing purposes.
* You deploy the apps in two different namespaces within the same cluster: `dev` into the development namespace, and `stage` into the staging namespace.
* Both apps listen on a path called `/myservice`, but you want to make sure that these apps are accessible by two different URLs.
To use the same ALB to manage traffic to these apps:
1. You use a Kubernetes service to expose each app in the respective namespaces.
2. You create an Ingress resource in the development namespace that specifies the host as `dev.mycluster.us-south.containers.mybluemix.net` and the path as `/myservice`.
3. You create an Ingress resource in the staging namespace that specifies the host as `stage.mycluster.us-south.containers.mybluemix.net` and the path as `/myservice`.
Now, both URLs resolve to the same domain and are thus both serviced by the same ALB. However, because the resource in the staging namespace is registered with the `stage` subdomain, the Ingress ALB correctly routes requests from the `stage.mycluster.us-south.containers.mybluemix.net/myservice` URL to only the `stage` version of the app.

### Choosing an Ingress configuration
{: #choose_config}

To choose the best configuration for Ingress, you can follow this decision tree:
{:shortdesc}

<img usemap="#ingress_map" border="0" class="image" src="images/networkingdt-ingress.png" width="750px" alt="This image walks you through choosing the best configuration for your Ingress application load balancer. If this image is not displaying, the information can still be found in the documentation." style="width:750px;" />
<map name="ingress_map" id="ingress_map">
<area href="/docs/containers/cs_ingress.html#private_ingress_no_tls" alt="Privately expose apps using a custom domain without TLS" shape="rect" coords="25, 246, 187, 294"/>
<area href="/docs/containers/cs_ingress.html#private_ingress_tls" alt="Privately expose apps using a custom domain with TLS" shape="rect" coords="161, 337, 309, 385"/>
<area href="/docs/containers/cs_ingress.html#external_endpoint" alt="Publicly exposing apps that are outside your cluster using the IBM-provided or a custom domain with TLS" shape="rect" coords="313, 229, 466, 282"/>
<area href="/docs/containers/cs_ingress.html#custom_domain_cert" alt="Publicly exposing apps using a custom domain with TLS" shape="rect" coords="365, 415, 518, 468"/>
<area href="/docs/containers/cs_ingress.html#ibm_domain" alt="Publicly exposing apps using the IBM-provided domain without TLS" shape="rect" coords="414, 629, 569, 679"/>
<area href="/docs/containers/cs_ingress.html#ibm_domain_cert" alt="Publicly exposing apps using the IBM-provided domain with TLS" shape="rect" coords="563, 711, 716, 764"/>
</map>

</staging>

<br />


## Exposing apps to the public
{: #ingress_expose_public}

When you create a standard cluster, an IBM-provided application load balancer (ALB) is automatically enabled and is assigned a portable public IP address and a public route.
{:shortdesc}

Every app that is exposed to the public via Ingress is assigned a unique path that is appended to the public route, so that you can use a unique URL to access an app publicly in your cluster. To expose your app to the public, you can configure Ingress for the following scenarios.

-   [Publicly expose apps using the IBM-provided domain without TLS](#ibm_domain)
-   [Publicly expose apps using the IBM-provided domain with TLS](#ibm_domain_cert)
-   [Publicly expose apps using a custom domain with TLS](#custom_domain_cert)
-   [Publicly expose apps that are outside your cluster using the IBM-provided or a custom domain with TLS](#external_endpoint)

### Publicly expose apps using the IBM-provided domain without TLS
{: #ibm_domain}

You can configure the ALB to load balance incoming HTTP network traffic to the apps in your cluster and use the IBM-provided domain to access your apps from the internet.
{:shortdesc}

Before you begin:

-   If you do not have one already, [create a standard cluster](cs_clusters.html#clusters_ui).
-   [Target your CLI](cs_cli_install.html#cs_cli_configure) to your cluster to run `kubectl` commands.

To expose an app by using the IBM-provided domain:

1.  [Deploy your app to the cluster](cs_app.html#app_cli). Ensure that you add a label to your deployment in the metadata section of your configuration file, such as `app: code`. This label is needed to identify all pods where your app is running so that the pods can be included in the Ingress load balancing.

2.   Create a Kubernetes service for each app that you want to expose. Your app must be exposed by a Kubernetes service to be included by the cluster ALB in the Ingress load balancing.
      1.  Open your preferred editor and create a service configuration file that is named, for example, `myalbservice.yaml`.
      2.  Define a service for the app that the ALB will expose to the public.

          ```
          apiVersion: v1
          kind: Service
          metadata:
            name: myalbservice
          spec:
            selector:
              <selector_key>: <selector_value>
            ports:
             - protocol: TCP
               port: 8080
          ```
          {: codeblock}

          <table>
          <thead>
          <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the ALB service YAML file components</th>
          </thead>
          <tbody>
          <tr>
          <td><code>selector</code></td>
          <td>Enter the label key (<em>&lt;selector_key&gt;</em>) and value (<em>&lt;selector_value&gt;</em>) pair that you want to use to target the pods where your app runs. To target your pods and include them in the service load balancing, make sure that the <em>&lt;selector_key&gt;</em> and <em>&lt;selector_value&gt;</em> are the same as the key/value pair that you used in the <code>spec.template.metadata.labels</code> section of your deployment yaml.</td>
           </tr>
           <tr>
           <td><code>port</code></td>
           <td>The port that the service listens on.</td>
           </tr>
           </tbody></table>
      3.  Save your changes.
      4.  Create the service in your cluster. If you have apps deployed in multiple namespaces in your cluster, ensure the service deploys into the same namespace as the app that you want to expose.

          ```
          kubectl apply -f myalbservice.yaml [-n <namespace>]
          ```
          {: pre}
      5.  Repeat these steps for every app that you want to expose to the public.

3. Get the details for your cluster to view the IBM-provided domain. Replace _&lt;cluster_name_or_ID&gt;_ with the name of the cluster where the app is deployed that you want to expose to the public.

    ```
    bx cs cluster-get <cluster_name_or_ID>
    ```
    {: pre}

    Example output:

    ```
    Name:                   mycluster
    ID:                     18a61a63c6a94b658596ca93d087aad9
    State:                  normal
    Created:                2018-01-12T18:33:35+0000
    Location:               dal10
    Master URL:             https://169.xx.xxx.xxx:26268
    Ingress Subdomain:      mycluster-12345.us-south.containers.mybluemix.net
    Ingress Secret:         <tls_secret>
    Workers:                3
    Version:                1.8.11
    Owner Email:            owner@email.com
    Monitoring Dashboard:   <dashboard_URL>
    ```
    {: screen}

    You can see the IBM-provided domain in the **Ingress subdomain** field.
4.  Create an Ingress resource. Ingress resources define the routing rules that the ALB uses to route traffic to your app service.
    * If your apps are all exposed by services in the same namespace, only one Ingress resource is required.
    * If your apps are exposed by services in different namespaces in one cluster, you must create one resource per namespace to define rules for the apps that are exposed there.
    1.  Open your preferred editor and create an Ingress configuration file that is named, for example, `myingressresource.yaml`.
    2.  Define an Ingress resource in your configuration file that uses the IBM-provided domain to route incoming network traffic to the services that you created earlier.

        ```
        apiVersion: extensions/v1beta1
        kind: Ingress
        metadata:
          name: myingressresource
        spec:
          rules:
          - host: <ibm_domain>
            http:
              paths:
              - path: /<service1_path>
                backend:
                  serviceName: <service1>
                  servicePort: 80
              - path: /<service2_path>
                backend:
                  serviceName: <service2>
                  servicePort: 80
        ```
        {: codeblock}

        <table>
        <thead>
        <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the YAML file components</th>
        </thead>
        <tbody>
        <tr>
        <td><code>host</code></td>
        <td>Replace <em>&lt;ibm_domain&gt;</em> with the IBM-provided <strong>Ingress subdomain</strong> name from the previous step. If your apps are exposed by services in different namespaces in one cluster, append a wildcard subdomain to the beginning of the subdomain, such as `subdomain1.mycluster.us-south.containers.mybluemix.net`. Use a unique subdomain for each resource that you create in the cluster.

        </br></br>
        <strong>Note:</strong> Do not use * for your host or leave the host property empty to avoid failures during Ingress creation.</td>
        </tr>
        <tr>
        <td><code>path</code></td>
        <td>Replace <em>&lt;service1_path&gt;</em> with a slash or the unique path that your app is listening on so that network traffic can be forwarded to the app.

        </br>
        For every Kubernetes service, you can define an individual path that is appended to the IBM-provided domain to create a unique path to your app; for example <code>ibm_domain/service1_path</code>. When you enter this route into a web browser, network traffic is routed to the ALB. The ALB looks up the associated service sends network traffic to the service. The service then forwards the traffic to the pods where the app is running. The app must be set up to listen on this path to receive incoming network traffic.

        </br></br>
        Many apps do not listen on a specific path, but use the root path and a specific port. In this case, define the root path as <code>/</code> and do not specify an individual path for your app.
        </br>
        Examples: <ul><li>For <code>http://ibm_domain/</code>, enter <code>/</code> as the path.</li><li>For <code>http://ibm_domain/service1_path</code>, enter <code>/service1_path</code> as the path.</li></ul>
        </br>
        <strong>Tip:</strong> To configure Ingress to listen on a path that is different than the path that your app listens on, you can use the [rewrite annotation](cs_annotations.html#rewrite-path) to establish proper routing to your app.</td>
        </tr>
        <tr>
        <td><code>serviceName</code></td>
        <td>Replace <em>&lt;service1&gt;</em> and <em>&lt;service2&gt;</em>, etc. with the name of the services you created to expose your app. If your apps are exposed by services in different namespaces in one cluster, include only app services that are in the same namespace.</td>
        </tr>
        <tr>
        <td><code>servicePort</code></td>
        <td>The port that your service listens to. Use the same port that you defined when you created the Kubernetes service for your app.</td>
        </tr>
        </tbody></table>

    3.  Create the Ingress resource for your cluster. If your apps are exposed by services in different namespaces in one cluster, ensure the resource deploys into the same namespace as the app services that you specified in the resource.

        ```
        kubectl apply -f myingressresource.yaml [-n <namespace>]
        ```
        {: pre}
    4.   Verify that the Ingress resource was created successfully.

      ```
      kubectl describe ingress myingressresource
      ```
      {: pre}

      1. If messages in the events describe an error in your resource configuration, change the values in your resource file and reapply the file for the resource.

    5. If your apps are exposed by services in different namespaces in one cluster, repeat these steps to create one resource per namespace to define rules for the apps that are exposed there. Use a unique subdomain for each resource that you create in the cluster.
6.   In a web browser, enter the URL of the app service to access.

      ```
      https://<ibm_domain>/<service1_path>
      ```
      {: codeblock}


<br />


### Publicly expose apps using the IBM-provided domain with TLS
{: #ibm_domain_cert}

You can configure the Ingress ALB to manage incoming TLS connections for your apps, decrypt the network traffic by using the IBM-provided TLS certificate, and forward the decrypted request to the apps that are exposed in your cluster.
{:shortdesc}

Before you begin:

-   If you do not have one already, [create a standard cluster](cs_clusters.html#clusters_ui).
-   [Target your CLI](cs_cli_install.html#cs_cli_configure) to your cluster to run `kubectl` commands.

To expose an app by using the IBM-provided domain with TLS:

1.  [Deploy your app to the cluster](cs_app.html#app_cli). Ensure that you add a label to your deployment in the metadata section of your configuration file, such as `app: code`. This label is needed to identify all pods where your app is running so that the pods can be included in the Ingress load balancing.

2.   Create a Kubernetes service for each app that you want to expose. Your app must be exposed by a Kubernetes service to be included by the cluster ALB in the Ingress load balancing.
      1.  Open your preferred editor and create a service configuration file that is named, for example, `myalbservice.yaml`.
      2.  Define a service for the app that the ALB will expose to the public.

          ```
          apiVersion: v1
          kind: Service
          metadata:
            name: myalbservice
          spec:
            selector:
              <selector_key>: <selector_value>
            ports:
             - protocol: TCP
               port: 8080
          ```
          {: codeblock}

          <table>
          <thead>
          <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the ALB service YAML file components</th>
          </thead>
          <tbody>
          <tr>
          <td><code>selector</code></td>
          <td>Enter the label key (<em>&lt;selector_key&gt;</em>) and value (<em>&lt;selector_value&gt;</em>) pair that you want to use to target the pods where your app runs. To target your pods and include them in the service load balancing, make sure that the <em>&lt;selector_key&gt;</em> and <em>&lt;selector_value&gt;</em> are the same as the key/value pair that you used in the <code>spec.template.metadata.labels</code> section of your deployment yaml.</td>
           </tr>
           <tr>
           <td><code>port</code></td>
           <td>The port that the service listens on.</td>
           </tr>
           </tbody></table>
      3.  Save your changes.
      4.  Create the service in your cluster. If you have apps deployed in multiple namespaces in your cluster, ensure the service deploys into the same namespace as the app that you want to expose.

          ```
          kubectl apply -f myalbservice.yaml [-n <namespace>]
          ```
          {: pre}
      5.  Repeat these steps for every app that you want to expose to the public.

3.   View the IBM-provided domain and TLS certificate. Replace _&lt;cluster_name_or_ID&gt;_ with the name of the cluster where the app is deployed.

      ```
      bx cs cluster-get <cluster_name_or_ID>
      ```
      {: pre}

      Example output:

      ```
      Name:                   mycluster
      ID:                     18a61a63c6a94b658596ca93d087aad9
      State:                  normal
      Created:                2018-01-12T18:33:35+0000
      Location:               dal10
      Master URL:             https://169.xx.xxx.xxx:26268
      Ingress Subdomain:      mycluster-12345.us-south.containers.mybluemix.net
      Ingress Secret:         <ibm_tls_secret>
      Workers:                3
      Version:                1.8.11
      Owner Email:            owner@email.com
      Monitoring Dashboard:   <dashboard_URL>
      ```
      {: screen}

      You can see the IBM-provided domain in the **Ingress subdomain** and the IBM-provided certificate in the **Ingress secret** fields.

4.  Create an Ingress resource. Ingress resources define the routing rules for the Kubernetes service that you created for your app and are used by the ALB to route incoming network traffic to the service. You must use one Ingress resource to define routing rules for multiple apps if every app is exposed via a Kubernetes service inside the cluster.
    1.  Open your preferred editor and create an Ingress configuration file that is named, for example, `myingressresource.yaml`.
    2.  Define an Ingress resource in your configuration file that uses the IBM-provided domain to route incoming network traffic to the services that you created earlier, and your custom certificate to manage the TLS termination.

        ```
        apiVersion: extensions/v1beta1
        kind: Ingress
        metadata:
          name: myingressresource
        spec:
          tls:
          - hosts:
            - <ibm_domain>
            secretName: <ibm_tls_secret>
          rules:
          - host: <ibm_domain>
            http:
              paths:
              - path: /<service1_path>
                backend:
                  serviceName: <service1>
                  servicePort: 80
              - path: /<service2_path>
                backend:
                  serviceName: <service2>
                  servicePort: 80
        ```
        {: codeblock}

        <table>
        <thead>
        <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the YAML file components</th>
        </thead>
        <tbody>
        <tr>
        <td><code>tls/hosts</code></td>
        <td>Replace <em>&lt;ibm_domain&gt;</em> with the IBM-provided <strong>Ingress subdomain</strong> name from the previous step. This domain is configured for TLS termination.

        </br></br>
        <strong>Note:</strong> Do not use &ast; for your host or leave the host property empty to avoid failures during Ingress creation.</td>
        </tr>
        <tr>
        <td><code>tls/secretName</code></td>
        <td>Replace <em>&lt;ibm_tls_secret&gt;</em> with the IBM-provided <strong>Ingress secret</strong> name from the previous step. This certificate manages the TLS termination.
        </tr>
        <tr>
        <td><code>host</code></td>
        <td>Replace <em>&lt;ibm_domain&gt;</em> with the IBM-provided <strong>Ingress subdomain</strong> name from the previous step. This domain is configured for TLS termination.

        </br></br>
        <strong>Note:</strong> Do not use &ast; for your host or leave the host property empty to avoid failures during Ingress creation.</td>
        </tr>
        <tr>
        <td><code>path</code></td>
        <td>Replace <em>&lt;service1_path&gt;</em> with a slash or the unique path that your app is listening on so that network traffic can be forwarded to the app.

        </br>
        For every Kubernetes service, you can define an individual path that is appended to the IBM-provided domain to create a unique path to your app. When you enter this route into a web browser, network traffic is routed to the ALB. The ALB looks up the associated service and sends network traffic to the service. The service then forwards the traffic to the pods where the app is running. The app must be set up to listen on this path to receive incoming network traffic.

        </br>
        Many apps do not listen on a specific path, but use the root path and a specific port. In this case, define the root path as <code>/</code> and do not specify an individual path for your app.

        </br>
        Examples: <ul><li>For <code>http://ibm_domain/</code>, enter <code>/</code> as the path.</li><li>For <code>http://ibm_domain/service1_path</code>, enter <code>/service1_path</code> as the path.</li></ul>
        <strong>Tip:</strong> To configure Ingress to listen on a path that is different than the path that your app listens on, you can use the [rewrite annotation](cs_annotations.html#rewrite-path) to establish proper routing to your app.</td>
        </tr>
        <tr>
        <td><code>serviceName</code></td>
        <td>Replace <em>&lt;service1&gt;</em> with the name of the service that you used when you created the Kubernetes service for your app.</td>
        </tr>
        <tr>
        <td><code>servicePort</code></td>
        <td>The port that your service listens to. Use the same port that you defined when you created the Kubernetes service for your app.</td>
        </tr>
        </tbody></table>

    3.  Create the Ingress resource for your cluster.

        ```
        kubectl apply -f myingressresource.yaml
        ```
        {: pre}
5.   Verify that the Ingress resource was created successfully.

      ```
      kubectl describe ingress myingressresource
      ```
      {: pre}

      1. If messages in the events describe an error in your resource configuration, change the values in your resource file and reapply the file for the resource.

6.   In a web browser, enter the URL of the app service to access.

      ```
      https://<ibm_domain>/<service1_path>
      ```
      {: codeblock}


<br />


### Publicly expose apps using a custom domain with TLS
{: #custom_domain_cert}

You can configure the ALB to route incoming network traffic to the apps in your cluster and use your own TLS certificate to manage the TLS termination, while using your custom domain rather than the IBM-provided domain.
{:shortdesc}

Before you begin:

-   If you do not have one already, [create a standard cluster](cs_clusters.html#clusters_ui).
-   [Target your CLI](cs_cli_install.html#cs_cli_configure) to your cluster to run `kubectl` commands.

To expose an app by using a custom domain with TLS:

1.  Create a custom domain. To create a custom domain, work with your Domain Name Service (DNS) provider or [{{site.data.keyword.Bluemix_notm}} ](/docs/infrastructure/dns/getting-started.html#getting-started-with-dns) to register your custom domain.
2.  Configure your domain to route incoming network traffic to the IBM-provided ALB. Choose between these options:
    -   Define an alias for your custom domain by specifying the IBM-provided domain as a Canonical Name record (CNAME). To find the IBM-provided Ingress domain, run `bx cs cluster-get <cluster_name>` and look for the **Ingress subdomain** field.
    -   Map your custom domain to the portable public IP address of the IBM-provided ALB by adding the IP address as a record. To find the portable public IP address of the ALB, run `bx cs alb-get <public_alb_ID>`.
3.  Either import or create a TLS certificate and key secret:
    * If a TLS certificate is stored in {{site.data.keyword.cloudcerts_long_notm}} that you want to use, you can import its associated secret into your cluster by running the following command:

      ```
      bx cs alb-cert-deploy --secret-name <secret_name> --cluster <cluster_name_or_ID> --cert-crn <certificate_crn>
      ```
      {: pre}

    * If you do not have a TLS certificate ready, follow these steps:
        1. Create a TLS certificate and key for your domain that is encoded in PEM format.
        2. Create a secret that uses your TLS certificate and key. Replace <em>&lt;tls_secret_name&gt;</em> with a name for your Kubernetes secret, <em>&lt;tls_key_filepath&gt;</em> with the path to your custom TLS key file, and <em>&lt;tls_cert_filepath&gt;</em> with the path to your custom TLS certificate file.

            ```
            kubectl create secret tls <tls_secret_name> --key <tls_key_filepath> --cert <tls_cert_filepath>
            ```
            {: pre}
4.  [Deploy your app to the cluster](cs_app.html#app_cli). Ensure that you add a label to your deployment in the metadata section of your configuration file, such as `app: code`. This label is needed to identify all pods where your app is running so that the pods can be included in the Ingress load balancing.

5.   Create a Kubernetes service for each app that you want to expose. Your app must be exposed by a Kubernetes service to be included by the cluster ALB in the Ingress load balancing.
      1.  Open your preferred editor and create a service configuration file that is named, for example, `myalbservice.yaml`.
      2.  Define a service for the app that the ALB will expose to the public.

          ```
          apiVersion: v1
          kind: Service
          metadata:
            name: myalbservice
          spec:
            selector:
              <selector_key>: <selector_value>
            ports:
             - protocol: TCP
               port: 8080
          ```
          {: codeblock}

          <table>
          <thead>
          <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the ALB service YAML file components</th>
          </thead>
          <tbody>
          <tr>
          <td><code>selector</code></td>
          <td>Enter the label key (<em>&lt;selector_key&gt;</em>) and value (<em>&lt;selector_value&gt;</em>) pair that you want to use to target the pods where your app runs. To target your pods and include them in the service load balancing, make sure that the <em>&lt;selector_key&gt;</em> and <em>&lt;selector_value&gt;</em> are the same as the key/value pair that you used in the <code>spec.template.metadata.labels</code> section of your deployment yaml.</td>
           </tr>
           <tr>
           <td><code>port</code></td>
           <td>The port that the service listens on.</td>
           </tr>
           </tbody></table>
      3.  Save your changes.
      4.  Create the service in your cluster. If you have apps deployed in multiple namespaces in your cluster, ensure the service deploys into the same namespace as the app that you want to expose.

          ```
          kubectl apply -f myalbservice.yaml [-n <namespace>]
          ```
          {: pre}
      5.  Repeat these steps for every app that you want to expose to the public.

6.  Create an Ingress resource. Ingress resources define the routing rules for the Kubernetes service that you created for your app and are used by the ALB to route incoming network traffic to the service. You must use one Ingress resource to define routing rules for multiple apps if every app is exposed via a Kubernetes service inside the cluster.
    1.  Open your preferred editor and create an Ingress configuration file that is named, for example, `myingressresource.yaml`.
    2.  Define an Ingress resource in your configuration file that uses your custom domain to route incoming network traffic to your services, and your custom certificate to manage the TLS termination.

        ```
        apiVersion: extensions/v1beta1
        kind: Ingress
        metadata:
          name: myingressresource
        spec:
          tls:
          - hosts:
            - <custom_domain>
            secretName: <tls_secret_name>
          rules:
          - host: <custom_domain>
            http:
              paths:
              - path: /<service1_path>
                backend:
                  serviceName: <service1>
                  servicePort: 80
              - path: /<service2_path>
                backend:
                  serviceName: <service2>
                  servicePort: 80
        ```
        {: codeblock}

        <table>
        <thead>
        <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the YAML file components</th>
        </thead>
        <tbody>
        <tr>
        <td><code>tls/hosts</code></td>
        <td>Replace <em>&lt;custom_domain&gt;</em> with our custom domain that you want to configure for TLS termination.

        </br></br>
        <strong>Note:</strong> Do not use &ast; for your host or leave the host property empty to avoid failures during Ingress creation.</td>
        </tr>
        <tr>
        <td><code>tls/secretName</code></td>
        <td>Replace <em>&lt;tls_secret_name&gt;</em> with the name of the secret that you created earlier that holds your custom TLS certificate and key. If you imported a certificate from {{site.data.keyword.cloudcerts_short}}, you can run <code>bx cs alb-cert-get --cluster <cluster_name_or_ID> --cert-crn <certificate_crn></code> to see the secrets that are associated with a TLS certificate.
        </tr>
        <tr>
        <td><code>host</code></td>
        <td>Replace <em>&lt;custom_domain&gt;</em> with our custom domain that you want to configure for TLS termination.

        </br></br>
        <strong>Note:</strong> Do not use &ast; for your host or leave the host property empty to avoid failures during Ingress creation.
        </td>
        </tr>
        <tr>
        <td><code>path</code></td>
        <td>Replace <em>&lt;service1_path&gt;</em> with a slash or the unique path that your app is listening on so that network traffic can be forwarded to the app.

        </br>
        For every service, you can define an individual path that is appended to your custom domain to create a unique path to your app. When you enter this route into a web browser, network traffic is routed to the ALB. The ALB looks up the associated service and sends network traffic to the service. The service then forwards the traffic to the pods where the app is running. The app must be set up to listen on this path to receive incoming network traffic.

        </br>
        Many apps do not listen on a specific path, but use the root path and a specific port. In this case, define the root path as <code>/</code> and do not specify an individual path for your app.

        </br></br>
        Examples: <ul><li>For <code>https://custom_domain/</code>, enter <code>/</code> as the path.</li><li>For <code>https://custom_domain/service1_path</code>, enter <code>/service1_path</code> as the path.</li></ul>
        <strong>Tip:</strong> To configure Ingress to listen on a path that is different than the path that your app listens on, you can use the [rewrite annotation](cs_annotations.html#rewrite-path) to establish proper routing to your app.
        </td>
        </tr>
        <tr>
        <td><code>serviceName</code></td>
        <td>Replace <em>&lt;service1&gt;</em> with the name of the service that you used when you created the Kubernetes service for your app.</td>
        </tr>
        <tr>
        <td><code>servicePort</code></td>
        <td>The port that your service listens to. Use the same port that you defined when you created the Kubernetes service for your app.</td>
        </tr>
        </tbody></table>

    3.  Save your changes.
    4.  Create the Ingress resource for your cluster.

        ```
        kubectl apply -f myingressresource.yaml
        ```
        {: pre}
7.   Verify that the Ingress resource was created successfully.

      ```
      kubectl describe ingress myingressresource
      ```
      {: pre}

      1. If messages in the events describe an error in your resource configuration, change the values in your resource file and reapply the file for the resource.

8.   In a web browser, enter the URL of the app service to access.

      ```
      https://<custom_domain>/<service1_path>
      ```
      {: codeblock}


<br />


### Publicly expose apps that are outside your cluster using the IBM-provided or a custom domain with TLS
{: #external_endpoint}

You can configure the ALB to include apps that are located outside your cluster. Incoming requests on the IBM-provided or your custom domain are forwarded automatically to the external app.
{:shortdesc}

Before you begin:

-   If you do not have one already, [create a standard cluster](cs_clusters.html#clusters_ui).
-   [Target your CLI](cs_cli_install.html#cs_cli_configure) to your cluster to run `kubectl` commands.
-   Ensure that the external app that you want to include into the cluster load balancing can be accessed by using a public IP address.

You can route incoming network traffic on the IBM-provided domain to apps that are located outside your cluster. If you want to use a custom domain and TLS certificate instead, replace the IBM-provided domain and TLS certificate with your [custom domain and TLS certificate](#custom_domain_cert).

1.  Create a Kubernetes service for your cluster that will forward incoming requests to an external endpoint that you will created.
    1.  Open your preferred editor and create a service configuration file that is named, for example, `myexternalservice.yaml`.
    2.  Define the ALB service.

        ```
        apiVersion: v1
        kind: Service
        metadata:
          name: myexternalservice
        spec:
          ports:
           - protocol: TCP
             port: 8080
        ```
        {: codeblock}

        <table>
        <caption>Understanding the ALB service file components</caption>
        <thead>
        <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the YAML file components</th>
        </thead>
        <tbody>
        <tr>
        <td><code>metadata/name</code></td>
        <td>Replace <em>&lt;myexternalservice&gt;</em> with a name for your service.<p>Learn more about [securing your personal information](cs_secure.html#pi) when you work with Kubernetes resources.</p></td>
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
2.  Configure a Kubernetes endpoint that defines the external location of the app that you want to include into the cluster load balancing.
    1.  Open your preferred editor and create an endpoint configuration file that is named, for example, `myexternalendpoint.yaml`.
    2.  Define your external endpoint. Include all public IP addresses and ports that you can use to access your external app.

        ```
        kind: Endpoints
        apiVersion: v1
        metadata:
          name: myexternalendpoint
        subsets:
          - addresses:
              - ip: <external_IP1>
              - ip: <external_IP2>
            ports:
              - port: <external_port>
        ```
        {: codeblock}

        <table>
        <thead>
        <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the YAML file components</th>
        </thead>
        <tbody>
        <tr>
        <td><code>name</code></td>
        <td>Replace <em>&lt;myexternalendpoint&gt;</em> with the name of the Kubernetes service that you created earlier.</td>
        </tr>
        <tr>
        <td><code>ip</code></td>
        <td>Replace <em>&lt;external_IP&gt;</em> with the public IP addresses to connect to your external app.</td>
         </tr>
         <td><code>port</code></td>
         <td>Replace <em>&lt;external_port&gt;</em> with the port that your external app listens to.</td>
         </tbody></table>

    3.  Save your changes.
    4.  Create the Kubernetes endpoint for your cluster.

        ```
        kubectl apply -f myexternalendpoint.yaml
        ```
        {: pre}
3.   View the IBM-provided domain and TLS certificate. Replace _&lt;cluster_name_or_ID&gt;_ with the name of the cluster where the app is deployed.

      ```
      bx cs cluster-get <cluster_name_or_ID>
      ```
      {: pre}

      Example output:

      ```
      Name:                   mycluster
      ID:                     18a61a63c6a94b658596ca93d087aad9
      State:                  normal
      Created:                2018-01-12T18:33:35+0000
      Location:               dal10
      Master URL:             https://169.xx.xxx.xxx:26268
      Ingress Subdomain:      mycluster-12345.us-south.containers.mybluemix.net
      Ingress Secret:         <ibm_tls_secret>
      Workers:                3
      Version:                1.8.11
      Owner Email:            owner@email.com
      Monitoring Dashboard:   <dashboard_URL>
      ```
      {: screen}

      You can see the IBM-provided domain in the **Ingress subdomain** and the IBM-provided certificate in the **Ingress secret** fields.

4.  Create an Ingress resource. Ingress resources define the routing rules for the Kubernetes service that you created for your app and are used by the ALB to route incoming network traffic to the service. You can use one Ingress resource to define routing rules for multiple external apps as long as every app is exposed with its external endpoint via a Kubernetes service inside the cluster.
    1.  Open your preferred editor and create an Ingress configuration file that is named, for example, `myexternalingress.yaml`.
    2.  Define an Ingress resource in your configuration file that uses the IBM-provided domain and TLS certificate to route incoming network traffic to your external app by using the external endpoint that you defined earlier.

        ```
        apiVersion: extensions/v1beta1
        kind: Ingress
        metadata:
          name: myexternalingress
        spec:
          tls:
          - hosts:
            - <ibm_domain>
            secretName: <ibm_tls_secret>
          rules:
          - host: <ibm_domain>
            http:
              paths:
              - path: /<external_service1_path>
                backend:
                  serviceName: <external_service1>
                  servicePort: 80
              - path: /<external_service2_path>
                backend:
                  serviceName: <external_service2>
                  servicePort: 80
        ```
        {: codeblock}

        <table>
        <thead>
        <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the YAML file components</th>
        </thead>
        <tbody>
        <tr>
        <td><code>tls/hosts</code></td>
        <td>Replace <em>&lt;ibm_domain&gt;</em> with the IBM-provided <strong>Ingress subdomain</strong> name from the previous step. This domain is configured for TLS termination.

        </br></br>
        <strong>Note:</strong> Do not use &ast; for your host or leave the host property empty to avoid failures during Ingress creation.</td>
        </tr>
        <tr>
        <td><code>tls/secretName</code></td>
        <td>Replace <em>&lt;ibm_tls_secret&gt;</em> with the IBM-provided <strong>Ingress secret</strong> from the previous step. This certificate manages the TLS termination.</td>
        </tr>
        <tr>
        <td><code>rules/host</code></td>
        <td>Replace <em>&lt;ibm_domain&gt;</em> with the IBM-provided <strong>Ingress subdomain</strong> name from the previous step. This domain is configured for TLS termination.

        </br></br>
        <strong>Note:</strong> Do not use &ast; for your host or leave the host property empty to avoid failures during Ingress creation.</td>
        </tr>
        <tr>
        <td><code>path</code></td>
        <td>Replace <em>&lt;external_service_path&gt;</em> with a slash or the unique path that your external app is listening on, so that network traffic can be forwarded to the app.

        </br>
        For every service, you can define an individual path that is appended to the IBM-provided or custom domain to create a unique path to your app, for example <code>http://ibm_domain/external_service_path</code> or <code>http://custom_domain/</code>. When you enter this route into a web browser, network traffic is routed to the ALB. The ALB looks up the associated service and sends network traffic to the service. The service the forwards traffic to the external app. The app must be set up to listen on this path to receive incoming network traffic.

        </br></br>
        Many apps do not listen on a specific path, but use the root path and a specific port. In this case, define the root path as <code>/</code> and do not specify an individual path for your app.

        </br></br>
        <strong>Tip:</strong> To configure Ingress to listen on a path that is different than the path that your app listens on, you can use the [rewrite annotation](cs_annotations.html#rewrite-path) to establish proper routing to your app.</td>
        </tr>
        <tr>
        <td><code>serviceName</code></td>
        <td>Replace <em>&lt;external_service&gt;</em> with the name of the service that you used when you created the Kubernetes service for your external app.</td>
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
5.   Verify that the Ingress resource was created successfully.

      ```
      kubectl describe ingress myingressresource
      ```
      {: pre}

      1. If messages in the events describe an error in your resource configuration, change the values in your resource file and reapply the file for the resource.

6.   In a web browser, enter the URL of the app service to access.

      ```
      https://<ibm_domain>/<service1_path>
      ```
      {: codeblock}


<br />


## Exposing apps to a private network
{: #ingress_expose_private}

When you create a standard cluster, an IBM-provided application load balancer (ALB) is created and assigned a portable private IP address and a private route. However, the default private ALB is not automatically enabled.
{:shortdesc}

To expose your app to private networks, first [enable the default private application load balancer](#private_ingress).

You can then configure Ingress for the following scenarios.
-   [Privately expose apps using a custom domain without TLS using an external DNS provider](#private_ingress_no_tls)
-   [Privately expose apps using a custom domain with TLS using an external DNS provider](#private_ingress_tls)
-   [Privately expose apps using an on-premises DNS service](#private_ingress_onprem_dns)

### Enabling the default private application load balancer
{: #private_ingress}

Before you can use the default private ALB, you must enable it with either the IBM-provided portable private IP address or your own portable private IP address.
{:shortdesc}

**Note**: If you used the `--no-subnet` flag when you created the cluster, then you must add a portable private subnet or a user-managed subnet before you can enable the private ALB. For more information, see [Requesting more subnets for your cluster](cs_subnets.html#request).

Before you begin:

-   If you do not have one already, [create a standard cluster](cs_clusters.html#clusters_ui).
-   [Target your CLI](cs_cli_install.html#cs_cli_configure) to your cluster.

To enable the private ALB by using the pre-assigned, IBM-provided portable private IP address:

1. List the available ALBs in your cluster to get the ID of the private ALB. Replace <em>&lt;cluser_name&gt;</em> with the name of the cluster where the app that you want to expose is deployed.

    ```
    bx cs albs --cluster <cluser_name>
    ```
    {: pre}

    The field **Status** for the private ALB is _disabled_.
    ```
    ALB ID                                            Enabled   Status     Type      ALB IP
    private-cr6d779503319d419ea3b4ab171d12c3b8-alb1   false     disabled   private   -
    public-cr6d779503319d419ea3b4ab171d12c3b8-alb1    true      enabled    public    169.xx.xxx.xxx
    ```
    {: screen}

2. Enable the private ALB. Replace <em>&lt;private_ALB_ID&gt;</em> with the ID for private ALB from the output in the previous step.

   ```
   bx cs alb-configure --albID <private_ALB_ID> --enable
   ```
   {: pre}


To enable the private ALB using your own portable private IP address:

1. Configure the user-managed subnet of your chosen IP address to route traffic on the private VLAN of your cluster. Replace <em>&lt;cluser_name&gt;</em> with the name or ID of the cluster where the app that you want to expose is deployed, <em>&lt;subnet_CIDR&gt;</em> with the CIDR of your user-managed subnet, and <em>&lt;private_VLAN_ID&gt;</em> with an available private VLAN ID. You can find the ID of an available private VLAN by running `bx cs vlans`.

   ```
   bx cs cluster-user-subnet-add <cluster_name> <subnet_CIDR> <private_VLAN_ID>
   ```
   {: pre}

2. List the available ALBs in your cluster to get the ID of private ALB.

    ```
    bx cs albs --cluster <cluster_name>
    ```
    {: pre}

    The field **Status** for the private ALB is _disabled_.
    ```
    ALB ID                                            Enabled   Status     Type      ALB IP
    private-cr6d779503319d419ea3b4ab171d12c3b8-alb1   false     disabled   private   -
    public-cr6d779503319d419ea3b4ab171d12c3b8-alb1    true      enabled    public    169.xx.xxx.xxx
    ```
    {: screen}

3. Enable the private ALB. Replace <em>&lt;private_ALB_ID&gt;</em> with the ID for private ALB from the output in the previous step and <em>&lt;user_IP&gt;</em> with the IP address from your user-managed subnet that you want to use.

   ```
   bx cs alb-configure --albID <private_ALB_ID> --enable --user-ip <user_IP>
   ```
   {: pre}

<br />


### Privately expose apps using a custom domain without TLS using an external DNS provider
{: #private_ingress_no_tls}

You can configure the private ALB to route incoming network traffic to the apps in your cluster using a custom domain.
{:shortdesc}

Before you begin:
* [Enable the private application load balancer](#private_ingress).
* If you have private worker nodes and want to use an external DNS provider, you must [configure edge nodes with public access](cs_edge.html#edge) and [configure a Vyatta Gateway Appliance ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/blogs/bluemix/2017/07/kubernetes-and-bluemix-container-based-workloads-part4/). If you want to remain on a private network only, see [Privately expose apps using an on-premises DNS service](#private_ingress_onprem_dns) instead.

To privately expose an app by using a custom domain without TLS using an external DNS provider:

1.  Create a custom domain. To create a custom domain, work with your Domain Name Service (DNS) provider or [{{site.data.keyword.Bluemix_notm}} ](/docs/infrastructure/dns/getting-started.html#getting-started-with-dns) to register your custom domain.
2.  Map your custom domain to the portable private IP address of the IBM-provided private ALB by adding the IP address as a record. To find the portable private IP address of the private ALB, run `bx cs albs --cluster <cluster_name>`.
3.  [Deploy your app to the cluster](cs_app.html#app_cli). Ensure that you add a label to your deployment in the metadata section of your configuration file, such as `app: code`. This label is needed to identify all pods where your app is running so that the pods can be included in the Ingress load balancing.

4.   Create a Kubernetes service for each app that you want to expose. Your app must be exposed by a Kubernetes service to be included by the cluster ALB in the Ingress load balancing.
      1.  Open your preferred editor and create a service configuration file that is named, for example, `myalbservice.yaml`.
      2.  Define a service for the app that the ALB will expose to the private network.

          ```
          apiVersion: v1
          kind: Service
          metadata:
            name: myalbservice
          spec:
            selector:
              <selector_key>: <selector_value>
            ports:
             - protocol: TCP
               port: 8080
          ```
          {: codeblock}

          <table>
          <thead>
          <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the ALB service YAML file components</th>
          </thead>
          <tbody>
          <tr>
          <td><code>selector</code></td>
          <td>Enter the label key (<em>&lt;selector_key&gt;</em>) and value (<em>&lt;selector_value&gt;</em>) pair that you want to use to target the pods where your app runs. To target your pods and include them in the service load balancing, make sure that the <em>&lt;selector_key&gt;</em> and <em>&lt;selector_value&gt;</em> are the same as the key/value pair that you used in the <code>spec.template.metadata.labels</code> section of your deployment yaml.</td>
           </tr>
           <tr>
           <td><code>port</code></td>
           <td>The port that the service listens on.</td>
           </tr>
           </tbody></table>
      3.  Save your changes.
      4.  Create the service in your cluster. If you have apps deployed in multiple namespaces in your cluster, ensure the service deploys into the same namespace as the app that you want to expose.

          ```
          kubectl apply -f myalbservice.yaml [-n <namespace>]
          ```
          {: pre}
      5.  Repeat these steps for every app that you want to expose to the private network.

5.  Create an Ingress resource. Ingress resources define the routing rules for the Kubernetes service that you created for your app and are used by the ALB to route incoming network traffic to the service. You must use one Ingress resource to define routing rules for multiple apps if every app is exposed via a Kubernetes service inside the cluster.
    1.  Open your preferred editor and create an Ingress configuration file that is named, for example, `myingressresource.yaml`.
    2.  Define an Ingress resource in your configuration file that uses your custom domain to route incoming network traffic to your services.

        ```
        apiVersion: extensions/v1beta1
        kind: Ingress
        metadata:
          name: myingressresource
          annotations:
            ingress.bluemix.net/ALB-ID: "<private_ALB_ID>"
        spec:
          rules:
          - host: <custom_domain>
            http:
              paths:
              - path: /<service1_path>
                backend:
                  serviceName: <service1>
                  servicePort: 80
              - path: /<service2_path>
                backend:
                  serviceName: <service2>
                  servicePort: 80
        ```
        {: codeblock}

        <table>
        <thead>
        <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the YAML file components</th>
        </thead>
        <tbody>
        <tr>
        <td><code>ingress.bluemix.net/ALB-ID</code></td>
        <td>Replace <em>&lt;private_ALB_ID&gt;</em> with the ID for your private ALB. Run <code>bx cs albs --cluster <my_cluster></code> to find the ALB ID. For more information about this Ingress annotation, see [Private application load balancer routing](cs_annotations.html#alb-id).</td>
        </tr>
        <td><code>host</code></td>
        <td>Replace <em>&lt;custom_domain&gt;</em> with your custom domain.

        </br></br>
        <strong>Note:</strong> Do not use &ast; for your host or leave the host property empty to avoid failures during Ingress creation.
        </td>
        </tr>
        <tr>
        <td><code>path</code></td>
        <td>Replace <em>&lt;service1_path&gt;</em> with a slash or the unique path that your app is listening on so that network traffic can be forwarded to the app.

        </br>

        For every service, you can define an individual path that is appended to your custom domain to create a unique path to your app. When you enter this route into a web browser, network traffic is routed to the ALB. The ALB looks up the associated service and sends network traffic to the service. The service then forwards the traffic to the pods where the app is running. The app must be set up to listen on this path to receive incoming network traffic.

        </br>
        Many apps do not listen on a specific path, but use the root path and a specific port. In this case, define the root path as <code>/</code> and do not specify an individual path for your app.

        </br></br>
        Examples: <ul><li>For <code>https://custom_domain/</code>, enter <code>/</code> as the path.</li><li>For <code>https://custom_domain/service1_path</code>, enter <code>/service1_path</code> as the path.</li></ul>
        <strong>Tip:</strong> To configure Ingress to listen on a path that is different than the path that your app listens on, you can use the [rewrite annotation](cs_annotations.html#rewrite-path) to establish proper routing to your app.
        </td>
        </tr>
        <tr>
        <td><code>serviceName</code></td>
        <td>Replace <em>&lt;service1&gt;</em> with the name of the service that you used when you created the Kubernetes service for your app.</td>
        </tr>
        <tr>
        <td><code>servicePort</code></td>
        <td>The port that your service listens to. Use the same port that you defined when you created the Kubernetes service for your app.</td>
        </tr>
        </tbody></table>

    3.  Save your changes.
    4.  Create the Ingress resource for your cluster.

        ```
        kubectl apply -f myingressresource.yaml
        ```
        {: pre}
6.   Verify that the Ingress resource was created successfully.

      ```
      kubectl describe ingress myingressresource
      ```
      {: pre}

      1. If messages in the events describe an error in your resource configuration, change the values in your resource file and reapply the file for the resource.

7.   In a web browser, enter the URL of the app service to access.

      ```
      https://<custom_domain>/<service1_path>
      ```
      {: codeblock}


<br />


### Privately expose apps using a custom domain with TLS using an external DNS provider
{: #private_ingress_tls}

You can use private ALBs to route incoming network traffic to apps in your cluster. Also, use your own TLS certificate to manage the TLS termination while using your custom domain.
{:shortdesc}

Before you begin:
* [Enable the private application load balancer](#private_ingress).
* If you have private worker nodes and want to use an external DNS provider, you must [configure edge nodes with public access](cs_edge.html#edge) and [configure a Vyatta Gateway Appliance ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/blogs/bluemix/2017/07/kubernetes-and-bluemix-container-based-workloads-part4/). If you want to remain on a private network only, see [Privately expose apps using an on-premises DNS service](#private_ingress_onprem_dns) instead.

To privately expose an app by using a custom domain with TLS using an external DNS provider:

1.  Create a custom domain. To create a custom domain, work with your Domain Name Service (DNS) provider or [{{site.data.keyword.Bluemix_notm}} ](/docs/infrastructure/dns/getting-started.html#getting-started-with-dns) to register your custom domain.
2.  Map your custom domain to the portable private IP address of the IBM-provided private ALB by adding the IP address as a record. To find the portable private IP address of the private ALB, run `bx cs albs --cluster <cluster_name>`.
3.  Either import or create a TLS certificate and key secret:
    * If a TLS certificate is stored in {{site.data.keyword.cloudcerts_long_notm}} that you want to use, you can import its associated secret into your cluster by running `bx cs alb-cert-deploy --secret-name <secret_name> --cluster <cluster_name_or_ID> --cert-crn <certificate_crn>`.
    * If you do not have a TLS certificate ready, follow these steps:
        1. Create a TLS certificate and key for your domain that is encoded in PEM format.
        2. Create a secret that uses your TLS certificate and key. Replace <em>&lt;tls_secret_name&gt;</em> with a name for your Kubernetes secret, <em>&lt;tls_key_filepath&gt;</em> with the path to your custom TLS key file, and <em>&lt;tls_cert_filepath&gt;</em> with the path to your custom TLS certificate file.

            ```
            kubectl create secret tls <tls_secret_name> --key <tls_key_filepath> --cert <tls_cert_filepath>
            ```
            {: pre}
4.  [Deploy your app to the cluster](cs_app.html#app_cli). Ensure that you add a label to your deployment in the metadata section of your configuration file, such as `app: code`. This label is needed to identify all pods where your app is running so that the pods can be included in the Ingress load balancing.

5.   Create a Kubernetes service for each app that you want to expose. Your app must be exposed by a Kubernetes service to be included by the cluster ALB in the Ingress load balancing.
      1.  Open your preferred editor and create a service configuration file that is named, for example, `myalbservice.yaml`.
      2.  Define a service for the app that the ALB will expose to the private network.

          ```
          apiVersion: v1
          kind: Service
          metadata:
            name: myalbservice
          spec:
            selector:
              <selector_key>: <selector_value>
            ports:
             - protocol: TCP
               port: 8080
          ```
          {: codeblock}

          <table>
          <thead>
          <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the ALB service YAML file components</th>
          </thead>
          <tbody>
          <tr>
          <td><code>selector</code></td>
          <td>Enter the label key (<em>&lt;selector_key&gt;</em>) and value (<em>&lt;selector_value&gt;</em>) pair that you want to use to target the pods where your app runs. To target your pods and include them in the service load balancing, make sure that the <em>&lt;selector_key&gt;</em> and <em>&lt;selector_value&gt;</em> are the same as the key/value pair that you used in the <code>spec.template.metadata.labels</code> section of your deployment yaml.</td>
           </tr>
           <tr>
           <td><code>port</code></td>
           <td>The port that the service listens on.</td>
           </tr>
           </tbody></table>
      3.  Save your changes.
      4.  Create the service in your cluster. If you have apps deployed in multiple namespaces in your cluster, ensure the service deploys into the same namespace as the app that you want to expose.

          ```
          kubectl apply -f myalbservice.yaml [-n <namespace>]
          ```
          {: pre}
      5.  Repeat these steps for every app that you want to expose to the private network.

6.  Create an Ingress resource. Ingress resources define the routing rules for the Kubernetes service that you created for your app and are used by the ALB to route incoming network traffic to the service. You must use one Ingress resource to define routing rules for multiple apps if every app is exposed via a Kubernetes service inside the cluster.
    1.  Open your preferred editor and create an Ingress configuration file that is named, for example, `myingressresource.yaml`.
    2.  Define an Ingress resource in your configuration file that uses your custom domain to route incoming network traffic to your services, and your custom certificate to manage the TLS termination.

          ```
          apiVersion: extensions/v1beta1
          kind: Ingress
          metadata:
            name: myingressresource
            annotations:
              ingress.bluemix.net/ALB-ID: "<private_ALB_ID>"
          spec:
            tls:
            - hosts:
              - <custom_domain>
              secretName: <tls_secret_name>
            rules:
            - host: <custom_domain>
              http:
                paths:
                - path: /<service1_path>
                  backend:
                    serviceName: <service1>
                    servicePort: 80
                - path: /<service2_path>
                  backend:
                    serviceName: <service2>
                    servicePort: 80
           ```
           {: pre}

           <table>
          <thead>
          <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the YAML file components</th>
          </thead>
          <tbody>
          <tr>
          <td><code>ingress.bluemix.net/ALB-ID</code></td>
          <td>Replace <em>&lt;private_ALB_ID&gt;</em> with the ID for your private ALB. Run <code>bx cs albs --cluster <cluster_name></code> to find the ALB ID. For more information about this Ingress annotation, see [Private application load balancer routing (ALB-ID)](cs_annotations.html#alb-id).</td>
          </tr>
          <tr>
          <td><code>tls/hosts</code></td>
          <td>Replace <em>&lt;custom_domain&gt;</em> with our custom domain that you want to configure for TLS termination.

          </br></br>
          <strong>Note:</strong> Do not use &ast; for your host or leave the host property empty to avoid failures during Ingress creation.</td>
          </tr>
          <tr>
          <td><code>tls/secretName</code></td>
          <td>Replace <em>&lt;tls_secret_name&gt;</em> with the name of the secret that you created earlier and that holds your custom TLS certificate and key. If you imported a certificate from {{site.data.keyword.cloudcerts_short}}, you can run <code>bx cs alb-cert-get --cluster <cluster_name_or_ID> --cert-crn <certificate_crn></code> to see the secrets that are associated with a TLS certificate.
          </tr>
          <tr>
          <td><code>host</code></td>
          <td>Replace <em>&lt;custom_domain&gt;</em> with your custom domain that you want to configure for TLS termination.

          </br></br>
          <strong>Note:</strong> Do not use &ast; for your host or leave the host property empty to avoid failures during Ingress creation.
          </td>
          </tr>
          <tr>
          <td><code>path</code></td>
          <td>Replace <em>&lt;service1_path&gt;</em> with a slash or the unique path that your app is listening on so that network traffic can be forwarded to the app.

          </br>
          For every service, you can define an individual path that is appended to your custom domain to create a unique path to your app. When you enter this route into a web browser, network traffic is routed to the ALB. The ALB looks up the associated service, and sends network traffic to the service and to the pods where the app is running by using the same path. The app must be set up to listen on this path to receive incoming network traffic.

          </br>
          Many apps do not listen on a specific path, but use the root path and a specific port. In this case, define the root path as <code>/</code> and do not specify an individual path for your app.

          </br></br>
          Examples: <ul><li>For <code>https://custom_domain/</code>, enter <code>/</code> as the path.</li><li>For <code>https://custom_domain/service1_path</code>, enter <code>/service1_path</code> as the path.</li></ul>
          <strong>Tip:</strong> To configure Ingress to listen on a path that is different than the path that your app listens on, you can use the [rewrite annotation](cs_annotations.html#rewrite-path) to establish proper routing to your app.
          </td>
          </tr>
          <tr>
          <td><code>serviceName</code></td>
          <td>Replace <em>&lt;service1&gt;</em> with the name of the service that you used when you created the Kubernetes service for your app.</td>
          </tr>
          <tr>
          <td><code>servicePort</code></td>
          <td>The port that your service listens to. Use the same port that you defined when you created the Kubernetes service for your app.</td>
          </tr>
           </tbody></table>

    3.  Save your changes.
    4.  Create the Ingress resource for your cluster.

        ```
        kubectl apply -f myingressresource.yaml
        ```
        {: pre}
7.   Verify that the Ingress resource was created successfully.

      ```
      kubectl describe ingress myingressresource
      ```
      {: pre}

      1. If messages in the events describe an error in your resource configuration, change the values in your resource file and reapply the file for the resource.

8.   In a web browser, enter the URL of the app service to access.

      ```
      https://<custom_domain>/<service1_path>
      ```
      {: codeblock}


For a comprehensive tutorial on how to secure microservice-to-microservice communication across your clusters by using the private ALB with TLS, check out [this blog post ![External link icon](../icons/launch-glyph.svg "External link icon")]](https://medium.com/ibm-cloud/secure-microservice-to-microservice-communication-across-kubernetes-clusters-using-a-private-ecbe2a8d4fe2).

<br />


### Privately expose apps using an on-premises DNS service
{: #private_ingress_onprem_dns}

You can configure the private ALB to route incoming network traffic to the apps in your cluster using a custom domain. When you have private worker nodes and want to remain on a private network only, you can use a private, on-premises DNS to resolve URL requests to your apps.
{:shortdesc}

1. [Enable the private application load balancer](#private_ingress).
2. To allow your private worker nodes to communicate with the Kubernetes master, [set up VPN connectivity](cs_vpn.html).
3. [Configure your DNS service ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/tasks/administer-cluster/dns-custom-nameservers/).
4. Create a custom domain and register it with your DNS service.
5.  Map your custom domain to the portable private IP address of the IBM-provided private ALB by adding the IP address as a record. To find the portable private IP address of the private ALB, run `bx cs albs --cluster <cluster_name>`.
6.  [Deploy your app to the cluster](cs_app.html#app_cli). Ensure that you add a label to your deployment in the metadata section of your configuration file, such as `app: code`. This label is needed to identify all pods where your app is running so that the pods can be included in the Ingress load balancing.

7.   Create a Kubernetes service for each app that you want to expose. Your app must be exposed by a Kubernetes service to be included by the cluster ALB in the Ingress load balancing.
      1.  Open your preferred editor and create a service configuration file that is named, for example, `myalbservice.yaml`.
      2.  Define a service for the app that the ALB will expose to the private network.

          ```
          apiVersion: v1
          kind: Service
          metadata:
            name: myalbservice
          spec:
            selector:
              <selector_key>: <selector_value>
            ports:
             - protocol: TCP
               port: 8080
          ```
          {: codeblock}

          <table>
          <thead>
          <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the ALB service YAML file components</th>
          </thead>
          <tbody>
          <tr>
          <td><code>selector</code></td>
          <td>Enter the label key (<em>&lt;selector_key&gt;</em>) and value (<em>&lt;selector_value&gt;</em>) pair that you want to use to target the pods where your app runs. To target your pods and include them in the service load balancing, make sure that the <em>&lt;selector_key&gt;</em> and <em>&lt;selector_value&gt;</em> are the same as the key/value pair that you used in the <code>spec.template.metadata.labels</code> section of your deployment yaml.</td>
           </tr>
           <tr>
           <td><code>port</code></td>
           <td>The port that the service listens on.</td>
           </tr>
           </tbody></table>
      3.  Save your changes.
      4.  Create the service in your cluster. If you have apps deployed in multiple namespaces in your cluster, ensure the service deploys into the same namespace as the app that you want to expose.

          ```
          kubectl apply -f myalbservice.yaml [-n <namespace>]
          ```
          {: pre}
      5.  Repeat these steps for every app that you want to expose to the private network.

8.  Create an Ingress resource. Ingress resources define the routing rules for the Kubernetes service that you created for your app and are used by the ALB to route incoming network traffic to the service. You must use one Ingress resource to define routing rules for multiple apps if every app is exposed via a Kubernetes service inside the cluster.
  1.  Open your preferred editor and create an Ingress configuration file that is named, for example, `myingressresource.yaml`.
  2.  Define an Ingress resource in your configuration file that uses your custom domain to route incoming network traffic to your services.

    **Note**: If you do not want to use TLS, remove the `tls` section from the following example.

    ```
    apiVersion: extensions/v1beta1
    kind: Ingress
    metadata:
      name: myingressresource
      annotations:
        ingress.bluemix.net/ALB-ID: "<private_ALB_ID>"
    spec:
      tls:
      - hosts:
        - <custom_domain>
        secretName: <tls_secret_name>
      rules:
      - host: <custom_domain>
        http:
          paths:
          - path: /<service1_path>
            backend:
              serviceName: <service1>
              servicePort: 80
          - path: /<service2_path>
            backend:
              serviceName: <service2>
              servicePort: 80
     ```
     {: codeblock}

    <table>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the YAML file components</th>
    </thead>
    <tbody>
    <tr>
    <td><code>ingress.bluemix.net/ALB-ID</code></td>
    <td>Replace <em>&lt;private_ALB_ID&gt;</em> with the ID for your private ALB. Run <code>bx cs albs --cluster <my_cluster></code> to find the ALB ID. For more information about this Ingress annotation, see [Private application load balancer routing (ALB-ID)](cs_annotations.html#alb-id).</td>
    </tr>
    <tr>
    <td><code>tls/hosts</code></td>
    <td>If you are using the `tls` section, replace <em>&lt;custom_domain&gt;</em> with our custom domain that you want to configure for TLS termination.
    </br></br>
    <strong>Note:</strong> Do not use &ast; for your host or leave the host property empty to avoid failures during Ingress creation. </td>
    </tr>
    <tr>
    <td><code>tls/secretName</code></td>
    <td>If you are using the `tls` section, replace <em>&lt;tls_secret_name&gt;</em> with the name of the secret that you created earlier and that holds your custom TLS certificate and key. If you imported a certificate from {{site.data.keyword.cloudcerts_short}}, you can run <code>bx cs alb-cert-get --cluster <cluster_name_or_ID> --cert-crn <certificate_crn></code> to see the secrets that are associated with a TLS certificate.
    </tr>
    <tr>
    <td><code>host</code></td>
    <td>Replace <em>&lt;custom_domain&gt;</em> with your custom domain that you want to configure for TLS termination.
    </br></br>
    <strong>Note:</strong> Do not use &ast; for your host or leave the host property empty to avoid failures during Ingress creation.
    </td>
    </tr>
    <tr>
    <td><code>path</code></td>
    <td>Replace <em>&lt;service1_path&gt;</em> with a slash or the unique path that your app is listening on so that network traffic can be forwarded to the app.
    </br>
    For every service, you can define an individual path that is appended to your custom domain to create a unique path to your app. When you enter this route into a web browser, network traffic is routed to the ALB. The ALB looks up the associated service, and sends network traffic to the service and to the pods where the app is running by using the same path. The app must be set up to listen on this path to receive incoming network traffic.
    </br>
    Many apps do not listen on a specific path, but use the root path and a specific port. In this case, define the root path as <code>/</code> and do not specify an individual path for your app.
    </br></br>
    Examples: <ul><li>For <code>https://custom_domain/</code>, enter <code>/</code> as the path.</li><li>For <code>https://custom_domain/service1_path</code>, enter <code>/service1_path</code> as the path.</li></ul>
    <strong>Tip:</strong> To configure Ingress to listen on a path that is different than the path that your app listens on, you can use the [rewrite annotation](cs_annotations.html#rewrite-path) to establish proper routing to your app.
    </td>
    </tr>
    <tr>
    <td><code>serviceName</code></td>
    <td>Replace <em>&lt;service1&gt;</em> with the name of the service that you used when you created the Kubernetes service for your app.</td>
    </tr>
    <tr>
    <td><code>servicePort</code></td>
    <td>The port that your service listens to. Use the same port that you defined when you created the Kubernetes service for your app.</td>
    </tr>
    </tbody></table>

  3.  Save your changes.
  4.  Create the Ingress resource for your cluster.

      ```
      kubectl apply -f myingressresource.yaml
      ```
      {: pre}
9.   Verify that the Ingress resource was created successfully.

      ```
      kubectl describe ingress myingressresource
      ```
      {: pre}

      1. If messages in the events describe an error in your resource configuration, change the values in your resource file and reapply the file for the resource.

10.   In a web browser, enter the URL of the app service to access.

      ```
      https://<custom_domain>/<service1_path>
      ```
      {: codeblock}


<br />


## Optional application load balancer configurations
{: #configure_alb}

You can further configure an application load balancer with the following options.

-   [Opening ports in the Ingress application load balancer](#opening_ingress_ports)
-   [Configuring SSL protocols and SSL ciphers at the HTTP level](#ssl_protocols_ciphers)
-   [Customizing the Ingress log format](#ingress_log_format)
-   [Customizing your application load balancer with annotations](cs_annotations.html)
{: #ingress_annotation}


### Opening ports in the Ingress application load balancer
{: #opening_ingress_ports}

By default, only ports 80 and 443 are exposed in the Ingress ALB. To expose other ports, you can edit the `ibm-cloud-provider-ingress-cm` configmap resource.
{:shortdesc}

1. Create and open a local version of the configuration file for the `ibm-cloud-provider-ingress-cm` configmap resource.

    ```
    kubectl edit cm ibm-cloud-provider-ingress-cm -n kube-system
    ```
    {: pre}

2. Add a <code>data</code> section and specify public ports `80`, `443`, and any other ports you want to expose separated by a semi-colon (;).

    **Note**: When specifying the ports, 80 and 443 must also be included to keep those ports open. Any port that is not specified is closed.

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
    {: screen}

3. Save the configuration file.

4. Verify that the configmap changes were applied.

 ```
 kubectl get cm ibm-cloud-provider-ingress-cm -n kube-system -o yaml
 ```
 {: pre}

 Output:
 ```
 Name:        ibm-cloud-provider-ingress-cm
 Namespace:   kube-system
 Labels:      <none>
 Annotations: <none>

 Data
 ====

  public-ports: "80;443;9443"
 ```
 {: screen}

For more information about configmap resources, see the [Kubernetes documentation](https://kubernetes-v1-4.github.io/docs/user-guide/configmap/).

### Configuring SSL protocols and SSL ciphers at the HTTP level
{: #ssl_protocols_ciphers}

Enable SSL protocols and ciphers at the global HTTP level by editing the `ibm-cloud-provider-ingress-cm` configmap.
{:shortdesc}



**Note**: When you specify the enabled protocols for all hosts, the TLSv1.1 and TLSv1.2 parameters (1.1.13, 1.0.12) work only when OpenSSL 1.0.1 or higher is used. The TLSv1.3 parameter (1.13.0) works only when OpenSSL 1.1.1 built with TLSv1.3 support is used.

To edit the configmap to enable SSL protocols and ciphers:

1. Create and open a local version of the configuration file for the `ibm-cloud-provider-ingress-cm` configmap resource.

    ```
    kubectl edit cm ibm-cloud-provider-ingress-cm -n kube-system
    ```
    {: pre}

2. Add the SSL protocols and ciphers. Format ciphers according to the [OpenSSL library cipher list format ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.openssl.org/docs/man1.0.2/apps/ciphers.html).

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

3. Save the configuration file.

4. Verify that the configmap changes were applied.

   ```
   kubectl get cm ibm-cloud-provider-ingress-cm -n kube-system -o yaml
   ```
   {: pre}

   Output:
   ```
   Name:        ibm-cloud-provider-ingress-cm
   Namespace:   kube-system
   Labels:      <none>
   Annotations: <none>

   Data
   ====

    ssl-protocols: "TLSv1 TLSv1.1 TLSv1.2"
    ssl-ciphers: "HIGH:!aNULL:!MD5"
   ```
   {: screen}

### Customizing Ingress log content and format
{: #ingress_log_format}

You can customize the content and format of logs that are collected for the Ingress ALB.
{:shortdesc}

By default, Ingress logs are formatted in JSON and display common log fields. However, you can also create a custom log format. To choose which log components are forwarded and how they are arranged in the log output:

1. Create and open a local version of the configuration file for the `ibm-cloud-provider-ingress-cm` configmap resource.

    ```
    kubectl edit cm ibm-cloud-provider-ingress-cm -n kube-system
    ```
    {: pre}

2. Add a <code>data</code> section. Add the `log-format` field and, optionally, the `log-format-escape-json` field.

    ```
    apiVersion: v1
    data:
      log-format: '{<key1>: <log_variable1>, <key2>: <log_variable2>, <key3>: <log_variable3>}'
      log-format-escape-json: "true"
    kind: ConfigMap
    metadata:
      name: ibm-cloud-provider-ingress-cm
      namespace: kube-system
    ```
    {: pre}

    <table>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the log-format configuration</th>
    </thead>
    <tbody>
    <tr>
    <td><code>log-format</code></td>
    <td>Replace <code>&lt;key&gt;</code> with the name for the log component and <code>&lt;log_variable&gt;</code> with a variable for the log component that you want to collect in log entries. You can include text and punctuation that you want the log entry to contain, such as quotation marks around string values and commas to separate log components. For example, formatting a component like <code>request: "$request",</code> generates the following in a log entry: <code>request: "GET / HTTP/1.1",</code> . For a list of all the variables you can use, see the <a href="http://nginx.org/en/docs/varindex.html">Nginx variable index</a>.<br><br>To log an additional header such as <em>x-custom-ID</em>, add the following key-value pair to the custom log content: <br><pre class="pre"><code>customID: $http_x_custom_id</code></pre> <br>Note that hyphens (<code>-</code>) are converted to underscores (<code>_</code>) and that <code>$http_</code> must be prepended to the custom header name.</td>
    </tr>
    <tr>
    <td><code>log-format-escape-json</code></td>
    <td>Optional: By default, logs are generated in text format. To generate logs in JSON format, add the <code>log-format-escape-json</code> field and use value <code>true</code>.</td>
    </tr>
    </tbody></table>
    </dd>
    </dl>

    For example, your log format might contain the following variables:
    ```
    apiVersion: v1
    data:
      log-format: '{remote_address: $remote_addr, remote_user: "$remote_user",
                    time_date: [$time_local], request: "$request",
                    status: $status, http_referer: "$http_referer",
                    http_user_agent: "$http_user_agent",
                    request_id: $request_id}'
    kind: ConfigMap
    metadata:
      name: ibm-cloud-provider-ingress-cm
      namespace: kube-system
    ```
    {: screen}

    A log entry according to this format looks like the following:
    ```
    remote_address: 127.0.0.1, remote_user: "dbmanager", time_date: [30/Mar/2018:18:52:17 +0000], request: "GET / HTTP/1.1", status: 401, http_referer: "-", http_user_agent: "Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:47.0) Gecko/20100101 Firefox/47.0", request_id: a02b2dea9cf06344a25611c1d7ad72db
    ```
    {: screen}

    If you want to create a custom log format that is based on the default format for ALB logs, you can add the following section to your configmap and modify it as needed:
    ```
    apiVersion: v1
    data:
      log-format: '{"time_date": "$time_iso8601", "client": "$remote_addr",
                    "host": "$http_host", "scheme": "$scheme",
                    "request_method": "$request_method", "request_uri": "$uri",
                    "request_id": "$request_id", "status": $status,
                    "upstream_addr": "$upstream_addr", "upstream_status":
                    $upstream_status, "request_time": $request_time,
                    "upstream_response_time": $upstream_response_time,
                    "upstream_connect_time": $upstream_connect_time,
                    "upstream_header_time": $upstream_header_time}'
      log-format-escape-json: "true"
    kind: ConfigMap
    metadata:
      name: ibm-cloud-provider-ingress-cm
      namespace: kube-system
    ```
    {: codeblock}

4. Save the configuration file.

5. Verify that the configmap changes were applied.

 ```
 kubectl get cm ibm-cloud-provider-ingress-cm -n kube-system -o yaml
 ```
 {: pre}

 Example output:
 ```
 Name:        ibm-cloud-provider-ingress-cm
 Namespace:   kube-system
 Labels:      <none>
 Annotations: <none>

 Data
 ====

  log-format: '{remote_address: $remote_addr, remote_user: "$remote_user", time_date: [$time_local], request: "$request", status: $status, http_referer: "$http_referer", http_user_agent: "$http_user_agent", request_id: $request_id}'
  log-format-escape-json: "true"
 ```
 {: screen}

4. To view the Ingress ALB logs, [create a logging configuration for the Ingress service](cs_health.html#logging) in your cluster.

<br />



