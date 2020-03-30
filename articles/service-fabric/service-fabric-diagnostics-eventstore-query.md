---
title: EventStore API'lerini kullanarak küme olayları için sorgu
description: Platform etkinliklerini sorgulamak için Azure Hizmet Kumaşı EventStore API'lerini nasıl kullanacağınızı öğrenin
author: srrengar
ms.topic: conceptual
ms.date: 02/25/2019
ms.author: srrengar
ms.openlocfilehash: 48350caef6bdaafda9aff7ac776d67b314aeaf8c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75614409"
---
# <a name="query-eventstore-apis-for-cluster-events"></a>Küme olayları için EventStore API'lerini sorgula

Bu makalede, Service Fabric sürüm 6.2 ve sonraki sürümlerde bulunan EventStore API'lerinin nasıl sorgulanacağını kapsar - EventStore hizmeti hakkında daha fazla bilgi edinmek [istiyorsanız, EventStore hizmetine genel bakışa](service-fabric-diagnostics-eventstore.md)bakın. Şu anda, EventStore hizmeti yalnızca son 7 gün için verilere erişebilir (bu, kümenizin tanılama veri saklama ilkesine bağlıdır).

>[!NOTE]
>EventStore API'leri, Yalnızca Azure'da çalışan Windows kümeleri için Service Fabric sürüm 6.4 itibariyle GA'dır.

EventStore API'leri doğrudan bir REST bitiş noktası üzerinden veya programlı olarak erişilebilir. Sorguya bağlı olarak, doğru verileri toplamak için gereken çeşitli parametreler vardır. Bu parametreler genellikle şunlardır:
* `api-version`: kullanmakta olduğunuz EventStore API'lerinin sürümü
* `StartTimeUtc`: bakmak istediğiniz dönemin başlangıcını tanımlar
* `EndTimeUtc`: zaman diliminin sonu

Bu parametrelere ek olarak, aşağıdakiler gibi isteğe bağlı parametreler de mevcuttur:
* `timeout`: istek işlemini gerçekleştirmek için varsayılan 60 saniyelik zaman asini geçersiz kılmak
* `eventstypesfilter`: bu size belirli olay türleri için filtreleme seçeneği sunar
* `ExcludeAnalysisEvents`: 'Analiz' olaylarını döndürmeyin. Varsayılan olarak, EventStore sorguları mümkün olduğunda "çözümleme" olayları ile geri döner. Analiz olayları, normal bir Hizmet Kumaşı olayının ötesinde ek bağlam veya bilgi içeren ve daha fazla derinlik sağlayan daha zengin operasyonel kanal olaylarıdır.
* `SkipCorrelationLookup`: kümedeki olası ilişkili olayları aretmeyin. Varsayılan olarak, EventStore bir kümedeki olayları ilişkilendirmeye çalışır ve mümkün olduğunda olayları birbirine bağlar. 

Kümedeki her varlık, olaylar için sorguolabilir. Ayrıca, türün tüm varlıkları için olayları sorgulayabilirsiniz. Örneğin, belirli bir düğüm için veya kümenizdeki tüm düğümler için olayları sorgulayabilirsiniz. Olaylar için sorgulayabildiğiniz geçerli varlıklar kümesi (sorgunun nasıl yapılandırıştırılaşacağı ile ilgilidir):
* Küme:`/EventsStore/Cluster/Events`
* Düğüm:`/EventsStore/Nodes/Events`
* Düğüm:`/EventsStore/Nodes/<NodeName>/$/Events`
* Uygulama:`/EventsStore/Applications/Events`
* Uygulama:`/EventsStore/Applications/<AppName>/$/Events`
* Hizmetleri:`/EventsStore/Services/Events`
* Hizmet:`/EventsStore/Services/<ServiceName>/$/Events`
* Bölüm:`/EventsStore/Partitions/Events`
* Bölüm:`/EventsStore/Partitions/<PartitionID>/$/Events`
* Yineleme:`/EventsStore/Partitions/<PartitionID>/$/Replicas/Events`
* Çoğaltma:`/EventsStore/Partitions/<PartitionID>/$/Replicas/<ReplicaID>/$/Events`

>[!NOTE]
>Bir uygulama veya hizmet adına başvururken, sorgunun "kumaş:/" Önek. Ayrıca, uygulamanızın veya hizmet adlarınızın içinde "/" varsa, sorguyu çalışır durumda tutmak için "~" olarak değiştirin. Örneğin, uygulamanız "kumaş:/App1/FrontendApp" olarak ortaya çıkarsa, uygulamaya özel sorgularınız `/EventsStore/Applications/App1~FrontendApp/$/Events`.
>Ayrıca, bugün hizmetlere ait sağlık raporları ilgili uygulama altında `DeployedServiceHealthReportCreated` gösteriş, böylece doğru uygulama varlığı için olayları sorgularsınız. 

