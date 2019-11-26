---
title: Azure Monitor supported metrics by resource type
description: List of metrics available for each resource type with Azure Monitor.
author: anirudhcavale
services: azure-monitor
ms.service: azure-monitor
ms.topic: reference
ms.date: 05/20/2019
ms.author: ancav
ms.subservice: metrics
ms.openlocfilehash: 30e2b880f32f896098778942deb67d7ced9f5c2d
ms.sourcegitcommit: 8cf199fbb3d7f36478a54700740eb2e9edb823e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2019
ms.locfileid: "74484217"
---
# <a name="supported-metrics-with-azure-monitor"></a>Supported metrics with Azure Monitor

Azure Monitor provides several ways to interact with metrics, including charting them in the portal, accessing them through the REST API, or querying them using PowerShell or CLI. Below is a complete list of all metrics currently available with Azure Monitor's metric pipeline. Other metrics may be available in the portal or using legacy APIs. This list below only includes metrics available using the consolidated Azure Monitor metric pipeline. To query for and access these metrics please use the [2018-01-01 api-version](https://docs.microsoft.com/rest/api/monitor/metricdefinitions)

> [!NOTE]
> Çok boyutlu ölçümlerin tanılama ayarları aracılığıyla gönderilmesi şu anda desteklenmemektedir. Boyutlu ölçümler, boyut değerlerinin toplamı alınarak düzleştirilmiş tek yönlü ölçümler olarak dışarı aktarılır.
>
> *Örneğin*: Bir Olay Hub'ındaki 'Gelen İletiler' ölçümü, kuyruk düzeyi temelinde araştırılıp grafiği oluşturulabilir. Ancak, tanılama ayarları aracılığıyla dışarı aktarılan ölçüm, Olay Hub’ındaki tüm kuyruklarda tüm gelen iletiler halinde ifade edilir.
>
>

## <a name="microsoftanalysisservicesservers"></a>Microsoft.AnalysisServices/servers

|Ölçüm|Metric Display Name|Birim|Aggregation Type|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|qpu_metric|QPU|Sayı|Ortalama|QPU. Range 0-100 for S1, 0-200 for S2 and 0-400 for S4|ServerResourceType|
|memory_metric|Hafıza|Bytes|Ortalama|Memory. Range 0-25 GB for S1, 0-50 GB for S2 and 0-100 GB for S4|ServerResourceType|
|private_bytes_metric|Private Bytes |Bytes|Ortalama|The total amount of memory the Analysis Services engine process and Mashup container processes have allocated, not including memory shared with other processes.|ServerResourceType|
|virtual_bytes_metric|Virtual Bytes |Bytes|Ortalama|The current size of the virtual address space that Analysis Services engine process and Mashup container processes are using.|ServerResourceType|
|TotalConnectionRequests|Total Connection Requests|Sayı|Ortalama|Total connection requests. These are arrivals.|ServerResourceType|
|SuccessfullConnectionsPerSec|Successful Connections Per Sec|CountPerSecond|Ortalama|Rate of successful connection completions.|ServerResourceType|
|TotalConnectionFailures|Total Connection Failures|Sayı|Ortalama|Total failed connection attempts.|ServerResourceType|
|CurrentUserSessions|Current User Sessions|Sayı|Ortalama|Current number of user sessions established.|ServerResourceType|
|QueryPoolBusyThreads|Query Pool Busy Threads|Sayı|Ortalama|Number of busy threads in the query thread pool.|ServerResourceType|
|CommandPoolJobQueueLength|Command Pool Job Queue Length|Sayı|Ortalama|Number of jobs in the queue of the command thread pool.|ServerResourceType|
|ProcessingPoolJobQueueLength|Processing Pool Job Queue Length|Sayı|Ortalama|Number of non-I/O jobs in the queue of the processing thread pool.|ServerResourceType|
|CurrentConnections|Connection: Current connections|Sayı|Ortalama|Current number of client connections established.|ServerResourceType|
|CleanerCurrentPrice|Memory: Cleaner Current Price|Sayı|Ortalama|Current price of memory, $/byte/time, normalized to 1000.|ServerResourceType|
|CleanerMemoryShrinkable|Memory: Cleaner Memory shrinkable|Bytes|Ortalama|Amount of memory, in bytes, subject to purging by the background cleaner.|ServerResourceType|
|CleanerMemoryNonshrinkable|Memory: Cleaner Memory nonshrinkable|Bytes|Ortalama|Amount of memory, in bytes, not subject to purging by the background cleaner.|ServerResourceType|
|MemoryUsage|Memory: Memory Usage|Bytes|Ortalama|Memory usage of the server process as used in calculating cleaner memory price. Equal to counter Process\PrivateBytes plus the size of memory-mapped data, ignoring any memory which was mapped or allocated by the xVelocity in-memory analytics engine (VertiPaq) in excess of the xVelocity engine Memory Limit.|ServerResourceType|
|MemoryLimitHard|Memory: Memory Limit Hard|Bytes|Ortalama|Hard memory limit, from configuration file.|ServerResourceType|
|MemoryLimitHigh|Memory: Memory Limit High|Bytes|Ortalama|High memory limit, from configuration file.|ServerResourceType|
|MemoryLimitLow|Memory: Memory Limit Low|Bytes|Ortalama|Low memory limit, from configuration file.|ServerResourceType|
|MemoryLimitVertiPaq|Memory: Memory Limit VertiPaq|Bytes|Ortalama|In-memory limit, from configuration file.|ServerResourceType|
|Kota|Memory: Quota|Bytes|Ortalama|Current memory quota, in bytes. Memory quota is also known as a memory grant or memory reservation.|ServerResourceType|
|QuotaBlocked|Memory: Quota Blocked|Sayı|Ortalama|Current number of quota requests that are blocked until other memory quotas are freed.|ServerResourceType|
|VertiPaqNonpaged|Memory: VertiPaq Nonpaged|Bytes|Ortalama|Bytes of memory locked in the working set for use by the in-memory engine.|ServerResourceType|
|VertiPaqPaged|Memory: VertiPaq Paged|Bytes|Ortalama|Bytes of paged memory in use for in-memory data.|ServerResourceType|
|RowsReadPerSec|Processing: Rows read per sec|CountPerSecond|Ortalama|Rate of rows read from all relational databases.|ServerResourceType|
|RowsConvertedPerSec|Processing: Rows converted per sec|CountPerSecond|Ortalama|Rate of rows converted during processing.|ServerResourceType|
|RowsWrittenPerSec|Processing: Rows written per sec|CountPerSecond|Ortalama|Rate of rows written during processing.|ServerResourceType|
|CommandPoolBusyThreads|Threads: Command pool busy threads|Sayı|Ortalama|Number of busy threads in the command thread pool.|ServerResourceType|
|CommandPoolIdleThreads|Threads: Command pool idle threads|Sayı|Ortalama|Number of idle threads in the command thread pool.|ServerResourceType|
|LongParsingBusyThreads|Threads: Long parsing busy threads|Sayı|Ortalama|Number of busy threads in the long parsing thread pool.|ServerResourceType|
|LongParsingIdleThreads|Threads: Long parsing idle threads|Sayı|Ortalama|Number of idle threads in the long parsing thread pool.|ServerResourceType|
|LongParsingJobQueueLength|Threads: Long parsing job queue length|Sayı|Ortalama|Number of jobs in the queue of the long parsing thread pool.|ServerResourceType|
|ProcessingPoolBusyIOJobThreads|Threads: Processing pool busy I/O job threads|Sayı|Ortalama|Number of threads running I/O jobs in the processing thread pool.|ServerResourceType|
|ProcessingPoolBusyNonIOThreads|Threads: Processing pool busy non-I/O threads|Sayı|Ortalama|Number of threads running non-I/O jobs in the processing thread pool.|ServerResourceType|
|ProcessingPoolIOJobQueueLength|Threads: Processing pool I/O job queue length|Sayı|Ortalama|Number of I/O jobs in the queue of the processing thread pool.|ServerResourceType|
|ProcessingPoolIdleIOJobThreads|Threads: Processing pool idle I/O job threads|Sayı|Ortalama|Number of idle threads for I/O jobs in the processing thread pool.|ServerResourceType|
|ProcessingPoolIdleNonIOThreads|Threads: Processing pool idle non-I/O threads|Sayı|Ortalama|Number of idle threads in the processing thread pool dedicated to non-I/O jobs.|ServerResourceType|
|QueryPoolIdleThreads|Threads: Query pool idle threads|Sayı|Ortalama|Number of idle threads for I/O jobs in the processing thread pool.|ServerResourceType|
|QueryPoolJobQueueLength|Threads: Query pool job queue length|Sayı|Ortalama|Number of jobs in the queue of the query thread pool.|ServerResourceType|
|ShortParsingBusyThreads|Threads: Short parsing busy threads|Sayı|Ortalama|Number of busy threads in the short parsing thread pool.|ServerResourceType|
|ShortParsingIdleThreads|Threads: Short parsing idle threads|Sayı|Ortalama|Number of idle threads in the short parsing thread pool.|ServerResourceType|
|ShortParsingJobQueueLength|Threads: Short parsing job queue length|Sayı|Ortalama|Number of jobs in the queue of the short parsing thread pool.|ServerResourceType|
|memory_thrashing_metric|Memory Thrashing|Yüzde|Ortalama|Average memory thrashing.|ServerResourceType|
|mashup_engine_qpu_metric|M Engine QPU|Sayı|Ortalama|QPU usage by mashup engine processes|ServerResourceType|
|mashup_engine_memory_metric|M Engine Memory|Bytes|Ortalama|Memory usage by mashup engine processes|ServerResourceType|
|mashup_engine_private_bytes_metric|M Engine Private Bytes |Bytes|Ortalama|The total amount of memory Mashup container processes have allocated, not including memory shared with other processes.|ServerResourceType|
|mashup_engine_virtual_bytes_metric|M Engine Virtual Bytes |Bytes|Ortalama|The current size of the virtual address space Mashup container processes are using.|ServerResourceType|

## <a name="microsoftapimanagementservice"></a>Microsoft.ApiManagement/service

|Ölçüm|Metric Display Name|Birim|Aggregation Type|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|İstekler|İstekler|Sayı|Toplam|The total number of gateway requests in a given period. It can be sliced by various dimensions to help you diagnose issues. |Location, BackendResponseCode, LastErrorReason, GatewayResponseCode|
|TotalRequests|Total Gateway Requests|Sayı|Toplam|The total number of gateway requests in a given period. This metric has been deprecated, we recommend using the new `Requests` metric. |Location, Hostname|
|SuccessfulRequests|Successful Gateway Requests|Sayı|Toplam|The total number of successful gateway requests in a given period. This metric has been deprecated, we recommend using the new `Requests` metric.|Location, Hostname|
|UnauthorizedRequests|Unauthorized Gateway Requests|Sayı|Toplam| The total number of unauthorized gateway requests in a given period. This metric has been deprecated, we recommend using the new `Requests` metric.|Location, Hostname|
|FailedRequests|Failed Gateway Requests|Sayı|Toplam|The total number of failed gateway requests in a given period. This metric has been deprecated, we recommend using the new `Requests` metric.|Location, Hostname|
|OtherRequests|Other Gateway Requests|Sayı|Toplam|The total number of gateway requests in a given period that do not fall into the successful, unauthorized, or failed categories. This metric has been deprecated, we recommend using the new `Requests` metric. |Location, Hostname|
|Süre|Overall Duration of Gateway Requests|Milliseconds|Ortalama|The time between when API Management receives a request from a client and when it returns a response to the client.|Location, Hostname|
|Kapasite|Kapasite|Yüzde|Ortalama|Indicator of load on an API Management instance for making informed decisions whether to scale the instance to accommodate more load.|Konum|
|EventHubTotalEvents|Total EventHub Events|Sayı|Toplam|The total number of events sent to EventHub from API Management in a given period.|Konum|
|EventHubSuccessfulEvents|Successful EventHub Events|Sayı|Toplam|The total number of successful EventHub events in a given period.|Konum|
|EventHubTotalFailedEvents|Failed EventHub Events|Sayı|Toplam|The total number of failed EventHub events in a given period.|Konum|
|EventHubRejectedEvents|Rejected EventHub Events|Sayı|Toplam|The total number of rejected EventHub events (wrong configuration or unauthorized) in a given period.|Konum|
|EventHubThrottledEvents|Throttled EventHub Events|Sayı|Toplam|The total number of throttled EventHub events in a given period.|Konum|
|EventHubTimedoutEvents|Timed Out EventHub Events|Sayı|Toplam|The total number of timed out EventHub events in a given period.|Konum|
|EventHubDroppedEvents|Dropped EventHub Events|Sayı|Toplam|The total number of events skipped because of queue size limit reached in a given period.|Konum|
|EventHubTotalBytesSent|Size of EventHub Events|Bytes|Toplam|The total size of EventHub events in bytes in a given period.|Konum|


## <a name="microsoftautomationautomationaccounts"></a>Microsoft.Automation/automationAccounts

|Ölçüm|Metric Display Name|Birim|Aggregation Type|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|TotalJob|Total Jobs|Sayı|Toplam|The total number of jobs|Runbook, Status|
|TotalUpdateDeploymentRuns|Total Update Deployment Runs|Sayı|Toplam|Total software update deployment runs|SoftwareUpdateConfigurationName, Status|
|TotalUpdateDeploymentMachineRuns|Total Update Deployment Machine Runs|Sayı|Toplam|Total software update deployment machine runs in a software update deployment run|SoftwareUpdateConfigurationName, Status, TargetComputer, SoftwareUpdateConfigurationRunId|

## <a name="microsoftbatchbatchaccounts"></a>Microsoft.Batch/batchAccounts

|Ölçüm|Metric Display Name|Birim|Aggregation Type|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|CoreCount|Dedicated Core Count|Sayı|Toplam|Total number of dedicated cores in the batch account|No Dimensions|
|TotalNodeCount|Dedicated Node Count|Sayı|Toplam|Total number of dedicated nodes in the batch account|No Dimensions|
|LowPriorityCoreCount|LowPriority Core Count|Sayı|Toplam|Total number of low-priority cores in the batch account|No Dimensions|
|TotalLowPriorityNodeCount|Low-Priority Node Count|Sayı|Toplam|Total number of low-priority nodes in the batch account|No Dimensions|
|CreatingNodeCount|Creating Node Count|Sayı|Toplam|Number of nodes being created|No Dimensions|
|StartingNodeCount|Starting Node Count|Sayı|Toplam|Number of nodes starting|No Dimensions|
|WaitingForStartTaskNodeCount|Waiting For Start Task Node Count|Sayı|Toplam|Number of nodes waiting for the Start Task to complete|No Dimensions|
|StartTaskFailedNodeCount|Start Task Failed Node Count|Sayı|Toplam|Number of nodes where the Start Task has failed|No Dimensions|
|IdleNodeCount|Idle Node Count|Sayı|Toplam|Number of idle nodes|No Dimensions|
|OfflineNodeCount|Offline Node Count|Sayı|Toplam|Number of offline nodes|No Dimensions|
|RebootingNodeCount|Rebooting Node Count|Sayı|Toplam|Number of rebooting nodes|No Dimensions|
|ReimagingNodeCount|Reimaging Node Count|Sayı|Toplam|Number of reimaging nodes|No Dimensions|
|RunningNodeCount|Running Node Count|Sayı|Toplam|Number of running nodes|No Dimensions|
|LeavingPoolNodeCount|Leaving Pool Node Count|Sayı|Toplam|Number of nodes leaving the Pool|No Dimensions|
|UnusableNodeCount|Unusable Node Count|Sayı|Toplam|Number of unusable nodes|No Dimensions|
|PreemptedNodeCount|Preempted Node Count|Sayı|Toplam|Number of preempted nodes|No Dimensions|
|TaskStartEvent|Task Start Events|Sayı|Toplam|Total number of tasks that have started|No Dimensions|
|TaskCompleteEvent|Task Complete Events|Sayı|Toplam|Total number of tasks that have completed|No Dimensions|
|TaskFailEvent|Task Fail Events|Sayı|Toplam|Total number of tasks that have completed in a failed state|No Dimensions|
|PoolCreateEvent|Pool Create Events|Sayı|Toplam|Total number of pools that have been created|No Dimensions|
|PoolResizeStartEvent|Pool Resize Start Events|Sayı|Toplam|Total number of pool resizes that have started|No Dimensions|
|PoolResizeCompleteEvent|Pool Resize Complete Events|Sayı|Toplam|Total number of pool resizes that have completed|No Dimensions|
|PoolDeleteStartEvent|Pool Delete Start Events|Sayı|Toplam|Total number of pool deletes that have started|No Dimensions|
|PoolDeleteCompleteEvent|Pool Delete Complete Events|Sayı|Toplam|Total number of pool deletes that have completed|No Dimensions|
|JobDeleteCompleteEvent|Job Delete Complete Events|Sayı|Toplam|Total number of jobs that have been successfully deleted.|No Dimensions|
|JobDeleteStartEvent|Job Delete Start Events|Sayı|Toplam|Total number of jobs that have been requested to be deleted.|No Dimensions|
|JobDisableCompleteEvent|Job Disable Complete Events|Sayı|Toplam|Total number of jobs that have been successfully disabled.|No Dimensions|
|JobDisableStartEvent|Job Disable Start Events|Sayı|Toplam|Total number of jobs that have been requested to be disabled.|No Dimensions|
|JobStartEvent|Job Start Events|Sayı|Toplam|Total number of jobs that have been successfully started.|No Dimensions|
|JobTerminateCompleteEvent|Job Terminate Complete Events|Sayı|Toplam|Total number of jobs that have been successfully terminated.|No Dimensions|
|JobTerminateStartEvent|Job Terminate Start Events|Sayı|Toplam|Total number of jobs that have been requested to be terminated.|No Dimensions|

## <a name="microsoftcacheredis"></a>Microsoft.Cache/redis

