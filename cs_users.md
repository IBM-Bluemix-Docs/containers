---

copyright:
  years: 2014, 2018
lastupdated: "2018-01-31"

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

You can grant access to a cluster for other users in your organization to ensure that only authorized users can work with the cluster and deploy apps to the cluster.
{:shortdesc}


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
<dd>Every user who is assigned an {{site.data.keyword.containershort_notm}} access policy is automatically assigned a Kubernetes RBAC role. In Kubernetes, RBAC roles determine the actions that you can perform on Kubernetes resources inside the cluster. RBAC roles are set up for the default namespace only. The cluster administrator can add RBAC roles for other namespaces in the cluster. See [Using RBAC Authorization ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/admin/authorization/rbac/#api-overview) in the Kubernetes documentation for more information.</dd>
</dl>

<br />


## Access policies and permissions
{: #access_policies}

Review the access policies and permissions that you can grant to users in your {{site.data.keyword.Bluemix_notm}} account. The operator and editor roles have separate permissions. If you want a user to, for example, add worker nodes and bind services, then you must assign the user both the operator and editor roles. If you change a user's access policy, {{site.data.keyword.containershort_notm}} cleans up the RBAC policies associated with the change in your cluster for you. For more details on corresponding infrastructure access policies, see [Customizing infrastructure permissions for a user](#infra_access).

|{{site.data.keyword.containershort_notm}} access policy|Cluster management permissions|Kubernetes resource permissions|
|-------------|------------------------------|-------------------------------|
|Administrator|This role inherits permissions from the Editor, Operator, and Viewer roles for all clusters in this account. <br/><br/>When set for all current service instances:<ul><li>Create a free or standard cluster</li><li>Set credentials for an {{site.data.keyword.Bluemix_notm}} account to access the IBM Cloud infrastructure (SoftLayer) portfolio</li><li>Remove a cluster</li><li>Assign and change {{site.data.keyword.containershort_notm}} access policies for other existing users in this account.</li></ul><p>When set for a specific cluster ID:<ul><li>Remove a specific cluster</li></ul></p>Corresponding infrastructure access policy: Super user<br/><br/><b>Note</b>: To create resources such as machines, VLANs, and subnets, users need the **Super user** infrastructure role.|<ul><li>RBAC Role: cluster-admin</li><li>Read/write access to resources in every namespace</li><li>Create roles within a namespace</li><li>Access Kubernetes dashboard</li><li>Create an Ingress resource that makes apps publically available</li></ul>|
|Operator|<ul><li>Add additional worker nodes to a cluster</li><li>Remove worker nodes from a cluster</li><li>Reboot a worker node</li><li>Reload a worker node</li><li>Add a subnet to a cluster</li></ul><p>Corresponding infrastructure access policy: [Custom](#infra_access)</p>|<ul><li>RBAC Role: admin</li><li>Read/write access to resources inside the default namespace but not to the namespace itself</li><li>Create roles within a namespace</li></ul>|
|Editor <br/><br/><b>Tip</b>: Use this role for app developers.|<ul><li>Bind an {{site.data.keyword.Bluemix_notm}} service to a cluster.</li><li>Unbind an {{site.data.keyword.Bluemix_notm}} service to a cluster.</li><li>Create a webhook.</li></ul><p>Corresponding infrastructure access policy: [Custom](#infra_access)|<ul><li>RBAC Role: edit</li><li>Read/write access to resources inside the default namespace</li></ul></p>|
|Viewer|<ul><li>List a cluster</li><li>View details for a cluster</li></ul><p>Corresponding infrastructure access policy: View only</p>|<ul><li>RBAC Role: view</li><li>Read access to resources inside the default namespace</li><li>No read access to Kubernetes secrets</li></ul>|

|Cloud Foundry access policy|Account management permissions|
|-------------|------------------------------|
|Organization role: Manager|<ul><li>Add additional users to an {{site.data.keyword.Bluemix_notm}} account</li></ul>| |
|Space role: Developer|<ul><li>Create {{site.data.keyword.Bluemix_notm}} service instances</li><li>Bind {{site.data.keyword.Bluemix_notm}} service instances to clusters</li></ul>| 

<br />


## Adding users to an {{site.data.keyword.Bluemix_notm}} account
{: #add_users}

You can add additional users to an {{site.data.keyword.Bluemix_notm}} account to grant access to your clusters.

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
4. [Optional: Assign an infrastructure role](/docs/iam/mnginfra.html#managing-infrastructure-access).
5. [Optional: Assign a Cloud Foundry role](/docs/iam/mngcf.html#mngcf).
5. Click **Invite users**.

<br />


## Customizing infrastructure permissions for a user
{: #infra_access}

When you set infrastructure policies in Identity and Access Management, a user is given permissions associated with a role. To customize those permissions, you must log in to IBM Cloud infrastructure (SoftLayer) and adjust the permissions there.
{: #view_access}

For example, **Basic Users** can reboot a worker node, but they cannot reload a worker node. Without giving that person **Super User** permissions, you can adjust the IBM Cloud infrastructure (SoftLayer) permissions and add the permission to run a reload command.


1.  Log in to your IBM Cloud infrastructure (SoftLayer) account.
2.  Select a user profile to update.
3.  In the **Portal Permissions**, customize the user's access. For example, to add reload permission, in the **Devices** tab, select **Issue OS Reloads and Initiate Rescue Kernel**.
9.  Save your changes.



