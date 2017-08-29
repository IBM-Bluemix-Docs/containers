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


# Setting up the CLI and API
{: #cs_cli_install}

You can use the {{site.data.keyword.containershort_notm}} CLI or API to create and manage your Kubernetes clusters.
{:shortdesc}

## Installing the CLI
{: #cs_cli_install_steps}

Install the required CLIs to create and manage your Kubernetes clusters in {{site.data.keyword.containershort_notm}}, and to deploy containerized apps to your cluster.
{:shortdesc}

This task includes the information for installing these CLIs and plug-ins:

-   {{site.data.keyword.Bluemix_notm}} CLI version 0.5.0 or later
-   {{site.data.keyword.containershort_notm}} plug-in
-   Kubernetes CLI version 1.5.6 or later
-   Optional: {{site.data.keyword.registryshort_notm}} plug-in
-   Optional: Docker version 1.9. or later

<br>
To install the CLIs:

1.  As a prerequisite for the {{site.data.keyword.containershort_notm}} plug-in, install the [{{site.data.keyword.Bluemix_notm}} CLI ![External link icon](../icons/launch-glyph.svg "External link icon")](https://clis.ng.bluemix.net/ui/home.html). The prefix for running commands by using the {{site.data.keyword.Bluemix_notm}} CLI is `bx`.

2.  Log in to the {{site.data.keyword.Bluemix_notm}} CLI. Enter your {{site.data.keyword.Bluemix_notm}} credentials when prompted.

    ```
    bx login
    ```
    {: pre}

    **Note:** If you have a federated ID, use `bx login --sso` to log in to the {{site.data.keyword.Bluemix_notm}} CLI. Enter your user name and use the provided URL in your CLI output to retrieve your one-time passcode. You know you have a federated ID when the login fails without the `--sso` and succeeds with the `--sso` option.



4.  To create Kubernetes clusters and manage worker nodes, install the {{site.data.keyword.containershort_notm}} plug-in. The prefix for running commands by using the {{site.data.keyword.containershort_notm}} plug-in is `bx cs`.

    ```
    bx plugin install container-service -r {{site.data.keyword.Bluemix_notm}}
    ```
    {: pre}

    To verify that the plug-in is installed properly, run the following command:

    ```
    bx plugin list
    ```
    {: pre}

    The {{site.data.keyword.containershort_notm}} plug-in is displayed in the results as container-service.

5.  To view a local version of the Kubernetes dashboard and to deploy apps into your clusters, [install the Kubernetes CLI ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/tasks/tools/install-kubectl/). The prefix for running commands by using the Kubernetes CLI is `kubectl`.

    1.  Download the Kubernetes CLI.

        OS X:   [https://storage.googleapis.com/kubernetes-release/release/v1.5.6/bin/darwin/amd64/kubectl ![External link icon](../icons/launch-glyph.svg "External link icon")](https://storage.googleapis.com/kubernetes-release/release/v1.5.6/bin/darwin/amd64/kubectl)

        Linux:   [https://storage.googleapis.com/kubernetes-release/release/v1.5.6/bin/linux/amd64/kubectl ![External link icon](../icons/launch-glyph.svg "External link icon")](https://storage.googleapis.com/kubernetes-release/release/v1.5.6/bin/linux/amd64/kubectl)

        Windows:   [https://storage.googleapis.com/kubernetes-release/release/v1.5.6/bin/windows/amd64/kubectl.exe ![External link icon](../icons/launch-glyph.svg "External link icon")](https://storage.googleapis.com/kubernetes-release/release/v1.5.6/bin/windows/amd64/kubectl.exe)

        **Tip:** If you are using Windows, install the Kubernetes CLI in the same directory as the {{site.data.keyword.Bluemix_notm}} CLI. This setup saves you some filepath changes when you run commands later.

    2.  For OSX and Linux users, complete the following steps.
        1.  Move the executable file to the `/usr/local/bin` directory.

            ```
            mv /<path_to_file>/kubectl/usr/local/bin/kubectl
            ```
            {: pre}

        2.  Make sure that `/usr/local/bin` is listed in your `PATH` system variable. The `PATH` variable contains all directories where your operating system can find executable files. The directories that are listed in the `PATH` variable serve different purposes. `/usr/local/bin` is used to store executable files for software that is not part of the operating system and that was manually installed by the system administrator.

            ```
            echo $PATH
            ```
            {: pre}

            Example CLI output:

            ```
            /usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin
            ```
            {: screen}

        3.  Convert the binary file to an executable.

            ```
            chmod +x /usr/local/bin/kubectl
            ```
            {: pre}

6.  To manage a private image repository, install the {{site.data.keyword.registryshort_notm}} plug-in. Use this plug-in to set up your own namespace in a multi-tenant, highly available, and scalable private image registry that is hosted by IBM, and to store and share Docker images with other users. Docker images are required to deploy containers into a cluster. The prefix for running registry commands is `bx cr`.

    ```
    bx plugin install container-registry -r {{site.data.keyword.Bluemix_notm}}
    ```
    {: pre}

    To verify that the plug-in is installed properly, run the following command:

    ```
    bx plugin list
    ```
    {: pre}

    The plug-in is displayed in the results as container-registry.

7.  To build images locally and push them to your registry namespace, [install Docker ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.docker.com/community-edition#/download). If you are using Windows 8 or earlier, you can install the [Docker Toolbox ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.docker.com/products/docker-toolbox) instead. The Docker CLI is used to build apps into images. The prefix for running commands by using the Docker CLI is `docker`.

Next, start [Creating Kubernetes clusters from the CLI with {{site.data.keyword.containershort_notm}}](cs_cluster.html#cs_cluster_cli).

For reference information about these CLIs, see the documentation for those tools.

-   [`bx` commands](/docs/cli/reference/bluemix_cli/bx_cli.html)
-   [`bx cs` commands](cs_cli_reference.html#cs_cli_reference)
-   [`kubectl` commands ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/user-guide/kubectl/v1.5/)
-   [`bx cr` commands](/docs/cli/plugins/registry/index.html#containerregcli)

## Configuring the CLI to run `kubectl`
{: #cs_cli_configure}

You can use the commands that are provided with the Kubernetes CLI to manage clusters in {{site.data.keyword.Bluemix_notm}}. All `kubectl` commands that are available in Kubernetes 1.5.6 are supported for use with clusters in {{site.data.keyword.Bluemix_notm}}. After you create a cluster, set the context for your local CLI to that cluster with an environment variable. Then, you can run the Kubernetes `kubectl` commands to work with your cluster in {{site.data.keyword.Bluemix_notm}}.
{:shortdesc}

Before you can run `kubectl` commands, [install the required CLIs](#cs_cli_install) and [create a cluster](cs_cluster.html#cs_cluster_cli).

1.  Log in to the {{site.data.keyword.Bluemix_notm}} CLI. Enter your {{site.data.keyword.Bluemix_notm}} credentials when prompted.

    ```
    bx login
    ```
    {: pre}

    To specify a specific {{site.data.keyword.Bluemix_notm}} region, include the API endpoint. If you have private Docker images that are stored in the container registry of a specific {{site.data.keyword.Bluemix_notm}} region, or {{site.data.keyword.Bluemix_notm}} services instances that you already created, log in to this region to access your images and {{site.data.keyword.Bluemix_notm}} services.

    The {{site.data.keyword.Bluemix_notm}} region that you log in to also determines the region where you can create your Kubernetes clusters, including the available datacenters. If you do not specify a region, you are automatically logged in to the region that is closest to you.
      -   US South

          ```
          bx login -a api.ng.bluemix.net
          ```
          {: pre}

      -   Sydney

          ```
          bx login -a api.au-syd.bluemix.net
          ```
          {: pre}

      -   Germany

          ```
          bx login -a api.eu-de.bluemix.net
          ```
          {: pre}

      -   United Kingdom

          ```
          bx login -a api.eu-gb.bluemix.net
          ```
          {: pre}

    **Note:** If you have a federated ID, use `bx login --sso` to log in to the {{site.data.keyword.Bluemix_notm}} CLI. Enter your user name and use the provided URL in your CLI output to retrieve your one-time passcode. You know you have a federated ID when the login fails without the `--sso` and succeeds with the `--sso` option.

2.  Select a {{site.data.keyword.Bluemix_notm}} account. If you are assigned to multiple {{site.data.keyword.Bluemix_notm}} organizations, select the organization where the cluster was created. Clusters are specific to an organization, but are independent from a {{site.data.keyword.Bluemix_notm}} space. Therefore, you are not required to select a space.

3.  If you want to create or access Kubernetes clusters in a region other than the {{site.data.keyword.Bluemix_notm}} region that you selected earlier, specify this region. For example, you might want to log in to another {{site.data.keyword.containershort_notm}} region for the following reasons:
   -   You created {{site.data.keyword.Bluemix_notm}} services or private Docker images in one region and want to use them with {{site.data.keyword.containershort_notm}} in another region.
   -   You want to access a cluster in a region that is different from the default {{site.data.keyword.Bluemix_notm}} region you are logged in to.<br>
   Choose among the following API endpoints:
        - US-South:
          ```
          bx cs init --host https://us-south.containers.bluemix.net
          ```
          {: pre}

        - UK-South:
          ```
          bx cs init --host https://uk-south.containers.bluemix.net
          ```
          {: pre}

        - EU-Central:
          ```
          bx cs init --host https://eu-central.containers.bluemix.net
          ```
          {: pre}

        - AP-South:
          ```
          bx cs init --host https://ap-south.containers.bluemix.net
          ```
          {: pre}

4.  List all of the clusters in the account to get the name of the cluster.

    ```
    bx cs clusters
    ```
    {: pre}

5.  Set the cluster you created as the context for this session. Complete these configuration steps every time that you work with your cluster.
    1.  Get the command to set the environment variable and download the Kubernetes configuration files.

        ```
        bx cs cluster-config <cluster_name_or_id>
        ```
        {: pre}

        After downloading the configuration files, a command is displayed that you can use to set the path to the local Kubernetes configuration file as an environment variable.

        Example:

        ```
        export KUBECONFIG=/Users/<user_name>/.bluemix/plugins/container-service/clusters/<cluster_name>/kube-config-prod-dal10-<cluster_name>.yml
        ```
        {: screen}

    2.  Copy and paste the command that is displayed in your terminal to set the `KUBECONFIG` environment variable.
    3.  Verify that the `KUBECONFIG` environment variable is set properly.

        Example:

        ```
        echo $KUBECONFIG
        ```
        {: pre}

        Output:
        ```
        /Users/<user_name>/.bluemix/plugins/container-service/clusters/<cluster_name>/kube-config-prod-dal10-<cluster_name>.yml
        ```
        {: screen}

6.  Verify that the `kubectl` commands run properly with your cluster by checking the Kubernetes CLI server version.

    ```
    kubectl version  --short
    ```
    {: pre}

    Example output:

    ```
    Client Version: v1.5.6
    Server Version: v1.5.6
    ```
    {: screen}


Now, you can run `kubectl` commands to manage your clusters in {{site.data.keyword.Bluemix_notm}}. For a full list of commands, see the [Kubernetes documentation ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/user-guide/kubectl/v1.5/).

**Tip:** If you are using Windows and the Kubernetes CLI is not installed in the same directory as the {{site.data.keyword.Bluemix_notm}} CLI, you must change directories to the path where the Kubernetes CLI is installed to run `kubectl` commands successfully.

## Updating the CLI
{: #cs_cli_upgrade}

You might want to update the CLIs periodically to use new features.
{:shortdesc}

This task includes the information for updating these CLIs.

-   {{site.data.keyword.Bluemix_notm}} CLI version 0.5.0 or later
-   {{site.data.keyword.containershort_notm}} plug-in
-   Kubernetes CLI version 1.5.6 or later
-   {{site.data.keyword.registryshort_notm}} plug-in
-   Docker version 1.9. or later

<br>
To update the CLIs:
1.  Update the {{site.data.keyword.Bluemix_notm}} CLI. Download the [latest version ![External link icon](../icons/launch-glyph.svg "External link icon")](https://clis.ng.bluemix.net/ui/home.html) and run the installer.

2.  Log in to the {{site.data.keyword.Bluemix_notm}} CLI. Enter your {{site.data.keyword.Bluemix_notm}} credentials when prompted.

    ```
    bx login
    ```
     {: pre}

    To specify a specific {{site.data.keyword.Bluemix_notm}} region, include the API endpoint. If you have private Docker images that are stored in the container registry of a specific {{site.data.keyword.Bluemix_notm}} region, or {{site.data.keyword.Bluemix_notm}} services instances that you already created, log in to this region to access your images and {{site.data.keyword.Bluemix_notm}} services.

    The {{site.data.keyword.Bluemix_notm}} region that you log in to also determines the region where you can create your Kubernetes clusters, including the available datacenters. If you do not specify a region, you are automatically logged in to the region that is closest to you.

    -   US South

        ```
        bx login -a api.ng.bluemix.net
        ```
        {: pre}

    -   Sydney

        ```
        bx login -a api.au-syd.bluemix.net
        ```
        {: pre}

    -   Germany

        ```
        bx login -a api.eu-de.bluemix.net
        ```
        {: pre}

    -   United Kingdom

        ```
        bx login -a api.eu-gb.bluemix.net
        ```
        {: pre}

        **Note:** If you have a federated ID, use `bx login --sso` to log in to the {{site.data.keyword.Bluemix_notm}} CLI. Enter your user name and use the provided URL in your CLI output to retrieve your one-time passcode. You know you have a federated ID when the login fails without the `--sso` and succeeds with the `--sso` option.

3.  Update the {{site.data.keyword.containershort_notm}} plug-in.
    1.  Install the update from the {{site.data.keyword.Bluemix_notm}} plug-in repository.

        ```
        bx plugin update container-service -r {{site.data.keyword.Bluemix_notm}}
        ```
        {: pre}

    2.  Verify the plug-in installation by running the following command and checking the list of the plug-ins that are installed.

        ```
        bx plugin list
        ```
        {: pre}

        The {{site.data.keyword.containershort_notm}} plug-in is displayed in the results as container-service.

    3.  Initialize the CLI.

        ```
        bx cs init
        ```
        {: pre}

4.  Update the Kubernetes CLI.
    1.  Download the Kubernetes CLI.

        OS X:   [https://storage.googleapis.com/kubernetes-release/release/v1.5.6/bin/darwin/amd64/kubectl ![External link icon](../icons/launch-glyph.svg "External link icon")](https://storage.googleapis.com/kubernetes-release/release/v1.5.6/bin/darwin/amd64/kubectl)

        Linux:   [https://storage.googleapis.com/kubernetes-release/release/v1.5.6/bin/linux/amd64/kubectl ![External link icon](../icons/launch-glyph.svg "External link icon")](https://storage.googleapis.com/kubernetes-release/release/v1.5.6/bin/linux/amd64/kubectl)

        Windows:   [https://storage.googleapis.com/kubernetes-release/release/v1.5.6/bin/windows/amd64/kubectl.exe ![External link icon](../icons/launch-glyph.svg "External link icon")](https://storage.googleapis.com/kubernetes-release/release/v1.5.6/bin/windows/amd64/kubectl.exe)

        **Tip:** If you are using Windows, install the Kubernetes CLI in the same directory as the {{site.data.keyword.Bluemix_notm}} CLI. This setup saves you some filepath changes when you run commands later.

    2.  For OSX and Linux users, complete the following steps.
        1.  Move the executable file to the /usr/local/bin directory.

            ```
            mv /<path_to_file>/kubectl/usr/local/bin/kubectl
            ```
            {: pre}

        2.  Make sure that `/usr/local/bin` is listed in your `PATH` system variable. The `PATH` variable contains all directories where your operating system can find executable files. The directories that are listed in the `PATH` variable serve different purposes. `/usr/local/bin` is used to store executable files for software that is not part of the operating system and that was manually installed by the system administrator.

            ```
            echo $PATH
            ```
            {: pre}

            Example CLI output:

            ```
            /usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin
            ```
            {: screen}

        3.  Convert the binary file to an executable.

            ```
            chmod +x /usr/local/bin/kubectl
            ```
            {: pre}

5.  Update the {{site.data.keyword.registryshort_notm}} plug-in.
    1.  Install the update from the {{site.data.keyword.Bluemix_notm}} plug-in repository.

        ```
        bx plugin update container-registry -r {{site.data.keyword.Bluemix_notm}}
        ```
        {: pre}

    2.  Verify the plug-in installation by running the following command and checking the list of the plug-ins that are installed.

        ```
        bx plugin list
        ```
        {: pre}

        The registry plug-in is displayed in the results as container-registry.

6.  Update Docker.
    -   If you are using Docker Community Edition, start Docker, click the **Docker** icon, and click **Check for updates**.
    -   If you are using Docker Toolbox, download the [latest version ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.docker.com/products/docker-toolbox) and run the installer.

## Uninstalling the CLI
{: #cs_cli_uninstall}

If you no longer need the CLI, you can uninstall it.
{:shortdesc}

This task includes the information for removing these CLIs:


-   {{site.data.keyword.containershort_notm}} plug-in
-   Kubernetes CLI version 1.5.6 or later
-   {{site.data.keyword.registryshort_notm}} plug-in
-   Docker version 1.9. or later

<br>
To uninstall the CLIs:

1.  Uninstall the {{site.data.keyword.containershort_notm}} plug-in.

    ```
    bx plugin uninstall container-service
    ```
    {: pre}

2.  Uninstall the {{site.data.keyword.registryshort_notm}} plug-in.

    ```
    bx plugin uninstall container-registry
    ```
    {: pre}

3.  Verify the plug-ins were uninstalled by running the following command and checking the list of the plug-ins that are installed.

    ```
    bx plugin list
    ```
    {: pre}

    The container-service and the container-registry plug-in are not displayed in the results.





6.  Uninstall Docker. Instructions to uninstall Docker vary based on the operating system that you use.

    <table summary="OS specific instructions to uninstall Docker">
     <tr>
      <th>Operating system</th>
      <th>Link</th>
     </tr>
     <tr>
      <td>OSX</td>
      <td>Choose to uninstall Docker from the [GUI or command line ![External link icon](../icons/launch-glyph.svg "External link icon")](https://docs.docker.com/docker-for-mac/#uninstall-or-reset)</td>
     </tr>
     <tr>
      <td>Linux</td>
      <td>Instructions to uninstall Docker vary based on the Linux distribution that you use. To uninstall Docker for Ubuntu, see [Uninstall Docker ![External link icon](../icons/launch-glyph.svg "External link icon")](https://docs.docker.com/engine/installation/linux/docker-ce/ubuntu/#uninstall-docker-ce). Use this link to find instructions about how to uninstall Docker for other Linux distributions by selecting your distribution from the navigation.</td>
     </tr>
      <tr>
        <td>Windows</td>
        <td>Uninstall the [Docker toolbox ![External link icon](../icons/launch-glyph.svg "External link icon")](https://docs.docker.com/toolbox/toolbox_install_mac/#how-to-uninstall-toolbox).</td>
      </tr>
    </table>

## Automating cluster deployments with the API
{: #cs_api}

You can use the {{site.data.keyword.containershort_notm}} API to automate the creation, deployment, and management of your Kubernetes clusters.
{:shortdesc}

The {{site.data.keyword.containershort_notm}} API requires header information that you must provide in your API request and that can vary depending on the API that you want to use. To determine what header information is needed for your API, see the [{{site.data.keyword.containershort_notm}} API documentation ![External link icon](../icons/launch-glyph.svg "External link icon")](https://us-south.containers.bluemix.net/swagger-api).



1.  Create your IAM (Identity and Access Management) access token. The IAM access token is required to work with {{site.data.keyword.containershort_notm}}. Replace _&lt;my_bluemix_username&gt;_ and _&lt;my_bluemix_password&gt;_ with your {{site.data.keyword.Bluemix_notm}} credentials.

    ```
    POST https://iam.<region>.bluemix.net/oidc/token
    ```
    {: pre}

    <table summary-"Input parameters to get tokens">
    <thead>
        <th>Input Parameters</th>
        <th>Values</th>
    </thead>
    <tbody>
    <tr>
    <td>Header</td>
    <td><ul><li>Content-Type:application/x-www-form-urlencoded</li> <li>Authorization: Basic Yng6Yng=</li></ul>
    </td>
    </tr>
    <tr>
    <td>Body </td>
    <td><ul><li>grant_type: password</li>
    <li>response_type: cloud_iam, uaa</li>
    <li>username: <em>&lt;my_bluemix_username&gt;</em></li>
    <li>password: <em>&lt;my_bluemix_password&gt;</em></li>
    <li>uaa_client_id: cf</li>
    <li>uaa_client_secret:</li></ul>
    <p>**Note:** Add the uaa_client_secret key with no value specified.</p></td>
    </tr>
    
    </tbody>
    </table>

    Example API output:

    ```
    {
    "access_token": "<iam_token>",
    "refresh_token": "<iam_refresh_token>",
    "uaa_token": "<uaa_token>",
    "uaa_refresh_token": "<uaa_refresh_token>",
    "token_type": "Bearer",
    "expires_in": 3600,
    "expiration": 1493747503
    }

    ```
    {: screen}

    You can find the IAM token in the **access_token**, and a UAA token in the **uaa_token** field of your API ouput. Note the IAM and the UAA token to retrieve additional header information in the next steps.

2.  Retrieve the ID of your {{site.data.keyword.Bluemix_notm}} account where you want to create and manage your clusters. Replace _&lt;iam_token&gt;_ with the IAM token that you retrieved in the previous step.

    ```
    GET https://accountmanagement.<region>.bluemix.net/v1/accounts
    ```
    {: pre}

    <table summary="Input parameters to get Bluemix account ID">
    <thead>
   <th>Input parameters</th>
   <th>Values</th>
    </thead>
    <tbody>
   <tr>
    <td>Headers</td>
    <td><ul><li>Content-Type: application/json</li>
      <li>Authorization: bearer &lt;iam_token&gt;</li>
      <li>Accept: application/json</li></ul></td>
   </tr>
    </tbody>
    </table>

    Example API output:

    ```
    {
      "total_results": 3,
      "total_pages": 1,
      "prev_url": null,
      "next_url": null,
      "resources":
        {
          "metadata": {
            "guid": "<my_bluemix_account_id>",
            "url": "/v1/accounts/<my_bluemix_account_id>",
            "created_at": "2016-01-07T18:55:09.726Z",
            "updated_at": "2017-04-28T23:46:03.739Z",
            "origin": "BSS"
    ...
    ```
    {: screen}

    You can find the ID of your {{site.data.keyword.Bluemix_notm}} account in the **resources/metadata/guid** field of your API output.

3.  Retrieve a new IAM token that includes your {{site.data.keyword.Bluemix_notm}} account information. Replace _&lt;my_bluemix_account_id&gt;_ with the ID of your {{site.data.keyword.Bluemix_notm}} account that you retrieved in the previous step.

    **Note:** IAM access tokens expire after 1 hour. Review [Refreshing your IAM access token with the API](#cs_api_refresh) to find instructions about how to refresh your access token.

    ```
    POST https://iam.<region>.bluemix.net/oidc/token
    ```
    {: pre}

    <table summary="Input parameters to get access tokens">
    <thead>
    <th>Input parameters</th>
    <th>Values</th>
    </thead>
    <tbody>
    <tr>
    <td>Headers</td>
    <td><ul><li>Content-Type: application/x-www-form-urlencoded</li>
    <li>Authorization: Basic Yng6Yng=</li></ul></td>
    </tr>
    <tr>
    <td>Body</td>
    <td><ul><li>grant_type: password</li><li>response_type: cloud_iam, uaa</li>
    <li>username: <em>&lt;my_bluemix_username&gt;</em></li>
    <li>password: <em>&lt;my_bluemix_password&gt;</em></li>
    <li>uaa_client_id: cf</li>
    <li>uaa_client_secret:<p>**Note:** Add the uaa_client_secret key with no value specified.</p>
    <li>bss_account: <em>&lt;my_bluemix_account_id&gt;</em></li></ul></td>
    </tr>
    </tbody>
    </table>

    Example API output:

    ```
    {
      "access_token": "<iam_token>",
      "refresh_token": "<iam_refresh_token>",
      "uaa_token": "<uaa_token>",
      "uaa_refresh_token": "<uaa_refresh_token>",
      "token_type": "Bearer",
      "expires_in": 3600,
      "expiration": 1493747503
    }

    ```
    {: screen}

    You can find the IAM token in the **access_token**, and your IAM refresh token in the **refresh_token** field of your CLI output.

4.  Retrieve the ID of your {{site.data.keyword.Bluemix_notm}} space where you want to create or manage your cluster.
    1.  Retrieve the API endpoint to access your space ID. Replace _&lt;uaa_token&gt;_ with the UAA token that you retrieved in the first step.

        ```
        GET https://api.<region>.bluemix.net/v2/organizations
        ```
        {: pre}

        <table summary="Input parametersto retrive space ID">
        <thead>
        <th>Input parameters</th>
        <th>Values</th>
        </thead>
        <tbody>
        <tr>
        <td>Header</td>
        <td><ul><li>Content-Type: application/x-www-form-urlencoded;charset=utf</li>
        <li>Authorization: bearer &lt;uaa_token&gt;</li>
        <li>Accept: application/json;charset=utf-8</li></ul></td>
        </tr>
        </tbody>
        </table>

      Example API output:

      ```
      {
            "metadata": {
              "guid": "<my_bluemix_org_id>",
              "url": "/v2/organizations/<my_bluemix_org_id>",
              "created_at": "2016-01-07T18:55:19Z",
              "updated_at": "2016-02-09T15:56:22Z"
            },
            "entity": {
              "name": "<my_bluemix_org_name>",
              "billing_enabled": false,
              "quota_definition_guid": "<my_bluemix_org_id>",
              "status": "active",
              "quota_definition_url": "/v2/quota_definitions/<my_bluemix_org_id>",
              "spaces_url": "/v2/organizations/<my_bluemix_org_id>/spaces",
      ...

      ```
      {: screen}

5.  Note the output of the **spaces_url** field.
6.  Retrieve the ID of your {{site.data.keyword.Bluemix_notm}} space by using the **spaces_url** endpoint.

      ```
      GET https://api.<region>.bluemix.net/v2/organizations/<my_bluemix_org_id>/spaces
      ```
      {: pre}

      Example API output:

      ```
      {
            "metadata": {
              "guid": "<my_bluemix_space_id>",
              "url": "/v2/spaces/<my_bluemix_space_id>",
              "created_at": "2016-01-07T18:55:22Z",
              "updated_at": null
            },
            "entity": {
              "name": "<my_bluemix_space_name>",
              "organization_guid": "<my_bluemix_org_id>",
              "space_quota_definition_guid": null,
              "allow_ssh": true,
      ...
      ```
      {: screen}

      You can find the ID of your {{site.data.keyword.Bluemix_notm}} space in the **metadata/guid** field of your API output.

7.  List all Kubernetes clusters in your account. Use the information that you retrieved in earlier steps to build your header information.

    -   US-South

        ```
        GET https://us-south.containers.bluemix.net/v1/clusters
        ```
        {: pre}

    -   UK-South

        ```
        GET https://uk-south.containers.bluemix.net/v1/clusters
        ```
        {: pre}

    -   EU-Central

        ```
        GET https://eu-central.containers.bluemix.net/v1/clusters
        ```
        {: pre}

    -   AP-South

        ```
        GET https://ap-south.containers.bluemix.net/v1/clusters
        ```
        {: pre}

        <table summary="Input parameters to work with API">
        <thead>
        <th>Input parameters</th>
        <th>Values</th>
        </thead>
        <tbody>
        <tr>
        <td>Header</td>
        <td><ul><li>Authorization: bearer <em>&lt;iam_token&gt;</em></li></ul>
        </tr>
        </tbody>
        </table>

8.  Review the [{{site.data.keyword.containershort_notm}} API documentation ![External link icon](../icons/launch-glyph.svg "External link icon")](https://us-south.containers.bluemix.net/swagger-api) to find a list of supported APIs.

## Refreshing IAM access tokens
{: #cs_api_refresh}

Every IAM (Identity and Access Management) access token that is issued via the API expires after one hour. You must refresh your access token on a regular basis to assure access to the {{site.data.keyword.containershort_notm}} API.
{:shortdesc}

Before you begin, make sure that you have an IAM refresh token that you can use to request a new access token. If you do not have a refresh token, review [Automating the cluster creation and management process with the {{site.data.keyword.containershort_notm}} API](#cs_api) to retrieve your access token.

Use the following steps if you want to refresh your IAM token.

1.  Retrieve a new IAM access token. Replace _&lt;iam_refresh_token&gt;_ with the IAM refresh token that you received when you first authenticated with {{site.data.keyword.Bluemix_notm}}.

    ```
    POST https://iam.ng.bluemix.net/oidc/token
    ```
    {: pre}

    <table summary="Input parameters for new IAM token">
    <thead>
    <th>Input parameters</th>
    <th>Values</th>
    </thead>
    <tbody>
    <tr>
    <td>Header</td>
    <td><ul><li>Content-Type: application/x-www-form-urlencoded</li>
    <li>Authorization: Basic Yng6Yng=</li></ul></td>
    </tr>
    <tr>
    <td>Body</td>
    <td><ul><li>grant_type: refresh_token</li>
    <li>response_type: cloud_iam, uaa</li>
    <li>refresh_token: <em>&lt;iam_refresh_token&gt;</em></li>
    <li>uaa_client_id: cf</li>
    <li>uaa_client_secret:<p>**Note:** Add the uaa_client_secret key with no value specified.</p></li></ul></td>
    </tr>
    </tbody>
    </table>

    Example API output:

    ```
    {
      "access_token": "<iam_token>",
      "refresh_token": "<iam_refresh_token>",
      "uaa_token": "<uaa_token>",
      "uaa_refresh_token": "<uaa_refresh_token>",
      "token_type": "Bearer",
      "expires_in": 3600,
      "expiration": 1493747503
    }

    ```
    {: screen}

    You can find your new IAM token in the **access_token**, and the IAM refresh token in the **refresh_token** field of your API output.

2.  Continue working with the [{{site.data.keyword.containershort_notm}} API documentation ![External link icon](../icons/launch-glyph.svg "External link icon")](https://us-south.containers.bluemix.net/swagger-api) by using the token from the previous step.
