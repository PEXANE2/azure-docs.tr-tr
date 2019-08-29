---
title: Dayanıklı İşlevler-Azure için tekton
description: Azure Işlevleri için Dayanıklı İşlevler uzantısında tekton kullanımı.
services: functions
author: cgillum
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: azfuncdf
ms.openlocfilehash: d9bf9687f60e649fee98869ef263117177ad5efd
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70097923"
---
# <a name="singleton-orchestrators-in-durable-functions-azure-functions"></a>Dayanıklı İşlevler içindeki tekil düzenleyiciler (Azure Işlevleri)

Arka plan işleri için genellikle belirli bir Orchestrator örneğinin aynı anda çalıştığından emin olmanız gerekir. Bu, bir Orchestrator 'a belirli bir örnek KIMLIĞI atanırken bir Orchestrator 'a atanarak [dayanıklı işlevler](durable-functions-overview.md) yapılabilir.

## <a name="singleton-example"></a>Tek örnek

Aşağıdaki C# ve JavaScript örnekleri, tek bir arka plan işi düzenlemesi oluşturan bir http tetikleyici işlevi gösterir. Kod, belirtilen örnek KIMLIĞI için yalnızca bir örneğin mevcut olmasını sağlar.

### <a name="c"></a>C#

```cs
[FunctionName("HttpStartSingle")]
public static async Task<HttpResponseMessage> RunSingle(
    [HttpTrigger(AuthorizationLevel.Function, methods: "post", Route = "orchestrators/{functionName}/{instanceId}")] HttpRequestMessage req,
    [OrchestrationClient] DurableOrchestrationClient starter,
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

### <a name="javascript-functions-2x-only"></a>JavaScript (yalnızca 2. x Işlevleri)

Function.json dosyası aşağıda verilmiştir:
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

JavaScript kod aşağıdaki gibidir:
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

Varsayılan olarak, örnek kimlikleri rastgele oluşturulan GUID 'lerdir. Ancak bu durumda, örnek KIMLIĞI URL 'den rota verilerinde geçirilir. Kod, belirtilen kimliğe sahip bir örneğinC#zaten çalışır `getStatus` durumda olup olmadığını denetlemek için [getstatusasync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_GetStatusAsync_) () veya (JavaScript) öğesini çağırır. Aksi takdirde, bu KIMLIKLE bir örnek oluşturulur.

> [!WARNING]
> JavaScript 'te yerel olarak geliştirme yaparken, ortam değişkenini `WEBSITE_HOSTNAME` `localhost:<port>`, EX olarak ayarlamanız gerekir. `localhost:7071`üzerinde `DurableOrchestrationClient`yöntemleri kullanmak için. Bu gereksinim hakkında daha fazla bilgi için bkz. [GitHub sorunu](https://github.com/Azure/azure-functions-durable-js/issues/28).

> [!NOTE]
> Bu örnekte olası bir yarış durumu var. İki tane **Httpstartone** örneği aynı anda yürütülecektir, her iki işlev çağrısı de başarıyı bildirir ancak yalnızca bir düzenleme örneği başlatılır. Gereksinimlerinize bağlı olarak, bu istenmeyen yan etkilere sahip olabilir. Bu nedenle, bu tetikleyici işlevini aynı anda yürütebilmesi için iki isteğin olmadığından emin olmanız önemlidir.

Orchestrator işlevinin uygulama ayrıntıları aslında önemi yoktur. Bu, başlayan ve tamamlanmış normal bir Orchestrator işlevi olabilir ya da sonsuza kadar (yani, bir [Eternal düzenleme](durable-functions-eternal-orchestrations.md)) çalışan bir düzenleyici olabilir. Önemli nokta, tek seferde çalışan yalnızca bir örnek vardır.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Alt düzenlemeleri çağırmayı öğrenin](durable-functions-sub-orchestrations.md)
