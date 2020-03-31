---
title: Dayanıklı Fonksiyonlar için Singletons - Azure
description: Azure İşlevler için Dayanıklı İşlevler uzantısında singleton nasıl kullanılır?
author: cgillum
ms.topic: conceptual
ms.date: 11/03/2019
ms.author: azfuncdf
ms.openlocfilehash: 4eff7c4c91ed664fcf1f4fc7a8be2d43d24e5c6b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76262818"
---
# <a name="singleton-orchestrators-in-durable-functions-azure-functions"></a>Dayanıklı İşlevler (Azure İşlevlerinde) Singleton orkestratörleri

Arka plan işleri için, genellikle belirli bir orkestratörün yalnızca bir örneğinin aynı anda çalıştığından emin olmanız gerekir. Bu tür singleton davranışı [Kalıcı İşlevler](durable-functions-overview.md) oluştururken bir orkestratöre belirli bir örnek kimliği atayarak sağlayabilirsiniz.

## <a name="singleton-example"></a>Singleton örneği

Aşağıdaki örnekte, tek tonlu arka plan iş orkestrasyonu oluşturan bir HTTP tetikleyici işlevi gösterilmektedir. Kod, belirtilen bir örnek kimliği için yalnızca bir örneğin bulunmasını sağlar.

# <a name="c"></a>[C #](#tab/csharp)

```cs
[FunctionName("HttpStartSingle")]
public static async Task<HttpResponseMessage> RunSingle(
    [HttpTrigger(AuthorizationLevel.Function, methods: "post", Route = "orchestrators/{functionName}/{instanceId}")] HttpRequestMessage req,
    [DurableClient] IDurableOrchestrationClient starter,
    string functionName,
    string instanceId,
    ILogger log)
{
    // Check if an instance with the specified ID already exists.
    var existingInstance = await starter.GetStatusAsync(instanceId);
    if (existingInstance == null)
    {
        // An instance with the specified ID doesn't exist, create one.
        dynamic eventData = await req.Content.ReadAsAsync<object>();
        await starter.StartNewAsync(functionName, instanceId, eventData);
        log.LogInformation($"Started orchestration with ID = '{instanceId}'.");
        return starter.CreateCheckStatusResponse(req, instanceId);
    }
    else
    {
        // An instance with the specified ID exists, don't create one.
        return req.CreateErrorResponse(
            HttpStatusCode.Conflict,
            $"An instance with ID '{instanceId}' already exists.");
    }
}
```

> [!NOTE]
> Önceki C# kodu Dayanıklı Fonksiyonlar 2.x içindir. Dayanıklı Fonksiyonlar 1.x için `OrchestrationClient` öznitelik yerine `DurableClient` öznitelik kullanmanız gerekir `DurableOrchestrationClient` ve `IDurableOrchestrationClient`parametre türünü yerine kullanmalısınız. Sürümler arasındaki farklar hakkında daha fazla bilgi için [Dayanıklı Işlevler sürümleri](durable-functions-versions.md) makalesine bakın.

# <a name="javascript"></a>[Javascript](#tab/javascript)

**fonksiyon.json**

```json
{
  "bindings": [
    {
      "authLevel": "function",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in",
      "route": "orchestrators/{functionName}/{instanceId}",
      "methods": ["post"]
    },
    {
      "name": "starter",
      "type": "orchestrationClient",
      "direction": "in"
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    }
  ]
}
```

**index.js**

```javascript
const df = require("durable-functions");

module.exports = async function(context, req) {
    const client = df.getClient(context);

    const instanceId = req.params.instanceId;
    const functionName = req.params.functionName;

    // Check if an instance with the specified ID already exists.
    const existingInstance = await client.getStatus(instanceId);
    if (!existingInstance) {
        // An instance with the specified ID doesn't exist, create one.
        const eventData = req.body;
        await client.startNew(functionName, instanceId, eventData);
        context.log(`Started orchestration with ID = '${instanceId}'.`);
        return client.createCheckStatusResponse(req, instanceId);
    } else {
        // An instance with the specified ID exists, don't create one.
        return {
            status: 409,
            body: `An instance with ID '${instanceId}' already exists.`,
        };
    }
};
```

---

Varsayılan olarak, örnek AD'lar rasgele oluşturulan GUID'ler. Önceki örnekte, ancak, örnek kimliği URL'den rota verilerine aktarılır. Belirtilen kimliğe sahip bir örneğin zaten çalışıyor olup olmadığını denetlemek için kod çağrıları `GetStatusAsync`(C#) veya `getStatus` (JavaScript). Böyle bir örnek çalıştırılamazsa, bu kimlikle birlikte yeni bir örnek oluşturulur.

> [!NOTE]
> Bu örnekte potansiyel bir Yarış koşulu var. **İki Örnek HttpStartSingle** aynı anda yürütülürse, her iki işlev çağrısı da başarıyı bildirir, ancak yalnızca bir düzenleme örneği gerçekten başlar. İhtiyaçlarınıza bağlı olarak, bu istenmeyen yan etkileri olabilir. Bu nedenle, iki istek aynı anda bu tetikleyici işlevi yürütebilir sağlamak önemlidir.

Orkestratör işlevinin uygulama ayrıntıları aslında önemli değildir. Bu başlar ve tamamlar düzenli bir orkestratör fonksiyonu olabilir, ya da sonsuza kadar çalışan biri olabilir (yani, bir [Ebedi Orkestrasyon).](durable-functions-eternal-orchestrations.md) Önemli olan nokta, aynı anda çalışan tek bir örnek olmasıdır.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Orkestrasyonların yerel HTTP özellikleri hakkında bilgi edinin](durable-functions-http-features.md)
