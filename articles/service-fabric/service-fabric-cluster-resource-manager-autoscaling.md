---
title: Azure Servis Kumaş Otomatik Ölçekleme Hizmetleri ve Konteynerleri
description: Azure Hizmet Kumaşı, hizmetler ve kapsayıcılar için otomatik ölçekleme ilkeleri ayarlamanıza olanak tanır.
author: radicmilos
ms.topic: conceptual
ms.date: 04/17/2018
ms.author: miradic
ms.openlocfilehash: 3660ece7add8f279292340aae9ab445b682fe045
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75452078"
---
# <a name="introduction-to-auto-scaling"></a>Otomatik Ölçekleme Giriş
Otomatik ölçeklendirme, hizmetlerinizi hizmetlerin bildirdiği yüke veya kaynak kullanımlarına göre dinamik olarak ölçeklendirmek için Hizmet Dokusu'nun ek bir özelliğidir. Otomatik ölçekleme büyük esneklik sağlar ve isteğe bağlı olarak ek örneklerin veya hizmet bölümlerinin sağlanmasını sağlar. Otomatik ölçekleme işleminin tamamı otomatik ve saydamdır ve bir hizmette ilkelerinizi ayarladıktan sonra hizmet düzeyinde el ile ölçekleme işlemlerine gerek yoktur. Otomatik ölçekleme, hizmet oluşturma zamanında veya hizmeti güncelleştirerek herhangi bir zamanda açılabilir.

Otomatik ölçeklemenin yararlı olduğu yaygın bir senaryo, belirli bir hizmetteki yükün zaman içinde değiştiğinde dir. Örneğin, ağ geçidi gibi bir hizmet, gelen istekleri işlemek için gereken kaynak miktarına göre ölçeklenebilir. Ölçekleme kurallarının nasıl görünebileceğine bir bakalım:
* Ağ geçidimin tüm örnekleri ortalama olarak ikiden fazla çekirdek kullanıyorsa, ağ geçidi hizmetini bir örnek daha ekleyerek ölçeklendirin. Bunu her saat yapın, ancak toplamda yediden fazla örneği nizyoktur.
* Ağ geçidimin tüm örnekleri ortalama 0,5 çekirdekten daha az kullanıyorsa, bir örneği kaldırarak hizmeti ölçeklendirin. Bunu her saat yapın, ancak toplamda üçörnekten az olmaz.

Otomatik ölçekleme hem konteynerler hem de düzenli Servis Kumaşı hizmetleri için desteklenir. Otomatik ölçekleme kullanmak için, Service Fabric çalışma süresinin 6.2 veya üzerinde sürümde çalışıyor olmanız gerekir. 

Bu makalenin geri kalanında ölçeklendirme ilkeleri, otomatik ölçeklendirmeyi etkinleştirmenin veya devre dışı etmenin yolları açıklanır ve bu özelliğin nasıl kullanılacağına ilişkin örnekler verir.

## <a name="describing-auto-scaling"></a>Otomatik ölçekleme açıklama
Bir Hizmet Kumaşı kümesindeki her hizmet için otomatik ölçekleme ilkeleri tanımlanabilir. Her ölçekleme ilkesi iki bölümden oluşur:
* **Ölçekleme** tetikleyicisi, hizmetin ölçeklemesinin ne zaman gerçekleştirileceğini açıklar. Tetikleyicide tanımlanan koşullar, bir hizmetin ölçeklendirilip ölçeklenmemesi gerektiğini belirlemek için düzenli aralıklarla denetlenir.

* **Ölçekleme mekanizması,** tetiklendiğinde ölçeklemenin nasıl gerçekleştirileceğini açıklar. Mekanizma yalnızca tetikleyiciden gelen koşullar karşılandığında uygulanır.

Şu anda desteklenen tüm tetikleyiciler [mantıksal yük ölçümleri](service-fabric-cluster-resource-manager-metrics.md)veya CPU veya bellek kullanımı gibi fiziksel ölçümlerle çalışır. Her iki şekilde de, Service Fabric ölçüm için bildirilen yükü izler ve ölçeklendirme gerekip gerekip gerekmeden önce belirlemek için tetikleyiciyi periyodik olarak değerlendirir.

