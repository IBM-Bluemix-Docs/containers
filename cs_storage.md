---

copyright:
  years: 2014, 2018
lastupdated: "2018-03-29"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}


# Saving data in your cluster
{: #storage}
You can persist data in {{site.data.keyword.containerlong}} to share data between app instances and to protect your data from being lost if a component in your Kubernetes cluster fails.

## Planning highly available storage
{: #planning}

In {{site.data.keyword.containerlong_notm}} you can choose from several options to store your app data and share data across pods in your cluster. However, not all storage options offer the same level of persistence and availability in situations where a component in your cluster or a whole site fails.
{: shortdesc}

### Non-persistent data storage options
{: #non_persistent}

You can use non-persistent storage options if your data is not required to be persistently stored, so that you can recover it after a component in your cluster fails, or if data does not need to be shared across app instances. Non-persistent storage options can also be used to unit-test your app components or try out new features.
{: shortdesc}

The following image shows available non-persistent data storage options in {{site.data.keyword.containerlong_notm}}. These options are available for free and standard clusters.
<p>
<img src="images/cs_storage_nonpersistent.png" alt="Non-persistent data storage options" width="450" style="width: 450px; border-style: none"/></p>

<table summary="The table shows non-persistent storage options. Rows are to be read from the left to right, with the number of the option in column one, the title of the otion in column two and a description in column three." style="width: 100%">
<caption>Table. Non-persistent storage options</caption>
  <thead>
  <th>Option</th>
  <th>Description</th>
  </thead>
  <tbody>
    <tr>
      <td>1. Inside the container or pod</td>
      <td>Containers and pods are, by design, short-lived and can fail unexpectedly. However, you can write data to the local file system of the container to store data throughout the lifecycle of the container. Data inside a container cannot be shared with other containers or pods and is lost when the container crashes or is removed. For more information, see [Storing data in a container](https://docs.docker.com/storage/).</td>
    </tr>
  <tr>
    <td>2. On the worker node</td>
    <td>Every worker node is set up with primary and secondary storage that is determined by the machine type that you select for your worker node. The primary storage is used to store data from the operating system and can be accessed by using a [Kubernetes <code>hostPath</code> volume ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/concepts/storage/volumes/#hostpath). The secondary storage is used to store data in <code>/var/lib/docker</code>, the directory that all the container data is written to. You can access the secondary storage by using a [Kubernetes <code>emptyDir</code> volume ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/concepts/storage/volumes/#emptydir)<br/><br/>While <code>hostPath</code> volumes are used to mount files from the worker node file system to your pod, <code>emptyDir</code> creates an empty directory that is assigned to a pod in your cluster. All containers in that pod can read from and write to that volume. Because the volume is assigned to one specific pod, data cannot be shared with other pods in a replica set.<br/><br/><p>A <code>hostPath</code> or <code>emptyDir</code> volume and its data are removed when: <ul><li>The worker node is deleted.</li><li>The worker node is reloaded or updated.</li><li>The cluster is deleted.</li><li>The {{site.data.keyword.Bluemix_notm}} account reaches a suspended state. </li></ul></p><p>In addtion, data in an <code>emptyDir</code> volume is removed when: <ul><li>The assigned pod is permanently deleted from the worker node.</li><li>The assigned pod is scheduled on another worker node.</li></ul></p><p><strong>Note:</strong> If the container inside the pod crashes, the data in the volume is still available on the worker node.</p></td>
    </tr>
    </tbody>
    </table>

### Persistent data storage options for high availability
{: persistent}

The main challenge when you create highly available stateful apps is to persist data across multiple app instances in multiple locations, and to keep data in sync at all times. For high available data, you want to make sure that you have a master database with multiple instances that are spread across multiple data centers or even multiple regions, and that data in this master is continuously replicated. All instances in your cluster must read from and write to this master database. In case one instance of the master is down, other instances can take over the workload, so that you do not experience downtime for your apps.
{: shortdesc}

The following image shows the options that you have in {{site.data.keyword.containerlong_notm}} to make your data highly available in a standard cluster. The option that is right for you depends on the following factors:
  * **The type of app that you have:** For example, you might have an app that must store data on a file basis rather than inside a database.
  * **Legal requirements for where to store and route the data:** For example, you might be obligated to store and route data in the United States only and you cannot use a service that is located in Europe.
  * **Backup and restore options:** Every storage options comes with capabilities to backup and restore data. Check that available backup and restore options meet the requirements of your disaster recovery plan, such as the frequency of backups or the capabilities of storing data outside your primary data center.
  * **Global replication:** For high availability, you might want to set up multiple instances of storage that are distributed and replicated across data centers worldwide.

<br/>
<img src="images/cs_storage_ha.png" alt="High availability options for persistent storage"/>

<table summary="The table shows persistent storage options. Rows are to be read from the left to right, with the number of the option in column one, the title of the otion in column two and a description in column three.">
<caption>Table. Persistent storage options</caption>
  <thead>
  <th>Option</th>
  <th>Description</th>
  </thead>
  <tbody>
  <tr>
  <td>1. NFS file storage</td>
  <td>With this option, you can persist app and container data by using Kubernetes persistent volumes. Volumes are hosted on [Endurance and Performance NFS-based file storage ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/cloud/file-storage/details) which can be used for apps that store data on a file basis rather than in a database. File storage is encrypted at REST.<p>{{site.data.keyword.containershort_notm}} provides predefined storage classes that define the range of sizes of the storage, IOPS, the delete policy, and the read and write permissions for the volume. To initiate a request for NFS-based file storage, you must create a [persistent volume claim (PVC)](cs_storage.html#create). After you submit a PVC, {{site.data.keyword.containershort_notm}} dynamically provisions a persistent volume that is hosted on NFS-based file storage. [You can mount the PVC](cs_storage.html#app_volume_mount) as a volume to your deployment to allow the containers to read from and write to the volume. </p><p>Persistent volumes are provisioned in the data center where the worker node is located. You can share data across the same replica set or with other deployments in the same cluster. You cannot share data across clusters when they are located in different data centers or regions. </p><p>By default, NFS storage is not backed up automatically. You can set up a periodic backup for your cluster by using the provided [backup and restore mechanisms](cs_storage.html#backup_restore). When a container crashes or a pod is removed from a worker node, the data is not removed and can still be accessed by other deployments that mount the volume. </p><p><strong>Note:</strong> Persistent NFS file share storage is charged on a monthly basis. If you provision persistent storage for your cluster and remove it immediately, you still pay the monthly charge for the persistent storage, even if you used it only for a short amount of time.</p></td>
  </tr>
  <tr>
    <td>2. Cloud database service</td>
    <td>With this option, you can persist data by using an {{site.data.keyword.Bluemix_notm}} database cloud service, such as [IBM Cloudant NoSQL DB](/docs/services/Cloudant/getting-started.html#getting-started-with-cloudant). Data that is stored with this option can be accessed across clusters, locations, and regions. <p> You can choose to configure a single database instance that all your apps access, or to [set up multiple instances across data centers and replication](/docs/services/Cloudant/guides/active-active.html#configuring-cloudant-nosql-db-for-cross-region-disaster-recovery) between the instances for higher availability. In IBM Cloudant NoSQL database, data is not backed up automatically. You can use the provided [backup and restore mechanisms](/docs/services/Cloudant/guides/backup-cookbook.html#cloudant-nosql-db-backup-and-recovery) to protect your data from a site failure.</p> <p> To use a service in your cluster, you must [bind the {{site.data.keyword.Bluemix_notm}} service](cs_integrations.html#adding_app) to a namespace in your cluster. When you bind the service to the cluster, a Kubernetes secret is created. The Kubernetes secret holds confidential information about the service, such as the URL to the service, your user name, and password. You can mount the secret as a secret volume to your pod and access the service by using the credentials in the secret. By mounting the secret volume to other pods, you can also share data between pods. When a container crashes or a pod is removed from a worker node, the data is not removed and can still be accessed by other pods that mount the secret volume. <p>Most {{site.data.keyword.Bluemix_notm}} database services provide disk space for a small amount of data at no cost, so you can test its features.</p></td>
  </tr>
  <tr>
    <td>3. On-prem database</td>
    <td>If your data must be stored on-site for legal reasons, you can [set up a VPN connection](cs_vpn.html#vpn) to your on-premise database and use existing storage, backup and replication mechanisms in your data center.</td>
  </tr>
  </tbody>
  </table>

{: caption="Table. Persistent data storage options for deployments in Kubernetes clusters" caption-side="top"}

<br />



## Using existing NFS file shares in clusters
{: #existing}

If you already have existing NFS file shares in your IBM Cloud infrastructure (SoftLayer) account that you want to use with Kubernetes, you can do so by creating a persistent volume (PV) for your existing storage.
{:shortdesc}

A persistent volume (PV) is a Kubernetes resource that represents an actual storage device that is provisioned in a data center. Persistent volumes abstract the details of how a specific storage type is provisioned by IBM Cloud Storage. To mount a PV to your cluster, you must request persistent storage for your pod by creating a persistent volume claim (PVC). The following diagram illustrates the relationship between PVs and PVCs.

![Create persistent volumes and persistent volume claims](images/cs_cluster_pv_pvc.png)

 As depicted in the diagram, to enable existing NFS file shares to be used with Kubernetes, you must create PVs with a certain size and access mode and create a PVC that matches the PV specification. If the PV and PVC match, they are bound to each other. Only bound PVCs can be used by the cluster user to mount the volume to a deployment. This process is referred to as static provisioning of persistent storage.

Before you begin, make sure that you have an existing NFS file share that you can use to create your PV. For example, if you previously [created a PVC with a `retain` storage class policy](#create), you can use that retained data in the existing NFS file share for this new PVC.

**Note:** Static provisioning of persistent storage only applies to existing NFS file shares. If you do not have existing NFS file shares, cluster users can use the [dynamic provisioning](cs_storage.html#create) process to add PVs.

To create a PV and matching PVC, follow these steps.

1.  In your IBM Cloud infrastructure (SoftLayer) account, look up the ID and path of the NFS file share where you want to create your PV object. In addition, authorize the file storage to the subnets in the cluster. This authorization gives your cluster access to the storage.
    1.  Log in to your IBM Cloud infrastructure (SoftLayer) account.
    2.  Click **Storage**.
    3.  Click **File Storage** and from the **Actions** menu, select **Authorize Host**.
    4.  Select **Subnets**.
    5.  From the drop down list, select the private VLAN subnet that your worker node is connected to. To find the subnet of your worker node, run `bx cs workers <cluster_name>` and compare the `Private IP` of your worker node with the subnet that you found in the drop down list.
    6.  Click **Submit**.
    6.  Click the name of the file storage.
    7.  Make note the **Mount Point** field. The field is displayed as `<server>:/<path>`.
2.  Create a storage configuration file for your PV. Include the server and path from the file storage **Mount Point** field.

    ```
    apiVersion: v1
    kind: PersistentVolume
    metadata:
     name: mypv
    spec:
     capacity:
       storage: "20Gi"
     accessModes:
       - ReadWriteMany
     nfs:
       server: "nfslon0410b-fz.service.networklayer.com"
       path: "/IBM01SEV8491247_0908/data01"
    ```
    {: codeblock}

    <table>
    <caption>Table. Understanding the YAML file components</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the YAML file components</th>
    </thead>
    <tbody>
    <tr>
    <td><code>name</code></td>
    <td>Enter the name of the PV object that you want to create.</td>
    </tr>
    <tr>
    <td><code>spec/capacity/storage</code></td>
    <td>Enter the storage size of the existing NFS file share. The storage size must be written in gigabytes, for example, 20Gi (20 GB) or 1000Gi (1 TB), and the size must match the size of the existing file share.</td>
    </tr>
    <tr>
    <td><code>accessMode</code></td>
    <td>Access modes define the way that the PVC can be mounted to a worker node.<ul><li>ReadWriteOnce (RWO): The PV can be mounted to deployments in a single worker node only. Containers in deployments that are mounted to this PV can read from and write to the volume.</li><li>ReadOnlyMany (ROX): The PV can be mounted to deployments that are hosted on multiple worker nodes. Deployments that are mounted to this PV can only read from the volume.</li><li>ReadWriteMany (RWX): This PV can be mounted to deployments that are hosted on multiple worker nodes. Deployments that are mounted to this PV can read from and write to the volume.</li></ul></td>
    </tr>
    <tr>
    <td><code>spec/nfs/server</code></td>
    <td>Enter the NFS file share server ID.</td>
    </tr>
    <tr>
    <td><code>path</code></td>
    <td>Enter the path to the NFS file share where you want to create the PV object.</td>
    </tr>
    </tbody></table>

3.  Create the PV object in your cluster.

    ```
    kubectl apply -f <yaml_path>
    ```
    {: pre}

    Example

    ```
    kubectl apply -f deploy/kube-config/pv.yaml
    ```
    {: pre}

4.  Verify that the PV is created.

    ```
    kubectl get pv
    ```
    {: pre}

5.  Create another configuration file to create your PVC. In order for the PVC to match the PV object that you created earlier, you must choose the same value for `storage` and `accessMode`. The `storage-class` field must be empty. If any of these fields do not match the PV, then a new PV is created automatically instead.

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
         storage: "20Gi"
    ```
    {: codeblock}

6.  Create your PVC.

    ```
    kubectl apply -f deploy/kube-config/mypvc.yaml
    ```
    {: pre}

7.  Verify that your PVC is created and bound to the PV object. This process can take a few minutes.

    ```
    kubectl describe pvc mypvc
    ```
    {: pre}

    Your output looks similar to the following.

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


You successfully created a PV object and bound it to a PVC. Cluster users can now [mount the PVC](#app_volume_mount) to their deployments and start reading from and writing to the PV object.

<br />




## Adding NFS file storage to apps
{: #create}

Create a persistent volume claim (PVC) to provision NFS file storage for your cluster. Then, mount this claim to a persistent volume (PV) to ensure that data is available even if the pods crash or shut down.
{:shortdesc}

The NFS file storage that backs the PV is clustered by IBM in order to provide high availability for your data. The storage classes describe the types of storage offerings available and define aspects such as the data retention policy, size in gigabytes, and IOPS when you create your PV.



**Before you begin**: If you have a firewall, [allow egress access](cs_firewall.html#pvc) for the IBM Cloud infrastructure (SoftLayer) IP ranges of the locations (data centers) that your clusters are in, so that you can create PVCs.

To add persistent storage:

1.  Review the available storage classes. {{site.data.keyword.containerlong}} provides pre-defined storage classes for NFS file storage so that the cluster admin does not have to create any storage classes. The `ibmc-file-bronze` storage class is the same as the `default` storage class.

    ```
    kubectl get storageclasses
    ```
    {: pre}

    ```
    $ kubectl get storageclasses
    NAME                         TYPE
    default                      ibm.io/ibmc-file
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

    **Tip:** If you want to change the default storage class, run `kubectl patch storageclass <storageclass> -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'` and replace `<storageclass>` with the name of the storage class.

2.  Decide if you want to keep your data and the NFS file share after you delete the PVC.
    - If you want to keep your data, then choose a `retain` storage class. When you delete the PVC, the PV is removed, but the NFS file and your data still exist in your IBM Cloud infrastructure (SoftLayer) account. Later, to access this data in your cluster, create a PVC and a matching PV that refers to your existing [NFS file](#existing).
    - If you want the data and your NFS file share to be deleted when you delete the PVC, choose a storage class without `retain`.

3.  **If you choose a bronze, silver, or gold storage class**: You get [Endurance storage ![External link icon](../icons/launch-glyph.svg "External link icon")](https://knowledgelayer.softlayer.com/topic/endurance-storage) that defines the IOPS per GB for each class. However, you can determine the total IOPS by choosing a size within the available range. You can select any whole number of gigabyte sizes within the allowed size range (such as 20 Gi, 256 Gi, 11854 Gi). For example, if you select a 1000Gi file share size in the silver storage class of 4 IOPS per GB, your volume has a total of 4000 IOPS. The more IOPS your PV has, the faster it processes input and output operations. The following table describes the IOPS per gigabyte and size range for each storage class.

    <table>
         <caption>Table of storage class size ranges and IOPS per gigabyte</caption>
         <thead>
         <th>Storage class</th>
         <th>IOPS per gigabyte</th>
         <th>Size range in gigabytes</th>
         </thead>
         <tbody>
         <tr>
         <td>Bronze (default)</td>
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

    <p>**Example command to show the details of a storage class**:</p>

    <pre class="pre"><code>kubectl describe storageclasses ibmc-file-silver</code></pre>

4.  **If you choose the custom storage class**: You get [Performance storage ![External link icon](../icons/launch-glyph.svg "External link icon")](https://knowledgelayer.softlayer.com/topic/performance-storage) and have more control over choosing the combination of IOPS and size. For example, if you select a size of 40Gi for your PVC, you can choose IOPS that is a multiple of 100 that is in the range of 100 - 2000 IOPS. The following table shows you what range of IOPS you can choose depending on the size that you select.

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

    <p>**Example command to show the details for a custom storage class**:</p>

    <pre class="pre"><code>kubectl describe storageclasses ibmc-file-retain-custom</code></pre>

5.  Decide whether you want to be billed on an hourly or monthly basis. By default, you are billed monthly.

6.  Create a configuration file to define your PVC and save the configuration as a `.yaml` file.

    -  **Example for bronze, silver, gold storage classes**:
       The following `.yaml` file creates a claim that is named `mypvc` of the `"ibmc-file-silver"` storage class, billed `"hourly"`, with a gigabyte size of `24Gi`. 

       ```
       apiVersion: v1
       kind: PersistentVolumeClaim
       metadata:
         name: mypvc
         annotations:
           volume.beta.kubernetes.io/storage-class: "ibmc-file-silver"
         labels:
           billingType: "hourly"
       spec:
         accessModes:
           - ReadWriteMany
         resources:
           requests:
             storage: 24Gi
        ```
        {: codeblock}

    -  **Example for custom storage classes**:
       The following `.yaml` file creates a claim that is named `mypvc` of the storage class `ibmc-file-retain-custom`, billed at the default of `"monthly"`, with a gigabyte size of `45Gi` and IOPS of `"300"`.

       ```
       apiVersion: v1
       kind: PersistentVolumeClaim
       metadata:
         name: mypvc
         annotations:
           volume.beta.kubernetes.io/storage-class: "ibmc-file-retain-custom"
         labels:
           billingType: "monthly"
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
	      <caption>Table. Understanding the YAML file components</caption>
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
        <td>Specify the storage class for the PV:
          <ul>
          <li>ibmc-file-bronze / ibmc-file-retain-bronze : 2 IOPS per GB.</li>
          <li>ibmc-file-silver / ibmc-file-retain-silver: 4 IOPS per GB.</li>
          <li>ibmc-file-gold / ibmc-file-retain-gold: 10 IOPS per GB.</li>
          <li>ibmc-file-custom / ibmc-file-retain-custom: Multiple values of IOPS available.</li></ul>
          <p>If you do not specify a storage class, the PV is created with the default storage class.</p><p>**Tip:** If you want to change the default storage class, run <code>kubectl patch storageclass &lt;storageclass&gt; -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'</code> and replace <code>&lt;storageclass&gt;</code> with the name of the storage class.</p></td>
        </tr>
        <tr>
          <td><code>metadata/labels/billingType</code></td>
          <td>Specify the frequency for which your storage bill is calculated, "monthly" or "hourly". The default is "monthly".</td>
        </tr>
        <tr>
        <td><code>spec/resources/requests/storage</code></td>
        <td>Enter the size of the file storage, in gigabytes (Gi). Choose a whole number within the allowable size range. </br></br><strong>Note: </strong> After your storage is provisioned, you cannot change the size of your NFS file share. Make sure to specify a size that matches the amount of data that you want to store. </td>
        </tr>
        <tr>
        <td><code>spec/resources/requests/iops</code></td>
        <td>This option is for custom storage classes only (`ibmc-file-custom / ibmc-file-retain-custom`). Specify the total IOPS for the storage, selecting a multiple of 100 within the allowable range. To see all options, run `kubectl describe storageclasses <storageclass>`. If you choose an IOPS other than one that is listed, the IOPS is rounded up.</td>
        </tr>
        </tbody></table>

7.  Create the PVC.

    ```
    kubectl apply -f <local_file_path>
    ```
    {: pre}

8.  Verify that your PVC is created and bound to the PV. This process can take a few minutes.

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

9.  {: #app_volume_mount}To mount the PVC to your deployment, create a configuration `.yaml` file.

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
    <caption>Table. Understanding the YAML file components</caption>
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
    <td>The name of the image that you want to use. To list available images in your {{site.data.keyword.registryshort_notm}} account, run `bx cr image-list`.</td>
    </tr>
    <tr>
    <td><code>spec/containers/name</code></td>
    <td>The name of the container that you want to deploy to your cluster.</td>
    </tr>
    <tr>
    <td><code>spec/containers/volumeMounts/mountPath</code></td>
    <td>The absolute path of the directory to where the volume is mounted inside the container.</td>
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
    <td>The name of the PVC that you want to use as your volume. When you mount the volume to the pod, Kubernetes identifies the PV that is bound to the PVC and enables the user to read from and write to the PV.</td>
    </tr>
    </tbody></table>

10.  Create the deployment and mount the PVC.
     ```
     kubectl apply -f <local_yaml_path>
     ```
     {: pre}

11.  Verify that the volume is successfully mounted.

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




## Setting up backup and restore solutions for NFS file shares
{: #backup_restore}

File shares are provisioned into the same location (data center) as your cluster and are clustered by {{site.data.keyword.IBM_notm}} to provide high availability. However, file shares are not backed up automatically and might be inaccessible if the entire location fails. To protect your data from being lost or damaged, you can set up periodic backups of your NFS file shares that you can use to restore your data when needed.
{: shortdesc}

Review the following backup and restore options for your NFS file shares:

<dl>
  <dt>Set up periodic snapshots of your NFS file share</dt>
  <dd>You can set up [periodic snapshots](/docs/infrastructure/FileStorage/snapshots.html) for your NFS file share, which is a read-only image of an NFS file share that captures the state of the volume at a point in time. Snapshots are stored on the same file share within the same location. You can restore data from a snapshot if a user accidentally removes important data from the volume.</dd>
  <dt>Replicate snapshots to an NFS file share in another location (data center)</dt>
 <dd>To protect your data from a location failure, you can [replicate snapshots](/docs/infrastructure/FileStorage/replication.html#working-with-replication) to an NFS file share that is set up in another location. Data can be replicated from the primary NFS file share to the backup NFS file share only. You cannot mount a replicated NFS file share to a cluster. When your primary NFS file share fails, you can manually set your backup NFS file share to be the primary one. Then, you can mount it to your cluster. After your primary NFS file share is restored, you can restore the data from the backup NFS file share.</dd>
  <dt>Backup data to Object Storage</dt>
  <dd>You can use the [**ibm-backup-restore image**](/docs/services/RegistryImages/ibm-backup-restore/index.html#ibmbackup_restore_starter) to spin up a backup and restore pod in your cluster. This pod contains a script to run a one-time or periodic backup for any persistent volume claim (PVC) in your cluster. Data is stored in your {{site.data.keyword.objectstoragefull}} instance that you set up in a location. To make your data even more highly available and protect your app from a location failure, set up a second {{site.data.keyword.objectstoragefull}} instance and replicate data across locations. If you need to restore data from your {{site.data.keyword.objectstoragefull}} instance, use the restore script that is provided with the image.</dd>
  </dl>

## Adding non-root user access to NFS file storage
{: #nonroot}

By default, non-root users do not have write permission on the volume mount path for NFS-backed storage. Some common images, such as Jenkins and Nexus3, specify a non-root user that owns the mount path in the Dockerfile. When you create a container from this Dockerfile, the creation of the container fails due to insufficient permissions of the non-root user on the mount path. To grant write permission, you can modify the Dockerfile to temporarily add the non-root user to the root user group before changing the mount path permissions, or use an init container.
{:shortdesc}

If you are using a Helm chart to deploy an image with a non-root user that you want to have write permissions on the NFS file share, first edit the Helm deployment to use an init container.
{:tip}



When you include an [init container![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/concepts/workloads/pods/init-containers/) in your deployment, you can give a non-root user that is specified in your Dockerfile write permissions to the volume mount path inside the container that points to your NFS file share. The init container starts before your app container starts. The init container creates the volume mount path inside the container, changes the mount path to be owned by the correct (non-root) user, and closes. Then, your app container starts, which includes the non-root user that must write to the mount path. Because the path is already owned by the non-root user, writing to the mount path is successful. If you do not want to use an init container, you can modify the Dockerfile to add non-root user access to NFS file storage.

Before you begin, [target your CLI](cs_cli_install.html#cs_cli_configure) to your cluster.

1.  Open the Dockerfile for your app and get the user ID (UID) and group ID (GID) from the user that you want to give writer permission on the volume mount path. In the example from a Jenkins Dockerfile, the information is:
    - UID: `1000`
    - GID: `1000`

    **Example**:

    ```
    FROM openjdk:8-jdk

    RUN apt-get update && apt-get install -y git curl && rm -rf /var/lib/apt/lists/*

    ARG user=jenkins
    ARG group=jenkins
    ARG uid=1000
    ARG gid=1000
    ARG http_port=8080
    ARG agent_port=50000

    ENV JENKINS_HOME /var/jenkins_home
    ENV JENKINS_SLAVE_AGENT_PORT ${agent_port}
    ...
    ```
    {:screen}

2.  Add persistent storage to your app by creating a persistent volume claim (PVC). This example uses the `ibmc-file-bronze` storage class. To review available storage classes, run `kubectl get storageclasses`.

    ```
    apiVersion: v1
    kind: PersistentVolumeClaim
    metadata:
      name: mypvc
      annotations:
        volume.beta.kubernetes.io/storage-class: "ibmc-file-bronze"
    spec:
      accessModes:
        - ReadWriteMany
      resources:
        requests:
          storage: 20Gi
    ```
    {: codeblock}

3.  Create the PVC.

    ```
    kubectl apply -f <local_file_path>
    ```
    {: pre}

4.  In your deployment `.yaml` file, add the init container. Include the UID and GID that you previously retrieved.

    ```
    initContainers:
    - name: initContainer # Or you can replace with any name
      image: alpine:latest
      command: ["/bin/sh", "-c"]
      args:
        - chown <UID>:<GID> /mount; # Replace UID and GID with values from the Dockerfile
      volumeMounts:
      - name: volume # Or you can replace with any name
        mountPath: /mount # Must match the mount path in the args line
    ```
    {: codeblock}

    **Example** for a Jenkins deployment:

    ```
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: my_pod
    spec:
      replicas: 1
      template:
        metadata:
          labels:
            app: jenkins
        spec:
          containers:
          - name: jenkins
            image: jenkins
            volumeMounts:
            - mountPath: /var/jenkins_home
              name: volume
          volumes:
          - name: volume
            persistentVolumeClaim:
              claimName: mypvc
          initContainers:
          - name: permissionsfix
            image: alpine:latest
            command: ["/bin/sh", "-c"]
            args:
              - chown 1000:1000 /mount;
            volumeMounts:
            - name: volume
              mountPath: /mount
    ```
    {: codeblock}

5.  Create the pod and mount the PVC to your pod.

    ```
    kubectl apply -f <local_yaml_path>
    ```
    {: pre}

6. Verify that the volume is successfully mounted to your pod. Note the pod name and **Containers/Mounts** path.

    ```
    kubectl describe pod <my_pod>
    ```
    {: pre}

    **Example output**:

    ```
    Name:		    mypod-123456789
    Namespace:	default
    ...
    Init Containers:
    ...
    Mounts:
      /mount from volume (rw)
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-cp9f0 (ro)
    ...
    Containers:
      jenkins:
        Container ID:
        Image:		jenkins
        Image ID:
        Port:		  <none>
        State:		Waiting
          Reason:		PodInitializing
        Ready:		False
        Restart Count:	0
        Environment:	<none>
        Mounts:
          /var/jenkins_home from volume (rw)
          /var/run/secrets/kubernetes.io/serviceaccount from default-token-cp9f0 (ro)
    ...
    Volumes:
      myvol:
        Type:	PersistentVolumeClaim (a reference to a PersistentVolumeClaim in the same namespace)
        ClaimName:	mypvc
        ReadOnly:	  false

    ```
    {: screen}

7.  Log in to the pod by using the pod name that you previously noted.

    ```
    kubectl exec -it <my_pod-123456789> /bin/bash
    ```
    {: pre}

8. Verify the permissions of your container's mount path. In the example, the mount path is `/var/jenkins_home`.

    ```
    ls -ln /var/jenkins_home
    ```
    {: pre}

    **Example output**:

    ```
    jenkins@mypod-123456789:/$ ls -ln /var/jenkins_home
    total 12
    -rw-r--r-- 1 1000 1000  102 Mar  9 19:58 copy_reference_file.log
    drwxr-xr-x 2 1000 1000 4096 Mar  9 19:58 init.groovy.d
    drwxr-xr-x 9 1000 1000 4096 Mar  9 20:16 war
    ```
    {: screen}

    This output shows that the GID and UID from your Dockerfile (in this example, `1000` and `1000`) own the mount path inside the container.


