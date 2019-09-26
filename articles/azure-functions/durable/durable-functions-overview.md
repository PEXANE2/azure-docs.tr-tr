---
title: Dayanıklı İşlevler genel bakış-Azure
description: Azure Işlevleri için Dayanıklı İşlevler uzantısına giriş.
services: functions
author: cgillum
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.topic: overview
ms.date: 08/07/2019
ms.author: cgillum
ms.reviewer: azfuncdf
ms.openlocfilehash: a917a823d47d6a072cf5a3ee5d636b432913df9a
ms.sourcegitcommit: 29880cf2e4ba9e441f7334c67c7e6a994df21cfe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/26/2019
ms.locfileid: "71299434"
---
# <a name="what-are-durable-functions"></a>Dayanıklı İşlevler nedir?

*Dayanıklı işlevler* , bir sunucusuz işlem ortamında durum bilgisi olan işlevler yazmanıza olanak sağlayan bir [Azure işlevleri](../functions-overview.md) uzantısıdır. Uzantı, Azure Işlevleri programlama modelini kullanarak [*varlık işlevleri*](durable-functions-entities.md) yazarak [*Orchestrator işlevlerini*](durable-functions-orchestrations.md) ve durum bilgisi olan varlıkları yazarak durum bilgisi olan iş akışları tanımlamanızı sağlar. Arka planda, uzantı durum, kontrol noktaları ve yeniden başlatma işlemlerini yönetip iş mantığınıza odaklanmanızı sağlar.

## <a name="language-support"></a>Desteklenen diller

Dayanıklı İşlevler Şu anda aşağıdaki dilleri desteklemektedir:

* **C#** : hem [önceden derlenmiş sınıf kitaplıkları](../functions-dotnet-class-library.md) hem [ C# de betiği](../functions-reference-csharp.md).
* **F#** : önceden derlenmiş sınıf kitaplıkları F# ve betiği. F#betik yalnızca Azure Işlevleri çalışma zamanının sürüm 1. x 'i için desteklenir.
* **JavaScript**: yalnızca Azure işlevleri çalışma zamanının 2. x sürümü için desteklenir. Dayanıklı İşlevler uzantısının veya sonraki bir sürümün sürüm 1.7.0 gerektirir. 

