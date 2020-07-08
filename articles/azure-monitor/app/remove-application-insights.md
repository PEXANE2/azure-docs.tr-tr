---
title: Visual Studio 'da Application Insights kaldırma-Azure Izleyici
description: Visual Studio 'da ASP.NET ve ASP.NET Core için Application Insights SDK 'Yı kaldırma.
ms.topic: conceptual
ms.date: 04/06/2020
ms.openlocfilehash: 1c9ff8d3d305645ac7d113421e2c6c5f8451bd2b
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "80805111"
---
# <a name="how-to-remove-application-insights-in-visual-studio"></a>Visual Studio 'da Application Insights kaldırma

Bu makalede, Visual Studio 'da ASP.NET ve ASP.NET Core Application Insights SDK 'sını nasıl kaldıracaksınız.

Application Insights kaldırmak için, uygulama içindeki API 'den NuGet paketlerini ve başvuruları kaldırmanız gerekir. NuGet paketlerini Paket Yönetimi konsolunu veya Visual Studio 'da NuGet çözümünü Yönet ' i kullanarak kaldırabilirsiniz. Aşağıdaki bölümler NuGet paketlerini kaldırmanın iki yolunu ve projenize otomatik olarak eklendiklerinizi gösterir. Eklenen dosyaları ve kendi kodunuzda bulunan ve API 'ye çağrı yaptığınız olan alanların kaldırıldığını onaylamanız gerekir.

## <a name="uninstall-using-the-package-management-console"></a>Paket Yönetimi konsolunu kullanarak kaldırma

# <a name="net"></a>[.NET](#tab/net)

