---

copyright:
  years: 2014, 2021
lastupdated: "2021-05-24"

keywords: kubernetes, iks, subnets, ips, vlans, networking

subcollection: containers

---

{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}
{:android: data-hd-operatingsystem="android"}
{:api: .ph data-hd-interface='api'}
{:apikey: data-credential-placeholder='apikey'}
{:app_key: data-hd-keyref="app_key"}
{:app_name: data-hd-keyref="app_name"}
{:app_secret: data-hd-keyref="app_secret"}
{:app_url: data-hd-keyref="app_url"}
{:authenticated-content: .authenticated-content}
{:beta: .beta}
{:c#: data-hd-programlang="c#"}
{:cli: .ph data-hd-interface='cli'}
{:codeblock: .codeblock}
{:curl: .ph data-hd-programlang='curl'}
{:deprecated: .deprecated}
{:dotnet-standard: .ph data-hd-programlang='dotnet-standard'}
{:download: .download}
{:external: target="_blank" .external}
{:faq: data-hd-content-type='faq'}
{:fuzzybunny: .ph data-hd-programlang='fuzzybunny'}
{:generic: data-hd-operatingsystem="generic"}
{:generic: data-hd-programlang="generic"}
{:gif: data-image-type='gif'}
{:go: .ph data-hd-programlang='go'}
{:help: data-hd-content-type='help'}
{:hide-dashboard: .hide-dashboard}
{:hide-in-docs: .hide-in-docs}
{:important: .important}
{:ios: data-hd-operatingsystem="ios"}
{:java: .ph data-hd-programlang='java'}
{:java: data-hd-programlang="java"}
{:javascript: .ph data-hd-programlang='javascript'}
{:javascript: data-hd-programlang="javascript"}
{:new_window: target="_blank"}
{:note .note}
{:note: .note}
{:objectc data-hd-programlang="objectc"}
{:org_name: data-hd-keyref="org_name"}
{:php: data-hd-programlang="php"}
{:pre: .pre}
{:preview: .preview}
{:python: .ph data-hd-programlang='python'}
{:python: data-hd-programlang="python"}
{:route: data-hd-keyref="route"}
{:row-headers: .row-headers}
{:ruby: .ph data-hd-programlang='ruby'}
{:ruby: data-hd-programlang="ruby"}
{:runtime: architecture="runtime"}
{:runtimeIcon: .runtimeIcon}
{:runtimeIconList: .runtimeIconList}
{:runtimeLink: .runtimeLink}
{:runtimeTitle: .runtimeTitle}
{:screen: .screen}
{:script: data-hd-video='script'}
{:service: architecture="service"}
{:service_instance_name: data-hd-keyref="service_instance_name"}
{:service_name: data-hd-keyref="service_name"}
{:shortdesc: .shortdesc}
{:space_name: data-hd-keyref="space_name"}
{:step: data-tutorial-type='step'}
{:subsection: outputclass="subsection"}
{:support: data-reuse='support'}
{:swift: .ph data-hd-programlang='swift'}
{:swift: data-hd-programlang="swift"}
{:table: .aria-labeledby="caption"}
{:term: .term}
{:terraform: .ph data-hd-interface='terraform'}
{:tip: .tip}
{:tooling-url: data-tooling-url-placeholder='tooling-url'}
{:troubleshoot: data-hd-content-type='troubleshoot'}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:tsSymptoms: .tsSymptoms}
{:tutorial: data-hd-content-type='tutorial'}
{:ui: .ph data-hd-interface='ui'}
{:unity: .ph data-hd-programlang='unity'}
{:url: data-credential-placeholder='url'}
{:user_ID: data-hd-keyref="user_ID"}
{:vbnet: .ph data-hd-programlang='vb.net'}
{:video: .video}
  
  

