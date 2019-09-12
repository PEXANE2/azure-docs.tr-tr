---
title: Azure Service Fabric, bulut hizmeti ve sanal makinelerde .NET uygulamaları için Snapshot Debugger etkinleştirme | Microsoft Docs
description: Azure Service Fabric, bulut hizmeti ve sanal makinelerde .NET uygulamaları için Snapshot Debugger etkinleştirme
services: application-insights
documentationcenter: ''
author: brahmnes
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.reviewer: mbullwin
ms.date: 03/07/2019
ms.author: bfung
ms.openlocfilehash: 5a6cf763ae16b55806df2acaf2e03fd8c13d1e76
ms.sourcegitcommit: 4b647be06d677151eb9db7dccc2bd7a8379e5871
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/19/2019
ms.locfileid: "68359280"
---
# <a name="enable-snapshot-debugger-for-net-apps-in-azure-service-fabric-cloud-service-and-virtual-machines"></a>Azure Service Fabric, bulut hizmeti ve sanal makinelerde .NET uygulamaları için Snapshot Debugger etkinleştirme

ASP.NET veya ASP.NET Core uygulamanız Azure App Service çalışıyorsa [Application Insights portalı sayfası aracılığıyla Snapshot Debugger etkinleştirmeniz](snapshot-debugger-appservice.md?toc=/azure/azure-monitor/toc.json)önemle önerilir. Bununla birlikte, uygulamanız özelleştirilmiş bir Snapshot Debugger yapılandırması veya .NET Core 'un önizleme sürümünü gerektiriyorsa, bu yönergenin [Application Insights Portal aracılığıyla etkinleştirilmesiyle ilgili yönergelere ek olarak izlenmesi gerekir sayfası](snapshot-debugger-appservice.md?toc=/azure/azure-monitor/toc.json).

Uygulamanız Azure Service Fabric, bulut hizmeti, sanal makineler veya şirket içi makinelerde çalışıyorsa, aşağıdaki yönergelerin kullanılması gerekir. 
    
## <a name="configure-snapshot-collection-for-aspnet-applications"></a>ASP.NET uygulamaları için anlık görüntü koleksiyonunu yapılandırma

