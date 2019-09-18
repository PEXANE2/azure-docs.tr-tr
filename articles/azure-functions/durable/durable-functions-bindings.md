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
ms.openlocfilehash: f297c89d2c3ba5692a44fab631c0d46c75f48692
ms.sourcegitcommit: 0fab4c4f2940e4c7b2ac5a93fcc52d2d5f7ff367
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/17/2019
ms.locfileid: "71033593"
---
# <a name="bindings-for-durable-functions-azure-functions"></a>Dayanıklı İşlevler bağlamaları (Azure Işlevleri)

[Dayanıklı işlevler](durable-functions-overview.md) uzantısı, Orchestrator ve etkinlik işlevlerinin yürütülmesini denetleyen iki yeni tetikleyici bağlama sunar. Ayrıca, Dayanıklı İşlevler çalışma zamanı için istemci görevi gören bir çıkış bağlaması da sunar.

## <a name="orchestration-trigger"></a>Düzenleme tetikleyicisi

Orchestration tetikleyicisi, [dayanıklı Orchestrator işlevlerini](durable-functions-types-features-overview.md#orchestrator-functions)yazmanıza olanak sağlar. Bu tetikleyici, yeni Orchestrator işlev örneklerinin başlamasını ve "bekleyen" bir görevi olan mevcut Orchestrator işlev örneklerinin devam ettirmeyi destekler.

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
> JavaScript içindeki nesne, durableorchestrationcontext, ancak [işlev bağlamını bir bütün olarak](../functions-reference-node.md#context-object)temsil etmez. `context` Düzenleme yöntemlerine `context` `df` nesnenin özelliği aracılığıyla erişebilirsiniz.

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

## <a name="activity-trigger"></a>Etkinlik tetikleyicisi

Etkinlik tetikleyicisi, [etkinlik işlevleri](durable-functions-types-features-overview.md#activity-functions)olarak bilinen Orchestrator işlevleri tarafından çağrılan işlevleri yazmanıza olanak sağlar.

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

* `activity`etkinliğin adıdır. Bu değer, Orchestrator işlevlerinin bu etkinlik işlevini çağırmak için kullandığı addır. Bu özellik isteğe bağlıdır. Belirtilmemişse, işlevin adı kullanılır.

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

Visual Studio kullanıyorsanız, Dayanıklı İşlevler 1,0 için [Orchestrationclientattribute](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.OrchestrationClientAttribute.html) .net özniteliğini kullanarak Orchestration istemcisine bağlanabilirsiniz. Dayanıklı işlevler 2,0 önizlemeden başlayarak, `DurableClientAttribute` .net özniteliğini kullanarak Orchestration istemcisine bağlanabilirsiniz.

Geliştirme için betik dilleri (örneğin, *. CSX* veya *. js* dosyaları) kullanıyorsanız, düzenleme tetikleyicisi `bindings` *function. JSON*dizisinde aşağıdaki JSON nesnesi tarafından tanımlanır:

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

.Net işlevlerinde, genellikle dayanıklı işlevler tarafından desteklenen tüm `DurableOrchestrationClient`istemci API 'lerine tam erişim sağlayan öğesine bağlayabilirsiniz. Dayanıklı işlevler 2,0 ' den başlayarak `IDurableOrchestrationClient` arabirimine bağlayın. JavaScript 'te, aynı API 'Ler tarafından döndürülen `getClient`nesne tarafından gösterilir. İstemci nesnesindeki API 'Ler şunları içerir:

* [StartNewAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_StartNewAsync_)
* [GetStatusAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_GetStatusAsync_)
* [Sonlandırateasync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_TerminateAsync_)
* [RaiseEventAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_RaiseEventAsync_)
* [PurgeInstanceHistoryAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_PurgeInstanceHistoryAsync_)
* [CreateCheckStatusResponse](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_CreateCheckStatusResponse_)
* [CreateHttpManagementPayload](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_CreateHttpManagementPayload_)

Alternatif olarak, .net işlevleri `IAsyncCollector<T>` [StartOrchestrationArgs](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.StartOrchestrationArgs.html) veya `JObject`olduğu yere `T` bağlanabilir.

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

## <a name="entity-trigger"></a>Varlık tetikleyicisi

Varlık Tetikleyicileri, [varlık işlevleri](durable-functions-entities.md)yazmanıza izin verir. Bu tetikleyici belirli bir varlık örneği için olayları işlemeyi destekler.

Azure işlevleri için Visual Studio Araçları 'nı kullandığınızda, varlık tetikleyicisi `EntityTriggerAttribute` .net özniteliği kullanılarak yapılandırılır.

> [!NOTE]
> Varlık Tetikleyicileri Dayanıklı İşlevler 2,0 ve üzeri sürümlerde kullanılabilir. Varlık Tetikleyicileri henüz JavaScript için kullanılabilir değil.

Dahili olarak bu tetikleyici bağlama, işlev uygulaması için varsayılan depolama hesabındaki bir dizi kuyruğu yoklar. Bu kuyruklar, uzantının iç uygulama ayrıntılardır ve bu nedenle bağlama özelliklerinde açıkça yapılandırılmazlar.

### <a name="trigger-behavior"></a>Tetikleme davranışı

Varlık tetikleyicisiyle ilgili bazı notlar aşağıda verilmiştir:

* **Tek iş parçacıklı**: Belirli bir varlığa yönelik işlemleri işlemek için tek bir dağıtıcı iş parçacığı kullanılır. Aynı anda birden fazla ileti tek bir varlığa gönderilirse, işlemler bir kerelik olarak işlenir.
* **Zehirli ileti işleme** -varlık tetikleyicilerinde bir zarar iletisi desteği yoktur.
* **İleti görünürlüğü** -varlık tetikleyici iletileri kuyruğa alınır ve yapılandırılabilir bir süre boyunca görünmez tutulur. İşlev uygulaması çalıştığı ve sağlıklı olduğu sürece bu iletilerin görünürlüğü otomatik olarak yenilenir.
* **Dönüş değerleri** -varlık işlevleri dönüş değerlerini desteklemiyor. Durumu kaydetmek veya değerleri düzenlemeye geri geçirmek için kullanılabilecek belirli API 'Ler vardır.

Yürütme işlemi sırasında bir varlıkta yapılan herhangi bir durum değişikliği, yürütme tamamlandıktan sonra otomatik olarak kalıcı hale getirilir.

### <a name="trigger-usage-net"></a>Kullanım tetiklemesi (.NET)

Her varlık işlevinin `IDurableEntityContext`, aşağıdaki üyelere sahip olan parametre türü vardır:

* **EntityName**: Yürütülmekte olan varlığın adını alır.
* **EntityKey**: Yürütülmekte olan varlığın anahtarını alır.
* **EntityId**: Yürütülmekte olan varlığın KIMLIĞINI alır.
* **OperationName**: geçerli işlemin adını alır.
* **Inewlyinşa**: varlık `true` işlemden önce yoksa, döndürür.
* **GetState\<TState > ()** : varlığın geçerli durumunu alır. `TState` Parametre bir ilkel veya JSON seri hale getirilen tür olmalıdır.
* **Setstate (nesne)** : varlığın durumunu güncelleştirir. `object` Parametre bir ilkel veya JSON-serializlenebilir nesne olmalıdır.
* **Getınput\<TInput > ()** : geçerli işlem için girişi alır. `TInput` Tür parametresi basit veya JSON seri hale getirilen bir türü temsil etmelidir.
* **Return (nesne)** : işlemi çağıran düzenleme için bir değer döndürür. `object` Parametre bir ilkel veya JSON-serializlenebilir nesne olmalıdır.
* **Yeniden dönüşlü şekilde**: geçerli işlem bittikten sonra varlığı siler.
* **İmza (EntityId, dize, nesne)** : bir varlığa tek yönlü bir ileti gönderir. `object` Parametre bir ilkel veya JSON-serializlenebilir nesne olmalıdır.

Sınıf tabanlı varlık programlama modunu kullanırken, `IDurableEntityContext` nesnesine `Entity.Current` thread-static özelliği kullanılarak başvurulabilir.

### <a name="trigger-sample---entity-function"></a>Tetikleyici örneği-varlık işlevi

Aşağıdaki kod, standart bir işlev olarak uygulanan basit bir *sayaç* varlığına bir örnektir. Bu işlev, `get` `add` `reset` herbiribirtamsayıdurumdeğeriüzerindeçalışanüçişlem,,,ve`currentValue`tanımlar.

```csharp
[FunctionName(nameof(Counter))]
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

### <a name="trigger-sample---entity-class"></a>Tetikleyici örneği-varlık sınıfı

Aşağıdaki örnek, önceki `Counter` varlığın .NET sınıfları ve yöntemlerini kullanarak eşdeğer bir uygulamasıdır.

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

> [!NOTE]
> Varlık sınıfları kullanılırken `[FunctionName]` özniteliği olan işlev giriş noktası *yöntemi bildirilmelidir.* `static` Statik olmayan giriş noktası yöntemleri, birden fazla nesne başlatmaya ve olasılıkla diğer tanımsız davranışlara neden olabilir.

Varlık sınıfları bağlamalarla ve .NET bağımlılığı ekleme ile etkileşim kurmak için özel mekanizmalarda yer vardır. Daha fazla bilgi için bkz. [dayanıklı varlıklar](durable-functions-entities.md) makalesi.

## <a name="entity-client"></a>Varlık istemcisi

Varlık istemci bağlaması, [varlık işlevlerini](#entity-trigger)zaman uyumsuz olarak tetiklemenize olanak sağlar. Bu işlevler bazen [istemci işlevleri](durable-functions-types-features-overview.md#client-functions)olarak adlandırılır.

Visual Studio kullanıyorsanız, `DurableClientAttribute` .net özniteliğini kullanarak Entity Client 'a bağlanabilirsiniz.

> [!NOTE]
> Orchestration istemcisine bağlamak için de kullanılabilir. [](#orchestration-client) `[DurableClientAttribute]`

Geliştirme için betik dilleri (örneğin, *. CSX* veya *. js* dosyaları) kullanıyorsanız, varlık tetikleyicisi, `bindings` *function. JSON*dizisinde aşağıdaki JSON nesnesi tarafından tanımlanır:

```json
{
    "name": "<Name of input parameter in function signature>",
    "taskHub": "<Optional - name of the task hub>",
    "connectionName": "<Optional - name of the connection string app setting>",
    "type": "durableClient",
    "direction": "out"
}
```

* `taskHub`-Birden çok işlevli uygulamanın aynı depolama hesabını paylaştığı ancak birbirinden yalıtılması gereken senaryolarda kullanılır. Belirtilmemişse, varsayılan değer `host.json` kullanılır. Bu değer, hedef varlık işlevleri tarafından kullanılan değerle eşleşmelidir.
* `connectionName`-Depolama hesabı bağlantı dizesi içeren bir uygulama ayarının adı. Bu bağlantı dizesi tarafından temsil edilen depolama hesabı, hedef varlık işlevleri tarafından kullanılan bir aynı olmalıdır. Belirtilmemişse, işlev uygulaması için varsayılan depolama hesabı bağlantı dizesi kullanılır.

> [!NOTE]
> Çoğu durumda, isteğe bağlı özellikleri atlamanızı ve varsayılan davranışa güvenmenizi öneririz.

### <a name="entity-client-usage"></a>Varlık istemci kullanımı

.Net işlevlerinde, genellikle, dayanıklı varlıkların desteklediği `IDurableEntityClient`tüm istemci API 'lerine tam erişim sağlayan öğesine bağlayabilirsiniz. Ayrıca, her iki varlık ve `IDurableClient` düzenleme için istemci API 'lerine erişim sağlayan arabirimine de bağlanabilirsiniz. İstemci nesnesindeki API 'Ler şunları içerir:

* **Readentitystateasync\<T >** : bir varlığın durumunu okur.
* **Tiflentityasync**: bir varlığa tek yönlü bir ileti gönderir ve kuyruğa alınıp alınmasını bekler.
* **Tiflentityasync\<TEntityInterface >** : ile `SignalEntityAsync` aynıdır, ancak türünde `TEntityInterface`oluşturulan bir proxy nesnesi kullanır.
* **Createentityproxy\<TEntityInterface >** : dinamik olarak, varlıklara tür açısından güvenli `TEntityInterface` çağrılar yapmak için türünde dinamik bir ara sunucu oluşturur.

> [!NOTE]
> Önceki "sinyal" işlemlerinin tümünün zaman uyumsuz olduğunu anlamak önemlidir. Bir varlık işlevini çağırmak ve bir istemciden dönüş değeri geri almak mümkün değildir. `SignalEntityAsync` Benzer şekilde, varlık işlemi yürütmeye başlamadan önce döndürebilir. Yalnızca Orchestrator işlevleri, varlık işlevlerini eşzamanlı olarak çağırabilir ve dönüş değerlerini işleyebilir.

API `SignalEntityAsync` 'ler, varlığın benzersiz tanımlayıcısının bir `EntityId`olarak belirtilmesini gerektirir. Bu API 'ler Ayrıca, isteğe bağlı olarak `string` varlık işleminin adını ve işlemin yükünü JSON-serializlenebilir `object`olarak alır. Hedef varlık yoksa, belirtilen varlık KIMLIĞIYLE otomatik olarak oluşturulur.

### <a name="client-sample-untyped"></a>İstemci örneği (türsüz)

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

### <a name="client-sample-typed"></a>İstemci örneği (yazılan)

Varlık işlemlerine güvenli tür erişimi için bir proxy nesnesi oluşturmak mümkündür. Tür açısından güvenli bir ara sunucu oluşturmak için varlık türünün bir arabirim uygulaması gerekir. Örneğin, daha önce bahsedilen `Counter` varlığın aşağıdaki gibi tanımlanmış bir `ICounter` arabirim uyguladığını varsayalım:

```csharp
public interface ICounter
{
    void Add(int amount);
    void Reset();
    int Get();
}

public class Counter : ICounter
{
    // ...
}
```

İstemci kodu daha sonra kullanabilir `SignalEntityAsync<TEntityInterface>` ve bir tür `ICounter` güvenli proxy oluşturmak için tür parametresi olarak arabirimi belirtebilir. Tür açısından güvenli proxy 'lerin bu kullanımı, aşağıdaki kod örneğinde gösterilmiştir:

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

Önceki örnekte, `proxy` parametresi, `Add` çağrısını iç olarak eşdeğer (türsüz) `ICounter`çağrıya `SignalEntityAsync`çeviren, dinamik olarak üretilmiş bir örneğidir.

Varlık arabirimlerini tanımlamaya yönelik birkaç kural vardır:

* `TEntityInterface` İçindeki`SignalEntityAsync<TEntityInterface>` tür parametresi bir arabirim olmalıdır.
* Varlık arabirimleri yalnızca yöntemleri tanımlamalıdır.
* Varlık arabirimi yöntemleri birden fazla parametre tanımlamalıdır.
* Varlık arabirimi yöntemleri, bir `void`dönüş `Task`değeri döndürmelidir `Task<T>` , `T` veya burada yer almalıdır.
* Varlık arabirimleri aynı derleme içinde (yani, Entity sınıfı) tam olarak bir somut uygulama sınıfına sahip olmalıdır.

Bu kurallardan herhangi biri ihlal edilirse, çalışma zamanında `InvalidOperationException` bir oluşturulur. Özel durum iletisi hangi kuralın bozulduğunu açıklayacak.

> [!NOTE]
> `SignalEntityAsync` API 'ler tek yönlü işlemleri temsil eder. Bir varlık arabirimleri döndürürse `Task<T>`, `T` parametresinin değeri her zaman null veya `default`olur.

<a name="host-json"></a>

## <a name="hostjson-settings"></a>Host.JSON ayarları

[!INCLUDE [durabletask](../../../includes/functions-host-json-durabletask.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Örnek yönetimi için yerleşik HTTP API başvurusu](durable-functions-http-api.md)