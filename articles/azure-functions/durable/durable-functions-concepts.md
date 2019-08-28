---
title: Azure Işlevlerinde Dayanıklı İşlevler desenler ve teknik kavramlar
description: Azure Işlevleri 'ndeki Dayanıklı İşlevler uzantısının, bulutta durum bilgisi yürütme avantajlarından yararlanmanızı nasıl sağladığını öğrenin.
services: functions
author: ggailey777
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: azfuncdf
ms.openlocfilehash: 828bcaa8c93454ba845c30c03c76144310891123
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70098247"
---
# <a name="durable-functions-patterns-and-technical-concepts-azure-functions"></a>Dayanıklı İşlevler desenleri ve teknik kavramlar (Azure Işlevleri)

Dayanıklı İşlevler, [Azure işlevleri](../functions-overview.md) ve [Azure WebJobs](../../app-service/web-sites-create-web-jobs.md)'un bir uzantısıdır. Bir sunucusuz ortamda durum bilgisi olan işlevleri yazmak için Dayanıklı İşlevler kullanabilirsiniz. Uzantı sizin için durumu, denetim noktalarını ve yeniden başlatmaları yönetir. 

Bu makale, Azure Işlevleri ve ortak uygulama desenleri için Dayanıklı İşlevler uzantısının davranışları hakkında ayrıntılı bilgiler sunar. Bilgiler, geliştirme güçlüklerinizi çözmeye yardımcı olmak için Dayanıklı İşlevler nasıl kullanacağınızı belirlemenize yardımcı olabilir.

> [!NOTE]
> Dayanıklı İşlevler, tüm uygulamalar için uygun olmayan Azure Işlevleri için gelişmiş bir uzantıdır. Bu makalede, [Azure işlevlerinde](../functions-overview.md) kavramlara ve sunucusuz uygulama geliştirmeye dahil olan güçlüklere sahip olduğunuz varsayılmaktadır.

## <a name="patterns"></a>Desenler

Bu bölümde Dayanıklı İşlevler yararlı olabilecek bazı yaygın uygulama desenleri açıklanmaktadır.

### <a name="chaining"></a>#1 model: İşlev zinciri oluşturma

İşlev zincirleme modelinde, bir dizi işlev belirli bir sırada yürütülür. Bu düzende, bir işlevin çıktısı başka bir işlevin girişine uygulanır.

![İşlev zincirleme deseninin diyagramı](./media/durable-functions-concepts/function-chaining.png)

Aşağıdaki örnekte gösterildiği gibi, öz işlev zincirleme modelini uygulamak için Dayanıklı İşlevler kullanabilirsiniz:

#### <a name="c-script"></a>C# betiği

```csharp
public static async Task<object> Run(DurableOrchestrationContext context)
{
    try
    {
        var x = await context.CallActivityAsync<object>("F1");
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

> [!NOTE]
> İçinde C# önceden derlenmiş dayanıklı bir işlev yazmak ve örnekte gösterilen C# betikte önceden derlenmiş dayanıklı bir işlev yazmak arasında hafif farklar vardır. Önceden derlenmiş C# bir işlevde, dayanıklı parametreler ilgili özniteliklerle birlikte tasarlanmalıdır. `[OrchestrationTrigger]` Parametrenin özniteliği`DurableOrchestrationContext` bir örnektir. Önceden derlenmiş C# dayanıklı bir işlevde, parametreler düzgün biçimde tasarlanmemişse, çalışma zamanı, değişkenleri işleve ekleyemiyor ve bir hata oluşur. Daha fazla örnek için [GitHub 'daki Azure-Functions-dayanıklı-Extension örneklerine](https://github.com/Azure/azure-functions-durable-extension/blob/master/samples)bakın.

#### <a name="javascript-functions-2x-only"></a>JavaScript (yalnızca 2. x Işlevleri)

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context) {
    const x = yield context.df.callActivity("F1");
    const y = yield context.df.callActivity("F2", x);
    const z = yield context.df.callActivity("F3", y);
    return yield context.df.callActivity("F4", z);
});
```

