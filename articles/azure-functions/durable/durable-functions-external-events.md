---
title: Dayanıklı İşlevler dış olayları işleme-Azure
description: Azure Işlevleri için Dayanıklı İşlevler uzantısı 'nda dış olayları nasıl işleyeceğinizi öğrenin.
services: functions
author: ggailey777
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: azfuncdf
ms.openlocfilehash: d9c546064589e82cfef367978ebea98c2c202307
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70087297"
---
# <a name="handling-external-events-in-durable-functions-azure-functions"></a>Dayanıklı İşlevler dış olayları işleme (Azure Işlevleri)

Orchestrator işlevlerinin dış olayları bekleme ve dinleme yeteneği vardır. Bu [dayanıklı işlevler](durable-functions-overview.md) özelliği, genellikle insan etkileşimini veya diğer dış Tetikleyicileri işlemek için yararlıdır.

> [!NOTE]
> Dış olaylar tek yönlü zaman uyumsuz işlemlerdir. Bu istemciler, olayı gönderen istemcinin Orchestrator işlevinden zaman uyumlu bir yanıt ihtiyacı olduğu durumlar için uygun değildir.

## <a name="wait-for-events"></a>Olayları bekle

[WaitForExternalEvent](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_WaitForExternalEvent_) yöntemi bir Orchestrator işlevinin bir dış olayı zaman uyumsuz olarak bekleyip dinlemesine olanak tanır. Dinleme Orchestrator işlevi, olayın *adını* ve almayı beklediği *verilerin şeklini* bildirir.

### <a name="c"></a>C#

```csharp
[FunctionName("BudgetApproval")]
public static async Task Run(
    [OrchestrationTrigger] DurableOrchestrationContext context)
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

### <a name="javascript-functions-2x-only"></a>JavaScript (yalnızca 2. x Işlevleri)

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

Yukarıdaki örnek, belirli bir olayı dinler ve alındığında işlem gerçekleştirir.

Aşağıdaki örnekte olduğu gibi birden çok olayı aynı anda dinleyebilir. Bu, olası üç olay bildiriminin birini bekler.

#### <a name="c"></a>C#

```csharp
[FunctionName("Select")]
public static async Task Run(
    [OrchestrationTrigger] DurableOrchestrationContext context)
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

#### <a name="javascript-functions-2x-only"></a>JavaScript (yalnızca 2. x Işlevleri)

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

Önceki örnek, birden çok olayı dinler. *Tüm* olayları beklemek da mümkündür.

#### <a name="c"></a>C#

```csharp
[FunctionName("NewBuildingPermit")]
public static async Task Run(
    [OrchestrationTrigger] DurableOrchestrationContext context)
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

#### <a name="javascript-functions-2x-only"></a>JavaScript (yalnızca 2. x Işlevleri)

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

[WaitForExternalEvent](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_WaitForExternalEvent_) , bazı girişler için süresiz olarak bekler.  İşlev uygulaması beklerken güvenli bir şekilde kaldırılabilir. Bu düzenleme örneğine bir olay ulaştığında, başlatılabilmesi uyandırılır otomatik olarak oluşturulur ve olayı anında işler.

> [!NOTE]
> İşlev uygulamanız tüketim planını kullanıyorsa, bir Orchestrator işlevi bir görevi `WaitForExternalEvent` beklerken (.net) veya `waitForExternalEvent` (JavaScript) bekleme süresi ne olursa olsun, hiçbir faturalandırma ücreti tahakkuk etmez.

.NET sürümünde, olay yükü beklenen türe `T`dönüştürülemiyorsa, bir özel durum oluşturulur.

## <a name="send-events"></a>Olayları gönderme

[Durableorchestrationclient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) sınıfının `WaitForExternalEvent` [RaiseEventAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_RaiseEventAsync_) yöntemi, (.net) veya `waitForExternalEvent` (JavaScript) için beklediği olayları gönderir.  Yöntemi `RaiseEventAsync` , *EventName* ve *eventdata* parametrelerini parametre olarak alır. Olay verileri JSON ile seri hale getirilebilir olmalıdır.

Aşağıda, bir Orchestrator işlev örneğine "onay" olayı gönderen örnek bir Queue-tetiklenen işlev verilmiştir. Orchestration örnek KIMLIĞI kuyruk iletisinin gövdesinden gelir.

### <a name="c"></a>C#

```csharp
[FunctionName("ApprovalQueueProcessor")]
public static async Task Run(
    [QueueTrigger("approval-queue")] string instanceId,
    [OrchestrationClient] DurableOrchestrationClient client)
{
    await client.RaiseEventAsync(instanceId, "Approval", true);
}
```

### <a name="javascript-functions-2x-only"></a>JavaScript (yalnızca 2. x Işlevleri)

```javascript
const df = require("durable-functions");

module.exports = async function(context, instanceId) {
    const client = df.getClient(context);
    await client.raiseEvent(instanceId, "Approval", true);
};
```

Dahili olarak `RaiseEventAsync` , (.net) `raiseEvent` veya (JavaScript), bekleyen Orchestrator işlevinin oluşturduğu bir iletiyi kuyruğa alır. Örnek, belirtilen *olay adında* beklemmediyse, olay iletisi bir bellek içi kuyruğa eklenir. Düzenleme örneği daha sonra bu *olay adını* dinlemeye başlarsa, olay iletileri için sırayı denetler.

> [!NOTE]
> Belirtilen *örnek kimliğine*sahip bir düzenleme örneği yoksa, olay iletisi atılır. Bu davranış hakkında daha fazla bilgi için bkz. [GitHub sorunu](https://github.com/Azure/azure-functions-durable-extension/issues/29). 

> [!WARNING]
> JavaScript 'te yerel olarak geliştirme yaparken, ortam değişkenini `WEBSITE_HOSTNAME` `localhost:<port>`, EX olarak ayarlamanız gerekir. `localhost:7071`üzerinde `DurableOrchestrationClient`yöntemleri kullanmak için. Bu gereksinim hakkında daha fazla bilgi için bkz. [GitHub sorunu](https://github.com/Azure/azure-functions-durable-js/issues/28).

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Dış düzenlemeleri ayarlamayı öğrenin](durable-functions-eternal-orchestrations.md)

> [!div class="nextstepaction"]
> [Dış olaylar için bekleyen bir örnek çalıştırma](durable-functions-phone-verification.md)

> [!div class="nextstepaction"]
> [İnsan etkileşimini bekleyen bir örnek çalıştırın](durable-functions-phone-verification.md)