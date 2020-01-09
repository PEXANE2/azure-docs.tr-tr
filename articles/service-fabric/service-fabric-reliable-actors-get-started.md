---
title: Azure Service Fabric aktör tabanlı hizmet oluşturma
description: Service Fabric Reliable Actors C# kullanarak ilk aktör tabanlı hizmetinizi oluşturma, hata ayıklama ve dağıtma hakkında bilgi edinin.
author: vturecek
ms.topic: conceptual
ms.date: 07/10/2019
ms.author: vturecek
ms.openlocfilehash: a6e4fb48653572139463738c82de632ff7d55074
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75466260"
---
# <a name="getting-started-with-reliable-actors"></a>Reliable Actors kullanmaya başlama
> [!div class="op_single_selector"]
> * [Windows üzerinde C#](service-fabric-reliable-actors-get-started.md)
> * [Linux üzerinde Java](service-fabric-reliable-actors-get-started-java.md)

Bu makalede, Visual Studio 'da basit bir güvenilir aktör uygulamasının oluşturulması ve hata ayıklaması adım adım anlatılmaktadır. Reliable Actors hakkında daha fazla bilgi için bkz. [Service Fabric Reliable Actors giriş](service-fabric-reliable-actors-introduction.md).

## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce, makinenizde ayarlanmış olan Visual Studio dahil Service Fabric geliştirme ortamına sahip olduğunuzdan emin olun. Ayrıntılar için bkz. [geliştirme ortamını ayarlama](service-fabric-get-started.md).

## <a name="create-a-new-project-in-visual-studio"></a>Visual Studio 'da yeni proje oluşturma

Visual Studio 2019 veya üstünü yönetici olarak başlatın ve ardından yeni bir **Service Fabric uygulama** projesi oluşturun:

![Visual Studio için Service Fabric araçları-yeni proje][1]

Sonraki iletişim kutusunda, **.NET Core 2,0** altında **aktör hizmeti** ' ni seçin ve hizmet için bir ad girin.

![Service Fabric proje şablonları][5]

Oluşturulan proje aşağıdaki yapıyı gösterir:

![Service Fabric proje yapısı][2]

## <a name="examine-the-solution"></a>Çözümü inceleyin

Çözüm üç proje içerir:

* **Uygulama Projesi (MyApplication)** . Bu proje, tüm hizmetleri dağıtım için birlikte paketler. Uygulamayı yönetmeye yönelik *ApplicationManifest. xml* ve PowerShell betiklerini içerir.

* **Arabirim projesi (HelloWorld. Interfaces)** . Bu proje aktör için arabirim tanımını içerir. Aktör arabirimleri, herhangi bir ada sahip herhangi bir projede tanımlanabilir.  Arabirim, aktör uygulamasıyla paylaşılan aktör sözleşmesini ve aktör çağıran istemcileri tanımlar.  İstemci projeleri kendisine bağlı olabileceğinden, genellikle onu aktör uygulamasından ayrı bir derlemede tanımlamak mantıklı olur.

* **Aktör hizmeti projesi (HelloWorld)** . Bu proje, aktöri barındıracak Service Fabric hizmetini tanımlar. Aktör, *HelloWorld.cs*uygulamasını içerir. Aktör uygulama `Actor` temel türden türetilen ve *Myactor. Interfaces* projesinde tanımlanan arabirimleri uygulayan bir sınıftır. Aktör sınıfı ayrıca, bir `ActorService` örneği ve `ActorId` kabul eden ve bunları temel `Actor` sınıfına ileten bir Oluşturucu uygulamalıdır.
    
    Bu proje Ayrıca, `ActorRuntime.RegisterActorAsync<T>()`kullanarak aktör sınıflarını Service Fabric çalışma zamanına kaydeden *program.cs*içerir. `HelloWorld` sınıfı zaten kayıtlı. Projeye eklenen diğer aktör uygulamalarının de `Main()` yönteminde kayıtlı olması gerekir.

## <a name="customize-the-helloworld-actor"></a>HelloWorld aktör 'i özelleştirme

Proje şablonu, `IHelloWorld` arabirimindeki bazı yöntemleri tanımlar ve bunları `HelloWorld` aktör uygulamasında uygular.  Aktör hizmetinin basit bir "Merhaba Dünya" dizesi döndürmesi için bu yöntemleri değiştirin.

*HelloWorld. Interfaces* projesinde, *IHelloWorld.cs* dosyasında, arabirim tanımını aşağıdaki gibi değiştirin:

```csharp
public interface IHelloWorld : IActor
{
    Task<string> GetHelloWorldAsync();
}
```

**HelloWorld** projesinde, **HelloWorld.cs**içinde, tüm sınıf tanımını aşağıdaki gibi değiştirin:

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

Projeyi derlemek için **Ctrl-Shift-B** tuşlarına basın ve her şeyin derlendiğinden emin olun.

## <a name="add-a-client"></a>İstemci Ekle

Aktör hizmetini çağırmak için basit bir konsol uygulaması oluşturun.

1. Çözüm Gezgini > > **Yeni Proje Ekle...** öğesine sağ tıklayın.

2. **.NET Core** proje türleri altında **konsol uygulaması (.NET Core)** öğesini seçin.  Projeyi *Actorclient*olarak adlandırın.
    
    ![Yeni Proje Ekle iletişim kutusu][6]    
    
    > [!NOTE]
    > Konsol uygulaması, genellikle Service Fabric istemci olarak kullandığınız uygulama türü değildir, ancak yerel Service Fabric kümesini kullanarak hata ayıklama ve test etme için kullanışlı bir örnek oluşturur.

3. Konsol uygulaması, arabirim projesiyle ve diğer bağımlılıklarla uyumluluğu sürdürmek için 64 bitlik bir uygulama olmalıdır.  Çözüm Gezgini, **Actorclient** projesine sağ tıklayın ve ardından **Özellikler**' e tıklayın.  **Derleme** sekmesinde **Platform hedefini** **x64**olarak ayarlayın.
    
    ![Derleme özellikleri][8]

4. İstemci projesi, güvenilir aktör NuGet paketini gerektirir.  **Araçlar** > **NuGet Paket Yöneticisi** > **Paket Yöneticisi Konsolu**’na tıklayın.  Paket Yöneticisi konsolunda aşağıdaki komutu girin:
    
    ```powershell
    Install-Package Microsoft.ServiceFabric.Actors -IncludePrerelease -ProjectName ActorClient
    ```

    NuGet paketi ve tüm bağımlılıkları ActorClient projesine yüklenir.

5. İstemci projesi Ayrıca, arabirimler projesine bir başvuru gerektirir.  ActorClient projesinde **Bağımlılıklar** ' a sağ tıklayın ve ardından **Başvuru Ekle...** öğesine tıklayın.  **Projeler > çözüm** ' ü seçin (zaten seçili değilse) ve ardından **HelloWorld. Interfaces**' in yanındaki onay kutusunu işaretleyin.  **Tamam**’a tıklayın.
    
    ![Başvuru Ekle iletişim kutusu][7]

6. ActorClient projesinde, *program.cs* öğesinin tüm içeriğini aşağıdaki kodla değiştirin:
    
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

## <a name="running-and-debugging"></a>Çalıştırma ve hata ayıklama

Uygulamayı Service Fabric geliştirme kümesinde yerel olarak derlemek, dağıtmak ve çalıştırmak için **F5** tuşuna basın.  Dağıtım işlemi sırasında, ilerleme durumunu **Çıkış** penceresinde görebilirsiniz.

![Hata ayıklama çıkışı penceresi Service Fabric][3]

Çıktı metin içerdiğinde, *uygulama hazırsa*, ActorClient uygulamasını kullanarak hizmeti test etmek mümkündür.  Çözüm Gezgini, **Actorclient** projesine sağ tıklayın ve ardından **yeni örnek Başlat** > **Hata Ayıkla** ' ya tıklayın.  Komut satırı uygulaması, aktör hizmetindeki çıktıyı görüntülemelidir.

![Uygulama çıkışı][9]

> [!TIP]
> Service Fabric aktör çalışma zamanı, [aktör yöntemleriyle ilgili bazı olayları ve performans sayaçlarını](service-fabric-reliable-actors-diagnostics.md#actor-method-events-and-performance-counters)yayar. Tanılama ve performans izleme için faydalıdır.

## <a name="next-steps"></a>Sonraki adımlar
[Reliable Actors Service Fabric platformunu kullanma](service-fabric-reliable-actors-platform.md)hakkında daha fazla bilgi edinin.


[1]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-newproject.PNG
[2]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-projectstructure.PNG
[3]: ./media/service-fabric-reliable-actors-get-started/debugging-output.PNG
[4]: ./media/service-fabric-reliable-actors-get-started/vs-context-menu.png
[5]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-newproject1.PNG
[6]: ./media/service-fabric-reliable-actors-get-started/new-console-app.png
[7]: ./media/service-fabric-reliable-actors-get-started/add-reference.png
[8]: ./media/service-fabric-reliable-actors-get-started/build-props.png
[9]: ./media/service-fabric-reliable-actors-get-started/app-output.png