---

copyright:
  years: 2014, 2018
lastupdated: "2018-01-11"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}

# Controlling traffic with network policies
{: #network_policies}

Every Kubernetes cluster is set up with a network plug-in that is called Calico. Default network policies are set up to secure the public network interface of every worker node. You can use Calico and native Kubernetes capabilities to configure more network policies for a cluster when you have unique security requirements. These network policies specify the network traffic that you want to allow or block to and from a pod in a cluster.
{: shortdesc}

You can choose between Calico and native Kubernetes capabilities to create network policies for your cluster. You might use Kubernetes network policies to get started, but for more robust capabilities, use the Calico network policies.

<ul>
  <li>[Kubernetes network policies ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/concepts/services-networking/network-policies/): Some basic options are provided, such as specifying which pods can communicate with each other. Incoming network traffic can be allowed or blocked for a protocol and port. This traffic can be filtered based on the labels and Kubernetes namespaces of the pod that is trying to connect to other pods.</br>These policies can be applied by using `kubectl` commands or the Kubernetes APIs. When these policies are applied, they are converted into Calico network policies and Calico enforces these policies.</li>
  <li>[Calico network policies ![External link icon](../icons/launch-glyph.svg "External link icon")](http://docs.projectcalico.org/v2.6/getting-started/kubernetes/tutorials/advanced-policy): These policies are a superset of the Kubernetes network policies and enhance the native Kubernetes capabilities with the following features.</li>
    <ul><ul><li>Allow or block network traffic on specific network interfaces, not only Kubernetes pod traffic.</li>
    <li>Allow or block incoming (ingress) and outgoing (egress) network traffic.</li>
    <li>[Block incoming (ingress) traffic to LoadBalancer or NodePort Kubernetes services](#block_ingress).</li>
    <li>Allow or block traffic that is based on a source or destination IP address or CIDR.</li></ul></ul></br>

These policies are applied by using `calicoctl` commands. Calico enforces these policies, including any Kubernetes network policies that are converted to Calico policies, by setting up Linux iptables rules on the Kubernetes worker nodes. Iptables rules serve as a firewall for the worker node to define the characteristics that the network traffic must meet to be forwarded to the targeted resource.</ul>

<br />


## Default policy configuration
{: #default_policy}

When a cluster is created, default network policies are automatically set up for the public network interface of each worker node to limit incoming traffic for a worker node from the public internet. These policies do not affect pod to pod traffic and are set up to allow access to the Kubernetes nodeport, load balancer, and Ingress services.
{:shortdesc}

Default policies are not applied to pods directly; they are applied to the public network interface of a worker node by using a Calico host endpoint. When a host endpoint is created in Calico, all traffic to and from that worker node's network interface is blocked, unless that traffic is allowed by a policy.

**Important:** Do not remove policies that are applied to a host endpoint unless you fully understand the policy and know that you do not need the traffic that is being allowed by the policy.


 <table summary="The first row in the table spans both columns. The rest of the rows should be read left to right, with the server location in column one and IP addresses to match in column two.">
 <caption>Default policies for each cluster</caption>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Default policies for each cluster</th>
  </thead>
  <tbody>
    <tr>
      <td><code>allow-all-outbound</code></td>
      <td>Allows all outbound traffic.</td>
    </tr>
    <tr>
      <td><code>allow-bixfix-port</code></td>
      <td>Allows incoming traffic on port 52311 to the bigfix app to allow necessary worker node updates.</td>
    </tr>
    <tr>
      <td><code>allow-icmp</code></td>
      <td>Allows incoming icmp packets (pings).</td>
     </tr>
    <tr>
      <td><code>allow-node-port-dnat</code></td>
      <td>Allows incoming nodeport, load balancer, and ingress service traffic to the pods that those services are exposing. Note the port that those services expose on the public interface does not have to be specified because Kubernetes uses destination network address translation (DNAT) to forward those service requests to the correct pods. That forwarding takes place before the host endpoint policies are applied in iptables.</td>
   </tr>
   <tr>
      <td><code>allow-sys-mgmt</code></td>
      <td>Allows incoming connections for specific IBM Cloud infrastructure (SoftLayer) systems that are used to manage the worker nodes.</td>
   </tr>
   <tr>
    <td><code>allow-vrrp</code></td>
    <td>Allow vrrp packets, which are used to monitor and move virtual IP addresses between worker nodes.</td>
   </tr>
  </tbody>
</table>

<br />


## Adding network policies
{: #adding_network_policies}

In most cases, the default policies do not need to be changed. Only advanced scenarios might require changes. If you find that you must make changes, install the Calico CLI and create your own network policies.
{:shortdesc}

Before you begin:

1.  [Install the {{site.data.keyword.containershort_notm}} and Kubernetes CLIs.](cs_cli_install.html#cs_cli_install)
2.  [Create a lite or standard cluster.](cs_clusters.html#clusters_ui)
3.  [Target the Kubernetes CLI to the cluster](cs_cli_install.html#cs_cli_configure). Include the `--admin` option with the `bx cs cluster-config` command, which is used to download the certificates and permission files. This download also includes the keys for the Super User role, which you need to run Calico commands.

  ```
  bx cs cluster-config <cluster_name> --admin
  ```
  {: pre}

  **Note**: Calico CLI version 1.6.1 is supported.

To add network policies:
1.  Install the Calico CLI.
    1.  [Download the Calico CLI ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/projectcalico/calicoctl/releases/tag/v1.6.1).

        **Tip:** If you are using Windows, install the Calico CLI in the same directory as the {{site.data.keyword.Bluemix_notm}} CLI. This setup saves you some filepath changes when you run commands later.

    2.  For OSX and Linux users, complete the following steps.
        1.  Move the executable file to the /usr/local/bin directory.
            -   Linux:

              ```
              mv /<path_to_file>/calicoctl /usr/local/bin/calicoctl
              ```
              {: pre}

            -   OS X:

              ```
              mv /<path_to_file>/calicoctl-darwin-amd64 /usr/local/bin/calicoctl
              ```
              {: pre}

        2.  Make the file executable.

            ```
            chmod +x /usr/local/bin/calicoctl
            ```
            {: pre}

    3.  Verify that the `calico` commands ran properly by checking the Calico CLI client version.

        ```
        calicoctl version
        ```
        {: pre}
        
    4.  If corporate network policies prevent access from your local system to public endpoints via proxies or firewalls, see [Running `calicoctl` commands from behind a firewall](cs_firewall.html#firewall) for instructions on how to allow TCP access for Calico commands.

2.  Configure the Calico CLI.

    1.  For Linux and OS X, create the `/etc/calico` directory. For Windows, any directory can be used.

      ```
      sudo mkdir -p /etc/calico/
      ```
      {: pre}

    2.  Create a `calicoctl.cfg` file.
        -   Linux and OS X:

          ```
          sudo vi /etc/calico/calicoctl.cfg
          ```
          {: pre}

        -   Windows: Create the file with a text editor.

    3.  Enter the following information in the <code>calicoctl.cfg</code> file.

        ```
        apiVersion: v1
        kind: calicoApiConfig
        metadata:
        spec:
            etcdEndpoints: <ETCD_URL>
            etcdKeyFile: <CERTS_DIR>/admin-key.pem
            etcdCertFile: <CERTS_DIR>/admin.pem
            etcdCACertFile: <CERTS_DIR>/<ca-*pem_file>
        ```
        {: codeblock}

        1.  Retrieve the `<ETCD_URL>`. If this command fails with a `calico-config not found` error, then see this [troubleshooting topic](cs_troubleshoot.html#cs_calico_fails).

          -   Linux and OS X:

              ```
              kubectl get cm -n kube-system calico-config -o yaml | grep "etcd_endpoints:" | awk '{ print $2 }'
              ```
              {: pre}

          -   Output example:

              ```
              https://169.1.1.1:30001
              ```
              {: screen}

          -   Windows:
            <ol>
            <li>Get the calico configuration values from the config map. </br><pre class="codeblock"><code>kubectl get cm -n kube-system calico-config -o yaml</code></pre></br>
            <li>In the `data` section, locate the etcd_endpoints value. Example: <code>https://169.1.1.1:30001</code>
            </ol>

        2.  Retrieve the `<CERTS_DIR>`, the directory that the Kubernetes certificates are downloaded in.

            -   Linux and OS X:

              ```
              dirname $KUBECONFIG
              ```
              {: pre}

                Output example:

              ```
              /home/sysadmin/.bluemix/plugins/container-service/clusters/<cluster_name>-admin/
              ```
              {: screen}

            -   Windows:

              ```
              ECHO %KUBECONFIG%
              ```
              {: pre}

                Output example:

              ```
              C:/Users/<user>/.bluemix/plugins/container-service/<cluster_name>-admin/kube-config-prod-<location>-<cluster_name>.yml
              ```
              {: screen}

            **Note**: To get the directory path, remove the file name `kube-config-prod-<location>-<cluster_name>.yml` from the end of the output.

        3.  Retrieve the <code>ca-*pem_file<code>.

            -   Linux and OS X:

              ```
              ls `dirname $KUBECONFIG` | grep "ca-"
              ```
              {: pre}

            -   Windows:
              <ol><li>Open the directory you retrieved in the last step.</br><pre class="codeblock"><code>C:\Users\<user>\.bluemix\plugins\container-service\&lt;cluster_name&gt;-admin\</code></pre>
              <li> Locate the <code>ca-*pem_file</code> file.</ol>

        4.  Verify that the Calico configuration is working correctly.

            -   Linux and OS X:

              ```
              calicoctl get nodes
              ```
              {: pre}

            -   Windows:

              ```
              calicoctl get nodes --config=<path_to_>/calicoctl.cfg
              ```
              {: pre}

              Output:

              ```
              NAME
              kube-dal10-crc21191ee3997497ca90c8173bbdaf560-w1.cloud.ibm
              kube-dal10-crc21191ee3997497ca90c8173bbdaf560-w2.cloud.ibm
              kube-dal10-crc21191ee3997497ca90c8173bbdaf560-w3.cloud.ibm
              ```
              {: screen}

3.  Examine the existing network policies.

    -   View the Calico host endpoint.

      ```
      calicoctl get hostendpoint -o yaml
      ```
      {: pre}

    -   View all of the Calico and Kubernetes network policies that were created for the cluster. This list includes policies that might not be applied to any pods or hosts yet. For a network policy to be enforced, it must find a Kubernetes resource that matches the selector that was defined in the Calico network policy.

      ```
      calicoctl get policy -o wide
      ```
      {: pre}

    -   View details for a network policy.

      ```
      calicoctl get policy -o yaml <policy_name>
      ```
      {: pre}

    -   View the details of all network policies for the cluster.

      ```
      calicoctl get policy -o yaml
      ```
      {: pre}

4.  Create the Calico network policies to allow or block traffic.

    1.  Define your [Calico network policy ![External link icon](../icons/launch-glyph.svg "External link icon")](http://docs.projectcalico.org/v2.6/reference/calicoctl/resources/policy) by creating a configuration script (.yaml). These configuration files include the selectors that describe what pods, namespaces, or hosts that these policies apply to. Refer to these [sample Calico policies ![External link icon](../icons/launch-glyph.svg "External link icon")](http://docs.projectcalico.org/v2.6/getting-started/kubernetes/tutorials/advanced-policy) to help you create your own.

    2.  Apply the policies to the cluster.
        -   Linux and OS X:

          ```
          calicoctl apply -f <policy_file_name.yaml>
          ```
          {: pre}

        -   Windows:

          ```
          calicoctl apply -f <path_to_>/<policy_file_name.yaml> --config=<path_to_>/calicoctl.cfg
          ```
          {: pre}

<br />


## Block incoming traffic to LoadBalancer or NodePort services.
{: #block_ingress}

By default, Kubernetes `NodePort` and `LoadBalancer` services are designed to make your app available on all public and private cluster interfaces. However, you can block incoming traffic to your services based on traffic source or destination. To block traffic, create Calico `preDNAT` network policies.
{:shortdesc}

A Kubernetes LoadBalancer service is also a NodePort service. A LoadBalancer service makes your app available over the load balancer IP address and port and makes your app available over the service's node port(s). Node ports are accessible on every IP address (public and private) for every node within the cluster.

The cluster administrator can use Calico `preDNAT` network policies to block:

  - Traffic to NodePort services. Traffic to LoadBalancer services is allowed.
  - Traffic that is based on a source address or CIDR.

Some common uses for Calico `preDNAT` network policies:

  - Block traffic to public node ports of a private LoadBalancer service.
  - Block traffic to public node ports on clusters that are running [edge worker nodes](cs_edge.html#edge). Blocking node ports ensures that the edge worker nodes are the only worker nodes that handle incoming traffic.

The `preDNAT` network policies are useful because default Kubernetes and Calico policies are difficult to apply to protecting Kubernetes NodePort and LoadBalancer services due to the DNAT iptables rules generated for these services.

Calico `preDNAT` network policies generate iptables rules based on a [Calico
network policy resource ![External link icon](../icons/launch-glyph.svg "External link icon")](https://docs.projectcalico.org/v2.6/reference/calicoctl/resources/policy).

1. Define a Calico `preDNAT` network policy for ingress access to Kubernetes services.

  Example that blocks all node ports:

  ```
  apiVersion: v1
  kind: policy
  metadata:
    name: deny-kube-node-port-services
  spec:
    preDNAT: true
    selector: ibm.role in { 'worker_public', 'master_public' }
    ingress:
    - action: deny
      protocol: tcp
      destination:
        ports:
        - 30000:32767
    - action: deny
      protocol: udp
      destination:
        ports:
        - 30000:32767
  ```
  {: codeblock}

2. Apply the Calico preDNAT network policy. It takes about 1 minute for the
policy changes to be applied throughout the cluster.

  ```
  calicoctl apply -f deny-kube-node-port-services.yaml
  ```
  {: pre}
