---
title: Azure Işlevleri Dayanıklı İşlevler uzantısı 'ndaki işlev türleri ve özellikleri
description: Azure Işlevlerinde bir Dayanıklı İşlevler düzenleme bölümünde işlev-işlev iletişimini destekleyen işlev ve rol türleri hakkında bilgi edinin.
services: functions
author: jeffhollan
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.topic: conceptual
ms.date: 07/04/2019
ms.author: azfuncdf
ms.openlocfilehash: 0d3087c768a02bb5c647fc0d10db3aa4274804f4
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70097752"
---
# <a name="durable-functions-types-and-features-azure-functions"></a>Dayanıklı İşlevler türleri ve özellikleri (Azure Işlevleri)

Dayanıklı İşlevler, [Azure işlevleri](../functions-overview.md)'nin bir uzantısıdır. İşlev yürütmenin durum bilgisi düzenlemesi için Dayanıklı İşlevler kullanabilirsiniz. Dayanıklı bir işlev, farklı Azure işlevlerinden oluşan bir çözümdür. İşlevler, dayanıklı bir işlev düzenlemesi içinde farklı roller oynayabilir. 

Bu makale, Dayanıklı İşlevler düzenleme içinde kullanabileceğiniz işlev türlerine genel bir bakış sunar. Makale, işlevleri bağlamak için kullanabileceğiniz bazı yaygın desenler içerir. Dayanıklı İşlevler uygulama geliştirme güçlüklerinizi çözmenize nasıl yardımcı olabileceğini öğrenin.

![Dayanıklı işlevlerin türlerini gösteren bir görüntü][1]  

## <a name="types-of-durable-functions"></a>Dayanıklı işlev türleri

Azure Işlevlerinde dört dayanıklı işlev türü kullanabilirsiniz: etkinlik, Orchestrator, varlık ve istemci.

### <a name="activity-functions"></a>Etkinlik işlevleri

Etkinlik işlevleri, dayanıklı bir işlev düzenleme içinde temel iş birimidir. Etkinlik işlevleri, işlemde düzenlenmiş işlevler ve görevlerdir. Örneğin, bir siparişi işlemek için dayanıklı bir işlev oluşturabilirsiniz. Görevler, envanteri denetlemeyi, müşteriyi borçlandırmayı ve bir sevkiyat oluşturmayı içerir. Her görev bir etkinlik işlevi olur. 

