---

copyright:
  years: 2014, 2021
lastupdated: "2021-04-15"

keywords: containers, iks, vpc, block storage

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


# Storing data on {{site.data.keyword.block_storage_is_short}} for unmanaged clusters
{: #vpc-block-storage-driver-unmanaged}

The following documentation covers the steps to deploy the {{site.data.keyword.block_storage_is_short}} driver on unmanaged OpenShift Container Platform clusters in IBM Cloud. This process is unsupported and any issues with the steps or the driver must be recreated in a Kubernetes cluster to receive support.
{: important}

**Supported infrastructure provider**:
  * <img src="images/icon-vpc.png" alt="VPC infrastructure provider icon" width="15" style="width:15px; border-style: none"/> VPC Generation 2 compute

Want to use {{site.data.keyword.block_storage_is_short}} in an {{site.data.keyword.containerlong_notm}} or {{site.data.keyword.openshiftlong_notm}} cluster? See [Storing data on Block Storage for VPC](/docs/containers?topic=containers-vpc-block) for more information.
{: tip}
 
## Prerequisites
{: #vpc-block-um-prereq}
To use the {{site.data.keyword.block_storage_is_short}} driver, complete the following tasks: 
{: shortdesc}

* Create an {{site.data.keyword.containerlong_notm}} cluster on VPC Gen 2 infrastructure.
* [Label your worker nodes](#vpc-block-label-um).
* [Create your storage secret](#vpc-block-create-storage-secret).
* [Create an image pull secret that uses your IAM credentials](#vpc-block-create-storage-secret).

### Labelling your worker nodes
{: #vpc-block-label-um}
Before you can deploy the {{site.data.keyword.block_storage_is_short}} driver, you must prepare your worker nodes by adding the required labels.
{: shortdesc}


Before you begin, [Log in to your account. If applicable, target the appropriate resource group. Set the context for your cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1. Retrieve the following details of your VPC instance. These parameters are used to apply labels to your worker nodes.

    * `<instanceID>` - The VPC instance ID. To retrieve this value, run `ibmcloud is ins`.
    * `<node-name>` - The names of the worker nodes. To retrieve this value, run `kubectl get nodes`.
    * `<region-of-instanceID>` and `<zone-of-instanceID>` - The region and zone that your VPC instance is in. To retrieve these values, run `ibmcloud is in <instanceID>`. Example region value: `eu-de`. Example zone value: `eu-de-1`.

2. Copy the following shell script and save it to a file on your local machine called `setup.sh`
    ```sh
    #!/bin/bash

    function help()
    {
        echo "Run the script in the following format..."
        echo "./setup.sh <node-name> <instanceID> <region-of-instanceID> <zone-of-instanceID>"
        exit 1
    }

    function apply_labels()
    {
        kubectl label nodes $1 "ibm-cloud.kubernetes.io/worker-id"=$2
        kubectl label nodes $1 "failure-domain.beta.kubernetes.io/region"=$3
        kubectl label nodes $1 "failure-domain.beta.kubernetes.io/zone"=$4
        kubectl label nodes $1 "topology.kubernetes.io/region"=$3
        kubectl label nodes $1 "topology.kubernetes.io/zone"=$4
    }

    function verify_node()
    {
        kubectl get nodes | grep $1
        if (( $? == 0 ))
        then
            return 0
        else
            return 1
        fi
    }

    if (( $# < 4 ))
    then
        help
    fi

    node=$1
    instanceID=$2
    region=$3
    zone=$4

    verify_node $node
    if (( $? == 0 ))
    then
        apply_labels $node $instanceID $region $zone
    else
        echo "Node " \'$node\' " not found in the cluster, please check the node or passing correct parameters while executing script"
        help
    fi
    ```
    {: pre}

3. Label your worker nodes by running the shell script and specifying the parameters that you retrieved earlier. Repeat this step for each worker node in your cluster.
    ```sh
    sh setup.sh <node-name> <instanceID> <region-of-instanceID> <zone-of-instanceID>
    ```
    {: pre}

### Retrieving IAM and VPC details
{: #vpc-block-driver-get-details}

To create the Kubernetes secret that is used in the {{site.data.keyword.block_storage_is_short}} configmap, you must retrieve your IAM and VPC details.
{: shortdesc}

1. Retrieve the following configuration parameter values. These values are used to create the Kubernetes secret that is required for {{site.data.keyword.block_storage_is_short}}.

    * `<g2_api_key>` - The IAM API key. You can use your existing API key or you can create an API key by running the `ibmcloud iam api-key-create NAME` command.
    * `<g2_riaas_endpoint>` - The VPC regional endpoint of your VPC Gen 2 cluster in the format `https://<region>.iaas.cloud.ibm.com`. Example: `https://eu-de.iaas.cloud.ibm.com`. For more information, see [VPC endpoints](/docs/vpc?topic=vpc-service-endpoints-for-vpc).
    * `<g2_resource_group_id>` - To retrieve this value, run the `ibmcloud is vpc <vpc-ID>` command and note the `Resource group` field.

2. Save the following TOML configuration file to your local machine called `config.toml`. Make sure that there are no blank lines between the values and no blank lines at the end of the file.
    ```
    [server]
    debug_trace = false
    [vpc]
    iam_client_id = "bx"
    iam_client_secret = "bx"
    g2_token_exchange_endpoint_url = "https://iam.bluemix.net"
    g2_riaas_endpoint_url = "<g2_riaas_endpoint>"
    g2_resource_group_id = "<resource_group_id>"
    g2_api_key = "<IAM_API_key>" 
    provider_type = "g2"
    ```
    {: codeblock}

3. Enter the values that you retrieved earlier and encode the TOML file to base64. Save the base64 output to use in the Block Storage driver configmap.
    ```sh
    cat ./config.toml | base64
    ```
    {: pre}

<br />

## Creating the image pull secret in your cluster
{: #vpc-block-create-storage-secret}

Create an image pull secret in your cluster. The secret you create is used to pull the {{site.data.keyword.block_storage_is_short}} driver images.

1. Review and retrieve the following values for your image pull secret.
    * `<docker-username>` - Enter the string: `iamapikey`.
    * `<docker-password>` - Enter your IAM API key. For more information about IAM API keys, see [Understanding API keys](/docs/account?topic=account-manapikey).
    * `<docker-email>` - Enter the string: `iamapikey`.

1. Run the following command to create the image pull secret in your cluster. Note that your secret must be named `icr-io-secret`.

    ```sh
    kubectl create secret docker-registry icr-io-secret --docker-server=icr.io --docker-username=iamapikey --docker-password=-<iam-api-key> --docker-email=iamapikey -n kube-system
    ```
    {: pre}

## Creating the {{site.data.keyword.block_storage_is_short}} driver deployment
{: #vpc-block-um-deploy-cm}

Select the {{site.data.keyword.block_storage_is_short}} driver configmap that matches the operating system of your worker nodes. When you create the deployment in your cluster, the {{site.data.keyword.block_storage_is_short}} driver and storage classes are installed.
{: shortdesc}

1. Save one of the following YAML configurations to a file on your local machine called `configmap.yaml`. Select the configmap based on your cluster operating system.
    * [RHEL or CentOS configmap](#vpc-block-rhel-cm)
    * [Ubuntu configmap](#vpc-block-ubuntu-cm)

2. Add the encoded TOML configuration details that you created earlier to the configmap in the `slclient.toml` secret configuration section.

3. Create the configmap in your cluster.
    ```sh
    oc create -f configmap.yaml
    ```
    {: pre}

4. Verify that the driver pods are deployed and the status is `Running`.
    ```sh
    oc get pods -n kube-system | grep vpc
    ```
    {: pre}

5. Verify that the `csidrivers` are created.
    ```sh
    oc get csidrivers | grep vpc
    ```
    {: pre}

    **Example output**:

    ```sh
    NAME                   ATTACHREQUIRED   PODINFOONMOUNT   MODES        AGE
    vpc.block.csi.ibm.io   true             true             Persistent   8m26s
    ```
    {: scree}

6. Verify that the storage classes are created.
    ```
    oc get sc
    ```
    {: pre}

    **Example output**
    ```sh
    NAME                                    PROVISIONER             RECLAIMPOLICY   VOLUMEBINDINGMODE      ALLOWVOLUMEEXPANSION   AGE
    local-path (default)                    rancher.io/local-path   Delete          WaitForFirstConsumer   false                  8d
    ibmc-vpc-block-10iops-tier (default)    vpc.block.csi.ibm.io    Delete          Immediate              false                  9m
    ibmc-vpc-block-5iops-tier               vpc.block.csi.ibm.io    Delete          Immediate              false                  9m
    ibmc-vpc-block-custom                   vpc.block.csi.ibm.io    Delete          Immediate              false                  9m
    ibmc-vpc-block-general-purpose          vpc.block.csi.ibm.io    Delete          Immediate              false                  9m
    ibmc-vpc-block-retain-10iops-tier       vpc.block.csi.ibm.io    Retain          Immediate              false                  9m
    ibmc-vpc-block-retain-5iops-tier        vpc.block.csi.ibm.io    Retain          Immediate              false                  8m59s
    ibmc-vpc-block-retain-custom            vpc.block.csi.ibm.io    Retain          Immediate              false                  8m59s
    ibmc-vpc-block-retain-general-purpose   vpc.block.csi.ibm.io    Retain          Immediate              false                  8m59s
    ```
    {: pre}


7. [Deploy a stateful set that uses {{site.data.keyword.block_storage_is_short}}](#vpc-block-stateful-set-deploy).

<br />
## Deploying a stateful set that uses {{site.data.keyword.block_storage_is_short}}
{: #vpc-block-stateful-set-deploy}

After you deploy the {{site.data.keyword.block_storage_is_short}} driver, you can create deployments that leverage {{site.data.keyword.block_storage_is_short}}. The following stateful set dynamically provisions {{site.data.keyword.block_storage_is_short}} volumes by creating PVCs that use `ibmc-vpc-block-5iops-tier` storage class.
{: shortdesc}

1. Save the following YAML configuration as a file on your local machine called `statefulset.yaml`.
```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx
  labels:
    app: nginx
spec:
  ports:
  - port: 80
    name: web
  clusterIP: None
  selector:
    app: nginx
---
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: web
spec:
  serviceName: "nginx"
  replicas: 2
  podManagementPolicy: "Parallel"
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        securityContext:
          privileged: false
        image: k8s.gcr.io/nginx-slim:0.8
        ports:
        - containerPort: 80
        name: web
        volumeMounts:
        - name: www
          mountPath: /usr/share/nginx/html
      tolerations:
      - operator: Exists 
  volumeClaimTemplates:
  - metadata:
      annotations:
        volume.beta.kubernetes.io/storage-class: ibmc-vpc-block-5iops-tier
      name: www
    spec:
      accessModes:
      - ReadWriteOnce # access mode
      resources:
        requests:
          storage: 25Gi #
```
    {: pre}

2. Create the stateful set in your cluster.
    ```sh
    kubectl create -f statefulset.yaml
    ```
    {: pre}

3. Verify that the stateful set pods are running.
    ```sh
    kubectl get pods
    ```
    {: pre}

    **Example output**
    ```sh
    NAME    READY   STATUS    RESTARTS   AGE
    web-0   1/1     Running   0          2m52s
    web-1   1/1     Running   0          2m52s
    ```
    {: screen}


<br />

## Removing the {{site.data.keyword.block_storage_is_short}} driver
{: #removing-the-block-storage-for-vpc-driver}
If you no longer want to use the {{site.data.keyword.block_storage_is_short}} driver in your cluster, you can remove the configmap to remove the driver pods.
{: shortdesc}

Removing the {{site.data.keyword.block_storage_is_short}} driver from your cluster does not remove the data in your storage volumes. If you want to fully remove your PVs and PVCs, see [Cleaning up persistent storage](/docs/openshift?topic=openshift-vpc-block#storage_remove).
{: important}

1. Delete the `ibm-vpc-block-csi-configmap` configmap from your cluster.
  ```sh
  oc rm cm ibm-vpc-block-csi-configmap -n kube-system
  ```
  {: pre}

2. Verify that the configmap is removed.
  ```sh
  oc get cm -n kube-system | grep ibm-vpc-block-csi-configmap
  ```
  {: pre}


<br />

## Config map reference
{: #vpc-block-um-cm-ref}
Select one of the following configmaps based on your worker node operating system.

* [RHEL or CentOS configmap](#vpc-block-rhel-cm)
* [Ubuntu configmap](#vpc-block-ubuntu-cm)

### RHEL or CentOS configmap
{: #vpc-block-rhel-cm}

Save the following configmap YAML as a file on your local machine.

```yaml
apiVersion: v1
items:
- apiVersion: v1
  data:
    CSI_ENDPOINT: unix:/csi/csi.sock
    IKS_BLOCK_PROVIDER_NAME: iks-vpc
    IKS_ENABLED: "False"
    SECRET_CONFIG_PATH: /etc/storage_ibmc
    VPC_API_GENERATION: "1"
    VPC_API_TIMEOUT: 180s
    VPC_API_VERSION: "2019-07-02"
    VPC_BLOCK_PROVIDER_NAME: vpc
    VPC_ENABLED: "True"
    VPC_RETRY_ATTEMPT: "10"
    VPC_RETRY_INTERVAL: "120"
  kind: ConfigMap
  metadata:
    annotations:
      version: 2.0.3_354
    labels:
      addonmanager.kubernetes.io/mode: Reconcile
      app: ibm-vpc-block-csi-driver
    name: ibm-vpc-block-csi-configmap
    namespace: kube-system
- apiVersion: v1
  imagePullSecrets:
  - name: bluemix-default-secret
  - name: bluemix-default-secret-regional
  - name: bluemix-default-secret-international
  - name: icr-io-secret
  kind: ServiceAccount
  metadata:
    annotations:
      version: 2.0.3_354
    labels:
      addonmanager.kubernetes.io/mode: Reconcile
      app: ibm-vpc-block-csi-driver
    name: ibm-vpc-block-node-sa
    namespace: kube-system
- apiVersion: v1
  data:
    cluster-config.json: |
      {}
  kind: ConfigMap
  metadata:
    annotations:
    name: cluster-info
    namespace: kube-system
- apiVersion: v1
  data:
    slclient.toml: # Enter the base64 encoded TOML file that you created earlier
  kind: Secret
  metadata:
    labels:
      addonmanager.kubernetes.io/mode: Reconcile
      app: ibm-vpc-block-csi-driver
      kubernetes.io/cluster-service: "true"
    name: storage-secret-store
    namespace: kube-system
  type: Opaque
- apiVersion: rbac.authorization.k8s.io/v1
  kind: ClusterRole
  metadata:
    annotations:
      version: 2.0.3_354
    labels:
      addonmanager.kubernetes.io/mode: Reconcile
      app: ibm-vpc-block-csi-driver
    name: vpc-block-driver-registrar-role
  rules:
  - apiGroups:
    - ""
    resources:
    - events
    verbs:
    - get
    - list
    - watch
    - create
    - update
    - patch
  - apiGroups:
    - ""
    resources:
    - nodes
    verbs:
    - get
    - list
    - watch
  - apiGroups:
    - ""
    resources:
    - persistentvolumes
    verbs:
    - get
    - list
    - watch
    - create
    - delete
  - apiGroups:
    - ""
    resources:
    - persistentvolumeclaims
    verbs:
    - get
    - list
    - watch
    - update
- apiVersion: rbac.authorization.k8s.io/v1
  kind: ClusterRoleBinding
  metadata:
    annotations:
      version: 2.0.3_354
    labels:
      addonmanager.kubernetes.io/mode: Reconcile
      app: ibm-vpc-block-csi-driver
    name: vpc-block-driver-registrar-binding
  roleRef:
    apiGroup: rbac.authorization.k8s.io
    kind: ClusterRole
    name: vpc-block-driver-registrar-role
  subjects:
  - kind: ServiceAccount
    name: ibm-vpc-block-node-sa
    namespace: kube-system
- apiVersion: v1
  imagePullSecrets:
  - name: bluemix-default-secret
  - name: bluemix-default-secret-regional
  - name: bluemix-default-secret-international
  - name: icr-io-secret
  kind: ServiceAccount
  metadata:
    annotations:
      version: 2.0.3_354
    labels:
      addonmanager.kubernetes.io/mode: Reconcile
      app: ibm-vpc-block-csi-driver
    name: ibm-vpc-block-controller-sa
    namespace: kube-system
- apiVersion: rbac.authorization.k8s.io/v1
  kind: ClusterRole
  metadata:
    annotations:
      version: 2.0.3_354
    labels:
      addonmanager.kubernetes.io/mode: Reconcile
      app: ibm-vpc-block-csi-driver
    name: vpc-block-provisioner-role
  rules:
  - apiGroups:
    - ""
    resources:
    - secrets
    verbs:
    - get
    - list
  - apiGroups:
    - ""
    resources:
    - persistentvolumes
    verbs:
    - get
    - list
    - watch
    - create
    - delete
  - apiGroups:
    - ""
    resources:
    - persistentvolumeclaims
    verbs:
    - get
    - list
    - watch
    - update
  - apiGroups:
    - storage.k8s.io
    resources:
    - storageclasses
    verbs:
    - get
    - list
    - watch
  - apiGroups:
    - ""
    resources:
    - events
    verbs:
    - list
    - watch
    - create
    - update
    - patch
  - apiGroups:
    - snapshot.storage.k8s.io
    resources:
    - volumesnapshots
    verbs:
    - get
    - list
  - apiGroups:
    - snapshot.storage.k8s.io
    resources:
    - volumesnapshotcontents
    verbs:
    - get
    - list
  - apiGroups:
    - storage.k8s.io
    resources:
    - csinodes
    verbs:
    - get
    - list
    - watch
  - apiGroups:
    - ""
    resources:
    - nodes
    verbs:
    - get
    - list
    - watch
- apiVersion: rbac.authorization.k8s.io/v1
  kind: ClusterRoleBinding
  metadata:
    annotations:
      version: 2.0.3_354
    labels:
      addonmanager.kubernetes.io/mode: Reconcile
      app: ibm-vpc-block-csi-driver
    name: vpc-block-provisioner-binding
  roleRef:
    apiGroup: rbac.authorization.k8s.io
    kind: ClusterRole
    name: vpc-block-provisioner-role
  subjects:
  - kind: ServiceAccount
    name: ibm-vpc-block-controller-sa
    namespace: kube-system
- apiVersion: rbac.authorization.k8s.io/v1
  kind: ClusterRole
  metadata:
    annotations:
      version: 2.0.3_354
    labels:
      addonmanager.kubernetes.io/mode: Reconcile
      app: ibm-vpc-block-csi-driver
    name: vpc-block-external-attacher-role
  rules:
  - apiGroups:
    - ""
    resources:
    - persistentvolumes
    verbs:
    - get
    - list
    - watch
    - update
    - patch
  - apiGroups:
    - ""
    resources:
    - nodes
    verbs:
    - get
    - list
    - watch
  - apiGroups:
    - csi.storage.k8s.io
    resources:
    - csinodeinfos
    verbs:
    - get
    - list
    - watch
  - apiGroups:
    - storage.k8s.io
    resources:
    - volumeattachments
    verbs:
    - get
    - list
    - watch
    - update
    - patch
- apiVersion: rbac.authorization.k8s.io/v1
  kind: ClusterRoleBinding
  metadata:
    annotations:
      version: 2.0.3_354
    labels:
      addonmanager.kubernetes.io/mode: Reconcile
      app: ibm-vpc-block-csi-driver
    name: vpc-block-external-attacher-binding
  roleRef:
    apiGroup: rbac.authorization.k8s.io
    kind: ClusterRole
    name: vpc-block-external-attacher-role
  subjects:
  - kind: ServiceAccount
    name: ibm-vpc-block-controller-sa
    namespace: kube-system
- apiVersion: storage.k8s.io/v1beta1
  kind: CSIDriver
  metadata:
    name: vpc.block.csi.ibm.io
  spec:
    attachRequired: true
    podInfoOnMount: true
    volumeLifecycleModes:
    - Persistent
- apiVersion: apps/v1
  kind: DaemonSet
  metadata:
    annotations:
      version: 2.0.3_354
    labels:
      addonmanager.kubernetes.io/mode: Reconcile
      app: ibm-vpc-block-csi-driver
    name: ibm-vpc-block-csi-node
    namespace: kube-system
  spec:
    selector:
      matchLabels:
        app: ibm-vpc-block-csi-driver
    template:
      metadata:
        labels:
          app: ibm-vpc-block-csi-driver
      spec:
        containers:
        - args:
          - --v=5
          - --csi-address=$(ADDRESS)
          - --kubelet-registration-path=$(DRIVER_REGISTRATION_SOCK)
          env:
          - name: ADDRESS
            value: /csi/csi.sock
          - name: DRIVER_REGISTRATION_SOCK
            value: /var/lib/kubelet/plugins/vpc.block.csi.ibm.io/csi.sock
          - name: KUBE_NODE_NAME
            valueFrom:
              fieldRef:
                fieldPath: spec.nodeName
          image: quay.io/k8scsi/csi-node-driver-registrar:v1.2.0
          imagePullPolicy: Always
          lifecycle:
            preStop:
              exec:
                command:
                - /bin/sh
                - -c
                - rm -rf /registration/vpc.block.csi.ibm.io /registration/vpc.block.csi.ibm.io-reg.sock
          name: csi-driver-registrar
          securityContext:
            runAsNonRoot: false
            runAsUser: 0
            privileged: false
          resources:
            limits:
              cpu: 100m
              memory: 100Mi
            requests:
              cpu: 10m
              memory: 20Mi
          volumeMounts:
          - mountPath: /csi
            name: plugin-dir
          - mountPath: /registration
            name: registration-dir
        - args:
          - --v=5
          - --endpoint=unix:/csi/csi.sock
          env:
          - name: KUBE_NODE_NAME
            valueFrom:
              fieldRef:
                fieldPath: spec.nodeName
          envFrom:
          - configMapRef:
              name: ibm-vpc-block-csi-configmap
          image: icr.io/ibm/ibm-vpc-block-csi-driver:v3.0.0
          imagePullPolicy: Always
          livenessProbe:
            failureThreshold: 5
            httpGet:
              path: /healthz
              port: healthz
            initialDelaySeconds: 10
            periodSeconds: 10
            timeoutSeconds: 3
          name: iks-vpc-block-node-driver
          ports:
          - containerPort: 9808
            name: healthz
            protocol: TCP
          resources:
            limits:
              cpu: 200m
              memory: 250Mi
            requests:
              cpu: 20m
              memory: 50Mi
          securityContext:
            runAsNonRoot: false
            runAsUser: 0
            privileged: true
          volumeMounts:
          - mountPath: /var/lib/kubelet
            mountPropagation: Bidirectional
            name: kubelet-data-dir
          - mountPath: /csi
            name: plugin-dir
          - mountPath: /dev
            name: device-dir
          - mountPath: /etc/udev
            name: etcudevpath
          - mountPath: /run/udev
            name: runudevpath
          - mountPath: /lib/udev
            name: libudevpath
          - mountPath: /sys
            name: syspath
          - mountPath: /etc/storage_ibmc
            name: customer-auth
            readOnly: true
          - mountPath: /etc/storage_ibmc/cluster_info
            name: cluster-info
            readOnly: true
        - args:
          - --csi-address=/csi/csi.sock
          image: quay.io/k8scsi/livenessprobe:v2.0.0
          name: liveness-probe
          securityContext:
              runAsNonRoot: false
              runAsUser: 0
              privileged: false
          resources:
            limits:
              cpu: 50m
              memory: 50Mi
            requests:
              cpu: 5m
              memory: 10Mi
          volumeMounts:
          - mountPath: /csi
            name: plugin-dir
        serviceAccountName: ibm-vpc-block-node-sa
        tolerations:
        - operator: Exists
        volumes:
        - hostPath:
            path: /var/lib/kubelet/plugins_registry/
            type: Directory
          name: registration-dir
        - hostPath:
            path: /var/lib/kubelet
            type: Directory
          name: kubelet-data-dir
        - hostPath:
            path: /var/lib/kubelet/plugins/vpc.block.csi.ibm.io/
            type: DirectoryOrCreate
          name: plugin-dir
        - hostPath:
            path: /dev
            type: Directory
          name: device-dir
        - hostPath:
            path: /etc/udev
            type: Directory
          name: etcudevpath
        - hostPath:
            path: /run/udev
            type: Directory
          name: runudevpath
        - hostPath:
            path: /lib/udev
            type: Directory
          name: libudevpath
        - hostPath:
            path: /sys
            type: Directory
          name: syspath
        - name: customer-auth
          secret:
            secretName: storage-secret-store
        - configMap:
            name: cluster-info
          name: cluster-info
- apiVersion: apps/v1
  kind: StatefulSet
  metadata:
    annotations:
      version: 2.0.3_354
    labels:
      addonmanager.kubernetes.io/mode: Reconcile
      app: ibm-vpc-block-csi-driver
    name: ibm-vpc-block-csi-controller
    namespace: kube-system
  spec:
    replicas: 1
    selector:
      matchLabels:
        app: ibm-vpc-block-csi-driver
    serviceName: ibm-vpc-block-service
    template:
      metadata:
        labels:
          app: ibm-vpc-block-csi-driver
      spec:
        securityContext:
            runAsNonRoot: true
            runAsUser: 2121
        containers:
        - args:
          - --v=5
          - --csi-address=$(ADDRESS)
          - --timeout=600s
          - --feature-gates=Topology=true
          env:
          - name: ADDRESS
            value: /csi/csi.sock
          image: quay.io/k8scsi/csi-provisioner:v1.6.0
          securityContext:
            privileged: false
            allowPrivilegeEscalation: false
          imagePullPolicy: Always
          name: csi-provisioner
          resources:
            limits:
              cpu: 100m
              memory: 100Mi
            requests:
              cpu: 10m
              memory: 20Mi
          volumeMounts:
          - mountPath: /csi
            name: socket-dir
        - args:
          - --v=5
          - --csi-address=/csi/csi.sock
          - --timeout=900s
          image: quay.io/k8scsi/csi-attacher:v2.2.0
          securityContext:
            privileged: false
            allowPrivilegeEscalation: false
          imagePullPolicy: Always
          name: csi-attacher
          resources:
            limits:
              cpu: 100m
              memory: 100Mi
            requests:
              cpu: 10m
              memory: 20Mi
          volumeMounts:
          - mountPath: /csi
            name: socket-dir
        - args:
          - --csi-address=/csi/csi.sock
          image: quay.io/k8scsi/livenessprobe:v2.0.0
          name: liveness-probe
          securityContext:
            privileged: false
            allowPrivilegeEscalation: false
          resources:
            limits:
              cpu: 50m
              memory: 50Mi
            requests:
              cpu: 5m
              memory: 10Mi
          volumeMounts:
          - mountPath: /csi
            name: socket-dir
        - args:
          - --v=5
          - --endpoint=$(CSI_ENDPOINT)
          - --lock_enabled=false
          env:
          - name: POD_NAME
            valueFrom:
              fieldRef:
                fieldPath: metadata.name
          - name: POD_NAMESPACE
            valueFrom:
              fieldRef:
                fieldPath: metadata.namespace
          envFrom:
          - configMapRef:
              name: ibm-vpc-block-csi-configmap
          image: icr.io/ibm/ibm-vpc-block-csi-driver:v3.0.0
          imagePullPolicy: Always
          securityContext:
            privileged: false
            allowPrivilegeEscalation: false
          livenessProbe:
            failureThreshold: 5
            httpGet:
              path: /healthz
              port: healthz
            initialDelaySeconds: 10
            periodSeconds: 10
            timeoutSeconds: 3
          name: iks-vpc-block-driver
          ports:
          - containerPort: 9808
            name: healthz
            protocol: TCP
          resources:
            limits:
              cpu: 500m
              memory: 500Mi
            requests:
              cpu: 50m
              memory: 100Mi
          volumeMounts:
          - mountPath: /csi
            name: socket-dir
          - mountPath: /etc/storage_ibmc
            name: customer-auth
            readOnly: true
          - mountPath: /etc/storage_ibmc/cluster_info
            name: cluster-info
            readOnly: true
        serviceAccountName: ibm-vpc-block-controller-sa
        volumes:
        - emptyDir: {}
          name: socket-dir
        - name: customer-auth
          secret:
            secretName: storage-secret-store
        - configMap:
            name: cluster-info
          name: cluster-info
    volumeClaimTemplates: []
- apiVersion: storage.k8s.io/v1
  kind: StorageClass
  metadata:
    annotations:
      storageclass.beta.kubernetes.io/is-default-class: "true"
      version: 2.0.3_354
    labels:
      addonmanager.kubernetes.io/mode: Reconcile
      app: ibm-vpc-block-csi-driver
      razee/force-apply: "true"
    name: ibmc-vpc-block-10iops-tier
  parameters:
    billingType: hourly
    classVersion: "1"
    csi.storage.k8s.io/fstype: ext4
    encrypted: "false"
    encryptionKey: ""
    profile: 10iops-tier
    region: ""
    resourceGroup: ""
    sizeRange: '[10-2000]GiB'
    tags: ""
    zone: ""
  provisioner: vpc.block.csi.ibm.io
  reclaimPolicy: Delete
- apiVersion: storage.k8s.io/v1
  kind: StorageClass
  metadata:
    annotations:
      version: 2.0.3_354
    labels:
      addonmanager.kubernetes.io/mode: Reconcile
      app: ibm-vpc-block-csi-driver
      razee/force-apply: "true"
    name: ibmc-vpc-block-5iops-tier
  parameters:
    billingType: hourly
    classVersion: "1"
    csi.storage.k8s.io/fstype: ext4
    encrypted: "false"
    encryptionKey: ""
    profile: 5iops-tier
    region: ""
    resourceGroup: ""
    sizeRange: '[10-2000]GiB'
    tags: ""
    zone: ""
  provisioner: vpc.block.csi.ibm.io
  reclaimPolicy: Delete
- apiVersion: storage.k8s.io/v1
  kind: StorageClass
  metadata:
    annotations:
      version: 2.0.3_354
    labels:
      addonmanager.kubernetes.io/mode: Reconcile
      app: ibm-vpc-block-csi-driver
      razee/force-apply: "true"
    name: ibmc-vpc-block-custom
  parameters:
    billingType: hourly
    classVersion: "1"
    csi.storage.k8s.io/fstype: ext4
    encrypted: "false"
    encryptionKey: ""
    iops: "400"
    profile: custom
    region: ""
    resourceGroup: ""
    sizeIOPSRange: |-
      [10-39]GiB:[100-1000]
      [40-79]GiB:[100-2000]
      [80-99]GiB:[100-4000]
      [100-499]GiB:[100-6000]
      [500-999]GiB:[100-10000]
      [1000-1999]GiB:[100-20000]
    sizeRange: '[10-2000]GiB'
    tags: ""
    zone: ""
  provisioner: vpc.block.csi.ibm.io
  reclaimPolicy: Delete
- apiVersion: storage.k8s.io/v1
  kind: StorageClass
  metadata:
    annotations:
      version: 2.0.3_354
    labels:
      addonmanager.kubernetes.io/mode: Reconcile
      app: ibm-vpc-block-csi-driver
      razee/force-apply: "true"
    name: ibmc-vpc-block-general-purpose
  parameters:
    billingType: hourly
    classVersion: "1"
    csi.storage.k8s.io/fstype: ext4
    encrypted: "false"
    encryptionKey: ""
    profile: general-purpose
    region: ""
    resourceGroup: ""
    sizeRange: '[10-2000]GiB'
    tags: ""
    zone: ""
  provisioner: vpc.block.csi.ibm.io
  reclaimPolicy: Delete
- apiVersion: storage.k8s.io/v1
  kind: StorageClass
  metadata:
    annotations:
      version: 2.0.3_354
    labels:
      addonmanager.kubernetes.io/mode: Reconcile
      app: ibm-vpc-block-csi-driver
      razee/force-apply: "true"
    name: ibmc-vpc-block-retain-10iops-tier
  parameters:
    billingType: hourly
    classVersion: "1"
    csi.storage.k8s.io/fstype: ext4
    encrypted: "false"
    encryptionKey: ""
    profile: 10iops-tier
    region: ""
    resourceGroup: ""
    sizeRange: '[10-2000]GiB'
    tags: ""
    zone: ""
  provisioner: vpc.block.csi.ibm.io
  reclaimPolicy: Retain
- apiVersion: storage.k8s.io/v1
  kind: StorageClass
  metadata:
    annotations:
      version: 2.0.3_354
    labels:
      addonmanager.kubernetes.io/mode: Reconcile
      app: ibm-vpc-block-csi-driver
      razee/force-apply: "true"
    name: ibmc-vpc-block-retain-5iops-tier
  parameters:
    billingType: hourly
    classVersion: "1"
    csi.storage.k8s.io/fstype: ext4
    encrypted: "false"
    encryptionKey: ""
    profile: 5iops-tier
    region: ""
    resourceGroup: ""
    sizeRange: '[10-2000]GiB'
    tags: ""
    zone: ""
  provisioner: vpc.block.csi.ibm.io
  reclaimPolicy: Retain
- apiVersion: storage.k8s.io/v1
  kind: StorageClass
  metadata:
    annotations:
      version: 2.0.3_354
    labels:
      addonmanager.kubernetes.io/mode: Reconcile
      app: ibm-vpc-block-csi-driver
      razee/force-apply: "true"
    name: ibmc-vpc-block-retain-custom
  parameters:
    billingType: hourly
    classVersion: "1"
    csi.storage.k8s.io/fstype: ext4
    encrypted: "false"
    encryptionKey: ""
    iops: "400"
    profile: custom
    region: ""
    resourceGroup: ""
    sizeIOPSRange: |-
      [10-39]GiB:[100-1000]
      [40-79]GiB:[100-2000]
      [80-99]GiB:[100-4000]
      [100-499]GiB:[100-6000]
      [500-999]GiB:[100-10000]
      [1000-1999]GiB:[100-20000]
    sizeRange: '[10-2000]GiB'
    tags: ""
    zone: ""
  provisioner: vpc.block.csi.ibm.io
  reclaimPolicy: Retain
- apiVersion: storage.k8s.io/v1
  kind: StorageClass
  metadata:
    annotations:
      version: 2.0.3_354
    labels:
      addonmanager.kubernetes.io/mode: Reconcile
      app: ibm-vpc-block-csi-driver
      razee/force-apply: "true"
    name: ibmc-vpc-block-retain-general-purpose
  parameters:
    billingType: hourly
    classVersion: "1"
    csi.storage.k8s.io/fstype: ext4
    encrypted: "false"
    encryptionKey: ""
    profile: general-purpose
    region: ""
    resourceGroup: ""
    sizeRange: '[10-2000]GiB'
    tags: ""
    zone: ""
  provisioner: vpc.block.csi.ibm.io
  reclaimPolicy: Retain
kind: List
metadata:
  annotations:
    version: 2.0.3_354
  name: ibm-vpc-block-csi-driver
  namespace: kube-system


```


{: codeblock}

### Ubuntu configmap
{: #vpc-block-ubuntu-cm}

Save the following the YAML configuration as a file on your local machine.

```yaml
apiVersion: v1
items:
- apiVersion: v1
  data:
    CSI_ENDPOINT: unix:/csi/csi.sock
    IKS_BLOCK_PROVIDER_NAME: iks-vpc
    IKS_ENABLED: "False"
    SECRET_CONFIG_PATH: /etc/storage_ibmc
    VPC_API_GENERATION: "1"
    VPC_API_TIMEOUT: 180s
    VPC_API_VERSION: "2019-07-02"
    VPC_BLOCK_PROVIDER_NAME: vpc
    VPC_ENABLED: "True"
    VPC_RETRY_ATTEMPT: "10"
    VPC_RETRY_INTERVAL: "120"
  kind: ConfigMap
  metadata:
    annotations:
      version: 2.0.2_285
    labels:
      addonmanager.kubernetes.io/mode: Reconcile
      app: ibm-vpc-block-csi-driver
    name: ibm-vpc-block-csi-configmap
    namespace: kube-system
- apiVersion: v1
  data:
    cluster-config.json: |
      {}
  kind: ConfigMap
  metadata:
    annotations:
    name: cluster-info
    namespace: kube-system
- apiVersion: v1
  data:
    slclient.toml: # Enter the base64 encoded TOML file that you created earlier.
  kind: Secret
  metadata:
    labels:
      addonmanager.kubernetes.io/mode: Reconcile
      app: ibm-vpc-block-csi-driver
      kubernetes.io/cluster-service: "true"
    name: storage-secret-store
    namespace: kube-system
  type: Opaque
- apiVersion: v1
  imagePullSecrets:
  - name: bluemix-default-secret
  - name: bluemix-default-secret-regional
  - name: bluemix-default-secret-international
  - name: icr-io-secret
  kind: ServiceAccount
  metadata:
    annotations:
      version: 2.0.2_285
    labels:
      addonmanager.kubernetes.io/mode: Reconcile
      app: ibm-vpc-block-csi-driver
    name: ibm-vpc-block-node-sa
    namespace: kube-system
- apiVersion: rbac.authorization.k8s.io/v1
  kind: ClusterRole
  metadata:
    annotations:
      version: 2.0.2_285
    labels:
      addonmanager.kubernetes.io/mode: Reconcile
      app: ibm-vpc-block-csi-driver
    name: vpc-block-driver-registrar-role
  rules:
  - apiGroups:
    - ""
    resources:
    - events
    verbs:
    - get
    - list
    - watch
    - create
    - update
    - patch
  - apiGroups:
    - ""
    resources:
    - nodes
    verbs:
    - get
    - list
    - watch
  - apiGroups:
    - ""
    resources:
    - persistentvolumes
    verbs:
    - get
    - list
    - watch
    - create
    - delete
  - apiGroups:
    - ""
    resources:
    - persistentvolumeclaims
    verbs:
    - get
    - list
    - watch
    - update
- apiVersion: rbac.authorization.k8s.io/v1
  kind: ClusterRoleBinding
  metadata:
    annotations:
      version: 2.0.2_285
    labels:
      addonmanager.kubernetes.io/mode: Reconcile
      app: ibm-vpc-block-csi-driver
    name: vpc-block-driver-registrar-binding
  roleRef:
    apiGroup: rbac.authorization.k8s.io
    kind: ClusterRole
    name: vpc-block-driver-registrar-role
  subjects:
  - kind: ServiceAccount
    name: ibm-vpc-block-node-sa
    namespace: kube-system
- apiVersion: v1
  imagePullSecrets:
  - name: bluemix-default-secret
  - name: bluemix-default-secret-regional
  - name: bluemix-default-secret-international
  - name: icr-io-secret
  kind: ServiceAccount
  metadata:
    annotations:
      version: 2.0.2_285
    labels:
      addonmanager.kubernetes.io/mode: Reconcile
      app: ibm-vpc-block-csi-driver
    name: ibm-vpc-block-controller-sa
    namespace: kube-system
- apiVersion: rbac.authorization.k8s.io/v1
  kind: ClusterRole
  metadata:
    annotations:
      version: 2.0.2_285
    labels:
      addonmanager.kubernetes.io/mode: Reconcile
      app: ibm-vpc-block-csi-driver
    name: vpc-block-provisioner-role
  rules:
  - apiGroups:
    - ""
    resources:
    - secrets
    verbs:
    - get
    - list
  - apiGroups:
    - ""
    resources:
    - persistentvolumes
    verbs:
    - get
    - list
    - watch
    - create
    - delete
  - apiGroups:
    - ""
    resources:
    - persistentvolumeclaims
    verbs:
    - get
    - list
    - watch
    - update
  - apiGroups:
    - storage.k8s.io
    resources:
    - storageclasses
    verbs:
    - get
    - list
    - watch
  - apiGroups:
    - ""
    resources:
    - events
    verbs:
    - list
    - watch
    - create
    - update
    - patch
  - apiGroups:
    - snapshot.storage.k8s.io
    resources:
    - volumesnapshots
    verbs:
    - get
    - list
  - apiGroups:
    - snapshot.storage.k8s.io
    resources:
    - volumesnapshotcontents
    verbs:
    - get
    - list
  - apiGroups:
    - storage.k8s.io
    resources:
    - csinodes
    verbs:
    - get
    - list
    - watch
  - apiGroups:
    - ""
    resources:
    - nodes
    verbs:
    - get
    - list
    - watch
- apiVersion: rbac.authorization.k8s.io/v1
  kind: ClusterRoleBinding
  metadata:
    annotations:
      version: 2.0.2_285
    labels:
      addonmanager.kubernetes.io/mode: Reconcile
      app: ibm-vpc-block-csi-driver
    name: vpc-block-provisioner-binding
  roleRef:
    apiGroup: rbac.authorization.k8s.io
    kind: ClusterRole
    name: vpc-block-provisioner-role
  subjects:
  - kind: ServiceAccount
    name: ibm-vpc-block-controller-sa
    namespace: kube-system
- apiVersion: rbac.authorization.k8s.io/v1
  kind: ClusterRole
  metadata:
    annotations:
      version: 2.0.2_285
    labels:
      addonmanager.kubernetes.io/mode: Reconcile
      app: ibm-vpc-block-csi-driver
    name: vpc-block-external-attacher-role
  rules:
  - apiGroups:
    - ""
    resources:
    - persistentvolumes
    verbs:
    - get
    - list
    - watch
    - update
    - patch
  - apiGroups:
    - ""
    resources:
    - nodes
    verbs:
    - get
    - list
    - watch
  - apiGroups:
    - csi.storage.k8s.io
    resources:
    - csinodeinfos
    verbs:
    - get
    - list
    - watch
  - apiGroups:
    - storage.k8s.io
    resources:
    - volumeattachments
    verbs:
    - get
    - list
    - watch
    - update
    - patch
- apiVersion: rbac.authorization.k8s.io/v1
  kind: ClusterRoleBinding
  metadata:
    annotations:
      version: 2.0.2_285
    labels:
      addonmanager.kubernetes.io/mode: Reconcile
      app: ibm-vpc-block-csi-driver
    name: vpc-block-external-attacher-binding
  roleRef:
    apiGroup: rbac.authorization.k8s.io
    kind: ClusterRole
    name: vpc-block-external-attacher-role
  subjects:
  - kind: ServiceAccount
    name: ibm-vpc-block-controller-sa
    namespace: kube-system
- apiVersion: apps/v1
  kind: DaemonSet
  metadata:
    annotations:
      version: 2.0.2_285
    labels:
      addonmanager.kubernetes.io/mode: Reconcile
      app: ibm-vpc-block-csi-driver
    name: ibm-vpc-block-csi-node
    namespace: kube-system
  spec:
    selector:
      matchLabels:
        app: ibm-vpc-block-csi-driver
    template:
      metadata:
        labels:
          app: ibm-vpc-block-csi-driver
      spec:
        containers:
        - args:
          - --v=5
          - --csi-address=$(ADDRESS)
          - --kubelet-registration-path=$(DRIVER_REGISTRATION_SOCK)
          env:
          - name: ADDRESS
            value: /csi/csi.sock
          - name: DRIVER_REGISTRATION_SOCK
            value: /var/lib/kubelet/csi-plugins/vpc.block.csi.ibm.io/csi.sock
          - name: KUBE_NODE_NAME
            valueFrom:
              fieldRef:
                fieldPath: spec.nodeName
          image: quay.io/k8scsi/csi-node-driver-registrar:v1.2.0
          lifecycle:
            preStop:
              exec:
                command:
                - /bin/sh
                - -c
                - rm -rf /registration/vpc.block.csi.ibm.io /registration/vpc.block.csi.ibm.io-reg.sock
          name: csi-driver-registrar
          securityContext:
            runAsNonRoot: false
            runAsUser: 0
            privileged: false
          volumeMounts:
          - mountPath: /csi
            name: plugin-dir
          - mountPath: /registration
            name: registration-dir
        - args:
          - --v=5
          - --endpoint=unix:/csi/csi.sock
          env:
          - name: KUBE_NODE_NAME
            valueFrom:
              fieldRef:
                fieldPath: spec.nodeName
          envFrom:
          - configMapRef:
              name: ibm-vpc-block-csi-configmap
          image: icr.io/ibm/ibm-vpc-block-csi-driver:v3.0.0
          imagePullPolicy: IfNotPresent
          name: iks-vpc-block-node-driver
          securityContext:
            runAsNonRoot: false
            runAsUser: 0
            privileged: true
          volumeMounts:
          - mountPath: /var/lib/kubelet
            mountPropagation: Bidirectional
            name: kubelet-data-dir
          - mountPath: /csi
            name: plugin-dir
          - mountPath: /dev
            name: device-dir
          - mountPath: /etc/udev
            name: etcudevpath
          - mountPath: /run/udev
            name: runudevpath
          - mountPath: /lib/udev
            name: libudevpath
          - mountPath: /sys
            name: syspath
          - mountPath: /etc/storage_ibmc
            name: customer-auth
            readOnly: true
          - mountPath: /etc/storage_ibmc/cluster_info
            name: cluster-info
            readOnly: true
        serviceAccountName: ibm-vpc-block-node-sa
        volumes:
        - hostPath:
            path: /var/lib/kubelet/plugins_registry/
            type: Directory
          name: registration-dir
        - hostPath:
            path: /var/lib/kubelet
            type: Directory
          name: kubelet-data-dir
        - hostPath:
            path: /var/lib/kubelet/csi-plugins/vpc.block.csi.ibm.io/
            type: DirectoryOrCreate
          name: plugin-dir
        - hostPath:
            path: /dev
            type: Directory
          name: device-dir
        - hostPath:
            path: /etc/udev
            type: Directory
          name: etcudevpath
        - hostPath:
            path: /run/udev
            type: Directory
          name: runudevpath
        - hostPath:
            path: /lib/udev
            type: Directory
          name: libudevpath
        - hostPath:
            path: /sys
            type: Directory
          name: syspath
        - name: customer-auth
          secret:
            secretName: storage-secret-store
        - configMap:
            name: cluster-info
          name: cluster-info
- apiVersion: apps/v1
  kind: StatefulSet
  metadata:
    annotations:
      version: 2.0.2_285
    labels:
      addonmanager.kubernetes.io/mode: Reconcile
      app: ibm-vpc-block-csi-driver
    name: ibm-vpc-block-csi-controller
    namespace: kube-system
  spec:
    replicas: 1
    selector:
      matchLabels:
        app: ibm-vpc-block-csi-driver
    serviceName: ibm-vpc-block-service
    template:
      metadata:
        labels:
          app: ibm-vpc-block-csi-driver
      spec:
        securityContext:
          runAsNonRoot: true
          runAsUser: 2121
        containers:
        - args:
          - --v=5
          - --csi-address=$(ADDRESS)
          - --timeout=600s
          - --feature-gates=Topology=true
          env:
          - name: ADDRESS
            value: /csi/csi.sock
          image: quay.io/k8scsi/csi-provisioner:v1.3.1
          securityContext:
            privileged: false
            allowPrivilegeEscalation: false
          name: csi-provisioner
          volumeMounts:
          - mountPath: /csi
            name: socket-dir
        - args:
          - --v=5
          - --csi-address=/csi/csi.sock
          - --timeout=900s
          image: quay.io/k8scsi/csi-attacher:v2.0.0
          name: csi-attacher
          securityContext:
            privileged: false
            allowPrivilegeEscalation: false
          volumeMounts:
          - mountPath: /csi
            name: socket-dir
        - args:
          - --v=5
          - --endpoint=$(CSI_ENDPOINT)
          env:
          - name: POD_NAME
            valueFrom:
              fieldRef:
                fieldPath: metadata.name
          - name: POD_NAMESPACE
            valueFrom:
              fieldRef:
                fieldPath: metadata.namespace
          envFrom:
          - configMapRef:
              name: ibm-vpc-block-csi-configmap
          image: icr.io/ibm/ibm-vpc-block-csi-driver:v3.0.0
          securityContext:
            privileged: false
            allowPrivilegeEscalation: false
          imagePullPolicy: IfNotPresent
          name: iks-vpc-block-driver
          volumeMounts:
          - mountPath: /csi
            name: socket-dir
          - mountPath: /etc/storage_ibmc
            name: customer-auth
            readOnly: true
          - mountPath: /etc/storage_ibmc/cluster_info
            name: cluster-info
            readOnly: true
        serviceAccountName: ibm-vpc-block-controller-sa
        volumes:
        - emptyDir: {}
          name: socket-dir
        - name: customer-auth
          secret:
            secretName: storage-secret-store
        - configMap:
            name: cluster-info
          name: cluster-info
    volumeClaimTemplates: []
- apiVersion: storage.k8s.io/v1
  kind: StorageClass
  metadata:
    annotations:
      storageclass.beta.kubernetes.io/is-default-class: "true"
      version: 2.0.2_285
    labels:
      addonmanager.kubernetes.io/mode: Reconcile
      app: ibm-vpc-block-csi-driver
      razee/force-apply: "true"
    name: ibmc-vpc-block-10iops-tier
  parameters:
    billingType: hourly
    classVersion: "1"
    csi.storage.k8s.io/fstype: ext4
    encrypted: "false"
    encryptionKey: ""
    profile: 10iops-tier
    region: ""
    resourceGroup: ""
    sizeRange: '[10-2000]GiB'
    tags: ""
    zone: ""
  provisioner: vpc.block.csi.ibm.io
  reclaimPolicy: Delete
- apiVersion: storage.k8s.io/v1
  kind: StorageClass
  metadata:
    annotations:
      version: 2.0.2_285
    labels:
      addonmanager.kubernetes.io/mode: Reconcile
      app: ibm-vpc-block-csi-driver
      razee/force-apply: "true"
    name: ibmc-vpc-block-5iops-tier
  parameters:
    billingType: hourly
    classVersion: "1"
    csi.storage.k8s.io/fstype: ext4
    encrypted: "false"
    encryptionKey: ""
    profile: 5iops-tier
    region: ""
    resourceGroup: ""
    sizeRange: '[10-2000]GiB'
    tags: ""
    zone: ""
  provisioner: vpc.block.csi.ibm.io
  reclaimPolicy: Delete
- apiVersion: storage.k8s.io/v1
  kind: StorageClass
  metadata:
    annotations:
      version: 2.0.2_285
    labels:
      addonmanager.kubernetes.io/mode: Reconcile
      app: ibm-vpc-block-csi-driver
      razee/force-apply: "true"
    name: ibmc-vpc-block-custom
  parameters:
    billingType: hourly
    classVersion: "1"
    csi.storage.k8s.io/fstype: ext4
    encrypted: "false"
    encryptionKey: ""
    iops: "400"
    profile: custom
    region: ""
    resourceGroup: ""
    sizeIOPSRange: |-
      [10-39]GiB:[100-1000]
      [40-79]GiB:[100-2000]
      [80-99]GiB:[100-4000]
      [100-499]GiB:[100-6000]
      [500-999]GiB:[100-10000]
      [1000-1999]GiB:[100-20000]
    sizeRange: '[10-2000]GiB'
    tags: ""
    zone: ""
  provisioner: vpc.block.csi.ibm.io
  reclaimPolicy: Delete
- apiVersion: storage.k8s.io/v1
  kind: StorageClass
  metadata:
    annotations:
      version: 2.0.2_285
    labels:
      addonmanager.kubernetes.io/mode: Reconcile
      app: ibm-vpc-block-csi-driver
      razee/force-apply: "true"
    name: ibmc-vpc-block-general-purpose
  parameters:
    billingType: hourly
    classVersion: "1"
    csi.storage.k8s.io/fstype: ext4
    encrypted: "false"
    encryptionKey: ""
    profile: general-purpose
    region: ""
    resourceGroup: ""
    sizeRange: '[10-2000]GiB'
    tags: ""
    zone: ""
  provisioner: vpc.block.csi.ibm.io
  reclaimPolicy: Delete
- apiVersion: storage.k8s.io/v1
  kind: StorageClass
  metadata:
    annotations:
      version: 2.0.2_285
    labels:
      addonmanager.kubernetes.io/mode: Reconcile
      app: ibm-vpc-block-csi-driver
      razee/force-apply: "true"
    name: ibmc-vpc-block-retain-10iops-tier
  parameters:
    billingType: hourly
    classVersion: "1"
    csi.storage.k8s.io/fstype: ext4
    encrypted: "false"
    encryptionKey: ""
    profile: 10iops-tier
    region: ""
    resourceGroup: ""
    sizeRange: '[10-2000]GiB'
    tags: ""
    zone: ""
  provisioner: vpc.block.csi.ibm.io
  reclaimPolicy: Retain
- apiVersion: storage.k8s.io/v1
  kind: StorageClass
  metadata:
    annotations:
      version: 2.0.2_285
    labels:
      addonmanager.kubernetes.io/mode: Reconcile
      app: ibm-vpc-block-csi-driver
      razee/force-apply: "true"
    name: ibmc-vpc-block-retain-5iops-tier
  parameters:
    billingType: hourly
    classVersion: "1"
    csi.storage.k8s.io/fstype: ext4
    encrypted: "false"
    encryptionKey: ""
    profile: 5iops-tier
    region: ""
    resourceGroup: ""
    sizeRange: '[10-2000]GiB'
    tags: ""
    zone: ""
  provisioner: vpc.block.csi.ibm.io
  reclaimPolicy: Retain
- apiVersion: storage.k8s.io/v1
  kind: StorageClass
  metadata:
    annotations:
      version: 2.0.2_285
    labels:
      addonmanager.kubernetes.io/mode: Reconcile
      app: ibm-vpc-block-csi-driver
      razee/force-apply: "true"
    name: ibmc-vpc-block-retain-custom
  parameters:
    billingType: hourly
    classVersion: "1"
    csi.storage.k8s.io/fstype: ext4
    encrypted: "false"
    encryptionKey: ""
    iops: "400"
    profile: custom
    region: ""
    resourceGroup: ""
    sizeIOPSRange: |-
      [10-39]GiB:[100-1000]
      [40-79]GiB:[100-2000]
      [80-99]GiB:[100-4000]
      [100-499]GiB:[100-6000]
      [500-999]GiB:[100-10000]
      [1000-1999]GiB:[100-20000]
    sizeRange: '[10-2000]GiB'
    tags: ""
    zone: ""
  provisioner: vpc.block.csi.ibm.io
  reclaimPolicy: Retain
- apiVersion: storage.k8s.io/v1
  kind: StorageClass
  metadata:
    annotations:
      version: 2.0.2_285
    labels:
      addonmanager.kubernetes.io/mode: Reconcile
      app: ibm-vpc-block-csi-driver
      razee/force-apply: "true"
    name: ibmc-vpc-block-retain-general-purpose
  parameters:
    billingType: hourly
    classVersion: "1"
    csi.storage.k8s.io/fstype: ext4
    encrypted: "false"
    encryptionKey: ""
    profile: general-purpose
    region: ""
    resourceGroup: ""
    sizeRange: '[10-2000]GiB'
    tags: ""
    zone: ""
  provisioner: vpc.block.csi.ibm.io
  reclaimPolicy: Retain
kind: List
metadata:
  annotations:
    version: 2.0.2_285
  name: ibm-vpc-block-csi-driver
  namespace: kube-system

```
{: codeblock}
