---

copyright:
  years: 2018
lastupdated: "2018-03-26"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# Viewing activation logs in the IBM Cloud

Activation logs can be viewed directly from the [{{site.data.keyword.openwhisk}} Monitoring page](https://console.bluemix.net/openwhisk/dashboard/). The logs are also forwarded to [IBM Cloud Log Analysis](https://console.bluemix.net/docs/services/CloudLogAnalysis/kibana/analyzing_logs_Kibana.html#analyzing_logs_Kibana) where they are indexed, enabling full-text search through all messages generated, and convenient querying based on specific fields (like the log-level).
{:shortdesc}

## Querying logs
{: #query-logs}

When using [IBM Cloud Log Analysis](https://console.bluemix.net/docs/services/CloudLogAnalysis/kibana/analyzing_logs_Kibana.html#analyzing_logs_Kibana) hosted Kibana, querying your logs is straightforward. Use Kibana's query syntax to find logs you are looking for.

The {{site.data.keyword.openwhisk_short}} UI allows you to directly navigate to the logs and results of your Actions in Kibana. The **Logs** link is found on the interior left navigation from the [{{site.data.keyword.openwhisk}} Monitoring page](https://console.bluemix.net/openwhisk/dashboard/). When accessing the details page of a specific Action, the **Logs** link takes you to the results (activation records) of that particular Action. The default value of the time frame to show logs for is set to 15 minutes. You can change this value directly in Kibana in the upper right corner if you would like to display older records.

Here are a couple of examples of queries that are helpful to debug errors.

### Finding all error logs:
```
type: user_logs AND stream_str: stderr
```
{: codeblock}

### Finding all error logs which are generated by "myAction":
```
type: user_logs AND stream_str: stderr AND action_str: "*myAction"
```
{: codeblock}

## Querying results
{: #query-results}

In addition to loglines, [IBM Cloud Log Analysis](https://console.bluemix.net/docs/services/CloudLogAnalysis/kibana/analyzing_logs_Kibana.html#analyzing_logs_Kibana) also indexes the results (activation records) generated by {{site.data.keyword.openwhisk_short}}. The results contain rich-metadata relevant for activations, such as their duration or result-code (success, error). All of the fields are queriable, and as such can help you understand how your {{site.data.keyword.openwhisk_short}} Actions are behaving.

Use Kibana's query syntax to find activations you are looking for. Here are a couple of examples of queries that are helpful to debug errors.

### Finding all failed activations:
```
type: activation_record AND NOT status_str: 0
```
{: codeblock}

Like in Unix commands, a "`0`" indicates a successfully exited Action while everything else is considered an error.

<!--
### Finding all activations that took longer than 30 seconds:

```
type: activation_record AND duration > 30000
```

Duration is in milliseconds.
-->

### Finding all activations that failed with a specific error:
```
type: activation_record AND NOT status_str:0 AND message: "*VerySpecificErrorMessage*"
```
{: codeblock}
