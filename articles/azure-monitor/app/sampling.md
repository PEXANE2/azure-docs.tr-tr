---
title: Azure Uygulama Öngörülerinde Telemetri örneklemesi | Microsoft Dokümanlar
description: Telemetri hacminin nasıl kontrol altında tutulması.
ms.topic: conceptual
ms.date: 01/17/2020
ms.reviewer: vitalyg
ms.custom: fasttrack-edit
ms.openlocfilehash: fc9db23f7733f97ca207e834d4543fbdb1b9db5c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79275834"
---
# <a name="sampling-in-application-insights"></a>Application Insights’ta örnekleme

Örnekleme, Azure [Uygulama Öngörüleri'nde](../../azure-monitor/app/app-insights-overview.md)bir özelliktir. Uygulama verilerinin istatistiksel olarak doğru analizini korurken, telemetri trafiğini, veri maliyetlerini ve depolama maliyetlerini azaltmanın önerilen yoludur. Örnekleme, uygulama öngörülerinin telemetrinizi azaltması konusunda da yardımcı olur. Örnekleme filtresi, tanılama incelemeleri yaparken öğeler arasında gezinmek için ilgili öğeleri seçer.

Portalda metrik sayımlar sunulduğunda, örneklemedikkate alınarak yeniden normale dönüştürülr. Bunu yapmak istatistikler üzerindeki herhangi bir etkiyi en aza indirir.

## <a name="brief-summary"></a>Kısa özet

* Üç farklı örnekleme türü vardır: uyarlanabilir örnekleme, sabit oranlı örnekleme ve yutma örneklemesi.
* Uyarlanabilir örnekleme, Uygulama Öngörüleri ASP.NET ve ASP.NET Temel Yazılım Geliştirme Kitleri'nin (SDK) en son sürümlerinde varsayılan olarak etkinleştirilir. Ayrıca [Azure Fonksiyonları](https://docs.microsoft.com/azure/azure-functions/functions-overview)tarafından kullanılır.
* Sabit oranlı örnekleme, ASP.NET, ASP.NET Core, Java ve Python için Application Insights SDK'larının son sürümlerinde kullanılabilir.
* Yutma örneklemesi Application Insights hizmet bitiş noktasında çalışır. Yalnızca başka bir örnekleme geçerli olmadığında geçerlidir. SDK telemetrinizi örnekalırsa, yutma örneklemesi devre dışı bırakılır.
* Web uygulamaları için, özel olayları günlüğe kaydederseniz ve bir dizi olayın birlikte tutulduğundan veya atıldığından emin olmanız gerekiyorsa, olayların aynı `OperationId` değere sahip olması gerekir.
* Analytics sorguları yazarsanız, [örneklemeyi dikkate almalısınız.](../../azure-monitor/log-query/aggregations.md) Özellikle, kayıtları saymak yerine kullanmalısınız. `summarize sum(itemCount)`
* Performans ölçümleri ve özel ölçümler de dahil olmak üzere bazı telemetri türleri, örneklemenin etkin olup olmadığına bakılmaksızın her zaman tutulur.

Aşağıdaki tablo, her SDK için kullanılabilir örnekleme türlerini ve uygulama türünü özetleyerek:

| Uygulama Öngörüleri SDK | Adaptif örnekleme destekli | Sabit oranlı örnekleme destekli | Yutma örneklemesi desteklendi |
|-|-|-|-|
| ASP.NET | [Evet (varsayılan olarak)](#configuring-adaptive-sampling-for-aspnet-applications) | [Evet](#configuring-fixed-rate-sampling-for-aspnet-applications) | Yalnızca başka bir örnekleme etkin değilse |
| ASP.NET Core | [Evet (varsayılan olarak)](#configuring-adaptive-sampling-for-aspnet-core-applications) | [Evet](#configuring-fixed-rate-sampling-for-aspnet-core-applications) | Yalnızca başka bir örnekleme etkin değilse |
| Azure İşlevleri | [Evet (varsayılan olarak)](#configuring-adaptive-sampling-for-azure-functions) | Hayır | Yalnızca başka bir örnekleme etkin değilse |
| Java | Hayır | [Evet](#configuring-fixed-rate-sampling-for-java-applications) | Yalnızca başka bir örnekleme etkin değilse |
| Python | Hayır | [Evet](#configuring-fixed-rate-sampling-for-opencensus-python-applications) | Yalnızca başka bir örnekleme etkin değilse |
| Diğer tüm | Hayır | Hayır | [Evet](#ingestion-sampling) |

> [!NOTE]
> Bu sayfanın çoğundaki bilgiler, Application Insights SDK'larının geçerli sürümleri için geçerlidir. SDK'ların eski sürümleri hakkında daha fazla bilgi için [aşağıdaki bölüme bakın.](#older-sdk-versions)

## <a name="types-of-sampling"></a>Örnekleme türleri

Üç farklı örnekleme yöntemi vardır:

* **Uyarlanabilir örnekleme,** ASP.NET/ASP.NET Core uygulamanızda ve Azure İşlerinden SDK'dan gönderilen telemetri nin hacmini otomatik olarak ayarlar. Bu, ASP.NET veya ASP.NET Core SDK kullandığınızda varsayılan örneklemedir. Uyarlamalı örnekleme şu anda yalnızca sunucu tarafındaki ASP.NET telemetri ve Azure İşlevler için kullanılabilir.

* **Sabit oranlı örnekleme,** hem ASP.NET hem de ASP.NET Core veya Java sunucunuzdan ve kullanıcılarınızın tarayıcılarından gönderilen telemetri hacmini azaltır. Fiyatı sen belirlersin. İstemci ve sunucu örneklemelerini eşitler, böylece Arama'da ilgili sayfa görünümleri ve istekleri arasında gezinebilirsiniz.

* **Yutma örneklemesi** Application Insights hizmet bitiş noktasında gerçekleşir. Uygulamanızdan gelen telemetrilerin bir kısmını, ayarladığınız bir örnekleme hızında atar. Uygulamanızdan gönderilen telemetri trafiğini azaltmaz, ancak aylık kotanızı aşmanıza yardımcı olur. Yutma örneklemesinin en büyük avantajı, uygulamanızı yeniden dağıtmadan örnekleme oranını belirleyebilmenizdir. Yutma örneklemesi tüm sunucular ve istemciler için eşit şekilde çalışır, ancak başka örnekleme türleri çalışırken geçerli değildir.

> [!IMPORTANT]
> Uyarlanabilir veya sabit oranlı örnekleme yöntemleri işletiliyorsa, yutma örneklemesi devre dışı bırakılır.

## <a name="adaptive-sampling"></a>Adaptif örnekleme

Uyarlanabilir örnekleme, web sunucu uygulamanızdan Application Insights hizmet bitiş noktasına gönderilen telemetri hacmini etkiler.

> [!TIP]
> Uyarlanabilir örnekleme, sdk veya ASP.NET Core SDK ASP.NET kullandığınızda varsayılan olarak etkinleştirilir ve Azure İşlevler için varsayılan olarak da etkinleştirilir.

Birim, belirtilen maksimum trafik hızında tutmak için otomatik olarak ayarlanır ve `MaxTelemetryItemsPerSecond`ayar üzerinden denetlenir. Uygulama hata ayıklama veya düşük kullanım nedeniyle gibi düşük miktarda telemetri üretirse, birim altında `MaxTelemetryItemsPerSecond`olduğu sürece öğeler örnekleme işlemcisi tarafından düşürülmez. Telemetri nin hacmi arttıkça, örnekleme hızı hedef hacme ulaşacak şekilde ayarlanır. Ayarlama düzenli aralıklarla yeniden hesaplanır ve giden aktarım hızının hareketli ortalamasına dayanır.

Hedef hacmi elde etmek için, oluşturulan telemetri bazı atılır. Ancak diğer örnekleme türleri gibi algoritma da ilgili telemetri öğelerini saklar. Örneğin, Arama'da telemetriyi incelerken, belirli bir özel durumla ilgili isteği bulabilirsiniz.

İstek oranı ve özel durum oranı gibi metrik sayımlar, Metrik Gezgin'de yaklaşık olarak doğru değerleri gösterecek şekilde örnekleme oranını telafi etmek üzere ayarlanır.

### <a name="configuring-adaptive-sampling-for-aspnet-applications"></a>ASP.NET uygulamalar için uyarlanabilir örneklemenin yapılandırılması

> [!NOTE]
> Bu bölüm, ASP.NET Çekirdek uygulamaları için değil, ASP.NET uygulamalar için geçerlidir. [ASP.NET Core uygulamaları için uyarlamalı örneklemeyi daha sonra bu belgede yapılandırma hakkında bilgi edinin.](../../azure-monitor/app/sampling.md#configuring-adaptive-sampling-for-aspnet-core-applications)

Düğümdeki [`ApplicationInsights.config`](../../azure-monitor/app/configuration-with-applicationinsights-config.md)birkaç parametreyi `AdaptiveSamplingTelemetryProcessor` ayarlayabilirsiniz. Gösterilen şekiller varsayılan değerlerdir:

* `<MaxTelemetryItemsPerSecond>5</MaxTelemetryItemsPerSecond>`
  
    Adaptif algoritmanın **her sunucu ana bilgisayarında**hedeflediği hedef oranı. Web uygulamanız birçok ana bilgisayarda çalışıyorsa, Application Insights portalında hedefleneğiniz dahilinde kalmak için bu değeri azaltın.

* `<EvaluationInterval>00:00:15</EvaluationInterval>` 
  
    Geçerli telemetri hızının yeniden değerlendirildiği aralık. Değerlendirme hareketli bir ortalama olarak gerçekleştirilir. Telemetriniz ani patlamalara karşı sorumluysa bu aralığı kısaltmak isteyebilirsiniz.

* `<SamplingPercentageDecreaseTimeout>00:02:00</SamplingPercentageDecreaseTimeout>`
  
    Örnekleme yüzdesi değeri değiştiğinde, daha az veri yakalamak için örnekleme yüzdesini ne kadar kısa sürede düşürmemize izin verilir?

* `<SamplingPercentageIncreaseTimeout>00:15:00</SamplingPercentageIncreaseTimeout>`
  
    Örnekleme yüzdesi değeri değiştiğinde, daha fazla veri yakalamak için örnekleme yüzdesini tekrar artırmamıza ne kadar zaman sonra izin verilir?

* `<MinSamplingPercentage>0.1</MinSamplingPercentage>`
  
    Örnekleme yüzdesi değiştiğinden, ayarlamamıza izin verilen minimum değer nedir?

* `<MaxSamplingPercentage>100.0</MaxSamplingPercentage>`
  
    Örnekleme yüzdesi değiştiğinden, ayarlamamıza izin verilen maksimum değer nedir?

* `<MovingAverageRatio>0.25</MovingAverageRatio>` 
  
    Hareketli ortalamanın hesaplanmasında, bu en son değere atanması gereken ağırlığı belirtir. 1'e eşit veya daha az bir değer kullanın. Daha küçük değerler algoritmayı ani değişikliklere karşı daha az tepkiverir.

* `<InitialSamplingPercentage>100</InitialSamplingPercentage>`
  
    Uygulama yeni başladığında örnek almak için telemetri miktarı. Hata ayıklarken bu değeri düşürmeyin.

* `<ExcludedTypes>Trace;Exception</ExcludedTypes>`
  
    Örneklemetabi olmak istemediğiniz türlerin yarı-kolon delimited listesi. Tanınan türleri `Dependency`şunlardır: `PageView` `Request`, `Trace` `Event`, `Exception`, , , . Belirtilen tiplerin tüm telemetrileri iletilir; belirtilmeyen türler örneklenecektir.

* `<IncludedTypes>Request;Dependency</IncludedTypes>`
  
    Örnekleme tabi istediğiniz türlerin yarı-kolon delimited listesi. Tanınan türleri `Dependency`şunlardır: `PageView` `Request`, `Trace` `Event`, `Exception`, , , . Belirtilen türler örneklenecektir; diğer türlerin tüm telemetriher zaman iletilir.

Uyarlanabilir örneklemeyi **kapatmak için** `AdaptiveSamplingTelemetryProcessor` düğüm(ler)'i `ApplicationInsights.config`kaldırın.

#### <a name="alternative-configure-adaptive-sampling-in-code"></a>Alternatif: Kodda uyarlamalı örneklemeyi yapılandırma

Dosyadaki örnekleme parametresini `.config` ayarlamak yerine, bu değerleri programlı olarak ayarlayabilirsiniz.

1. Dosyadaki `AdaptiveSamplingTelemetryProcessor` tüm düğüm(ler)i `.config` kaldırın.
2. Uyarlanabilir örneklemeyi yapılandırmak için aşağıdaki snippet'i kullanın:

    ```csharp
    using Microsoft.ApplicationInsights;
    using Microsoft.ApplicationInsights.Extensibility;
    using Microsoft.ApplicationInsights.WindowsServer.Channel.Implementation;
    using Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel;
    
    // ...

    var builder = TelemetryConfiguration.Active.DefaultTelemetrySink.TelemetryProcessorChainBuilder;
    // For older versions of the Application Insights SDK, use the following line instead:
    // var builder = TelemetryConfiguration.Active.TelemetryProcessorChainBuilder;

    // Enable AdaptiveSampling so as to keep overall telemetry volume to 5 items per second.
    builder.UseAdaptiveSampling(maxTelemetryItemsPerSecond:5);

    // If you have other telemetry processors:
    builder.Use((next) => new AnotherProcessor(next));

    builder.Build();
    ```

    ([Telemetri işlemcileri hakkında bilgi edinin](../../azure-monitor/app/api-filtering-sampling.md#filtering).)

Ayrıca, her telemetri türü için örnekleme oranını ayrı ayrı ayarlayabilir veya belirli türleri örneklenmekten bile dışlayabilirsiniz:

```csharp
// The following configures adaptive sampling with 5 items per second, and also excludes Dependency telemetry from being subjected to sampling.
builder.UseAdaptiveSampling(maxTelemetryItemsPerSecond:5, excludedTypes: "Dependency");
```

### <a name="configuring-adaptive-sampling-for-aspnet-core-applications"></a>ASP.NET Core uygulamaları için uyarlanabilir örneklemenin yapılandırılması

ASP.NET Core `ApplicationInsights.config` uygulamaları için yoktur, bu nedenle tüm yapılandırma kod üzerinden yapılır.
Tüm ASP.NET Core uygulamaları için varsayılan olarak uyarlanabilir örnekleme etkinleştirilir. Örnekleme davranışını devre dışı layabilir veya özelleştirebilirsiniz.

#### <a name="turning-off-adaptive-sampling"></a>Uyarlanabilir örneklemeyi kapatma

Varsayılan örnekleme özelliği, `ConfigureServices` `ApplicationInsightsServiceOptions` `Startup.cs` dosya içinde kullanarak yöntemde Application Insights hizmetini eklerken devre dışı bilebilir:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // ...

    var aiOptions = new Microsoft.ApplicationInsights.AspNetCore.Extensions.ApplicationInsightsServiceOptions();
    aiOptions.EnableAdaptiveSampling = false;
    services.AddApplicationInsightsTelemetry(aiOptions);

    //...
}
```

Yukarıdaki kod uyarlanabilir örneklemeyi devre dışı kalacaktır. Daha fazla özelleştirme seçeneğiyle örnekleme eklemek için aşağıdaki adımları izleyin.

#### <a name="configure-sampling-settings"></a>Örnekleme ayarlarını yapılandırma

Örnekleme davranışını `TelemetryProcessorChainBuilder` özelleştirmek için aşağıda gösterildiği gibi uzantı yöntemlerini kullanın.

> [!IMPORTANT]
> Örneklemeyi yapılandırmak için bu yöntemi kullanıyorsanız, `aiOptions.EnableAdaptiveSampling` lütfen `false` aramayı `AddApplicationInsightsTelemetry()`yaparken özelliği ni ayarlamak için emin olun.

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env, TelemetryConfiguration configuration)
{
    var builder = configuration.DefaultTelemetrySink.TelemetryProcessorChainBuilder;
    // For older versions of the Application Insights SDK, use the following line instead:
    // var builder = configuration.TelemetryProcessorChainBuilder;

    // Using adaptive sampling
    builder.UseAdaptiveSampling(maxTelemetryItemsPerSecond:5);

    // Alternately, the following configures adaptive sampling with 5 items per second, and also excludes DependencyTelemetry from being subject to sampling.
    // builder.UseAdaptiveSampling(maxTelemetryItemsPerSecond:5, excludedTypes: "Dependency");

    // If you have other telemetry processors:
    builder.Use((next) => new AnotherProcessor(next));
    
    builder.Build();

    // ...
}
```

### <a name="configuring-adaptive-sampling-for-azure-functions"></a>Azure İşlevleri için uyarlamalı örnekleme yapılandırma

Azure İşlevlerinde çalışan uygulamalar için uyarlanabilir örneklemeyi yapılandırmak için [bu sayfadaki](https://docs.microsoft.com/azure/azure-functions/functions-monitoring#configure-sampling) yönergeleri izleyin.

## <a name="fixed-rate-sampling"></a>Sabit oranlı örnekleme

Sabit oranlı örnekleme, web sunucunuzdan ve web tarayıcılarınızdan gönderilen trafiği azaltır. Adaptif örneklemenin aksine, telemetriyi sizin karar verilebildiğiniz sabit bir oranda azaltır. ASP.NET, ASP.NET Core, Java ve Python uygulamaları için sabit oranlı örnekleme mevcuttur.

Diğer örnekleme teknikleri gibi, bu da ilgili öğeleri korur. Ayrıca, ilgili öğelerin korunması için istemci ve sunucu örneklemesini eşitler ( örneğin, Arama'da bir sayfa görünümüne baktığınızda, ilgili sunucu isteklerini bulabilirsiniz. 

Metrics Explorer'da, istek ve özel durum sayıları gibi oranlar, örnekleme oranını telafi etmek için bir faktörle çarpılır, böylece yaklaşık olarak doğru olurlar.

### <a name="configuring-fixed-rate-sampling-for-aspnet-applications"></a>ASP.NET uygulamalar için sabit oranlı örneklemenin yapılandırılması

1. **Uyarlamalı örneklemeyi devre**dışı [`ApplicationInsights.config`](../../azure-monitor/app/configuration-with-applicationinsights-config.md)bırakın : Düğümü `AdaptiveSamplingTelemetryProcessor` çıkarın veya çıkarın.

    ```xml
    <TelemetryProcessors>
        <!-- Disabled adaptive sampling:
        <Add Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.AdaptiveSamplingTelemetryProcessor, Microsoft.AI.ServerTelemetryChannel">
            <MaxTelemetryItemsPerSecond>5</MaxTelemetryItemsPerSecond>
        </Add>
        -->
    ```

2. **Sabit oranlı örnekleme modüllerini etkinleştirin.** Bu parçacığı [`ApplicationInsights.config`](../../azure-monitor/app/configuration-with-applicationinsights-config.md)ekleyin:
   
    ```XML
    <TelemetryProcessors>
        <Add Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.SamplingTelemetryProcessor, Microsoft.AI.ServerTelemetryChannel">
            <!-- Set a percentage close to 100/N where N is an integer. -->
            <!-- E.g. 50 (=100/2), 33.33 (=100/3), 25 (=100/4), 20, 1 (=100/100), 0.1 (=100/1000) -->
            <SamplingPercentage>10</SamplingPercentage>
        </Add>
    </TelemetryProcessors>
    ```

      Alternatif olarak, dosyadaki örnekleme parametresini `ApplicationInsights.config` ayarlamak yerine, şu değerleri programlı olarak ayarlayabilirsiniz:

    ```csharp
    using Microsoft.ApplicationInsights.Extensibility;
    using Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel;

    // ...

    var builder = TelemetryConfiguration.Active.DefaultTelemetrySink.TelemetryProcessorChainBuilder;
    // For older versions of the Application Insights SDK, use the following line instead:
    // var builder = TelemetryConfiguration.Active.TelemetryProcessorChainBuilder;

    builder.UseSampling(10.0); // percentage

    // If you have other telemetry processors:
    builder.Use((next) => new AnotherProcessor(next));

    builder.Build();
    ```

    ([Telemetri işlemcileri hakkında bilgi edinin](../../azure-monitor/app/api-filtering-sampling.md#filtering).)

### <a name="configuring-fixed-rate-sampling-for-aspnet-core-applications"></a>ASP.NET Core uygulamaları için sabit oranlı örneklemenin yapılandırılması

1. **Adaptif örneklemeyi devre dışı alma**: `ConfigureServices` Yöntemde `ApplicationInsightsServiceOptions`değişiklikler yapılabilir, aşağıdakiler kullanılarak:

    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        // ...

        var aiOptions = new Microsoft.ApplicationInsights.AspNetCore.Extensions.ApplicationInsightsServiceOptions();
        aiOptions.EnableAdaptiveSampling = false;
        services.AddApplicationInsightsTelemetry(aiOptions);

        //...
    }
    ```

2. **Sabit oranlı örnekleme modüllerini etkinleştirin.** Aşağıdaki snippet'te gösterildiği gibi `Configure` yöntemde değişiklikler yapılabilir:

    ```csharp
    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
        var configuration = app.ApplicationServices.GetService<TelemetryConfiguration>();

        var builder = configuration.DefaultTelemetrySink.TelemetryProcessorChainBuilder;
        // For older versions of the Application Insights SDK, use the following line instead:
        // var builder = TelemetryConfiguration.Active.TelemetryProcessorChainBuilder;

        // Using fixed rate sampling   
        double fixedSamplingPercentage = 10;
        builder.UseSampling(fixedSamplingPercentage);

        builder.Build();

        // ...
    }
    ```

### <a name="configuring-fixed-rate-sampling-for-java-applications"></a>Java uygulamaları için sabit oranlı örneklemenin yapılandırılması

Varsayılan olarak Java SDK'da örnekleme etkin değildir. Şu anda yalnızca sabit oranlı örneklemeyi destekler. Java SDK'da uyarlanabilir örnekleme desteklenmez.

1. Web uygulamanızı en son [Application Insights Java SDK](../../azure-monitor/app/java-get-started.md)ile indirin ve yapılandırın.

2. Dosyaya aşağıdaki snippet ekleyerek **sabit oranlı örnekleme modüllerini etkinleştirin:** `ApplicationInsights.xml`

    ```XML
    <TelemetryProcessors>
        <BuiltInProcessors>
            <Processor type="FixedRateSamplingTelemetryProcessor">
                <!-- Set a percentage close to 100/N where N is an integer. -->
                <!-- E.g. 50 (=100/2), 33.33 (=100/3), 25 (=100/4), 20, 1 (=100/100), 0.1 (=100/1000) -->
                <Add name="SamplingPercentage" value="50" />
            </Processor>
        </BuiltInProcessors>
    </TelemetryProcessors>
    ```

3. `Processor` Etiketin içindeki aşağıdaki etiketleri kullanarak belirli telemetri türlerini örneklemeden ekleyebilir veya hariç `FixedRateSamplingTelemetryProcessor`tutabilirsiniz:
   
    ```XML
    <ExcludedTypes>
        <ExcludedType>Request</ExcludedType>
    </ExcludedTypes>

    <IncludedTypes>
        <IncludedType>Exception</IncludedType>
    </IncludedTypes>
    ```

Örnekleme dışında tutulabilen veya hariç tutulabilen telemetri `Exception` `PageView`türleri `Request`şunlardır: `Trace` `Dependency`, , `Event`, , ve .

> [!NOTE]
> Örnekleme yüzdesi için, N'nin tamsayı olduğu 100/N'ye yakın bir yüzde seçin.  Şu anda örnekleme diğer değerleri desteklemez.

### <a name="configuring-fixed-rate-sampling-for-opencensus-python-applications"></a>OpenCensus Python uygulamaları için sabit oranlı örneklemenin yapılandırılması

En son [OpenCensus Azure Monitor ihracatçıları](../../azure-monitor/app/opencensus-python.md)ile uygulamanızı enstrümanlayın.

> [!NOTE]
> Metrik ihracatçısı için sabit oranlı örnekleme kullanılamaz. Bu, özel ölçümlerin, örneklemenin yapılandırılabildiği tek telemetri türleri olduğu anlamına gelir. Ölçümler ihracatçısı izleyen tüm telemetri göndereceğiz.

#### <a name="fixed-rate-sampling-for-tracing"></a>İzleme için sabit oranlı örnekleme ####
`Tracer` yapılandırmanız kapsamında bir `sampler` belirtmelisiniz. Açık bir örnekleyici sağlanmazsa, varsayılan `ProbabilitySampler` olarak kullanılır. Varsayılan `ProbabilitySampler` olarak 1/10000 oranında bir oran kullanılacak, yani her 10000 istekten biri Application Insights'a gönderilecektir. Örnekleme oranı belirtmek isterseniz aşağıya bakın.

Örnekleme oranını belirtmek için, `Tracer` örnekleme oranı 0,0 ile 1,0 dahil arasında olan bir örnekleyici belirttiğinden emin olun. 1,0 örnekleme oranı %100'ü temsil eder, yani tüm istekleriniz Uygulama Öngörüleri'ne telemetri olarak gönderilir.

```python
tracer = Tracer(
    exporter=AzureExporter(
        instrumentation_key='00000000-0000-0000-0000-000000000000',
    ),
    sampler=ProbabilitySampler(1.0),
)
```

#### <a name="fixed-rate-sampling-for-logs"></a>Günlükler için sabit oranlı örnekleme ####
İsteğe bağlı bağımsız değişkeni `AzureLogHandler` değiştirerek sabit oranlı örneklemeyi yapılandırabilirsiniz. `logging_sampling_rate` Bağımsız değişken sağlanmazsa, örnekleme hızı 1.0 olarak kullanılır. 1,0 örnekleme oranı %100'ü temsil eder, yani tüm istekleriniz Uygulama Öngörüleri'ne telemetri olarak gönderilir.

```python
exporter = metrics_exporter.new_metrics_exporter(
    instrumentation_key='00000000-0000-0000-0000-000000000000',
    logging_sampling_rate=0.5,
)
```

### <a name="configuring-fixed-rate-sampling-for-web-pages-with-javascript"></a>JavaScript ile web sayfaları için sabit oranlı örnekleme yapılandırma

JavaScript tabanlı web sayfaları, Application Insights'ı kullanacak şekilde yapılandırılabilir. Telemetri, kullanıcının tarayıcısı içinde çalışan istemci uygulamasından gönderilir ve sayfalar herhangi bir sunucudan barındırılabilir.

Application [Insights için JavaScript tabanlı web sayfalarınızı yapılandırdığınızda,](javascript.md)Application Insights portalından aldığınız JavaScript snippet'ini değiştirin.

> [!TIP]
> JavaScript'in bulunduğu ASP.NET uygulamalarda, parçacık genellikle `_Layout.cshtml`.

Enstrümantasyon tuşu ndan önce olduğu gibi `samplingPercentage: 10,` bir satır ekleyin:

```xml
<script>
    var appInsights = // ... 
    ({ 
      // Value must be 100/N where N is an integer.
      // Valid examples: 50, 25, 20, 10, 5, 1, 0.1, ...
      samplingPercentage: 10, 

      instrumentationKey: ...
    }); 

    window.appInsights = appInsights; 
    appInsights.trackPageView(); 
</script>
```

Örnekleme yüzdesi için, N'nin tamsayı olduğu 100/N'ye yakın bir yüzde seçin. Şu anda örnekleme diğer değerleri desteklemez.

#### <a name="coordinating-server-side-and-client-side-sampling"></a>Sunucu tarafı ve istemci tarafı örneklemeyi koordine etme

İstemci tarafındaki JavaScript SDK, sunucu tarafındaki SDK ile birlikte sabit oranlı örneklemeye katılır. İşletilen sayfalar yalnızca sunucu tarafındaki SDK'nın örneklemeyi ekleme kararını verdiği aynı kullanıcıdan istemci tarafı telemetrisi gönderir. Bu mantık, istemci ve sunucu tarafındaki uygulamalar arasında kullanıcı oturumlarının bütünlüğünü korumak için tasarlanmıştır. Sonuç olarak, Uygulama Öngörüleri'ndeki belirli bir telemetri öğesinden bu kullanıcı veya oturum için diğer tüm telemetri öğelerini bulabilir ve Arama'da, ilgili sayfa görünümleri ve istekleri arasında gezinebilirsiniz.

İstemciniz ve sunucu tarafındaki telemetriniz eşgüdümlü örnekler göstermiyorsa:

* Hem sunucuda hem de istemcide örneklemeyi etkinleştirdiğinizi doğrulayın.
* Hem istemcide hem de sunucuda aynı örnekleme yüzdesini ayarladığınızı denetleyin.
* SDK sürümünün 2.0 veya üzeri olduğundan emin olun.

## <a name="ingestion-sampling"></a>Yutma örneklemesi

Yutma örneklemesi, web sunucunuzdan, tarayıcılarınızdan ve aygıtlarınızdan telemetrinin Application Insights hizmet bitiş noktasına ulaştığı noktada çalışır. Uygulamanızdan gönderilen telemetri trafiğini azaltmasa da, Application Insights tarafından işlenen ve tutulan (ve ücretlendirilen) miktarı azaltır.

Uygulamanız genellikle aylık kotasını aşıyorsa ve SDK tabanlı örnekleme türlerinden birini kullanma seçeneğiniz yoksa bu tür örneklemeleri kullanın. 

Kullanım ve tahmini maliyetler sayfasındaörnekleme oranını ayarlayın:

![Uygulamanın Genel Bakış bıçağından Ayarlar, Kota, Örnekler'i tıklatın, ardından bir örnekleme oranı seçin ve Güncelleştir'i tıklatın.](./media/sampling/data-sampling.png)

Diğer örnekleme türleri gibi algoritma da ilgili telemetri öğelerini saklar. Örneğin, Arama'da telemetriyi incelerken, belirli bir özel durumla ilgili isteği bulabilirsiniz. İstek oranı ve özel durum oranı gibi metrik sayımlar doğru şekilde korunur.

Örnekleme tarafından atılan veri [noktaları, Sürekli Dışa Aktarma](../../azure-monitor/app/export-telemetry.md)gibi herhangi bir Uygulama Öngörüleri özelliğinde kullanılamaz.

Uyarlamalı veya sabit oranlı örnekleme çalışırken yutma örneklemesi çalışmaz. Uyarlanabilir örnekleme, ASP.NET SDK veya ASP.NET Core SDK kullanıldığında veya [Azure Uygulama Hizmeti'nde](azure-web-apps.md) veya Durum İzleyicisi kullanılarak Uygulama Öngörüleri etkinleştirildiğinde varsayılan olarak etkinleştirilir. Telemetri, Application Insights hizmet bitiş noktası tarafından alındığı zaman, telemetriyi inceler ve örnekleme oranının %100'den az olduğu bildirilirse (bu da telemetrinin örneklendiğini gösterir) sonra ayarladığınız yutma örnekleme hızı göz ardı edilir.

> [!WARNING]
> Portal döşemesinde gösterilen değer, yutma örneklemesi için belirlediğiniz değeri gösterir. Herhangi bir SDK örneklemesi (uyarlanabilir veya sabit oranlı örnekleme) işletilmesi durumunda gerçek örnekleme oranını temsil etmez.

## <a name="when-to-use-sampling"></a>Örnekleme ne zaman kullanılır?

Genel olarak, çoğu küçük ve orta ölçekli uygulamalar için örnekleme gerekmez. En yararlı tanılama bilgileri ve en doğru istatistikler, tüm kullanıcı faaliyetleriniz hakkında veri toplayarak elde edilir. 

Örneklemenin başlıca avantajları şunlardır:

* Uygulama Öngörüleri hizmeti, uygulamanız kısa bir zaman aralığında çok yüksek bir telemetri hızı gönderdiğinde veri puanlarını düşürür ("azaltma"). Örnekleme, uygulamanızın azaltma nın meydana geldiğini görme olasılığını azaltır.
* Fiyatlandırma katmanınız için veri noktaları [kotası](pricing.md) içinde kalmak için. 
* Telemetri koleksiyonundan ağ trafiğini azaltmak için. 

### <a name="which-type-of-sampling-should-i-use"></a>Hangi örnekleme türünü kullanmalıyım?

**Eğer yutma örneklemesi kullanın:**

* Aylık telemetri kotanızı sık sık kullanırsınız.
* Kullanıcılarınızın web tarayıcılarından çok fazla telemetri elde emmmiyorsanız.
* SDK'nın örneklemeyi desteklemeyen bir sürümünü kullanıyorsunuz - örneğin 2'den önceki ASP.NET sürümleri.

**Şu şekilde sabit oranlı örnekleme kullanın:**

* İstemci ve sunucu arasında senkronize örnekleme istiyorsunuz, böylece [Arama'daki](../../azure-monitor/app/diagnostic-search.md)olayları araştırırken, istemci ve sunucudaki sayfa görünümleri ve HTTP istekleri gibi ilgili olaylar arasında gezinebilirsiniz.
* Uygulamanız için uygun örnekleme yüzdesini nizden eminsiniz. Doğru ölçümleri almak için yeterince yüksek olmalıdır, ancak fiyatlandırma kotanızı ve azaltma limitlerini aşan oranın altında olmalıdır.

**Uyarlanabilir örnekleme kullanın:**

Diğer örnekleme biçimlerini kullanma koşulları geçerli değilse, uyarlanabilir örnekleme öneririz. Bu ayar, Core SDK ASP.NET/ASP.NET varsayılan olarak etkinleştirilir. Belirli bir minimum ücrete ulaşılıncaya kadar trafiği azaltmaz, bu nedenle düşük kullanımlı siteler büyük olasılıkla hiç örneklenmemiştir.

## <a name="knowing-whether-sampling-is-in-operation"></a>Örneklemenin çalışıp çalışmadığını bilme

Gerçek örnekleme oranını nerede uygulanırsa uygulansın keşfetmek için aşağıdaki gibi bir [Analytics sorgusu](../../azure-monitor/app/analytics.md) kullanın:

```kusto
union requests,dependencies,pageViews,browserTimings,exceptions,traces
| where timestamp > ago(1d)
| summarize RetainedPercentage = 100/avg(itemCount) by bin(timestamp, 1h), itemType
```

Herhangi bir `RetainedPercentage` tür için 100'den az olduğunu görürseniz, o zaman bu tür bir telemetri örnekleniyor.

> [!IMPORTANT]
> Uygulama Öngörüleri, örnekleme tekniklerinden herhangi birinde oturum, ölçümler (özel ölçümler dahil) veya performans sayacı telemetri semetrisi türlerini örneklemez. Bu tür her zaman hassas bir azalma bu telemetri türleri için son derece istenmeyen olabilir gibi örnekleme dışındadır.

## <a name="how-sampling-works"></a>Örnekleme nasıl çalışır?

Örnekleme algoritması hangi telemetri öğelerinin bırakılalanave hangilerinin saklanacağını belirtir. Örneklemenin SDK tarafından mı yoksa Application Insights hizmetinde mi yapıldığı doğrudur. Örnekleme kararı, birbiriyle ilişkili tüm veri noktalarını sağlam bir şekilde korumayı amaçlayan ve uygulama öngörüleri içinde daha az veri seti ile bile işlem yapılabilir ve güvenilir bir tanılama deneyimi sağlamayı amaçlayan çeşitli kurallara dayanır. Örneğin, uygulamanızın bir örnekte yer alan başarısız bir isteği varsa, ek telemetri öğeleri (bu istek için günlüğe kaydedilmiş özel durum ve izlemeler gibi) korunur. Örnekleme ya tutar ya da hepsini bir araya düşürür. Sonuç olarak, Uygulama Öngörüleri'ndeki istek ayrıntılarına baktığınızda, isteği ilişkili telemetri öğeleriyle birlikte her zaman görebilirsiniz.

Örnekleme kararı, isteğin çalışma kimliğine dayanır, bu da belirli bir işletmeye ait tüm telemetri öğelerinin korunduğu veya bırakıldığı anlamına gelir. İşlem kimliği kümesi olmayan telemetri öğeleri için (http bağlamı olmayan eşzamanlı iş parçacığından bildirilen telemetri öğeleri gibi) örnekleme yalnızca her türdeki telemetri öğelerinin bir yüzdesini yakalar.

Telemetriyi size geri sunarken, Application Insights hizmeti, eksik veri noktalarını telafi etmek için ölçümleri toplama sırasında kullanılan aynı örnekleme yüzdesine göre ayarlar. Bu nedenle, Uygulama Öngörüleri'ndeki telemetriye bakarken, kullanıcılar gerçek sayılara çok yakın istatistiksel olarak doğru yaklaşımlar görürler.

Yaklaşık doğruluğu büyük ölçüde yapılandırılan örnekleme yüzdesi bağlıdır. Ayrıca, çok sayıda kullanıcıdan gelen genel olarak benzer istekleri niçin büyük bir hacimde işleyen uygulamaların doğruluğu artar. Diğer taraftan, önemli bir yük ile çalışmayan uygulamalar için, bu uygulamalar genellikle kota içinde kalırken, daralma veri kaybına neden olmadan tüm telemetri gönderebilirsiniz gibi örnekleme gerekli değildir. 

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

*Core SDK'lar ASP.NET ve ASP.NET varsayılan örnekleme davranışı nedir?*

* Yukarıdaki SDK'nın en son sürümlerinden birini kullanıyorsanız, Uyarlanabilir Örnekleme varsayılan olarak saniyede beş telemetri öğesi ile etkinleştirilir.
  Varsayılan olarak `AdaptiveSamplingTelemetryProcessor` eklenen iki düğüm vardır ve `Event` biri örnekleme türünü içerirken, `Event` diğeri türü örneklemeden dışlar. Bu yapılandırma, SDK'nın telemetri öğelerini beş telemetri `Event` öğesiyle ve diğer tüm türlerdeki beş telemetri `Events` öğesiyle sınırlandırmaya çalışacağı ve böylece diğer telemetri türlerinden ayrı olarak örnekalınmasını sağlaması anlamına gelir. Olaylar genellikle iş telemetrisi için kullanılır ve büyük olasılıkla tanısal telemetri hacimleri etkilenmemelidir.
  
  Aşağıda oluşturulan varsayılan `ApplicationInsights.config` dosya gösterilmektedir. ASP.NET Çekirdek'te, aynı varsayılan davranış kod olarak etkinleştirilir. Bu varsayılan davranışı değiştirmek için [bu sayfanın önceki bölümündeki örnekleri](#configuring-adaptive-sampling-for-aspnet-core-applications) kullanın.

    ```xml
    <TelemetryProcessors>
        <Add Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.AdaptiveSamplingTelemetryProcessor, Microsoft.AI.ServerTelemetryChannel">
            <MaxTelemetryItemsPerSecond>5</MaxTelemetryItemsPerSecond>
            <ExcludedTypes>Event</ExcludedTypes>
        </Add>
        <Add Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.AdaptiveSamplingTelemetryProcessor, Microsoft.AI.ServerTelemetryChannel">
            <MaxTelemetryItemsPerSecond>5</MaxTelemetryItemsPerSecond>
            <IncludedTypes>Event</IncludedTypes>
        </Add>
    </TelemetryProcessors>
    ```

*Telemetri den fazla örneklenebilir mi?*

* Hayır. ÖrneklemeTelemetryİşlemler öğe zaten örneklenmişse, örnekleme göz önünde bulundurulması öğelerini yoksalar. Aynı şey, SDK'nın kendisinde örneklenmiş olan bu öğeleriçin örnekleme uygulamayacak olan yutma örneklemesi için de geçerlidir.

*Neden basit bir örnekleme "her telemetri türünden yüzde x toplamak" değil mi?*

* Bu örnekleme yaklaşımı metrik yaklaşımlarda yüksek düzeyde hassasiyet sağlarken, tanılama için kritik öneme sahip kullanıcı, oturum ve istek başına tanılama verilerini ilişkilendirme yeteneğini kırar. Bu nedenle, örnekleme "uygulama kullanıcılarının x yüzdesi için tüm telemetri öğelerini topla" veya "uygulama isteklerinin x yüzdesi için tüm telemetrileri topla" gibi ilkelerle daha iyi çalışır. İsteklerle ilişkili olmayan telemetri öğeleri için (arka plan asynchronous işleme gibi), geri dönüş "her telemetri türü için tüm öğelerin X yüzdesini toplamaktır." 

*Örnekleme yüzdesi zaman içinde değişebilir mi?*

* Evet, uyarlanabilir örnekleme, telemetrinin şu anda gözlenen hacmine bağlı olarak örnekleme yüzdesini kademeli olarak değiştirir.

*Sabit oranlı örnekleme kullanırsam, uygulamam için hangi örnekleme yüzdesinin en iyi şekilde çalışacağını nasıl bilebilirim?*

* Bir yolu uyarlanabilir örnekleme ile başlamak için, hangi oranda yerleşir bulmak (yukarıdaki soruya bakın) ve sonra bu oranı kullanarak sabit oranlı örnekleme geçmek. 
  
    Aksi takdirde, tahmin etmek zorunda. Application Insights'taki mevcut telemetri kullanımınızı analiz edin, oluşan herhangi bir daralma gözlemleyin ve toplanan telemetrinin hacmini tahmin edin. Bu üç giriş, seçtiğiniz fiyatlandırma katmanıyla birlikte, toplanan telemetrinin hacmini ne kadar azaltmak isteyebileceğini gösterir. Ancak, kullanıcılarınızın sayısındaki artış veya telemetri hacmindeki başka bir değişiklik tahmininizi geçersiz kılabilecektir.

*Örnekleme yüzdesini çok düşük olacak şekilde yapılandırsam ne olur?*

* Aşırı düşük örnekleme yüzdeleri aşırı agresif örneklemeye neden olur ve Application Insights veri hacmi azaltma için verilerin görselleştirilmesini dengelemeye çalıştığında yaklaşıkların doğruluğunu azaltır. Ayrıca, seyrek olarak başarısız olan veya yavaş isteklerinden bazıları örneklenebileceğinden, tanılama deneyiminiz de olumsuz etkilenebilir.

*Örnekleme yüzdesini çok yüksek olacak şekilde yapılandırsam ne olur?*

* Çok yüksek bir örnekleme yüzdesinin (yeterince agresif olmayan) yapılandırılması, toplanan telemetrinin hacminde yetersiz bir azalmaya neden olur. Azaltmayla ilgili telemetri veri kaybı yaşamaya devam edebilirsiniz ve Uygulama Öngörüleri'ni kullanmanın maliyeti, fazla ücretlendirme nedeniyle planladığınuzun üzerinde olabilir.

*Örneklemeyi hangi platformlarda kullanabilirim?*

* SDK örnekleme gerçekleştirmiyorsa, belirli bir birimin üzerindeki herhangi bir telemetri için otomatik olarak yutma örneklemesi oluşabilir. Bu yapılandırma, örneğin SDK veya Java SDK ASP.NET eski bir sürümünü kullanıyorsanız çalışır.
* Geçerli ASP.NET veya ASP.NET Core SDK'ları (Azure'da veya kendi sunucunuzda barındırılan) kullanıyorsanız, varsayılan olarak uyarlanabilir örnekleme alırsınız, ancak yukarıda açıklandığı gibi sabit oranlı olarak geçiş yapabilirsiniz. Sabit oranlı örnekleme ile tarayıcı SDK, ilgili örneklerle otomatik olarak senkronize olur. 
* Geçerli Java SDK'sını kullanıyorsanız, sabit `ApplicationInsights.xml` oranlı örneklemeyi açmak için yapılandırabilirsiniz. Örnekleme varsayılan olarak kapatılır. Sabit oranlı örnekleme ile tarayıcı SDK ve sunucu, ilgili örneklerle otomatik olarak senkronize olur.

*Her zaman görmek istediğim bazı nadir olaylar vardır. Örnekleme modüllerini nasıl geçebilirim?*

* Bunu başarabilmenin en iyi yolu, aşağıda gösterildiği gibi, `SamplingPercentage` muhafaza sını istediğiniz telemetri öğesinde 100'e ayarlayan özel bir [TelemetryInitializer](../../azure-monitor/app/api-filtering-sampling.md#addmodify-properties-itelemetryinitializer)yazmaktır. Başlangıçlayıcıların telemetri işlemcilerinden önce çalıştırılacağı garanti olduğundan (örnekleme dahil), bu, tüm örnekleme tekniklerinin bu öğeyi örnekleme hususlarından yok saymasını sağlar. Özel telemetri başlatma layıcıları SDK, ASP.NET Core SDK, JavaScript SDK ve Java SDK ASP.NET mevcuttur. Örneğin, sdk ASP.NET kullanarak bir telemetri başharfi yapılandırabilirsiniz:

    ```csharp
    public class MyTelemetryInitializer : ITelemetryInitializer
    {
        public void Initialize(ITelemetry telemetry)
        {
            if(somecondition)
            {
                ((ISupportSampling)telemetry).SamplingPercentage = 100;
            }
        }
    }
    ```

## <a name="older-sdk-versions"></a>Eski SDK sürümleri

Adaptif örnekleme, ASP.NET v2.0.0-beta3 ve daha sonra, Microsoft.ApplicationInsights.AspNetCore SDK v2.2.0-beta1 ve sonrası için Uygulama Öngörüleri SDK için kullanılabilir ve varsayılan olarak etkinleştirilir.

Sabit oranlı örnekleme, SDK'nın 2.0.0 ve Java SDK sürüm 2.0.1 ve sonraki sürümlerinden ASP.NET sürümlerinde bir özelliğidir.

ASP.NET SDK v2.5.0-beta2 ve ASP.NET Core SDK v2.2.0-beta3 önce, örnekleme kararı "kullanıcı" (yani, en tipik web uygulamaları) tanımlayan uygulamalar için kullanıcı kimliği karma dayanmaktadır. Kullanıcıları tanımlamayan uygulama türleri (web hizmetleri gibi) örnekleme kararı isteğin çalışma kimliğine dayanıyordu. ASP.NET ve ASP.NET Core SDK'ların son sürümleri, örnekleme kararı için işlem kimliğini kullanır.

## <a name="next-steps"></a>Sonraki adımlar

* [Filtreleme,](../../azure-monitor/app/api-filtering-sampling.md) SDK'nızın ne gönderdiği üzerinde daha sıkı denetim sağlayabilir.
* [Uygulama Öngörüleri ile](https://msdn.microsoft.com/magazine/mt808502.aspx)Geliştirici Ağı makalesini Optimize Etme makalesini okuyun.
