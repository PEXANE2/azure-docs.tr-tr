---
title: Güvenilir Aktörler Çerçevesinde Polimorfizm
description: İşlevselliği ve API tanımlarını yeniden kullanmak için Güvenilir Aktörler çerçevesinde .NET arabirimlerinin ve türlerinin hiyerarşilerini oluşturun.
author: vturecek
ms.topic: conceptual
ms.date: 11/02/2017
ms.author: vturecek
ms.openlocfilehash: 4e485463f41cdfbadeb166ecbb3a86d4a32c1589
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75348932"
---
# <a name="polymorphism-in-the-reliable-actors-framework"></a>Güvenilir Aktörler Çerçevesinde Polimorfizm
Güvenilir Aktörler çerçevesi, nesne yönelimli tasarımda kullanacağınız aynı tekniklerin çoğunu kullanarak aktörler oluşturmanıza olanak tanır. Bu tekniklerden biri, türlerin ve arayüzlerin daha genelleştirilmiş ebeveynlerden devralmasına olanak tanıyan çok biçimliliktir. Güvenilir Aktörler çerçevesindeki kalıtım genellikle .NET modelini birkaç ek kısıtlamayla izler. Java/Linux durumunda Java modelini izler.

## <a name="interfaces"></a>Arabirimler
Güvenilir Aktörler çerçevesi, aktör türünüz tarafından uygulanacak en az bir arabirim tanımlamanızı gerektirir. Bu arabirim, aktörlerinizle iletişim kurmak için istemciler tarafından kullanılabilecek bir proxy sınıfı oluşturmak için kullanılır. Arabirimler, bir aktör türü tarafından uygulanan her arabirim ve tüm ebeveynleri sonuçta IActor(C#) veya Actor(Java) türetilmiştir sürece diğer arabirimlerden devralınabilir. IActor(C#) ve Actor(Java) sırasıyla .NET ve Java çerçevelerinde aktörler için platform tanımlı temel arabirimleridir. Böylece, şekilleri kullanarak klasik polimorfizm örneği şuna benzer:

![Şekil aktörleri için arabirim hiyerarşisi][shapes-interface-hierarchy]

## <a name="types"></a>Türler
Ayrıca, platform tarafından sağlanan temel Aktör sınıfından türetilen aktör türleri hiyerarşisi de oluşturabilirsiniz. Şekiller söz konusu olduğunda, bir `Shape`taban (C#) veya `ShapeImpl`(Java) türü ne olabilir:

```csharp
public abstract class Shape : Actor, IShape
{
    public abstract Task<int> GetVerticeCount();

    public abstract Task<double> GetAreaAsync();
}
```
```Java
public abstract class ShapeImpl extends FabricActor implements Shape
{
    public abstract CompletableFuture<int> getVerticeCount();

    public abstract CompletableFuture<double> getAreaAsync();
}
```

`Shape`(C#) veya `ShapeImpl`(Java) alt türleri temelden yöntemleri geçersiz kılabilir.

```csharp
[ActorService(Name = "Circle")]
[StatePersistence(StatePersistence.Persisted)]
public class Circle : Shape, ICircle
{
    public override Task<int> GetVerticeCount()
    {
        return Task.FromResult(0);
    }

    public override async Task<double> GetAreaAsync()
    {
        CircleState state = await this.StateManager.GetStateAsync<CircleState>("circle");

        return Math.PI *
            state.Radius *
            state.Radius;
    }
}
```
```Java
@ActorServiceAttribute(name = "Circle")
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
public class Circle extends ShapeImpl implements Circle
{
    @Override
    public CompletableFuture<Integer> getVerticeCount()
    {
        return CompletableFuture.completedFuture(0);
    }

    @Override
    public CompletableFuture<Double> getAreaAsync()
    {
        return (this.stateManager().getStateAsync<CircleState>("circle").thenApply(state->{
          return Math.PI * state.radius * state.radius;
        }));
    }
}
```

`ActorService` Aktör türündeki öznitelik dikkat edin. Bu öznitelik, Güvenilir Aktör çerçevesine, bu tür aktörleri barındırmak için otomatik olarak bir hizmet oluşturması gerektiğini söyler. Bazı durumlarda, yalnızca işlevselliği alt tiplerle paylaşmak için tasarlanmış ve somut aktörleri anında oluşturmak için asla kullanılmayacak bir taban türü oluşturmak isteyebilirsiniz. Bu gibi durumlarda, bu `abstract` türe dayalı bir aktör asla oluşturamayacağınızı belirtmek için anahtar sözcüğü kullanmanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar
* Güvenilir Aktörler çerçevesinin güvenilirlik, ölçeklenebilirlik ve tutarlı durum sağlamak için [Service Fabric platformunu nasıl kaldırdığını](service-fabric-reliable-actors-platform.md) görün.
* [Aktör yaşam döngüsü](service-fabric-reliable-actors-lifecycle.md)hakkında bilgi edinin.

<!-- Image references -->

[shapes-interface-hierarchy]: ./media/service-fabric-reliable-actors-polymorphism/Shapes-Interface-Hierarchy.png
