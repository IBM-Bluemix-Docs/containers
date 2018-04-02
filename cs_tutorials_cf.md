<staging>
---

copyright:
  years: 2014, 2018
lastupdated: "2017-03-30"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}


# Tutorial: Deploying a Cloud Foundry Node.js app in a cluster
{: #cf_tutorial}

You can take an app that you deployed previously by using Cloud Foundry and deploy the same code in a container to a Kubernetes cluster in  {{site.data.keyword.containershort_notm}}.
{: shortdesc}


## Objectives

- Learn the general process of deploying containers to a Kubernetes cluster
- Learn what must be included in a Dockerfile to build a container image

## Time required
30 minutes

## Audience
Cloud Foundry app developers

## Prerequisites

- [Create a private image registry in IBM Cloud Container Registry](../services/Registry/index.html)
- [Create a cluster](cs_clusters.html#clusters_ui).
- [Target your CLI to the cluster](cs_cli_install.html#cs_cli_configure).


<br />



## Lesson 1: Download Cloud Foundry Node.js app code

Get your Node.js code ready to go. Don't have any code yet? You can download starter code to use in this tutorial.
{: shortdesc}

To get and organize the app code:

1. Create a directory that is named `cf-pi` and navigate into it. In this directory, you save all the files that are required to build the Docker image and to run your Node.js app. You can use any name for the directory. 

  ```
  mkdir cf-pi && cd cf-pi
  ```
  {: pre}

2. Copy the Node.js app code and all related files into the directory. You can use your own Node.js app code or download the Personal Insights app from Cloud. To download the Personal Insights app code, [clone the personality-insights-nodejs repository]( https://github.com/watson-developer-cloud/personality-insights-nodejs).
        
Your Cloud Foundry app code is ready to be containerized!


<br />


## Lesson 2: Creating a Docker image with your CF app code

Create a Dockerfile that includes your Node.js app code and the necessary configurations for your container. Then, build a Docker image from that Dockerfile and push it to your private Cloud registry.
{: shortdesc}

To create a Docker image with your app code, follow these steps.

1. In the `cf-pi` directory that you created in the previous lesson, create a `Dockerfile`, which is the basis for creating a container. You can create the Dockerfile by using your preferred CLI editor or a text editor on your computer. The following example shows how to create a Dockerfile file with the nano editor.

  ```
  nano Dockerfile
  ```
  {: pre}

2. Copy the following script into the Dockerfile.

  ```
  #Use the Node image from DockerHub as a base image
  FROM node:latest

  #Expose the port for your Personal Insights app, and set
  #it as an environment variable as expected by cf apps
  ENV PORT=3000
  EXPOSE 3000
  ENV NODE_ENV production

  #Copy all app files from the current directory into the app
  #directory in your container. Set the app directory
  #as the working directory
  ADD . /personality-insights-nodejs-master/
  WORKDIR /personality-insights-nodejs-master/

  #Install any necessary requirements from package.json
  RUN npm install
  
  #Update the curl and openssl packages
  RUN apt-get update && apt-get install -y \
    curl \
    openssl

  #Start the Personal Insight app.
  CMD ["node", "/personality-insights-nodejs-master/app.js"]
  ```
  {: codeblock}

3. Save your changes by pressing `ctrl + o`. Confirm your changes by pressing `ENTER`. Exit the nano editor by pressing `ctrl + x`.

4. Build the Docker image with your app code and push it to your private registry.

  ```
  bx cr build -t registry.<region>.bluemix.net/namespace/cf-pi .
  ```
  {: pre}
  
  <table>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="This icon indicates there is more information to learn about this step of the task."/> Understanding this command's components</th>
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
  <td><code>-t registry.<region>.bluemix.net/namespace/cf-pi</code></td>
  <td>Your private registry path, which includes your unique namespace and the name of the image. For this example, the same name is used for the image as the CF node app cf-pi, but you can choose any name for the image in your private registry. If you are unsure what your namespace is, run the `bx cr namespaces` command to find it.</td>
  </tr>
  <tr>
  <td><code>.</code></td>
  <td>The location of the Dockerfile. If you are running the build command from the directory that includes the Dockerfile, enter a period (.). Otherwise, use a relative path to the Dockerfile.</td>
  </tr>
  </table>

  The image is created in your registry. You can run the `bx cr images` command to verify that the image was created.

  ```
  REPOSITORY                                     NAMESPACE   TAG      DIGEST         CREATED         SIZE     VULNERABILITY STATUS   
  registry.ng.bluemix.net/namespace/cf-pi        namespace   latest   cb03170b2cb2   3 minutes ago   271 MB   Vulnerable 
  ```
  {: pre}


<br />



## Lesson 3: Deploying a container from your image

Deploy your app as a container in the cloud.
{: shortdesc}

1. Create a deployment from the image that you created in the previous lesson.

  ```
  kubectl run cf-pi-deployment --image=registry.<region>.bluemix.net/<namespace>/cf-pi:latest
  ```
  {: pre}

  <table>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="This icon indicates there is more information to learn about this step of the task."/> Understanding this command's components</th>
  </thead>
  <tbody>
  <tr>
  <td>Parameter</td>
  <td>Description</td>
  </tr>
  <tr>
  <td><code>run</code></td>
  <td>The command to create a Kubernetes deployment that includes a container</td>
  </tr>
  <tr>
  <td><code>cf-pi-deployment</code></td>
  <td>The name of the Kubernetes deployment resource</td>
  </tr>
  <tr>
  <td><code>--image=registry.<region>.bluemix.net/<namespace>/cf-pi:latest</code></td>
  <td>The image to deploy the container from</td>
  </tr>
  </table>

2. Make the app accessible to the world by exposing the deployment as a NodePort service. Just as you might expose a port for a Cloud Foundry app, the NodePort you expose is the port on which the worker node listens for traffic.

    ```
    kubectl expose deployment/cf-pi-deployment --type=NodePort --port=8080 --name=cf-pi-service --target-port=8080
    ```
    {: pre}

    <table>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="This icon indicates there is more information to learn about this step of the task."/> Understanding this command's components</th>
    </thead>
    <tbody>
    <tr>
    <td>Parameter</td>
    <td>Description</td>
    </tr>
    <tr>
    <td><code>expose</code></td>
    <td>The command to create a Kubernetes service resource</td>
    </tr>
    <tr>
    <td><code>deployment/cf-pi-deployment</code></td>
    <td>The name of the deployment to expose from the service</td>
    </tr>
    <tr>
    <td><code>--type=NodePort</code></td>
    <td>The type of networking to use</td>
    </tr>
    <tr>
    <td><code>--port=8080</code></td>
    <td>The port on which the service should serve</td>
    </tr>
    <tr>
    <td><code>--name=cf-pi-service</code></td>
    <td>The name of the service</td>
    </tr>
    <tr>
    <td><code>--target-port=8080</code></td>
    <td>The port to which the service directs traffic. In this instance, the target-port is the same as the port, but other apps you create might differ.</td>
    </tr>
    </table>
    
3. Now that all the deployment work is done, you can test your app in a browser. Get the details to form the URL.
    
    a.  Get information about the service to see which NodePort was assigned.

    ```
    kubectl describe service cf-pi-service
    ```
    {: pre}

    Output:

    ```
    Name:                   cf-pi-service
    Namespace:              default
    Labels:                 run=cf-pi-deployment
    Selector:               run=cf-pi-deployment
    Type:                   NodePort
    IP:                     10.10.10.8
    Port:                   <unset> 8080/TCP
    NodePort:               <unset> 30872/TCP
    Endpoints:              172.30.171.87:8080
    Session Affinity:       None
    No events.
    ```
    {: screen}

      The NodePorts are randomly assigned when they are generated with the `expose` command, but within 30000-32767. In this example, the NodePort is 30872.

    b.  Get the public IP address for the worker node in the cluster.

    ```
    bx cs workers <cluster_name>
    ```
    {: pre}

    Output:

    ```
    ID                                                 Public IP        Private IP     Machine Type        State    Status   Zone    Version   
    kube-dal10-cr18e61e63c6e94b658596ca93d087eed9-w1   169.47.227.138   10.176.48.67   u2c.2x4.encrypted   normal   Ready    dal10   1.8.8
    ```
    {: screen}

4. Open a browser and check out the app with the following URL: `http://<IP_address>:<NodePort>`. With the example values, the URL is `http://169.47.227.138:30872`. You can give this URL to a co-worker to try or enter it in your cell phone's browser, so that you can see that the app really is publicly available.

5. [Launch the Kubernetes dashboard](cs_app.html#cli_dashboard). Note that the steps differ depending on your version of Kubernetes.

6. In the **Workloads** tab, you can see the resources that you created. When you are done exploring the Kubernetes dashboard, use CTRL+C to exit the `proxy` command.

Congratulations! Your app is deployed in a container!
<staging>