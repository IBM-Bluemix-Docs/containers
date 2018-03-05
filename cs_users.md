---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-28"


---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}


# Assigning user access to clusters
{: #users}

You can grant access to a Kubernetes cluster to ensure that only authorized users can work with the cluster and deploy containers to the cluster in {{site.data.keyword.containerlong}}.
{:shortdesc}


## Planning communication processes
As a cluster administrator, consider how you might establish a communication process for the members of your organization to communicate access requests to you so that you can stay organized.
{:shortdesc}

Provide instructions to your cluster users about how to request access to a cluster or how to get help with any kinds of common tasks from a cluster administrator. Because Kubernetes does not facilitate this kind of communication, each team can have variations on their preferred process.

You might choose any of the following methods or establish your own method.
- Create a ticket system
- Create a form template
- Create a wiki page
- Require an email request
- Use the issue tracking method you already use to track your team's daily work


## Managing cluster access
{: #managing}

Every user that works with {{site.data.keyword.containershort_notm}} must be assigned a combination of service-specific user roles that determine which actions the user can perform.
{:shortdesc}

<dl>
<dt>{{site.data.keyword.containershort_notm}} access policies</dt>
<dd>In Identity and Access Management, {{site.data.keyword.containershort_notm}} access policies determine the cluster management actions that you can perform on a cluster, such as creating or removing clusters, and adding or removing extra worker nodes. These policies must be set in conjunction with infrastructure policies.</dd>
<dt>Infrastructure access policies</dt>
<dd>In Identity and Access Management, infrastructure access policies allow the actions that are requested from the {{site.data.keyword.containershort_notm}} user interface or the CLI to be completed in IBM Cloud infrastructure (SoftLayer). These policies must be set in conjunction with {{site.data.keyword.containershort_notm}} access policies. [Learn more about the available infrastructure roles](/docs/iam/infrastructureaccess.html#infrapermission).</dd>
<dt>Resource groups</dt>
<dd>A resource group is a way to organize {{site.data.keyword.Bluemix_notm}} services into groupings so that you can quickly assign users access to more than one resource at a time. [Learn how to manage users by using resource groups](/docs/account/resourcegroups.html#rgs).</dd>
<dt>Cloud Foundry roles</dt>
<dd>In Identity and Access Management, every user must be assigned a Cloud Foundry user role. This role determines the actions that the user can perform on the {{site.data.keyword.Bluemix_notm}} account, such as inviting other users, or viewing the quota usage. [Learn more about the available Cloud Foundry roles](/docs/iam/cfaccess.html#cfaccess).</dd>
<dt>Kubernetes RBAC roles</dt>
<dd>Every user who is assigned an {{site.data.keyword.containershort_notm}} access policy is automatically assigned a Kubernetes RBAC role.  In Kubernetes, RBAC roles determine the actions that you can perform on Kubernetes resources inside the cluster. RBAC roles are set up for the default namespace only. The cluster administrator can add RBAC roles for other namespaces in the cluster. See the following table in the [Access policies and permissions](#access_policies) section to see which RBAC role corresponds to which {{site.data.keyword.containershort_notm}} access policy. For more information about RBAC roles in general, see [Using RBAC Authorization ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/admin/authorization/rbac/#api-overview) in the Kubernetes documentation.</dd>
</dl>

<br />


## Access policies and permissions
{: #access_policies}

Review the access policies and permissions that you can grant to users in your {{site.data.keyword.Bluemix_notm}} account.
{:shortdesc}

The operator and editor roles have separate permissions. If you want a user to, for example, add worker nodes and bind services, then you must assign the user both the operator and editor roles. For more details on corresponding infrastructure access policies, see [Customizing infrastructure permissions for a user](#infra_access).<br/><br/>If you change a user's access policy, {{site.data.keyword.containershort_notm}} cleans up the RBAC policies associated with the change in your cluster for you. </br></br>**Note:** When you downgrade permissions, for example you want to assign viewer access to a former cluster admin, you must wait a few minutes for the downgrade to complete.

|{{site.data.keyword.containershort_notm}} access policy|Cluster management permissions|Kubernetes resource permissions|
|-------------|------------------------------|-------------------------------|
|Administrator|This role inherits permissions from the Editor, Operator, and Viewer roles for all clusters in this account. <br/><br/>When set for all current service instances:<ul><li>Create a free or standard cluster</li><li>Set credentials for an {{site.data.keyword.Bluemix_notm}} account to access the IBM Cloud infrastructure (SoftLayer) portfolio</li><li>Remove a cluster</li><li>Assign and change {{site.data.keyword.containershort_notm}} access policies for other existing users in this account.</li></ul><p>When set for a specific cluster ID:<ul><li>Remove a specific cluster</li></ul></p>Corresponding infrastructure access policy: Super user<br/><br/><strong>Note</strong>: To create resources such as machines, VLANs, and subnets, users need the **Super user** infrastructure role.|<ul><li>RBAC Role: cluster-admin</li><li>Read/write access to resources in every namespace</li><li>Create roles within a namespace</li><li>Access Kubernetes dashboard</li><li>Create an Ingress resource that makes apps publically available</li></ul>|
|Operator|<ul><li>Add additional worker nodes to a cluster</li><li>Remove worker nodes from a cluster</li><li>Reboot a worker node</li><li>Reload a worker node</li><li>Add a subnet to a cluster</li></ul><p>Corresponding infrastructure access policy: [Custom](#infra_access)</p>|<ul><li>RBAC Role: admin</li><li>Read/write access to resources inside the default namespace but not to the namespace itself</li><li>Create roles within a namespace</li></ul>|
|Editor <br/><br/><strong>Tip</strong>: Use this role for app developers.|<ul><li>Bind an {{site.data.keyword.Bluemix_notm}} service to a cluster.</li><li>Unbind an {{site.data.keyword.Bluemix_notm}} service to a cluster.</li><li>Create a webhook.</li></ul><p>Corresponding infrastructure access policy: [Custom](#infra_access)|<ul><li>RBAC Role: edit</li><li>Read/write access to resources inside the default namespace</li></ul></p>|
|Viewer|<ul><li>List a cluster</li><li>View details for a cluster</li></ul><p>Corresponding infrastructure access policy: View only</p>|<ul><li>RBAC Role: view</li><li>Read access to resources inside the default namespace</li><li>No read access to Kubernetes secrets</li></ul>|

|Cloud Foundry access policy|Account management permissions|
|-------------|------------------------------|
|Organization role: Manager|<ul><li>Add additional users to an {{site.data.keyword.Bluemix_notm}} account</li></ul>| |
|Space role: Developer|<ul><li>Create {{site.data.keyword.Bluemix_notm}} service instances</li><li>Bind {{site.data.keyword.Bluemix_notm}} service instances to clusters</li></ul>| 

<br />



## Understanding the IAM API key and the `bx cs credentials-set` command
{: #api_key}

To succesfully provision and work with clusters in your account, you must ensure that your account is correctly set up to access the IBM Cloud infrastructure (SoftLayer) portfolio. Depending on your account setup, you either use the IAM API key or infrastructure credentials that you manually set by using the `bx cs credentials-set` command. 

<dl>
  <dt>IAM API key</dt>
  <dd>The Identity and Access Management (IAM) API key is automatically set for a region when the first action that requires the {{site.data.keyword.containershort_notm}} admin access policy is performed. For example, one of your admin users creates the first cluster in the <code>us-south</code> region. By doing that, the IAM API key for this user is stored in the account for this region. The API key is used to order IBM Cloud infrastructure (SoftLayer), such as new worker nodes or VLANs. </br></br>
When a different user performs an action in this region that requires interaction with the IBM Cloud infrastructure (SoftLayer) portfolio, such as creating a new cluster or reloading a worker node, the stored API key is used to determine if sufficient permissions exist to perform that action. To make sure that infrastructure-related actions in your cluster can be successfully performed, assign your {{site.data.keyword.containershort_notm}} admin users the <strong>Super user</strong> infrastructure access policy. </br></br>You can find the current API key owner by running [<code>bx cs api-key-info</code>](cs_cli_reference.html#cs_api_key_info). If you find that you need to update the API key that is stored for a region, you can do so by running the [<code>bx cs api-key-reset</code>](cs_cli_reference.html#cs_api_key_reset) command. This command requires the {{site.data.keyword.containershort_notm}} admin access policy and stores the API key of the user that executes this command in the account. </br></br> <strong>Note:</strong> The API key that is stored for the region might not be used if IBM Cloud infrastructure (SoftLayer) credentials were manually set by using the <code>bx cs credentials-set</code> command. </dd>
<dt>IBM Cloud infrastructure (SoftLayer) credentials via <code>bx cs credentials-set</code></dt>
<dd>If you have an {{site.data.keyword.Bluemix_notm}} Pay-As-You-Go account, you have access to the IBM Cloud infrastructure (SoftLayer) portfolio by default. However, you might want to use a different IBM Cloud infrastructure (SoftLayer) account that you already have to order infrastructure. You can link this infrastructure account to your {{site.data.keyword.Bluemix_notm}} account by using the [<code>bx cs credentials-set</code>](cs_cli_reference.html#cs_credentials_set) command. </br></br>If IBM Cloud infrastructure (SoftLayer) credentials are manually set, these credentials are used to order infrastructure, even if an IAM API key already exists for the account. If the user whose credentials are stored does not have the required permissions to order infrastructure, then infrastructure-related actions, such as creating a cluster or reloading a worker node can fail. </br></br> To remove IBM Cloud infrastructure (SoftLayer) credentials that were manually set, you can use the [<code>bx cs credentials-unset</code>](cs_cli_reference.html#cs_credentials_unset) command. After the credentials are removed, the IAM API key is used to order infrastructure. </dd>
</dl>


## Adding users to an {{site.data.keyword.Bluemix_notm}} account
{: #add_users}

You can add users to an {{site.data.keyword.Bluemix_notm}} account to grant access to your clusters.
{:shortdesc}

Before you begin, verify that you have been assigned the Manager Cloud Foundry role for an {{site.data.keyword.Bluemix_notm}} account.

1.  [Add the user to the account](../iam/iamuserinv.html#iamuserinv).
2.  In the **Access** section, expand **Services**.
3.  Assign an {{site.data.keyword.containershort_notm}} access role. From the **Assign access to** drop-down list, decide whether you want to grant access only to your {{site.data.keyword.containershort_notm}} account (**Resource**), or to a collection of various resources within your account (**Resource group**).
  -  For **Resource**:
      1. From the **Services** drop-down list, select **{{site.data.keyword.containershort_notm}}**.
      2. From the **Region** drop-down list, select the region to invite the user to.
      3. From the **Service instance** drop-down list, select the cluster to invite the user to. To find the ID of a specific cluster, run `bx cs clusters`.
      4. In the **Select roles** section, choose a role. To find a list of supported actions per role, see [Access policies and permissions](#access_policies).
  - For **Resource group**:
      1. From the **Resource group** drop-down list, select the resource group that includes permissions for your account's {{site.data.keyword.containershort_notm}} resource.
      2. From the **Assign access to a resource group** drop-down list, select a role. To find a list of supported actions per role, see [Access policies and permissions](#access_policies).
4. [Optional: Assign a Cloud Foundry role](/docs/iam/mngcf.html#mngcf).
5. [Optional: Assign an infrastructure role](/docs/iam/infrastructureaccess.html#infrapermission).
6. Click **Invite users**.

<br />


## Customizing infrastructure permissions for a user
{: #infra_access}

When you set infrastructure policies in Identity and Access Management, a user is given permissions that are associated with a role. To customize those permissions, you must log in to IBM Cloud infrastructure (SoftLayer) and adjust the permissions there.
{: #view_access}

For example, **Basic Users** can reboot a worker node, but they cannot reload a worker node. Without giving that person **Super User** permissions, you can adjust the IBM Cloud infrastructure (SoftLayer) permissions and add the permission to run a reload command.

1.  Log in to your [{{site.data.keyword.Bluemix_notm}} account](https://console.bluemix.net/), then from the menu select **Infrastructure**.

2.  Go to **Account** > **Users** > **User List**.

3.  To modify permissions, select a user profile's name or the **Device Access** column.

4.  In the **Portal Permissions** tab, customize the user's access. The permissions that users need depend on what infrastructure resources they need to use:

    * Use the **Quick Permissions** dropdown list to assign the **Super User** role, which gives the user all permissions.
    * Use the **Quick Permissions** dropdown list to assign the **Basic User** role, which gives the user some, but not all, needed permissions.
    * If you don't want to grant all permissions with the **Super User** role or need to add permissions beyond the **Basic User** role, review the following table that describes permissions needed to perform common tasks in {{site.data.keyword.containershort_notm}}.

    <table summary="Infrastructure permissions for common {{site.data.keyword.containershort_notm}} scenarios.">
     <caption>Commonly required infrastructure permissions for {{site.data.keyword.containershort_notm}}</caption>
     <thead>
     <th>Common tasks in {{site.data.keyword.containershort_notm}}</th>
     <th>Required infrastructure permissions by tab</th>
     </thead>
     <tbody>
     <tr>
     <td><strong>Minimum permissions</strong>: <ul><li>Create a cluster.</li></ul></td>
     <td><strong>Devices</strong>:<ul><li>View Virtual Server Details</li><li>Reboot server and view IPMI system information</li><li>Issue OS Reloads and Initiate Rescue Kernel</li></ul><strong>Account</strong>: <ul><li>Add/Upgrade Cloud Instances</li><li>Add Server</li></ul></td>
     </tr>
     <tr>
     <td><strong>Cluster Administration</strong>: <ul><li>Create, update, and delete clusters.</li><li>Add, reload, and reboot worker nodes.</li><li>View VLANs.</li><li>Create subnets.</li><li>Deploy pods and load balancer services.</li></ul></td>
     <td><strong>Support</strong>:<ul><li>View Tickets</li><li>Add Tickets</li><li>Edit Tickets</li></ul>
     <strong>Devices</strong>:<ul><li>View Virtual Server Details</li><li>Reboot server and view IPMI system information</li><li>Upgrade Server</li><li>Issue OS Reloads and Initiate Rescue Kernel</li></ul>
     <strong>Services</strong>:<ul><li>Manage SSH Keys</li></ul>
     <strong>Account</strong>:<ul><li>View Account Summary</li><li>Add/Upgrade Cloud Instances</li><li>Cancel Server</li><li>Add Server</li></ul></td>
     </tr>
     <tr>
     <td><strong>Storage</strong>: <ul><li>Create persistent volume claims to provision persistent volumes.</li><li>Create and manage storage infrastructure resources.</li></ul></td>
     <td><strong>Services</strong>:<ul><li>Manage Storage</li></ul><strong>Account</strong>:<ul><li>Add Storage</li></ul></td>
     </tr>
     <tr>
     <td><strong>Private Networking</strong>: <ul><li>Manage private VLANs for in-cluster networking.</li><li>Set up VPN connectivity to private networks.</li></ul></td>
     <td><strong>Network</strong>:<ul><li>Manage Network Subnet Routes</li><li>Manage Network VLAN Spanning</li><li>Manage IPSEC Network Tunnels</li><li>Manage Network Gateways</li><li>VPN Administration</li></ul></td>
     </tr>
     <tr>
     <td><strong>Public Networking</strong>:<ul><li>Set up public load balancer or Ingress networking to expose apps.</li></ul></td>
     <td><strong>Devices</strong>:<ul><li>Manage Load Balancers</li><li>Edit Hostname/Domain</li><li>Manage Port Control</li></ul>
     <strong>Network</strong>:<ul><li>Add Compute with Public Network Port</li><li>Manage Network Subnet Routes</li><li>Manage Network VLAN Spanning</li><li>Add IP Addresses</li></ul>
     <strong>Services</strong>:<ul><li>Manage DNS, Reverse DNS, and WHOIS</li><li>View Certificates (SSL)</li><li>Manage Certificates (SSL)</li></ul></td>
     </tr>
     </tbody></table>

5.  To save your changes, click **Edit Portal Permissions**.

6.  In the **Device Access** tab, select the devices to grant access to.

    * In the **Device Type** dropdown, you can grant access to **All Virtual Servers**.
    * To allow users access to new devices that are created, check **Automatically grant access when new devices are added**.
    * To save your changes, click **Update Device Access**.

7.  Return to the user profile list and verify that **Device Access** is granted.


