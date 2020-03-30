---
title: Dayanıklı İşlevlerde işleme hataları - Azure
description: Azure İşlevler için Dayanıklı İşlevler uzantısındaki hataları nasıl işleyeceğinizi öğrenin.
ms.topic: conceptual
ms.date: 11/02/2019
ms.author: azfuncdf
ms.openlocfilehash: 447b3dcf5040835f5a853beff68bde794ece51f5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79277862"
---
# <a name="handling-errors-in-durable-functions-azure-functions"></a>Dayanıklı İşlevlerde (Azure İşlevleri) hataları işleme

Dayanıklı Fonksiyon orkestrasyonlar kod olarak uygulanır ve programlama dilinin yerleşik hata işleme özelliklerini kullanabilir. Gerçekten sizin orkestrasyonlar içine hata işleme ve tazminat eklemek için öğrenmek için gereken herhangi bir yeni kavramlar yoktur. Ancak, bilmeniz gereken birkaç davranış vardır.

## <a name="errors-in-activity-functions"></a>Etkinlik işlevlerindeki hatalar

Bir etkinlik işlevine atılan herhangi bir özel durum, orkestratör işlevine `FunctionFailedException`geri döndürülr ve bir . Orkestratör işlevinde ihtiyaçlarınıza uygun hata işleme ve telafi kodu yazabilirsiniz.

Örneğin, bir hesaptan diğerine para aktaran aşağıdaki orkestratör işlevini göz önünde bulundurun:

# <a name="c"></a>[C #](#tab/csharp)

```csharp
[FunctionName("TransferFunds")]
public static async Task Run([OrchestrationTrigger] IDurableOrchestrationContext context)
{
    var transferDetails = context.GetInput<TransferOperation>();

    await context.CallActivityAsync("DebitAccount",
        new
        {
            Account = transferDetails.SourceAccount,
            Amount = transferDetails.Amount
        });

    try
    {
        await context.CallActivityAsync("CreditAccount",
            new
            {
                Account = transferDetails.DestinationAccount,
                Amount = transferDetails.Amount
            });
    }
    catch (Exception)
    {
        // Refund the source account.
        // Another try/catch could be used here based on the needs of the application.
        await context.CallActivityAsync("CreditAccount",
            new
            {
                Account = transferDetails.SourceAccount,
                Amount = transferDetails.Amount
            });
    }
}
```

> [!NOTE]
> Önceki C# örnekleri Dayanıklı Fonksiyonlar 2.x içindir. Dayanıklı Fonksiyonlar 1.x `DurableOrchestrationContext` `IDurableOrchestrationContext`için, 'yi yerine kullanmalısınız. Sürümler arasındaki farklar hakkında daha fazla bilgi için [Dayanıklı Işlevler sürümleri](durable-functions-versions.md) makalesine bakın.

# <a name="javascript"></a>[Javascript](#tab/javascript)

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context) {
    const transferDetails = context.df.getInput();

    yield context.df.callActivity("DebitAccount",
        {
            account = transferDetails.sourceAccount,
            amount = transferDetails.amount,
        }
    );

    try {
        yield context.df.callActivity("CreditAccount",
            {
                account = transferDetails.destinationAccount,
                amount = transferDetails.amount,
            }
        );
    }
    catch (error) {
        // Refund the source account.
        // Another try/catch could be used here based on the needs of the application.
        yield context.df.callActivity("CreditAccount",
            {
                account = transferDetails.sourceAccount,
                amount = transferDetails.amount,
            }
        );
    }
});
```

---

İlk **CreditAccount** işlev çağrısı başarısız olursa, orkestratör işlevi fonları kaynak hesaba yatırarak telafi eder.

## <a name="automatic-retry-on-failure"></a>Hatada otomatik yeniden deneme

Etkinlik işlevlerini veya alt orkestrasyon işlevlerini çağırdığınızda, otomatik yeniden deneme ilkesi belirtebilirsiniz. Aşağıdaki örnek, bir işlevi en fazla üç kez çağırmayı dener ve her yeniden deneme arasında 5 saniye bekler:

# <a name="c"></a>[C #](#tab/csharp)

```csharp
[FunctionName("TimerOrchestratorWithRetry")]
public static async Task Run([OrchestrationTrigger] IDurableOrchestrationContext context)
{
    var retryOptions = new RetryOptions(
        firstRetryInterval: TimeSpan.FromSeconds(5),
        maxNumberOfAttempts: 3);

    await context.CallActivityWithRetryAsync("FlakyFunction", retryOptions, null);

    // ...
}
```

> [!NOTE]
> Önceki C# örnekleri Dayanıklı Fonksiyonlar 2.x içindir. Dayanıklı Fonksiyonlar 1.x `DurableOrchestrationContext` `IDurableOrchestrationContext`için, 'yi yerine kullanmalısınız. Sürümler arasındaki farklar hakkında daha fazla bilgi için [Dayanıklı Işlevler sürümleri](durable-functions-versions.md) makalesine bakın.

# <a name="javascript"></a>[Javascript](#tab/javascript)

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context) {
    const firstRetryIntervalInMilliseconds = 5000;
    const maxNumberOfAttempts = 3;

    const retryOptions = 
        new df.RetryOptions(firstRetryIntervalInMilliseconds, maxNumberOfAttempts);

    yield context.df.callActivityWithRetry("FlakyFunction", retryOptions);

    // ...
});
```

