---

copyright:
  years: 2014, 2020
lastupdated: "2020-01-03"

keywords: kubernetes, iks, subnets, ips, vlans, networking

subcollection: containers

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


# Planning your cluster network setup
{: #plan_clusters}

Design a network setup for your {{site.data.keyword.containerlong}} cluster that meets the needs of your workloads and environment.
{: shortdesc}

Get started by planning your setup for a VPC or a classic cluster.
* With [{{site.data.keyword.containerlong_notm}} clusters in VPC](#vpc_basics), you can create your cluster in the next generation of the {{site.data.keyword.cloud_notm}} platform, in [Virtual Private Cloud](/docs/infrastructure/vpc?topic=vpc-about-vpc) for Generation 1 compute resources. VPC gives you the security of a private cloud environment with the dynamic scalability of a public cloud.
* With [{{site.data.keyword.containerlong_notm}} classic clusters](#plan_basics), you can create your cluster on classic infrastructure. Classic clusters include all of the {{site.data.keyword.containerlong_notm}} mature and robust features for compute, networking, and storage.

First time creating a cluster? First, try out the [tutorial for creating a VPC cluster](/docs/containers?topic=containers-cs_cluster_tutorial) or the [tutorial for creating a classic cluster](/docs/containers?topic=containers-cs_cluster_tutorial). Then, come back here when you’re ready to plan out your production-ready clusters.
{: tip}


## Understanding network basics of VPC clusters
{: #vpc_basics}

When you create your cluster, you must choose a networking setup so that certain cluster components can communicate with each other and with networks or services outside of the cluster.
{: shortdesc}

* [Worker-to-worker communication](#vpc-worker-worker): All worker nodes must be able to communicate with each other on the private network through VPC subnets.
* [Worker-to-master and user-to-master communication](#vpc-workeruser-master): Your worker nodes and your authorized cluster users can communicate with the Kubernetes master securely over the private network through a private service endpoint, or the public network with TLS through a public service endpoint.
* [Worker communication to other services or networks](#vpc-worker-services-onprem): Allow your worker nodes to securely communicate with other {{site.data.keyword.cloud_notm}} services, such as {{site.data.keyword.registrylong}}, to on-premises networks, to other VPCs, or to classic infrastructure resources.
* [External communication to apps that run on worker nodes](#vpc-external-workers): Allow public or private requests into the cluster as well as requests out of the cluster to a public endpoint.
</br>

### Worker-to-worker communication: VPC subnets
{: #vpc-worker-worker}

Before you create a VPC cluster for the first time, you must [create a VPC subnet ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/vpc/provision/network) in each zone where you want to deploy worker nodes. A VPC subnet is a specified private IP address range (CIDR block) and configures a group of worker nodes and pods as if they are attached to the same physical wire.
{: shortdesc}

When you create a cluster, you specify an existing VPC subnet for each zone. Each worker node that you add in a cluster is deployed with a private IP address from the VPC subnet in that zone. For a list of IP address ranges per VPC zone, see [VPC default address prefixes](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-working-with-ip-address-ranges-address-prefixes-regions-and-subnets#default-vpc-address-prefixes). After the worker node is provisioned, the worker node IP address persists after a `reboot` operation, but the worker node IP address changes after `replace` and `update` operations.

Subnets provide a channel for connectivity among the worker nodes within the cluster. Additionally, any system that is connected to any of the private subnets in the same VPC can communicate with workers. For example, all subnets in one VPC can communicate through private layer 3 routing with a built-in VPC router. If you have multiple clusters that must communicate with each other, you can create the clusters in the same VPC. However, if your clusters do not need to communicate, you can achieve better network segmentation by creating the clusters in separate VPCs. You can also create [access control lists (ACLs)](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-setting-up-network-acls) for your VPC subnets to mediate traffic on the private network. ACLs consist of inbound and outbound rules that define which ingress and egress is permitted for each VPC subnet.

If your worker nodes must access a public endpoint outside of the cluster, you can enable a public gateway on the VPC subnet that the worker nodes are deployed to. A public gateway can be attached to or detached from a subnet at any time.

The default IP address range for VPC subnets is 10.0.0.0 – 10.255.255.255. Need to create your cluster by using custom-range subnets? Check out this guidance on [custom address prefixes](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-working-with-ip-address-ranges-address-prefixes-regions-and-subnets#address-prefixes-and-the-ibm-cloud-console-ui).
{: tip}

When you create VPC subnets for your clusters, keep in mind the following features and limitations. For more information about VPC subnets, see [Characteristics of subnets in the VPC](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-about-networking-for-vpc#characteristics-of-subnets).
* The default CIDR size of each VPC subnet is `/24`, which can support up to 253 worker nodes. If you plan to deploy more than 250 worker nodes per zone in one cluster, consider creating a subnet of a larger size.
* After you create a VPC subnet, you cannot resize it or change its IP range.
* Multiple clusters in the same VPC can share subnets.
* VPC subnets are bound to a single zone and cannot span multiple zones or regions.
* After you create a subnet, you cannot move it to a different zone, region, or VPC.
* If you have worker nodes that are attached to an existing subnet in a zone, you cannot change the subnet for that zone in the cluster.
* The `172.16.0.0/16`, `172.18.0.0/16`, `172.19.0.0/16`, and `172.20.0.0/16` ranges are prohibited.

</br>

### Worker-to-master and user-to-master communication: Service endpoints
{: #vpc-workeruser-master}

A communication channel must be set up so that worker nodes and authorized cluster users can establish a connection to the Kubernetes master. You can allow communication to the Kubernetes master by enabling the public and private service endpoints or the private service endpoint only. Note that using private service endpoint only incurs no billed or metered bandwidth charges.
{: shortdesc}

To secure communication over public and private service endpoints, {{site.data.keyword.containerlong_notm}} automatically sets up an OpenVPN connection between the Kubernetes master and the worker node when the cluster is created. Workers securely talk to the master through TLS certificates, and the master talks to workers through the OpenVPN connection. Note that you must enable your account to use service endpoints. To enable service endpoints, run `ibmcloud account update --service-endpoint-enable true`.

**Public and private service endpoints**</br>
* Communication between worker nodes and master is established over the private network through the private service endpoint only.
* By default, all calls to the master that are initiated by authorized cluster users are routed through the public service endpoint. If authorized cluster users are in your VPC network or are connected through a [VPC VPN connection](/docs/vpc-on-classic-network?topic=vpc-on-classic-network---using-vpn-with-your-vpc), the master is privately accessible through the private service endpoint.

**Private service endpoint only**</br>
* Communication between worker nodes and master is established over the private network through the private service endpoint.
* To access the master through the private service endpoint, authorized cluster users must either be in your VPC network or are connected through a [VPC VPN connection](/docs/vpc-on-classic-network?topic=vpc-on-classic-network---using-vpn-with-your-vpc).

In VPC clusters in {{site.data.keyword.containerlong_notm}}, you cannot disable the private service endpoint or set up a cluster with the public service endpoint only.
{: note}

</br>

### Worker communication to other services or networks
{: #vpc-worker-services-onprem}

Allow your worker nodes to securely communicate with other {{site.data.keyword.cloud_notm}} services, on-premises networks, other VPCs, and {{site.data.keyword.cloud_notm}} classic infrastructure resources.
{: shortdesc}

**Communication with other {{site.data.keyword.cloud_notm}} services over the private or public network**</br>
Your worker nodes can automatically and securely communicate with other [{{site.data.keyword.cloud_notm}} services that support private service endpoints](/docs/resources?topic=resources-private-network-endpoints), such as {{site.data.keyword.registrylong}}, over the private network. If an {{site.data.keyword.cloud_notm}} service does not support private service endpoints, your worker nodes must be connected to a subnet that has a public gateway attached to it. The pods on those worker nodes can securely communicate with the services over the public network through the subnet's public gateway.

Note that if you use [access control lists (ACLs)](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-setting-up-network-acls) for your VPC subnets, you must create inbound or outbound rules to allow your worker nodes to communicate with these services.
* If you do not attach public gateways to your subnets, you can create inbound rules to allow ingress from services that support private service endpoints.
* If you attach public gateways to your subnets, you can create inbound and outbound rules to allow ingress from and egress to services that support public service endpoints only.

**Communication with resources in on-premises data centers**</br>
To connect your cluster with your on-premises data center, you can set up the VPC VPN service or a strongSwan IPSec VPN service.
* With the {{site.data.keyword.vpc_short}} VPN, you connect an entire VPC to an on-premises data center. To get started by creating a VPC gateway for your subnets, see [Using VPN with your VPC](/docs/vpc-on-classic-network?topic=vpc-on-classic-network---using-vpn-with-your-vpc).
* With the [strongSwan IPSec VPN service ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.strongswan.org/about.html), you set up a VPN load balancer directly in your cluster. Note that you must enable a public gateway on the subnet where you deploy the strongSwan service. To get started, [configure and deploy the strongSwan IPSec VPN service](/docs/containers?topic=containers-vpn#vpn-setup).

If you plan to connect your cluster to on-premises networks, you might have subnet conflicts with the IBM-provided default 172.30.0.0/16 range for pods and 172.21.0.0/16 range for services. You can avoid subnet conflicts when you [create a cluster in the CLI](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cli_cluster-create-vpc-classic) by specifying a custom subnet CIDR for pods in the `--pod-subnet` flag and a custom subnet CIDR for services in the `--service-subnet` flag.
{: tip}

**Communication with resources in other VPCs**</br>
To connect an entire VPC to another VPC in your account, you can use the {{site.data.keyword.vpc_short}} VPN. For example, you can use the {{site.data.keyword.vpc_short}} VPN to connect subnets in a VPC in one region to subnets in a VPC in another region. To get started by creating a VPC gateway for your subnets, see [Using VPN with your VPC](/docs/vpc-on-classic-network?topic=vpc-on-classic-network---using-vpn-with-your-vpc). Note that if you use [access control lists (ACLs)](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-setting-up-network-acls) for your VPC subnets, you must create inbound or outbound rules to allow your worker nodes to communicate with the subnets in other VPCs.

**Communication with {{site.data.keyword.cloud_notm}} classic resources**</br>
If you need to connect your cluster to resources in your {{site.data.keyword.cloud_notm}} classic infrastructure, you can set up access between one VPC in each region to one {{site.data.keyword.cloud_notm}} classic infrastructure account. You must enable the VPC for classic access when you create the VPC, and you cannot convert an existing VPC to use classic access. To get started, see [Setting up access to your Classic Infrastructure from VPC](/docs/vpc-on-classic-network?topic=vpc-on-classic-setting-up-access-to-your-classic-infrastructure-from-vpc).

When you create a VPC with classic infrastructure access, you can set up an [{{site.data.keyword.cloud_notm}} Direct Link](/docs/infrastructure/direct-link?topic=direct-link-get-started-with-ibm-cloud-direct-link) connection between your classic infrastructure and your remote networks. Any clusters that you create in the VPC with classic infrastructure access can access the Direct Link connection.

</br>

### External communication to apps that run on worker nodes
{: #vpc-external-workers}

Allow private or public traffic requests from outside the cluster to your apps that run on worker nodes.
{: shortdesc}

**Private traffic to cluster apps**</br>
When you deploy an app in your cluster, you might want to make the app accessible to only users and services that are on the same private network as your cluster. Private load balancing is ideal for making your app available to requests from outside the cluster without exposing the app to the general public. You can also use private load balancing to test access, request routing, and other configurations for your app before you later expose your app to the public with public network services.

To allow private network traffic requests from outside the cluster to your apps, you can use private Kubernetes networking services, such as creating [`LoadBalancer` services](/docs/containers?topic=containers-vpc-lbaas) or using your cluster's private [Ingress application load balancers (ALBs)](/docs/containers?topic=containers-ingress-about). For example, when you create a Kubernetes `LoadBalancer` service in your cluster, a load balancer for VPC is automatically created in your VPC outside of your cluster.  When you enable private ALBs in your cluster, one VPC load balancer is created for all of the private ALBs. The load balancer is multizonal and routes requests for your app through the private NodePorts that are automatically opened on your worker nodes. You can then create access control lists (ACLs) for your VPC subnets to allow inbound network traffic requests from specified sources.

For more information, see [Planning private external load balancing](/docs/containers?topic=containers-cs_network_planning#private_access).

**Public traffic to cluster apps**</br>
To make your apps accessible from the public internet, you can use public networking services. Even though your worker nodes are connected to private VPC subnets only, the VPC load balancer that is created for public networking services can route public requests to your app on the private network by providing your app a public URL. When an app is publicly exposed, anyone that has the public URL can send a request to your app.

You can create public Kubernetes [`LoadBalancer` services](/docs/containers?topic=containers-vpc-lbaas) or use the default public [Ingress application load balancers (ALBs)](/docs/containers?topic=containers-ingress-about). For example, when you create a Kubernetes `LoadBalancer` service in your cluster, a load balancer for VPC is automatically created in your VPC outside of your cluster. One VPC load balancer also exists by default for all of the public ALBs in your cluster. The load balancer is multizonal and routes requests for your app through the private NodePorts that are automatically opened on your worker nodes. You can then create access control lists (ACLs) for your VPC subnets to allow inbound network traffic requests from specified sources.

Note that a public gateway is not required on your subnets to allow inbound network traffic from the internet to `LoadBalancer` services or ALBs. Public gateways are required only to allow worker nodes to make outbound requests to public endpoints. For more information, see [Planning public external load balancing](/docs/containers?topic=containers-cs_network_planning#public_access).

<br />


## Example scenarios for VPC cluster network setups
{: #vpc-scenarios}

Now that you understand the basics of cluster networking, check out some example scenarios in which various VPC cluster network setups can meet your workload needs.
{: shortdesc}

### Scenario: Run internet-facing app workloads in a VPC cluster
{: #vpc-no-pgw}

In this scenario, you run workloads in a VPC cluster that are accessible to requests from the Internet. Public access is controlled by ACLs so that end users can access your apps while unwanted public requests to your apps are denied. Additionally, your workers have automatic access to any {{site.data.keyword.cloud_notm}} services that support private service endpoints.
{: shortdesc}

<p>
<figure>
 <img src="images/vpc_no_pgw.png" width="850" style="width:850px; border-style: none" alt="Network setup for a VPC cluster that runs internet-facing app workloads"/>
 <figcaption>Network setup for a VPC cluster that runs internet-facing app workloads</figcaption>
</figure>
</p>

**Worker-to-worker communication**

To achieve this setup, you create VPC subnets in each zone where you want to deploy worker nodes. No public gateways are required for these subnets. Then, you create a VPC cluster that uses these VPC subnets.

**Worker-to-master and user-to-master communication**

You can choose to allow worker-to-master and user-to-master communication over the public and private networks, or over the private network only.
* Public and private service endpoints: Communication between worker nodes and master is established over the private network through the private service endpoint. By default, all calls to the master that are initiated by authorized cluster users are routed through the public service endpoint.
* Private service endpoint only: Communication to master from both worker nodes and cluster users is established over the private network through the private service endpoint. Cluster users must either be in your VPC network or connect through a [VPC VPN connection](/docs/vpc-on-classic-network?topic=vpc-on-classic-network---using-vpn-with-your-vpc).

**Worker communication to other services or networks**

If your app workload requires other {{site.data.keyword.cloud_notm}} services, your worker nodes can automatically, securely communicate with {{site.data.keyword.cloud_notm}} services that support private service endpoints over the private VPC network.

**External communication to apps that run on worker nodes**

After you test your app, you can expose it to the internet by creating a public Kubernetes `LoadBalancer` service or using the default public Ingress application load balancers (ALBs). The VPC load balancer that is automatically created in your VPC outside of your cluster when you use one of these services routes traffic to your app. You can improve the security of your cluster and control public network traffic to your apps by creating access control lists (ACLs). ACLs consist of rules that define which inbound traffic is permitted for the VPC subnets that your worker nodes are connected to.

Ready to get started with a cluster for this scenario? After you plan your [high availability](/docs/containers?topic=containers-ha_clusters) and [worker node](/docs/containers?topic=containers-planning_worker_nodes) setups, see [Creating VPC Gen 1 compute clusters](/docs/containers?topic=containers-clusters).

<br />


### Scenario: Run internet-facing app workloads in a VPC cluster with limited public egress
{: #vpc-pgw}

In this scenario, you run workloads in a VPC cluster that are accessible to requests from the Internet. Public access is controlled so that end users can access your apps while unwanted public requests to your apps are denied. However, you might need to also provide limited public egress from your worker nodes to a public endpoint, and want to ensure that this public egress is controlled and isolated in your cluster. For example, you might need your app pods to access an {{site.data.keyword.cloud_notm}} service that does not support private service endpoints, and must be accessed over the public network.
{: shortdesc}

<p>
<figure>
 <img src="images/cs_org_ov_vpc.png" width="850" style="width:850px; border-style: none" alt="Network setup for a cluster that allows limited, secure public access"/>
 <figcaption>Network setup for a VPC cluster that allows limited, secure public access</figcaption>
</figure>
</p>

**Worker-to-worker communication**

To achieve this setup in, for example, a multizone cluster that has worker nodes in two zones, you create a VPC subnet in one zone that has no public gateway attached, and a VPC subnet in another zone that does have a public gateway attached. Then, you create a VPC cluster that uses these VPC subnets and zones.

**Worker-to-master and user-to-master communication**

When you create the cluster you can choose to allow worker-to-master and user-to-master communication over the public and private networks, or over the private network only.
* Public and private service endpoints: Communication between worker nodes and master is established over the private network through the private service endpoint. By default, all calls to the master that are initiated by authorized cluster users are routed through the public service endpoint.
* Private service endpoint only: Communication to master from both worker nodes and cluster users is established over the private network through the private service endpoint. Cluster users must either be in your VPC network or connect through a [VPC VPN connection](/docs/vpc-on-classic-network?topic=vpc-on-classic-network---using-vpn-with-your-vpc).

**Worker communication to other services or networks**

After the cluster is created, you create a worker pool that is deployed only to the zone where subnet has an attached public gateway. Any app pods that are deployed to the worker nodes in this pool can make requests to a public endpoint through the public gateway. For example, if you want your app pods to send logs to {{site.data.keyword.at_full}} (which does not support private endpoints), you can add an affinity rule for the worker pool ID label to the app deployment. This affinity rule ensures that the app pods which require public egress are confined to only one worker pool on one subnet. If you deploy other apps that do not require public egress, you can instead add anti-affinity rules to the app deployment so that the app pods deploy to only worker pools on subnets without a public gateway.

If your app workload requires other {{site.data.keyword.cloud_notm}} services that support private service endpoints, your worker nodes can automatically, securely communicate with these services over the private VPC network without using a public gateway.

**External communication to apps that run on worker nodes**

After you test your app, you can expose it to the internet by creating a public Kubernetes `LoadBalancer` service or using the default public Ingress application load balancers (ALBs). The VPC load balancer that is automatically created in your VPC outside of your cluster when you use one of these services routes traffic to your app. You can improve the security of your cluster and control public traffic apps by creating access control lists (ACLs). ACLs consist of rules that define which inbound traffic is permitted for the VPC subnets that your worker nodes are connected to. For example, you can use inbound rules to control incoming public traffic to your apps through the VPC load balancer, and outbound rules to control outgoing requests from your apps through the public gateway.

Ready to get started with a cluster for this scenario? After you plan your [high availability](/docs/containers?topic=containers-ha_clusters) and [worker node](/docs/containers?topic=containers-planning_worker_nodes) setups, see [Creating VPC Gen 1 compute clusters](/docs/containers?topic=containers-clusters).

<br />



### Scenario: Extend your on-premises data center to a VPC cluster
{: #vpc-vpn}

In this scenario, you run workloads in a VPC cluster. However, you want these workloads to be accessible only to services, databases, or other resources in your private networks in an on-premises data center. Your cluster workloads might need to access a few other {{site.data.keyword.cloud_notm}} services that support communication over the private network.
{: shortdesc}

<p>
<figure>
 <img src="images/vpc_extend.png" width="850" style="width:850px; border-style: none" alt="Network setup for a VPC cluster that extends an on-prem data center"/>
 <figcaption>Network setup for a VPC cluster that extends an on-prem data center</figcaption>
</figure>
</p>

**Worker-to-worker communication**

To achieve this setup, you create VPC subnets in each zone where you want to deploy worker nodes. No public gateways are required for these subnets. Then, you create a VPC cluster that uses these VPC subnets.

Note that you might have subnet conflicts between the default ranges for workers nodes, pods, and services, and the subnets in your on-premises networks. When you create your VPC subnets, you can choose [custom address prefixes](/docs/vpc-on-classic-network?topic=vpc-on-classic-network-working-with-ip-address-ranges-address-prefixes-regions-and-subnets#address-prefixes-and-the-ibm-cloud-console-ui) and the create your cluster by using these subnets. Additionally, you can specify a custom subnet CIDR for pods and services by using the `--pod-subnet` and `--service-subnet` flags in the `ibmcloud ks cluster create` command when you create your cluster.

**Worker-to-master and user-to-master communication**

When you create the cluster, you enable private service endpoint only to allow worker-to-master and user-to-master communication over the private network. Cluster users must either be in your VPC network or connect through a [VPC VPN connection](/docs/vpc-on-classic-network?topic=vpc-on-classic-network---using-vpn-with-your-vpc).

**Worker communication to other services or networks**

To connect your cluster with your on-premises data center, you can set up the VPC VPN service. The {{site.data.keyword.vpc_short}} VPN connects your entire VPC to an on-premises data center. If your app workload requires other {{site.data.keyword.cloud_notm}} services that support private service endpoints, your worker nodes can automatically, securely communicate with these services over the private VPC network.

**External communication to apps that run on worker nodes**

After you test your app, you can expose it to the private network by creating a private Kubernetes `LoadBalancer` service or using the default private Ingress application load balancers (ALBs). The VPC load balancer that is automatically created in your VPC outside of your cluster when you use one of these services routes traffic to your app. Note that the VPC load balancer exposes your app to the private network only so that any on-premises system with a connection to the VPC subnet can access the app. You can improve the security of your cluster and control public traffic apps by creating access control lists (ACLs). ACLs consist of rules that define which inbound traffic is permitted for the VPC subnets that your worker nodes are connected to.

Ready to get started with a cluster for this scenario? After you plan your [high availability](/docs/containers?topic=containers-ha_clusters) and [worker node](/docs/containers?topic=containers-planning_worker_nodes) setups, see [Creating VPC Gen 1 compute clusters](/docs/containers?topic=containers-clusters).

<br />


## Understanding network basics of classic clusters
{: #plan_basics}

When you create a classic cluster, you must choose a networking setup so that certain cluster components can communicate with each other and with networks or services outside of the cluster.
{: shortdesc}

* [Worker-to-worker communication](#worker-worker): All worker nodes must be able to communicate with each other on the private network. In many cases, communication must be permitted across multiple private VLANs to allow workers on different VLANs and in different zones to connect with each other.
* [Worker-to-master and user-to-master communication](#workeruser-master): Your worker nodes and your authorized cluster users can communicate with the Kubernetes master securely over the public network with TLS or over the private network through private service endpoints.
* [Worker communication to other {{site.data.keyword.cloud_notm}} services or on-premises networks](#worker-services-onprem): Allow your worker nodes to securely communicate with other {{site.data.keyword.cloud_notm}} services, such as {{site.data.keyword.registrylong}}, and to an on-premises network.
* [External communication to apps that run on worker nodes](#external-workers): Allow public or private requests into the cluster as well as requests out of the cluster to a public endpoint.

### Worker-to-worker communication: classic VLANs and subnets
{: #worker-worker}

When you create a classic cluster, the cluster's worker nodes are connected automatically to a private VLAN and optionally connected to a public VLAN. A VLAN configures a group of worker nodes and pods as if they were attached to the same physical wire and provides a channel for connectivity among the workers.
{: shortdesc}

**VLAN connections for worker nodes**</br>
All worker nodes must be connected to a private VLAN so that each worker node can send information to and receive information from other worker nodes. The private VLAN provides private subnets that are used to assign private IP addresses to your worker nodes and private app services. You can create a cluster with worker nodes that are also connected to a public VLAN. The public VLAN provides public subnets that are used to assign public IP addresses to your worker nodes and public app services. However, if you need to secure your apps from the public network interface, several options are available to secure your cluster such as using [Calico network policies](/docs/containers?topic=containers-network_policies) or isolating external network workloads to [edge worker nodes](/docs/containers?topic=containers-edge).
* Free clusters: In free clusters, the cluster's worker nodes are connected to an IBM-owned public VLAN and private VLAN by default. Because IBM controls the VLANs, subnets, and IP addresses, you cannot create multizone clusters or add subnets to your cluster, and can use only NodePort services to expose your app.</dd>
* Standard clusters: In standard clusters, the first time that you create a cluster in a zone, a public VLAN, and a private VLAN in that zone are automatically provisioned for you in your IBM Cloud infrastructure account. If you specify that worker nodes must be connected to a private VLAN only, then a private VLAN only in that zone is automatically provisioned. For every subsequent cluster that you create in that zone, you can specify the VLAN pair that you want to use. You can reuse the same public and private VLANs that were created for you because multiple clusters can share VLANs.

For more information about VLANs, subnets, and IP addresses, see [Overview of networking in {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-subnets#basics).

Need to create your cluster by using custom subnets? Check out [Using existing subnets to create a cluster](/docs/containers?topic=containers-subnets#subnets_custom).
{: tip}

**Worker node communication across subnets and VLANs**</br>
In several situations, components in your cluster must be permitted to communicate across multiple private VLANs. For example, if you want to create a multizone cluster, if you have multiple VLANs for a cluster, or if you have multiple subnets on the same VLAN, the worker nodes on different subnets in the same VLAN or in different VLANs cannot automatically communicate with each other. You must enable either Virtual Routing and Forwarding (VRF) or VLAN spanning for your IBM Cloud infrastructure account.

* [Virtual Routing and Forwarding (VRF)](/docs/resources?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud): VRF enables all the private VLANs and subnets in your infrastructure account to communicate with each other. Additionally, VRF is required to allow your workers and master to communicate over the private service endpoint, and to communicate with other {{site.data.keyword.cloud_notm}} instances that support private service endpoints. To check whether a VRF is already enabled, use the `ibmcloud account show` command. To enable VRF, run `ibmcloud account update --service-endpoint-enable true`. This command output prompts you to open a support case to enable your account to use VRF and service endpoints. VRF eliminates the VLAN spanning option for your account because all VLANs are able to communicate.</br></br>
When VRF is enabled, any system that is connected to any of the private VLANs in the same {{site.data.keyword.cloud_notm}} account can communicate with the cluster worker nodes. You can isolate your cluster from other systems on the private network by applying [Calico private network policies](/docs/containers?topic=containers-network_policies#isolate_workers).</dd>
* [VLAN spanning](/docs/infrastructure/vlans?topic=vlans-vlan-spanning#vlan-spanning): If you cannot or do not want to enable VRF, such as if you do not need the master to be accessible on the private network or if you use a gateway appliance to access the master over the public VLAN, enable VLAN spanning. For example, if you have an existing gateway appliance and then add a cluster, the new portable subnets that are ordered for the cluster aren't configured on the gateway appliance but VLAN spanning enables routing between the subnets. To enable VLAN spanning, you need the **Network > Manage Network VLAN Spanning** [infrastructure permission](/docs/containers?topic=containers-users#infra_access), or you can request the account owner to enable it. To check whether VLAN spanning is already enabled, use the `ibmcloud ks vlan spanning get` [command](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlan_spanning_get). You cannot enable the private service endpoint if you choose to enable VLAN spanning instead of VRF.

</br>

### Worker-to-master and user-to-master communication: Service endpoints
{: #workeruser-master}

A communication channel must be set up so that worker nodes can establish a connection to the Kubernetes master. You can allow your worker nodes and Kubernetes master to communicate by enabling the public service endpoint only, public and private service endpoints, or the private service endpoint only.
{: shortdesc}

To secure communication over public and private service endpoints, {{site.data.keyword.containerlong_notm}} automatically sets up an OpenVPN connection between the Kubernetes master and the worker node when the cluster is created. Workers securely talk to the master through TLS certificates, and the master talks to workers through the OpenVPN connection.

**Public service endpoint only**</br>
If you don’t want to or cannot enable VRF for your account, your worker nodes can automatically connect to the Kubernetes master over the public VLAN through the public service endpoint.
* Communication between worker nodes and master is established securely over the public network through the public service endpoint.
* The master is publicly accessible to authorized cluster users only through the public service endpoint. Your cluster users can securely access your Kubernetes master over the internet to run `kubectl` commands, for example.

**Public and private service endpoints**</br>
To make your master publicly or privately accessible to cluster users, you can enable the public and private service endpoints. VRF is required in your {{site.data.keyword.cloud_notm}} account, and you must enable your account to use service endpoints. To enable VRF and service endpoints, run `ibmcloud account update --service-endpoint-enable true`.
* If worker nodes are connected to public and private VLANs, communication between worker nodes and master is established over both the private network through the private service endpoint and the public network through the public service endpoint. By routing half of the worker-to-master traffic over the public endpoint and half over the private endpoint, your master-to-worker communication is protected from potential outages of the public or private network. If worker nodes are connected to private VLANs only, communication between worker nodes and master is established over the private network through the private service endpoint only.
* The master is publicly accessible to authorized cluster users through the public service endpoint. The master is privately accessible through the private service endpoint if authorized cluster users are in your {{site.data.keyword.cloud_notm}} private network or are connected to the private network through a VPN connection or {{site.data.keyword.cloud_notm}} Direct Link. Note that you must [expose the master endpoint through a private load balancer](/docs/containers?topic=containers-access_cluster#access_private_se) so that users can access the master through a VPN or {{site.data.keyword.cloud_notm}} Direct Link connection.


**Private service endpoint only**</br>
To make your master only privately accessible, you can enable the private service endpoint. VRF is required in your {{site.data.keyword.cloud_notm}} account, and you must enable your account to use service endpoints. To enable VRF and service endpoints, run `ibmcloud account update --service-endpoint-enable true`. Note that using private service endpoint only incurs no billed or metered bandwidth charges.
* Communication between worker nodes and master is established over the private network through the private service endpoint.
* The master is privately accessible if authorized cluster users are in your {{site.data.keyword.cloud_notm}} private network or are connected to the private network through a VPN connection or DirectLink. Note that you must [expose the master endpoint through a private load balancer](/docs/containers?topic=containers-access_cluster#access_private_se) so that users can access the master through a VPN or DirectLink connection.

</br>

### Worker communication to other {{site.data.keyword.cloud_notm}} services or on-premises networks
{: #worker-services-onprem}

Allow your worker nodes to securely communicate with other {{site.data.keyword.cloud_notm}} services and to an on-premises network.
{: shortdesc}

**Communication with other {{site.data.keyword.cloud_notm}} services over the private or public network**</br>
Your worker nodes can automatically and securely communicate with other [{{site.data.keyword.cloud_notm}} services that support private service endpoints](/docs/resources?topic=resources-private-network-endpoints), such as {{site.data.keyword.registrylong}}, over your IBM Cloud infrastructure private network. If an {{site.data.keyword.cloud_notm}} service does not support private service endpoints, your worker nodes must be connected to a public VLAN so that they can securely communicate with the services over the public network.

If you use Calico policies or a gateway appliance to control the public or private networks of your worker nodes, you must allow access to the public IP addresses of the services that support public service endpoints, and optionally to the private IP addresses of the services that support private service endpoints.
* [Allow access to services' public IP addresses in Calico policies](/docs/containers?topic=containers-network_policies#isolate_workers_public)
* [Allow access to the private IP addresses of services that support private service endpoints in Calico policies](/docs/containers?topic=containers-network_policies#isolate_workers)
* [Allow access to services' public IP addresses and to the private IP addresses of services that support private service endpoints in a gateway appliance firewall](/docs/containers?topic=containers-firewall#firewall_outbound)

**{{site.data.keyword.BluDirectLink}} for communication over the private network with resources in on-premises data centers**</br>
To connect your cluster with your on-premises data center, such as with {{site.data.keyword.icpfull_notm}}, you can set up [{{site.data.keyword.cloud_notm}} Direct Link](/docs/infrastructure/direct-link?topic=direct-link-get-started-with-ibm-cloud-direct-link). With {{site.data.keyword.cloud_notm}} Direct Link, you create a direct, private connection between your remote network environments and {{site.data.keyword.containerlong_notm}} without routing over the public internet.

**strongSwan IPSec VPN connection for communication over the public network with resources in on-premises data centers**
* Worker nodes that are connected to public and private VLANs: Set up a [strongSwan IPSec VPN service ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.strongswan.org/about.html) directly in your cluster. The strongSwan IPSec VPN service provides a secure end-to-end communication channel over the internet that is based on the industry-standard Internet Protocol Security (IPSec) protocol suite. To set up a secure connection between your cluster and an on-premises network, [configure and deploy the strongSwan IPSec VPN service](/docs/containers?topic=containers-vpn#vpn-setup) directly in a pod in your cluster.

* Worker nodes connected to a private VLAN only: Set up an IPSec VPN endpoint on a gateway appliance, such as a Virtual Router Appliance (Vyatta). Then, [configure the strongSwan IPSec VPN service](/docs/containers?topic=containers-vpn#vpn-setup) in your cluster to use the VPN endpoint on your gateway. If you do not want to use strongSwan, you can [set up VPN connectivity directly with VRA](/docs/containers?topic=containers-vpn#vyatta).

Standard clusters that run Kubernetes 1.15 or later: If you plan to connect your cluster to on-premises networks, you might have subnet conflicts with the IBM-provided default 172.30.0.0/16 range for pods and 172.21.0.0/16 range for services. You can avoid subnet conflicts when you [create a cluster in the CLI](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cli_cluster-create-vpc-classic) by specifying a custom subnet CIDR for pods in the `--pod-subnet` flag and a custom subnet CIDR for services in the `--service-subnet` flag.
{: tip}

</br>

### External communication to apps that run on worker nodes
{: #external-workers}

Allow public or private traffic requests from outside the cluster to your apps that run on worker nodes.
{: shortdesc}

**Private traffic to cluster apps**</br>
When you deploy an app in your cluster, you might want to make the app accessible to only users and services that are on the same private network as your cluster. Private load balancing is ideal for making your app available to requests from outside the cluster without exposing the app to the general public. You can also use private load balancing to test access, request routing, and other configurations for your app before you later expose your app to the public with public network services. To allow private traffic requests from outside the cluster to your apps, you can create private Kubernetes networking services, such as private NodePorts, NLBs, and Ingress ALBs. You can then use Calico pre-DNAT policies to block traffic to public NodePorts of private networking services. For more information, see [Planning private external load balancing](/docs/containers?topic=containers-cs_network_planning#private_access).

**Public traffic to cluster apps**</br>
To make your apps externally accessible from the public internet, you can create public NodePorts, network load balancers (NLBs), and Ingress application load balancers (ALBs). Public networking services connect to this public network interface by providing your app with a public IP address and, depending on the service, a public URL. When an app is publicly exposed, anyone that has the public service IP address or the URL that you set up for your app can send a request to your app. You can then use Calico pre-DNAT policies to control traffic to public networking services, such as whitelisting traffic from only certain source IP addresses or CIDRs and blocking all other traffic. For more information, see [Planning public external load balancing](/docs/containers?topic=containers-cs_network_planning#public_access).

For additional security, you can isolate networking workloads to gateway worker nodes and edge worker nodes.
* **Gateway-enabled classic clusters**: Gateway worker nodes help you achieve network connectivity separation between the internet or an on-premises data center and the compute workload that runs in your cluster. When you create a classic cluster with a gateway, the cluster is created with a `compute` worker pool of compute worker nodes that are connected to a private VLAN only, and a `gateway` worker pool of gateway worker nodes that are connected to public and private VLANs. The gateway worker pool provides an edge firewall in the form of Calico policies for ingress and egress traffic, load balancers for ingress traffic to the cluster, and a gateway for egress traffic from the cluster. All NLB pods deploy to the gateway worker nodes, which are also tainted so that no compute workloads can be scheduled onto them. Then, if you want to provide another level of network separation between the public network and your worker pools of compute worker nodes, you can optionally create an edge worker pool. When you create an edge node worker pool in a gateway-enabled cluster, ALB pods are deployed to only those specified worker nodes. By deploying only ALBs to edge nodes, all layer 7 proxy management is kept separate from the gateway worker nodes so that TLS termination and HTTP request routing is completed by the ALBs on the private network only.
* **Classic clusters without a gateway**: Edge worker nodes can improve the security of your cluster by allowing fewer worker nodes that are connected to public VLANs to be accessed externally and by isolating the networking workload. When you [label worker nodes as edge nodes](/docs/containers?topic=containers-edge#edge_nodes), NLB and ALB pods are deployed to only those specified worker nodes. To also prevent other workloads from running on edge nodes, you can [taint the edge nodes](/docs/containers?topic=containers-edge#edge_workloads). In Kubernetes version 1.14 and later, you can deploy both public and private NLBs and ALBs to edge nodes.
For example, if your worker nodes are connected to a private VLAN only, but you need to permit public access to an app in your cluster, you can create an edge worker pool in which the edge nodes are connected to public and private VLANs. You can deploy public NLBs and ALBs to these edge nodes to ensure that only those workers handle public connections.

<br />


## Example scenarios for classic cluster network setups
{: #classic-scenarios}

Now that you understand the basics of cluster networking, check out some example scenarios in which various classic cluster network setups can meet your workload needs.
{: shortdesc}

### Scenario: Run internet-facing app workloads in a classic cluster
{: #internet-facing}

In this scenario, you want to run workloads in a classic cluster that are accessible to requests from the Internet so that end users can access your apps. You want the option of isolating public access in your cluster and of controlling what public requests are permitted to your cluster. Additionally, your workers have automatic access to any {{site.data.keyword.cloud_notm}} services that you want to connect with your cluster.
{: shortdesc}

<p>
<figure>
 <img src="images/cs_clusters_planning_internet.png" alt="Architecture image for a cluster that runs internet-facing workloads"/>
 <figcaption>Network setup for a cluster that runs internet-facing workloads</figcaption>
</figure>
</p>

**Worker-to-worker communication**

To achieve this setup, you create a cluster by connecting worker nodes to public and private VLANs.

If you create the cluster with both public and private VLANs, you cannot later remove all public VLANs from that cluster. Removing all public VLANs from a cluster causes several cluster components to stop working. Instead, create a new worker pool that is connected to a private VLAN only.
{: note}

**Worker-to-master and user-to-master communication**

You can choose to allow worker-to-master and user-to-master communication over the public and private networks, or over the public network only.
* Public and private service endpoints: Your account must be enabled with VRF and enabled to use service endpoints. Communication between worker nodes and master is established over both the private network through the private service endpoint and the public network through the public service endpoint. The master is publicly accessible to authorized cluster users through the public service endpoint.
* Public service endpoint: If you don’t want to or cannot enable VRF for your account, your worker nodes and authorized cluster users can automatically connect to the Kubernetes master over the public network through the public service endpoint.

**Worker communication to other services or networks**

Your worker nodes can automatically, securely communicate with other {{site.data.keyword.cloud_notm}} services that support private service endpoints over your IBM Cloud infrastructure private network. If an {{site.data.keyword.cloud_notm}} service does not support private service endpoints, workers can securely communicate with the services over the public network. You can lock down the public or private interfaces of worker nodes by using Calico network policies for public network or private network isolation. You might need to allow access to the public and private IP addresses of the services that you want to use in these Calico isolation policies.

If your worker nodes need to access services in private networks outside of your {{site.data.keyword.cloud_notm}} account, you can configure and deploy the strongSwan IPSec VPN service in your cluster or leverage {{site.data.keyword.cloud_notm}} {{site.data.keyword.cloud_notm}} Direct Link services to connect to these networks.

**External communication to apps that run on worker nodes**

To expose an app in your cluster to the internet, you can create a public network load balancer (NLB) or Ingress application load balancer (ALB) service. You can improve the security of your cluster by creating a pool of worker nodes that are labeled as edge nodes. The pods for public network services are deployed to the edge nodes so that external traffic workloads are isolated to only a few workers in your cluster. You can further control public traffic to the network services that expose your apps by creating Calico pre-DNAT policies, such as whitelist and blacklist policies.

Ready to get started with a cluster for this scenario? After you plan your [high availability](/docs/containers?topic=containers-ha_clusters) and [worker node](/docs/containers?topic=containers-planning_worker_nodes) setups, see [Creating clusters](/docs/containers?topic=containers-clusters).

<br />


### Scenario: Extend your on-premises data center to a classic cluster and add limited public access
{: #limited-public}

In this scenario, you want to run workloads in a classic cluster that are accessible to services, databases, or other resources in your on-premises data center. However, you might need to provide limited public access to your cluster, and want to ensure that any public access is controlled and isolated in your cluster. For example, you might need your workers to access an {{site.data.keyword.cloud_notm}} service that does not support private service endpoints, and must be accessed over the public network. Or you might need to provide limited public access to an app that runs in your cluster.
{: shortdesc}

To achieve this cluster setup, you can create a firewall by creating a [gateway-enabled cluster](#gateway) or [using a gateway appliance](#vyatta-gateway).

#### Using a gateway-enabled cluster
{: #gateway}

Keep your compute workloads private and allow limited public connectivity to your classic cluster by enabling a gateway.
{: shortdesc}

<p>
<figure>
 <img src="images/cs_clusters_planning_classic_gateway.png" alt="Architecture image for a cluster that a gateway for secure public access"/>
 <figcaption>Network setup for a cluster that uses a gateway for secure public access</figcaption>
</figure>
</p>

**Worker-to-worker communication**

With this setup, you create a cluster that is connected to a public VLAN and a private VLAN and has an enabled gateway. The cluster is created with a `compute` worker pool of compute worker nodes that are connected to a private VLAN only, and a `gateway` worker pool of gateway worker nodes that are connected to public and private VLANs. Gateway worker nodes help you achieve network connectivity separation between the internet or an on-premises data center and the compute workload that runs in your cluster. After your cluster is created, if you want to provide another level of network separation between the public network and your worker pools of compute worker nodes, you can optionally [create an edge worker pool](/docs/containers?topic=containers-edge#edge_gateway), which is connected to a private VLAN only. Only ALB pods are deployed to the edge worker nodes.

**Worker-to-master and user-to-master communication**

To set up and use the private service endpoint, your account must be enabled with VRF and enabled to use private service endpoints. You can choose to allow worker-to-master and user-to-master communication over the public and private networks, or over the private network only.
* Public and private service endpoints: Communication between compute worker nodes and the master is established over the private network through the private service endpoint only. Communication between gateway worker nodes and the master is established over both the private network through the private service endpoint and the public network through the public service endpoint. The master is publicly accessible to authorized cluster users through the public service endpoint.
* Private service endpoint only: Communication between all worker nodes and the master is established over the private network through the private service endpoint. The Kubernetes master is accessible through the private service endpoint if authorized cluster users are in your {{site.data.keyword.cloud_notm}} private network or are connected to the private network through a [VPN connection](/docs/infrastructure/iaas-vpn?topic=iaas-vpn-getting-started) or [{{site.data.keyword.cloud_notm}} Direct Link](/docs/infrastructure/direct-link?topic=direct-link-get-started-with-ibm-cloud-direct-link). However, communication with the Kubernetes master over the private service endpoint must go through the <code>166.X.X.X</code> IP address range, which is not routable from a VPN connection or through {{site.data.keyword.cloud_notm}} Direct Link. You can expose the private service endpoint of the master for your cluster users by using a private network load balancer (NLB). The private NLB exposes the private service endpoint of the master as an internal <code>10.X.X.X</code> IP address range that users can access with the VPN or {{site.data.keyword.cloud_notm}} Direct Link connection. If you enable only the private service endpoint, you can use the Kubernetes dashboard or temporarily enable the public service endpoint to create the private NLB.

**Worker communication to other services or networks**

Your worker nodes can automatically, securely communicate with other {{site.data.keyword.cloud_notm}} services that support private service endpoints over your {{site.data.keyword.cloud_notm}} private network. If an {{site.data.keyword.cloud_notm}} service does not support private service endpoints, your gateway worker nodes that are connected to a public VLAN can securely communicate with the services over the public network. The gateway worker nodes are automatically protected by default Calico policies that act as a firewall for your cluster. If you create additional Calico network policies to further lock down the public or private network interfaces of your worker nodes, you might need to allow access to the public and private IP addresses of the services that you want to use in these Calico isolation policies.

To securely access services outside of {{site.data.keyword.cloud_notm}} and other on-premises networks, you can configure and deploy the strongSwan IPSec VPN service in your cluster. The strongSwan load balancer pod deploys to a worker in the gateway pool, where the pod establishes a secure connection to the on-premises network through an encrypted VPN tunnel over the public network. Alternatively, you can use {{site.data.keyword.cloud_notm}} Direct Link services to connect your cluster to your on-premises data center over the private network only.

**External communication to apps that run on worker nodes**

To provide private access to an app in your cluster, you can create a private network load balancer (NLB) or Ingress application load balancer (ALB) to expose your app to the private network only. If you need to provide public access to an app in your cluster, you can create a public NLB or ALB to expose your app. The pods for private and public NLBs and public and private ALBs deploy to the gateway worker nodes, which are also tainted so that no compute workloads can be scheduled onto them. If you also choose to create an edge node worker pool, ALB pods are deployed to the edge worker nodes instead of the gateway worker nodes. The gateway worker nodes also provide Equal Cost Multipath (ECMP) gateways for egress traffic from the cluster. When the app returns a response, the ECMP protocol is used to balance the response traffic through a gateway on one of the gateway worker nodes to the client. The gateway worker nodes are automatically protected by default Calico policies that act as a firewall for your cluster. You can further limit public traffic to the network services that expose your apps by creating Calico pre-DNAT policies, such as whitelist and blacklist policies.

Ready to get started with a cluster for this scenario? After you plan your [high availability](/docs/containers?topic=containers-ha_clusters) and [worker node](/docs/containers?topic=containers-planning_worker_nodes) setups, see [Prepare to create clusters at the account level](/docs/containers?topic=containers-clusters). Then, follow the steps in [Creating a standard classic cluster with a gateway](/docs/containers?topic=containers-clusters#gateway_cluster_cli).

</br>

#### Using a gateway appliance
{: #vyatta-gateway}

Allow limited public connectivity to your classic cluster by configuring a gateway appliance, such as a Virtual Router Appliance (Vyatta), as a public gateway and firewall.
{: shortdesc}

<p>
<figure>
 <img src="images/cs_clusters_planning_gateway.png" width="850" style="width:850px; border-style: none" alt="Architecture image for a cluster that uses a gateway appliance for secure public access"/>
 <figcaption>Network setup for a cluster that uses a gateway appliance for secure public access</figcaption>
</figure>
</p>

**Worker-to-worker communication, worker-to-master and user-to-master communication**

If you set up your worker nodes on a private VLAN only and you don’t want to or cannot enable VRF for your account, you must configure a gateway appliance to provide network connectivity between your worker nodes and the master over the public network. For example, you might choose to set up a [Virtual Router Appliance](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-about-the-vra) or a [Fortigate Security Appliance](/docs/services/vmwaresolutions/services?topic=vmware-solutions-fsa_considerations).

You can set up your gateway appliance with custom network policies to provide dedicated network security for your cluster and to detect and remediate network intrusion. When you set up a firewall on the public network, you must open up the required ports and private IP addresses for each region so that the master and the worker nodes can communicate. If you also configure this firewall for the private network, you must also open up the required ports and private IP addresses to allow communication between worker nodes and let your cluster access infrastructure resources over the private network. You must also enable VLAN spanning for your account so that subnets can route on the same VLAN and across VLANs.

**Worker communication to other services or networks**

To securely connect your worker nodes and apps to an on-premises network or services outside of {{site.data.keyword.cloud_notm}}, set up an IPSec VPN endpoint on your gateway appliance and the strongSwan IPSec VPN service in your cluster to use the gateway VPN endpoint. If you do not want to use strongSwan, you can set up VPN connectivity directly with VRA.

Your worker nodes can securely communicate with other {{site.data.keyword.cloud_notm}} services and public services outside of {{site.data.keyword.cloud_notm}} through your gateway appliance. You can configure your firewall allow access to the public and private IP addresses of only the services that you want to use

**External communication to apps that run on worker nodes**

To provide private access to an app in your cluster, you can create a private network load balancer (NLB) or Ingress application load balancer (ALB) to expose your app to the private network only. If you need to provide limited public access to an app in your cluster, you can create a public NLB or ALB to expose your app. Because all traffic goes through your gateway appliance firewall, you can control public and private traffic to the network services that expose your apps by opening up the service's ports and IP addresses in your firewall to permit inbound traffic to these services.

Ready to get started with a cluster for this scenario? After you plan your [high availability](/docs/containers?topic=containers-ha_clusters) and [worker node](/docs/containers?topic=containers-planning_worker_nodes) setups, see [Creating clusters](/docs/containers?topic=containers-clusters).

<br />



### Scenario: Extend your on-premises data center to a classic cluster
{: #private_clusters}

In this scenario, you want to run workloads in a classic cluster. However, you want these workloads to be accessible only to services, databases, or other resources in your on-premises data center, such as {{site.data.keyword.icpfull_notm}}. Your cluster workloads might need to access a few other {{site.data.keyword.cloud_notm}} services that support communication over the private network, such as {{site.data.keyword.cos_full_notm}}.
{: shortdesc}

<p>
<figure>
 <img src="images/cs_clusters_planning_extend.png" width="850" style="width:850px; border-style: none" alt="Architecture image for a cluster that connects to an on-premises data center on the private network"/>
 <figcaption>Network setup for a cluster that connects to an on-premises data center on the private network</figcaption>
</figure>
</p>

**Worker-to-worker communication**

To achieve this setup, you create a cluster by connecting worker nodes to a private VLAN only. To provide connectivity between the cluster master and worker nodes over the private network through the private service endpoint only, your account must be enabled with VRF and enabled to use service endpoints. Because your cluster is visible to any resource on the private network when VRF is enabled, you can isolate your cluster from other systems on the private network by applying Calico private network policies.

Note that you might have subnet conflicts between the default ranges for workers nodes, pods, and services, and the subnets in your on-premises networks. You create your cluster without IBM-provided subnets by including the `--no-subnet` flag. After the cluster is created, you can [add custom subnets](/docs/containers?topic=containers-subnets#subnets_custom) to your cluster. Additionally, you can specify a custom subnet CIDR for pods and services by using the `--pod-subnet` and `--service-subnet` flags in the `ibmcloud ks cluster create` command when you create your cluster.

**Worker-to-master and user-to-master communication**

The Kubernetes master is accessible through the private service endpoint if authorized cluster users are in your {{site.data.keyword.cloud_notm}} private network or are connected to the private network through a [VPN connection](/docs/infrastructure/iaas-vpn?topic=iaas-vpn-getting-started) or [{{site.data.keyword.cloud_notm}} Direct Link](/docs/infrastructure/direct-link?topic=direct-link-get-started-with-ibm-cloud-direct-link). However, communication with the Kubernetes master over the private service endpoint must go through the <code>166.X.X.X</code> IP address range, which is not routable from a VPN connection or through {{site.data.keyword.cloud_notm}} Direct Link. You can expose the private service endpoint of the master for your cluster users by using a private network load balancer (NLB). The private NLB exposes the private service endpoint of the master as an internal <code>10.X.X.X</code> IP address range that users can access with the VPN or {{site.data.keyword.cloud_notm}} Direct Link connection. If you enable only the private service endpoint, you can use the Kubernetes dashboard or temporarily enable the public service endpoint to create the private NLB.

**Worker communication to other services or networks**

Your worker nodes can automatically, securely communicate with other {{site.data.keyword.cloud_notm}} services that support private service endpoints, such as {{site.data.keyword.registrylong}}, over your IBM Cloud infrastructure private network. For example, dedicated hardware environments for all standard plan instances of {{site.data.keyword.cloudant_short_notm}} support private service endpoints. If an {{site.data.keyword.cloud_notm}} service does not support private service endpoints, your cluster cannot access that service.

**External communication to apps that run on worker nodes**

To provide private access to an app in your cluster, you can create a private network load balancer (NLB) or Ingress application load balancer (ALB). These Kubernetes network services expose your app to the private network only so that any on-premises system with a connection to the subnet that the NLB IP is on can access the app.

Ready to get started with a cluster for this scenario? After you plan your [high availability](/docs/containers?topic=containers-ha_clusters) and [worker node](/docs/containers?topic=containers-planning_worker_nodes) setups, see [Creating clusters](/docs/containers?topic=containers-clusters).






