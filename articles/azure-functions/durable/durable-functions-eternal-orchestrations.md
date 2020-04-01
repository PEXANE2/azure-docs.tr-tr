---
title: Dayanıklı İşlevlerde Sonsuz Orkestrasyon - Azure
description: Azure İşlevler için Dayanıklı İşlevler uzantısını kullanarak sonsuz işlevleri nasıl uygulayacağınızı öğrenin.
author: cgillum
ms.topic: conceptual
ms.date: 11/02/2019
ms.author: azfuncdf
ms.openlocfilehash: d55e08fecbd1338284607ac59fe354c6fa8cb1ea
ms.sourcegitcommit: efefce53f1b75e5d90e27d3fd3719e146983a780
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80478808"
---
# <a name="eternal-orchestrations-in-durable-functions-azure-functions"></a>Dayanıklı İşlevlerde Sonsuz Orkestrasyon (Azure Fonksiyonları)

*Ebedi orkestrasyonlar* hiç bitmeyen orkestrasyon işlevleridir. Toplayıcılar ve sonsuz döngü gerektiren herhangi bir senaryo için [Dayanıklı İşlevler](durable-functions-overview.md) kullanmak istediğinizde yararlıdır.

## <a name="orchestration-history"></a>Orkestrasyon tarihi

[Orkestrasyon geçmişi](durable-functions-orchestrations.md#orchestration-history) konusunda açıklandığı gibi, Dayanıklı Görev Çerçevesi her işlev intarihini izler. Orkestratör işlevi yeni çalışma zamanlamak için devam ettikçe bu tarih sürekli olarak büyür. Orkestratör işlevi sonsuz bir döngüye girerse ve sürekli olarak çalışmayı planlarsa, bu geçmiş kritik derecede büyüyebilir ve önemli performans sorunlarına neden olabilir. *Ebedi orkestrasyon* kavramı sonsuz döngüler gerektiren uygulamalar için bu tür sorunları azaltmak için tasarlanmıştır.

## <a name="resetting-and-restarting"></a>Sıfırlama ve yeniden başlatma

Sonsuz döngüler kullanmak yerine, orkestratör işlevleri (.NET) veya `ContinueAsNew` `continueAsNew` (JavaScript) yöntemini [çağırarak](durable-functions-bindings.md#orchestration-trigger)durumlarını sıfırlar. Bu yöntem, bir sonraki orchestrator işlev oluşturma için yeni girdi olur tek bir JSON-serializable parametre alır.

Çağrıldığında, `ContinueAsNew` örnek çıkmadan önce bir iletiyi kendisine sıralar. İleti, yeni giriş değeriyle örneği yeniden başlatır. Aynı örnek kimliği tutulur, ancak orkestratör işlevinin geçmişi etkin bir şekilde kesilir.

> [!NOTE]
> Dayanıklı Görev Çerçevesi aynı örnek kimliği korur, ancak dahili olarak tarafından sıfırlanan orchestrator `ContinueAsNew`işlevi için yeni bir yürütme *kimliği* oluşturur. Bu yürütme kimliği genellikle dışarıdan açıklanmaz, ancak yürütme hata ayıklama ne zaman bilmek yararlı olabilir.

## <a name="periodic-work-example"></a>Periyodik çalışma örneği

Ebedi orkestrasyonlar için bir kullanım örneği süresiz olarak periyodik çalışma yapmak için gereken koddur.

# <a name="c"></a>[C#](#tab/csharp)

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
> Önceki C# örneği Dayanıklı Fonksiyonlar 2.x içindir. Dayanıklı Fonksiyonlar 1.x `DurableOrchestrationContext` `IDurableOrchestrationContext`için, 'yi yerine kullanmalısınız. Sürümler arasındaki farklar hakkında daha fazla bilgi için [Dayanıklı Işlevler sürümleri](durable-functions-versions.md) makalesine bakın.

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

Bu örnek le zamanlayıcı tarafından tetiklenen işlev arasındaki fark, buradaki temizleme tetikleyici saatlerinin zamanlamaya dayanmamasıdır. Örneğin, bir işlevi her saat çalıştıran bir CRON zamanlaması işlevi 1:00, 2:00, 3:00 vb. olarak yürütecek ve çakışma sorunlarıyla karşılaşabilir. Bu örnekte, ancak, temizleme 30 dakika sürerse, o zaman 1:00, 2:30, 4:00, vb zamanlanmış olacak ve çakışma şansı yoktur.

## <a name="starting-an-eternal-orchestration"></a>Ebedi bir orkestrasyon başlatma

Ebedi `StartNewAsync` bir orkestrasyon `startNew` başlatmak için (.NET) veya (JavaScript) yöntemini kullanın, tıpkı diğer orkestrasyon işlevinde olduğu gibi.  

> [!NOTE]
> Eğer tek ton ebedi orkestrasyon çalışıyor emin olmak gerekiyorsa, orkestrasyon başlatırken aynı örneği `id` korumak önemlidir. Daha fazla bilgi için [Bkz. Örnek Yönetimi.](durable-functions-instance-management.md)

# <a name="c"></a>[C#](#tab/csharp)

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
> Önceki kod Dayanıklı Fonksiyonlar 2.x içindir. Dayanıklı Fonksiyonlar 1.x için `OrchestrationClient` öznitelik yerine `DurableClient` öznitelik kullanmanız gerekir `DurableOrchestrationClient` ve `IDurableOrchestrationClient`parametre türünü yerine kullanmalısınız. Sürümler arasındaki farklar hakkında daha fazla bilgi için [Dayanıklı Işlevler sürümleri](durable-functions-versions.md) makalesine bakın.

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

## <a name="exit-from-an-eternal-orchestration"></a>Ebedi bir orkestrasyondan çıkın

Bir orkestratör işlevinin sonunda tamamlanması gerekiyorsa, yapmanız gereken tek `ContinueAsNew` şey arama ve işlevin çıkmasına izin *vermemektir.*

Bir orkestratör işlevi sonsuz bir döngüdeyse ve durdurulması `TerminateAsync` gerekiyorsa, onu `terminate` durdurmak için [orkestrasyon istemcisinin](durable-functions-bindings.md#orchestration-client) (.NET) veya (JavaScript) yöntemini kullanın. Daha fazla bilgi için [Bkz. Örnek Yönetimi.](durable-functions-instance-management.md)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Singleton orkestrasyonlarını nasıl uygulayacağınızı öğrenin](durable-functions-singletons.md)
