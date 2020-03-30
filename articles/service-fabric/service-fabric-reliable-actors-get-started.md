---
title: Azure Hizmet Kumaşı'nda aktör tabanlı bir hizmet oluşturma
description: Service Fabric Reliable Actors'u kullanarak c#'da ilk aktör tabanlı hizmetinizi nasıl oluşturup, hata ayıklamave dağıtılamayı öğrenin.
author: vturecek
ms.topic: conceptual
ms.date: 07/10/2019
ms.author: vturecek
ms.openlocfilehash: a6e4fb48653572139463738c82de632ff7d55074
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75466260"
---
# <a name="getting-started-with-reliable-actors"></a>Güvenilir Aktörler le başlarken
> [!div class="op_single_selector"]
> * [Windows üzerinde C#](service-fabric-reliable-actors-get-started.md)
> * [Linux üzerinde Java](service-fabric-reliable-actors-get-started-java.md)

Bu makale, Visual Studio'da basit bir Güvenilir Aktör uygulaması oluşturma ve hata ayıklama yoluyla yürür. Güvenilir Aktörler hakkında daha fazla bilgi için, [Hizmet Kumaş Güvenilir Aktörler Giriş](service-fabric-reliable-actors-introduction.md)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce, Visual Studio da dahil olmak üzere, makinenize kurulum yapan Service Fabric geliştirme ortamına sahip olduğundan emin olun. Ayrıntılar [için, geliştirme ortamının nasıl ayarlanıldığını](service-fabric-get-started.md)görün.

## <a name="create-a-new-project-in-visual-studio"></a>Visual Studio'da yeni bir proje oluşturun

Visual Studio 2019 veya daha sonra yönetici olarak başlatın ve ardından yeni bir **Hizmet Kumaş Uygulaması** projesi oluşturun:

![Visual Studio için Servis Kumaş araçları - yeni proje][1]

Bir sonraki iletişim kutusunda **,NET Core 2.0** altında **Aktör Hizmeti'ni** seçin ve hizmet için bir ad girin.

![Service Fabric proje şablonları][5]

Oluşturulan proje aşağıdaki yapıyı gösterir:

![Servis Kumaşproje yapısı][2]

## <a name="examine-the-solution"></a>Çözümü inceleyin

Çözüm üç proje içerir:

* **Uygulama projesi (MyApplication)**. Bu proje dağıtım için tüm hizmetleri bir araya getirir. Uygulamayı yönetmek için *ApplicationManifest.xml* ve PowerShell komut dosyalarını içerir.

* **Arayüz projesi (HelloWorld.Interfaces)**. Bu proje aktör için arabirim tanımı nı içerir. Aktör arabirimleri herhangi bir projede herhangi bir ada sahip olarak tanımlanabilir.  Arabirim, aktör uygulaması ve aktörü çağıran istemciler tarafından paylaşılan aktör sözleşmesini tanımlar.  İstemci projeleri buna bağlı olabileceğinden, genellikle onu aktör uygulamasından ayrı bir derlemede tanımlamak mantıklıdır.

* **Aktör hizmet projesi (HelloWorld)**. Bu proje, aktörü barındıracak Hizmet Kumaşı hizmetini tanımlar. Bu aktör, *HelloWorld.cs*uygulanmasını içerir. Aktör uygulaması, temel türden `Actor` türeyen ve *MyActor.Interfaces* projesinde tanımlanan arabirimleri uygulayan bir sınıftır. Bir aktör sınıfı da bir örnek ve `ActorService` bir `ActorId` kabul eden ve `Actor` bunları taban sınıfa geçiren bir oluşturucu uygulamalıdır.
    
    Bu proje aynı zamanda *Program.cs*içerir , hangi kullanarak `ActorRuntime.RegisterActorAsync<T>()`Hizmet Kumaş çalışma süresi ile aktör sınıfları kaydeder. Sınıf `HelloWorld` zaten kayıtlı. Projeye eklenen ek aktör uygulamaları da `Main()` yönteme kaydedilmelidir.

## <a name="customize-the-helloworld-actor"></a>HelloWorld aktörözelleştirin

Proje şablonu `IHelloWorld` arabirimdeki bazı yöntemleri tanımlar ve `HelloWorld` bunları aktör uygulamasında uygular.  Aktör hizmeti basit bir "Hello World" dizesini döndürür, böylece bu yöntemleri değiştirin.

*HelloWorld.Interfaces* projesinde, *IHelloWorld.cs* dosyasında, arayüz tanımını aşağıdaki gibi değiştirin:

```csharp
public interface IHelloWorld : IActor
{
    Task<string> GetHelloWorldAsync();
}
```

**HelloWorld** projesinde, **HelloWorld.cs,** tüm sınıf tanımını aşağıdaki gibi değiştirin:

```csharp
[StatePersistence(StatePersistence.Persisted)]
internal class HelloWorld : Actor, IHelloWorld
{
    public HelloWorld(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task<string> GetHelloWorldAsync()
    {
        return Task.FromResult("Hello from my reliable actor!");
    }
}
```

Projeyi oluşturmak ve her şeyin derlediğinden emin olmak için **Ctrl-Shift-B** tuşuna basın.

## <a name="add-a-client"></a>İstemci ekleme

Aktör hizmetini aramak için basit bir konsol uygulaması oluşturun.

1. Çözüm Gezgini'nde çözüme sağ tıklayın > Yeni Proje **Ekle...** > **New Project...**.

2. **.NET Core** proje türlerine göre **Konsol Uygulaması'nı (.NET Core)** seçin.  Proje *ActorClient*adı .
    
    ![Yeni Proje iletişim kutusu ekle][6]    
    
    > [!NOTE]
    > Konsol uygulaması, Genellikle Hizmet Kumaşı'nda istemci olarak kullanacağınız uygulama türü değildir, ancak yerel Service Fabric kümesini kullanarak hata ayıklama ve test etmek için kullanışlı bir örnek olur.

3. Konsol uygulaması, arabirim projesi ve diğer bağımlılıklarla uyumluluğu korumak için 64 bit lik bir uygulama olmalıdır.  Çözüm Gezgini'nde **ActorClient** projesini sağ tıklatın ve ardından **Özellikler'i**tıklatın.  **Yapı** sekmesinde Platform **hedefini** **x64**olarak ayarlayın.
    
    ![Özellikler oluşturma][8]

4. İstemci projesi güvenilir aktörler NuGet paketi gerektirir.  **Araçlar** > **NuGet Paket Yöneticisi** > **Paket Yöneticisi Konsolu'nu**tıklatın.  Paket Yöneticisi Konsoluna aşağıdaki komutu girin:
    
    ```powershell
    Install-Package Microsoft.ServiceFabric.Actors -IncludePrerelease -ProjectName ActorClient
    ```

    NuGet paketi ve tüm bağımlılıkları ActorClient projesine yüklenir.

5. İstemci projesi de arabirimler projesiiçin bir başvuru gerektirir.  ActorClient projesinde, **Bağımlılıklar'ı** sağ tıklatın ve ardından **başvuru ekle'yi tıklatın...**.  **Çözüm > Projeler'i** seçin (zaten seçilmemişse) ve ardından **HelloWorld.Interfaces'in**yanındaki onay kutusunu işaretleyin.  **Tamam**'a tıklayın.
    
    ![Başvuru iletişim kutusu ekleme][7]

6. ActorClient projesinde, *Program.cs* tüm içeriğini aşağıdaki kodla değiştirin:
    
    ```csharp
    using System;
    using System.Threading.Tasks;
    using Microsoft.ServiceFabric.Actors;
    using Microsoft.ServiceFabric.Actors.Client;
    using HelloWorld.Interfaces;
    
    namespace ActorClient
    {
        class Program
        {
            static void Main(string[] args)
            {
                IHelloWorld actor = ActorProxy.Create<IHelloWorld>(ActorId.CreateRandom(), new Uri("fabric:/MyApplication/HelloWorldActorService"));
                Task<string> retval = actor.GetHelloWorldAsync();
                Console.Write(retval.Result);
                Console.ReadLine();
            }
        }
    }
    ```

## <a name="running-and-debugging"></a>Koşma ve hata ayıklama

Hizmeti Kumaş geliştirme kümesinde uygulamayı yerel olarak oluşturmak, dağıtmak ve çalıştırmak için **F5** tuşuna basın.  Dağıtım işlemi **sırasında, Çıktı** penceresinde ilerlemeyi görebilirsiniz.

![Servis Kumaş hata ayıklama çıkış penceresi][3]

Çıktı metni içerdiğinde, *uygulama hazırdır,* ActorClient uygulamasını kullanarak hizmeti sınamak mümkündür.  Solution Explorer'da **ActorClient** projesine sağ tıklayın ve hata **ayıklama** > **başlat'ı**tıklatın.  Komut satırı uygulaması aktör hizmetinden çıktı görüntülemelidir.

![Uygulama çıktısı][9]

> [!TIP]
> Service Fabric Actors çalışma zamanı aktör yöntemleri ile ilgili bazı [olaylar ve performans sayaçları](service-fabric-reliable-actors-diagnostics.md#actor-method-events-and-performance-counters)yakar. Tanılama ve performans izlemede yararlıdırlar.

## <a name="next-steps"></a>Sonraki adımlar
[Güvenilir Aktörlerin Service Fabric platformını nasıl kullandıkları](service-fabric-reliable-actors-platform.md)hakkında daha fazla bilgi edinin.


[1]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-newproject.PNG
[2]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-projectstructure.PNG
[3]: ./media/service-fabric-reliable-actors-get-started/debugging-output.PNG
[4]: ./media/service-fabric-reliable-actors-get-started/vs-context-menu.png
[5]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-newproject1.PNG
[6]: ./media/service-fabric-reliable-actors-get-started/new-console-app.png
[7]: ./media/service-fabric-reliable-actors-get-started/add-reference.png
[8]: ./media/service-fabric-reliable-actors-get-started/build-props.png
[9]: ./media/service-fabric-reliable-actors-get-started/app-output.png