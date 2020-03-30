---
title: Dayanıklı İşlevlerde Monitörler - Azure
description: Azure İşlevler için Dayanıklı İşlevler uzantısını kullanarak durum monitörünü nasıl uygulayacağınızı öğrenin.
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: azfuncdf
ms.openlocfilehash: ed92156df9d8e1e07b56cea4b1e64edee11d68d9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77562131"
---
# <a name="monitor-scenario-in-durable-functions---weather-watcher-sample"></a>Dayanıklı Fonksiyonlar'da izleme senaryosu - Hava durumu izleyicisi örneği

İzleyici deseni, bir iş akışında esnek *bir yinelenen* işlemi ifade eder - örneğin, belirli koşullar karşılanana kadar yoklama. Bu makalede, izleme uygulamak için [Dayanıklı İşlevler](durable-functions-overview.md) kullanan bir örnek açıklanmaktadır.

[!INCLUDE [durable-functions-prerequisites](../../../includes/durable-functions-prerequisites.md)]

## <a name="scenario-overview"></a>Senaryoya genel bakış

Bu örnek, bir konumun mevcut hava koşullarını izler ve gökyüzü açık olduğunda kullanıcıyı SMS ile uyarır. Hava durumunu kontrol etmek ve uyarılar göndermek için düzenli bir zamanlayıcı tetikleme işlevi kullanabilirsiniz. Ancak, bu yaklaşım ile bir sorun **ömür boyu yönetim**. Yalnızca bir uyarı gönderilirse, net hava tespit edildikten sonra monitörün kendisini devre dışı bilmelidir. İzleme deseni, diğer faydaların yanı sıra kendi yürütme sona erebilir:

* Monitörler zamanlamaları değil, aralıklarla çalışır: zamanlayıcı tetikleyicisi her saat *çalışır;* bir monitör eylemler arasında bir saat *bekler.* Uzun süren görevler için önemli olabilecek, belirtilmediği sürece bir monitörün eylemleri çakışmaz.
* Monitörlerin dinamik aralıkları olabilir: bekleme süresi bazı koşullara bağlı olarak değişebilir.
* Bazı koşullar karşılandığında veya başka bir işlem tarafından sonlandırıldığında, izleyenler sonlandırılabilir.
* Monitörler parametreleri alabilir. Örnek, aynı hava durumu izleme işleminin istenen herhangi bir konum ve telefon numarasına nasıl uygulanabileceğini gösterir.
* Monitörler ölçeklenebilir. Her monitör bir düzenleme örneği olduğundan, yeni işlevler oluşturmak veya daha fazla kod tanımlamak zorunda kalmadan birden çok monitör oluşturulabilir.
* Monitörler daha büyük iş akışlarına kolayca entegre olabilir. Bir monitör, daha karmaşık bir orkestrasyon işlevinin bir bölümü veya bir [alt orkestrasyon](durable-functions-sub-orchestrations.md)olabilir.

## <a name="configuration"></a>Yapılandırma

### <a name="configuring-twilio-integration"></a>Twilio entegrasyonunun yapılandırılması

[!INCLUDE [functions-twilio-integration](../../../includes/functions-twilio-integration.md)]

### <a name="configuring-weather-underground-integration"></a>Weather Underground entegrasyonunun yapılandırılması

Bu örnek, bir konum için geçerli hava koşullarını kontrol etmek için Weather Underground API'sını kullanmayı içerir.

İhtiyacınız olan ilk şey bir Weather Underground hesabı. 'de [https://www.wunderground.com/signup](https://www.wunderground.com/signup)ücretsiz olarak bir tane oluşturabilirsiniz. Bir hesabınız olduktan sonra, bir API anahtarı edinmeniz gerekir. Bunu ziyaret [https://www.wunderground.com/weather/api](https://www.wunderground.com/weather/api/?MR=1)ederek, ardından Anahtar Ayarları'nı seçerek yapabilirsiniz. Stratus Geliştirici planı ücretsizdir ve bu örneği çalıştırmak için yeterlidir.

BIR API anahtarına sahip olduktan sonra, işlev uygulamanıza aşağıdaki **uygulama ayarını** ekleyin.

| Uygulama ayar adı | Değer açıklaması |
| - | - |
| **Hava DurumuUndergroundApiKey**  | Weather Underground API anahtarınız. |

## <a name="the-functions"></a>Fonksiyonlar

Bu makalede, örnek uygulamada aşağıdaki işlevler açıklanmaktadır:

* `E3_Monitor`: Periyodik olarak çağıran `E3_GetIsClear` bir [orkestratör işlevi.](durable-functions-bindings.md#orchestration-trigger) Eğer `E3_SendGoodWeatherAlert` `E3_GetIsClear` doğru dönerse arar.
* `E3_GetIsClear`: Bir konum için geçerli hava koşullarını kontrol eden bir [etkinlik işlevi.](durable-functions-bindings.md#activity-trigger)
* `E3_SendGoodWeatherAlert`: Twilio üzerinden SMS mesajı gönderen bir etkinlik işlevi.

### <a name="e3_monitor-orchestrator-function"></a>E3_Monitor orkestratör fonksiyonu

# <a name="c"></a>[C #](#tab/csharp)

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/Monitor.cs?range=41-78,97-115)]

Orkestratör, izlenecek bir konum ve konumda netleşip netleşmeyeceğine mesaj göndermek için bir telefon numarası gerektirir. Bu veriler, güçlü bir şekilde yazılan `MonitorRequest` bir nesne olarak orkestratöre aktarılır.

# <a name="javascript"></a>[Javascript](#tab/javascript)

