---

copyright:
  years: 2014, 2018
lastupdated: "2018-4-23"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}


# Accessing the IBM Cloud Infrastructure (SoftLayer) portfolio
{: #infrastructure}

To create a standard Kubernetes cluster, you must have access to the {[softlayer]} portfolio. This access is needed to request paid infrastructure resources, such as worker nodes, portable public IP addresses, or persistent storage for your Kubernetes cluster in {{site.data.keyword.containerlong}}.
{:shortdesc}

## Access the {[softlayer]} portfolio
{: #unify_accounts}

{{site.data.keyword.Bluemix_notm}} Pay-As-You-Go accounts that were created after automatic account linking was enabled are already set up with access to the {[softlayer]} portfolio. You can purchase infrastructure resources for your cluster without additional configuration.
{:shortdesc}

Users with other {{site.data.keyword.Bluemix_notm}} account types or users that have an existing {[softlayer]} account that is not linked to their {{site.data.keyword.Bluemix_notm}} account, must configure their accounts to create standard clusters.
{:shortdesc}

Review the following table to find available options for each account type.

|Account type|Description|Available options to create a standard cluster|
|------------|-----------|----------------------------------------------|
|Lite accounts|Lite accounts cannot provision clusters.|[Upgrade your Lite account to an {{site.data.keyword.Bluemix_notm}} Pay-As-You-Go account](/docs/account/index.html#billableacts) that is set up with access to the IBM Cloud infrastructure (SoftLayer) portfolio.|
|Older Pay-As-You-Go accounts|Pay-As-You-Go accounts that were created before automatic account linking was available, did not come with access to the {[softlayer]} portfolio.<p>If you have an existing {[softlayer]} account, you cannot link this account to an older Pay-As-You-Go account.</p>|Option 1: [Create a new Pay-As-You-Go account](/docs/account/index.html#billableacts) that is set up with access to the {[softlayer]} portfolio. When you choose this option, you have two separate {{site.data.keyword.Bluemix_notm}} accounts and billings.<p>If you want to continue using your old Pay-As-You-Go account to create standard clusters, you can use your new Pay-As-You-Go account to generate an API key to access the {[softlayer]} portfolio. Then, you must set the API key for your old Pay-As-You-Go account. For more information, see [Generating an API key for old Pay-As-You-Go and Subscription accounts](#old_account). Keep in mind that {[softlayer]} resources are billed through your new Pay-As-You-Go account.</p></br><p>Option 2: If you already have an existing {[softlayer]} account that you want to use, you can [set your credentials](cs_cli_reference.html#cs_credentials_set) for your {{site.data.keyword.Bluemix_notm}} account.</p><p>**Note:** The {[softlayer]} account that you use with your {{site.data.keyword.Bluemix_notm}} account must be set up with Super User permissions.</p>|
|Subscription accounts|Subscription accounts are not set up with access to the {[softlayer]} portfolio.|Option 1: [Create a new Pay-As-You-Go account](/docs/account/index.html#billableacts) that is set up with access to the {[softlayer]} portfolio. When you choose this option, you have two separate {{site.data.keyword.Bluemix_notm}} accounts and billings.<p>If you want to continue using your Subscription account to create standard clusters, you can use your new Pay-As-You-Go account to generate an API key to access the {[softlayer]} portfolio. Then, you must set the API key for your Subscription account. For more information, see [Generating an API key for old Pay-As-You-Go and Subscription accounts](#old_account). Keep in mind that {[softlayer]} resources are billed through your new Pay-As-You-Go account.</p></br><p>Option 2: If you already have an existing {[softlayer]} account that you want to use, you can [set your credentials](cs_cli_reference.html#cs_credentials_set) for your {{site.data.keyword.Bluemix_notm}} account.<p>**Note:** The {[softlayer]} account that you use with your {{site.data.keyword.Bluemix_notm}} account must be set up with Super User permissions.</p>|
|{[softlayer]} accounts, no {{site.data.keyword.Bluemix_notm}} account|To create a standard cluster, you must have an {{site.data.keyword.Bluemix_notm}} account.|<p>[Create a new Pay-As-You-Go account](/docs/account/index.html#billableacts) that is set up with access to the {[softlayer]} portfolio. When you choose this option, an {[softlayer]} is created for you. You have two separate {[softlayer]} accounts and billing.</p>|

{[white-space.md]}

## Generating an {[softlayer]} API key to use with {{site.data.keyword.Bluemix_notm}} accounts
{: #old_account}

To continue using your old Pay-As-You-Go or Subscription account to create standard clusters, generate an API key with your new Pay-As-You-Go account and set the API key for your old account.
{:shortdesc}

Before you begin, create an {{site.data.keyword.Bluemix_notm}} Pay-As-You-Go account that is automatically set up with access to the {[softlayer]} portfolio.

1.  Log in to the [{[softlayer]} portal ![External link icon](../icons/launch-glyph.svg "External link icon")](https://control.softlayer.com/) by using the {{site.data.keyword.ibmid}} and password that you created for your new Pay-As-You-Go account.
2.  Select **Account**, and then **Users**.
3.  Click **Generate** to generate an {[softlayer]} API key for your new Pay-As-You-Go account.
4.  Copy the API key.
5.  From the CLI, log in to {{site.data.keyword.Bluemix_notm}} by using the {{site.data.keyword.ibmid}} and password of your old Pay-As-You-Go or Subscription account.

  ```
  {[bx]} login
  ```
  {: pre}

6.  Set the API key that you generated earlier to access the {[softlayer]} portfolio. Replace `<API_key>` with the API key and `<username>` with the {{site.data.keyword.ibmid}} of your new Pay-As-You-Go account.

  ```
  {[bxcs]} credentials-set --infrastructure-api-key <API_key> --infrastructure-username <username>
  ```
  {: pre}

7.  Start [creating standard clusters](cs_clusters.html#clusters_cli).

**Note:** To review your API key after you generated it, follow step 1 and 2, and then in the **API key** section, click on **View** to see the API key for your user ID.

