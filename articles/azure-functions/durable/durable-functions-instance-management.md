---
title: Dayanıklı İşlevler örnekleri yönetme-Azure
description: Azure Işlevleri için Dayanıklı İşlevler uzantısı 'ndaki örnekleri yönetmeyi öğrenin.
author: cgillum
ms.topic: conceptual
ms.date: 11/02/2019
ms.author: azfuncdf
ms.openlocfilehash: 1837d342c4476633ee33a8579abe7389ac9bbddf
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80476821"
---
# <a name="manage-instances-in-durable-functions-in-azure"></a>Azure 'da Dayanıklı İşlevler örnekleri yönetme

Azure Işlevleri için [dayanıklı işlevler](durable-functions-overview.md) uzantısını kullanıyorsanız veya bunu yapmaya başlamak istiyorsanız, en iyi kullanım düzeyini kullandığınızdan emin olun. Nasıl yönetileceği hakkında daha fazla bilgi edinmek için Dayanıklı İşlevler düzenleme örneklerinizi iyileştirebilirsiniz. Bu makale her örnek yönetimi işleminin ayrıntılarına gider.

Örneğin, örnekleri başlatabilir ve sonlandırabilirsiniz, ancak tüm örnekleri ve sorgu örneklerini filtre ile sorgulama özelliği de dahil olmak üzere örnekleri sorgulayabilirsiniz. Ayrıca, örneklere olay gönderebilir, düzenleme tamamlanmasını bekleyebilir ve HTTP yönetimi Web kancası URL 'Lerini alabilirsiniz. Bu makalede, örnekleri geri sarma, örnek geçmişini temizleme ve bir görev merkezini silme dahil olmak üzere diğer yönetim işlemleri de ele alınmaktadır.

