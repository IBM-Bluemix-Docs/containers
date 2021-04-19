---

copyright:
  years: 2014, 2021
lastupdated: "2021-04-15"

keywords: kubernetes, iks, encrypt, security, kms, root key, crk

subcollection: containers

---

{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}
{:android: data-hd-operatingsystem="android"}
{:api: .ph data-hd-interface='api'}
{:apikey: data-credential-placeholder='apikey'}
{:app_key: data-hd-keyref="app_key"}
{:app_name: data-hd-keyref="app_name"}
{:app_secret: data-hd-keyref="app_secret"}
{:app_url: data-hd-keyref="app_url"}
{:authenticated-content: .authenticated-content}
{:beta: .beta}
{:c#: data-hd-programlang="c#"}
{:cli: .ph data-hd-interface='cli'}
{:codeblock: .codeblock}
{:curl: .ph data-hd-programlang='curl'}
{:deprecated: .deprecated}
{:dotnet-standard: .ph data-hd-programlang='dotnet-standard'}
{:download: .download}
{:external: target="_blank" .external}
{:faq: data-hd-content-type='faq'}
{:fuzzybunny: .ph data-hd-programlang='fuzzybunny'}
{:generic: data-hd-operatingsystem="generic"}
{:generic: data-hd-programlang="generic"}
{:gif: data-image-type='gif'}
{:go: .ph data-hd-programlang='go'}
{:help: data-hd-content-type='help'}
{:hide-dashboard: .hide-dashboard}
{:hide-in-docs: .hide-in-docs}
{:important: .important}
{:ios: data-hd-operatingsystem="ios"}
{:java: .ph data-hd-programlang='java'}
{:java: data-hd-programlang="java"}
{:javascript: .ph data-hd-programlang='javascript'}
{:javascript: data-hd-programlang="javascript"}
{:new_window: target="_blank"}
{:note .note}
{:note: .note}
{:objectc data-hd-programlang="objectc"}
{:org_name: data-hd-keyref="org_name"}
{:php: data-hd-programlang="php"}
{:pre: .pre}
{:preview: .preview}
{:python: .ph data-hd-programlang='python'}
{:python: data-hd-programlang="python"}
{:route: data-hd-keyref="route"}
{:row-headers: .row-headers}
{:ruby: .ph data-hd-programlang='ruby'}
{:ruby: data-hd-programlang="ruby"}
{:runtime: architecture="runtime"}
{:runtimeIcon: .runtimeIcon}
{:runtimeIconList: .runtimeIconList}
{:runtimeLink: .runtimeLink}
{:runtimeTitle: .runtimeTitle}
{:screen: .screen}
{:script: data-hd-video='script'}
{:service: architecture="service"}
{:service_instance_name: data-hd-keyref="service_instance_name"}
{:service_name: data-hd-keyref="service_name"}
{:shortdesc: .shortdesc}
{:space_name: data-hd-keyref="space_name"}
{:step: data-tutorial-type='step'}
{:subsection: outputclass="subsection"}
{:support: data-reuse='support'}
{:swift: .ph data-hd-programlang='swift'}
{:swift: data-hd-programlang="swift"}
{:table: .aria-labeledby="caption"}
{:term: .term}
{:tip: .tip}
{:tooling-url: data-tooling-url-placeholder='tooling-url'}
{:troubleshoot: data-hd-content-type='troubleshoot'}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:tsSymptoms: .tsSymptoms}
{:tutorial: data-hd-content-type='tutorial'}
{:ui: .ph data-hd-interface='ui'}
{:unity: .ph data-hd-programlang='unity'}
{:url: data-credential-placeholder='url'}
{:user_ID: data-hd-keyref="user_ID"}
{:vbnet: .ph data-hd-programlang='vb.net'}
{:video: .video}
 


