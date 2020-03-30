---
title: Service Fabric küme kaynak yöneticisinde azaltma
description: Service Fabric Cluster Resource Manager tarafından sağlanan gazları yapılandırmayı öğrenin.
author: masnider
ms.topic: conceptual
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: b4d78b339bab02b5c44a31939e0da769dc21c3ec
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75452171"
---
# <a name="throttling-the-service-fabric-cluster-resource-manager"></a>Hizmet Kumaş Kümesi Kaynak Yöneticisini Daraltma
Küme Kaynak Yöneticisi'ni doğru şekilde yapılandırmış olsanız bile, küme bozulabilir. Örneğin, eşzamanlı düğüm ve hata etki alanı hataları olabilir - bu bir yükseltme sırasında meydana gelirse ne olur? Küme Kaynak Yöneticisi her zaman her şeyi düzeltmeye çalışır ve kümenin kaynaklarını yeniden düzenlemeye ve düzeltmeye çalışır. Throttles küme stabilize etmek için kaynakları kullanabilirsiniz böylece bir backstop sağlamaya yardımcı olur - düğümleri geri gelir, ağ bölümleri iyileşir, düzeltilmiş bit dağıtılır.

Bu tür durumlarda yardımcı olmak için, Service Fabric Cluster Kaynak Yöneticisi birkaç azaltma içerir. Bu gazların hepsi oldukça büyük çekiçler. Genellikle dikkatli planlama ve test olmadan değiştirilmemelidir.

Küme Kaynak Yöneticisi'nin gazlarını değiştirirseniz, bunları beklenen gerçek yüke ayarlamanız gerekir. Bazı durumlarda kümenin sabitleminin daha uzun sürdüğü anlamına gelse bile, bazı gazların yerinde olması gerektiğine karar verebilirsiniz. Azaltma için doğru değerleri belirlemek için test gereklidir. Throttles kümenin değişikliklere makul bir süre içinde yanıt vermesi için yeterince yüksek ve aslında çok fazla kaynak tüketimini önlemek için yeterince düşük olması gerekir. 

Çoğu zaman müşterilerin zaten kaynak kısıtlı bir ortamda oldukları için gaz kullandığını gördük. Bazı örnekler, tek tek düğümler veya iş verme sınırlamaları nedeniyle paralel olarak çok sayıda durumlu yineleme oluşturamayan diskler için sınırlı ağ bant genişliği olacaktır. Azaltma olmadan, işlemler bu kaynakları bastırarak işlemlerin başarısız veya yavaş olmasını sağlayabilir. Bu gibi durumlarda müşteriler gaz kullandı ve kümenin kararlı bir duruma ulaşması için gereken süreyi genişlettiklerini biliyorlardı. Müşteriler ayrıca, daraltılırken genel güvenilirlik daha düşük bir hızda çalışmaya devam edebileceklerini de anladılar.


## <a name="configuring-the-throttles"></a>Gazların yapılandırılması

Service Fabric çoğaltma hareketlerinin sayısını daraltma için iki mekanizmavardır. Service Fabric 5.7'den önce var olan varsayılan mekanizma, izin verilen mutlak hareket sayısı olarak azaltmayı temsil eder. Bu, her boyuttaki kümeler için çalışmaz. Özellikle, büyük kümeler için varsayılan değer çok küçük olabilir, gerektiğinde bile dengelemeyi önemli ölçüde yavaşlatabilir, ancak küçük kümelerde hiçbir etkisi yoktur. Bu önceki mekanizma, hizmet ve düğüm sayısının düzenli olarak değiştiği dinamik kümelerle daha iyi ölçeklenen yüzde tabanlı azaltma ile değiştirilmiştir.

Azaltma kümelerde çoğaltma sayısının bir yüzdesi dayanmaktadır. Yüzde tabanlı gazlar kuralı ifade etmenizi sağlar: "örneğin, 10 dakikalık bir aralıkta yinelemelerin %10'undan fazlasını hareket ettirin".

