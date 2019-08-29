---
title: Dayanıklı İşlevler-Azure 'da HTTP API 'Leri
description: Azure Işlevleri için Dayanıklı İşlevler uzantısında HTTP API 'Leri uygulamayı öğrenin.
services: functions
author: cgillum
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.topic: conceptual
ms.date: 07/08/2019
ms.author: azfuncdf
ms.openlocfilehash: 11ae418ddbe007c6fd5aa44ef22ed7fddec9c702
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70087261"
---
# <a name="http-apis-in-durable-functions-azure-functions"></a>Dayanıklı İşlevler 'de HTTP API 'Leri (Azure Işlevleri)

Dayanıklı görev uzantısı, aşağıdaki görevleri gerçekleştirmek için kullanılabilecek bir dizi HTTP API 'si sunar:

* Bir Orchestration örneğinin durumunu getir.
* Bekleyen bir düzenleme örneğine olay gönderin.
* Çalışan bir düzenleme örneğini sonlandırın.

Bu HTTP API 'Lerinin her biri, doğrudan dayanıklı görev uzantısı tarafından işlenen bir Web kancası işlemidir. İşlev uygulamasındaki herhangi bir işleve özgü değildir.

> [!NOTE]
> Bu işlemler, [Durableorchestrationclient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) sınıfında örnek yönetimi API 'leri kullanılarak doğrudan da çağrılabilir. Daha fazla bilgi için bkz. [örnek yönetimi](durable-functions-instance-management.md).

## <a name="http-api-url-discovery"></a>HTTP API URL keşfi

[Durableorchestrationclient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) sınıfı, desteklenen tüm işlemlerin bağlantılarını IÇEREN bir http yanıt yükü oluşturmak için kullanılabilen bir [Createcheckstatusresponse](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_CreateCheckStatusResponse_) API 'si sunar. Bu API 'nin nasıl kullanılacağını gösteren örnek bir HTTP tetikleyici işlevi aşağıda verilmiştir:

### <a name="c"></a>C#

[!code-csharp[Main](~/samples-durable-functions/samples/csx/HttpStart/run.csx)]

### <a name="javascript-functions-2x-only"></a>JavaScript (yalnızca 2. x Işlevleri)

[!code-javascript[Main](~/samples-durable-functions/samples/javascript/HttpStart/index.js)]

Bu örnek işlevler aşağıdaki JSON yanıt verilerini üretir. Tüm alanların `string`veri türü.

| Alan                   |Açıklama                           |
|-----------------------------|--------------------------------------|
| **`id`**                    |Orchestration örneğinin KIMLIĞI. |
| **`statusQueryGetUri`**     |Orchestration örneğinin durum URL 'SI. |
| **`sendEventPostUri`**      |Orchestration örneğinin "olay oluştur" URL 'SI. |
| **`terminatePostUri`**      |Orchestration örneğinin "Terminate" URL 'SI. |
| **`purgeHistoryDeleteUri`** |Orchestration örneğinin "Temizleme geçmişi" URL 'SI. |
| **`rewindPostUri`**         |Önizle Orchestration örneğinin "geri sarma" URL 'SI. |

Örnek bir yanıt aşağıda verilmiştir:

```http
HTTP/1.1 202 Accepted
Content-Length: 923
Content-Type: application/json; charset=utf-8
Location: https://{host}/runtime/webhooks/durabletask/instances/34ce9a28a6834d8492ce6a295f1a80e2?taskHub=DurableFunctionsHub&connection=Storage&code=XXX

{
    "id":"34ce9a28a6834d8492ce6a295f1a80e2",
    "statusQueryGetUri":"https://{host}/runtime/webhooks/durabletask/instances/34ce9a28a6834d8492ce6a295f1a80e2?taskHub=DurableFunctionsHub&connection=Storage&code=XXX",
    "sendEventPostUri":"https://{host}/runtime/webhooks/durabletask/instances/34ce9a28a6834d8492ce6a295f1a80e2/raiseEvent/{eventName}?taskHub=DurableFunctionsHub&connection=Storage&code=XXX",
    "terminatePostUri":"https://{host}/runtime/webhooks/durabletask/instances/34ce9a28a6834d8492ce6a295f1a80e2/terminate?reason={text}&taskHub=DurableFunctionsHub&connection=Storage&code=XXX",
    "purgeHistoryDeleteUri":"https://{host}/runtime/webhooks/durabletask/instances/34ce9a28a6834d8492ce6a295f1a80e2?taskHub=DurableFunctionsHub&connection=Storage&code=XXX"
    "rewindPostUri":"https://{host}/runtime/webhooks/durabletask/instances/34ce9a28a6834d8492ce6a295f1a80e2/rewind?reason={text}&taskHub=DurableFunctionsHub&connection=Storage&code=XXX"
}
```

