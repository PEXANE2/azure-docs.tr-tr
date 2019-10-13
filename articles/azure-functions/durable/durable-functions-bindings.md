---
title: Dayanıklı İşlevler için bağlamalar-Azure
description: Azure Işlevleri için Dayanıklı İşlevler uzantısı için Tetikleyiciler ve bağlamaları kullanma.
services: functions
author: ggailey777
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.topic: conceptual
ms.date: 09/04/2019
ms.author: azfuncdf
ms.openlocfilehash: 1b056ce8afe86fcd6629aff23ac95acae02ed9ba
ms.sourcegitcommit: 8b44498b922f7d7d34e4de7189b3ad5a9ba1488b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/13/2019
ms.locfileid: "72299871"
---
# <a name="bindings-for-durable-functions-azure-functions"></a>Dayanıklı İşlevler bağlamaları (Azure Işlevleri)

[Dayanıklı işlevler](durable-functions-overview.md) uzantısı, Orchestrator ve etkinlik işlevlerinin yürütülmesini denetleyen iki yeni tetikleyici bağlama sunar. Ayrıca, Dayanıklı İşlevler çalışma zamanı için istemci görevi gören bir çıkış bağlaması da sunar.

## <a name="orchestration-trigger"></a>Düzenleme tetikleyicisi

