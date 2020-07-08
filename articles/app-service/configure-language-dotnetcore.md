---
title: Windows ASP.NET Core uygulamalarını yapılandırma
description: App Service yerel Windows örneklerinde ASP.NET Core uygulamasının nasıl yapılandırılacağını öğrenin. Bu makalede en sık kullanılan yapılandırma görevleri gösterilmektedir.
ms.devlang: dotnet
ms.topic: article
ms.date: 06/02/2020
ms.openlocfilehash: 5819fc5b2d6e64d1812dacd88a2a4f840f6e03c5
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84908203"
---
# <a name="configure-a-windows-aspnet-core-app-for-azure-app-service"></a>Azure App Service için bir Windows ASP.NET Core uygulaması yapılandırma

> [!NOTE]
> .NET Framework 'de ASP.NET için bkz. [Azure App Service için bir ASP.NET uygulaması yapılandırma](configure-language-dotnet-framework.md)

ASP.NET Core uygulamalar, derlenmiş ikili dosyalar olarak Azure App Service dağıtılacak olmalıdır. Visual Studio yayımlama aracı çözümü oluşturur ve ardından derlenmiş ikilileri doğrudan dağıtır, ancak App Service dağıtım motoru önce kod deposunu dağıtır ve ardından ikilileri derler. Linux uygulamaları hakkında daha fazla bilgi için bkz. [bir Linux ASP.NET Core uygulamasını Azure App Service Için yapılandırma](containers/configure-language-dotnetcore.md).

Bu kılavuz ASP.NET Core geliştiricilere yönelik temel kavramlar ve yönergeler sağlar. Azure App Service hiç kullanmadıysanız, önce [SQL veritabanı öğreticisi ile](app-service-web-tutorial-dotnetcore-sqldb.md) [ASP.net hızlı başlangıç](app-service-web-get-started-dotnet.md) ve ASP.NET Core izleyin.

## <a name="show-supported-net-core-runtime-versions"></a>Desteklenen .NET Core çalışma zamanı sürümlerini göster

App Service, Windows örnekleri için desteklenen tüm .NET Core sürümleri zaten yüklüdür. Kullanabileceğiniz .NET Core çalışma zamanı ve SDK sürümlerini göstermek için, sayfasına gidin `https://<app-name>.scm.azurewebsites.net/DebugConsole` ve tarayıcı tabanlı konsolunda aşağıdaki komutu çalıştırın:

```azurecli-interactive
dotnet --info
```

## <a name="set-net-core-version"></a>.NET Core sürümünü ayarla

ASP.NET Core projeniz için proje dosyasında hedef Framework 'ü ayarlayın. Daha fazla bilgi için bkz. .NET Core belgelerinde [kullanılacak .NET Core sürümünü seçme](https://docs.microsoft.com/dotnet/core/versions/selection) .

## <a name="access-environment-variables"></a> Ortam değişkenlerine erişme

App Service, uygulama ayarlarınızı uygulama kodunuzun dışında [ayarlayabilirsiniz](configure-common.md#configure-app-settings) . Daha sonra, standart ASP.NET Core bağımlılığı ekleme modelini kullanarak herhangi bir sınıfta bunlara erişebilirsiniz:

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

## <a name="deploy-multi-project-solutions"></a>Çoklu proje çözümlerini dağıtma

Visual Studio çözümü birden çok proje içerdiğinde, Visual Studio Publish işlemi dağıtılacak projeyi seçmeyi zaten içerir. Git ile veya ZIP dağıtımı ile olduğu gibi App Service dağıtım altyapısına dağıtırken, derleme Otomasyonu açıkken, App Service dağıtım altyapısı App Service uygulama olarak bulduğu ilk Web sitesini veya Web uygulaması projesini seçer. Uygulama ayarını belirterek App Service hangi projenin kullanılması gerektiğini belirtebilirsiniz `PROJECT` . Örneğin, [Cloud Shell](https://shell.azure.com)aşağıdakileri çalıştırın:

```azurecli-interactive
az webapp config appsettings set --resource-group <resource-group-name> --name <app-name> --settings PROJECT="<project-name>/<project-name>.csproj"
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

[!INCLUDE [Access diagnostic logs](../../includes/app-service-web-logs-access-no-h.md)]

App Service ASP.NET Core uygulamalar hakkında sorun giderme hakkında daha fazla bilgi için bkz. [ASP.NET Core Azure App Service ve IIS 'de sorun giderme](https://docs.microsoft.com/aspnet/core/test/troubleshoot-azure-iis)

## <a name="get-detailed-exceptions-page"></a>Ayrıntılı özel durumlar sayfası al

ASP.NET Core uygulamanız Visual Studio hata ayıklayıcısında bir özel durum oluşturduğunda, tarayıcıda ayrıntılı bir özel durum sayfası görüntülenir, ancak bu sayfanın App Service bir genel **HTTP 500** hatası ile değiştirildiğini veya **isteğiniz işlenirken bir hata meydana geldi.** iletisi döndürmektedir. Ayrıntılı özel durum sayfasını App Service göstermek için, `ASPNETCORE_ENVIRONMENT` <a target="_blank" href="https://shell.azure.com" >Cloud Shell</a>aşağıdaki komutu çalıştırarak uygulama ayarını uygulamanıza ekleyin.

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

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Öğretici: SQL veritabanı ile uygulama ASP.NET Core](app-service-web-tutorial-dotnetcore-sqldb.md)
