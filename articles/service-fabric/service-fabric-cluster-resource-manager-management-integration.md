---
title: Küme Kaynak Yöneticisi - Yönetim Tümleştirme
description: Küme Kaynak Yöneticisi ve Hizmet Kumaş Yönetimi arasındaki tümleştirme noktalarına genel bakış.
author: masnider
ms.topic: conceptual
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 50751c7d23797a597dc5e2d209c1e3eecf6f7a40
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79258752"
---
# <a name="cluster-resource-manager-integration-with-service-fabric-cluster-management"></a>Service Fabric küme yönetimi ile küme kaynak yöneticisi tümleştirmesi
Service Fabric Cluster Kaynak Yöneticisi, Service Fabric'teki yükseltmeleri yönlendirmez, ancak bu işin içindedir. Küme Kaynak Yöneticisi'nin yönetime yardımcı olmasının ilk yolu kümenin istenilen durumunu ve içindeki hizmetleri izlemektir. Küme Kaynak Yöneticisi, kümeyi istenilen yapılandırmaya koyamadığı zaman sistem durumu raporları gönderir. Örneğin, kapasite yetersizse Küme Kaynak Yöneticisi, sorunu gösteren sistem durumu uyarıları ve hataları gönderir. Entegrasyonun başka bir parçası yükseltmelerin nasıl çalıştığıyla ilgili. Küme Kaynak Yöneticisi yükseltmeleri sırasında davranışını biraz değiştirir.  

## <a name="health-integration"></a>Sağlık entegrasyonu
Küme Kaynak Yöneticisi, hizmetlerinizi yerleştirmek için tanımladığınız kuralları sürekli olarak izler. Ayrıca düğümler ve küme ve bir bütün olarak kümede her metrik için kalan kapasite izler. Bu kuralları yerine getirmez veya yeterli kapasite yoksa, sağlık uyarıları ve hataları yayılır. Örneğin, bir düğüm kapasitenin üzerindeyse ve Küme Kaynak Yöneticisi hizmetleri taşıyarak durumu düzeltmeye çalışır. Durumu düzeltemezse, hangi düğümün kapasitenin üzerinde olduğunu ve hangi ölçümler için olduğunu belirten bir sistem durumu uyarısı yayır.

Kaynak Yöneticisi'nin sistem durumu uyarılarının bir diğer örneği de yerleşim kısıtlamalarının ihlalidir. Örneğin, bir yerleşim kısıtlaması tanımladıysanız `“NodeColor == Blue”`(örneğin) ve Kaynak Yöneticisi bu kısıtlamanın ihlalini algılarsa, sistem durumu uyarısı yayır. Bu, özel kısıtlamalar ve varsayılan kısıtlamalar (Hata Etki Alanı ve Yükseltme Etki Alanı kısıtlamaları gibi) için geçerlidir.

İşte böyle bir sağlık raporu örneği. Bu durumda, sistem raporu sistem hizmetinin bölümlerinden biri içindir. Sistem durumu iletisi, bu bölümün yinelemelerinin geçici olarak çok az Yükseltme Etki Alanına paketlenmiş olduğunu gösterir.

