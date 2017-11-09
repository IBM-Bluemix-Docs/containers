---

copyright:
  years: 2014, 2017
lastupdated: "2017-10-25"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}


# CLI reference for managing clusters
{: #cs_cli_reference}

Refer to these commands to create and manage clusters.
{:shortdesc}

**Tip:** Looking for `bx cr` commands? See the [{{site.data.keyword.registryshort_notm}} CLI reference](/docs/cli/plugins/registry/index.html). Looking for `kubectl` commands? See the [Kubernetes documentation ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/user-guide/kubectl/v1.5/).


<!--[https://github.ibm.com/alchemy-containers/armada-cli ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.ibm.com/alchemy-containers/armada-cli)-->

<table summary="Commands for creating clusters on {{site.data.keyword.Bluemix_notm}}">
 <thead>
    <th colspan=5>Commands for creating clusters on {{site.data.keyword.Bluemix_notm}}</th>
 </thead>
 <tbody>
 <tr>
    <td>[bx cs cluster-config](cs_cli_devtools.html#cs_cluster_config)</td>
    <td>[bx cs cluster-create](cs_cli_devtools.html#cs_cluster_create)</td>
    <td>[bx cs cluster-get](cs_cli_devtools.html#cs_cluster_get)</td>
    <td>[bx cs cluster-rm](cs_cli_devtools.html#cs_cluster_rm)</td>
    <td>[bx cs cluster-service-bind](cs_cli_devtools.html#cs_cluster_service_bind)</td>
 </tr>
 <tr>
    <td>[bx cs cluster-service-unbind](cs_cli_devtools.html#cs_cluster_service_unbind)</td>
    <td>[bx cs cluster-services](cs_cli_devtools.html#cs_cluster_services)</td>
    <td>[bx cs cluster-subnet-add](cs_cli_devtools.html#cs_cluster_subnet_add)</td>
    <td>[bx cs clusters](cs_cli_devtools.html#cs_clusters)</td>
    <td>[bx cs credentials-set](cs_cli_devtools.html#cs_credentials_set)</td>
 </tr>
 <tr>
   <td>[bx cs credentials-unset](cs_cli_devtools.html#cs_credentials_unset)</td>
   <td>[bx cs help](cs_cli_devtools.html#cs_help)</td>
   <td>[bx cs init](cs_cli_devtools.html#cs_init)</td>
   <td>[bx cs locations](cs_cli_devtools.html#cs_datacenters)</td>
   <td>[bx cs machine-types](cs_cli_devtools.html#cs_machine_types)</td>
   </tr>
 <tr>
    <td>[bx cs subnets](cs_cli_devtools.html#cs_subnets)</td>
    <td>[bx cs vlans](cs_cli_devtools.html#cs_vlans)</td>
    <td>[bx cs webhook-create](cs_cli_devtools.html#cs_webhook_create)</td>
    <td>[bx cs worker-add](cs_cli_devtools.html#cs_worker_add)</td>
    <td>[bx cs worker-get](cs_cli_devtools.html#cs_worker_get)</td>
    </tr>
 <tr>
   <td>[bx cs worker-reboot](cs_cli_devtools.html#cs_worker_reboot)</td>
   <td>[bx cs worker-reload](cs_cli_devtools.html#cs_worker_reload)</td>
   <td>[bx cs worker-rm](cs_cli_devtools.html#cs_worker_rm)</td>
   <td>[bx cs workers](cs_cli_devtools.html#cs_workers)</td>
   
  </tr>
 </tbody>
 </table>

**Tip:** To see the version of the {{site.data.keyword.containershort_notm}} plug-in, run the following command.

```
bx plugin list
```
{: pre}


## bx cs commands
{: #cs_commands}

### bx cs cluster-config CLUSTER [--admin] [--export]
{: #cs_cluster_config}

After logging in, download Kubernetes configuration data and certificates to connect to your cluster and to run `kubectl` commands. The files are downloaded to `user_home_directory/.bluemix/plugins/container-service/clusters/<cluster_name>`.

**Command options**:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>The name or ID of the cluster. This value is required.</dd>

   <dt><code>--admin</code></dt>
   <dd>Download the TLS certificates and permission files for the Super User role. You can use the certs to automate tasks in a cluster without having to re-authenticate. The files are downloaded to `<user_home_directory>/.bluemix/plugins/container-service/clusters/<cluster_name>-admin`. This value is optional.</dd>

   <dt><code>--export</code></dt>
   <dd>Download Kubernetes configuration data and certificates without any messages other than the export command. Because no messages are displayed, you can use this flag when you create automated scripts. This value is optional.</dd>
   </dl>

**Example**:

```
bx cs cluster-config my_cluster
```
{: pre}



### bx cs cluster-create [--file FILE_LOCATION] [--hardware HARDWARE] --location LOCATION --machine-type MACHINE_TYPE --name NAME [--no-subnet] [--private-vlan PRIVATE_VLAN] [--public-vlan PUBLIC_VLAN] [--workers WORKER]
{: #cs_cluster_create}

To create a cluster in your organization.

<strong>Command options</strong>

<dl>
<dt><code>--file <em>FILE_LOCATION</em></code></dt>

<dd>The path to the YAML file to create your standard cluster. Instead of defining the characteristics of your cluster by using the options provided in this command, you can use a YAML file.  This value is optional for standard clusters and is not available for lite clusters.

<p><strong>Note:</strong> If you provide the same option in the command as parameter in the YAML file, the value in the command takes precedence over the value in the YAML. For example, you define a location in your YAML file and use the <code>--location</code> option in the command, the value that you entered in the command option overrides the value in the YAML file.

<pre class="codeblock">
<code>name: <em>&lt;cluster_name&gt;</em>
location: <em>&lt;location&gt;</em>
machine-type: <em>&lt;machine_type&gt;</em>
private-vlan: <em>&lt;private_vlan&gt;</em>
public-vlan: <em>&lt;public_vlan&gt;</em>
hardware: <em>&lt;shared_or_dedicated&gt;</em>
workerNum: <em>&lt;number_workers&gt;</em></code></pre>


<table>
    <caption>Table 1.Understanding the YAML file components</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the YAML file components</th>
    </thead>
    <tbody>
    <tr>
    <td><code><em>name</em></code></td>
    <td>Replace <code><em>&lt;cluster_name&gt;</em></code> with a name for your cluster.</td>
    </tr>
    <tr>
    <td><code><em>location</em></code></td>
    <td>Replace <code><em>&lt;location&gt;</em></code> with the location where you want to create your cluster. The available locations are dependent on the region that you are logged in. To list available locations, run <code>bx cs locations</code>. </td>
     </tr>
     <tr>
     <td><code><em>machine-type</em></code></td>
     <td>Replace <code><em>&lt;machine_type&gt;</em></code> with the machine type that you want for your worker nodes. To list available machine types for your location, run <code>bx cs machine-types <em>&lt;location&gt;</em></code>.</td>
     </tr>
     <tr>
     <td><code><em>private-vlan</em></code></td>
     <td>Replace <code><em>&lt;private_vlan&gt;</em></code> with the ID of the private VLAN that you want to use for your worker nodes. To list available VLANs, run <code>bx cs vlans <em>&lt;location&gt;</em></code> and look for VLAN routers that start with <code>bcr</code> (back-end router).</td>
     </tr>
     <tr>
     <td><code><em>public-vlan</em></code></td>
     <td>Replace <code><em>&lt;public_vlan&gt;</em></code> with the ID of the public VLAN that you want to use for your worker nodes. To list available VLANs, run <code>bx cs vlans <em>&lt;location&gt;</em></code> and look for VLAN routers that start with <code>fcr</code> (front-end router).</td>
     </tr>
     <tr>
     <td><code><em>hardware</em></code></td>
     <td>The level of hardware isolation for your worker node. Use dedicated if you want to have available physical resources dedicated to you only, or shared to allow physical resources to be shared with other IBM customers. The default is <code>shared</code>.</td>
     </tr>
     <tr>
     <td><code><em>workerNum</em></code></td>
     <td>Replace <code><em>&lt;number_workers&gt;</em></code> with the number of worker nodes that you want to deploy.</td>
     </tr>
     </tbody></table>
    </p></dd>

<dt><code>--hardware <em>HARDWARE</em></code></dt>
<dd>The level of hardware isolation for your worker node. Use dedicated to have available physical resources dedicated to you only, or shared to allow physical resources to be shared with other IBM customers. The default is shared.  This value is optional for standard clusters and is not available for lite clusters.</dd>

<dt><code>--location <em>LOCATION</em></code></dt>
<dd>The location where you want to create the cluster. The locations that are available to you depend on the {{site.data.keyword.Bluemix_notm}} region you are logged in to. Select the region that is physically closest to you for best performance.  This value is required for standard clusters and is optional for lite clusters.

<p>Review [available locations](cs_regions.html#locations).
</p>

<p><strong>Note:</strong> When you select a location that is located outside your country, keep in mind that you might require legal authorization before data can be physically stored in a foreign country.</p>
</dd>

<dt><code>--machine-type <em>MACHINE_TYPE</em></code></dt>
<dd>The machine type that you choose impacts the amount of memory and disk space that is available to the containers that are deployed to your worker node. To list available machine types, see [bx cs machine-types <em>LOCATION</em>](cs_cli_reference.html#cs_machine_types).  This value is required for standard clusters and is not available for lite clusters.</dd>

<dt><code>--name <em>NAME</em></code></dt>
<dd>The name for the cluster.  This value is required.</dd>

<dt><code>--no-subnet</code></dt>
<dd>Include the flag to create a cluster without a portable subnet. The default is to not use the flag and to create a subnet in your IBM Cloud infrastructure (SoftLayer) portfolio. This value is optional.</dd>

<dt><code>--private-vlan <em>PRIVATE_VLAN</em></code></dt>
<dd>

<ul>
<li>This parameter is not available for lite clusters.</li>
<li>If this standard cluster is the first standard cluster that you create in this location, do not include this flag. A private VLAN is created for you when the clusters is created.</li>
<li>If you created a standard cluster before in this location or created a private VLAN in IBM Cloud infrastructure (SoftLayer) before, you must specify that private VLAN.

<p><strong>Note:</strong> The public and private VLANs that you specify with the create command must match. Private VLAN routers always begin with <code>bcr</code> (back-end router) and public VLAN routers always begin with <code>fcr</code> (front-end router). The number and letter combination after those prefixes must match to use those VLANs when creating a cluster. Do not use public and private VLANs that do not match to create a cluster.</p></li>
</ul>

<p>To find out if you already have a private VLAN for a specific location or to find the name of an existing private VLAN, run <code>bx cs vlans <em>&lt;location&gt;</em></code>.</p></dd>

<dt><code>--public-vlan <em>PUBLIC_VLAN</em></code></dt>
<dd>
<ul>
<li>This parameter is not available for lite clusters.</li>
<li>If this standard cluster is the first standard cluster that you create in this location, do not use this flag. A public VLAN is created for you when the cluster is created.</li>
<li>If you created a standard cluster before in this location or created a public VLAN in IBM Cloud infrastructure (SoftLayer) before, you must specify that public VLAN.

<p><strong>Note:</strong> The public and private VLANs that you specify with the create command must match. Private VLAN routers always begin with <code>bcr</code> (back-end router) and public VLAN routers always begin with <code>fcr</code> (front-end router). The number and letter combination after those prefixes must match to use those VLANs when creating a cluster. Do not use public and private VLANs that do not match to create a cluster.</p></li>
</ul>

<p>To find out if you already have a public VLAN for a specific location or to find the name of an existing public VLAN, run <code>bx cs vlans <em>&lt;location&gt;</em></code>.</p></dd>

<dt><code>--workers WORKER</code></dt>
<dd>The number of worker nodes that you want to deploy in your cluster. If you do not specify this option, a cluster with 1 worker node is created. This value is optional for standard clusters and is not available for lite clusters.

<p><strong>Note:</strong> Every worker node is assigned a unique worker node ID and domain name that must not be manually changed after the cluster is created. Changing the ID or domain name prevents the Kubernetes master from managing your cluster.</p></dd>
</dl>

**Examples**:

  

  Example for a standard cluster:
  {: #example_cluster_create}

  ```
  bx cs cluster-create --location dal10 --public-vlan my_public_vlan_id --private-vlan my_private_vlan_id --machine-type u1c.2x4 --name my_cluster --hardware shared --workers 2
  ```
  {: pre}

  Example for a lite cluster:

  ```
  bx cs cluster-create --name my_cluster
  ```
  {: pre}

  Example for an {{site.data.keyword.Bluemix_dedicated_notm}} environment:

  ```
  bx cs cluster-create --machine-type machine-type --workers number --name cluster_name
  ```
  {: pre}


### bx cs cluster-get CLUSTER [--showResources]
{: #cs_cluster_get}

View information about a cluster in your organization.

<strong>Command options</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>The name or ID of the cluster. This value is required.</dd>

   <dt><code><em>--showResources</em></code></dt>
   <dd>Shows the VLANs and subnets for a cluster.</dd>
   </dl>

**Example**:

  ```
  bx cs cluster-get my_cluster
  ```
  {: pre}


### bx cs cluster-rm [-f] CLUSTER
{: #cs_cluster_rm}

Remove a cluster from your organization.

<strong>Command options</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>The name or ID of the cluster. This value is required.</dd>

   <dt><code>-f</code></dt>
   <dd>Use this option to force the removal of a cluster with no user prompts. This value is optional.</dd>
   </dl>

**Example**:

  ```
  bx cs cluster-rm my_cluster
  ```
  {: pre}


### bx cs cluster-service-bind CLUSTER KUBERNETES_NAMESPACE SERVICE_INSTANCE_GUID
{: #cs_cluster_service_bind}

Add an {{site.data.keyword.Bluemix_notm}} service to a cluster.

**Tip:** For {{site.data.keyword.Bluemix_dedicated_notm}} users, see [Adding {{site.data.keyword.Bluemix_notm}} services to clusters in {{site.data.keyword.Bluemix_dedicated_notm}} (Closed Beta)](cs_cluster.html#binding_dedicated).

<strong>Command options</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>The name or ID of the cluster. This value is required.</dd>

   <dt><code><em>KUBERNETES_NAMESPACE</em></code></dt>
   <dd>The name of the Kubernetes namespace. This value is required.</dd>

   <dt><code><em>SERVICE_INSTANCE_GUID</em></code></dt>
   <dd>The ID of the {{site.data.keyword.Bluemix_notm}} service instance that you want to bind. This value is required.</dd>
   </dl>

**Example**:

  ```
  bx cs cluster-service-bind my_cluster my_namespace my_service_instance_GUID
  ```
  {: pre}


### bx cs cluster-service-unbind CLUSTER KUBERNETES_NAMESPACE SERVICE_INSTANCE_GUID
{: #cs_cluster_service_unbind}

Remove an {{site.data.keyword.Bluemix_notm}} service from a cluster.

**Note:** When you remove an {{site.data.keyword.Bluemix_notm}} service, the service credentials are removed from the cluster. If a pod is still using the service, it fails because the service credentials cannot be found.

<strong>Command options</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>The name or ID of the cluster. This value is required.</dd>

   <dt><code><em>KUBERNETES_NAMESPACE</em></code></dt>
   <dd>The name of the Kubernetes namespace. This value is required.</dd>

   <dt><code><em>SERVICE_INSTANCE_GUID</em></code></dt>
   <dd>The ID of the {{site.data.keyword.Bluemix_notm}} service instance that you want to remove. This value is required.</dd>
   </dl>

**Example**:

  ```
  bx cs cluster-service-unbind my_cluster my_namespace my_service_instance_GUID
  ```
  {: pre}


### bx cs cluster-services CLUSTER [--namespace KUBERNETES_NAMESPACE] [--all-namespaces]
{: #cs_cluster_services}

List the services that are bound to one or all of the Kubernetes namespace in a cluster. If no options are specified, the services for the default namespace are displayed.

<strong>Command options</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>The name or ID of the cluster. This value is required.</dd>

   <dt><code>--namespace <em>KUBERNETES_NAMESPACE</em></code>, <code>-n <em>KUBERNETES_NAMESPACE</em></code></dt>
   <dd>Include the services that are bound to a specific namespace in a cluster. This value is optional.</dd>

   <dt><code>--all-namespaces</code></dt>
    <dd>Include the services that are bound to all of the namespaces in a cluster. This value is optional.</dd>
    </dl>

**Example**:

  ```
  bx cs cluster-services my_cluster --namespace my_namespace
  ```
  {: pre}


### bx cs cluster-subnet-add CLUSTER SUBNET
{: #cs_cluster_subnet_add}

Make a subnet in an IBM Cloud infrastructure (SoftLayer) account available to a specified cluster.

**Note:** When you make a subnet available to a cluster, IP addresses of this subnet are used for cluster networking purposes. To avoid IP address conflicts, make sure that you use a subnet with one cluster only. Do not use a subnet for multiple clusters or for other purposes outside of {{site.data.keyword.containershort_notm}} at the same time.

<strong>Command options</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>The name or ID of the cluster. This value is required.</dd>

   <dt><code><em>SUBNET</em></code></dt>
   <dd>The ID of the subnet. This value is required.</dd>
   </dl>

**Example**:

  ```
  bx cs cluster-subnet-add my_cluster subnet
  ```
  {: pre}

### bx cs cluster-subnet-create CLUSTER SIZE VLAN_ID
{: #cs_cluster_subnet_create}

Create a subnet in an IBM Cloud infrastructure (SoftLayer) account and make it available to a specified cluster in {{site.data.keyword.containershort_notm}}.

**Note:** When you make a subnet available to a cluster, IP addresses of this subnet are used for cluster networking purposes. To avoid IP address conflicts, make sure that you use a subnet with one cluster only. Do not use a subnet for multiple clusters or for other purposes outside of {{site.data.keyword.containershort_notm}} at the same time.

<strong>Command options</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>The name or ID of the cluster. This value is required. To list your clusters, use the `bx cs clusters` [command](#cs_clusters).</dd>

   <dt><code><em>SIZE</em></code></dt>
   <dd>The number of subnet IP addresses. This value is required. Possible values are 8, 16, 32, or 64.</dd>

   <dt><code><em>VLAN_ID</em></code></dt>
   <dd>The VLAN in which to create the subnet. This value is required. To list available VLANS, use the `bx cs vlans <location>` [command](#cs_vlans).</dd>
   </dl>

**Example**:

  ```
  bx cs cluster-subnet-create my_cluster 8 1764905
  ```
  {: pre}

### bx cs cluster-user-subnet-add CLUSTER SUBNET_CIDR PRIVATE_VLAN
{: #cs_cluster_user_subnet_add}

Bring your own private subnet to your {{site.data.keyword.containershort_notm}} clusters.

This private subnet is not one provided by IBM Cloud infrastructure (SoftLayer). As such, you must configure any inbound and outbound network traffic routing for the subnet. To add an IBM Cloud infrastructure (SoftLayer) subnet, use the `bx cs cluster-subnet-add` [command](#cs_cluster_subnet_add).

**Note**: When you add a private user subnet to a cluster, IP addresses of this subnet are used for private Load Balancers in the cluster. To avoid IP address conflicts, make sure that you use a subnet with one cluster only. Do not use a subnet for multiple clusters or for other purposes outside of {{site.data.keyword.containershort_notm}} at the same time.

<strong>Command options</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>The name or ID of the cluster. This value is required.</dd>

   <dt><code><em>SUBNET_CIDR</em></code></dt>
   <dd>The subnet Classless InterDomain Routing (CIDR). This value is required, and must not conflict with any subnet that is used by IBM Cloud infrastructure (SoftLayer).

   Supported prefixes range from `/30` (1 IP address) to `/24` (253 IP addresses). If you set the CIDR at one prefix length and later need to change it, first add the new CIDR, then [remove the old CIDR](#cs_cluster_user_subnet_rm).</dd>

   <dt><code><em>PRIVATE_VLAN</em></code></dt>
   <dd>The ID of the private VLAN. This value is required. It must match the private VLAN ID of one or more of the worker nodes in the cluster.</dd>
   </dl>

**Example**:

  ```
  bx cs cluster-user-subnet-add my_cluster 192.168.10.0/29 1502175
  ```
  {: pre}


### bx cs cluster-user-subnet-rm CLUSTER SUBNET_CIDR PRIVATE_VLAN
{: #cs_cluster_user_subnet_rm}

Remove your own private subnet from a specified cluster.

**Note:** Any service that was deployed to an IP address from your own private subnet remains active after the subnet is removed.

<strong>Command options</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>The name or ID of the cluster. This value is required.</dd>

   <dt><code><em>SUBNET_CIDR</em></code></dt>
   <dd>The subnet Classless InterDomain Routing (CIDR). This value is required, and must match the CIDR that was set by the `bx cs cluster-user-subnet-add` [command](#cs_cluster_user_subnet_add).</dd>

   <dt><code><em>PRIVATE_VLAN</em></code></dt>
   <dd>The ID of the private VLAN. This value is required, and must match the VLAN ID that was set by the `bx cs cluster-user-subnet-add` [command](#cs_cluster_user_subnet_add).</dd>
   </dl>

**Example**:

  ```
  bx cs cluster-user-subnet-rm my_cluster 192.168.10.0/29 1502175
  ```
  {: pre}


### bx cs cluster-update [-f] CLUSTER
{: #cs_cluster_update}

Update the Kubernetes master to the latest API version. During the update, you cannot access or change the cluster. Worker nodes, apps, and resources that have been deployed by the user are not modified and will continue to run.

You might need to change your YAML files for future deployments. Review this [release note](cs_versions.html) for details.

<strong>Command options</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>The name or ID of the cluster. This value is required.</dd>

   <dt><code>-f</code></dt>
   <dd>Use this option to force the update of the master with no user prompts. This value is optional.</dd>
   </dl>

**Example**:

  ```
  bx cs cluster-update my_cluster
  ```
  {: pre}

### bx cs clusters
{: #cs_clusters}

View a list of clusters in your organization.

<strong>Command options</strong>:

  None

**Example**:

  ```
  bx cs clusters
  ```
  {: pre}


### bx cs credentials-set --infrastructure-api-key API_KEY --infrastructure-username USERNAME
{: #cs_credentials_set}

Set IBM Cloud infrastructure (SoftLayer) account credentials for your {{site.data.keyword.Bluemix_notm}} account. These credentials allow you to access the IBM Cloud infrastructure (SoftLayer) portfolio through your {{site.data.keyword.Bluemix_notm}} account.

**Note:** Do not set multiple credentials for one {{site.data.keyword.Bluemix_notm}} account. Every {{site.data.keyword.Bluemix_notm}} account is linked to one IBM Cloud infrastructure (SoftLayer) portfolio only.

<strong>Command options</strong>:

   <dl>
   <dt><code>--infrastructure-username <em>USERNAME</em></code></dt>
   <dd>IBM Cloud infrastructure (SoftLayer) account username. This value is required.</dd>
   </dl>

   <dt><code>--infrastructure-api-key <em>API_KEY</em></code></dt>
   <dd>IBM Cloud infrastructure (SoftLayer) account API key. This value is required.

 <p>
  To generate an API key:

  <ol>
  <li>Log in to the [IBM Cloud infrastructure (SoftLayer) portal ![External link icon](../icons/launch-glyph.svg "External link icon")](https://control.softlayer.com/).</li>
  <li>Select <strong>Account</strong>, and then <strong>Users</strong>.</li>
  <li>Click <strong>Generate</strong> to generate an IBM Cloud infrastructure (SoftLayer) API key for your account.</li>
  <li>Copy the API key to use in this command.</li>
  </ol>

  To view your existing API key:
  <ol>
  <li>Log in to the [IBM Cloud infrastructure (SoftLayer)portal ![External link icon](../icons/launch-glyph.svg "External link icon")](https://control.softlayer.com/).</li>
  <li>Select <strong>Account</strong>, and then <strong>Users</strong>.</li>
  <li>Click <strong>View</strong> to see your existing API key.</li>
  <li>Copy the API key to use in this command.</li>
  </ol></p></dd>

**Example**:

  ```
  bx cs credentials-set --infrastructure-api-key API_KEY --infrastructure-username USERNAME
  ```
  {: pre}


### bx cs credentials-unset
{: #cs_credentials_unset}

Remove IBM Cloud infrastructure (SoftLayer) account credentials from your {{site.data.keyword.Bluemix_notm}} account. After removing the credentials, you cannot access the IBM Cloud infrastructure (SoftLayer) portfolio through your {{site.data.keyword.Bluemix_notm}} account anymore.

<strong>Command options</strong>:

   None

**Example**:

  ```
  bx cs credentials-unset
  ```
  {: pre}



### bx cs help
{: #cs_help}

View a list of supported commands and parameters.

<strong>Command options</strong>:

   None

**Example**:

  ```
  bx cs help
  ```
  {: pre}


### bx cs init [--host HOST]
{: #cs_init}

Initialize the {{site.data.keyword.containershort_notm}} plug-in or specify the region where you want to create or access Kubernetes clusters.

<strong>Command options</strong>:

   <dl>
   <dt><code>--host <em>HOST</em></code></dt>
   <dd>The {{site.data.keyword.containershort_notm}} API endpoint that you want to use.  This value is optional. Examples:

    <ul>
    <li>US South:

    <pre class="codeblock">
    <code>bx cs init --host https://us-south.containers.bluemix.net</code>
    </pre></li>

    <li>US East:

    <pre class="codeblock">
    <code>bx cs init --host https://us-east.containers.bluemix.net</code>
    </pre>
    <p><strong>Note</strong>: US-East is available for use with CLI commands only.</p></li>

    <li>UK South:

    <pre class="codeblock">
    <code>bx cs init --host https://uk-south.containers.bluemix.net</code>
    </pre></li>

    <li>EU Central:

    <pre class="codeblock">
    <code>bx cs init --host https://eu-central.containers.bluemix.net</code>
    </pre></li>

    <li>AP South:

    <pre class="codeblock">
    <code>bx cs init --host https://ap-south.containers.bluemix.net</code>
    </pre></li></ul>
</dd>
</dl>





### bx cs locations
{: #cs_datacenters}

View a list of available locations for you to create a cluster in.

<strong>Command options</strong>:

   None

**Example**:

  ```
  bx cs locations
  ```
  {: pre}

### bx cs logging-config-create CLUSTER [--namespace KUBERNETES_NAMESPACE] [--logsource LOG_SOURCE] [--hostname LOG_SERVER_HOSTNAME] [--port LOG_SERVER_PORT] --type LOG_TYPE
{: #cs_logging_create}

Create a logging configuration. By default, namespace logs are forwarded to {{site.data.keyword.loganalysislong_notm}}. You can use this command to forward namespace logs to an external syslog server. You can also use this command to forward logs for applications, worker nodes, Kubernetes clusters, and Ingress controllers to {{site.data.keyword.loganalysisshort_notm}} or to an external syslog server.

<strong>Command options</strong>:

<dl>
<dt><code><em>CLUSTER</em></code></dt>
<dd>The name or ID of the cluster.</dd>
<dt><code>--logsource <em>LOG_SOURCE</em></code></dt>
<dd>The log source for which you want to enable log forwarding. Accepted values are <code>application</code>, <code>worker</code>, <code>kubernetes</code>, and <code>ingress</code>. This value is required for log sources other than Docker container namespaces.</dd>
<dt><code>--namespace <em>KUBERNETES_NAMESPACE</em></code></dt>
<dd>The Docker container namespace from which you want to forward logs to syslog. Log forwarding is not supported for the <code>ibm-system</code> and <code>kube-system</code> Kubernetes namespaces. This value is required for namespaces. If you do not specify a namespace, then all namespaces in the container use this configuration.</dd>
<dt><code>--hostname <em>LOG_SERVER_HOSTNAME</em></code></dt>
<dd>The hostname or IP address of the log collector server. This value is required when the logging type is <code>syslog</code>.</dd>
<dt><code>--port <em>LOG_SERVER_PORT</em></code></dt>
<dd>The port of the log collector server. This value is optional when the logging type is <code>syslog</code>. If you do not specify a port, then the standard port <code>514</code> is used for <code>syslog</code>.</dd>
<dt><code>--type <em>LOG_TYPE</em></code></dt>
<dd>The log forwarding protocol that you want to use. Currently, <code>syslog</code> and <code>ibm</code> are supported. This value is required.</dd>
</dl>

**Example for log source `namespace`**:

  ```
  bx cs logging-config-create my_cluster --namespace my_namespace --hostname localhost --port 5514 --type syslog
  ```
  {: pre}

**Example for log source `ingress`**:

  ```
  bx cs logging-config-create my_cluster f4bc77c0-ee7d-422d-aabf-a4e6b977264e --type ibm
  ```
  {: pre}

### bx cs logging-config-get CLUSTER [--logsource LOG_SOURCE]
{: #cs_logging_get}

View all log forwarding configurations for a cluster, or filter logging configurations based on log source.

<strong>Command options</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>The name or ID of the cluster. This value is required.</dd>
   <dt><code>--logsource <em>LOG_SOURCE</em></code></dt>
   <dd>The kind of log source for which you want to filter. Only logging configurations of this log source in the cluster are returned. Accepted values are <code>namespace</code>, <code>application</code>, <code>worker</code>, <code>kubernetes</code>, and <code>ingress</code>. This value is optional.</dd>
   </dl>

**Example**:

  ```
  bx cs logging-config-get my_cluster --logsource worker
  ```
  {: pre}


### bx cs logging-config-rm CLUSTER [--namespace KUBERNETES_NAMESPACE] [--id LOG_SOURCE_LOGGING_ID]
{: #cs_logging_rm}

Deletes a log forwarding configuration. For a Docker container namespace, you can stop forwarding logs to a syslog server. The namespace continues to forward logs to {{site.data.keyword.loganalysislong_notm}}. For a log source other than a Docker container namespace, you can stop forwarding logs to a syslog server or to {{site.data.keyword.loganalysisshort_notm}}.

<strong>Command options</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>The name or ID of the cluster. This value is required.</dd>
   <dt><code>--namespace <em>KUBERNETES_NAMESPACE</em></code></dt>
   <dd>The Docker container namespace from which you want to stop forwarding logs to syslog. This value is required for Docker container namespaces.</dd>
   <dt><code>--id <em>LOG_SOURCE_LOGGING_ID</em></code></dt>
   <dd>The logging configuration ID that you want to remove from the log source. This value is required for log sources other than Docker container namespaces.</dd>
   </dl>

**Example**:

  ```
  bx cs logging-config-rm my_cluster --namespace my_namespace
  ```
  {: pre}


### bx cs logging-config-update CLUSTER [--namespace NAMESPACE] [--id LOG_SOURCE_LOGGING_ID] [--logsource LOG_SOURCE] [--hostname LOG_SERVER_HOSTNAME] [--port LOG_SERVER_PORT] --type LOG_TYPE
{: #cs_logging_update}

Update log forwarding to the logging server you want use. For a Docker container namespace, you can use this command to update details for the current syslog server or change to a different syslog server. For a logging source other than a Docker container namespace, you can use this command to change the log collector server type. Currently, 'syslog' and 'ibm' are supported as log types.

<strong>Command options</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>The name or ID of the cluster. This value is required.</dd>
   <dt><code>--namespace <em>NAMESPACE</em></code></dt>
   <dd>The Docker container namespace from which you want to forward logs to syslog. Log forwarding is not supported for the <code>ibm-system</code> and <code>kube-system</code> Kubernetes namespaces. This value is required for namespaces.</dd>
   <dt><code>--logsource <em>LOG_SOURCE</em></code></dt>
   <dd>The log source for which you want to update log forwarding. Accepted values are <code>application</code>, <code>worker</code>, <code>kubernetes</code>, and <code>ingress</code>. This value is required for log sources other than Docker container namespaces.</dd>
   <dt><code>--id <em>LOG_SOURCE_LOGGING_ID</em></code></dt>
   <dd>The logging configuration ID that you want to update. This value is required for log sources other than Docker container namespaces.</dd>
   <dt><code>--hostname <em>LOG_SERVER_HOSTNAME</em></code></dt>
   <dd>The hostname or IP address of the log collector server. This value is required when the logging type is <code>syslog</code>.</dd>
   <dt><code>--port <em>LOG_SERVER_PORT</em></code></dt>
   <dd>The port of the log collector server. This value is optional when the logging type is <code>syslog</code>. If you do not specify a port, then the standard port 514 is used for <code>syslog</code>.</dd>
   <dt><code>--type <em>LOG_TYPE</em></code></dt>
   <dd>The log forwarding protocol that you want to use. Currently, <code>syslog</code> and <code>ibm</code> are supported. This value is required.</dd>
   </dl>

**Example for log type `ibm`**:

  ```
  bx cs logging-config-update my_cluster --id f4bc77c0-ee7d-422d-aabf-a4e6b977264e --type ibm
  ```
  {: pre}

**Example for log type `syslog`**:

  ```
  bx cs logging-config-update my_cluster --namespace my_namespace --hostname localhost --port 5514 --type syslog
  ```
  {: pre}

### bx cs machine-types LOCATION
{: #cs_machine_types}

View a list of available machine types for your worker nodes. Each machine type includes the amount of virtual CPU, memory, and disk space for each worker node in the cluster.

<strong>Command options</strong>:

   <dl>
   <dt><code><em>LOCATION</em></code></dt>
   <dd>Enter the location where you want to list available machine types. This value is required. Review [available locations](cs_regions.html#locations).</dd></dl>

**Example**:

  ```
  bx cs machine-types LOCATION
  ```
  {: pre}


### bx cs subnets
{: #cs_subnets}

View a list of subnets that are available in an IBM Cloud infrastructure (SoftLayer) account.

<strong>Command options</strong>:

   None

**Example**:

  ```
  bx cs subnets
  ```
  {: pre}


### bx cs vlans LOCATION
{: #cs_vlans}

List the public and private VLANs that are available for a location in your IBM Cloud infrastructure (SoftLayer) account. To list available VLANs, you must have a paid account.

<strong>Command options</strong>:

   <dl>
   <dt><code><em>LOCATION</em></code></dt>
   <dd>Enter the location where you want to list your private and public VLANs. This value is required. Review [available locations](cs_regions.html#locations).</dd>
   </dl>

**Example**:

  ```
  bx cs vlans dal10
  ```
  {: pre}


### bx cs webhook-create --cluster CLUSTER --level LEVEL --type slack --URL URL
{: #cs_webhook_create}

Create webhooks.

<strong>Command options</strong>:

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>The name or ID of the cluster. This value is required.</dd>

   <dt><code>--level <em>LEVEL</em></code></dt>
   <dd>The notification level, such as <code>Normal</code> or <code>Warning</code>. <code>Warning</code> is the default value. This value is optional.</dd>

   <dt><code>--type <em>slack</em></code></dt>
   <dd>The webhook type, such as slack. Only slack is supported. This value is required.</dd>

   <dt><code>--URL <em>URL</em></code></dt>
   <dd>The URL for the webhook. This value is required.</dd>
   </dl>

**Example**:

  ```
  bx cs webhook-create --cluster my_cluster --level Normal --type slack --URL http://github.com/<mywebhook>
  ```
  {: pre}


### bx cs worker-add --cluster CLUSTER [--file FILE_LOCATION] [--hardware HARDWARE] --machine-type MACHINE_TYPE --number NUMBER --private-vlan PRIVATE_VLAN --public-vlan PUBLIC_VLAN
{: #cs_worker_add}

Add worker nodes to your standard cluster.

<strong>Command options</strong>:

<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>The name or ID of the cluster. This value is required.</dd>

<dt><code>--file <em>FILE_LOCATION</em></code></dt>
<dd>The path to the YAML file to add worker nodes to your cluster. Instead of defining your additional worker nodes by using the options provided in this command, you can use a YAML file. This value is optional.

<p><strong>Note:</strong> If you provide the same option in the command as parameter in the YAML file, the value in the command takes precedence over the value in the YAML. For example, you define a machine type in your YAML file and use the --machine-type option in the command, the value that you entered in the command option overrides the value in the YAML file.

<pre class="codeblock">
<code>name: <em>&lt;cluster_name_or_id&gt;</em>
location: <em>&lt;location&gt;</em>
machine-type: <em>&lt;machine_type&gt;</em>
private-vlan: <em>&lt;private_vlan&gt;</em>
public-vlan: <em>&lt;public_vlan&gt;</em>
hardware: <em>&lt;shared_or_dedicated&gt;</em>
workerNum: <em>&lt;number_workers&gt;</em></code></pre>

<table>
<caption>Table 2. Understanding the YAML file components</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the YAML file components</th>
</thead>
<tbody>
<tr>
<td><code><em>name</em></code></td>
<td>Replace <code><em>&lt;cluster_name_or_id&gt;</em></code> with the name or ID of the cluster where you want to add worker nodes.</td>
</tr>
<tr>
<td><code><em>location</em></code></td>
<td>Replace <code><em>&lt;location&gt;</em></code> with the location where you want to deploy your worker nodes. The available locations are dependent on the region that you are logged in. To list available locations, run <code>bx cs locations</code>.</td>
</tr>
<tr>
<td><code><em>machine-type</em></code></td>
<td>Replace <code><em>&lt;machine_type&gt;</em></code> with the machine type that you want for your worker nodes. To list available machine types for your location, run <code>bx cs machine-types <em>&lt;location&gt;</em></code>.</td>
</tr>
<tr>
<td><code><em>private-vlan</em></code></td>
<td>Replace <code><em>&lt;private_vlan&gt;</em></code> with the ID of the private VLAN that you want to use for your worker nodes. To list available VLANs, run <code>bx cs vlans <em>&lt;location&gt;</em></code> and look for VLAN routers that start with <code>bcr</code> (back-end router).</td>
</tr>
<tr>
<td><code>public-vlan</code></td>
<td>Replace <code>&lt;public_vlan&gt;</code> with the ID of the public VLAN that you want to use for your worker nodes. To list available VLANs, run <code>bx cs vlans &lt;location&gt;</code> and look for VLAN routers that start with <code>fcr</code> (front-end router).</td>
</tr>
<tr>
<td><code>hardware</code></td>
<td>The level of hardware isolation for your worker node. Use dedicated if you want to have available physical resources dedicated to you only, or shared to allow physical resources to be shared with other IBM customers. The default is shared.</td>
</tr>
<tr>
<td><code>workerNum</code></td>
<td>Replace <code><em>&lt;number_workers&gt;</em></code> with the number of worker nodes that you want to deploy.</td>
</tr>
</tbody></table></p></dd>

<dt><code>--hardware <em>HARDWARE</em></code></dt>
<dd>The level of hardware isolation for your worker node. Use dedicated if you want to have available physical resources dedicated to you only, or shared to allow physical resources to be shared with other IBM customers. The default is shared. This value is optional.</dd>

<dt><code>--machine-type <em>MACHINE_TYPE</em></code></dt>
<dd>The machine type that you choose impacts the amount of memory and disk space that is available to the containers that are deployed to your worker node. This value is required. To list available machine types, see [bx cs machine-types LOCATION](cs_cli_reference.html#cs_machine_types).</dd>

<dt><code>--number <em>NUMBER</em></code></dt>
<dd>An integer that represents the number of worker nodes to create in the cluster. The default value is 1. This value is optional.</dd>

<dt><code>--private-vlan <em>PRIVATE_VLAN</em></code></dt>
<dd>The private VLAN that was specified when the cluster was created. This value is required.

<p><strong>Note:</strong> The public and private VLANs that you specify must match. Private VLAN routers always begin with <code>bcr</code> (back-end router) and public VLAN routers always begin with <code>fcr</code> (front-end router). The number and letter combination after those prefixes must match to use those VLANs when creating a cluster. Do not use public and private VLANs that do not match to create a cluster.</p></dd>

<dt><code>--public-vlan <em>PUBLIC_VLAN</em></code></dt>
<dd>The public VLAN that was specified when the cluster was created. This value is optional.

<p><strong>Note:</strong> The public and private VLANs that you specify must match. Private VLAN routers always begin with <code>bcr</code> (back-end router) and public VLAN routers always begin with <code>fcr</code> (front-end router). The number and letter combination after those prefixes must match to use those VLANs when creating a cluster. Do not use public and private VLANs that do not match to create a cluster.</p></dd>
</dl>

**Examples**:

  ```
  bx cs worker-add --cluster my_cluster --number 3 --public-vlan my_public_vlan_id --private-vlan my_private_vlan_id --machine-type u1c.2x4 --hardware shared
  ```
  {: pre}

  Example for {{site.data.keyword.Bluemix_dedicated_notm}}:

  ```
  bx cs worker-add --cluster my_cluster --number 3 --machine-type u1c.2x4
  ```
  {: pre}


### bx cs worker-get WORKER_NODE_ID
{: #cs_worker_get}

View details of a worker node.

<strong>Command options</strong>:

   <dl>
   <dt><code><em>WORKER_NODE_ID</em></code></dt>
   <dd>The ID for a worker node. Run <code>bx cs workers <em>CLUSTER</em></code> to view the IDs for the worker nodes in a cluster. This value is required.</dd>
   </dl>

**Example**:

  ```
  bx cs worker-get WORKER_NODE_ID
  ```
  {: pre}


### bx cs worker-reboot [-f] [--hard] CLUSTER WORKER [WORKER]
{: #cs_worker_reboot}

Reboot the worker nodes in a cluster. If a problem exists with a worker node, first try rebooting the worker node, which restarts it. If the reboot does not solve the issue, then try the `worker-reload` command. The state of the workers does not change during the reboot. The state remains `deployed`, but the status updates.

<strong>Command options</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>The name or ID of the cluster. This value is required.</dd>

   <dt><code>-f</code></dt>
   <dd>Use this option to force the restart of the worker node with no user prompts. This value is optional.</dd>

   <dt><code>--hard</code></dt>
   <dd>Use this option to force a hard restart of a worker node by cutting off power to the worker node. Use this option if the worker node is unresponsive or the worker node has a Docker hang. This value is optional.</dd>

   <dt><code><em>WORKER</em></code></dt>
   <dd>The name or ID of one or more worker nodes. Use a space to list multiple worker nodes. This value is required.</dd>
   </dl>

**Example**:

  ```
  bx cs worker-reboot my_cluster my_node1 my_node2
  ```
  {: pre}


### bx cs worker-reload [-f] CLUSTER WORKER [WORKER]
{: #cs_worker_reload}

Reload the worker nodes in a cluster. If a problem exists with a worker node, first try rebooting the worker node. If the reboot does not solve the problem, try the `worker-reload` command, which re-loads all of the necessary configurations for the worker node.

<strong>Command options</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>The name or ID of the cluster. This value is required.</dd>

   <dt><code>-f</code></dt>
   <dd>Use this option to force the reload of a worker node with no user prompts. This value is optional.</dd>

   <dt><code><em>WORKER</em></code></dt>
   <dd>The name or ID of one or more worker nodes. Use a space to list multiple worker nodes. This value is required.</dd>
   </dl>

**Example**:

  ```
  bx cs worker-reload my_cluster my_node1 my_node2
  ```
  {: pre}

### bx cs worker-rm [-f] CLUSTER WORKER [WORKER]
{: #cs_worker_rm}

Remove one or more worker nodes from a cluster.

<strong>Command options</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>The name or ID of the cluster. This value is required.</dd>

   <dt><code>-f</code></dt>
   <dd>Use this option to force the removal of a worker node with no user prompts. This value is optional.</dd>

   <dt><code><em>WORKER</em></code></dt>
   <dd>The name or ID of one or more worker nodes. Use a space to list multiple worker nodes. This value is required.</dd>
   </dl>

**Example**:

  ```
  bx cs worker-rm my_cluster my_node1 my_node2
  ```
  {: pre}

### bx cs worker-update [-f] CLUSTER WORKER [WORKER]
{: #cs_worker_update}

Update worker nodes to the latest Kubernetes version. Running `bx cs worker-update` can cause downtime for your apps and services. During the update, all pods are rescheduled onto other worker nodes and data is deleted if not stored outside the pod. To avoid downtime, ensure that you have enough worker nodes to handle your workload while the selected worker nodes are updating.

You might need to change your YAML files for deployments before updating. Review this [release note](cs_versions.html) for details.

<strong>Command options</strong>:

   <dl>

   <dt><em>CLUSTER</em></dt>
   <dd>The name or ID of the cluster where you list available worker nodes. This value is required.</dd>

   <dt><code>-f</code></dt>
   <dd>Use this option to force the update of the master with no user prompts. This value is optional.</dd>

   <dt><code><em>WORKER</em></code></dt>
   <dd>The ID of one or more worker nodes. Use a space to list multiple worker nodes. This value is required.</dd>
   </dl>

**Example**:

  ```
  bx cs worker-update my_cluster my_node1 my_node2
  ```
  {: pre}

### bx cs workers CLUSTER
{: #cs_workers}

View a list of worker nodes and the status for each in a cluster.

<strong>Command options</strong>:

   <dl>
   <dt><em>CLUSTER</em></dt>
   <dd>The name or ID of the cluster where you list available worker nodes. This value is required.</dd>
   </dl>

**Example**:

  ```
  bx cs workers mycluster
  ```
  {: pre}