Dayanıklı İşlevler tüm [Azure işlevleri dillerini](../supported-languages.md)destekleme amacını içerir. Ek dilleri desteklemek için işin en son durumunun [dayanıklı işlevler sorunlar listesine](https://github.com/Azure/azure-functions-durable-extension/issues) bakın.

Azure Işlevleri gibi, [Visual Studio 2019](durable-functions-create-first-csharp.md), [Visual Studio Code](quickstart-js-vscode.md)ve [Azure Portal](durable-functions-create-portal.md)kullanarak dayanıklı işlevler geliştirmenize yardımcı olacak şablonlar vardır.

## <a name="application-patterns"></a>Uygulama desenleri

Dayanıklı İşlevler için birincil kullanım örneği sunucusuz uygulamalarda karmaşık, durum bilgisi olan düzenleme gereksinimlerini basitleştirir. Aşağıdaki bölümlerde Dayanıklı İşlevler avantajlarından faydalanabilecek tipik uygulama desenleri açıklanır:

* [İşlev Zinciri](#chaining)
* [Fan-çıkış/fan](#fan-in-out)
* [Zaman uyumsuz HTTP API 'Leri](#async-http)
* [İzleme](#monitoring)
* [İnsan etkileşimi](#human)
* ['Yı](#aggregator)

### <a name="chaining"></a>#1 model: İşlev zinciri oluşturma

İşlev zincirleme modelinde, bir dizi işlev belirli bir sırada yürütülür. Bu düzende, bir işlevin çıktısı başka bir işlevin girişine uygulanır.

![İşlev zincirleme deseninin diyagramı](./media/durable-functions-concepts/function-chaining.png)

Aşağıdaki örnekte gösterildiği gibi, öz işlev zincirleme modelini uygulamak için Dayanıklı İşlevler kullanabilirsiniz:

#### <a name="c"></a>C#

```csharp
[FunctionName("Chaining")]
public static async Task<object> Run(
    [OrchestrationTrigger] DurableOrchestrationContext context)
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

#### <a name="javascript-functions-2x-only"></a>JavaScript (yalnızca 2. x Işlevleri)

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context) {
    const x = yield context.df.callActivity("F1");
    const y = yield context.df.callActivity("F2", x);
    const z = yield context.df.callActivity("F3", y);
    return    yield context.df.callActivity("F4", z);
});
```

Bu örnekte,,, ve `F1` `F3` `F4` değerleri `F2`işlev uygulamasındaki diğer işlevlerin adlarıdır. Normal kesinlik temelli kodlama yapılarını kullanarak denetim akışı uygulayabilirsiniz. Kod yukarıdan aşağı doğru çalışır. Kod, koşul ve döngüler gibi var olan dil denetim akışı semantiğini içerebilir. `try` / Bloklara`catch` hata işleme mantığını/dahiledebilirsiniz. `finally`

[ `context` Durableorchestrationcontext] \(.net\) parametresini ve `context.df` nesne (JavaScript) kullanarak diğer işlevleri ada, pass parametrelerine ve işlev çıktısını döndürecek şekilde kullanabilirsiniz. Kod her çağırdığında `await` (C#) veya `yield` (JavaScript), dayanıklı işlevler Framework geçerli işlev örneğinin ilerlemesini kontrol etmektedir. İşlem veya VM, yürütme yoluyla geri dönüştürüldüğünde, işlev örneği önceki `await` veya `yield` çağrıdan devam eder. Daha fazla bilgi için, bkz. bir sonraki bölüm, #2 model: İçindeki fan/fan.

> [!NOTE]
> JavaScript içindeki nesne yalnızca [durableorchestrationcontext] parametresini değil, tüm [işlev bağlamını](../functions-reference-node.md#context-object)temsil eder. `context`

### <a name="fan-in-out"></a>#2 model: Fan çıkışı/fanı

Diğer bir deyişle, desenli çıkış/fan, birden çok işlevi paralel olarak yürütür ve sonra tüm işlevlerin bitmesini bekler. Genellikle, bazı toplama işleri işlevlerden döndürülen sonuçlar üzerinde yapılır.

![Fan çıkış/fan deseninin diyagramı](./media/durable-functions-concepts/fan-out-fan-in.png)

Normal işlevlerle, işlevi bir sıraya birden çok ileti göndermesini sağlayarak dışarı aktarabilirsiniz. Geriye doğru zor, çok daha zordur. İçinde fanı için, normal bir işlevde, kuyruk tetiklenen işlevlerin ne zaman sona erdirmek için kod yazarsınız ve sonra işlev çıkışlarını depoladığınızda.

Dayanıklı İşlevler uzantısı görece basit kodla bu düzene sahiptir:

#### <a name="c"></a>C#

```csharp
[FunctionName("FanOutFanIn")]
public static async Task Run(
    [OrchestrationTrigger] DurableOrchestrationContext context)
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

#### <a name="javascript-functions-2x-only"></a>JavaScript (yalnızca 2. x Işlevleri)

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

Fanı-Out işi `F2` işlevin birden çok örneğine dağıtılır. İş, dinamik bir görev listesi kullanılarak izlenir. Çağrılan tüm `Task.WhenAll` işlevlerin bitmesini beklemek `context.df.Task.all` için .NET API veya JavaScript API 'si çağrılır. Sonra, `F2` işlev çıkışları dinamik görev listesinden toplanır ve `F3` işleve geçirilir.

`await` Ya daçağrısında`Task.WhenAll` gerçekleşen otomatik CheckIn veya olası bir Midway kilitlenmesinin veya yeniden başlatmanın zaten tamamlanmış bir görevin yeniden başlatılmasını gerektirmemesinisağlar.`context.df.Task.all` `yield`

> [!NOTE]
> Nadir koşullarda, bir etkinlik işlevi tamamlandıktan sonra ancak tamamlanma alanı düzenleme geçmişine kaydedilmeden önce, bir kilitlenme olması mümkündür. Bu durumda, etkinlik işlevi işlem kurtardıktan sonra baştan sonra yeniden çalıştırılır.

### <a name="async-http"></a>#3 Model: Zaman uyumsuz HTTP API 'Leri

Zaman uyumsuz HTTP API 'SI stili, uzun süreli işlemlerin durumunu dış istemcilerle koordine etme sorununu ele alınmaktadır. Bu düzenin uygulanması için ortak bir yol, bir HTTP uç noktasının uzun süreli eylemi tetiklemesine sahip olunmalıdır. Daha sonra, istemciyi, işlem bittiğinde öğrenmek için yokladığı bir durum uç noktasına yönlendirin.

![HTTP API deseninin diyagramı](./media/durable-functions-concepts/async-http-api.png)

Dayanıklı İşlevler, bu düzene yönelik **yerleşik destek** sağlar, bu sayede yazmanız gereken kod, uzun süre çalışan işlev yürütmeleri ile etkileşime geçmek için gerekir. Örneğin, Dayanıklı İşlevler hızlı başlangıç örnekleri ([C#](durable-functions-create-first-csharp.md) ve [JavaScript](quickstart-js-vscode.md)), yeni Orchestrator işlev örnekleri başlatmak IÇIN kullanabileceğiniz bir basit rest komutu gösterir. Bir örnek başlatıldıktan sonra uzantı, Orchestrator işlev durumunu sorgulayan Web kancası HTTP API 'Lerini kullanıma sunar. 

Aşağıdaki örnekte, bir Orchestrator başlatan ve durumunu sorgulayan REST komutları gösterilmektedir. Anlaşılırsa, bazı protokol ayrıntıları örnekteki atlanır.

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

Dayanıklı İşlevler Runtime, durumu sizin için yönettiğinden, kendi durum izleme mekanizmanızı uygulamanız gerekmez.

Dayanıklı İşlevler uzantısı, uzun süre çalışan düzenlemeleri yöneten yerleşik HTTP API 'Lerini kullanıma sunar. Bunun yerine kendi işlev tetikleyiclerinizi (HTTP, kuyruk veya Azure Event Hubs gibi) ve [Orchestration istemci bağlamasını](durable-functions-bindings.md#orchestration-client)kullanarak kendiniz uygulayabilirsiniz. Örneğin, sonlandırma tetiklenmesi için bir kuyruk iletisi kullanabilirsiniz. Ya da, kimlik doğrulaması için oluşturulmuş bir anahtar kullanan yerleşik HTTP API 'Leri yerine Azure Active Directory kimlik doğrulama ilkesi tarafından korunan bir HTTP tetikleyicisi kullanabilirsiniz.

Daha fazla bilgi için, Dayanıklı İşlevler uzantısını kullanarak HTTP üzerinden zaman uyumsuz, uzun süreli işlemleri nasıl sergilebileceğinizi anlatan [http özellikleri](durable-functions-http-features.md) makalesine bakın.

### <a name="monitoring"></a>#4 Model: İzleme

İzleyici deseninin bir iş akışında esnek ve yinelenen bir işlem anlamına gelir. Belirli koşullar karşılanana kadar bir örnek yoklanıyor. Düzenli bir temizleme işi gibi temel bir senaryoya yönelik olarak normal bir [Zamanlayıcı tetikleyicisi](../functions-bindings-timer.md) kullanabilirsiniz, ancak aralığı statiktir ve örnek yaşam sürelerinin yönetilmesi karmaşık hale gelir. Esnek yineleme aralıkları oluşturmak, görev yaşam sürelerini yönetmek ve tek bir düzenleme işleminden birden çok izleme işlemi oluşturmak için Dayanıklı İşlevler kullanabilirsiniz.

İzleyici deseninin bir örneği, önceki zaman uyumsuz HTTP API senaryosunun tersine çevrilmeye yönelik bir örnektir. Bir dış istemcinin uzun süreli bir işlemi izlemeye yönelik bir uç nokta göstermek yerine, uzun süre çalışan izleyici bir dış uç nokta kullanır ve ardından bir durum değişikliği bekler.

![İzleyici deseninin diyagramı](./media/durable-functions-concepts/monitor.png)

Birkaç kod satırı içinde, rastgele uç noktaları gözlemleyecek birden çok izleyici oluşturmak için Dayanıklı İşlevler kullanabilirsiniz. İzleyiciler bir koşula uyulduğunda veya [Durableorchestrationclient](durable-functions-instance-management.md) izleyicileri sonlandırıyorsa yürütme işlemini sonlandırabilir. Bir izleyicinin `wait` aralığını belirli bir koşula göre değiştirebilirsiniz (örneğin üstel geri alma) 

Aşağıdaki kod temel bir izleyiciyi uygular:

#### <a name="c"></a>C#

```csharp
[FunctionName("MonitorJobStatus")]
public static async Task Run(
    [OrchestrationTrigger] DurableOrchestrationContext context)
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

#### <a name="javascript-functions-2x-only"></a>JavaScript (yalnızca 2. x Işlevleri)

```javascript
const df = require("durable-functions");
const moment = require("moment");

module.exports = df.orchestrator(function*(context) {
    const jobId = context.df.getInput();
    const pollingInternal = getPollingInterval();
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

Bir istek alındığında, bu iş KIMLIĞI için yeni bir düzenleme örneği oluşturulur. Örnek, bir koşul karşılanana ve döngünün çıkış yapılıncaya kadar bir durumu yoklar. Dayanıklı bir Zamanlayıcı yoklama aralığını denetler. Daha sonra, daha fazla iş gerçekleştirilebilir veya düzenleme sona erdirmek üzere. (.Net) veya `context.df.currentUtcDateTime` `expiryTime` (JavaScript) değeri aştığında, izleyici sona erer. `context.CurrentUtcDateTime`

### <a name="human"></a>#5 model: İnsan etkileşimi

Birçok otomatikleştirilmiş süreç, bazı insan etkileşimini içerir. İnsanlar yüksek düzeyde kullanılabilir olmadığından ve bulut hizmetleri olarak yanıt veremediği için otomatikleştirilmiş bir işlem içindeki insanların dahil olması önemlidir. Otomatik bir işlem, zaman aşımları ve maaş mantığı kullanılarak bu etkileşime izin verebilir.

Onay süreci, insan etkileşimini içeren bir iş sürecinin örneğidir. Belirli bir dolar tutarını aşan bir gider raporu için bir yöneticinin onayı gerekli olabilir. Yönetici, 72 saat içinde gider raporunu onaylamaz (örneğin, bu da yönetici tatilde durumunda olursa), bir yükseltme işlemi, başka birinden (Belki de yöneticinin Yöneticisi) onay almak için ' de açılır.

![İnsan etkileşimi deseninin diyagramı](./media/durable-functions-concepts/approval.png)

Bu örnekte, bir Orchestrator işlevi kullanarak bir stili uygulayabilirsiniz. Orchestrator onay istemek için [dayanıklı bir Zamanlayıcı](durable-functions-timers.md) kullanır. Zaman aşımı oluşursa Orchestrator ilerletir. Orchestrator, bir insan etkileşimi tarafından oluşturulan bildirim gibi bir [dış olay](durable-functions-external-events.md)bekler.

Bu örnekler insan etkileşimi modelini göstermek için bir onay işlemi oluşturur:

#### <a name="c"></a>C#

```csharp
[FunctionName("ApprovalWorkflow")]
public static async Task Run(
    [OrchestrationTrigger] DurableOrchestrationContext context)
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

#### <a name="javascript-functions-2x-only"></a>JavaScript (yalnızca 2. x Işlevleri)

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

Dayanıklı süreölçer oluşturmak için (.net) `context.CreateTimer` veya `context.df.createTimer` (JavaScript) çağırın. Bildirim (.net) veya `context.WaitForExternalEvent` `context.df.waitForExternalEvent` (JavaScript) tarafından alınır. Daha sonra `Task.WhenAny` , (.net) `context.df.Task.any` veya (JavaScript), ilerletilip yükseltilmeyeceğine karar vermek için çağrılır (Öncelikle zaman aşımı olur) veya onay işlemini işleyin (onay zaman aşımından önce alınır).

Bir dış istemci, [YERLEŞIK HTTP API 'lerini](durable-functions-http-api.md#raise-event) kullanarak veya başka bir Işlevden [Durableorchestrationclient. RaiseEventAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_RaiseEventAsync_System_String_System_String_System_Object_) API 'sini kullanarak olay bildirimini bekleyen bir Orchestrator işlevine teslim edebilir:

```csharp
[FunctionName("RaiseEventToOrchestration")]
public static async Task Run(
    [HttpTrigger] string instanceId,
    [OrchestrationClient] DurableOrchestrationClient client)
{
    bool isApproved = true;
    await client.RaiseEventAsync(instanceId, "ApprovalEvent", isApproved);
}
```

```javascript
const df = require("durable-functions");

module.exports = async function (context) {
    const client = df.getClient(context);
    const isApproved = true;
    await client.raiseEvent(instanceId, "ApprovalEvent", isApproved);
};
```

```bash
curl -d "true" http://localhost:7071/runtime/webhooks/durabletask/instances/{instanceId}/raiseEvent/ApprovalEvent -H "Content-Type: application/json"
```

### <a name="aggregator"></a>#6 Model: Toplayıcı (Önizleme)

Altıncı model, olay verilerini bir süre içinde tek bir adreslenebilir *varlığa*toplamak üzere kullanılır. Bu düzende, toplanan veriler birden çok kaynaktan gelebilir, toplu olarak teslim edilebilir veya uzun sürelerle dağılmış olabilir. Toplayıcı, ulaşan olay verileri üzerinde işlem yapması gerekebilir ve dış istemcilerin toplanan verileri sorgulaması gerekebilir.

![Toplayıcı diyagramı](./media/durable-functions-concepts/aggregator.png)

Bu düzenin normal, durum bilgisiz işlevlerle uygulamaya çalışılması, eşzamanlılık denetiminin çok büyük bir zorluk haline gelmesi. Aynı anda aynı verileri değiştiren birden çok iş parçacığı hakkında endişelenmeniz gerekmez, ayrıca toplayıcı 'nın aynı anda yalnızca tek bir VM üzerinde çalışmasını sağlamaya de endişelenmeniz gerekir.

Dayanıklı bir [varlık işlevi](durable-functions-preview.md#entity-functions)kullanarak, tek bir işlev olarak bu model kolayca uygulanabilir.

```csharp
[FunctionName("Counter")]
public static void Counter([EntityTrigger] IDurableEntityContext ctx)
{
    int currentValue = ctx.GetState<int>();

    switch (ctx.OperationName.ToLowerInvariant())
    {
        case "add":
            int amount = ctx.GetInput<int>();
            currentValue += amount;
            break;
        case "reset":
            currentValue = 0;
            break;
        case "get":
            ctx.Return(currentValue);
            break;
    }

    ctx.SetState(currentValue);
}
```

Dayanıklı varlıklar ayrıca .NET sınıfları olarak modellenebilir. Bu model, işlem listesi düzeltildiğinde ve büyük olursa yararlı olabilir. Aşağıdaki örnek, .NET sınıfları ve yöntemleri kullanılarak `Counter` varlığın eşdeğer bir uygulamasıdır.

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

İstemciler, [varlık istemci bağlamasını](durable-functions-bindings.md#entity-client)kullanarak bir varlık işlevi için ("sinyal" olarak da bilinir) *işlemleri* sıraya alabilir.

```csharp
[FunctionName("EventHubTriggerCSharp")]
public static async Task Run(
    [EventHubTrigger("device-sensor-events")] EventData eventData,
    [OrchestrationClient] IDurableOrchestrationClient entityClient)
{
    var metricType = (string)eventData.Properties["metric"];
    var delta = BitConverter.ToInt32(eventData.Body, eventData.Body.Offset);

    // The "Counter/{metricType}" entity is created on-demand.
    var entityId = new EntityId("Counter", metricType);
    await entityClient.SignalEntityAsync(entityId, "add", delta);
}
```

Dinamik olarak oluşturulan proxy 'ler, bir tür kullanımı güvenli şekilde sinyal varlıkları için de kullanılabilir. Ayrıca, istemciler, düzenleme istemci bağlamasında [tür kullanımı uyumlu yöntemler](durable-functions-bindings.md#entity-client-usage) kullanarak bir varlık işlevinin durumunu da sorgulayabilir.

> [!NOTE]
> Varlık işlevleri şu anda yalnızca .NET içinde [Dayanıklı İşlevler 2,0 önizlemenin](durable-functions-preview.md)bir parçası olarak kullanılabilir.

## <a name="the-technology"></a>Teknoloji

Arka planda Dayanıklı İşlevler uzantısı, GitHub 'daki açık kaynaklı ve kodda iş akışları oluşturmak için kullanılan açık kaynaklı bir kitaplık olan [dayanıklı görev çerçevesinin](https://github.com/Azure/durabletask)üzerine kurulmuştur. Azure Işlevleri gibi, Azure WebJobs 'ın sunucusuz bir gelişimi olduğundan, Dayanıklı İşlevler dayanıklı görev çerçevesinin sunucusuz bir gelişmidir. Microsoft ve diğer kuruluşlar, görev açısından kritik işlemleri otomatik hale getirmek için dayanıklı görev çerçevesini yoğun bir şekilde kullanır. Sunucusuz Azure Işlevleri ortamına yönelik doğal bir uyum.

## <a name="code-constraints"></a>Kod kısıtlamaları

Güvenilir ve uzun süreli yürütme garantisi sağlamak için, Orchestrator işlevlerinin izlenmesi gereken bir dizi kodlama kuralı vardır. Daha fazla bilgi için bkz. [Orchestrator işlev kodu kısıtlamaları](durable-functions-code-constraints.md) makalesi.

## <a name="billing"></a>Faturalandırma

Dayanıklı İşlevler Azure Işlevleri ile aynı faturalandırılır. Daha fazla bilgi için [Azure işlevleri fiyatlandırması](https://azure.microsoft.com/pricing/details/functions/). Azure Işlevleri [Tüketim planında](../functions-scale.md#consumption-plan)Orchestrator işlevleri çalıştırıldığında, bazı fatura davranışları göz önünde bulundurulmalıdır. Bu davranışlar hakkında daha fazla bilgi için [dayanıklı işlevler faturalandırma](durable-functions-billing.md) makalesine bakın.

## <a name="jump-right-in"></a>Hemen geç

Bu dile özgü hızlı başlangıç öğreticilerden birini tamamlayarak 10 dakikadan kısa bir süre içinde Dayanıklı İşlevler kullanmaya başlamanızı sağlayabilirsiniz:

* [C#Visual Studio 2019 kullanma](durable-functions-create-first-csharp.md)
* [Visual Studio Code kullanan JavaScript](quickstart-js-vscode.md)

Her iki hızlı başlangıçlarda, "Hello World" dayanıklı işlevini yerel olarak oluşturup test edersiniz. Ardından işlev kodunu Azure’da yayımlayacaksınız. Oluşturduğunuz işlev, diğer işlevlere yapılan çağrıları düzenler ve birbirine zincirler.

## <a name="learn-more"></a>Daha fazla bilgi edinin

Aşağıdaki videoda Dayanıklı İşlevler avantajları vurgulanmıştır:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Durable-Functions-in-Azure-Functions/player] 

Dayanıklı İşlevler ve temel alınan teknolojinin daha ayrıntılı bir tartışması için aşağıdaki videoya bakın (.NET 'e odaklanılmıştır, ancak kavramlar desteklenen diğer diller için de geçerlidir):

> [!VIDEO https://channel9.msdn.com/Events/dotnetConf/2018/S204/player]

Dayanıklı İşlevler [Azure işlevleri](../functions-overview.md)için gelişmiş bir uzantı olduğundan, tüm uygulamalar için uygun değildir. Diğer Azure düzenleme teknolojileriyle bir karşılaştırma için bkz. [Azure Işlevlerini karşılaştırma ve Azure Logic Apps](../functions-compare-logic-apps-ms-flow-webjobs.md#compare-azure-functions-and-azure-logic-apps).

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [İşlev türlerini ve özellikleri Dayanıklı İşlevler](durable-functions-types-features-overview.md)