> [!NOTE]
> Web kancası URL 'Lerinin biçimi, çalıştırdığınız Azure Işlevleri ana bilgisayarı sürümüne bağlı olarak farklılık gösterebilir. Yukarıdaki örnek, Azure Işlevleri 2. x ana bilgisayarı içindir.

## <a name="async-operation-tracking"></a>Zaman uyumsuz işlem izleme

Daha önce bahsedilen HTTP yanıtı, Dayanıklı İşlevler uzun süre çalışan HTTP zaman uyumsuz API 'Leri uygulamaya yardımcı olacak şekilde tasarlanmıştır. Bu, bazen *yoklama tüketici stili*olarak adlandırılır. İstemci/sunucu akışı aşağıdaki gibi çalışmaktadır:

1. İstemci, Orchestrator işlevi gibi uzun süre çalışan bir işlemi başlatmak için bir HTTP isteği yayınlar.
2. Hedef http tetikleyicisi, bir `Location` üst `statusQueryGetUri` bilgiyle değeri olan bir http 202 yanıtı döndürür.
3. İstemci `Location` başlıktaki URL 'yi yoklar. Bir `Location` üst bilgiyle http 202 yanıtlarını görmeyi sürdürür.
4. Örnek tamamlandığında (veya başarısız olursa), `Location` üstbilgideki uç nokta http 200 döndürür.

Bu protokol, `Location` bir HTTP uç noktası yoklamayı ve üstbilgiyi takip eden dış istemcilerle veya hizmetlerle uzun süreli işlemlerin koordine etmesine olanak tanır. Temel parçalar Dayanıklı İşlevler HTTP API 'Lerinde zaten yerleşik olarak bulunur.

