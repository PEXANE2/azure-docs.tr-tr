---
title: Linux ASP.NET Core uygulamalarını yapılandırma
description: Uygulamanız için önceden oluşturulmuş ASP.NET Core kapsayıcısını nasıl yapılandıracaklarınızı öğrenin. Bu makalede, en yaygın yapılandırma görevleri gösterir.
ms.devlang: dotnet
ms.topic: article
ms.date: 08/13/2019
ms.openlocfilehash: b1d9e59109f5ace25abb9840b48e44ff03d394e7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78255904"
---
# <a name="configure-a-linux-aspnet-core-app-for-azure-app-service"></a>Azure Uygulama Hizmeti için Bir Linux ASP.NET Core uygulamasını yapılandırma

ASP.NET Core uygulamaları derlenmiş ikili olarak dağıtılmalıdır. Visual Studio yayımlama aracı çözümü oluşturur ve ardından derlenen ikilileri doğrudan dağıtır, oysa App Service dağıtım altyapısı önce kod deposunu dağıtır ve sonra ikilileri derler.

Bu kılavuz, App Service'te yerleşik bir Linux kapsayıcısı kullanan ASP.NET Core geliştiricileri için temel kavramlar ve yönergeler sağlar. Azure Uygulama Hizmeti'ni hiç kullanmadıysanız, önce SQL Veritabanı öğreticisiyle [ASP.NET Core quickstart](quickstart-dotnetcore.md) ve [ASP.NET Core'u](tutorial-dotnetcore-sqldb-app.md) izleyin.

## <a name="show-net-core-version"></a>.NET Core sürümünü göster

Geçerli .NET Core sürümünü göstermek için Bulut [Kabuğu'nda](https://shell.azure.com)aşağıdaki komutu çalıştırın:

```azurecli-interactive
az webapp config show --resource-group <resource-group-name> --name <app-name> --query linuxFxVersion
```

Desteklenen tüm .NET Core sürümlerini göstermek için [Bulut Kabuğu'nda](https://shell.azure.com)aşağıdaki komutu çalıştırın:

```azurecli-interactive
az webapp list-runtimes --linux | grep DOTNETCORE
```

## <a name="set-net-core-version"></a>Set .NET Core sürümü

.NET Core sürümünü 2.1 olarak ayarlamak için [Bulut Kabuğu'nda](https://shell.azure.com) aşağıdaki komutu çalıştırın:

```azurecli-interactive
az webapp config set --name <app-name> --resource-group <resource-group-name> --linux-fx-version "DOTNETCORE|2.1"
```

## <a name="customize-build-automation"></a>Yapı otomasyonu özelleştirme

Uygulamanızı Git veya zip paketlerini yapı otomasyonu açık ken kullanırsanız, Uygulama Hizmeti aşağıdaki sırayla otomasyon adımları oluşturur:

1. 'ye göre belirtilirse özel komut dosyası çalıştırın `PRE_BUILD_SCRIPT_PATH`
1. NuGet bağımlılıklarını geri yüklemek için çalıştırın. `dotnet restore`
1. Üretim `dotnet publish` için bir ikili oluşturmak için çalıştırın.
1. 'ye göre belirtilirse özel komut dosyası çalıştırın `POST_BUILD_SCRIPT_PATH`

`PRE_BUILD_COMMAND`ve `POST_BUILD_COMMAND` varsayılan olarak boş olan ortam değişkenleridir. Önceden yapı komutları çalıştırmak `PRE_BUILD_COMMAND`için tanımlayın. Yapı sonrası komutları çalıştırmak `POST_BUILD_COMMAND`için.

Aşağıdaki örnekte, iki değişken virgülle ayrılmış bir dizi komuta göre belirtilir.

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings PRE_BUILD_COMMAND="echo foo, scripts/prebuild.sh"
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings POST_BUILD_COMMAND="echo foo, scripts/postbuild.sh"
```

Yapı otomasyonu özelleştirmek için ek ortam değişkenleri için [Bkz. Oryx yapılandırması.](https://github.com/microsoft/Oryx/blob/master/doc/configuration.md)

Uygulama Hizmeti'nin Linux'ta ASP.NET Core uygulamaları nasıl çalıştığı ve oluşturduğu hakkında daha fazla bilgi için [Oryx belgelerine bakın: .NET Core uygulamaları nasıl algılanır ve oluşturulur.](https://github.com/microsoft/Oryx/blob/master/doc/runtimes/dotnetcore.md)

## <a name="access-environment-variables"></a> Ortam değişkenlerine erişme

Uygulama Hizmeti'nde, [uygulama ayarlarınızı](../configure-common.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#configure-app-settings) uygulama kodunuzu dışında ayarlayabilirsiniz. Daha sonra standart ASP.NET Core bağımlılık enjeksiyon deseni kullanarak herhangi bir sınıfta onlara erişebilirsiniz:

```csharp
using Microsoft.Extensions.Configuration;

namespace SomeNamespace 
{
    public class SomeClass
    {
        private IConfiguration _configuration;
    
        public SomeClass(IConfiguration configuration)
        {
            _configuration = configuration;
        }
    
        public SomeMethod()
        {
            // retrieve App Service app setting
            var myAppSetting = _configuration["MySetting"];
            // retrieve App Service connection string
            var myConnString = _configuration.GetConnectionString("MyDbConnection");
        }
    }
}
```

Örneğin, App Service'de ve *appsettings.json'da*aynı ada sahip bir uygulama ayarını yapılandırırsanız, App Service değeri *appsettings.json* değerinden önceliklidir. Yerel *appsettings.json* değeri uygulamayı yerel olarak hata ayıklamanızı sağlar, ancak App Service değeri uygulamayı üretim ayarlarıyla üründe çalıştırmanızı sağlar. Bağlantı dizeleri aynı şekilde çalışır. Bu şekilde, uygulama sırlarınızı kod deponuzun dışında tutabilir ve kodunuzu değiştirmeden uygun değerlere erişebilirsiniz.

## <a name="get-detailed-exceptions-page"></a>Ayrıntılı özel durumlar sayfası alın

ASP.NET uygulamanız Visual Studio hata ayıklayıcısında bir özel durum oluşturduğunda, tarayıcı ayrıntılı bir özel durum sayfası görüntüler, ancak App Service'de bu sayfa genel bir **HTTP 500** hatasıyla değiştirilir veya **isteğinizi işlerken bir hata oluşur.** iletisi döndürmektedir. Uygulama Hizmeti'nde ayrıntılı özel durum `ASPNETCORE_ENVIRONMENT` sayfasını görüntülemek <a target="_blank" href="https://shell.azure.com" >için, Bulut Kabuğu'nda</a>aşağıdaki komutu çalıştırarak uygulama ayarını uygulamanız için ekleyin.

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings ASPNETCORE_ENVIRONMENT="Development"
```

## <a name="detect-https-session"></a>HTTPS oturumunu algıla

App Service'te, [SSL sonlandırma](https://wikipedia.org/wiki/TLS_termination_proxy) ağı yük dengeleyicilerinde gerçekleşir, böylece tüm HTTPS istekleri uygulamanıza şifrelenmemiş HTTP istekleri olarak ulaşır. Uygulama mantığınızın kullanıcı isteklerinin şifreli olup olmadığını bilmesi gerekiyorsa, İleriye Dönük Üstbilgi Ara ware'i *Startup.cs*olarak yapılandırın:

- Aracı, 'deki üstbilgi `X-Forwarded-For` ve `X-Forwarded-Proto` üstileri iletmek için `Startup.ConfigureServices` [ForwardedHeadersOptions](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) ile yapılandırın.
- Ara yazılımların App Service yük bakiyesine güvenebileceği şekilde bilinen ağlara özel IP adresi aralıkları ekleyin.
- Diğer ara yazılımları çağırmadan `Startup.Configure` önce [UseForwardedHeaders](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) yöntemini çağırın.

Üç öğeyi bir araya getirerek kodunuz aşağıdaki örnek gibi görünür:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.Configure<ForwardedHeadersOptions>(options =>
    {
        options.ForwardedHeaders =
            ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto;
        options.KnownNetworks.Add(new IPNetwork(IPAddress.Parse("::ffff:10.0.0.0"), 104));
        options.KnownNetworks.Add(new IPNetwork(IPAddress.Parse("::ffff:192.168.0.0"), 112));
        options.KnownNetworks.Add(new IPNetwork(IPAddress.Parse("::ffff:172.16.0.0"), 108));
    });
}

public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseForwardedHeaders();

    ...

    app.UseMvc();
}
```

Daha fazla bilgi için proxy [sunucuları ve yük dengeleyicileri ile çalışmak için ASP.NET Core'u yapılandırın'](https://docs.microsoft.com/aspnet/core/host-and-deploy/proxy-load-balancer)a bakın.

## <a name="deploy-multi-project-solutions"></a>Çok projeli çözümleri dağıtma

Kök dizininde *.csproj* dosyası bulunan dağıtım altyapısına bir ASP.NET deposu dağıttığınızda, motor projeyi dağıtır. Kök dizininde *.sln* dosyası bulunan bir ASP.NET deposu dağıttığınızda, motor App Service uygulaması olarak bulduğu ilk Web Sitesini veya Web Uygulama Projesi'ni seçer. Motorun istediğiniz projeyi seçmemesi mümkündür.

Çok projeli bir çözüm dağıtmak için, Uygulama Hizmetinde kullanılacak projeyi iki farklı şekilde belirtebilirsiniz:

### <a name="using-deployment-file"></a>.deployment dosyalarını kullanma

Depo köküne bir *.deployment* dosyası ekleyin ve aşağıdaki kodu ekleyin:

```
[config]
project = <project-name>/<project-name>.csproj
```

### <a name="using-app-settings"></a>Uygulama ayarlarını kullanma

Azure <a target="_blank" href="https://shell.azure.com">Bulut</a>Kabuğu'nda, aşağıdaki CLI komutunu çalıştırarak Uygulama Hizmeti uygulamanıza bir uygulama ayarı ekleyin. * \<Uygulama adı>, * * \<kaynak grubu adı>* ve * \<proje adı>* uygun değerlerle değiştirin.

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings PROJECT="<project-name>/<project-name>.csproj"
```

## <a name="access-diagnostic-logs"></a>Tanılama günlüklerine erişim

[!INCLUDE [Access diagnostic logs](../../../includes/app-service-web-logs-access-no-h.md)]

## <a name="open-ssh-session-in-browser"></a>Tarayıcıda SSH oturumunu açma

[!INCLUDE [Open SSH session in browser](../../../includes/app-service-web-ssh-connect-builtin-no-h.md)]

[!INCLUDE [robots933456](../../../includes/app-service-web-configure-robots933456.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Öğretici: SQL Veritabanı ile ASP.NET Core uygulaması](tutorial-dotnetcore-sqldb-app.md)

> [!div class="nextstepaction"]
> [Uygulama Hizmeti Linux SSS](app-service-linux-faq.md)