Yüzde tabanlı azaltma için yapılandırma ayarları şunlardır:

  - GlobalMovementThrottleThresholdPercentage - Kümede herhangi bir zamanda izin verilen maksimum hareket sayısı, kümedeki toplam yineleme sayısının yüzdesi olarak ifade edilir. 0 sınır yok gösterir. Varsayılan değer 0’dır. Hem bu ayar hem de GlobalMovementThrottleThreshold belirtilirse, daha konservatif sınır kullanılır.
  - GlobalMovementThrotleThresholdPercentageForPlacement - Kümedeki toplam yineleme sayısının yüzdesi olarak ifade edilen yerleşim aşamasında izin verilen maksimum hareket sayısı. 0 sınır yok gösterir. Varsayılan değer 0’dır. Hem bu ayar hem de GlobalMovementThrottleThresholdForPlacement belirtilirse, daha tutucu sınır kullanılır.
  - GlobalMovementThrotleThresholdPercentageForBalancing - Dengeleme aşamasında izin verilen maksimum hareket sayısı, kümedeki toplam kopya sayısının yüzdesi olarak ifade edilir. 0 sınır yok gösterir. Varsayılan değer 0’dır. Hem bu ayar hem de GlobalMovementThrottleThresholdForBalancing belirtilirse, daha konservatif sınır kullanılır.

Gaz yüzdesini belirtirken, %5'i 0,05 olarak belirtirsiniz. Bu gazların yönetildiği aralık, saniyeler içinde belirtilen GlobalMovementThrottleCountingInterval'dır.


``` xml
<Section Name="PlacementAndLoadBalancing">
     <Parameter Name="GlobalMovementThrottleThresholdPercentage" Value="0" />
     <Parameter Name="GlobalMovementThrottleThresholdPercentageForPlacement" Value="0" />
     <Parameter Name="GlobalMovementThrottleThresholdPercentageForBalancing" Value="0" />
     <Parameter Name="GlobalMovementThrottleCountingInterval" Value="600" />
</Section>
```

Bağımsız dağıtımlar için ClusterConfig.json veya Azure barındırılan kümeler için Template.json aracılığıyla:

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

### <a name="default-count-based-throttles"></a>Varsayılan sayı tabanlı azaltma
Bu bilgiler, eski kümelere sahip seniz veya bu yapılandırmaları o zamandan beri yükseltilmiş kümelerde tutmaya devam ederseniz sağlanır. Genel olarak, bunların yukarıdaki yüzde tabanlı gazlarla değiştirilmesi önerilir. Yüzde tabanlı azaltma varsayılan olarak devre dışı bırakıldığından, bu azaltmalar devre dışı bırakılana ve yüzde tabanlı gazlarla değiştirilene kadar küme için varsayılan azaltma olarak kalır. 

  - GlobalMovementThrotleThreshold – bu ayar kümedeki toplam hareket sayısını bir süre boyunca denetler. Süre, GlobalMovementThrottleCountingInterval olarak saniyecinsinden belirtilir. GlobalMovementThrottleThreshold için varsayılan değer 1000 ve GlobalMovementThrottleCountingInterval için varsayılan değer 600'dür.
  - MovementPerPartitionThrotleThreshold – Bu ayar, bir süre içinde herhangi bir hizmet bölümü için toplam hareket sayısını kontrol eder. Zaman miktarı MovementPerPartitionThrottleCountingInterval olarak saniye cinsinden belirtilir. MovementPerPartitionThrottleThreshold için varsayılan değer 50 ve MovementPerPartitionThrottleCountingInterval için varsayılan değer 600'dür.

Bu azaltmalar için yapılandırma yüzde tabanlı azaltma aynı desen izler.

## <a name="next-steps"></a>Sonraki adımlar
- Küme Kaynak Yöneticisi'nin kümedeki yükü nasıl yönettiği ve dengeler yaptığı hakkında bilgi edinmek [için, yükü dengeleme](service-fabric-cluster-resource-manager-balancing.md) makalesine göz atın
- Küme Kaynak Yöneticisi kümeyi açıklamak için birçok seçenek vardır. Onlar hakkında daha fazla bilgi edinmek için, [Bir Hizmet Kumaş kümesi açıklayan](service-fabric-cluster-resource-manager-cluster-description.md) bu makaleye göz atın
