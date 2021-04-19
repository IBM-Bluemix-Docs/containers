---

copyright:
  years: 2014, 2021
lastupdated: "2021-04-15"

keywords: kubernetes, iks, nginx, ingress controller, help

subcollection: containers
content-type: troubleshoot

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
 


# Debugging Ingress
{: #cs_troubleshoot_debug_ingress}

As you use {{site.data.keyword.containerlong}}, consider these techniques for general Ingress troubleshooting and debugging.
{: shortdesc}

While you troubleshoot, you can use the [{{site.data.keyword.containerlong_notm}} Diagnostics and Debug Tool](#debug-tool-ingress) to run tests and gather pertinent Ingress information from your cluster.
{: tip}

## Checking the status of Ingress components
{: #ingress-status}

**Infrastructure provider**:
  * <img src="images/icon-classic.png" alt="Classic infrastructure provider icon" width="15" style="width:15px; border-style: none"/> Classic
  * <img src="images/icon-vpc.png" alt="VPC infrastructure provider icon" width="15" style="width:15px; border-style: none"/> VPC Generation 2 compute

To check the overall health and status of your cluster's Ingress components:
{: shortdesc}

```
ibmcloud ks ingress status -c <cluster_name_or_ID>
```
{: pre}

The state of the Ingress components are reported in an **Ingress Status** and **Ingress Message**. Example output:
```
Ingress Status:   healthy
Message:          All Ingress components are healthy

Component                                        Status    Type
public-crdf253b6025d64944ab99ed63bb4567b6-alb1   healthy   alb
public-crdf253b6025d64944ab99ed63bb4567b6-alb2   healthy   alb
```
{: screen}

The **Ingress Status** and **Ingress Message** fields are also returned in the output of the `ibmcloud ks cluster get` command. The health of your Ingress components might impact the health of your cluster master. For example,  if your Ingress components are unhealthy, your cluster master might show a `warning` state. However, the health of your Ingress components does not cause your master health to become `critical`.
{: tip}

The **Ingress Status** reflects the overall health of the Ingress components. The **Ingress Message** provides details of what operation is in progress or information about any components that are unhealthy. Each status and message is described in the following tables.

|Ingress status|Description|
|--- |--- |
|`healthy`|The Ingress components are healthy. Check the **Ingress Message** field to verify that all operations for the Ingress components are complete.|
|`warning`|The Ingress components might not function properly due to errors. Check the **Ingress Message** field for more information and troubleshooting.|
{: caption="Ingress statuses"}
{: summary="Table rows read from left to right, with the Ingress status in column one and a description in column two."}

</br>

|Ingress message|Description|
|--- |--- |
|`ALB is disabled`|Your public ALBs were manually disabled. For more information, see the [`ibmcloud ks ingress alb enable` CLI command reference](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_configure).|
|`ALB is unhealthy or unreachable`|One or more ALB IP addresses cannot be reached. For troubleshooting information, see [Ping the ALB subdomain and public IP addresses](#ping).|
|`ALBs are not health checked in clusters created with no subnets`|Ingress health reporting is not supported for clusters that were created with the `--no-subnet` flag.|
|`ALBs are not health checked in private-only clusters`|Ingress health reporting is not supported for clusters that are connected to private VLANs only.|
|`ALBs cannot be created because no portable subnet is available`|Each ALB is created with a portable public or private IP address from the public or private subnet on the VLANs that your cluster is connected to. If no portable IP address is available, the ALB is not created. You might need to add a new subnet to your cluster or order a new VLAN. For troubleshooting information, see [Classic clusters: ALB does not deploy in a zone](#cs_subnet_limit).|
|`All Ingress components are healthy`|The Ingress components are successfully deployed and are healthy.|
|`Creating Ingress ALBs`|Your ALBs are currently deploying. Wait until your ALBs are fully deployed to review the health of your Ingress components. Note that ALB creation can take up to 15 minutes to complete. |
|`Creating TLS certificate for Ingress subdomain, which might take several minutes. Ensure you have the correct IAM permissions.` |The default Ingress subdomain for your cluster is created with a default TLS certificate, which is stored in the **Ingress Secret**. The certificate is currently being created and stored in the default {{site.data.keyword.cloudcerts_long_notm}} instance for your cluster.<p class="note">When the creation of the {{site.data.keyword.cloudcerts_short}} instance is triggered, the {{site.data.keyword.cloudcerts_short}} instance might take up to an hour to become visible in the {{site.data.keyword.cloud_notm}} console. If this message continues to be displayed, see [No Ingress secret exists after cluster creation](#ingress_secret).</p>|
|`Could not create a Certificate Manager instance. Ensure you have the correct IAM platform permissions.` | A default {{site.data.keyword.cloudcerts_long_notm}} instance for your cluster was not created to store the TLS certificate for the Ingress subdomain. The API key for the resource group and region that your cluster is in does not have the correct IAM permissions for {{site.data.keyword.cloudcerts_short}}. For troubleshooting steps, see [No Ingress secret exists after cluster creation](#ingress_secret).|
|`Could not upload certificates to Certificate Manager instance. Ensure you have the correct IAM service permissions.`|The TLS certificate for your cluster's default Ingress subdomain is created, but cannot be stored in the default {{site.data.keyword.cloudcerts_long_notm}} instance for your cluster. The API key for the resource group and region that your cluster is in does not have the correct IAM permissions for {{site.data.keyword.cloudcerts_short}}. For troubleshooting steps, see [No Ingress secret exists after cluster creation](#ingress_secret).|
|`Ingress is not supported for free clusters`|In a free cluster, you can expose your app only by using a [NodePort service](/docs/containers?topic=containers-nodeport).|
|`Ingress subdomain is unreachable`|Your cluster is assigned an Ingress subdomain in the format `<cluster_name>.<region>.containers.mybluemix.net` that cannot be reached. For troubleshooting information, see [Ping the ALB subdomain and public IP addresses](#ping).|
|`Load balancer service for ALB or router is not ready`|<ul><li>VPC clusters: The VPC load balancer that routes requests to the apps that your ALBs expose either might still be creating or did not correctly deploy to your VPC. For troubleshooting information, see [VPC clusters: Cannot connect to an app via Ingress](#vpc_ts_alb).</li><li>Classic clusters: The load balancer service that exposes your ALB did not correctly deploy to your cluster. For troubleshooting information, see [Classic clusters: ALB does not deploy in a zone](#cs_subnet_limit).</li></ul>|
|`One or more ALBs are unhealthy`|The external IP address for one or more of your ALBs was reported as unhealthy. For troubleshooting information, see [Ping the ALB subdomain and public IP addresses](#ping).|
|`Pending update or enable operation for ALB in progress`|Your ALB is currently updating to a new version, or your ALB that was previously disabled is enabling. For information about updating ALBs, see [Updating ALBs](/docs/containers?topic=containers-ingress#alb-update). For information about enabling ALBs, see the [`ibmcloud ks ingress alb enable` CLI command reference](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_configure).|
|`Registering Ingress subdomain`|The default **Ingress Subdomain** for your cluster is currently being created. The Ingress subdomain and secret creation follows a process that might take more than 15 minutes to complete. For troubleshooting information, see [No Ingress subdomain exists after cluster creation](#ingress_subdomain).|
|`The expiration dates reported by Ingress secrets are out of sync across namespaces.`| To resynchronize the expiration dates, [regenerate the secrets for your Ingress subdomain certificate](#sync_cert_dates).|
{: caption="Ingress messages"}
{: summary="Table rows read from left to right, with the Ingress message in column one and a description in column two."}

<br />


## No Ingress subdomain exists after cluster creation
{: #ingress_subdomain}
{: troubleshoot}
{: support}

**Infrastructure provider**:
  * <img src="images/icon-classic.png" alt="Classic infrastructure provider icon" width="15" style="width:15px; border-style: none"/> Classic
  * <img src="images/icon-vpc.png" alt="VPC infrastructure provider icon" width="15" style="width:15px; border-style: none"/> VPC Generation 2 compute

{: tsSymptoms}
You create a cluster and run `ibmcloud ks cluster get --cluster <cluster>` to check its status. The cluster **State** is `normal`, but the **Ingress Subdomain** and **Ingress Secret** are not available.

{: tsCauses}
Even if the cluster is in a `normal` state, the Ingress subdomain and secret might still be in progress. The Ingress subdomain and secret creation follows a process that might take more than 15 minutes to complete.

<img src="images/icon-classic.png" alt="Classic infrastructure provider icon" width="15" style="width:15px; border-style: none"/> **Classic clusters**:

1. When worker nodes are fully deployed and ready on the VLANs, a portable public and a portable private subnet for the VLANs are ordered.
2. After the portable subnet orders are successfully fulfilled, the `ibm-cloud-provider-vlan-ip-config` config map is updated with the portable public and portable private IP addresses.
3. When the `ibm-cloud-provider-vlan-ip-config` config map is updated, one public ALB per zone is triggered for creation.
4. A load balancer service that exposes the ALB is created and assigned an IP address.
5. The load balancer IP address is used to register the Ingress subdomain in Cloudflare. Cloudflare might have latency during the registration process.

If you create a classic cluster that is connected to private VLANs only, or if you create a free cluster, no Ingress subdomain or secret are created.
{: note}

<img src="images/icon-vpc.png" alt="VPC infrastructure provider icon" width="15" style="width:15px; border-style: none"/> **VPC clusters**:

1. When you create a VPC cluster, one public and one private VPC load balancer are automatically created outside of your cluster in your VPC.
2. One public ALB per zone is triggered for creation.
3. A load balancer service that exposes the ALB is created and assigned a hostname.
4. The load balancer hostname is used to register the Ingress subdomain in Cloudflare. Cloudflare might have latency during the registration process.

Creating a cluster after deleting a cluster the same or similar name? See [No Ingress subdomain exists after you create clusters of the same or similar name](#cs_rate_limit) instead.
{: tip}

{: tsResolve}
Typically, after the cluster is ready, the Ingress subdomain and secret are created after 15 minutes. If the Ingress subdomain and secret are still unavailable after your cluster is in a `normal` state for more than 15 minutes, you can check the progress of the creation process by following these steps:

1. Verify that the worker nodes have a **State** of `normal` and a **Status** of `Ready`. After you create the cluster, it can take up to 20 minutes for the worker nodes to be ready.
   ```
   ibmcloud ks worker ls -c <cluster_name_or_ID>
   ```
   {: pre}

   Example output:
   ```
   ID                                                     Public IP         Private IP      Flavor              State     Status   Zone    Version
   kube-blrs3b1d0p0p2f7haq0g-mycluster-default-000001f7   169.xx.xxx.xxx    10.xxx.xx.xxx   u3c.2x4.encrypted   deployed   Ready    dal10   1.19.9
   ```
   {: screen}

2. Verify that the prerequisite steps for your ALB creation are completed.
  * <img src="images/icon-classic.png" alt="Classic infrastructure provider icon" width="15" style="width:15px; border-style: none"/> **Classic clusters**: Get the details of the `ibm-cloud-provider-vlan-ip-config` config map.
    ```
    kubectl describe cm ibm-cloud-provider-vlan-ip-config -n kube-system
    ```
    {: pre}
    * If the config map shows IP addresses, continue to the next step.
    * If the **Events** section shows a warning message similar to `ErrorSubnetLimitReached: There are already the maximum number of subnets permitted in this VLAN`, see the [VLAN capacity troubleshooting topic](#cs_subnet_limit).

    Example output of a config map populated with IP addresses:
    ```
    Name:         ibm-cloud-provider-vlan-ip-config
    Namespace:    kube-system
    Labels:       <none>
    Annotations:  <none>

    Data
    ====
    reserved_public_vlan_id:
    ----

    vlanipmap.json:
    ----
    {
      "vlans": [
        {
          "id": "2234947",
          "subnets": [
            {
              "id": "2215454",
              "ips": [
                "10.XXX.XXX.XXX",
                "10.XXX.XXX.XXX",
                "10.XXX.XXX.XXX",
                "10.XXX.XXX.XXX",
                "10.XXX.XXX.XXX"
              ],
              "is_public": false,
              "is_byoip": false,
              "cidr": "10.XXX.XXX.X/29"
            }
          ],
          "zone": "dal10",
          "region": "us-south"
        },
        {
          "id": "2234945",
          "subnets": [
            {
              "id": "2219170",
              "ips": [
                "169.XX.XXX.XX",
                "169.XX.XXX.XX",
                "169.XX.XXX.XX",
                "169.XX.XXX.XX",
                "169.XX.XXX.XX"
              ],
              "is_public": true,
              "is_byoip": false,
              "cidr": "169.XX.XXX.X/29"
            }
          ],
          "zone": "dal10",
          "region": "us-south"
        }
      ],
      "vlan_errors": [],
      "reserved_ips": []
    }
    cluster_id:
    ----
    bmnj1b1d09lpvv3oof0g
    reserved_private_ip:
    ----

    reserved_private_vlan_id:
    ----

    reserved_public_ip:
    ----

    Events:  <none>
    ```
    {: screen}
  * <img src="images/icon-vpc.png" alt="VPC infrastructure provider icon" width="15" style="width:15px; border-style: none"/> **VPC clusters**: Verify that the VPC load balancer for your ALBs exists. In the output, look for the VPC load balancer **Name** that starts with `kube-crtmgr-<cluster_ID>`. If you did not install the `infrastructure-service` plug-in, install it by running `ibmcloud plugin install infrastructure-service`.
    ```
    ibmcloud is load-balancers
    ```
    {: pre}
    <p class="note">Even though the VPC load balancer is listed, its DNS entry might still be registering. When a VPC load balancer is created, the hostname is registered through a public DNS. In some cases, it can take several minutes for this DNS entry to be replicated to the specific DNS that your client is using.</p>

3. Check whether an ALB exists for your cluster and that the ALB has a public IP address assigned.
  ```
  ibmcloud ks ingress alb ls -c <cluster_name_or_ID>
  ```
  {: pre}

  Example output:
  ```
  ALB ID                                Enabled   Status     Type      ALB IP          Zone    Build                          ALB VLAN ID   NLB Version
  private-crbmnj1b1d09lpvv3oof0g-alb1   false     disabled   private   -               dal10   ingress:2469/ingress-auth:989   2234947       2.0
  public-crbmnj1b1d09lpvv3oof0g-alb1    true      enabled    public    169.XX.XXX.XX   dal10   ingress:2469/ingress-auth:989   2234945       2.0
  ```
  {: screen}
  * If a public ALB is listed and is assigned an IP address (classic clusters) or hostname (VPC clusters), continue to the next step.
  * If a public ALB is listed and but is not assigned an IP address (classic clusters) or hostname (VPC clusters), try to disable and re-enable the ALBs.
    * <img src="images/icon-classic.png" alt="Classic infrastructure provider icon" width="15" style="width:15px; border-style: none"/> Classic clusters:
      ```
      ibmcloud ks ingress alb disable --alb <ALB_ID> -c <cluster_name_or_ID>
      ```
      {: pre}
      ```
      ibmcloud ks ingress alb enable classic --alb <ALB_ID> -c <cluster_name_or_ID>
      ```
      {: pre}
    * <img src="images/icon-vpc.png" alt="VPC infrastructure provider icon" width="15" style="width:15px; border-style: none"/> VPC Gen 2 clusters:
      ```
      ibmcloud ks ingress alb disable --alb <ALB_ID> -c <cluster_name_or_ID>
      ```
      {: pre}
      ```
      ibmcloud ks ingress alb enable vpc-gen2 --alb <ALB_ID> -c <cluster_name_or_ID>
      ```
      {: pre}
  * If no ALBs are created after several minutes, [review ways to get help](/docs/containers?topic=containers-get-help).

4. Check whether the `LoadBalancer` service that exposes the ALB exists and is assigned the same IP address (classic clusters) or hostname (VPC clusters) as the public ALB.
  * If a `LoadBalancer` service is listed and is assigned an IP address (classic clusters) or hostname (VPC clusters), continue to the next step.
  * If no `LoadBalancer` services are created after several minutes, [review ways to get help](/docs/containers?topic=containers-get-help).

    ```
    kubectl get svc -n kube-system | grep LoadBalancer
    ```
    {: pre}

    Example output:
    ```
    public-crbmnj1b1d09lpvv3oof0g-alb1   LoadBalancer   172.21.XXX.XXX   169.XX.XXX.XX   80:30723/TCP,443:31241/TCP   1d
    ```
    {: screen}

5. Check again whether the Ingress subdomain and secret are created. If they are not available, but you verified that all of the components in steps 1 - 4 exist, [review ways to get help](/docs/containers?topic=containers-get-help).
  ```
  ibmcloud ks cluster get -c <cluster_name_or_ID>
  ```
  {: pre}

<br />

## No Ingress secret exists after cluster creation
{: #ingress_secret}

{: tsSymptoms}
When you run `ibmcloud ks ingress status -c <cluster_name_or_ID>`, one of the following messages continues to be displayed:
```
Creating TLS certificate for Ingress subdomain, which might take several minutes. Ensure you have the correct IAM permissions.
```
{: screen}
```
Could not upload certificates to Certificate Manager instance. Ensure you have the correct IAM service permissions.
```
{: screen}
```
Could not create a Certificate Manager instance. Ensure you have the correct IAM platform permissions.
```
{: screen}

When you run `ibmcloud ks ingress secret ls`, no secrets are listed.

{: tsCauses}
As of 24 August 2020, an [{{site.data.keyword.cloudcerts_long}}](/docs/certificate-manager?topic=certificate-manager-about-certificate-manager) instance is automatically created for each cluster that you can use to manage the cluster's Ingress TLS certificates.

For a {{site.data.keyword.cloudcerts_short}} instance to be created for your new or existing cluster, the API key for the region and resource group that the cluster is created in must have the correct IAM permissions. The API key that your cluster uses does not have the correct IAM permissions to create and access a {{site.data.keyword.cloudcerts_short}} instance.

Also, if you used the same cluster name repeatedly, you might have a rate limiting issue. For more information, see [No Ingress subdomain exists after you create clusters of the same or similar name](#cs_rate_limit).

{: tsResolve}
1. Check the ID of the user or functional user who sets the API key for this cluster.
  ```
  ibmcloud ks api-key info -c <cluster_name_or_ID>
  ```
  {: pre}
2. [Assign the following IAM permissions](/docs/containers?topic=containers-users#add_users) to the user or functional user who sets the API key.
  * The **Administrator** or **Editor** platform access role for {{site.data.keyword.cloudcerts_short}} in **All resource groups**
  * The **Manager** service access role for {{site.data.keyword.cloudcerts_short}} in **All resource groups**
3. The user must [reset the API key for the region and resource group](/docs/containers?topic=containers-users#api_key_most_cases).<p class="warning">When the API key is reset, the previous API key that was used for the region and resource group is deleted. Before you reset the API key, check whether you have other services that use the existing API key, such as a [key management service (KMS) provider](/docs/containers?topic=containers-encryption#keyprotect).</p>
4. After the cluster has access to the updated permissions in the API key, the creation of the {{site.data.keyword.cloudcerts_short}} instance is automatically triggered. Note that the {{site.data.keyword.cloudcerts_short}} instance might take up to an hour to become visible in the {{site.data.keyword.cloud_notm}} console.
5. Verify that your cluster is automatically assigned a {{site.data.keyword.cloudcerts_short}} instance.
  1. In the {{site.data.keyword.cloud_notm}} console, navigate to your [{{site.data.keyword.cloud_notm}} resource list](https://cloud.ibm.com/resources){: external}.
  2. Expand the **Services** row.
  3. Look for a {{site.data.keyword.cloudcerts_short}} instance that is named in the format `kube-crtmgr-<cluster_ID>`. To find your cluster's ID, run `ibmcloud ks cluster ls`.
  4. Click the instance's name. The **Your certificates** details page opens.
6. Verify that the TLS secret for your cluster's Ingress subdomain is created and listed in your cluster.
  ```
  ibmcloud ks ingress secret ls -c <cluster_name_or_ID>
  ```
  {: pre}

For more information, see [Managing TLS certificates and secrets](/docs/containers?topic=containers-ingress-types#manage_certs).

## Classic clusters: Cannot connect to an app via Ingress
{: #cs_ingress_fails}

**Infrastructure provider**:
* <img src="images/icon-classic.png" alt="Classic infrastructure provider icon" width="15" style="width:15px; border-style: none"/> Classic

{: tsSymptoms}
<img src="images/icon-classic.png" alt="Classic infrastructure provider icon" width="15" style="width:15px; border-style: none"/> You publicly exposed your app by creating an Ingress resource for your app in your classic cluster. When you tried to connect to your app by using the public IP address or Ingress subdomain, the connection failed or timed out.

{: tsResolve}
First, check that your cluster is fully deployed and has at least 2 worker nodes available per zone to ensure high availability for your ALB.
```
ibmcloud ks worker ls --cluster <cluster_name_or_ID>
```
{: pre}

In your CLI output, make sure that the **Status** of your worker nodes displays **Ready** and that the **Machine Type** shows a flavor other than **free**.

* If your standard cluster is fully deployed and has at least 2 worker nodes per zone, but no **Ingress Subdomain** is available, see [No Ingress subdomain exists after cluster creation](/docs/containers?topic=containers-cs_troubleshoot_debug_ingress#ingress_subdomain).
* For other issues, troubleshoot your Ingress setup by following the steps in [Debugging Ingress](#ingress-debug).

If you recently restarted your ALB pods or enabled an ALB, a [readiness check](/docs/containers?topic=containers-ingress#readiness-check) prevents ALB pods from attempting to route traffic requests until all of the Ingress resource files are parsed. This readiness check prevents request loss and can take up to 5 minutes.
{: note}

<br />



## VPC clusters: Cannot connect to an app via Ingress
{: #vpc_ts_alb}

**Infrastructure provider**: <img src="images/icon-vpc.png" alt="VPC infrastructure provider icon" width="15" style="width:15px; border-style: none"/> VPC Generation 2 compute

{: tsSymptoms}
<img src="images/icon-vpc.png" alt="VPC infrastructure provider icon" width="15" style="width:15px; border-style: none"/> You publicly exposed your app by creating an Ingress resource for your app in your VPC cluster. When you tried to connect to your app by using the subdomain of the Ingress application load balancer (ALB), the connection failed or timed out.

{: tsCauses}
When you create a VPC cluster, one public and one private VPC load balancer are automatically created outside of your cluster in your VPC. The VPC load balancer routes requests to the apps that the ALBs expose. Requests cannot be routed to your app in the following situations:
* A VPC security group is blocking incoming traffic to your worker nodes, including incoming requests to your app.
* The VPC load balancer is offline, such as due to load balancer provisioning errors or VSI connection errors.
* The VPC load balancer is deleted through the VPC console or the CLI.
* The VPC load balancer's DNS entry is still registering.

{: tsResolve}
Verify that no VPC security groups are blocking traffic to your cluster and that the VPC load balancer is available.
1. Install the `infrastructure-service` plug-in. The prefix for running commands is `ibmcloud is`.
  ```
  ibmcloud plugin install infrastructure-service
  ```
  {: pre}

2. <img src="images/icon-vpc.png" alt="VPC infrastructure provider icon" width="15" style="width:15px; border-style: none"/> VPC Gen 2 clusters that run Kubernetes version 1.18 or earlier: [Allow traffic requests that are routed by the VPC load balancer to node ports on your worker nodes](/docs/containers?topic=containers-vpc-network-policy#security_groups).

3. Verify that the VPC load balancer for your ALBs exists. In the output, look for the VPC load balancer **Name** that starts with `kube-crtmgr-<cluster_ID>`. If you did not install the `infrastructure-service` plug-in, install it by running `ibmcloud plugin install infrastructure-service`.
  ```
  ibmcloud is load-balancers
  ```
  {: pre}
  * If the VPC load balancer is not listed, it was deleted through the VPC console or the CLI. To re-create the VPC load balancer for your ALBs, disable all of the public or private ALBs that are assigned that VPC load balancer's hostname by running `ibmcloud ks ingress alb disable vpc-gen2 --alb <ALB_ID> -c <cluster_name_or_ID>` for each ALB. Then, re-enable those ALBs by running `ibmcloud ks ingress alb enable vpc-gen2 --alb <ALB_ID> -c <cluster_name_or_ID>` for each ALB. A new VPC load balancer for the ALBs takes a few minutes to provision in your VPC. You cannot access your app until the VPC load balancer for your ALBs is fully provisioned.
  * If the VPC load balancer is listed, its DNS entry might still be registering. When a VPC load balancer is created, the hostname is registered through a public DNS. In some cases, it can take several minutes for this DNS entry to be replicated to the specific DNS that your client is using. You can either wait for the hostname to be registered in your DNS, or access the VPC load balancer directly by using one of its IP addresses. To find the VPC load balancer IP addresses, run `ibmcloud is lb <LB_ID>` and look for the **Public IPs** field. If after several minutes you cannot reach the load balancer, it might be offline due to provisioning or connection issues. [Open an {{site.data.keyword.cloud_notm}} support case](https://cloud.ibm.com/unifiedsupport/cases/add). For the type, select **Technical**. For the category, select **Network** in the VPC section. In the description, include your cluster ID and the VPC load balancer ID.

<br />

## Debugging Ingress
{: #ingress-debug}
{: troubleshoot}
{: support}

**Infrastructure provider**: <img src="images/icon-classic.png" alt="Classic infrastructure provider icon" width="15" style="width:15px; border-style: none"/> Classic



{: tsSymptoms}
You publicly exposed your app by creating an Ingress resource for your app in your cluster. However, when you try to connect to your app through the Ingress subdomain or the ALBs' IP addresses, the connection fails or times out.

{: tsResolve}
The steps in the following sections can help you debug your Ingress setup.

Before you begin, ensure you have the following [{{site.data.keyword.cloud_notm}} IAM access policies](/docs/containers?topic=containers-users#platform) for {{site.data.keyword.containerlong_notm}}:
  - **Editor** or **Administrator** platform access role for the cluster
  - **Writer** or **Manager** service access role

### Step 1: Check your app deployment
{: #app-debug-ingress}

Before you debug Ingress, first check out [Debugging app deployments](/docs/containers?topic=containers-cs_troubleshoot_app#debug_apps).
{: shortdesc}

Ingress issues are often caused by underlying issues in your app deployment or in the `ClusterIP` service that exposes your app. For example, your app label and service selector might not match, or your app and service target ports might not match.

### Step 2: Run Ingress tests in the Diagnostics and Debug Tool
{: #debug-tool-ingress}

While you troubleshoot, you can use the {{site.data.keyword.containerlong_notm}} Diagnostics and Debug Tool to run Ingress tests and gather pertinent Ingress information from your cluster. To use the debug tool, you can enable the add-on in your cluster.
{: shortdesc}

1. In your [cluster dashboard](https://cloud.ibm.com/kubernetes/clusters){: external}, click the name of the cluster where you want to install the debug tool add-on.

2. Click the **Add-ons** tab.

3. On the Diagnostics and Debug Tool card, click **Install**.

4. In the dialog box, click **Install**. Note that it can take a few minutes for the add-on to be installed. <p class="tip">To resolve some common issues that you might encounter during the add-on deployment, see [Reviewing add-on state and statuses](/docs/containers?topic=containers-cs_troubleshoot_addons#debug_addons).</p>

5. On the Diagnostics and Debug Tool card, click **Dashboard**.

5. In the debug tool dashboard, select the **ingress** group of tests. Some tests check for potential warnings, errors, or issues, and some tests only gather information that you can reference while you troubleshoot. For more information about the function of each test, click the information icon next to the test's name.

6. Click **Run**.

7. Check the results of each test.
  * If any test fails, click the information icon next to the test's name in the left-hand column for information about how to resolve the issue.
  * You can also use the results of tests that only gather information while you debug your Ingress service in the following sections.

### Step 3: Check for error messages in your Ingress deployment and the ALB pod logs
{: #errors}

Start by checking for error messages in the Ingress resource deployment events and ALB pod logs. These error messages can help you find the root causes for failures and further debug your Ingress setup in the next sections.
{: shortdesc}

1. Check your Ingress resource deployment and look for warnings or error messages.
    ```
    kubectl describe ingress <myingress>
    ```
    {: pre}

    In the **Events** section of the output, you might see warning messages about invalid values in your Ingress resource or in certain annotations that you used. Check the [Ingress resource configuration documentation](/docs/containers?topic=containers-ingress#public_inside_4) or the [annotations documentation](/docs/containers?topic=containers-ingress_annotation).

    ```
    Name:             myingress
    Namespace:        default
    Address:          169.xx.xxx.xxx,169.xx.xxx.xxx
    Default backend:  default-http-backend:80 (<none>)
    Rules:
      Host                                             Path  Backends
      ----                                             ----  --------
      mycluster-<hash>-0000.us-south.containers.appdomain.cloud
                                                       /tea      myservice1:80 (<none>)
                                                       /coffee   myservice2:80 (<none>)
    Annotations:
      custom-port:        protocol=http port=7490; protocol=https port=4431
      location-modifier:  modifier='~' serviceName=myservice1;modifier='^~' serviceName=myservice2
    Events:
      Type     Reason             Age   From                                                            Message
      ----     ------             ----  ----                                                            -------
      Normal   Success            1m    public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Successfully applied ingress resource.
      Warning  TLSSecretNotFound  1m    public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Failed to apply ingress resource.
      Normal   Success            59s   public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Successfully applied ingress resource.
      Warning  AnnotationError    40s   public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Failed to apply ingress.bluemix.net/custom-port annotation. Error annotation format error : One of the mandatory fields not valid/missing for annotation ingress.bluemix.net/custom-port
      Normal   Success            40s   public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Successfully applied ingress resource.
      Warning  AnnotationError    2s    public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Failed to apply ingress.bluemix.net/custom-port annotation. Invalid port 7490. Annotation cannot use ports 7481 - 7490
      Normal   Success            2s    public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Successfully applied ingress resource.
    ```
    {: screen}
{: #check_pods}
2. Check the status of your ALB pods.
    1. Get the ALB pods that are running in your cluster.
        ```
        kubectl get pods -n kube-system | grep alb
        ```
        {: pre}

    2. Make sure that all pods are running by checking the **STATUS** column.

    3. If a pod does not have a `Running` status, you can disable and re-enable the ALB. In the following commands, replace `<ALB_ID>` with the ID of the pod's ALB. For example, if the pod that is not running has the name `public-crb2f60e9735254ac8b20b9c1e38b649a5-alb1-5d6d86fbbc-kxj6z`, the ALB ID is `public-crb2f60e9735254ac8b20b9c1e38b649a5-alb1`.

      When the pod restarts, a [readiness check](/docs/containers?topic=containers-ingress#readiness-check) prevents the ALB pod from attempting to route traffic requests until all of the Ingress resource files are parsed. This readiness check prevents request loss and can take up to 5 minutes by default.
      {: note}
      * <img src="images/icon-classic.png" alt="Classic infrastructure provider icon" width="15" style="width:15px; border-style: none"/> Classic clusters:
        ```
        ibmcloud ks ingress alb disable --alb <ALB_ID> -c <cluster_name_or_ID>
        ```
        {: pre}

        ```
        ibmcloud ks ingress alb enable classic --alb <ALB_ID> -c <cluster_name_or_ID>
        ```
        {: pre}
      * <img src="images/icon-vpc.png" alt="VPC infrastructure provider icon" width="15" style="width:15px; border-style: none"/> VPC Gen 2 clusters:
        ```
        ibmcloud ks ingress alb disable --alb <ALB_ID> -c <cluster_name_or_ID>
        ```
        {: pre}
        ```
        ibmcloud ks ingress alb enable vpc-gen2 --alb <ALB_ID> -c <cluster_name_or_ID>
        ```
        {: pre}

3. Check the logs for your ALB.
    1.  Get the IDs of the ALB pods that are running in your cluster.
        ```
        kubectl get pods -n kube-system | grep alb
        ```
        {: pre}

    3. Get the logs for the `nginx-ingress` container on each ALB pod.
        ```
        kubectl logs <ingress_pod_ID> nginx-ingress -n kube-system
        ```
        {: pre}

    4. Look for error messages in the ALB logs.

### Step 4: Ping the ALB subdomain and public IP addresses
{: #ping}

Check the availability of your Ingress subdomain and ALBs' public IP addresses. Additionally, ensure that the Cloudflare multizone load balancer can access your ALBs to health check them.
{: shortdesc}

1. Get the IP addresses (classic) or hostname (VPC) that your public ALBs are listening on.
    ```
    ibmcloud ks ingress alb ls --cluster <cluster_name_or_ID>
    ```
    {: pre}

    Example output for a classic multizone cluster with worker nodes in `dal10` and `dal13`:

    ```
    ALB ID                                            Enabled   Status     Type      ALB IP          Zone    Build                          ALB VLAN ID   NLB Version
    private-cr24a9f2caf6554648836337d240064935-alb1   false     disabled   private   -               dal13   ingress:2469/ingress-auth:989   2294021       -
    private-cr24a9f2caf6554648836337d240064935-alb2   false     disabled   private   -               dal10   ingress:2469/ingress-auth:989   2234947       -
    public-cr24a9f2caf6554648836337d240064935-alb1    true      enabled    public    169.62.196.238  dal13   ingress:2469/ingress-auth:989   2294019       -
    public-cr24a9f2caf6554648836337d240064935-alb2    true      enabled    public    169.46.52.222   dal10   ingress:2469/ingress-auth:989   2234945       -
    ```
    {: screen}

    * If a public ALB has no IP address(classic) or hostname (VPC), see [Ingress ALB does not deploy in a zone](/docs/containers?topic=containers-cs_troubleshoot_debug_ingress#cs_subnet_limit).

2. Verify that your ALB IP addresses are reachable by the ALB health check.
  * **Classic**: If you use Calico pre-DNAT network policies or another custom firewall to block incoming traffic to your cluster, you must allow inbound access on port 80 from the Kubernetes control plane and Cloudflare's IPv4 IP addresses to the IP addresses of your ALBs so that the Kubernetes control plane can check the health of your ALBs. For example, if you use Calico policies, [create a Calico pre-DNAT policy](/docs/containers?topic=containers-policy_tutorial#lesson3) to allow inbound access to your ALB IP addresses from [Cloudflare's IPv4 IP addresses](https://www.cloudflare.com/ips/){: external} on port 80 and [the IP addresses in step 6 of this section](/docs/containers?topic=containers-firewall#iam_allowlist).
  * **VPC**: If you set up [VPC security groups](/docs/openshift?topic=openshift-vpc-network-policy#security_groups) or [VPC access control lists (ACLs)](/docs/openshift?topic=openshift-vpc-network-policy#acls) to secure your cluster network, the necessary inbound access for ALB health checks is blocked. The Kubernetes control plane IP addresses are blocked from checking the health and reporting the overall status of your Ingress components. The Cloudflare IPv4 IP addresses are blocked from checking the health of the IP addresses of your ALB services. Because each ACL has a quota of only 50 rules and each security group has a quota of only 25 rules, you currently cannot add enough inbound and outbound rules for each Kubernetes control plane and Cloudflare IP address. Due to this known limitation your Ingress status shows that your ALBs are unreachable; however, this status does not reflect your ALB services' actual health. If you notice that traffic is not flowing correctly to your apps and you want to manually check your ALB services' health, you can continue to the next step. Alternatively, to allow the inbound traffic for ALB healthchecks, you can create a single rule to allow all incoming traffic on port 80.

3. Check the health of your ALB IPs (classic) or hostname (VPC).

    * For single zone cluster and multizone clusters: Ping the IP address (classic) or hostname (VPC) of each public ALB to ensure that each ALB is able to successfully receive packets. If you are using private ALBs, you can ping their IP addresses (classic) or hostname (VPC) only from the private network.
        ```
        ping <ALB_IP>
        ```
        {: pre}

        * If the CLI returns a timeout and you have a custom firewall that is protecting your worker nodes, make sure that you allow ICMP in your firewall.
        * If you don't have a firewall or your firewall does not block the pings and the pings still timeout, [check the status of your ALB pods](#check_pods).

    * Multizone clusters only: You can use the MZLB health check to determine the status of your ALB IPs (classic) or hostname (VPC). For more information about the MZLB, see [Multizone load balancer (MZLB)](/docs/containers?topic=containers-ingress-about#ingress_components). The following HTTP cURL command uses the `albhealth` host, which is configured by {{site.data.keyword.containerlong_notm}} to return the `healthy` or `unhealthy` status for an ALB IP.
        ```
        curl -X GET http://169.62.196.238/ -H "Host: albhealth.mycluster-<hash>-0000.us-south.containers.appdomain.cloud"
        ```
        {: pre}

        Example output:
        ```
        healthy
        ```
        {: screen}
        If one or more of the IPs returns `unhealthy`, [check the status of your ALB pods](#check_pods).

4. Get the IBM-provided Ingress subdomain.
    ```
    ibmcloud ks cluster get --cluster <cluster_name_or_ID> | grep Ingress
    ```
    {: pre}

    Example output:
    ```
    Ingress Subdomain:      mycluster-<hash>-0000.us-south.containers.appdomain.cloud
    Ingress Secret:         mycluster-<hash>-0000
    ```
    {: screen}

5. Ensure that the IPs (classic) or hostname (VPC) for each public ALB that you got in step 1 of this section are registered with your cluster's IBM-provided Ingress subdomain. For example, in a classic multizone cluster, the public ALB IP in each zone where you have worker nodes must be registered under the same subdomain.

    ```
    kubectl get ingress -o wide
    ```
    {: pre}

    Example output:
    ```
    NAME                HOSTS                                                    ADDRESS                        PORTS     AGE
    myingressresource   mycluster-<hash>-0000.us-south.containers.appdomain.cloud      169.46.52.222,169.62.196.238   80        1h
    ```
    {: screen}

### Step 5: Check your domain mappings and Ingress resource configuration
{: #ts_ingress_config}

1. If you use a custom domain, verify that you used your DNS provider to map the custom domain to the IBM-provided subdomain or the ALB's public IP address. Note that using a CNAME is preferred because IBM provides automatic health checks on the IBM subdomain and removes any failing IPs from the DNS response.
    * **IBM-provided subdomain CNAME**: Check that your custom domain is mapped to the cluster's IBM-provided subdomain in the Canonical Name record (CNAME).
        ```
        host www.my-domain.com
        ```
        {: pre}

        Example output:
        ```
        www.my-domain.com is an alias for mycluster-<hash>-0000.us-south.containers.appdomain.cloud
        mycluster-<hash>-0000.us-south.containers.appdomain.cloud has address 169.46.52.222
        mycluster-<hash>-0000.us-south.containers.appdomain.cloud has address 169.62.196.238
        ```
        {: screen}

    * **Public IP address A record**: Check that your custom domain is mapped to the ALB's portable public IP address in the A record. The IPs should match the public ALB IPs that you got in step 1 of the [previous section](#ping).
        ```
        host www.my-domain.com
        ```
        {: pre}

        Example output:
        ```
        www.my-domain.com has address 169.46.52.222
        www.my-domain.com has address 169.62.196.238
        ```
        {: screen}

2. Check the Ingress resource configuration files for your cluster.
    ```
    kubectl get ingress -o yaml
    ```
    {: pre}

    1. Ensure that you define a host in only one Ingress resource. If one host is defined in multiple Ingress resources, the ALB might not forward traffic properly and you might experience errors.

    2. Check that the subdomain and TLS certificate are correct. To find the IBM provided Ingress subdomain and TLS certificate, run `ibmcloud ks cluster get --cluster <cluster_name_or_ID>`.

    3.  Make sure that your app listens on the same path that is configured in the **path** section of your Ingress. If your app is set up to listen on the root path, use `/` as the path. If incoming traffic to this path must be routed to a different path that your app listens on, use the [rewrite paths](/docs/containers?topic=containers-ingress_annotation#rewrite-path) annotation.

    4. Edit your resource configuration YAML as needed. When you close the editor, your changes are saved and automatically applied.
        ```
        kubectl edit ingress <myingressresource>
        ```
        {: pre}

### Removing an ALB from DNS for debugging
{: #one_alb}

If you can't access your app through a specific ALB IP, you can temporarily remove the ALB from production by disabling its DNS registration. Then, you can use the ALB's IP address to run debugging tests on that ALB.

For example, say you have a multizone cluster in 2 zones, and the 2 public ALBs have IP addresses `169.46.52.222` and `169.62.196.238`. Although the health check is returning healthy for the second zone's ALB, your app isn't directly reachable through it. You decide to remove that ALB's IP address, `169.62.196.238`, from production for debugging. The first zone's ALB IP, `169.46.52.222`, is registered with your domain and continues to route traffic while you debug the second zone's ALB.

1. Get the name of the ALB with the unreachable IP address.
    ```
    ibmcloud ks ingress alb ls --cluster <cluster_name> | grep <ALB_IP>
    ```
    {: pre}

    For example, the unreachable IP `169.62.196.238` belongs to the ALB `public-cr24a9f2caf6554648836337d240064935-alb1`:
    ```
    ALB ID                                            Enabled   Status     Type      ALB IP           Zone    Build                          ALB VLAN ID   NLB Version
    public-cr24a9f2caf6554648836337d240064935-alb1    false     disabled   private   169.62.196.238   dal13   ingress:2469/ingress-auth:989   2294021       -
    ```
    {: screen}

2. Using the ALB name from the previous step, get the names of the ALB pods. The following command uses the example ALB name from the previous step:
    ```
    kubectl get pods -n kube-system | grep public-cr24a9f2caf6554648836337d240064935-alb1
    ```
    {: pre}

    Example output:
    ```
    public-cr24a9f2caf6554648836337d240064935-alb1-7f78686c9d-8rvtq   2/2       Running   0          24m
    public-cr24a9f2caf6554648836337d240064935-alb1-7f78686c9d-trqxc   2/2       Running   0          24m
    ```
    {: screen}

3. Disable the health check that runs for all ALB pods. Repeat these steps for each ALB pod that you got in the previous step. The example commands and output in these steps use the first pod, `public-cr24a9f2caf6554648836337d240064935-alb1-7f78686c9d-8rvtq`.
    1. Log in to the ALB pod and check the `server_name` line in the NGINX configuration file.
        ```
        kubectl exec -ti public-cr24a9f2caf6554648836337d240064935-alb1-7f78686c9d-8rvtq -n kube-system -c nginx-ingress -- grep server_name /etc/nginx/conf.d/kube-system-alb-health.conf
        ```
        {: pre}

        Example output that confirms the ALB pod is configured with the correct health check subdomain, `albhealth.<domain>`:
        ```
        server_name albhealth.mycluster-<hash>-0000.us-south.containers.appdomain.cloud;
        ```
        {: screen}

    2. To remove the IP by disabling the health check, insert `#` in front of the `server_name`. When the `albhealth.mycluster-<hash>-0000.us-south.containers.appdomain.cloud` virtual host is disabled for the ALB, the automated health check automatically removes the IP from the DNS response.
        ```
        kubectl exec -ti public-cr24a9f2caf6554648836337d240064935-alb1-7f78686c9d-8rvtq -n kube-system -c nginx-ingress -- sed -i -e 's*server_name*#server_name*g' /etc/nginx/conf.d/kube-system-alb-health.conf
        ```
        {: pre}

    3. Verify that the change was applied.
        ```
        kubectl exec -ti public-cr24a9f2caf6554648836337d240064935-alb1-7f78686c9d-8rvtq -n kube-system -c nginx-ingress -- grep server_name /etc/nginx/conf.d/kube-system-alb-health.conf
        ```
        {: pre}

        Example output:
        ```
        #server_name albhealth.mycluster-<hash>-0000.us-south.containers.appdomain.cloud
        ```
        {: screen}

    4. To remove the IP from the DNS registration, reload the NGINX configuration.
        ```
        kubectl exec -ti public-cr24a9f2caf6554648836337d240064935-alb1-7f78686c9d-8rvtq -n kube-system -c nginx-ingress -- nginx -s reload
        ```
        {: pre}

    5. Repeat these steps for each ALB pod.

4. Now, when you attempt to cURL the `albhealth` host to health check the ALB IP, the check fails.
    ```
    curl -X GET http://169.62.196.238/ -H "Host: albhealth.mycluster-<hash>-0000.us-south.containers.appdomain.cloud"
    ```
    {: pre}

    Output:
    ```
    <html>
        <head>
            <title>404 Not Found</title>
        </head>
        <body bgcolor="white"><center>
            <h1>404 Not Found</h1>
        </body>
    </html>
    ```
    {: screen}

5. Verify that the ALB IP address is removed from the DNS registration for your domain by checking the Cloudflare server. Note that the DNS registration might take a few minutes to update.
    ```
    host mycluster-<hash>-0000.us-south.containers.appdomain.cloud ada.ns.cloudflare.com
    ```
    {: pre}

    Example output that confirms that only the healthy ALB IP, `169.46.52.222`, remains in the DNS registration and that the unhealthy ALB IP, `169.62.196.238`, has been removed:
    ```
    mycluster-<hash>-0000.us-south.containers.appdomain.cloud has address 169.46.52.222
    ```
    {: screen}

6. Now that the ALB IP has been removed from production, you can run debugging tests against your app through it. To test communication to your app through this IP, you can run the following cURL command, replacing the example values with your own values:
    ```
    curl -X GET --resolve mycluster-<hash>-0000.us-south.containers.appdomain.cloud:443:169.62.196.238 https://mycluster-<hash>-0000.us-south.containers.appdomain.cloud/
    ```
    {: pre}

    * If everything is configured correctly, you get back the expected response from your app.
    * If you get an error in response, there might be an error in your app or in a configuration that applies only to this specific ALB. Check your app code, your [Ingress resource configuration files](/docs/containers?topic=containers-ingress#public_inside_4), or any other configurations you have applied to only this ALB.

7. After you finish debugging, restore the health check on the ALB pods. Repeat these steps for each ALB pod.
  1. Log in to the ALB pod and remove the `#` from the `server_name`.
    ```
    kubectl exec -ti <pod_name> -n kube-system -c nginx-ingress -- sed -i -e 's*#server_name*server_name*g' /etc/nginx/conf.d/kube-system-alb-health.conf
    ```
    {: pre}

  2. Reload the NGINX configuration so that the health check restoration is applied.
    ```
    kubectl exec -ti <pod_name> -n kube-system -c nginx-ingress -- nginx -s reload
    ```
    {: pre}

9. Now, when you cURL the `albhealth` host to health check the ALB IP, the check returns `healthy`.
    ```
    curl -X GET http://169.62.196.238/ -H "Host: albhealth.mycluster-<hash>-0000.us-south.containers.appdomain.cloud"
    ```
    {: pre}

10. Verify that the ALB IP address has been restored in the DNS registration for your domain by checking the Cloudflare server. Note that the DNS registration might take a few minutes to update.
    ```
    host mycluster-<hash>-0000.us-south.containers.appdomain.cloud ada.ns.cloudflare.com
    ```
    {: pre}

    Example output:
    ```
    mycluster-<hash>-0000.us-south.containers.appdomain.cloud has address 169.46.52.222
    mycluster-<hash>-0000.us-south.containers.appdomain.cloud has address 169.62.196.238
    ```
    {: screen}

<br />

## Ingress application load balancer (ALB) secret issues
{: #cs_albsecret_fails}

**Infrastructure provider**:
  * <img src="images/icon-classic.png" alt="Classic infrastructure provider icon" width="15" style="width:15px; border-style: none"/> Classic
  * <img src="images/icon-vpc.png" alt="VPC infrastructure provider icon" width="15" style="width:15px; border-style: none"/> VPC Generation 2 compute



{: tsSymptoms}
After you deploy an Ingress application load balancer (ALB) secret to your cluster by using the `ibmcloud ks ingress secret create` command, the `Description` field is not updating with the secret name when you view your certificate in {{site.data.keyword.cloudcerts_full_notm}}.

When you list information about the ALB secret, the state says `*_failed`. For example, `create_failed`, `update_failed`, `delete_failed`.

List the ALB secret details (`ibmcloud ks ingress secretget`) and view the ALB secret `status` to get more information on the reason for failure.

{: tsResolve}
Review the following reasons why the ALB secret might fail and the corresponding troubleshooting steps:

<table summary="The columns are read from left to right. The first column describes why the issue happens. The second column provides steps to resolve the error.">
<caption>Troubleshooting Ingress application load balancer secrets</caption>
 <col width="25%">
 <thead>
 <th>Why it's happening</th>
 <th>How to fix it</th>
 </thead>
 <tbody>
 <tr>
 <td>The owner of the cluster's API Key does not have the required access roles to download and update certificate data.</td>
 <td>Check with your account Administrator to assign the owner of the cluster's API Key, the following {{site.data.keyword.cloud_notm}} IAM roles:<ul><li>The **Manager** and **Writer** service access roles for your {{site.data.keyword.cloudcerts_full_notm}} instance. For more information, see <a href="/docs/certificate-manager?topic=certificate-manager-managing-service-access-roles#managing-service-access-roles">Managing service access</a> for {{site.data.keyword.cloudcerts_short}}.</li><li>The <a href="/docs/containers?topic=containers-users#platform">**Administrator** platform access role</a> for the cluster.</li></ul></td>
 </tr>
 <tr>
 <td>The certificate CRN provided at time of create, update, or remove does not belong to the same account as the cluster.</td>
 <td>Check that the certificate CRN you provided is imported to an instance of the {{site.data.keyword.cloudcerts_short}} service that is deployed in the same account as your cluster.</td>
 </tr>
 <tr>
 <td>The certificate CRN provided at time of create is incorrect.</td>
 <td><ol><li>Check the accuracy of the certificate CRN string you provide.</li><li>If the certificate CRN is found to be accurate, then try to update the secret: <code>ibmcloud ks ingress secret update --cluster &lt;cluster_name_or_ID&gt; --name &lt;secret_name&gt; --namespace &lt;namespace&gt; --cert-crn &lt;certificate_CRN&gt;</code></li><li>If this command results in the <code>update_failed</code> status, then remove the secret: <code>ibmcloud ks ingress secret rm --cluster &lt;cluster_name_or_ID&gt; --name &lt;secret_name&gt; --namespace &lt;namespace&gt;</code></li><li>Deploy the secret again: <code>ibmcloud ks ingress secret create --cluster &lt;cluster_name_or_ID&gt; --name &lt;secret_name&gt; --cert-crn &lt;certificate_CRN&gt; --namespace &lt;namespace&gt;</code></li></ol></td>
 </tr>
 <tr>
 <td>The certificate CRN provided at time of update is incorrect.</td>
 <td><ol><li>Check the accuracy of the certificate CRN string you provide.</li><li>If the certificate CRN is found to be accurate, then remove the secret: <code>ibmcloud ks ingress secret rm --cluster &lt;cluster_name_or_ID&gt; --name &lt;secret_name&gt; --namespace &lt;namespace&gt;</code></li><li>Deploy the secret again: <code>ibmcloud ks ingress secret create --cluster &lt;cluster_name_or_ID&gt; --name &lt;secret_name&gt; --cert-crn &lt;certificate_CRN&gt; --namespace &lt;namespace&gt;</code></li><li>Try to update the secret: <code>ibmcloud ks ingress secret update --cluster &lt;cluster_name_or_ID&gt; --name &lt;secret_name&gt; --namespace &lt;namespace&gt; --cert-crn &lt;certificate_CRN&gt;</code></li></ol></td>
 </tr>
 <tr>
 <td>The {{site.data.keyword.cloudcerts_long_notm}} service is experiencing downtime.</td>
 <td>Check that your {{site.data.keyword.cloudcerts_short}} service is up and running.</td>
 </tr>
 <tr>
 <td>Your imported secret has the same name as the IBM-provided Ingress secret.</td>
 <td>Rename your secret. You can check the name of the IBM-provided Ingress secret by running `ibmcloud ks cluster get --cluster <cluster_name_or_ID> | grep Ingress`.</td>
 </tr>
  <tr>
  <td>The description for the certificate is not updated with the secret name when you view the certificate in {{site.data.keyword.cloudcerts_full_notm}}.</td>
  <td>Check whether the length of the certificate description reached the <a href="/apidocs/certificate-manager#update-a-certificate-s-metadata">upper limit of 1024 characters</a>.</td>
  </tr>
 </tbody></table>

<br />

## ALB pods do not deploy to worker nodes
{: #alb-pod-affinity}

**Infrastructure provider**:
  * <img src="images/icon-classic.png" alt="Classic infrastructure provider icon" width="15" style="width:15px; border-style: none"/> Classic
  * <img src="images/icon-vpc.png" alt="VPC infrastructure provider icon" width="15" style="width:15px; border-style: none"/> VPC Generation 2 compute



{: tsSymptoms}
When you run `kubectl get pods -n kube-system | grep alb`, either no ALB pods or only some ALB pods successfully deployed to your worker nodes. When you describe an ALB pod by running `kubectl describe pod -n kube-system <pod_name>`, you see a message similar to the following in the **Events** section of the output.
```
0/3 nodes are available: 1 node(s) didn’t match pod affinity/anti-affinity, 2 node(s) didn’t match node selector.
```
{: screen}

{: tsCauses}
Ingress requires at least two worker nodes per zone to ensure high availability and that periodic updates are applied. By default, ALB pods have two replicas. However, ALB pods have anti-affinity rules to ensure that only one pod is scheduled to each worker node for high availability. When only one worker node exists per zone in your cluster, ALB pods cannot deploy correctly.

{: tsResolve}
The method to increase the number of worker nodes per zone depends on whether you restrict network traffic to edge worker nodes.
* **If you do not use edge nodes**: Ensure that at least two worker nodes exist in each zone by [resizing an existing worker pool](/docs/containers?topic=containers-add_workers#resize_pool), [creating a new worker pool in a VPC cluster](/docs/containers?topic=containers-add_workers#vpc_add_pool), or [creating a new worker pool in a classic cluster](/docs/containers?topic=containers-add_workers#add_pool).
* **If you use edge nodes**: Ensure that at least two [edge worker nodes](/docs/containers?topic=containers-edge) are enabled in each zone.

After the new worker nodes deploy, the ALB pods are automatically scheduled to deploy to those worker nodes.
<br />

## Classic clusters: ALB does not deploy in a zone
{: #cs_subnet_limit}

**Infrastructure provider**: <img src="images/icon-classic.png" alt="Classic infrastructure provider icon" width="15" style="width:15px; border-style: none"/> Classic



{: tsSymptoms}
When you have a multizone classic cluster and run `ibmcloud ks ingress alb ls --cluster <cluster>`, no ALB is deployed in a zone. For example, if you have worker nodes in 3 zones, you might see an output similar to the following in which a public ALB did not deploy to the third zone.
```
ALB ID                                            Enabled    Status     Type      ALB IP           Zone    Build                          ALB VLAN ID   NLB Version
private-cr96039a75fddb4ad1a09ced6699c88888-alb1   false      disabled   private   -                dal10   ingress:2469/ingress-auth:989   2294021       -
private-cr96039a75fddb4ad1a09ced6699c88888-alb2   false      disabled   private   -                dal12   ingress:2469/ingress-auth:989   2234947       -
private-cr96039a75fddb4ad1a09ced6699c88888-alb3   false      disabled   private   -                dal13   ingress:2469/ingress-auth:989   2234943       -
public-cr96039a75fddb4ad1a09ced6699c88888-alb1    true       enabled    public    169.xx.xxx.xxx   dal10   ingress:2469/ingress-auth:989   2294019       -
public-cr96039a75fddb4ad1a09ced6699c88888-alb2    true       enabled    public    169.xx.xxx.xxx   dal12   ingress:2469/ingress-auth:989   2234945       -
```
{: screen}

{: tsCauses}
In standard clusters, the first time that you create a cluster in a zone, a public VLAN and a private VLAN in that zone are automatically provisioned for you in your IBM Cloud infrastructure account. In that zone, 1 public portable subnet is requested on the public VLAN that you specify and 1 private portable subnet is requested on the private VLAN that you specify. For {{site.data.keyword.containerlong_notm}}, VLANs have a limit of 40 subnets. If the cluster's VLAN in a zone already reached that limit, the **Ingress Subdomain** fails to provision and the public Ingress ALB for that zone fails to provision.

To view how many subnets a VLAN has:
1.  From the [IBM Cloud infrastructure console](https://cloud.ibm.com/classic?), select **Network** > **IP Management** > **VLANs**.
2.  Click the **VLAN Number** of the VLAN that you used to create your cluster. Review the **Subnets** section to see whether 40 or more subnets exist.

{: tsResolve}
Option 1: If you need a new VLAN, order one by [contacting {{site.data.keyword.cloud_notm}} support](/docs/vlans?topic=vlans-ordering-premium-vlans#ordering-premium-vlans). Then, [create a cluster](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_create) that uses this new VLAN.

Option 2: If you have another VLAN that is available, you can [set up VLAN spanning](/docs/vlans?topic=vlans-vlan-spanning#vlan-spanning) in your existing cluster. After, you can add new worker nodes to the cluster that use the other VLAN with available subnets. To check if VLAN spanning is already enabled, use the `ibmcloud ks vlan spanning get --region <region>` [command](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlan_spanning_get).

Option 3: If you are not using all the subnets in the VLAN, you can reuse subnets on the VLAN by adding them to your cluster.
1. Check that the subnet that you want to use is available.
  <p class="note">The infrastructure account that you use might be shared across multiple {{site.data.keyword.cloud_notm}} accounts. In this case, even if you run the `ibmcloud ks subnets` command to see subnets with **Bound Clusters**, you can see information only for your clusters. Check with the infrastructure account owner to make sure that the subnets are available and not in use by any other account or team.</p>

2. Use the [`ibmcloud ks cluster subnet add` command](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_subnet_add) to make an existing subnet available to your cluster.

3. Verify that the subnet was successfully created and added to your cluster. The subnet CIDR is listed in the **Subnet VLANs** section.
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

4. Verify that the portable IP addresses from the subnet that you added are used for the ALBs in your cluster. It might take several minutes for the services to use the portable IP addresses from the newly-added subnet.
  * **No Ingress subdomain**: Run `ibmcloud ks cluster get --cluster <cluster>` to verify that the **Ingress Subdomain** is populated.
  * **An ALB does not deploy in a zone**: Run `ibmcloud ks ingress alb ls --cluster <cluster>` to verify that the missing ALB is deployed.

<br />

## Ingress ALB cannot be enabled due to subnet errors
{: #cs_alb_subnet}

**Infrastructure provider**:
  * <img src="images/icon-classic.png" alt="Classic infrastructure provider icon" width="15" style="width:15px; border-style: none"/> Classic
  * <img src="images/icon-vpc.png" alt="VPC infrastructure provider icon" width="15" style="width:15px; border-style: none"/> VPC Generation 2 compute



{: tsSymptoms}
When you try to enable an Ingress ALB by running the `ibmcloud ks ingress alb enable` command, you see the following error:
```
No valid subnets found for the specified zone. Verify that a subnet exists on the VLAN in the zone that you specify by running 'ibmcloud ks subnets'. Note: If the problem persists, verify that your ALBs and worker nodes are on the same VLANs by following the steps in this troubleshooting doc: <https://ibm.biz/alb-vlan-ts>
```
{: screen}

However, you ran `ibmcloud ks ks subnets` and verified that one or more subnets are available on the VLAN in the zone where the ALB exists.

{: tsCauses}
Your ALBs and your worker nodes might not exist on the same VLANs. This can occur when you delete worker nodes on the VLANs that the ALBs were also originally created on, and then create new worker nodes on new VLANs.

{: tsResolve}
Move your ALBs to the same VLANs that your worker nodes exist on by following the steps in [Moving ALBs across VLANs](/docs/containers?topic=containers-ingress#migrate-alb-vlan).

## Source IP preservation fails when using tainted nodes
{: #cs_source_ip_fails}

**Infrastructure provider**:
  * <img src="images/icon-classic.png" alt="Classic infrastructure provider icon" width="15" style="width:15px; border-style: none"/> Classic
  * <img src="images/icon-vpc.png" alt="VPC infrastructure provider icon" width="15" style="width:15px; border-style: none"/> VPC Generation 2 compute



{: tsSymptoms}
You enabled source IP preservation for an [Ingress ALB](/docs/containers?topic=containers-ingress_annotation#preserve_source_ip) service by changing `externalTrafficPolicy` to `Local` in the service's configuration file. However, no traffic reaches the back-end service for your app.

{: tsCauses}
When you enable source IP preservation for Ingress ALB services, the source IP address of the client request is preserved. The service forwards traffic to app pods on the same worker node only to ensure that the request packet's IP address isn't changed. Typically, Ingress ALB service pods are deployed to the same worker nodes that the app pods are deployed to. However, some situations exist where the service pods and app pods might not be scheduled onto the same worker node. If you use [Kubernetes taints](https://kubernetes.io/docs/concepts/scheduling-eviction/taint-and-toleration/){: external} on worker nodes, any pods that don't have a taint toleration are prevented from running on the tainted worker nodes. Source IP preservation might not be working based on the type of taint you used:

* **Edge node taints**: You [added the `dedicated=edge` label](/docs/containers?topic=containers-edge#edge_nodes) to two or more worker nodes on each public VLAN in your cluster to ensure that Ingress pods deploy to those worker nodes only. Then, you also [tainted those edge nodes](/docs/containers?topic=containers-edge#edge_workloads) to prevent any other workloads from running on edge nodes. However, you didn't add an edge node affinity rule and toleration to your app deployment. Your app pods can't be scheduled on the same tainted nodes as the service pods, and no traffic reaches the back-end service for your app.

* **Custom taints**: You used custom taints on several nodes so that only app pods with that taint toleration can deploy to those nodes. You added affinity rules and tolerations to the deployments of your app and Ingress service so that their pods deploy to only those nodes. However, `ibm-cloud-provider-ip` `keepalived` pods that are automatically created in the `ibm-system` namespace ensure that the ALB pods and the app pods are always scheduled onto the same worker node. These `keepalived` pods don't have the tolerations for the custom taints that you used. They can't be scheduled on the same tainted nodes that your app pods are running on, and no traffic reaches the back-end service for your app.

{: tsResolve}
Resolve the issue by choosing one of the following options:

* **Edge node taints**: To ensure that your ALB and app pods deploy to tainted edge nodes, [add edge node affinity rules and tolerations to your app deployment](/docs/containers?topic=containers-loadbalancer#lb_edge_nodes). Ingress ALB pods have these affinity rules and tolerations by default.

* **Custom taints**: Remove custom taints that the `keepalived` pods don't have tolerations for. Instead, you can [label worker nodes as edge nodes, and then taint those edge nodes](/docs/containers?topic=containers-edge).

If you complete one of the above options but the `keepalived` pods are still not scheduled, you can get more information about the `keepalived` pods:

1. Get the `keepalived` pods.
    ```
    kubectl get pods -n ibm-system
    ```
    {: pre}

2. In the output, look for `ibm-cloud-provider-ip` pods that have a **Status** of `Pending`. Example:
    ```
    ibm-cloud-provider-ip-169-61-XX-XX-55967b5b8c-7zv9t     0/1       Pending   0          2m        <none>          <none>
    ibm-cloud-provider-ip-169-61-XX-XX-55967b5b8c-8ptvg     0/1       Pending   0          2m        <none>          <none>
    ```
    {:screen}

3. Describe each `keepalived` pod and look for the **Events** section. Address any error or warning messages that are listed.
    ```
    kubectl describe pod ibm-cloud-provider-ip-169-61-XX-XX-55967b5b8c-7zv9t -n ibm-system
    ```
    {: pre}

<br />

## No Ingress subdomain exists after you create clusters of the same or similar name
{: #cs_rate_limit}

**Infrastructure provider**:
  * <img src="images/icon-classic.png" alt="Classic infrastructure provider icon" width="15" style="width:15px; border-style: none"/> Classic
  * <img src="images/icon-vpc.png" alt="VPC infrastructure provider icon" width="15" style="width:15px; border-style: none"/> VPC Generation 2 compute

{: tsSymptoms}
You create and delete a cluster multiple times, such as for automation purposes, and you either use the same name for the cluster every time that you create it, or a name that is very similar to previous names that you used. When you run `ibmcloud ks cluster get --cluster <cluster>`, your cluster is in a `normal` state but no **Ingress Subdomain** or **Ingress Secret** are available.

{: tsCauses}
When you create and delete a cluster that uses the same name multiple times, the Ingress subdomain for that cluster in the format `<cluster_name>.<globally_unique_account_HASH>-0000.<region>.containers.appdomain.cloud` is registered and unregistered each time. The certificate for the subdomain is also generated and deleted each time. If you create and delete a cluster with the same name 5 times or more within 7 days, you might reach the Let's Encrypt [Duplicate Certificate rate limit](https://letsencrypt.org/docs/rate-limits/?origin_team=T4LT36D1N){: external}, because the same Ingress subdomain and certificate are registered every time that you create the cluster. Because very long cluster names are truncated to 24 characters in the Ingress subdomain for the cluster, you can also reach the rate limit if you use multiple cluster names that have the same first 24 characters.

{: tsResolve}
If you need to continue testing, you can change the name of the cluster so that when you create the new cluster a new, different Ingress subdomain and secret are registered.

<br />

## Ingress secret expiration date is not updated
{: #sync_cert_dates}

**Infrastructure provider**:
  * <img src="images/icon-classic.png" alt="Classic infrastructure provider icon" width="15" style="width:15px; border-style: none"/> Classic
  * <img src="images/icon-vpc.png" alt="VPC infrastructure provider icon" width="15" style="width:15px; border-style: none"/> VPC Generation 2 compute

{: tsSymptoms}
When you run `ibmcloud ks cluster get -c <cluster_name_or_ID>` or `ibmcloud ks ingress status -c <cluster_name_or_ID>`, you see the following **Ingress Message**:

```
The expiration dates reported by Ingress secrets are out of sync across namespaces. To resynchronize the expiration dates, see http://ibm.biz/ingress-secret-sync
```
{: screen}

When you run `ibmcloud ks ingress secret ls -c <cluster_name_or_ID>`, you notice that old expiration dates are listed for some certificates.

{: tsCauses}
In some cases, the expiration date that is reported by the secrets in some namespaces in your cluster can become out of sync with the expiration date of secrets for the same certificate in other namespaces. Even though your actual certificate is renewed, a secret in your cluster can show an older expiration date that is not updated to the most recent expiration date for the certificate. However, a secret for this certificate in another namespace might show the correct expiration date.

{: tsResolve}
To resynchronize the expiration dates, you can regenerate the secrets for your Ingress subdomain certificate.

1. Get your **Ingress Subdomain**.
  ```
  ibmcloud ks cluster get --cluster <cluster_name_or_ID> | grep 'Ingress Subdomain'
  ```
  {: pre}

2. Regenerate the Ingress subdomain secret. The certificate is renewed, a new expiration date is generated, and the updates are synchronized across the secret in different namespaces. Secret regeneration is not disruptive, and traffic continues to flow while the secret regenerates. Note that it might take up to 30 minutes for the secret regeneration to complete.
  ```
  ibmcloud ks nlb-dns secret regenerate -c <cluster_name_or_ID> --nlb-subdomain <ingress_subdomain>
  ```
  {: pre}

3. List all secrets associated with the certificate for your Ingress subdomain. In the output, verify that the **Expiration date** for the secrets are more than 30 days later than today.
  ```
  ibmcloud ks ingress secret ls -c <cluster_name_or_ID> | grep <ingress_subdomain>
  ```
  {: pre}

  Example output:
  ```
  Name                                              Namespace        CRN                                                                                                                                                           Expires On                 Domain                                                                                  Status
  mycluster-35366fb2d3d90fd50548180f69e7d12a-0000   default          crn:v1:bluemix:public:cloudcerts:eu-de:a/6ef045fd2b43266cfe8e6388dd2ec098:4ebc1d51-8a74-4675-8c4c-b2922ceba2d4:certificate:70f08c8a0fc1eed1f147b28443ba6dcd   2020-12-10T18:00:58+0000   mycluster-35366fb2d3d90fd50548180f69e7d12a-0000.eu-de.containers.appdomain.cloud     created
  mycluster-35366fb2d3d90fd50548180f69e7d12a-0000   ibm-cert-store   crn:v1:bluemix:public:cloudcerts:eu-de:a/6ef045fd2b43266cfe8e6388dd2ec098:4ebc1d51-8a74-4675-8c4c-b2922ceba2d4:certificate:70f08c8a0fc1eed1f147b28443ba6dcd   2020-12-10T18:00:58+0000   *.mycluster-35366fb2d3d90fd50548180f69e7d12a-0000.eu-de.containers.appdomain.cloud   created
  mycluster-35366fb2d3d90fd50548180f69e7d12a-0000   kube-system      crn:v1:bluemix:public:cloudcerts:eu-de:a/6ef045fd2b43266cfe8e6388dd2ec098:4ebc1d51-8a74-4675-8c4c-b2922ceba2d4:certificate:70f08c8a0fc1eed1f147b28443ba6dcd   2020-12-10T18:00:58+0000   *.mycluster-35366fb2d3d90fd50548180f69e7d12a-0000.eu-de.containers.appdomain.cloud   created
  ```
  {: screen}

4. Optional: Other secrets for network load balancer (NLB) subdomains in your cluster might report expiration dates that are out of sync with the certificate's expiration date. You can run the following steps to resynchronize the expiration dates for other secrets in your cluster.
  1. List all NLB subdomains in your cluster.
    ```
    ibmcloud ks nlb-dns ls -c <cluster_name_or_ID>
    ```
    {: pre}
  2. For each subdomain besides the Ingress subdomain, regenerate its secret. The certificate is renewed, a new expiration date is generated, and the updates are synchronized across the secret in different namespaces. Secret regeneration is not disruptive, and traffic continues to flow while the secret regenerates.<p class="note">It might take up to 30 minutes for the secret regeneration to complete.</p>
    ```
    ibmcloud ks nlb-dns secret regenerate -c <cluster_name_or_ID> --nlb-subdomain <NLB_DNS_subdomain>
    ```
    {: pre}
  3. List all secrets associated with the certificate for your Ingress subdomain. In the output, verify that the **Expiration date** for the secrets are more than 30 days later than today.
    ```
    ibmcloud ks ingress secret ls -c <cluster_name_or_ID> | grep <ingress_subdomain>
    ```
    {: pre}

<br />

## Connection via WebSocket closes after 60 seconds
{: #cs_ingress_websocket}

**Infrastructure provider**:
  * <img src="images/icon-classic.png" alt="Classic infrastructure provider icon" width="15" style="width:15px; border-style: none"/> Classic
  * <img src="images/icon-vpc.png" alt="VPC infrastructure provider icon" width="15" style="width:15px; border-style: none"/> VPC Generation 2 compute

{: tsSymptoms}
Your Ingress service exposes an app that uses a WebSocket. However, the connection between a client and your WebSocket app closes when no traffic is sent between them for 60 seconds.

{: tsCauses}
The connection to your WebSocket app might drop after 60 seconds of inactivity for one of the following reasons:

* Your Internet connection has a proxy or firewall that doesn't tolerate long connections.
* A timeout in the ALB to the WebSocket app terminates the connection.

{: tsResolve}
To prevent the connection from closing after 60 seconds of inactivity:

1. If you connect to your WebSocket app through a proxy or firewall, make sure the proxy or firewall isn't configured to automatically terminate long connections.

2. To keep the connection alive, you can increase the value of the timeout or set up a heartbeat in your app.
<dl><dt>Change the timeout</dt>
<dd>Increase the value of the `proxy-read-timeout` in your ALB configuration. For example, to change the timeout from `60s` to a larger value like `300s`, add this [annotation](/docs/containers?topic=containers-ingress_annotation#connection) to your Ingress resource file: `ingress.bluemix.net/proxy-read-timeout: "serviceName=<service_name> timeout=300s"`. The timeout is changed for all public ALBs in your cluster.</dd>
<dt>Set up a heartbeat</dt>
<dd>If you don't want to change the ALB's default read timeout value, set up a heartbeat in your WebSocket app. When you set up a heartbeat protocol by using a framework like [WAMP ![External link icon](../icons/launch-glyph.svg "External link icon")](https://wamp-proto.org/), the app's upstream server periodically sends a "ping" message on a timed interval and the client responds with a "pong" message. Set the heartbeat interval to 58 seconds or less so that the "ping/pong" traffic keeps the connection open before the 60-second timeout is enforced.</dd></dl>
