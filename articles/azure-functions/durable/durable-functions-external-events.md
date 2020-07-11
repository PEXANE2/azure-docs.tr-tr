---
title: Dayanıklı İşlevler dış olayları işleme-Azure
description: Azure Işlevleri için Dayanıklı İşlevler uzantısı 'nda dış olayları nasıl işleyeceğinizi öğrenin.
ms.topic: conceptual
ms.date: 11/02/2019
ms.author: azfuncdf
ms.openlocfilehash: 387b5d920de4a295366cc7e948862a12cea901d3
ms.sourcegitcommit: 1e6c13dc1917f85983772812a3c62c265150d1e7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2020
ms.locfileid: "86165558"
---
# <a name="handling-external-events-in-durable-functions-azure-functions"></a>Dayanıklı İşlevler dış olayları işleme (Azure Işlevleri)

Orchestrator işlevlerinin dış olayları bekleme ve dinleme yeteneği vardır. Bu [dayanıklı işlevler](durable-functions-overview.md) özelliği, genellikle insan etkileşimini veya diğer dış Tetikleyicileri işlemek için yararlıdır.

> [!NOTE]
> Dış olaylar tek yönlü zaman uyumsuz işlemlerdir. Bu istemciler, olayı gönderen istemcinin Orchestrator işlevinden zaman uyumlu bir yanıt ihtiyacı olduğu durumlar için uygun değildir.

## <a name="wait-for-events"></a>Olayları bekle

Orchestration tetikleyicisi bağlamasının [WaitForExternalEvent](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_WaitForExternalEvent_) (.net) ve `waitForExternalEvent` (JavaScript) yöntemleri [orchestration trigger binding](durable-functions-bindings.md#orchestration-trigger) bir Orchestrator işlevinin bir dış olayı zaman uyumsuz olarak bekleyip dinlemesine izin verir. Dinleme Orchestrator işlevi, olayın *adını* ve almayı beklediği *verilerin şeklini* bildirir.

# <a name="c"></a>[C#](#tab/csharp)

```csharp
[FunctionName("BudgetApproval")]
public static async Task Run(
    [OrchestrationTrigger] IDurableOrchestrationContext context)
{
    bool approved = await context.WaitForExternalEvent<bool>("Approval");
    if (approved)
    {
        // approval granted - do the approved action
    }
    else
    {
        // approval denied - send a notification
    }
}
```

> [!NOTE]
> Önceki C# kodu Dayanıklı İşlevler 2. x içindir. Dayanıklı İşlevler 1. x için yerine kullanmanız gerekir `DurableOrchestrationContext` `IDurableOrchestrationContext` . Sürümler arasındaki farklılıklar hakkında daha fazla bilgi için [dayanıklı işlevler sürümler](durable-functions-versions.md) makalesine bakın.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context) {
    const approved = yield context.df.waitForExternalEvent("Approval");
    if (approved) {
        // approval granted - do the approved action
    } else {
        // approval denied - send a notification
    }
});
```

---

Yukarıdaki örnek, belirli bir olayı dinler ve alındığında işlem gerçekleştirir.

Aşağıdaki örnekte olduğu gibi birden çok olayı aynı anda dinleyebilir. Bu, olası üç olay bildiriminin birini bekler.

# <a name="c"></a>[C#](#tab/csharp)

```csharp
[FunctionName("Select")]
public static async Task Run(
    [OrchestrationTrigger] IDurableOrchestrationContext context)
{
    var event1 = context.WaitForExternalEvent<float>("Event1");
    var event2 = context.WaitForExternalEvent<bool>("Event2");
    var event3 = context.WaitForExternalEvent<int>("Event3");

    var winner = await Task.WhenAny(event1, event2, event3);
    if (winner == event1)
    {
        // ...
    }
    else if (winner == event2)
    {
        // ...
    }
    else if (winner == event3)
    {
        // ...
    }
}
```

> [!NOTE]
> Önceki C# kodu Dayanıklı İşlevler 2. x içindir. Dayanıklı İşlevler 1. x için yerine kullanmanız gerekir `DurableOrchestrationContext` `IDurableOrchestrationContext` . Sürümler arasındaki farklılıklar hakkında daha fazla bilgi için [dayanıklı işlevler sürümler](durable-functions-versions.md) makalesine bakın.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context) {
    const event1 = context.df.waitForExternalEvent("Event1");
    const event2 = context.df.waitForExternalEvent("Event2");
    const event3 = context.df.waitForExternalEvent("Event3");

    const winner = yield context.df.Task.any([event1, event2, event3]);
    if (winner === event1) {
        // ...
    } else if (winner === event2) {
        // ...
    } else if (winner === event3) {
        // ...
    }
});
```

---

Önceki örnek, birden çok *any* olayı dinler. *Tüm* olayları beklemek da mümkündür.

# <a name="c"></a>[C#](#tab/csharp)

```csharp
[FunctionName("NewBuildingPermit")]
public static async Task Run(
    [OrchestrationTrigger] IDurableOrchestrationContext context)
{
    string applicationId = context.GetInput<string>();

    var gate1 = context.WaitForExternalEvent("CityPlanningApproval");
    var gate2 = context.WaitForExternalEvent("FireDeptApproval");
    var gate3 = context.WaitForExternalEvent("BuildingDeptApproval");

    // all three departments must grant approval before a permit can be issued
    await Task.WhenAll(gate1, gate2, gate3);

    await context.CallActivityAsync("IssueBuildingPermit", applicationId);
}
```

