---
title: Azure'da Hizmet Dokusu kümesini izleme
description: Bu eğitimde, Service Fabric olaylarını görüntüleyerek, EventStore API'lerini sorgulayarak, perf sayaçlarını izleyerek ve sistem durumu raporlarını görüntüleyerek bir kümeyi nasıl izleyeceğinizi öğrenirsiniz.
author: srrengar
ms.topic: tutorial
ms.date: 07/22/2019
ms.author: srrengar
ms.custom: mvc
ms.openlocfilehash: ab58d622511e0d5793eb6df312bc3fd6dd15bfd6
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "75376639"
---
# <a name="tutorial-monitor-a-service-fabric-cluster-in-azure"></a>Öğretici: Azure'da Hizmet Dokusu kümesini izleme

İzleme ve tanılama, iş yüklerinin herhangi bir bulut ortamında geliştirilmesi, test edilmesi ve dağıtılması açısından önemlidir. Bu öğretici, bir serinin ikinci bölümüdür ve olayları, performans sayaçlarını ve sistem durumu raporlarını kullanarak Bir Hizmet Kumaşı kümesini nasıl izleyip tanılayabileceğinizi gösterir.   Daha fazla bilgi için [küme izleme](service-fabric-diagnostics-overview.md#platform-cluster-monitoring) ve [altyapı izleme](service-fabric-diagnostics-overview.md#infrastructure-performance-monitoring)hakkında genel bilgileri okuyun.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Servis Kumaşı etkinliklerini görüntüle
> * Küme olayları için EventStore API'lerini sorgula
> * Altyapıyı izleme/perf sayaçları toplama
> * Küme sağlık raporlarını görüntüleme

Bu öğretici dizisinde şunların nasıl yapıldığını öğrenirsiniz:
> [!div class="checklist"]
> * Şablon kullanarak Azure'da güvenli bir [Windows kümesi](service-fabric-tutorial-create-vnet-and-windows-cluster.md) oluşturma
> * Bir kümeyi izleme
> * [Bir kümenin ölçeğini daraltma veya genişletme](service-fabric-tutorial-scale-cluster.md)
> * [Bir kümenin çalışma zamanını yükseltme](service-fabric-tutorial-upgrade-cluster.md)
> * [Küme silme](service-fabric-tutorial-delete-cluster.md)


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiye başlamadan önce:

* Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun
* [Azure Powershell](https://docs.microsoft.com/powershell/azure/install-Az-ps) veya [Azure CLI'yi yükleyin.](/cli/azure/install-azure-cli)
* Güvenli bir [Windows kümesi](service-fabric-tutorial-create-vnet-and-windows-cluster.md) oluşturma 
* Küme için kurulum [tanılama koleksiyonu](service-fabric-tutorial-create-vnet-and-windows-cluster.md#configurediagnostics_anchor)
* [Kümedeki EventStore hizmetini](service-fabric-tutorial-create-vnet-and-windows-cluster.md#configureeventstore_anchor) etkinleştirme
* Azure Monitor günlüklerini ve küme için [Log Analytics aracısını](service-fabric-tutorial-create-vnet-and-windows-cluster.md#configureloganalytics_anchor) yapılandırma

## <a name="view-service-fabric-events-using-azure-monitor-logs"></a>Azure Monitör günlüklerini kullanarak Hizmet Kumaşı olaylarını görüntüleme

Azure Monitor günlükleri bulutta barındırılan uygulama ve hizmetlerden telemetri toplar ve analiz eder ve bunların kullanılabilirliğini ve performansını en üst düzeye çıkarmanıza yardımcı olacak analiz araçları sağlar. Öngörüler kazanmak ve kümenizde neler olup bittiğini gidermek için Azure Monitor günlüklerinde sorgular çalıştırabilirsiniz.

Service Fabric Analytics çözümüne erişmek için [Azure portalına](https://portal.azure.com) gidin ve Service Fabric Analytics çözümünü oluşturduğunuz kaynak grubunu seçin.

Kaynak **ServiceFabric(mysfomsworkspace)** seçin.

**Genel** Bakış'ta, biri Service Fabric için olmak üzere etkinleştirilen çözümlerin her biri için grafik şeklinde kutucuklar görürsünüz. Service **Fabric** Analytics çözümüne devam etmek için Service Fabric grafiğini tıklatın.

![Servis Kumaş çözümü](media/service-fabric-tutorial-monitor-cluster/oms-service-fabric-summary.png)

Aşağıdaki resimde Service Fabric Analytics çözümünün ana sayfası gösterilmektedir. Bu ana sayfa, kümenizde neler olup bittiğine dair anlık görüntü sağlar.

![Servis Kumaş çözümü](media/service-fabric-tutorial-monitor-cluster/oms-service-fabric-solution.png)

 Küme oluşturma üzerine tanılamayı etkinleştirdiyseniz, 

* [Servis Kumaş küme etkinlikleri](service-fabric-diagnostics-event-generation-operational.md)
* [Güvenilir Aktörler programlama modeli olaylar](service-fabric-reliable-actors-diagnostics.md)
* [Güvenilir Hizmetler programlama modeli olaylar](service-fabric-reliable-services-diagnostics.md)

>[!NOTE]
>Kutunun dışında Hizmet Kumaş olaylara ek olarak, daha ayrıntılı sistem olayları [tanılama uzantısı config güncelleyerek](service-fabric-diagnostics-event-aggregation-wad.md#log-collection-configurations)toplanabilir.

### <a name="view-service-fabric-events-including-actions-on-nodes"></a>Düğümlerle ilgili eylemler de dahil olmak üzere Hizmet Kumaş Etkinliklerini görüntüleyin

Service Fabric Analytics sayfasında, **Küme Olayları**için grafiğe tıklayın.  Toplanan tüm sistem olaylarının günlükleri görüntülenir. Başvuru için bunlar Azure Depolama hesabındaki **WADServiceFabricSystemEventsTable'dan** gelir ve benzer şekilde bir sonraki gördüğünüz güvenilir hizmetler ve aktörler etkinlikleri de ilgili tablolardan dır.
    
![Operasyonel Kanalı Sorgula](media/service-fabric-tutorial-monitor-cluster/oms-service-fabric-events.png)

Sorgu, aradığınızı hassaslaştırmak için değiştirebileceğiniz Kusto sorgu dilini kullanır. Örneğin, kümedeki düğümler üzerinde yapılan tüm eylemleri bulmak için aşağıdaki sorguyu kullanabilirsiniz. Aşağıda kullanılan olay işlleri [operasyonel kanal olayları](service-fabric-diagnostics-event-generation-operational.md)referansında bulunur.

```kusto
ServiceFabricOperationalEvent
| where EventId < 25627 and EventId > 25619 
```

Kusto sorgu dili güçlüdür. Diğer bazı yararlı sorgular aşağıda veda eder.

Query'yi ServiceFabricEvent takma adıyla bir işlev olarak kaydederek kullanıcı tanımlı işlev olarak *ServiceFabricEvent* arama tablosu oluşturun:

```kusto
let ServiceFabricEvent = datatable(EventId: int, EventName: string)
[
    ...
    18603, 'NodeUpOperational',
    18604, 'NodeDownOperational',
    ...
];
ServiceFabricEvent
```

Son bir saat içinde kaydedilen operasyonel olayları döndürme:
```kusto
ServiceFabricOperationalEvent
| where TimeGenerated > ago(1h)
| join kind=leftouter ServiceFabricEvent on EventId
| project EventId, EventName, TaskName, Computer, ApplicationName, EventMessage, TimeGenerated
| sort by TimeGenerated
```

EventId == 18604 ve EventName == 'NodeDownOperational' ile operasyonel olayları döndür:
```kusto
ServiceFabricOperationalEvent
| where EventId == 18604
| project EventId, EventName = 'NodeDownOperational', TaskName, Computer, EventMessage, TimeGenerated
| sort by TimeGenerated 
```

EventId == 18604 ve EventName == 'NodeUpOperational' ile operasyonel olayları döndür:
```kusto
ServiceFabricOperationalEvent
| where EventId == 18603
| project EventId, EventName = 'NodeUpOperational', TaskName, Computer, EventMessage, TimeGenerated
| sort by TimeGenerated 
``` 
 
Sağlık Raporları'nı HealthState == 3 (Hata) ile döndürür ve EventMessage alanından ek özellikler ayıklayın:

```kusto
ServiceFabricOperationalEvent
| join kind=leftouter ServiceFabricEvent on EventId
| extend HealthStateId = extract(@"HealthState=(\S+) ", 1, EventMessage, typeof(int))
| where TaskName == 'HM' and HealthStateId == 3
| extend SourceId = extract(@"SourceId=(\S+) ", 1, EventMessage, typeof(string)),
         Property = extract(@"Property=(\S+) ", 1, EventMessage, typeof(string)),
         HealthState = case(HealthStateId == 0, 'Invalid', HealthStateId == 1, 'Ok', HealthStateId == 2, 'Warning', HealthStateId == 3, 'Error', 'Unknown'),
         TTL = extract(@"TTL=(\S+) ", 1, EventMessage, typeof(string)),
         SequenceNumber = extract(@"SequenceNumber=(\S+) ", 1, EventMessage, typeof(string)),
         Description = extract(@"Description='([\S\s, ^']+)' ", 1, EventMessage, typeof(string)),
         RemoveWhenExpired = extract(@"RemoveWhenExpired=(\S+) ", 1, EventMessage, typeof(bool)),
         SourceUTCTimestamp = extract(@"SourceUTCTimestamp=(\S+)", 1, EventMessage, typeof(datetime)),
         ApplicationName = extract(@"ApplicationName=(\S+) ", 1, EventMessage, typeof(string)),
         ServiceManifest = extract(@"ServiceManifest=(\S+) ", 1, EventMessage, typeof(string)),
         InstanceId = extract(@"InstanceId=(\S+) ", 1, EventMessage, typeof(string)),
         ServicePackageActivationId = extract(@"ServicePackageActivationId=(\S+) ", 1, EventMessage, typeof(string)),
         NodeName = extract(@"NodeName=(\S+) ", 1, EventMessage, typeof(string)),
         Partition = extract(@"Partition=(\S+) ", 1, EventMessage, typeof(string)),
         StatelessInstance = extract(@"StatelessInstance=(\S+) ", 1, EventMessage, typeof(string)),
         StatefulReplica = extract(@"StatefulReplica=(\S+) ", 1, EventMessage, typeof(string))
```

EventId != 17523 ile olayların bir zaman çizelgesi ni döndürün:

```kusto
ServiceFabricOperationalEvent
| join kind=leftouter ServiceFabricEvent on EventId
| where EventId != 17523
| summarize Count = count() by Timestamp = bin(TimeGenerated, 1h), strcat(tostring(EventId), " - ", case(EventName != "", EventName, "Unknown"))
| render timechart 
```

Servis Kumaşı operasyonel etkinliklerini belirli hizmet ve düğümle bir araya getirin:

```kusto
ServiceFabricOperationalEvent
| where ApplicationName  != "" and ServiceName != ""
| summarize AggregatedValue = count() by ApplicationName, ServiceName, Computer 
```

Çapraz kaynak sorgusunu kullanarak EventId / EventName'ye göre Hizmet Kumaşı olaylarının sayısını oluşturma:

```kusto
app('PlunkoServiceFabricCluster').traces
| where customDimensions.ProviderName == 'Microsoft-ServiceFabric'
| extend EventId = toint(customDimensions.EventId), TaskName = tostring(customDimensions.TaskName)
| where EventId != 17523
| join kind=leftouter ServiceFabricEvent on EventId
| extend EventName = case(EventName != '', EventName, 'Undocumented')
| summarize ["Event Count"]= count() by bin(timestamp, 30m), EventName = strcat(tostring(EventId), " - ", EventName)
| render timechart
```

### <a name="view-service-fabric-application-events"></a>Servis Kumaşı uygulama olaylarını görüntüle

Kümede dağıtılan güvenilir hizmetler ve güvenilir aktörler uygulamaları için olayları görüntüleyebilirsiniz.  Hizmet Kumaş Analizi sayfasında, **Uygulama Etkinlikleri**için grafiği tıklatın.

Güvenilir hizmet uygulamalarınızdaki olayları görüntülemek için aşağıdaki sorguyu çalıştırın:
```kusto
ServiceFabricReliableServiceEvent
| sort by TimeGenerated desc
```

Hizmet runasync başlatıldığında ve genellikle dağıtımlar ve yükseltmeleri olur tamamlandığında için farklı olaylar görebilirsiniz.

![Servis Kumaş Çözümü Güvenilir Hizmetler](media/service-fabric-tutorial-monitor-cluster/oms-reliable-services-events-selection.png)

ServiceName == "kumaş:/Watchdog/WatchdogService" ile güvenilir hizmet için etkinlikler de bulabilirsiniz:

```kusto
ServiceFabricReliableServiceEvent
| where ServiceName == "fabric:/Watchdog/WatchdogService"
| project TimeGenerated, EventMessage
| order by TimeGenerated desc  
```
 
Güvenilir aktör olayları benzer bir şekilde görülebilir:

```kusto
ServiceFabricReliableActorEvent
| sort by TimeGenerated desc
```
Güvenilir aktörler için daha ayrıntılı olayları yapılandırmak `scheduledTransferKeywordFilter` için, küme şablonundaki tanı uzantısı için config'dekini değiştirebilirsiniz. Bunlar için değerler ayrıntıları [güvenilir aktörler olaylar referans](service-fabric-reliable-actors-diagnostics.md#keywords)bulunmaktadır.

```json
"EtwEventSourceProviderConfiguration": [
                {
                    "provider": "Microsoft-ServiceFabric-Actors",
                    "scheduledTransferKeywordFilter": "1",
                    "scheduledTransferPeriod": "PT5M",
                    "DefaultEvents": {
                    "eventDestination": "ServiceFabricReliableActorEventTable"
                    }
                },
```

## <a name="view-performance-counters-with-azure-monitor-logs"></a>Azure Monitor günlükleriyle performans sayaçlarını görüntüleme
Performans sayaçlarını görüntülemek için [Azure portalına](https://portal.azure.com) ve Service Fabric Analytics çözümlerini oluşturduğunuz kaynak grubuna gidin. 

Kaynak **ServiceFabric(mysfomsworkspace) seçin,** sonra **Log Analytics Workspace**, ve sonra **Gelişmiş Ayarlar**.

**Veri'yi**tıklatın, ardından **Windows Performans Sayaçları'nı**tıklatın. Etkinleştirmeyi seçebileceğiniz varsayılan sayaçların bir listesi vardır ve toplama aralığını da ayarlayabilirsiniz. Toplamak için [ek performans sayaçları](service-fabric-diagnostics-event-generation-perf.md) da ekleyebilirsiniz. Uygun biçim bu [makalede](/windows/desktop/PerfCtrs/specifying-a-counter-path)başvurulan . **Kaydet'i**tıklatın, ardından **Tamam'ı**tıklatın.

Gelişmiş Ayarlar bıçağını kapatın ve **Genel** başlık altında **Çalışma Alanı özetini** seçin. Etkinleştirilen çözümlerin her biri için, biri Service Fabric için olmak üzere bir grafik döşeme satrvardır. Service **Fabric** Analytics çözümüne devam etmek için Service Fabric grafiğini tıklatın.

Operasyonel kanal ve güvenilir hizmet etkinlikleri için grafik karolar vardır. Seçtiğiniz sayaçlar için akan verilerin grafik gösterimi **Düğüm Ölçümleri**altında görünür. 

Ek ayrıntıları görmek için **Kapsayıcı Metrik** grafiğini seçin. Ayrıca, Kusto sorgu dilini kullanarak performans sayacı verilerini kümeleme olaylarına benzer şekilde sorgulayabilir ve düğümlerde, perf sayacı adı ve değerleri filtreleyebilirsiniz.

## <a name="query-the-eventstore-service"></a>EventStore hizmetini sorgula
[EventStore hizmeti,](service-fabric-diagnostics-eventstore.md) kümenizin durumunu veya iş yüklerinizi belirli bir zamanda anlamanın bir yolunu sağlar. EventStore, kümedeki olayları koruyan, devlete hizmet veren bir Hizmet Kumaşı hizmetidir. Olaylar Service Fabric [Explorer,](service-fabric-visualizing-your-cluster.md)REST ve API'ler aracılığıyla ortaya çıkarır. EventStore, kümenizdeki herhangi bir varlıkla ilgili tanılama verilerini almak için kümeyi doğrudan sorgular EventStore'da bulunan olayların tam listesini görmek için [Service Fabric olaylarına](service-fabric-diagnostics-event-generation-operational.md)bakın.

EventStore API'leri, [Service Fabric istemci kitaplığı](/dotnet/api/overview/azure/service-fabric?view=azure-dotnet#client-library)kullanılarak programlı olarak sorgulanabilir.

GetClusterEventListAsync fonksiyonu üzerinden 2018-04-03T18:00:00:00Z ve 2018-04-04T18:00:00Z arasındaki tüm küme olayları için örnek bir istek aşağıda verilmiştir.

```csharp
var sfhttpClient = ServiceFabricClientFactory.Create(clusterUrl, settings);

var clstrEvents = sfhttpClient.EventsStore.GetClusterEventListAsync(
    "2018-04-03T18:00:00Z",
    "2018-04-04T18:00:00Z")
    .GetAwaiter()
    .GetResult()
    .ToList();
```

Burada, Eylül 2018'deki küme durumu ve tüm düğüm olaylarını sorgulayan ve bunları yazdıran başka bir örnek daha verilmiştir.

```csharp
const int timeoutSecs = 60;
var clusterUrl = new Uri(@"http://localhost:19080"); // This example is for a Local cluster
var sfhttpClient = ServiceFabricClientFactory.Create(clusterUrl);

var clusterHealth = sfhttpClient.Cluster.GetClusterHealthAsync().GetAwaiter().GetResult();
Console.WriteLine("Cluster Health: {0}", clusterHealth.AggregatedHealthState.Value.ToString());


Console.WriteLine("Querying for node events...");
var nodesEvents = sfhttpClient.EventsStore.GetNodesEventListAsync(
    "2018-09-01T00:00:00Z",
    "2018-09-30T23:59:59Z",
    timeoutSecs,
    "NodeDown,NodeUp")
    .GetAwaiter()
    .GetResult()
    .ToList();
Console.WriteLine("Result Count: {0}", nodesEvents.Count());

foreach (var nodeEvent in nodesEvents)
{
    Console.Write("Node event happened at {0}, Node name: {1} ", nodeEvent.TimeStamp, nodeEvent.NodeName);
    if (nodeEvent is NodeDownEvent)
    {
        var nodeDownEvent = nodeEvent as NodeDownEvent;
        Console.WriteLine("(Node is down, and it was last up at {0})", nodeDownEvent.LastNodeUpAt);
    }
    else if (nodeEvent is NodeUpEvent)
    {
        var nodeUpEvent = nodeEvent as NodeUpEvent;
        Console.WriteLine("(Node is up, and it was last down at {0})", nodeUpEvent.LastNodeDownAt);
    }
}
```


## <a name="monitor-cluster-health"></a>Küme durumunu izleme
Service Fabric, sistem bileşenlerinin ve izleme köpeklerinin izdiklerini yerel koşulları bildirebildiği sağlık kuruluşlarıyla bir [sağlık modeli](service-fabric-health-introduction.md) sunar. [Sistem durumu deposu,](service-fabric-health-introduction.md#health-store) varlıkların sağlıklı olup olmadığını belirlemek için tüm sistem durumu verilerini toplar.

Küme, sistem bileşenleri tarafından gönderilen sistem raporlarıyla otomatik olarak doldurulur. [Sorun gidermek için sistem sağlık raporlarını kullan'da](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)daha fazla bilgi edinin.

Service Fabric, desteklenen [varlık türlerinin](service-fabric-health-introduction.md#health-entities-and-hierarchy)her biri için sistem durumu sorgularını ortaya çıkarır. [FabricClient.HealthManager,](/dotnet/api/system.fabric.fabricclient.healthmanager?view=azure-dotnet)PowerShell cmdlets ve REST üzerindeki yöntemler kullanılarak API üzerinden erişilebilirler. Bu sorgular varlık la ilgili tam sağlık bilgilerini döndürmektedir: toplu sağlık durumu, varlık sağlık olayları, çocuk sağlığı durumları (varsa), sağlıksız değerlendirmeler (varlık sağlıklı olmadığında) ve çocuk sağlığı istatistikleri ( uygulanabilir).

### <a name="get-cluster-health"></a>Küme sağlığı nı elde edin
[Get-ServiceFabricClusterHealth cmdlet](/powershell/module/servicefabric/get-servicefabricclusterhealth) küme varlığının durumunu döndürür ve uygulamaların ve düğümlerin (kümenin alt ları) sistem durumu durumunu içerir.  İlk olarak [Connect-ServiceFabricCluster cmdlet'i](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps)kullanarak kümeye bağlanın.

Kümenin durumu 11 düğüm, sistem uygulaması ve kumaş:/Oylama açıklandığı gibi yapılandırılmıştır.

Aşağıdaki örnekte varsayılan sistem durumu ilkeleri kullanarak küme durumu alır. 11 düğümleri sağlıklıdır, ancak küme toplanmış sistem durumu hatadır çünkü kumaş:/Oylama uygulaması Hata'dadır. Sağlıksız değerlendirmelerin toplu sağlığı tetikleyen koşullar hakkında nasıl ayrıntılı bilgi verdiğine dikkat edin.

```powershell
Get-ServiceFabricClusterHealth

AggregatedHealthState   : Error
UnhealthyEvaluations    : 
                          100% (1/1) applications are unhealthy. The evaluation tolerates 0% unhealthy applications.
                          
                          Application 'fabric:/Voting' is in Error.
                          
                            33% (1/3) deployed applications are unhealthy. The evaluation tolerates 0% unhealthy deployed applications.
                          
                            Deployed application on node '_nt2vm_3' is in Error.
                          
                                50% (1/2) deployed service packages are unhealthy.
                          
                                Service package for manifest 'VotingWebPkg' and service package activation ID '8723eb73-9b83-406b-9de3-172142ba15f3' is in Error.
                          
                                    'System.Hosting' reported Error for property 'CodePackageActivation:Code:SetupEntryPoint:131959376195593305'.
                                    There was an error during CodePackage activation.The service host terminated with exit code:1
                          
                          
NodeHealthStates        : 
                          NodeName              : _nt2vm_3
                          AggregatedHealthState : Ok
                          
                          NodeName              : _nt1vm_4
                          AggregatedHealthState : Ok
                          
                          NodeName              : _nt2vm_2
                          AggregatedHealthState : Ok
                          
                          NodeName              : _nt1vm_3
                          AggregatedHealthState : Ok
                          
                          NodeName              : _nt2vm_1
                          AggregatedHealthState : Ok
                          
                          NodeName              : _nt1vm_2
                          AggregatedHealthState : Ok
                          
                          NodeName              : _nt2vm_0
                          AggregatedHealthState : Ok
                          
                          NodeName              : _nt1vm_1
                          AggregatedHealthState : Ok
                          
                          NodeName              : _nt1vm_0
                          AggregatedHealthState : Ok
                          
                          NodeName              : _nt3vm_0
                          AggregatedHealthState : Ok
                          
                          NodeName              : _nt2vm_4
                          AggregatedHealthState : Ok
                          
ApplicationHealthStates : 
                          ApplicationName       : fabric:/System
                          AggregatedHealthState : Ok
                          
                          ApplicationName       : fabric:/Voting
                          AggregatedHealthState : Error
                          
HealthEvents            : None
HealthStatistics        : 
                          Node                  : 11 Ok, 0 Warning, 0 Error
                          Replica               : 4 Ok, 0 Warning, 0 Error
                          Partition             : 2 Ok, 0 Warning, 0 Error
                          Service               : 2 Ok, 0 Warning, 0 Error
                          DeployedServicePackage : 3 Ok, 1 Warning, 1 Error
                          DeployedApplication   : 1 Ok, 1 Warning, 1 Error
                          Application           : 0 Ok, 0 Warning, 1 Error
```

Aşağıdaki örnek, özel bir uygulama ilkesi kullanarak kümenin durumunu alır. Yalnızca uygulamaları ve düğümleri hata veya uyarı olarak almak için sonuçları filtreler. Bu örnekte düğümler, hepsi sağlıklı olduğu için döndürülür. Yalnızca kumaş:/Oylama uygulaması uygulamalar filtresine saygı duyar. Özel ilke, uyarıları kumaş:/Voting uygulaması için hata olarak dikkate almak üzere belirtildiğinden, uygulama hata olarak değerlendirilir ve küme de öyle.

```powershell
$appHealthPolicy = New-Object -TypeName System.Fabric.Health.ApplicationHealthPolicy
$appHealthPolicy.ConsiderWarningAsError = $true
$appHealthPolicyMap = New-Object -TypeName System.Fabric.Health.ApplicationHealthPolicyMap
$appUri1 = New-Object -TypeName System.Uri -ArgumentList "fabric:/Voting"
$appHealthPolicyMap.Add($appUri1, $appHealthPolicy)
Get-ServiceFabricClusterHealth -ApplicationHealthPolicyMap $appHealthPolicyMap -ApplicationsFilter "Warning,Error" -NodesFilter "Warning,Error" -ExcludeHealthStatistics

AggregatedHealthState   : Error
UnhealthyEvaluations    : 
                          100% (1/1) applications are unhealthy. The evaluation tolerates 0% unhealthy applications.
                          
                          Application 'fabric:/Voting' is in Error.
                          
                            100% (5/5) deployed applications are unhealthy. The evaluation tolerates 0% unhealthy deployed applications.
                          
                            Deployed application on node '_nt2vm_3' is in Error.
                          
                                50% (1/2) deployed service packages are unhealthy.
                          
                                Service package for manifest 'VotingWebPkg' and service package activation ID '8723eb73-9b83-406b-9de3-172142ba15f3' is in Error.
                          
                                    'System.Hosting' reported Error for property 'CodePackageActivation:Code:SetupEntryPoint:131959376195593305'.
                                    There was an error during CodePackage activation.The service host terminated with exit code:1
                          
                            Deployed application on node '_nt2vm_2' is in Error.
                          
                                50% (1/2) deployed service packages are unhealthy.
                          
                                Service package for manifest 'VotingWebPkg' and service package activation ID '2466f2f9-d5fd-410c-a6a4-5b1e00630cca' is in Error.
                          
                                    'System.Hosting' reported Error for property 'CodePackageActivation:Code:SetupEntryPoint:131959376486201388'.
                                    There was an error during CodePackage activation.The service host terminated with exit code:1
                          
                            Deployed application on node '_nt2vm_4' is in Error.
                          
                                100% (1/1) deployed service packages are unhealthy.
                          
                                Service package for manifest 'VotingWebPkg' and service package activation ID '5faa5201-eede-400a-865f-07f7f886aa32' is in Error.
                          
                                    'System.Hosting' reported Warning for property 'CodePackageActivation:Code:SetupEntryPoint:131959376207396204'. The evaluation treats 
                          Warning as Error.
                                    There was an error during CodePackage activation.The service host terminated with exit code:1
                          
                            Deployed application on node '_nt2vm_0' is in Error.
                          
                                100% (1/1) deployed service packages are unhealthy.
                          
                                Service package for manifest 'VotingWebPkg' and service package activation ID '204f1783-f774-4f3a-b371-d9983afaf059' is in Error.
                          
                                    'System.Hosting' reported Error for property 'CodePackageActivation:Code:SetupEntryPoint:131959375885791093'.
                                    There was an error during CodePackage activation.The service host terminated with exit code:1
                          
                            Deployed application on node '_nt3vm_0' is in Error.
                          
                                50% (1/2) deployed service packages are unhealthy.
                          
                                Service package for manifest 'VotingWebPkg' and service package activation ID '2533ae95-2d2a-4f8b-beef-41e13e4c0081' is in Error.
                          
                                    'System.Hosting' reported Error for property 'CodePackageActivation:Code:SetupEntryPoint:131959376108346272'.
                                    There was an error during CodePackage activation.The service host terminated with exit code:1                         
                          
NodeHealthStates        : None
ApplicationHealthStates : 
                          ApplicationName       : fabric:/Voting
                          AggregatedHealthState : Error
                          
HealthEvents            : None
```

### <a name="get-node-health"></a>Düğüm sağlığı alın
[Get-ServiceFabricNodeHealth cmdlet](/powershell/module/servicefabric/get-servicefabricnodehealth) bir düğüm varlığının durumunu döndürür ve düğümüzerinde bildirilen sağlık olaylarını içerir. İlk olarak [Connect-ServiceFabricCluster cmdlet](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps)kullanarak kümeye bağlanın. Aşağıdaki örnekte, varsayılan sistem durumu ilkelerini kullanarak belirli bir düğümün sistem durumu alır:

```powershell
Get-ServiceFabricNodeHealth _nt1vm_3
```

Aşağıdaki örnek kümedeki tüm düğümlerin durumunu alır:
```powershell
Get-ServiceFabricNode | Get-ServiceFabricNodeHealth | select NodeName, AggregatedHealthState | ft -AutoSize
```

### <a name="get-system-service-health"></a>Sistem hizmeti nin sağlığını elde edin 

Sistem hizmetlerinin toplu sağlık durumunu elde edin:

```powershell
Get-ServiceFabricService -ApplicationName fabric:/System | Get-ServiceFabricServiceHealth | select ServiceName, AggregatedHealthState | ft -AutoSize
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Servis Kumaşı etkinliklerini görüntüle
> * Küme olayları için EventStore API'lerini sorgula
> * Altyapıyı izleme/perf sayaçları toplama
> * Küme sağlık raporlarını görüntüleme

Ardından, kümeyi nasıl ölçeklendireceklerini öğrenmek için aşağıdaki öğreticiye ilerleyin.
> [!div class="nextstepaction"]
> [Kümeyi ölçeklendirme](service-fabric-tutorial-scale-cluster.md)

[durability]: service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster
[template]: https://github.com/Azure-Samples/service-fabric-cluster-templates/blob/master/7-VM-Windows-3-NodeTypes-Secure-NSG/AzureDeploy.json
