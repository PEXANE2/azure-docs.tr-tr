---
title: Visual Studio'da Uygulama Öngörülerini Kaldırma - Azure Monitör
description: Visual Studio'da ASP.NET ve ASP.NET Core için Application Insights SDK nasıl kaldırılır?
ms.topic: conceptual
ms.date: 04/06/2020
ms.openlocfilehash: 1c9ff8d3d305645ac7d113421e2c6c5f8451bd2b
ms.sourcegitcommit: 6397c1774a1358c79138976071989287f4a81a83
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "80805111"
---
# <a name="how-to-remove-application-insights-in-visual-studio"></a>Visual Studio'da Uygulama Öngörüleri nasıl kaldırılır?

Bu makalede, Visual Studio'da ASP.NET ve ASP.NET Core Application Insights SDK'yı nasıl kaldıracağınızı gösterecektir.

Uygulama Öngörülerini kaldırmak için, uygulamanızdaki NuGet paketlerini ve başvurularını API'den kaldırmanız gerekir. NuGet paketlerini Paket Yönetim Konsolu'nu kullanarak kaldırabilir veya Visual Studio'da NuGet Çözümlerini Yönetebilirsiniz. Aşağıdaki bölümler, NuGet Paketlerini kaldırmanın iki yolunu ve projenize otomatik olarak eklenenleri gösterir. Eklenen dosyaları ve API'ye arama yaptığınız kendi kodunuzdaki alanların kaldırıldığını doğruladığınızdan emin olun.

## <a name="uninstall-using-the-package-management-console"></a>Paket Yönetim Konsolu'nu kullanarak kaldırma

# <a name="net"></a>[.NET](#tab/net)

