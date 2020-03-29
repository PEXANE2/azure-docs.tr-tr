---
title: Dayanıklı İşlevlerde dış olayları işleme - Azure
description: Azure İşlevler için Dayanıklı İşlevler uzantısındaki dış olayları nasıl işleyeceğinizi öğrenin.
ms.topic: conceptual
ms.date: 11/02/2019
ms.author: azfuncdf
ms.openlocfilehash: 0877161f8d668141c8efb7c06b10643bf209341f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76262971"
---
# <a name="handling-external-events-in-durable-functions-azure-functions"></a>Dayanıklı İşlevlerde dış olayları işleme (Azure İşlevleri)

Orkestratör işlevleri dış olayları bekleme ve dinleme yeteneğine sahiptir. [Dayanıklı İşlevler'in](durable-functions-overview.md) bu özelliği genellikle insan etkileşimini veya diğer dış tetikleyicileri işlemek için yararlıdır.

> [!NOTE]
> Dış olaylar tek yönlü eşzamanlı işlemlerdir. Bunlar, olayı gönderen istemcinin orkestratör işlevinden eşzamanlı yanıt alması gereken durumlar için uygun değildir.

## <a name="wait-for-events"></a>Etkinlikleri bekleyin

`WaitForExternalEvent` (.NET) ve `waitForExternalEvent` (JavaScript) [düzenleme tetikleme bağlama](durable-functions-bindings.md#orchestration-trigger) yöntemleri, bir orkestratör işlevinin harici bir olayı eş senkronize bir şekilde beklemesini ve dinlemesini sağlar. Dinleme orkestratörü işlevi, olayın *adını* ve almayı beklediği *verilerin şeklini* bildirir.

# <a name="c"></a>[C #](#tab/csharp)

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
> Önceki C# kodu Dayanıklı Fonksiyonlar 2.x içindir. Dayanıklı Fonksiyonlar 1.x `DurableOrchestrationContext` `IDurableOrchestrationContext`için, 'yi yerine kullanmalısınız. Sürümler arasındaki farklar hakkında daha fazla bilgi için [Dayanıklı Işlevler sürümleri](durable-functions-versions.md) makalesine bakın.

# <a name="javascript"></a>[Javascript](#tab/javascript)

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

Önceki örnek belirli bir tek olayı dinler ve alındığı zaman harekete geçer.

Aşağıdaki örnekte olduğu gibi, üç olası olay bildiriminden birini bekleyen birden çok olayı aynı anda dinleyebilirsiniz.

# <a name="c"></a>[C #](#tab/csharp)

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
> Önceki C# kodu Dayanıklı Fonksiyonlar 2.x içindir. Dayanıklı Fonksiyonlar 1.x `DurableOrchestrationContext` `IDurableOrchestrationContext`için, 'yi yerine kullanmalısınız. Sürümler arasındaki farklar hakkında daha fazla bilgi için [Dayanıklı Işlevler sürümleri](durable-functions-versions.md) makalesine bakın.

# <a name="javascript"></a>[Javascript](#tab/javascript)

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

Önceki örnek, birden çok olaydan *herhangi* birini dinler. *Tüm* olayları beklemek de mümkündür.

# <a name="c"></a>[C #](#tab/csharp)

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
> Önceki kod Dayanıklı Fonksiyonlar 2.x içindir. Dayanıklı Fonksiyonlar 1.x `DurableOrchestrationContext` `IDurableOrchestrationContext`için, 'yi yerine kullanmalısınız. Sürümler arasındaki farklar hakkında daha fazla bilgi için [Dayanıklı Işlevler sürümleri](durable-functions-versions.md) makalesine bakın.

.NET'te, olay yükü beklenen türe `T`dönüştürülemiyorsa, bir özel durum atılır.

# <a name="javascript"></a>[Javascript](#tab/javascript)

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

`WaitForExternalEvent`bazı giriş için süresiz olarak bekler.  İşlev uygulaması beklerken güvenli bir şekilde boşaltılabilir. Bu orkestrasyon örneği için bir olay geldiğinde, otomatik olarak uyandırılır ve olayı hemen işler.

> [!NOTE]
> İşlev uygulamanız Tüketim Planı'nı kullanıyorsa, ne kadar beklerse beklemesin, `WaitForExternalEvent` (.NET) veya `waitForExternalEvent` (JavaScript) bir orkestratör işlevi görev beklerken faturalandırma ücreti alınmaz.

## <a name="send-events"></a>Olayları gönderme

`RaiseEventAsync` (.NET) veya `raiseEvent` (JavaScript) [düzenleme istemcisi bağlama](durable-functions-bindings.md#orchestration-client) yöntemi `WaitForExternalEvent` (.NET) `waitForExternalEvent` veya (JavaScript) beklediği olayları gönderir.  Yöntem, `RaiseEventAsync` *eventName* ve *eventData'yı* parametre olarak alır. Olay verileri JSON serileştirilebilir olmalıdır.

Aşağıda, bir orkestratör işlev örneğine "Onay" olayı gönderen örnek bir sıra tetiklenmiş işlev verilmiştir. Düzenleme örneği kimliği sıra iletisinin gövdesinden gelir.

# <a name="c"></a>[C #](#tab/csharp)

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
> Önceki C# kodu Dayanıklı Fonksiyonlar 2.x içindir. Dayanıklı Fonksiyonlar 1.x için `OrchestrationClient` öznitelik yerine `DurableClient` öznitelik kullanmanız gerekir `DurableOrchestrationClient` ve `IDurableOrchestrationClient`parametre türünü yerine kullanmalısınız. Sürümler arasındaki farklar hakkında daha fazla bilgi için [Dayanıklı Işlevler sürümleri](durable-functions-versions.md) makalesine bakın.

# <a name="javascript"></a>[Javascript](#tab/javascript)

```javascript
const df = require("durable-functions");

module.exports = async function(context, instanceId) {
    const client = df.getClient(context);
    await client.raiseEvent(instanceId, "Approval", true);
};
```

---

Dahili `RaiseEventAsync` olarak, (.NET) veya `raiseEvent` (JavaScript) bekleyen orkestratör işlevi tarafından alınan bir ileti nin sırasını alır. Örnek belirtilen *olay adı* üzerinde beklemiyorsa, olay iletisi bellek içi sıraya eklenir. Düzenleme örneği daha sonra bu olay adını dinlemeye *başlarsa,* olay iletileri için sırayı denetler.

> [!NOTE]
> Belirtilen *örnek kimliği*yle bir düzenleme örneği yoksa, olay iletisi atılır.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Hata işlemeyi nasıl uygulayacağınızı öğrenin](durable-functions-error-handling.md)

> [!div class="nextstepaction"]
> [İnsan etkileşimini bekleyen bir örnek çalıştırın](durable-functions-phone-verification.md)