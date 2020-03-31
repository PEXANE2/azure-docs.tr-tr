---
title: Azure Hizmet Kumaşı durumunu yönetme
description: Azure Hizmet Kumaşı Güvenilir Aktör için duruma erişme, kaydetme ve kaldırma hakkında bilgi edinin ve bir uygulama tasarlarken göz önünde bulunun.
author: vturecek
ms.topic: conceptual
ms.date: 03/19/2018
ms.author: vturecek
ms.openlocfilehash: 788c337a37ec66c5aa1521c5cd9f2816ed7a8bf9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75645642"
---
# <a name="access-save-and-remove-reliable-actors-state"></a>Güvenilir Aktörler durumuna erişin, kaydedin ve kaldırın
[Güvenilir Aktörler,](service-fabric-reliable-actors-introduction.md) hem mantığı hem de durumu kapsülleyebilen ve durumu güvenilir bir şekilde koruyabilen tek iş parçacığı nesnelerdir. Her aktör örneğinin kendi [durum yöneticisi](service-fabric-reliable-actors-state-management.md)vardır: anahtar/değer çiftlerini güvenilir bir şekilde depolayan sözlük benzeri bir veri yapısı. Devlet müdürü bir devlet sağlayıcı etrafında bir sarmalayıcı olduğunu. Hangi [kalıcılık ayarı](service-fabric-reliable-actors-state-management.md#state-persistence-and-replication) kullanılırsa kullanılsın verileri depolamak için kullanabilirsiniz.

Devlet yöneticisi anahtarları dizeleri olmalıdır. Değerler geneldir ve özel türler de dahil olmak üzere her türde olabilir. Çoğaltma sırasında ağ üzerinden diğer düğümlere aktarılabileceği nden ve aktörün durum kalıcılığı ayarına bağlı olarak diske yazılabileceğinden, durum yöneticisinde depolanan değerler veri sözleşmesi nin seri olarak geçirilebiliyor olması gerekir.

Devlet yöneticisi, Güvenilir Sözlük'te bulunanlara benzer şekilde durumu yönetmek için ortak sözlük yöntemlerini ortaya çıkarır.

Bilgi için, [aktör durumunu yönetmedeki en iyi uygulamalara](service-fabric-reliable-actors-state-management.md#best-practices)bakın.

## <a name="access-state"></a>Erişim durumu
Devlet yöneticisi aracılığıyla anahtarla erişilir. Durum yöneticisi yöntemleri, aktörler inanıldığında disk G/Ç gerektirebileceğinden tüm eşzamanlı yöntemlerdir. İlk erişimden sonra, durum nesneleri bellekte önbelleğe alınır. Erişim işlemlerini yinelemek nesnelere doğrudan bellekten erişin ve disk G/Ç veya eşzamanlı bağlam anahtarlama yükü olmadan eşzamanlı olarak döndürün. Bir durum nesnesi aşağıdaki durumlarda önbellekten kaldırılır:

* Bir aktör yöntemi, bir nesneyi devlet yöneticisinden aldıktan sonra işlenmemiş bir özel durum atar.
* Bir aktör devre dışı bırakıldıktan sonra veya hatadan sonra yeniden etkinleştirilir.
* Durum sağlayıcı sayfaları diske durum. Bu davranış, devlet sağlayıcı uygulamasına bağlıdır. `Persisted` Ayar için varsayılan durum sağlayıcısı bu davranışı vardır.

Anahtar için bir giriş *Get* yoksa `KeyNotFoundException`(C#) veya `NoSuchElementException`(Java) atan standart bir Get işlemi kullanarak durumu alabilirsiniz:

```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task<int> GetCountAsync()
    {
        return this.StateManager.GetStateAsync<int>("MyState");
    }
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
class MyActorImpl extends FabricActor implements  MyActor
{
    public MyActorImpl(ActorService actorService, ActorId actorId)
    {
        super(actorService, actorId);
    }

    public CompletableFuture<Integer> getCountAsync()
    {
        return this.stateManager().getStateAsync("MyState");
    }
}
```

Bir anahtar için giriş yoksa atmayan bir *TryGet* yöntemi ni kullanarak durum da alabilirsiniz:

```csharp
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public async Task<int> GetCountAsync()
    {
        ConditionalValue<int> result = await this.StateManager.TryGetStateAsync<int>("MyState");
        if (result.HasValue)
        {
            return result.Value;
        }

        return 0;
    }
}
```
```Java
class MyActorImpl extends FabricActor implements  MyActor
{
    public MyActorImpl(ActorService actorService, ActorId actorId)
    {
        super(actorService, actorId);
    }

    public CompletableFuture<Integer> getCountAsync()
    {
        return this.stateManager().<Integer>tryGetStateAsync("MyState").thenApply(result -> {
            if (result.hasValue()) {
                return result.getValue();
            } else {
                return 0;
            });
    }
}
```

## <a name="save-state"></a>Durumu kaydet
Eyalet yöneticisi alma yöntemleri yerel bellekteki bir nesneye başvuru döndürer. Bu nesneyi yalnızca yerel bellekte değiştirmek, nesnenin kalıcı olarak kaydedilmesine neden olmaz. Bir nesne devlet yöneticisinden alınıp değiştirildiğinde, kaydedilmesi için devlet yöneticisine yeniden eklenmesi gerekir.

Sözdizimieşdeğeri olan koşulsuz *Bir Küme*kullanarak durum ekleyebilirsiniz: `dictionary["key"] = value`

```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task SetCountAsync(int value)
    {
        return this.StateManager.SetStateAsync<int>("MyState", value);
    }
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
class MyActorImpl extends FabricActor implements  MyActor
{
    public MyActorImpl(ActorService actorService, ActorId actorId)
    {
        super(actorService, actorId);
    }

    public CompletableFuture setCountAsync(int value)
    {
        return this.stateManager().setStateAsync("MyState", value);
    }
}
```

*Ekle* yöntemini kullanarak durum ekleyebilirsiniz. Bu yöntem, `InvalidOperationException`zaten var `IllegalStateException`olan bir anahtar eklemeye çalıştığında (C#) veya (Java) atar.

```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task AddCountAsync(int value)
    {
        return this.StateManager.AddStateAsync<int>("MyState", value);
    }
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
class MyActorImpl extends FabricActor implements  MyActor
{
    public MyActorImpl(ActorService actorService, ActorId actorId)
    {
        super(actorService, actorId);
    }

    public CompletableFuture addCountAsync(int value)
    {
        return this.stateManager().addOrUpdateStateAsync("MyState", value, (key, old_value) -> old_value + value);
    }
}
```

*TryAdd* yöntemini kullanarak durum ekleyebilirsiniz. Bu yöntem, zaten var olan bir anahtar eklemeye çalıştığında atmaz.

```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public async Task AddCountAsync(int value)
    {
        bool result = await this.StateManager.TryAddStateAsync<int>("MyState", value);

        if (result)
        {
            // Added successfully!
        }
    }
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
class MyActorImpl extends FabricActor implements  MyActor
{
    public MyActorImpl(ActorService actorService, ActorId actorId)
    {
        super(actorService, actorId);
    }

    public CompletableFuture addCountAsync(int value)
    {
        return this.stateManager().tryAddStateAsync("MyState", value).thenApply((result)->{
            if(result)
            {
                // Added successfully!
            }
        });
    }
}
```

Bir aktör yönteminin sonunda, durum yöneticisi bir ekleme veya güncelleştirme işlemi tarafından eklenen veya değiştirilen değerleri otomatik olarak kaydeder. "Kaydet", kullanılan ayarlara bağlı olarak diskve çoğaltmada kalıcıolmayı içerebilir. Değiştirilmemiş değerler kalıcı veya çoğaltılmış değil. Hiçbir değer değiştirilmemişse, kaydet işlemi hiçbir şey yapmaz. Kaydetme başarısız olursa, değiştirilen durum atılır ve özgün durum yeniden yüklenir.

Ayrıca, aktör tabanındayöntemi `SaveStateAsync` arayarak durumu el ile kaydedebilirsiniz:

```csharp
async Task IMyActor.SetCountAsync(int count)
{
    await this.StateManager.AddOrUpdateStateAsync("count", count, (key, value) => count > value ? count : value);

    await this.SaveStateAsync();
}
```
```Java
interface MyActor {
    CompletableFuture setCountAsync(int count)
    {
        this.stateManager().addOrUpdateStateAsync("count", count, (key, value) -> count > value ? count : value).thenApply();

        this.stateManager().saveStateAsync().thenApply();
    }
}
```

## <a name="remove-state"></a>Durumu kaldırma
*Kaldır* yöntemini arayarak durumu kalıcı olarak bir aktörün eyalet yöneticisinden kaldırabilirsiniz. Bu yöntem, `KeyNotFoundException`var olmayan `NoSuchElementException`bir anahtarı kaldırmaya çalıştığında (C#) veya (Java) atar.

```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task RemoveCountAsync()
    {
        return this.StateManager.RemoveStateAsync("MyState");
    }
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
class MyActorImpl extends FabricActor implements  MyActor
{
    public MyActorImpl(ActorService actorService, ActorId actorId)
    {
        super(actorService, actorId);
    }

    public CompletableFuture removeCountAsync()
    {
        return this.stateManager().removeStateAsync("MyState");
    }
}
```

*TryRemove* yöntemini kullanarak durumu kalıcı olarak kaldırabilirsiniz. Bu yöntem, var olmayan bir anahtarı kaldırmaya çalıştığında atmaz.

```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public async Task RemoveCountAsync()
    {
        bool result = await this.StateManager.TryRemoveStateAsync("MyState");

        if (result)
        {
            // State removed!
        }
    }
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
class MyActorImpl extends FabricActor implements  MyActor
{
    public MyActorImpl(ActorService actorService, ActorId actorId)
    {
        super(actorService, actorId);
    }

    public CompletableFuture removeCountAsync()
    {
        return this.stateManager().tryRemoveStateAsync("MyState").thenApply((result)->{
            if(result)
            {
                // State removed!
            }
        });
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar

Güvenilir Aktörler'de depolanan durum, diske yazılmadan ve yüksek kullanılabilirlik için çoğaltılmadan önce seri hale getirilmelidir. [Aktör türü serileştirme](service-fabric-reliable-actors-notes-on-actor-type-serialization.md)hakkında daha fazla bilgi edinin.

Ardından, Aktör [tanılama ve performans izleme](service-fabric-reliable-actors-diagnostics.md)hakkında daha fazla bilgi edinin.
