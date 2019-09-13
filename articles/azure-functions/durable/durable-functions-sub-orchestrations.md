---
title: Dayanıklı İşlevler için alt düzenlemeler-Azure
description: Azure Işlevleri için Dayanıklı İşlevler uzantısı 'ndaki genişletmelerin nasıl çağrılacağını çağırma.
services: functions
author: ggailey777
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.topic: conceptual
ms.date: 09/07/2019
ms.author: azfuncdf
ms.openlocfilehash: 74f33a1ce1026424a6cdb97699223aeb5ff8277f
ms.sourcegitcommit: f3f4ec75b74124c2b4e827c29b49ae6b94adbbb7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2019
ms.locfileid: "70933138"
---
# <a name="sub-orchestrations-in-durable-functions-azure-functions"></a>Dayanıklı İşlevler alt düzenlemeler (Azure Işlevleri)

Orchestrator işlevleri, etkinlik işlevlerini çağırmanın yanı sıra diğer Orchestrator işlevlerini de çağırabilir. Örneğin, bir Orchestrator işlevleri kitaplığından daha büyük bir düzenleme oluşturabilirsiniz. Alternatif olarak, bir Orchestrator işlevinin birden fazla örneğini paralel olarak çalıştırabilirsiniz.

Orchestrator işlevi, .net `callSubOrchestrator` `callSubOrchestratorWithRetry` 'teki [callsuborchestratoraysync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_CallSubOrchestratorAsync_) veya [CallSubOrchestratorWithRetryAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_CallSubOrchestratorWithRetryAsync_) yöntemlerini ya da JavaScript içindeki yöntemleri çağırarak başka bir Orchestrator işlevini çağırabilir. [& Dengeleme makalesindeki hata işleme](durable-functions-error-handling.md#automatic-retry-on-failure) otomatik yeniden deneme hakkında daha fazla bilgi içerir.

Alt Orchestrator işlevleri, çağıranın perspektifinden yalnızca etkinlik işlevleri gibi davranır. Bunlar bir değer döndürebilir, bir özel durum oluşturabilir ve üst Orchestrator işlevi tarafından beklelenebilir.

## <a name="example"></a>Örnek

Aşağıdaki örnekte, sağlanması gereken birden çok cihaz olduğu IoT ("Nesnelerin İnterneti") senaryosu gösterilmektedir. Cihazların her biri için gerçekleşmesi gereken belirli bir düzenleme vardır ve bu, aşağıdakine benzer şekilde görünebilir:

### <a name="c"></a>C#

```csharp
public static async Task DeviceProvisioningOrchestration(
    [OrchestrationTrigger] DurableOrchestrationContext context)
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

### <a name="javascript-functions-2x-only"></a>JavaScript (yalnızca 2. x Işlevleri)

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

Bu Orchestrator işlevi, tek seferlik cihaz sağlama için olduğu gibi kullanılabilir veya daha büyük bir düzenleme parçası olabilir. İkinci durumda `DeviceProvisioningOrchestration` , üst Orchestrator işlevi `CallSubOrchestratorAsync` (C#) veya `callSubOrchestrator` (JavaScript) API 'sini kullanarak örnekleri zamanlayabilir.

Birden çok Orchestrator işlevinin paralel olarak nasıl çalıştırılacağını gösteren bir örnek aşağıda verilmiştir.

### <a name="c"></a>C#

```csharp
[FunctionName("ProvisionNewDevices")]
public static async Task ProvisionNewDevices(
    [OrchestrationTrigger] DurableOrchestrationContext context)
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

### <a name="javascript-functions-2x-only"></a>JavaScript (yalnızca 2. x Işlevleri)

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context) {
    const deviceIds = yield context.df.callActivity("GetNewDeviceIds");

    // Run multiple device provisioning flows in parallel
    const provisioningTasks = [];
    for (const deviceId of deviceIds) {
        const provisionTask = context.df.callSubOrchestrator("DeviceProvisioningOrchestration", deviceId);
        provisioningTasks.push(provisionTask);
    }

    yield context.df.Task.all(provisioningTasks);

    // ...
});
```

> [!NOTE]
> Alt düzenlemeler, üst düzenleme ile aynı işlev uygulamasında tanımlanmalıdır. Başka bir işlev uygulamasındaki düzenlemeleri çağırmanız ve beklemeniz gerekiyorsa, HTTP API 'Leri için yerleşik desteği ve HTTP 202 yoklaması tüketici modelini kullanmayı göz önünde bulundurun. Daha fazla bilgi için bkz. [http özellikleri](durable-functions-http-features.md) konusu.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Özel bir düzenleme durumu ayarlamayı öğrenin](durable-functions-custom-orchestration-status.md)