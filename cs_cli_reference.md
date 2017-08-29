---

copyright:
  years: 2014, 2017
lastupdated: "2017-08-14"

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

**Tip:** Looking for `bx cr` commands? See the [{{site.data.keyword.registryshort_notm}} CLI reference](/docs/cli/plugins/registry/index.html#containerregcli). Looking for `kubectl` commands? See the [Kubernetes documentation ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/user-guide/kubectl/v1.5/).

 
<!--[https://github.ibm.com/alchemy-containers/armada-cli ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.ibm.com/alchemy-containers/armada-cli)-->

<table summary="Commands for creating clusters on {{site.data.keyword.Bluemix_notm}}">
 <thead>
    <th colspan=5>Commands for creating clusters on {{site.data.keyword.Bluemix_notm}}</th>
 </thead>
 <tbody>
 <tr>
    <td>[bx cs cluster-config](cs_cli_reference.html#cs_cluster_config)</td>
    <td>[bx cs cluster-create](cs_cli_reference.html#cs_cluster_create)</td> 
    <td>[bx cs cluster-get](cs_cli_reference.html#cs_cluster_get)</td>
    <td>[bx cs cluster-rm](cs_cli_reference.html#cs_cluster_rm)</td>
    <td>[bx cs cluster-service-bind](cs_cli_reference.html#cs_cluster_service_bind)</td>
 </tr>
 <tr>
    <td>[bx cs cluster-service-unbind](cs_cli_reference.html#cs_cluster_service_unbind)</td>
    <td>[bx cs cluster-services](cs_cli_reference.html#cs_cluster_services)</td> 
    <td>[bx cs cluster-subnet-add](cs_cli_reference.html#cs_cluster_subnet_add)</td>
    <td>[bx cs clusters](cs_cli_reference.html#cs_clusters)</td>
    <td>[bx cs credentials-set](cs_cli_reference.html#cs_credentials_set)</td>
 </tr>
 <tr>
   <td>[bx cs credentials-unset](cs_cli_reference.html#cs_credentials_unset)</td>
   <td>[bx cs locations](cs_cli_reference.html#cs_datacenters)</td> 
   <td>[bx cs help](cs_cli_reference.html#cs_help)</td>
   <td>[bx cs init](cs_cli_reference.html#cs_init)</td>
   <td>[bx cs machine-types](cs_cli_reference.html#cs_machine_types)</td>
   </tr>
 <tr>
    <td>[bx cs subnets](cs_cli_reference.html#cs_subnets)</td>
    <td>[bx cs vlans](cs_cli_reference.html#cs_vlans)</td> 
    <td>[bx cs webhook-create](cs_cli_reference.html#cs_webhook_create)</td>
    <td>[bx cs worker-add](cs_cli_reference.html#cs_worker_add)</td>
    <td>[bx cs worker-get](cs_cli_reference.html#cs_worker_get)</td>
    </tr>
 <tr>
   <td>[bx cs worker-reboot](cs_cli_reference.html#cs_worker_reboot)</td>
   <td>[bx cs worker-reload](cs_cli_reference.html#cs_worker_reload)</td> 
   <td>[bx cs worker-rm](cs_cli_reference.html#cs_worker_rm)</td>
   <td>[bx cs workers](cs_cli_reference.html#cs_workers)</td>
   
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

### bx cs cluster-config CLUSTER [--admin]
{: #cs_cluster_config}

After logging in, download Kubernetes configuration data and certificates to connect to your cluster and to run `kubectl` commands. The files are downloaded to `user_home_directory/.bluemix/plugins/container-service/clusters/<cluster_name>`.

**Command options**:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>(Required) The name or ID of the cluster.</dd>

   <dt><code>--admin</code></dt>
   <dd>(Optional) Download the certificates and permission files for the Administrator rbac role. Users with these files can perform admin actions on the cluster, such as removing the cluster.</dd>
   </dl>

**Examples**:

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

<dd>(Optional for standard clusters. Not available for lite clusters.) The path to the YAML file to create your standard cluster. Instead of defining the characteristics of your cluster by using the options provided in this command, you can use a YAML file.

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
    <th colspan=2><img src="images/idea.png"/> Understanding the YAML file components</th>
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
<dd>(Optional for standard clusters. Not available for lite clusters.) The level of hardware isolation for your worker node. Use dedicated if you want to have available physical resources dedicated to you only, or shared to allow physical resources to be shared with other IBM customers. The default is shared.</dd>

<dt><code>--location <em>LOCATION</em></code></dt>
<dd>(Required for standard clusters. Optional for lite clusters.) The location where you want to create the cluster. The locations that are available to you depend on the {{site.data.keyword.Bluemix_notm}} region you are logged in to. Select the region that is physically closest to you for best performance.

<p>Available locations are:<ul><li>US-South<ul><li>dal10 [Dallas]</li><li>dal12 [Dallas]</li></ul></li><li>UK-South<ul><li>lon02 [London]</li><li>lon04 [London]</li></ul></li><li>EU-Central<ul><li>ams03 [Amsterdam]</li><li>ra02 [Frankfurt]</li></ul></li><li>AP-South<ul><li>syd01 [Sydney]</li><li>syd04 [Sydney]</li></ul></li></ul>
</p>

<p><strong>Note:</strong> When you select a location that is located outside your country, keep in mind that you might require legal authorization before data can be physically stored in a foreign country.</p>
</dd>

<dt><code>--machine-type <em>MACHINE_TYPE</em></code></dt>
<dd>(Required for standard clusters. Not available for lite clusters.) The machine type that you choose impacts the amount of memory and disk space that is available to the containers that are deployed to your worker node. To list available machine types, see [bx cs machine-types <em>LOCATION</em>](cs_cli_reference.html#cs_machine_types).</dd>

<dt><code>--name <em>NAME</em></code></dt>
<dd>(Required) The name for the cluster.</dd>

<dt><code>--no-subnet</code></dt>
<dd>Include the flag to create a cluster without a portable subnet. The default is to not use the flag and to create a subnet in your {{site.data.keyword.BluSoftlayer_full}} portfolio.</dd>

<dt><code>--private-vlan <em>PRIVATE_VLAN</em></code></dt>
<dd>(Not available for lite clusters.)

<ul>
<li>If this cluster is the first cluster that you create in this location, do not include this flag. A private VLAN is created for you when the clusters is created.</li>
<li>If you created a cluster before in this location or created a private VLAN in {{site.data.keyword.BluSoftlayer_notm}} before, you must specify that private VLAN.

<p><strong>Note:</strong> The public and private VLANs that you specify with the create command must match. Private VLAN routers always begin with <code>bcr</code> (back-end router) and public VLAN routers always begin with <code>fcr</code> (front-end router). The number and letter combination after those prefixes must match to use those VLANs when creating a cluster. Do not use public and private VLANs that do not match to create a cluster.</p></li>
</ul>

<p>To find out if you already have a private VLAN for a specific location or to find the name of an existing private VLAN, run <code>bx cs vlans <em>&lt;location&gt;</em></code>.</p></dd>

<dt><code>--public-vlan <em>PUBLIC_VLAN</em></code></dt>
<dd>(Not available for lite clusters.)

<ul>
<li>If this cluster is the first cluster that you create in this location, do not use this flag. A public VLAN is created for you when the cluster is created.</li>
<li>If you created a cluster before in this location or created a public VLAN in {{site.data.keyword.BluSoftlayer_notm}} before, you must specify that public VLAN.

<p><strong>Note:</strong> The public and private VLANs that you specify with the create command must match. Private VLAN routers always begin with <code>bcr</code> (back-end router) and public VLAN routers always begin with <code>fcr</code> (front-end router). The number and letter combination after those prefixes must match to use those VLANs when creating a cluster. Do not use public and private VLANs that do not match to create a cluster.</p></li>
</ul>

<p>To find out if you already have a public VLAN for a specific location or to find the name of an existing public VLAN, run <code>bx cs vlans <em>&lt;location&gt;</em></code>.</p></dd>

<dt><code>--workers WORKER</code></dt>
<dd>(Optional for standard clusters. Not available for lite clusters.) The number of worker nodes that you want to deploy in your cluster. If you do not specify this option, a cluster with 1 worker node is created.

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

  Example for a {{site.data.keyword.Bluemix_notm}} Dedicated environment:

  ```
  bx cs cluster-create --machine-type machine-type --workers number --name cluster_name
  ```
  {: pre}


### bx cs cluster-get CLUSTER
{: #cs_cluster_get}

View information about a cluster in your organization.

<strong>Command options</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>(Required) The name or ID of the cluster.</dd>
   </dl>

**Examples**:

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
   <dd>(Required) The name or ID of the cluster.</dd>

   <dt><code>-f</code></dt>
   <dd>(Optional) Use this option to force the removal of a cluster with no user prompts.</dd>
   </dl>

**Examples**:

  ```
  bx cs cluster-rm my_cluster
  ```
  {: pre}


### bx cs cluster-service-bind CLUSTER KUBERNETES_NAMESPACE SERVICE_INSTANCE_GUID
{: #cs_cluster_service_bind}

Add a {{site.data.keyword.Bluemix_notm}} service to a cluster.

**Tip:** For {{site.data.keyword.Bluemix_notm}} Dedicated users, see [Adding {{site.data.keyword.Bluemix_notm}} services to clusters in {{site.data.keyword.Bluemix_notm}} Dedicated (Closed Beta)](cs_cluster.html#binding_dedicated).

<strong>Command options</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>(Required) The name or ID of the cluster.</dd>

   <dt><code><em>KUBERNETES_NAMESPACE</em></code></dt>
   <dd>(Required) The name of the Kubernetes namespace.</dd>

   <dt><code><em>SERVICE_INSTANCE_GUID</em></code></dt>
   <dd>(Required) The ID of the {{site.data.keyword.Bluemix_notm}} service instance that you want to bind.</dd>
   </dl>

**Examples**:

  ```
  bx cs cluster-service-bind my_cluster my_namespace my_service_instance_GUID
  ```
  {: pre}


### bx cs cluster-service-unbind CLUSTER KUBERNETES_NAMESPACE SERVICE_INSTANCE_GUID
{: #cs_cluster_service_unbind}

Remove a {{site.data.keyword.Bluemix_notm}} service from a cluster.

**Note:** When you remove a {{site.data.keyword.Bluemix_notm}} service, the service credentials are removed from the cluster. If a pod is still using the service, it fails because the service credentials cannot be found.

<strong>Command options</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>(Required) The name or ID of the cluster.</dd>

   <dt><code><em>KUBERNETES_NAMESPACE</em></code></dt>
   <dd>(Required) The name of the Kubernetes namespace.</dd>

   <dt><code><em>SERVICE_INSTANCE_GUID</em></code></dt>
   <dd>(Required) The ID of the {{site.data.keyword.Bluemix_notm}} service instance that you want to remove.</dd>
   </dl>

**Examples**:

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
   <dd>(Required) The name or ID of the cluster.</dd>

   <dt><code>--namespace <em>KUBERNETES_NAMESPACE</em></code>, <code>-n <em>KUBERNETES_NAMESPACE</em></code></dt>
   <dd>(Optional) Include the services that are bound to a specific namespace in a cluster.</dd>

   <dt><code>--all-namespaces</code></dt>
    <dd>(Optional) Include the services that are bound to all of the namespaces in a cluster.</dd>
    </dl>

**Examples**:

  ```
  bx cs cluster-services my_cluster --namespace my_namespace
  ```
  {: pre}


### bx cs cluster-subnet-add CLUSTER SUBNET
{: #cs_cluster_subnet_add}

Make a subnet in a {{site.data.keyword.BluSoftlayer_notm}} account available to a specified cluster.

**Note:** When you make a subnet available to a cluster, IP addresses of this subnet are used for cluster networking purposes. To avoid IP address conflicts, make sure that you use a subnet with one cluster only. Do not use a subnet for multiple clusters or for other purposes outside of {{site.data.keyword.containershort_notm}} at the same time.

<strong>Command options</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>(Required) The name or ID of the cluster.</dd>

   <dt><code><em>SUBNET</em></code></dt>
   <dd>(Required) The ID of the subnet.</dd>
   </dl>

**Examples**:

  ```
  bx cs cluster-subnet-add my_cluster subnet
  ```
  {: pre}



### bx cs clusters
{: #cs_clusters}

View a list of clusters in your organization.

<strong>Command options</strong>:

  None

**Examples**:

  ```
  bx cs clusters
  ```
  {: pre}


### bx cs credentials-set --infrastructure-api-key API_KEY --infrastructure-username USERNAME
{: #cs_credentials_set}

Set {{site.data.keyword.BluSoftlayer_notm}} account credentials for your {{site.data.keyword.Bluemix_notm}} account. These credentials allow you to access the {{site.data.keyword.BluSoftlayer_notm}} portfolio through your {{site.data.keyword.Bluemix_notm}} account.

<strong>Command options</strong>:

   <dl>
   <dt><code>--infrastructure-username <em>USERNAME</em></code></dt>
   <dd>(Required) A {{site.data.keyword.BluSoftlayer_notm}} account username.</dd>
   </dl>

   <dt><code>--infrastructure-api-key <em>API_KEY</em></code></dt>
   <dd>(Required) A {{site.data.keyword.BluSoftlayer_notm}} account API key.
   
 <p>
  To generate an API key:
    
  <ol>
  <li>Log in to the [{{site.data.keyword.BluSoftlayer_notm}} portal ![External link icon](../icons/launch-glyph.svg "External link icon")](https://control.softlayer.com/).</li>
  <li>Select <strong>Account</strong>, and then <strong>Users</strong>.</li>
  <li>Click <strong>Generate</strong> to generate a {{site.data.keyword.BluSoftlayer_notm}} API key for your account.</li>
  <li>Copy the API key to use in this command.</li>
  </ol>

  To view your existing API key:
  <ol>
  <li>Log in to the [{{site.data.keyword.BluSoftlayer_notm}} portal ![External link icon](../icons/launch-glyph.svg "External link icon")](https://control.softlayer.com/).</li>
  <li>Select <strong>Account</strong>, and then <strong>Users</strong>.</li>
  <li>Click <strong>View</strong> to see your existing API key.</li>
  <li>Copy the API key to use in this command.</li>
  </ol></p></dd>
    
**Examples**:

  ```
  bx cs credentials-set --infrastructure-api-key API_KEY --infrastructure-username USERNAME
  ```
  {: pre}


### bx cs credentials-unset
{: #cs_credentials_unset}

Remove {{site.data.keyword.BluSoftlayer_notm}} account credentials from your {{site.data.keyword.Bluemix_notm}} account. After removing the credentials, you cannot access the {{site.data.keyword.BluSoftlayer_notm}} portfolio through your {{site.data.keyword.Bluemix_notm}} account anymore.

<strong>Command options</strong>:

   None

**Examples**:

  ```
  bx cs credentials-unset
  ```
  {: pre}


### bx cs locations
{: #cs_datacenters}

View a list of available locations for you to create a cluster in.

<strong>Command options</strong>:

   None

**Examples**:

  ```
  bx cs locations
  ```
  {: pre}


### bx cs help
{: #cs_help}

View a list of supported commands and parameters.

<strong>Command options</strong>:

   None

**Examples**:

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
   <dd>(Optional) The {{site.data.keyword.containershort_notm}} API endpoint that you want to use. Examples:

    <ul>
    <li>US-South:

    <pre class="codeblock">
    <code>bx cs init --host https://us-south.containers.bluemix.net</code>
    </pre></li>

    <li>UK-South:

    <pre class="codeblock">
    <code>bx cs init --host https://uk-south.containers.bluemix.net</code>
    </pre></li>

    <li>EU-Central:

    <pre class="codeblock">
    <code>bx cs init --host https://eu-central.containers.bluemix.net</code>
    </pre></li>

    <li>AP-South:

    <pre class="codeblock">
    <code>bx cs init --host https://ap-south.containers.bluemix.net</code>
    </pre></li></ul>
</dd>
</dl>



### bx cs machine-types LOCATION
{: #cs_machine_types}

View a list of available machine types for your worker nodes. Each machine type includes the amount of virtual CPU, memory, and disk space for each worker node in the cluster.

<strong>Command options</strong>:

   <dl>
   <dt><em>LOCATION</em></dt>
   <dd>(Required) Enter the location where you want to list available machine types. Available locations are: <ul><li>US-South<ul><li>dal10 [Dallas]</li><li>dal12 [Dallas]</li></ul></li><li>UK-South<ul><li>lon02 [London]</li><li>lon04 [London]</li></ul></li><li>EU-Central<ul><li>ams03 [Amsterdam]</li><li>ra02 [Frankfurt]</li></ul></li><li>AP-South<ul><li>syd01 [Sydney]</li><li>syd04 [Sydney]</li></ul></li></ul></dd></dl>
   
**Examples**:

  ```
  bx cs machine-types LOCATION
  ```
  {: pre}


### bx cs subnets
{: #cs_subnets}

View a list of subnets that are available in a {{site.data.keyword.BluSoftlayer_notm}} account.

<strong>Command options</strong>:

   None

**Examples**:

  ```
  bx cs subnets
  ```
  {: pre}


### bx cs vlans LOCATION
{: #cs_vlans}

List the public and private VLANs that are available for a location in your {{site.data.keyword.BluSoftlayer_notm}} account. To list available VLANs, you must have a paid account.

<strong>Command options</strong>:

   <dl>
   <dt>LOCATION</dt>
   <dd>(Required) Enter the location where you want to list your private and public VLANs. Available locations are: <ul><li>US-South<ul><li>dal10 [Dallas]</li><li>dal12 [Dallas]</li></ul></li><li>UK-South<ul><li>lon02 [London]</li><li>lon04 [London]</li></ul></li><li>EU-Central<ul><li>ams03 [Amsterdam]</li><li>ra02 [Frankfurt]</li></ul></li><li>AP-South<ul><li>syd01 [Sydney]</li><li>syd04 [Sydney]</li></ul></li></ul></dd>
   </dl>
   
**Examples**:

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
   <dd>(Required) The name or ID of the cluster.</dd>

   <dt><code>--level <em>LEVEL</em></code></dt>
   <dd>(Optional) The notification level, such as <code>Normal</code> or <code>Warning</code>. <code>Warning</code> is the default value.</dd>

   <dt><code>--type <em>slack</em></code></dt>
   <dd>(Required) The webhook type, such as slack. Only slack is supported.</dd>

   <dt><code>--URL <em>URL</em></code></dt>
   <dd>(Required) The URL for the webhook.</dd>
   </dl>

**Examples**:

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
<dd>(Required) The name or ID of the cluster.</dd>

<dt><code>--file <em>FILE_LOCATION</em></code></dt>
<dd>The path to the YAML file to add worker nodes to your cluster. Instead of defining your additional worker nodes by using the options provided in this command, you can use a YAML file.

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
<th colspan=2><img src="images/idea.png"/> Understanding the YAML file components</th>
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
<dd>(Optional) The level of hardware isolation for your worker node. Use dedicated if you want to have available physical resources dedicated to you only, or shared to allow physical resources to be shared with other IBM customers. The default is shared.</dd>

<dt><code>--machine-type <em>MACHINE_TYPE</em></code></dt>
<dd>(Required) The machine type that you choose impacts the amount of memory and disk space that is available to the containers that are deployed to your worker node. To list available machine types, see [bx cs machine-types LOCATION](cs_cli_reference.html#cs_machine_types).</dd>

<dt><code>--number <em>NUMBER</em></code></dt>
<dd>(Required) An integer that represents the number of worker nodes to create in the cluster.</dd>

<dt><code>--private-vlan <em>PRIVATE_VLAN</em></code></dt>
<dd>(Required) If you have a private VLAN available to use in the location, you must specify the VLAN. If this is the first cluster that you create in this location, do not use this flag. A private VLAN is created for you.

<p><strong>Note:</strong> The public and private VLANs that you specify with the create command must match. Private VLAN routers always begin with <code>bcr</code> (back-end router) and public VLAN routers always begin with <code>fcr</code> (front-end router). The number and letter combination after those prefixes must match to use those VLANs when creating a cluster. Do not use public and private VLANs that do not match to create a cluster.</p></dd>

<dt><code>--public-vlan <em>PUBLIC_VLAN</em></code></dt>
<dd>(Required) If you have a public VLAN available to use in the location, you must specify the VLAN. If this is the first cluster that you create in this location, do not use this flag. A public VLAN is created for you.

<p><strong>Note:</strong> The public and private VLANs that you specify with the create command must match. Private VLAN routers always begin with <code>bcr</code> (back-end router) and public VLAN routers always begin with <code>fcr</code> (front-end router). The number and letter combination after those prefixes must match to use those VLANs when creating a cluster. Do not use public and private VLANs that do not match to create a cluster.</p></dd>
</dl>

**Examples**:

  ```
  bx cs worker-add --cluster my_cluster --number 3 --public-vlan my_public_vlan_id --private-vlan my_private_vlan_id --machine-type u1c.2x4 --hardware shared
  ```
  {: pre}

  Example for {{site.data.keyword.Bluemix_notm}} Dedicated:

  ```
  bx cs worker-add --cluster my_cluster --number 3 --machine-type u1c.2x4
  ```
  {: pre}


### bx cs worker-get WORKER_NODE_ID
{: #cs_worker_get}

View details of a worker node.

<strong>Command options</strong>:

   <dl>
   <dt><em>WORKER_NODE_ID</em></dt>
   <dd>The ID for a worker node. Run <code>bx cs workers <em>CLUSTER</em></code> to view the IDs for the worker nodes in a cluster.</dd>
   </dl>

**Examples**:

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
   <dd>(Required) The name or ID of the cluster.</dd>

   <dt><code>-f</code></dt>
   <dd>(Optional) Use this option to force the restart of the worker node with no user prompts.</dd>

   <dt><code>--hard</code></dt>
   <dd>(Optional) Use this option to force a hard restart of a worker node by cutting off power to the worker node. Use this option if the worker node is unresponsive or the worker node has a Docker hang.</dd>

   <dt><code><em>WORKER</em></code></dt>
   <dd>(Required) The name or ID of one or more worker nodes. Use a space to list multiple worker nodes.</dd>
   </dl>

**Examples**:

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
   <dd>(Required) The name or ID of the cluster.</dd>

   <dt><code>-f</code></dt>
   <dd>(Optional) Use this option to force the reload of a worker node with no user prompts.</dd>

   <dt><code><em>WORKER</em></code></dt>
   <dd>(Required) The name or ID of one or more worker nodes. Use a space to list multiple worker nodes.</dd>
   </dl>

**Examples**:

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
   <dd>(Required) The name or ID of the cluster.</dd>

   <dt><code>-f</code></dt>
   <dd>(Optional) Use this option to force the removal of a worker node with no user prompts.</dd>

   <dt><code><em>WORKER</em></code></dt>
   <dd>(Required) The name or ID of one or more worker nodes. Use a space to list multiple worker nodes.</dd>
   </dl>

**Examples**:

  ```
  bx cs worker-rm my_cluster my_node1 my_node2
  ```
  {: pre}
  


### bx cs workers CLUSTER
{: #cs_workers}

View a list of worker nodes and the status for each in a cluster.

<strong>Command options</strong>:

   <dl>
   <dt><em>CLUSTER</em></dt>
   <dd>(Required) The name or ID of the cluster where you list available worker nodes.</dd>
   </dl>

**Examples**:

  ```
  bx cs workers mycluster
  ```
  {: pre}

## Cluster states
{: #cs_cluster_states}

You can view the current cluster state by running the bx cs clusters command and locating the **State** field. The cluster state gives you information about the availability and capacity of the cluster, and potential problems that might have occurred.
{:shortdesc}

|Cluster state|Reason|
|-------------|------|
|Deploying|The Kubernetes master is not fully deployed yet. You cannot access your cluster.|
|Pending|The Kubernetes master is deployed. The worker nodes are being provisioned and are not available in the cluster yet. You can access the cluster, but you cannot deploy apps to the cluster.|
|Normal|All worker nodes in a cluster are up and running. You can access the cluster and deploy apps to the cluster.|
|Warning|At least one worker node in the cluster is not available, but other worker nodes are available and can take over the workload. <ol><li>List the worker nodes in your cluster and note the ID of the worker nodes that show a <strong>Warning</strong> state.<pre class="pre"><code>bx cs workers &lt;cluster_name_or_id&gt;</code></pre><li>Get the details for a worker node.<pre class="pre"><code>bx cs worker-get &lt;worker_id&gt;</code></pre><li>Review the <strong>State</strong>, <strong>Status</strong>, and <strong>Details</strong> fields to find the root problem for why the worker node is down.</li><li>If your worker node almost reached the memory or disk space limit, reduce work load on your worker node or add a worker node to your cluster to help load balance the work load.</li></ol>|
|Critical|The Kubernetes master cannot be reached or all worker nodes in the cluster are down. <ol><li>List the worker nodes in your cluster.<pre class="pre"><code>bx cs workers &lt;cluser_name_or_id&gt;</code></pre><li>Get the details for each worker node.<pre class="pre"><code>bx cs worker-get &lt;worker_id&gt;</code></pre></li><li>Review the <strong>State</strong>, <strong>Status</strong>, and <strong>Details</strong> field to find the root problem for why the worker node is down.</li><li>If the worker node state shows <strong>Provision_failed</strong>, you might not have the required permissions to provision a worker node from the {{site.data.keyword.BluSoftlayer_notm}} portfolio. To find the required permissions, see [Configure access to the {{site.data.keyword.BluSoftlayer_notm}} portfolio to create standard Kubernetes clusters](cs_planning.html#cs_planning_unify_accounts).</li><li>If the worker node state shows <strong>Critical</strong> and the worker node status shows <strong>Out of disk</strong>, then your worker node ran out of capacity. You can either reduce work load on your worker node or add a worker node to your cluster to help load balance the work load.</li><li>If the worker node state shows <strong>Critical</strong> and the worker node status shows <strong>Unknown</strong>, then the Kubernetes master is not available. Contact {{site.data.keyword.Bluemix_notm}} support by opening a [{{site.data.keyword.Bluemix_notm}} support ticket](/docs/support/index.html#contacting-support).</li></ol>|
{: caption="Table 3. Cluster states" caption-side="top"}