|Ölçüm|Metric Display Name|Birim|Aggregation Type|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|connectedclients|Connected Clients|Sayı|Maksimum||ShardId|
|totalcommandsprocessed|Total Operations|Sayı|Toplam||ShardId|
|cachehits|Cache Hits|Sayı|Toplam||ShardId|
|cachemisses|Cache Misses|Sayı|Toplam||ShardId|
|getcommands|Gets|Sayı|Toplam||ShardId|
|setcommands|Sets|Sayı|Toplam||ShardId|
|operationsPerSecond|Operations Per Second|Sayı|Maksimum||ShardId|
|evictedkeys|Evicted Keys|Sayı|Toplam||ShardId|
|totalkeys|Total Keys|Sayı|Maksimum||ShardId|
|expiredkeys|Expired Keys|Sayı|Toplam||ShardId|
|usedmemory|Used Memory|Bytes|Maksimum||ShardId|
|usedmemorypercentage|Used Memory Percentage|Yüzde|Maksimum||ShardId|
|usedmemoryRss|Used Memory RSS|Bytes|Maksimum||ShardId|
|serverLoad|Server Load|Yüzde|Maksimum||ShardId|
|cacheWrite|Cache Write|BytesPerSecond|Maksimum||ShardId|
|cacheRead|Cache Read|BytesPerSecond|Maksimum||ShardId|
|percentProcessorTime|CPU|Yüzde|Maksimum||ShardId|
|cacheLatency|Cache Latency Microseconds (Preview)|Sayı|Ortalama||ShardId, SampleType|
|hatalar|Hatalar|Sayı|Maksimum||ShardId, ErrorType|
|connectedclients0|Connected Clients (Shard 0)|Sayı|Maksimum||No Dimensions|
|totalcommandsprocessed0|Total Operations (Shard 0)|Sayı|Toplam||No Dimensions|
|cachehits0|Cache Hits (Shard 0)|Sayı|Toplam||No Dimensions|
|cachemisses0|Cache Misses (Shard 0)|Sayı|Toplam||No Dimensions|
|getcommands0|Gets (Shard 0)|Sayı|Toplam||No Dimensions|
|setcommands0|Sets (Shard 0)|Sayı|Toplam||No Dimensions|
|operationsPerSecond0|Operations Per Second (Shard 0)|Sayı|Maksimum||No Dimensions|
|evictedkeys0|Evicted Keys (Shard 0)|Sayı|Toplam||No Dimensions|
|totalkeys0|Total Keys (Shard 0)|Sayı|Maksimum||No Dimensions|
|expiredkeys0|Expired Keys (Shard 0)|Sayı|Toplam||No Dimensions|
|usedmemory0|Used Memory (Shard 0)|Bytes|Maksimum||No Dimensions|
|usedmemoryRss0|Used Memory RSS (Shard 0)|Bytes|Maksimum||No Dimensions|
|serverLoad0|Server Load (Shard 0)|Yüzde|Maksimum||No Dimensions|
|cacheWrite0|Cache Write (Shard 0)|BytesPerSecond|Maksimum||No Dimensions|
|cacheRead0|Cache Read (Shard 0)|BytesPerSecond|Maksimum||No Dimensions|
|percentProcessorTime0|CPU (Shard 0)|Yüzde|Maksimum||No Dimensions|
|connectedclients1|Connected Clients (Shard 1)|Sayı|Maksimum||No Dimensions|
|totalcommandsprocessed1|Total Operations (Shard 1)|Sayı|Toplam||No Dimensions|
|cachehits1|Cache Hits (Shard 1)|Sayı|Toplam||No Dimensions|
|cachemisses1|Cache Misses (Shard 1)|Sayı|Toplam||No Dimensions|
|getcommands1|Gets (Shard 1)|Sayı|Toplam||No Dimensions|
|setcommands1|Sets (Shard 1)|Sayı|Toplam||No Dimensions|
|operationsPerSecond1|Operations Per Second (Shard 1)|Sayı|Maksimum||No Dimensions|
|evictedkeys1|Evicted Keys (Shard 1)|Sayı|Toplam||No Dimensions|
|totalkeys1|Total Keys (Shard 1)|Sayı|Maksimum||No Dimensions|
|expiredkeys1|Expired Keys (Shard 1)|Sayı|Toplam||No Dimensions|
|usedmemory1|Used Memory (Shard 1)|Bytes|Maksimum||No Dimensions|
|usedmemoryRss1|Used Memory RSS (Shard 1)|Bytes|Maksimum||No Dimensions|
|serverLoad1|Server Load (Shard 1)|Yüzde|Maksimum||No Dimensions|
|cacheWrite1|Cache Write (Shard 1)|BytesPerSecond|Maksimum||No Dimensions|
|cacheRead1|Cache Read (Shard 1)|BytesPerSecond|Maksimum||No Dimensions|
|percentProcessorTime1|CPU (Shard 1)|Yüzde|Maksimum||No Dimensions|
|connectedclients2|Connected Clients (Shard 2)|Sayı|Maksimum||No Dimensions|
|totalcommandsprocessed2|Total Operations (Shard 2)|Sayı|Toplam||No Dimensions|
|cachehits2|Cache Hits (Shard 2)|Sayı|Toplam||No Dimensions|
|cachemisses2|Cache Misses (Shard 2)|Sayı|Toplam||No Dimensions|
|getcommands2|Gets (Shard 2)|Sayı|Toplam||No Dimensions|
|setcommands2|Sets (Shard 2)|Sayı|Toplam||No Dimensions|
|operationsPerSecond2|Operations Per Second (Shard 2)|Sayı|Maksimum||No Dimensions|
|evictedkeys2|Evicted Keys (Shard 2)|Sayı|Toplam||No Dimensions|
|totalkeys2|Total Keys (Shard 2)|Sayı|Maksimum||No Dimensions|
|expiredkeys2|Expired Keys (Shard 2)|Sayı|Toplam||No Dimensions|
|usedmemory2|Used Memory (Shard 2)|Bytes|Maksimum||No Dimensions|
|usedmemoryRss2|Used Memory RSS (Shard 2)|Bytes|Maksimum||No Dimensions|
|serverLoad2|Server Load (Shard 2)|Yüzde|Maksimum||No Dimensions|
|cacheWrite2|Cache Write (Shard 2)|BytesPerSecond|Maksimum||No Dimensions|
|cacheRead2|Cache Read (Shard 2)|BytesPerSecond|Maksimum||No Dimensions|
|percentProcessorTime2|CPU (Shard 2)|Yüzde|Maksimum||No Dimensions|
|connectedclients3|Connected Clients (Shard 3)|Sayı|Maksimum||No Dimensions|
|totalcommandsprocessed3|Total Operations (Shard 3)|Sayı|Toplam||No Dimensions|
|cachehits3|Cache Hits (Shard 3)|Sayı|Toplam||No Dimensions|
|cachemisses3|Cache Misses (Shard 3)|Sayı|Toplam||No Dimensions|
|getcommands3|Gets (Shard 3)|Sayı|Toplam||No Dimensions|
|setcommands3|Sets (Shard 3)|Sayı|Toplam||No Dimensions|
|operationsPerSecond3|Operations Per Second (Shard 3)|Sayı|Maksimum||No Dimensions|
|evictedkeys3|Evicted Keys (Shard 3)|Sayı|Toplam||No Dimensions|
|totalkeys3|Total Keys (Shard 3)|Sayı|Maksimum||No Dimensions|
|expiredkeys3|Expired Keys (Shard 3)|Sayı|Toplam||No Dimensions|
|usedmemory3|Used Memory (Shard 3)|Bytes|Maksimum||No Dimensions|
|usedmemoryRss3|Used Memory RSS (Shard 3)|Bytes|Maksimum||No Dimensions|
|serverLoad3|Server Load (Shard 3)|Yüzde|Maksimum||No Dimensions|
|cacheWrite3|Cache Write (Shard 3)|BytesPerSecond|Maksimum||No Dimensions|
|cacheRead3|Cache Read (Shard 3)|BytesPerSecond|Maksimum||No Dimensions|
|percentProcessorTime3|CPU (Shard 3)|Yüzde|Maksimum||No Dimensions|
|connectedclients4|Connected Clients (Shard 4)|Sayı|Maksimum||No Dimensions|
|totalcommandsprocessed4|Total Operations (Shard 4)|Sayı|Toplam||No Dimensions|
|cachehits4|Cache Hits (Shard 4)|Sayı|Toplam||No Dimensions|
|cachemisses4|Cache Misses (Shard 4)|Sayı|Toplam||No Dimensions|
|getcommands4|Gets (Shard 4)|Sayı|Toplam||No Dimensions|
|setcommands4|Sets (Shard 4)|Sayı|Toplam||No Dimensions|
|operationsPerSecond4|Operations Per Second (Shard 4)|Sayı|Maksimum||No Dimensions|
|evictedkeys4|Evicted Keys (Shard 4)|Sayı|Toplam||No Dimensions|
|totalkeys4|Total Keys (Shard 4)|Sayı|Maksimum||No Dimensions|
|expiredkeys4|Expired Keys (Shard 4)|Sayı|Toplam||No Dimensions|
|usedmemory4|Used Memory (Shard 4)|Bytes|Maksimum||No Dimensions|
|usedmemoryRss4|Used Memory RSS (Shard 4)|Bytes|Maksimum||No Dimensions|
|serverLoad4|Server Load (Shard 4)|Yüzde|Maksimum||No Dimensions|
|cacheWrite4|Cache Write (Shard 4)|BytesPerSecond|Maksimum||No Dimensions|
|cacheRead4|Cache Read (Shard 4)|BytesPerSecond|Maksimum||No Dimensions|
|percentProcessorTime4|CPU (Shard 4)|Yüzde|Maksimum||No Dimensions|
|connectedclients5|Connected Clients (Shard 5)|Sayı|Maksimum||No Dimensions|
|totalcommandsprocessed5|Total Operations (Shard 5)|Sayı|Toplam||No Dimensions|
|cachehits5|Cache Hits (Shard 5)|Sayı|Toplam||No Dimensions|
|cachemisses5|Cache Misses (Shard 5)|Sayı|Toplam||No Dimensions|
|getcommands5|Gets (Shard 5)|Sayı|Toplam||No Dimensions|
|setcommands5|Sets (Shard 5)|Sayı|Toplam||No Dimensions|
|operationsPerSecond5|Operations Per Second (Shard 5)|Sayı|Maksimum||No Dimensions|
|evictedkeys5|Evicted Keys (Shard 5)|Sayı|Toplam||No Dimensions|
|totalkeys5|Total Keys (Shard 5)|Sayı|Maksimum||No Dimensions|
|expiredkeys5|Expired Keys (Shard 5)|Sayı|Toplam||No Dimensions|
|usedmemory5|Used Memory (Shard 5)|Bytes|Maksimum||No Dimensions|
|usedmemoryRss5|Used Memory RSS (Shard 5)|Bytes|Maksimum||No Dimensions|
|serverLoad5|Server Load (Shard 5)|Yüzde|Maksimum||No Dimensions|
|cacheWrite5|Cache Write (Shard 5)|BytesPerSecond|Maksimum||No Dimensions|
|cacheRead5|Cache Read (Shard 5)|BytesPerSecond|Maksimum||No Dimensions|
|percentProcessorTime5|CPU (Shard 5)|Yüzde|Maksimum||No Dimensions|
|connectedclients6|Connected Clients (Shard 6)|Sayı|Maksimum||No Dimensions|
|totalcommandsprocessed6|Total Operations (Shard 6)|Sayı|Toplam||No Dimensions|
|cachehits6|Cache Hits (Shard 6)|Sayı|Toplam||No Dimensions|
|cachemisses6|Cache Misses (Shard 6)|Sayı|Toplam||No Dimensions|
|getcommands6|Gets (Shard 6)|Sayı|Toplam||No Dimensions|
|setcommands6|Sets (Shard 6)|Sayı|Toplam||No Dimensions|
|operationsPerSecond6|Operations Per Second (Shard 6)|Sayı|Maksimum||No Dimensions|
|evictedkeys6|Evicted Keys (Shard 6)|Sayı|Toplam||No Dimensions|
|totalkeys6|Total Keys (Shard 6)|Sayı|Maksimum||No Dimensions|
|expiredkeys6|Expired Keys (Shard 6)|Sayı|Toplam||No Dimensions|
|usedmemory6|Used Memory (Shard 6)|Bytes|Maksimum||No Dimensions|
|usedmemoryRss6|Used Memory RSS (Shard 6)|Bytes|Maksimum||No Dimensions|
|serverLoad6|Server Load (Shard 6)|Yüzde|Maksimum||No Dimensions|
|cacheWrite6|Cache Write (Shard 6)|BytesPerSecond|Maksimum||No Dimensions|
|cacheRead6|Cache Read (Shard 6)|BytesPerSecond|Maksimum||No Dimensions|
|percentProcessorTime6|CPU (Shard 6)|Yüzde|Maksimum||No Dimensions|
|connectedclients7|Connected Clients (Shard 7)|Sayı|Maksimum||No Dimensions|
|totalcommandsprocessed7|Total Operations (Shard 7)|Sayı|Toplam||No Dimensions|
|cachehits7|Cache Hits (Shard 7)|Sayı|Toplam||No Dimensions|
|cachemisses7|Cache Misses (Shard 7)|Sayı|Toplam||No Dimensions|
|getcommands7|Gets (Shard 7)|Sayı|Toplam||No Dimensions|
|setcommands7|Sets (Shard 7)|Sayı|Toplam||No Dimensions|
|operationsPerSecond7|Operations Per Second (Shard 7)|Sayı|Maksimum||No Dimensions|
|evictedkeys7|Evicted Keys (Shard 7)|Sayı|Toplam||No Dimensions|
|totalkeys7|Total Keys (Shard 7)|Sayı|Maksimum||No Dimensions|
|expiredkeys7|Expired Keys (Shard 7)|Sayı|Toplam||No Dimensions|
|usedmemory7|Used Memory (Shard 7)|Bytes|Maksimum||No Dimensions|
|usedmemoryRss7|Used Memory RSS (Shard 7)|Bytes|Maksimum||No Dimensions|
|serverLoad7|Server Load (Shard 7)|Yüzde|Maksimum||No Dimensions|
|cacheWrite7|Cache Write (Shard 7)|BytesPerSecond|Maksimum||No Dimensions|
|cacheRead7|Cache Read (Shard 7)|BytesPerSecond|Maksimum||No Dimensions|
|percentProcessorTime7|CPU (Shard 7)|Yüzde|Maksimum||No Dimensions|
|connectedclients8|Connected Clients (Shard 8)|Sayı|Maksimum||No Dimensions|
|totalcommandsprocessed8|Total Operations (Shard 8)|Sayı|Toplam||No Dimensions|
|cachehits8|Cache Hits (Shard 8)|Sayı|Toplam||No Dimensions|
|cachemisses8|Cache Misses (Shard 8)|Sayı|Toplam||No Dimensions|
|getcommands8|Gets (Shard 8)|Sayı|Toplam||No Dimensions|
|setcommands8|Sets (Shard 8)|Sayı|Toplam||No Dimensions|
|operationsPerSecond8|Operations Per Second (Shard 8)|Sayı|Maksimum||No Dimensions|
|evictedkeys8|Evicted Keys (Shard 8)|Sayı|Toplam||No Dimensions|
|totalkeys8|Total Keys (Shard 8)|Sayı|Maksimum||No Dimensions|
|expiredkeys8|Expired Keys (Shard 8)|Sayı|Toplam||No Dimensions|
|usedmemory8|Used Memory (Shard 8)|Bytes|Maksimum||No Dimensions|
|usedmemoryRss8|Used Memory RSS (Shard 8)|Bytes|Maksimum||No Dimensions|
|serverLoad8|Server Load (Shard 8)|Yüzde|Maksimum||No Dimensions|
|cacheWrite8|Cache Write (Shard 8)|BytesPerSecond|Maksimum||No Dimensions|
|cacheRead8|Cache Read (Shard 8)|BytesPerSecond|Maksimum||No Dimensions|
|percentProcessorTime8|CPU (Shard 8)|Yüzde|Maksimum||No Dimensions|
|connectedclients9|Connected Clients (Shard 9)|Sayı|Maksimum||No Dimensions|
|totalcommandsprocessed9|Total Operations (Shard 9)|Sayı|Toplam||No Dimensions|
|cachehits9|Cache Hits (Shard 9)|Sayı|Toplam||No Dimensions|
|cachemisses9|Cache Misses (Shard 9)|Sayı|Toplam||No Dimensions|
|getcommands9|Gets (Shard 9)|Sayı|Toplam||No Dimensions|
|setcommands9|Sets (Shard 9)|Sayı|Toplam||No Dimensions|
|operationsPerSecond9|Operations Per Second (Shard 9)|Sayı|Maksimum||No Dimensions|
|evictedkeys9|Evicted Keys (Shard 9)|Sayı|Toplam||No Dimensions|
|totalkeys9|Total Keys (Shard 9)|Sayı|Maksimum||No Dimensions|
|expiredkeys9|Expired Keys (Shard 9)|Sayı|Toplam||No Dimensions|
|usedmemory9|Used Memory (Shard 9)|Bytes|Maksimum||No Dimensions|
|usedmemoryRss9|Used Memory RSS (Shard 9)|Bytes|Maksimum||No Dimensions|
|serverLoad9|Server Load (Shard 9)|Yüzde|Maksimum||No Dimensions|
|cacheWrite9|Cache Write (Shard 9)|BytesPerSecond|Maksimum||No Dimensions|
|cacheRead9|Cache Read (Shard 9)|BytesPerSecond|Maksimum||No Dimensions|
|percentProcessorTime9|CPU (Shard 9)|Yüzde|Maksimum||No Dimensions|

## <a name="microsoftclassiccomputevirtualmachines"></a>Microsoft.ClassicCompute/virtualMachines

|Ölçüm|Metric Display Name|Birim|Aggregation Type|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|CPU yüzdesi|CPU yüzdesi|Yüzde|Ortalama|The percentage of allocated compute units that are currently in use by the Virtual Machine(s).|No Dimensions|
|Ağ Girişi|Ağ Girişi|Bytes|Toplam|The number of bytes received on all network interfaces by the Virtual Machine(s) (Incoming Traffic).|No Dimensions|
|Ağ Çıkışı|Ağ Çıkışı|Bytes|Toplam|The number of bytes out on all network interfaces by the Virtual Machine(s) (Outgoing Traffic).|No Dimensions|
|Disk Read Bytes/Sec|Disk Read|BytesPerSecond|Ortalama|Average bytes read from disk during monitoring period.|No Dimensions|
|Disk Write Bytes/Sec|Disk Write|BytesPerSecond|Ortalama|Average bytes written to disk during monitoring period.|No Dimensions|
|Disk Read Operations/Sec|Disk Read Operations/Sec|CountPerSecond|Ortalama|Disk Read IOPS.|No Dimensions|
|Disk Write Operations/Sec|Disk Write Operations/Sec|CountPerSecond|Ortalama|Disk Write IOPS.|No Dimensions|

## <a name="microsoftclassiccomputedomainnamesslotsroles"></a>Microsoft.ClassicCompute/domainNames/slots/roles

|Ölçüm|Metric Display Name|Birim|Aggregation Type|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|CPU yüzdesi|CPU yüzdesi|Yüzde|Ortalama|The percentage of allocated compute units that are currently in use by the Virtual Machine(s).|RoleInstanceId|
|Ağ Girişi|Ağ Girişi|Bytes|Toplam|The number of bytes received on all network interfaces by the Virtual Machine(s) (Incoming Traffic).|RoleInstanceId|
|Ağ Çıkışı|Ağ Çıkışı|Bytes|Toplam|The number of bytes out on all network interfaces by the Virtual Machine(s) (Outgoing Traffic).|RoleInstanceId|
|Disk Read Bytes/Sec|Disk Read|BytesPerSecond|Ortalama|Average bytes read from disk during monitoring period.|RoleInstanceId|
|Disk Write Bytes/Sec|Disk Write|BytesPerSecond|Ortalama|Average bytes written to disk during monitoring period.|RoleInstanceId|
|Disk Read Operations/Sec|Disk Read Operations/Sec|CountPerSecond|Ortalama|Disk Read IOPS.|RoleInstanceId|
|Disk Write Operations/Sec|Disk Write Operations/Sec|CountPerSecond|Ortalama|Disk Write IOPS.|RoleInstanceId|

## <a name="microsoftcognitiveservicesaccounts"></a>Microsoft.CognitiveServices/accounts

|Ölçüm|Metric Display Name|Birim|Aggregation Type|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|TotalCalls|Total Calls|Sayı|Toplam|Total number of calls.|ApiName, OperationName, Region|
|SuccessfulCalls|Successful Calls|Sayı|Toplam|Number of successful calls.|ApiName, OperationName, Region|
|TotalErrors|Total Errors|Sayı|Toplam|Total number of calls with error response (HTTP response code 4xx or 5xx).|ApiName, OperationName, Region|
|BlockedCalls|Blocked Calls|Sayı|Toplam|Number of calls that exceeded rate or quota limit.|ApiName, OperationName, Region|
|ServerErrors|Server Errors|Sayı|Toplam|Number of calls with service internal error (HTTP response code 5xx).|ApiName, OperationName, Region|
|ClientErrors|Client Errors|Sayı|Toplam|Number of calls with client side error (HTTP response code 4xx).|ApiName, OperationName, Region|
|DataIn|Data In|Bytes|Toplam|Size of incoming data in bytes.|ApiName, OperationName, Region|
|DataOut|Data Out|Bytes|Toplam|Size of outgoing data in bytes.|ApiName, OperationName, Region|
|Gecikme süresi|Gecikme süresi|MilliSeconds|Ortalama|Latency in milliseconds.|ApiName, OperationName, Region|
|CharactersTranslated|Characters Translated|Sayı|Toplam|Total number of characters in incoming text request.|ApiName, OperationName, Region|
|CharactersTrained|Characters Trained|Sayı|Toplam|Total number of characters trained.|ApiName, OperationName, Region|
|SpeechSessionDuration|Speech Session Duration|Saniye|Toplam|Total duration of speech session in seconds.|ApiName, OperationName, Region|
|TotalTransactions|Total Transactions|Sayı|Toplam|Total number of transactions.|No Dimensions|
|TotalTokenCalls|Total Token Calls|Sayı|Toplam|Total number of token calls.|ApiName, OperationName, Region|

## <a name="microsoftcomputevirtualmachines"></a>Microsoft.Compute/virtualMachines

|Ölçüm|Metric Display Name|Birim|Aggregation Type|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|CPU yüzdesi|CPU yüzdesi|Yüzde|Ortalama|The percentage of allocated compute units that are currently in use by the Virtual Machine(s)|No Dimensions|
|Ağ Girişi|Network In Billable|Bytes|Toplam|The number of billable bytes received on all network interfaces by the Virtual Machine(s) (Incoming Traffic)|No Dimensions|
|Ağ Çıkışı|Network Out Billable|Bytes|Toplam|The number of billable bytes out on all network interfaces by the Virtual Machine(s) (Outgoing Traffic)|No Dimensions|
|Disk Read Bytes|Disk Read Bytes|Bytes|Toplam|Bytes read from disk during monitoring period|No Dimensions|
|Disk Write Bytes|Disk Write Bytes|Bytes|Toplam|Bytes written to disk during monitoring period|No Dimensions|
|Disk Read Operations/Sec|Disk Read Operations/Sec|CountPerSecond|Ortalama|Disk Read IOPS|No Dimensions|
|Disk Write Operations/Sec|Disk Write Operations/Sec|CountPerSecond|Ortalama|Disk Write IOPS|No Dimensions|
|CPU Credits Remaining|CPU Credits Remaining|Sayı|Ortalama|Total number of credits available to burst|No Dimensions|
|CPU Credits Consumed|CPU Credits Consumed|Sayı|Ortalama|Total number of credits consumed by the Virtual Machine|No Dimensions|
|Per Disk Read Bytes/sec|Data Disk Read Bytes/Sec (Deprecated)|CountPerSecond|Ortalama|Bytes/Sec read from a single disk during monitoring period|SlotId|
|Per Disk Write Bytes/sec|Data Disk Write Bytes/Sec (Deprecated)|CountPerSecond|Ortalama|Bytes/Sec written to a single disk during monitoring period|SlotId|
|Per Disk Read Operations/Sec|Data Disk Read Operations/Sec (Deprecated)|CountPerSecond|Ortalama|Read IOPS from a single disk during monitoring period|SlotId|
|Per Disk Write Operations/Sec|Data Disk Write Operations/Sec (Deprecated)|CountPerSecond|Ortalama|Write IOPS from a single disk during monitoring period|SlotId|
|Per Disk QD|Data Disk QD (Deprecated)|Sayı|Ortalama|Data Disk Queue Depth(or Queue Length)|SlotId|
|OS Per Disk Read Bytes/sec|OS Disk Read Bytes/Sec (Deprecated)|CountPerSecond|Ortalama|Bytes/Sec read from a single disk during monitoring period for OS disk|No Dimensions|
|OS Per Disk Write Bytes/sec|OS Disk Write Bytes/Sec (Deprecated)|CountPerSecond|Ortalama|Bytes/Sec written to a single disk during monitoring period for OS disk|No Dimensions|
|OS Per Disk Read Operations/Sec|OS Disk Read Operations/Sec (Deprecated)|CountPerSecond|Ortalama|Read IOPS from a single disk during monitoring period for OS disk|No Dimensions|
|OS Per Disk Write Operations/Sec|OS Disk Write Operations/Sec (Deprecated)|CountPerSecond|Ortalama|Write IOPS from a single disk during monitoring period for OS disk|No Dimensions|
|OS Per Disk QD|OS Disk QD (Deprecated)|Sayı|Ortalama|OS Disk Queue Depth(or Queue Length)|No Dimensions|
|Data Disk Read Bytes/sec|Data Disk Read Bytes/Sec (Preview)|CountPerSecond|Ortalama|Bytes/Sec read from a single disk during monitoring period|LUN|
|Data Disk Write Bytes/sec|Data Disk Write Bytes/Sec (Preview)|CountPerSecond|Ortalama|Bytes/Sec written to a single disk during monitoring period|LUN|
|Data Disk Read Operations/Sec|Data Disk Read Operations/Sec (Preview)|CountPerSecond|Ortalama|Read IOPS from a single disk during monitoring period|LUN|
|Data Disk Write Operations/Sec|Data Disk Write Operations/Sec (Preview)|CountPerSecond|Ortalama|Write IOPS from a single disk during monitoring period|LUN|
|Data Disk Queue Depth|Data Disk Queue Depth (Preview)|Sayı|Ortalama|Data Disk Queue Depth(or Queue Length)|LUN|
|OS Disk Read Bytes/sec|OS Disk Read Bytes/Sec (Preview)|CountPerSecond|Ortalama|Bytes/Sec read from a single disk during monitoring period for OS disk|No Dimensions|
|OS Disk Write Bytes/sec|OS Disk Write Bytes/Sec (Preview)|CountPerSecond|Ortalama|Bytes/Sec written to a single disk during monitoring period for OS disk|No Dimensions|
|OS Disk Read Operations/Sec|OS Disk Read Operations/Sec (Preview)|CountPerSecond|Ortalama|Read IOPS from a single disk during monitoring period for OS disk|No Dimensions|
|OS Disk Write Operations/Sec|OS Disk Write Operations/Sec (Preview)|CountPerSecond|Ortalama|Write IOPS from a single disk during monitoring period for OS disk|No Dimensions|
|OS Disk Queue Depth|OS Disk Queue Depth (Preview)|Sayı|Ortalama|OS Disk Queue Depth(or Queue Length)|No Dimensions|
|Inbound Flows|Inbound Flows (Preview)|Sayı|Ortalama|Inbound Flows are number of current flows in the inbound direction (traffic going into the VM)|No Dimensions|
|Outbound Flows|Outbound Flows (Preview)|Sayı|Ortalama|Outbound Flows are number of current flows in the outbound direction (traffic going out of the VM)|No Dimensions|
|Inbound Flows Maximum Creation Rate|Inbound Flows Maximum Creation Rate (Preview)|CountPerSecond|Ortalama|The maximum creation rate of inbound flows (traffic going into the VM)|No Dimensions|
|Outbound Flows Maximum Creation Rate|Outbound Flows Maximum Creation Rate (Preview)|CountPerSecond|Ortalama|The maximum creation rate of outbound flows (traffic going out of the VM)|No Dimensions|
|Premium Data Disk Cache Read Hit|Premium Data Disk Cache Read Hit (Preview)|Yüzde|Ortalama|Premium Data Disk Cache Read Hit|LUN|
|Premium Data Disk Cache Read Miss|Premium Data Disk Cache Read Miss (Preview)|Yüzde|Ortalama|Premium Data Disk Cache Read Miss|LUN|
|Premium OS Disk Cache Read Hit|Premium OS Disk Cache Read Hit (Preview)|Yüzde|Ortalama|Premium OS Disk Cache Read Hit|No Dimensions|
|Premium OS Disk Cache Read Miss|Premium OS Disk Cache Read Miss (Preview)|Yüzde|Ortalama|Premium OS Disk Cache Read Miss|No Dimensions|
|Network In Total|Network In Total|Bytes|Toplam|The number of bytes received on all network interfaces by the Virtual Machine(s) (Incoming Traffic)|No Dimensions|
|Network Out Total|Network Out Total|Bytes|Toplam|The number of bytes out on all network interfaces by the Virtual Machine(s) (Outgoing Traffic)|No Dimensions|

## <a name="microsoftcomputevirtualmachinescalesets"></a>Microsoft.Compute/virtualMachineScaleSets

|Ölçüm|Metric Display Name|Birim|Aggregation Type|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|CPU yüzdesi|CPU yüzdesi|Yüzde|Ortalama|The percentage of allocated compute units that are currently in use by the Virtual Machine(s)|VMName|
|Ağ Girişi|Network In Billable|Bytes|Toplam|The number of billable bytes received on all network interfaces by the Virtual Machine(s) (Incoming Traffic)|VMName|
|Ağ Çıkışı|Network Out Billable|Bytes|Toplam|The number of billable bytes out on all network interfaces by the Virtual Machine(s) (Outgoing Traffic)|VMName|
|Disk Read Bytes|Disk Read Bytes|Bytes|Toplam|Bytes read from disk during monitoring period|VMName|
|Disk Write Bytes|Disk Write Bytes|Bytes|Toplam|Bytes written to disk during monitoring period|VMName|
|Disk Read Operations/Sec|Disk Read Operations/Sec|CountPerSecond|Ortalama|Disk Read IOPS|VMName|
|Disk Write Operations/Sec|Disk Write Operations/Sec|CountPerSecond|Ortalama|Disk Write IOPS|VMName|
|CPU Credits Remaining|CPU Credits Remaining|Sayı|Ortalama|Total number of credits available to burst|No Dimensions|
|CPU Credits Consumed|CPU Credits Consumed|Sayı|Ortalama|Total number of credits consumed by the Virtual Machine|No Dimensions|
|Per Disk Read Bytes/sec|Data Disk Read Bytes/Sec (Deprecated)|CountPerSecond|Ortalama|Bytes/Sec read from a single disk during monitoring period|SlotId|
|Per Disk Write Bytes/sec|Data Disk Write Bytes/Sec (Deprecated)|CountPerSecond|Ortalama|Bytes/Sec written to a single disk during monitoring period|SlotId|
|Per Disk Read Operations/Sec|Data Disk Read Operations/Sec (Deprecated)|CountPerSecond|Ortalama|Read IOPS from a single disk during monitoring period|SlotId|
|Per Disk Write Operations/Sec|Data Disk Write Operations/Sec (Deprecated)|CountPerSecond|Ortalama|Write IOPS from a single disk during monitoring period|SlotId|
|Per Disk QD|Data Disk QD (Deprecated)|Sayı|Ortalama|Data Disk Queue Depth(or Queue Length)|SlotId|
|OS Per Disk Read Bytes/sec|OS Disk Read Bytes/Sec (Deprecated)|CountPerSecond|Ortalama|Bytes/Sec read from a single disk during monitoring period for OS disk|No Dimensions|
|OS Per Disk Write Bytes/sec|OS Disk Write Bytes/Sec (Deprecated)|CountPerSecond|Ortalama|Bytes/Sec written to a single disk during monitoring period for OS disk|No Dimensions|
|OS Per Disk Read Operations/Sec|OS Disk Read Operations/Sec (Deprecated)|CountPerSecond|Ortalama|Read IOPS from a single disk during monitoring period for OS disk|No Dimensions|
|OS Per Disk Write Operations/Sec|OS Disk Write Operations/Sec (Deprecated)|CountPerSecond|Ortalama|Write IOPS from a single disk during monitoring period for OS disk|No Dimensions|
|OS Per Disk QD|OS Disk QD (Deprecated)|Sayı|Ortalama|OS Disk Queue Depth(or Queue Length)|No Dimensions|
|Data Disk Read Bytes/sec|Data Disk Read Bytes/Sec (Preview)|CountPerSecond|Ortalama|Bytes/Sec read from a single disk during monitoring period|LUN, VMName|
|Data Disk Write Bytes/sec|Data Disk Write Bytes/Sec (Preview)|CountPerSecond|Ortalama|Bytes/Sec written to a single disk during monitoring period|LUN, VMName|
|Data Disk Read Operations/Sec|Data Disk Read Operations/Sec (Preview)|CountPerSecond|Ortalama|Read IOPS from a single disk during monitoring period|LUN, VMName|
|Data Disk Write Operations/Sec|Data Disk Write Operations/Sec (Preview)|CountPerSecond|Ortalama|Write IOPS from a single disk during monitoring period|LUN, VMName|
|Data Disk Queue Depth|Data Disk Queue Depth (Preview)|Sayı|Ortalama|Data Disk Queue Depth(or Queue Length)|LUN, VMName|
|OS Disk Read Bytes/sec|OS Disk Read Bytes/Sec (Preview)|CountPerSecond|Ortalama|Bytes/Sec read from a single disk during monitoring period for OS disk|VMName|
|OS Disk Write Bytes/sec|OS Disk Write Bytes/Sec (Preview)|CountPerSecond|Ortalama|Bytes/Sec written to a single disk during monitoring period for OS disk|VMName|
|OS Disk Read Operations/Sec|OS Disk Read Operations/Sec (Preview)|CountPerSecond|Ortalama|Read IOPS from a single disk during monitoring period for OS disk|VMName|
|OS Disk Write Operations/Sec|OS Disk Write Operations/Sec (Preview)|CountPerSecond|Ortalama|Write IOPS from a single disk during monitoring period for OS disk|VMName|
|OS Disk Queue Depth|OS Disk Queue Depth (Preview)|Sayı|Ortalama|OS Disk Queue Depth(or Queue Length)|VMName|
|Inbound Flows|Inbound Flows (Preview)|Sayı|Ortalama|Inbound Flows are number of current flows in the inbound direction (traffic going into the VM)|VMName|
|Outbound Flows|Outbound Flows (Preview)|Sayı|Ortalama|Outbound Flows are number of current flows in the outbound direction (traffic going out of the VM)|VMName|
|Inbound Flows Maximum Creation Rate|Inbound Flows Maximum Creation Rate (Preview)|CountPerSecond|Ortalama|The maximum creation rate of inbound flows (traffic going into the VM)|VMName|
|Outbound Flows Maximum Creation Rate|Outbound Flows Maximum Creation Rate (Preview)|CountPerSecond|Ortalama|The maximum creation rate of outbound flows (traffic going out of the VM)|VMName|
|Premium Data Disk Cache Read Hit|Premium Data Disk Cache Read Hit (Preview)|Yüzde|Ortalama|Premium Data Disk Cache Read Hit|LUN, VMName|
|Premium Data Disk Cache Read Miss|Premium Data Disk Cache Read Miss (Preview)|Yüzde|Ortalama|Premium Data Disk Cache Read Miss|LUN, VMName|
|Premium OS Disk Cache Read Hit|Premium OS Disk Cache Read Hit (Preview)|Yüzde|Ortalama|Premium OS Disk Cache Read Hit|VMName|
|Premium OS Disk Cache Read Miss|Premium OS Disk Cache Read Miss (Preview)|Yüzde|Ortalama|Premium OS Disk Cache Read Miss|VMName|
|Network In Total|Network In Total|Bytes|Toplam|The number of bytes received on all network interfaces by the Virtual Machine(s) (Incoming Traffic)|VMName|
|Network Out Total|Network Out Total|Bytes|Toplam|The number of bytes out on all network interfaces by the Virtual Machine(s) (Outgoing Traffic)|VMName|

