---
title: Dayanıklı İşlevler-Azure 'da insan etkileşimi ve zaman aşımları
description: Azure Işlevleri için Dayanıklı İşlevler uzantısında insan etkileşimini ve zaman aşımlarını nasıl işleyeceğinizi öğrenin.
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: azfuncdf
ms.openlocfilehash: 4e0f71369bc02fdce5625d9c74e1d52264ed86be
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "80335754"
---
# <a name="human-interaction-in-durable-functions---phone-verification-sample"></a>Dayanıklı İşlevler-telefon doğrulama örneğindeki insan etkileşimi

Bu örnek, insan etkileşimini içeren [dayanıklı işlevler](durable-functions-overview.md) bir düzenleme oluşturmayı gösterir. Her gerçek kişi otomatik bir işleme dahil olduğunda, işlem kişiye bildirim gönderebilmeli ve yanıtları zaman uyumsuz olarak alamaz. Ayrıca, kişinin kullanılamama olasılığa izin vermelidir. (Bu son bölüm, zaman aşımlarının önemli hale geldiği yerdir.)

Bu örnek SMS tabanlı bir telefon doğrulama sistemi uygular. Bu akış türleri genellikle müşterinin telefon numarası doğrulanırken veya Multi-Factor Authentication (MFA) için kullanılır. Uygulamanın tamamı birkaç küçük işlev kullanılarak yapıldığından, bu güçlü bir örnektir. Veritabanı gibi dış veri deposu gerekli değildir.

[!INCLUDE [durable-functions-prerequisites](../../../includes/durable-functions-prerequisites.md)]

## <a name="scenario-overview"></a>Senaryoya genel bakış

Telefon doğrulaması, uygulamanızın son kullanıcılarının istenmeyen posta olmaması ve bunları söyledikleri kim olduğunu doğrulamak için kullanılır. Multi-Factor Authentication, Kullanıcı hesaplarını korsanlardan korumak için kullanılan yaygın bir kullanım durumdur. Kendi telefon doğrulamanızı uygulamayla ilgili zorluk, insanla ilgili **durum bilgisi** olan bir etkileşim gerektirmesidir. Son Kullanıcı genellikle bazı kodları (örneğin, 4 basamaklı bir sayı) ve **makul bir süre içinde**yanıt vermelidir.

Sıradan Azure Işlevleri durum bilgisi yoktur (diğer platformlarda birçok diğer bulut uç noktası olduğu gibi), bu tür etkileşimler bu şekilde bir veritabanında veya diğer kalıcı depolardaki durumu açıkça yönetme ile ilgilidir. Ayrıca, etkileşim birlikte koordine edilebilir birden çok işleve bölünmelidir. Örneğin, bir kod üzerinde seçim yapmak, bir yere kalıcı hale getirme ve kullanıcının telefonuna gönderilmesi için en az bir işleve ihtiyacınız vardır. Ayrıca, kullanıcıdan yanıt almak için en az bir diğer işleve ihtiyacınız vardır ve kod doğrulamasını yapmak için onu özgün işlev çağrısına geri eşleyin. Bir zaman aşımı, güvenliğin güvence altına almak için de önemli bir yönüdür. Hızlıca karmaşık olabilir.

Dayanıklı İşlevler kullandığınızda bu senaryonun karmaşıklığı büyük ölçüde azalır. Bu örnekte göreceğiniz gibi, bir Orchestrator işlevi, herhangi bir dış veri deposu eklemeden, durum bilgisi olan etkileşimi kolayca yönetebilir. Orchestrator işlevleri *dayanıklı*olduğundan, bu etkileşimli akışlar da son derece güvenilirdir.

## <a name="configuring-twilio-integration"></a>Twilio tümleştirmesini yapılandırma

[!INCLUDE [functions-twilio-integration](../../../includes/functions-twilio-integration.md)]

## <a name="the-functions"></a>İşlevler

Bu makalede örnek uygulamada aşağıdaki işlevler gösterilmektedir:

* `E4_SmsPhoneVerification`: Zaman aşımlarını ve yeniden denemeleri yönetme dahil olmak üzere telefon doğrulama işlemini gerçekleştiren bir [Orchestrator işlevi](durable-functions-bindings.md#orchestration-trigger) .
* `E4_SendSmsChallenge`: SMS mesajı aracılığıyla kod gönderen bir [etkinlik işlevi](durable-functions-bindings.md#activity-trigger) .

### <a name="e4_smsphoneverification-orchestrator-function"></a>E4_SmsPhoneVerification Orchestrator işlevi

# <a name="c"></a>[C#](#tab/csharp)

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/PhoneVerification.cs?range=17-70)]

> [!NOTE]
> İlk başta açık olmayabilir, ancak bu Orchestrator işlevi tamamen belirleyici olur. `CurrentUtcDateTime`Özelliğin Zamanlayıcı süre sonu süresini hesaplamak için kullanıldığı ve Orchestrator kodundaki bu noktada her yeniden yürütmeye aynı değeri döndürdüğü için belirleyici olur. Bu davranış, yinelenen her çağrının aynı sonuçlarının aynı olduğundan emin olmak için önemlidir `winner` `Task.WhenAny` .

# <a name="javascript"></a>[JavaScript](#tab/javascript)

**E4_SmsPhoneVerification** işlevi, Orchestrator işlevleri için *üzerinde standartfunction.js* kullanır.

[!code-json[Main](~/samples-durable-functions/samples/javascript/E4_SmsPhoneVerification/function.json)]

İşlevi uygulayan kod aşağıda verilmiştir:

[!code-javascript[Main](~/samples-durable-functions/samples/javascript/E4_SmsPhoneVerification/index.js)]

> [!NOTE]
> İlk başta açık olmayabilir, ancak bu Orchestrator işlevi tamamen belirleyici olur. `currentUtcDateTime`Özelliğin Zamanlayıcı süre sonu süresini hesaplamak için kullanıldığı ve Orchestrator kodundaki bu noktada her yeniden yürütmeye aynı değeri döndürdüğü için belirleyici olur. Bu davranış, yinelenen her çağrının aynı sonuçlarının aynı olduğundan emin olmak için önemlidir `winner` `context.df.Task.any` .

---

Bu Orchestrator işlevi başlatıldıktan sonra şunları yapar:

1. SMS bildirimini *gönderecek* telefon numarasını alır.
2. Kullanıcıya SMS iletisi göndermek için **E4_SendSmsChallenge** çağırır ve beklenen 4 basamaklı sınama kodunu geri döndürür.
3. Geçerli zamandan 90 saniye tetikleyen dayanıklı bir zamanlayıcı oluşturur.
4. Zamanlayıcı ile paralel olarak, kullanıcıdan **Smschallengeresbir** olay bekler.

Kullanıcı dört basamaklı kod içeren bir SMS mesajı alır. Doğrulama işlemini gerçekleştirmek için aynı dört basamaklı kodun Orchestrator işlev örneğine geri gönderilmesi 90 saniye sürer. Yanlış kodu gönderiyorlarsa, bu iki adım daha (aynı 90-ikinci pencere içinde) daha fazla üç denemeye sahip olur.

> [!WARNING]
> Sınama yanıtı kabul edildiğinde Yukarıdaki örnekte olduğu gibi, artık kullanım süreleri dolana kadar gerekmiyorsa, [zamanlayıcıları iptal etmeniz](durable-functions-timers.md) önemlidir.

## <a name="e4_sendsmschallenge-activity-function"></a>E4_SendSmsChallenge Activity işlevi

**E4_SendSmsChallenge** IşLEVI, SMS iletisini son kullanıcıya dört basamaklı kodla göndermek için Twilio bağlamasını kullanır.

# <a name="c"></a>[C#](#tab/csharp)

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/PhoneVerification.cs?range=72-89)]

> [!NOTE]
> `Microsoft.Azure.WebJobs.Extensions.Twilio`Örnek kodu çalıştırmak için NuGet paketini yüklemeniz gerekir.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

*function.js* , aşağıdaki gibi tanımlanır:

[!code-json[Main](~/samples-durable-functions/samples/javascript/E4_SendSmsChallenge/function.json)]

Dört basamaklı sınama kodu üreten ve SMS iletisini gönderen kod aşağıda verilmiştir:

