---
title: Dayanıklı İşlevler zamanlayıcılar-Azure
description: Azure Işlevleri için Dayanıklı İşlevler uzantısında dayanıklı zamanlayıcıları nasıl uygulayacağınızı öğrenin.
services: functions
author: ggailey777
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.topic: conceptual
ms.date: 12/08/2018
ms.author: azfuncdf
ms.openlocfilehash: 11edfc11fc1e54684a99774c21517d4c322348b1
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70087053"
---
# <a name="timers-in-durable-functions-azure-functions"></a>Dayanıklı İşlevler süreölçerler (Azure Işlevleri)

[Dayanıklı işlevler](durable-functions-overview.md) , gecikme süreleri uygulamak veya zaman uyumsuz eylemlerde zaman aşımları ayarlamak için Orchestrator işlevlerinde kullanılmak üzere *dayanıklı zamanlayıcılar* sağlar. Dayanıklı `Thread.Sleep` zamanlayıcılar, `setInterval()` ve `Task.Delay` (C#) veya `setTimeout()` ve (JavaScript) yerine Orchestrator işlevlerinde kullanılmalıdır.

.Net 'teki [durableorchestrationcontext](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html) `createTimer` `DurableOrchestrationContext` [CreateTimer](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_CreateTimer_) yöntemini veya JavaScript içindeki yöntemini çağırarak dayanıklı bir Zamanlayıcı oluşturursunuz. Yöntemi, belirtilen tarih ve saatte devam eden bir görev döndürür.

## <a name="timer-limitations"></a>Süreölçer sınırlamaları

4:30 pm tarihinde süresi dolan bir Zamanlayıcı oluşturduğunuzda, temeldeki dayanıklı görev çerçevesi yalnızca 4:30 PM 'de görünür hale gelen bir iletiyi sıraya alır. Azure Işlevleri tüketim planında çalışırken, yeni görünür Zamanlayıcı iletisi, işlev uygulamasının uygun bir VM üzerinde etkinleştirilmesini sağlayacaktır.

> [!NOTE]
> * Azure depolama 'daki sınırlamalar nedeniyle dayanıklı zamanlayıcılar 7 günden daha uzun olamaz. Bir özellik isteği üzerinde, [zamanlayıcıları 7 günden fazla bir süre sonra uzatmak için](https://github.com/Azure/azure-functions-durable-extension/issues/14)çalışıyoruz.
> * Sürekli `DateTime.UtcNow` olarak bir zamanlayıcının göreli son tarihini `currentUtcDateTime` hesaplarken aşağıdaki örneklerde gösterildiği gibi, her zaman `Date.now` .NET veya `Date.UTC` JavaScript yerine [CurrentUtcDateTime](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_CurrentUtcDateTime) kullanın.

## <a name="usage-for-delay"></a>Gecikme kullanımı

Aşağıdaki örnek, bir erteleme yürütmesi için dayanıklı zamanlayıcıları nasıl kullanacağınızı gösterir. Bu örnek, on gün boyunca her gün bir fatura bildirimi yayınlanıyor.

### <a name="c"></a>C#

```csharp
[FunctionName("BillingIssuer")]
public static async Task Run(
    [OrchestrationTrigger] DurableOrchestrationContext context)
{
    for (int i = 0; i < 10; i++)
    {
        DateTime deadline = context.CurrentUtcDateTime.Add(TimeSpan.FromDays(1));
        await context.CreateTimer(deadline, CancellationToken.None);
        await context.CallActivityAsync("SendBillingEvent");
    }
}
```

### <a name="javascript-functions-2x-only"></a>JavaScript (yalnızca 2. x Işlevleri)

```js
const df = require("durable-functions");
const moment = require("moment");

module.exports = df.orchestrator(function*(context) {
    for (let i = 0; i < 10; i++) {
        const dayOfMonth = context.df.currentUtcDateTime.getDate();
        const deadline = moment.utc(context.df.currentUtcDateTime).add(1, 'd');
        yield context.df.createTimer(deadline.toDate());
        yield context.df.callActivity("SendBillingEvent");
    }
});
```

> [!WARNING]
> Orchestrator işlevlerinde sonsuz döngüleri önleyin. Sonsuz döngü senaryolarını güvenle ve verimli bir şekilde uygulama hakkında daha fazla bilgi için bkz. [Eternal düzenlemeleri](durable-functions-eternal-orchestrations.md).

## <a name="usage-for-timeout"></a>Zaman aşımı kullanımı

Bu örnek, zaman aşımlarını uygulamak için dayanıklı zamanlayıcıları nasıl kullanacağınızı gösterir.

### <a name="c"></a>C#

```csharp
[FunctionName("TryGetQuote")]
public static async Task<bool> Run(
    [OrchestrationTrigger] DurableOrchestrationContext context)
{
    TimeSpan timeout = TimeSpan.FromSeconds(30);
    DateTime deadline = context.CurrentUtcDateTime.Add(timeout);

    using (var cts = new CancellationTokenSource())
    {
        Task activityTask = context.CallActivityAsync("GetQuote");
        Task timeoutTask = context.CreateTimer(deadline, cts.Token);

        Task winner = await Task.WhenAny(activityTask, timeoutTask);
        if (winner == activityTask)
        {
            // success case
            cts.Cancel();
            return true;
        }
        else
        {
            // timeout case
            return false;
        }
    }
}
```

### <a name="javascript-functions-2x-only"></a>JavaScript (yalnızca 2. x Işlevleri)

```js
const df = require("durable-functions");
const moment = require("moment");

module.exports = df.orchestrator(function*(context) {
    const deadline = moment.utc(context.df.currentUtcDateTime).add(30, "s");

    const activityTask = context.df.callActivity("GetQuote");
    const timeoutTask = context.df.createTimer(deadline.toDate());

    const winner = yield context.df.Task.any([activityTask, timeoutTask]);
    if (winner === activityTask) {
        // success case
        timeoutTask.cancel();
        return true;
    }
    else
    {
        // timeout case
        return false;
    }
});
```

> [!WARNING]
> Kodunuzun tamamlanmasını `CancellationTokenSource` beketmeyeceğini dayanıklı bir zamanlayıcıyıC#() veya `cancel()` döndürülen `TimerTask` (JavaScript) çağrısını iptal etmek için bir kullanın. Tüm bekleyen görevler tamamlanana veya iptal edilene kadar dayanıklı görev çerçevesi bir düzenleme durumunun "tamamlandı" olarak değiştirilmesine izin vermez.

Bu mekanizma, devam eden etkinlik işlevi yürütmeyi gerçekten sonlandırır. Bunun yerine, Orchestrator işlevinin sonucu yok saymasına ve üzerinde hareket etmesine izin verir. İşlev uygulamanız tüketim planını kullanıyorsa, terk edilen etkinlik işlevi tarafından tüketilen herhangi bir zaman ve bellek için faturalandırılırsınız. Varsayılan olarak, tüketim planında çalışan işlevlerin beş dakikalık bir zaman aşımı vardır. Bu sınır aşılırsa, tüm yürütmeyi durdurmak ve geri ödeme durumunun önlenmesi için Azure Işlevleri ana bilgisayarı geri dönüştürülür. [İşlev zaman aşımı yapılandırılabilir](../functions-host-json.md#functiontimeout).

Orchestrator işlevlerinde zaman aşımları uygulama hakkında daha ayrıntılı bir örnek için bkz. [ınsan etkileşimi & zaman aşımları-telefon doğrulama](durable-functions-phone-verification.md) Kılavuzu.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Dış olayları nasıl yükselteceğinizi ve işleyeceğinizi öğrenin](durable-functions-external-events.md)