## <a name="microsoftcontainerinstancecontainergroups"></a>Microsoft.ContainerInstance/containerGroups

|Ölçüm|Metric Display Name|Birim|Aggregation Type|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|CpuUsage|CPU Usage|Sayı|Ortalama|CPU usage on all cores in millicores.|containerName|
|MemoryUsage|Memory Usage|Bytes|Ortalama|Total memory usage in byte.|containerName|
|NetworkBytesReceivedPerSecond|Network Bytes Received Per Second|Bytes|Ortalama|The network bytes received per second.|No Dimensions|
|NetworkBytesTransmittedPerSecond|Network Bytes Transmitted Per Second|Bytes|Ortalama|The network bytes transmitted per second.|No Dimensions|

## <a name="microsoftcontainerregistryregistries"></a>Microsoft.ContainerRegistry/registries

|Ölçüm|Metric Display Name|Birim|Aggregation Type|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|TotalPullCount|Total Pull Count|Sayı|Ortalama|Number of image pulls in total|No Dimensions|
|SuccessfulPullCount|Successful Pull Count|Sayı|Ortalama|Number of successful image pulls|No Dimensions|
|TotalPushCount|Total Push Count|Sayı|Ortalama|Number of image pushes in total|No Dimensions|
|SuccessfulPushCount|Successful Push Count|Sayı|Ortalama|Number of successful image pushes|No Dimensions|
|RunDuration|Run Duration|Milliseconds|Toplam|Run Duration in milliseconds|No Dimensions|

## <a name="microsoftcontainerservicemanagedclusters"></a>Microsoft.ContainerService/managedClusters

|Ölçüm|Metric Display Name|Birim|Aggregation Type|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|kube_node_status_allocatable_cpu_cores|Total number of available cpu cores in a managed cluster|Sayı|Toplam|Total number of available cpu cores in a managed cluster|No Dimensions|
|kube_node_status_allocatable_memory_bytes|Total amount of available memory in a managed cluster|Bytes|Toplam|Total amount of available memory in a managed cluster|No Dimensions|
|kube_pod_status_ready|Number of pods in Ready state|Sayı|Toplam|Number of pods in Ready state|namespace, pod|
|kube_node_status_condition|Statuses for various node conditions|Sayı|Toplam|Statuses for various node conditions|condition, status, status2, node|
|kube_pod_status_phase|Number of pods by phase|Sayı|Toplam|Number of pods by phase|phase, namespace, pod|

## <a name="microsoftcustomerinsightshubs"></a>Microsoft.CustomerInsights/hubs

|Ölçüm|Metric Display Name|Birim|Aggregation Type|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|DCIApiCalls|Customer Insights API Calls|Sayı|Toplam||No Dimensions|
|DCIMappingImportOperationSuccessfulLines|Mapping Import Operation Successful Lines|Sayı|Toplam||No Dimensions|
|DCIMappingImportOperationFailedLines|Mapping Import Operation Failed Lines|Sayı|Toplam||No Dimensions|
|DCIMappingImportOperationTotalLines|Mapping Import Operation Total Lines|Sayı|Toplam||No Dimensions|
|DCIMappingImportOperationRuntimeInSeconds|Mapping Import Operation Runtime In Seconds|Saniye|Toplam||No Dimensions|
|DCIOutboundProfileExportSucceeded|Outbound Profile Export Succeeded|Sayı|Toplam||No Dimensions|
|DCIOutboundProfileExportFailed|Outbound Profile Export Failed|Sayı|Toplam||No Dimensions|
|DCIOutboundProfileExportDuration|Outbound Profile Export Duration|Saniye|Toplam||No Dimensions|
|DCIOutboundKpiExportSucceeded|Outbound Kpi Export Succeeded|Sayı|Toplam||No Dimensions|
|DCIOutboundKpiExportFailed|Outbound Kpi Export Failed|Sayı|Toplam||No Dimensions|
|DCIOutboundKpiExportDuration|Outbound Kpi Export Duration|Saniye|Toplam||No Dimensions|
|DCIOutboundKpiExportStarted|Outbound Kpi Export Started|Saniye|Toplam||No Dimensions|
|DCIOutboundKpiRecordCount|Outbound Kpi Record Count|Saniye|Toplam||No Dimensions|
|DCIOutboundProfileExportCount|Outbound Profile Export Count|Saniye|Toplam||No Dimensions|
|DCIOutboundInitialProfileExportFailed|Outbound Initial Profile Export Failed|Saniye|Toplam||No Dimensions|
|DCIOutboundInitialProfileExportSucceeded|Outbound Initial Profile Export Succeeded|Saniye|Toplam||No Dimensions|
|DCIOutboundInitialKpiExportFailed|Outbound Initial Kpi Export Failed|Saniye|Toplam||No Dimensions|
|DCIOutboundInitialKpiExportSucceeded|Outbound Initial Kpi Export Succeeded|Saniye|Toplam||No Dimensions|
|DCIOutboundInitialProfileExportDurationInSeconds|Outbound Initial Profile Export Duration In Seconds|Saniye|Toplam||No Dimensions|
|AdlaJobForStandardKpiFailed|Adla Job For Standard Kpi Failed In Seconds|Saniye|Toplam||No Dimensions|
|AdlaJobForStandardKpiTimeOut|Adla Job For Standard Kpi TimeOut In Seconds|Saniye|Toplam||No Dimensions|
|AdlaJobForStandardKpiCompleted|Adla Job For Standard Kpi Completed In Seconds|Saniye|Toplam||No Dimensions|
|ImportASAValuesFailed|Import ASA Values Failed Count|Sayı|Toplam||No Dimensions|
|ImportASAValuesSucceeded|Import ASA Values Succeeded Count|Sayı|Toplam||No Dimensions|
|DCIProfilesCount|Profile Instance Count|Sayı|Last||No Dimensions|
|DCIInteractionsPerMonthCount|Interactions per Month Count|Sayı|Last||No Dimensions|
|DCIKpisCount|KPI Count|Sayı|Last||No Dimensions|
|DCISegmentsCount|Segment Count|Sayı|Last||No Dimensions|
|DCIPredictiveMatchPoliciesCount|Predictive Match Count|Sayı|Last||No Dimensions|
|DCIPredictionsCount|Prediction Count|Sayı|Last||No Dimensions|

## <a name="microsoftdataboxedgedataboxedgedevices"></a>Microsoft.DataBoxEdge/dataBoxEdgeDevices

|Ölçüm|Metric Display Name|Birim|Aggregation Type|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|NICReadThroughput|Read Throughput (Network)|BytesPerSecond|Ortalama|The read throughput of the network interface on the device in the reporting period for all volumes in the gateway.|InstanceName|
|NICWriteThroughput|Write Throughput (Network)|BytesPerSecond|Ortalama|The write throughput of the network interface on the device in the reporting period for all volumes in the gateway.|InstanceName|
|CloudReadThroughputPerShare|Cloud Download Throughput (Share)|BytesPerSecond|Ortalama|The download throughput to Azure from a share during the reporting period.|Paylaşın|
|CloudUploadThroughputPerShare|Cloud Upload Throughput (Share)|BytesPerSecond|Ortalama|The upload throughput to Azure from a share during the reporting period.|Paylaşın|
|BytesUploadedToCloudPerShare|Cloud Bytes Uploaded (Share)|Bytes|Ortalama|The total number of bytes that is uploaded to Azure from a share during the reporting period.|Paylaşın|
|TotalCapacity|Total Capacity|Bytes|Ortalama|Total Capacity|No Dimensions|
|AvailableCapacity|Available Capacity|Bytes|Ortalama|The available capacity in bytes during the reporting period.|No Dimensions|
|CloudUploadThroughput|Cloud Upload Throughput|BytesPerSecond|Ortalama|The cloud upload throughput  to Azure during the reporting period.|No Dimensions|
|CloudReadThroughput|Cloud Download Throughput|BytesPerSecond|Ortalama|The cloud download throughput to Azure during the reporting period.|No Dimensions|
|BytesUploadedToCloud|Cloud Bytes Uploaded (Device)|Bytes|Ortalama|The total number of bytes that is uploaded to Azure from a device during the reporting period.|No Dimensions|
|HyperVVirtualProcessorUtilization|Edge Compute - Percentage CPU|Yüzde|Ortalama|Percent CPU Usage|InstanceName|
|HyperVMemoryUtilization|Edge Compute - Memory Usage|Yüzde|Ortalama|Amount of RAM in Use|InstanceName|

## <a name="microsoftdatafactorydatafactories"></a>Microsoft.DataFactory/datafactories

|Ölçüm|Metric Display Name|Birim|Aggregation Type|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|FailedRuns|Failed Runs|Sayı|Toplam||pipelineName, activityName|
|SuccessfulRuns|Successful Runs|Sayı|Toplam||pipelineName, activityName|

## <a name="microsoftdatafactoryfactories"></a>Microsoft.DataFactory/factories

|Ölçüm|Metric Display Name|Birim|Aggregation Type|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|PipelineFailedRuns|Failed pipeline runs metrics|Sayı|Toplam||FailureType, Name|
|PipelineSucceededRuns|Succeeded pipeline runs metrics|Sayı|Toplam||FailureType, Name|
|ActivityFailedRuns|Failed activity runs metrics|Sayı|Toplam||ActivityType, PipelineName, FailureType, Name|
|ActivitySucceededRuns|Succeeded activity runs metrics|Sayı|Toplam||ActivityType, PipelineName, FailureType, Name|
|TriggerFailedRuns|Failed trigger runs metrics|Sayı|Toplam||Name, FailureType|
|TriggerSucceededRuns|Succeeded trigger runs metrics|Sayı|Toplam||Name, FailureType|
|IntegrationRuntimeCpuPercentage|Integration runtime CPU utilization|Yüzde|Ortalama||IntegrationRuntimeName, NodeName|
|IntegrationRuntimeAvailableMemory|Integration runtime available memory|Bytes|Ortalama||IntegrationRuntimeName, NodeName|
|MaxAllowedResourceCount|Maximum allowed entities count|Sayı|Maksimum||No Dimensions|
|MaxAllowedFactorySizeInGbUnits|Maximum allowed factory size (GB unit)|Sayı|Maksimum||No Dimensions|
|ResourceCount|Total entities count|Sayı|Maksimum||No Dimensions|
|FactorySizeInGbUnits|Total factory size (GB unit)|Sayı|Maksimum||No Dimensions|

## <a name="microsoftdatalakeanalyticsaccounts"></a>Microsoft.DataLakeAnalytics/accounts

|Ölçüm|Metric Display Name|Birim|Aggregation Type|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|JobEndedSuccess|Successful Jobs|Sayı|Toplam|Count of successful jobs.|No Dimensions|
|JobEndedFailure|Failed Jobs|Sayı|Toplam|Count of failed jobs.|No Dimensions|
|JobEndedCancelled|Canceled Jobs|Sayı|Toplam|Count of canceled jobs.|No Dimensions|
|JobAUEndedSuccess|Successful AU Time|Saniye|Toplam|Total AU time for successful jobs.|No Dimensions|
|JobAUEndedFailure|Failed AU Time|Saniye|Toplam|Total AU time for failed jobs.|No Dimensions|
|JobAUEndedCancelled|Canceled AU Time|Saniye|Toplam|Total AU time for canceled jobs.|No Dimensions|

## <a name="microsoftdatalakestoreaccounts"></a>Microsoft.DataLakeStore/accounts

|Ölçüm|Metric Display Name|Birim|Aggregation Type|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|TotalStorage|Toplam Depolama Alanı|Bytes|Maksimum|Total amount of data stored in the account.|No Dimensions|
|DataWritten|Data Written|Bytes|Toplam|Total amount of data written to the account.|No Dimensions|
|DataRead|Data Read|Bytes|Toplam|Total amount of data read from the account.|No Dimensions|
|WriteRequests|Write Requests|Sayı|Toplam|Count of data write requests to the account.|No Dimensions|
|ReadRequests|Read Requests|Sayı|Toplam|Count of data read requests to the account.|No Dimensions|

## <a name="microsoftdbformariadbservers"></a>Microsoft.DBforMariaDB/servers

|Ölçüm|Metric Display Name|Birim|Aggregation Type|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|cpu_percent|CPU percent|Yüzde|Ortalama|CPU percent|No Dimensions|
|memory_percent|Memory percent|Yüzde|Ortalama|Memory percent|No Dimensions|
|io_consumption_percent|IO percent|Yüzde|Ortalama|IO percent|No Dimensions|
|storage_percent|Storage percent|Yüzde|Ortalama|Storage percent|No Dimensions|
|storage_used|Kullanılan depolama|Bytes|Ortalama|Kullanılan depolama|No Dimensions|
|storage_limit|Storage limit|Bytes|Ortalama|Storage limit|No Dimensions|
|serverlog_storage_percent|Server Log storage percent|Yüzde|Ortalama|Server Log storage percent|No Dimensions|
|serverlog_storage_usage|Server Log storage used|Bytes|Ortalama|Server Log storage used|No Dimensions|
|serverlog_storage_limit|Server Log storage limit|Bytes|Ortalama|Server Log storage limit|No Dimensions|
|active_connections|Active Connections|Sayı|Ortalama|Active Connections|No Dimensions|
|connections_failed|Başarısız Bağlantılar|Sayı|Toplam|Başarısız Bağlantılar|No Dimensions|
|seconds_behind_master|Replication lag in seconds|Sayı|Ortalama|Replication lag in seconds|No Dimensions|
|backup_storage_used|Backup Storage used|Bytes|Ortalama|Backup Storage used|No Dimensions|
|network_bytes_egress|Ağ Çıkışı|Bytes|Toplam|Network Out across active connections|No Dimensions|
|network_bytes_ingress|Ağ Girişi|Bytes|Toplam|Network In across active connections|No Dimensions|

## <a name="microsoftdbformysqlservers"></a>Microsoft.DBforMySQL/servers

|Ölçüm|Metric Display Name|Birim|Aggregation Type|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|cpu_percent|CPU percent|Yüzde|Ortalama|CPU percent|No Dimensions|
|memory_percent|Memory percent|Yüzde|Ortalama|Memory percent|No Dimensions|
|io_consumption_percent|IO percent|Yüzde|Ortalama|IO percent|No Dimensions|
|storage_percent|Storage percent|Yüzde|Ortalama|Storage percent|No Dimensions|
|storage_used|Kullanılan depolama|Bytes|Ortalama|Kullanılan depolama|No Dimensions|
|storage_limit|Storage limit|Bytes|Ortalama|Storage limit|No Dimensions|
|serverlog_storage_percent|Server Log storage percent|Yüzde|Ortalama|Server Log storage percent|No Dimensions|
|serverlog_storage_usage|Server Log storage used|Bytes|Ortalama|Server Log storage used|No Dimensions|
|serverlog_storage_limit|Server Log storage limit|Bytes|Ortalama|Server Log storage limit|No Dimensions|
|active_connections|Active Connections|Sayı|Ortalama|Active Connections|No Dimensions|
|connections_failed|Başarısız Bağlantılar|Sayı|Toplam|Başarısız Bağlantılar|No Dimensions|
|seconds_behind_master|Replication lag in seconds|Sayı|Ortalama|Replication lag in seconds|No Dimensions|
|backup_storage_used|Backup Storage used|Bytes|Ortalama|Backup Storage used|No Dimensions|
|network_bytes_egress|Ağ Çıkışı|Bytes|Toplam|Network Out across active connections|No Dimensions|
|network_bytes_ingress|Ağ Girişi|Bytes|Toplam|Network In across active connections|No Dimensions|

## <a name="microsoftdbforpostgresqlservers"></a>Microsoft.DBforPostgreSQL/servers

|Ölçüm|Metric Display Name|Birim|Aggregation Type|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|cpu_percent|CPU percent|Yüzde|Ortalama|CPU percent|No Dimensions|
|memory_percent|Memory percent|Yüzde|Ortalama|Memory percent|No Dimensions|
|io_consumption_percent|IO percent|Yüzde|Ortalama|IO percent|No Dimensions|
|storage_percent|Storage percent|Yüzde|Ortalama|Storage percent|No Dimensions|
|storage_used|Kullanılan depolama|Bytes|Ortalama|Kullanılan depolama|No Dimensions|
|storage_limit|Storage limit|Bytes|Ortalama|Storage limit|No Dimensions|
|serverlog_storage_percent|Server Log storage percent|Yüzde|Ortalama|Server Log storage percent|No Dimensions|
|serverlog_storage_usage|Server Log storage used|Bytes|Ortalama|Server Log storage used|No Dimensions|
|serverlog_storage_limit|Server Log storage limit|Bytes|Ortalama|Server Log storage limit|No Dimensions|
|active_connections|Active Connections|Sayı|Ortalama|Active Connections|No Dimensions|
|connections_failed|Başarısız Bağlantılar|Sayı|Toplam|Başarısız Bağlantılar|No Dimensions|
|backup_storage_used|Backup Storage used|Bytes|Ortalama|Backup Storage used|No Dimensions|
|network_bytes_egress|Ağ Çıkışı|Bytes|Toplam|Network Out across active connections|No Dimensions|
|network_bytes_ingress|Ağ Girişi|Bytes|Toplam|Network In across active connections|No Dimensions|
|pg_replica_log_delay_in_seconds|Replica Lag|Saniye|Maksimum|Replica lag in seconds|No Dimensions|
|pg_replica_log_delay_in_bytes|Max Lag Across Replicas|Bytes|Maksimum|Lag in bytes of the most lagging replica|No Dimensions|

## <a name="microsoftdbforpostgresqlserversv2"></a>Microsoft.DBforPostgreSQL/serversv2

|Ölçüm|Metric Display Name|Birim|Aggregation Type|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|cpu_percent|CPU percent|Yüzde|Ortalama|CPU percent|No Dimensions|
|memory_percent|Memory percent|Yüzde|Ortalama|Memory percent|No Dimensions|
|iops|IOPS|Sayı|Ortalama|IO Operations per second|No Dimensions|
|storage_percent|Storage percent|Yüzde|Ortalama|Storage percent|No Dimensions|
|storage_used|Kullanılan depolama|Bytes|Ortalama|Kullanılan depolama|No Dimensions|
|active_connections|Active Connections|Sayı|Ortalama|Active Connections|No Dimensions|
|network_bytes_egress|Ağ Çıkışı|Bytes|Toplam|Network Out across active connections|No Dimensions|
|network_bytes_ingress|Ağ Girişi|Bytes|Toplam|Network In across active connections|No Dimensions|

## <a name="microsoftdevicesiothubs"></a>Microsoft.Devices/IotHubs

