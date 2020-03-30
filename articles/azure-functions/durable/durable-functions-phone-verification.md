---
title: Dayanıklı İşlevlerde Insan etkileşimi ve zaman ekmeleri - Azure
description: Azure İşlevler için Dayanıklı Fonksiyonlar uzantısında insan etkileşimi ve zaman ekmeleriyle nasıl başa çıkacağınızı öğrenin.
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: azfuncdf
ms.openlocfilehash: 4e0f71369bc02fdce5625d9c74e1d52264ed86be
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80335754"
---
# <a name="human-interaction-in-durable-functions---phone-verification-sample"></a>Dayanıklı Fonksiyonlarda İnsan etkileşimi - Telefon doğrulama örneği

Bu örnek, insan etkileşimini içeren dayanıklı [fonksiyonlar](durable-functions-overview.md) orkestrasyonunun nasıl inşa edilebildiğini göstermektedir. Gerçek bir kişi otomatik bir işleme dahil olduğunda, işlem kişiye bildirim gönderebilmeli ve eşit olarak yanıt alabilmeli. Ayrıca kişinin kullanılamaması olasılığına da izin vermelidir. (Bu son bölüm zaman zaman eklerinin önemli hale geldiği yerdir.)

Bu örnek, SMS tabanlı telefon doğrulama sistemi uygular. Bu tür akışlar genellikle müşterinin telefon numarasını doğrularken veya çok faktörlü kimlik doğrulaması (MFA) için kullanılır. Tüm uygulama birkaç küçük işlevleri kullanılarak yapılır, çünkü güçlü bir örnektir. Veritabanı gibi hiçbir dış veri deposu gerekli değildir.

[!INCLUDE [durable-functions-prerequisites](../../../includes/durable-functions-prerequisites.md)]

## <a name="scenario-overview"></a>Senaryoya genel bakış

Telefon doğrulaması, uygulamanızın son kullanıcılarının spam gönderen ler olmadığını ve söyledikleri kişiler olduklarını doğrulamak için kullanılır. Çok faktörlü kimlik doğrulama, kullanıcı hesaplarını bilgisayar korsanlarından korumak için yaygın bir kullanım örneğidir. Kendi telefon doğrulama uygulama ile sorun bir insan ile **devletli** bir etkileşim gerektirir olmasıdır. Son kullanıcıya genellikle bazı kod (örneğin, 4 basamaklı bir sayı) sağlanır ve **makul bir süre içinde**yanıt vermesi gerekir.

Sıradan Azure İşlevleri (diğer platformlardaki diğer birçok bulut uç noktası gibi) durum ifadesüztür, bu nedenle bu tür etkileşimler, bir veritabanında veya başka bir kalıcı depoda durumu açıkça dışarıdan yönetmeyi içerir. Buna ek olarak, etkileşim birlikte koordine edilebilir birden çok fonksiyona bölünmelidir. Örneğin, bir koda karar vermek, bir yerde kalıcı hale getirmek ve kullanıcının telefonuna göndermek için en az bir işlev gerekir. Ayrıca, kullanıcıdan yanıt almak ve kod doğrulaması yapmak için bir şekilde orijinal işlev çağrısına geri eşlemek için en az bir başka işleve ihtiyacınız vardır. Zaman alabilmeleri de güvenliği sağlamak için önemli bir unsurdur. Oldukça hızlı bir şekilde karmaşık alabilirsiniz.

Dayanıklı İşlevler kullandığınızda bu senaryonun karmaşıklığı büyük ölçüde azalır. Bu örnekte göreceğiniz gibi, bir orkestratör işlevi durumlu etkileşimi herhangi bir dış veri deposunu dahil etmeden kolayca yönetebilir. Orkestratör işlevleri *dayanıklı*olduğundan, bu etkileşimli akışlar da son derece güvenilirdir.

## <a name="configuring-twilio-integration"></a>Twilio entegrasyonunun yapılandırılması

