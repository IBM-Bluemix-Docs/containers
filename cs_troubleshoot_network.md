---

copyright:
  years: 2014, 2018
lastupdated: "2018-03-30"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}


# Troubleshooting cluster networking
{: #cs_troubleshoot_network}

As you use {{site.data.keyword.containerlong}}, consider these techniques for troubleshooting cluster networking. Before trying these techniques, you can take some general steps to [debug your cluster and check for common issues](cs_troubleshoot.html).
{: shortdesc}

## Cannot connect to an app via a load balancer service
{: #cs_loadbalancer_fails}

{: tsSymptoms}
You publicly exposed your app by creating a load balancer service in your cluster. When you tried to connect to your app via the public IP address of the load balancer, the connection failed or timed out.

{: tsCauses}
Your load balancer service might not be working properly for one of the following reasons:

-   The cluster is a free cluster or a standard cluster with only one worker node.
-   The cluster is not fully deployed yet.
-   The configuration script for your load balancer service includes errors.

{: tsResolve}
To troubleshoot your load balancer service:

1.  Check that you set up a standard cluster that is fully deployed and has at least two worker nodes to assure high availability for your load balancer service.

  ```
  bx cs workers <cluster_name_or_id>
  ```
  {: pre}

    In your CLI output, make sure that the **Status** of your worker nodes displays **Ready** and that the **Machine Type** shows a machine type other than **free**.

2.  Check the accuracy of the configuration file for your load balancer service.

  ```
  apiVersion: v1
  kind: Service
  metadata:
    name: myservice
  spec:
    type: LoadBalancer
    selector:
      <selectorkey>:<selectorvalue>
    ports:
     - protocol: TCP
       port: 8080
  ```
  {: pre}

    1.  Check that you defined **LoadBalancer** as the type for your service.
    2.  Make sure that you used the same **<selectorkey>** and **<selectorvalue>** that you used in the **label/metadata** section when you deployed your app.
    3.  Check that you used the **port** that your app listens on.

3.  Check your load balancer service and review the **Events** section to find potential errors.

  ```
  kubectl describe service <myservice>
  ```
  {: pre}

    Look for the following error messages:

    <ul><li><pre class="screen"><code>Clusters with one node must use services of type NodePort</code></pre></br>To use the load balancer service, you must have a standard cluster with at least two worker nodes.</li>
    <li><pre class="screen"><code>No cloud provider IPs are available to fulfill the load balancer service request. Add a portable subnet to the cluster and try again</code></pre></br>This error message indicates that no portable public IP addresses are left to be allocated to your load balancer service. Refer to <a href="cs_subnets.html#subnets">Adding subnets to clusters</a> to find information about how to request portable public IP addresses for your cluster. After portable public IP addresses are available to the cluster, the load balancer service is automatically created.</li>
    <li><pre class="screen"><code>Requested cloud provider IP <cloud-provider-ip> is not available. The following cloud provider IPs are available: <available-cloud-provider-ips></code></pre></br>You defined a portable public IP address for your load balancer service by using the **loadBalancerIP** section, but this portable public IP address is not available in your portable public subnet. Change your load balancer service configuration script and either choose one of the available portable public IP addresses, or remove the **loadBalancerIP** section from your script so that an available portable public IP address can be allocated automatically.</li>
    <li><pre class="screen"><code>No available nodes for load balancer services</code></pre>You do not have enough worker nodes to deploy a load balancer service. One reason might be that you deployed a standard cluster with more than one worker node, but the provisioning of the worker nodes failed.</li>
    <ol><li>List available worker nodes.</br><pre class="codeblock"><code>kubectl get nodes</code></pre></li>
    <li>If at least two available worker nodes are found, list the worker node details.</br><pre class="codeblock"><code>bx cs worker-get [&lt;cluster_name_or_id&gt;] &lt;worker_ID&gt;</code></pre></li>
    <li>Make sure that the public and private VLAN IDs for the worker nodes that were returned by the <code>kubectl get nodes</code> and the <code>bx cs [&lt;cluster_name_or_id&gt;] worker-get</code> commands match.</li></ol></li></ul>

4.  If you are using a custom domain to connect to your load balancer service, make sure that your custom domain is mapped to the public IP address of your load balancer service.
    1.  Find the public IP address of your load balancer service.

      ```
      kubectl describe service <myservice> | grep "LoadBalancer Ingress"
      ```
      {: pre}

    2.  Check that your custom domain is mapped to the portable public IP address of your load balancer service in the Pointer record (PTR).

<br />


## Cannot connect to an app via Ingress
{: #cs_ingress_fails}

{: tsSymptoms}
You publicly exposed your app by creating an Ingress resource for your app in your cluster. When you tried to connect to your app via the public IP address or subdomain of the Ingress application load balancer, the connection failed or timed out.

{: tsCauses}
Ingress might not be working properly for the following reasons:
<ul><ul>
<li>The cluster is not fully deployed yet.
<li>The cluster was set up as a free cluster or as a standard cluster with only one worker node.
<li>The Ingress configuration script includes errors.
</ul></ul>

{: tsResolve}
To troubleshoot your Ingress:

1.  Check that you set up a standard cluster that is fully deployed and has at least two worker nodes to assure high availability for your Ingress application load balancer.

  ```
  bx cs workers <cluster_name_or_id>
  ```
  {: pre}

    In your CLI output, make sure that the **Status** of your worker nodes displays **Ready** and that the **Machine Type** shows a machine type other than **free**.

2.  Retrieve the Ingress application load balancer subdomain and public IP address, and then ping each one.

    1.  Retrieve the application load balancer subdomain.

      ```
      bx cs cluster-get <cluster_name_or_id> | grep "Ingress subdomain"
      ```
      {: pre}

    2.  Ping the Ingress application load balancer subdomain.

      ```
      ping <ingress_controller_subdomain>
      ```
      {: pre}

    3.  Retrieve the public IP address of your Ingress application load balancer.

      ```
      nslookup <ingress_controller_subdomain>
      ```
      {: pre}

    4.  Ping the Ingress application load balancer public IP address.

      ```
      ping <ingress_controller_ip>
      ```
      {: pre}

    If the CLI returns a timeout for the public IP address or subdomain of the Ingress application load balancer, and you have set up a custom firewall that is protecting your worker nodes, you might need to open additional ports and networking groups in your [firewall](cs_troubleshoot_clusters.html#cs_firewall).

3.  If you are using a custom domain, make sure that your custom domain is mapped to the public IP address or subdomain of the IBM-provided Ingress application load balancer with your Domain Name Service (DNS) provider.
    1.  If you used the Ingress application load balancer subdomain, check your Canonical Name record (CNAME).
    2.  If you used the Ingress application load balancer public IP address, check that your custom domain is mapped to the portable public IP address in the Pointer record (PTR).
4.  Check your Ingress resource configuration file.

    ```
    apiVersion: extensions/v1beta1
    kind: Ingress
    metadata:
      name: myingress
    spec:
      tls:
      - hosts:
        - <ingress_subdomain>
        secretName: <ingress_tlssecret>
      rules:
      - host: <ingress_subdomain>
        http:
          paths:
          - path: /
            backend:
              serviceName: myservice
              servicePort: 80
    ```
    {: codeblock}

    1.  Check that the Ingress application load balancer subdomain and TLS certificate are correct. To find the IBM provided subdomain and TLS certificate, run `bx cs cluster-get <cluster_name_or_id>`.
    2.  Make sure that your app listens on the same path that is configured in the **path** section of your Ingress. If your app is set up to listen on the root path, include **/** as your path.
5.  Check your Ingress deployment and look for potential warning or error messages.

    ```
    kubectl describe ingress <myingress>
    ```
    {: pre}

    For example, in the **Events** section of the output, you might see warning messages about invalid values in your Ingress resource or in certain annotations you used.

    ```
    Name:             myingress
    Namespace:        default
    Address:          169.xx.xxx.xx,169.xx.xxx.xx
    Default backend:  default-http-backend:80 (<none>)
    Rules:
      Host                                             Path  Backends
      ----                                             ----  --------
      mycluster.us-south.containers.mybluemix.net
                                                       /tea      myservice1:80 (<none>)
                                                       /coffee   myservice2:80 (<none>)
    Annotations:
      custom-port:        protocol=http port=7490; protocol=https port=4431
      location-modifier:  modifier='~' serviceName=myservice1;modifier='^~' serviceName=myservice2
    Events:
      Type     Reason             Age   From                                                            Message
      ----     ------             ----  ----                                                            -------
      Normal   Success            1m    public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Successfully applied ingress resource.
      Warning  TLSSecretNotFound  1m    public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Failed to apply ingress resource.
      Normal   Success            59s   public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Successfully applied ingress resource.
      Warning  AnnotationError    40s   public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Failed to apply ingress.bluemix.net/custom-port annotation. Error annotation format error : One of the mandatory fields not valid/missing for annotation ingress.bluemix.net/custom-port
      Normal   Success            40s   public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Successfully applied ingress resource.
      Warning  AnnotationError    2s    public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Failed to apply ingress.bluemix.net/custom-port annotation. Invalid port 7490. Annotation cannot use ports 7481 - 7490
      Normal   Success            2s    public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Successfully applied ingress resource.
    ```
    {: screen}

6.  Check the logs for your application load balancer.
    1.  Retrieve the ID of the Ingress pods that are running in your cluster.

      ```
      kubectl get pods -n kube-system | grep alb1
      ```
      {: pre}

    2.  Retrieve the logs for each Ingress pod.

      ```
      kubectl logs <ingress_pod_id> nginx-ingress -n kube-system
      ```
      {: pre}

    3.  Look for error messages in the application load balancer logs.

<br />



## Ingress application load balancer secret issues
{: #cs_albsecret_fails}

{: tsSymptoms}
After deploying an Ingress application load balancer secret to your cluster, the `Description` field is not updating with the secret name when you view your certificate in {{site.data.keyword.cloudcerts_full_notm}}.

When you list information about the application load balancer secret, the status says `*_failed`. For example, `create_failed`, `update_failed`, `delete_failed`.

{: tsResolve}
Review the following reasons why the application load balancer secret might fail and the corresponding troubleshooting steps:

<table>
 <thead>
 <th>Why it's happening</th>
 <th>How to fix it</th>
 </thead>
 <tbody>
 <tr>
 <td>You do not have the required access roles to download and update certificate data.</td>
 <td>Check with your account Administrator to assign you both the **Operator** and **Editor** roles for your {{site.data.keyword.cloudcerts_full_notm}} instance. For more details, see <a href="/docs/services/certificate-manager/access-management.html#managing-service-access-roles">Managing service access</a> for {{site.data.keyword.cloudcerts_short}}.</td>
 </tr>
 <tr>
 <td>The certificate CRN provided at time of create, update, or remove does not belong to the same account as the cluster.</td>
 <td>Check that the certificate CRN you provided is imported to an instance of the {{site.data.keyword.cloudcerts_short}} service that is deployed in the same account as your cluster.</td>
 </tr>
 <tr>
 <td>The certificate CRN provided at time of create is incorrect.</td>
 <td><ol><li>Check the accuracy of the certificate CRN string you provide.</li><li>If the certificate CRN is found to be accurate, then try to update the secret: <code>bx cs alb-cert-deploy --update --cluster &lt;cluster_name_or_id&gt; --secret-name &lt;secret_name&gt; --cert-crn &lt;certificate_CRN&gt;</code></li><li>If this command results in the <code>update_failed</code> status, then remove the secret: <code>bx cs alb-cert-rm --cluster &lt;cluster_name_or_id&gt; --secret-name &lt;secret_name&gt;</code></li><li>Deploy the secret again: <code>bx cs alb-cert-deploy --cluster &lt;cluster_name_or_id&gt; --secret-name &lt;secret_name&gt; --cert-crn &lt;certificate_CRN&gt;</code></li></ol></td>
 </tr>
 <tr>
 <td>The certificate CRN provided at time of update is incorrect.</td>
 <td><ol><li>Check the accuracy of the certificate CRN string you provide.</li><li>If the certificate CRN is found to be accurate, then remove the secret: <code>bx cs alb-cert-rm --cluster &lt;cluster_name_or_id&gt; --secret-name &lt;secret_name&gt;</code></li><li>Deploy the secret again: <code>bx cs alb-cert-deploy --cluster &lt;cluster_name_or_id&gt; --secret-name &lt;secret_name&gt; --cert-crn &lt;certificate_CRN&gt;</code></li><li>Try to update the secret: <code>bx cs alb-cert-deploy --update --cluster &lt;cluster_name_or_id&gt; --secret-name &lt;secret_name&gt; --cert-crn &lt;certificate_CRN&gt;</code></li></ol></td>
 </tr>
 <tr>
 <td>The {{site.data.keyword.cloudcerts_long_notm}} service is experiencing downtime.</td>
 <td>Check that your {{site.data.keyword.cloudcerts_short}} service is up and running.</td>
 </tr>
 </tbody></table>

<br />


## Cannot establish VPN connectivity with the strongSwan Helm chart
{: #cs_vpn_fails}

{: tsSymptoms}
When you check VPN connectivity by running `kubectl exec -n kube-system  $STRONGSWAN_POD -- ipsec status`, you do not see a status of `ESTABLISHED`, or the VPN pod is in an `ERROR` state or continues to crash and restart.

{: tsCauses}
Your Helm chart configuration file has incorrect values, missing values, or syntax errors.

{: tsResolve}
When you try to establish VPN connectivity with the strongSwan Helm chart, it is likely that the VPN status will not be `ESTABLISHED` the first time. You might need to check for several types of issues and change your configuration file accordingly. To troubleshoot your strongSwan VPN connectivity:

1. Check the on-prem VPN endpoint settings against the settings in your configuration file. If there are mismatches:

    <ol>
    <li>Delete the existing Helm chart.</br><pre class="codeblock"><code>helm delete --purge <release_name></code></pre></li>
    <li>Fix the incorrect values in the <code>config.yaml</code> file and save the updated file.</li>
    <li>Install the new Helm chart.</br><pre class="codeblock"><code>helm install -f config.yaml --namespace=kube-system --name=<release_name> bluemix/strongswan</code></pre></li>
    </ol>

2. If the VPN pod is in an `ERROR` state or continues to crash and restart, it might be due to parameter validation of the `ipsec.conf` settings in the chart's config map.

    <ol>
    <li>Check for any validation errors in the Strongswan pod logs.</br><pre class="codeblock"><code>kubectl logs -n kube-system $STRONGSWAN_POD</code></pre></li>
    <li>If there are validation errors, delete the existing Helm chart.</br><pre class="codeblock"><code>helm delete --purge <release_name></code></pre></li>
    <li>Fix the incorrect values in the `config.yaml` file and save the updated file.</li>
    <li>Install the new Helm chart.</br><pre class="codeblock"><code>helm install -f config.yaml --namespace=kube-system --name=<release_name> bluemix/strongswan</code></pre></li>
    </ol>

3. Run the 5 Helm tests included in the strongSwan chart definition.

    <ol>
    <li>Run the Helm tests.</br><pre class="codeblock"><code>helm test vpn</code></pre></li>
    <li>If any test fails, refer to [Understanding the Helm VPN connectivity tests](cs_vpn.html#vpn_tests_table) for information about each test and why it might fail. <b>Note</b>: Some of the tests have requirements that are optional settings in the VPN configuration. If some of the tests fail, the failures might be acceptable depending on whether you specified these optional settings.</li>
    <li>View the output of a failed test by looking at the logs of the test pod.<br><pre class="codeblock"><code>kubectl logs -n kube-system <test_program></code></pre></li>
    <li>Delete the existing Helm chart.</br><pre class="codeblock"><code>helm delete --purge <release_name></code></pre></li>
    <li>Fix the incorrect values in the <code>config.yaml</code> file and save the updated file.</li>
    <li>Install the new Helm chart.</br><pre class="codeblock"><code>helm install -f config.yaml --namespace=kube-system --name=<release_name> bluemix/strongswan</code></pre></li>
    <li>To check your changes:<ol><li>Get the current test pods.</br><pre class="codeblock"><code>kubectl get pods -a -n kube-system -l app=strongswan-test</code></pre></li><li>Clean up the current test pods.</br><pre class="codeblock"><code>kubectl delete pods -n kube-system -l app=strongswan-test</code></pre></li><li>Run the tests again.</br><pre class="codeblock"><code>helm test vpn</code></pre></li>
    </ol></ol>

4. Run the VPN debugging tool packaged inside of the VPN pod image.

    1. Set the `STRONGSWAN_POD` environment variable.

        ```
        export STRONGSWAN_POD=$(kubectl get pod -n kube-system -l app=strongswan,release=vpn -o jsonpath='{ .items[0].metadata.name }')
        ```
        {: pre}

    2. Run the debugging tool.

        ```
        kubectl exec -n kube-system  $STRONGSWAN_POD -- vpnDebug
        ```
        {: pre}

        The tool outputs several pages of information as it runs various tests for common networking issues. Output lines that begin with `ERROR`, `WARNING`, `VERIFY`, or `CHECK` indicate possible errors with the VPN connectivity.

    <br />


## strongSwan VPN connectivity fails after worker node addition or deletion
{: #cs_vpn_fails_worker_add}

{: tsSymptoms}
You previously established a working VPN connection by using the strongSwan IPSec VPN service. However, after you added or deleted a worker node on your cluster, you experience one or more of the following symptoms:

* you do not have a VPN status of `ESTABLISHED`
* you cannot access new worker nodes from your on-prem network
* you can not access the remote network from pods that are running on new worker nodes

{: tsCauses}
If you added a worker node:

* the worker node was provisioned on a new private subnet that is not exposed over the VPN connection by your existing `localSubnetNAT` or `local.subnet` settings
* VPN routes cannot be added to the worker node because the worker has taints or labels that are not included in your existing `tolerations` or `nodeSelector` settings
* the VPN pod is running on the new worker node, but the public IP address of that worker node is not allowed through the on-premises firewall

If you deleted a worker node:

* that worker node was the only node where a VPN pod was running, due to restrictions on certain taints or labels in your existing `tolerations` or `nodeSelector` settings

{: tsResolve}
Update the Helm chart values to reflect the worker node changes:

1. Delete the existing Helm chart.

    ```
    helm delete --purge <release_name>
    ```
    {: pre}

2. Open the configuration file for your strongSwan VPN service.

    ```
    helm inspect values ibm/strongswan > config.yaml
    ```
    {: pre}

3. Check the following settings and make changes to reflect the deleted or added worker nodes as necessary.

    If you added a worker node:

    <table>
     <thead>
     <th>Setting</th>
     <th>Description</th>
     </thead>
     <tbody>
     <tr>
     <td><code>localSubnetNAT</code></td>
     <td>The added worker node might be deployed on a new, different private subnet than the other existing subnets that other worker nodes are on. If you are using subnet NAT to remap your cluster's private local IP addresses and worker node was added on a new subnet, add the new subnet CIDR to this setting.</td>
     </tr>
     <tr>
     <td><code>nodeSelector</code></td>
     <td>If you previously limited the VPN pod to running on any worker nodes with a specific label, and you want VPN routes to be added to the worker, make sure the added worker node has that label.</td>
     </tr>
     <tr>
     <td><code>tolerations</code></td>
     <td>If the added worker node is tainted, and you want VPN routes to be added to the worker, then change this setting to allow the VPN pod to run on all tainted worker nodes or worker nodes with specific taints.</td>
     </tr>
     <tr>
     <td><code>local.subnet</code></td>
     <td>The added worker node might be deployed on a new, different private subnet than the other existing subnets that other worker nodes are on. If your apps are exposed by NodePort or LoadBalancer services on the private network and they are on a new worker node that you added, add the new subnet CIDR to this setting. **Note**: If you add values to `local.subnet`, check the VPN settings for the on-premises subnet to see whether they also must be updated.</td>
     </tr>
     </tbody></table>

    If you deleted a worker node:

    <table>
     <thead>
     <th>Setting</th>
     <th>Description</th>
     </thead>
     <tbody>
     <tr>
     <td><code>localSubnetNAT</code></td>
     <td>If you are using subnet NAT to remap specific private local IP addresses, remove any IP addresses from this setting that are from the old worker node. If you are using subnet NAT to remap entire subnets and you have no worker nodes remaining on a subnet, remove that subnet CIDR from this setting.</td>
     </tr>
     <tr>
     <td><code>nodeSelector</code></td>
     <td>If you previously limited the VPN pod to running on a single worker node and that worker node was deleted, change this setting to allow the VPN pod to run on other worker nodes.</td>
     </tr>
     <tr>
     <td><code>tolerations</code></td>
     <td>If the worker node that you deleted was not tainted, but the only worker nodes that remain are tainted, change this setting to allow the VPN pod to run on all tainted worker nodes or worker nodes with specific taints.
     </td>
     </tr>
     </tbody></table>

4. Install the new Helm chart with your updated values.

    ```
    helm install -f config.yaml --namespace=kube-system --name=<release_name> ibm/strongswan
    ```
    {: pre}

5. Check the chart deployment status. When the chart is ready, the **STATUS** field near the top of the output has a value of `DEPLOYED`.

    ```
    helm status <release_name>
    ```
    {: pre}

6. In some cases, you might need to change your on-prem settings and your firewall settings to match the changes you made to the VPN config file.

7. Start the VPN.
    * If the VPN connection is initiated by the cluster (`ipsec.auto` is set to `start`), start the VPN on the on-prem gateway, and then start the VPN on the cluster.
    * If the VPN connection is initiated by the on-prem gateway (`ipsec.auto` is set to `auto`), start the VPN on the cluster, and then start the VPN on the on-prem gateway.

8. Set the `STRONGSWAN_POD` environment variable.

    ```
    export STRONGSWAN_POD=$(kubectl get pod -n kube-system -l app=strongswan,release=<release_name> -o jsonpath='{ .items[0].metadata.name }')
    ```
    {: pre}

9. Check the status of the VPN.

    ```
    kubectl exec -n kube-system  $STRONGSWAN_POD -- ipsec status
    ```
    {: pre}

    * If the VPN connection has a status of `ESTABLISHED`, the VPN connection was successful. No further action is needed.

    * If you are still having connection issues, see [Cannot establish VPN connectivity with the strongSwan Helm chart](#cs_vpn_fails) to further troubleshoot your VPN connection.

<br />


## Cannot retrieve the ETCD URL for Calico CLI configuration
{: #cs_calico_fails}

{: tsSymptoms}
When you retrieve the `<ETCD_URL>` to [add network policies](cs_network_policy.html#adding_network_policies), you get a `calico-config not found` error message.

{: tsCauses}
Your cluster is not at [Kubernetes version 1.7](cs_versions.html) or later.

{: tsResolve}
[Update your cluster](cs_cluster_update.html#master) or retrieve the `<ETCD_URL>` with commands that are compatible with earlier versions of Kubernetes.

To retrieve the `<ETCD_URL>`, run one of the following commands:

- Linux and OS X:

    ```
    kubectl describe pod -n kube-system `kubectl get pod -n kube-system | grep calico-policy-controller | awk '{print $1}'` | grep ETCD_ENDPOINTS | awk '{print $2}'
    ```
    {: pre}

- Windows:
    <ol>
    <li> Get a list of the pods in the kube-system namespace and locate the Calico controller pod. </br><pre class="codeblock"><code>kubectl get pod -n kube-system</code></pre></br>Example:</br><pre class="screen"><code>calico-policy-controller-1674857634-k2ckm</code></pre>
    <li> View the details of the Calico controller pod.</br> <pre class="codeblock"><code>kubectl describe pod -n kube-system calico-policy-controller-&lt;ID&gt;</code></pre>
    <li> Locate the ETCD endpoints value. Example: <code>https://169.1.1.1:30001</code>
    </ol>

When you retrieve the `<ETCD_URL>`, continue with the steps as listed in (Adding network policies)[cs_network_policy.html#adding_network_policies].

<br />


## Getting help and support
{: #ts_getting_help}

Still having issues with your cluster?
{: shortdesc}

-   To see whether {{site.data.keyword.Bluemix_notm}} is available, [check the {{site.data.keyword.Bluemix_notm}} status page ![External link icon](../icons/launch-glyph.svg "External link icon")](https://developer.ibm.com/bluemix/support/#status).
-   Post a question in the [{{site.data.keyword.containershort_notm}} Slack. ![External link icon](../icons/launch-glyph.svg "External link icon")](https://ibm-container-service.slack.com) Tip: If you are not using an IBM ID for your {{site.data.keyword.Bluemix_notm}} account, [request an invitation](https://bxcs-slack-invite.mybluemix.net/) to this Slack.
{: tip}
-   Review the forums to see whether other users ran into the same issue. When you use the forums to ask a question, tag your question so that it is seen by the {{site.data.keyword.Bluemix_notm}} development teams.

    -   If you have technical questions about developing or deploying clusters or apps with {{site.data.keyword.containershort_notm}}, post your question on [Stack Overflow ![External link icon](../icons/launch-glyph.svg "External link icon")](https://stackoverflow.com/questions/tagged/ibm-cloud+containers) and tag your question with `ibm-cloud`, `kubernetes`, and `containers`.
    -   For questions about the service and getting started instructions, use the [IBM developerWorks dW Answers ![External link icon](../icons/launch-glyph.svg "External link icon")](https://developer.ibm.com/answers/topics/containers/?smartspace=bluemix) forum. Include the `ibm-cloud` and `containers` tags.
    See [Getting help](/docs/get-support/howtogetsupport.html#using-avatar) for more details about using the forums.

-   Contact IBM Support by opening a ticket. For information about opening an IBM support ticket, or about support levels and ticket severities, see [Contacting support](/docs/get-support/howtogetsupport.html#getting-customer-support).

{:tip}
When reporting an issue, include your cluster ID. To get your cluster ID, run `bx cs clusters`.

