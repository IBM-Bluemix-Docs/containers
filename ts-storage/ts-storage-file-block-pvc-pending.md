---

copyright:
  years: 2014, 2021
lastupdated: "2021-06-02"

keywords: kubernetes, iks, help, network, connectivity

subcollection: containers
content-type: troubleshoot

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
{:terraform: .ph data-hd-interface='terraform'}
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
  
  

# File storage and block storage: Why does my PVC remain in a pending state?
{: #file_pvc_pending}

**Infrastructure provider**:
  * <img src="../images/icon-classic.png" alt="Classic infrastructure provider icon" width="15" style="width:15px; border-style: none"/> Classic
  * <img src="../images/icon-vpc.png" alt="VPC infrastructure provider icon" width="15" style="width:15px; border-style: none"/> VPC

{: tsSymptoms}
When you create a PVC and you run `kubectl get pvc <pvc_name>`, your PVC remains in a **Pending** state, even after waiting for some time.

{: tsCauses}
During the PVC creation and binding, many different tasks are executed by the file and block storage plug-in. Each task can fail and cause a different error message.

{: tsResolve}

1. Find the root cause for why the PVC remains in a **Pending** state.
   ```sh
   kubectl describe pvc <pvc_name> -n <namespace>
   ```
   {: pre}

2. Review common error message descriptions and resolutions.

| Error message | Description | Steps to resolve |
| --- | --- | --- |
| `User doesn't have permissions to create or manage Storage` `Failed to find any valid softlayer credentials in configuration file` `Storage with the order ID %d could not be created after retrying for %d seconds.` `Unable to locate datacenter with name <datacenter_name>.` | The IAM API key or the IBM Cloud infrastructure API key that is stored in the `storage-secret-store` Kubernetes secret of your cluster does not have all the required permissions to provision persistent storage. | See [PVC creation fails because of missing permissions](/docs/containers?topic=containers-missing_permissions). |
| `Your order will exceed the maximum number of storage volumes allowed. Please contact Sales` | Every {{site.data.keyword.cloud_notm}} account is set up with a maximum number of file and block storage instances that can be created. By creating the PVC, you exceed the maximum number of storage instances. For more information about the maximum number of volumes that you can create and how to retrieve the number of volumes in your account, see the documentation for [file](/docs/FileStorage?topic=FileStorage-managinglimits) and [block](/docs/BlockStorage?topic=BlockStorage-managingstoragelimits) storage. | To create a PVC, choose from the following options. <ul><li>Remove any unused PVCs.</li><li>Ask the {{site.data.keyword.cloud_notm}} account owner to increase your storage quota by [opening a support case](/docs/get-support?topic=get-support-using-avatar).</li></ul> |
| `Unable to find the exact ItemPriceIds(type|size|iops) for the specified storage` `Failed to place storage order with the storage provider` | The storage size and IOPS that you specified in your PVC are not supported by the storage type that you chose and cannot be used with the specified storage class. | Review [Deciding on the file storage configuration](/docs/containers?topic=containers-file_storage#file_predefined_storageclass) and [Deciding on the block storage configuration](/docs/containers?topic=containers-block_storage#block_predefined_storageclass) to find supported storage sizes and IOPS for the storage class that you want to use. Correct the size and IOPS, and re-create the PVC. |
| Failed to find the datacenter name in configuration file. | The data center that you specified in your PVC does not exist. | Run `ibmcloud ks locations` to list available data centers. Correct the data center in your PVC and re-create the PVC. |
| `Failed to place storage order with the storage provider` `Storage with the order ID 12345 could not be created after retrying for xx seconds.` `Failed to do subnet authorizations for the storage 12345.` `Storage 12345 has ongoing active transactions and could not be completed after retrying for xx seconds.` | The storage size, IOPS, and storage type might be incompatible with the storage class that you chose, or the {{site.data.keyword.cloud_notm}} infrastructure API endpoint is currently unavailable. | Review [Deciding on the file storage configuration](/docs/containers?topic=containers-file_storage#file_predefined_storageclass) and [Deciding on the block storage configuration](/docs/containers?topic=containers-block_storage#block_predefined_storageclass) to find supported storage sizes and IOPS for the storage class and storage type that you want to use. Then, delete the PVC and re-create the PVC. |
| Failed to find the storage with storage id 12345.` | You want to create a PVC for an existing storage instance by using Kubernetes static provisioning, but the storage instance that you specified could not be found. | Follow the instructions to provision existing [file storage](/docs/containers?topic=containers-file_storage#existing_file) or [block storage](/docs/containers?topic=containers-block_storage#existing_block) in your cluster and make sure to retrieve the correct information for your existing storage instance. Then, delete the PVC and re-create the PVC. |
| Storage type not provided, expected storage type is `Endurance` or `Performance`. | You created a custom storage class and specified a storage type that is not supported. | Update your custom storage class to specify `Endurance` or `Performance` as your storage type. To find examples for custom storage classes, see the sample custom storage classes for [file storage](/docs/containers?topic=containers-file_storage#file_custom_storageclass) and [block storage](/docs/containers?topic=containers-block_storage#block_custom_storageclass). | 
{: caption="File Storage error messages" caption-side="top"}
{: summary="The columns are read from left to right. The first column has the error message. The second column describes the error message. The third column provides steps to resolve the error."}



