---
title: Dayanıklı İşlevler örnekleri yönetme-Azure
description: Azure Işlevleri için Dayanıklı İşlevler uzantısı 'ndaki örnekleri yönetmeyi öğrenin.
services: functions
author: cgillum
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: azfuncdf
ms.openlocfilehash: 3db0cd3dd01e3f5f6af6b4b668d1ccac094624a2
ms.sourcegitcommit: 97605f3e7ff9b6f74e81f327edd19aefe79135d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70735168"
---
# <a name="manage-instances-in-durable-functions-in-azure"></a>Azure 'da Dayanıklı İşlevler örnekleri yönetme

Azure Işlevleri için [dayanıklı işlevler](durable-functions-overview.md) uzantısını kullanıyorsanız veya bunu yapmaya başlamak istiyorsanız, en iyi kullanım düzeyini kullandığınızdan emin olun. Nasıl yönetileceği hakkında daha fazla bilgi edinmek için Dayanıklı İşlevler düzenleme örneklerinizi iyileştirebilirsiniz. Bu makale her örnek yönetimi işleminin ayrıntılarına gider.

Örneğin, örnekleri başlatabilir ve sonlandırabilirsiniz, ancak tüm örnekleri ve sorgu örneklerini filtre ile sorgulama özelliği de dahil olmak üzere örnekleri sorgulayabilirsiniz. Ayrıca, örneklere olay gönderebilir, düzenleme tamamlanmasını bekleyebilir ve HTTP yönetimi Web kancası URL 'Lerini alabilirsiniz. Bu makalede, örnekleri geri sarma, örnek geçmişini temizleme ve bir görev merkezini silme dahil olmak üzere diğer yönetim işlemleri de ele alınmaktadır.