Orchestration tetikleyicisi, [dayanıklı Orchestrator işlevlerini](durable-functions-types-features-overview.md#orchestrator-functions)yazmanıza olanak sağlar. Bu tetikleyici, yeni Orchestrator işlev örneklerinin başlamasını ve "bekleyen" bir görevi olan mevcut Orchestrator işlev örneklerinin devam ettirmeyi destekler.

Azure Işlevleri için Visual Studio Araçları 'nı kullandığınızda Orchestration tetikleyicisi, [Orchestrationtriggerattribute](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.OrchestrationTriggerAttribute.html) .net özniteliği kullanılarak yapılandırılır.

Komut dosyası dillerinde Orchestrator işlevlerini yazdığınızda (örneğin, JavaScript veya C# Scripting), düzenleme tetikleyicisi, *function. JSON* dosyasının `bindings` dizisinde aşağıdaki JSON nesnesi tarafından tanımlanır:

```json
{
    "name": "<Name of input parameter in function signature>",
    "orchestration": "<Optional - name of the orchestration>",
    "type": "orchestrationTrigger",
    "direction": "in"
}
```

* `orchestration`, düzenleme adıdır. Bu, bu Orchestrator işlevinin yeni örneklerini başlatmak istediklerinde istemcilerin kullanması gereken değerdir. Bu özellik isteğe bağlıdır. Belirtilmemişse, işlevin adı kullanılır.

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
> JavaScript Orchestrator işlevleri hiçbir şekilde `async` olarak bildirilmelidir.

### <a name="trigger-usage-net"></a>Kullanım tetiklemesi (.NET)

Düzenleme tetikleyicisi bağlaması hem giriş hem de çıkışları destekler. Giriş ve çıkış işleme hakkında daha fazla bilgi edinmek için aşağıdaki noktalara dikkat edin:

* **girişler** -.net Orchestration işlevleri parametre türü olarak yalnızca [Durableorchestrationcontext](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html) 'i destekler. Doğrudan işlev imzasında girişlerin serisini kaldırma desteklenmez. Kod, Orchestrator işlev girişlerini getirmek için [Getınput @ no__t-1T >](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_GetInput__1)(.net) veya `getInput` (JavaScript) metodunu kullanmalıdır. Bu girişler JSON serileştirilebilir türler olmalıdır.
* **çıktılar** -düzenleme Tetikleyicileri, çıkış değerlerinin yanı sıra girdileri de destekler. İşlevin dönüş değeri, çıkış değerini atamak için kullanılır ve JSON ile seri hale getirilebilir olmalıdır. Bir .NET işlevi `Task` veya `void` döndürürse, bir @no__t 2 değeri çıkış olarak kaydedilir.

### <a name="trigger-sample"></a>Tetikleyici örneği

Aşağıdaki örnek kod, en basit "Merhaba Dünya" Orchestrator işlevinin nasıl görünebileceğini göstermektedir:

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
> JavaScript 'teki `context` nesnesi DurableOrchestrationContext, ancak [işlev bağlamını bir bütün olarak](../functions-reference-node.md#context-object)temsil etmez. Düzenleme yöntemlerine `context` nesnesinin `df` özelliği aracılığıyla erişebilirsiniz.

> [!NOTE]
> JavaScript yöneticileri `return` kullanmalıdır. @No__t-0 kitaplığı, `context.done` metodunu çağırma işlemini gerçekleştirir.

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

## <a name="activity-trigger"></a>Etkinlik tetikleyicisi

Etkinlik tetikleyicisi, [etkinlik işlevleri](durable-functions-types-features-overview.md#activity-functions)olarak bilinen Orchestrator işlevleri tarafından çağrılan işlevleri yazmanıza olanak sağlar.

Visual Studio kullanıyorsanız, etkinlik tetikleyicisi [Activitytriggerattribute](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.ActivityTriggerAttribute.html) .net özniteliği kullanılarak yapılandırılır.

Geliştirme için VS Code veya Azure portal kullanıyorsanız, etkinlik tetikleyicisi *function. JSON*' ın `bindings` DIZISINDE şu JSON nesnesi tarafından tanımlanır:

```json
{
    "name": "<Name of input parameter in function signature>",
    "activity": "<Optional - name of the activity>",
    "type": "activityTrigger",
    "direction": "in"
}
```

* `activity` etkinliğin adıdır. Bu değer, Orchestrator işlevlerinin bu etkinlik işlevini çağırmak için kullandığı addır. Bu özellik isteğe bağlıdır. Belirtilmemişse, işlevin adı kullanılır.

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

* **girişler** -.net etkinlik işlevleri, bir parametre türü olarak [durableactivitycontext](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableActivityContext.html) kullanımını yerel olarak kullanır. Alternatif olarak, bir etkinlik işlevi JSON ile seri hale getirilebilir olan herhangi bir parametre türüyle bildirilebilecek. @No__t-0 ' ı kullandığınızda, etkinlik işlevi girişini getirmek ve seri durumdan çıkarmak için [Getınput @ no__t-2T >](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableActivityContext.html#Microsoft_Azure_WebJobs_DurableActivityContext_GetInput__1) çağırabilirsiniz.
* **çıktılar** -etkinlik işlevleri, çıkış değerlerinin yanı sıra girdileri de destekler. İşlevin dönüş değeri, çıkış değerini atamak için kullanılır ve JSON ile seri hale getirilebilir olmalıdır. Bir .NET işlevi `Task` veya `void` döndürürse, bir @no__t 2 değeri çıkış olarak kaydedilir.
* **meta veri** -.net etkinlik işlevleri, üst düzenleme örnek kimliğini almak için bir `string instanceId` parametresine bağlanabilir.

### <a name="trigger-sample"></a>Tetikleyici örneği

Aşağıdaki örnek kod, basit bir "Merhaba Dünya" Activity işlevinin nasıl görünebileceğini göstermektedir:

#### <a name="c"></a>C#

```csharp
[FunctionName("SayHello")]
public static string SayHello([ActivityTrigger] DurableActivityContext helloContext)
{
    string name = helloContext.GetInput<string>();
    return $"Hello {name}!";
}
```

.NET `ActivityTriggerAttribute` bağlamasının varsayılan parametre türü `DurableActivityContext` ' dir. Ancak, .NET etkinliği Tetikleyicileri doğrudan JSON-serializlenebilir türlere (ilkel türler dahil) bağlamayı da destekler, bu nedenle aynı işlev aşağıdaki gibi basitleştirilebilir:

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

Orchestration istemci bağlaması, Orchestrator işlevleriyle etkileşime geçen işlevler yazmanızı sağlar. Bu işlevler bazen [istemci işlevleri](durable-functions-types-features-overview.md#client-functions)olarak adlandırılır. Örneğin, düzenleme örnekleri üzerinde aşağıdaki yollarla işlem yapabilirsiniz:

* Bunları başlatın.
* Durumlarını sorgulayın.
* Sonlandırın.
* Olayları çalışırken onlara gönderin.
* Örnek geçmişini temizle.

Visual Studio kullanıyorsanız, Dayanıklı İşlevler 1,0 için [Orchestrationclientattribute](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.OrchestrationClientAttribute.html) .net özniteliğini kullanarak Orchestration istemcisine bağlanabilirsiniz. Dayanıklı İşlevler 2,0 önizlemesinde başlayarak, `DurableClientAttribute` .NET özniteliğini kullanarak Orchestration istemcisine bağlanabilirsiniz.

Geliştirme için betik dilleri (örneğin, *. CSX* veya *. js* dosyaları) kullanıyorsanız, düzenleme tetikleyicisi *function. JSON*' nın `bindings` dizisinde şu JSON nesnesi tarafından tanımlanır:

```json
{
    "name": "<Name of input parameter in function signature>",
    "taskHub": "<Optional - name of the task hub>",
    "connectionName": "<Optional - name of the connection string app setting>",
    "type": "orchestrationClient",
    "direction": "in"
}
```

* `taskHub`-birden çok işlevli uygulamanın aynı depolama hesabını paylaştığı ancak birbirinden yalıtılmış olmaları gereken senaryolarda kullanılır. Belirtilmemişse, `host.json` ' dan varsayılan değer kullanılır. Bu değer, hedef Orchestrator işlevleri tarafından kullanılan değerle aynı olmalıdır.
* `connectionName`-depolama hesabı bağlantı dizesi içeren bir uygulama ayarının adı. Bu bağlantı dizesi tarafından temsil edilen depolama hesabı, hedef Orchestrator işlevlerinin kullandığı aynı olmalıdır. Belirtilmemişse, işlev uygulaması için varsayılan depolama hesabı bağlantı dizesi kullanılır.

> [!NOTE]
> Çoğu durumda, bu özellikleri atlamanızı ve varsayılan davranışa güvenmenizi öneririz.

### <a name="client-usage"></a>İstemci kullanımı

.NET işlevlerinde, genellikle Dayanıklı İşlevler tarafından desteklenen tüm istemci API 'Lerine tam erişim sağlayan `DurableOrchestrationClient` ' a bağlarsınız. Dayanıklı İşlevler 2,0 ' den başlayarak `IDurableOrchestrationClient` arabirimine bağlamanız gerekir. JavaScript 'te, aynı API 'Ler `getClient` ' dan döndürülen nesne tarafından gösterilir. İstemci nesnesindeki API 'Ler şunları içerir:

* [StartNewAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_StartNewAsync_)
* [GetStatusAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_GetStatusAsync_)
* [Sonlandırateasync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_TerminateAsync_)
* [RaiseEventAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_RaiseEventAsync_)
* [PurgeInstanceHistoryAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_PurgeInstanceHistoryAsync_)
* [CreateCheckStatusResponse](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_CreateCheckStatusResponse_)
* [CreateHttpManagementPayload](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_CreateHttpManagementPayload_)

Alternatif olarak, .NET işlevleri `T` ' in [StartOrchestrationArgs](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.StartOrchestrationArgs.html) veya `JObject` olduğu `IAsyncCollector<T>` ' a bağlanabilir.

Bu işlemler hakkında daha fazla bilgi için bkz. [Durableorchestrationclient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) API 'si belgeleri.

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

#### <a name="c-sample"></a>C#Örnekli

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

## <a name="entity-trigger"></a>Varlık tetikleyicisi

Varlık Tetikleyicileri, [varlık işlevleri](durable-functions-entities.md)yazmanıza izin verir. Bu tetikleyici belirli bir varlık örneği için olayları işlemeyi destekler.

Azure Işlevleri için Visual Studio Araçları 'nı kullandığınızda, varlık tetikleyicisi `EntityTriggerAttribute` .NET özniteliği kullanılarak yapılandırılır.

> [!NOTE]
> Varlık Tetikleyicileri Dayanıklı İşlevler 2,0 ve üzeri sürümlerde kullanılabilir. Varlık Tetikleyicileri henüz JavaScript için kullanılabilir değil.

Dahili olarak bu tetikleyici bağlama, işlev uygulaması için varsayılan depolama hesabındaki bir dizi kuyruğu yoklar. Bu kuyruklar, uzantının iç uygulama ayrıntılardır ve bu nedenle bağlama özelliklerinde açıkça yapılandırılmazlar.

### <a name="trigger-behavior"></a>Tetikleme davranışı

Varlık tetikleyicisiyle ilgili bazı notlar aşağıda verilmiştir:

* **Tek iş parçacıklı**: belirli bir varlığa yönelik işlemleri işlemek için tek bir dağıtıcı iş parçacığı kullanılır. Aynı anda birden fazla ileti tek bir varlığa gönderilirse, işlemler bir kerelik olarak işlenir.
* **Zehirli ileti işleme** -varlık tetikleyicilerinde bir zarar iletisi desteği yoktur.
* **İleti görünürlüğü** -varlık tetikleyici iletileri kuyruğa alınır ve yapılandırılabilir bir süre boyunca görünmez tutulur. İşlev uygulaması çalıştığı ve sağlıklı olduğu sürece bu iletilerin görünürlüğü otomatik olarak yenilenir.
* **Dönüş değerleri** -varlık işlevleri dönüş değerlerini desteklemiyor. Durumu kaydetmek veya değerleri düzenlemeye geri geçirmek için kullanılabilecek belirli API 'Ler vardır.

Yürütme işlemi sırasında bir varlıkta yapılan herhangi bir durum değişikliği, yürütme tamamlandıktan sonra otomatik olarak kalıcı hale getirilir.

### <a name="trigger-usage-net"></a>Kullanım tetiklemesi (.NET)

Her varlık işlevinin, aşağıdaki üyelere sahip `IDurableEntityContext` parametre türü vardır:

* **EntityName**: Şu anda yürütülmekte olan varlığın adı.
* **EntityKey**: Şu anda yürütülmekte olan varlığın anahtarı.
* **EntityId**: Şu anda yürütülmekte olan varlığın kimliği.
* **OperationName**: geçerli işlemin adı.
* **Hasstate**: varlığın var olup olmadığı, yani bir durumu var. 
* **GetState @ no__t-1TState > ()** : varlığın geçerli durumunu alır. Zaten mevcut değilse, oluşturulur ve `default<TState>` olarak başlatılır. @No__t-0 parametresi, ilkel veya JSON seri hale getirilen bir tür olmalıdır. 
* **GetState @ no__t-1TState > (ınitfunction)** : varlığın geçerli durumunu alır. Zaten mevcut değilse, belirtilen `initfunction` parametresi çağırarak oluşturulur. @No__t-0 parametresi, ilkel veya JSON seri hale getirilen bir tür olmalıdır. 
* **Setstate (arg)** : varlığın durumunu oluşturur veya güncelleştirir. @No__t-0 parametresi JSON-serializlenebilir nesne veya ilkel olmalıdır.
* **DeleteState ()** : varlığın durumunu siler. 
* **Getınput @ no__t-1Tınput > ()** : geçerli işlem için girişi alır. @No__t-0 tür parametresi, ilkel veya JSON seri hale getirilen bir tür olmalıdır.
* **Return (arg)** : işlemi çağıran düzenleme için bir değer döndürür. @No__t-0 parametresi, ilkel veya JSON seri hale getirilen bir nesne olmalıdır.
* **İmza (EntityId, işlem, giriş)** : bir varlığa tek yönlü bir ileti gönderir. @No__t-0 parametresi null olmayan bir dize olmalı ve `input` parametresi basit veya JSON-serializlenebilir nesne olmalıdır.
* **CreateNewOrchestration (Orchestratorfonksiyonadı, giriş)** : yeni bir düzenleme başlatır. @No__t-0 parametresi, ilkel veya JSON seri hale getirilen bir nesne olmalıdır.

Varlık işlevine geçirilen `IDurableEntityContext` nesnesine `Entity.Current` Async-Local özelliği kullanılarak erişilebilir. Bu yaklaşım, sınıf tabanlı programlama modeli kullanılırken kullanışlıdır.

### <a name="trigger-sample-function-based-syntax"></a>Tetikleyici örneği (işlev tabanlı sözdizimi)

Aşağıdaki kod, dayanıklı bir işlev olarak uygulanan basit bir *sayaç* varlığına bir örnektir. Bu işlev, her biri bir tamsayı durumu üzerinde çalışan üç işlem olan `add`, `reset` ve `get` tanımlar.

```csharp
[FunctionName("Counter")]
public static void Counter([EntityTrigger] IDurableEntityContext ctx)
{
    switch (ctx.OperationName.ToLowerInvariant())
    {
        case "add":
            ctx.SetState(ctx.GetState<int>() + ctx.GetInput<int>());
            break;
        case "reset":
            ctx.SetState(0);
            break;
        case "get":
            ctx.Return(ctx.GetState<int>()));
            break;
    }
}
```

İşlev tabanlı sözdizimi ve nasıl kullanılacağı hakkında daha fazla bilgi için bkz. [Işlev tabanlı sözdizimi](durable-functions-dotnet-entities.md#function-based-syntax).

### <a name="trigger-sample-class-based-syntax"></a>Tetikleyici örneği (sınıf tabanlı sözdizimi)

Aşağıdaki örnek, sınıfları ve yöntemleri kullanarak `Counter` varlığının eşdeğer bir uygulamasıdır.

```csharp
[JsonObject(MemberSerialization.OptIn)]
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

Bu varlığın durumu, sayacın geçerli değerini depolayan bir alan içeren `Counter` türünde bir nesnedir. Bu nesneyi depolamada kalıcı hale getirmek için, [JSON.net](https://www.newtonsoft.com/json) kitaplığı tarafından serileştirilmiş ve seri durumdan çıkarılmış olur. 

Sınıf tabanlı sözdizimi ve nasıl kullanılacağı hakkında daha fazla bilgi için bkz. [varlık sınıfları tanımlama](durable-functions-dotnet-entities.md#defining-entity-classes).

> [!NOTE]
> @No__t-0 özniteliğine sahip işlev giriş noktası yöntemi, varlık sınıfları kullanılırken-2 *@no__t bildirilmelidir.* Statik olmayan giriş noktası yöntemleri, birden fazla nesne başlatmaya ve olasılıkla diğer tanımsız davranışlara neden olabilir.

Varlık sınıfları bağlamalarla ve .NET bağımlılığı ekleme ile etkileşim kurmak için özel mekanizmalarda yer vardır. Daha fazla bilgi için bkz. [varlık oluşturma](durable-functions-dotnet-entities.md#entity-construction).

## <a name="entity-client"></a>Varlık istemcisi

Varlık istemci bağlaması, [varlık işlevlerini](#entity-trigger)zaman uyumsuz olarak tetiklemenize olanak sağlar. Bu işlevler bazen [istemci işlevleri](durable-functions-types-features-overview.md#client-functions)olarak adlandırılır.

Visual Studio kullanıyorsanız, `DurableClientAttribute` .NET özniteliğini kullanarak varlık istemcisine bağlanabilirsiniz.

> [!NOTE]
> @No__t-0, [Orchestration istemcisine](#orchestration-client)bağlamak için de kullanılabilir.

Geliştirme için betik dilleri (örneğin, *. CSX* veya *. js* dosyaları) kullanıyorsanız, varlık tetikleyicisi, *function. JSON*' nın `bindings` dizisinde şu JSON nesnesi tarafından tanımlanır:

```json
{
    "name": "<Name of input parameter in function signature>",
    "taskHub": "<Optional - name of the task hub>",
    "connectionName": "<Optional - name of the connection string app setting>",
    "type": "durableClient",
    "direction": "out"
}
```

* `taskHub`-birden çok işlevli uygulamanın aynı depolama hesabını paylaştığı ancak birbirinden yalıtılmış olmaları gereken senaryolarda kullanılır. Belirtilmemişse, `host.json` ' dan varsayılan değer kullanılır. Bu değer, hedef varlık işlevleri tarafından kullanılan değerle eşleşmelidir.
* `connectionName`-depolama hesabı bağlantı dizesi içeren bir uygulama ayarının adı. Bu bağlantı dizesi tarafından temsil edilen depolama hesabı, hedef varlık işlevleri tarafından kullanılan bir aynı olmalıdır. Belirtilmemişse, işlev uygulaması için varsayılan depolama hesabı bağlantı dizesi kullanılır.

> [!NOTE]
> Çoğu durumda, isteğe bağlı özellikleri atlamanızı ve varsayılan davranışa güvenmenizi öneririz.

### <a name="entity-client-usage"></a>Varlık istemci kullanımı

.NET işlevlerinde, genellikle dayanıklı varlıkların desteklediği tüm istemci API 'Lerine tam erişim sağlayan `IDurableEntityClient` ' a bağlarsınız. Ayrıca, her iki varlık ve düzenleme için istemci API 'Lerine erişim sağlayan `IDurableClient` arabirimine da bağlanabilirsiniz. İstemci nesnesindeki API 'Ler şunları içerir:

* **Readentitystateasync @ no__t-1T >** : bir varlığın durumunu okur. Hedef varlığın mevcut olup olmadığını ve Öyleyse durumunun ne olduğunu belirten bir yanıt döndürür.
* **Tiflentityasync**: bir varlığa tek yönlü bir ileti gönderir ve kuyruğa alınıp alınmasını bekler.

Bir sinyal göndermeden önce hedef varlık oluşturmanız gerekmez; varlık durumu, sinyali işleyen varlık işlevinin içinden oluşturulabilir.

> [!NOTE]
> İstemciden gönderilen "sinyaller", daha sonra zaman uyumsuz olarak işlenmek üzere sıraya alınmış olduğunu anlamak önemlidir. Özellikle, `SignalEntityAsync` genellikle varlık işlem başlamadan önce döndürülür ve dönüş değerini geri almak veya özel durumları gözlemlemek mümkün değildir. Daha güçlü garantiler gerekliyse (örneğin, iş akışları için), *Orchestrator işlevlerinin* kullanılması gerekir ve bu da varlık işlemlerinin tamamlanmasını bekleyebilir ve dönüş değerlerini işleyebilir ve özel durumları gözlemleyebilirsiniz.

### <a name="example-client-signals-entity-directly"></a>Örnek: istemci doğrudan varlığa bildirir

Aşağıda, bir "Counter" varlığı çağıran, kuyruğa geçirilmiş örnek bir işlev verilmiştir.

```csharp
[FunctionName("AddFromQueue")]
public static Task Run(
    [QueueTrigger("durable-function-trigger")] string input,
    [DurableClient] IDurableEntityClient client)
{
    // Entity operation input comes from the queue message content.
    var entityId = new EntityId(nameof(Counter), "myCounter");
    int amount = int.Parse(input);
    return client.SignalEntityAsync(entityId, "Add", amount);
}
```

### <a name="example-client-signals-entity-via-interface"></a>Örnek: istemci, arabirimi aracılığıyla varlığa işaret eder

Mümkün olduğunda, daha fazla tür denetimi sağladığından [varlıklara arabirimler üzerinden erişmenizi](durable-functions-dotnet-entities.md#accessing-entities-through-interfaces) öneririz. Örneğin, daha önce bahsedilen `Counter` varlığının, aşağıdaki gibi tanımlanan bir `ICounter` arabirimini uyguladığını varsayalım:

```csharp
public interface ICounter
{
    void Add(int amount);
    void Reset();
    Task<int> Get();
}

public class Counter : ICounter
{
    // ...
}
```

İstemci kodu daha sonra tür açısından güvenli bir ara sunucu oluşturmak için `SignalEntityAsync<ICounter>` kullanabilir:

```csharp
[FunctionName("UserDeleteAvailable")]
public static async Task AddValueClient(
    [QueueTrigger("my-queue")] string message,
    [DurableClient] IDurableEntityClient client)
{
    var target = new EntityId(nameof(Counter), "myCounter");
    int amount = int.Parse(message);
    await client.SignalEntityAsync<ICounter>(target, proxy => proxy.Add(amount));
}
```

@No__t-0 parametresi, `Add` çağrısını dahili olarak `SignalEntityAsync` ' e eşit (türsüz) çağrıya çeviren `ICounter` ' in dinamik olarak üretilmiş bir örneğidir.

> [!NOTE]
> @No__t-0 API 'Leri tek yönlü işlemleri temsil eder. Bir varlık arabirimleri `Task<T>` döndürürse `T` parametresinin değeri her zaman null veya `default` olur.

Özellikle, hiçbir değer döndürülmediğinden `Get` işlemini işaret etmek mantıklı değildir. Bunun yerine, istemciler sayaç durumuna doğrudan erişmek için `ReadStateAsync` kullanabilir ya da `Get` işlemini çağıran bir Orchestrator işlevini başlatabilir. 

<a name="host-json"></a>
## <a name="hostjson-settings"></a>Host. JSON ayarları

[!INCLUDE [durabletask](../../../includes/functions-host-json-durabletask.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Örnek yönetimi için yerleşik HTTP API başvurusu](durable-functions-http-api.md)
