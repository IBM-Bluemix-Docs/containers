---

copyright:
  years: 2014, 2019
lastupdated: "2019-07-19"

keywords: kubernetes, iks

subcollection: containers

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:preview: .preview}

# Tutorial overview
{: #tutorials-ov}

<style>
<!--
    #tutorials { /* hide the page header */
        display: none !important
    }
    .allCategories {
        display: flex !important;
        flex-direction: row !important;
        flex-wrap: wrap !important;
    }
    .solutionBoxContainer {}
    .solutionBoxContainer a {
        text-decoration: none !important;
        border: none !important;
    }
    .solutionBox {
        display: inline-block !important;
        width: 600px !important;
        margin: 0 10px 20px 0 !important;
        padding: 10px !important;
        border: 1px #dfe6eb solid !important;
        box-shadow: 0 1px 2px 0 rgba(0, 0, 0, 0.2) !important;
    }
    @media screen and (min-width: 960px) {
        .solutionBox {
        width: 27% !important;
        }
        .solutionBoxContent {
        height: 320px !important;
        }
    }
    @media screen and (min-width: 1298px) {
        .solutionBox {
        width: calc(33% - 2%) !important;
        }
        .solutionBoxContent {
        min-height: 320px !important;
        }
    }
    .solutionBox:hover {
        border-color: rgb(136, 151, 162) !important;
    }
    .solutionBoxContent {
        display: flex !important;
        flex-direction: column !important;
    }
    .solutionBoxDescription {
        flex-grow: 1 !important;
        display: flex !important;
        flex-direction: column !important;
    }
    .descriptionContainer {
    }
    .descriptionContainer p {
        margin: 2px !important;
        overflow: hidden !important;
        display: -webkit-box !important;
        -webkit-line-clamp: 4 !important;
        -webkit-box-orient: vertical !important;
        font-size: 12px !important;
        font-weight: 400 !important;
        line-height: 1.5 !important;
        letter-spacing: 0 !important;
        max-height: 70px !important;
    }
    .architectureDiagramContainer {
        flex-grow: 1 !important;
        min-width: 200px !important;
        padding: 0 10px !important;
        text-align: center !important;
        display: flex !important;
        flex-direction: column !important;
        justify-content: center !important;
    }
    .architectureDiagram {
        max-height: 170px !important;
        padding: 5px !important;
        margin: 0 auto !important;
    }
-->
</style>