> [!NOTE]
> Önceki kod Dayanıklı İşlevler 2. x içindir. Dayanıklı İşlevler 1. x için yerine kullanmanız gerekir `DurableOrchestrationContext` `IDurableOrchestrationContext` . Sürümler arasındaki farklılıklar hakkında daha fazla bilgi için [dayanıklı işlevler sürümler](durable-functions-versions.md) makalesine bakın.

.NET sürümünde, olay yükü beklenen türe dönüştürülemiyorsa `T` , bir özel durum oluşturulur.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context) {
    const applicationId = context.df.getInput();

    const gate1 = context.df.waitForExternalEvent("CityPlanningApproval");
    const gate2 = context.df.waitForExternalEvent("FireDeptApproval");
    const gate3 = context.df.waitForExternalEvent("BuildingDeptApproval");

    // all three departments must grant approval before a permit can be issued
    yield context.df.Task.all([gate1, gate2, gate3]);

    yield context.df.callActivity("IssueBuildingPermit", applicationId);
});
```

---

`WaitForExternalEvent`Bazı girişler için süresiz olarak bekler.  İşlev uygulaması beklerken güvenli bir şekilde kaldırılabilir. Bu düzenleme örneğine bir olay ulaştığında, başlatılabilmesi uyandırılır otomatik olarak oluşturulur ve olayı anında işler.

> [!NOTE]
> İşlev uygulamanız tüketim planını kullanıyorsa, bir Orchestrator işlevi bir görevi beklerken `WaitForExternalEvent` (.net) veya (JavaScript) bekleme süresi ne olursa olsun, hiçbir faturalandırma ücreti tahakkuk `waitForExternalEvent` etmez.

## <a name="send-events"></a>Olayları gönderme

[RaiseEventAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_RaiseEventAsync_) `raiseEventAsync` Bir Orchestration 'a dış olay göndermek için RaiseEventAsync (.net) veya (JavaScript) yöntemlerini kullanabilirsiniz. Bu yöntemler [Orchestration istemci](durable-functions-bindings.md#orchestration-client) bağlaması tarafından gösterilir. Ayrıca, bir Orchestration 'a dış olay göndermek için yerleşik oluşturma [OLAYı HTTP API](durable-functions-http-api.md#raise-event) 'sini de kullanabilirsiniz.

Oluşturulan olay bir *örnek kimliği*, bir *EventName*ve *eventdata* parametrelerini içerir. Orchestrator işlevleri, `WaitForExternalEvent` (.net) veya `waitForExternalEvent` (JavaScript) API 'lerini kullanarak bu olayları işler. Olayın işlenebilmesi için, söz konusu hem gönderme hem de alma uçları *ile eşleşmelidir.* Olay verileri de JSON ile seri hale getirilebilir olmalıdır.

Dahili olarak, "olay oluştur" mekanizmaları, bekleyen Orchestrator işlevi tarafından çekilen bir iletiyi sıraya alır. Örnek, belirtilen *olay adında* beklemmediyse, olay iletisi bir bellek içi kuyruğa eklenir. Düzenleme örneği daha sonra bu *olay adını* dinlemeye başlarsa, olay iletileri için sırayı denetler.

> [!NOTE]
> Belirtilen *örnek kimliğine*sahip bir düzenleme örneği yoksa, olay iletisi atılır.

Aşağıda, bir Orchestrator işlev örneğine "onay" olayı gönderen örnek bir Queue-tetiklenen işlev verilmiştir. Orchestration örnek KIMLIĞI kuyruk iletisinin gövdesinden gelir.

# <a name="c"></a>[C#](#tab/csharp)

```csharp
[FunctionName("ApprovalQueueProcessor")]
public static async Task Run(
    [QueueTrigger("approval-queue")] string instanceId,
    [DurableClient] IDurableOrchestrationClient client)
{
    await client.RaiseEventAsync(instanceId, "Approval", true);
}
```

> [!NOTE]
> Önceki C# kodu Dayanıklı İşlevler 2. x içindir. Dayanıklı İşlevler 1. x için `OrchestrationClient` özniteliği yerine özniteliği kullanmanız gerekir `DurableClient` ve `DurableOrchestrationClient` yerine parametre türünü kullanmanız gerekir `IDurableOrchestrationClient` . Sürümler arasındaki farklılıklar hakkında daha fazla bilgi için [dayanıklı işlevler sürümler](durable-functions-versions.md) makalesine bakın.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

```javascript
const df = require("durable-functions");

module.exports = async function(context, instanceId) {
    const client = df.getClient(context);
    await client.raiseEvent(instanceId, "Approval", true);
};
```

---

Dahili olarak, `RaiseEventAsync` (.net) veya `raiseEvent` (JavaScript), bekleyen Orchestrator işlevinin oluşturduğu bir iletiyi kuyruğa alır. Örnek, belirtilen *olay adında* beklemmediyse, olay iletisi bir bellek içi kuyruğa eklenir. Düzenleme örneği daha sonra bu *olay adını* dinlemeye başlarsa, olay iletileri için sırayı denetler.

> [!NOTE]
> Belirtilen *örnek kimliğine*sahip bir düzenleme örneği yoksa, olay iletisi atılır.

### <a name="http"></a>HTTP

Aşağıda, bir Orchestration örneğine "onay" olayı oluşturan HTTP isteğinin bir örneği verilmiştir. 

```http
POST /runtime/webhooks/durabletask/instances/MyInstanceId/raiseEvent/Approval&code=XXX
Content-Type: application/json

"true"
```

Bu durumda, örnek KIMLIĞI *Myınstanceıd*olarak sabit olarak kodlanır.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Hata işlemeyi nasıl uygulayacağınızı öğrenin](durable-functions-error-handling.md)

> [!div class="nextstepaction"]
> [İnsan etkileşimini bekleyen bir örnek çalıştırın](durable-functions-phone-verification.md)