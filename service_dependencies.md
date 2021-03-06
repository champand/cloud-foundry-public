---

copyright:
  years: 2015, 2020
lastupdated: "2020-10-08"

keywords: cloud foundry

subcollection: cloud-foundry-public



---


{:beta: .beta}
{:codeblock: .codeblock}
{:deprecated: .deprecated}
{:download: .download}
{:external: target="_blank" .external}
{:faq: data-hd-content-type='faq'}
{:gif: data-image-type='gif'}
{:help: data-hd-content-type='help'}
{:important: .important}
{:java: data-hd-programlang="java"}
{:javascript: data-hd-programlang="javascript"}
{:new_window: target="_blank"}
{:note: .note}
{:pre: .pre}
{:preview: .preview}
{:screen: .screen}
{:shortdesc: .shortdesc}
{:support: data-reuse='support'}
{:table: .aria-labeledby="caption"}
{:tip: .tip}
{:troubleshoot: data-hd-content-type='troubleshoot'}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:tsSymptoms: .tsSymptoms}



# Dependencies on other services
{: #dependencies}

{{site.data.keyword.ibmcf_full}} uses several services.  How {{site.data.keyword.ibmcf_notm}} connects to these services and how they are used depends on the service. 

|Service name|Description|
|---------|--------------------|
|{{site.data.keyword.at_short}}|{{site.data.keyword.ibmcf_notm}} integrates with {{site.data.keyword.at_short}} to forward workspace audit events to the {{site.data.keyword.at_short}} service instance that is set up and owned by the {{site.data.keyword.ibmcf_notm}} user. For more information, see [IBM Cloud Activity Tracker with LogDNA events](/docs/cloud-foundry-public?topic=cloud-foundry-public-at-events). Additionally, the {{site.data.keyword.ibmcf_notm}} service team uses {{site.data.keyword.at_short}} to monitor service-internal audit events.|
|Business Support Service (BSS) for {{site.data.keyword.cloud_notm}}| The BSS component is used to access information about the {{site.data.keyword.cloud_notm}} account, service subscription, service usage, and billing.|
|{{site.data.keyword.IBM_notm}} DataPower Gateway|This service is used to provide the global load balancer and firewall for {{site.data.keyword.ibmcf_notm}} in a region.|
|Identity and Access Management (IAM)| To authenticate requests to the service and authorize user actions, {{site.data.keyword.ibmcf_notm}} implements service access roles in Identity and Access Management (IAM). For more information about required IAM permissions to work with the service, see [Managing Cloud Foundry access](/docs/cloud-foundry-public?topic=account-mngcf).|
|{{site.data.keyword.la_full_notm}}|{{site.data.keyword.ibmcf_notm}} sends service logs to {{site.data.keyword.la_full_notm}}. These logs are monitored and analyzed by the {{site.data.keyword.ibmcf_notm}} service team to detect service issues and malicious activities.|
|{{site.data.keyword.cos_full_notm}}|This service is used to store data used by {{site.data.keyword.ibmcf_notm}}; for example, from an `ibmcloud cf push` command. All data is encrypted by using [Server-Side Encryption with Key Protect](/docs/cloud-object-storage?topic=cloud-object-storage-encryption) in transit and at rest.|

