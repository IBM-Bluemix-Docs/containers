---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-05"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}


# Tutorial: Installing Istio on {{site.data.keyword.containerlong_notm}}
{: #istio_tutorial}

[Istio](https://www.ibm.com/cloud/info/istio) is an open platform to connect, secure, and manage a network of microservices, also known as a service mesh, on cloud platforms such as Kubernetes. With Istio, manage network traffic, load balance across microservices, enforce access policies, verify service identity on the service mesh, and more.
{:shortdesc}

In this tutorial, you can see how to install Istio alongside four microservices for a simple mock bookstore app called BookInfo. The microservices include a product web page, book details, reviews, and ratings. When you deploy BookInfo's microservices into an {{site.data.keyword.containershort}} cluster where Istio is installed, you inject the Istio Envoy sidecar proxies in the pods of each microservice.

**Note**: Some configurations and features of the Istio platform are still under development and are subject to change based on user feedback. Allow a few months for stablilization before you use Istio in production. 

## Objectives

-   Download and install Istio in your cluster
-   Deploy the BookInfo sample app
-   Inject Envoy sidecar proxies into the pods of the app's four microservices to connect the microservices in the service mesh
-   Verify the BookInfo app deployment and round robin through the three versions of the ratings service

## Time required

30 minutes

## Audience

This tutorial is intended for software developers and network administrators who have never used Istio before.

## Prerequisites

-  [Install the CLI](cs_cli_install.html#cs_cli_install_steps)
-  [Create a cluster](cs_clusters.html#clusters_cli)
-  [Target the CLI to your cluster](cs_cli_install.html#cs_cli_configure)

## Lesson 1: Download and install Istio
{: #istio_tutorial1}

Download and install Istio in your cluster.
{:shortdesc}

1. Either download Istio directly from [https://github.com/istio/istio/releases ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/istio/istio/releases) or get the latest version by using curl:

   ```
   curl -L https://git.io/getLatestIstio | sh -
   ```
   {: pre}

2. Extract the installation files.

3. Add the `istioctl` client to your PATH. For example, run the following command on a MacOS or Linux system:

   ```
   export PATH=$PWD/istio-0.4.0/bin:$PATH
   ```
   {: pre}

4. Change the directory to the Istio file location.

   ```
   cd <path_to_istio-0.4.0>
   ```
   {: pre}

5. Install Istio on the Kubernetes cluster. Istio is deployed in the Kubernetes namespace `istio-system`.

   ```
   kubectl apply -f install/kubernetes/istio.yaml
   ```
   {: pre}

   **Note**: If you need to enable mutual TLS authentication between sidecars, you can install the `istio-auth` file instead: `kubectl apply -f install/kubernetes/istio-auth.yaml`

6. Ensure that the Kubernetes services `istio-pilot`, `istio-mixer`, and `istio-ingress` are fully deployed before you continue.

   ```
   kubectl get svc -n istio-system
   ```
   {: pre}

   ```
   NAME            TYPE           CLUSTER-IP       EXTERNAL-IP      PORT(S)                                                            AGE
   istio-ingress   LoadBalancer   172.21.121.139   169.48.221.218   80:31176/TCP,443:30288/TCP                                         2m
   istio-mixer     ClusterIP      172.21.31.30     <none>           9091/TCP,15004/TCP,9093/TCP,9094/TCP,9102/TCP,9125/UDP,42422/TCP   2m
   istio-pilot     ClusterIP      172.21.97.191    <none>           15003/TCP,443/TCP                                                  2m
   ```
   {: screen}

7. Ensure the corresponding pods `istio-pilot-*`, `istio-mixer-*`, `istio-ingress-*`, and `istio-ca-*` are also fully deployed before you continue.

   ```
   kubectl get pods -n istio-system
   ```
   {: pre}

   ```
   istio-ca-3657790228-j21b9           1/1       Running   0          5m
   istio-ingress-1842462111-j3vcs      1/1       Running   0          5m
   istio-pilot-2275554717-93c43        1/1       Running   0          5m
   istio-mixer-2104784889-20rm8        2/2       Running   0          5m
   ```
   {: screen}


Congratulations! You successfully installed Istio into your cluster. Next, deploy the BookInfo sample app into your cluster.


## Lesson 2: Deploy the BookInfo app
{: #istio_tutorial2}

Deploy the BookInfo sample app's microservices to your Kubernetes cluster. 
{:shortdesc}

These four microservices include a product web page, book details, reviews (with several versions of the review microservice), and ratings. You can find all files that are used in this example in your Istio installation's `samples/bookinfo` directory.

When you deploy BookInfo, Envoy sidecar proxies are injected as containers into your app microservices' pods before the microservice pods are deployed. Istio uses an extended version of the Envoy proxy to mediate all inbound and outbound traffic for all microservices in the service mesh. For more about Envoy, see the [Istio documentation ![External link icon](../icons/launch-glyph.svg "External link icon")](https://istio.io/docs/concepts/what-is-istio/overview.html#envoy).

1. Deploy the BookInfo app. The `kube-inject` command adds Envoy to the `bookinfo.yaml` file and uses this updated file to deploy the app. When the app microservices deploy, the Envoy sidecar is also deployed in each microservice pod.

   ```
   kubectl apply -f <(istioctl kube-inject -f samples/bookinfo/kube/bookinfo.yaml)
   ```
   {: pre}

2. Ensure that the microservices and their corresponding pods are deployed:

   ```
   kubectl get svc
   ```
   {: pre}

   ```
   NAME                       CLUSTER-IP   EXTERNAL-IP   PORT(S)              AGE
   details                    10.0.0.31    <none>        9080/TCP             6m
   kubernetes                 10.0.0.1     <none>        443/TCP              30m
   productpage                10.0.0.120   <none>        9080/TCP             6m
   ratings                    10.0.0.15    <none>        9080/TCP             6m
   reviews                    10.0.0.170   <none>        9080/TCP             6m
   ```
   {: screen}

   ```
   kubectl get pods
   ```
   {: pre}

   ```
   NAME                                        READY     STATUS    RESTARTS   AGE
   details-v1-1520924117-48z17                 2/2       Running   0          6m
   productpage-v1-560495357-jk1lz              2/2       Running   0          6m
   ratings-v1-734492171-rnr5l                  2/2       Running   0          6m
   reviews-v1-874083890-f0qf0                  2/2       Running   0          6m
   reviews-v2-1343845940-b34q5                 2/2       Running   0          6m
   reviews-v3-1813607990-8ch52                 2/2       Running   0          6m
   ```
   {: screen}

3. To verify the application deployment, get the public address for your cluster.

    * If you are working with a standard cluster, run the following command to get the Ingress IP and port of your cluster:

       ```
       kubectl get ingress
       ```
       {: pre}

       The output looks like the following:

       ```
       NAME      HOSTS     ADDRESS          PORTS     AGE
       gateway   *         169.48.221.218   80        3m
       ```
       {: screen}

       The resulting Ingress address for this example is `169.48.221.218:80`. Export the address as the gateway URL with the following command. You will use the gateway URL in the next step to access the BookInfo product page.

       ```
       export GATEWAY_URL=169.48.221.218:80
       ```
       {: pre}

    * If you are working with a free cluster, you must use the public IP of the worker node and the NodePort. Run the following command to get the public IP of the worker node:

       ```
       bx cs workers <cluster_name_or_ID>
       ```
       {: pre}

       Export the public IP of the worker node as the gateway URL with the following command. You will use the gateway URL in the next step to access the BookInfo product page.

       ```
       export GATEWAY_URL=<worker_node_public_IP>:$(kubectl get svc istio-ingress -n istio-system -o jsonpath='{.spec.ports[0].nodePort}')
       ```
       {: pre}

4. Curl the `GATEWAY_URL` variable to check that BookInfo is running. A `200` response means that BookInfo is running properly with Istio.

   ```
   curl -I http://$GATEWAY_URL/productpage
   ```
   {: pre}

5. In a browser, navigate to `http://$GATEWAY_URL/productpage` to view the BookInfo web page.

6. Try refreshing the page several times. Different versions of the reviews section round robin through red stars, black stars, and no stars.

Congratulations! You successfully deployed the BookInfo sample app with Istio Envoy sidecars. Next, you can clean up your resources or continue on with more tutorials to explore Istio functionality further.

## Cleanup
{: #istio_tutorial_cleanup}

If you don't want to explore more Istio functionality that is provided in [What's next?](#istio_tutorial_whatsnext), then you can clean up your Istio resources in your cluster.
{:shortdesc}

1. Delete all BookInfo services, pods, and deployments in the cluster.

   ```
   samples/bookinfo/kube/cleanup.sh
   ```
   {: pre}

2. Uninstall Istio.

   ```
   kubectl delete -f install/kubernetes/istio.yaml
   ```
   {: pre}

## What's next?
{: #istio_tutorial_whatsnext}

To explore Istio functionality further, you can find more guides in the [Istio documentation ![External link icon](../icons/launch-glyph.svg "External link icon")](https://istio.io/).

* [Intelligent Routing ![External link icon](../icons/launch-glyph.svg "External link icon")](https://istio.io/docs/guides/intelligent-routing.html): This example shows how to route traffic to a specific version of BookInfo's reviews and ratings microservices by using Istio's traffic management capabilities.

* [In-Depth Telemetry ![External link icon](../icons/launch-glyph.svg "External link icon")](https://istio.io/docs/guides/telemetry.html): This example shows how to get uniform metrics, logs, and traces across BookInfo's microservices by using Istio Mixer and the Envoy proxy.
