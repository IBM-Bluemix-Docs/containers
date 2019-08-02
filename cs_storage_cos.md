---

copyright:
  years: 2014, 2019
lastupdated: "2019-07-31"

keywords: kubernetes, iks

subcollection: containers

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:preview: .preview}

# Storing data on IBM Cloud Object Storage
{: #object_storage}

[{{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-about-ibm-cloud-object-storage) is persistent, highly available storage that you can mount to apps that run in a Kubernetes cluster by using the {{site.data.keyword.cos_full_notm}} plug-in. The plug-in is a Kubernetes Flex-Volume plug-in that connects Cloud {{site.data.keyword.cos_short}} buckets to pods in your cluster. Information that is stored with {{site.data.keyword.cos_full_notm}} is encrypted in transit and at rest, dispersed across multiple geographic locations, and accessed over HTTP by using a REST API.
{: shortdesc}

To connect to {{site.data.keyword.cos_full_notm}}, your cluster requires public network access to authenticate with {{site.data.keyword.cloud_notm}} Identity and Access Management. If you have a private-only cluster, you can communicate with the {{site.data.keyword.cos_full_notm}} private service endpoint if you install the plug-in version `1.0.3` or later, and set up your {{site.data.keyword.cos_full_notm}} service instance for HMAC authentication. If you don't want to use HMAC authentication, you must open up all outbound network traffic on port 443 for the plug-in to work properly in a private cluster.
{: important}

With version 1.0.5, the {{site.data.keyword.cos_full_notm}} plug-in is renamed from `ibmcloud-object-storage-plugin` to `ibm-object-storage-plugin`. To install the new version of the plug-in, you must [uninstall the old Helm chart installation](#remove_cos_plugin) and [reinstall the Helm chart with the new {{site.data.keyword.cos_full_notm}} plug-in version](#install_cos).
{: note}

With version 1.0.8, the {{site.data.keyword.cos_full_notm}} plug-in Helm chart is now available in the `ibm-charts` Helm repository. Make sure to fetch the latest version of the Helm chart from this repository. To add the repository, run `helm repo add ibm-charts https://icr.io/helm/ibm-charts`.
{: note}

## Creating your object storage service instance
{: #create_cos_service}

Before you can start using object storage in your cluster, you must provision an {{site.data.keyword.cos_full_notm}} service instance in your account.
{: shortdesc}

The {{site.data.keyword.cos_full_notm}} plug-in is configured to work with any s3 API endpoint. For example, you might want to use a local Cloud Object Storage server, such as [Minio](https://cloud.ibm.com/kubernetes/solutions/helm-charts/ibm-charts/ibm-minio-objectstore), or connect to an s3 API endpoint that you set up at a different cloud provider instead of using an {{site.data.keyword.cos_full_notm}} service instance.

Follow these steps to create an {{site.data.keyword.cos_full_notm}} service instance. If you plan to use a local Cloud Object Storage server or a different s3 API endpoint, refer to the provider documentation to set up your Cloud Object Storage instance.

1. Deploy an {{site.data.keyword.cos_full_notm}} service instance.
   1.  Open the [{{site.data.keyword.cos_full_notm}} catalog page](https://cloud.ibm.com/catalog/services/cloud-object-storage).
   2.  Enter a name for your service instance, such as `cos-backup`, and select the same resource group that your cluster is in. To view the resource group of your cluster, run `ibmcloud ks cluster-get --cluster <cluster_name_or_ID>`.   
   3.  Review the [plan options ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/cloud-computing/bluemix/pricing-object-storage#s3api) for pricing information and select a plan.
   4.  Click **Create**. The service details page opens.
2. {: #service_credentials}Retrieve the {{site.data.keyword.cos_full_notm}} service credentials.
   1.  In the navigation on the service details page, click **Service Credentials**.
   2.  Click **New credential**. A dialog box displays.
   3.  Enter a name for your credentials.
   4.  From the **Role** drop-down, select `Writer` or `Manager`. When you select `Reader`, then you cannot use the credentials to create buckets in {{site.data.keyword.cos_full_notm}} and write data to it.
   5.  Optional: In **Add Inline Configuration Parameters (Optional)**, enter `{"HMAC":true}` to create additional HMAC credentials for the {{site.data.keyword.cos_full_notm}} service. HMAC authentication adds an extra layer of security to the OAuth2 authentication by preventing the misuse of expired or randomly created OAuth2 tokens. **Important**: If you have a private-only cluster with no public access, you must use HMAC authentication so that you can access the {{site.data.keyword.cos_full_notm}} service over the private network.
   6.  Click **Add**. Your new credentials are listed in the **Service Credentials** table.
   7.  Click **View credentials**.
   8.  Make note of the **apikey** to use OAuth2 tokens to authenticate with the {{site.data.keyword.cos_full_notm}} service. For HMAC authentication, in the **cos_hmac_keys** section, note the **access_key_id** and the **secret_access_key**.
3. [Store your service credentials in a Kubernetes secret inside the cluster](#create_cos_secret) to enable access to your {{site.data.keyword.cos_full_notm}} service instance.

## Creating a secret for the object storage service credentials
{: #create_cos_secret}

To access your {{site.data.keyword.cos_full_notm}} service instance to read and write data, you must securely store the service credentials in a Kubernetes secret. The {{site.data.keyword.cos_full_notm}} plug-in uses these credentials for every read or write operation to your bucket.
{: shortdesc}

Follow these steps to create a Kubernetes secret for the credentials of an {{site.data.keyword.cos_full_notm}} service instance. If you plan to use a local Cloud Object Storage server or a different s3 API endpoint, create a Kubernetes secret with the appropriate credentials.

Before you begin: [Log in to your account. If applicable, target the appropriate resource group. Set the context for your cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1. Retrieve the **apikey**, or the **access_key_id** and the **secret_access_key** of your [{{site.data.keyword.cos_full_notm}} service credentials](#service_credentials).

2. Get the **GUID** of your {{site.data.keyword.cos_full_notm}} service instance.
   ```
   ibmcloud resource service-instance <service_name> | grep GUID
   ```
   {: pre}

3. Create a Kubernetes secret to store your service credentials. When you create your secret, all values are automatically encoded to base64.

   **Example for using the API key:**
   ```
   kubectl create secret generic cos-write-access --type=ibm/ibmc-s3fs --from-literal=api-key=<api_key> --from-literal=service-instance-id=<service_instance_guid>
   ```
   {: pre}

   **Example for HMAC authentication:**
   ```
   kubectl create secret generic cos-write-access --type=ibm/ibmc-s3fs --from-literal=access-key=<access_key_ID> --from-literal=secret-key=<secret_access_key>    
   ```
   {: pre}

   <table>
   <caption>Understanding the command components</caption>
   <thead>
   <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the command components</th>
   </thead>
   <tbody>
   <tr>
   <td><code>api-key</code></td>
   <td>Enter the API key that you retrieved from your {{site.data.keyword.cos_full_notm}} service credentials earlier. If you want to use HMAC authentication, specify the <code>access-key</code> and <code>secret-key</code> instead.  </td>
   </tr>
   <tr>
   <td><code>access-key</code></td>
   <td>Enter the access key ID that you retrieved from your {{site.data.keyword.cos_full_notm}} service credentials earlier. If you want to use OAuth2 authentication, specify the <code>api-key</code> instead.  </td>
   </tr>
   <tr>
   <td><code>secret-key</code></td>
   <td>Enter the secret access key that you retrieved from your {{site.data.keyword.cos_full_notm}} service credentials earlier. If you want to use OAuth2 authentication, specify the <code>api-key</code> instead.</td>
   </tr>
   <tr>
   <td><code>service-instance-id</code></td>
   <td>Enter the GUID of your {{site.data.keyword.cos_full_notm}} service instance that you retrieved earlier. </td>
   </tr>
   </tbody>
   </table>

4. Verify that the secret is created in your namespace.
   ```
   kubectl get secret
   ```
   {: pre}

5. [Install the {{site.data.keyword.cos_full_notm}} plug-in](#install_cos), or if you already installed the plug-in, [decide on the configuration]( #configure_cos) for your {{site.data.keyword.cos_full_notm}} bucket.

## Installing the IBM Cloud Object Storage plug-in
{: #install_cos}

Install the {{site.data.keyword.cos_full_notm}} plug-in with a Helm chart to set up pre-defined storage classes for {{site.data.keyword.cos_full_notm}}. You can use these storage classes to create a PVC to provision {{site.data.keyword.cos_full_notm}} for your apps.
{: shortdesc}

Looking for instructions for how to update or remove the {{site.data.keyword.cos_full_notm}} plug-in? See [Updating the plug-in](#update_cos_plugin) and [Removing the plug-in](#remove_cos_plugin).
{: tip}

Before you begin: [Log in to your account. If applicable, target the appropriate resource group. Set the context for your cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1. Make sure that your worker node applies the latest patch for your minor version.
   1. List the current patch version of your worker nodes.
      ```
      ibmcloud ks workers --cluster <cluster_name_or_ID>
      ```
      {: pre}

      Example output:
      ```
      OK
      ID                                                  Public IP        Private IP     Machine Type           State    Status   Zone    Version
      kube-dal10-crb1a23b456789ac1b20b2nc1e12b345ab-w26   169.xx.xxx.xxx    10.xxx.xx.xxx   b3c.4x16.encrypted     normal   Ready    dal10   1.13.8_1523*
      ```
      {: screen}

      If your worker node does not apply the latest patch version, you see an asterisk (`*`) in the **Version** column of your CLI output.

   2. Review the [version changelog](/docs/containers?topic=containers-changelog#changelog) to find the changes that are included in the latest patch version.

   3. Apply the latest patch version by reloading your worker node. Follow the instructions in the [ibmcloud ks worker-reload command](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload) to gracefully reschedule any running pods on your worker node before you reload your worker node. Note that during the reload, your worker node machine is updated with the latest image and data is deleted if not [stored outside the worker node](/docs/containers?topic=containers-storage_planning#persistent_storage_overview).

2.  Choose if you want to install the {{site.data.keyword.cos_full_notm}} plug-in with or without the Helm server, Tiller. Then, [follow the instructions](/docs/containers?topic=containers-helm#public_helm_install) to install the Helm client on your local machine and optionally Tiller with a service account in your cluster. **Note**: If you use Windows, you must install Tiller.

3. If you want to install the plug-in with Tiller, verify that Tiller is installed with a service account.
   ```
   kubectl get serviceaccount -n kube-system tiller
   ```
   {: pre}

   Example output:
   ```
   NAME                                 SECRETS   AGE
   tiller                               1         2m
   ```
   {: screen}

4. Add the {{site.data.keyword.cloud_notm}} Helm repo to your cluster.
   ```
   helm repo add ibm-charts https://icr.io/helm/ibm-charts
   ```
   {: pre}

4. Update the Helm repo to retrieve the latest version of all Helm charts in this repo.
   ```
   helm repo update
   ```
   {: pre}

5. Download the Helm charts and unpack the charts in your current directory.
   ```
   helm fetch --untar ibm-charts/ibm-object-storage-plugin
   ```
   {: pre}

7. If you use OS X or a Linux distribution, install the {{site.data.keyword.cos_full_notm}} Helm plug-in `ibmc`. The plug-in is used to automatically retrieve your cluster location and to set the API endpoint for your {{site.data.keyword.cos_full_notm}} buckets in your storage classes. If you use Windows as your operating system, continue with the next step.
   1. Install the Helm plug-in.
      ```
      helm plugin install ./ibm-object-storage-plugin/helm-ibmc
      ```
      {: pre}

      Example output:
      ```
      Installed plugin: ibmc
      ```
      {: screen}

      If you see the error `Error: plugin already exists`, remove the `ibmc` Helm plug-in by running `rm -rf ~/.helm/plugins/helm-ibmc`.
      {: tip}

   2. Verify that the `ibmc` plug-in is installed successfully.
      ```
      helm ibmc --help
      ```
      {: pre}

      Example output:
      ```
      Install or upgrade Helm charts in IBM K8S Service(IKS) and IBM Cloud Private(ICP)

      Available Commands:
         helm ibmc install [CHART] [flags]                      Install a Helm chart
         helm ibmc upgrade [RELEASE] [CHART] [flags]            Upgrade the release to a new version of the Helm chart
         helm ibmc template [CHART] [flags] [--apply|--delete]  Install/uninstall a Helm chart without tiller

      Available Flags:
         -h, --help                    (Optional) This text.
         -u, --update                  (Optional) Update this plugin to the latest version

      Example Usage:
         With Tiller:
             Install:   helm ibmc install ibm-charts/ibm-object-storage-plugin --name ibm-object-storage-plugin
         Without Tiller:
             Install:   helm ibmc template ibm-charts/ibm-object-storage-plugin --apply
             Dry-run:   helm ibmc template ibm-charts/ibm-object-storage-plugin
             Uninstall: helm ibmc template ibm-charts/ibm-object-storage-plugin --delete

      Note:
         1. It is always recommended to install latest version of ibm-object-storage-plugin chart.
         2. It is always recommended to have 'kubectl' client up-to-date.
      ```
      {: screen}

      If the output shows the error `Error: fork/exec /home/iksadmin/.helm/plugins/helm-ibmc/ibmc.sh: permission denied`, run `chmod 755 ~/.helm/plugins/helm-ibmc/ibmc.sh`. Then, rerun `helm ibmc --help`.
      {: tip}

8. Optional: Limit the {{site.data.keyword.cos_full_notm}} plug-in to access only the Kubernetes secrets that hold your {{site.data.keyword.cos_full_notm}} service credentials. By default, the plug-in is authorized to access all Kubernetes secrets in your cluster.
   1. [Create your {{site.data.keyword.cos_full_notm}} service instance](#create_cos_service).
   2. [Store your {{site.data.keyword.cos_full_notm}} service credentials in a Kubernetes secret](#create_cos_secret).
   3. Navigate to the `templates` directory and list available files.  
      ```
      cd ibm-object-storage-plugin/templates && ls
      ```
      {: pre}

   4. Open the `provisioner-sa.yaml` file and look for the `ibmcloud-object-storage-secret-reader` `ClusterRole` definition.
   6. Add the name of the secret that you created earlier to the list of secrets that the plug-in is authorized to access in the `resourceNames` section.
      ```
      kind: ClusterRole
      apiVersion: rbac.authorization.k8s.io/v1beta1
      metadata:
        name: ibmcloud-object-storage-secret-reader
      rules:
      - apiGroups: [""]
        resources: ["secrets"]
        resourceNames: ["<secret_name1>","<secret_name2>"]
        verbs: ["get"]
      ```
      {: codeblock}
   7. Save your changes.

9. Install the {{site.data.keyword.cos_full_notm}} plug-in. When you install the plug-in, pre-defined storage classes are added to your cluster.

   - **For OS X and Linux:**
     - If you skipped the previous step, install without a limitation to specific Kubernetes secrets.</br>
       **Without Tiller**:
       ```
       helm ibmc template ibm-charts/ibm-object-storage-plugin --apply
       ```
       {: pre}

       **With Tiller**:
       ```
       helm ibmc install ibm-charts/ibm-object-storage-plugin --name ibm-object-storage-plugin
       ```
       {: pre}

     - If you completed the previous step, install with a limitation to specific Kubernetes secrets.</br>
       **Without Tiller**:
       ```
       cd ../..
       helm ibmc template ./ibm-object-storage-plugin --apply
       ```
       {: pre}

       **With Tiller**:
       ```
       cd ../..
       helm ibmc install ./ibm-object-storage-plugin --name ibm-object-storage-plugin
       ```
       {: pre}

   - **For Windows:**
     1. Retrieve the zone where your cluster is deployed and store the zone in an environment variable.
        ```
        export DC_NAME=$(kubectl get cm cluster-info -n kube-system -o jsonpath='{.data.cluster-config\.json}' | grep datacenter | awk -F ': ' '{print $2}' | sed 's/\"//g' |sed 's/,//g')
        ```
        {: pre}

     2. Verify that the environment variable is set.
        ```
        printenv
        ```
        {: pre}

     3. Install the Helm chart.
        - If you skipped the previous step, install without a limitation to specific Kubernetes secrets.</br>
          ```
          helm install ibm-charts/ibm-object-storage-plugin --name ibm-object-storage-plugin
          ```
          {: pre}

        - If you completed the previous step, install with a limitation to specific Kubernetes secrets.</br>
          ```
          cd ../..
          helm install ./ibm-object-storage-plugin --name ibm-object-storage-plugin
          ```
          {: pre}

   Example output for installing without Tiller:
   ```
   Rendering the Helm chart templates...
   DC: dal10
   Chart: ibm-charts/ibm-object-storage-plugin
   wrote object-storage-templates/ibm-object-storage-plugin/templates/ibmc-s3fs-cold-cross-region.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/ibmc-s3fs-cold-regional.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/ibmc-s3fs-flex-cross-region.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/ibmc-s3fs-flex-perf-cross-region.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/ibmc-s3fs-flex-perf-regional.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/ibmc-s3fs-flex-regional.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/ibmc-s3fs-standard-cross-region.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/ibmc-s3fs-standard-perf-cross-region.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/ibmc-s3fs-standard-perf-regional.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/ibmc-s3fs-standard-regional.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/ibmc-s3fs-vault-cross-region.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/ibmc-s3fs-vault-regional.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/flex-driver-sa.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/provisioner-sa.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/flex-driver.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/tests/check-driver-install.yaml
   wrote object-storage-templates/ibm-object-storage-plugin/templates/provisioner.yaml
   Installing the Helm chart...
   serviceaccount/ibmcloud-object-storage-driver created
   daemonset.apps/ibmcloud-object-storage-driver created
   storageclass.storage.k8s.io/ibmc-s3fs-cold-cross-region created
   storageclass.storage.k8s.io/ibmc-s3fs-cold-regional created
   storageclass.storage.k8s.io/ibmc-s3fs-flex-cross-region created
   storageclass.storage.k8s.io/ibmc-s3fs-flex-perf-cross-region created
   storageclass.storage.k8s.io/ibmc-s3fs-flex-perf-regional created
   storageclass.storage.k8s.io/ibmc-s3fs-flex-regional created
   storageclass.storage.k8s.io/ibmc-s3fs-standard-cross-region created
   storageclass.storage.k8s.io/ibmc-s3fs-standard-perf-cross-region created
   storageclass.storage.k8s.io/ibmc-s3fs-standard-perf-regional created
   storageclass.storage.k8s.io/ibmc-s3fs-standard-regional created
   storageclass.storage.k8s.io/ibmc-s3fs-vault-cross-region created
   storageclass.storage.k8s.io/ibmc-s3fs-vault-regional created
   serviceaccount/ibmcloud-object-storage-plugin created
   clusterrole.rbac.authorization.k8s.io/ibmcloud-object-storage-plugin created
   clusterrole.rbac.authorization.k8s.io/ibmcloud-object-storage-secret-reader created
   clusterrolebinding.rbac.authorization.k8s.io/ibmcloud-object-storage-plugin created
   clusterrolebinding.rbac.authorization.k8s.io/ibmcloud-object-storage-secret-reader created
   deployment.apps/ibmcloud-object-storage-plugin created
   pod/ibmcloud-object-storage-driver-test created
   ```
   {: screen}

10. Verify that the plug-in is installed correctly.
    ```
    kubectl get pod -n default -o wide | grep object
    ```
    {: pre}

    Example output:
    ```
    ibmcloud-object-storage-driver-9n8g8                              1/1       Running   0          2m
    ibmcloud-object-storage-plugin-7c774d484b-pcnnx                   1/1       Running   0          2m
    ```
    {: screen}

    The installation is successful when you see one `ibmcloud-object-storage-plugin` pod and one or more `ibmcloud-object-storage-driver` pods. The number of `ibmcloud-object-storage-driver` pods equals the number of worker nodes in your cluster. All pods must be in a `Running` state for the plug-in to function properly. If the pods fail, run `kubectl describe pod -n kube-system <pod_name>` to find the root cause for the failure.

11. Verify that the storage classes are created successfully.
    ```
    kubectl get storageclass | grep s3
    ```
    {: pre}

    Example output:
    ```
    ibmc-s3fs-cold-cross-region            ibm.io/ibmc-s3fs   8m
    ibmc-s3fs-cold-regional                ibm.io/ibmc-s3fs   8m
    ibmc-s3fs-flex-cross-region            ibm.io/ibmc-s3fs   8m
    ibmc-s3fs-flex-perf-cross-region       ibm.io/ibmc-s3fs   8m
    ibmc-s3fs-flex-perf-regional           ibm.io/ibmc-s3fs   8m
    ibmc-s3fs-flex-regional                ibm.io/ibmc-s3fs   8m
    ibmc-s3fs-standard-cross-region        ibm.io/ibmc-s3fs   8m
    ibmc-s3fs-standard-perf-cross-region   ibm.io/ibmc-s3fs   8m
    ibmc-s3fs-standard-perf-regional       ibm.io/ibmc-s3fs   8m
    ibmc-s3fs-standard-regional            ibm.io/ibmc-s3fs   8m
    ibmc-s3fs-vault-cross-region           ibm.io/ibmc-s3fs   8m
    ibmc-s3fs-vault-regional               ibm.io/ibmc-s3fs   8m
    ```
    {: screen}

12. Repeat the steps for all clusters where you want to access {{site.data.keyword.cos_full_notm}} buckets.

### Updating the IBM Cloud Object Storage plug-in
{: #update_cos_plugin}

You can upgrade the existing {{site.data.keyword.cos_full_notm}} plug-in to the latest version.
{: shortdesc}

1. If you previously installed version 1.0.4 or earlier of the Helm chart that is named `ibmcloud-object-storage-plugin`, remove this Helm installation from your cluster. Then, reinstall the Helm chart.
   1. Check whether the old version of the {{site.data.keyword.cos_full_notm}} Helm chart is installed in your cluster.  
      ```
      helm ls | grep ibmcloud-object-storage-plugin
      ```
      {: pre}

      Example output:
      ```
      ibmcloud-object-storage-plugin	1       	Mon Sep 18 15:31:40 2017	DEPLOYED	ibmcloud-object-storage-plugin-1.0.4	default
      ```
      {: screen}

   2. If you have version 1.0.4 or earlier of the Helm chart that is named `ibmcloud-object-storage-plugin`, remove the Helm chart from your cluster. If you have version 1.0.5 or later of the Helm chart that is named `ibm-object-storage-plugin`, continue with Step 2.
      ```
      helm delete --purge ibmcloud-object-storage-plugin
      ```
      {: pre}

   3. Follow the steps in [Installing the {{site.data.keyword.cos_full_notm}} plug-in](#install_cos) to install the latest version of the {{site.data.keyword.cos_full_notm}} plug-in.

2. Update the {{site.data.keyword.cloud_notm}} Helm repo to retrieve the latest version of all Helm charts in this repo.
   ```
   helm repo update
   ```
   {: pre}

3. If you use OS X or a Linux distribution, update the {{site.data.keyword.cos_full_notm}} `ibmc` Helm plug-in to the latest version.
   ```
   helm ibmc --update
   ```
   {: pre}

4. Download the latest {{site.data.keyword.cos_full_notm}} Helm chart to your local machine and extract the package to review the `release.md` file to find the latest release information.
   ```
   helm fetch --untar ibm-charts/ibm-object-storage-plugin
   ```
   {: pre}

5. Upgrade the plug-in. </br>
   **Without Tiller**:
   ```
   helm ibmc template ibm-charts/ibm-object-storage-plugin --update
   ```
   {: pre}

   **With Tiller**:
   1. Find the installation name of your Helm chart.
      ```
      helm ls | grep ibm-object-storage-plugin
      ```
      {: pre}

      Example output:
      ```
      <helm_chart_name> 	1       	Mon Sep 18 15:31:40 2017	DEPLOYED	ibm-object-storage-plugin-1.0.5	default
      ```
      {: screen}

   2. Upgrade the {{site.data.keyword.cos_full_notm}} Helm chart to the latest version.
      ```   
      helm ibmc upgrade <helm_chart_name> ibm-charts/ibm-object-storage-plugin --force --recreate-pods -f
      ```
      {: pre}

6. Verify that the `ibmcloud-object-storage-plugin` is successfully upgraded.  
   ```
   kubectl rollout status deployment/ibmcloud-object-storage-plugin -n kube-system
   ```
   {: pre}

   The upgrade of the plug-in is successful when you see `deployment "ibmcloud-object-storage-plugin" successfully rolled out` in your CLI output.

7. Verify that the `ibmcloud-object-storage-driver` is successfully upgraded.
   ```
   kubectl rollout status ds/ibmcloud-object-storage-driver -n kube-system
   ```
   {: pre}

   The upgrade is successful when you see `daemon set "ibmcloud-object-storage-driver" successfully rolled out` in your CLI output.

8. Verify that the {{site.data.keyword.cos_full_notm}} pods are in a `Running` state.
   ```
   kubectl get pods -n kube-system -o wide | grep object-storage
   ```
   {: pre}


### Removing the IBM Cloud Object Storage plug-in
{: #remove_cos_plugin}

If you do not want to provision and use {{site.data.keyword.cos_full_notm}} in your cluster, you can uninstall the plug-in.
{: shortdesc}

Removing the plug-in does not remove existing PVCs, PVs, or data. When you remove the plug-in, all the related pods and daemon sets are removed from your cluster. You cannot provision new {{site.data.keyword.cos_full_notm}} for your cluster or use existing PVCs and PVs after you remove the plug-in, unless you configure your app to use the {{site.data.keyword.cos_full_notm}} API directly.
{: important}

Before you begin:

- [Target your CLI to the cluster](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure).
- Make sure that you do not have any PVCs or PVs in your cluster that use {{site.data.keyword.cos_full_notm}}. To list all pods that mount a specific PVC, run `kubectl get pods --all-namespaces -o=jsonpath='{range .items[*]}{"\n"}{.metadata.name}{":\t"}{range .spec.volumes[*]}{.persistentVolumeClaim.claimName}{" "}{end}{end}' | grep "<pvc_name>"`.

To remove the plug-in:

1. Remove the plug-in from your cluster. </br>
   **With Tiller**:
   1. Find the installation name of your Helm chart.
      ```
      helm ls | grep object-storage-plugin
      ```
      {: pre}

      Example output:
      ```
      <helm_chart_name> 	1       	Mon Sep 18 15:31:40 2017	DEPLOYED	ibmcloud-object-storage-plugin-1.0.0	default
      ```
      {: screen}

   2. Delete the {{site.data.keyword.cos_full_notm}} plug-in by removing the Helm chart.
      ```
      helm delete --purge <helm_chart_name>
      ```
      {: pre}

   **Without Tiller**:
   ```
   helm ibmc template ibm-charts/ibm-object-storage-plugin --delete
   ```
   {: pre}

2. Verify that the {{site.data.keyword.cos_full_notm}} pods are removed.
   ```
   kubectl get pod -n kube-system | grep object-storage
   ```
   {: pre}

   The removal of the pods is successful if no pods are displayed in your CLI output.

3. Verify that the storage classes are removed.
   ```
   kubectl get storageclasses | grep s3
   ```
   {: pre}

   The removal of the storage classes is successful if no storage classes are displayed in your CLI output.

4. If you use OS X or a Linux distribution, remove the `ibmc` Helm plug-in. If you use Windows, this step is not required.
   1. Remove the `ibmc` plug-in.
      ```
      rm -rf ~/.helm/plugins/helm-ibmc
      ```
      {: pre}

   2. Verify that the `ibmc` plug-in is removed.
      ```
      helm plugin list
      ```
      {: pre}

      Example output:
     ```
     NAME	VERSION	DESCRIPTION
     ```
     {: screen}

     The `ibmc` plug-in is removed successfully if the `ibmc` plug-in is not listed in your CLI output.


## Deciding on the object storage configuration
{: #configure_cos}

{{site.data.keyword.containerlong_notm}} provides pre-defined storage classes that you can use to create buckets with a specific configuration.
{: shortdesc}

1. List available storage classes in {{site.data.keyword.containerlong_notm}}.
   ```
   kubectl get storageclasses | grep s3
   ```
   {: pre}

   Example output:
   ```
   ibmc-s3fs-cold-cross-region            ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-cold-regional                ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-flex-cross-region            ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-flex-perf-cross-region       ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-flex-perf-regional           ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-flex-regional                ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-standard-cross-region        ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-standard-perf-cross-region   ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-standard-perf-regional       ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-standard-regional            ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-vault-cross-region           ibm.io/ibmc-s3fs   8m
   ibmc-s3fs-vault-regional               ibm.io/ibmc-s3fs   8m
   ```
   {: screen}

2. Choose a storage class that fits your data access requirements. The storage class determines the [pricing ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/cloud-computing/bluemix/pricing-object-storage#s3api) for storage capacity, read and write operations, and outbound bandwidth for a bucket. The option that is right for you is based on how frequently data is read and written to your service instance.
   - **Standard**: This option is used for hot data that is accessed frequently. Common use cases are web or mobile apps.
   - **Vault**: This option is used for workloads or cool data that are accessed infrequently, such as once a month or less. Common use cases are archives, short-term data retention, digital asset preservation, tape replacement, and disaster recovery.
   - **Cold**: This option is used for cold data that is rarely accessed (every 90 days or less), or inactive data. Common use cases are archives, long-term backups, historical data that you keep for compliance, or workloads and apps that are rarely accessed.
   - **Flex**: This option is used for workloads and data that do not follow a specific usage pattern, or that are too huge to determine or predict a usage pattern. **Tip:** Check out this [blog ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/blogs/bluemix/2017/03/interconnect-2017-changing-rules-storage/) to learn how the Flex storage class works compared to traditional storage tiers.   

3. Decide on the level of resiliency for the data that is stored in your bucket.
   - **Cross-region**: With this option, your data is stored across three regions within a geolocation for highest availability. If you have workloads that are distributed across regions, requests are routed to the nearest regional endpoint. The API endpoint for the geolocation is automatically set by the `ibmc` Helm plug-in that you installed earlier based on the location that your cluster is in. For example, if your cluster is in `US South`, then your storage classes are configured to use the `US GEO` API endpoint for your buckets. For more information, see [Regions and endpoints](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-endpoints#endpoints).  
   - **Regional**: With this option, your data is replicated across multiple zones within one region. If you have workloads that are located in the same region, you see lower latency and better performance than in a cross-regional setup. The regional endpoint is automatically set by the `ibm` Helm plug-in that you installed earlier based on the location that your cluster is in. For example, if your cluster is in `US South`, then your storage classes were configured to use `US South` as the regional endpoint for your buckets. For more information, see [Regions and endpoints](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-endpoints#endpoints).

4. Review the detailed {{site.data.keyword.cos_full_notm}} bucket configuration for a storage class.
   ```
   kubectl describe storageclass <storageclass_name>
   ```
   {: pre}

   Example output:
   ```
   Name:                  ibmc-s3fs-standard-cross-region
   IsDefaultClass:        No
   Annotations:           <none>
   Provisioner:           ibm.io/ibmc-s3fs
   Parameters:            ibm.io/chunk-size-mb=16,ibm.io/curl-debug=false,ibm.io/debug-level=warn,ibm.io/iam-endpoint=https://iam.bluemix.net,ibm.io/kernel-cache=true,ibm.io/multireq-max=20,ibm.io/object-store-endpoint=https://s3-api.dal-us-geo.objectstorage.service.networklayer.com,ibm.io/object-store-storage-class=us-standard,ibm.io/parallel-count=2,ibm.io/s3fs-fuse-retry-count=5,ibm.io/stat-cache-size=100000,ibm.io/tls-cipher-suite=AESGCM
   AllowVolumeExpansion:  <unset>
   MountOptions:          <none>
   ReclaimPolicy:         Delete
   VolumeBindingMode:     Immediate
   Events:                <none>
   ```
   {: screen}

   <table>
   <caption>Understanding the storage class details</caption>
   <thead>
   <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the YAML file components</th>
   </thead>
   <tbody>
   <tr>
   <td><code>ibm.io/chunk-size-mb</code></td>
   <td>The size of a data chunk that is read from or written to {{site.data.keyword.cos_full_notm}} in megabytes. Storage classes with <code>perf</code> in their name are set up with 52 megabytes. Storage classes without <code>perf</code> in their name use 16 megabyte chunks. For example, if you want to read a file that is `1GB`, the plug-in reads this file in multiple 16 or 52-megabyte chunks. </td>
   </tr>
   <tr>
   <td><code>ibm.io/curl-debug</code></td>
   <td>Enable the logging of requests that are sent to the {{site.data.keyword.cos_full_notm}} service instance. If enabled, logs are sent to `syslog` and you can [forward the logs to an external logging server](/docs/containers?topic=containers-health#logging). By default, all storage classes are set to <strong>false</strong> to disable this logging feature. </td>
   </tr>
   <tr>
   <td><code>ibm.io/debug-level</code></td>
   <td>The logging level that is set by the {{site.data.keyword.cos_full_notm}} plug-in. All storage classes are set up with the <strong>WARN</strong> logging level. </td>
   </tr>
   <tr>
   <td><code>ibm.io/iam-endpoint</code></td>
   <td>The API endpoint for {{site.data.keyword.cloud_notm}} Identity and Access Management (IAM). </td>
   </tr>
   <tr>
   <td><code>ibm.io/kernel-cache</code></td>
   <td>Enable or disable the kernel buffer cache for the volume mount point. If enabled, data that is read from {{site.data.keyword.cos_full_notm}} is stored in the kernel cache to ensure fast read access to your data. If disabled, data is not cached and always read from {{site.data.keyword.cos_full_notm}}. Kernel cache is enabled for <code>standard</code> and <code>flex</code> storage classes, and disabled for <code>cold</code> and <code>vault</code> storage classes. </td>
   </tr>
   <tr>
   <td><code>ibm.io/multireq-max</code></td>
   <td>The maximum number of parallel requests that can be sent to the {{site.data.keyword.cos_full_notm}} service instance to list files in a single directory. All storage classes are set up with a maximum of 20 parallel requests.  </td>
   </tr>
   <tr>
   <td><code>ibm.io/object-store-endpoint</code></td>
   <td>The API endpoint to use to access the bucket in your {{site.data.keyword.cos_full_notm}} service instance. The endpoint is automatically set based on the region of your cluster. **Note**: If you want to access an existing bucket that is located in a different region than the one where your cluster is in, you must create a [custom storage class](/docs/containers?topic=containers-kube_concepts#customized_storageclass) and use the API endpoint for your bucket.</td>
   </tr>
   <tr>
   <td><code>ibm.io/object-store-storage-class</code></td>
   <td>The name of the storage class. </td>
   </tr>
   <tr>
   <td><code>ibm.io/parallel-count</code></td>
   <td>The maximum number of parallel requests that can be sent to the {{site.data.keyword.cos_full_notm}} service instance for a single read or write operation. Storage classes with <code>perf</code> in their name are set up with a maximum of 20 parallel requests. Storage classes without <code>perf</code> are set up with two parallel requests by default.  </td>
   </tr>
   <tr>
   <td><code>ibm.io/s3fs-fuse-retry-count</code></td>
   <td>The maximum number of retries for a read or write operation before the operation is considered unsuccessful. All storage classes are set up with a maximum of five retries.  </td>
   </tr>
   <tr>
   <td><code>ibm.io/stat-cache-size</code></td>
   <td>The maximum number of records that are kept in the {{site.data.keyword.cos_full_notm}} metadata cache. Every record can take up to 0.5 kilobytes. All storage classes set the maximum number of records to 100000 by default. </td>
   </tr>
   <tr>
   <td><code>ibm.io/tls-cipher-suite</code></td>
   <td>The TLS cipher suite that must be used when a connection to {{site.data.keyword.cos_full_notm}} is established via the HTTPS endpoint. The value for the cipher suite must follow the [OpenSSL format ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.openssl.org/docs/man1.0.2/apps/ciphers.html). If your worker nodes run an Ubuntu operating system, your storage classes are set up to use the <strong><code>AESGCM</code></strong> cipher suite by default. For worker nodes that run a Red Hat operating system, the <strong><code>ecdhe_rsa_aes_128_gcm_sha_256</code></strong> cipher suite is used by default.    </td>
   </tr>
   </tbody>
   </table>

   For more information about each storage class, see the [storage class reference](#cos_storageclass_reference). If you want to change any of the pre-set values, create your own [customized storage class](/docs/containers?topic=containers-kube_concepts#customized_storageclass).
   {: tip}

5. Decide on a name for your bucket. The name of a bucket must be unique in {{site.data.keyword.cos_full_notm}}. You can also choose to automatically create a name for your bucket by the {{site.data.keyword.cos_full_notm}} plug-in. To organize data in a bucket, you can create subdirectories.

   The storage class that you chose earlier determines the pricing for the entire bucket. You cannot define different storage classes for subdirectories. If you want to store data with different access requirements, consider creating multiple buckets by using multiple PVCs.
   {: note}

6. Choose if you want to keep your data and the bucket after the cluster or the persistent volume claim (PVC) is deleted. When you delete the PVC, the PV is always deleted. You can choose if you want to also automatically delete the data and the bucket when you delete the PVC. Your {{site.data.keyword.cos_full_notm}} service instance is independent from the retention policy that you select for your data and is never removed when you delete a PVC.

Now that you decided on the configuration that you want, you are ready to [create a PVC](#add_cos) to provision {{site.data.keyword.cos_full_notm}}.

## Adding object storage to apps
{: #add_cos}

Create a persistent volume claim (PVC) to provision {{site.data.keyword.cos_full_notm}} for your cluster.
{: shortdesc}

Depending on the settings that you choose in your PVC, you can provision {{site.data.keyword.cos_full_notm}} in the following ways:
- [Dynamic provisioning](/docs/containers?topic=containers-kube_concepts#dynamic_provisioning): When you create the PVC, the matching persistent volume (PV) and the bucket in your {{site.data.keyword.cos_full_notm}} service instance are automatically created.
- [Static provisioning](/docs/containers?topic=containers-kube_concepts#static_provisioning): You can reference an existing bucket in your {{site.data.keyword.cos_full_notm}} service instance in your PVC. When you create the PVC, only the matching PV is automatically created and linked to your existing bucket in {{site.data.keyword.cos_full_notm}}.

Before you begin:
- [Create and prepare your {{site.data.keyword.cos_full_notm}} service instance](#create_cos_service).
- [Create a secret to store your {{site.data.keyword.cos_full_notm}} service credentials](#create_cos_secret).
- [Decide on the configuration for your {{site.data.keyword.cos_full_notm}}](#configure_cos).

To add {{site.data.keyword.cos_full_notm}} to your cluster:

1. Create a configuration file to define your persistent volume claim (PVC).
   ```
   kind: PersistentVolumeClaim
   apiVersion: v1
   metadata:
     name: <pvc_name>
     namespace: <namespace>
     annotations:
       ibm.io/auto-create-bucket: "<true_or_false>"
       ibm.io/auto-delete-bucket: "<true_or_false>"
       ibm.io/bucket: "<bucket_name>"
       ibm.io/object-path: "<bucket_subdirectory>"
       ibm.io/secret-name: "<secret_name>"
       ibm.io/endpoint: "https://<s3fs_service_endpoint>"
   spec:
     accessModes:
       - ReadWriteOnce
     resources:
       requests:
         storage: 8Gi # Enter a fictitious value
     storageClassName: <storage_class>
   ```
   {: codeblock}

   <table>
   <caption>Understanding the YAML file components</caption>
   <thead>
   <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the YAML file components</th>
   </thead>
   <tbody>
   <tr>
   <td><code>metadata.name</code></td>
   <td>Enter the name of the PVC.</td>
   </tr>
   <tr>
   <td><code>metadata.namespace</code></td>
   <td>Enter the namespace where you want to create the PVC. The PVC must be created in the same namespace where you created the Kubernetes secret for your {{site.data.keyword.cos_full_notm}} service credentials and where you want to run your pod. </td>
   </tr>
   <tr>
   <td><code>ibm.io/auto-create-bucket</code></td>
   <td>Choose between the following options: <ul><li><strong>true</strong>: When you create the PVC, the PV and the bucket in your {{site.data.keyword.cos_full_notm}} service instance are automatically created. Choose this option to create a new bucket in your {{site.data.keyword.cos_full_notm}} service instance. </li><li><strong>false</strong>: Choose this option if you want to access data in an existing bucket. When you create the PVC, the PV is automatically created and linked to the bucket that you specify in <code>ibm.io/bucket</code>.</td>
   </tr>
   <tr>
   <td><code>ibm.io/auto-delete-bucket</code></td>
   <td>Choose between the following options: <ul><li><strong>true</strong>: Your data, the bucket, and the PV is automatically removed when you delete the PVC. Your {{site.data.keyword.cos_full_notm}} service instance remains and is not deleted. If you choose to set this option to <strong>true</strong>, then you must set <code>ibm.io/auto-create-bucket: true</code> and <code>ibm.io/bucket: ""</code> so that your bucket is automatically created with a name with the format <code>tmp-s3fs-xxxx</code>. </li><li><strong>false</strong>: When you delete the PVC, the PV is deleted automatically, but your data and the bucket in your {{site.data.keyword.cos_full_notm}} service instance remain. To access your data, you must create a new PVC with the name of your existing bucket. </li></ul>
   <tr>
   <td><code>ibm.io/bucket</code></td>
   <td>Choose between the following options: <ul><li>If <code>ibm.io/auto-create-bucket</code> is set to <strong>true</strong>: Enter the name of the bucket that you want to create in {{site.data.keyword.cos_full_notm}}. If in addition <code>ibm.io/auto-delete-bucket</code> is set to <strong>true</strong>, you must leave this field blank to automatically assign your bucket a name with the format <code>tmp-s3fs-xxxx</code>. The name must be unique in {{site.data.keyword.cos_full_notm}}. </li><li>If <code>ibm.io/auto-create-bucket</code> is set to <strong>false</strong>: Enter the name of the existing bucket that you want to access in the cluster. </li></ul> </td>
   </tr>
   <tr>
   <td><code>ibm.io/object-path</code></td>
   <td>Optional: Enter the name of the existing subdirectory in your bucket that you want to mount. Use this option if you want to mount a subdirectory only and not the entire bucket. To mount a subdirectory, you must set <code>ibm.io/auto-create-bucket: "false"</code> and provide the name of the bucket in <code>ibm.io/bucket</code>. </li></ul> </td>
   </tr>
   <tr>
   <td><code>ibm.io/secret-name</code></td>
   <td>Enter the name of the secret that holds the {{site.data.keyword.cos_full_notm}} credentials that you created earlier. </td>
   </tr>
   <tr>
  <td><code>ibm.io/endpoint</code></td>
  <td>If you created your {{site.data.keyword.cos_full_notm}} service instance in a location that is different from your cluster, enter the private or public service endpoint of your {{site.data.keyword.cos_full_notm}} service instance that you want to use. For an overview of available service endpoints, see [Additional endpoint information](/docs/services/cloud-object-storage?topic=cloud-object-storage-advanced-endpoints). By default, the <code>ibmc</code> Helm plug-in automatically retrieves your cluster location and creates the storage classes by using the {{site.data.keyword.cos_full_notm}} private service endpoint that matches your cluster location. If the cluster is in one of the metro city zones, such as `dal10`, the {{site.data.keyword.cos_full_notm}} private service endpoint for the metro city, in this case Dallas, is used. To verify that the service endpoint in your storage classes matches the service endpoint of your service instance, run `kubectl describe storageclass <storageclassname>`. Make sure that you enter your service endpoint in the format `https://<s3fs_private_service_endpoint>` for private service endpoints, or `http://<s3fs_public_service_endpoint>` for public service endpoints. If the service endpoint in your storage class matches the service endpoint of your {{site.data.keyword.cos_full_notm}} service instance, do not include the <code>ibm.io/endpoint</code> option in your PVC YAML file. </td>
  </tr>
   <tr>
   <td><code>resources.requests.storage</code></td>
   <td>A fictitious size for your {{site.data.keyword.cos_full_notm}} bucket in gigabytes. The size is required by Kubernetes, but not respected in {{site.data.keyword.cos_full_notm}}. You can enter any size that you want. The actual space that you use in {{site.data.keyword.cos_full_notm}} might be different and is billed based on the [pricing table ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/cloud-computing/bluemix/pricing-object-storage#s3api). </td>
   </tr>
   <tr>
   <td><code>spec.storageClassName</code></td>
   <td>Choose between the following options: <ul><li>If <code>ibm.io/auto-create-bucket</code> is set to <strong>true</strong>: Enter the storage class that you want to use for your new bucket. </li><li>If <code>ibm.io/auto-create-bucket</code> is set to <strong>false</strong>: Enter the storage class that you used to create your existing bucket. </br></br>If you manually created the bucket in your {{site.data.keyword.cos_full_notm}} service instance or you cannot remember the storage class that you used, find your service instance in the {{site.data.keyword.Bluemix}} dashboard and review the <strong>Class</strong> and <strong>Location</strong> of your existing bucket. Then, use the appropriate [storage class](#cos_storageclass_reference). <p class="note">The {{site.data.keyword.cos_full_notm}} API endpoint that is set in your storage class is based on the region that your cluster is in. If you want to access a bucket that is located in a different region than the one where your cluster is in, you must create a [custom storage class](/docs/containers?topic=containers-kube_concepts#customized_storageclass) and use the appropriate API endpoint for your bucket.</p></li></ul>  </td>
   </tr>
   </tbody>
   </table>

2. Create the PVC.
   ```
   kubectl apply -f filepath/pvc.yaml
   ```
   {: pre}

3. Verify that your PVC is created and bound to the PV.
   ```
   kubectl get pvc
   ```
   {: pre}

   Example output:
   ```
   NAME                  STATUS    VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS                     AGE
   s3fs-test-pvc         Bound     pvc-b38b30f9-1234-11e8-ad2b-t910456jbe12   8Gi        RWO            ibmc-s3fs-standard-cross-region  1h
   ```
   {: screen}

4. Optional: If you plan to access your data with a non-root user, or added files to an existing {{site.data.keyword.cos_full_notm}} bucket by using the console or the API directly, make sure that the [files have the correct permission](/docs/containers?topic=containers-cs_troubleshoot_storage#cos_nonroot_access) assigned so that your app can successfully read and update the files as needed.

4.  {: #cos_app_volume_mount}To mount the PV to your deployment, create a configuration `.yaml` file and specify the PVC that binds the PV.

    ```
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: <deployment_name>
      labels:
        app: <deployment_label>
    spec:
      selector:
        matchLabels:
          app: <app_name>
      template:
        metadata:
          labels:
            app: <app_name>
        spec:
          containers:
          - image: <image_name>
            name: <container_name>
            securityContext:
              runAsUser: <non_root_user>
              fsGroup: <non_root_user> #only applicable for clusters that run Kubernetes version 1.13 or later
            volumeMounts:
            - name: <volume_name>
              mountPath: /<file_path>
          volumes:
          - name: <volume_name>
            persistentVolumeClaim:
              claimName: <pvc_name>
    ```
    {: codeblock}

    <table>
    <caption>Understanding the YAML file components</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the YAML file components</th>
    </thead>
    <tbody>
        <tr>
    <td><code>metadata.labels.app</code></td>
    <td>A label for the deployment.</td>
      </tr>
      <tr>
        <td><code>spec.selector.matchLabels.app</code> <br/> <code>spec.template.metadata.labels.app</code></td>
        <td>A label for your app.</td>
      </tr>
    <tr>
    <td><code>template.metadata.labels.app</code></td>
    <td>A label for the deployment.</td>
      </tr>
    <tr>
    <td><code>spec.containers.image</code></td>
    <td>The name of the image that you want to use. To list available images in your {{site.data.keyword.registryshort_notm}} account, run `ibmcloud cr image-list`.</td>
    </tr>
    <tr>
    <td><code>spec.containers.name</code></td>
    <td>The name of the container that you want to deploy to your cluster.</td>
    </tr>
    <tr>
    <td><code>spec.containers.securityContext.runAsUser</code></td>
    <td>Optional: To run the app with a non-root user in a cluster that runs Kubernetes version 1.12 or earlier, specify the [security context ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/) for your pod by defining the non-root user without setting the `fsGroup` in your deployment YAML at the same time. Setting `fsGroup` triggers the {{site.data.keyword.cos_full_notm}} plug-in to update the group permissions for all files in a bucket when the pod is deployed. Updating the permissions is a write operation and impacts performance. Depending on how many files you have, updating the permissions might prevent your pod from coming up and getting into a <code>Running</code> state. </br></br>If you have a cluster that runs Kubernetes version 1.13 or later and the {{site.data.keyword.cloud_notm}} Object Storage plug-in version 1.0.4 or later, you can change the owner of the s3fs mount point. To change the owner, specify the security context by setting `runAsUser` and `fsGroup` to the same non-root user ID that you want to own the s3fs mount point. If these two values do not match, the mount point is automatically owned by the `root` user.  </td>
    </tr>
    <tr>
    <td><code>spec.containers.volumeMounts.mountPath</code></td>
    <td>The absolute path of the directory to where the volume is mounted inside the container. If you want to share a volume between different apps, you can specify [volume sub paths ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/concepts/storage/volumes/#using-subpath) for each of your apps.</td>
    </tr>
    <tr>
    <td><code>spec.containers.volumeMounts.name</code></td>
    <td>The name of the volume to mount to your pod.</td>
    </tr>
    <tr>
    <td><code>volumes.name</code></td>
    <td>The name of the volume to mount to your pod. Typically this name is the same as <code>volumeMounts/name</code>.</td>
    </tr>
    <tr>
    <td><code>volumes.persistentVolumeClaim.claimName</code></td>
    <td>The name of the PVC that binds the PV that you want to use. </td>
    </tr>
    </tbody></table>

5.  Create the deployment.
     ```
     kubectl apply -f <local_yaml_path>
     ```
     {: pre}

6.  Verify that the PV is successfully mounted.

     ```
     kubectl describe deployment <deployment_name>
     ```
     {: pre}

     The mount point is in the **Volume Mounts** field and the volume is in the **Volumes** field.

     ```
      Volume Mounts:
           /var/run/secrets/kubernetes.io/serviceaccount from default-token-tqp61 (ro)
           /volumemount from myvol (rw)
     ...
     Volumes:
       myvol:
         Type:	PersistentVolumeClaim (a reference to a PersistentVolumeClaim in the same namespace)
         ClaimName:	mypvc
         ReadOnly:	false
     ```
     {: screen}

7. Verify that you can write data to your {{site.data.keyword.cos_full_notm}} service instance.
   1. Log in to the pod that mounts your PV.
      ```
      kubectl exec <pod_name> -it bash
      ```
      {: pre}

   2. Navigate to your volume mount path that you defined in your app deployment.
   3. Create a text file.
      ```
      echo "This is a test" > test.txt
      ```
      {: pre}

   4. From the {{site.data.keyword.Bluemix}} dashboard, navigate to your {{site.data.keyword.cos_full_notm}} service instance.
   5. From the menu, select **Buckets**.
   6. Open your bucket, and verify that you can see the `test.txt` that you created.


## Using object storage in a stateful set
{: #cos_statefulset}

If you have a stateful app such as a database, you can create stateful sets that use {{site.data.keyword.cos_full_notm}} to store your app's data. Alternatively, you can use an {{site.data.keyword.cloud_notm}} database-as-a-service, such as {{site.data.keyword.cloudant_short_notm}} and store your data in the cloud.
{: shortdesc}

Before you begin:
- [Create and prepare your {{site.data.keyword.cos_full_notm}} service instance](#create_cos_service).
- [Create a secret to store your {{site.data.keyword.cos_full_notm}} service credentials](#create_cos_secret).
- [Decide on the configuration for your {{site.data.keyword.cos_full_notm}}](#configure_cos).

To deploy a stateful set that uses object storage:

1. Create a configuration file for your stateful set and the service that you use to expose the stateful set. The following examples show how to deploy NGINX as a stateful set with three replicas, each replica with a separate bucket or sharing the same bucket.

   **Example to create a stateful set with three replicas, with each replica using a separate bucket**:
   ```
   apiVersion: v1
   kind: Service
   metadata:
     name: nginx-v01
     namespace: default
     labels:
       app: nginx-v01 # must match spec.template.metadata.labels and spec.selector.matchLabels in stateful set YAML
   spec:
     ports:
     - port: 80
       name: web
     clusterIP: None
     selector:
       app: nginx-v01 # must match spec.template.metadata.labels and spec.selector.matchLabels in stateful set YAML
   ---
   apiVersion: apps/v1
   kind: StatefulSet
   metadata:
     name: web-v01
     namespace: default
   spec:
     selector:
       matchLabels:
         app: nginx-v01 # must match spec.template.metadata.labels in stateful set YAML and metadata.labels in service YAML
     serviceName: "nginx-v01"
     replicas: 3
     template:
       metadata:
         labels:
           app: nginx-v01 # must match spec.selector.matchLabels in stateful set YAML and metadata.labels in service YAML
       spec:
         terminationGracePeriodSeconds: 10
         containers:
         - name: nginx
           image: k8s.gcr.io/nginx-slim:0.8
           ports:
           - containerPort: 80
             name: web
           volumeMounts:
           - name: mypvc
             mountPath: /usr/share/nginx/html
     volumeClaimTemplates:
     - metadata:
         name: mypvc
         annotations:
           ibm.io/auto-create-bucket: "true"
           ibm.io/auto-delete-bucket: "true"
           ibm.io/bucket: ""
           ibm.io/secret-name: mysecret
           volume.beta.kubernetes.io/storage-class: ibmc-s3fs-standard-perf-cross-region
           volume.beta.kubernetes.io/storage-provisioner: ibm.io/ibmc-s3fs
       spec:
         accessModes: [ "ReadWriteOnce" ]
         storageClassName: "ibmc-s3fs-standard-perf-cross-region"
         resources:
           requests:
             storage: 1Gi
   ```
   {: codeblock}

   **Example to create a stateful set with three replicas that share the same bucket `mybucket`**:
   ```
   apiVersion: v1
   kind: Service
   metadata:
     name: nginx-v01
     namespace: default
     labels:
       app: nginx-v01 # must match spec.template.metadata.labels and spec.selector.matchLabels in stateful set YAML
   spec:
     ports:
     - port: 80
       name: web
     clusterIP: None
     selector:
       app: nginx-v01 # must match spec.template.metadata.labels and spec.selector.matchLabels in stateful set YAML
   ---
   apiVersion: apps/v1
   kind: StatefulSet
   metadata:
     name: web-v01
     namespace: default
   spec:
     selector:
       matchLabels:
         app: nginx-v01 # must match spec.template.metadata.labels in stateful set YAML and metadata.labels in service YAML
     serviceName: "nginx-v01"
     replicas: 3
     template:
       metadata:
         labels:
           app: nginx-v01 # must match spec.selector.matchLabels in stateful set YAML and metadata.labels in service YAML
       spec:
         terminationGracePeriodSeconds: 10
         containers:
         - name: nginx
           image: k8s.gcr.io/nginx-slim:0.8
           ports:
           - containerPort: 80
             name: web
           volumeMounts:
           - name: mypvc
             mountPath: /usr/share/nginx/html
     volumeClaimTemplates:
     - metadata:
         name: mypvc
         annotations:
           ibm.io/auto-create-bucket: "false"
           ibm.io/auto-delete-bucket: "false"
           ibm.io/bucket: mybucket
           ibm.io/secret-name: mysecret
           volume.beta.kubernetes.io/storage-class: ibmc-s3fs-standard-perf-cross-region
           volume.beta.kubernetes.io/storage-provisioner: ibm.io/ibmc-s3fs
       spec:
         accessModes: [ "ReadOnlyMany" ]
         storageClassName: "ibmc-s3fs-standard-perf-cross-region"
         resources:
           requests:
             storage: 1Gi
   ```
   {: codeblock}


   <table>
    <caption>Understanding the stateful set YAML file components</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the stateful set YAML file components</th>
    </thead>
    <tbody>
    <tr>
    <td style="text-align:left"><code>metadata.name</code></td>
    <td style="text-align:left">Enter a name for your stateful set. The name that you enter is used to create the name for your PVC in the format: <code>&lt;volume_name&gt;-&lt;statefulset_name&gt;-&lt;replica_number&gt;</code>. </td>
    </tr>
    <tr>
    <td style="text-align:left"><code>spec.serviceName</code></td>
    <td style="text-align:left">Enter the name of the service that you want to use to expose your stateful set. </td>
    </tr>
    <tr>
    <td style="text-align:left"><code>spec.replicas</code></td>
    <td style="text-align:left">Enter the number of replicas for your stateful set. </td>
    </tr>
    <tr>
    <td style="text-align:left"><code>spec.selector.matchLabels</code></td>
    <td style="text-align:left">Enter all labels that you want to include in your stateful set and your PVC. Labels that you include in the <code>volumeClaimTemplates</code> of your stateful set are not recognized by Kubernetes. Instead, you must define these labels in the <code>spec.selector.matchLabels</code> and <code>spec.template.metadata.labels</code> section of your stateful set YAML. To make sure that all your stateful set replicas are included into the load balancing of your service, include the same label that you used in the <code>spec.selector</code> section of your service YAML. </td>
    </tr>
    <tr>
    <td style="text-align:left"><code>spec.template.metadata.labels</code></td>
    <td style="text-align:left">Enter the same labels that you added to the <code>spec.selector.matchLabels</code> section of your stateful set YAML. </td>
    </tr>
    <tr>
    <td><code>spec.template.spec.</code></br><code>terminationGracePeriodSeconds</code></td>
    <td>Enter the number of seconds to give the <code>kubelet</code> to gracefully terminate the pod that runs your stateful set replica. For more information, see [Delete Pods ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/tasks/run-application/force-delete-stateful-set-pod/#delete-pods). </td>
    </tr>
    <tr>
    <td style="text-align:left"><code>spec.volumeClaimTemplates.</code></br><code>metadata.name</code></td>
    <td style="text-align:left">Enter a name for your volume. Use the same name that you defined in the <code>spec.containers.volumeMount.name</code> section. The name that you enter here is used to create the name for your PVC in the format: <code>&lt;volume_name&gt;-&lt;statefulset_name&gt;-&lt;replica_number&gt;</code>. </td>
    </tr>
    <tr>
    <td><code>spec.volumeClaimTemplates.metadata</code></br><code>annotions.ibm.io/auto-create-bucket</code></td>
    <td>Choose between the following options: <ul><li><strong>true: </strong>Choose this option to automatically create a bucket for each stateful set replica. </li><li><strong>false: </strong>Choose this option if you want to share an existing bucket across your stateful set replicas. Make sure to define the name of the bucket in the <code>spec.volumeClaimTemplates.metadata.annotions.ibm.io/bucket</code> section of your stateful set YAML.</li></ul></td>
    </tr>
    <tr>
    <td><code>spec.volumeClaimTemplates.metadata</code></br><code>annotions.ibm.io/auto-delete-bucket</code></td>
    <td>Choose between the following options: <ul><li><strong>true: </strong>Your data, the bucket, and the PV is automatically removed when you delete the PVC. Your {{site.data.keyword.cos_full_notm}} service instance remains and is not deleted. If you choose to set this option to true, then you must set <code>ibm.io/auto-create-bucket: true</code> and <code>ibm.io/bucket: ""</code> so that your bucket is automatically created with a name with the format <code>tmp-s3fs-xxxx</code>. </li><li><strong>false: </strong>When you delete the PVC, the PV is deleted automatically, but your data and the bucket in your {{site.data.keyword.cos_full_notm}} service instance remain. To access your data, you must create a new PVC with the name of your existing bucket.</li></ul></td>
    </tr>
    <tr>
    <td><code>spec.volumeClaimTemplates.metadata</code></br><code>annotions.ibm.io/bucket</code></td>
    <td>Choose between the following options: <ul><li><strong>If <code>ibm.io/auto-create-bucket</code> is set to true: </strong>Enter the name of the bucket that you want to create in {{site.data.keyword.cos_full_notm}}. If in addition <code>ibm.io/auto-delete-bucket</code> is set to <strong>true</strong>, you must leave this field blank to automatically assign your bucket a name with the format tmp-s3fs-xxxx. The name must be unique in {{site.data.keyword.cos_full_notm}}.</li><li><strong>If <code>ibm.io/auto-create-bucket</code> is set to false: </strong>Enter the name of the existing bucket that you want to access in the cluster.</li></ul></td>
    </tr>
    <tr>
    <td><code>spec.volumeClaimTemplates.metadata</code></br><code>annotions.ibm.io/secret-name</code></td>
    <td>Enter the name of the secret that holds the {{site.data.keyword.cos_full_notm}} credentials that you created earlier.</td>
    </tr>
    <tr>
    <td style="text-align:left"><code>spec.volumeClaimTemplates.metadata.</code></br><code>annotations.volume.beta.</code></br><code>kubernetes.io/storage-class</code></td>
    <td style="text-align:left">Enter the storage class that you want to use. Choose between the following options: <ul><li><strong>If <code>ibm.io/auto-create-bucket</code> is set to true: </strong>Enter the storage class that you want to use for your new bucket.</li><li><strong>If <code>ibm.io/auto-create-bucket</code> is set to false: </strong>Enter the storage class that you used to create your existing bucket. </li></ul></br>  To list existing storage classes, run <code>kubectl get storageclasses | grep s3</code>. If you do not specify a storage class, the PVC is created with the default storage class that is set in your cluster. Make sure that the default storage class uses the <code>ibm.io/ibmc-s3fs</code> provisioner so that your stateful set is provisioned with object storage.</td>
    </tr>
    <tr>
    <td style="text-align:left"><code>spec.volumeClaimTemplates.</code></br><code>spec.storageClassName</code></td>
    <td>Enter the same storage class that you entered in the <code>spec.volumeClaimTemplates.metadata.annotations.volume.beta.kubernetes.io/storage-class</code> section of your stateful set YAML.  </td>
    </tr>
    <tr>
    <td style="text-align:left"><code>spec.volumeClaimTemplates.spec.</code></br><code>resource.requests.storage</code></td>
    <td>Enter a fictitious size for your {{site.data.keyword.cos_full_notm}} bucket in gigabytes. The size is required by Kubernetes, but not respected in {{site.data.keyword.cos_full_notm}}. You can enter any size that you want. The actual space that you use in {{site.data.keyword.cos_full_notm}} might be different and is billed based on the [pricing table ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/cloud-computing/bluemix/pricing-object-storage#s3api).</td>
    </tr>
    </tbody></table>


## Backing up and restoring data
{: #cos_backup_restore}

{{site.data.keyword.cos_full_notm}} is set up to provide high durability for your data so that your data is protected from being lost. You can find the SLA in the [{{site.data.keyword.cos_full_notm}} service terms ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www-03.ibm.com/software/sla/sladb.nsf/sla/bm-7857-03).
{: shortdesc}

{{site.data.keyword.cos_full_notm}} does not provide a version history for your data. If you need to maintain and access older versions of your data, you must set up your app to manage the history of data or implement alternative backup solutions. For example, you might want to store your {{site.data.keyword.cos_full_notm}} data in your on-prem database or use tapes to archive your data.
{: note}

## Storage class reference
{: #cos_storageclass_reference}

### Standard
{: #standard}

<table>
<caption>Object storage class: standard</caption>
<thead>
<th>Characteristics</th>
<th>Setting</th>
</thead>
<tbody>
<tr>
<td>Name</td>
<td><code>ibmc-s3fs-standard-cross-region</code></br><code>ibmc-s3fs-standard-perf-cross-region</code></br><code>ibmc-s3fs-standard-regional</code></br><code>ibmc-s3fs-standard-perf-regional</code></td>
</tr>
<tr>
<td>Default resiliency endpoint</td>
<td>The resiliency endpoint is automatically set based on the location that your cluster is in. For more information, see [Regions and endpoints](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-endpoints#endpoints). </td>
</tr>
<tr>
<td>Chunk size</td>
<td>Storage classes without `perf`: 16 MB</br>Storage classes with `perf`: 52 MB</td>
</tr>
<tr>
<td>Kernel cache</td>
<td>Enabled</td>
</tr>
<tr>
<td>Billing</td>
<td>Monthly</td>
</tr>
<tr>
<td>Pricing</td>
<td>[Pricing ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/cloud-computing/bluemix/pricing-object-storage#s3api)</td>
</tr>
</tbody>
</table>

### Vault
{: #Vault}

<table>
<caption>Object storage class: vault</caption>
<thead>
<th>Characteristics</th>
<th>Setting</th>
</thead>
<tbody>
<tr>
<td>Name</td>
<td><code>ibmc-s3fs-vault-cross-region</code></br><code>ibmc-s3fs-vault-regional</code></td>
</tr>
<tr>
<td>Default resiliency endpoint</td>
<td>The resiliency endpoint is automatically set based on the location that your cluster is in. For more information, see [Regions and endpoints](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-endpoints#endpoints). </td>
</tr>
<tr>
<td>Chunk size</td>
<td>16 MB</td>
</tr>
<tr>
<td>Kernel cache</td>
<td>Disabled</td>
</tr>
<tr>
<td>Billing</td>
<td>Monthly</td>
</tr>
<tr>
<td>Pricing</td>
<td>[Pricing ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/cloud-computing/bluemix/pricing-object-storage#s3api)</td>
</tr>
</tbody>
</table>

### Cold
{: #cold}

<table>
<caption>Object storage class: cold</caption>
<thead>
<th>Characteristics</th>
<th>Setting</th>
</thead>
<tbody>
<tr>
<td>Name</td>
<td><code>ibmc-s3fs-flex-cross-region</code></br><code>ibmc-s3fs-flex-perf-cross-region</code></br><code>ibmc-s3fs-flex-regional</code></br><code>ibmc-s3fs-flex-perf-regional</code></td>
</tr>
<tr>
<td>Default resiliency endpoint</td>
<td>The resiliency endpoint is automatically set based on the location that your cluster is in. For more information, see [Regions and endpoints](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-endpoints#endpoints). </td>
</tr>
<tr>
<td>Chunk size</td>
<td>16 MB</td>
</tr>
<tr>
<td>Kernel cache</td>
<td>Disabled</td>
</tr>
<tr>
<td>Billing</td>
<td>Monthly</td>
</tr>
<tr>
<td>Pricing</td>
<td>[Pricing ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/cloud-computing/bluemix/pricing-object-storage#s3api)</td>
</tr>
</tbody>
</table>

### Flex
{: #flex}

<table>
<caption>Object storage class: flex</caption>
<thead>
<th>Characteristics</th>
<th>Setting</th>
</thead>
<tbody>
<tr>
<td>Name</td>
<td><code>ibmc-s3fs-cold-cross-region</code></br><code>ibmc-s3fs-flex-perf-cross-region</code></br><code>ibmc-s3fs-cold-regional</code></br><code>ibmc-s3fs-flex-perf-regional</code></td>
</tr>
<tr>
<td>Default resiliency endpoint</td>
<td>The resiliency endpoint is automatically set based on the location that your cluster is in. For more information, see [Regions and endpoints](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-endpoints#endpoints). </td>
</tr>
<tr>
<td>Chunk size</td>
<td>Storage classes without `perf`: 16 MB</br>Storage classes with `perf`: 52 MB</td>
</tr>
<tr>
<td>Kernel cache</td>
<td>Enabled</td>
</tr>
<tr>
<td>Billing</td>
<td>Monthly</td>
</tr>
<tr>
<td>Pricing</td>
<td>[Pricing ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/cloud-computing/bluemix/pricing-object-storage#s3api)</td>
</tr>
</tbody>
</table>


