---

copyright:
  years: 2014, 2018
lastupdated: "2018-03-02"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}


# Setting up Load Balancer services
{: #loadbalancer}

Expose a port and use a portable IP address for the load balancer to access a containerized app. Use a public IP address to make an app accessible on the internet, or a private IP address to make an app accessible on your private infrastructure network in {{site.data.keyword.containerlong}}.
{:shortdesc}

## Planning external networking with LoadBalancer services
{: #planning}

Expose a port and use the public or private IP address for the load balancer to access the app.
{:shortdesc}


When you create a standard cluster, {{site.data.keyword.containershort_notm}} automatically requests five portable public and five portable private IP addresses and provisions them into your IBM Cloud infrastructure (SoftLayer) account during cluster creation. Two of the portable IP addresses, one public and one private, are used for [Ingress application load balancers](cs_ingress.html#planning). Four portable public and four portable private IP addresses can be used to expose apps by creating a LoadBalancer service.

When you create a Kubernetes LoadBalancer service in a cluster on a public VLAN, an external load balancer is created. One of the four available public IP addresses is assigned to the load balancer. If no portable public IP address is available, the creation of your LoadBalancer service fails. The LoadBalancer service serves as the external entry point for incoming requests for the app. Unlike with NodePort services, you can assign any port to your load balancer and are not bound to a certain port range. The portable public IP address that is assigned to your LoadBalancer service is permanent and does not change when a worker node is removed or re-created. Therefore, the LoadBalancer service is more available than the NodePort service. To access the LoadBalancer service from the internet, use the public IP address of your load balancer and the assigned port in the format `<ip_address>:<port>`.

The following diagram shows how the LoadBalancer directs communication from the internet to an app:

![Expose a service by using a Kubernetes LoadBalancer service type](images/cs_loadbalancer.png)

As depicted in the diagram, when a request arrives at the LoadBalancer service, the request is automatically forwarded to the internal cluster IP address that is assigned to the LoadBalancer service during service creation. The cluster IP address is accessible inside the cluster only. From the cluster IP address, incoming requests are further forwarded to the `kube-proxy` component of your worker node. Then the requests are forwarded to the private IP address of the pod where the app is deployed. If you have multiple replicas of your app that are running in different pods, the `kube-proxy` component load balances incoming requests across all replicas.

If you use a LoadBalancer service, a node port is also available on each IP address of any worker node. To block access to node port while you are using a LoadBalancer service, see [Blocking incoming traffic](cs_network_policy.html#block_ingress).

Your options for IP addresses when you create a LoadBalancer service are as follows:

- If your cluster is on a public VLAN, a portable public IP address is used.
- If your cluster is available on a private VLAN only, then a portable private IP address is used.
- You can request a portable public or private IP address for a LoadBalancer service by adding an annotation to the configuration file: `service.kubernetes.io/ibm-load-balancer-cloud-provider-ip-type: <public_or_private>`.




<br />




## Configuring access to an app by using the load balancer service type
{: #config}

Before you begin:

-   This feature is available for standard clusters only.
-   You must have a portable public or private IP address available to assign to the load balancer service.
-   A load balancer service with a portable private IP address still has a public node port open on every worker node. To add a network policy to prevent public traffic, see [Blocking incoming traffic](cs_network_policy.html#block_ingress).

To create a load balancer service:

1.  [Deploy your app to the cluster](cs_app.html#app_cli). When you deploy your app to the cluster, one or more pods are created for you that run your app in a container. Ensure that you add a label to your deployment in the metadata section of your configuration file. This label is needed to identify all pods where your app is running so that they can be included in the load balancing.
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
          loadBalancerIP: <private_ip_address>
        ```
        {: codeblock}

        <table>
        <caption>Understanding the LoadBalancer service file components</caption>
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
        </tr>
        <tr>
          <td><code>loadBalancerIP</code></td>
          <td>When creating a private LoadBalancer or to use a a specific portable IP address for a public LoadBalancer, replace <em>&lt;loadBalancerIP&gt;</em> with the IP address that you want to use. For more information, see the [Kubernetes documentation ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/concepts/services-networking/service/#type-loadbalancer).</td>
        </tr>
        </tbody></table>

    3.  Optional: Configure a firewall by specifying the `loadBalancerSourceRanges` in the spec section. For more information, see the [Kubernetes documentation ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/tasks/access-application-cluster/configure-cloud-provider-firewall/).

    4.  Create the service in your cluster.

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
    Location:               dal10
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
