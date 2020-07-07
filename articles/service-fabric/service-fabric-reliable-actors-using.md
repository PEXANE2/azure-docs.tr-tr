---
title: Azure Service Fabric aktörlerin özelliklerini uygulama
description: StatefulService ' i kalýtýmla aynı şekilde hizmet düzeyi özellikleri uygulayan kendi aktör hizmetinizin nasıl yazılacağını açıklar.
author: vturecek
ms.topic: conceptual
ms.date: 03/19/2018
ms.author: vturecek
ms.openlocfilehash: 9f5f9e00c374b16026f22d4efdee51ec94d2902a
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "79502278"
---
# <a name="implement-service-level-features-in-your-actor-service"></a>Aktör hizmetinizde hizmet düzeyi özellikleri uygulama

[Hizmet katmanlama](service-fabric-reliable-actors-platform.md#service-layering)bölümünde açıklandığı gibi, aktör hizmeti güvenilir bir hizmettir. ' Den türetilen kendi hizmetinizi yazabilirsiniz `ActorService` . Ayrıca, hizmet düzeyi özellikleri durum bilgisi olan bir hizmeti kalýtýmla aynı şekilde uygulayabilirsiniz; örneğin:

- Hizmet yedekleme ve geri yükleme.
- Tüm aktörler için paylaşılan işlevsellik; Örneğin, devre kesici.
- Aktör hizmetinin kendisinde ve her bir aktör üzerinde uzak yordam çağrıları.

## <a name="use-the-actor-service"></a>Aktör hizmetini kullanma

Aktör örneklerinin çalıştıkları aktör hizmetine erişimi vardır. Aktör hizmeti sayesinde aktör örnekleri program aracılığıyla hizmet bağlamını elde edebilir. Hizmet bağlamının bölüm KIMLIĞI, hizmet adı, uygulama adı ve diğer Azure Service Fabric platforma özgü bilgileri vardır.

```csharp
Task MyActorMethod()
{
    Guid partitionId = this.ActorService.Context.PartitionId;
    string serviceTypeName = this.ActorService.Context.ServiceTypeName;
    Uri serviceInstanceName = this.ActorService.Context.ServiceName;
    string applicationInstanceName = this.ActorService.Context.CodePackageActivationContext.ApplicationName;
}
```
```Java
CompletableFuture<?> MyActorMethod()
{
    UUID partitionId = this.getActorService().getServiceContext().getPartitionId();
    String serviceTypeName = this.getActorService().getServiceContext().getServiceTypeName();
    URI serviceInstanceName = this.getActorService().getServiceContext().getServiceName();
    String applicationInstanceName = this.getActorService().getServiceContext().getCodePackageActivationContext().getApplicationName();
}
```

Tüm Reliable Services gibi, aktör hizmeti, Service Fabric çalışma zamanında bir hizmet türü ile kaydedilmelidir. Aktör hizmetinin aktör örneklerinizi çalıştırması için aktör türünün de aktör hizmetine kayıtlı olması gerekir. `ActorRuntime` kayıt yöntemi bu işi aktörlerin yerine gerçekleştirir. En basit durumda, aktör türünü kaydedebilir ve aktör hizmeti varsayılan ayarları kullanır.

```csharp
static class Program
{
    private static void Main()
    {
        ActorRuntime.RegisterActorAsync<MyActor>().GetAwaiter().GetResult();

        Thread.Sleep(Timeout.Infinite);
    }
}
```

Alternatif olarak, aktör hizmetini kendiniz oluşturmak için kayıt yöntemiyle sunulan bir lambda kullanabilirsiniz. Ardından aktör hizmetini yapılandırabilir ve aktör örneklerinizi açıkça oluşturabilirsiniz. Aktörünüz için Oluşturucu aracılığıyla bağımlılıklar ekleyebilirsiniz.

```csharp
static class Program
{
    private static void Main()
    {
        ActorRuntime.RegisterActorAsync<MyActor>(
            (context, actorType) => new ActorService(context, actorType, () => new MyActor()))
            .GetAwaiter().GetResult();

        Thread.Sleep(Timeout.Infinite);
    }
}
```
```Java
static class Program
{
    private static void Main()
    {
      ActorRuntime.registerActorAsync(
              MyActor.class,
              (context, actorTypeInfo) -> new FabricActorService(context, actorTypeInfo),
              timeout);

        Thread.sleep(Long.MAX_VALUE);
    }
}
```

## <a name="actor-service-methods"></a>Aktör hizmeti yöntemleri

Aktör hizmeti, (c#) veya (Java) uygulayan (c#) veya (Java) uygular `IActorService` `ActorService` `IService` `Service` . Bu arabirim, hizmet yöntemlerinde uzak yordam çağrılarına izin veren Reliable Services uzaktan iletişim tarafından kullanılır. Hizmet uzaktan iletişim üzerinden çağrılabilen hizmet düzeyi yöntemleri içerir. Aktörleri [listelemek](service-fabric-reliable-actors-enumerate.md) ve [silmek](service-fabric-reliable-actors-delete-actors.md) için kullanabilirsiniz.


## <a name="custom-actor-service"></a>Özel aktör hizmeti

Aktör kayıt lambdası kullanarak, `ActorService` (C#) ve `FabricActorService` (Java) ' den türetilen kendi özel aktör hizmetinizi kaydedebilirsiniz. Daha sonra, `ActorService` (C#) veya `FabricActorService` (Java) devralan bir hizmet sınıfı yazarak kendi hizmet düzeyi işlevselliklerinizi uygulayabilirsiniz. Özel aktör hizmeti, `ActorService` (C#) veya `FabricActorService` (Java) içindeki tüm aktör çalışma zamanı işlevlerini devralır. Kendi hizmet yöntemlerinizi uygulamak için kullanılabilir.

```csharp
class MyActorService : ActorService
{
    public MyActorService(StatefulServiceContext context, ActorTypeInformation typeInfo, Func<ActorBase> newActor)
        : base(context, typeInfo, newActor)
    { }
}
```
```Java
class MyActorService extends FabricActorService
{
    public MyActorService(StatefulServiceContext context, ActorTypeInformation typeInfo, BiFunction<FabricActorService, ActorId, ActorBase> newActor)
    {
         super(context, typeInfo, newActor);
    }
}
```

```csharp
static class Program
{
    private static void Main()
    {
        ActorRuntime.RegisterActorAsync<MyActor>(
            (context, actorType) => new MyActorService(context, actorType, () => new MyActor()))
            .GetAwaiter().GetResult();

        Thread.Sleep(Timeout.Infinite);
    }
}
```
```Java
public class Program
{
    public static void main(String[] args)
    {
        ActorRuntime.registerActorAsync(
                MyActor.class,
                (context, actorTypeInfo) -> new FabricActorService(context, actorTypeInfo),
                timeout);
        Thread.sleep(Long.MAX_VALUE);
    }
}
```

## <a name="implement-actor-backup-and-restore"></a>Aktör yedekleme ve geri yükleme uygulama

Özel bir aktör hizmeti, ' de zaten mevcut olan uzaktan iletişim dinleyicisinden yararlanarak aktör verilerini yedeklemek için bir yöntem sunabilir `ActorService` . Bir örnek için bkz. [aktörleri yedekleme ve geri yükleme](service-fabric-reliable-actors-backup-and-restore.md).

## <a name="actor-that-uses-a-remoting-v2-interface-compatible-stack"></a>Remoting v2 (arabirim uyumlu) yığınını kullanan aktör

Remoting v2 (arabirim uyumlu, V2_1 olarak bilinir) yığını, V2 Uzaktan iletişim yığınının tüm özelliklerine sahiptir. Arabirimi, uzaktan iletişim v1 Stack ile uyumludur, ancak v2 ve v1 ile geriye dönük olarak uyumlu değildir. Hizmet kullanılabilirliği üzerinde hiçbir etkisi olmadan v1 'den V2_1 'e yükseltmek için sonraki bölümde yer alan adımları izleyin.

Uzaktan iletişim V2_1 yığınını kullanmak için aşağıdaki değişiklikler gereklidir:

1. Aktör arabirimlerine aşağıdaki derleme özniteliğini ekleyin.
  
   ```csharp
   [assembly:FabricTransportActorRemotingProvider(RemotingListenerVersion = RemotingListenerVersion.V2_1,RemotingClientVersion = RemotingClientVersion.V2_1)]
   ```

2. V2 yığınını kullanmaya başlamak için aktör hizmeti ve aktör istemci projelerini derleyin ve yükseltin.

### <a name="actor-service-upgrade-to-remoting-v2-interface-compatible-stack-without-affecting-service-availability"></a>Aktör hizmeti hizmeti kullanılabilirliğini etkilemeden uzaktan iletişim v2 (arabirim uyumlu) yığınına yükseltme

Bu değişiklik, iki adımlı bir yükseltmeye sahiptir. Bu dizideki adımları izleyin.

1. Aktör arabirimlerine aşağıdaki derleme özniteliğini ekleyin. Bu öznitelik aktör hizmeti, v1 (mevcut) ve V2_1 dinleyicisi için iki dinleyici başlatır. Aktör hizmetini bu değişiklik ile yükseltin.

   ```csharp
   [assembly:FabricTransportActorRemotingProvider(RemotingListenerVersion = RemotingListenerVersion.V1|RemotingListenerVersion.V2_1,RemotingClientVersion = RemotingClientVersion.V2_1)]
   ```

2. Önceki yükseltmeyi tamamladıktan sonra aktör istemcilerini yükseltin.
   Bu adım, aktör proxy 'sinin uzaktan iletişim V2_1 yığınını kullandığından emin olur.

3. Bu adım isteğe bağlıdır. V1 dinleyicisini kaldırmak için önceki özniteliği değiştirin.

    ```csharp
    [assembly:FabricTransportActorRemotingProvider(RemotingListenerVersion = RemotingListenerVersion.V2_1,RemotingClientVersion = RemotingClientVersion.V2_1)]
    ```

## <a name="actor-that-uses-the-remoting-v2-stack"></a>Remoting v2 yığınını kullanan aktör

Sürüm 2,8 NuGet paketiyle, kullanıcılar artık daha iyi performans sağlayan ve özel serileştirme gibi özellikler sağlayan Remoting v2 yığınını kullanabilir. Remoting v2, mevcut uzaktan iletişim yığını ile geriye dönük olarak uyumlu değil (şimdi v1 uzaktan iletişim yığını olarak adlandırılır).

Remoting v2 yığınını kullanmak için aşağıdaki değişiklikler gereklidir.

1. Aktör arabirimlerine aşağıdaki derleme özniteliğini ekleyin.

   ```csharp
   [assembly:FabricTransportActorRemotingProvider(RemotingListenerVersion = RemotingListenerVersion.V2,RemotingClientVersion = RemotingClientVersion.V2)]
   ```

2. V2 yığınını kullanmaya başlamak için aktör hizmeti ve aktör istemci projelerini derleyin ve yükseltin.

### <a name="upgrade-the-actor-service-to-the-remoting-v2-stack-without-affecting-service-availability"></a>Aktör hizmetini, hizmet kullanılabilirliğini etkilemeden Remoting v2 yığınına yükseltin

Bu değişiklik, iki adımlı bir yükseltmeye sahiptir. Bu dizideki adımları izleyin.

1. Aktör arabirimlerine aşağıdaki derleme özniteliğini ekleyin. Bu öznitelik aktör hizmeti, v1 (mevcut) ve v2 dinleyicisi için iki dinleyici başlatır. Aktör hizmetini bu değişiklik ile yükseltin.

   ```csharp
   [assembly:FabricTransportActorRemotingProvider(RemotingListenerVersion = RemotingListenerVersion.V1|RemotingListenerVersion.V2,RemotingClientVersion = RemotingClientVersion.V2)]
   ```

2. Önceki yükseltmeyi tamamladıktan sonra aktör istemcilerini yükseltin.
   Bu adım, aktör proxy 'sinin Remoting v2 yığınını kullandığından emin olur.

3. Bu adım isteğe bağlıdır. V1 dinleyicisini kaldırmak için önceki özniteliği değiştirin.

    ```csharp
    [assembly:FabricTransportActorRemotingProvider(RemotingListenerVersion = RemotingListenerVersion.V2,RemotingClientVersion = RemotingClientVersion.V2)]
    ```

## <a name="next-steps"></a>Sonraki adımlar

* [Aktör durumu yönetimi](service-fabric-reliable-actors-state-management.md)
* [Aktör yaşam döngüsü ve çöp toplama](service-fabric-reliable-actors-lifecycle.md)
* [Aktör API 'SI başvuru belgeleri](https://msdn.microsoft.com/library/azure/dn971626.aspx)
* [.NET örnek kodu](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [Java örnek kodu](https://github.com/Azure-Samples/service-fabric-java-getting-started)

<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-platform/actor-service.png
[2]: ./media/service-fabric-reliable-actors-platform/app-deployment-scripts.png
[3]: ./media/service-fabric-reliable-actors-platform/actor-partition-info.png
[4]: ./media/service-fabric-reliable-actors-platform/actor-replica-role.png
[5]: ./media/service-fabric-reliable-actors-introduction/distribution.png
