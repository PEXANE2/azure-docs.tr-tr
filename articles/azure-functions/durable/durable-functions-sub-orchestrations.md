---
title: Dayanıklı Işlevler için alt orkestrasyonlar - Azure
description: Azure İşlevler için Dayanıklı İşlevler uzantısındaki orkestrasyonlardan orkestrasyon çağırma.
ms.topic: conceptual
ms.date: 11/03/2019
ms.author: azfuncdf
ms.openlocfilehash: d4d599063f727510cbf504ea3d121bdabfe001c9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76261526"
---
# <a name="sub-orchestrations-in-durable-functions-azure-functions"></a>Dayanıklı İşlevlerde Alt Orkestrasyonlar (Azure İşlevleri)

Etkinlik işlevlerini çağırmanın yanı sıra, orkestratör işlevleri diğer orkestratör işlevlerini de çağırabilir. Örneğin, daha küçük orkestratör işlevlerinden oluşan bir kitaplıktan daha büyük bir orkestrasyon oluşturabilirsiniz. Veya bir orkestratör işlevinin birden çok örneğini paralel olarak çalıştırabilirsiniz.

`CallSubOrchestratorAsync` Bir orkestratör işlevi ,NET'teki `CallSubOrchestratorWithRetryAsync` `callSubOrchestrator` veya JavaScript'teki `callSubOrchestratorWithRetry` veya yöntemleri kullanarak başka bir orchestrator işlevini çağırabilir. [Hata İşleme & Telafisi](durable-functions-error-handling.md#automatic-retry-on-failure) makalesi, otomatik yeniden deneme hakkında daha fazla bilgiye sahiptir.

Alt orkestratör işlevleri, arayanın bakış açısından etkinlik işlevleri gibi çalışır. Bir değer döndürebilir, bir özel durum atabilir ve ana orchestrator işlevi tarafından beklenebilir. 
## <a name="example"></a>Örnek

Aşağıdaki örnekte, sağlanması gereken birden çok aygıt olan bir IoT ("Nesnelerin İnterneti") senaryosu gösterilebilir. Aşağıdaki işlev, her aygıt için yürütülmesi gereken sağlama iş akışını temsil eder:

# <a name="c"></a>[C #](#tab/csharp)

```csharp
public static async Task DeviceProvisioningOrchestration(
    [OrchestrationTrigger] IDurableOrchestrationContext context)
{
    string deviceId = context.GetInput<string>();

    // Step 1: Create an installation package in blob storage and return a SAS URL.
    Uri sasUrl = await context.CallActivityAsync<Uri>("CreateInstallationPackage", deviceId);

    // Step 2: Notify the device that the installation package is ready.
    await context.CallActivityAsync("SendPackageUrlToDevice", Tuple.Create(deviceId, sasUrl));

    // Step 3: Wait for the device to acknowledge that it has downloaded the new package.
    await context.WaitForExternalEvent<bool>("DownloadCompletedAck");

    // Step 4: ...
}
```

# <a name="javascript"></a>[Javascript](#tab/javascript)

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context) {
    const deviceId = context.df.getInput();

    // Step 1: Create an installation package in blob storage and return a SAS URL.
    const sasUrl = yield context.df.callActivity("CreateInstallationPackage", deviceId);

    // Step 2: Notify the device that the installation package is ready.
    yield context.df.callActivity("SendPackageUrlToDevice", { id: deviceId, url: sasUrl });

    // Step 3: Wait for the device to acknowledge that it has downloaded the new package.
    yield context.df.waitForExternalEvent("DownloadCompletedAck");

    // Step 4: ...
});
```

---

Bu orkestratör işlevi, tek seferlik aygıt sağlama için olduğu gibi kullanılabilir veya daha büyük bir orkestrasyonun parçası olabilir. İkinci durumda, ana orchestrator işlevi `DeviceProvisioningOrchestration` `CallSubOrchestratorAsync` (.NET) veya `callSubOrchestrator` (JavaScript) API kullanarak örnekleri zamanlayabilirsiniz.

Burada, birden çok orkestratör işlevinin paralel olarak nasıl çalıştırılabildiğini gösteren bir örnek verilmiştir.

# <a name="c"></a>[C #](#tab/csharp)

```csharp
[FunctionName("ProvisionNewDevices")]
public static async Task ProvisionNewDevices(
    [OrchestrationTrigger] IDurableOrchestrationContext context)
{
    string[] deviceIds = await context.CallActivityAsync<string[]>("GetNewDeviceIds");

    // Run multiple device provisioning flows in parallel
    var provisioningTasks = new List<Task>();
    foreach (string deviceId in deviceIds)
    {
        Task provisionTask = context.CallSubOrchestratorAsync("DeviceProvisioningOrchestration", deviceId);
        provisioningTasks.Add(provisionTask);
    }

    await Task.WhenAll(provisioningTasks);

    // ...
}
```

> [!NOTE]
> Önceki C# örnekleri Dayanıklı Fonksiyonlar 2.x içindir. Dayanıklı Fonksiyonlar 1.x `DurableOrchestrationContext` `IDurableOrchestrationContext`için, 'yi yerine kullanmalısınız. Sürümler arasındaki farklar hakkında daha fazla bilgi için [Dayanıklı Işlevler sürümleri](durable-functions-versions.md) makalesine bakın.

# <a name="javascript"></a>[Javascript](#tab/javascript)

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context) {
    const deviceIds = yield context.df.callActivity("GetNewDeviceIds");

    // Run multiple device provisioning flows in parallel
    const provisioningTasks = [];
    var id = 0;
    for (const deviceId of deviceIds) {
        const child_id = context.df.instanceId+`:${id}`;
        const provisionTask = context.df.callSubOrchestrator("DeviceProvisioningOrchestration", deviceId, child_id);
        provisioningTasks.push(provisionTask);
        id++;
    }

    yield context.df.Task.all(provisioningTasks);

    // ...
});
```

---

> [!NOTE]
> Alt orkestrasyonlar, ana orkestrasyonla aynı işlev uygulamasında tanımlanmalıdır. Başka bir işlev uygulamasında ki orkestrasyonları aramanız ve beklemeniz gerekiyorsa, HTTP API'leri ve HTTP 202 yoklama tüketici deseni için yerleşik desteği kullanmayı düşünün. Daha fazla bilgi için [HTTP Özellikler](durable-functions-http-features.md) konusuna bakın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Özel bir orkestrasyon durumunu ayarlamayı öğrenin](durable-functions-custom-orchestration-status.md)