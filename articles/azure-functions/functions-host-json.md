---
title: Azure Işlevleri 2. x için Host. JSON başvurusu
description: V2 çalışma zamanına sahip Azure Işlevleri Host. JSON dosyası için başvuru belgeleri.
services: functions
author: ggailey777
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.topic: conceptual
ms.date: 09/08/2018
ms.author: glenga
ms.openlocfilehash: 3ae75dc988ad70871efa45eb8c61db15804922ee
ms.sourcegitcommit: 42748f80351b336b7a5b6335786096da49febf6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72176575"
---
# <a name="hostjson-reference-for-azure-functions-2x"></a>Azure Işlevleri 2. x için Host. JSON başvurusu  

> [!div class="op_single_selector" title1="Kullanmakta olduğunuz Azure Işlevleri çalışma zamanının sürümünü seçin: "]
> * [Sürüm 1](functions-host-json-v1.md)
> * [Sürüm 2](functions-host-json.md)

*Host. JSON* meta veri dosyası, bir işlev uygulaması için tüm işlevleri etkileyen genel yapılandırma seçeneklerini içerir. Bu makalede v2 çalışma zamanı için kullanılabilen ayarlar listelenir.  

> [!NOTE]
> Bu makale, Azure Işlevleri 2. x içindir.  1\. x Işlevleri içindeki Host. JSON başvurusu için bkz. [Azure işlevleri için Host. JSON başvurusu 1. x](functions-host-json-v1.md).

Diğer işlev uygulaması yapılandırma seçenekleri [uygulama ayarlarınızda](functions-app-settings.md)yönetilir.

