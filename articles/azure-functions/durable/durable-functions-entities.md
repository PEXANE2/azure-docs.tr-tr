---
title: Dayanıklı varlıklar - Azure Fonksiyonları
description: Azure İşlevler için Dayanıklı Fonksiyonlar uzantısında dayanıklı varlıkların ne olduğunu ve bunları nasıl kullanacağınızı öğrenin.
author: cgillum
ms.topic: overview
ms.date: 12/17/2019
ms.author: azfuncdf
ms.openlocfilehash: 4f45ac40e7df865bdb4722d086325096c377cd59
ms.sourcegitcommit: 2d7910337e66bbf4bd8ad47390c625f13551510b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80877551"
---
# <a name="entity-functions"></a>Varlık işlevleri

Varlık işlevleri, *dayanıklı varlıklar*olarak bilinen küçük durum parçalarını okuma ve güncelleştirme işlemlerini tanımlar. Orchestrator işlevleri gibi, varlık işlevleri özel bir tetikleyici türü ile işlevler, *varlık tetikleyici*. Orchestrator işlevlerinin aksine, varlık işlevleri denetim akışı yoluyla durumu dolaylı olarak temsil etmek yerine bir varlığın durumunu açıkça yönetir.
Varlıklar, her biri mütevazı büyüklükte bir duruma sahip olan birçok kuruluşa iş dağıtarak uygulamaları ölçekleme aracı sağlar.

> [!NOTE]
> Varlık işlevleri ve ilgili işlevler yalnızca Dayanıklı Fonksiyonlar 2.0 ve üzeri işlevlerde kullanılabilir.

## <a name="general-concepts"></a>Genel kavramlar

Varlıklar, iletiler aracılığıyla iletişim sağlayan küçük hizmetler gibi biraz iyi bir şekilde birlikte olur. Her varlığın benzersiz bir kimliği ve bir iç durumu vardır (varsa). Hizmetler veya nesneler gibi, varlıklar istendiğinde işlemleri gerçekleştirir. Bir işlem yürütüldüğünde, varlığın iç durumunu güncelleştirebilir. Ayrıca harici hizmetleri arayabilir ve bir yanıt bekleyebilir. Varlıklar, güvenilir kuyruklar aracılığıyla dolaylı olarak gönderilen iletileri kullanarak diğer varlıklar, orkestrasyonlar ve istemcilerle iletişim kurar. 

Çakışmaları önlemek için, tek bir varlık üzerindeki tüm işlemlerin seri olarak, yani birbiri ardına yürütülmesi garanti edilir. 

### <a name="entity-id"></a>Varlık Kimliği
Varlıklara benzersiz bir tanımlayıcı, *varlık kimliği*üzerinden erişilir. Varlık kimliği, yalnızca bir varlık örneğini benzersiz olarak tanımlayan bir dize çiftidir. Bir oluşur:

* **Varlık adını,** varlığın türünü tanımlayan bir addır. Bir örnek "Sayaç"tır. Bu ad, varlığı uygulayan varlık işlevinin adıyla eşleşmelidir. Davaya karşı hassas değil.
* **Varlık anahtarı,** aynı adı taşıyan diğer tüm varlıklar arasında varlığı benzersiz olarak tanımlayan bir dizedir. Bir örnek bir GUID olduğunu.

Örneğin, bir `Counter` varlık işlevi bir çevrimiçi oyunda puanı tutmak için kullanılabilir. Oyunun her örneği gibi `@Counter@Game1` benzersiz bir varlık `@Counter@Game2`kimliği vardır. Belirli bir varlığı hedefleyen tüm işlemler, bir varlık kimliğini n parametre olarak belirtilmesi ni gerektirir.

### <a name="entity-operations"></a>Varlık işlemleri ###

Bir varlık üzerinde bir işlem çağırmak için şunları belirtin:

* Hedef varlığın **varlık kimliği.**
* **İşlem adı,** gerçekleştirmek için işlem belirten bir dize. Örneğin, `Counter` varlık , `add`veya `get` `reset` işlemleri destekleyebilir.
* İşlem için isteğe bağlı bir giriş parametresi olan **işlem girişi.** Örneğin, ekleme işlemi giriş olarak bir toplam miktarı alabilir.
* **Zamanlanan saat,** işlemin teslim süresini belirtmek için isteğe bağlı bir parametredir. Örneğin, bir işlem gelecekte birkaç gün çalışacak şekilde güvenilir bir şekilde zamanlanabilir.

