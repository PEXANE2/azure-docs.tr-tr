---
title: .NET - Azure İşlevlerinde Dayanıklı Varlıklar Için Geliştirici Kılavuzu
description: Azure İşlevler için Dayanıklı İşlevler uzantısı ile .NET'te dayanıklı varlıklarla nasıl çalışılı.
author: sebastianburckhardt
ms.topic: conceptual
ms.date: 10/06/2019
ms.author: azfuncdf
ms.openlocfilehash: 01e07eaee705634b03cc4462c4058e290daa8bc2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79278135"
---
# <a name="developers-guide-to-durable-entities-in-net"></a>.NET'te dayanıklı varlıklar için geliştirici kılavuzu

Bu makalede, .NET'e sahip dayanıklı varlıklar geliştirmek için mevcut arabirimleri örnekler ve genel tavsiyeler dahil olmak üzere ayrıntılı olarak açıklıyoruz. 

Varlık işlevleri, sunucusuz uygulama geliştiricilere, ince taneli varlıklar topluluğu olarak uygulama durumunu düzenlemek için kullanışlı bir yol sağlar. Temel kavramlar hakkında daha fazla ayrıntı için [Dayanıklı Varlıklar: Kavramlar](durable-functions-entities.md) makalesine bakın.

Şu anda varlıkları tanımlamak için iki API sunuyoruz:

- **Sınıf tabanlı sözdizimi,** varlıkları ve işlemleri sınıflar ve yöntemler olarak temsil eder. Bu sözdizimi kolayca okunabilir kod üretir ve işlemlerin arabirimler aracılığıyla tür işaretli bir şekilde çağrılmasını sağlar. 

- **İşlev tabanlı sözdizimi,** varlıkları işlev olarak temsil eden alt düzey bir arabirimdir. Varlık işlemlerinin nasıl gönderildiği ve varlık durumunun nasıl yönetildiği üzerinde hassas denetim sağlar.  

