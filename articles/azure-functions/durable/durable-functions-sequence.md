---
title: Dayanıklı İşlevler-Azure 'da işlev zinciri oluşturma
description: İşlev sırasını yürüten bir Dayanıklı İşlevler örneğini çalıştırmayı öğrenin.
services: functions
author: cgillum
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: azfuncdf
ms.openlocfilehash: 1168963c0698c6bdafe20babe2e5143585bf90a8
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70087116"
---
# <a name="function-chaining-in-durable-functions---hello-sequence-sample"></a>Dayanıklı İşlevler-Merhaba dizisi örneğinde işlev zinciri oluşturma

İşlev zinciri, belirli bir sırada işlev dizisini yürütme düzenine başvurur. Genellikle bir işlevin çıktısının başka bir işlevin girişine uygulanması gerekir. Bu makalede, Dayanıklı İşlevler hızlı başlangıcı 'ni ([C#](durable-functions-create-first-csharp.md) veya [JavaScript](quickstart-js-vscode.md)) tamamladığınızda oluşturduğunuz zincirleme dizisi açıklanmaktadır. Dayanıklı İşlevler hakkında daha fazla bilgi için bkz. [dayanıklı işlevler desenler ve teknik kavramlar](durable-functions-concepts.md).

[!INCLUDE [durable-functions-prerequisites](../../../includes/durable-functions-prerequisites.md)]

## <a name="the-functions"></a>İşlevler

Bu makalede örnek uygulamada aşağıdaki işlevler açıklanmaktadır:

* `E1_HelloSequence`: Bir dizide birden çok kez `E1_SayHello` çağıran bir Orchestrator işlevi. `E1_SayHello` Çağrılardan çıkışları depolar ve sonuçları kaydeder.
* `E1_SayHello`: "Hello" ile bir dizeyi eklenen bir etkinlik işlevi.

Aşağıdaki bölümlerde, komut dosyası ve JavaScript için C# kullanılan yapılandırma ve kod açıklanmaktadır. Visual Studio geliştirme kodu makalenin sonunda gösterilmektedir.

> [!NOTE]
> JavaScript Dayanıklı İşlevler yalnızca Işlevler 2. x çalışma zamanı için kullanılabilir.

## <a name="e1_hellosequence"></a>E1_HelloSequence

### <a name="functionjson-file"></a>function. JSON dosyası

Geliştirme için Visual Studio Code veya Azure portal kullanıyorsanız, Orchestrator işlevinin *function. JSON* dosyasının içeriği aşağıda verilmiştir. Çoğu Orchestrator *işlevi. JSON* dosyası neredeyse tam olarak bu şekilde görünür.

[!code-json[Main](~/samples-durable-functions/samples/csx/E1_HelloSequence/function.json)]

`orchestrationTrigger` Bağlama türü önemli bir şeydir. Tüm Orchestrator işlevlerinin bu tetikleyici türünü kullanması gerekir.

> [!WARNING]
> Orchestrator işlevlerinin "g/ç" kuralına göre ABIDE 'ye, `orchestrationTrigger` tetikleyici bağlamayı kullanırken herhangi bir giriş veya çıkış bağlaması kullanmayın.  Diğer giriş veya çıkış bağlamaları gerekliyse, bunun yerine Orchestrator tarafından çağrılan `activityTrigger` işlevler bağlamında kullanılması gerekir.

### <a name="c-script-visual-studio-code-and-azure-portal-sample-code"></a>C#betik (Visual Studio Code ve Azure portal örnek kod)

Kaynak kodu aşağıda verilmiştir:

[!code-csharp[Main](~/samples-durable-functions/samples/csx/E1_HelloSequence/run.csx)]

Tüm C# düzenleme işlevleri, `DurableOrchestrationContext` `Microsoft.Azure.WebJobs.Extensions.DurableTask` derlemede bulunan türünde bir parametreye sahip olmalıdır. Betiği kullanıyorsanız C# , derlemeye `#r` gösterimi kullanılarak başvurulabilir. Bu bağlam nesnesi diğer *etkinlik* işlevlerini çağırmanıza ve [CallActivityAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_CallActivityAsync_) yöntemini kullanarak giriş parametrelerini geçirmeye olanak tanır.

Kod, farklı `E1_SayHello` parametre değerleriyle sırayla üç kez çağrı çağırır. Her çağrının dönüş değeri, işlevin sonunda döndürülen `outputs` listeye eklenir.

### <a name="javascript"></a>Javascript

Kaynak kodu aşağıda verilmiştir:

[!code-javascript[Main](~/samples-durable-functions/samples/javascript/E1_HelloSequence/index.js)]