Dayanıklı İşlevler, bu yönetim işlemlerinin her birini nasıl uygulamak istediğinize ilişkin seçenekleriniz vardır. Bu makalede hem .NET (C#) hem de JavaScript için [Azure Functions Core Tools](../functions-run-local.md) kullanan örnekler sağlanmaktadır.

## <a name="start-instances"></a>Başlangıç örnekleri

Bir düzenleme örneğini başlamamak önemlidir. Bu, başka bir işlevin tetikleyicisinde Dayanıklı İşlevler bağlama kullandığınızda genellikle yapılır.

[Durableorchestrationclient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) (.net) `startNew` `DurableOrchestrationClient` veya (JavaScript) üzerindeki [startnewasync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_StartNewAsync_) yöntemi yeni bir örnek başlatır. `orchestrationClient` Bağlamayı kullanarak bu sınıfın örneklerini elde edersiniz. Bu yöntem, dahili olarak bir iletiyi denetim kuyruğuna sıraya alır, daha sonra `orchestrationTrigger` tetikleyici bağlamasını kullanan belirtilen ada sahip bir işlevin başlangıcını tetikler.

Bu zaman uyumsuz işlem, Orchestration işlemi başarıyla zamanlandığında tamamlanır. Düzenleme işlemi 30 saniye içinde başlamalıdır. Daha uzun sürerse, bir `TimeoutException`görürsünüz.

> [!WARNING]
> JavaScript 'te yerel olarak geliştirme yaparken, `WEBSITE_HOSTNAME` ortam değişkenini üzerinde `DurableOrchestrationClient`Yöntemler `localhost:<port>` kullanmak için (örneğin `localhost:7071`,) olarak ayarlayın. Bu gereksinim hakkında daha fazla bilgi için bkz. [GitHub sorunu](https://github.com/Azure/azure-functions-durable-js/issues/28).

### <a name="net"></a>.NET

[Startnewasync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_StartNewAsync_) parametreleri aşağıdaki gibidir:

* **Ad**: Zamanlanacak Orchestrator işlevinin adı.
* **Giriş**: Orchestrator işlevine giriş olarak geçirilmesi gereken tüm JSON seri hale getirilebilir verileri.
* **InstanceId**: Seçim Örneğin benzersiz KIMLIĞI. Bu parametreyi belirtmezseniz, yöntemi rastgele bir KIMLIK kullanır.

Bazı örnekler şunlardır:

### <a name="c"></a>C#

```csharp
[FunctionName("HelloWorldManualStart")]
public static async Task Run(
    [ManualTrigger] string input,
    [OrchestrationClient] DurableOrchestrationClient starter,
    ILogger log)
{
    string instanceId = await starter.StartNewAsync("HelloWorld", input);
    log.LogInformation($"Started orchestration with ID = '{instanceId}'.");
}
```

### <a name="javascript-functions-2x-only"></a>JavaScript (yalnızca 2. x Işlevleri)

Parametreleri `startNew` aşağıdaki gibidir:

* **Ad**: Zamanlanacak Orchestrator işlevinin adı.
* **InstanceId**: Seçim Örneğin benzersiz KIMLIĞI. Bu parametreyi belirtmezseniz, yöntemi rastgele bir KIMLIK kullanır.
* **Giriş**: Seçim Orchestrator işlevine giriş olarak geçirilmesi gereken tüm JSON seri hale getirilebilir verileri.

Basit bir JavaScript örneği aşağıda verilmiştir:

```javascript
const df = require("durable-functions");

module.exports = async function(context, input) {
    const client = df.getClient(context);

    const instanceId = await client.startNew("HelloWorld", undefined, input);
    context.log(`Started orchestration with ID = ${instanceId}.`);
};
```

> [!TIP]
> Örnek KIMLIĞI için rastgele bir tanımlayıcı kullanın. Bu, birden çok VM genelinde Orchestrator işlevlerini ölçeklendirirken eşit yük dağıtımına yardımcı olur. Rastgele olmayan örnek kimlikleri kullanmak için uygun zaman, KIMLIğIN bir dış kaynaktan gelmesi veya [Singleton Orchestrator](durable-functions-singletons.md) deseninin ne zaman uygulanacağı olur.

### <a name="azure-functions-core-tools"></a>Azure Functions Core Tools

Ayrıca, [Azure Functions Core Tools](../functions-run-local.md) `durable start-new` komutunu kullanarak doğrudan bir örnek başlatabilirsiniz. Aşağıdaki parametreleri alır:

* (gerekli): **`function-name`** Başlatılacak işlevin adı.
* (isteğe bağlı): **`input`** İşleve satır içi veya bir JSON dosyası aracılığıyla giriş. Dosyalar için, dosyasının yolunu içeren `@` `@path/to/file.json`bir ön eki ekleyin.
* (isteğe bağlı): **`id`** Orchestration örneğinin KIMLIĞI. Bu parametreyi belirtmezseniz, komut rastgele bir GUID kullanır.
* (isteğe bağlı): **`connection-string-setting`** Kullanılacak depolama bağlantı dizesini içeren uygulama ayarının adı. Varsayılan değer AzureWebJobsStorage ' dir.
* (isteğe bağlı): **`task-hub-name`** Kullanılacak Dayanıklı İşlevler görev hub 'ının adı. Varsayılan değer DurableFunctionsHub ' dır. Bunu, durableTask: HubName kullanarak [Host. JSON](durable-functions-bindings.md#host-json) içinde de ayarlayabilirsiniz.

> [!NOTE]
> Temel araçlar komutları, bunları bir işlev uygulamasının kök dizininden kullandığınızı varsayar. `connection-string-setting` Ve`task-hub-name` parametrelerini açık bir şekilde sağlarsanız, komutları herhangi bir dizinden çalıştırabilirsiniz. Bu komutları çalışan bir işlev uygulama konağı olmadan çalıştırabilmenize karşın, ana bilgisayar çalışmadığı takdirde bazı etkileri gözlemleyemiyorum olabilirsiniz. Örneğin, `start-new` komut bir başlangıç iletisini hedef görev merkezine sıralar, ancak iletiyi işleyemeyen bir işlev uygulama ana bilgisayarı işlemi olmadıkça düzenleme aslında çalışmaz.

Aşağıdaki komut HelloWorld adlı işlevi başlatır ve dosyanın `counter-data.json` içeriğini buna geçirir:

```bash
func durable start-new --function-name HelloWorld --input @counter-data.json --task-hub-name TestTaskHub
```

## <a name="query-instances"></a>Sorgu örnekleri

Düzenlemeleri yönetme çabalarınızın bir parçası olarak, büyük olasılıkla bir düzenleme örneğinin durumu hakkında bilgi toplamanız gerekir (örneğin, normal veya başarısız olup olmadığını).

[Durableorchestrationclient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) sınıfında (.net `getStatus` ) [getstatusasync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_GetStatusAsync_) yöntemi veya `DurableOrchestrationClient` sınıftaki yöntemi (JavaScript) bir Orchestration örneğinin durumunu sorgular.

Parametre olarak ( `instanceId` gerekli `showHistory` ) (isteğe bağlı) `showHistoryOutput` (isteğe bağlı) ve `showInput` (isteğe bağlı, yalnızca .net) alır.

* **`showHistory`** : Olarak `true`ayarlanırsa, yanıt yürütme geçmişini içerir.
* **`showHistoryOutput`** : Olarak `true`ayarlanırsa, yürütme geçmişi etkinlik çıkışları içerir.
* **`showInput`** : Olarak `false`ayarlanırsa, yanıt işlevin girişini içermez. Varsayılan değer `true` şeklindedir. (Yalnızca .NET)

Yöntemi, aşağıdaki özelliklere sahip bir JSON nesnesi döndürür:

* **Ad**: Orchestrator işlevinin adı.
* **InstanceId**: Orchestration örnek kimliği ( `instanceId` girişle aynı olmalıdır).
* **CreatedTime**: Orchestrator işlevinin çalışmaya başladığı zaman.
* **LastUpdatedTime**: Düzenleme son denetim noktası zaman.
* **Giriş**: İşlevin JSON değeri olarak girişi. Bu alan `showInput` , yanlışsa doldurulur.
* **Customstatus**: JSON biçiminde özel düzenleme durumu.
* **Çıkış**: İşlevin JSON değeri olarak çıktısı (işlev tamamlandıysa). Orchestrator işlevi başarısız olduysa, bu özellik hata ayrıntılarını içerir. Orchestrator işlevi sonlandırıldıysa, bu özellik sonlandırma sebebini (varsa) içerir.
* **RuntimeStatus**: Aşağıdaki değerlerden biri:
  * **Bekliyor**: Örnek zamanlandı ancak henüz çalışmaya başlamadı.
  * **Çalıştırma**: Örnek çalışmaya başladı.
  * **Tamamlandı**: Örnek normal şekilde tamamlandı.
  * **Devam Dasnew**: Örnek, yeni bir geçmiş ile kendisini yeniden başlattı. Bu geçici bir durumdur.
  * **Başarısız oldu**: Örnek bir hata vererek başarısız oldu.
  * **Sona erdirildi**: Örnek aniden durduruldu.
* **Geçmiş**: Orchestration yürütme geçmişi. Bu alan yalnızca, olarak `showHistory` `true`ayarlanmışsa doldurulur.

Bu yöntem, `null` örnek yoksa veya henüz çalışmaya başlamadıysa döndürür.

### <a name="c"></a>C#

```csharp
[FunctionName("GetStatus")]
public static async Task Run(
    [OrchestrationClient] DurableOrchestrationClient client,
    [ManualTrigger] string instanceId)
{
    var status = await client.GetStatusAsync(instanceId);
    // do something based on the current status.
}
```

### <a name="javascript-functions-2x-only"></a>JavaScript (yalnızca 2. x Işlevleri)

```javascript
const df = require("durable-functions");

module.exports = async function(context, instanceId) {
    const client = df.getClient(context);

    const status = await client.getStatus(instanceId);
    // do something based on the current status.
}
```

### <a name="azure-functions-core-tools"></a>Azure Functions Core Tools

Ayrıca, [Azure Functions Core Tools](../functions-run-local.md) `durable get-runtime-status` komutunu kullanarak bir düzenleme örneğinin durumunu doğrudan almak mümkündür. Aşağıdaki parametreleri alır:

* (gerekli): **`id`** Orchestration örneğinin KIMLIĞI.
* (isteğe bağlı): **`show-input`** Olarak `true`ayarlanırsa, yanıt işlevin girişini içerir. Varsayılan değer `false` şeklindedir.
* (isteğe bağlı): **`show-output`** Olarak `true`ayarlanırsa, yanıt işlevin çıktısını içerir. Varsayılan değer `false` şeklindedir.
* (isteğe bağlı): **`connection-string-setting`** Kullanılacak depolama bağlantı dizesini içeren uygulama ayarının adı. Varsayılan, `AzureWebJobsStorage` değeridir.
* (isteğe bağlı): **`task-hub-name`** Kullanılacak Dayanıklı İşlevler görev hub 'ının adı. Varsayılan, `DurableFunctionsHub` değeridir. Ayrıca, durableTask: HubName kullanılarak [Host. JSON](durable-functions-bindings.md#host-json)içinde de ayarlanabilir.

Aşağıdaki komut, 0ab8c55a66644d68a3a8b220b12d209c düzenleme örneği KIMLIĞINE sahip bir örnek için durumu (giriş ve çıkış dahil) alır. İşlev uygulamasının kök dizininden `func` komutunu çalıştırdığınız varsayılır:

```bash
func durable get-runtime-status --id 0ab8c55a66644d68a3a8b220b12d209c --show-input true --show-output true
```

Bir Orchestration örneğinin geçmişini `durable get-history` almak için komutunu kullanabilirsiniz. Aşağıdaki parametreleri alır:

* (gerekli): **`id`** Orchestration örneğinin KIMLIĞI.
* (isteğe bağlı): **`connection-string-setting`** Kullanılacak depolama bağlantı dizesini içeren uygulama ayarının adı. Varsayılan, `AzureWebJobsStorage` değeridir.
* (isteğe bağlı): **`task-hub-name`** Kullanılacak Dayanıklı İşlevler görev hub 'ının adı. Varsayılan, `DurableFunctionsHub` değeridir. Ayrıca, durableTask: HubName kullanılarak Host. JSON içinde de ayarlanabilir.

```bash
func durable get-history --id 0ab8c55a66644d68a3a8b220b12d209c
```

## <a name="query-all-instances"></a>Tüm örnekleri sorgula

Tek seferde düzenleme yaptığınız bir örneği sorgulamak yerine, bunların tümünü aynı anda sorgulamak daha verimli olabilir.

Tüm düzenleme örneklerinin durumlarını `GetStatusAsync` sorgulamak için (.net `getStatusAll` ) veya (JavaScript) yöntemini kullanabilirsiniz. .Net ' te, bir `CancellationToken` nesneyi iptal etmek istediğiniz durumda geçirebilirsiniz. Yöntemi, parametreleriyle aynı özelliklere `GetStatusAsync` sahip nesneleri döndürür.

### <a name="c"></a>C#

```csharp
[FunctionName("GetAllStatus")]
public static async Task Run(
    [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post")]HttpRequestMessage req,
    [OrchestrationClient] DurableOrchestrationClient client,
    ILogger log)
{
    IList<DurableOrchestrationStatus> instances = await client.GetStatusAsync(); // You can pass CancellationToken as a parameter.
    foreach (var instance in instances)
    {
        log.LogInformation(JsonConvert.SerializeObject(instance));
    };
}
```

### <a name="javascript-functions-2x-only"></a>JavaScript (yalnızca 2. x Işlevleri)

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

### <a name="azure-functions-core-tools"></a>Azure Functions Core Tools

Ayrıca, [Azure Functions Core Tools](../functions-run-local.md) `durable get-instances` komutu kullanılarak örnekleri doğrudan sorgulamak de mümkündür. Aşağıdaki parametreleri alır:

* (isteğe bağlı): **`top`** Bu komut sayfalamayı destekler. Bu parametre, istek başına alınan örnek sayısına karşılık gelir. Varsayılan değer 10 ' dur.
* (isteğe bağlı): **`continuation-token`** Hangi sayfa ya da örneklerin hangi bölüme veya bölümüne alınacağı belirten belirteç. Her `get-instances` yürütme, bir sonraki örnek kümesine belirteç döndürür.
* (isteğe bağlı): **`connection-string-setting`** Kullanılacak depolama bağlantı dizesini içeren uygulama ayarının adı. Varsayılan, `AzureWebJobsStorage` değeridir.
* (isteğe bağlı): **`task-hub-name`** Kullanılacak Dayanıklı İşlevler görev hub 'ının adı. Varsayılan, `DurableFunctionsHub` değeridir. Ayrıca, durableTask: HubName kullanılarak [Host. JSON](durable-functions-bindings.md#host-json)içinde de ayarlanabilir.

```bash
func durable get-instances
```

## <a name="query-instances-with-filters"></a>Filtreleri olan sorgu örnekleri

Standart örnek sorgusunun sağlayabilecekleri tüm bilgilere gerçekten ihtiyaç duymuyorsanız ne yapabilirsiniz? Örneğin, düzenleme oluşturma zamanını veya Orchestration çalışma zamanı durumunu yalnızca siz arıyorsanız ne olacak? Filtre uygulayarak sorgunuzu daraltabilirsiniz.

Önceden tanımlanmış bir filtre kümesiyle eşleşen `getStatusBy` düzenleme örneklerinin listesini almak için (.net)veya(JavaScript)metodunukullanın.`GetStatusAsync`

### <a name="c"></a>C#

```csharp
[FunctionName("QueryStatus")]
public static async Task Run(
    [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post")]HttpRequestMessage req,
    [OrchestrationClient] DurableOrchestrationClient client,
    ILogger log)
{
    IEnumerable<OrchestrationRuntimeStatus> runtimeStatus = new List<OrchestrationRuntimeStatus> {
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

### <a name="javascript-functions-2x-only"></a>JavaScript (yalnızca 2. x Işlevleri)

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

### <a name="azure-functions-core-tools"></a>Azure Functions Core Tools

Azure Functions Core Tools, `durable get-instances` komutu filtreler ile de kullanabilirsiniz. `top`Belirtilen `runtime-status``created-after` `created-before`, `continuation-token`, ,`connection-string-setting`ve parametrelerineekolarak,üçfiltreparametresi(,`task-hub-name` ve) kullanabilirsiniz.

* (isteğe bağlı): **`created-after`** Bu tarih/saat (UTC) sonrasında oluşturulan örnekleri alın. ISO 8601 biçimlendirildi tarih saat kabul edildi.
* (isteğe bağlı): **`created-before`** Bu tarih/saat (UTC) öncesinde oluşturulan örnekleri alın. ISO 8601 biçimlendirildi tarih saat kabul edildi.
* (isteğe bağlı): **`runtime-status`** Belirli bir duruma sahip örnekleri alın (örneğin, çalışıyor veya tamamlandı). Birden çok (boşlukla ayrılmış) durum sağlayabilir.
* (isteğe bağlı): **`top`** İstek başına alınan örnek sayısı. Varsayılan değer 10 ' dur.
* (isteğe bağlı): **`continuation-token`** Hangi sayfa ya da örneklerin hangi bölüme veya bölümüne alınacağı belirten belirteç. Her `get-instances` yürütme, bir sonraki örnek kümesine belirteç döndürür.
* (isteğe bağlı): **`connection-string-setting`** Kullanılacak depolama bağlantı dizesini içeren uygulama ayarının adı. Varsayılan, `AzureWebJobsStorage` değeridir.
* (isteğe bağlı): **`task-hub-name`** Kullanılacak Dayanıklı İşlevler görev hub 'ının adı. Varsayılan, `DurableFunctionsHub` değeridir. Ayrıca, durableTask: HubName kullanılarak [Host. JSON](durable-functions-bindings.md#host-json)içinde de ayarlanabilir.

Herhangi bir filtre`created-after`(, `created-before`, veya `runtime-status`) sağlamazsanız, komut çalışma zamanı durumu veya oluşturulma `top` zamanına göre yalnızca örnekleri alır.

```bash
func durable get-instances --created-after 2018-03-10T13:57:31Z --created-before  2018-03-10T23:59Z --top 15
```

## <a name="terminate-instances"></a>Örnekleri Sonlandır

Çalışması çok uzun süren bir düzenleme örneğiniz varsa veya herhangi bir nedenle tamamlanmadan önce bunu durdurmanız gerekiyorsa, bunu sonlandırma seçeneğine sahipsiniz.

[Durableorchestrationclient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) sınıfının (.net) veya `terminate` `DurableOrchestrationClient` sınıfın yönteminin (JavaScript) [sonlandırateasync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_TerminateAsync_) yöntemini kullanabilirsiniz. İki parametre, günlüklere ve `instanceId` örnek durumuna `reason` yazılan bir ve bir dizedir. Sonlandırılan bir örnek, bir `await` sonraki (.net) veya `yield` (JavaScript) noktasına ulaştığında çalışmayı durdurur veya zaten bir `await` veya `yield`varsa hemen sonlanır.

### <a name="c"></a>C#

```csharp
[FunctionName("TerminateInstance")]
public static Task Run(
    [OrchestrationClient] DurableOrchestrationClient client,
    [ManualTrigger] string instanceId)
{
    string reason = "It was time to be done.";
    return client.TerminateAsync(instanceId, reason);
}
```

### <a name="javascript-functions-2x-only"></a>JavaScript (yalnızca 2. x Işlevleri)

```javascript
const df = require("durable-functions");

module.exports = async function(context, instanceId) {
    const client = df.getClient(context);

    const reason = "It was time to be done.";
    return client.terminate(instanceId, reason);
};
```

> [!NOTE]
> Örnek sonlandırma Şu anda yaymıyor. Etkinlik işlevleri ve alt düzenlemeler, onları çağıran düzenleme örneğini sonlandırıp sonlandırmadığına bakılmaksızın tamamlama için çalışır.

### <a name="azure-functions-core-tools"></a>Azure Functions Core Tools

Ayrıca, [Azure Functions Core Tools](../functions-run-local.md) `durable terminate` komutunu kullanarak bir düzenleme örneğini doğrudan sonlandırabilirsiniz. Aşağıdaki parametreleri alır:

* (gerekli): **`id`** Sonlandırılacak düzenleme örneğinin KIMLIĞI.
* (isteğe bağlı): **`reason`** Sonlandırma nedeni.
* (isteğe bağlı): **`connection-string-setting`** Kullanılacak depolama bağlantı dizesini içeren uygulama ayarının adı. Varsayılan, `AzureWebJobsStorage` değeridir.
* (isteğe bağlı): **`task-hub-name`** Kullanılacak Dayanıklı İşlevler görev hub 'ının adı. Varsayılan, `DurableFunctionsHub` değeridir. Ayrıca, durableTask: HubName kullanılarak [Host. JSON](durable-functions-bindings.md#host-json)içinde de ayarlanabilir.

Aşağıdaki komut, 0ab8c55a66644d68a3a8b220b12d209c KIMLIKLI bir Orchestration örneğini sonlandırır:

```bash
func durable terminate --id 0ab8c55a66644d68a3a8b220b12d209c --reason "It was time to be done."
```

## <a name="send-events-to-instances"></a>Olayları örneklere gönder

Bazı senaryolarda, Orchestrator işlevlerinin dış olayları bekleyip dinleyebilmesi önemlidir. Bu, [insan etkileşimi](durable-functions-concepts.md#human)için bekleyen [izleme işlevlerini](durable-functions-concepts.md#monitoring) ve işlevlerini içerir.

[Durableorchestrationclient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) sınıfının (.net `raiseEvent` ) veya `DurableOrchestrationClient` sınıfın yönteminin (JavaScript) [RaiseEventAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_RaiseEventAsync_) yöntemini kullanarak çalışan örneklere olay bildirimleri gönderin. Bu olayları işleyebilen örnekler, [WaitForExternalEvent](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_WaitForExternalEvent_) (.net) veya `waitForExternalEvent` (JavaScript) çağrısını bekleyen olanlardır.

[RaiseEventAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_RaiseEventAsync_) (.net) ve `raiseEvent` (JavaScript) parametreleri aşağıdaki gibidir:

* **InstanceId**: Örneğin benzersiz KIMLIĞI.
* **EventName**: Gönderecek etkinliğin adı.
* **Eventdata**: Örneğe gönderilmek üzere JSON seri hale getirilebilir yük.

### <a name="c"></a>C#

```csharp
[FunctionName("RaiseEvent")]
public static Task Run(
    [OrchestrationClient] DurableOrchestrationClient client,
    [ManualTrigger] string instanceId)
{
    int[] eventData = new int[] { 1, 2, 3 };
    return client.RaiseEventAsync(instanceId, "MyEvent", eventData);
}
```

### <a name="javascript-functions-2x-only"></a>JavaScript (yalnızca 2. x Işlevleri)

```javascript
const df = require("durable-functions");

module.exports = async function(context, instanceId) {
    const client = df.getClient(context);

    const eventData = [ 1, 2, 3 ];
    return client.raiseEvent(instanceId, "MyEvent", eventData);
};
```

> [!IMPORTANT]
> Belirtilen örnek KIMLIĞINE sahip bir Orchestration örneği yoksa veya örnek belirtilen olay adını beklemediği takdirde, olay iletisi atılır. Bu davranış hakkında daha fazla bilgi için bkz. [GitHub sorunu](https://github.com/Azure/azure-functions-durable-extension/issues/29).

### <a name="azure-functions-core-tools"></a>Azure Functions Core Tools

Ayrıca, [Azure Functions Core Tools](../functions-run-local.md) `durable raise-event` komutunu kullanarak bir düzenleme örneğine doğrudan bir olay da yükseltebilirsiniz. Aşağıdaki parametreleri alır:

* (gerekli): **`id`** Orchestration örneğinin KIMLIĞI.
* (isteğe bağlı): **`event-name`** Tetikedilecek etkinliğin adı. Varsayılan, `$"Event_{RandomGUID}"` değeridir.
* (isteğe bağlı): **`event-data`** Orchestration örneğine gönderilen veriler. Bu bir JSON dosyasının yolu olabilir veya verileri doğrudan komut satırına sağlayabilirsiniz.
* (isteğe bağlı): **`connection-string-setting`** Kullanılacak depolama bağlantı dizesini içeren uygulama ayarının adı. Varsayılan, `AzureWebJobsStorage` değeridir.
* (isteğe bağlı): **`task-hub-name`** Kullanılacak Dayanıklı İşlevler görev hub 'ının adı. Varsayılan, `DurableFunctionsHub` değeridir. Ayrıca, durableTask: HubName kullanılarak [Host. JSON](durable-functions-bindings.md#host-json)içinde de ayarlanabilir.

```bash
func durable raise-event --id 0ab8c55a66644d68a3a8b220b12d209c --event-name MyEvent --event-data @eventdata.json
```

```bash
func durable raise-event --id 1234567 --event-name MyOtherEvent --event-data 3
```

## <a name="wait-for-orchestration-completion"></a>Düzenleme tamamlanmasını bekle

Uzun süre çalışan düzenlemeler içinde, bir düzenleme işleminin sonuçlarını beklemek ve almak isteyebilirsiniz. Bu gibi durumlarda, Orchestration üzerinde bir zaman aşımı süresi tanımlayabilmek de yararlı olur. Zaman aşımı aşılırsa, sonuçlar yerine Orchestration 'un durumu döndürülmelidir.

[Durableorchestrationclient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) sınıfı .net 'Te bir [Waitforcompletionorcreatecheckstatusresponseasync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_WaitForCompletionOrCreateCheckStatusResponseAsync_) API 'si sunar. Bu API 'yi, bir Orchestration örneğinden zaman uyumlu olarak gerçek çıktıyı almak için kullanabilirsiniz. JavaScript 'te, `DurableOrchestrationClient` sınıfı aynı amaçla bir `waitForCompletionOrCreateCheckStatusResponse` API sunar. Ayarlanmayan Yöntemler, için varsayılan değer olan 10 saniye `timeout`ve için `retryInterval`1 saniye kullanır.  

Bu API 'nin nasıl kullanılacağını gösteren örnek bir HTTP tetikleyici işlevi aşağıda verilmiştir:

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/HttpSyncStart.cs)]

[!code-javascript[Main](~/samples-durable-functions/samples/javascript/HttpSyncStart/index.js)]

Aşağıdaki satırı kullanarak işlevi çağırın. Zaman aşımı için 2 saniye ve yeniden deneme aralığı için 0,5 saniye kullanın:

```bash
    http POST http://localhost:7071/orchestrators/E1_HelloSequence/wait?timeout=2&retryInterval=0.5
```

Orchestration örneğinden yanıt almak için gereken zamana bağlı olarak iki durum vardır:

* Düzenleme örnekleri tanımlı zaman aşımı (Bu durumda 2 saniye) içinde tamamlanır ve yanıt, zaman uyumlu olarak teslim edilen gerçek düzenleme örneği çıktıdır:

    ```http
        HTTP/1.1 200 OK
        Content-Type: application/json; charset=utf-8
        Date: Thu, 14 Dec 2017 06:14:29 GMT
        Server: Microsoft-HTTPAPI/2.0
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
        Date: Thu, 14 Dec 2017 06:13:51 GMT
        Location: http://localhost:7071/admin/extensions/DurableTaskExtension/instances/d3b72dddefce4e758d92f4d411567177?taskHub={taskHub}&connection={connection}&code={systemKey}
        Retry-After: 10
        Server: Microsoft-HTTPAPI/2.0
        Transfer-Encoding: chunked

        {
            "id": "d3b72dddefce4e758d92f4d411567177",
            "sendEventPostUri": "http://localhost:7071/admin/extensions/DurableTaskExtension/instances/d3b72dddefce4e758d92f4d411567177/raiseEvent/{eventName}?taskHub={taskHub}&connection={connection}&code={systemKey}",
            "statusQueryGetUri": "http://localhost:7071/admin/extensions/DurableTaskExtension/instances/d3b72dddefce4e758d92f4d411567177?taskHub={taskHub}&connection={connection}&code={systemKey}",
            "terminatePostUri": "http://localhost:7071/admin/extensions/DurableTaskExtension/instances/d3b72dddefce4e758d92f4d411567177/terminate?reason={text}&taskHub={taskHub}&connection={connection}&code={systemKey}",
            "rewindPostUri": "https://localhost:7071/admin/extensions/DurableTaskExtension/instances/d3b72dddefce4e758d92f4d411567177/rewind?reason={text}&taskHub={taskHub}&connection={connection}&code={systemKey}"
        }
    ```

> [!NOTE]
> Web kancası URL 'Lerinin biçimi, çalıştırdığınız Azure Işlevleri ana bilgisayarı sürümüne bağlı olarak farklılık gösterebilir. Yukarıdaki örnek, Azure Işlevleri 2. x ana bilgisayarı içindir.

## <a name="retrieve-http-management-webhook-urls"></a>HTTP yönetimi Web kancası URL 'Lerini alma

Bir düzenleme için olayları izlemek veya yükseltmek üzere bir dış sistem kullanabilirsiniz. Dış sistemler, [http API URL 'si bulma](durable-functions-http-api.md)bölümünde açıklanan varsayılan yanıtın parçası olan Web kancası URL 'leri aracılığıyla dayanıklı işlevler ile iletişim kurabilir. Ancak, Web kancası URL 'Lerine Ayrıca Orchestration istemcisinde veya bir etkinlik işlevinde programlı olarak erişilebilir. Bunu, [durableorchestrationclient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) sınıfının (.net) `createHttpManagementPayload` [createhttpmanagementpayload](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_CreateHttpManagementPayload_) yöntemini veya `DurableOrchestrationClient` sınıfının yöntemini (JavaScript) kullanarak yapın.

[Createhttpmanagementpayload](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_CreateHttpManagementPayload_) ve `createHttpManagementPayload` bir parametreye sahip:

* **InstanceId**: Örneğin benzersiz KIMLIĞI.

Yöntemler, aşağıdaki dize özellikleriyle bir [Httpmanagementpayload](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.Extensions.DurableTask.HttpManagementPayload.html#Microsoft_Azure_WebJobs_Extensions_DurableTask_HttpManagementPayload_) (.net) veya bir nesne (JavaScript) örneği döndürür:

* **Kimliği**: Orchestration örnek kimliği ( `InstanceId` girişle aynı olmalıdır).
* **Statusquerygeturi**: Orchestration örneğinin durum URL 'SI.
* **SendEventPostUri**: Orchestration örneğinin "olay oluştur" URL 'SI.
* **Terminateposturi**: Orchestration örneğinin "Terminate" URL 'SI.
* **Rewınwınposturi**: Orchestration örneğinin "geri sarma" URL 'SI.

Etkinlik işlevleri, bir düzenleme için olayları izlemek veya yükseltmek üzere bu nesnelerin bir örneğini dış sistemlere gönderebilir:

### <a name="c"></a>C#

```csharp
[FunctionName("SendInstanceInfo")]
public static void SendInstanceInfo(
    [ActivityTrigger] DurableActivityContext ctx,
    [OrchestrationClient] DurableOrchestrationClient client,
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

### <a name="javascript-functions-2x-only"></a>JavaScript (yalnızca 2. x Işlevleri)

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

## <a name="rewind-instances-preview"></a>Örnekleri geri sar (Önizleme)

Beklenmeyen bir nedenden dolayı düzenleme hatası varsa, bu amaçla derlenen bir API kullanarak örneği daha önceden sağlıklı bir duruma *geri sarmaya* dönüştürebilirsiniz.

> [!NOTE]
> Bu API, doğru hata işleme ve yeniden deneme ilkelerine yönelik bir değişiklik yapmak üzere tasarlanmamıştır. Bunun yerine, yalnızca düzenleme örneklerinin beklenmeyen nedenlerle başarısız olduğu durumlarda kullanılmak üzere tasarlanmıştır. Hata işleme ve yeniden deneme ilkeleri hakkında daha fazla bilgi için bkz. [hata işleme](durable-functions-error-handling.md) konusu.

Düzenlemeyi *çalışma* durumuna geri koymak için [rewindadsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_RewindAsync_System_String_System_String_) (.net) veya `rewindAsync` (JavaScript) API 'sini kullanın. Düzenleme hatasına neden olan etkinliği veya alt düzenleme yürütme hatalarını yeniden çalıştırın.

Örneğin, bir dizi [insan onaylarını](durable-functions-concepts.md#human)içeren bir iş akışınız olduğunu varsayalım. Birinin onayını gerekli olduğunu bildiren ve gerçek zamanlı yanıtı bekleyen bir dizi etkinlik işlevi olduğunu varsayalım. Tüm onay etkinlikleri yanıt aldıktan veya zaman aşımına uğradıktan sonra, geçersiz bir veritabanı bağlantı dizesi gibi uygulamanın yanlış yapılandırılması nedeniyle başka bir etkinliğin başarısız olduğunu varsayalım. Sonuç, iş akışının derinlemesine bir düzenleme hatasıdır. (.Net) veya `rewindAsync` (JavaScript) API 'si ile, bir uygulama Yöneticisi yapılandırma hatasını giderebilir ve başarısız düzenlemeyi hatadan hemen önce geri Sara geri sarabilirler. `RewindAsync` İnsan etkileşimi adımlarının hiçbirinin yeniden onaylanması gerekmez ve düzenleme artık başarıyla tamamlanabilir.

> [!NOTE]
> *Geri sarma* özelliği, dayanıklı zamanlayıcılar kullanan düzenleme örneklerinin yeniden sarlarını desteklemez.

### <a name="c"></a>C#

```csharp
[FunctionName("RewindInstance")]
public static Task Run(
    [OrchestrationClient] DurableOrchestrationClient client,
    [ManualTrigger] string instanceId)
{
    string reason = "Orchestrator failed and needs to be revived.";
    return client.RewindAsync(instanceId, reason);
}
```

### <a name="javascript-functions-2x-only"></a>JavaScript (yalnızca 2. x Işlevleri)

```javascript
const df = require("durable-functions");

module.exports = async function(context, instanceId) {
    const client = df.getClient(context);

    const reason = "Orchestrator failed and needs to be revived.";
    return client.rewind(instanceId, reason);
};
```

### <a name="azure-functions-core-tools"></a>Azure Functions Core Tools

Ayrıca, [Azure Functions Core Tools](../functions-run-local.md) `durable rewind` komutunu kullanarak bir düzenleme örneğini doğrudan geri sarın. Aşağıdaki parametreleri alır:

* (gerekli): **`id`** Orchestration örneğinin KIMLIĞI.
* (isteğe bağlı): **`reason`** Orchestration örneğini yeniden sargı nedeni.
* (isteğe bağlı): **`connection-string-setting`** Kullanılacak depolama bağlantı dizesini içeren uygulama ayarının adı. Varsayılan, `AzureWebJobsStorage` değeridir.
* (isteğe bağlı): **`task-hub-name`** Kullanılacak Dayanıklı İşlevler görev hub 'ının adı. Varsayılan, `DurableFunctionsHub` değeridir. Ayrıca, durableTask: HubName kullanılarak [Host. JSON](durable-functions-bindings.md#host-json)içinde de ayarlanabilir.

```bash
func durable rewind --id 0ab8c55a66644d68a3a8b220b12d209c --reason "Orchestrator failed and needs to be revived."
```

## <a name="purge-instance-history"></a>Örnek geçmişini temizle

Bir düzenleme ile ilişkili tüm verileri kaldırmak için örnek geçmişini temizleyebilirsiniz. Örneğin, varsa, Azure Tablo satırlarından ve büyük ileti Bloblarından kurtul etmek isteyebilirsiniz. Bunu yapmak için [PurgeInstanceHistoryAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_PurgeInstanceHistoryAsync_) API 'sini kullanın.

> [!NOTE]
> API Şu anda yalnızca için C#kullanılabilir. `PurgeInstanceHistoryAsync`

 Yöntemin iki aşırı yüklemesi vardır. İlk bir, düzenleme örneğinin KIMLIĞINE göre geçmişi temizler:

### <a name="c"></a>C#

```csharp
[FunctionName("PurgeInstanceHistory")]
public static Task Run(
    [OrchestrationClient] DurableOrchestrationClient client,
    [ManualTrigger] string instanceId)
{
    return client.PurgeInstanceHistoryAsync(instanceId);
}
```

İkinci örnek, belirtilen zaman aralığından sonra tamamlanan tüm düzenleme örneklerinin geçmişini temizler bir Zamanlayıcı tarafından tetiklenen bir işlev gösterir. Bu durumda, 30 veya daha fazla gün önce tamamlanan tüm örnekler için verileri kaldırır. Her gün bir kez çalışmak üzere zamanlandı, 12:

### <a name="c"></a>C#

```csharp
[FunctionName("PurgeInstanceHistory")]
public static Task Run(
    [OrchestrationClient] DurableOrchestrationClient client,
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
> Zaman tetiklenen işlev işleminin başarılı olması için, çalışma zamanı durumunun **tamamlanması**, **sonlandırılması**veya **başarısız**olması gerekir.

### <a name="azure-functions-core-tools"></a>Azure Functions Core Tools

[](../functions-run-local.md) AzureFunctionsCoreTools`durable purge-history` komutunu kullanarak bir düzenleme örneğinin geçmişini temizleyebilirsiniz. Önceki bölümdeki ikinci C# örneğe benzer şekilde, belirtilen zaman aralığı boyunca oluşturulan tüm düzenleme örneklerinin geçmişini temizler. Temizlenen örnekleri çalışma zamanı durumuna göre daha fazla filtrelemek için. Komutun çeşitli parametreleri vardır:

* (isteğe bağlı): **`created-after`** Bu tarih/saat (UTC) sonrasında oluşturulan örneklerin geçmişini temizle. ISO 8601 biçimlendirildi tarih saat kabul edildi.
* (isteğe bağlı): **`created-before`** Bu tarih/saat (UTC) öncesinde oluşturulan örneklerin geçmişini temizle. ISO 8601 biçimlendirildi tarih saat kabul edildi.
* (isteğe bağlı): **`runtime-status`** Örnek geçmişini belirli bir durum (örneğin, çalışıyor veya tamamlandı) olarak temizleyin. Birden çok (boşlukla ayrılmış) durum sağlayabilir.
* (isteğe bağlı): **`connection-string-setting`** Kullanılacak depolama bağlantı dizesini içeren uygulama ayarının adı. Varsayılan, `AzureWebJobsStorage` değeridir.
* (isteğe bağlı): **`task-hub-name`** Kullanılacak Dayanıklı İşlevler görev hub 'ının adı. Varsayılan, `DurableFunctionsHub` değeridir. Ayrıca, durableTask: HubName kullanılarak [Host. JSON](durable-functions-bindings.md#host-json)içinde de ayarlanabilir.

Aşağıdaki komut 14 Kasım 2018, 7:35 PM (UTC) tarihinden önce oluşturulan tüm başarısız örneklerin geçmişini siler.

```bash
func durable purge-history --created-before 2018-11-14T19:35:00.0000000Z --runtime-status failed
```

## <a name="delete-a-task-hub"></a>Bir görev hub 'ını silme

[](../functions-run-local.md) AzureFunctionsCoreTools`durable delete-task-hub` komutunu kullanarak, belirli bir görev hub 'ı ile ilişkili tüm depolama yapıtları silebilirsiniz. Buna Azure depolama tabloları, kuyrukları ve Blobları dahildir. Komutun iki parametresi vardır:

* (isteğe bağlı): **`connection-string-setting`** Kullanılacak depolama bağlantı dizesini içeren uygulama ayarının adı. Varsayılan, `AzureWebJobsStorage` değeridir.
* (isteğe bağlı): **`task-hub-name`** Kullanılacak Dayanıklı İşlevler görev hub 'ının adı. Varsayılan, `DurableFunctionsHub` değeridir. Ayrıca, durableTask: HubName kullanılarak [Host. JSON](durable-functions-bindings.md#host-json)içinde de ayarlanabilir.

Aşağıdaki komut, `UserTest` görev hub 'ı ile ilişkili tüm Azure depolama verilerini siler.

```bash
func durable delete-task-hub --task-hub-name UserTest
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Örnek yönetimi için HTTP API 'Lerini nasıl kullanacağınızı öğrenin](durable-functions-http-api.md)