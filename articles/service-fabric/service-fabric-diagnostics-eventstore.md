---
title: Azure Service Fabric olay deposu
description: Bir kümenin veya iş yükünün durumunu dilediğiniz zaman anlamak ve izlemek için bir yol olan Azure Service Fabric EventStore hakkında bilgi edinin.
author: srrengar
ms.topic: conceptual
ms.date: 6/6/2019
ms.author: srrengar
ms.openlocfilehash: d23c8114bf10ef3225775accef6910c0ba539e15
ms.sourcegitcommit: f788bc6bc524516f186386376ca6651ce80f334d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2020
ms.locfileid: "75645744"
---
# <a name="eventstore-overview"></a>EventStore 'A genel bakış

>[!NOTE]
>Service Fabric sürümünden itibaren 6,4. EventStore API 'Leri yalnızca yalnızca Azure üzerinde çalışan Windows kümelerinde kullanılabilir. Tek başına kümelerimizin yanı sıra bu işlevselliği Linux 'a taşıma konusunda çalışıyoruz.

## <a name="overview"></a>Genel Bakış

Sürüm 6,2 ' de tanıtılan EventStore hizmeti Service Fabric bir izleme seçeneğidir. EventStore, belirli bir noktadaki kümenizin veya iş yüklerinizin durumunu anlamak için bir yol sağlar.
EventStore, kümeden olayları tutan, durum bilgisi olan bir Service Fabric hizmetidir. Olay Service Fabric Explorer, REST ve API 'Ler aracılığıyla sunulur. EventStore, kümenizdeki herhangi bir varlıkta Tanılama verileri almak için kümeyi doğrudan sorgular ve yardımcı olması için kullanılmalıdır:

* Geliştirme veya test ederken sorunları tanılayın veya bir izleme işlem hattı kullanıyor olabilirsiniz
* Kümenizde yaptığınız yönetim eylemlerinin doğru şekilde işleneceğini onaylayın
* Service Fabric belirli bir varlıkla nasıl etkileşim kurduğuna ilişkin bir "Snapshot" alın

![EventStore](media/service-fabric-diagnostics-eventstore/eventstore.png)

EventStore 'da bulunan olayların tam listesini görmek için bkz. [Service Fabric olayları](service-fabric-diagnostics-event-generation-operational.md).

>[!NOTE]
>Service Fabric sürümünden itibaren 6,4. EventStore API 'Leri ve UX, Azure Windows kümeleri için genel kullanıma sunulmuştur. Tek başına kümelerimizin yanı sıra bu işlevselliği Linux 'a taşıma konusunda çalışıyoruz.

Olayınızda her varlık ve varlık türü için kullanılabilen olaylar için EventStore hizmeti sorgulanabilir. Bu, aşağıdaki düzeylerde olayları sorgulayabilmeniz anlamına gelir:
* Küme: kümeye özgü Olaylar (örneğin, küme yükseltmesi)
* Düğümler: tüm düğüm düzeyi olaylar
* Düğüm: `nodeName` tarafından tanımlanan, bir düğüme özgü olaylar
* Uygulamalar: tüm uygulama düzeyi olaylar
* Uygulama: `applicationId` tarafından tanımlanan bir uygulamaya özgü olaylar
* Hizmetler: kümelerinizdeki tüm hizmetlerden gelen olaylar
* Hizmet: `serviceId` tarafından tanımlanan belirli bir hizmetten alınan olaylar
* Bölümler: tüm bölümlerin olayları
* Partition: `partitionId` tarafından tanımlanan belirli bir bölümden alınan olaylar
* Bölüm çoğaltmaları: `partitionId` tarafından tanımlanan belirli bir bölüm içindeki tüm çoğaltmalardan/örneklerden alınan olaylar
* Bölüm çoğaltma: `replicaId` ve `partitionId` tarafından tanımlanan belirli bir çoğaltmadan/örnekten gelen olaylar

