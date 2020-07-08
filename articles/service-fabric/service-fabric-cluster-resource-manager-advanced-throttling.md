---
title: Service Fabric kümesi kaynak yöneticisi 'nde azaltma
description: Service Fabric kümesi Kaynak Yöneticisi tarafından sunulan kısıtları yapılandırmayı öğrenin.
author: masnider
ms.topic: conceptual
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: b4d78b339bab02b5c44a31939e0da769dc21c3ec
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "75452171"
---
# <a name="throttling-the-service-fabric-cluster-resource-manager"></a>Service Fabric kümesini daraltma Kaynak Yöneticisi
Küme Kaynak Yöneticisi doğru şekilde yapılandırsanız bile, küme kesilebilir. Örneğin, eşzamanlı düğüm ve hata etki alanı hatası olabilir. yükseltme sırasında bu durum oluşursa ne olur? Küme Kaynak Yöneticisi her zaman her şeyi gidermeye çalışır ve kümeyi yeniden düzenlemeye ve gidermeye çalışan kümenin kaynaklarını tüketerek. Kısıtlar ki, kümenin kararlı hale getirmek için kaynakları kullanabilmesi için bir geri durağı sağlar. düğümler geri gelir, ağ bölümleri, düzeltilen bitler dağıtılır.

Bu tür durumlara yardımcı olmak için Service Fabric kümesi Kaynak Yöneticisi çeşitli kısıtları içerir. Bu kısıtların hepsi oldukça büyük hakökleri. Genellikle dikkatli planlama ve test olmadan değiştirilmemelidir.

Küme Kaynak Yöneticisi kısıtlarını değiştirirseniz, bunları beklenen gerçek yükle ayarlamanız gerekir. Kümenin bazı durumlarda daha uzun süre sürme anlamına geliyor olsa bile, bazı kısıtlamalar yapmanız gerektiğini belirleyebilirsiniz. Kısıtlar için doğru değerleri belirlemek için test gereklidir. Kümenin değişikliklere makul bir süre içinde yanıt vermesini ve çok fazla kaynak tüketimine engel olmak için yeterince düşük olması gerekir. 

Müşterilerin gördük çoğu zaman, zaten kaynak kısıtlı bir ortamda olduğu için bu bir azaldık. Bazı örnekler, tek tek düğümler için sınırlı ağ bant genişliğidir veya üretilen iş sınırlamaları nedeniyle paralel olarak çok sayıda durum bilgisi olan çoğaltmalar oluşturamayacak diskler olabilir. Kısıtlamadan, işlemler bu kaynakları kesintiye sokabilir ve işlemlerin başarısız olmasına veya yavaş olmasına neden olabilir. Bu durumlarda, müşteriler, kümenin kararlı duruma ulaşması için gereken süreyi genişlettikleri ve bildikleri süreyi azaltır. Müşteriler, azaldıkça daha düşük bir genel güvenilirlik altında çalışmaya bitebilirler.


## <a name="configuring-the-throttles"></a>Kısıtları yapılandırma

Service Fabric çoğaltma hareketlerinin sayısını daraltma için iki mekanizma vardır. Service Fabric 5,7 ' den önce mevcut olan varsayılan mekanizma, izin verilen mutlak sayıda hareketi olarak azaltmayı temsil eder. Bu, her boyuttaki kümeler için çalışmaz. Özellikle, büyük kümeler için varsayılan değer çok küçük olabilir, gerekli olduğunda bile dengelemeyi önemli ölçüde yavaşlatarak daha küçük kümeler üzerinde hiçbir etkisi olmaz. Bu önceki mekanizmanın yerine, hizmet ve düğümlerin sayısı düzenli olarak değişen dinamik kümelerle daha iyi ölçeklendirerek yüzde tabanlı azaltmaya karşı değiştirilmiştir.

Kısıtlar, kümelerdeki yineleme sayısının yüzdesini temel alır. Yüzde tabanlı kısıtlar kuralın "10 dakikalık bir aralıkta %10 ' dan fazlasını taşıma", örneğin

