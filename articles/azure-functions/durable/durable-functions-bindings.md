---
title: Dayanıklı Işlevler için Bağlamalar - Azure
description: Azure İşlevler için Dayanıklı İşlevler uzantısı için tetikleyiciler ve bağlamalar nasıl kullanılır.
ms.topic: conceptual
ms.date: 12/17/2019
ms.author: azfuncdf
ms.openlocfilehash: 1f42c6c9b0086d49e539040334c83cfc0c6feb42
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79278226"
---
# <a name="bindings-for-durable-functions-azure-functions"></a>Dayanıklı Işlevler için Bağlamalar (Azure İşlevleri)

[Dayanıklı Fonksiyonlar](durable-functions-overview.md) uzantısı, orkestratör ve etkinlik işlevlerinin yürütülmesini kontrol eden iki yeni tetikleyici bağlaması sunar. Ayrıca, Dayanıklı İşlevler çalışma zamanı için istemci görevi gören bir çıktı bağlama da sunar.

## <a name="orchestration-trigger"></a>Orkestrasyon tetikleyicisi

Orkestrasyon [tetikleyicidayanıklı orkestratör fonksiyonları](durable-functions-types-features-overview.md#orchestrator-functions)yazar sağlar. Bu tetikleyici, yeni orchestrator işlev örneklerinin başlatılmasını ve bir görevi "bekleyen" varolan orchestrator işlev örneklerini sürdürmeyi destekler.

Azure İşlevler için Visual Studio araçlarını kullandığınızda, [orkestrasyon tetikleyicisi OrchestrationTriggerAttribute](https://docs.microsoft.com/dotnet/api/Microsoft.Azure.WebJobs.Extensions.DurableTask.OrchestrationTriggerAttribute?view=azure-dotnet) .NET özniteliği kullanılarak yapılandırılır.

Komut dosyası dillerinde (örneğin, JavaScript veya C# komut dosyası) orkestratör işlevleri yazdığınızda, düzenleme `bindings` *tetikleyicisi function.json* dosyasının dizisinde aşağıdaki JSON nesnesi tarafından tanımlanır:

```json
{
    "name": "<Name of input parameter in function signature>",
    "orchestration": "<Optional - name of the orchestration>",
    "type": "orchestrationTrigger",
    "direction": "in"
}
```

* `orchestration`bu orkestratör işlevinin yeni örneklerini başlatmak istediklerinde istemcilerin kullanması gereken orkestrasyonun adıdır. Bu özellik isteğe bağlıdır. Belirtilmemişse, işlevin adı kullanılır.

Dahili olarak bu tetikleyici bağlama işlev uygulaması için varsayılan depolama hesabında bir dizi kuyrukları yoklar. Bu kuyruklar uzantının iç uygulama ayrıntılarıdır, bu nedenle bağlama özelliklerinde açıkça yapılandırılmamıştır.

### <a name="trigger-behavior"></a>Tetikleyici davranışı

Burada orkestrasyon tetikleyici hakkında bazı notlar şunlardır:

* **Tek iş parçacığı** - Tek bir ana bilgisayar örneğinde tüm orchestrator işlevi yürütme için tek bir sevk iş parçacığı kullanılır. Bu nedenle, orchestrator işlev kodunun verimli olduğundan ve herhangi bir G/Ç gerçekleştirmediğinden emin olmak önemlidir. Bu iş parçacığının Dayanıklı İşlevlere özgü görev türlerini beklemek dışında herhangi bir async çalışması yapmamasını sağlamak da önemlidir.
* **Zehir-mesaj işleme** - Orkestrasyon tetikleyicileri hiçbir zehirli mesaj desteği yoktur.
* **İleti görünürlüğü** - Orkestrasyon tetikleyici iletileri sıradan silinerek yapılandırılabilir bir süre boyunca görünmez tutulur. Bu iletilerin görünürlüğü, işlev uygulaması çalıştığı ve sağlıklı olduğu sürece otomatik olarak yenilenir.
* **İade değerleri** - İade değerleri JSON'a seri olarak ve Azure Tablo depolamasındaki düzenleme geçmişi tablosunda kalıcı olarak sayılsın. Bu iade değerleri, daha sonra açıklanan orkestrayon istemcisi bağlama tarafından sorgulanabilir.

> [!WARNING]
> Orchestrator işlevleri, orkestrasyon tetikleyici bağlama dışında hiçbir giriş veya çıkış bağlama sı kullanılmamalıdır. Bu bağlamalar tek iş parçacığı ve G/Ç kurallarına uymayabileceğinden, bunu yapmak Dayanıklı Görev uzantısı ile ilgili sorunlara neden olabilir. Diğer bağlamaları kullanmak istiyorsanız, bunları Orchestrator işlevinizden çağrılan bir Etkinlik işlevine ekleyin.

> [!WARNING]
> JavaScript orchestrator işlevleri asla `async`beyan edilmemelidir.

### <a name="trigger-usage-net"></a>Tetikleme kullanımı (.NET)

Orkestrasyon tetikleme bağlama hem giriş hem de çıkışları destekler. Giriş ve çıktı işleme hakkında bilmeniz gereken bazı şeyler şunlardır:

* **girişleri** - .NET orkestrasyon işlevleri yalnızca `DurableOrchestrationContext` parametre türü olarak desteksağlar. Girdilerin doğrudan işlev imzası içinde deserialization desteklenmez. Kod, orchestrator işlev girişlerini getirmek için `GetInput<T>` (.NET) veya `getInput` (JavaScript) yöntemini kullanmalıdır. Bu girişler JSON serileştirilebilir türleri olmalıdır.
* **çıkışlar** - Orkestrasyon, çıkışların yanı sıra girdileri de destekler. İşlevin dönüş değeri çıkış değerini atamak için kullanılır ve JSON serileştirilebilir olmalıdır. Bir .NET işlevi `Task` `void`dönerse `null` veya , çıktı olarak bir değer kaydedilir.

### <a name="trigger-sample"></a>Tetikleme örneği

Aşağıdaki örnek kod, en basit "Hello World" orkestratör işlevinin nasıl görünebileceğini gösterir:

#### <a name="c"></a>C#

```csharp
[FunctionName("HelloWorld")]
public static string Run([OrchestrationTrigger] IDurableOrchestrationContext context)
{
    string name = context.GetInput<string>();
    return $"Hello {name}!";
}
```
> [!NOTE]
> Önceki kod Dayanıklı Fonksiyonlar 2.x içindir. Dayanıklı Fonksiyonlar 1.x `DurableOrchestrationContext` `IDurableOrchestrationContext`için, 'yi yerine kullanmalısınız. Sürümler arasındaki farklar hakkında daha fazla bilgi için [Dayanıklı İşlevler Sürümleri](durable-functions-versions.md) makalesine bakın.

#### <a name="javascript-functions-20-only"></a>JavaScript (Yalnızca 2.0 Fonksiyonları)

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context) {
    const name = context.df.getInput();
    return `Hello ${name}!`;
});
```

> [!NOTE]
> JavaScript'teki `context` nesne Dayanıklı Düzenleme Bağlamını temsil etmez, ancak [işlev bağlamını bir bütün olarak](../functions-reference-node.md#context-object)temsil eder. Düzenleme yöntemlerine nesnenin `context` `df` özelliği üzerinden erişebilirsiniz.

> [!NOTE]
> JavaScript orchestrators `return`kullanmalısınız. Kitaplık `durable-functions` `context.done` yöntemi arama ilgilenir.

Çoğu orkestratör işlevi etkinlik işlevlerini çağırır, bu nedenle burada bir etkinlik işlevinin nasıl çağrıldığını gösteren bir "Hello World" örneği verilmiştir:

#### <a name="c"></a>C#

```csharp
[FunctionName("HelloWorld")]
public static async Task<string> Run(
    [OrchestrationTrigger] IDurableOrchestrationContext context)
{
    string name = context.GetInput<string>();
    string result = await context.CallActivityAsync<string>("SayHello", name);
    return result;
}
```

> [!NOTE]
> Önceki kod Dayanıklı Fonksiyonlar 2.x içindir. Dayanıklı Fonksiyonlar 1.x `DurableOrchestrationContext` `IDurableOrchestrationContext`için, 'yi yerine kullanmalısınız. Sürümler arasındaki farklar hakkında daha fazla bilgi için [Dayanıklı Işlevler sürümleri](durable-functions-versions.md) makalesine bakın.

#### <a name="javascript-functions-20-only"></a>JavaScript (Yalnızca 2.0 Fonksiyonları)

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context) {
    const name = context.df.getInput();
    const result = yield context.df.callActivity("SayHello", name);
    return result;
});
```

## <a name="activity-trigger"></a>Etkinlik tetikleyicisi

Etkinlik tetikleyicisi, [etkinlik işlevleri](durable-functions-types-features-overview.md#activity-functions)olarak bilinen orchestrator işlevleri tarafından çağrılan işlevleri yazmanızı sağlar.

Visual Studio kullanıyorsanız, etkinlik tetikleyicisi `ActivityTriggerAttribute` .NET özniteliği kullanılarak yapılandırılır.

Geliştirme için VS Kodu veya Azure portalı kullanıyorsanız, etkinlik tetikleyicisi `bindings` *function.json*dizisinde aşağıdaki JSON nesnesi tarafından tanımlanır:

```json
{
    "name": "<Name of input parameter in function signature>",
    "activity": "<Optional - name of the activity>",
    "type": "activityTrigger",
    "direction": "in"
}
```

* `activity`etkinliğin adıdır. Bu değer, orkestratör işlevlerinin bu etkinlik işlevini çağırmak için kullandığı addır. Bu özellik isteğe bağlıdır. Belirtilmemişse, işlevin adı kullanılır.

Dahili olarak bu, işlev uygulaması için varsayılan depolama hesabında bir sıra bağlama yı tetikler. Bu sıra, bağlama özelliklerinde açıkça yapılandırılmamış olmasının nedeni olan uzantının bir iç uygulama ayrıntısIdir.

### <a name="trigger-behavior"></a>Tetikleyici davranışı

Etkinlik tetikleyicisi hakkında bazı notlar aşağıda veda edebilirsiniz:

* **İş parçacığı** - Düzenleme tetikleyicisinin aksine, etkinlik tetikleyicilerinin iş parçacığı veya G/Ç ile ilgili herhangi bir kısıtlaması yoktur. Onlar düzenli işlevleri gibi tedavi edilebilir.
* **Zehir iletisi işleme** - Etkinlik tetikleyicilerinde zehirli mesaj desteği yoktur.
* **İleti görünürlüğü** - Etkinlik tetikleyici iletileri sıradan silinür ve yapılandırılabilir bir süre boyunca görünmez tutulur. Bu iletilerin görünürlüğü, işlev uygulaması çalıştığı ve sağlıklı olduğu sürece otomatik olarak yenilenir.
* **İade değerleri** - İade değerleri JSON'a seri olarak ve Azure Tablo depolamasındaki düzenleme geçmişi tablosunda kalıcı olarak sayılsın.

> [!WARNING]
> Etkinlik işlevleri için depolama arka ucu bir uygulama ayrıntısýr ve kullanıcı kodu bu depolama varlıklarıyla doğrudan etkileşimkurmamalıdır.

### <a name="trigger-usage-net"></a>Tetikleme kullanımı (.NET)

Etkinlik tetikleyici bağlama, tıpkı orkestrasyon tetikleyicisi gibi hem giriş hem de çıkışları destekler. Giriş ve çıktı işleme hakkında bilmeniz gereken bazı şeyler şunlardır:

* **girişleri** - .NET etkinlik fonksiyonları `DurableActivityContext` yerel olarak parametre türü olarak kullanılır. Alternatif olarak, bir etkinlik işlevi JSON-serializable herhangi bir parametre türü ile bildirilebilir. Kullandığınızda, `DurableActivityContext`etkinlik işlevi `GetInput<T>` girdisini almak ve deserialize etmek için arayabilirsiniz.
* **çıktılar** - Etkinlik fonksiyonları çıktı değerlerinin yanı sıra girdileri de destekler. İşlevin dönüş değeri çıkış değerini atamak için kullanılır ve JSON serileştirilebilir olmalıdır. Bir .NET işlevi `Task` `void`dönerse `null` veya , çıktı olarak bir değer kaydedilir.
* **meta data** - .NET etkinlik işlevleri, üst orkestrasyonun örnek kimliğini almak için bir `string instanceId` parametreye bağlanabilir.

### <a name="trigger-sample"></a>Tetikleme örneği

Aşağıdaki örnek kod basit bir "Hello World" etkinlik işlevinin nasıl görünebileceğini gösterir:

#### <a name="c"></a>C#

```csharp
[FunctionName("SayHello")]
public static string SayHello([ActivityTrigger] IDurableActivityContext helloContext)
{
    string name = helloContext.GetInput<string>();
    return $"Hello {name}!";
}
```

> [!NOTE]
> Önceki kod Dayanıklı Fonksiyonlar 2.x içindir. Dayanıklı Fonksiyonlar 1.x `DurableActivityContext` `IDurableActivityContext`için, 'yi yerine kullanmalısınız. Sürümler arasındaki farklar hakkında daha fazla bilgi için [Dayanıklı İşlevler Sürümleri](durable-functions-versions.md) makalesine bakın.

.NET `ActivityTriggerAttribute` bağlama için varsayılan parametre `IDurableActivityContext`türü . Ancak, .NET etkinlik tetikleyicileri de doğrudan JSON-serializeable türleri (ilkel türleri dahil) bağlama desteği, böylece aynı işlevi aşağıdaki gibi basitleştirilmiş olabilir:

```csharp
[FunctionName("SayHello")]
public static string SayHello([ActivityTrigger] string name)
{
    return $"Hello {name}!";
}
```

#### <a name="javascript-functions-20-only"></a>JavaScript (Yalnızca 2.0 Fonksiyonları)

```javascript
module.exports = async function(context) {
    return `Hello ${context.bindings.name}!`;
};
```

JavaScript bağlamaları ek parametreler olarak da geçirilebilir, böylece aynı işlev aşağıdaki gibi basitleştirilmiş olabilir:

```javascript
module.exports = async function(context, name) {
    return `Hello ${name}!`;
};
```


### <a name="using-input-and-output-bindings"></a>Giriş ve çıktı bağlamalarını kullanma

Etkinlik tetikleme bağlamasına ek olarak düzenli giriş ve çıkış bağlamaları kullanabilirsiniz. Örneğin, etkinlik bağlama girişi alabilir ve EventHub çıkış bağlama kullanarak bir EventHub bir ileti gönderebilirsiniz:

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

## <a name="orchestration-client"></a>Orkestrasyon istemcisi

Orkestrasyon istemcisi bağlama, orkestratör işlevleriyle etkileşimedebilen işlevleri yazmanızı sağlar. Bu işlevler bazen [istemci işlevleri](durable-functions-types-features-overview.md#client-functions)olarak adlandırılır. Örneğin, orkestrasyon örneklerine aşağıdaki yollarla hareket edebilirsiniz:

* Onları başlatın.
* Durumlarını sorgula.
* Onları yok edin.
* Çalışırken onlara etkinlik gönderin.
* Örnek geçmişi temizleme.

Visual Studio kullanıyorsanız, Dayanıklı Fonksiyonlar 1.0 için .NET özniteliğini `OrchestrationClientAttribute` kullanarak orkestrasyon istemcisine bağlanabilirsiniz. Dayanıklı Fonksiyonlar 2.0'dan başlayarak ,NET özniteliğini `DurableClientAttribute` kullanarak orkestrasyon istemcisine bağlanabilirsiniz.

Geliştirme için komut dosyası dillerini (örneğin,.csx veya *.js* dosyaları) kullanıyorsanız, düzenleme tetikleyicisi `bindings` *function.json*dizisinde aşağıdaki JSON nesnesi tarafından tanımlanır: *.csx*

```json
{
    "name": "<Name of input parameter in function signature>",
    "taskHub": "<Optional - name of the task hub>",
    "connectionName": "<Optional - name of the connection string app setting>",
    "type": "orchestrationClient",
    "direction": "in"
}
```

* `taskHub`- Birden çok işlevli uygulamanın aynı depolama hesabını paylaştığı, ancak birbirinden izole edilmesi gereken senaryolarda kullanılır. Belirtilmemişse, varsayılan `host.json` değer kullanılır. Bu değer, hedef orchestrator işlevleri tarafından kullanılan değer eşleşmelidir.
* `connectionName`- Depolama hesabı bağlantı dizesi içeren bir uygulama ayarının adı. Bu bağlantı dizesi tarafından temsil edilen depolama hesabı, hedef orchestrator işlevleri tarafından kullanılan aynı olmalıdır. Belirtilmemişse, işlev uygulamasının varsayılan depolama hesabı bağlantı dizesi kullanılır.

> [!NOTE]
> Çoğu durumda, bu özellikleri atlayıp varsayılan davranışa güvenmenizi öneririz.

### <a name="client-usage"></a>İstemci kullanımı

.NET işlevlerinde, genellikle `IDurableOrchestrationClient`kalıcı işlevler tarafından desteklenen tüm orkestrasyon istemci API'lerine tam erişim sağlayan ,'a bağlanırsınız. Eski Dayanıklı Fonksiyonlar 2.x sürümlerinde, bunun `DurableOrchestrationClient` yerine sınıfa bağlanır. JavaScript'te, aynı API'ler 'den `getClient`döndürülen nesne tarafından ortaya çıkar. İstemci nesnesindeki API'ler şunlardır:

* `StartNewAsync`
* `GetStatusAsync`
* `TerminateAsync`
* `RaiseEventAsync`
* `PurgeInstanceHistoryAsync`
* `CreateCheckStatusResponse`
* `CreateHttpManagementPayload`

Alternatif olarak, .NET işlevleri `IAsyncCollector<T>` nerede `T` `StartOrchestrationArgs` veya `JObject`.

Bu işlemler hakkında daha fazla `IDurableOrchestrationClient` bilgi için API belgelerine bakın.

### <a name="client-sample-visual-studio-development"></a>İstemci örneği (Visual Studio geliştirme)

Burada bir "HelloWorld" orkestrasyon başlatan bir örnek kuyruk tetiklenen işlevidir.

```csharp
[FunctionName("QueueStart")]
public static Task Run(
    [QueueTrigger("durable-function-trigger")] string input,
    [DurableClient] IDurableOrchestrationClient starter)
{
    // Orchestration input comes from the queue message content.
    return starter.StartNewAsync("HelloWorld", input);
}
```

> [!NOTE]
> Önceki C# kodu Dayanıklı Fonksiyonlar 2.x içindir. Dayanıklı Fonksiyonlar 1.x için `OrchestrationClient` öznitelik yerine `DurableClient` öznitelik kullanmanız gerekir `DurableOrchestrationClient` ve `IDurableOrchestrationClient`parametre türünü yerine kullanmalısınız. Sürümler arasındaki farklar hakkında daha fazla bilgi için [Dayanıklı İşlevler Sürümleri](durable-functions-versions.md) makalesine bakın.

### <a name="client-sample-not-visual-studio"></a>İstemci örneği (Visual Studio değil)

Visual Studio'yu geliştirme için kullanmıyorsanız, aşağıdaki *function.json* dosyasını oluşturabilirsiniz. Bu örnek, dayanıklı orkestrasyon istemcisi bağlama kullanan bir sıra tetiklenen işlevi yapılandırmak için nasıl gösterir:

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
      "type": "durableClient",
      "direction": "in"
    }
  ]
}
```

> [!NOTE]
> Önceki JSON Dayanıklı Fonksiyonlar 2.x içindir. Dayanıklı Fonksiyonlar 1.x için `orchestrationClient` tetikleyici `durableClient` türü yerine kullanmanız gerekir. Sürümler arasındaki farklar hakkında daha fazla bilgi için [Dayanıklı İşlevler Sürümleri](durable-functions-versions.md) makalesine bakın.

Aşağıda, yeni orkestratör işlev örnekleri başlatan dile özgü örnekler vereme vardır.

#### <a name="c-script-sample"></a>C# Komut Dosyası Örneği

Aşağıdaki örnek, kuyruk tetiklenen C# işlevinden yeni bir işlev örneği başlatmak için dayanıklı orkestrasyon istemcisi bağlamanın nasıl kullanılacağını gösterir:

```csharp
#r "Microsoft.Azure.WebJobs.Extensions.DurableTask"