API hakkında daha fazla bilgi edinmek için [Eventstore API başvurusunu](https://docs.microsoft.com/rest/api/servicefabric/sfclient-index-eventsstore)inceleyin.

EventStore hizmeti Ayrıca kümenizdeki olayları ilişkilendirebilme özelliğine sahiptir. Aynı anda birbirini etkilemiş olabilecek farklı varlıklardan yazılmış olaylara bakarak, EventStore hizmeti bu olayları, kümenizdeki etkinliklerin nedenlerini tanımlamaya yardımcı olmak için bağlayabilir. Örneğin, uygulamalarınızdan biri herhangi bir değişiklik yapılmadan sağlıksız hale gelirse, EventStore Ayrıca platform tarafından kullanıma sunulan diğer olaylara de bakar ve bunu bir `Error` veya `Warning` olayı ile ilişkilendirecektir. Bu, daha hızlı hata algılama ve kök nedenler analizine yardımcı olur.

## <a name="enable-eventstore-on-your-cluster"></a>Kümenizde EventStore 'u etkinleştirme

### <a name="local-cluster"></a>Yerel küme

[Kümenizdeki Fabricsettings. JSON](service-fabric-cluster-fabric-settings.md)dosyasında, EventStoreService ' i eklenti özelliği olarak ekleyin ve bir küme yükseltmesi gerçekleştirin.

```json
    "addOnFeatures": [
        "EventStoreService"
    ],
```

### <a name="azure-cluster-version-65"></a>Azure küme sürümü 6.5 +
Azure kümeniz sürüm 6,5 veya üzeri bir sürüme yükseltilirse, EventStore otomatik olarak kümenizde etkinleştirilir. Devre dışı bırakmak için, küme şablonunuzu aşağıdakiler ile güncelleştirmeniz gerekir:

* `2019-03-01` veya daha yeni bir API sürümü kullanın 
* Aşağıdaki kodu kümenizdeki Özellikler bölümüne ekleyin
  ```json  
    "fabricSettings": [
      …
    ],
    "eventStoreServiceEnabled": false
  ```

### <a name="azure-cluster-version-64"></a>Azure Cluster sürüm 6,4

6,4 sürümünü kullanıyorsanız, EventStore hizmetini açmak için Azure Resource Manager şablonunuzu düzenleyebilirsiniz. Bu, bir [küme yapılandırma yükseltmesi](service-fabric-cluster-config-upgrade-azure.md) gerçekleştirerek ve aşağıdaki kodu ekleyerek yapılır, eventstore hizmetinin çoğaltmalarını belirli bir NodeType (örneğin, sistem hizmetleri için ayrılmış bir NodeType) yerleştirmek Için Placementkýsýtlamalarını kullanabilirsiniz. `upgradeDescription` bölümü, düğümlerde yeniden başlatma tetiklenmesi için yapılandırma yükseltmesini yapılandırır. Başka bir güncelleştirmedeki bölümünü kaldırabilirsiniz.

```json
    "fabricSettings": [
          …
          …
          …,
         {
            "name": "EventStoreService",
            "parameters": [
              {
                "name": "TargetReplicaSetSize",
                "value": "3"
              },
              {
                "name": "MinReplicaSetSize",
                "value": "1"
              },
              {
                "name": "PlacementConstraints",
                "value": "(NodeType==<node_type_name_here>)"
              }
            ]
          }
        ],
        "upgradeDescription": {
          "forceRestart": true,
          "upgradeReplicaSetCheckTimeout": "10675199.02:48:05.4775807",
          "healthCheckWaitDuration": "00:01:00",
          "healthCheckStableDuration": "00:01:00",
          "healthCheckRetryTimeout": "00:5:00",
          "upgradeTimeout": "1:00:00",
          "upgradeDomainTimeout": "00:10:00",
          "healthPolicy": {
            "maxPercentUnhealthyNodes": 100,
            "maxPercentUnhealthyApplications": 100
          },
          "deltaHealthPolicy": {
            "maxPercentDeltaUnhealthyNodes": 0,
            "maxPercentUpgradeDomainDeltaUnhealthyNodes": 0,
            "maxPercentDeltaUnhealthyApplications": 0
          }
        }
```


## <a name="next-steps"></a>Sonraki adımlar
* [Azure Service Fabric kümelerinde EventStore API 'Lerini kullanarak](service-fabric-diagnostics-eventstore-query.md) EVENTSTORE API 'si ile çalışmaya başlama
* EventStore- [Service Fabric olayları](service-fabric-diagnostics-event-generation-operational.md) tarafından sunulan olayların listesi hakkında daha fazla bilgi edinin
* [Service Fabric için Service Fabric izleme ve tanılama](service-fabric-diagnostics-overview.md) 'da izleme ve tanılama 'ya genel bakış
* API çağrılarının tam listesini görüntüleyin- [Eventstore REST API başvurusu](https://docs.microsoft.com/rest/api/servicefabric/sfclient-index-eventsstore)
* Küme [ve platformu](service-fabric-diagnostics-event-generation-infra.md)izleme hakkında daha fazla bilgi edinin.
