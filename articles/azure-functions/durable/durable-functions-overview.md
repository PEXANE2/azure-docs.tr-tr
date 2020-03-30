---
title: Dayanıklı Fonksiyonlar Genel Bakış - Azure
description: Azure İşlevler için Dayanıklı İşlevler uzantısına giriş.
author: cgillum
ms.topic: overview
ms.date: 08/07/2019
ms.author: cgillum
ms.reviewer: azfuncdf
ms.openlocfilehash: 5d454aefaba89bef9dc9009ff442fa5543dae2ef
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "79241349"
---
# <a name="what-are-durable-functions"></a>Dayanıklı İşlevler nedir?

*Dayanıklı Işlevler,* sunucusuz bir bilgi işlem ortamında durumlu işlevleri yazmanıza olanak tanıyan [Azure İşlevlerinin](../functions-overview.md) bir uzantısıdır. Uzantı, Azure İşlevleri programlama modelini kullanarak [*varlık işlevleri*](durable-functions-entities.md) yazarak [*düzenbaz işlevlerini*](durable-functions-orchestrations.md) ve düzel kişileri yazarak durumlu iş akışlarını tanımlamanızı sağlar. Arka planda, uzantı durumu, denetim noktalarını yönetir ve sizin için yeniden başlatArak iş mantığınıza odaklanmanızı sağlar.

## <a name="supported-languages"></a><a name="language-support"></a>Desteklenen diller

Dayanıklı İşlevler şu anda aşağıdaki dilleri destekler:

* **C#**: hem [önceden derlenmiş sınıf kitaplıkları](../functions-dotnet-class-library.md) hem de [C# komut dosyası.](../functions-reference-csharp.md)
* **JavaScript**: yalnızca Azure Fonksiyonları çalışma zamanının 2.x sürümü için desteklenir. Dayanıklı Fonksiyonlar uzantısı veya daha sonraki bir sürümü sürüm 1.7.0 gerektirir. 
* **F#**: önceden derlenmiş sınıf kitaplıkları ve F# komut dosyası. F# komut dosyası yalnızca Azure İşlevleri çalışma zamanının 1.x sürümü için desteklenir.

Dayanıklı İşlevler'in amacı tüm [Azure İşlevlerini dilleri](../supported-languages.md)desteklemedir. Ek dilleri desteklemek için işin en son durumu için [Dayanıklı İşlevler sorunları listesine](https://github.com/Azure/azure-functions-durable-extension/issues) bakın.

Azure İşlevleri gibi, [Visual Studio 2019](durable-functions-create-first-csharp.md), Visual [Studio Code](quickstart-js-vscode.md)ve [Azure portalını](durable-functions-create-portal.md)kullanarak Dayanıklı İşlevler geliştirmenize yardımcı olacak şablonlar vardır.

## <a name="application-patterns"></a>Uygulama desenleri

Dayanıklı Işlevler için birincil kullanım örneği, sunucusuz uygulamalarda karmaşık ve durumlu koordinasyon gereksinimlerini basitleştirmektir. Aşağıdaki bölümlerde Dayanıklı İşlevler yararlanabilir tipik uygulama desenleri açıklanabilir:

