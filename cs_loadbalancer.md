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



# Exposing apps with LoadBalancers
{: #loadbalancer}

Expose a port and use a portable IP address for the load balancer to access a containerized app.
{:shortdesc}

## Managing network traffic by using LoadBalancers
{: #planning}

When you create a standard cluster, {{site.data.keyword.containershort_notm}} automatically requests five portable public and five portable private IP addresses and provisions them into your IBM Cloud infrastructure (SoftLayer) account during cluster creation. Two of the portable IP addresses, one public and one private, are used for [Ingress application load balancers](cs_ingress.html). Four portable public and four portable private IP addresses can be used to expose apps by creating a LoadBalancer service.

When you create a Kubernetes LoadBalancer service in a cluster on a public VLAN, an external load balancer is created. Your options for IP addresses when you create a LoadBalancer service are as follows:

- If your cluster is on a public VLAN, one of the four available portable public IP addresses is used.
- If your cluster is available on a private VLAN only, one of the four available portable private IP addresses is used.
- You can request a portable public or private IP address for a LoadBalancer service by adding an annotation to the configuration file: `service.kubernetes.io/ibm-load-balancer-cloud-provider-ip-type: <public_or_private>`.

The portable public IP address that is assigned to your LoadBalancer service is permanent and does not change when a worker node is removed or re-created. Therefore, the LoadBalancer service is more available than the NodePort service. Unlike with NodePort services, you can assign any port to your load balancer and are not bound to a certain port range. If you use a LoadBalancer service, a node port is also available on each IP address of any worker node. To block access to node port while you are using a LoadBalancer service, see [Blocking incoming traffic](cs_network_policy.html#block_ingress).

The LoadBalancer service serves as the external entry point for incoming requests for the app. To access the LoadBalancer service from the internet, use the public IP address of your load balancer and the assigned port in the format `<IP_address>:<port>`. The following diagram shows how a load balancer directs communication from the internet to an app:

<img src="images/cs_loadbalancer_planning.png" width="550" alt="Expose an app in {{site.data.keyword.containershort_notm}} by using a load balancer" style="width:550px; border-style: none"/>

1. A request is sent to your app by using the public IP address of your load balancer and the assigned port on the worker node.

2. The request is automatically forwarded to the load balancer service's internal cluster IP address and port. The internal cluster IP address is accessible inside the cluster only.

3. `kube-proxy` routes the request to the Kubernetes load balancer service for the app.

4. The request is forwarded to the private IP address of the pod where the app is deployed. If multiple app instances are deployed in the cluster, the load balancer routes the requests between the app pods.




<br />




## Enabling public or private access to an app by using a LoadBalancer service
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
          name: myloadbalancer
        spec:
          type: LoadBalancer
          selector:
            <selector_key>:<selector_value>
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
          name: myloadbalancer
          annotations:
            service.kubernetes.io/ibm-load-balancer-cloud-provider-ip-type: <public_or_private>
        spec:
          type: LoadBalancer
          selector:
            <selector_key>: <selector_value>
          ports:
           - protocol: TCP
             port: 8080
          loadBalancerIP: <IP_address>
        ```
        {: codeblock}

        <table>
        <thead>
        <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the YAML file components</th>
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
        <tr>
          <td>`service.kubernetes.io/ibm-load-balancer-cloud-provider-ip-type:`
          <td>Annotation to specify the type of LoadBalancer. Accepted values are `private` and `public`. If you are creating a public LoadBalancer in clusters on public VLANs, this annotation is not required.</td>
        </tr>
        <tr>
          <td><code>loadBalancerIP</code></td>
          <td>To create a private LoadBalancer or to use a specific portable IP address for a public LoadBalancer, replace <em>&lt;IP_address&gt;</em> with the IP address that you want to use. For more information, see the [Kubernetes documentation ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/concepts/services-networking/service/#type-loadbalancer).</td>
        </tr>
        </tbody></table>

    3.  Optional: Configure a firewall by specifying the `loadBalancerSourceRanges` in the **spec** section. For more information, see the [Kubernetes documentation ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/tasks/access-application-cluster/configure-cloud-provider-firewall/).

    4.  Create the service in your cluster.

        ```
        kubectl apply -f myloadbalancer.yaml
        ```
        {: pre}

        When your load balancer service is created, a portable IP address is automatically assigned to the load balancer. If no portable IP address is available, the load balancer service cannot be created.

3.  Verify that the load balancer service was created successfully.

    ```
    kubectl describe service myloadbalancer
    ```
    {: pre}

    **Note:** It might take a few minutes for the load balancer service to be created properly and for the app to be available.

    Example CLI output:

    ```
    Name:                   myloadbalancer
    Namespace:              default
    Labels:                 <none>
    Selector:               app=liberty
    Type:                   LoadBalancer
    Location:               dal10
    IP:                     172.21.xxx.xxx
    LoadBalancer Ingress:   169.xx.xxx.xxx
    Port:                   <unset> 8080/TCP
    NodePort:               <unset> 32040/TCP
    Endpoints:              172.30.xxx.xxx:8080
    Session Affinity:       None
    Events:
      FirstSeen	LastSeen	Count	From			SubObjectPath	Type	 Reason			          Message
      ---------	--------	-----	----			-------------	----	 ------			          -------
      10s		    10s		    1	    {service-controller }	  Normal CreatingLoadBalancer	Creating load balancer
      10s		    10s		    1	    {service-controller }		Normal CreatedLoadBalancer	Created load balancer
    ```
    {: screen}

    The **LoadBalancer Ingress** IP address is the portable IP address that was assigned to your load balancer service.

4.  If you created a public load balancer, access your app from the internet.
    1.  Open your preferred web browser.
    2.  Enter the portable public IP address of the load balancer and port.

        ```
        http://169.xx.xxx.xxx:8080
        ```
        {: codeblock}

5. If you choose to [preserve the source IP address of the incoming package![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/tutorials/services/source-ip/#source-ip-for-services-with-typeloadbalancer) and have edge nodes, private-only worker nodes, or multiple VLANs, ensure that app pods are included in load balancing by [adding node affinity and tolerations to app pods](#node_affinity_tolerations).

<br />


## Adding node affinity and tolerations to app pods for source IP
{: #node_affinity_tolerations}

Whenever you deploy app pods, load balancer service pods are also deployed to the worker nodes that the app pods are deployed to. However, some situations exist where the load balancer pods and app pods might not be scheduled onto the same worker node:
{: shortdesc}

* You have tainted edge nodes that only load balancer service pods can deploy to. App pods are not permitted to deploy to those nodes.
* Your cluster is connected to multiple public or private VLANs, and your app pods might deploy to worker nodes that are connected only to one VLAN. Load balancer service pods might not deploy to those worker nodes because the load balancer IP address is connected to a different VLAN than the worker nodes.

When a client request to your app is sent to your cluster, the request is routed to a pod for the Kubernetes load balancer service that exposes the app. If an app pod does not exist on the same worker node as the load balancer service pod, the load balancer forwards the request to a different worker node where an app pod is deployed. The source IP address of the package is changed to the public IP address of the worker node where the app pod is running.

If you want to preserve the original source IP address of the client request, you can [enable source IP ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/tutorials/services/source-ip/#source-ip-for-services-with-typeloadbalancer) for load balancer services. Preserving the client’s IP is useful, for example, when app servers have to apply security and access-control policies. After you enable the source IP, load balancer service pods must forward requests to app pods that are deployed to the same worker node only. To force your app to deploy to specific worker nodes that load balancer service pods can also deploy to, you must add affinity rules and tolerations to your app deployment.

### Adding edge node affinity rules and tolerations
{: #edge_nodes}

When you [label worker nodes as edge nodes](cs_edge.html#edge_nodes) and also [taint the edge nodes](cs_edge.html#edge_workloads), load balancer service pods deploy only to those edge nodes, and app pods cannot deploy to edge nodes. When source IP is enabled for the load balancer service, the load balancer pods on the edge nodes cannot forward incoming requests to your app pods on other worker nodes.
{:shortdesc}

To force your app pods to deploy to edge nodes, add an edge node [affinity rule ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/concepts/configuration/assign-pod-node/#node-affinity-beta-feature) and [toleration ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/#concepts) to the app deployment.

Example deployment yaml with edge node affinity and edge node toleration:

```
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: with-node-affinity
spec:
  template:
    spec:
      affinity:
        nodeAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
            nodeSelectorTerms:
            - matchExpressions:
              - key: dedicated
                operator: In
                values:
                - edge
      tolerations:
        - key: dedicated
          value: edge
...
```
{: codeblock}

Note that both the **affinity** and **tolerations** sections have `dedicated` as the `key` and `edge` as the `value`.

### Adding affinity rules for multiple public or private VLANs
{: #edge_nodes}

When your cluster is connected to multiple public or private VLANs, your app pods might deploy to worker nodes that are connected only to one VLAN. If the load balancer IP address is connected to a different VLAN than these worker nodes, load balancer service pods won't deploy to those worker nodes.
{:shortdesc}

When source IP is enabled, schedule app pods on worker nodes that are the same VLAN as the load balancer's IP address by adding an affinity rule to the app deployment.

Before you begin, [target your CLI](cs_cli_install.html#cs_cli_configure) to your cluster.

1. Get the IP address of the load balancer service that you want to use. Look for the IP address in the **LoadBalancer Ingress** field.
    ```
    kubectl describe service <loadbalancer_service_name>
    ```
    {: pre}

2. Retrieve the VLAN ID that your load balancer service is connected to.

    1. List portable public VLANs for your cluster.
        ```
        bx cs cluster-get <cluster_name_or_ID> --showResources
        ```
        {: pre}

        Example output:
        ```
        ...

        Subnet VLANs
        VLAN ID   Subnet CIDR       Public   User-managed
        2234947   10.xxx.xx.xxx/29  false    false
        2234945   169.36.5.xxx/29   true     false
        ```
        {: screen}

    2. In the output under **Subnet VLANs**, look for the subnet CIDR that matches the load balancer IP address that you retrieved earlier and note the VLAN ID.

        For example, if the load balancer service IP address is `169.36.5.xxx`, the matching subnet in the above example output is `169.36.5.xxx/29`. The VLAN ID that the subnet is connected to is `2234945`.

3. [Add an affinity rule ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/concepts/configuration/assign-pod-node/#node-affinity-beta-feature) to the app deployment for the VLAN ID that you noted in the previous step.

    For example, if you have multiple VLANs but want your app pods to deploy to worker nodes on the `2234945` public VLAN only:

    ```
    apiVersion: extensions/v1beta1
    kind: Deployment
    metadata:
      name: with-node-affinity
    spec:
      template:
        spec:
          affinity:
            nodeAffinity:
              requiredDuringSchedulingIgnoredDuringExecution:
                nodeSelectorTerms:
                - matchExpressions:
                  - key: publicVLAN
                    operator: In
                    values:
                    - "2234945"
    ...
    ```
    {: codeblock}

    In the above yaml, the **affinity** section has `publicVLAN` as the `key` and `"2234945"` as the `value`.

4. Apply the updated deployment configuration file.
    ```
    kubectl apply -f with-node-affinity.yaml
    ```
    {: pre}

5. Verify that the app pods deployed to worker nodes connected to the designated VLAN.

    1. List the pods in your cluster. Replace `<selector>` with the label that you used for the app.
        ```
        kubectl get pods -o wide app=<selector>
        ```
        {: pre}

        Example output:
        ```
        NAME                   READY     STATUS              RESTARTS   AGE       IP               NODE
        cf-py-d7b7d94db-vp8pq  1/1       Running             0          15d       172.30.xxx.xxx   10.176.48.78
        ```
        {: screen}

    2. In the output, identify a pod for your app. Note the **NODE** ID of the worker node that the pod is on.

        In the above example output, the app pod `cf-py-d7b7d94db-vp8pq` is on worker node `10.176.48.78`.

    3. List the details for the worker node.

        ```
        kubectl describe node <worker_node_ID>
        ```
        {: pre}

        Example output:

        ```
        Name:                   10.xxx.xx.xxx
        Role:
        Labels:                 arch=amd64
                                beta.kubernetes.io/arch=amd64
                                beta.kubernetes.io/os=linux
                                failure-domain.beta.kubernetes.io/region=us-south
                                failure-domain.beta.kubernetes.io/zone=dal10
                                ibm-cloud.kubernetes.io/encrypted-docker-data=true
                                kubernetes.io/hostname=10.xxx.xx.xxx
                                privateVLAN=2234945
                                publicVLAN=2234967
        ...
        ```
        {: screen}

    4. In the **Labels** section of the output, verify that the public or private VLAN is the VLAN that you designated in previous steps.