Dayanıklı İşlevler, bu yönetim işlemlerinin her birini nasıl uygulamak istediğinize ilişkin seçenekleriniz vardır. Bu makalede hem .NET (C#) hem de JavaScript için [Azure Functions Core Tools](../functions-run-local.md) kullanan örnekler sağlanmaktadır.

## <a name="start-instances"></a>Başlangıç örnekleri

Bir düzenleme örneğini başlamamak önemlidir. Bu, başka bir işlevin tetikleyicisinde Dayanıklı İşlevler bağlama kullandığınızda genellikle yapılır.

`StartNewAsync` [Orchestration istemcisi bağlamasındaki](durable-functions-bindings.md#orchestration-client) ( `startNew` .net) veya (JavaScript) yöntemi yeni bir örnek başlatır. Bu yöntem, dahili olarak bir iletiyi denetim kuyruğuna sıraya alır, daha sonra [düzenleme tetikleyicisi bağlamasını](durable-functions-bindings.md#orchestration-trigger)kullanan belirtilen ada sahip bir işlevin başlangıcını tetikler.

Bu zaman uyumsuz işlem, Orchestration işlemi başarıyla zamanlandığında tamamlanır.

Yeni bir Orchestration örneği başlatmak için parametreler aşağıdaki gibidir:

* **Ad**: zamanlanacak Orchestrator işlevinin adı.
* **Giriş**: Orchestrator işlevine giriş olarak geçirilmesi gereken tüm JSON seri hale getirilebilir verileri.
* **InstanceId**: (isteğe bağlı) ÖRNEĞIN benzersiz kimliği. Bu parametreyi belirtmezseniz, yöntemi rastgele bir KIMLIK kullanır.

> [!TIP]
> Örnek KIMLIĞI için rastgele bir tanımlayıcı kullanın. Rastgele örnek kimlikleri, Orchestrator işlevlerini birden çok VM genelinde ölçeklendirirken eşit yük dağıtımına yardımcı olur. Rastgele olmayan örnek kimlikleri kullanmak için uygun zaman, KIMLIğIN bir dış kaynaktan gelmesi veya [Singleton Orchestrator](durable-functions-singletons.md) deseninin ne zaman uygulanacağı olur.

Aşağıdaki kod, yeni bir Orchestration örneği başlatan örnek bir işlevdir:

# <a name="c"></a>[, #](#tab/csharp)

```csharp
[FunctionName("HelloWorldQueueTrigger")]
public static async Task Run(
    [QueueTrigger("start-queue")] string input,
    [DurableClient] IDurableOrchestrationClient starter,
    ILogger log)
{
    string instanceId = await starter.StartNewAsync("HelloWorld", input);
    log.LogInformation($"Started orchestration with ID = '{instanceId}'.");
}
```

> [!NOTE]
> Önceki C# kodu Dayanıklı İşlevler 2. x içindir. `OrchestrationClient` Dayanıklı işlevler 1. x için `DurableClient` özniteliği yerine özniteliği kullanmanız gerekir ve yerine `DurableOrchestrationClient` parametre türünü kullanmanız gerekir. `IDurableOrchestrationClient` Sürümler arasındaki farklılıklar hakkında daha fazla bilgi için [dayanıklı işlevler sürümler](durable-functions-versions.md) makalesine bakın.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

<a name="javascript-function-json"></a>Aksi belirtilmediği takdirde, bu sayfadaki örnekler HTTP tetikleyicisini Aşağıdaki Function. JSON ile kullanır.

**function. JSON**

```json
{
  "bindings": [
    {
      "name": "req",
      "type": "httpTrigger",
      "direction": "in",
      "methods": ["post"]
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    },
    {
      "name": "starter",
      "type": "durableClient",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

> [!NOTE]
> Bu örnek, sürüm 2. x Dayanıklı İşlevler hedefler. Sürüm 1. x içinde yerine kullanın `orchestrationClient` `durableClient`.

**index. js**

```javascript
const df = require("durable-functions");

module.exports = async function(context, input) {
    const client = df.getClient(context);

    const instanceId = await client.startNew("HelloWorld", undefined, input);
    context.log(`Started orchestration with ID = ${instanceId}.`);
};
```

---

### <a name="azure-functions-core-tools"></a>Azure Functions Core Tools

Ayrıca, [Azure Functions Core Tools](../functions-run-local.md) `durable start-new` komutunu kullanarak doğrudan bir örnek başlatabilirsiniz. Aşağıdaki parametreleri alır:

* (gerekli): başlatılacak işlevin adı. ** `function-name` **
* (isteğe bağlı): işleve satır içi veya bir JSON dosyası aracılığıyla giriş. ** `input` ** Dosyalar için, dosyasının yolunu içeren `@`bir ön eki ekleyin. `@path/to/file.json`
* (isteğe bağlı): Orchestration örneğinin kimliği. ** `id` ** Bu parametreyi belirtmezseniz, komut rastgele bir GUID kullanır.
* (isteğe bağlı): kullanılacak depolama bağlantı dizesini içeren uygulama ayarının adı. ** `connection-string-setting` ** Varsayılan değer AzureWebJobsStorage ' dir.
* (isteğe bağlı): kullanılacak dayanıklı işlevler görev hub 'ının adı. ** `task-hub-name` ** Varsayılan değer DurableFunctionsHub ' dır. Bunu, durableTask: HubName kullanarak [Host. JSON](durable-functions-bindings.md#host-json) içinde de ayarlayabilirsiniz.

> [!NOTE]
> Temel araçlar komutları, bunları bir işlev uygulamasının kök dizininden kullandığınızı varsayar. `connection-string-setting` Ve `task-hub-name` parametrelerini açık bir şekilde sağlarsanız, komutları herhangi bir dizinden çalıştırabilirsiniz. Bu komutları çalışan bir işlev uygulama konağı olmadan çalıştırabilmenize karşın, ana bilgisayar çalışmadığı takdirde bazı etkileri gözlemleyemiyorum olabilirsiniz. Örneğin, `start-new` komut bir başlangıç iletisini hedef görev merkezine sıralar, ancak iletiyi işleyemeyen bir işlev uygulama ana bilgisayarı işlemi olmadıkça düzenleme aslında çalışmaz.

Aşağıdaki komut HelloWorld adlı işlevi başlatır ve dosyanın `counter-data.json` içeriğini buna geçirir:

```bash
func durable start-new --function-name HelloWorld --input @counter-data.json --task-hub-name TestTaskHub
```

## <a name="query-instances"></a>Sorgu örnekleri

Düzenlemeleri yönetme çabalarınızın bir parçası olarak, büyük olasılıkla bir düzenleme örneğinin durumu hakkında bilgi toplamanız gerekir (örneğin, normal veya başarısız olup olmadığını).

`GetStatusAsync` [Orchestration istemcisi bağlamasındaki](durable-functions-bindings.md#orchestration-client) (.net `getStatus` ) veya (JavaScript) yöntemi bir Orchestration örneğinin durumunu sorgular.

`instanceId` (Gerekli) `showHistory` (isteğe bağlı), `showHistoryOutput` (isteğe bağlı) ve `showInput` (isteğe bağlı) parametreleri parametre olarak alır.

* **`showHistory`**: Olarak `true`ayarlanırsa, yanıt yürütme geçmişini içerir.
* **`showHistoryOutput`**: Olarak `true`ayarlanırsa, yürütme geçmişi etkinlik çıkışları içerir.
* **`showInput`**: Olarak `false`ayarlanırsa, yanıt işlevin girişini içermez. Varsayılan değer: `true`.

Yöntemi aşağıdaki özelliklerle bir nesne döndürür:

* **Ad**: Orchestrator işlevinin adı.
* **InstanceId**: Orchestration 'un örnek kimliği ( `instanceId` girişle aynı olmalıdır).
* **CreatedTime**: Orchestrator işlevinin çalışmaya başladığı zaman.
* **LastUpdatedTime**: Orchestration son denetim noktası zaman.
* **Giriş**: işlevin JSON değeri olarak girişi. Bu alan `showInput` , yanlışsa doldurulur.
* **Customstatus**: JSON biçiminde özel düzenleme durumu.
* **Çıktı**: işlevin JSON değeri olarak çıktısı (işlev tamamlandıysa). Orchestrator işlevi başarısız olduysa, bu özellik hata ayrıntılarını içerir. Orchestrator işlevi sonlandırıldıysa, bu özellik sonlandırma sebebini (varsa) içerir.
* **RuntimeStatus**: aşağıdaki değerlerden biri:
  * **Bekliyor**: örnek zamanlandı ancak henüz çalışmaya başlamadı.
  * **Çalışıyor**: örnek çalışmaya başladı.
  * **Tamamlandı**: örnek normal şekilde tamamlandı.
  * **Devam dasnew**: örnek, kendisini yeni bir geçmiş ile yeniden başlattı. Bu durum, geçici bir durumdur.
  * **Başarısız**: örnek bir hata vererek başarısız oldu.
  * **Sonlandırıldı**: örnek aniden durduruldu.
* **Geçmiş**: Orchestration yürütme geçmişi. Bu alan yalnızca, olarak `showHistory` `true`ayarlanmışsa doldurulur.

Örnek yoksa, `null` bu Yöntem (.net `undefined` ) veya (JavaScript) döndürür.

# <a name="c"></a>[, #](#tab/csharp)

```csharp
[FunctionName("GetStatus")]
public static async Task Run(
    [DurableClient] IDurableOrchestrationClient client,
    [QueueTrigger("check-status-queue")] string instanceId)
{
    DurableOrchestrationStatus status = await client.GetStatusAsync(instanceId);
    // do something based on the current status.
}
```

> [!NOTE]
> Önceki C# kodu Dayanıklı İşlevler 2. x içindir. `OrchestrationClient` Dayanıklı işlevler 1. x için `DurableClient` özniteliği yerine özniteliği kullanmanız gerekir ve yerine `DurableOrchestrationClient` parametre türünü kullanmanız gerekir. `IDurableOrchestrationClient` Sürümler arasındaki farklılıklar hakkında daha fazla bilgi için [dayanıklı işlevler sürümler](durable-functions-versions.md) makalesine bakın.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

```javascript
const df = require("durable-functions");

module.exports = async function(context, instanceId) {
    const client = df.getClient(context);

    const status = await client.getStatus(instanceId);
    // do something based on the current status.
}
```

Bkz. Function. JSON yapılandırması için [Başlangıç örnekleri](#javascript-function-json) .

---

### <a name="azure-functions-core-tools"></a>Azure Functions Core Tools

Ayrıca, [Azure Functions Core Tools](../functions-run-local.md) `durable get-runtime-status` komutunu kullanarak bir düzenleme örneğinin durumunu doğrudan almak mümkündür. Aşağıdaki parametreleri alır:

* (gerekli): Orchestration örneğinin kimliği. ** `id` **
* (isteğe bağlı): olarak `true`ayarlanırsa, yanıt işlevin girdisini içerir. ** `show-input` ** Varsayılan değer: `false`.
* (isteğe bağlı): olarak `true`ayarlanırsa, yanıt işlevin çıktısını içerir. ** `show-output` ** Varsayılan değer: `false`.
* (isteğe bağlı): kullanılacak depolama bağlantı dizesini içeren uygulama ayarının adı. ** `connection-string-setting` ** Varsayılan değer: `AzureWebJobsStorage`.
* (isteğe bağlı): kullanılacak dayanıklı işlevler görev hub 'ının adı. ** `task-hub-name` ** Varsayılan değer: `DurableFunctionsHub`. Ayrıca, durableTask: HubName kullanılarak [Host. JSON](durable-functions-bindings.md#host-json)içinde de ayarlanabilir.

Aşağıdaki komut, 0ab8c55a66644d68a3a8b220b12d209c düzenleme örneği KIMLIĞINE sahip bir örnek için durumu (giriş ve çıkış dahil) alır. İşlev uygulamasının kök dizininden `func` komutunu çalıştırdığınız varsayılır:

```bash
func durable get-runtime-status --id 0ab8c55a66644d68a3a8b220b12d209c --show-input true --show-output true
```

Bir Orchestration örneğinin geçmişini `durable get-history` almak için komutunu kullanabilirsiniz. Aşağıdaki parametreleri alır:

* (gerekli): Orchestration örneğinin kimliği. ** `id` **
* (isteğe bağlı): kullanılacak depolama bağlantı dizesini içeren uygulama ayarının adı. ** `connection-string-setting` ** Varsayılan değer: `AzureWebJobsStorage`.
* (isteğe bağlı): kullanılacak dayanıklı işlevler görev hub 'ının adı. ** `task-hub-name` ** Varsayılan değer: `DurableFunctionsHub`. Ayrıca, durableTask: HubName kullanılarak Host. JSON içinde de ayarlanabilir.

```bash
func durable get-history --id 0ab8c55a66644d68a3a8b220b12d209c
```

## <a name="query-all-instances"></a>Tüm örnekleri sorgula

Tek seferde düzenleme yaptığınız bir örneği sorgulamak yerine, bunların tümünü aynı anda sorgulamak daha verimli olabilir.

Tüm düzenleme örneklerinin durumlarını `GetStatusAsync` sorgulamak için (.net `getStatusAll` ) veya (JavaScript) yöntemini kullanabilirsiniz. .NET ' te, bir `CancellationToken` nesneyi iptal etmek istediğiniz durumda geçirebilirsiniz. Yöntemi, parametreleriyle aynı özelliklere `GetStatusAsync` sahip nesneleri döndürür.

# <a name="c"></a>[, #](#tab/csharp)

```csharp
[FunctionName("GetAllStatus")]
public static async Task Run(
    [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post")]HttpRequestMessage req,
    [DurableClient] IDurableOrchestrationClient client,
    ILogger log)
{
    IList<DurableOrchestrationStatus> instances = await client.GetStatusAsync(); // You can pass CancellationToken as a parameter.
    foreach (var instance in instances)
    {
        log.LogInformation(JsonConvert.SerializeObject(instance));
    };
}
```

> [!NOTE]
> Önceki C# kodu Dayanıklı İşlevler 2. x içindir. `OrchestrationClient` Dayanıklı işlevler 1. x için `DurableClient` özniteliği yerine özniteliği kullanmanız gerekir ve yerine `DurableOrchestrationClient` parametre türünü kullanmanız gerekir. `IDurableOrchestrationClient` Sürümler arasındaki farklılıklar hakkında daha fazla bilgi için [dayanıklı işlevler sürümler](durable-functions-versions.md) makalesine bakın.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

```javascript
const df = require("durable-functions");

module.exports = async function(context, req) {
    const client = df.getClient(context);

    const instances = await client.getStatusAll();
    instances.forEach((instance) => {
        context.log(JSON.stringify(instance));
    });
};
```

Bkz. Function. JSON yapılandırması için [Başlangıç örnekleri](#javascript-function-json) .

---

### <a name="azure-functions-core-tools"></a>Azure Functions Core Tools

Ayrıca, [Azure Functions Core Tools](../functions-run-local.md) `durable get-instances` komutu kullanılarak örnekleri doğrudan sorgulamak de mümkündür. Aşağıdaki parametreleri alır:

* (isteğe bağlı): Bu komut sayfalamayı destekler. ** `top` ** Bu parametre, istek başına alınan örnek sayısına karşılık gelir. Varsayılan değer 10 ' dur.
* (isteğe bağlı): hangi sayfa veya örneklerin hangi bölüme alındığını belirten bir belirteç. ** `continuation-token` ** Her `get-instances` yürütme, bir sonraki örnek kümesine belirteç döndürür.
* (isteğe bağlı): kullanılacak depolama bağlantı dizesini içeren uygulama ayarının adı. ** `connection-string-setting` ** Varsayılan değer: `AzureWebJobsStorage`.
* (isteğe bağlı): kullanılacak dayanıklı işlevler görev hub 'ının adı. ** `task-hub-name` ** Varsayılan değer: `DurableFunctionsHub`. Ayrıca, durableTask: HubName kullanılarak [Host. JSON](durable-functions-bindings.md#host-json)içinde de ayarlanabilir.

```bash
func durable get-instances
```

## <a name="query-instances-with-filters"></a>Filtreleri olan sorgu örnekleri

Standart örnek sorgusunun sağlayabilecekleri tüm bilgilere gerçekten ihtiyaç duymuyorsanız ne yapabilirsiniz? Örneğin, düzenleme oluşturma zamanını veya Orchestration çalışma zamanı durumunu yalnızca siz arıyorsanız ne olacak? Filtre uygulayarak sorgunuzu daraltabilirsiniz.

Önceden tanımlanmış `GetStatusAsync` bir filtre kümesiyle eşleşen `getStatusBy` düzenleme örneklerinin listesini almak için (.net) veya (JavaScript) metodunu kullanın.

# <a name="c"></a>[, #](#tab/csharp)

```csharp
[FunctionName("QueryStatus")]
public static async Task Run(
    [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post")]HttpRequestMessage req,
    [DurableClient] IDurableOrchestrationClient client,
    ILogger log)
{
    var runtimeStatus = new List<OrchestrationRuntimeStatus> {
        OrchestrationRuntimeStatus.Completed,
        OrchestrationRuntimeStatus.Running
    };
    IList<DurableOrchestrationStatus> instances = await starter.GetStatusAsync(
        new DateTime(2018, 3, 10, 10, 1, 0),
        new DateTime(2018, 3, 10, 10, 23, 59),
        runtimeStatus
    ); // You can pass CancellationToken as a parameter.
    foreach (var instance in instances)
    {
        log.LogInformation(JsonConvert.SerializeObject(instance));
    };
}
```

> [!NOTE]
> Önceki C# kodu Dayanıklı İşlevler 2. x içindir. `OrchestrationClient` Dayanıklı işlevler 1. x için `DurableClient` özniteliği yerine özniteliği kullanmanız gerekir ve yerine `DurableOrchestrationClient` parametre türünü kullanmanız gerekir. `IDurableOrchestrationClient` Sürümler arasındaki farklılıklar hakkında daha fazla bilgi için [dayanıklı işlevler sürümler](durable-functions-versions.md) makalesine bakın.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

```javascript
const df = require("durable-functions");

module.exports = async function(context, req) {
    const client = df.getClient(context);

    const runtimeStatus = [
        df.OrchestrationRuntimeStatus.Completed,
        df.OrchestrationRuntimeStatus.Running,
    ];
    const instances = await client.getStatusBy(
        new Date(2018, 3, 10, 10, 1, 0),
        new Date(2018, 3, 10, 10, 23, 59),
        runtimeStatus
    );
    instances.forEach((instance) => {
        context.log(JSON.stringify(instance));
    });
};
```

Bkz. Function. JSON yapılandırması için [Başlangıç örnekleri](#javascript-function-json) .

---

### <a name="azure-functions-core-tools"></a>Azure Functions Core Tools

Azure Functions Core Tools, `durable get-instances` komutu filtreler ile de kullanabilirsiniz. `top`Belirtilen, `continuation-token`, `connection-string-setting`, ve `task-hub-name` parametrelerine ek olarak, üç filtre parametresi (`created-after`, `created-before`ve `runtime-status`) kullanabilirsiniz.

* (isteğe bağlı): Bu tarih/saat (UTC) sonrasında oluşturulan örnekleri alın. ** `created-after` ** ISO 8601 biçimlendirildi tarih saat kabul edildi.
* (isteğe bağlı): Bu tarih/saat (UTC) öncesinde oluşturulan örnekleri alın. ** `created-before` ** ISO 8601 biçimlendirildi tarih saat kabul edildi.
* (isteğe bağlı): belirli bir durumdaki örnekleri alın (örneğin, çalışıyor veya tamamlandı). ** `runtime-status` ** Birden çok (boşlukla ayrılmış) durum sağlayabilir.
* (isteğe bağlı): istek başına alınan örnek sayısı. ** `top` ** Varsayılan değer 10 ' dur.
* (isteğe bağlı): hangi sayfa veya örneklerin hangi bölüme alındığını belirten bir belirteç. ** `continuation-token` ** Her `get-instances` yürütme, bir sonraki örnek kümesine belirteç döndürür.
* (isteğe bağlı): kullanılacak depolama bağlantı dizesini içeren uygulama ayarının adı. ** `connection-string-setting` ** Varsayılan değer: `AzureWebJobsStorage`.
* (isteğe bağlı): kullanılacak dayanıklı işlevler görev hub 'ının adı. ** `task-hub-name` ** Varsayılan değer: `DurableFunctionsHub`. Ayrıca, durableTask: HubName kullanılarak [Host. JSON](durable-functions-bindings.md#host-json)içinde de ayarlanabilir.

Herhangi bir filtre`created-after`(, `created-before`, veya `runtime-status`) sağlamazsanız, komut çalışma zamanı durumu veya oluşturulma `top` zamanına göre yalnızca örnekleri alır.

```bash
func durable get-instances --created-after 2018-03-10T13:57:31Z --created-before  2018-03-10T23:59Z --top 15
```

## <a name="terminate-instances"></a>Örnekleri Sonlandır

Çalışması çok uzun süren bir düzenleme örneğiniz varsa veya herhangi bir nedenle tamamlanmadan önce bunu durdurmanız gerekiyorsa, bunu sonlandırma seçeneğine sahipsiniz.

Örnekleri sonlandırmak için [Orchestration istemci bağlamasının](durable-functions-bindings.md#orchestration-client) `TerminateAsync` (.net) `terminate` veya (JavaScript) yöntemini kullanabilirsiniz. İki parametre, günlüklere ve `instanceId` örnek durumuna `reason` yazılan bir ve bir dizedir.

# <a name="c"></a>[, #](#tab/csharp)

```csharp
[FunctionName("TerminateInstance")]
public static Task Run(
    [DurableClient] IDurableOrchestrationClient client,
    [QueueTrigger("terminate-queue")] string instanceId)
{
    string reason = "It was time to be done.";
    return client.TerminateAsync(instanceId, reason);
}
```

> [!NOTE]
> Önceki C# kodu Dayanıklı İşlevler 2. x içindir. `OrchestrationClient` Dayanıklı işlevler 1. x için `DurableClient` özniteliği yerine özniteliği kullanmanız gerekir ve yerine `DurableOrchestrationClient` parametre türünü kullanmanız gerekir. `IDurableOrchestrationClient` Sürümler arasındaki farklılıklar hakkında daha fazla bilgi için [dayanıklı işlevler sürümler](durable-functions-versions.md) makalesine bakın.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

```javascript
const df = require("durable-functions");

module.exports = async function(context, instanceId) {
    const client = df.getClient(context);

    const reason = "It was time to be done.";
    return client.terminate(instanceId, reason);
};
```

Bkz. Function. JSON yapılandırması için [Başlangıç örnekleri](#javascript-function-json) .

---

Sonlandırılan bir örnek sonunda `Terminated` durumuna geçer. Ancak, bu geçiş hemen gerçekleşmeyecektir. Bunun yerine, sonlandırma işlemi görev hub 'ında bu örneğe ilişkin diğer işlemlerle birlikte sıraya alınır. [Örnek sorgu](#query-instances) API 'lerini kullanarak, sonlandırılmış bir örnek `Terminated` duruma gerçekten ulaştığında emin olabilirsiniz.

> [!NOTE]
> Örnek sonlandırma Şu anda yaymıyor. Etkinlik işlevleri ve alt düzenlemeler, onları çağıran düzenleme örneğini sonlandırıp sonlandırmadığına bakılmaksızın tamamlama için çalışır.

### <a name="azure-functions-core-tools"></a>Azure Functions Core Tools

Ayrıca, [Azure Functions Core Tools](../functions-run-local.md) `durable terminate` komutunu kullanarak bir düzenleme örneğini doğrudan sonlandırabilirsiniz. Aşağıdaki parametreleri alır:

* (gerekli): sonlandırılacak düzenleme örneğinin kimliği. ** `id` **
* (isteğe bağlı): sonlandırma nedeni. ** `reason` **
* (isteğe bağlı): kullanılacak depolama bağlantı dizesini içeren uygulama ayarının adı. ** `connection-string-setting` ** Varsayılan değer: `AzureWebJobsStorage`.
* (isteğe bağlı): kullanılacak dayanıklı işlevler görev hub 'ının adı. ** `task-hub-name` ** Varsayılan değer: `DurableFunctionsHub`. Ayrıca, durableTask: HubName kullanılarak [Host. JSON](durable-functions-bindings.md#host-json)içinde de ayarlanabilir.

Aşağıdaki komut, 0ab8c55a66644d68a3a8b220b12d209c KIMLIKLI bir Orchestration örneğini sonlandırır:

```bash
func durable terminate --id 0ab8c55a66644d68a3a8b220b12d209c --reason "It was time to be done."
```

## <a name="send-events-to-instances"></a>Olayları örneklere gönder

Bazı senaryolarda, Orchestrator işlevlerinin dış olayları bekleyip dinleyebilmesi önemlidir. Bu, [insan etkileşimi](durable-functions-overview.md#human)için bekleyen [izleme işlevlerini](durable-functions-overview.md#monitoring) ve işlevlerini içerir.

`RaiseEventAsync` `raiseEvent` [Düzenleme istemci bağlamasının](durable-functions-bindings.md#orchestration-client)(.net) yöntemini veya (JavaScript) yöntemini kullanarak çalışan örneklere olay bildirimleri gönderin. Bu olayları işleyebilen örnekler `WaitForExternalEvent` (.net) veya `waitForExternalEvent` (JavaScript) çağrısına bir çağrı bekleyen olanlardır.

`RaiseEventAsync` (.Net) ve `raiseEvent` (JavaScript) parametreleri aşağıdaki gibidir:

* **InstanceId**: ÖRNEĞIN benzersiz kimliği.
* **EventName**: gönderileceği etkinliğin adı.
* **Eventdata**: örneğe gönderilmek üzere bir JSON seri hale getirilebilir yük.

# <a name="c"></a>[, #](#tab/csharp)

```csharp
[FunctionName("RaiseEvent")]
public static Task Run(
    [DurableClient] IDurableOrchestrationClient client,
    [QueueTrigger("event-queue")] string instanceId)
{
    int[] eventData = new int[] { 1, 2, 3 };
    return client.RaiseEventAsync(instanceId, "MyEvent", eventData);
}
```

> [!NOTE]
> Önceki C# kodu Dayanıklı İşlevler 2. x içindir. `OrchestrationClient` Dayanıklı işlevler 1. x için `DurableClient` özniteliği yerine özniteliği kullanmanız gerekir ve yerine `DurableOrchestrationClient` parametre türünü kullanmanız gerekir. `IDurableOrchestrationClient` Sürümler arasındaki farklılıklar hakkında daha fazla bilgi için [dayanıklı işlevler sürümler](durable-functions-versions.md) makalesine bakın.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

```javascript
const df = require("durable-functions");

module.exports = async function(context, instanceId) {
    const client = df.getClient(context);

    const eventData = [ 1, 2, 3 ];
    return client.raiseEvent(instanceId, "MyEvent", eventData);
};
```

Bkz. Function. JSON yapılandırması için [Başlangıç örnekleri](#javascript-function-json) .

---

> [!NOTE]
> Belirtilen örnek KIMLIĞINE sahip bir düzenleme örneği yoksa, olay iletisi atılır. Bir örnek varsa ancak henüz olay bekliyorsa, olay alınana ve işlenmek üzere hazırlanana kadar örnek durumunda depolanır.

### <a name="azure-functions-core-tools"></a>Azure Functions Core Tools

Ayrıca, [Azure Functions Core Tools](../functions-run-local.md) `durable raise-event` komutunu kullanarak bir düzenleme örneğine doğrudan bir olay da yükseltebilirsiniz. Aşağıdaki parametreleri alır:

* (gerekli): Orchestration örneğinin kimliği. ** `id` **
* **`event-name`**: Tetikedilecek etkinliğin adı.
* (isteğe bağlı): Orchestration örneğine gönderilen veriler. ** `event-data` ** Bu bir JSON dosyasının yolu olabilir veya verileri doğrudan komut satırına sağlayabilirsiniz.
* (isteğe bağlı): kullanılacak depolama bağlantı dizesini içeren uygulama ayarının adı. ** `connection-string-setting` ** Varsayılan değer: `AzureWebJobsStorage`.
* (isteğe bağlı): kullanılacak dayanıklı işlevler görev hub 'ının adı. ** `task-hub-name` ** Varsayılan değer: `DurableFunctionsHub`. Ayrıca, durableTask: HubName kullanılarak [Host. JSON](durable-functions-bindings.md#host-json)içinde de ayarlanabilir.

```bash
func durable raise-event --id 0ab8c55a66644d68a3a8b220b12d209c --event-name MyEvent --event-data @eventdata.json
```

```bash
func durable raise-event --id 1234567 --event-name MyOtherEvent --event-data 3
```

## <a name="wait-for-orchestration-completion"></a>Düzenleme tamamlanmasını bekle

Uzun süre çalışan düzenlemeler içinde, bir düzenleme işleminin sonuçlarını beklemek ve almak isteyebilirsiniz. Bu gibi durumlarda, Orchestration üzerinde bir zaman aşımı süresi tanımlayabilmek de yararlı olur. Zaman aşımı aşılırsa, sonuçlar yerine Orchestration 'un durumu döndürülmelidir.

`WaitForCompletionOrCreateCheckStatusResponseAsync` (.Net) veya `waitForCompletionOrCreateCheckStatusResponse` (JavaScript) yöntemi bir Orchestration örneğinden zaman uyumlu olarak gerçek çıktıyı almak için kullanılabilir. Varsayılan olarak, bu yöntemler için `timeout`varsayılan değer olan 10 saniye ve için `retryInterval`1 saniye kullanır.  

Bu API 'nin nasıl kullanılacağını gösteren örnek bir HTTP tetikleyici işlevi aşağıda verilmiştir:

# <a name="c"></a>[, #](#tab/csharp)

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/HttpSyncStart.cs)]

# <a name="javascript"></a>[JavaScript](#tab/javascript)

[!code-javascript[Main](~/samples-durable-functions/samples/javascript/HttpSyncStart/index.js)]

Bkz. Function. JSON yapılandırması için [Başlangıç örnekleri](#javascript-function-json) .

---

Aşağıdaki satırı kullanarak işlevi çağırın. Zaman aşımı için 2 saniye ve yeniden deneme aralığı için 0,5 saniye kullanın:

```bash
    http POST http://localhost:7071/orchestrators/E1_HelloSequence/wait?timeout=2&retryInterval=0.5
```

Orchestration örneğinden yanıt almak için gereken zamana bağlı olarak iki durum vardır:

* Düzenleme örnekleri tanımlı zaman aşımı (Bu durumda 2 saniye) içinde tamamlanır ve yanıt, zaman uyumlu olarak teslim edilen gerçek düzenleme örneği çıktıdır:

    ```http
        HTTP/1.1 200 OK
        Content-Type: application/json; charset=utf-8
        Date: Thu, 14 Dec 2018 06:14:29 GMT
        Transfer-Encoding: chunked

        [
            "Hello Tokyo!",
            "Hello Seattle!",
            "Hello London!"
        ]
    ```

* Düzenleme örnekleri tanımlı zaman aşımı içinde tamamlanamaz ve yanıt, [http API URL 'si bulma](durable-functions-http-api.md)bölümünde açıklanan varsayılan bir değer değildir:

    ```http
        HTTP/1.1 202 Accepted
        Content-Type: application/json; charset=utf-8
        Date: Thu, 14 Dec 2018 06:13:51 GMT
        Location: http://localhost:7071/runtime/webhooks/durabletask/instances/d3b72dddefce4e758d92f4d411567177?taskHub={taskHub}&connection={connection}&code={systemKey}
        Retry-After: 10
        Transfer-Encoding: chunked

        {
            "id": "d3b72dddefce4e758d92f4d411567177",
            "sendEventPostUri": "http://localhost:7071/runtime/webhooks/durabletask/instances/d3b72dddefce4e758d92f4d411567177/raiseEvent/{eventName}?taskHub={taskHub}&connection={connection}&code={systemKey}",
            "statusQueryGetUri": "http://localhost:7071/runtime/webhooks/durabletask/instances/d3b72dddefce4e758d92f4d411567177?taskHub={taskHub}&connection={connection}&code={systemKey}",
            "terminatePostUri": "http://localhost:7071/runtime/webhooks/durabletask/instances/d3b72dddefce4e758d92f4d411567177/terminate?reason={text}&taskHub={taskHub}&connection={connection}&code={systemKey}"
        }
    ```

> [!NOTE]
> Web kancası URL 'Lerinin biçimi, çalıştırdığınız Azure Işlevleri ana bilgisayarı sürümüne bağlı olarak farklılık gösterebilir. Yukarıdaki örnek, Azure Işlevleri 2,0 konağına yöneliktir.

## <a name="retrieve-http-management-webhook-urls"></a>HTTP yönetimi Web kancası URL 'Lerini alma

Bir düzenleme için olayları izlemek veya izlemek üzere bir dış sistem kullanabilirsiniz. Dış sistemler, [http API URL 'si bulma](durable-functions-http-features.md#http-api-url-discovery)bölümünde açıklanan varsayılan yanıtın parçası olan Web kancası URL 'leri aracılığıyla dayanıklı işlevler ile iletişim kurabilir. Web kancası URL 'Lerine alternatif olarak [Orchestration istemci bağlaması](durable-functions-bindings.md#orchestration-client)kullanılarak programlı bir şekilde erişilebilir. ( `CreateHttpManagementPayload` .Net) veya `createHttpManagementPayload` (JavaScript) yöntemleri, bu Web kancası URL 'lerini içeren bir seri hale getirilebilir nesne almak için kullanılabilir.

`CreateHttpManagementPayload` (.Net) ve `createHttpManagementPayload` (JavaScript) yöntemlerinde bir parametre vardır:

* **InstanceId**: ÖRNEĞIN benzersiz kimliği.

Yöntemler aşağıdaki dize özelliklerine sahip bir nesne döndürür:

* **ID**: Orchestration 'un örnek kimliği ( `InstanceId` girişle aynı olmalıdır).
* **Statusquerygeturi**: Orchestration örneğinin durum URL 'si.
* **SendEventPostUri**: Orchestration örneğinin "olay oluştur" URL 'si.
* **Terminateposturi**: Orchestration örneğinin "Terminate" URL 'si.
* **PurgeHistoryDeleteUri**: Orchestration örneğinin "Temizleme geçmişi" URL 'si.

İşlevler, aşağıdaki örneklerde gösterildiği gibi, ilgili düzenleyiclerdeki olayları izlemek veya yükseltmek için bu nesnelerin örneklerini Harici sistemlere gönderebilir:

# <a name="c"></a>[, #](#tab/csharp)

```csharp
[FunctionName("SendInstanceInfo")]
public static void SendInstanceInfo(
    [ActivityTrigger] IDurableActivityContext ctx,
    [DurableClient] IDurableOrchestrationClient client,
    [DocumentDB(
        databaseName: "MonitorDB",
        collectionName: "HttpManagementPayloads",
        ConnectionStringSetting = "CosmosDBConnection")]out dynamic document)
{
    HttpManagementPayload payload = client.CreateHttpManagementPayload(ctx.InstanceId);

    // send the payload to Cosmos DB
    document = new { Payload = payload, id = ctx.InstanceId };
}
```

> [!NOTE]
> Önceki C# kodu Dayanıklı İşlevler 2. x içindir. `DurableActivityContext` Dayanıklı işlevler 1. x için yerine kullanmanız gerekir `IDurableActivityContext`, özniteliği yerine `OrchestrationClient` `DurableClient` özniteliğini kullanmanız gerekir ve yerine `DurableOrchestrationClient` parametre türünü kullanmanız gerekir. `IDurableOrchestrationClient` Sürümler arasındaki farklılıklar hakkında daha fazla bilgi için [dayanıklı işlevler sürümler](durable-functions-versions.md) makalesine bakın.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

```javascript
const df = require("durable-functions");

modules.exports = async function(context, ctx) {
    const client = df.getClient(context);

    const payload = client.createHttpManagementPayload(ctx.instanceId);

    // send the payload to Cosmos DB
    context.bindings.document = JSON.stringify({
        id: ctx.instanceId,
        payload,
    });
};
```

Bkz. Function. JSON yapılandırması için [Başlangıç örnekleri](#javascript-function-json) .

---

## <a name="rewind-instances-preview"></a>Örnekleri geri sar (Önizleme)

Beklenmeyen bir nedenden dolayı düzenleme hatası varsa, bu amaçla derlenen bir API kullanarak örneği daha önceden sağlıklı bir duruma *geri sarmaya* dönüştürebilirsiniz.

> [!NOTE]
> Bu API, doğru hata işleme ve yeniden deneme ilkelerine yönelik bir değişiklik yapmak üzere tasarlanmamıştır. Bunun yerine, yalnızca düzenleme örneklerinin beklenmeyen nedenlerle başarısız olduğu durumlarda kullanılmak üzere tasarlanmıştır. Hata işleme ve yeniden deneme ilkeleri hakkında daha fazla bilgi için bkz. [hata işleme](durable-functions-error-handling.md) makalesi.

Orchestration `RewindAsync` [istemci bağlamasının](durable-functions-bindings.md#orchestration-client) (.net) `rewind` veya (JavaScript) yöntemini kullanarak düzenlemeyi *çalışır* duruma geri yerleştirebilirsiniz. Bu yöntem, düzenleme hatasına neden olan etkinliği veya alt düzenleme yürütme hatalarını da yeniden çalıştırır.

Örneğin, bir dizi [insan onaylarını](durable-functions-overview.md#human)içeren bir iş akışınız olduğunu varsayalım. Birinin onayını gerekli olduğunu bildiren ve gerçek zamanlı yanıtı bekleyen bir dizi etkinlik işlevi olduğunu varsayalım. Tüm onay etkinlikleri yanıt aldıktan veya zaman aşımına uğradıktan sonra, geçersiz bir veritabanı bağlantı dizesi gibi uygulamanın yanlış yapılandırılması nedeniyle başka bir etkinliğin başarısız olduğunu varsayalım. Sonuç, iş akışının derinlemesine bir düzenleme hatasıdır. `RewindAsync` (.Net) veya `rewind` (JavaScript) API 'si ile, bir uygulama Yöneticisi yapılandırma hatasını giderebilir ve başarısız düzenlemeyi hatadan hemen önce geri Sara geri sarabilirler. İnsan etkileşimi adımlarının hiçbirinin yeniden onaylanması gerekmez ve düzenleme artık başarıyla tamamlanabilir.

> [!NOTE]
> *Geri sarma* özelliği, dayanıklı zamanlayıcılar kullanan düzenleme örneklerinin yeniden sarlarını desteklemez.

# <a name="c"></a>[, #](#tab/csharp)

```csharp
[FunctionName("RewindInstance")]
public static Task Run(
    [DurableClient] IDurableOrchestrationClient client,
    [QueueTrigger("rewind-queue")] string instanceId)
{
    string reason = "Orchestrator failed and needs to be revived.";
    return client.RewindAsync(instanceId, reason);
}
```

> [!NOTE]
> Önceki C# kodu Dayanıklı İşlevler 2. x içindir. `OrchestrationClient` Dayanıklı işlevler 1. x için `DurableClient` özniteliği yerine özniteliği kullanmanız gerekir ve yerine `DurableOrchestrationClient` parametre türünü kullanmanız gerekir. `IDurableOrchestrationClient` Sürümler arasındaki farklılıklar hakkında daha fazla bilgi için [dayanıklı işlevler sürümler](durable-functions-versions.md) makalesine bakın.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

```javascript
const df = require("durable-functions");

module.exports = async function(context, instanceId) {
    const client = df.getClient(context);

    const reason = "Orchestrator failed and needs to be revived.";
    return client.rewind(instanceId, reason);
};
```

Bkz. Function. JSON yapılandırması için [Başlangıç örnekleri](#javascript-function-json) .

---

### <a name="azure-functions-core-tools"></a>Azure Functions Core Tools

Ayrıca, [Azure Functions Core Tools](../functions-run-local.md) `durable rewind` komutunu kullanarak bir düzenleme örneğini doğrudan geri sarın. Aşağıdaki parametreleri alır:

* (gerekli): Orchestration örneğinin kimliği. ** `id` **
* (isteğe bağlı): Orchestration örneğini yeniden sargı nedeni. ** `reason` **
* (isteğe bağlı): kullanılacak depolama bağlantı dizesini içeren uygulama ayarının adı. ** `connection-string-setting` ** Varsayılan değer: `AzureWebJobsStorage`.
* (isteğe bağlı): kullanılacak dayanıklı işlevler görev hub 'ının adı. ** `task-hub-name` ** Varsayılan olarak, [Host. JSON](durable-functions-bindings.md#host-json) dosyasındaki görev hub 'ı adı kullanılır.

```bash
func durable rewind --id 0ab8c55a66644d68a3a8b220b12d209c --reason "Orchestrator failed and needs to be revived."
```

## <a name="purge-instance-history"></a>Örnek geçmişini temizle

Bir düzenleme ile ilişkili tüm verileri kaldırmak için örnek geçmişini temizleyebilirsiniz. Örneğin, tamamlanmış bir örnekle ilişkili tüm Azure Tablo satırlarını ve büyük ileti bloblarını silmek isteyebilirsiniz. Bunu yapmak için [Orchestration istemci bağlamasının](durable-functions-bindings.md#orchestration-client) `PurgeInstanceHistoryAsync` (.net) veya `purgeInstanceHistory` (JavaScript) metodunu kullanın.

Bu yöntemin iki aşırı yüklemesi vardır. İlk aşırı yükleme, düzenleme örneğinin KIMLIĞINE göre geçmişi temizler:

# <a name="c"></a>[, #](#tab/csharp)

```csharp
[FunctionName("PurgeInstanceHistory")]
public static Task Run(
    [DurableClient] IDurableOrchestrationClient client,
    [QueueTrigger("purge-queue")] string instanceId)
{
    return client.PurgeInstanceHistoryAsync(instanceId);
}
```

# <a name="javascript"></a>[JavaScript](#tab/javascript)

```javascript
const df = require("durable-functions");

module.exports = async function(context, instanceId) {
    const client = df.getClient(context);
    return client.purgeInstanceHistory(instanceId);
};
```

Bkz. Function. JSON yapılandırması için [Başlangıç örnekleri](#javascript-function-json) .

---

Sonraki örnek, belirtilen zaman aralığından sonra tamamlanan tüm düzenleme örneklerinin geçmişini temizler bir Zamanlayıcı tarafından tetiklenen bir işlev gösterir. Bu durumda, 30 veya daha fazla gün önce tamamlanan tüm örnekler için verileri kaldırır. Her gün bir kez çalışmak üzere zamanlandı, 12:

# <a name="c"></a>[, #](#tab/csharp)

```csharp
[FunctionName("PurgeInstanceHistory")]
public static Task Run(
    [DurableClient] IDurableOrchestrationClient client,
    [TimerTrigger("0 0 12 * * *")]TimerInfo myTimer)
{
    return client.PurgeInstanceHistoryAsync(
        DateTime.MinValue,
        DateTime.UtcNow.AddDays(-30),  
        new List<OrchestrationStatus>
        {
            OrchestrationStatus.Completed
        });
}
```

> [!NOTE]
> Önceki C# kodu Dayanıklı İşlevler 2. x içindir. `OrchestrationClient` Dayanıklı işlevler 1. x için `DurableClient` özniteliği yerine özniteliği kullanmanız gerekir ve yerine `DurableOrchestrationClient` parametre türünü kullanmanız gerekir. `IDurableOrchestrationClient` Sürümler arasındaki farklılıklar hakkında daha fazla bilgi için [dayanıklı işlevler sürümler](durable-functions-versions.md) makalesine bakın.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Yöntemi `purgeInstanceHistoryBy` , birden çok örnek için örnek geçmişini koşullu olarak temizlemek üzere kullanılabilir.

**function. JSON**

```json
{
  "bindings": [
    {
      "schedule": "0 0 12 * * *",
      "name": "myTimer",
      "type": "timerTrigger",
      "direction": "in"
    },
    {
      "name": "starter",
      "type": "durableClient",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

> [!NOTE]
> Bu örnek, sürüm 2. x Dayanıklı İşlevler hedefler. Sürüm 1. x içinde yerine kullanın `orchestrationClient` `durableClient`.

**index. js**

```javascript
const df = require("durable-functions");

module.exports = async function (context, myTimer) {
    const client = df.getClient(context);
    const createdTimeFrom = new Date(0);
    const createdTimeTo = new Date().setDate(today.getDate() - 30);
    const runtimeStatuses = [ df.OrchestrationRuntimeStatus.Completed ];
    return client.purgeInstanceHistoryBy(createdTimeFrom, createdTimeTo, runtimeStatuses);
};
```

---

> [!NOTE]
> Temizleme geçmişi işleminin başarılı olması için, hedef örneğin çalışma zamanı durumu **tamamlanmalıdır**, **sonlandırılmış**veya **başarısız**olmalıdır.

### <a name="azure-functions-core-tools"></a>Azure Functions Core Tools

[Azure Functions Core Tools](../functions-run-local.md) `durable purge-history` komutunu kullanarak bir düzenleme örneğinin geçmişini temizleyebilirsiniz. Önceki bölümdeki ikinci C# örneğine benzer şekilde, belirtilen bir zaman aralığı boyunca oluşturulan tüm düzenleme örneklerinin geçmişini temizler. Temizlenen örnekleri çalışma zamanı durumuna göre daha fazla filtrelemek için. Komutun çeşitli parametreleri vardır:

* (isteğe bağlı): Bu tarih/saat sonra oluşturulan örneklerin geçmişini temizle (UTC). ** `created-after` ** ISO 8601 biçimlendirildi tarih saat kabul edildi.
* (isteğe bağlı): Bu tarih/saat (UTC) öncesinde oluşturulan örneklerin geçmişini temizle. ** `created-before` ** ISO 8601 biçimlendirildi tarih saat kabul edildi.
* (isteğe bağlı): belirli bir duruma sahip örnek geçmişini (örneğin, çalışıyor veya tamamlandı) temizleyin. ** `runtime-status` ** Birden çok (boşlukla ayrılmış) durum sağlayabilir.
* (isteğe bağlı): kullanılacak depolama bağlantı dizesini içeren uygulama ayarının adı. ** `connection-string-setting` ** Varsayılan değer: `AzureWebJobsStorage`.
* (isteğe bağlı): kullanılacak dayanıklı işlevler görev hub 'ının adı. ** `task-hub-name` ** Varsayılan olarak, [Host. JSON](durable-functions-bindings.md#host-json) dosyasındaki görev hub 'ı adı kullanılır.

Aşağıdaki komut 14 Kasım 2018, 7:35 PM (UTC) tarihinden önce oluşturulan tüm başarısız örneklerin geçmişini siler.

```bash
func durable purge-history --created-before 2018-11-14T19:35:00.0000000Z --runtime-status failed
```

## <a name="delete-a-task-hub"></a>Bir görev hub 'ını silme

[Azure Functions Core Tools](../functions-run-local.md) `durable delete-task-hub` komutunu kullanarak, Azure depolama tabloları, kuyrukları ve Blobları dahil olmak üzere belirli bir görev hub 'ı ile ilişkili tüm depolama yapılarını silebilirsiniz. Komutun iki parametresi vardır:

* (isteğe bağlı): kullanılacak depolama bağlantı dizesini içeren uygulama ayarının adı. ** `connection-string-setting` ** Varsayılan değer: `AzureWebJobsStorage`.
* (isteğe bağlı): kullanılacak dayanıklı işlevler görev hub 'ının adı. ** `task-hub-name` ** Varsayılan olarak, [Host. JSON](durable-functions-bindings.md#host-json) dosyasındaki görev hub 'ı adı kullanılır.

Aşağıdaki komut, `UserTest` görev hub 'ı ile Ilişkili tüm Azure depolama verilerini siler.

```bash
func durable delete-task-hub --task-hub-name UserTest
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Sürüm oluşturmayı nasıl ele alabileceğinizi öğrenin](durable-functions-versioning.md)

> [!div class="nextstepaction"]
> [Örnek yönetimi için yerleşik HTTP API başvurusu](durable-functions-http-api.md)