* [İşlev zinciri oluşturma](#chaining)
* [Yelpaze dışarı/yelpaze içeri](#fan-in-out)
* [Zaman uyumsuz HTTP API’leri](#async-http)
* [İzleme](#monitoring)
* [İnsan etkileşimi](#human)
* [Toplayıcı (durum lu varlıklar)](#aggregator)

### <a name="pattern-1-function-chaining"></a><a name="chaining"></a>Desen #1: Fonksiyon zincirleme

İşlev zincirleme deseninde, bir dizi işlev belirli bir sırada yürütülür. Bu desende, bir işlevin çıktısı başka bir işlevin girişine uygulanır.

![Fonksiyon zincirleme deseninin diyagramı](./media/durable-functions-concepts/function-chaining.png)

Aşağıdaki örnekte gösterildiği gibi işlev zincirleme deseni kısa bir şekilde uygulamak için Dayanıklı İşlevler kullanabilirsiniz.

Bu örnekte, `F1`değerler `F2` `F3`, `F4` , , ve aynı işlev uygulamasındaki diğer işlevlerin adlarıdır. Normal zorunlu kodlama yapılarını kullanarak kontrol akışını uygulayabilirsiniz. Kod yukarıdan aşağıya doğru yürütülür. Kod, koşullu lar ve döngüler gibi varolan dil denetimi akışı anlambilimini içerebilir. Bloklar `try` / `catch` / `finally` halinde hata işleme mantığı ekleyebilirsiniz.

# <a name="c"></a>[C #](#tab/csharp)

```csharp
[FunctionName("Chaining")]
public static async Task<object> Run(
    [OrchestrationTrigger] IDurableOrchestrationContext context)
{
    try
    {
        var x = await context.CallActivityAsync<object>("F1", null);
        var y = await context.CallActivityAsync<object>("F2", x);
        var z = await context.CallActivityAsync<object>("F3", y);
        return  await context.CallActivityAsync<object>("F4", z);
    }
    catch (Exception)
    {
        // Error handling or compensation goes here.
    }
}
```

`context` Diğer işlevleri ada göre çağırmak, parametreleri geçmek ve işlev çıktısını döndürmek için parametreyi kullanabilirsiniz. Kod her aradığında, `await`Dayanıklı Işlevler çerçevesi geçerli işlev örneğinin ilerlemesini kontrol eder. İşlem veya sanal makine yürütmenin ortasında geri dönüşüm yaparsa, işlev örneği `await` önceki çağrıdan devam eder. Daha fazla bilgi için, sonraki bölüme bakın, Desen #2: Fan out /fan.

# <a name="javascript"></a>[Javascript](#tab/javascript)

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context) {
    try {
        const x = yield context.df.callActivity("F1");
        const y = yield context.df.callActivity("F2", x);
        const z = yield context.df.callActivity("F3", y);
        return    yield context.df.callActivity("F4", z);
    } catch (error) {
        // Error handling or compensation goes here.
    }
});
```

Nesneyi `context.df` diğer işlevleri ada göre çağırmak, parametreleri geçmek ve işlev çıktısını döndürmek için kullanabilirsiniz. Kod her aradığında, `yield`Dayanıklı Işlevler çerçevesi geçerli işlev örneğinin ilerlemesini kontrol eder. İşlem veya sanal makine yürütmenin ortasında geri dönüşüm yaparsa, işlev örneği `yield` önceki çağrıdan devam eder. Daha fazla bilgi için, sonraki bölüme bakın, Desen #2: Fan out /fan.

> [!NOTE]
> JavaScript'teki `context` nesne tüm [işlev bağlamını](../functions-reference-node.md#context-object)temsil eder. Ana bağlamüzerindeki özelliği `df` kullanarak Dayanıklı İşlevler bağlamına erişin.

---

### <a name="pattern-2-fan-outfan-in"></a><a name="fan-in-out"></a>Desen #2: Fan out/fan

Desendeki fan/fan da, birden çok işlevi paralel olarak çalıştırır ve tüm işlevlerin tamamlanmasını beklersiniz. Genellikle, bazı toplama çalışmaları işlevleri döndürülen sonuçlar üzerinde yapılır.

![Fan çıkış/fan deseninin diyagramı](./media/durable-functions-concepts/fan-out-fan-in.png)

Normal işlevlerle, işlevin kuyruğa birden çok ileti göndermesini sağlayarak dışarı çıkabilirsiniz. Geri dönmek çok daha zor. Normal bir işlevde, normal bir işlevde, sıra nın tetiklediği işlevlerin ne zaman sona erini izlemek için kod yazarsınız ve ardından işlev çıktılarını depolarsınız.

Dayanıklı İşlevler uzantısı bu deseni nispeten basit bir kodla işler:

# <a name="c"></a>[C #](#tab/csharp)

```csharp
[FunctionName("FanOutFanIn")]
public static async Task Run(
    [OrchestrationTrigger] IDurableOrchestrationContext context)
{
    var parallelTasks = new List<Task<int>>();

    // Get a list of N work items to process in parallel.
    object[] workBatch = await context.CallActivityAsync<object[]>("F1", null);
    for (int i = 0; i < workBatch.Length; i++)
    {
        Task<int> task = context.CallActivityAsync<int>("F2", workBatch[i]);
        parallelTasks.Add(task);
    }

    await Task.WhenAll(parallelTasks);

    // Aggregate all N outputs and send the result to F3.
    int sum = parallelTasks.Sum(t => t.Result);
    await context.CallActivityAsync("F3", sum);
}
```

Fan-out çalışması `F2` işlevin birden çok örneğine dağıtılır. Çalışma, dinamik bir görev listesi kullanılarak izlenir. `Task.WhenAll`tüm çağrılan işlevlerin tamamlanmasını beklemek için çağrılır. Daha sonra, `F2` işlev çıktıları dinamik görev listesinden toplanır `F3` ve işleve aktarılır.

`await` Arama `Task.WhenAll` sırasında gerçekleşen otomatik denetim noktası, olası bir yarıyol çökmesi veya yeniden başlatma nın zaten tamamlanmış bir görevi yeniden başlatmayı gerektirmemesini sağlar.

# <a name="javascript"></a>[Javascript](#tab/javascript)

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context) {
    const parallelTasks = [];

    // Get a list of N work items to process in parallel.
    const workBatch = yield context.df.callActivity("F1");
    for (let i = 0; i < workBatch.length; i++) {
        parallelTasks.push(context.df.callActivity("F2", workBatch[i]));
    }

    yield context.df.Task.all(parallelTasks);

    // Aggregate all N outputs and send the result to F3.
    const sum = parallelTasks.reduce((prev, curr) => prev + curr, 0);
    yield context.df.callActivity("F3", sum);
});
```

Fan-out çalışması `F2` işlevin birden çok örneğine dağıtılır. Çalışma, dinamik bir görev listesi kullanılarak izlenir. `context.df.Task.all`API tüm çağrılan işlevlerin tamamlanmasını beklemek için çağrılır. Daha sonra, `F2` işlev çıktıları dinamik görev listesinden toplanır `F3` ve işleve aktarılır.

`yield` Arama `context.df.Task.all` sırasında gerçekleşen otomatik denetim noktası, olası bir yarıyol çökmesi veya yeniden başlatma nın zaten tamamlanmış bir görevi yeniden başlatmayı gerektirmemesini sağlar.

---

> [!NOTE]
> Nadir durumlarda, bir etkinlik işlevi tamamlandıktan sonra pencerede bir kilitlenme meydana gelebilir, ancak tamamlanmadan önce orkestrasyon geçmişine kaydedilir. Bu durumda, işlem kurtarıldıktan sonra etkinlik işlevi baştan yeniden çalıştırılır.

### <a name="pattern-3-async-http-apis"></a><a name="async-http"></a>Desen #3: Async HTTP API'ler

Async HTTP API deseni, uzun süren işlemlerin durumunu dış istemcilerle koordine etme sorununu gidermektedir. Bu deseni uygulamanın yaygın bir yolu, bir HTTP bitiş noktasının uzun süren eylemi tetiklemekten geçer. Ardından, istemciyi işlem ne zaman tamamladığını öğrenmek için istemcinin yok ettiği durum bitiş noktasına yönlendirin.

![HTTP API deseni diyagramı](./media/durable-functions-concepts/async-http-api.png)

Dayanıklı Işlevler, uzun süren işlev yürütmeleri ile etkileşimkurmak için yazmanız gereken kodu basitleştirerek ve hatta kaldırarak bu desen için **yerleşik destek** sağlar. Örneğin, Dayanıklı Fonksiyonlar quickstart örnekleri[(C#](durable-functions-create-first-csharp.md) ve [JavaScript)](quickstart-js-vscode.md)yeni orchestrator işlev örnekleri başlatmak için kullanabileceğiniz basit bir REST komutu gösterir. Bir örnek başladıktan sonra, uzantı, orkestratör işlev durumunu sorgulayan webhook HTTP API'lerini ortaya çıkarır. 

Aşağıdaki örnekte, bir orkestratör başlatan ve durumunu sorgulayan REST komutları gösterilmektedir. Netlik için, bazı protokol ayrıntıları örnekten atlanır.

```
> curl -X POST https://myfunc.azurewebsites.net/orchestrators/DoWork -H "Content-Length: 0" -i
HTTP/1.1 202 Accepted
Content-Type: application/json
Location: https://myfunc.azurewebsites.net/runtime/webhooks/durabletask/b79baf67f717453ca9e86c5da21e03ec

{"id":"b79baf67f717453ca9e86c5da21e03ec", ...}

> curl https://myfunc.azurewebsites.net/runtime/webhooks/durabletask/b79baf67f717453ca9e86c5da21e03ec -i
HTTP/1.1 202 Accepted
Content-Type: application/json
Location: https://myfunc.azurewebsites.net/runtime/webhooks/durabletask/b79baf67f717453ca9e86c5da21e03ec

{"runtimeStatus":"Running","lastUpdatedTime":"2019-03-16T21:20:47Z", ...}

> curl https://myfunc.azurewebsites.net/runtime/webhooks/durabletask/b79baf67f717453ca9e86c5da21e03ec -i
HTTP/1.1 200 OK
Content-Length: 175
Content-Type: application/json

{"runtimeStatus":"Completed","lastUpdatedTime":"2019-03-16T21:20:57Z", ...}
```

Dayanıklı Işlevler çalışma süresi sizin için durumu yönettiğinden, kendi durum izleme mekanizmasınızı uygulamanız gerekmez.

Dayanıklı İşlevler uzantısı, uzun süreli orkestrasyonları yöneten yerleşik HTTP API'lerini ortaya çıkarır. Alternatif olarak kendi işlev tetikleyicilerinizi (HTTP, kuyruk veya Azure Olay Hub'ları gibi) ve [düzenleme istemcisi bağlamayı](durable-functions-bindings.md#orchestration-client)kullanarak bu deseni kendiniz uygulayabilirsiniz. Örneğin, sonlandırmayı tetiklemek için bir sıra iletisi kullanabilirsiniz. Veya, kimlik doğrulaması için oluşturulmuş bir anahtar kullanan yerleşik HTTP API'leri yerine Azure Etkin Dizin kimlik doğrulama ilkesi yle korunan bir HTTP tetikleyicisi kullanabilirsiniz.

Daha fazla bilgi için, Dayanıklı İşlevler uzantısını kullanarak HTTP üzerinden eşzamanlı ve uzun süren süreçleri nasıl ortaya çıkarabileceğinizi açıklayan [HTTP özellikleri](durable-functions-http-features.md) makalesine bakın.

### <a name="pattern-4-monitor"></a><a name="monitoring"></a>Desen #4: Monitör

Monitör deseni, iş akışındaki esnek ve yinelenen bir işlemi ifade eder. Bir örnek, belirli koşullar yerine getirile kadar yoklamadır. Periyodik temizleme işi gibi temel bir senaryoyu gidermek için düzenli bir [zamanlayıcı tetikleyicisi](../functions-bindings-timer.md) kullanabilirsiniz, ancak aralığı statiktir ve örnek yaşam sürelerini yönetmek karmaşık hale gelir. Esnek yineleme aralıkları oluşturmak, görev yaşam sürelerini yönetmek ve tek bir düzenlemeden birden çok monitör işlemi oluşturmak için Dayanıklı İşlevler'i kullanabilirsiniz.

Monitör desenine bir örnek, önceki async HTTP API senaryosunu tersine çevirmektir. Uzun süren bir işlemi izlemek için harici bir istemciiçin bir bitiş noktası ortaya çıkarmak yerine, uzun soluklu monitör harici bir uç noktası tüketir ve ardından durum değişikliğini bekler.

![Monitör deseninin diyagramı](./media/durable-functions-concepts/monitor.png)

Birkaç kod satırında, rasgele uç noktaları izleyen birden çok monitör oluşturmak için Dayanıklı İşlevler kullanabilirsiniz. Bir koşul karşılandığında monitörler yürütmeyi sonlandırabilir veya başka bir işlev monitörleri sonlandırmak için dayanıklı düzenleme istemcisini kullanabilir. Belirli bir duruma göre `wait` monitörün aralığını değiştirebilirsiniz (örneğin, üstel geri tepme.) 

Aşağıdaki kod temel bir monitör uygular:

# <a name="c"></a>[C #](#tab/csharp)

```csharp
[FunctionName("MonitorJobStatus")]
public static async Task Run(
    [OrchestrationTrigger] IDurableOrchestrationContext context)
{
    int jobId = context.GetInput<int>();
    int pollingInterval = GetPollingInterval();
    DateTime expiryTime = GetExpiryTime();

    while (context.CurrentUtcDateTime < expiryTime)
    {
        var jobStatus = await context.CallActivityAsync<string>("GetJobStatus", jobId);
        if (jobStatus == "Completed")
        {
            // Perform an action when a condition is met.
            await context.CallActivityAsync("SendAlert", machineId);
            break;
        }

        // Orchestration sleeps until this time.
        var nextCheck = context.CurrentUtcDateTime.AddSeconds(pollingInterval);
        await context.CreateTimer(nextCheck, CancellationToken.None);
    }

    // Perform more work here, or let the orchestration end.
}
```

# <a name="javascript"></a>[Javascript](#tab/javascript)

```javascript
const df = require("durable-functions");
const moment = require("moment");

module.exports = df.orchestrator(function*(context) {
    const jobId = context.df.getInput();
    const pollingInterval = getPollingInterval();
    const expiryTime = getExpiryTime();

    while (moment.utc(context.df.currentUtcDateTime).isBefore(expiryTime)) {
        const jobStatus = yield context.df.callActivity("GetJobStatus", jobId);
        if (jobStatus === "Completed") {
            // Perform an action when a condition is met.
            yield context.df.callActivity("SendAlert", machineId);
            break;
        }

        // Orchestration sleeps until this time.
        const nextCheck = moment.utc(context.df.currentUtcDateTime).add(pollingInterval, 's');
        yield context.df.createTimer(nextCheck.toDate());
    }

    // Perform more work here, or let the orchestration end.
});
```

---

Bir istek alındığı zaman, bu iş kimliği için yeni bir düzenleme örneği oluşturulur. Örnek, bir koşul karşılanana ve döngü çıkana kadar bir durumu yoklar. Dayanıklı bir zamanlayıcı yoklama aralığını denetler. Daha sonra, daha fazla iş yapılabilir veya orkestrasyon sona erebilir. `nextCheck` Aşıldığında, `expiryTime`monitör sona erer.

### <a name="pattern-5-human-interaction"></a><a name="human"></a>Desen #5: İnsan etkileşimi

Birçok otomatik süreçler insan etkileşimi çeşit içerir. İnsanları otomatik bir işleme dahil etmek zordur, çünkü insanlar bulut hizmetleri kadar yüksek kullanılabilir ve duyarlı değildir. Otomatik leştirilmiş bir işlem, zaman ekmeleri ve telafi mantığı kullanarak bu etkileşime izin verebilir.

Onay süreci, insan etkileşimini içeren bir iş sürecine bir örnektir. Belirli bir dolar tutarını aşan bir gider raporu için yöneticiden onay gerekebilir. Yönetici gider raporunu 72 saat içinde onaylamazsa (belki de yönetici tatile çıktı), bir yükseltme işlemi başka birinden (belki de yöneticinin yöneticisinin) onayını almak için devreye girmektedir.

![İnsan etkileşim deseninin diyagramı](./media/durable-functions-concepts/approval.png)

Bu örnekte bir orkestratör işlevi kullanarak deseni uygulayabilirsiniz. Orkestratör onay istemek için dayanıklı bir [zamanlayıcı](durable-functions-timers.md) kullanır. Zaman acısı oluşursa orkestratör yükselir. Orkestratör, insan etkileşimi tarafından oluşturulan bir bildirim gibi harici bir [olayı](durable-functions-external-events.md)bekler.

Bu örnekler, insan etkileşimi modelini göstermek için bir onay süreci oluşturur:

# <a name="c"></a>[C #](#tab/csharp)

```csharp
[FunctionName("ApprovalWorkflow")]
public static async Task Run(
    [OrchestrationTrigger] IDurableOrchestrationContext context)
{
    await context.CallActivityAsync("RequestApproval", null);
    using (var timeoutCts = new CancellationTokenSource())
    {
        DateTime dueTime = context.CurrentUtcDateTime.AddHours(72);
        Task durableTimeout = context.CreateTimer(dueTime, timeoutCts.Token);

        Task<bool> approvalEvent = context.WaitForExternalEvent<bool>("ApprovalEvent");
        if (approvalEvent == await Task.WhenAny(approvalEvent, durableTimeout))
        {
            timeoutCts.Cancel();
            await context.CallActivityAsync("ProcessApproval", approvalEvent.Result);
        }
        else
        {
            await context.CallActivityAsync("Escalate", null);
        }
    }
}
```

Dayanıklı zamanlayıcıyı oluşturmak `context.CreateTimer`için. Bildirim tarafından `context.WaitForExternalEvent`alınır. Daha `Task.WhenAny` sonra, yükseltme (zaman acısı önce olur) veya onay işleme (onay zaman ödemeden önce alınır) karar vermek için çağrılır.

# <a name="javascript"></a>[Javascript](#tab/javascript)

```javascript
const df = require("durable-functions");
const moment = require('moment');

module.exports = df.orchestrator(function*(context) {
    yield context.df.callActivity("RequestApproval");

    const dueTime = moment.utc(context.df.currentUtcDateTime).add(72, 'h');
    const durableTimeout = context.df.createTimer(dueTime.toDate());

    const approvalEvent = context.df.waitForExternalEvent("ApprovalEvent");
    if (approvalEvent === yield context.df.Task.any([approvalEvent, durableTimeout])) {
        durableTimeout.cancel();
        yield context.df.callActivity("ProcessApproval", approvalEvent.result);
    } else {
        yield context.df.callActivity("Escalate");
    }
});
```

Dayanıklı zamanlayıcıyı oluşturmak `context.df.createTimer`için. Bildirim tarafından `context.df.waitForExternalEvent`alınır. Daha `context.df.Task.any` sonra, yükseltme (zaman acısı önce olur) veya onay işleme (onay zaman ödemeden önce alınır) karar vermek için çağrılır.

---

Harici bir istemci, [yerleşik HTTP API'lerini](durable-functions-http-api.md#raise-event)kullanarak olay bildirimini bekleyen bir orkestratör işlevine sunabilir:

```bash
curl -d "true" http://localhost:7071/runtime/webhooks/durabletask/instances/{instanceId}/raiseEvent/ApprovalEvent -H "Content-Type: application/json"
```

Bir olay aynı işlev uygulamasında başka bir işlevden dayanıklı orkestrasyon istemcisi kullanılarak da yükseltilebilir:

# <a name="c"></a>[C #](#tab/csharp)

```csharp
[FunctionName("RaiseEventToOrchestration")]
public static async Task Run(
    [HttpTrigger] string instanceId,
    [DurableClient] IDurableOrchestrationClient client)
{
    bool isApproved = true;
    await client.RaiseEventAsync(instanceId, "ApprovalEvent", isApproved);
}
```

# <a name="javascript"></a>[Javascript](#tab/javascript)

```javascript
const df = require("durable-functions");

module.exports = async function (context) {
    const client = df.getClient(context);
    const isApproved = true;
    await client.raiseEvent(instanceId, "ApprovalEvent", isApproved);
};
```

---

### <a name="pattern-6-aggregator-stateful-entities"></a><a name="aggregator"></a>Desen #6: Toplayıcı (durumlu varlıklar)

Altıncı desen, olay verilerini belirli bir süre içinde tek bir, adreslenebilir *varlığa*toplamayla ilgilidir. Bu desende, toplanan veriler birden çok kaynaktan gelebilir, toplu olarak teslim edilebilir veya uzun süreler boyunca dağıtılabilir. Toplayıcının olay verileri üzerinde harekete geçebileceği ve dış istemcilerin toplanan verileri sorgulaması gerekebilir.

![Toplayıcı diyagramı](./media/durable-functions-concepts/aggregator.png)

Bu deseni normal, devletsiz işlevlerle uygulamaya çalışmanın zor yanı eşzamanlılık kontrolünün büyük bir sorun haline gelmesidir. Aynı anda aynı verileri değiştiren birden çok iş parçacığı hakkında endişelenmeniz gerekmez, aynı zamanda toplayıcının aynı anda yalnızca tek bir VM'de çalışmasını sağlama konusunda da endişelenmeniz gerekir.

Bu deseni tek bir işlev olarak kolayca uygulamak için [Dayanıklı varlıklar](durable-functions-entities.md) kullanabilirsiniz.

# <a name="c"></a>[C #](#tab/csharp)

```csharp
[FunctionName("Counter")]
public static void Counter([EntityTrigger] IDurableEntityContext ctx)
{
    int currentValue = ctx.GetState<int>();
    switch (ctx.OperationName.ToLowerInvariant())
    {
        case "add":
            int amount = ctx.GetInput<int>();
            ctx.SetState(currentValue + amount);
            break;
        case "reset":
            ctx.SetState(0);
            break;
        case "get":
            ctx.Return(currentValue);
            break;
    }
}
```

Dayanıklı varlıklar .NET'te sınıf olarak da modellenebilir. Bu model, işlem listesi sabitlenir ve büyük olursa yararlı olabilir. Aşağıdaki örnek, .NET sınıfları ve yöntemlerini kullanarak varlığın `Counter` eşdeğer bir uygulamasıdır.

```csharp
public class Counter
{
    [JsonProperty("value")]
    public int CurrentValue { get; set; }

    public void Add(int amount) => this.CurrentValue += amount;

    public void Reset() => this.CurrentValue = 0;

    public int Get() => this.CurrentValue;

    [FunctionName(nameof(Counter))]
    public static Task Run([EntityTrigger] IDurableEntityContext ctx)
        => ctx.DispatchAsync<Counter>();
}
```

# <a name="javascript"></a>[Javascript](#tab/javascript)

```javascript
const df = require("durable-functions");

module.exports = df.entity(function(context) {
    const currentValue = context.df.getState(() => 0);
    switch (context.df.operationName) {
        case "add":
            const amount = context.df.getInput();
            context.df.setState(currentValue + amount);
            break;
        case "reset":
            context.df.setState(0);
            break;
        case "get":
            context.df.return(currentValue);
            break;
    }
});
```

---

İstemciler, [varlık istemcisi bağlamayı](durable-functions-bindings.md#entity-client)kullanarak bir varlık işlevi için ("sinyalleme" olarak da bilinir) *işlemleri* sıraya ekleyebilir.

# <a name="c"></a>[C #](#tab/csharp)

```csharp
[FunctionName("EventHubTriggerCSharp")]
public static async Task Run(
    [EventHubTrigger("device-sensor-events")] EventData eventData,
    [DurableClient] IDurableOrchestrationClient entityClient)
{
    var metricType = (string)eventData.Properties["metric"];
    var delta = BitConverter.ToInt32(eventData.Body, eventData.Body.Offset);

    // The "Counter/{metricType}" entity is created on-demand.
    var entityId = new EntityId("Counter", metricType);
    await entityClient.SignalEntityAsync(entityId, "add", delta);
}
```

> [!NOTE]
> Dinamik olarak oluşturulan yakınlıklar, varlıkları tür güvenli bir şekilde sinyalleştirmek için .NET'te de kullanılabilir. Ayrıca, istemciler sinyale ek olarak, düzenleme istemcisi bağlamada [tür güvenli yöntemleri](durable-functions-bindings.md#entity-client-usage) kullanarak bir varlık işlevinin durumunu da sorgulayabilir.

# <a name="javascript"></a>[Javascript](#tab/javascript)

```javascript
const df = require("durable-functions");

module.exports = async function (context) {
    const client = df.getClient(context);
    const entityId = new df.EntityId("Counter", "myCounter");
    await context.df.signalEntity(entityId, "add", 1);
};
```

---

Varlık işlevleri [Dayanıklı Fonksiyonlar 2.0](durable-functions-versions.md) ve üzeri mevcuttur.

## <a name="the-technology"></a>Teknoloji

Arka planda, Dayanıklı İşlevler uzantısı, GitHub'da kod içinde iş akışları oluşturmak için kullanılan açık kaynak kitaplığı olan [Dayanıklı Görev Çerçevesi'nin](https://github.com/Azure/durabletask)üzerine inşa edilmiştir. Azure İşfonksiyonları nın Azure Web İşlerinin sunucusuz evrimi olduğu gibi, Dayanıklı İşlevler de Dayanıklı Görev Çerçevesi'nin sunucusuz evrimidir. Microsoft ve diğer kuruluşlar, görev açısından kritik işlemleri otomatikleştirmek için Dayanıklı Görev Çerçevesini kapsamlı bir şekilde kullanır. Sunucusuz Azure İşlevleri ortamı için doğal bir uyum sağlar.

## <a name="code-constraints"></a>Kod kısıtlamaları

Güvenilir ve uzun süreli yürütme garantileri sağlamak için, orkestratör işlevlerinin izlenmesi gereken bir dizi kodlama kuralı vardır. Daha fazla bilgi için [Orchestrator işlev kodu kısıtlamaları](durable-functions-code-constraints.md) makalesine bakın.

## <a name="billing"></a>Faturalandırma

Dayanıklı Işlevler, Azure İşlevleri ile aynı faturalandırılır. Daha fazla bilgi için Azure [İşlevler fiyatlandırması](https://azure.microsoft.com/pricing/details/functions/)'na bakın. Azure İşlevler [Tüketim planında](../functions-scale.md#consumption-plan)orchestrator işlevlerini yürütürken, dikkat edilmesi gereken bazı faturalandırma davranışları vardır. Bu davranışlar hakkında daha fazla bilgi [için, Dayanıklı Işlevler faturalandırma](durable-functions-billing.md) makalesine bakın.

## <a name="jump-right-in"></a>Hemen atla

Bu dile özel hızlı başlangıç eğitimlerinden birini tamamlayarak Dayanıklı Fonksiyonlar'a 10 dakikadan kısa bir sürede başlayabilirsiniz:

* [Visual Studio 2019'u kullanarak C#](durable-functions-create-first-csharp.md)
* [Visual Studio Code kullanarak JavaScript](quickstart-js-vscode.md)

Her iki quickstarts, yerel oluşturmak ve bir "merhaba dünya" dayanıklı fonksiyonu test. Ardından işlev kodunu Azure’da yayımlayacaksınız. Birlikte orkestralar ve zincirler oluşturduğunuz işlev diğer işlevleri çağırır.

## <a name="learn-more"></a>Daha fazla bilgi edinin

Aşağıdaki video, Dayanıklı Fonksiyonların yararlarını vurgulamaktadır:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Durable-Functions-in-Azure-Functions/player] 

Dayanıklı Fonksiyonlar ve altta yatan teknoloji hakkında daha ayrıntılı bir tartışma için aşağıdaki videoya bakın (.NET'e odaklanmıştır, ancak kavramlar diğer desteklenen diller için de geçerlidir):

> [!VIDEO https://channel9.msdn.com/Events/dotnetConf/2018/S204/player]

Dayanıklı İşlevler Azure [İşlevler](../functions-overview.md)için gelişmiş bir uzantı olduğundan, tüm uygulamalar için uygun değildir. Diğer Azure düzenleme teknolojileriyle karşılaştırma kinlerini görmek için [Azure İşlerini ve Azure Mantıksal Uygulamalarını Karşılaştır'a](../functions-compare-logic-apps-ms-flow-webjobs.md#compare-azure-functions-and-azure-logic-apps)bakın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Dayanıklı Fonksiyonlar fonksiyon türleri ve özellikleri](durable-functions-types-features-overview.md)
