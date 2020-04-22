---
title: Azure İşlevler için host.json başvurusu 1.x
description: V1 çalışma zamanı ile Azure İşlemeleri host.json dosyası için başvuru belgeleri.
ms.topic: conceptual
ms.date: 10/19/2018
ms.openlocfilehash: 36d028d09c94ae28e77404297bd576f5e20404c6
ms.sourcegitcommit: 31e9f369e5ff4dd4dda6cf05edf71046b33164d3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81757524"
---
# <a name="hostjson-reference-for-azure-functions-1x"></a>Azure İşlevler için host.json başvurusu 1.x

> [!div class="op_single_selector" title1="Kullandığınız Azure İşlevleri çalışma zamanı sürümünü seçin: "]
> * [Sürüm 1](functions-host-json-v1.md)
> * [Sürüm 2](functions-host-json.md)

*Host.json* meta veri dosyası, bir işlev uygulamasının tüm işlevlerini etkileyen genel yapılandırma seçenekleri içerir. Bu makalede, v1 çalışma zamanı için kullanılabilir ayarları listeler. JSON http://json.schemastore.org/hostşeması.

> [!NOTE]
> Bu makale Azure İşlevler 1.x içindir.  Functions 2.x ve sonraki alanlarda host.json başvurusu [için Azure İşlevleri 2.x için host.json başvurusuna](functions-host-json.md)bakın.

Diğer işlev uygulaması yapılandırma seçenekleri [uygulama ayarlarınızda](functions-app-settings.md)yönetilir.