Şu anda otomatik ölçekleme için desteklenen iki mekanizma vardır. İlki, devletsiz hizmetler veya örnekleri ekleyerek veya kaldırarak otomatik ölçeklemenin gerçekleştirildiği [kapsayıcılar](service-fabric-concepts-replica-lifecycle.md)içindir. Hem durum lu hem de durumsuz hizmetler için, otomatik ölçekleme, hizmetin adlandırılmış [bölümleri](service-fabric-concepts-partitioning.md) ekleyerek veya kaldırarak da gerçekleştirilebilir.

> [!NOTE]
> Şu anda hizmet başına yalnızca bir ölçekleme ilkesi ve ölçekleme ilkesi başına yalnızca bir ölçekleme tetikleyicisi desteği vardır.

## <a name="average-partition-load-trigger-with-instance-based-scaling"></a>Örnek tabanlı ölçekleme ile ortalama bölüm yükü tetikleyicisi
İlk tetikleyici türü, devletsiz hizmet bölmesindeki örneklerin yükünü temel adatır. Bir bölümün her örneği için yükü elde etmek için önce metrik yükler düzeltilir ve daha sonra bu değerler bölümün tüm örneklerinde ortalamaya bölünür. Hizmetin ne zaman ölçeklendirilecek olacağını belirleyen üç etken vardır:

* _Daha düşük yük eşiği,_ hizmetin ne zaman **ölçeklendirileceğini**belirleyen bir değerdir. Bölümlerin tüm örneklerinin ortalama yükü bu değerden daha düşükse, hizmet ölçeklendirilir.
* _Üst yük eşiği,_ hizmetin ne zaman **ölçeklendirileceğini**belirleyen bir değerdir. Bölümün tüm örneklerinin ortalama yükü bu değerden daha yüksekse, hizmet ölçeklendirilir.
* _Ölçekleme aralığı_ tetikleyicinin ne sıklıkta denetleneceğini belirler. Tetikleyici kontrol edildikten sonra, ölçekleme gerekiyorsa mekanizma uygulanır. Ölçekleme gerekli değilse, hiçbir işlem yapılmaz. Her iki durumda da, ölçekleme aralığı yeniden sona ermeden önce tetikleyici yeniden denetlenmez.

Bu tetikleyici yalnızca devletsiz hizmetlerde (devletsiz kapsayıcılar veya Service Fabric hizmetleri) kullanılabilir. Bir hizmetin birden çok bölümü varsa, tetikleyici her bölüm için ayrı ayrı değerlendirilir ve her bölüm için belirtilen mekanizma bağımsız olarak uygulanır. Bu nedenle, bu durumda, hizmetin bazı bölümleri ölçeklendirilir, bazı ölçeklendirilir ve bazı yüklerine göre, aynı anda hiç ölçeklendirilmez mümkündür.

Bu tetikleyici ile kullanılabilecek tek mekanizma PartitionInstanceCountScaleMechanism olduğunu. Bu mekanizmanın nasıl uygulandığını belirleyen üç faktör vardır:
* _Ölçek Artış,_ mekanizma tetiklendiğinde kaç örnek ekleneceğini veya kaldırılacağını belirler.
* _Maksimum Örnek Sayısı_ ölçekleme için üst sınırı tanımlar. Bölümün örnek sayısı bu sınıra ulaşırsa, yükten bağımsız olarak hizmet ölçeklendirilmeyecektir. -1 değerini belirterek bu sınırı atlayabilir ve bu durumda hizmet mümkün olduğunca ölçeklendirilir (sınır kümede bulunan düğüm sayısıdır).
* _Minimum Örnek Sayısı_ ölçekleme için alt sınırı tanımlar. Bölümün örnek sayısı bu sınıra ulaşırsa, hizmet yükten bağımsız olarak ölçeklendirilmeyecektir.

## <a name="setting-auto-scaling-policy"></a>Otomatik ölçekleme ilkesini ayarlama

