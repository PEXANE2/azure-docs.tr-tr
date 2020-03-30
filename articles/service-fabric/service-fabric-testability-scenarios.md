---
title: Azure Hizmet Kumaşı için kaos ve başarısız testler oluşturun
description: Hataları tetiklemek ve hizmetlerinizin güvenilirliğini doğrulamak için Service Fabric kaos testi ve başarısız test senaryolarını kullanma.
author: motanv
ms.topic: conceptual
ms.date: 10/1/2019
ms.author: motanv
ms.openlocfilehash: 206b02024ad052a12e87cfdf1773815027e8aec4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75465542"
---
# <a name="testability-scenarios"></a>Test edilebilirlik senaryoları
Bulut altyapıları gibi büyük dağıtılmış sistemler doğal olarak güvenilmezdir. Azure Hizmet Kumaşı, geliştiricilere güvenilmez altyapıların üzerinde çalışacak hizmet yazma olanağı sağlar. Yüksek kaliteli hizmetler yazmak için, geliştiricilerin hizmetlerinin istikrarını test etmek için bu kadar güvenilmez bir altyapıyı teşvik edebilmeleri gerekir.

Hata Çözümleme Hizmeti, geliştiricilere hataların varlığında hizmetleri test etmek için hata eylemleri neden olma olanağı sağlar. Ancak, hedeflenen simüle hatalar sadece şimdiye kadar alırsınız. Testi daha da ileri götürmek için Service Fabric:bir kaos testi ve bir başarısızlık testinde test senaryolarını kullanabilirsiniz. Bu senaryolar, küme boyunca uzun süreler boyunca hem zarif hem de zarif olmayan, sürekli ara sıra edilmiş hataları simüle eder. Bir test, hata oranı ve türüyle yapılandırıldıktan sonra, kümede ve hizmetinizde hata oluşturmak için C# API'leri veya PowerShell üzerinden başlatılabilir.

> [!WARNING]
> ChaosTestScenario daha esnek, hizmet tabanlı Chaos ile değiştiriliyor. Daha fazla bilgi için lütfen yeni makale [Controlled Chaos](service-fabric-controlled-chaos.md) bakın.
> 
> 

## <a name="chaos-test"></a>Kaos testi
Kaos senaryosu tüm Service Fabric kümesinde hatalar oluşturur. Senaryo genellikle aylar veya yıllar içinde birkaç saat e kadar görülen hataları sıkıştırır. Yüksek hata oranı ile ara sıra hataların birleşimi aksi takdirde cevapsız köşe durumlarda bulur. Bu, hizmetin kod kalitesinde önemli bir iyileşmeye yol açar.

### <a name="faults-simulated-in-the-chaos-test"></a>Kaos testinde simüle edilen hatalar
* Düğümü yeniden başlat
* Dağıtılan kod paketini yeniden başlatma
* Yinelemeyi kaldırma
* Yinelemeyi yeniden başlatma
* Birincil yinelemeyi taşıma (isteğe bağlı)
* İkincil yinelemeyi taşıma (isteğe bağlı)

Kaos testi, belirtilen süre boyunca hataların ve küme doğrulamalarının birden çok yinelemesini çalıştırAr. Kümenin stabilize edilmesi ve doğrulamanın başarılı olması için harcanan süre de yapılandırılabilir. Küme doğrulamada tek bir hata yaptığınızda senaryo başarısız olur.

Örneğin, en fazla üç eşzamanlı hatayla bir saat çalışacak bir test kümesi düşünün. Test üç hata neden olur ve sonra küme durumunu doğrular. Test, küme sağlıksız hale gelene veya bir saat geçene kadar önceki adımda yinelenir. Küme herhangi bir yinelemede sağlıksız hale gelirse, yani yapılandırılmış bir süre içinde sabitlenmez, test bir istisna dışında başarısız olur. Bu istisna, bir şeylerin yanlış gittiğini ve daha fazla araştırma gerektiğini gösterir.

Mevcut haliyle, kaos testindeki fay üretim motoru sadece güvenli arızalara neden olur. Bu, dış hatalar yokluğunda, bir çoğunluk veya veri kaybı asla meydana asla anlamına gelir.

### <a name="important-configuration-options"></a>Önemli yapılandırma seçenekleri
* **TimeToRun**: Testin başarı ile bitirmeden önce çalışacağı toplam süre. Doğrulama hatası yerine test daha erken bitirilebilir.
* **MaxClusterStabilizationTimeout**: Testten geçemeden kümenin sağlıklı olmasını beklemek için maksimum süre. Gerçekleştirilen denetimler küme durumu Tamam olup olmadığı, hizmet durumu Tamam olup olmadığı, hedef yineleme kümesi boyutu hizmet bölümü için elde edilir ve hiçbir InBuild yinelemeleri vardır.
* **MaxConcurrentFaults**: Her yinelemede indüklenen maksimum eşzamanlı hata sayısı. Sayı ne kadar yüksekse, test o kadar agresif olur ve bu da daha karmaşık arızalar ve geçiş kombinasyonları ile sonuçlanır. Test, dış hata lar olmadığında, bu yapılandırmanın ne kadar yüksek olduğuna bakılmaksızın bir çoğunluk veya veri kaybı olmayacağını garanti eder.
* **EnableMoveReplicaFaults**: Birincil veya ikincil yinelemelerin taşınmasına neden olan hataları etkinleştirer veya devre dışı eder. Bu hatalar varsayılan olarak devre dışı bırakılır.
* **WaitTimeBetweenIterations**: Yinelemeler arasında bekleme süresi, yani bir hata turundan ve ilgili doğrulamadan sonra.

