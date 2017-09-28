---

copyright:
  years: 2014, 2017
lastupdated: "2017-09-25"

---

{:new_window: target="blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}


# Tutorial: Creating clusters
{: #cs_cluster_tutorial}

Deploy and manage your own Kubernetes cluster in the cloud that lets you automate the deployment, operation, scaling, and monitoring of containerized apps over a cluster of independent compute hosts called worker nodes.
{:shortdesc}

This tutorial series demonstrates how a fictional public relations firm can use Kubernetes to deploy a containerized app in {{site.data.keyword.Bluemix_short}} that leverages {{site.data.keyword.watson}} Tone Analyzer. The PR firm uses {{site.data.keyword.watson}} Tone Analyzer to analyze their press releases and receive feedback on the tone in their messages. In this first tutorial, the PR firm's networking administrator sets up a custom Kubernetes cluster that is the compute infrastructure for the firm's {{site.data.keyword.watson}} Tone Analyzer app. This cluster is used to deploy and test a Hello World version of the PR firm's app.

## Objectives

-   Create a Kubernetes cluster with one worker node
-   Install the CLIs for using the Kubernetes API and managing Docker images
-   Create a private image repository in {{site.data.keyword.registrylong}} to store your images
-   Add the Watson Tone Analyzer {{site.data.keyword.Bluemix_notm}} service to the cluster so that any app in the cluster can use the service

## Time required

25 minutes

## Audience

This tutorial is intended for software developers and network administrators who have never created a Kubernetes cluster before.

## Lesson 1: Setting up the CLI
{: #cs_cluster_tutorial_lesson1}

Install the {{site.data.keyword.containershort_notm}} CLI, the {{site.data.keyword.registryshort_notm}} CLI, and their prerequisites. These CLIs are used in later lessons and are required to manage your Kubernetes cluster from your local machine, create images to deploy as containers, and in a later tutorial, deploy apps into the cluster.

This lesson includes the information for installing the following CLIs.

-   {{site.data.keyword.Bluemix_notm}} CLI
-   {{site.data.keyword.containershort_notm}} plug-in
-   Kubernetes CLI
-   {{site.data.keyword.registryshort_notm}} plug-in
-   Docker CLI


To install the CLIs:
1.  If you do not have one yet, create a [{{site.data.keyword.Bluemix_notm}} account ![External link icon](../icons/launch-glyph.svg "External link icon")](https://console.ng.bluemix.net/registration/). Make note of your user name and password as this information is required later.
2.  As a prerequisite for the {{site.data.keyword.containershort_notm}} plug-in, install the [{{site.data.keyword.Bluemix_notm}} CLI ![External link icon](../icons/launch-glyph.svg "External link icon")](https://clis.ng.bluemix.net/ui/home.html). The prefix for running commands by using the {{site.data.keyword.Bluemix_notm}} CLI is `bx`.
3.  Follow the prompts to select an account and a {{site.data.keyword.Bluemix_notm}} organization. Clusters are specific to an account, but are independent from a {{site.data.keyword.Bluemix_notm}} organization or space.

5.  To create Kubernetes clusters and manage worker nodes, install the {{site.data.keyword.containershort_notm}} plug-in. The prefix for running commands by using the {{site.data.keyword.containershort_notm}} plug-in is `bx cs`.

    ```
    bx plugin install container-service -r {{site.data.keyword.Bluemix_notm}}
    ```
    {: pre}
6.  Log in to the {{site.data.keyword.Bluemix_notm}} CLI. Enter your {{site.data.keyword.Bluemix_notm}} credentials when prompted.

    ```
    bx login
    ```
    {: pre}

    To specify a specific {{site.data.keyword.Bluemix_notm}} region, include the API endpoint. If you have private Docker images that are stored in the container registry of a specific {{site.data.keyword.Bluemix_notm}} region, or {{site.data.keyword.Bluemix_notm}} services instances that you already created, log in to this region to access your images and {{site.data.keyword.Bluemix_notm}} services.

    The {{site.data.keyword.Bluemix_notm}} region that you log in to also determines the region where you can create Kubernetes clusters, including the available datacenters. If you do not specify a region, you are automatically logged in to the region that is closest to you.

       -  US South and US East

           ```
           bx login -a api.ng.bluemix.net
           ```
           {: pre}

       -  Sydney

           ```
           bx login -a api.au-syd.bluemix.net
           ```
           {: pre}

       -  Germany

           ```
           bx login -a api.eu-de.bluemix.net
           ```
           {: pre}

       -  United Kingdom

           ```
           bx login -a api.eu-gb.bluemix.net
           ```
           {: pre}
           

    **Note:** If you have a federated ID, use `bx login --sso` to log in to the {{site.data.keyword.Bluemix_notm}} CLI. Enter your user name and use the provided URL in your CLI output to retrieve your one-time passcode. You know you have a federated ID when the login fails without the `--sso` and succeeds with the `--sso` option.

7.  If you want to create a Kubernetes cluster in a region other than the {{site.data.keyword.Bluemix_notm}} region that you selected earlier, specify this region. For example, you created {{site.data.keyword.Bluemix_notm}} services or private Docker images in one region and want to use them with {{site.data.keyword.containershort_notm}} in another region.

**Note**: If you want to create a cluster in US East, you must log in to the US East container region API endpoint using the `bx cs init --host https://us-east.containers.bluemix.net` command.

    Choose between the following API endpoints:

    * US-South:

        ```
        bx cs init --host https://us-south.containers.bluemix.net
        ```
        {: pre}

    * US-East:

        ```
        bx cs init --host https://us-east.containers.bluemix.net
        ```
        {: pre}

    * UK-South:

        ```
        bx cs init --host https://uk-south.containers.bluemix.net
        ```
        {: pre}

    * EU-Central:

        ```
        bx cs init --host https://eu-central.containers.bluemix.net
        ```
        {: pre}

    * AP-South:

        ```
        bx cs init --host https://ap-south.containers.bluemix.net
        ```
        {: pre}
        

8.  To view a local version of the Kubernetes dashboard and to deploy apps into your clusters, [install the Kubernetes CLI ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/tasks/tools/install-kubectl/). The prefix for running commands by using the Kubernetes CLI is `kubectl`.
    1.  Download the Kubernetes CLI.

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

        2.  Make sure that /usr/local/bin is listed in your `PATH` system variable. The `PATH` variable contains all directories where your operating system can find executable files. The directories that are listed in the `PATH` variable serve different purposes. /usr/local/bin is used to store executable files for software that is not part of the operating system and that was manually installed by the system administrator.

            ```
            echo $PATH
            ```
            {: pre}

            Your CLI output looks similar to the following.

            ```
            /usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin
            ```
            {: screen}

        3.  Convert the binary file to an executable file.

            ```
            chmod +x /usr/local/bin/kubectl
            ```
            {: pre}

9. To set up and manage a private image repository in {{site.data.keyword.registryshort_notm}}, install the {{site.data.keyword.registryshort_notm}} plug-in. The prefix for running registry commands is `bx cr`.

    ```
    bx plugin install container-registry -r {{site.data.keyword.Bluemix_notm}}
    ```
    {: pre}

    To verify that the container-service and container-registry plug-ins are installed properly, run the following command:

    ```
    bx plugin list
    ```
    {: pre}

10. To build images locally and push them to your private image repository, [install the Docker CE CLI ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.docker.com/community-edition#/download). If you are using Windows 8 or earlier, you can install the [Docker Toolbox ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.docker.com/products/docker-toolbox) instead.

Congratulations! You successfully created your {{site.data.keyword.Bluemix_notm}} account and installed the CLIs for the following lessons and tutorials. Next, access your cluster by using the CLI and start adding {{site.data.keyword.Bluemix_notm}} services.

## Lesson 2: Setting up your cluster environment
{: #cs_cluster_tutorial_lesson2}

Create your Kubernetes cluster, set up a private image repository in {{site.data.keyword.registryshort_notm}}, and add secrets to your cluster so that the app can access the {{site.data.keyword.watson}} {{site.data.keyword.toneanalyzerfull}} service.

1.  Create your lite Kubernetes cluster.

    ```
    bx cs cluster-create --name <pr_firm_cluster>
    ```
    {: pre}

    A lite cluster comes with one worker node to deploy container pods upon. A worker node is the compute host, typically a virtual machine, that your apps run on. An app in production runs replicas of the app across multiple worker nodes to provide higher availability for your app.

    **Note:** It can take up to 15 minutes for the worker node machine to be ordered and for the cluster to be set up and provisioned.

2.  Set up your own private image repository in {{site.data.keyword.registryshort_notm}} to securely store and share Docker images with all cluster users. A private image repository in {{site.data.keyword.Bluemix_notm}} is identified by a namespace that you set in this step. The namespace is used to create a unique URL to your image repository that developers can use to access private Docker images. You can create multiple namespaces in your account to group and organize your images. For example, you can create a namespace for every department, environment, or app.

    In this example, the PR firm wants to create only one image repository in {{site.data.keyword.registryshort_notm}}, so they choose _pr_firm_ as their namespace to group all images in their account. Replace _&lt;your_namespace&gt;_ with a namespace of your choice and not something that is related to the tutorial.

    ```
    bx cr namespace-add <your_namespace>
    ```
    {: pre}

3.  Before you continue to the next step, verify that the deployment of your worker node is complete.

    ```
    bx cs workers <pr_firm_cluster>
    ```
     {: pre}

    When the provisioning of your worker node is completed, the status changes to **Ready** and you can start binding {{site.data.keyword.Bluemix_notm}} services to use in a future tutorial.

    ```
    ID                                                 Public IP       Private IP       Machine Type   State    Status
    kube-dal10-pafe24f557f070463caf9e31ecf2d96625-w1   169.48.131.37   10.177.161.132   free           normal    Ready
    ```
    {: screen}

4.  Set the context for your cluster in your CLI. Every time you log in to the container CLI to work with clusters, you must run these commands to set the path to the cluster's configuration file as a session variable. The Kubernetes CLI uses this variable to find a local configuration file and certificates that are necessary to connect with the cluster in {{site.data.keyword.Bluemix_notm}}.

    1.  Get the command to set the environment variable and download the Kubernetes configuration files.

        ```
        bx cs cluster-config pr_firm_cluster
        ```
        {: pre}

        When the download of the configuration files is finished, a command is displayed that you can use to set the path to the local Kubernetes configuration file as an environment variable.

        Example for OS X:

        ```
        export KUBECONFIG=/Users/<user_name>/.bluemix/plugins/container-service/clusters/pr_firm_cluster/kube-config-prod-dal10-pr_firm_cluster.yml
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
        /Users/<user_name>/.bluemix/plugins/container-service/clusters/pr_firm_cluster/kube-config-prod-dal10-pr_firm_cluster.yml
        ```
        {: screen}

    4.  Verify that the `kubectl` commands run properly with your cluster by checking the Kubernetes CLI server version.

        ```
        kubectl version  --short
        ```
        {: pre}

        Example output:

        ```
        Client Version: v1.7.4
        Server Version: v1.7.4
        ```
        {: screen}

5.  Add the {{site.data.keyword.watson}} {{site.data.keyword.toneanalyzershort}} {{site.data.keyword.Bluemix_notm}} service to the cluster. With {{site.data.keyword.Bluemix_notm}} services, such as the {{site.data.keyword.watson}} {{site.data.keyword.toneanalyzershort}}, you can take advantage of already developed functionality in your apps. Any {{site.data.keyword.Bluemix_notm}} service that is bound to the cluster can be used by any app that is deployed in that cluster. Repeat the following steps for every {{site.data.keyword.Bluemix_notm}} service that you want to use with your apps.
    1.  Add the {{site.data.keyword.watson}} {{site.data.keyword.toneanalyzershort}} service to your {{site.data.keyword.Bluemix_notm}} account.

        **Note:** When you add the {{site.data.keyword.watson}} {{site.data.keyword.toneanalyzershort}} service to your account, a message is displayed that the service is not free. If you limit your API call, this tutorial does not incur charges from the {{site.data.keyword.watson}} service. You can [review the pricing information for the {{site.data.keyword.watson}} {{site.data.keyword.toneanalyzershort}} ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/watson/developercloud/tone-analyzer.html#pricing-block).

        ```
        bx service create tone_analyzer standard <mytoneanalyzer>
        ```
        {: pre}

    2.  Bind the {{site.data.keyword.watson}} {{site.data.keyword.toneanalyzershort}} instance to the `default` Kubernetes namespace for the cluster. Later, you can create your own namespaces to manage user access to Kubernetes resources, but for now, use the `default` namespace. Kubernetes namespaces are different from the registry namespace you created earlier.

        ```
        bx cs cluster-service-bind <pr_firm_cluster> default <mytoneanalyzer>
        ```
        {: pre}

        Output:

        ```
        bx cs cluster-service-bind <pr_firm_cluster> default <mytoneanalyzer>
        Binding service instance to namespace...
        OK
        Namespace: default
        Secret name: binding-mytoneanalyzer
        ```
        {: screen}

6.  Verify that the Kubernetes secret was created in your cluster namespace. Every {{site.data.keyword.Bluemix_notm}} service is defined by a JSON file that includes confidential information about the service, such as the user name, password and URL that the container uses to access the service. To securely store this information, Kubernetes secrets are used. In this example, the secret includes the credentials for accessing the instance of the {{site.data.keyword.watson}} {{site.data.keyword.toneanalyzershort}} instance that is provisioned in your {{site.data.keyword.Bluemix_notm}} account.

    ```
    kubectl get secrets --namespace=default
    ```
    {: pre}

    Output:

    ```
    NAME                       TYPE                                  DATA      AGE
    binding-mytoneanalyzer     Opaque                                1         1m
    bluemix-default-secret     kubernetes.io/dockercfg               1         1h
    default-token-kf97z        kubernetes.io/service-account-token   3         1h
    ```
    {: screen}


Great work! The cluster is created, configured, and your local environment is ready for you to deploy apps into the cluster.

## What's next?

* [Test your knowledge and take a quiz! ![External link icon](../icons/launch-glyph.svg "External link icon")](https://bluemix-quizzes.mybluemix.net/containers/cluster_tutorial/quiz.php)

* Try the [Tutorial: Deploying apps into Kubernetes clusters in {{site.data.keyword.containershort_notm}}](cs_tutorials_apps.html#cs_apps_tutorial) to deploy the PR firm's app into the cluster that you created.