## <a name="query-the-eventstore-via-rest-api-endpoints"></a>REST API uç noktaları üzerinden EventStore'u sorgulayın

EventStore'u doğrudan bir REST bitiş noktası `GET` üzerinden sorgulayarak aşağıdaki lere istekte bulunabilirsiniz: `<your cluster address>/EventsStore/<entity>/Events/`.

Örneğin, tüm Cluster olaylarını `2018-04-03T18:00:00Z` `2018-04-04T18:00:00Z`sorgulamak için isteğiniz şu şekilde görünür:

```
Method: GET 
URL: http://mycluster:19080/EventsStore/Cluster/Events?api-version=6.4&StartTimeUtc=2018-04-03T18:00:00Z&EndTimeUtc=2018-04-04T18:00:00Z
```

Bu, hiçbir olayı veya json'da döndürülen olayların listesini döndürebilir:

```json
Response: 200
Body:
[
  {
    "Kind": "ClusterUpgradeStart",
    "CurrentClusterVersion": "0.0.0.0:",
    "TargetClusterVersion": "6.2:1.0",
    "UpgradeType": "Rolling",
    "RollingUpgradeMode": "UnmonitoredAuto",
    "FailureAction": "Manual",
    "EventInstanceId": "090add3c-8f56-4d35-8d57-a855745b6064",
    "TimeStamp": "2018-04-03T20:18:59.4313064Z",
    "HasCorrelatedEvents": false
  },
  {
    "Kind": "ClusterUpgradeDomainComplete",
    "TargetClusterVersion": "6.2:1.0",
    "UpgradeState": "RollingForward",
    "UpgradeDomains": "(0 1 2)",
    "UpgradeDomainElapsedTimeInMs": "78.5288",
    "EventInstanceId": "090add3c-8f56-4d35-8d57-a855745b6064",
    "TimeStamp": "2018-04-03T20:19:59.5729953Z",
    "HasCorrelatedEvents": false
  },
  {
    "Kind": "ClusterUpgradeDomainComplete",
    "TargetClusterVersion": "6.2:1.0",
    "UpgradeState": "RollingForward",
    "UpgradeDomains": "(3 4)",
    "UpgradeDomainElapsedTimeInMs": "0",
    "EventInstanceId": "090add3c-8f56-4d35-8d57-a855745b6064",
    "TimeStamp": "2018-04-03T20:20:59.6271949Z",
    "HasCorrelatedEvents": false
  },
  {
    "Kind": "ClusterUpgradeComplete",
    "TargetClusterVersion": "6.2:1.0",
    "OverallUpgradeElapsedTimeInMs": "120196.5212",
    "EventInstanceId": "090add3c-8f56-4d35-8d57-a855745b6064",
    "TimeStamp": "2018-04-03T20:20:59.8134457Z",
    "HasCorrelatedEvents": false
  }
]
```

Burada, bu küme `2018-04-03T18:00:00Z` `2018-04-04T18:00:00Z`nin ilk yükseltmesini başarıyla tamamladığını görebiliriz. `"CurrentClusterVersion": "0.0.0.0:"` `"TargetClusterVersion": "6.2:1.0"` `"OverallUpgradeElapsedTimeInMs": "120196.5212"`

## <a name="query-the-eventstore-programmatically"></a>EventStore'u programlı olarak sorgulayın

