---
title: Azure Hizmet Kumaşı, Bulut Hizmeti ve Sanal Makinelerde .NET uygulamaları için Anlık Görüntü Hata Ayıklama'yı etkinleştirin | Microsoft Dokümanlar
description: Azure Hizmet Kumaşı, Bulut Hizmeti ve Sanal Makinelerdeki .NET uygulamaları için Anlık Görüntü Hata Ayıklama'yı etkinleştirme
ms.topic: conceptual
author: brahmnes
ms.author: bfung
ms.date: 03/07/2019
ms.reviewer: mbullwin
ms.openlocfilehash: 194a2da23c8fb405c492df8f6ee173cc97fde4ec
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77671363"
---
# <a name="enable-snapshot-debugger-for-net-apps-in-azure-service-fabric-cloud-service-and-virtual-machines"></a>Azure Hizmet Kumaşı, Bulut Hizmeti ve Sanal Makinelerdeki .NET uygulamaları için Anlık Görüntü Hata Ayıklama'yı etkinleştirme

ASP.NET veya ASP.NET temel uygulamanız Azure Uygulama Hizmeti'nde çalışıyorsa, [Application Insights portal sayfası üzerinden Anlık Görüntü Hata Ayıklayıcı'sını etkinleştirmeniz](snapshot-debugger-appservice.md?toc=/azure/azure-monitor/toc.json)önerilir. Ancak, uygulamanız özelleştirilmiş bir Anlık Görüntü Hata Ayıklayıcı yapılandırması veya .NET çekirdeğinin önizleme sürümü gerektiriyorsa, bu yönerge, [Application Insights portal sayfasından etkinleştirme](snapshot-debugger-appservice.md?toc=/azure/azure-monitor/toc.json)yönergelerine ***ek olarak*** izlenmelidir.

Uygulamanız Azure Hizmet Kumaşı, Bulut Hizmeti, Sanal Makineler veya şirket içi makinelerde çalışıyorsa, aşağıdaki talimatlar kullanılmalıdır. 
    
## <a name="configure-snapshot-collection-for-aspnet-applications"></a>ASP.NET uygulamalar için anlık görüntü koleksiyonunu yapılandırma

1. Henüz yapmadıysanız, [web uygulamanızda Uygulama Öngörüleri'ni etkinleştirin.](../../azure-monitor/app/asp-net.md)

2. [Microsoft.ApplicationInsights.SnapshotCollector](https://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) NuGet paketini uygulamanıza ekleyin.

3. Gerekirse, [ApplicationInsights.config](../../azure-monitor/app/configuration-with-applicationinsights-config.md)eklendi Snapshot Debugger yapılandırması özelleştirilmiş . Varsayılan Anlık Görüntü Hata Ayıklama yapılandırması çoğunlukla boştur ve tüm ayarlar isteğe bağlıdır. Varsayılan yapılandırmaya eşdeğer bir yapılandırmayı gösteren bir örnek aşağıda verilmiştir:

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

4. Anlık görüntüler yalnızca Application Insights'a bildirilen özel durumlar üzerinde toplanır. Bazı durumlarda (örneğin, .NET platformunun eski sürümleri), portaldaki anlık görüntülerle özel durumları görmek için [özel durum koleksiyonunu yapılandırmanız](../../azure-monitor/app/asp-net-exceptions.md#exceptions) gerekebilir.


## <a name="configure-snapshot-collection-for-applications-using-aspnet-core-20-or-above"></a>Core 2.0 veya üzeri ASP.NET kullanarak uygulamalar için anlık görüntü koleksiyonunu yapılandırma

1. Henüz [yapmadıysanız, ASP.NET Core web uygulamanızda Uygulama Öngörülerini etkinleştirin.](../../azure-monitor/app/asp-net-core.md)

    > [!NOTE]
    > Uygulamanızın Microsoft.ApplicationInsights.AspNetCore paketinin 2.1.1 sürümüne veya daha yeni sürümüne atıfta olduğundan emin olun.

2. [Microsoft.ApplicationInsights.SnapshotCollector](https://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) NuGet paketini uygulamanıza ekleyin.

3. Snapshot Collector'ın `Startup` telemetri işlemcisini eklemek ve yapılandırmak için uygulamanızın sınıfını değiştirin.
    1. [Microsoft.ApplicationInsights.SnapshotCollector](https://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) NuGet paket sürümü 1.3.5 veya üzeri kullanılırsa, `Startup.cs`aşağıdaki ifadeleri kullanarak ekleyin.

       ```csharp
            using Microsoft.ApplicationInsights.SnapshotCollector;
       ```

       `Startup` 'deki sınıftaki ConfigureServices yönteminin sonuna aşağıdakileri `Startup.cs`ekleyin.

       ```csharp
            services.AddSnapshotCollector((configuration) => Configuration.Bind(nameof(SnapshotCollectorConfiguration), configuration));
       ```
    2. [Microsoft.ApplicationInsights.SnapshotCollector](https://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) NuGet paket sürümü 1.3.4 veya altında kullanılırsa, `Startup.cs`aşağıdaki ifadeleri kullanarak ekleyin.

       ```csharp
       using Microsoft.ApplicationInsights.SnapshotCollector;
       using Microsoft.Extensions.Options;
       using Microsoft.ApplicationInsights.AspNetCore;
       using Microsoft.ApplicationInsights.Extensibility;
       ```
    
       Aşağıdaki `SnapshotCollectorTelemetryProcessorFactory` sınıfı sınıfa `Startup` ekleyin.
    
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
        Başlangıç `SnapshotCollectorConfiguration` ardışık hattına ve `SnapshotCollectorTelemetryProcessorFactory` hizmetleri ekleyin:
    
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

4. Gerekirse, appsettings.json'a SnapshotCollectorConfiguration bölümü ekleyerek Snapshot Debugger yapılandırmasını özelleştirebilirsiniz. Anlık Görüntü Hata Ayıklama yapılandırmasındaki tüm ayarlar isteğe bağlıdır. Varsayılan yapılandırmaya eşdeğer bir yapılandırmayı gösteren bir örnek aşağıda verilmiştir:

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

1. Uygulamanız Uygulama Öngörüleri ile zaten belirtilmiş değilse, [Uygulama Öngörülerini etkinleştirerek ve enstrümantasyon anahtarını ayarlayarak](../../azure-monitor/app/windows-desktop.md)başlayın.

2. Uygulamanıza [Microsoft.ApplicationInsights.SnapshotCollector](https://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) NuGet paketini ekleyin.

3. Anlık görüntüler yalnızca Application Insights'a bildirilen özel durumlar üzerinde toplanır. Bunları bildirmek için kodunuzu değiştirmeniz gerekebilir. Özel durum işleme kodu uygulamanızın yapısına bağlıdır, ancak bir örnek aşağıdadır:
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

- Uygulamanızda bir özel durum tetikleyebilen trafik oluşturun. Ardından, anlık görüntünün Uygulama Öngörüleri örneğine gönderilmesi için 10 ila 15 dakika bekleyin.
- Azure portalındaki [anlık görüntülere](snapshot-debugger.md?toc=/azure/azure-monitor/toc.json#view-snapshots-in-the-portal) bakın.
- Anlık Görüntü Hata Ayıklama sorunlarıyla ilgili yardım için Bkz. Anlık Hata [Ayıklama sorun giderme.](snapshot-debugger-troubleshoot.md?toc=/azure/azure-monitor/toc.json)
