---
title: Azure Hizmet Kumaşı aktörlerinde yeniden canlandırma
description: Hizmet Kumaş Güvenilir Aktörler için reentrancy giriş, mantıksal çağrı bağlamına dayalı engelleme önlemek için bir yol.
author: vturecek
ms.topic: conceptual
ms.date: 11/02/2017
ms.author: vturecek
ms.openlocfilehash: 46ce91e607341e2fbdc0b6a3018e74cb24e76839
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75645540"
---
# <a name="reliable-actors-reentrancy"></a>Güvenilir Aktörler reentrancy
Güvenilir Aktörler çalışma süresi, varsayılan olarak, mantıksal arama bağlam tabanlı yeniden canlandırma sağlar. Bu, aktörlerin aynı çağrı bağlamı zincirinde olmaları durumunda yeniden canlandırmalarına olanak tanır. Örneğin, Aktör A, Aktör C'ye ileti gönderen Aktör B'ye bir ileti gönderir. İleti işlemenin bir parçası olarak, Aktör C Aktör A'yı çağırırsa, ileti yeniden işlem yapılır, bu nedenle iletiye izin verilir. Farklı bir çağrı bağlamının parçası olan diğer iletiler, işleme bitene kadar Aktör A'da engellenir.

`ActorReentrancyMode` Enum'da tanımlanan aktör reentrancy için iki seçenek vardır:

* `LogicalCallContext`(varsayılan davranış)
* `Disallowed`- yeniden canlandırma devre dışı

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
Reentrancy kayıt sırasında bir `ActorService`'ayarları nda yapılandırılabilir. Ayar, aktör hizmetinde oluşturulan tüm aktör örnekleri için geçerlidir.

Aşağıdaki örnekte, yeniden canlandırma modunu `ActorReentrancyMode.Disallowed`. Bu durumda, bir aktör başka bir aktöre reentrant iletisi gönderirse, tür `FabricException` bir özel durum atılır.

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
* [Aktör API başvuru belgelerinde](https://msdn.microsoft.com/library/azure/dn971626.aspx) yeniden entrancy hakkında daha fazla bilgi edinin