### <a name="using-application-manifest"></a>Uygulama bildirimini kullanma
``` xml
<LoadMetrics>
<LoadMetric Name="MetricB" Weight="High"/>
</LoadMetrics>
<ServiceScalingPolicies>
<ScalingPolicy>
    <AveragePartitionLoadScalingTrigger MetricName="MetricB" LowerLoadThreshold="1" UpperLoadThreshold="2" ScaleIntervalInSeconds="100"/>
    <InstanceCountScalingMechanism MinInstanceCount="3" MaxInstanceCount="4" ScaleIncrement="1"/>
</ScalingPolicy>
</ServiceScalingPolicies>
```
### <a name="using-c-apis"></a>C# API'lerini kullanma
```csharp
FabricClient fabricClient = new FabricClient();
StatelessServiceDescription serviceDescription = new StatelessServiceDescription();
//set up the rest of the ServiceDescription
AveragePartitionLoadScalingTrigger trigger = new AveragePartitionLoadScalingTrigger();
PartitionInstanceCountScaleMechanism mechanism = new PartitionInstanceCountScaleMechanism();
mechanism.MaxInstanceCount = 3;
mechanism.MinInstanceCount = 1;
mechanism.ScaleIncrement = 1;
trigger.MetricName = "servicefabric:/_CpuCores";
trigger.ScaleInterval = TimeSpan.FromMinutes(20);
trigger.LowerLoadThreshold = 1.0;
trigger.UpperLoadThreshold = 2.0;
ScalingPolicyDescription policy = new ScalingPolicyDescription(mechanism, trigger);
serviceDescription.ScalingPolicies.Add(policy);
//as we are using scaling on a resource this must be exclusive service
//also resource monitor service needs to be enabled
serviceDescription.ServicePackageActivationMode = ServicePackageActivationMode.ExclusiveProcess
await fabricClient.ServiceManager.CreateServiceAsync(serviceDescription);
```
### <a name="using-powershell"></a>Powershell'i Kullanma
```posh
$mechanism = New-Object -TypeName System.Fabric.Description.PartitionInstanceCountScaleMechanism
$mechanism.MinInstanceCount = 1
$mechanism.MaxInstanceCount = 6
$mechanism.ScaleIncrement = 2
$trigger = New-Object -TypeName System.Fabric.Description.AveragePartitionLoadScalingTrigger
$trigger.MetricName = "servicefabric:/_CpuCores"
$trigger.LowerLoadThreshold = 0.3
$trigger.UpperLoadThreshold = 0.8
$trigger.ScaleInterval = New-TimeSpan -Minutes 10
$scalingpolicy = New-Object -TypeName System.Fabric.Description.ScalingPolicyDescription
$scalingpolicy.ScalingMechanism = $mechanism
$scalingpolicy.ScalingTrigger = $trigger
$scalingpolicies = New-Object 'System.Collections.Generic.List[System.Fabric.Description.ScalingPolicyDescription]'
$scalingpolicies.Add($scalingpolicy)
#as we are using scaling on a resource this must be exclusive service
#also resource monitor service needs to be enabled
Update-ServiceFabricService -Stateless -ServiceName "fabric:/AppName/ServiceName" -ScalingPolicies $scalingpolicies
```

## <a name="average-service-load-trigger-with-partition-based-scaling"></a>Bölüm tabanlı ölçekleme ile ortalama hizmet yükü tetikleyicisi
İkinci tetikleyici, bir hizmetin tüm bölümlerinin yükünü temel adatır. Metrik yükler, bir bölümün her yinelemesi veya örneği için yükü elde etmek için önce düzeltilir. Devlet hizmetleri için, bölümün yükü birincil yinelemenin yükü olarak kabul edilirken, devletsiz hizmetler için bölümün yükü bölümün tüm örneklerinin ortalama yüküdür. Bu değerler hizmetin tüm bölümleri arasında ortalamaya yerleştirilir ve bu değer otomatik ölçekleme tetiklemek için kullanılır. Önceki mekanizmada olduğu gibi, hizmetin ne zaman ölçeklendirilecek olacağını belirleyen üç etken vardır:

