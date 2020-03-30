---
title: Dayanıklı İşlevlerde HTTP API'ler - Azure Fonksiyonları
description: Azure İşlevler için Dayanıklı İşlevler uzantısında HTTP API'leri nasıl uygulayacağınızı öğrenin.
author: cgillum
ms.topic: conceptual
ms.date: 12/17/2019
ms.author: azfuncdf
ms.openlocfilehash: 4e4081ecca4714c713d105d363a83a4f96a0d3fc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79278174"
---
# <a name="http-api-reference"></a>HTTP API başvurusu

Dayanıklı İşlevler uzantısı, [orkestrasyonlar,](durable-functions-types-features-overview.md#orchestrator-functions) [varlıklar](durable-functions-types-features-overview.md#entity-functions)ve görev hub'ları üzerinde yönetim görevlerini gerçekleştirmek için kullanılabilecek yerleşik HTTP API'leri kümesini ortaya çıkarır. [task hubs](durable-functions-task-hubs.md) Bu HTTP API'leri, Azure İşlevleri ana bilgisayarı tarafından yetkilendirilen ancak doğrudan Dayanıklı İşlevler uzantısı tarafından işlenen genişletilebilirlik webhook'larıdır.

Uzantı tarafından uygulanan tüm HTTP API'leri için aşağıdaki parametreler gerekir. Tüm parametrelerin veri `string`türü.

| Parametre        | Parametre Türü  | Açıklama |
|------------------|-----------------|-------------|
| **`taskHub`**    | Sorgu dizesi    | [Görev merkezinin](durable-functions-task-hubs.md)adı. Belirtilmemişse, geçerli işlev uygulamasının görev merkezi adı kabul edilir. |
| **`connection`** | Sorgu dizesi    | Depolama hesabının bağlantı dizesinin **adı.** Belirtilmemişse, işlev uygulaması nın varsayılan bağlantı dizesi varsayalım. |
| **`systemKey`**  | Sorgu dizesi    | API çağırmak için gereken yetkilendirme anahtarı. |

`systemKey`Azure İşlevleri ana bilgisayar tarafından otomatik olarak oluşturulan bir yetkilendirme anahtarıdır. Özellikle Dayanıklı Görev uzantısı API'lerine erişim sağlar ve [diğer yetkilendirme anahtarları](https://github.com/Azure/azure-webjobs-sdk-script/wiki/Key-management-API)yla aynı şekilde yönetilebilir. `taskHub`.NET'teki `connection` `systemKey` `createCheckStatusResponse` `createHttpManagementPayload` `CreateCheckStatusResponse` API'ler veya `CreateHttpManagementPayload` JavaScript'teki API'ler gibi düzenleme [istemcisi bağlama](durable-functions-bindings.md#orchestration-client) API'lerini kullanarak doğru , ve sorgu dize değerlerini içeren URL'ler oluşturabilirsiniz.

Sonraki birkaç bölüm, uzantı tarafından desteklenen belirli HTTP API'lerini kapsar ve bunların nasıl kullanılabileceğinin örneklerini sağlar.

## <a name="start-orchestration"></a>Orkestrasyonu başlatın

Belirtilen orchestrator işlevinin yeni bir örneğini yürütmeye başlar.

### <a name="request"></a>İstek

İşlevler çalışma süresinin sürüm 1.x sürümü için istek aşağıdaki gibi biçimlendirilir (netlik için birden çok satır gösterilir):

```http
POST /admin/extensions/DurableTaskExtension/orchestrators/{functionName}/{instanceId?}
     ?taskHub={taskHub}
     &connection={connectionName}
     &code={systemKey}
```

Fonksiyonlar çalışma zamanı sürüm 2.x'te, URL biçimi aynı parametrelere sahiptir, ancak biraz farklı bir önek ile:

```http
POST /runtime/webhooks/durabletask/orchestrators/{functionName}/{instanceId?}
     ?taskHub={taskHub}
     &connection={connectionName}
     &code={systemKey}
```

Bu API için istek parametreleri, daha önce bahsedilen varsayılan kümenin yanı sıra aşağıdaki benzersiz parametreleri içerir:

| Alan              | Parametre türü  | Açıklama |
|--------------------|-----------------|-------------|
| **`functionName`** | URL'si             | Başlamak için orkestratör işlevinin adı. |
| **`instanceId`**   | URL'si             | İsteğe bağlı parametre. Orkestrasyon örneğinin kimliği. Belirtilmemişse, orkestratör işlevi rasgele bir örnek kimliğiyle başlar. |
| **`{content}`**    | İçerik isteme | İsteğe bağlı. JSON biçimli orchestrator fonksiyon girişi. |

### <a name="response"></a>Yanıt

Birkaç olası durum kodu değerleri döndürülebilir.

* **HTTP 202 (Kabul)**: Belirtilen orkestratör işlevi çalışmaya başlamak üzere zamanlandı. Yanıt `Location` üstbilgisi, düzenleme durumunu yoklama için bir URL içerir.
* **HTTP 400 (Kötü istek)**: Belirtilen orkestratör işlevi yok, belirtilen örnek kimliği geçerli değildi veya istek içeriği JSON geçerli değildi.

Aşağıdaki bir orkestratör işlevi `RestartVMs` başlatan ve JSON nesne yükünü içeren bir örnek istektir:

```http
POST /runtime/webhooks/durabletask/orchestrators/RestartVMs?code=XXX
Content-Type: application/json
Content-Length: 83

{
    "resourceGroup": "myRG",
    "subscriptionId": "111deb5d-09df-4604-992e-a968345530a9"
}
```

**HTTP 202** servis talepleri için yanıt yükü aşağıdaki alanları içeren bir JSON nesnesidir:

| Alan                       | Açıklama                          |
|-----------------------------|--------------------------------------|
| **`id`**                    |Orkestrasyon örneğinin kimliği. |
| **`statusQueryGetUri`**     |Orkestrasyon örneğinin durum URL'si. |
| **`sendEventPostUri`**      |Orkestrasyon örneğinin "yükseltme olayı" URL'si. |
| **`terminatePostUri`**      |Düzenleme örneğinin "sonlandırma" URL'si. |
| **`purgeHistoryDeleteUri`** |Orkestrasyon örneğinin "temizleme geçmişi" URL'si. |
| **`rewindPostUri`**         |(önizleme) Orkestrasyon örneğinin "geri sarma" URL'si. |

Tüm alanların veri `string`türü.

Burada kimliği (okunabilirlik için biçimlendirilmiş) `abc123` ile bir düzenleme örneği için örnek bir yanıt yükü verilmiştir:

```http
{
    "id": "abc123",
    "purgeHistoryDeleteUri": "http://localhost:7071/runtime/webhooks/durabletask/instances/abc123?code=XXX",
    "sendEventPostUri": "http://localhost:7071/runtime/webhooks/durabletask/instances/abc123/raiseEvent/{eventName}?code=XXX",
    "statusQueryGetUri": "http://localhost:7071/runtime/webhooks/durabletask/instances/abc123?code=XXX",
    "terminatePostUri": "http://localhost:7071/runtime/webhooks/durabletask/instances/abc123/terminate?reason={text}&code=XXX"
}
```

HTTP *yanıtı, Yoklama Tüketici Deseni*ile uyumlu olması amaçlanmıştır. Ayrıca aşağıdaki önemli yanıt üstbilgilerini içerir:

* **Konum**: Durum bitiş noktasının URL'si. Bu `statusQueryGetUri` URL, alanla aynı değeri içerir.
* **Yeniden Deneme-Sonra**: Yoklama işlemleri arasında bekleyecek saniye sayısı. Varsayılan değer: `10`.

Eşzamanlı HTTP yoklama deseni hakkında daha fazla bilgi için [HTTP async işlem izleme](durable-functions-http-features.md#async-operation-tracking) belgelerine bakın.

## <a name="get-instance-status"></a>Örnek durumu alma

Belirli bir orkestrasyon örneğinin durumunu alır.

### <a name="request"></a>İstek

İşlevler çalışma süresinin sürüm 1.x sürümü için istek aşağıdaki gibi biçimlendirilir (netlik için birden çok satır gösterilir):

```http
GET /admin/extensions/DurableTaskExtension/instances/{instanceId}
    ?taskHub={taskHub}
    &connection={connectionName}
    &code={systemKey}
    &showHistory=[true|false]
    &showHistoryOutput=[true|false]
    &showInput=[true|false]
```

Fonksiyonlar çalışma zamanı sürüm 2.x'te, URL biçimi aynı parametrelere sahiptir, ancak biraz farklı bir önek ile:

```http
GET /runtime/webhooks/durabletask/instances/{instanceId}
    ?taskHub={taskHub}
    &connection={connectionName}
    &code={systemKey}
    &showHistory=[true|false]
    &showHistoryOutput=[true|false]
    &showInput=[true|false]
```

Bu API için istek parametreleri, daha önce bahsedilen varsayılan kümenin yanı sıra aşağıdaki benzersiz parametreleri içerir:

| Alan                   | Parametre türü  | Açıklama |
|-------------------------|-----------------|-------------|
| **`instanceId`**        | URL'si             | Orkestrasyon örneğinin kimliği. |
| **`showInput`**         | Sorgu dizesi    | İsteğe bağlı parametre. `false`Ayarlanırsa, işlev girişi yanıt yüküne dahil edilmez.|
| **`showHistory`**       | Sorgu dizesi    | İsteğe bağlı parametre. `true`Ayarlanırsa, düzenleme yürütme geçmişi yanıt yüküne dahil edilir.|
| **`showHistoryOutput`** | Sorgu dizesi    | İsteğe bağlı parametre. `true`Ayarlanırsa, işlev çıktıları yürütme yürütme geçmişine dahil edilir.|
| **`createdTimeFrom`**   | Sorgu dizesi    | İsteğe bağlı parametre. Belirtildiğinde, verilen ISO8601 zaman damgası sırasında veya sonrasında oluşturulan döndürülen örneklerin listesini filtreler.|
| **`createdTimeTo`**     | Sorgu dizesi    | İsteğe bağlı parametre. Belirtildiğinde, verilen ISO8601 zaman damgası sırasında veya öncesinde oluşturulan döndürülen örneklerin listesini filtreler.|
| **`runtimeStatus`**     | Sorgu dizesi    | İsteğe bağlı parametre. Belirtildiğinde, döndürülen örneklerin listesini çalışma zamanı durumuna göre filtreler. Olası çalışma zamanı durum değerlerinin listesini görmek için [Sorgu örnekleri](durable-functions-instance-management.md) makalesine bakın. |

### <a name="response"></a>Yanıt

Birkaç olası durum kodu değerleri döndürülebilir.

* **HTTP 200 (Tamam)**: Belirtilen örnek tamamlanmış bir durumdadır.
* **HTTP 202 (Kabul)**: Belirtilen örnek devam etmektedir.
* **HTTP 400 (Kötü İstek)**: Belirtilen örnek başarısız oldu veya sonlandırıldı.
* **HTTP 404 (Bulunamadı)**: Belirtilen örnek yok veya çalışmaya başlamadı.
* **HTTP 500 (Internal Server Error)**: Belirtilen örnek işlenmemiş bir özel durumla başarısız oldu.

**HTTP 200** ve HTTP **202** durumlarının yanıt yükü aşağıdaki alanları içeren bir JSON nesnesidir:

| Alan                 | Veri türü | Açıklama |
|-----------------------|-----------|-------------|
| **`runtimeStatus`**   | string    | Örneğin çalışma zamanı durumu. Değerler *Arasında Çalışan*, *Bekleyen*, *Başarısız*, *İptal ,* *Sonlandırılan*, *Tamamlandı*. |
| **`input`**           | JSON      | Örneği başlatmada kullanılan JSON verileri. Bu alan, `null` `showInput` sorgu dize parametresi olarak ayarlanmışsa. `false`|
| **`customStatus`**    | JSON      | Özel orkestrasyon durumu için kullanılan JSON verileri. Bu alan `null` ayarlanmazsa. |
| **`output`**          | JSON      | Örneğin JSON çıktısı. Bu alan, `null` örneğin tamamlanmış bir durumda değilse. |
| **`createdTime`**     | string    | Örneğin oluşturulduğu saat. ISO 8601 genişletilmiş gösterimkullanır. |
| **`lastUpdatedTime`** | string    | Örneğin son olarak devam ettiği zaman. ISO 8601 genişletilmiş gösterimkullanır. |
| **`historyEvents`**   | JSON      | Orkestrasyon yürütme geçmişini içeren bir JSON dizisi. Bu alan, `null` `showHistory` sorgu dize parametresi `true`olarak ayarlanmadığı sürece. |

Burada orkestrasyon yürütme geçmişi ve etkinlik çıktıları (okunabilirlik için biçimlendirilmiş) dahil olmak üzere örnek bir yanıt yükü:

```json
{
  "createdTime": "2018-02-28T05:18:49Z",
  "historyEvents": [
      {
          "EventType": "ExecutionStarted",
          "FunctionName": "E1_HelloSequence",
          "Timestamp": "2018-02-28T05:18:49.3452372Z"
      },
      {
          "EventType": "TaskCompleted",
          "FunctionName": "E1_SayHello",
          "Result": "Hello Tokyo!",
          "ScheduledTime": "2018-02-28T05:18:51.3939873Z",
          "Timestamp": "2018-02-28T05:18:52.2895622Z"
      },
      {
          "EventType": "TaskCompleted",
          "FunctionName": "E1_SayHello",
          "Result": "Hello Seattle!",
          "ScheduledTime": "2018-02-28T05:18:52.8755705Z",
          "Timestamp": "2018-02-28T05:18:53.1765771Z"
      },
      {
          "EventType": "TaskCompleted",
          "FunctionName": "E1_SayHello",
          "Result": "Hello London!",
          "ScheduledTime": "2018-02-28T05:18:53.5170791Z",
          "Timestamp": "2018-02-28T05:18:53.891081Z"
      },
      {
          "EventType": "ExecutionCompleted",
          "OrchestrationStatus": "Completed",
          "Result": [
              "Hello Tokyo!",
              "Hello Seattle!",
              "Hello London!"
          ],
          "Timestamp": "2018-02-28T05:18:54.3660895Z"
      }
  ],
  "input": null,
  "customStatus": { "nextActions": ["A", "B", "C"], "foo": 2 },
  "lastUpdatedTime": "2018-02-28T05:18:54Z",
  "output": [
      "Hello Tokyo!",
      "Hello Seattle!",
      "Hello London!"
  ],
  "runtimeStatus": "Completed"
}
```

**HTTP 202** yanıtı, daha önce bahsedilen alanla aynı `statusQueryGetUri` URL'ye başvuran bir **Konum** yanıt üstbilgisini de içerir.

## <a name="get-all-instances-status"></a>Tüm örneklerin durumunu alma

Ayrıca, 'Örnek durumunu al' isteğini kaldırarak `instanceId` tüm örneklerin durumunu sorgulayabilirsiniz. Bu durumda, temel parametreler 'Örnek durumunu al' ile aynıdır. Filtreleme için sorgu dize parametreleri de desteklenir.

Unutulmaması gereken bir `connection` `code` şey ve isteğe bağlıdır. İşlev üzerinde anonim auth varsa, o zaman `code` gerekli değildir.
AzureWebJobsStorage uygulama ayarında tanımlandığı farklı bir depolama bağlantı dizesi kullanmak istemiyorsanız, bağlantı sorgusu dize parametresini güvenle yok sayabilirsiniz.

### <a name="request"></a>İstek

İşlevler çalışma süresinin sürüm 1.x sürümü için istek aşağıdaki gibi biçimlendirilir (netlik için birden çok satır gösterilir):

```http
GET /admin/extensions/DurableTaskExtension/instances
    ?taskHub={taskHub}
    &connection={connectionName}
    &code={systemKey}
    &createdTimeFrom={timestamp}
    &createdTimeTo={timestamp}
    &runtimeStatus={runtimeStatus1,runtimeStatus2,...}
    &showInput=[true|false]
    &top={integer}
```

Fonksiyonlar çalışma zamanı sürüm 2.x'te, URL biçimi aynı parametrelere sahiptir, ancak biraz farklı bir önek ile:

```http
GET /runtime/webhooks/durableTask/instances?
    taskHub={taskHub}
    &connection={connectionName}
    &code={systemKey}
    &createdTimeFrom={timestamp}
    &createdTimeTo={timestamp}
    &runtimeStatus={runtimeStatus1,runtimeStatus2,...}
    &showInput=[true|false]
    &top={integer}
```

Bu API için istek parametreleri, daha önce bahsedilen varsayılan kümenin yanı sıra aşağıdaki benzersiz parametreleri içerir:

| Alan                   | Parametre türü  | Açıklama |
|-------------------------|-----------------|-------------|
| **`instanceId`**        | URL'si             | Orkestrasyon örneğinin kimliği. |
| **`showInput`**         | Sorgu dizesi    | İsteğe bağlı parametre. `false`Ayarlanırsa, işlev girişi yanıt yüküne dahil edilmez.|
| **`showHistory`**       | Sorgu dizesi    | İsteğe bağlı parametre. `true`Ayarlanırsa, düzenleme yürütme geçmişi yanıt yüküne dahil edilir.|
| **`showHistoryOutput`** | Sorgu dizesi    | İsteğe bağlı parametre. `true`Ayarlanırsa, işlev çıktıları yürütme yürütme geçmişine dahil edilir.|
| **`createdTimeFrom`**   | Sorgu dizesi    | İsteğe bağlı parametre. Belirtildiğinde, verilen ISO8601 zaman damgası sırasında veya sonrasında oluşturulan döndürülen örneklerin listesini filtreler.|
| **`createdTimeTo`**     | Sorgu dizesi    | İsteğe bağlı parametre. Belirtildiğinde, verilen ISO8601 zaman damgası sırasında veya öncesinde oluşturulan döndürülen örneklerin listesini filtreler.|
| **`runtimeStatus`**     | Sorgu dizesi    | İsteğe bağlı parametre. Belirtildiğinde, döndürülen örneklerin listesini çalışma zamanı durumuna göre filtreler. Olası çalışma zamanı durum değerlerinin listesini görmek için [Sorgu örnekleri](durable-functions-instance-management.md) makalesine bakın. |
| **`top`**               | Sorgu dizesi    | İsteğe bağlı parametre. Belirtildiğinde, sorgu tarafından döndürülen örnek sayısını sınırlar. |

### <a name="response"></a>Yanıt

Burada düzenleme durumu (okunabilirlik için biçimlendirilmiş) dahil olmak üzere yanıt yükleri bir örnektir:

```json
[
    {
        "instanceId": "7af46ff000564c65aafbfe99d07c32a5",
        "runtimeStatus": "Completed",
        "input": null,
        "customStatus": null,
        "output": [
            "Hello Tokyo!",
            "Hello Seattle!",
            "Hello London!"
        ],
        "createdTime": "2018-06-04T10:46:39Z",
        "lastUpdatedTime": "2018-06-04T10:46:47Z"
    },
    {
        "instanceId": "80eb7dd5c22f4eeba9f42b062794321e",
        "runtimeStatus": "Running",
        "input": null,
        "customStatus": null,
        "output": null,
        "createdTime": "2018-06-04T15:18:28Z",
        "lastUpdatedTime": "2018-06-04T15:18:38Z"
    },
    {
        "instanceId": "9124518926db408ab8dfe84822aba2b1",
        "runtimeStatus": "Completed",
        "input": null,
        "customStatus": null,
        "output": [
            "Hello Tokyo!",
            "Hello Seattle!",
            "Hello London!"
        ],
        "createdTime": "2018-06-04T10:46:54Z",
        "lastUpdatedTime": "2018-06-04T10:47:03Z"
    },
    {
        "instanceId": "d100b90b903c4009ba1a90868331b11b",
        "runtimeStatus": "Pending",
        "input": null,
        "customStatus": null,
        "output": null,
        "createdTime": "2018-06-04T15:18:39Z",
        "lastUpdatedTime": "2018-06-04T15:18:39Z"
    }
]
```

> [!NOTE]
> Örnekler tablosunda çok sayıda satır varsa, bu işlem Azure Depolama G/Ç açısından çok pahalı olabilir. Örnek tablosunda daha fazla [ayrıntı, Dayanıklı İşlevler (Azure İşlevleri) belgelerinde Performans ve ölçekte](durable-functions-perf-and-scale.md#instances-table) bulunabilir.
>

Daha fazla sonuç varsa, yanıt üstbilgisinde bir devam belirteci döndürülür.  Üstbilginin `x-ms-continuation-token`adı.

Bir sonraki istek üstbilgisinde devam belirteç değeri ayarlarsanız, sonuçların bir sonraki sayfasını alabilirsiniz. İstek üstbilgisinin bu adı `x-ms-continuation-token`da.

## <a name="purge-single-instance-history"></a>Tek örnek geçmişini temizleme

Belirli bir düzenleme örneği için geçmiş ve ilgili yapıları siler.

### <a name="request"></a>İstek

İşlevler çalışma süresinin sürüm 1.x sürümü için istek aşağıdaki gibi biçimlendirilir (netlik için birden çok satır gösterilir):

```http
DELETE /admin/extensions/DurableTaskExtension/instances/{instanceId}
    ?taskHub={taskHub}
    &connection={connection}
    &code={systemKey}
```

Fonksiyonlar çalışma zamanı sürüm 2.x'te, URL biçimi aynı parametrelere sahiptir, ancak biraz farklı bir önek ile:

```http
DELETE /runtime/webhooks/durabletask/instances/{instanceId}
    ?taskHub={taskHub}
    &connection={connection}
    &code={systemKey}
```

Bu API için istek parametreleri, daha önce bahsedilen varsayılan kümenin yanı sıra aşağıdaki benzersiz parametreleri içerir:

| Alan             | Parametre türü  | Açıklama |
|-------------------|-----------------|-------------|
| **`instanceId`**  | URL'si             | Orkestrasyon örneğinin kimliği. |

### <a name="response"></a>Yanıt

Aşağıdaki HTTP durum kodu değerleri döndürülebilir.

* **HTTP 200 (Tamam)**: Örnek geçmişi başarıyla temizlendi.
* **HTTP 404 (Bulunamadı)**: Belirtilen örnek yok.

**HTTP 200** kılıfıiçin yanıt yükü aşağıdaki alana sahip bir JSON nesnesidir:

| Alan                  | Veri türü | Açıklama |
|------------------------|-----------|-------------|
| **`instancesDeleted`** | integer   | Silinen örnek sayısı. Tek örnek durumda, bu değer `1`her zaman olmalıdır. |

Burada bir örnek yanıt yükü (okunabilirlik için biçimlendirilmiş):

```json
{
    "instancesDeleted": 1
}
```

## <a name="purge-multiple-instance-histories"></a>Birden çok örnek geçmişini temizleme

Ayrıca, 'Tek örnek geçmişini temizleme' isteğini kaldırarak `{instanceId}` görev hub'ındaki birden çok örnek için geçmişi ve ilgili yapıları da silebilirsiniz. Örnek geçmişi seçiçle temizlemişolmak için, 'Bütün durumlar ışığını al' istekte açıklanan aynı filtreleri kullanım.

### <a name="request"></a>İstek

İşlevler çalışma süresinin sürüm 1.x sürümü için istek aşağıdaki gibi biçimlendirilir (netlik için birden çok satır gösterilir):

```http
DELETE /admin/extensions/DurableTaskExtension/instances
    ?taskHub={taskHub}
    &connection={connectionName}
    &code={systemKey}
    &createdTimeFrom={timestamp}
    &createdTimeTo={timestamp}
    &runtimeStatus={runtimeStatus1,runtimeStatus2,...}
```

Fonksiyonlar çalışma zamanı sürüm 2.x'te, URL biçimi aynı parametrelere sahiptir, ancak biraz farklı bir önek ile:

```http
DELETE /runtime/webhooks/durabletask/instances
    ?taskHub={taskHub}
    &connection={connectionName}
    &code={systemKey}
    &createdTimeFrom={timestamp}
    &createdTimeTo={timestamp}
    &runtimeStatus={runtimeStatus1,runtimeStatus2,...}
```

Bu API için istek parametreleri, daha önce bahsedilen varsayılan kümenin yanı sıra aşağıdaki benzersiz parametreleri içerir:

| Alan                 | Parametre türü  | Açıklama |
|-----------------------|-----------------|-------------|
| **`createdTimeFrom`** | Sorgu dizesi    | Verilen ISO8601 zaman damgası sırasında veya sonrasında oluşturulan tasfiye örneklerilistesini filtreler.|
| **`createdTimeTo`**   | Sorgu dizesi    | İsteğe bağlı parametre. Belirtildiğinde, verilen ISO8601 zaman damgası sırasında veya öncesinde oluşturulan tasfiye örnekleri listesini filtreler.|
| **`runtimeStatus`**   | Sorgu dizesi    | İsteğe bağlı parametre. Belirtildiğinde, çalışma zamanı durumuna göre tasfiye edilen örnekler listesini filtreler. Olası çalışma zamanı durum değerlerinin listesini görmek için [Sorgu örnekleri](durable-functions-instance-management.md) makalesine bakın. |

> [!NOTE]
> Örnekler ve/veya Geçmiş tablolarında çok sayıda satır varsa, bu işlem Azure Depolama G/Ç açısından çok pahalı olabilir. Bu tablolarla ilgili daha fazla [ayrıntı, Dayanıklı İşlevler (Azure İşlevleri) belgelerinde Performans ve ölçekte](durable-functions-perf-and-scale.md#instances-table) bulunabilir.

### <a name="response"></a>Yanıt

Aşağıdaki HTTP durum kodu değerleri döndürülebilir.

* **HTTP 200 (Tamam)**: Örnek geçmişi başarıyla temizlendi.
* **HTTP 404 (Bulunamadı)**: Filtre ifadesiyle eşleşen hiçbir örnek bulunamadı.

**HTTP 200** kılıfıiçin yanıt yükü aşağıdaki alana sahip bir JSON nesnesidir:

| Alan                   | Veri türü | Açıklama |
|-------------------------|-----------|-------------|
| **`instancesDeleted`**  | integer   | Silinen örnek sayısı. |

Burada bir örnek yanıt yükü (okunabilirlik için biçimlendirilmiş):

```json
{
    "instancesDeleted": 250
}
```

## <a name="raise-event"></a>Etkinliği yükseltme

Çalışan bir orkestrasyon örneğine olay bildirimi iletisi gönderir.

### <a name="request"></a>İstek

İşlevler çalışma süresinin sürüm 1.x sürümü için istek aşağıdaki gibi biçimlendirilir (netlik için birden çok satır gösterilir):

```http
POST /admin/extensions/DurableTaskExtension/instances/{instanceId}/raiseEvent/{eventName}
    ?taskHub={taskHub}
    &connection={connectionName}
    &code={systemKey}
```

Fonksiyonlar çalışma zamanı sürüm 2.x'te, URL biçimi aynı parametrelere sahiptir, ancak biraz farklı bir önek ile:

```http
POST /runtime/webhooks/durabletask/instances/{instanceId}/raiseEvent/{eventName}
    ?taskHub={taskHub}
    &connection={connectionName}
    &code={systemKey}
```

Bu API için istek parametreleri, daha önce bahsedilen varsayılan kümenin yanı sıra aşağıdaki benzersiz parametreleri içerir:

| Alan             | Parametre türü  | Açıklama |
|-------------------|-----------------|-------------|
| **`instanceId`**  | URL'si             | Orkestrasyon örneğinin kimliği. |
| **`eventName`**   | URL'si             | Hedef orkestrasyon örneğinin beklediği olayın adı. |
| **`{content}`**   | İçerik isteme | JSON biçimlendirilmiş olay yükü. |

### <a name="response"></a>Yanıt

Birkaç olası durum kodu değerleri döndürülebilir.

* **HTTP 202 (Kabul)**: Yükseltilen olay işlenmek üzere kabul edildi.
* **HTTP 400 (Kötü istek)**: İstek `application/json` içeriği türünde değildi veya JSON geçerli değildi.
* **HTTP 404 (Bulunamadı)**: Belirtilen örnek bulunamadı.
* **HTTP 410 (Gone)**: Belirtilen örnek tamamlandı veya başarısız oldu ve yükseltilen olayları işleyemez.

JSON dizesini `"incr"` **işlem**adlı bir olayı bekleyen bir örneğe gönderen bir örnek istek aşağıda verilmiştir:

```http
POST /admin/extensions/DurableTaskExtension/instances/bcf6fb5067b046fbb021b52ba7deae5a/raiseEvent/operation?taskHub=DurableFunctionsHub&connection=Storage&code=XXX
Content-Type: application/json
Content-Length: 6

"incr"
```

Bu API için yanıtlar herhangi bir içerik içermez.

## <a name="terminate-instance"></a>Örneği sonlandırma

Çalışan bir orkestrasyon örneğini sonlandırır.

### <a name="request"></a>İstek

İşlevler çalışma süresinin sürüm 1.x sürümü için istek aşağıdaki gibi biçimlendirilir (netlik için birden çok satır gösterilir):

```http
POST /admin/extensions/DurableTaskExtension/instances/{instanceId}/terminate
    ?taskHub={taskHub}
    &connection={connectionName}
    &code={systemKey}
    &reason={text}
```

Fonksiyonlar çalışma zamanı sürüm 2.x'te, URL biçimi aynı parametrelere sahiptir, ancak biraz farklı bir önek ile:

```http
POST /runtime/webhooks/durabletask/instances/{instanceId}/terminate
    ?taskHub={taskHub}
    &connection={connectionName}
    &code={systemKey}
    &reason={text}
```

Bu API için istek parametreleri, daha önce bahsedilen varsayılan kümenin yanı sıra aşağıdaki benzersiz parametreyi de içerir.

| Alan             | Parametre Türü  | Açıklama |
|-------------------|-----------------|-------------|
| **`instanceId`**  | URL'si             | Orkestrasyon örneğinin kimliği. |
| **`reason`**      | Sorgu dizesi    | İsteğe bağlı. Orkestrasyon örneğini sonlandırma nedeni. |

### <a name="response"></a>Yanıt

Birkaç olası durum kodu değerleri döndürülebilir.

* **HTTP 202 (Kabul)**: Sonlandırma isteği işleme alındı.
* **HTTP 404 (Bulunamadı)**: Belirtilen örnek bulunamadı.
* **HTTP 410 (Gone)**: Belirtilen örnek tamamlandı veya başarısız oldu.

Çalışan bir örneği sonlandıran ve **buggy**nedenini belirten bir örnek istek aşağıda verilmiştir:

```
POST /admin/extensions/DurableTaskExtension/instances/bcf6fb5067b046fbb021b52ba7deae5a/terminate?reason=buggy&taskHub=DurableFunctionsHub&connection=Storage&code=XXX
```

Bu API için yanıtlar herhangi bir içerik içermez.

## <a name="rewind-instance-preview"></a>Geri sarma örneği (önizleme)

En son başarısız işlemleri yeniden oynatarak başarısız bir düzenleme örneğini çalışan duruma geri yükler.

### <a name="request"></a>İstek

İşlevler çalışma süresinin sürüm 1.x sürümü için istek aşağıdaki gibi biçimlendirilir (netlik için birden çok satır gösterilir):

```http
POST /admin/extensions/DurableTaskExtension/instances/{instanceId}/rewind
    ?taskHub={taskHub}
    &connection={connectionName}
    &code={systemKey}
    &reason={text}
```

Fonksiyonlar çalışma zamanı sürüm 2.x'te, URL biçimi aynı parametrelere sahiptir, ancak biraz farklı bir önek ile:

```http
POST /runtime/webhooks/durabletask/instances/{instanceId}/rewind
    ?taskHub={taskHub}
    &connection={connectionName}
    &code={systemKey}
    &reason={text}
```

Bu API için istek parametreleri, daha önce bahsedilen varsayılan kümenin yanı sıra aşağıdaki benzersiz parametreyi de içerir.

| Alan             | Parametre Türü  | Açıklama |
|-------------------|-----------------|-------------|
| **`instanceId`**  | URL'si             | Orkestrasyon örneğinin kimliği. |
| **`reason`**      | Sorgu dizesi    | İsteğe bağlı. Orkestrasyon örneğini geri sarma nedeni. |

### <a name="response"></a>Yanıt

Birkaç olası durum kodu değerleri döndürülebilir.

* **HTTP 202 (Kabul)**: Geri sarma isteği işleme alındı.
* **HTTP 404 (Bulunamadı)**: Belirtilen örnek bulunamadı.
* **HTTP 410 (Gone)**: Belirtilen örnek tamamlandı veya sonlandırıldı.

Başarısız bir örneği geri saran ve düzeltilen bir nedeni belirten bir örnek istek aşağıda **verilmiştir:**

```http
POST /admin/extensions/DurableTaskExtension/instances/bcf6fb5067b046fbb021b52ba7deae5a/rewind?reason=fixed&taskHub=DurableFunctionsHub&connection=Storage&code=XXX
```

Bu API için yanıtlar herhangi bir içerik içermez.

## <a name="signal-entity"></a>Sinyal varlığı

[Dayanıklı](durable-functions-types-features-overview.md#entity-functions)Varlık'a tek yönlü bir işlem iletisi gönderir. Varlık yoksa, otomatik olarak oluşturulur.

> [!NOTE]
> Dayanıklı varlıklar Dayanıklı Fonksiyonlar 2.0'dan başlayarak kullanılabilir.

### <a name="request"></a>İstek

HTTP isteği aşağıdaki gibi biçimlendirilir (netlik için birden çok satır gösterilir):

```http
POST /runtime/webhooks/durabletask/entities/{entityName}/{entityKey}
    ?taskHub={taskHub}
    &connection={connectionName}
    &code={systemKey}
    &op={operationName}
```

Bu API için istek parametreleri, daha önce bahsedilen varsayılan kümenin yanı sıra aşağıdaki benzersiz parametreleri içerir:

| Alan             | Parametre türü  | Açıklama |
|-------------------|-----------------|-------------|
| **`entityName`**  | URL'si             | Varlığın adı (türü). |
| **`entityKey`**   | URL'si             | Varlığın anahtarı (benzersiz kimlik). |
| **`op`**          | Sorgu dizesi    | İsteğe bağlı. Çağırmak için kullanıcı tanımlı işlemin adı. |
| **`{content}`**   | İçerik isteme | JSON biçimlendirilmiş olay yükü. |

Burada, kullanıcı tarafından tanımlanan "Ekle" iletisi `Counter` gönderen `steps`bir örnek istek var. İletinin içeriği değeridir. `5` Varlık zaten yoksa, bu istek tarafından oluşturulur:

```http
POST /runtime/webhooks/durabletask/entities/Counter/steps?op=Add
Content-Type: application/json

5
```

> [!NOTE]
> Varsayılan olarak [.NET'teki sınıf tabanlı varlıklar,](durable-functions-dotnet-entities.md#defining-entity-classes)bir varlığın `op` durumunu silecek değerini `delete` belirterek. Varlık adlı `delete`bir işlem tanımlarsa , ancak, bu kullanıcı tanımlı işlem yerine çağrılır.

### <a name="response"></a>Yanıt

Bu işlem birkaç olası yanıtları vardır:

* **HTTP 202 (Kabul)**: Sinyal işlemi eşzamanlı işlem için kabul edildi.
* **HTTP 400 (Kötü istek)**: İstek `application/json`içeriği türünde değildi, JSON geçerli `entityKey` değildi veya geçersiz bir değeri vardı.
* **HTTP 404 (Bulunamadı)**: `entityName` Belirtilen bulunamadı.

Başarılı bir HTTP isteği yanıtta herhangi bir içerik içermez. Başarısız bir HTTP isteği yanıt içeriğinde JSON biçimlendirilmiş hata bilgileri içerebilir.

## <a name="get-entity"></a>Varlık alın

Belirtilen varlığın durumunu alır.

### <a name="request"></a>İstek

HTTP isteği aşağıdaki gibi biçimlendirilir (netlik için birden çok satır gösterilir):

```http
GET /runtime/webhooks/durabletask/entities/{entityName}/{entityKey}
    ?taskHub={taskHub}
    &connection={connectionName}
    &code={systemKey}
```

### <a name="response"></a>Yanıt

Bu işlemin iki olası yanıtı vardır:

* **HTTP 200 (Ok)**: Belirtilen varlık vardır.
* **HTTP 404 (Bulunamadı)**: Belirtilen varlık bulunamadı.

Başarılı bir yanıt, varlığın içeriği olarak JSON serileştirilmiş durumunu içerir.

### <a name="example"></a>Örnek
Aşağıdaki örnek HTTP isteği adlı `Counter` `steps`varolan bir varlığın durumunu alır:

```http
GET /runtime/webhooks/durabletask/entities/Counter/steps
```

`Counter` Varlık yalnızca bir `currentValue` alanda kaydedilmiş birkaç adım içeriyorsa, yanıt içeriği aşağıdaki gibi görünebilir (okunabilirlik için biçimlendirilmiş):

```json
{
    "currentValue": 5
}
```

## <a name="list-entities"></a>Liste varlıkları

Birden çok varlığı varlık adına veya son işlem tarihine göre sorgulayabilirsiniz.

### <a name="request"></a>İstek

HTTP isteği aşağıdaki gibi biçimlendirilir (netlik için birden çok satır gösterilir):

```http
GET /runtime/webhooks/durabletask/entities/{entityName}
    ?taskHub={taskHub}
    &connection={connectionName}
    &code={systemKey}
    &lastOperationTimeFrom={timestamp}
    &lastOperationTimeTo={timestamp}
    &fetchState=[true|false]
    &top={integer}
```

Bu API için istek parametreleri, daha önce bahsedilen varsayılan kümenin yanı sıra aşağıdaki benzersiz parametreleri içerir:

| Alan                       | Parametre türü  | Açıklama |
|-----------------------------|-----------------|-------------|
| **`entityName`**            | URL'si             | İsteğe bağlı. Belirtildiğinde, döndürülen varlıkların listesini varlık adlarına göre filtreler (büyük/küçük harf duyarsız). |
| **`fetchState`**            | Sorgu dizesi    | İsteğe bağlı parametre. `true`Ayarlanırsa, varlık durumu yanıt yüküne dahil edilir. |
| **`lastOperationTimeFrom`** | Sorgu dizesi    | İsteğe bağlı parametre. Belirtildiğinde, verilen ISO8601 zaman damgası sonra işlemleri işleyen döndürülen varlıkların listesini filtreler. |
| **`lastOperationTimeTo`**   | Sorgu dizesi    | İsteğe bağlı parametre. Belirtildiğinde, verilen ISO8601 zaman damgası önce işlemleri işleyen döndürülen varlıkların listesini filtreler. |
| **`top`**                   | Sorgu dizesi    | İsteğe bağlı parametre. Belirtildiğinde, sorgu tarafından döndürülen varlıkların sayısını sınırlar. |


### <a name="response"></a>Yanıt

Başarılı bir HTTP 200 yanıtı, JSON serileştirilmiş bir varlık dizisi ve isteğe bağlı olarak her varlığın durumunu içerir.

Varsayılan olarak işlem, sorgu ölçütleri ile eşleşen ilk 100 varlığı döndürür. Arayan, farklı bir maksimum sonuç `top` sayısını döndürmek için sorgu dizesi parametre değeri belirtebilir. Döndürülenin ötesinde daha fazla sonuç varsa, yanıt üstbilgisinde bir devam belirteci de döndürülür. Üstbilginin `x-ms-continuation-token`adı.

Bir sonraki istek üstbilgisinde devam belirteç değeri ayarlarsanız, sonuçların bir sonraki sayfasını alabilirsiniz. İstek üstbilgisinin bu adı `x-ms-continuation-token`da.

### <a name="example---list-all-entities"></a>Örnek - tüm varlıkları listele

Aşağıdaki örnek HTTP isteği, görev merkezindeki tüm varlıkları listeler:

```http
GET /runtime/webhooks/durabletask/entities
```

Yanıt JSON aşağıdaki gibi görünebilir (okunabilirlik için biçimlendirilmiş):

```json
[
    {
        "entityId": { "key": "cats", "name": "counter" },
        "lastOperationTime": "2019-12-18T21:45:44.6326361Z",
    },
    {
        "entityId": { "key": "dogs", "name": "counter" },
        "lastOperationTime": "2019-12-18T21:46:01.9477382Z"
    },
    {
        "entityId": { "key": "mice", "name": "counter" },
        "lastOperationTime": "2019-12-18T21:46:15.4626159Z"
    },
    {
        "entityId": { "key": "radio", "name": "device" },
        "lastOperationTime": "2019-12-18T21:46:18.2616154Z"
    },
]
```

### <a name="example---filtering-the-list-of-entities"></a>Örnek - varlıklar listesini filtreleme

Aşağıdaki örnek HTTP isteği, yalnızca türün `counter` ilk iki varlıklarını listeler ve durumlarını da getirir:

```http
GET /runtime/webhooks/durabletask/entities/counter?top=2&fetchState=true
```

Yanıt JSON aşağıdaki gibi görünebilir (okunabilirlik için biçimlendirilmiş):

```json
[
    {
        "entityId": { "key": "cats", "name": "counter" },
        "lastOperationTime": "2019-12-18T21:45:44.6326361Z",
        "state": { "value": 9 }
    },
    {
        "entityId": { "key": "dogs", "name": "counter" },
        "lastOperationTime": "2019-12-18T21:46:01.9477382Z",
        "state": { "value": 10 }
    }
]
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Dayanıklı işlevlerinizi izlemek için Uygulama Öngörüleri'ni nasıl kullanacağınızı öğrenin](durable-functions-diagnostics.md)