## Create a cluster and deploy your first app
{: #tutorials-create-cluster-deploy-app}

<div class = "solutionBoxContainer">
    <a href = "/docs/containers?topic=containers-cs_cluster_tutorial">
    <div class = "solutionBox">
        <div class = "solutionBoxContent">
          Kubernetes-native cluster on Ubuntu
          <div class="solutionBoxDescription">
                <div class="descriptionContainer">
                  </br><p>Create a Kubernetes cluster on managed {{site.data.keyword.containerlong_notm}} with Classic infrastructure worker nodes that run an Ubuntu operating system. </p></br>
                </div>
                <div class="architectureDiagramContainer">
                    <img class="architectureDiagram" src = "images/tutorial_ov.png" alt="Architecture diagram for the solution Create a Kubernetes cluster" />
                </div>
            </div>
        </div>
    </div>
    </a>
  <a href = "/docs/containers?topic=containers-openshift_tutorial">
    <div class = "solutionBox">
        <div class = "solutionBoxContent">
          Red Hat OpenShift cluster
            <div class="solutionBoxDescription">
                <div class="descriptionContainer">
                  </br><p>Create an {{site.data.keyword.containerlong_notm}} cluster with worker nodes that come installed with the OpenShift container orchestration platform software. </p></br>
                </div>
                <div class="architectureDiagramContainer">
                    <img class="architectureDiagram" src = "images/cs_org_ov_both_ses_rhos.png" alt="Architecture RHOS" /></br>
                </div>
            </div>
        </div>
    </div>
    </a>
</div>


## Deploy apps to a cluster 
{: #tutorials-deploy-app}

<div class = "solutionBoxContainer">
    <a href = "/docs/tutorials?topic=solution-tutorials-scalable-webapp-kubernetes">
    <div class = "solutionBox">
        <div class = "solutionBoxContent">
                Scalable web application on Kubernetes
          <div class="solutionBoxDescription">
                <div class="descriptionContainer">
                  </br> <p>Scaffold a web app, deploy it to a cluster, and learn how to scale your app and monitor its health. </p></br>
                </div>
                <div class="architectureDiagramContainer">
                    <img class="architectureDiagram" src = "https://raw.githubusercontent.com/IBM-Bluemix-Docs/tutorials/master/images/solution2/Architecture.png" alt="Architecture diagram for deploying web apps with {{site.data.keyword.containerlong_notm}}" />
                </div>
            </div>
        </div>
    </div>
    </a>
    <a href = "https://github.com/IBM-Cloud/logistics-wizard-kubernetes">
    <div class = "solutionBox">
        <div class = "solutionBoxContent">
                Run Kubernetes and Cloud Foundry apps
          <div class="solutionBoxDescription">
                <div class="descriptionContainer">
                  </br> <p>Create a Logistics Wizard deployment where the ERP and Controller services are deployed in Kubernetes, and the web user interface stays deployed as a Cloud Foundry app. </p></br>
                </div>
                <div class="architectureDiagramContainer">
                    <img class="architectureDiagram" src = "https://raw.githubusercontent.com/IBM-Cloud/logistics-wizard-kubernetes/master/lw_kube_architecture.png" alt="Logistics Wizard architecture overview" />
                </div>
            </div>
        </div>
    </div>
    </a>
    <a href = "/docs/tutorials?topic=solution-tutorials-pub-sub-object-storage#pub-sub-object-storage">
    <div class = "solutionBox">
        <div class = "solutionBoxContent">
                Asynchronous data processing for apps
          <div class="solutionBoxDescription">
                <div class="descriptionContainer">
                  </br> <p>Use an Apache Kafka-based messaging service to orchestrate long running workloads to apps that run in a Kubernetes cluster. </p></br>
                </div>
                <div class="architectureDiagramContainer">
                    <img class="architectureDiagram" src = "https://raw.githubusercontent.com/IBM-Bluemix-Docs/tutorials/master/images/solution25/Architecture.png" alt="Asynchronous data processing architecture" />
                </div>
            </div>
        </div>
    </div>
    </a>
</div>


## Set up high availability and security
{: #tutorials-ov-ha-network-security}

<div class = "solutionBoxContainer">
    <a href = "/docs/tutorials?topic=solution-tutorials-multi-region-k8s-cis#multi-region-k8s-cis">
    <div class = "solutionBox">
        <div class = "solutionBoxContent">
                Resilient and secure multi-region clusters with Cloud Internet Services
          <div class="solutionBoxDescription">
                <div class="descriptionContainer">
                  </br> <p>Integrate Cloud Internet Services with Kubernetes clusters to deliver a resilient and secure solution across multiple {{site.data.keyword.cloud_notm}} regions. </p></br>
                </div>
                <div class="architectureDiagramContainer">
                    <img class="architectureDiagram" src = "https://raw.githubusercontent.com/IBM-Bluemix-Docs/tutorials/master/images/solution32-multi-region-k8s-cis/Architecture.png" alt="Architecture diagram for using Cloud Internet Service with {{site.data.keyword.containerlong_notm}}" />
                </div>
            </div>
        </div>
    </div>
    </a>
    <a href = "/docs/tutorials?topic=solution-tutorials-strategies-for-resilient-applications">
    <div class = "solutionBox">
        <div class = "solutionBoxContent">
                Strategies for resilient applications in the cloud
          <div class="solutionBoxDescription">
                <div class="descriptionContainer">
                  </br> <p>Learn what to consider when creating resilient apps in the cloud and what {{site.data.keyword.cloud_notm}} services you can use. </p></br>
                </div>
                <div class="architectureDiagramContainer">
                    <img class="architectureDiagram" src = "https://raw.githubusercontent.com/IBM-Bluemix-Docs/tutorials/master/images/solution39/Architecture.png" alt="Architecture diagram for creating resilient applications" />
                </div>
            </div>
        </div>
    </div>
    </a>
    <a href = "/docs/containers?topic=containers-policy_tutorial">
    <div class = "solutionBox">
        <div class = "solutionBoxContent">
                Block unwanted network traffic with Calico policies
          <div class="solutionBoxDescription">
                <div class="descriptionContainer">
                  </br> <p>Learn how to use Calico policies to whitelist or blacklist network traffic from and to certain IP addresses. </p></br>
                </div>
                <div class="architectureDiagramContainer">
                    <img class="architectureDiagram" src = "images/cs_tutorial_policies_L4.png" alt="Block incoming network traffic with Calico network policies" />
                </div>
            </div>
        </div>
    </div>
    </a>
    <a href = "/docs/containers?topic=containers-istio">
    <div class = "solutionBox">
        <div class = "solutionBoxContent">
                Secure, manage, and monitor a network of microservices with Istio
          <div class="solutionBoxDescription">
                <div class="descriptionContainer">
                  </br> <p>Explore the intelligent routing and monitoring capability of Istio to control and secure your microservices in the cloud. </p></br>
                </div>
                <div class="architectureDiagramContainer">
                    <img class="architectureDiagram" src = "images/istio_ov.png" alt="Overview of Istio components and dependencies" /></br>
                </div>
            </div>
        </div>
    </div>
    </a>
    <a href = "/docs/tutorials?topic=solution-tutorials-cloud-e2e-security#cloud-e2e-security">
    <div class = "solutionBox">
        <div class = "solutionBoxContent">
                Apply end to end security to an {{site.data.keyword.cloud_notm}} app
          <div class="solutionBoxDescription">
                <div class="descriptionContainer">
                  </br> <p>Learn how to use authentication and encryption to protect your app, and how to monitor and audit cluster activities. </p></br>
                </div>
                <div class="architectureDiagramContainer">
                    <img class="architectureDiagram" src = "https://github.com/IBM-Bluemix-Docs/tutorials/blob/master/images/solution34-cloud-e2e-security/Architecture.png?raw=true" alt="Architecture diagram for applying end-to-end security to a cloud app" /></br>
                </div>
            </div>
        </div>
    </div>
    </a>
    <a href = "/docs/tutorials?topic=solution-tutorials-users-teams-applications#users-teams-applications">
    <div class = "solutionBox">
        <div class = "solutionBoxContent">
                Organizing users and teams with {{site.data.keyword.cloud_notm}} Identity and Access Management
          <div class="solutionBoxDescription">
                <div class="descriptionContainer">
                  </br> <p>Set up cluster access for users and teams and learn how to replicate this setup across environments. </p></br>
                </div>
                <div class="architectureDiagramContainer">
                    <img class="architectureDiagram" src = "https://raw.githubusercontent.com/IBM-Bluemix-Docs/tutorials/master/images/solution20-users-teams-applications/architecture.png" alt="Architecture diagram for organizing users and teams" /></br>
                </div>
            </div>
        </div>
    </div>
    </a>
</div>



## Automate app and cluster deployments
{: #tutorials-ov-app-cluster-deployments}

<div class = "solutionBoxContainer">
    <a href = "/docs/containers?topic=containers-serverless-apps-knative">
    <div class = "solutionBox">
        <div class = "solutionBoxContent">
                Deploy serverless apps with Knative services
          <div class="solutionBoxDescription">
                <div class="descriptionContainer">
                  </br><p>Create modern, source-centric, containerized, and serverless apps on top of your Kubernetes cluster. </p></br>
                </div>
                <div class="architectureDiagramContainer">
                    <img class="architectureDiagram" src = "images/knative_ov.png" alt="Overview of Knative components and dependencies" />
                </div>
            </div>
        </div>
    </div>
    </a>
    <a href = "/docs/tutorials?topic=solution-tutorials-continuous-deployment-to-kubernetes#continuous-deployment-to-kubernetes">
    <div class = "solutionBox">
        <div class = "solutionBoxContent">
                Continuous deployment to Kubernetes clusters
          <div class="solutionBoxDescription">
                <div class="descriptionContainer">
                  </br>  <p>Set up a DevOps pipeline for containerized apps that run in Kubernetes and add integrations such as security scanner, Slack notifications, and analytics.  </p></br>
                </div>
                <div class="architectureDiagramContainer">
                    <img class="architectureDiagram" src = "https://raw.githubusercontent.com/IBM-Bluemix-Docs/tutorials/master/images/solution21/Architecture.png" alt="Architecture diagram for setting up a continuous delivery and continuous integration pipeline" />
                </div>
            </div>
        </div>
    </div>
    </a>
    <a href = "https://github.com/razee-io/Razee">
    <div class = "solutionBox">
        <div class = "solutionBoxContent">
                Automate multi-cluster deployments with Razee 
            <div class="solutionBoxDescription">
                <div class="descriptionContainer">
                  </br>  <p>Automate, manage, and visualize the deployment of Kubernetes resources across clusters, environments, and cloud providers with Razee.</p></br>
                </div>
                <div class="architectureDiagramContainer">
                    <img class="architectureDiagram" src = "images/razee_ov_no_txt.png" alt="Razee deployment automation architecture" />
                </div>
            </div>
        </div>
    </div>
    </a>
</div>


## Monitor and log cluster activity
{: #tutorials-ov-monitor-log}

<div class = "solutionBoxContainer">
    <a href = "/docs/services/Log-Analysis-with-LogDNA?topic=LogDNA-kube#kube">
    <div class = "solutionBox">
        <div class = "solutionBoxContent">
                Managing Kubernetes cluster logs with {{site.data.keyword.la_full_notm}}
          <div class="solutionBoxDescription">
                <div class="descriptionContainer">
                  </br> <p>Configure a logging agent in your cluster and monitor different log sources with  {{site.data.keyword.la_full_notm}}. </p></br>
                </div>
                <div class="architectureDiagramContainer">
                    <img class="architectureDiagram" src = "https://raw.githubusercontent.com/IBM-Bluemix-Docs/Log-Analysis-with-LogDNA/master/images/kube.png" alt="LogDNA architecture overview" />
                </div>
            </div>
        </div>
    </div>
    </a>
  <a href = "/docs/services/Monitoring-with-Sysdig?topic=Sysdig-kubernetes_cluster#kubernetes_cluster">
    <div class = "solutionBox">
        <div class = "solutionBoxContent">
                Analyze cluster metrics with {{site.data.keyword.mon_full_notm}} 
            <div class="solutionBoxDescription">
                <div class="descriptionContainer">
                  </br><p>Set up a Sysdig metrics agent in your cluster and explore how you can monitor your cluster's health.  </p></br>
                </div>
                <div class="architectureDiagramContainer">
                    <img class="architectureDiagram" src = "https://raw.githubusercontent.com/IBM-Bluemix-Docs/Monitoring-with-Sysdig/master/images/kube.png" alt="Sysdig architecture overview" />
                </div>
            </div>
        </div>
    </div>
    </a>
</div>


## Migrate apps to the cloud 
{: #tutorials-ov-migrate-apps}

<div class = "solutionBoxContainer">
    <a href = "/docs/containers?topic=containers-cf_tutorial">
    <div class = "solutionBox">
        <div class = "solutionBoxContent">
                Containerize a Cloud Foundry app
          <div class="solutionBoxDescription">
                <div class="descriptionContainer">
                  </br> <p>Learn how to create a Docker image from a Python Cloud Foundry app, push this image to {{site.data.keyword.registryshort_notm}}, and deploy your app to a Kubernetes cluster.  </p></br>
                </div>
                <div class="architectureDiagramContainer">
                    <img class="architectureDiagram" src = "images/python_flask.png" alt="Python hello world welcome screen" />
                </div>
            </div>
        </div>
    </div>
    </a>
  <a href = "/docs/tutorials?topic=solution-tutorials-vm-to-containers-and-kubernetes#move-a-vm-based-application-to-kubernetes">
    <div class = "solutionBox">
        <div class = "solutionBoxContent">
                Migrate a VM-based app
            <div class="solutionBoxDescription">
                <div class="descriptionContainer">
                  </br><p>Prepare your app code, containerize your VM-based app, and deploy this app to a Kubernetes cluster.  </p></br>
                </div>
                <div class="architectureDiagramContainer">
                    <img class="architectureDiagram" src = "https://raw.githubusercontent.com/IBM-Bluemix-Docs/tutorials/master/images/solution30/modern_architecture.png" alt="VM-based app migration to Kubernetes" />
                </div>
            </div>
        </div>
    </div>
    </a>
    <a href = "https://github.com/IBM-Cloud/jpetstore-kubernetes">
    <div class = "solutionBox">
        <div class = "solutionBoxContent">
                Modernize a Java web app
            <div class="solutionBoxDescription">
                <div class="descriptionContainer">
                  </br><p>Containerize the JPetStore app and extend it with Watson Visual Recognition and Twilio text messaging.</p></br>
                </div>
                <div class="architectureDiagramContainer">
                    <img class="architectureDiagram" src = "https://raw.githubusercontent.com/IBM-Cloud/jpetstore-kubernetes/master/readme_images/architecture.png" alt="Modernize and extend a Java web app" />
                </div>
            </div>
        </div>
    </div>
    </a>
</div>
