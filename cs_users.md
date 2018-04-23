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




# Assigning cluster access
{: #users}

As a cluster administrator, you can define access policies for your Kubernetes cluster to create different levels of access for different users. For example, you can authorize certain users to work with cluster resources while others can only deploy containers.
{: shortdesc}

## Planning for access requests
{: #planning_access}

As a cluster administrator, it might be difficult to keep track of access requests. Establishing a pattern of communication for access requests is essential to maintaining the security of your cluster.
{: shortdesc}

To ensure that the right people have the right access, be very clear with those that have access to the cluster on your policies for requesting access, or getting help with common tasks.

You might already have a method that works for your team, and that's great! If you're looking for a place to start, consider trying one of the following methods.

*  Create a ticket system
*  Create a form template
*  Create a wiki page
*  Require an email request
*  Use the issue tracking system that you already use to track your team's daily work

Feeling overwhelmed? Try out this tutorial about the [best practices for organizing users, teams, and applications](/docs/tutorials/users-teams-applications.html).
{: tip}

## Access policies and permissions
{: #access_policies}

The scope of an access policy is based on a users defined role or roles that determine the actions that they are allowed to perform. You can set policies that are specific to your cluster, infrastructure, instances of the service, or Cloud Foundry roles.
{: shortdesc}

You must define access policies for every user that works with {{site.data.keyword.containershort_notm}}. Some policies are pre-defined, but others can be customized. Check out the following image and definitions to see which roles align with common user tasks and identify places where you might want to customize a policy.

![{{site.data.keyword.containershort_notm}} access roles](/images/user-policies.png)

Figure. {{site.data.keyword.containershort_notm}} access roles

<dl>
  <dt>Identity and Access Management (IAM) policies</dt>
    <dd><p><strong>Platform</strong>: You can determine the actions that individuals can perform on an {{site.data.keyword.containershort_notm}} cluster. You can set these policies by region. Example actions are creating or removing clusters, or adding extra worker nodes. These policies must be set in conjunction with infrastructure policies.</p>
    <p><strong>Infrastructure</strong>: You can determine the access levels for your infrastructure such as the cluster node machines, networking, or storage resources. The same policy is enforced whether the user makes the request from the {{site.data.keyword.containershort_notm}} GUI or through the CLI; even when the actions are completed in IBM Cloud infrastructure (SoftLayer). You must set this type of policy in conjunction with {{site.data.keyword.containershort_notm}} platform access policies. To learn about the available roles, check out [infrastructure permissions](/docs/iam/infrastructureaccess.html#infrapermission).</p></dd>
  <dt>Kubernetes Resource Based Access Control (RBAC) roles</dt>
    <dd>Every user who is assigned a platform access policy is automatically assigned a Kubernetes role. In Kubernetes, [Role Based Access Control (RBAC) ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/admin/authorization/rbac/#api-overview) determines the actions that a user can perform on the resources inside of a cluster. <strong>Note</strong>: RBAC roles are automatically configured for the <code>default</code> namespace, but as the cluster administrator, you can assign roles for other namespaces.</dd>
  <dt>Cloud Foundry</dt>
    <dd>At this time, not all services can be managed with Cloud IAM. If you are using one of these services, you can continue to use the [Cloud Foundry user roles](/docs/iam/cfaccess.html#cfaccess) to control access to your services.</dd>
</dl>


Downgrading permissions? It can take a few minutes for the action to complete.
{: tip}

### Platform roles
{: #platform_roles}

{{site.data.keyword.containershort_notm}} is configured to use {{site.data.keyword.Bluemix_notm}} platform roles. The role permissions build on each other, which means that the `Editor` role has the same permissions as the `Viewer` role, plus the permissions that are granted to an editor. The following table explains the types of actions that each role can perform. **Note**: The corresponding RBAC roles are automatically assigned when you assign a platform role. You can always change them later!

<table>
  <tr>
    <th>Platform roles</th>
    <th>Example actions</th>
    <th>Corresponding RBAC role</th>
  </tr>
  <tr>
      <td>Viewer</td>
      <td>View the details for a cluster or other service instances.</td>
      <td>View</td>
  </tr>
  <tr>
    <td>Editor</td>
    <td>Can bind or unbind an IBM Cloud service to a cluster, or create a webhook.</td>
    <td>Edit</td>
  </tr>
  <tr>
    <td>Operator</td>
    <td>Can create, remove, reboot, or reload a worker node. Can add a subnet to a cluster.</td>
    <td>Admin</td>
  </tr>
  <tr>
    <td>Administrator</td>
    <td>Can create and remove clusters. Can edit access policies for others at the account level for the service and infrastructure.</td>
    <td>Cluster-admin</td>
  </tr>
</table>

For more information about assigning user roles in the UI, see [Managing IAM access](/docs/iam/mngiam.html#iammanidaccser).

### Infrastructure roles
{: #infrastructure_roles}

Infrastructure roles enable users to perform tasks on resources at the infrastructure level. The following table explains the types of actions that each role can perform. Infrastructure roles are customizable; be sure to only give users the access that they need to do their job.

<table>
  <tr>
    <th>Infrastructure role</th>
    <th>Example actions</th>
  </tr>
  <tr>
    <td><i>View only</i></td>
    <td>Can view the infrastructure details. Can see an account summary, including invoices and payments.</td>
  </tr>
  <tr>
    <td><i>Basic user</i></td>
    <td>Can edit service configurations, including IP addresses, add or edit DNS records, and add new users with access to infrastructure.</td>
  </tr>
  <tr>
    <td><i>Super user</i></td>
    <td>Can perform all actions related to infrastructure.</td>
  </tr>
</table>

To start assigning roles, follow the steps in [Customizing infrastructure permissions for a user](#infra_access).

### RBAC roles
{: #rbac_roles}

Resource-based access control (RBAC) is a way of securing your resources that are inside of your cluster and deciding who can perform which Kubernetes actions. In the following table you can see the types of RBAC roles and the types of actions that users can perform with that role. The permissions build on each other which means that an `Admin` also has all of the policies that come with the `View` and `Edit` roles. Be sure to give users only the access that they need.

<table>
  <tr>
    <th>RBAC role</th>
    <th>Example actions</th>
  </tr>
  <tr>
    <td>View</td>
    <td>Can view resources inside of the default namespace.</td>
  </tr>
  <tr>
    <td>Edit</td>
    <td>Can read and write to resources inside the default namespace.</td>
  </tr>
  <tr>
    <td>Admin</td>
    <td>Can read and write to resources within the default namespace but not to the namespace itself. Can create roles within a namespace.</td>
  </tr>
  <tr>
    <td>Cluster admin</td>
    <td>Can read and write to resources in every namespace. Can create roles within a namespace. Can access the Kubernetes dashboard. Can create an Ingress resource that makes apps publicly available.</td>
  </tr>
</table>

<br />


## Adding users to an {{site.data.keyword.Bluemix_notm}} account
{: #add_users}

You can add users to an {{site.data.keyword.Bluemix_notm}} account to grant access to your clusters.
{:shortdesc}

Before you begin, verify that you have been assigned the `Manager` Cloud Foundry role for an {{site.data.keyword.Bluemix_notm}} account.

1.  [Add the user to the account](../iam/iamuserinv.html#iamuserinv).
2.  In the **Access** section, expand **Services**.
3.  Assign a platform role to a user to set access for {{site.data.keyword.containershort_notm}}.
      1. From the **Services** drop-down list, select **{{site.data.keyword.containershort_notm}}**.
      2. From the **Region** drop-down list, select the region to invite the user to.
      3. From the **Service instance** drop-down list, select the cluster to invite the user to. To find the ID of a specific cluster, run `bx cs clusters`.
      4. In the **Select roles** section, choose a role. To find a list of supported actions per role, see [Access policies and permissions](#access_policies).
4. [Optional: Assign a Cloud Foundry role](/docs/iam/mngcf.html#mngcf).
5. [Optional: Assign an infrastructure role](/docs/iam/infrastructureaccess.html#infrapermission).
6. Click **Invite users**.

<br />


## Understanding the IAM API key and the `bx cs credentials-set` command
{: #api_key}

To successfully provision and work with clusters in your account, you must ensure that your account is correctly set up to access the IBM Cloud infrastructure (SoftLayer) portfolio. Depending on your account setup, you either use the IAM API key or infrastructure credentials that you manually set by using the `bx cs credentials-set` command.

<dl>
  <dt>IAM API key</dt>
  <dd>The Identity and Access Management (IAM) API key is automatically set for a region when the first action that requires the {{site.data.keyword.containershort_notm}} admin access policy is performed. For example, one of your admin users creates the first cluster in the <code>us-south</code> region. By doing that, the IAM API key for this user is stored in the account for this region. The API key is used to order IBM Cloud infrastructure (SoftLayer), such as new worker nodes or VLANs. </br></br>
When a different user performs an action in this region that requires interaction with the IBM Cloud infrastructure (SoftLayer) portfolio, such as creating a new cluster or reloading a worker node, the stored API key is used to determine if sufficient permissions exist to perform that action. To make sure that infrastructure-related actions in your cluster can be successfully performed, assign your {{site.data.keyword.containershort_notm}} admin users the <strong>Super user</strong> infrastructure access policy. </br></br>You can find the current API key owner by running [<code>bx cs api-key-info</code>](cs_cli_reference.html#cs_api_key_info). If you find that you need to update the API key that is stored for a region, you can do so by running the [<code>bx cs api-key-reset</code>](cs_cli_reference.html#cs_api_key_reset) command. This command requires the {{site.data.keyword.containershort_notm}} admin access policy and stores the API key of the user that executes this command in the account. </br></br> <strong>Note:</strong> The API key that is stored for the region might not be used if IBM Cloud infrastructure (SoftLayer) credentials were manually set by using the <code>bx cs credentials-set</code> command. </dd>
<dt>IBM Cloud infrastructure (SoftLayer) credentials via <code>bx cs credentials-set</code></dt>
<dd>If you have an {{site.data.keyword.Bluemix_notm}} Pay-As-You-Go account, you have access to the IBM Cloud infrastructure (SoftLayer) portfolio by default. However, you might want to use a different IBM Cloud infrastructure (SoftLayer) account that you already have to order infrastructure. You can link this infrastructure account to your {{site.data.keyword.Bluemix_notm}} account by using the [<code>bx cs credentials-set</code>](cs_cli_reference.html#cs_credentials_set) command. </br></br>If IBM Cloud infrastructure (SoftLayer) credentials are manually set, these credentials are used to order infrastructure, even if an IAM API key already exists for the account. If the user whose credentials are stored does not have the required permissions to order infrastructure, then infrastructure-related actions, such as creating a cluster or reloading a worker node can fail. </br></br> To remove IBM Cloud infrastructure (SoftLayer) credentials that were manually set, you can use the [<code>bx cs credentials-unset</code>](cs_cli_reference.html#cs_credentials_unset) command. After the credentials are removed, the IAM API key is used to order infrastructure. </dd>
</dl>

<br />


## Customizing infrastructure permissions for a user
{: #infra_access}

When you set infrastructure policies in Identity and Access Management, a user is given permissions that are associated with a role. To customize those permissions, you must log in to IBM Cloud infrastructure (SoftLayer) and adjust the permissions there.
{: #view_access}

For example, **Basic Users** can reboot a worker node, but they cannot reload a worker node. Without giving that person **Super User** permissions, you can adjust the IBM Cloud infrastructure (SoftLayer) permissions and add the permission to run a reload command.

1.  Log in to your [{{site.data.keyword.Bluemix_notm}} account](https://console.bluemix.net/), then from the menu, select **Infrastructure**.

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
     </tbody>
    </table>

5.  To save your changes, click **Edit Portal Permissions**.

6.  In the **Device Access** tab, select the devices to grant access to.

    * In the **Device Type** dropdown, you can grant access to **All Virtual Servers**.
    * To allow users access to new devices that are created, check **Automatically grant access when new devices are added**.
    * To save your changes, click **Update Device Access**.

<br />


## Authorizing users with custom Kubernetes RBAC roles
{: #rbac}

{{site.data.keyword.containershort_notm}} access policies correspond with certain Kubernetes role-based access control (RBAC) roles as described in [Access policies and permissions](#access_policies). To authorize other Kubernetes roles that differ from the corresponding access policy, you can customize RBAC roles and then assign the roles to individuals or groups of users.
{: shortdesc}

For example, you might want to grant permissions to a team of developers to work on a certain API group or with resources within a Kubernetes namespace in the cluster, but not across the entire cluster. You create a role and then bind the role to users, using a user name that is unique to {{site.data.keyword.containershort_notm}}. For more detailed information, see [Using RBAC Authorization ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/admin/authorization/rbac/#api-overview) in the Kubernetes documentation.

Before you begin, [target the Kubernetes CLI to the cluster](cs_cli_install.html#cs_cli_configure).

1.  Create the role with the access that you want to assign.

    1. Make a `.yaml` file to define the role with the access that you want to assign.

        ```
        kind: Role
        apiVersion: rbac.authorization.k8s.io/v1
        metadata:
          namespace: default
          name: my_role
        rules:
        - apiGroups: [""]
          resources: ["pods"]
          verbs: ["get", "watch", "list"]
        - apiGroups: ["apps", "extensions"]
          resources: ["daemonsets", "deployments"]
          verbs: ["get", "list", "watch", "create", "update", "patch", "delete"]
        ```
        {: codeblock}

        <table>
        <thead>
        <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding this YAML components</th>
        </thead>
        <tbody>
        <tr>
        <td><code>kind</code></td>
        <td>Use `Role` to grant access to resources within a single namespace, or `ClusterRole` for resources cluster-wide.</td>
        </tr>
        <tr>
        <td><code>apiVersion</code></td>
        <td><ul><li>For clusters that run Kubernetes 1.8 or later, use `rbac.authorization.k8s.io/v1`. </li><li>For earlier versions, use `apiVersion: rbac.authorization.k8s.io/v1beta1`.</li></ul></td>
        </tr>
        <tr>
        <td><code>metadata/namespace</code></td>
        <td><ul><li>For `Role` kind: Specify the Kubernetes namespace to which access is granted.</li><li>Do not use the `namespace` field if you are creating a `ClusterRole` that applies at the cluster-level.</li></ul></td>
        </tr>
        <tr>
        <td><code>metadata/name</code></td>
        <td>Name the role and use the name later when you bind the role.</td>
        </tr>
        <tr>
        <td><code>rules/apiGroups</code></td>
        <td><ul><li>Specify the Kubernetes API groups that you want users to be able to interact with, such as `"apps"`, `"batch"`, or `"extensions"`. </li><li>For access to the core API group at REST path `api/v1`, leave the group blank: `[""]`.</li><li>For more information, see [API groups![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/reference/api-overview/#api-groups) in the Kubernetes documentation.</li></ul></td>
        </tr>
        <tr>
        <td><code>rules/resources</code></td>
        <td><ul><li>Specify the Kubernetes resources to which you want to grant access, such as `"daemonsets"`, `"deployments"`, `"events"`, or `"ingresses"`.</li><li>If you specify `"nodes"`, then the role kind must be `ClusterRole`.</li><li>For a list of resources, see the table of [Resource types![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/reference/kubectl/cheatsheet/) in the Kubernetes cheatsheet.</li></ul></td>
        </tr>
        <tr>
        <td><code>rules/verbs</code></td>
        <td><ul><li>Specify the types of actions that you want users to be able to do, such as `"get"`, `"list"`, `"describe"`, `"create"`, or `"delete"`. </li><li>For a full list of verbs, see the [`kubectl` documentation![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands).</li></ul></td>
        </tr>
        </tbody>
        </table>

    2.  Create the role in your cluster.

        ```
        kubectl apply -f <path_to_yaml_file>
        ```
        {: pre}

    3.  Verify that the role is created.

        ```
        kubectl get roles
        ```
        {: pre}

2.  Bind users to the role.

    1. Make a `.yaml` file to bind users to your role. Note the unique URL to use for each subject's name.

        ```
        kind: RoleBinding
        apiVersion: rbac.authorization.k8s.io/v1
        metadata:
          name: my_role_team1
          namespace: default
        subjects:
        - kind: User
          name: https://iam.ng.bluemix.net/kubernetes#user1@example.com
          apiGroup: rbac.authorization.k8s.io
        - kind: User
          name: https://iam.ng.bluemix.net/kubernetes#user2@example.com
          apiGroup: rbac.authorization.k8s.io
        roleRef:
          kind: Role
          name: custom-rbac-test
          apiGroup: rbac.authorization.k8s.io
        ```
        {: codeblock}

        <table>
        <thead>
        <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding this YAML components</th>
        </thead>
        <tbody>
        <tr>
        <td><code>kind</code></td>
        <td>Specify the `kind` as `RoleBinding` for both types of role `.yaml` files: namespace `Role` and cluster-wide `ClusterRole`.</td>
        </tr>
        <tr>
        <td><code>apiVersion</code></td>
        <td><ul><li>For clusters that run Kubernetes 1.8 or later, use `rbac.authorization.k8s.io/v1`. </li><li>For earlier versions, use `apiVersion: rbac.authorization.k8s.io/v1beta1`.</li></ul></td>
        </tr>
        <tr>
        <td><code>metadata/namespace</code></td>
        <td><ul><li>For `Role` kind: Specify the Kubernetes namespace to which access is granted.</li><li>Do not use the `namespace` field if you are creating a `ClusterRole` that applies at the cluster-level.</li></ul></td>
        </tr>
        <tr>
        <td><code>metadata/name</code></td>
        <td>Name the role binding.</td>
        </tr>
        <tr>
        <td><code>subjects/kind</code></td>
        <td>Specify the kind as `User`.</td>
        </tr>
        <tr>
        <td><code>subjects/name</code></td>
        <td><ul><li>Append the user's email address to the following URL: `https://iam.ng.bluemix.net/kubernetes#`.</li><li>For example, `https://iam.ng.bluemix.net/kubernetes#user1@example.com`</li></ul></td>
        </tr>
        <tr>
        <td><code>subjects/apiGroup</code></td>
        <td>Use `rbac.authorization.k8s.io`.</td>
        </tr>
        <tr>
        <td><code>roleRef/kind</code></td>
        <td>Enter the same value as the `kind` in the role `.yaml` file: `Role` or `ClusterRole`.</td>
        </tr>
        <tr>
        <td><code>roleRef/name</code></td>
        <td>Enter the name of the role `.yaml` file.</td>
        </tr>
        <tr>
        <td><code>roleRef/apiGroup</code></td>
        <td>Use `rbac.authorization.k8s.io`.</td>
        </tr>
        </tbody>
        </table>

    2. Create the role binding resource in your cluster.

        ```
        kubectl apply -f filepath/my_role_team1.yaml
        ```
        {: pre}

    3.  Verify that the binding is created.

        ```
        kubectl get rolebinding
        ```
        {: pre}

Now that you created and bound a custom Kubernetes RBAC role, follow up with users. Ask them to test an action that they have permission to complete due to the role, such as deleting a pod.