> [!NOTE]
> Varsayılan olarak, [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/) tarafından sunulan tüm HTTP tabanlı eylemler standart zaman uyumsuz işlem modelini destekler. Bu özellik, uzun süreli dayanıklı bir işlevi Logic Apps bir iş akışının parçası olarak katıştırmayı mümkün kılar. Zaman uyumsuz HTTP desenlerine yönelik Logic Apps desteği hakkında daha fazla ayrıntı [Azure Logic Apps iş akışı eylemleri ve Tetikleyiciler belgelerinde](../../logic-apps/logic-apps-workflow-actions-triggers.md#asynchronous-patterns)bulunabilir.

## <a name="http-api-reference"></a>HTTP API başvurusu

Uzantı tarafından uygulanan tüm HTTP API 'Leri aşağıdaki parametreleri alır. Tüm parametrelerin `string`veri türü.

| Parametre        | Parametre türü  | Açıklama |
|------------------|-----------------|-------------|
| **`taskHub`**    | Sorgu dizesi    | [Görev hub 'ının](durable-functions-task-hubs.md)adı. Belirtilmemişse, geçerli işlev uygulamasının görev hub 'ı adı varsayılır. |
| **`connection`** | Sorgu dizesi    | Depolama hesabı için bağlantı dizesinin **adı** . Belirtilmemişse, işlev uygulaması için varsayılan bağlantı dizesi varsayılır. |
| **`systemKey`**  | Sorgu dizesi    | API 'YI çağırmak için gereken yetkilendirme anahtarı. |

`systemKey`, Azure Işlevleri ana bilgisayarı tarafından otomatik olarak oluşturulan bir yetkilendirme anahtarıdır. Bu, özellikle dayanıklı görev uzantısı API 'Lerine erişim verir ve [diğer yetkilendirme anahtarlarıyla](https://github.com/Azure/azure-webjobs-sdk-script/wiki/Key-management-API)aynı şekilde yönetilebilir. `systemKey` Değeri keşfetmenin en kolay yolu, daha önce bahsedilen `CreateCheckStatusResponse` API 'yi kullanmaktır.

Sonraki birkaç bölümde uzantı tarafından desteklenen belirli HTTP API 'Leri ele alınmaktadır ve bunların nasıl kullanılacağına ilişkin örnekler sağlanmaktadır.

### <a name="get-instance-status"></a>Örnek durumunu al

Belirtilen bir düzenleme örneğinin durumunu alır.

#### <a name="request"></a>İstek

Işlevler çalışma zamanının 1. x sürümü için, istek aşağıdaki gibi biçimlendirilir (açıklık için birden fazla satır gösterilir):

```http
GET /admin/extensions/DurableTaskExtension/instances/{instanceId}
    ?taskHub={taskHub
    &connection={connectionName}
    &code={systemKey}
    &showHistory=[true|false]
    &showHistoryOutput=[true|false]
    &showInput=[true|false]
```

Işlevler çalışma zamanının 2. x sürümünde, URL biçimi aynı parametrelere sahip ancak biraz farklı bir ön eke sahiptir:

```http
GET /runtime/webhooks/durabletask/instances/{instanceId}
    ?taskHub={taskHub}
    &connection={connectionName}
    &code={systemKey}
    &showHistory=[true|false]
    &showHistoryOutput=[true|false]
    &showInput=[true|false]
```

Bu API için istek parametreleri, daha önce belirtilen varsayılan kümeyi ve aşağıdaki benzersiz parametreleri içerir:

| Alan                   | Parametre türü  | Açıklama |
|-------------------------|-----------------|-------------|
| **`instanceId`**        | URL             | Orchestration örneğinin KIMLIĞI. |
| **`showInput`**         | Sorgu dizesi    | İsteğe bağlı parametre. Olarak `false`ayarlanırsa, işlev girişi yanıt yüküne dahil edilmez.|
| **`showHistory`**       | Sorgu dizesi    | İsteğe bağlı parametre. Olarak `true`ayarlanırsa, Orchestration yürütme geçmişi yanıt yüküne dahil edilir.|
| **`showHistoryOutput`** | Sorgu dizesi    | İsteğe bağlı parametre. Olarak `true`ayarlanırsa, işlev çıkışları Orchestration yürütme geçmişine dahil edilir.|
| **`createdTimeFrom`**   | Sorgu dizesi    | İsteğe bağlı parametre. Belirtildiğinde, belirtilen ıSO8601 zaman damgasında veya bundan sonra oluşturulan döndürülen örneklerin listesini filtreler.|
| **`createdTimeTo`**     | Sorgu dizesi    | İsteğe bağlı parametre. Belirtildiğinde, belirtilen ıSO8601 zaman damgasında veya daha önce oluşturulan döndürülen örneklerin listesini filtreler.|
| **`runtimeStatus`**     | Sorgu dizesi    | İsteğe bağlı parametre. Belirtildiğinde, döndürülen örneklerin listesini çalışma zamanı durumlarına göre filtreler. Olası çalışma zamanı durum değerlerinin listesini görmek için, [örnekleri sorgulama](durable-functions-instance-management.md) konusuna bakın. |

#### <a name="response"></a>Yanıt

Olası birkaç durum kodu değeri döndürülebilir.

* **HTTP 200 (TAMAM)** : Belirtilen örnek tamamlanmış durumda.
* **HTTP 202 (kabul edildi)** : Belirtilen örnek devam ediyor.
* **HTTP 400 (hatalı istek)** : Belirtilen örnek başarısız oldu veya sonlandırıldı.
* **HTTP 404 (bulunamadı)** : Belirtilen örnek yok veya çalışmaya başlamadı.
* **HTTP 500 (Iç sunucu hatası)** : Belirtilen örnek işlenmeyen bir özel durumla başarısız oldu.

**Http 200** ve **http 202** örnekleri için yanıt yükü AŞAĞıDAKI alanları içeren bir JSON nesnesidir:

| Alan                 | Veri türü | Açıklama |
|-----------------------|-----------|-------------|
| **`runtimeStatus`**   | dize    | Örneğin çalışma zamanı durumu. Değerler *çalışıyor*, *bekliyor*, *başarısız*, *iptal edildi*, *sonlandırıldı*, *tamamlandı*olarak verilebilir. |
| **`input`**           | JSON      | Örneği başlatmak için kullanılan JSON verileri. Bu alan, `null` `showInput` sorgu dizesi parametresi olarak `false`ayarlanmışsa olur.|
| **`customStatus`**    | JSON      | Özel düzenleme durumu için kullanılan JSON verileri. Bu alan `null` ayarlanmamışsa. |
| **`output`**          | JSON      | Örneğin JSON çıktısı. Bu alan, `null` örneğin tamamlanmış durumda olmaması durumunda olur. |
| **`createdTime`**     | dize    | Örneğin oluşturulduğu zaman. ISO 8601 genişletilmiş gösterimini kullanır. |
| **`lastUpdatedTime`** | dize    | Örneğin son kalıcı olduğu zaman. ISO 8601 genişletilmiş gösterimini kullanır. |
| **`historyEvents`**   | JSON      | Orchestration yürütme geçmişini içeren bir JSON dizisi. Bu alan, `null` `showHistory` sorgu dizesi parametresi olarak `true`ayarlanmadığı müddetçe olur. |

Orchestration yürütme geçmişi ve etkinlik çıkışları (okunabilirlik için biçimlendirilir) dahil olmak üzere örnek bir yanıt yükü aşağıda verilmiştir:

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

**Http 202** yanıtı Ayrıca, daha önce bahsedilen `statusQueryGetUri` alanla aynı URL 'ye başvuran bir konum yanıt üst bilgisi içerir.

### <a name="get-all-instances-status"></a>Tüm örneklerin durumunu al

' Örnek durumunu Al ' isteğinden öğesini kaldırarak `instanceId` tüm örneklerin durumunu da sorgulayabilirsiniz. Bu durumda, temel parametreler ' Get Instance Status ' ile aynıdır. Filtreleme için sorgu dizesi parametreleri de desteklenir.

Anımsanması gereken `connection` tek şey, ve `code` isteğe bağlıdır. İşlev üzerinde anonim kimlik doğrulaması varsa, kod gerekli değildir.
AzureWebJobsStorage uygulama ayarında tanımlı dışında farklı bir depolama bağlantı dizesi kullanmak istemiyorsanız, bağlantı sorgusu dize parametresini güvenle yoksayabilirsiniz.

#### <a name="request"></a>İstek

Işlevler çalışma zamanının 1. x sürümü için, istek aşağıdaki gibi biçimlendirilir (açıklık için birden fazla satır gösterilir):

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

Işlevler çalışma zamanının 2. x sürümünde, URL biçimi aynı parametrelere sahip ancak biraz farklı bir ön eke sahiptir:

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

Bu API için istek parametreleri, daha önce belirtilen varsayılan kümeyi ve aşağıdaki benzersiz parametreleri içerir:

| Alan                   | Parametre türü  | Açıklama |
|-------------------------|-----------------|-------------|
| **`instanceId`**        | URL             | Orchestration örneğinin KIMLIĞI. |
| **`showInput`**         | Sorgu dizesi    | İsteğe bağlı parametre. Olarak `false`ayarlanırsa, işlev girişi yanıt yüküne dahil edilmez.|
| **`showHistory`**       | Sorgu dizesi    | İsteğe bağlı parametre. Olarak `true`ayarlanırsa, Orchestration yürütme geçmişi yanıt yüküne dahil edilir.|
| **`showHistoryOutput`** | Sorgu dizesi    | İsteğe bağlı parametre. Olarak `true`ayarlanırsa, işlev çıkışları Orchestration yürütme geçmişine dahil edilir.|
| **`createdTimeFrom`**   | Sorgu dizesi    | İsteğe bağlı parametre. Belirtildiğinde, belirtilen ıSO8601 zaman damgasında veya bundan sonra oluşturulan döndürülen örneklerin listesini filtreler.|
| **`createdTimeTo`**     | Sorgu dizesi    | İsteğe bağlı parametre. Belirtildiğinde, belirtilen ıSO8601 zaman damgasında veya daha önce oluşturulan döndürülen örneklerin listesini filtreler.|
| **`runtimeStatus`**     | Sorgu dizesi    | İsteğe bağlı parametre. Belirtildiğinde, döndürülen örneklerin listesini çalışma zamanı durumlarına göre filtreler. Olası çalışma zamanı durum değerlerinin listesini görmek için, [örnekleri sorgulama](durable-functions-instance-management.md) konusuna bakın. |
| **`top`**               | Sorgu dizesi    | İsteğe bağlı parametre. Belirtildiğinde, sorgu tarafından döndürülen örneklerin sayısını kısıtlar. |

#### <a name="response"></a>Yanıt

Düzenleme durumu da dahil olmak üzere yanıt yükleri örneği aşağıda verilmiştir (okunabilirlik için biçimlendirilir):

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
> Örnekler tablosunda çok sayıda satır varsa, bu işlem Azure Storage g/ç açısından çok pahalı olabilir. Örnek tablosu hakkında daha fazla ayrıntı, [dayanıklı işlevler (Azure işlevleri) belgelerinde performans ve ölçek](durable-functions-perf-and-scale.md#instances-table) bölümünde bulunabilir.
>

Daha fazla sonuç varsa, yanıt üst bilgisinde bir devamlılık belirteci döndürülür.  Üstbilginin `x-ms-continuation-token`adı.

Sonraki istek üstbilgisinde devamlılık belirteci değerini ayarlarsanız, sonraki sonuç sayfasına ulaşabilirsiniz. İstek üst bilgisinin bu adı da `x-ms-continuation-token`vardır.

### <a name="purge-single-instance-history"></a>Tek örnek geçmişini temizle

Belirtilen bir düzenleme örneği için geçmişi ve ilgili yapıtları siler.

#### <a name="request"></a>İstek

Işlevler çalışma zamanının 1. x sürümü için, istek aşağıdaki gibi biçimlendirilir (açıklık için birden fazla satır gösterilir):

```http
DELETE /admin/extensions/DurableTaskExtension/instances/{instanceId}
    ?taskHub={taskHub}
    &connection={connection}
    &code={systemKey}
```

Işlevler çalışma zamanının 2. x sürümünde, URL biçimi aynı parametrelere sahip ancak biraz farklı bir ön eke sahiptir:

```http
DELETE /runtime/webhooks/durabletask/instances/{instanceId}
    ?taskHub={taskHub}
    &connection={connection}
    &code={systemKey}
```

Bu API için istek parametreleri, daha önce belirtilen varsayılan kümeyi ve aşağıdaki benzersiz parametreleri içerir:

| Alan             | Parametre türü  | Açıklama |
|-------------------|-----------------|-------------|
| **`instanceId`**  | URL             | Orchestration örneğinin KIMLIĞI. |

#### <a name="response"></a>Yanıt

Aşağıdaki HTTP durum kodu değerleri döndürülebilir.

* **HTTP 200 (TAMAM)** : Örnek geçmişi başarıyla temizlendi.
* **HTTP 404 (bulunamadı)** : Belirtilen örnek yok.

**HTTP 200** Case için yanıt yükü aşağıdaki alanı IÇEREN bir JSON nesnesidir:

| Alan                  | Veri türü | Açıklama |
|------------------------|-----------|-------------|
| **`instancesDeleted`** | integer   | Silinen örneklerin sayısı. Tek örnekli durum için bu değer her zaman olmalıdır `1`. |

Örnek bir yanıt yükü (okunabilirlik için biçimlendirilir) aşağıda verilmiştir:

```json
{
    "instancesDeleted": 1
}
```

### <a name="purge-multiple-instance-history"></a>Birden çok örnek geçmişini temizle

Ayrıca, `{instanceId}` ' tek örnekli geçmiş ' isteğinden ' ı kaldırarak bir görev hub 'ında bulunan birden çok örnek için geçmişi ve ilgili yapıları silebilirsiniz. Örnek geçmişini seçmeli olarak temizlemek için ' tüm örnekleri al ' isteğinde açıklanan filtreleri kullanın.

#### <a name="request"></a>İstek

Işlevler çalışma zamanının 1. x sürümü için, istek aşağıdaki gibi biçimlendirilir (açıklık için birden fazla satır gösterilir):

```http
DELETE /admin/extensions/DurableTaskExtension/instances
    ?taskHub={taskHub}
    &connection={connectionName}
    &code={systemKey}
    &createdTimeFrom={timestamp}
    &createdTimeTo={timestamp}
    &runtimeStatus={runtimeStatus1,runtimeStatus2,...}
```

Işlevler çalışma zamanının 2. x sürümünde, URL biçimi aynı parametrelere sahip ancak biraz farklı bir ön eke sahiptir:

```http
DELETE /runtime/webhooks/durabletask/instances
    ?taskHub={taskHub}
    &connection={connectionName}
    &code={systemKey}
    &createdTimeFrom={timestamp}
    &createdTimeTo={timestamp}
    &runtimeStatus={runtimeStatus1,runtimeStatus2,...}
```

Bu API için istek parametreleri, daha önce belirtilen varsayılan kümeyi ve aşağıdaki benzersiz parametreleri içerir:

| Alan                 | Parametre türü  | Açıklama |
|-----------------------|-----------------|-------------|
| **`createdTimeFrom`** | Sorgu dizesi    | Verilen ıSO8601 zaman damgasında veya bundan sonra oluşturulan temizlenen örneklerin listesini filtreler.|
| **`createdTimeTo`**   | Sorgu dizesi    | İsteğe bağlı parametre. Belirtildiğinde, belirtilen ıSO8601 zaman damgasında veya daha önce oluşturulan temizlenen örneklerin listesini filtreler.|
| **`runtimeStatus`**   | Sorgu dizesi    | İsteğe bağlı parametre. Belirtildiğinde, temizlenen örneklerin listesini çalışma zamanı durumlarına göre filtreler. Olası çalışma zamanı durum değerlerinin listesini görmek için, [örnekleri sorgulama](durable-functions-instance-management.md) konusuna bakın. |

> [!NOTE]
> Örneklerde ve/veya geçmiş tablolarında çok sayıda satır varsa, bu işlem Azure Storage g/ç açısından çok pahalı olabilir. Bu tablolar hakkında daha fazla ayrıntı, [dayanıklı işlevler (Azure işlevleri) belgelerinde performans ve ölçek](durable-functions-perf-and-scale.md#instances-table) bölümünde bulunabilir.

#### <a name="response"></a>Yanıt

Aşağıdaki HTTP durum kodu değerleri döndürülebilir.

* **HTTP 200 (TAMAM)** : Örnek geçmişi başarıyla temizlendi.
* **HTTP 404 (bulunamadı)** : Filtre ifadesiyle eşleşen hiçbir örnek bulunamadı.

**HTTP 200** Case için yanıt yükü aşağıdaki alanı IÇEREN bir JSON nesnesidir:

| Alan                   | Veri türü | Açıklama |
|-------------------------|-----------|-------------|
| **`instancesDeleted`**  | integer   | Silinen örneklerin sayısı. |

Örnek bir yanıt yükü (okunabilirlik için biçimlendirilir) aşağıda verilmiştir:

```json
{
    "instancesDeleted": 250
}
```

### <a name="raise-event"></a>Olay oluştur

Çalışan bir düzenleme örneğine bir olay bildirim iletisi gönderir.

#### <a name="request"></a>İstek

Işlevler çalışma zamanının 1. x sürümü için, istek aşağıdaki gibi biçimlendirilir (açıklık için birden fazla satır gösterilir):

```http
POST /admin/extensions/DurableTaskExtension/instances/{instanceId}/raiseEvent/{eventName}
    ?taskHub={taskHub}
    &connection={connectionName}
    &code={systemKey}
```

Işlevler çalışma zamanının 2. x sürümünde, URL biçimi aynı parametrelere sahip ancak biraz farklı bir ön eke sahiptir:

```http
POST /runtime/webhooks/durabletask/instances/{instanceId}/raiseEvent/{eventName}
    ?taskHub={taskHub}
    &connection={connectionName}
    &code={systemKey}
```

Bu API için istek parametreleri, daha önce belirtilen varsayılan kümeyi ve aşağıdaki benzersiz parametreleri içerir:

| Alan             | Parametre türü  | Açıklama |
|-------------------|-----------------|-------------|
| **`instanceId`**  | URL             | Orchestration örneğinin KIMLIĞI. |
| **`eventName`**   | URL             | Hedef düzenleme örneğinin beklediği olayın adı. |
| **`{content}`**   | İstek içeriği | JSON biçimli olay yükü. |

#### <a name="response"></a>Yanıt

Olası birkaç durum kodu değeri döndürülebilir.

* **HTTP 202 (kabul edildi)** : Oluşturulan olay işleme için kabul edildi.
* **HTTP 400 (hatalı istek)** : İstek içeriği türünde `application/json` değil veya JSON geçerli değil.
* **HTTP 404 (bulunamadı)** : Belirtilen örnek bulunamadı.
* **HTTP 410 (gitti)** : Belirtilen örnek tamamlandı veya başarısız oldu ve herhangi bir oluşturulan olay işlenemiyor.

İşte bir olay adlı **işlem**için bekleyen bir örneğe JSON `"incr"` dizesi gönderen örnek bir istek:

```http
POST /admin/extensions/DurableTaskExtension/instances/bcf6fb5067b046fbb021b52ba7deae5a/raiseEvent/operation?taskHub=DurableFunctionsHub&connection=Storage&code=XXX
Content-Type: application/json
Content-Length: 6

"incr"
```

Bu API 'nin yanıtları herhangi bir içerik içermiyor.

### <a name="terminate-instance"></a>Örneği Sonlandır

Çalışan bir düzenleme örneğini sonlandırır.

#### <a name="request"></a>İstek

Işlevler çalışma zamanının 1. x sürümü için, istek aşağıdaki gibi biçimlendirilir (açıklık için birden fazla satır gösterilir):

```http
POST /admin/extensions/DurableTaskExtension/instances/{instanceId}/terminate
    ?taskHub={taskHub}
    &connection={connectionName}
    &code={systemKey}
    &reason={text}
```

Işlevler çalışma zamanının 2. x sürümünde, URL biçimi aynı parametrelere sahip ancak biraz farklı bir ön eke sahiptir:

```http
POST /runtime/webhooks/durabletask/instances/{instanceId}/terminate
    ?taskHub={taskHub}
    &connection={connectionName}
    &code={systemKey}
    &reason={text}
```

Bu API için istek parametreleri, daha önce belirtilen varsayılan kümeyi ve aşağıdaki benzersiz parametreyi içerir.

| Alan             | Parametre türü  | Açıklama |
|-------------------|-----------------|-------------|
| **`instanceId`**  | URL             | Orchestration örneğinin KIMLIĞI. |
| **`reason`**      | Sorgu dizesi    | İsteğe bağlı. Orchestration örneğini sonlandırma nedeni. |

#### <a name="response"></a>Yanıt

Olası birkaç durum kodu değeri döndürülebilir.

* **HTTP 202 (kabul edildi)** : Sonlandırma isteği işleme için kabul edildi.
* **HTTP 404 (bulunamadı)** : Belirtilen örnek bulunamadı.
* **HTTP 410 (gitti)** : Belirtilen örnek tamamlandı veya başarısız oldu.

İşte çalışan bir örneği sonlandıran ve **önemlidir**nedenini belirten bir örnek istek aşağıdadır:

```
POST /admin/extensions/DurableTaskExtension/instances/bcf6fb5067b046fbb021b52ba7deae5a/terminate?reason=buggy&taskHub=DurableFunctionsHub&connection=Storage&code=XXX
```

Bu API 'nin yanıtları herhangi bir içerik içermiyor.

### <a name="rewind-instance-preview"></a>Geri sarma örneği (Önizleme)

Hatalı düzenleme örneğini, en son başarısız işlemleri yeniden gerçekleştirerek çalışır duruma geri yükler.

#### <a name="request"></a>İstek

Işlevler çalışma zamanının 1. x sürümü için, istek aşağıdaki gibi biçimlendirilir (açıklık için birden fazla satır gösterilir):

```http
POST /admin/extensions/DurableTaskExtension/instances/{instanceId}/rewind
    ?taskHub={taskHub}
    &connection={connectionName}
    &code={systemKey}
    &reason={text}
```

Işlevler çalışma zamanının 2. x sürümünde, URL biçimi aynı parametrelere sahip ancak biraz farklı bir ön eke sahiptir:

```http
POST /runtime/webhooks/durabletask/instances/{instanceId}/rewind
    ?taskHub={taskHub}
    &connection={connectionName}
    &code={systemKey}
    &reason={text}
```

Bu API için istek parametreleri, daha önce belirtilen varsayılan kümeyi ve aşağıdaki benzersiz parametreyi içerir.

| Alan             | Parametre türü  | Açıklama |
|-------------------|-----------------|-------------|
| **`instanceId`**  | URL             | Orchestration örneğinin KIMLIĞI. |
| **`reason`**      | Sorgu dizesi    | İsteğe bağlı. Orchestration örneğini yeniden sargı nedeni. |

#### <a name="response"></a>Yanıt

Olası birkaç durum kodu değeri döndürülebilir.

* **HTTP 202 (kabul edildi)** : Geri sarma isteği işleme için kabul edildi.
* **HTTP 404 (bulunamadı)** : Belirtilen örnek bulunamadı.
* **HTTP 410 (gitti)** : Belirtilen örnek tamamlandı veya sonlandırıldı.

Aşağıda, başarısız bir örneği yeniden denetleyen ve bir **sabit**neden belirten örnek bir istek verilmiştir:

```http
POST /admin/extensions/DurableTaskExtension/instances/bcf6fb5067b046fbb021b52ba7deae5a/rewind?reason=fixed&taskHub=DurableFunctionsHub&connection=Storage&code=XXX
```

Bu API 'nin yanıtları herhangi bir içerik içermiyor.

### <a name="signal-entity-preview"></a>Sinyal varlığı (Önizleme)

[Dayanıklı bir varlığa](durable-functions-types-features-overview.md#entity-functions)tek yönlü bir işlem iletisi gönderir. Varlık yoksa, otomatik olarak oluşturulur.

#### <a name="request"></a>İstek

HTTP isteği aşağıdaki gibi biçimlendirilir (açıklık için birden fazla satır gösterilir):

```http
POST /runtime/webhooks/durabletask/entities/{entityType}/{entityKey}
    ?taskHub={taskHub}
    &connection={connectionName}
    &code={systemKey}
    &op={operationName}
```

Bu API için istek parametreleri, daha önce belirtilen varsayılan kümeyi ve aşağıdaki benzersiz parametreleri içerir:

| Alan             | Parametre türü  | Açıklama |
|-------------------|-----------------|-------------|
| **`entityType`**  | URL             | Varlığın türü. |
| **`entityKey`**   | URL             | Varlığın benzersiz adı. |
| **`op`**          | Sorgu dizesi    | İsteğe bağlı. Çağrılacak Kullanıcı tanımlı işlemin adı. |
| **`{content}`**   | İstek içeriği | JSON biçimli olay yükü. |

`Counter` Adlı`steps`bir varlığa Kullanıcı tanımlı "Ekle" iletisi gönderen örnek bir istek aşağıda verilmiştir. İletinin içeriği değerdir `5`. Varlık zaten mevcut değilse, bu istek tarafından oluşturulacaktır:

```http
POST /runtime/webhooks/durabletask/entities/Counter/steps?op=Add
Content-Type: application/json

5
```

#### <a name="response"></a>Yanıt

Bu işlemin birkaç olası yanıtı vardır:

* **HTTP 202 (kabul edildi)** : Sinyal işlemi zaman uyumsuz işleme için kabul edildi.
* **HTTP 400 (hatalı istek)** : İstek içeriği türünde `application/json`değildi, geçerli bir JSON değildi veya geçersiz `entityKey` bir değere sahipti.
* **HTTP 404 (bulunamadı)** : Belirtilen `entityType` bulunamadı.

Başarılı bir HTTP isteği yanıtta içerik içermiyor. Başarısız bir HTTP isteği, yanıt içeriğinde JSON biçimli hata bilgileri içerebilir.

### <a name="query-entity-preview"></a>Sorgu varlığı (Önizleme)

Belirtilen varlığın durumunu alır.

#### <a name="request"></a>İstek

HTTP isteği aşağıdaki gibi biçimlendirilir (açıklık için birden fazla satır gösterilir):

```http
GET /runtime/webhooks/durabletask/entities/{entityType}/{entityKey}
    ?taskHub={taskHub}
    &connection={connectionName}
    &code={systemKey}
```

#### <a name="response"></a>Yanıt

Bu işlemin iki olası yanıtı vardır:

* **HTTP 200 (TAMAM)** : Belirtilen varlık var.
* **HTTP 404 (bulunamadı)** : Belirtilen varlık bulunamadı.

Başarılı bir yanıt, varlığın içeriği olarak JSON seri hale getirilmiş durumunu içerir.

#### <a name="example"></a>Örnek
Aşağıda adlı `Counter` `steps`var olan bir varlığın durumunu alan bir http isteği örneği verilmiştir:

```http
GET /runtime/webhooks/durabletask/entities/Counter/steps
```

`Counter` Varlık bir`currentValue` alana kaydedilmiş birkaç adım içeriyorsa, yanıt içeriği aşağıdaki gibi görünebilir (okunabilirlik için biçimlendirilir):

```json
{
    "currentValue": 5
}
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Hataları nasıl işleyeceğinizi öğrenin](durable-functions-error-handling.md)
