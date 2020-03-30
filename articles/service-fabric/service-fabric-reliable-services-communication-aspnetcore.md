---
title: ASP.NET Core ile hizmet iletişimi
description: ASP.NET Core'u, durum ifadesüz ve özel Azure Hizmeti Kumaş ı Güvenilir Hizmetler uygulamalarında nasıl kullanacağınızı öğrenin.
author: vturecek
ms.topic: conceptual
ms.date: 10/12/2018
ms.author: vturecek
ms.openlocfilehash: 0d432bd19d0689ef508fca0bf24eed4406929f82
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75639641"
---
# <a name="aspnet-core-in-azure-service-fabric-reliable-services"></a>Azure Hizmet Kumaş ı Güvenilir Hizmetlerde ASP.NET Çekirdek

ASP.NET Core açık kaynak ve çapraz platform çerçevesidir. Bu çerçeve, web uygulamaları, IoT uygulamaları ve mobil arka uçlar gibi bulut tabanlı, Internet'e bağlı uygulamalar oluşturmak için tasarlanmıştır.

Bu makale, **Microsoft.ServiceFabric.AspNetCore** kullanarak Hizmet Kumaş Güvenilir Hizmetler ASP.NET Çekirdek hizmetleri barındırma için ayrıntılı bir kılavuzdur. NuGet paketleri kümesi.

ASP.NET Core in Service Fabric ve geliştirme ortamınızı ayarlama yönergeleri hakkında bir giriş öğreticisi için [Bkz. Öğretici: ASP.NET Core Web API ön uç hizmeti ve devletsel bir arka uç hizmeti yle bir uygulama oluşturun ve dağıtın.](service-fabric-tutorial-create-dotnet-app.md)

