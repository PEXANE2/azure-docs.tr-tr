---
title: Azure Hizmet Kumaşı aktörlerini silme
description: Azure Hizmet Kumaşı uygulamasında Güvenilir Aktörleri ve durumlarını el ile ve tam olarak nasıl silebilirsiniz öğrenin.
author: amanbha
ms.topic: conceptual
ms.date: 03/19/2018
ms.author: amanbha
ms.openlocfilehash: b90c5a10c64e273f1c8f48c7bf5713859796db65
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75645625"
---
# <a name="delete-reliable-actors-and-their-state"></a>Güvenilir Aktörleri ve durumlarını silme
Devre dışı bırakılan aktörlerin çöp toplaması yalnızca aktör nesnesini temizler, ancak bir aktörün Durum Yöneticisi'nde depolanan verileri kaldırmaz. Bir aktör yeniden etkinleştirildiğinde, verileri yine Devlet Yöneticisi aracılığıyla kullanılabilir hale getirilir. Aktörlerin verileri Devlet Yöneticisi'nde depoladığı ve devre dışı bırakıldığı ancak hiçbir zaman yeniden etkinleştirilmediği durumlarda, verilerini temizlemek gerekebilir.

[Aktör Hizmeti,](service-fabric-reliable-actors-platform.md) aktörleri uzaktan arayandan silen bir işlev sağlar:

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

Bir aktörü silerken, aktörün şu anda etkin olup olmadığına bağlı olarak aşağıdaki efektler vardır:

* **Aktif Aktör**
  * Aktör etkin aktörler listesinden kaldırılır ve devre dışı bırakılır.
  * Durumu kalıcı olarak silinir.
* **Aktif Olmayan Aktör**
  * Durumu kalıcı olarak silinir.

Aktör, tek iş parçacığı erişimini zorlamak için aktör çağrısının etrafında bir kilit aldığı bir aktör çağrı bağlamında yürütülürken silinemediği için aktör kendi aktör yöntemlerinden birinden silmeyi çağıramaz.

Güvenilir Aktörler hakkında daha fazla bilgi için aşağıdakileri okuyun:
* [Aktör zamanlayıcıları ve anımsatıcılar](service-fabric-reliable-actors-timers-reminders.md)
* [Aktör etkinlikleri](service-fabric-reliable-actors-events.md)
* [Aktör reentrancy](service-fabric-reliable-actors-reentrancy.md)
* [Aktör tanılama ve performans izleme](service-fabric-reliable-actors-diagnostics.md)
* [Aktör API başvuru belgeleri](https://msdn.microsoft.com/library/azure/dn971626.aspx)
* [C# Örnek kod](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [Java Örnek kodu](https://github.com/Azure-Samples/service-fabric-java-getting-started)

<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-lifecycle/garbage-collection.png
