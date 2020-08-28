---
title: Azure Service Fabric aktörleri silme
description: Azure Service Fabric uygulamasındaki Reliable Actors ve bunların durumunu el ile ve tamamen silmeyi öğrenin.
author: amanbha
ms.topic: conceptual
ms.date: 03/19/2018
ms.author: amanbha
ms.custom: devx-track-csharp
ms.openlocfilehash: 80192aef564317e36fba56025aa31c787676d974
ms.sourcegitcommit: 419cf179f9597936378ed5098ef77437dbf16295
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89006865"
---
# <a name="delete-reliable-actors-and-their-state"></a>Güvenilir Aktörleri ve durumlarını silme
Devre dışı bırakılmış aktörlerin atık toplama işlemi yalnızca aktör nesnesini temizler, ancak aktörün durum yöneticisinde depolanan verileri kaldırmaz. Bir aktör yeniden etkinleştirildiğinde, verileri durum Yöneticisi aracılığıyla tekrar kullanılabilir hale getirilir. Aktörlerin verileri durum Yöneticisi 'nde depolaması ve devre dışı bırakılmaması, ancak hiçbir zaman yeniden etkinleştirilmemesi durumunda, verilerinin temizlenmesi gerekebilir.

[Aktör hizmeti](service-fabric-reliable-actors-platform.md) , uzak bir çağırandan aktörleri silmek için bir işlev sağlar:

```csharp
ActorId actorToDelete = new ActorId(id);

IActorService myActorServiceProxy = ActorServiceProxy.Create(
    new Uri("fabric:/MyApp/MyService"), actorToDelete);

await myActorServiceProxy.DeleteActorAsync(actorToDelete, cancellationToken)
```
```Java
ActorId actorToDelete = new ActorId(id);

ActorService myActorServiceProxy = ActorServiceProxy.create(
    new Uri("fabric:/MyApp/MyService"), actorToDelete);

myActorServiceProxy.deleteActorAsync(actorToDelete);
```

Aktörün silinmesi, aktörün Şu anda etkin olup olmadığına bağlı olarak aşağıdaki etkilere sahiptir:

* **Etkin aktör**
  * Aktör etkin aktör listesinden kaldırıldı ve devre dışı bırakıldı.
  * Durumu kalıcı olarak silinir.
* **Etkin olmayan aktör**
  * Durumu kalıcı olarak silinir.

Aktör tek iş parçacıklı erişimi zorlamak için aktör çağrısının etrafında bir kilit edinildiği için bir aktör, kendisini gerçekleştiren metotlarından birinin içinden silme işlemi çağrılamaz.

Reliable Actors hakkında daha fazla bilgi için aşağıdakileri okuyun:
* [Aktör zamanlayıcılar ve anımsatıcıları](service-fabric-reliable-actors-timers-reminders.md)
* [Aktör olayları](service-fabric-reliable-actors-events.md)
* [Aktör yeniden girişi](service-fabric-reliable-actors-reentrancy.md)
* [Aktör tanılama ve performans izleme](service-fabric-reliable-actors-diagnostics.md)
* [Aktör API 'SI başvuru belgeleri](/previous-versions/azure/dn971626(v=azure.100))
* [C# örnek kodu](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [Java örnek kodu](https://github.com/Azure-Samples/service-fabric-java-getting-started)

<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-lifecycle/garbage-collection.png
