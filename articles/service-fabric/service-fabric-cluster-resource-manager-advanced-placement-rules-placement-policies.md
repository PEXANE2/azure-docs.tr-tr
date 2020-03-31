---
title: Hizmet Kumaş Küme Kaynak Yöneticisi - Yerleşim İlkeleri
description: Hizmet Kumaşı Hizmetleri için ek yerleşim ilkelerine ve kurallarına genel bakış
author: masnider
ms.topic: conceptual
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 9aea157d03f344e07a81f0588d3e0127f17ca75d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75834459"
---
# <a name="placement-policies-for-service-fabric-services"></a>Hizmet kumaş ı hizmetleri için yerleştirme ilkeleri
Yerleşim ilkeleri, hizmet yerleşimini belirli, daha az yaygın senaryolarda yönetmek için kullanılabilecek ek kurallardır. Bu senaryolara örnek olarak şunlar verilebilir:

- Hizmet Kumaşı kümeniz, birden çok şirket içi veri merkezi veya Azure bölgeleri gibi coğrafi mesafeleri kapsar
- Ortamınız, jeopolitik veya yasal kontrolün birden fazla alanını veya uygulamanız gereken politika sınırlarına sahip olduğunuz başka bir
- Büyük mesafeler veya daha yavaş veya daha az güvenilir ağ bağlantılarının kullanımı nedeniyle iletişim performansı veya gecikme seçimleri var
- Diğer iş yükleri veya müşterilere yakın olarak, en iyi çaba olarak belirli iş yüklerini bir arada tutmanız gerekir

Bu gereksinimlerin çoğu kümenin hata etki alanları olarak temsil edilen kümenin fiziksel düzeniyle hizalanır. 

Bu senaryoları ele almaya yardımcı olan gelişmiş yerleşim ilkeleri şunlardır:

1. Geçersiz etki alanları
2. Gerekli etki alanları
3. Tercih edilen etki alanları
4. Replika ambalajına izin vermeme

Aşağıdaki denetimlerin çoğu düğüm özellikleri ve yerleşim kısıtlamaları ile yapılandırılabilir, ancak bazıları daha karmaşıktır. İşleri kolaylaştırmak için, Service Fabric Cluster Kaynak Yöneticisi bu ek yerleşim ilkelerini sağlar. Yerleşim ilkeleri, adlandırılmış hizmet örneği bazında yapılandırılır. Dinamik olarak da güncellenebilir.

## <a name="specifying-invalid-domains"></a>Geçersiz etki alanlarını belirtme
**Geçersiz Etki Alanı** yerleşim ilkesi, belirli bir Hata Etki Alanının belirli bir hizmet için geçersiz olduğunu belirtmenize olanak tanır. Bu politika, belirli bir hizmetin jeopolitik veya kurumsal politika nedenleriyle belirli bir alanda çalışmamasını sağlar. Birden çok geçersiz etki alanı ayrı ilkeler aracılığıyla belirtilebilir.

<center>

![Geçersiz Etki Alanı Örneği][Image1]
</center>

Kod:

```csharp
ServicePlacementInvalidDomainPolicyDescription invalidDomain = new ServicePlacementInvalidDomainPolicyDescription();
invalidDomain.DomainName = "fd:/DCEast"; //regulations prohibit this workload here
serviceDescription.PlacementPolicies.Add(invalidDomain);
```

PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("InvalidDomain,fd:/DCEast”)
```
## <a name="specifying-required-domains"></a>Gerekli etki alanlarını belirtme
Gerekli etki alanı yerleşimi ilkesi, hizmetin yalnızca belirtilen etki alanında bulunmasını gerektirir. Birden çok gerekli etki alanı ayrı ilkeler aracılığıyla belirtilebilir.

<center>

![Gerekli Etki Alanı Örneği][Image2]
</center>

Kod:

```csharp
ServicePlacementRequiredDomainPolicyDescription requiredDomain = new ServicePlacementRequiredDomainPolicyDescription();
requiredDomain.DomainName = "fd:/DC01/RK03/BL2";
serviceDescription.PlacementPolicies.Add(requiredDomain);
```

PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("RequiredDomain,fd:/DC01/RK03/BL2")
```

## <a name="specifying-a-preferred-domain-for-the-primary-replicas-of-a-stateful-service"></a>Devlet hizmetinin birincil yinelemeleri için tercih edilen etki alanını belirtme
Tercih Edilen Birincil Etki Alanı, Birincil'i yerleştirmek için hata etki alanını belirtir. Birincil her şey sağlıklı olduğunda bu etki alanında sona erer. Etki alanı veya Birincil yineleme başarısız olursa veya kapanırsa, Birincil ideal olarak aynı etki alanında başka bir konuma taşınır. Bu yeni konum tercih edilen etki alanında değilse, Cluster Resource Manager onu en kısa sürede tercih edilen etki alanına geri taşır. Doğal olarak bu ayar sadece devlet hizmetleri için mantıklı. Bu ilke, azure bölgeleri veya birden çok veri merkezi arasında yayılan ancak belirli bir konumda yerleşimi tercih eden hizmetleri olan kümelerde en kullanışlıdır. Öncelikler'i kullanıcılarına veya diğer hizmetlere yakın tutmak, özellikle varsayılan olarak Öncelikler tarafından işlenen okumalar için daha düşük gecikme gecikmesi sağlamaya yardımcı olur.

<center>

![Tercih Edilen Birincil Etki Alanları ve Failover][Image3]
</center>

```csharp
ServicePlacementPreferPrimaryDomainPolicyDescription primaryDomain = new ServicePlacementPreferPrimaryDomainPolicyDescription();
primaryDomain.DomainName = "fd:/EastUS/";
serviceDescription.PlacementPolicies.Add(primaryDomain);
```

PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("PreferredPrimaryDomain,fd:/EastUS")
```

## <a name="requiring-replica-distribution-and-disallowing-packing"></a>Çoğaltma dağıtımı gerektiren ve ambalajlamanın izin vermemesi
Yinelemeler _normalde_ küme sağlıklı olduğunda hata ve yükseltme etki alanları arasında dağıtılır. Ancak, belirli bir bölüm için birden fazla yinelemenin geçici olarak tek bir etki alanına paketlenebileceği durumlar vardır. Örneğin, kümenin üç hata etki alanında fd:/0, fd:/1 ve fd:/2 olmak üzere dokuz düğümü olduğunu varsayalım. Hizmetinizin üç kopyası olduğunu da varsayalım. Diyelim ki fd:/1 ve fd:/2'deki kopyalar için kullanılan düğümler indi. Normalde Küme Kaynak Yöneticisi, aynı hata etki alanlarındaki diğer düğümleri tercih eder. Bu durumda, kapasite sorunları nedeniyle bu etki alanlarındaki diğer düğümlerin hiçbirinin geçerli olmadığını söyleyebiliriz. Küme Kaynak Yöneticisi bu yinelemeler için yedekler oluşturursa, fd:/0 düğümleri seçmek zorunda kalır. Ancak, _bunu_ yapmak Hata Etki Alanı kısıtlamasının ihlal edildiği bir durum oluşturur. Yinelemeleri paketleme, tüm çoğaltma kümesinin aşağı inme veya kaybolma olasılığını artırır. 

> [!NOTE]
> Kısıtlamalar ve kısıtlama öncelikleri hakkında genel olarak daha fazla bilgi için [bu konuya](service-fabric-cluster-resource-manager-management-integration.md#constraint-priorities)göz atın.
>

Eğer hiç gibi bir sağlık mesajı`The Load Balancer has detected a Constraint Violation for this Replica:fabric:/<some service name> Secondary Partition <some partition ID> is violating the Constraint: FaultDomain`gördüyseniz " ", o zaman bu durum ya da buna benzer bir şey isabet ettik. Genellikle yalnızca bir veya iki yineleme geçici olarak birlikte paketlenir. Belirli bir etki alanında çok sayıda çoğaltma olduğu sürece güvendesiniz. Ambalaj nadirdir, ancak olabilir, ve genellikle bu durumlar düğümleri geri geldiğinden beri geçicidir. Düğümler aşağıda kalırsa ve Küme Kaynak Yöneticisi'nin değiştirmeler oluşturması gerekiyorsa, genellikle ideal hata etki alanlarında başka düğümler de vardır.

Bazı iş yükleri, daha az etki alanına paketlenmiş olsalar bile, her zaman hedef çoğaltma sayısına sahip olmayı tercih eder. Bu iş yükleri, toplam eşzamanlı kalıcı etki alanı hatalarına karşı bahis vardır ve genellikle yerel durumu kurtarabilir. Diğer iş yükleri, risk doğruluğu veya veri kaybı yerine kapalı kalma süresini daha erken almayı tercih eder. Çoğu üretim iş yükü üçten fazla yineleme, üçten fazla hata etki alanı ve hata etki alanı başına çok sayıda geçerli düğümle çalışır. Bu nedenle, varsayılan davranış varsayılan olarak etki alanı paketleme sağlar. Varsayılan davranış, geçici etki alanı paketleme anlamına gelse bile, normal dengeleme ve bu aşırı durumlarda başarısızlık sağlar.

Belirli bir iş yükü için bu tür ambalajı devre `RequireDomainDistribution` dışı kılmış olmak istiyorsanız, hizmetteki ilkeyi belirtebilirsiniz. Bu ilke ayarlandığında, Küme Kaynak Yöneticisi aynı bölümden iki yinelemenin aynı hatada veya yükseltme etki alanında çalışmasını sağlamaz.

Kod:

```csharp
ServicePlacementRequireDomainDistributionPolicyDescription distributeDomain = new ServicePlacementRequireDomainDistributionPolicyDescription();
serviceDescription.PlacementPolicies.Add(distributeDomain);
```

PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("RequiredDomainDistribution")
```

Şimdi, bu yapılandırmaları coğrafi olarak yayılmamış bir kümedeki hizmetler için kullanmak mümkün mü? Yapabilirdin, ama büyük bir sebep de yok. Gerekli, geçersiz ve tercih edilen etki alanı yapılandırmaları, senaryolar gerektirmedikçe kaçınılmalıdır. Belirli bir iş yükünü tek bir rafta çalıştırmaya zorlamak veya yerel kümenizin bir bölümünü başka bir rafta tercih etmek mantıklı değildir. Farklı donanım yapılandırmaları hata etki alanları arasında yayılmalıdır ve normal yerleşim kısıtlamaları ve düğüm özellikleri ile işlenir.

## <a name="next-steps"></a>Sonraki adımlar
- Hizmetleri yapılandırma hakkında daha fazla bilgi [için, Hizmetleri yapılandırma hakkında bilgi edinin](service-fabric-cluster-resource-manager-configure-services.md)

[Image1]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies/cluster-invalid-placement-domain.png
[Image2]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies/cluster-required-placement-domain.png
[Image3]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies/cluster-preferred-primary-domain.png
