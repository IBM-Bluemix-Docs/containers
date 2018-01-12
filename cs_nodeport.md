---

copyright:
  years: 2014, 2018
lastupdated: "2018-01-11"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}


# Setting up NodePort services
{: #nodeport}

Make your app available to internet access by using the public IP address of any worker node in a cluster and exposing a node port. Use this option for testing and short-term public access.
{:shortdesc}

## Configuring public access to an app by using the NodePort service type
{: #config}

You can expose your app as a Kubernetes NodePort service for lite or standard clusters.
{:shortdesc}

**Note:** The public IP address of a worker node is not permanent. If the worker node must be re-created, a new public IP address is assigned to the worker node. If you need a stable public IP address and more availability for your service, expose your app by using a [LoadBalancer service](cs_nodeport.html#config) or [Ingress](cs_ingress.html#config).




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
    prod-dal10-pa215dcf5bbc0844a990fa6b0fcdbff286-w1  192.0.2.23  10.100.10.10  u2c.2x4  normal   Ready
    prod-dal10-pa215dcf5bbc0844a990fa6b0fcdbff286-w2  192.0.2.27  10.100.10.15  u2c.2x4  normal   Ready
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