```posh
PS C:\Users\User > Get-ServiceFabricPartitionHealth -PartitionId '00000000-0000-0000-0000-000000000001'


PartitionId           : 00000000-0000-0000-0000-000000000001
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='System.PLB', Property='ReplicaConstraintViolation_UpgradeDomain', HealthState='Warning', ConsiderWarningAsError=false.

ReplicaHealthStates   :
                        ReplicaId             : 130766528804733380
                        AggregatedHealthState : Ok

                        ReplicaId             : 130766528804577821
                        AggregatedHealthState : Ok

                        ReplicaId             : 130766528854889931
                        AggregatedHealthState : Ok

                        ReplicaId             : 130766528804577822
                        AggregatedHealthState : Ok

                        ReplicaId             : 130837073190680024
                        AggregatedHealthState : Ok

HealthEvents          :
                        SourceId              : System.PLB
                        Property              : ReplicaConstraintViolation_UpgradeDomain
                        HealthState           : Warning
                        SequenceNumber        : 130837100116930204
                        SentAt                : 8/10/2015 7:53:31 PM
                        ReceivedAt            : 8/10/2015 7:53:33 PM
                        TTL                   : 00:01:05
                        Description           : The Load Balancer has detected a Constraint Violation for this Replica: fabric:/System/FailoverManagerService Secondary Partition 00000000-0000-0000-0000-000000000001 is
                        violating the Constraint: UpgradeDomain Details: UpgradeDomain ID -- 4, Replica on NodeName -- Node.8 Currently Upgrading -- false Distribution Policy -- Packing
                        RemoveWhenExpired     : True
                        IsExpired             : False
                        Transitions           : Ok->Warning = 8/10/2015 7:13:02 PM, LastError = 1/1/0001 12:00:00 AM
```

Bu sağlık mesajının bize söylediği şu:

1. Tüm kopyaları kendilerini sağlıklı: Her AggregatedHealthState vardır: Ok
2. Yükseltme Etki Alanı dağıtım kısıtlaması şu anda ihlal ediliyor. Bu, belirli bir Yükseltme Etki Alanı'nın bu bölümden olması gerekenden daha fazla yinelemeye sahip olduğu anlamına gelir.
3. Hangi düğüm, ihlale neden olan yinelemeyi içerir. Bu durumda "Düğüm.8" adı ile düğüm
4. Bu bölüm için şu anda bir yükseltme olup olmadığı ("Şu anda Yükseltme -- yanlış")
5. Bu hizmetin dağıtım politikası: "Dağıtım Politikası -- Ambalaj". Bu, `RequireDomainDistribution` [yerleşim ilkesi](service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies.md#requiring-replica-distribution-and-disallowing-packing)tarafından yönetilir. "Packing", bu durumda DomainDistribution'un gerekli _olmadığını_ gösterir, bu nedenle bu hizmet için yerleşim ilkesinin belirtİlmediğini biliyoruz. 
6. Rapor ne zaman oldu - 08.10.2015 19:13:02

Bu güçler gibi bilgiler, bir şeylerin yanlış gittiğini ve kötü yükseltmeleri tespit etmek ve durdurmak için de kullanıldığını bildirmek için üretimdeki yangının sizi uyardığını uyarır. Bu durumda, Kaynak Yöneticisi'nin kopyaları neden Yükseltme Etki Alanı'na paketlemek zorunda kaldığını anlamaya calısabilir miyiz görmek isteriz. Örneğin, diğer Yükseltme Etki Alanlarındaki düğümler aşağıda olduğundan, genellikle paketleme geçicidir.

Küme Kaynak Yöneticisi'nin bazı hizmetleri yerleştirmeye çalıştığını, ancak işe yarayacak herhangi bir çözüm olmadığını varsayalım. Hizmetler yerleştirilemediğinde, genellikle aşağıdaki nedenlerden biridir:

1. Bazı geçici koşullar, bu hizmet örneğini veya yinelemeyi doğru bir şekilde yerleştirmeyi imkansız hale getirmiştir
2. Hizmetin yerleştirme gereksinimleri tatmin edici değildir.

Bu gibi durumlarda, Küme Kaynak Yöneticisi'nin sağlık raporları, hizmetin neden yerleştirilemeyeceğini belirlemenize yardımcı olur. Bu sürece kısıtlama giderme sırası diyoruz. Bu süre zarfında, sistem hizmeti etkileyen yapılandırılmış kısıtlamalar üzerinden yürür ve ne ortadan kaldırır kaydeder. Bu şekilde, hizmetler yerleştirilemediğinde, hangi düğümlerin ortadan kaldırıldığını ve neden ortadan kaldırıldığını görebilirsiniz.

## <a name="constraint-types"></a>Kısıtlama türleri
Bu sağlık raporlarındaki farklı kısıtlamaların her biri hakkında konuşalım. Yinelemeler yerleştirilmediğinde bu kısıtlamalarla ilgili sağlık iletilerini görürsünüz.

* **ReplicaExclusionStatic** ve **ReplicaExclusionDynamic**: Bu kısıtlamalar, aynı bölümden iki hizmet nesnesinin aynı düğüme yerleştirilmesi gerekeceği için bir çözümün reddedildiğini gösterir. Bu izin verilmez, çünkü o düğümün başarısızlığı bu bölümü aşırı etkiler. ReplicaExclusionStatic ve ReplicaExclusionDynamic hemen hemen aynı kural ve farklılıklar gerçekten önemli değildir. ÇoğaltmaExclusionStatic veya ReplicaExclusionDynamic kısıtlamasını içeren bir kısıtlama eleme dizisi görüyorsanız, Küme Kaynak Yöneticisi yeterli düğüm olmadığını düşünür. Bu, izin verilmeyen bu geçersiz yerleşimleri kullanmak için kalan çözümler gerektirir. Dizideki diğer kısıtlamalar genellikle düğümlerin neden ilk etapta ortadan kaldırıldığını bize söyleyecektir.
* **YerleşimKısıtlaması**: Bu iletiyi görürseniz, hizmetin yerleşim kısıtlamalarıyla eşleşmediği için bazı düğümleri ortadan kaldırdığımız anlamına gelir. Bu iletinin bir parçası olarak şu anda yapılandırılan yerleşim kısıtlamalarını izleriz. Tanımlanmış bir yerleşim kısıtlamanız varsa bu normaldir. Ancak, yerleşim kısıtlaması yanlış bir şekilde çok fazla düğümün ortadan kaldırılmasına neden oluyorsa, bunu fark esiniz.
* **Düğüm Kapasitesi**: Bu kısıtlama, Küme Kaynak Yöneticisi'nin yinelemeleri belirtilen düğümlere yerleştiremeyeceği anlamına gelir, çünkü bu onları kapasitenin üzerine koyar.
* **Yakınlık**: Bu kısıtlama, yinelemeyi etkilenen düğümlere yerleştiremediğimizi gösterir, çünkü yakınlık kısıtlamasının ihlaline neden olur. Yakınlık hakkında daha fazla bilgi [bu makalede](service-fabric-cluster-resource-manager-advanced-placement-rules-affinity.md)
* **FaultDomain** ve **UpgradeDomain**: Belirtilen düğümlere yinelemenin yerleştirilmesi belirli bir hata veya yükseltme etki alanında paketlemeye neden olacaksa, bu kısıtlama düğümleri ortadan kaldırır. Bu kısıtlamayı tartışan çeşitli [örnekler, hata ve yükseltme etki alanı kısıtlamaları ve ortaya çıkan davranış](service-fabric-cluster-resource-manager-cluster-description.md) konusu sunulmaktadır
* **PreferredLocation**: Varsayılan olarak bir optimizasyon olarak çalıştığından, normalde bu kısıtlamanın düğümleri çözümden kaldırdığını görmemeniz gerekir. Tercih edilen konum kısıtlaması yükseltmeler sırasında da bulunur. Yükseltme sırasında, hizmetleri yükseltme başladığında oldukları yere taşımak için kullanılır.

## <a name="blocklisting-nodes"></a>Engelleme Düğümleri
Küme Kaynak Yöneticisi'nin bildirdiği bir diğer sistem durumu iletisi de düğümlerin engellenmeleridir. Blocklisting'i sizin için otomatik olarak uygulanan geçici bir kısıtlama olarak düşünebilirsiniz. Bu hizmet türündeki örnekleri başlatırken yinelenen hatalarla karşılaştıklarında düğümler engellenir. Düğümler hizmet başına tür bazında engellenir. Bir düğüm bir hizmet türü için engellenebilir, ancak başka bir hizmet türü için engellenebilir. 

Geliştirme sırasında engelleme listesi nin sık sık devreye sayılacağını görürsünüz: bazı hata, servis ana bilgisayarınızın başlangıçta çökmesine neden olur. Service Fabric birkaç kez hizmet ana bilgisayar ını oluşturmaya çalışır ve hata oluşmaya devam eder. Birkaç denemeden sonra düğüm engellenir ve Küme Kaynak Yöneticisi hizmeti başka bir yerde oluşturmaya çalışır. Bu hata birden çok düğümde olmaya devam ederse, kümedeki geçerli düğümlerin tümü engellenmiş olabilir. Blocklisting ayrıca, istenen ölçeği karşılamak için hizmeti başarıyla başlatabilecek kadar çok düğümü de kaldırabilir. Genellikle Cluster Resource Manager'dan, hizmetin istenen yineleme veya örnek sayısının altında olduğunu belirten ek hatalar veya uyarılar ve ilk sırada engelleme listesine yol açan hatanın ne olduğunu gösteren sistem durumu iletileri görürsünüz. Yer.

Blocklisting kalıcı bir koşul değildir. Birkaç dakika sonra düğüm bloklisteden kaldırılır ve Servis Kumaşı bu düğümdeki hizmetleri yeniden etkinleştirebilir. Hizmetler başarısız olmaya devam ederse, düğüm bu hizmet türü için yeniden engellenir. 

### <a name="constraint-priorities"></a>Kısıtlama öncelikleri

> [!WARNING]
> Kısıtlama önceliklerini değiştirmek önerilmez ve kümeniz üzerinde önemli yan etkilere neden olabilir. Aşağıdaki bilgiler, varsayılan kısıtlama öncelikleri ve davranışları referans için sağlanmıştır. 
>

Tüm bu kısıtlamalar ile, "Hey - Ben hata etki alanı kısıtlamaları benim sistemdeki en önemli şey olduğunu düşünüyorum düşünüyor olabilir. Hata etki alanı kısıtlamasının ihlal olmadığından emin olmak için diğer kısıtlamaları ihlal etmeye hazırım."

Kısıtlamalar farklı öncelik düzeyleriile yapılandırılabilir. Bunlar:

   - "sert" (0)
   - "yumuşak" (1)
   - "optimizasyon" (2)
   - "kapalı" (-1). 
   
Kısıtlamaların çoğu varsayılan olarak sabit kısıtlamalar olarak yapılandırılır.

Kısıtlamaların önceliğini değiştirmek nadirdir. Kısıtlama önceliklerinin değişmesi gereken zamanlar olmuştur, genellikle çevreyi etkileyen başka bir hata veya davranış etrafında çalışmak için. Genellikle kısıtlama öncelikli altyapı esnekliği çok iyi çalıştı, ancak sık sık gerekli değildir. Çoğu zaman her şey varsayılan öncelikleri oturur. 

Öncelik düzeyleri, belirli bir kısıtlamanın ihlal _edilmeyeceği_ veya her zaman karşılanacağı anlamına gelmez. Kısıtlama öncelikleri, kısıtlamaların uygulandığı bir sıratanımlar. Tüm kısıtlamaların karşılanması imkansız olduğunda, öncelikler dengeleri tanımlar. Genellikle çevrede başka bir şey olmadığı sürece tüm kısıtlamalar tatmin edilebilir. Kısıtlama ihlallerine yol açacak senaryolara bazı örnekler, çakışan kısıtlamalar veya çok sayıda eşzamanlı hatadır.

Gelişmiş durumlarda, kısıtlama önceliklerini değiştirebilirsiniz. Örneğin, düğüm kapasitesi sorunlarını çözmek için gerektiğinde her zaman yakınlığın ihlal edildiğinden emin olmak istediğinizi varsan. Bunu başarmak için, yakınlık kısıtlamasının önceliğini "yumuşak" (1) olarak ayarlayabilir ve kapasite kısıtlamasını "sabit" (0) olarak ayarlayabilirsiniz.

Farklı kısıtlamalar için varsayılan öncelik değerleri aşağıdaki config belirtilir:

ClusterManifest.xml

```xml
        <Section Name="PlacementAndLoadBalancing">
            <Parameter Name="PlacementConstraintPriority" Value="0" />
            <Parameter Name="CapacityConstraintPriority" Value="0" />
            <Parameter Name="AffinityConstraintPriority" Value="0" />
            <Parameter Name="FaultDomainConstraintPriority" Value="0" />
            <Parameter Name="UpgradeDomainConstraintPriority" Value="1" />
            <Parameter Name="PreferredLocationConstraintPriority" Value="2" />
        </Section>
```

Bağımsız dağıtımlar için ClusterConfig.json veya Azure barındırılan kümeler için Template.json aracılığıyla:

```json
"fabricSettings": [
  {
    "name": "PlacementAndLoadBalancing",
    "parameters": [
      {
          "name": "PlacementConstraintPriority",
          "value": "0"
      },
      {
          "name": "CapacityConstraintPriority",
          "value": "0"
      },
      {
          "name": "AffinityConstraintPriority",
          "value": "0"
      },
      {
          "name": "FaultDomainConstraintPriority",
          "value": "0"
      },
      {
          "name": "UpgradeDomainConstraintPriority",
          "value": "1"
      },
      {
          "name": "PreferredLocationConstraintPriority",
          "value": "2"
      }
    ]
  }
]
```

## <a name="fault-domain-and-upgrade-domain-constraints"></a>Hata etki alanı ve yükseltme etki alanı kısıtlamaları
Küme Kaynak Yöneticisi, hizmetlerin hata ve yükseltme etki alanları arasında yayılmasını sağlamak istiyor. Bunu Cluster Resource Manager'ın altyapısında bir kısıtlama olarak modeller. Nasıl kullanıldıkları ve belirli davranışları hakkında daha fazla bilgi için [küme yapılandırması](service-fabric-cluster-resource-manager-cluster-description.md#fault-and-upgrade-domain-constraints-and-resulting-behavior)hakkındaki makaleye göz atın.

Küme Kaynak Yöneticisi yükseltmeleri, hataları veya diğer kısıtlama ihlalleri ile başa çıkmak için bir yükseltme etki içine birkaç yineleme paketi gerekebilir. Hata veya yükseltme etki alanları içine ambalaj normalde sadece birkaç hatalar veya sistemde doğru yerleşim engelleyen diğer karmaşa olduğunda olur. Bu durumlarda bile ambalaj önlemek istiyorsanız, `RequireDomainDistribution` [yerleşim ilkesini](service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies.md#requiring-replica-distribution-and-disallowing-packing)kullanabilirsiniz. Bunun bir yan etki olarak hizmet kullanılabilirliğini ve güvenilirliğini etkileyebileceğini unutmayın, bu nedenle dikkatli olun.

Ortam doğru şekilde yapılandırılırsa, yükseltmeler sırasında bile tüm kısıtlamalara tam olarak uyulmaktadır. Önemli olan Küme Kaynak Yöneticisi'nin kısıtlamalarınızı izlemesidir. Bir ihlali algıladığında hemen bildirir ve sorunu düzeltmeye çalışır.

## <a name="the-preferred-location-constraint"></a>Tercih edilen konum kısıtlaması
PreferredLocation kısıtlaması, iki farklı kullanım alanı olduğundan biraz farklıdır. Bu kısıtlamanın bir kullanımı uygulama yükseltmeleri sırasında. Küme Kaynak Yöneticisi yükseltmeleri sırasında bu kısıtlamayı otomatik olarak yönetir. Yükseltmeler tamamlandığında, yinelemelerin ilk konumlarına dönmesini sağlamak için kullanılır. Tercih Edilen Konum kısıtlamasının diğer kullanımı [ `PreferredPrimaryDomain` yerleşim ilkesi](service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies.md)içindir. Bunların her ikisi de optimizasyonlar ve bu nedenle PreferredLocation kısıtlamavarsayılan olarak "Optimizasyon" için ayarlanmış tek kısıtlamadır.

## <a name="upgrades"></a>Yükseltme
Küme Kaynak Yöneticisi, iki işi olduğu uygulama ve küme yükseltmeleri sırasında da yardımcı olur:

* kümenin kurallarından ödün verilmediğından emin olun
* yükseltme sorunsuz geçmesine yardımcı olmaya çalışın

### <a name="keep-enforcing-the-rules"></a>Kuralları zorlamaya devam edin
Dikkat edilmesi gereken en önemli şey, kuralların - yerleşim kısıtlamaları ve kapasiteleri gibi katı kısıtlamalar - yükseltmeleri sırasında hala uygulanmasıdır. Yerleşim kısıtlamaları, iş yüklerinizin yükseltmeler sırasında bile yalnızca izin verilen yerlerde çalışmasını sağlar. Hizmetler son derece kısıtlandığında, yükseltmeler daha uzun sürebilir. Üzerinde çalıştığı hizmet veya düğüm bir güncelleştirme için aşağı getirildiğinde, gidebileceği yerler için birkaç seçenek olabilir.

### <a name="smart-replacements"></a>Akıllı yedek
Yükseltme başladığında, Kaynak Yöneticisi kümenin geçerli düzenlemesinin anlık görüntüsünü alır. Her Yükseltme Etki Alanı tamamlandığında, bu Yükseltme Etki Alanı'nda bulunan hizmetleri özgün düzenlemelerine döndürmeye çalışır. Bu şekilde yükseltme sırasında bir hizmet için en fazla iki geçiş vardır. Etkilenen düğümden bir hareket, bir hamle geri. Kümeveya hizmeti yükseltmeden önceki hale döndürmek, yükseltmenin kümenin düzenini etkilememesini de sağlar. 

### <a name="reduced-churn"></a>Azaltılmış çalkalama
Yükseltmeler sırasında gerçekleşen bir diğer şey de Küme Kaynak Yöneticisi'nin dengelemeyi kapatmasIdır. Dengelemenin engellenmesi, hizmetleri yükseltme için boşaltılan düğümlere taşımak gibi yükseltmenin kendisine yapılan gereksiz reaksiyonları önler. Söz konusu yükseltme bir Küme yükseltmesi ise, yükseltme sırasında tüm küme dengelenmez. Kısıtlama denetimleri etkin kalır, yalnızca ölçümlerin proaktif dengelemesine dayalı hareket devre dışı bırakılır.

### <a name="buffered-capacity--upgrade"></a>Arabelleğe Alma Kapasitesi & Yükseltme
Genellikle küme kısıtlı veya doluya yakın olsa bile yükseltmenin tamamlanmasını istersiniz. Kümenin kapasitesini yönetmek yükseltmeler sırasında normalden daha önemlidir. Yükseltme etki alanlarının sayısına bağlı olarak, yükseltme kümede yuvarlanırken kapasitenin yüzde 5 ila 20'si geçirilmelidir. Bu iş bir yere gitmeli. Bu, [arabelleğe alma kapasiteleri](service-fabric-cluster-resource-manager-cluster-description.md#buffered-capacity) kavramının yararlı olduğu yerdir. Arabelleğe alma kapasitesi normal çalışma sırasında saygı duyulur. Küme Kaynak Yöneticisi, gerekirse yükseltmeler sırasında düğümleri toplam kapasitelerine (arabellek tüketen) kadar doldurabilir.

## <a name="next-steps"></a>Sonraki adımlar
* Baştan başlayın ve [Hizmet Kumaş Küme Kaynak Yöneticisi'ne Giriş alın](service-fabric-cluster-resource-manager-introduction.md)
