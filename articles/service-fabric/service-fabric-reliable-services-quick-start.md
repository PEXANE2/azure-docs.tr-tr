---
title: "C'de ilk Servis Kumaşı uygulamanızı oluşturun #"
description: Durum disları ve durum ları kullanım durumu yla bir Microsoft Azure Hizmet Kumaşı uygulaması oluşturmaya giriş.
ms.topic: conceptual
ms.date: 07/10/2019
ms.custom: sfrev
ms.openlocfilehash: 15dd9bf6ac19bdac7bc8b50fc70e0b3b0a4e9a83
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77083787"
---
# <a name="get-started-with-reliable-services"></a>Reliable Services özelliğini kullanmaya başlayın

> [!div class="op_single_selector"]
> * [Windows üzerinde C#](service-fabric-reliable-services-quick-start.md)
> * [Linux üzerinde Java](service-fabric-reliable-services-quick-start-java.md)

Azure Hizmet Kumaşı uygulaması, kodunuzu çalıştıran bir veya daha fazla hizmet içerir. Bu kılavuz, [Güvenilir Hizmetler](service-fabric-reliable-services-introduction.md)ile hem durumsuz hem de durumlu Hizmet Kumaşı uygulamalarının nasıl oluşturulabileceğinizi gösterir.  

## <a name="basic-concepts"></a>Temel kavramlar

Güvenilir Hizmetler'e başlamak için yalnızca birkaç temel kavramı anlamanız gerekir:

* **Hizmet türü**: Bu sizin hizmet uygulamanızdır. Yazdığınız sınıf tarafından tanımlanır `StatelessService` ve burada kullanılan diğer kod lar veya bağımlılıklar, bir ad ve sürüm numarası ile birlikte.
* **Adlandırılmış hizmet örneği**: Hizmetinizi çalıştırmak için, tıpkı bir sınıf türünün nesne örneklerini oluşturduğunuz gibi hizmet türünün adlandırılmış örneklerini oluşturursunuz. Bir hizmet örneğinin "kumaş:/" kullanarak URI şeklinde bir adı vardır. "kumaş:/MyApp/MyService" gibi şema.
* **Hizmet ana bilgisayarı**: Oluşturduğunuz adlandırılmış hizmet örneklerinin ana bilgisayar işlemi içinde çalışması gerekir. Hizmet ana bilgisayar, hizmet inizin örneklerinin çalıştırılabildiği bir işlemdir.
* **Hizmet kaydı**: Kayıt her şeyi bir araya getirir. Hizmet dokusunun çalışmasına izin vermek için hizmet türünün servis ana bilgisayarda Hizmet Kumaşı'na kaydedilmesi gerekir.  

## <a name="create-a-stateless-service"></a>Devletsiz hizmet oluşturma

Devletsiz hizmet, bulut uygulamalarında şu anda norm olan bir hizmet türüdür. Hizmetin kendisi güvenilir bir şekilde depolanması veya yüksek kullanılabilir hale getirilmesi gereken veriler içermediğinden, durum dilemez olarak kabul edilir. Bir devletsiz hizmet örneği kapanırsa, tüm iç durumu kaybolur. Bu tür bir hizmette, durum, yüksek kullanılabilir ve güvenilir hale getirilebilmek için Azure Tabloları veya SQL veritabanı gibi harici bir depoda kalıcı olmalıdır.

Visual Studio 2017 veya Visual Studio 2019'u yönetici olarak başlatın ve *HelloWorld*adında yeni bir Service Fabric uygulama projesi oluşturun:

![Yeni Bir Hizmet Kumaşı uygulaması oluşturmak için Yeni Proje iletişim kutusunu kullanma](media/service-fabric-reliable-services-quick-start/hello-stateless-NewProject.png)

Sonra **.NET Core 2.0** *HelloWorldStateless*adlı kullanarak bir devletsiz hizmet projesi oluşturun:

![İkinci iletişim kutusunda, bir devletsiz hizmet projesi oluşturma](media/service-fabric-reliable-services-quick-start/hello-stateless-NewProject2.png)

Çözümünüz artık iki proje içeriyor:

* *HelloWorld*. Bu, *hizmetlerinizi*içeren *uygulama* projesidir. Ayrıca, uygulamayı açıklayan uygulama bildiriminin yanı sıra uygulamanızı dağıtmanıza yardımcı olan bir dizi PowerShell komut dosyasını da içerir.
* *HelloWorldStateLess*. Bu hizmet projesi. Bu devletsiz hizmet uygulamasını içerir.

## <a name="implement-the-service"></a>Hizmeti uygulayın

Hizmet projesinde **HelloWorldStateless.cs** dosyasını açın. Service Fabric'te bir hizmet herhangi bir iş mantığını çalıştırabilir. Hizmet API'si kodunuz için iki giriş noktası sağlar:

* Uzun süren işlem iş yükleri de dahil olmak üzere tüm iş yüklerini yürütmeye başlayabileceğiniz *RunAsync*adlı açık uçlu giriş noktası yöntemi.

```csharp
protected override async Task RunAsync(CancellationToken cancellationToken)
{
    ...
}
```

* ASP.NET Core gibi iletişim tercihinizi takabileceğiniz bir iletişim giriş noktası. Bu, kullanıcılardan ve diğer hizmetlerden istek almaya başlabildiğiniz yerdir.

```csharp
protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
{
    ...
}
```

Bu eğitimde, `RunAsync()` giriş noktası yöntemi üzerinde durulacak. Bu, kodunuzu hemen çalıştırmaya başlabildiğiniz yerdir.
Proje şablonu, yuvarlanma sayısı artışlarının `RunAsync()` örnek bir uygulamasını içerir.

> [!NOTE]
> İletişim yığınıyla nasıl çalışılabilenlerle ilgili ayrıntılar için [ASP.NET Core ile Hizmet iletişimine](service-fabric-reliable-services-communication-aspnetcore.md) bakın

### <a name="runasync"></a>RunAsync

```csharp
protected override async Task RunAsync(CancellationToken cancellationToken)
{
    // TODO: Replace the following sample code with your own logic
    //       or remove this RunAsync override if it's not needed in your service.

    long iterations = 0;

    while (true)
    {
        cancellationToken.ThrowIfCancellationRequested();

        ServiceEventSource.Current.ServiceMessage(this.Context, "Working-{0}", ++iterations);

        await Task.Delay(TimeSpan.FromSeconds(1), cancellationToken);
    }
}
```

Platform, bir hizmet örneği yerleştirildiğinde ve yürütülmeye hazır olduğunda bu yöntemi çağırır. Devletsiz bir hizmet için bu, hizmet örneğinin ne zaman açıldığı anlamına gelir. Hizmet örneğinizin kapatılması gerektiğinde koordine etmek için bir iptal belirteci sağlanır. Hizmet Kumaşı'nda, bir hizmet örneğinin bu açma/kapama döngüsü, hizmetin bir bütün olarak kullanım ömrü boyunca birçok kez oluşabilir. Bu, aşağıdakiler de dahil olmak üzere çeşitli nedenlerle gerçekleşebilir:

* Sistem, kaynak dengeleme için hizmet örneklerinizi taşır.
* Kodunuzda hatalar oluşur.
* Uygulama veya sistem yükseltilir.
* Altta yatan donanım bir kesinti yle karşılar.

Bu orkestrasyon, hizmetinizi yüksek oranda kullanılabilir ve düzgün bir şekilde dengede tutmak için sistem tarafından yönetilir.

`RunAsync()`senkronize olarak engellememelidir. RunAsync uygulamanız bir Görevi döndürmeli veya çalışma süresinin devam etmesine izin vermek için uzun süren veya engelleme işlemlerinde beklemeli. Önceki örnekte `while(true)` döngüde not, görev döndüren `await Task.Delay()` bir kullanılır. İş yükünüz eşzamanlı olarak engellenmesi gerekiyorsa, uygulamanızda `Task.Run()` yeni `RunAsync` bir Görev zamanlamanız gerekir.

İş yükünüzün iptali, sağlanan iptal belirteci tarafından yönetilen bir işbirliği çabasıdır. Sistem, devam etmeden önce görevin bitmesini (başarılı bir şekilde tamamlayarak, iptal ederek veya hata yaparak) bekler. İptal jetonuna uymak, herhangi bir işi bitirmek `RunAsync()` ve sistem iptal talebinde de diğinde mümkün olan en kısa sürede çıkmak önemlidir.

Bu durumsuz hizmet örneğinde, sayım yerel bir değişkende depolanır. Ancak bu durum devletsiz bir hizmet olduğundan, depolanan değer yalnızca hizmet örneğinin geçerli yaşam döngüsü için vardır. Hizmet hareket ettiğinde veya yeniden başlatıldığında, değer kaybolur.

## <a name="create-a-stateful-service"></a>Devlet hizmeti oluşturma

Service Fabric, devlete ait yeni bir hizmet türünü sunar. Devlet hizmeti, hizmeti kullanan kodla birlikte bulunan, hizmetin kendi içinde durumu güvenilir bir şekilde koruyabilir. Durum, harici bir mağazaya durumu devam etmeye gerek kalmadan Service Fabric tarafından yüksek oranda kullanılabilir hale getirilir.

Hizmet hareket etse veya yeniden başlatıldığında bile, karşı değeri durum suzdan yüksek kullanılabilir ve kalıcı ya da kalıcı hale getirmek için, devlet hizmetine ihtiyacınız vardır.

Aynı *HelloWorld* uygulamasında, uygulama projesindeki Hizmetler referanslarını sağ tıklayarak ve **Ekle -> Yeni Hizmet Kumaş Hizmeti'ni**seçerek yeni bir hizmet ekleyebilirsiniz.

![Service Fabric uygulamanıza hizmet ekleme](media/service-fabric-reliable-services-quick-start/hello-stateful-NewService.png)

**.NET Core 2.0 -> Stateful Service'i** seçin ve adını *HelloWorldStateful*olarak adlandırın. **Tamam**'a tıklayın.

![Yeni Bir Hizmet Kumaşı devlet hizmeti oluşturmak için Yeni Proje iletişim kutusunu kullanma](media/service-fabric-reliable-services-quick-start/hello-stateful-NewProject.png)

Uygulamanız şimdi iki hizmet olmalıdır: devletsiz hizmet *HelloWorldStateless* ve devlet hizmeti *HelloWorldStateful*.

Devlet hizmeti, devletsiz bir hizmetle aynı giriş noktalarına sahiptir. Temel fark, durumu güvenilir bir şekilde depolayabilen bir *devlet sağlayıcısının* kullanılabilirliğidir. Service Fabric, Güvenilir Devlet Yöneticisi aracılığıyla çoğaltılan veri yapıları oluşturmanıza olanak tanıyan [Güvenilir Koleksiyonlar](service-fabric-reliable-services-reliable-collections.md)adlı bir devlet sağlayıcı uygulamasıyla birlikte gelir. Duruma durumuna geçen Güvenilir Hizmet varsayılan olarak bu durum sağlayıcısını kullanır.

*HelloWorldStateful'da*açık **HelloWorldStateful.cs,** aşağıdaki RunAsync yöntemini içerir:

```csharp
protected override async Task RunAsync(CancellationToken cancellationToken)
{
    // TODO: Replace the following sample code with your own logic
    //       or remove this RunAsync override if it's not needed in your service.

    var myDictionary = await this.StateManager.GetOrAddAsync<IReliableDictionary<string, long>>("myDictionary");

    while (true)
    {
        cancellationToken.ThrowIfCancellationRequested();

        using (var tx = this.StateManager.CreateTransaction())
        {
            var result = await myDictionary.TryGetValueAsync(tx, "Counter");

            ServiceEventSource.Current.ServiceMessage(this.Context, "Current Counter Value: {0}",
                result.HasValue ? result.Value.ToString() : "Value does not exist.");

            await myDictionary.AddOrUpdateAsync(tx, "Counter", 0, (key, value) => ++value);

            // If an exception is thrown before calling CommitAsync, the transaction aborts, all changes are
            // discarded, and nothing is saved to the secondary replicas.
            await tx.CommitAsync();
        }

        await Task.Delay(TimeSpan.FromSeconds(1), cancellationToken);
    }
```

### <a name="runasync"></a>RunAsync

`RunAsync()`aynı şekilde devletli ve devletsiz hizmetlerde de çalışır. Ancak, devlet hizmetinde, platform yürütülmeden önce sizin `RunAsync()`adınıza ek iş gerçekleştirir. Bu çalışma, Güvenilir Devlet Yöneticisi ve Güvenilir Koleksiyonların kullanıma hazır olmasını sağlamayı içerebilir.

### <a name="reliable-collections-and-the-reliable-state-manager"></a>Güvenilir Koleksiyonlar ve Güvenilir Devlet Müdürü

```csharp
var myDictionary = await this.StateManager.GetOrAddAsync<IReliableDictionary<string, long>>("myDictionary");
```

[IReliableDictionary,](https://msdn.microsoft.com/library/dn971511.aspx) hizmetteki durumu güvenilir bir şekilde depolamak için kullanabileceğiniz bir sözlük uygulamasıdır. Servis Kumaşı ve Güvenilir Koleksiyonlar ile, harici kalıcı depoya gerek kalmadan verileri doğrudan hizmetinizde depolayabilirsiniz. Güvenilir Koleksiyonlar, verilerinizi son derece kullanılabilir hale getirin. Service Fabric bunu, hizmetinizin birden çok *kopyasını* sizin için oluşturarak ve yöneterek gerçekleştirir. Ayrıca, bu yinelemeleri ve durum geçişlerini yönetme karmaşıklığını özetleyen bir API sağlar.

Güvenilir Koleksiyonlar, özel türleriniz de dahil olmak üzere herhangi bir .NET türünü birkaç uyarıyla depolayabilir:

* Hizmet Kumaşı, düğümler arasında durumu *çoğaltarak* durumunuzu yüksek oranda kullanılabilir hale getirir ve Güvenilir Koleksiyonlar verilerinizi her yinelemede yerel diske depolar. Bu, Güvenilir Koleksiyonlarda depolanan her şeyin *serileştirilebilir*olması gerektiği anlamına gelir. Varsayılan olarak, Güvenilir Koleksiyonlar Serileştirme için [DataContract'ı](https://msdn.microsoft.com/library/system.runtime.serialization.datacontractattribute%28v=vs.110%29.aspx) kullanır, bu nedenle varsayılan serializer'ı kullandığınızda türünüzün [Veri Sözleşmesi Serializer tarafından desteklendiğinden](https://msdn.microsoft.com/library/ms731923%28v=vs.110%29.aspx) emin olmak önemlidir.
* Güvenilir Koleksiyonlar üzerinde işlem yaptığınızda nesneler yüksek kullanılabilirlik için çoğaltılır. Güvenilir Koleksiyonlarda depolanan nesneler hizmetinizde yerel bellekte tutulur. Bu, nesneye yerel bir başvurunuz olduğu anlamına gelir.
  
   Bir işlemdeki güvenilir koleksiyonda güncelleştirme işlemi gerçekleştirmeden bu nesnelerin yerel örneklerini mutasyona uğratmamanız önemlidir. Bunun nedeni, nesnelerin yerel örneklerindeki değişikliklerin otomatik olarak çoğaltılamamasıdır. Nesneyi sözlüğe yeniden eklemeniz veya sözlükteki *güncelleştirme* yöntemlerinden birini kullanmanız gerekir.

Güvenilir Devlet Yöneticisi sizin için Güvenilir Koleksiyonları yönetir. Güvenilir Devlet Müdürü'nden istediğiniz zaman ve hizmetinizde herhangi bir yerde ada göre güvenilir bir koleksiyon isteyebilirsiniz. Güvenilir Durum Yöneticisi, bir başvuruyu geri almanızı sağlar. Sınıf üye değişkenlerinde veya özelliklerinde güvenilir toplama örneklerine başvuru kaydetmenizi önermiyoruz. Başvurunun hizmet yaşam döngüsünde her zaman bir örneğe ayarlandığından emin olmak için özel özen gerekir. Güvenilir Devlet Yöneticisi bu işi sizin için işler ve tekrar ziyaretler için optimize edicisidir.

### <a name="transactional-and-asynchronous-operations"></a>İşlemsel ve eşzamanlı işlemler

```csharp
using (ITransaction tx = this.StateManager.CreateTransaction())
{
    var result = await myDictionary.TryGetValueAsync(tx, "Counter-1");

    await myDictionary.AddOrUpdateAsync(tx, "Counter-1", 0, (k, v) => ++v);

    await tx.CommitAsync();
}
```

Güvenilir Koleksiyonlar, Dil Tümleşik `System.Collections.Generic` `System.Collections.Concurrent` Sorgusu (LINQ) dışında, kendi ve benzerlerinin yaptığı işlemlerin çoğuna sahiptir. Güvenilir Koleksiyonlar üzerindeki işlemler eşzamanlıdır. Bunun nedeni, Güvenilir Koleksiyonlar ile yazma işlemleri çoğaltmak ve diske veri kalıcı gindirimi işlemleri gerçekleştirmek olmasıdır.

Güvenilir Tahsilat işlemleri *işlemseldir,* böylece birden çok Güvenilir Koleksiyon ve işlem de durumu tutarlı tutabilirsiniz. Örneğin, bir iş öğesini Güvenilir Kuyruktan ayırabilir, üzerinde bir işlem gerçekleştirebilir ve sonucu tek bir işlem içinde Güvenilir Sözlük'te kaydedebilirsiniz. Bu bir atomik işlem olarak kabul edilir ve ya tüm işlemin başarılı olacağını ya da tüm işlemin geri döneceğini garanti eder. Maddenin kuyruğunu çıkardıktan sonra ancak sonucu kaydetmeden önce bir hata oluşursa, tüm işlem geri alınır ve öğe işleme için sırada kalır.

## <a name="run-the-application"></a>Uygulamayı çalıştırma
Şimdi *HelloWorld* uygulamasına geri dönüyoruz. Artık hizmetlerinizi oluşturabilir ve dağıtabilirsiniz. **F5**tuşuna bastığınızda, uygulamanız yerel kümenize oluşturulur ve dağıtılır.

Hizmetler çalışmaya başladıktan sonra, oluşturulan Windows için Olay İzleme (ETW) olaylarını **tanılama olayları** penceresinde görüntüleyebilirsiniz. Görüntülenen olayların hem devletsiz hizmetten hem de uygulamadaki stateful hizmetinden olduğunu unutmayın. **Duraklat** düğmesini tıklatarak akışı duraklatabilirsiniz. Daha sonra bu iletiyi genişleterek iletinin ayrıntılarını inceleyebilirsiniz.

> [!NOTE]
> Uygulamayı çalıştırmadan önce, çalışan yerel bir geliştirme kümesi olduğundan emin olun. Yerel ortamınızı ayarlama hakkında bilgi almak için [başlangıç kılavuzuna](service-fabric-get-started.md) göz atın.
> 
> 

![Visual Studio'da Tanılama Olaylarını Görüntüle](media/service-fabric-reliable-services-quick-start/hello-stateful-Output.png)

## <a name="next-steps"></a>Sonraki adımlar
[Visual Studio'da Servis Kumaşı uygulamanızı hata ayıklama](service-fabric-debugging-your-application.md)

[Başlayın: OWIN self-hosting ile Servis Kumaş Web API hizmetleri](service-fabric-reliable-services-communication-webapi.md)

[Güvenilir Koleksiyonlar hakkında daha fazla bilgi edinin](service-fabric-reliable-services-reliable-collections.md)

[Uygulama dağıtma](service-fabric-deploy-remove-applications.md)

[Uygulama yükseltme](service-fabric-application-upgrade.md)

[Güvenilir Hizmetler için geliştirici başvurusu](https://msdn.microsoft.com/library/azure/dn706529.aspx)

