---
title: Dayanıklı İşlevler-Azure 'da Eternal düzenlemeleri
description: Azure işlevleri için dayanıklı işlevler uzantısını kullanarak dış düzenlemeleri uygulamayı öğrenin.
services: functions
author: cgillum
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: azfuncdf
ms.openlocfilehash: d96229bb5e3d288915b64e5a7ce29a8651f2a181
ms.sourcegitcommit: 42748f80351b336b7a5b6335786096da49febf6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72177374"
---
# <a name="eternal-orchestrations-in-durable-functions-azure-functions"></a>Dayanıklı İşlevler 'de Eternal düzenlemeleri (Azure Işlevleri)

*Eternal düzenlemeleri* , hiçbir şekilde bitmeyeceğinden Orchestrator işlevleridir. Bu, toplayıcısını değiştirme için [dayanıklı işlevler](durable-functions-overview.md) kullanmak istediğinizde ve sonsuz bir döngü gerektiren herhangi bir senaryoda yararlıdır.

## <a name="orchestration-history"></a>Düzenleme geçmişi

[Orchestration geçmişi](durable-functions-orchestrations.md#orchestration-history) konusunda açıklandığı gibi, dayanıklı görev çerçevesi her bir işlev düzenlemesi geçmişini izler. Orchestrator işlevi yeni çalışma zamanlamaya devam ettiği sürece bu geçmiş sürekli olarak artar. Orchestrator işlevi sonsuz bir döngüye geçtiğinde ve sürekli olarak iş zamanlıyor ise, bu geçmiş kritik ölçüde büyüyerek önemli performans sorunlarına neden olabilir. *Dış düzenleme* kavramı, sonsuz döngüler gerektiren uygulamalar için bu tür sorunları azaltmak üzere tasarlanmıştır.

## <a name="resetting-and-restarting"></a>Sıfırlama ve yeniden başlatma

Sonsuz döngüler kullanmak yerine, Orchestrator işlevleri [Continueasnew](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_ContinueAsNew_) yöntemini çağırarak durumlarını sıfırlar. Bu yöntem, bir sonraki Orchestrator işlevi oluşturma için yeni giriş haline gelen tek bir JSON-Serializable parametresi alır.

@No__t-0 çağrıldığında, örnek çıkış yapmadan önce bir iletiyi kendisine sıraya alır. İleti, örneği yeni giriş değeri ile yeniden başlatır. Aynı örnek KIMLIĞI tutulur, ancak Orchestrator işlevinin geçmişi etkin bir şekilde kesilir.

> [!NOTE]
> Dayanıklı görev çerçevesi aynı örnek KIMLIĞINI tutar, ancak dahili olarak `ContinueAsNew` tarafından sıfırlanan Orchestrator işlevi için yeni bir *yürütme kimliği* oluşturur. Bu yürütme KIMLIĞI genellikle dışarıdan gösterilmez, ancak düzenleme yürütmesinin hata ayıklaması sırasında öğrenmek faydalı olabilir.

## <a name="periodic-work-example"></a>Düzenli iş örneği

Dış düzenlemeleri için bir kullanım örneği, düzenli olarak sürekli çalışması gereken koddur.

### <a name="c"></a>C#

```csharp
[FunctionName("Periodic_Cleanup_Loop")]
public static async Task Run(
    [OrchestrationTrigger] DurableOrchestrationContext context)
{
    await context.CallActivityAsync("DoCleanup", null);

    // sleep for one hour between cleanups
    DateTime nextCleanup = context.CurrentUtcDateTime.AddHours(1);
    await context.CreateTimer(nextCleanup, CancellationToken.None);

    context.ContinueAsNew(null);
}
```

### <a name="javascript-functions-2x-only"></a>JavaScript (yalnızca 2. x Işlevleri)

```javascript
const df = require("durable-functions");
const moment = require("moment");

module.exports = df.orchestrator(function*(context) {
    yield context.df.callActivity("DoCleanup");

    // sleep for one hour between cleanups
    const nextCleanup = moment.utc(context.df.currentUtcDateTime).add(1, "h");
    yield context.df.createTimer(nextCleanup.toDate());

    context.df.continueAsNew(undefined);
});
```

Bu örnek ve Zamanlayıcı tarafından tetiklenen bir işlev arasındaki fark, temizleme tetikleme sürelerinin bir zamanlamaya göre değil, burada yer alır. Örneğin, bir işlevi her saat yürüten bir CRON zamanlaması, bunu 1:00, 2:00, 3:00 vb. ve büyük olasılıkla çakışma sorunları halinde çalıştıracaktır. Bu örnekte, temizleme işlemi 30 dakika sürüyorsa, 1:00, 2:30, 4:00, vb. olarak zamanlanır.

## <a name="starting-an-eternal-orchestration"></a>Dış düzenleme başlatılıyor
Bir dış düzenlemesi başlatmak için [startnewasync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_StartNewAsync_) yöntemini kullanın. Bu, başka bir düzenleme işlevini tetiklemeden farklı değildir.  

> [!NOTE]
> Tek bir dış Orchestration 'un çalıştığından emin olmanız gerekiyorsa, düzenleme başlatılırken aynı örneğin `id` ' ı korumak önemlidir. Daha fazla bilgi için bkz. [örnek yönetimi](durable-functions-instance-management.md).

```csharp
[FunctionName("Trigger_Eternal_Orchestration")]
public static async Task<HttpResponseMessage> OrchestrationTrigger(
    [HttpTrigger(AuthorizationLevel.Function, "post", Route = null)] HttpRequestMessage request,
    [OrchestrationClient] DurableOrchestrationClientBase client)
{
    string instanceId = "StaticId";
    // Null is used as the input, since there is no input in "Periodic_Cleanup_Loop".
    await client.StartNewAsync("Periodic_Cleanup_Loop", instanceId, null); 
    return client.CreateCheckStatusResponse(request, instanceId);
}
```

## <a name="exit-from-an-eternal-orchestration"></a>Bir dış Orchestration 'tan çıkma

Bir Orchestrator işlevinin sonunda tamamlanması gerekiyorsa, tek yapmanız gereken `ContinueAsNew` *çağırmamalıdır* ve işlevin çıkmasına izin vermez.

Orchestrator işlevi sonsuz bir döngüde ve durdurulması gerekiyorsa, durdurmak için [Sonlandırateasync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_TerminateAsync_) yöntemini kullanın. Daha fazla bilgi için bkz. [örnek yönetimi](durable-functions-instance-management.md).

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Tek tek düzenlemeleri nasıl uygulayacağınızı öğrenin](durable-functions-singletons.md)