[!INCLUDE [functions-twilio-integration](../../../includes/functions-twilio-integration.md)]

## <a name="the-functions"></a>Fonksiyonlar

Bu makalede, örnek uygulamada aşağıdaki işlevler üzerinden yürür:

* `E4_SmsPhoneVerification`: Zaman ekmelerini ve yeniden denemeleri yönetmek de dahil olmak üzere telefon doğrulama işlemini gerçekleştiren bir [orkestratör işlevi.](durable-functions-bindings.md#orchestration-trigger)
* `E4_SendSmsChallenge`: Kısa mesaj la kod gönderen bir [etkinlik işlevi.](durable-functions-bindings.md#activity-trigger)

### <a name="e4_smsphoneverification-orchestrator-function"></a>E4_SmsPhoneVerification orkestratör fonksiyonu

# <a name="c"></a>[C #](#tab/csharp)

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/PhoneVerification.cs?range=17-70)]

> [!NOTE]
> İlk başta belli olmayabilir, ama bu orkestratör işlevi tamamen belirleyicidir. `CurrentUtcDateTime` Özellik zamanlayıcı son kullanma süresini hesaplamak için kullanıldığından ve orkestratör kodunun bu noktasındaki her tekrarda aynı değeri döndürdüğünden deterministiktir. Bu davranış, her tekrarlanan `winner` aramadan `Task.WhenAny`aynı sonuçların .

# <a name="javascript"></a>[Javascript](#tab/javascript)

**E4_SmsPhoneVerification** işlevi, orkestratör işlevleri için standart *function.json* kullanır.

[!code-json[Main](~/samples-durable-functions/samples/javascript/E4_SmsPhoneVerification/function.json)]

İşlevi uygulayan kod aşağıda veda eder:

[!code-javascript[Main](~/samples-durable-functions/samples/javascript/E4_SmsPhoneVerification/index.js)]

> [!NOTE]
> İlk başta belli olmayabilir, ama bu orkestratör işlevi tamamen belirleyicidir. `currentUtcDateTime` Özellik zamanlayıcı son kullanma süresini hesaplamak için kullanıldığından ve orkestratör kodunun bu noktasındaki her tekrarda aynı değeri döndürdüğünden deterministiktir. Bu davranış, her tekrarlanan `winner` aramadan `context.df.Task.any`aynı sonuçların .

---

Bir kez başladıktan sonra, bu orkestratör işlevi aşağıdakileri yapar:

1. SMS bildirimini *göndereceği* bir telefon numarası alır.
2. Kullanıcıya SMS iletisi göndermek için **E4_SendSmsChallenge** çağırır ve beklenen 4 basamaklı meydan okuma kodunu geri döndürür.
3. Geçerli zamandan 90 saniye tetikleyen dayanıklı bir zamanlayıcı oluşturur.
4. Zamanlayıcıya paralel olarak, kullanıcıdan **smsChallengeResponse** olay bekler.

Kullanıcı, dört basamaklı bir kodiçeren bir SMS iletisi alır. Doğrulama işlemini tamamlamak için aynı dört basamaklı kodu orkestratör işlev örneğine geri göndermek için 90 saniyeleri vardır. Yanlış kod gönderirlerse, doğru yapmak için üç deneme daha yaparlar (aynı 90 saniyelik pencere içinde).

> [!WARNING]
> Yukarıdaki örnekte olduğu gibi, bir meydan okuma yanıtı kabul edildiğinde, zamanlayıcıların süresinin dolmasına gerek yoksa, [zamanlayıcıları iptal](durable-functions-timers.md) etmek önemlidir.

## <a name="e4_sendsmschallenge-activity-function"></a>E4_SendSmsChallenge etkinlik fonksiyonu

**E4_SendSmsChallenge** işlevi, son kullanıcıya dört basamaklı kodiçeren SMS iletisini göndermek için Twilio bağlamayı kullanır.