* _Daha düşük yük eşiği,_ hizmetin ne zaman **ölçeklendirileceğini**belirleyen bir değerdir. Hizmetin tüm bölümlerinin ortalama yükü bu değerden daha düşükse, hizmet ölçeklendirilir.
* _Üst yük eşiği,_ hizmetin ne zaman **ölçeklendirileceğini**belirleyen bir değerdir. Hizmetin tüm bölümlerinin ortalama yükü bu değerden yüksekse, hizmet ölçeklendirilir.
* _Ölçekleme aralığı_ tetikleyicinin ne sıklıkta denetleneceğini belirler. Tetikleyici kontrol edildikten sonra, ölçekleme gerekiyorsa mekanizma uygulanır. Ölçekleme gerekli değilse, hiçbir işlem yapılmaz. Her iki durumda da, ölçekleme aralığı yeniden sona ermeden önce tetikleyici yeniden denetlenmez.

Bu tetikleyici hem durumlu hem de durumsuz hizmetlerde kullanılabilir. Bu tetikleyici ile kullanılabilecek tek mekanizma AddRemoveIncrementalNamedPartitionScalingMechanism olduğunu. Hizmet ölçeklendirildiğinde yeni bir bölüm eklenir ve hizmet varolan bölümlerden birinde ölçeklendirildiğinde kaldırılır. Hizmet oluşturulduğunda veya güncelleştirildiğinde denetlenecek kısıtlamalar vardır ve bu koşullar yerine getirilmezse hizmet oluşturma/güncelleştirme başarısız olur:
* Adlandırılmış bölüm düzeni hizmet için kullanılmalıdır.
* Bölüm adları "0", "1" gibi ardışık toplam numaraları olmalıdır...
* İlk bölüm adı "0" olmalıdır.

Örneğin, bir hizmet başlangıçta üç bölümden oluşan oluşturulursa, bölüm adları için tek geçerli olasılık "0", "1" ve "2"dir.

Gerçekleştirilen gerçek otomatik ölçeklendirme işlemi de bu adlandırma düzenine saygı gösterecektir:
* Hizmetin geçerli bölümlerine "0", "1" ve "2" adlandırılmışsa, ölçekleme için eklenecek bölüm "3" olarak adlandırılır.
* Hizmetin geçerli bölümleri "0", "1" ve "2" olarak adlandırılırsa, ölçekleme için kaldırılacak bölüm "2" adı ile bölümdür.

Örnekleri ekleyerek veya kaldırarak ölçekleme kullanan mekanizmayla aynı şekilde, bu mekanizmanın nasıl uygulandığını belirleyen üç parametre vardır:
* _Ölçek Artış,_ mekanizma tetiklendiğinde kaç bölüm ekleneceğini veya kaldırılacağını belirler.
* _Maksimum Bölüm Sayısı_ ölçekleme için üst sınırı tanımlar. Hizmetin bölüm sayısı bu sınıra ulaşırsa, yükten bağımsız olarak hizmet ölçeklendirilmeyecektir. -1 değerini belirterek bu sınırı atlayabilir ve bu durumda hizmet mümkün olduğunca ölçeklendirilir (sınır kümenin gerçek kapasitesidir).
* _Minimum Örnek Sayısı_ ölçekleme için alt sınırı tanımlar. Hizmetin bölüm sayısı bu sınıra ulaşırsa, hizmet yükten bağımsız olarak ölçeklendirilmeyecektir.

> [!WARNING] 
> AddRemoveIncrementalNamedPartitionScalingMechanism durumlu hizmetlerle kullanıldığında, Hizmet **Kumaşı bildirim veya uyarı olmadan**bölümleri ekler veya kaldırır. Ölçekleme mekanizması tetiklendiğinde verilerin yeniden bölümlemesi yapılmaz. Çalışma ölçeklendirme durumunda, yeni bölümler boş olacak ve küçültme işlemi durumunda, **bölüm içerdiği tüm verilerle birlikte silinir.**

## <a name="setting-auto-scaling-policy"></a>Otomatik ölçekleme ilkesini ayarlama

