---

copyright:
  years: 2014, 2017
lastupdated: "2017-10-16"

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

Deploy and manage your own Kubernetes cluster in the cloud. You can automate the deployment, operation, scaling, and monitoring of containerized apps in a cluster of independent compute hosts called worker nodes.
{:shortdesc}

In this tutorial series, you can see how a fictional public relations firm uses Kubernetes to deploy a containerized app in {{site.data.keyword.Bluemix_short}}. Leveraging {{site.data.keyword.toneanalyzerfull}}, the PR firm analyzes their press releases and receives feedback.


## Objectives

In this first tutorial, you act as the PR firm's networking administrator. You configure a custom Kubernetes cluster that is used to deploy and test a Hello World version of the app.

To set up the infrastructure:

-   Create a Kubernetes cluster with one worker node
-   Install the CLIs for using the Kubernetes API and managing Docker images
-   Create a private image repository in {{site.data.keyword.registrylong_notm}} to store your images
-   Add the {{site.data.keyword.toneanalyzershort}} service to the cluster so that any app in the cluster can use the service


## Time required

40 minutes


## Audience

This tutorial is intended for software developers and network administrators who have never created a Kubernetes cluster before.


## Prerequisites

-  An [{{site.data.keyword.Bluemix_notm}} account ![External link icon](../icons/launch-glyph.svg "External link icon")](https://console.bluemix.net/registration/)



## Lesson 1: Creating a cluster and setting up the CLI
{: #cs_cluster_tutorial_lesson1}

Create your cluster in the GUI and install the required CLIs. For this tutorial, create your cluster in the UK South region.


To create your cluster:

1. It can take a few minutes to provision your cluster. To make the most of your time, [create your cluster ![External link icon](../icons/launch-glyph.svg "External link icon")](https://console.bluemix.net/containers-kubernetes/launch?env_id=ibm:yp:united-kingdom) before installing the CLIs. A lite cluster comes with one worker node in which to deploy container pods. A worker node is the compute host, typically a virtual machine, on which your apps run.


The following CLIs and their prerequisites are used to manage clusters through the CLI:
-   {{site.data.keyword.Bluemix_notm}} CLI
-   {{site.data.keyword.containershort_notm}} plug-in
-   Kubernetes CLI
-   {{site.data.keyword.registryshort_notm}} plug-in
-   Docker CLI

</br>
To install the CLIs:

1.  As a prerequisite for the {{site.data.keyword.containershort_notm}} plug-in, install the [{{site.data.keyword.Bluemix_notm}} CLI ![External link icon](../icons/launch-glyph.svg "External link icon")](https://clis.ng.bluemix.net/ui/home.html). To run {{site.data.keyword.Bluemix_notm}} CLI commands, use the prefix `bx`.
2.  Follow the prompts to select an account and an {{site.data.keyword.Bluemix_notm}} organization. Clusters are specific to an account, but are independent from an {{site.data.keyword.Bluemix_notm}} organization or space.

4.  Install the {{site.data.keyword.containershort_notm}} plug-in to create Kubernetes clusters and manage worker nodes. To run {{site.data.keyword.containershort_notm}} plug-in commands, use the prefix `bx cs`.

    ```
    bx plugin install container-service -r {{site.data.keyword.Bluemix_notm}}
    ```
    {: pre}

5.  To view a local version of the Kubernetes dashboard and deploy apps into your clusters, [install the Kubernetes CLI ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/tasks/tools/install-kubectl/). To run commands by using the Kubernetes CLI, use the prefix `kubectl`.
    1.  For complete functional compatibility, download the Kubernetes CLI version that matches the Kubernetes cluster version you plan to use. The current {{site.data.keyword.containershort_notm}} default Kubernetes version is 1.7.4.

        OS X:   [https://storage.googleapis.com/kubernetes-release/release/v1.7.4/bin/darwin/amd64/kubectl ![External link icon](../icons/launch-glyph.svg "External link icon")](https://storage.googleapis.com/kubernetes-release/release/v1.7.4/bin/darwin/amd64/kubectl)

        Linux:   [https://storage.googleapis.com/kubernetes-release/release/v1.7.4/bin/linux/amd64/kubectl ![External link icon](../icons/launch-glyph.svg "External link icon")](https://storage.googleapis.com/kubernetes-release/release/v1.7.4/bin/linux/amd64/kubectl)

        Windows:   [https://storage.googleapis.com/kubernetes-release/release/v1.7.4/bin/windows/amd64/kubectl.exe ![External link icon](../icons/launch-glyph.svg "External link icon")](https://storage.googleapis.com/kubernetes-release/release/v1.7.4/bin/windows/amd64/kubectl.exe)

          **Tip:** If you are using Windows, install the Kubernetes CLI in the same directory as the {{site.data.keyword.Bluemix_notm}} CLI. This setup saves you some filepath changes when you run commands later.

    2.  If you're using OS X or Linux, complete the following steps.
        1.  Move the executable file to the `/usr/local/bin` directory.

            ```
            mv /<path_to_file>/kubectl /usr/local/bin/kubectl
            ```
            {: pre}

        2.  Be sure that /usr/local/bin is listed in your `PATH` system variable. The `PATH` variable contains all directories where your operating system can find executable files. The directories that are listed in the `PATH` variable serve different purposes. `/usr/local/bin` is used to store executable files for software that is not part of the operating system and that was manually installed by the system administrator.

            ```
            echo $PATH
            ```
            {: pre}

            Your CLI output looks similar to the following.

            ```
            /usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin
            ```
            {: screen}

        3.  Make the file executable.

            ```
            chmod +x /usr/local/bin/kubectl
            ```
            {: pre}

6. To set up and manage a private image repository in {{site.data.keyword.registryshort_notm}}, install the {{site.data.keyword.registryshort_notm}} plug-in. To run registry commands, use the prefix `bx cr`.

    ```
    bx plugin install container-registry -r {{site.data.keyword.Bluemix_notm}}
    ```
    {: pre}

    To verify that the container-service and container-registry plug-ins are properly installed, run the following command:

    ```
    bx plugin list
    ```
    {: pre}

7. To build images locally and push them to your private image repository, [install the Docker CE CLI ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.docker.com/community-edition#/download). If you are using Windows 8 or earlier, you can install the [Docker Toolbox ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.docker.com/products/docker-toolbox) instead.

Congratulations! You've successfully installed the CLIs for the following lessons and tutorials. Next, set up your cluster environment and add the {{site.data.keyword.toneanalyzershort}} service.


## Lesson 2: Setting up your cluster environment
{: #cs_cluster_tutorial_lesson2}

Create your Kubernetes cluster, set up a private image repository in {{site.data.keyword.registryshort_notm}}, and add secrets to your cluster so that the app can access the {{site.data.keyword.toneanalyzershort}} service.

1.  Log in to the {{site.data.keyword.Bluemix_notm}} CLI by using your {{site.data.keyword.Bluemix_notm}} credentials, when prompted.

    ```
    bx login [--sso] -a api.eu-gb.bluemix.net
    ```
    {: pre}

    **Note:** If you have a federated ID, use the `--sso` flag to log in. Enter your user name and use the provided URL in your CLI output to retrieve your one-time passcode.

2.  Set up your own private image repository in {{site.data.keyword.registryshort_notm}} to securely store and share Docker images with all cluster users. A private image repository in {{site.data.keyword.Bluemix_notm}} is identified by a namespace. The namespace is used to create a unique URL to your image repository that developers can use to access private Docker images.

    In this example, the PR firm wants to create only one image repository in {{site.data.keyword.registryshort_notm}}, so they choose _pr_firm_ as their namespace to group all images in their account. Replace _&lt;your_namespace&gt;_ with a namespace of your choice and not something that is related to the tutorial.

    ```
    bx cr namespace-add <your_namespace>
    ```
    {: pre}

3.  Before you continue to the next step, verify that the deployment of your worker node is complete.

    ```
    bx cs workers <cluster_name>
    ```
     {: pre}

    When the provisioning of your worker node is completed, the status changes to **Ready** and you can start binding {{site.data.keyword.Bluemix_notm}} services to use in a future tutorial.

    ```
    ID                                                 Public IP       Private IP       Machine Type   State    Status
    kube-par02-pafe24f557f070463caf9e31ecf2d96625-w1   169.48.131.37   10.177.161.132   free           normal    Ready
    ```
    {: screen}

4.  Set the context for your cluster in your CLI. Every time you log in to the container CLI to work with clusters, you must run these commands to set the path to the cluster's configuration file as a session variable. The Kubernetes CLI uses this variable to find a local configuration file and certificates that are necessary to connect with the cluster in {{site.data.keyword.Bluemix_notm}}.

    1.  Get the command to set the environment variable and download the Kubernetes configuration files.

        ```
        bx cs cluster-config <cluster_name>
        ```
        {: pre}

        When the download of the configuration files is finished, a command is displayed that you can use to set the path to the local Kubernetes configuration file as an environment variable.

        Example for OS X:

        ```
        export KUBECONFIG=/Users/<user_name>/.bluemix/plugins/container-service/clusters/pr_firm_cluster/kube-config-prod-par02-pr_firm_cluster.yml
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
        /Users/<user_name>/.bluemix/plugins/container-service/clusters/pr_firm_cluster/kube-config-prod-par02-pr_firm_cluster.yml
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

5.  Add the {{site.data.keyword.toneanalyzershort}} service to the cluster. With {{site.data.keyword.Bluemix_notm}} services, you can take advantage of already developed functionality in your apps. Any {{site.data.keyword.Bluemix_notm}} service that is bound to the cluster can be used by any app that is deployed in that cluster. Repeat the following steps for every {{site.data.keyword.Bluemix_notm}} service that you want to use with your apps.
    1.  Add the {{site.data.keyword.toneanalyzershort}} service to your {{site.data.keyword.Bluemix_notm}} account.

        **Note:** When you add the {{site.data.keyword.toneanalyzershort}} service to your account, a message is displayed that the service is not free. If you limit your API call, this tutorial does not incur charges from the {{site.data.keyword.watson}} service. [Review the pricing information for the {{site.data.keyword.watson}} {{site.data.keyword.toneanalyzershort}} service ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/watson/developercloud/tone-analyzer.html#pricing-block).

        ```
        bx service create tone_analyzer standard <mytoneanalyzer>
        ```
        {: pre}

    2.  Bind the {{site.data.keyword.toneanalyzershort}} instance to the `default` Kubernetes namespace for the cluster. Later, you can create your own namespaces to manage user access to Kubernetes resources, but for now, use the `default` namespace. Kubernetes namespaces are different from the registry namespace you created earlier.

        ```
        bx cs cluster-service-bind <cluster_name> default <mytoneanalyzer>
        ```
        {: pre}

        Output:

        ```
        bx cs cluster-service-bind <cluster_name> default <mytoneanalyzer>
        Binding service instance to namespace...
        OK
        Namespace:	default
        Secret name:	binding-mytoneanalyzer
        ```
        {: screen}

6.  Verify that the Kubernetes secret was created in your cluster namespace. Every {{site.data.keyword.Bluemix_notm}} service is defined by a JSON file that includes confidential information about the service, such as the user name, password and URL that the container uses to access the service. To securely store this information, Kubernetes secrets are used. In this example, the secret includes the credentials for accessing the instance of the {{site.data.keyword.watson}} {{site.data.keyword.toneanalyzershort}} that is provisioned in your account.

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

</br>
Great work! You've configured your cluster and your local environment is ready for you to start deploying apps into the cluster.

## What's next?

* [Test your knowledge and take a quiz! ![External link icon](../icons/launch-glyph.svg "External link icon")](https://ibmcloud-quizzes.mybluemix.net/containers/cluster_tutorial/quiz.php)

* Try the [Tutorial: Deploying apps into Kubernetes clusters in {{site.data.keyword.containershort_notm}}](cs_tutorials_apps.html#cs_apps_tutorial) to deploy the PR firm's app into the cluster that you created.