Bu makalenin geri kalanı zaten ASP.NET Core aşina olduğunuzu varsayar. Değilse, ASP.NET Temel [leri](https://docs.microsoft.com/aspnet/core/fundamentals/index)okuyun .

## <a name="aspnet-core-in-the-service-fabric-environment"></a>Hizmet Kumaşı Ortamında ASP.NET Çekirdek

Hem ASP.NET Core hem de Service Fabric uygulamaları .NET Core veya full .NET Framework üzerinde çalıştırılabilir. Hizmet Kumaşı'nda ASP.NET Core'u iki farklı şekilde kullanabilirsiniz:
 - **Bir konuk yürütülebilir olarak barındırılan**. Bu şekilde, öncelikle hizmet kumaşı üzerinde varolan ASP.NET Çekirdek uygulamalarını kod değişikliği olmadan çalıştırmak için kullanılır.
 - **Güvenilir bir hizmet içinde çalıştırın.** Bu şekilde Service Fabric çalışma süresi ile daha iyi entegrasyon sağlar ve stateful ASP.NET Core hizmetleri sağlar.

Bu makalenin geri kalanı, Service Fabric SDK ile birlikte sevk edilen ASP.NET Core tümleştirme bileşenleri aracılığıyla, güvenilir bir hizmet içinde ASP.NET Core'un nasıl kullanılacağını açıklar.

## <a name="service-fabric-service-hosting"></a>Hizmet Kumaş servis hosting

Service Fabric'te, hizmetinizin bir veya daha fazla örneği ve/veya yinelemesi bir *hizmet ana bilgisayar işleminde*çalışır: hizmet kodunuzu çalıştıran yürütülebilir bir dosya. Bir hizmet yazarı olarak, hizmet ana bilgisayar işlemine sahipsiniz ve Service Fabric bunu sizin için etkinleştirir ve izler.

Geleneksel ASP.NET (MVC 5'e kadar) System.Web.dll aracılığıyla IIS ile sıkıca birleştirilmiştir. ASP.NET Core web sunucusu ve web uygulama arasında bir ayrım sağlar. Bu ayrım, web uygulamalarının farklı web sunucuları arasında taşınabilir olmasını sağlar. Ayrıca web sunucuları *kendi kendine barındırılan*olmasını sağlar. Bu, IIS gibi özel web sunucusu yazılımlarına ait bir işlemin aksine, kendi sürecinizde bir web sunucusu başlatabileceğiniz anlamına gelir.

Bir Service Fabric hizmetini birleştirmek ve ASP.NET, ya bir konuk olarak yürütülebilir olarak ya da güvenilir bir hizmette, hizmet ana bilgisayar sürecinizde ASP.NET başlatabilmelisiniz. ASP.NET Core kendi kendine barındırma bunu yapmanızı sağlar.

## <a name="hosting-aspnet-core-in-a-reliable-service"></a>ASP.NET Core'u güvenilir bir hizmette barındırma
Genellikle, kendi kendine barındırılan ASP.NET Core uygulamaları, uygulamanın giriş noktasında `static void Main()` ki `Program.cs`yöntem gibi bir WebHost oluşturur. Bu durumda, WebHost yaşam döngüsü sürecin yaşam döngüsüne bağlıdır.

![Bir süreçte ASP.NET Core barındırma][0]

Ancak uygulama giriş noktası, güvenilir bir hizmette WebHost oluşturmak için doğru yer değildir. Bunun nedeni, uygulama giriş noktasının yalnızca hizmet dokusu çalışma süresine sahip bir hizmet türünü kaydetmek için kullanılmasıdır, böylece bu hizmet türünden örnekler oluşturabilir. WebHost güvenilir bir hizmet kendisi oluşturulmalıdır. Hizmet ana bilgisayar işlemi içinde, hizmet örnekleri ve/veya yinelemeler birden çok yaşam döngüsünden geçebilir. 

Güvenilir Hizmet örneği, hizmet sınıfınızdan kaynaklanan `StatelessService` veya `StatefulService`. Bir hizmetin iletişim yığını, `ICommunicationListener` hizmet sınıfınızdaki bir uygulamada bulunur. `Microsoft.ServiceFabric.AspNetCore.*` NuGet `ICommunicationListener` paketleri, bu başlangıç uygulamalarını içerir ve güvenilir bir hizmette Kestrel veya HTTP.sys için ASP.NET Core WebHost'u yönetir.

![ASP.NET Core'u güvenilir bir hizmette barındırmak için diyagram][1]

## <a name="aspnet-core-icommunicationlisteners"></a>ASP.NET Çekirdek ICommunicationListeners
`ICommunicationListener` NuGet paketlerindeki Kestrel ve HTTP.sys uygulamaları benzer kullanım modellerine `Microsoft.ServiceFabric.AspNetCore.*` sahiptir. Ancak her web sunucusuna özgü biraz farklı eylemler gerçekleştirirler. 

Her iki iletişim dinleyicisi aşağıdaki bağımsız değişkenleri alan bir oluşturucu sağlar:
 - **`ServiceContext serviceContext`**: Bu, `ServiceContext` çalışan hizmet hakkında bilgi içeren nesnedir.
 - **`string endpointName`**: ServiceManifest.xml'deki bir `Endpoint` yapılandırmanın adıdır. Öncelikle iki iletişim dinleyicisinin farklı olduğu yer. HTTP.sys bir `Endpoint` yapılandırma *gerektirir,* Kestrel ise gerektirmez.
 - **`Func<string, AspNetCoreCommunicationListener, IWebHost> build`**: Bu, sizin uyguladığınız bir lambdadır, `IWebHost`içinde bir . Bir ASP.NET Core `IWebHost` uygulamasında normalde yaptığınız şekilde yapılandırmanızı sağlar. Lambda, kullandığınız Service Fabric tümleştirme seçeneklerine ve sağladığınız yapılandırmaya `Endpoint` bağlı olarak sizin için oluşturulan bir URL sağlar. Daha sonra web sunucusunu başlatmak için bu URL'yi değiştirebilir veya kullanabilirsiniz.

## <a name="service-fabric-integration-middleware"></a>Servis Kumaş entegrasyonu middleware
`Microsoft.ServiceFabric.AspNetCore` NuGet paketi, `UseServiceFabricIntegration` Service Fabric-aware ara yazılımekler üzerinde `IWebHostBuilder` uzatma yöntemi içerir. Bu ara yazılım, Kestrel veya HTTP.sys'yi `ICommunicationListener` Hizmet Kumaşı Adlandırma Hizmeti'ne benzersiz bir hizmet URL'si kaydetmek için yapılandırır. Daha sonra istemci isteklerini doğru hizmete bağlandığından emin olmak için doğrular. 

Bu adım, istemcilerin yanlışlıkla yanlış hizmete bağlanmasını önlemek için gereklidir. Bunun nedeni, Service Fabric gibi ortak barındırış ortamında, birden çok web uygulamasının aynı fiziksel veya sanal makinede çalıştırılabiliyor, ancak benzersiz ana bilgisayar adları kullanmayabilir. Bu senaryo sonraki bölümde daha ayrıntılı olarak açıklanmıştır.

### <a name="a-case-of-mistaken-identity"></a>Yanlış kimlik durumu
Hizmet yinelemeleri, protokolden bağımsız olarak benzersiz bir IP:port kombinasyonunu dinler. Bir hizmet yinelemesi IP:port bitiş noktasında dinlemeye başladıktan sonra, bu uç nokta adresini Hizmet Kumaşı Adlandırma Hizmeti'ne bildirir. Orada, istemciler veya diğer hizmetler bunu keşfedebilir. Hizmetler dinamik olarak atanmış uygulama bağlantı noktalarını kullanıyorsa, bir hizmet yinelemesi tesadüfen aynı fiziksel veya sanal makinede daha önce başka bir hizmetin aynı IP:port bitiş noktasını kullanabilir. Bu, istemcinin yanlışlıkla yanlış hizmete bağlanmasına neden olabilir. Aşağıdaki olaylar dizisi oluşursa, bu senaryo ortaya çıkabilir:

 1. Hizmet A http üzerinden 10.0.0.1:30000 üzerinde dinler. 
 2. İstemci Hizmet A'yı çözer ve 10.0.0.1:30000 adresini alır.
 3. Hizmet A farklı bir düğüme taşınır.
 4. Hizmet B 10.0.0.1 yerleştirilir ve tesadüfen aynı bağlantı noktası 30000 kullanır.
 5. İstemci, önbelleğe alınmış adres10.0.0.1:30000 olan Hizmet A'ya bağlanmayı dener.
 6. İstemci artık b hizmetine başarıyla bağdaştırılır ve yanlış hizmete bağlı olduğunu fark etmez.

Bu tanılamak zor olabilir rasgele zamanlarda hataları neden olabilir.

### <a name="using-unique-service-urls"></a>Benzersiz servis URL'lerini kullanma
Bu hataları önlemek için, hizmetler özgün bir tanımlayıcıyla Adlandırma Hizmeti'ne bir bitiş noktası gönderebilir ve istemci istekleri sırasında bu benzersiz tanımlayıcıyı doğrulayabilir. Bu, düşman-kiracı olmayan güvenilir bir ortamda hizmetler arasında işbirliğine dayalı bir eylemdir. Düşman-kiracı ortamında güvenli hizmet kimlik doğrulaması sağlamaz.

Güvenilir bir ortamda, `UseServiceFabricIntegration` yöntem tarafından eklenen ara yazılım, Adlandırma Hizmeti'ne gönderilen adrese otomatik olarak benzersiz bir tanımlayıcı ekler. Her istekte bu tanımlayıcıyı doğrular. Tanımlayıcı eşleşmiyorsa, ara yazılım hemen bir HTTP 410 Gone yanıtını döndürür.

Dinamik olarak atanmış bir bağlantı noktası kullanan hizmetler bu ara yazılımdan yararlanmalıdır.

Sabit benzersiz bağlantı noktası kullanan hizmetlerin işbirliği ortamında bu sorunu yoktur. Sabit benzersiz bağlantı noktası genellikle bağlanmak için istemci uygulamaları için iyi bilinen bir bağlantı noktası gerektiren harici bakan hizmetler için kullanılır. Örneğin, internete bakan web uygulamalarının çoğu web tarayıcısı bağlantıları için 80 veya 443 bağlantı noktasını kullanır. Bu durumda, benzersiz tanımlayıcı etkinleştirilmemelidir.

Aşağıdaki diyagram, ara yazılım etkinleştirilmiş istek akışını gösterir:

![Hizmet Kumaş ASP.NET Core entegrasyonu][2]

Hem Kerkenez hem de `ICommunicationListener` HTTP.sys uygulamaları bu mekanizmayı tam olarak aynı şekilde kullanır. HTTP.sys, temel **http.sys** bağlantı noktası paylaşımı özelliğini kullanarak istekleri benzersiz URL yollarına göre dahili olarak `ICommunicationListener` ayırt edebilse de, bu işlevsellik HTTP.sys uygulaması tarafından *kullanılmaz.* Bunun nedeni, daha önce açıklanan senaryoda HTTP 503 ve HTTP 404 hata durum kodlarıile sonuçlanmalarıdır. Bu da, HTTP 503 ve HTTP 404'ün diğer hataları belirtmek için yaygın olarak kullanıldığından, istemcilerin hatanın amacını belirlemesini zorlaştırır. 

Böylece, hem Kestrel hem de `ICommunicationListener` HTTP.sys `UseServiceFabricIntegration` uygulamaları, uzantı yöntemi tarafından sağlanan ara yazılımlarda standartlaşır. Bu nedenle, istemcilerin yalnızca HTTP 410 yanıtlarında bir hizmet bitiş noktası yeniden çözümle eylemi gerçekleştirmesi gerekir.

## <a name="httpsys-in-reliable-services"></a>Güvenilir Hizmetlerde HTTP.sys
Http.sys'yi Güvenilir Hizmetler'de **Microsoft.ServiceFabric.AspNetCore.HttpSys** NuGet paketini içe aktararak kullanabilirsiniz. Bu paket `HttpSysCommunicationListener`, bir `ICommunicationListener`uygulama içerir. `HttpSysCommunicationListener`web sunucusu olarak HTTP.sys kullanarak güvenilir bir hizmet içinde ASP.NET Core WebHost oluşturmanıza olanak sağlar.

HTTP.sys [Windows HTTP Server API](https://msdn.microsoft.com/library/windows/desktop/aa364510(v=vs.85).aspx)üzerine inşa edilmiştir. Bu API, HTTP isteklerini işlemek ve bunları web uygulamalarını çalıştıran işlemlere yönlendirmek için **HTTP.sys** çekirdek sürücüsünü kullanır. Bu, aynı fiziksel veya sanal makinedeki birden çok işlemin, benzersiz bir URL yolu veya ana bilgisayar adı tarafından farklı olarak aynı bağlantı noktasında web uygulamalarını barındırmasına olanak tanır. Bu özellikler, aynı kümede birden çok web sitesini barındırmak için Service Fabric'te kullanışlıdır.

>[!NOTE]
>HTTP.sys uygulaması yalnızca Windows platformunda çalışır.

Aşağıdaki diyagram, HTTP.sys'nin bağlantı noktası paylaşımı için Windows'ta **HTTP.sys** çekirdek sürücüsünü nasıl kullandığını göstermektedir:

![HTTP.sys diyagramı][3]

### <a name="httpsys-in-a-stateless-service"></a>HTTP.sys bir devletsiz hizmet
Devletsiz `HttpSys` bir hizmette kullanmak için `CreateServiceInstanceListeners` yöntemi geçersiz `HttpSysCommunicationListener` kılın ve bir örneği döndürün:

```csharp
protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
{
    return new ServiceInstanceListener[]
    {
        new ServiceInstanceListener(serviceContext =>
            new HttpSysCommunicationListener(serviceContext, "ServiceEndpoint", (url, listener) =>
                new WebHostBuilder()
                    .UseHttpSys()
                    .ConfigureServices(
                        services => services
                            .AddSingleton<StatelessServiceContext>(serviceContext))
                    .UseContentRoot(Directory.GetCurrentDirectory())
                    .UseServiceFabricIntegration(listener, ServiceFabricIntegrationOptions.None)
                    .UseStartup<Startup>()
                    .UseUrls(url)
                    .Build()))
    };
}
```

### <a name="httpsys-in-a-stateful-service"></a>HTTP.sys bir devlet hizmeti

`HttpSysCommunicationListener`şu anda altta yatan **HTTP.sys** bağlantı noktası paylaşımı özelliği ile ilgili komplikasyonlar nedeniyle devlet hizmetlerinde kullanılmak üzere tasarlanmamaktadır. Daha fazla bilgi için HTTP.sys ile dinamik bağlantı noktası tahsisi ile ilgili aşağıdaki bölüme bakın. Devlet hizmetleri için, Kestrel önerilen web sunucusudur.

### <a name="endpoint-configuration"></a>Bitiş noktası yapılandırması

HTTP.sys de dahil olmak üzere Windows HTTP Server API'sını kullanan web sunucuları için `Endpoint` yapılandırma gereklidir. Windows HTTP Server API'yi kullanan Web sunucuları öncelikle URL'lerini HTTP.sys ile rezerve etmelidir (bu normalde [netsh](https://msdn.microsoft.com/library/windows/desktop/cc307236(v=vs.85).aspx) aracıyla gerçekleştirilir). 

Bu eylem, hizmetlerinizin varsayılan olarak sahip olmadığı yüksek ayrıcalıklar gerektirir. ServiceManifest.xml'deki `Endpoint` yapılandırmanın `Protocol` özelliği için "http" veya "https" seçenekleri, Service Fabric çalışma zamanını sizin adınıza HTTP.sys'ye kaydettirmeleri için özellikle kullanmak için kullanılır. Bunu güçlü joker [*karakter*](https://msdn.microsoft.com/library/windows/desktop/aa364698(v=vs.85).aspx) URL önekini kullanarak yapar.

Örneğin, bir `http://+:80` hizmet için rezervasyon yapmak için ServiceManifest.xml'de aşağıdaki yapılandırmayı kullanın:

```xml
<ServiceManifest ... >
    ...
    <Resources>
        <Endpoints>
            <Endpoint Name="ServiceEndpoint" Protocol="http" Port="80" />
        </Endpoints>
    </Resources>

</ServiceManifest>
```

Ve bitiş noktası adı `HttpSysCommunicationListener` oluşturucuya geçirilmelidir:

```csharp
 new HttpSysCommunicationListener(serviceContext, "ServiceEndpoint", (url, listener) =>
 {
     return new WebHostBuilder()
         .UseHttpSys()
         .UseServiceFabricIntegration(listener, ServiceFabricIntegrationOptions.None)
         .UseUrls(url)
         .Build();
 })
```

#### <a name="use-httpsys-with-a-static-port"></a>Statik bağlantı noktası ile HTTP.sys kullanın
HTTP.sys ile statik bağlantı noktası kullanmak için `Endpoint` yapılandırmada bağlantı noktası numarasını sağlayın:

```xml
  <Resources>
    <Endpoints>
      <Endpoint Protocol="http" Name="ServiceEndpoint" Port="80" />
    </Endpoints>
  </Resources>
```

#### <a name="use-httpsys-with-a-dynamic-port"></a>Dinamik bir bağlantı noktası ile HTTP.sys kullanın
HTTP.sys ile dinamik olarak atanmış bir bağlantı `Port` noktası kullanmak `Endpoint` için yapılandırmadaki özelliği atla:

```xml
  <Resources>
    <Endpoints>
      <Endpoint Protocol="http" Name="ServiceEndpoint" />
    </Endpoints>
  </Resources>
```

Yapılandırma tarafından ayrılan dinamik `Endpoint` bağlantı *noktası, ana bilgisayar işlemi başına*yalnızca bir bağlantı noktası sağlar. Geçerli Service Fabric barındırma modeli, aynı işlemde birden çok hizmet örneğinin ve/veya yinelemenin barındırılmasına olanak tanır. Bu, `Endpoint` yapılandırma aracılığıyla tahsis edildiğinde her birinin aynı bağlantı noktasını paylaşacağı anlamına gelir. Birden çok **HTTP.sys** örneği, temel **http.sys** bağlantı noktası paylaşımı özelliğini kullanarak bir bağlantı noktasını paylaşabilir. Ama müşteri istekleri için `HttpSysCommunicationListener` tanıttı komplikasyonlar nedeniyle desteklenmez. Dinamik bağlantı noktası kullanımı için Kerkenez önerilen web sunucusudur.

## <a name="kestrel-in-reliable-services"></a>Güvenilir Hizmetlerde Kerkenez
**Microsoft.ServiceFabric.AspNetCore.Kestrel** NuGet paketini içe aktararak Güvenilir Hizmetlerde Kestrel'i kullanabilirsiniz. Bu paket `KestrelCommunicationListener`, bir `ICommunicationListener`uygulama içerir. `KestrelCommunicationListener`web sunucusu olarak Kestrel kullanarak güvenilir bir hizmet içinde ASP.NET Core WebHost oluşturmanıza olanak sağlar.

Kerkenez, ASP.NET Core için bir çapraz platform web sunucusudur. HTTP.sys'nin aksine, Kestrel merkezi bir uç nokta yöneticisi kullanmaz. Ayrıca HTTP.sys aksine, Kestrel birden fazla işlem arasında bağlantı noktası paylaşımı desteklemez. Kerkenez'in her örneği benzersiz bir bağlantı noktası kullanmalıdır. Kerkenez hakkında daha fazla bilgi için [Uygulama Ayrıntıları'na](https://docs.microsoft.com/aspnet/core/fundamentals/servers/kestrel?view=aspnetcore-2.2)bakın.

![Kerkenez diyagramı][4]

### <a name="kestrel-in-a-stateless-service"></a>Kerkenez bir devletsiz hizmet
Devletsiz `Kestrel` bir hizmette kullanmak için `CreateServiceInstanceListeners` yöntemi geçersiz `KestrelCommunicationListener` kılın ve bir örneği döndürün:

```csharp
protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
{
    return new ServiceInstanceListener[]
    {
        new ServiceInstanceListener(serviceContext =>
            new KestrelCommunicationListener(serviceContext, "ServiceEndpoint", (url, listener) =>
                new WebHostBuilder()
                    .UseKestrel()
                    .ConfigureServices(
                        services => services
                            .AddSingleton<StatelessServiceContext>(serviceContext))
                    .UseContentRoot(Directory.GetCurrentDirectory())
                    .UseServiceFabricIntegration(listener, ServiceFabricIntegrationOptions.UseUniqueServiceUrl)
                    .UseStartup<Startup>()
                    .UseUrls(url)
                    .Build();
            ))
    };
}
```

### <a name="kestrel-in-a-stateful-service"></a>Kerkenez devlet hizmetinde
Devlet `Kestrel` hizmetinde kullanmak için `CreateServiceReplicaListeners` yöntemi geçersiz kılın `KestrelCommunicationListener` ve bir örneği döndürün:

```csharp
protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
{
    return new ServiceReplicaListener[]
    {
        new ServiceReplicaListener(serviceContext =>
            new KestrelCommunicationListener(serviceContext, (url, listener) =>
                new WebHostBuilder()
                    .UseKestrel()
                    .ConfigureServices(
                         services => services
                             .AddSingleton<StatefulServiceContext>(serviceContext)
                             .AddSingleton<IReliableStateManager>(this.StateManager))
                    .UseContentRoot(Directory.GetCurrentDirectory())
                    .UseServiceFabricIntegration(listener, ServiceFabricIntegrationOptions.UseUniqueServiceUrl)
                    .UseStartup<Startup>()
                    .UseUrls(url)
                    .Build();
            ))
    };
}
```

Bu örnekte, WebHost bağımlılık `IReliableStateManager` enjeksiyon konteynerine tekton bir örnek sağlanır. Bu kesinlikle gerekli değildir, ancak MVC `IReliableStateManager` denetleyici eylem yöntemleri ve Güvenilir Koleksiyonları kullanmanıza olanak sağlar.

Bir `Endpoint` yapılandırma adı devlet `KestrelCommunicationListener` hizmetinde *sağlanmaz.* Bu, aşağıdaki bölümde daha ayrıntılı olarak açıklanmıştır.

### <a name="configure-kestrel-to-use-https"></a>Kestrel’i HTTPS kullanacak şekilde yapılandırma
Kerkenez hizmetinizdeyken HTTPS'yi etkinleştirirken birkaç dinleme seçeneği ayarlamanız gerekir. `ServiceInstanceListener` *EndpointHttps* bitiş noktası nı kullanmak ve belirli bir bağlantı noktasında (port 443 gibi) dinlemek için güncelleştirme. Kestrel web sunucusunu kullanacak şekilde web ana bilgisayarını yapılandırırken, tüm ağ arabirimlerinde IPv6 adreslerini dinleyecek şekilde Kerkenez'i yapılandırmanız gerekir: 

```csharp
new ServiceInstanceListener(
serviceContext =>
    new KestrelCommunicationListener(
        serviceContext,
        "EndpointHttps",
        (url, listener) =>
        {
            ServiceEventSource.Current.ServiceMessage(serviceContext, $"Starting Kestrel on {url}");

            return new WebHostBuilder()
                .UseKestrel(opt =>
                {
                    int port = serviceContext.CodePackageActivationContext.GetEndpoint("EndpointHttps").Port;
                    opt.Listen(IPAddress.IPv6Any, port, listenOptions =>
                    {
                        listenOptions.UseHttps(GetCertificateFromStore());
                        listenOptions.NoDelay = true;
                    });
                })
                .ConfigureAppConfiguration((builderContext, config) =>
                {
                    config.AddJsonFile("appsettings.json", optional: false, reloadOnChange: true);
                })

                .ConfigureServices(
                    services => services
                        .AddSingleton<HttpClient>(new HttpClient())
                        .AddSingleton<FabricClient>(new FabricClient())
                        .AddSingleton<StatelessServiceContext>(serviceContext))
                .UseContentRoot(Directory.GetCurrentDirectory())
                .UseStartup<Startup>()
                .UseServiceFabricIntegration(listener, ServiceFabricIntegrationOptions.None)
                .UseUrls(url)
                .Build();
        }))
```

Bir öğreticideki tam bir örnek için, [HTTPS'yi kullanmak için Kerkenez'i Yapılandır'a](service-fabric-tutorial-dotnet-app-enable-https-endpoint.md#configure-kestrel-to-use-https)bakın.


### <a name="endpoint-configuration"></a>Bitiş noktası yapılandırması
Kerkenez kullanmak için yapılandırma `Endpoint` gerekmez. 

Kerkenez basit bir bağımsız web sunucusudur. HTTP.sys (veya HttpListener) aksine, başlatmadan önce URL kaydı gerektirmez, çünkü ServiceManifest.xml bir `Endpoint` yapılandırma gerekmez. 

#### <a name="use-kestrel-with-a-static-port"></a>Statik bağlantı noktası ile Kerkenez kullanın
Kestrel ile kullanmak üzere `Endpoint` ServiceManifest.xml yapılandırmasında statik bir bağlantı noktası yapılandırabilirsiniz. Bu kesinlikle gerekli olmasa da, iki potansiyel faydalar sunar:
 - Bağlantı noktası uygulama bağlantı noktası aralığında düşmezse, Service Fabric tarafından os güvenlik duvarı üzerinden açılır.
 - Size `KestrelCommunicationListener` sağlanan URL bu bağlantı noktasını kullanır.

```xml
  <Resources>
    <Endpoints>
      <Endpoint Protocol="http" Name="ServiceEndpoint" Port="80" />
    </Endpoints>
  </Resources>
```

Bir `Endpoint` yapılandırılan varsa, adı `KestrelCommunicationListener` oluşturucuya geçirilmelidir: 

```csharp
new KestrelCommunicationListener(serviceContext, "ServiceEndpoint", (url, listener) => ...
```

ServiceManifest.xml yapılandırma `Endpoint` kullanmıyorsa, `KestrelCommunicationListener` oluşturucudaki adı atla. Bu durumda, dinamik bir bağlantı noktası kullanır. Bu konuda daha fazla bilgi için sonraki bölüme bakın.

#### <a name="use-kestrel-with-a-dynamic-port"></a>Dinamik bir bağlantı noktası ile Kerkenez kullanın
Kerkenez ServiceManifest.xml'deki yapılandırmadan otomatik bağlantı noktası atamasını `Endpoint` kullanamaz. Bunun nedeni, bir `Endpoint` yapılandırmadan otomatik bağlantı noktası *ataması ana bilgisayar işlemi*başına benzersiz bir bağlantı noktası atar ve tek bir ana bilgisayar işlemi birden çok Kestrel örneği içerebilir. Bu, bağlantı noktası paylaşımını desteklemediği için Kestrel ile çalışmaz. Bu nedenle, her Kerkenez örneği benzersiz bir bağlantı noktasında açılmalıdır.

Kestrel ile dinamik bağlantı noktası ataması `Endpoint` kullanmak için ServiceManifest.xml'deki yapılandırmayı tamamen atayın `KestrelCommunicationListener` ve kurucuya aşağıdaki gibi bir bitiş noktası adı geçmeyin:

```csharp
new KestrelCommunicationListener(serviceContext, (url, listener) => ...
```

Bu yapılandırmada, `KestrelCommunicationListener` uygulama bağlantı noktası aralığından kullanılmayan bir bağlantı noktası otomatik olarak seçilir.

HTTPS için, ServiceManifest.xml'de belirtilen bir bağlantı noktası olmadan BITIŞ Noktası'nı HTTPS protokolü ile yapılandırmalı ve bitiş noktası adını KestrelCommunicationListener constructor'a geçirmelidir.


## <a name="service-fabric-configuration-provider"></a>Service Fabric yapılandırma sağlayıcısı
ASP.NET Core'daki uygulama yapılandırması, yapılandırma sağlayıcısı tarafından oluşturulan anahtar değer çiftlerini temel adatır. Genel ASP.NET Core yapılandırma desteği hakkında daha fazla bilgi almak için [ASP.NET Core'da Configuration'ı](https://docs.microsoft.com/aspnet/core/fundamentals/configuration/) okuyun.

Bu bölümde, Service Fabric yapılandırma sağlayıcısının `Microsoft.ServiceFabric.AspNetCore.Configuration` NuGet paketini içe aktararak ASP.NET Core yapılandırmasıyla nasıl entegre olduğu açıklanmaktadır.

### <a name="addservicefabricconfiguration-startup-extensions"></a>AddServiceFabricConfiguration başlangıç uzantıları
NuGet paketini `Microsoft.ServiceFabric.AspNetCore.Configuration` aldıktan sonra, Service Fabric Configuration kaynağını ASP.NET Core yapılandırma API'sine kaydetmeniz gerekir. Bunu, ad alanındaki **AddServiceFabricConfiguration** uzantılarını `Microsoft.ServiceFabric.AspNetCore.Configuration` ' `IConfigurationBuilder`ya karşı denetleyerek yaparsınız.

```csharp
using Microsoft.ServiceFabric.AspNetCore.Configuration;

public Startup(IHostingEnvironment env)
{
    var builder = new ConfigurationBuilder()
        .SetBasePath(env.ContentRootPath)
        .AddJsonFile("appsettings.json", optional: false, reloadOnChange: true)
        .AddJsonFile($"appsettings.{env.EnvironmentName}.json", optional: true)
        .AddServiceFabricConfiguration() // Add Service Fabric configuration settings.
        .AddEnvironmentVariables();
    Configuration = builder.Build();
}

public IConfigurationRoot Configuration { get; }
```

Artık ASP.NET Core hizmeti, diğer uygulama ayarları gibi Service Fabric yapılandırma ayarlarına erişebilir. Örneğin, ayarları güçlü bir şekilde yazılan nesnelere yüklemek için seçenekler deseni kullanabilirsiniz.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.Configure<MyOptions>(Configuration);  // Strongly typed configuration object.
    services.AddMvc();
}
```
### <a name="default-key-mapping"></a>Varsayılan anahtar eşleme
Varsayılan olarak, Service Fabric yapılandırma sağlayıcısı paket adını, bölüm adını ve özellik adını içerir. Bunlar birlikte, aşağıdaki gibi ASP.NET Core yapılandırma anahtarını oluşturur:
```csharp
$"{this.PackageName}{ConfigurationPath.KeyDelimiter}{section.Name}{ConfigurationPath.KeyDelimiter}{property.Name}"
```

Örneğin, aşağıdaki içerikle birlikte `MyConfigPackage` bir yapılandırma paketiniz varsa, yapılandırma değeri `IConfiguration` *MyConfigPackage:MyConfigSection:MyParameter*aracılığıyla ASP.NET Core'da kullanılabilir.
```xml
<?xml version="1.0" encoding="utf-8" ?>
<Settings xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">  
  <Section Name="MyConfigSection">
    <Parameter Name="MyParameter" Value="Value1" />
  </Section>  
</Settings>
```
### <a name="service-fabric-configuration-options"></a>Servis Kumaş ı yapılandırma seçenekleri
Service Fabric yapılandırma sağlayıcısı, anahtar eşlemenin varsayılan davranışını değiştirmeyi de destekler. `ServiceFabricConfigurationOptions`

#### <a name="encrypted-settings"></a>Şifreli ayarlar
Service Fabric, Service Fabric yapılandırma sağlayıcısının yaptığı gibi şifreli ayarları destekler. Şifrelenmiş ayarlar varsayılan olarak ASP.NET Core'a `IConfiguration` ait şifresi çözülmez. Şifrelenmiş değerler bunun yerine orada depolanır. Ancak, ASP.NET Core IConfiguration'da depolayacağağımının deşifresini deşifre etmek isterseniz, *DecryptValue* bürücüsünü `AddServiceFabricConfiguration` uzantıda yanlışolarak düşürü

```csharp
public Startup()
{
    ICodePackageActivationContext activationContext = FabricRuntime.GetActivationContext();
    var builder = new ConfigurationBuilder()        
        .AddServiceFabricConfiguration(activationContext, (options) => options.DecryptValue = false); // set flag to decrypt the value
    Configuration = builder.Build();
}
```
#### <a name="multiple-configuration-packages"></a>Birden çok yapılandırma paketi
Service Fabric birden çok yapılandırma paketini destekler. Varsayılan olarak, paket adı yapılandırma anahtarına dahil edilir. Ancak `IncludePackageName` bayrağı aşağıdaki gibi yanlış olarak ayarlayabilirsiniz:
```csharp
public Startup()
{
    ICodePackageActivationContext activationContext = FabricRuntime.GetActivationContext();
    var builder = new ConfigurationBuilder()        
        // exclude package name from key.
        .AddServiceFabricConfiguration(activationContext, (options) => options.IncludePackageName = false); 
    Configuration = builder.Build();
}
```
#### <a name="custom-key-mapping-value-extraction-and-data-population"></a>Özel anahtar eşleme, değer çıkarma ve veri popülasyonu
Service Fabric yapılandırma sağlayıcısı, anahtar eşleciliğini özelleştirmek `ExtractKeyFunc` ve değerleri `ExtractValueFunc`özel olarak ayıklamak için daha gelişmiş senaryoları da destekler. Hatta tüm veri doldurma işlemini Service Fabric yapılandırmasından ASP.NET Core `ConfigAction`yapılandırmasına çevirebilirsiniz.

Aşağıdaki örnekler, veri `ConfigAction` popülasyonu özelleştirmek için nasıl kullanılacağını göstermektedir:
```csharp
public Startup()
{
    ICodePackageActivationContext activationContext = FabricRuntime.GetActivationContext();
    
    this.valueCount = 0;
    this.sectionCount = 0;
    var builder = new ConfigurationBuilder();
    builder.AddServiceFabricConfiguration(activationContext, (options) =>
        {
            options.ConfigAction = (package, configData) =>
            {
                ILogger logger = new ConsoleLogger("Test", null, false);
                logger.LogInformation($"Config Update for package {package.Path} started");

                foreach (var section in package.Settings.Sections)
                {
                    this.sectionCount++;

                    foreach (var param in section.Parameters)
                    {
                        configData[options.ExtractKeyFunc(section, param)] = options.ExtractValueFunc(section, param);
                        this.valueCount++;
                    }
                }

                logger.LogInformation($"Config Update for package {package.Path} finished");
            };
        });
  Configuration = builder.Build();
}
```

### <a name="configuration-updates"></a>Yapılandırma güncelleştirmeleri
Service Fabric yapılandırma sağlayıcısı yapılandırma güncelleştirmelerini de destekler. Değişiklik bildirimlerini `IOptionsMonitor` almak için ASP.NET Core'u `IOptionsSnapshot` ve ardından yapılandırma verilerini yeniden yüklemek için kullanabilirsiniz. Daha fazla bilgi için [ASP.NET Core seçeneklerine](https://docs.microsoft.com/aspnet/core/fundamentals/configuration/options)bakın.

Bu seçenekler varsayılan olarak desteklenir. Yapılandırma güncelleştirmelerini etkinleştirmek için başka kodlama gerekmez.

## <a name="scenarios-and-configurations"></a>Senaryolar ve yapılandırmalar
Bu bölümde, aşağıdaki senaryoları gidermek için öneririz web sunucusu, bağlantı noktası yapılandırması, Hizmet Kumaş tümleştirme seçenekleri ve çeşitli ayarların birleşimini sağlar:
 - Core ASP.NET stateless hizmetleri harici maruz
 - İçASP.NET Core vatansız hizmetler
 - Yalnızca ASP.NET Core devlet hizmetleri

**Dışarıdan maruz kalan bir hizmet,** genellikle bir yük dengeleyicisi aracılığıyla kümenin dışından çağrılan bir bitiş noktasını ortaya çıkaran hizmettir.

Yalnızca **dahili** hizmet, bitiş noktası yalnızca küme içinden çağrılan hizmettir.

> [!NOTE]
> Devlet hizmeti bitiş noktaları genellikle internete maruz kalmamalıdır. Azure Yük Dengeleyicisi gibi Service Fabric hizmet çözünürlüğünden habersiz olan yük dengeleyicilerinin arkasındaki kümeler, devlet tarafından sunulan hizmetleri ortaya çıkaramaz. Bunun nedeni, yük dengeleyicinin trafiği uygun durumlu hizmet yinelemesine bulup yönlendiremeyeceğidir. 

### <a name="externally-exposed-aspnet-core-stateless-services"></a>Core ASP.NET stateless hizmetleri harici maruz
Kerkenez dış, internete bakan HTTP uç noktalarını ortaya çıkaran ön uç hizmetleri için önerilen web sunucusudur. Windows'da HTTP.sys, aynı bağlantı noktasını kullanarak aynı düğüm kümesinde birden çok web hizmeti barındırmanıza olanak tanıyan bağlantı noktası paylaşımı özelliği sağlayabilir. Bu senaryoda, web hizmetleri http yönlendirme sağlamak için bir ön uç proxy veya ağ geçidi güvenmeden, ana bilgisayar adı veya yol ile ayırt edilir.
 
Internet'e maruz kaldığında, devletsiz bir hizmet, yük dengeleyicisi aracılığıyla ulaşılabilen iyi bilinen ve kararlı bir uç nokta kullanmalıdır. Bu URL'yi uygulamanızın kullanıcılarına sağlarsınız. Aşağıdaki yapılandırmayı öneririz:

|  |  | **Notlar** |
| --- | --- | --- |
| Web sunucusu | Kestrel | Kerkenez, Windows ve Linux'ta desteklendikçe tercih edilen web sunucusudur. |
| Bağlantı noktası yapılandırması | static | `Endpoints` ServiceManifest.xml'in yapılandırmasında, http için 80 veya HTTPS için 443 gibi iyi bilinen statik bağlantı noktası yapılandırılmalıdır. |
| ServiceFabricIntegrationOptions | None | Hizmetin `ServiceFabricIntegrationOptions.None` benzersiz bir tanımlayıcı için gelen isteklerini doğrulamaya çalışmaması için Service Fabric tümleştirme aracını yapılandırırken seçeneği kullanın. Uygulamanızın harici kullanıcıları, ara yazılımın kullandığı benzersiz tanımlayıcı bilgileri bilmeyecek. |
| Örnek Sayısı | -1 | Tipik kullanım durumlarında, örnek sayısı ayarı *-1*olarak ayarlanmalıdır. Bu, bir örneğin yük dengeleyicisinden trafik alan tüm düğümlerde kullanılabilen şekilde yapılır. |

Birden çok dışarıdan maruz kalan hizmet aynı düğüm kümesini paylaşıyorsa, http.sys'yi benzersiz ancak kararlı bir URL yolu ile kullanabilirsiniz. Bunu, IWebHost'u yapılandırırken sağlanan URL'yi değiştirerek gerçekleştirebilirsiniz. Bunun yalnızca HTTP.sys için geçerli olduğunu unutmayın.

 ```csharp
 new HttpSysCommunicationListener(serviceContext, "ServiceEndpoint", (url, listener) =>
 {
     url += "/MyUniqueServicePath";
 
     return new WebHostBuilder()
         .UseHttpSys()
         ...
         .UseUrls(url)
         .Build();
 })
 ```

### <a name="internal-only-stateless-aspnet-core-service"></a>Dahili sadece vatansız ASP.NET Core hizmeti
Yalnızca küme içinden çağrılan devletsiz hizmetler, birden çok hizmet arasında işbirliği sağlamak için benzersiz URL'ler ve dinamik olarak atanmış bağlantı noktaları kullanmalıdır. Aşağıdaki yapılandırmayı öneririz:

|  |  | **Notlar** |
| --- | --- | --- |
| Web sunucusu | Kestrel | Http.sys'yi dahili devletsiz hizmetler için kullanabiliyor olsanız da, Kestrel birden çok hizmet örneğinin ana bilgisayarını paylaşmasına izin veren en iyi sunucudur.  |
| Bağlantı noktası yapılandırması | dinamik olarak atanmış | Bir devlet hizmetinin birden çok yinelemesi bir ana bilgisayar işlemini veya ana bilgisayar işletim sistemini paylaşabilir ve bu nedenle benzersiz bağlantı noktaları gerekir. |
| ServiceFabricIntegrationOptions | UseUniqueServiceUrl | Dinamik bağlantı noktası ataması ile bu ayar, daha önce açıklanan yanlış kimlik sorununu önler. |
| InstanceCount | herhangi bir | Örnek sayısı ayarı, hizmeti çalıştırmak için gereken herhangi bir değere ayarlanabilir. |

### <a name="internal-only-stateful-aspnet-core-service"></a>Dahili yalnızca devletASP.NET Core hizmeti
Yalnızca küme içinden çağrılan devlet hizmetleri, birden çok hizmet arasında işbirliği sağlamak için dinamik olarak atanmış bağlantı noktalarını kullanmalıdır. Aşağıdaki yapılandırmayı öneririz:

|  |  | **Notlar** |
| --- | --- | --- |
| Web sunucusu | Kestrel | Yinelemeler `HttpSysCommunicationListener` bir ana bilgisayar işlemini paylaştığı durum lu hizmetler tarafından kullanılmak üzere tasarlanmıyor. |
| Bağlantı noktası yapılandırması | dinamik olarak atanmış | Bir devlet hizmetinin birden çok yinelemesi bir ana bilgisayar işlemini veya ana bilgisayar işletim sistemini paylaşabilir ve bu nedenle benzersiz bağlantı noktaları gerekir. |
| ServiceFabricIntegrationOptions | UseUniqueServiceUrl | Dinamik bağlantı noktası ataması ile bu ayar, daha önce açıklanan yanlış kimlik sorununu önler. |

## <a name="next-steps"></a>Sonraki adımlar
[Visual Studio'yu kullanarak Service Fabric uygulamanızda hata ayıklama](service-fabric-debugging-your-application.md)


<!--Image references-->
[0]:./media/service-fabric-reliable-services-communication-aspnetcore/webhost-standalone.png
[1]:./media/service-fabric-reliable-services-communication-aspnetcore/webhost-servicefabric.png
[2]:./media/service-fabric-reliable-services-communication-aspnetcore/integration.png
[3]:./media/service-fabric-reliable-services-communication-aspnetcore/httpsys.png
[4]:./media/service-fabric-reliable-services-communication-aspnetcore/kestrel.png
