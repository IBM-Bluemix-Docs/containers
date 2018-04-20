---

copyright:
  years: 2014, 2018
lastupdated: "2018-4-20"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}

# {{site.data.keyword.containerlong_notm}} technology

Learn more about the technology behind {{site.data.keyword.containerlong}}.
{:shortdesc}

## Docker containers
{: #docker_containers}

Built on existing Linux container technology (LXC), the open source project named Docker became a software platform for building, testing, deploying, and scaling apps quickly. Docker packages software into standardized units, called containers, that include all of the elements that an app needs to run.
{:shortdesc}

Learn about some basic Docker concepts:

<dl>
<dt>Image</dt>
<dd>A Docker image is built from a Dockerfile, a text file that defines how to build the image and which build artifacts to include in it, such as the app, the app's configuration, and its dependencies. Images are always built from other images, making them quick to configure. Let someone else do the bulk of the work on an image and then tweak it for your use.</dd>
<dt>Registry</dt>
<dd>An image registry is a place to store, retrieve, and share Docker images. Images that are stored in a registry can either be publicly available (public registry) or accessible by a small group of users (private registry). {{site.data.keyword.containershort_notm}} offers public images, such as ibmliberty, that you can use to create your first containerized app. When it comes to enterprise applications, use a private registry like the one that is provided in {{site.data.keyword.Bluemix_notm}} to protect your images from being used by unauthorized users.
</dd>
<dt>Container</dt>
<dd>Every container is created from an image. A container is a packaged app with all of its dependencies so that the app can be moved between environments and run without changes. Unlike virtual machines, containers do not virtualize a device, its operating system, and the underlying hardware. Only the app code, run time, system tools, libraries, and settings are packaged inside the container. Containers run as isolated processes on Ubuntu compute hosts and share the host operating system and its hardware resources. This approach makes a container more lightweight, portable, and efficient than a virtual machine.</dd>
</dl>



### Key benefits of using containers
{: #container_benefits}

<dl>
<dt>Containers are agile</dt>
<dd>Containers simplify system administration by providing standardized environments for development and production deployments. The lightweight run time enables rapid scale-up and scale-down of deployments. Remove the complexity of managing different operating system platforms and their underlying infrastructures by using containers to help you deploy and run any app on any infrastructure quickly and reliably.</dd>
<dt>Containers are small</dt>
<dd>You can fit many containers in the amount of space that a single virtual machine requires.</dd>
<dt>Containers are portable</dt>
<dd>
<ul>
  <li>Reuse pieces of images to build containers. </li>
  <li>Move app code quickly from staging to production environments.</li>
  <li>Automate your processes with continuous delivery tools.</li>
  </ul>
  </dd>


  
<p>Ready to gain deeper knowledge of Docker? <a href="https://developer.ibm.com/courses/all/docker-essentials-extend-your-apps-with-containers/" target="_blank">Learn how Docker and {{site.data.keyword.containershort_notm}} work together by completing this course.</a></p>

</dl>

{[white-space.md]}

## Kubernetes clusters
{: #kubernetes_basics}

<img src="images/certified-kubernetes-resized.png" style="padding-right: 10px;" align="left" alt="This badge indicates Kubernetes certification for IBM Cloud Container Service."/>The open source project named Kubernetes combines running a containerized infrastructure with production work loads, open source contributions, and Docker container management tools. The Kubernetes infrastructure provides an isolated and secure app platform for managing containers that is portable, extensible, and self-healing in case of failovers.
{:shortdesc}

Learn about some basic Kubernetes concepts as shown in the following diagram.

![Deployment setup](images/cs_app_tutorial_components1.png)

<dl>
<dt>Account</dt>
<dd>Your account refers to your {{site.data.keyword.Bluemix_notm}} account.</dd>

<dt>Cluster</dt>
<dd>A Kubernetes cluster consists of one or more compute hosts that are called worker nodes. Worker nodes are managed by a Kubernetes master that centrally controls and monitors all Kubernetes resources in the cluster. So when you deploy the resources for a containerized app, the Kubernetes master decides which worker node to deploy those resources on, taking into account the deployment requirements and available capacity in the cluster. Kubernetes resources include services, deployments, and pods.</dd>

<dt>Service</dt>
<dd>A service is a Kubernetes resource that groups a set of pods and provides network connectivity to these pods without exposing the actual private IP address of each pod. You can use a service to make your app available within your cluster or to the public internet.
</dd>

<dt>Deployment</dt>
<dd>A deployment is a Kubernetes resource where you might specify information about other resources or capabilities that are required to run your app, such as services, persistent storage, or annotations. You document a deployment in a configuration YAML file, and then apply it to the cluster. The Kubernetes master configures the resources and deploys containers into pods on the worker nodes with available capacity.
</br></br>
Define update strategies for your app, including the number of pods that you want to add during a rolling update and the number of pods that can be unavailable at a time. When you perform a rolling update, the deployment checks whether the update is working and stops the rollout when failures are detected.</dd>

<dt>Pod</dt>
<dd>Every containerized app that is deployed into a cluster is deployed, run, and managed by a Kubernetes resource that is called a pod. Pods represent small deployable units in a Kubernetes cluster and are used to group the containers that must be treated as a single unit. In most cases, each container is deployed in its own pod. However, an app might require a container and other helper containers to be deployed into one pod so that those containers can be addressed by using the same private IP address.</dd>

<dt>App</dt>
<dd>An app might refer to a complete app or a component of an app. You might deploy components of an app in separate pods or separate worker nodes.</dd>


  
<p>Ready to gain deeper knowledge of Kubernetes?</p>
<ul><li><a href="cs_tutorials.html#cs_cluster_tutorial" target="_blank">Expand your terminology knowledge with the Creating clusters tutorial</a>.</li>
<li><a href="https://developer.ibm.com/courses/all/get-started-kubernetes-ibm-cloud-container-service/" target="_blank">Learn how Kubernetes and {{site.data.keyword.containershort_notm}} work together by completing this course.</a></li></ul>


</dl>

{[white-space.md]}

## Service architecture
{: #architecture}

In a Kubernetes cluster that runs on {{site.data.keyword.containershort_notm}}, your containerized apps are hosted on compute hosts that are called worker nodes. Well, to be more specific, the apps run in pods and the pods are hosted on worker nodes. Worker nodes are managed by the Kubernetes master. The Kubernetes master and the worker nodes communicate with each other through secure TLS certificates and an openVPN connection to orchestrate your cluster configurations.
{: shortdesc}

What's the difference between the Kubernetes master and a worker node? Glad you asked.

<dl>
  <dt>Kubernetes master</dt>
    <dd>The Kubernetes master is tasked with managing all compute, network, and storage resources in the cluster. The Kubernetes master ensures that your containerized apps and services are equally deployed to the worker nodes in the cluster. Depending on how you configure your app and services the master determines the worker node that has sufficient resources to fulfill the app's requirements.</dd>
  <dt>Worker node</dt>
    <dd>Each worker node is a physical machine (bare metal) or a virtual machine that runs on physical hardware in the cloud environment. When you provision a worker node, you determine the resources that are available to the containers hosted on that worker node. Out of the box, your worker nodes are set up with an {{site.data.keyword.IBM_notm}} managed Docker Engine, separate compute resources, networking, and a volume service. The built-in security features provide isolation, resource management capabilities, and worker node security compliance.</dd>
</dl>

![{{site.data.keyword.containerlong_notm}} Kubernetes architecture](images/cs_org_ov.png)
Figure. {{site.data.keyword.containershort_notm}} architecture

Want to see how {{site.data.keyword.containerlong_notm}} can be used with other products and services? Check out some of the [integrations](cs_integrations.html#integrations).


{[white-space.md]}
