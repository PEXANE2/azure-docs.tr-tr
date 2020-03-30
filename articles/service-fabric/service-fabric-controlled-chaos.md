---
title: Hizmet Kumaş kümelerinde Kaos adüklemek
description: Kümedeki Kaos'u yönetmek için Hata Enjeksiyonu ve Küme Analizi Hizmeti API'lerini kullanma.
author: motanv
ms.topic: conceptual
ms.date: 02/05/2018
ms.author: motanv
ms.openlocfilehash: 37b451abd0a519dff17aba9b2d6c42b4762f30cd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75463178"
---
# <a name="induce-controlled-chaos-in-service-fabric-clusters"></a>Hizmet Kumaş kümelerinde kontrollü Kaos uğrama
Bulut altyapıları gibi büyük ölçekli dağıtılmış sistemler doğal olarak güvenilmezdir. Azure Hizmet Yapısı, geliştiricilerin güvenilir olmayan bir altyapının üzerine güvenilir dağıtılmış hizmetler yazmalarını sağlar. Güvenilmez bir altyapının üzerine sağlam dağıtılmış hizmetler yazmak için, altta yatan güvenilmez altyapı hatalar nedeniyle karmaşık devlet geçişlerinden geçerken geliştiricilerin hizmetlerinin kararlılığını sınayabilmesi gerekir.

