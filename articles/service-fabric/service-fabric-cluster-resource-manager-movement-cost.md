---
title: 'Hizmet Kumaş Küme Kaynak Yöneticisi: Hareket maliyeti'
description: Service Fabric hizmetlerinin hareket maliyeti ve dinamik yapılandırma da dahil olmak üzere herhangi bir mimari ihtiyaca uyacak şekilde nasıl belirtilebileceği hakkında bilgi edinin.
author: masnider
ms.topic: conceptual
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: af3e01d0d5a605c052be24eed8e14ee3449e2c79
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75563352"
---
# <a name="service-movement-cost"></a>Servis hareketi maliyeti
Hizmet Kumaş Küme Kaynak Yöneticisi'nin bir kümede hangi değişiklikleri yapacağını belirlemeye çalışırken dikkate aldığında göz önünde bulundururbir etken, bu değişikliklerin maliyetidir. "Maliyet" kavramı kümenin ne kadar geliştirilebileceğine göre işlem görür. Dengeleme, parçalanma ve diğer gereksinimler için hizmetlerin taşınmasında maliyet hesaba katLanır. Amaç, gereksinimleri en az yıkıcı veya pahalı bir şekilde karşılamaktır.

Taşıma hizmetleri CPU süresine ve ağ bant genişliğine en az maliyeti. Devlet hizmetleri için, bu hizmetlerin durumunu kopyalamak, ek bellek ve disk tüketerek gerektirir. Azure Hizmet Kumaş Küme Kaynak Yöneticisi'nin birlikte geldiği çözümlerin maliyetini en aza indirmek, kümenin kaynaklarının gereksiz yere harcanmamasını sağlamaya yardımcı olur. Ancak, kümedeki kaynakların tahsisini önemli ölçüde artıracak çözümleri de yok saymak istemezsiniz.

Küme Kaynak Yöneticisi'nin, kümeyi yönetmeye çalışırken maliyetleri hesaplamanın ve sınırlamanın iki yolu vardır. İlk mekanizma sadece yapacağı her hareketi saymaktır. Yaklaşık aynı bakiye (skor) ile iki çözüm oluşturulursa, Küme Kaynak Yöneticisi en düşük maliyete (toplam hamle sayısı) sahip olanı tercih eder.

Bu strateji iyi çalışıyor. Ancak varsayılan veya statik yüklerde olduğu gibi, karmaşık bir sistemde tüm hareketlerin eşit olması olası değildir. Bazı çok daha pahalı olması muhtemeldir.

## <a name="setting-move-costs"></a>Taşıma Maliyetlerini Ayarlama 
Oluşturulduğunda bir hizmetiçin varsayılan taşıma maliyetini belirtebilirsiniz:

PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -DefaultMoveCost Medium
```

C#: 

```csharp
FabricClient fabricClient = new FabricClient();
StatefulServiceDescription serviceDescription = new StatefulServiceDescription();
//set up the rest of the ServiceDescription
serviceDescription.DefaultMoveCost = MoveCost.Medium;
await fabricClient.ServiceManager.CreateServiceAsync(serviceDescription);
```

Ayrıca, hizmet oluşturulduktan sonra bir hizmet için MoveCost'u dinamik olarak belirtebilir veya güncelleştirebilirsiniz: 

PowerShell: 

```posh
Update-ServiceFabricService -Stateful -ServiceName "fabric:/AppName/ServiceName" -DefaultMoveCost High
```

C#:

```csharp
StatefulServiceUpdateDescription updateDescription = new StatefulServiceUpdateDescription();
updateDescription.DefaultMoveCost = MoveCost.High;
await fabricClient.ServiceManager.UpdateServiceAsync(new Uri("fabric:/AppName/ServiceName"), updateDescription);
```

## <a name="dynamically-specifying-move-cost-on-a-per-replica-basis"></a>Yineleme başına hareket maliyetini dinamik olarak belirtme

Önceki parçacıkların tümü, hizmetin kendisi dışından bir kerede bütün bir hizmet için MoveCost belirtmek içindir. Ancak, taşıma maliyeti en yararlı olan, belirli bir hizmet nesnesinin taşıma maliyetinin ömrü boyunca değiştiğinde olmasıdır. Hizmetlerin kendileri muhtemelen belirli bir zamanı taşımak için ne kadar maliyetli oldukları konusunda en iyi fikre sahip olduğundan, hizmetlerin çalışma süresi boyunca kendi bireysel taşıma maliyetini bildirmesi için bir API vardır. 

C#:

```csharp
this.Partition.ReportMoveCost(MoveCost.Medium);
```

## <a name="impact-of-move-cost"></a>Taşıma maliyetinin etkisi
MoveCost beş seviyeleri vardır: Sıfır, Düşük, Orta, Yüksek ve Çok Yüksek. Aşağıdaki kurallar geçerlidir:

* MoveCosts, Sıfır ve Çok Yüksek hariç olmak üzere birbirine göredir. 
* Sıfır hareket maliyeti, hareketin serbest olduğu ve çözümün puanına göre sayılmaması gerektiği anlamına gelir.
* Taşıma maliyetinizi Yüksek veya Çok Yüksek olarak ayarlamak, yinelemenin *hiçbir zaman* taşınmayacağıma dair bir garanti *sağlamaz.*
* VeryHigh taşıma maliyetine sahip yinelemeler yalnızca kümede başka bir şekilde düzeltilemeyen bir kısıtlama ihlali varsa taşınır (ihlali düzeltmek için diğer birçok yinelemenin taşınması gerekse bile)



<center>

![Hareket için yinelemeleri seçerken maliyeti bir faktör olarak taşıma][Image1]
</center>

MoveCost, genel olarak en az kesintiye neden olan ve eşdeğer bakiyeye ulaşmaya devam ederken elde etmesi en kolay çözümleri bulmanıza yardımcı olur. Bir hizmetin maliyet kavramı birçok şeye göreolabilir. Taşıma maliyetinizi hesaplamada en sık karşılaşılan etkenler şunlardır:

- Hizmetin taşıması gereken durum veya veri miktarı.
- Müşterilerin kopukluk maliyeti. Birincil yinelemeyi taşımak genellikle ikincil bir yinelemeyi taşıma maliyetinden daha maliyetlidir.
- Bir uçuş operasyonunu bölmenin bedeli. Veri deposu düzeyindeki bazı işlemler veya istemci çağrısına yanıt olarak gerçekleştirilen işlemler maliyetlidir. Belli bir noktadan sonra, eğer yapmak zorunda değilsen onları durdurmak istemezsin. Bu nedenle, işlem devam ederken, hareket etme olasılığını azaltmak için bu hizmet nesnesinin taşıma maliyetini artırırsınız. İşlem bittiğinde, maliyeti normale ayarlarsınız.

> [!IMPORTANT]
> Çok Yüksek taşıma maliyetini kullanmak, Küme Kaynak Yöneticisi'nin kümede genel olarak en iyi yerleşim çözümlerini bulma yeteneğini önemli ölçüde kısıtlayıştır olduğundan dikkatle düşünülmelidir. VeryHigh taşıma maliyetine sahip yinelemeler yalnızca kümede başka bir şekilde düzeltilemeyen bir kısıtlama ihlali varsa taşınır (ihlali düzeltmek için diğer birçok yinelemenin taşınması gerekse bile)

## <a name="enabling-move-cost-in-your-cluster"></a>Kümenizde taşıma maliyetini etkinleştirme
Daha ayrıntılı MoveCosts'in dikkate alınabilmesi için, MoveCost'un kümenizde etkinleştirilmesi gerekir. Bu ayar olmadan, movecost hesaplamak için hamle sayma varsayılan modu kullanılır ve MoveCost raporları yoksayılır.


ClusterManifest.xml:

``` xml
        <Section Name="PlacementAndLoadBalancing">
            <Parameter Name="UseMoveCostReports" Value="true" />
        </Section>
```

Bağımsız dağıtımlar için ClusterConfig.json veya Azure barındırılan kümeler için Template.json aracılığıyla:

```json
"fabricSettings": [
  {
    "name": "PlacementAndLoadBalancing",
    "parameters": [
      {
          "name": "UseMoveCostReports",
          "value": "true"
      }
    ]
  }
]
```

## <a name="next-steps"></a>Sonraki adımlar
- Service Fabric Cluster Resource Manger kümedeki tüketimi ve kapasiteyi yönetmek için ölçümler kullanır. Ölçümler ve bunları nasıl yapılandıracağınız hakkında daha fazla bilgi edinmek için, [kaynak tüketimini yönetme ve Hizmet Kumaşı'ndaki yükü ölçümlerle](service-fabric-cluster-resource-manager-metrics.md)yönetme'ye göz atın.
- Küme Kaynak Yöneticisi'nin kümedeki yükü nasıl yönettiği ve dengelerde nasıl yönettiği hakkında bilgi edinmek [için, Hizmet Kumaşı kümenizi dengeleme](service-fabric-cluster-resource-manager-balancing.md)konusuna göz atın.

[Image1]:./media/service-fabric-cluster-resource-manager-movement-cost/service-most-cost-example.png