### <a name="using-application-manifest"></a>Uygulama bildirimini kullanma
``` xml
<ServiceScalingPolicies>
    <ScalingPolicy>
        <AverageServiceLoadScalingTrigger MetricName="servicefabric:/_MemoryInMB" LowerLoadThreshold="300" UpperLoadThreshold="500" ScaleIntervalInSeconds="600"/>
        <AddRemoveIncrementalNamedPartitionScalingMechanism MinPartitionCount="1" MaxPartitionCount="3" ScaleIncrement="1"/>
    </ScalingPolicy>
</ServiceScalingPolicies>
```
### <a name="using-c-apis"></a>C# API'lerini kullanma
```csharp
FabricClient fabricClient = new FabricClient();
StatefulServiceUpdateDescription serviceUpdate = new StatefulServiceUpdateDescription();
AveragePartitionLoadScalingTrigger trigger = new AverageServiceLoadScalingTrigger();
PartitionInstanceCountScaleMechanism mechanism = new AddRemoveIncrementalNamedPartitionScalingMechanism();
mechanism.MaxPartitionCount = 4;
mechanism.MinPartitionCount = 1;
mechanism.ScaleIncrement = 1;
//expecting that the service already has metric NumberOfConnections
trigger.MetricName = "NumberOfConnections";
trigger.ScaleInterval = TimeSpan.FromMinutes(15);
trigger.LowerLoadThreshold = 10000;
trigger.UpperLoadThreshold = 20000;
ScalingPolicyDescription policy = new ScalingPolicyDescription(mechanism, trigger);
serviceUpdate.ScalingPolicies = new List<ScalingPolicyDescription>;
serviceUpdate.ScalingPolicies.Add(policy);
await fabricClient.ServiceManager.UpdateServiceAsync(new Uri("fabric:/AppName/ServiceName"), serviceUpdate);
```
### <a name="using-powershell"></a>Powershell'i Kullanma
```posh
$mechanism = New-Object -TypeName System.Fabric.Description.AddRemoveIncrementalNamedPartitionScalingMechanism
$mechanism.MinPartitionCount = 1
$mechanism.MaxPartitionCount = 3
$mechanism.ScaleIncrement = 2
$trigger = New-Object -TypeName System.Fabric.Description.AverageServiceLoadScalingTrigger
$trigger.MetricName = "servicefabric:/_MemoryInMB"
$trigger.LowerLoadThreshold = 5000
$trigger.UpperLoadThreshold = 10000
$trigger.ScaleInterval = New-TimeSpan -Minutes 25
$scalingpolicy = New-Object -TypeName System.Fabric.Description.ScalingPolicyDescription
$scalingpolicy.ScalingMechanism = $mechanism
$scalingpolicy.ScalingTrigger = $trigger
$scalingpolicies = New-Object 'System.Collections.Generic.List[System.Fabric.Description.ScalingPolicyDescription]'
$scalingpolicies.Add($scalingpolicy)
#as we are using scaling on a resource this must be exclusive service
#also resource monitor service needs to be enabled
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -TargetReplicaSetSize 3 -MinReplicaSetSize 2 -HasPersistedState true -PartitionNames @("0","1") -ServicePackageActivationMode ExclusiveProcess -ScalingPolicies $scalingpolicies
```

## <a name="auto-scaling-based-on-resources"></a>Kaynaklara dayalı otomatik ölçekleme

Kaynak izleme hizmetinin gerçek kaynaklara göre ölçeklemesini sağlamak için

``` json
"fabricSettings": [
...      
],
"addonFeatures": [
    "ResourceMonitorService"
],
```
Gerçek fiziksel kaynakları temsil eden iki ölçüm vardır. Bunlardan biri servicefabric:/_CpuCores gerçek işlemci kullanımını temsil eder (yani 0,5 yarım çekirdeği temsil eder) ve diğeri de MB'lerde bellek kullanımını temsil eden servicefabric:/_MemoryInMB' dir.
ResourceMonitorService, kullanıcı hizmetlerinin işlemci ve bellek kullanımını izlemekle yükümlüdür. Bu hizmet, olası kısa ömürlü ani artışları hesaba katmak için ağırlıklı hareketli ortalama uygular. Kaynak izleme, Windows'daki hem kapsayıcı hem de kapsayıcı olmayan uygulamalar ve Linux'taki kapsayıcı uygulamalar için desteklenir. Kaynaklarda otomatik ölçekleme yalnızca [özel işlem modelinde](service-fabric-hosting-model.md#exclusive-process-model)etkinleştirilen hizmetler için etkinleştirilir.

## <a name="next-steps"></a>Sonraki adımlar
[Uygulama ölçeklenebilirliği](service-fabric-concepts-scalability.md)hakkında daha fazla bilgi edinin.
