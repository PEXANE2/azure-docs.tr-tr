---
title: Reliable Actors çerçevesinde çok biçimlilik
description: İşlevselliği ve API tanımlarını yeniden kullanmak için Reliable Actors Framework 'te .NET arabirimlerinin ve türlerinin hiyerarşileri oluşturun.
author: vturecek
ms.topic: conceptual
ms.date: 11/02/2017
ms.author: vturecek
ms.openlocfilehash: 4e485463f41cdfbadeb166ecbb3a86d4a32c1589
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "75348932"
---
# <a name="polymorphism-in-the-reliable-actors-framework"></a>Reliable Actors çerçevesinde çok biçimlilik
Reliable Actors Framework, nesne odaklı tasarımda kullandığınız tekniklerin birçoğunu kullanarak aktör oluşturmanıza olanak sağlar. Bu tekniklerin biri çok biçimlerdir, bu da türlerin ve arabirimlerin daha Genelleştirilmiş üst öğeden devralınmasını sağlar. Reliable Actors Framework 'te devralma, genellikle birkaç ek kısıtlama içeren .NET modelini izler. Java/Linux durumunda Java modelini izler.

## <a name="interfaces"></a>Arabirimler
Reliable Actors Framework, aktör türü tarafından uygulanması için en az bir arabirim tanımlamanızı gerektirir. Bu arabirim, istemciler tarafından aktörlerle iletişim kurmak için kullanılabilecek bir ara sunucu sınıfı oluşturmak için kullanılır. Bir aktör türü tarafından uygulanan her arabirim ve tüm üst öğeleri ıaktör (C#) veya aktör (Java) ile türetilmediği sürece, arabirimler diğer arabirimlerden devralınabilir. Iaktör (C#) ve aktör (Java), sırasıyla .NET ve Java 'daki aktörler için platform tarafından tanımlanan temel arabirimlerdir. Bu nedenle, şekiller kullanılarak klasik çok biçimlilik örneği şuna benzer görünebilir:

![Şekil aktörleri için arabirim hiyerarşisi][shapes-interface-hierarchy]

## <a name="types"></a>Türler
Ayrıca, platform tarafından sunulan temel aktör sınıfından türetilmiş bir aktör türleri hiyerarşisi oluşturabilirsiniz. Şekiller söz konusu olduğunda, bir temel `Shape`(C#) veya `ShapeImpl`(Java) türü olabilir:

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

`Shape`(C#) veya `ShapeImpl`(Java) alt türleri, temel alınan yöntemleri geçersiz kılabilir.

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

Aktör türündeki `ActorService` özniteliği aklınızda edin. Bu öznitelik, güvenilir aktör çerçevesine, bu türden aktörlerin barındırılmasına yönelik otomatik olarak bir hizmet oluşturmasını söyler. Bazı durumlarda, yalnızca alt türlerine sahip işlevselliği paylaşmaya yönelik bir temel tür oluşturmak isteyebilirsiniz ve somut aktörlerin örneklendirilecek şekilde hiçbir zaman kullanılmaz. Bu durumlarda, bu tür temelinde hiçbir oyuncu `abstract` oluşturmayacağını belirtmek için anahtar sözcüğünü kullanmanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar
* Reliable Actors çerçevesinin güvenilirlik, ölçeklenebilirlik ve tutarlı durum sağlamak için [Service Fabric platformunu nasıl](service-fabric-reliable-actors-platform.md) kullandığını görün.
* [Aktör yaşam döngüsü](service-fabric-reliable-actors-lifecycle.md)hakkında bilgi edinin.

<!-- Image references -->

[shapes-interface-hierarchy]: ./media/service-fabric-reliable-actors-polymorphism/Shapes-Interface-Hierarchy.png
