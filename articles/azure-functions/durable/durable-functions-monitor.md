---
title: Dayanıklı İşlevler izleyiciler-Azure
description: Azure Işlevleri için Dayanıklı İşlevler uzantısını kullanarak bir durum İzleyicisi uygulamayı öğrenin.
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: azfuncdf
ms.openlocfilehash: ed92156df9d8e1e07b56cea4b1e64edee11d68d9
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "77562131"
---
# <a name="monitor-scenario-in-durable-functions---weather-watcher-sample"></a>Dayanıklı İşlevler-Hava durumu izleyici örneğinde izleme senaryosu

İzleyici stili, bir iş akışında esnek bir *yinelenen* işleme anlamına gelir. Örneğin, belirli koşullar karşılanana kadar yoklama yapar. Bu makalede, izlemeyi uygulamak için [dayanıklı işlevler](durable-functions-overview.md) kullanan bir örnek açıklanmaktadır.

[!INCLUDE [durable-functions-prerequisites](../../../includes/durable-functions-prerequisites.md)]

## <a name="scenario-overview"></a>Senaryoya genel bakış

Bu örnek, bir konumun geçerli hava durumu koşullarını izler ve skıes açık olduğunda bir kullanıcıyı SMS ile uyarır. Hava durumunu denetlemek ve uyarı göndermek için normal bir Zamanlayıcı tarafından tetiklenen bir işlev kullanabilirsiniz. Ancak, Bu yaklaşımdaki bir sorun **ömür yönetirüdir**. Yalnızca bir uyarı gönderilmesi gerekiyorsa, izleyicinin açık hava durumu algılandıktan sonra kendisini devre dışı bırakması gerekir. İzleme deseninin diğer avantajları arasında kendi yürütmesi sona erdirmek için:

* İzleyiciler, zamanlama değil, aralıklar üzerinde çalışır: bir Zamanlayıcı tetikleyicisi her saat *çalışır* ; izleyici, eylemler arasında bir saat *bekler* . Bir izleyicinin eylemleri belirtilmediyse, bu işlemler, uzun süre çalışan görevler için önemli olabilen, belirtilmediği sürece örtüşmeyecektir.
* İzleyicilerin dinamik aralıkları olabilir: bekleme süresi bazı koşula göre değişebilir.
* İzleyiciler, bazı koşullar karşılandığında veya başka bir işlem tarafından sonlandırılırsa sonlandırılabilir.
* İzleyiciler parametre alabilir. Örnek, aynı Hava durumu izleme işleminin istenen konuma ve telefon numarasına nasıl uygulanabileceğini gösterir.
* İzleyiciler ölçeklenebilir. Her izleyici bir düzenleme örneği olduğundan, yeni işlevler oluşturmak veya daha fazla kod tanımlamak zorunda kalmadan birden fazla izleyici oluşturulabilir.
* İzleyiciler daha büyük iş akışlarıyla kolayca tümleşir. İzleyici, daha karmaşık bir düzenleme işlevinin veya [alt Orchestration](durable-functions-sub-orchestrations.md)'un bir bölümü olabilir.

## <a name="configuration"></a>Yapılandırma

### <a name="configuring-twilio-integration"></a>Twilio tümleştirmesini yapılandırma

[!INCLUDE [functions-twilio-integration](../../../includes/functions-twilio-integration.md)]

### <a name="configuring-weather-underground-integration"></a>Hava durumu yetersiz tümleştirmesini yapılandırma

Bu örnek, bir konum için geçerli hava durumu koşullarını denetlemek için hava durumu düşük olan API 'sini kullanmayı içerir.

İhtiyaç duyduğunuz ilk şey, hava durumu düşük bir hesaptır. ' De ücretsiz olarak bir tane oluşturabilirsiniz [https://www.wunderground.com/signup](https://www.wunderground.com/signup) . Hesabınız olduktan sonra bir API anahtarı edinmeniz gerekir. Bunu [https://www.wunderground.com/weather/api](https://www.wunderground.com/weather/api/?MR=1) , sonra anahtar ayarları ' nı ziyaret ederek yapabilirsiniz. Bu örneği çalıştırmak için, Stratler geliştirici planı ücretsizdir ve yeterlidir.

Bir API anahtarınız olduğunda, işlev uygulamanıza aşağıdaki **uygulama ayarını** ekleyin.

| Uygulama ayarı adı | Değer açıklaması |
| - | - |
| **Dalgalı Therundergroundadıkey**  | Hava durumu düşük API anahtarınız. |

## <a name="the-functions"></a>İşlevler

Bu makalede örnek uygulamada aşağıdaki işlevler açıklanmaktadır:

