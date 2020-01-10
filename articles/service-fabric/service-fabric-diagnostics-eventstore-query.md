---
title: EventStore API 'Lerini kullanarak küme olaylarını sorgulama
description: Platform olaylarını sorgulamak için Azure Service Fabric EventStore API 'Lerini nasıl kullanacağınızı öğrenin
author: srrengar
ms.topic: conceptual
ms.date: 02/25/2019
ms.author: srrengar
ms.openlocfilehash: 48350caef6bdaafda9aff7ac776d67b314aeaf8c
ms.sourcegitcommit: 003e73f8eea1e3e9df248d55c65348779c79b1d6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/02/2020
ms.locfileid: "75614409"
---
# <a name="query-eventstore-apis-for-cluster-events"></a>Küme olayları için EventStore API 'Lerini sorgulama

Bu makalede, Service Fabric sürüm 6,2 ve üzeri sürümlerde bulunan EventStore API 'Lerinin nasıl sorgulanyapılacağı ele alınmaktadır. EventStore hizmeti hakkında daha fazla bilgi edinmek istiyorsanız, [eventstore hizmetine genel bakış](service-fabric-diagnostics-eventstore.md)bölümüne bakın. Şu anda EventStore hizmeti yalnızca son 7 güne ait verilere erişebilir (Bu, kümenizin tanılama veri saklama ilkesini temel alır).

>[!NOTE]
>EventStore API 'Leri, yalnızca Azure üzerinde çalışan Windows kümeleri için Service Fabric sürüm 6,4 itibariyle GA ' dir.

EventStore API 'Lerine doğrudan bir REST uç noktası veya program aracılığıyla erişilebilir. Sorguya bağlı olarak, doğru verileri toplamak için gereken birkaç parametre vardır. Bu parametreler genellikle şunları içerir:
* `api-version`: kullandığınız EventStore API 'Lerinin sürümü
* `StartTimeUtc`: bakmakla ilgilendiğiniz dönemin başlangıcını tanımlar
* `EndTimeUtc`: zaman döneminin sonu

Bu parametrelere ek olarak, aşağıdaki gibi isteğe bağlı parametreler de mevcuttur:
* `timeout`: istek işlemini gerçekleştirmek için varsayılan 60 saniyelik zaman aşımını geçersiz kılın
* `eventstypesfilter`: Bu size belirli olay türlerini filtreleme seçeneği sunar
* `ExcludeAnalysisEvents`: ' Analysis ' olaylarını döndürmeyin. Varsayılan olarak, EventStore sorguları mümkün olduğunca "analiz" olaylarıyla birlikte döndürülür. Analiz olayları, normal bir Service Fabric olayının ötesinde ek bağlam veya bilgiler içeren ve daha ayrıntılı bilgi sağlayan daha zengin işlem kanalı olaylardır.
* `SkipCorrelationLookup`: kümede olası bağıntılı olayları arama. Varsayılan olarak, EventStore olayları bir küme genelinde ilişkilendirmeyi dener ve mümkünse olaylarınızı birbirine bağlar. 

Bir kümedeki her varlık olay sorguları olabilir. Ayrıca, bu türdeki tüm varlıkların olaylarını da sorgulayabilirsiniz. Örneğin, belirli bir düğümün veya kümenizdeki tüm düğümlerin olaylarını sorgulayabilirsiniz. Olayları sorgulayabilmeniz için kullanabileceğiniz geçerli varlıklar kümesi (sorgunun yapılandırılması ile birlikte):
* Küme: `/EventsStore/Cluster/Events`
* Düğümler: `/EventsStore/Nodes/Events`
* Düğüm: `/EventsStore/Nodes/<NodeName>/$/Events`
* Uygulamalar: `/EventsStore/Applications/Events`
* Uygulama: `/EventsStore/Applications/<AppName>/$/Events`
* Hizmetler: `/EventsStore/Services/Events`
* Hizmet: `/EventsStore/Services/<ServiceName>/$/Events`
* Bölümler: `/EventsStore/Partitions/Events`
* Bölüm: `/EventsStore/Partitions/<PartitionID>/$/Events`
* Çoğaltmalar: `/EventsStore/Partitions/<PartitionID>/$/Replicas/Events`
* Çoğaltma: `/EventsStore/Partitions/<PartitionID>/$/Replicas/<ReplicaID>/$/Events`