[Ayrıca, Service Fabric istemci kitaplığı](https://docs.microsoft.com/dotnet/api/overview/azure/service-fabric?view=azure-dotnet#client-library)aracılığıyla EventStore'u programlı olarak sorgulayabilirsiniz.

Service Fabric Client'ınızı ayarladıktan sonra EventStore'a şu şekilde erişerek olayları sorgulayabilirsiniz:`sfhttpClient.EventStore.<request>`

Burada, işlev üzerinden `2018-04-03T18:00:00Z` ve `2018-04-04T18:00:00Z`arasında tüm küme olayları için örnek bir istek verilmiştir. `GetClusterEventListAsync`

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

## <a name="sample-scenarios-and-queries"></a>Örnek senaryolar ve sorgular

Kümenizin durumunu anlamak için Event Store REST API'lerini nasıl arayabilirsiniz birkaç örnek aşağıda verilmiştir.

*Küme yükseltmeleri:*

Kümenizin geçen hafta en son ne zaman başarılı bir şekilde yükseltilmediğini veya yükseltilmeye çalışıldığını görmek için, EventStore'daki "ClusterUpgradeCompleted" olaylarını sorgulayarak kümenize yapılan son yükseltmeler için API'leri sorgulayabilirsiniz:`https://mycluster.cloudapp.azure.com:19080/EventsStore/Cluster/Events?api-version=6.4&starttimeutc=2017-04-22T17:01:51Z&endtimeutc=2018-04-29T17:02:51Z&EventsTypesFilter=ClusterUpgradeCompleted`

*Küme yükseltme sorunları:*

Benzer şekilde, son küme yükseltmesi ile ilgili sorunlar varsa, küme varlığı için tüm olayları sorgulayabilirsiniz. Yükseltmelerin başlatılması ve yükseltmenin başarıyla yuvarlandığı her UD dahil olmak üzere çeşitli olaylar görürsünüz. Ayrıca, geri almanın başladığı noktaya ve ilgili sağlık olaylarına ait olayları da görürsünüz. Bunun için kullanacağınız sorgu aşağıda veda edebilirsiniz:`https://mycluster.cloudapp.azure.com:19080/EventsStore/Cluster/Events?api-version=6.4&starttimeutc=2017-04-22T17:01:51Z&endtimeutc=2018-04-29T17:02:51Z`

*Düğüm durumu değişiklikleri:*

Düğüm durumlarınızın son birkaç gün içinde değiştiğini görmek için - düğümler yukarı veya aşağı gittiğinde veya etkinleştirildiğinde veya devre dışı bırakıldığında (platform, kaos hizmeti veya kullanıcı girişi tarafından) aşağıdaki sorguyu kullanın:`https://mycluster.cloudapp.azure.com:19080/EventsStore/Nodes/Events?api-version=6.4&starttimeutc=2017-04-22T17:01:51Z&endtimeutc=2018-04-29T17:02:51Z`

*Uygulama olayları:*

Ayrıca, son uygulama dağıtımlarınızı ve yükseltmelerinizi de izleyebilirsiniz. Kümenizdeki tüm uygulama olaylarını görmek için aşağıdaki sorguyu kullanın:`https://mycluster.cloudapp.azure.com:19080/EventsStore/Applications/Events?api-version=6.4&starttimeutc=2017-04-22T17:01:51Z&endtimeutc=2018-04-29T17:02:51Z`

*Bir uygulama için geçmiş sağlık:*

Yalnızca uygulama yaşam döngüsü olaylarını görmenin yanı sıra, belirli bir uygulamanın durumu yla ilgili geçmiş verileri de görmek isteyebilirsiniz. Bunu, verileri toplamak istediğiniz uygulama adını belirterek yapabilirsiniz. Tüm uygulama sistem durumu olaylarını `https://mycluster.cloudapp.azure.com:19080/EventsStore/Applications/myApp/$/Events?api-version=6.4&starttimeutc=2018-03-24T17:01:51Z&endtimeutc=2018-03-29T17:02:51Z&EventsTypesFilter=ApplicationNewHealthReport`almak için bu sorguyu kullanın: . Süresi dolmuş olabilecek sağlık olaylarını (yaşam sürelerini geçmiş (TTL) `,ApplicationHealthReportExpired` eklemek istiyorsanız, iki tür olaya filtre uygulayarak sorgunun sonuna ekleyin.

*"myApp"taki tüm hizmetler için tarihsel sağlık:*

Şu anda, hizmetlere ilişkin `DeployedServicePackageNewHealthReport` sağlık raporu olayları ilgili uygulama varlığı altında olay olarak gösteriş. Hizmetlerinizin "App1" için nasıl olduğunu görmek için aşağıdaki sorguyu kullanın:`https://winlrc-staging-10.southcentralus.cloudapp.azure.com:19080/EventsStore/Applications/myapp/$/Events?api-version=6.4&starttimeutc=2017-04-22T17:01:51Z&endtimeutc=2018-04-29T17:02:51Z&EventsTypesFilter=DeployedServicePackageNewHealthReport`

*Bölüm yeniden yapılandırma:*

Kümenizde gerçekleşen tüm bölüm hareketlerini görmek için `PartitionReconfigured` olayı sorgula. Bu, kümenizdeki sorunları tanılarken belirli zamanlarda hangi düğümde hangi iş yüklerinin koştuğundan anlamanıza yardımcı olabilir. Bunu yapan örnek bir sorgu aşağıda veda eder:`https://mycluster.cloudapp.azure.com:19080/EventsStore/Partitions/Events?api-version=6.4&starttimeutc=2018-04-22T17:01:51Z&endtimeutc=2018-04-29T17:02:51Z&EventsTypesFilter=PartitionReconfigured`

*Kaos hizmeti:*

Küme düzeyinde açıkta kalan Kaos hizmetinin başlatıldığında veya durdurulduğu bir olay vardır. Kaos hizmetini son zamanlarda kullandığınızı görmek için aşağıdaki sorguyu kullanın:`https://mycluster.cloudapp.azure.com:19080/EventsStore/Cluster/Events?api-version=6.4&starttimeutc=2017-04-22T17:01:51Z&endtimeutc=2018-04-29T17:02:51Z&EventsTypesFilter=ChaosStarted,ChaosStopped`

