---
title: Dayanıklı İşlevlerde Fonksiyon zincirleme - Azure
description: Bir dizi işlev çalıştıran Dayanıklı İşlevler örneğini nasıl çalıştırıştıyor.
author: cgillum
ms.topic: conceptual
ms.date: 11/29/2019
ms.author: azfuncdf
ms.openlocfilehash: 8da4ce7801cc98f9ffb32eb7b506eaf1ccd877dd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77562079"
---
# <a name="function-chaining-in-durable-functions---hello-sequence-sample"></a>Dayanıklı Fonksiyonlarda Fonksiyon Zincirleme - Merhaba sıra örneği

İşlev zincirleme belirli bir sırada işlevlerin bir dizi yürütme deseni anlamına gelir. Genellikle bir işlevin çıktısının başka bir işlevin girişine uygulanması gerekir. Bu makalede, Dayanıklı İşlevler quickstart[(C#](durable-functions-create-first-csharp.md) veya [JavaScript)](quickstart-js-vscode.md)tamamlandığında oluşturduğunuz zincirleme sırası açıklanır. Dayanıklı Fonksiyonlar hakkında daha fazla bilgi için Bkz. [Dayanıklı Fonksiyonlar genel bakış.](durable-functions-overview.md)

[!INCLUDE [durable-functions-prerequisites](../../../includes/durable-functions-prerequisites.md)]

## <a name="the-functions"></a>Fonksiyonlar

Bu makalede, örnek uygulamada aşağıdaki işlevler açıklanmaktadır:

* `E1_HelloSequence`: Bir sırayla birden `E1_SayHello` çok kez çağıran bir [orkestratör işlevi.](durable-functions-bindings.md#orchestration-trigger) `E1_SayHello` Aramaların çıktılarını depolar ve sonuçları kaydeder.
* `E1_SayHello`: Bir dizeyi "Merhaba" ile hazırlayan bir [etkinlik işlevi.](durable-functions-bindings.md#activity-trigger)
* `HttpStart`: Orkestratörün bir örneğini başlatan bir HTTP tetikleme işlevi.

### <a name="e1_hellosequence-orchestrator-function"></a>E1_HelloSequence orkestratör fonksiyonu

# <a name="c"></a>[C #](#tab/csharp)

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/HelloSequence.cs?range=13-25)]

Tüm C# orkestrasyon işlevleri, `DurableOrchestrationContext` `Microsoft.Azure.WebJobs.Extensions.DurableTask` derlemede bulunan bir tür parametresi olmalıdır. Bu bağlam nesnesi, diğer *etkinlik* işlevlerini `CallActivityAsync` aramanızı ve kendi yöntemini kullanarak giriş parametrelerini geçirmenizi sağlar.

Kod, `E1_SayHello` farklı parametre değerleri yle sırayla üç kez çağırır. Her çağrının geri dönüş değeri, işlevin sonunda döndürülen `outputs` listeye eklenir.

# <a name="javascript"></a>[Javascript](#tab/javascript)

> [!NOTE]
> JavaScript Dayanıklı Fonksiyonlar yalnızca 2.0 çalışma süresi işlevleri için kullanılabilir.

#### <a name="functionjson"></a>function.json

Geliştirme için Visual Studio Code veya Azure portalını kullanıyorsanız, orkestratör işlevi için *function.json* dosyasının içeriği aşağıda veda edebilirsiniz. Çoğu orkestratör *fonksiyon.json* dosyaları neredeyse tam olarak bu gibi görünüyor.

[!code-json[Main](~/samples-durable-functions/samples/javascript/E1_HelloSequence/function.json)]

Önemli olan `orchestrationTrigger` bağlama türüdür. Tüm orchestrator işlevleri bu tetikleyici türünü kullanmalıdır.

> [!WARNING]
> Orchestrator işlevlerinin "G/Ç yok" kuralına uymak için, tetikleyici bağlamayı kullanırken giriş `orchestrationTrigger` veya çıkış bağlamaları kullanmayın.  Diğer giriş veya çıktı bağlamaları gerekiyorsa, bunun yerine orkestratör `activityTrigger` tarafından çağrılan işlevler bağlamında kullanılmalıdır. Daha fazla bilgi [için, orchestrator işlev kodu kısıtlamaları](durable-functions-code-constraints.md) makalesine bakın.

#### <a name="indexjs"></a>index.js

İşte işlevi:

[!code-javascript[Main](~/samples-durable-functions/samples/javascript/E1_HelloSequence/index.js)]

Tüm JavaScript düzenleme işlevleri [ `durable-functions` modülü](https://www.npmjs.com/package/durable-functions)içermelidir. JavaScript'te Dayanıklı Fonksiyonlar yazmanızı sağlayan bir kitaplık. Bir orkestrasyon işlevi ve diğer JavaScript işlevleri arasında üç önemli fark vardır:

1. Fonksiyon bir [jeneratör fonksiyonudur. .](https://docs.microsoft.com/scripting/javascript/advanced/iterators-and-generators-javascript)
2. İşlev, modülün `durable-functions` `orchestrator` yöntemine yapılan bir çağrıya `df`sarılır (burada).
3. Fonksiyon eşzamanlı olmalıdır. 'Orkestratör' yöntemi 'bağlam.bitti' demeyi işlediği için, işlev sadece 'return' olmalıdır.

Nesne, `context` diğer `df` *etkinlik* işlevlerini aramanızı ve kendi yöntemini kullanarak giriş `callActivity` parametrelerini geçirmenizi sağlayan dayanıklı bir düzenleme bağlamı nesnesi içerir. Kod, `E1_SayHello` yürütmeyi belirtmek için kullanarak `yield` üç kez sırayla çağırır, döndürülecek async etkinlik işlevi çağrıları üzerinde beklemeniz gerekir. Her çağrının geri dönüş değeri, işlevin sonunda döndürülen `outputs` diziye eklenir.

---

### <a name="e1_sayhello-activity-function"></a>E1_SayHello etkinlik fonksiyonu

# <a name="c"></a>[C #](#tab/csharp)

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/HelloSequence.cs?range=27-32)]

Etkinlikler özniteliği kullanır. `ActivityTrigger` Sağlananı, `IDurableActivityContext` giriş değerine erişmek gibi etkinlikle ilgili `GetInput<T>`eylemleri gerçekleştirmek için kullanın.

Uygulanması nispeten önemsiz bir dize biçimlendirme `E1_SayHello` işlemidir.

Bir `IDurableActivityContext`,'ye bağlanmak yerine, doğrudan etkinlik işlevine geçirilen türe bağlayabilirsiniz. Örnek:

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/HelloSequence.cs?range=34-38)]

