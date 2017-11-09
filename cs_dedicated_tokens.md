---

copyright:
  years: 2014, 2017
lastupdated: "2017-11-02"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}


# Creating a {{site.data.keyword.registryshort_notm}} token for an {{site.data.keyword.Bluemix_dedicated_notm}} image registry
{: #cs_dedicated_tokens}

Create a non-expiring token to use an image registry that you used for single and scalable groups with clusters.
{:shortdesc}

1.  Log in to the {{site.data.keyword.Bluemix_dedicated_notm}} environment.

    ```
    bx login -a api.<dedicated_domain>
    ```
    {: pre}

2.  Request an `oauth-token` for the current session and save it as a variable.

    ```
    OAUTH_TOKEN=`bx iam oauth-tokens | awk 'FNR == 2 {print $3 " " $4}'`
    ```
    {: pre}

3.  Request the ID of the org for the current session and save it as a variable.

    ```
    ORG_GUID=`bx iam org <org_name> --guid`
    ```
    {: pre}

4.  Request a permanent registry token for the current session. Replace <dedicated_domain> with the domain for your {{site.data.keyword.Bluemix_dedicated_notm}} environment. This token grants access to the images in the current namespace.

    ```
    curl -XPOST -H "Authorization: ${OAUTH_TOKEN}" -H "Organization: ${ORG_GUID}" https://registry.<dedicated_domain>/api/v1/tokens?permanent=true
    ```
    {: pre}

    Output:

    ```
    {
        "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJqdGkiOiI2MzdiM2Q4Yy1hMDg3LTVhZjktYTYzNi0xNmU3ZWZjNzA5NjciLCJpc3MiOiJyZWdpc3RyeS5jZnNkZWRpY2F0ZWQxLnVzLXNvdXRoLmJsdWVtaXgubmV0"
    }
    ```
    {: screen}

5.  Verify the Kubernetes secret.

    ```
    kubectl describe secrets
    ```
    {: pre}

    You can use this secret to work with IBM {{site.data.keyword.Bluemix_notm}} Container Service.

6.  Create the Kubernetes secret to store your token information.

    ```
    kubectl --namespace <kubernetes_namespace> create secret docker-registry <secret_name>  --docker-server=<registry_url> --docker-username=token --docker-password=<token_value> --docker-email=<docker_email>
    ```
    {: pre}

    <table>
    <caption>Table 1. Understanding this command's components</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding this command's components</th>
    </thead>
    <tbody>
    <tr>
    <td><code>--namespace &lt;kubernetes_namespace&gt;</code></td>
    <td>Required. The Kubernetes namespace of your cluster where you want to use the secret and deploy containers to. Run <code>kubectl get namespaces</code> to list all namespaces in your cluster.</td>
    </tr>
    <tr>
    <td><code>&lt;secret_name&gt;</code></td>
    <td>Required. The name that you want to use for your imagePullSecret.</td>
    </tr>
    <tr>
    <td><code>--docker-server &lt;registry_url&gt;</code></td>
    <td>Required. The URL to the image registry where your namespace is set up: registry.&lt;dedicated_domain&gt;</li></ul></td>
    </tr>
    <tr>
    <td><code>--docker-username &lt;docker_username&gt;</code></td>
    <td>Required. The user name to log in to your private registry.</td>
    </tr>
    <tr>
    <td><code>--docker-password &lt;token_value&gt;</code></td>
    <td>Required. The value of your registry token that you retrieved earlier.</td>
    </tr>
    <tr>
    <td><code>--docker-email &lt;docker-email&gt;</code></td>
    <td>Required. If you have one, enter your Docker email address. If you do not have one, enter a fictional email address, as for example a@b.c. This email is mandatory to create a Kubernetes secret, but is not used after creation.</td>
    </tr>
    </tbody></table>

7.  Create a pod that references the imagePullSecret.

    1.  Open your preferred editor and create a pod configuration script that is named mypod.yaml.
    2.  Define the pod and the imagePullSecret that you want to use to access the registry. To use a private image from a namespace:

        ```
        apiVersion: v1
        kind: Pod
        metadata:
          name: <pod_name>
        spec:
          containers:
            - name: <container_name>
              image: registry.<dedicated_domain>/<my_namespace>/<my_image>:<tag>
          imagePullSecrets:
            - name: <secret_name>
        ```
        {: codeblock}

        <table>
        <caption>Table 2. Understanding the YAML file components</caption>
        <thead>
        <th colspan=2><img src="images/idea.png"/> Understanding the YAML file components</th>
        </thead>
        <tbody>
        <tr>
        <td><code>&lt;pod_name&gt;</code></td>
        <td>The name of the pod that you want to create.</td>
        </tr>
        <tr>
        <td><code>&lt;container_name&gt;</code></td>
        <td>The name of the container that you want to deploy to your cluster.</td>
        </tr>
        <tr>
        <td><code>&lt;my_namespace&gt;</code></td>
        <td>The namespace where your image is stored. To list available namespaces, run `bx cr namespace-list`.</td>
        </tr>
        <td><code>&lt;my_image&gt;</code></td>
        <td>The name of the image that you want to use. To list available images in an {{site.data.keyword.Bluemix_notm}} account, run <code>bx cr image-list</code>.</td>
        </tr>
        <tr>
        <td><code>&lt;tag&gt;</code></td>
        <td>The version of the image that you want to use. If no tag is specified, the image that is tagged <strong>latest</strong> is used by default.</td>
        </tr>
        <tr>
        <td><code>&lt;secret_name&gt;</code></td>
        <td>The name of the imagePullSecret that you created earlier.</td>
        </tr>
        </tbody></table>

    3.  Save your changes.

    4.  Create the deployment in your cluster.

          ```
          kubectl apply -f mypod.yaml
          ```
          {: pre}
