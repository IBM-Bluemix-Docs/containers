---

copyright:
  years: 2014, 2018
lastupdated: "2018-01-24"

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

You can expose your app as a Kubernetes NodePort service for free or standard clusters.
{:shortdesc}

**Note:** The public IP address of a worker node is not permanent. If the worker node must be re-created, a new public IP address is assigned to the worker node. If you need a stable public IP address and more availability for your service, expose your app by using a [LoadBalancer service](cs_loadbalancer.html) or [Ingress](cs_ingress.html).

If you do not already have an app ready, you can use a Kubernetes example app called [Guestbook ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/kubernetes/kubernetes/blob/master/examples/guestbook/all-in-one/guestbook-all-in-one.yaml).

1.  In the configuration file for you app, define a [service ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/concepts/services-networking/service/) section.

    Example:

    ```
    apiVersion: v1
    kind: Service
    metadata:
      name: <my-nodeport-service>
      labels:
        run: <my-demo>
    spec:
      selector:
        run: <my-demo>
      type: NodePort
      ports:
       - port: <8081>
         # nodePort: <31514>

    ```
    {: codeblock}

    <table>
    <caption>Understanding this YAML file's components</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the NodePort service section components</th>
    </thead>
    <tbody>
    <tr>
    <td><code>name</code></td>
    <td>Replace <code><em>&lt;my-nodeport-service&gt;</em></code> with a name for your NodePort service.</td>
    </tr>
    <tr>
    <td><code>run</code></td>
    <td>Replace <code><em>&lt;my-demo&gt;</em></code> with the name of your deployment.</td>
    </tr>
    <tr>
    <td><code>port</code></td>
    <td>Replace <code><em>&lt;8081&gt;</em></code> with the port that you service listens on. </td>
     </tr>
     <tr>
     <td><code>nodePort</code></td>
     <td>Optional: Replace <code><em>&lt;31514&gt;</em></code> with a NodePort in the 30000 - 32767 range. Do not specify a NodePort that is already in use by another service. If no NodePort is assigned, a random one is assigned for you.<br><br>If you want to specify a NodePort and want to see which NodePorts are already in use, you can run the following command: <pre class="pre"><code>kubectl get svc</code></pre>Any NodePorts in use appear under the **Ports** field.</td>
     </tr>
     </tbody></table>


    * For the Guestbook example, a front-end service section already exists in the configuration file. To make the Guestbook app available externally, add the NodePort type and a NodePort in the range 30000 - 32767 to the front-end service section.

      ```
      apiVersion: v1
      kind: Service
      metadata:
        name: frontend
        labels:
          app: guestbook
          tier: frontend
      spec:
        type: NodePort
        ports:
        - port: 80
          nodePort: 31513
        selector:
          app: guestbook
          tier: frontend
      ```
      {: codeblock}

2.  Save the updated configuration file.

3.  Repeat these steps to create a NodePort service for each app that you want to expose to the internet.

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
