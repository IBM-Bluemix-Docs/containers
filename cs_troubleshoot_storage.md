---

copyright:
  years: 2014, 2018
lastupdated: "2018-12-19"

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
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}



# Troubleshooting cluster storage
{: #cs_troubleshoot_storage}

As you use {{site.data.keyword.containerlong}}, consider these techniques for troubleshooting cluster storage.
{: shortdesc}

If you have a more general issue, try out [cluster debugging](cs_troubleshoot.html).
{: tip}

## In a multizone cluster, a persistent volume fails to mount to a pod
{: #mz_pv_mount}

{: tsSymptoms}
Your cluster was previously a single-zone cluster with stand-alone worker nodes that were not in worker pools. You successfully mounted a persistent volume claim (PVC) that described the persistent volume (PV) to use for your app's pod deployment. Now that you have worker pools and added zones to your cluster, however, the PV fails to mount to a pod.

{: tsCauses}
For multizone clusters, PVs must have the following labels so that pods do not try to mount volumes in a different zone.
* `failure-domain.beta.kubernetes.io/region`
* `failure-domain.beta.kubernetes.io/zone`

New clusters with worker pools that can span multiple zones label the PVs by default. If you created your clusters before worker pools were introduced, you must add the labels manually.

{: tsResolve}
[Update the PVs in your cluster with the region and zone labels](cs_storage_basics.html#multizone).

<br />


## File storage: File systems for worker nodes change to read-only
{: #readonly_nodes}

{: tsSymptoms}
{: #stuck_creating_state}
You might see one of the following symptoms:
- When you run `kubectl get pods -o wide`, you see that multiple pods that are running on the same worker node are stuck in the `ContainerCreating` state.
- When you run a `kubectl describe` command, you see the following error in the **Events** section: `MountVolume.SetUp failed for volume ... read-only file system`.

{: tsCauses}
The file system on the worker node is read-only.

{: tsResolve}
1.  Back up any data that might be stored on the worker node or in your containers.
2.  For a short-term fix to the existing worker node, reload the worker node.
    <pre class="pre"><code>ibmcloud ks worker-reload &lt;cluster_name&gt; &lt;worker_ID&gt;</code></pre>

For a long-term fix, [update the machine type of your worker pool](cs_cluster_update.html#machine_type).

<br />



## File storage: App fails when a non-root user owns the NFS file storage mount path
{: #nonroot}

{: tsSymptoms}
After you [add NFS storage](cs_storage_file.html#app_volume_mount) to your deployment, the deployment of your container fails. When you retrieve the logs for your container, you might see errors such as "write-permission" or "do not have required permission". The pod fails and is stuck in a reload cycle.

{: tsCauses}
By default, non-root users do not have write permission on the volume mount path for NFS-backed storage. Some common app images, such as Jenkins and Nexus3, specify a non-root user that owns the mount path in the Dockerfile. When you create a container from this Dockerfile, the creation of the container fails due to insufficient permissions of the non-root user on the mount path. To grant write permission, you can modify the Dockerfile to temporarily add the non-root user to the root user group before it changes the mount path permissions, or use an init container.

If you use a Helm chart to deploy the image, edit the Helm deployment to use an init container.
{:tip}



{: tsResolve}
When you include an [init container![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/concepts/workloads/pods/init-containers/) in your deployment, you can give a non-root user that is specified in your Dockerfile write permissions for the volume mount path inside the container. The init container starts before your app container starts. The init container creates the volume mount path inside the container, changes the mount path to be owned by the correct (non-root) user, and closes. Then, your app container starts with the non-root user that must write to the mount path. Because the path is already owned by the non-root user, writing to the mount path is successful. If you do not want to use an init container, you can modify the Dockerfile to add non-root user access to NFS file storage.


Before you begin: [Log in to your account. Target the appropriate region and, if applicable, resource group. Set the context for your cluster](cs_cli_install.html#cs_cli_configure).

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
    kubectl apply -f mypvc.yaml
    ```
    {: pre}

4.  In your deployment `.yaml` file, add the init container. Include the UID and GID that you previously retrieved.

    ```
    initContainers:
    - name: initcontainer # Or you can replace with any name
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
    kubectl apply -f my_pod.yaml
    ```
    {: pre}

6. Verify that the volume is successfully mounted to your pod. Note the pod name and **Containers/Mounts** path.

    ```
    kubectl describe pod <my_pod>
    ```
    {: pre}

    **Example output**:

    ```
    Name:       mypod-123456789
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

<br />


## File storage: Adding non-root user access to persistent storage fails
{: #cs_storage_nonroot}

{: tsSymptoms}
After you [add non-root user access to persistent storage](#nonroot) or deploying a Helm chart with a non-root user ID specified, the user cannot write to the mounted storage.

{: tsCauses}
The deployment or Helm chart configuration specifies the [security context](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/) for the pod's `fsGroup` (group ID) and `runAsUser` (user ID). Currently, {{site.data.keyword.containerlong_notm}} does not support the `fsGroup` specification, and supports only `runAsUser` set as `0` (root permissions).

{: tsResolve}
Remove the configuration's `securityContext` fields for `fsGroup` and `runAsUser` from the image, deployment, or Helm chart configuration file and redeploy. If you need to change the ownership of the mount path from `nobody`, [add non-root user access](#nonroot). After you add the [non-root initContainer](#nonroot), set `runAsUser` at the container level, not the pod level.

<br />




## Block storage: Mounting existing block storage to a pod fails due to the wrong file system
{: #block_filesystem}

{: tsSymptoms}
When you run `kubectl describe pod <pod_name>`, you see the following error:
```
failed to mount the volume as "ext4", it already contains xfs. Mount error: mount failed: exit status 32
```
{: screen}

{: tsCauses}
You have an existing block storage device that is set up with an `XFS` file system. To mount this device to your pod, you [created a PV](cs_storage_block.html#existing_block) that specified `ext4` as your file system or no file system in the `spec/flexVolume/fsType` section. If no file system is defined, the PV defaults to `ext4`.
The PV was created successfully and was linked to your existing block storage instance. However, when you try to mount the PV to your cluster by using a matching PVC, the volume fails to mount. You cannot mount your `XFS` block storage instance with an `ext4` file system to the pod.

{: tsResolve}
Update the file system in the existing PV from `ext4` to `XFS`.

1. List the existing PVs in your cluster and note the name of the PV that you used for your existing block storage instance.
   ```
   kubectl get pv
   ```
   {: pre}

2. Save the PV yaml on your local machine.
   ```
   kubectl get pv <pv_name> -o yaml > <filepath/xfs_pv.yaml>
   ```
   {: pre}

3. Open the yaml file and change the `fsType` from `ext4` to `xfs`.
4. Replace the PV in your cluster.
   ```
   kubectl replace --force -f <filepath/xfs_pv.yaml>
   ```
   {: pre}

5. Log in to the pod where you mounted the PV.
   ```
   kubectl exec -it <pod_name> sh
   ```
   {: pre}

6. Verify that the file system changed to `XFS`.
   ```
   df -Th
   ```
   {: pre}

   Example output:
   ```
   Filesystem Type Size Used Avail Use% Mounted on /dev/mapper/3600a098031234546d5d4c9876654e35 xfs 20G 33M 20G 1% /myvolumepath
   ```
   {: screen}

<br />



## Object storage: Installing the {{site.data.keyword.cos_full_notm}} `ibmc` Helm plug-in fails
{: #cos_helm_fails}

{: tsSymptoms}
When you install the {{site.data.keyword.cos_full_notm}} `ibmc` Helm plug-in, the installation fails with the following error:
```
Error: symlink /Users/ibm/ibmcloud-object-storage-plugin/helm-ibmc /Users/ibm/.helm/plugins/helm-ibmc: file exists
```
{: screen}

{: tsCauses}
When the `ibmc` Helm plug-in is installed, a symlink is created from the `./helm/plugins/helm-ibmc` directory to the directory where the `ibmc` Helm plug-in is located on your local system, which is usually in `./ibmcloud-object-storage-plugin/helm-ibmc`. When you remove the `ibmc` Helm plug-in from your local system, or you move the `ibmc` Helm plug-in directory to a different location, the symlink is not removed.

{: tsResolve}
1. Remove the {{site.data.keyword.cos_full_notm}} Helm plug-in.
   ```
   rm -rf ~/.helm/plugins/helm-ibmc
   ```
   {: pre}

2. [Install the {{site.data.keyword.cos_full_notm}}](cs_storage_cos.html#install_cos).

<br />


## Object storage: PVC or pod creation fails due to not finding the Kubernetes secret
{: #cos_secret_access_fails}

{: tsSymptoms}
When you create your PVC or deploy a pod that mounts the PVC, the creation or deployment fails.

- Example error message for a PVC creation failure:
  ```
  pvc-3:1b23159vn367eb0489c16cain12345:cannot get credentials: cannot get secret tsecret-key: secrets "secret-key" not found
  ```
  {: screen}

- Example error message for a pod creation failure:
  ```
  persistentvolumeclaim "pvc-3" not found (repeated 3 times)
  ```
  {: screen}

{: tsCauses}
The Kubernetes secret where you store your {{site.data.keyword.cos_full_notm}} service credentials, the PVC, and the pod are not all in the same Kubernetes namespace. When the secret is deployed to a different namespace than your PVC or pod, the secret cannot be accessed.

{: tsResolve}


1. List the secrets in your cluster and review the Kubernetes namespace where the Kubernetes secret for your {{site.data.keyword.cos_full_notm}} service instance is created. The secret must show `ibm/ibmc-s3fs` as the **Type**.
   ```
   kubectl get secrets --all-namespaces
   ```
   {: pre}

2. Check your YAML configuration file for your PVC and pod to verify that you used the same namespace. If you want to deploy a pod in a different namespace than the one where your secret exists, [create another secret](cs_storage_cos.html#create_cos_secret) in the desired namespace.

3. Create the PVC or deploy the pod in the desired namespace.

<br />


## Object storage: PVC creation fails due to wrong credentials or access denied
{: #cred_failure}

{: tsSymptoms}
When you create the PVC, you see an error message similar to one of the following:

```
SignatureDoesNotMatch: The request signature we calculated does not match the signature you provided. Check your AWS Secret Access Key and signing method. For more information, see REST Authentication and SOAP Authentication for details.
```
{: screen}

```
AccessDenied: Access Denied status code: 403
```
{: screen}

```
CredentialsEndpointError: failed to load credentials
```
{: screen}

{: tsCauses}
The {{site.data.keyword.cos_full_notm}} service credentials that you use to access the service instance might be wrong, or allow only read access to your bucket.

{: tsResolve}
1. In the navigation on the service details page, click **Service Credentials**.
2. Find your credentials, then click **View credentials**.
3. Verify that you use the correct **access_key_id** and **secret_access_key** in your Kubernetes secret. If not, update your Kubernetes secret.
   1. Get the YAML that you used to create the secret.
      ```
      kubectl get secret <secret_name> -o yaml
      ```
      {: pre}

   2. Update the **access_key_id** and **secret_access_key**.
   3. Update the secret.
      ```
      kubectl apply -f secret.yaml
      ```
      {: pre}

4. In the **iam_role_crn** section, verify that you have the `Writer` or `Manager` role. If you do not have the correct role, you must [create new {{site.data.keyword.cos_full_notm}} service credentials with the correct permission](cs_storage_cos.html#create_cos_service). Then, update your existing secret or [create a new secret](cs_storage_cos.html#create_cos_secret) with your new service credentials.

<br />


## Object storage: Cannot access an existing bucket

{: tsSymptoms}
When you create the PVC, the bucket in {{site.data.keyword.cos_full_notm}} cannot be accessed. You see an error message similar to the following:

```
Failed to provision volume with StorageClass "ibmc-s3fs-standard-regional": pvc:1b2345678b69175abc98y873e2:cannot access bucket <bucket_name>: NotFound: Not Found
```
{: screen}

{: tsCauses}
You might have used the wrong storage class to access your existing bucket, or you tried to access a bucket that you did not create.

{: tsResolve}
1. From the [{{site.data.keyword.Bluemix_notm}} dashboard ![External link icon](../icons/launch-glyph.svg "External link icon")](https://console.bluemix.net/dashboard/apps), select your {{site.data.keyword.cos_full_notm}} service instance.
2. Select **Buckets**.
3. Review the **Class** and **Location** information for your existing bucket.
4. Choose the appropriate [storage class](cs_storage_cos.html#storageclass_reference).

<br />


## Object storage: Accessing files with a non-root user fails
{: #cos_nonroot_access}

{: tsSymptoms}
You uploaded files to your {{site.data.keyword.cos_full_notm}} service instance by using the console or the REST API. When you try to access these files with a non-root user that you defined with `runAsUser` in your app deployment, access to the files is denied.

{: tsCauses}
In Linux, a file or a directory has 3 access groups: `Owner`, `Group`, and `Other`. When you upload a file to {{site.data.keyword.cos_full_notm}} by using the console or the REST API, the permissions for the `Owner`, `Group`, and `Other` are removed. The permission of each file looks as follows:

```
d--------- 1 root root 0 Jan 1 1970 <file_name>
```
{: screen}

When you upload a file by using the {{site.data.keyword.cos_full_notm}} plug-in, the permissions for the file are preserved and not changed.

{: tsResolve}
To access the file with a non-root user, the non-root user must have read and write permissions for the file. Changing the permission on a file as part of your pod deployment requires a write operation. {{site.data.keyword.cos_full_notm}} is not designed for write workloads. Updating permissions during the pod deployment might prevent your pod from getting into a `Running` state.

To resolve this issue, before you mount the PVC to your app pod, create another pod to set the correct permission for the non-root user.

1. Check the permissions of your files in your bucket.
   1. Create a configuration file for your `test-permission` pod and name the file `test-permission.yaml`.
      ```
      apiVersion: v1
      kind: Pod
      metadata:
        name: test-permission
      spec:
        containers:
        - name: test-permission
          image: nginx
          volumeMounts:
          - name: cos-vol
            mountPath: /test
        volumes:
        - name: cos-vol
          persistentVolumeClaim:
            claimName: <pvc_name>
      ```
      {: codeblock}

   2. Create the `test-permission` pod.
      ```
      kubectl apply -f test-permission.yaml
      ```
      {: pre}

   3. Log in to your pod.
      ```
      kubectl exec test-permission -it bash
      ```
      {: pre}

   4. Navigate to your mount path and list the permissions for your files.
      ```
      cd test && ls -al
      ```
      {: pre}

      Example output:
      ```
      d--------- 1 root root 0 Jan 1 1970 <file_name>
      ```
      {: screen}

2. Delete the pod.
   ```
   kubectl delete pod test-permission
   ```
   {: pre}

3. Create a configuration file for the pod that you use to correct the permissions of your files and name it `fix-permission.yaml`.
   ```
   apiVersion: v1
   kind: Pod
   metadata:
     name: fix-permission
     namespace: <namespace>
   spec:
     containers:
     - name: fix-permission
       image: busybox
       command: ['sh', '-c']
       args: ['chown -R <nonroot_userID> <mount_path>/*; find <mount_path>/ -type d -print -exec chmod u=+rwx,g=+rx {} \;']
       volumeMounts:
       - mountPath: "<mount_path>"
         name: cos-volume
     volumes:
     - name: cos-volume
       persistentVolumeClaim:
         claimName: <pvc_name>
    ```
    {: codeblock}

3. Create the `fix-permission` pod.
   ```
   kubectl apply -f fix-permission.yaml
   ```
   {: pre}

4. Wait for the pod to go into a `Completed` state.  
   ```
   kubectl get pod fix-permission
   ```
   {: pre}

5. Delete the `fix-permission` pod.
   ```
   kubectl delete pod fix-permission
   ```
   {: pre}

5. Re-create the `test-permission` pod that you used earlier to check the permissions.
   ```
   kubectl apply -f test-permission.yaml
   ```
   {: pre}

5. Verify that the permissions for your files are updated.
   1. Log in to your pod.
      ```
      kubectl exec test-permission -it bash
      ```
      {: pre}

   2. Navigate to your mount path and list the permissions for your files.
      ```
      cd test && ls -al
      ```
      {: pre}

      Example output:
      ```
      -rwxrwx--- 1 <nonroot_userID> root 6193 Aug 21 17:06 <file_name>
      ```
      {: screen}

6. Delete the `test-permission` pod.
   ```
   kubectl delete pod test-permission
   ```
   {: pre}

7. Mount the PVC to the app with the non-root user.

   Define the non-root user as `runAsUser` without setting `fsGroup` in your deployment YAML at the same time. Setting `fsGroup` triggers the {{site.data.keyword.cos_full_notm}} plug-in to update the group permissions for all files in a bucket when the pod is deployed. Updating the permissions is a write operation and might prevent your pod from getting into a `Running` state.
   {: important}

After you set the correct file permissions in your {{site.data.keyword.cos_full_notm}} service instance, do not upload files by using the console or the REST API. Use the {{site.data.keyword.cos_full_notm}} plug-in to add files to your service instance.
{: tip}

<br />




## Getting help and support
{: #ts_getting_help}

Still having issues with your cluster?
{: shortdesc}

-  In the terminal, you are notified when updates to the `ibmcloud` CLI and plug-ins are available. Be sure to keep your CLI up-to-date so that you can use all the available commands and flags.
-   To see whether {{site.data.keyword.Bluemix_notm}} is available, [check the {{site.data.keyword.Bluemix_notm}} status page ![External link icon](../icons/launch-glyph.svg "External link icon")](https://developer.ibm.com/bluemix/support/#status).
-   Post a question in the [{{site.data.keyword.containerlong_notm}} Slack ![External link icon](../icons/launch-glyph.svg "External link icon")](https://ibm-container-service.slack.com).
    If you are not using an IBM ID for your {{site.data.keyword.Bluemix_notm}} account, [request an invitation](https://bxcs-slack-invite.mybluemix.net/) to this Slack.
    {: tip}
-   Review the forums to see whether other users ran into the same issue. When you use the forums to ask a question, tag your question so that it is seen by the {{site.data.keyword.Bluemix_notm}} development teams.
    -   If you have technical questions about developing or deploying clusters or apps with {{site.data.keyword.containerlong_notm}}, post your question on [Stack Overflow ![External link icon](../icons/launch-glyph.svg "External link icon")](https://stackoverflow.com/questions/tagged/ibm-cloud+containers) and tag your question with `ibm-cloud`, `kubernetes`, and `containers`.
    -   For questions about the service and getting started instructions, use the [IBM Developer Answers ![External link icon](../icons/launch-glyph.svg "External link icon")](https://developer.ibm.com/answers/topics/containers/?smartspace=bluemix) forum. Include the `ibm-cloud` and `containers` tags.
    See [Getting help](/docs/get-support/howtogetsupport.html#using-avatar) for more details about using the forums.
-   Contact IBM Support by opening a case. To learn about opening an IBM support case, or about support levels and case severities, see [Contacting support](/docs/get-support/howtogetsupport.html#getting-customer-support).
When you report an issue, include your cluster ID. To get your cluster ID, run `ibmcloud ks clusters`.
{: tip}

