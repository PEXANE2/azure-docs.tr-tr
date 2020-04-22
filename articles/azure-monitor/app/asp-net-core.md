---
title: ASP.NET Temel uygulamalar için Azure Uygulama Öngörüleri | Microsoft Dokümanlar
description: Kullanılabilirlik, performans ve kullanım için Core web uygulamalarını ASP.NET izleyin.
ms.topic: conceptual
ms.date: 05/22/2019
ms.openlocfilehash: 1a9a81d76df7f14fb99b8521e7bfa2edff6c9e9e
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81687381"
---
# <a name="application-insights-for-aspnet-core-applications"></a>ASP.NET Çekirdek uygulamaları için Uygulama Öngörüleri

Bu makalede, [ASP.NET Çekirdek](https://docs.microsoft.com/aspnet/core) uygulaması için Uygulama Öngörüleri nasıl etkinleştirilir açıklanmaktadır. Bu makaledeki yönergeleri tamamladığınızda, Application Insights ASP.NET Core uygulamanızdan istekler, bağımlılıklar, özel durumlar, performans sayaçları, sinyaller ve günlükler toplar.

Burada kullanacağımız örnek, hedefleyen `netcoreapp2.2`bir [MVC uygulamasıdır.](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app) Bu talimatları tüm ASP.NET Core uygulamalarına uygulayabilirsiniz.

## <a name="supported-scenarios"></a>Desteklenen senaryolar

[ASP.NET Core için Uygulama Öngörüleri SDK,](https://nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore) uygulamalarınızı nerede ve nasıl çalıştırdıklarına bakılmaksızın izleyebilir. Uygulamanız çalışıyorsa ve Azure'a ağ bağlantısı varsa, telemetri toplanabilir. Uygulama Öngörüleri izleme her yerde desteklenmektedir .NET Core desteklenmektedir. Destek kapakları:
* **İşletim sistemi**: Windows, Linux veya Mac.
* **Barındırma yöntemi**: Süreç içinde veya süreç dışında.
* **Dağıtım yöntemi**: Çerçeveye bağımlı veya bağımsız.
* **Web sunucusu**: IIS (Internet Information Server) veya Kerkenez.
* **Barındırma platformu**: Azure Uygulama Hizmeti, Azure VM, Docker, Azure Kubernetes Hizmeti (AKS) vb. Web Apps özelliği.
* **.NET Çekirdek Çalışma Süresi sürümü**: 1.XX, 2.XX veya 3.XX
* **IDE**: Visual Studio, VS Kodu veya komut satırı.

> [!NOTE]
> Application Insights ile birlikte ASP.NET Core 3.X kullanıyorsanız, lütfen [2.8.0](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore/2.8.0) sürümünü veya daha yüksek sürümünü kullanın. Bu, Core 3.X ASP.NET destekleyen tek sürümüdür.

## <a name="prerequisites"></a>Ön koşullar

- Core uygulaması nın işleyişi ASP.NET. Bir ASP.NET Core uygulaması oluşturmanız gerekiyorsa, bu [ASP.NET Core öğreticisini](https://docs.microsoft.com/aspnet/core/getting-started/)izleyin.
- Geçerli bir Application Insights enstrümantasyon anahtarı. Bu anahtar, Uygulama Öngörüleri'ne herhangi bir telemetri göndermek için gereklidir. Enstrümantasyon anahtarı almak için yeni bir Application Insights [kaynağı](https://docs.microsoft.com/azure/azure-monitor/app/create-new-resource)oluşturmanız gerekiyorsa, bkz.

## <a name="enable-application-insights-server-side-telemetry-visual-studio"></a>Uygulama Öngörülerini etkinleştirserver-side telemetri (Visual Studio)

1. Görsel Stüdyo'da projenizi açın.

    > [!TIP]
    > İsterseniz, Uygulama Öngörüleri'nin yaptığı tüm değişiklikleri izleyebilmek için projeniz için kaynak denetimi ayarlayabilirsiniz. Kaynak denetimini **File** > etkinleştirmek için**Kaynak Denetimine Dosya Ekle'yi**seçin.

2. **Proje** > **Ekle Uygulama Öngörüleri Telemetri'yi seçin.**

3. **Başlat'ı**seçin. Bu seçimin metni Visual Studio'nun sürümüne bağlı olarak değişebilir. Bazı önceki sürümler bunun yerine **Ücretsiz Başlat** düğmesi kullanır.

4. Aboneliğinizi seçin. Ardından **Kaynak** > **Kaydı'nı**seçin.

5. Projenize Uygulama Öngörüleri ekledikten sonra, SDK'nın en son kararlı sürümünden kullandığınızı doğrulamak için kontrol edin. **Proje** > **Yönet NuGet Paketleri** > **Microsoft.ApplicationInsights.AspNetCore**gidin. Gerekirse **Güncelleştir'i**seçin.

     ![Güncelleştirme için Uygulama Öngörüleri paketinin nerede seçilebileceğinizi gösteren ekran görüntüsü](./media/asp-net-core/update-nuget-package.png)

6. İsteğe bağlı ipucunu takip ettiyseniz ve projenizi kaynak denetimine eklediyseniz,**Takım Gezgini** > **Değişikliklerini** **Görüntüle'ye** > gidin. Ardından, Application Insights telemetrisi tarafından yapılan değişikliklerin diff görünümünü görmek için her dosyayı seçin.

## <a name="enable-application-insights-server-side-telemetry-no-visual-studio"></a>Uygulama Öngörüleri sunucu tarafı telemetrisini etkinleştirme (Visual Studio yok)

1. ASP.NET [Core için Uygulama Öngörüleri SDK NuGet paketini](https://nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore)yükleyin. Her zaman en son kararlı sürümü kullanmanızı öneririz. [Açık kaynak GitHub repo](https://github.com/Microsoft/ApplicationInsights-aspnetcore/releases)SDK için tam sürüm notları bulun.

    Aşağıdaki kod örneği, projenizin `.csproj` dosyasına eklenecek değişiklikleri gösterir.

    ```xml
        <ItemGroup>
          <PackageReference Include="Microsoft.ApplicationInsights.AspNetCore" Version="2.13.1" />
        </ItemGroup>
    ```

2. Sınıfınızdaki `services.AddApplicationInsightsTelemetry();` `Startup` `ConfigureServices()` yönteme, aşağıdaki örnekte olduğu gibi ekleyin:

    ```csharp
        // This method gets called by the runtime. Use this method to add services to the container.
        public void ConfigureServices(IServiceCollection services)
        {
            // The following line enables Application Insights telemetry collection.
            services.AddApplicationInsightsTelemetry();
    
            // This code adds other services for your application.
            services.AddMvc();
        }
    ```

3. Enstrümantasyon anahtarını ayarlayın.

    Enstrümantasyon anahtarını bağımsız değişken olarak `AddApplicationInsightsTelemetry`sağlayabilseniz de, enstrümantasyon anahtarını yapılandırmada belirtmenizi öneririz. Aşağıdaki kod örneği, ''de `appsettings.json`bir enstrümantasyon anahtarının nasıl belirtilen Yayımlama `appsettings.json` sırasında uygulama kökü klasörüne kopyalandığından emin olun.

    ```json
        {
          "ApplicationInsights": {
            "InstrumentationKey": "putinstrumentationkeyhere"
          },
          "Logging": {
            "LogLevel": {
              "Default": "Warning"
            }
          }
        }
    ```

    Alternatif olarak, aşağıdaki ortam değişkenlerinden birinde enstrümantasyon anahtarını belirtin:

    * `APPINSIGHTS_INSTRUMENTATIONKEY`

    * `ApplicationInsights:InstrumentationKey`

    Örneğin:

    * `SET ApplicationInsights:InstrumentationKey=putinstrumentationkeyhere`

    * `SET APPINSIGHTS_INSTRUMENTATIONKEY=putinstrumentationkeyhere`

    Genellikle, `APPINSIGHTS_INSTRUMENTATIONKEY` Azure Web Apps'a dağıtılan uygulamalar için enstrümantasyon anahtarını belirtir.

    > [!NOTE]
    > Kodda belirtilen bir enstrümantasyon anahtarı, diğer seçeneklerden daha fazla kazanan ortam değişkenini `APPINSIGHTS_INSTRUMENTATIONKEY`kazanır.

## <a name="run-your-application"></a>Uygulamanızı çalıştırma

Başvurunuzu çalıştırın ve istekte bulunun. Telemetri artık Application Insights'a akmalıdır. Application Insights SDK, aşağıdaki telemetriile birlikte, başvurunuza gelen web isteklerini otomatik olarak toplar.

### <a name="live-metrics"></a>Canlı Ölçümler

[Canlı Ölçümler,](https://docs.microsoft.com/azure/application-insights/app-insights-live-stream) Uygulama Öngörüleri izlemenin doğru yapılandırıp yapılandırılmadı sını hızlı bir şekilde doğrulamak için kullanılabilir. Telemetrinin portalda ve analitikte görünmeye başlaması birkaç dakika sürebilir, ancak Canlı Ölçümler, çalışan işlemin CPU kullanımını neredeyse gerçek zamanlı olarak gösterir. Ayrıca İstekler, Bağımlılıklar, İzler, vb gibi diğer telemetri gösterebilir.

### <a name="ilogger-logs"></a>ILogger günlükleri

Önem derecesi `Warning` veya `ILogger` daha büyük bir şekilde yayılan günlükler otomatik olarak yakalanır. Uygulama Öngörüleri tarafından hangi günlük düzeylerinin yakalandığını özelleştirmek için [ILogger dokümanlarını](ilogger.md#control-logging-level) izleyin.

### <a name="dependencies"></a>Bağımlılıklar

Bağımlılık toplama varsayılan olarak etkinleştirilir. [Bu](asp-net-dependencies.md#automatically-tracked-dependencies) makalede, otomatik olarak toplanan bağımlılıkları açıklar ve el ile izleme yapmak için adımlar da içerir.

### <a name="performance-counters"></a>Performans sayaçları

ASP.NET Core'daki [performans sayaçları](https://azure.microsoft.com/documentation/articles/app-insights-web-monitor-performance/) desteği sınırlıdır:

* SDK sürüm2.4.1 ve daha sonra uygulama Azure Web Apps (Windows) üzerinde çalışıyorsa performans sayaçları toplayın.
* SDK sürümleri 2.7.1 ve daha sonra uygulama Windows ve `NETSTANDARD2.0` hedefleri veya daha sonra çalışıyorsa performans sayaçları toplamak.
* .NET Framework'ü hedefleyen uygulamalarda, SDK'nın tüm sürümleri performans sayaçlarını destekler.
* SDK Sürümleri 2.8.0 ve daha sonra Linux cpu / bellek sayacı destekler. Linux'ta başka hiçbir sayaç desteklenmez. Linux'ta (ve Diğer Windows dışındaki ortamlarda) sistem sayaçları almak için önerilen yol [EventCounters](#eventcounter) kullanarak

### <a name="eventcounter"></a>EventCounter

`EventCounterCollectionModule`varsayılan olarak etkinleştirilir ve .NET Core 3.X uygulamalarından varsayılan bir sayaç kümesi toplar. [EventCounter](eventcounters.md) öğreticisi toplanan varsayılan sayaç kümesini listeler. Ayrıca liste özelleştirme talimatları vardır.

## <a name="enable-client-side-telemetry-for-web-applications"></a>Web uygulamaları için istemci tarafı telemetrisini etkinleştirme

Önceki adımlar, sunucu tarafındaki telemetrileri toplamaya başlamanıza yardımcı olmak için yeterlidir. Uygulamanızda istemci tarafı bileşenleri varsa, [kullanım telemetrisini](https://docs.microsoft.com/azure/azure-monitor/app/usage-overview)toplamaya başlamak için sonraki adımları izleyin.

1. In `_ViewImports.cshtml`, enjeksiyon ekleyin:

    ```cshtml
        @inject Microsoft.ApplicationInsights.AspNetCore.JavaScriptSnippet JavaScriptSnippet
    ```

2. In `_Layout.cshtml`, `HtmlHelper` `<head>` bölümün sonuna ekle ama başka bir komut dosyası önce. Sayfadan herhangi bir özel JavaScript telemetrisini bildirmek istiyorsanız, bu parçacıktan sonra enjekte edin:

    ```cshtml
        @Html.Raw(JavaScriptSnippet.FullScript)
        </head>
    ```
    
Alternatif olarak `FullScript` `ScriptBody` SDK v2.14'ten başlayarak kullanılabilir. İçerik Güvenlik İlkesi `<script>` ayarlamak için etiketi denetlemeniz gerekiyorsa bunu kullanın:

```cshtml
 <script> // apply custom changes to this script tag.
     @Html.Raw(JavaScriptSnippet.ScriptBody)
 </script>
```

Daha `.cshtml` önce başvurulan dosya adları varsayılan Bir MVC uygulama şablonundan dır. Sonuç olarak, uygulamanız için istemci tarafı izlemeyi düzgün bir şekilde etkinleştirmek istiyorsanız, JavaScript `<head>` snippet'inin izlemek istediğiniz uygulamanızın her sayfasının bölümünde görünmesi gerekir. Bu uygulama şablonu için bu hedefi JavaScript snippet ekleyerek `_Layout.cshtml`gerçekleştirebilirsiniz. 

Projenizde dahil `_Layout.cshtml`değilse, istemci tarafı [izleme](https://docs.microsoft.com/azure/azure-monitor/app/website-monitoring)eklemeye devam edebilirsiniz. Bunu, uygulamanızdaki tüm sayfaların `<head>` denetimini sağlayan eşdeğer bir dosyaya JavaScript snippet'i ekleyerek yapabilirsiniz. Veya snippet'i birden çok sayfaya ekleyebilirsiniz, ancak bu çözümü korumak zordur ve genellikle bunu önermiyoruz.

## <a name="configure-the-application-insights-sdk"></a>Uygulama Öngörülerini Yapılandırma SDK

Varsayılan yapılandırmayı değiştirmek için ASP.NET Core için Application Insights SDK'yı özelleştirebilirsiniz. SDK'ASP.NET Uygulama Öngörüleri kullanıcıları kullanarak `ApplicationInsights.config` veya değiştirerek yapılandırmayı `TelemetryConfiguration.Active`değiştirmeye aşina olabilirler. ASP.NET Core için yapılandırmayı farklı şekilde değiştirirsiniz. Uygulamaya ASP.NET Core SDK ekleyin ve ASP.NET Core yerleşik [bağımlılık enjeksiyonu](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection)kullanarak yapılandırır. Aksi yönde yönlendirilmedikçe, sınıfınızın `ConfigureServices()` `Startup.cs` yönteminde hemen hemen tüm yapılandırma değişiklikleri yapın. Aşağıdaki bölümler daha fazla bilgi sunar.

> [!NOTE]
> ASP.NET Core uygulamalarında, değiştirerek `TelemetryConfiguration.Active` yapılandırmayı değiştirmek desteklenmez.

### <a name="using-applicationinsightsserviceoptions"></a>ApplicationInsightsServiceOptions kullanma

Bu örnekte olduğu `ApplicationInsightsServiceOptions` `AddApplicationInsightsTelemetry`gibi, birkaç ortak ayarı , geçerek değiştirebilirsiniz:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    Microsoft.ApplicationInsights.AspNetCore.Extensions.ApplicationInsightsServiceOptions aiOptions
                = new Microsoft.ApplicationInsights.AspNetCore.Extensions.ApplicationInsightsServiceOptions();
    // Disables adaptive sampling.
    aiOptions.EnableAdaptiveSampling = false;

    // Disables QuickPulse (Live Metrics stream).
    aiOptions.EnableQuickPulseMetricStream = false;
    services.AddApplicationInsightsTelemetry(aiOptions);
}
```

Ayarlar tam listesi`ApplicationInsightsServiceOptions`

|Ayar | Açıklama | Varsayılan
|---------------|-------|-------
|Performans Karşıtoplama Modül  | Etkinleştirme/Devre Dışı`PerformanceCounterCollectionModule` | true
|EnableRequestTrackingTelemetryModülü   | Etkinleştirme/Devre Dışı`RequestTrackingTelemetryModule` | true
|EventCounterCollectionModule'i Etkinleştirme   | Etkinleştirme/Devre Dışı`EventCounterCollectionModule` | true
|EnableDependencyTrackingTelemetryModule   | Etkinleştirme/Devre Dışı`DependencyTrackingTelemetryModule` | true
|EnableAppServicesHeartbeatTelemetryModule  |  Etkinleştirme/Devre Dışı`AppServicesHeartbeatTelemetryModule` | true
|EnableAzureInstanceMetadataTelemetryModule   |  Etkinleştirme/Devre Dışı`AzureInstanceMetadataTelemetryModule` | true
|EnableQuickPulseMetricStream | LiveMetrics özelliğini etkinleştirme/devre dışı | true
|AdaptiveSampling'ı Etkinleştir | Uyarlamalı Örneklemeyi Etkinleştirme/Devre Dışı | true
|Sinyal Atışını Etkinleştirme | Düzenli aralıklarla (15 dakika varsayılan) .NET Sürümü, Azure Ortamı bilgileri gibi çalışma zamanı hakkında bilgi içeren 'HeartBeatState' adlı özel bir metrik gönderen Kalp Atışlarını Etkinleştir/Devre Dışı Kaltın özelliği, örneğin. | true
|AddAutoCollectedMetricExtractor | Örnekleme gerçekleşmeden önce İstekler/Bağımlılıklar hakkında önceden toplanmış ölçümler gönderen bir Telemetri İşlemcisi olan Otomatik Toplanan Metrikleri etkinleştirme/devre dışı etme. | true
|İstekCollectionOptions.TrackExceptions | İstek toplama modülü tarafından işlenmemiş Özel Durum izlemeraporlamasını etkinleştirin/devre dışı. | NETSTANDARD2.0 yanlış (Çünkü Özel Durumlar ApplicationInsightsLoggerProvider ile izlenir), aksi doğru.

En güncel liste için [yapılandırılabilir `ApplicationInsightsServiceOptions` ayarlara](https://github.com/microsoft/ApplicationInsights-dotnet/blob/develop/NETCORE/src/Shared/Extensions/ApplicationInsightsServiceOptions.cs) bakın.

### <a name="sampling"></a>Örnekleme

ASP.NET Core için Application Insights SDK hem sabit oranlı hem de uyarlanabilir örneklemeyi destekler. Uyarlanabilir örnekleme varsayılan olarak etkinleştirilir. 

Daha fazla bilgi için bkz: [ASP.NET Core uygulamaları için uyarlamauyarlama uyarlama.](../../azure-monitor/app/sampling.md#configuring-adaptive-sampling-for-aspnet-core-applications)

### <a name="adding-telemetryinitializers"></a>Telemetri EklemeInitializers

Tüm telemetriile gönderilen genel özellikleri tanımlamak istediğinizde [telemetri başharflerini](https://docs.microsoft.com/azure/azure-monitor/app/api-filtering-sampling#addmodify-properties-itelemetryinitializer) kullanın.

Aşağıdaki kodda `DependencyInjection` gösterildiği gibi kapsayıcıya yeni `TelemetryInitializer` lerini ekleyin. SDK, kapsayıcıya eklenen `TelemetryInitializer` leri otomatik olarak alır. `DependencyInjection`

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<ITelemetryInitializer, MyCustomTelemetryInitializer>();
}
```

### <a name="removing-telemetryinitializers"></a>TelemetriBaşlangıçlarını Kaldırma

Telemetri başharfleri varsayılan olarak mevcuttur. Tüm veya belirli telemetri başharflerini kaldırmak için, aşağıdaki `AddApplicationInsightsTelemetry()`örnek *kodu* çağırın.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddApplicationInsightsTelemetry();

    // Remove a specific built-in telemetry initializer
    var tiToRemove = services.FirstOrDefault<ServiceDescriptor>
                        (t => t.ImplementationType == typeof(AspNetCoreEnvironmentTelemetryInitializer));
    if (tiToRemove != null)
    {
        services.Remove(tiToRemove);
    }

    // Remove all initializers
    // This requires importing namespace by using Microsoft.Extensions.DependencyInjection.Extensions;
    services.RemoveAll(typeof(ITelemetryInitializer));
}
```

### <a name="adding-telemetry-processors"></a>Telemetri işlemcileri ekleme

'deki `IServiceCollection`uzantı yöntemini `AddApplicationInsightsTelemetryProcessor` kullanarak `TelemetryConfiguration` özel telemetri işlemcileri ekleyebilirsiniz. [Gelişmiş filtreleme senaryolarında](https://docs.microsoft.com/azure/azure-monitor/app/api-filtering-sampling#itelemetryprocessor-and-itelemetryinitializer)telemetri işlemcileri kullanırsınız. Aşağıdaki örneği kullanın.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // ...
    services.AddApplicationInsightsTelemetry();
    services.AddApplicationInsightsTelemetryProcessor<MyFirstCustomTelemetryProcessor>();

    // If you have more processors:
    services.AddApplicationInsightsTelemetryProcessor<MySecondCustomTelemetryProcessor>();
}
```

### <a name="configuring-or-removing-default-telemetrymodules"></a>Varsayılan Telemetri Modüllerini yapılandırma veya kaldırma

Application Insights, kullanıcı tarafından manuel izleme gerektirmeden belirli iş yükleri hakkında yararlı telemetrileri otomatik olarak toplamak için telemetri modüllerini kullanır.

Aşağıdaki otomatik toplama modülleri varsayılan olarak etkinleştirilir. Bu modüller otomatik olarak telemetri toplamakiçin sorumludur. Varsayılan davranışlarını değiştirmek için devre dışı bırakabilirsiniz veya bunları yapılandırabilirsiniz.

* `RequestTrackingTelemetryModule`- Gelen web isteklerinden RequestTelemetry toplar.
* `DependencyTrackingTelemetryModule`- Giden http aramalarından ve sql aramalarından DependencyTelemetry'yi toplar.
* `PerformanceCollectorModule`- Windows PerformanceCounters toplar.
* `QuickPulseTelemetryModule`- Canlı Ölçümler portalında göstermek için telemetri toplar.
* `AppServicesHeartbeatTelemetryModule`- Uygulamanın barındırıldığı Azure Uygulama Hizmeti ortamı hakkında kalp atışlarını (özel ölçümler olarak gönderilir) toplar.
* `AzureInstanceMetadataTelemetryModule`- Uygulamanın barındırıldığı Azure VM ortamı hakkında kalp atışlarını (özel ölçümler olarak gönderilir) toplar.
* `EventCounterCollectionModule`- [EventCounters toplar.](eventcounters.md) Bu modül yeni bir özelliktir ve SDK Sürüm 2.8.0 ve üzeri sürümlerde mevcuttur.

Herhangi bir varsayılan `TelemetryModule`yapılandırmak için, `IServiceCollection`aşağıdaki örnekte gösterildiği gibi, uzantı yöntemini `ConfigureTelemetryModule<T>` kullanın.

```csharp
using Microsoft.ApplicationInsights.DependencyCollector;
using Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector;

public void ConfigureServices(IServiceCollection services)
{
    services.AddApplicationInsightsTelemetry();

    // The following configures DependencyTrackingTelemetryModule.
    // Similarly, any other default modules can be configured.
    services.ConfigureTelemetryModule<DependencyTrackingTelemetryModule>((module, o) =>
            {
                module.EnableW3CHeadersInjection = true;
            });

    // The following removes all default counters from EventCounterCollectionModule, and adds a single one.
    services.ConfigureTelemetryModule<EventCounterCollectionModule>(
            (module, o) =>
            {
                module.Counters.Clear();
                module.Counters.Add(new EventCounterCollectionRequest("System.Runtime", "gen-0-size"));
            }
        );

    // The following removes PerformanceCollectorModule to disable perf-counter collection.
    // Similarly, any other default modules can be removed.
    var performanceCounterService = services.FirstOrDefault<ServiceDescriptor>(t => t.ImplementationType == typeof(PerformanceCollectorModule));
    if (performanceCounterService != null)
    {
        services.Remove(performanceCounterService);
    }
}
```

2.12.2 sürümüyle [`ApplicationInsightsServiceOptions`](#using-applicationinsightsserviceoptions) başlayarak, varsayılan modüllerden herhangi birini devre dışı bırakmanın kolay bir seçenek olduğunu içerir.

### <a name="configuring-a-telemetry-channel"></a>Bir telemetri kanalının yapılandırılması

Varsayılan `ServerTelemetryChannel`kanal. Aşağıdaki örnekte görüldüğü gibi geçersiz kılabilirsiniz.

```csharp
using Microsoft.ApplicationInsights.Channel;

    public void ConfigureServices(IServiceCollection services)
    {
        // Use the following to replace the default channel with InMemoryChannel.
        // This can also be applied to ServerTelemetryChannel.
        services.AddSingleton(typeof(ITelemetryChannel), new InMemoryChannel() {MaxTelemetryBufferCapacity = 19898 });

        services.AddApplicationInsightsTelemetry();
    }
```

### <a name="disable-telemetry-dynamically"></a>Telemetriyi dinamik olarak devre dışı

Telemetriyi koşullu ve dinamik olarak devre dışı kılmış olmak istiyorsanız, kodunuzun herhangi bir yerinde ASP.NET Çekirdek bağımlılık enjeksiyon konteyneri ile örneği çözebilir `TelemetryConfiguration` ve üzerine bayrak ayarlayabilirsiniz. `DisableTelemetry`

```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddApplicationInsightsTelemetry();
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env, TelemetryConfiguration configuration)
    {
        configuration.DisableTelemetry = true;
        ...
    }
```

Yukarıdaki telemetri toplama herhangi bir otomatik toplama modülleri engellemez. Yukarıdaki yaklaşımla yalnızca Uygulama Öngörüleri'ne telemetri gönderilmesi devre dışı bırakılır. Belirli bir otomatik toplama modülü istenmiyorsa, [telemetri modüllerini çıkarmak](#configuring-or-removing-default-telemetrymodules) en iyisidir

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

### <a name="does-application-insights-support-aspnet-core-3x"></a>Application Insights Core 3.X ASP.NET destekliyor mu?

Evet. ASP.NET Core sürüm 2.8.0 veya üzeri [için Application Insights SDK'ya](https://nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore) güncelleştirin. SDK'nın eski sürümleri Core 3.X ASP.NET desteklemez.

Ayrıca, [buradan](#enable-application-insights-server-side-telemetry-visual-studio)Visual Studio tabanlı yönergeleri kullanıyorsanız, Visual Studio 2019 (16.3.0) en son sürümüne onboard güncelleyin. Visual Studio'nun önceki sürümleri, ASP.NET Core 3.X uygulamaları için otomatik onboarding'i desteklemez.

### <a name="how-can-i-track-telemetry-thats-not-automatically-collected"></a>Otomatik olarak toplanmayan telemetriyi nasıl izleyebilirim?

Yapıcı enjeksiyon `TelemetryClient` kullanarak bir örnek alın ve `TrackXXX()` üzerinde gerekli yöntemi arayın. ASP.NET Core uygulamasında yeni `TelemetryClient` örnekler oluşturmanızı önermiyoruz. Tek tonlu `TelemetryClient` bir örnek zaten `DependencyInjection` telemetri `TelemetryConfiguration` geri kalanı ile paylaşan konteyner, kayıtlıdır. Yeni `TelemetryClient` bir örnek oluşturma, yalnızca telemetrinin geri kalanından ayrı bir yapılandırmaya ihtiyaç duyduğunda önerilir.

Aşağıdaki örnek, denetleyiciden ek telemetrinin nasıl izlenebildiğini gösterir.

```csharp
using Microsoft.ApplicationInsights;

public class HomeController : Controller
{
    private TelemetryClient telemetry;

    // Use constructor injection to get a TelemetryClient instance.
    public HomeController(TelemetryClient telemetry)
    {
        this.telemetry = telemetry;
    }

    public IActionResult Index()
    {
        // Call the required TrackXXX method.
        this.telemetry.TrackEvent("HomePageRequested");
        return View();
    }
```

Uygulama Öngörüleri'nde özel veri raporlaması hakkında daha fazla bilgi için [Bkz. Application Insights özel ölçümleri API referansı.](https://docs.microsoft.com/azure/azure-monitor/app/api-custom-events-metrics/)

### <a name="some-visual-studio-templates-used-the-useapplicationinsights-extension-method-on-iwebhostbuilder-to-enable-application-insights-is-this-usage-still-valid"></a>Bazı Visual Studio şablonları Uygulama Öngörüleri etkinleştirmek için IWebHostBuilder'da UseApplicationInsights() uzantı yöntemini kullanmaktadır. Bu kullanım hala geçerli mi?

Uzantı yöntemi `UseApplicationInsights()` hala desteklenirken, Application Insights SDK sürüm 2.8.0'da eski olarak işaretlenir. SDK'nın bir sonraki ana sürümünde kaldırılacak. Uygulama Öngörüleri telemetrisini etkinleştirmek için `AddApplicationInsightsTelemetry()` önerilen yol, bazı yapılandırmaları denetlemek için aşırı yükleme sağladığından kullanmaktır. Ayrıca, Core 3.X uygulamaları `services.AddApplicationInsightsTelemetry()` ASP.NET uygulama öngörülerini etkinleştirmenin tek yolu.

### <a name="im-deploying-my-aspnet-core-application-to-web-apps-should-i-still-enable-the-application-insights-extension-from-web-apps"></a>ASP.NET Core uygulamamı Web Apps'a dağıtıyorum. Web Apps'tan Uygulama Öngörüleri uzantısını etkinleştirmeli miyim?

SDK bu makalede gösterildiği gibi inşa zamanında yüklenmişse, Uygulama Hizmeti portalından [Uygulama Öngörüleri uzantısını](https://docs.microsoft.com/azure/azure-monitor/app/azure-web-apps) etkinleştirmeniz gerekmez. Uzantı yüklü olsa bile, SDK'nın uygulamaya zaten eklenmiş olduğunu algıladığında geri çekilecek. Uzantıdan Uygulama Öngörüleri'ni etkinleştiriseniz, SDK'yı yüklemeniz ve güncelleştirmeniz gerekmez. Ancak bu makaledeki yönergeleri izleyerek Uygulama Öngörüleri'ni etkinleştiriseniz, daha fazla esnekliğe sahipsiniz, çünkü:

   * Uygulama Öngörüleri telemetri çalışmaya devam edecektir:
       * Windows, Linux ve Mac dahil olmak üzere tüm işletim sistemleri.
       * Bağımsız veya çerçevebağımlısı dahil olmak üzere tüm yayımlama modları.
       * Tam .NET Framework dahil tüm hedef çerçeveler.
       * Web Apps, VM'ler, Linux, kapsayıcılar, Azure Kubernetes Hizmeti ve Azure olmayan barındırma dahil olmak üzere tüm barındırma seçenekleri.
       * Önizleme sürümleri de dahil olmak üzere tüm .NET Core sürümleri.
   * Visual Studio'dan hata ayıklarken telemetriyi yerel olarak görebilirsiniz.
   * API'yi kullanarak ek özel `TrackXXX()` telemetriyi izleyebilirsiniz.
   * Yapılandırma üzerinde tam kontrole sahipsiniz.

### <a name="can-i-enable-application-insights-monitoring-by-using-tools-like-status-monitor"></a>Durum Monitörü gibi araçları kullanarak Uygulama Öngörüleri izlemeyi etkinleştirebilir miyim?

Hayır. [Durum İzle](https://docs.microsoft.com/azure/azure-monitor/app/monitor-performance-live-website-now) mi ve [Durum İzleyicisi v2](https://docs.microsoft.com/azure/azure-monitor/app/status-monitor-v2-overview) şu anda yalnızca ASP.NET 4.x'i destekliyor.

### <a name="is-application-insights-automatically-enabled-for-my-aspnet-core-20-application"></a>ASP.NET Core 2.0 uygulamam için Uygulama Öngörüleri otomatik olarak etkinleştirildi mi?

`Microsoft.AspNetCore.All` 2.0 metapaketi Uygulama Öngörüleri SDK (sürüm 2.1.0) dahil. Uygulamayı Visual Studio hata ayıklama altında çalıştırıyorsanız, Visual Studio Uygulama Öngörüleri sağlar ve Telemetri'yi IDE'nin kendi içinde yerel olarak gösterir. Bir enstrümantasyon anahtarı belirtilmedikçe Telemetri Uygulama Öngörüleri hizmetine gönderilmedi. 2.0 uygulamaları için bile Uygulama Öngörülerini etkinleştirmek için bu makaledeki yönergeleri izlemenizi öneririz.

### <a name="if-i-run-my-application-in-linux-are-all-features-supported"></a>Uygulamamı Linux'ta çalıştırıyorumsa, tüm özellikler desteklenir mi?

Evet. SDK için özellik desteği aşağıdaki istisnalar dışında tüm platformlarda aynıdır:

* [Performans Sayaçları](https://docs.microsoft.com/azure/azure-monitor/app/performance-counters) yalnızca Windows'da desteklendirilir, çünkü SDK Linux'ta Etkinlik [Sayaçları](https://docs.microsoft.com/azure/azure-monitor/app/eventcounters) toplar. Çoğu ölçüm aynıdır.
* Varsayılan `ServerTelemetryChannel` olarak etkin olsa da, uygulama Linux veya MacOS'ta çalışıyorsa, kanal ağ sorunları olduğunda telemetriyi geçici olarak tutmak için otomatik olarak yerel bir depolama klasörü oluşturmaz. Bu sınırlama nedeniyle, geçici ağ veya sunucu sorunları olduğunda telemetri kaybolur. Bu sorunu çözmek için kanal için yerel bir klasör yapılandırır:

```csharp
using Microsoft.ApplicationInsights.Channel;
using Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel;

    public void ConfigureServices(IServiceCollection services)
    {
        // The following will configure the channel to use the given folder to temporarily
        // store telemetry items during network or Application Insights server issues.
        // User should ensure that the given folder already exists
        // and that the application has read/write permissions.
        services.AddSingleton(typeof(ITelemetryChannel),
                                new ServerTelemetryChannel () {StorageFolder = "/tmp/myfolder"});
        services.AddApplicationInsightsTelemetry();
    }
```

### <a name="is-this-sdk-supported-for-the-new-net-core-3x-worker-service-template-applications"></a>Bu SDK yeni .NET Core 3.X İşçi Hizmeti şablonu uygulamaları için desteklendi mi?

Bu SDK `HttpContext`gerektirir ve bu nedenle .NET Core 3.X İşçi Hizmeti uygulamaları da dahil olmak üzere herhangi bir HTTP dışı uygulamalarda çalışmaz. Yeni yayımlanan Microsoft.ApplicationInsights.WorkerService SDK'yı kullanarak bu tür uygulamalarda uygulama öngörülerini etkinleştirmek için [bu](worker-service.md) belgeye bakın.

## <a name="open-source-sdk"></a>Açık kaynak SDK

[Kodu okuyun ve katkıda bulunun.](https://github.com/microsoft/ApplicationInsights-dotnet#recent-updates)

## <a name="video"></a>Video

- [Application Insights'ı .NET Core ve Visual Studio ile](https://www.youtube.com/watch?v=NoS9UhcR4gA&t) sıfırdan yapılandırmak için bu harici adım adım videoya göz atın.
- [Application Insights'ı .NET Core ve Visual Studio Code ile](https://youtu.be/ygGt84GDync) sıfırdan yapılandırmak için bu harici adım adım videoya göz atın.

## <a name="next-steps"></a>Sonraki adımlar

* Kullanıcıların uygulamanızda nasıl gezinenleri anlamak için [kullanıcı akışlarını keşfedin.](../../azure-monitor/app/usage-flows.md)
* Bir özel durum atıldığı anda kaynak kodun ve değişkenlerin durumunu görmek için [anlık görüntü koleksiyonunu yapılandırın.](https://docs.microsoft.com/azure/application-insights/app-insights-snapshot-debugger)
* Uygulamanızın performansı nın ve kullanımının ayrıntılı bir görünümü için kendi etkinliklerinizi ve ölçümlerinizi göndermek için [API'yi kullanın.](../../azure-monitor/app/api-custom-events-metrics.md)
* Uygulamanızı dünyanın dört bir yanından sürekli olarak kontrol etmek için [kullanılabilirlik testlerini](../../azure-monitor/app/monitor-web-app-availability.md) kullanın.
* [ASP.NET Çekirdekte Bağımlılık Enjeksiyonu](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection)