İşlemler, JavaScript hatası veya .NET özel durumu gibi bir sonuç değeri veya hata sonucu döndürebilir. Bu sonuç veya hata işlemi denilen orkestrasyonlar tarafından gözlemlenebilir.

Bir varlık işlemi de oluşturabilir, okuyabilir, güncelleyebilir ve varlığın durumunu silebilir. Varlığın durumu her zaman kalıcı depolama devam edilir.

## <a name="define-entities"></a>Varlıkları tanımlama

Şu anda, varlıkları tanımlamak için iki farklı API'ler şunlardır:

Varlıkların işlev olarak temsil edildiği ve işlemlerin uygulama tarafından açıkça gönderildiği **işlev tabanlı sözdizimi.** Bu sözdizimi, basit durumu, birkaç işlemi veya uygulama çerçevelerinde olduğu gibi dinamik bir işlem kümesine sahip varlıklar için iyi çalışır. Bu sözdizimi, derleme zamanında tür hatalarını yakalamadığından, bakımı için sıkıcı olabilir.

Sınıf **tabanlı sözdizimi (.NET yalnızca)**, varlıklar ve işlemler sınıflar ve yöntemlerle temsil edilir. Bu sözdizimi daha kolay okunabilir kod üretir ve işlemlerin tür güvenli bir şekilde çağrılmasını sağlar. Sınıf tabanlı sözdizimi, işlev tabanlı sözdiziminin üstündeki ince bir katmandır, bu nedenle her iki varyant da aynı uygulamada birbirinin yerine kullanılabilir.

# <a name="c"></a>[C#](#tab/csharp)

### <a name="example-function-based-syntax---c"></a>Örnek: Fonksiyon tabanlı sözdizimi - C #

Aşağıdaki kod, dayanıklı bir `Counter` işlev olarak uygulanan basit bir varlığın bir örneğidir. Bu işlev, `add` `reset` `get`her biri tamsayı durumunda çalışan üç işlemi tanımlar.

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
            ctx.Return(ctx.GetState<int>());
            break;
    }
}
```

İşlev tabanlı sözdizimi ve nasıl kullanılacağı hakkında daha fazla bilgi için [Bkz. İşlev tabanlı sözdizimi.](durable-functions-dotnet-entities.md#function-based-syntax)

### <a name="example-class-based-syntax---c"></a>Örnek: Sınıf tabanlı sözdizimi - C #

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

Bu varlığın durumu, sayacın geçerli değerini depolayan bir alan içeren bir tür `Counter`nesnesidir. Bu nesneyi depolamada kalıcı hale getirmek [için, Json.NET](https://www.newtonsoft.com/json) kitaplığı tarafından seri hale getirilip deserialize edilir. 

Sınıf tabanlı sözdizimi ve nasıl kullanılacağı hakkında daha fazla bilgi için varlık [sınıflarını tanımlama'ya](durable-functions-dotnet-entities.md#defining-entity-classes)bakın.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

### <a name="example-javascript-entity"></a>Örnek: JavaScript varlığı

Dayanıklı varlıklar `durable-functions` NPM paketinin sürüm **1.3.0** ile başlayan JavaScript mevcuttur. Aşağıdaki kod JavaScript'te yazılmış dayanıklı bir işlev olarak uygulanan `Counter` varlıktır.

**Sayaç/fonksiyon.json**
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

**Sayaç/index.js**
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

---

## <a name="access-entities"></a>Erişim varlıkları

Varlıklara tek yönlü veya çift yönlü iletişim kullanılarak erişilebilir. Aşağıdaki terminoloji iki iletişim biçimini ayırır: 

* Bir varlığı **çağırmak,** iki yönlü (gidiş-dönüş) iletişim kullanır. Varlığa bir işlem iletisi gönderirsiniz ve devam etmeden önce yanıt iletisini beklersiniz. Yanıt iletisi, JavaScript hatası veya .NET özel durumu gibi bir sonuç değeri veya hata sonucu sağlayabilir. Bu sonuç veya hata daha sonra arayan tarafından gözlenir.
* **Bir** varlığı sinyal vermek tek yönlü (yangın ve unut) iletişimi kullanır. Bir işlem iletisi gönderirsiniz, ancak yanıt beklemezsiniz. İletinin sonunda teslim edilmesi garanti edilirken, gönderen ne zaman sonuç değerini veya hatalarını gözlemleyeceğini bilmez.

Varlıklara istemci işlevleri içinden, orkestratör işlevlerinden veya varlık işlevlerinin içinden erişilebilir. Tüm iletişim biçimleri tüm bağlamlar tarafından desteklenmez:

* İstemcilerin içinden, varlıklara sinyal verebilir ve varlık durumunu okuyabilirsiniz.
* Orkestrasyonların içinden, varlıklara sinyal verebilir ve varlıkları arayabilirsiniz.
* Varlıkların içinden, varlıklara sinyal verebilirsiniz.

Aşağıdaki örnekler, varlıklara erişmenin çeşitli yollarını göstermektedir.

### <a name="example-client-signals-an-entity"></a>Örnek: İstemci bir varlığı işaret ediyor

İstemci işlevi olarak da bilinen sıradan bir Azure İşlevi'nden varlıklara erişmek için [varlık istemcisi bağlamayı](durable-functions-bindings.md#entity-client)kullanın. Aşağıdaki örnek, bu bağlamayı kullanarak bir varlık sinyali bir kuyruk tetiklenen işlevi gösterir.

# <a name="c"></a>[C#](#tab/csharp)

> [!NOTE]
> Basitlik için aşağıdaki örnekler, varlıklara erişmek için gevşek olarak yazılan sözdizimini gösterir. Genel olarak, daha fazla tür denetimi [sağladığından, varlıklara arabirimler üzerinden erişmenizi](durable-functions-dotnet-entities.md#accessing-entities-through-interfaces) öneririz.

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

# <a name="javascript"></a>[JavaScript](#tab/javascript)

```javascript
const df = require("durable-functions");

