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

# Opening required ports and IP addresses in your firewall
{: #firewall}

Review these situations in which you might need to open specific ports and IP addresses in your firewalls for {{site.data.keyword.containerlong}}.
{:shortdesc}

* [To run `bx` commands](#firewall_bx) from your local system when corporate network policies prevent access to public internet endpoints via proxies or firewalls.
* [To run `kubectl` commands](#firewall_kubectl) from your local system when corporate network policies prevent access to public internet endpoints via proxies or firewalls.
* [To run `calicoctl` commands](#firewall_calicoctl) from your local system when corporate network policies prevent access to public internet endpoints via proxies or firewalls.
* [To allow communication between the Kubernetes master and the worker nodes](#firewall_outbound) when either a firewall is set up for the worker nodes or the firewall settings are customized in your {[softlayer]} account.
* [To access the NodePort service, LoadBalancer service, or Ingress from outside of the cluster](#firewall_inbound).

{[white-space.md]}

## Running `{[bxcs]}` commands from behind a firewall
{: #firewall_bx}

If corporate network policies prevent access from your local system to public endpoints via proxies or firewalls, to run `{[bxcs]}` commands, you must allow TCP access for {{site.data.keyword.containerlong_notm}}.
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

{[white-space.md]}

## Running `kubectl` commands from behind a firewall
{: #firewall_kubectl}

If corporate network policies prevent access from your local system to public endpoints via proxies or firewalls, to run `kubectl` commands, you must allow TCP access for the cluster.
{:shortdesc}

When a cluster is created, the port in the master URL is randomly assigned from within 20000-32767. You can either choose to open port range 20000-32767 for any cluster that might get created or you can choose to allow access for a specific existing cluster.

Before you begin, allow access to [run `{[bxcs]}` commands](#firewall_bx).

To allow access for a specific cluster:

1. Log in to the {{site.data.keyword.Bluemix_notm}} CLI. Enter your {{site.data.keyword.Bluemix_notm}} credentials when prompted. If you have a federated account, include the `--sso` option.

    ```
    {[bx]} login [--sso]
    ```
    {: pre}

2. Select the region that your cluster is in.

   ```
   {[bxcs]} region-set
   ```
   {: pre}

3. Get the name of your cluster.

   ```
   {[bxcs]} clusters
   ```
   {: pre}

4. Retrieve the **Master URL** for your cluster.

   ```
   {[bxcs]} cluster-get <cluster_name_or_ID>
   ```
   {: pre}

   Example output:
   ```
   ...
   Master URL:		https://{[public_IP]}:31142
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
   curl --insecure https://{[public_IP]}:31142/version
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

{[white-space.md]}

## Running `calicoctl` commands from behind a firewall
{: #firewall_calicoctl}

If corporate network policies prevent access from your local system to public endpoints via proxies or firewalls, to run `calicoctl` commands, you must allow TCP access for the Calico commands.
{:shortdesc}

Before you begin, allow access to run [`{[bx]}` commands](#firewall_bx) and [`kubectl` commands](#firewall_kubectl).

1. Retrieve the IP address from the master URL that you used to allow the [`kubectl` commands](#firewall_kubectl).

2. Get the port for ETCD.

  ```
  kubectl get cm -n kube-system calico-config -o yaml | grep etcd_endpoints
  ```
  {: pre}

3. Allow access for the Calico policies via the master URL IP address and the ETCD port.

{[white-space.md]}

## Allowing the cluster to access infrastructure resources and other services
{: #firewall_outbound}

Let your cluster access infrastructure resources and services from behind a firewall, such as for {{site.data.keyword.containershort_notm}} regions, {{site.data.keyword.registrylong_notm}}, {{site.data.keyword.monitoringlong_notm}}, {{site.data.keyword.loganalysislong_notm}}, {[softlayer]} private IPs, and egress for persistent volume claims.
{:shortdesc}

  1.  Note the public IP address for all your worker nodes in the cluster.

      ```
      {[bxcs]} workers <cluster_name_or_ID>
      ```
      {: pre}

  2.  Allow outgoing network traffic from the source _<each_worker_node_publicIP>_ to the destination TCP/UDP port range 20000-32767 and port 443, and the following IP addresses and network groups. If you have a corporate firewall that prevents your local machine from accessing public internet endpoints, do this step for both your source worker nodes and your local machine.
      - **Important**: You must allow outgoing traffic to port 443 for all of the locations within the region, to balance the load during the bootstrapping process. For example, if your cluster is in US South, you must allow traffic from port 443 to the IP addresses for all of the locations (dal10, dal12, and dal13).
      <p>{[firewall-ips-outbound.md]}</p>

  3.  Allow outgoing network traffic from the worker nodes to [{{site.data.keyword.registrylong_notm}} regions](/docs/services/Registry/registry_overview.html#registry_regions):
      - `TCP port 443 FROM <each_worker_node_publicIP> TO <registry_publicIP>`
      - Replace <em>&lt;registry_publicIP&gt;</em> with the registry IP addresses to which you want to allow traffic. The global registry stores IBM-provided public images, and regional registries store your own private or public images.
        <p>{[firewall-ips-registry.md]}</p>

  4.  Optional: Allow outgoing network traffic from the worker nodes to {{site.data.keyword.monitoringlong_notm}} and {{site.data.keyword.loganalysislong_notm}} services:
      - `TCP port 443, port 9095 FROM <each_worker_node_public_IP> TO <monitoring_public_IP>`
      - Replace <em>&lt;monitoring_public_IP&gt;</em> with all of the addresses for the monitoring regions to which you want to allow traffic:
        <p>{[firewall-ips-monitoring.md]}</p>
      - `TCP port 443, port 9091 FROM <each_worker_node_public_IP> TO <logging_public_IP>`
      - Replace <em>&lt;logging_public_IP&gt;</em> with all of the addresses for the logging regions to which you want to allow traffic:
        <p>{[firewall-ips-logging.md]}</p>

  5. For private firewalls, allow the appropriate {[softlayer]} private IP ranges. Consult [this link](https://knowledgelayer.softlayer.com/faq/what-ip-ranges-do-i-allow-through-firewall) beginning with the **Backend (private) Network** section.
      - Add all of the [locations within the regions](cs_regions.html#locations) that you are using.
      - Note that you must add the dal01 location (data center).
      - Open ports 80 and 443 to allow the cluster bootstrapping process.

  6. {: #pvc}To create persistent volume claims for data storage, allow egress access through your firewall for the [{[softlayer]} IP addresses](https://knowledgelayer.softlayer.com/faq/what-ip-ranges-do-i-allow-through-firewall) of the location (data center) that your cluster is in.
      - To find the location (data center) of your cluster, run `{[bxcs]} clusters`.
      - Allow access to the IP range for both the **Frontend (public) network** and **Backend (private) Network**.
      - Note that you must add the dal01 location (data center) for the **Backend (private) Network**.

{[white-space.md]}

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
