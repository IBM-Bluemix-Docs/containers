---

copyright:
  years: 2014, 2020
lastupdated: "2020-11-16"

keywords: kubernetes, iks

subcollection: containers

content-type: tutorial
account-plan:
completion-time: 30m

---

{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}
{:android: data-hd-operatingsystem="android"}
{:apikey: data-credential-placeholder='apikey'}
{:app_key: data-hd-keyref="app_key"}
{:app_name: data-hd-keyref="app_name"}
{:app_secret: data-hd-keyref="app_secret"}
{:app_url: data-hd-keyref="app_url"}
{:authenticated-content: .authenticated-content}
{:beta: .beta}
{:c#: data-hd-programlang="c#"}
{:codeblock: .codeblock}
{:curl: .ph data-hd-programlang='curl'}
{:deprecated: .deprecated}
{:dotnet-standard: .ph data-hd-programlang='dotnet-standard'}
{:download: .download}
{:external: target="_blank" .external}
{:faq: data-hd-content-type='faq'}
{:fuzzybunny: .ph data-hd-programlang='fuzzybunny'}
{:generic: data-hd-operatingsystem="generic"}
{:generic: data-hd-programlang="generic"}
{:gif: data-image-type='gif'}
{:go: .ph data-hd-programlang='go'}
{:help: data-hd-content-type='help'}
{:hide-dashboard: .hide-dashboard}
{:hide-in-docs: .hide-in-docs}
{:important: .important}
{:ios: data-hd-operatingsystem="ios"}
{:java: #java .ph data-hd-programlang='java'}
{:java: .ph data-hd-programlang='java'}
{:java: data-hd-programlang="java"}
{:javascript: .ph data-hd-programlang='javascript'}
{:javascript: data-hd-programlang="javascript"}
{:new_window: target="_blank"}
{:note .note}
{:note: .note}
{:objectc data-hd-programlang="objectc"}
{:org_name: data-hd-keyref="org_name"}
{:php: data-hd-programlang="php"}
{:pre: .pre}
{:preview: .preview}
{:python: .ph data-hd-programlang='python'}
{:python: data-hd-programlang="python"}
{:route: data-hd-keyref="route"}
{:row-headers: .row-headers}
{:ruby: .ph data-hd-programlang='ruby'}
{:ruby: data-hd-programlang="ruby"}
{:runtime: architecture="runtime"}
{:runtimeIcon: .runtimeIcon}
{:runtimeIconList: .runtimeIconList}
{:runtimeLink: .runtimeLink}
{:runtimeTitle: .runtimeTitle}
{:screen: .screen}
{:script: data-hd-video='script'}
{:service: architecture="service"}
{:service_instance_name: data-hd-keyref="service_instance_name"}
{:service_name: data-hd-keyref="service_name"}
{:shortdesc: .shortdesc}
{:space_name: data-hd-keyref="space_name"}
{:step: data-tutorial-type='step'}
{:subsection: outputclass="subsection"}
{:support: data-reuse='support'}
{:swift: #swift .ph data-hd-programlang='swift'}
{:swift: .ph data-hd-programlang='swift'}
{:swift: data-hd-programlang="swift"}
{:table: .aria-labeledby="caption"}
{:term: .term}
{:tip: .tip}
{:tooling-url: data-tooling-url-placeholder='tooling-url'}
{:troubleshoot: data-hd-content-type='troubleshoot'}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:tsSymptoms: .tsSymptoms}
{:tutorial: data-hd-content-type='tutorial'}
{:unity: .ph data-hd-programlang='unity'}
{:url: data-credential-placeholder='url'}
{:user_ID: data-hd-keyref="user_ID"}
{:vb.net: .ph data-hd-programlang='vb.net'}
{:video: .video}



# Migrating an app from Cloud Foundry to a cluster
{: #cf_tutorial}
{: toc-content-type="tutorial"}
{: toc-services="containers"}
{: toc-completion-time="30m"}

Take an app that you deployed previously by using Cloud Foundry and deploy the same code in a container to an {{site.data.keyword.containerlong_notm}}.
{: shortdesc}


## Objectives
{: #cf_objectives}

- Learn the general process of deploying apps in containers to a Kubernetes cluster.
- Create a Dockerfile from your app code to build a container image.
- Deploy a container from that image into a Kubernetes cluster.

## Audience
{: #cf_audience}

This tutorial is intended for Cloud Foundry app developers.

## Prerequisites
{: #cf_prereqs}

- [Create a private image registry in {{site.data.keyword.registrylong_notm}}](/docs/Registry?topic=Registry-getting-started).
- [Create a cluster](/docs/containers?topic=containers-clusters#clusters_ui).
- [Target your CLI to the cluster](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure).
- Ensure you have the following {{site.data.keyword.cloud_notm}} IAM access policies for {{site.data.keyword.containerlong_notm}}:
    - [Any platform role](/docs/containers?topic=containers-users#platform)
    - The [**Writer** or **Manager** service role](/docs/containers?topic=containers-users#platform)
- [Learn about Docker and Kubernetes terminology](/docs/containers?topic=containers-service-arch).


<br />


## Download app code
{: #cf_1}
{: step}

Get your code ready to go. Don't have any code yet? You can download starter code to use in this tutorial.
{: shortdesc}

1. Create a directory that is named `cf-py` and navigate into it. In this directory, you save all the files that are required to build the Docker image and to run your app.

  ```
  mkdir cf-py && cd cf-py
  ```
  {: pre}

2. Copy the app code and all related files into the directory. You can use your own app code or download boilerplate from the catalog. This tutorial uses the Python Flask boilerplate. However, you can use the same basic steps with a Node.js, Java, or [Kitura](https://github.com/IBM-Cloud/Kitura-Starter) app.

    To download the Python Flask app code:

    a. In the catalog, in **Boilerplates**, click **Python Flask**. This boilerplate includes a runtime environment for both Python 2 and Python 3 apps.

    b. Enter the app name `cf-py-<name>` and click **CREATE**. To access the app code for the boilerplate, you must deploy the CF app to the cloud first. You can use any name for the app. If you use the name from the example, replace `<name>` with a unique identifier, such as `cf-py-msx`.

    **Attention**: Do not use personal information in any app, container image, or Kubernetes resource names.

    As the app is deployed, instructions for "Download, modify, and redeploy your app with the command line interface" are displayed.

    c. From step 1 in the console instructions, click **DOWNLOAD STARTER CODE**.

    d. Extract the `.zip` file and save its contents to your `cf-py` directory.

Your app code is ready to be containerized!


<br />


## Creating a Docker image with your app code
{: #cf_2}
{: step}

Create a Dockerfile that includes your app code and the necessary configurations for your container. Then, build a Docker image from that Dockerfile and push it to your private image registry.
{: shortdesc}

1. In the `cf-py` directory that you created in the previous lesson, create a `Dockerfile`, which is the basis for creating a container image. You can create the Dockerfile by using your preferred CLI editor or a text editor on your computer. The following example shows how to create a Dockerfile file with the nano editor.

  ```
  nano Dockerfile
  ```
  {: pre}

2. Copy the following script into the Dockerfile. This Dockerfile applies specifically to a Python app. If you are using another type of code, your Dockerfile must include a different base image and might require other fields to be defined. You can use [Konveyor Move2Kube](https://konveyor.io/move2kube/) to autogenerate Dockerfiles for your application.

  ```
  #Use the Python image from DockerHub as a base image
  FROM python:3-slim

  #Expose the port for your python app
  EXPOSE 5000

  #Copy all app files from the current directory into the app
  #directory in your container. Set the app directory
  #as the working directory
  WORKDIR /cf-py/
  COPY .  .

  #Install any requirements that are defined
  RUN pip install --no-cache-dir -r requirements.txt

  #Update the openssl package
  RUN apt-get update && apt-get install -y \
  openssl

  #Start the app.
  CMD ["python", "welcome.py"]
  ```
  {: codeblock}

3. Save your changes in the nano editor by pressing `ctrl + o`. Confirm your changes by pressing `enter`. Exit the nano editor by pressing `ctrl + x`.

4. Build a Docker image that includes your app code and push it to your private registry.

  ```
  ibmcloud cr build -t <region>.icr.io/namespace/cf-py .
  ```
  {: pre}

  <table summary="The columns are read from left to right. The first column has the parameter of the command. The second column describes the parameter.">
  <caption>Understanding this command's components</caption>
  <col width="15%">
  <thead>
  <th>Parameter</th>
  <th>Description</th>
  </thead>
  <tbody>
  <tr>
  <td>Parameter</td>
  <td>Description</td>
  </tr>
  <tr>
  <td><code>build</code></td>
  <td>The build command.</td>
  </tr>
  <tr>
  <td><code>-t registry.&lt;region&gt;.bluemix.net/namespace/cf-py</code></td>
  <td>Your private registry path, which includes your unique namespace and the name of the image. For this example, the same name is used for the image as the app directory, but you can choose any name for the image in your private registry. If you are unsure what your namespace is, run the `ibmcloud cr namespaces` command to find it.</td>
  </tr>
  <tr>
  <td><code>.</code></td>
  <td>The location of the Dockerfile. If you are running the build command from the directory that includes the Dockerfile, enter a period (.). Otherwise, use the relative path to the Dockerfile.</td>
  </tr>
  </tbody>
  </table>

  The image is created in your private registry. You can run the `ibmcloud cr images` command to verify that the image was created.

  ```
  REPOSITORY                       NAMESPACE   TAG      DIGEST         CREATED         SIZE     VULNERABILITY STATUS   
  us.icr.io/namespace/cf-py        namespace   latest   cb03170b2cb2   3 minutes ago   271 MB   OK
  ```
  {: screen}


<br />


## Deploying a container from your image
{: #cf_3}
{: step}

Deploy your app as a container in a Kubernetes cluster.
{: shortdesc}

1. Create a configuration YAML file that is named `cf-py.yaml` and update `<registry_namespace>` with the name of your private image registry. This configuration file defines a container deployment from the image that you created in the previous lesson and a service to expose the app to the public. You can use [Konveyor Move2Kube](https://konveyor.io/move2kube/) to autogenerate configuration files for your application.

  ```
  apiVersion: apps/v1
  kind: Deployment
  metadata:
    labels:
      app: cf-py
    name: cf-py
    namespace: default
  spec:
    selector:
      matchLabels:
        app: cf-py
    replicas: 1
    template:
      metadata:
        labels:
          app: cf-py
      spec:
        containers:
        - image: us.icr.io/<registry_namespace>/cf-py:latest
          name: cf-py
  ---
  apiVersion: v1
  kind: Service
  metadata:
    name: cf-py-nodeport
    labels:
      app: cf-py
  spec:
    selector:
      app: cf-py
    type: NodePort
    ports:
     - port: 5000
       nodePort: 30872
  ```
  {: codeblock}

  <table summary="The columns are read from left to right. The first column has the parameter of the YAML file. The second column describes the parameter.">
  <caption>Understanding the YAML file components</caption>
  <col width="15%">
  <thead>
  <th>Parameter</th>
  <th>Description</th>
  </thead>
  <tbody>
  <tr>
  <td><code>image</code></td>
  <td>In `us.icr.io/<registry_namespace>/cf-py:latest`, replace &lt;registry_namespace&gt; with the namespace of your private image registry. If you are unsure what your namespace is, run the `ibmcloud cr namespaces` command to find it.</td>
  </tr>
  <tr>
  <td><code>nodePort</code></td>
  <td>Expose your app by creating a Kubernetes service of type NodePort. NodePorts are in the range of 30000 - 32767. You use this port to test your app in a browser later.</td>
  </tr>
  </tbody></table>

2. Apply the configuration file to create the deployment and the service in your cluster.

  ```
  kubectl apply -f <filepath>/cf-py.yaml
  ```
  {: pre}

  Output:

  ```
  deployment "cf-py" configured
  service "cf-py-nodeport" configured
  ```
  {: screen}

3. Now that all the deployment work is done, you can test your app in a browser. Get the details to form the URL.

    a.  Get the public IP address for the worker node in the cluster.

    ```
    ibmcloud ks worker ls --cluster <cluster_name>
    ```
    {: pre}

    Output:

    ```
    ID                                                 Public IP        Private IP     Machine Type        State    Status   Zone    Version   
    kube-dal10-cr18e61e63c6e94b658596ca93d087eed9-w1   169.xx.xxx.xxx   10.xxx.xx.xxx   u3c.2x4.encrypted   normal   Ready    dal10   1.18.12
    ```
    {: screen}

    b. Open a browser and check out the app with the following URL: `http://<public_IP_address>:<NodePort>`. With the example values, the URL is `http://169.xx.xxx.xxx:30872`. You can give this URL to a co-worker to try or enter it in your cell phone's browser so that you can see that the app really is publicly available. **Note**: Because worker nodes in VPC clusters do not have a public IP address, you can access an app through a NodePort only if you are connected to your private VPC network, such as through a VPN connection or by using the [Kubernetes web terminal](/docs/containers?topic=containers-cs_cli_install#cli_web). Then, you can use the worker node's private IP address and NodePort: `http://<private_IP_address>:<NodePort>`.

    <img src="images/python_flask.png" alt="A screen capture of the deployed boilerplate Python Flask app." />

5.  [Launch the Kubernetes dashboard](/docs/containers?topic=containers-deploy_app#cli_dashboard).

    If you select your cluster in the [{{site.data.keyword.cloud_notm}} console](https://cloud.ibm.com/), you can use the **Kubernetes Dashboard** button to launch your dashboard with one click.
    {: tip}

6. In the **Workloads** tab, you can see the resources that you created.

Good job! Your app is deployed in a container!