module.exports = async function (context) {
    const client = df.getClient(context);
    const entityId = new df.EntityId("Counter", "myCounter");
    await client.signalEntity(entityId, "add", 1);
};
```

---

*Sinyal* terimi, varlık API çağırmasının tek yönlü ve eşzamanlı olduğu anlamına gelir. Bir istemci işlevinin varlığın işlemi ne zaman işlediğini bilmesi mümkün değildir. Ayrıca, istemci işlevi herhangi bir sonuç değerlerini veya özel durumlarını gözlemleyemez. 

### <a name="example-client-reads-an-entity-state"></a>Örnek: İstemci bir varlık durumunu okur

İstemci işlevleri, aşağıdaki örnekte gösterildiği gibi bir varlığın durumunu da sorgulayabilir:

# <a name="c"></a>[C#](#tab/csharp)

```csharp
[FunctionName("QueryCounter")]
public static async Task<HttpResponseMessage> Run(
    [HttpTrigger(AuthorizationLevel.Function)] HttpRequestMessage req,
    [DurableClient] IDurableEntityClient client)
{
    var entityId = new EntityId(nameof(Counter), "myCounter");
    EntityStateResponse<JObject> stateResponse = await client.ReadEntityStateAsync<JObject>(entityId);
    return req.CreateResponse(HttpStatusCode.OK, stateResponse.EntityState);
}
```

# <a name="javascript"></a>[JavaScript](#tab/javascript)

```javascript
const df = require("durable-functions");

