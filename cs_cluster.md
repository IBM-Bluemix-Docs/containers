---

copyright:
  years: 2014, 2017
lastupdated: "2017-12-13"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:codeblock: .codeblock}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}


# Setting up clusters
{: #cs_cluster}

Design your cluster setup for maximum availability and capacity.
{:shortdesc}

The following diagram includes common cluster configurations with increasing availability.

![Stages of high availability for a cluster](images/cs_cluster_ha_roadmap.png)

As shown in the diagram, deploying your apps across multiple worker nodes makes the apps more highly available. Deploying your apps across multiple clusters makes them even more highly available. For the highest availability, deploy your apps across clusters in different regions. [For more detail, review the options for highly available cluster configurations.](cs_planning.html#cs_planning_cluster_config)

<br />


## Creating clusters with the GUI
{: #cs_cluster_ui}

A Kubernetes cluster is a set of worker nodes that are organized into a network. The purpose of the cluster is to define a set of resources, nodes, networks, and storage devices that keep applications highly available. Before you can deploy an app, you must create a cluster and set the definitions for the worker nodes in that cluster.
{:shortdesc}

To create a cluster:
1. In the catalog, select **Kubernetes Cluster**.
2. Select a type of cluster plan. You can choose either **Lite** or **Pay-As-You-Go**. With the Pay-As-You-Go plan, you can provision a standard cluster with features like multiple worker nodes for a highly available environment.
3. Configure your cluster details.
    1. Give your cluster a name, choose a version of Kubernetes, and select a location in which to deploy. Select the location that is physically closest to you for the best performance. Keep in mind that you might require legal authorization before data can be physically stored in a foreign country if you select a location outside your country.
    2. Select a type of machine and specify the number of worker nodes that you need. The machine type defines the amount of virtual CPU and memory that is set up in each worker node and made available to the containers.
        - The micro machine type indicates the smallest option.
        - A balanced machine has an equal amount of memory that is assigned to each CPU, which optimizes performance.
        - Machine types that include `encrypted` in the name encrypt the host's Docker data. The `/var/lib/docker` directory, where all container data is stored, is encrypted with LUKS encryption.
    3. Select a Public and Private VLAN from your IBM Cloud infrastructure (SoftLayer) account. Both VLANs communicate between worker nodes but the public VLAN also communicates with the IBM-managed Kubernetes master. You can use the same VLAN for multiple clusters.
        **Note**: If you choose not to select a public VLAN, you must configure an alternative solution.
    4. Select a type of hardware. Shared is a sufficient option for most situations.
        - **Dedicated**: Ensure complete isolation of your physical resources.
        - **Shared**: Allow storage of your physical resources on the same hardware as other IBM customers.
4. Click **Create cluster**. You can see the progress of the worker node deployment in the **Worker nodes** tab. When the deploy is done, you can see that your cluster is ready in the **Overview** tab.
    **Note:** Every worker node is assigned a unique worker node ID and domain name that must not be manually changed after the cluster is created. Changing the ID or domain name prevents the Kubernetes master from managing your cluster.


**What's next?**

When the cluster is up and running, you can check out the following tasks:

-   [Install the CLIs to start working with your cluster.](cs_cli_install.html#cs_cli_install)
-   [Deploy an app in your cluster.](cs_apps.html#cs_apps_cli)
-   [Set up your own private registry in {{site.data.keyword.Bluemix_notm}} to store and share Docker images with other users.](/docs/services/Registry/index.html)
- If you have a firewall, you might need to [open the required ports](cs_security.html#opening_ports) to use `bx`, `kubectl`, or `calicotl` commands, to allow outbound traffic from your cluster, or to allow inbound traffic for networking services.

<br />


## Creating clusters with the CLI
{: #cs_cluster_cli}

A cluster is a set of worker nodes that are organized into a network. The purpose of the cluster is to define a set of resources, nodes, networks, and storage devices that keep applications highly available. Before you can deploy an app, you must create a cluster and set the definitions for the worker nodes in that cluster.
{:shortdesc}

To create a cluster:
1.  Install the {{site.data.keyword.Bluemix_notm}} CLI and the [{{site.data.keyword.containershort_notm}} plug-in](cs_cli_install.html#cs_cli_install).
2.  Log in to the {{site.data.keyword.Bluemix_notm}} CLI. Enter your {{site.data.keyword.Bluemix_notm}} credentials when prompted.

    ```
    bx login
    ```
    {: pre}

    **Note:** If you have a federated ID, use `bx login --sso` to log in to the {{site.data.keyword.Bluemix_notm}} CLI. Enter your user name and use the provided URL in your CLI output to retrieve your one-time passcode. You know you have a federated ID when the login fails without the `--sso` and succeeds with the `--sso` option.

3. If you have multiple {{site.data.keyword.Bluemix_notm}} accounts, select the account where you want to create your Kubernetes cluster.

4.  If you want to create or access Kubernetes clusters in a region other than the {{site.data.keyword.Bluemix_notm}} region that you selected earlier, run `bx cs region-set`.

6.  Create a cluster.
    1.  Review the locations that are available. The locations that are shown depend on the {{site.data.keyword.containershort_notm}} region that you are logged in.

        ```
        bx cs locations
        ```
        {: pre}

        Your CLI output matches the [locations for the container region](cs_regions.html#locations).

    2.  Choose a location and review the machine types available in that location. The machine type specifies the virtual compute resources that are available to each worker node.

        ```
        bx cs machine-types <location>
        ```
        {: pre}

        ```
        Getting machine types list...
        OK
        Machine Types
        Name         Cores   Memory   Network Speed   OS             Storage   Server Type
        u2c.2x4      2       4GB      1000Mbps        UBUNTU_16_64   100GB     virtual
        b2c.4x16     4       16GB     1000Mbps        UBUNTU_16_64   100GB     virtual
        b2c.16x64    16      64GB     1000Mbps        UBUNTU_16_64   100GB     virtual
        b2c.32x128   32      128GB    1000Mbps        UBUNTU_16_64   100GB     virtual
        b2c.56x242   56      242GB    1000Mbps        UBUNTU_16_64   100GB     virtual
        ```
        {: screen}

    3.  Check to see if a public and private VLAN already exists in the IBM Cloud infrastructure (SoftLayer) for this account.

        ```
        bx cs vlans <location>
        ```
        {: pre}

        ```
        ID        Name                Number   Type      Router
        1519999   vlan   1355     private   bcr02a.dal10
        1519898   vlan   1357     private   bcr02a.dal10
        1518787   vlan   1252     public   fcr02a.dal10
        1518888   vlan   1254     public    fcr02a.dal10
        ```
        {: screen}

        If a public and private VLAN already exists, note the matching routers. Private VLAN routers always begin with `bcr` (back-end router) and public VLAN routers always begin with `fcr` (front-end router). The number and letter combination after those prefixes must match to use those VLANs when creating a cluster. In the example output, any of the private VLANs can be used with any of public VLANs because the routers all include `02a.dal10`.

    4.  Run the `cluster-create` command. You can choose between a lite cluster, which includes one worker node set up with 2vCPU and 4GB memory, or a standard cluster, which can include as many worker nodes as you choose in your IBM Cloud infrastructure (SoftLayer) account. When you create a standard cluster, by default, the hardware of the worker node is shared by multiple IBM customers and billed by hours of usage. </br>Example for a standard cluster:

        ```
        bx cs cluster-create --location dal10 --public-vlan <public_vlan_id> --private-vlan <private_vlan_id> --machine-type u2c.2x4 --workers 3 --name <cluster_name> --kube-version <major.minor.patch>
        ```
        {: pre}

        Example for a lite cluster:

        ```
        bx cs cluster-create --name my_cluster
        ```
        {: pre}

        <table>
        <caption>Table 1. Understanding <code>bx cs cluster-create</code> command components</caption>
        <thead>
        <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding this command's components</th>
        </thead>
        <tbody>
        <tr>
        <td><code>cluster-create</code></td>
        <td>The command to create a cluster in your {{site.data.keyword.Bluemix_notm}} organization.</td>
        </tr>
        <tr>
        <td><code>--location <em>&lt;location&gt;</em></code></td>
        <td>Replace <em>&lt;location&gt;</em> with the {{site.data.keyword.Bluemix_notm}} location ID where you want to create your cluster. [Available locations](cs_regions.html#locations) depend on the {{site.data.keyword.containershort_notm}} region you are logged in to.</td>
        </tr>
        <tr>
        <td><code>--machine-type <em>&lt;machine_type&gt;</em></code></td>
        <td>If you are creating a standard cluster, choose a machine type. The machine type specifies the virtual compute resources that are available to each worker node. Review [Comparison of lite and standard clusters for {{site.data.keyword.containershort_notm}}](cs_planning.html#cs_planning_cluster_type) for more information. For lite clusters, you do not have to define the machine type.</td>
        </tr>
        <tr>
        <td><code>--public-vlan <em>&lt;public_vlan_id&gt;</em></code></td>
        <td><ul>
          <li>For lite clusters, you do not have to define a public VLAN. Your lite cluster is automatically connected to a public VLAN that is owned by IBM.</li>
          <li>For a standard cluster, if you already have a public VLAN set up in your IBM Cloud infrastructure (SoftLayer) account for that location, enter the ID of the public VLAN. If you do not have both a public and a private VLAN in your account, do not specify this option. {{site.data.keyword.containershort_notm}} automatically creates a public VLAN for you.<br/><br/>
          <strong>Note</strong>: Private VLAN routers always begin with <code>bcr</code> (back-end router) and public VLAN routers always begin with <code>fcr</code> (front-end router). The number and letter combination after those prefixes must match to use those VLANs when creating a cluster.</li>
        </ul></td>
        </tr>
        <tr>
        <td><code>--private-vlan <em>&lt;private_vlan_id&gt;</em></code></td>
        <td><ul><li>For lite clusters, you do not have to define a private VLAN. Your lite cluster is automatically connected to a private VLAN that is owned by IBM.</li><li>For a standard cluster, if you already have a private VLAN set up in your IBM Cloud infrastructure (SoftLayer) account for that location, enter the ID of the private VLAN. If you do not have both a public and a private VLAN in your account, do not specify this option. {{site.data.keyword.containershort_notm}} automatically creates a public VLAN for you.<br/><br/><strong>Note</strong>: Private VLAN routers always begin with <code>bcr</code> (back-end router) and public VLAN routers always begin with <code>fcr</code> (front-end router). The number and letter combination after those prefixes must match to use those VLANs when creating a cluster.</li></ul></td>
        </tr>
        <tr>
        <td><code>--name <em>&lt;name&gt;</em></code></td>
        <td>Replace <em>&lt;name&gt;</em> with a name for your cluster.</td>
        </tr>
        <tr>
        <td><code>--workers <em>&lt;number&gt;</em></code></td>
        <td>The number of worker nodes to include in the cluster. If the <code>--workers</code> option is not specified, 1 worker node is created.</td>
        </tr>
        <tr>
          <td><code>--kube-version <em>&lt;major.minor.patch&gt;</em></code></td>
          <td>The Kubernetes version for the cluster master node. This value is optional. Unless specified, the cluster is created with the default of supported Kubernetes versions. To see available versions, run <code>bx cs kube-versions</code>.</td>
        </tr>
        </tbody></table>

7.  Verify that the creation of the cluster was requested.

    ```
    bx cs clusters
    ```
    {: pre}

    **Note:** It can take up to 15 minutes for the worker node machines to be ordered, and for the cluster to be set up and provisioned in your account.

    When the provisioning of your cluster is completed, the status of your cluster changes to **deployed**.

    ```
    Name         ID                                   State      Created          Workers
    my_cluster   paf97e8843e29941b49c598f516de72101   deployed   20170201162433   1
    ```
    {: screen}

8.  Check the status of the worker nodes.

    ```
    bx cs workers <cluster>
    ```
    {: pre}

    When the worker nodes are ready, the state changes to **normal** and the status is **Ready**. When the node status is **Ready**, you can then access the cluster.

    **Note:** Every worker node is assigned a unique worker node ID and domain name that must not be changed manually after the cluster is created. Changing the ID or domain name prevents the Kubernetes master from managing your cluster.

    ```
    ID                                                  Public IP        Private IP     Machine Type   State      Status
    prod-dal10-pa8dfcc5223804439c87489886dbbc9c07-w1   169.47.223.113   10.171.42.93   free           normal    Ready
    ```
    {: screen}

9. Set the cluster you created as the context for this session. Complete these configuration steps every time that you work with your cluster.
    1.  Get the command to set the environment variable and download the Kubernetes configuration files.

        ```
        bx cs cluster-config <cluster_name_or_id>
        ```
        {: pre}

        When the download of the configuration files is finished, a command is displayed that you can use to set the path to the local Kubernetes configuration file as an environment variable.

        Example for OS X:

        ```
        export KUBECONFIG=/Users/<user_name>/.bluemix/plugins/container-service/clusters/<cluster_name>/kube-config-prod-dal10-<cluster_name>.yml
        ```
        {: screen}

    2.  Copy and paste the command that is displayed in your terminal to set the `KUBECONFIG` environment variable.
    3.  Verify that the `KUBECONFIG` environment variable is set properly.

        Example for OS X:

        ```
        echo $KUBECONFIG
        ```
        {: pre}

        Output:

        ```
        /Users/<user_name>/.bluemix/plugins/container-service/clusters/<cluster_name>/kube-config-prod-dal10-<cluster_name>.yml

        ```
        {: screen}

10. Launch your Kubernetes dashboard with the default port `8001`.
    1.  Set the proxy with the default port number.

        ```
        kubectl proxy
        ```
        {: pre}

        ```
        Starting to serve on 127.0.0.1:8001
        ```
        {: screen}

    2.  Open the following URL in a web browser to see the Kubernetes dashboard.

        ```
        http://localhost:8001/ui
        ```
        {: codeblock}


**What's next?**

-   [Deploy an app in your cluster.](cs_apps.html#cs_apps_cli)
-   [Manage your cluster with the `kubectl` command line. ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/user-guide/kubectl/)
-   [Set up your own private registry in {{site.data.keyword.Bluemix_notm}} to store and share Docker images with other users.](/docs/services/Registry/index.html)
- If you have a firewall, you might need to [open the required ports](cs_security.html#opening_ports) to use `bx`, `kubectl`, or `calicotl` commands, to allow outbound traffic from your cluster, or to allow inbound traffic for networking services.

<br />


## Using private and public image registries
{: #cs_apps_images}

A Docker image is the basis for every container that you create. An image is created from a Dockerfile, which is a file that contains instructions to build the image. A Dockerfile might reference build artifacts in its instructions that are stored separately, such as an app, the app's configuration, and its dependencies. Images are typically stored in a registry that can either be accessible by the public (public registry) or set up with limited access for a small group of users (private registry).
{:shortdesc}

Review the following options to find information about how to set up an image registry and how to use an image from the registry.

-   [Accessing a namespace in {{site.data.keyword.registryshort_notm}} to work with IBM-provided images and your own private Docker images](#bx_registry_default).
-   [Accessing public images from Docker Hub](#dockerhub).
-   [Accessing private images that are stored in other private registries](#private_registry).

### Accessing a namespace in {{site.data.keyword.registryshort_notm}} to work with IBM-provided images and your own private Docker images
{: #bx_registry_default}

You can deploy containers to your cluster from an IBM-provided public image or a private image that is stored in your namespace in {{site.data.keyword.registryshort_notm}}.

Before you begin:

1. [Set up a namespace in {{site.data.keyword.registryshort_notm}} on {{site.data.keyword.Bluemix_notm}} Public or {{site.data.keyword.Bluemix_dedicated_notm}} and push images to this namespace](/docs/services/Registry/registry_setup_cli_namespace.html#registry_namespace_add).
2. [Create a cluster](#cs_cluster_cli).
3. [Target your CLI to your cluster](cs_cli_install.html#cs_cli_configure).

When you create a cluster, a non-expiring registry token is automatically created for the cluster. This token is used to authorize read-only access to any of your namespaces that you set up in {{site.data.keyword.registryshort_notm}} so that you can work with IBM-provided public and your own private Docker images. Tokens must be stored in a Kubernetes `imagePullSecret` so that they are accessible to a Kubernetes cluster when you deploy a containerized app. When your cluster is created, {{site.data.keyword.containershort_notm}} automatically stores this token in a Kubernetes `imagePullSecret`. The `imagePullSecret` is added to the default Kubernetes namespace, the default list of secrets in the ServiceAccount for that namespace, and the kube-system namespace.

**Note:** By using this initial setup, you can deploy containers from any image that is available in a namespace in your {{site.data.keyword.Bluemix_notm}} account into the **default** namespace of your cluster. If you want to deploy a container into other namespaces of your cluster, or if you want to use an image that is stored in another {{site.data.keyword.Bluemix_notm}} region or in another {{site.data.keyword.Bluemix_notm}} account, you must [create your own imagePullSecret for your cluster](#bx_registry_other).

To deploy a container into the **default** namespace of your cluster, create a configuration file.

1.  Create a deployment configuration file that is named `mydeployment.yaml`.
2.  Define the deployment and the image that you want to use from your namespace in {{site.data.keyword.registryshort_notm}}.

    To use a private image from a namespace in {{site.data.keyword.registryshort_notm}}:

    ```
    apiVersion: extensions/v1beta1
    kind: Deployment
    metadata:
      name: ibmliberty-deployment
    spec:
      replicas: 3
      template:
        metadata:
          labels:
            app: ibmliberty
        spec:
          containers:
          - name: ibmliberty
            image: registry.<region>.bluemix.net/<namespace>/<my_image>:<tag>
    ```
    {: codeblock}

    **Tip:** To retrieve your namespace information, run `bx cr namespace-list`.

3.  Create the deployment in your cluster.

    ```
    kubectl apply -f mydeployment.yaml
    ```
    {: pre}

    **Tip:** You can also deploy an existing configuration file, such as one of the IBM-provided public images. This example uses the **ibmliberty** image in the US-South region.

    ```
    kubectl apply -f https://raw.githubusercontent.com/IBM-{{site.data.keyword.Bluemix_notm}}/kube-samples/master/deploy-apps-clusters/deploy-ibmliberty.yaml
    ```
    {: pre}

### Deploying images to other Kubernetes namespaces or accessing images in other {{site.data.keyword.Bluemix_notm}} regions and accounts
{: #bx_registry_other}

You can deploy containers to other Kubernetes namespaces, use images that are stored in other {{site.data.keyword.Bluemix_notm}} regions or accounts, or use images that are stored in {{site.data.keyword.Bluemix_dedicated_notm}} by creating your own imagePullSecret.

Before you begin:

1.  [Set up a namespace in {{site.data.keyword.registryshort_notm}} on {{site.data.keyword.Bluemix_notm}} Public or {{site.data.keyword.Bluemix_dedicated_notm}} and push images to this namespace](/docs/services/Registry/registry_setup_cli_namespace.html#registry_namespace_add).
2.  [Create a cluster](#cs_cluster_cli).
3.  [Target your CLI to your cluster](cs_cli_install.html#cs_cli_configure).

To create your own imagePullSecret:

**Note:** ImagePullSecrets are valid only for the Kubernetes namespaces that they were created for. Repeat these steps for every namespace where you want to deploy containers. Images from [DockerHub](#dockerhub) do not require ImagePullSecrets.

1.  If you do not have a token, [create a token for the registry that you want to access.](/docs/services/Registry/registry_tokens.html#registry_tokens_create)
2.  List tokens in your {{site.data.keyword.Bluemix_notm}} account.

    ```
    bx cr token-list
    ```
    {: pre}

3.  Note the token ID that you want to use.
4.  Retrieve the value for your token. Replace <em>&lt;token_id&gt;</em> with the ID of the token that you retrieved in the previous step.

    ```
    bx cr token-get <token_id>
    ```
    {: pre}

    Your token value is displayed in the **Token** field of your CLI output.

5.  Create the Kubernetes secret to store your token information.

    ```
    kubectl --namespace <kubernetes_namespace> create secret docker-registry <secret_name>  --docker-server=<registry_url> --docker-username=token --docker-password=<token_value> --docker-email=<docker_email>
    ```
    {: pre}

    <table>
    <caption>Table 3. Understanding this command's components</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding this command's components</th>
    </thead>
    <tbody>
    <tr>
    <td><code>--namespace <em>&lt;kubernetes_namespace&gt;</em></code></td>
    <td>Required. The Kubernetes namespace of your cluster where you want to use the secret and deploy containers to. Run <code>kubectl get namespaces</code> to list all namespaces in your cluster.</td>
    </tr>
    <tr>
    <td><code><em>&lt;secret_name&gt;</em></code></td>
    <td>Required. The name that you want to use for your imagePullSecret.</td>
    </tr>
    <tr>
    <td><code>--docker-server <em>&lt;registry_url&gt;</em></code></td>
    <td>Required. The URL to the image registry where your namespace is set up.<ul><li>For namespaces that are set up in US-South and US-East registry.ng.bluemix.net</li><li>For namespaces that are set up in UK-South registry.eu-gb.bluemix.net</li><li>For namespaces that are set up in EU-Central (Frankfurt) registry.eu-de.bluemix.net</li><li>For namespaces that are set up in Australia (Sydney) registry.au-syd.bluemix.net</li><li>For namespaces that are set up in {{site.data.keyword.Bluemix_dedicated_notm}} registry.<em>&lt;dedicated_domain&gt;</em></li></ul></td>
    </tr>
    <tr>
    <td><code>--docker-username <em>&lt;docker_username&gt;</em></code></td>
    <td>Required. The user name to log in to your private registry. For {{site.data.keyword.registryshort_notm}}, the user name is set to <code>token</code>.</td>
    </tr>
    <tr>
    <td><code>--docker-password <em>&lt;token_value&gt;</em></code></td>
    <td>Required. The value of your registry token that you retrieved earlier.</td>
    </tr>
    <tr>
    <td><code>--docker-email <em>&lt;docker-email&gt;</em></code></td>
    <td>Required. If you have one, enter your Docker email address. If you do not have one, enter a fictional email address, as for example a@b.c. This email is mandatory to create a Kubernetes secret, but is not used after creation.</td>
    </tr>
    </tbody></table>

6.  Verify that the secret was created successfully. Replace <em>&lt;kubernetes_namespace&gt;</em> with the name of the namespace where you created the imagePullSecret.

    ```
    kubectl get secrets --namespace <kubernetes_namespace>
    ```
    {: pre}

7.  Create a pod that references the imagePullSecret.
    1.  Create a pod configuration file that is named `mypod.yaml`.
    2.  Define the pod and the imagePullSecret that you want to use to access the private {{site.data.keyword.Bluemix_notm}} registry.

        A private image from a namespace:

        ```
        apiVersion: v1
        kind: Pod
        metadata:
          name: <pod_name>
        spec:
          containers:
            - name: <container_name>
              image: registry.<region>.bluemix.net/<my_namespace>/<my_image>:<tag>
          imagePullSecrets:
            - name: <secret_name>
        ```
        {: codeblock}

        An {{site.data.keyword.Bluemix_notm}} public image:

        ```
        apiVersion: v1
        kind: Pod
        metadata:
          name: <pod_name>
        spec:
          containers:
            - name: <container_name>
              image: registry.<region>.bluemix.net/
          imagePullSecrets:
            - name: <secret_name>
        ```
        {: codeblock}

        <table>
        <caption>Table 4. Understanding the YAML file components</caption>
        <thead>
        <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the YAML file components</th>
        </thead>
        <tbody>
        <tr>
        <td><code><em>&lt;container_name&gt;</em></code></td>
        <td>The name of the container that you want to deploy to your cluster.</td>
        </tr>
        <tr>
        <td><code><em>&lt;my_namespace&gt;</em></code></td>
        <td>The namespace where your image is stored. To list available namespaces, run `bx cr namespace-list`.</td>
        </tr>
        <tr>
        <td><code><em>&lt;my_image&gt;</em></code></td>
        <td>The name of the image that you want to use. To list available images in an {{site.data.keyword.Bluemix_notm}} account, run `bx cr image-list`.</td>
        </tr>
        <tr>
        <td><code><em>&lt;tag&gt;</em></code></td>
        <td>The version of the image that you want to use. If no tag is specified, the image that is tagged <strong>latest</strong> is used by default.</td>
        </tr>
        <tr>
        <td><code><em>&lt;secret_name&gt;</em></code></td>
        <td>The name of the imagePullSecret that you created earlier.</td>
        </tr>
        </tbody></table>

   3.  Save your changes.
   4.  Create the deployment in your cluster.

        ```
        kubectl apply -f mypod.yaml
        ```
        {: pre}


### Accessing public images from Docker Hub
{: #dockerhub}

You can use any public image that is stored in Docker Hub to deploy a container to your cluster without any additional configuration.

Before you begin:

1.  [Create a cluster](#cs_cluster_cli).
2.  [Target your CLI to your cluster](cs_cli_install.html#cs_cli_configure).

Create a deployment configuration file.

1.  Create a configuration file that is named `mydeployment.yaml`.
2.  Define the deployment and the public image from Docker Hub that you want to use. The following configuration file uses the public NGINX image that is available on Docker Hub.

    ```
    apiVersion: extensions/v1beta1
    kind: Deployment
    metadata:
      name: nginx-deployment
    spec:
      replicas: 3
      template:
        metadata:
          labels:
            app: nginx
        spec:
          containers:
          - name: nginx
            image: nginx
    ```
    {: codeblock}

3.  Create the deployment in your cluster.

    ```
    kubectl apply -f mydeployment.yaml
    ```
    {: pre}

    **Tip:** Alternatively, deploy an existing configuration file. The following example uses the same public NGINX image, but applies it directly to your cluster.

    ```
    kubectl apply -f https://raw.githubusercontent.com/IBM-{{site.data.keyword.Bluemix_notm}}/kube-samples/master/deploy-apps-clusters/deploy-nginx.yaml
    ```
    {: pre}


### Accessing private images that are stored in other private registries
{: #private_registry}

If you already have a private registry that you want to use, you must store the registry credentials in a Kubernetes imagePullSecret and reference this secret in your configuration file.

Before you begin:

1.  [Create a cluster](#cs_cluster_cli).
2.  [Target your CLI to your cluster](cs_cli_install.html#cs_cli_configure).

To create an imagePullSecret:

**Note:** ImagePullSecrets are valid for the Kubernetes namespaces they were created for. Repeat these steps for every namespace where you want to deploy containers from an image in a private {{site.data.keyword.Bluemix_notm}} registry.

1.  Create the Kubernetes secret to store your private registry credentials.

    ```
    kubectl --namespace <kubernetes_namespace> create secret docker-registry <secret_name>  --docker-server=<registry_url> --docker-username=<docker_username> --docker-password=<docker_password> --docker-email=<docker_email>
    ```
    {: pre}

    <table>
    <caption>Table 5. Understanding this command's components</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding this command's components</th>
    </thead>
    <tbody>
    <tr>
    <td><code>--namespace <em>&lt;kubernetes_namespace&gt;</em></code></td>
    <td>Required. The Kubernetes namespace of your cluster where you want to use the secret and deploy containers to. Run <code>kubectl get namespaces</code> to list all namespaces in your cluster.</td>
    </tr>
    <tr>
    <td><code><em>&lt;secret_name&gt;</em></code></td>
    <td>Required. The name that you want to use for your imagePullSecret.</td>
    </tr>
    <tr>
    <td><code>--docker-server <em>&lt;registry_url&gt;</em></code></td>
    <td>Required. The URL to the registry where your private images are stored.</td>
    </tr>
    <tr>
    <td><code>--docker-username <em>&lt;docker_username&gt;</em></code></td>
    <td>Required. The user name to log in to your private registry.</td>
    </tr>
    <tr>
    <td><code>--docker-password <em>&lt;token_value&gt;</em></code></td>
    <td>Required. The value of your registry token that you retrieved earlier.</td>
    </tr>
    <tr>
    <td><code>--docker-email <em>&lt;docker-email&gt;</em></code></td>
    <td>Required. If you have one, enter your Docker email address. If you do not have one, enter a fictional email address, as for example a@b.c. This email is mandatory to create a Kubernetes secret, but is not used after creation.</td>
    </tr>
    </tbody></table>

2.  Verify that the secret was created successfully. Replace <em>&lt;kubernetes_namespace&gt;</em> with the name of the namespace where you created the imagePullSecret.

    ```
    kubectl get secrets --namespace <kubernetes_namespace>
    ```
    {: pre}

3.  Create a pod that references the imagePullSecret.
    1.  Create a pod configuration file that is named `mypod.yaml`.
    2.  Define the pod and the imagePullSecret that you want to use to access the private {{site.data.keyword.Bluemix_notm}} registry. To use a private image from your private registry:

        ```
        apiVersion: v1
        kind: Pod
        metadata:
          name: <pod_name>
        spec:
          containers:
            - name: <container_name>
              image: <my_image>:<tag>
          imagePullSecrets:
            - name: <secret_name>
        ```
        {: codeblock}

        <table>
        <caption>Table 6. Understanding the YAML file components</caption>
        <thead>
        <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the YAML file components</th>
        </thead>
        <tbody>
        <tr>
        <td><code><em>&lt;pod_name&gt;</em></code></td>
        <td>The name of the pod that you want to create.</td>
        </tr>
        <tr>
        <td><code><em>&lt;container_name&gt;</em></code></td>
        <td>The name of the container that you want to deploy to your cluster.</td>
        </tr>
        <tr>
        <td><code><em>&lt;my_image&gt;</em></code></td>
        <td>The full path to the image in your private registry that you want to use.</td>
        </tr>
        <tr>
        <td><code><em>&lt;tag&gt;</em></code></td>
        <td>The version of the image that you want to use. If no tag is specified, the image that is tagged <strong>latest</strong> is used by default.</td>
        </tr>
        <tr>
        <td><code><em>&lt;secret_name&gt;</em></code></td>
        <td>The name of the imagePullSecret that you created earlier.</td>
        </tr>
        </tbody></table>

  3.  Save your changes.
  4.  Create the deployment in your cluster.

        ```
        kubectl apply -f mypod.yaml
        ```
        {: pre}

<br />


## Adding {{site.data.keyword.Bluemix_notm}} services to clusters
{: #cs_cluster_service}

Add an existing {{site.data.keyword.Bluemix_notm}} service instance to your cluster to enable your cluster users to access and use the {{site.data.keyword.Bluemix_notm}} service when they deploy an app to the cluster.
{:shortdesc}

Before you begin:

1. [Target your CLI](cs_cli_install.html#cs_cli_configure) to your cluster.
2. [Request an instance of the {{site.data.keyword.Bluemix_notm}} service](/docs/manageapps/reqnsi.html#req_instance).
   **Note:** To create an instance of a service in the Washington DC location, you must use the CLI.

**Note:**
<ul><ul>
<li>You can only add {{site.data.keyword.Bluemix_notm}} services that support service keys. If the service does not support service keys, see [Enabling external apps to use {{site.data.keyword.Bluemix_notm}} services](/docs/manageapps/reqnsi.html#req_instance).</li>
<li>The cluster and the worker nodes must be deployed fully before you can add a service.</li>
</ul></ul>


To add a service:
2.  List available {{site.data.keyword.Bluemix_notm}} services.

    ```
    bx service list
    ```
    {: pre}

    Example CLI output:

    ```
    name                      service           plan    bound apps   last operation
    <service_instance_name>   <service_name>    spark                create succeeded
    ```
    {: screen}

3.  Note the **name** of the service instance that you want to add to your cluster.
4.  Identify the cluster namespace that you want to use to add your service. Choose between the following options.
    -   List existing namespaces and choose a namespace that you want to use.

        ```
        kubectl get namespaces
        ```
        {: pre}

    -   Create a new namespace in your cluster.

        ```
        kubectl create namespace <namespace_name>
        ```
        {: pre}

5.  Add the service to your cluster.

    ```
    bx cs cluster-service-bind <cluster_name_or_id> <namespace> <service_instance_name>
    ```
    {: pre}

    When the service is successfully added to your cluster, a cluster secret is created that holds the credentials of your service instance. Example CLI output:

    ```
    bx cs cluster-service-bind mycluster mynamespace cleardb
    Binding service instance to namespace...
    OK
    Namespace:	mynamespace
    Secret name:     binding-<service_instance_name>
    ```
    {: screen}

6.  Verify that the secret was created in your cluster namespace.

    ```
    kubectl get secrets --namespace=<namespace>
    ```
    {: pre}


To use the service in a pod that is deployed in the cluster, cluster users can access the service credentials of the {{site.data.keyword.Bluemix_notm}} service by [mounting the Kubernetes secret as a secret volume to a pod](cs_apps.html#cs_apps_service).

<br />



## Managing cluster access
{: #cs_cluster_user}

Every user that works with {{site.data.keyword.containershort_notm}} must be assigned a combination of service-specific user roles that determine which actions the user can perform.
{:shortdesc}

<dl>
<dt>{{site.data.keyword.containershort_notm}} access policies</dt>
<dd>In Identity and Access Management, {{site.data.keyword.containershort_notm}} access policies determine the cluster management actions that you can perform on a cluster, such as creating or removing clusters, and adding or removing extra worker nodes. These policies must be set in conjunction with infrastructure policies.</dd>
<dt>Infrastructure access policies</dt>
<dd>In Identity and Access Management, infrastructure access policies allow the actions that are requested from the {{site.data.keyword.containershort_notm}} user interface or the CLI to be completed in IBM Cloud infrastructure (SoftLayer). These policies must be set in conjunction with {{site.data.keyword.containershort_notm}} access policies. [Learn more about the available infrastructure roles](/docs/iam/infrastructureaccess.html#infrapermission).</dd>
<dt>Resource groups</dt>
<dd>A resource group is a way to organize {{site.data.keyword.Bluemix_notm}} services into groupings so that you can quickly assign users access to more than one resource at a time. [Learn how to manage users by using resource groups](/docs/admin/resourcegroups.html#rgs).</dd>
<dt>Cloud Foundry roles</dt>
<dd>In Identity and Access Management, every user must be assigned a Cloud Foundry user role. This role determines the actions that the user can perform on the {{site.data.keyword.Bluemix_notm}} account, such as inviting other users, or viewing the quota usage. [Learn more about the available Cloud Foundry roles](/docs/iam/cfaccess.html#cfaccess).</dd>
<dt>Kubernetes RBAC roles</dt>
<dd>Every user who is assigned an {{site.data.keyword.containershort_notm}} access policy is automatically assigned a Kubernetes RBAC role. In Kubernetes, RBAC roles determine the actions that you can perform on Kubernetes resources inside the cluster. RBAC roles are set up for the default namespace only. The cluster administrator can add RBAC roles for other namespaces in the cluster. See [Using RBAC Authorization ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/admin/authorization/rbac/#api-overview) in the Kubernetes documentation for more information.</dd>
</dl>

In this section:

-   [Access policies and permissions](#access_ov)
-   [Adding users to the {{site.data.keyword.Bluemix_notm}} account](#add_users)
-   [Customizing infrastructure permissions for a user](#infrastructure_permissions)

### Access policies and permissions
{: #access_ov}

Review the access policies and permissions that you can grant to users in your {{site.data.keyword.Bluemix_notm}} account. The operator and editor roles have separate permissions. If you want a user to, for example, add worker nodes and bind services, then you must assign the user both the operator and editor roles.

|{{site.data.keyword.containershort_notm}} access policy|Cluster management permissions|Kubernetes resource permissions|
|-------------|------------------------------|-------------------------------|
|Administrator|This role inherits permissions from the Editor, Operator, and Viewer roles for all clusters in this account. <br/><br/>When set for all current service instances:<ul><li>Create a lite or standard cluster</li><li>Set credentials for an {{site.data.keyword.Bluemix_notm}} account to access the IBM Cloud infrastructure (SoftLayer) portfolio</li><li>Remove a cluster</li><li>Assign and change {{site.data.keyword.containershort_notm}} access policies for other existing users in this account.</li></ul><p>When set for a specific cluster ID:<ul><li>Remove a specific cluster</li></ul></p>Corresponding infrastructure access policy: Super user<br/><br/><b>Note</b>: To create resources such as machines, VLANs, and subnets, users need the **Super user** infrastructure role.|<ul><li>RBAC Role: cluster-admin</li><li>Read/write access to resources in every namespace</li><li>Create roles within a namespace</li><li>Access Kubernetes dashboard</li><li>Create an Ingress resource that makes apps publically available</li></ul>|
|Operator|<ul><li>Add additional worker nodes to a cluster</li><li>Remove worker nodes from a cluster</li><li>Reboot a worker node</li><li>Reload a worker node</li><li>Add a subnet to a cluster</li></ul><p>Corresponding infrastructure access policy: Basic user</p>|<ul><li>RBAC Role: admin</li><li>Read/write access to resources inside the default namespace but not to the namespace itself</li><li>Create roles within a namespace</li></ul>|
|Editor <br/><br/><b>Tip</b>: Use this role for app developers.|<ul><li>Bind an {{site.data.keyword.Bluemix_notm}} service to a cluster.</li><li>Unbind an {{site.data.keyword.Bluemix_notm}} service to a cluster.</li><li>Create a webhook.</li></ul><p>Corresponding infrastructure access policy: Basic user|<ul><li>RBAC Role: edit</li><li>Read/write access to resources inside the default namespace</li></ul></p>|
|Viewer|<ul><li>List a cluster</li><li>View details for a cluster</li></ul><p>Corresponding infrastructure access policy: View only</p>|<ul><li>RBAC Role: view</li><li>Read access to resources inside the default namespace</li><li>No read access to Kubernetes secrets</li></ul>|
{: caption="Table 7. {{site.data.keyword.containershort_notm}} access policies and permissions" caption-side="top"}

|Cloud Foundry access policy|Account management permissions|
|-------------|------------------------------|
|Organization role: Manager|<ul><li>Add additional users to an {{site.data.keyword.Bluemix_notm}} account</li></ul>| |
|Space role: Developer|<ul><li>Create {{site.data.keyword.Bluemix_notm}} service instances</li><li>Bind {{site.data.keyword.Bluemix_notm}} service instances to clusters</li></ul>| 
{: caption="Table 8. Cloud Foundry access policies and permissions" caption-side="top"}


### Adding users to an {{site.data.keyword.Bluemix_notm}} account
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
      4. In the **Select roles** section, choose a role. To find a list of supported actions per role, see [Access policies and permissions](#access_ov).
  - For **Resource group**:
      1. From the **Resource group** drop-down list, select the resource group that includes permissions for your account's {{site.data.keyword.containershort_notm}} resource.
      2. From the **Assign access to a resource group** drop-down list, select a role. To find a list of supported actions per role, see [Access policies and permissions](#access_ov).
4. [Optional: Assign an infrastructure role](/docs/iam/mnginfra.html#managing-infrastructure-access).
5. [Optional: Assign a Cloud Foundry role](/docs/iam/mngcf.html#mngcf).
5. Click **Invite users**.



### Customizing infrastructure permissions for a user
{: #infrastructure_permissions}

When you set infrastructure policies in Identity and Access Management, a user is given permissions associated with a role. To customize those permissions, you must log into IBM Cloud infrastructure (SoftLayer) and adjust the permissions there.
{: #view_access}

For example, basic users can reboot a worker node, but they cannot reload a worker node. Without giving that person super user permissions, you can adjust the IBM Cloud infrastructure (SoftLayer) permissions and add the permission to run a reload command.

1.  Log in to your IBM Cloud infrastructure (SoftLayer) account.
2.  Select a user profile to update.
3.  In the **Portal Permissions**, customize the user's access. For example, to add reload permission, in the **Devices** tab, select **Issue OS Reloads and Initiate Rescue Kernel**.
9.  Save your changes.

<br />



## Updating the Kubernetes master
{: #cs_cluster_update}

Kubernetes periodically updates [major, minor, and patch versions](cs_versions.html#version_types), which impacts your clusters. Updating a cluster is a two-step process. First, you must update the Kubernetes master, and then you can update each of the worker nodes.

By default, you cannot update a Kubernetes master more than two minor versions ahead. For example, if your current master is version 1.5 and you want to update to 1.8, you must update to 1.7 first. You can force the update to continue, but updating more than two minor versions might cause unexpected results.

**Attention**: Follow the update instructions and use a test cluster to address potential app outages and interruptions during the update. You cannot roll back a cluster to a previous version.

When making a _major_ or _minor_ update, complete the following steps.

1. Review the [Kubernetes changes](cs_versions.html) and make any updates marked _Update before master_.
2. Update your Kubernetes master by using the GUI or running the [CLI command](cs_cli_reference.html#cs_cluster_update). When you update the Kubernetes master, the master is down for about 5 - 10 minutes. During the update, you cannot access or change the cluster. However, worker nodes, apps, and resources that cluster users have deployed are not modified and continue to run.
3. Confirm that the update is complete. Review the Kubernetes version on the {{site.data.keyword.Bluemix_notm}} Dashboard or run `bx cs clusters`.

When the Kubernetes master update is complete, you can update your worker nodes.

<br />


## Updating worker nodes
{: #cs_cluster_worker_update}

While IBM automatically applies patches to the Kubernetes master, you must explicitly update worker nodes for major and minor updates. The worker node version cannot be higher than the Kubernetes master.

**Attention**: Updates to worker nodes can cause downtime for your apps and services:
- Data is deleted if not stored outside the pod.
- Use [replicas ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#replicas) in your deployments to reschedule pods on available nodes.

Updating production-level clusters:
- To help avoid downtime for your apps, the update process prevents pods from being scheduled on the worker node during the update. See [`kubectl drain` ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#drain) for more information.
- Use a test cluster to validate that your workloads and the delivery process are not impacted by the update. You cannot roll back worker nodes to a previous version.
- Production-level clusters should have capacity to survive a worker node failure. If your cluster does not, add a worker node before updating the cluster.
- A rolling update is performed when multiple worker nodes are requested to upgrade. A maximum of 20 percent of the total amount of worker nodes in a cluster can be upgraded simultaneously. The upgrade process waits for a worker node to complete the upgrade before another worker starts upgrading.


1. Install the version of the [`kubectl cli` ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/tasks/tools/install-kubectl/) that matches the Kubernetes version of the Kubernetes master.

2. Make any changes that are marked _Update after master_ in [Kubernetes changes](cs_versions.html).

3. Update your worker nodes:
  * To update from the {{site.data.keyword.Bluemix_notm}} Dashboard, navigate to the `Worker Nodes` section of your cluster, and click `Update Worker`.
  * To get worker node IDs, run `bx cs workers <cluster_name_or_id>`. If you select multiple worker nodes, the worker nodes are updated one at a time.

    ```
    bx cs worker-update <cluster_name_or_id> <worker_node_id1> <worker_node_id2>
    ```
    {: pre}

4. Confirm that the update is complete:
  * Review the Kubernetes version on the {{site.data.keyword.Bluemix_notm}} Dashboard or run `bx cs workers <cluster_name_or_id>`.
  * Review the Kubernets version of the worker nodes by running `kubectl get nodes`.
  * In some cases, older clusters might list duplicate worker nodes with a **NotReady** status after an update. To remove duplicates, see [troubleshooting](cs_troubleshoot.html#cs_duplicate_nodes).

After you complete the update:
  - Repeat the update process with other clusters.
  - Inform developers who work in the cluster to update their `kubectl` CLI to the version of the Kubernetes master.
  - If the Kubernetes dashboard does not display utilization graphs, [delete the `kube-dashboard` pod](cs_troubleshoot.html#cs_dashboard_graphs).

<br />


## Adding subnets to clusters
{: #cs_cluster_subnet}

Change the pool of available portable public or private IP addresses by adding subnets to your cluster.
{:shortdesc}

In {{site.data.keyword.containershort_notm}}, you can add stable, portable IPs for Kubernetes services by adding network subnets to the cluster. In this case, subnets are not being used with netmasking to create connectivity across one or more clusters. Instead, the subnets are used to provide permanent fixed IPs for a service from a cluster that can be used to access that service.

When you create a standard cluster, {{site.data.keyword.containershort_notm}} automatically provisions a portable public subnet with 5 public IP addresses and a portable private subnet with 5 private IP addresses. Portable public and private IP addresses are static and do not change when a worker node, or even the cluster, is removed. For each subnet, one of the portable public and one of the portable private IP addresses are used for [application load balancers](cs_apps.html#cs_apps_public_ingress) that you can use to expose multiple apps in your cluster. The remaining 4 portable public and 4 portable private IP addresses can be used to expose single apps to the public by [creating a load balancer service](cs_apps.html#cs_apps_public_load_balancer).

**Note:** Portable public IP addresses are charged on a monthly basis. If you choose to remove portable public IP addresses after your cluster is provisioned, you still have to pay the monthly charge, even if you used them only for a short amount of time.

### Requesting additional subnets for your cluster
{: #add_subnet}

You can add stable, portable public or private IPs to the cluster by assigning subnets to the cluster.

Before you begin, make sure that you can access the IBM Cloud infrastructure (SoftLayer) portfolio through the {{site.data.keyword.Bluemix_notm}} GUI. To access the portfolio, you must set up or use an existing {{site.data.keyword.Bluemix_notm}} Pay-As-You-Go account.

1.  From the catalog, in the **Infrastructure** section, select **Network**.
2.  Select **Subnet/IPs** and click **Create**.
3.  From the **Select the type of subnet to add to this account** drop-down menu, select **Portable Public** or **Portable Private**.
4.  Select the number of IP addresses that you want to add from your portable subnet.

    **Note:** When you add portable IP addresses for your subnet, three IP addresses are used to establish cluster-internal networking, so that you cannot use them for your application load balancer or to create a load balancer service. For example, if you request eight portable public IP addresses, you can use five of them to expose your apps to the public.

5.  Select the public or private VLAN where you want to route the portable public or private IP addresses to. You must select the public or private VLAN that an existing worker node is connected to. Review the public or private VLAN for a worker node.

    ```
    bx cs worker-get <worker_id>
    ```
    {: pre}

6.  Complete the questionnaire and click **Place order**.

    **Note:** Portable public IP addresses are charged on a monthly basis. If you choose to remove portable public IP addresses after you created them, you still must pay the monthly charge, even if you used them only part of the month.

7.  After the subnet is provisioned, make the subnet available to your Kubernetes cluster.
    1.  From the Infrastructure dashboard, select the subnet that you created and note the ID of the subnet.
    2.  Log in to the {{site.data.keyword.Bluemix_notm}} CLI. To specify an {{site.data.keyword.Bluemix_notm}} region, [include the API endpoint](cs_regions.html#bluemix_regions).

        ```
        bx login
        ```
        {: pre}

        **Note:** If you have a federated ID, use `bx login --sso` to log in to the {{site.data.keyword.Bluemix_notm}} CLI. Enter your user name and use the provided URL in your CLI output to retrieve your one-time passcode. You know you have a federated ID when the login fails without the `--sso` and succeeds with the `--sso` option.

    3.  List all clusters in your account and note the ID of the cluster where you want to make your subnet available.

        ```
        bx cs clusters
        ```
        {: pre}

    4.  Add the subnet to your cluster. When you make a subnet available to a cluster, a Kubernetes config map is created for you that includes all available portable public or private IP addresses that you can use. If no application load balancer exists for your cluster, one portable public IP address is automatically used to create the public application load balancer and one portable private IP address is automatically used to create the private application load balancer. All other portable public and private IP addresses can be used to create load balancer services for your apps.

        ```
        bx cs cluster-subnet-add <cluster name or id> <subnet id>
        ```
        {: pre}

8.  Verify that the subnet was successfully added to your cluster. The subnet CIDR is listed in the **VLANs** section.

    ```
    bx cs cluster-get --showResources <cluster name or id>
    ```
    {: pre}

### Adding custom and existing subnets to Kubernetes clusters
{: #custom_subnet}

You can add existing portable public or private subnets to your Kubernetes cluster.

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
    Name         ID                                   State      Created          Workers
    my_cluster   paf97e8843e29941b49c598f516de72101   deployed   20170201162433   3
    ```
    {: screen}

5.  Check the status of the worker nodes.

    ```
    bx cs workers <cluster>
    ```
    {: pre}

    When the worker nodes are ready, the state changes to **normal** and the status is **Ready**. When the node status is **Ready**, you can then access the cluster.

    ```
    ID                                                  Public IP        Private IP     Machine Type   State      Status
    prod-dal10-pa8dfcc5223804439c87489886dbbc9c07-w1   169.47.223.113   10.171.42.93   free           normal    Ready
    ```
    {: screen}

6.  Add the subnet to your cluster by specifying the subnet ID. When you make a subnet available to a cluster, a Kubernetes config map is created for you that includes all available portable public IP addresses that you can use. If no application load balancers already exist for your cluster, one portable public and one portable private IP address is automatically used to create the public and private application load balancers. All other portable public and private IP addresses can be used to create load balancer services for your apps.

    ```
    bx cs cluster-subnet-add mycluster 807861
    ```
    {: pre}

### Adding user-managed subnets and IP addresses to Kubernetes clusters
{: #user_subnet}

Provide your own subnet from an on-premises network that you want {{site.data.keyword.containershort_notm}} to access. Then, you can add private IP addresses from that subnet to load balancer services in your Kubernetes cluster.

Requirements:
- User-managed subnets can be added to private VLANs only.
- The subnet prefix length limit is /24 to /30. For example, `203.0.113.0/24` specifies 253 usable private IP addresses, while `203.0.113.0/30` specifies 1 usable private IP address.
- The first IP address in the subnet must be used as the gateway for the subnet.

Before you begin: Configure the routing of network traffic into and out of the external subnet. In addition, confirm that you have VPN connectivity between the on-premises data center gateway device and the private network Vyatta in your IBM Cloud infrastructure (SoftLayer) portfolio. For more information see this [blog post ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/blogs/bluemix/2017/07/kubernetes-and-bluemix-container-based-workloads-part4/).

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

4. Add a private load balancer service or a private Ingress application load balancer to access your app over the private network. If you want to use a private IP address from the subnet that you added when you create a private load balancer or a private Ingress application load balancer, you must specify an IP address. Otherwise, an IP address is chosen at random from the IBM Cloud infrastructure (SoftLayer) subnets or user-provided subnets on the private VLAN. For more information, see [Configuring access to an app by using the load balancer service type](cs_apps.html#cs_apps_public_load_balancer) or [Enabling the private application load balancer](cs_apps.html#private_ingress).

<br />


## Using existing NFS file shares in clusters
{: #cs_cluster_volume_create}

If you already have existing NFS file shares in your IBM Cloud infrastructure (SoftLayer) account that you want to use with Kubernetes, you can do so by creating persistent volumes on your existing NFS file share. A persistent volume is a piece of actual hardware that serves as a Kubernetes cluster resource and can be consumed by the cluster user.
{:shortdesc}

Kubernetes differentiates between persistent volumes that represent the actual hardware and persistent volume claims that are requests for storage usually initiated by the cluster user. The following diagram illustrates the relationship between persistent volumes and persistent volume claims.

![Create persistent volumes and persistent volume claims](images/cs_cluster_pv_pvc.png)

 As depicted in the diagram, to enable existing NFS file shares to be used with Kubernetes, you must create persistent volumes with a certain size and access mode and create a persistent volume claim that matches the persistent volume specification. If persistent volume and persistent volume claim match, they are bound to each other. Only bound persistent volume claims can be used by the cluster user to mount the volume to a deployment. This process is referred to as static provisioning of persistent storage.

Before you begin, make sure that you have an existing NFS file share that you can use to create your persistent volume.

**Note:** Static provisioning of persistent storage only applies to existing NFS file shares. If you do not have existing NFS file shares, cluster users can use the [dynamic provisioning](cs_apps.html#cs_apps_volume_claim) process to add persistent volumes.

To create a persistent volume and matching persistent volume claim, follow these steps.

1.  In your IBM Cloud infrastructure (SoftLayer) account, look up the ID and path of the NFS file share where you want to create your persistent volume object. In addition, authorize the file storage to the subnets in the cluster. This authorization gives your cluster access to the storage.
    1.  Log in to your IBM Cloud infrastructure (SoftLayer) account.
    2.  Click **Storage**.
    3.  Click **File Storage** and from the **Actions** menu, select **Authorize Host**.
    4.  Click **Subnets**. After you authorize, every worker node on the subnet has access to the file storage.
    5.  Select the subnet of your cluster's public VLAN from the menu and click **Submit**. If you need to find the subnet, run `bx cs cluster-get <cluster_name> --showResources`.
    6.  Click the name of the file storage.
    7.  Make note the **Mount Point** field. The field is displayed as `<server>:/<path>`.
2.  Create a storage configuration file for your persistent volume. Include the server and path from the file storage **Mount Point** field.

    ```
    apiVersion: v1
    kind: PersistentVolume
    metadata:
     name: mypv
    spec:
     capacity:
       storage: "20Gi"
     accessModes:
       - ReadWriteMany
     nfs:
       server: "nfslon0410b-fz.service.networklayer.com"
       path: "/IBM01SEV8491247_0908"
    ```
    {: codeblock}

    <table>
    <caption>Table 9. Understanding the YAML file components</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the YAML file components</th>
    </thead>
    <tbody>
    <tr>
    <td><code>name</code></td>
    <td>Enter the name of the persistent volume object that you want to create.</td>
    </tr>
    <tr>
    <td><code>storage</code></td>
    <td>Enter the storage size of the existing NFS file share. The storage size must be written in gigabytes, for example, 20Gi (20 GB) or 1000Gi (1 TB), and the size must match the size of the existing file share.</td>
    </tr>
    <tr>
    <td><code>accessMode</code></td>
    <td>Access modes define the way that the persistent volume claim can be mounted to a worker node.<ul><li>ReadWriteOnce (RWO): The persistent volume can be mounted to deployments in a single worker node only. Containers in deployments that are mounted to this persistent volume can read from and write to the volume.</li><li>ReadOnlyMany (ROX): The persistent volume can be mounted to deployments that are hosted on multiple worker nodes. Deployments that are mounted to this persistent volume can only read from the volume.</li><li>ReadWriteMany (RWX): This persistent volume can be mounted to deployments that are hosted on multiple worker nodes. Deployments that are mounted to this persistent volume can read from and write to the volume.</li></ul></td>
    </tr>
    <tr>
    <td><code>server</code></td>
    <td>Enter the NFS file share server ID.</td>
    </tr>
    <tr>
    <td><code>path</code></td>
    <td>Enter the path to the NFS file share where you want to create the persistent volume object.</td>
    </tr>
    </tbody></table>

3.  Create the persistent volume object in your cluster.

    ```
    kubectl apply -f <yaml_path>
    ```
    {: pre}

    Example

    ```
    kubectl apply -f deploy/kube-config/pv.yaml
    ```
    {: pre}

4.  Verify that the persistent volume is created.

    ```
    kubectl get pv
    ```
    {: pre}

5.  Create another configuration file to create your persistent volume claim. In order for the persistent volume claim to match the persistent volume object that you created earlier, you must choose the same value for `storage` and `accessMode`. The `storage-class` field must be empty. If any of these fields do not match the persistent volume, then a new persistent volume is created automatically instead.

    ```
    kind: PersistentVolumeClaim
    apiVersion: v1
    metadata:
     name: mypvc
     annotations:
       volume.beta.kubernetes.io/storage-class: ""
    spec:
     accessModes:
       - ReadWriteMany
     resources:
       requests:
         storage: "20Gi"
    ```
    {: codeblock}

6.  Create your persistent volume claim.

    ```
    kubectl apply -f deploy/kube-config/mypvc.yaml
    ```
    {: pre}

7.  Verify that your persistent volume claim is created and bound to the persistent volume object. This process can take a few minutes.

    ```
    kubectl describe pvc mypvc
    ```
    {: pre}

    Your output looks similar to the following.

    ```
    Name: mypvc
    Namespace: default
    StorageClass:	""
    Status: Bound
    Volume: pvc-0d787071-3a67-11e7-aafc-eef80dd2dea2
    Labels: <none>
    Capacity: 20Gi
    Access Modes: RWX
    Events:
      FirstSeen LastSeen Count From        SubObjectPath Type Reason Message
      --------- -------- ----- ----        ------------- -------- ------ -------
      3m 3m 1 {ibm.io/ibmc-file 31898035-3011-11e7-a6a4-7a08779efd33 } Normal Provisioning External provisioner is provisioning volume for claim "default/my-persistent-volume-claim"
      3m 1m	 10 {persistentvolume-controller } Normal ExternalProvisioning cannot find provisioner "ibm.io/ibmc-file", expecting that a volume for the claim is provisioned either manually or via external software
      1m 1m 1 {ibm.io/ibmc-file 31898035-3011-11e7-a6a4-7a08779efd33 } Normal ProvisioningSucceeded	Successfully provisioned volume pvc-0d787071-3a67-11e7-aafc-eef80dd2dea2
    ```
    {: screen}


You successfully created a persistent volume object and bound it to a persistent volume claim. Cluster users can now [mount the persistent volume claim](cs_apps.html#cs_apps_volume_mount) to their deployments and start reading from and writing to the persistent volume object.

<br />


## Configuring cluster logging
{: #cs_logging}

Logs help you troubleshoot issues with your clusters and apps. Sometimes, you might want to send logs to a specific location for processing or long-term storage. On a Kubernetes cluster in {{site.data.keyword.containershort_notm}}, you can enable log forwarding for your cluster and choose where your logs are forwarded. **Note**: Log forwarding is supported only for standard clusters.
{:shortdesc}

You can forward logs for log sources such as containers, applications, worker nodes, Kubernetes clusters, and Ingress controllers. Review the following table for information about each log source.

|Log source|Characteristics|Log paths|
|----------|---------------|-----|
|`container`|Logs for your container that runs in a Kubernetes cluster.|-|
|`application`|Logs for your own application that runs in a Kubernetes cluster.|`/var/log/apps/**/*.log`, `/var/log/apps/**/*.err|
|`worker`|Logs for virtual machine worker nodes within a Kubernetes cluster.|`/var/log/syslog`, `/var/log/auth.log`|
|`kubernetes`|Logs for the Kubernetes system component.|`/var/log/kubelet.log`, `/var/log/kube-proxy.log`|
|`ingress`|Logs for an application load balancer, managed by the Ingress controller, that manages network traffic coming into a Kubernetes cluster.|`/var/log/alb/ids/*.log`, `/var/log/alb/ids/*.err`, `/var/log/alb/customerlogs/*.log`, `/var/log/alb/customerlogs/*.err`|
{: caption="Table 9. Log source characteristics." caption-side="top"}

#### Enabling log forwarding
{: #cs_log_sources_enable}

You can forward logs to {{site.data.keyword.loganalysislong_notm}} or to an external syslog server. If you want to forward logs from one log source to both log collector servers, then you must create two logging configurations. **Note**: To forward logs for applications, see [Enabling log forwarding for applications](#cs_apps_enable).
{:shortdesc}

Before you begin:

1. If you want to forward logs to an external syslog server, you can set up a server that accepts a syslog protocol in two ways:
  * Set up and manage your own server or have a provider manage it for you. If a provider manages the server for you, get the logging endpoint from the logging provider.
  * Run syslog from a container. For example, you can use this [deployment .yaml file ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/IBM-Bluemix/kube-samples/blob/master/deploy-apps-clusters/deploy-syslog-from-kube.yaml) to fetch a Docker public image that runs a container in a Kubernetes cluster. The image publishes the port `514` on the public cluster IP address, and uses this public cluster IP address to configure the syslog host.

2. [Target your CLI](cs_cli_install.html#cs_cli_configure) to the cluster where the log source is located.

To enable log forwarding for a container, worker node, Kubernetes system component, application, or Ingress application load balancer:

1. Create a log forwarding configuration.

  * To forward logs to {{site.data.keyword.loganalysislong_notm}}:

    ```
    bx cs logging-config-create <my_cluster> --logsource <my_log_source> --namespace <kubernetes_namespace> --hostname <ingestion_URL> --port <ingestion_port> --spaceName <cluster_space> --orgName <cluster_org> --type ibm
    ```
    {: pre}

    <table>
    <caption>Table 10. Understanding this command's components</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding this command's components</th>
    </thead>
    <tbody>
    <tr>
    <td><code>logging-config-create</code></td>
    <td>The command to create a {{site.data.keyword.loganalysislong_notm}} log forwarding configuration.</td>
    </tr>
    <tr>
    <td><code><em>&lt;my_cluster&gt;</em></code></td>
    <td>Replace <em>&lt;my_cluster&gt;</em> with the name or ID of the cluster.</td>
    </tr>
    <tr>
    <td><code>--logsource <em>&lt;my_log_source&gt;</em></code></td>
    <td>Replace <em>&lt;my_log_source&gt;</em> with the log source. Accepted values are <code>container</code>, <code>application</code>, <code>worker</code>, <code>kubernetes</code>, and <code>ingress</code>.</td>
    </tr>
    <tr>
    <td><code><em>&lt;kubernetes_namespace&gt;</em></code></td>
    <td>Replace <em>&lt;kubernetes_namespace&gt;</em> with the Docker container namespace that you want to forward logs from. Log forwarding is not supported for the <code>ibm-system</code> and <code>kube-system</code> Kubernetes namespaces. This value is valid only for the container log source and is optional. If you do not specify a namespace, then all namespaces in the container use this configuration.</td>
    </tr>
    <tr>
    <td><code>--hostname <em>&lt;ingestion_URL&gt;</em></code></td>
    <td>Replace <em>&lt;ingestion_URL&gt;</em> with the {{site.data.keyword.loganalysisshort_notm}} ingestion URL. You can find the list of available ingestion URLs [here](/docs/services/CloudLogAnalysis/log_ingestion.html#log_ingestion_urls). If you do not specify an ingestion URL, the endpoint for the region where your cluster was created is used.</td>
    </tr>
    <tr>
    <td><code>--port <em>&lt;ingestion_port&gt;</em></code></td>
    <td>Replace <em>&lt;ingestion_port&gt;</em> with the ingestion port. If you do not specify a port, then the standard port <code>9091</code> is used.</td>
    </tr>
    <tr>
    <td><code>--spaceName <em>&lt;cluster_space&gt;</em></code></td>
    <td>Replace <em>&lt;cluster_space&gt;</em> with the name of the space that you want to send logs to. If you do not specify a space, logs are sent to the account level.</td>
    </tr>
    <tr>
    <td><code>--orgName <em>&lt;cluster_org&gt;</em></code></td>
    <td>Replace <em>&lt;cluster_org&gt;</em> with the name of the org that the space is in. This value is required if you specified a space.</td>
    </tr>
    <tr>
    <td><code>--type ibm</code></td>
    <td>The log type for sending logs to {{site.data.keyword.loganalysisshort_notm}}.</td>
    </tr>
    </tbody></table>

  * To forward logs to an external syslog server:

    ```
    bx cs logging-config-create <my_cluster> --logsource <my_log_source> --namespace <kubernetes_namespace> --hostname <log_server_hostname> --port <log_server_port> --type syslog
    ```
    {: pre}

    <table>
    <caption>Table 11. Understanding this command's components</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding this command's components</th>
    </thead>
    <tbody>
    <tr>
    <td><code>logging-config-create</code></td>
    <td>The command to create a syslog log forwarding configuration.</td>
    </tr>
    <tr>
    <td><code><em>&lt;my_cluster&gt;</em></code></td>
    <td>Replace <em>&lt;my_cluster&gt;</em> with the name or ID of the cluster.</td>
    </tr>
    <tr>
    <td><code>--logsource <em>&lt;my_log_source&gt;</em></code></td>
    <td>Replace <em>&lt;my_log_source&gt;</em> with the log source. Accepted values are <code>container</code>, <code>application</code>, <code>worker</code>, <code>kubernetes</code>, and <code>ingress</code>.</td>
    </tr>
    <tr>
    <td><code><em>&lt;kubernetes_namespace&gt;</em></code></td>
    <td>Replace <em>&lt;kubernetes_namespace&gt;</em> with the Docker container namespace that you want to forward logs from. Log forwarding is not supported for the <code>ibm-system</code> and <code>kube-system</code> Kubernetes namespaces. This value is valid only for the container log source and is optional. If you do not specify a namespace, then all namespaces in the container use this configuration.</td>
    </tr>
    <tr>
    <td><code>--hostname <em>&lt;log_server_hostname&gt;</em></code></td>
    <td>Replace <em>&lt;log_server_hostname&gt;</em> with the hostname or IP address of the log collector server.</td>
    </tr>
    <tr>
    <td><code>--port <em>&lt;log_server_port&gt;</em></code></td>
    <td>Replace <em>&lt;log_server_port&gt;</em> with the port of the log collector server. If you do not specify a port, then the standard port <code>514</code> is used.</td>
    </tr>
    <tr>
    <td><code>--type syslog</code></td>
    <td>The log type for sending logs to an external syslog server.</td>
    </tr>
    </tbody></table>

2. Verify that the log forwarding configuration was created.

    * To list all the logging configurations in the cluster:
      ```
      bx cs logging-config-get <my_cluster>
      ```
      {: pre}

      Example output:

      ```
      Id                                    Source       Protocol   Namespace     Host                          Port   Paths                                         Org    Space     Account
      f4bc77c0-ee7d-422d-aabf-a4e6b977264e  kubernetes   syslog     -             172.30.162.138                5514   /var/log/kubelet.log,/var/log/kube-proxy.log  -        -         -
      5bd9c609-13c8-4c48-9d6e-3a6664c825a9  application  ibm        -             ingest.logging.ng.bluemix.net 9091   /var/log/apps/**/*.log,/var/log/apps/**/*.err my_org   my_space  example@ibm.com
      8a284f1a-451c-4c48-b1b4-a4e6b977264e  containers   syslog     my-namespace  myhostname.common             5514   -                                             -        -         -
      ```
      {: screen}

    * To list logging configurations for one type of log source:
      ```
      bx cs logging-config-get <my_cluster> --logsource worker
      ```
      {: pre}

      Example output:

      ```
      Id                                    Source    Protocol   Namespace   Host                          Port   Paths                              Org   Space  Account
      f4bc77c0-ee7d-422d-aabf-a4e6b977264e  worker    ibm        -           ingest.logging.ng.bluemix.net 9091   /var/log/syslog,/var/log/auth.log  -     -      example@ibm.com  
      5bd9c609-13c8-4c48-9d6e-3a6664c825a9  worker    syslog     -           172.30.162.138                5514   /var/log/syslog,/var/log/auth.log  -     -      -
      ```
      {: screen}

#### Enabling log forwarding for applications
{: #cs_apps_enable}

Logs from applications must be constrained to a specific directory on the host node. You can do this by mounting a host path volume to your containers with a mount path. This mount path serves as the directory on your containers where application logs are sent. The predefined host path directory, `/var/log/apps`, is automatically created when you create the volume mount.

Review the following aspects of application log forwarding:
* Logs are read recursively from the /var/log/apps path. This means that you can put application logs in subdirectories of the /var/log/apps path.
* Only application log files with `.log` or `.err` file extensions are forwarded.
* When you enable log forwarding for the first time, application logs are tailed instead of being read from head. This means that the contents of any logs already present before application logging was enabled are not read. The logs are read from the point that logging was enabled. However, after the first time that log forwarding is enabled, logs are always picked up from where they last left off.
* When you mount the `/var/log/apps` host path volume to containers, the containers all write to this same directory. This means that if your containers are writing to the same file name, the containers will write to the exact same file on the host. If this is not your intention, you can prevent your containers from overwriting the same log files by naming the log files from each container differently.
* Because all containers write to the same file name, do not use this method to forward application logs for ReplicaSets. Instead, you can write logs from the application to STDOUT and STDERR, which are picked up as container logs. To forward application logs written to STDOUT and STDERR, follow the steps in [Enabling log forwarding](cs_cluster.html#cs_log_sources_enable).

Before you start, [target your CLI](cs_cli_install.html#cs_cli_configure) to the cluster where the log source is located.

1. Open the `.yaml` configuration file for the application's pod.

2. Add the following `volumeMounts` and `volumes` to the configuration file:

    ```
    apiVersion: v1
    kind: Pod
    metadata:
      name: <pod_name>
    containers:
    - name: fluentd
      image: "<your_registry_image>"
      volumeMounts:
        # Docker paths
          - mountPath: /var/log/my-app-log-files-directory
            name: app-log
    volumes:
    - name: app-log
      hostPath:
        path: /var/log/apps
    ```
    {: codeblock}

2. Mount the volume to the pod.

    ```
    kubectl apply -f <local_yaml_path>
    ```
    {:pre}

3. To create a log forwarding configuration, follow the steps in [Enabling log forwarding](cs_cluster.html#cs_log_sources_enable).

#### Updating the log forwarding configuration
{: #cs_log_sources_update}

You can update a logging configuration for a container, application, worker node, Kubernetes system component, or Ingress application load balancer.
{: shortdesc}

Before you begin:

1. If you are changing the log collector server to syslog, you can set up a server that accepts a syslog protocol in two ways:
  * Set up and manage your own server or have a provider manage it for you. If a provider manages the server for you, get the logging endpoint from the logging provider.
  * Run syslog from a container. For example, you can use this [deployment .yaml file ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/IBM-Bluemix/kube-samples/blob/master/deploy-apps-clusters/deploy-syslog-from-kube.yaml) to fetch a Docker public image that runs a container in a Kubernetes cluster. The image publishes the port `514` on the public cluster IP address, and uses this public cluster IP address to configure the syslog host.

2. [Target your CLI](cs_cli_install.html#cs_cli_configure) to the cluster where the log source is located.

To change the details of a logging configuration:

1. Update the logging configuration.

    ```
    bx cs logging-config-update <my_cluster> <log_config_id> --logsource <my_log_source> --hostname <log_server_hostname> --port <log_server_port> --spaceName <cluster_space> --orgName <cluster_org> --type <logging_type>
    ```
    {: pre}

    <table>
    <caption>Table 12. Understanding this command's components</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding this command's components</th>
    </thead>
    <tbody>
    <tr>
    <td><code>logging-config-update</code></td>
    <td>The command to update the log forwarding configuration for your log source.</td>
    </tr>
    <tr>
    <td><code><em>&lt;my_cluster&gt;</em></code></td>
    <td>Replace <em>&lt;my_cluster&gt;</em> with the name or ID of the cluster.</td>
    </tr>
    <tr>
    <td><code><em>&lt;log_config_id&gt;</em></code></td>
    <td>Replace <em>&lt;log_config_id&gt;</em> with the ID of the log source configuration.</td>
    </tr>
    <tr>
    <td><code>--logsource <em>&lt;my_log_source&gt;</em></code></td>
    <td>Replace <em>&lt;my_log_source&gt;</em> with the log source. Accepted values are <code>container</code>, <code>application</code>, <code>worker</code>, <code>kubernetes</code>, and <code>ingress</code>.</td>
    </tr>
    <tr>
    <td><code>--hostname <em>&lt;log_server_hostname&gt;</em></code></td>
    <td>When the logging type is <code>syslog</code>, replace <em>&lt;log_server_hostname&gt;</em> with the hostname or IP address of the log collector server. When the logging type is <code>ibm</code>, replace <em>&lt;log_server_hostname&gt;</em> with the {{site.data.keyword.loganalysislong_notm}} ingestion URL. You can find the list of available ingestion URLs [here](/docs/services/CloudLogAnalysis/log_ingestion.html#log_ingestion_urls). If you do not specify an ingestion URL, the endpoint for the region where your cluster was created is used.</td>
    </tr>
    <tr>
    <td><code>--port <em>&lt;log_collector_port&gt;</em></code></td>
    <td>Replace <em>&lt;log_server_port&gt;</em> with the port of the log collector server. If you do not specify a port, then the standard port <code>514</code> is used for <code>syslog</code> and <code>9091</code> is used for <code>ibm</code>.</td>
    </tr>
    <td><code>--spaceName <em>&lt;cluster_space&gt;</em></code></td>
    <td>Replace <em>&lt;cluster_space&gt;</em> with the name of the space that you want to send logs to. This value is valid only for log type <code>ibm</code> and is optional. If you do not specify a space, logs are sent to the account level.</td>
    </tr>
    <tr>
    <td><code>--orgName <em>&lt;cluster_org&gt;</em></code></td>
    <td>Replace <em>&lt;cluster_org&gt;</em> with the name of the org that the space is in. This value is valid only for log type <code>ibm</code> and is required if you specified a space.</td>
    </tr>
    <tr>
    <td><code>--type <em>&lt;logging_type&gt;</em></code></td>
    <td>Replace <em>&lt;logging_type&gt;</em> with the new log forwarding protocol you want to use. Currently, <code>syslog</code> and <code>ibm</code> are supported.</td>
    </tr>
    </tbody></table>

2. Verify that the log forwarding configuration was updated.

    * To list all the logging configurations in the cluster:
    ```
    bx cs logging-config-get <my_cluster>
    ```
    {: pre}

    Example output:

    ```
    Id                                    Source       Protocol   Namespace     Host                          Port   Paths                                         Org    Space     Account
    f4bc77c0-ee7d-422d-aabf-a4e6b977264e  kubernetes   syslog     -             172.30.162.138                5514   /var/log/kubelet.log,/var/log/kube-proxy.log  -        -         -
    5bd9c609-13c8-4c48-9d6e-3a6664c825a9  application  ibm        -             ingest.logging.ng.bluemix.net 9091   /var/log/apps/**/*.log,/var/log/apps/**/*.err my_org   my_space  example@ibm.com
    8a284f1a-451c-4c48-b1b4-a4e6b977264e  containers   syslog     my-namespace  myhostname.common             5514   -                                             -        -         -
    ```
    {: screen}

    * To list logging configurations for one type of log source:
    ```
    bx cs logging-config-get <my_cluster> --logsource worker
    ```
    {: pre}

    Example output:

    ```
    Id                                    Source    Protocol   Namespace   Host                          Port   Paths                              Org   Space  Account
    f4bc77c0-ee7d-422d-aabf-a4e6b977264e  worker    ibm        -           ingest.logging.ng.bluemix.net 9091   /var/log/syslog,/var/log/auth.log  -     -      example@ibm.com  
    5bd9c609-13c8-4c48-9d6e-3a6664c825a9  worker    syslog     -           172.30.162.138                5514   /var/log/syslog,/var/log/auth.log  -     -      -
    ```
    {: screen}

#### Stopping log forwarding
{: #cs_log_sources_delete}

You can stop forwarding logs by deleting the logging configuration.

Before you begin, [target your CLI](cs_cli_install.html#cs_cli_configure) to your cluster where the log source is located.

1. Delete the logging configuration.

    ```
    bx cs logging-config-rm <my_cluster> <log_config_id>
    ```
    {: pre}
    Replace <em>&lt;my_cluster&gt;</em> with the name of the cluster that the logging configuration is in and <em>&lt;log_config_id&gt;</em> with the ID of the log source configuration.

### Configuring log forwarding for Kubernetes API audit logs
{: #cs_configure_api_audit_logs}

Kubernetes API audit logs capture any calls to the Kubernetes API server from your cluster. To start collecting Kubernetes API audit logs, you can configure the Kubernetes API server to set up a webhook backend for your cluster. This webhook backend enables logs to be sent to a remote server. For more information about Kubernetes audit logs, see the <a href="https://kubernetes.io/docs/tasks/debug-application-cluster/audit/" target="_blank">auditing topic <img src="../icons/launch-glyph.svg" alt="External link icon"></a> in the Kubernetes documentation.

**Note**:
* Forwarding for Kubernetes API audit logs is only supported for Kubernetes version 1.7 and newer.
* Currently, a default audit policy is used for all clusters with this logging configuration.

#### Enabling Kubernetes API audit log forwarding
{: #cs_audit_enable}

Before you begin, [target your CLI](cs_cli_install.html#cs_cli_configure) to the cluster that you want to collect API server audit logs from.

1. Set the webhook backend for the API server configuration. A webhook configuration is created based on the information you provide in this command's flags. If you do not provide any information in the flags, a default webhook configuration is used.

    ```
    bx cs apiserver-config-set audit-webhook <my_cluster> --remoteServer <server_URL> --caCert <CA_cert_path> --clientCert <client_cert_path> --clientKey <client_key_path>
    ```
    {: pre}

    <table>
    <caption>Table 13. Understanding this command's components</caption>
    <thead>
    <th colspan=2><img src="images/idea.png"/> Understanding this command's components</th>
    </thead>
    <tbody>
    <tr>
    <td><code>apiserver-config-set</code></td>
    <td>The command to set an option for the cluster's Kubernetes API server configuration.</td>
    </tr>
    <tr>
    <td><code>audit-webhook</code></td>
    <td>The subcommand to set the audit webhook configuration for the cluster's Kubernetes API server.</td>
    </tr>
    <tr>
    <td><code><em>&lt;my_cluster&gt;</em></code></td>
    <td>Replace <em>&lt;my_cluster&gt;</em> with the name or ID of the cluster.</td>
    </tr>
    <tr>
    <td><code>--remoteServer <em>&lt;server_URL&gt;</em></code></td>
    <td>Replace <em>&lt;server_URL&gt;</em> with the URL for the remote logging service you want to send logs to. If you provide an insecure serverURL, any certificates are ignored. If you do not specify a remote server URL, a default QRadar configuration is used and the logs are sent to the QRadar instance for the region the cluster is in.</td>
    </tr>
    <tr>
    <td><code>--caCert <em>&lt;CA_cert_path&gt;</em></code></td>
    <td>Replace <em>&lt;CA_cert_path&gt;</em> with the filepath for the CA certificate that is used to verify the remote logging service.</td>
    </tr>
    <tr>
    <td><code>--clientCert <em>&lt;client_cert_path&gt;</em></code></td>
    <td>Replace <em>&lt;client_cert_path&gt;</em> with the filepath for the client certificate that is used to authenticate against the remote logging service.</td>
    </tr>
    <tr>
    <td><code>--clientKey <em>&lt;client_key_path&gt;</em></code></td>
    <td>Replace <em>&lt;client_key_path&gt;</em> with the filepath for the corresponding client key that is used to connect to the remote logging service.</td>
    </tr>
    </tbody></table>

2. Verify that the log forwarding was enabled by viewing the URL for the remote logging service.

    ```
    bx cs apiserver-config-get audit-webhook <my_cluster>
    ```
    {: pre}

    Example output:
    ```
    OK
    Server:			https://8.8.8.8
    ```
    {: screen}

3. Apply the configuration update by restarting the Kubernetes master.

    ```
    bx cs apiserver-refresh <my_cluster>
    ```
    {: pre}

#### Stopping Kubernetes API audit log forwarding
{: #cs_audit_delete}

You can stop forwarding audit logs by disabling the webhook backend configuration for the cluster's API server.

Before you begin, [target your CLI](cs_cli_install.html#cs_cli_configure) to the cluster that you want to stop collecting API server audit logs from.

1. Disable the webhook backend configuration for the cluster's API server.

    ```
    bx cs apiserver-config-unset audit-webhook <my_cluster>
    ```
    {: pre}

2. Apply the configuration update by restarting the Kubernetes master.

    ```
    {[bx cs]} apiserver-refresh <my_cluster>
    ```
    {: pre}

### Viewing logs
{: #cs_view_logs}

To view logs for clusters and containers, you can use the standard Kubernetes and Docker logging features.
{:shortdesc}

#### {{site.data.keyword.loganalysislong_notm}}
{: #cs_view_logs_k8s}

For standard clusters, logs are located in the {{site.data.keyword.Bluemix_notm}} account you were logged in to when you created the Kubernetes cluster. If you specified an {{site.data.keyword.Bluemix_notm}} space when you created the cluster or when you created the logging configuration, then logs are located in that space. Logs are monitored and forwarded outside of the container. You can access logs for a container using the Kibana dashboard. For more information about logging, see [Logging for the {{site.data.keyword.containershort_notm}}](/docs/services/CloudLogAnalysis/containers/containers_kubernetes.html#containers_kubernetes).

**Note**: If you specified a space when you created the cluster or the logging configuration, then the account owner needs Manager, Developer, or Auditor permissions to that space to view logs. For more information about changing {{site.data.keyword.containershort_notm}} access policies and permissions, see [Managing cluster access](cs_cluster.html#cs_cluster_user). Once permissions are changed, it can take up to 24 hours for logs to start appearing.

To access the Kibana dashboard, go to one of the following URLs and select the {{site.data.keyword.Bluemix_notm}} account or space where you created the cluster.
- US-South and US-East: https://logging.ng.bluemix.net
- UK-South and EU-Central: https://logging.eu-fra.bluemix.net
- AP-South: https://logging.au-syd.bluemix.net

For more information about viewing logs, see [Navigating to Kibana from a web browser](/docs/services/CloudLogAnalysis/kibana/launch.html#launch_Kibana_from_browser).

#### Docker logs
{: #cs_view_logs_docker}

You can leverage the built-in Docker logging capabilities to review activities on the standard STDOUT and STDERR output streams. For more information, see [Viewing container logs for a container that runs in a Kubernetes cluster](/docs/services/CloudLogAnalysis/containers/containers_kubernetes.html#containers_kubernetes).

<br />


## Configuring cluster monitoring
{: #cs_monitoring}

Metrics help you monitor the health and performance of your clusters. You can configure health monitoring for worker nodes to automatically detect and correct any workers that enter a degraded or nonoperational state. **Note**: Monitoring is supported only for standard clusters.
{:shortdesc}

### Viewing metrics
{: #cs_view_metrics}

You can use the standard Kubernetes and Docker features to monitor the health of your clusters and apps.
{:shortdesc}

<dl>
<dt>Cluster details page in {{site.data.keyword.Bluemix_notm}}</dt>
<dd>{{site.data.keyword.containershort_notm}} provides information about the health and capacity of your cluster and the usage of your cluster resources. You can use this GUI to scale out your cluster, work with your persistent storage, and add more capabilities to your cluster through {{site.data.keyword.Bluemix_notm}} service binding. To view the cluster details page, go to your **{{site.data.keyword.Bluemix_notm}} Dashboard** and select a cluster.</dd>
<dt>Kubernetes dashboard</dt>
<dd>The Kubernetes dashboard is an administrative web interface where you can review the health of your worker nodes, find Kubernetes resources, deploy containerized apps, and troubleshoot apps using logging and monitoring information. For more information about how to access your Kubernetes dashboard, see [Launching the Kubernetes dashboard for {{site.data.keyword.containershort_notm}}](cs_apps.html#cs_cli_dashboard).</dd>
<dt>{{site.data.keyword.monitoringlong_notm}}</dt>
<dd>Metrics for standard clusters are located in the {{site.data.keyword.Bluemix_notm}} account that was logged in to when the Kubernetes cluster was created. If you specified an {{site.data.keyword.Bluemix_notm}} space when you created the cluster, then metrics are located in that space. Container metrics are collected automatically for all containers that are deployed in a cluster. These metrics are sent and are made available through Grafana. For more information about metrics, see [Monitoring for the {{site.data.keyword.containershort_notm}}](/docs/services/cloud-monitoring/containers/monitoring_containers_ov.html#monitoring_bmx_containers_ov).<p>To access the Grafana dashboard, go to one of the following URLs and select the {{site.data.keyword.Bluemix_notm}} account or space where you created the cluster.<ul><li>US-South and US-East: https://metrics.ng.bluemix.net</li><li>UK-South: https://metrics.eu-gb.bluemix.net</li></ul></p></dd></dl>

#### Other health monitoring tools
{: #cs_health_tools}

You can configure other tools for more monitoring capabilities.
<dl>
<dt>Prometheus</dt>
<dd>Prometheus is an open source monitoring, logging, and alerting tool that was designed for Kubernetes. The tool retrieves detailed information about the cluster, worker nodes, and deployment health based on the Kubernetes logging information. For setup information, see [Integrating services with {{site.data.keyword.containershort_notm}}](cs_planning.html#cs_planning_integrations).</dd>
</dl>

### Configuring health monitoring for worker nodes with Autorecovery
{: #cs_configure_worker_monitoring}

The {{site.data.keyword.containerlong_notm}} Autorecovery system can be deployed into existing clusters of Kubernetes version 1.7 or later. The Autorecovery system uses various checks to query worker node health status. If Autorecovery detects an unhealthy worker node based on the configured checks, Autorecovery triggers a corrective action like an OS reload on the worker node. Only one worker node undergoes a corrective action at a time. The worker node must successfully complete the corrective action before any other worker node undergoes a corrective action. For more information, see this [Autorecovery blog post ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/blogs/bluemix/2017/12/autorecovery-utilizes-consistent-hashing-high-availability/).
**NOTE**: Autorecovery requires at least one healthy node to function properly. Configure Autorecovery with active checks only in clusters with two or more worker nodes.

Before you begin, [target your CLI](cs_cli_install.html#cs_cli_configure) to the cluster where you want to check worker node statuses.

1. Create a configuration map file that defines your checks in JSON format. For example, the following YAML file defines three checks: an HTTP check and two Kubernetes API server checks.

    ```
    kind: ConfigMap
    apiVersion: v1
    metadata:
      name: ibm-worker-recovery-checks
      namespace: kube-system
    data:
      checkhttp.json: |
        {
          "Check":"HTTP",
          "FailureThreshold":3,
          "CorrectiveAction":"REBOOT",
          "CooloffSeconds":1800,
          "IntervalSeconds":180,
          "TimeoutSeconds":10,
          "Port":80,
          "ExpectedStatus":200,
          "Route":"/myhealth",
          "Enabled":false
        }
      checknode.json: |
        {
          "Check":"KUBEAPI",
          "Resource":"NODE",
          "FailureThreshold":3,
          "CorrectiveAction":"RELOAD",
          "CooloffSeconds":1800,
          "IntervalSeconds":180,
          "TimeoutSeconds":10,
          "Enabled":false
        }
      checkpod.json: |
        {
          "Check":"KUBEAPI",
          "Resource":"POD",
          "PodFailureThresholdPercent":50,
          "FailureThreshold":3,
          "CorrectiveAction":"REBOOT",
          "CooloffSeconds":1800,
          "IntervalSeconds":180,
          "TimeoutSeconds":10,
          "Enabled":false
      }
    ```
    {:codeblock}

    <table>
    <caption>Table 14. Understanding the YAML file components</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the YAML file components</th>
    </thead>
    <tbody>
    <tr>
    <td><code>name</code></td>
    <td>The configuration name <code>ibm-worker-recovery-checks</code> is a constant and cannot be changed.</td>
    </tr>
    <tr>
    <td><code>namespace</code></td>
    <td>The <code>kube-system</code> namespace is a constant and cannot be changed.</td>
    </tr>
    <tr>
    <tr>
    <td><code>Check</code></td>
    <td>Enter the type of check that you want Autorecovery to use. <ul><li><code>HTTP</code>: Autorecovery calls HTTP servers that run on each node to determine whether the nodes are running properly.</li><li><code>KUBEAPI</code>: Autorecovery calls the Kubernetes API server and reads the health status data reported by the worker nodes.</li></ul></td>
    </tr>
    <tr>
    <td><code>Resource</code></td>
    <td>When the check type is <code>KUBEAPI</code>, enter the type of resource that you want Autorecovery to check. Accepted values are <code>NODE</code> or <code>PODS</code>.</td>
    <tr>
    <td><code>FailureThreshold</code></td>
    <td>Enter the threshold for the number of consecutive failed checks. When this threshold is met, Autorecovery triggers the specified corrective action. For example, if the value is 3 and Autorecovery fails a configured check three consecutive times, Autorecovery triggers the corrective action that is associated with the check.</td>
    </tr>
    <tr>
    <td><code>PodFailureThresholdPercent</code></td>
    <td>When the resource type is <code>PODS</code>, enter the threshold for the percentage of pods on a worker node that can be in a [NotReady ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/#define-readiness-probes) state. This percentage is based on the total number of pods that are scheduled to a worker node. When a check determines that the percentage of unhealthy pods is greater than the threshold, the check counts as one failure.</td>
    </tr>
    <tr>
    <td><code>CorrectiveAction</code></td>
    <td>Enter the action to run when the failure threshold is met. A corrective action runs only while no other workers are being repaired and when this worker node is not in a cool-off period from a previous action. <ul><li><code>REBOOT</code>: Reboots the worker node.</li><li><code>RELOAD</code>: Reloads all of the necessary configurations for the worker node from a clean OS.</li></ul></td>
    </tr>
    <tr>
    <td><code>CooloffSeconds</code></td>
    <td>Enter the number of seconds Autorecovery must wait to issue another corrective action for a node that was already issued a corrective action. The cooloff period starts at the time a corrective action is issued.</td>
    </tr>
    <tr>
    <td><code>IntervalSeconds</code></td>
    <td>Enter the number of seconds in between consecutive checks. For example, if the value is 180, Autorecovery runs the check on each node every 3 minutes.</td>
    </tr>
    <tr>
    <td><code>TimeoutSeconds</code></td>
    <td>Enter the maximum number of seconds that a check call to the database takes before Autorecovery terminates the call operation. The value for <code>TimeoutSeconds</code> must be less than the value for <code>IntervalSeconds</code>.</td>
    </tr>
    <tr>
    <td><code>Port</code></td>
    <td>When the check type is <code>HTTP</code>, enter the port that the HTTP server must bind to on the worker nodes. This port must be exposed on the IP of every worker node in the cluster. Autorecovery requires a constant port number across all nodes for checking servers. Use [DaemonSets ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/)when deploying a custom server into a cluster.</td>
    </tr>
    <tr>
    <td><code>ExpectedStatus</code></td>
    <td>When the check type is <code>HTTP</code>, enter the HTTP server status that you expect to be returned from the check. For example, a value of 200 indicates that you expect an <code>OK</code> response from the server.</td>
    </tr>
    <tr>
    <td><code>Route</code></td>
    <td>When the check type is <code>HTTP</code>, enter the path that is requested from the HTTP server. This value is typically the metrics path for the server that is running on all of the worker nodes.</td>
    </tr>
    <tr>
    <td><code>Enabled</code></td>
    <td>Enter <code>true</code> to enable the check or <code>false</code> to disable the check.</td>
    </tr>
    </tbody></table>

2. Create the configuration map in your cluster.

    ```
    kubectl apply -f <my_file.yaml>
    ```
    {: pre}

3. Verify that you created the configuration map with the name `ibm-worker-recovery-checks` in the `kube-system` namespace with the proper checks.

    ```
    kubectl -n kube-system get cm ibm-worker-recovery-checks -o yaml
    ```
    {: pre}

4. Ensure that you created a Docker pull secret with the name `international-registry-docker-secret` in the `kube-system` namespace. Autorecovery is hosted in {{site.data.keyword.registryshort_notm}}'s international Docker registry. If you have not created a Docker registry secret that contains valid credentials for the international registry, create one to run the Autorecovery system.

    1. Install the {{site.data.keyword.registryshort_notm}} plug-in.

        ```
        bx plugin install container-registry -r Bluemix
        ```
        {: pre}

    2. Target the international registry.

        ```
        bx cr region-set international
        ```
        {: pre}

    3. Create an international registry token.

        ```
        bx cr token-add --non-expiring --description internationalRegistryToken
        ```
        {: pre}

    4. Set the `INTERNATIONAL_REGISTRY_TOKEN` environment variable to the token you created.

        ```
        INTERNATIONAL_REGISTRY_TOKEN=$(bx cr token-get $(bx cr tokens | grep internationalRegistryToken | awk '{print $1}') -q)
        ```
        {: pre}

    5. Set the `DOCKER_EMAIL` environment variable to the current user. You email address is needed only to run the `kubectl` command in the next step.

        ```
        DOCKER_EMAIL=$(bx target | grep "User" | awk '{print $2}')
        ```
        {: pre}

    6. Create the Docker pull secret.

        ```
        kubectl -n kube-system create secret docker-registry international-registry-docker-secret --docker-username=token --docker-password="$INTERNATIONAL_REGISTRY_TOKEN" --docker-server=registry.bluemix.net --docker-email="$DOCKER_EMAIL"
        ```
        {: pre}

5. Deploy Autorecovery into your cluster by applying this YAML file.

   ```
   kubectl apply -f https://raw.githubusercontent.com/IBM-Bluemix/kube-samples/master/ibm-worker-recovery/ibm-worker-recovery.yml
   ```
   {: pre}

6. After a few minutes, you can check the `Events` section in the output of the following command to see activity on the Autorecovery deployment.

    ```
    kubectl -n kube-system describe deployment ibm-worker-recovery
    ```
    {: pre}

<br />


## Visualizing Kubernetes cluster resources
{: #cs_weavescope}

Weave Scope provides a visual diagram of your resources within a Kubernetes cluster, including services, pods, containers, processes, nodes, and more. Weave Scope provides interactive metrics for CPU and memory and also provides tools to tail and exec into a container.
{:shortdesc}

Before you begin:

-   Remember not to expose your cluster information on the public internet. Complete these steps to deploy Weave Scope securely and access it from a web browser locally.
-   If you do not have one already, [create a standard cluster](#cs_cluster_ui). Weave Scope can be CPU intensive, especially the app. Run Weave Scope with larger standard clusters, not lite clusters.
-   [Target your CLI](cs_cli_install.html#cs_cli_configure) to your cluster to run `kubectl` commands.


To use Weave Scope with a cluster:
2.  Deploy one of the provided RBAC permissions configuration file in the cluster.

    To enable read/write permissions:

    ```
    kubectl apply -f "https://raw.githubusercontent.com/IBM-{{site.data.keyword.Bluemix_notm}}/kube-samples/master/weave-scope/weave-scope-rbac.yaml"
    ```
    {: pre}

    To enable read-only permissions:

    ```
    kubectl apply -f "https://raw.githubusercontent.com/IBM-{{site.data.keyword.Bluemix_notm}}/kube-samples/master/weave-scope/weave-scope-rbac-readonly.yaml"
    ```
    {: pre}

    Output:

    ```
    clusterrole "weave-scope-mgr" created
    clusterrolebinding "weave-scope-mgr-role-binding" created
    ```
    {: screen}

3.  Deploy the Weave Scope service, which is privately accessible by the cluster IP address.

    <pre class="pre">
    <code>kubectl apply --namespace kube-system -f "https://cloud.weave.works/k8s/scope.yaml?k8s-version=$(kubectl version | base64 | tr -d '&bsol;n')"</code>
    </pre>

    Output:

    ```
    serviceaccount "weave-scope" created
    deployment "weave-scope-app" created
    service "weave-scope-app" created
    daemonset "weave-scope-agent" created
    ```
    {: screen}

4.  Run a port forwarding command to bring up the service on your computer. Now that Weave Scope is configured with the cluster, to access Weave Scope next time, you can run this port forwarding command without completing the previous configuration steps again.

    ```
    kubectl port-forward -n kube-system "$(kubectl get -n kube-system pod --selector=weave-scope-component=app -o jsonpath='{.items..metadata.name}')" 4040
    ```
    {: pre}

    Output:

    ```
    Forwarding from 127.0.0.1:4040 -> 4040
    Forwarding from [::1]:4040 -> 4040
    Handling connection for 4040
    ```
    {: screen}

5.  Open your web browser to `http://localhost:4040`. Without the default components deployed, you see the following diagram. You can choose to view topology diagrams or tables of the Kubernetes resources in the cluster.

     <img src="images/weave_scope.png" alt="Example topology from Weave Scope" style="width:357px;" />


[Learn more about the Weave Scope features ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.weave.works/docs/scope/latest/features/).

<br />


## Removing clusters
{: #cs_cluster_remove}

When you are finished with a cluster, you can remove it so that the cluster is no longer consuming resources.
{:shortdesc}

Lite and standard clusters created with a Pay-As-You-Go account must be removed manually by the user when they are not needed anymore.

When you delete a cluster, you are also deleting resources on the cluster, including containers, pods, bound services, and secrets. If you do not delete your storage when you delete your cluster, you can delete your storage through the IBM Cloud infrastructure (SoftLayer) dashboard in the {{site.data.keyword.Bluemix_notm}} GUI. Due to the monthly billing cycle, a persistent volume claim cannot be deleted on the last day of a month. If you delete the persistent volume claim on the last day of the month, the deletion remains pending until the beginning of the next month.

**Warning:** No backups are created of your cluster or your data in your persistent storage. Deleting a cluster is permanent and cannot be undone.

-   From the {{site.data.keyword.Bluemix_notm}} GUI
    1.  Select your cluster and click **Delete** from the **More actions...** menu.
-   From the {{site.data.keyword.Bluemix_notm}} CLI
    1.  List the available clusters.

        ```
        bx cs clusters
        ```
        {: pre}

    2.  Delete the cluster.

        ```
        bx cs cluster-rm my_cluster
        ```
        {: pre}

    3.  Follow the prompts and choose whether to delete cluster resources.

When you remove a cluster, you can choose to remove the portable subnets and persistent storage associated with it:
- Subnets are used to assign portable public IP addresses to load balancer services or your Ingress controller. If you keep them, you can reuse them in a new cluster or manually delete them later from your IBM Cloud infrastructure (SoftLayer) portfolio.
- If you created a persistent volume claim by using an (existing file share)[#cs_cluster_volume_create], then you cannot delete the file share when you delete the cluster. You must manually delete the file share later from your IBM Cloud infrastructure (SoftLayer) portfolio.
- Persistent storage provides high availability for your data. If you delete it, you cannot recover your data.
