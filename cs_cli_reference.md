---

copyright:
  years: 2014, 2018
lastupdated: "2018-01-18"

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

Refer to these commands to create and manage clusters on {{site.data.keyword.Bluemix_notm}}.
{:shortdesc}

## bx cs commands
{: #cs_commands}

**Tip:** Looking for `bx cr` commands? See the [{{site.data.keyword.registryshort_notm}} CLI reference](/docs/cli/plugins/registry/index.html). Looking for `kubectl` commands? See the [Kubernetes documentation ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands).

**Tip:** To see the version of the {{site.data.keyword.containershort_notm}} plug-in, run the following command.

```
bx plugin list
```
{: pre}

<!--[https://github.ibm.com/alchemy-containers/armada-cli ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.ibm.com/alchemy-containers/armada-cli)-->

<table summary="Application load balancer commands">
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>Application load balancer commands</th>
 </thead>
 <tbody>
  <tr>
    <td>[bx cs alb-cert-deploy](#cs_alb_cert_deploy)</td>
    <td>[bx cs alb-cert-get](#cs_alb_cert_get)</td>
    <td>[bx cs alb-cert-rm](#cs_alb_cert_rm)</td>
    <td>[bx cs alb-certs](#cs_alb_certs)</td>
  </tr>
  <tr>
    <td>[bx cs alb-configure](#cs_alb_configure)</td>
    <td>[bx cs alb-get](#cs_alb_get)</td>
    <td>[bx cs alb-types](#cs_alb_types)</td>
    <td>[bx cs albs](#cs_albs)</td>
 </tr>
</tbody>
</table>

<br>

<table summary="API commands">
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>API commands</th>
 </thead>
 <tbody>
  <tr>
    <td>[bx cs api-key-info](#cs_api_key_info)</td>
    <td>[bx cs api-key-reset](#cs_api_key_reset)</td>
    <td>[bx cs apiserver-config-get](#cs_apiserver_config_get)</td>
    <td>[bx cs apiserver-config-set](#cs_apiserver_config_set)</td>
  </tr>
  <tr>
    <td>[bx cs apiserver-config-unset](#cs_apiserver_config_unset)</td>
    <td>[bx cs apiserver-refresh](#cs_apiserver_refresh)</td>
    <td></td>
    <td></td>
 </tr>
</tbody>
</table>

<br>

<table summary="CLI plug-in usage commands">
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>CLI plug-in usage commands</th>
 </thead>
 <tbody>
  <tr>
    <td>[bx cs help](#cs_help)</td>
    <td>[bx cs init](#cs_init)</td>
    <td>[bx cs messages](#cs_messages)</td>
    <td></td>
  </tr>
</tbody>
</table>

<br>

<table summary="Cluster commands: Management">
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>Cluster commands: Management</th>
 </thead>
 <tbody>
  <tr>
    <td>[bx cs cluster-config](#cs_cluster_config)</td>
    <td>[bx cs cluster-create](#cs_cluster_create)</td>
    <td>[bx cs cluster-get](#cs_cluster_get)</td>
    <td>[bx cs cluster-rm](#cs_cluster_rm)</td>
  </tr>
  <tr>
    <td>[bx cs cluster-update](#cs_cluster_update)</td>
    <td>[bx cs clusters](#cs_clusters)</td>
    <td>[bx cs kube-versions](#cs_kube_versions)</td>
    <td></td>
  </tr>
</tbody>
</table>

<br>

<table summary="Cluster commands: Services and integrations">
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>Cluster commands: Services and integrations</th>
 </thead>
 <tbody>
  <tr>
    <td>[bx cs cluster-service-bind](#cs_cluster_service_bind)</td>
    <td>[bx cs cluster-service-unbind](#cs_cluster_service_unbind)</td>
    <td>[bx cs cluster-services](#cs_cluster_services)</td>
    <td>[bx cs webhook-create](#cs_webhook_create)</td>
  </tr>
</tbody>
</table>

<br>

<table summary="Cluster commands: Subnets">
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>Cluster commands: Subnets</th>
 </thead>
 <tbody>
  <tr>
    <td>[bx cs cluster-subnet-add](#cs_cluster_subnet_add)</td>
    <td>[bx cs cluster-subnet-create](#cs_cluster_subnet_create)</td>
    <td>[bx cs cluster-user-subnet-add](#cs_cluster_user_subnet_add)</td>
    <td>[bx cs cluster-user-subnet-rm](#cs_cluster_user_subnet_rm)</td>
  </tr>
  <tr>
    <td>[bx cs subnets](#cs_subnets)</td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
</tbody>
</table>

<br>

<table summary="Infrastructure commands">
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>Infrastructure commands</th>
 </thead>
 <tbody>
  <tr>
    <td>[bx cs credentials-set](#cs_credentials_set)</td>
    <td>[bx cs credentials-unset](#cs_credentials_unset)</td>
    <td>[bx cs machine-types](#cs_machine_types)</td>
    <td>[bx cs vlans](#cs_vlans)</td>
  </tr>
</tbody>
</table>

<br>

<table summary="Logging commands">
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>Logging commands</th>
 </thead>
 <tbody>
  <tr>
    <td>[bx cs logging-config-create](#cs_logging_create)</td>
    <td>[bx cs logging-config-get](#cs_logging_get)</td>
    <td>[bx cs logging-config-refresh](#cs_logging_refresh)</td>
    <td>[bx cs logging-config-rm](#cs_logging_rm)</td>
  </tr>
  <tr>
    <td>[bx cs logging-config-update](#cs_logging_update)</td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
</tbody>
</table>

<br>

<table summary="Region commands">
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>Region commands</th>
 </thead>
 <tbody>
  <tr>
    <td>[bx cs locations](#cs_datacenters)</td>
    <td>[bx cs region](#cs_region)</td>
    <td>[bx cs region-set](#cs_region-set)</td>
    <td>[bx cs regions](#cs_regions)</td>
  </tr>
</tbody>
</table>

<br>

<table summary="Worker node commands">
<col width="25%">
<col width="25%">
<col width="25%">
 <thead>
    <th colspan=4>Worker node commands</th>
 </thead>
 <tbody>
  <tr>
    <td>[bx cs worker-add](#cs_worker_add)</td>
    <td>[bx cs worker-get](#cs_worker_get)</td>
    <td>[bx cs worker-reboot](#cs_worker_reboot)</td>
    <td>[bx cs worker-reload](#cs_worker_reload)</td>
  </tr>
  <tr>
    <td>[bx cs worker-rm](#cs_worker_rm)</td>
    <td>[bx cs worker-update](#cs_worker_update)</td>
    <td>[bx cs workers](#cs_workers)</td>
    <td></td>
  </tr>
</tbody>
</table>
</staging>

## Application load balancer commands
{: #alb_commands}

### bx cs alb-cert-deploy [--update] --cluster CLUSTER --secret-name SECRET_NAME --cert-crn CERTIFICATE_CRN
{: #cs_alb_cert_deploy}

Deploy or update a certificate from your {{site.data.keyword.cloudcerts_long_notm}} instance to the application load balancer in a cluster.

**Note:**
* Only a user with the Administrator access role can execute this command.
* You can only update certificates that are imported from the same {{site.data.keyword.cloudcerts_long_notm}} instance.

<strong>Command options</strong>

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>The name or ID of the cluster. This value is required.</dd>

   <dt><code>--update</code></dt>
   <dd>Include this flag to update the certificate for an application load balancer secret in a cluster. This value is optional.</dd>

   <dt><code>--secret-name <em>SECRET_NAME</em></code></dt>
   <dd>The name of the application load balancer secret. This value is required.</dd>

   <dt><code>--cert-crn <em>CERTIFICATE_CRN</em></code></dt>
   <dd>The certificate CRN. This value is required.</dd>
   </dl>

**Examples**:

Example for deploying an application load balancer secret:

   ```
   bx cs alb-cert-deploy --secret-name my_alb_secret_name --cluster my_cluster --cert-crn crn:v1:staging:public:cloudcerts:us-south:a/06580c923e40314421d3b6cb40c01c68:0db4351b-0ee1-479d-af37-56a4da9ef30f:certificate:4bc35b7e0badb304e60aef00947ae7ff
   ```
 {: pre}

Example for updating an existing application load balancer secret:

 ```
 bx cs alb-cert-deploy --update --secret-name my_alb_secret_name --cluster my_cluster --cert-crn crn:v1:staging:public:cloudcerts:us-south:a/06580c923e40314421d3b6cb40c01c68:0db4351b-0ee1-479d-af37-56a4da9ef30f:certificate:7e21fde8ee84a96d29240327daee3eb2
 ```
 {: pre}


### bx cs alb-cert-get --cluster CLUSTER [--secret-name SECRET_NAME] [--cert-crn CERTIFICATE_CRN]
{: #cs_alb_cert_get}

View information about an application load balancer secret in a cluster.

**Note:** Only a user with the Administrator access role can execute this command.

<strong>Command options</strong>

  <dl>
  <dt><code>--cluster <em>CLUSTER</em></code></dt>
  <dd>The name or ID of the cluster. This value is required.</dd>

  <dt><code>--secret-name <em>SECRET_NAME</em></code></dt>
  <dd>The name of the application load balancer secret. This value is required to get information on a specific application load balancer secret in the cluster.</dd>

  <dt><code>--cert-crn <em>CERTIFICATE_CRN</em></code></dt>
  <dd>The certificate CRN. This value is required to get information on all application load balancer secrets matching a specific certificate CRN in the cluster.</dd>
  </dl>

**Examples**:

 Example for fetching information on an application load balancer secret:

 ```
 bx cs alb-cert-get --cluster my_cluster --secret-name my_alb_secret_name
 ```
 {: pre}

 Example for fetching information on all application load balancer secrets that match a specified certificate CRN:

 ```
 bx cs alb-cert-get --cluster my_cluster --cert-crn  crn:v1:staging:public:cloudcerts:us-south:a/06580c923e40314421d3b6cb40c01c68:0db4351b-0ee1-479d-af37-56a4da9ef30f:certificate:4bc35b7e0badb304e60aef00947ae7ff
 ```
 {: pre}


### bx cs alb-cert-rm --cluster CLUSTER [--secret-name SECRET_NAME] [--cert-crn CERTIFICATE_CRN]
{: #cs_alb_cert_rm}

Remove an application load balancer secret in a cluster.

**Note:** Only a user with the Administrator access role can execute this command.

<strong>Command options</strong>

  <dl>
  <dt><code>--cluster <em>CLUSTER</em></code></dt>
  <dd>The name or ID of the cluster. This value is required.</dd>

  <dt><code>--secret-name <em>SECRET_NAME</em></code></dt>
  <dd>The name of the ALB secret. This value is required to remove a specific application load balancer secret in the cluster.</dd>

  <dt><code>--cert-crn <em>CERTIFICATE_CRN</em></code></dt>
  <dd>The certificate CRN. This value is required to remove all application load balancer secrets matching a specific certificate CRN in the cluster.</dd>
  </dl>

**Examples**:

 Example for removing an application load balancer secret:

 ```
 bx cs alb-cert-rm --cluster my_cluster --secret-name my_alb_secret_name
 ```
 {: pre}

 Example for removing all application load balancer secrets that match a specified certificate CRN:

 ```
 bx cs alb-cert-rm --cluster my_cluster --cert-crn crn:v1:staging:public:cloudcerts:us-south:a/06580c923e40314421d3b6cb40c01c68:0db4351b-0ee1-479d-af37-56a4da9ef30f:certificate:4bc35b7e0badb304e60aef00947ae7ff
 ```
 {: pre}


### bx cs alb-certs --cluster CLUSTER
{: #cs_alb_certs}

View a list of application load balancer secrets in a cluster.

**Note:** Only a user with the Administrator access role can execute this command.

<strong>Command options</strong>

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>The name or ID of the cluster. This value is required.</dd>
   </dl>

**Example**:

 ```
 bx cs alb-certs --cluster my_cluster
 ```
 {: pre}


### bx cs alb-configure --albID ALB_ID [--enable][--disable][--user-ip USERIP]
{: #cs_alb_configure}

Enable or disable an application load balancer in your standard cluster. The public application load balancer is enabled by default.

**Command options**:

   <dl>
   <dt><code><em>--albID </em>ALB_ID</code></dt>
   <dd>The ID for an application load balancer. Run <code>bx cs albs <em>--cluster </em>CLUSTER</code> to view the IDs for the application load balancers in a cluster. This value is required.</dd>

   <dt><code>--enable</code></dt>
   <dd>Include this flag to enable an application load balancer in a cluster.</dd>

   <dt><code>--disable</code></dt>
   <dd>Include this flag to disable an application load balancer in a cluster.</dd>

   <dt><code>--user-ip <em>USER_IP</em></code></dt>
   <dd>

   <ul>
    <li>This parameter is available for a private application load balancer only</li>
    <li>The private application load balancer is deployed with an IP address from a user-provided private subnet. If no IP address is provided, the application load balancer is deployed with a private IP address from the portable private subnet that was provisioned automatically when you created the cluster.</li>
   </ul>
   </dd>
   </dl>

**Examples**:

  Example for enabling an application load balancer:

  ```
  bx cs alb-configure --albID my_alb_id --enable
  ```
  {: pre}

  Example for disabling an application load balancer:

  ```
  bx cs alb-configure --albID my_alb_id --disable
  ```
  {: pre}

  Example for enabling an application load balancer with a user-provided IP address:

  ```
  bx cs alb-configure --albID my_private_alb_id --enable --user-ip user_ip
  ```
  {: pre}

### bx cs alb-get --albID ALB_ID
{: #cs_alb_get}

View the details of an application load balancer.

<strong>Command options</strong>:

   <dl>
   <dt><code><em>--albID </em>ALB_ID</code></dt>
   <dd>The ID for an application load balancer. Run <code>bx cs albs --cluster <em>CLUSTER</em></code> to view the IDs for the application load balancers in a cluster. This value is required.</dd>
   </dl>

**Example**:

  ```
  bx cs alb-get --albID ALB_ID
  ```
  {: pre}

### bx cs alb-types
{: #cs_alb_types}

View the application load balancer types that are supported in the region.

<strong>Command options</strong>:

   None

**Example**:

  ```
  bx cs alb-types
  ```
  {: pre}


### bx cs albs --cluster CLUSTER
{: #cs_albs}

View the status of all application load balancers in a cluster. If no application load balancer IDs are returned, then the cluster does not have a portable subnet. You can [create](#cs_cluster_subnet_create) or [add](#cs_cluster_subnet_add) subnets to a cluster.

<strong>Command options</strong>:

   <dl>
   <dt><code><em>--cluster </em>CLUSTER</code></dt>
   <dd>The name or ID of the cluster where you list available application load balancers. This value is required.</dd>
   </dl>

**Example**:

  ```
  bx cs albs --cluster mycluster
  ```
  {: pre}


<br />


## API commands
{: #api_commands}

### bx cs api-key-info CLUSTER
{: #cs_api_key_info}

View the name and email address for the owner of the cluster's IAM API key.

<strong>Command options</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>The name or ID of the cluster. This value is required.</dd>
   </dl>

**Example**:

  ```
  bx cs api-key-info my_cluster
  ```
  {: pre}


### bx cs api-key-reset
{: #cs_api_key_reset}

Replace the API key. The API key is required to manage your clusters. To avoid service interruptions, do not replace the API key unless your existing key is compromised.

**Example**:

  ```
  bx cs api-key-reset
  ```
  {: pre}


### bx cs apiserver-config-get
{: #cs_apiserver_config_get}

Get information about an option for a cluster's Kubernetes API server configuration. This command must be combined with one of the following subcommands for the configuration option you want information on.

#### bx cs apiserver-config-get audit-webhook CLUSTER
{: #cs_apiserver_api_webhook_get}

View the URL for the remote logging service that you are sending API server audit logs to. The URL was specified when you created the webhook backend for the API server configuration.

<strong>Command options</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>The name or ID of the cluster. This value is required.</dd>
   </dl>

**Example**:

  ```
  bx cs apiserver-config-get audit-webhook my_cluster
  ```
  {: pre}

### bx cs apiserver-config-set
{: #cs_apiserver_config_set}

Set an option for a cluster's Kubernetes API server configuration. This command must be combined with one of the following subcommands for the configuration option you want to set.

#### bx cs apiserver-config-set audit-webhook CLUSTER [--remoteServer SERVER_URL_OR_IP] [--caCert CA_CERT_PATH] [--clientCert CLIENT_CERT_PATH] [--clientKey CLIENT_KEY_PATH]
{: #cs_apiserver_api_webhook_set}

Set the webhook backend for the API server configuration. The webhook backend forwards API server audit logs to a remote server. A webhook configuration is created based on the information you provide in this command's flags. If you do not provide any information in the flags, a default webhook configuration is used.

<strong>Command options</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>The name or ID of the cluster. This value is required.</dd>

   <dt><code>--remoteServer <em>SERVER_URL</em></code></dt>
   <dd>The URL or IP address for the remote logging service you want to send audit logs to. If you provide an insecure server URL, any certificates are ignored. This value is optional.</dd>

   <dt><code>--caCert <em>CA_CERT_PATH</em></code></dt>
   <dd>The filepath for the CA certificate that is used to verify the remote logging service. This value is optional.</dd>

   <dt><code>--clientCert <em>CLIENT_CERT_PATH</em></code></dt>
   <dd>The filepath for the client certificate that is used to authenticate against the remote logging service. This value is optional.</dd>

   <dt><code>--clientKey <em> CLIENT_KEY_PATH</em></code></dt>
   <dd>The filepath for the corresponding client key that is used to connect to the remote logging service. This value is optional.</dd>
   </dl>

**Example**:

  ```
  bx cs apiserver-config-set audit-webhook my_cluster --remoteServer https://audit.example.com/audit --caCert /mnt/etc/kubernetes/apiserver-audit/ca.pem --clientCert /mnt/etc/kubernetes/apiserver-audit/cert.pem --clientKey /mnt/etc/kubernetes/apiserver-audit/key.pem
  ```
  {: pre}


### bx cs apiserver-config-unset
{: #cs_apiserver_config_unset}

Disable an option for a cluster's Kubernetes API server configuration. This command must be combined with one of the following subcommands for the configuration option you want to unset.

#### bx cs apiserver-config-unset audit-webhook CLUSTER
{: #cs_apiserver_api_webhook_unset}

Disable the webhook backend configuration for the cluster's API server. Diabling the webhook backend stops forwarding API server audit logs to a remote server.

<strong>Command options</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>The name or ID of the cluster. This value is required.</dd>
   </dl>

**Example**:

  ```
  bx cs apiserver-config-unset audit-webhook my_cluster
  ```
  {: pre}

### bx cs apiserver-refresh CLUSTER
{: #cs_apiserver_refresh}

Restart the Kubernetes master in the cluster to apply changes to the API server configuration.

<strong>Command options</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>The name or ID of the cluster. This value is required.</dd>
   </dl>

**Example**:

  ```
  bx cs apiserver-refresh my_cluster
  ```
  {: pre}


<br />


## CLI plug-in usage commands
{: #cli_plug-in_commands}

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
   <dd>The {{site.data.keyword.containershort_notm}} API endpoint to use.  This value is optional. [View the available API endpoint values.](cs_regions.html#container_regions)</dd>
   </dl>

**Example**:


```
bx cs init --host https://uk-south.containers.bluemix.net
```
{: pre}


### bx cs messages
{: #cs_messages}

View current messages for the IBMid user.

**Example**:

```
bx cs messages
```
{: pre}


<br />


## Cluster commands: Management
{: #cluster_mgmt_commands}


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


### bx cs cluster-create [--file FILE_LOCATION] [--hardware HARDWARE] --location LOCATION --machine-type MACHINE_TYPE --name NAME [--kube-version MAJOR.MINOR.PATCH] [--no-subnet] [--private-vlan PRIVATE_VLAN] [--public-vlan PUBLIC_VLAN] [--workers WORKER] [--disable-disk-encrypt]
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
no-subnet: <em>&lt;no-subnet&gt;</em>
machine-type: <em>&lt;machine_type&gt;</em>
private-vlan: <em>&lt;private_vlan&gt;</em>
public-vlan: <em>&lt;public_vlan&gt;</em>
hardware: <em>&lt;shared_or_dedicated&gt;</em>
workerNum: <em>&lt;number_workers&gt;</em>
kube-version: <em>&lt;kube-version&gt;</em>

</code></pre>


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
     <td><code><em>no-subnet</em></code></td>
     <td>By default, both a public and a private portable subnets are created on the VLAN associated with the cluster. Replace <code><em>&lt;no-subnet&gt;</em></code> with <code><em>true</em></code> to avoid creating subnets with the cluster. You can [create](#cs_cluster_subnet_create) or [add](#cs_cluster_subnet_add) subnets to a cluster later.</td>
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
     <tr>
      <td><code><em>kube-version</em></code></td>
      <td>The Kubernetes version for the cluster master node. This value is optional. Unless specified, the cluster is created with the default of supported Kubernetes versions. To see available versions, run <code>bx cs kube-versions</code>.</td></tr>
      <tr>
      <td><code>diskEncryption: <em>false</em></code></td>
      <td>Worker nodes feature disk encryption by default; [learn more](cs_secure.html#worker). To disable encryption, include this option and set the value to <code>false</code>.</td></tr>
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
<dd>The machine type that you choose impacts the amount of memory and disk space that is available to the containers that are deployed to your worker node. To list available machine types, see [bx cs machine-types <em>LOCATION</em>](#cs_machine_types).  This value is required for standard clusters and is not available for lite clusters.</dd>

<dt><code>--name <em>NAME</em></code></dt>
<dd>The name for the cluster.  This value is required.</dd>

<dt><code>--kube-version <em>MAJOR.MINOR.PATCH</em></code></dt>
<dd>The Kubernetes version for the cluster master node. This value is optional. Unless specified, the cluster is created with the default of supported Kubernetes versions. To see available versions, run <code>bx cs kube-versions</code>.</dd>

<dt><code>--no-subnet</code></dt>
<dd>By default, both a public and a private portable subnets are created on the VLAN associated with the cluster. Include the <code>--no-subnet</code> flag to avoid creating subnets with the cluster. You can [create](#cs_cluster_subnet_create) or [add](#cs_cluster_subnet_add) subnets to a cluster later.</dd>

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

<dt><code>--disable-disk-encrypt</code></dt>
<dd>Worker nodes feature disk encryption by default; [learn more](cs_secure.html#worker). To disable encryption, include this option.</dd>
</dl>

**Examples**:

  

  Example for a standard cluster:
  {: #example_cluster_create}

  ```
  bx cs cluster-create --location dal10 --public-vlan my_public_vlan_id --private-vlan my_private_vlan_id --machine-type u2c.2x4 --name my_cluster --hardware shared --workers 2
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


### bx cs cluster-update [-f] CLUSTER [--kube-version MAJOR.MINOR.PATCH] [--force-update]
{: #cs_cluster_update}

Update the Kubernetes master to the default API version. During the update, you cannot access or change the cluster. Worker nodes, apps, and resources that have been deployed by the user are not modified and will continue to run.

You might need to change your YAML files for future deployments. Review this [release note](cs_versions.html) for details.

<strong>Command options</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>The name or ID of the cluster. This value is required.</dd>

   <dt><code>--kube-version <em>MAJOR.MINOR.PATCH</em></code></dt>
   <dd>The Kubernetes version of the cluster. If this flag is not specified, the Kubernetes master is update to the default API version. To see available versions, run [bx cs kube-versions](#cs_kube_versions). This value is optional.</dd>

   <dt><code>-f</code></dt>
   <dd>Use this option to force the update of the master with no user prompts. This value is optional.</dd>

   <dt><code>--force-update</code></dt>
   <dd>Attempt the update even if the change is greater than two minor versions. This value is optional.</dd>
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


### bx cs kube-versions
{: #cs_kube_versions}

View a list of Kubernetes versions supported in {{site.data.keyword.containershort_notm}}. Update your [cluster master](#cs_cluster_update) and [worker nodes](#cs_worker_update) to the default version for the latest, stable capabilities.

**Command options**:

  None

**Example**:

  ```
  bx cs kube-versions
  ```
  {: pre}


<br />


## Cluster commands: Services and integrations
{: #cluster_services_commands}

### bx cs cluster-service-bind CLUSTER KUBERNETES_NAMESPACE SERVICE_INSTANCE_GUID
{: #cs_cluster_service_bind}

Add an {{site.data.keyword.Bluemix_notm}} service to a cluster.

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


### bx cs webhook-create --cluster CLUSTER --level LEVEL --type slack --URL URL
{: #cs_webhook_create}

Register a webhook.

<strong>Command options</strong>:

   <dl>
   <dt><code>--cluster <em>CLUSTER</em></code></dt>
   <dd>The name or ID of the cluster. This value is required.</dd>

   <dt><code>--level <em>LEVEL</em></code></dt>
   <dd>The notification level, such as <code>Normal</code> or <code>Warning</code>. <code>Warning</code> is the default value. This value is optional.</dd>

   <dt><code>--type <em>slack</em></code></dt>
   <dd>The webhook type. Currently slack is supported. This value is required.</dd>

   <dt><code>--URL <em>URL</em></code></dt>
   <dd>The URL for the webhook. This value is required.</dd>
   </dl>

**Example**:

  ```
  bx cs webhook-create --cluster my_cluster --level Normal --type slack --URL http://github.com/<mywebhook>
  ```
  {: pre}


<br />


## Cluster commands: Subnets
{: #cluster_subnets_commands}

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


<br />


## Infrastructure commands
{: #infrastructure_commands}

### bx cs credentials-set --infrastructure-api-key API_KEY --infrastructure-username USERNAME
{: #cs_credentials_set}

Set IBM Cloud infrastructure (SoftLayer) account credentials for your {{site.data.keyword.Bluemix_notm}} account. These credentials allow you to access the IBM Cloud infrastructure (SoftLayer) portfolio through your {{site.data.keyword.Bluemix_notm}} account.

**Note:** Do not set multiple credentials for one {{site.data.keyword.Bluemix_notm}} account. Every {{site.data.keyword.Bluemix_notm}} account is linked to one IBM Cloud infrastructure (SoftLayer) portfolio only.

<strong>Command options</strong>:

   <dl>
   <dt><code>--infrastructure-username <em>USERNAME</em></code></dt>
   <dd>IBM Cloud infrastructure (SoftLayer) account username. This value is required.</dd>


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
  </ol>
  </p></dd>
  </dl>

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


### bx cs machine-types LOCATION
{: #cs_machine_types}

View a list of available machine types for your worker nodes. Each machine type includes the amount of virtual CPU, memory, and disk space for each worker node in the cluster.
- By default, the host's Docker data is encrypted in the machine types. The `/var/lib/docker` directory, where all container data is stored, is encrypted with LUKS encryption. If the `disable-disk-encrypt` option is included during cluster creation, then the host's Docker data is not encrypted. [Learn more about the encryption.](cs_secure.html#encrypted_disks)
- Machine types with `u2c` or `b2c` in the name use local disk instead of storage area networing (SAN) for reliability. Reliability benefits include higher throughput when serializing bytes to the local disk and reduced file system degradation due to network failures. These machine types contain 25GB local disk storage for the OS file system and 100GB local disk storage for `/var/lib/docker`, the directory that all the container data is written to.
- Machine types with `u1c` or `b1c` in the name are deprecated, such as `u1c.2x4`. To start using `u2c` and `b2c` machine types, use the `bx cs worker-add` command to add  worker nodes with the updated machine type. Then, remove the worker nodes that are using the deprecated machine types by using the `bx cs worker-rm` command.
</p>


<strong>Command options</strong>:

   <dl>
   <dt><code><em>LOCATION</em></code></dt>
   <dd>Enter the location where you want to list available machine types. This value is required. Review [available locations](cs_regions.html#locations).</dd></dl>

**Example**:

  ```
  bx cs machine-types dal10
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


<br />


## Logging commands
{: #logging_commands}

### bx cs logging-config-create CLUSTER --logsource LOG_SOURCE [--namespace KUBERNETES_NAMESPACE] [--hostname LOG_SERVER_HOSTNAME_OR_IP] [--port LOG_SERVER_PORT] [--space CLUSTER_SPACE] [--org CLUSTER_ORG] --type LOG_TYPE [--json]
{: #cs_logging_create}

Create a logging configuration. You can use this command to forward logs for containers, applications, worker nodes, Kubernetes clusters, and Ingress application load balancers to {{site.data.keyword.loganalysisshort_notm}} or to an external syslog server.

<strong>Command options</strong>:

<dl>
<dt><code><em>CLUSTER</em></code></dt>
<dd>The name or ID of the cluster.</dd>
<dt><code>--logsource <em>LOG_SOURCE</em></code></dt>
<dd>The log source that you want to enable log forwarding for. Accepted values are <code>container</code>, <code>application</code>, <code>worker</code>, <code>kubernetes</code>, and <code>ingress</code>. This value is required.</dd>
<dt><code>--namespace <em>KUBERNETES_NAMESPACE</em></code></dt>
<dd>The Kubernetes namespace that you want to forward logs from. Log forwarding is not supported for the <code>ibm-system</code> and <code>kube-system</code> Kubernetes namespaces. This value is valid only for the container log source and is optional. If you do not specify a namespace, then all namespaces in the cluster use this configuration.</dd>
<dt><code>--hostname <em>LOG_SERVER_HOSTNAME</em></code></dt>
<dd>When the logging type is <code>syslog</code>, the hostname or IP address of the log collector server. This value is required for <code>syslog</code>. When the logging type is <code>ibm</code>, the {{site.data.keyword.loganalysislong_notm}} ingestion URL. You can find the list of available ingestion URLs [here](/docs/services/CloudLogAnalysis/log_ingestion.html#log_ingestion_urls). If you do not specify an ingestion URL, the endpoint for the region where your cluster was created is used.</dd>
<dt><code>--port <em>LOG_SERVER_PORT</em></code></dt>
<dd>The port of the log collector server. This value is optional. If you do not specify a port, then the standard port <code>514</code> is used for <code>syslog</code> and the standard port <code>9091</code> is used for <code>ibm</code>.</dd>
<dt><code>--space <em>CLUSTER_SPACE</em></code></dt>
<dd>The name of the Cloud Foundry space that you want to send logs to. This value is valid only for log type <code>ibm</code> and is optional. If you do not specify a space, logs are sent to the account level.</dd>
<dt><code>--org <em>CLUSTER_ORG</em></code></dt>
<dd>The name of the Cloud Foundry org that the space is in. This value is valid only for log type <code>ibm</code> and is required if you specified a space.</dd>
<dt><code>--type <em>LOG_TYPE</em></code></dt>
<dd>The log forwarding protocol that you want to use. Currently, <code>syslog</code> and <code>ibm</code> are supported. This value is required.</dd>
<dt><code>--json</code></dt>
<dd>Optionally prints the command output in JSON format.</dd>
</dl>

**Examples**:

Example for log type `ibm` that forwards from a `container` log source on default port 9091:

  ```
  bx cs logging-config-create my_cluster --logsource container --namespace my_namespace --hostname ingest.logging.ng.bluemix.net --type ibm
  ```
  {: pre}

Example for log type `syslog` that forwards from a `container` log source on default port 514:

  ```
  bx cs logging-config-create my_cluster --logsource container --namespace my_namespace  --hostname my_hostname-or-IP --type syslog
  ```
  {: pre}

Example for log type `syslog` that forwards logs from an `ingress` source on a port different than the default:

  ```
  bx cs logging-config-create my_cluster --logsource container --hostname my_hostname-or-IP --port 5514 --type syslog
  ```
  {: pre}

### bx cs logging-config-get CLUSTER [--logsource LOG_SOURCE] [--json]
{: #cs_logging_get}

View all log forwarding configurations for a cluster, or filter logging configurations based on log source.

<strong>Command options</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>The name or ID of the cluster. This value is required.</dd>
   <dt><code>--logsource <em>LOG_SOURCE</em></code></dt>
   <dd>The kind of log source for which you want to filter. Only logging configurations of this log source in the cluster are returned. Accepted values are <code>container</code>, <code>application</code>, <code>worker</code>, <code>kubernetes</code>, and <code>ingress</code>. This value is optional.</dd>
   <dt><code>--json</code></dt>
   <dd>Optionally prints the command output in JSON format.</dd>
   </dl>

**Example**:

  ```
  bx cs logging-config-get my_cluster --logsource worker
  ```
  {: pre}


### bx cs logging-config-refresh CLUSTER
{: #cs_logging_refresh}

Refresh the logging configuration for the cluster. This refreshes the logging token for any logging configuration that is forwarding to the space level in your cluster.

<strong>Command options</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>The name or ID of the cluster. This value is required.</dd>
   </dl>

**Example**:

  ```
  bx cs logging-config-refresh my_cluster
  ```
  {: pre}


### bx cs logging-config-rm CLUSTER --id LOG_CONFIG_ID
{: #cs_logging_rm}

Delete a log forwarding configuration. This stops log forwarding to a remote syslog server or to {{site.data.keyword.loganalysisshort_notm}}.

<strong>Command options</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>The name or ID of the cluster. This value is required.</dd>
   <dt><code>--id <em>LOG_CONFIG_ID</em></code></dt>
   <dd>The logging configuration ID that you want to remove from the log source. This value is required.</dd>
   </dl>

**Example**:

  ```
  bx cs logging-config-rm my_cluster --id f4bc77c0-ee7d-422d-aabf-a4e6b977264e
  ```
  {: pre}


### bx cs logging-config-update CLUSTER --id LOG_CONFIG_ID [--hostname LOG_SERVER_HOSTNAME_OR_IP] [--port LOG_SERVER_PORT] [--space CLUSTER_SPACE] [--org CLUSTER_ORG] --type LOG_TYPE [--json]
{: #cs_logging_update}

Update the details of a log forwarding configuration.

<strong>Command options</strong>:

   <dl>
   <dt><code><em>CLUSTER</em></code></dt>
   <dd>The name or ID of the cluster. This value is required.</dd>
   <dt><code>--id <em>LOG_CONFIG_ID</em></code></dt>
   <dd>The logging configuration ID that you want to update. This value is required.</dd>
   <dt><code>--hostname <em>LOG_SERVER_HOSTNAME</em></code></dt>
   <dd>When the logging type is <code>syslog</code>, the hostname or IP address of the log collector server. This value is required for <code>syslog</code>. When the logging type is <code>ibm</code>, the {{site.data.keyword.loganalysislong_notm}} ingestion URL. You can find the list of available ingestion URLs [here](/docs/services/CloudLogAnalysis/log_ingestion.html#log_ingestion_urls). If you do not specify an ingestion URL, the endpoint for the region where your cluster was created is used.</dd>
   <dt><code>--port <em>LOG_SERVER_PORT</em></code></dt>
   <dd>The port of the log collector server. This value is optional when the logging type is <code>syslog</code>. If you do not specify a port, then the standard port <code>514</code> is used for <code>syslog</code> and <code>9091</code> is used for <code>ibm</code>.</dd>
   <dt><code>--space <em>CLUSTER_SPACE</em></code></dt>
   <dd>The name of the space that you want to send logs to. This value is valid only for log type <code>ibm</code> and is optional. If you do not specify a space, logs are sent to the account level.</dd>
   <dt><code>--org <em>CLUSTER_ORG</em></code></dt>
   <dd>The name of the org that the space is in. This value is valid only for log type <code>ibm</code> and is required if you specified a space.</dd>
   <dt><code>--type <em>LOG_TYPE</em></code></dt>
   <dd>The log forwarding protocol that you want to use. Currently, <code>syslog</code> and <code>ibm</code> are supported. This value is required.</dd>
   <dt><code>--json</code></dt>
   <dd>Optionally prints the command output in JSON format.</dd>
   </dl>

**Example for log type `ibm`**:

  ```
  bx cs logging-config-update my_cluster --id f4bc77c0-ee7d-422d-aabf-a4e6b977264e --type ibm
  ```
  {: pre}

**Example for log type `syslog`**:

  ```
  bx cs logging-config-update my_cluster --id f4bc77c0-ee7d-422d-aabf-a4e6b977264e --hostname localhost --port 5514 --type syslog
  ```
  {: pre}


<br />


## Region commands
{: #region_commands}

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


### bx cs region
{: #cs_region}

Find the {{site.data.keyword.containershort_notm}} region that you are currently in. You create and manage clusters specific to the region. Use the `bx cs region-set` command to change regions.

**Example**:

```
bx cs region
```
{: pre}

**Output**:
```
Region: us-south
```
{: screen}

### bx cs region-set [REGION]
{: #cs_region-set}

Set the region for {{site.data.keyword.containershort_notm}}. You create and manage clusters specific to the region, and you might want clusters in multiple regions for high availability.

For example, you can log in to {{site.data.keyword.Bluemix_notm}} in the US South region and create a cluster. Next, you can use `bx cs region-set eu-central` to target the EU Central region and create another cluster. Finally, you can use `bx cs region-set us-south` to return to US South to manage your cluster in that region.

**Command options**:

<dl>
<dt><code><em>REGION</em></code></dt>
<dd>Enter the region that you want to target. This value is optional. If you do not provide the region, you can select it from the list in the output.

For a list of available regions, review [regions and locations](cs_regions.html) or use the `bx cs regions` [command](#cs_regions).</dd></dl>

**Example**:

```
bx cs region-set eu-central
```
{: pre}

```
bx cs region-set
```
{: pre}

**Output**:
```
Choose a region:
1. ap-north
2. ap-south
3. eu-central
4. uk-south
5. us-east
6. us-south
Enter a number> 3
OK
```
{: screen}

### bx cs regions
{: #cs_regions}

Lists the available regions. The `Region Name` is the {{site.data.keyword.containershort_notm}} name, and the `Region Alias` is the general {{site.data.keyword.Bluemix_notm}} name for the region.

**Example**:

```
bx cs regions
```
{: pre}

**Output**:
```
Region Name   Region Alias
ap-north      jp-tok
ap-south      au-syd
eu-central    eu-de
uk-south      eu-gb
us-east       us-east
us-south      us-south
```
{: screen}


<br />


## Worker node commands
{: worker_node_commands}

### bx cs worker-add --cluster CLUSTER [--file FILE_LOCATION] [--hardware HARDWARE] --machine-type MACHINE_TYPE --number NUMBER --private-vlan PRIVATE_VLAN --public-vlan PUBLIC_VLAN [--disable-disk-encrypt]
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
workerNum: <em>&lt;number_workers&gt;</em>
</code></pre>

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
<tr>
<td><code>diskEncryption: <em>false</em></code></td>
<td>Worker nodes feature disk encryption by default; [learn more](cs_secure.html#worker). To disable encryption, include this option and set the value to <code>false</code>.</td></tr>
</tbody></table></p></dd>

<dt><code>--hardware <em>HARDWARE</em></code></dt>
<dd>The level of hardware isolation for your worker node. Use dedicated if you want to have available physical resources dedicated to you only, or shared to allow physical resources to be shared with other IBM customers. The default is shared. This value is optional.</dd>

<dt><code>--machine-type <em>MACHINE_TYPE</em></code></dt>
<dd>The machine type that you choose impacts the amount of memory and disk space that is available to the containers that are deployed to your worker node. This value is required. To list available machine types, see [bx cs machine-types LOCATION](#cs_machine_types).</dd>

<dt><code>--number <em>NUMBER</em></code></dt>
<dd>An integer that represents the number of worker nodes to create in the cluster. The default value is 1. This value is optional.</dd>

<dt><code>--private-vlan <em>PRIVATE_VLAN</em></code></dt>
<dd>The private VLAN that was specified when the cluster was created. This value is required.

<p><strong>Note:</strong> The public and private VLANs that you specify must match. Private VLAN routers always begin with <code>bcr</code> (back-end router) and public VLAN routers always begin with <code>fcr</code> (front-end router). The number and letter combination after those prefixes must match to use those VLANs when creating a cluster. Do not use public and private VLANs that do not match to create a cluster.</p></dd>

<dt><code>--public-vlan <em>PUBLIC_VLAN</em></code></dt>
<dd>The public VLAN that was specified when the cluster was created. This value is optional.

<p><strong>Note:</strong> The public and private VLANs that you specify must match. Private VLAN routers always begin with <code>bcr</code> (back-end router) and public VLAN routers always begin with <code>fcr</code> (front-end router). The number and letter combination after those prefixes must match to use those VLANs when creating a cluster. Do not use public and private VLANs that do not match to create a cluster.</p></dd>

<dt><code>--disable-disk-encrypt</code></dt>
<dd>Worker nodes feature disk encryption by default; [learn more](cs_secure.html#worker). To disable encryption, include this option.</dd>
</dl>

**Examples**:

  ```
  bx cs worker-add --cluster my_cluster --number 3 --public-vlan my_public_vlan_id --private-vlan my_private_vlan_id --machine-type u2c.2x4 --hardware shared
  ```
  {: pre}

  Example for {{site.data.keyword.Bluemix_dedicated_notm}}:

  ```
  bx cs worker-add --cluster my_cluster --number 3 --machine-type u2c.2x4
  ```
  {: pre}


### bx cs worker-get [CLUSTER_NAME_OR_ID] WORKER_NODE_ID
{: #cs_worker_get}

View details of a worker node.

<strong>Command options</strong>:

   <dl>
   <dt><code><em>CLUSTER_NAME_OR_ID</em></code></dt>
   <dd>The name or ID of the worker node's cluster. This value is optional.</dd>
   <dt><code><em>WORKER_NODE_ID</em></code></dt>
   <dd>The ID for a worker node. Run <code>bx cs workers <em>CLUSTER</em></code> to view the IDs for the worker nodes in a cluster. This value is required.</dd>
   </dl>

**Example**:

  ```
  bx cs worker-get [CLUSTER_NAME_OR_ID] WORKER_NODE_ID
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

### bx cs worker-update [-f] CLUSTER WORKER [WORKER] [--kube-version MAJOR.MINOR.PATCH] [--force-update]
{: #cs_worker_update}

Update worker nodes to the latest Kubernetes version. Running `bx cs worker-update` can cause downtime for your apps and services. During the update, all pods are rescheduled onto other worker nodes and data is deleted if not stored outside the pod. To avoid downtime, [ensure that you have enough worker nodes to handle your workload while the selected worker nodes are updating](cs_cluster_update.html#worker_node).

You might need to change your YAML files for deployments before updating. Review this [release note](cs_versions.html) for details.

<strong>Command options</strong>:

   <dl>

   <dt><em>CLUSTER</em></dt>
   <dd>The name or ID of the cluster where you list available worker nodes. This value is required.</dd>

   <dt><code>--kube-version <em>MAJOR.MINOR.PATCH</em></code></dt>
   <dd>The Kubernetes version of the cluster. If this flag is not specified, the worker node is update to the default version. To see available versions, run [bx cs kube-versions](#cs_kube_versions). This value is optional.</dd>

   <dt><code>-f</code></dt>
   <dd>Use this option to force the update of the master with no user prompts. This value is optional.</dd>

   <dt><code>--force-update</code></dt>
   <dd>Attempt the update even if the change is greater than two minor versions. This value is optional.</dd>

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
