---
title: Aktör tabanlı Azure Hizmet Kumaşı aktörlerinde etkinlikler
description: Hizmet Kumaş Güvenilir Aktörler, aktör ve müşteri arasında iletişim kurmak için etkili bir yol için olaylar hakkında bilgi edinin.
author: vturecek
ms.topic: conceptual
ms.date: 10/06/2017
ms.author: amanbha
ms.openlocfilehash: 73c149a0d0992fecd1acf633891057570285df64
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75639675"
---
# <a name="actor-events"></a>Aktör etkinlikleri
Aktör olayları, aktörden istemcilere en iyi çabayı gösterme bildirimleri göndermenin bir yolunu sağlar. Aktör etkinlikleri aktörden müşteriye iletişim için tasarlanmıştır ve aktör-oyuncu iletişimi için kullanılmamalıdır.

Aşağıdaki kod parçacıkları, uygulamanızda aktör olaylarının nasıl kullanılacağını gösterir.

Aktör tarafından yayınlanan olayları açıklayan bir arabirim tanımlayın. Bu arabirim `IActorEvents` arabirimden türetilmelidir. Yöntemlerin bağımsız değişkenleri [veri sözleşmesi serializable](service-fabric-reliable-actors-notes-on-actor-type-serialization.md)olmalıdır. Olay bildirimleri tek yönlü ve en iyi çaba olduğundan, yöntemler geçersiz olarak döndürülmelidir.

```csharp
public interface IGameEvents : IActorEvents
{
    void GameScoreUpdated(Guid gameId, string currentScore);
}
```
```Java
public interface GameEvents implements ActorEvents
{
    void gameScoreUpdated(UUID gameId, String currentScore);
}
```
Aktör arabiriminde aktör tarafından yayınlanan olayları bildirin.

```csharp
public interface IGameActor : IActor, IActorEventPublisher<IGameEvents>
{
    Task UpdateGameStatus(GameStatus status);

    Task<string> GetGameScore();
}
```
```Java
public interface GameActor extends Actor, ActorEventPublisherE<GameEvents>
{
    CompletableFuture<?> updateGameStatus(GameStatus status);

    CompletableFuture<String> getGameScore();
}
```
İstemci tarafında, olay işleyicisi uygulayın.

```csharp
class GameEventsHandler : IGameEvents
{
    public void GameScoreUpdated(Guid gameId, string currentScore)
    {
        Console.WriteLine(@"Updates: Game: {0}, Score: {1}", gameId, currentScore);
    }
}
```

```Java
class GameEventsHandler implements GameEvents {
    public void gameScoreUpdated(UUID gameId, String currentScore)
    {
        System.out.println("Updates: Game: "+gameId+" ,Score: "+currentScore);
    }
}
```

İstemci üzerinde, olayı yayımlayan aktöre bir proxy oluşturun ve etkinliklerine abone olun.

```csharp
var proxy = ActorProxy.Create<IGameActor>(
                    new ActorId(Guid.Parse(arg)), ApplicationName);

await proxy.SubscribeAsync<IGameEvents>(new GameEventsHandler());
```

```Java
GameActor actorProxy = ActorProxyBase.create<GameActor>(GameActor.class, new ActorId(UUID.fromString(args)));

return ActorProxyEventUtility.subscribeAsync(actorProxy, new GameEventsHandler());
```

Başarısız olması durumunda, aktör farklı bir işlem veya düğüm üzerinde başarısız olabilir. Aktör proxy etkin abonelikleri yönetir ve otomatik olarak yeniden abone olur. `ActorProxyEventExtensions.SubscribeAsync<TEvent>` YENIDEN abonelik aralığını API üzerinden denetleyebilirsiniz. Aboneliği iptal etmek `ActorProxyEventExtensions.UnsubscribeAsync<TEvent>` için API'yi kullanın.

Aktör, olayları olduğu gibi yayınlayın. Etkinliğin aboneleri varsa, Aktörler çalışma zamanı onlara bildirimi gönderir.

```csharp
var ev = GetEvent<IGameEvents>();
ev.GameScoreUpdated(Id.GetGuidId(), score);
```
```Java
GameEvents event = getEvent<GameEvents>(GameEvents.class);
event.gameScoreUpdated(Id.getUUIDId(), score);
```


## <a name="next-steps"></a>Sonraki adımlar
* [Aktör reentrancy](service-fabric-reliable-actors-reentrancy.md)
* [Aktör tanılama ve performans izleme](service-fabric-reliable-actors-diagnostics.md)
* [Aktör API başvuru belgeleri](https://msdn.microsoft.com/library/azure/dn971626.aspx)
* [C# Örnek kod](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [C# .NET Çekirdek Örnek kodu](https://github.com/Azure-Samples/service-fabric-dotnet-core-getting-started)
* [Java Örnek kodu](https://github.com/Azure-Samples/service-fabric-java-getting-started)