**E3_Monitor** işlevi, orkestratör işlevleri için standart *function.json* kullanır.

[!code-json[Main](~/samples-durable-functions/samples/javascript/E3_Monitor/function.json)]

İşlevi uygulayan kod aşağıda veda eder:

[!code-javascript[Main](~/samples-durable-functions/samples/javascript/E3_Monitor/index.js)]

---

Bu orchestrator işlevi aşağıdaki eylemleri gerçekleştirir:

1. İzlenecek *konum* ve SMS bildirimi göndereceği *telefon numarasını* içeren **MonitorRequest'ı** alır.
2. Monitörün son kullanma süresini belirler. Örnek, kısalık için sabit kodlanmış bir değer kullanır.
3. İstenen yerde açık gökyüzü olup olmadığını belirlemek için **E3_GetIsClear** arar.
4. Hava açıksa, aramalar istenen telefon numarasına SMS bildirimi göndermek için **E3_SendGoodWeatherAlert.**
5. Bir sonraki yoklama aralığında orkestrasyon devam etmek için dayanıklı bir zamanlayıcı oluşturur. Örnek, kısalık için sabit kodlanmış bir değer kullanır.
6. Geçerli UTC süresi monitörün son kullanma süresi geçene veya SMS uyarısı gönderilene kadar çalışmaya devam eder.

Birden çok orkestratör örnekleri aynı anda birden çok kez orchestrator işlevini çağırarak çalıştırabilirsiniz. İzlenecek yer ve SMS uyarısı gönderilecek telefon numarası belirtilebilir.

### <a name="e3_getisclear-activity-function"></a>E3_GetIsClear etkinlik fonksiyonu

Diğer örneklerde olduğu gibi, yardımcı etkinlik işlevleri `activityTrigger` tetikleyici bağlamayı kullanan düzenli işlevlerdir. **E3_GetIsClear** fonksiyonu Weather Underground API kullanarak mevcut hava koşullarını alır ve gökyüzünün açık olup olmadığını belirler.

# <a name="c"></a>[C #](#tab/csharp)

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/Monitor.cs?range=80-85)]

# <a name="javascript"></a>[Javascript](#tab/javascript)

*Function.json* aşağıdaki gibi tanımlanır:

[!code-json[Main](~/samples-durable-functions/samples/javascript/E3_GetIsClear/function.json)]

Ve burada uygulamadır.

[!code-javascript[Main](~/samples-durable-functions/samples/javascript/E3_GetIsClear/index.js)]

---

### <a name="e3_sendgoodweatheralert-activity-function"></a>E3_SendGoodWeatherAlert etkinlik fonksiyonu

**E3_SendGoodWeatherAlert** işlevi, son kullanıcıya yürüyüş için iyi bir zaman olduğunu bildiren bir SMS iletisi göndermek için Twilio bağlamayı kullanır.

# <a name="c"></a>[C #](#tab/csharp)

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/Monitor.cs?range=87-96,140-205)]

> [!NOTE]
> Örnek kodu çalıştırmak `Microsoft.Azure.WebJobs.Extensions.Twilio` için Nuget paketini yüklemeniz gerekir.

# <a name="javascript"></a>[Javascript](#tab/javascript)

Onun *fonksiyonu.json* basittir:

[!code-json[Main](~/samples-durable-functions/samples/javascript/E3_SendGoodWeatherAlert/function.json)]

Ve burada SMS mesajı gönderir kodu:

[!code-javascript[Main](~/samples-durable-functions/samples/javascript/E3_SendGoodWeatherAlert/index.js)]

---

## <a name="run-the-sample"></a>Örneği çalıştırma

Örnekte yer alan HTTP tetikleme işlevlerini kullanarak, aşağıdaki HTTP POST isteğini göndererek orkestrasyona başlayabilirsiniz:

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

**E3_Monitor** örneği başlar ve istenen konumun geçerli hava koşullarını sorgular. Hava açıksa, bir uyarı göndermek için bir etkinlik işlevi çağırır; aksi takdirde, bir zamanlayıcı ayarlar. Zamanlayıcının süresi dolduğunda, orkestrasyon devam eder.

Azure İşlevler portalındaki işlev günlüklerine bakarak orkestrasyonun etkinliğini görebilirsiniz.

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

Zaman aşılması veya açık gökyüzü tespit edildikten sonra orkestrasyon [sona erer.](durable-functions-instance-management.md) Ayrıca (.NET) `TerminateAsync` veya `terminate` (JavaScript) başka bir işlev içinde kullanabilirsiniz veya fesih için neden ile değiştirerek, `{text}` yukarıdaki 202 yanıtatıfta **terminatePostUri** HTTP POST webhook çağırmak:

```
POST https://{host}/runtime/webhooks/durabletask/instances/f6893f25acf64df2ab53a35c09d52635/terminate?reason=Because&taskHub=SampleHubVS&connection=Storage&code={systemKey}
```

## <a name="next-steps"></a>Sonraki adımlar

Bu örnek, [dayanıklı zamanlayıcılar](durable-functions-timers.md) ve koşullu mantık kullanarak harici bir kaynağın durumunu izlemek için Dayanıklı Fonksiyonlar'ın nasıl kullanılacağını göstermiştir. Sonraki örnek, insan etkileşimini işlemek için dış olayların ve [dayanıklı zamanlayıcıların](durable-functions-timers.md) nasıl kullanılacağını gösterir.

> [!div class="nextstepaction"]
> [İnsan etkileşim örneğini çalıştırın](durable-functions-phone-verification.md)