# Configuring classic subnets and IP addresses
{: #subnets}

Change the pool of available portable public or private IP addresses by adding subnets to your {{site.data.keyword.containerlong}} cluster.
{: shortdesc}

<img src="images/icon-classic.png" alt="Classic infrastructure provider icon" width="15" style="width:15px; border-style: none"/> The content on this page is specific to classic clusters. For information about VPC clusters, see [Configuring subnets and IP addresses for VPC clusters](/docs/containers?topic=containers-vpc-subnets).
{: note}


## Overview of classic networking in {{site.data.keyword.containerlong_notm}}
{: #basics}

Understand the basic concepts of classic networking in {{site.data.keyword.containerlong_notm}} clusters. {{site.data.keyword.containerlong_notm}} uses VLANs, subnets, and IP addresses to give cluster components network connectivity.
{: shortdesc}


### VLANs
{: #basics_vlans}

When you create a cluster, the cluster's worker nodes are connected automatically to a VLAN. A VLAN configures a group of worker nodes and pods as if they were attached to the same physical wire and provides a channel for connectivity among the workers and pods.
{: shortdesc}

<dl>
<dt>VLANs for free clusters</dt>
<dd>In free clusters, the cluster's worker nodes are connected to an IBM-owned public VLAN and private VLAN by default. Because IBM controls the VLANs, subnets, and IP addresses, you cannot create multizone clusters or add subnets to your cluster, and can use only NodePort services to expose your app.</dd>
<dt>VLANs for standard clusters</dt>
<dd>In standard clusters, the first time that you create a cluster in a zone, a public VLAN and a private VLAN in that zone are automatically provisioned for you in your IBM Cloud infrastructure account. For every subsequent cluster that you create in that zone, you must specify the VLAN pair that you want to use in that zone. You can reuse the same public and private VLANs that were created for you because multiple clusters can share VLANs.</br>
</br>You can either connect your worker nodes to both a public VLAN and the private VLAN, or to the private VLAN only. If you want to connect your worker nodes to a private VLAN only, you can use the ID of an existing private VLAN or [create a private VLAN](/docs/cli/reference/ibmcloud?topic=cli-manage-classic-vlans#sl_vlan_create) and use the ID during cluster creation.</dd></dl>

To see the VLANs that are provisioned in each zone for your account, run `ibmcloud ks vlan ls --zone <zone>.` To see the VLANs that one cluster is provisioned on, run `ibmcloud ks cluster get --cluster <cluster_name_or_ID> --show-resources` and look for the **Subnet VLANs** section.

IBM Cloud infrastructure manages the VLANs that are automatically provisioned when you create your first cluster in a zone. If you let a VLAN become unused, such as by removing all worker nodes from a VLAN, IBM Cloud infrastructure reclaims the VLAN. After, if you need a new VLAN, [contact {{site.data.keyword.cloud_notm}} support](/docs/vlans?topic=vlans-ordering-premium-vlans#ordering-premium-vlans).

**Can I change my VLAN decision later?**

You can change your VLAN setup by modifying the worker pools in your cluster. For more information, see [Changing your worker node VLAN connections](/docs/containers?topic=containers-cs_network_cluster#change-vlans).


### Subnets and IP addresses
{: #basics_subnets}

In addition to worker nodes and pods, subnets are also automatically provisioned onto VLANs. Subnets provide network connectivity to your cluster components by assigning IP addresses to them.
{: shortdesc}

The following subnets are automatically provisioned on the default public and private VLANs:

**Public VLAN subnets**
* The primary public subnet determines the public IP addresses that are assigned to worker nodes during cluster creation. Multiple clusters on the same VLAN can share one primary public subnet.
* The portable public subnet is bound to one cluster only and provides the cluster with 8 public IP addresses. 3 IPs are reserved for IBM Cloud infrastructure functions. 1 IP is used by the default public Ingress ALB and 4 IPs can be used to create public network load balancer (NLB) services or more public ALBs. Portable public IPs are permanent, fixed IP addresses that can be used to access NLBs or ALBs over the internet. If you need more than 4 IPs for NLBs or ALBs, see [Adding portable IP addresses](/docs/containers?topic=containers-subnets#adding_ips). Note that these IP addresses are intended for use in {{site.data.keyword.containerlong_notm}}. Do not use these IP addresses for other purposes outside of {{site.data.keyword.containerlong_notm}}.

**Private VLAN subnets**
* The primary private subnet determines the private IP addresses that are assigned to worker nodes during cluster creation. Multiple clusters on the same VLAN can share one primary private subnet.
* The portable private subnet is bound to one cluster only and provides the cluster with 8 private IP addresses. 3 IPs are reserved for IBM Cloud infrastructure functions. 1 IP is used by the default private Ingress ALB and 4 IPs can be used to create private network load balancer (NLB) services or more private ALBs. Portable private IPs are permanent, fixed IP addresses that can be used to access NLBs or ALBs over a private network. If you need more than 4 IPs for private NLBs or ALBs, see [Adding portable IP addresses](/docs/containers?topic=containers-subnets#adding_ips). Note that these IP addresses are intended for use in {{site.data.keyword.containerlong_notm}}. Do not use these IP addresses for other purposes outside of {{site.data.keyword.containerlong_notm}}.

To see all of the subnets provisioned in all resource groups of your account, run `ibmcloud ks subnets --provider classic`. To see the portable public and portable private subnets that are bound to one cluster, you can run `ibmcloud ks cluster get --cluster <cluster_name_or_ID> --show-resources` and look for the **Subnet VLANs** section.

In {{site.data.keyword.containerlong_notm}}, VLANs have a limit of 40 subnets. If you reach this limit, first check to see whether you can [reuse subnets in the VLAN to create new clusters](/docs/containers?topic=containers-subnets#subnets_custom). If you need a new VLAN, order one by [contacting {{site.data.keyword.cloud_notm}} support](/docs/vlans?topic=vlans-ordering-premium-vlans#ordering-premium-vlans). Then, [create a cluster](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_create) that uses this new VLAN.
{: note}

**Do the IP addresses for my worker nodes change?**

Your worker node is assigned an IP address on the public or private VLANs that your cluster uses. After the worker node is provisioned, the worker node IP address persists across `reboot` and `update` operations, but the worker node IP address changes after a `replace` operation. Additionally, the private IP address of the worker node is used for the worker node identity in most `kubectl` commands. If you change the VLANs that the worker pool uses, new worker nodes that are provisioned in that pool use the new VLANs for their IP addresses. Existing worker node IP addresses do not change, but you can choose to remove the worker nodes that use the old VLANs.

**Can I specify subnets for pods and services in my cluster?**

If you plan to connect your cluster to on-premises networks through {{site.data.keyword.dl_full_notm}} or a VPN service, you can avoid subnet conflicts by specifying a custom subnet CIDR that provides the private IP addresses for your pods, and a custom subnet CIDR to provide the private IP addresses for services.

To specify custom pod and service subnets during cluster creation, use the `--pod-subnet` and `--service-subnet` flags in the `ibmcloud ks cluster create` CLI command.

**Pods**:
* Default range: All services that are deployed to the cluster are assigned a private IP address in the `172.30.0.0/16` range by default.
* Size requirements: When you specify a custom subnet, consider the size of the cluster that you plan to create and the number of worker nodes that you might add in the future. The subnet must have a CIDR of at least `/23`, which provides enough pod IPs for a maximum of four worker nodes in a cluster. For larger clusters, use `/22` to have enough pod IP addresses for eight worker nodes, `/21` to have enough pod IP addresses for 16 worker nodes, and so on.
* Range requirements: The pod and service subnets cannot overlap each other, and the pod subnet cannot overlap the subnets for your worker nodes. The subnet that you choose must be within one of the following ranges:
    * `172.17.0.0 - 172.17.255.255`
    * `172.21.0.0 - 172.31.255.255`
    * `192.168.0.0 - 192.168.254.255`
    * `198.18.0.0 - 198.19.255.255`

**Services**:
* Default range: All services that are deployed to the cluster are assigned a private IP address in the `172.21.0.0/16` range by default.
* Size requirements: When you specify a custom subnet, the subnet must be specified in CIDR format with a size of at least `/24`, which allows a maximum of 255 services in the cluster, or larger.
* Range requirements: The pod and service subnets cannot overlap each other. The subnet that you choose must be within one of the following ranges:
    * `172.17.0.0 - 172.17.255.255`
    * `172.21.0.0 - 172.31.255.255`
    * `192.168.0.0 - 192.168.254.255`
    * `198.18.0.0 - 198.19.255.255`

### Network segmentation
{: #basics_segmentation}

Network segmentation describes the approach to divide a network into multiple sub-networks. Apps that run in one sub-network cannot see or access apps in another sub-network. For more information about network segmentation options and how they relate to VLANs, see [this cluster security topic](/docs/containers?topic=containers-security#network_segmentation).
{: shortdesc}

However, in several situations, components in your cluster must be permitted to communicate across multiple private VLANs. For example, if you want to create a multizone cluster, if you have multiple VLANs for a cluster, or if you have multiple subnets on the same VLAN, the worker nodes on different subnets in the same VLAN or in different VLANs cannot automatically communicate with each other. You must enable either a Virtual Router Function (VRF) or VLAN spanning for your IBM Cloud infrastructure account.

**What are Virtual Router Functions (VRF) and VLAN spanning?**


<dl>
<dt>Virtual Router Function (VRF)</dt>
<dd>A VRF enables all the VLANs and subnets in your infrastructure account to communicate with each other. Additionally, a VRF is required to allow your workers and master to communicate over the private cloud service endpoint. To enable VRF, see [Enabling VRF](/docs/account?topic=account-vrf-service-endpoint#vrf). To check whether a VRF is already enabled, use the `ibmcloud account show` command. Note that VRF eliminates the VLAN spanning option for your account, because all VLANs are able to communicate unless you configure a gateway appliance to manage traffic.</dd>
<dt>VLAN spanning</dt>
<dd>If you cannot or do not want to enable VRF, [enable VLAN spanning](/docs/vlans?topic=vlans-vlan-spanning#vlan-spanning). To perform this action, you need the **Network > Manage Network VLAN Spanning** [infrastructure permission](/docs/containers?topic=containers-users#infra_access), or you can request the account owner to enable it. To check if VLAN spanning is already enabled, use the `ibmcloud ks vlan spanning get --region <region>` [command](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlan_spanning_get). Note that you cannot enable the private cloud service endpoint if you choose to enable VLAN spanning instead of a VRF.</dd>
</dl>

**How does VRF or VLAN spanning affect network segmentation?**


When VRF or VLAN spanning is enabled, any system that is connected to any of the private VLANs in the same {{site.data.keyword.cloud_notm}} account can communicate with workers. You can isolate your cluster from other systems on the private network by applying [Calico private network policies](/docs/containers?topic=containers-network_policies#isolate_workers). {{site.data.keyword.containerlong_notm}} is also compatible with all [IBM Cloud infrastructure firewall offerings](https://www.ibm.com/cloud/network-security){: external}. You can set up a firewall, such as a [Virtual Router Appliance](/docs/virtual-router-appliance?topic=virtual-router-appliance-about-the-vra), with custom network policies to provide dedicated network security for your standard cluster and to detect and remediate network intrusion.

<br />


## Using existing subnets to create a cluster
{: #subnets_custom}

When you create a standard cluster, subnets are automatically created for you. However, instead of using the automatically provisioned subnets, you can use existing portable subnets from your IBM Cloud infrastructure account or reuse subnets from a deleted cluster.
{: shortdesc}

Use this option to retain stable static IP addresses across cluster removals and creations, or to order larger blocks of IP addresses. If instead you want to get more portable public or private IP addresses to create network load balancer (NLB) or Ingress application load balancer (ALB) services, see [Adding portable IP addresses](#adding_ips).

All subnets that were automatically ordered during cluster creation are immediately marked for deletion after you delete a cluster, and you cannot reuse the subnets to create a new cluster.
{: note}

Before you begin:
- [Log in to your account. If applicable, target the appropriate resource group. Set the context for your cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
- To reuse user-managed private subnets from a cluster that you no longer need, delete the unneeded cluster.
   ```
   ibmcloud ks cluster rm --cluster <cluster_name_or_ID>
   ```
   {: pre}

</br>To create a cluster by using existing subnets:

1. Get the subnet ID and the ID of the VLAN that the subnet is on.

    ```
    ibmcloud ks subnets --provider classic
    ```
    {: pre}

    In this example output, the subnet ID is `1602829` and the VLAN ID is `2234945`:
    ```
    Getting subnet list...
    OK
    ID        Network             Gateway          VLAN ID   Type      Bound Cluster
    1550165   10.xxx.xx.xxx/26    10.xxx.xx.xxx    2234947   private
    1602829   169.xx.xxx.xxx/28   169.xx.xxx.xxx   2234945   public
    ```
    {: screen}

2. [Create a cluster from the CLI](/docs/containers?topic=containers-clusters#clusters_cli_steps) by using the VLAN ID that you identified. Include the `--no-subnet` flag to prevent a new portable public IP subnet and a new portable private IP subnet from being created automatically.

    ```
    ibmcloud ks cluster create classic --zone dal10 --flavor b3c.4x16 --no-subnet --public-vlan 2234945 --private-vlan 2234947 --workers 3 --name my_cluster
    ```
    {: pre}
    If you can't remember which zone the VLAN is in for the `--zone` flag, you can check whether the VLAN is in a certain zone by running `ibmcloud ks vlan ls --zone <zone>`.
    {: tip}

3.  Verify that the cluster was created. It can take up to 15 minutes for the worker node machines to be ordered and for the cluster to be set up and provisioned in your account.

    ```
    ibmcloud ks cluster ls
    ```
    {: pre}

    When your cluster is fully provisioned, the **State** changes to `deployed`.

    ```
    Name         ID                                   State      Created          Workers    Zone      Version     Resource Group Name   Provider
    mycluster    aaf97a8843a29941b49a598f516da72101   deployed   20170201162433   3          dal10     1.20.7      Default             classic
    ```
    {: screen}

4.  Check the status of the worker nodes.

    ```
    ibmcloud ks worker ls --cluster <cluster_name_or_ID>
    ```
    {: pre}

    Before continuing to the next step, the worker nodes must be ready. The **State** changes to `normal` and the **Status** is `Ready`.

    ```
    ID                                                  Public IP        Private IP     Machine Type   State      Status   Zone     Version
    prod-dal10-pa8dfcc5223804439c87489886dbbc9c07-w1    169.xx.xxx.xxx   10.xxx.xx.xxx  free           normal     Ready    dal10      1.20.7
    ```
    {: screen}

5.  Add the subnet to your cluster by specifying the subnet ID. When you make a subnet available to a cluster, a Kubernetes configmap is created for you that includes all available portable public IP addresses that you can use. If no Ingress ALBs exist in the zone where the subnet's VLAN is located, one portable public and one portable private IP address is automatically used to create the public and private ALBs for that zone. You can use all other portable public and private IP addresses from the subnet to create NLB services for your apps.
  ```
  ibmcloud ks cluster subnet add --cluster <cluster_name_or_id> --subnet-id <subnet_ID>
  ```
  {: pre}

6. Verify that the subnet is added to your cluster.

    ```
    ibmcloud ks cluster get --cluster <cluster_name> --show-resources
    ```
    {: pre}

7. **Important**: To enable communication between workers that are on different subnets on the same VLAN, you must [enable routing between subnets on the same VLAN](#subnet-routing).

<br />


## Managing existing portable IP addresses
{: #managing_ips}

By default, 4 portable public and 4 portable private IP addresses can be used to expose single apps to the public or private network by [creating a network load balancer (NLB) service](/docs/containers?topic=containers-loadbalancer) or by [creating additional Ingress application load balancers (ALBs)](/docs/containers?topic=containers-ingress#scale_albs). To create an NLB or ALB service, you must have at least 1 portable IP address of the correct type available. You can view portable IP addresses that are available or free up a used portable IP address.
{: shortdesc}

### Viewing available portable public IP addresses
{: #review_ip}

To list all of the portable IP addresses in your cluster, both used and available, you can run the following command.
{: shortdesc}

```
kubectl get cm ibm-cloud-provider-vlan-ip-config -n kube-system -o yaml
```
{: pre}

To list only portable public IP addresses that are available to create public NLBs or more public ALBs, you can use the following steps:

Before you begin:
-  Ensure that you have the [**Writer** or **Manager** {{site.data.keyword.cloud_notm}} IAM service access role](/docs/containers?topic=containers-users#platform) for the `default` namespace.
- [Log in to your account. If applicable, target the appropriate resource group. Set the context for your cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

To list available portable public IP addresses:

1.  Create a Kubernetes service configuration file that is named `myservice.yaml` and define a service of type `LoadBalancer` with a dummy NLB IP address. The following example uses the IP address 1.1.1.1 as the NLB IP address. Replace `<zone>` with the zone where you want to check for available IPs.

    ```yaml
    apiVersion: v1
    kind: Service
    metadata:
      labels:
        run: myservice
      name: myservice
      namespace: default
      annotations:
        service.kubernetes.io/ibm-load-balancer-cloud-provider-zone: "<zone>"
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

    The creation of this service fails because the Kubernetes master cannot find the specified NLB IP address in the Kubernetes configmap. When you run this command, you can see the error message and a list of available public IP addresses for the cluster.

    ```
    Error on cloud load balancer a8bfa26552e8511e7bee4324285f6a4a for service default/myservice with UID 8bfa2655-2e85-11e7-bee4-324285f6a4af: Requested cloud provider IP 1.1.1.1 is not available. The following cloud provider IP addresses are available: <list_of_IP_addresses>
    ```
    {: screen}

<br />

### Freeing up used IP addresses
{: #free}

You can free up a used portable IP address by deleting the network load balancer (NLB) service or disabling the Ingress application load balancer (ALB) that is using the portable IP address.
{: shortdesc}

Before you begin:
-  Ensure that you have the [**Writer** or **Manager** {{site.data.keyword.cloud_notm}} IAM service access role](/docs/containers?topic=containers-users#platform) for the `default` namespace.
- [Log in to your account. If applicable, target the appropriate resource group. Set the context for your cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

To delete an NLB or disable an ALB:

1. List available services in your cluster.
    ```
    kubectl get services | grep LoadBalancer
    ```
    {: pre}

2. Remove the load balancer service or disable the ALB that uses a public or private IP address.
  * Delete an NLB:
    ```
    kubectl delete service <service_name>
    ```
    {: pre}
  * Disable an ALB:
    ```
    ibmcloud ks ingress alb disable --alb <ALB_ID> -c <cluster_name_or_ID>
    ```
    {: pre}

<br />

## Adding portable IP addresses
{: #adding_ips}

By default, 4 portable public and 4 portable private IP addresses can be used to expose single apps to the public or private network by [creating a network load balancer (NLB) service](/docs/containers?topic=containers-loadbalancer). To create more than 4 public or 4 private NLBs, you can get more portable IP addresses by adding network subnets to the cluster.
{: shortdesc}

When you make a subnet available to a cluster, IP addresses of this subnet are used for cluster networking purposes. To avoid IP address conflicts, make sure to use a subnet with one cluster only. Do not use a subnet for multiple clusters or for other purposes outside of {{site.data.keyword.containerlong_notm}} at the same time.
{: important}

### Adding portable IPs by ordering more subnets
{: #request}

You can get more portable IPs for NLB services by creating a new subnet in an IBM Cloud infrastructure account and making it available to your specified cluster.
{: shortdesc}

Portable public IP addresses are charged monthly. If you remove portable public IP addresses after your subnet is provisioned, you still must pay the monthly charge, even if you used them only for a short amount of time.
{: note}

Before you begin:
-  Ensure that you have the [**Operator** or **Administrator** {{site.data.keyword.cloud_notm}} IAM platform access role](/docs/containers?topic=containers-users#platform) for the cluster.
- [Log in to your account. If applicable, target the appropriate resource group. Set the context for your cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

To order a subnet:

1. Provision a new subnet.

    ```
    ibmcloud ks cluster subnet create --cluster <cluster_name_or_id> --size <subnet_size> --vlan <VLAN_ID>
    ```
    {: pre}

    <table summary="The columns are read from left to right. The first column has the parameter of the command. The second column describes the parameter.">
    <caption>Understanding this command's components</caption>
    <col style="width:30%">
	  <col style="width:70%">
      <thead>
	      <th>Parameter</th>
	      <th>Description</th>
    </thead>
    <tbody>
    <tr>
    <td><code><em>&lt;cluster_name_or_id&gt;</em></code></td>
    <td>Replace <code>&lt;cluster_name_or_id&gt;</code> with the name or ID of the cluster.</td>
    </tr>
    <tr>
    <td><code><em>&lt;subnet_size&gt;</em></code></td>
    <td>Replace <code>&lt;subnet_size&gt;</code> with the number of IP addresses that you want to create in the portable subnet. Accepted values are 8, 16, 32, or 64. <p class="note"> When you add portable IP addresses for your subnet, three IP addresses are used to establish cluster-internal networking. You cannot use these three IP addresses for your Ingress application load balancers (ALBs) or to create network load balancer (NLB) services. For example, if you request eight portable public IP addresses, you can use five of them to expose your apps to the public.</p> </td>
    </tr>
    <tr>
    <td><code><em>&lt;VLAN_ID&gt;</em></code></td>
    <td>Replace <code>&lt;VLAN_ID&gt;</code> with the ID of the public or private VLAN on which you want to allocate the portable public or private IP addresses. You must select a public or private VLAN that an existing worker node is connected to. To review the public or private VLANs that your worker nodes are connected to, run <code>ibmcloud ks cluster get --cluster &lt;cluster&gt; --show-resources</code> and look for the <strong>Subnet VLANs</strong> section in the output. The subnet is provisioned in the same zone that the VLAN is in.</td>
    </tr>
    </tbody></table>

2. Verify that the subnet was successfully created and added to your cluster. The subnet CIDR is listed in the **Subnet VLANs** section.

    ```
    ibmcloud ks cluster get --cluster <cluster_name_or_ID> --show-resources
    ```
    {: pre}

    In this example output, a second subnet was added to the `2234945` public VLAN:
    ```
    Subnet VLANs
    VLAN ID   Subnet CIDR          Public   User-managed
    2234947   10.xxx.xx.xxx/29     false    false
    2234945   169.xx.xxx.xxx/29    true     false
    2234945   169.xx.xxx.xxx/29    true     false
    ```
    {: screen}

3. **Important**: To enable communication between workers that are on different subnets on the same VLAN, you must [enable routing between subnets on the same VLAN](#subnet-routing).

### Adding portable IPs by adding existing subnets to your cluster
{: #add-existing}

You can get more portable IPs for NLB services by making an existing subnet in an IBM Cloud infrastructure account available to your cluster.
{: shortdesc}

Before you begin:
-  Ensure that you have the [**Operator** or **Administrator** {{site.data.keyword.cloud_notm}} IAM platform access role](/docs/containers?topic=containers-users#platform) for the cluster.
- [Log in to your account. If applicable, target the appropriate resource group. Set the context for your cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

To make a subnet available to your cluster:

1. Review the IDs of the public or private VLANs on which you want to allocate the portable public or private IP addresses. You must select a public or private VLAN that an existing worker node is connected to.
  ```
  ibmcloud ks cluster get --cluster <cluster_name_or_id> --show-resources
  ```
  {: pre}

  In the output, look for **VLAN ID**s in the **Subnet VLANs** section. Example output:
  ```
  Subnet VLANs
  VLAN ID   Subnet CIDR          Public   User-managed
  2234947   10.xxx.xx.xxx/29     false    false
  2234945   169.xx.xxx.xxx/29    true     false
  ```
  {: screen}

2. Get the ID of the subnet to use. Ensure that the subnet is on one of the VLAN IDs that you found in the previous step, and that the subnet is not already bound to another cluster.
  ```
  ibmcloud ks subnets --provider classic
  ```
  {: pre}

  In this example output, the subnet ID is `1602829`, which is on the VLAN ID `2234945`:
  ```
  Getting subnet list...
  OK
  ID        Network             Gateway          VLAN ID   Type      Bound Cluster
  1550165   10.xxx.xx.xxx/26    10.xxx.xx.xxx    2234947   private
  1602829   169.xx.xxx.xxx/28   169.xx.xxx.xxx   2234945   public
  ```
  {: screen}

3. Make the subnet available to your cluster.
  ```
  ibmcloud ks cluster subnet add --cluster <cluster_name_or_id> --subnet-id <subnet_ID>
  ```
  {: pre}

4. Verify that the subnet was successfully created and added to your cluster. The subnet CIDR is listed in the **Subnet VLANs** section.
  ```
  ibmcloud ks cluster get --cluster <cluster_name> --show-resources
  ```
  {: pre}

  In this example output, a second subnet was added to the `2234945` public VLAN:
  ```
  Subnet VLANs
  VLAN ID   Subnet CIDR          Public   User-managed
  2234947   10.xxx.xx.xxx/29     false    false
  2234945   169.xx.xxx.xxx/29    true     false
  2234945   169.xx.xxx.xxx/29    true     false
  ```
  {: screen}

5. **Important**: To enable communication between workers that are on different subnets on the same VLAN, you must [enable routing between subnets on the same VLAN](#subnet-routing).

<br />

## Managing subnet routing
{: #subnet-routing}

In classic clusters, if you have multiple VLANs for your cluster, multiple subnets on the same VLAN, or a multizone classic cluster, you must enable a [Virtual Router Function (VRF)](/docs/account?topic=account-vrf-service-endpoint#vrf) for your IBM Cloud infrastructure account so your worker nodes can communicate with each other on the private network. To enable VRF, see [Enabling VRF](/docs/account?topic=account-vrf-service-endpoint#vrf). To check whether a VRF is already enabled, use the `ibmcloud account show` command. If you cannot or do not want to enable VRF, enable [VLAN spanning](/docs/vlans?topic=vlans-vlan-spanning#vlan-spanning). To perform this action, you need the **Network > Manage Network VLAN Spanning** [infrastructure permission](/docs/containers?topic=containers-users#infra_access), or you can request the account owner to enable it. To check whether VLAN spanning is already enabled, use the `ibmcloud ks vlan spanning get --region <region>` [command](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlan_spanning_get).

Review the following scenarios in which VLAN spanning is also required.

The VLAN spanning option is disabled for clusters that are created in a VRF-enabled account. When VRF is enabled, all VLANs in the account can automatically communicate with each other over the private network. For more information, see [Planning your cluster network setup: Worker-to-worker communication](/docs/containers?topic=containers-plan_clusters#worker-worker).
{: note}

### Enabling routing between primary subnets on the same VLAN
{: #vlan-spanning}

When you create a cluster, primary public and private subnets are provisioned on the public and private VLANs. The primary public subnet ends in `/28` and provides 14 public IPs for worker nodes. The primary private subnet ends in `/26` and provides private IPs for up to 62 worker nodes.
{: shortdesc}

You might exceed the initial 14 public and 62 private IPs for worker nodes by having a large cluster or several smaller clusters in the same location on the same VLAN. When a public or private subnet reaches the limit of worker nodes, another primary subnet in the same VLAN is ordered.

To ensure that workers in these primary subnets on the same VLAN can communicate, you must turn on VLAN spanning. For instructions, see [Enable or disable VLAN spanning](/docs/vlans?topic=vlans-vlan-spanning#vlan-spanning).

To check if VLAN spanning is already enabled, use the `ibmcloud ks vlan spanning get --region <region>` [command](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlan_spanning_get).
{: tip}

### Managing subnet routing for gateway appliances
{: #vra-routing}

When you create a cluster, a portable public and a portable private subnet are ordered on the VLANs that the cluster is connected to. These subnets provide IP addresses for Ingress application load balancer (ALB) and network load balancer (NLB) services.
{: shortdesc}

However, if you have an existing router appliance, such as a [Virtual Router Appliance (VRA)](/docs/virtual-router-appliance?topic=virtual-router-appliance-about-the-vra#about-the-vra), the newly added portable subnets from those VLANs that the cluster is connected to are not configured on the router. To use NLBs or Ingress ALBs, you must ensure that network devices can route between different subnets on the same VLAN by [enabling VLAN spanning](/docs/vlans?topic=vlans-vlan-spanning#vlan-spanning).

To check if VLAN spanning is already enabled, use the `ibmcloud ks vlan spanning get --region <region>` [command](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlan_spanning_get).
{: tip}

<br />


## Removing subnets from a cluster
{: #remove-subnets}

If you no longer need subnets, you can remove them from your cluster. After you remove the subnet, it is no longer available to your cluster, but it still exists in your IBM Cloud infrastructure account.
{: shortdesc}

**Before you begin**, review the following considerations:
* Subnets can only be detached from a cluster if none of the IP addresses derived from that subnet range are in use in your cluster.
* Portable public IP addresses are charged monthly. If you remove the subnet, you still must pay the monthly charge for the IP addresses, even if you used them only for a short amount of time.
* If your worker nodes previously used the subnet that you want to detach, but no workers are currently attached to any subnets on this subnet's VLAN, then the subnet is not visible to the cluster. You can instead cancel the subnet directly in the [{{site.data.keyword.cloud_notm}} Classic Infrastructure console](https://cloud.ibm.com/gen1/infrastructure/devices){: external}.

1. Find the CIDR for the subnet that you want to remove.
  ```
  ibmcloud ks cluster get --cluster <cluster_name> --show-resources
  ```
  {: pre}

  In this example output, the CIDR of the subnet to be removed is `169.1.1.1/29`.
  ```
  Subnet VLANs
  VLAN ID   Subnet CIDR          Public   User-managed
  2234947   10.xxx.xx.xxx/29     false    false
  2234945   169.xx.xxx.xxx/29    true     false
  2234945   169.1.1.1/29         true     false
  ```
  {: screen}

2. Using the CIDR that you found in the previous step, get the ID of the subnet to remove.
  ```
  ibmcloud ks subnets --provider classic
  ```
  {: pre}

  In this example output, the subnet with the `169.1.1.1/29` CIDR has the ID `1602829`.
  ```
  ID        Network             Gateway          VLAN ID   Type      Bound Cluster
  ...
  1602829   169.1.1.1/29        169.1.1.2        2234945   public    df253b6025d64944ab99ed63bb4567b6
  ```
  {: screen}

3. Detach the subnet from your cluster. The subnet remains available in your IBM Cloud infrastructure account.
  ```
  ibmcloud ks cluster subnet detach --cluster <cluster_name_or_ID> --subnet-id <subnet_ID>
  ```
  {: pre}

4. Verify that the subnet is no longer bound to your cluster.
  ```
  ibmcloud ks cluster get --cluster <cluster_name> --show-resources
  ```
  {: pre}

  In this example output, the subnet with the `169.1.1.1/29` CIDR is removed.
  ```
  Subnet VLANs
  VLAN ID   Subnet CIDR          Public   User-managed
  2234947   10.xxx.xx.xxx/29     false    false
  2234945   169.xx.xxx.xxx/29    true     false
  ```
  {: screen}