Yüzde tabanlı azaltma için yapılandırma ayarları şunlardır:

  - GlobalMovementThrottleThresholdPercentage-kümede her zaman izin verilen maksimum hareket sayısı (kümedeki toplam çoğaltma sayısının yüzdesi olarak ifade edilir). 0 sınır olmadığını gösterir. Varsayılan değer 0’dır. Hem bu ayar hem de GlobalMovementThrottleThreshold belirtilmişse, daha fazla koruyucu sınırı kullanılır.
  - GlobalMovementThrottleThresholdPercentageForPlacement-yerleştirme aşamasında izin verilen en fazla taşıma sayısı, kümedeki toplam çoğaltma sayısının yüzdesi olarak ifade edilir. 0 sınır olmadığını gösterir. Varsayılan değer 0’dır. Hem bu ayar hem de Globalmovementthrottlethresholdforyerleştirmesi belirtilmişse, daha fazla koruyucu sınır kullanılır.
  - GlobalMovementThrottleThresholdPercentageForBalancing-Dengeleme aşamasında izin verilen en fazla hareket sayısı, kümedeki toplam çoğaltma sayısının yüzdesi olarak ifade edilir. 0 sınır olmadığını gösterir. Varsayılan değer 0’dır. Hem bu ayar hem de Globalmovementthrottlethresholdfordengeleme belirtilmişse, daha fazla koruyucu sınırı kullanılır.

Kısıtlama yüzdesini belirtirken, 0,05 olarak %5 değerini belirtmeniz gerekir. Bu kısıtlamaların yönetileceği Aralık, saniyeler içinde belirtilen Globalmovementthrottlecountingınterval ' dır.


``` xml
<Section Name="PlacementAndLoadBalancing">
     <Parameter Name="GlobalMovementThrottleThresholdPercentage" Value="0" />
     <Parameter Name="GlobalMovementThrottleThresholdPercentageForPlacement" Value="0" />
     <Parameter Name="GlobalMovementThrottleThresholdPercentageForBalancing" Value="0" />
     <Parameter Name="GlobalMovementThrottleCountingInterval" Value="600" />
</Section>
```

Tek başına dağıtımlar veya Azure 'da barındırılan kümeler için Template.jsClusterConfig.jsaracılığıyla:

```json
"fabricSettings": [
  {
    "name": "PlacementAndLoadBalancing",
    "parameters": [
      {
          "name": "GlobalMovementThrottleThresholdPercentage",
          "value": "0.0"
      },
      {
          "name": "GlobalMovementThrottleThresholdPercentageForPlacement",
          "value": "0.0"
      },
      {
          "name": "GlobalMovementThrottleThresholdPercentageForBalancing",
          "value": "0.0"
      },
      {
          "name": "GlobalMovementThrottleCountingInterval",
          "value": "600"
      }
    ]
  }
]
```

### <a name="default-count-based-throttles"></a>Varsayılan sayı tabanlı kısıtları
Bu bilgiler, daha eski kümeleriniz varsa veya bu yapılandırmaların yükseltilme sonrasında bu yapılandırmaların hala tutulması durumunda sunulmaktadır. Genel olarak, bunların yukarıdaki yüzde tabanlı kısıtlamalar ile değiştirilmesini öneririz. Yüzde tabanlı azaltma varsayılan olarak devre dışı bırakıldığı için, bu kısıtlamalar devre dışı bırakılıncaya ve yüzde tabanlı kısıtlarla değiştirilene kadar bir küme için varsayılan kısıtları azaltır. 

  - GlobalMovementThrottleThreshold – Bu ayar, kümedeki hareketlerin bir süre içinde toplam hareket sayısını denetler. Toplam süre, Globalmovementthrottlecountingınterval olarak Saniyeler içinde belirtilir. GlobalMovementThrottleThreshold için varsayılan değer 1000, Globalmovementthrottlecountingınterval için de varsayılan değer 600 ' dir.
  - MovementPerPartitionThrottleThreshold – Bu ayar, herhangi bir zamanda herhangi bir hizmet bölümünün toplam hareket sayısını denetler. Süre, Movementperpartitionthrottlecountingınterval olarak Saniyeler içinde belirtilir. MovementPerPartitionThrottleThreshold için varsayılan değer 50 ' dir ve Movementperpartitionthrottlecountingınterval için varsayılan değer 600 ' dir.

Bu kısıtların yapılandırması, yüzde tabanlı daraltma ile aynı kalıbı izler.

## <a name="next-steps"></a>Sonraki adımlar
- Kaynak Yöneticisi kümenin, kümedeki yükü nasıl yönettiğini ve dengelemediğini öğrenmek için [Yük Dengeleme](service-fabric-cluster-resource-manager-balancing.md) sayfasındaki makaleye göz atın
- Küme Kaynak Yöneticisi, kümeyi açıklamak için birçok seçenek içerir. Bunlarla ilgili daha fazla bilgi edinmek için [Service Fabric kümesini açıklama](service-fabric-cluster-resource-manager-cluster-description.md) konusunda bu makaleye göz atın