# Protecting sensitive information in your cluster
{: #encryption}

Protect sensitive information in your {{site.data.keyword.containerlong}} cluster to ensure data integrity and to prevent your data from being exposed to unauthorized users.
{: shortdesc}

You can create sensitive data on different levels in your cluster that each require appropriate protection.
- **Cluster-level:** Cluster configuration data is stored in the etcd component of your Kubernetes master. Data in etcd is stored on the local disk of the Kubernetes master and is backed up to {{site.data.keyword.cos_full_notm}}. Data is encrypted during transit to {{site.data.keyword.cos_full_notm}} and at rest. You can choose to enable encryption for your etcd data on the local disk of your Kubernetes master by [enabling a key management service provider](#keyprotect) for your cluster.
- **App-level:** When you deploy your app, do not store confidential information, such as credentials or keys, in the YAML configuration file, configmaps, or scripts. Instead, use [Kubernetes secrets](https://kubernetes.io/docs/concepts/configuration/secret/){: external}, such as an `imagePullSecret` for registry credentials. You can also [encrypt data in Kubernetes secrets](#keyprotect) to prevent unauthorized users from accessing sensitive app information.

For more information about securing your cluster and personal information, see [Security for {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-security#security) and [Storing personal information](/docs/containers?topic=containers-security#pi).

## Overview of cluster encryption
{: #encrypt_ov}

The following image and description outline default and optional data encryption for {{site.data.keyword.containerlong_notm}} clusters.
{: shortdesc}

<img src="images/cs_encrypt_ov_kms.png" width="700" alt="Overview of cluster encryption" style="width:700px; border-style: none"/>

_Figure: Overview of data encryption in a cluster_

1.  **Kubernetes master control plane startup**: Components in the Kubernetes master, such as etcd, boot up on a LUKS-encrypted drive by using an IBM-managed key.
2.  **Bring your own key (BYOK)**: When you [enable a key management service (KMS) provider](#keyprotect) in your cluster, you can bring your own root key to create data encryption keys (DEKs) that encrypt the secrets in your cluster. The root key is stored in the KMS instance that you control. For example, if you use {{site.data.keyword.keymanagementservicelong_notm}}, the root key is stored in a FIPS 120-3 Level 3 hardware security module (HSM).
3.  **etcd data**: Etcd is the component of the master that stores the configuration files of your Kubernetes resources, such as deployments and secrets. Data in etcd is stored on the local disk of the Kubernetes master and is backed up to {{site.data.keyword.cos_full_notm}}. Data is encrypted during transit to {{site.data.keyword.cos_full_notm}} and at rest. When you enable a KMS provider, a wrapped data encryption key (DEK) is stored in etcd. The DEK encrypts the secrets in your cluster that store service credentials and the LUKS key. Because the root key is in your KMS instance, you control access to your encrypted secrets. To unwrap the DEK, the cluster uses the root key from your KMS instance. For more information about how key encryption works, see [Envelope encryption](/docs/key-protect/concepts?topic=key-protect-envelope-encryption#envelope-encryption).
4.  **Worker node disks**: Attached disks are used to boot your worker node, host the container file system, and store locally pulled images. The encryption and number of disks varies by infrastructure provider.
    * <img src="images/icon-vpc.png" alt="VPC infrastructure provider icon" width="15" style="width:15px; border-style: none"/> **VPC only**: The one primary disk is AES-256 bit encrypted at rest by the [underlying VPC infrastructure provider](/docs/vpc?topic=vpc-block-storage-about#vpc-storage-encryption). You cannot manage the encryption on your own with a KMS provider.
    * <img src="images/icon-classic.png" alt="Classic infrastructure provider icon" width="15" style="width:15px; border-style: none"/> **Classic only**: The primary disk that contains the kernel images to boot your worker node is unecrypted. The secondary disk that hosts the container file system and locally pulled images is AES 256-bit encrypted with an IBM-managed LUKS encryption key that is unique to the worker node and stored as a secret in etcd. When you reload or update your worker nodes, the LUKS keys are rotated. If you enable a KMS provider, the etcd secret that holds the LUKS key is encrypted by the root key and DEK of your KMS provider.
5.  **Cluster secrets**: By default, [Kubernetes secrets](https://kubernetes.io/docs/concepts/configuration/secret/){: external} are base64 encoded. To manage encryption of the Kubernetes secrets in your cluster, you can enable a KMS provider . The secrets are encrypted by KMS-provided encryption until their information is used. For example, if you update a Kubernetes pod that mounts a secret, the pod requests the secret values from the master API server. The master API server asks the KMS provider to use the root key to unwrap the DEK and encode its values to base64. Then, the master API server uses the KMS provider DEK that is stored in etcd to read the secret, and sends the secret to the pod by using TLS.
6.  **Persistent storage encryption**: You can choose to store data by [setting up file, block, object, or software-defined Portworx persistent storage](/docs/containers?topic=containers-storage_planning#persistent_storage_overview). If you store your data on file or block storage, your data is automatically encrypted at rest. If you use object storage, your data is also encrypted during transit. With Portworx, you can choose to [set up volume encryption](/docs/containers?topic=containers-portworx#encrypt_volumes) to protect your data during transit and at rest. The IBM Cloud infrastructure storage instances save the data on encrypted disks, so your data at rest is encrypted.
7.  **Data-in-use encryption**: For select, SGX-enabled classic worker node flavors, you can use [{{site.data.keyword.datashield_short}}](#datashield) to encrypt data-in-use within the worker node.


<br />

## Understanding Key Management Service (KMS) providers
{: #kms}

You can protect the etcd component in your Kubernetes master and Kubernetes secrets by using a Kubernetes [key management service (KMS) provider](https://kubernetes.io/docs/tasks/administer-cluster/kms-provider/){: external} that encrypts secrets with encryption keys that you control.
{: shortdesc}

**What KMS providers are available by default? Can I add other providers?**

{{site.data.keyword.containerlong_notm}} supports the following KMS providers:
* {{site.data.keyword.keymanagementservicefull}} for [public cloud](/docs/key-protect?topic=key-protect-getting-started-tutorial) or [on-prem](https://www.ibm.com/docs/en/cloud-private/3.2.0?topic=apis-key-management-service){: external} environments.
* [{{site.data.keyword.hscrypto}}](https://cloud.ibm.com/catalog/services/hyper-protect-crypto-services){: external} for keep your own key (KYOK) crypto unit support.

Because adding a different KMS provider requires updating the managed master default configuration, you cannot add other KMS providers to the cluster.

**Can I change the KMS provider?**

You can have one KMS provider enabled in the cluster. You can switch the KMS provider, but you cannot disable KMS provider encryption after it is enabled. For example, if you enabled {{site.data.keyword.keymanagementserviceshort}} in your cluster, but want to use {{site.data.keyword.hscrypto}} instead, you can [enable](#keyprotect) {{site.data.keyword.hscrypto}} as the KMS provider.

<p class="important">You cannot disable KMS provider encryption. Do not delete root keys in your KMS instance, even if you rotate to use a new key. If you delete a root key that a cluster uses, the cluster becomes unusable, loses all its data, and cannot be recovered.<br><br>Similarly, if you disable a root key, operations that rely on reading secrets fail. Unlike deleting a root key, however, you can reenable a disabled key to make your cluster usable again.</p>

**With a KMS provider, do I control the encryption in my cluster?**

Yes. When you enable a KMS provider in your cluster, your own KMS root key is used to encrypt data in etcd, including the LUKS secrets. Using your own encryption root key adds a layer of security to your etcd data and Kubernetes secrets and gives you more granular control of who can access sensitive cluster information. For more information, see the [overview](#encrypt_ov) and your KMS provider's documentation, such as [{{site.data.keyword.keymanagementserviceshort}} envelope encryption](/docs/key-protect?topic=key-protect-envelope-encryption).

**Can I use all the features of the KMS provider with my cluster?**

Review the following known limitations:
* Customizing the IP addresses that are allowed to connect to your {{site.data.keyword.keymanagementserviceshort}} instance is not supported.

**Do the features change depending on my cluster version?**
{: #kms-keyprotect-features}

Yes. Your cluster version impacts the functionality of the KMS provider. To see what {{site.data.keyword.keymanagementserviceshort}} features are available for different cluster versions of {{site.data.keyword.containerlong_notm}}, review the following table.

To check your cluster version, run the following command.
```
ibmcloud ks cluster ls
```
{: pre}

To use the additional {{site.data.keyword.keymanagementserviceshort}} features:
1.  [Update your cluster](/docs/containers?topic=containers-update) to at least version `1.18.8_1525`.
2.  [Reenable KMS encryption](#keyprotect) to register your cluster with {{site.data.keyword.keymanagementserviceshort}} again.

| {{site.data.keyword.keymanagementserviceshort}} feature | Cluster version earlier than `1.18.8_1525` | Cluster version `1.18.8_1525` or later |
| --- | --- | --- |
| You can enable the cluster to use {{site.data.keyword.keymanagementserviceshort}} root keys to encrypt secrets. | <img src="images/confirm.svg" width="32" alt="Feature available" style="width:32px;" /> | <img src="images/confirm.svg" width="32" alt="Feature available" style="width:32px;" /> |
| You must rewrite cluster secrets manually after rotating root keys in {{site.data.keyword.keymanagementserviceshort}}.  | <img src="images/confirm.svg" width="32" alt="Feature available" style="width:32px;" /> | |
| Cluster secrets are automatically updated after rotating root keys in {{site.data.keyword.keymanagementserviceshort}}. | | <img src="images/confirm.svg" width="32" alt="Feature available" style="width:32px;" /> |
| You can view clusters that use the root key from the {{site.data.keyword.keymanagementserviceshort}} interface. | | <img src="images/confirm.svg" width="32" alt="Feature available" style="width:32px;" /> |
| Clusters automatically respond if you disable, enable, or restore root keys in {{site.data.keyword.keymanagementserviceshort}}. | | <img src="images/confirm.svg" width="32" alt="Feature available" style="width:32px;" /> |
| Disabling a root key restricts cluster functionality until you reenable the key. | <img src="images/confirm.svg" width="32" alt="Feature available" style="width:32px;" /> | <img src="images/confirm.svg" width="32" alt="Feature available" style="width:32px;" /> |
| Deleting a root key makes the cluster unusable and irrecoverable. | <img src="images/confirm.svg" width="32" alt="Feature available" style="width:32px;" /> | <img src="images/confirm.svg" width="32" alt="Feature available" style="width:32px;" /> |
| Root keys cannot be deleted if the key is used by a cluster. | | <img src="images/confirm.svg" width="32" alt="Feature available" style="width:32px;" /> |
{: row-headers}
{: class="comparison-table"}
{: caption="{{site.data.keyword.keymanagementserviceshort}} features by cluster version." caption-side="top"}
{: summary="The rows are read from left to right. The first column describes the feature. The second column checks whether the feature available in the older version. The second column checks whether the feature available in the newer version."}

<br />

## Encrypting the Kubernetes master's local disk and secrets by using a KMS provider
{: #keyprotect}

Enable a Kubernetes [key management service (KMS) provider](https://kubernetes.io/docs/tasks/administer-cluster/kms-provider/){: external} such as [{{site.data.keyword.keymanagementserviceshort}}](/docs/key-protect?topic=key-protect-getting-started-tutorial){: external} to encrypt the Kubernetes secrets and etcd component of your Kubernetes master.
{: shortdesc}

**Clusters that run version 1.17**: To rotate your encryption key, repeat the [CLI](#kms_cli) or [console](#kms_ui) steps to enable KMS provider encryption with a new root key ID. The new root key is added to the cluster configuration along with the previous root key so that existing encrypted data is still protected. To encrypt your existing secrets with the new root key, you must rewrite the secrets. When you rotate a root key, you cannot reuse a previous root key for the same cluster.
{: note}

### Prerequisites
{: #kms_prereqs}

Before you enable a key management service (KMS) provider in your cluster, create a KMS instance and complete the following steps.
{: shortdesc}

1.  Create a KMS instance, such as [{{site.data.keyword.keymanagementserviceshort}}](/docs/key-protect?topic=key-protect-provision#provision) or [{{site.data.keyword.hscrypto}}](https://cloud.ibm.com/catalog/services/hyper-protect-crypto-services){: external}.
2.  Create a customer root key (CRK) in your KMS instance, such as a [{{site.data.keyword.keymanagementserviceshort}} root key](/docs/key-protect?topic=key-protect-create-root-keys#create-root-keys). By default, the root key is created without an expiration date.

    Need to set an expiration date to comply with internal security policies? [Create the root key by using the API](/docs/key-protect?topic=key-protect-create-root-keys#create-root-key-api) and include the `expirationDate` parameter. **Important**: Before your root key expires, repeat these steps to update your cluster to use a new root key. When a root key expires, the cluster secrets cannot be decrypted and your cluster becomes unusable. Depending on the cluster version, the time lapse between the root key expiring and the cluster no longer being able to decrypt secrets might be about an hour, or when the master is refreshed.
    {: tip}

3.  Make sure that you have the correct permissions to enable KMS in your cluster.
    * Ensure that you have the [**Administrator** {{site.data.keyword.cloud_notm}} IAM platform access role](/docs/containers?topic=containers-users#platform) for the cluster.
    * Make sure that the API key that is set for the region that your cluster is in is authorized to use the KMS provider. For example, to create an instance and root key, you need at least the **Editor** platform and **Writer** service access roles for [{{site.data.keyword.keymanagementserviceshort}}](/docs/key-protect?topic=key-protect-grant-access-keys). To check the API key owner whose credentials are stored for the region, run `ibmcloud ks api-key info -c <cluster_name_or_ID>`.

    **For clusters that run Kubernetes 1.18.8_1525 or later**: An additional **Reader** [service-to-service authorization policy](/docs/account?topic=account-serviceauth) between {{site.data.keyword.containerlong_notm}} and {{site.data.keyword.keymanagementserviceshort}} is automatically created for your cluster, if the policy does not already exist. Without this policy, your cluster cannot use all the [{{site.data.keyword.keymanagementserviceshort}} features](#kms-keyprotect-features).
    {: note}
4.  Consider [updating your cluster](/docs/containers?topic=containers-update) to at least version `1.18.8_1525` to get the latest [{{site.data.keyword.keymanagementserviceshort}} features](#kms-keyprotect-features).
5.  Enable KMS encryption through the [CLI](#kms_cli) or [console](#kms_ui).

### Enabling KMS encryption through the CLI
{: #kms_cli}

You can enable a KMS provider or update the instance or root key that encrypts secrets in the cluster through the CLI.
{: shortdesc}

1.  Complete the [prerequisite steps](#kms_prereqs) to create a KMS instance and root key.
2.  Get the ID of the KMS instance that you previously created.
    ```
    ibmcloud ks kms instance ls
    ```
    {: pre}
3.  Get the **ID** of the root key that you previously created.
    ```
    ibmcloud ks kms crk ls --instance-id <KMS_instance_ID>
    ```
    {: pre}
4.  Enable the KMS provider to encrypt secrets in your cluster. Fill in the flags with the information that you previously retrieved. The KMS provider's private cloud service endpoint is used by default to download the encryption keys. To use the public cloud service endpoint instead, include the `--public-endpoint` flag. The enablement process can take some time to complete.<p class="important">During the enablement, you might not be able to access the Kubernetes master such as to update YAML configurations for deployments.</p>
    ```
    ibmcloud ks kms enable -c <cluster_name_or_ID> --instance-id <kms_instance_ID> --crk <root_key_ID> [--public-endpoint]
    ```
    {: pre}
5.  Verify that the KMS enablement process is finished. The process is finished when that the **Master Status** is **Ready** and **Key management service** is **enabled**.
    ```
    ibmcloud ks cluster get -c <cluster_name_or_ID>
    ```
    {: pre}

    Example output when the enablement is in progress:
    ```
    Name:                   <cluster_name>   
    ID:                     <cluster_ID>   
    ...
    Master Status:          Key management service feature enablement in progress.  
    ```
    {: screen}

    Example output when the master is ready:
    ```
    Name:                   <cluster_name>   
    ID:                     <cluster_ID>   
    ...
    Master Status:          Ready (1 min ago)
    ...
    Key Management Service: enabled   
    ```
    {: screen}

    After the KMS provider is enabled in the cluster, data in `etcd` and new secrets that are created in the cluster are automatically encrypted by using your root key.
    {: note}

6.  **Clusters that run version 1.17**: To encrypt existing secrets with the root key, rewrite the secrets.
    1.  [Log in to your account. If applicable, target the appropriate resource group. Set the context for your cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
    2.  With `cluster-admin` access, rewrite the secrets.
        ```
        kubectl get secrets --all-namespaces -o json | kubectl replace -f -
        ```
        {: pre}
7.  Optional: [Verify that your secrets are encrypted](#verify_kms).

<p class="important">Do not delete root keys in your KMS instance, even if you rotate to use a new key. If you delete a root key that a cluster uses, the cluster becomes unusable, loses all its data, and cannot be recovered. When you rotate a root key, you cannot reuse a previous root key for the same cluster.<br><br>Similarly, if you disable a root key, operations that rely on reading secrets fail. Unlike deleting a root key, however, you can reenable a disabled key to make your cluster usable again.</p>

### Enabling KMS encryption through the console
{: #kms_ui}

You can enable a KMS provider or update the instance or root key that encrypts secrets in the cluster through the {{site.data.keyword.cloud_notm}} console.
{: shortdesc}

1.  Complete the [prerequisite steps](#kms_prereqs) to create a KMS instance and root key.
2.  From the [Clusters](https://cloud.ibm.com/kubernetes/clusters){: external} console, select the cluster that you want to enable encryption for.
3.  From the **Overview** tab, in the **Summary > Key management service** section, click **Enable**. If you already enabled the KMS provider, click **Update**.
4.  Select the **Key management service instance** and **Root key** that you want to use for the encryption.<p class="important">During the enablement, you might not be able to access the Kubernetes master such as to update YAML configurations for deployments.</p>
5.  Click **Enable** (or **Update**).
6.  Verify that the KMS enablement process is finished. From the **Summary > Master status** section, you can check the progress.
    Example output when the enablement is in progress:
    ```
    Master status   KMS feature enablement in progress.  
    ```
    {: screen}

    Example output when the master is ready:
    ```
    Master status   Ready
    ```
    {: screen}

    After the KMS provider is enabled in the cluster, data in `etcd` and new secrets that are created in the cluster are automatically encrypted by using your root key.
    {: note}

6.  **Clusters that run version 1.17**: To encrypt existing secrets with the root key, rewrite the secrets.
    1.  [Log in to your account. If applicable, target the appropriate resource group. Set the context for your cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
    2.  With `cluster-admin` access, rewrite the secrets.
        ```
        kubectl get secrets --all-namespaces -o json | kubectl replace -f -
        ```
        {: pre}
7.  Optional: [Verify that your secrets are encrypted](#verify_kms).

<p class="important">Do not delete root keys in your KMS instance, even if you rotate to use a new key. If you delete a root key that a cluster uses, the cluster becomes unusable, loses all its data, and cannot be recovered. When you rotate a root key, you cannot reuse a previous root key for the same cluster.<br><br>Similarly, if you disable a root key, operations that rely on reading secrets fail. Unlike deleting a root key, however, you can reenable a disabled key to make your cluster usable again.</p>

### Rotating the root key for your cluster
{: #kms_rotate}

To rotate the root key that is used to encrypt your cluster, you can repeat the steps to enable KMS encryption from the [CLI](#kms_cli) or [console](#kms_ui). When you rotate a root key, you cannot reuse a previous root key for the same cluster.
{: shortdesc}

Additionally, if your cluster runs version `1.18.8_1525` or later, you can also [rotate the root key](/docs/key-protect?topic=key-protect-rotate-keys) from your {{site.data.keyword.keymanagementserviceshort}} instance.

## Verifying secret encryption
{: #verify_kms}

After you enable a KMS provider in your {{site.data.keyword.containerlong_notm}} cluster, you can verify that your cluster secrets are encrypted by disabling the root key. When you disable the root key, the cluster can no longer decrypt the secrets and becomes unusable, which signifies that your secrets were encrypted.
{: shortdesc}

Before you begin:
* Consider [updating your cluster](/docs/containers?topic=containers-update) to at least Kubernetes version `1.19`. If you do not update your cluster to this version, changes to the root key are not reported in the cluster health status and take longer to take effect in your cluster.
* Make sure that you have the {{site.data.keyword.cloud_notm}} IAM **Administrator** platform and **Manager** service access role for the cluster.

To verify secret encryption by disabling a root key:
1.  [Enable KMS encryption in your cluster](#keyprotect). To check that KMS encryption is enabled, verify that the **Key Management Service** status is set to `enabled` in the output of the following command.
    ```
    ibmcloud ks cluster get -c <cluster_name_or_ID>
    ```
    {: pre}
2.  [Log in to your account. If applicable, target the appropriate resource group. Set the context for your cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
3.  Verify that you can list the secrets in your cluster.
    ```
    kubectl get secrets --all-namespaces
    ```
    {: pre}
4.  In your {{site.data.keyword.keymanagementserviceshort}} instance, [disable the root key](/docs/key-protect?topic=key-protect-disable-keys) that is used to encrypt your cluster.
5.  Wait for the cluster to detect the change to your root key.

    In clusters that run a version earlier than `1.19`, you might need to wait for an hour or longer.
    {: note}

6.  Try to list your secrets. You get a timeout error because you can no longer connect to your cluster. If you try to set the context for your cluster by running `ibmcloud ks cluster config`, the command fails.
    ```
    kubectl get secrets --all-namespaces
    ```
    {: pre}

    Example output:
    ```
    Unable to connect to the server: dial tcp 169.48.110.250:32346: i/o timeout
    ```
    {: screen}
7.  For clusters that run Kubernetes version `1.19` or later, check that your cluster is in a **warning** state. Your cluster remains in this state and is unusable until you enable your root key again.
    ```
    ibmcloud ks cluster get -c <cluster_name_or_ID>
    ```
    {: pre}
8.  In your {{site.data.keyword.keymanagementserviceshort}} instance, [enable the root key](/docs/key-protect?topic=key-protect-disable-keys) so that your cluster returns to a **normal** state and becomes usable again.

## Encrypting data in classic clusters by using IBM Cloud Data Shield 
{: #datashield}

{{site.data.keyword.datashield_short}} is integrated with Intel® Software Guard Extensions (SGX) and Fortanix® technology so that the app code and data of your containerized workloads are protected in use. The app code and data run in CPU-hardened enclaves, which are trusted areas of memory on the worker node that protect critical aspects of the app, which helps to keep the code and data confidential and unmodified.
{: shortdesc}


<img src="images/icon-classic.png" alt="Classic infrastructure provider icon" width="15" style="width:15px; border-style: none"/> Applies to only classic clusters. VPC clusters cannot have bare metal worker nodes, which are required to use {{site.data.keyword.datashield_short}}.
{: note}

When it comes to protecting your data, encryption is one of the most popular and effective controls. But, the data must be encrypted at each step of its lifecycle for your data to be protected. During its lifecycle, data has three phases. It can be at rest, in motion, or in use. Data at rest and in motion are generally the area of focus when you think of securing your data. But, after an application starts to run, data that is in use by CPU and memory is vulnerable to various attacks. The attacks might include malicious insiders, root users, credential compromise, OS zero-day, network intruders, and others. Taking that protection one step further, you can now encrypt data in use.

If you or your company require data sensitivity due to internal policies, government regulations, or industry compliance requirements, this solution might help you to move to the cloud. Example solutions include financial and healthcare institutions, or countries with government policies that require on-premises cloud solutions.


To get started, provision an SGX-enabled bare metal worker cluster with a [supported flavor for {{site.data.keyword.datashield_short}}](/docs/data-shield?topic=data-shield-getting-started).

<br>


