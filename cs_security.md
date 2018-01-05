---

copyright:
  years: 2014, 2018
lastupdated: "2018-01-05"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}


# Security for {{site.data.keyword.containerlong_notm}}
{: #cs_security}

You can use built-in security features for risk analysis and security protection. These features help you to protect your cluster infrastructure and network communication, isolate your compute resources, and ensure security compliance across your infrastructure components and container deployments.
{: shortdesc}

## Security by cluster component
{: #cs_security_cluster}

Each {{site.data.keyword.containerlong_notm}} cluster has security features that are built into its [master](#cs_security_master) and [worker](#cs_security_worker) nodes. If you have a firewall, need to access load balancing from outside the cluster, or want to run `kubectl` commands from your local system when corporate network policies prevent access to public internet endpoints, [open ports in your firewall](#opening_ports). If you want to connect apps in your cluster to an on-premises network or to other apps external to your cluster, [set up VPN connectivity](#vpn).
{: shortdesc}

In the following diagram, you can see security features that are grouped by Kubernetes master, worker nodes, and container images.

<img src="images/cs_security.png" width="400" alt="{{site.data.keyword.containershort_notm}} cluster security" style="width:400px; border-style: none"/>


  <table summary="The first row in the table spans both columns. The remaining rows are to be read left to right, with the server location in column one and IP addresses to match in column two.">
  <caption>Table 1. Security features</caption>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Built-in cluster security settings in {{site.data.keyword.containershort_notm}}</th>
  </thead>
  <tbody>
    <tr>
      <td>Kubernetes master</td>
      <td>The Kubernetes master in each cluster is managed by IBM and is highly available. It includes {{site.data.keyword.containershort_notm}} security settings that ensure security compliance and secure communication to and from the worker nodes. Updates are performed by IBM as required. The dedicated Kubernetes master centrally controls and monitors all Kubernetes resources in the cluster. Based on the deployment requirements and capacity in the cluster, the Kubernetes master automatically schedules your containerized apps to deploy across available worker nodes. For more information, see [Kubernetes master security](#cs_security_master).</td>
    </tr>
    <tr>
      <td>Worker node</td>
      <td>Containers are deployed on worker nodes that are dedicated to a cluster and that ensure compute, network, and storage isolation for IBM customers. {{site.data.keyword.containershort_notm}} provides built-in security features to keep your worker nodes secure on the private and public network and to ensure worker node security compliance. For more information, see [Worker node security](#cs_security_worker). In addition, you can add [Calico network policies](#cs_security_network_policies) to further specify the network traffic that you want to allow or block to and from a pod on a worker node. </td>
     </tr>
     <tr>
      <td>Images</td>
      <td>As the cluster admin, you can set up your own secure Docker image repository in {{site.data.keyword.registryshort_notm}} where you can store and share Docker images between your cluster users. To ensure safe container deployments, every image in your private registry is scanned by Vulnerability Advisor. Vulnerability Advisor is a component of {{site.data.keyword.registryshort_notm}} that scans for potential vulnerabilities, makes security recommendations, and provides instructions to resolve vulnerabilities. For more information, see [Image security in {{site.data.keyword.containershort_notm}}](#cs_security_deployment).</td>
    </tr>
  </tbody>
</table>

### Kubernetes master
{: #cs_security_master}

Review the built-in Kubernetes master security features to protect the Kubernetes master and to secure the cluster network communication.
{: shortdesc}

<dl>
  <dt>Fully managed and dedicated Kubernetes master</dt>
    <dd>Every Kubernetes cluster in {{site.data.keyword.containershort_notm}} is controlled by a dedicated Kubernetes master that is managed by IBM in an IBM-owned IBM Cloud infrastructure (SoftLayer) account. The Kubernetes master is set up with the following dedicated components that are not shared with other IBM customers.
    <ul><li>etcd data store: Stores all Kubernetes resources of a cluster, such as Services, Deployments, and Pods. Kubernetes ConfigMaps and Secrets are app data that are stored as key value pairs so that they can be used by an app that runs in a pod. Data in etcd is stored on an encrypted disk that is managed by IBM and is encrypted via TLS when sent to a pod to assure data protection and integrity.</li>
    <li>kube-apiserver: Serves as the main entry point for all requests from the worker node to the Kubernetes master. The kube-apiserver validates and processes requests and can read from and write to the etcd data store.</li>
    <li>kube-scheduler: Decides where to deploy pods, taking into account capacity and performance needs, hardware and software policy constraints, anti-affinity specifications, and workload requirements. If no worker node can be found that matches the requirements, the pod is not deployed in the cluster.</li>
    <li>kube-controller-manager: Responsible for monitoring replica sets, and creating corresponding pods to achieve the desired state.</li>
    <li>OpenVPN: {{site.data.keyword.containershort_notm}}-specific component to provide secured network connectivity for all Kubernetes master to worker node communication.</li></ul></dd>
  <dt>TLS secured network connectivity for all worker node to Kubernetes master communication</dt>
    <dd>To secure the network communication to the Kubernetes master, {{site.data.keyword.containershort_notm}} generates TLS certificates that encrypts the communication to and from the kube-apiserver and etcd data store components for every cluster. These certificates are never shared across clusters or across Kubernetes master components.</dd>
  <dt>OpenVPN secured network connectivity for all Kubernetes master to worker node communication</dt>
    <dd>Although Kubernetes secures the communication between the Kubernetes master and worker nodes by using the `https` protocol, no authentication is provided on the worker node by default. To secure this communication, {{site.data.keyword.containershort_notm}} automatically sets up an OpenVPN connection between the Kubernetes master and the worker node when the cluster is created.</dd>
  <dt>Continuous Kubernetes master network monitoring</dt>
    <dd>Every Kubernetes master is continuously monitored by IBM to control and remediate process level Denial-Of-Service (DOS) attacks.</dd>
  <dt>Kubernetes master node security compliance</dt>
    <dd>{{site.data.keyword.containershort_notm}} automatically scans every node where the Kubernetes master is deployed for vulnerabilities found in Kubernetes and OS-sepcific security fixes that need to be applied to assure master node protection. If vulnerabilities are found, {{site.data.keyword.containershort_notm}} automatically applies fixes and resolves vulnerabilities on behalf of the user.</dd>
</dl>

<br />


### Worker nodes
{: #cs_security_worker}

Review the built-in worker node security features to protect the worker node environment and to assure resource, network and storage isolation.
{: shortdesc}

<dl>
  <dt>Compute, network and storage infrastructure isolation</dt>
    <dd>When you create a cluster, virtual machines are provisioned as worker nodes in the customer IBM Cloud infrastructure (SoftLayer) account or in the dedicated IBM Cloud infrastructure (SoftLayer) account by IBM. Worker nodes are dedicated to a cluster and do not host workloads of other clusters.</br> Every {{site.data.keyword.Bluemix_notm}} account is set up with IBM Cloud infrastructure (SoftLayer) VLANs to assure quality network performance and isolation on the worker nodes. </br>To persist data in your cluster, you can provision dedicated NFS based file storage from IBM Cloud infrastructure (SoftLayer) and leverage the built-in data security features of that platform.</dd>
  <dt>Secured worker node set up</dt>
    <dd>Every worker node is set up with an Ubuntu operating system that cannot be changed by the user. To protect the operating system of the worker nodes from potential attacks, every worker node is configured with expert firewall settings that are enforced by Linux iptable rules.</br> All containers that run on Kubernetes are protected by predefined Calico network policy settings that are configured on every worker node during cluster creation. This set up ensures secure network communication between worker nodes and pods. To further restrict the actions that a container can perform on the worker node, users can choose to configure [AppArmor policies ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/tutorials/clusters/apparmor/) on the worker nodes.</br> SSH access is disabled on the worker node. If you want to install additional features on your worker node, you can use [Kubernetes daemon sets ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset) for everything that you want to run on every worker node, or [Kubernetes jobs ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/concepts/workloads/controllers/jobs-run-to-completion/) for any one-time action you must execute.</dd>
  <dt>Kubernetes worker node security compliance</dt>
    <dd>IBM works with internal and external security advisory teams to address potential security compliance vulnerabilities. IBM maintains access to the worker nodes in order to deploy updates and security patches to the operating system.</br> <b>Important</b>: Reboot your worker nodes on a regular basis to ensure the installation of the updates and security patches that are automatically deployed to the operating system. IBM does not reboot your worker nodes.</dd>
  <dt>Encrypted disk</dt>
  <dd>By default, {{site.data.keyword.containershort_notm}} provides two local SSD encrypted data partitions for all worker nodes when the worker nodes are provisioned. The first partition is not encrypted, and the second partition mounted to _/var/lib/docker_ is unlocked by using LUKS encryption keys. Each worker in each Kubernetes cluster has its own unique LUKS encryption key, managed by {{site.data.keyword.containershort_notm}}. When you create a cluster or add a worker node to an existing cluster, the keys are pulled securely and then discarded after the encrypted disk is unlocked.
  <p><b>Note</b>: Encryption can impact disk I/O performance. For workloads that require high-performance disk I/O, test a cluster with encryption both enabled and disabled to help you decide whether to turn off encryption.</p>
  </dd>
  <dt>Support for IBM Cloud infrastructure (SoftLayer) network firewalls</dt>
    <dd>{{site.data.keyword.containershort_notm}} is compatible with all [IBM Cloud infrastructure (SoftLayer) firewall offerings ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/cloud-computing/bluemix/network-security). On {{site.data.keyword.Bluemix_notm}} Public, you can set up a firewall with custom network policies to provide dedicated network security for your cluster and to detect and remediate network intrusion. For example, you might choose to set up a [Vyatta ![External link icon](../icons/launch-glyph.svg "External link icon")](https://knowledgelayer.softlayer.com/topic/vyatta-1) to act as your firewall and block unwanted traffic. When you set up a firewall, [you must also open up the required ports and IP addresses](#opening_ports) for each region so that the master and the worker nodes can communicate.</dd>
  <dt>Keep services private or selectively expose services and apps to the public internet</dt>
    <dd>You can choose to keep your services and apps private and leverage the built-in security features described in this topic to assure secured communication between worker nodes and pods. To expose services and apps to the public internet, you can leverage the Ingress and load balancer support to securely make your services publicly available.</dd>
  <dt>Securely connect your worker nodes and apps to an on-premises data center</dt>
  <dd>To connect your worker nodes and apps to an on-premises data center, you can configure a VPN IPSec endpoint with a Strongswan service or with a Vyatta Gateway Appliance or a Fortigate Appliance.<br><ul><li><b>Strongswan IPSec VPN Service</b>: You can set up a [Strongswan IPSec VPN service ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.strongswan.org/) that securely connects your Kubernetes cluster with an on-premises network. The Strongswan IPSec VPN service provides a secure end-to-end communication channel over the internet that is based on the industry-standard Internet Protocol Security (IPsec) protocol suite. To set up a secure connection between your cluster and an on-premises network, you must have an IPsec VPN gateway or IBM Cloud infrastructure (SoftLayer) server installed in your on-premises data center. Then you can [configure and deploy the Strongswan IPSec VPN service](cs_security.html#vpn) in a Kubernetes pod.</li><li><b>Vyatta Gateway Appliance or Fortigate Appliance</b>: If you have a larger cluster, you might choose to set up a Vyatta Gateway Appliance or Fortigate Appliance to configure an IPSec VPN endpoint. For more information, see this blog post on [Connecting a cluster to an on-premise data center ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/blogs/bluemix/2017/07/kubernetes-and-bluemix-container-based-workloads-part4/).</li></ul></dd>
  <dt>Continuous monitoring and logging of cluster activity</dt>
    <dd>For standard clusters, all cluster-related events, such as adding a worker node, rolling update progress, or capacity usage information can be logged and monitored by {{site.data.keyword.containershort_notm}} and sent to {{site.data.keyword.loganalysislong_notm}} and {{site.data.keyword.monitoringlong_notm}}. For information about setting up logging and monitoring, see [Configuring cluster logging](https://console.bluemix.net/docs/containers/cs_cluster.html#cs_logging) and [Configuring cluster monitoring](https://console.bluemix.net/docs/containers/cs_cluster.html#cs_monitoring).</dd>
</dl>

### Images
{: #cs_security_deployment}

Manage the security and integrity of your images with built-in security features.
{: shortdesc}

<dl>
<dt>Secured Docker private image repository in {{site.data.keyword.registryshort_notm}}</dt>
<dd>You can set up your own Docker image repository in a multi-tenant, highly available, and scalable private image registry that is hosted and managed by IBM to build, securely store, and share Docker images across cluster users.</dd>

<dt>Image security compliance</dt>
<dd>When you use {{site.data.keyword.registryshort_notm}}, you can leverage the built-in security scanning that is provided by Vulnerability Advisor. Every image that is pushed to your namespace is automatically scanned for vulnerabilities against a database of known CentOS, Debian, Red Hat, and Ubuntu issues. If vulnerabilities are found, Vulnerability Advisor provides instructions for how to resolve them to assure image integrity and security.</dd>
</dl>

To view the vulnerability assessment for your images, [review the Vulnerability Advisor documentation](/docs/services/va/va_index.html#va_registry_cli).

<br />


## Opening required ports and IP addresses in your firewall
{: #opening_ports}

Review these situations in which you might need to open specific ports and IP addresses in your firewalls:
* [To run `bx` commands](#firewall_bx) from your local system when corporate network policies prevent access to public internet endpoints via proxies or firewalls.
* [To run `kubectl` commands](#firewall_kubectl) from your local system when corporate network policies prevent access to public internet endpoints via proxies or firewalls.
* [To run `calicoctl` commands](#firewall_calicoctl) from your local system when corporate network policies prevent access to public internet endpoints via proxies or firewalls.
* [To allow communication between the Kubernetes master and the worker nodes](#firewall_outbound) when either a firewall is set up for the worker nodes or the firewall settings are customized in your IBM Cloud infrastructure (SoftLayer) account.
* [To access the NodePort service, LoadBalancer service, or Ingress from outside of the cluster](#firewall_inbound).

### Running `bx cs` commands from behind a firewall
{: #firewall_bx}

If corporate network policies prevent access from your local system to public endpoints via proxies or firewalls, to run `bx cs` commands, you must allow TCP access for {{site.data.keyword.containerlong_notm}}.

1. Allow access to `containers.bluemix.net` on port 443.
2. Verify your connection. If access is configured correctly, ships are displayed in the output.
   ```
   curl https://containers.bluemix.net/v1/
   ```
   {: pre}

   Example output:
   ```
                                     )___(
                              _______/__/_
                     ___     /===========|   ___
    ____       __   [\\\]___/____________|__[///]   __
    \   \_____[\\]__/___________________________\__[//]___
     \                                                    |
      \                                                  /
   ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

   ```
   {: screen}

### Running `kubectl` commands from behind a firewall
{: #firewall_kubectl}

If corporate network policies prevent access from your local system to public endpoints via proxies or firewalls, to run `kubectl` commands, you must allow TCP access for the cluster.

When a cluster is created, the port in the master URL is randomly assigned from within 20000-32767. You can either choose to open port range 20000-32767 for any cluster that might get created or you can choose to allow access for a specific existing cluster.

Before you begin, allow access to [run `bx cs` commands](#firewall_bx).

To allow access for a specific cluster:

1. Log in to the {{site.data.keyword.Bluemix_notm}} CLI. Enter your {{site.data.keyword.Bluemix_notm}} credentials when prompted. If you have a federated account, include the `--sso` option.

    ```
    bx login [--sso]
    ```
    {: pre}

2. Select the region that your cluster is in.

   ```
   bx cs region-set
   ```
   {: pre}

3. Get the name of your cluster.

   ```
   bx cs clusters
   ```
   {: pre}

4. Retrieve the **Master URL** for your cluster.

   ```
   bx cs cluster-get <cluster_name_or_id>
   ```
   {: pre}

   Example output:
   ```
   ...
   Master URL:		https://169.46.7.238:31142
   ...
   ```
   {: screen}

5. Allow access to the **Master URL** on the port, such as port `31142` in the previous example.

6. Verify your connection.

   ```
   curl --insecure <master_URL>/version
   ```
   {: pre}

   Example command:
   ```
   curl --insecure https://169.46.7.238:31142/version
   ```
   {: pre}

   Example output:
   ```
   {
     "major": "1",
     "minor": "7+",
     "gitVersion": "v1.7.4-2+eb9172c211dc41",
     "gitCommit": "eb9172c211dc4108341c0fd5340ee5200f0ec534",
     "gitTreeState": "clean",
     "buildDate": "2017-11-16T08:13:08Z",
     "goVersion": "go1.8.3",
     "compiler": "gc",
     "platform": "linux/amd64"
   }
   ```
   {: screen}

7. Optional: Repeat these steps for each cluster that you need to expose.

### Running `calicoctl` commands from behind a firewall
{: #firewall_calicoctl}

If corporate network policies prevent access from your local system to public endpoints via proxies or firewalls, to run `calicoctl` commands, you must allow TCP access for the Calico commands.

Before you begin, allow access to run [`bx` commands](#firewall_bx) and [`kubectl` commands](#firewall_kubectl).

1. Retrieve the IP address from the master URL that you used to allow the [`kubectl` commands](#firewall_kubectl).

2. Get the port for ETCD.

  ```
  kubectl get cm -n kube-system calico-config -o yaml | grep etcd_endpoints
  ```
  {: pre}

3. Allow access for the Calico policies via the master URL IP address and the ETCD port.

### Allowing the cluster to access infrastructure resources and other services
{: #firewall_outbound}

  1.  Note the public IP address for all your worker nodes in the cluster.

      ```
      bx cs workers <cluster_name_or_id>
      ```
      {: pre}

  2.  Allow outgoing network traffic from the source _<each_worker_node_publicIP>_ to the destination TCP/UDP port range 20000-32767 and port 443, and the following IP addresses and network groups. If you have a corporate firewall that prevents your local machine from accessing public internet endpoints, do this step for both your source worker nodes and your local machine.
      - **Important**: You must allow outgoing traffic to port 443 for all of the locations within the region, to balance the load during the bootstrapping process. For example, if your cluster is in US South, you must allow traffic from port 443 to the IP addresses for all of the locations (dal10, dal12, and dal13).
      <p>
  <table summary="The first row in the table spans both columns. The rest of the rows should be read left to right, with the server location in column one and IP addresses to match in column two.">
      <thead>
      <th>Region</th>
      <th>Location</th>
      <th>IP address</th>
      </thead>
    <tbody>
      <tr>
        <td>AP North</td>
        <td>hkg02<br>sng01<br>tok02</td>
        <td><code>169.56.132.234</code><br><code>161.202.186.226</code><br><code>161.202.126.210</code></td>
       </tr>
      <tr>
         <td>AP South</td>
         <td>mel01<br>syd01<br>syd04</td>
         <td><code>168.1.97.67</code><br><code>168.1.8.195</code><br><code>130.198.64.19, 130.198.66.34</code></td>
      </tr>
      <tr>
         <td>EU Central</td>
         <td>ams03<br>fra02<br>mil01<br>par01</td>
         <td><code>169.50.169.106, 169.50.154.194</code><br><code>169.50.56.170, 169.50.56.174</code><br><code>159.122.190.98</code><br><code>159.8.86.149, 159.8.98.170</code></td>
        </tr>
      <tr>
        <td>UK South</td>
        <td>lon02<br>lon04</td>
        <td><code>159.122.242.78</code><br><code>158.175.65.170, 158.175.74.170, 158.175.76.2</code></td>
      </tr>
      <tr>
        <td>US East</td>
         <td>tor01<br>wdc06<br>wdc07</td>
         <td><code>169.53.167.50</code><br><code>169.60.73.142</code><br><code>169.61.83.62</code></td>
      </tr>
      <tr>
        <td>US South</td>
        <td>dal10<br>dal12<br>dal13</td>
        <td><code>169.47.234.18, 169.46.7.234</code><br><code>169.47.70.10</code><br><code>169.60.128.2</code></td>
      </tr>
      </tbody>
    </table>
</p>

  3.  Allow outgoing network traffic from the worker nodes to {{site.data.keyword.registrylong_notm}}:
      - `TCP port 443 FROM <each_worker_node_publicIP> TO <registry_publicIP>`
      - Replace <em>&lt;registry_publicIP&gt;</em> with all of the addresses for registry regions to which you want to allow traffic:
        <p>
<table summary="The first row in the table spans both columns. The rest of the rows should be read left to right, with the server location in column one and IP addresses to match in column two.">
      <thead>
        <th>Container region</th>
        <th>Registry address</th>
        <th>Registry IP address</th>
      </thead>
      <tbody>
        <tr>
          <td>AP North, AP South</td>
          <td>registry.au-syd.bluemix.net</td>
          <td><code>168.1.45.160/27</code></br><code>168.1.139.32/27</code></td>
        </tr>
        <tr>
          <td>EU Central</td>
          <td>registry.eu-de.bluemix.net</td>
          <td><code>169.50.56.144/28</code></br><code>159.8.73.80/28</code></td>
         </tr>
         <tr>
          <td>UK South</td>
          <td>registry.eu-gb.bluemix.net</td>
          <td><code>159.8.188.160/27</code></br><code>169.50.153.64/27</code></td>
         </tr>
         <tr>
          <td>US East, US South</td>
          <td>registry.ng.bluemix.net</td>
          <td><code>169.55.39.112/28</code></br><code>169.46.9.0/27</code></br><code>169.55.211.0/27</code></td>
         </tr>
        </tbody>
      </table>
</p>

  4.  Optional: Allow outgoing network traffic from the worker nodes to {{site.data.keyword.monitoringlong_notm}} and {{site.data.keyword.loganalysislong_notm}} services:
      - `TCP port 443, port 9095 FROM <each_worker_node_publicIP> TO <monitoring_publicIP>`
      - Replace <em>&lt;monitoring_publicIP&gt;</em> with all of the addresses for the monitoring regions to which you want to allow traffic:
        <p><table summary="The first row in the table spans both columns. The rest of the rows should be read left to right, with the server location in column one and IP addresses to match in column two.">
        <thead>
        <th>Container region</th>
        <th>Monitoring address</th>
        <th>Monitoring IP addresses</th>
        </thead>
      <tbody>
        <tr>
         <td>EU Central</td>
         <td>metrics.eu-de.bluemix.net</td>
         <td><code>159.122.78.136/29</code></td>
        </tr>
        <tr>
         <td>UK South</td>
         <td>metrics.eu-gb.bluemix.net</td>
         <td><code>169.50.196.136/29</code></td>
        </tr>
        <tr>
          <td>US East, US South, AP North</td>
          <td>metrics.ng.bluemix.net</td>
          <td><code>169.47.204.128/29</code></td>
         </tr>
         
        </tbody>
      </table>
</p>
      - `TCP port 443, port 9091 FROM <each_worker_node_publicIP> TO <logging_publicIP>`
      - Replace <em>&lt;logging_publicIP&gt;</em> with all of the addresses for the logging regions to which you want to allow traffic:
        <p><table summary="The first row in the table spans both columns. The rest of the rows should be read left to right, with the server location in column one and IP addresses to match in column two.">
        <thead>
        <th>Container region</th>
        <th>Logging address</th>
        <th>Logging IP addresses</th>
        </thead>
        <tbody>
          <tr>
            <td>US East, US South</td>
            <td>ingest.logging.ng.bluemix.net</td>
            <td><code>169.48.79.236</code><br><code>169.46.186.113</code></td>
           </tr>
          <tr>
           <td>EU Central, UK South</td>
           <td>ingest-eu-fra.logging.bluemix.net</td>
           <td><code>158.177.88.43</code><br><code>159.122.87.107</code></td>
          </tr>
          <tr>
           <td>AP South, AP North</td>
           <td>ingest-au-syd.logging.bluemix.net</td>
           <td><code>130.198.76.125</code><br><code>168.1.209.20</code></td>
          </tr>
         </tbody>
       </table>
</p>

  5. For private firewalls, allow the appropriate IBM Cloud infrastructure (SoftLayer) private IP ranges. Consult [this link](https://knowledgelayer.softlayer.com/faq/what-ip-ranges-do-i-allow-through-firewall) beginning with the **Backend (private) Network** section.
      - Add all of the [locations within the regions](cs_regions.html#locations) that you are using.
      - Note that you must add the dal01 location (data center).
      - Open ports 80 and 443 to allow the cluster bootstrapping process.

  6. To create persistent volume claims for data storage, allow egress access through your firewall for the [IBM Cloud infrastructure (SoftLayer) IP addresses](https://knowledgelayer.softlayer.com/faq/what-ip-ranges-do-i-allow-through-firewall) of the location (data center) that your cluster is in.
      - To find the location (data center) of your cluster, run `bx cs clusters`.
      - Allow access to the IP range for both the **Frontend (public) network** and **Backend (private) Network**.
      - Note that you must add the dal01 location (data center) for the **Backend (private) Network**.

### Accessing NodePort, load balancer, and Ingress services from outside the cluster
{: #firewall_inbound}

You can allow incoming access to NodePort, load balancer, and Ingress services.

<dl>
  <dt>NodePort service</dt>
  <dd>Open the port that you configured when you deployed the service to the public IP addresses for all of the worker nodes to allow traffic to. To find the port, run `kubectl get svc`. The port is in the 20000-32000 range.<dd>
  <dt>LoadBalancer service</dt>
  <dd>Open the port that you configured when you deployed the service to the load balancer service's public IP address.</dd>
  <dt>Ingress</dt>
  <dd>Open port 80 for HTTP or port 443 for HTTPS to the IP address for the Ingress application load balancer.</dd>
</dl>

<br />


## Setting up VPN connectivity with the Strongswan IPSec VPN service Helm chart
{: #vpn}

VPN connectivity allows you to securely connect apps in a Kubernetes cluster to an on-premises network. You can also connect apps that are external to your cluster to an app that is running inside your cluster. To set up VPN connectivity, you can use a Helm Chart to configure and deploy the [Strongswan IPSec VPN service ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.strongswan.org/) inside of a Kubernetes pod. All VPN traffic is then routed through this pod. For more information about the Helm commands used to set up the Strongswan chart, see the [Helm documentation ![External link icon](../icons/launch-glyph.svg "External link icon")](https://docs.helm.sh/helm/).

Before you begin:

- [Create a standard cluster.](cs_cluster.html#cs_cluster_cli)
- [If you are using an existing cluster, update it to version 1.7.4 or later.](cs_cluster.html#cs_cluster_update)
- The cluster must have at least one available public Load Balancer IP address.
- [Target the Kubernetes CLI to the cluster](cs_cli_install.html#cs_cli_configure).

To set up VPN connectivity with Strongswan:

1. If it is not already enabled, install and initialize Helm for your cluster.

    1. [Install the Helm CLI ![External link icon](../icons/launch-glyph.svg "External link icon")](https://docs.helm.sh/using_helm/#installing-helm).

    2. Initialize Helm and install `tiller`.

        ```
        helm init
        ```
        {: pre}

    3. Verify that the `tiller-deploy` pod has status `Running` in your cluster.

        ```
        kubectl get pods -n kube-system -l app=helm
        ```
        {: pre}

        Example output:

        ```
        NAME                            READY     STATUS    RESTARTS   AGE
        tiller-deploy-352283156-nzbcm   1/1       Running   0          10m
        ```
        {: screen}

    4. Add the {{site.data.keyword.containershort_notm}} Helm repository to your Helm instance.

        ```
        helm repo add bluemix  https://registry.bluemix.net/helm
        ```
        {: pre}

    5. Verify that the Strongswan chart is listed in the Helm repository.

        ```
        helm search bluemix
        ```
        {: pre}

2. Save the default configuration settings for the Strongswan Helm Chart in a local YAML file.

    ```
    helm inspect values bluemix/strongswan > config.yaml
    ```
    {: pre}

3. Open the `config.yaml` file and make the following changes to the default values according to the VPN configuration you want. If a property has specific values that you can choose from, those values are listed in comments above each property in the file. **Important**: If you do not need to change a property, comment that property out by placing a `#` in front of it.

    <table>
    <caption>Table 2. Understanding the YAML file components</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the YAML file components</th>
    </thead>
    <tbody>
    <tr>
    <td><code>overRideIpsecConf</code></td>
    <td>If you have an existing <code>ipsec.conf</code> file that you want to use, remove the curly brackets (<code>{}</code>) and add the contents of your file after this property. The file contents must be indented. **Note:** If you use your own file, any values for the <code>ipsec</code>, <code>local</code>, and <code>remote</code> sections are not used.</td>
    </tr>
    <tr>
    <td><code>overRideIpsecSecrets</code></td>
    <td>If you have an existing <code>ipsec.secrets</code> file that you want to use, remove the curly brackets (<code>{}</code>) and add the contents of your file after this property. The file contents must be indented. **Note:** If you use your own file, any values for the <code>preshared</code> section are not used.</td>
    </tr>
    <tr>
    <td><code>ipsec.keyexchange</code></td>
    <td>If your on-premises VPN tunnel endpoint does not support <code>ikev2</code> as a protocol for initializing the connection, change this value to <code>ikev1</code>.</td>
    </tr>
    <tr>
    <td><code>ipsec.esp</code></td>
    <td>Change this value to the list of ESP encryption/authentication algorithms your on-premises VPN tunnel endpoint uses for the connection.</td>
    </tr>
    <tr>
    <td><code>ipsec.ike</code></td>
    <td>Change this value to the list of IKE/ISAKMP SA encryption/authentication algorithms your on-premises VPN tunnel endpoint uses for the connection.</td>
    </tr>
    <tr>
    <td><code>ipsec.auto</code></td>
    <td>If you want the cluster to initiate the VPN connection, then change this value to <code>start</code>.</td>
    </tr>
    <tr>
    <td><code>local.subnet</code></td>
    <td>Change this value to the list of cluster subnet CIDRs that should be exposed over the VPN connection to the on-premises network. This list can include the following subnets: <ul><li>The Kubernetes pod subnet CIDR: <code>172.30.0.0/16</code></li><li>The Kubernetes service subnet CIDR: <code>172.21.0.0/16</code></li><li>If you have applications exposed by a NodePort service on the private network, the worker node's private subnet CIDR. To find this value, run <code>bx cs subnets | grep <xxx.yyy.zzz></code> where &lt;xxx.yyy.zzz&gt; is the first 3 octects of the worker node's private IP address.</li><li>If you have applications exposed by LoadBalancer services on the private network, the cluster's private or user-managed subnet CIDRs. To find these values, run <code>bx cs cluster-get <cluster name> --showResources</code>. In the <b>VLANS</b> section, look for CIDRs that have a <b>Public</b> value of <code>false</code>.</li></ul></td>
    </tr>
    <tr>
    <td><code>local.id</code></td>
    <td>Change this value to the string identifier for the local Kubernetes cluster side your VPN tunnel endpoint uses for the connection.</td>
    </tr>
    <tr>
    <td><code>remote.gateway</code></td>
    <td>Change this value to the public IP address for the on-premises VPN gateway.</td>
    </tr>
    <td><code>remote.subnet</code></td>
    <td>Change this value to the list of on-premises private subnet CIDRs that the Kubernetes clusters is allowed to access.</td>
    </tr>
    <tr>
    <td><code>remote.id</code></td>
    <td>Change this value to the string identifier for the remote on-premises side your VPN tunnel endpoint uses for the connection.</td>
    </tr>
    <tr>
    <td><code>preshared.secret</code></td>
    <td>Change this value to the pre-shared secret that your on-premises VPN tunnel endpoint gateway uses for the connection.</td>
    </tr>
    </tbody></table>

4. Save the updated `config.yaml` file.

5. Install the Helm Chart to your cluster with the updated `config.yaml` file. The updated properties are stored in a config map for your chart.

    ```
    helm install -f config.yaml --namespace=kube-system --name=vpn bluemix/strongswan
    ```
    {: pre}

6. Check the chart deployment status. When the chart is ready, the **STATUS** field near the top of the output has a value of `DEPLOYED`.

    ```
    helm status vpn
    ```
    {: pre}

7. Once the chart is deployed, verify that the updated settings in the `config.yaml` file were used.

    ```
    helm get values vpn
    ```
    {: pre}

8. Test the new VPN connectivity.
    1. If the VPN on the on-premises gateway is not active, start the VPN.

    2. Set the `STRONGSWAN_POD` environment variable.

        ```
        export STRONGSWAN_POD=$(kubectl get pod -n kube-system -l app=strongswan,release=vpn -o jsonpath='{ .items[0].metadata.name }')
        ```
        {: pre}

    3. Check the status of the VPN. A status of `ESTABLISHED` means that the VPN connection was successful.

        ```
        kubectl exec -n kube-system  $STRONGSWAN_POD -- ipsec status
        ```
        {: pre}

        Example output:
        ```
        Security Associations (1 up, 0 connecting):
            k8s-conn[1]: ESTABLISHED 17 minutes ago, 172.30.244.42[ibm-cloud]...192.168.253.253[on-prem]
            k8s-conn{2}:  INSTALLED, TUNNEL, reqid 12, ESP in UDP SPIs: c78cb6b1_i c5d0d1c3_o
            k8s-conn{2}:   172.21.0.0/16 172.30.0.0/16 === 10.91.152.128/26
        ```
        {: screen}

        **Note**:
          - It is highly likely that the VPN will not have a status of `ESTABLISHED` the first time you use this Helm Chart. You might need to check the on-premises VPN endpoint settings and return to step 3 to change the `config.yaml` file several times before the connection is successful.
          - If the VPN pod is in an `ERROR` state or continues to crash and restart, it might be due to parameter validation of the `ipsec.conf` settings in the chart's config map. To see if this is the case, check for any validation errors in the Strongswan pod logs by running `kubectl logs -n kube-system $STRONGSWAN_POD`. If there are validation errors, run `helm delete --purge vpn`, return to step 3 to fix the incorrect values in the `config.yaml` file, and repeat steps 4 through 8. If your cluster has a high number of worker nodes, you can also use `helm upgrade` to more quickly apply your changes instead of running `helm delete` and `helm install`.

    4. Once the VPN has a status of `ESTABLISHED`, test the connection with `ping`. The following example sends a ping from the VPN pod in the Kubernetes cluster to the private IP address of the on-premises VPN gateway. Make sure that the correct `remote.subnet` and `local.subnet` are specified in the configuration file, and that the local subnet list includes the source IP address from which you are sending the ping.

        ```
        kubectl exec -n kube-system  $STRONGSWAN_POD -- ping -c 3  <on-prem_gateway_private_IP>
        ```
        {: pre}

To disable the Strongswan IPSec VPN service:

1. Delete the Helm Chart.

    ```
    helm delete --purge vpn
    ```
    {: pre}

<br />


## Network policies
{: #cs_security_network_policies}

Every Kubernetes cluster is set up with a network plug-in that is called Calico. Default network policies are set up to secure the public network interface of every worker node. You can use Calico and native Kubernetes capabilities to configure more network policies for a cluster when you have unique security requirements. These network policies specify the network traffic that you want to allow or block to and from a pod in a cluster.
{: shortdesc}

You can choose between Calico and native Kubernetes capabilities to create network policies for your cluster. You might use Kubernetes network policies to get started, but for more robust capabilities, use the Calico network policies.

<ul>
  <li>[Kubernetes network policies ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/concepts/services-networking/network-policies/): Some basic options are provided, such as specifying which pods can communicate with each other. Incoming network traffic can be allowed or blocked for a protocol and port. This traffic can be filtered based on the labels and Kubernetes namespaces of the pod that is trying to connect to other pods.</br>These policies can be applied by using `kubectl` commands or the Kubernetes APIs. When these policies are applied, they are converted into Calico network policies and Calico enforces these policies.</li>
  <li>[Calico network policies ![External link icon](../icons/launch-glyph.svg "External link icon")](http://docs.projectcalico.org/v2.4/getting-started/kubernetes/tutorials/advanced-policy): These policies are a superset of the Kubernetes network policies and enhance the native Kubernetes capabilities with the following features.</li>
    <ul><ul><li>Allow or block network traffic on specific network interfaces, not only Kubernetes pod traffic.</li>
    <li>Allow or block incoming (ingress) and outgoing (egress) network traffic.</li>
    <li>[Block incoming (ingress) traffic to LoadBalancer or NodePort Kubernetes services](#cs_block_ingress).</li>
    <li>Allow or block traffic that is based on a source or destination IP address or CIDR.</li></ul></ul></br>

These policies are applied by using `calicoctl` commands. Calico enforces these policies, including any Kubernetes network policies that are converted to Calico policies, by setting up Linux iptables rules on the Kubernetes worker nodes. Iptables rules serve as a firewall for the worker node to define the characteristics that the network traffic must meet to be forwarded to the targeted resource.</ul>


### Default policy configuration
{: #concept_nq1_2rn_4z}

When a cluster is created, default network policies are automatically set up for the public network interface of each worker node to limit incoming traffic for a worker node from the public internet. These policies do not affect pod to pod traffic and are set up to allow access to the Kubernetes nodeport, load balancer, and Ingress services.

Default policies are not applied to pods directly; they are applied to the public network interface of a worker node by using a Calico host endpoint. When a host endpoint is created in Calico, all traffic to and from that worker node's network interface is blocked, unless that traffic is allowed by a policy.

**Important:** Do not remove policies that are applied to a host endpoint unless you fully understand the policy and know that you do not need the traffic that is being allowed by the policy.


 <table summary="The first row in the table spans both columns. The rest of the rows should be read left to right, with the server location in column one and IP addresses to match in column two.">
 <caption>Table 3. Default policies for each cluster</caption>
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


### Adding network policies
{: #adding_network_policies}

In most cases, the default policies do not need to be changed. Only advanced scenarios might require changes. If you find that you must make changes, install the Calico CLI and create your own network policies

Before you begin:

1.  [Install the {{site.data.keyword.containershort_notm}} and Kubernetes CLIs.](cs_cli_install.html#cs_cli_install)
2.  [Create a lite or standard cluster.](cs_cluster.html#cs_cluster_ui)
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

    1.  Define your [Calico network policy ![External link icon](../icons/launch-glyph.svg "External link icon")](http://docs.projectcalico.org/v2.1/reference/calicoctl/resources/policy) by creating a configuration script (.yaml). These configuration files include the selectors that describe what pods, namespaces, or hosts that these policies apply to. Refer to these [sample Calico policies ![External link icon](../icons/launch-glyph.svg "External link icon")](http://docs.projectcalico.org/v2.0/getting-started/kubernetes/tutorials/advanced-policy) to help you create your own.

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

### Block incoming traffic to LoadBalancer or NodePort services.
{: #cs_block_ingress}

By default, Kubernetes `NodePort` and `LoadBalancer` services are designed to make your app available on all public and private cluster interfaces. However, you can block incoming traffic to your services based on traffic source or destination. To block traffic, create Calico `preDNAT` network policies.

A Kubernetes LoadBalancer service is also a NodePort service. A LoadBalancer service makes your app available over the load balancer IP address and port and makes your app available over the service's node port(s). Node ports are accessible on every IP address (public and private) for every node within the cluster.

The cluster administrator can use Calico `preDNAT` network policies to block:

  - Traffic to NodePort services. Traffic to LoadBalancer services is allowed.
  - Traffic that is based on a source address or CIDR.

Some common uses for Calico `preDNAT` network policies:

  - Block traffic to public node ports of a private LoadBalancer service.
  - Block traffic to public node ports on clusters that are running [edge worker nodes](#cs_edge). Blocking node ports ensures that the edge worker nodes are the only worker nodes that handle incoming traffic.

The `preDNAT` network policies are useful because default Kubernetes and Calico policies are difficult to apply to protecting Kubernetes NodePort and LoadBalancer services due to the DNAT iptables rules generated for these services.

Calico `preDNAT` network policies generate iptables rules based on a [Calico
network policy resource ![External link icon](../icons/launch-glyph.svg "External link icon")](https://docs.projectcalico.org/v2.4/reference/calicoctl/resources/policy).

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

<br />



## Restricting network traffic to edge worker nodes
{: #cs_edge}

Add the `dedicated=edge` label to two or more worker nodes in your cluster to ensure that Ingress and load balancers are deployed to those worker nodes only.

Edge worker nodes can improve the security of your cluster by allowing fewer worker nodes to be accessed externally and by isolating the networking workload. When these worker nodes are marked for networking only, other workloads cannot consume the CPU or memory of the worker node and interfere with networking.

Before you begin:

- [Create a standard cluster.](cs_cluster.html#cs_cluster_cli)
- Ensure that your cluster has a least one public VLAN. Edge worker nodes are not available for clusters with private VLANs only.
- [Target the Kubernetes CLI to the cluster](cs_cli_install.html#cs_cli_configure).


1. List all of the worker nodes in the cluster. Use the private IP address from the **NAME** column to identify the nodes. Select at least two worker nodes to be edge worker nodes. Using two or more worker nodes improves availability of the networking resources.

  ```
  kubectl get nodes -L publicVLAN,privateVLAN,dedicated
  ```
  {: pre}

2. Label the worker nodes with `dedicated=edge`. After a worker node is marked with `dedicated=edge`, all subsequent Ingress and load balancers are deployed to an edge worker node.

  ```
  kubectl label nodes <node_name> <node_name2> dedicated=edge
  ```
  {: pre}

3. Retrieve all existing load balancer services in your cluster.

  ```
  kubectl get services --all-namespaces -o jsonpath='{range .items[*]}kubectl get service -n {.metadata.namespace} {.metadata.name} -o yaml | kubectl apply -f - :{.spec.type},{end}' | tr "," "\n" | grep "LoadBalancer" | cut -d':' -f1
  ```
  {: pre}

  Output:

  ```
  kubectl get service -n <namespace> <name> -o yaml | kubectl apply -f
  ```
  {: screen}

4. Using the output from the previous step, copy and paste each `kubectl get service` line. This command redeploys the load balancer to an edge worker node. Only public load balancers need to be redeployed.

  Output:

  ```
  service "<name>" configured
  ```
  {: screen}

You labeled worker nodes with `dedicated=edge` and redeployed all existing load balancers and Ingress to the edge worker nodes. Next, prevent other [workloads from running on edge worker nodes](#cs_edge_workloads) and [block inbound traffic to node ports on worker nodes](#cs_block_ingress).

### Prevent workloads from running on edge worker nodes
{: #cs_edge_workloads}

One of the benefits of edge worker nodes is that these worker nodes can be specified to run networking services only. Using the `dedicated=edge` toleration means that all load balancer and Ingress services are deployed to the labeled worker nodes only. However, to prevent other workloads from running on edge worker nodes and consuming worker node resources, you must use [Kubernetes taints ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/).

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
