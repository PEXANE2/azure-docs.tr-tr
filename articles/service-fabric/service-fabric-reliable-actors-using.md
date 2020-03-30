---
title: Azure Service Fabric aktörlerinde özellikleri uygulama
description: Service düzeyindeki özellikleri, StatefulService'i devraldığınızda olduğu gibi uygulayan kendi aktör hizmetinizi nasıl yazacağınızı açıklar.
author: vturecek
ms.topic: conceptual
ms.date: 03/19/2018
ms.author: vturecek
ms.openlocfilehash: 55ee4c7498dcda3060d4e4221711793b80132bdf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79502278"
---
# <a name="implement-service-level-features-in-your-actor-service"></a>Aktör hizmetinizde hizmet düzeyi özelliklerini uygulama

[Hizmet katmanlamada](service-fabric-reliable-actors-platform.md#service-layering)açıklandığı gibi, aktör hizmetinin kendisi güvenilir bir hizmettir. Eğer türetilmiş kendi hizmet `ActorService`yazabilirsiniz. Ayrıca, hizmet düzeyi özelliklerini, aşağıdakiler gibi, devlet hizmeti devraldığınız zamanki gibi uygulayabilirsiniz:

- Servis yedekleme ve geri yükleme.
- Tüm aktörler için paylaşılan işlevsellik, örneğin, bir devre kesici.
- Uzaktan yordam aktör hizmeti kendisi ve her aktör çağırır.

## <a name="use-the-actor-service"></a>Aktör hizmetini kullanma

Aktör örnekleri, çalıştıkları aktör hizmetine erişebilir. Aktör hizmeti aracılığıyla, aktör örnekleri programlı olarak hizmet bağlamını elde edebilirsiniz. Hizmet bağlamında bölüm kimliği, hizmet adı, uygulama adı ve diğer Azure Hizmet Kumaşı platformuna özgü bilgiler vardır.

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

Tüm Güvenilir Hizmetler gibi, aktör hizmeti de Service Fabric çalışma zamanında bir hizmet türüyle kaydedilmelidir. Aktör hizmetinin aktör örneklerinizi çalıştırabilmesi için aktör türünüzün de aktör hizmetine kaydedilmesi gerekir. `ActorRuntime` kayıt yöntemi bu işi aktörlerin yerine gerçekleştirir. En basit durumda, aktör türünüzü kaydedebilirsiniz ve aktör hizmeti varsayılan ayarları kullanır.

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

Alternatif olarak, aktör hizmeti kendiniz oluşturmak için kayıt yöntemi tarafından sağlanan bir lambda kullanabilirsiniz. Daha sonra aktör hizmetini yapılandırabilir ve aktör örneklerinizi açıkça oluşturabilirsiniz. Aktörünüze bağımlılıkları yapıcısı aracılığıyla enjekte edebilirsiniz.

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

## <a name="actor-service-methods"></a>Aktör hizmet yöntemleri

Aktör hizmeti uygular `IActorService` (C#) `ActorService` veya (Java), hangi `IService` sırayla (C# veya `Service` (Java). Bu arabirim, servis yöntemlerine uzaktan yordam çağrıları na izin veren Güvenilir Hizmetler remotingtarafından kullanılır. Hizmet remoting yoluyla uzaktan çağrılabilen hizmet düzeyi yöntemleri içerir. Aktörleri [sayısallandırmak](service-fabric-reliable-actors-enumerate.md) ve [silmek](service-fabric-reliable-actors-delete-actors.md) için kullanabilirsiniz.


## <a name="custom-actor-service"></a>Özel aktör hizmeti

Aktör kaydı lambda kullanarak, `ActorService` (C# ) ve `FabricActorService` (Java) türetilen kendi özel aktör hizmeti kaydedebilirsiniz. Daha sonra devralan `ActorService` (C#) veya `FabricActorService` (Java) bir hizmet sınıfı yazarak kendi hizmet düzeyi işlevselliğinizi uygulayabilirsiniz. Özel bir aktör hizmeti( C# ) `ActorService` veya `FabricActorService` (Java) tüm aktör çalışma zamanı işlevselliği devralır. Kendi hizmet yöntemlerinizi uygulamak için kullanılabilir.

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

Özel bir aktör hizmeti zaten mevcut remoting dinleyici yararlanarak aktör verilerini yedeklemek için bir yöntem ortaya `ActorService`çıkarabilir. Örneğin, Yedekleme [ve geri yükleme aktörleri](../synapse-analytics/sql-data-warehouse/backup-and-restore.md)bakın.

## <a name="actor-that-uses-a-remoting-v2-interface-compatible-stack"></a>Remoting V2 (arabirim uyumlu) yığını kullanan aktör

Remoting V2 (arayüz uyumlu, V2_1 olarak bilinen) yığını V2 remoting yığınının tüm özelliklerine sahiptir. Arayüzü remoting V1 yığını ile uyumludur, ancak V2 ve V1 ile geriye doğru uyumlu değildir. V1'den V2_1'a hizmet kullanılabilirliği üzerinde hiçbir etkisi olmayan yükseltme yapmak için bir sonraki bölümdeki adımları izleyin.

Remoting V2_1 yığınını kullanmak için aşağıdaki değişiklikler gereklidir:

1. Aktör arabirimlerinde aşağıdaki derleme özniteliğini ekleyin.
  
   ```csharp
   [assembly:FabricTransportActorRemotingProvider(RemotingListenerVersion = RemotingListenerVersion.V2_1,RemotingClientVersion = RemotingClientVersion.V2_1)]
   ```

2. V2 yığınını kullanmaya başlamak için aktör hizmeti ve aktör istemci projeleri oluşturun ve yükseltin.

### <a name="actor-service-upgrade-to-remoting-v2-interface-compatible-stack-without-affecting-service-availability"></a>Hizmet kullanılabilirliğini etkilemeden V2 (arayüz uyumlu) yığınını yeniden ifade etmek için aktör hizmet yükseltmesi

Bu değişiklik iki adımlı bir yükseltmedir. Bu sırada ki adımları izleyin.

1. Aktör arabirimlerinde aşağıdaki derleme özniteliğini ekleyin. Bu öznitelik aktör hizmeti, V1 (varolan) ve V2_1 dinleyici için iki dinleyici başlar. Bu değişiklikle aktör hizmetini yükseltin.

   ```csharp
   [assembly:FabricTransportActorRemotingProvider(RemotingListenerVersion = RemotingListenerVersion.V1|RemotingListenerVersion.V2_1,RemotingClientVersion = RemotingClientVersion.V2_1)]
   ```

2. Önceki yükseltmeyi tamamladıktan sonra aktör istemcileri yükseltin.
   Bu adım, aktör proxy remoting V2_1 yığını kullanır emin olmasını sağlar.

3. Bu adım isteğe bağlıdır. V1 dinleyicisini kaldırmak için önceki özniteliği değiştirin.

    ```csharp
    [assembly:FabricTransportActorRemotingProvider(RemotingListenerVersion = RemotingListenerVersion.V2_1,RemotingClientVersion = RemotingClientVersion.V2_1)]
    ```

## <a name="actor-that-uses-the-remoting-v2-stack"></a>Remoting V2 yığınını kullanan aktör

Sürüm 2.8 NuGet paketi ile, kullanıcılar artık daha iyi performans ve özel serileştirme gibi özellikler sağlayan remoting V2 yığını kullanabilirsiniz. V2 remoting varolan remoting yığını (şimdi V1 remoting yığını olarak adlandırılır) ile geriye doğru uyumlu değildir.

Remoting V2 yığınını kullanmak için aşağıdaki değişiklikler gereklidir.

1. Aktör arabirimlerinde aşağıdaki derleme özniteliğini ekleyin.

   ```csharp
   [assembly:FabricTransportActorRemotingProvider(RemotingListenerVersion = RemotingListenerVersion.V2,RemotingClientVersion = RemotingClientVersion.V2)]
   ```

2. V2 yığınını kullanmaya başlamak için aktör hizmeti ve aktör istemci projelerini oluşturun ve yükseltin.

### <a name="upgrade-the-actor-service-to-the-remoting-v2-stack-without-affecting-service-availability"></a>Oyuncu hizmetini, hizmet kullanılabilirliğini etkilemeden yeniden ifade eden V2 yığınına yükseltin

Bu değişiklik iki adımlı bir yükseltmedir. Bu sırada ki adımları izleyin.

1. Aktör arabirimlerinde aşağıdaki derleme özniteliğini ekleyin. Bu öznitelik aktör hizmeti için iki dinleyici başlar, V1 (varolan) ve V2 dinleyici. Bu değişiklikle aktör hizmetini yükseltin.

   ```csharp
   [assembly:FabricTransportActorRemotingProvider(RemotingListenerVersion = RemotingListenerVersion.V1|RemotingListenerVersion.V2,RemotingClientVersion = RemotingClientVersion.V2)]
   ```

2. Önceki yükseltmeyi tamamladıktan sonra aktör istemcileri yükseltin.
   Bu adım, aktör proxy remoting V2 yığını nı kullandığından emin olun.

3. Bu adım isteğe bağlıdır. V1 dinleyicisini kaldırmak için önceki özniteliği değiştirin.

    ```csharp
    [assembly:FabricTransportActorRemotingProvider(RemotingListenerVersion = RemotingListenerVersion.V2,RemotingClientVersion = RemotingClientVersion.V2)]
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
