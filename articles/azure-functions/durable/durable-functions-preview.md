---
title: Dayanıklı İşlevler Önizleme özellikleri-Azure Işlevleri
description: Dayanıklı İşlevler için Önizleme özellikleri hakkında bilgi edinin.
services: functions
author: cgillum
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.topic: article
ms.date: 09/04/2019
ms.author: azfuncdf
ms.openlocfilehash: 8f2560141eac4e7d9fed267d347990e65bfe9c26
ms.sourcegitcommit: f3f4ec75b74124c2b4e827c29b49ae6b94adbbb7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2019
ms.locfileid: "70933237"
---
# <a name="durable-functions-20-preview-azure-functions"></a>Dayanıklı İşlevler 2,0 Preview (Azure Işlevleri)

*Dayanıklı işlevler* , [Azure Işlevleri](../functions-overview.md) ve [Azure Web işleri](../../app-service/web-sites-create-web-jobs.md) 'nin bir uzantısıdır ve bu da sunucusuz bir ortamda durum bilgisi olan işlevler yazmanızı sağlar. Uzantı sizin için durumu, denetim noktalarını ve yeniden başlatmaları yönetir. Daha önce Dayanıklı İşlevler hakkında bilginiz yoksa [genel bakış belgelerine](durable-functions-overview.md)bakın.

Dayanıklı İşlevler 1. x, Azure Işlevlerinin bir GA (genel kullanıma sunuldu) özelliğidir, ancak şu anda genel önizleme aşamasında olan çeşitli alt özellikleri de içerir. Bu makalede yeni yayınlanan Önizleme özellikleri açıklanmakta ve bunların nasıl çalıştığı ve bunları nasıl kullanabileceğiniz hakkında ayrıntılı bilgiler yer alır.

> [!NOTE]
> Bu Önizleme özellikleri, şu anda birçok önemli değişiklik içeren bir **Önizleme kalitesi sürümü** olan dayanıklı işlevler 2,0 sürümünün bir parçasıdır. Azure Işlevleri dayanıklı Uzantı paketi yapıları, **2.0.0-betaX**biçimindeki sürümlerle NuGet.org üzerinde bulunabilir. Bu derlemeler üretim iş yükleri için tasarlanmamıştır ve sonraki sürümlerde ek son değişiklikler olabilir.

## <a name="breaking-changes"></a>Yeni değişiklikler

Dayanıklı İşlevler 2,0 ' de birçok önemli değişiklik sunulmuştur. Mevcut uygulamaların kod değişikliği olmadan Dayanıklı İşlevler 2,0 ile uyumlu olması beklenmez. Bu bölümde bazı değişiklikler listelenir:

### <a name="hostjson-schema"></a>Host. JSON şeması

Aşağıdaki kod parçacığında Host. JSON için yeni şema gösterilmektedir. Dikkat edilecek ana değişiklikler yeni alt kısımlar:

* `"storageProvider"`depolama birimine özgü `"azureStorage"` yapılandırma için (ve alt bölüm)
* `"tracking"`izleme ve günlüğe kaydetme yapılandırması için
* `"notifications"`Event Grid bildirim `"eventGrid"` yapılandırması için (ve alt bölüm)

```json
{
  "version": "2.0",
  "extensions": {
    "durableTask": {
      "hubName": <string>,
      "storageProvider": {
        "azureStorage": {
          "connectionStringName": <string>,
          "controlQueueBatchSize": <int?>,
          "partitionCount": <int?>,
          "controlQueueVisibilityTimeout": <hh:mm:ss?>,
          "workItemQueueVisibilityTimeout": <hh:mm:ss?>,
          "trackingStoreConnectionStringName": <string?>,
          "trackingStoreNamePrefix": <string?>,
          "maxQueuePollingInterval": <hh:mm:ss?>
        }
      },
      "tracking": {
        "traceInputsAndOutputs": <bool?>,
        "traceReplayEvents": <bool?>,
      },
      "notifications": {
        "eventGrid": {
          "topicEndpoint": <string?>,
          "keySettingName": <string?>,
          "publishRetryCount": <string?>,
          "publishRetryInterval": <hh:mm:ss?>,
          "publishRetryHttpStatus": <int[]?>,
          "publishEventTypes": <string[]?>,
        }
      },
      "maxConcurrentActivityFunctions": <int?>,
      "maxConcurrentOrchestratorFunctions": <int?>,
      "extendedSessionsEnabled": <bool?>,
      "extendedSessionIdleTimeoutInSeconds": <int?>,
      "customLifeCycleNotificationHelperType": <string?>
  }
}
```

