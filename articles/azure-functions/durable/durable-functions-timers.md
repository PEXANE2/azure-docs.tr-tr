---
title: Dayanıklı İşlevlerde Zamanlayıcılar - Azure
description: Azure İşlevler için Dayanıklı İşlevler uzantısında dayanıklı zamanlayıcıları nasıl uygulayacağınızı öğrenin.
ms.topic: conceptual
ms.date: 11/03/2019
ms.author: azfuncdf
ms.openlocfilehash: 0565cc149a36baf31d8516fffcf48b194c465760
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76261492"
---
# <a name="timers-in-durable-functions-azure-functions"></a>Dayanıklı İşlevlerde Zamanlayıcılar (Azure Fonksiyonları)

[Dayanıklı Fonksiyonlar,](durable-functions-overview.md) gecikmeleri uygulamak veya async eylemleri üzerinde zaman zaman aşması ayarlamak için orchestrator işlevlerinde kullanım için *dayanıklı zamanlayıcılar* sağlar. Dayanıklı `Thread.Sleep` zamanlayıcılar yerine ve `Task.Delay` (C#) veya `setTimeout()` `setInterval()` (JavaScript) yerine orchestrator işlevleri kullanılmalıdır.

(.NET) yöntemini `CreateTimer` veya `createTimer` (JavaScript) yöntemini çağırarak dayanıklı bir zamanlayıcı [oluşturursunuz.](durable-functions-bindings.md#orchestration-trigger) Yöntem, belirli bir tarih ve saatte tamamlayan bir görevi döndürür.

## <a name="timer-limitations"></a>Zamanlayıcı sınırlamaları

Saat 16:30'da sona erecek bir zamanlayıcı oluşturduğunuzda, altta yatan Dayanıklı Görev Çerçevesi yalnızca 16:30'da görünür hale gelen bir ileti sıraya girer. Azure İşlevler Tüketimi planında çalışırken, yeni görünen zamanlayıcı iletisi işlev uygulamasının uygun bir VM'de etkinleştirilmesini sağlar.

> [!NOTE]
> * Dayanıklı zamanlayıcılar şu anda 7 gün ile sınırlıdır. Daha uzun gecikmeler gerekiyorsa, bir `while` döngü içinde zamanlayıcı API'leri kullanılarak simüle edilebilir.
> * Dayanıklı `CurrentUtcDateTime` zamanlayıcılar için yangın süresini hesaplarken her zaman .NET yerine `DateTime.UtcNow` veya `currentUtcDateTime` JavaScript yerine `Date.now` veya `Date.UTC` JavaScript'te kullanın. Daha fazla bilgi [için, orchestrator işlev kodu kısıtlamaları](durable-functions-code-constraints.md) makalesine bakın.

## <a name="usage-for-delay"></a>Gecikme için kullanım

Aşağıdaki örnek, yürütmeyi geciktirmek için dayanıklı zamanlayıcıların nasıl kullanılacağını göstermektedir. Örnek, 10 gün boyunca her gün bir fatura bildirimi veriyor.

# <a name="c"></a>[C #](#tab/csharp)

```csharp
[FunctionName("BillingIssuer")]
public static async Task Run(
    [OrchestrationTrigger] IDurableOrchestrationContext context)
{
    for (int i = 0; i < 10; i++)
    {
        DateTime deadline = context.CurrentUtcDateTime.Add(TimeSpan.FromDays(1));
        await context.CreateTimer(deadline, CancellationToken.None);
        await context.CallActivityAsync("SendBillingEvent");
    }
}
```

> [!NOTE]
> Önceki C# örneği Dayanıklı Fonksiyonlar 2.x'i hedefler. Dayanıklı Fonksiyonlar 1.x `DurableOrchestrationContext` `IDurableOrchestrationContext`için, 'yi yerine kullanmalısınız. Sürümler arasındaki farklar hakkında daha fazla bilgi için [Dayanıklı Işlevler sürümleri](durable-functions-versions.md) makalesine bakın.

# <a name="javascript"></a>[Javascript](#tab/javascript)

```js
const df = require("durable-functions");
const moment = require("moment");

module.exports = df.orchestrator(function*(context) {
    for (let i = 0; i < 10; i++) {
        const deadline = moment.utc(context.df.currentUtcDateTime).add(1, 'd');
        yield context.df.createTimer(deadline.toDate());
        yield context.df.callActivity("SendBillingEvent");
    }
});
```

---

> [!WARNING]
> Orkestratör işlevlerinde sonsuz döngülerden kaçının. Sonsuz döngü senaryolarının güvenli ve verimli bir şekilde nasıl uygulanacağı hakkında bilgi için [Sonsuz Orkestrasyon'a](durable-functions-eternal-orchestrations.md)bakın.

## <a name="usage-for-timeout"></a>Zaman adabı için kullanım

Bu örnek, zaman zaman larını uygulamak için dayanıklı zamanlayıcıların nasıl kullanılacağını göstermektedir.

# <a name="c"></a>[C #](#tab/csharp)

```csharp
[FunctionName("TryGetQuote")]
public static async Task<bool> Run(
    [OrchestrationTrigger] IDurableOrchestrationContext context)
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

> [!NOTE]
> Önceki C# örneği Dayanıklı Fonksiyonlar 2.x'i hedefler. Dayanıklı Fonksiyonlar 1.x `DurableOrchestrationContext` `IDurableOrchestrationContext`için, 'yi yerine kullanmalısınız. Sürümler arasındaki farklar hakkında daha fazla bilgi için [Dayanıklı Işlevler sürümleri](durable-functions-versions.md) makalesine bakın.

# <a name="javascript"></a>[Javascript](#tab/javascript)

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

---

> [!WARNING]
> Kodunuzun tamamlanmasını beklememesi durumunda dayanıklı bir zamanlayıcıyı iptal etmek için (.NET) `CancellationTokenSource` veya döndürülen `cancel()` `TimerTask` (JavaScript) üzerinde arama kullanın. Dayanıklı Görev Çerçevesi, bekleyen tüm görevler tamamlanana veya iptal edilene kadar bir orkestrasyonun durumunu "tamamlanmış" olarak değiştirmez.

Bu iptal mekanizması devam eden etkinlik işlevini veya alt orkestrasyon yürütmelerini sonlandırmaz. Bunun yerine, sadece orkestratör işlevi sonucu göz ardı etmek ve hareket sağlar. İşlev uygulamanız Tüketim planını kullanıyorsa, terk edilen etkinlik işlevi tarafından tüketilen herhangi bir zaman ve bellek için faturalandırılırsınız. Varsayılan olarak, Tüketim planında çalışan işlevlerin zaman ası beş dakikadır. Bu sınır aşılırsa, Azure İşlevler ana bilgisayar tüm yürütmeyi durdurmak ve kaçak faturalandırma durumunu önlemek için geri dönüştürülür. [İşlev zaman yapılandırılabilir.](../functions-host-json.md#functiontimeout)

Orkestratör işlevlerinde zaman aşamaların nasıl uygulanacağının daha ayrıntılı bir örneği [için, İnsan Etkileşimi & Zaman Çıkışları - Telefon Doğrulama](durable-functions-phone-verification.md) makalesine bakın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Dış olayları nasıl yükseltip işleyeceğinizi öğrenin](durable-functions-external-events.md)