module.exports = async function (context) {
    const client = df.getClient(context);
    const entityId = new df.EntityId("Counter", "myCounter");
    const stateResponse = await client.readEntityState(entityId);
    return stateResponse.entityState;
};
```

---

Varlık durumu sorguları Dayanıklı izleme deposuna gönderilir ve varlığın en son kalıcı durumunu döndürür. Bu durum her zaman "kararlı" bir durumdur, yani bir işlemi yürütmenin ortasında kabul edilen geçici bir ara durum değildir. Ancak, bu durum varlığın bellek durumu ile karşılaştırıldığında bayat olabilir. Aşağıdaki bölümde açıklandığı gibi, bir varlığın bellek durumunu yalnızca orkestrasyonlar okuyabilir.

### <a name="example-orchestration-signals-and-calls-an-entity"></a>Örnek: Orkestrasyon sinyalleri ve bir varlık çağırır

Orchestrator [işlevleri, düzenleme tetikleyici bağlamada](durable-functions-bindings.md#orchestration-trigger)API'ler kullanarak varlıklara erişebilir. Aşağıdaki örnek kod, bir `Counter` varlığı çağıran ve işaretleyen bir orkestratör işlevini gösterir.

# <a name="c"></a>[C#](#tab/csharp)

```csharp
[FunctionName("CounterOrchestration")]
public static async Task Run(
    [OrchestrationTrigger] IDurableOrchestrationContext context)
{
    var entityId = new EntityId(nameof(Counter), "myCounter");

    // Two-way call to the entity which returns a value - awaits the response
    int currentValue = await context.CallEntityAsync<int>(entityId, "Get");
    if (currentValue < 10)
    {
        // One-way signal to the entity which updates the value - does not await a response
        context.SignalEntity(entityId, "Add", 1);
    }
}
```

# <a name="javascript"></a>[JavaScript](#tab/javascript)

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context){
    const entityId = new df.EntityId("Counter", "myCounter");

    // Two-way call to the entity which returns a value - awaits the response
    currentValue = yield context.df.callEntity(entityId, "get");
});
```

> [!NOTE]
> JavaScript şu anda bir orchestrator bir varlık sinyal desteklemez. Bunun yerine `callEntity` kullanın.

---