Bu makalede, çoğu uygulama için daha uygun olmasını beklediğimiz için öncelikle sınıf tabanlı sözdizimine odaklanın. Ancak, [işlev tabanlı sözdizimi,](#function-based-syntax) varlık durumu ve işlemleri için kendi soyutlamalarını tanımlamak veya yönetmek isteyen uygulamalar için uygun olabilir. Ayrıca, genellik gerektiren kitaplıkları uygulamak için uygun olabilir, sınıf tabanlı sözdizimi tarafından desteklenmeyen. 

> [!NOTE]
> Sınıf tabanlı sözdizimi, işlev tabanlı sözdiziminin üstündeki bir katmandır, bu nedenle her iki türde de aynı uygulamada birbirinin yerine kullanılabilir. 
 
## <a name="defining-entity-classes"></a>Varlık sınıflarını tanımlama

`Counter` Aşağıdaki örnek, tür tamsayı tek bir değer depolar ve dört işlem `Add` `Reset`, `Get`, `Delete`, ve .

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

İşlev, `Run` sınıf tabanlı sözdizimini kullanmak için gereken ortak plakayı içerir. *Statik* bir Azure İşlevi olmalıdır. Varlık tarafından işlenen her işlem iletisi için bir kez yürütür. Çağrıldığında `DispatchAsync<T>` ve varlık bellekte zaten yoksa, bir tür `T` nesnesi inşa eder ve depolamada bulunan son kalıcı JSON'dan (varsa) alanlarını doldurur. Sonra eşleşen adı ile yöntem çağırır.

> [!NOTE]
> Sınıf tabanlı bir varlığın durumu, varlık bir işlemi işleyinceden önce **örtülü olarak oluşturulur** ve bir işlemde '' çağırılarak `Entity.Current.DeleteState()`açıkça **silinebilir.**

### <a name="class-requirements"></a>Sınıf Gereksinimleri
 
Varlık sınıfları, özel üst sınıflar, arabirimler veya öznitelikler gerektirmeyen POCOs 'lar (düz eski CLR nesneleri) içerir. Ancak:

- Sınıf inşa edilebilir olmalıdır (bkz. [Varlık yapısı).](#entity-construction)
- Sınıf JSON serializable olmalıdır (Bkz. [Varlık serileştirme).](#entity-serialization)

Ayrıca, bir işlem olarak çağrılması amaçlanan herhangi bir yöntem ek gereksinimleri karşılamalıdır:

- Bir işlem en fazla bir bağımsız değişkene sahip olmalıdır ve herhangi bir aşırı yükleme veya genel tür bağımsız değişkenleri olmamalıdır.
- Bir arabirim kullanarak bir orkestrasyondan çağrılması gereken bir işlem geri dönmelidir `Task` veya `Task<T>`.
- Bağımsız değişkenler ve iade değerleri serileştirilebilir değerler veya nesneler olmalıdır.

### <a name="what-can-operations-do"></a>Operasyonlar ne yapabilir?

Tüm varlık işlemleri varlık durumunu okuyabilir ve güncelleyebilir ve durumdaki değişiklikler otomatik olarak depolamaya devam edilir. Ayrıca, işlemler tüm Azure İşlevlerinin ortak genel sınırları dahilinde harici G/Ç veya diğer hesaplamaları gerçekleştirebilir.

İşlemler ayrıca bağlam tarafından `Entity.Current` sağlanan işlevsellik erişimine de sahiptir:

* `EntityName`: şu anda icra eden varlığın adı.
* `EntityKey`: şu anda çalıştırılamakta olan varlığın anahtarı.
* `EntityId`: şu anda çalıştırılamakta olan varlığın kimliği (ad ve anahtar içerir).
* `SignalEntity`: bir varlığa tek yönlü bir ileti gönderir.
* `CreateNewOrchestration`: yeni bir orkestrasyon başlatır.
* `DeleteState`: bu varlığın durumunu siler.

Örneğin, sayaç 100'e ulaştığında ve varlık kimliğini giriş bağımsız değişkeni olarak geçirince bir düzenleme başlatabilmesi için karşı varlığı değiştirebiliriz:

```csharp
    public void Add(int amount) 
    {
        if (this.Value < 100 && this.Value + amount > 100)
        {
            Entity.Current.StartNewOrchestration("MilestoneReached", Entity.Current.EntityId)
        }
        this.Value += amount;      
    }
```

## <a name="accessing-entities-directly"></a>Varlıklara doğrudan erişim

Sınıf tabanlı varlıklar, varlık ve işlemleri için açık dize adlarını kullanarak doğrudan erişilebilir. Aşağıda bazı örnekler vermekteyiz; temel kavramların daha derin bir açıklaması için (sinyaller ve aramalar gibi) [Access varlıklarındaki](durable-functions-entities.md#access-entities)tartışmaya bakın. 

> [!NOTE]
> Mümkün olduğunda, daha fazla tür denetimi sağladığından, [varlıklara arabirimler aracılığıyla erişmenizi](#accessing-entities-through-interfaces)öneririz.

### <a name="example-client-signals-entity"></a>Örnek: istemci varlık sinyalleri

Aşağıdaki Azure Http İşlevi, REST kurallarını kullanarak bir DELETE işlemi uygular. Anahtarı URL yolunda geçirilen karşı varlığa silme sinyali gönderir.

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

Aşağıdaki Azure Http İşlevi, REST kurallarını kullanarak bir GET işlemi uygular. AnahtarURL yolunda geçirilen karşı varlığın geçerli durumunu okur.

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
> Döndürülen `ReadEntityStateAsync` nesne yalnızca yerel bir kopyadır, yani zaman içinde daha önceki bir noktadan varlık durumunun anlık görüntüsüdür. Özellikle, eski olabilir ve bu nesneyi değiştirmenin gerçek varlık üzerinde hiçbir etkisi yoktur. 

### <a name="example-orchestration-first-signals-then-calls-entity"></a>Örnek: orkestrasyon ilk sinyalleri, sonra varlık çağırır

Aşağıdaki orkestrasyon, karşı bir varlığın artışını bildirir ve ardından aynı varlığı en son değerini okumaya çağırır.

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

## <a name="accessing-entities-through-interfaces"></a>Arayüzler aracılığıyla varlıklara erişim

Arabirimler, oluşturulan proxy nesneleri aracılığıyla varlıklara erişmek için kullanılabilir. Bu yaklaşım, bir işlemin ad ve bağımsız değişken türünün uygulananla eşleşmesini sağlar. Varlıklara mümkün olduğunca erişmek için arabirimleri kullanmanızı öneririz.

Örneğin, sayaç örneğini aşağıdaki gibi değiştirebiliriz:

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

Varlık sınıfları ve varlık arabirimleri [Orleans](https://www.microsoft.com/research/project/orleans-virtual-actors/)tarafından popüler taneler ve tahıl arabirimleri benzer. Dayanıklı Varlıklar ve Orleans arasındaki benzerlikler ve farklılıklar hakkında daha fazla bilgi için, [bkz.](durable-functions-entities.md#comparison-with-virtual-actors)

Tür denetimi sağlamanın yanı sıra, arabirimler uygulama içindeki endişelerin daha iyi ayrılması için yararlıdır. Örneğin, bir varlık birden çok arabirim uygulayabildiği için, tek bir varlık birden çok rol sunabilir. Ayrıca, bir arabirim birden fazla varlık tarafından uygulanabileceğinden, genel iletişim kalıpları yeniden kullanılabilir kitaplıklar olarak uygulanabilir.

### <a name="example-client-signals-entity-through-interface"></a>Örnek: istemci, varlığı arabirim üzerinden bildirir

İstemci `SignalEntityAsync<TEntityInterface>` kodu, uygulayan `TEntityInterface`varlıklara sinyal göndermek için kullanabilir. Örnek:

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

Bu örnekte, `proxy` parametre dinamik olarak oluşturulan `ICounter`bir örneğidir , dahili bir sinyal içine `Delete` çağrı çevirir.

> [!NOTE]
> API'ler `SignalEntityAsync` yalnızca tek yönlü işlemler için kullanılabilir. Bir işlem dönse `Task<T>`bile, `T` parametrenin değeri `default`her zaman null veya , gerçek sonuç değil olacaktır.
Örneğin, hiçbir değer döndürülmediği için `Get` işlemi sinyalvermek mantıklı değildir. Bunun yerine, istemciler sayaç durumuna doğrudan erişmek `ReadStateAsync` için kullanabilir veya `Get` işlemi çağıran bir orchestrator işlevi başlatabilir. 

### <a name="example-orchestration-first-signals-then-calls-entity-through-proxy"></a>Örnek: orkestrasyon ilk sinyalleri, sonra proxy üzerinden varlık çağırır

Bir varlığı bir orkestrasyon içinden çağırmak veya işaret etmek için, `CreateEntityProxy` varlık için bir proxy oluşturmak için arabirim türüyle birlikte kullanılabilir. Bu proxy daha sonra arama veya sinyal işlemleri için kullanılabilir:

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

Dolaylı olarak, dönen `void` tüm işlemler sinyal verilir ve `Task` `Task<T>` geri dönen veya çağrılan işlemler. Bu varsayılan davranışı değiştirebilir ve yöntemi açıkça kullanarak Görev'i `SignalEntity<IInterfaceType>` döndürseler bile sinyal işlemleri ne kadar önemli olur.

### <a name="shorter-option-for-specifying-the-target"></a>Hedefi belirtmek için daha kısa seçenek

Arabirimi kullanarak bir varlığı ararken veya işaret ederken, ilk bağımsız değişken hedef varlığı belirtmelidir. Hedef, varlık kimliğini belirterek veya varlığı uygulayan tek bir sınıfın olduğu durumlarda, sadece varlık anahtarı belirtilerek belirtilebilir:

```csharp
context.SignalEntity<ICounter>(new EntityId(nameof(Counter), "myCounter"), ...);
context.SignalEntity<ICounter>("myCounter", ...);
```

Yalnızca varlık anahtarı belirtilirse ve çalışma zamanında benzersiz bir `InvalidOperationException` uygulama bulunamazsa, atılır. 

### <a name="restrictions-on-entity-interfaces"></a>Varlık arabirimlerindeki kısıtlamalar

Her zamanki gibi, tüm parametre ve iade türleri JSON-serializable olmalıdır. Aksi takdirde, serileştirme özel durumları çalışma zamanında atılır.

Ayrıca bazı ek kurallar da uyguluyoruz:
* Varlık arabirimleri yalnızca yöntemleri tanımlamalıdır.
* Varlık arabirimleri genel parametreler içermemelidir.
* Varlık arabirimi yöntemlerinin birden fazla parametresi olmamalıdır.
* Varlık arabirimi `void`yöntemleri, `Task`veya döndürme`Task<T>` 

Bu kurallardan herhangi biri ihlal `InvalidOperationException` edilirse, arabirim bir tür bağımsız değişkeni `SignalEntity` `CreateProxy`olarak kullanıldığında çalışma zamanında bir atılmıştır veya . Özel durum iletisi hangi kuralın kırıldığını açıklar.

> [!NOTE]
> Dönen `void` arabirim yöntemleri yalnızca sinyal (tek yönlü), çağrılabilir (iki yönlü). Arabirim yöntemleri `Task` `Task<T>` geri döner veya çağrılabilir veya sinyal verebilir. Çağrıldığı takdirde, işlemin sonucunu döndürer veya işlem tarafından atılan özel durumları yeniden atarlar. Ancak, sinyal verildiğinde, işlemden gerçek sonucu veya özel durumu değil, sadece varsayılan değeri döndürürler.

## <a name="entity-serialization"></a>Varlık serileştirme

Bir varlığın durumu kalıcı olduğundan, varlık sınıfı serileştirilebilir olmalıdır. Dayanıklı Işlevler çalışma süresi, serileştirme ve deserialization işlemini denetlemek için bir dizi ilke ve öznitelikleri destekleyen bu amaç için [Json.NET](https://www.newtonsoft.com/json) kitaplığını kullanır. En sık kullanılan C# veri türleri (diziler ve koleksiyon türleri dahil) zaten serileştirilebilir ve dayanıklı varlıkların durumunu tanımlamak için kolayca kullanılabilir.

Örneğin, Json.NET aşağıdaki sınıfı kolayca serileştirebilir ve deserialize edebilir:

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

### <a name="serialization-attributes"></a>Serileştirme Öznitelikleri

Yukarıdaki örnekte, temel serileştirmeyi daha görünür hale getirmek için birkaç öznitelik eklemeyi seçtik:
- Sınıfın serileştirilebilir olması gerektiğini `[JsonObject(MemberSerialization.OptIn)]` hatırlatmak ve yalnızca JSON özellikleri olarak açıkça işaretlenmiş üyeleri kalıcı hale getirmek için sınıfa açıklama ekine işaret ediyoruz.
-  Bir alanın kalıcı varlık durumunun bir `[JsonProperty("name")]` parçası olduğunu bize hatırlatmak ve JSON gösteriminde kullanılacak özellik adını belirtmek için kalıcı olması gereken alanlara açıklama ekine yer ediyoruz.

Ancak, bu öznitelikler gerekli değildir; diğer sözleşmelere veya niteliklere, Json.NET çalıştıkları sürece izin verilir. Örneğin, bir öznitelikleri kullanabilirsiniz, `[DataContract]` ya da hiç öznitelikleri:

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

Varsayılan olarak, sınıfın adı JSON gösteriminin bir parçası olarak *depolanmaz:* yani varsayılan ayar olarak kullanırız. `TypeNameHandling.None` Bu varsayılan davranış kullanılarak veya `JsonObject` `JsonProperty` öznitelikleri geçersiz kılınabilir.

### <a name="making-changes-to-class-definitions"></a>Sınıf tanımlarında değişiklik yapma

Depolanan JSON nesnesi artık yeni sınıf tanımıyla eşleşmeyebileceğinden, bir uygulama çalıştırıldıktan sonra sınıf tanımında değişiklik yaparken bazı bakımlar gereklidir. Yine de, bir tarafından `JsonConvert.PopulateObject`kullanılan deserialization işlemi anlar sürece veri biçimleri değişen ile doğru başa çıkmak genellikle mümkündür.

Örneğin, değişikliklerin bazı örnekleri ve bunların etkisi aşağıda verilmiştir:

1. Depolanan JSON'da bulunmayan yeni bir özellik eklenirse, varsayılan değerini varsayar.
1. Depolanan JSON'da bulunan bir özellik kaldırılırsa, önceki içerik kaybolur.
1. Bir özelliğin adı yeniden adlandırıldıysa, efekt eskisini kaldırıp yenisini eklemek gibidir.
1. Bir özelliğin türü, depolanan JSON'dan artık deserialed edilemeyecek şekilde değiştirilirse, bir özel durum atılır.
1. Bir özelliğin türü değiştirilirse, ancak yine de depolanan JSON'dan deserialed edilebilirse, bunu yapar.

Json.NET davranışını özelleştirmek için birçok seçenek vardır. Örneğin, depolanan JSON sınıfta bulunmayan bir alan içeriyorsa, bir özel durum `JsonObject(MissingMemberHandling = MissingMemberHandling.Error)`zorlamak için özniteliği belirtin. Ayrıca, rasgele biçimlerde depolanan JSON'u okuyabilen deserialization için özel kod yazmak da mümkündür.

## <a name="entity-construction"></a>Varlık inşaatı

Bazen varlık nesnelerinin nasıl oluşturuldukları üzerinde daha fazla denetim uygulamak isteriz. Şimdi varlık nesneleri inşa ederken varsayılan davranışı değiştirmek için çeşitli seçenekler açıklar. 

### <a name="custom-initialization-on-first-access"></a>İlk erişimde özel başlatma

Bazen, daha önce erişilememiş veya silinmiş bir varlığa bir işlem göndermeden önce bazı özel başlatma gerçekleştirmemiz gerekir. Bu davranışı belirtmek için, bir önce `DispatchAsync`koşullu ekleyebilirsiniz:

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

Normal işlevlerin aksine, varlık sınıfı yöntemleri giriş ve çıktı bağlamalarına doğrudan erişime sahip değildir. Bunun yerine, bağlama verileri giriş noktası işlev bildiriminde yakalanıp `DispatchAsync<T>` yönteme geçirilmelidir. Geçirilen `DispatchAsync<T>` tüm nesneler, bağımsız değişken olarak otomatik olarak varlık sınıfı oluşturucuya aktarılır.

Aşağıdaki örnek, `CloudBlobContainer` [blob giriş bağlamasından](../functions-bindings-storage-blob-input.md) bir başvurunun sınıf tabanlı bir varlık için nasıl kullanılabileceğini gösterir.

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

Azure İşlevler'deki bağlamalar hakkında daha fazla bilgi için [Azure İşi Tetikleyicileri ve Bağlamalar](../functions-triggers-bindings.md) belgelerine bakın.

### <a name="dependency-injection-in-entity-classes"></a>Varlık sınıflarında bağımlılık enjeksiyonu

Varlık sınıfları [Azure İşlevlerini Bağımlılık Enjeksiyonu'na](../functions-dotnet-dependency-injection.md)destekler. Aşağıdaki örnek, bir `IHttpClientFactory` hizmetin sınıf tabanlı bir varlığa nasıl kaydedilebildiğini gösterir.

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

Aşağıdaki parçacık, enjekte edilen hizmetin varlık sınıfınıza nasıl dahil edilebildiğini gösterir.

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
> Serileştirme yle ilgili sorunları önlemek için, enjekte edilen değerleri serileştirmeden depolamak için gereken alanları hariç tutun.

> [!NOTE]
> Normal .NET Azure İşlevlerini yapılandırmak için kullanılanların aksine, sınıf tabanlı *varlıklar* için işlevler giriş noktası yöntemi bildirilmelidir. `static` Statik olmayan bir işlev giriş noktası bildirmek, normal Azure İşlevleri nesnesi başlatılması ile Dayanıklı Varlıklar nesnesi başlatılması arasında çakışmalara neden olabilir.

## <a name="function-based-syntax"></a>İşlev tabanlı sözdizimi

Şimdiye kadar, çoğu uygulama için daha uygun olmasını beklediğimiz için sınıf tabanlı sözdizimine odaklandık. Ancak, işlev tabanlı sözdizimi, varlık durumu ve işlemleri için kendi soyutlamalarını tanımlamak veya yönetmek isteyen uygulamalar için uygun olabilir. Ayrıca, genellik gerektiren kitaplıkları uygularken sınıf tabanlı sözdizimi tarafından desteklenmeyen ler de uygun olabilir. 

İşlev tabanlı sözdizimi ile Varlık İşlevi, işlem sevkiyatını açıkça işler ve varlığın durumunu açıkça yönetir. Örneğin, aşağıdaki kod işlev tabanlı sözdizimi kullanılarak uygulanan *Karşı* varlık gösterir.  

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

Varlığa özgü işlevsellik türünden `IDurableEntityContext`bir bağlam nesnesi üzerinden erişilebilir. Bu bağlam nesnesi varlık işlevi için bir parametre olarak kullanılabilir `Entity.Current`ve async-yerel özellik üzerinden .

Aşağıdaki üyeler geçerli işlem hakkında bilgi sağlar ve bir iade değeri belirtmemize izin verir. 

* `EntityName`: şu anda icra eden varlığın adı.
* `EntityKey`: şu anda çalıştırılamakta olan varlığın anahtarı.
* `EntityId`: şu anda çalıştırılamakta olan varlığın kimliği (ad ve anahtar içerir).
* `OperationName`: geçerli işlemin adı.
* `GetInput<TInput>()`: geçerli işlem için girişi alır.
* `Return(arg)`: işlemi adı verilen orkestrasyona bir değer verir.

Aşağıdaki üyeler varlığın durumunu yönetir (oluşturma, oku, güncelle, sil). 

* `HasState`: varlığın var olup olmadığı, yani bir devlete sahip olması. 
* `GetState<TState>()`: varlığın geçerli durumunu alır. Zaten yoksa, oluşturulur.
* `SetState(arg)`: varlığın durumunu oluşturur veya günceller.
* `DeleteState()`: varsa varlığın durumunu siler. 

Döndürülen `GetState` durum bir nesneyse, doğrudan uygulama kodu tarafından değiştirilebilir. Sonunda tekrar aramaya `SetState` gerek yoktur (ama aynı zamanda zarar). Birden `GetState<TState>` çok kez çağrılırsa, aynı tür kullanılmalıdır.

Son olarak, aşağıdaki üyeler diğer varlıklara sinyal vermek veya yeni orkestrasyonlar başlatmak için kullanılır:

* `SignalEntity(EntityId, operation, input)`: bir varlığa tek yönlü bir ileti gönderir.
* `CreateNewOrchestration(orchestratorFunctionName, input)`: yeni bir orkestrasyon başlatır.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Varlık kavramları hakkında bilgi edinin](durable-functions-entities.md)
