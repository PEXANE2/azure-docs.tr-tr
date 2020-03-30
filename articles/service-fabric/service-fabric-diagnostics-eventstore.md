---
title: Azure Hizmet Kumaş Etkinlik Mağazası
description: Bir kümenin durumunu veya iş yüklerini her zaman anlamanın ve izlemenin bir yolu olan Azure Hizmet Kumaşı'nın EventStore'u hakkında bilgi edinin.
author: srrengar
ms.topic: conceptual
ms.date: 6/6/2019
ms.author: srrengar
ms.openlocfilehash: d23c8114bf10ef3225775accef6910c0ba539e15
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75645744"
---
# <a name="eventstore-overview"></a>EventStore Genel Bakış

>[!NOTE]
>Hizmet Kumaş sürüm 6.4 itibariyle. EventStore API'leri yalnızca Azure'da çalışan Windows kümeleri için kullanılabilir. Bu işlevselliği Linux'a ve Bağımsız kümelerimize taşımaya çalışıyoruz.

## <a name="overview"></a>Genel Bakış

Sürüm 6.2'de tanıtılan EventStore hizmeti, Service Fabric'te bir izleme seçeneğidir. EventStore, kümenizin durumunu veya iş yüklerinizi belirli bir zamanda anlamanın bir yolunu sağlar.
EventStore, kümedeki olayları koruyan, devlete hizmet veren bir Hizmet Kumaşı hizmetidir. Olay Service Fabric Explorer, REST ve API'ler aracılığıyla ortaya çıkarır. EventStore kümenizdeki herhangi bir varlık üzerinde tanılama verileri almak için doğrudan kümesorgular ve yardımcı olmak için kullanılmalıdır:

* Geliştirme veya sınama sorunları veya izleme ardışık bir denetim hattı kullanıyor olabileceğiniz sorunları tanılama
* Kümenizde gerçekleştirdiğiniz yönetim eylemlerinin doğru şekilde işlendiğini doğrulayın
* Service Fabric'in belirli bir varlıkla nasıl etkileşimde bulunduklarının bir "anlık görüntüsünü" alın

![EventStore](media/service-fabric-diagnostics-eventstore/eventstore.png)

EventStore'da bulunan olayların tam listesini görmek için [Service Fabric etkinliklerine](service-fabric-diagnostics-event-generation-operational.md)bakın.

>[!NOTE]
>Hizmet Kumaş sürüm 6.4 itibariyle. EventStore API'leri ve UX genellikle Azure Windows kümeleri için kullanılabilir. Bu işlevselliği Linux'a ve Bağımsız kümelerimize taşımaya çalışıyoruz.

EventStore hizmeti, kümenizdeki her varlık ve varlık türü için kullanılabilen olaylar için sorgulanabilir. Bu, aşağıdaki düzeylerdeki olayları sorgulayabileceğiniz anlamına gelir:
* Küme: kümenin kendisine özgü olaylar (örn. küme yükseltmesi)
* Düğümler: tüm düğüm düzeyi olayları
* Düğüm: bir düğüme özgü olaylar,`nodeName`
* Uygulamalar: tüm uygulama düzeyi olayları
* Uygulama: bir uygulamaya özgü olaylar tarafından tanımlanan`applicationId`
* Hizmetler: kümelerinizdeki tüm hizmetlerden etkinlikler
* Hizmet: belirli bir hizmetten olaylar tarafından tanımlanan`serviceId`
* Bölümler: tüm bölümlerden olaylar
* Bölüm: belirli bir bölümden olaylar tarafından tanımlanan`partitionId`
* Partition Replicas: belirli bir bölüm içinde tüm yinelemeler / örnekleri olaylar tarafından tanımlanan`partitionId`
* Partition Replica: belirli bir yineleme / `replicaId` örnek olaylar tarafından tanımlanan ve`partitionId`

API hakkında daha fazla bilgi edinmek için [EventStore API başvurusuna](https://docs.microsoft.com/rest/api/servicefabric/sfclient-index-eventsstore)göz atın.

EventStore hizmeti, kümenizdeki olayları ilişkilendirme özelliğine de sahiptir. EventStore hizmeti, birbirini etkileyebilecek farklı varlıklardan aynı anda yazılmış olaylara bakarak, kümenizdeki etkinliklerin nedenlerini belirlemeye yardımcı olmak için bu olayları bağlayabilir. Örneğin, uygulamalarınızdan biri herhangi bir değişiklik yapılmadan sağlıksız hale gelirse, EventStore platformun maruz kaçtığı diğer olaylara `Error` da `Warning` bakar ve bunu bir veya olayla ilişkilendirebilir. Bu daha hızlı hata algılama ve kök neden analizi ile yardımcı olur.

## <a name="enable-eventstore-on-your-cluster"></a>Cluster'ınızda EventStore'u etkinleştirin

### <a name="local-cluster"></a>Yerel Küme

[Cluster'ınızdaki fabricSettings.json'da,](service-fabric-cluster-fabric-settings.md)addOn özelliği olarak EventStoreService'i ekleyin ve küme yükseltmesi yapın.

```json
    "addOnFeatures": [
        "EventStoreService"
    ],
```

### <a name="azure-cluster-version-65"></a>Azure küme sürümü 6.5+
Azure kümeniz sürüm 6.5 veya daha yüksek yükseltilirse, EventStore kümenizde otomatik olarak etkinleştirilir. Devre dışı bırakmak için küme şablonunuzu aşağıdakilerle güncelleştirmeniz gerekir:

* Bir API sürümü `2019-03-01` veya daha yeni kullanma 
* Kümenizdeki özellikler bölümüne aşağıdaki kodu ekleyin
  ```json  
    "fabricSettings": [
      …
    ],
    "eventStoreServiceEnabled": false
  ```

### <a name="azure-cluster-version-64"></a>Azure küme sürümü 6.4

Sürüm 6.4 kullanıyorsanız, Azure Kaynak Yöneticisi şablonunuzu EventStore hizmetini açmak için ayarlayabilirsiniz. Bu bir küme [config yükseltme](service-fabric-cluster-config-upgrade-azure.md) gerçekleştirerek ve aşağıdaki kodu ekleyerek yapılır, belirli bir NodeType örneğin sistem hizmetleri için özel bir NodeType EventStore hizmetinin kopyalarını koymak için PlacementKısıtlamalar kullanabilirsiniz. Bölüm, `upgradeDescription` düğümlerde yeniden başlatmayı tetiklemek için config yükseltmesini yapılandırır. Başka bir güncelleştirmedeki bölümü kaldırabilirsiniz.

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
* EventStore API ile başlayın - [Azure Hizmet Kumaşı kümelerinde EventStore API'lerini kullanma](service-fabric-diagnostics-eventstore-query.md)
* EventStore - [Service Fabric etkinlikleri](service-fabric-diagnostics-event-generation-operational.md) tarafından sunulan etkinlikler listesi hakkında daha fazla bilgi edinin
* Hizmet Kumaşında İzleme ve Tanılama Genel Bakış - [Hizmet Kumaşı için İzleme ve Tanılama](service-fabric-diagnostics-overview.md)
* API çağrılarının tam listesini görüntüleyin - [EventStore REST API Referans](https://docs.microsoft.com/rest/api/servicefabric/sfclient-index-eventsstore)
* Kümenizi izleme hakkında daha fazla bilgi edinin - [Küme ve platformu izleme](service-fabric-diagnostics-event-generation-infra.md).
