---

copyright:
  years: 2014, 2018
lastupdated: "2018-04-18"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}

# Restricting network traffic to edge worker nodes
{: #edge}

Edge worker nodes can improve the security of your Kubernetes cluster by allowing fewer worker nodes to be accessed externally and by isolating the networking workload in {{site.data.keyword.containerlong}}.
{:shortdesc}

When these worker nodes are marked for networking only, other workloads cannot consume the CPU or memory of the worker node and interfere with networking.




## Label worker nodes as edge nodes
{: #edge_nodes}

Add the `dedicated=edge` label to two or more worker nodes on each public VLAN in your cluster to ensure that Ingress and load balancers are deployed to those worker nodes only.
{:shortdesc}

Before you begin:

- [Create a standard cluster.](cs_clusters.html#clusters_cli)
- Ensure that your cluster has a least one public VLAN. Edge worker nodes are not available for clusters with private VLANs only.
- [Target the Kubernetes CLI to the cluster](cs_cli_install.html#cs_cli_configure).

Steps:

1. List all of the worker nodes in the cluster. Use the private IP address from the **NAME** column to identify the nodes. Select at least two worker nodes on each public VLAN to be edge worker nodes. Using two or more worker nodes improves availability of the networking resources.

  ```
  kubectl get nodes -L publicVLAN,privateVLAN,dedicated
  ```
  {: pre}

2. Label the worker nodes with `dedicated=edge`. After a worker node is marked with `dedicated=edge`, all subsequent Ingress and load balancers are deployed to an edge worker node.

  ```
  kubectl label nodes <node1_name> <node2_name> dedicated=edge
  ```
  {: pre}

3. Retrieve all existing load balancer services in your cluster.

  ```
  kubectl get services --all-namespaces -o jsonpath='{range .items[*]}kubectl get service -n {.metadata.namespace} {.metadata.name} -o yaml | kubectl apply -f - :{.spec.type},{end}' | tr "," "\n" | grep "LoadBalancer" | cut -d':' -f1
  ```
  {: pre}

  Output:

  ```
  kubectl get service -n <namespace> <service_name> -o yaml | kubectl apply -f
  ```
  {: screen}

4. Using the output from the previous step, copy and paste each `kubectl get service` line. This command redeploys the load balancer to an edge worker node. Only public load balancers need to be redeployed.

  Output:

  ```
  service "my_loadbalancer" configured
  ```
  {: screen}

You labeled worker nodes with `dedicated=edge` and redeployed all existing load balancers and Ingress to the edge worker nodes. Next, prevent other [workloads from running on edge worker nodes](#edge_workloads) and [block inbound traffic to node ports on worker nodes](cs_network_policy.html#block_ingress).

<br />


## Prevent workloads from running on edge worker nodes

{: #edge_workloads}

One benefit of edge worker nodes is that they can be specified to run networking services only.
{:shortdesc}

Using the `dedicated=edge` toleration means that all load balancer and Ingress services are deployed to the labeled worker nodes only. However, to prevent other workloads from running on edge worker nodes and consuming worker node resources, you must use [Kubernetes taints ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/).


1. List all worker nodes with the `edge` label.

  ```
  kubectl get nodes -L publicVLAN,privateVLAN,dedicated -l dedicated=edge
  ```
  {: pre}

2. Apply a taint to each worker node that prevents pods from running on the worker node and that removes pods that do not have the `edge` label from the worker node. The pods that are removed are redeployed on other worker nodes with capacity.

  ```
  kubectl taint node <node_name> dedicated=edge:NoSchedule dedicated=edge:NoExecute
  ```
  Now, only pods with the `dedicated=edge` toleration are deployed to your edge worker nodes.
