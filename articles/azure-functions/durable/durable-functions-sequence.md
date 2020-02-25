---
title: Dayanıklı İşlevler-Azure 'da işlev zinciri oluşturma
description: İşlev sırasını yürüten bir Dayanıklı İşlevler örneğini çalıştırmayı öğrenin.
author: cgillum
ms.topic: conceptual
ms.date: 11/29/2019
ms.author: azfuncdf
ms.openlocfilehash: 8da4ce7801cc98f9ffb32eb7b506eaf1ccd877dd
ms.sourcegitcommit: dd3db8d8d31d0ebd3e34c34b4636af2e7540bd20
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/22/2020
ms.locfileid: "77562079"
---
# <a name="function-chaining-in-durable-functions---hello-sequence-sample"></a>Dayanıklı İşlevler-Merhaba dizisi örneğinde işlev zinciri oluşturma

İşlev zinciri, belirli bir sırada işlev dizisini yürütme düzenine başvurur. Genellikle bir işlevin çıktısının başka bir işlevin girişine uygulanması gerekir. Bu makalede, Dayanıklı İşlevler hızlı başlangıcı 'ni ([C#](durable-functions-create-first-csharp.md) veya [JavaScript](quickstart-js-vscode.md)) tamamladığınızda oluşturduğunuz zincirleme dizisi açıklanmaktadır. Dayanıklı İşlevler hakkında daha fazla bilgi için bkz. [dayanıklı işlevler genel bakış](durable-functions-overview.md).

[!INCLUDE [durable-functions-prerequisites](../../../includes/durable-functions-prerequisites.md)]

## <a name="the-functions"></a>İşlevler

Bu makalede örnek uygulamada aşağıdaki işlevler açıklanmaktadır:

* `E1_HelloSequence`: bir dizide birden çok kez `E1_SayHello` çağıran bir [Orchestrator işlevi](durable-functions-bindings.md#orchestration-trigger) . `E1_SayHello` çağrılarındaki çıktıları depolar ve sonuçları kaydeder.
* `E1_SayHello`: "Hello" ile bir dizeyi eklenen bir [etkinlik işlevi](durable-functions-bindings.md#activity-trigger) .
* `HttpStart`: Orchestrator örneğini başlatan HTTP ile tetiklenen bir işlev.

### <a name="e1_hellosequence-orchestrator-function"></a>E1_HelloSequence Orchestrator işlevi

# <a name="c"></a>[C#](#tab/csharp)

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/HelloSequence.cs?range=13-25)]

Tüm C# düzenleme işlevleri, `Microsoft.Azure.WebJobs.Extensions.DurableTask` derlemesinde bulunan `DurableOrchestrationContext`türünde bir parametreye sahip olmalıdır. Bu bağlam nesnesi diğer *etkinlik* işlevlerini çağırmanıza ve `CallActivityAsync` yöntemini kullanarak giriş parametrelerini geçirmeye olanak tanır.

Kod, farklı parametre değerleri ile sırayla üç kez `E1_SayHello` çağırır. Her çağrının dönüş değeri, işlevin sonunda döndürülen `outputs` listesine eklenir.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

> [!NOTE]
> JavaScript Dayanıklı İşlevler yalnızca Işlevler 2,0 çalışma zamanı için kullanılabilir.

#### <a name="functionjson"></a>function. JSON

Geliştirme için Visual Studio Code veya Azure portal kullanıyorsanız, Orchestrator işlevinin *function. JSON* dosyasının içeriği aşağıda verilmiştir. Çoğu Orchestrator *işlevi. JSON* dosyası neredeyse tam olarak bu şekilde görünür.

[!code-json[Main](~/samples-durable-functions/samples/javascript/E1_HelloSequence/function.json)]

Önemli şey `orchestrationTrigger` bağlama türüdür. Tüm Orchestrator işlevlerinin bu tetikleyici türünü kullanması gerekir.

> [!WARNING]
> Orchestrator işlevlerinin "g/ç" kuralına göre ABIDE 'ye `orchestrationTrigger` tetikleyici bağlamayı kullanırken herhangi bir giriş veya çıkış bağlaması kullanmayın.  Diğer giriş veya çıkış bağlamaları gerekliyse, bunun yerine Orchestrator tarafından çağrılan `activityTrigger` işlevleri bağlamında kullanılması gerekir. Daha fazla bilgi için bkz. [Orchestrator işlev kodu kısıtlamaları](durable-functions-code-constraints.md) makalesi.