### <a name="how-to-run-the-chaos-test"></a>Kaos testi nasıl çalıştırılır?
C# örneği

```csharp
using System;
using System.Fabric;
using System.Fabric.Testability.Scenario;
using System.Threading;
using System.Threading.Tasks;

class Test
{
    public static int Main(string[] args)
    {
        string clusterConnection = "localhost:19000";

        Console.WriteLine("Starting Chaos Test Scenario...");
        try
        {
            RunChaosTestScenarioAsync(clusterConnection).Wait();
        }
        catch (AggregateException ae)
        {
            Console.WriteLine("Chaos Test Scenario did not complete: ");
            foreach (Exception ex in ae.InnerExceptions)
            {
                if (ex is FabricException)
                {
                    Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
                }
            }
            return -1;
        }

        Console.WriteLine("Chaos Test Scenario completed.");
        return 0;
    }

    static async Task RunChaosTestScenarioAsync(string clusterConnection)
    {
        TimeSpan maxClusterStabilizationTimeout = TimeSpan.FromSeconds(180);
        uint maxConcurrentFaults = 3;
        bool enableMoveReplicaFaults = true;

        // Create FabricClient with connection and security information here.
        FabricClient fabricClient = new FabricClient(clusterConnection);

        // The chaos test scenario should run at least 60 minutes or until it fails.
        TimeSpan timeToRun = TimeSpan.FromMinutes(60);
        ChaosTestScenarioParameters scenarioParameters = new ChaosTestScenarioParameters(
          maxClusterStabilizationTimeout,
          maxConcurrentFaults,
          enableMoveReplicaFaults,
          timeToRun);

        // Other related parameters:
        // Pause between two iterations for a random duration bound by this value.
        // scenarioParameters.WaitTimeBetweenIterations = TimeSpan.FromSeconds(30);
        // Pause between concurrent actions for a random duration bound by this value.
        // scenarioParameters.WaitTimeBetweenFaults = TimeSpan.FromSeconds(10);

        // Create the scenario class and execute it asynchronously.
        ChaosTestScenario chaosScenario = new ChaosTestScenario(fabricClient, scenarioParameters);

        try
        {
            await chaosScenario.ExecuteAsync(CancellationToken.None);
        }
        catch (AggregateException ae)
        {
            throw ae.InnerException;
        }
    }
}
```

PowerShell