>[!NOTE]
>Bir uygulamaya veya hizmet adına başvurulduğunda, sorgunun "Fabric:/" eklemesi gerekmez koy. Ayrıca, uygulama veya hizmet adlarınızın içinde "/" varsa, sorguyu çalışır durumda tutmak için "~" olarak değiştirin. Örneğin, uygulamanız "Fabric:/app1/FrontendApp" olarak görünüyorsa, uygulamaya özgü sorgular `/EventsStore/Applications/App1~FrontendApp/$/Events`olarak yapılandırılır.
>Ayrıca, günümüzde hizmetler için sistem durumu raporları, ilgili uygulamanın altında görünür, bu sayede doğru uygulama varlığına yönelik `DeployedServiceHealthReportCreated` olaylarını sorgulamanızı sağlayabilirsiniz. 

## <a name="query-the-eventstore-via-rest-api-endpoints"></a>EventStore 'u REST API uç noktaları aracılığıyla sorgulama

EventStore 'u doğrudan bir REST uç noktası aracılığıyla sorgulayabilirsiniz: `<your cluster address>/EventsStore/<entity>/Events/``GET` istekleri yaparak.

Örneğin, `2018-04-03T18:00:00Z` ve `2018-04-04T18:00:00Z`arasındaki tüm küme olaylarını sorgulamak için isteğiniz şöyle görünür:

```
Method: GET 
URL: http://mycluster:19080/EventsStore/Cluster/Events?api-version=6.4&StartTimeUtc=2018-04-03T18:00:00Z&EndTimeUtc=2018-04-04T18:00:00Z
```

Bu, herhangi bir olay veya JSON 'da döndürülen olay listesini döndürebilir:

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

Burada, `2018-04-03T18:00:00Z` ve `2018-04-04T18:00:00Z`arasında, bu kümenin ilk Stood ilk yükseltmesini başarıyla tamamladığı, `"CurrentClusterVersion": "0.0.0.0:"` `"OverallUpgradeElapsedTimeInMs": "120196.5212"``"TargetClusterVersion": "6.2:1.0"`.

## <a name="query-the-eventstore-programmatically"></a>EventStore 'u programlı olarak sorgulama

