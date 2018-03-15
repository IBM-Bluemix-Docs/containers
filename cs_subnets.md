---

copyright:
  years: 2014, 2018
lastupdated: "2018-03-15"

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

In {{site.data.keyword.containershort_notm}}, you can add stable, portable IPs for Kubernetes services by adding network subnets to the cluster. In this case, subnets are not being used with netmasking to create connectivity across one or more clusters. Instead, the subnets are used to provide permanent fixed IPs for a service from a cluster that can be used to access that service.

When you create a standard cluster, {{site.data.keyword.containershort_notm}} automatically provisions a portable public subnet with 5 public IP addresses and a portable private subnet with 5 private IP addresses. Portable public and private IP addresses are static and do not change when a worker node, or even the cluster, is removed. For each subnet, one of the portable public and one of the portable private IP addresses are used for [application load balancers](cs_ingress.html) that you can use to expose multiple apps in your cluster. The remaining 4 portable public and 4 portable private IP addresses can be used to expose single apps to the public by [creating a load balancer service](cs_loadbalancer.html).

**Note:** Portable public IP addresses are charged on a monthly basis. If you choose to remove portable public IP addresses after your cluster is provisioned, you still have to pay the monthly charge, even if you used them only for a short amount of time.

## Requesting additional subnets for your cluster
{: #request}

You can add stable, portable public or private IPs to the cluster by assigning subnets to the cluster.
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
    <caption>Understanding this command's components</caption>
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
    <td>Replace <code>&lt;subnet_size&gt;</code> with the number of IP addresses that you want to add from your portable subnet. Accepted values are 8, 16, 32, or 64. <p>**Note:** When you add portable IP addresses for your subnet, three IP addresses are used to establish cluster-internal networking, so that you cannot use them for your application load balancer or to create a load balancer service. For example, if you request eight portable public IP addresses, you can use five of them to expose your apps to the public.</p> </td>
    </tr>
    <tr>
    <td><code><em>&lt;VLAN_ID&gt;</em></code></td>
    <td>Replace <code>&lt;VLAN_ID&gt;</code> with the ID of the public or private VLAN on which you want to allocate the portable public or private IP addresses. You must select the public or private VLAN that an existing worker node is connected to. To review the public or private VLAN for a worker node, run the <code>bx cs worker-get &lt;worker_id&gt;</code> command. </td>
    </tr>
    </tbody></table>

2.  Verify that the subnet was successfully created and added to your cluster. The subnet CIDR is listed in the **VLANs** section.

    ```
    bx cs cluster-get --showResources <cluster name or id>
    ```
    {: pre}

3. Optional: [Enable routing between subnets on the same VLAN](#vlan-spanning).

<br />


## Adding custom and existing subnets to Kubernetes clusters
{: #custom}

You can add existing portable public or private subnets to your Kubernetes cluster.
{:shortdesc}

Before you begin, [target your CLI](cs_cli_install.html#cs_cli_configure) to your cluster.

If you have an existing subnet in your IBM Cloud infrastructure (SoftLayer) portfolio with custom firewall rules or available IP addresses that you want to use, create a cluster with no subnet and make your existing subnet available to the cluster when the cluster provisions.

1.  Identify the subnet to use. Note the ID of the subnet and the VLAN ID. In this example, the subnet ID is 807861 and the VLAN ID is 1901230.

    ```
    bx cs subnets
    ```
    {: pre}

    ```
    Getting subnet list...
    OK
    ID        Network                                      Gateway                                   VLAN ID   Type      Bound Cluster
    553242    203.0.113.0/24                               10.87.15.00                               1565280   private
    807861    192.0.2.0/24                                 10.121.167.180                            1901230   public

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
    ID        Name                  Number   Type      Router
    1900403   vlan                    1391     private   bcr01a.dal10
    1901230   vlan                    1180     public   fcr02a.dal10
    ```
    {: screen}

3.  Create a cluster by using the location and VLAN ID that you identified. Include the `--no-subnet` flag to prevent a new portable public IP subnet and a new portable private IP subnet from being created automatically.

    ```
    bx cs cluster-create --location dal10 --machine-type u2c.2x4 --no-subnet --public-vlan 1901230 --private-vlan 1900403 --workers 3 --name my_cluster
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
    my_cluster   paf97e8843e29941b49c598f516de72101   deployed   20170201162433   3         dal10      1.8.8
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
    prod-dal10-pa8dfcc5223804439c87489886dbbc9c07-w1    169.47.223.113   10.171.42.93   free           normal     Ready    dal10      1.8.8
    ```
    {: screen}

6.  Add the subnet to your cluster by specifying the subnet ID. When you make a subnet available to a cluster, a Kubernetes config map is created for you that includes all available portable public IP addresses that you can use. If no application load balancers already exist for your cluster, one portable public and one portable private IP address is automatically used to create the public and private application load balancers. All other portable public and private IP addresses can be used to create load balancer services for your apps.

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
- The subnet prefix length limit is /24 to /30. For example, `203.0.113.0/24` specifies 253 usable private IP addresses, while `203.0.113.0/30` specifies 1 usable private IP address.
- The first IP address in the subnet must be used as the gateway for the subnet.

Before you begin:
- Configure the routing of network traffic into and out of the external subnet.
- Confirm that you have VPN connectivity between the on-premises data center gateway device and either the private network Vyatta in your IBM Cloud infrastructure (SoftLayer) portfolio or the Strongswan VPN service running in your cluster. To use a Vyatta, see this [blog post ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/blogs/bluemix/2017/07/kubernetes-and-bluemix-container-based-workloads-part4/)]. To use Strongswan, see [Setting up VPN connectivity with the Strongswan IPSec VPN service](cs_vpn.html).

To add a subnet from an on-prem network:

1. View the ID of your cluster's Private VLAN. Locate the **VLANs** section. In the field **User-managed**, identify the VLAN ID with _false_.

    ```
    bx cs cluster-get --showResources <cluster_name>
    ```
    {: pre}

    ```
    VLANs
    VLAN ID   Subnet CIDR         Public       User-managed
    1555503   192.0.2.0/24        true         false
    1555505   198.51.100.0/24     false        false
    ```
    {: screen}

2. Add the external subnet to your private VLAN. The portable private IP addresses are added to the cluster's config map.

    ```
    bx cs cluster-user-subnet-add <cluster_name> <subnet_CIDR> <VLAN_ID>
    ```
    {: pre}

    Example:

    ```
    bx cs cluster-user-subnet-add my_cluster 203.0.113.0/24 1555505
    ```
    {: pre}

3. Verify that the user-provided subnet is added. The field **User-managed** is _true_.

    ```
    bx cs cluster-get --showResources <cluster_name>
    ```
    {: pre}

    ```
    VLANs
    VLAN ID   Subnet CIDR         Public       User-managed
    1555503   192.0.2.0/24        true         false
    1555505   198.51.100.0/24     false        false
    1555505   203.0.113.0/24      false        true
    ```
    {: screen}

4. Optional: [Enable routing between subnets on the same VLAN](#vlan-spanning).

5. Add a private load balancer service or a private Ingress application load balancer to access your app over the private network. If you want to use a private IP address from the subnet that you added when you create a private load balancer or a private Ingress application load balancer, you must specify an IP address. Otherwise, an IP address is chosen at random from the IBM Cloud infrastructure (SoftLayer) subnets or user-provided subnets on the private VLAN. For more information, see [Configuring access to an app by using the load balancer service type](cs_loadbalancer.html#config) or [Enabling the private application load balancer](cs_ingress.html#private_ingress).

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

    **Note:** The creation of this service fails because the Kubernetes master cannot find the specified load balancer IP address in the Kubernetes config map. When you run this command, you can see the error message and the list of available public IP addresses for the cluster.

    ```
    Error on cloud load balancer a8bfa26552e8511e7bee4324285f6a4a for service default/myservice with UID 8bfa2655-2e85-11e7-bee4-324285f6a4af: Requested cloud provider IP 1.1.1.1 is not available. The following cloud provider IPs are available: <list_of_IP_addresses>
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
    kubectl delete service <myservice>
    ```
    {: pre}

### Enabling routing between subnets on the same VLAN
{: #vlan-spanning}

When you create a cluster, a subnet ending in `/26` is provisioned in the same VLAN that the cluster is on. This primary subnet can hold up to 62 worker nodes.
{:shortdesc}

This 62 worker node limit might be exceeded by a large cluster or by several smaller clusters in a single region that are on the same VLAN. When the 62 worker node limit is reached, a second primary subnet in the same VLAN is ordered.

To route between subnets on the same VLAN, you must turn on VLAN spanning. For instructions, see [Enable or disable VLAN spanning](/docs/infrastructure/vlans/vlan-spanning.html#enable-or-disable-vlan-spanning).