Bazı host.json ayarları yalnızca [local.settings.json](functions-run-local.md#local-settings-file) dosyasında yerel olarak çalışırken kullanılır.

## <a name="sample-hostjson-file"></a>Örnek host.json dosyası

Aşağıdaki örnek *host.json* dosyaları belirtilen tüm olası seçenekleri var.


```json
{
    "aggregator": {
        "batchSize": 1000,
        "flushTimeout": "00:00:30"
    },
    "applicationInsights": {
        "sampling": {
          "isEnabled": true,
          "maxTelemetryItemsPerSecond" : 5
        }
    },
    "documentDB": {
        "connectionMode": "Gateway",
        "protocol": "Https",
        "leaseOptions": {
            "leasePrefix": "prefix"
        }
    },
    "eventHub": {
      "maxBatchSize": 64,
      "prefetchCount": 256,
      "batchCheckpointFrequency": 1
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
    "http": {
        "routePrefix": "api",
        "maxOutstandingRequests": 20,
        "maxConcurrentRequests": 10,
        "dynamicThrottlesEnabled": false
    },
    "id": "9f4ea53c5136457d883d685e57164f08",
    "logger": {
        "categoryFilter": {
            "defaultLevel": "Information",
            "categoryLevels": {
                "Host": "Error",
                "Function": "Error",
                "Host.Aggregator": "Information"
            }
        }
    },
    "queues": {
      "maxPollingInterval": 2000,
      "visibilityTimeout" : "00:00:30",
      "batchSize": 16,
      "maxDequeueCount": 5,
      "newBatchThreshold": 8
    },
    "sendGrid": {
        "from": "Contoso Group <admin@contoso.com>"
    },
    "serviceBus": {
      "maxConcurrentCalls": 16,
      "prefetchCount": 100,
      "autoRenewTimeout": "00:05:00"
    },
    "singleton": {
      "lockPeriod": "00:00:15",
      "listenerLockPeriod": "00:01:00",
      "listenerLockRecoveryPollingInterval": "00:01:00",
      "lockAcquisitionTimeout": "00:01:00",
      "lockAcquisitionPollingInterval": "00:00:03"
    },
    "tracing": {
      "consoleLevel": "verbose",
      "fileLoggingMode": "debugOnly"
    },
    "watchDirectories": [ "Shared" ],
}
```

Bu makalenin aşağıdaki bölümleri her üst düzey özelliği açıklar. Aksi belirtilmedikçe hepsi isteğe bağlıdır.

## <a name="aggregator"></a>Toplayıcı

[!INCLUDE [aggregator](../../includes/functions-host-json-aggregator.md)]

## <a name="applicationinsights"></a>uygulamaInsights

[!INCLUDE [applicationInsights](../../includes/functions-host-json-applicationinsights.md)]

## <a name="documentdb"></a>DocumentDB

[Azure Cosmos DB tetikleyicisi ve bağlamaları](functions-bindings-cosmosdb.md)için yapılandırma ayarları.

```json
{
    "documentDB": {
        "connectionMode": "Gateway",
        "protocol": "Https",
        "leaseOptions": {
            "leasePrefix": "prefix1"
        }
    }
}
```

|Özellik  |Varsayılan | Açıklama |
|---------|---------|---------|
|Ağ Geçidi Modu|Ağ geçidi|Azure Cosmos DB hizmetine bağlanırken işlev tarafından kullanılan bağlantı modu. Seçenekler `Direct` ve`Gateway`|
|Protokol|Https://tr.,|Azure Cosmos DB hizmetine bağlantı kurarken işlev tarafından kullanılan bağlantı protokolü.  [Her iki modun bir açıklama için burayı](../cosmos-db/performance-tips.md#networking) okuyun|
|kiralamaÖnek|yok|Bir uygulamadaki tüm işlevlerde kullanılacak önkekleri kiralayın.|

## <a name="durabletask"></a>durableTask

[!INCLUDE [durabletask](../../includes/functions-host-json-durabletask.md)]

## <a name="eventhub"></a>eventHub

Olay Hub'ı için yapılandırma ayarları [tetikleyiciler ve bağlamalar.](functions-bindings-event-hubs-trigger.md#functions-1x)

## <a name="functions"></a> işlevleri

İş ana bilgisayarının çalıştırdığı işlevlerin listesi. Boş bir dizi tüm işlevleri çalıştırmak anlamına gelir. Yalnızca [yerel olarak çalışırken](functions-run-local.md)kullanılmak üzere tasarlanmıştır. Azure'daki işlev uygulamalarında, bunun yerine, bu ayarı kullanmak yerine belirli işlevleri devre dışı bırakarak [Azure İşlevlerinde işlevleri devre dışı bırakabilme](disable-function.md) adımlarını izlemeniz gerekir.

```json
{
    "functions": [ "QueueProcessor", "GitHubWebHook" ]
}
```

## <a name="functiontimeout"></a>fonksiyonTimeout

Tüm işlevler için zaman adabı süresini gösterir. Sunucusuz Tüketim planında geçerli aralık 1 saniye ile 10 dakika arasındadır ve varsayılan değer 5 dakikadır. Bir Uygulama Hizmeti planında, genel bir sınır yoktur ve varsayılan değer _geçersizdir,_ bu da zaman alameti olmadığını gösterir.

```json
{
    "functionTimeout": "00:05:00"
}
```

## <a name="healthmonitor"></a>healthMonitor

[Ana bilgisayar durumu monitörü](https://github.com/Azure/azure-webjobs-sdk-script/wiki/Host-Health-Monitor)için yapılandırma ayarları.

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

|Özellik  |Varsayılan | Açıklama |
|---------|---------|---------| 
|enabled|true|Özelliğin etkin olup olmadığını belirtir. | 
|sağlıkCheckInterval|10 saniye|Periyodik arka plan sağlık kontrolleri arasındaki zaman aralığı. | 
|sağlıkCheckWindow|2 dakika|`healthCheckThreshold` Ayarı ile birlikte kullanılan bir sürgülü zaman penceresi.| 
|sağlıkCheckThreshold|6|Ana bilgisayar geri dönüşümü başlatılmadan önce en fazla kaç kez sistem durumu denetimi başarısız olabilir.| 
|karşıEşik|0,80|Performans sayacının sağlıksız olarak kabul ediliş eşiği.| 

## <a name="http"></a>http

[http tetikleyiciler ve bağlamalar](functions-bindings-http-webhook.md)için yapılandırma ayarları.

```json
{
    "http": {
        "routePrefix": "api",
        "maxOutstandingRequests": 200,
        "maxConcurrentRequests": 100,
        "dynamicThrottlesEnabled": true
    }
}
```

|Özellik  |Varsayılan | Açıklama |
|---------|---------|---------| 
|dynamicThrottlesEnabled|yanlış|Etkinleştirildiğinde, bu ayar istek işleme ardışık ardışık lığı, bağlantılar/iş parçacıkları/süreçler/bellek/işlemci/vb. gibi sistem performans sayaçlarını periyodik olarak denetlemesine neden olur ve bu sayaçlardan herhangi biri yerleşik yüksek eşiğin (%80) üzerindeyse, sayaç normal düzeylere dönene kadar istekler 429 "Çok Meşgul" yanıtı ile reddedilir.|
|maxConcurrentRequests|sınırsız (`-1`)|Paralel olarak yürütülecek maksimum HTTP işlevi sayısı. Bu, kaynak kullanımını yönetmenize yardımcı olabilecek eşzamanlılığı denetlemenize olanak tanır. Örneğin, eşzamanlılık çok yüksek olduğunda sorunlara neden olacak şekilde çok sayıda sistem kaynağı (bellek/işlemci/soket) kullanan bir HTTP işleviniz olabilir. Veya bir üçüncü taraf hizmetine giden isteklerde bulunan bir işleviniz olabilir ve bu çağrıların oran sınırı olması gerekir. Bu gibi durumlarda, burada bir gaz uygulayarak yardımcı olabilir.|
|maxOutstandingRequests|sınırsız (`-1`)|Herhangi bir zamanda tutulan en fazla bekleyen istek sayısı. Bu sınır, sıraya dizilen ancak yürütmeye başlamamış istekleri ve devam eden yürütmeleri içerir. Bu sınır üzerinden gelen tüm istekler 429 "Çok Meşgul" yanıtıyla reddedilir. Bu, arayanların zaman tabanlı yeniden deneme stratejilerini kullanmalarına olanak tanır ve aynı zamanda maksimum istek gecikmelerini denetlemenize yardımcı olur. Bu yalnızca komut dosyası ana bilgisayar yürütme yolu içinde oluşan sıraya denetler. ASP.NET istek sırası gibi diğer kuyruklar yine de etkin ve bu ayardan etkilenmez.|
|rotaÖnek|API|Tüm rotalar için geçerli olan rota öneki. Varsayılan önekikaldırmak için boş bir dize kullanın. |

## <a name="id"></a>id

İş barındıran biri için benzersiz kimlik. Tire kaldırılmış küçük bir büyük harf GUID olabilir. Yerel olarak çalışırken gereklidir. Azure'da çalışırken, bir kimlik değeri belirlememenizi öneririz. Kimlik, atlandığında Azure'da `id` otomatik olarak oluşturulur. 

Bir Depolama hesabını birden çok işlevli uygulamada paylaşıyorsanız, her `id`işlev uygulamasının farklı bir . `id` Özelliği atlayabilir veya her işlev uygulamasının kini `id` el ile farklı bir değere ayarlayabilirsiniz. Zamanlayıcı tetikleyicisi, bir işlev uygulaması birden çok örneğe ölçeklendiğinde yalnızca bir zamanlayıcı örneği olmasını sağlamak için bir depolama kilidi kullanır. İki işlev uygulaması aynı `id` payı paylaşıyorsa ve her biri bir zamanlayıcı tetikleyicisi kullanıyorsa, yalnızca bir zamanlayıcı çalışır.

```json
{
    "id": "9f4ea53c5136457d883d685e57164f08"
}
```

## <a name="logger"></a>Günlükçü

[Bir ILogger nesnesi](functions-monitoring.md#write-logs-in-c-functions) veya [context.log](functions-monitoring.md#write-logs-in-javascript-functions)tarafından yazılan günlükler için filtreleme denetimlerini denetler.

```json
{
    "logger": {
        "categoryFilter": {
            "defaultLevel": "Information",
            "categoryLevels": {
                "Host": "Error",
                "Function": "Error",
                "Host.Aggregator": "Information"
            }
        }
    }
}
```

|Özellik  |Varsayılan | Açıklama |
|---------|---------|---------| 
|kategoriFiltre|yok|Kategoriye göre filtreleme belirtir| 
|defaultDüzey|Bilgi|`categoryLevels` Dizide belirtilmeyen kategoriler için, günlükleri bu düzeyde ve yukarıda Uygulama Öngörüleri'ne gönderin.| 
|kategoriSeviyeler|yok|Her kategori için Uygulama Öngörüleri'ne gönderilecek minimum günlük düzeyini belirten bir dizi kategori. Burada belirtilen kategori, aynı değerle başlayan tüm kategorileri denetler ve daha uzun değerler önceliklidir. Önceki örnek *host.json* dosyasında, "Host.Aggregator" ile başlayan `Information` tüm kategoriler düzeyinde günlük. "Host.Executor" gibi "Ana Bilgisayar" ile başlayan diğer `Error` tüm kategoriler düzeyinde oturum açar.| 

## <a name="queues"></a>Sıra

[Depolama sırası tetikleyicileri ve bağlamaları](functions-bindings-storage-queue.md)için yapılandırma ayarları.

```json
{
    "queues": {
      "maxPollingInterval": 2000,
      "visibilityTimeout" : "00:00:30",
      "batchSize": 16,
      "maxDequeueCount": 5,
      "newBatchThreshold": 8
    }
}
```

|Özellik  |Varsayılan | Açıklama |
|---------|---------|---------| 
|maxPollingInterval|60000|Sıra yoklamaları arasındaki milisaniye cinsinden en büyük aralık.| 
|görünürlükTimeout|0|İletinin işlenmesi başarısız olduğunda yeniden denemeler arasındaki zaman aralığı.| 
|batchSize|16|İşlevler çalışma zamanının aynı anda aldığı sıra iletilerinin sayısı ve paralel olarak işleyen. İşlenen sayı `newBatchThreshold`, çalışma zamanı başka bir toplu iş alır ve bu iletileri işleme başlar aşağı alır. Bu nedenle, işlev başına işlenen en fazla `batchSize` eşzamanlı `newBatchThreshold`ileti sayısı artıdır. Bu sınır, sıra tetiklenen her işlev için ayrı ayrı uygulanır. <br><br>Bir kuyruğa alınan iletiler için paralel yürütmeyi önlemek istiyorsanız, 1 olarak ayarlayabilirsiniz. `batchSize` Ancak, bu ayar, işlev uygulamanız tek bir sanal makinede (VM) çalıştığı sürece eşzamanlılığı ortadan kaldırır. İşlev uygulaması birden çok VM'ye ölçeklenirse, her VM, kuyruk tetiklenen her işlevin bir örneğini çalıştırabilir.<br><br>En `batchSize` fazla 32. | 
|maxDequeueCount|5|Bir iletiyi zehir kuyruğuna taşımadan önce işlemeyi deneme sayısı.| 
|newBatchThreshold|batchSize/2|Aynı anda işlenen ileti sayısı bu sayıya indiğinde, çalışma zamanı başka bir toplu iş alır.| 

## <a name="sendgrid"></a>SendGrid

[SendGrind çıktı bağlama](functions-bindings-sendgrid.md) için yapılandırma ayarı

```json
{
    "sendGrid": {
        "from": "Contoso Group <admin@contoso.com>"
    }
```

|Özellik  |Varsayılan | Açıklama |
|---------|---------|---------| 
|Kaynak|yok|Gönderenin tüm işlevler arasında e-posta adresi.| 

## <a name="servicebus"></a>serviceBus

[Servis Veri Kurumu tetikleyicileri ve bağlamaları](functions-bindings-service-bus.md)için yapılandırma ayarı.

```json
{
    "serviceBus": {
      "maxConcurrentCalls": 16,
      "prefetchCount": 100,
      "autoRenewTimeout": "00:05:00"
    }
}
```

|Özellik  |Varsayılan | Açıklama |
|---------|---------|---------| 
|Maxconcurrentcalls|16|İleti pompasının başlatması gereken geri aramaiçin en fazla eşzamanlı çağrı sayısı. Varsayılan olarak, İşlevler çalışma zamanı aynı anda birden çok iletiyi işler. Çalışma zamanını tek bir sıra veya konu iletisini aynı `maxConcurrentCalls` anda işlemek üzere yönlendirmek için 1 olarak ayarlayın. | 
|prefetchCount|yok|Temel MessageReceiver tarafından kullanılacak varsayılan PrefetchCount.| 
|autoRenewTimeout|00:05:00|İleti kilidinin otomatik olarak yenileneceği maksimum süre.| 

## <a name="singleton"></a>Singleton

Singleton kilit davranışı için yapılandırma ayarları. Daha fazla bilgi için [singleton desteği yle ilgili GitHub sorununa](https://github.com/Azure/azure-webjobs-sdk-script/issues/912)bakın.

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

|Özellik  |Varsayılan | Açıklama |
|---------|---------|---------| 
|kilitDönem|00:00:15|İşlev düzeyi kilitlerinin alındığı dönem. Kilitler otomatik yenileme.| 
|dinleyiciLockPeriod|00:01:00|Dinleyici kilitlerinin alındığı süre.| 
|dinleyiciLockRecoveryPollingInterval|00:01:00|Dinleyici kilidi başlangıçta elde edilemeseydi dinleyici kilidi kurtarma için kullanılan zaman aralığı.| 
|lockAcquisitionTimeout|00:01:00|Çalışma süresinin kilit elde etmeye çalışacağı maksimum süre.| 
|lockAcquisitionPollingInterval|yok|Kilit edinme denemeleri arasındaki aralık.| 

## <a name="tracing"></a>Izleme

*Sürüm 1.x*

Bir `TraceWriter` nesne kullanarak oluşturduğunuz günlükler için yapılandırma ayarları. [Bkz. C# Günlük](functions-reference-csharp.md#logging) ve [Düğüm.js Günlüğü](functions-reference-node.md#writing-trace-output-to-the-console).

```json
{
    "tracing": {
      "consoleLevel": "verbose",
      "fileLoggingMode": "debugOnly"
    }
}
```

|Özellik  |Varsayılan | Açıklama |
|---------|---------|---------| 
|consoleSeviye|bilgiler|Konsol günlüğe kaydetme nin izleme düzeyi. Seçenekler `off`şunlardır: `warning` `info`, `error` `verbose`, , ve .|
|fileLoggingMode|hataAyıkSadece|Dosya günlüğe kaydetme nin izleme düzeyi. Seçenekler `never`, `always` `debugOnly`, .| 

## <a name="watchdirectories"></a>watchDirectories

Değişiklikler için izlenmesi gereken [paylaşılan kod dizinleri](functions-reference-csharp.md#watched-directories) kümesi.  Bu dizinlerde kod değiştirildiğinde, değişikliklerin işlevleriniz tarafından alınmasını sağlar.

```json
{
    "watchDirectories": [ "Shared" ]
}
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [host.json dosyasını nasıl güncelleştirin](functions-reference.md#fileupdate)

> [!div class="nextstepaction"]
> [Ortam değişkenlerinde genel ayarları görün](functions-app-settings.md)
