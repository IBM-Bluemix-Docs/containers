---

copyright:
  years: 2014, 2018
lastupdated: "2018-09-25"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}





# Storing data on IBM Block Storage for IBM Cloud
{: #block_storage}


## Installing the {{site.data.keyword.Bluemix_notm}} Block Storage plug-in in your cluster
{: #install_block}

Install the {{site.data.keyword.Bluemix_notm}} Block Storage plug-in with a Helm chart to set up pre-defined storage classes for block storage. You can use these storage classes to create a PVC to provision block storage for your apps.
{: shortdesc}

Before you begin, [target your CLI](cs_cli_install.html#cs_cli_configure) to the cluster where you want to install the {{site.data.keyword.Bluemix_notm}} Block Storage plug-in.


1. Follow the [instructions](cs_integrations.html#helm) to install the Helm client on your local machine, install the Helm server (tiller) in your cluster, and add the {{site.data.keyword.Bluemix_notm}} Helm chart repository to the cluster where you want to use the {{site.data.keyword.Bluemix_notm}} Block Storage plug-in.

   **Important:** If you use Helm version 2.9 or higher, make sure that you installed tiller with a [service account](cs_integrations.html#helm).

2. Update the Helm repo to retrieve the latest version of all Helm charts in this repo.
   ```
   helm repo update
   ```
   {: pre}

3. Install the {{site.data.keyword.Bluemix_notm}} Block Storage plug-in. When you install the plug-in, pre-defined block storage classes are added to your cluster.
   ```
   helm install ibm/ibmcloud-block-storage-plugin
   ```
   {: pre}

   Example output:
   ```
   NAME:   bald-olm
   LAST DEPLOYED: Wed Apr 18 10:02:55 2018
   NAMESPACE: default
   STATUS: DEPLOYED

   RESOURCES:
   ==> v1beta1/DaemonSet
   NAME                           DESIRED  CURRENT  READY  UP-TO-DATE  AVAILABLE  NODE SELECTOR  AGE
   ibmcloud-block-storage-driver  0        0        0      0           0          <none>         0s

   ==> v1beta1/Deployment
   NAME                           DESIRED  CURRENT  UP-TO-DATE  AVAILABLE  AGE
   ibmcloud-block-storage-plugin  1        0        0           0          0s

   ==> v1/StorageClass
   NAME                      PROVISIONER        AGE
   ibmc-block-bronze         ibm.io/ibmc-block  0s
   ibmc-block-custom         ibm.io/ibmc-block  0s
   ibmc-block-gold           ibm.io/ibmc-block  0s
   ibmc-block-retain-bronze  ibm.io/ibmc-block  0s
   ibmc-block-retain-custom  ibm.io/ibmc-block  0s
   ibmc-block-retain-gold    ibm.io/ibmc-block  0s
   ibmc-block-retain-silver  ibm.io/ibmc-block  0s
   ibmc-block-silver         ibm.io/ibmc-block  0s

   ==> v1/ServiceAccount
   NAME                           SECRETS  AGE
   ibmcloud-block-storage-plugin  1        0s

   ==> v1beta1/ClusterRole
   NAME                           AGE
   ibmcloud-block-storage-plugin  0s

   ==> v1beta1/ClusterRoleBinding
   NAME                           AGE
   ibmcloud-block-storage-plugin  0s

   NOTES:
   Thank you for installing: ibmcloud-block-storage-plugin.   Your release is named: bald-olm
   ```
   {: screen}

4. Verify that the installation was successful.
   ```
   kubectl get pod -n kube-system | grep block
   ```
   {: pre}

   Example output:
   ```
   ibmcloud-block-storage-driver-kh4mt                              1/1       Running   0          27d       10.118.98.19   10.118.98.19
   ibmcloud-block-storage-plugin-58c5f9dc86-pbl4t                   1/1       Running   0          14d       172.21.0.204   10.118.98.19
   ```
   {: screen}

   The installation is successful when you see one `ibmcloud-block-storage-plugin` pod and one or more `ibmcloud-block-storage-driver` pods. The number of `ibmcloud-block-storage-driver` pods equals the number of worker nodes in your cluster. All pods must be in a **Running** state.

5. Verify that the storage classes for block storage were added to your cluster.
   ```
   kubectl get storageclasses | grep block
   ```
   {: pre}

   Example output:
   ```
   ibmc-block-bronze            ibm.io/ibmc-block
   ibmc-block-custom            ibm.io/ibmc-block
   ibmc-block-gold              ibm.io/ibmc-block
   ibmc-block-retain-bronze     ibm.io/ibmc-block
   ibmc-block-retain-custom     ibm.io/ibmc-block
   ibmc-block-retain-gold       ibm.io/ibmc-block
   ibmc-block-retain-silver     ibm.io/ibmc-block
   ibmc-block-silver            ibm.io/ibmc-block
   ```
   {: screen}

6. Repeat these steps for every cluster where you want to provision block storage.

You can now continue to [create a PVC](#add_block) to provision block storage for your app.


### Updating the {{site.data.keyword.Bluemix_notm}} Block Storage plug-in
You can upgrade the existing {{site.data.keyword.Bluemix_notm}} Block Storage plug-in to the latest version.
{: shortdesc}

Before you begin, [target your CLI](cs_cli_install.html#cs_cli_configure) to the cluster.

1. Update the Helm repo to retrieve the latest version of all helm charts in this repo.
   ```
   helm repo update
   ```
   {: pre}

2. Optional: Download the latest Helm chart to your local machine. Then, unzip the package and review the `release.md` file to find the latest release information.
   ```
   helm fetch ibm/ibmcloud-block-storage-plugin
   ```
   {: pre}

3. Find the name of the block storage Helm chart that you installed in your cluster.
   ```
   helm ls | grep ibmcloud-block-storage-plugin
   ```
   {: pre}

   Example output:
   ```
   myhelmchart 	1       	Mon Sep 18 15:31:40 2017	DEPLOYED	ibmcloud-block-storage-plugin-0.1.0	default
   ```
   {: screen}

4. Upgrade the {{site.data.keyword.Bluemix_notm}} Block Storage plug-in to the latest version.
   ```
   helm upgrade --force --recreate-pods <helm_chart_name>  ibm/ibmcloud-block-storage-plugin
   ```
   {: pre}

5. Optional: When you update the plug-in, the `default` storage class is unset. If you want to set the default storage class to a storage class of your choice, run the following command.
   ```
   kubectl patch storageclass <storageclass> -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'
   ```
   {: pre}


### Removing the {{site.data.keyword.Bluemix_notm}} Block Storage plug-in
If you do not want to provision and use {{site.data.keyword.Bluemix_notm}} Block Storage in your cluster, you can uninstall the Helm chart.
{: shortdesc}

**Note:** Removing the plug-in does not remove existing PVCs, PVs, or data. When you remove the plug-in, all the related pods and daemon sets are removed from your cluster. You cannot provision new block storage for your cluster or use existing block storage PVCs and PVs after you remove the plug-in.

Before you begin:
- [Target your CLI](cs_cli_install.html#cs_cli_configure) to the cluster.
- Make sure that you do not have any PVCs or PVs in your cluster that use block storage.

To remove the plug-in:

1. Find the name of the block storage Helm chart that you installed in your cluster.
   ```
   helm ls | grep ibmcloud-block-storage-plugin
   ```
   {: pre}

   Example output:
   ```
   myhelmchart 	1       	Mon Sep 18 15:31:40 2017	DEPLOYED	ibmcloud-block-storage-plugin-0.1.0	default
   ```
   {: screen}

2. Delete the {{site.data.keyword.Bluemix_notm}} Block Storage plug-in.
   ```
   helm delete <helm_chart_name>
   ```
   {: pre}

3. Verify that the block storage pods are removed.
   ```
   kubectl get pod -n kube-system | grep ibmcloud-block-storage-plugin
   ```
   {: pre}
   The removal of the pods is successful if no pods are displayed in your CLI output.

4. Verify that the block storage storage classes are removed.
   ```
   kubectl get storageclasses | grep block
   ```
   {: pre}
   The removal of the storage classes is successful if no storage classes are displayed in your CLI output.

<br />



## Deciding on the block storage configuration
{: #predefined_storageclass}

{{site.data.keyword.containerlong}} provides pre-defined storage classes for block storage that you can use to provision block storage with a specific configuration.
{: shortdesc}

Every storage class specifies the type of block storage that you provision, including available size, IOPS, file system, and the retention policy.  

**Important:** Make sure to choose your storage configuration carefully to have enough capacity to store your data. After you provision a specific type of storage by using a storage class, you cannot change the size, type, IOPS, or retention policy for the storage device. If you need more storage or storage with a different configuration, you must [create a new storage instance and copy the data](cs_storage_basics.html#update_storageclass) from the old storage instance to your new one.

1. List available storage classes in {{site.data.keyword.containerlong}}.
    ```
    kubectl get storageclasses | grep block
    ```
    {: pre}

    Example output:
    ```
    $ kubectl get storageclasses
    NAME                         TYPE
    ibmc-block-custom            ibm.io/ibmc-block
    ibmc-block-bronze            ibm.io/ibmc-block
    ibmc-block-gold              ibm.io/ibmc-block
    ibmc-block-silver            ibm.io/ibmc-block
    ibmc-block-retain-bronze     ibm.io/ibmc-block
    ibmc-block-retain-silver     ibm.io/ibmc-block
    ibmc-block-retain-gold       ibm.io/ibmc-block
    ibmc-block-retain-custom     ibm.io/ibmc-block
    ```
    {: screen}

2. Review the configuration of a storage class.
   ```
   kubectl describe storageclass <storageclass_name>
   ```
   {: pre}

   For more information about each storage class, see the [storage class reference](#storageclass_reference). If you do not find what you are looking for, consider creating your own customized storage class. To get started, check out the [customized storage class samples](#custom_storageclass).
   {: tip}

3. Choose the type of block storage that you want to provision.
   - **Bronze, silver, and gold storage classes:** These storage classes provision [Endurance storage ![External link icon](../icons/launch-glyph.svg "External link icon")](https://knowledgelayer.softlayer.com/topic/endurance-storage). Endurance storage lets you choose the size of the storage in gigabytes at predefined IOPS tiers.
   - **Custom storage class:** This storage class provisions [Performance storage ![External link icon](../icons/launch-glyph.svg "External link icon")](https://knowledgelayer.softlayer.com/topic/performance-storage). With performance storage, you have more control over the size of the storage and the IOPS.

4. Choose the size and IOPS for your block storage. The size and the number of IOPS define the total number of IOPS (input/ output operations per second) that serves as an indicator for how fast your storage is. The more total IOPS your storage has, the faster it processes read and write operations.
   - **Bronze, silver, and gold storage classes:** These storage classes come with a fixed number of IOPS per gigabyte and are provisioned on SSD hard disks. The total number of IOPS depends on the size of the storage that you choose. You can select any whole number of gigabyte within the allowed size range, such as 20 Gi, 256 Gi, or 11854 Gi. To determine the total number of IOPS, you must multiply the IOPS with the selected size. For example, if you select a 1000Gi block storage size in the silver storage class that comes with 4 IOPS per GB, your storage has a total of 4000 IOPS.  
     <table>
         <caption>Table of storage class size ranges and IOPS per gigabyte</caption>
         <thead>
         <th>Storage class</th>
         <th>IOPS per gigabyte</th>
         <th>Size range in gigabytes</th>
         </thead>
         <tbody>
         <tr>
         <td>Bronze</td>
         <td>2 IOPS/GB</td>
         <td>20-12000 Gi</td>
         </tr>
         <tr>
         <td>Silver</td>
         <td>4 IOPS/GB</td>
         <td>20-12000 Gi</td>
         </tr>
         <tr>
         <td>Gold</td>
         <td>10 IOPS/GB</td>
         <td>20-4000 Gi</td>
         </tr>
         </tbody></table>
   - **Custom storage class:** When you choose this storage class, you have more control over the size and IOPS that you want. For the size, you can select any whole number of gigabyte within the allowed size range. The size that you choose determines the IOPS range that is available to you. You can choose an IOPS that is a multiple of 100 that is in the specified range. The IOPS that you choose is static and does not scale with the size of the storage. For example, if you choose 40Gi with 100 IOPS, your total IOPS remains 100. </br></br>The IOPS to gigabyte ratio also determines the type of hard disk that is provisioned for you. For example, if you have 500Gi at 100 IOPS, your IOPS to gigabyte ratio is 0.2. Storage with a ratio of less than or equal to 0.3 is provisioned on SATA hard disks. If your ratio is greater than 0.3, then your storage is provisioned on SSD hard disks.
     <table>
         <caption>Table of custom storage class size ranges and IOPS</caption>
         <thead>
         <th>Size range in gigabytes</th>
         <th>IOPS range in multiples of 100</th>
         </thead>
         <tbody>
         <tr>
         <td>20-39 Gi</td>
         <td>100-1000 IOPS</td>
         </tr>
         <tr>
         <td>40-79 Gi</td>
         <td>100-2000 IOPS</td>
         </tr>
         <tr>
         <td>80-99 Gi</td>
         <td>100-4000 IOPS</td>
         </tr>
         <tr>
         <td>100-499 Gi</td>
         <td>100-6000 IOPS</td>
         </tr>
         <tr>
         <td>500-999 Gi</td>
         <td>100-10000 IOPS</td>
         </tr>
         <tr>
         <td>1000-1999 Gi</td>
         <td>100-20000 IOPS</td>
         </tr>
         <tr>
         <td>2000-2999 Gi</td>
         <td>200-40000 IOPS</td>
         </tr>
         <tr>
         <td>3000-3999 Gi</td>
         <td>200-48000 IOPS</td>
         </tr>
         <tr>
         <td>4000-7999 Gi</td>
         <td>300-48000 IOPS</td>
         </tr>
         <tr>
         <td>8000-9999 Gi</td>
         <td>500-48000 IOPS</td>
         </tr>
         <tr>
         <td>10000-12000 Gi</td>
         <td>1000-48000 IOPS</td>
         </tr>
         </tbody></table>

5. Choose if you want to keep your data after the cluster or the persistent volume claim (PVC) is deleted.
   - If you want to keep your data, then choose a `retain` storage class. When you delete the PVC, only the PVC is deleted. The PV, the physical storage device in your IBM Cloud infrastructure (SoftLayer) account, and your data still exist. To reclaim the storage and use it in your cluster again, you must remove the PV and follow the steps for [using existing block storage](#existing_block).
   - If you want the PV, the data, and your physical block storage device to be deleted when you delete the PVC, choose a storage class without `retain`.

6. Choose if you want to be billed hourly or monthly. Check the [pricing ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/cloud/block-storage/pricing) for more information. By default, all block storage devices are provisioned with an hourly billing type.

<br />



## Adding block storage to apps
{: #add_block}

Create a persistent volume claim (PVC) to [dynamically provision](cs_storage_basics.html#dynamic_provisioning) block storage for your cluster. Dynamic provisioning automatically creates the matching persistent volume (PV) and orders the actual storage device in your IBM Cloud infrastructure (SoftLayer) account.
{:shortdesc}

**Important**: Block storage comes with a `ReadWriteOnce` access mode. You can mount it to only one pod on one worker node in the cluster at a time.

Before you begin:
- If you have a firewall, [allow egress access](cs_firewall.html#pvc) for the IBM Cloud infrastructure (SoftLayer) IP ranges of the zones that your clusters are in so that you can create PVCs.
- Install the [{{site.data.keyword.Bluemix_notm}} block storage plug-in](#install_block).
- [Decide on a pre-defined storage class](#predefined_storageclass) or create a [customized storage class](#custom_storageclass).

To add block storage:

1.  Create a configuration file to define your persistent volume claim (PVC) and save the configuration as a `.yaml` file.

    -  **Example for bronze, silver, gold storage classes**:
       The following `.yaml` file creates a claim that is named `mypvc` of the `"ibmc-block-silver"` storage class, billed `"hourly"`, with a gigabyte size of `24Gi`.

       ```
       apiVersion: v1
       kind: PersistentVolumeClaim
       metadata:
         name: mypvc
         annotations:
           volume.beta.kubernetes.io/storage-class: "ibmc-block-silver"
         labels:
           billingType: "hourly"
	         region: us-south
           zone: dal13
       spec:
         accessModes:
           - ReadWriteOnce
         resources:
           requests:
             storage: 24Gi
       ```
       {: codeblock}

    -  **Example for using the custom storage class**:
       The following `.yaml` file creates a claim that is named `mypvc` of the storage class `ibmc-block-retain-custom`, billed `"hourly"`, with a gigabyte size of `45Gi` and IOPS of `"300"`.

       ```
       apiVersion: v1
       kind: PersistentVolumeClaim
       metadata:
         name: mypvc
         annotations:
           volume.beta.kubernetes.io/storage-class: "ibmc-block-retain-custom"
         labels:
           billingType: "hourly"
	         region: us-south
           zone: dal13
       spec:
         accessModes:
           - ReadWriteOnce
         resources:
           requests:
             storage: 45Gi
             iops: "300"
       ```
       {: codeblock}

       <table>
       <caption>Understanding the YAML file components</caption>
       <thead>
       <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the YAML file components</th>
       </thead>
       <tbody>
       <tr>
       <td><code>metadata/name</code></td>
       <td>Enter the name of the PVC.</td>
       </tr>
       <tr>
       <td><code>metadata/annotations</code></td>
       <td>The name of the storage class that you want to use to provision block storage. </br> If you do not specify a storage class, the PV is created with the default storage class <code>ibmc-file-bronze</code><p>**Tip:** If you want to change the default storage class, run <code>kubectl patch storageclass &lt;storageclass&gt; -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'</code> and replace <code>&lt;storageclass&gt;</code> with the name of the storage class.</p></td>
       </tr>
       <tr>
         <td><code>metadata/labels/billingType</code></td>
         <td>Specify the frequency for which your storage bill is calculated, "monthly" or "hourly". The default is "hourly".</td>
       </tr>
       <tr>
       <td><code>metadata/labels/region</code></td>
       <td>Specify the region where you want to provision your block storage. If you specify the region, you must also specify a zone. If you do not specify a region, or the specified region is not found, the storage is created in the same region as your cluster. </br><strong>Note:</strong> This option is supported only with the IBM Cloud Block Storage plug-in version 1.0.1 or higher. For older plug-in versions, if you have a multizone cluster, the zone in which your storage is provisioned is selected on a round-robin basis to balance volume requests evenly across all zones. If you want to specify the zone for your storage, create a [customized storage class](#multizone_yaml) first. Then, create a PVC with your customized storage class.</td>
       </tr>
       <tr>
       <td><code>metadata/labels/zone</code></td>
	<td>Specify the zone where you want to provision your block storage. If you specify the zone, you must also specify a region. If you do not specify a zone or the specified zone is not found in a multizone cluster, the zone is selected on a round-robin basis. </br><strong>Note:</strong> This option is supported only with the IBM Cloud Block Storage plug-in version 1.0.1 or higher. For older plug-in versions, if you have a multizone cluster, the zone in which your storage is provisioned is selected on a round-robin basis to balance volume requests evenly across all zones. If you want to specify the zone for your storage, create a [customized storage class](#multizone_yaml) first. Then, create a PVC with your customized storage class.</td>
	</tr>
        <tr>
        <td><code>spec/resources/requests/storage</code></td>
        <td>Enter the size of the block storage, in gigabytes (Gi). </br></br><strong>Note: </strong> After your storage is provisioned, you cannot change the size of your block storage. Make sure to specify a size that matches the amount of data that you want to store. </td>
        </tr>
        <tr>
        <td><code>spec/resources/requests/iops</code></td>
        <td>This option is available for the custom storage classes only (`ibmc-block-custom / ibmc-block-retain-custom`). Specify the total IOPS for the storage, selecting a multiple of 100 within the allowable range. If you choose an IOPS other than one that is listed, the IOPS is rounded up.</td>
        </tr>
        </tbody></table>

    If you want to use a customized storage class, create your PVC with the corresponding storage class name, a valid IOPS and size.   
    {: tip}

2.  Create the PVC.

    ```
    kubectl apply -f mypvc.yaml
    ```
    {: pre}

3.  Verify that your PVC is created and bound to the PV. This process can take a few minutes.

    ```
    kubectl describe pvc mypvc
    ```
    {: pre}

    Example output:

    ```
    Name:		mypvc
    Namespace:	default
    StorageClass:	""
    Status:		Bound
    Volume:		pvc-0d787071-3a67-11e7-aafc-eef80dd2dea2
    Labels:		<none>
    Capacity:	20Gi
    Access Modes:	RWX
    Events:
      FirstSeen	LastSeen	Count	From								SubObjectPath	Type		Reason			Message
      ---------	--------	-----	----								-------------	--------	------			-------
      3m		3m		1	{ibm.io/ibmc-block 31898035-3011-11e7-a6a4-7a08779efd33 }			Normal		Provisioning		External provisioner is provisioning volume for claim "default/my-persistent-volume-claim"
      3m		1m		10	{persistentvolume-controller }							Normal		ExternalProvisioning	cannot find provisioner "ibm.io/ibmc-block", expecting that a volume for the claim is provisioned either manually or via external software
      1m		1m		1	{ibm.io/ibmc-block 31898035-3011-11e7-a6a4-7a08779efd33 }			Normal		ProvisioningSucceeded	Successfully provisioned volume pvc-0d787071-3a67-11e7-aafc-eef80dd2dea2

    ```
    {: screen}

4.  {: #app_volume_mount}To mount the PV to your deployment, create a configuration `.yaml` file and specify the PVC that binds the PV.

    ```
    apiVersion: apps/v1beta1
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
    <td><code>metadata/labels/app</code></td>
    <td>A label for the deployment.</td>
      </tr>
      <tr>
        <td><code>spec/selector/matchLabels/app</code> <br/> <code>spec/template/metadata/labels/app</code></td>
        <td>A label for your app.</td>
      </tr>
    <tr>
    <td><code>template/metadata/labels/app</code></td>
    <td>A label for the deployment.</td>
      </tr>
    <tr>
    <td><code>spec/containers/image</code></td>
    <td>The name of the image that you want to use. To list available images in your {{site.data.keyword.registryshort_notm}} account, run `ibmcloud cr image-list`.</td>
    </tr>
    <tr>
    <td><code>spec/containers/name</code></td>
    <td>The name of the container that you want to deploy to your cluster.</td>
    </tr>
    <tr>
    <td><code>spec/containers/volumeMounts/mountPath</code></td>
    <td>The absolute path of the directory to where the volume is mounted inside the container. Data that is written to the mount path is stored under the root directory in your physical file storage instance. To create directories in your physical file storage instance, you must create subdirectories in your mount path.</td>
    </tr>
    <tr>
    <td><code>spec/containers/volumeMounts/name</code></td>
    <td>The name of the volume to mount to your pod.</td>
    </tr>
    <tr>
    <td><code>volumes/name</code></td>
    <td>The name of the volume to mount to your pod. Typically this name is the same as <code>volumeMounts/name</code>.</td>
    </tr>
    <tr>
    <td><code>volumes/persistentVolumeClaim/claimName</code></td>
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

<br />




## Using existing block storage in your cluster
{: #existing_block}

If you have an existing physical storage device that you want to use in your cluster, you can manually create the PV and PVC to [statically provision](cs_storage_basics.html#static_provisioning) the storage.

Before you can start to mount your existing storage to an app, you must retrieve all necessary information for your PV.  
{: shortdesc}

### Step 1: Retrieving the information of your existing block storage

1.  Retrieve or generate an API key for your IBM Cloud infrastructure (SoftLayer) account.
    1. Log in to the [IBM Cloud infrastructure (SoftLayer) portal ![External link icon](../icons/launch-glyph.svg "External link icon")](https://control.bluemix.net/).
    2. Select **Account**, then **Users**, and then **User List**.
    3. Find your user ID.
    4. In the **API KEY** column, click **Generate** to generate an API key or **View** to view your existing API key.
2.  Retrieve the API user name for your IBM Cloud infrastructure (SoftLayer) account.
    1. From the **User List** menu, select your user ID.
    2. In the **API Access Information** section, find your **API Username**.
3.  Log in to the IBM Cloud infrastructure CLI plug-in.
    ```
    ibmcloud sl init
    ```
    {: pre}

4.  Choose to authenticate by using the user name and API key for your IBM Cloud infrastructure (SoftLayer) account.
5.  Enter the user name and API key that you retrieved in the previous steps.
6.  List available block storage devices.
    ```
    ibmcloud sl block volume-list
    ```
    {: pre}

    Example output:
    ```
    id         username            datacenter   storage_type              capacity_gb   bytes_used   ip_addr         lunId   active_transactions
    38642141   IBM02SEL1543159-1   dal10        endurance_block_storage   20            -            169.xx.xxx.xxx   170     0
    ```
    {: screen}

7.  Note the `id`, `ip_addr`, `capacity_gb`, the `datacenter`, and `lunId` of the block storage device that you want to mount to your cluster. **Note:** To mount existing storage to a cluster, you must have a worker node in the same zone as your storage. To verify the zone of your worker node, run `ibmcloud ks workers <cluster_name_or_ID>`.

### Step 2: Creating a persistent volume (PV) and a matching persistent volume claim (PVC)

1.  Optional: If you have storage that you provisioned with a `retain` storage class, when you remove the PVC, the PV and the physical storage device are not removed. To reuse the storage in your cluster, you must remove the PV first.
    1. List existing PVs.
       ```
       kubectl get pv
       ```
       {: pre}

       Look for the PV that belongs to your persistent storage. The PV is in a `released` state.

    2. Remove the PV.
       ```
       kubectl delete pv <pv_name>
       ```
       {: pre}

    3. Verify that the PV is removed.
       ```
       kubectl get pv
       ```
       {: pre}

2.  Create a configuration file for your PV. Include the block storage `id`, `ip_addr`, `capacity_gb`, the `datacenter`, and `lunIdID` that you retrieved earlier.

    ```
    apiVersion: v1
    kind: PersistentVolume
    metadata:
      name: mypv
      labels:
         failure-domain.beta.kubernetes.io/region: <region>
         failure-domain.beta.kubernetes.io/zone: <zone>
    spec:
      capacity:
        storage: "<storage_size>"
      accessModes:
        - ReadWriteOnce
      flexVolume:
        driver: "ibm/ibmc-block"
        fsType: "<fs_type>"
        options:
          "Lun": "<lun_ID>"
          "TargetPortal": "<IP_address>"
          "VolumeID": "<volume_ID>"
          "volumeName": "<volume_name>"
      ```
      {: codeblock}

    <table>
    <caption>Understanding the YAML file components</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the YAML file components</th>
    </thead>
    <tbody>
    <tr>
    <td><code>metadata/name</code></td>
    <td>Enter the name of the PV that you want to create.</td>
    </tr>
    <tr>
    <td><code>metadata/labels</code></td>
    <td>Enter the region and the zone that you retrieved earlier. You must have at least one worker node in the same region and zone as your persistent storage to mount the storage in your cluster. If a PV for your storage already exists, [add the zone and region label](cs_storage_basics.html#multizone) to your PV.
    </tr>
    <tr>
    <td><code>spec/flexVolume/fsType</code></td>
    <td>Enter the file system type that is configured for your existing block storage. Choose between <code>ext4</code> or <code>xfs</code>. If you do not specify this option, the PV defaults to <code>ext4</code>. When the wrong fsType is defined, then the PV creation succeeds, but the mounting of the PV to a pod fails. </td></tr>	    
    <tr>
    <td><code>spec/capacity/storage</code></td>
    <td>Enter the storage size of the existing block storage that you retrieved in the previous step as <code>capacity-gb</code>. The storage size must be written in gigabytes, for example, 20Gi (20 GB) or 1000Gi (1 TB).</td>
    </tr>
    <tr>
    <td><code>flexVolume/options/Lun</code></td>
    <td>Enter the lun ID for your block storage that you retrieved earlier as <code>lunId</code>.</td>
    </tr>
    <tr>
    <td><code>flexVolume/options/TargetPortal</code></td>
    <td>Enter the IP address of your block storage that you retrieved earlier as <code>ip_addr</code>. </td>
    </tr>
    <tr>
	    <td><code>flexVolume/options/VolumeId</code></td>
	    <td>Enter the ID of your block storage that you retrieved earlier as <code>id</code>.</td>
	    </tr>
	    <tr>
		    <td><code>flexVolume/options/volumeName</code></td>
		    <td>Enter a name for your volume.</td>
	    </tr>
    </tbody></table>

3.  Create the PV in your cluster.
    ```
    kubectl apply -f mypv.yaml
    ```
    {: pre}

4. Verify that the PV is created.
    ```
    kubectl get pv
    ```
    {: pre}

5. Create another configuration file to create your PVC. In order for the PVC to match the PV that you created earlier, you must choose the same value for `storage` and `accessMode`. The `storage-class` field must be empty. If any of these fields do not match the PV, then a new PV is created automatically instead.

     ```
     kind: PersistentVolumeClaim
     apiVersion: v1
     metadata:
      name: mypvc
      annotations:
        volume.beta.kubernetes.io/storage-class: ""
     spec:
      accessModes:
        - ReadWriteOnce
      resources:
        requests:
          storage: "<storage_size>"
     ```
     {: codeblock}

6.  Create your PVC.
     ```
     kubectl apply -f mypvc.yaml
     ```
     {: pre}

7.  Verify that your PVC is created and bound to the PV that you created earlier. This process can take a few minutes.
     ```
     kubectl describe pvc mypvc
     ```
     {: pre}

     Example output:

     ```
     Name: mypvc
     Namespace: default
     StorageClass:	""
     Status: Bound
     Volume: pvc-0d787071-3a67-11e7-aafc-eef80dd2dea2
     Labels: <none>
     Capacity: 20Gi
     Access Modes: RWO
     Events:
       FirstSeen LastSeen Count From        SubObjectPath Type Reason Message
       --------- -------- ----- ----        ------------- -------- ------ -------
       3m 3m 1 {ibm.io/ibmc-block 31898035-3011-11e7-a6a4-7a08779efd33 } Normal Provisioning External provisioner is provisioning volume  for claim "default/my-persistent-volume-claim"
       3m 1m	 10 {persistentvolume-controller } Normal ExternalProvisioning cannot find provisioner "ibm.io/ibmc-block", expecting that  a volume for the claim is provisioned either manually or via external software
       1m 1m 1 {ibm.io/ibmc-block 31898035-3011-11e7-a6a4-7a08779efd33 } Normal ProvisioningSucceeded	Successfully provisioned volume  pvc-0d787071-3a67-11e7-aafc-eef80dd2dea2
     ```
     {: screen}

You successfully created a PV and bound it to a PVC. Cluster users can now [mount the PVC](#app_volume_mount) to their deployments and start reading from and writing to the PV.

<br />



## Backing up and restoring data
{: #backup_restore}

Block storage is provisioned into the same location as the worker nodes in your cluster. The storage is hosted on clustered servers by IBM to provide availability in case a server goes down. However, block storage is not backed up automatically and might be inaccessible if the entire location fails. To protect your data from being lost or damaged, you can set up periodic backups that you can use to restore your data when needed.

Review the following backup and restore options for your block storage:

<dl>
  <dt>Set up periodic snapshots</dt>
  <dd><p>You can [set up periodic snapshots for your block storage](/docs/infrastructure/BlockStorage/snapshots.html#snapshots), which is a read-only image that captures the state of the instance at a point in time. To store the snapshot, you must request snapshot space on your block storage. Snapshots are stored on the existing storage instance within the same zone. You can restore data from a snapshot if a user accidentally removes important data from the volume. <strong>Note</strong>: If you have a Dedicated account, you must <a href="/docs/get-support/howtogetsupport.html#getting-customer-support">open a support ticket</a>.</br></br> <strong>To create a snapshot for your volume: </strong><ol><li>[Target your CLI](cs_cli_install.html#cs_cli_configure) to your cluster. </li><li>Log in to the `ibmcloud sl` CLI. <pre class="pre"><code>ibmcloud sl init</code></pre></li><li>List existing PVs in your cluster. <pre class="pre"><code>kubectl get pv</code></pre></li><li>Get the details for the PV for which you want to create snapshot space and note the volume ID, the size and the IOPS. <pre class="pre"><code>kubectl describe pv &lt;pv_name&gt;</code></pre> The size and IOPS are shown in the <strong>Labels</strong> section of your CLI output. To find the volume ID, review the <code>ibm.io/network-storage-id</code> annotation of your CLI output. </li><li>Create the snapshot size for your existing volume with the parameters that you retrieved in the previous step. <pre class="pre"><code>ibmcloud sl block snapshot-order &lt;volume_ID&gt; --size &lt;size&gt; --tier &lt;iops&gt;</code></pre></li><li>Wait for the snapshot size to create. <pre class="pre"><code>ibmcloud sl block volume-detail &lt;volume_ID&gt;</code></pre>The snapshot size is successfully provisioned when the <strong>Snapshot Size (GB)</strong> in your CLI output changes from 0 to the size that you ordered. </li><li>Create the snapshot for your volume and note the ID of the snapshot that is created for you. <pre class="pre"><code>ibmcloud sl block snapshot-create &lt;volume_ID&gt;</code></pre></li><li>Verify that the snapshot is created successfully. <pre class="pre"><code>ibmcloud sl block snapshot-list &lt;volume_ID&gt;</code></pre></li></ol></br><strong>To restore data from a snapshot to an existing volume: </strong><pre class="pre"><code>ibmcloud sl block snapshot-restore &lt;volume_ID&gt; &lt;snapshot_ID&gt;</code></pre></p></dd>
  <dt>Replicate snapshots to another zone</dt>
 <dd><p>To protect your data from a zone failure, you can [replicate snapshots](/docs/infrastructure/BlockStorage/replication.html#replicating-data) to a block storage instance that is set up in another zone. Data can be replicated from the primary storage to the backup storage only. You cannot mount a replicated block storage instance to a cluster. When your primary storage fails, you can manually set your replicated backup storage to be the primary one. Then, you can mount it to your cluster. After your primary storage is restored, you can restore the data from the backup storage. <strong>Note</strong>: If you have a Dedicated account, you cannot replicate snapshots to another zone.</p></dd>
 <dt>Duplicate storage</dt>
 <dd><p>You can [duplicate your block storage instance](/docs/infrastructure/BlockStorage/how-to-create-duplicate-volume.html#creating-a-duplicate-block-volume) in the same zone as the original storage instance. A duplicate has the same data as the original storage instance at the point in time that you create the duplicate. Unlike replicas, use the duplicate as an independent storage instance from the original. To duplicate, first set up snapshots for the volume. <strong>Note</strong>: If you have a Dedicated account, you must <a href="/docs/get-support/howtogetsupport.html#getting-customer-support">open a support ticket</a>.</p></dd>
  <dt>Back up data to {{site.data.keyword.cos_full}}</dt>
  <dd><p>You can use the [**ibm-backup-restore image**](/docs/services/RegistryImages/ibm-backup-restore/index.html#ibmbackup_restore_starter) to spin up a backup and restore pod in your cluster. This pod contains a script to run a one-time or periodic backup for any persistent volume claim (PVC) in your cluster. Data is stored in your {{site.data.keyword.cos_full}} instance that you set up in a zone.</p><strong>Note:</strong> Block storage is mounted with a RWO access mode. This access allows only one pod to be mounted to the block storage at a time. To backup your data, you must unmount the app pod from the storage, mount it to your backup pod, back up the data, and re-mount the storage to your app pod. </br></br>
To make your data even more highly available and protect your app from a zone failure, set up a second {{site.data.keyword.cos_short}} instance and replicate data across zones. If you need to restore data from your {{site.data.keyword.cos_short}} instance, use the restore script that is provided with the image.</dd>
<dt>Copy data to and from pods and containers</dt>
<dd><p>You can use the `kubectl cp` [command![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/reference/kubectl/overview/#cp) to copy files and directories to and from pods or specific containers in your cluster.</p>
<p>Before you begin, [target your Kubernetes CLI](cs_cli_install.html#cs_cli_configure) to the cluster that you want to use. If you do not specify a container with <code>-c</code>, the command uses to the first available container in the pod.</p>
<p>You can use the command in various ways:</p>
<ul>
<li>Copy data from your local machine to a pod in your cluster: <pre class="pre"><code>kubectl cp <var>&lt;local_filepath&gt;/&lt;filename&gt;</var> <var>&lt;namespace&gt;/&lt;pod&gt;:&lt;pod_filepath&gt;</var></code></pre></li>
<li>Copy data from a pod in your cluster to your local machine: <pre class="pre"><code>kubectl cp <var>&lt;namespace&gt;/&lt;pod&gt;:&lt;pod_filepath&gt;/&lt;filename&gt;</var> <var>&lt;local_filepath&gt;/&lt;filename&gt;</var></code></pre></li>
<li>Copy data from your local machine to a specific container that runs in a pod in your cluster: <pre class="pre"><code>kubectl cp <var>&lt;local_filepath&gt;/&lt;filename&gt;</var> <var>&lt;namespace&gt;/&lt;pod&gt;:&lt;pod_filepath&gt;</var> -c <var>&lt;container></var></code></pre></li>
</ul></dd>
  </dl>

<br />


## Storage class reference
{: #storageclass_reference}

### Bronze
{: #bronze}

<table>
<caption>Block storage class: bronze</caption>
<thead>
<th>Characteristics</th>
<th>Setting</th>
</thead>
<tbody>
<tr>
<td>Name</td>
<td><code>ibmc-block-bronze</code></br><code>ibmc-block-retain-bronze</code></td>
</tr>
<tr>
<td>Type</td>
<td>[Endurance storage ![External link icon](../icons/launch-glyph.svg "External link icon")](https://knowledgelayer.softlayer.com/topic/endurance-storage)</td>
</tr>
<tr>
<td>File system</td>
<td>ext4</td>
</tr>
<tr>
<td>IOPS per gigabyte</td>
<td>2</td>
</tr>
<tr>
<td>Size range in gigabytes</td>
<td>20-12000 Gi</td>
</tr>
<tr>
<td>Hard disk</td>
<td>SSD</td>
</tr>
<tr>
<td>Billing</td>
<td>The default billing type depends on the version of your {{site.data.keyword.Bluemix_notm}} Block Storage plug-in: <ul><li> Version 1.0.1 and higher: Hourly</li><li>Version 1.0.0 and lower: Monthly</li></ul></td>
</tr>
<tr>
<td>Pricing</td>
<td>[Pricing info ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/cloud/block-storage/pricing)</td>
</tr>
</tbody>
</table>


### Silver
{: #silver}

<table>
<caption>Block storage class: silver</caption>
<thead>
<th>Characteristics</th>
<th>Setting</th>
</thead>
<tbody>
<tr>
<td>Name</td>
<td><code>ibmc-block-silver</code></br><code>ibmc-block-retain-silver</code></td>
</tr>
<tr>
<td>Type</td>
<td>[Endurance storage ![External link icon](../icons/launch-glyph.svg "External link icon")](https://knowledgelayer.softlayer.com/topic/endurance-storage)</td>
</tr>
<tr>
<td>File system</td>
<td>ext4</td>
</tr>
<tr>
<td>IOPS per gigabyte</td>
<td>4</td>
</tr>
<tr>
<td>Size range in gigabytes</td>
<td>20-12000 Gi</td>
</tr>
<tr>
<td>Hard disk</td>
<td>SSD</td>
</tr>
<tr>
<td>Billing</td>
<td>The default billing type depends on the version of your {{site.data.keyword.Bluemix_notm}} Block Storage plug-in: <ul><li> Version 1.0.1 and higher: Hourly</li><li>Version 1.0.0 and lower: Monthly</li></ul></td>
</tr>
<tr>
<td>Pricing</td>
<td>[Pricing info ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/cloud/block-storage/pricing)</td>
</tr>
</tbody>
</table>

### Gold
{: #gold}

<table>
<caption>Block storage class: gold</caption>
<thead>
<th>Characteristics</th>
<th>Setting</th>
</thead>
<tbody>
<tr>
<td>Name</td>
<td><code>ibmc-block-gold</code></br><code>ibmc-block-retain-gold</code></td>
</tr>
<tr>
<td>Type</td>
<td>[Endurance storage ![External link icon](../icons/launch-glyph.svg "External link icon")](https://knowledgelayer.softlayer.com/topic/endurance-storage)</td>
</tr>
<tr>
<td>File system</td>
<td>ext4</td>
</tr>
<tr>
<td>IOPS per gigabyte</td>
<td>10</td>
</tr>
<tr>
<td>Size range in gigabytes</td>
<td>20-4000 Gi</td>
</tr>
<tr>
<td>Hard disk</td>
<td>SSD</td>
</tr>
<tr>
<td>Billing</td>
<td>The default billing type depends on the version of your {{site.data.keyword.Bluemix_notm}} Block Storage plug-in: <ul><li> Version 1.0.1 and higher: Hourly</li><li>Version 1.0.0 and lower: Monthly</li></ul></td>
</tr>
<tr>
<td>Pricing</td>
<td>[Pricing info ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/cloud/block-storage/pricing)</td>
</tr>
</tbody>
</table>

### Custom
{: #custom}

<table>
<caption>Block storage class: custom</caption>
<thead>
<th>Characteristics</th>
<th>Setting</th>
</thead>
<tbody>
<tr>
<td>Name</td>
<td><code>ibmc-block-custom</code></br><code>ibmc-block-retain-custom</code></td>
</tr>
<tr>
<td>Type</td>
<td>[Performance ![External link icon](../icons/launch-glyph.svg "External link icon")](https://knowledgelayer.softlayer.com/topic/performance-storage)</td>
</tr>
<tr>
<td>File system</td>
<td>ext4</td>
</tr>
<tr>
<td>IOPS and size</td>
<td><strong>Size range in gigabytes / IOPS range in multiples of 100</strong><ul><li>20-39 Gi / 100-1000 IOPS</li><li>40-79 Gi / 100-2000 IOPS</li><li>80-99 Gi / 100-4000 IOPS</li><li>100-499 Gi / 100-6000 IOPS</li><li>500-999 Gi / 100-10000 IOPS</li><li>1000-1999 Gi / 100-20000 IOPS</li><li>2000-2999 Gi / 200-40000 IOPS</li><li>3000-3999 Gi / 200-48000 IOPS</li><li>4000-7999 Gi / 300-48000 IOPS</li><li>8000-9999 Gi / 500-48000 IOPS</li><li>10000-12000 Gi / 1000-48000 IOPS</li></ul></td>
</tr>
<tr>
<td>Hard disk</td>
<td>The IOPS to gigabyte ratio determines the type of hard disk that is provisioned. To determine your IOPS to gigabyte ratio, you divide the IOPS by the size of your storage. </br></br>Example: </br>You chose 500Gi of storage with 100 IOPS. Your ratio is 0.2 (100 IOPS/500Gi). </br></br><strong>Overview of hard disk types per ratio:</strong><ul><li>Less than or equal to 0.3: SATA</li><li>Greater than 0.3: SSD</li></ul></td>
</tr>
<tr>
<td>Billing</td>
<td>The default billing type depends on the version of your {{site.data.keyword.Bluemix_notm}} Block Storage plug-in: <ul><li> Version 1.0.1 and higher: Hourly</li><li>Version 1.0.0 and lower: Monthly</li></ul></td>
</tr>
<tr>
<td>Pricing</td>
<td>[Pricing info ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/cloud/block-storage/pricing)</td>
</tr>
</tbody>
</table>

<br />


## Sample customized storage classes
{: #custom_storageclass}

### Specifying the zone for multizone clusters
{: #multizone_yaml}

The following `.yaml` file customizes a storage class that is based on the `ibm-block-silver` non-retaining storage class: the `type` is `"Endurance"`, the `iopsPerGB` is `4`, the `sizeRange` is `"[20-12000]Gi"`, and the `reclaimPolicy` is set to `"Delete"`. The zone is specified as `dal12`. You can review the previous information on `ibmc` storage classes to help you choose acceptable values for these </br>

To use a different storage class as your base, see the [storage class reference](#storageclass_reference).

```
apiVersion: storage.k8s.io/v1beta1
kind: StorageClass
metadata:
  name: ibmc-block-silver-mycustom-storageclass
  labels:
    kubernetes.io/cluster-service: "true"
provisioner: ibm.io/ibmc-block
parameters:
  zone: "dal12"
  type: "Endurance"
  iopsPerGB: "4"
  sizeRange: "[20-12000]Gi"
reclaimPolicy: "Delete"
```
{: codeblock}

Note that to create a persistentVolumeClaim with the above storage class, a `zone` label must be added to make the customization effective.

```
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: mypvc
  annotations:
    volume.beta.kubernetes.io/storage-class: "ibmc-block-silver-mycustom-storageclass"
  labels:
    zone: "dal12"
spec:
  accessModes:
    - ReadWriteMany
  resources:
    requests:
      storage: 24Gi
```
{: codeblock}

### Mounting block storage with an `XFS` file system
{: #xfs}

The following example creates a storage class that is named `ibmc-block-custom-xfs` and that provisions performance block storage with an `XFS` file system.

```
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: ibmc-block-custom-xfs
  labels:
    addonmanager.kubernetes.io/mode: Reconcile
provisioner: ibm.io/ibmc-block
parameters:
  type: "Performance"
  sizeIOPSRange: |-
    [20-39]Gi:[100-1000]
    [40-79]Gi:[100-2000]
    [80-99]Gi:[100-4000]
    [100-499]Gi:[100-6000]
    [500-999]Gi:[100-10000]
    [1000-1999]Gi:[100-20000]
    [2000-2999]Gi:[200-40000]
    [3000-3999]Gi:[200-48000]
    [4000-7999]Gi:[300-48000]
    [8000-9999]Gi:[500-48000]
    [10000-12000]Gi:[1000-48000]
  fsType: "xfs"
  reclaimPolicy: "Delete"
  classVersion: "2"
```
{: codeblock}

<br />