1. [Web uygulamanızda Application Insights'ı etkinleştirmek](../../azure-monitor/app/asp-net.md), henüz yapmadınız.

2. Dahil [Microsoft.applicationınsights.snapshotcollectya da](https://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) uygulamanıza NuGet paketi.

3. Gerekirse, [ApplicationInsights. config](../../azure-monitor/app/configuration-with-applicationinsights-config.md)dosyasına eklenen Snapshot Debugger yapılandırması özelleştirilir. Varsayılan Snapshot Debugger yapılandırması çoğunlukla boştur ve tüm ayarlar isteğe bağlıdır. Varsayılan yapılandırmaya bir yapılandırma eşdeğerini gösteren bir örnek aşağıda verilmiştir:

    ```xml
    <TelemetryProcessors>
        <Add Type="Microsoft.ApplicationInsights.SnapshotCollector.SnapshotCollectorTelemetryProcessor, Microsoft.ApplicationInsights.SnapshotCollector">
        <!-- The default is true, but you can disable Snapshot Debugging by setting it to false -->
        <IsEnabled>true</IsEnabled>
        <!-- Snapshot Debugging is usually disabled in developer mode, but you can enable it by setting this to true. -->
        <!-- DeveloperMode is a property on the active TelemetryChannel. -->
        <IsEnabledInDeveloperMode>false</IsEnabledInDeveloperMode>
        <!-- How many times we need to see an exception before we ask for snapshots. -->
        <ThresholdForSnapshotting>1</ThresholdForSnapshotting>
        <!-- The maximum number of examples we create for a single problem. -->
        <MaximumSnapshotsRequired>3</MaximumSnapshotsRequired>
        <!-- The maximum number of problems that we can be tracking at any time. -->
        <MaximumCollectionPlanSize>50</MaximumCollectionPlanSize>
        <!-- How often we reconnect to the stamp. The default value is 15 minutes.-->
        <ReconnectInterval>00:15:00</ReconnectInterval>
        <!-- How often to reset problem counters. -->
        <ProblemCounterResetInterval>1.00:00:00</ProblemCounterResetInterval>
        <!-- The maximum number of snapshots allowed in ten minutes.The default value is 1. -->
        <SnapshotsPerTenMinutesLimit>3</SnapshotsPerTenMinutesLimit>
        <!-- The maximum number of snapshots allowed per day. -->
        <SnapshotsPerDayLimit>30</SnapshotsPerDayLimit>
        <!-- Whether or not to collect snapshot in low IO priority thread. The default value is true. -->
        <SnapshotInLowPriorityThread>true</SnapshotInLowPriorityThread>
        <!-- Agree to send anonymous data to Microsoft to make this product better. -->
        <ProvideAnonymousTelemetry>true</ProvideAnonymousTelemetry>
        <!-- The limit on the number of failed requests to request snapshots before the telemetry processor is disabled. -->
        <FailedRequestLimit>3</FailedRequestLimit>
        </Add>
    </TelemetryProcessors>
    ```

4. Application ınsights'ı raporlanan özel durumların anlık görüntülerini toplanır. Bazı durumlarda (örneğin, eski sürümleri .NET platformu) için gereksinim duyabileceğiniz [özel durum koleksiyonunu yapılandırma](../../azure-monitor/app/asp-net-exceptions.md#exceptions) portalında anlık görüntülerle özel durumları görmek için.


## <a name="configure-snapshot-collection-for-applications-using-aspnet-core-20-or-above"></a>ASP.NET Core 2,0 veya üstünü kullanarak uygulamalar için anlık görüntü toplamayı yapılandırma

1. [ASP.NET Core web uygulamanızı Application Insights'ı etkinleştirme](../../azure-monitor/app/asp-net-core.md), henüz yapmadınız.

    > [!NOTE]
    > Uygulamanızı sürüm 2.1.1 başvuran emin veya Microsoft.ApplicationInsights.AspNetCore paketin daha yeni olması.

2. Dahil [Microsoft.applicationınsights.snapshotcollectya da](https://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) uygulamanıza NuGet paketi.

3. Uygulamanızın değiştirme `Startup` eklemek ve anlık görüntü toplayıcının telemetri işlemci yapılandırmak için sınıf.
    1. [Microsoft. ApplicationInsights. SnapshotCollector](https://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) NuGet paketi sürüm 1.3.5 veya üzeri kullanılıyorsa, aşağıdaki using deyimlerini öğesine `Startup.cs`ekleyin.

       ```csharp
            using Microsoft.ApplicationInsights.SnapshotCollector;
       ```

       `Startup` İçinde`Startup.cs`sınıfındaki ConfigureServices yönteminin sonuna aşağıdakileri ekleyin.

       ```csharp
            services.AddSnapshotCollector((configuration) => Configuration.Bind(nameof(SnapshotCollectorConfiguration), configuration));
       ```
    2. [Microsoft. ApplicationInsights. SnapshotCollector](https://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) NuGet paketi sürüm 1.3.4 veya sonraki bir sürümü kullanılırsa, aşağıdaki using deyimlerini öğesine `Startup.cs`ekleyin.

       ```csharp
       using Microsoft.ApplicationInsights.SnapshotCollector;
       using Microsoft.Extensions.Options;
       using Microsoft.ApplicationInsights.AspNetCore;
       using Microsoft.ApplicationInsights.Extensibility;
       ```
    
       Aşağıdaki `SnapshotCollectorTelemetryProcessorFactory` sınıfının `Startup` sınıfı.
    
       ```csharp
       class Startup
       {
           private class SnapshotCollectorTelemetryProcessorFactory : ITelemetryProcessorFactory
           {
               private readonly IServiceProvider _serviceProvider;
    
               public SnapshotCollectorTelemetryProcessorFactory(IServiceProvider serviceProvider) =>
                   _serviceProvider = serviceProvider;
    
               public ITelemetryProcessor Create(ITelemetryProcessor next)
               {
                   var snapshotConfigurationOptions = _serviceProvider.GetService<IOptions<SnapshotCollectorConfiguration>>();
                   return new SnapshotCollectorTelemetryProcessor(next, configuration: snapshotConfigurationOptions.Value);
               }
           }
           ...
        ```
        Ekleme `SnapshotCollectorConfiguration` ve `SnapshotCollectorTelemetryProcessorFactory` başlangıç ardışık düzenine Hizmetleri:
    
        ```csharp
           // This method gets called by the runtime. Use this method to add services to the container.
           public void ConfigureServices(IServiceCollection services)
           {
               // Configure SnapshotCollector from application settings
               services.Configure<SnapshotCollectorConfiguration>(Configuration.GetSection(nameof(SnapshotCollectorConfiguration)));
    
               // Add SnapshotCollector telemetry processor.
               services.AddSingleton<ITelemetryProcessorFactory>(sp => new SnapshotCollectorTelemetryProcessorFactory(sp));
    
               // TODO: Add other services your application needs here.
           }
       }
       ```

4. Gerekirse, appSettings. json ' a bir SnapshotCollectorConfiguration bölümü ekleyerek Snapshot Debugger yapılandırmasını özelleştirdiniz. Snapshot Debugger yapılandırmasındaki tüm ayarlar isteğe bağlıdır. Varsayılan yapılandırmaya bir yapılandırma eşdeğerini gösteren bir örnek aşağıda verilmiştir:

   ```json
   {
     "SnapshotCollectorConfiguration": {
       "IsEnabledInDeveloperMode": false,
       "ThresholdForSnapshotting": 1,
       "MaximumSnapshotsRequired": 3,
       "MaximumCollectionPlanSize": 50,
       "ReconnectInterval": "00:15:00",
       "ProblemCounterResetInterval":"1.00:00:00",
       "SnapshotsPerTenMinutesLimit": 1,
       "SnapshotsPerDayLimit": 30,
       "SnapshotInLowPriorityThread": true,
       "ProvideAnonymousTelemetry": true,
       "FailedRequestLimit": 3
     }
   }
   ```

## <a name="configure-snapshot-collection-for-other-net-applications"></a>Diğer .NET uygulamaları için anlık görüntü koleksiyonunu yapılandırma

1. Uygulamanızı Application Insights ile izlenen olması değil, başlayın [Application Insights'ı etkinleştirme ve izleme anahtarı ayarını](../../azure-monitor/app/windows-desktop.md).

2. Ekleme [Microsoft.applicationınsights.snapshotcollectya da](https://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) uygulamanıza NuGet paketi.

3. Application ınsights'ı raporlanan özel durumların anlık görüntülerini toplanır. Bunları rapor için kodunuzu değiştirmeniz gerekebilir. Özel durum işleme kodunu uygulamanızın yapısına bağlıdır, ancak bir örnek aşağıda verilmiştir:
    ```csharp
   TelemetryClient _telemetryClient = new TelemetryClient();

   void ExampleRequest()
   {
        try
        {
            // TODO: Handle the request.
        }
        catch (Exception ex)
        {
            // Report the exception to Application Insights.
            _telemetryClient.TrackException(ex);

            // TODO: Rethrow the exception if desired.
        }
   }
    ```

## <a name="next-steps"></a>Sonraki adımlar

- Uygulamanıza bir özel durum tetikleyebilmesi için trafik oluşturun. Ardından, anlık görüntülerin Application Insights örneğine gönderilmesi için 10 ila 15 dakika bekleyin.
- Azure portal [anlık görüntüleri](snapshot-debugger.md?toc=/azure/azure-monitor/toc.json#view-snapshots-in-the-portal) görüntüleyin.
- Sorunları giderme Snapshot Debugger konusunda yardım için bkz. [Snapshot Debugger sorun giderme](snapshot-debugger-troubleshoot.md?toc=/azure/azure-monitor/toc.json).
