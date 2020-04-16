---
title: Azure Uygulama Öngörüleri SDK'da filtreleme ve ön işleme | Microsoft Dokümanlar
description: Telemetri Uygulama Öngörüleri portalına gönderilmeden önce verilere özellikleri filtrelemek veya eklemek için SDK'ya Telemetri İşlemcileri ve Telemetri Başharfleri yazın.
ms.topic: conceptual
ms.date: 11/23/2016
ms.openlocfilehash: 8b81849726ad546a24ce1bb56a139b384eb54c42
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81405371"
---
# <a name="filtering-and-preprocessing-telemetry-in-the-application-insights-sdk"></a>Uygulama Öngörüleri SDK'da filtreleme ve ön işleme telemetrisi

Uygulama Öngörüleri hizmetine gönderilmeden önce telemetrinin nasıl zenginleştirilip işlenebileceğini özelleştirmek için Application Insights SDK için eklentiler yazabilir ve yapılandırabilirsiniz.

* [Örnekleme,](sampling.md) istatistiklerinizi etkilemeden telemetri hacmini azaltır. Bir sorunu tanılarken aralarında gezinmek için ilgili veri noktalarını bir arada tutar. Portalda, örneklemeyi telafi etmek için toplam sayımlar çarpılır.
* Telemetri İşlemcileri ile filtreleme, sunucuya gönderilmeden önce SDK'daki telemetriyi filtrelemenizi sağlar. Örneğin, robotlardan gelen istekleri hariç telemetri hacmini azaltabilirsiniz. Filtreleme, trafiği azaltmak için örneklemeden daha temel bir yaklaşımdır. Aktarılanlar üzerinde daha fazla denetim sağlar, ancak istatistiklerinizi etkilediğinin farkında olmanızı sağlar - örneğin, tüm başarılı istekleri filtrelerseniz.
* [Telemetri Initializatörler,](#add-properties) standart modüllerden telemetri de dahil olmak üzere uygulamanızdan gönderilen herhangi bir telemetriye özellikler ekler veya değiştirir. Örneğin, hesaplanan değerler ekleyebilirsiniz; veya portaldaki verileri filtrelemek için sürüm numaraları.
* [SDK API](../../azure-monitor/app/api-custom-events-metrics.md) özel olaylar ve ölçümler göndermek için kullanılır.

Başlamadan önce:

* Uygulamanız için uygun SDK'yı yükleyin: [ASP.NET](asp-net.md), [ASP.NET Core](asp-net-core.md), [Non HTTP/Worker for .NET/.NET Core](worker-service.md)veya [JavaScript](javascript.md)

<a name="filtering"></a>

## <a name="filtering"></a>Filtreleme

Bu teknik, telemetri akışına dahil edilen veya dışlanan lar üzerinde doğrudan kontrol sağlar. Filtreleme, telemetri öğelerini Uygulama Öngörüleri'ne gönderilmekten düşürmek için kullanılabilir. Örnekleme ile birlikte veya ayrı ayrı kullanabilirsiniz.

Telemetriyi filtrelemek için bir telemetri işlemcisi yazar ve `TelemetryConfiguration`.. Tüm telemetri işlemcinizden geçer ve akışından düşürmeyi veya zincirdeki bir sonraki işlemciye vermeyi seçebilirsiniz. Buna, HTTP istek toplayıcısı ve bağımlılık toplayıcısı gibi standart modüllerden gelen telemetri ve kendiniz izlediğiniz telemetri dahildir. Örneğin, robotlardan gelen istekler veya başarılı bağımlılık çağrıları hakkında telemetri filtreleyebilirsiniz.

> [!WARNING]
> İşlemciler kullanarak SDK'dan gönderilen telemetriyi filtrelemek, portalda gördüğünüz istatistikleri çarpıtabilir ve ilgili öğeleri izlemeyi zorlaştırabilir.
>
> Bunun yerine, [örnekleme](../../azure-monitor/app/sampling.md)kullanmayı düşünün.
>
>

### <a name="create-a-telemetry-processor-c"></a>Bir telemetri işlemcisi oluşturma (C#)

1. Bir filtre oluşturmak `ITelemetryProcessor`için uygulayın.

    Telemetri İşlemcileri'nin bir işleme zinciri oluşturmaya dikkat edin. Bir telemetri işlemcisini anında yaptığınızda, zincirdeki bir sonraki işlemciye bir başvuru verilir. Bir telemetri veri noktası İşlem yöntemine geçirildiğinde, işini yapar ve zincirdeki bir sonraki Telemetri İşlemcisi'ni çağırır (veya aramaz).

    ```csharp
    using Microsoft.ApplicationInsights.Channel;
    using Microsoft.ApplicationInsights.Extensibility;

    public class SuccessfulDependencyFilter : ITelemetryProcessor
    {
        private ITelemetryProcessor Next { get; set; }

        // next will point to the next TelemetryProcessor in the chain.
        public SuccessfulDependencyFilter(ITelemetryProcessor next)
        {
            this.Next = next;
        }

        public void Process(ITelemetry item)
        {
            // To filter out an item, return without calling the next processor.
            if (!OKtoSend(item)) { return; }

            this.Next.Process(item);
        }

        // Example: replace with your own criteria.
        private bool OKtoSend (ITelemetry item)
        {
            var dependency = item as DependencyTelemetry;
            if (dependency == null) return true;

            return dependency.Success != true;
        }
    }
    ```

2. İşlemcinizi ekleyin.

**ASP.NET uygulamaları** Bu parçacığı ApplicationInsights.config'e ekleyin:

```xml
<TelemetryProcessors>
  <Add Type="WebApplication9.SuccessfulDependencyFilter, WebApplication9">
     <!-- Set public property -->
     <MyParamFromConfigFile>2-beta</MyParamFromConfigFile>
  </Add>
</TelemetryProcessors>
```

Sınıfınızda genel adlandırılmış özellikler sağlayarak .config dosyasından dize değerlerini geçirebilirsiniz.

> [!WARNING]
> .config dosyasındaki tür adı ve özellik adlarını koddaki sınıf ve özellik adlarıyla eşleştirmeye özen bulunun. .config dosyası var olmayan bir türe veya özelliğe başvuruyorsa, SDK sessizce herhangi bir telemetri gönderemeyebilir.
>

**Alternatif olarak,** filtreyi kodda başlatmayapabilirsiniz. Uygun bir başlatma sınıfında (örneğin AppStart in) `Global.asax.cs` işlemcinizi zincire takın:

```csharp
var builder = TelemetryConfiguration.Active.DefaultTelemetrySink.TelemetryProcessorChainBuilder;
builder.Use((next) => new SuccessfulDependencyFilter(next));

// If you have more processors:
builder.Use((next) => new AnotherProcessor(next));

builder.Build();
```

Bu noktadan sonra oluşturulan TelemetryClients işlemcilerinizi kullanır.

**ASP.NET Çekirdek/ İşçi Hizmeti uygulamaları**

> [!NOTE]
> Kullanarak `ApplicationInsights.config` veya kullanarak `TelemetryConfiguration.Active` işlemci eklemek, ASP.NET Core uygulamaları veya Microsoft.ApplicationInsights.WorkerService SDK kullanıyorsanız geçerli değildir.

[Core](asp-net-core.md#adding-telemetry-processors) veya [WorkerService](worker-service.md#adding-telemetry-processors)ASP.NET kullanılarak yazılan `TelemetryProcessor` uygulamalar için, `AddApplicationInsightsTelemetryProcessor` `IServiceCollection`aşağıda gösterildiği gibi, uzantı yöntemi kullanılarak yeni bir ekleme yapılır. Bu yöntem, `ConfigureServices` sınıf yöntemi `Startup.cs` denir.

```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        // ...
        services.AddApplicationInsightsTelemetry();
        services.AddApplicationInsightsTelemetryProcessor<SuccessfulDependencyFilter>();

        // If you have more processors:
        services.AddApplicationInsightsTelemetryProcessor<AnotherProcessor>();
    }
```

### <a name="example-filters"></a>Örnek filtreler

#### <a name="synthetic-requests"></a>Sentetik istekler

Botları ve web testlerini filtreleyin. Metrics Explorer size sentetik kaynakları filtreleme seçeneği sunsa da, bu seçenek SDK'da filtreleyerek trafik ve yutma boyutunu azaltır.

```csharp
public void Process(ITelemetry item)
{
  if (!string.IsNullOrEmpty(item.Context.Operation.SyntheticSource)) {return;}

  // Send everything else:
  this.Next.Process(item);
}
```

#### <a name="failed-authentication"></a>Başarısız kimlik doğrulama

İstekleri "401" yanıtıyla filtreleyin.

```csharp
public void Process(ITelemetry item)
{
    var request = item as RequestTelemetry;

    if (request != null &&
    request.ResponseCode.Equals("401", StringComparison.OrdinalIgnoreCase))
    {
        // To filter out an item, return without calling the next processor.
        return;
    }

    // Send everything else
    this.Next.Process(item);
}
```

#### <a name="filter-out-fast-remote-dependency-calls"></a>Hızlı uzaktan bağımlılık çağrılarını filtreleme

Yalnızca yavaş olan aramaları tanılamak istiyorsanız, hızlı aramaları filtreleyin.

> [!NOTE]
> Bu, portalda gördüğünüz istatistikleri çarpıtacaktır.
>
>

```csharp
public void Process(ITelemetry item)
{
    var request = item as DependencyTelemetry;

    if (request != null && request.Duration.TotalMilliseconds < 100)
    {
        return;
    }
    this.Next.Process(item);
}
```

#### <a name="diagnose-dependency-issues"></a>Bağımlılık sorunlarını tanılama

[Bu blog,](https://azure.microsoft.com/blog/implement-an-application-insights-telemetry-processor/) bağımlılıklara otomatik olarak düzenli ping göndererek bağımlılık sorunlarını tanılamak için bir proje açıklar.

<a name="add-properties"></a>

### <a name="javascript-web-applications"></a>JavaScript Web uygulamaları

**ITelemetryInitializer kullanarak filtreleme**

1. Bir telemetri başlandırıcı geri çağırma işlevi oluşturun. Geri arama işlevi `ITelemetryItem` bir parametre olarak alır, bu da işlenen olaydır. Bu `false` geri aramadan döndürülen telemetri öğesi filtrelenecek sonuçlanır.  

   ```JS
   var filteringFunction = (envelope) => {
     if (envelope.data.someField === 'tobefilteredout') {
         return false;
     }
  
     return true;
   };
   ```

2. Telemetri başlandırıcı geri çağırmanızı ekleyin:

   ```JS
   appInsights.addTelemetryInitializer(filteringFunction);
   ```

## <a name="addmodify-properties-itelemetryinitializer"></a>Özellikleri ekleme/değiştirme: ITelemetryInitializer


Telemetri başharflerini ek bilgilerle zenginleştirmek ve/veya standart telemetri modülleri tarafından belirlenen telemetri özelliklerini geçersiz kılmak için kullanın.

Örneğin, Web paketi için Uygulama Öngörüleri HTTP istekleri hakkında telemetri toplar. Varsayılan olarak, yanıt kodu >= 400 olan herhangi bir isteği başarısız olarak işaretler. Ancak 400'ü başarı olarak ele almak istiyorsanız, Başarı özelliğini belirleyen bir telemetri başlandırıcısı sağlayabilirsiniz.

Bir telemetri başharfi sağlarsanız, Track*() yöntemlerinden herhangi biri çağrıldığında çağrılır. Bu `Track()` standart telemetri modülleri tarafından çağrılan yöntemleri içerir. Bu modüller, sözleşmeye göre, zaten bir baş harfi atör tarafından ayarlanmış herhangi bir özellik belirlemez. Telemetri başharfleri telemetri işlemcileri aramadan önce çağrılır. Bu nedenle, baş harflertarafından yapılan herhangi bir zenginleştirme işlemciler tarafından görülebilir.

**Baş harfinizi tanımlayın**

*C #*

```csharp
using System;
using Microsoft.ApplicationInsights.Channel;
using Microsoft.ApplicationInsights.DataContracts;
using Microsoft.ApplicationInsights.Extensibility;

namespace MvcWebRole.Telemetry
{
  /*
   * Custom TelemetryInitializer that overrides the default SDK
   * behavior of treating response codes >= 400 as failed requests
   *
   */
  public class MyTelemetryInitializer : ITelemetryInitializer
  {
    public void Initialize(ITelemetry telemetry)
    {
        var requestTelemetry = telemetry as RequestTelemetry;
        // Is this a TrackRequest() ?
        if (requestTelemetry == null) return;
        int code;
        bool parsed = Int32.TryParse(requestTelemetry.ResponseCode, out code);
        if (!parsed) return;
        if (code >= 400 && code < 500)
        {
            // If we set the Success property, the SDK won't change it:
            requestTelemetry.Success = true;

            // Allow us to filter these requests in the portal:
            requestTelemetry.Properties["Overridden400s"] = "true";
        }
        // else leave the SDK to set the Success property
    }
  }
}
```

**ASP.NET uygulamaları: Baş harfünüzü yükleyin**

In ApplicationInsights.config:

```xml
<ApplicationInsights>
  <TelemetryInitializers>
    <!-- Fully qualified type name, assembly name: -->
    <Add Type="MvcWebRole.Telemetry.MyTelemetryInitializer, MvcWebRole"/>
    ...
  </TelemetryInitializers>
</ApplicationInsights>
```

*Alternatif olarak,* koddaki baş harfe ait, örneğin Global.aspx.cs:

```csharp
protected void Application_Start()
{
    // ...
    TelemetryConfiguration.Active.TelemetryInitializers.Add(new MyTelemetryInitializer());
}
```

[Bu örnek daha fazla bakın.](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/Samples/AzureEmailService/MvcWebRole)

**ASP.NET Çekirdek/ İşçi Hizmeti uygulamaları: Baş harfünüzü yükleyin**

> [!NOTE]
> Kullanarak `ApplicationInsights.config` veya kullanma `TelemetryConfiguration.Active` başlatma yı kullanarak başlatma eklemek, ASP.NET Core uygulamaları veya Microsoft.ApplicationInsights.WorkerService SDK kullanıyorsanız geçerli değildir.

Core veya [WorkerService](worker-service.md#adding-telemetryinitializers) [kullanılarak yazılan ASP.NET](asp-net-core.md#adding-telemetryinitializers) `TelemetryInitializer` uygulamalar için, aşağıda gösterildiği gibi Bağımlılık Enjeksiyonu kabına ekleyerek yeni bir ekleme yapılır. Bu yöntemle `Startup.ConfigureServices` yapılır.

```csharp
 using Microsoft.ApplicationInsights.Extensibility;
 using CustomInitializer.Telemetry;
 public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<ITelemetryInitializer, MyTelemetryInitializer>();
}
```
### <a name="javascript-telemetry-initializers"></a>JavaScript telemetri başharfleri
*JavaScript*

Portaldan aldığınız başlatma kodundan hemen sonra bir telemetri başharfi ekleyin:

```JS
<script type="text/javascript">
    // ... initialization code
    ...({
        instrumentationKey: "your instrumentation key"
    });
    window.appInsights = appInsights;


    // Adding telemetry initializer.
    // This is called whenever a new telemetry item
    // is created.

    appInsights.queue.push(function () {
        appInsights.context.addTelemetryInitializer(function (envelope) {
            var telemetryItem = envelope.data.baseData;

            // To check the telemetry items type - for example PageView:
            if (envelope.name == Microsoft.ApplicationInsights.Telemetry.PageView.envelopeType) {
                // this statement removes url from all page view documents
                telemetryItem.url = "URL CENSORED";
            }

            // To set custom properties:
            telemetryItem.properties = telemetryItem.properties || {};
            telemetryItem.properties["globalProperty"] = "boo";

            // To set custom metrics:
            telemetryItem.measurements = telemetryItem.measurements || {};
            telemetryItem.measurements["globalMetric"] = 100;
        });
    });

    // End of inserted code.

    appInsights.trackPageView();
</script>
```

Telemetri Öğesi'nde bulunan özel olmayan özelliklerin bir özeti [için](../../azure-monitor/app/export-data-model.md)Bkz.

İstediğiniz kadar baş harf ekleyebilirsiniz ve bunlar eklendikleri sırada çağrılır.

### <a name="opencensus-python-telemetry-processors"></a>OpenCensus Python telemetri işlemcileri

OpenCensus Python'daki Telemetri işlemcileri, dışa aktarılmadan önce telemetriyi işlemek için çağrılan geri arama işlevleridir. Geri arama işlevi, parametresi olarak [zarf](https://github.com/census-instrumentation/opencensus-python/blob/master/contrib/opencensus-ext-azure/opencensus/ext/azure/common/protocol.py#L86) veri türünü kabul etmelidir. Dışa aktarılan telemetriyi filtrelemek için, geri `False`arama işlevinin döndürüldiğinden emin olun. Burada [zarflarda](https://github.com/census-instrumentation/opencensus-python/blob/master/contrib/opencensus-ext-azure/opencensus/ext/azure/common/protocol.py)Azure Monitor veri türleri için şema görebilirsiniz.

> [!NOTE]
> Alandaki özniteliği `cloud_RoleName` değiştirerek `ai.cloud.role` değiştirebilirsiniz. `tags`

```python
def callback_function(envelope):
    envelope.tags['ai.cloud.role'] = 'new_role_name.py'
```

```python
# Example for log exporter
import logging

from opencensus.ext.azure.log_exporter import AzureLogHandler

logger = logging.getLogger(__name__)

# Callback function to append '_hello' to each log message telemetry
def callback_function(envelope):
    envelope.data.baseData.message += '_hello'
    return True

handler = AzureLogHandler(connection_string='InstrumentationKey=<your-instrumentation_key-here>')
handler.add_telemetry_processor(callback_function)
logger.addHandler(handler)
logger.warning('Hello, World!')
```
```python
# Example for trace exporter
import requests

from opencensus.ext.azure.trace_exporter import AzureExporter
from opencensus.trace import config_integration
from opencensus.trace.samplers import ProbabilitySampler
from opencensus.trace.tracer import Tracer

config_integration.trace_integrations(['requests'])

# Callback function to add os_type: linux to span properties
def callback_function(envelope):
    envelope.data.baseData.properties['os_type'] = 'linux'
    return True

exporter = AzureExporter(
    connection_string='InstrumentationKey=<your-instrumentation-key-here>'
)
exporter.add_telemetry_processor(callback_function)
tracer = Tracer(exporter=exporter, sampler=ProbabilitySampler(1.0))
with tracer.span(name='parent'):
response = requests.get(url='https://www.wikipedia.org/wiki/Rabbit')
```

```python
# Example for metrics exporter
import time

from opencensus.ext.azure import metrics_exporter
from opencensus.stats import aggregation as aggregation_module
from opencensus.stats import measure as measure_module
from opencensus.stats import stats as stats_module
from opencensus.stats import view as view_module
from opencensus.tags import tag_map as tag_map_module

stats = stats_module.stats
view_manager = stats.view_manager
stats_recorder = stats.stats_recorder

CARROTS_MEASURE = measure_module.MeasureInt("carrots",
                                            "number of carrots",
                                            "carrots")
CARROTS_VIEW = view_module.View("carrots_view",
                                "number of carrots",
                                [],
                                CARROTS_MEASURE,
                                aggregation_module.CountAggregation())

# Callback function to only export the metric if value is greater than 0
def callback_function(envelope):
    return envelope.data.baseData.metrics[0].value > 0

def main():
    # Enable metrics
    # Set the interval in seconds in which you want to send metrics
    exporter = metrics_exporter.new_metrics_exporter(connection_string='InstrumentationKey=<your-instrumentation-key-here>')
    exporter.add_telemetry_processor(callback_function)
    view_manager.register_exporter(exporter)

    view_manager.register_view(CARROTS_VIEW)
    mmap = stats_recorder.new_measurement_map()
    tmap = tag_map_module.TagMap()

    mmap.measure_int_put(CARROTS_MEASURE, 1000)
    mmap.record(tmap)
    # Default export interval is every 15.0s
    # Your application should run for at least this amount
    # of time so the exporter will meet this interval
    # Sleep can fulfill this
    time.sleep(60)

    print("Done recording metrics")

if __name__ == "__main__":
    main()
```
İstediğiniz kadar işlemci ekleyebilirsiniz ve bunlar eklendikleri sırada çağrılır. Bir işlemci bir özel durum atarsa, aşağıdaki işlemcileri etkilemez.

### <a name="example-telemetryinitializers"></a>Örnek TelemetriInitializörler

#### <a name="add-custom-property"></a>Özel özellik ekleme

Aşağıdaki örnek baş harf, izlenen her telemetriye özel bir özellik ekler.

```csharp
public void Initialize(ITelemetry item)
{
  var itemProperties = item as ISupportProperties;
  if(itemProperties != null && !itemProperties.Properties.ContainsKey("customProp"))
    {
        itemProperties.Properties["customProp"] = "customValue";
    }
}
```

#### <a name="add-cloud-role-name"></a>Bulut rol adı ekleme

Aşağıdaki örnek baş harf, izlenen her telemetriiçin bulut rol adını ayarlar.

```csharp
public void Initialize(ITelemetry telemetry)
{
    if (string.IsNullOrEmpty(telemetry.Context.Cloud.RoleName))
    {
        telemetry.Context.Cloud.RoleName = "MyCloudRoleName";
    }
}
```

#### <a name="add-information-from-httpcontext"></a>HttpContext'tan bilgi ekleme

Aşağıdaki örnek initializer verileri [`HttpContext`](https://docs.microsoft.com/aspnet/core/fundamentals/http-context?view=aspnetcore-3.1) okur ve bir `RequestTelemetry` örneğe ekler. Otomatik `IHttpContextAccessor` olarak yapıcı bağımlılık enjeksiyonu ile sağlanır.

```csharp
public class HttpContextRequestTelemetryInitializer : ITelemetryInitializer
{
    private readonly IHttpContextAccessor httpContextAccessor;

    public HttpContextRequestTelemetryInitializer(IHttpContextAccessor httpContextAccessor)
    {
        this.httpContextAccessor =
            httpContextAccessor ??
            throw new ArgumentNullException(nameof(httpContextAccessor));
    }

    public void Initialize(ITelemetry telemetry)
    {
        var requestTelemetry = telemetry as RequestTelemetry;
        if (requestTelemetry == null) return;

        var claims = this.httpContextAccessor.HttpContext.User.Claims;
        Claim oidClaim = claims.FirstOrDefault(claim => claim.Type == "oid");
        requestTelemetry.Properties.Add("UserOid", oidClaim?.Value);
    }
}
```

## <a name="itelemetryprocessor-and-itelemetryinitializer"></a>ITelemetryİşlemci ve ITelemetryInitializer

Telemetri işlemcileri ile telemetri başharfleri arasındaki fark nedir?

* Onlarla yapabileceklerinızda bazı çakışmalar vardır: her ikisi de telemetrinin özelliklerini eklemek veya değiştirmek için kullanılabilir, ancak bu amaçla baş harflerini kullanmak önerilir.
* TelemetriInitializers her zaman Telemetryİşlemciler önce çalıştırın.
* TelemetriInitializers birden fazla kez çağrılabilir. Sözleşmeye göre, önceden ayarlanmış herhangi bir özellik belirlemez.
* Telemetriİşlemcileri, bir telemetri öğesini tamamen değiştirmenize veya atmanıza olanak sağlar.
* Tüm kayıtlı TelemetriInitializers her telemetri öğesi için çağrılması garanti edilir. Telemetri işlemciler için SDK, ilk telemetri işlemcisini aramayı garanti eder. Geri kalan işlemcilerin çağrılması veya çağrılmadığı, önceki telemetri işlemcileri tarafından belirlenir.
* Telemetri'yi ek özelliklerle zenginleştirmek veya varolan özelliği geçersiz kılmak için TelemetryInitializers'ı kullanın. Telemetriyi filtrelemek için TelemetriProcessor'i kullanın.

## <a name="troubleshooting-applicationinsightsconfig"></a>Sorun Giderme ApplicationInsights.config

* Tam nitelikli tür adının ve montaj adının doğru olduğunu doğrulayın.
* Applicationinsights.config dosyasının çıktı dizininizde olduğunu ve son değişiklikleri içerdiğini doğrulayın.

## <a name="reference-docs"></a>Referans dokümanları

* [API’ye Genel Bakış](../../azure-monitor/app/api-custom-events-metrics.md)
* [ASP.NET referans](https://msdn.microsoft.com/library/dn817570.aspx)

## <a name="sdk-code"></a>SDK Kodu

* [ASP.NET Core SDK](https://github.com/Microsoft/ApplicationInsights-aspnetcore)
* [ASP.NET SDK](https://github.com/Microsoft/ApplicationInsights-dotnet)
* [JavaScript SDK'sı](https://github.com/Microsoft/ApplicationInsights-JS)

## <a name="next-steps"></a><a name="next"></a>Sonraki adımlar
* [Arama olayları ve günlükleri](../../azure-monitor/app/diagnostic-search.md)
* [Örnekleme](../../azure-monitor/app/sampling.md)
* [Sorun giderme](../../azure-monitor/app/troubleshoot-faq.md)