#### <a name="indexjs"></a>index. js

İşlev şu şekildedir:

[!code-javascript[Main](~/samples-durable-functions/samples/javascript/E1_HelloSequence/index.js)]

Tüm JavaScript düzenleme işlevleri [`durable-functions` modülünü](https://www.npmjs.com/package/durable-functions)içermelidir. JavaScript içinde Dayanıklı İşlevler yazmanızı sağlayan bir kitaplıktır. Bir Orchestration işlevi ve diğer JavaScript işlevleri arasında üç önemli fark vardır:

1. İşlev bir [Oluşturucu işlevidir.](https://docs.microsoft.com/scripting/javascript/advanced/iterators-and-generators-javascript).
2. İşlev, `durable-functions` modülünün `orchestrator` yöntemine (`df`) bir çağrıda paketlenir.
3. İşlevin zaman uyumlu olması gerekir. ' Orchestrator ' yöntemi ' Context. Done ' çağrılmasını işletiğinden, işlev yalnızca ' Return ' olmalıdır.

`context` nesnesi, diğer *etkinlik* işlevlerini çağırmanıza ve `callActivity` metodunu kullanarak giriş parametrelerini geçirmeye imkan tanıyan `df` dayanıklı bir düzenleme bağlamı nesnesi içerir. Kod, farklı parametre değerleriyle sırayla `E1_SayHello` çağırır, yürütmenin zaman uyumsuz etkinlik işlev çağrılarının döndürülecek şekilde beklemesi gerektiğini belirtmek için `yield` kullanarak. Her çağrının dönüş değeri, işlevin sonunda döndürülen `outputs` dizisine eklenir.

---

### <a name="e1_sayhello-activity-function"></a>E1_SayHello Activity işlevi

# <a name="c"></a>[C#](#tab/csharp)

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/HelloSequence.cs?range=27-32)]

Etkinlikler `ActivityTrigger` özniteliğini kullanır. `GetInput<T>`kullanarak giriş değerine erişme gibi etkinlik ile ilgili eylemler gerçekleştirmek için, belirtilen `IDurableActivityContext` kullanın.

`E1_SayHello` uygulanması görece basit bir dize biçimlendirme işlemidir.

Bir `IDurableActivityContext`bağlama yerine, doğrudan etkinlik işlevine geçirilen türe bağlayabilirsiniz. Örnek:

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/HelloSequence.cs?range=34-38)]

# <a name="javascript"></a>[JavaScript](#tab/javascript)

#### <a name="e1_sayhellofunctionjson"></a>E1_SayHello/function.exe JSON

`E1_SayHello` Activity işlevi için *function. JSON* dosyası, `orchestrationTrigger` bağlama türü yerine `activityTrigger` bağlama türü kullanması dışında `E1_HelloSequence` benzerdir.

[!code-json[Main](~/samples-durable-functions/samples/javascript/E1_SayHello/function.json)]

> [!NOTE]
> Bir Orchestration işlevi tarafından çağrılan tüm işlevleri `activityTrigger` bağlamasını kullanmalıdır.

`E1_SayHello` uygulanması görece basit bir dize biçimlendirme işlemidir.

#### <a name="e1_sayhelloindexjs"></a>E1_SayHello/Index.js

[!code-javascript[Main](~/samples-durable-functions/samples/javascript/E1_SayHello/index.js)]

JavaScript Orchestration işlevinin aksine, etkinlik işlevinin özel bir kurulum yapması gerekmez. Orchestrator işlevi tarafından kendisine geçirilen giriş, bu örnekte `context.bindings.name``activityTrigger` bağlamanın adı altında `context.bindings` nesnesi üzerinde bulunur. Bağlama adı, içe aktarılmış işlevin parametresi olarak ayarlanabilir ve doğrudan erişilebilen, örnek kodun yaptığı şeydir.

---

### <a name="httpstart-client-function"></a>HttpStart istemci işlevi

Bir istemci işlevi kullanarak Orchestrator işlevinin bir örneğini başlatabilirsiniz. `E1_HelloSequence`örneklerini başlatmak için `HttpStart` HTTP tarafından tetiklenen işlevi kullanacaksınız.

# <a name="c"></a>[C#](#tab/csharp)

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/HttpStart.cs?range=13-30)]