Ayrıca, [Service Fabric istemci kitaplığı](https://docs.microsoft.com/dotnet/api/overview/azure/service-fabric?view=azure-dotnet#client-library)aracılığıyla eventstore 'u programlı bir şekilde sorgulayabilirsiniz.

Service Fabric Istemcinizi ayarladıktan sonra, aşağıdaki gibi EventStore 'a erişerek olayları sorgulayabilirsiniz: `sfhttpClient.EventStore.<request>`

`2018-04-03T18:00:00Z` ve `2018-04-04T18:00:00Z`arasındaki tüm küme olayları için `GetClusterEventListAsync` işlevi aracılığıyla örnek bir istek aşağıda verilmiştir.

```csharp
var sfhttpClient = ServiceFabricClientFactory.Create(clusterUrl, settings);

var clstrEvents = sfhttpClient.EventsStore.GetClusterEventListAsync(
    "2018-04-03T18:00:00Z",
    "2018-04-04T18:00:00Z")
    .GetAwaiter()
    .GetResult()
    .ToList();
```

İşte, küme durumunu ve 2018 Eylül 'deki tüm düğüm olaylarını sorgulayan ve bunları yazdıran bir örnek.

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

Kümenizin durumunu anlamak için olay deposu REST API 'Lerini nasıl çağırabileceğinizi gösteren birkaç örnek aşağıda verilmiştir.

*Küme yükseltmeleri:*

Kümenizin son haftada başarıyla yükseltildiğine veya son hafta yükseltildiğine bakmak için, EventStore 'daki "ClusterUpgradeCompleted" olaylarını sorgulayarak kümenizdeki son tamamlanan yükseltmelere yönelik API 'Leri sorgulayabilirsiniz. `https://mycluster.cloudapp.azure.com:19080/EventsStore/Cluster/Events?api-version=6.4&starttimeutc=2017-04-22T17:01:51Z&endtimeutc=2018-04-29T17:02:51Z&EventsTypesFilter=ClusterUpgradeCompleted`

*Küme yükseltme sorunları:*

Benzer şekilde, son küme yükseltmesinde sorunlar varsa, küme varlığının tüm olaylarını sorgulayabilirsiniz. Yükseltmelerin başlatılması ve yükseltmenin başarıyla alındığı her bir UD dahil olmak üzere çeşitli olaylar görürsünüz. Ayrıca, geri almanın başlatıldığı noktaya yönelik olayları ve buna karşılık gelen sistem durumu olaylarını görürsünüz. Bunun için kullanacağınız sorgu şu şekildedir: `https://mycluster.cloudapp.azure.com:19080/EventsStore/Cluster/Events?api-version=6.4&starttimeutc=2017-04-22T17:01:51Z&endtimeutc=2018-04-29T17:02:51Z`

*Düğüm durumu değişiklikleri:*

Düğüm durumu değişikliklerinizi son birkaç güne göre görmek için-düğümler ne zaman kaldığını veya devre dışı bırakıldığını ya da etkin ya da devre dışı bırakıldığını (Platform, Chaos hizmeti veya Kullanıcı girişinden)-Şu sorguyu kullanın: `https://mycluster.cloudapp.azure.com:19080/EventsStore/Nodes/Events?api-version=6.4&starttimeutc=2017-04-22T17:01:51Z&endtimeutc=2018-04-29T17:02:51Z`

*Uygulama olayları:*

Ayrıca, son uygulama dağıtımlarınızı ve yükseltmelerini izleyebilirsiniz. Kümenizdeki tüm uygulama olaylarını görmek için aşağıdaki sorguyu kullanın: `https://mycluster.cloudapp.azure.com:19080/EventsStore/Applications/Events?api-version=6.4&starttimeutc=2017-04-22T17:01:51Z&endtimeutc=2018-04-29T17:02:51Z`

*Bir uygulamanın geçmiş durumu:*

Yalnızca uygulama yaşam döngüsü olaylarını görmenin yanı sıra, belirli bir uygulamanın sistem durumunda geçmiş verileri de görmek isteyebilirsiniz. Bunu, verileri toplamak istediğiniz uygulama adını belirterek yapabilirsiniz. Tüm uygulama sistem durumu olaylarını almak için bu sorguyu kullanın: `https://mycluster.cloudapp.azure.com:19080/EventsStore/Applications/myApp/$/Events?api-version=6.4&starttimeutc=2018-03-24T17:01:51Z&endtimeutc=2018-03-29T17:02:51Z&EventsTypesFilter=ApplicationNewHealthReport`. Süresi sona ermiş olabilecek sistem durumu olaylarını (yaşam süresi (TTL)) eklemek istiyorsanız, iki tür olayı filtrelemek için sorgunun sonuna `,ApplicationHealthReportExpired` ekleyin.

*"MyApp" içindeki tüm hizmetler için geçmiş durumu:*

Şu anda, hizmetler için sistem durumu raporu olayları, ilgili uygulama varlığı altında `DeployedServicePackageNewHealthReport` olay olarak görünür. Hizmetlerinizin "APP1" için nasıl çalıştığını görmek için aşağıdaki sorguyu kullanın: `https://winlrc-staging-10.southcentralus.cloudapp.azure.com:19080/EventsStore/Applications/myapp/$/Events?api-version=6.4&starttimeutc=2017-04-22T17:01:51Z&endtimeutc=2018-04-29T17:02:51Z&EventsTypesFilter=DeployedServicePackageNewHealthReport`

*Bölüm yeniden yapılandırması:*

Kümenizde gerçekleşen tüm bölüm hareketlerini görmek için `PartitionReconfigured` olayı sorgulayın. Bu, kümenizdeki sorunları tanılarken belirli zamanlarda hangi iş yüklerinin hangi düğümlerde çalıştığını bulmanıza yardımcı olabilir. İşte şöyle bir örnek sorgu: `https://mycluster.cloudapp.azure.com:19080/EventsStore/Partitions/Events?api-version=6.4&starttimeutc=2018-04-22T17:01:51Z&endtimeutc=2018-04-29T17:02:51Z&EventsTypesFilter=PartitionReconfigured`

*Chaos hizmeti:*

Chaos hizmeti başlatıldığında veya durdurulduğunda, küme düzeyinde sunulan bir olay vardır. Chaos hizmetini son kullanma hakkında bilgi için aşağıdaki sorguyu kullanın: `https://mycluster.cloudapp.azure.com:19080/EventsStore/Cluster/Events?api-version=6.4&starttimeutc=2017-04-22T17:01:51Z&endtimeutc=2018-04-29T17:02:51Z&EventsTypesFilter=ChaosStarted,ChaosStopped`