Bu örnekte,,, ve `F1` `F3` `F4` değerleri `F2`işlev uygulamasındaki diğer işlevlerin adlarıdır. Normal kesinlik temelli kodlama yapılarını kullanarak denetim akışı uygulayabilirsiniz. Kod yukarıdan aşağı doğru çalışır. Kod, koşul ve döngüler gibi var olan dil denetim akışı semantiğini içerebilir. `try` / Bloklara`catch` hata işleme mantığını/dahiledebilirsiniz. `finally`

Diğer işlevleri ada, `context` geçiş parametrelerine ve işlev çıkışını döndürecek\) şekilde çağırmak `context.df` için [durableorchestrationcontext] \(.net parametresini ve nesnesini (JavaScript) kullanabilirsiniz. Kod her çağırdığında `await` (C#) veya `yield` (JavaScript), dayanıklı işlevler Framework geçerli işlev örneğinin ilerlemesini kontrol etmektedir. İşlem veya VM, yürütme yoluyla geri dönüştürüldüğünde, işlev örneği önceki `await` veya `yield` çağrıdan devam eder. Daha fazla bilgi için, bkz. bir sonraki bölüm, #2 model: İçindeki fan/fan.

> [!NOTE]
> JavaScript içindeki [](../functions-reference-node.md#context-object)nesne, yalnızca [durableorchestrationcontext] parametresini değil, tüm işlev bağlamını temsil eder. `context`

### <a name="fan-in-out"></a>#2 model: Fan çıkışı/fanı

Diğer bir deyişle, desenli çıkış/fan, birden çok işlevi paralel olarak yürütür ve sonra tüm işlevlerin bitmesini bekler. Genellikle, bazı toplama işleri işlevlerden döndürülen sonuçlar üzerinde yapılır.

![Fan çıkış/fan deseninin diyagramı](./media/durable-functions-concepts/fan-out-fan-in.png)

Normal işlevlerle, işlevi bir sıraya birden çok ileti göndermesini sağlayarak dışarı aktarabilirsiniz. Geriye doğru zor, çok daha zordur. İçinde fanı için, normal bir işlevde, kuyruk tetiklenen işlevlerin ne zaman sona erdirmek için kod yazarsınız ve sonra işlev çıkışlarını depoladığınızda. 

Dayanıklı İşlevler uzantısı görece basit kodla bu düzene sahiptir:

#### <a name="c-script"></a>C# betiği

```csharp
public static async Task Run(DurableOrchestrationContext context)
{
    var parallelTasks = new List<Task<int>>();

    // Get a list of N work items to process in parallel.
    object[] workBatch = await context.CallActivityAsync<object[]>("F1");
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

### <a name="async-http"></a>#3 Model: Zaman uyumsuz HTTP API 'Leri

Zaman uyumsuz HTTP API 'Leri, uzun süreli işlemlerin durumunu dış istemcilerle koordine etme sorununa yöneliktir. Bu düzenin uygulanması için ortak bir yol, bir HTTP çağrısının uzun süreli eylemi tetiklemesine sahip değildir. Daha sonra, istemciyi, işlem bittiğinde öğrenmek için yokladığı bir durum uç noktasına yönlendirin.

![HTTP API deseninin diyagramı](./media/durable-functions-concepts/async-http-api.png)

Dayanıklı İşlevler, uzun süreli işlev yürütmeleri ile etkileşim kurmak için yazdığınız kodu basitleştiren yerleşik API 'Ler sağlar. Dayanıklı İşlevler hızlı başlangıç örnekleri ([C#](durable-functions-create-first-csharp.md) ve [JavaScript](quickstart-js-vscode.md)), yeni Orchestrator işlev örnekleri başlatmak IÇIN kullanabileceğiniz bir basit rest komutu gösterir. Bir örnek başlatıldıktan sonra uzantı, Orchestrator işlev durumunu sorgulayan Web kancası HTTP API 'Lerini kullanıma sunar. 

Aşağıdaki örnekte, bir Orchestrator başlatan ve durumunu sorgulayan REST komutları gösterilmektedir. Daha fazla bilgi için, örnekteki bazı ayrıntılar atlanır.

```
> curl -X POST https://myfunc.azurewebsites.net/orchestrators/DoWork -H "Content-Length: 0" -i
HTTP/1.1 202 Accepted
Content-Type: application/json
Location: https://myfunc.azurewebsites.net/admin/extensions/DurableTaskExtension/b79baf67f717453ca9e86c5da21e03ec

{"id":"b79baf67f717453ca9e86c5da21e03ec", ...}

> curl https://myfunc.azurewebsites.net/admin/extensions/DurableTaskExtension/b79baf67f717453ca9e86c5da21e03ec -i
HTTP/1.1 202 Accepted
Content-Type: application/json
Location: https://myfunc.azurewebsites.net/admin/extensions/DurableTaskExtension/b79baf67f717453ca9e86c5da21e03ec

{"runtimeStatus":"Running","lastUpdatedTime":"2017-03-16T21:20:47Z", ...}

> curl https://myfunc.azurewebsites.net/admin/extensions/DurableTaskExtension/b79baf67f717453ca9e86c5da21e03ec -i
HTTP/1.1 200 OK
Content-Length: 175
Content-Type: application/json

{"runtimeStatus":"Completed","lastUpdatedTime":"2017-03-16T21:20:57Z", ...}
```

Dayanıklı İşlevler çalışma zamanı durumu yönettiğinden, kendi durum izleme mekanizmanızı uygulamanız gerekmez.

Dayanıklı İşlevler uzantısı, uzun süre çalışan düzenlemeleri yöneten yerleşik web kancalarına sahiptir. Kendi işlev tetikleyiclerinizi (http, bir kuyruk veya Azure Event Hubs gibi) ve `orchestrationClient` bağlamayı kullanarak kendiniz de uygulayabilirsiniz. Örneğin, sonlandırma tetiklenmesi için bir kuyruk iletisi kullanabilirsiniz. Veya, kimlik doğrulama için oluşturulmuş bir anahtar kullanan yerleşik web kancaları yerine Azure Active Directory bir kimlik doğrulama ilkesi tarafından korunan bir HTTP tetikleyicisi kullanabilirsiniz.

HTTP API deseninin nasıl kullanılacağına ilişkin bazı örnekler şunlardır:

#### <a name="c"></a>C#

```csharp
// An HTTP-triggered function starts a new orchestrator function instance.
public static async Task<HttpResponseMessage> Run(
    HttpRequestMessage req,
    DurableOrchestrationClient starter,
    string functionName,
    ILogger log)
{
    // The function name comes from the request URL.
    // The function input comes from the request content.
    dynamic eventData = await req.Content.ReadAsAsync<object>();
    string instanceId = await starter.StartNewAsync(functionName, eventData);

    log.LogInformation($"Started orchestration with ID = '{instanceId}'.");

    return starter.CreateCheckStatusResponse(req, instanceId);
}
```

#### <a name="javascript-functions-2x-only"></a>JavaScript (yalnızca 2. x Işlevleri)

```javascript
// An HTTP-triggered function starts a new orchestrator function instance.
const df = require("durable-functions");

module.exports = async function (context, req) {
    const client = df.getClient(context);

    // The function name comes from the request URL.
    // The function input comes from the request content.
    const eventData = req.body;
    const instanceId = await client.startNew(req.params.functionName, undefined, eventData);

    context.log(`Started orchestration with ID = '${instanceId}'.`);

    return client.createCheckStatusResponse(req, instanceId);
};
```

.Net ' te, [durableorchestrationclient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) `starter` parametresi, dayanıklı İşlevler uzantısının `orchestrationClient` bir parçası olan çıkış bağlamasındaki bir değerdir. JavaScript 'te, bu nesne çağırarak `df.getClient(context)`döndürülür. Bu nesneler, yeni veya mevcut Orchestrator işlev örneklerine yönelik olarak başlamak, olayları göndermek, sonlandırmak ve sorgulamak için kullanabileceğiniz yöntemler sağlar.

Yukarıdaki örneklerde, http ile tetiklenen bir işlev gelen URL 'den bir `functionName` değer alır ve değeri [startnewasync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_StartNewAsync_)öğesine geçirir. [Createcheckstatusresponse](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_CreateCheckStatusResponse_System_Net_Http_HttpRequestMessage_System_String_) bağlama API 'si daha sonra bir `Location` başlık ve örnekle ilgili ek bilgiler içeren bir yanıt döndürür. Daha sonra, başlatılan örneğin durumunu aramak veya örneği sonlandırmak için bu bilgileri kullanabilirsiniz.

### <a name="monitoring"></a>#4 Model: İzleme

İzleyici deseninin bir iş akışında esnek ve yinelenen bir işlem anlamına gelir. Belirli koşullar karşılanana kadar bir örnek yoklanıyor. Düzenli bir temizleme işi gibi temel bir senaryoya yönelik olarak normal bir [Zamanlayıcı tetikleyicisi](../functions-bindings-timer.md) kullanabilirsiniz, ancak aralığı statiktir ve örnek yaşam sürelerinin yönetilmesi karmaşık hale gelir. Esnek yineleme aralıkları oluşturmak, görev yaşam sürelerini yönetmek ve tek bir düzenleme işleminden birden çok izleme işlemi oluşturmak için Dayanıklı İşlevler kullanabilirsiniz.

İzleyici deseninin bir örneği, önceki zaman uyumsuz HTTP API senaryosunun tersine çevrilmeye yönelik bir örnektir. Bir dış istemcinin uzun süreli bir işlemi izlemeye yönelik bir uç nokta göstermek yerine, uzun süre çalışan izleyici bir dış uç nokta kullanır ve ardından bir durum değişikliği bekler.

![İzleyici deseninin diyagramı](./media/durable-functions-concepts/monitor.png)

Birkaç kod satırı içinde, rastgele uç noktaları gözlemleyecek birden çok izleyici oluşturmak için Dayanıklı İşlevler kullanabilirsiniz. İzleyiciler bir koşula uyulduğunda veya [Durableorchestrationclient](durable-functions-instance-management.md) izleyicileri sonlandırıyorsa yürütme işlemini sonlandırabilir. Bir izleyicinin `wait` aralığını belirli bir koşula göre değiştirebilirsiniz (örneğin üstel geri alma) 

Aşağıdaki kod temel bir izleyiciyi uygular:

#### <a name="c-script"></a>C# betiği

```csharp
public static async Task Run(DurableOrchestrationContext context)
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

Birçok otomatikleştirilmiş süreç, bazı insan etkileşimini içerir. İnsanlar yüksek düzeyde kullanılabilir olmadığından ve bulut hizmetleri olarak yanıt veremediği için otomatikleştirilmiş bir işlem içindeki insanların dahil olması önemlidir. Otomatik bir işlem, zaman aşımları ve dengeleme mantığını kullanarak buna izin verebilir.

Onay süreci, insan etkileşimini içeren bir iş sürecinin örneğidir. Belirli bir dolar tutarını aşan bir gider raporu için bir yöneticinin onayı gerekli olabilir. Yönetici, 72 saat içinde gider raporunu onaylamaz (örneğin, bu da yönetici tatilde durumunda olursa), bir yükseltme işlemi, başka birinden (Belki de yöneticinin Yöneticisi) onay almak için ' de açılır.

![İnsan etkileşimi deseninin diyagramı](./media/durable-functions-concepts/approval.png)

Bu örnekte, bir Orchestrator işlevi kullanarak bir stili uygulayabilirsiniz. Orchestrator onay istemek için [dayanıklı bir Zamanlayıcı](durable-functions-timers.md) kullanır. Zaman aşımı oluşursa Orchestrator ilerletir. Orchestrator, bir insan etkileşimi tarafından oluşturulan bildirim gibi bir [dış olay](durable-functions-external-events.md)bekler.

Bu örnekler insan etkileşimi modelini göstermek için bir onay işlemi oluşturur:

#### <a name="c-script"></a>C# betiği

```csharp
public static async Task Run(DurableOrchestrationContext context)
{
    await context.CallActivityAsync("RequestApproval");
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
            await context.CallActivityAsync("Escalate");
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
public static async Task Run(string instanceId, DurableOrchestrationClient client)
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
            currentValue += operand;
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

Dayanıklı varlıklar ayrıca .NET sınıfları olarak modellenebilir. Bu işlem, işlemler listesi büyük hale gelirse ve genellikle statikse yararlı olabilir. Aşağıdaki örnek, .NET sınıfları ve yöntemleri kullanılarak `Counter` varlığın eşdeğer bir uygulamasıdır.

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

İstemciler, `orchestrationClient` bağlamayı kullanarak bir varlık işlevi için ("sinyal" olarak da bilinir) *işlemleri* sıraya alabilir.

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

Dinamik olarak oluşturulan proxy 'ler, bir tür kullanımı güvenli şekilde sinyal varlıkları için de kullanılabilir. Ayrıca, istemciler, `orchestrationClient` bağlamadaki yöntemleri kullanarak bir varlık işlevinin durumunu da sorgulayabilir.

> [!NOTE]
> Varlık işlevleri şu anda yalnızca [Dayanıklı İşlevler 2,0 önizlemede](durable-functions-preview.md)kullanılabilir.

## <a name="the-technology"></a>Teknoloji

Arka planda Dayanıklı İşlevler uzantısı, GitHub üzerinde dayanıklı görev düzenlemeleri oluşturmak için kullanılan açık kaynaklı bir kitaplık olan [dayanıklı görev çerçevesinin](https://github.com/Azure/durabletask)üzerine kurulmuştur. Azure Işlevleri gibi, Azure WebJobs 'ın sunucusuz bir gelişimi olduğundan, Dayanıklı İşlevler dayanıklı görev çerçevesinin sunucusuz bir gelişmidir. Microsoft ve diğer kuruluşlar, görev açısından kritik işlemleri otomatik hale getirmek için dayanıklı görev çerçevesini yoğun bir şekilde kullanır. Sunucusuz Azure Işlevleri ortamına yönelik doğal bir uyum.

### <a name="event-sourcing-checkpointing-and-replay"></a>Olay kaynağını belirleme, CheckIn ve Replay

Orchestrator işlevleri, yürütme durumlarını [olay](https://docs.microsoft.com/azure/architecture/patterns/event-sourcing) kaynağını belirleme tasarım düzenini kullanarak güvenilir bir şekilde korur. Bir Orchestration 'un geçerli durumunu doğrudan depolamak yerine, Dayanıklı İşlevler uzantısı, düzenleme işlevinin aldığı eylemlerin tam serisini kaydetmek için yalnızca bir Append deposu kullanır. Yalnızca bir Append deposunda, tam çalışma zamanı durumunun "dökümünü alma" ile karşılaştırıldığında birçok avantaj bulunur. Avantajlar, performansı, ölçeklenebilirliği ve yanıt hızını daha da içerir. Ayrıca işlem verileri için nihai tutarlılık ve tam denetim izleri ve geçmişi de alırsınız. Denetim izleri güvenilir telafi eylemlerini destekler.

Dayanıklı İşlevler, saydam olarak olay kaynağını kullanır. Arka planda, bir Orchestrator `await` işlevindekiC#() `yield` veya (JavaScript) işleci, Orchestrator iş parçacığının denetimini dayanıklı görev çerçevesi dağıtıcısına verir. Dağıtıcı daha sonra Orchestrator işlevinin zamanladığı (bir veya daha fazla alt işlevi çağırma ya da dayanıklı bir Zamanlayıcı zamanlama gibi) tüm yeni eylemleri depolamaya kaydeder. Saydam işleme eylemi Orchestration örneğinin yürütme geçmişine ekler. Geçmiş bir depolama tablosunda depolanır. Sonra Kaydet eylemi, gerçek işi zamanlamak için bir kuyruğa ileti ekler. Bu noktada, Orchestrator işlevi bellekten bellekten kaldırılabilir. 

Azure Işlevleri tüketim planını kullanıyorsanız Orchestrator işlevinin faturalandırılması durduruluyor. Yapılacak daha fazla iş olduğunda, işlev yeniden başlatılır ve durumu yeniden oluşturulur.

Bir Orchestration işlevine daha fazla iş verildiğinde (örneğin, bir yanıt iletisi alındığında veya dayanıklı Zamanlayıcı sona erdiğinde), Orchestrator başlatılır ve yerel durumu yeniden derlemek için başlangıçtan itibaren tüm işlevi yeniden yürütür. 

Yeniden yürütme sırasında, kod bir işlevi çağırmaya çalışırsa (veya başka bir zaman uyumsuz çalışma yaparsanız), dayanıklı görev çerçevesi geçerli düzenleme 'nin yürütme geçmişini çağırır. [Etkinlik işlevinin](durable-functions-types-features-overview.md#activity-functions) zaten yürütüldüğünü ve bir sonuç verdiğini belirlerse, bu işlevin sonucunu yeniden yürütür ve Orchestrator kodu çalışmaya devam eder. Yeniden yürütme, işlev kodu tamamlanana kadar veya yeni zaman uyumsuz çalışmayı zamanlana kadar devam eder.

### <a name="orchestrator-code-constraints"></a>Orchestrator kod kısıtlamaları

Orchestrator kodunun yeniden yürütme davranışı, bir Orchestrator işlevinde yazabileceğiniz kod türü üzerinde kısıtlamalar oluşturur. Örneğin, Orchestrator kodunun belirleyici olması gerekir, çünkü birden çok kez yeniden yürütülecektir ve her seferinde aynı sonucu üretmelidir. Kısıtlamaların tüm listesi için bkz. [Orchestrator Code kısıtlamalar](durable-functions-checkpointing-and-replay.md#orchestrator-code-constraints).

## <a name="monitoring-and-diagnostics"></a>İzleme ve tanılama

İşlev uygulamanızı bir Azure Application Insights izleme anahtarıyla ayarlarsanız Dayanıklı İşlevler uzantısı, yapılandırılmış izleme verilerini otomatik olarak [Application Insights](../functions-monitoring.md) olarak yayar. İzleme verilerini, izlemelerinizin eylemlerini ve ilerlemesini izlemek için kullanabilirsiniz.

Dayanıklı İşlevler izleme olaylarının, [Application Insights Analytics](../../application-insights/app-insights-analytics.md)kullandığınızda Application Insights portalında nasıl göründüğünü bir örnek aşağıda verilmiştir:

![Sorgu sonuçlarını Application Insights](./media/durable-functions-concepts/app-insights-1.png)

Her günlük girişinde yararlı yapılandırılmış verileri `customDimensions` alanda bulabilirsiniz. Tam olarak genişletilen bir giriş örneği aşağıda verilmiştir:

![Bir Application Insights sorgusunda customDimensions alanı](./media/durable-functions-concepts/app-insights-2.png)

Dayanıklı görev çerçevesi dağıtıcılarının yeniden yürütme davranışı nedeniyle, yeniden yürütülmüş eylemler için gereksiz günlük girdilerini görmeyi bekleyebilir. Gereksiz günlük girişleri, çekirdek altyapısının yeniden yürütme davranışını anlamanıza yardımcı olabilir. [Tanılama](durable-functions-diagnostics.md) makalesinde, yalnızca "gerçek zamanlı" günlükleri görebileceğiniz şekilde, yeniden yürütme günlüklerini filtreleyerek örnek sorgular gösterilmektedir.

## <a name="storage-and-scalability"></a>Depolama ve ölçeklenebilirlik

Dayanıklı İşlevler uzantısı, yürütme geçmişi durumunu sürdürmek ve işlev yürütmeyi tetiklemek için Azure Storage 'daki kuyrukları, tabloları ve Blobları kullanır. İşlev uygulaması için varsayılan depolama hesabını kullanabilir veya ayrı bir depolama hesabı yapılandırabilirsiniz. Depolama aktarım hızı limitlerini temel alan ayrı bir hesap isteyebilirsiniz. Yazdığınız Orchestrator kodu, bu depolama hesaplarındaki varlıklarla etkileşime girmez. Dayanıklı görev çerçevesi, varlıkları doğrudan bir uygulama ayrıntısı olarak yönetir.

Orchestrator işlevleri etkinlik işlevlerini zamanlayın ve bunların yanıtlarını iç sıra iletileri aracılığıyla alır. Bir işlev uygulaması Azure Işlevleri tüketim planında çalıştığında, [Azure işlevleri ölçek denetleyicisi](../functions-scale.md#how-the-consumption-and-premium-plans-work) bu kuyrukları izler. Yeni işlem örnekleri gerektiği şekilde eklenir. Birden çok VM 'ye ölçeklendirildiğinde, bir sanal makine üzerinde bir Orchestrator işlevi çalışabilir, ancak Orchestrator işlevinin çağırdığı etkinlik işlevleri birçok farklı VM üzerinde çalışabilir. Dayanıklı İşlevler ölçek davranışı hakkında daha fazla bilgi için bkz. [performans ve ölçek](durable-functions-perf-and-scale.md).

Orchestrator hesaplarının yürütme geçmişi tablo depolama alanında depolanır. Bir örnek belirli bir sanal makineye yeniden doldurma yaptığınızda, Orchestrator kendi yürütme geçmişini tablo depolamadan getirir, böylece yerel durumunu yeniden oluşturabilir. Tablo depolamada geçmişi sağlamak için uygun bir boyut, [Azure Depolama Gezgini](../../vs-azure-tools-storage-manage-with-storage-explorer.md) gibi araçları kullanarak yaptığınız düzenlemeleri görmenizi sağlar.

Depolama Blobları, birincil olarak birden çok VM genelinde Orchestration örneklerinin ölçeğini koordine etmek için bir kiralama mekanizması olarak kullanılır. Depolama Blobları, doğrudan tablolarda veya kuyruklarda depolanabilecek büyük iletilerle ilgili verileri tutar.

![Azure Depolama Gezgini ekran görüntüsü](./media/durable-functions-concepts/storage-explorer.png)

> [!NOTE]
> Tablo depolamada yürütme geçmişini görmek kolay ve kullanışlı olsa da, bu tablo üzerinde hiçbir bağımlılık yapmayın. Dayanıklı İşlevler uzantısı geliştikçe tablo değişebilir.

## <a name="known-issues"></a>Bilinen sorunlar

Tüm bilinen sorunlar [GitHub sorunları](https://github.com/Azure/azure-functions-durable-extension/issues) listesinde izlenmelidir. Bir sorunla karşılaşırsanız ve sorunu GitHub 'da bulamazsanız yeni bir sorun açın. Sorunun ayrıntılı bir açıklamasını ekleyin.

## <a name="next-steps"></a>Sonraki adımlar

Dayanıklı İşlevler hakkında daha fazla bilgi için bkz. [dayanıklı işlevler işlev türleri ve özellikleri](durable-functions-types-features-overview.md). 

Kullanmaya başlamak için:

> [!div class="nextstepaction"]
> [İlk dayanıklı işlevinizi oluşturma](durable-functions-create-first-csharp.md)

[DurableOrchestrationContext]: https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html
