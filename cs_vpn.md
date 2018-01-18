---

copyright:
  years: 2014, 2018
lastupdated: "2018-01-12"

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

With VPN connectivity, you can securely connect apps in a Kubernetes cluster to an on-premises network. You can also connect apps that are external to your cluster to an app that is running inside your cluster.
{:shortdesc}

## Setting up VPN connectivity with the Strongswan IPSec VPN service Helm Chart
{: #vpn-setup}

To set up VPN connectivity, you can use a Helm Chart to configure and deploy the [Strongswan IPSec VPN service ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.strongswan.org/) inside of a Kubernetes pod. All VPN traffic is then routed through this pod. For more information about the Helm commands that are used to set up the Strongswan chart, see the <a href="https://docs.helm.sh/helm/" target="_blank">Helm documentation <img src="../icons/launch-glyph.svg" alt="External link icon"></a>.
{:shortdesc}

Before you begin:

- [Create a standard cluster.](cs_clusters.html#clusters_cli)
- [If you are using an existing cluster, update it to version 1.7.4 or later.](cs_cluster_update.html#master)
- The cluster must have at least one available public Load Balancer IP address.
- [Target the Kubernetes CLI to the cluster](cs_cli_install.html#cs_cli_configure).

To set up VPN connectivity with Strongswan:

1. If it is not already enabled, install and initialize Helm for your cluster.

    1. Install the <a href="https://docs.helm.sh/using_helm/#installing-helm" target="_blank">Helm CLI <img src="../icons/launch-glyph.svg" alt="External link icon"></a>.

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
    <caption>Understanding the YAML file components</caption>
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
    <td>Change this value to the list of cluster subnet CIDRs to expose over the VPN connection to the on-premises network. This list can include the following subnets: <ul><li>The Kubernetes pod subnet CIDR: <code>172.30.0.0/16</code></li><li>The Kubernetes service subnet CIDR: <code>172.21.0.0/16</code></li><li>If your applications are exposed by a NodePort service on the private network, the worker node's private subnet CIDR. To find this value, run <code>bx cs subnets | grep <xxx.yyy.zzz></code> where &lt;xxx.yyy.zzz&gt; is the first three octects of the worker node's private IP address.</li><li>If you have applications exposed by LoadBalancer services on the private network, the cluster's private or user-managed subnet CIDRs. To find these values, run <code>bx cs cluster-get <cluster name> --showResources</code>. In the <b>VLANS</b> section, look for CIDRs that have a <b>Public</b> value of <code>false</code>.</li></ul></td>
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
    <td>Change this value to the list of on-premises private subnet CIDRs that the Kubernetes clusters are allowed to access.</td>
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
            k8s-conn{2}: INSTALLED, TUNNEL, reqid 12, ESP in UDP SPIs: c78cb6b1_i c5d0d1c3_o
            k8s-conn{2}: 172.21.0.0/16 172.30.0.0/16 === 10.91.152.128/26
        ```
        {: screen}

      **Note**:
          - It is likely that the VPN status is not `ESTABLISHED` the first time you use this Helm Chart. You might need to check the on-premises VPN endpoint settings and return to step 3 to change the `config.yaml` file several times before the connection is successful.
          - If the VPN pod is in an `ERROR` state or continues to crash and restart, it might be due to parameter validation of the `ipsec.conf` settings in the chart's config map. Check for any validation errors in the Strongswan pod logs by running `kubectl logs -n kube-system $STRONGSWAN_POD`. If there are validation errors, run `helm delete --purge vpn`, return to step 3 to fix the incorrect values in the `config.yaml` file, and repeat steps 4 - 8. If your cluster has a high number of worker nodes, you can also use `helm upgrade` to more quickly apply your changes instead of running `helm delete` and `helm install`.

    4. Once the VPN has a status of `ESTABLISHED`, test the connection with `ping`. The following example sends a ping from the VPN pod in the Kubernetes cluster to the private IP address of the on-premises VPN gateway. Make sure that the correct `remote.subnet` and `local.subnet` are specified in the configuration file, and that the local subnet list includes the source IP address from which you are sending the ping.

        ```
        kubectl exec -n kube-system  $STRONGSWAN_POD -- ping -c 3  <on-prem_gateway_private_IP>
        ```
        {: pre}

### Disabling the Strongswan IPSec VPN service
{: vpn_disable}

1. Delete the Helm Chart.

    ```
    helm delete --purge vpn
    ```
    {: pre}
