---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-12"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}

# Opening required ports and IP addresses in your firewall
{: #firewall}

Review these situations in which you might need to open specific ports and IP addresses in your firewalls:
{:shortdesc}

* [To run `bx` commands](#firewall_bx) from your local system when corporate network policies prevent access to public internet endpoints via proxies or firewalls.
* [To run `kubectl` commands](#firewall_kubectl) from your local system when corporate network policies prevent access to public internet endpoints via proxies or firewalls.
* [To run `calicoctl` commands](#firewall_calicoctl) from your local system when corporate network policies prevent access to public internet endpoints via proxies or firewalls.
* [To allow communication between the Kubernetes master and the worker nodes](#firewall_outbound) when either a firewall is set up for the worker nodes or the firewall settings are customized in your IBM Cloud infrastructure (SoftLayer) account.
* [To access the NodePort service, LoadBalancer service, or Ingress from outside of the cluster](#firewall_inbound).

<br />


## Running `bx cs` commands from behind a firewall
{: #firewall_bx}

If corporate network policies prevent access from your local system to public endpoints via proxies or firewalls, to run `bx cs` commands, you must allow TCP access for {{site.data.keyword.containerlong_notm}}.
{:shortdesc}

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

<br />


## Running `kubectl` commands from behind a firewall
{: #firewall_kubectl}

If corporate network policies prevent access from your local system to public endpoints via proxies or firewalls, to run `kubectl` commands, you must allow TCP access for the cluster.
{:shortdesc}

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

<br />


## Running `calicoctl` commands from behind a firewall
{: #firewall_calicoctl}

If corporate network policies prevent access from your local system to public endpoints via proxies or firewalls, to run `calicoctl` commands, you must allow TCP access for the Calico commands.
{:shortdesc}

Before you begin, allow access to run [`bx` commands](#firewall_bx) and [`kubectl` commands](#firewall_kubectl).

1. Retrieve the IP address from the master URL that you used to allow the [`kubectl` commands](#firewall_kubectl).

2. Get the port for ETCD.

  ```
  kubectl get cm -n kube-system calico-config -o yaml | grep etcd_endpoints
  ```
  {: pre}

3. Allow access for the Calico policies via the master URL IP address and the ETCD port.

<br />


## Allowing the cluster to access infrastructure resources and other services
{: #firewall_outbound}

Let your cluster access infrastructure resources and services from behind a firewall, such as for {{site.data.keyword.containershort_notm}} regions, {{site.data.keyword.registrylong_notm}}, {{site.data.keyword.monitoringlong_notm}}, {{site.data.keyword.loganalysislong_notm}}, IBM Cloud infrastructure (SoftLayer) private IPs, and egress for persistent volume claims.
{:shortdesc}

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
         <td><code>169.50.169.106, 169.50.154.194</code><br><code>169.50.56.170</code><br><code>159.122.190.98</code><br><code>159.8.86.149, 159.8.98.170</code></td>
        </tr>
      <tr>
        <td>UK South</td>
        <td>lon02<br>lon04</td>
        <td><code>159.122.242.78</code><br><code>158.175.65.170, 158.175.74.170, 158.175.76.2</code></td>
      </tr>
      <tr>
        <td>US East</td>
         <td>mon01<br>tor01<br>wdc06<br>wdc07</td>
         <td><code>169.54.126.219</code><br><code>169.53.167.50</code><br><code>169.60.73.142</code><br><code>169.61.83.62</code></td>
      </tr>
      <tr>
        <td>US South</td>
        <td>dal10<br>dal12<br>dal13<br>hou02</td>
        <td><code>169.47.234.18, 169.46.7.238</code><br><code>169.47.70.10</code><br><code>169.60.128.2</code><br><code>184.173.44.62</code></td>
      </tr>
      </tbody>
    </table>
</p>

  3.  Allow outgoing network traffic from the worker nodes to [{{site.data.keyword.registrylong_notm}} regions](/docs/services/Registry/registry_overview.html#registry_regions):
      - `TCP port 443 FROM <each_worker_node_publicIP> TO <registry_publicIP>`
      - Replace <em>&lt;registry_publicIP&gt;</em> with the registry IP addresses to which you want to allow traffic. The global registry stores IBM-provided public images, and regional registries store your own private or public images.
        <p>
<table summary="The first row in the table spans both columns. The rest of the rows should be read left to right, with the server location in column one and IP addresses to match in column two.">
      <thead>
        <th>{{site.data.keyword.containershort_notm}} region</th>
        <th>Registry address</th>
        <th>Registry IP address</th>
      </thead>
      <tbody>
        <tr>
          <td>Global registry across container regions</td>
          <td>registry.bluemix.net</td>
          <td><code>169.60.72.144/28</code><br><code>169.61.76.176/28</code></td>
        </tr>
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
           <td>UK South</td>
           <td>ingest.logging.eu-gb.bluemix.net</td>
           <td><code>169.50.115.113</code></td>
          </tr>
          <tr>
           <td>EU Central</td>
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

  6. {: #pvc}To create persistent volume claims for data storage, allow egress access through your firewall for the [IBM Cloud infrastructure (SoftLayer) IP addresses](https://knowledgelayer.softlayer.com/faq/what-ip-ranges-do-i-allow-through-firewall) of the location (data center) that your cluster is in.
      - To find the location (data center) of your cluster, run `bx cs clusters`.
      - Allow access to the IP range for both the **Frontend (public) network** and **Backend (private) Network**.
      - Note that you must add the dal01 location (data center) for the **Backend (private) Network**.

<br />


## Accessing NodePort, load balancer, and Ingress services from outside the cluster
{: #firewall_inbound}

You can allow incoming access to NodePort, load balancer, and Ingress services.
{:shortdesc}

<dl>
  <dt>NodePort service</dt>
  <dd>Open the port that you configured when you deployed the service to the public IP addresses for all of the worker nodes to allow traffic to. To find the port, run `kubectl get svc`. The port is in the 20000-32000 range.<dd>
  <dt>LoadBalancer service</dt>
  <dd>Open the port that you configured when you deployed the service to the load balancer service's public IP address.</dd>
  <dt>Ingress</dt>
  <dd>Open port 80 for HTTP or port 443 for HTTPS to the IP address for the Ingress application load balancer.</dd>
</dl>