Service Fabric Powershell modülü bir kaos senaryosu başlatmak için iki yol içerir. `Invoke-ServiceFabricChaosTestScenario`istemci tabanlıdır ve istemci makine testin ortasında kapatılırsa, başka hata getirilmeyecektir. Alternatif olarak, makinenin kapatılması durumunda testin çalışmasını sağlamak için bir dizi komut vardır. `Start-ServiceFabricChaos`FaultAnalysisService adı verilen, ZamanToRun bitene kadar hataların devam etmesini sağlayan, devlete ait ve güvenilir bir sistem hizmeti kullanır. `Stop-ServiceFabricChaos`senaryoyu el ile durdurmak için `Get-ServiceFabricChaosReport` kullanılabilir ve bir rapor elde edecektir. Daha fazla bilgi için [Azure Service Fabric Powershell başvurusu](https://docs.microsoft.com/powershell/module/servicefabric/?view=azureservicefabricps) na bakın ve Service Fabric [kümelerinde kontrollü kaos uyguluyor.](service-fabric-controlled-chaos.md)

```powershell
$connection = "localhost:19000"
$timeToRun = 60
$maxStabilizationTimeSecs = 180
$concurrentFaults = 3
$waitTimeBetweenIterationsSec = 60

Connect-ServiceFabricCluster $connection

Invoke-ServiceFabricChaosTestScenario -TimeToRunMinute $timeToRun -MaxClusterStabilizationTimeoutSec $maxStabilizationTimeSecs -MaxConcurrentFaults $concurrentFaults -EnableMoveReplicaFaults -WaitTimeBetweenIterationsSec $waitTimeBetweenIterationsSec
```


## <a name="failover-test"></a>Başarısız test
Başarısız test senaryosu, belirli bir hizmet bölümü hedefleyen kaos testi senaryosunun bir sürümüdür. Diğer hizmetleri etkilenmeden bırakırken, belirli bir hizmet bölümü üzerinde failover etkisini sınar. Hedef bölüm bilgileri ve diğer parametrelerle yapılandırıldıktan sonra, bir hizmet bölümü için hata oluşturmak için C# API'leri veya PowerShell'i kullanan istemci tarafı aracı olarak çalışır. Senaryo, iş yükünü sağlamak için yan tarafta çalışırken, simüle edilmiş hatalar ve hizmet doğrulama dizisi ile yineler. Hizmet doğrulamasındaki bir hata, daha fazla araştırma yapılması gereken bir sorunu gösterir.

### <a name="faults-simulated-in-the-failover-test"></a>Başarısız testte simüle edilen hatalar
* Bölümün barındırıldığı dağıtılmış kod paketini yeniden başlatma
* Birincil/ikincil yinelemeyi veya durum bilgisisiz örneği kaldırma
* Birincil ikincil yinelemeyi yeniden başlatın (kalıcı bir hizmet varsa)
* Birincil yinelemeyi taşıma
* İkincil yinelemeyi taşıma
* Bölümü yeniden başlatın

Failover testi seçilen bir hatayı neden olur ve daha sonra kararlılığını sağlamak için hizmet üzerinde doğrulama çalışır. Başarısızlık testi, kaos testindeki olası birden fazla hatanın aksine, aynı anda yalnızca bir hataya neden olur. Hizmet bölümü, her hatadan sonra yapılandırılan zaman arızası içinde sabitlenmezse, test başarısız olur. Test sadece güvenli hatalara neden olur. Bu, dış hatalar olmadığı nda, bir çoğunluk veya veri kaybı nın meydana gelmeyeceğini zedler.

### <a name="important-configuration-options"></a>Önemli yapılandırma seçenekleri
* **PartitionSelector**: Hedef alınması gereken bölümü belirten seçici nesne.
* **TimeToRun**: Testin bitmeden önce çalışacağı toplam süre.
* **MaxServiceStabilizationTimeout**: Testten geçemeden kümenin sağlıklı olmasını beklemek için maksimum süre. Gerçekleştirilen denetimler hizmet durumu Tamam olup olmadığı, hedef çoğaltma kümesi boyutu tüm bölümler için elde edilir ve hiçbir InBuild yinelemeleri vardır.
* **WaitTimeBetweenFaults**: Her hata ve doğrulama döngüsü arasında bekleme süresi.

### <a name="how-to-run-the-failover-test"></a>Başarısızlık testi nasıl çalıştırılır?
**C #**

```csharp
using System;
using System.Fabric;
using System.Fabric.Testability.Scenario;
using System.Threading;
using System.Threading.Tasks;

class Test
{
    public static int Main(string[] args)
    {
        string clusterConnection = "localhost:19000";
        Uri serviceName = new Uri("fabric:/samples/PersistentToDoListApp/PersistentToDoListService");

        Console.WriteLine("Starting Chaos Test Scenario...");
        try
        {
            RunFailoverTestScenarioAsync(clusterConnection, serviceName).Wait();
        }
        catch (AggregateException ae)
        {
            Console.WriteLine("Chaos Test Scenario did not complete: ");
            foreach (Exception ex in ae.InnerExceptions)
            {
                if (ex is FabricException)
                {
                    Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
                }
            }
            return -1;
        }

        Console.WriteLine("Chaos Test Scenario completed.");
        return 0;
    }

    static async Task RunFailoverTestScenarioAsync(string clusterConnection, Uri serviceName)
    {
        TimeSpan maxServiceStabilizationTimeout = TimeSpan.FromSeconds(180);
        PartitionSelector randomPartitionSelector = PartitionSelector.RandomOf(serviceName);

        // Create FabricClient with connection and security information here.
        FabricClient fabricClient = new FabricClient(clusterConnection);

        // The chaos test scenario should run at least 60 minutes or until it fails.
        TimeSpan timeToRun = TimeSpan.FromMinutes(60);
        FailoverTestScenarioParameters scenarioParameters = new FailoverTestScenarioParameters(
          randomPartitionSelector,
          timeToRun,
          maxServiceStabilizationTimeout);

        // Other related parameters:
        // Pause between two iterations for a random duration bound by this value.
        // scenarioParameters.WaitTimeBetweenIterations = TimeSpan.FromSeconds(30);
        // Pause between concurrent actions for a random duration bound by this value.
        // scenarioParameters.WaitTimeBetweenFaults = TimeSpan.FromSeconds(10);

        // Create the scenario class and execute it asynchronously.
        FailoverTestScenario failoverScenario = new FailoverTestScenario(fabricClient, scenarioParameters);

        try
        {
            await failoverScenario.ExecuteAsync(CancellationToken.None);
        }
        catch (AggregateException ae)
        {
            throw ae.InnerException;
        }
    }
}
```


**Powershell**

```powershell
$connection = "localhost:19000"
$timeToRun = 60
$maxStabilizationTimeSecs = 180
$waitTimeBetweenFaultsSec = 10
$serviceName = "fabric:/SampleApp/SampleService"

Connect-ServiceFabricCluster $connection

Invoke-ServiceFabricFailoverTestScenario -TimeToRunMinute $timeToRun -MaxServiceStabilizationTimeoutSec $maxStabilizationTimeSecs -WaitTimeBetweenFaultsSec $waitTimeBetweenFaultsSec -ServiceName $serviceName -PartitionKindSingleton
```
