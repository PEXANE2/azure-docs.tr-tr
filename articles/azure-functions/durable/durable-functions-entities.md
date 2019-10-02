---
title: Dayanıklı varlıklar-Azure Işlevleri
description: Dayanıklı varlıkların ne olduğunu ve Azure Işlevleri için Dayanıklı İşlevler uzantısı 'nda nasıl kullanılacağını öğrenin.
services: functions
author: cgillum
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.topic: overview
ms.date: 08/31/2019
ms.author: azfuncdf
ms.openlocfilehash: 06dfa40b6f320646513ab759f0ad5f4d10790236
ms.sourcegitcommit: a19f4b35a0123256e76f2789cd5083921ac73daf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71719989"
---
# <a name="entity-functions-preview"></a>Varlık işlevleri (Önizleme)

Varlık işlevleri, *dayanıklı varlıklar*olarak bilinen küçük durum parçalarını okumak ve güncelleştirmek için işlemleri tanımlar. Orchestrator işlevleri gibi, varlık işlevleri de özel tetikleyici türü, *varlık tetikleyicisi*olan işlevlerdir. Orchestrator işlevlerinin aksine, varlık işlevlerinin belirli kod kısıtlamaları yoktur. Varlık işlevleri, durumu denetim akışı aracılığıyla örtük olarak temsil etmek yerine, durumu açıkça da yönetir.

> [!NOTE]
> Varlık işlevleri ve ilgili işlevler yalnızca Dayanıklı İşlevler 2,0 ve üzeri sürümlerde kullanılabilir. Varlık işlevleri şu anda genel önizlemededir.

## <a name="entity-identity"></a>Varlık kimliği

Varlıklar (bazen varlık *örnekleri*olarak adlandırılır) benzersiz bir tanımlayıcı, *varlık kimliği*aracılığıyla erişilir. Bir varlık KIMLIĞI, bir varlık örneğini benzersiz bir şekilde tanımlayan dizelerin bir çiftidir. Aşağıdakilerden oluşur:

* Bir **varlık adı**: varlığın türünü tanımlayan bir ad (örneğin, "Counter").
* Bir **varlık anahtarı**: varlığı, aynı ada sahip diğer tüm varlıklar arasında benzersiz şekilde tanımlayan bir dize (örneğin, bir GUID).

Örneğin, bir *sayaç* varlığı işlevi çevrimiçi bir oyunda puanı korumak için kullanılabilir. Oyunun her örneği, `@Counter@Game1`, `@Counter@Game2` gibi benzersiz bir varlık KIMLIĞINE sahip olur. Belirli bir varlığı hedefleyen tüm işlemler, bir varlık KIMLIĞINI parametre olarak belirtmeyi gerektirir.

## <a name="programming-models"></a>Programlama modelleri

Dayanıklı varlıklar iki farklı programlama modelini destekler. İlk model, varlığın tek bir işlevle tanımlandığı dinamik bir "işlevsel" modeldir. İkinci model, varlığın bir sınıf ve yöntemlerle tanımlandığı nesne odaklı bir modeldir. Bu modeller ve varlıklarla etkileşim için programlama modelleri sonraki bölümlerde açıklanmıştır.

### <a name="defining-entities"></a>Varlıkları tanımlama

Dayanıklı varlıklar yazmak için iki isteğe bağlı programlama modeli vardır. Aşağıdaki kod, standart bir işlev olarak uygulanan basit bir *sayaç* varlığına bir örnektir. Bu işlev, `add`, `reset` ve `get`, her biri bir tamsayı durum değeri üzerinde çalışan üç *işlem*tanımlar, `currentValue`.

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

Bu model basit varlık uygulamaları veya dinamik bir işlem kümesi olan uygulamalar için en iyi şekilde kullanılır. Ancak, statik olan ancak daha karmaşık uygulamalar içeren varlıklar için kullanışlı olan sınıf tabanlı bir programlama modeli de kullanabilirsiniz. Aşağıdaki örnek, sınıfları ve yöntemleri kullanarak `Counter` varlığının eşdeğer bir uygulamasıdır.

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
> @No__t-0 özniteliğine sahip işlev giriş noktası yöntemi, varlık sınıfları kullanılırken-2 *@no__t bildirilmelidir.* Statik olmayan giriş noktası yöntemleri, birden fazla nesne başlatmaya ve olasılıkla diğer tanımsız davranışlara neden olabilir.

