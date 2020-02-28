---
title: Azure Application Insights SDK 'sında filtreleme ve ön işleme | Microsoft Docs
description: Telemetri Application Insights portalına gönderilmeden önce verileri filtrelemek veya eklemek için SDK için telemetri Işlemcileri ve telemetri başlatıcıları yazın.
ms.topic: conceptual
ms.date: 11/23/2016
ms.openlocfilehash: 9f4df83ed60ba94913702b9a32a298f0ac62f9f4
ms.sourcegitcommit: 747a20b40b12755faa0a69f0c373bd79349f39e3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2020
ms.locfileid: "77666471"
---
# <a name="filtering-and-preprocessing-telemetry-in-the-application-insights-sdk"></a>Application Insights SDK 'da telemetri filtreleme ve ön işleme

Application Insights hizmetine gönderilmeden önce Telemetriyi nasıl zenginleştirdiğini ve işlenebileceğinizi özelleştirmek için Application Insights SDK 'Sı için eklentiler yazabilir ve yapılandırabilirsiniz.

* [Örnekleme](sampling.md) , istatistiklerinizi etkilemeden telemetri hacmini azaltır. Bir sorunu tanılarken aralarında gezinebilmeniz için ilgili veri noktalarını birlikte tutar. Portalda toplam sayımlar, örnekleme için telafi ile çarpılır.
* Telemetri Işlemcilerle filtreleme, SDK 'da, sunucuya gönderilmeden önce Telemetriyi filtrelemenizi sağlar. Örneğin, robotlardan gelen istekleri dışlayarak telemetri hacmini azaltabilirsiniz. Filtreleme, trafiği örneklemeden azaltmak için daha temel bir yaklaşımdır. Aktarılmaları üzerinde daha fazla denetim sağlar, ancak tüm başarılı istekleri filtrelemeniz gibi, istatistiklerinizi etkilediğine dikkat etmeniz gerekir.
* Telemetri başlatıcıları, standart modüllerden gelen telemetri dahil olmak üzere uygulamanızdan gönderilen herhangi bir telemetriye [Özellikler ekler veya değiştirir](#add-properties) . Örneğin, hesaplanmış değerler ekleyebilirsiniz; veya portaldaki verilerin filtreleneceği sürüm numaraları.
* [SDK API 'si](../../azure-monitor/app/api-custom-events-metrics.md) özel olayları ve ölçümleri göndermek için kullanılır.

Başlamadan önce:

* Uygulamanız için uygun SDK 'Yı yükler: .NET/.NET Core, [Java](../../azure-monitor/app/java-get-started.md) veya [JavaScript](javascript.md) için [ASP.net](asp-net.md), [ASP.NET Core](asp-net-core.md), [http olmayan/Worker](worker-service.md)

<a name="filtering"></a>

## <a name="filtering"></a>Filtreleme

Bu teknik, telemetri akışından dahil edilen veya dışlanan Özellikler üzerinde doğrudan denetim elde etmenizi sağlar. Filtreleme, telemetri öğelerini Application Insights gönderilmeden bırakmak için kullanılabilir. Bunu örneklemeyle veya ayrı olarak kullanabilirsiniz.

Telemetriyi filtrelemek için bir telemetri işlemcisi yazın ve `TelemetryConfiguration`kaydedin. Tüm telemetri işlemcinizden geçer ve akıştan bırakmayı veya zincirdeki bir sonraki işlemciye vermenizi seçebilirsiniz. Bu, HTTP istek toplayıcısı ve bağımlılık toplayıcısı gibi standart modüllerden ve kendi kendinize izettiğiniz Telemetriyi içerir. Örneğin, robots veya başarılı bağımlılık çağrılarından gelen istekler hakkında telemetri filtreleyebilirsiniz.

> [!WARNING]
> İşlemciler kullanılarak SDK 'dan gönderilen telemetrinin filtrelenmesi, portalda gördüğünüz istatistikleri eğebilir ve ilgili öğeleri izlemeyi zorlaştırır.
>
> Bunun yerine [örnekleme](../../azure-monitor/app/sampling.md)kullanmayı düşünün.
>
>

### <a name="create-a-telemetry-processor-c"></a>Telemetri işlemcisi (C#) oluşturma

1. Bir filtre oluşturmak için `ITelemetryProcessor`uygulayın.

    Telemetri Işlemcilerin bir işlem zinciri oluşturduğuna dikkat edin. Bir telemetri işlemcisini örneklediğinizde, zincirdeki bir sonraki işlemciye bir başvuru verilir. Bir telemetri veri noktası Işlem yöntemine geçirildiğinde, işi çalışır ve sonra zincirde bir sonraki telemetri Işlemcisini çağırır (veya çağırmaz).

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

**ASP.NET uygulamaları** Bu kod parçacığını ApplicationInsights. config dosyasına ekleyin:

```xml
<TelemetryProcessors>
  <Add Type="WebApplication9.SuccessfulDependencyFilter, WebApplication9">
     <!-- Set public property -->
     <MyParamFromConfigFile>2-beta</MyParamFromConfigFile>
  </Add>
</TelemetryProcessors>
```

Sınıfınıza ortak adlandırılmış özellikler sağlayarak. config dosyasından dize değerlerini geçirebilirsiniz.

> [!WARNING]
> . Config dosyasındaki tür adı ve tüm özellik adlarını, koddaki sınıf ve özellik adlarına eşleştirmek için dikkatli olmak. . Config dosyası var olmayan bir türe veya özelliğe başvuruyorsa, SDK hiçbir Telemetriyi sessizce gönderemeyebilir.
>

**Alternatif olarak,** kodda filtreyi başlatabilirsiniz. Uygun bir başlatma sınıfında; Örneğin, `Global.asax.cs`: uygulama zincirinde

```csharp
var builder = TelemetryConfiguration.Active.DefaultTelemetrySink.TelemetryProcessorChainBuilder;
builder.Use((next) => new SuccessfulDependencyFilter(next));

// If you have more processors:
builder.Use((next) => new AnotherProcessor(next));

builder.Build();
```

Bu noktadan sonra oluşturulan TelemetryClients, işlemcinizi kullanacaktır.

**ASP.NET Core/çalışan hizmeti uygulamaları**

> [!NOTE]
> `ApplicationInsights.config` veya `TelemetryConfiguration.Active` kullanarak işlemciyi eklemek ASP.NET Core uygulamaları için geçerli değildir veya Microsoft. ApplicationInsights. WorkerService SDK kullanıyorsanız.

[ASP.NET Core](asp-net-core.md#adding-telemetry-processors) veya [workerservice](worker-service.md#adding-telemetry-processors)kullanılarak yazılan uygulamalar için, aşağıda gösterildiği gibi yeni bir `TelemetryProcessor` eklemek `IServiceCollection`üzerinde `AddApplicationInsightsTelemetryProcessor` genişletme yöntemi kullanılarak yapılır. Bu yöntem, `Startup.cs` sınıfınızın `ConfigureServices` yönteminde çağrılır.

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

#### <a name="synthetic-requests"></a>Yapay istekler

Botları ve Web testlerini filtreleyin. Ölçüm Gezgini yapay kaynakları filtreleme seçeneği sunmakla birlikte bu seçenek, trafiği ve alma boyutunu SDK 'da filtreleyerek azaltır.

```csharp
public void Process(ITelemetry item)
{
  if (!string.IsNullOrEmpty(item.Context.Operation.SyntheticSource)) {return;}

  // Send everything else:
  this.Next.Process(item);
}
```

#### <a name="failed-authentication"></a>Kimlik doğrulaması başarısız

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

#### <a name="filter-out-fast-remote-dependency-calls"></a>Hızlı uzak bağımlılık çağrılarını filtrele

Yalnızca yavaş olan çağrıları tanılamak istiyorsanız hızlı bir şekilde filtreleyin.

> [!NOTE]
> Bu, portalda gördüğünüz istatistikleri eğriltebilir.
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

#### <a name="diagnose-dependency-issues"></a>Bağımlılık sorunlarını Tanıla

[Bu blog](https://azure.microsoft.com/blog/implement-an-application-insights-telemetry-processor/) , bağımlılıklara otomatik olarak ping göndererek bağımlılık sorunlarını tanılamaya yönelik bir projeyi açıklar.

<a name="add-properties"></a>

### <a name="javascript-web-applications"></a>JavaScript Web uygulamaları

**Ilemetrybaşlatıcısı kullanarak filtreleme**

1. Telemetri başlatıcısı geri çağırma işlevi oluşturun. Geri çağırma işlevi, işlenen olay olan `ITelemetryItem` bir parametre olarak alır. Bu geri aramadan `false` döndürmek telemetri öğesinin filtrelenmesini sağlar.  

   ```JS
   var filteringFunction = (envelope) => {
     if (envelope.data.someField === 'tobefilteredout') {
        return false;
     }
  
     return true;
   };
   ```

2. Telemetri başlatıcısı geri aramasını ekleyin:

   ```JS
   appInsights.addTelemetryInitializer(filteringFunction);
   ```

## <a name="addmodify-properties-itelemetryinitializer"></a>Özellik Ekle/Değiştir: ıtelemetrybaşlatıcısı


Telemetri başlatıcıları 'nı ek bilgilerle birlikte zenginleştirmek ve/veya standart telemetri modülleri tarafından ayarlanan telemetri özelliklerini geçersiz kılmak için kullanın.

Örneğin, Web paketi için Application Insights HTTP istekleri hakkında telemetri toplar. Varsayılan olarak, yanıt kodu > = 400 olan herhangi bir istek başarısız olarak işaretler. Ancak, 400 ' i başarılı olarak değerlendirmek istiyorsanız, başarı özelliğini ayarlayan bir telemetri başlatıcısı sağlayabilirsiniz.

Bir telemetri başlatıcısı sağlarsanız, her bir Track * () yöntemi çağrıldığında çağırılır. Buna standart telemetri modülleri tarafından çağrılan `Track()` Yöntemler dahildir. Kurala göre, bu modüller zaten bir başlatıcı tarafından ayarlanmış herhangi bir özelliği ayarlamayın. Telemetri başlatıcıları, telemetri işlemcileri çağrılmadan önce çağrılır. Bu nedenle, başlatıcıların yaptığı her türlü zenginleştirme işlemciler tarafından görülebilir.

**Başlatıcısını tanımlama**

*C#*

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

**ASP.NET uygulamalar: başlatıcısını yükleme**

ApplicationInsights. config dosyasında:

```xml
<ApplicationInsights>
  <TelemetryInitializers>
    <!-- Fully qualified type name, assembly name: -->
    <Add Type="MvcWebRole.Telemetry.MyTelemetryInitializer, MvcWebRole"/>
    ...
  </TelemetryInitializers>
</ApplicationInsights>
```

*Alternatif olarak,* Global.aspx.cs içinde örneğin, başlatıcıda Başlatıcı örneğini oluşturabilirsiniz:

```csharp
protected void Application_Start()
{
    // ...
    TelemetryConfiguration.Active.TelemetryInitializers.Add(new MyTelemetryInitializer());
}
```

[Bu örnekten daha fazla bilgi görüntüleyin.](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/Samples/AzureEmailService/MvcWebRole)

**ASP.NET Core/çalışan hizmeti uygulamaları: başlatıcısını yükleyin**

> [!NOTE]
> `ApplicationInsights.config` veya `TelemetryConfiguration.Active` kullanarak Başlatıcı eklemek ASP.NET Core uygulamaları için geçerli değildir veya Microsoft. ApplicationInsights. WorkerService SDK kullanıyorsanız.

[ASP.NET Core](asp-net-core.md#adding-telemetryinitializers) veya [workerservice](worker-service.md#adding-telemetryinitializers)kullanılarak yazılan uygulamalar için, aşağıda gösterildiği gibi, yeni bir `TelemetryInitializer` eklemek, bağımlılık ekleme kapsayıcısına eklenerek yapılır. Bu, `Startup.ConfigureServices` yönteminde yapılır.

```csharp
 using Microsoft.ApplicationInsights.Extensibility;
 using CustomInitializer.Telemetry;
 public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<ITelemetryInitializer, MyTelemetryInitializer>();
}
```

### <a name="java-telemetry-initializers"></a>Java telemetri başlatıcıları

[Java SDK belgeleri](https://docs.microsoft.com/java/api/com.microsoft.applicationinsights.extensibility.telemetryinitializer?view=azure-java-stable)

```Java
public interface TelemetryInitializer
{ /** Initializes properties of the specified object. * @param telemetry The {@link com.microsoft.applicationinsights.telemetry.Telemetry} to initialize. */

void initialize(Telemetry telemetry); }
```

Ardından, özel başlatıcıyı ApplicationInsights. xml dosyanıza kaydedin.

```xml
<Add type="mypackage.MyConfigurableContextInitializer">
    <Param name="some_config_property" value="some_value" />
</Add>
```

### <a name="javascript-telemetry-initializers"></a>JavaScript telemetri başlatıcıları
*JavaScript*

Portaldan aldığınız başlatma kodundan hemen sonra bir telemetri başlatıcısı ekleyin:

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

TelemetryItem üzerinde bulunan özel olmayan özelliklerin bir özeti için bkz. [Application Insights dışarı aktarma veri modeli](../../azure-monitor/app/export-data-model.md).

İstediğiniz kadar fazla Başlatıcı ekleyebilirsiniz ve bunlar eklendikleri sırada çağırılır.

### <a name="opencensus-python-telemetry-processors"></a>OpenCensus Python telemetri işlemcileri

OpenCensus Python 'da telemetri işlemcileri, yalnızca Telemetriyi işlemek için çağrılan geri çağırma işlevleridir. Geri çağırma işlevi, parametresi olarak bir [Envelope](https://github.com/census-instrumentation/opencensus-python/blob/master/contrib/opencensus-ext-azure/opencensus/ext/azure/common/protocol.py#L86) veri türü kabul etmelidir. Telemetriyi dışarı aktarma işleminden filtrelemek için geri çağırma işlevinin `False`döndürdüğünden emin olun. Azure Izleyici veri türleri şemasını [burada](https://github.com/census-instrumentation/opencensus-python/blob/master/contrib/opencensus-ext-azure/opencensus/ext/azure/common/protocol.py)zarfların içinde görebilirsiniz.

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
İstediğiniz kadar işlemci ekleyebilirsiniz ve bunlar eklendikleri sırada çağırılır. Bir işlemcinin bir özel durum oluşturması gerekiyorsa, bu, aşağıdaki işlemcileri etkilemez.

### <a name="example-telemetryinitializers"></a>Örnek TelemetryInitializers

#### <a name="add-custom-property"></a>Özel Özellik Ekle

Aşağıdaki örnek başlatıcı, izlenen her telemetriye özel bir özellik ekler.

```csharp
public void Initialize(ITelemetry item)
{
  var itemProperties = item as ISupportProperties;
  if(itemProperties != null && !itemProperties.ContainsKey("customProp"))
    {
        itemProperties.Properties["customProp"] = "customValue";
    }
}
```

#### <a name="add-cloud-role-name"></a>Bulut rolü adı Ekle

Aşağıdaki örnek başlatıcı, bulut rolü adını izlenen her telemetriye ayarlar.

```csharp
public void Initialize(ITelemetry telemetry)
{
    if(string.IsNullOrEmpty(telemetry.Context.Cloud.RoleName))
    {
        telemetry.Context.Cloud.RoleName = "MyCloudRoleName";
    }
}
```

## <a name="itelemetryprocessor-and-itelemetryinitializer"></a>Ilemetryprocessor ve ılemetrybaşlatıcısı

Telemetri işlemcileri ve telemetri başlatıcıları arasındaki fark nedir?

* Bunlarla neler yapabileceğinize ilişkin bazı örtüşmeler vardır: her ikisi de telemetri özellikleri eklemek veya değiştirmek için kullanılabilir, ancak bu amaçla başlatıcılar kullanılması önerilir.
* TelemetryInitializers, TelemetryProcessors 'den önce her zaman çalıştırın.
* TelemetryInitializers birden çok kez çağrılabilir. Kurala göre, zaten ayarlanmış herhangi bir özelliği ayarlamayın.
* TelemetryProcessors bir telemetri öğesini tamamen değiştirmenizi veya atmayı sağlar.
* Tüm kayıtlı TelemetryInitializers her telemetri öğesi için çağrılması garanti edilir. Telemetri işlemcileri için SDK, ilk telemetri işlemcisinin çağrılmasını garanti eder. İşlemcilerin geri kalanının adlandırılıp çağrılmayacağı, önceki telemetri işlemcileri tarafından belirlenir.
* Ek özelliklerle telemetri zenginleştirmek veya var olanı geçersiz kılmak için TelemetryInitializers kullanın. Telemetriyi filtrelemek için TelemetryProcessor kullanın.

## <a name="troubleshooting-applicationinsightsconfig"></a>ApplicationInsights. config sorunlarını giderme

* Tam nitelikli tür adı ve derleme adının doğru olduğunu onaylayın.
* ApplicationInsights. config dosyasının çıkış dizininizde olduğunu ve en son değişiklikleri içerdiğini doğrulayın.

## <a name="reference-docs"></a>Başvuru belgeleri

* [API’ye Genel Bakış](../../azure-monitor/app/api-custom-events-metrics.md)
* [ASP.NET başvurusu](https://msdn.microsoft.com/library/dn817570.aspx)

## <a name="sdk-code"></a>SDK kodu

* [ASP.NET Core SDK](https://github.com/Microsoft/ApplicationInsights-aspnetcore)
* [ASP.NET SDK](https://github.com/Microsoft/ApplicationInsights-dotnet)
* [JavaScript SDK'sı](https://github.com/Microsoft/ApplicationInsights-JS)

## <a name="next"></a>Sonraki adımlar
* [Olayları ve günlükleri ara](../../azure-monitor/app/diagnostic-search.md)
* [Örnekleme](../../azure-monitor/app/sampling.md)
* [Sorun giderme](../../azure-monitor/app/troubleshoot-faq.md)
