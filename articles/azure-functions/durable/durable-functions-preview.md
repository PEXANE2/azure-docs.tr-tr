---
title: Dayanıklı İşlevler Önizleme özellikleri-Azure Işlevleri
description: Dayanıklı İşlevler için Önizleme özellikleri hakkında bilgi edinin.
services: functions
author: cgillum
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: article
ms.date: 07/08/2019
ms.author: azfuncdf
ms.openlocfilehash: 1609931cd5fcab0977ff64f680fbb1f253f3caaf
ms.sourcegitcommit: f7998db5e6ba35cbf2a133174027dc8ccf8ce957
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/05/2019
ms.locfileid: "68782183"
---
# <a name="durable-functions-20-preview-azure-functions"></a>Dayanıklı İşlevler 2,0 Preview (Azure Işlevleri)

*Dayanıklı işlevler* , [Azure Işlevleri](../functions-overview.md) ve [Azure Web işleri](../../app-service/web-sites-create-web-jobs.md) 'nin bir uzantısıdır ve bu da sunucusuz bir ortamda durum bilgisi olan işlevler yazmanızı sağlar. Uzantı sizin için durumu, denetim noktalarını ve yeniden başlatmaları yönetir. Daha önce Dayanıklı İşlevler hakkında bilginiz yoksa [genel bakış belgelerine](durable-functions-overview.md)bakın.

Dayanıklı İşlevler 1. x, Azure Işlevlerinin bir GA (genel kullanıma sunuldu) özelliğidir, ancak şu anda genel önizleme aşamasında olan çeşitli alt özellikleri de içerir. Bu makalede yeni yayınlanan Önizleme özellikleri açıklanmakta ve bunların nasıl çalıştığı ve bunları nasıl kullanabileceğiniz hakkında ayrıntılı bilgiler yer alır.

> [!NOTE]
> Bu Önizleme özellikleri, şu anda birçok önemli değişiklik içeren bir **Önizleme kalitesi sürümü** olan dayanıklı işlevler 2,0 sürümünün bir parçasıdır. Azure Işlevleri dayanıklı Uzantı paketi yapıları, **2.0.0-betaX**biçimindeki sürümlerle NuGet.org üzerinde bulunabilir. Bu derlemeler üretim iş yükleri için tasarlanmamıştır ve sonraki sürümlerde ek son değişiklikler olabilir.

## <a name="breaking-changes"></a>Yeni değişiklikler

Dayanıklı İşlevler 2,0 ' de birçok önemli değişiklik sunulmuştur. Mevcut uygulamaların kod değişikliği olmadan Dayanıklı İşlevler 2,0 ile uyumlu olması beklenmez. Bu bölümde bazı değişiklikler listelenir:

### <a name="hostjson-schema"></a>Host. JSON şeması

Aşağıdaki kod parçacığında Host. JSON için yeni şema gösterilmektedir. Dikkat edilecek ana değişiklikler yeni alt kısımlar:

* `"storageProvider"`depolama birimine özgü `"azureStorage"` yapılandırma için (ve alt bölüm)
* `"tracking"`izleme ve günlüğe kaydetme yapılandırması için
* `"notifications"`Event Grid bildirim `"eventGrid"` yapılandırması için (ve alt bölüm)

```json
{
  "version": "2.0",
  "extensions": {
    "durableTask": {
      "hubName": <string>,
      "storageProvider": {
        "azureStorage": {
          "connectionStringName": <string>,
          "controlQueueBatchSize": <int?>,
          "partitionCount": <int?>,
          "controlQueueVisibilityTimeout": <hh:mm:ss?>,
          "workItemQueueVisibilityTimeout": <hh:mm:ss?>,
          "trackingStoreConnectionStringName": <string?>,
          "trackingStoreNamePrefix": <string?>,
          "maxQueuePollingInterval": <hh:mm:ss?>
        }
      },
      "tracking": {
        "traceInputsAndOutputs": <bool?>,
        "traceReplayEvents": <bool?>,
      },
      "notifications": {
        "eventGrid": {
          "topicEndpoint": <string?>,
          "keySettingName": <string?>,
          "publishRetryCount": <string?>,
          "publishRetryInterval": <hh:mm:ss?>,
          "publishRetryHttpStatus": <int[]?>,
          "publishEventTypes": <string[]?>,
        }
      },
      "maxConcurrentActivityFunctions": <int?>,
      "maxConcurrentOrchestratorFunctions": <int?>,
      "extendedSessionsEnabled": <bool?>,
      "extendedSessionIdleTimeoutInSeconds": <int?>,
      "customLifeCycleNotificationHelperType": <string?>
  }
}
```