1. Paket Yönetim Konsolu'nu açmak için üst menüde NuGet Paket Yöneticisi > Paket Yöneticisi Konsolu > Araçlar'ı seçin.
     
    ![Üst menüde Araçlar > NuGet Paket Yöneticisi > Paket Yöneticisi Konsolu'nu tıklatın](./media/remove-application-insights/package-manager.png)

    > [!NOTE]
    > İzleme koleksiyonu etkinse, önce Microsoft.ApplicationInsights.TraceListener'ı kaldırmanız gerekir. Microsoft.ApplicationInsights.Web'i kaldırmak için aşağıdaki adımı girin. `Uninstall-package Microsoft.ApplicationInsights.TraceListener`

1. Aşağıdaki komutu girin: `Uninstall-Package Microsoft.ApplicationInsights.Web -RemoveDependencies`

    Komutu girdikten sonra, Uygulama Öngörüleri paketi ve tüm bağımlılıkları projeden kaldırılır.
    
    ![Konsola komut girin](./media/remove-application-insights/package-management-console.png)

# <a name="net-core"></a>[.NET Core](#tab/netcore)

1. Paket Yönetim Konsolu'nu açmak için üst menüde NuGet Paket Yöneticisi > Paket Yöneticisi Konsolu > Araçlar'ı seçin.

    ![Üst menüde Araçlar > NuGet Paket Yöneticisi > Paket Yöneticisi Konsolu'nu tıklatın](./media/remove-application-insights/package-manager.png)

1. Aşağıdaki komutu girin: ` Uninstall-Package Microsoft.ApplicationInsights.AspNetCore -RemoveDependencies`

    Komutu girdikten sonra, Uygulama Öngörüleri paketi ve tüm bağımlılıkları projeden kaldırılır.

---

## <a name="uninstall-using-the-visual-studio-nugetui"></a>Visual Studio NuGet UI kullanarak kaldırma

# <a name="net"></a>[.NET](#tab/net)

1. Sağdaki *Çözüm Gezgini'nde*  **Çözüm'e** sağ tıklayın ve **Çözüm için Paketleri Yönet'i**seçin.

    Daha sonra projenin bir parçası olan tüm NuGet paketlerini düzenlemesine olanak tanıyan bir ekran görürsünüz.
    
     ![Çözüm Gezgini'nde Sağ tıklatma, ardından Çözüm için Paketleri Yönet'i seçin](./media/remove-application-insights/manage-nuget-framework.png)

    > [!NOTE]
    > İzleme koleksiyonu etkinse, önce microsoft.applicationinsights.TraceListener'ı seçilen bağımlılıkları kaldırmadan kaldırmanız ve ardından microsoft.applicationInsights.Web'i kaldırma bağımlılıklarını kaldırmanız için aşağıdaki adımları izlemeniz gerekir.
    
1. "Microsoft.ApplicationInsights.Web" paketine tıklayın.Sağda, tüm projeleri seçmek için *Project'in* yanındaki onay kutusunu işaretleyin.
    
1. Tüm bağımlılıkları kaldırmak için, projeyi seçtiğiniz bölümün altındaki **Seçenekler** açılır düğmesini seçin.

     *Kaldırma Seçenekleri'nin*altında, *bağımlılıkları kaldır'ın*yanındaki onay kutusunu seçin.

1. **Kaldır**'ı seçin.
    
    ![Bağımlılıkları kaldır'ı denetleyin, ardından kaldırın](./media/remove-application-insights/uninstall-framework.png)

    Uygulamadan kaldırılacak tüm bağımlılıkları gösteren bir iletişim kutusu görüntülenir.Kaldırmak için **tamam'ı** seçin.
    
    ![Bağımlılıkları kaldır'ı denetleyin, ardından kaldırın](./media/remove-application-insights/preview-uninstall-framework.png)
    
1.  Her şey yüklendikten *sonra, Solution Explorer'da*"ApplicationInsights.config" ve "AiHandleErrorAttribute.cs" ile karşılaşabilirsiniz.İki dosyayı el ile silebilirsiniz.

# <a name="net-core"></a>[.NET Core](#tab/netcore)

1. Sağdaki *Çözüm Gezgini'nde*  **Çözüm'e** sağ tıklayın ve **Çözüm için Paketleri Yönet'i**seçin.

    Daha sonra projenin bir parçası olan tüm NuGet paketlerini düzenlemesine olanak tanıyan bir ekran görürsünüz.

    ![Çözüm Gezgini'nde Sağ tıklatma, ardından Çözüm için Paketleri Yönet'i seçin](./media/remove-application-insights/manage-nuget-core.png)

1. "Microsoft.ApplicationInsights.AspNetCore" paketine tıklayın. Sağda, tüm projeleri seçmek için *Project'in* yanındaki onay kutusunu işaretleyin ve ardından **Kaldır'ı**seçin.

    ![Bağımlılıkları kaldır'ı denetleyin, ardından kaldırın](./media/remove-application-insights/uninstall-core.png)

---

## <a name="what-is-created-when-you-add-application-insights"></a>Uygulama Öngörüleri eklediğinizde neler oluşturulur?

Projenize Uygulama Öngörüleri eklediğinizde, dosya oluşturur ve bazı dosyalarınıza kod ekler. NuGet Paketlerini yalnızca kaldırma, dosyaları ve kodu her zaman atmaz. Uygulama Öngörülerini tam olarak kaldırmak için, projenize eklediğiniz API çağrılarıyla birlikte eklenen kodu veya dosyaları kontrol etmeli ve el ile silmeniz gerekir.

# <a name="net"></a>[.NET](#tab/net)

Visual Studio ASP.NET projesine Uygulama Öngörüleri Telemetrisi eklediğinizde, aşağıdaki dosyaları ekler:

- ApplicationInsights.config
- AiHandleErrorAttribute.cs

Aşağıdaki kod parçaları eklenir:

- [Projenizin adı].csproj

    ```C#
     <ApplicationInsightsResourceId>/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/Default-ApplicationInsights-EastUS/providers/microsoft.insights/components/WebApplication4</ApplicationInsightsResourceId>
    ```

- Paketler.config

    ```xml
    <packages>
    ...
    
      <package id="Microsoft.ApplicationInsights" version="2.12.0" targetFramework="net472" />
      <package id="Microsoft.ApplicationInsights.Agent.Intercept" version="2.4.0" targetFramework="net472" />
      <package id="Microsoft.ApplicationInsights.DependencyCollector" version="2.12.0" targetFramework="net472" />
      <package id="Microsoft.ApplicationInsights.PerfCounterCollector" version="2.12.0" targetFramework="net472" />
      <package id="Microsoft.ApplicationInsights.Web" version="2.12.0" targetFramework="net472" />
      <package id="Microsoft.ApplicationInsights.WindowsServer" version="2.12.0" targetFramework="net472" />
      <package id="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel" version="2.12.0" targetFramework="net472" />
    
      <package id="Microsoft.AspNet.TelemetryCorrelation" version="1.0.7" targetFramework="net472" />
    
      <package id="System.Buffers" version="4.4.0" targetFramework="net472" />
      <package id="System.Diagnostics.DiagnosticSource" version="4.6.0" targetFramework="net472" />
      <package id="System.Memory" version="4.5.3" targetFramework="net472" />
      <package id="System.Numerics.Vectors" version="4.4.0" targetFramework="net472" />
      <package id="System.Runtime.CompilerServices.Unsafe" version="4.5.2" targetFramework="net472" />
    ...
    </packages>
    ```

- Düzen.cshtml

    Projenizde Layout.cshtml dosyası varsa aşağıdaki kod eklenir.
    
    ```html
    <head>
    ...
        <script type = 'text/javascript' >
            var appInsights=window.appInsights||function(config)
            {
                function r(config){ t[config] = function(){ var i = arguments; t.queue.push(function(){ t[config].apply(t, i)})} }
                var t = { config:config},u=document,e=window,o='script',s=u.createElement(o),i,f;for(s.src=config.url||'//az416426.vo.msecnd.net/scripts/a/ai.0.js',u.getElementsByTagName(o)[0].parentNode.appendChild(s),t.cookie=u.cookie,t.queue=[],i=['Event','Exception','Metric','PageView','Trace','Ajax'];i.length;)r('track'+i.pop());return r('setAuthenticatedUserContext'),r('clearAuthenticatedUserContext'),config.disableExceptionTracking||(i='onerror',r('_'+i),f=e[i],e[i]=function(config, r, u, e, o) { var s = f && f(config, r, u, e, o); return s !== !0 && t['_' + i](config, r, u, e, o),s}),t
            }({
                instrumentationKey:'00000000-0000-0000-0000-000000000000'
            });
            
            window.appInsights=appInsights;
            appInsights.trackPageView();
        </script>
    ...
    </head>
    ```

- ConnectedService.json

    ```json
    {
      "ProviderId": "Microsoft.ApplicationInsights.ConnectedService.ConnectedServiceProvider",
      "Version": "16.0.0.0",
      "GettingStartedDocument": {
        "Uri": "https://go.microsoft.com/fwlink/?LinkID=613413"
      }
    }
    ```

- FilterConfig.cs

    ```csharp
            public static void RegisterGlobalFilters(GlobalFilterCollection filters)
            {
                filters.Add(new ErrorHandler.AiHandleErrorAttribute());// This line was added
            }
    ```

# <a name="net-core"></a>[.NET Core](#tab/netcore)

Visual Studio ASP.NET Core şablon projesine Uygulama Öngörüleri Telemetrisi eklediğinizde, aşağıdaki kodu ekler:

- [Projenizin adı].csproj

    ```csharp
      <PropertyGroup>
        <TargetFramework>netcoreapp3.1</TargetFramework>
        <ApplicationInsightsResourceId>/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/Default-ApplicationInsights-EastUS/providers/microsoft.insights/components/WebApplication4core</ApplicationInsightsResourceId>
      </PropertyGroup>
    
      <ItemGroup>
        <PackageReference Include="Microsoft.ApplicationInsights.AspNetCore" Version="2.12.0" />
      </ItemGroup>
    
      <ItemGroup>
        <WCFMetadata Include="Connected Services" />
      </ItemGroup>
    ```

- Appsettings.json:

    ```json
    "ApplicationInsights": {
        "InstrumentationKey": "00000000-0000-0000-0000-000000000000"
    ```

- ConnectedService.json
    
    ```json
    {
      "ProviderId": "Microsoft.ApplicationInsights.ConnectedService.ConnectedServiceProvider",
      "Version": "16.0.0.0",
      "GettingStartedDocument": {
        "Uri": "https://go.microsoft.com/fwlink/?LinkID=798432"
      }
    }
    ```
- Startup.cs

    ```csharp
       public void ConfigureServices(IServiceCollection services)
            {
                services.AddRazorPages();
                services.AddApplicationInsightsTelemetry(); // This is added
            }
    ```

---

## <a name="next-steps"></a>Sonraki adımlar

- [Azure İzleyici](https://docs.microsoft.com/azure/azure-monitor/overview)