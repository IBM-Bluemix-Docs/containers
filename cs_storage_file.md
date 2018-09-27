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




# Storing data on IBM File Storage for IBM Cloud
{: #file_storage}


## Deciding on the file storage configuration
{: #predefined_storageclass}

{{site.data.keyword.containerlong}} provides pre-defined storage classes for file storage that you can use to provision file storage with a specific configuration.
{: shortdesc}

Every storage class specifies the type of file storage that you provision, including available size, IOPS, file system, and the retention policy.  

**Important:** Make sure to choose your storage configuration carefully to have enough capacity to store your data. After you provision a specific type of storage by using a storage class, you cannot change the size, type, IOPS, or retention policy for the storage device. If you need more storage or storage with a different configuration, you must [create a new storage instance and copy the data](cs_storage_basics.html#update_storageclass) from the old storage instance to your new one.

1. List available storage classes in {{site.data.keyword.containerlong}}.
    ```
    kubectl get storageclasses | grep file
    ```
    {: pre}

    Example output:
    ```
    $ kubectl get storageclasses
    NAME                         TYPE
    ibmc-file-bronze (default)   ibm.io/ibmc-file
    ibmc-file-custom             ibm.io/ibmc-file
    ibmc-file-gold               ibm.io/ibmc-file
    ibmc-file-retain-bronze      ibm.io/ibmc-file
    ibmc-file-retain-custom      ibm.io/ibmc-file
    ibmc-file-retain-gold        ibm.io/ibmc-file
    ibmc-file-retain-silver      ibm.io/ibmc-file
    ibmc-file-silver             ibm.io/ibmc-file
    ```
    {: screen}

2. Review the configuration of a storage class.
   ```
   kubectl describe storageclass <storageclass_name>
   ```
   {: pre}

   For more information about each storage class, see the [storage class reference](#storageclass_reference). If you do not find what you are looking for, consider creating your own customized storage class. To get started, check out the [customized storage class samples](#custom_storageclass).
   {: tip}

3. Choose the type of file storage that you want to provision.
   - **Bronze, silver, and gold storage classes:** These storage classes provision [Endurance storage ![External link icon](../icons/launch-glyph.svg "External link icon")](https://knowledgelayer.softlayer.com/topic/endurance-storage). Endurance storage lets you choose the size of the storage in gigabytes at predefined IOPS tiers.
   - **Custom storage class:** This storage class provisions [Performance storage ![External link icon](../icons/launch-glyph.svg "External link icon")](https://knowledgelayer.softlayer.com/topic/performance-storage). With performance storage, you have more control over the size of the storage and the IOPS.

4. Choose the size and IOPS for your file storage. The size and the number of IOPS define the total number of IOPS (input/ output operations per second) that serves as an indicator for how fast your storage is. The more total IOPS your storage has, the faster it processes read and write operations.
   - **Bronze, silver, and gold storage classes:** These storage classes come with a fixed number of IOPS per gigabyte and are provisioned on SSD hard disks. The total number of IOPS depends on the size of the storage that you choose. You can select any whole number of gigabyte within the allowed size range, such as 20 Gi, 256 Gi, or 11854 Gi. To determine the total number of IOPS, you must multiply the IOPS with the selected size. For example, if you select a 1000Gi file storage size in the silver storage class that comes with 4 IOPS per GB, your storage has a total of 4000 IOPS.
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
   - **Custom storage class:** When you choose this storage class, you have more control over the size and IOPS that you want. For the size, you can select any whole number of gigabyte within the allowed size range. The size that you choose determines the IOPS range that is available to you. You can choose an IOPS that is a multiple of 100 that is in the specified range. The IOPS that you choose is static and does not scale with the size of the storage. For example, if you choose 40Gi with 100 IOPS, your total IOPS remains 100. </br></br> The IOPS to gigabyte ratio also determines the type of hard disk that is provisioned for you. For example, if you have 500Gi at 100 IOPS, your IOPS to gigabyte ratio is 0.2. Storage with a ratio of less than or equal to 0.3 is provisioned on SATA hard disks. If your ratio is greater than 0.3, then your storage is provisioned on SSD hard disks.  
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
   - If you want to keep your data, then choose a `retain` storage class. When you delete the PVC, only the PVC is deleted. The PV, the physical storage device in your IBM Cloud infrastructure (SoftLayer) account, and your data still exist. To reclaim the storage and use it in your cluster again, you must remove the PV and follow the steps for [using existing file storage](#existing_file).
   - If you want the PV, the data, and your physical file storage device to be deleted when you delete the PVC, choose a storage class without `retain`. **Note**: If you have a Dedicated account, choose a storage class without `retain` to prevent orphaned volumes in IBM Cloud infrastructure (SoftLayer).

6. Choose if you want to be billed hourly or monthly. Check the [pricing ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/cloud/file-storage/pricing) for more information. By default, all file storage devices are provisioned with an hourly billing type.
   **Note:** If you choose a monthly billing type, when you remove the persistent storage, you still pay the monthly charge for it, even if you used it only for a short amount of time.

<br />



## Adding file storage to apps
{: #add_file}

Create a persistent volume claim (PVC) to [dynamically provision](cs_storage_basics.html#dynamic_provisioning) file storage for your cluster. Dynamic provisioning automatically creates the matching persistent volume (PV) and orders the physical storage device in your IBM Cloud infrastructure (SoftLayer) account.
{:shortdesc}

Before you begin:
- If you have a firewall, [allow egress access](cs_firewall.html#pvc) for the IBM Cloud infrastructure (SoftLayer) IP ranges of the zones that your clusters are in so that you can create PVCs.
- [Decide on a pre-defined storage class](#predefined_storageclass) or create a [customized storage class](#custom_storageclass).

  **Tip:** If you have a multizone cluster, the zone in which your storage is provisioned is selected on a round-robin basis to balance volume requests evenly across all zones. If you want to specify the zone for your storage, create a [customized storage class](#multizone_yaml) first. Then, follow the steps in this topic to provision storage by using your customized storage class.

To add file storage:

1.  Create a configuration file to define your persistent volume claim (PVC) and save the configuration as a `.yaml` file.

    - **Example for bronze, silver, gold storage classes**:
       The following `.yaml` file creates a claim that is named `mypvc` of the `"ibmc-file-silver"` storage class, billed `"monthly"`, with a gigabyte size of `24Gi`.

       ```
       apiVersion: v1
       kind: PersistentVolumeClaim
       metadata:
         name: mypvc
         annotations:
           volume.beta.kubernetes.io/storage-class: "ibmc-file-silver"
         labels:
           billingType: "monthly"
       spec:
         accessModes:
           - ReadWriteMany
         resources:
           requests:
             storage: 24Gi
       ```
       {: codeblock}

    -  **Example for using the custom storage class**:
       The following `.yaml` file creates a claim that is named `mypvc` of the storage class `ibmc-file-retain-custom`, billed `"hourly"`, with a gigabyte size of `45Gi` and IOPS of `"300"`.

       ```
       apiVersion: v1
       kind: PersistentVolumeClaim
       metadata:
         name: mypvc
         annotations:
           volume.beta.kubernetes.io/storage-class: "ibmc-file-retain-custom"
         labels:
           billingType: "hourly"
       spec:
         accessModes:
           - ReadWriteMany
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
       <td>The name of the storage class that you want to use to provision file storage. </br> If you do not specify a storage class, the PV is created with the default storage class <code>ibmc-file-bronze</code><p>**Tip:** If you want to change the default storage class, run <code>kubectl patch storageclass &lt;storageclass&gt; -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'</code> and replace <code>&lt;storageclass&gt;</code> with the name of the storage class.</p></td>
       </tr>
       <tr>
         <td><code>metadata/labels/billingType</code></td>
          <td>Specify the frequency for which your storage bill is calculated, "monthly" or "hourly". If you do not specify a billing type, the storage is provisioned with an hourly billing type. </td>
       </tr>
       <tr>
       <td><code>spec/accessMode</code></td>
       <td>Specify one of the following options: <ul><li><strong>ReadWriteMany: </strong>The PVC can be mounted by multiple pods. All pods can read from and write to the volume. </li><li><strong>ReadOnlyMany: </strong>The PVC can be mounted by multiple pods. All pods have read-only access. <li><strong>ReadWriteOnce: </strong>The PVC can be mounted by one pod only. This pod can read from and write to the volume. </li></ul></td>
       </tr>
       <tr>
       <td><code>spec/resources/requests/storage</code></td>
       <td>Enter the size of the file storage, in gigabytes (Gi). </br></br><strong>Note: </strong> After your storage is provisioned, you cannot change the size of your file storage. Make sure to specify a size that matches the amount of data that you want to store. </td>
       </tr>
       <tr>
       <td><code>spec/resources/requests/iops</code></td>
       <td>This option is available for the custom storage classes only (`ibmc-file-custom / ibmc-file-retain-custom`). Specify the total IOPS for the storage, selecting a multiple of 100 within the allowable range. If you choose an IOPS other than one that is listed, the IOPS is rounded up.</td>
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
      3m		3m		1	{ibm.io/ibmc-file 31898035-3011-11e7-a6a4-7a08779efd33 }			Normal		Provisioning		External provisioner is provisioning volume for claim "default/my-persistent-volume-claim"
      3m		1m		10	{persistentvolume-controller }							Normal		ExternalProvisioning	cannot find provisioner "ibm.io/ibmc-file", expecting that a volume for the claim is provisioned either manually or via external software
      1m		1m		1	{ibm.io/ibmc-file 31898035-3011-11e7-a6a4-7a08779efd33 }			Normal		ProvisioningSucceeded	Successfully provisioned volume pvc-0d787071-3a67-11e7-aafc-eef80dd2dea2

    ```
    {: screen}

4.  {: #app_volume_mount}To mount the storage to your deployment, create a configuration `.yaml` file and specify the PVC that binds the PV.

    If you have an app that requires a non-root user to write to the persistent storage, or an app that requires that the mount path is owned by the root user, see [Adding non-root user access to NFS file storage](cs_troubleshoot_storage.html#nonroot) or [Enabling root permission for NFS file storage](cs_troubleshoot_storage.html#nonroot).
    {: tip}

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
    <td>The absolute path of the directory to where the volume is mounted inside the container. Data that is written to the mount path is stored under the <code>root</code> directory in your physical file storage instance. To create directories in your physical file storage instance, you must create subdirectories in your mount path. </td>
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




## Using existing file storage in your cluster
{: #existing_file}

If you have an existing physical storage device that you want to use in your cluster, you can manually create the PV and PVC to [statically provision](cs_storage_basics.html#static_provisioning) the storage.

Before you begin, make sure that you have at least one worker node that exists in the same zone as your existing file storage instance.

### Step 1: Preparing your existing storage.

Before you can start to mount your existing storage to an app, you must retrieve all necessary information for your PV and prepare the storage to be accessible in your cluster.  

**For storage that was provisioned with a `retain` storage class:** </br>
If you provisioned storage with a `retain` storage class and you remove the PVC, the PV and the physical storage device are not automatically removed. To reuse the storage in your cluster, you must remove the remaining PV first.

To use existing storage in a different cluster than the one where you provisioned it, follow the steps for [storage that was created outside of the cluster](#external_storage) to add the storage to the subnet of your worker node.
{: tip}

1. List existing PVs.
   ```
   kubectl get pv
   ```
   {: pre}

   Look for the PV that belongs to your persistent storage. The PV is in a `released` state.

2. Get the details of the PV.
   ```
   kubectl describe pv <pv_name>
   ```
   {: pre}

3. Note the `CapacityGb`, `storageClass`, `failure-domain.beta.kubernetes.io/region`, `failure-domain.beta.kubernetes.io/zone`, `server`, and `path`.

4. Remove the PV.
   ```
   kubectl delete pv <pv_name>
   ```
   {: pre}

5. Verify that the PV is removed.
   ```
   kubectl get pv
   ```
   {: pre}

</br>

**For persistent storage that was provisioned outside the cluster:** </br>
If you want to use existing storage that you provisioned earlier, but never used in your cluster before, you must make the storage available in the same subnet as your worker nodes.

**Note**: If you have a Dedicated account, you must [open a support ticket](/docs/get-support/howtogetsupport.html#getting-customer-support).

1.  {: #external_storage}From the [IBM Cloud infrastructure (SoftLayer) portal ![External link icon](../icons/launch-glyph.svg "External link icon")](https://control.bluemix.net/), click **Storage**.
2.  Click **File Storage** and from the **Actions** menu, select **Authorize Host**.
3.  Select **Subnets**.
4.  From the drop-down list, select the private VLAN subnet that your worker node is connected to. To find the subnet of your worker node, run `ibmcloud ks workers <cluster_name>` and compare the `Private IP` of your worker node with the subnet that you found in the drop-down list.
5.  Click **Submit**.
6.  Click the name of the file storage.
7.  Note the `Mount Point`, the `size`, and the `Location` field. The `Mount Point` field is displayed as `<nfs_server>:<file_storage_path>`.

### Step 2: Creating a persistent volume (PV) and a matching persistent volume claim (PVC)

1.  Create a storage configuration file for your PV. Include the values that you retrieved earlier.

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
       storage: "<size>"
     accessModes:
       - ReadWriteMany
     nfs:
       server: "<nfs_server>"
       path: "<file_storage_path>"
    ```
    {: codeblock}

    <table>
    <caption>Understanding the YAML file components</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the YAML file components</th>
    </thead>
    <tbody>
    <tr>
    <td><code>name</code></td>
    <td>Enter the name of the PV object to create.</td>
    </tr>
    <tr>
    <td><code>metadata/labels</code></td>
    <td>Enter the region and the zone that you retrieved earlier. You must have at least one worker node in the same region and zone as your persistent storage to mount the storage in your cluster. If a PV for your storage already exists, [add the zone and region label](cs_storage_basics.html#multizone) to your PV.
    </tr>
    <tr>
    <td><code>spec/capacity/storage</code></td>
    <td>Enter the storage size of the existing NFS file share that you retrieved earlier. The storage size must be written in gigabytes, for example, 20Gi (20 GB) or 1000Gi (1 TB), and the size must match the size of the existing file share.</td>
    </tr>
    <tr>
    <td><code>spec/accessMode</code></td>
    <td>Specify one of the following options: <ul><li><strong>ReadWriteMany: </strong>The PVC can be mounted by multiple pods. All pods can read from and write to the volume. </li><li><strong>ReadOnlyMany: </strong>The PVC can be mounted by multiple pods. All pods have read-only access. <li><strong>ReadWriteOnce: </strong>The PVC can be mounted by one pod only. This pod can read from and write to the volume. </li></ul></td>
    </tr>
    <tr>
    <td><code>spec/nfs/server</code></td>
    <td>Enter the NFS file share server ID that you retrieved earlier.</td>
    </tr>
    <tr>
    <td><code>path</code></td>
    <td>Enter the path to the NFS file share that you retrieved earlier.</td>
    </tr>
    </tbody></table>

3.  Create the PV in your cluster.

    ```
    kubectl apply -f mypv.yaml
    ```
    {: pre}

4.  Verify that the PV is created.

    ```
    kubectl get pv
    ```
    {: pre}

5.  Create another configuration file to create your PVC. In order for the PVC to match the PV that you created earlier, you must choose the same value for `storage` and `accessMode`. The `storage-class` field must be empty. If any of these fields do not match the PV, then a new PV, and a new physical storage instance is [dynamically provisioned](cs_storage_basics.html#dynamic_provisioning).

    ```
    kind: PersistentVolumeClaim
    apiVersion: v1
    metadata:
     name: mypvc
     annotations:
       volume.beta.kubernetes.io/storage-class: ""
    spec:
     accessModes:
       - ReadWriteMany
     resources:
       requests:
         storage: "<size>"
    ```
    {: codeblock}

6.  Create your PVC.

    ```
    kubectl apply -f mypvc.yaml
    ```
    {: pre}

7.  Verify that your PVC is created and bound to the PV. This process can take a few minutes.

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
    Access Modes: RWX
    Events:
      FirstSeen LastSeen Count From        SubObjectPath Type Reason Message
      --------- -------- ----- ----        ------------- -------- ------ -------
      3m 3m 1 {ibm.io/ibmc-file 31898035-3011-11e7-a6a4-7a08779efd33 } Normal Provisioning External provisioner is provisioning volume for claim "default/my-persistent-volume-claim"
      3m 1m	 10 {persistentvolume-controller } Normal ExternalProvisioning cannot find provisioner "ibm.io/ibmc-file", expecting that a volume for the claim is provisioned either manually or via external software
      1m 1m 1 {ibm.io/ibmc-file 31898035-3011-11e7-a6a4-7a08779efd33 } Normal ProvisioningSucceeded	Successfully provisioned volume pvc-0d787071-3a67-11e7-aafc-eef80dd2dea2
    ```
    {: screen}


You successfully created a PV and bound it to a PVC. Cluster users can now [mount the PVC](#app_volume_mount) to their deployments and start reading from and writing to the PV object.

<br />



## Changing the default NFS version
{: #nfs_version}

The version of the file storage determines the protocol that is used to communicate with the {{site.data.keyword.Bluemix_notm}} file storage server. By default, all file storage instances are set up with NFS version 4. You can change your existing PV to an older NFS version if your app requires a specific version to properly function.
{: shortdesc}

To change the default NFS version, you can either create a new storage class to dynamically provision file storage in your cluster, or choose to change an existing PV that is mounted to your pod.

**Important:** To apply the latest security updates and for a better performance, use the default NFS version and do not change to an older NFS version.

**To create a customized storage class with the desired NFS version:**
1. Create a [customized storage class](#nfs_version_class) with the NFS version that you want to provision.
2. Create the storage class in your cluster.
   ```
   kubectl apply -f nfsversion_storageclass.yaml
   ```
   {: pre}

3. Verify that the customized storage class was created.
   ```
   kubectl get storageclasses
   ```
   {: pre}

4. Provision [file storage](#add_file) with your customized storage class.

**To change your existing PV to use a different NFS version:**

1. Get the PV of the file storage where you want to change the NFS version and note the name of the PV.
   ```
   kubectl get pv
   ```
   {: pre}

2. Add an annotation to your PV. Replace `<version_number>` with the NFS version that you want to use. For example, to change to NFS version 3.0, enter **3**.  
   ```
   kubectl patch pv <pv_name> -p '{"metadata": {"annotations":{"volume.beta.kubernetes.io/mount-options":"vers=<version_number>"}}}'
   ```
   {: pre}

3. Delete the pod that uses the file storage and re-create the pod.
   1. Save the pod yaml to your local machine.
      ```
      kubect get pod <pod_name> -o yaml > <filepath/pod.yaml>
      ```
      {: pre}

   2. Delete the pod.
      ```
      kubectl deleted pod <pod_name>
      ```
      {: pre}

   3. Re-create the pod.
      ```
      kubectl apply -f pod.yaml
      ```
      {: pre}

4. Wait for the pod to deploy.
   ```
   kubectl get pods
   ```
   {: pre}

   The pod is fully deployed when the status changes to `Running`.

5. Log in to your pod.
   ```
   kubectl exec -it <pod_name> sh
   ```
   {: pre}

6. Verify that the file storage was mounted with the NFS version that you specified earlier.
   ```
   mount | grep "nfs" | awk -F" |," '{ print $5, $8 }'
   ```
   {: pre}

   Example output:
   ```
   nfs vers=3.0
   ```
   {: screen}

<br />


## Backing up and restoring data
{: #backup_restore}

File storage is provisioned into the same location as the worker nodes in your cluster. The storage is hosted on clustered servers by IBM to provide availability in case a server goes down. However, file storage is not backed up automatically and might be inaccessible if the entire location fails. To protect your data from being lost or damaged, you can set up periodic backups that you can use to restore your data when needed.
{: shortdesc}

Review the following backup and restore options for your file storage:

<dl>
  <dt>Set up periodic snapshots</dt>
  <dd><p>You can [set up periodic snapshots for your file storage](/docs/infrastructure/FileStorage/snapshots.html), which is a read-only image that captures the state of the instance at a point in time. To store the snapshot, you must request snapshot space on your file storage. Snapshots are stored on the existing storage instance within the same zone. You can restore data from a snapshot if a user accidentally removes important data from the volume. <strong>Note</strong>: If you have a Dedicated account, you must [open a support ticket](/docs/get-support/howtogetsupport.html#getting-customer-support).</br></br> <strong>To create a snapshot for your volume: </strong><ol><li>[Target your CLI](cs_cli_install.html#cs_cli_configure) to your cluster. </li><li>Log in to the `ibmcloud sl` CLI. <pre class="pre"><code>ibmcloud sl init</code></pre></li><li>List existing PVs in your cluster. <pre class="pre"><code>kubectl get pv</code></pre></li><li>Get the details for the PV for which you want to create snapshot space and note the volume ID, the size and the IOPS. <pre class="pre"><code>kubectl describe pv &lt;pv_name&gt;</code></pre> The volume ID, the size and the IOPS can be found in the <strong>Labels</strong> section of your CLI output. </li><li>Create the snapshot size for your existing volume with the parameters that you retrieved in the previous step. <pre class="pre"><code>ibmcloud sl file snapshot-order &lt;volume_ID&gt; --size &lt;size&gt; --tier &lt;iops&gt;</code></pre></li><li>Wait for the snapshot size to create. <pre class="pre"><code>ibmcloud sl file volume-detail &lt;volume_ID&gt;</code></pre>The snapshot size is successfully provisioned when the <strong>Snapshot Size (GB)</strong> in your CLI output changes from 0 to the size that you ordered. </li><li>Create the snapshot for your volume and note the ID of the snapshot that is created for you. <pre class="pre"><code>ibmcloud sl file snapshot-create &lt;volume_ID&gt;</code></pre></li><li>Verify that the snapshot is created successfully. <pre class="pre"><code>ibmcloud sl file snapshot-list &lt;volume_ID&gt;</code></pre></li></ol></br><strong>To restore data from a snapshot to an existing volume: </strong><pre class="pre"><code>ibmcloud sl file snapshot-restore &lt;volume_ID&gt; &lt;snapshot_ID&gt;</code></pre></p></dd>
  <dt>Replicate snapshots to another zone</dt>
 <dd><p>To protect your data from a zone failure, you can [replicate snapshots](/docs/infrastructure/FileStorage/replication.html#replicating-data) to a file storage instance that is set up in another zone. Data can be replicated from the primary storage to the backup storage only. You cannot mount a replicated file storage instance to a cluster. When your primary storage fails, you can manually set your replicated backup storage to be the primary one. Then, you can mount it to your cluster. After your primary storage is restored, you can restore the data from the backup storage. <strong>Note</strong>: If you have a Dedicated account, you cannot replicate snapshots to another zone.</p></dd>
 <dt>Duplicate storage</dt>
 <dd><p>You can [duplicate your file storage instance](/docs/infrastructure/FileStorage/how-to-create-duplicate-volume.html#creating-a-duplicate-file-storage) in the same zone as the original storage instance. A duplicate has the same data as the original storage instance at the point in time that you create the duplicate. Unlike replicas, use the duplicate as an independent storage instance from the original. To duplicate, first [set up snapshots for the volume](/docs/infrastructure/FileStorage/snapshots.html). <strong>Note</strong>: If you have a Dedicated account, you must <a href="/docs/get-support/howtogetsupport.html#getting-customer-support">open a support ticket</a>.</p></dd>
  <dt>Back up data to {{site.data.keyword.cos_full}}</dt>
  <dd><p>You can use the [**ibm-backup-restore image**](/docs/services/RegistryImages/ibm-backup-restore/index.html#ibmbackup_restore_starter) to spin up a backup and restore pod in your cluster. This pod contains a script to run a one-time or periodic backup for any persistent volume claim (PVC) in your cluster. Data is stored in your {{site.data.keyword.cos_full}} instance that you set up in a zone.</p>
  <p>To make your data even more highly available and protect your app from a zone failure, set up a second {{site.data.keyword.cos_full}} instance and replicate data across zones. If you need to restore data from your {{site.data.keyword.cos_full}} instance, use the restore script that is provided with the image.</p></dd>
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
<caption>File storage class: bronze</caption>
<thead>
<th>Characteristics</th>
<th>Setting</th>
</thead>
<tbody>
<tr>
<td>Name</td>
<td><code>ibmc-file-bronze</code></br><code>ibmc-file-retain-bronze</code></td>
</tr>
<tr>
<td>Type</td>
<td>[Endurance storage ![External link icon](../icons/launch-glyph.svg "External link icon")](https://knowledgelayer.softlayer.com/topic/endurance-storage)</td>
</tr>
<tr>
<td>File system</td>
<td>NFS</td>
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
<td>Hourly</td>
</tr>
<tr>
<td>Pricing</td>
<td>[Pricing info ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/cloud/file-storage/pricing)</td>
</tr>
</tbody>
</table>


### Silver
{: #silver}

<table>
<caption>File storage class: silver</caption>
<thead>
<th>Characteristics</th>
<th>Setting</th>
</thead>
<tbody>
<tr>
<td>Name</td>
<td><code>ibmc-file-silver</code></br><code>ibmc-file-retain-silver</code></td>
</tr>
<tr>
<td>Type</td>
<td>[Endurance storage ![External link icon](../icons/launch-glyph.svg "External link icon")](https://knowledgelayer.softlayer.com/topic/endurance-storage)</td>
</tr>
<tr>
<td>File system</td>
<td>NFS</td>
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
<td>Hourly</li></ul></td>
</tr>
<tr>
<td>Pricing</td>
<td>[Pricing info ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/cloud/file-storage/pricing)</td>
</tr>
</tbody>
</table>

### Gold
{: #gold}

<table>
<caption>File storage class: gold</caption>
<thead>
<th>Characteristics</th>
<th>Setting</th>
</thead>
<tbody>
<tr>
<td>Name</td>
<td><code>ibmc-file-gold</code></br><code>ibmc-file-retain-gold</code></td>
</tr>
<tr>
<td>Type</td>
<td>[Endurance storage ![External link icon](../icons/launch-glyph.svg "External link icon")](https://knowledgelayer.softlayer.com/topic/endurance-storage)</td>
</tr>
<tr>
<td>File system</td>
<td>NFS</td>
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
<td>Hourly</li></ul></td>
</tr>
<tr>
<td>Pricing</td>
<td>[Pricing info ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/cloud/file-storage/pricing)</td>
</tr>
</tbody>
</table>

### Custom
{: #custom}

<table>
<caption>File storage class: custom</caption>
<thead>
<th>Characteristics</th>
<th>Setting</th>
</thead>
<tbody>
<tr>
<td>Name</td>
<td><code>ibmc-file-custom</code></br><code>ibmc-file-retain-custom</code></td>
</tr>
<tr>
<td>Type</td>
<td>[Performance ![External link icon](../icons/launch-glyph.svg "External link icon")](https://knowledgelayer.softlayer.com/topic/performance-storage)</td>
</tr>
<tr>
<td>File system</td>
<td>NFS</td>
</tr>
<tr>
<td>IOPS and size</td>
<td><p><strong>Size range in gigabytes / IOPS range in multiples of 100</strong></p><ul><li>20-39 Gi / 100-1000 IOPS</li><li>40-79 Gi / 100-2000 IOPS</li><li>80-99 Gi / 100-4000 IOPS</li><li>100-499 Gi / 100-6000 IOPS</li><li>500-999 Gi / 100-10000 IOPS</li><li>1000-1999 Gi / 100-20000 IOPS</li><li>2000-2999 Gi / 200-40000 IOPS</li><li>3000-3999 Gi / 200-48000 IOPS</li><li>4000-7999 Gi / 300-48000 IOPS</li><li>8000-9999 Gi / 500-48000 IOPS</li><li>10000-12000 Gi / 1000-48000 IOPS</li></ul></td>
</tr>
<tr>
<td>Hard disk</td>
<td>The IOPS to gigabyte ratio determines the type of hard disk that is provisioned. To determine your IOPS to gigabyte ratio, you divide the IOPS by the size of your storage. </br></br>Example: </br>You chose 500Gi of storage with 100 IOPS. Your ratio is 0.2 (100 IOPS/500Gi). </br></br><strong>Overview of hard disk types per ratio:</strong><ul><li>Less than or equal to 0.3: SATA</li><li>Greater than 0.3: SSD</li></ul></td>
</tr>
<tr>
<td>Billing</td>
<td>Hourly</li></ul></td>
</tr>
<tr>
<td>Pricing</td>
<td>[Pricing info ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/cloud/file-storage/pricing)</td>
</tr>
</tbody>
</table>

<br />


## Sample customized storage classes
{: #custom_storageclass}

### Specifying the zone for multizone clusters
{: #multizone_yaml}

The following `.yaml` file customizes a storage class that is based on the `ibm-file-silver` non-retaining storage class: the `type` is `"Endurance"`, the `iopsPerGB` is `4`, the `sizeRange` is `"[20-12000]Gi"`, and the `reclaimPolicy` is set to `"Delete"`. The zone is specified as `dal12`. You can review the previous information on `ibmc` storage classes to help you choose acceptable values for these </br>

```
apiVersion: storage.k8s.io/v1beta1
kind: StorageClass
metadata:
  name: ibmc-file-silver-mycustom-storageclass
  labels:
    kubernetes.io/cluster-service: "true"
provisioner: ibm.io/ibmc-file
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
    volume.beta.kubernetes.io/storage-class: "ibmc-file-silver-mycustom-storageclass"
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

### Changing the default NFS version
{: #nfs_version_class}

The following customized storage class is based on the [`ibmc-file-bronze` storage class](#bronze) and lets you define the NFS version that you want to provision. For example, to provision NFS version 3.0, replace `<nfs_version>` with **3.0**.
```
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: ibmc-file-mount
  #annotations:
  #  storageclass.beta.kubernetes.io/is-default-class: "true"
  labels:
    kubernetes.io/cluster-service: "true"
provisioner: ibm.io/ibmc-file
parameters:
  type: "Endurance"
  iopsPerGB: "2"
  sizeRange: "[1-12000]Gi"
  reclaimPolicy: "Delete"
  classVersion: "2"
  mountOptions: nfsvers=<nfs_version>
```
{: codeblock}