|Ölçüm|Metric Display Name|Birim|Aggregation Type|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|d2c.telemetry.ingress.allProtocol|Telemetry message send attempts|Sayı|Toplam|Number of device-to-cloud telemetry messages attempted to be sent to your IoT hub|No Dimensions|
|d2c.telemetry.ingress.success|Telemetry messages sent|Sayı|Toplam|Number of device-to-cloud telemetry messages sent successfully to your IoT hub|No Dimensions|
|c2d.commands.egress.complete.success|Commands completed|Sayı|Toplam|Number of cloud-to-device commands completed successfully by the device|No Dimensions|
|c2d.commands.egress.abandon.success|Commands abandoned|Sayı|Toplam|Number of cloud-to-device commands abandoned by the device|No Dimensions|
|c2d.commands.egress.reject.success|Commands rejected|Sayı|Toplam|Number of cloud-to-device commands rejected by the device|No Dimensions|
|devices.totalDevices|Total devices (deprecated)|Sayı|Toplam|Number of devices registered to your IoT hub|No Dimensions|
|devices.connectedDevices.allProtocol|Connected devices (deprecated) |Sayı|Toplam|Number of devices connected to your IoT hub|No Dimensions|
|d2c.telemetry.egress.success|Routing: telemetry messages delivered|Sayı|Toplam|The number of times messages were successfully delivered to all endpoints using IoT Hub routing. If a message is routed to multiple endpoints, this value increases by one for each successful delivery. If a message is delivered to the same endpoint multiple times, this value increases by one for each successful delivery.|No Dimensions|
|d2c.telemetry.egress.dropped|Routing: telemetry messages dropped |Sayı|Toplam|The number of times messages were dropped by IoT Hub routing due to dead endpoints. This value does not count messages delivered to fallback route as dropped messages are not delivered there.|No Dimensions|
|d2c.telemetry.egress.orphaned|Routing: telemetry messages orphaned |Sayı|Toplam|The number of times messages were orphaned by IoT Hub routing because they didn't match any routing rules (including the fallback rule). |No Dimensions|
|d2c.telemetry.egress.invalid|Routing: telemetry messages incompatible|Sayı|Toplam|The number of times IoT Hub routing failed to deliver messages due to an incompatibility with the endpoint. This value does not include retries.|No Dimensions|
|d2c.telemetry.egress.fallback|Routing: messages delivered to fallback|Sayı|Toplam|The number of times IoT Hub routing delivered messages to the endpoint associated with the fallback route.|No Dimensions|
|d2c.endpoints.egress.eventHubs|Routing: messages delivered to Event Hub|Sayı|Toplam|The number of times IoT Hub routing successfully delivered messages to Event Hub endpoints.|No Dimensions|
|d2c.endpoints.latency.eventHubs|Routing: message latency for Event Hub|Milliseconds|Ortalama|The average latency (milliseconds) between message ingress to IoT Hub and message ingress into an Event Hub endpoint.|No Dimensions|
|d2c.endpoints.egress.serviceBusQueues|Routing: messages delivered to Service Bus Queue|Sayı|Toplam|The number of times IoT Hub routing successfully delivered messages to Service Bus queue endpoints.|No Dimensions|
|d2c.endpoints.latency.serviceBusQueues|Routing: message latency for Service Bus Queue|Milliseconds|Ortalama|The average latency (milliseconds) between message ingress to IoT Hub and telemetry message ingress into a Service Bus queue endpoint.|No Dimensions|
|d2c.endpoints.egress.serviceBusTopics|Routing: messages delivered to Service Bus Topic|Sayı|Toplam|The number of times IoT Hub routing successfully delivered messages to Service Bus topic endpoints.|No Dimensions|
|d2c.endpoints.latency.serviceBusTopics|Routing: message latency for Service Bus Topic|Milliseconds|Ortalama|The average latency (milliseconds) between message ingress to IoT Hub and telemetry message ingress into a Service Bus topic endpoint.|No Dimensions|
|d2c.endpoints.egress.builtIn.events|Routing: messages delivered to messages/events|Sayı|Toplam|The number of times IoT Hub routing successfully delivered messages to the built-in endpoint (messages/events). This metric only starts working when routing is enabled (https://aka.ms/iotrouting) for the IoT hub.|No Dimensions|
|d2c.endpoints.latency.builtIn.events|Routing: message latency for messages/events|Milliseconds|Ortalama|The average latency (milliseconds) between message ingress to IoT Hub and telemetry message ingress into the built-in endpoint (messages/events). This metric only starts working when routing is enabled (https://aka.ms/iotrouting) for the IoT hub.|No Dimensions|
|d2c.endpoints.egress.storage|Routing: messages delivered to storage|Sayı|Toplam|The number of times IoT Hub routing successfully delivered messages to storage endpoints.|No Dimensions|
|d2c.endpoints.latency.storage|Routing: message latency for storage|Milliseconds|Ortalama|The average latency (milliseconds) between message ingress to IoT Hub and telemetry message ingress into a storage endpoint.|No Dimensions|
|d2c.endpoints.egress.storage.bytes|Routing: data delivered to storage|Bytes|Toplam|The amount of data (bytes) IoT Hub routing delivered to storage endpoints.|No Dimensions|
|d2c.endpoints.egress.storage.blobs|Routing: blobs delivered to storage|Sayı|Toplam|The number of times IoT Hub routing delivered blobs to storage endpoints.|No Dimensions|
|EventGridDeliveries|Event Grid deliveries (preview)|Sayı|Toplam|The number of IoT Hub events published to Event Grid. Use the Result dimension for the number of successful and failed requests. EventType dimension shows the type of event (https://aka.ms/ioteventgrid).|Result, EventType|
|EventGridLatency|The average latency (milliseconds) from when the Iot Hub event was generated to when the event was published to Event Grid. This number is an average between all event types. Use the EventType dimension to see latency of a specific type of event.|EventType|
|d2c.twin.read.success|Successful twin reads from devices|Sayı|Toplam|The count of all successful device-initiated twin reads.|No Dimensions|
|d2c.twin.read.failure|Failed twin reads from devices|Sayı|Toplam|The count of all failed device-initiated twin reads.|No Dimensions|
|d2c.twin.read.size|Response size of twin reads from devices|Bytes|Ortalama|The average, min, and max of all successful device-initiated twin reads.|No Dimensions|
|d2c.twin.update.success|Successful twin updates from devices|Sayı|Toplam|The count of all successful device-initiated twin updates.|No Dimensions|
|d2c.twin.update.failure|Failed twin updates from devices|Sayı|Toplam|The count of all failed device-initiated twin updates.|No Dimensions|
|d2c.twin.update.size|Size of twin updates from devices|Bytes|Ortalama|The average, min, and max size of all successful device-initiated twin updates.|No Dimensions|
|c2d.methods.success|Successful direct method invocations|Sayı|Toplam|The count of all successful direct method calls.|No Dimensions|
|c2d.methods.failure|Failed direct method invocations|Sayı|Toplam|The count of all failed direct method calls.|No Dimensions|
|c2d.methods.requestSize|Request size of direct method invocations|Bytes|Ortalama|The average, min, and max of all successful direct method requests.|No Dimensions|
|c2d.methods.responseSize|Response size of direct method invocations|Bytes|Ortalama|The average, min, and max of all successful direct method responses.|No Dimensions|
|c2d.twin.read.success|Successful twin reads from back end|Sayı|Toplam|The count of all successful back-end-initiated twin reads.|No Dimensions|
|c2d.twin.read.failure|Failed twin reads from back end|Sayı|Toplam|The count of all failed back-end-initiated twin reads.|No Dimensions|
|c2d.twin.read.size|Response size of twin reads from back end|Bytes|Ortalama|The average, min, and max of all successful back-end-initiated twin reads.|No Dimensions|
|c2d.twin.update.success|Successful twin updates from back end|Sayı|Toplam|The count of all successful back-end-initiated twin updates.|No Dimensions|
|c2d.twin.update.failure|Failed twin updates from back end|Sayı|Toplam|The count of all failed back-end-initiated twin updates.|No Dimensions|
|c2d.twin.update.size|Size of twin updates from back end|Bytes|Ortalama|The average, min, and max size of all successful back-end-initiated twin updates.|No Dimensions|
|twinQueries.success|Successful twin queries|Sayı|Toplam|The count of all successful twin queries.|No Dimensions|
|twinQueries.failure|Failed twin queries|Sayı|Toplam|The count of all failed twin queries.|No Dimensions|
|twinQueries.resultSize|Twin queries result size|Bytes|Ortalama|The average, min, and max of the result size of all successful twin queries.|No Dimensions|
|jobs.createTwinUpdateJob.success|Successful creations of twin update jobs|Sayı|Toplam|The count of all successful creation of twin update jobs.|No Dimensions|
|jobs.createTwinUpdateJob.failure|Failed creations of twin update jobs|Sayı|Toplam|The count of all failed creation of twin update jobs.|No Dimensions|
|jobs.createDirectMethodJob.success|Successful creations of method invocation jobs|Sayı|Toplam|The count of all successful creation of direct method invocation jobs.|No Dimensions|
|jobs.createDirectMethodJob.failure|Failed creations of method invocation jobs|Sayı|Toplam|The count of all failed creation of direct method invocation jobs.|No Dimensions|
|jobs.listJobs.success|Successful calls to list jobs|Sayı|Toplam|The count of all successful calls to list jobs.|No Dimensions|
|jobs.listJobs.failure|Failed calls to list jobs|Sayı|Toplam|The count of all failed calls to list jobs.|No Dimensions|
|jobs.cancelJob.success|Successful job cancellations|Sayı|Toplam|The count of all successful calls to cancel a job.|No Dimensions|
|jobs.cancelJob.failure|Failed job cancellations|Sayı|Toplam|The count of all failed calls to cancel a job.|No Dimensions|
|jobs.queryJobs.success|Successful job queries|Sayı|Toplam|The count of all successful calls to query jobs.|No Dimensions|
|jobs.queryJobs.failure|Failed job queries|Sayı|Toplam|The count of all failed calls to query jobs.|No Dimensions|
|jobs.completed|Completed jobs|Sayı|Toplam|The count of all completed jobs.|No Dimensions|
|jobs.failed|Failed jobs|Sayı|Toplam|The count of all failed jobs.|No Dimensions|
|d2c.telemetry.ingress.sendThrottle|Number of throttling errors|Sayı|Toplam|Number of throttling errors due to device throughput throttles|No Dimensions|
|dailyMessageQuotaUsed|Total number of messages used|Sayı|Ortalama|Number of total messages used today. This is a cumulative value that is reset to zero at 00:00 UTC every day.|No Dimensions|
|deviceDataUsage|Total device data usage|Bytes|Toplam|Bytes transferred to and from any devices connected to IotHub|No Dimensions|
|totalDeviceCount|Total devices (preview)|Sayı|Ortalama|Number of devices registered to your IoT hub|No Dimensions|
|connectedDeviceCount|Connected devices (preview)|Sayı|Ortalama|Number of devices connected to your IoT hub|No Dimensions|
|configurations|Configuration Metrics|Sayı|Toplam|Metrics for Configuration Operations|No Dimensions|

## <a name="microsoftdevicesprovisioningservices"></a>Microsoft.Devices/provisioningServices

|Ölçüm|Metric Display Name|Birim|Aggregation Type|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|RegistrationAttempts|Registration attempts|Sayı|Toplam|Number of device registrations attempted|ProvisioningServiceName, IotHubName, Status|
|DeviceAssignments|Devices assigned|Sayı|Toplam|Number of devices assigned to an IoT hub|ProvisioningServiceName, IotHubName|
|AttestationAttempts|Attestation attempts|Sayı|Toplam|Number of device attestations attempted|ProvisioningServiceName, Status, Protocol|

## <a name="microsoftdocumentdbdatabaseaccounts"></a>Microsoft.DocumentDB/databaseAccounts

|Ölçüm|Metric Display Name|Birim|Aggregation Type|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|AvailableStorage|Available Storage|Bytes|Toplam|Total available storage reported at 5 minutes granularity|CollectionName, DatabaseName, Region|
|CassandraConnectionClosures|Cassandra Connection Closures|Sayı|Toplam|Number of Cassandra connections that were closed, reported at a 1 minute granularity|Region, ClosureReason|
|CassandraRequestCharges|Cassandra Request Charges|Sayı|Toplam|RUs consumed for Cassandra requests made|DatabaseName, CollectionName, Region, OperationType, ResourceType|
|CassandraRequests|Cassandra Requests|Sayı|Sayı|Number of Cassandra requests made|DatabaseName, CollectionName, Region, OperationType, ResourceType, ErrorCode|
|DataUsage|Veri Kullanımı|Bytes|Toplam|Total data usage reported at 5 minutes granularity|CollectionName, DatabaseName, Region|
|DocumentCount|Document Count|Sayı|Toplam|Total document count reported at 5 minutes granularity|CollectionName, DatabaseName, Region|
|DocumentQuota|Document Quota|Bytes|Toplam|Total storage quota reported at 5 minutes granularity|CollectionName, DatabaseName, Region|
|IndexUsage|Index Usage|Bytes|Toplam|Total index usage reported at 5 minutes granularity|CollectionName, DatabaseName, Region|
|MetadataRequests|Metadata Requests|Sayı|Sayı|Count of metadata requests. Cosmos DB maintains system metadata collection for each account, that allows you to enumerate collections, databases, etc., and their configurations, free of charge.|DatabaseName, CollectionName, Region, StatusCode, |
|MongoRequestCharge|Mongo Request Charge|Sayı|Toplam|Mongo Request Units Consumed|DatabaseName, CollectionName, Region, CommandName, ErrorCode|
|MongoRequests|Mongo Requests|Sayı|Sayı|Number of Mongo Requests Made|DatabaseName, CollectionName, Region, CommandName, ErrorCode|
|ProvisionedThroughput|Sağlanan Aktarım Hızı|Sayı|Maksimum|Sağlanan Aktarım Hızı|DatabaseName, CollectionName|
|ReplicationLatency|P99 Replication Latency|MilliSeconds|Ortalama|P99 Replication Latency across source and target regions for geo-enabled account|SourceRegion, TargetRegion|
|ServiceAvailability|Service Availability|Yüzde|Ortalama|Account requests availability at one hour, day or month granularity|No Dimensions|
|TotalRequestUnits|Total Request Units|Sayı|Toplam|Request Units consumed|DatabaseName, CollectionName, Region, StatusCode, OperationType|
|TotalRequests|Toplam İstek Sayısı|Sayı|Sayı|Number of requests made|DatabaseName, CollectionName, Region, StatusCode, OperationType|

## <a name="microsofteventgridtopics"></a>Microsoft.EventGrid/topics

|Ölçüm|Metric Display Name|Birim|Aggregation Type|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|PublishSuccessCount|Published Events|Sayı|Toplam|Total events published to this topic|No Dimensions|
|PublishFailCount|Publish Failed Events|Sayı|Toplam|Total events failed to publish to this topic|ErrorType, Error|
|UnmatchedEventCount|Unmatched Events|Sayı|Toplam|Total events not matching any of the event subscriptions for this topic|No Dimensions|
|PublishSuccessLatencyInMs|Publish Success Latency|Sayı|Toplam|Publish success latency in milliseconds|No Dimensions|

## <a name="microsofteventgrideventsubscriptions"></a>Microsoft.EventGrid/eventSubscriptions

|Ölçüm|Metric Display Name|Birim|Aggregation Type|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|MatchedEventCount|Matched Events|Sayı|Toplam|Total events matched to this event subscription|No Dimensions|
|DeliveryAttemptFailCount|Delivery Failed Events|Sayı|Toplam|Total events failed to deliver to this event subscription|Error, ErrorType|
|DeliverySuccessCount|Delivered Events|Sayı|Toplam|Total events delivered to this event subscription|No Dimensions|
|DestinationProcessingDurationInMs|Destination Processing Duration|Milliseconds|Ortalama|Destination processing duration in milliseconds|No Dimensions|
|DroppedEventCount|Dropped Events|Sayı|Toplam|Total dropped events matching to this event subscription|DropReason|
|DeadLetteredCount|Dead Lettered Events|Sayı|Toplam|Total dead lettered events matching to this event subscription|DeadLetterReason|

## <a name="microsofteventgridextensiontopics"></a>Microsoft.EventGrid/extensionTopics

|Ölçüm|Metric Display Name|Birim|Aggregation Type|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|PublishSuccessCount|Published Events|Sayı|Toplam|Total events published to this topic|No Dimensions|
|PublishFailCount|Failed Events|Sayı|Toplam|Total events failed to publish to this topic|ErrorType, Error|
|UnmatchedEventCount|Unmatched Events|Sayı|Toplam|Total events not matching any of the event subscriptions for this topic|No Dimensions|
|PublishSuccessLatencyInMs|Publish Success Latency|Sayı|Toplam|Publish success latency in milliseconds|No Dimensions|

## <a name="microsofteventhubnamespaces"></a>Microsoft.EventHub/namespaces

|Ölçüm|Metric Display Name|Birim|Aggregation Type|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|SuccessfulRequests|Successful Requests|Sayı|Toplam|Successful Requests for Microsoft.EventHub.|EntityName, |
|ServerErrors|Server Errors.|Sayı|Toplam|Server Errors for Microsoft.EventHub.|EntityName, |
|UserErrors|User Errors.|Sayı|Toplam|User Errors for Microsoft.EventHub.|EntityName, |
|QuotaExceededErrors|Quota Exceeded Errors.|Sayı|Toplam|Quota Exceeded Errors for Microsoft.EventHub.|EntityName, |
|OperationName|Throttled Requests.|Sayı|Toplam|Throttled Requests for Microsoft.EventHub.|EntityName, |
|IncomingRequests|Incoming Requests|Sayı|Toplam|Incoming Requests for Microsoft.EventHub.|EntityName|
|IncomingMessages|Incoming Messages|Sayı|Toplam|Incoming Messages for Microsoft.EventHub.|EntityName|
|OutgoingMessages|Outgoing Messages|Sayı|Toplam|Outgoing Messages for Microsoft.EventHub.|EntityName|
|IncomingBytes|Incoming Bytes.|Bytes|Toplam|Incoming Bytes for Microsoft.EventHub.|EntityName|
|OutgoingBytes|Outgoing Bytes.|Bytes|Toplam|Outgoing Bytes for Microsoft.EventHub.|EntityName|
|ActiveConnections|ActiveConnections|Sayı|Ortalama|Total Active Connections for Microsoft.EventHub.|No Dimensions|
|ConnectionsOpened|Connections Opened.|Sayı|Ortalama|Connections Opened for Microsoft.EventHub.|EntityName|
|ConnectionsClosed|Connections Closed.|Sayı|Ortalama|Connections Closed for Microsoft.EventHub.|EntityName|
|CaptureBacklog|Capture Backlog.|Sayı|Toplam|Capture Backlog for Microsoft.EventHub.|EntityName|
|CapturedMessages|Captured Messages.|Sayı|Toplam|Captured Messages for Microsoft.EventHub.|EntityName|
|CapturedBytes|Captured Bytes.|Bytes|Toplam|Captured Bytes for Microsoft.EventHub.|EntityName|
|Boyut|Boyut|Bytes|Ortalama|Size of an EventHub in Bytes.|EntityName|
|INREQS|Incoming Requests (Deprecated)|Sayı|Toplam|Total incoming send requests for a namespace (Deprecated)|No Dimensions|
|SUCCREQ|Successful Requests (Deprecated)|Sayı|Toplam|Total successful requests for a namespace (Deprecated)|No Dimensions|
|FAILREQ|Failed Requests (Deprecated)|Sayı|Toplam|Total failed requests for a namespace (Deprecated)|No Dimensions|
|SVRBSY|Server Busy Errors (Deprecated)|Sayı|Toplam|Total server busy errors for a namespace (Deprecated)|No Dimensions|
|INTERR|Internal Server Errors (Deprecated)|Sayı|Toplam|Total internal server errors for a namespace (Deprecated)|No Dimensions|
|MISCERR|Other Errors (Deprecated)|Sayı|Toplam|Total failed requests for a namespace (Deprecated)|No Dimensions|
|INMSGS|Incoming Messages (Deprecated) (Deprecated)|Sayı|Toplam|Total incoming messages for a namespace. This metric is deprecated. Please use Incoming Messages metric instead (Deprecated)|No Dimensions|
|EHINMSGS|Incoming Messages (Deprecated)|Sayı|Toplam|Total incoming messages for a namespace (Deprecated)|No Dimensions|
|OUTMSGS|Outgoing Messages (Deprecated) (Deprecated)|Sayı|Toplam|Total outgoing messages for a namespace. This metric is deprecated. Please use Outgoing Messages metric instead (Deprecated)|No Dimensions|
|EHOUTMSGS|Outgoing Messages (Deprecated)|Sayı|Toplam|Total outgoing messages for a namespace (Deprecated)|No Dimensions|
|EHINMBS|Incoming bytes (Deprecated) (Deprecated)|Bytes|Toplam|Event Hub incoming message throughput for a namespace. This metric is deprecated. Please use Incoming bytes metric instead (Deprecated)|No Dimensions|
|EHINBYTES|Incoming bytes (Deprecated)|Bytes|Toplam|Event Hub incoming message throughput for a namespace (Deprecated)|No Dimensions|
|EHOUTMBS|Outgoing bytes (Deprecated) (Deprecated)|Bytes|Toplam|Event Hub outgoing message throughput for a namespace. This metric is deprecated. Please use Outgoing bytes metric instead (Deprecated)|No Dimensions|
|EHOUTBYTES|Outgoing bytes (Deprecated)|Bytes|Toplam|Event Hub outgoing message throughput for a namespace (Deprecated)|No Dimensions|
|EHABL|Archive backlog messages (Deprecated)|Sayı|Toplam|Event Hub archive messages in backlog for a namespace (Deprecated)|No Dimensions|
|EHAMSGS|Archive messages (Deprecated)|Sayı|Toplam|Event Hub archived messages in a namespace (Deprecated)|No Dimensions|
|EHAMBS|Archive message throughput (Deprecated)|Bytes|Toplam|Event Hub archived message throughput in a namespace (Deprecated)|No Dimensions|

## <a name="microsofteventhubclusters"></a>Microsoft.EventHub/clusters

|Ölçüm|Metric Display Name|Birim|Aggregation Type|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|SuccessfulRequests|Successful Requests (Preview)|Sayı|Toplam|Successful Requests for Microsoft.EventHub. (Önizleme)|No Dimensions|
|ServerErrors|Server Errors. (Önizleme)|Sayı|Toplam|Server Errors for Microsoft.EventHub. (Önizleme)|No Dimensions|
|UserErrors|User Errors. (Önizleme)|Sayı|Toplam|User Errors for Microsoft.EventHub. (Önizleme)|No Dimensions|
|QuotaExceededErrors|Quota Exceeded Errors. (Önizleme)|Sayı|Toplam|Quota Exceeded Errors for Microsoft.EventHub. (Önizleme)|No Dimensions|
|OperationName|Throttled Requests. (Önizleme)|Sayı|Toplam|Throttled Requests for Microsoft.EventHub. (Önizleme)|No Dimensions|
|IncomingRequests|Incoming Requests (Preview)|Sayı|Toplam|Incoming Requests for Microsoft.EventHub. (Önizleme)|No Dimensions|
|IncomingMessages|Incoming Messages (Preview)|Sayı|Toplam|Incoming Messages for Microsoft.EventHub. (Önizleme)|No Dimensions|
|OutgoingMessages|Outgoing Messages (Preview)|Sayı|Toplam|Outgoing Messages for Microsoft.EventHub. (Önizleme)|No Dimensions|
|IncomingBytes|Incoming Bytes. (Önizleme)|Bytes|Toplam|Incoming Bytes for Microsoft.EventHub. (Önizleme)|No Dimensions|
|OutgoingBytes|Outgoing Bytes. (Önizleme)|Bytes|Toplam|Outgoing Bytes for Microsoft.EventHub. (Önizleme)|No Dimensions|
|ActiveConnections|ActiveConnections (Preview)|Sayı|Ortalama|Total Active Connections for Microsoft.EventHub. (Önizleme)|No Dimensions|
|ConnectionsOpened|Connections Opened. (Önizleme)|Sayı|Ortalama|Connections Opened for Microsoft.EventHub. (Önizleme)|No Dimensions|
|ConnectionsClosed|Connections Closed. (Önizleme)|Sayı|Ortalama|Connections Closed for Microsoft.EventHub. (Önizleme)|No Dimensions|
|CaptureBacklog|Capture Backlog. (Önizleme)|Sayı|Toplam|Capture Backlog for Microsoft.EventHub. (Önizleme)|No Dimensions|
|CapturedMessages|Captured Messages. (Önizleme)|Sayı|Toplam|Captured Messages for Microsoft.EventHub. (Önizleme)|No Dimensions|
|CapturedBytes|Captured Bytes. (Önizleme)|Bytes|Toplam|Captured Bytes for Microsoft.EventHub. (Önizleme)|No Dimensions|
|CPU|CPU (Preview)|Yüzde|Maksimum|CPU utilization for the Event Hub Cluster as a percentage|Rol|
|AvailableMemory|Available Memory (Preview)|Sayı|Maksimum|Available memory for the Event Hub Cluster in bytes|Rol|

## <a name="microsofthdinsightclusters"></a>Microsoft.HDInsight/clusters

|Ölçüm|Metric Display Name|Birim|Aggregation Type|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|GatewayRequests|Gateway Requests|Sayı|Toplam|Number of gateway requests|ClusterDnsName, HttpStatus|
|CategorizedGatewayRequests|Categorized Gateway Requests|Sayı|Toplam|Number of gateway requests by categories (1xx/2xx/3xx/4xx/5xx)|ClusterDnsName, HttpStatus|
|NumActiveWorkers|Number of Active Workers|Sayı|Maksimum|Number of Active Workers|ClusterDnsName, MetricName|

## <a name="microsoftinsightsautoscalesettings"></a>Microsoft.Insights/AutoscaleSettings

|Ölçüm|Metric Display Name|Birim|Aggregation Type|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|ObservedMetricValue|Observed Metric Value|Sayı|Ortalama|The value computed by autoscale when executed|MetricTriggerSource|
|MetricThreshold|Metric Threshold|Sayı|Ortalama|The configured autoscale threshold when autoscale ran.|MetricTriggerRule|
|ObservedCapacity|Observed Capacity|Sayı|Ortalama|The capacity reported to autoscale when it executed.|No Dimensions|
|ScaleActionsInitiated|Scale Actions Initiated|Sayı|Toplam|The direction of the scale operation.|ScaleDirection|

## <a name="microsoftinsightscomponents"></a>Microsoft.Insights/Components

(Public Preview)

|Ölçüm|Metric Display Name|Birim|Aggregation Type|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|availabilityResults/availabilityPercentage|Erişilebilirlik|Yüzde|Ortalama|Percentage of successfully completed availability tests|availabilityResult/name, availabilityResult/location|
|availabilityResults/count|Kullanılabilirlik testleri|Sayı|Sayı|Count of availability tests|availabilityResult/name, availabilityResult/location, availabilityResult/success|
|availabilityResults/duration|Availability test duration|MilliSeconds|Ortalama|Availability test duration|availabilityResult/name, availabilityResult/location, availabilityResult/success|
|browserTimings/networkDuration|Page load network connect time|MilliSeconds|Ortalama|Time between user request and network connection. Includes DNS lookup and transport connection.|No Dimensions|
|browserTimings/processingDuration|Client processing time|MilliSeconds|Ortalama|Time between receiving the last byte of a document until the DOM is loaded. Async requests may still be processing.|No Dimensions|
|browserTimings/receiveDuration|Receiving response time|MilliSeconds|Ortalama|Time between the first and last bytes, or until disconnection.|No Dimensions|
|browserTimings/sendDuration|Send request time|MilliSeconds|Ortalama|Time between network connection and receiving the first byte.|No Dimensions|
|browserTimings/totalDuration|Browser page load time|MilliSeconds|Ortalama|Time from user request until DOM, stylesheets, scripts and images are loaded.|No Dimensions|
|dependencies/count|Dependency calls|Sayı|Sayı|Count of calls made by the application to external resources.|dependency/type, dependency/performanceBucket, dependency/success, operation/synthetic, cloud/roleInstance, cloud/roleName|
|dependencies/duration|Dependency duration|MilliSeconds|Ortalama|Duration of calls made by the application to external resources.|dependency/type, dependency/performanceBucket, dependency/success, operation/synthetic, cloud/roleInstance, cloud/roleName|
|dependencies/failed|Dependency call failures|Sayı|Sayı|Count of failed dependency calls made by the application to external resources.|dependency/type, dependency/performanceBucket, operation/synthetic, cloud/roleInstance, cloud/roleName|
|pageViews/count|Page views|Sayı|Sayı|Count of page views.|operation/synthetic|
|pageViews/duration|Page view load time|MilliSeconds|Ortalama|Page view load time|operation/synthetic|
|performanceCounters/requestExecutionTime|HTTP request execution time|MilliSeconds|Ortalama|Execution time of the most recent request.|cloud/roleInstance|
|performanceCounters/requestsInQueue|HTTP requests in application queue|Sayı|Ortalama|Length of the application request queue.|cloud/roleInstance|
|performanceCounters/requestsPerSecond|HTTP request rate|CountPerSecond|Ortalama|Rate of all requests to the application per second from ASP.NET.|cloud/roleInstance|
|performanceCounters/exceptionsPerSecond|Exception rate|CountPerSecond|Ortalama|Count of handled and unhandled exceptions reported to windows, including .NET exceptions and unmanaged exceptions that are converted into .NET exceptions.|cloud/roleInstance|
|performanceCounters/processIOBytesPerSecond|Process IO rate|BytesPerSecond|Ortalama|Total bytes per second read and written to files, network and devices.|cloud/roleInstance|
|performanceCounters/processCpuPercentage|Process CPU|Yüzde|Ortalama|The percentage of elapsed time that all process threads used the processor to execute instructions. This can vary between 0 to 100. This metric indicates the performance of w3wp process alone.|cloud/roleInstance|
|performanceCounters/processorCpuPercentage|Processor time|Yüzde|Ortalama|The percentage of time that the processor spends in non-idle threads.|cloud/roleInstance|
|performanceCounters/memoryAvailableBytes|Available memory|Bytes|Ortalama|Physical memory immediately available for allocation to a process or for system use.|cloud/roleInstance|
|performanceCounters/processPrivateBytes|Process private bytes|Bytes|Ortalama|Memory exclusively assigned to the monitored application's processes.|cloud/roleInstance|
|requests/duration|Server response time|MilliSeconds|Ortalama|Time between receiving an HTTP request and finishing sending the response.|request/performanceBucket, request/resultCode, operation/synthetic, cloud/roleInstance, request/success, cloud/roleName|
|requests/count|Server requests|Sayı|Sayı|Count of HTTP requests completed.|request/performanceBucket, request/resultCode, operation/synthetic, cloud/roleInstance, request/success, cloud/roleName|
|requests/failed|Başarısız istekler|Sayı|Sayı|Count of HTTP requests marked as failed. In most cases these are requests with a response code >= 400 and not equal to 401.|request/performanceBucket, request/resultCode, operation/synthetic, cloud/roleInstance, cloud/roleName|
|requests/rate|Server request rate|CountPerSecond|Ortalama|Rate of server requests per second|request/performanceBucket, request/resultCode, operation/synthetic, cloud/roleInstance, request/success, cloud/roleName|
|exceptions/count|Özel Durumlar|Sayı|Sayı|Combined count of all uncaught exceptions.|cloud/roleName, cloud/roleInstance, client/type|
|exceptions/browser|Tarayıcı özel durumları|Sayı|Sayı|Count of uncaught exceptions thrown in the browser.|No Dimensions|
|exceptions/server|Server exceptions|Sayı|Sayı|Count of uncaught exceptions thrown in the server application.|cloud/roleName, cloud/roleInstance|
|traces/count|Traces|Sayı|Sayı|Trace document count|trace/severityLevel, operation/synthetic, cloud/roleName, cloud/roleInstance|

## <a name="microsoftkeyvaultvaults"></a>Microsoft.KeyVault/vaults

|Ölçüm|Metric Display Name|Birim|Aggregation Type|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|ServiceApiHit|Total Service Api Hits|Sayı|Sayı|Number of total service api hits|ActivityType, ActivityName|
|ServiceApiLatency|Overall Service Api Latency|Milliseconds|Ortalama|Overall latency of service api requests|ActivityType, ActivityName, StatusCode|
|ServiceApiResult|Total Service Api Results|Sayı|Sayı|Number of total service api results|ActivityType, ActivityName, StatusCode|

## <a name="microsoftkustoclusters"></a>Microsoft.Kusto/Clusters

|Ölçüm|Metric Display Name|Birim|Aggregation Type|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|CacheUtilization|Cache Utilization|Yüzde|Ortalama|Utilization level in the cluster scope|Hiçbiri|
|QueryDuration|Query Duration|Milliseconds|Ortalama|Queries’ duration in seconds|Query Status|
|IngestionUtilization|Ingestion Utilization|Yüzde|Ortalama|Ratio of used ingestion slots in the cluster|Hiçbiri|
|KeepAlive|Keep Alive|Sayı|Ortalama|Sanity check indicates the cluster responds to queries|Hiçbiri|
|IngestionVolumeInMB|Ingestion Volume (In MB)|Sayı|Toplam|Overall volume of ingested data to the cluster (in MB)|Database|
|IngestionLatencyInSeconds|Ingestion Latency (In seconds)|Saniye|Ortalama|Ingestion time from the source (e.g. message is in EventHub) to the cluster in seconds|Hiçbiri|
|EventProcessedForEventHubs|Events Processed (for Event Hubs)|Sayı|Toplam|Number of events processed by the cluster when ingesting from Event Hub|Hiçbiri|
|IngestionResult|Ingestion Result|Sayı|Sayı|Number of ingestion operations|Durum|
|CPU|CPU|Yüzde|Ortalama|CPU utilization level|Hiçbiri|
| ContinuousExportNumOfRecordsExported | Number of records exported in continuous export | Sayı | Toplam | Number of records exported for every storage artifact written during the export operation  | Hiçbiri |
| ExportUtilization | Export Utilization | Yüzde | Maksimum | Export utilization | Hiçbiri |
| ContinuousExportPendingCount | Continuous Export Pending Count | Sayı | Maksimum | The number of pending continuous export jobs ready for execution | Hiçbiri |
| ContinuousExportMaxLatenessMinutes | Continuous Export Max Lateness Minutes | Sayı | Maksimum | The max time in minutes of all continuous exports which are pending and ready for execution | Hiçbiri |

## <a name="microsoftlocationbasedservicesaccounts"></a>Microsoft.LocationBasedServices/accounts

|Ölçüm|Metric Display Name|Birim|Aggregation Type|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Kullanım|Kullanım|Sayı|Sayı|Count of API calls|ApiCategory, ApiName|

## <a name="microsoftlogicworkflows"></a>Microsoft.Logic/workflows

|Ölçüm|Metric Display Name|Birim|Aggregation Type|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|RunsStarted|Runs Started|Sayı|Toplam|Number of workflow runs started.|No Dimensions|
|RunsCompleted|Runs Completed|Sayı|Toplam|Number of workflow runs completed.|No Dimensions|
|RunsSucceeded|Runs Succeeded|Sayı|Toplam|Number of workflow runs succeeded.|No Dimensions|
|RunsFailed|Runs Failed|Sayı|Toplam|Number of workflow runs failed.|No Dimensions|
|RunsCancelled|Runs Canceled|Sayı|Toplam|Number of workflow runs canceled.|No Dimensions|
|RunLatency|Run Latency|Saniye|Ortalama|Latency of completed workflow runs.|No Dimensions|
|RunSuccessLatency|Run Success Latency|Saniye|Ortalama|Latency of succeeded workflow runs.|No Dimensions|
|RunThrottledEvents|Run Throttled Events|Sayı|Toplam|Number of workflow action or trigger throttled events.|No Dimensions|
|RunFailurePercentage|Run Failure Percentage|Yüzde|Toplam|Percentage of workflow runs failed.|No Dimensions|
|ActionsStarted|Actions Started |Sayı|Toplam|Number of workflow actions started.|No Dimensions|
|ActionsCompleted|Actions Completed |Sayı|Toplam|Number of workflow actions completed.|No Dimensions|
|ActionsSucceeded|Actions Succeeded |Sayı|Toplam|Number of workflow actions succeeded.|No Dimensions|
|ActionsFailed|Actions Failed|Sayı|Toplam|Number of workflow actions failed.|No Dimensions|
|ActionsSkipped|Actions Skipped |Sayı|Toplam|Number of workflow actions skipped.|No Dimensions|
|ActionLatency|Action Latency |Saniye|Ortalama|Latency of completed workflow actions.|No Dimensions|
|ActionSuccessLatency|Action Success Latency |Saniye|Ortalama|Latency of succeeded workflow actions.|No Dimensions|
|ActionThrottledEvents|Action Throttled Events|Sayı|Toplam|Number of workflow action throttled events..|No Dimensions|
|TriggersStarted|Triggers Started |Sayı|Toplam|Number of workflow triggers started.|No Dimensions|
|TriggersCompleted|Triggers Completed |Sayı|Toplam|Number of workflow triggers completed.|No Dimensions|
|TriggersSucceeded|Triggers Succeeded |Sayı|Toplam|Number of workflow triggers succeeded.|No Dimensions|
|TriggersFailed|Triggers Failed |Sayı|Toplam|Number of workflow triggers failed.|No Dimensions|
|TriggersSkipped|Triggers Skipped|Sayı|Toplam|Number of workflow triggers skipped.|No Dimensions|
|TriggersFired|Triggers Fired |Sayı|Toplam|Number of workflow triggers fired.|No Dimensions|
|TriggerLatency|Trigger Latency |Saniye|Ortalama|Latency of completed workflow triggers.|No Dimensions|
|TriggerFireLatency|Trigger Fire Latency |Saniye|Ortalama|Latency of fired workflow triggers.|No Dimensions|
|TriggerSuccessLatency|Trigger Success Latency |Saniye|Ortalama|Latency of succeeded workflow triggers.|No Dimensions|
|TriggerThrottledEvents|Trigger Throttled Events|Sayı|Toplam|Number of workflow trigger throttled events.|No Dimensions|
|BillableActionExecutions|Billable Action Executions|Sayı|Toplam|Number of workflow action executions getting billed.|No Dimensions|
|BillableTriggerExecutions|Billable Trigger Executions|Sayı|Toplam|Number of workflow trigger executions getting billed.|No Dimensions|
|TotalBillableExecutions|Total Billable Executions|Sayı|Toplam|Number of workflow executions getting billed.|No Dimensions|
|BillingUsageNativeOperation|Billing Usage for Native Operation Executions|Sayı|Toplam|Number of native operation executions getting billed.|No Dimensions|
|BillingUsageStandardConnector|Billing Usage for Standard Connector Executions|Sayı|Toplam|Number of standard connector executions getting billed.|No Dimensions|
|BillingUsageStorageConsumption|Billing Usage for Storage Consumption Executions|Sayı|Toplam|Number of storage consumption executions getting billed.|No Dimensions|
|BillingUsageNativeOperation|Billing Usage for Native Operation Executions|Sayı|Toplam|Number of native operation executions getting billed.|No Dimensions|
|BillingUsageStandardConnector|Billing Usage for Standard Connector Executions|Sayı|Toplam|Number of standard connector executions getting billed.|No Dimensions|
|BillingUsageStorageConsumption|Billing Usage for Storage Consumption Executions|Sayı|Toplam|Number of storage consumption executions getting billed.|No Dimensions|

## <a name="microsoftlogicintegrationserviceenvironments"></a>Microsoft.Logic/integrationServiceEnvironments

|Ölçüm|Metric Display Name|Birim|Aggregation Type|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|RunsStarted|Runs Started|Sayı|Toplam|Number of workflow runs started.|No Dimensions|
|RunsCompleted|Runs Completed|Sayı|Toplam|Number of workflow runs completed.|No Dimensions|
|RunsSucceeded|Runs Succeeded|Sayı|Toplam|Number of workflow runs succeeded.|No Dimensions|
|RunsFailed|Runs Failed|Sayı|Toplam|Number of workflow runs failed.|No Dimensions|
|RunsCancelled|Runs Canceled|Sayı|Toplam|Number of workflow runs canceled.|No Dimensions|
|RunLatency|Run Latency|Saniye|Ortalama|Latency of completed workflow runs.|No Dimensions|
|RunSuccessLatency|Run Success Latency|Saniye|Ortalama|Latency of succeeded workflow runs.|No Dimensions|
|RunThrottledEvents|Run Throttled Events|Sayı|Toplam|Number of workflow action or trigger throttled events.|No Dimensions|
|RunStartThrottledEvents|Run Start Throttled Events|Sayı|Toplam|Number of workflow run start throttled events.|No Dimensions|
|RunFailurePercentage|Run Failure Percentage|Yüzde|Toplam|Percentage of workflow runs failed.|No Dimensions|
|ActionsStarted|Actions Started |Sayı|Toplam|Number of workflow actions started.|No Dimensions|
|ActionsCompleted|Actions Completed |Sayı|Toplam|Number of workflow actions completed.|No Dimensions|
|ActionsSucceeded|Actions Succeeded |Sayı|Toplam|Number of workflow actions succeeded.|No Dimensions|
|ActionsFailed|Actions Failed |Sayı|Toplam|Number of workflow actions failed.|No Dimensions|
|ActionsSkipped|Actions Skipped |Sayı|Toplam|Number of workflow actions skipped.|No Dimensions|
|ActionLatency|Action Latency |Saniye|Ortalama|Latency of completed workflow actions.|No Dimensions|
|ActionSuccessLatency|Action Success Latency |Saniye|Ortalama|Latency of succeeded workflow actions.|No Dimensions|
|ActionThrottledEvents|Action Throttled Events|Sayı|Toplam|Number of workflow action throttled events..|No Dimensions|
|TriggersStarted|Triggers Started |Sayı|Toplam|Number of workflow triggers started.|No Dimensions|
|TriggersCompleted|Triggers Completed |Sayı|Toplam|Number of workflow triggers completed.|No Dimensions|
|TriggersSucceeded|Triggers Succeeded |Sayı|Toplam|Number of workflow triggers succeeded.|No Dimensions|
|TriggersFailed|Triggers Failed |Sayı|Toplam|Number of workflow triggers failed.|No Dimensions|
|TriggersSkipped|Triggers Skipped|Sayı|Toplam|Number of workflow triggers skipped.|No Dimensions|
|TriggersFired|Triggers Fired |Sayı|Toplam|Number of workflow triggers fired.|No Dimensions|
|TriggerLatency|Trigger Latency |Saniye|Ortalama|Latency of completed workflow triggers.|No Dimensions|
|TriggerFireLatency|Trigger Fire Latency |Saniye|Ortalama|Latency of fired workflow triggers.|No Dimensions|
|TriggerSuccessLatency|Trigger Success Latency |Saniye|Ortalama|Latency of succeeded workflow triggers.|No Dimensions|
|TriggerThrottledEvents|Trigger Throttled Events|Sayı|Toplam|Number of workflow trigger throttled events.|No Dimensions|
|IntegrationServiceEnvironmentWorkflowProcessorUsage|Workflow Processor Usage for Integration Service Environment|Yüzde|Ortalama|Workflow processor usage for integration service environment.|No Dimensions|
|IntegrationServiceEnvironmentWorkflowMemoryUsage|Workflow Memory Usage for Integration Service Environment|Yüzde|Ortalama|Workflow memory usage for integration service environment.|No Dimensions|
|IntegrationServiceEnvironmentConnectorProcessorUsage|Connector Processor Usage for Integration Service Environment|Yüzde|Ortalama|Connector processor usage for integration service environment.|No Dimensions|
|IntegrationServiceEnvironmentConnectorMemoryUsage|Connector Memory Usage for Integration Service Environment|Yüzde|Ortalama|Connector memory usage for integration service environment.|No Dimensions|

## <a name="microsoftmachinelearningservicesworkspaces"></a>Microsoft.MachineLearningServices/workspaces

|Ölçüm|Metric Display Name|Birim|Aggregation Type|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Completed Runs|Completed Runs|Sayı|Toplam|Number of runs completed successfully for this workspace|Senaryo|
|Started Runs|Started Runs|Sayı|Toplam|Number of runs started for this workspace|Senaryo|
|Failed Runs|Failed Runs|Sayı|Toplam|Number of runs failed for this workspace|Senaryo|

## <a name="microsoftmapsaccounts"></a>Microsoft.Maps/accounts

|Ölçüm|Metric Display Name|Birim|Aggregation Type|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Kullanım|Kullanım|Sayı|Sayı|Count of API calls|ApiCategory, ApiName, ResultType, ResponseCode|
|Erişilebilirlik|Erişilebilirlik|Yüzde|Ortalama|Availability of the APIs|ApiCategory, ApiName|

## <a name="microsoftnetappnetappaccountscapacitypoolsvolumes"></a>Microsoft.NetApp/netAppAccounts/capacityPools/Volumes

|Ölçüm|Metric Display Name|Birim|Aggregation Type|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|AverageOtherLatency|Average other latency|ms/op|Ortalama|Average other latency (that is not read or write) in milliseconds per operation|No Dimensions|
|AverageReadLatency|Average read latency|ms/op|Ortalama|Average read latency in milliseconds per operation|No Dimensions|
|AverageTotalLatency|Average total latency|ms/op|Ortalama|Average total latency in milliseconds per operation|No Dimensions|
|AverageWriteLatency|Average write latency|ms/op|Ortalama|Average write latency in milliseconds per operation|No Dimensions|
|FilesystemOtherOps|Filesystem other ops|ops|Ortalama|Number of filesystem other operations (that is not read or write)|No Dimensions|
|FilesystemReadOps|Filesystem read ops|ops|Ortalama|Number of filesystem read operations|No Dimensions|
|FilesystemTotalOps|Filesystem total ops|ops|Ortalama|Sum of all filesystem operations|No Dimensions|
|FilesystemWriteOps|Filesystem write ops|ops|Ortalama|Number of filesystem write operations|No Dimensions|
|IoBytesPerOtherOps|Io bytes per other ops|bytes/op|Ortalama|Number of In/out bytes per other operations (that is not read or write)|No Dimensions|
|IoBytesPerReadOps|Io bytes per read ops|bytes/op|Ortalama|Number of In/out bytes per read operation|No Dimensions|
|IoBytesPerTotalOps|Io bytes per op across all operations|bytes/op|Ortalama|Sum of all In/out bytes operation|No Dimensions|
|IoBytesPerWriteOps|Io bytes per write ops|bytes/op|Ortalama|Number of In/out bytes per write operation|No Dimensions|
|OtherIops|Other iops|operations/second|Ortalama|Other In/out operation per second|No Dimensions|
|OtherThroughput|Other throughput|MBps|Ortalama|Other throughput (that is not read or write) in megabytes per second|No Dimensions|
|ReadIops|Read iops|operations/second|Ortalama|Read In/out operations per second|No Dimensions|
|ReadThroughput|Read throughput|MBps|Ortalama|Read throughput in megabytes per second|No Dimensions|
|TotalIops|Total iops|operations/second|Ortalama|Sum of all In/out operations per second|No Dimensions|
|TotalThroughput|Total throughput|MBps|Ortalama|Sum of all throughput in megabytes per second|No Dimensions|
|VolumeAllocatedSize|Volume allocated size|bytes|Ortalama|Allocated size of the volume (Not the actual used bytes)|No Dimensions|
|VolumeLogicalSize|Volume logical size|bytes|Ortalama|Logical size of the volume (used bytes)|No Dimensions|
|VolumeSnapshotSize|Volume snapshot size|bytes|Ortalama|Size of all snapshots in volume|No Dimensions|
|WriteIops|Write iops|operations/second|Ortalama|Write In/out operations per second|No Dimensions|
|WriteThroughput|Write throughput|MBps|Ortalama|Write throughput in megabytes per second|No Dimensions|

## <a name="microsoftnetappnetappaccountscapacitypools"></a>Microsoft.NetApp/netAppAccounts/capacityPools

|Ölçüm|Metric Display Name|Birim|Aggregation Type|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|VolumePoolAllocatedSize|Volume pool allocated size|bytes|Ortalama|Allocated size of the pool (Not the actual used bytes)|No Dimensions|
|VolumePoolAllocatedUsed|Volume pool allocated used|bytes|Ortalama|Allocated used size of the pool|No Dimensions|
|VolumePoolTotalLogicalSize|Volume pool total logical size|bytes|Ortalama|Sum of the logical size of all the volumes belonging to the pool|No Dimensions|
|VolumePoolTotalSnapshotSize|Volume pool total snapshot size|bytes|Ortalama|Sum of all snapshots in pool|No Dimensions|

## <a name="microsoftnetworknetworkinterfaces"></a>Microsoft.Network/networkInterfaces

|Ölçüm|Metric Display Name|Birim|Aggregation Type|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|BytesSentRate|Bytes Sent|Sayı|Toplam|Number of bytes the Network Interface sent|No Dimensions|
|BytesReceivedRate|Bytes Received|Sayı|Toplam|Number of bytes the Network Interface received|No Dimensions|
|PacketsSentRate|Packets Sent|Sayı|Toplam|Number of packets the Network Interface sent|No Dimensions|
|PacketsReceivedRate|Packets Received|Sayı|Toplam|Number of packets the Network Interface received|No Dimensions|

## <a name="microsoftnetworkloadbalancers"></a>Microsoft.Network/loadBalancers

|Ölçüm|Metric Display Name|Birim|Aggregation Type|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|VipAvailability|Data Path Availability|Sayı|Ortalama|Average Load Balancer data path availability per time duration|FrontendIPAddress, FrontendPort|
|DipAvailability|Health Probe Status|Sayı|Ortalama|Average Load Balancer health probe status per time duration|ProtocolType, BackendPort, FrontendIPAddress, FrontendPort, BackendIPAddress|
|ByteCount|Byte Count|Sayı|Toplam|Total number of Bytes transmitted within time period|FrontendIPAddress, FrontendPort, Direction|
|PacketCount|Packet Count|Sayı|Toplam|Total number of Packets transmitted within time period|FrontendIPAddress, FrontendPort, Direction|
|SYNCount|SYN Count|Sayı|Toplam|Total number of SYN Packets transmitted within time period|FrontendIPAddress, FrontendPort, Direction|
|SnatConnectionCount|SNAT Connection Count|Sayı|Toplam|Total number of new SNAT connections created within time period|FrontendIPAddress, BackendIPAddress, ConnectionState|
|AllocatedSnatPorts|Allocated SNAT Ports (Preview)|Sayı|Toplam|Total number of SNAT ports allocated within time period|FrontendIPAddress, BackendIPAddress, ProtocolType|
|UsedSnatPorts|Used SNAT Ports (Preview)|Sayı|Toplam|Total number of SNAT ports used within time period|FrontendIPAddress, BackendIPAddress, ProtocolType|

## <a name="microsoftnetworkdnszones"></a>Microsoft.Network/dnszones

|Ölçüm|Metric Display Name|Birim|Aggregation Type|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|QueryVolume|Query Volume|Sayı|Toplam|Number of queries served for a DNS zone|No Dimensions|
|RecordSetCount|Record Set Count|Sayı|Maksimum|Number of Record Sets in a DNS zone|No Dimensions|
|RecordSetCapacityUtilization|Record Set Capacity Utilization|Yüzde|Maksimum|Percent of Record Set capacity utilized by a DNS zone|No Dimensions|

## <a name="microsoftnetworkpublicipaddresses"></a>Microsoft.Network/publicIPAddresses

|Ölçüm|Metric Display Name|Birim|Aggregation Type|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|PacketsInDDoS|Inbound packets DDoS|CountPerSecond|Maksimum|Inbound packets DDoS|No Dimensions|
|PacketsDroppedDDoS|Inbound packets dropped DDoS|CountPerSecond|Maksimum|Inbound packets dropped DDoS|No Dimensions|
|PacketsForwardedDDoS|Inbound packets forwarded DDoS|CountPerSecond|Maksimum|Inbound packets forwarded DDoS|No Dimensions|
|TCPPacketsInDDoS|Inbound TCP packets DDoS|CountPerSecond|Maksimum|Inbound TCP packets DDoS|No Dimensions|
|TCPPacketsDroppedDDoS|Inbound TCP packets dropped DDoS|CountPerSecond|Maksimum|Inbound TCP packets dropped DDoS|No Dimensions|
|TCPPacketsForwardedDDoS|Inbound TCP packets forwarded DDoS|CountPerSecond|Maksimum|Inbound TCP packets forwarded DDoS|No Dimensions|
|UDPPacketsInDDoS|Inbound UDP packets DDoS|CountPerSecond|Maksimum|Inbound UDP packets DDoS|No Dimensions|
|UDPPacketsDroppedDDoS|Inbound UDP packets dropped DDoS|CountPerSecond|Maksimum|Inbound UDP packets dropped DDoS|No Dimensions|
|UDPPacketsForwardedDDoS|Inbound UDP packets forwarded DDoS|CountPerSecond|Maksimum|Inbound UDP packets forwarded DDoS|No Dimensions|
|BytesInDDoS|Inbound bytes DDoS|BytesPerSecond|Maksimum|Inbound bytes DDoS|No Dimensions|
|BytesDroppedDDoS|Inbound bytes dropped DDoS|BytesPerSecond|Maksimum|Inbound bytes dropped DDoS|No Dimensions|
|BytesForwardedDDoS|Inbound bytes forwarded DDoS|BytesPerSecond|Maksimum|Inbound bytes forwarded DDoS|No Dimensions|
|TCPBytesInDDoS|Inbound TCP bytes DDoS|BytesPerSecond|Maksimum|Inbound TCP bytes DDoS|No Dimensions|
|TCPBytesDroppedDDoS|Inbound TCP bytes dropped DDoS|BytesPerSecond|Maksimum|Inbound TCP bytes dropped DDoS|No Dimensions|
|TCPBytesForwardedDDoS|Inbound TCP bytes forwarded DDoS|BytesPerSecond|Maksimum|Inbound TCP bytes forwarded DDoS|No Dimensions|
|UDPBytesInDDoS|Inbound UDP bytes DDoS|BytesPerSecond|Maksimum|Inbound UDP bytes DDoS|No Dimensions|
|UDPBytesDroppedDDoS|Inbound UDP bytes dropped DDoS|BytesPerSecond|Maksimum|Inbound UDP bytes dropped DDoS|No Dimensions|
|UDPBytesForwardedDDoS|Inbound UDP bytes forwarded DDoS|BytesPerSecond|Maksimum|Inbound UDP bytes forwarded DDoS|No Dimensions|
|IfUnderDDoSAttack|Under DDoS attack or not|Sayı|Maksimum|Under DDoS attack or not|No Dimensions|
|DDoSTriggerTCPPackets|Inbound TCP packets to trigger DDoS mitigation|CountPerSecond|Maksimum|Inbound TCP packets to trigger DDoS mitigation|No Dimensions|
|DDoSTriggerUDPPackets|Inbound UDP packets to trigger DDoS mitigation|CountPerSecond|Maksimum|Inbound UDP packets to trigger DDoS mitigation|No Dimensions|
|DDoSTriggerSYNPackets|Inbound SYN packets to trigger DDoS mitigation|CountPerSecond|Maksimum|Inbound SYN packets to trigger DDoS mitigation|No Dimensions|
|VipAvailability|Data Path Availability|Sayı|Ortalama|Average IP Address availability per time duration|Bağlantı noktası|
|ByteCount|Byte Count|Sayı|Toplam|Total number of Bytes transmitted within time period|Port, Direction|
|PacketCount|Packet Count|Sayı|Toplam|Total number of Packets transmitted within time period|Port, Direction|
|SynCount|SYN Count|Sayı|Toplam|Total number of SYN Packets transmitted within time period|Port, Direction|

## <a name="microsoftnetworkazurefirewalls"></a>Microsoft.Network/azurefirewalls

|Ölçüm|Metric Display Name|Birim|Aggregation Type|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|ApplicationRuleHit|Application rules hit count|Sayı|Toplam|Number of times Application rules were hit|Status, Reason, Protocol|
|NetworkRuleHit|Network rules hit count|Sayı|Toplam|Number of times Network rules were hit|Status, Reason, Protocol|

## <a name="microsoftnetworkapplicationgateways"></a>Microsoft.Network/applicationGateways

|Ölçüm|Metric Display Name|Birim|Aggregation Type|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|İşleme|İşleme|BytesPerSecond|Toplam|Number of bytes per second the Application Gateway has served|No Dimensions|
|UnhealthyHostCount|Unhealthy Host Count|Sayı|Ortalama|Number of unhealthy backend hosts|BackendSettingsPool|
|HealthyHostCount|Healthy Host Count|Sayı|Ortalama|Number of healthy backend hosts|BackendSettingsPool|
|TotalRequests|Toplam İstek Sayısı|Sayı|Toplam|Count of successful requests that Application Gateway has served|BackendSettingsPool|
|FailedRequests|Başarısız İstekler|Sayı|Toplam|Count of failed requests that Application Gateway has served|BackendSettingsPool|
|ResponseStatus|Response Status|Sayı|Toplam|Http response status returned by Application Gateway|HttpStatusGroup|
|CurrentConnections|Current Connections|Sayı|Toplam|Count of current connections established with Application Gateway|No Dimensions|
|CapacityUnits|Current Capacity Units|Sayı|Ortalama|Capacity Units consumed|No Dimensions|

## <a name="microsoftnetworkvirtualnetworkgateways"></a>Microsoft.Network/virtualNetworkGateways

|Ölçüm|Metric Display Name|Birim|Aggregation Type|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|AverageBandwidth|Gateway S2S Bandwidth|BytesPerSecond|Ortalama|Average site-to-site bandwidth of a gateway in bytes per second|No Dimensions|
|P2SBandwidth|Gateway P2S Bandwidth|BytesPerSecond|Ortalama|Average point-to-site bandwidth of a gateway in bytes per second|No Dimensions|
|P2SConnectionCount|P2S Connection Count|Sayı|Maksimum|Point-to-site connection count of a gateway|Protokol|
|TunnelAverageBandwidth|Tunnel Bandwidth|BytesPerSecond|Ortalama|Average bandwidth of a tunnel in bytes per second|ConnectionName, RemoteIP|
|TunnelEgressBytes|Tunnel Egress Bytes|Bytes|Toplam|Outgoing bytes of a tunnel|ConnectionName, RemoteIP|
|TunnelIngressBytes|Tunnel Ingress Bytes|Bytes|Toplam|Incoming bytes of a tunnel|ConnectionName, RemoteIP|
|TunnelEgressPackets|Tunnel Egress Packets|Sayı|Toplam|Outgoing packet count of a tunnel|ConnectionName, RemoteIP|
|TunnelIngressPackets|Tunnel Ingress Packets|Sayı|Toplam|Incoming packet count of a tunnel|ConnectionName, RemoteIP|
|TunnelEgressPacketDropTSMismatch|Tunnel Egress TS Mismatch Packet Drop|Sayı|Toplam|Outgoing packet drop count from traffic selector mismatch of a tunnel|ConnectionName, RemoteIP|
|TunnelIngressPacketDropTSMismatch|Tunnel Ingress TS Mismatch Packet Drop|Sayı|Toplam|Incoming packet drop count from traffic selector mismatch of a tunnel|ConnectionName, RemoteIP|

## <a name="microsoftnetworkexpressroutecircuits"></a>Microsoft.Network/expressRouteCircuits

|Ölçüm|Metric Display Name|Birim|Aggregation Type|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|BitsInPerSecond|BitsInPerSecond|CountPerSecond|Ortalama|Bits ingressing Azure per second|PeeringType|
|BitsOutPerSecond|BitsOutPerSecond|CountPerSecond|Ortalama|Bits egressing Azure per second|PeeringType|

## <a name="microsoftnetworkexpressroutecircuitspeerings"></a>Microsoft.Network/expressRouteCircuits/peerings

|Ölçüm|Metric Display Name|Birim|Aggregation Type|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|BitsInPerSecond|BitsInPerSecond|CountPerSecond|Ortalama|Bits ingressing Azure per second|No Dimensions|
|BitsOutPerSecond|BitsOutPerSecond|CountPerSecond|Ortalama|Bits egressing Azure per second|No Dimensions|

## <a name="microsoftnetworkconnections"></a>Microsoft.Network/connections

|Ölçüm|Metric Display Name|Birim|Aggregation Type|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|BitsInPerSecond|BitsInPerSecond|CountPerSecond|Ortalama|Bits ingressing Azure per second|No Dimensions|
|BitsOutPerSecond|BitsOutPerSecond|CountPerSecond|Ortalama|Bits egressing Azure per second|No Dimensions|

## <a name="microsoftnetworktrafficmanagerprofiles"></a>Microsoft.Network/trafficManagerProfiles

|Ölçüm|Metric Display Name|Birim|Aggregation Type|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|QpsByEndpoint|Queries by Endpoint Returned|Sayı|Toplam|Number of times a Traffic Manager endpoint was returned in the given time frame|EndpointName|
|ProbeAgentCurrentEndpointStateByProfileResourceId|Endpoint Status by Endpoint|Sayı|Maksimum|1 if an endpoint's probe status is "Enabled", 0 otherwise.|EndpointName|

## <a name="microsoftnetworknetworkwatchersconnectionmonitors"></a>Microsoft.Network/networkWatchers/connectionMonitors

|Ölçüm|Metric Display Name|Birim|Aggregation Type|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|ProbesFailedPercent|% Probes Failed|Yüzde|Ortalama|% of connectivity monitoring probes failed|No Dimensions|
|AverageRoundtripMs|Avg. Round-trip Time (ms)|MilliSeconds|Ortalama|Average network round-trip time (ms) for connectivity monitoring probes sent between source and destination|No Dimensions|

## <a name="microsoftnetworkfrontdoors"></a>Microsoft.Network/frontdoors

|Ölçüm|Metric Display Name|Birim|Aggregation Type|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|RequestCount|İstek Sayısı|Sayı|Toplam|The number of client requests served by the HTTP/S proxy|HttpStatus, HttpStatusGroup, ClientRegion, ClientCountry|
|RequestSize|Request Size|Bytes|Toplam|The number of bytes sent as requests from clients to the HTTP/S proxy|HttpStatus, HttpStatusGroup, ClientRegion, ClientCountry|
|ResponseSize|Response Size|Bytes|Toplam|The number of bytes sent as responses from HTTP/S proxy to clients|HttpStatus, HttpStatusGroup, ClientRegion, ClientCountry|
|BackendRequestCount|Backend Request Count|Sayı|Toplam|The number of requests sent from the HTTP/S proxy to backends|HttpStatus, HttpStatusGroup, Backend|
|BackendRequestLatency|Backend Request Latency|MilliSeconds|Ortalama|The time calculated from when the request was sent by the HTTP/S proxy to the backend until the HTTP/S proxy received the last response byte from the backend|Backend|
|TotalLatency|Total Latency|MilliSeconds|Ortalama|The time calculated from when the client request was received by the HTTP/S proxy until the client acknowledged the last response byte from the HTTP/S proxy|HttpStatus, HttpStatusGroup, ClientRegion, ClientCountry|
|BackendHealthPercentage|Backend Health Percentage|Yüzde|Ortalama|The percentage of successful health probes from the HTTP/S proxy to backends|Backend, BackendPool|
|WebApplicationFirewallRequestCount|Web Application Firewall Request Count|Sayı|Toplam|The number of client requests processed by the Web Application Firewall|PolicyName, RuleName, Action|

## <a name="microsoftnotificationhubsnamespacesnotificationhubs"></a>Microsoft.NotificationHubs/Namespaces/NotificationHubs

|Ölçüm|Metric Display Name|Birim|Aggregation Type|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|registration.all|Registration Operations|Sayı|Toplam|The count of all successful registration operations (creations updates queries and deletions). |No Dimensions|
|registration.create|Registration Create Operations|Sayı|Toplam|The count of all successful registration creations.|No Dimensions|
|registration.update|Registration Update Operations|Sayı|Toplam|The count of all successful registration updates.|No Dimensions|
|registration.get|Registration Read Operations|Sayı|Toplam|The count of all successful registration queries.|No Dimensions|
|registration.delete|Registration Delete Operations|Sayı|Toplam|The count of all successful registration deletions.|No Dimensions|
|incoming|Incoming Messages|Sayı|Toplam|The count of all successful send API calls. |No Dimensions|
|incoming.scheduled|Scheduled Push Notifications Sent|Sayı|Toplam|Scheduled Push Notifications Canceled|No Dimensions|
|incoming.scheduled.cancel|Scheduled Push Notifications Canceled|Sayı|Toplam|Scheduled Push Notifications Canceled|No Dimensions|
|scheduled.pending|Pending Scheduled Notifications|Sayı|Toplam|Pending Scheduled Notifications|No Dimensions|
|installation.all|Installation Management Operations|Sayı|Toplam|Installation Management Operations|No Dimensions|
|installation.get|Get Installation Operations|Sayı|Toplam|Get Installation Operations|No Dimensions|
|installation.upsert|Create or Update Installation Operations|Sayı|Toplam|Create or Update Installation Operations|No Dimensions|
|installation.patch|Patch Installation Operations|Sayı|Toplam|Patch Installation Operations|No Dimensions|
|installation.delete|Delete Installation Operations|Sayı|Toplam|Delete Installation Operations|No Dimensions|
|outgoing.allpns.success|Successful notifications|Sayı|Toplam|The count of all successful notifications.|No Dimensions|
|outgoing.allpns.invalidpayload|Payload Errors|Sayı|Toplam|The count of pushes that failed because the PNS returned a bad payload error.|No Dimensions|
|outgoing.allpns.pnserror|External Notification System Errors|Sayı|Toplam|The count of pushes that failed because there was a problem communicating with the PNS (excludes authentication problems).|No Dimensions|
|outgoing.allpns.channelerror|Channel Errors|Sayı|Toplam|The count of pushes that failed because the channel was invalid not associated with the correct app throttled or expired.|No Dimensions|
|outgoing.allpns.badorexpiredchannel|Bad or Expired Channel Errors|Sayı|Toplam|The count of pushes that failed because the channel/token/registrationId in the registration was expired or invalid.|No Dimensions|
|outgoing.wns.success|WNS Successful Notifications|Sayı|Toplam|The count of all successful notifications.|No Dimensions|
|outgoing.wns.invalidcredentials|WNS Authorization Errors (Invalid Credentials)|Sayı|Toplam|The count of pushes that failed because the PNS did not accept the provided credentials or the credentials are blocked. (Windows Live does not recognize the credentials).|No Dimensions|
|outgoing.wns.badchannel|WNS Bad Channel Error|Sayı|Toplam|The count of pushes that failed because the ChannelURI in the registration was not recognized (WNS status: 404 not found).|No Dimensions|
|outgoing.wns.expiredchannel|WNS Expired Channel Error|Sayı|Toplam|The count of pushes that failed because the ChannelURI is expired (WNS status: 410 Gone).|No Dimensions|
|outgoing.wns.throttled|WNS Throttled Notifications|Sayı|Toplam|The count of pushes that failed because WNS is throttling this app (WNS status: 406 Not Acceptable).|No Dimensions|
|outgoing.wns.tokenproviderunreachable|WNS Authorization Errors (Unreachable)|Sayı|Toplam|Windows Live is not reachable.|No Dimensions|
|outgoing.wns.invalidtoken|WNS Authorization Errors (Invalid Token)|Sayı|Toplam|The token provided to WNS is not valid (WNS status: 401 Unauthorized).|No Dimensions|
|outgoing.wns.wrongtoken|WNS Authorization Errors (Wrong Token)|Sayı|Toplam|The token provided to WNS is valid but for another application (WNS status: 403 Forbidden). This can happen if the ChannelURI in the registration is associated with another app. Check that the client app is associated with the same app whose credentials are in the notification hub.|No Dimensions|
|outgoing.wns.invalidnotificationformat|WNS Invalid Notification Format|Sayı|Toplam|The format of the notification is invalid (WNS status: 400). Note that WNS does not reject all invalid payloads.|No Dimensions|
|outgoing.wns.invalidnotificationsize|WNS Invalid Notification Size Error|Sayı|Toplam|The notification payload is too large (WNS status: 413).|No Dimensions|
|outgoing.wns.channelthrottled|WNS Channel Throttled|Sayı|Toplam|The notification was dropped because the ChannelURI in the registration is throttled (WNS response header: X-WNS-NotificationStatus:channelThrottled).|No Dimensions|
|outgoing.wns.channeldisconnected|WNS Channel Disconnected|Sayı|Toplam|The notification was dropped because the ChannelURI in the registration is throttled (WNS response header: X-WNS-DeviceConnectionStatus: disconnected).|No Dimensions|
|outgoing.wns.dropped|WNS Dropped Notifications|Sayı|Toplam|The notification was dropped because the ChannelURI in the registration is throttled (X-WNS-NotificationStatus: dropped but not X-WNS-DeviceConnectionStatus: disconnected).|No Dimensions|
|outgoing.wns.pnserror|WNS Errors|Sayı|Toplam|Notification not delivered because of errors communicating with WNS.|No Dimensions|
|outgoing.wns.authenticationerror|WNS Authentication Errors|Sayı|Toplam|Notification not delivered because of errors communicating with Windows Live invalid credentials or wrong token.|No Dimensions|
|outgoing.apns.success|APNS Successful Notifications|Sayı|Toplam|The count of all successful notifications.|No Dimensions|
|outgoing.apns.invalidcredentials|APNS Authorization Errors|Sayı|Toplam|The count of pushes that failed because the PNS did not accept the provided credentials or the credentials are blocked.|No Dimensions|
|outgoing.apns.badchannel|APNS Bad Channel Error|Sayı|Toplam|The count of pushes that failed because the token is invalid (APNS binary protocol status code: 8. APNS HTTP protocol status code: 400 with "BadDeviceToken").|No Dimensions|
|outgoing.apns.expiredchannel|APNS Expired Channel Error|Sayı|Toplam|The count of token that were invalidated by the APNS feedback channel.|No Dimensions|
|outgoing.apns.invalidnotificationsize|APNS Invalid Notification Size Error|Sayı|Toplam|The count of pushes that failed because the payload was too large (APNS binary protocol status code: 7).|No Dimensions|
|outgoing.apns.pnserror|APNS Errors|Sayı|Toplam|The count of pushes that failed because of errors communicating with APNS.|No Dimensions|
|outgoing.gcm.success|GCM Successful Notifications|Sayı|Toplam|The count of all successful notifications.|No Dimensions|
|outgoing.gcm.invalidcredentials|GCM Authorization Errors (Invalid Credentials)|Sayı|Toplam|The count of pushes that failed because the PNS did not accept the provided credentials or the credentials are blocked.|No Dimensions|
|outgoing.gcm.badchannel|GCM Bad Channel Error|Sayı|Toplam|The count of pushes that failed because the registrationId in the registration was not recognized (GCM result: Invalid Registration).|No Dimensions|
|outgoing.gcm.expiredchannel|GCM Expired Channel Error|Sayı|Toplam|The count of pushes that failed because the registrationId in the registration was expired (GCM result: NotRegistered).|No Dimensions|
|outgoing.gcm.throttled|GCM Throttled Notifications|Sayı|Toplam|The count of pushes that failed because GCM throttled this app (GCM status code: 501-599 or result:Unavailable).|No Dimensions|
|outgoing.gcm.invalidnotificationformat|GCM Invalid Notification Format|Sayı|Toplam|The count of pushes that failed because the payload was not formatted correctly (GCM result: InvalidDataKey or InvalidTtl).|No Dimensions|
|outgoing.gcm.invalidnotificationsize|GCM Invalid Notification Size Error|Sayı|Toplam|The count of pushes that failed because the payload was too large (GCM result: MessageTooBig).|No Dimensions|
|outgoing.gcm.wrongchannel|GCM Wrong Channel Error|Sayı|Toplam|The count of pushes that failed because the registrationId in the registration is not associated to the current app (GCM result: InvalidPackageName).|No Dimensions|
|outgoing.gcm.pnserror|GCM Errors|Sayı|Toplam|The count of pushes that failed because of errors communicating with GCM.|No Dimensions|
|outgoing.gcm.authenticationerror|GCM Authentication Errors|Sayı|Toplam|The count of pushes that failed because the PNS did not accept the provided credentials the credentials are blocked or the SenderId is not correctly configured in the app (GCM result: MismatchedSenderId).|No Dimensions|
|outgoing.mpns.success|MPNS Successful Notifications|Sayı|Toplam|The count of all successful notifications.|No Dimensions|
|outgoing.mpns.invalidcredentials|MPNS Invalid Credentials|Sayı|Toplam|The count of pushes that failed because the PNS did not accept the provided credentials or the credentials are blocked.|No Dimensions|
|outgoing.mpns.badchannel|MPNS Bad Channel Error|Sayı|Toplam|The count of pushes that failed because the ChannelURI in the registration was not recognized (MPNS status: 404 not found).|No Dimensions|
|outgoing.mpns.throttled|MPNS Throttled Notifications|Sayı|Toplam|The count of pushes that failed because MPNS is throttling this app (WNS MPNS: 406 Not Acceptable).|No Dimensions|
|outgoing.mpns.invalidnotificationformat|MPNS Invalid Notification Format|Sayı|Toplam|The count of pushes that failed because the payload of the notification was too large.|No Dimensions|
|outgoing.mpns.channeldisconnected|MPNS Channel Disconnected|Sayı|Toplam|The count of pushes that failed because the ChannelURI in the registration was disconnected (MPNS status: 412 not found).|No Dimensions|
|outgoing.mpns.dropped|MPNS Dropped Notifications|Sayı|Toplam|The count of pushes that were dropped by MPNS (MPNS response header: X-NotificationStatus: QueueFull or Suppressed).|No Dimensions|
|outgoing.mpns.pnserror|MPNS Errors|Sayı|Toplam|The count of pushes that failed because of errors communicating with MPNS.|No Dimensions|
|outgoing.mpns.authenticationerror|MPNS Authentication Errors|Sayı|Toplam|The count of pushes that failed because the PNS did not accept the provided credentials or the credentials are blocked.|No Dimensions|
|notificationhub.pushes|All Outgoing Notifications|Sayı|Toplam|All outgoing notifications of the notification hub|No Dimensions|
|incoming.all.requests|All Incoming Requests|Sayı|Toplam|Total incoming requests for a notification hub|No Dimensions|
|incoming.all.failedrequests|All Incoming Failed Requests|Sayı|Toplam|Total incoming failed requests for a notification hub|No Dimensions|

## <a name="microsoftoperationalinsightsworkspaces"></a>Microsoft.OperationalInsights/workspaces

|Ölçüm|Metric Display Name|Birim|Aggregation Type|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Average_% Free Inodes|% Free Inodes|Sayı|Ortalama|Average_% Free Inodes|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Free Space|% Free Space|Sayı|Ortalama|Average_% Free Space|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Used Inodes|% Used Inodes|Sayı|Ortalama|Average_% Used Inodes|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Used Space|% Used Space|Sayı|Ortalama|Average_% Used Space|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Disk Read Bytes/sec|Disk Read Bytes/sec|Sayı|Ortalama|Average_Disk Read Bytes/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Disk Reads/sec|Disk Reads/sec|Sayı|Ortalama|Average_Disk Reads/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Disk Transfers/sec|Disk Transfers/sec|Sayı|Ortalama|Average_Disk Transfers/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Disk Write Bytes/sec|Disk Write Bytes/sec|Sayı|Ortalama|Average_Disk Write Bytes/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Disk Writes/sec|Disk Writes/sec|Sayı|Ortalama|Average_Disk Writes/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Free Megabytes|Free Megabytes|Sayı|Ortalama|Average_Free Megabytes|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Logical Disk Bytes/sec|Logical Disk Bytes/sec|Sayı|Ortalama|Average_Logical Disk Bytes/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Available Memory|% Available Memory|Sayı|Ortalama|Average_% Available Memory|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Available Swap Space|% Available Swap Space|Sayı|Ortalama|Average_% Available Swap Space|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Used Memory|% Used Memory|Sayı|Ortalama|Average_% Used Memory|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Used Swap Space|% Used Swap Space|Sayı|Ortalama|Average_% Used Swap Space|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Available MBytes Memory|Available MBytes Memory|Sayı|Ortalama|Average_Available MBytes Memory|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Available MBytes Swap|Available MBytes Swap|Sayı|Ortalama|Average_Available MBytes Swap|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Page Reads/sec|Page Reads/sec|Sayı|Ortalama|Average_Page Reads/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Page Writes/sec|Page Writes/sec|Sayı|Ortalama|Average_Page Writes/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Pages/sec|Pages/sec|Sayı|Ortalama|Average_Pages/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Used MBytes Swap Space|Used MBytes Swap Space|Sayı|Ortalama|Average_Used MBytes Swap Space|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Used Memory MBytes|Used Memory MBytes|Sayı|Ortalama|Average_Used Memory MBytes|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Total Bytes Transmitted|Total Bytes Transmitted|Sayı|Ortalama|Average_Total Bytes Transmitted|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Total Bytes Received|Total Bytes Received|Sayı|Ortalama|Average_Total Bytes Received|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Total Bytes|Total Bytes|Sayı|Ortalama|Average_Total Bytes|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Total Packets Transmitted|Total Packets Transmitted|Sayı|Ortalama|Average_Total Packets Transmitted|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Total Packets Received|Total Packets Received|Sayı|Ortalama|Average_Total Packets Received|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Total Rx Errors|Total Rx Errors|Sayı|Ortalama|Average_Total Rx Errors|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Total Tx Errors|Total Tx Errors|Sayı|Ortalama|Average_Total Tx Errors|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Total Collisions|Total Collisions|Sayı|Ortalama|Average_Total Collisions|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Avg. Disk sec/Read|Avg. Disk sec/Read|Sayı|Ortalama|Average_Avg. Disk sec/Read|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Avg. Disk sec/Transfer|Avg. Disk sec/Transfer|Sayı|Ortalama|Average_Avg. Disk sec/Transfer|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Avg. Disk sec/Write|Avg. Disk sec/Write|Sayı|Ortalama|Average_Avg. Disk sec/Write|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Physical Disk Bytes/sec|Physical Disk Bytes/sec|Sayı|Ortalama|Average_Physical Disk Bytes/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Pct Privileged Time|Pct Privileged Time|Sayı|Ortalama|Average_Pct Privileged Time|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Pct User Time|Pct User Time|Sayı|Ortalama|Average_Pct User Time|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Used Memory kBytes|Used Memory kBytes|Sayı|Ortalama|Average_Used Memory kBytes|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Virtual Shared Memory|Virtual Shared Memory|Sayı|Ortalama|Average_Virtual Shared Memory|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% DPC Time|% DPC Time|Sayı|Ortalama|Average_% DPC Time|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Idle Time|% Idle Time|Sayı|Ortalama|Average_% Idle Time|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Interrupt Time|% Interrupt Time|Sayı|Ortalama|Average_% Interrupt Time|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% IO Wait Time|% IO Wait Time|Sayı|Ortalama|Average_% IO Wait Time|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Nice Time|% Nice Time|Sayı|Ortalama|Average_% Nice Time|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Privileged Time|% Privileged Time|Sayı|Ortalama|Average_% Privileged Time|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Processor Time|% Processor Time|Sayı|Ortalama|Average_% Processor Time|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% User Time|% User Time|Sayı|Ortalama|Average_% User Time|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Free Physical Memory|Free Physical Memory|Sayı|Ortalama|Average_Free Physical Memory|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Free Space in Paging Files|Free Space in Paging Files|Sayı|Ortalama|Average_Free Space in Paging Files|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Free Virtual Memory|Free Virtual Memory|Sayı|Ortalama|Average_Free Virtual Memory|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Processes|İşlemler|Sayı|Ortalama|Average_Processes|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Size Stored In Paging Files|Size Stored In Paging Files|Sayı|Ortalama|Average_Size Stored In Paging Files|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Uptime|Uptime|Sayı|Ortalama|Average_Uptime|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Users|Kullanıcılar|Sayı|Ortalama|Average_Users|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Avg. Disk sec/Read|Avg. Disk sec/Read|Sayı|Ortalama|Average_Avg. Disk sec/Read|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Avg. Disk sec/Write|Avg. Disk sec/Write|Sayı|Ortalama|Average_Avg. Disk sec/Write|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Current Disk Queue Length|Current Disk Queue Length|Sayı|Ortalama|Average_Current Disk Queue Length|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Disk Reads/sec|Disk Reads/sec|Sayı|Ortalama|Average_Disk Reads/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Disk Transfers/sec|Disk Transfers/sec|Sayı|Ortalama|Average_Disk Transfers/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Disk Writes/sec|Disk Writes/sec|Sayı|Ortalama|Average_Disk Writes/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Free Megabytes|Free Megabytes|Sayı|Ortalama|Average_Free Megabytes|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Free Space|% Free Space|Sayı|Ortalama|Average_% Free Space|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Available MBytes|Available MBytes|Sayı|Ortalama|Average_Available MBytes|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Committed Bytes In Use|% Committed Bytes In Use|Sayı|Ortalama|Average_% Committed Bytes In Use|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Bytes Received/sec|Bytes Received/sec|Sayı|Ortalama|Average_Bytes Received/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Bytes Sent/sec|Bytes Sent/sec|Sayı|Ortalama|Average_Bytes Sent/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Bytes Total/sec|Bytes Total/sec|Sayı|Ortalama|Average_Bytes Total/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Processor Time|% Processor Time|Sayı|Ortalama|Average_% Processor Time|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Processor Queue Length|Processor Queue Length|Sayı|Ortalama|Average_Processor Queue Length|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Heartbeat|Heartbeat|Sayı|Toplam|Heartbeat|Computer, OSType, Version, SourceComputerId|
|Güncelleştir|Güncelleştir|Sayı|Ortalama|Güncelleştir|Computer, Product, Classification, UpdateState, Optional, Approved|
|Olay|Olay|Sayı|Ortalama|Olay|Source, EventLog, Computer, EventCategory, EventLevel, EventLevelName, EventID|

## <a name="microsoftpowerbidedicatedcapacities"></a>Microsoft.PowerBIDedicated/capacities

|Ölçüm|Metric Display Name|Birim|Aggregation Type|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|QueryDuration|Query Duration|Milliseconds|Ortalama|DAX Query duration in last interval|No Dimensions|
|QueryPoolJobQueueLength|Threads: Query pool job queue length|Sayı|Ortalama|Number of jobs in the queue of the query thread pool.|No Dimensions|
|qpu_high_utilization_metric|QPU High Utilization|Sayı|Toplam|QPU High Utilization In Last Minute, 1 For High QPU Utilization, Otherwise 0|No Dimensions|
|memory_metric|Hafıza|Bytes|Ortalama|Memory. Range 0-3 GB for A1, 0-5 GB for A2, 0-10 GB for A3, 0-25 GB for A4, 0-50 GB for A5 and 0-100 GB for A6|No Dimensions|
|memory_thrashing_metric|Memory Thrashing|Yüzde|Ortalama|Average memory thrashing.|No Dimensions|

## <a name="microsoftrelaynamespaces"></a>Microsoft.Relay/namespaces

|Ölçüm|Metric Display Name|Birim|Aggregation Type|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|ListenerConnections-Success|ListenerConnections-Success|Sayı|Toplam|Successful ListenerConnections for Microsoft.Relay.|EntityName|
|ListenerConnections-ClientError|ListenerConnections-ClientError|Sayı|Toplam|ClientError on ListenerConnections for Microsoft.Relay.|EntityName|
|ListenerConnections-ServerError|ListenerConnections-ServerError|Sayı|Toplam|ServerError on ListenerConnections for Microsoft.Relay.|EntityName|
|SenderConnections-Success|SenderConnections-Success|Sayı|Toplam|Successful SenderConnections for Microsoft.Relay.|EntityName|
|SenderConnections-ClientError|SenderConnections-ClientError|Sayı|Toplam|ClientError on SenderConnections for Microsoft.Relay.|EntityName|
|SenderConnections-ServerError|SenderConnections-ServerError|Sayı|Toplam|ServerError on SenderConnections for Microsoft.Relay.|EntityName|
|ListenerConnections-TotalRequests|ListenerConnections-TotalRequests|Sayı|Toplam|Total ListenerConnections for Microsoft.Relay.|EntityName|
|SenderConnections-TotalRequests|SenderConnections-TotalRequests|Sayı|Toplam|Total SenderConnections requests for Microsoft.Relay.|EntityName|
|ActiveConnections|ActiveConnections|Sayı|Toplam|Total ActiveConnections for Microsoft.Relay.|EntityName|
|ActiveListeners|ActiveListeners|Sayı|Toplam|Total ActiveListeners for Microsoft.Relay.|EntityName|
|BytesTransferred|BytesTransferred|Sayı|Toplam|Total BytesTransferred for Microsoft.Relay.|EntityName|
|ListenerDisconnects|ListenerDisconnects|Sayı|Toplam|Total ListenerDisconnects for Microsoft.Relay.|EntityName|
|SenderDisconnects|SenderDisconnects|Sayı|Toplam|Total SenderDisconnects for Microsoft.Relay.|EntityName|

## <a name="microsoftsearchsearchservices"></a>Microsoft.Search/searchServices

|Ölçüm|Metric Display Name|Birim|Aggregation Type|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|SearchLatency|Search Latency|Saniye|Ortalama|Average search latency for the search service|No Dimensions|
|SearchQueriesPerSecond|Search queries per second|CountPerSecond|Ortalama|Search queries per second for the search service|No Dimensions|
|ThrottledSearchQueriesPercentage|Throttled search queries percentage|Yüzde|Ortalama|Percentage of search queries that were throttled for the search service|No Dimensions|

## <a name="microsoftservicebusnamespaces"></a>Microsoft.ServiceBus/namespaces

|Ölçüm|Metric Display Name|Birim|Aggregation Type|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|SuccessfulRequests|Successful Requests (Preview)|Sayı|Toplam|Total successful requests for a namespace (Preview)|EntityName|
|ServerErrors|Server Errors. (Önizleme)|Sayı|Toplam|Server Errors for Microsoft.ServiceBus. (Önizleme)|EntityName|
|UserErrors|User Errors. (Önizleme)|Sayı|Toplam|User Errors for Microsoft.ServiceBus. (Önizleme)|EntityName|
|OperationName|Throttled Requests. (Önizleme)|Sayı|Toplam|Throttled Requests for Microsoft.ServiceBus. (Önizleme)|EntityName|
|IncomingRequests|Incoming Requests (Preview)|Sayı|Toplam|Incoming Requests for Microsoft.ServiceBus. (Önizleme)|EntityName|
|IncomingMessages|Incoming Messages (Preview)|Sayı|Toplam|Incoming Messages for Microsoft.ServiceBus. (Önizleme)|EntityName|
|OutgoingMessages|Outgoing Messages (Preview)|Sayı|Toplam|Outgoing Messages for Microsoft.ServiceBus. (Önizleme)|EntityName|
|ActiveConnections|ActiveConnections (Preview)|Sayı|Toplam|Total Active Connections for Microsoft.ServiceBus. (Önizleme)|No Dimensions|
|Boyut|Size (Preview)|Bytes|Ortalama|Size of an Queue/Topic in Bytes. (Önizleme)|EntityName|
|Mesajlar|Count of messages in a Queue/Topic. (Önizleme)|Sayı|Ortalama|Count of messages in a Queue/Topic. (Önizleme)|EntityName|
|ActiveMessages|Count of active messages in a Queue/Topic. (Önizleme)|Sayı|Ortalama|Count of active messages in a Queue/Topic. (Önizleme)|EntityName|
|DeadletteredMessages|Count of dead-lettered messages in a Queue/Topic. (Önizleme)|Sayı|Ortalama|Count of dead-lettered messages in a Queue/Topic. (Önizleme)|EntityName|
|ScheduledMessages|Count of scheduled messages in a Queue/Topic. (Önizleme)|Sayı|Ortalama|Count of scheduled messages in a Queue/Topic. (Önizleme)|EntityName|
|CPUXNS|CPU usage per namespace|Yüzde|Maksimum|Service bus premium namespace CPU usage metric|No Dimensions|
|WSXNS|Memory size usage per namespace|Yüzde|Maksimum|Service bus premium namespace memory usage metric|No Dimensions|

## <a name="microsoftservicefabricmeshapplications"></a>Microsoft.ServiceFabricMesh/applications

|Ölçüm|Metric Display Name|Birim|Aggregation Type|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|AllocatedCpu|AllocatedCpu|Sayı|Ortalama|Cpu allocated to this container in millicores|ApplicationName, ServiceName, CodePackageName, ServiceReplicaName|
|AllocatedMemory|AllocatedMemory|Bytes|Ortalama|Memory allocated to this container in MB|ApplicationName, ServiceName, CodePackageName, ServiceReplicaName|
|ActualCpu|ActualCpu|Sayı|Ortalama|Actual CPU usage in millicores|ApplicationName, ServiceName, CodePackageName, ServiceReplicaName|
|ActualMemory|ActualMemory|Bytes|Ortalama|Actual memory usage in MB|ApplicationName, ServiceName, CodePackageName, ServiceReplicaName|
|CpuUtilization|CpuUtilization|Yüzde|Ortalama|Utilization of CPU for this container as percentage of AllocatedCpu|ApplicationName, ServiceName, CodePackageName, ServiceReplicaName|
|MemoryUtilization|MemoryUtilization|Yüzde|Ortalama|Utilization of CPU for this container as percentage of AllocatedCpu|ApplicationName, ServiceName, CodePackageName, ServiceReplicaName|
|ApplicationStatus|ApplicationStatus|Sayı|Ortalama|Status of Service Fabric Mesh application|ApplicationName, Status|
|ServiceStatus|ServiceStatus|Sayı|Ortalama|Health Status of a service in Service Fabric Mesh application|ApplicationName, Status, ServiceName|
|ServiceReplicaStatus|ServiceReplicaStatus|Sayı|Ortalama|Health Status of a service replica in Service Fabric Mesh application|ApplicationName, Status, ServiceName, ServiceReplicaName|
|ContainerStatus|ContainerStatus|Sayı|Ortalama|Status of the container in Service Fabric Mesh application|ApplicationName, ServiceName, CodePackageName, ServiceReplicaName, Status|
|RestartCount|RestartCount|Sayı|Ortalama|Restart count of a container in Service Fabric Mesh application|ApplicationName, Status, ServiceName, ServiceReplicaName, CodePackageName|

## <a name="microsoftsignalrservicesignalr"></a>Microsoft.SignalRService/SignalR

|Ölçüm|Metric Display Name|Birim|Aggregation Type|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|ConnectionCount|Connection Count|Sayı|Maksimum|The amount of user connection.|Uç nokta|
|MessageCount|Message Count|Sayı|Toplam|The total amount of messages.|No Dimensions|
|InboundTraffic|Gelen Trafik|Bytes|Toplam|The inbound traffic of service|No Dimensions|
|OutboundTraffic|Giden Trafik|Bytes|Toplam|The outbound traffic of service|No Dimensions|
|UserErrors|User Errors|Yüzde|Maksimum|The percentage of user errors|No Dimensions|
|SystemErrors|System Errors|Yüzde|Maksimum|The percentage of system errors|No Dimensions|

## <a name="microsoftsqlserversdatabases"></a>Microsoft.Sql/servers/databases

|Ölçüm|Metric Display Name|Birim|Aggregation Type|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|allocated_data_storage|Data space allocated|Bytes|Ortalama|Data space allocated. Not applicable to data warehouses.|No Dimensions|
|app_cpu_billed|App CPU billed|Sayı|Toplam|App CPU billed. Applies to serverless databases.|No Dimensions|
|app_cpu_percent|App CPU percentage|Yüzde|Ortalama|App CPU percentage. Applies to serverless databases.|No Dimensions|
|app_memory_percent|App memory used percentage|Yüzde|Ortalama|App memory used percentage. Applies to serverless databases.|No Dimensions|
|blocked_by_firewall|Blocked by Firewall|Sayı|Toplam|Blocked by Firewall|No Dimensions|
|cache_hit_percent|Cache hit percentage|Yüzde|Maksimum|Cache hit percentage. Applies only to data warehouses.|No Dimensions|
|cache_used_percent|Cache used percentage|Yüzde|Maksimum|Cache used percentage. Applies only to data warehouses.|No Dimensions|
|connection_failed|Başarısız Bağlantılar|Sayı|Toplam|Başarısız Bağlantılar|No Dimensions|
|connection_successful|Successful Connections|Sayı|Toplam|Successful Connections|No Dimensions|
|cpu_percent|CPU yüzdesi|Yüzde|Ortalama|CPU yüzdesi|No Dimensions|
|cpu_limit|CPU limit|Sayı|Ortalama|CPU limit. Applies to vCore-based databases.|No Dimensions|
|cpu_used|CPU used|Sayı|Ortalama|CPU used. Applies to vCore-based databases.|No Dimensions|
|deadlock|Deadlocks|Sayı|Toplam|Deadlocks. Not applicable to data warehouses.|No Dimensions|
|diff_backup_size_bytes|Differential backup storage size|Bytes|Maksimum|Cumulative differential backup storage size. Applies to General Purpose and Business Critical databases. Does not currently apply to Manage instance databases.|No Dimensions|
|dtu_limit|DTU Limit|Sayı|Ortalama|DTU Limit. Applies to DTU-based databases.|No Dimensions|
|dtu_consumption_percent|DTU yüzdesi|Yüzde|Ortalama|DTU percentage. Applies to DTU-based databases.|No Dimensions|
|dtu_used|DTU used|Sayı|Ortalama|DTU used. Applies to DTU-based databases.|No Dimensions|
|dw_cpu_percent|DW node level CPU percentage|Yüzde|Ortalama|DW node level CPU percentage|DwLogicalNodeId|
|dw_physical_data_read_percent|DW node level Data IO percentage|Yüzde|Ortalama|DW node level Data IO percentage|DwLogicalNodeId|
|dwu_consumption_percent|DWU percentage|Yüzde|Maksimum|DWU percentage. Applies only to data warehouses.|No Dimensions|
|dwu_limit|DWU limit|Sayı|Maksimum|DWU limit. Applies only to data warehouses.|No Dimensions|
|dwu_used|DWU used|Sayı|Maksimum|DWU used. Applies only to data warehouses.|No Dimensions|
|full_backup_size_bytes|Full backup storage size|Bytes|Maksimum|Cumulative full backup storage size. Applies to General Purpose and Business Critical databases. Does not currently apply to Manage instance databases.|No Dimensions|
|local_tempdb_usage_percent|Local tempdb percentage|Yüzde|Ortalama|Local tempdb percentage. Applies only to data warehouses.|No Dimensions|
|log_backup_size_bytes|Log backup storage size|Bytes|Maksimum|Cumulative log backup storage size. Applies to General Purpose and Business Critical databases. Does not currently apply to Manage instance databases.|No Dimensions|
|log_write_percent|Log IO percentage|Yüzde|Ortalama|Log IO percentage. Not applicable to data warehouses.|No Dimensions|
|physical_data_read_percent|Veri G/Ç yüzdesi|Yüzde|Ortalama|Veri G/Ç yüzdesi|No Dimensions|
|sessions_percent|Sessions percentage|Yüzde|Ortalama|Sessions percentage. Not applicable to data warehouses.|No Dimensions|
|sqlserver_process_core_percent|SQL Server process core percent|Yüzde|Maksimum|This metric is a placeholder and not populated at this time.|No Dimensions|
|sqlserver_process_memory_percent|SQL Server process memory percent|Yüzde|Maksimum|This metric is a placeholder and not populated at this time.|No Dimensions|
|depolama|Data space used|Bytes|Maksimum|Total database size. Not applicable to data warehouses.|No Dimensions|
|storage_percent|Data space used percent|Yüzde|Maksimum|Database size percentage. Not applicable to data warehouses or hyperscale databases.|No Dimensions|
|tempdb_data_size|Tempdb Data File Size Kilobytes|Sayı|Maksimum|Tempdb Data File Size Kilobytes. Not applicable to data warehouses. This metric will be available for databases using the vCore purchasing model or 100 DTU and higher for DTU-based purchasing models.|No Dimensions|
|tempdb_log_size|Tempdb Log File Size Kilobytes|Sayı|Maksimum|Tempdb Log File Size Kilobytes. Not applicable to data warehouses. This metric will be available for databases using the vCore purchasing model or 100 DTU and higher for DTU-based purchasing models.|No Dimensions|
|tempdb_log_used_percent|Tempdb Percent Log Used|Yüzde|Maksimum|Tempdb Percent Log Used. Not applicable to data warehouses. This metric will be available for databases using the vCore purchasing model or 100 DTU and higher for DTU-based purchasing models.|No Dimensions|
|workers_percent|Workers percentage|Yüzde|Ortalama|Workers percentage. Not applicable to data warehouses.|No Dimensions|
|xtp_storage_percent|In-Memory OLTP storage percent|Yüzde|Ortalama|In-Memory OLTP storage percent. Not applicable to data warehouses.|No Dimensions|

## <a name="microsoftsqlserverselasticpools"></a>Microsoft.Sql/servers/elasticPools

|Ölçüm|Metric Display Name|Birim|Aggregation Type|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|allocated_data_storage|Data space allocated|Bytes|Ortalama|Data space allocated|No Dimensions|
|allocated_data_storage_percent|Data space allocated percent|Yüzde|Maksimum|Data space allocated percent|No Dimensions|
|cpu_limit|CPU limit|Sayı|Ortalama|CPU limit. Applies to vCore-based elastic pools.|No Dimensions|
|cpu_percent|CPU yüzdesi|Yüzde|Ortalama|CPU yüzdesi|No Dimensions|
|cpu_used|CPU used|Sayı|Ortalama|CPU used. Applies to vCore-based elastic pools.|No Dimensions|
|dtu_consumption_percent|DTU yüzdesi|Yüzde|Ortalama|DTU percentage. Applies to DTU-based elastic pools.|No Dimensions|
|eDTU_limit|eDTU limit|Sayı|Ortalama|eDTU limit. Applies to DTU-based elastic pools.|No Dimensions|
|eDTU_used|eDTU used|Sayı|Ortalama|eDTU used. Applies to DTU-based elastic pools.|No Dimensions|
|log_write_percent|Log IO percentage|Yüzde|Ortalama|Log IO percentage|No Dimensions|
|physical_data_read_percent|Veri G/Ç yüzdesi|Yüzde|Ortalama|Veri G/Ç yüzdesi|No Dimensions|
|sessions_percent|Sessions percentage|Yüzde|Ortalama|Sessions percentage|No Dimensions|
|storage_limit|Data max size|Bytes|Ortalama|Storage limit|No Dimensions|
|storage_percent|Data space used percent||Yüzde|Ortalama|Storage percentage|No Dimensions|
|storage_used|Data space used|Bytes|Ortalama|Kullanılan depolama|No Dimensions|
|sqlserver_process_core_percent|SQL Server process core percent|Yüzde|Maksimum|This metric is a placeholder and not populated at this time.|No Dimensions|
|sqlserver_process_memory_percent|SQL Server process memory percent|Yüzde|Maksimum|This metric is a placeholder and not populated at this time.|No Dimensions|
|tempdb_data_size|Tempdb Data File Size Kilobytes|Sayı|Maksimum|Tempdb Data File Size Kilobytes. Not applicable to data warehouses. This metric will be available for databases using the vCore purchasing model or 100 DTU and higher for DTU-based purchasing models.|No Dimensions|
|tempdb_log_size|Tempdb Log File Size Kilobytes|Sayı|Maksimum|Tempdb Log File Size Kilobytes. Not applicable to data warehouses. This metric will be available for databases using the vCore purchasing model or 100 DTU and higher for DTU-based purchasing models.|No Dimensions|
|tempdb_log_used_percent|Tempdb Percent Log Used|Yüzde|Maksimum|Tempdb Percent Log Used. Not applicable to data warehouses. This metric will be available for databases using the vCore purchasing model or 100 DTU and higher for DTU-based purchasing models.|No Dimensions|
|workers_percent|Workers percentage|Yüzde|Ortalama|Workers percentage|No Dimensions|
|xtp_storage_percent|In-Memory OLTP storage percent|Yüzde|Ortalama|In-Memory OLTP storage percent|No Dimensions|

## <a name="microsoftsqlmanagedinstances"></a>Microsoft.Sql/managedInstances

|Ölçüm|Metric Display Name|Birim|Aggregation Type|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|avg_cpu_percent|Average CPU percentage|Yüzde|Ortalama|Average CPU percentage|No Dimensions|
|io_bytes_read|IO bytes read|Bytes|Ortalama|IO bytes read|No Dimensions|
|io_requests|IO requests count|Sayı|Ortalama|IO requests count|No Dimensions|
|io_bytes_written|IO bytes written|Bytes|Ortalama|IO bytes written|No Dimensions|
|reserved_storage_mb|Storage space reserved|Sayı|Ortalama|Storage space reserved|No Dimensions|
|storage_space_used_mb|Storage space used|Sayı|Ortalama|Storage space used|No Dimensions|
|virtual_core_count|Virtual core count|Sayı|Ortalama|Virtual core count|No Dimensions|

## <a name="microsoftstoragestorageaccounts"></a>Microsoft.Storage/storageAccounts

|Ölçüm|Metric Display Name|Birim|Aggregation Type|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|UsedCapacity|Used capacity|Bytes|Ortalama|Account used capacity|No Dimensions|
|İşlemler|İşlemler|Sayı|Toplam|Bir depolama hizmetine yapılan isteklerin veya belirtilen API işlemi sayısı. Bu sayı, başarılı ve başarısız isteklerin yanı sıra hata üreten istekleri içerir. Use ResponseType dimension for the number of different type of response.|ResponseType, GeoType, ApiName, Authentication|
|Giriş|Giriş|Bytes|Toplam|The amount of ingress data, in bytes. Bu sayı, dış istemciden Azure Depolama'ya giren ve Azure içinde giren verileri içerir.|GeoType, ApiName, Authentication|
|Çıkış|Çıkış|Bytes|Toplam|The amount of egress data, in bytes. Bu sayı, dış istemciden Azure Depolama'ya çıkan ve Azure içinde çıkan verileri içerir. Sonuç olarak bu sayı, faturalanabilir çıkışı yansıtmaz.|GeoType, ApiName, Authentication|
|SuccessServerLatency|Success Server Latency|Milliseconds|Ortalama|The average latency used by Azure Storage to process a successful request, in milliseconds. Bu değer, AverageE2ELatency öğesinde belirtilen ağ gecikme süresini içermez.|GeoType, ApiName, Authentication|
|SuccessE2ELatency|Success E2E Latency|Milliseconds|Ortalama|The average end-to-end latency of successful requests made to a storage service or the specified API operation, in milliseconds. Bu değer, isteği okumak, yanıtı göndermek ve yanıtın onayını almak için Azure Depolama içinde gerekli işleme süresini içerir.|GeoType, ApiName, Authentication|
|Erişilebilirlik|Erişilebilirlik|Yüzde|Ortalama|The percentage of availability for the storage service or the specified API operation. Kullanılabilirlik, TotalBillableRequests değerinin beklenmeyen hata üretenler dahil olmak üzere ilgili istek sayısına bölünmesiyle hesaplanır. Beklenmeyen tüm hatalar, depolama hizmeti veya belirtilen API işlemi için kullanılabilirliğin azalmasıyla sonuçlanır.|GeoType, ApiName, Authentication|

## <a name="microsoftstoragestorageaccountsblobservices"></a>Microsoft.Storage/storageAccounts/blobServices

|Ölçüm|Metric Display Name|Birim|Aggregation Type|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|BlobCapacity|Blob Capacity|Bytes|Ortalama|The amount of storage used by the storage account’s Blob service in bytes.|BlobType, Tier|
|BlobCount|Blob Count|Sayı|Toplam|The number of Blob in the storage account’s Blob service.|BlobType|       |BlobCount|Blob Count|Sayı|Ortalama|The number of Blob in the storage account’s Blob service.|BlobType, Tier|
|ContainerCount|Blob Container Count|Sayı|Ortalama|The number of containers in the storage account’s Blob service.|No Dimensions|
|IndexCapacity|Index Capacity|Bytes|Ortalama|The amount of storage used by ADLS Gen2 (Hierarchical) Index in bytes.|No Dimensions|
|İşlemler|İşlemler|Sayı|Toplam|Bir depolama hizmetine yapılan isteklerin veya belirtilen API işlemi sayısı. Bu sayı, başarılı ve başarısız isteklerin yanı sıra hata üreten istekleri içerir. Use ResponseType dimension for the number of different type of response.|ResponseType, GeoType, ApiName, Authentication|
|Giriş|Giriş|Bytes|Toplam|The amount of ingress data, in bytes. Bu sayı, dış istemciden Azure Depolama'ya giren ve Azure içinde giren verileri içerir.|GeoType, ApiName, Authentication|
|Çıkış|Çıkış|Bytes|Toplam|The amount of egress data, in bytes. Bu sayı, dış istemciden Azure Depolama'ya çıkan ve Azure içinde çıkan verileri içerir. Sonuç olarak bu sayı, faturalanabilir çıkışı yansıtmaz.|GeoType, ApiName, Authentication|
|SuccessServerLatency|Success Server Latency|Milliseconds|Ortalama|The average latency used by Azure Storage to process a successful request, in milliseconds. Bu değer, AverageE2ELatency öğesinde belirtilen ağ gecikme süresini içermez.|GeoType, ApiName, Authentication|
|SuccessE2ELatency|Success E2E Latency|Milliseconds|Ortalama|The average end-to-end latency of successful requests made to a storage service or the specified API operation, in milliseconds. Bu değer, isteği okumak, yanıtı göndermek ve yanıtın onayını almak için Azure Depolama içinde gerekli işleme süresini içerir.|GeoType, ApiName, Authentication|
|Erişilebilirlik|Erişilebilirlik|Yüzde|Ortalama|The percentage of availability for the storage service or the specified API operation. Kullanılabilirlik, TotalBillableRequests değerinin beklenmeyen hata üretenler dahil olmak üzere ilgili istek sayısına bölünmesiyle hesaplanır. Beklenmeyen tüm hatalar, depolama hizmeti veya belirtilen API işlemi için kullanılabilirliğin azalmasıyla sonuçlanır.|GeoType, ApiName, Authentication|

## <a name="microsoftstoragestorageaccountsfileservices"></a>Microsoft.Storage/storageAccounts/fileServices

|Ölçüm|Metric Display Name|Birim|Aggregation Type|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|FileCapacity|File Capacity|Bytes|Ortalama|The amount of storage used by the storage account’s File service in bytes.|No Dimensions|
|FileCount|File Count|Sayı|Ortalama|The number of file in the storage account’s File service.|No Dimensions|
|FileShareCount|File Share Count|Sayı|Ortalama|The number of file shares in the storage account’s File service.|No Dimensions|
|İşlemler|İşlemler|Sayı|Toplam|Bir depolama hizmetine yapılan isteklerin veya belirtilen API işlemi sayısı. Bu sayı, başarılı ve başarısız isteklerin yanı sıra hata üreten istekleri içerir. Use ResponseType dimension for the number of different type of response.|ResponseType, GeoType, ApiName, Authentication|
|Giriş|Giriş|Bytes|Toplam|The amount of ingress data, in bytes. Bu sayı, dış istemciden Azure Depolama'ya giren ve Azure içinde giren verileri içerir.|GeoType, ApiName, Authentication|
|Çıkış|Çıkış|Bytes|Toplam|The amount of egress data, in bytes. Bu sayı, dış istemciden Azure Depolama'ya çıkan ve Azure içinde çıkan verileri içerir. Sonuç olarak bu sayı, faturalanabilir çıkışı yansıtmaz.|GeoType, ApiName, Authentication|
|SuccessServerLatency|Success Server Latency|Milliseconds|Ortalama|The average latency used by Azure Storage to process a successful request, in milliseconds. Bu değer, AverageE2ELatency öğesinde belirtilen ağ gecikme süresini içermez.|GeoType, ApiName, Authentication|
|SuccessE2ELatency|Success E2E Latency|Milliseconds|Ortalama|The average end-to-end latency of successful requests made to a storage service or the specified API operation, in milliseconds. Bu değer, isteği okumak, yanıtı göndermek ve yanıtın onayını almak için Azure Depolama içinde gerekli işleme süresini içerir.|GeoType, ApiName, Authentication|
|Erişilebilirlik|Erişilebilirlik|Yüzde|Ortalama|The percentage of availability for the storage service or the specified API operation. Kullanılabilirlik, TotalBillableRequests değerinin beklenmeyen hata üretenler dahil olmak üzere ilgili istek sayısına bölünmesiyle hesaplanır. Beklenmeyen tüm hatalar, depolama hizmeti veya belirtilen API işlemi için kullanılabilirliğin azalmasıyla sonuçlanır.|GeoType, ApiName, Authentication|

## <a name="microsoftstoragestorageaccountsqueueservices"></a>Microsoft.Storage/storageAccounts/queueServices

|Ölçüm|Metric Display Name|Birim|Aggregation Type|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|QueueCapacity|Queue Capacity|Bytes|Ortalama|The amount of storage used by the storage account’s Queue service in bytes.|No Dimensions|
|QueueCount|Queue Count|Sayı|Ortalama|The number of queue in the storage account’s Queue service.|No Dimensions|
|QueueMessageCount|Queue Message Count|Sayı|Ortalama|The approximate number of queue messages in the storage account’s Queue service.|No Dimensions|
|İşlemler|İşlemler|Sayı|Toplam|Bir depolama hizmetine yapılan isteklerin veya belirtilen API işlemi sayısı. Bu sayı, başarılı ve başarısız isteklerin yanı sıra hata üreten istekleri içerir. Use ResponseType dimension for the number of different type of response.|ResponseType, GeoType, ApiName, Authentication|
|Giriş|Giriş|Bytes|Toplam|The amount of ingress data, in bytes. Bu sayı, dış istemciden Azure Depolama'ya giren ve Azure içinde giren verileri içerir.|GeoType, ApiName, Authentication|
|Çıkış|Çıkış|Bytes|Toplam|The amount of egress data, in bytes. Bu sayı, dış istemciden Azure Depolama'ya çıkan ve Azure içinde çıkan verileri içerir. Sonuç olarak bu sayı, faturalanabilir çıkışı yansıtmaz.|GeoType, ApiName, Authentication|
|SuccessServerLatency|Success Server Latency|Milliseconds|Ortalama|The average latency used by Azure Storage to process a successful request, in milliseconds. Bu değer, AverageE2ELatency öğesinde belirtilen ağ gecikme süresini içermez.|GeoType, ApiName, Authentication|
|SuccessE2ELatency|Success E2E Latency|Milliseconds|Ortalama|The average end-to-end latency of successful requests made to a storage service or the specified API operation, in milliseconds. Bu değer, isteği okumak, yanıtı göndermek ve yanıtın onayını almak için Azure Depolama içinde gerekli işleme süresini içerir.|GeoType, ApiName, Authentication|
|Erişilebilirlik|Erişilebilirlik|Yüzde|Ortalama|The percentage of availability for the storage service or the specified API operation. Kullanılabilirlik, TotalBillableRequests değerinin beklenmeyen hata üretenler dahil olmak üzere ilgili istek sayısına bölünmesiyle hesaplanır. Beklenmeyen tüm hatalar, depolama hizmeti veya belirtilen API işlemi için kullanılabilirliğin azalmasıyla sonuçlanır.|GeoType, ApiName, Authentication|

## <a name="microsoftstoragestorageaccountstableservices"></a>Microsoft.Storage/storageAccounts/tableServices

|Ölçüm|Metric Display Name|Birim|Aggregation Type|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|TableCapacity|Table Capacity|Bytes|Ortalama|The amount of storage used by the storage account’s Table service in bytes.|No Dimensions|
|TableCount|Table Count|Sayı|Ortalama|The number of table in the storage account’s Table service.|No Dimensions|
|TableEntityCount|Table Entity Count|Sayı|Ortalama|The number of table entities in the storage account’s Table service.|No Dimensions|
|İşlemler|İşlemler|Sayı|Toplam|Bir depolama hizmetine yapılan isteklerin veya belirtilen API işlemi sayısı. Bu sayı, başarılı ve başarısız isteklerin yanı sıra hata üreten istekleri içerir. Use ResponseType dimension for the number of different type of response.|ResponseType, GeoType, ApiName, Authentication|
|Giriş|Giriş|Bytes|Toplam|The amount of ingress data, in bytes. Bu sayı, dış istemciden Azure Depolama'ya giren ve Azure içinde giren verileri içerir.|GeoType, ApiName, Authentication|
|Çıkış|Çıkış|Bytes|Toplam|The amount of egress data, in bytes. Bu sayı, dış istemciden Azure Depolama'ya çıkan ve Azure içinde çıkan verileri içerir. Sonuç olarak bu sayı, faturalanabilir çıkışı yansıtmaz.|GeoType, ApiName, Authentication|
|SuccessServerLatency|Success Server Latency|Milliseconds|Ortalama|The average latency used by Azure Storage to process a successful request, in milliseconds. Bu değer, AverageE2ELatency öğesinde belirtilen ağ gecikme süresini içermez.|GeoType, ApiName, Authentication|
|SuccessE2ELatency|Success E2E Latency|Milliseconds|Ortalama|The average end-to-end latency of successful requests made to a storage service or the specified API operation, in milliseconds. Bu değer, isteği okumak, yanıtı göndermek ve yanıtın onayını almak için Azure Depolama içinde gerekli işleme süresini içerir.|GeoType, ApiName, Authentication|
|Erişilebilirlik|Erişilebilirlik|Yüzde|Ortalama|The percentage of availability for the storage service or the specified API operation. Kullanılabilirlik, TotalBillableRequests değerinin beklenmeyen hata üretenler dahil olmak üzere ilgili istek sayısına bölünmesiyle hesaplanır. Beklenmeyen tüm hatalar, depolama hizmeti veya belirtilen API işlemi için kullanılabilirliğin azalmasıyla sonuçlanır.|GeoType, ApiName, Authentication|

## <a name="microsoftstoragesyncstoragesyncservices"></a>microsoft.storagesync/storageSyncServices

|Ölçüm|Metric Display Name|Birim|Aggregation Type|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|ServerSyncSessionResult|Sync Session Result|Sayı|Ortalama|Metric that logs a value of 1 each time the Server Endpoint successfully completes a Sync Session with the Cloud Endpoint|SyncGroupName, ServerEndpointName, SyncDirection|
|StorageSyncSyncSessionAppliedFilesCount|Files Synced|Sayı|Toplam|Count of Files synced|SyncGroupName, ServerEndpointName, SyncDirection|
|StorageSyncSyncSessionPerItemErrorsCount|Files not syncing|Sayı|Toplam|Count of files failed to sync|SyncGroupName, ServerEndpointName, SyncDirection|
|StorageSyncBatchTransferredFileBytes|Bytes synced|Bytes|Toplam|Total file size transferred for Sync Sessions|SyncGroupName, ServerEndpointName, SyncDirection|
|StorageSyncServerHeartbeat|Server Online Status|Sayı|Maksimum|Metric that logs a value of 1 each time the registered server successfully records a heartbeat with the Cloud Endpoint|aboneliğinde ve|
|StorageSyncRecallIOTotalSizeBytes|Cloud tiering recall|Bytes|Toplam|Total size of data recalled by the server|aboneliğinde ve|

## <a name="microsoftstreamanalyticsstreamingjobs"></a>Microsoft.StreamAnalytics/streamingjobs

|Ölçüm|Metric Display Name|Birim|Aggregation Type|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|ResourceUtilization|SU % Utilization|Yüzde|Maksimum|SU % Utilization|LogicalName, PartitionId|
|InputEvents|Input Events|Sayı|Toplam|Input Events|LogicalName, PartitionId|
|InputEventBytes|Input Event Bytes|Bytes|Toplam|Input Event Bytes|LogicalName, PartitionId|
|LateInputEvents|Late Input Events|Sayı|Toplam|Late Input Events|LogicalName, PartitionId|
|OutputEvents|Output Events|Sayı|Toplam|Output Events|LogicalName, PartitionId|
|ConversionErrors|Data Conversion Errors|Sayı|Toplam|Data Conversion Errors|LogicalName, PartitionId|
|Hatalar|Runtime Errors|Sayı|Toplam|Runtime Errors|LogicalName, PartitionId|
|DroppedOrAdjustedEvents|Out of order Events|Sayı|Toplam|Out of order Events|LogicalName, PartitionId|
|AMLCalloutRequests|Function Requests|Sayı|Toplam|Function Requests|LogicalName, PartitionId|
|AMLCalloutFailedRequests|Failed Function Requests|Sayı|Toplam|Failed Function Requests|LogicalName, PartitionId|
|AMLCalloutInputEvents|Function Events|Sayı|Toplam|Function Events|LogicalName, PartitionId|
|DeserializationError|Input Deserialization Errors|Sayı|Toplam|Input Deserialization Errors|LogicalName, PartitionId|
|EarlyInputEvents|Early Input Events|Sayı|Toplam|Early Input Events|LogicalName, PartitionId|
|OutputWatermarkDelaySeconds|Watermark Delay|Saniye|Maksimum|Watermark Delay|LogicalName, PartitionId|
|InputEventsSourcesBacklogged|Backlogged Input Events|Sayı|Maksimum|Backlogged Input Events|LogicalName, PartitionId|
|InputEventsSourcesPerSecond|Input Sources Received|Sayı|Toplam|Input Sources Received|LogicalName, PartitionId|

## <a name="microsofttimeseriesinsightsenvironments"></a>Microsoft.TimeSeriesInsights/environments

|Ölçüm|Metric Display Name|Birim|Aggregation Type|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|IngressReceivedMessages|Ingress Received Messages|Sayı|Toplam|Count of messages read from all Event hub or IoT hub event sources|No Dimensions|
|IngressReceivedInvalidMessages|Ingress Received Invalid Messages|Sayı|Toplam|Count of invalid messages read from all Event hub or IoT hub event sources|No Dimensions|
|IngressReceivedBytes|Ingress Received Bytes|Bytes|Toplam|Count of bytes read from all event sources|No Dimensions|
|IngressStoredBytes|Ingress Stored Bytes|Bytes|Toplam|Total size of events successfully processed and available for query|No Dimensions|
|IngressStoredEvents|Ingress Stored Events|Sayı|Toplam|Count of flattened events successfully processed and available for query|No Dimensions|
|IngressReceivedMessagesTimeLag|Ingress Received Messages Time Lag|Saniye|Maksimum|Difference between the time that the message is enqueued in the event source and the time it is processed in Ingress|No Dimensions|
|IngressReceivedMessagesCountLag|Ingress Received Messages Count Lag|Sayı|Ortalama|Difference between the sequence number of last enqueued message in the event source partition and sequence number of message being processed in Ingress|No Dimensions|
|WarmStorageMaxProperties|Warm Storage Max Properties|Sayı|Maksimum|Maximum number of properties used allowed by the environment for S1/S2 SKU and maximum number of properties allowed by Warm Store for PAYG SKU|No Dimensions|
|WarmStorageUsedProperties|Warm Storage Used Properties |Sayı|Maksimum|Number of properties used by the environment for S1/S2 SKU and number of properties used by Warm Store for PAYG SKU|No Dimensions|

## <a name="microsofttimeseriesinsightsenvironmentseventsources"></a>Microsoft.TimeSeriesInsights/environments/eventsources

|Ölçüm|Metric Display Name|Birim|Aggregation Type|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|IngressReceivedMessages|Ingress Received Messages|Sayı|Toplam|Count of messages read from the event source|No Dimensions|
|IngressReceivedInvalidMessages|Ingress Received Invalid Messages|Sayı|Toplam|Count of invalid messages read from the event source|No Dimensions|
|IngressReceivedBytes|Ingress Received Bytes|Bytes|Toplam|Count of bytes read from the event source|No Dimensions|
|IngressStoredBytes|Ingress Stored Bytes|Bytes|Toplam|Total size of events successfully processed and available for query|No Dimensions|
|IngressStoredEvents|Ingress Stored Events|Sayı|Toplam|Count of flattened events successfully processed and available for query|No Dimensions|
|IngressReceivedMessagesTimeLag|Ingress Received Messages Time Lag|Saniye|Maksimum|Difference between the time that the message is enqueued in the event source and the time it is processed in Ingress|No Dimensions|
|IngressReceivedMessagesCountLag|Ingress Received Messages Count Lag|Sayı|Ortalama|Difference between the sequence number of last enqueued message in the event source partition and sequence number of message being processed in Ingress|No Dimensions|
|WarmStorageMaxProperties|Warm Storage Max Properties|Sayı|Maksimum|Maximum number of properties used allowed by the environment for S1/S2 SKU and maximum number of properties allowed by Warm Store for PAYG SKU|No Dimensions|
|WarmStorageUsedProperties|Warm Storage Used Properties |Sayı|Maksimum|Number of properties used by the environment for S1/S2 SKU and number of properties used by Warm Store for PAYG SKU|No Dimensions|

## <a name="microsoftvmwarecloudsimplevirtualmachines"></a>Microsoft.VMwareCloudSimple/virtualMachines

|Ölçüm|Metric Display Name|Birim|Aggregation Type|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|DiskReadBytesPerSecond|Disk Read Bytes/Sec|BytesPerSecond|Ortalama|Average disk throughput due to read operations over the sample period.|No Dimensions|
|DiskWriteBytesPerSecond|Disk Write Bytes/Sec|BytesPerSecond|Ortalama|Average disk throughput due to write operations over the sample period.|No Dimensions|
|Disk Read Bytes|Disk Read Bytes|Bytes|Toplam|Total disk throughput due to read operations over the sample period.|No Dimensions|
|Disk Write Bytes|Disk Write Bytes|Bytes|Toplam|Total disk throughput due to write operations over the sample period.|No Dimensions|
|DiskReadOperations|Disk Read Operations|Sayı|Toplam|The number of IO read operations in the previous sample period. Note that these operations may be variable sized.|No Dimensions|
|DiskWriteOperations|Disk Write Operations|Sayı|Toplam|The number of IO write operations in the previous sample period. Note that these operations may be variable sized.|No Dimensions|
|Disk Read Operations/Sec|Disk Read Operations/Sec|CountPerSecond|Ortalama|The average number of IO read operations in the previous sample period. Note that these operations may be variable sized.|No Dimensions|
|Disk Write Operations/Sec|Disk Write Operations/Sec|CountPerSecond|Ortalama|The average number of IO write operations in the previous sample period. Note that these operations may be variable sized.|No Dimensions|
|DiskReadLatency|Disk Read Latency|Milliseconds|Ortalama|Total read latency. The sum of the device and kernel read latencies.|No Dimensions|
|DiskWriteLatency|Disk Write Latency|Milliseconds|Ortalama|Total write latency. The sum of the device and kernel write latencies.|No Dimensions|
|NetworkInBytesPerSecond|Network In Bytes/Sec|BytesPerSecond|Ortalama|Average network throughput for received traffic.|No Dimensions|
|NetworkOutBytesPerSecond|Network Out Bytes/Sec|BytesPerSecond|Ortalama|Average network throughput for transmitted traffic.|No Dimensions|
|Ağ Girişi|Ağ Girişi|Bytes|Toplam|Total network throughput for received traffic.|No Dimensions|
|Ağ Çıkışı|Ağ Çıkışı|Bytes|Toplam|Total network throughput for transmitted traffic.|No Dimensions|
|MemoryUsed|Memory Used|Bytes|Ortalama|The amount of machine memory that is in use by the VM.|No Dimensions|
|MemoryGranted|Memory Granted|Bytes|Ortalama|The amount of memory that was granted to the VM by the host. Memory is not granted to the host until it is touched one time and granted memory may be swapped out or ballooned away if the VMkernel needs the memory.|No Dimensions|
|MemoryActive|Memory Active|Bytes|Ortalama|The amount of memory used by the VM in the past small window of time. This is the "true" number of how much memory the VM currently has need of. Additional, unused memory may be swapped out or ballooned with no impact to the guest's performance.|No Dimensions|
|CPU yüzdesi|CPU yüzdesi|Yüzde|Ortalama|The CPU utilization. This value is reported with 100% representing all processor cores on the system. As an example, a 2-way VM using 50% of a four-core system is completely using two cores.|No Dimensions|
|PercentageCpuReady|Percentage CPU Ready|Milliseconds|Toplam|Ready time is the time spend waiting for CPU(s) to become available in the past update interval.|No Dimensions|

## <a name="microsoftwebserverfarms"></a>Microsoft.Web/serverfarms

|Ölçüm|Metric Display Name|Birim|Aggregation Type|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|CpuPercentage|CPU Yüzdesi|Yüzde|Ortalama|CPU Yüzdesi|Örnek|
|MemoryPercentage|Memory Percentage|Yüzde|Ortalama|Memory Percentage|Örnek|
|DiskQueueLength|Disk Queue Length|Sayı|Ortalama|Disk Queue Length|Örnek|
|HttpQueueLength|Http Queue Length|Sayı|Ortalama|Http Queue Length|Örnek|
|BytesReceived|Data In|Bytes|Toplam|Data In|Örnek|
|BytesSent|Data Out|Bytes|Toplam|Data Out|Örnek|

## <a name="microsoftwebsites-excluding-functions"></a>Microsoft.Web/sites (excluding functions)

|Ölçüm|Metric Display Name|Birim|Aggregation Type|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|CpuTime|CPU Time|Saniye|Toplam|CPU Time|Örnek|
|İstekler|İstekler|Sayı|Toplam|İstekler|Örnek|
|BytesReceived|Data In|Bytes|Toplam|Data In|Örnek|
|BytesSent|Data Out|Bytes|Toplam|Data Out|Örnek|
|Http101|Http 101|Sayı|Toplam|Http 101|Örnek|
|Http2xx|Http 2xx|Sayı|Toplam|Http 2xx|Örnek|
|Http3xx|Http 3xx|Sayı|Toplam|Http 3xx|Örnek|
|Http401|Http 401|Sayı|Toplam|Http 401|Örnek|
|Http403|Http 403|Sayı|Toplam|Http 403|Örnek|
|Http404|Http 404|Sayı|Toplam|Http 404|Örnek|
|Http406|Http 406|Sayı|Toplam|Http 406|Örnek|
|Http4xx|Http 4xx|Sayı|Toplam|Http 4xx|Örnek|
|Http5xx|Http Server Errors|Sayı|Toplam|Http Server Errors|Örnek|
|MemoryWorkingSet|Memory working set|Bytes|Ortalama|Memory working set|Örnek|
|AverageMemoryWorkingSet|Average memory working set|Bytes|Ortalama|Average memory working set|Örnek|
|AverageResponseTime|Average Response Time|Saniye|Ortalama|Average Response Time|Örnek|
|AppConnections|Bağlantılar|Sayı|Ortalama|Bağlantılar|Örnek|
|Handles|Handle Count|Sayı|Ortalama|Handle Count|Örnek|
|Threads|Thread Count|Sayı|Ortalama|Thread Count|Örnek|
|PrivateBytes|Private Bytes|Bytes|Ortalama|Private Bytes|Örnek|
|IoReadBytesPerSecond|IO Read Bytes Per Second|BytesPerSecond|Toplam|IO Read Bytes Per Second|Örnek|
|IoWriteBytesPerSecond|IO Write Bytes Per Second|BytesPerSecond|Toplam|IO Write Bytes Per Second|Örnek|
|IoOtherBytesPerSecond|IO Other Bytes Per Second|BytesPerSecond|Toplam|IO Other Bytes Per Second|Örnek|
|IoReadOperationsPerSecond|IO Read Operations Per Second|BytesPerSecond|Toplam|IO Read Operations Per Second|Örnek|
|IoWriteOperationsPerSecond|IO Write Operations Per Second|BytesPerSecond|Toplam|IO Write Operations Per Second|Örnek|
|IoOtherOperationsPerSecond|IO Other Operations Per Second|BytesPerSecond|Toplam|IO Other Operations Per Second|Örnek|
|RequestsInApplicationQueue|Requests In Application Queue|Sayı|Ortalama|Requests In Application Queue|Örnek|
|CurrentAssemblies|Current Assemblies|Sayı|Ortalama|Current Assemblies|Örnek|
|TotalAppDomains|Total App Domains|Sayı|Ortalama|Total App Domains|Örnek|
|TotalAppDomainsUnloaded|Total App Domains Unloaded|Sayı|Ortalama|Total App Domains Unloaded|Örnek|
|Gen0Collections|Gen 0 Garbage Collections|Sayı|Toplam|Gen 0 Garbage Collections|Örnek|
|Gen1Collections|Gen 1 Garbage Collections|Sayı|Toplam|Gen 1 Garbage Collections|Örnek|
|Gen2Collections|Gen 2 Garbage Collections|Sayı|Toplam|Gen 2 Garbage Collections|Örnek|

## <a name="microsoftwebsites-functions"></a>Microsoft.Web/sites (functions)

|Ölçüm|Metric Display Name|Birim|Aggregation Type|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|BytesReceived|Data In|Bytes|Toplam|Data In|Örnek|
|BytesSent|Data Out|Bytes|Toplam|Data Out|Örnek|
|Http5xx|Http Server Errors|Sayı|Toplam|Http Server Errors|Örnek|
|MemoryWorkingSet|Memory working set|Bytes|Ortalama|Memory working set|Örnek|
|AverageMemoryWorkingSet|Average memory working set|Bytes|Ortalama|Average memory working set|Örnek|
|FunctionExecutionUnits|Function Execution Units|MB / Milliseconds|Toplam|[Function Execution Units](https://github.com/Azure/Azure-Functions/wiki/Consumption-Plan-Cost-Billing-FAQ#how-can-i-view-graphs-of-execution-count-and-gb-seconds)|Örnek|
|FunctionExecutionCount|Function Execution Count|Sayı|Toplam|Function Execution Count|Örnek|
|PrivateBytes|Private Bytes|Bytes|Ortalama|Private Bytes|Örnek|
|IoReadBytesPerSecond|IO Read Bytes Per Second|BytesPerSecond|Toplam|IO Read Bytes Per Second|Örnek|
|IoWriteBytesPerSecond|IO Write Bytes Per Second|BytesPerSecond|Toplam|IO Write Bytes Per Second|Örnek|
|IoOtherBytesPerSecond|IO Other Bytes Per Second|BytesPerSecond|Toplam|IO Other Bytes Per Second|Örnek|
|IoReadOperationsPerSecond|IO Read Operations Per Second|BytesPerSecond|Toplam|IO Read Operations Per Second|Örnek|
|IoWriteOperationsPerSecond|IO Write Operations Per Second|BytesPerSecond|Toplam|IO Write Operations Per Second|Örnek|
|IoOtherOperationsPerSecond|IO Other Operations Per Second|BytesPerSecond|Toplam|IO Other Operations Per Second|Örnek|
|RequestsInApplicationQueue|Requests In Application Queue|Sayı|Ortalama|Requests In Application Queue|Örnek|
|CurrentAssemblies|Current Assemblies|Sayı|Ortalama|Current Assemblies|Örnek|
|TotalAppDomains|Total App Domains|Sayı|Ortalama|Total App Domains|Örnek|
|TotalAppDomainsUnloaded|Total App Domains Unloaded|Sayı|Ortalama|Total App Domains Unloaded|Örnek|
|Gen0Collections|Gen 0 Garbage Collections|Sayı|Toplam|Gen 0 Garbage Collections|Örnek|
|Gen1Collections|Gen 1 Garbage Collections|Sayı|Toplam|Gen 1 Garbage Collections|Örnek|
|Gen2Collections|Gen 2 Garbage Collections|Sayı|Toplam|Gen 2 Garbage Collections|Örnek|

## <a name="microsoftwebsitesslots"></a>Microsoft.Web/sites/slots

|Ölçüm|Metric Display Name|Birim|Aggregation Type|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|CpuTime|CPU Time|Saniye|Toplam|CPU Time|Örnek|
|İstekler|İstekler|Sayı|Toplam|İstekler|Örnek|
|BytesReceived|Data In|Bytes|Toplam|Data In|Örnek|
|BytesSent|Data Out|Bytes|Toplam|Data Out|Örnek|
|Http101|Http 101|Sayı|Toplam|Http 101|Örnek|
|Http2xx|Http 2xx|Sayı|Toplam|Http 2xx|Örnek|
|Http3xx|Http 3xx|Sayı|Toplam|Http 3xx|Örnek|
|Http401|Http 401|Sayı|Toplam|Http 401|Örnek|
|Http403|Http 403|Sayı|Toplam|Http 403|Örnek|
|Http404|Http 404|Sayı|Toplam|Http 404|Örnek|
|Http406|Http 406|Sayı|Toplam|Http 406|Örnek|
|Http4xx|Http 4xx|Sayı|Toplam|Http 4xx|Örnek|
|Http5xx|Http Server Errors|Sayı|Toplam|Http Server Errors|Örnek|
|MemoryWorkingSet|Memory working set|Bytes|Ortalama|Memory working set|Örnek|
|AverageMemoryWorkingSet|Average memory working set|Bytes|Ortalama|Average memory working set|Örnek|
|AverageResponseTime|Average Response Time|Saniye|Ortalama|Average Response Time|Örnek|
|FunctionExecutionUnits|Function Execution Units|Sayı|Toplam|Function Execution Units|Örnek|
|FunctionExecutionCount|Function Execution Count|Sayı|Toplam|Function Execution Count|Örnek|
|AppConnections|Bağlantılar|Sayı|Ortalama|Bağlantılar|Örnek|
|Handles|Handle Count|Sayı|Ortalama|Handle Count|Örnek|
|Threads|Thread Count|Sayı|Ortalama|Thread Count|Örnek|
|PrivateBytes|Private Bytes|Bytes|Ortalama|Private Bytes|Örnek|
|IoReadBytesPerSecond|IO Read Bytes Per Second|BytesPerSecond|Toplam|IO Read Bytes Per Second|Örnek|
|IoWriteBytesPerSecond|IO Write Bytes Per Second|BytesPerSecond|Toplam|IO Write Bytes Per Second|Örnek|
|IoOtherBytesPerSecond|IO Other Bytes Per Second|BytesPerSecond|Toplam|IO Other Bytes Per Second|Örnek|
|IoReadOperationsPerSecond|IO Read Operations Per Second|BytesPerSecond|Toplam|IO Read Operations Per Second|Örnek|
|IoWriteOperationsPerSecond|IO Write Operations Per Second|BytesPerSecond|Toplam|IO Write Operations Per Second|Örnek|
|IoOtherOperationsPerSecond|IO Other Operations Per Second|BytesPerSecond|Toplam|IO Other Operations Per Second|Örnek|
|RequestsInApplicationQueue|Requests In Application Queue|Sayı|Ortalama|Requests In Application Queue|Örnek|
|CurrentAssemblies|Current Assemblies|Sayı|Ortalama|Current Assemblies|Örnek|
|TotalAppDomains|Total App Domains|Sayı|Ortalama|Total App Domains|Örnek|
|TotalAppDomainsUnloaded|Total App Domains Unloaded|Sayı|Ortalama|Total App Domains Unloaded|Örnek|
|Gen0Collections|Gen 0 Garbage Collections|Sayı|Toplam|Gen 0 Garbage Collections|Örnek|
|Gen1Collections|Gen 1 Garbage Collections|Sayı|Toplam|Gen 1 Garbage Collections|Örnek|
|Gen2Collections|Gen 2 Garbage Collections|Sayı|Toplam|Gen 2 Garbage Collections|Örnek|

## <a name="microsoftwebhostingenvironmentsmultirolepools"></a>Microsoft.Web/hostingEnvironments/multiRolePools

|Ölçüm|Metric Display Name|Birim|Aggregation Type|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|İstekler|İstekler|Sayı|Toplam|İstekler|Örnek|
|BytesReceived|Data In|Bytes|Toplam|Data In|Örnek|
|BytesSent|Data Out|Bytes|Toplam|Data Out|Örnek|
|Http101|Http 101|Sayı|Toplam|Http 101|Örnek|
|Http2xx|Http 2xx|Sayı|Toplam|Http 2xx|Örnek|
|Http3xx|Http 3xx|Sayı|Toplam|Http 3xx|Örnek|
|Http401|Http 401|Sayı|Toplam|Http 401|Örnek|
|Http403|Http 403|Sayı|Toplam|Http 403|Örnek|
|Http404|Http 404|Sayı|Toplam|Http 404|Örnek|
|Http406|Http 406|Sayı|Toplam|Http 406|Örnek|
|Http4xx|Http 4xx|Sayı|Toplam|Http 4xx|Örnek|
|Http5xx|Http Server Errors|Sayı|Toplam|Http Server Errors|Örnek|
|AverageResponseTime|Average Response Time|Saniye|Ortalama|Average Response Time|Örnek|
|CpuPercentage|CPU Yüzdesi|Yüzde|Ortalama|CPU Yüzdesi|Örnek|
|MemoryPercentage|Memory Percentage|Yüzde|Ortalama|Memory Percentage|Örnek|
|DiskQueueLength|Disk Queue Length|Sayı|Ortalama|Disk Queue Length|Örnek|
|HttpQueueLength|Http Queue Length|Sayı|Ortalama|Http Queue Length|Örnek|
|ActiveRequests|Active Requests|Sayı|Toplam|Active Requests|Örnek|
|TotalFrontEnds|Total Front Ends|Sayı|Ortalama|Total Front Ends|No Dimensions|
|SmallAppServicePlanInstances|Small App Service Plan Workers|Sayı|Ortalama|Small App Service Plan Workers|No Dimensions|
|MediumAppServicePlanInstances|Medium App Service Plan Workers|Sayı|Ortalama|Medium App Service Plan Workers|No Dimensions|
|LargeAppServicePlanInstances|Large App Service Plan Workers|Sayı|Ortalama|Large App Service Plan Workers|No Dimensions|

## <a name="microsoftwebhostingenvironmentsworkerpools"></a>Microsoft.Web/hostingEnvironments/workerPools

|Ölçüm|Metric Display Name|Birim|Aggregation Type|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|WorkersTotal|Total Workers|Sayı|Ortalama|Total Workers|No Dimensions|
|WorkersAvailable|Available Workers|Sayı|Ortalama|Available Workers|No Dimensions|
|WorkersUsed|Used Workers|Sayı|Ortalama|Used Workers|No Dimensions|
|CpuPercentage|CPU Yüzdesi|Yüzde|Ortalama|CPU Yüzdesi|Örnek|
|MemoryPercentage|Memory Percentage|Yüzde|Ortalama|Memory Percentage|Örnek|

## <a name="next-steps"></a>Sonraki adımlar
* [Read about metrics in Azure Monitor](data-platform.md)
* [Create alerts on metrics](alerts-overview.md)
* [Export metrics to storage, Event Hub, or Log Analytics](resource-logs-overview.md)
