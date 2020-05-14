# Ingress management with Ambassador Edge Stack

This tutorial covers the use of Ambassador Edge Stack (AES) to manage {{site.data.keyword.cloud_notm}} traffic to your services.

[Ambassador Edge Stack](https://getambassador.io/) is a popular, high-performance Ingress Controller and API Gateway built on [Envoy Proxy](https://www.envoyproxy.io/). In {{site.data.keyword.containerlong_notm}}, IBM-provided application load balancers (ALBs) are based on a custom NGINX ingress controller. Envoy Proxy was designed from the ground up for cloud-native applications. Ambassador exposes Envoy's functionality as Custom Resource Definitions, with integrated support for rate limiting, authentication, load balancing, and observability.

IBM does not provide support for AES deployment. You are responsible for configuring, updating, and managing your system. The following sections help you get started, but you are responsible for any changes that you make to the steps.  For additional help and support, or to contribute to the Ambassador project itself, you can explore [the Ambassador documentation and community](https://www.getambassador.io/docs/latest/)

# Requirements

1. Classic cluster \[[get started here](/docs/containers?topic=containers-getting-started#clusters_gs)\]
2. ibmcloud CLI \[[get it here](/docs/containers?topic=containers-cs_cli_install)\]
3. kubectl CLI \[[get it here](https://kubernetes.io/docs/tasks/tools/install-kubectl/)\]

Before starting, make sure you have [Administrator and Manager](/docs/containers?topic=containers-users#platform) access configured for your account.

# Installation and Deployment

First we are going to deploy Ambassador Edge Stack to {{site.data.keyword.containerlong_notm}}, if you haven't created a cluster, visit the [getting started page](/docs/containers?topic=containers-getting-started#clusters_gs)to learn how.  Ensure that you are working on the right cluster by running `kubectl config current-context`.  There are multiple ways of deploying AES, this tutorial covers installing the [Ambassador Operator](https://www.getambassador.io/docs/latest/topics/install/aes-operator/) via a manual `yaml` deployment.  We will then use the Operator to install Ambassador Edge Stack to the cluster.  You can explore other installation methods at Ambassador's [installation page](https://www.getambassador.io/docs/latest/topics/install/).

Installing Ambassador via the Operator provides a more robust workflow by automating and integrating the Ambassador installation and upgrade process.  The Operator also allows for additional configuration options by applying Helm chart values.

### Installation

1. Apply the Operator crds.

```bash
kubectl apply -f https://github.com/datawire/ambassador-operator/releases/latest/download/ambassador-operator-crds.yaml
```

2. Apply the Operator deployment manifest.

```bash
kubectl apply -n ambassador -f https://github.com/datawire/ambassador-operator/releases/latest/download/ambassador-operator.yaml
```

3. Create an "AmbassadorInstallation" deployment manifest.

```yaml
---
apiVersion: getambassador.io/v2
kind: AmbassadorInstallation
metadata:
  name: ambassador
spec:
  version: *
```

4. Apply the manifest.

```bash
kubectl apply -n ambassador -f installation.yaml
```

This tutorial only covers a basic Ambassador installation via Operator.  To learn more about the Ambassador Operator and its additional configurations, you can explore the [installation page](https://www.getambassador.io/docs/latest/topics/install/aes-operator/).

### Exposing a Service

To test our deployment, we are going to deploy a sample application to deliver random quotes to users.  The manifest below describes a Quote deployment exposed via a Quote service.

```yaml
---
apiVersion: v1
kind: Service
metadata:
  name: quote
spec:
  ports:
  - name: http
    port: 80
    targetPort: 8080
  selector:
    app: quote
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: quote
spec:
  replicas: 1
  selector:
    matchLabels:
      app: quote
  strategy:
    type: RollingUpdate
  template:
    metadata:
      labels:
        app: quote
    spec:
      containers:
      - name: backend
        image: quay.io/datawire/quote:0.3.0
        ports:
        - name: http
          containerPort: 8080
```

1. Apply with `kubectl apply -f quote.yaml`.

2. We have our deployment and it's exposed with a service, but we still cannot connect to it without telling Ambassador where it is.  This is where the [Mapping](https://getambassador.io/docs/latest/topics/using/intro-mappings/) comes into play.  With a mapping, we can use kube-dns to let Ambassador know exactly where to look for the Quote service when the right request comes in.

```yaml
---
apiVersion: getambassador.io/v2
kind: Mapping
metadata:
  name: backend
spec:
  prefix: /backend/
  service: quote.<namespace>
```

3. Test that it works by using `curl -k https://{{AMBASSADOR_IP}}/backend/` in your terminal.  You should see something similar to the following:

```
{
    "server": "bleak-kumquat-n9qg6ra1",
    "quote": "Non-locality is the driver of truth. By summoning, we vibrate.",
    "time": "2020-05-08T18:33:54.578661743Z"
}% 
```

# Configure Ambassador for TLS termination

1. We can use the NLB's DNS provisioner to both give us a FQDN and give us a valid SSL certificate to serve client requests.  Start by getting Ambassador's external IP.

```bash
kubectl get svc ambassador -n ambassador
```

2. Using this IP address, create a new DNS hostname.

```bash
ibmcloud ks nlb-dns create classic --cluster <cluster name> --ip <ambassador ip>
```

3. Verify it was created.

```bash
ibmcloud ks nlb-dns ls --cluster <cluster name>
```

4. Now we need to create a [Host](https://www.getambassador.io/docs/latest/topics/running/host-crd/) and configure it.  We can use the Hostname and the SSL Cert Secret Name fields from the previous to populate our Host CRD.

```yaml
---
apiVersion: getambassador.io/v2
kind: Host
metadata:
  name: ibm-public
spec:
    acmeProvider:
      authority: none
    hostname: <hostname>
    tlsSecret:
      name: <ssl cert secret name>
```

5. Navigate to your new, secure service on the browser at `https://<hostname>/backend/`.  Not only should you not get any SSL warnings, you should see the lock icon next to the URL.

# Additional questions/FAQ

If you have more questions or need more help with respect to Ambassador implementation or functionality, you can [check the FAQ](https://www.getambassador.io/docs/latest/about/faq/), [read the docs](https://www.getambassador.io/docs/latest/), or [join the Datawire slack](http://d6e.co/slack).