# <a name="javascript"></a>[Javascript](#tab/javascript)

#### <a name="e1_sayhellofunctionjson"></a>E1_SayHello/fonksiyon.json

Etkinlik işlevi `E1_SayHello` için *function.json* dosyası, `orchestrationTrigger` bağlama `E1_HelloSequence` türü yerine `activityTrigger` bağlayıcı bir tür kullanması dışında benzer.

[!code-json[Main](~/samples-durable-functions/samples/javascript/E1_SayHello/function.json)]

> [!NOTE]
> Bir orkestrasyon işlevi tarafından `activityTrigger` çağrılan herhangi bir işlev bağlama kullanmanız gerekir.

Uygulanması nispeten önemsiz bir dize biçimlendirme `E1_SayHello` işlemidir.

#### <a name="e1_sayhelloindexjs"></a>E1_SayHello/index.js

[!code-javascript[Main](~/samples-durable-functions/samples/javascript/E1_SayHello/index.js)]

JavaScript düzenleme işlevinin aksine, bir etkinlik işlevinin özel bir kuruluma ihtiyacı yoktur. Orkestratör işlevi tarafından ona aktarılan giriş `context.bindings` `activityTrigger` bağlayıcı adı altında nesne üzerinde yer alır - `context.bindings.name`bu durumda, . Bağlama adı dışa aktarılan işlevin bir parametresi olarak ayarlanabilir ve örnek kodun yaptığı gibi doğrudan erişilebilir.

---

### <a name="httpstart-client-function"></a>HttpBaşlat istemci işlevi

Bir istemci işlevini kullanarak orchestrator işlevinin bir örneğini başlatabilirsiniz. `HttpStart` Http tetiklenen işlevi kullanarak ' ın `E1_HelloSequence`örneklerini başlatabilirsiniz.

# <a name="c"></a>[C #](#tab/csharp)

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/HttpStart.cs?range=13-30)]