Sınıf tabanlı programlama modelinde, `IDurableEntityContext` nesnesi `Entity.Current` statik özelliğinde kullanılabilir.

Sınıf tabanlı model, [Orleans](https://www.microsoft.com/research/project/orleans-virtual-actors/)tarafından popularile benzerdir. Bu modelde, bir varlık türü .NET sınıfı olarak tanımlanmıştır. Sınıfının her yöntemi, bir dış istemci tarafından çağrılabilen bir işlemdir. Ancak, Orleans 'un aksine .NET arabirimleri isteğe bağlıdır. Önceki *sayaç* örneği bir arabirim kullanmadı, ancak yine de diğer IŞLEVLER veya HTTP API çağrıları aracılığıyla çağrılabilir.

> [!NOTE]
> Varlık tetikleyicisi işlevleri Dayanıklı İşlevler 2,0 ve üzeri sürümlerde kullanılabilir. Şu anda, varlık tetikleyici işlevleri yalnızca .NET işlev uygulamaları için kullanılabilir.

### <a name="accessing-entities-from-clients"></a>İstemcilerden varlıklara erişme

Dayanıklı varlıklar, *istemci işlevi* olarak da bilinen sıradan işlevlerden çağrılabilir veya sorgulanabilir ve [varlık istemci çıkış bağlaması](durable-functions-bindings.md#entity-client)kullanılarak bilinir. Aşağıdaki örnek, bu bağlamayı kullanarak bir varlığı *işaret* eden kuyruk tarafından tetiklenen bir işlev gösterir.

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

> [!NOTE]
> .NET işlevleri, sinyal varlıkları için hem gevşek olarak yazılmış hem de tür açısından güvenli yöntemleri destekler. Ayrıntılar için [varlık istemci bağlama](durable-functions-bindings.md#entity-client-usage) başvurusu belgelerine bakın.

*Sinyal* terimi, varlık API çağrısı tek yönlü ve zaman uyumsuz olduğu anlamına gelir. Bir *istemci işlevinin* , varlığın işlemi ne zaman işlediğini bilmesi veya bir varlık işlevinin bir istemci işlevine değer döndürmesi mümkün değildir. Tek yönlü kuyruk tabanlı mesajlaşma, dayanıklı varlıkların performans üzerinde dayanıklılığa öncelik vermek için bilerek bir tasarım seçimiydi. Bu tasarım seçeneği, diğer benzer teknolojiler ile karşılaştırıldığında dayanıklı varlıkların dengelarından biridir. Şu anda yalnızca düzenlemeler, sonraki bölümde açıklandığı gibi varlıklardan dönüş değerlerini işleyebilir.

Aşağıdaki örnekte gösterildiği gibi, istemci işlevleri de varlıkların durumunu sorgulayabilir:

```csharp
[FunctionName("QueryCounter")]
public static async Task<HttpResponseMessage> Run(
    [HttpTrigger(AuthorizationLevel.Function)] HttpRequestMessage req,
    [DurableClient] IDurableEntityClient client)
{
    var entityId = new EntityId(nameof(Counter), "myCounter");
    JObject state = await client.ReadEntityStateAsync<JObject>(entityId);
    return req.CreateResponse(HttpStatusCode.OK, state);
}
```

Varlık durumu sorguları, dayanıklı izleme deposuna gönderilir ve varlığın en son *kalıcı* durumunu döndürür. Döndürülen durum, varlığın bellek içi durumuna kıyasla eski olabilir. Aşağıdaki bölümde açıklandığı gibi, yalnızca düzenlemeler bir varlığın bellek içi durumunu okuyabilir.

### <a name="accessing-entities-from-orchestrations"></a>Organize lerden varlıklara erişme

Orchestrator işlevleri, [düzenleme tetikleyicisi bağlamasında](durable-functions-bindings.md#orchestration-trigger)API 'leri kullanarak varlıklara erişebilir. Orchestrator işlevleri tek yönlü iletişim ( *sinyal*olarak da adlandırılır) ve iki yönlü iletişim ( *çağrı*olarak da anılır) arasında seçim yapabilir. Aşağıdaki örnek kod, bir *sayaç* varlığını *çağıran* ve *işaret* eden bir Orchestrator işlevini gösterir.

```csharp
[FunctionName("CounterOrchestration")]
public static async Task Run(
    [OrchestrationTrigger] IDurableOrchestrationContext context)
{
    var entityId = new EntityId(nameof(Counter), "myCounter");

    // Synchronous call to the entity which returns a value
    int currentValue = await context.CallEntityAsync<int>(entityId, "Get");
    if (currentValue < 10)
    {
        // Asynchronous call which updates the value
        await context.SignalEntityAsync<int>(entityId, "Add", 1);
    }
}
```

Yalnızca düzenlemeler, varlık çağırma ve bir dönüş değeri ya da özel durum olabilen bir yanıt alma yeteneğine sahiptir. [İstemci bağlamasını](durable-functions-bindings.md#entity-client) kullanan istemci işlevleri yalnızca varlıklara *sinyal* verebilir.

> [!NOTE]
> Bir orchestrtor işlevinden bir varlığı çağırmak, bir Orchestrator işlevinden [etkinlik işlevi](durable-functions-types-features-overview.md#activity-functions) çağırmaya benzerdir. Temel fark, varlık işlevlerinin bir adresle ( *VARLıK kimliği*) dayanıklı nesneler olması ve bir işlem adının belirtilmesini desteklemeleridir. Diğer taraftan etkinlik işlevleri durum bilgisiz değildir ve işlem kavramına sahip değildir.

### <a name="dependency-injection-in-entity-classes-net"></a>Varlık sınıflarında bağımlılık ekleme (.NET)

Varlık sınıfları [Azure Işlevleri bağımlılığı ekleme](../functions-dotnet-dependency-injection.md)işlemini destekler. Aşağıdaki örnekte, bir `IHttpClientFactory` hizmetinin sınıf tabanlı bir varlığa nasıl kaydedileceği gösterilmektedir.

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

    // The function entry point must be declared static
    [FunctionName(nameof(HttpEntity))]
    public static Task Run([EntityTrigger] IDurableEntityContext ctx)
        => ctx.DispatchAsync<HttpEntity>();
}
```

> [!NOTE]
> Normal .NET Azure Işlevlerinde Oluşturucu Ekleme kullanmanın aksine, sınıf tabanlı varlıkların işlevler giriş noktası yöntemi- *1 olarak bildirilmelidir* @no__t. Statik olmayan bir işlev giriş noktası bildirmek, normal Azure Işlevleri nesne Başlatıcısı ve dayanıklı varlıklar nesne Başlatıcısı arasında çakışmalara neden olabilir.

### <a name="bindings-in-entity-classes-net"></a>Varlık sınıflarında bağlamalar (.NET)

Normal işlevlerin aksine, varlık sınıfı yöntemlerinin giriş ve çıkış bağlamalarına doğrudan erişimi yoktur. Bunun yerine, bağlama verileri giriş noktası işlev bildiriminde yakalanmalı ve sonra `DispatchAsync<T>` yöntemine geçirilmelidir. @No__t-0 ' a geçirilen tüm nesneler, bağımsız değişken olarak varlık sınıfı oluşturucusuna otomatik olarak geçirilir.

Aşağıdaki örnek, [BLOB giriş bağlamasındaki](../functions-bindings-storage-blob.md#input) bir `CloudBlobContainer` başvurusunun, sınıf tabanlı bir varlık için nasıl kullanılabilir hale getirilebilir olduğunu gösterir.

```csharp
public class BlobBackedEntity
{
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

## <a name="entity-coordination"></a>Varlık düzenlemesi

Birden çok varlık arasında işlem koordine etmeniz gerektiğinde zaman alabilir. Örneğin, bir bankacılık uygulamasında, tek tek banka hesaplarını temsil eden varlıklara sahip olabilirsiniz. Fonları bir hesaptan diğerine aktarırken, _kaynak_ hesapta yeterli fon bulunduğundan ve hem _kaynak_ hem de _hedef_ hesaplara yapılan güncelleştirmelerin işlem temelli olarak tutarlı bir şekilde yapıldığından emin olmanız gerekir.

### <a name="transfer-funds-example-in-c"></a>Fonları buraya aktarC#

Aşağıdaki örnek kod, bir Orchestrator işlevi kullanarak iki _Hesap_ varlığı arasındaki fonları aktarır. Varlık güncelleştirmelerini koordine etmek, düzenleme içinde _kritik bir bölüm_ oluşturmak için `LockAsync` yönteminin kullanılmasını gerektirir:

> [!NOTE]
> Kolaylık olması için bu örnek, daha önce tanımlanan `Counter` varlığını yeniden kullanır. Ancak gerçek bir uygulamada, daha ayrıntılı bir `BankAccount` varlığı tanımlanması daha iyi olacaktır.

```csharp
// This is a method called by an orchestrator function
public static async Task<bool> TransferFundsAsync(
    string sourceId,
    string destinationId,
    int transferAmount,
    IDurableOrchestrationContext context)
{
    var sourceEntity = new EntityId(nameof(Counter), sourceId);
    var destinationEntity = new EntityId(nameof(Counter), destinationId);

    // Create a critical section to avoid race conditions.
    // No operations can be performed on either the source or
    // destination accounts until the locks are released.
    using (await context.LockAsync(sourceEntity, destinationEntity))
    {
        ICounter sourceProxy = 
            context.CreateEntityProxy<ICounter>(sourceEntity);
        ICounter destinationProxy =
            context.CreateEntityProxy<ICounter>(destinationEntity);

        int sourceBalance = await sourceProxy.Get();

        if (sourceBalance >= transferAmount)
        {
            await sourceProxy.Add(-transferAmount);
            await destinationProxy.Add(transferAmount);

            // the transfer succeeded
            return true;
        }
        else
        {
            // the transfer failed due to insufficient funds
            return false;
        }
    }
}
```

.NET ' te, `LockAsync`, aktiften çıkarıldığı zaman kritik bölümü sonlandıran bir `IDisposable` döndürür. Bu `IDisposable` sonucu, kritik bölümün sözdizimsel bir gösterimini elde etmek için bir `using` bloğu ile birlikte kullanılabilir.

Önceki örnekte, bir Orchestrator işlevi bir _kaynak_ varlıktan bir _hedef_ varlığa fon aktardı. @No__t-0 yöntemi hem _kaynak_ hem de _hedef_ hesap varlıklarını kilitlediği. Bu, düzenleme mantığı `using` ifadesinin sonundaki _önemli bölümden_ çıkana kadar, başka hiçbir istemcinin herhangi bir hesabın durumunu sorgulayamaması veya değiştirememesi için bu kilidi kilitler. Bu, _kaynak_ hesaptan daha fazla taslak oluşturma olasılığını ortadan kaldırmayacak şekilde engelledi.

### <a name="critical-section-behavior"></a>Kritik bölüm davranışı

@No__t-0 yöntemi bir düzenleme içinde _kritik bir bölüm_ oluşturur. Bu _kritik bölümler_ , diğer düzenlemeler için, belirli bir varlık kümesinde çakışan değişiklikler yapılmasını engeller. Dahili olarak, `LockAsync` API 'SI varlıklara "kilit" işlemleri gönderir ve aynı varlıkların her birinden "kilit alındı" yanıt iletisi aldığında bunu döndürür. Hem *kilit* hem de *kilit açma* , tüm varlıkların desteklediği yerleşik işlemlerdir.

Kilitli durumda olan bir varlıkta diğer istemcilerden herhangi bir işlem yapılmasına izin verilmez. Bu davranış, tek seferde yalnızca bir düzenleme örneğinin bir varlığı kilitleyebilmesini sağlar. Bir çağıran bir düzenleme tarafından kilitliyken bir varlık üzerinde bir işlem çağırmaya çalışırsa, bu işlem *bekleyen bir işlem kuyruğuna*yerleştirilir. Bekleyen bir işlem, ' ın düzenleme kilidini serbest bırakana kadar işlenmeyecektir.

> [!NOTE] 
> Bu, içindeki C#`lock` açıklaması gibi çoğu programlama dilinde kullanılan eşitleme temel elemanlarından biraz farklıdır. Örneğin, ' de C#, `lock` ifadesinin birden çok iş parçacığında doğru bir şekilde eşitlenip emin olmak için tüm iş parçacıkları tarafından kullanılması gerekir. Ancak varlıklar, tüm çağıranların bir varlığı açıkça _kilitlemesine_ gerek yoktur. Herhangi bir çağıran bir varlığı kilitlerse, o varlıktaki tüm diğer işlemler engellenir ve bu kilidin arkasında sıraya alınır.

Varlıkların kilitleri dayanıklı olduğundan, yürütülmekte olan işlem geri dönüştürülüp bile devam ederler. Kilitler, bir varlığın dayanıklı durumunun bir parçası olarak dahili olarak kalıcı hale getirilir.

### <a name="critical-section-restrictions"></a>Kritik bölüm kısıtlamaları

Kritik bölümlerin nasıl kullanılabileceği konusunda çeşitli kısıtlamalar sunuyoruz. Bu kısıtlamalar kilitlenmeleri ve yeniden giriş yapılmasını engeller.

* Kritik bölümler iç içe geçirilemez.
* Kritik bölümler, alt düzenlemeler oluşturamaz.
* Kritik bölümler, yalnızca kilitlediği varlıkları çağırabilir.
* Kritik bölümler birden çok paralel çağrı kullanarak aynı varlığı çağıramaz.
* Kritik bölümler yalnızca kilitlendikleri varlıkları işaret edebilir.

## <a name="comparison-with-virtual-actors"></a>Sanal aktörler ile karşılaştırma

Birçok dayanıklı varlık özelliği [aktör modeli](https://en.wikipedia.org/wiki/Actor_model)tarafından ilham olarak sağlanır. Aktörlerle zaten bilgi sahibiyseniz, bu makalede açıklanan kavramların birçoğunu de tanıyabilirsiniz. Özellikle, dayanıklı varlıklar [sanal aktörlerin](https://research.microsoft.com/projects/orleans/) çoğunu birçok şekilde benzerdir:

* Dayanıklı varlıklar bir *VARLıK kimliği*aracılığıyla adreslidir.
* Yarış durumlarını engellemek için, sürekli varlık işlemleri tek seferde bir kez yürütülür.
* Dayanıklı varlıklar, çağrıldığında veya sinyalde olduğunda otomatik olarak oluşturulur.
* İşlemler yürütümemekte, kalıcı varlıkların belleği sessizce kaldırılır.

Ancak, dikkat edilmesi gereken bazı önemli farklılıklar vardır:

* Dayanıklı varlıklar *dayanıklılığı* *gecikme süresine*göre önceliklendirmez ve bu nedenle, katı gecikme süresi gereksinimleri olan uygulamalar için uygun olmayabilir.
* Varlıklar arasında gönderilen iletiler güvenilir bir şekilde ve sırayla dağıtılır.
* Dayanıklı varlıklar, dayanıklı düzenlemeler ile birlikte kullanılabilir ve dağıtılmış kilitleme mekanizmalarını destekler.
* Varlıklarda istek/yanıt desenleri, düzenleme ile sınırlıdır. *İstemci-varlık* ve *varlık* arası iletişim için, özgün aktör modelinde olduğu gibi yalnızca tek yönlü mesajlaşmaya ("sinyal" olarak da bilinir) izin verilir. Bu davranış, dağıtılmış kilitlenmeleri engeller.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Görev hub 'ları hakkında bilgi edinin](durable-functions-task-hubs.md)
