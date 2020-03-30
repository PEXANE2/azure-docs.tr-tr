---
title: Azure Hizmet Kumaşı'nda aktörleri sayısala
description: Örnekler kullanarak Bir Azure Hizmet Kumaşı uygulamasında Güvenilir Aktörlerin numaralandırması ve bunların meta verileri hakkında bilgi edinin.
author: vturecek
ms.topic: conceptual
ms.date: 03/19/2018
ms.author: vturecek
ms.openlocfilehash: 1516c9005a7c4dd0adcb279e9954e5f882c575c1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75645608"
---
# <a name="enumerate-service-fabric-reliable-actors"></a>Omerate Servis Kumaş Güvenilir Aktörler
Güvenilir Aktörler hizmeti, istemcinin hizmetin barındırdığı aktörler le ilgili meta verileri sayısala masına olanak tanır. Aktör hizmeti bölümlenmiş bir durum hizmeti olduğundan, numaralandırma bölüm başına gerçekleştirilir. Her bölüm çok sayıda aktör içerebileceğinden, numaralandırma sayfalı sonuçlar kümesi olarak döndürülür. Tüm sayfalar okunana kadar sayfalar döngüye alınır. Aşağıdaki örnek, bir aktör hizmetinin tek bir bölümündeki tüm etkin aktörlerin listesini nasıl oluşturulur:

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
* [Aktör devlet yönetimi](service-fabric-reliable-actors-state-management.md)
* [Aktör yaşam döngüsü ve çöp toplama](service-fabric-reliable-actors-lifecycle.md)
* [Aktörler API başvuru belgeleri](https://msdn.microsoft.com/library/azure/dn971626.aspx)
* [.NET örnek kodu](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [Java örnek kodu](https://github.com/Azure-Samples/service-fabric-java-getting-started)

<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-platform/actor-service.png
[2]: ./media/service-fabric-reliable-actors-platform/app-deployment-scripts.png
[3]: ./media/service-fabric-reliable-actors-platform/actor-partition-info.png
[4]: ./media/service-fabric-reliable-actors-platform/actor-replica-role.png
[5]: ./media/service-fabric-reliable-actors-introduction/distribution.png
