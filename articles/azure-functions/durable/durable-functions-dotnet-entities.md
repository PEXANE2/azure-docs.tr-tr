---
title: .NET-Azure Işlevlerinde dayanıklı varlıklara Geliştirici Kılavuzu
description: Azure Işlevleri için Dayanıklı İşlevler Uzantısı ile .NET 'teki dayanıklı varlıklarla çalışma.
author: sebastianburckhardt
ms.topic: conceptual
ms.date: 10/06/2019
ms.author: azfuncdf
ms.openlocfilehash: 8fdf298357370415c1b3af95dd9ed22ad8539786
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85125489"
---
# <a name="developers-guide-to-durable-entities-in-net"></a>.NET 'teki dayanıklı varlıklara Geliştirici Kılavuzu

Bu makalede, örnekler ve genel öneriler de dahil olmak üzere .NET ile dayanıklı varlıkların geliştirilmesi için kullanılabilir arabirimler anlatılmaktadır. 

Varlık işlevleri, uygulama durumunu hassas bir varlık koleksiyonu olarak düzenlemenin kolay bir yolunu sunan sunucusuz uygulama geliştiricileri sağlar. Temel kavramlar hakkında daha fazla ayrıntı için bkz. [dayanıklı varlıklar: kavramlar](durable-functions-entities.md) makalesi.

Şu anda varlıkları tanımlamak için iki API sunuyoruz:

- **Sınıf tabanlı sözdizimi** , varlıkları ve işlemleri sınıflar ve yöntemler olarak temsil eder. Bu söz dizimi kolayca okunabilir bir kod üretir ve işlemler, arabirimler aracılığıyla tür denetimli bir şekilde çağrılabilir. 

- **İşlev tabanlı sözdizimi** , varlıkları işlev olarak temsil eden bir alt düzey arabirimdir. Varlık işlemlerinin nasıl dağıtıldığı ve varlık durumunun nasıl yönetildiği üzerinde kesin denetim sağlar.  

