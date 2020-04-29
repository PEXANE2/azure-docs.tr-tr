---
title: Dayanıklı İşlevler-Azure 'da Eternal düzenlemeleri
description: Azure işlevleri için dayanıklı işlevler uzantısını kullanarak dış düzenlemeleri uygulamayı öğrenin.
author: cgillum
ms.topic: conceptual
ms.date: 11/02/2019
ms.author: azfuncdf
ms.openlocfilehash: d55e08fecbd1338284607ac59fe354c6fa8cb1ea
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80478808"
---
# <a name="eternal-orchestrations-in-durable-functions-azure-functions"></a>Dayanıklı İşlevler 'de Eternal düzenlemeleri (Azure Işlevleri)

*Eternal düzenlemeleri* , hiçbir şekilde bitmeyeceğinden Orchestrator işlevleridir. Bu, toplayıcısını değiştirme için [dayanıklı işlevler](durable-functions-overview.md) kullanmak istediğinizde ve sonsuz bir döngü gerektiren herhangi bir senaryoda yararlıdır.

## <a name="orchestration-history"></a>Düzenleme geçmişi

[Orchestration geçmişi](durable-functions-orchestrations.md#orchestration-history) konusunda açıklandığı gibi, dayanıklı görev çerçevesi her bir işlev düzenlemesi geçmişini izler. Orchestrator işlevi yeni çalışma zamanlamaya devam ettiği sürece bu geçmiş sürekli olarak artar. Orchestrator işlevi sonsuz bir döngüye geçtiğinde ve sürekli olarak iş zamanlıyor ise, bu geçmiş kritik ölçüde büyüyerek önemli performans sorunlarına neden olabilir. *Dış düzenleme* kavramı, sonsuz döngüler gerektiren uygulamalar için bu tür sorunları azaltmak üzere tasarlanmıştır.

## <a name="resetting-and-restarting"></a>Sıfırlama ve yeniden başlatma

Sonsuz döngüler kullanmak yerine, düzenleyici işlevleri [düzenleme tetikleyicisi bağlamasının](durable-functions-bindings.md#orchestration-trigger) `ContinueAsNew` (.net) veya `continueAsNew` (JavaScript) yöntemini çağırarak durumlarını sıfırlar. Bu yöntem, bir sonraki Orchestrator işlevi oluşturma için yeni giriş haline gelen tek bir JSON-Serializable parametresi alır.

`ContinueAsNew` Çağrıldığında, örnek çıkış yapmadan önce bir iletiyi kendisine sıraya alır. İleti, örneği yeni giriş değeri ile yeniden başlatır. Aynı örnek KIMLIĞI tutulur, ancak Orchestrator işlevinin geçmişi etkin bir şekilde kesilir.

> [!NOTE]
> Dayanıklı görev çerçevesi aynı örnek KIMLIĞINI tutar, ancak tarafından `ContinueAsNew`sıfırlanan Orchestrator işlevi için dahili olarak yeni BIR *yürütme kimliği* oluşturur. Bu yürütme KIMLIĞI genellikle dışarıdan gösterilmez, ancak düzenleme yürütmesinin hata ayıklaması sırasında öğrenmek faydalı olabilir.

## <a name="periodic-work-example"></a>Düzenli iş örneği

Dış düzenlemeleri için bir kullanım örneği, düzenli olarak sürekli çalışması gereken koddur.

# <a name="c"></a>[, #](#tab/csharp)

```csharp
[FunctionName("Periodic_Cleanup_Loop")]
public static async Task Run(
    [OrchestrationTrigger] IDurableOrchestrationContext context)
{
    await context.CallActivityAsync("DoCleanup", null);

    // sleep for one hour between cleanups
    DateTime nextCleanup = context.CurrentUtcDateTime.AddHours(1);
    await context.CreateTimer(nextCleanup, CancellationToken.None);

    context.ContinueAsNew(null);
}
```

> [!NOTE]
> Önceki C# örneği Dayanıklı İşlevler 2. x içindir. Dayanıklı İşlevler 1. x için yerine kullanmanız `DurableOrchestrationContext` gerekir. `IDurableOrchestrationContext` Sürümler arasındaki farklılıklar hakkında daha fazla bilgi için [dayanıklı işlevler sürümler](durable-functions-versions.md) makalesine bakın.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

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

---

Bu örnek ve Zamanlayıcı tarafından tetiklenen bir işlev arasındaki fark, temizleme tetikleme sürelerinin bir zamanlamaya göre değil, burada yer alır. Örneğin, bir işlevi her saat yürüten bir CRON zamanlaması, bunu 1:00, 2:00, 3:00 vb. ve büyük olasılıkla çakışma sorunları halinde çalıştıracaktır. Bu örnekte, temizleme işlemi 30 dakika sürüyorsa, 1:00, 2:30, 4:00, vb. olarak zamanlanır.

## <a name="starting-an-eternal-orchestration"></a>Dış düzenleme başlatılıyor

Diğer düzenleme `StartNewAsync` işlevlerine benzer şekilde, `startNew` dış bir düzenleme başlatmak için (.net) veya (JavaScript) yöntemini kullanın.  

> [!NOTE]
> Tek bir dış Orchestration 'un çalıştığından emin olmanız gerekiyorsa, düzenleme başlatılırken aynı örneği `id` korumak önemlidir. Daha fazla bilgi için bkz. [örnek yönetimi](durable-functions-instance-management.md).

# <a name="c"></a>[, #](#tab/csharp)

```csharp
[FunctionName("Trigger_Eternal_Orchestration")]
public static async Task<HttpResponseMessage> OrchestrationTrigger(
    [HttpTrigger(AuthorizationLevel.Function, "post", Route = null)] HttpRequestMessage request,
    [DurableClient] IDurableOrchestrationClient client)
{
    string instanceId = "StaticId";

    await client.StartNewAsync("Periodic_Cleanup_Loop", instanceId); 
    return client.CreateCheckStatusResponse(request, instanceId);
}
```

> [!NOTE]
> Önceki kod Dayanıklı İşlevler 2. x içindir. `OrchestrationClient` Dayanıklı işlevler 1. x için `DurableClient` özniteliği yerine özniteliği kullanmanız gerekir ve yerine `DurableOrchestrationClient` parametre türünü kullanmanız gerekir. `IDurableOrchestrationClient` Sürümler arasındaki farklılıklar hakkında daha fazla bilgi için [dayanıklı işlevler sürümler](durable-functions-versions.md) makalesine bakın.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

```javascript
const df = require("durable-functions");

module.exports = async function (context, req) {
    const client = df.getClient(context);
    const instanceId = "StaticId";
    
    // null is used as the input, since there is no input in "Periodic_Cleanup_Loop".
    await client.startNew("Periodic_Cleanup_Loop", instanceId, null);

    context.log(`Started orchestration with ID = '${instanceId}'.`);
    return client.createCheckStatusResponse(context.bindingData.req, instanceId);
};
```

---

## <a name="exit-from-an-eternal-orchestration"></a>Bir dış Orchestration 'tan çıkma

Bir Orchestrator işlevinin sonunda tamamlanması gerekiyorsa, tüm yapmanız gereken çağrı `ContinueAsNew` *yapmamalıdır* ve işlevin çıkmasına izin vermez.

Orchestrator işlevi sonsuz bir döngüde ve durdurulması gerekiyorsa, bunu durdurmak için [Orchestration istemci bağlamasının](durable-functions-bindings.md#orchestration-client) `TerminateAsync` (.net) veya `terminate` (JavaScript) metodunu kullanın. Daha fazla bilgi için bkz. [örnek yönetimi](durable-functions-instance-management.md).

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Tek tek düzenlemeleri nasıl uygulayacağınızı öğrenin](durable-functions-singletons.md)
