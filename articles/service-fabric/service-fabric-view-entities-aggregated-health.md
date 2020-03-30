---
title: Azure Hizmet Kumaşı kuruluşlarının toplu sağlık durumunu görüntüleme
description: Sistem durumu sorguları ve genel sorgular aracılığıyla Azure Hizmet Kumaşı kuruluşlarının toplu sağlık durumunu nasıl sorgulayın, görüntülenin ve değerlendireceklerini açıklar.
author: oanapl
ms.topic: conceptual
ms.date: 2/28/2018
ms.author: oanapl
ms.openlocfilehash: d02d8f717801bf51e43c9dafa5eb9379d0737674
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75464120"
---
# <a name="view-service-fabric-health-reports"></a>Hizmet Kumaşı sağlık raporlarını görüntüleyin
Azure Hizmet Kumaşı, sistem bileşenlerinin ve izleme kuruluşlarının izdiklerini yerel koşulları bildirebildiği sağlık kuruluşlarıyla bir [sistem modeli](service-fabric-health-introduction.md) sunar. [Sistem durumu deposu,](service-fabric-health-introduction.md#health-store) varlıkların sağlıklı olup olmadığını belirlemek için tüm sistem durumu verilerini toplar.

Küme, sistem bileşenleri tarafından gönderilen sistem raporlarıyla otomatik olarak doldurulur. [Sorun gidermek için sistem sağlık raporlarını kullan'da](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)daha fazla bilgi edinin.

Service Fabric, varlıkların toplu sağlık durumunu elde etmek için birden çok yol sağlar:

* [Hizmet Kumaş Explorer](service-fabric-visualizing-your-cluster.md) veya diğer görselleştirme araçları
* Sistem durumu sorguları (PowerShell, API veya REST aracılığıyla)
* Özelliklerden biri olarak sağlık durumu olan varlıkların listesini döndüren genel sorgular (PowerShell, API veya REST aracılığıyla)

Bu seçenekleri göstermek için, beş düğüm ve kumaş içeren yerel bir küme [kullanalım:/WordCount uygulaması.](https://github.com/Azure-Samples/service-fabric-wordcount/raw/master/WordCountV1.sfpkg) **Kumaş:/WordCount** uygulaması iki varsayılan hizmet, tür `WordCountServiceType`bir stateful hizmet ve `WordCountWebServiceType`türü nde bir stateless hizmet içerir. Ben devlet `ApplicationManifest.xml` hizmeti ve bir bölüm için yedi hedef yinelemeler gerektiren değiştirildi. Kümede yalnızca beş düğüm olduğundan, sistem bileşenleri hedef sayısının altında olduğundan hizmet bölümüne bir uyarı bildirir.

```xml
<Service Name="WordCountService">
  <StatefulService ServiceTypeName="WordCountServiceType" TargetReplicaSetSize="7" MinReplicaSetSize="2">
    <UniformInt64Partition PartitionCount="[WordCountService_PartitionCount]" LowKey="1" HighKey="26" />
  </StatefulService>
</Service>
```

## <a name="health-in-service-fabric-explorer"></a>Hizmet Kumaş Explorer Sağlık
Service Fabric Explorer kümenin görsel görünümünü sağlar. Aşağıdaki resimde şunları görebilirsiniz:

* Uygulama **kumaş:/WordCount** kırmızı (hata) çünkü bir hata olay **MyWatchdog** tarafından mülkiyet **Durumu**için rapor vardır.
* Hizmetlerinden biri, **kumaş:/WordCount/WordCountService** sarıdır (uyarı olarak). Hizmet yedi yinelemeyle yapılandırılır ve kümenin beş düğümü vardır, bu nedenle iki yineleme yerleştirilemez. Burada gösterilmese de, hizmet bölümü sarı dır çünkü `System.FM` bir `Partition is below target replica or instance count`sistem raporu olduğunu söylüyor. Sarı bölüm sarı hizmeti tetikler.
* Küme kırmızı uygulama nedeniyle kırmızıdır.

Değerlendirme, küme bildirimi ve uygulama bildiriminden varsayılan ilkeleri kullanır. Bunlar katı politikalardır ve herhangi bir başarısızlığa müsamaha göstermezler.

Service Fabric Explorer ile kümegörünümü:

![Service Fabric Explorer ile kümegörünümü.][1]

[1]: ./media/service-fabric-view-entities-aggregated-health/servicefabric-explorer-cluster-health.png


> [!NOTE]
> [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md)hakkında daha fazla bilgi edinin.
>
>

## <a name="health-queries"></a>Sistem durumu sorguları
Service Fabric, desteklenen [varlık türlerinin](service-fabric-health-introduction.md#health-entities-and-hierarchy)her biri için sistem durumu sorgularını ortaya çıkarır. [FabricClient.HealthManager,](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthmanager?view=azure-dotnet)PowerShell cmdlets ve REST üzerindeki yöntemler kullanılarak API üzerinden erişilebilirler. Bu sorgular varlık la ilgili tam sağlık bilgilerini döndürmektedir: toplu sağlık durumu, varlık sağlık olayları, çocuk sağlığı durumları (varsa), sağlıksız değerlendirmeler (varlık sağlıklı olmadığında) ve çocuk sağlığı istatistikleri ( uygulanabilir).

> [!NOTE]
> Bir sağlık varlığı, sağlık deposunda tam olarak doldurulduğunda döndürülür. Varlık etkin (silinmemiş) ve bir sistem raporu olmalıdır. Hiyerarşi zincirindeki ana varlıklarının da sistem raporları olması gerekir. Bu koşullardan herhangi biri yerine getirilmezse, sistem durumu sorguları, varlığın neden döndürülmediğini gösteren [FabricErrorCode](https://docs.microsoft.com/dotnet/api/system.fabric.fabricerrorcode) `FabricHealthEntityNotFound` ile [bir FabricException](https://docs.microsoft.com/dotnet/api/system.fabric.fabricexception) döndürür.
>
>

Sistem durumu sorguları, varlık türüne bağlı olarak varlık tanımlayıcısında geçmelidir. Sorgular isteğe bağlı sistem durumu ilkesi parametrelerini kabul eder. Sağlık ilkeleri belirtilmemişse, değerlendirme için kümedeki veya uygulama bildirimindeki [sistem durumu ilkeleri](service-fabric-health-introduction.md#health-policies) kullanılır. Bildirimler sistem durumu ilkeleri için bir tanım içermiyorsa, değerlendirme için varsayılan sistem durumu ilkeleri kullanılır. Varsayılan sistem durumu ilkeleri herhangi bir hatator etmez. Sorgular, yalnızca kısmi alt çocukları veya olayları (belirtilen filtrelere saygı gösterenleri- döndürmek için filtreleri de kabul eder. Başka bir filtre, çocuk istatistiklerinin hariç çıkarılmasına izin verir.

> [!NOTE]
> Çıktı filtreleri sunucu tarafında uygulanır, böylece ileti yanıt boyutu küçültülür. İstemci tarafında filtre uygulamak yerine, döndürülen verileri sınırlamak için çıktı filtrelerini kullanmanızı tavsiye ettik.
>
>

Bir varlığın sağlığı şunları içerir:

* Varlığın toplu sağlık durumu. Varlık sağlık raporları, çocuk sağlığı durumları (varsa) ve sağlık politikalarına göre sağlık deposu tarafından hesaplanır. [Varlık sağlık değerlendirmesi](service-fabric-health-introduction.md#health-evaluation)hakkında daha fazla bilgi edinin.  
* Varlıktaki sağlık olayları.
* Çocuk sahibi olabilecek varlıklar için tüm çocukların sağlık durumlarının toplanması. Sağlık durumları varlık tanımlayıcıları ve toplu sağlık durumu içerir. Bir çocuk için tam sağlık almak için, çocuk varlık türü için sorgu sağlık çağrı ve alt tanımlayıcı geçmek.
* Varlık sağlıklı değilse, varlığın durumunu tetikleyen rapora işaret eden sağlıksız değerlendirmeler. Değerlendirmeler özyinelemeli olup, mevcut sağlık durumunu tetikleyen çocukların sağlık değerlendirmelerini de içeren değerlendirmelerdir. Örneğin, bir izleme örgütü bir yinelemeye karşı bir hata bildirdi. Uygulama durumu sağlıksız bir hizmet nedeniyle sağlıksız bir değerlendirme gösterir; hizmet hata bir bölüm nedeniyle sağlıksız; bölüm hata bir yineleme nedeniyle sağlıksız; çoğaltma watchdog hata sağlık raporu nedeniyle sağlıksız.
* Çocuk sahibi olan varlıkların tüm çocuk türleri için sağlık istatistikleri. Örneğin, küme durumu kümedeki toplam uygulama, hizmet, bölüm, yineleme ve dağıtılan varlıkların toplam sayısını gösterir. Hizmet durumu, belirtilen hizmet altında toplam bölüm ve yineleme sayısını gösterir.

## <a name="get-cluster-health"></a>Küme sağlığı nı elde edin
Küme varlığının durumunu verir ve uygulamaların ve düğümlerin (kümenin alt ları) sistem durumunu içerir. Giriş:

* [İsteğe bağlı] Düğümleri ve küme olaylarını değerlendirmek için kullanılan küme sistem durumu ilkesi.
* [İsteğe bağlı] Uygulama bildirimi ilkelerini geçersiz kılmak için kullanılan sistem durumu ilkeleriyle birlikte uygulama durumu ilkesi haritası.
* [İsteğe bağlı] Hangi girişlerin ilgi çekici olduğunu ve sonuç olarak döndürülmesi gerektiğini belirten olaylar, düğümler ve uygulamalar için filtreler (örneğin, yalnızca hatalar veya her iki uyarı ve hata). Tüm olaylar, düğümler ve uygulamalar, filtreden bağımsız olarak toplanan varlığı değerlendirmek için kullanılır.
* [İsteğe bağlı] Sistem durumu istatistiklerini hariç tutmak için filtre uygulayın.
* [İsteğe bağlı] Filtre doku eklemek için:/Sistem sağlık istatistikleri sağlık istatistikleri. Yalnızca sağlık istatistikleri hariç olmadığında geçerlidir. Varsayılan olarak, sistem istatistikleri sistem uygulaması için değil, yalnızca kullanıcı uygulamaları için istatistikleri içerir.

### <a name="api"></a>API
Küme durumu almak için, bir `FabricClient` oluşturma ve Onun **HealthManager** [getClusterHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getclusterhealthasync) yöntemi arayın.

Aşağıdaki çağrı küme durumunu alır:

```csharp
ClusterHealth clusterHealth = await fabricClient.HealthManager.GetClusterHealthAsync();
```

Aşağıdaki kod, özel küme durumu ilkesini kullanarak küme durumunu alır ve düğümler ve uygulamalar için filtreler ilerler. Bu sağlık istatistikleri kumaş:/ Sistem istatistikleri içerir belirtir. Bu giriş bilgilerini içeren [ClusterHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.clusterhealthquerydescription)oluşturur.

```csharp
var policy = new ClusterHealthPolicy()
{
    MaxPercentUnhealthyNodes = 20
};
var nodesFilter = new NodeHealthStatesFilter()
{
    HealthStateFilterValue = HealthStateFilter.Error | HealthStateFilter.Warning
};
var applicationsFilter = new ApplicationHealthStatesFilter()
{
    HealthStateFilterValue = HealthStateFilter.Error
};
var healthStatisticsFilter = new ClusterHealthStatisticsFilter()
{
    ExcludeHealthStatistics = false,
    IncludeSystemApplicationHealthStatistics = true
};
var queryDescription = new ClusterHealthQueryDescription()
{
    HealthPolicy = policy,
    ApplicationsFilter = applicationsFilter,
    NodesFilter = nodesFilter,
    HealthStatisticsFilter = healthStatisticsFilter
};

ClusterHealth clusterHealth = await fabricClient.HealthManager.GetClusterHealthAsync(queryDescription);
```

### <a name="powershell"></a>PowerShell
Cmdlet küme sağlık almak için [Get-ServiceFabricClusterHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricclusterhealth)olduğunu. İlk olarak [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet'i kullanarak kümeye bağlanın.

Kümenin durumu beş düğüm, sistem uygulaması ve kumaş:/WordCount açıklandığı gibi yapılandırılmıştır.

Aşağıdaki cmdlet varsayılan sistem durumu ilkeleri kullanarak küme durumu alır. Birleştirilmiş sağlık durumu uyarıdır, çünkü kumaş:/WordCount uygulaması uyarıdadır. Sağlıksız değerlendirmelerin toplu sağlığı tetikleyen koşullar hakkında nasıl ayrıntılı bilgi verdiğine dikkat edin.

```xml
PS D:\ServiceFabric> Get-ServiceFabricClusterHealth


AggregatedHealthState   : Warning
UnhealthyEvaluations    : 
                          Unhealthy applications: 100% (1/1), MaxPercentUnhealthyApplications=0%.

                          Unhealthy application: ApplicationName='fabric:/WordCount', AggregatedHealthState='Warning'.

                            Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.

                            Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Warning'.

                                Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.

                                Unhealthy partition: PartitionId='af2e3e44-a8f8-45ac-9f31-4093eb897600', AggregatedHealthState='Warning'.

                                    Unhealthy event: SourceId='System.FM', Property='State', HealthState='Warning', ConsiderWarningAsError=false.


NodeHealthStates        : 
                          NodeName              : _Node_4
                          AggregatedHealthState : Ok

                          NodeName              : _Node_3
                          AggregatedHealthState : Ok

                          NodeName              : _Node_2
                          AggregatedHealthState : Ok

                          NodeName              : _Node_1
                          AggregatedHealthState : Ok

                          NodeName              : _Node_0
                          AggregatedHealthState : Ok

ApplicationHealthStates : 
                          ApplicationName       : fabric:/System
                          AggregatedHealthState : Ok

                          ApplicationName       : fabric:/WordCount
                          AggregatedHealthState : Warning

HealthEvents            : None
HealthStatistics        : 
                          Node                  : 5 Ok, 0 Warning, 0 Error
                          Replica               : 6 Ok, 0 Warning, 0 Error
                          Partition             : 1 Ok, 1 Warning, 0 Error
                          Service               : 1 Ok, 1 Warning, 0 Error
                          DeployedServicePackage : 6 Ok, 0 Warning, 0 Error
                          DeployedApplication   : 5 Ok, 0 Warning, 0 Error
                          Application           : 0 Ok, 1 Warning, 0 Error
```

Aşağıdaki PowerShell cmdlet özel bir uygulama ilkesi kullanarak kümenin durumunu alır. Yalnızca uygulamaları ve düğümleri hata veya uyarı olarak almak için sonuçları filtreler. Sonuç olarak, düğümlerin tümü sağlıklı olduğu için döndürülür. Yalnızca kumaş:/WordCount uygulaması uygulamalar filtresine saygı duyar. Özel ilke, uyarıları kumaş:/WordCount uygulaması için hata olarak değerlendirmek üzere belirtildiğinden, uygulama hata olarak değerlendirilir ve küme de öyle.

```powershell
PS D:\ServiceFabric> $appHealthPolicy = New-Object -TypeName System.Fabric.Health.ApplicationHealthPolicy
$appHealthPolicy.ConsiderWarningAsError = $true
$appHealthPolicyMap = New-Object -TypeName System.Fabric.Health.ApplicationHealthPolicyMap
$appUri1 = New-Object -TypeName System.Uri -ArgumentList "fabric:/WordCount"
$appHealthPolicyMap.Add($appUri1, $appHealthPolicy)
Get-ServiceFabricClusterHealth -ApplicationHealthPolicyMap $appHealthPolicyMap -ApplicationsFilter "Warning,Error" -NodesFilter "Warning,Error" -ExcludeHealthStatistics


AggregatedHealthState   : Error
UnhealthyEvaluations    : 
                          Unhealthy applications: 100% (1/1), MaxPercentUnhealthyApplications=0%.

                          Unhealthy application: ApplicationName='fabric:/WordCount', AggregatedHealthState='Error'.

                            Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.

                            Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Error'.

                                Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.

                                Unhealthy partition: PartitionId='af2e3e44-a8f8-45ac-9f31-4093eb897600', AggregatedHealthState='Error'.

                                    Unhealthy event: SourceId='System.FM', Property='State', HealthState='Warning', ConsiderWarningAsError=true.


NodeHealthStates        : None
ApplicationHealthStates : 
                          ApplicationName       : fabric:/WordCount
                          AggregatedHealthState : Error

HealthEvents            : None
```

### <a name="rest"></a>REST
Bir [GET isteği](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-cluster) veya vücutta açıklanan sağlık ilkeleri içeren bir [POST isteği](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-cluster-by-using-a-health-policy) ile küme durumu alabilirsiniz.

## <a name="get-node-health"></a>Düğüm sağlığı alın
Bir düğüm varlığının durumunu verir ve düğümde bildirilen sistem durumu olaylarını içerir. Giriş:

* [Gerekli] Düğümü tanımlayan düğüm adı.
* [İsteğe bağlı] Sistem durumunu değerlendirmek için kullanılan küme durumu ilkesi ayarları.
* [İsteğe bağlı] Hangi girişlerin ilgi çekici olduğunu ve sonuç olarak döndürülmesi gerektiğini belirten olaylar için filtreler (örneğin, yalnızca hatalar veya hem uyarılar hem de hatalar). Tüm olaylar, filtreden bağımsız olarak toplanan varlığı değerlendirmek için kullanılır.

### <a name="api"></a>API
API aracılığıyla düğüm sağlığı elde etmek `FabricClient` için, bir oluşturmak ve Onun HealthManager [getNodeHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getnodehealthasync) yöntemi arayın.

Aşağıdaki kod belirtilen düğüm adı için düğüm durumunu alır:

```csharp
NodeHealth nodeHealth = await fabricClient.HealthManager.GetNodeHealthAsync(nodeName);
```

Aşağıdaki kod belirtilen düğüm adı için düğüm durumu alır ve [NodeHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.nodehealthquerydescription)üzerinden olaylar filtre ve özel ilke geçer:

```csharp
var queryDescription = new NodeHealthQueryDescription(nodeName)
{
    HealthPolicy = new ClusterHealthPolicy() {  ConsiderWarningAsError = true },
    EventsFilter = new HealthEventsFilter() { HealthStateFilterValue = HealthStateFilter.Warning },
};

NodeHealth nodeHealth = await fabricClient.HealthManager.GetNodeHealthAsync(queryDescription);
```

### <a name="powershell"></a>PowerShell
Düğüm sağlık almak için cmdlet [Get-ServiceFabricNodeHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricnodehealth)olduğunu. İlk olarak [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet'i kullanarak kümeye bağlanın.
Aşağıdaki cmdlet varsayılan sistem durumu ilkeleri kullanarak düğüm durumunu alır:

```powershell
PS D:\ServiceFabric> Get-ServiceFabricNodeHealth _Node_1


NodeName              : _Node_1
AggregatedHealthState : Ok
HealthEvents          : 
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 3
                        SentAt                : 7/13/2017 4:39:23 PM
                        ReceivedAt            : 7/13/2017 4:40:47 PM
                        TTL                   : Infinite
                        Description           : Fabric node is up.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 7/13/2017 4:40:47 PM, LastWarning = 1/1/0001 12:00:00 AM
```

Aşağıdaki cmdlet kümedeki tüm düğümlerin sağlığını alır:

```powershell
PS D:\ServiceFabric> Get-ServiceFabricNode | Get-ServiceFabricNodeHealth | select NodeName, AggregatedHealthState | ft -AutoSize

NodeName AggregatedHealthState
-------- ---------------------
_Node_4                     Ok
_Node_3                     Ok
_Node_2                     Ok
_Node_1                     Ok
_Node_0                     Ok
```

### <a name="rest"></a>REST
Bir [GET isteği](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-node) veya vücutta açıklanan sağlık ilkeleri içeren bir [POST isteği](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-node-by-using-a-health-policy) ile düğüm sağlığı alabilirsiniz.

## <a name="get-application-health"></a>Uygulama sağlığını elde edin
Bir uygulama varlığının durumunu verir. Dağıtılan uygulama ve hizmet çocuklarının sağlık durumlarını içerir. Giriş:

* [Gerekli] Uygulamayı tanımlayan uygulama adı (URI).
* [İsteğe bağlı] Uygulama bildirimi ilkelerini geçersiz kılmak için kullanılan uygulama durumu ilkesi.
* [İsteğe bağlı] Hangi girişlerin ilgi çekici olduğunu ve sonuç olarak döndürülmesi gerektiğini belirten olaylar, hizmetler ve dağıtılmış uygulamalar için filtreler (örneğin, yalnızca hatalar veya hem uyarılar hem de hatalar). Tüm olaylar, hizmetler ve dağıtılan uygulamalar, filtreden bağımsız olarak toplanan varlığı değerlendirmek için kullanılır.
* [İsteğe bağlı] Sistem durumu istatistiklerini hariç tutmak için filtre uygulayın. Belirtilmemişse, sistem durumu istatistikleri tüm uygulama çocukları için ok, uyarı ve hata sayısını içerir: hizmetler, bölümler, yinelemeler, dağıtılan uygulamalar ve dağıtılan hizmet paketleri.

### <a name="api"></a>API
Uygulama durumunu elde etmek `FabricClient` için, bir oluşturma ve HealthManager [getApplicationHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getapplicationhealthasync) yöntemini arayın.

Belirtilen uygulama adı (URI) için aşağıdaki kod uygulama durumunu alır:

```csharp
ApplicationHealth applicationHealth = await fabricClient.HealthManager.GetApplicationHealthAsync(applicationName);
```

Aşağıdaki [kod, ApplicationHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.applicationhealthquerydescription)ile belirtilen filtreler ve özel ilkeler ile belirtilen uygulama adı (URI) için uygulama durumunu alır.

```csharp
HealthStateFilter warningAndErrors = HealthStateFilter.Error | HealthStateFilter.Warning;
var serviceTypePolicy = new ServiceTypeHealthPolicy()
{
    MaxPercentUnhealthyPartitionsPerService = 0,
    MaxPercentUnhealthyReplicasPerPartition = 5,
    MaxPercentUnhealthyServices = 0,
};
var policy = new ApplicationHealthPolicy()
{
    ConsiderWarningAsError = false,
    DefaultServiceTypeHealthPolicy = serviceTypePolicy,
    MaxPercentUnhealthyDeployedApplications = 0,
};

var queryDescription = new ApplicationHealthQueryDescription(applicationName)
{
    HealthPolicy = policy,
    EventsFilter = new HealthEventsFilter() { HealthStateFilterValue = warningAndErrors },
    ServicesFilter = new ServiceHealthStatesFilter() { HealthStateFilterValue = warningAndErrors },
    DeployedApplicationsFilter = new DeployedApplicationHealthStatesFilter() { HealthStateFilterValue = warningAndErrors },
};

ApplicationHealth applicationHealth = await fabricClient.HealthManager.GetApplicationHealthAsync(queryDescription);
```

### <a name="powershell"></a>PowerShell
Uygulama nın sağlığını almak için cmdlet [Get-ServiceFabricApplicationHealth](/powershell/module/servicefabric/get-servicefabricapplicationhealth?view=azureservicefabricps)olduğunu. İlk olarak [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet'i kullanarak kümeye bağlanın.

Aşağıdaki cmdlet kumaşın sağlığını **döndürür:/WordCount** uygulaması:

```powershell
PS D:\ServiceFabric> Get-ServiceFabricApplicationHealth fabric:/WordCount


ApplicationName                 : fabric:/WordCount
AggregatedHealthState           : Warning
UnhealthyEvaluations            : 
                                  Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.

                                  Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Warning'.

                                    Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.

                                    Unhealthy partition: PartitionId='af2e3e44-a8f8-45ac-9f31-4093eb897600', AggregatedHealthState='Warning'.

                                        Unhealthy event: SourceId='System.FM', Property='State', HealthState='Warning', ConsiderWarningAsError=false.

ServiceHealthStates             : 
                                  ServiceName           : fabric:/WordCount/WordCountWebService
                                  AggregatedHealthState : Ok

                                  ServiceName           : fabric:/WordCount/WordCountService
                                  AggregatedHealthState : Warning

DeployedApplicationHealthStates : 
                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_4
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_3
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_0
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_2
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_1
                                  AggregatedHealthState : Ok

HealthEvents                    : 
                                  SourceId              : System.CM
                                  Property              : State
                                  HealthState           : Ok
                                  SequenceNumber        : 282
                                  SentAt                : 7/13/2017 5:57:05 PM
                                  ReceivedAt            : 7/13/2017 5:57:05 PM
                                  TTL                   : Infinite
                                  Description           : Application has been created.
                                  RemoveWhenExpired     : False
                                  IsExpired             : False
                                  Transitions           : Error->Ok = 7/13/2017 5:57:05 PM, LastWarning = 1/1/0001 12:00:00 AM

HealthStatistics                : 
                                  Replica               : 6 Ok, 0 Warning, 0 Error
                                  Partition             : 1 Ok, 1 Warning, 0 Error
                                  Service               : 1 Ok, 1 Warning, 0 Error
                                  DeployedServicePackage : 6 Ok, 0 Warning, 0 Error
                                  DeployedApplication   : 5 Ok, 0 Warning, 0 Error
```

Aşağıdaki PowerShell cmdlet özel ilkelerde geçer. Ayrıca çocukları ve olayları filtreler.

```powershell
PS D:\ServiceFabric> Get-ServiceFabricApplicationHealth -ApplicationName fabric:/WordCount -ConsiderWarningAsError $true -ServicesFilter Error -EventsFilter Error -DeployedApplicationsFilter Error -ExcludeHealthStatistics


ApplicationName                 : fabric:/WordCount
AggregatedHealthState           : Error
UnhealthyEvaluations            : 
                                  Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.

                                  Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Error'.

                                    Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.

                                    Unhealthy partition: PartitionId='af2e3e44-a8f8-45ac-9f31-4093eb897600', AggregatedHealthState='Error'.

                                        Unhealthy event: SourceId='System.FM', Property='State', HealthState='Warning', ConsiderWarningAsError=true.

ServiceHealthStates             : 
                                  ServiceName           : fabric:/WordCount/WordCountService
                                  AggregatedHealthState : Error

DeployedApplicationHealthStates : None
HealthEvents                    : None
```

### <a name="rest"></a>REST
Bir [GET isteği](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-an-application) veya vücutta açıklanan sağlık ilkeleri içeren bir [POST isteği](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-an-application-by-using-an-application-health-policy) ile uygulama sağlığı alabilirsiniz.

## <a name="get-service-health"></a>Hizmet sağlığına kavuşun
Bir hizmet kuruluşunun durumunu verir. Bölüm sistem durumu durumlarını içerir. Giriş:

* [Gerekli] Hizmeti tanımlayan hizmet adı (URI).
* [İsteğe bağlı] Uygulama bildirimi ilkesini geçersiz kılmak için kullanılan uygulama durumu ilkesi.
* [İsteğe bağlı] Hangi girişlerin ilgi çekici olduğunu ve sonuç olarak döndürülmesi gerektiğini belirten olaylar ve bölümler için filtreler (örneğin, yalnızca hatalar veya hem uyarılar hem de hatalar). Tüm olaylar ve bölümler, filtreden bağımsız olarak toplanan varlığı değerlendirmek için kullanılır.
* [İsteğe bağlı] Sistem durumu istatistiklerini hariç tutmak için filtre uygulayın. Belirtilmemişse, sistem durumu istatistikleri, hizmetin tüm bölümleri ve yinelemeleri için tamam, uyarı ve hata sayısını gösterir.

### <a name="api"></a>API
API aracılığıyla hizmet sağlığı elde `FabricClient` etmek için, bir oluşturmak ve Onun HealthManager [getServiceHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getservicehealthasync) yöntemi arayın.

Aşağıdaki örnek, belirtilen hizmet adı (URI) ile bir hizmetin durumunu alır:

```csharp
ServiceHealth serviceHealth = await fabricClient.HealthManager.GetServiceHealthAsync(serviceName);
```

Aşağıdaki kod [ServiceHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.servicehealthquerydescription)üzerinden filtreler ve özel politika belirterek, belirtilen hizmet adı (URI) için hizmet durumu alır:

```csharp
var queryDescription = new ServiceHealthQueryDescription(serviceName)
{
    EventsFilter = new HealthEventsFilter() { HealthStateFilterValue = HealthStateFilter.All },
    PartitionsFilter = new PartitionHealthStatesFilter() { HealthStateFilterValue = HealthStateFilter.Error },
};

ServiceHealth serviceHealth = await fabricClient.HealthManager.GetServiceHealthAsync(queryDescription);
```

### <a name="powershell"></a>PowerShell
Cmdlet hizmet sağlık almak için [Get-ServiceFabricServiceHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricservicehealth)olduğunu. İlk olarak [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet'i kullanarak kümeye bağlanın.

Aşağıdaki cmdlet varsayılan sistem durumu ilkelerini kullanarak hizmet durumunu alır:

```powershell
PS D:\ServiceFabric> Get-ServiceFabricServiceHealth -ServiceName fabric:/WordCount/WordCountService


ServiceName           : fabric:/WordCount/WordCountService
AggregatedHealthState : Warning
UnhealthyEvaluations  : 
                        Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.

                        Unhealthy partition: PartitionId='af2e3e44-a8f8-45ac-9f31-4093eb897600', AggregatedHealthState='Warning'.

                            Unhealthy event: SourceId='System.FM', Property='State', HealthState='Warning', ConsiderWarningAsError=false.

PartitionHealthStates : 
                        PartitionId           : af2e3e44-a8f8-45ac-9f31-4093eb897600
                        AggregatedHealthState : Warning

HealthEvents          : 
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 15
                        SentAt                : 7/13/2017 5:57:05 PM
                        ReceivedAt            : 7/13/2017 5:57:18 PM
                        TTL                   : Infinite
                        Description           : Service has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 7/13/2017 5:57:18 PM, LastWarning = 1/1/0001 12:00:00 AM

HealthStatistics      : 
                        Replica               : 5 Ok, 0 Warning, 0 Error
                        Partition             : 0 Ok, 1 Warning, 0 Error
```

### <a name="rest"></a>REST
Bir [GET isteği](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-service) veya vücutta açıklanan sağlık ilkeleri içeren bir [POST isteği](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-service-by-using-a-health-policy) ile hizmet sağlığı alabilirsiniz.

## <a name="get-partition-health"></a>Bölüm durumu alın
Bir bölüm varlığının durumunu döndürür. Yineleme sistem durumunu içerir. Giriş:

* [Gerekli] Bölümü tanımlayan bölüm kimliği (GUID).
* [İsteğe bağlı] Uygulama bildirimi ilkesini geçersiz kılmak için kullanılan uygulama durumu ilkesi.
* [İsteğe bağlı] Hangi girişlerin ilgi çekici olduğunu ve sonuç olarak döndürülmesi gerektiğini belirten olaylar ve yinelemeler için filtreler (örneğin, yalnızca hatalar veya hem uyarılar hem de hatalar). Tüm olaylar ve yinelemeler, filtreden bağımsız olarak toplanan varlığı değerlendirmek için kullanılır.
* [İsteğe bağlı] Sistem durumu istatistiklerini hariç tutmak için filtre uygulayın. Belirtilmemişse, sistem durumu istatistikleri kaç yinelemenin tamam, uyarı ve hata durumlarında olduğunu gösterir.

### <a name="api"></a>API
API üzerinden bölüm durumu elde `FabricClient` etmek için, bir oluşturmak ve Onun HealthManager [GetPartitionHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getpartitionhealthasync) yöntemi arayın. İsteğe bağlı parametreleri belirtmek için [PartitionHealthQueryDescription'ı](https://docs.microsoft.com/dotnet/api/system.fabric.description.partitionhealthquerydescription)oluşturun.

```csharp
PartitionHealth partitionHealth = await fabricClient.HealthManager.GetPartitionHealthAsync(partitionId);
```

### <a name="powershell"></a>PowerShell
Cmdlet bölüm sağlık almak için [Get-ServiceFabricPartitionHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricpartitionhealth)olduğunu. İlk olarak [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet'i kullanarak kümeye bağlanın.

Aşağıdaki cmdlet kumaş Tüm bölümleri için sağlık **alır:/WordCount /WordCountService** hizmeti ve çoğaltma sağlık durumları filtreler:

```powershell
PS D:\ServiceFabric> Get-ServiceFabricPartition fabric:/WordCount/WordCountService | Get-ServiceFabricPartitionHealth -ReplicasFilter None

PartitionId           : af2e3e44-a8f8-45ac-9f31-4093eb897600
AggregatedHealthState : Warning
UnhealthyEvaluations  : 
                        Unhealthy event: SourceId='System.FM', Property='State', HealthState='Warning', ConsiderWarningAsError=false.

ReplicaHealthStates   : None
HealthEvents          : 
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Warning
                        SequenceNumber        : 72
                        SentAt                : 7/13/2017 5:57:29 PM
                        ReceivedAt            : 7/13/2017 5:57:48 PM
                        TTL                   : Infinite
                        Description           : Partition is below target replica or instance count.
                        fabric:/WordCount/WordCountService 7 2 af2e3e44-a8f8-45ac-9f31-4093eb897600
                          N/P RD _Node_2 Up 131444422260002646
                          N/S RD _Node_4 Up 131444422293113678
                          N/S RD _Node_3 Up 131444422293113679
                          N/S RD _Node_1 Up 131444422293118720
                          N/S RD _Node_0 Up 131444422293118721
                          (Showing 5 out of 5 replicas. Total available replicas: 5.)

                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Ok->Warning = 7/13/2017 5:57:48 PM, LastError = 1/1/0001 12:00:00 AM

                        SourceId              : System.PLB
                        Property              : ServiceReplicaUnplacedHealth_Secondary_af2e3e44-a8f8-45ac-9f31-4093eb897600
                        HealthState           : Warning
                        SequenceNumber        : 131444445174851664
                        SentAt                : 7/13/2017 6:35:17 PM
                        ReceivedAt            : 7/13/2017 6:35:18 PM
                        TTL                   : 00:01:05
                        Description           : The Load Balancer was unable to find a placement for one or more of the Service's Replicas:
                        Secondary replica could not be placed due to the following constraints and properties:  
                        TargetReplicaSetSize: 7
                        Placement Constraint: N/A
                        Parent Service: N/A

                        Constraint Elimination Sequence:
                        Existing Secondary Replicas eliminated 4 possible node(s) for placement -- 1/5 node(s) remain.
                        Existing Primary Replica eliminated 1 possible node(s) for placement -- 0/5 node(s) remain.

                        Nodes Eliminated By Constraints:

                        Existing Secondary Replicas -- Nodes with Partition's Existing Secondary Replicas/Instances:
                        --
                        FaultDomain:fd:/4 NodeName:_Node_4 NodeType:NodeType4 UpgradeDomain:4 UpgradeDomain: ud:/4 Deactivation Intent/Status: None/None
                        FaultDomain:fd:/3 NodeName:_Node_3 NodeType:NodeType3 UpgradeDomain:3 UpgradeDomain: ud:/3 Deactivation Intent/Status: None/None
                        FaultDomain:fd:/1 NodeName:_Node_1 NodeType:NodeType1 UpgradeDomain:1 UpgradeDomain: ud:/1 Deactivation Intent/Status: None/None
                        FaultDomain:fd:/0 NodeName:_Node_0 NodeType:NodeType0 UpgradeDomain:0 UpgradeDomain: ud:/0 Deactivation Intent/Status: None/None

                        Existing Primary Replica -- Nodes with Partition's Existing Primary Replica or Secondary Replicas:
                        --
                        FaultDomain:fd:/2 NodeName:_Node_2 NodeType:NodeType2 UpgradeDomain:2 UpgradeDomain: ud:/2 Deactivation Intent/Status: None/None


                        RemoveWhenExpired     : True
                        IsExpired             : False
                        Transitions           : Error->Warning = 7/13/2017 5:57:48 PM, LastOk = 1/1/0001 12:00:00 AM

HealthStatistics      : 
                        Replica               : 5 Ok, 0 Warning, 0 Error
```

### <a name="rest"></a>REST
Bir [GET isteği](/rest/api/servicefabric/sfclient-api-getpartitionhealth) veya vücutta açıklanan sağlık ilkeleri içeren bir [POST isteği](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-partition-by-using-a-health-policy) ile bölüm sağlık alabilirsiniz.

## <a name="get-replica-health"></a>Çoğaltma durumu alın
Bir devlet hizmeti yineleme veya devletsiz hizmet örneği sistem durumunu döndürür. Giriş:

* [Gerekli] Çoğaltmayı tanımlayan bölüm kimliği (GUID) ve çoğaltma kimliği.
* [İsteğe bağlı] Uygulama bildirimi ilkelerini geçersiz kılmak için kullanılan uygulama durumu ilkesi parametreleri.
* [İsteğe bağlı] Hangi girişlerin ilgi çekici olduğunu ve sonuç olarak döndürülmesi gerektiğini belirten olaylar için filtreler (örneğin, yalnızca hatalar veya hem uyarılar hem de hatalar). Tüm olaylar, filtreden bağımsız olarak toplanan varlığı değerlendirmek için kullanılır.

### <a name="api"></a>API
API üzerinden çoğaltma durumu almak için, bir `FabricClient` oluşturmak ve Onun HealthManager [getReplicaHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getreplicahealthasync) yöntemi arayın. Gelişmiş parametreleri belirtmek için [ReplicaHealthQueryDescription'ı](https://docs.microsoft.com/dotnet/api/system.fabric.description.replicahealthquerydescription)kullanın.

```csharp
ReplicaHealth replicaHealth = await fabricClient.HealthManager.GetReplicaHealthAsync(partitionId, replicaId);
```

### <a name="powershell"></a>PowerShell
Çoğaltma sağlık almak için cmdlet [Get-ServiceFabricReplicaHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricreplicahealth)olduğunu. İlk olarak [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet'i kullanarak kümeye bağlanın.

Aşağıdaki cmdlet hizmetin tüm bölümleri için birincil yineleme nin durumunu alır:

```powershell
PS D:\ServiceFabric> Get-ServiceFabricPartition fabric:/WordCount/WordCountService | Get-ServiceFabricReplica | where {$_.ReplicaRole -eq "Primary"} | Get-ServiceFabricReplicaHealth


PartitionId           : af2e3e44-a8f8-45ac-9f31-4093eb897600
ReplicaId             : 131444422260002646
AggregatedHealthState : Ok
HealthEvents          : 
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 131444422263668344
                        SentAt                : 7/13/2017 5:57:06 PM
                        ReceivedAt            : 7/13/2017 5:57:18 PM
                        TTL                   : Infinite
                        Description           : Replica has been created._Node_2
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 7/13/2017 5:57:18 PM, LastWarning = 1/1/0001 12:00:00 AM
```

### <a name="rest"></a>REST
Bir [GET isteği](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-replica) veya vücutta açıklanan sağlık ilkeleri içeren bir [POST isteği](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-replica-by-using-a-health-policy) ile çoğaltma sağlık alabilirsiniz.

## <a name="get-deployed-application-health"></a>Dağıtılan uygulama durumu
Düğüm varlığında dağıtılan bir uygulamanın durumunu döndürür. Dağıtılan hizmet paketi sistem durumu durumlarını içerir. Giriş:

* [Gerekli] Dağıtılan uygulamayı tanımlayan uygulama adı (URI) ve düğüm adı (dize).
* [İsteğe bağlı] Uygulama bildirimi ilkelerini geçersiz kılmak için kullanılan uygulama durumu ilkesi.
* [İsteğe bağlı] Hangi girişlerin ilgi çekici olduğunu ve sonuç olarak döndürülmesi gerektiğini belirten olaylar ve dağıtılmış hizmet paketleri için filtreler (örneğin, yalnızca hatalar veya hem uyarılar hem de hatalar). Tüm olaylar ve dağıtılan hizmet paketleri, filtreden bağımsız olarak toplanan varlığı değerlendirmek için kullanılır.
* [İsteğe bağlı] Sistem durumu istatistiklerini hariç tutmak için filtre uygulayın. Belirtilmemişse, sistem durumu istatistikleri tamam, uyarı ve hata durumu durumlarında dağıtılan hizmet paketlerinin sayısını gösterir.

### <a name="api"></a>API
API aracılığıyla bir düğüm üzerinde dağıtılan bir uygulamanın durumunu `FabricClient` elde etmek için, bir uygulama oluşturun ve HealthManager'da [GetDeployedApplicationHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getdeployedapplicationhealthasync) yöntemini arayın. İsteğe bağlı parametreleri belirtmek için [DeployedApplicationHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.deployedapplicationhealthquerydescription)kullanın.

```csharp
DeployedApplicationHealth health = await fabricClient.HealthManager.GetDeployedApplicationHealthAsync(
    new DeployedApplicationHealthQueryDescription(applicationName, nodeName));
```

### <a name="powershell"></a>PowerShell
Dağıtılan uygulama sağlığını almak için cmdlet [Get-ServiceFabricDeployedApplicationHealth](/powershell/module/servicefabric/get-servicefabricdeployedapplicationhealth?view=azureservicefabricps)olduğunu. İlk olarak [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet'i kullanarak kümeye bağlanın. Bir uygulamanın nerede dağıtılan olduğunu öğrenmek için [Get-ServiceFabricApplicationHealth'i](/powershell/module/servicefabric/get-servicefabricapplicationhealth?view=azureservicefabricps) çalıştırın ve dağıtılan uygulama altlarına bakın.

Aşağıdaki cmdlet kumaş ın sağlığını **alır:/WordCount** uygulaması **_Node_2.**

```powershell
PS D:\ServiceFabric> Get-ServiceFabricDeployedApplicationHealth -ApplicationName fabric:/WordCount -NodeName _Node_0


ApplicationName                    : fabric:/WordCount
NodeName                           : _Node_0
AggregatedHealthState              : Ok
DeployedServicePackageHealthStates : 
                                     ServiceManifestName   : WordCountServicePkg
                                     ServicePackageActivationId : 
                                     NodeName              : _Node_0
                                     AggregatedHealthState : Ok

                                     ServiceManifestName   : WordCountWebServicePkg
                                     ServicePackageActivationId : 
                                     NodeName              : _Node_0
                                     AggregatedHealthState : Ok

HealthEvents                       : 
                                     SourceId              : System.Hosting
                                     Property              : Activation
                                     HealthState           : Ok
                                     SequenceNumber        : 131444422261848308
                                     SentAt                : 7/13/2017 5:57:06 PM
                                     ReceivedAt            : 7/13/2017 5:57:17 PM
                                     TTL                   : Infinite
                                     Description           : The application was activated successfully.
                                     RemoveWhenExpired     : False
                                     IsExpired             : False
                                     Transitions           : Error->Ok = 7/13/2017 5:57:17 PM, LastWarning = 1/1/0001 12:00:00 AM

HealthStatistics                   : 
                                     DeployedServicePackage : 2 Ok, 0 Warning, 0 Error
```

### <a name="rest"></a>REST
Get [isteği](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-deployed-application) veya vücutta açıklanan sağlık ilkeleri içeren bir [POST isteği](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-deployed-application-by-using-a-health-policy) yle dağıtılan uygulama durumu alabilirsiniz.

## <a name="get-deployed-service-package-health"></a>Dağıtılan servis paketi sağlık durumunu alın
Dağıtılan bir hizmet paketi varlığının durumunu verir. Giriş:

* [Gerekli] Dağıtılan hizmet paketini tanımlayan uygulama adı (URI), düğüm adı (dize) ve hizmet bildirimi adı (string).
* [İsteğe bağlı] Uygulama bildirimi ilkesini geçersiz kılmak için kullanılan uygulama durumu ilkesi.
* [İsteğe bağlı] Hangi girişlerin ilgi çekici olduğunu ve sonuç olarak döndürülmesi gerektiğini belirten olaylar için filtreler (örneğin, yalnızca hatalar veya hem uyarılar hem de hatalar). Tüm olaylar, filtreden bağımsız olarak toplanan varlığı değerlendirmek için kullanılır.

### <a name="api"></a>API
Dağıtılan bir hizmet paketinin durumunu API üzerinden almak `FabricClient` için, Bir oluşturma ve HealthManager [getdeployedServicePackageHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getdeployedservicepackagehealthasync) yöntemini arayın. İsteğe bağlı parametreleri belirtmek için [DeployedServicePackageHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.deployedservicepackagehealthquerydescription)kullanın.

```csharp
DeployedServicePackageHealth health = await fabricClient.HealthManager.GetDeployedServicePackageHealthAsync(
    new DeployedServicePackageHealthQueryDescription(applicationName, nodeName, serviceManifestName));
```

### <a name="powershell"></a>PowerShell
Dağıtılan servis paketi sağlık almak için cmdlet [Get-ServiceFabricDeployedServicePackageHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricdeployedservicepackagehealth)olduğunu. İlk olarak [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet'i kullanarak kümeye bağlanın. Bir uygulamanın nerede dağıtılan ını görmek için [Get-ServiceFabricApplicationHealth'i](/powershell/module/servicefabric/get-servicefabricapplicationhealth?view=azureservicefabricps) çalıştırın ve dağıtılan uygulamalara bakın. Bir uygulamada hangi hizmet paketlerinin olduğunu görmek için [Get-ServiceFabricDeployedApplicationHealth](/powershell/module/servicefabric/get-servicefabricdeployedapplicationhealth?view=azureservicefabricps) çıktısında dağıtılan servis paketi altlarına bakın.

Aşağıdaki cmdlet kumaş **WordCountServicePkg** hizmet paketinin sağlık **alır:/WordCount** uygulaması **_Node_2**dağıtılır. Varlık, başarılı hizmet paketi ve giriş noktası etkinleştirme için **System.Hosting** raporları ve başarılı hizmet türü kaydı vardır.

```powershell
PS D:\ServiceFabric> Get-ServiceFabricDeployedApplication -ApplicationName fabric:/WordCount -NodeName _Node_2 | Get-ServiceFabricDeployedServicePackageHealth -ServiceManifestName WordCountServicePkg


ApplicationName            : fabric:/WordCount
ServiceManifestName        : WordCountServicePkg
ServicePackageActivationId : 
NodeName                   : _Node_2
AggregatedHealthState      : Ok
HealthEvents               : 
                             SourceId              : System.Hosting
                             Property              : Activation
                             HealthState           : Ok
                             SequenceNumber        : 131444422267693359
                             SentAt                : 7/13/2017 5:57:06 PM
                             ReceivedAt            : 7/13/2017 5:57:18 PM
                             TTL                   : Infinite
                             Description           : The ServicePackage was activated successfully.
                             RemoveWhenExpired     : False
                             IsExpired             : False
                             Transitions           : Error->Ok = 7/13/2017 5:57:18 PM, LastWarning = 1/1/0001 12:00:00 AM

                             SourceId              : System.Hosting
                             Property              : CodePackageActivation:Code:EntryPoint
                             HealthState           : Ok
                             SequenceNumber        : 131444422267903345
                             SentAt                : 7/13/2017 5:57:06 PM
                             ReceivedAt            : 7/13/2017 5:57:18 PM
                             TTL                   : Infinite
                             Description           : The CodePackage was activated successfully.
                             RemoveWhenExpired     : False
                             IsExpired             : False
                             Transitions           : Error->Ok = 7/13/2017 5:57:18 PM, LastWarning = 1/1/0001 12:00:00 AM

                             SourceId              : System.Hosting
                             Property              : ServiceTypeRegistration:WordCountServiceType
                             HealthState           : Ok
                             SequenceNumber        : 131444422272458374
                             SentAt                : 7/13/2017 5:57:07 PM
                             ReceivedAt            : 7/13/2017 5:57:18 PM
                             TTL                   : Infinite
                             Description           : The ServiceType was registered successfully.
                             RemoveWhenExpired     : False
                             IsExpired             : False
                             Transitions           : Error->Ok = 7/13/2017 5:57:18 PM, LastWarning = 1/1/0001 12:00:00 AM
```

### <a name="rest"></a>REST
Get [isteği](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-service-package) veya vücutta açıklanan sağlık ilkeleri içeren bir [POST isteği](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-service-package-by-using-a-health-policy) yle dağıtılmış hizmet paketi sağlık hizmeti alabilirsiniz.

## <a name="health-chunk-queries"></a>Sistem durumu öbek sorguları
Sistem durumu öbek sorguları, giriş filtreleri başına çok düzeyli küme alt larını (özyinelemeli) döndürebilir. İade edilecek çocukları seçerken çok fazla esneklik sağlayan gelişmiş filtreleri destekler. Filtreler, çocukları benzersiz tanımlayıcıya veya diğer grup tanımlayıcılarına ve/veya sağlık durumlarına göre belirtebilir. Varsayılan olarak, her zaman birinci düzey alt çocukları içeren sağlık komutlarının aksine, hiçbir alt değer dahil değildir.

[Sistem durumu sorguları,](service-fabric-view-entities-aggregated-health.md#health-queries) gerekli filtreler başına belirtilen varlığın yalnızca birinci düzey alt düzeylerini döndürür. Çocukların çocuklarını almak için, ilgi her varlık için ek sağlık API'leri aramanız gerekir. Benzer şekilde, belirli varlıkların durumunu elde etmek için, istediğiniz her varlık için bir sağlık API'si aramanız gerekir. Öbek sorgusu gelişmiş filtreleme, ileti boyutunu ve ileti sayısını en aza indirerek, tek bir sorguda birden çok ilgi çekici öğe istemenize olanak tanır.

Öbek sorgusunun değeri, tek bir çağrıda daha fazla küme varlığı (gerekli kökten başlayan potansiyel olarak tüm küme varlıkları) için sistem durumu elde edebilirsiniz. Karmaşık sistem durumu sorgusu gibi ifade edebilirsiniz:

* Yalnızca hataları yanlışlıkla döndürün ve bu uygulamalar için uyarı veya hata tüm hizmetleri içerir. Döndürülen hizmetler için tüm bölümleri ekleyin.
* Yalnızca adlarıyla belirtilen dört uygulamanın durumunu döndürün.
* Yalnızca istenilen uygulama türündeki uygulamaların sağlığını döndürün.
* Dağıtılan tüm varlıkları bir düğümüzerinde döndürün. Belirtilen düğümdeki tüm uygulamaları, tüm dağıtılan uygulamaları ve bu düğümdeki tüm dağıtılan hizmet paketlerini döndürür.
* Tüm yinelemeleri yanlışlıkla döndürün. Tüm uygulamaları, hizmetleri, bölümleri ve yalnızca yinelemeleri hatalı olarak döndürür.
* Tüm uygulamaları iade edin. Belirli bir hizmet için tüm bölümleri içerir.

Şu anda, sistem durumu öbek sorgusu yalnızca küme varlığı için açıktadır. Bir küme sistem durumu öbek döndürür:

* Küme toplanmış sağlık durumu.
* Giriş filtrelerine saygı gösteren düğümlerin sistem durumu yığın listesi.
* Giriş filtrelerine saygı gösteren uygulamaların sistem durumu yığın listesi. Her uygulama durumu durumu öbek, giriş filtrelerine saygı gösteren tüm hizmetlerle birlikte bir yığın listesi ve filtrelere saygı gösteren tüm dağıtılmış uygulamalarla birlikte bir yığın listesi içerir. Hizmetlerin ve dağıtılan uygulamaların çocukları için de geçerlidir. Bu şekilde, kümedeki tüm varlıklar istenirse hiyerarşik bir biçimde döndürülebilir.

### <a name="cluster-health-chunk-query"></a>Küme durumu öbek sorgusu
Küme varlığının durumunu verir ve gerekli alt çocukların hiyerarşik sistem durumu yığınlarını içerir. Giriş:

* [İsteğe bağlı] Düğümleri ve küme olaylarını değerlendirmek için kullanılan küme sistem durumu ilkesi.
* [İsteğe bağlı] Uygulama bildirimi ilkelerini geçersiz kılmak için kullanılan sistem durumu ilkeleriyle birlikte uygulama durumu ilkesi haritası.
* [İsteğe bağlı] Hangi girişlerin ilgi çekici olduğunu ve sonuç olarak döndürülmesi gerektiğini belirten düğümler ve uygulamalar için filtreler. Filtreler bir varlık/varlık grubuna özgütür veya bu düzeydeki tüm varlıklar için geçerlidir. Filtreler listesi, sorgu tarafından döndürülen ince taneli varlıklara belirli tanımlayıcılar için bir genel filtre ve/veya filtre içerebilir. Boşsa, çocuklar varsayılan olarak döndürülmez.
  [NodeHealthStateFilter](https://docs.microsoft.com/dotnet/api/system.fabric.health.nodehealthstatefilter) ve [ApplicationHealthStateFilter](https://docs.microsoft.com/dotnet/api/system.fabric.health.applicationhealthstatefilter)de filtreler hakkında daha fazla bilgi edinin. Uygulama filtreleri, çocuklar için gelişmiş filtreleri özyinelemeli olarak belirtebilir.

Yığın sonucu, filtrelere saygı gösteren çocukları içerir.

Şu anda, öbek sorgusu sağlıksız değerlendirmeleri veya varlık olaylarını döndürmez. Bu ek bilgi varolan küme durumu sorgusu kullanılarak elde edilebilir.

### <a name="api"></a>API
Küme durumu öbek almak `FabricClient` için, bir oluşturmak ve Onun **HealthManager** [getClusterHealthChunkAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getclusterhealthchunkasync) yöntemi arayın. Sağlık ilkeleri ve gelişmiş filtreleri açıklamak için [ClusterHealthQueryDescription'dan](https://docs.microsoft.com/dotnet/api/system.fabric.description.clusterhealthchunkquerydescription) geçebilirsiniz.

Aşağıdaki kod gelişmiş filtrelerile küme durumu öbek alır.

```csharp
var queryDescription = new ClusterHealthChunkQueryDescription();
queryDescription.ApplicationFilters.Add(new ApplicationHealthStateFilter()
    {
        // Return applications only if they are in error
        HealthStateFilter = HealthStateFilter.Error
    });

// Return all replicas
var wordCountServiceReplicaFilter = new ReplicaHealthStateFilter()
    {
        HealthStateFilter = HealthStateFilter.All
    };

// Return all replicas and all partitions
var wordCountServicePartitionFilter = new PartitionHealthStateFilter()
    {
        HealthStateFilter = HealthStateFilter.All
    };
wordCountServicePartitionFilter.ReplicaFilters.Add(wordCountServiceReplicaFilter);

// For specific service, return all partitions and all replicas
var wordCountServiceFilter = new ServiceHealthStateFilter()
{
    ServiceNameFilter = new Uri("fabric:/WordCount/WordCountService"),
};
wordCountServiceFilter.PartitionFilters.Add(wordCountServicePartitionFilter);

// Application filter: for specific application, return no services except the ones of interest
var wordCountApplicationFilter = new ApplicationHealthStateFilter()
    {
        // Always return fabric:/WordCount application
        ApplicationNameFilter = new Uri("fabric:/WordCount"),
    };
wordCountApplicationFilter.ServiceFilters.Add(wordCountServiceFilter);

queryDescription.ApplicationFilters.Add(wordCountApplicationFilter);

var result = await fabricClient.HealthManager.GetClusterHealthChunkAsync(queryDescription);
```

### <a name="powershell"></a>PowerShell
Cmdlet küme sağlık almak için [Get-ServiceFabricClusterChunkHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricclusterhealthchunk)olduğunu. İlk olarak [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet'i kullanarak kümeye bağlanın.

Aşağıdaki kod, her zaman döndürülmesi gereken belirli bir düğüm dışında yalnızca Hata'da olmaları durumunda düğümleri alır.

```xml
PS D:\ServiceFabric> $errorFilter = [System.Fabric.Health.HealthStateFilter]::Error;
$allFilter = [System.Fabric.Health.HealthStateFilter]::All;

$nodeFilter1 = New-Object System.Fabric.Health.NodeHealthStateFilter -Property @{HealthStateFilter=$errorFilter}
$nodeFilter2 = New-Object System.Fabric.Health.NodeHealthStateFilter -Property @{NodeNameFilter="_Node_1";HealthStateFilter=$allFilter}
# Create node filter list that will be passed in the cmdlet
$nodeFilters = New-Object System.Collections.Generic.List[System.Fabric.Health.NodeHealthStateFilter]
$nodeFilters.Add($nodeFilter1)
$nodeFilters.Add($nodeFilter2)

Get-ServiceFabricClusterHealthChunk -NodeFilters $nodeFilters


HealthState                  : Warning
NodeHealthStateChunks        : 
                               TotalCount            : 1

                               NodeName              : _Node_1
                               HealthState           : Ok

ApplicationHealthStateChunks : None
```

Aşağıdaki cmdlet uygulama filtreleri ile küme yığın alır.

```xml
PS D:\ServiceFabric> $errorFilter = [System.Fabric.Health.HealthStateFilter]::Error;
$allFilter = [System.Fabric.Health.HealthStateFilter]::All;

# All replicas
$replicaFilter = New-Object System.Fabric.Health.ReplicaHealthStateFilter -Property @{HealthStateFilter=$allFilter}

# All partitions
$partitionFilter = New-Object System.Fabric.Health.PartitionHealthStateFilter -Property @{HealthStateFilter=$allFilter}
$partitionFilter.ReplicaFilters.Add($replicaFilter)

# For WordCountService, return all partitions and all replicas
$svcFilter1 = New-Object System.Fabric.Health.ServiceHealthStateFilter -Property @{ServiceNameFilter="fabric:/WordCount/WordCountService"}
$svcFilter1.PartitionFilters.Add($partitionFilter)

$svcFilter2 = New-Object System.Fabric.Health.ServiceHealthStateFilter -Property @{HealthStateFilter=$errorFilter}

$appFilter = New-Object System.Fabric.Health.ApplicationHealthStateFilter -Property @{ApplicationNameFilter="fabric:/WordCount"}
$appFilter.ServiceFilters.Add($svcFilter1)
$appFilter.ServiceFilters.Add($svcFilter2)

$appFilters = New-Object System.Collections.Generic.List[System.Fabric.Health.ApplicationHealthStateFilter]
$appFilters.Add($appFilter)

Get-ServiceFabricClusterHealthChunk -ApplicationFilters $appFilters


HealthState                  : Error
NodeHealthStateChunks        : None
ApplicationHealthStateChunks : 
                               TotalCount            : 1

                               ApplicationName       : fabric:/WordCount
                               ApplicationTypeName   : WordCount
                               HealthState           : Error
                               ServiceHealthStateChunks : 
                                TotalCount            : 1

                                ServiceName           : fabric:/WordCount/WordCountService
                                HealthState           : Error
                                PartitionHealthStateChunks : 
                                    TotalCount            : 1

                                    PartitionId           : af2e3e44-a8f8-45ac-9f31-4093eb897600
                                    HealthState           : Error
                                    ReplicaHealthStateChunks : 
                                        TotalCount            : 5

                                        ReplicaOrInstanceId   : 131444422293118720
                                        HealthState           : Ok

                                        ReplicaOrInstanceId   : 131444422293118721
                                        HealthState           : Ok

                                        ReplicaOrInstanceId   : 131444422293113678
                                        HealthState           : Ok

                                        ReplicaOrInstanceId   : 131444422293113679
                                        HealthState           : Ok

                                        ReplicaOrInstanceId   : 131444422260002646
                                        HealthState           : Error
```

Aşağıdaki cmdlet, dağıtılan tüm varlıkları bir düğüm üzerinde döndürür.

```xml
PS D:\ServiceFabric> $errorFilter = [System.Fabric.Health.HealthStateFilter]::Error;
$allFilter = [System.Fabric.Health.HealthStateFilter]::All;

$dspFilter = New-Object System.Fabric.Health.DeployedServicePackageHealthStateFilter -Property @{HealthStateFilter=$allFilter}
$daFilter =  New-Object System.Fabric.Health.DeployedApplicationHealthStateFilter -Property @{HealthStateFilter=$allFilter;NodeNameFilter="_Node_2"}
$daFilter.DeployedServicePackageFilters.Add($dspFilter)

$appFilter = New-Object System.Fabric.Health.ApplicationHealthStateFilter -Property @{HealthStateFilter=$allFilter}
$appFilter.DeployedApplicationFilters.Add($daFilter)

$appFilters = New-Object System.Collections.Generic.List[System.Fabric.Health.ApplicationHealthStateFilter]
$appFilters.Add($appFilter)
Get-ServiceFabricClusterHealthChunk -ApplicationFilters $appFilters


HealthState                  : Error
NodeHealthStateChunks        : None
ApplicationHealthStateChunks : 
                               TotalCount            : 2

                               ApplicationName       : fabric:/System
                               HealthState           : Ok
                               DeployedApplicationHealthStateChunks : 
                                TotalCount            : 1

                                NodeName              : _Node_2
                                HealthState           : Ok
                                DeployedServicePackageHealthStateChunks :
                                    TotalCount            : 1

                                    ServiceManifestName   : FAS
                                    ServicePackageActivationId : 
                                    HealthState           : Ok



                               ApplicationName       : fabric:/WordCount
                               ApplicationTypeName   : WordCount
                               HealthState           : Error
                               DeployedApplicationHealthStateChunks : 
                                TotalCount            : 1

                                NodeName              : _Node_2
                                HealthState           : Ok
                                DeployedServicePackageHealthStateChunks :
                                    TotalCount            : 1

                                    ServiceManifestName   : WordCountServicePkg
                                    ServicePackageActivationId : 
                                    HealthState           : Ok
```

### <a name="rest"></a>REST
Bir [GET isteği](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-cluster-using-health-chunks) veya vücutta açıklanan sistem durumu ilkeleri ve gelişmiş filtreleri içeren bir [POST isteğiyle](https://docs.microsoft.com/rest/api/servicefabric/health-of-cluster) küme durumu öbek alabilirsiniz.

## <a name="general-queries"></a>Genel sorgular
Genel sorgular, belirli bir türdeki Hizmet Kumaşı varlıklarının listesini döndürür. Onlar API **(FabricClient.QueryManager**yöntemleri ile), PowerShell cmdlets ve REST ile maruz kalır. Bu sorgular, birden çok bileşenden alt sorguları toplar. Bunlardan biri, her sorgu sonucu için toplu sağlık durumunu dolduran [sağlık deposudur.](service-fabric-health-introduction.md#health-store)  

> [!NOTE]
> Genel sorgular varlığın toplu sağlık durumunu döndürür ve zengin sistem durumu verileri içermez. Bir varlık sağlıklı değilse, olaylar, çocuk sağlığı durumları ve sağlıksız değerlendirmeler de dahil olmak üzere tüm sağlık bilgilerini almak için sağlık sorgularını takip edebilirsiniz.
>
>

Genel sorgular bir varlık için bilinmeyen bir sistem durumu döndürürse, sistem durumu deposunun varlık la ilgili tam veriye sahip olmaması mümkündür. Sistem durumu deposuna yapılan bir alt sorgunun başarılı olmaması da mümkündür (örneğin, bir iletişim hatası veya sistem durumu deposu azaltıldı). Varlık için bir sistem durumu sorgusuyla izleyin. Alt sorgu ağ sorunları gibi geçici hatalarla karşılaşırsa, bu izleme sorgusu başarılı olabilir. Ayrıca, varlığın neden maruz kalınmadığı hakkında sağlık mağazasından daha fazla bilgi verebilir.

Varlıklar için **HealthState** içeren sorgular şunlardır:

* Düğüm listesi: Kümedeki (sayfalı) liste düğümlerini döndürür.
  * API: [FabricClient.QueryClient.GetNodeListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getnodelistasync)
  * PowerShell: Get-ServiceFabricNode
* Uygulama listesi: Kümedeki (sayfalı) uygulamaların listesini verir.
  * API: [FabricClient.QueryClient.GetApplicationListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getapplicationlistasync)
  * PowerShell: Get-ServiceFabricApplication
* Hizmet listesi: Bir uygulamadaki hizmetlerin listesini verir (sayfalı).
  * API: [FabricClient.QueryClient.GetServiceListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getservicelistasync)
  * PowerShell: Get-ServiceFabricService
* Bölüm listesi: Bir hizmetteki (sayfalı) bölümlerin listesini verir.
  * API: [FabricClient.QueryClient.GetPartitionListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getpartitionlistasync)
  * PowerShell: Get-ServiceFabricPartition
* Yineleme listesi: Bir bölümdeki (sayfalı) yineleme listesini verir.
  * API: [FabricClient.QueryClient.GetReplicaListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getreplicalistasync)
  * PowerShell: Get-ServiceFabricReplica
* Dağıtılan uygulama listesi: Bir düğüm üzerinde dağıtılan uygulamaların listesini verir.
  * API: [FabricClient.QueryClient.GetDeployedApplicationListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getdeployedapplicationlistasync)
  * PowerShell: Get-ServiceFabricDeployedApplication
* Dağıtılan hizmet paketi listesi: Dağıtılan bir uygulamada hizmet paketleri listesini verir.
  * API: [FabricClient.QueryClient.GetDeployedServicePackageListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getdeployedservicepackagelistasync)
  * PowerShell: Get-ServiceFabricDeployedApplication

> [!NOTE]
> Sorguların bazıları çağrılı sonuçları döndürer. Bu sorguların dönüşü [PagedList\<T>'nden ](https://docs.microsoft.com/dotnet/api/system.fabric.query.pagedlist-1)türetilen bir listedir. Sonuçlar bir iletiye uymuyorsa, yalnızca bir sayfa döndürülür ve numaralandırmanın durdurulduğu yeri izleyen bir ContinuationToken döndürülür. Sonraki sonuçları almak için aynı sorguyu çağırmaya ve önceki sorgudan devam jetonuna geçirmeye devam edin.

### <a name="examples"></a>Örnekler
Aşağıdaki kod kümedeki sağlıksız uygulamaları alır:

```csharp
var applications = fabricClient.QueryManager.GetApplicationListAsync().Result.Where(
  app => app.HealthState == HealthState.Error);
```

Aşağıdaki cmdlet kumaş:/WordCount uygulaması için uygulama bilgilerini alır. Sağlık durumunun uyarıda olduğuna dikkat edin.

```powershell
PS C:\> Get-ServiceFabricApplication -ApplicationName fabric:/WordCount

ApplicationName        : fabric:/WordCount
ApplicationTypeName    : WordCount
ApplicationTypeVersion : 1.0.0
ApplicationStatus      : Ready
HealthState            : Warning
ApplicationParameters  : { "WordCountWebService_InstanceCount" = "1";
                         "_WFDebugParams_" = "[{"ServiceManifestName":"WordCountWebServicePkg","CodePackageName":"Code","EntryPointType":"Main","Debug
                         ExePath":"C:\\Program Files (x86)\\Microsoft Visual Studio
                         14.0\\Common7\\Packages\\Debugger\\VsDebugLaunchNotify.exe","DebugArguments":" {74f7e5d5-71a9-47e2-a8cd-1878ec4734f1} -p
                         [ProcessId] -tid [ThreadId]","EnvironmentBlock":"_NO_DEBUG_HEAP=1\u0000"},{"ServiceManifestName":"WordCountServicePkg","CodeP
                         ackageName":"Code","EntryPointType":"Main","DebugExePath":"C:\\Program Files (x86)\\Microsoft Visual Studio
                         14.0\\Common7\\Packages\\Debugger\\VsDebugLaunchNotify.exe","DebugArguments":" {2ab462e6-e0d1-4fda-a844-972f561fe751} -p
                         [ProcessId] -tid [ThreadId]","EnvironmentBlock":"_NO_DEBUG_HEAP=1\u0000"}]" }
```

Aşağıdaki cmdlet hata bir sağlık durumu ile hizmetleri alır:

```powershell
PS D:\ServiceFabric> Get-ServiceFabricApplication | Get-ServiceFabricService | where {$_.HealthState -eq "Error"}


ServiceName            : fabric:/WordCount/WordCountService
ServiceKind            : Stateful
ServiceTypeName        : WordCountServiceType
IsServiceGroup         : False
ServiceManifestVersion : 1.0.0
HasPersistedState      : True
ServiceStatus          : Active
HealthState            : Error
```

## <a name="cluster-and-application-upgrades"></a>Küme ve uygulama yükseltmeleri
Küme nin ve uygulamanın izlenen yükseltmesi sırasında Service Fabric, her şeyin sağlıklı kalmasını sağlamak için sağlığı denetler. Bir varlık yapılandırılan sistem durumu ilkeleri kullanılarak değerlendirildiği kadar sağlıksızsa, yükseltme sonraki eylemi belirlemek için yükseltmeye özgü ilkeler uygular. Yükseltme, kullanıcı etkileşimine (hata koşullarını düzeltme veya ilkeleri değiştirme gibi) izin vermek için duraklatılmış olabilir veya otomatik olarak önceki iyi sürüme geri dönebilir.

*Küme* yükseltmesi sırasında küme yükseltme durumunu alabilirsiniz. Yükseltme durumu, kümedeki sağlıksız değerlendirmeleri işaret eden sağlıksız değerlendirmeler içerir. Yükseltme, sistem durumu sorunları nedeniyle geri alınırsa, yükseltme durumu son sağlıksız nedenleri hatırlar. Bu bilgiler, yöneticilerin yükseltme geri alındıktan veya durdurulduktan sonra neyin yanlış gittiğini araştırmana yardımcı olabilir.

Benzer şekilde, bir *uygulama* yükseltmesırasında, tüm sağlıksız değerlendirmeler uygulama yükseltme durumunda bulunur.

Aşağıdaki değiştirilmiş bir kumaş için uygulama yükseltme durumunu gösterir:/WordCount uygulaması. Bir izleme örgütü kopyalarından birinde bir hata bildirdi. Sistem durumu denetimleri ne kadar önemli değilse, yükseltme geri alınır.

```powershell
PS C:\> Get-ServiceFabricApplicationUpgrade fabric:/WordCount

ApplicationName               : fabric:/WordCount
ApplicationTypeName           : WordCount
TargetApplicationTypeVersion  : 1.0.0.0
ApplicationParameters         : {}
StartTimestampUtc             : 4/21/2017 5:23:26 PM
FailureTimestampUtc           : 4/21/2017 5:23:37 PM
FailureReason                 : HealthCheck
UpgradeState                  : RollingBackInProgress
UpgradeDuration               : 00:00:23
CurrentUpgradeDomainDuration  : 00:00:00
CurrentUpgradeDomainProgress  : UD1

                                NodeName            : _Node_1
                                UpgradePhase        : Upgrading

                                NodeName            : _Node_2
                                UpgradePhase        : Upgrading

                                NodeName            : _Node_3
                                UpgradePhase        : PreUpgradeSafetyCheck
                                PendingSafetyChecks :
                                EnsurePartitionQuorum - PartitionId: 30db5be6-4e20-4698-8185-4bd7ca744020
NextUpgradeDomain             : UD2
UpgradeDomainsStatus          : { "UD1" = "Completed";
                                "UD2" = "Pending";
                                "UD3" = "Pending";
                                "UD4" = "Pending" }
UnhealthyEvaluations          :
                                Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.

                                  Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Error'.

                                      Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.

                                      Unhealthy partition: PartitionId='a1f83a35-d6bf-4d39-b90d-28d15f39599b', AggregatedHealthState='Error'.

                                          Unhealthy replicas: 20% (1/5), MaxPercentUnhealthyReplicasPerPartition=0%.

                                          Unhealthy replica: PartitionId='a1f83a35-d6bf-4d39-b90d-28d15f39599b',
                                  ReplicaOrInstanceId='131031502346844058', AggregatedHealthState='Error'.

                                              Error event: SourceId='DiskWatcher', Property='Disk'.

UpgradeKind                   : Rolling
RollingUpgradeMode            : UnmonitoredAuto
ForceRestart                  : False
UpgradeReplicaSetCheckTimeout : 00:15:00
```

Service Fabric [uygulama yükseltmesi](service-fabric-application-upgrade.md)hakkında daha fazla bilgi edinin.

## <a name="use-health-evaluations-to-troubleshoot"></a>Sorun giderme için sistem durumu değerlendirmelerini kullanma
Küme veya uygulamayla ilgili bir sorun olduğunda, sorunun ne olduğunu saptamak için kümeye veya uygulama sağlığına bakın. Sağlıksız değerlendirmeler, mevcut sağlıksız durumu neyin tetiklediği hakkında ayrıntılı bilgi sağlar. Gerekirse, temel nedeni belirlemek için sağlıksız çocuk varlıklarına sondaj yapabilirsiniz.

Örneğin, yinelemelerinden birinde bir hata raporu olduğundan, uygulamanın sağlıksız olduğunu düşünün. Aşağıdaki Powershell cmdlet sağlıksız değerlendirmeleri gösterir:

```powershell
PS D:\ServiceFabric> Get-ServiceFabricApplicationHealth fabric:/WordCount -EventsFilter None -ServicesFilter None -DeployedApplicationsFilter None -ExcludeHealthStatistics


ApplicationName                 : fabric:/WordCount
AggregatedHealthState           : Error
UnhealthyEvaluations            : 
                                  Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.

                                  Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Error'.

                                    Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.

                                    Unhealthy partition: PartitionId='af2e3e44-a8f8-45ac-9f31-4093eb897600', AggregatedHealthState='Error'.

                                        Unhealthy replicas: 20% (1/5), MaxPercentUnhealthyReplicasPerPartition=0%.

                                        Unhealthy replica: PartitionId='af2e3e44-a8f8-45ac-9f31-4093eb897600', ReplicaOrInstanceId='131444422260002646', AggregatedHealthState='Error'.

                                            Error event: SourceId='MyWatchdog', Property='Memory'.

ServiceHealthStates             : None
DeployedApplicationHealthStates : None
HealthEvents                    : None
```

Daha fazla bilgi almak için yinelemeye bakabilirsiniz:

```powershell
PS D:\ServiceFabric> Get-ServiceFabricReplicaHealth -ReplicaOrInstanceId 131444422260002646 -PartitionId af2e3e44-a8f8-45ac-9f31-4093eb897600


PartitionId           : af2e3e44-a8f8-45ac-9f31-4093eb897600
ReplicaId             : 131444422260002646
AggregatedHealthState : Error
UnhealthyEvaluations  : 
                        Error event: SourceId='MyWatchdog', Property='Memory'.

HealthEvents          : 
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 131444422263668344
                        SentAt                : 7/13/2017 5:57:06 PM
                        ReceivedAt            : 7/13/2017 5:57:18 PM
                        TTL                   : Infinite
                        Description           : Replica has been created._Node_2
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 7/13/2017 5:57:18 PM, LastWarning = 1/1/0001 12:00:00 AM

                        SourceId              : MyWatchdog
                        Property              : Memory
                        HealthState           : Error
                        SequenceNumber        : 131444451657749403
                        SentAt                : 7/13/2017 6:46:05 PM
                        ReceivedAt            : 7/13/2017 6:46:05 PM
                        TTL                   : Infinite
                        Description           : 
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Warning->Error = 7/13/2017 6:46:05 PM, LastOk = 1/1/0001 12:00:00 AM
```

> [!NOTE]
> Sağlıksız değerlendirmeler, varlığın mevcut sağlık durumuna değerlendirilmesinin ilk nedenini göstermektedir. Bu durumu tetikleyen birden çok başka olay olabilir, ancak bunlar değerlendirmelere yansıtılmamaktadır. Daha fazla bilgi almak için, kümedeki tüm sağlıksız raporları bulmak için sağlık varlıklarını ayrıntılı olarak araştırın.
>
>

## <a name="next-steps"></a>Sonraki adımlar
[Sorun gidermek için sistem durumu raporlarını kullanma](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)

[Özel Hizmet Kumaşı sağlık raporları ekleme](service-fabric-report-health.md)

[Hizmet durumunu bildirme ve kontrol etme](service-fabric-diagnostics-how-to-report-and-check-service-health.md)

[Hizmetleri yerel olarak izleme ve tanılama](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Hizmet Kumaş uygulama yükseltme](service-fabric-application-upgrade.md)
