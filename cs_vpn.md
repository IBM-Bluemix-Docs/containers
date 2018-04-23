---

copyright:
  years: 2014, 2018
lastupdated: "2018-4-23"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}


# Setting up VPN connectivity
{: #vpn}

With VPN connectivity, you can securely connect apps in a Kubernetes cluster on {{site.data.keyword.containerlong}} to an on-premises network. You can also connect apps that are external to your cluster to an app that is running inside your cluster.
{:shortdesc}

To connect your worker nodes and apps to an on-premises data center, you can configure a VPN IPSec endpoint with a strongSwan service or with a Vyatta Gateway Appliance or a Fortigate Appliance.

- **Vyatta Gateway Appliance or Fortigate Appliance**: If you have a larger cluster, want to access non-Kubernetes resources over the VPN, or want to access multiple clusters over a single VPN, you might choose to set up a Vyatta Gateway Appliance or [Fortigate Security Appliance![External link icon](../icons/launch-glyph.svg "External link icon")](/docs/infrastructure/fortigate-10g/getting-started.html#getting-started-with-fortigate-security-appliance-10gbps) to configure an IPSec VPN endpoint. To configure a Vyatta, see [Setting up VPN connectivity with Vyatta](#vyatta).

- **strongSwan IPSec VPN Service**: You can set up a [strongSwan IPSec VPN service ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.strongswan.org/) that securely connects your Kubernetes cluster with an on-premises network. The strongSwan IPSec VPN service provides a secure end-to-end communication channel over the internet that is based on the industry-standard Internet Protocol Security (IPsec) protocol suite. To set up a secure connection between your cluster and an on-premises network, [configure and deploy the strongSwan IPSec VPN service](#vpn-setup) directly in a pod in your cluster.

## Setting up VPN connectivity with a Vyatta Gateway Appliance
{: #vyatta}

The [Vyatta Gateway Appliance ![External link icon](../icons/launch-glyph.svg "External link icon")](http://knowledgelayer.softlayer.com/learning/network-gateway-devices-vyatta) is a bare metal server that runs a special distribution of Linux. You can use a Vyatta as VPN gateway to securely connect to an on-premises network.
{:shortdesc}

All public and private network traffic that enters or leaves the cluster VLANs is routed through the Vyatta. You can use the Vyatta as a VPN endpoint to create an encrypted IPSec tunnel between servers in IBM Cloud infrastructure (SoftLayer) and on-premises resources. For example, the following diagram shows how an app on a private-only worker node in {{site.data.keyword.containershort_notm}} can communicate with an on-premises server via a Vyatta VPN connection:

<img src="images/cs_vpn_vyatta.png" width="725" alt="Expose an app in {{site.data.keyword.containershort_notm}} by using a load balancer" style="width:725px; border-style: none"/>

1. An app in your cluster, `myapp2`, receives a request from an Ingress or LoadBalancer service and needs to securely connect to data in your on-premises network.

2. Because `myapp2` is on a worker node that is on a private VLAN only, the Vyatta acts as a secure connection between the worker nodes and the on-premises network. The Vyatta uses the destination IP address to determine which network packets to send to the on-premises network.

3. The request is encrypted and sent over the VPN tunnel to the on-premises data center.

4. The incoming request passes through the on-premises firewall and is delivered to the VPN tunnel endpoint (router) where it is decrypted.

5. The VPN tunnel endpoint (router) forwards the request to the on-premises server or mainframe depending on the destination IP address specified in step 2. The necessary data is sent back over the VPN connection to `myapp2` through the same process.

To set up a Vyatta Gateway Appliance:

1. [Order a Vyatta ![External link icon](../icons/launch-glyph.svg "External link icon")](https://knowledgelayer.softlayer.com/procedure/how-order-vyatta).

2. [Configure the private VLAN on the Vyatta ![External link icon](../icons/launch-glyph.svg "External link icon")](https://knowledgelayer.softlayer.com/procedure/basic-configuration-vyatta).

3. To enable a VPN connection using the Vyatta, [configure IPSec on the Vyatta ![External link icon](../icons/launch-glyph.svg "External link icon")](https://knowledgelayer.softlayer.com/procedure/how-configure-ipsec-vyatta).

For more information, see this blog post on [connecting a cluster to an on-premises data center ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/blogs/bluemix/2017/07/kubernetes-and-bluemix-container-based-workloads-part4/).

## Setting up VPN connectivity with the strongSwan IPSec VPN service Helm chart
{: #vpn-setup}

Use a Helm chart to configure and deploy the strongSwan IPSec VPN service inside of a Kubernetes pod.
{:shortdesc}

Because strongSwan is integrated within your cluster, you don't need an external gateway device. When VPN connectivity is established, routes are automatically configured on all of the worker nodes in the cluster. These routes allow two-way connectivity through the VPN tunnel between pods on any worker node and the remote system. For example, the following diagram shows how an app in {{site.data.keyword.containershort_notm}} can communicate with an on-premises server via a strongSwan VPN connection:

<img src="images/cs_vpn_strongswan.png" width="700" alt="Expose an app in {{site.data.keyword.containershort_notm}} by using a load balancer" style="width:700px; border-style: none"/>

1. An app in your cluster, `myapp`, receives a request from an Ingress or LoadBalancer service and needs to securely connect to data in your on-premises network.

2. The request to the on-premises data center is forwarded to the IPSec strongSwan VPN pod. The destination IP address is used to determine which network packets should be sent to the IPSec strongSwan VPN pod.

3. The request is encrypted and sent over the VPN tunnel to the on-premises data center.

4. The incoming request passes through the on-premises firewall and is delivered to the VPN tunnel endpoint (router) where it is decrypted.

5. The VPN tunnel endpoint (router) forwards the request to the on-premises server or mainframe depending on the destination IP address specified in step 2. The necessary data is sent back over the VPN connection to `myapp` through the same process.

### Configure the strongSwan Helm chart
{: #vpn_configure}

Before you begin:
* [Install an IPsec VPN gateway in your on-premises data center](/docs/infrastructure/iaas-vpn/set-up-ipsec-vpn.html#setting-up-an-ipsec-connection).
* Either [create a standard cluster](cs_clusters.html#clusters_cli) or [update an existing cluster to version 1.7.16 or later](cs_cluster_update.html#master).
* The cluster must have at least one available public Load Balancer IP address. [You can check to see your available public IP addresses](cs_subnets.html#manage) or [free up a used IP address](cs_subnets.html#free).
* [Target the Kubernetes CLI to the cluster](cs_cli_install.html#cs_cli_configure).

For more information about the Helm commands that are used to set up the strongSwan chart, see the <a href="https://docs.helm.sh/helm/" target="_blank">Helm documentation <img src="../icons/launch-glyph.svg" alt="External link icon"></a>.

To configure the Helm chart:

1. [Install Helm for your cluster and add the {{site.data.keyword.Bluemix_notm}} repository to your Helm instance](cs_integrations.html#helm).

2. Save the default configuration settings for the strongSwan Helm chart in a local YAML file.

    ```
    helm inspect values ibm/strongswan > config.yaml
    ```
    {: pre}

3. Open the `config.yaml` file and make the following changes to the default values according to the VPN configuration you want. You can find descriptions for more advanced settings in the configuration file comments.

    **Important**: If you do not need to change a property, comment that property out by placing a `#` in front of it.

    <table>
    <col width="22%">
    <col width="78%">
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the YAML file components</th>
    </thead>
    <tbody>
    <tr>
    <td><code>localSubnetNAT</code></td>
    <td>Network Address Translation (NAT) for subnets provides a workaround for subnet conflicts between the local and on-premises networks. You can use NAT to remap the cluster's private local IP subnets, the pod subnet (172.30.0.0/16), or the pod service subnet (172.21.0.0/16) to a different private subnet. The VPN tunnel sees remapped IP subnets instead of the original subnets. Remapping happens before the packets are sent over the VPN tunnel as well as after the packets arrive from the VPN tunnel. You can expose both remapped and non-remapped subnets at the same time over the VPN.<br><br>To enable NAT, you can either add an entire subnet or individual IP addresses. If you add an entire subnet (in the format <code>10.171.42.0/24=10.10.10.0/24</code>), remapping is 1-to-1: all of the IP addresses in the internal network subnet are mapped over to external network subnet and vice versa. If you add individual IP addresses (in the format <code>10.171.42.17/32=10.10.10.2/32, 10.171.42.29/32=10.10.10.3/32</code>), only those internal IP addresses are mapped to the specified external IP addresses.<br><br>If you use this option, the local subnet that is exposed over the VPN connection is the "outside" subnet that the "internal" subnet is being mapped to.
    </td>
    </tr>
    <tr>
    <td><code>loadBalancerIP</code></td>
    <td>If you want to specify a portable public IP address for the strongSwan VPN service for inbound VPN connections, add that IP address. Specifying an IP address is useful when you need a stable IP address, such as when you must designate which IP addresses are permitted through an on-premises firewall.<br><br>To view available portable public IP addresses assigned to this cluster, see [managing IP addresses and subnets](cs_subnets.html#manage). If you leave this setting blank, a free portable public IP address is used. If the VPN connection is initiated from the on-premises gateway (<code>ipsec.auto</code> is set to <code>add</code>), you can use this property to configure a persistent public IP address on the on-premises gateway for the cluster.</td>
    </tr>
    <tr>
    <td><code>connectUsingLoadBalancerIP</code></td>
    <td>Use the load balancer IP address that you added in <code>loadBalancerIP</code> to also establish the outbound VPN connection. If this option is enabled, all of the cluster worker nodes must be on the same public VLAN. Otherwise, you must use the <code>nodeSelector</code> setting to ensure that the VPN pod deploys to a worker node on the same public VLAN as the <code>loadBalancerIP</code>. This option is ignored if <code>ipsec.auto</code> is set to <code>add</code>.<p>Accepted values:</p><ul><li><code>"false"</code>: Do not connect the VPN using the load balancer IP. The public IP address of the worker node that the VPN pod is running on is used instead.</li><li><code>"true"</code>: Establish the VPN using the load balancer IP as the local source IP. If <code>loadBalancerIP</code> is not set, the external IP address assigned to the load balancer service is used.</li><li><code>"auto"</code>: When <code>ipsec.auto</code> is set to <code>start</code> and <code>loadBalancerIP</code> is set, establish the VPN using the load balancer IP as the local source IP.</li></ul></td>
    </tr>
    <tr>
    <td><code>nodeSelector</code></td>
    <td>To limit which nodes the strongSwan VPN pod deploys to, add the IP address of a specific worker node or a worker node label. For example, the value <code>kubernetes.io/hostname: 10.xxx.xx.xxx</code> restricts the VPN pod to running on that worker node only. The value <code>strongswan: vpn</code> restricts the VPN pod to running on any worker nodes with that label. You can use any worker node label, but it is recommended that you use: <code>strongswan: &lt;release_name&gt;</code> so that different worker nodes can be used with different deployments of this chart.<br><br>If the VPN connection is initiated by the cluster (<code>ipsec.auto</code> is set to <code>start</code>), you can use this property to limit the source IP addresses of the VPN connection that are exposed to the on-premises gateway. This value is optional.</td>
    </tr>
    <tr>
    <td><code>ipsec.keyexchange</code></td>
    <td>If your on-premises VPN tunnel endpoint does not support <code>ikev2</code> as a protocol for initializing the connection, change this value to <code>ikev1</code> or <code>ike</code>.</td>
    </tr>
    <tr>
    <td><code>ipsec.esp</code></td>
    <td>Add the list of ESP encryption and authentication algorithms that your on-premises VPN tunnel endpoint uses for the connection.<ul><li>If <code>ipsec.keyexchange</code> is set to <code>ikev1</code>, this setting must be specified.</li><li>If <code>ipsec.keyexchange</code> is set to <code>ikev2</code>, this setting is optional. If you leave this setting blank, the default strongSwan algorithms <code>aes128-sha1,3des-sha1</code> are used for the connection.</li></ul></td>
    </tr>
    <tr>
    <td><code>ipsec.ike</code></td>
    <td>Add the list of IKE/ISAKMP SA encryption and authentication algorithms that your on-premises VPN tunnel endpoint uses for the connection.<ul><li>If <code>ipsec.keyexchange</code> is set to <code>ikev1</code>, this setting must be specified.</li><li>If <code>ipsec.keyexchange</code> is set to <code>ikev2</code>, this setting is optional. If you leave this setting blank, the default strongSwan algorithms <code>aes128-sha1-modp2048,3des-sha1-modp1536</code> are used for the connection.</li></ul></td>
    </tr>
    <tr>
    <td><code>ipsec.auto</code></td>
    <td>If you want the cluster to initiate the VPN connection, change this value to <code>start</code>.</td>
    </tr>
    <tr>
    <td><code>local.subnet</code></td>
    <td>Change this value to the list of cluster subnet CIDRs to expose over the VPN connection to the on-premises network. This list can include the following subnets: <ul><li>The Kubernetes pod subnet CIDR: <code>172.30.0.0/16</code></li><li>The Kubernetes service subnet CIDR: <code>172.21.0.0/16</code></li><li>If your apps are exposed by a NodePort service on the private network, the worker node's private subnet CIDR. Retrieve the first three octets of your worker's private IP address by running <code>bx cs worker &lt;cluster_name&gt;</code>. For example, if it is <code>&lt;10.176.48.xx&gt;</code> then note <code>&lt;10.176.48&gt;</code>. Next, get the worker private subnet CIDR by running the following command, replacing <code>&lt;xxx.yyy.zz&gt;</code> with the octet that you previously retrieved: <code>bx cs subnets | grep &lt;xxx.yyy.zzz&gt;</code>.</li><li>If you have apps that are exposed by LoadBalancer services on the private network, the cluster's private or user-managed subnet CIDRs. To find these values, run <code>bx cs cluster-get &lt;cluster_name&gt; --showResources</code>. In the **VLANS** section, look for CIDRs that have a **Public** value of <code>false</code>.</li></ul>**Note**: If <code>ipsec.keyexchange</code> is set to <code>ikev1</code>, you can specify only one subnet.</td>
    </tr>
    <tr>
    <td><code>local.id</code></td>
    <td>Change this value to the string identifier for the local Kubernetes cluster side your VPN tunnel endpoint uses for the connection.</td>
    </tr>
    <tr>
    <td><code>remote.gateway</code></td>
    <td>Change this value to the public IP address for the on-premises VPN gateway. When <code>ipsec.auto</code> is set to <code>start</code>, this value is required.</td>
    </tr>
    <td><code>remote.subnet</code></td>
    <td>Change this value to the list of on-premises private subnet CIDRs that the Kubernetes clusters are allowed to access. **Note**: If <code>ipsec.keyexchange</code> is set to <code>ikev1</code>, you can specify only one subnet.</td>
    </tr>
    <tr>
    <td><code>remote.id</code></td>
    <td>Change this value to the string identifier for the remote on-premises side your VPN tunnel endpoint uses for the connection.</td>
    </tr>
    <tr>
    <td><code>remote.privateIPtoPing</code></td>
    <td>Add the private IP address in the remote subnet to be used by the Helm test validation programs for VPN ping connectivity tests. This value is optional.</td>
    </tr>
    <tr>
    <td><code>preshared.secret</code></td>
    <td>Change this value to the pre-shared secret that your on-premises VPN tunnel endpoint gateway uses for the connection. This value is stored in <code>ipsec.secrets</code>.</td>
    </tr>
    </tbody></table>

4. Save the updated `config.yaml` file.

5. Install the Helm chart to your cluster with the updated `config.yaml` file. The updated properties are stored in a configmap for your chart.

    **Note**: If you have multiple VPN deployments in a single cluster, you can avoid naming conflicts and differentiate between your deployments by choosing more descriptive release names than `vpn`. To avoid the truncation of the release name, limit the release name to 35 characters or less.

    ```
    helm install -f config.yaml --namespace=kube-system --name=vpn ibm/strongswan
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


### Test and verify the VPN connectivity
{: #vpn_test}

After you deploy your Helm chart, test the VPN connectivity.
{:shortdesc}

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
    k8s-conn[1]: ESTABLISHED 17 minutes ago, 172.30.xxx.xxx[ibm-cloud]...192.xxx.xxx.xxx[on-premises]
    k8s-conn{2}: INSTALLED, TUNNEL, reqid 12, ESP in UDP SPIs: c78cb6b1_i c5d0d1c3_o
    k8s-conn{2}: 172.21.0.0/16 172.30.0.0/16 === 10.91.152.xxx/26
    ```
    {: screen}

    **Note**:

    <ul>
    <li>When you try to establish VPN connectivity with the strongSwan Helm chart, it is likely that the VPN status is not `ESTABLISHED` the first time. You might need to check the on-premises VPN endpoint settings and change the configuration file several times before the connection is successful: <ol><li>Run `helm delete --purge <release_name>`</li><li>Fix the incorrect values in the configuration file.</li><li>Run `helm install -f config.yaml --namespace=kube-system --name=<release_name> ibm/strongswan`</li></ol>You can also run more checks in the next step.</li>
    <li>If the VPN pod is in an `ERROR` state or continues to crash and restart, it might be due to parameter validation of the `ipsec.conf` settings in the chart's configmap.<ol><li>Check for any validation errors in the strongSwan pod logs by running `kubectl logs -n kube-system $STRONGSWAN_POD`.</li><li>If validation errors exist, run `helm delete --purge <release_name>`<li>Fix the incorrect values in the configuration file.</li><li>Run `helm install -f config.yaml --namespace=kube-system --name=<release_name> ibm/strongswan`</li></ol>If your cluster has a high number of worker nodes, you can also use `helm upgrade` to more quickly apply your changes instead of running `helm delete` and `helm install`.</li>
    </ul>

4. You can further test the VPN connectivity by running the five Helm tests that are included in the strongSwan chart definition.

    ```
    helm test vpn
    ```
    {: pre}

    * If all tests pass, your strongSwan VPN connection is successfully set up.

    * If any test fails, continue to the next step.

5. View the output of a failed test by looking at the logs of the test pod.

    ```
    kubectl logs -n kube-system <test_program>
    ```
    {: pre}

    **Note**: Some of the tests have requirements that are optional settings in the VPN configuration. If some of the tests fail, the failures might be acceptable depending on whether you specified these optional settings. Refer to the following table for more information about each test and why it might fail.

    {: #vpn_tests_table}
    <table>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the Helm VPN connectivity tests</th>
    </thead>
    <tbody>
    <tr>
    <td><code>vpn-strongswan-check-config</code></td>
    <td>Validates the syntax of the <code>ipsec.conf</code> file that is generated from the <code>config.yaml</code> file. This test might fail due to incorrect values in the <code>config.yaml</code> file.</td>
    </tr>
    <tr>
    <td><code>vpn-strongswan-check-state</code></td>
    <td>Checks that the VPN connection has a status of <code>ESTABLISHED</code>. This test might fail for the following reasons:<ul><li>Differences between the values in the <code>config.yaml</code> file and the on-premises VPN endpoint settings.</li><li>If the cluster is in "listen" mode (<code>ipsec.auto</code> is set to <code>add</code>), the connection is not established on the on-premises side.</li></ul></td>
    </tr>
    <tr>
    <td><code>vpn-strongswan-ping-remote-gw</code></td>
    <td>Pings the <code>remote.gateway</code> public IP address that you configured in the <code>config.yaml</code> file. This test might fail for the following reasons:<ul><li>You did not specify an on-premises VPN gateway IP address. If <code>ipsec.auto</code> is set to <code>start</code>, the <code>remote.gateway</code> IP address is required.</li><li>The VPN connection does not have the <code>ESTABLISHED</code> status. See <code>vpn-strongswan-check-state</code> for more information.</li><li>The VPN connectivity is <code>ESTABLISHED</code>, but ICMP packets are being blocked by a firewall.</li></ul></td>
    </tr>
    <tr>
    <td><code>vpn-strongswan-ping-remote-ip-1</code></td>
    <td>Pings the <code>remote.privateIPtoPing</code> private IP address of the on-premises VPN gateway from the VPN pod in the cluster. This test might fail for the following reasons:<ul><li>You did not specify a <code>remote.privateIPtoPing</code> IP address. If you intentionally did not specify an IP address, this failure is acceptable.</li><li>You did not specify the cluster pod subnet CIDR, <code>172.30.0.0/16</code>, in the <code>local.subnet</code> list.</li></ul></td>
    </tr>
    <tr>
    <td><code>vpn-strongswan-ping-remote-ip-2</code></td>
    <td>Pings the <code>remote.privateIPtoPing</code> private IP address of the on-premises VPN gateway from the worker node in the cluster. This test might fail for the following reasons:<ul><li>You did not specify a <code>remote.privateIPtoPing</code> IP address. If you intentionally did not specify an IP address, this failure is acceptable.</li><li>You did not specify the cluster worker node private subnet CIDR in the <code>local.subnet</code> list.</li></ul></td>
    </tr>
    </tbody></table>

6. Delete the current Helm chart.

    ```
    helm delete --purge vpn
    ```
    {: pre}

7. Open the `config.yaml` file and fix the incorrect values.

8. Save the updated `config.yaml` file.

9. Install the Helm chart to your cluster with the updated `config.yaml` file. The updated properties are stored in a configmap for your chart.

    ```
    helm install -f config.yaml --namespace=kube-system --name=<release_name> ibm/strongswan
    ```
    {: pre}

10. Check the chart deployment status. When the chart is ready, the **STATUS** field near the top of the output has a value of `DEPLOYED`.

    ```
    helm status vpn
    ```
    {: pre}

11. Once the chart is deployed, verify that the updated settings in the `config.yaml` file were used.

    ```
    helm get values vpn
    ```
    {: pre}

12. Clean up the current test pods.

    ```
    kubectl get pods -a -n kube-system -l app=strongswan-test
    ```
    {: pre}

    ```
    kubectl delete pods -n kube-system -l app=strongswan-test
    ```
    {: pre}

13. Run the tests again.

    ```
    helm test vpn
    ```
    {: pre}

<br />


## Upgrading the strongSwan Helm chart
{: #vpn_upgrade}

Make sure your strongSwan Helm chart is up-to-date by upgrading it.
{:shortdesc}

To upgrade your strongSwan Helm chart to the latest version:

  ```
  helm upgrade -f config.yaml --namespace kube-system <release_name> ibm/strongswan
  ```
  {: pre}


### Upgrading from version 1.0.0
{: #vpn_upgrade_1.0.0}

Due to some of the settings that are used in the version 1.0.0 Helm chart, you cannot use `helm upgrade` to update from 1.0.0 to the latest version.
{:shortdesc}

To upgrade from version 1.0.0, you must delete the 1.0.0 chart and install the latest version:

1. Delete the 1.0.0 Helm chart.

    ```
    helm delete --purge <release_name>
    ```
    {: pre}

2. Save the default configuration settings for the latest version of the strongSwan Helm chart in a local YAML file.

    ```
    helm inspect values ibm/strongswan > config.yaml
    ```
    {: pre}

3. Update the configuration file and save the file with your changes.

4. Install the Helm chart to your cluster with the updated `config.yaml` file.

    ```
    helm install -f config.yaml --namespace=kube-system --name=<release_name> ibm/strongswan
    ```
    {: pre}

Additionally, certain `ipsec.conf` timeout settings that were hardcoded in 1.0.0 are exposed as configurable properties in later versions. The names and defaults of some of these configurable `ipsec.conf` timeout settings were also changed to be more consistent with strongSwan standards. If you are upgrading your Helm chart from 1.0.0 and want to retain the 1.0.0 version defaults for the timeout settings, add the new settings to your chart configuration file with the old default values.

  <table>
  <caption>ipsec.conf settings differences between version 1.0.0 and the latest version</caption>
  <thead>
  <th>1.0.0 setting name</th>
  <th>1.0.0 default</th>
  <th>Latest version setting name</th>
  <th>Latest version default</th>
  </thead>
  <tbody>
  <tr>
  <td><code>ikelifetime</code></td>
  <td>60m</td>
  <td><code>ikelifetime</code></td>
  <td>3h</td>
  </tr>
  <tr>
  <td><code>keylife</code></td>
  <td>20m</td>
  <td><code>lifetime</code></td>
  <td>1h</td>
  </tr>
  <tr>
  <td><code>rekeymargin</code></td>
  <td>3m</td>
  <td><code>margintime</code></td>
  <td>9m</td>
  </tr>
  </tbody></table>


## Disabling the strongSwan IPSec VPN service
{: vpn_disable}

You can disable the VPN connection by deleting the Helm chart.
{:shortdesc}

  ```
  helm delete --purge <release_name>
  ```
  {: pre}

