---
title: Linux ASP.NET Core uygulamalarını yapılandırma
description: Uygulamanız için önceden oluşturulmuş bir ASP.NET Core kapsayıcısını yapılandırmayı öğrenin. Bu makalede en sık kullanılan yapılandırma görevleri gösterilmektedir.
ms.devlang: dotnet
ms.topic: article
ms.date: 06/02/2020
ms.openlocfilehash: e009f5b1fc656f700b3f0e76dda6e545aed535d2
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84905774"
---
# <a name="configure-a-linux-aspnet-core-app-for-azure-app-service"></a>Azure App Service için bir Linux ASP.NET Core uygulaması yapılandırma

ASP.NET Core uygulamalar derlenmiş ikili dosyalar olarak dağıtılmalıdır. Visual Studio yayımlama aracı çözümü oluşturur ve ardından derlenmiş ikilileri doğrudan dağıtır, ancak App Service dağıtım motoru önce kod deposunu dağıtır ve ardından ikilileri derler.

Bu kılavuzda, App Service yerleşik bir Linux kapsayıcısını kullanan ASP.NET Core geliştiricilere yönelik temel kavramlar ve yönergeler sağlanmaktadır. Azure App Service hiç kullanmadıysanız, önce [ASP.NET Core hızlı başlangıç](quickstart-dotnetcore.md) ve [ASP.NET Core SQL veritabanı öğreticisiyle](tutorial-dotnetcore-sqldb-app.md) izleyin.

## <a name="show-net-core-version"></a>.NET Core sürümünü göster