Dayanıklı işlevler 2,0 kararlı olmaya devam ettiğinden, Host. JSON `durableTask` bölümüne daha fazla değişiklik sunulacaktır. Bu değişiklikler hakkında daha fazla bilgi için [Bu GitHub sorununa](https://github.com/Azure/azure-functions-durable-extension/issues/641)bakın.

### <a name="public-interface-changes"></a>Ortak arabirim değişiklikleri

Dayanıklı İşlevler tarafından desteklenen çeşitli "bağlam" nesneleri, birim testinde kullanılmak üzere tasarlanan soyut temel sınıflara sahipti. Dayanıklı İşlevler 2,0 ' nın bir parçası olarak bu soyut temel sınıflar, arabirimlerle değiştirilmiştir. Somut türleri doğrudan kullanan işlev kodu bundan etkilenmez.

Aşağıdaki tablo, ana değişiklikleri temsil eder:

| Eski tür | Yeni tür |
|----------|----------|
| DurableOrchestrationClientBase | Idurableorchestrationclient |
| DurableOrchestrationContextBase | IDurableOrchestrationContext |
| DurableActivityContextBase | Idurableactivitycontext |
| OrchestrationClientAttribute | DurableClientAttribute |

Soyut bir temel sınıfın sanal yöntemler içerdiği durumlarda, bu sanal yöntemler ' de `DurableContextExtensions`tanımlanan genişletme yöntemleriyle değiştirilmiştir.

## <a name="entity-functions"></a>Varlık işlevleri

Dayanıklı İşlevler v 2.0.0-Alpha ' d a başlayarak yeni bir [varlık işlevleri](durable-functions-entities.md) kavramı ekledik.

Varlık işlevleri, *dayanıklı varlıklar*olarak bilinen küçük durum parçalarını okumak ve güncelleştirmek için işlemleri tanımlar. Orchestrator işlevleri gibi, varlık işlevleri de özel tetikleyici türü, *varlık tetikleyicisi*olan işlevlerdir. Orchestrator işlevlerinin aksine, varlık işlevlerinin belirli kod kısıtlamaları yoktur. Varlık işlevleri, durumu denetim akışı aracılığıyla örtük olarak temsil etmek yerine, durumu açıkça da yönetir.

İlk Kullanıcı geri bildirimlerine göre, daha sonra Dayanıklı İşlevler v 2.0.0-Beta1 içindeki varlıklar için sınıf tabanlı bir programlama modeli desteği ekledik.

Daha fazla bilgi için bkz. [varlık işlevleri](durable-functions-entities.md) makalesi.

## <a name="durable-http"></a>Kalıcı HTTP

Dayanıklı İşlevler v 2.0.0-Beta2 'den başlayarak şunları yapmanıza olanak sağlayan yeni bir [DAYANıKLı http](durable-functions-http-features.md) özelliği sunuyoruz:

* HTTP API 'Lerini doğrudan düzenleme işlevlerinden çağırma (bazı belgelenen sınırlamalar ile)
* Otomatik istemci tarafı HTTP 202 durum yoklamasını uygular
* [Azure Yönetilen kimlikler](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview) için yerleşik destek

Daha fazla bilgi için bkz. [http özellikleri](durable-functions-http-features.md#consuming-http-apis) makalesi.

## <a name="alternate-storage-providers"></a>Alternatif depolama sağlayıcıları

Dayanıklı görev çerçevesi, [Azure depolama](https://github.com/Azure/durabletask/tree/master/src/DurableTask.AzureStorage), [Azure Service Bus](https://github.com/Azure/durabletask/tree/master/src/DurableTask.ServiceBus), [bellek Içi öykünücü](https://github.com/Azure/durabletask/tree/master/src/DurableTask.Emulator)ve deneysel [redin](https://github.com/Azure/durabletask/tree/redis/src/DurableTask.Redis) sağlayıcısı dahil olmak üzere bugün birden çok depolama sağlayıcısını destekler. Ancak, şu anda Azure Işlevleri için dayanıklı görev uzantısı yalnızca Azure Storage sağlayıcısını destekliyordu. Dayanıklı İşlevler 2,0 ' den başlayarak, redin sağlayıcısı ile başlayarak alternatif depolama sağlayıcıları için destek ekleniyor.

> [!NOTE]
> Dayanıklı İşlevler 2,0 yalnızca .NET Standard 2,0 ile uyumlu sağlayıcıları destekler.

### <a name="emulator"></a>Benzeti

[Durabletask. öykünücü](https://www.nuget.org/packages/Microsoft.Azure.DurableTask.Emulator/) sağlayıcısı yerel bir bellek, yerel test senaryoları için uygun, dayanıklı olmayan bir depolama sağlayıcıdır. Aşağıdaki en az **Host. JSON** şeması kullanılarak yapılandırılabilir:

```json
{
  "version": "2.0",
  "extensions": {
    "durableTask": {
      "hubName": <string>,
      "storageProvider": {
        "emulator": { "enabled": true }
      }
    }
  }
}
```

### <a name="redis-experimental"></a>Reddir (deneysel)

[Durabletask. reddir](https://www.nuget.org/packages/Microsoft.Azure.DurableTask.Redis/) sağlayıcısı, tüm düzenleme durumlarını yapılandırılmış bir redsıs kümesine devam ettirir.

```json
{
  "version": "2.0",
  "extensions": {
    "durableTask": {
      "hubName": <string>,
      "storageProvider": {
        "redis": {
          "connectionStringName": <string>,
        }
      }
    }
  }
}
```

, `connectionStringName` Bir uygulama ayarı veya ortam değişkeni adına başvurmalıdır. Bu uygulama ayarı veya ortam değişkeni, *sunucu: bağlantı noktası*biçiminde bir redsıs bağlantı dizesi değeri içermelidir. Örneğin, `localhost:6379` yerel bir redto kümesine bağlanmak için.

> [!NOTE]
> Redsıs sağlayıcısı şu anda deneysel ve yalnızca tek bir düğümde çalışan işlev uygulamalarını destekliyor. Redin sağlayıcısının genel kullanıma açık hale getirilme garantisi yoktur ve sonraki bir sürümde kaldırılabilir.
