---

copyright:
  years: 2014, 2018
lastupdated: "2017-04-16"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}


# Tutorial: Migrating an app from Cloud Foundry to a cluster
{: #cf_tutorial}

You can take an app that you deployed previously by using Cloud Foundry and deploy the same code in a container to a Kubernetes cluster in  {{site.data.keyword.containershort_notm}}.
{: shortdesc}


## Objectives

- Learn the general process of deploying apps in containers to a Kubernetes cluster.
- Create a Dockerfile from your app code to build a container image.
- Deploy a container from that image into a Kubernetes cluster.

## Time required
30 minutes

## Audience
This tutorial is intended for Cloud Foundry app developers.

## Prerequisites

- [Create a private image registry in {{site.data.keyword.registrylong_notm}}](../services/Registry/index.html).
- [Create a cluster](cs_clusters.html#clusters_ui).
- [Target your CLI to the cluster](cs_cli_install.html#cs_cli_configure).
- [Learn about Docker and Kubernetes terminology](cs_tech.html).


<br />



## Lesson 1: Download app code

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

    b. Enter the app name `cf-py-<myinitials>` and click **CREATE**. To access the app code for the boilerplate, you must deploy the CF app to the cloud first. You can use any name for the app. If you use the name from the example, replace `<myinitials>` with your initials, such as `cf-py-ms`, to ensure that the name is unique.

    As the app is deployed, instructions for "Download, modify, and redeploy your app with the command line interface" are displayed.

    c. From step 1 in the GUI instructions, click **DOWNLOAD STARTER CODE**.

    d. Extract the .zip file and save its contents to your `cf-py` directory.

Your app code is ready to be containerized!


<br />


## Lesson 2: Creating a Docker image with your app code

Create a Dockerfile that includes your app code and the necessary configurations for your container. Then, build a Docker image from that Dockerfile and push it to your private image registry.
{: shortdesc}

1. In the `cf-py` directory that you created in the previous lesson, create a `Dockerfile`, which is the basis for creating a container image. You can create the Dockerfile by using your preferred CLI editor or a text editor on your computer. The following example shows how to create a Dockerfile file with the nano editor.

  ```
  nano Dockerfile
  ```
  {: pre}

2. Copy the following script into the Dockerfile. This Dockerfile applies specifically to a Python app. If you are using another type of code, your Dockerfile must include a different base image and might require other fields to be defined.

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
  bx cr build -t registry.<region>.bluemix.net/namespace/cf-py .
  ```
  {: pre}

  <table>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="This icon indicates there is more information to learn about this command's components."/> Understanding this command's components</th>
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
  <td>Your private registry path, which includes your unique namespace and the name of the image. For this example, the same name is used for the image as the app directory, but you can choose any name for the image in your private registry. If you are unsure what your namespace is, run the `bx cr namespaces` command to find it.</td>
  </tr>
  <tr>
  <td><code>.</code></td>
  <td>The location of the Dockerfile. If you are running the build command from the directory that includes the Dockerfile, enter a period (.). Otherwise, use the relative path to the Dockerfile.</td>
  </tr>
  </table>

  The image is created in your private registry. You can run the `bx cr images` command to verify that the image was created.

  ```
  REPOSITORY                                     NAMESPACE   TAG      DIGEST         CREATED         SIZE     VULNERABILITY STATUS   
  registry.ng.bluemix.net/namespace/cf-py        namespace   latest   cb03170b2cb2   3 minutes ago   271 MB   OK
  ```
  {: screen}


<br />



## Lesson 3: Deploying a container from your image

Deploy your app as a container in a Kubernetes cluster.
{: shortdesc}

1. Create a configuration YAML file that is named `cf-py.yaml` and update `<registry_namespace>` with the name of your private image registry. This configuration file defines a container deployment from the image that you created in the previous lesson and a service to expose the app to the public.

  ```
  apiVersion: extensions/v1beta1
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
        - image: registry.ng.bluemix.net/<registry_namespace>/cf-py:latest
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

  <table>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the YAML file components</th>
  </thead>
  <tbody>
  <tr>
  <td><code>image</code></td>
  <td>In `registry.ng.bluemix.net/<registry_namespace>/cf-py:latest`, replace &lt;registry_namespace&gt; with the namespace of your private image registry. If you are unsure what your namespace is, run the `bx cr namespaces` command to find it.</td>
  </tr>
  <tr>
  <td><code>nodePort</code></td>
  <td>Expose your app by creating a Kubernetes service of type NodePort. NodePorts are in the range of 30000 - 32767. You use this port to test your app in a browser later.</td>
  </tr>
  </tbody></table>

2. Apply the configuration file to create the deployment and the service in your cluster.

  ```
  kubectl apply -f filepath/cf-py.yaml
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
    bx cs workers <cluster_name>
    ```
    {: pre}

    Output:

    ```
    ID                                                 Public IP        Private IP     Machine Type        State    Status   Zone    Version   
    kube-dal10-cr18e61e63c6e94b658596ca93d087eed9-w1   169.xx.xxx.xxx   10.xxx.xx.xxx   u2c.2x4.encrypted   normal   Ready    dal10   1.8.8
    ```
    {: screen}

    b. Open a browser and check out the app with the following URL: `http://<public_IP_address>:<NodePort>`. With the example values, the URL is `http://169.xx.xxx.xxx:30872`. You can give this URL to a co-worker to try or enter it in your cell phone's browser, so that you can see that the app really is publicly available.

    <img src="images/python_flask.png" alt="A screenshot of the deployed boilerplate Python Flask app." />

5. [Launch the Kubernetes dashboard](cs_app.html#cli_dashboard). Note that the steps differ depending on your version of Kubernetes.

6. In the **Workloads** tab, you can see the resources that you created. When you are done exploring the Kubernetes dashboard, use `ctrl + c` to exit the `proxy` command.

Congratulations! Your app is deployed in a container!