---

Önceki örnekte etkinlik işlevi çağrısı, otomatik yeniden deneme ilkesini yapılandırmak için bir parametre alır. Otomatik yeniden deneme ilkesini özelleştirmek için çeşitli seçenekler vardır:

* **Maksimum deneme sayısı**: En fazla yeniden deneme denemesi sayısı.
* **İlk yeniden deneme aralığı**: İlk yeniden deneme denemesinden önce bekleme süresi.
* **Geri tepme katsayısı**: Geri tepme artış oranını belirlemek için kullanılan katsayı. Varsayılan olarak 1'dir.
* **Maksimum yeniden deneme aralığı**: Yeniden deneme denemeleri arasında beklemek için maksimum süre.
* **Yeniden deneme zaman ayırma**: Yeniden denemeler yaparak harcanacak maksimum zaman miktarıdır. Varsayılan davranış süresiz olarak yeniden denemektir.
* **Tanıtıcı**: Bir işlevin yeniden denenip denenmemesi gerektiğini belirlemek için kullanıcı tanımlı geri arama belirtilebilir.

## <a name="function-timeouts"></a>Fonksiyon zaman ları

Tamamlanması çok uzun sürüyorsa, bir orkestratör işlevi içindeki bir işlev çağrısını terk etmek isteyebilirsiniz. Bugün bunu yapmanın en uygun yolu, aşağıdaki `context.CreateTimer` örnekte olduğu `context.df.createTimer` gibi (.NET) `Task.WhenAny` veya (JavaScript) ile birlikte (.NET) veya `context.df.Task.any` (JavaScript) kullanarak dayanıklı bir [zamanlayıcı](durable-functions-timers.md) oluşturmaktır:

# <a name="c"></a>[C #](#tab/csharp)

```csharp
[FunctionName("TimerOrchestrator")]
public static async Task<bool> Run([OrchestrationTrigger] IDurableOrchestrationContext context)
{
    TimeSpan timeout = TimeSpan.FromSeconds(30);
    DateTime deadline = context.CurrentUtcDateTime.Add(timeout);

    using (var cts = new CancellationTokenSource())
    {
        Task activityTask = context.CallActivityAsync("FlakyFunction");
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
> Önceki C# örnekleri Dayanıklı Fonksiyonlar 2.x içindir. Dayanıklı Fonksiyonlar 1.x `DurableOrchestrationContext` `IDurableOrchestrationContext`için, 'yi yerine kullanmalısınız. Sürümler arasındaki farklar hakkında daha fazla bilgi için [Dayanıklı Işlevler sürümleri](durable-functions-versions.md) makalesine bakın.

# <a name="javascript"></a>[Javascript](#tab/javascript)

```javascript
const df = require("durable-functions");
const moment = require("moment");

module.exports = df.orchestrator(function*(context) {
    const deadline = moment.utc(context.df.currentUtcDateTime).add(30, "s");

    const activityTask = context.df.callActivity("FlakyFunction");
    const timeoutTask = context.df.createTimer(deadline.toDate());

    const winner = yield context.df.Task.any([activityTask, timeoutTask]);
    if (winner === activityTask) {
        // success case
        timeoutTask.cancel();
        return true;
    } else {
        // timeout case
        return false;
    }
});
```

---

> [!NOTE]
> Bu mekanizma aslında devam eden etkinlik işlevi yürütme sona erdirmez. Bunun yerine, sadece orkestratör işlevi sonucu göz ardı etmek ve hareket sağlar. Daha fazla bilgi için [Timers](durable-functions-timers.md#usage-for-timeout) belgelerine bakın.

## <a name="unhandled-exceptions"></a>İşlenmeyen özel durumlar

Bir orchestrator işlevi işlenmemiş bir özel durumla başarısız olursa, özel durumun ayrıntıları `Failed` günlüğe kaydedilir ve örnek bir durumla tamamlanır.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Ebedi orkestrasyonlar hakkında bilgi edinin](durable-functions-eternal-orchestrations.md)

> [!div class="nextstepaction"]
> [Sorunları nasıl tanılayın öğrenin](durable-functions-diagnostics.md)
