---
title: Azure Service Fabric aktörleri için yeniden giriş
description: Çağrı bağlamına göre engellemeyi mantıksal olarak önlemenin bir yolu olan Service Fabric Reliable Actors için yeniden giriş yapın.
ms.topic: conceptual
ms.date: 11/02/2017
ms.custom: devx-track-csharp
ms.openlocfilehash: 9578b676e46d21fe5d30de92ad59c852a8b05f70
ms.sourcegitcommit: 67b44a02af0c8d615b35ec5e57a29d21419d7668
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/06/2021
ms.locfileid: "97915887"
---
# <a name="reliable-actors-reentrancy"></a>Reliable Actors yeniden giriş
Reliable Actors çalışma zamanı, varsayılan olarak mantıksal çağrı bağlamı tabanlı yeniden giriş yapılmasına izin verir. Bu, aktörlerin aynı çağrı bağlamı zincirinde olmaları durumunda yer almasına izin verir. Örneğin, oyuncu aktör C 'ye bir ileti gönderen aktör B 'ye bir ileti gönderir. İleti işlemenin bir parçası olarak, aktör C aktör A 'yı çağırırsa ileti yeniden alınır, bu nedenle izin verilir. Farklı bir çağrı bağlamının parçası olan diğer tüm iletiler, işleme bitene kadar aktör A 'da engellenir.

Sabit listesinde bir aktör yeniden girişi için kullanılabilen iki seçenek vardır `ActorReentrancyMode` :

* `LogicalCallContext` (varsayılan davranış)
* `Disallowed` -yeniden girişi devre dışı bırakır

```csharp
public enum ActorReentrancyMode
{
    LogicalCallContext = 1,
    Disallowed = 2
}
```
```Java
public enum ActorReentrancyMode
{
    LogicalCallContext(1),
    Disallowed(2)
}
```
Yeniden giriş, kayıt sırasında bir ' ın `ActorService` ayarlarından yapılandırılabilir. Ayar, aktör hizmetinde oluşturulan tüm aktör örneklerine uygulanır.

Aşağıdaki örnek, yeniden giriş modunu olarak ayarlayan bir aktör hizmetini gösterir `ActorReentrancyMode.Disallowed` . Bu durumda, bir aktör başka bir aktöre bir yeniden gelen iletisi gönderirse, türünde bir özel durum `FabricException` oluşturulur.

```csharp
static class Program
{
    static void Main()
    {
        try
        {
            ActorRuntime.RegisterActorAsync<Actor1>(
                (context, actorType) => new ActorService(
                    context,
                    actorType, () => new Actor1(),
                    settings: new ActorServiceSettings()
                    {
                        ActorConcurrencySettings = new ActorConcurrencySettings()
                        {
                            ReentrancyMode = ActorReentrancyMode.Disallowed
                        }
                    }))
                .GetAwaiter().GetResult();

            Thread.Sleep(Timeout.Infinite);
        }
        catch (Exception e)
        {
            ActorEventSource.Current.ActorHostInitializationFailed(e.ToString());
            throw;
        }
    }
}
```
```Java
static class Program
{
    static void Main()
    {
        try
        {
            ActorConcurrencySettings actorConcurrencySettings = new ActorConcurrencySettings();
            actorConcurrencySettings.setReentrancyMode(ActorReentrancyMode.Disallowed);

            ActorServiceSettings actorServiceSettings = new ActorServiceSettings();
            actorServiceSettings.setActorConcurrencySettings(actorConcurrencySettings);

            ActorRuntime.registerActorAsync(
                Actor1.getClass(),
                (context, actorType) -> new FabricActorService(
                    context,
                    actorType, () -> new Actor1(),
                    null,
                    stateProvider,
                    actorServiceSettings, timeout);

            Thread.sleep(Long.MAX_VALUE);
        }
        catch (Exception e)
        {
            throw e;
        }
    }
}
```


## <a name="next-steps"></a>Sonraki adımlar
* [Aktör API 'si başvuru belgelerinde](/dotnet/api/microsoft.servicefabric.actors?view=azure-dotnet)yeniden giriş hakkında daha fazla bilgi edinin