[Arıza Enjeksiyonu ve Küme Analizi Hizmeti](https://docs.microsoft.com/azure/service-fabric/service-fabric-testability-overview) (Hata Analizi Hizmeti olarak da bilinir) geliştiricilere hizmetlerini test etmek için hataları neden etme olanağı sağlar. Bu hedeflenen simüle hatalar, [bir bölümü yeniden başlatma](https://docs.microsoft.com/powershell/module/servicefabric/start-servicefabricpartitionrestart?view=azureservicefabricps)gibi, en yaygın durum geçişleri egzersiz yardımcı olabilir. Ancak hedeflenen simüle hatalar tanım olarak önyargılı ve bu nedenle devlet geçişleri sadece zor tahmin, uzun ve karmaşık dizi göstermek hataları kaçırabilir. Tarafsız bir test için Chaos'u kullanabilirsiniz.

Kaos, küme boyunca uzun süreler boyunca periyodik, ara sıra verilen hataları (hem zarif hem de zarif olmayan) simüle eder. Zarif bir hata Hizmet Kumaş API çağrıları kümesi oluşur, örneğin, çoğaltma yıkıntımı yeniden başlatma bu bir çoğaltma açık tarafından takip yakın olduğundan zarif bir hatadır. Yinelemeyi kaldırın, birincil yinelemeyi taşıyın ve ikincil yinelemeyi taşıyın, Kaos tarafından yapılan diğer zarif hatalardır. Ungraceful hataları işlem çıkışları, yeniden düğüm ve yeniden kodu paketi gibi. 

Kaos'u oranı ve hataların türüyle yapılandırdıktan sonra, kümede ve hizmetlerinizde hata oluşturmaya başlamak için C#, Powershell veya REST API ile Kaos'u başlatabilirsiniz. Kaos'u belirli bir süre (örneğin, bir saat için) çalışacak şekilde yapılandırabilirsiniz, ardından Kaos otomatik olarak durur veya istediğiniz zaman durdurmak için StopChaos API'sını (C#, Powershell veya REST) arayabilirsiniz.

> [!NOTE]
> Mevcut haliyle, Kaos yalnızca güvenli hatalara neden olur, bu da dış hataların yokluğunda bir çoğunluk kaybının veya veri kaybının asla gerçekleşmediği anlamına gelir.
>

Kaos çalışırken, şu anda çalıştırma durumunu yakalayan farklı olaylar üretir. Örneğin, bir ExecutingFaultsEvent, Chaos'un bu yinelemede yürütmeye karar verdiği tüm hataları içerir. DoğrulamaFailedEvent kümenin doğrulanması sırasında bulunan bir doğrulama hatası (sistem durumu veya kararlılık sorunları) ayrıntılarını içerir. Kaos raporu almak için GetChaosReport API'yi (C#, Powershell veya REST) çağırabilirsiniz. Bu olaylar, iki yapılandırma tarafından dikte edilen bir kesilme ilkesine sahip olan güvenilir bir [sözlükte](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-reliable-collections)kalıcıdır: **MaxStoredChaosEventCount** (varsayılan değer 25000'dir) ve **StoredActionCleanupIntervalInSeconds** (varsayılan değer 3600'dür). Her *StoredActionCleanupIntervalInSeconds* Chaos kontrolleri ve tüm ama en son *MaxStoredChaosEventCount* olaylar, güvenilir sözlükten tasfiye edilir.

## <a name="faults-induced-in-chaos"></a>Kaos'ta indüklenen hatalar
Kaos, Tüm Service Fabric kümesinde hatalar oluşturur ve aylar veya yıllar içinde görülen hataları birkaç saate sıkıştırır. Ara sıra yapılan hataların yüksek hata oranı ile birleşimi, aksi takdirde gözden kçıkabilecek köşe durumlarını bulur. Kaos bu egzersiz hizmetin kod kalitesinde önemli bir iyileşmeye yol açar.

Kaos aşağıdaki kategorilerden hataları neden olur:

* Düğümü yeniden başlat
* Dağıtılan kod paketini yeniden başlatma
* Yinelemeyi kaldırma
* Yinelemeyi yeniden başlatma
* Birincil yinelemeyi taşıma (yapılandırılabilir)
* İkincil yinelemeyi taşıma (yapılandırılabilir)

Kaos birden çok yinelemede çalışır. Her yineleme, belirtilen döneme ait hatalar dan ve küme doğrulamadan oluşur. Kümenin stabilize edilmesi ve doğrulamanın başarılı olması için harcanan zamanı yapılandırabilirsiniz. Küme doğrulamasında bir hata bulunursa, Kaos UTC zaman damgası ve hata ayrıntılarıyla bir DoğrulamaFailedEvent oluşturur ve devam eder. Örneğin, en fazla üç eşzamanlı hatayla bir saat çalışacak şekilde ayarlanmış bir Kaos örneğini düşünün. Kaos üç hataya neden olur ve küme sağlığını doğrular. StopChaosAsync API veya bir saatlik geçişler yoluyla açıkça durdurulana kadar önceki adımı yineler. Küme herhangi bir yinelemede sağlıksız hale gelirse (yani, sabitlenmez veya geçirilen MaxClusterStabilizationTimeout içinde sağlıklı hale gelmez), Kaos bir DoğrulamaFailedEvent oluşturur. Bu olay, bir şeylerin ters gittiğini ve daha fazla araştırma gerekebileceğini gösterir.

Kaos'un neden olduğu hataları almak için GetChaosReport API'yi (powershell, C#veya REST) kullanabilirsiniz. API, geçen devam belirteci veya geçirilen zaman aralığını temel alan Kaos raporunun bir sonraki bölümünü alır. Kaos raporunun bir sonraki bölümünü almak için ContinuationToken'ı belirtebilirsiniz veya StartTimeUtc ve EndTimeUtc üzerinden zaman aralığını belirtebilirsiniz, ancak aynı çağrıda hem ContinuationToken'ı hem de zaman aralığını belirtemezsiniz. 100'den fazla Kaos olayı olduğunda, Kaos raporu, bir segmentin en fazla 100 Kaos olayı nın bulunduğu bölümler halinde döndürülür.

## <a name="important-configuration-options"></a>Önemli yapılandırma seçenekleri
* **TimeToRun**: Kaos'un başarı ile bitmeden çalıştığı toplam süre. Chaos'u StopChaos API'si aracılığıyla TimeToRun dönemi boyunca çalıştırmadan durdurabilirsiniz.

* **MaxClusterStabilizationTimeout**: Bir DoğrulamaFailedEvent üretmeden önce kümenin sağlıklı olmasını beklemek için maksimum süre. Bu bekleme, toparlanırken kümeüzerindeki yükü azaltmaktır. Gerçekleştirilen denetimler şunlardır:
  * Küme durumu iyiyse
  * Hizmet durumu iyiyse
  * Hizmet bölümü için hedef çoğaltma kümesi boyutu elde edilirse
  * InBuild yinelemeleri yok
* **MaxConcurrentFaults**: Her yinelemede indüklenen en fazla eşzamanlı hata sayısıdır. Sayı ne kadar yüksekse, kaos o kadar agresiftir ve kümenin geçtiği başarısızlıklar ve durum geçiş kombinasyonları da o kadar karmaşıktır. 

> [!NOTE]
> Ne olursa olsun ne kadar yüksek bir değer *MaxConcurrentFaults* vardır, Kaos garanti - dış hatalar yokluğunda - hiçbir çoğunluk kaybı veya veri kaybı yoktur.
>

* **EnableMoveReplicaFaults**: Birincil veya ikincil yinelemelerin taşınmasına neden olan hataları etkinleştirer veya devre dışı eder. Bu hatalar varsayılan olarak etkinleştirilir.
* **WaitTimeBetweenIterations**: Yinelemeler arasında bekleme süresi. Diğer bir tarihte, Kaos'un bir hata turunu uyguladıktan ve kümenin sağlığının ilgili doğrulanmasını tamamladıktan sonra duraklama süresi. Değer ne kadar yüksekse, ortalama hata enjeksiyon hızı da o kadar düşüktür.
* **WaitTimeBetweenFaults**: Tek bir yinelemede art arda iki hata arasında bekleme süresi. Değer ne kadar yüksekse, hataların eşzamanlılığı (veya çakışması) o kadar düşük olur.
* **ClusterHealthPolicy**: Küme durumu ilkesi, Kaos yinelemeleri arasındaki kümenin durumunu doğrulamak için kullanılır. Küme durumu hatalıysa veya hata yürütme sırasında beklenmeyen bir özel durum gerçekleşirse, Kaos kümeyi iyileşmek için biraz zaman sağlamak için bir sonraki sistem durumu denetiminden önce 30 dakika bekler.
* **Bağlam**: (string, string) türü anahtar değeri çiftleri topluluğu. Harita, Kaos koşusu hakkındaki bilgileri kaydetmek için kullanılabilir. Bu tür 100'den fazla çift olamaz ve her dize (anahtar veya değer) en fazla 4095 karakter uzunluğunda olabilir. Bu harita, belirli çalıştırmayla ilgili bağlamı isteğe bağlı olarak depolamak için Kaos çalıştırın başlatıcısı tarafından ayarlanır.
* **ChaosTargetFilter**: Bu filtre, Kaos hatalarını yalnızca belirli düğüm türlerine veya yalnızca belirli uygulama örneklerine hedeflemek için kullanılabilir. ChaosTargetFilter kullanılmazsa, Kaos tüm küme varlıklarını hatalar. ChaosTargetFilter kullanılırsa, Chaos yalnızca ChaosTargetFilter belirtimini karşılayan varlıkları hatalar. NodeTypeInclusionList ve ApplicationInclusionList yalnızca birlik semantikine izin verir. Başka bir deyişle, NodeTypeInclusionList ve ApplicationInclusionList bir kesişim belirtmek mümkün değildir. Örneğin, "bu uygulamayı yalnızca bu düğüm türünde yken hata" belirtmek mümkün değildir. Bir varlık NodeTypeInclusionList veya ApplicationInclusionList'e dahil edildikten sonra, bu varlık ChaosTargetFilter kullanılarak dışlanamaz. ApplicationInclusionList'te applicationX görünmese bile, bazı Kaos yineleme uygulamalarındaX, NodeTypeI'de yer alan bir düğümde olduğu için hatalı olabilir. Hem NodeTypeInclusionList hem de ApplicationInclusionList boş veya boşsa, Bir ArgumentException atılır.
    * **NodeTypeInclusionList**: Kaos hatalarına dahil edilen düğüm türlerinin listesi. Bu düğüm türlerinin düğümleri için tüm hata türleri (düğümyeniden başlatma, kod paketini yeniden başlatma, yinelemeyi kaldırma, yinelemeyi yeniden başlatma, birincil taşıma ve ikincil taşıma) etkinleştirilir. Bir düğüm tipi (NodeTypeX deyin) NodeTypeInclusionList'te görünmüyorsa, düğüm düzeyindeki hatalar (NodeRestart gibi) NodeTypeX düğümleri için hiçbir zaman etkinleştirilmez, ancak bir uygulama ApplicationInclusionList, NodeTypeX düğümünde yer alır. Bu sayıyı artırmak için en fazla 100 düğüm türü adı bu listeye eklenebilir, MaxNumberOfNodeTypesInChaosTargetFilter yapılandırması için bir config yükseltmesi gereklidir.
    * **ApplicationInclusionList**: Kaos hatalarına dahil edilen uygulama URI'lerinin listesi. Bu uygulamaların hizmetlerine ait tüm yinelemeler, Kaos tarafından yineleme hataları (yinelemeyi yeniden başlatma, çoğaltmayı kaldırma, birincil taşıma ve ikincil taşıma) için uygundur. Kaos, yalnızca kod paketi yalnızca bu uygulamaların yinelemelerini barındırıyorsa bir kod paketini yeniden başlatabilir. Bir uygulama bu listede görünmüyorsa, uygulama NodeTypeInclusionList'e dahil edilmiş bir düğüm düğümünde biterse, bazı Kaos yinelemesinde yine de hatalı olabilir. Ancak applicationX yerleşim kısıtlamaları yoluyla nodeTypeY'ye bağlıysa ve applicationX ApplicationInclusionList'te yoksa ve nodeTypeY NodeTypeInclusionList'te yoksa, applicationX hiçbir zaman hata yapılmaz. En fazla 1000 uygulama adları bu listeye dahil edilebilir, bu sayıyı artırmak için, bir config yükseltme MaxNumberOfApplicationsInChaosTargetFilter yapılandırma sı için gereklidir.

## <a name="how-to-run-chaos"></a>Kaos nasıl çalıştırılır?

```csharp
using System;
using System.Collections.Generic;
using System.Threading.Tasks;
using System.Fabric;

using System.Diagnostics;
using System.Fabric.Chaos.DataStructures;

class Program
{
    private class ChaosEventComparer : IEqualityComparer<ChaosEvent>
    {
        public bool Equals(ChaosEvent x, ChaosEvent y)
        {
            return x.TimeStampUtc.Equals(y.TimeStampUtc);
        }
        public int GetHashCode(ChaosEvent obj)
        {
            return obj.TimeStampUtc.GetHashCode();
        }
    }

    static void Main(string[] args)
    {
        var clusterConnectionString = "localhost:19000";
        using (var client = new FabricClient(clusterConnectionString))
        {
            var startTimeUtc = DateTime.UtcNow;

            // The maximum amount of time to wait for all cluster entities to become stable and healthy. 
            // Chaos executes in iterations and at the start of each iteration it validates the health of cluster
            // entities. 
            // During validation if a cluster entity is not stable and healthy within
            // MaxClusterStabilizationTimeoutInSeconds, Chaos generates a validation failed event.
            var maxClusterStabilizationTimeout = TimeSpan.FromSeconds(30.0);

            var timeToRun = TimeSpan.FromMinutes(60.0);

            // MaxConcurrentFaults is the maximum number of concurrent faults induced per iteration. 
            // Chaos executes in iterations and two consecutive iterations are separated by a validation phase.
            // The higher the concurrency, the more aggressive the injection of faults -- inducing more complex
            // series of states to uncover bugs.
            // The recommendation is to start with a value of 2 or 3 and to exercise caution while moving up.
            var maxConcurrentFaults = 3;

            // Describes a map, which is a collection of (string, string) type key-value pairs. The map can be
            // used to record information about the Chaos run. There cannot be more than 100 such pairs and
            // each string (key or value) can be at most 4095 characters long.
            // This map is set by the starter of the Chaos run to optionally store the context about the specific run.
            var startContext = new Dictionary<string, string>{{"ReasonForStart", "Testing"}};

            // Time-separation (in seconds) between two consecutive iterations of Chaos. The larger the value, the
            // lower the fault injection rate.
            var waitTimeBetweenIterations = TimeSpan.FromSeconds(10);

            // Wait time (in seconds) between consecutive faults within a single iteration.
            // The larger the value, the lower the overlapping between faults and the simpler the sequence of
            // state transitions that the cluster goes through. 
            // The recommendation is to start with a value between 1 and 5 and exercise caution while moving up.
            var waitTimeBetweenFaults = TimeSpan.Zero;

            // Passed-in cluster health policy is used to validate health of the cluster in between Chaos iterations. 
            var clusterHealthPolicy = new ClusterHealthPolicy
            {
                ConsiderWarningAsError = false,
                MaxPercentUnhealthyApplications = 100,
                MaxPercentUnhealthyNodes = 100
            };

            // All types of faults, restart node, restart code package, restart replica, move primary replica,
            // and move secondary replica will happen for nodes of type 'FrontEndType'
            var nodetypeInclusionList = new List<string> { "FrontEndType"};

            // In addition to the faults included by nodetypeInclusionList,
            // restart code package, restart replica, move primary replica, move secondary replica faults will
            // happen for 'fabric:/TestApp2' even if a replica or code package from 'fabric:/TestApp2' is residing
            // on a node which is not of type included in nodeypeInclusionList.
            var applicationInclusionList = new List<string> { "fabric:/TestApp2" };

            // List of cluster entities to target for Chaos faults.
            var chaosTargetFilter = new ChaosTargetFilter
            {
                NodeTypeInclusionList = nodetypeInclusionList,
                ApplicationInclusionList = applicationInclusionList
            };

            var parameters = new ChaosParameters(
                maxClusterStabilizationTimeout,
                maxConcurrentFaults,
                true, /* EnableMoveReplicaFault */
                timeToRun,
                startContext,
                waitTimeBetweenIterations,
                waitTimeBetweenFaults,
                clusterHealthPolicy) {ChaosTargetFilter = chaosTargetFilter};

            try
            {
                client.TestManager.StartChaosAsync(parameters).GetAwaiter().GetResult();
            }
            catch (FabricChaosAlreadyRunningException)
            {
                Console.WriteLine("An instance of Chaos is already running in the cluster.");
            }

            var filter = new ChaosReportFilter(startTimeUtc, DateTime.MaxValue);

            var eventSet = new HashSet<ChaosEvent>(new ChaosEventComparer());

            string continuationToken = null;

            while (true)
            {
                ChaosReport report;
                try
                {
                    report = string.IsNullOrEmpty(continuationToken)
                        ? client.TestManager.GetChaosReportAsync(filter).GetAwaiter().GetResult()
                        : client.TestManager.GetChaosReportAsync(continuationToken).GetAwaiter().GetResult();
                }
                catch (Exception e)
                {
                    if (e is FabricTransientException)
                    {
                        Console.WriteLine("A transient exception happened: '{0}'", e);
                    }
                    else if(e is TimeoutException)
                    {
                        Console.WriteLine("A timeout exception happened: '{0}'", e);
                    }
                    else
                    {
                        throw;
                    }

                    Task.Delay(TimeSpan.FromSeconds(1.0)).GetAwaiter().GetResult();
                    continue;
                }

                continuationToken = report.ContinuationToken;

                foreach (var chaosEvent in report.History)
                {
                    if (eventSet.Add(chaosEvent))
                    {
                        Console.WriteLine(chaosEvent);
                    }
                }

                // When Chaos stops, a StoppedEvent is created.
                // If a StoppedEvent is found, exit the loop.
                var lastEvent = report.History.LastOrDefault();

                if (lastEvent is StoppedEvent)
                {
                    break;
                }

                Task.Delay(TimeSpan.FromSeconds(1.0)).GetAwaiter().GetResult();
            }
        }
    }
}
```

```powershell
$clusterConnectionString = "localhost:19000"
$timeToRunMinute = 60

# The maximum amount of time to wait for all cluster entities to become stable and healthy.
# Chaos executes in iterations and at the start of each iteration it validates the health of cluster entities.
# During validation if a cluster entity is not stable and healthy within MaxClusterStabilizationTimeoutInSeconds,
# Chaos generates a validation failed event.
$maxClusterStabilizationTimeSecs = 30

# MaxConcurrentFaults is the maximum number of concurrent faults induced per iteration.
# Chaos executes in iterations and two consecutive iterations are separated by a validation phase.
# The higher the concurrency, the more aggressive the injection of faults -- inducing more complex series of
# states to uncover bugs.
# The recommendation is to start with a value of 2 or 3 and to exercise caution while moving up.
$maxConcurrentFaults = 3

# Time-separation (in seconds) between two consecutive iterations of Chaos. The larger the value, the lower the
# fault injection rate.
$waitTimeBetweenIterationsSec = 10

# Wait time (in seconds) between consecutive faults within a single iteration.
# The larger the value, the lower the overlapping between faults and the simpler the sequence of state
# transitions that the cluster goes through.
# The recommendation is to start with a value between 1 and 5 and exercise caution while moving up.
$waitTimeBetweenFaultsSec = 0

# Passed-in cluster health policy is used to validate health of the cluster in between Chaos iterations. 
$clusterHealthPolicy = new-object -TypeName System.Fabric.Health.ClusterHealthPolicy
$clusterHealthPolicy.MaxPercentUnhealthyNodes = 100
$clusterHealthPolicy.MaxPercentUnhealthyApplications = 100
$clusterHealthPolicy.ConsiderWarningAsError = $False

# Describes a map, which is a collection of (string, string) type key-value pairs. The map can be used to record
# information about the Chaos run.
# There cannot be more than 100 such pairs and each string (key or value) can be at most 4095 characters long.
# This map is set by the starter of the Chaos run to optionally store the context about the specific run.
$context = @{"ReasonForStart" = "Testing"}

#List of cluster entities to target for Chaos faults.
$chaosTargetFilter = new-object -TypeName System.Fabric.Chaos.DataStructures.ChaosTargetFilter
$chaosTargetFilter.NodeTypeInclusionList = new-object -TypeName "System.Collections.Generic.List[String]"

# All types of faults, restart node, restart code package, restart replica, move primary replica, and move
# secondary replica will happen for nodes of type 'FrontEndType'
$chaosTargetFilter.NodeTypeInclusionList.AddRange( [string[]]@("FrontEndType") )
$chaosTargetFilter.ApplicationInclusionList = new-object -TypeName "System.Collections.Generic.List[String]"

# In addition to the faults included by nodetypeInclusionList, 
# restart code package, restart replica, move primary replica, move secondary replica faults will happen for
# 'fabric:/TestApp2' even if a replica or code package from 'fabric:/TestApp2' is residing on a node which is
# not of type included in nodeypeInclusionList.
$chaosTargetFilter.ApplicationInclusionList.Add("fabric:/TestApp2")

Connect-ServiceFabricCluster $clusterConnectionString

$events = @{}
$now = [System.DateTime]::UtcNow

Start-ServiceFabricChaos -TimeToRunMinute $timeToRunMinute -MaxConcurrentFaults $maxConcurrentFaults -MaxClusterStabilizationTimeoutSec $maxClusterStabilizationTimeSecs -EnableMoveReplicaFaults -WaitTimeBetweenIterationsSec $waitTimeBetweenIterationsSec -WaitTimeBetweenFaultsSec $waitTimeBetweenFaultsSec -ClusterHealthPolicy $clusterHealthPolicy -ChaosTargetFilter $chaosTargetFilter

while($true)
{
    $stopped = $false
    $report = Get-ServiceFabricChaosReport -StartTimeUtc $now -EndTimeUtc ([System.DateTime]::MaxValue)

    foreach ($e in $report.History) {

        if(-Not ($events.Contains($e.TimeStampUtc.Ticks)))
        {
            $events.Add($e.TimeStampUtc.Ticks, $e)
            if($e -is [System.Fabric.Chaos.DataStructures.ValidationFailedEvent])
            {
                Write-Host -BackgroundColor White -ForegroundColor Red $e
            }
            else
            {
                Write-Host $e
                # When Chaos stops, a StoppedEvent is created.
                # If a StoppedEvent is found, exit the loop.
                if($e -is [System.Fabric.Chaos.DataStructures.StoppedEvent])
                {
                    return
                }
            }
        }
    }

    Start-Sleep -Seconds 1
}
```