Dayanıklı işlevler 2,0 kararlı olmaya devam ettiğinden, Host. JSON `durableTask` bölümüne daha fazla değişiklik sunulacaktır. Bu değişiklikler hakkında daha fazla bilgi için [Bu GitHub sorununa](https://github.com/Azure/azure-functions-durable-extension/issues/641)bakın.

### <a name="public-interface-changes"></a>Ortak arabirim değişiklikleri

Dayanıklı İşlevler tarafından desteklenen çeşitli "bağlam" nesneleri, birim testinde kullanılmak üzere tasarlanan soyut temel sınıflara sahipti. Dayanıklı İşlevler 2,0 ' nın bir parçası olarak bu soyut temel sınıflar, arabirimlerle değiştirilmiştir. Somut türleri doğrudan kullanan işlev kodu bundan etkilenmez.

Aşağıdaki tablo, ana değişiklikleri temsil eder:

| Eski tür | Yeni tür |
|----------|----------|
| DurableOrchestrationClientBase | Idurableorchestrationclient |
| DurableOrchestrationContextBase | IDurableOrchestrationContext |
| DurableActivityContextBase | Idurableactivitycontext |

Soyut bir temel sınıfın sanal yöntemler içerdiği durumlarda, bu sanal yöntemler ' de `DurableContextExtensions`tanımlanan genişletme yöntemleriyle değiştirilmiştir.

## <a name="entity-functions"></a>Varlık işlevleri

Varlık işlevleri, *dayanıklı varlıklar*olarak bilinen küçük durum parçalarını okumak ve güncelleştirmek için işlemleri tanımlar. Orchestrator işlevleri gibi, varlık işlevleri de özel tetikleyici türü, *varlık tetikleyicisi*olan işlevlerdir. Orchestrator işlevlerinin aksine, varlık işlevlerinin belirli kod kısıtlamaları yoktur. Varlık işlevleri, durumu denetim akışı aracılığıyla örtük olarak temsil etmek yerine, durumu açıkça da yönetir.

### <a name="net-programing-models"></a>.NET programlama modelleri

Dayanıklı varlıklar yazmak için iki isteğe bağlı programlama modeli vardır. Aşağıdaki kod, standart bir işlev olarak uygulanan basit bir *sayaç* varlığına bir örnektir. Bu işlev, `get` `add` `reset` herbiribirtamsayıdurumdeğeriüzerindeçalışanüçişlem,,,ve`currentValue`tanımlar.

```csharp
[FunctionName("Counter")]
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

Bu model basit varlık uygulamaları veya dinamik bir işlem kümesi olan uygulamalar için en iyi şekilde kullanılır. Ancak, statik olan ancak daha karmaşık uygulamalar içeren varlıklar için yararlı olan sınıf tabanlı bir programlama modeli de vardır. Aşağıdaki örnek, .NET sınıfları ve yöntemleri kullanılarak `Counter` varlığın eşdeğer bir uygulamasıdır.

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

Sınıf tabanlı model, [Orleans](https://www.microsoft.com/research/project/orleans-virtual-actors/)tarafından popularile benzerdir. Bu modelde, bir varlık türü .NET sınıfı olarak tanımlanmıştır. Sınıfının her yöntemi, bir dış istemci tarafından çağrılabilen bir işlemdir. Ancak, Orleans 'un aksine .NET arabirimleri isteğe bağlıdır. Önceki *sayaç* örneği bir arabirim kullanmadı, ancak yine de diğer IŞLEVLER veya HTTP API çağrıları aracılığıyla çağrılabilir.

Varlık *örneklerine* benzersiz bir tanımlayıcı, *varlık kimliği*üzerinden erişilir. Bir varlık KIMLIĞI, bir varlık örneğini benzersiz bir şekilde tanımlayan dizelerin bir çiftidir. Aşağıdakilerden oluşur:

* Bir **varlık adı**: varlığın türünü tanımlayan bir ad (örneğin, "Counter").
* Bir **varlık anahtarı**: varlığı, aynı ada sahip diğer tüm varlıklar arasında benzersiz şekilde tanımlayan bir dize (örneğin, bir GUID).

Örneğin, bir *sayaç* varlığı işlevi çevrimiçi bir oyunda puanı korumak için kullanılabilir. Oyunun her bir örneği, ve gibi benzersiz bir varlık kimliğine `@Counter@Game1` `@Counter@Game2`sahip olacaktır.

### <a name="comparison-with-virtual-actors"></a>Sanal aktörler ile karşılaştırma

Dayanıklı varlıkların tasarımı, [aktör modeliyle](https://en.wikipedia.org/wiki/Actor_model)büyük ölçüde etkilenir. Aktörlerle zaten bilgi sahibiyseniz, dayanıklı varlıkların arkasındaki kavramların size tanıdık olması gerekir. Özellikle, dayanıklı varlıklar [sanal aktörlerin](https://research.microsoft.com/projects/orleans/) çoğunu birçok şekilde benzerdir:

* Dayanıklı varlıklar bir *VARLıK kimliği*aracılığıyla adreslidir.
* Yarış durumlarını engellemek için, sürekli varlık işlemleri tek seferde bir kez yürütülür.
* Dayanıklı varlıklar, çağrıldığında veya sinyalde olduğunda otomatik olarak oluşturulur.
* İşlemler yürütümemekte, kalıcı varlıkların belleği sessizce kaldırılır.

Ancak, dikkat edilmesi gereken bazı önemli farklılıklar vardır:

* Dayanıklı varlıklar *dayanıklılığı* *gecikme süresine*göre önceliklendirmez ve bu nedenle, katı gecikme süresi gereksinimleri olan uygulamalar için uygun olmayabilir.
* Varlıklar arasında gönderilen iletiler güvenilir bir şekilde ve sırayla dağıtılır.
* Dayanıklı varlıklar, dayanıklı düzenlemeler ile birlikte kullanılabilir ve bu makalede daha sonra açıklanan dağıtılmış kilitler olarak işlev görebilir.
* Varlıklarda istek/yanıt desenleri, düzenleme ile sınırlıdır. Varlıkla varlığa iletişim için, özgün aktör modelinde olduğu gibi yalnızca tek yönlü mesajlara ("sinyal" olarak da bilinir) izin verilir. Bu davranış, dağıtılmış kilitlenmeleri engeller.

### <a name="durable-entity-net-apis"></a>Dayanıklı varlık .NET API 'Leri

Varlık desteği birçok API içerir. Bir varlık için, bir varlıkta bir işlem çağrıldığında ne olacağını belirten, yukarıda gösterildiği gibi varlık işlevlerini tanımlamaya yönelik yeni bir API vardır. Ayrıca, istemcileri ve düzenlemeleri için mevcut API 'Ler, varlıklarla etkileşim için yeni işlevlerle güncelleştirilmiştir.

#### <a name="implementing-entity-operations"></a>Varlık işlemlerini uygulama

Bir varlık üzerindeki bir işlemin yürütülmesi, bu üyeleri bağlam nesnesi üzerinde çağırabilir (`IDurableEntityContext` .net 'te):

* **OperationName**: işlemin adını alır.
* **Getınput\<TInput >** : işlemin girdisini alır.
* **GetState\<TState >** : varlığın geçerli durumunu alır.
* **SetState**: varlığın durumunu güncelleştirir.
* **Tiflentity**: bir varlığa tek yönlü bir ileti gönderir.
* **Self**: varlığın kimliğini alır.
* **Dönüş**: istemciye veya düzenlemeye işlemi çağıran bir değer döndürür.
* **Inewlyinşa**: varlık `true` işlemden önce yoksa, döndürür.
* **Yeniden dönüşlü**bir işlem: işlemi tamamladıktan sonra varlığı siler.

İşlemler, düzenleyiclerden daha az kısıtlanıyor:

* İşlemler, zaman uyumlu veya zaman uyumsuz API 'Ler kullanarak dış g/ç 'yi çağırabilir (yalnızca zaman uyumsuz olanları kullanmanızı öneririz).
* İşlemler belirleyici olmayan bir işlem olabilir. Örneğin, `DateTime.UtcNow` `Guid.NewGuid()` veya çağırmakgüvenlidir.`new Random()`

#### <a name="accessing-entities-from-clients"></a>İstemcilerden varlıklara erişme

Dayanıklı varlıklar `orchestrationClient` bağlama aracılığıyla sıradan işlevlerden çağrılabilir (`IDurableOrchestrationClient` .net 'te). Aşağıdaki yöntemler desteklenir:

* **Readentitystateasync\<T >** : bir varlığın durumunu okur.
* **Tiflentityasync**: bir varlığa tek yönlü bir ileti gönderir ve kuyruğa alınıp alınmasını bekler.
* **Tiflentityasync\<T >** : ile `SignalEntityAsync` aynıdır, ancak türünde `T`oluşturulan bir proxy nesnesi kullanır.

Önceki `SignalEntityAsync` çağrı bir `string` olarak varlık işleminin adını ve işlemin `object`yükünü bir olarak belirtmeyi gerektirir. Aşağıdaki örnek kod bu düzenin bir örneğidir:

```csharp
EntityId id = // ...
object amount = 5;
context.SignalEntityAsync(id, "Add", amount);
```

Tür kullanımı uyumlu erişim için bir proxy nesnesi oluşturmak da mümkündür. Tür açısından güvenli bir ara sunucu oluşturmak için varlık türünün bir arabirim uygulaması gerekir. Örneğin, daha önce bahsedilen `Counter` varlığın aşağıdaki gibi tanımlanmış bir `ICounter` arabirim uyguladığını varsayalım:

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

İstemci kodu daha sonra kullanabilir `SignalEntityAsync<T>` ve bir tür `ICounter` güvenli proxy oluşturmak için tür parametresi olarak arabirimi belirtebilir. Tür açısından güvenli proxy 'lerin bu kullanımı, aşağıdaki kod örneğinde gösterilmiştir:

```csharp
[FunctionName("UserDeleteAvailable")]
public static async Task AddValueClient(
    [QueueTrigger("my-queue")] string message,
    [OrchestrationClient] IDurableOrchestrationClient client)
{
    int amount = int.Parse(message);
    var target = new EntityId(nameof(Counter), "MyCounter");
    await client.SignalEntityAsync<ICounter>(target, proxy => proxy.Add(amount));
}
```

Önceki örnekte, `proxy` parametresi, `Add` çağrısını iç olarak eşdeğer (türsüz) `ICounter`çağrıya `SignalEntityAsync`çeviren, dinamik olarak üretilmiş bir örneğidir.

İçin `SignalEntityAsync<T>` tür parametresi aşağıdaki kısıtlamalara sahiptir:

* Tür parametresi bir arabirim olmalıdır.
* Arabirim üzerinde yalnızca Yöntemler tanımlanabilir. Özellikler desteklenmiyor.
* Her yöntemin bir veya hiç parametre tanımlamamalıdır.
* Her yöntemin `void`, `Task`, `Task<T>` veya`T` , bazı JSON-serializlenebilir tür olduğunu döndürmesi gerekir.
* Arabirimin, arabirimin derlemesi içinde tam olarak bir tür tarafından uygulanması gerekir.

Çoğu durumda, bu gereksinimleri karşılamayan arabirimler çalışma zamanı özel durumuna neden olur.

> [!NOTE]
> Tutarlılık açısından performansı `ReadEntityStateAsync` `IDurableOrchestrationClient` önceliklendirmek için ve `SignalEntityAsync` yöntemlerinin dikkate alınacağını göz önünde bulundurulmalıdır. `ReadEntityStateAsync`Eski bir değer döndürebilir ve `SignalEntityAsync` işlem tamamlanmadan önce dönebilir.

#### <a name="accessing-entities-from-orchestrations"></a>Organize lerden varlıklara erişme

Nesneler, `IDurableOrchestrationContext` nesneleri kullanarak varlıklara erişebilir. Tek yönlü iletişim (ateş ve unutma) ve iki yönlü iletişim (istek ve yanıt) arasında seçim yapabilir. İlgili yöntemler şunlardır:

* **Tiflentity**: bir varlığa tek yönlü bir ileti gönderir.
* **Callentityasync**: bir varlığa bir ileti gönderir ve işlemin tamamlandığını belirten bir yanıt bekler.
* **Callentityasync\<T >** : bir varlığa bir ileti gönderir ve T türünde bir sonuç içeren bir yanıt bekler.

İki yönlü iletişim kullanılırken, işlemin yürütülmesi sırasında oluşan tüm özel durumlar da çağıran düzenleme ve yeniden oluşturma işlemine geri iletilir. Buna karşılık, Fire-ve-unut kullanırken özel durumlar gözlemlenmez.

Tür kullanımı uyumlu erişim için düzenleme işlevleri bir arabirime göre proxy 'ler oluşturabilir. `CreateEntityProxy` Uzantı yöntemi bu amaçla kullanılabilir:

```csharp
public interface IAsyncCounter
{
    Task AddAsync(int amount);
    Task ResetAsync();
    Task<int> GetAsync();
}

[FunctionName("CounterOrchestration")]
public static async Task Run(
    [OrchestrationTrigger] IDurableOrchestrationContext context)
{
    // ...
    IAsyncCounter proxy = context.CreateEntityProxy<IAsyncCounter>("MyCounter");
    await proxy.AddAsync(5);
    int newValue = await proxy.GetAsync();
    // ...
}
```

Önceki örnekte, `IAsyncCounter` arabirimi uygulayan bir "Counter" varlığının var olduğu varsayılır. Böylece düzenleme, varlık ile eşzamanlı olarak etkileşimde `IAsyncCounter` bulunmak için bir proxy türü oluşturmak üzere tür tanımını kullanabildi.

### <a name="locking-entities-from-orchestrations"></a>Varlıkları düzenleyiclerden kilitleme

Düzenlemeler, varlıkları kilitleyebilir. Bu özellik, *önemli bölümleri*kullanarak istenmeyen engelleri önlemenin basit bir yolunu sağlar.

Bağlam nesnesi aşağıdaki yöntemleri sağlar:

* **Lockasync**: bir veya daha fazla varlık üzerinde kilitler elde edin.
* **Iskilitlendi**: Şu anda kritik bir bölümde true, değilse false döndürür.

Önemli bölüm sonlanır ve düzenleme sona erdiğinde tüm kilitler serbest bırakılır. .Net ' te `LockAsync` , kritik `IDisposable` bölümün sözdizimsel bir gösterimini almak için bir `using` yan tümcesiyle birlikte kullanılabilecek, atıldığında kritik bölümü sonlandıran bir döndürür.

Örneğin, iki oyuncunun kullanılabilir olup olmadığını test etmek için gereken bir düzenleme düşünün ve sonra bunları bir oyuna atayın. Bu görev, kritik bir bölüm kullanılarak aşağıdaki gibi uygulanabilir:

```csharp
[FunctionName("Orchestrator")]
public static async Task RunOrchestrator(
    [OrchestrationTrigger] IDurableOrchestrationContext ctx)
{
    EntityId player1 = /* ... */;
    EntityId player2 = /* ... */;

    using (await ctx.LockAsync(player1, player2))
    {
        bool available1 = await ctx.CallEntityAsync<bool>(player1, "is-available");
        bool available2 = await ctx.CallEntityAsync<bool>(player2, "is-available");

        if (available1 && available2)
        {
            Guid gameId = ctx.NewGuid();

            await ctx.CallEntityAsync(player1, "assign-game", gameId);
            await ctx.CallEntityAsync(player2, "assign-game", gameId);
        }
    }
}
```

Kritik bölümünde, oynatıcı varlıklarının her ikisi de kilitlidir ve bu, kritik bölümün içinden çağırıların dışında herhangi bir işlem yürütmeyeceği anlamına gelir. Bu davranış, farklı bir oyuna atanmakta olan oyuncular veya oturum kapatma gibi çakışan işlemlerle bu engelleri engeller.

Kritik bölümlerin nasıl kullanılabileceği konusunda çeşitli kısıtlamalar sunuyoruz. Bu kısıtlamalar kilitlenmeleri ve yeniden giriş yapılmasını engeller.

* Kritik bölümler iç içe geçirilemez.
* Kritik bölümler, alt düzenlemeler oluşturamaz.
* Kritik bölümler, yalnızca kilitlediği varlıkları çağırabilir.
* Kritik bölümler birden çok paralel çağrı kullanarak aynı varlığı çağıramaz.
* Kritik bölümler yalnızca kilitlendikleri varlıkları işaret edebilir.

## <a name="alternate-storage-providers"></a>Alternatif depolama sağlayıcıları

Dayanıklı görev çerçevesi, [Azure depolama](https://github.com/Azure/durabletask/tree/master/src/DurableTask.AzureStorage), [Azure Service Bus](https://github.com/Azure/durabletask/tree/master/src/DurableTask.ServiceBus), [bellek Içi öykünücü](https://github.com/Azure/durabletask/tree/master/src/DurableTask.Emulator)ve deneysel [redin](https://github.com/Azure/durabletask/tree/redis/src/DurableTask.Redis) sağlayıcısı dahil olmak üzere bugün birden çok depolama sağlayıcısını destekler. Ancak, şu anda Azure Işlevleri için dayanıklı görev uzantısı yalnızca Azure Storage sağlayıcısını destekliyordu. Dayanıklı İşlevler 2,0 ' den başlayarak, redin sağlayıcısı ile başlayarak alternatif depolama sağlayıcıları için destek ekleniyor.

> [!NOTE]
> Dayanıklı İşlevler 2,0 yalnızca .NET Standard 2,0 ile uyumlu sağlayıcıları destekler. Yazma sırasında, Azure Service Bus sağlayıcısı .NET Standard 2,0 desteklemez ve bu nedenle alternatif bir depolama sağlayıcısı olarak kullanılamaz.

### <a name="emulator"></a>Benzeti

[Durabletask. öykünücü](https://www.nuget.org/packages/Microsoft.Azure.DurableTask.Emulator/) sağlayıcısı yerel bir bellek, yerel test senaryoları için uygun, dayanıklı olmayan bir depolama sağlayıcıdır. Aşağıdaki en az **Host. JSON** şeması kullanılarak yapılandırılabilir:

```json
{
  "version": "2.0",
  "extensions": {
    "durableTask": {
      "hubName": <string>,
      "storageProvider": {
        "emulator": { }
      }
    }
  }
}
```

### <a name="redis-experimental"></a>Reddir (deneysel)

[Durabletask. reddir](https://www.nuget.org/packages/Microsoft.Azure.DurableTask.Redis/) sağlayıcısı, tüm düzenleme durumlarını yapılandırılmış bir redsıs kümesine devam ettirir.

```json
{
  "version": "2.0",
  "extensions": {
    "durableTask": {
      "hubName": <string>,
      "storageProvider": {
        "redis": {
          "connectionStringName": <string>,
        }
      }
    }
  }
}
```

, `connectionStringName` Bir uygulama ayarı veya ortam değişkeni adına başvurmalıdır. Bu uygulama ayarı veya ortam değişkeni, *sunucu: bağlantı noktası*biçiminde bir redsıs bağlantı dizesi değeri içermelidir. Örneğin, `localhost:6379` yerel bir redto kümesine bağlanmak için.

> [!NOTE]
> Redsıs sağlayıcısı şu anda deneysel ve yalnızca tek bir düğümde çalışan işlev uygulamalarını destekliyor. Redin sağlayıcısının genel kullanıma açık hale getirilme garantisi yoktur ve sonraki bir sürümde kaldırılabilir.