Orkestratörlerle etkileşimkurmak için işlevin `DurableClient` bir giriş bağlaması içermesi gerekir. Bir orkestrasyon başlatmak için istemciyi kullanırsınız. Ayrıca, yeni orkestrasyonun durumunu denetlemek için URL'ler içeren bir HTTP yanıtı döndürmenize de yardımcı olabilir.

# <a name="javascript"></a>[Javascript](#tab/javascript)

#### <a name="httpstartfunctionjson"></a>HttpBaşlat/fonksiyon.json

[!code-json[Main](~/samples-durable-functions/samples/javascript/HttpStart/function.json?highlight=16-20)]

Orkestratörlerle etkileşimkurmak için işlevin `durableClient` bir giriş bağlaması içermesi gerekir.

#### <a name="httpstartindexjs"></a>Http://Başlat/index.js

[!code-javascript[Main](~/samples-durable-functions/samples/javascript/HttpStart/index.js)]

Bir `df.getClient` `DurableOrchestrationClient` nesne elde etmek için kullanın. Bir orkestrasyon başlatmak için istemciyi kullanırsınız. Ayrıca, yeni orkestrasyonun durumunu denetlemek için URL'ler içeren bir HTTP yanıtı döndürmenize de yardımcı olabilir.

---

## <a name="run-the-sample"></a>Örneği çalıştırma

Orkestrasyonu `E1_HelloSequence` yürütmek için, işleve `HttpStart` aşağıdaki HTTP POST isteğini gönderin.

```
POST http://{host}/orchestrators/E1_HelloSequence
```

> [!NOTE]
> Önceki HTTP snippet tüm HTTP tetik `host.json` işlevleri URL'lerinden `api/` varsayılan önek kaldırır dosyada bir giriş olduğunu varsayar. Bu yapılandırmanın biçimlendirmesini örneklerde `host.json` dosyada bulabilirsiniz.

Örneğin, örneği "myfunctionapp" adlı bir işlev uygulamasında çalıştırıyorsanız, "{host}" yerine "myfunctionapp.azurewebsites.net" olarak değiştirin.

Sonuç bir HTTP 202 yanıtı, bu gibi (kısalık için kesilmiş):

```
HTTP/1.1 202 Accepted
Content-Length: 719
Content-Type: application/json; charset=utf-8
Location: http://{host}/runtime/webhooks/durabletask/instances/96924899c16d43b08a536de376ac786b?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}

(...trimmed...)
```

Bu noktada, orkestrasyon sıraya alınır ve hemen çalışmaya başlar. `Location` Üstbilgideki URL yürütmedurumunu denetlemek için kullanılabilir.

```
GET http://{host}/runtime/webhooks/durabletask/instances/96924899c16d43b08a536de376ac786b?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}
```

Sonuç, orkestrasyonun durumudur. Bu çalışır ve hızlı bir şekilde tamamlar, bu yüzden bu gibi görünen bir yanıt ile *Tamamlanmış* durumda görmek (kısalık için kesilmiş):

```
HTTP/1.1 200 OK
Content-Length: 179
Content-Type: application/json; charset=utf-8

{"runtimeStatus":"Completed","input":null,"output":["Hello Tokyo!","Hello Seattle!","Hello London!"],"createdTime":"2017-06-29T05:24:57Z","lastUpdatedTime":"2017-06-29T05:24:59Z"}
```

Gördüğünüz gibi, örneğin `runtimeStatus` tamamlandı *ve* orkestratör `output` işlevi yürütme JSON serileştirilmiş sonucu içerir.

> [!NOTE]
> Diğer tetikleyici türleri için benzer başlangıç `queueTrigger`mantığı `eventHubTrigger`uygulayabilirsiniz, gibi , , . `timerTrigger`

İşlev yürütme günlüklerine bak. İşlev, `E1_HelloSequence` [orkestrasyon güvenilirliği](durable-functions-orchestrations.md#reliability) konusunda açıklanan yeniden oynatma davranışı nedeniyle birden çok kez başlatıldı ve tamamlandı. Öte yandan, bu işlev yürütmeleri `E1_SayHello` tekrar alamadım beri sadece üç yürütmeleri vardı.

## <a name="next-steps"></a>Sonraki adımlar

Bu örnek basit bir fonksiyon zincirleme orkestrasyon göstermiştir. Bir sonraki örnek, fan-out/fan-in deseninin nasıl uygulanacağını gösterir.

> [!div class="nextstepaction"]
> [Fan-out/fan-in örneğini çalıştırma](durable-functions-cloud-backup.md)
