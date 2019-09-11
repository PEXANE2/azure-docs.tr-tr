---
title: Dayanıklı İşlevler için bağlamalar-Azure
description: Azure Işlevleri için Dayanıklı İşlevler uzantısı için Tetikleyiciler ve bağlamaları kullanma.
services: functions
author: ggailey777
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: azfuncdf
ms.openlocfilehash: fbee98d64d37b2cdfc515eb733324902e238a768
ms.sourcegitcommit: 49c4b9c797c09c92632d7cedfec0ac1cf783631b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/05/2019
ms.locfileid: "70383100"
---
# <a name="bindings-for-durable-functions-azure-functions"></a>Dayanıklı İşlevler bağlamaları (Azure Işlevleri)

[Dayanıklı işlevler](durable-functions-overview.md) uzantısı, Orchestrator ve etkinlik işlevlerinin yürütülmesini denetleyen iki yeni tetikleyici bağlama sunar. Ayrıca, Dayanıklı İşlevler çalışma zamanı için istemci görevi gören bir çıkış bağlaması da sunar.

## <a name="orchestration-triggers"></a>Düzenleme Tetikleyicileri

Orchestration tetikleyicisi, dayanıklı Orchestrator işlevlerini yazmanıza olanak sağlar. Bu tetikleyici, yeni Orchestrator işlev örneklerinin başlamasını ve "bekleyen" bir görevi olan mevcut Orchestrator işlev örneklerinin devam ettirmeyi destekler.

Azure Işlevleri için Visual Studio Araçları 'nı kullandığınızda Orchestration tetikleyicisi, [Orchestrationtriggerattribute](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.OrchestrationTriggerAttribute.html) .net özniteliği kullanılarak yapılandırılır.

