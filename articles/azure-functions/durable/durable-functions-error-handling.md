---
title: Dayanıklı İşlevler hataları işleme-Azure
description: Azure Işlevleri için Dayanıklı İşlevler uzantısı 'ndaki hataların nasıl işleneceğini öğrenin.
services: functions
author: ggailey777
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: azfuncdf
ms.openlocfilehash: 7b357189a9ce67f27952985b78dd3134517ffba5
ms.sourcegitcommit: 97605f3e7ff9b6f74e81f327edd19aefe79135d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70734299"
---
# <a name="handling-errors-in-durable-functions-azure-functions"></a>Dayanıklı İşlevler hataları işleme (Azure Işlevleri)

Dayanıklı Işlev düzenlemeleri kodda uygulanır ve programlama dilinin hata işleme yeteneklerini kullanabilir. Bu göz önünde bulundurularak, hata işleme ve tazminat ile tazminat ekleme hakkında bilgi edinmek için ihtiyacınız olan yeni kavramlardan emin olmanız gerekir. Ancak, bilmeniz gereken birkaç davranış vardır.

## <a name="errors-in-activity-functions"></a>Etkinlik işlevlerinde hatalar

Bir etkinlik işlevinde oluşturulan herhangi bir özel durum, Orchestrator işlevine geri dönerek bir `FunctionFailedException`olarak oluşturulur. Orchestrator işlevindeki gereksinimlerinize uyan hata işleme ve dengeleme kodu yazabilirsiniz.

Örneğin, bir hesaptan diğerine fonları aktaran aşağıdaki Orchestrator işlevini göz önünde bulundurun:

### <a name="precompiled-c"></a>DerlemesiC#

```csharp
[FunctionName("TransferFunds")]
public static async Task Run([OrchestrationTrigger] DurableOrchestrationContext context)
{
    var transferDetails = ctx.GetInput<TransferOperation>();

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

### <a name="c-script"></a>C#SCRIPT

```csharp
#r "Microsoft.Azure.WebJobs.Extensions.DurableTask"

public static async Task Run(DurableOrchestrationContext context)
{
    var transferDetails = ctx.GetInput<TransferOperation>();

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

### <a name="javascript-functions-2x-only"></a>JavaScript (yalnızca 2. x Işlevleri)

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

Hedef hesap için **alacaklı Taccount** işlevine yapılan çağrı başarısız olursa, Orchestrator işlevi bu fontları kaynak hesaba geri alacak şekilde dengeleyerek bunu dengeler.

## <a name="automatic-retry-on-failure"></a>Hata durumunda otomatik yeniden deneme

Etkinlik işlevlerini veya alt düzenleme işlevlerini çağırdığınızda otomatik yeniden deneme ilkesi belirtebilirsiniz. Aşağıdaki örnek, bir işlevi üç kez çağırmaya çalışır ve her yeniden deneme arasında 5 saniye bekler:

### <a name="precompiled-c"></a>DerlemesiC#

```csharp
[FunctionName("TimerOrchestratorWithRetry")]
public static async Task Run([OrchestrationTrigger] DurableOrchestrationContext context)
{
    var retryOptions = new RetryOptions(
        firstRetryInterval: TimeSpan.FromSeconds(5),
        maxNumberOfAttempts: 3);

    await ctx.CallActivityWithRetryAsync("FlakyFunction", retryOptions, null);

    // ...
}
```

### <a name="c-script"></a>C#SCRIPT

```csharp
public static async Task Run(DurableOrchestrationContext context)
{
    var retryOptions = new RetryOptions(
        firstRetryInterval: TimeSpan.FromSeconds(5),
        maxNumberOfAttempts: 3);

    await ctx.CallActivityWithRetryAsync("FlakyFunction", retryOptions, null);

    // ...
}
```

### <a name="javascript-functions-2x-only"></a>JavaScript (yalnızca 2. x Işlevleri)

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context) {
    const retryOptions = new df.RetryOptions(5000, 3);

    yield context.df.callActivityWithRetry("FlakyFunction", retryOptions);

    // ...
});
```

(.Net) veya `callActivityWithRetry` (JavaScript) API 'si bir `RetryOptions` parametre alır. `CallActivityWithRetryAsync` `CallSubOrchestratorWithRetryAsync` (.Net) veya `callSubOrchestratorWithRetry` (JavaScript) API kullanan alt düzenleme çağrıları aynı yeniden deneme ilkelerini kullanabilir.

Otomatik yeniden deneme ilkesini özelleştirmek için çeşitli seçenekler vardır. Bu ülkelere şunlar dahildir:

* **Deneme sayısı üst sınırı**: En fazla yeniden deneme denemesi sayısı.
* **İlk yeniden deneme aralığı**: İlk yeniden deneme girişiminden önce beklenecek süre.
* **Geri dönüşlü katsayı**: Geri alma artışının oranını belirlemede kullanılan katsayı. Varsayılan değer 1 ' dir.
* **En fazla yeniden deneme aralığı**: Yeniden deneme girişimleri arasında beklenecek en uzun süre.
* **Yeniden deneme zaman aşımı**: Yeniden denemeler yapmak için harcayabileceğiniz en uzun süre. Varsayılan davranış süresiz olarak yeniden denenecektir.
* **Tanıtıcı**: Bir işlev çağrısının yeniden denenip denenmeyeceğini belirleyen, Kullanıcı tanımlı bir geri çağırma belirtilebilir.

## <a name="function-timeouts"></a>İşlev zaman aşımları

Bir Orchestrator işlevi içindeki bir işlev çağrısını, tamamlanamayacak kadar uzun sürerse iptal etmek isteyebilirsiniz. Bu işlemi bugün yapmanın doğru yolu, (.net `context.df.createTimer` ) veya [](durable-functions-timers.md) (JavaScript) `context.CreateTimer` ile `Task.WhenAny` `context.df.Task.any` birlikte (.net) veya (JavaScript) kullanarak, aşağıdaki örnekte olduğu gibi dayanıklı bir Zamanlayıcı oluşturmaktır:

### <a name="precompiled-c"></a>DerlemesiC#

```csharp
[FunctionName("TimerOrchestrator")]
public static async Task<bool> Run([OrchestrationTrigger] DurableOrchestrationContext context)
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

### <a name="c-script"></a>C#SCRIPT

```csharp
public static async Task<bool> Run(DurableOrchestrationContext context)
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

### <a name="javascript-functions-2x-only"></a>JavaScript (yalnızca 2. x Işlevleri)

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

> [!NOTE]
> Bu mekanizma, devam eden etkinlik işlevi yürütmeyi gerçekten sonlandırır. Bunun yerine, Orchestrator işlevinin sonucu yok saymasına ve üzerinde hareket etmesine izin verir. Daha fazla bilgi için bkz. [zamanlayıcılar](durable-functions-timers.md#usage-for-timeout) belgeleri.

## <a name="unhandled-exceptions"></a>İşlenmeyen özel durumlar

Orchestrator işlevi işlenmeyen bir özel durumla başarısız olursa, özel durumun ayrıntıları günlüğe kaydedilir ve örnek bir `Failed` durum ile tamamlanır.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Sorunları tanılamayı öğrenin](durable-functions-diagnostics.md)