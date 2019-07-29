---

copyright:
  years: 2014, 2019
lastupdated: "2019-07-26"

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


# Deploying serverless apps with Knative
{: #serverless-apps-knative}

Learn how to install and use Knative in a Kubernetes cluster in {{site.data.keyword.containerlong_notm}}.
{: shortdesc}

**What is Knative and why I want use it?**</br>
[Knative](https://github.com/knative/docs) is an open source platform that was developed by IBM, Google, Pivotal, Red Hat, Cisco, and others. The goal is to extend the capabilities of Kubernetes to help you create modern, source-centric containerized, and serverless apps on top of your Kubernetes cluster. The platform is designed to address the needs of developers who today must decide what type of app they want to run in the cloud: 12-factor apps, containers, or functions. Each type of app requires an open source or proprietary solution that is tailored to these apps: Cloud Foundry for 12-factor apps, Kubernetes for containers, and OpenWhisk and others for functions. In the past, developers had to decide what approach they wanted to follow, which led to inflexibility and complexity when different types of apps had to be combined.  

Knative uses a consistent approach across programming languages and frameworks to abstract the operational burden of building, deploying, and managing workloads in Kubernetes so that developers can focus on what matters most to them: the source code. You can use proven build processes that you are already familiar with, such as Kaniko, Dockerfile, Bazel, and others. By integrating with Istio, Knative ensures that your serverless and containerized workloads can be easily exposed on the internet, monitored, and controlled, and that your data is encrypted during transit.

**How does Knative work?**</br>
Knative comes with two key components, or _primitives_, that help you to deploy, and manage your serverless apps in your Kubernetes cluster:

- **Serving:** The `Serving` primitive helps to deploy serverless apps as Knative services and to automatically scale them, even down to zero instances. To expose your serverless and containerized workloads, Knative uses Istio. When you install the managed Knative add-on, the managed Istio add-on is automatically installed as well. By using the traffic management and intelligent routing capabilities of Istio, you can control what traffic is routed to a specific version of your service, which makes it easy for a developer to test and roll out a new app version or do A-B testing.
- **Eventing:** With the `Eventing` primitive, you can create triggers or event streams that other services can subscribe to. For example, you might want to kick off a new build of your app every time code is pushed to your GitHub master repo. Or you want to run a serverless app only if the temperature drops below freezing point. For example, the `Eventing` primitive can be integrated into your CI/CD pipeline to automate the build and deployment of apps in case a specific event occurs.

Note: Knative used to have a component called **Build** which supported creating a set of steps to build your app from source code to a container image. This code base has been spun off from Knative into a new project called [Tekton](https:/tekton.dev).

**What is the Managed Knative on {{site.data.keyword.containerlong_notm}} (experimental) add-on?** </br>
Managed Knative on {{site.data.keyword.containerlong_notm}} is a [managed add-on](/docs/containers?topic=containers-managed-addons#managed-addons) that integrates Knative and Istio directly with your Kubernetes cluster. The Knative and Istio versions in the add-on are tested by IBM and supported for the use in {{site.data.keyword.containerlong_notm}}. For more information about managed add-ons, see [Adding services by using managed add-ons](/docs/containers?topic=containers-managed-addons#managed-addons).

**Are there any limitations?** </br>
If you installed the [container image security enforcer admission controller](/docs/services/Registry?topic=registry-security_enforce#security_enforce) in your cluster, you cannot enable the managed Knative add-on in your cluster.

## Setting up Knative in your cluster
{: #knative-setup}

Knative builds on top of Istio to ensure that your serverless and containerized workloads can be exposed within the cluster and on the internet. With Istio, you can also monitor and control network traffic between your services and ensure that your data is encrypted during transit. When you install the managed Knative add-on, the managed Istio add-on is automatically installed as well. However, for Knative's use of Istio, this is all hidden from the developer.
{: shortdesc}

Before you begin:
-  [Install the IBM Cloud CLI, the {{site.data.keyword.containerlong_notm}} plug-in, and the Kubernetes CLI](/docs/containers?topic=containers-cs_cli_install#cs_cli_install_steps). Make sure to install the `kubectl` CLI version that matches the Kubernetes version of your cluster.
-  [Create a standard cluster with at least 3 worker nodes that each have 4 cores and 16 GB memory (`b3c.4x16`) or more](/docs/containers?topic=containers-clusters#clusters_ui). Additionally, the cluster and worker nodes must run at least the minimum supported version of Kubernetes, which you can review by running `ibmcloud ks addon-versions --addon knative`.
-  Ensure that you have the [**Writer** or **Manager** {{site.data.keyword.cloud_notm}} IAM service role](/docs/containers?topic=containers-users#platform) for {{site.data.keyword.containerlong_notm}}.
-  [Target the CLI to your cluster](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure).
</br>

To install Knative in your cluster:

1. Enable the managed Knative add-on in your cluster. When you enable Knative in your cluster, Istio and all Knative components are installed in your cluster.
   ```
   ibmcloud ks cluster-addon-enable knative --cluster <cluster_name_or_ID> -y
   ```
   {: pre}

   Example output:
   ```
   Enabling add-on knative for cluster knative...
   OK
   ```
   {: screen}

   The installation of all Knative components might take a few minutes to complete.

2. Verify that Knagive is successfully installed. All pods for the Knative services must be in a `Running` status.
   ```
   kubectl get pods --namespace knative-serving
   ```
   {: pre}

   Example output:
   ```
   NAME                                      READY   STATUS    RESTARTS   AGE
   activator-7674448cb7-hjpn8                2/2     Running   0          13d
   autoscaler-79b474fddd-tnnwh               2/2     Running   0          13d
   controller-946f66cf9-6tq78                1/1     Running   0          13d
   networking-certmanager-6f48595847-xvmln   1/1     Running   0          13d
   networking-istio-5f8d474b6d-prvmz         1/1     Running   0          13d
   webhook-8896fd46b-wl2bf                   1/1     Running   0          9d
   ```
   {: screen}

3. Optional: If you want to use Istio for all apps in the `default` namespace, add the `istio-injection=enabled` label to the namespace. Each serverless app pod must run an Envoy proxy sidecar so that the app can be included in the Istio service mesh. This label allows Istio to automatically modify the pod template specification in new app deployments so that pods are created with Envoy proxy sidecar containers. Knative will continue to work even without Istio enabled in your namespaces though.
   ```
   kubectl label namespace default istio-injection=enabled
   ```
   {: pre}

4. Verify that all pods of the Knative `Serving` component are in a `Running` state.
   ```
   kubectl get pods --namespace knative-serving
   ```
   {: pre}

   Example output:
   ```
   NAME                          READY     STATUS    RESTARTS   AGE
   activator-598b4b7787-ps7mj    2/2       Running   0          21m
   autoscaler-5cf5cfb4dc-mcc2l   2/2       Running   0          21m
   controller-7fc84c6584-qlzk2   1/1       Running   0          21m
   webhook-7797ffb6bf-wg46v      1/1       Running   0          21m
   ```
   {: screen}

## Using Knative services to deploy a serverless app
{: #knative-deploy-app}

After you set up Knative in your cluster, you can deploy your serverless app as a Knative service.
{: shortdesc}

**What is a Knative service?** </br>
To deploy an app with Knative, you must specify a Knative `Service` resource. A Knative service is managed by the Knative `Serving` primitive and is responsible to manage the entire lifecycle of the workload. When you create the service, the Knative `Serving` primitive automatically creates a version for your serverless app and adds this version to the revision history of the service. Your serverless app is assigned a public URL from your Ingress subdomain in the format `<knative_service_name>-<namespace>.<ingress_subdomain>` that you can use to access the app from the internet. In addition, a private host name is assigned to your app in the format `<knative_service_name>.<namespace>.cluster.local` that you can use to access your app from within the cluster.

**What happens behind the scenes when I create the Knative service?**</br>
When you create a Knative service, your app is automatically deployed as a Kubernetes pod in your cluster and exposed by using a Kubernetes service. To assign the public host name, Knative uses the IBM-provided Ingress subdomain and TLS certificate. Incoming network traffic is routed based on the default IBM-provided Ingress routing rules.

**How can I roll out a new version of my app?**</br>
When you update your Knative service, a new version of your serverless app is created. This version is assigned the same public and private host names as your previous version. By default, all incoming network traffic is routed to the latest version of your app. However, you can also specify the percentage of incoming network traffic that you want to route to a specific app version so that you can do A-B testing. You can split incoming network traffic between two app versions at a time, the current version of your app and the new version that you want to roll over to.  

**Can I bring my own custom domain and TLS certificate?** </br>
You can change the configmap of your Istio Ingress gateway and the Ingress routing rules to use your custom domain name and TLS certificate when you assign a host name to your serverless app. For more information, see [Setting up custom domain names and certificates](#knative-custom-domain-tls).

To deploy your serverless app as a Knative service:

1. Create a YAML file for your first serverless [`Hello World`](https://hub.docker.com/r/ibmcom/kn-helloworld) app in Go with Knative. When you send a request to your sample app, the app reads the environment variable `TARGET` and prints `"Hello ${TARGET}!"`. If this environment variable is empty, `"Hello World!"` is returned.
   ```
   apiVersion: serving.knative.dev/v1alpha1
   kind: Service
   metadata:
     name: kn-helloworld
     namespace: default
   spec:
     template:
       spec:
         containers:
         - image: docker.io/ibmcom/kn-helloworld
           env:
           - name: TARGET
             value: "Go Sample v1"
    ```
    {: codeblock}

    <table>
    <caption>Understanding the YAML file components</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the YAML file components</th>
    </thead>
    <tbody>
    <tr>
    <td><code>metadata.name</code></td>
    <td>The name of your Knative service.</td>
    </tr>
    <tr>
    <td><code>metadata.namespace</td>
    <td>Optional: The Kubernetes namespace where you want to deploy your app as a Knative service. By default, all services are deployed to the <code>default</code> Kubernetes namespace. </td>
    </tr>
    <tr>
    <td><code>spec.template.spec.containers.image</code></td>
    <td>The URL to the container registry where your image is stored. In this example, you deploy a Knative Hello World app that is stored in the <code>ibmcom</code> namespace in Docker Hub. </td>
    </tr>
    <tr>
    <td><code>spec.template.spec.containers.env</code></td>
    <td>Optional: A list of environment variables that you want your Knative service to have. In this example, the value of the environment variable <code>TARGET</code> is read by the sample app and returned when you send a request to your app in the format <code>"Hello ${TARGET}!"</code>. If no value is provided, the sample app returns <code>"Hello World!"</code>.  </td>
    </tr>
    </tbody>
    </table>

2. Create the Knative service in your cluster.
   ```
   kubectl apply -f service.yaml
   ```
   {: pre}

   Example output:
   ```
   service.serving.knative.dev "kn-helloworld" created
   ```
   {: screen}

3. Verify that your Knative service is created. In your CLI output, you can view the public **DOMAIN** that is assigned to your serverless app. The **LATESTCREATED** and **LATESTREADY** columns show the version of your app that was last created and that is currently deployed in the format `<knative_service_name>-<version>`. The version that is assigned to your app is a random string value. In this example, the version of your serverless app is `rjmwt`. When you update the service, a new version of your app is created and assigned a new random string for the version.  
   ```
   kubectl get ksvc/kn-helloworld
   ```
   {: pre}

   Example output:
   ```
   NAME            DOMAIN                                                                LATESTCREATED         LATESTREADY           READY   REASON
   kn-helloworld   kn-helloworld-default.mycluster.us-south.containers.appdomain.cloud   kn-helloworld-rjmwt   kn-helloworld-rjmwt   True
   ```
   {: screen}

4. Try out your `Hello World` app by sending a request to the public URL that is assigned to your app.
   ```
   curl -v <public_app_url>
   ```
   {: pre}

   Example output:
   ```
   * Rebuilt URL to: kn-helloworld-default.mycluster.us-south.containers.appdomain.cloud/
   *   Trying 169.46.XX.XX...
   * TCP_NODELAY set
   * Connected to kn-helloworld-default.mycluster.us-south.containers.appdomain.cloud (169.46.XX.XX) port 80 (#0)
   > GET / HTTP/1.1
   > Host: kn-helloworld-default.mycluster.us-south.containers.appdomain.cloud
   > User-Agent: curl/7.54.0
   > Accept: */*
   >
   < HTTP/1.1 200 OK
   < Date: Thu, 21 Mar 2019 01:12:48 GMT
   < Content-Type: text/plain; charset=utf-8
   < Content-Length: 20
   < Connection: keep-alive
   < x-envoy-upstream-service-time: 17
   <
   Hello Go Sample v1!
   * Connection #0 to host kn-helloworld-default.mycluster.us-south.containers.appdomain.cloud left intact
   ```
   {: screen}

5. List the number of pods that were created for your Knative service. In the example in this topic, one pod is deployed that consists of two containers. One container runs your `Hello World` app and the other container is a side car that runs Istio and Knative monitoring and logging tools.
   ```
   kubectl get pods
   ```
   {: pre}

   Example output:
   ```
   NAME                                             READY     STATUS    RESTARTS   AGE
   kn-helloworld-rjmwt-deployment-55db6bf4c5-2vftm  2/2      Running   0          16s
   ```
   {: screen}

6. Wait a few minutes to let Knative scale down your pod. Knative evaluates the number of pods that must be up at a time to process incoming workload. If no network traffic is received, Knative automatically scales down your pods, even to zero pods as shown in this example.

   Want to see how Knative scales up your pods? Try to increase workload for your app, for example by using tools like the [Simple cloud-based load tester](https://loader.io/).
   {: tip}
   ```
   kubectl get pods
   ```
   {: pre}

   If you do not see any `kn-helloworld` pods, Knative scaled down your app to zero pods.

7. Update your Knative service sample and enter a different value for the `TARGET` environment variable.

   Example service YAML:
   ```
   apiVersion: serving.knative.dev/v1alpha1
   kind: Service
   metadata:
     name: kn-helloworld
     namespace: default
   spec:
     template:
       spec:
         containers:
         - image: docker.io/ibmcom/kn-helloworld
           env:
           - name: TARGET
             value: "Mr. Smith"
   ```
   {: codeblock}

8. Apply the change to your service. When you change the configuration, Knative automatically creates a new version for your app.
   ```
   kubectl apply -f service.yaml
   ```
   {: pre}

9. Verify that a new version of your app is deployed. In your CLI output, you can see the new version of your app in the **LATESTCREATED** column. When you see the same app version in the **LATESTREADY** column, your app is all set up and ready to receive incoming network traffic on the assigned public URL.
   ```
   kubectl get ksvc/kn-helloworld
   ```
   {: pre}

   Example output:
   ```
   NAME            DOMAIN                                                                LATESTCREATED         LATESTREADY           READY   REASON
   kn-helloworld   kn-helloworld-default.mycluster.us-south.containers.appdomain.cloud   kn-helloworld-ghyei   kn-helloworld-ghyei   True
   ```
   {: screen}

9. Make a new request to your app to verify that your change was applied.
   ```
   curl -v <service_domain>
   ```

   Example output:
   ```
   ...
   Hello Mr. Smith!
   ```
   {: screen}

10. Verify that Knative scaled up your pod again to account for the increased network traffic.
    ```
    kubectl get pods
    ```

    Example output:
    ```
    NAME                                              READY     STATUS    RESTARTS   AGE
    kn-helloworld-ghyei-deployment-55db6bf4c5-2vftm   2/2       Running   0          16s
    ```
    {: screen}

11. Optional: Clean up your Knative service.
    ```
    kubectl delete -f service.yaml
    ```
    {: pre}


## Setting up custom domain names and certificates
{: #knative-custom-domain-tls}

You can configure Knative to assign host names from your own custom domain that you configured with TLS.
{: shortdesc}

By default, every app is assigned a public subdomain from your Ingress subdomain in the format `<knative_service_name>-<namespace>.<ingress_subdomain>` that you can use to access the app from the Internet. In addition, a private host name is assigned to your app in the format `<knative_service_name>.<namespace>.cluster.local` that you can use to access your app from within the cluster. If you want to assign host names from a custom domain that you own, you can change the Knative configmap to use the custom domain instead.

1. Create a custom domain. To register your custom domain, work with your Domain Name Service (DNS) provider or [IBM Cloud DNS](/docs/infrastructure/dns?topic=dns-getting-started).
2. Configure your domain to route incoming network traffic to the IBM-provided Ingress gateway. Choose between these options:
   - Define an alias for your custom domain by specifying the IBM-provided domain as a Canonical Name record (CNAME). To find the IBM-provided Ingress domain, run `ibmcloud ks cluster-get --cluster <cluster_name>` and look for the **Ingress subdomain** field. Using a CNAME is preferred because IBM provides automatic health checks on the IBM subdomain and removes any failing IPs from the DNS response.
   - Map your custom domain to the portable public IP address of the Ingress gateway by adding the IP address as an A record. To find the public IP address of the Ingress gateway, run `nslookup <ingress_subdomain>`.
3. Purchase an official wildcard TLS certificate for your custom domain. If you want to purchase multiple TLS certificates, make sure the [CN ![External link icon](../icons/launch-glyph.svg "External link icon")](https://support.dnsimple.com/articles/what-is-common-name/) is different for each certificate.
4. Create a Kubernetes secret for your cert and key.
   1. Encode the cert and key into base-64 and save the base-64 encoded value in a new file.
      ```
      openssl base64 -in tls.key -out tls.key.base64
      ```
      {: pre}

   2. View the base-64 encoded value for your cert and key.
      ```
      cat tls.key.base64
      ```
      {: pre}

   3. Create a secret YAML file by using the cert and key.
      ```
      apiVersion: v1
      kind: Secret
      metadata:
        name: mydomain-ssl
      type: Opaque
      data:
        tls.crt: <client_certificate>
        tls.key: <client_key>
      ```
      {: codeblock}

   4. Create the certificate in your cluster.
      ```
      kubectl apply -f secret.yaml
      ```
      {: pre}

5. Open the `iks-knative-ingress` Ingress resource in the `istio-system` namespace of your cluster to start editing it.
   ```
   kubectl edit ingress iks-knative-ingress -n istio-system
   ```
   {: pre}

6. Change the default routing rules for your Ingress.
   - Add your custom wildcard domain to the `spec.rules.host` section so that all incoming network traffic from your custom domain and any subdomain is routed to the `istio-ingressgateway`.
   - Configure all hosts of your custom wildcard domain to use the TLS secret that you created earlier in the `spec.tls.hosts` section.

   Example Ingress:
   ```
   apiVersion: extensions/v1beta1
   kind: Ingress
   metadata:
     name: iks-knative-ingress
     namespace: istio-system
   spec:
     rules:
     - host: '*.mydomain'
       http:
         paths:
         - backend:
             serviceName: istio-ingressgateway
             servicePort: 80
           path: /
     tls:
     - hosts:
       - '*.mydomain'
       secretName: mydomain-ssl
   ```
   {: codeblock}

   The `spec.rules.host` and `spec.tls.hosts` sections are lists and can include multiple custom domains and TLS certificates.
   {: tip}

7. Modify the Knative `config-domain` configmap to use your custom domain to assign host names to new Knative services.
   1. Open the `config-domain` configmap to start editing it.
      ```
      kubectl edit configmap config-domain -n knative-serving
      ```
      {: pre}

   2. Specify your custom domain in the `data` section of your configmap and remove the default domain that is set for your cluster.
      - **Example to assign a host name from your custom domain for all Knative services**:
        ```
        apiVersion: v1
        kind: ConfigMap
        data:
          <custom_domain>: ""
        metadata:
          name: config-domain
          namespace: knative-serving
        ```
        {: codeblock}

        By adding `""` to your custom domain, all Knative services that you create are assigned a host name from your custom domain.  

      - **Example to assign a host name from your custom domain for select Knative services**:
        ```
        apiVersion: v1
        kind: ConfigMap
        data:
          <custom_domain>: |
            selector:
              app: sample
          mycluster.us-south.containers.appdomain.cloud: ""
        metadata:
          name: config-domain
          namespace: knative-serving
        ```
        {: codeblock}

        To assign a host name from your custom domain for select Knative services only, add a `data.selector` label key and value to your configmap. In this example, all services with the label `app: sample` are assigned a host name from your custom domain. Make sure to also have a domain name that you want to assign to all other apps that do not have the `app: sample` label. In this example, the default IBM-provided domain `mycluster.us-south.containers.appdomain.cloud` is used.
    3. Save your changes.

With your Ingress routing rules and Knative configmaps all set up, you can create Knative services with your custom domain and TLS certificate.

## Accessing a Knative service from another Knative service
{: #knative-access-service}

You can access your Knative service from another Knative service by using a REST API call to the URL that is assigned to your Knative service.
{: shortdesc}

1. List all Knative services in your cluster.
   ```
   kubectl get ksvc --all-namespaces
   ```
   {: pre}

2. Retrieve the **DOMAIN** that is assigned to your Knative service.
   ```
   kubect get ksvc/<knative_service_name>
   ```
   {: pre}

   Example output:
   ```
   NAME        DOMAIN                                                            LATESTCREATED     LATESTREADY       READY   REASON
   myservice   myservice-default.mycluster.us-south.containers.appdomain.cloud   myservice-rjmwt   myservice-rjmwt   True
   ```
   {: screen}

3. Use the domain name to implement a REST API call to access your Knative service. This REST API call must be part of the app for which you create a Knative service. If the Knative service that you want to access is assigned a local URL in the format `<service_name>-<namespace>.svc.cluster.local`, Knative keeps the REST API request within the cluster-internal network.

   Example code snippet in Go:
   ```go
   resp, err := http.Get("<knative_service_domain_name>")
   if err != nil {
       fmt.Fprintf(os.Stderr, "Error: %s\n", err)
   } else if resp.Body != nil {
       body, _ := ioutil.ReadAll(resp.Body)
       fmt.Printf("Response: %s\n", string(body))
   }
   ```
   {: codeblock}

## Common Knative service settings
{: #knative-service-settings}

Review common Knative service settings that you might find useful as you develop your serverless app.
{: shortdesc}

- [Setting minimum and maximum number of pods](#knative-min-max-pods)
- [Specifying the maximum number of requests per pod](#max-request-per-pod)
- [Creating private-only serverless apps](#knative-private-only)
- [Forcing the Knative service to repull a container image](#knative-repull-image)

### Setting minimum and maximum number of pods
{: #knative-min-max-pods}

You can specify the minimum and maximum number of pods that you want to run for your apps by using an annotation. For example, if you don't want Knative to scale down your app to zero instances, set the minimum number of pods to 1.
{: shortdesc}

```
apiVersion: serving.knative.dev/v1alpha1
kind: Service
metadata:
  name: ...
spec:
  template:
    metadata:
      annotations:
        autoscaling.knative.dev/minScale: "1"
        autoscaling.knative.dev/maxScale: "100"
    spec:
      containers:
      - image:
...
```
{: codeblock}

<table>
<caption>Understanding the YAML file components</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the YAML file components</th>
</thead>
<tbody>
<tr>
<td><code>autoscaling.knative.dev/minScale</code></td>
<td>Enter the minimum number of pods that you want to run in your cluster. Knative cannot scale down your app lower than the number that you set, even if no network traffic is received by your app. The default number of pods is zero.  </td>
</tr>
<tr>
<td><code>autoscaling.knative.dev/maxScale</code></td>
<td>Enter the maximum number of pods that you want to run in your cluster. Knative cannot scale up your app higher than the number that you set, even if you have more requests that your current app instances can handle.</td>
</tr>
</tbody>
</table>

### Specifying the maximum number of requests per pod
{: #max-request-per-pod}

You can specify the maximum number of requests that an app instance can receive and process before Knative considers to scale up your app instances. For example, if you set the maximum number of requests to 1, then your app instance can receive one request at a time. If a second request arrives before the first one is fully processed, Knative scales up another instance.

```
apiVersion: serving.knative.dev/v1alpha1
kind: Service
metadata:
  name: myservice
spec:
  template:
    spec:
      containers:
      - image: myimage
      containerConcurrency: 1
....
```
{: codeblock}

<table>
<caption>Understanding the YAML file components</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the YAML file components</th>
</thead>
<tbody>
<tr>
<td><code>containerConcurrency</code></td>
<td>Enter the maximum number of requests an app instance can receive at a time before Knative considers to scale up your app instances.  </td>
</tr>
</tbody>
</table>

### Creating private-only serverless apps
{: #knative-private-only}

By default, every Knative service is assigned a public route from your Istio Ingress subdomain and a private route in the format `<service_name>.<namespace>.cluster.local`. You can use the public route to access your app from the public network. If you want to keep your service private, you can add the `serving.knative.dev/visibility` label to your Knative service. This label instructs Knative to assign a private host name to your service only.
{: shortdesc}

```
apiVersion: serving.knative.dev/v1alpha1
kind: Service
metadata:
  name: myservice
  labels:
    serving.knative.dev/visibility: cluster-local
spec:
  template:
    spec:
      containers:
      - image:
...
```
{: codeblock}

<table>
<caption>Understanding the YAML file components</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the YAML file components</th>
</thead>
<tbody>
<tr>
<td><code>serving.knative.dev/visibility</code></td>
  <td>If you add the <code>serving.knative.dev/visibility: cluster-local</code> label, your service is assigned only a private route in the format <code>&lt;service_name&gt;.&lt;namespace&gt;.cluster.local</code>. You can use the private host name to access your service from within the cluster, but you cannot access your service from the public network.  </td>
</tr>
</tbody>
</table>

### Forcing the Knative service to repull a container image
{: #knative-repull-image}

The current implementation of Knative does not provide a standard way to force your Knative `Serving` component to repull a container image. To repull an image from your registry, choose between the following options:

- **Modify the Knative service `template`**: The `template` of a Knative service is used to create a revision of your Knative service. If you modify this template and for example, add the `repullFlag` annotation, Knative must create a new revision for your app. As part of creating the revision, Knative must check for container image updates. When you set `imagePullPolicy: Always`, Knative cannot use the image cache in the cluster, but instead must pull the image from your container registry.
   ```
   apiVersion: serving.knative.dev/v1alpha1
   kind: Service
   metadata:
     name: <service_name>
   spec:
     template:
       metadata:
         annotations:
           repullFlag: 123
       spec:
         containers:
         - image: <image_name>
           imagePullPolicy: Always
    ```
    {: codeblock}

    You must change the `repullFlag` value every time that you want to create a new revision of your service that pulls the latest image version from your container registry. Make sure that you use a unique value for each revision to avoid that Knative uses an old image version because of two identical Knative service configurations.  
    {: note}

- **Use tags to create unique container images**: You can use unique tags for every container image that you create and reference this image in your Knative service `container.image` configuration. In the following example, `v1` is used as the image tag. To force Knative to pull a new image from your container registry, you must change the image tag. For example, use `v2` as your new image tag.
  ```
  apiVersion: serving.knative.dev/v1alpha1
  kind: Service
  metadata:
    name: <service_name>
  spec:
    template:
      spec:
        containers:
        - image: myapp:v1
          imagePullPolicy: Always
    ```
    {: codeblock}


## Related links  
{: #knative-related-links}

- Try out this [Knative workshop ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/IBM/knative101/tree/master/workshop) to deploy your first `Node.js` fibonacci app to your cluster.
  - Explore how to use the Knative `Build` primitive to build an image from a Dockerfile in GitHub and automatically push the image to your namespace in {{site.data.keyword.registrylong_notm}}.  
  - Learn how you can set up routing for network traffic from the IBM-provided Ingress subdomain to the Istio Ingress gateway that is provided by Knative.
  - Roll out a new version of your app and use Istio to control the amount of traffic that is routed to each app version.
- Explore [Knative `Eventing` ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/knative/docs/tree/master/eventing/samples) samples.
- Learn more about Knative with the [Knative documentation ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/knative/docs).
