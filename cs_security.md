---

copyright:
  years: 2014, 2017
lastupdated: "2017-09-08"

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

<a href="https://console.bluemix.net/docs/api/content/containers/images/cs_security.png" ><img src="images/cs_security.png" width="400" alt="{{site.data.keyword.containershort_notm}} cluster security" style="width:400px; border-style: none"/></a>


  <table summary="The first row in the table spans both columns. The rest of the rows should be read left to right, with the server location in column one and IP addresses to match in column two.">
  <thead>
  <th colspan=2><img src="images/idea.png"/> Built-in cluster security settings in {{site.data.keyword.containershort_notm}}</th>
  </thead>
  <tbody>
    <tr>
      <td>Kubernetes master</td>
      <td>The Kubernetes master in each cluster is managed by IBM and is highly available. It includes {{site.data.keyword.containershort_notm}} security settings that ensure security compliance and secure communication to and from the worker nodes. Updates are performed by IBM as required. The dedicated Kubernetes master centrally controls and monitors all Kubernetes resources in the cluster. Based on the deployment requirements and capacity in the cluster, the Kubernetes master automatically schedules your containerized apps to deploy across available worker nodes. For more information, see [Kubernetes master security](#cs_security_master).</td>
    </tr>
    <tr>
      <td>Worker node</td>
      <td>Containers are deployed on worker nodes that are dedicated to a cluster and that ensure compute, network, and storage isolation for IBM customers. {{site.data.keyword.containershort_notm}} provides built-in security features to keep your worker nodes secure on the private and public network and to ensure worker node security compliance. For more information, see [Worker node security](#cs_security_worker).</td>
     </tr>
     <tr>
      <td>Images</td>
      <td>As the cluster admin, you can set up your own secure Docker image repository in {{site.data.keyword.registryshort_notm}} where you can store and share Docker images between your cluster users. To ensure safe container deployments, every image in your private registry is scanned by Vulnerability Advisor. Vulnerability Advisor is a component of {{site.data.keyword.registryshort_notm}} that scans for potential vulnerabilities, makes security recommendations, and provides instructions to resolve vulnerabilities. For more information, see [Image security in {{site.data.keyword.containershort_notm}}](#cs_security_deployment).</td>
    </tr>
  </tbody>
</table>

## Kubernetes master
{: #cs_security_master}

Review the built-in Kubernetes master security features to protect the Kubernetes master and to secure the cluster network communication.
{: shortdesc}

<dl>
  <dt>Fully managed and dedicated Kubernetes master</dt>
    <dd>Every Kubernetes cluster in {{site.data.keyword.containershort_notm}} is controlled by a dedicated Kubernetes master that is managed by IBM in an IBM-owned {{site.data.keyword.BluSoftlayer_full}} account. The Kubernetes master is set up with the following dedicated components that are not shared with other IBM customers.
    <ul><li>etcd data store: Stores all Kubernetes resources of a cluster, such as Services, Deployments, and Pods. Kubernetes ConfigMaps and Secrets are app data that are stored as key value pairs so that they can be used by an app that runs in a pod. Data in etcd is stored on an encrypted disk that is managed by IBM and is encrypted via TLS when sent to a pod to assure data protection and integrity.</li>
    <li>kube-apiserver: Serves as the main entry point for all requests from the worker node to the Kubernetes master. The kube-apiserver validates and processes requests and can read from and write to the etcd data store.</li>
    <li>kube-scheduler: Decides where to deploy pods, taking into account capacity and performance needs, hardware and software policy constraints, anti-affinity specifications, and workload requirements. If no worker node can be found that matches the requirements, the pod is not deployed in the cluster.</li>
    <li>kube-controller-manager: Responsible for monitoring replica sets, and creating corresponding pods to achieve the desired state.</li>
    <li>OpenVPN: {{site.data.keyword.containershort_notm}} specific component to provide secured network connectivity for all Kubernetes master to worker node communication.</li></ul></dd>
  <dt>TLS secured network connectivity for all worker node to Kubernetes master communication</dt>
    <dd>To secure the network communication to the Kubernetes master, {{site.data.keyword.containershort_notm}} generates TLS certificates that encrypts the communication to and from the kube-apiserver and etcd data store components for every cluster. These certificates are never shared across clusters or across Kubernetes master components.</dd>
  <dt>OpenVPN secured network connectivity for all Kubernetes master to worker node communication</dt>
    <dd>Although Kubernetes secures the communication between the Kubernetes master and worker nodes by using the `https` protocol, no authentication is provided on the worker node by default. To secure this communication, {{site.data.keyword.containershort_notm}} automatically sets up an OpenVPN connection between the Kubernetes master and the worker node when the cluster is created.</dd>
  <dt>Continuous Kubernetes master network monitoring</dt>
    <dd>Every Kubernetes master is continuously monitored by IBM to control and remediate process level Denial-Of-Service (DOS) attacks.</dd>
  <dt>Kubernetes master node security compliance</dt>
    <dd>{{site.data.keyword.containershort_notm}} automatically scans every node where the Kubernetes master is deployed for vulnerabilities found in Kubernetes and OS-sepcific security fixes that need to be applied to assure master node protection. If vulnerabilities are found, {{site.data.keyword.containershort_notm}} automatically applies fixes and resolves vulnerabilities on behalf of the user.</dd>
</dl>

## Worker nodes
{: #cs_security_worker}

Review the built-in worker node security features to protect the worker node environment and to assure resource, network and storage isolation.
{: shortdesc}

<dl>
  <dt>Compute, network and storage infrastructure isolation</dt>
    <dd>When you create a cluster, virtual machines are provisioned as worker nodes in the customer {{site.data.keyword.BluSoftlayer_notm}} account or in the dedicated {{site.data.keyword.BluSoftlayer_notm}} account by IBM. Worker nodes are dedicated to a cluster and do not host workloads of other clusters.</br> Every {{site.data.keyword.Bluemix_notm}} account is set up with {{site.data.keyword.BluSoftlayer_notm}} VLANs to assure quality network performance and isolation on the worker nodes. </br>To persist data in your cluster, you can provision dedicated NFS based file storage from {{site.data.keyword.BluSoftlayer_notm}} and leverage the built-in data security features of that platform.</dd>
  <dt>Secured worker node set up</dt>
    <dd>Every worker node is set up with an Ubuntu operating system that cannot be changed by the user. To protect the operating system of the worker nodes from potential attacks, every worker node is configured with expert firewall settings that are enforced by Linux iptable rules.</br> All containers that run on Kubernetes are protected by predefined Calico network policy settings that are configured on every worker node during cluster creation. This set up assures secure network communication between worker nodes and pods. To further restrict the actions that a container can perform on the worker node, users can choose to configure [AppArmor policies ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/tutorials/clusters/apparmor/) on the worker nodes.</br> By default, SSH access for the root user is disabled on the worker node. If you want to install additional features on your worker node, you can use [Kubernetes daemon sets ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset) for everything that you want to run on every worker node, or [Kubernetes jobs ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/concepts/workloads/controllers/jobs-run-to-completion/) for any one-time action you must execute.</dd>
  <dt>Kubernetes worker node security compliance</dt>
    <dd>IBM works with security advisory teams internally and externally to identify potential security compliance issues for worker nodes and continuously releases compliance updates and security patches to address any vulnerabilities that are found. Updates and security patches are automatically deployed to the operating system of the worker node by IBM. To do that, IBM maintains SSH access to the worker nodes.</br> <b>Note</b>: Some updates require a worker node reboot. However, IBM does not reboot worker nodes during the installation of updates or security patches. Users are advised to reboot worker nodes on a regular basis to ensure that the installation of updates and security patches can finish.</dd>
  <dt>Support for SoftLayer network firewalls</dt>
    <dd>{{site.data.keyword.containershort_notm}} is compatible with all [{{site.data.keyword.BluSoftlayer_notm}} firewall offerings ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/cloud-computing/bluemix/network-security). On {{site.data.keyword.Bluemix_notm}} Public, you can set up a firewall with custom network policies to provide dedicated network security for your cluster and to detect and remediate network intrusion. For example, you might choose to set up a [Vyatta ![External link icon](../icons/launch-glyph.svg "External link icon")](https://knowledgelayer.softlayer.com/topic/vyatta-1) to act as your firewall and block unwanted traffic. When you set up a firewall, [you must also open up the required ports and IP addresses](#opening_ports) for each region so that the master and the worker nodes can communicate. On {{site.data.keyword.Bluemix_notm}} Dedicated, firewalls, DataPower, Fortigate, and DNS are already configured as part of the standard dedicated environment deployment.</dd>
  <dt>Keep services private or selectively expose services and apps to the public internet</dt>
    <dd>You can choose to keep your services and apps private and leverage the built-in security features described in this topic to assure secured communication between worker nodes and pods. To expose services and apps to the public internet, you can leverage the Ingress and load balancer support to securely make your services publicly available.</dd>
  <dt>Securely connect your worker nodes and apps to an on-premise data center</dt>
    <dd>You can set up a Vyatta Gateway Appliance or Fortigate Appliance to configure an IPSec VPN endpoint that connects your Kubernetes cluster with an on-premise data center. Over an encrypted tunnel, all services that run in your Kubernetes cluster can communicate securely with on-premise apps, such as user directories, databases, or mainframes. For more information, see [Connecting a cluster to an on-premise data center ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/blogs/bluemix/2017/07/kubernetes-and-bluemix-container-based-workloads-part4/).</dd>
  <dt>Continuous monitoring and logging of cluster activity</dt>
    <dd>For standard clusters, all cluster-related events, such as adding a worker node, rolling update progress, or capacity usage information are logged and monitored by {{site.data.keyword.containershort_notm}} and sent to the IBM Monitoring and Logging Service.</dd>
</dl>

### Opening required ports and IP addresses in your firewall
{: #opening_ports}

When you set up a firewall for your worker nodes or customize the firewall settings in your {{site.data.keyword.BluSoftlayer_notm}} account, you must open certain ports and IP addresses so that the worker node and the Kubernetes master can communicate. To access the load balancer or Ingress controller from outside of the cluster, you must also open ports in your firewall.

1.  Note the public IP address for all your worker nodes in the cluster.

    ```
    bx cs workers <cluster_name_or_id>
    ```
    {: pre}

2.  In your firewall, allow the following connections to and from your worker nodes.

  ```
  TCP port 443 FROM <each_worker_node_publicIP> TO registry.<region>.bluemix.net, apt.dockerproject.org
  ```
  {: codeblock}

    <ul>
    <li>For INBOUND connectivity to your worker nodes, allow incoming network traffic from the following source network groups and IP addresses to the destination TCP/UDP port 10250 and `<public_IP_of _each_worker_node>`:</br>
    
  <table summary="The first row in the table spans both columns. The rest of the rows should be read left to right, with the server location in column one and IP addresses to match in column two.">
      <thead>
      <th colspan=2><img src="images/idea.png"/> Inbound IP addresses</th>
      </thead>
    <tbody>
      <tr>
        <td>ams03</td>
        <td><code>169.50.144.128/28</code></br><code>169.50.169.104/29</code></br><code>169.50.185.32/27</code></td>
      </tr>
      <tr>
        <td>dal10</td>
        <td><code>169.46.7.232/29</code></br><code>169.48.138.64/26</code></br><code>169.48.180.128/25</code></td>
       </tr>
       <tr>
        <td>dal12</td>
        <td><code>169.47.70.8/29</code></br><code>169.47.79.192/26</code></br><code>169.47.126.192/27</code></td>
       </tr>
       <tr>
        <td>fra02</td>
        <td><code>169.50.48.160/28</code></br><code>169.50.56.168/29</code></br><code>169.50.58.160/27</code></td>
       </tr>
      </tbody>
      <tr>
       <td>lon02</td>
       <td><code>159.122.242.78</code></td>
      </tr>
      <tr>
       <td>lon04</td>
       <td><code>158.175.68.192/26</code></td>
      </tr>
      <tr>
       <td>syd01</td>
       <td><code>168.1.209.192/26</code></td>
      </tr>
      <tr>
       <td>syd04</td>
       <td><code>130.198.67.0/26</code></td>
      </tr>
    </table>

    </li>
    <li>For OUTBOUND connectivity from your worker nodes, allow outgoing network traffic from the source worker node to the destination TCP/UDP port range 20000-32767 for `<each_worker_node_publicIP>`, and the following IP addresses and network groups:</br>
    
  <table summary="The first row in the table spans both columns. The rest of the rows should be read left to right, with the server location in column one and IP addresses to match in column two.">
      <thead>
      <th colspan=2><img src="images/idea.png"/> Outbound IP addresses</th>
      </thead>
    <tbody>
      <tr>
        <td>ams03</td>
        <td><code>169.50.169.110</code></td>
      </tr>
      <tr>
        <td>dal10</td>
        <td><code>169.46.7.238</code></td>
       </tr>
       <tr>
        <td>dal12</td>
        <td><code>169.47.70.10</code></td>
       </tr>
       <tr>
        <td>fra02</td>
        <td><code>169.50.56.174</code></td>
       </tr>
      </tbody>
      <tr>
       <td>lon02</td>
       <td><code>159.122.242.78</code></td>
      </tr>
      <tr>
       <td>lon04</td>
       <td><code>158.175.65.170</code></td>
      </tr>
      <tr>
       <td>syd01</td>
       <td><code>168.1.8.195</code></td>
      </tr>
      <tr>
       <td>syd04</td>
       <td><code>130.198.64.19</code></td>
      </tr>
    </table>

    </li>
    </ul>

3. Optional: To access the load balancer from outside of the VLAN, open the port for incoming network traffic on the specific IP address of that load balancer.
 
4. Optional: To access the Ingress controller from outside of the VLAN, open either port 80 or 443 for incoming network traffic on the specific IP address of that Ingress controller, depending on which port you have configured.


## Network policies
{: #cs_security_network_policies}

Every Kubernetes cluster is set up with a network plug-in that is called Calico. Default network policies are set up to secure the public network interface of every worker node. You can use Calico and native Kubernetes capabilities to configure more network policies for a cluster when you have unique security requirements. These network policies specify the network traffic that you want to allow or block to and from a pod in a cluster.
{: shortdesc}

You can choose between Calico and native Kubernetes capabilities to create network policies for your cluster. You might use Kubernetes network policies to get started, but for more robust capabilities, use the Calico network policies.

<ul><li>[Kubernetes network policies ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/concepts/services-networking/network-policies/): Some basic options are provided, such as specifying which pods can communicate with each other. Incoming network traffic for pods can be allowed or blocked for a protocol and port based on the labels and Kubernetes namespaces of the pod that is trying to connect to them.</br>These policies can be applied by using `kubectl` commands or the Kubernetes APIs. When these policies are applied, they are converted into Calico network policies and Calico enforces these policies.
<li>[Calico network policies ![External link icon](../icons/launch-glyph.svg "External link icon")](http://docs.projectcalico.org/v2.0/getting-started/kubernetes/tutorials/advanced-policy): These policies are a superset of the Kubernetes network policies and enhance the native Kubernetes capabilities with the following features.
<ul><ul><li>Allow or block network traffic on specific network interfaces, not only Kubernetes pod traffic.<li>Allow or block incoming (ingress) and outgoing (egress) network traffic.<li>Allow or block traffic that is based on a source or destination IP address or CIDR.</ul></ul></br>
These policies are applied by using `calicoctl` commands. Calico enforces these policies, including any Kubernetes network policies that are converted to Calico policies, by setting up Linux iptables rules on the Kubernetes worker nodes. Iptables rules serve as a firewall for the worker node to define the characteristics that the network traffic must meet to be forwarded to the targeted resource.</ul>


### Default policy configuration
{: #concept_nq1_2rn_4z}

When a cluster is created, default network policies are automatically set up for the public network interface of each worker node to limit incoming traffic for a worker node from the public internet. These policies do not affect pod to pod traffic and are set up to allow access to the Kubernetes nodeport, load balancer, and Ingress services.

Default policies are not applied to pods directly; they are applied to the public network interface of a worker node by using a Calico [host endpoint ![External link icon](../icons/launch-glyph.svg "External link icon")](http://docs.projectcalico.org/v2.0/getting-started/bare-metal/bare-metal). When a host endpoint is created in Calico, all traffic to and from that worker node's network interface is blocked, unless that traffic is allowed by a policy.

Note that a policy to allow SSH does not exist, so SSH access by way of the public network interface is blocked, as are all other ports that do not have a policy to open them. SSH access, and other access, is available on the private network interface of each worker node.

**Important:** Do not remove policies that are applied to a host endpoint unless you fully understand the policy and know that you do not need the traffic that is being allowed by the policy.


 <table summary="The first row in the table spans both columns. The rest of the rows should be read left to right, with the server location in column one and IP addresses to match in column two.">
  <thead>
  <th colspan=2><img src="images/idea.png"/> Default policies for each cluster</th>
  </thead>
  <tbody>
    <tr>
      <td><code>allow-all-outbound</code></td>
      <td>Allows all outbound traffic.</td>
    </tr>
    <tr>
      <td><code>allow-icmp</code></td>
      <td>Allows incoming icmp packets (pings).</td>
     </tr>
     <tr>
      <td><code>allow-kubelet-port</code></td>
      <td>Allows all incoming traffic to port 10250, which is the port that is used by the kubelet. This policy allows `kubectl logs` and `kubectl exec` to work properly in the Kubernetes cluster.</td>
    </tr>
    <tr>
      <td><code>allow-node-port-dnat</code></td>
      <td>Allows incoming nodeport, load balancer, and ingress service traffic to the pods that those services are exposing. Note the port that those services expose on the public interface does not have to be specified because Kubernetes uses destination network address translation (DNAT) to forward those service requests to the correct pods. That forwarding takes place before the host endpoint policies are applied in iptables.</td>
   </tr>
   <tr>
      <td><code>allow-sys-mgmt</code></td>
      <td>Allows incoming connections for specific {{site.data.keyword.BluSoftlayer_notm}} systems that are used to manage the worker nodes.</td>
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

Before you begin, complete the following steps.

1.  [Install the {{site.data.keyword.containershort_notm}} and Kubernetes CLIs.](cs_cli_install.html#cs_cli_install)
2.  [Create a lite or standard cluster.](cs_cluster.html#cs_cluster_ui)
3.  [Target the Kubernetes CLI to the cluster](cs_cli_install.html#cs_cli_configure). Include the `--admin` option with the `bx cs cluster-config` command, which is used to download the certificates and permission files. This download also includes the keys for the Administrator rbac role, which you need to run Calico commands.

  ```
  bx cs cluster-config <cluster_name> 
  ```
  {: pre}


To add network policies:
1.  Install the Calico CLI.
    1.  [Download the Calico CLI ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/projectcalico/calicoctl/releases/).

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
              mv /<path_to_file>/calico-darwin-amd64 /usr/local/bin/calicoctl
              ```
              {: pre}

        2.  Convert the binary file to an executable.

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

    1.  For Linux and OS X, create the '/etc/calico' directory. For Windows, any directory can be used.

      ```
      mkdir -p /etc/calico/
      ```
      {: pre}

    2.  Create a 'calicoctl.cfg' file.
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
      {: pre}

        1.  Retrieve the `<ETCD_URL>`. 

          -   Linux and OS X:
              
              ```
              kubectl describe pod -n kube-system `kubectl get pod -n kube-system | grep calico-policy-controller | awk '{print $1}'` | grep ETCD_ENDPOINTS | awk '{print $2}'
              ```
              {: pre}
              
              

          -   Output example:

              ```
              https://169.1.1.1:30001
              ```
              {: screen}

          -   Windows:
            
            <ol>
            <li>Get a list of the pods in the kube-system namespace and locate the Calico controller pod. </br><pre class="codeblock"><code>kubectl get pod -n kube-system</code></pre></br>Example:</br><pre class="screen"><code>calico-policy-controller-1674857634-k2ckm</code></pre>
            <li>View the details of the Calico controller pod.</br> <pre class="codeblock"><code>kubectl describe pod -n kube-system calico-policy-controller-&lt;ID&gt;</code></pre>
            <li>Locate the ETCD endpoints value. Example: <code>https://169.1.1.1:30001</code>
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
              ls `dirname $KUBECONFIG` | grep ca-.*pem
              ```
              {: pre}

            -   Windows:
              <ol><li>Open the directory you retrieved in the last step.</br><pre class="codeblock"><code>C:\Users\<user>\.bluemix\plugins\container-service\&#60;cluster_name&#62;-admin\</code></pre>
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


## Images
{: #cs_security_deployment}

Manage the security and integrity of your images with built-in security features.
{: shortdesc}

### Secured Docker private image repository in {{site.data.keyword.registryshort_notm}}:

 You can set up your own Docker image repository in a multi-tenant, highly available, and scalable private image registry that is hosted and managed by IBM to build, securely store, and share Docker images across cluster users.

### Image security compliance:

When you use {{site.data.keyword.registryshort_notm}}, you can leverage the built-in security scanning that is provided by Vulnerability Advisor. Every image that is pushed to your namespace is automatically scanned for vulnerabilities against a database of known CentOS, Debian, Red Hat, and Ubuntu issues. If vulnerabilities are found, Vulnerability Advisor provides instructions for how to resolve them to assure image integrity and security.

To view the vulnerability assessment for your image:

1.  From the **catalog**, in the Containers section, select **Container Registry**.
2.  On the **Private Repositories** page, in the **Repositories** table, identify the image.
3.  In the **Security Report** column, click the status of the image to retrieve its vulnerability assessment.
