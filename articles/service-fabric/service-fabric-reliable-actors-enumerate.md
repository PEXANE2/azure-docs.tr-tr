---
title: Azure Service Fabric aktörleri listeleme
description: Örnekleri kullanarak bir Azure Service Fabric uygulamasındaki Reliable Actors ve bunların meta verilerinin numaralandırılması hakkında bilgi edinin.
author: vturecek
ms.topic: conceptual
ms.date: 03/19/2018
ms.author: vturecek
ms.custom: devx-track-csharp
ms.openlocfilehash: dd3a61db32fb8e442beb42bd45c88da8559a29dd
ms.sourcegitcommit: 419cf179f9597936378ed5098ef77437dbf16295
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89016657"
---
# <a name="enumerate-service-fabric-reliable-actors"></a>Service Fabric Reliable Actors listeleme
Reliable Actors hizmeti, bir istemcinin, hizmetin barındırdığı aktörlerin meta verilerini listeletmesi için izin verir. Aktör hizmeti bölümlenmiş bir durum bilgisi olmayan hizmet olduğundan, bölüm başına listeleme gerçekleştirilir. Her bölümde birçok aktör olabileceği için, numaralandırma bir disk belleğine alınmış sonuçlar kümesi olarak döndürülür. Sayfalar, tüm sayfalar okunana kadar üzerine işlenir. Aşağıdaki örnek, bir aktör hizmetinin bir bölümündeki tüm etkin aktörlerin listesinin nasıl oluşturulacağını gösterir:

```csharp
IActorService actorServiceProxy = ActorServiceProxy.Create(
    new Uri("fabric:/MyApp/MyService"), partitionKey);

ContinuationToken continuationToken = null;
List<ActorInformation> activeActors = new List<ActorInformation>();

do
{
    PagedResult<ActorInformation> page = await actorServiceProxy.GetActorsAsync(continuationToken, cancellationToken);

    activeActors.AddRange(page.Items.Where(x => x.IsActive));

    continuationToken = page.ContinuationToken;
}
while (continuationToken != null);
```

```Java
ActorService actorServiceProxy = ActorServiceProxy.create(
    new URI("fabric:/MyApp/MyService"), partitionKey);

ContinuationToken continuationToken = null;
List<ActorInformation> activeActors = new ArrayList<ActorInformation>();

do
{
    PagedResult<ActorInformation> page = actorServiceProxy.getActorsAsync(continuationToken);

    while(ActorInformation x: page.getItems())
    {
         if(x.isActive()){
              activeActors.add(x);
         }
    }

    continuationToken = page.getContinuationToken();
}
while (continuationToken != null);
```



## <a name="next-steps"></a>Sonraki adımlar
* [Aktör durumu yönetimi](service-fabric-reliable-actors-state-management.md)
* [Aktör yaşam döngüsü ve çöp toplama](service-fabric-reliable-actors-lifecycle.md)
* [Aktör API 'SI başvuru belgeleri](/previous-versions/azure/dn971626(v=azure.100))
* [.NET örnek kodu](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [Java örnek kodu](https://github.com/Azure-Samples/service-fabric-java-getting-started)

<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-platform/actor-service.png
[2]: ./media/service-fabric-reliable-actors-platform/app-deployment-scripts.png
[3]: ./media/service-fabric-reliable-actors-platform/actor-partition-info.png
[4]: ./media/service-fabric-reliable-actors-platform/actor-replica-role.png
[5]: ./media/service-fabric-reliable-actors-introduction/distribution.png