[!code-javascript[Main](~/samples-durable-functions/samples/javascript/E4_SendSmsChallenge/index.js)]

---

## <a name="run-the-sample"></a>Örneği çalıştırma

Örneğe dahil edilen HTTP ile tetiklenen işlevleri kullanarak, aşağıdaki HTTP POST isteğini göndererek düzenleme işlemini başlatabilirsiniz:

```
POST http://{host}/orchestrators/E4_SmsPhoneVerification
Content-Length: 14
Content-Type: application/json

"+1425XXXXXXX"
```

```
HTTP/1.1 202 Accepted
Content-Length: 695
Content-Type: application/json; charset=utf-8
Location: http://{host}/runtime/webhooks/durabletask/instances/741c65651d4c40cea29acdd5bb47baf1?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}

{"id":"741c65651d4c40cea29acdd5bb47baf1","statusQueryGetUri":"http://{host}/runtime/webhooks/durabletask/instances/741c65651d4c40cea29acdd5bb47baf1?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}","sendEventPostUri":"http://{host}/runtime/webhooks/durabletask/instances/741c65651d4c40cea29acdd5bb47baf1/raiseEvent/{eventName}?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}","terminatePostUri":"http://{host}/runtime/webhooks/durabletask/instances/741c65651d4c40cea29acdd5bb47baf1/terminate?reason={text}&taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}"}
```

Orchestrator işlevi, sağlanan telefon numarasını alır ve anında rastgele oluşturulan 4 basamaklı bir doğrulama kodu içeren bir SMS iletisi gönderir &mdash; . Örneğin, *2168*. Bu işlev, yanıt için 90 saniye bekler.

Kodla yanıt vermek için, başka bir işlevin içinde [ `RaiseEventAsync` (.net) veya `raiseEvent` (JavaScript)](durable-functions-instance-management.md) kullanabilirsiniz veya yukarıdaki 202 YANıTıNDA başvurulan **sendEventUrl** http post Web kancasını çağırabilirsiniz, `{eventName}` olayın adıyla değiştirin `SmsChallengeResponse` :

```
POST http://{host}/runtime/webhooks/durabletask/instances/741c65651d4c40cea29acdd5bb47baf1/raiseEvent/SmsChallengeResponse?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}
Content-Length: 4
Content-Type: application/json

2168
```

Bunu zamanlayıcı süresi dolmadan önce gönderirseniz, düzenleme tamamlanır ve `output` alan `true` , başarılı bir doğrulama olduğunu belirten olarak ayarlanır.

```
GET http://{host}/runtime/webhooks/durabletask/instances/741c65651d4c40cea29acdd5bb47baf1?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}
```

```
HTTP/1.1 200 OK
Content-Length: 144
Content-Type: application/json; charset=utf-8

{"runtimeStatus":"Completed","input":"+1425XXXXXXX","output":true,"createdTime":"2017-06-29T19:10:49Z","lastUpdatedTime":"2017-06-29T19:12:23Z"}
```

Süreölçerin süresi sona ersin veya dört kez yanlış kod girerseniz, durumu sorgulayabilir ve `false` Telefon doğrulamasının başarısız olduğunu belirten bir düzenleme işlevi çıkışı görebilirsiniz.

```
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
Content-Length: 145

{"runtimeStatus":"Completed","input":"+1425XXXXXXX","output":false,"createdTime":"2017-06-29T19:20:49Z","lastUpdatedTime":"2017-06-29T19:22:23Z"}
```

## <a name="next-steps"></a>Sonraki adımlar

Bu örnek Dayanıklı İşlevler, özellikle ve API 'lerin gelişmiş özelliklerini göstermiştir `WaitForExternalEvent` `CreateTimer` . Bu `Task.WaitAny` kullanıcıların güvenilir bir zaman aşımı sistemi uygulamak için ile nasıl birleştirilebileceği gördünüz, bu da genellikle gerçek kişilerle etkileşim kurma açısından kullanışlıdır. Belirli konuların ayrıntılı kapsamını sunan bir dizi makaleyi okuyarak Dayanıklı İşlevler kullanma hakkında daha fazla bilgi edinebilirsiniz.

> [!div class="nextstepaction"]
> [Serideki ilk makaleye git](durable-functions-bindings.md)