# <a name="c"></a>[C #](#tab/csharp)

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/PhoneVerification.cs?range=72-89)]

> [!NOTE]
> Örnek kodu çalıştırmak `Microsoft.Azure.WebJobs.Extensions.Twilio` için Nuget paketini yüklemeniz gerekir.

# <a name="javascript"></a>[Javascript](#tab/javascript)

*Function.json* aşağıdaki gibi tanımlanır:

[!code-json[Main](~/samples-durable-functions/samples/javascript/E4_SendSmsChallenge/function.json)]

Ve burada dört basamaklı meydan okuma kodu oluşturur ve SMS mesajı gönderir kodu:

[!code-javascript[Main](~/samples-durable-functions/samples/javascript/E4_SendSmsChallenge/index.js)]

---

## <a name="run-the-sample"></a>Örneği çalıştırma

Örnekte yer alan HTTP tetikleme işlevlerini kullanarak, aşağıdaki HTTP POST isteğini göndererek orkestrasyona başlayabilirsiniz:

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

Orchestrator işlevi verilen telefon numarasını alır ve hemen örneğin, *2168*için &mdash; rasgele oluşturulan 4 haneli doğrulama kodu ile bir SMS mesajı gönderir. İşlev daha sonra yanıt için 90 saniye bekler.

Kodla cevap vermek için, [ `RaiseEventAsync` (.NET) `raiseEvent` veya (JavaScript)](durable-functions-instance-management.md) başka bir işlevin içinde kullanabilir veya yukarıdaki 202 yanıtında `{eventName}` atıfta bulunulan **sendEventUrl** HTTP POST webhook'u çağırabilir, olayın adını değiştirebilirsiniz: `SmsChallengeResponse`

```
POST http://{host}/runtime/webhooks/durabletask/instances/741c65651d4c40cea29acdd5bb47baf1/raiseEvent/SmsChallengeResponse?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}
Content-Length: 4
Content-Type: application/json

2168
```

Bunu zamanlayıcı nın süresi dolmadan önce gönderirseniz, `output` orkestrasyon `true`tamamlanır ve alan başarılı bir doğrulamayı gösteren olarak ayarlanır.

```
GET http://{host}/runtime/webhooks/durabletask/instances/741c65651d4c40cea29acdd5bb47baf1?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}
```

```
HTTP/1.1 200 OK
Content-Length: 144
Content-Type: application/json; charset=utf-8

{"runtimeStatus":"Completed","input":"+1425XXXXXXX","output":true,"createdTime":"2017-06-29T19:10:49Z","lastUpdatedTime":"2017-06-29T19:12:23Z"}
```

Zamanlayıcının süresinin dolmasına izin verdiyseniz veya yanlış kodu dört kez girerseniz, durumu sorgulayabilir ve telefon doğrulamasının başarısız olduğunu belirten bir `false` düzenleme işlevi çıktısı görebilirsiniz.

```
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
Content-Length: 145

{"runtimeStatus":"Completed","input":"+1425XXXXXXX","output":false,"createdTime":"2017-06-29T19:20:49Z","lastUpdatedTime":"2017-06-29T19:22:23Z"}
```

## <a name="next-steps"></a>Sonraki adımlar

Bu örnek, özellikle `WaitForExternalEvent` ve `CreateTimer` API'lerin dayanıklı işlevlerinin bazı gelişmiş yeteneklerini göstermiştir. Bunlar genellikle `Task.WaitAny` gerçek insanlarla etkileşim için yararlı güvenilir bir zaman zaman önceliği sistemi uygulamak için kombine edilebilir nasıl gördüm. Belirli konuların derinlemesine kapsamını sunan bir dizi makaleyi okuyarak Dayanıklı İşlevler'in nasıl kullanılacağı hakkında daha fazla bilgi edinebilirsiniz.

> [!div class="nextstepaction"]
> [Serinin ilk makalesine gidin](durable-functions-bindings.md)