Geçerli .NET Core sürümünü göstermek için [Cloud Shell](https://shell.azure.com)aşağıdaki komutu çalıştırın:

```azurecli-interactive
az webapp config show --resource-group <resource-group-name> --name <app-name> --query linuxFxVersion
```

Desteklenen tüm .NET Core sürümlerini göstermek için [Cloud Shell](https://shell.azure.com)aşağıdaki komutu çalıştırın:

```azurecli-interactive
az webapp list-runtimes --linux | grep DOTNETCORE
```

## <a name="set-net-core-version"></a>.NET Core sürümünü ayarla

.NET Core sürümünü 2,1 olarak ayarlamak için [Cloud Shell](https://shell.azure.com) aşağıdaki komutu çalıştırın:

```azurecli-interactive
az webapp config set --name <app-name> --resource-group <resource-group-name> --linux-fx-version "DOTNETCORE|2.1"
```

## <a name="customize-build-automation"></a>Derleme Otomasyonu 'nu özelleştirme

Uygulamanızı, derleme Otomasyonu açıkken git veya ZIP paketleri kullanarak dağıtırsanız, App Service aşağıdaki sırayla Otomasyon adımları oluşturun:

1. Tarafından belirtilmişse özel betiği çalıştırın `PRE_BUILD_SCRIPT_PATH` .
1. `dotnet restore`NuGet bağımlılıklarını geri yüklemek için ' i çalıştırın.
1. `dotnet publish`Üretim için bir ikili oluşturmak üzere ' i çalıştırın.
1. Tarafından belirtilmişse özel betiği çalıştırın `POST_BUILD_SCRIPT_PATH` .

`PRE_BUILD_COMMAND`ve `POST_BUILD_COMMAND` Varsayılan olarak boş olan ortam değişkenleridir. Oluşturma öncesi komutları çalıştırmak için, tanımlayın `PRE_BUILD_COMMAND` . Oluşturma sonrası komutları çalıştırmak için, tanımlayın `POST_BUILD_COMMAND` .

Aşağıdaki örnek, virgülle ayrılmış bir dizi komuta iki değişkeni belirtir.

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings PRE_BUILD_COMMAND="echo foo, scripts/prebuild.sh"
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings POST_BUILD_COMMAND="echo foo, scripts/postbuild.sh"
```

Derleme Otomasyonu 'nu özelleştirmek için ek ortam değişkenleri için bkz. [Oryx Configuration](https://github.com/microsoft/Oryx/blob/master/doc/configuration.md).

App Service nasıl çalıştığı ve Linux 'ta ASP.NET Core uygulamalar derleme hakkında daha fazla bilgi için bkz. [Oryx belgeleri: .NET Core uygulamaları nasıl algılanır ve oluşturulur](https://github.com/microsoft/Oryx/blob/master/doc/runtimes/dotnetcore.md).

## <a name="access-environment-variables"></a> Ortam değişkenlerine erişme

App Service, uygulama ayarlarınızı uygulama kodunuzun dışında [ayarlayabilirsiniz](../configure-common.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#configure-app-settings) . Daha sonra, standart ASP.NET Core bağımlılığı ekleme modelini kullanarak herhangi bir sınıfta bunlara erişebilirsiniz:

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
            // retrieve nested App Service app setting
            var myHierarchicalConfig = _configuration["My:Hierarchical:Config:Data"];
            // retrieve App Service connection string
            var myConnString = _configuration.GetConnectionString("MyDbConnection");
        }
    }
}
```

App Service ve *appsettings.jsüzerinde*aynı ada sahip bir uygulama ayarı yapılandırırsanız, örneğin App Service değeri değeri *appsettings.js* üzerinden önceliklidir. Değer *üzerindeki yerelappsettings.js* , uygulamada yerel olarak hata ayıklamanıza olanak tanır, ancak App Service değeri, uygulamayı üretim ayarları ile birlikte çalıştırmanıza olanak sağlar. Bağlantı dizeleri aynı şekilde çalışır. Bu şekilde, uygulamanızın gizli dizilerini kod deponuzun dışında tutabilir ve kodunuzda değişiklik yapmadan uygun değerlere erişebilirsiniz.

> [!NOTE]
> *appsettings.jsüzerindeki* [Hiyerarşik yapılandırma verilerine](https://docs.microsoft.com/aspnet/core/fundamentals/configuration/#hierarchical-configuration-data) `:` .NET Core için standart olan sınırlayıcı kullanılarak erişildiğini unutmayın. App Service belirli bir hiyerarşik yapılandırma ayarını geçersiz kılmak için, uygulama ayarı adını anahtarda aynı ayrılmış biçimle ayarlayın. [Cloud Shell](https://shell.azure.com)aşağıdaki örneği çalıştırabilirsiniz:

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings My:Hierarchical:Config:Data="some value"
```

## <a name="get-detailed-exceptions-page"></a>Ayrıntılı özel durumlar sayfası al

ASP.NET uygulamanız Visual Studio hata ayıklayıcısında bir özel durum oluşturduğunda, tarayıcıda ayrıntılı bir özel durum sayfası görüntülenir, ancak bu sayfa App Service bir genel **HTTP 500** hatası ile **değiştirilirken veya isteğiniz işlenirken bir hata oluştu.** iletisi döndürmektedir. Ayrıntılı özel durum sayfasını App Service göstermek için, `ASPNETCORE_ENVIRONMENT` <a target="_blank" href="https://shell.azure.com" >Cloud Shell</a>aşağıdaki komutu çalıştırarak uygulama ayarını uygulamanıza ekleyin.

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings ASPNETCORE_ENVIRONMENT="Development"
```

## <a name="detect-https-session"></a>HTTPS oturumunu Algıla

App Service, [SSL sonlandırması](https://wikipedia.org/wiki/TLS_termination_proxy) ağ yükü dengeleyicilerde gerçekleşinceye kadar, tüm https istekleri UYGULAMANıZA şifrelenmemiş HTTP istekleri olarak ulaşacak. Uygulama mantığınızın kullanıcı isteklerinin şifrelenip şifrelenmediğini bilmeleri gerekiyorsa, Iletilen üstbilgiler ara yazılımını *Startup.cs*içinde yapılandırın:

- İçindeki ve üst bilgilerini iletmek için, yazılım yazılımını [Forwardedheadersoptions](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) ile yapılandırın `X-Forwarded-For` `X-Forwarded-Proto` `Startup.ConfigureServices` .
- Bilinen ağlara özel IP adresi aralıkları ekleyin, böylece ara yazılım App Service yük dengeleyiciye güvenebilirler.
- Diğer middlewares çağrılmadan önce içinde [Useforwardedheaders](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) yöntemini çağırın `Startup.Configure` .

Üç öğeyi birlikte koymak, kodunuz aşağıdaki örneğe benzer şekilde görünür:

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

Daha fazla bilgi için bkz. [proxy sunucularıyla ve yük dengeleyicilerle çalışacak ASP.NET Core yapılandırma](https://docs.microsoft.com/aspnet/core/host-and-deploy/proxy-load-balancer).

## <a name="deploy-multi-project-solutions"></a>Çoklu proje çözümlerini dağıtma

Dağıtım altyapısına kök dizinde bir *. csproj* dosyası ile bir ASP.net deposu dağıttığınızda, motor projeyi dağıtır. Kök dizinde *. sln* dosyası ile bir ASP.net deposu dağıttığınızda, altyapı, App Service uygulaması olarak bulduğu Ilk Web sitesini veya Web uygulaması projesini seçer. Altyapının istediğiniz projeyi seçmeme olasılığı vardır.

Çoklu projeli bir çözümü dağıtmak için, App Service ' de kullanmak üzere projeyi iki farklı şekilde belirtebilirsiniz:

### <a name="using-deployment-file"></a>. Deployment dosyasını kullanma

Depo köküne bir *. Deployment* dosyası ekleyin ve aşağıdaki kodu ekleyin:

```
[config]
project = <project-name>/<project-name>.csproj
```

### <a name="using-app-settings"></a>Uygulama ayarlarını kullanma

<a target="_blank" href="https://shell.azure.com">Azure Cloud Shell</a>, aşağıdaki CLI komutunu çalıştırarak App Service uygulamanıza bir uygulama ayarı ekleyin. *\<app-name>*, *\<resource-group-name>* Ve *\<project-name>* değerlerini uygun değerlerle değiştirin.

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings PROJECT="<project-name>/<project-name>.csproj"
```

## <a name="access-diagnostic-logs"></a>Tanılama günlüklerine erişim

ASP.NET Core, [App Service için yerleşik bir günlük oluşturma sağlayıcısı](https://docs.microsoft.com/aspnet/core/fundamentals/logging/#azure-app-service)sağlar. Projenizin *program.cs* içinde, `ConfigureLogging` Aşağıdaki örnekte gösterildiği gibi, sağlayıcıyı uzantı yöntemi aracılığıyla uygulamanıza ekleyin:

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureLogging(logging =>
        {
            logging.AddAzureWebAppDiagnostics();
        })
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.UseStartup<Startup>();
        });
```

Daha sonra [Standart .NET Core düzeniyle](https://docs.microsoft.com/aspnet/core/fundamentals/logging)günlükleri yapılandırabilir ve oluşturabilirsiniz.

[!INCLUDE [Access diagnostic logs](../../../includes/app-service-web-logs-access-linux-no-h.md)]

App Service ASP.NET Core uygulamalar hakkında sorun giderme hakkında daha fazla bilgi için bkz. [ASP.NET Core Azure App Service ve IIS 'de sorun giderme](https://docs.microsoft.com/aspnet/core/test/troubleshoot-azure-iis)

## <a name="open-ssh-session-in-browser"></a>SSH oturumunu tarayıcıda aç

[!INCLUDE [Open SSH session in browser](../../../includes/app-service-web-ssh-connect-builtin-no-h.md)]

[!INCLUDE [robots933456](../../../includes/app-service-web-configure-robots933456.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Öğretici: SQL veritabanı ile uygulama ASP.NET Core](tutorial-dotnetcore-sqldb-app.md)

> [!div class="nextstepaction"]
> [App Service Linux SSS](app-service-linux-faq.md)
