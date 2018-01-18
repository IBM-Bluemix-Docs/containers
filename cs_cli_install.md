---

copyright:
  years: 2014, 2018
lastupdated: "2017-12-01"

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

<br />


## Installing the CLI
{: #cs_cli_install_steps}

Install the required CLIs to create and manage your Kubernetes clusters in {{site.data.keyword.containershort_notm}}, and to deploy containerized apps to your cluster.
{:shortdesc}

This task includes the information for installing these CLIs and plug-ins:

-   {{site.data.keyword.Bluemix_notm}} CLI version 0.5.0 or later
-   {{site.data.keyword.containershort_notm}} plug-in
-   Kubernetes CLI version 1.8.6 or later
-   Optional: {{site.data.keyword.registryshort_notm}} plug-in
-   Optional: Docker version 1.9 or later

<br>
To install the CLIs:

1.  As a prerequisite for the {{site.data.keyword.containershort_notm}} plug-in, install the [{{site.data.keyword.Bluemix_notm}} CLI ![External link icon](../icons/launch-glyph.svg "External link icon")](https://clis.ng.bluemix.net/ui/home.html). The prefix for running commands by using the {{site.data.keyword.Bluemix_notm}} CLI is `bx`.

2.  Log in to the {{site.data.keyword.Bluemix_notm}} CLI. Enter your {{site.data.keyword.Bluemix_notm}} credentials when prompted.

    ```
    bx login
    ```
    {: pre}

    **Note:** If you have a federated ID, use `bx login --sso` to log in to the {{site.data.keyword.Bluemix_notm}} CLI. Enter your user name and use the provided URL in your CLI output to retrieve your one-time passcode. You know you have a federated ID when the login fails without the `--sso` and succeeds with the `--sso` option.

3.  To create Kubernetes clusters and manage worker nodes, install the {{site.data.keyword.containershort_notm}} plug-in. The prefix for running commands by using the {{site.data.keyword.containershort_notm}} plug-in is `bx cs`.

    ```
    bx plugin install container-service -r Bluemix
    ```
    {: pre}

    To verify that the plug-in is installed properly, run the following command:

    ```
    bx plugin list
    ```
    {: pre}

    The {{site.data.keyword.containershort_notm}} plug-in is displayed in the results as container-service.

4.  To view a local version of the Kubernetes dashboard and to deploy apps into your clusters, [install the Kubernetes CLI ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/tasks/tools/install-kubectl/). The prefix for running commands by using the Kubernetes CLI is `kubectl`.

    1.  For complete functional compatibility, download the Kubernetes CLI version that matches the Kubernetes cluster version you plan to use. The current {{site.data.keyword.containershort_notm}} default Kubernetes version is 1.8.6.

        OS X:   [https://storage.googleapis.com/kubernetes-release/release/v1.7.4/bin/darwin/amd64/kubectl ![External link icon](../icons/launch-glyph.svg "External link icon")](https://storage.googleapis.com/kubernetes-release/release/v1.7.4/bin/darwin/amd64/kubectl)

        Linux:   [https://storage.googleapis.com/kubernetes-release/release/v1.7.4/bin/linux/amd64/kubectl ![External link icon](../icons/launch-glyph.svg "External link icon")](https://storage.googleapis.com/kubernetes-release/release/v1.7.4/bin/linux/amd64/kubectl)

        Windows:   [https://storage.googleapis.com/kubernetes-release/release/v1.7.4/bin/windows/amd64/kubectl.exe ![External link icon](../icons/launch-glyph.svg "External link icon")](https://storage.googleapis.com/kubernetes-release/release/v1.7.4/bin/windows/amd64/kubectl.exe)

        **Tip:** If you are using Windows, install the Kubernetes CLI in the same directory as the {{site.data.keyword.Bluemix_notm}} CLI. This setup saves you some filepath changes when you run commands later.

    2.  For OSX and Linux users, complete the following steps.
        1.  Move the executable file to the `/usr/local/bin` directory.

            ```
            mv /<path_to_file>/kubectl /usr/local/bin/kubectl
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

        3.  Make the file executable.

            ```
            chmod +x /usr/local/bin/kubectl
            ```
            {: pre}

5.  To manage a private image repository, install the {{site.data.keyword.registryshort_notm}} plug-in. Use this plug-in to set up your own namespace in a multi-tenant, highly available, and scalable private image registry that is hosted by IBM, and to store and share Docker images with other users. Docker images are required to deploy containers into a cluster. The prefix for running registry commands is `bx cr`.

    ```
    bx plugin install container-registry -r Bluemix
    ```
    {: pre}

    To verify that the plug-in is installed properly, run the following command:

    ```
    bx plugin list
    ```
    {: pre}

    The plug-in is displayed in the results as container-registry.

6.  To build images locally and push them to your registry namespace, [install Docker ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.docker.com/community-edition#/download). If you are using Windows 8 or earlier, you can install the [Docker Toolbox ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.docker.com/products/docker-toolbox) instead. The Docker CLI is used to build apps into images. The prefix for running commands by using the Docker CLI is `docker`.

Next, start [Creating Kubernetes clusters from the CLI with {{site.data.keyword.containershort_notm}}](cs_clusters.html#clusters_cli).

For reference information about these CLIs, see the documentation for those tools.

-   [`bx` commands](/docs/cli/reference/bluemix_cli/bx_cli.html)
-   [`bx cs` commands](cs_cli_reference.html#cs_cli_reference)
-   [`kubectl` commands ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands)
-   [`bx cr` commands](/docs/cli/plugins/registry/index.html)

<br />


## Configuring the CLI to run `kubectl`
{: #cs_cli_configure}

You can use the commands that are provided with the Kubernetes CLI to manage clusters in {{site.data.keyword.Bluemix_notm}}. All `kubectl` commands that are available in Kubernetes 1.8.6 are supported for use with clusters in {{site.data.keyword.Bluemix_notm}}. After you create a cluster, set the context for your local CLI to that cluster with an environment variable. Then, you can run the Kubernetes `kubectl` commands to work with your cluster in {{site.data.keyword.Bluemix_notm}}.
{:shortdesc}

Before you can run `kubectl` commands, [install the required CLIs](#cs_cli_install) and [create a cluster](cs_clusters.html#clusters_cli).

1.  Log in to the {{site.data.keyword.Bluemix_notm}} CLI. Enter your {{site.data.keyword.Bluemix_notm}} credentials when prompted. To specify an {{site.data.keyword.Bluemix_notm}} region, [include the API endpoint](cs_regions.html#bluemix_regions).

      ```
      bx login
      ```
      {: pre}

      **Note:** If you have a federated ID, use `bx login --sso` to log in to the {{site.data.keyword.Bluemix_notm}} CLI. Enter your user name and use the provided URL in your CLI output to retrieve your one-time passcode. You know you have a federated ID when the login fails without the `--sso` and succeeds with the `--sso` option.

  2.  Select an {{site.data.keyword.Bluemix_notm}} account. If you are assigned to multiple {{site.data.keyword.Bluemix_notm}} organizations, select the organization where the cluster was created. Clusters are specific to an organization, but are independent from an {{site.data.keyword.Bluemix_notm}} space. Therefore, you are not required to select a space.

  3.  If you want to create or access Kubernetes clusters in a region other than the {{site.data.keyword.Bluemix_notm}} region that you selected earlier, run `bx cs region-set`.

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
      Client Version: v1.8.6
      Server Version: v1.8.6
      ```
      {: screen}

Now, you can run `kubectl` commands to manage your clusters in {{site.data.keyword.Bluemix_notm}}. For a full list of commands, see the [Kubernetes documentation ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands).

**Tip:** If you are using Windows and the Kubernetes CLI is not installed in the same directory as the {{site.data.keyword.Bluemix_notm}} CLI, you must change directories to the path where the Kubernetes CLI is installed to run `kubectl` commands successfully.


<br />


## Updating the CLI
{: #cs_cli_upgrade}

You might want to update the CLIs periodically to use new features.
{:shortdesc}

This task includes the information for updating these CLIs.

-   {{site.data.keyword.Bluemix_notm}} CLI version 0.5.0 or later
-   {{site.data.keyword.containershort_notm}} plug-in
-   Kubernetes CLI version 1.8.6 or later
-   {{site.data.keyword.registryshort_notm}} plug-in
-   Docker version 1.9. or later

<br>
To update the CLIs:

1.  Update the {{site.data.keyword.Bluemix_notm}} CLI. Download the [latest version ![External link icon](../icons/launch-glyph.svg "External link icon")](https://clis.ng.bluemix.net/ui/home.html) and run the installer.

2. Log in to the {{site.data.keyword.Bluemix_notm}} CLI. Enter your {{site.data.keyword.Bluemix_notm}} credentials when prompted. To specify an {{site.data.keyword.Bluemix_notm}} region, [include the API endpoint](cs_regions.html#bluemix_regions).

    ```
    bx login
    ```
    {: pre}

     **Note:** If you have a federated ID, use `bx login --sso` to log in to the {{site.data.keyword.Bluemix_notm}} CLI. Enter your user name and use the provided URL in your CLI output to retrieve your one-time passcode. You know you have a federated ID when the login fails without the `--sso` and succeeds with the `--sso` option.

3.  Update the {{site.data.keyword.containershort_notm}} plug-in.
    1.  Install the update from the {{site.data.keyword.Bluemix_notm}} plug-in repository.

        ```
        bx plugin update container-service -r Bluemix
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
    1.  Update to the Kubernetes CLI version that matches the Kubernetes cluster version you plan to use. The current {{site.data.keyword.containershort_notm}} default Kubernetes version is 1.8.6.

        OS X:   [https://storage.googleapis.com/kubernetes-release/release/v1.7.4/bin/darwin/amd64/kubectl ![External link icon](../icons/launch-glyph.svg "External link icon")](https://storage.googleapis.com/kubernetes-release/release/v1.7.4/bin/darwin/amd64/kubectl)

        Linux:   [https://storage.googleapis.com/kubernetes-release/release/v1.7.4/bin/linux/amd64/kubectl ![External link icon](../icons/launch-glyph.svg "External link icon")](https://storage.googleapis.com/kubernetes-release/release/v1.7.4/bin/linux/amd64/kubectl)

        Windows:   [https://storage.googleapis.com/kubernetes-release/release/v1.7.4/bin/windows/amd64/kubectl.exe ![External link icon](../icons/launch-glyph.svg "External link icon")](https://storage.googleapis.com/kubernetes-release/release/v1.7.4/bin/windows/amd64/kubectl.exe)

        **Tip:** If you are using Windows, install the Kubernetes CLI in the same directory as the {{site.data.keyword.Bluemix_notm}} CLI. This setup saves you some filepath changes when you run commands later.

    2.  For OSX and Linux users, complete the following steps.
        1.  Move the executable file to the `/usr/local/bin` directory.

            ```
            mv /<path_to_file>/kubectl /usr/local/bin/kubectl
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

        3.  Make the file executable.

            ```
            chmod +x /usr/local/bin/kubectl
            ```
            {: pre}

5.  Update the {{site.data.keyword.registryshort_notm}} plug-in.
    1.  Install the update from the {{site.data.keyword.Bluemix_notm}} plug-in repository.

        ```
        bx plugin update container-registry -r Bluemix
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

<br />


## Uninstalling the CLI
{: #cs_cli_uninstall}

If you no longer need the CLI, you can uninstall it.
{:shortdesc}

This task includes the information for removing these CLIs:


-   {{site.data.keyword.containershort_notm}} plug-in
-   Kubernetes CLI version 1.8.6 or later
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

    - [OSX ![External link icon](../icons/launch-glyph.svg "External link icon")](https://docs.docker.com/docker-for-mac/#uninstall-or-reset)
    - [Linux ![External link icon](../icons/launch-glyph.svg "External link icon")](https://docs.docker.com/engine/installation/linux/docker-ce/ubuntu/#uninstall-docker-ce)
    - [Windows ![External link icon](../icons/launch-glyph.svg "External link icon")](https://docs.docker.com/toolbox/toolbox_install_mac/#how-to-uninstall-toolbox)

<br />


## Automating cluster deployments with the API
{: #cs_api}

You can use the {{site.data.keyword.containershort_notm}} API to automate the creation, deployment, and management of your Kubernetes clusters.
{:shortdesc}

The {{site.data.keyword.containershort_notm}} API requires header information that you must provide in your API request and that can vary depending on the API that you want to use. To determine what header information is needed for your API, see the [{{site.data.keyword.containershort_notm}} API documentation ![External link icon](../icons/launch-glyph.svg "External link icon")](https://us-south.containers.bluemix.net/swagger-api).

**Note:** To authenticate with {{site.data.keyword.containershort_notm}}, you must provide an Identity and Access Management (IAM) token that is generated with your {{site.data.keyword.Bluemix_notm}} credentials and that includes the {{site.data.keyword.Bluemix_notm}} account ID where the cluster was created. Depending on the way you authenticate with {{site.data.keyword.Bluemix_notm}}, you can choose between the following options to automate the creation of your IAM token.

<table>
<thead>
<th>{{site.data.keyword.Bluemix_notm}} ID</th>
<th>My options</th>
</thead>
<tbody>
<tr>
<td>Unfederated ID</td>
<td><ul><li><strong>{{site.data.keyword.Bluemix_notm}} user name and password:</strong> You can follow the steps in this topic to fully automate the creation of your IAM access token.</li>
<li><strong>Generate an {{site.data.keyword.Bluemix_notm}} API key:</strong> As an alternative to using the {{site.data.keyword.Bluemix_notm}} user name and password, you can <a href="../iam/apikeys.html#manapikey" target="_blank">use {{site.data.keyword.Bluemix_notm}} API keys</a> {{site.data.keyword.Bluemix_notm}} API keys are dependent on the {{site.data.keyword.Bluemix_notm}} account they are generated for. You cannot combine your {{site.data.keyword.Bluemix_notm}} API key with a different account ID in the same IAM token. To access clusters that were created with an account other than the one your {{site.data.keyword.Bluemix_notm}} API key is based on, you must log in to the account to generate a new API key. </li></ul></tr>
<tr>
<td>Federated ID</td>
<td><ul><li><strong>Generate an {{site.data.keyword.Bluemix_notm}} API key:</strong> <a href="../iam/apikeys.html#manapikey" target="_blank">{{site.data.keyword.Bluemix_notm}} API keys</a> are dependent on the {{site.data.keyword.Bluemix_notm}} account they are generated for. You cannot combine your {{site.data.keyword.Bluemix_notm}} API key with a different account ID in the same IAM token. To access clusters that were created with an account other than the one your {{site.data.keyword.Bluemix_notm}} API key is based on, you must log in to the account to generate a new API key. </li><li><strong>Use a one-time passcode: </strong> If you authenticate with {{site.data.keyword.Bluemix_notm}} by using a one-time passcode, you cannot fully automate the creation of your IAM token because the retrieval of your one-time passcode requires a manual interaction with your web browser. To fully automate the creation of your IAM token, you must create an {{site.data.keyword.Bluemix_notm}} API key instead. </ul></td>
</tr>
</tbody>
</table>

1.  Create your IAM (Identity and Access Management) access token. The body information that is included in your request varies based on the {{site.data.keyword.Bluemix_notm}} authentication method that you use. Replace the following values:
  - _&lt;my_username&gt;_: Your {{site.data.keyword.Bluemix_notm}} user name.
  - _&lt;my_password&gt;_: Your {{site.data.keyword.Bluemix_notm}} password.
  - _&lt;my_api_key&gt;_: Your {{site.data.keyword.Bluemix_notm}} API key.
  - _&lt;my_passcode&gt;_: Your {{site.data.keyword.Bluemix_notm}} one-time passcode. Run `bx login --sso` and follow the instructions in your CLI output to retrieve your one-time passcode by using your web browser.

    ```
    POST https://iam.<region>.bluemix.net/oidc/token
    ```
    {: codeblock}

    Example:
    ```
    POST https://iam.ng.bluemix.net/oidc/token
    ```
    {: pre}

    To specify an {{site.data.keyword.Bluemix_notm}} region, [review the region abbreviations as they are used in the API endpoints](cs_regions.html#bluemix_regions).

    <table summary-"Input parameters to get tokens">
    <thead>
        <th>Input Parameters</th>
        <th>Values</th>
    </thead>
    <tbody>
    <tr>
    <td>Header</td>
    <td><ul><li>Content-Type:application/x-www-form-urlencoded</li> <li>Authorization: Basic Yng6Yng=<p><b>Note</b>: Provided for you is Yng6Yng=, the URL-encoded authorization for the username **bx** and the password **bx**.</p></li></ul>
    </td>
    </tr>
    <tr>
    <td>Body for {{site.data.keyword.Bluemix_notm}} user name and password</td>
    <td><ul><li>grant_type: password</li>
    <li>response_type: cloud_iam, uaa</li>
    <li>username: <em>&lt;my_username&gt;</em></li>
    <li>password: <em>&lt;my_password&gt;</em></li>
    <li>uaa_client_id: cf</li>
    <li>uaa_client_secret:</li></ul>
    <p><b>Note</b>: Add the uaa_client_secret key with no value specified.</p></td>
    </tr>
    <tr>
    <td>Body for {{site.data.keyword.Bluemix_notm}} API keys</td>
    <td><ul><li>grant_type: urn:ibm:params:oauth:grant-type:apikey</li>
    <li>response_type: cloud_iam, uaa</li>
    <li>apikey: <em>&lt;my_api_key&gt;</em></li>
    <li>uaa_client_id: cf</li>
    <li>uaa_client_secret:</li></ul>
    <p><b>Note</b>: Add the uaa_client_secret key with no value specified.</p></td>
    </tr>
    <tr>
    <td>Body for {{site.data.keyword.Bluemix_notm}} one-time passcode</td>
    <td><ul><li>grant_type: urn:ibm:params:oauth:grant-type:passcode</li>
    <li>response_type: cloud_iam, uaa</li>
    <li>passcode: <em>&lt;my_passcode&gt;</em></li>
    <li>uaa_client_id: cf</li>
    <li>uaa_client_secret:</li></ul>
    <p><b>Note</b>: Add the uaa_client_secret key with no value specified.</p></td>
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

    You can find the IAM token in the **access_token** field of your API ouput. Note the IAM token to retrieve additional header information in the next steps.

2.  Retrieve the ID of the {{site.data.keyword.Bluemix_notm}} account where the cluster was created. Replace _&lt;iam_token&gt;_ with the IAM token that you retrieved in the previous step.

    ```
    GET https://accountmanagement.<region>.bluemix.net/v1/accounts
    ```
    {: codeblock}

    <table summary="Input parameters to get {{site.data.keyword.Bluemix_notm}} account ID">
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
            "guid": "<my_account_id>",
            "url": "/v1/accounts/<my_account_id>",
            "created_at": "2016-01-07T18:55:09.726Z",
            "updated_at": "2017-04-28T23:46:03.739Z",
            "origin": "BSS"
    ...
    ```
    {: screen}

    You can find the ID of your {{site.data.keyword.Bluemix_notm}} account in the **resources/metadata/guid** field of your API output.

3.  Generate a new IAM token that includes your {{site.data.keyword.Bluemix_notm}} credentials and the account ID where the cluster was created. Replace _&lt;my_account_id&gt;_ with the ID of the {{site.data.keyword.Bluemix_notm}} account that you retrieved in the previous step.

    **Note:** If you are using an {{site.data.keyword.Bluemix_notm}} API key, you must use the {{site.data.keyword.Bluemix_notm}} account ID the API key was created for. To access clusters in other accounts, log into this account and create an {{site.data.keyword.Bluemix_notm}} API key that is based on this account.

    ```
    POST https://iam.<region>.bluemix.net/oidc/token
    ```
    {: codeblock}

    Example:
    ```
    POST https://iam.ng.bluemix.net/oidc/token
    ```
    {: pre}

    To specify an {{site.data.keyword.Bluemix_notm}} region, [review the region abbreviations as they are used in the API endpoints](cs_regions.html#bluemix_regions).

    <table summary-"Input parameters to get tokens">
    <thead>
        <th>Input Parameters</th>
        <th>Values</th>
    </thead>
    <tbody>
    <tr>
    <td>Header</td>
    <td><ul><li>Content-Type:application/x-www-form-urlencoded</li> <li>Authorization: Basic Yng6Yng=<p><b>Note</b>: Provided for you is Yng6Yng=, the URL-encoded authorization for the username **bx** and the password **bx**.</p></li></ul>
    </td>
    </tr>
    <tr>
    <td>Body for {{site.data.keyword.Bluemix_notm}} user name and password</td>
    <td><ul><li>grant_type: password</li>
    <li>response_type: cloud_iam, uaa</li>
    <li>username: <em>&lt;my_username&gt;</em></li>
    <li>password: <em>&lt;my_password&gt;</em></li>
    <li>uaa_client_id: cf</li>
    <li>uaa_client_secret:</li>
    <li>bss_account: <em>&lt;my_account_id&gt;</em></li></ul>
    <p><b>Note</b>: Add the uaa_client_secret key with no value specified.</p></td>
    </tr>
    <tr>
    <td>Body for {{site.data.keyword.Bluemix_notm}} API keys</td>
    <td><ul><li>grant_type: urn:ibm:params:oauth:grant-type:apikey</li>
    <li>response_type: cloud_iam, uaa</li>
    <li>apikey: <em>&lt;my_api_key&gt;</em></li>
    <li>uaa_client_id: cf</li>
    <li>uaa_client_secret:</li>
    <li>bss_account: <em>&lt;my_account_id&gt;</em></li></ul>
    <p><b>Note</b>: Add the uaa_client_secret key with no value specified.</p></td>
    </tr>
    <tr>
    <td>Body for {{site.data.keyword.Bluemix_notm}} one-time passcode</td>
    <td><ul><li>grant_type: urn:ibm:params:oauth:grant-type:passcode</li>
    <li>response_type: cloud_iam, uaa</li>
    <li>passcode: <em>&lt;my_passcode&gt;</em></li>
    <li>uaa_client_id: cf</li>
    <li>uaa_client_secret:</li>
    <li>bss_account: <em>&lt;my_account_id&gt;</em></li></ul>
    <p><b>Note<b>: Add the uaa_client_secret key with no value specified.</p></td>
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

    You can find the IAM token in the **access_token** and the IAM refresh token in the **refresh_token**.

4.  List all Kubernetes clusters in your account. Use the information that you retrieved in earlier steps to build your header information.

        ```
        GET https://containers.bluemix.net/v1/clusters
        ```
        {: codeblock}

        <table summary="Input parameters to work with API">
        <thead>
        <th>Input parameters</th>
        <th>Values</th>
        </thead>
        <tbody>
        <tr>
        <td>Header</td>
        <td><ul><li>Authorization: bearer <em>&lt;iam_token&gt;</em></li>
        <li>X-Auth-Refresh-Token: <em>&lt;refresh_token&gt;</em></li></ul></td>
        </tr>
        </tbody>
        </table>

5.  Review the [{{site.data.keyword.containershort_notm}} API documentation ![External link icon](../icons/launch-glyph.svg "External link icon")](https://containers.bluemix.net/swagger-api) to find a list of supported APIs.

<br />


## Refreshing IAM access tokens
{: #cs_api_refresh}

Every IAM (Identity and Access Management) access token that is issued via the API expires after one hour. You must refresh your access token on a regular basis to assure access to the {{site.data.keyword.containershort_notm}} API.
{:shortdesc}

Before you begin, make sure that you have an IAM refresh token that you can use to request a new access token. If you do not have a refresh token, review [Automating the cluster creation and management process with the {{site.data.keyword.containershort_notm}} API](#cs_api) to retrieve your access token.

Use the following steps if you want to refresh your IAM token.

1.  Generate a new IAM access token. Replace _&lt;iam_refresh_token&gt;_ with the IAM refresh token that you received when you authenticated with {{site.data.keyword.Bluemix_notm}}.

    ```
    POST https://iam.bluemix.net/identity/token
    ```
    {: codeblock}

    <table summary="Input parameters for new IAM token">
    <thead>
    <th>Input parameters</th>
    <th>Values</th>
    </thead>
    <tbody>
    <tr>
    <td>Header</td>
    <td><ul><li>Content-Type: application/x-www-form-urlencoded</li>
    <li>Authorization: Basic Yng6Yng=<p><b>Note</b>: Provided for you is Yng6Yng=, the URL-encoded authorization for the username **bx** and the password **bx**.</p></li></ul></td>
    </tr>
    <tr>
    <td>Body</td>
    <td><ul><li>grant_type: refresh_token</li>
    <li>response_type: cloud_iam, uaa</li>
    <li>refresh_token: <em>&lt;iam_refresh_token&gt;</em></li>
    <li>uaa_client_id: cf</li>
    <li>uaa_client_secret:</li>
    <li>bss_account: <em>&lt;account_id&gt;</em></li></ul><p><b>Note</b>: Add the uaa_client_secret key with no value specified.</p></td>
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