Tüm JavaScript düzenleme işlevleri [ `durable-functions` modül](https://www.npmjs.com/package/durable-functions)içermelidir. Bu, JavaScript 'e Dayanıklı İşlevler yazmanızı sağlayan bir kitaplıktır. Bir Orchestration işlevi ve diğer JavaScript işlevleri arasında üç önemli fark vardır:

1. İşlev bir [Oluşturucu işlevidir.](https://docs.microsoft.com/scripting/javascript/advanced/iterators-and-generators-javascript)
2. İşlev, `durable-functions` `orchestrator` modülün yöntemine (burada `df`) yapılan bir çağrıda sarmalanır.
3. İşlevin zaman uyumlu olması gerekir. ' Orchestrator ' yöntemi ' Context. Done ' çağrılmasını işletiğinden, işlev yalnızca ' Return ' olmalıdır.

Nesnesi bir `df` nesnesi içerir diğer `callActivity` etkinlik işlevlerini çağırmanıza ve metodunu kullanarak giriş parametrelerini geçirmeye olanak tanır. `context` Kod, farklı `E1_SayHello` parametre değerleriyle `yield` sırayla üç kez çağrı yaparak yürütmenin zaman uyumsuz etkinlik işlev çağrılarının döndürülecek şekilde beklemesi gerektiğini belirtmek için kullanılır. Her çağrının dönüş değeri, işlevin sonunda döndürülen `outputs` listeye eklenir.

## <a name="e1_sayhello"></a>E1_SayHello

### <a name="functionjson-file"></a>function. JSON dosyası

Etkinlik işlevine `E1_SayHello` yönelik *function. JSON* dosyası, `orchestrationTrigger` bağlama türü yerine bir `activityTrigger` bağlama türü `E1_HelloSequence` kullanması dışında, öğesinin öğesine benzerdir.

[!code-json[Main](~/samples-durable-functions/samples/csx/E1_SayHello/function.json)]

> [!NOTE]
> Bir Orchestration işlevi tarafından çağrılan tüm işlevleri `activityTrigger` bağlamayı kullanmalıdır.

Uygulamasının uygulanması `E1_SayHello` görece önemsiz bir dize biçimlendirme işlemidir.

### <a name="c"></a>C#

[!code-csharp[Main](~/samples-durable-functions/samples/csx/E1_SayHello/run.csx)]

Bu işlev [Durableactivitycontext](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableActivityContext.html)türünde bir parametreye sahiptir ve bu, Orchestrator işlevinin öğesine [`CallActivityAsync<T>`](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_CallActivityAsync_)çağrısı tarafından kendisine geçirilen girişi almak için kullanır.

### <a name="javascript"></a>JavaScript

[!code-javascript[Main](~/samples-durable-functions/samples/javascript/E1_SayHello/index.js)]

JavaScript Orchestration işlevinin aksine, etkinlik işlevinin özel bir kurulum yapması gerekmez. Orchestrator işlevi tarafından kendisine geçirilen giriş, bu örnekte, `context.bindings` `activityTrigger` `context.bindings.name`bağlamanın adı altındaki nesne üzerinde bulunur. Bağlama adı, içe aktarılmış işlevin parametresi olarak ayarlanabilir ve doğrudan erişilebilen, örnek kodun yaptığı şeydir.

## <a name="run-the-sample"></a>Örneği çalıştırma

`E1_HelloSequence` Orchestration işlemini yürütmek için aşağıdaki http post isteğini gönderin.

```
POST http://{host}/orchestrators/E1_HelloSequence
```

> [!NOTE]
> Önceki http parçacığı, `host.json` dosyada, tüm http tetikleyici işlevleri URL 'lerinden varsayılan `api/` öneki kaldıran bir giriş olduğunu varsayar. Bu yapılandırmanın `host.json` işaretlemesini, örneklerdeki dosyada bulabilirsiniz.

Örneğin, örneği "myfunctionapp" adlı bir işlev uygulamasında çalıştırıyorsanız, "{Host}" ifadesini "myfunctionapp.azurewebsites.net" ile değiştirin.

Sonuç, şöyle bir HTTP 202 yanıt olur (breçekimi için kırpılmış):

```
HTTP/1.1 202 Accepted
Content-Length: 719
Content-Type: application/json; charset=utf-8
Location: http://{host}/admin/extensions/DurableTaskExtension/instances/96924899c16d43b08a536de376ac786b?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}

(...trimmed...)
```

Bu noktada düzenleme sıraya alınır ve hemen çalışmaya başlar. `Location` Başlıktaki URL, yürütmenin durumunu denetlemek için kullanılabilir.

```
GET http://{host}/admin/extensions/DurableTaskExtension/instances/96924899c16d43b08a536de376ac786b?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}
```

Sonuç, düzenleme durumudur. Daha çabuk çalışır ve tamamlanır, bu sayede *tamamlandı* durumunda şuna benzer bir Yanıt ile görürsünüz (breçekimi için kırpılmış):

```
HTTP/1.1 200 OK
Content-Length: 179
Content-Type: application/json; charset=utf-8

{"runtimeStatus":"Completed","input":null,"output":["Hello Tokyo!","Hello Seattle!","Hello London!"],"createdTime":"2017-06-29T05:24:57Z","lastUpdatedTime":"2017-06-29T05:24:59Z"}
```

Gördüğünüz `runtimeStatus` gibi, örneği `output` *tamamlanır* ve Orchestrator işlevi yürütmenin JSON serileştirilmiş sonucunu içerir.

> [!NOTE]
> Orchestrator işlevini Başlatan HTTP POST uç noktası, örnek uygulamada "HttpStart" adlı bir HTTP tetikleyici işlevi olarak uygulanır. , Veya `queueTrigger` gibi`timerTrigger`diğer tetikleyici türleri için benzer başlangıç mantığı uygulayabilirsiniz. `eventHubTrigger`

İşlev yürütme günlüklerine bakın. İşlev, genel bakışta açıklanan yeniden yürütme davranışı nedeniyle birden çok kez başlatıldı ve tamamlandı. [](durable-functions-concepts.md) `E1_HelloSequence` Diğer taraftan, bu işlev yürütmelerinin yeniden çalınmadığından yalnızca üç `E1_SayHello` yürütmesi vardı.

## <a name="visual-studio-sample-code"></a>Visual Studio örnek kodu

Visual Studio projesindeki tek C# bir dosya olarak Orchestration aşağıda verilmiştir:

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/HelloSequence.cs)]

## <a name="next-steps"></a>Sonraki adımlar

Bu örnek, basit bir işlev zincirleme düzenlemesi göstermiştir. Sonraki örnek, fan/fan deseninin nasıl uygulanacağını gösterir.

> [!div class="nextstepaction"]
> [Fan/fan örneğini çalıştırın](durable-functions-cloud-backup.md)