Düzenleyiciler ile etkileşim kurmak için işlev bir `DurableClient` girişi bağlaması içermelidir. Bir düzenleme başlatmak için istemcisini kullanırsınız. Ayrıca, yeni Orchestration 'un durumunu denetlemek için URL 'Leri içeren bir HTTP yanıtı döndürbaşlamanıza da yardımcı olabilir.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

#### <a name="httpstartfunctionjson"></a>HttpStart/function. JSON

[!code-json[Main](~/samples-durable-functions/samples/javascript/HttpStart/function.json?highlight=16-20)]

Düzenleyiciler ile etkileşim kurmak için işlev bir `durableClient` girişi bağlaması içermelidir.

#### <a name="httpstartindexjs"></a>HttpStart/index. js

[!code-javascript[Main](~/samples-durable-functions/samples/javascript/HttpStart/index.js)]

`DurableOrchestrationClient` bir nesne almak için `df.getClient` kullanın. Bir düzenleme başlatmak için istemcisini kullanırsınız. Ayrıca, yeni Orchestration 'un durumunu denetlemek için URL 'Leri içeren bir HTTP yanıtı döndürbaşlamanıza da yardımcı olabilir.

---

## <a name="run-the-sample"></a>Örneği çalıştırma

`E1_HelloSequence` Orchestration 'u yürütmek için, aşağıdaki HTTP POST isteğini `HttpStart` işlevine gönderin.

```
POST http://{host}/orchestrators/E1_HelloSequence
```

> [!NOTE]
> Önceki HTTP kod parçacığında, tüm HTTP tetikleyici işlevleri URL 'Lerinden varsayılan `api/` önekini kaldıran `host.json` dosyasında bir giriş olduğunu varsaymaktadır. Bu yapılandırmanın işaretlemesini, örneklerdeki `host.json` dosyasında bulabilirsiniz.

Örneğin, örneği "myfunctionapp" adlı bir işlev uygulamasında çalıştırıyorsanız, "{Host}" ifadesini "myfunctionapp.azurewebsites.net" ile değiştirin.

Sonuç, şöyle bir HTTP 202 yanıt olur (breçekimi için kırpılmış):

```
HTTP/1.1 202 Accepted
Content-Length: 719
Content-Type: application/json; charset=utf-8
Location: http://{host}/runtime/webhooks/durabletask/instances/96924899c16d43b08a536de376ac786b?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}

(...trimmed...)
```

Bu noktada düzenleme sıraya alınır ve hemen çalışmaya başlar. `Location` üstbilgisindeki URL, yürütmenin durumunu denetlemek için kullanılabilir.

```
GET http://{host}/runtime/webhooks/durabletask/instances/96924899c16d43b08a536de376ac786b?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}
```

Sonuç, düzenleme durumudur. Daha çabuk çalışır ve tamamlanır, bu sayede *tamamlandı* durumunda şuna benzer bir Yanıt ile görürsünüz (breçekimi için kırpılmış):

```
HTTP/1.1 200 OK
Content-Length: 179
Content-Type: application/json; charset=utf-8

{"runtimeStatus":"Completed","input":null,"output":["Hello Tokyo!","Hello Seattle!","Hello London!"],"createdTime":"2017-06-29T05:24:57Z","lastUpdatedTime":"2017-06-29T05:24:59Z"}
```

Gördüğünüz gibi, örnek `runtimeStatus` *tamamlanır* ve `output` Orchestrator IşLEVI yürütmenin JSON tarafından serileştirilmiş sonucunu içerir.

> [!NOTE]
> `queueTrigger`, `eventHubTrigger`veya `timerTrigger`gibi diğer tetikleyici türleri için de benzer başlangıç mantığı uygulayabilirsiniz.

İşlev yürütme günlüklerine bakın. `E1_HelloSequence` işlevi, [düzenleme güvenilirliği](durable-functions-orchestrations.md#reliability) konusunda açıklanan yeniden yürütme davranışı nedeniyle birden çok kez başlatıldı ve tamamlandı. Diğer taraftan, bu işlev yürütmelerinin yeniden yürütülmediğinden yalnızca üç yürütme `E1_SayHello` vardı.

## <a name="next-steps"></a>Sonraki adımlar

Bu örnek, basit bir işlev zincirleme düzenlemesi göstermiştir. Sonraki örnek, fan/fan deseninin nasıl uygulanacağını gösterir.

> [!div class="nextstepaction"]
> [Fan/fan örneğini çalıştırın](durable-functions-cloud-backup.md)
