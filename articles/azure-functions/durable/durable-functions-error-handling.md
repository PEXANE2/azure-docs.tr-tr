---
title: Dayanıklı İşlevler hataları işleme-Azure
description: Azure Işlevleri için Dayanıklı İşlevler uzantısı 'ndaki hataların nasıl işleneceğini öğrenin.
ms.topic: conceptual
ms.date: 11/02/2019
ms.author: azfuncdf
ms.openlocfilehash: 447b3dcf5040835f5a853beff68bde794ece51f5
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79277862"
---
# <a name="handling-errors-in-durable-functions-azure-functions"></a>Dayanıklı İşlevler hataları işleme (Azure Işlevleri)

Dayanıklı Işlev düzenlemeleri kodda uygulanır ve programlama dilinin yerleşik hata işleme özelliklerini kullanabilir. Organize etmeniz için hata işleme ve dengeleme eklemek için öğrenmeniz gereken yeni kavramlar aslında yoktur. Ancak, bilmeniz gereken birkaç davranış vardır.

## <a name="errors-in-activity-functions"></a>Etkinlik işlevlerinde hatalar

Bir etkinlik işlevinde oluşturulan herhangi bir özel durum Orchestrator işlevine geri hazırlanır ve bir `FunctionFailedException`olarak oluşturulur. Orchestrator işlevindeki gereksinimlerinize uyan hata işleme ve dengeleme kodu yazabilirsiniz.

Örneğin, bir hesaptan diğerine fonları aktaran aşağıdaki Orchestrator işlevini göz önünde bulundurun:

# <a name="c"></a>[, #](#tab/csharp)

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
> Önceki C# örnekleri Dayanıklı İşlevler 2. x içindir. Dayanıklı İşlevler 1. x için yerine kullanmanız `DurableOrchestrationContext` gerekir. `IDurableOrchestrationContext` Sürümler arasındaki farklılıklar hakkında daha fazla bilgi için [dayanıklı işlevler sürümler](durable-functions-versions.md) makalesine bakın.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

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

İlk **alacaklı Taccount** işlev çağrısı başarısız olursa, Orchestrator işlevi fontları kaynak hesaba geri alacak şekilde dengeler.

## <a name="automatic-retry-on-failure"></a>Hata durumunda otomatik yeniden deneme

Etkinlik işlevlerini veya alt düzenleme işlevlerini çağırdığınızda otomatik yeniden deneme ilkesi belirtebilirsiniz. Aşağıdaki örnek, bir işlevi üç kez çağırmaya çalışır ve her yeniden deneme arasında 5 saniye bekler:

# <a name="c"></a>[, #](#tab/csharp)

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
> Önceki C# örnekleri Dayanıklı İşlevler 2. x içindir. Dayanıklı İşlevler 1. x için yerine kullanmanız `DurableOrchestrationContext` gerekir. `IDurableOrchestrationContext` Sürümler arasındaki farklılıklar hakkında daha fazla bilgi için [dayanıklı işlevler sürümler](durable-functions-versions.md) makalesine bakın.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

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

Önceki örnekteki etkinlik işlevi çağrısı otomatik yeniden deneme ilkesini yapılandırmak için bir parametre alır. Otomatik yeniden deneme ilkesini özelleştirmek için çeşitli seçenekler vardır:

* **Deneme sayısı**üst sınırı: en fazla yeniden deneme denemesi sayısı.
* **İlk yeniden deneme aralığı**: ilk yeniden deneme girişiminden önce beklenecek süre.
* **Geri dönüşlü katsayı**: geri alma artışının oranını belirlemede kullanılan katsayı. Varsayılan değer 1 ' dir.
* **En fazla yeniden deneme aralığı**: yeniden deneme girişimleri arasında beklenecek en uzun süre.
* **Yeniden deneme zaman aşımı**: yeniden denemeler yapmak için harcayabileceğiniz en uzun süre. Varsayılan davranış süresiz olarak yeniden denenecektir.
* **Tanıtıcı**: bir işlevin yeniden denenip denenmeyeceğini anlamak için Kullanıcı tanımlı geri çağırma belirtilebilir.

## <a name="function-timeouts"></a>İşlev zaman aşımları

Bir Orchestrator işlevi içindeki bir işlev çağrısını, tamamlanamayacak kadar uzun sürerse iptal etmek isteyebilirsiniz. Bu işlemi bugün yapmanın doğru yolu, (.net `context.df.createTimer` ) veya (JavaScript) `context.CreateTimer` ile `Task.WhenAny` `context.df.Task.any` birlikte (.net) veya (JavaScript) kullanarak, aşağıdaki örnekte olduğu gibi dayanıklı bir [Zamanlayıcı](durable-functions-timers.md) oluşturmaktır:

# <a name="c"></a>[, #](#tab/csharp)

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
> Önceki C# örnekleri Dayanıklı İşlevler 2. x içindir. Dayanıklı İşlevler 1. x için yerine kullanmanız `DurableOrchestrationContext` gerekir. `IDurableOrchestrationContext` Sürümler arasındaki farklılıklar hakkında daha fazla bilgi için [dayanıklı işlevler sürümler](durable-functions-versions.md) makalesine bakın.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

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
> Bu mekanizma, devam eden etkinlik işlevi yürütmeyi gerçekten sonlandırır. Bunun yerine, Orchestrator işlevinin sonucu yok saymasına ve üzerinde hareket etmesine izin verir. Daha fazla bilgi için bkz. [zamanlayıcılar](durable-functions-timers.md#usage-for-timeout) belgeleri.

## <a name="unhandled-exceptions"></a>İşlenmeyen özel durumlar

Orchestrator işlevi işlenmeyen bir özel durumla başarısız olursa, özel durumun ayrıntıları günlüğe kaydedilir ve örnek bir `Failed` durum ile tamamlanır.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Dış düzenlemeleri hakkında bilgi edinin](durable-functions-eternal-orchestrations.md)

> [!div class="nextstepaction"]
> [Sorunları tanılamayı öğrenin](durable-functions-diagnostics.md)