using Microsoft.Azure.WebJobs.Extensions.DurableTask;

public static Task Run(string input, IDurableOrchestrationClient starter)
{
    return starter.StartNewAsync("HelloWorld", input);
}
```

> [!NOTE]
> Önceki kod Dayanıklı Fonksiyonlar 2.x içindir. Dayanıklı Fonksiyonlar 1.x için `DurableOrchestrationClient` `IDurableOrchestrationClient`, . yerine parametre türünü kullanmanız gerekir Sürümler arasındaki farklar hakkında daha fazla bilgi için [Dayanıklı İşlevler Sürümleri](durable-functions-versions.md) makalesine bakın.

#### <a name="javascript-sample"></a>JavaScript Örneği

Aşağıdaki örnek, javascript işlevinden yeni bir işlev örneği başlatmak için dayanıklı düzenleme istemcisi bağlamanın nasıl kullanılacağını gösterir:

```javascript
const df = require("durable-functions");

module.exports = async function (context) {
    const client = df.getClient(context);
    return instanceId = await client.startNew("HelloWorld", undefined, context.bindings.input);
};
```

Başlangıç örnekleri hakkında daha fazla ayrıntı [Instance yönetiminde](durable-functions-instance-management.md)bulunabilir.

## <a name="entity-trigger"></a>Varlık tetikleyicisi

Varlık tetikleyicileri varlık [işlevlerini](durable-functions-entities.md)yazarsağlar. Bu tetikleyici, belirli bir varlık örneği için işleme olaylarını destekler.

Azure İşlevler için Visual Studio araçlarını kullandığınızda, varlık `EntityTriggerAttribute` tetikleyicisi .NET özniteliği kullanılarak yapılandırılır.

> [!NOTE]
> Varlık tetikleyicileri Dayanıklı Fonksiyonlar 2.x'ten başlayarak kullanılabilir.

Dahili olarak bu tetikleyici bağlama işlev uygulaması için varsayılan depolama hesabında bir dizi kuyrukları yoklar. Bu kuyruklar uzantının iç uygulama ayrıntılarıdır, bu nedenle bağlama özelliklerinde açıkça yapılandırılmamıştır.

### <a name="trigger-behavior"></a>Tetikleyici davranışı

Varlık tetikleyicisi hakkında bazı notlar aşağıda veda edebilirsiniz:

* **Tek dişli**: Belirli bir varlığın işlemlerini işlemek için tek bir sevk irsaliyesi iş parçacığı kullanılır. Aynı anda tek bir varlığa birden çok ileti gönderilirse, işlemler tek seferde işlenir.
* **Zehirli ileti işleme** - Varlık tetikleyicilerinde zehirli mesaj desteği yoktur.
* **İleti görünürlüğü** - Varlık tetikleyici iletileri sıradan silinür ve yapılandırılabilir bir süre boyunca görünmez tutulur. Bu iletilerin görünürlüğü, işlev uygulaması çalıştığı ve sağlıklı olduğu sürece otomatik olarak yenilenir.
* **İade değerleri** - Varlık işlevleri iade değerlerini desteklemez. Durumu kaydetmek veya değerleri orkestrasyonlara geri geçirmek için kullanılabilecek özel API'ler vardır.

Yürütme sırasında bir varlıkta yapılan tüm durum değişiklikleri, yürütme tamamlandıktan sonra otomatik olarak devam edilir.

### <a name="trigger-usage-net"></a>Tetikleme kullanımı (.NET)

Her varlık işlevinin `IDurableEntityContext`bir parametre türü vardır, aşağıdaki üyeler vardır:

* **EntityName**: şu anda çalıştırılamakta olan varlığın adı.
* **EntityKey**: şu anda çalıştırılamakta olan varlığın anahtarı.
* **EntityId**: Şu anda çalıştırılamakta olan varlığın kimliği.
* **OperationName**: geçerli işlemin adı.
* **HasState**: varlığın var olup olmadığı, yani bir durumu vardır. 
* **GetState\<TState>()**: varlığın geçerli durumunu alır. Zaten yoksa, oluşturulur ve '' için `default<TState>`başharf. Parametre `TState` ilkel veya JSON serileştirilebilir bir tür olmalıdır. 
* **GetState\<TState>(initfunction)**: varlığın geçerli durumunu alır. Zaten yoksa, sağlanan `initfunction` parametre çağırArak oluşturulur. Parametre `TState` ilkel veya JSON serileştirilebilir bir tür olmalıdır. 
* **SetState(arg)**: varlığın durumunu oluşturur veya güncelleştirir. Parametre `arg` JSON serileştirilebilir nesne veya ilkel olmalıdır.
* **DeleteState()**: varlığın durumunu siler. 
* **GetInput\<TInput>()**: geçerli işlem için giriş alır. Tür `TInput` parametresi ilkel veya JSON serileştirilebilir bir tür olmalıdır.
* **Return(arg)**: işlemi adı verilen orkestrasyona bir değer verir. Parametre `arg` ilkel veya JSON serileştirilebilir bir nesne olmalıdır.
* **SignalEntity(EntityId, scheduledTimeUtc, operation, input)**: bir varlığa tek yönlü ileti gönderir. Parametre `operation` null olmayan bir dize olmalıdır, isteğe bağlı `scheduledTimeUtc` bir UTC tarih saati `input` olmalıdır hangi işlem çağırmak için ve parametre ilkel veya JSON-serileştirilebilir nesne olmalıdır.
* **CreateNewOrchestration (orchestratorFunctionName, input)**: yeni bir orkestrasyon başlatır. Parametre `input` ilkel veya JSON serileştirilebilir bir nesne olmalıdır.

Varlık `IDurableEntityContext` işlevine geçirilen nesneye `Entity.Current` async-local özelliği kullanılarak erişilebilir. Bu yaklaşım, sınıf tabanlı programlama modelini kullanırken kullanışlıdır.

### <a name="trigger-sample-c-function-based-syntax"></a>Tetikleyici örnek (C# fonksiyon tabanlı sözdizimi)

Aşağıdaki kod, dayanıklı bir işlev olarak uygulanan basit bir *Karşı* varlık örneğidir. Bu işlev, `add` `reset` `get`her biri tamsayı durumunda çalışan üç işlem tanımlar.

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

İşlev tabanlı sözdizimi ve nasıl kullanılacağı hakkında daha fazla bilgi için [Bkz. İşlev Tabanlı Sözdizimi.](durable-functions-dotnet-entities.md#function-based-syntax)

### <a name="trigger-sample-c-class-based-syntax"></a>Tetikleyici örnek (C# sınıfı tabanlı sözdizimi)

Aşağıdaki örnek, sınıfları ve `Counter` yöntemleri kullanarak varlığın eşdeğer bir uygulamasıdır.

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

Bu varlığın durumu, sayacın geçerli değerini depolayan bir alan içeren bir tür `Counter`nesnesidir. Bu nesneyi depolamada kalıcı hale getirmek [için, Json.NET](https://www.newtonsoft.com/json) kitaplığı tarafından seri hale getirilir ve deserialize edilir. 

Sınıf tabanlı sözdizimi ve nasıl kullanılacağı hakkında daha fazla bilgi için varlık [sınıflarını tanımlama'ya](durable-functions-dotnet-entities.md#defining-entity-classes)bakın.

> [!NOTE]
> Varlık sınıfları kullanırken `[FunctionName]` öznitelik ile `static` işlev giriş noktası yöntemi *bildirilmelidir.* Statik olmayan giriş noktası yöntemleri birden çok nesne başlatma ve potansiyel olarak diğer tanımlanmamış davranışlara neden olabilir.

Varlık sınıfları bağlamalar ve .NET bağımlılık enjeksiyonu ile etkileşim için özel mekanizmalara sahiptir. Daha fazla bilgi için Entity [construction 'a](durable-functions-dotnet-entities.md#entity-construction)bakın.

### <a name="trigger-sample-javascript"></a>Tetikleme örneği (JavaScript)

Aşağıdaki kod, JavaScript'te yazılmış dayanıklı bir işlev olarak uygulanan basit bir *Karşı* varlık örneğidir. Bu işlev, `add` `reset` `get`her biri tamsayı durumunda çalışan üç işlem tanımlar.

**fonksiyon.json**
```json
{
  "bindings": [
    {
      "name": "context",
      "type": "entityTrigger",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

**index.js**
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

> [!NOTE]
> Dayanıklı varlıklar `durable-functions` NPM paketinin sürüm **1.3.0** ile başlayan JavaScript mevcuttur.

## <a name="entity-client"></a>Varlık istemcisi

Varlık istemcisi [bağlama, varlık işlevlerini](#entity-trigger)eş senkronize olarak tetiklemenizi sağlar. Bu işlevler bazen [istemci işlevleri](durable-functions-types-features-overview.md#client-functions)olarak adlandırılır.

Visual Studio kullanıyorsanız, .NET özniteliğini `DurableClientAttribute` kullanarak varlık istemcisine bağlanabilirsiniz.

> [!NOTE]
> Ayrıca `[DurableClientAttribute]` [orkestrasyon istemcisine](#orchestration-client)bağlamak için kullanılabilir.

Geliştirme için komut dosyası dillerini (örneğin, *.csx* veya *.js* dosyaları) kullanıyorsanız, varlık `bindings` *tetikleyicisi function.json*dizisinde aşağıdaki JSON nesnesi tarafından tanımlanır:

```json
{
    "name": "<Name of input parameter in function signature>",
    "taskHub": "<Optional - name of the task hub>",
    "connectionName": "<Optional - name of the connection string app setting>",
    "type": "durableClient",
    "direction": "in"
}
```

* `taskHub`- Birden çok işlevli uygulamanın aynı depolama hesabını paylaştığı, ancak birbirinden izole edilmesi gereken senaryolarda kullanılır. Belirtilmemişse, varsayılan `host.json` değer kullanılır. Bu değer, hedef varlık işlevleri tarafından kullanılan değerle eşleşmelidir.
* `connectionName`- Depolama hesabı bağlantı dizesi içeren bir uygulama ayarının adı. Bu bağlantı dizesi tarafından temsil edilen depolama hesabı, hedef varlık işlevleri tarafından kullanılan depo hesabıyla aynı olmalıdır. Belirtilmemişse, işlev uygulamasının varsayılan depolama hesabı bağlantı dizesi kullanılır.

> [!NOTE]
> Çoğu durumda, isteğe bağlı özellikleri atlayıp varsayılan davranışa güvenmenizi öneririz.

### <a name="entity-client-usage"></a>Varlık istemci kullanımı

.NET işlevlerinde, genellikle `IDurableEntityClient`,dayanıklı varlıklar tarafından desteklenen tüm istemci API'lerine tam erişim sağlar. Ayrıca, hem varlıklar `IDurableOrchestrationClient` hem de orkestrasyonlar için istemci API'lerine erişim sağlayan arabirimeye de bağlanabilirsiniz. İstemci nesnesindeki API'ler şunlardır:

* **ReadEntityStateAsync\<T>**: bir varlığın durumunu okur. Hedef varlığın var olup olmadığını ve varsa durumunun ne olduğunu belirten bir yanıt döndürür.
* **SignalEntityAsync**: bir varlığa tek yönlü bir ileti gönderir ve sırayla gelmesini bekler.
* **ListEntitiesAsync**: birden çok varlığın durumu için sorgular. Varlıklar *ada* ve son *işlem saatine*göre sorgulanabilir.

Sinyal göndermeden önce hedef varlığı oluşturmaya gerek yoktur - varlık durumu sinyali işleyen varlık işlevi içinden oluşturulabilir.

> [!NOTE]
> İstemciden gönderilen "sinyallerin" daha sonra eşzamanlı olarak işlenmek üzere sıraya gireceğini anlamak önemlidir. Özellikle, varlık `SignalEntityAsync` işlemi başlatmadan önce genellikle döner ve iade değerini geri almak veya özel durumları gözlemlemek mümkün değildir. Daha güçlü garantiler gerekiyorsa (örneğin iş akışları için), varlık işlemlerinin tamamlanmasını bekleyecek ve iade değerlerini işleyip özel durumlara göz yutabilen *orchestrator işlevleri* kullanılmalıdır.

### <a name="example-client-signals-entity-directly---c"></a>Örnek: İstemci varlığı doğrudan bildirir - C #

Burada bir "Sayaç" varlık çağıran bir örnek kuyruk tetiklenen işlevdir.

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

### <a name="example-client-signals-entity-via-interface---c"></a>Örnek: istemci arabirimi üzerinden varlık sinyalleri - C #

Mümkün olduğunda, daha fazla tür denetimi [sağladığından, varlıklara arabirimler üzerinden erişmenizi](durable-functions-dotnet-entities.md#accessing-entities-through-interfaces) öneririz. Örneğin, daha `Counter` önce bahsedilen varlığın `ICounter` aşağıdaki gibi tanımlanan bir arabirim uyguladığını varsayalım:

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

İstemci kodu `SignalEntityAsync<ICounter>` daha sonra tür güvenli bir proxy oluşturmak için kullanabilirsiniz:

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

Parametre, çağrıyı eşdeğer (yazılmamış) `Add` çağrıya dahili olarak çeviren dinamik olarak `SignalEntityAsync`oluşturulan bir örneğidir. `proxy` `ICounter`

> [!NOTE]
> API'ler `SignalEntityAsync` tek yönlü işlemleri temsil ediyor. Bir varlık arabirimleri `Task<T>`dönerse, `T` parametrenin değeri her `default`zaman null veya .

Özellikle, hiçbir değer döndürülmeden `Get` işlemi sinyal vermek mantıklı değildir. Bunun yerine, istemciler sayaç durumuna doğrudan erişmek `ReadStateAsync` için kullanabilir veya `Get` işlemi çağıran bir orchestrator işlevi başlatabilir.

### <a name="example-client-signals-entity---javascript"></a>Örnek: istemci sinyalleri varlık - JavaScript

JavaScript'te bir "Sayaç" varlığı sinyali veren örnek bir sıra tetiklenen işlev aşağıda verilmiştir.

**fonksiyon.json**
```json
{
    "bindings": [
      {
        "name": "input",
        "type": "queueTrigger",
        "queueName": "durable-entity-trigger",
        "direction": "in",
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

**index.js**
```javascript
const df = require("durable-functions");

module.exports = async function (context) {
    const client = df.getClient(context);
    const entityId = new df.EntityId("Counter", "myCounter");
    await context.df.signalEntity(entityId, "add", 1);
};
```

> [!NOTE]
> Dayanıklı varlıklar `durable-functions` NPM paketinin sürüm **1.3.0** ile başlayan JavaScript mevcuttur.

<a name="host-json"></a>
## <a name="hostjson-settings"></a>host.json ayarları

[!INCLUDE [durabletask](../../../includes/functions-host-json-durabletask.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Örnek yönetimi için yerleşik HTTP API başvurusu](durable-functions-http-api.md)
