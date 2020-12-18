---

copyright:
  years: 2014, 2020
lastupdated: "2020-12-18"

keywords: kubernetes, iks, dev, development, odo

subcollection: containers

content-type: tutorial
services: containers, apps
account-plan:
completion-time: 45m

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



# Developing with OpenShift Do on Kubernetes Clusters
{: #tutorial-odo-iks}
{: toc-content-type="tutorial"}
{: toc-services="containers, apps"}
{: toc-completion-time="45m"}

Leverage the `odo` command line tool to accelerate development on any Kubernetes cluster.
{: shortdesc}

OpenShift Do (`odo`) is command line tool designed to make creating applications on the OpenShift Container Platform quick and easy.   It has a simple syntax that makes getting started and pushing to a cluster easy, and even has features designed to aid in interactive development on Kubernetes platforms.  Though odo was originally designed for OpenShift, odo now supports all flavors of Kubernetes as of the 2.0 release.  This means that all of the `odo` productivity features that aid in development on OpenShift can now be used on non-OpenShift clusters too, including the {{site.data.keyword.cloud_notm}} Kubernetes Service.  

## Objectives
{: #objectives-odo-iks}

- Create a NodeJS app and deploy it into a Kubernetes cluster using `odo` command line utility.
- Use `odo` iterative development capabilities to push code changes into your cluster in real time.  
- View deployment information and verify that your app is up and running.

## Audience
{: #audience-odo-iks}

This tutorial is intended for software developers who want to learn how to create and deploy a new application using the `odo` command line utility, or for developers that want to learn about iterative development in Kubernetes clusters using `odo`.

## Prerequisites
{: #prereqs-odo-iks}

* [Install the {{site.data.keyword.cloud_notm}} CLI ](https://cloud.ibm.com/docs/cli?topic=cli-getting-started)
* [Install `odo`](https://docs.openshift.com/container-platform/4.6/cli_reference/developer_cli_odo/installing-odo.html)


## Creating a new microservice with `odo`
{: #new-microservice-odo-iks}
Once you've installed both the prerequisite command line utilities, then you're ready to get started creating a new microservice using `odo`.  This tutorial demonstrates a workflow  this with the {{site.data.keyword.cloud_notm}} Kubernetes Service, but the same approach could work with Minikube or some other Kubernetes environment.

Before diving straight into `odo`, we will first examine details for the cluster where I will deploy a new microservice.  Using the `ibmcloud ks cluster ls` command, you can list all of your clusters on {{site.data.keyword.cloud_notm}}.   Below you can see that I have two clusters.  The dev-cluster instance is an OpenShift 4.4 cluster, and the devex-playground cluster is a standard Kuberntes 1.18 cluster.  

![List of Kubernetes clusters](images/odo/01-clusters.png)

For this tutorial we will be using the `devex-playground` Kubernetes 1.18 cluster.

Next's you need to set your `KUBECONFIG` environment variable so that `odo` and `kubectl` commands are able to communicate with the cluster.   Do this by running the `ibmcloud ks cluster config --cluster <cluster name>` command.

![Set KUBECONFIG environment variable](images/odo/02-cluster-config.png)

Once you've configured your Kubernetes configuration you can test to make sure that the `odo` cli is able to communicate with your cluster by running the `odo project list` command.  This will list all of the "projects" within your cluster.

![List projects using odo](images/odo/03-odo-project-list.png)

If you are familiar with Kubernetes, you might be asking yourself "What is a project?"   In OpenShift, cluster resources are organized into "projects".   In reality, an OpenShift project is the same thing as a Kubernetes namespaces.  Running the `odo project list` command will output the namespaces in your kubernetes cluster.  A project is not a new construct that has been added to your Kubernetes cluster.

There are a few other OpenShift terms that you'll also want to be aware of when working with `odo`.   Whenever you see reference to an "application", it is referring to a program that is designed for end users.  For any given web application, this refers to the overall experience, not an individual microservice.  An application could be made up of 1, 2, 5, or 100 microservices.  Each one of those microservices is generally referred to as a "component".   So, a project is an organization mechanism which may contain any number of components that, together, create an application.

Each component could be a microservice (for example a NodeJS or Java microservice), and is a deployment within the Kubernetes cluster.  The `odo` CLI makes it easy to setup and configure new components (microservices) in your clusters.

Next we will go ahead and create a new component.  The first thing to do is setup a local directory and create a Kubernetes namespace/project to deploy the microservice into.   You can create a new namespace by running the `kubectl create namespace <namespace>` command. 

![Create a new Kubernetes namespace](images/odo/04-create-project.png)

> Note: You could also have created a namespace/project using the "odo project create" command.

To create a new microservice, `cd` into your working directory, and run the `odo create` command.  The `odo` command line utility will guide you through a prompted flow to select the language for the new microservice/component, a component name, and select the project/namespace that you just created in the previous step.

![Create a new microservice using the "odo create" command](images/odo/05-odo-create.png)

`odo` will create a devfile (workspace configuration file), and ask if you'd like to download a starter project.  If you select "yes", then it will clone a starter microservice codebase to your local directory.  


At this point, you could immediately run the code locally using node/npm, or start editing code right away.   In the screenshot below, you can see the new starter Node.js codebase in a local editing environment.

![Viewing source code in an editor](images/odo/06-local-editor.png]


## Pushing a microservice into the cluster with `odo`
{: #push-microservice-odo-iks}

Once you have the code running locally, the next step is to push this Node.js microservice into a Kubernetes cluster.   You may notice that there is no dockerfile, no Kubernetes yaml, and no helm chart... these files are not necessary when using the `odo` cli.  The `odo` command line utility will use OpenShift's `source-to-image` capabilty to generate all files needed to deploy the microservice into the Kubernetes cluster.   

Use the `odo push` command to push the application into the cluster. 

![Using `odo push` to deploy a microservice](images/odo/07-odo-push.png)


`odo push` will validate the Node.js component and will take care of containerization/packaging and deployment of the microservice's source code.  There is no need for a specific Kubernetes yaml file to push your component/deployment into the cluster.  

Once the Node.js microservice has been pushed, you'll be able to see the deployed resources in your Kubernetes cluster dashboard. 

![Viewing the Kubernetes dashboard](images/odo/08-kube-dashboard.png]

At this point the deployed component has not been exposed externally from the cluster, so you can't hit it from a web browser.  To expose the microservice outside of hte cluster, use the `odo url create` command.  This will create an ingress rule to expose the Node.js microservice's pod using the cluster's ingress subdomain.  

Use the `ibmcloud ks cluster get` command to retrieve the cluster's ingress subdomain.  In the screenshot below you can this is used with the "grep" command to fetch the ingress subdomain, and use that as a parameter to the `odo url create` command.  

```
odo url create --port 3000 --host  devex-playground-02c37e76d52f84d326f2c39229cba3e1-0000.us-south.containers.appdomain.cloud
```

![Using the `odo url create` command](images/odo/09-url-create.png)

After creating the url, run the `odo push` command again to push your local changes up into the Kubernetes cluster.   Once the changes have been applied, you'll be able to access your application on the URL that was created.

![Viewing the microservice in the web browser](images/odo/10-app-in-web-browser.png)

You have now created a new Node.js microservice and pushed it into a cluster in a matter of minutes.  You can now make any changes you want and then push them to the cluster as needed.  


## Iterative development with `odo`
{: #iterative-dev-odo-iks}

Now that you have create a new microeservice and deployed it using `odo`, think about how developers write their applications.  Oftern it is an iterative process; changing code, pushing changes, testing, making more changes etc...  


If you're running everything locally, you can use `nodemon` to automatically hot-reload changes on the fly every time a file is saved.  This kind of iterative development process is also possible for microservices running inside of a cluster by using the `odo watch` command.  

The `odo watch` command will monitor for local file changes, and automatically push them into the cluster.  So, as a developer, all that you need to do is write code and save your files.  They will automatically be pushed into the cluster, and the changes will automatically be available in the browser when you go to test them!

![Viewing the microservice in the web browser](images/odo/11-iterative-development.png)



## What's next?
{: #next-steps-odo-iks notoc}

* **Learn more about OpenShift Do**: Learn more about the features of `odo` by visiting the [RedHat OpenShift Do Documentation]
(https://docs.openshift.com/container-platform/4.2/cli_reference/openshift_developer_cli/understanding-odo.html)