Komut dosyası dillerinde Orchestrator işlevlerini yazdığınızda (örneğin, JavaScript veya C# Scripting), düzenleme tetikleyicisi, `bindings` *function. JSON* dosyasının dizisinde aşağıdaki JSON nesnesi tarafından tanımlanır:

```json
{
    "name": "<Name of input parameter in function signature>",
    "orchestration": "<Optional - name of the orchestration>",
    "type": "orchestrationTrigger",
    "direction": "in"
}
```

* `orchestration`Orchestration 'un adıdır. Bu, bu Orchestrator işlevinin yeni örneklerini başlatmak istediklerinde istemcilerin kullanması gereken değerdir. Bu özellik isteğe bağlıdır. Belirtilmemişse, işlevin adı kullanılır.

Dahili olarak bu tetikleyici bağlama, işlev uygulaması için varsayılan depolama hesabındaki bir dizi kuyruğu yoklar. Bu kuyruklar, uzantının iç uygulama ayrıntılardır ve bu nedenle bağlama özelliklerinde açıkça yapılandırılmazlar.

### <a name="trigger-behavior"></a>Tetikleme davranışı

Düzenleme tetikleyicisiyle ilgili bazı notlar aşağıda verilmiştir:

* **Tek iş parçacığı** oluşturma-tek bir konak örneğindeki tüm Orchestrator işlev yürütmesi için tek bir dağıtıcı iş parçacığı kullanılır. Bu nedenle, Orchestrator işlev kodunun etkin olduğundan ve herhangi bir g/ç gerçekleştirmediğinden emin olmak önemlidir. Bu iş parçacığının Dayanıklı İşlevler özel görev türlerinde beklerken zaman uyumsuz bir iş gerektirmediğinden emin olmak da önemlidir.
* **Zehirli ileti işleme** -düzenleme tetikleyicilerinde bir zarar iletisi desteği yoktur.
* **İleti görünürlüğü** -düzenleme tetikleyici iletileri kuyruğa alınır ve yapılandırılabilir bir süre boyunca görünmez tutulur. İşlev uygulaması çalıştığı ve sağlıklı olduğu sürece bu iletilerin görünürlüğü otomatik olarak yenilenir.
* **Dönüş değerleri** -dönüş değerleri JSON olarak serileştirilir ve Azure Tablo depolama alanındaki Orchestration geçmişi tablosunda kalıcı hale getirilir. Bu dönüş değerleri, daha sonra açıklanan Orchestration istemci bağlaması tarafından sorgulanabilir.

> [!WARNING]
> Orchestrator işlevleri, düzenleme tetikleyicisi bağlaması dışında herhangi bir giriş veya çıkış bağlamasını asla kullanmamalıdır. Bunun yapılması, bu bağlamalar tek iş parçacığı ve g/ç kurallarına uymadığı için dayanıklı görev uzantısıyla ilgili sorunlara neden olabilir. Diğer bağlamaları kullanmak istiyorsanız, bunları Orchestrator işlevinizden çağrılan bir etkinlik işlevine ekleyin.

> [!WARNING]
> JavaScript Orchestrator işlevleri asla bildirilmelidir `async`.

### <a name="trigger-usage-net"></a>Kullanım tetiklemesi (.NET)

Düzenleme tetikleyicisi bağlaması hem giriş hem de çıkışları destekler. Giriş ve çıkış işleme hakkında daha fazla bilgi edinmek için aşağıdaki noktalara dikkat edin:

* **girişler** -.net Orchestration işlevleri parametre türü olarak yalnızca [Durableorchestrationcontext](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html) 'i destekler. Doğrudan işlev imzasında girişlerin serisini kaldırma desteklenmez. Kod, Orchestrator işlev girişlerini getirmek için [getınput\<T >](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_GetInput__1)( `getInput` .net) veya (JavaScript) metodunu kullanmalıdır. Bu girişler JSON serileştirilebilir türler olmalıdır.
* **çıktılar** -düzenleme Tetikleyicileri, çıkış değerlerinin yanı sıra girdileri de destekler. İşlevin dönüş değeri, çıkış değerini atamak için kullanılır ve JSON ile seri hale getirilebilir olmalıdır. Bir .NET işlevi veya `Task` `void`döndürürse, bir `null` değer çıktı olarak kaydedilir.

### <a name="trigger-sample"></a>Tetikleyici örneği

Aşağıda, en basit "Merhaba Dünya" Orchestrator işlevinin nasıl görünebileceğini bir örnek verilmiştir:

#### <a name="c"></a>C#

```csharp
[FunctionName("HelloWorld")]
public static string Run([OrchestrationTrigger] DurableOrchestrationContext context)
{
    string name = context.GetInput<string>();
    return $"Hello {name}!";
}
```

#### <a name="javascript-functions-2x-only"></a>JavaScript (yalnızca 2. x Işlevleri)

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context) {
    const name = context.df.getInput();
    return `Hello ${name}!`;
});
```

> [!NOTE]
> JavaScript içindeki nesne, durableorchestrationcontext, ancak [işlev bağlamını bir bütün olarak temsil etmez.](../functions-reference-node.md#context-object) `context` Düzenleme yöntemlerine `context` `df` nesnenin özelliği aracılığıyla erişebilirsiniz.

> [!NOTE]
> JavaScript düzenleyicilerinin kullanması `return`gerekir. `durable-functions` Kitaplık ,`context.done` yöntemi çağırma işlemini gerçekleştirir.

Çoğu Orchestrator işlevi etkinlik işlevlerini çağırır, bu nedenle bir etkinlik işlevinin nasıl çağrılacağını gösteren bir "Merhaba Dünya" örneği aşağıda verilmiştir:

#### <a name="c"></a>C#

```csharp
[FunctionName("HelloWorld")]
public static async Task<string> Run(
    [OrchestrationTrigger] DurableOrchestrationContext context)
{
    string name = context.GetInput<string>();
    string result = await context.CallActivityAsync<string>("SayHello", name);
    return result;
}
```

#### <a name="javascript-functions-2x-only"></a>JavaScript (yalnızca 2. x Işlevleri)

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context) {
    const name = context.df.getInput();
    const result = yield context.df.callActivity("SayHello", name);
    return result;
});
```

## <a name="activity-triggers"></a>Etkinlik Tetikleyicileri

Etkinlik tetikleyicisi, Orchestrator işlevleri tarafından çağrılan işlevleri yazmanıza olanak sağlar.

Visual Studio kullanıyorsanız, etkinlik tetikleyicisi [Activitytriggerattribute](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.ActivityTriggerAttribute.html) .net özniteliği kullanılarak yapılandırılır.

Geliştirme için vs Code veya Azure Portal kullanıyorsanız, etkinlik tetikleyicisi `bindings` *function. JSON*dizisinde aşağıdaki JSON nesnesi tarafından tanımlanır:

```json
{
    "name": "<Name of input parameter in function signature>",
    "activity": "<Optional - name of the activity>",
    "type": "activityTrigger",
    "direction": "in"
}
```

* `activity`etkinliğin adıdır. Bu, Orchestrator işlevlerinin bu etkinlik işlevini çağırmak için kullandığı değerdir. Bu özellik isteğe bağlıdır. Belirtilmemişse, işlevin adı kullanılır.