Bazı Host. JSON ayarları yalnızca [yerel. Settings. JSON](functions-run-local.md#local-settings-file) dosyasında yerel olarak çalıştırılırken kullanılır.

## <a name="sample-hostjson-file"></a>Örnek Host. JSON dosyası

Aşağıdaki örnek *Host. JSON* dosyaları tüm olası seçenekleri belirtti.

```json
{
    "version": "2.0",
    "aggregator": {
        "batchSize": 1000,
        "flushTimeout": "00:00:30"
    },
    "extensions": {
        "cosmosDb": {},
        "durableTask": {},
        "eventHubs": {},
        "http": {},
        "queues": {},
        "sendGrid": {},
        "serviceBus": {}
    },
    "functions": [ "QueueProcessor", "GitHubWebHook" ],
    "functionTimeout": "00:05:00",
    "healthMonitor": {
        "enabled": true,
        "healthCheckInterval": "00:00:10",
        "healthCheckWindow": "00:02:00",
        "healthCheckThreshold": 6,
        "counterThreshold": 0.80
    },
    "logging": {
        "fileLoggingMode": "debugOnly",
        "logLevel": {
          "Function.MyFunction": "Information",
          "default": "None"
        },
        "applicationInsights": {
            "samplingSettings": {
              "isEnabled": true,
              "maxTelemetryItemsPerSecond" : 5
            }
        }
    },
    "singleton": {
      "lockPeriod": "00:00:15",
      "listenerLockPeriod": "00:01:00",
      "listenerLockRecoveryPollingInterval": "00:01:00",
      "lockAcquisitionTimeout": "00:01:00",
      "lockAcquisitionPollingInterval": "00:00:03"
    },
    "watchDirectories": [ "Shared", "Test" ],
    "managedDependency": {
        "enabled": true
    }
}
```

Bu makalenin aşağıdaki bölümlerinde her üst düzey özellik açıklanmaktadır. Aksi belirtilmedikçe tümü isteğe bağlıdır.

## <a name="aggregator"></a>'yı

[!INCLUDE [aggregator](../../includes/functions-host-json-aggregator.md)]

## <a name="applicationinsights"></a>ApplicationInsights

Bu ayar [günlüğe kaydetme](#logging)işleminin bir alt öğesidir.

[Application Insights 'de örnekleme özelliğini](./functions-monitoring.md#configure-sampling)denetler.

```json
{
    "applicationInsights": {
        "samplingSettings": {
          "isEnabled": true,
          "maxTelemetryItemsPerSecond" : 5
        }
    }
}
```

> [!NOTE]
> Günlük örnekleme, bazı yürütmelerin Application Insights İzleyicisi dikey penceresinde gösterilmemesine neden olabilir.

|Özellik  |Varsayılanını | Description |
|---------|---------|---------| 
|IsEnabled|true|Örneklemeyi etkinleştirilir veya devre dışı bırakır.| 
|maxTelemetryItemsPerSecond|5|Örneklemenin başladığı eşik.| 
|Enableliveölçümleri |true|Canlı ölçüm toplamayı etkin bir şekilde sunar.|
|EnableDependencyTracking|true|Bağımlılık izlemeyi etkinleştirilir.|
|EnablePerformanceCountersCollection|true|Kudu performans sayaçlarını toplamayı etkinleştirilir.|

## <a name="cosmosdb"></a>cosmosDb

Yapılandırma ayarı, [Cosmos DB Tetikleyiciler ve bağlamalarda](functions-bindings-cosmosdb-v2.md#host-json)bulunabilir.

## <a name="durabletask"></a>durableTask

Yapılandırma ayarı, [dayanıklı işlevler bağlamalarında](durable/durable-functions-bindings.md#host-json)bulunabilir.

## <a name="eventhub"></a>eventHub

Yapılandırma ayarları, [Olay Hub 'ı Tetikleyicileri ve bağlamaları](functions-bindings-event-hubs.md#host-json)' nda bulunabilir. 

## <a name="extensions"></a>Uzantılardan

[Http](#http) ve [eventHub](#eventhub)gibi bağlamaya özgü tüm ayarları içeren bir nesne döndüren özellik.

## <a name="functions"></a>lerdir

İş konağının çalıştığı işlevlerin listesi. Boş bir dizi tüm işlevleri Çalıştır anlamına gelir. Yalnızca [yerel olarak çalışırken](functions-run-local.md)kullanılmak üzere tasarlanmıştır. Azure 'daki işlev uygulamaları ' nda, bu ayarı kullanmak yerine belirli işlevleri devre dışı bırakmak için [Azure işlevlerinde işlevleri devre dışı](disable-function.md) bırakma bölümündeki adımları izlemeniz gerekir.

```json
{
    "functions": [ "QueueProcessor", "GitHubWebHook" ]
}
```

## <a name="functiontimeout"></a>functionTimeout

Tüm işlevler için zaman aşımı süresini gösterir. TimeSpan dize biçimini izler. Sunucusuz tüketim planında geçerli Aralık 1 saniye ila 10 dakika ve varsayılan değer 5 dakikadır.  
Adanmış bir (App Service) planında, genel bir sınır yoktur ve varsayılan değer çalışma zamanı sürümüne bağlıdır: 
+ Sürüm 1. x: varsayılan değer, zaman aşımı olmadığını gösteren *null*.   
+ Sürüm 2. x: varsayılan değer 30 dakikadır. @No__t-0 değeri, sınırsız yürütmeyi gösterir.

```json
{
    "functionTimeout": "00:05:00"
}
```

## <a name="healthmonitor"></a>healthMonitor

[Konak sistem durumu izleyicisinin](https://github.com/Azure/azure-webjobs-sdk-script/wiki/Host-Health-Monitor)yapılandırma ayarları.

```
{
    "healthMonitor": {
        "enabled": true,
        "healthCheckInterval": "00:00:10",
        "healthCheckWindow": "00:02:00",
        "healthCheckThreshold": 6,
        "counterThreshold": 0.80
    }
}
```

|Özellik  |Varsayılanını | Description |
|---------|---------|---------| 
|etkinletir|true|Özelliğin etkinleştirilip etkinleştirilmeyeceğini belirtir. | 
|Healthcheckınterval|10 saniye|Düzenli arka plan sistem durumu denetimleri arasındaki zaman aralığı. | 
|healthCheckWindow|2 dakika|@No__t-0 ayarıyla birlikte kullanılan bir kayan zaman penceresi.| 
|healthCheckThreshold|6|Konak geri dönüşüm başlatılmadan önce sistem durumu denetiminin başarısız olması için en fazla sayı.| 
|Onay eşiği|0,80|Performans sayacının sağlıksız olduğu kabul edilecek eşik.| 

## <a name="http"></a>http

Yapılandırma ayarları, [http Tetikleyicileri ve bağlamaları](functions-bindings-http-webhook.md)içinde bulunabilir.

[!INCLUDE [functions-host-json-http](../../includes/functions-host-json-http.md)]

## <a name="logging"></a>Açmak

Application Insights dahil olmak üzere, işlev uygulamasının günlük davranışlarını denetler.

```json
"logging": {
    "fileLoggingMode": "debugOnly",
    "logLevel": {
      "Function.MyFunction": "Information",
      "default": "None"
    },
    "console": {
        ...
    },
    "applicationInsights": {
        ...
    }
}
```

|Özellik  |Varsayılanını | Description |
|---------|---------|---------|
|fileLoggingMode|yalnızca Debug|Hangi dosya günlüğü düzeyinin etkin olduğunu tanımlar.  Seçenekler `never`, `always`, `debugOnly` ' dir. |
|logLevel|yok|Uygulamadaki işlevler için günlük kategorisi filtrelemeyi tanımlayan nesne. Sürüm 2. x, günlük kategorisi filtrelemesinin ASP.NET Core yerleşimini izler. Bu, belirli işlevler için günlüğü filtrelemenizi sağlar. Daha fazla bilgi için ASP.NET Core belgelerine [günlük filtreleme](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.1#log-filtering) bölümüne bakın. |
|Konsola|yok| [Konsol](#console) günlüğü ayarı. |
|ApplicationInsights|yok| [ApplicationInsights](#applicationinsights) ayarı. |

## <a name="console"></a>Konsola

Bu ayar [günlüğe kaydetme](#logging)işleminin bir alt öğesidir. Hata ayıklama modunda olmadığında konsol günlüğünü denetler.

```json
{
    "logging": {
    ...
        "console": {
          "isEnabled": "false"
        },
    ...
    }
}
```

|Özellik  |Varsayılanını | Description |
|---------|---------|---------| 
|IsEnabled|yanlış|Konsol günlüğünü etkinleştir veya devre dışı bırakır.| 

## <a name="queues"></a>klarında

Yapılandırma ayarları, [depolama kuyruğu Tetikleyicileri ve bağlamaları](functions-bindings-storage-queue.md#host-json)bölümünde bulunabilir.  

## <a name="sendgrid"></a>sendGrid

Yapılandırma ayarı, [SendGrid Tetikleyicileri ve bağlamaları](functions-bindings-sendgrid.md#host-json)içinde bulunabilir.

## <a name="servicebus"></a>serviceBus

Yapılandırma ayarı, [Service Bus Tetikleyiciler ve bağlamalarda](functions-bindings-service-bus.md#host-json)bulunabilir.

## <a name="singleton"></a>adet

Tek kilit davranışı için yapılandırma ayarları. Daha fazla bilgi için bkz. [Singleton desteği hakkında GitHub sorunu](https://github.com/Azure/azure-webjobs-sdk-script/issues/912).

```json
{
    "singleton": {
      "lockPeriod": "00:00:15",
      "listenerLockPeriod": "00:01:00",
      "listenerLockRecoveryPollingInterval": "00:01:00",
      "lockAcquisitionTimeout": "00:01:00",
      "lockAcquisitionPollingInterval": "00:00:03"
    }
}
```

|Özellik  |Varsayılanını | Description |
|---------|---------|---------| 
|Kilit dönemi|00:00:15|İşlev düzeyi kilitlerinin alındığı dönem için. Kilitleri otomatik yenileme.| 
|listenerLockPeriod|00:01:00|Dinleyici kilitlerinin alındığı dönem.| 
|Listenerlockrecoverypollingınterval|00:01:00|Başlangıçta dinleyici kilidi alınamadığından, dinleyici kilidi kurtarma için kullanılan zaman aralığı.| 
|Locktanışılationtimeout|00:01:00|Çalışma zamanının kilit edinmeye çalışacak en uzun süre.| 
|Locktanışmalationpollingınterval|yok|Kilit alma denemeleri arasındaki Aralık.| 

## <a name="version"></a>sürüm

V2 çalışma zamanını hedefleyen bir işlev uygulaması için `"version": "2.0"` sürüm dizesi gereklidir.

## <a name="watchdirectories"></a>watchDirectories

Değişiklikler için izlenmesi gereken bir [paylaşılan kod dizinleri](functions-reference-csharp.md#watched-directories) kümesi.  Bu dizinlerdeki kod değiştirildiğinde, değişikliklerin işlevleriniz tarafından çekilmesini sağlar.

```json
{
    "watchDirectories": [ "Shared" ]
}
```

## <a name="manageddependency"></a>managedDependency bağımlılığı

Yönetilen bağımlılık Şu anda yalnızca PowerShell tabanlı işlevlerde desteklenen bir önizleme özelliğidir. Bağımlılıkların hizmet tarafından otomatik olarak yönetilmesine olanak sağlar. Enabled özelliği true olarak ayarlandığında, [requirements. psd1](functions-reference-powershell.md#dependency-management) dosyası işlenir. Herhangi bir ikincil sürüm bırakıldığında bağımlılıklar güncelleştirilecektir.

```json
{
    "managedDependency": {
        "enabled": true
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Host. json dosyasını güncelleştirme hakkında bilgi edinin](functions-reference.md#fileupdate)

> [!div class="nextstepaction"]
> [Ortam değişkenlerinde genel ayarları gör](functions-app-settings.md)
