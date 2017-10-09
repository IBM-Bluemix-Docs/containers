---

copyright:
  years: 2017
lastupdated: "2017-10-06"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}


# Automating access to your namespaces in {{site.data.keyword.registrylong_notm}} by using tokens
{: #registry_tokens}

You can use tokens to automate the pushing and pulling of Docker images to and from your namespace.
{:shortdesc}

Before you begin, [install the {{site.data.keyword.registrylong}} and Docker CLI](registry_setup_cli_namespace.html#registry_cli_install).

A security token allows everyone in possession of the token to access secured information. Tokens are used in a similar way to API keys. By creating a token for your {{site.data.keyword.Bluemix_notm}} account, you can grant access to all of your namespaces that you set up in a {{site.data.keyword.Bluemix_notm}} region for users outside your {{site.data.keyword.Bluemix_notm}} account. Every user or app in possession of this token can push and pull images to and from your namespaces without installing the container-registry plug-in.

When you create a token for your {{site.data.keyword.Bluemix_notm}} account, you can decide whether that token authorizes read-only (pull) or write access (push and pull) to the registry. You can also specify whether a token is permanent or if it expires after 24 hours. You can create and use multiple tokens to control different types of access.


## Creating a {{site.data.keyword.registrylong_notm}} token for your {{site.data.keyword.Bluemix_notm}} account
{: #registry_tokens_create}

You can create a token to grant access to all your namespaces of a {{site.data.keyword.Bluemix_notm}} region.
{:shortdesc}

1.  Create a token. The following example creates a non-expiring token that has read and write access to all namespaces that are set up in a {{site.data.keyword.Bluemix_notm}} region.

    ```
    bx cr token-add --description "This is a token" --non-expiring --readwrite
    ```
    {: pre}

    <table>
        <thead>
        <th colspan=2><img src="images/idea.png"/> Understanding the command components</th>
        </thead>
        <tbody>
        <tr>
        <td>`--description`</td>
        <td>Optional. Use this option to describe your token so that you can identify it more easily later.</td>
        </tr>
        <tr>
        <td>`--non-expiring`</td>
        <td>Optional. Use this option to create a non-expiring token. If you do not specify this option, the token becomes invalid after 24 hours. **Tip:** When you no longer need a non-expiring token to block access to your namespaces, remember to remove the token.</td>
        </tr>
        <tr>
        <td>`--readwrite`</td>
        <td>Optional. Use this option to create a token that allows users to push and pull images to and from your namespaces. If you do not specify this option, the token can be used to pull images only.</td>
        </tr>
        </tbody>
        </table>

    Your CLI output looks similar to the following output:

    ```
    Token identifier   58669dd6-3ddd-5c78-99f9-ad0a5aabd9ad   
    Token              <token_value>
    ```
    {: screen}

2.  Verify that the token was created.

    ```
    bx cr token-list
    ```
    {: pre}


## Using a token to automate access to a namespace in {{site.data.keyword.registrylong_notm}}
{: #registry_tokens_use}

You can use a token in your Docker login command to automate access to a namespace. Depending on whether you set read-only or read-write access for your token, users can push and pull images to and from your namespace.
{:shortdesc}

1.  Log in to {{site.data.keyword.Bluemix_notm}}.

    ```
    bx login
    ```
    {: pre}

2.  List all tokens in your account and note the token ID that you want to use.

    ```
    bx cr token-list
    ```
    {: pre}

3.  Retrieve the token value for the token. Replace &lt;token_id&gt; with the ID of the token.

    ```
    bx cr token-get <token_id>
    ```
    {: pre}

    Your token value is displayed in **Token** of your CLI output.

4.  Use the token as part of your Docker login. Replace &lt;token_value&gt; with the token value that you retrieved in the previous step and &lt;registry_url&gt; with the URL to the registry where your namespace is set up.

    -   For namespaces set up in US-South: registry.ng.bluemix.net
    -   For namespaces set up in UK-South: registry.eu-gb.bluemix.net
    -   For namespaces set up in EU-Central: registry.eu-de.bluemix.net
    -   For namespaces set up in AP-South: registry.au-syd.bluemix.net

    ```
    docker login -u token -p <token_value> <registry_url>
    ```
    {: pre}

    After you logged into Docker by using the token, you can now push or pull images to and from your namespace.


## Removing a {{site.data.keyword.registrylong_notm}} token from a {{site.data.keyword.Bluemix_notm}} account
{: #registry_tokens_remove}

Remove a token when you do not need it anymore.
{:shortdesc}

**Note:** Expired tokens are removed automatically from your {{site.data.keyword.Bluemix_notm}} account and do not need to be removed manually.

1.  Log in to {{site.data.keyword.Bluemix_notm}}.

    ```
    bx login
    ```
    {: pre}

2.  List all tokens in your account and note the token ID that you want to remove.

    ```
    bx cr token-list
    ```
    {: pre}

3.  Remove the token.

    ```
    bx cr token-rm <token_id>
    ```
    {: pre}