Dahili olarak bu tetikleyici bağlama, işlev uygulaması için varsayılan depolama hesabındaki bir kuyruğu yoklar. Bu kuyruk, uzantının iç uygulama ayrıntısı olduğundan, bağlama özelliklerinde açıkça yapılandırılmamış olabilir.

### <a name="trigger-behavior"></a>Tetikleme davranışı

Etkinlik tetikleyicisiyle ilgili bazı notlar aşağıda verilmiştir:

* **Iş parçacığı** oluşturma tetikleyicisinden farklı olarak, etkinlik tetikleyicilerinin iş parçacığı veya g/ç etrafında herhangi bir kısıtlama yoktur. Bunlar, normal işlevler gibi davranılanırlar.
* **Zehirli ileti işleme** -etkinlik tetikleyicilerinde bir zarar iletisi desteği yoktur.
* **İleti görünürlüğü** -etkinlik tetikleyicisi iletileri kuyruğa alınır ve yapılandırılabilir bir süre boyunca görünmez tutulur. İşlev uygulaması çalıştığı ve sağlıklı olduğu sürece bu iletilerin görünürlüğü otomatik olarak yenilenir.
* **Dönüş değerleri** -dönüş değerleri JSON olarak serileştirilir ve Azure Tablo depolama alanındaki Orchestration geçmişi tablosunda kalıcı hale getirilir.

> [!WARNING]
> Etkinlik işlevleri için depolama arka ucu, bir uygulama ayrıntısı ve Kullanıcı kodu bu depolama varlıklarıyla doğrudan etkileşmemelidir.

### <a name="trigger-usage-net"></a>Kullanım tetiklemesi (.NET)

Etkinlik tetikleyicisi bağlaması, tıpkı düzenleme tetikleyicisi gibi giriş ve çıkışları destekler. Giriş ve çıkış işleme hakkında daha fazla bilgi edinmek için aşağıdaki noktalara dikkat edin:

* **girişler** -.net etkinlik işlevleri, bir parametre türü olarak [durableactivitycontext](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableActivityContext.html) kullanımını yerel olarak kullanır. Alternatif olarak, bir etkinlik işlevi JSON ile seri hale getirilebilir olan herhangi bir parametre türüyle bildirilebilecek. Kullandığınızda `DurableActivityContext`, etkinlik işlevi girişini getirmek ve seri durumdan çıkarmak için [\<getınput T >](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableActivityContext.html#Microsoft_Azure_WebJobs_DurableActivityContext_GetInput__1) çağırabilirsiniz.
* **çıktılar** -etkinlik işlevleri, çıkış değerlerinin yanı sıra girdileri de destekler. İşlevin dönüş değeri, çıkış değerini atamak için kullanılır ve JSON ile seri hale getirilebilir olmalıdır. Bir .NET işlevi veya `Task` `void`döndürürse, bir `null` değer çıktı olarak kaydedilir.
* **meta veri** -.net etkinlik işlevleri, üst düzenleme `string instanceId` örnek kimliğini almak için bir parametreye bağlanabilir.

### <a name="trigger-sample"></a>Tetikleyici örneği

Aşağıda basit bir "Merhaba Dünya" etkinlik işlevinin nasıl görünebileceğini bir örnek verilmiştir:

#### <a name="c"></a>C#

```csharp
[FunctionName("SayHello")]
public static string SayHello([ActivityTrigger] DurableActivityContext helloContext)
{
    string name = helloContext.GetInput<string>();
    return $"Hello {name}!";
}
```

.Net `ActivityTriggerAttribute` bağlaması için varsayılan parametre türü ' dir `DurableActivityContext`. Ancak, .NET etkinliği Tetikleyicileri doğrudan JSON-serializlenebilir türlere (ilkel türler dahil) bağlamayı da destekler, bu nedenle aynı işlev aşağıdaki gibi basitleştirilebilir:

```csharp
[FunctionName("SayHello")]
public static string SayHello([ActivityTrigger] string name)
{
    return $"Hello {name}!";
}
```

#### <a name="javascript-functions-2x-only"></a>JavaScript (yalnızca 2. x Işlevleri)

```javascript
module.exports = async function(context) {
    return `Hello ${context.bindings.name}!`;
};
```

JavaScript bağlamaları ek parametreler olarak da geçirilebilir, bu nedenle aynı işlev şu şekilde basitleştirilebilir:

```javascript
module.exports = async function(context, name) {
    return `Hello ${name}!`;
};
```

### <a name="passing-multiple-parameters"></a>Birden çok parametre geçirme

Birden çok parametreyi doğrudan bir etkinlik işlevine geçirmek mümkün değildir. Bu durumda, bir dizi nesnenin veya .NET 'teki [Valuetuples](https://docs.microsoft.com/dotnet/csharp/tuples) nesnelerini kullanmanın önerisi.

Aşağıdaki örnek, [ C# 7](https://docs.microsoft.com/dotnet/csharp/whats-new/csharp-7#tuples)ile eklenen [valuetuples](https://docs.microsoft.com/dotnet/csharp/tuples) 'in yeni özelliklerini kullanıyor:

```csharp
[FunctionName("GetCourseRecommendations")]
public static async Task<dynamic> RunOrchestrator(
    [OrchestrationTrigger] DurableOrchestrationContext context)
{
    string major = "ComputerScience";
    int universityYear = context.GetInput<int>();

    dynamic courseRecommendations = await context.CallActivityAsync<dynamic>("CourseRecommendations", (major, universityYear));
    return courseRecommendations;
}

[FunctionName("CourseRecommendations")]
public static async Task<dynamic> Mapper([ActivityTrigger] DurableActivityContext inputs)
{
    // parse input for student's major and year in university
    (string Major, int UniversityYear) studentInfo = inputs.GetInput<(string, int)>();

    // retrieve and return course recommendations by major and university year
    return new {
        major = studentInfo.Major,
        universityYear = studentInfo.UniversityYear,
        recommendedCourses = new []
        {
            "Introduction to .NET Programming",
            "Introduction to Linux",
            "Becoming an Entrepreneur"
        }
    };
}
```

### <a name="using-input-and-output-bindings"></a>Giriş ve çıkış bağlamalarını kullanma

Etkinlik tetikleyicisi bağlamasının yanı sıra normal giriş ve çıkış bağlamalarını de kullanabilirsiniz. Örneğin, etkinlik Bağlamalarınızın girişini alabilir ve EventHub çıkış bağlamasını kullanarak bir EventHub 'e ileti gönderebilirsiniz:

```json
{
  "bindings": [
    {
      "name": "message",
      "type": "activityTrigger",
      "direction": "in"
    },
    {
      "type": "eventHub",
      "name": "outputEventHubMessage",
      "connection": "EventhubConnectionSetting",
      "eventHubName": "eh_messages",
      "direction": "out"
  }
  ]
}
```

```javascript
module.exports = async function (context) {
    context.bindings.outputEventHubMessage = context.bindings.message;
};
```

## <a name="orchestration-client"></a>Orchestration istemcisi

Orchestration istemci bağlaması, Orchestrator işlevleriyle etkileşime geçen işlevler yazmanızı sağlar. Örneğin, düzenleme örnekleri üzerinde aşağıdaki yollarla işlem yapabilirsiniz:

* Bunları başlatın.
* Durumlarını sorgulayın.
* Sonlandırın.
* Olayları çalışırken onlara gönderin.
* Örnek geçmişini temizle.

Visual Studio kullanıyorsanız, düzenleme istemcisine [Orchestrationclientattribute](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.OrchestrationClientAttribute.html) .net özniteliğini kullanarak bağlanabilirsiniz.

Geliştirme için betik dilleri (örn. *CSX* veya *. js* dosyaları) kullanıyorsanız, düzenleme tetikleyicisi `bindings` *function. JSON*dizisinde aşağıdaki JSON nesnesi tarafından tanımlanır:

```json
{
    "name": "<Name of input parameter in function signature>",
    "taskHub": "<Optional - name of the task hub>",
    "connectionName": "<Optional - name of the connection string app setting>",
    "type": "orchestrationClient",
    "direction": "in"
}
```

* `taskHub`-Birden çok işlevli uygulamanın aynı depolama hesabını paylaştığı ancak birbirinden yalıtılması gereken senaryolarda kullanılır. Belirtilmemişse, varsayılan değer `host.json` kullanılır. Bu değer, hedef Orchestrator işlevleri tarafından kullanılan değerle aynı olmalıdır.
* `connectionName`-Depolama hesabı bağlantı dizesi içeren bir uygulama ayarının adı. Bu bağlantı dizesi tarafından temsil edilen depolama hesabı, hedef Orchestrator işlevlerinin kullandığı aynı olmalıdır. Belirtilmemişse, işlev uygulaması için varsayılan depolama hesabı bağlantı dizesi kullanılır.

> [!NOTE]
> Çoğu durumda, bu özellikleri atlamanızı ve varsayılan davranışa güvenmenizi öneririz.

### <a name="client-usage"></a>İstemci kullanımı

.Net işlevlerinde, genellikle dayanıklı işlevler tarafından desteklenen tüm `DurableOrchestrationClient`istemci API 'lerine tam erişim sağlayan öğesine bağlayabilirsiniz. JavaScript 'te, aynı API 'ler tarafından `DurableOrchestrationClient` `getClient`döndürülen nesne tarafından gösterilir. İstemci nesnesindeki API 'Ler şunları içerir:

* [StartNewAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_StartNewAsync_)
* [GetStatusAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_GetStatusAsync_)
* [Sonlandırateasync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_TerminateAsync_)
* [RaiseEventAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_RaiseEventAsync_)
* [PurgeInstanceHistoryAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_PurgeInstanceHistoryAsync_) (Şu anda yalnızca .NET)

Alternatif olarak, .net işlevleri `IAsyncCollector<T>` [StartOrchestrationArgs](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.StartOrchestrationArgs.html) veya `JObject`olduğu yere `T` bağlanabilir.

Bu işlemlerle ilgili ek ayrıntılar için bkz. [Durableorchestrationclient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) API 'si belgeleri.

> [!WARNING]
> JavaScript 'te yerel olarak geliştirme yaparken, ortam değişkenini `WEBSITE_HOSTNAME` `localhost:<port>`, EX olarak ayarlamanız gerekir. `localhost:7071`üzerinde `DurableOrchestrationClient`yöntemleri kullanmak için. Bu gereksinim hakkında daha fazla bilgi için bkz. [GitHub sorunu](https://github.com/Azure/azure-functions-durable-js/issues/28).

### <a name="client-sample-visual-studio-development"></a>İstemci örneği (Visual Studio geliştirme)

Aşağıda, "HelloWorld" düzenlemesini başlatan örnek bir Queue-tetiklenen işlev verilmiştir.

```csharp
[FunctionName("QueueStart")]
public static Task Run(
    [QueueTrigger("durable-function-trigger")] string input,
    [OrchestrationClient] DurableOrchestrationClient starter)
{
    // Orchestration input comes from the queue message content.
    return starter.StartNewAsync("HelloWorld", input);
}
```

### <a name="client-sample-not-visual-studio"></a>İstemci örneği (Visual Studio değil)

Geliştirme için Visual Studio kullanmıyorsanız, aşağıdaki *function. JSON* dosyasını oluşturabilirsiniz. Bu örnek, dayanıklı düzenleme istemci bağlamasını kullanan bir Queue-tetiklenen işlevin nasıl yapılandırılacağını gösterir:

```json
{
  "bindings": [
    {
      "name": "input",
      "type": "queueTrigger",
      "queueName": "durable-function-trigger",
      "direction": "in"
    },
    {
      "name": "starter",
      "type": "orchestrationClient",
      "direction": "in"
    }
  ]
}
```

Yeni Orchestrator işlev örnekleri Başlatan dile özgü örnekler aşağıda verilmiştir.

#### <a name="c-sample"></a>C# örneği

Aşağıdaki örnek, bir C# betik işlevinden yeni bir işlev örneği başlatmak için dayanıklı düzenleme istemci bağlamasının nasıl kullanılacağını gösterir:

```csharp
#r "Microsoft.Azure.WebJobs.Extensions.DurableTask"

public static Task<string> Run(string input, DurableOrchestrationClient starter)
{
    return starter.StartNewAsync("HelloWorld", input);
}
```

#### <a name="javascript-sample"></a>JavaScript örneği

Aşağıdaki örnek, bir JavaScript işlevinden yeni bir işlev örneği başlatmak için dayanıklı düzenleme istemci bağlamasının nasıl kullanılacağını gösterir:

```javascript
const df = require("durable-functions");

module.exports = async function (context) {
    const client = df.getClient(context);
    return instanceId = await client.startNew("HelloWorld", undefined, context.bindings.input);
};
```

[Örnek yönetimi](durable-functions-instance-management.md)'nde, başlatma örnekleri hakkında daha fazla ayrıntı bulunabilir.

<a name="host-json"></a>

## <a name="hostjson-settings"></a>Host.JSON ayarları

[!INCLUDE [durabletask](../../../includes/functions-host-json-durabletask.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Checkişaret ve yeniden yürütme davranışları hakkında bilgi edinin](durable-functions-checkpointing-and-replay.md)