* `E3_Monitor`: Düzenli aralıklarla çağıran bir [Orchestrator işlevi](durable-functions-bindings.md#orchestration-trigger) `E3_GetIsClear` . `E3_SendGoodWeatherAlert` `E3_GetIsClear` Doğru döndürürse çağırır.
* `E3_GetIsClear`: Bir konum için geçerli hava durumu koşullarını denetleyen bir [etkinlik işlevi](durable-functions-bindings.md#activity-trigger) .
* `E3_SendGoodWeatherAlert`: Twilio aracılığıyla SMS iletisi gönderen bir etkinlik işlevi.

### <a name="e3_monitor-orchestrator-function"></a>E3_Monitor Orchestrator işlevi

# <a name="c"></a>[C#](#tab/csharp)

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/Monitor.cs?range=41-78,97-115)]

Orchestrator, konum üzerinde açık hale geldiğinde bir ileti göndermek için bir konum ve bir telefon numarası gerektirir. Bu veriler, kesin olarak belirlenmiş bir nesne olarak Orchestrator 'a geçirilir `MonitorRequest` .

# <a name="javascript"></a>[JavaScript](#tab/javascript)

**E3_Monitor** işlevi, Orchestrator işlevleri için *üzerinde standartfunction.js* kullanır.

[!code-json[Main](~/samples-durable-functions/samples/javascript/E3_Monitor/function.json)]

İşlevi uygulayan kod aşağıda verilmiştir:

[!code-javascript[Main](~/samples-durable-functions/samples/javascript/E3_Monitor/index.js)]

---

Bu Orchestrator işlevi aşağıdaki eylemleri gerçekleştirir:

1. İzlenecek *konumdan* ve SMS bildirimi gönderecek *telefon numarasına* sahip olan **monitorrequest** 'i alır.
2. İzleyicinin sona erme süresini belirler. Örnek, breçekimi için sabit kodlanmış bir değer kullanır.
3. İstenen konumda açık skiler olup olmadığını anlamak için **E3_GetIsClear** çağırır.
4. Hava durumu açık ise, istenen telefon numarasına SMS bildirimi göndermek için **E3_SendGoodWeatherAlert** çağırır.
5. Bir sonraki yoklama aralığında Orchestration işlemini sürdürecek dayanıklı bir zamanlayıcı oluşturur. Örnek, breçekimi için sabit kodlanmış bir değer kullanır.
6. Geçerli UTC saati izleyicinin sona erme zamanını geçirene veya bir SMS uyarısı gönderildiğinde çalışmaya devam eder.

Orchestrator işlevini birden çok kez çağırarak, birden fazla Orchestrator örneği aynı anda çalışabilir. İzlenecek konum ve SMS uyarısı gönderilecek telefon numarası belirtilebilir.

### <a name="e3_getisclear-activity-function"></a>E3_GetIsClear Activity işlevi

Diğer örneklerde olduğu gibi, yardımcı etkinlik işlevleri de tetikleyici bağlamayı kullanan normal işlevlerdir `activityTrigger` . **E3_GetIsClear** Işlevi, hava durumu düşük olan API 'yi kullanarak geçerli hava durumu koşullarını alır ve çatonun açık olup olmadığını belirler.

# <a name="c"></a>[C#](#tab/csharp)

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/Monitor.cs?range=80-85)]

# <a name="javascript"></a>[JavaScript](#tab/javascript)

*function.js* , aşağıdaki gibi tanımlanır:

[!code-json[Main](~/samples-durable-functions/samples/javascript/E3_GetIsClear/function.json)]

İşte uygulama.

[!code-javascript[Main](~/samples-durable-functions/samples/javascript/E3_GetIsClear/index.js)]

---

### <a name="e3_sendgoodweatheralert-activity-function"></a>E3_SendGoodWeatherAlert Activity işlevi

**E3_SendGoodWeatherAlert** işlevi, son kullanıcıya bir ilerleme için iyi bir zaman olduğunu BILDIREN bir SMS iletisi göndermek için Twilio bağlamasını kullanır.

# <a name="c"></a>[C#](#tab/csharp)

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/Monitor.cs?range=87-96,140-205)]

> [!NOTE]
> `Microsoft.Azure.WebJobs.Extensions.Twilio`Örnek kodu çalıştırmak için NuGet paketini yüklemeniz gerekir.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

*Üzerindefunction.js* basittir:

[!code-json[Main](~/samples-durable-functions/samples/javascript/E3_SendGoodWeatherAlert/function.json)]

SMS iletisini gönderen kod aşağıda verilmiştir:

[!code-javascript[Main](~/samples-durable-functions/samples/javascript/E3_SendGoodWeatherAlert/index.js)]

---

## <a name="run-the-sample"></a>Örneği çalıştırma

Örneğe dahil edilen HTTP ile tetiklenen işlevleri kullanarak, aşağıdaki HTTP POST isteğini göndererek düzenleme işlemini başlatabilirsiniz:

```
POST https://{host}/orchestrators/E3_Monitor
Content-Length: 77
Content-Type: application/json

{ "location": { "city": "Redmond", "state": "WA" }, "phone": "+1425XXXXXXX" }
```

```
HTTP/1.1 202 Accepted
Content-Type: application/json; charset=utf-8
Location: https://{host}/runtime/webhooks/durabletask/instances/f6893f25acf64df2ab53a35c09d52635?taskHub=SampleHubVS&connection=Storage&code={SystemKey}
RetryAfter: 10

{"id": "f6893f25acf64df2ab53a35c09d52635", "statusQueryGetUri": "https://{host}/runtime/webhooks/durabletask/instances/f6893f25acf64df2ab53a35c09d52635?taskHub=SampleHubVS&connection=Storage&code={systemKey}", "sendEventPostUri": "https://{host}/runtime/webhooks/durabletask/instances/f6893f25acf64df2ab53a35c09d52635/raiseEvent/{eventName}?taskHub=SampleHubVS&connection=Storage&code={systemKey}", "terminatePostUri": "https://{host}/runtime/webhooks/durabletask/instances/f6893f25acf64df2ab53a35c09d52635/terminate?reason={text}&taskHub=SampleHubVS&connection=Storage&code={systemKey}"}
```

**E3_Monitor** örnek başlatılır ve istenen konum için geçerli hava durumu koşullarını sorgular. Hava durumu açık ise, uyarı göndermek için bir etkinlik işlevi çağırır; Aksi takdirde, bir Zamanlayıcı ayarlar. Süreölçerin süresi dolduğunda düzenleme sürdürülecek.

Azure Işlevleri portalındaki işlev günlüklerine bakarak Orchestration 'un etkinliğini görebilirsiniz.

```
2018-03-01T01:14:41.649 Function started (Id=2d5fcadf-275b-4226-a174-f9f943c90cd1)
2018-03-01T01:14:42.741 Started orchestration with ID = '1608200bb2ce4b7face5fc3b8e674f2e'.
2018-03-01T01:14:42.780 Function completed (Success, Id=2d5fcadf-275b-4226-a174-f9f943c90cd1, Duration=1111ms)
2018-03-01T01:14:52.765 Function started (Id=b1b7eb4a-96d3-4f11-a0ff-893e08dd4cfb)
2018-03-01T01:14:52.890 Received monitor request. Location: Redmond, WA. Phone: +1425XXXXXXX.
2018-03-01T01:14:52.895 Instantiating monitor for Redmond, WA. Expires: 3/1/2018 7:14:52 AM.
2018-03-01T01:14:52.909 Checking current weather conditions for Redmond, WA at 3/1/2018 1:14:52 AM.
2018-03-01T01:14:52.954 Function completed (Success, Id=b1b7eb4a-96d3-4f11-a0ff-893e08dd4cfb, Duration=189ms)
2018-03-01T01:14:53.226 Function started (Id=80a4cb26-c4be-46ba-85c8-ea0c6d07d859)
2018-03-01T01:14:53.808 Function completed (Success, Id=80a4cb26-c4be-46ba-85c8-ea0c6d07d859, Duration=582ms)
2018-03-01T01:14:53.967 Function started (Id=561d0c78-ee6e-46cb-b6db-39ef639c9a2c)
2018-03-01T01:14:53.996 Next check for Redmond, WA at 3/1/2018 1:44:53 AM.
2018-03-01T01:14:54.030 Function completed (Success, Id=561d0c78-ee6e-46cb-b6db-39ef639c9a2c, Duration=62ms)
```

Zaman aşımı süresine ulaşıldığında düzenleme [sonlandırılır](durable-functions-instance-management.md) veya temizleme işlemi algılanır. Ayrıca, `TerminateAsync` (.net) veya `terminate` (JavaScript) öğesini başka bir işlev içinde kullanabilir veya yukarıdaki 202 yanıtında başvurulan **terminateposturi** http post Web kancasını çağırarak `{text}` sonlandırma nedeni ile değiştirin:

```
POST https://{host}/runtime/webhooks/durabletask/instances/f6893f25acf64df2ab53a35c09d52635/terminate?reason=Because&taskHub=SampleHubVS&connection=Storage&code={systemKey}
```

## <a name="next-steps"></a>Sonraki adımlar

Bu örnekte, bir dış kaynağın durumunu [dayanıklı zamanlayıcılar](durable-functions-timers.md) ve koşullu mantık kullanarak izlemek için dayanıklı işlevler nasıl kullanılacağı gösterilmiştir. Sonraki örnek, insan etkileşimini işlemek için dış olayların ve [dayanıklı zamanlayıcıları](durable-functions-timers.md) nasıl kullanacağınızı gösterir.

> [!div class="nextstepaction"]
> [İnsan etkileşimi örneğini çalıştırma](durable-functions-phone-verification.md)