1. Paket Yönetimi konsolunu açmak için üstteki menüden Araçlar > NuGet Paket Yöneticisi > Paket Yöneticisi Konsolu ' nu seçin.
     
    ![Üstteki menüde Araçlar > NuGet Paket Yöneticisi > Paket Yöneticisi Konsolu ' na tıklayın.](./media/remove-application-insights/package-manager.png)

    > [!NOTE]
    > İzleme koleksiyonu etkinse, öncelikle Microsoft. ApplicationInsights. TraceListener ' i kaldırmanız gerekir. `Uninstall-package Microsoft.ApplicationInsights.TraceListener`Daha sonra Microsoft. ApplicationInsights. Web ' i kaldırmak için aşağıdaki adımları izleyin.

1. Aşağıdaki komutu girin: `Uninstall-Package Microsoft.ApplicationInsights.Web -RemoveDependencies`

    Komutu girdikten sonra, Application Insights paketi ve tüm bağımlılıkları projeden kaldırılır.
    
    ![Konsola komut girin](./media/remove-application-insights/package-management-console.png)

# <a name="net-core"></a>[.NET Core](#tab/netcore)

1. Paket Yönetimi konsolunu açmak için üstteki menüden Araçlar > NuGet Paket Yöneticisi > Paket Yöneticisi Konsolu ' nu seçin.

    ![Üstteki menüde Araçlar > NuGet Paket Yöneticisi > Paket Yöneticisi Konsolu ' na tıklayın.](./media/remove-application-insights/package-manager.png)

1. Aşağıdaki komutu girin: ` Uninstall-Package Microsoft.ApplicationInsights.AspNetCore -RemoveDependencies`

    Komutu girdikten sonra, Application Insights paketi ve tüm bağımlılıkları projeden kaldırılır.

---

## <a name="uninstall-using-the-visual-studio-nugetui"></a>Visual Studio NuGet Kullanıcı arabirimini kullanarak kaldırma

# <a name="net"></a>[.NET](#tab/net)

1. Sağdaki *Çözüm Gezgini*    **çözüme**sağ tıklayın   ve **çözüm için NuGet Paketlerini Yönet**' i seçin.

    Bundan sonra, projenin parçası olan tüm NuGet paketlerini düzenlemenize olanak tanıyan bir ekran görürsünüz.
    
     ![Çözüm Gezgini sağ tıklayın, sonra çözüm için NuGet Paketlerini Yönet ' i seçin.](./media/remove-application-insights/manage-nuget-framework.png)

    > [!NOTE]
    > İzleme koleksiyonu etkinse, önce bağımlılıkları kaldırmadan Microsoft. ApplicationInsights. TraceListener ' ı kaldırmanız ve ardından aşağıdaki adımları izleyerek Microsoft. ApplicationInsights. Web 'i Kaldır seçili bir şekilde kaldırmanız gerekir.
    
1. "Microsoft. ApplicationInsights. Web" paketine tıklayın.Sağ tarafta, tüm projeler ' i seçmek için *Proje*' nin yanındaki onay kutusunu işaretleyin   .
    
1. Kaldırma sırasında tüm bağımlılıkları kaldırmak için, **Options**   projeyi seçtiğiniz bölümün altında bulunan seçenekler açılan düğmesini seçin.

     *Kaldırma seçenekleri*altında *bağımlılıkları kaldır*' ın yanındaki onay kutusunu işaretleyin.

1. **Kaldır**'ı seçin.
    
    ![Bağımlılıkları kaldır ve Kaldır](./media/remove-application-insights/uninstall-framework.png)

    Uygulamadan kaldırılacak tüm bağımlılıkları gösteren bir iletişim kutusu görüntülenir.Kaldırmak için **Tamam ' ı**seçin   .
    
    ![Bağımlılıkları kaldır ve Kaldır](./media/remove-application-insights/preview-uninstall-framework.png)
    
1.  Her şey kaldırıldıktan sonra, *Çözüm Gezgini*"ApplicationInsights.config" ve "AiHandleErrorAttribute.cs" görünmeye devam edebilir.İki dosyayı el ile silebilirsiniz.

# <a name="net-core"></a>[.NET Core](#tab/netcore)

1. Sağdaki *Çözüm Gezgini*    **çözüme**sağ tıklayın   ve **çözüm için NuGet Paketlerini Yönet**' i seçin.

    Bundan sonra, projenin parçası olan tüm NuGet paketlerini düzenlemenize olanak tanıyan bir ekran görürsünüz.

    ![Çözüm Gezgini sağ tıklayın, sonra çözüm için NuGet Paketlerini Yönet ' i seçin.](./media/remove-application-insights/manage-nuget-core.png)

1. "Microsoft. ApplicationInsights. AspNetCore" paketine tıklayın. Sağ tarafta, *Proje* ' nin yanındaki onay kutusunu işaretleyerek tüm projeler ' i seçin ve **Kaldır**' ı seçin.

    ![Bağımlılıkları kaldır ve Kaldır](./media/remove-application-insights/uninstall-core.png)

---

## <a name="what-is-created-when-you-add-application-insights"></a>Application Insights eklediğinizde ne oluşturulur

Projenize Application Insights eklediğinizde dosya oluşturup dosyalarınıza bazılarına kod ekler. NuGet paketlerini yalnızca kaldırmak, dosyaları ve kodu her zaman atmaz. Application Insights tamamen kaldırmak için eklenen kodu veya dosyaları projenize eklediğiniz tüm API çağrılarıyla birlikte denetlemeniz ve el ile silmeniz gerekir.

# <a name="net"></a>[.NET](#tab/net)

Visual Studio ASP.NET projesine Application Insights Telemetri eklediğinizde, aşağıdaki dosyaları ekler:

- ApplicationInsights.config
- AiHandleErrorAttribute.cs

Aşağıdaki kod parçaları eklenmiştir:

- [Projenizin adı]. csproj

    ```C#
     <ApplicationInsightsResourceId>/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/Default-ApplicationInsights-EastUS/providers/microsoft.insights/components/WebApplication4</ApplicationInsightsResourceId>
    ```

- Packages.config

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

- Layout. cshtml

    Projenizde bir Layout. cshtml dosyası varsa aşağıdaki kod eklenir.
    
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

- Üzerinde ConnectedService.js

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

Visual Studio ASP.NET Core şablon projesine Application Insights Telemetri eklediğinizde aşağıdaki kodu ekler:

- [Projenizin adı]. csproj

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

- Appsettings.js:

    ```json
    "ApplicationInsights": {
        "InstrumentationKey": "00000000-0000-0000-0000-000000000000"
    ```

- Üzerinde ConnectedService.js
    
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