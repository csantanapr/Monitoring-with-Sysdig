---

copyright:
  years:  2018, 2020
lastupdated: "2020-02-12"

keywords: Sysdig, IBM Cloud, monitoring, access key

subcollection: Monitoring-with-Sysdig

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}
{:important: .important}
{:note: .note}

# Managing access keys
{: #access_key}

The **Access Key** is a token that you must use to configure Sysdig agents to successfully forward data to your {{site.data.keyword.mon_full_notm}} instance in {{site.data.keyword.cloud_notm}}.   
{:shortdesc}


## Getting the access key through the {{site.data.keyword.cloud_notm}} UI
{: #access_key_ibm_cloud_ui}

To get the access key for an {{site.data.keyword.mon_full_notm}} instance through the {{site.data.keyword.cloud_notm}} UI, complete the following steps:

1. [Log in to the {{site.data.keyword.cloud_notm}} console ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/login){:new_window}.

2. Go to the Menu icon ![Menu icon](../icons/icon_hamburger.svg) &gt; **Observability**. 

3. Select **Monitoring**. The {{site.data.keyword.mon_full_notm}} dashboard opens. You can see the list of monitoring instances that are available on {{site.data.keyword.cloud_notm}}.

3. Identify the instance for which you want to get the access key, and click **View access key**.

4. A pop up window opens where you can click **Show** to view the access key.



## Getting the access key through the CLI
{: #access_key_cli}

To get the access key for a Sysdig instance through the command line, complete the following steps:

1. [Pre-requisite] [Install the {{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cloud-cli-getting-started).

2. Log in to the region in the {{site.data.keyword.cloud_notm}} where the Sysdig instance is running. Run the following command: [`ibmcloud login`](/docs/cli/reference/ibmcloud?topic=cloud-cli-ibmcloud_cli#ibmcloud_login)

3. Set the resource group where the Sysdig instance is running. Run the following command: [`ibmcloud target`](/docs/cli/reference/ibmcloud?topic=cloud-cli-ibmcloud_cli#ibmcloud_target)

    By default, the `default` resource group is set.

4. Get the instance name. Run the following command: [`ibmcloud resource service-instances`](/docs/cli/reference/ibmcloud?topic=cloud-cli-ibmcloud_commands_resource#ibmcloud_resource_service_instances)

    ```
    ibmcloud resource service-instances
    ```
    {: pre}

5. Get the name of the API key that is associated with the Sysdig instance. Run the [`ibmcloud resource service-keys`](/docs/cli/reference/ibmcloud?topic=cloud-cli-ibmcloud_commands_resource#ibmcloud_resource_service_instances) command:

    ```
    ibmcloud resource service-keys --instance-name INSTANCE_NAME
    ```
    {: pre}

    where INSTANCE_NAME is the name of the instance that you obtained in the previous step.

6. Get the access key. Run the [`ibmcloud resource service-key`](/docs/cli/reference/ibmcloud?topic=cloud-cli-ibmcloud_commands_resource#ibmcloud_resource_service_key) command:

    ```
    ibmcloud resource service-key APIKEY_NAME
    ```
    {: pre}

    where APIKEY_NAME is the name of the API key.
 
    The output from this command includes the field **Sysdig Access Key** that contains the access key for the instance.





## Creating additional access keys
{: #access_key_create}

If the access key is compromised or you have a policy to renew it after a number of days, you can generate a new access key and disable the old one.

To create a new access key for an {{site.data.keyword.mon_full_notm}} instance, complete the following steps:

1. Obtain the API token from the {{site.data.keyword.mon_full_notm}} UI. [Learn more](/docs/Monitoring-with-Sysdig?topic=Monitoring-with-Sysdig-api_token#api_token_get).

2. Issue a curl POST request against the Sysdig endpoint to generate a new access key.

    ```
    curl -XPOST -H'Bearer: API_TOKEN' https:ENDPOINT/api/customer/accessKeys
    ```
    {: pre}

    Where

    * `ENDPOINT` is the URL for the region where the monitoring instance is available. For more information, see [Sysdig endpoints](/docs/Monitoring-with-Sysdig?topic=Monitoring-with-Sysdig-endpoints#endpoints).
    * `API_TOKEN` is the API token that you get in step 1.

    The output will provide the newly generated access key in the response.

    ```
    {
        "customerAccessKey": {
            "enabled": true,
            "accessKey": "12345678-1234-1234-1234-123456789012",
            "dateCreated": 1573852152224,
            "dateDisabled": null
        }
    }
    ```
    {: screen}

3. The access key can now be used in the Sysdig agent configuration files.

## Disabling an access key
{: #access_key_disable}

To disable an existing access key for an {{site.data.keyword.mon_full_notm}} instance, complete the following steps:

1. Obtain the API Token from the {{site.data.keyword.mon_full_notm}} UI ( [see instructions](/docs/Monitoring-with-Sysdig?topic=Monitoring-with-Sysdig-api_token#api_token_get) ).

2. Issue a curl POST request against the Sysdig endpoint to disable the given access key.

    ```
    curl -XPOST -H'Bearer: API_TOKEN' https:ENDPOINT/api/customer/accessKeys/ACCESS_KEY/disable
    ```
    {: pre}

    Where

    * `ENDPOINT` is the URL for the region where the monitoring instance is available. For more information, see [Sysdig endpoints](/docs/Monitoring-with-Sysdig?topic=Monitoring-with-Sysdig-endpoints#endpoints).
    * `API_TOKEN` is the API Token retrieved in step 1.
    * `ACCESS_KEY` is the access key that you wish to disable.

Once you disable the Sysdig access key, the agents connected with the access key will be immeditely blocked from sending metrics to this {{site.data.keyword.mon_full_notm}} instance.

There is no option to delete access keys at this time.
{: note}

## Enabling an access key
{: #access_key_enable}

To enable an existing access key for an {{site.data.keyword.mon_full_notm}} instance, complete the following steps:

1. Obtain the API Token from the {{site.data.keyword.mon_full_notm}} UI. [Learn more](/docs/Monitoring-with-Sysdig?topic=Monitoring-with-Sysdig-api_token#api_token_get).

2. Issue a curl POST request against the Sysdig endpoint to enable the given access key.

    ```
    curl -XPOST -H'Bearer: API_TOKEN' https://ENDPOINT/api/customer/accessKeys/ACCESS_KEY/enable
    ```
    {: pre}

    Where

    * `ENDPOINT` is the URL for the region where the monitoring instance is available. For more information, see [Sysdig endpoints](/docs/Monitoring-with-Sysdig?topic=Monitoring-with-Sysdig-endpoints#endpoints).
    * `API_TOKEN` is the API Token retrieved in step 1.
    * `ACCESS_KEY` is the access key that you wish to enable.


After you enable the Sysdig access key, the agents will need to be manually restarted since an agent that connects with a disabled access key will be terminated.
{: note}

## Viewing the available access keys
{: #access_key_view}

To view all of the access keys for an {{site.data.keyword.mon_full_notm}} instance, complete the following steps:

1. Obtain the API Token from the {{site.data.keyword.mon_full_notm}} UI. [Learn more](/docs/Monitoring-with-Sysdig?topic=Monitoring-with-Sysdig-api_token#api_token_get).

2. Issue a curl GET request against the regional monitoring endpoint to cause Sysdig to enable the given access key.

    ```
    curl -XGET -H'Bearer: API_TOKEN' https://ENDPOINT/api/customer/accessKeys
    ```
    {: pre}

    Where

    * `ENDPOINT` is the URL for the region where the monitoring instance is available. For more information, see [Sysdig endpoints](/docs/Monitoring-with-Sysdig?topic=Monitoring-with-Sysdig-endpoints#endpoints).
    * `API_TOKEN` is the API Token retrieved in Step 1.

    The output will provide a list of the access keys in the response and whether they are enabled.

    ```
    {
        "customerAccessKeys": [
            {
                "enabled": true,
                "accessKey": "12345678-1234-1234-1234-123456789012",
                "dateCreated": 1541096409000,
                "dateDisabled": null
            },
            {
                "enabled": false,
                "accessKey": "87654321-1234-1234-1234-123456789012",
                "dateCreated": 1573849361000,
                "dateDisabled": 1573849367000
            }
        ]
    }
    ```
    {: screen}