Yalnızca orkestrasyonlar varlıkları arama ve bir yanıt alma yeteneğine sahiptir, bu da bir iade değeri veya özel bir durum olabilir. [İstemci bağlamakullanan](durable-functions-bindings.md#entity-client) istemci işlevleri yalnızca varlıkları işaretleyebilir.

> [!NOTE]
> Bir varlığı bir orkestratör işlevinden çağırmak, bir orkestratör işlevinden [etkinlik işlevini](durable-functions-types-features-overview.md#activity-functions) çağırmaya benzer. Temel fark, varlık işlevlerinin bir adrese sahip dayanıklı nesneler olmasıdır, yani varlık kimliğidir. Varlık işlevleri bir işlem adı belirten desteği. Etkinlik işlevleri ise devletsizdir ve işlem kavramına sahip değildir.

### <a name="example-entity-signals-an-entity"></a>Örnek: Varlık bir varlığı işaret ediyor

Bir varlık işlevi, bir işlemi yürütürken diğer varlıklara, hatta kendisine sinyal gönderebilir.
Örneğin, sayaç 100 `Counter` değerine ulaştığında bazı monitör varlığına "kilometre taşına ulaşılan" sinyali gönderebilecek şekilde önceki varlık örneğini değiştirebiliriz.

# <a name="c"></a>[C#](#tab/csharp)

```csharp
   case "add":
        var currentValue = ctx.GetState<int>();
        var amount = ctx.GetInput<int>();
        if (currentValue < 100 && currentValue + amount >= 100)
        {
            ctx.SignalEntity(new EntityId("MonitorEntity", ""), "milestone-reached", ctx.EntityKey);
        }

        ctx.SetState(currentValue + amount);
        break;
```

# <a name="javascript"></a>[JavaScript](#tab/javascript)

```javascript
    case "add":
        const amount = context.df.getInput();
        if (currentValue < 100 && currentValue + amount >= 100) {
            const entityId = new df.EntityId("MonitorEntity", "");
            context.df.signalEntity(entityId, "milestone-reached", context.df.instanceId);
        }
        context.df.setState(currentValue + amount);
        break;
```

---

## <a name="entity-coordination-currently-net-only"></a><a name="entity-coordination"></a>Varlık koordinasyonu (şu anda sadece .NET)

Birden çok varlık arasında işlemleri koordine etmek zorunda kaldığınız zamanlar olabilir. Örneğin, bir bankacılık uygulamasında, tek tek banka hesaplarını temsil eden varlıklar olabilir. Bir hesaptan diğerine para aktardığınızda, kaynak hesabın yeterli paraya sahip olduğundan emin olmalısınız. Ayrıca, hem kaynak hem de hedef hesaplardaki güncelleştirmelerin işlem açısından tutarlı bir şekilde yapıldığından emin olmalısınız.

### <a name="example-transfer-funds-c"></a>Örnek: Transfer fonları (C#)

Aşağıdaki örnek kod, bir orkestratör işlevi kullanarak iki hesap varlığı arasında para aktarMaktadır. Varlık güncelleştirmelerini koordine `LockAsync` etmek, orkestrasyonda kritik bir _bölüm_ oluşturmak için yöntemi kullanmayı gerektirir.

> [!NOTE]
> Basitlik için bu örnek, `Counter` daha önce tanımlanan varlığı yeniden kullanır. Gerçek bir uygulamada, daha ayrıntılı `BankAccount` bir varlık tanımlamak daha iyi olacaktır.

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

`LockAsync` .NET'te, `IDisposable`atıldığında kritik bölümü sona erdir. Bu `IDisposable` sonuç, kritik bölümün `using` sözdizimi temsilini almak için bir blokla birlikte kullanılabilir.

Önceki örnekte, bir orkestratör işlevi kaynak bir varlıktan hedef varlığa para aktarıldı. Yöntem `LockAsync` hem kaynak hem de hedef hesap varlıklarını kilitledi. Bu kilitleme, düzenleme mantığı `using` ifadenin sonundaki kritik bölümden çıkana kadar başka hiçbir istemcinin her iki hesabın durumunu sorgulayamamasını veya durumunu değiştiremeyeceğini garanti etti. Bu davranış, kaynak hesaptan aşırı çekim olasılığını önler.

> [!NOTE] 
> Bir düzenleme normal olarak veya bir hatayla sona erdiğinde, devam etmekte olan tüm kritik bölümler örtülü olarak sona erer ve tüm kilitler serbest bırakılır.

### <a name="critical-section-behavior"></a>Kritik kesit davranışı

Yöntem, `LockAsync` bir orkestrasyonda kritik bir bölüm oluşturur. Bu kritik bölümler, diğer orkestrasyonların belirli bir varlık kümesinde çakışan değişiklikler yapmasını engeller. Dahili olarak, `LockAsync` API varlıklara "kilitleme" işlemleri gönderir ve bu aynı varlıkların her birinden bir "kilit edinilmiş" yanıt iletisi aldığında döndürür. Hem kilit hem de kilit açma tüm varlıklar tarafından desteklenen yerleşik işlemlerdir.

Kilitli durumdayken bir varlıkta diğer istemcilerden gelen işlemlere izin verilmez. Bu davranış, bir anda bir varlığı yalnızca bir düzenleme örneğinin kilitlebilmesini sağlar. Bir arayan, bir düzenleme tarafından kilitlenirken bir varlık üzerinde bir işlem çağırmaya çalışırsa, bu işlem bekleyen bir işlem kuyruğuna yerleştirilir. Beklemedeki işlemler, bekleme orkestrası kilitlerini serbest bırakana kadar işleme alınır.

> [!NOTE] 
> Bu davranış, C#'daki `lock` deyim gibi çoğu programlama dilinde kullanılan eşitleme ilkellerinden biraz farklıdır. Örneğin, C#'da `lock` deyim, birden çok iş parçacığı arasında uygun eşitleme sağlamak için tüm iş parçacıkları tarafından kullanılmalıdır. Ancak varlıklar, tüm arayanların bir varlığı açıkça kilitlemesini gerektirmez. Herhangi bir arayan bir varlığı kilitlerse, bu varlıktaki diğer tüm işlemler engellenir ve bu kilidin arkasında sıraya alınır.

Varlıklarüzerindeki kilitler dayanıklıdır, bu nedenle yürütme işlemi geri dönüştürülse bile devam ederler. Kilitler, bir varlığın dayanıklı durumunun bir parçası olarak dahili olarak kalıcıdır.

İşlemlerin aksine, kritik bölümler hata durumunda değişiklikleri otomatik olarak geri almaz. Bunun yerine, geri alma veya yeniden deneme gibi herhangi bir hata işleme, örneğin hataları veya özel durumları yakalayarak açıkça kodlanmalıdır. Bu tasarım seçimi kasıtlıdır. Bir orkestrasyonun tüm etkilerini otomatik olarak geri almak genel olarak zor veya imkansızdır, çünkü orkestrasyon etkinlikleri çalıştırabilir ve geri alınamayan harici hizmetlere çağrılar yapabilir. Ayrıca, geri alma girişimleri kendilerini başarısız olabilir ve daha fazla hata işleme gerektirir.

### <a name="critical-section-rules"></a>Kritik bölüm kuralları

Çoğu programlama dilinde ki düşük seviyeli kilitleme ilkellerinin aksine, kritik bölümlerin *kilitlenmemesi garanti edilir.* Kilitlenmeleri önlemek için aşağıdaki kısıtlamaları uygularız: 

* Kritik bölümler iç içe işlenmez.
* Kritik bölümler alt orkestrasyon oluşturamaz.
* Kritik bölümler yalnızca kilitledikleri varlıkları çağırabilir.
* Kritik bölümler, birden çok paralel çağrı kullanarak aynı varlığı arayamaz.
* Kritik bölümler yalnızca kilitlemedikleri varlıklara sinyal verebilir.

Bu kuralların herhangi bir ihlali, hangi kuralın kırıldığını açıklayan bir ileti içeren .NET'teki gibi `LockingRulesViolationException` çalışma zamanı hatasına neden olur.

## <a name="comparison-with-virtual-actors"></a>Sanal aktörlerle karşılaştırma

Dayanıklı varlıklar özelliklerinin çoğu aktör [modeli](https://en.wikipedia.org/wiki/Actor_model)esinlenilmiştir. Aktörleri zaten biliyorsanız, bu makalede açıklanan kavramların çoğunu tanıyabilirsiniz. Dayanıklı varlıklar özellikle sanal [aktörler](https://research.microsoft.com/projects/orleans/)benzer , veya tahıl, [Orleans projesi](http://dotnet.github.io/orleans/)tarafından popüler olarak . Örneğin:

* Dayanıklı varlıklar bir varlık kimliği ile adreslenebilir.
* Dayanıklı varlık işlemleri, yarış koşullarını önlemek için teker teker seri olarak yürütülür.
* Dayanıklı varlıklar çağrıldıklarında veya sinyal verildiğinde dolaylı olarak oluşturulur.
* İşlemler yürütülmediğinde, dayanıklı varlıklar sessizce bellekten boşaltılır.

Kayda değer bazı önemli farklılıklar vardır:

* Dayanıklı varlıklar gecikme süresine göre dayanıklılığa öncelik verir ve bu nedenle katı gecikme gereksinimleri olan uygulamalar için uygun olmayabilir.
* Dayanıklı varlıklarda iletiler için yerleşik zaman aşımları yoktur. Orleans'ta, tüm iletiler yapılandırılabilir bir süre sonra zaman ları doldu. Varsayılan değer 30 saniyedir.
* Varlıklar arasında gönderilen iletiler güvenilir ve düzenli olarak teslim edilir. Orleans'ta, güvenilir veya siparişli teslimat akışlar aracılığıyla gönderilen içerik için desteklenir, ancak tahıllar arasındaki tüm iletiler için garanti edilir.
* Varlıklardaki istek-yanıt kalıpları orkestrasyonlar ile sınırlıdır. Varlıklar içinden, orijinal aktör modelinde olduğu gibi ve Orleans'taki tahılların aksine, yalnızca tek yönlü mesajlaşmaya (sinyalizasyon olarak da bilinir) izin verilir. 
* Dayanıklı varlıklar kilitlenmez. Orleans'ta kilitlenmeler oluşabilir ve iletiler zaman dolana kadar çözülemez.
* Dayanıklı varlıklar dayanıklı orkestrasyonlar ve destek dağıtılmış kilitleme mekanizmaları ile birlikte kullanılabilir. 


## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [.NET'te dayanıklı varlıklar için Geliştirici kılavuzunu okuyun](durable-functions-dotnet-entities.md)

> [!div class="nextstepaction"]
> [Görev merkezleri hakkında bilgi edinin](durable-functions-task-hubs.md)