Etkinlik işlevleri, bunlarda yapabileceğiniz iş türünde Kısıtlanmamış. Desteklenen herhangi bir dilde bir etkinlik işlevi yazabilirsiniz [dayanıklı işlevler](durable-functions-overview.md#language-support). Dayanıklı görev çerçevesi, her çağrılan etkinlik işlevinin düzenleme sırasında en az bir kez yürütüleceğini garanti eder.

Etkinlik işlevini tetiklemek için bir [etkinlik tetikleyicisi](durable-functions-bindings.md#activity-triggers) kullanın. .NET işlevleri bir [Durableactivitycontext](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableActivityContext.html) parametresini parametre olarak alır. Ayrıca, tetiklemeyi bir işleve geçirilecek başka bir nesneye de bağlayabilirsiniz. JavaScript 'te, `<activity trigger binding name>` [ `context.bindings` nesne](../functions-reference-node.md#bindings)üzerindeki özelliği aracılığıyla bir girişe erişebilirsiniz.

Etkinlik işleviniz Ayrıca verileri Orchestrator 'a döndürebilir. Etkinlik işlevinden çok sayıda değer gönderirseniz veya döndürürler, [Tanımlama grupları veya diziler](durable-functions-bindings.md#passing-multiple-parameters)kullanabilirsiniz. Bir etkinlik işlevini yalnızca bir Orchestration örneğinden tetikleyebilirsiniz. Bir etkinlik işlevi ve başka bir işlev (HTTP ile tetiklenen bir işlev gibi) bazı kodları paylaşabilse de, her bir işlev yalnızca bir tetikleyicisine sahip olabilir.

Daha fazla bilgi ve örnekler için bkz. [etkinlik işlevleri](durable-functions-bindings.md#activity-triggers).

### <a name="orchestrator-functions"></a>Orchestrator işlevleri

Orchestrator işlevleri, eylemlerin nasıl yürütüleceğini ve eylemlerin yürütülme sırasını açıklamaktadır. Orchestrator işlevleri,C# [dayanıklı işlevler desenleri ve teknik kavramlar](durable-functions-concepts.md)bölümünde gösterildiği gibi kodda (veya JavaScript) Orchestration 'u anlatmaktadır. Bir düzenleme, [etkinlik işlevleri](#activity-functions), [alt](#sub-orchestrations)düzenlemeler dahil olmak üzere çok sayıda farklı eylem türüne sahip olabilir, [dış olaylar için bekliyor](#external-events)ve [zamanlayıcılar](#durable-timers). Orchestrator işlevleri de [varlık işlevleriyle](#entity-functions)etkileşime geçebilir.

Bir düzenleyici işlevi bir [düzenleme tetikleyicisi](durable-functions-bindings.md#orchestration-triggers)tarafından tetiklenmelidir.

Orchestrator [istemcisi](#client-functions)tarafından bir Orchestrator başlatılır. Orchestrator 'ı herhangi bir kaynaktan (HTTP, kuyruk, olay akışı) tetikleyebilirsiniz. Bir Orchestration 'un her örneğinin bir örnek tanımlayıcısı vardır. Örnek tanımlayıcısı otomatik olarak oluşturulabilir (önerilir) veya Kullanıcı tarafından oluşturulmuş olabilir. Düzenleme [örneklerini yönetmek](durable-functions-instance-management.md) için örnek tanımlayıcısı ' nı kullanabilirsiniz.

Daha fazla bilgi ve örnekler için bkz. [düzenleme Tetikleyicileri](durable-functions-bindings.md#orchestration-triggers).

###  <a name="entity-functions"></a>Varlık işlevleri (Önizleme)

Varlık işlevleri, *dayanıklı varlıklar*olarak bilinen küçük durum parçalarını okumak ve güncelleştirmek için işlemleri tanımlar. Orchestrator işlevleri gibi, varlık işlevleri de özel tetikleyici türü, *varlık tetikleyicisi*olan işlevlerdir. Orchestrator işlevlerinin aksine, varlık işlevlerinin belirli kod kısıtlamaları yoktur. Varlık işlevleri, durumu denetim akışı aracılığıyla örtük olarak temsil etmek yerine, durumu açıkça da yönetir.

> [!NOTE]
> Varlık işlevleri ve ilgili işlevler yalnızca Dayanıklı İşlevler 2,0 ve üzeri sürümlerde kullanılabilir.

Varlık işlevleri hakkında daha fazla bilgi için bkz. [varlık işlevleri](durable-functions-preview.md#entity-functions) önizleme özelliği belgeleri.

### <a name="client-functions"></a>İstemci işlevleri

İstemci işlevleri, düzenleme ve varlık örnekleri oluşturan ve yöneten işlevler tetiklenir. Dayanıklı İşlevler etkileşimde bulunmak için etkin bir giriş noktasıdır. İstemci işlevini herhangi bir kaynaktan (HTTP, kuyruk, olay akışı vb.) tetikleyebilirsiniz. İstemci işlevi, sürekli düzenlemeler ve varlıklar oluşturmak ve yönetmek için [Orchestration istemci bağlamasını](durable-functions-bindings.md#orchestration-client) kullanır.

Bir istemci işlevinin en temel örneği, bir Orchestrator işlevi Başlatan HTTP ile tetiklenen bir işlevdir ve sonra bir denetim durumu yanıtı döndürür. Bir örnek için bkz. [http API URL 'si bulma](durable-functions-http-api.md#http-api-url-discovery).

Daha fazla bilgi ve örnekler için bkz. [Orchestration Client](durable-functions-bindings.md#orchestration-client).

## <a name="features-and-patterns"></a>Özellikler ve desenler

Sonraki bölümlerde Dayanıklı İşlevler türlerinin özellikleri ve desenleri açıklanır.

### <a name="sub-orchestrations"></a>Alt düzenlemeler

Orchestrator işlevleri etkinlik işlevlerini çağırabilir, ancak diğer Orchestrator işlevlerini de çağırabilir. Örneğin, bir Orchestrator işlevleri kitaplığından daha büyük bir düzenleme oluşturabilirsiniz. Ya da, bir Orchestrator işlevinin birden fazla örneğini paralel olarak çalıştırabilirsiniz.

Daha fazla bilgi ve örnekler için bkz. [alt](durable-functions-sub-orchestrations.md)düzenlemeler.

### <a name="durable-timers"></a>Dayanıklı zamanlayıcılar

[Dayanıklı işlevler](durable-functions-overview.md) , gecikme süreleri uygulamak veya zaman uyumsuz eylemlerde zaman aşımları ayarlamak için Orchestrator işlevlerinde kullanabileceğiniz *dayanıklı zamanlayıcılar* sağlar. `Thread.Sleep` C#Ve ()`setTimeout()` veya ve`setInterval()` (JavaScript) yerine Orchestrator işlevlerinde dayanıklı zamanlayıcılar kullanın. `Task.Delay`

Daha fazla bilgi ve örnekler için bkz. [dayanıklı zamanlayıcılar](durable-functions-timers.md).

### <a name="external-events"></a>Dış olaylar

Orchestrator işlevleri, dış olayların bir düzenleme örneğini güncelleştirmesini bekleyebilir. Bu Dayanıklı İşlevler özelliği genellikle insan etkileşimini veya diğer dış geri çağırmaları işlemek için yararlıdır.

Daha fazla bilgi ve örnekler için bkz. [dış olaylar](durable-functions-external-events.md).

### <a name="error-handling"></a>Hata işleme

Dayanıklı İşlevler düzenlemeleri uygulamak için kodu kullanın. Programlama dilinin hata işleme özelliklerini kullanabilirsiniz. Orchestration 'inizdeki çalışma gibi `try` / `catch` desenler. 

Dayanıklı İşlevler Ayrıca, yerleşik yeniden deneme ilkeleriyle de gelir. Bir eylem, bir özel durum oluştuğunda etkinlikleri otomatik olarak erteleyebilir ve yeniden deneyebilir. Yeniden denemeleri, düzenleme yapılmadan geçici özel durumları işlemek için kullanabilirsiniz.

Daha fazla bilgi ve örnekler için bkz. [hata işleme](durable-functions-error-handling.md).

### <a name="cross-function-app-communication"></a>İşlev işlevleri arası uygulama iletişimi

Sürekli bir düzenleme tek bir işlev uygulaması bağlamında çalışsa da, birçok işlev uygulamasındaki düzenlemeleri koordine etmek için desenleri kullanabilirsiniz. Platformlar arası iletişim HTTP üzerinden gerçekleşebilir, ancak her etkinlik için dayanıklı Framework 'ün kullanılması, iki uygulama arasında dayanıklı bir işlem sürdürmenize yol açabilir.

Aşağıdaki örneklerde, ve JavaScript 'de C# işlev işlevleri arası uygulama düzenlemesi gösterilmektedir. Her örnekte, bir etkinlik dış Orchestration 'u başlatır. Başka bir etkinlik alır ve durumunu döndürür. Orchestrator, durumun devam etmeden önce olmasını `Complete` bekler.

İşlevleri arası uygulama düzenleme örnekleri aşağıda verilmiştir:

#### <a name="c"></a>C#

```csharp
[FunctionName("OrchestratorA")]
public static async Task RunRemoteOrchestrator(
    [OrchestrationTrigger] DurableOrchestrationContext context)
{
    // Do some work...

    // Call a remote orchestration.
    string statusUrl = await context.CallActivityAsync<string>(
        "StartRemoteOrchestration", "OrchestratorB");

    // Wait for the remote orchestration to complete.
    while (true)
    {
        bool isComplete = await context.CallActivityAsync<bool>("CheckIsComplete", statusUrl);
        if (isComplete)
        {
            break;
        }

        await context.CreateTimer(context.CurrentUtcDateTime.AddMinutes(1), CancellationToken.None);
    }

    // B is done. Now, go do more work...
}

[FunctionName("StartRemoteOrchestration")]
public static async Task<string> StartRemoteOrchestration([ActivityTrigger] string orchestratorName)
{
    using (var response = await HttpClient.PostAsync(
        $"https://appB.azurewebsites.net/orchestrations/{orchestratorName}",
        new StringContent("")))
    {
        string statusUrl = await response.Content.ReadAsAsync<string>();
        return statusUrl;
    }
}

[FunctionName("CheckIsComplete")]
public static async Task<bool> CheckIsComplete([ActivityTrigger] string statusUrl)
{
    using (var response = await HttpClient.GetAsync(statusUrl))
    {
        // 200 = Complete, 202 = Running
        return response.StatusCode == HttpStatusCode.OK;
    }
}
```

#### <a name="javascript-functions-2x-only"></a>JavaScript (yalnızca 2. x Işlevleri)

```javascript
const df = require("durable-functions");
const moment = require("moment");

module.exports = df.orchestrator(function*(context) {
    // Do some work...

    // Call a remote orchestration.
    const statusUrl = yield context.df.callActivity("StartRemoteOrchestration", "OrchestratorB");

    // Wait for the remote orchestration to complete.
    while (true) {
        const isComplete = yield context.df.callActivity("CheckIsComplete", statusUrl);
        if (isComplete) {
            break;
        }

        const waitTime = moment(context.df.currentUtcDateTime).add(1, "m").toDate();
        yield context.df.createTimer(waitTime);
    }

    // B is done. Now, go do more work...
});
```

```javascript
const request = require("request-promise-native");

module.exports = async function(context, orchestratorName) {
    const options = {
        method: "POST",
        uri: `https://appB.azurewebsites.net/orchestrations/${orchestratorName}`,
        body: ""
    };

    const statusUrl = await request(options);
    return statusUrl;
};
```

```javascript
const request = require("request-promise-native");

module.exports = async function(context, statusUrl) {
    const options = {
        method: "GET",
        uri: statusUrl,
        resolveWithFullResponse: true,
    };

    const response = await request(options);
    // 200 = Complete, 202 = Running
    return response.statusCode === 200;
};
```

## <a name="next-steps"></a>Sonraki adımlar

Başlamak için, veya [C#](durable-functions-create-first-csharp.md) [JavaScript](quickstart-js-vscode.md)'te ilk dayanıklı işlevinizi oluşturun.

> [!div class="nextstepaction"]
> [Dayanıklı İşlevler hakkında daha fazla bilgi edinin](durable-functions-bindings.md)

<!-- Media references -->
[1]: media/durable-functions-types-features-overview/durable-concepts.png