Bu makalede, uygulamanın çoğu uygulama için daha uygun olması beklendiğinden, temel olarak sınıf tabanlı söz dizimine odaklanılır. Ancak, [işlev tabanlı sözdizimi](#function-based-syntax) , varlık durumu ve işlemleri için kendi soyutlamalarını tanımlamak veya yönetmek isteyen uygulamalar için uygun olabilir. Ayrıca, şu anda sınıf tabanlı sözdizimi tarafından desteklenmeyen genericity gerektiren kitaplıkları uygulamak için uygun olabilir. 

> [!NOTE]
> Sınıf tabanlı sözdizimi yalnızca işlev tabanlı sözdiziminin üzerinde bulunan bir katmandır, bu nedenle her iki çeşit de aynı uygulamadaki birbirlerinin yerine kullanılabilir. 
 
## <a name="defining-entity-classes"></a>Varlık sınıfları tanımlama

Aşağıdaki örnek, `Counter` tamsayı türünde tek bir değer depolayan ve dört işlem,, ve sunan bir varlık uygulamasıdır `Add` `Reset` `Get` `Delete` .

```csharp
[JsonObject(MemberSerialization.OptIn)]
public class Counter
{
    [JsonProperty("value")]
    public int Value { get; set; }

    public void Add(int amount) 
    {
        this.Value += amount;
    }

    public Task Reset() 
    {
        this.Value = 0;
        return Task.CompletedTask;
    }

    public Task<int> Get() 
    {
        return Task.FromResult(this.Value);
    }

    public void Delete() 
    {
        Entity.Current.DeleteState();
    }

    [FunctionName(nameof(Counter))]
    public static Task Run([EntityTrigger] IDurableEntityContext ctx)
        => ctx.DispatchAsync<Counter>();
}
```

`Run`İşlevi, sınıf tabanlı sözdiziminin kullanılması için gereken ortak içeriği içerir. *Statik* bir Azure işlevi olmalıdır. Varlık tarafından işlenen her bir işlem iletisi için bir kez yürütülür. `DispatchAsync<T>`Çağrıldığında ve varlık bellekte değilse, türünde bir nesne oluşturur `T` ve alanlarını depolama alanında bulunan son kalıcı JSON (varsa) olarak doldurur. Ardından, eşleşen ada sahip yöntemi çağırır.

> [!NOTE]
> Sınıf tabanlı bir varlığın durumu, varlık bir işlemi işlemden önce **örtük olarak oluşturulur** ve çağırarak bir işlemde **açıkça silinebilir** `Entity.Current.DeleteState()` .

### <a name="class-requirements"></a>Sınıf gereksinimleri
 
Varlık sınıfları, özel süper sınıflar, arabirimler veya öznitelikler gerektirmeyen POCOs (düz eski CLR nesneleri). Ancak

- Sınıf oluşturulabilir olmalıdır (bkz. [varlık oluşturma](#entity-construction)).
- Sınıf JSON ile seri hale getirilebilir olmalıdır (bkz. [varlık serileştirmesi](#entity-serialization)).

Ayrıca, bir işlem olarak çağrılması amaçlanan her yöntemin ek gereksinimleri karşılaması gerekir:

- Bir işlem en fazla bir bağımsız değişkene sahip olmalı ve herhangi bir aşırı yükleme veya genel tür bağımsız değişkeni içermemelidir.
- Arabirim kullanarak bir Orchestration 'tan çağrılması amaçlanan bir işlemin veya döndürmesi gerekir `Task` `Task<T>` .
- Bağımsız değişkenler ve dönüş değerleri seri hale getirilebilir değerler veya nesneler olmalıdır.

### <a name="what-can-operations-do"></a>İşlemler ne yapabilir?

Tüm varlık işlemleri varlık durumunu okuyabilir ve güncelleştirebilir ve durumdaki değişiklikler otomatik olarak depolama için kalıcı hale getirilir. Üstelik, işlemler, tüm Azure Işlevleri için genel limitlerde dış g/ç veya diğer hesaplamalar gerçekleştirebilir.

İşlemler ayrıca bağlam tarafından sunulan işlevlere erişebilir `Entity.Current` :

* `EntityName`: Şu anda yürütülmekte olan varlığın adı.
* `EntityKey`: Şu anda yürütülmekte olan varlığın anahtarı.
* `EntityId`: Şu anda yürütülmekte olan varlığın KIMLIĞI (ad ve anahtar içerir).
* `SignalEntity`: bir varlığa tek yönlü bir ileti gönderir.
* `CreateNewOrchestration`: yeni bir düzenleme başlatır.
* `DeleteState`: Bu varlığın durumunu siler.

Örneğin, sayaç varlığı 100 'e ulaştığında bir düzenleme başlatan ve varlık KIMLIĞINI bir giriş bağımsız değişkeni olarak geçirdiğinde, sayaç varlığını değiştirebiliriz:

```csharp
    public void Add(int amount) 
    {
        if (this.Value < 100 && this.Value + amount >= 100)
        {
            Entity.Current.StartNewOrchestration("MilestoneReached", Entity.Current.EntityId);
        }
        this.Value += amount;      
    }
```

## <a name="accessing-entities-directly"></a>Varlıklara doğrudan erişme

Sınıf tabanlı varlıklara, varlık ve onun işlemleri için açık dize adları kullanılarak doğrudan erişilebilir. Aşağıda bazı örnekler sunuyoruz; temel kavramların daha derin bir açıklaması (sinyaller ve çağrılar gibi) için bkz. [erişim varlıkları](durable-functions-entities.md#access-entities)içindeki tartışma. 

> [!NOTE]
> Mümkün olduğunda, daha fazla tür denetimi sağladığından [varlıklara arabirimler üzerinden erişmenizi](#accessing-entities-through-interfaces)öneririz.

### <a name="example-client-signals-entity"></a>Örnek: istemci sinyalleri varlığı

Aşağıdaki Azure http Işlevi, REST kurallarını kullanarak bir SILME işlemi uygular. Anahtar, URL yolunda geçilen sayaç varlığına bir silme sinyali gönderir.

```csharp
[FunctionName("DeleteCounter")]
public static async Task<HttpResponseMessage> DeleteCounter(
    [HttpTrigger(AuthorizationLevel.Function, "delete", Route = "Counter/{entityKey}")] HttpRequestMessage req,
    [DurableClient] IDurableEntityClient client,
    string entityKey)
{
    var entityId = new EntityId("Counter", entityKey);
    await client.SignalEntityAsync(entityId, "Delete");    
    return req.CreateResponse(HttpStatusCode.Accepted);
}
```

### <a name="example-client-reads-entity-state"></a>Örnek: istemci varlık durumunu okur

Aşağıdaki Azure http Işlevi, REST kurallarını kullanarak bir GET işlemi uygular. Anahtar, URL yolunda geçilen sayaç varlığının geçerli durumunu okur.

```csharp
[FunctionName("GetCounter")]
public static async Task<HttpResponseMessage> GetCounter(
    [HttpTrigger(AuthorizationLevel.Function, "get", Route = "Counter/{entityKey}")] HttpRequestMessage req,
    [DurableClient] IDurableEntityClient client,
    string entityKey)
{
    var entityId = new EntityId("Counter", entityKey);
    var state = await client.ReadEntityStateAsync<Counter>(entityId); 
    return req.CreateResponse(state);
}
```

> [!NOTE]
> Tarafından döndürülen nesne `ReadEntityStateAsync` yalnızca yerel bir kopyadır, diğer bir deyişle, bir önceki zaman noktasından varlık durumunun bir anlık görüntüsüdür. Özellikle, eski olabilir ve bu nesnenin değiştirilmesi gerçek varlık üzerinde hiçbir etkiye sahip değildir. 

### <a name="example-orchestration-first-signals-then-calls-entity"></a>Örnek: düzenleme ilk sinyalleri, sonra varlığı çağırır

Aşağıdaki düzenleme bir sayaç varlığına onu arttırmasını bildirir ve ardından en son değerini okumak için aynı varlığı çağırır.

```csharp
[FunctionName("IncrementThenGet")]
public static async Task<int> Run(
    [OrchestrationTrigger] IDurableOrchestrationContext context)
{
    var entityId = new EntityId("Counter", "myCounter");

    // One-way signal to the entity - does not await a response
    context.SignalEntity(entityId, "Add", 1);

    // Two-way call to the entity which returns a value - awaits the response
    int currentValue = await context.CallEntityAsync<int>(entityId, "Get");

    return currentValue;
}
```

## <a name="accessing-entities-through-interfaces"></a>Arabirimler aracılığıyla varlıklara erişme

Arabirimler, oluşturulan proxy nesneleri aracılığıyla varlıklara erişmek için kullanılabilir. Bu yaklaşım, bir işlemin ad ve bağımsız değişken türünün uygulanmış ile eşleşmesini sağlar. Mümkün olduğunda varlıklara erişmek için arabirimler kullanmanızı öneririz.

Örneğin, sayaç örneğini şu şekilde değiştirebiliriz:

```csharp
public interface ICounter
{
    void Add(int amount);
    Task Reset();
    Task<int> Get();
    void Delete();
}

public class Counter : ICounter
{
    ...
}
```

Varlık sınıfları ve varlık arabirimleri, [Orleans](https://www.microsoft.com/research/project/orleans-virtual-actors/)tarafından oluşturulan graıns ve gres arabirimine benzerdir. Dayanıklı varlıklar ve Orleans arasındaki benzerlikler ve farklar hakkında daha fazla bilgi için bkz. [sanal aktörler Ile karşılaştırma](durable-functions-entities.md#comparison-with-virtual-actors).

Tür denetimi sağlamanın yanı sıra, arabirimler uygulamanın içindeki kaygıları daha iyi bir şekilde ayırmada yararlı olur. Örneğin, bir varlık birden çok arabirim uygulayabileceğinizden, tek bir varlık birden çok rol sunabilir. Ayrıca, bir arabirim birden çok varlık tarafından uygulanmayabilir, ancak genel iletişim desenleri yeniden kullanılabilir kitaplıklar olarak uygulanabilir.

### <a name="example-client-signals-entity-through-interface"></a>Örnek: istemci, varlığa arabirim aracılığıyla işaret eder

İstemci kodu, `SignalEntityAsync<TEntityInterface>` uygulayan varlıklara sinyal göndermek için kullanabilir `TEntityInterface` . Örneğin:

```csharp
[FunctionName("DeleteCounter")]
public static async Task<HttpResponseMessage> DeleteCounter(
    [HttpTrigger(AuthorizationLevel.Function, "delete", Route = "Counter/{entityKey}")] HttpRequestMessage req,
    [DurableClient] IDurableEntityClient client,
    string entityKey)
{
    var entityId = new EntityId("Counter", entityKey);
    await client.SignalEntityAsync<ICounter>(entityId, proxy => proxy.Delete());    
    return req.CreateResponse(HttpStatusCode.Accepted);
}
```

Bu örnekte, parametresi, `proxy` `ICounter` çağrısını dahili olarak bir sinyaline çeviren dinamik olarak üretilmiş bir örneğidir `Delete` .

> [!NOTE]
> `SignalEntityAsync`API 'ler yalnızca tek yönlü işlemler için kullanılabilir. Bir işlem dönüşse bile `Task<T>` , `T` parametrenin değeri her zaman null veya `default` gerçek sonuç değildir.
Örneğin, `Get` hiçbir değer döndürülmediğinden işlemin sinyal almak mantıklı değildir. Bunun yerine, istemciler `ReadStateAsync` sayaç durumuna doğrudan erişmek için ya da işlemi çağıran bir Orchestrator işlevini başlatabilir `Get` . 

### <a name="example-orchestration-first-signals-then-calls-entity-through-proxy"></a>Örnek: düzenleme ilk sinyalleri, sonra da varlığı ara sunucu aracılığıyla çağırır

Bir varlığı bir düzenleme içinden çağırmak veya sinyal almak için, `CreateEntityProxy` varlık için bir ara sunucu oluşturmak üzere arabirim türü ile birlikte kullanılabilir. Bu proxy daha sonra, işlemleri çağırmak veya sinyal almak için kullanılabilir:

```csharp
[FunctionName("IncrementThenGet")]
public static async Task<int> Run(
    [OrchestrationTrigger] IDurableOrchestrationContext context)
{
    var entityId = new EntityId("Counter", "myCounter");
    var proxy = context.CreateEntityProxy<ICounter>(entityId);

    // One-way signal to the entity - does not await a response
    proxy.Add(1);

    // Two-way call to the entity which returns a value - awaits the response
    int currentValue = await proxy.Get();

    return currentValue;
}
```

Örtük olarak, döndüren tüm işlemlere `void` ve döndürülen veya çağrılan işlemlere işaret edilir `Task` `Task<T>` . Bu varsayılan davranışı değiştirebilir ve yöntemi açık bir şekilde kullanarak görevi döndürseler bile sinyal işlemleri olabilir `SignalEntity<IInterfaceType>` .

### <a name="shorter-option-for-specifying-the-target"></a>Hedefi belirtmek için daha kısa seçenek

Bir arabirim kullanarak bir varlığı çağırırken veya sinyalde olduğunda, ilk bağımsız değişken hedef varlığı belirtmelidir. Hedef, varlık KIMLIĞI belirtilerek veya varlığı uygulayan tek bir sınıfın olması durumunda yalnızca varlık anahtarı olarak belirtilebilir.

```csharp
context.SignalEntity<ICounter>(new EntityId(nameof(Counter), "myCounter"), ...);
context.SignalEntity<ICounter>("myCounter", ...);
```

Yalnızca varlık anahtarı belirtilirse ve çalışma zamanında benzersiz bir uygulama bulunamazsa `InvalidOperationException` oluşturulur. 

### <a name="restrictions-on-entity-interfaces"></a>Varlık arabirimleriyle ilgili kısıtlamalar

Her zamanki gibi, tüm parametre ve dönüş türleri JSON ile seri hale getirilebilir olmalıdır. Aksi takdirde, serileştirme özel durumları çalışma zamanında oluşturulur.

Ayrıca bazı ek kurallar uyguladık:
* Varlık arabirimleri yalnızca yöntemleri tanımlamalıdır.
* Varlık arabirimleri genel parametreler içermemelidir.
* Varlık Arabirim yöntemlerinin birden fazla parametresi olmamalıdır.
* Varlık arabirimi yöntemleri,, `void` `Task` veya döndürmelidir`Task<T>` 

Bu kurallardan herhangi biri ihlal edilirse, arabirim, `InvalidOperationException` veya için tür bağımsız değişkeni olarak kullanıldığında çalışma zamanında oluşturulur `SignalEntity` `CreateProxy` . Özel durum iletisi hangi kuralın bozulduğunu açıklar.

> [!NOTE]
> Döndüren arabirim metotları `void` yalnızca sinyal alabilir (tek yönlü), çağrılmaz (iki yönlü). Veya döndüren arabirim yöntemleri, veya çağrısı yapılabilir ya `Task` `Task<T>` da çağrılabilir. Çağrılırsa, işlemin sonucunu döndürür ya da işlem tarafından oluşturulan özel durumları yeniden atar. Ancak, imzalaymadığında, işlemden gerçek sonucu veya özel durumu döndürmez, ancak yalnızca varsayılan değer.

## <a name="entity-serialization"></a>Varlık serileştirme

Bir varlığın durumu durda kalıcı olduğundan, varlık sınıfı seri hale getirilebilir olmalıdır. Dayanıklı İşlevler çalışma zamanı, bu amaçla, serileştirme ve seri durumdan çıkarma işlemini denetlemek için bir dizi ilkeyi ve özniteliği destekleyen [JSON.net](https://www.newtonsoft.com/json) kitaplığını kullanır. En yaygın olarak kullanılan C# veri türleri (diziler ve koleksiyon türleri dahil) zaten serileştirilebilir olur ve dayanıklı varlıkların durumunu tanımlamak için kolayca kullanılabilir.

Örneğin, Json.NET, aşağıdaki sınıfı kolayca serileştirilir ve seri durumdan çıkarkaydedebilir:

```csharp
[JsonObject(MemberSerialization = MemberSerialization.OptIn)]
public class User
{
    [JsonProperty("name")]
    public string Name { get; set; }

    [JsonProperty("yearOfBirth")]
    public int YearOfBirth { get; set; }

    [JsonProperty("timestamp")]
    public DateTime Timestamp { get; set; }

    [JsonProperty("contacts")]
    public Dictionary<Guid, Contact> Contacts { get; set; } = new Dictionary<Guid, Contact>();

    [JsonObject(MemberSerialization = MemberSerialization.OptOut)]
    public struct Contact
    {
        public string Name;
        public string Number;
    }

    ...
}
```

### <a name="serialization-attributes"></a>Serileştirme öznitelikleri

Yukarıdaki örnekte, temeldeki Serileştirmeyi daha görünür hale getirmek için birkaç öznitelik eklemeyi tercih ediyoruz:
- `[JsonObject(MemberSerialization.OptIn)]`Sınıfın seri hale getirilebilir olması gerektiğini ve yalnızca JSON özellikleri olarak açıkça işaretlenmiş üyeleri kalıcı hale getirmek için, sınıfına açıklama eklenir.
-  `[JsonProperty("name")]`Bir alanın kalıcı varlık durumunun bir parçası olduğunu ve JSON gösteriminde kullanılacak özellik adını belirtmesini hatırlatmak için ile kalıcı olacak alanlara açıklama ekleyeceğiz.

Ancak, bu öznitelikler gerekli değildir; Json.NET ile çalıştıkları sürece diğer kurallara veya özniteliklere izin verilir. Örneğin, birisi `[DataContract]` öznitelikleri kullanabilir veya hiç öznitelik kullanamaz:

```csharp
[DataContract]
public class Counter
{
    [DataMember]
    public int Value { get; set; }
    ...
}

public class Counter
{
    public int Value;
    ...
}
```

Varsayılan olarak, sınıfın adı JSON gösteriminin bir parçası *olarak depolanmaz: diğer bir deyişle,* `TypeNameHandling.None` varsayılan ayar olarak kullanırız. Bu varsayılan davranış, veya öznitelikleri kullanılarak geçersiz kılınabilir `JsonObject` `JsonProperty` .

### <a name="making-changes-to-class-definitions"></a>Sınıf tanımlarında değişiklik yapma

Depolanan JSON nesnesi artık yeni sınıf tanımıyla eşleşmemesinden, bir uygulama çalıştırıldıktan sonra bir sınıf tanımında değişiklik yapıldığında bazı dikkatli olunması gerekir. Yine de, tarafından kullanılan seri kaldırma işlemini anladığı sürece, değişen veri biçimleri ile doğru bir şekilde uğraşmak mümkündür `JsonConvert.PopulateObject` .

Örneğin, bazı değişiklik örnekleri ve bunların etkileri aşağıda verilmiştir:

1. Depolanan JSON 'da mevcut olmayan yeni bir özellik eklenirse, varsayılan değerini varsayar.
1. Depolanan JSON 'da bulunan bir özellik kaldırılırsa, önceki içerik kaybedilir.
1. Bir özellik yeniden adlandırılırsa, efekt eskisini kaldırmış ve yeni bir tane ekliyor gibi olur.
1. Bir özelliğin türü, depolanan JSON 'dan daha sonra seri durumdan çıkarılmayacak şekilde değiştirilirse, bir özel durum oluşturulur.
1. Bir özelliğin türü değiştirilirse, ancak depolanan JSON 'dan seri durumdan çıkarılabiliyorsa, bunu olur.

Json.NET davranışını özelleştirmek için kullanılabilecek birçok seçenek vardır. Örneğin, saklı JSON sınıfta mevcut olmayan bir alan içeriyorsa bir özel durum zorlamak için özniteliğini belirtin `JsonObject(MissingMemberHandling = MissingMemberHandling.Error)` . Rastgele biçimlerde depolanmış JSON okuyabilen seri kaldırma için özel kod yazmak da mümkündür.

## <a name="entity-construction"></a>Varlık oluşturma

Bazen varlık nesnelerinin nasıl oluşturulduğu hakkında daha fazla denetime sahip olmak istiyoruz. Artık varlık nesneleri oluştururken varsayılan davranışı değiştirmek için birkaç seçenek açıklıyoruz. 

### <a name="custom-initialization-on-first-access"></a>İlk erişimde özel başlatma

Bazen, hiçbir zaman erişilemeyen veya silinen bir varlığa bir işlem göndermeden önce bazı özel başlatma işlemleri gerçekleştirmeniz gerekir. Bu davranışı belirtmek için, bunlardan önce bir koşullu eklenebilir `DispatchAsync` :

```csharp
[FunctionName(nameof(Counter))]
public static Task Run([EntityTrigger] IDurableEntityContext ctx)
{
    if (!ctx.HasState)
    {
        ctx.SetState(...);
    }
    return ctx.DispatchAsync<Counter>();
}
```

### <a name="bindings-in-entity-classes"></a>Varlık sınıflarında bağlamalar

Normal işlevlerin aksine, varlık sınıfı yöntemlerinin giriş ve çıkış bağlamalarına doğrudan erişimi yoktur. Bunun yerine, bağlama verileri giriş noktası işlev bildiriminde yakalanmalı ve ardından `DispatchAsync<T>` yöntemine geçirilmelidir. Öğesine geçirilen herhangi bir nesne `DispatchAsync<T>` , bağımsız değişken olarak varlık sınıfı oluşturucusuna otomatik olarak geçirilir.

Aşağıdaki örnek, `CloudBlobContainer` [BLOB giriş bağlamasındaki](../functions-bindings-storage-blob-input.md) bir başvurunun, sınıf tabanlı bir varlık için nasıl kullanılabilir hale getirilebilir olduğunu gösterir.

```csharp
public class BlobBackedEntity
{
    [JsonIgnore]
    private readonly CloudBlobContainer container;

    public BlobBackedEntity(CloudBlobContainer container)
    {
        this.container = container;
    }

    // ... entity methods can use this.container in their implementations ...
    
    [FunctionName(nameof(BlobBackedEntity))]
    public static Task Run(
        [EntityTrigger] IDurableEntityContext context,
        [Blob("my-container", FileAccess.Read)] CloudBlobContainer container)
    {
        // passing the binding object as a parameter makes it available to the
        // entity class constructor
        return context.DispatchAsync<BlobBackedEntity>(container);
    }
}
```

Azure Işlevleri 'ndeki bağlamalar hakkında daha fazla bilgi için bkz. [Azure Işlevleri Tetikleyicileri ve bağlamaları](../functions-triggers-bindings.md) belgeleri.

### <a name="dependency-injection-in-entity-classes"></a>Varlık sınıflarında bağımlılık ekleme

Varlık sınıfları [Azure Işlevleri bağımlılığı ekleme](../functions-dotnet-dependency-injection.md)işlemini destekler. Aşağıdaki örnekte, bir `IHttpClientFactory` hizmetin sınıf tabanlı bir varlığa nasıl kaydedileceği gösterilmektedir.

```csharp
[assembly: FunctionsStartup(typeof(MyNamespace.Startup))]

namespace MyNamespace
{
    public class Startup : FunctionsStartup
    {
        public override void Configure(IFunctionsHostBuilder builder)
        {
            builder.Services.AddHttpClient();
        }
    }
}
```

Aşağıdaki kod parçacığında, eklenen hizmetin varlık sınıfınıza nasıl dahil olduğu gösterilmektedir.

```csharp
public class HttpEntity
{
    [JsonIgnore]
    private readonly HttpClient client;

    public class HttpEntity(IHttpClientFactory factory)
    {
        this.client = factory.CreateClient();
    }

    public Task<int> GetAsync(string url)
    {
        using (var response = await this.client.GetAsync(url))
        {
            return (int)response.StatusCode;
        }
    }

    [FunctionName(nameof(HttpEntity))]
    public static Task Run([EntityTrigger] IDurableEntityContext ctx)
        => ctx.DispatchAsync<HttpEntity>();
}
```

> [!NOTE]
> Serileştirme ile ilgili sorunlardan kaçınmak için, eklenen değerleri Serileştirmeden depolayabileceği alanları dışdığınızdan emin olun.

> [!NOTE]
> Normal .NET Azure Işlevlerinde Oluşturucu Ekleme kullanmanın aksine, sınıf *tabanlı varlıkların işlevler* giriş noktası yöntemi bildirilmelidir `static` . Statik olmayan bir işlev giriş noktası bildirmek, normal Azure Işlevleri nesne Başlatıcısı ve dayanıklı varlıklar nesne Başlatıcısı arasında çakışmalara neden olabilir.

## <a name="function-based-syntax"></a>İşlev tabanlı sözdizimi

Şimdiye kadar, çoğu uygulama için daha uygun olması beklendiğinden, sınıf tabanlı sözdizimine odaklandık. Ancak, işlev tabanlı sözdizimi, varlık durumu ve işlemleri için kendi soyutlamalarını tanımlamak veya yönetmek isteyen uygulamalar için uygun olabilir. Ayrıca, şu anda sınıf tabanlı sözdizimi tarafından desteklenmeyen, genericity gerektiren kitaplıkları uygularken uygun olabilir. 

İşlev tabanlı sözdizimi ile Entity Işlevi, işlem gönderimi açıkça işler ve varlığın durumunu açıkça yönetir. Örneğin, aşağıdaki kod, işlev tabanlı sözdizimi kullanılarak uygulanan *sayaç* varlığını gösterir.  

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
        case "delete":
            ctx.DeleteState();
            break;
    }
}
```

### <a name="the-entity-context-object"></a>Varlık bağlam nesnesi

Varlığa özgü işlevlere, türünde bir bağlam nesnesi aracılığıyla erişilebilir `IDurableEntityContext` . Bu bağlam nesnesi, varlık işlevine parametre olarak ve Async-Local özelliği aracılığıyla kullanılabilir `Entity.Current` .

Aşağıdaki Üyeler geçerli işlem hakkında bilgi sağlar ve bir dönüş değeri belirtmemizi sağlar. 

* `EntityName`: Şu anda yürütülmekte olan varlığın adı.
* `EntityKey`: Şu anda yürütülmekte olan varlığın anahtarı.
* `EntityId`: Şu anda yürütülmekte olan varlığın KIMLIĞI (ad ve anahtar içerir).
* `OperationName`: geçerli işlemin adı.
* `GetInput<TInput>()`: geçerli işlem için girişi alır.
* `Return(arg)`: işlemi çağıran düzenleme için bir değer döndürür.

Aşağıdaki Üyeler varlığın durumunu yönetir (oluşturma, okuma, güncelleştirme, silme). 

* `HasState`: varlığın var olup olmadığı, yani, bir durumu var. 
* `GetState<TState>()`: varlığın geçerli durumunu alır. Henüz yoksa, oluşturulur.
* `SetState(arg)`: varlığın durumunu oluşturur veya güncelleştirir.
* `DeleteState()`: varsa varlığın durumunu siler. 

Tarafından döndürülen durum `GetState` bir nesnedir, uygulama kodu tarafından doğrudan değiştirilebilir. `SetState`Sonda (aynı zamanda hiçbir zarar olmadan) tekrar çağrılmaya gerek yoktur. `GetState<TState>`Birden çok kez çağrılırsa, aynı tür kullanılmalıdır.

Son olarak, aşağıdaki Üyeler diğer varlıkları işaret etmek veya yeni düzenlemeler başlatmak için kullanılır:

* `SignalEntity(EntityId, operation, input)`: bir varlığa tek yönlü bir ileti gönderir.
* `CreateNewOrchestration(orchestratorFunctionName, input)`: yeni bir düzenleme başlatır.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Varlık kavramları hakkında bilgi edinin](durable-functions-entities.md)
