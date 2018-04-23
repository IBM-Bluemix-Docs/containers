---

copyright:
  years: 2014, 2018
lastupdated: "2018-4-23"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}





# Configuring subnets for clusters
{: #subnets}

Change the pool of available portable public or private IP addresses by adding subnets to your Kubernetes cluster in {{site.data.keyword.containerlong}}.
{:shortdesc}

In {{site.data.keyword.containershort_notm}}, you can add stable, portable IP addresses for Kubernetes services by adding network subnets to the cluster. In this case, subnets are not being used with netmasking to create connectivity across one or more clusters. Instead, the subnets are used to provide permanent fixed IP addresses for a service from a cluster that can be used to access that service.

<dl>
  <dt>Creating a cluster includes subnet creation by default</dt>
  <dd>When you create a standard cluster, {{site.data.keyword.containershort_notm}} automatically provisions the following subnets:
    <ul><li>A portable public subnet with 5 public IP addresses</li>
      <li>A portable private subnet with 5 private IP addresses </li></ul>
      Portable public and private IP addresses are static and do not change when a worker node is removed. For each subnet, one portable public and one portable private IP address is used for the [Ingress application load balancers](cs_ingress.html). You can use the Ingress application load balancer to expose multiple apps in your cluster. The remaining four portable public and four portable private IP addresses can be used to expose single apps to the public or private network by [creating a load balancer service](cs_loadbalancer.html).</dd>
  <dt>[Ordering and managing your own existing subnets](#custom)</dt>
  <dd>You can order and manage existing portable subnets in your IBM Cloud infrastructure (SoftLayer) account instead of using the automatically provisioned subnets. Use this option to retain stable static IP addresses across cluster removals and creations, or to order larger blocks of IP addresses. First, create a cluster without subnets by using the `cluster-create --no-subnet` command, and then add the subnet to the cluster with the `cluster-subnet-add` command. </dd>
</dl>

**Note:** Portable public IP addresses are charged monthly. If you remove portable public IP addresses after your cluster is provisioned, you still must pay the monthly charge, even if you used them only for a short amount of time.

## Requesting more subnets for your cluster
{: #request}

You can add stable, portable public, or private IP addresses to the cluster by assigning subnets to the cluster.
{:shortdesc}

**Note:** When you make a subnet available to a cluster, IP addresses of this subnet are used for cluster networking purposes. To avoid IP address conflicts, make sure that you use a subnet with one cluster only. Do not use a subnet for multiple clusters or for other purposes outside of {{site.data.keyword.containershort_notm}} at the same time.

Before you begin, [target your CLI](cs_cli_install.html#cs_cli_configure) to your cluster.

To create a subnet in an IBM Cloud infrastructure (SoftLayer) account and make it available to a specified cluster:

1. Provision a new subnet.

    ```
    bx cs cluster-subnet-create <cluster_name_or_id> <subnet_size> <VLAN_ID>
    ```
    {: pre}

    <table>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding this command's components</th>
    </thead>
    <tbody>
    <tr>
    <td><code>cluster-subnet-create</code></td>
    <td>The command to provision a subnet for your cluster.</td>
    </tr>
    <tr>
    <td><code><em>&lt;cluster_name_or_id&gt;</em></code></td>
    <td>Replace <code>&lt;cluster_name_or_id&gt;</code> with the name or ID of the cluster.</td>
    </tr>
    <tr>
    <td><code><em>&lt;subnet_size&gt;</em></code></td>
    <td>Replace <code>&lt;subnet_size&gt;</code> with the number of IP addresses that you want to add from your portable subnet. Accepted values are 8, 16, 32, or 64. <p>**Note:** When you add portable IP addresses for your subnet, three IP addresses are used to establish cluster-internal networking. You cannot use these three IP addresses for your application load balancer or to create a load balancer service. For example, if you request eight portable public IP addresses, you can use five of them to expose your apps to the public.</p> </td>
    </tr>
    <tr>
    <td><code><em>&lt;VLAN_ID&gt;</em></code></td>
    <td>Replace <code>&lt;VLAN_ID&gt;</code> with the ID of the public or private VLAN on which you want to allocate the portable public or private IP addresses. You must select the public or private VLAN that an existing worker node is connected to. To review the public or private VLAN for a worker node, run the <code>bx cs worker-get &lt;worker_id&gt;</code> command. </td>
    </tr>
    </tbody></table>

2.  Verify that the subnet was successfully created and added to your cluster. The subnet CIDR is listed in the **VLANs** section.

    ```
    bx cs cluster-get --showResources <cluster_name_or_ID>
    ```
    {: pre}

3. Optional: [Enable routing between subnets on the same VLAN](#vlan-spanning).

<br />


## Adding or reusing custom and existing subnets in Kubernetes clusters
{: #custom}

You can add existing portable public or private subnets to your Kubernetes cluster or reuse subnets from a deleted cluster.
{:shortdesc}

Before you begin,
- [Target your CLI](cs_cli_install.html#cs_cli_configure) to your cluster.
- To reuse subnets from a cluster that you no longer need, delete the unneeded cluster. The subnets are deleted within 24 hours.

   ```
   bx cs cluster-rm <cluster_name_or_ID
   ```
   {: pre}

To use an existing subnet in your IBM Cloud infrastructure (SoftLayer) portfolio with custom firewall rules or available IP addresses:

1.  Identify the subnet to use. Note the ID of the subnet and the VLAN ID. In this example, the subnet ID is `1602829` and the VLAN ID is `2234945`.

    ```
    bx cs subnets
    ```
    {: pre}

    ```
    Getting subnet list...
    OK
    ID        Network             Gateway          VLAN ID   Type      Bound Cluster
    1550165   10.xxx.xx.xxx/26    10.xxx.xx.xxx    2234947   private
    1602829   169.xx.xxx.xxx/28   169.xx.xxx.xxx   2234945   public

    ```
    {: screen}

2.  Confirm the location that the VLAN is located. In this example, the location is dal10.

    ```
    bx cs vlans dal10
    ```
    {: pre}

    ```
    Getting VLAN list...
    OK
    ID        Name   Number   Type      Router         Supports Virtual Workers
    2234947          1813     private   bcr01a.dal10   true
    2234945          1618     public    fcr01a.dal10   true
    ```
    {: screen}

3.  Create a cluster by using the location and VLAN ID that you identified. To reuse an existing subnet, include the `--no-subnet` flag to prevent a new portable public IP subnet and a new portable private IP subnet from being created automatically.

    ```
    bx cs cluster-create --location dal10 --machine-type u2c.2x4 --no-subnet --public-vlan 2234945 --private-vlan 2234947 --workers 3 --name my_cluster
    ```
    {: pre}

4.  Verify that the creation of the cluster was requested.

    ```
    bx cs clusters
    ```
    {: pre}

    **Note:** It can take up to 15 minutes for the worker node machines to be ordered, and for the cluster to be set up and provisioned in your account.

    When the provisioning of your cluster is completed, the status of your cluster changes to **deployed**.

    ```
    Name         ID                                   State      Created          Workers   Location   Version
    mycluster    aaf97a8843a29941b49a598f516da72101   deployed   20170201162433   3         dal10      1.8.11
    ```
    {: screen}

5.  Check the status of the worker nodes.

    ```
    bx cs workers <cluster>
    ```
    {: pre}

    When the worker nodes are ready, the state changes to **normal** and the status is **Ready**. When the node status is **Ready**, you can then access the cluster.

    ```
    ID                                                  Public IP        Private IP     Machine Type   State      Status   Location   Version
    prod-dal10-pa8dfcc5223804439c87489886dbbc9c07-w1    169.xx.xxx.xxx   10.xxx.xx.xxx   free           normal     Ready    dal10      1.8.11
    ```
    {: screen}

6.  Add the subnet to your cluster by specifying the subnet ID. When you make a subnet available to a cluster, a Kubernetes configmap is created for you that includes all available portable public IP addresses that you can use. If no application load balancers exist for your cluster, one portable public and one portable private IP address is automatically used to create the public and private application load balancers. All other portable public and private IP addresses can be used to create load balancer services for your apps.

    ```
    bx cs cluster-subnet-add mycluster 807861
    ```
    {: pre}

7. Optional: [Enable routing between subnets on the same VLAN](#vlan-spanning).

<br />


## Adding user-managed subnets and IP addresses to Kubernetes clusters
{: #user_managed}

Provide a subnet from an on-premises network that you want {{site.data.keyword.containershort_notm}} to access. Then, you can add private IP addresses from that subnet to load balancer services in your Kubernetes cluster.
{:shortdesc}

Requirements:
- User-managed subnets can be added to private VLANs only.
- The subnet prefix length limit is /24 to /30. For example, `169.xx.xxx.xxx/24` specifies 253 usable private IP addresses, while `169.xx.xxx.xxx/30` specifies 1 usable private IP address.
- The first IP address in the subnet must be used as the gateway for the subnet.

Before you begin:
- Configure the routing of network traffic into and out of the external subnet.
- Confirm that you have VPN connectivity between the on-premises data center network gateway and either the private network Vyatta or the strongSwan VPN service that runs in your cluster. For more information, see [Setting up VPN connectivity](cs_vpn.html).

To add a subnet from an on-premises network:

1. View the ID of your cluster's Private VLAN. Locate the **VLANs** section. In the field **User-managed**, identify the VLAN ID with _false_.

    ```
    bx cs cluster-get --showResources <cluster_name>
    ```
    {: pre}

    ```
    VLANs
    VLAN ID   Subnet CIDR       Public   User-managed
    2234947   10.xxx.xx.xxx/29  false    false
    2234945   169.xx.xxx.xxx/29 true     false
    ```
    {: screen}

2. Add the external subnet to your private VLAN. The portable private IP addresses are added to the cluster's configmap.

    ```
    bx cs cluster-user-subnet-add <cluster_name> <subnet_CIDR> <VLAN_ID>
    ```
    {: pre}

    Example:

    ```
    bx cs cluster-user-subnet-add mycluster 10.xxx.xx.xxx/24 2234947
    ```
    {: pre}

3. Verify that the user-provided subnet is added. The field **User-managed** is _true_.

    ```
    bx cs cluster-get --showResources <cluster_name>
    ```
    {: pre}

    ```
    VLANs
    VLAN ID   Subnet CIDR       Public   User-managed
    2234947   10.xxx.xx.xxx/29  false    false
    2234945   169.xx.xxx.xxx/29 true   false
    2234947   10.xxx.xx.xxx/24  false    true
    ```
    {: screen}

4. Optional: [Enable routing between subnets on the same VLAN](#vlan-spanning).

5. Add a private load balancer service or a private Ingress application load balancer to access your app over the private network. To use a private IP address from the subnet that you added, you must specify an IP address. Otherwise, an IP address is chosen at random from the IBM Cloud infrastructure (SoftLayer) subnets or user-provided subnets on the private VLAN. For more information, see [Enabling public or private access to an app by using a LoadBalancer service](cs_loadbalancer.html#config) or [Enabling the private application load balancer](cs_ingress.html#private_ingress).

<br />


## Managing IP addresses and subnets
{: #manage}

Review the following options for listing available public IP addresses, freeing up used IP addresses, and routing between multiple subnets on the same VLAN.
{:shortdesc}

### Viewing available portable public IP addresses
{: #review_ip}

To list all of the IP addresses in your cluster, both used and available, you can run:

  ```
  kubectl get cm ibm-cloud-provider-vlan-ip-config -n kube-system -o yaml
  ```
  {: pre}

To list only available public IP addresses for your cluster, you can use the following steps:

Before you begin, [set the context for the cluster you want to use.](cs_cli_install.html#cs_cli_configure)

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

    **Note:** The creation of this service fails because the Kubernetes master cannot find the specified load balancer IP address in the Kubernetes configmap. When you run this command, you can see the error message and the list of available public IP addresses for the cluster.

    ```
    Error on cloud load balancer a8bfa26552e8511e7bee4324285f6a4a for service default/myservice with UID 8bfa2655-2e85-11e7-bee4-324285f6a4af: Requested cloud provider IP 1.1.1.1 is not available. The following cloud provider IP addresses are available: <list_of_IP_addresses>
    ```
    {: screen}

### Freeing up used IP addresses
{: #free}

You can free up a used portable IP address by deleting the load balancer service that is using the portable IP address.
{:shortdesc}

Before you begin, [set the context for the cluster you want to use.](cs_cli_install.html#cs_cli_configure)

1.  List available services in your cluster.

    ```
    kubectl get services
    ```
    {: pre}

2.  Remove the load balancer service that uses a public or private IP address.

    ```
    kubectl delete service <service_name>
    ```
    {: pre}

### Enabling routing between subnets on the same VLAN
{: #vlan-spanning}

When you create a cluster, a subnet that ends in `/26` is provisioned in the same VLAN that the cluster is on. This primary subnet can hold up to 62 worker nodes.
{:shortdesc}

This 62 worker node limit might be exceeded by a large cluster or by several smaller clusters in a single region that are on the same VLAN. When the 62 worker node limit is reached, a second primary subnet in the same VLAN is ordered.

To route between subnets on the same VLAN, you must turn on VLAN spanning. For instructions, see [Enable or disable VLAN spanning](/docs/infrastructure/vlans/vlan-spanning.html#enable-or-disable-vlan-spanning).

