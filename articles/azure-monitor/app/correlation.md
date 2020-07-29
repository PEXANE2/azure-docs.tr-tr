---
title: Azure Application Insights telemetri bağıntısı | Microsoft Docs
description: Application Insights telemetri bağıntısı
ms.topic: conceptual
author: lgayhardt
ms.author: lagayhar
ms.date: 06/07/2019
ms.reviewer: sergkanz
ms.custom: tracking-python
ms.openlocfilehash: b4facaee44a0bc5c7d64376ca80e5aaf8d0768d0
ms.sourcegitcommit: a76ff927bd57d2fcc122fa36f7cb21eb22154cfa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87323171"
---
# <a name="telemetry-correlation-in-application-insights"></a>Application Insights telemetri bağıntısı

Mikro hizmetler dünyasında, her mantıksal işlem, hizmetin çeşitli bileşenlerinde iş yapılmasını gerektirir. [Application Insights](./app-insights-overview.md)kullanarak, bu bileşenlerin her birini ayrı ayrı izleyebilirsiniz. Application Insights, hangi bileşenin hatalardan veya performans düşüşüne karşı sorumlu olduğunu saptamak için kullandığınız dağıtılmış telemetri bağıntısını destekler.

Bu makalede, birden çok bileşen tarafından gönderilen telemetrinin ilişkilendirilmesi için Application Insights tarafından kullanılan veri modeli açıklanmaktadır. Bağlam yayma tekniklerini ve protokollerini içerir. Ayrıca, farklı diller ve platformlarda bağıntı taclerin uygulanmasını da ele alır.

## <a name="data-model-for-telemetry-correlation"></a>Telemetri bağıntısı için veri modeli

Application Insights, dağıtılmış telemetri bağıntısı için bir [veri modeli](./data-model.md) tanımlar. Telemetrinin bir mantıksal işlemle ilişkilendirilmesi için, her telemetri öğesinin adında bir bağlam alanı vardır `operation_Id` . Bu tanımlayıcı, dağıtılmış izlemede her telemetri öğesi tarafından paylaşılır. Bu nedenle, tek bir katmanda telemetri kaybetseniz bile, diğer bileşenler tarafından raporlanan Telemetriyi yine de ilişkilendirebilirsiniz.

Dağıtılmış bir mantıksal işlem, genellikle bileşenlerinden biri tarafından işlenen istekler olan daha küçük bir işlem kümesinden oluşur. Bu işlemler [istek telemetrisi](./data-model-request-telemetry.md)tarafından tanımlanır. Her istek telemetri öğesi `id` kendisini benzersiz ve küresel olarak tanımlayan kendi kendine sahiptir. Ve istekle ilişkili tüm telemetri öğeleri (izlemeler ve özel durumlar gibi) `operation_parentId` isteğin değerine ayarlanmalıdır `id` .

Başka bir bileşene yönelik HTTP çağrısı gibi her giden işlem, [bağımlılık telemetrisi](./data-model-dependency-telemetry.md)tarafından temsil edilir. Bağımlılık telemetrisi, `id` genel olarak benzersiz olan kendisini de tanımlar. Bu bağımlılık çağrısıyla başlatılan istek telemetrisi, bunu `id` olarak kullanır `operation_parentId` .

`operation_Id`, Ve ile kullanarak dağıtılmış mantıksal işlemin bir görünümünü oluşturabilirsiniz `operation_parentId` `request.id` `dependency.id` . Bu alanlar telemetri çağrılarının önem derecesine göre de tanımlar.

Mikro hizmetler ortamında, bileşenlerden izlemeler farklı depolama öğelerine gidebilir. Her bileşen Application Insights kendi izleme anahtarına sahip olabilir. Mantıksal işlem için telemetri almak için, Application Insights her depolama öğesinden verileri sorgular. Depolama öğelerinin sayısı büyükse, ileri bakabileceğiniz bir ipucu gerekir. Application Insights veri modeli, bu sorunu çözmek için iki alanı tanımlar: `request.source` ve `dependency.target` . İlk alan, bağımlılık isteğini Başlatan bileşeni tanımlar. İkinci alan, bağımlılık çağrısının yanıtını döndüren bileşeni tanımlar.

## <a name="example"></a>Örnek

Bir örneğe göz atalım. Hisse senedi fiyatları adlı bir uygulama, hisse senedi adlı bir dış API kullanarak bir stokun geçerli pazar fiyatını gösterir. Hisse senedi fiyatları uygulaması, kullanarak istemci Web tarayıcısının açtığı hisse senedi sayfası adlı bir sayfa içerir `GET /Home/Stock` . Uygulama, HTTP çağrısını kullanarak hisse senedi API 'sini sorgular `GET /api/stock/value` .

Bir sorgu çalıştırarak elde edilen telemetrisini çözümleyebilirsiniz:

```kusto
(requests | union dependencies | union pageViews)
| where operation_Id == "STYz"
| project timestamp, itemType, name, id, operation_ParentId, operation_Id
```

Sonuçlarda, tüm telemetri öğelerinin kökü paylaştığından emin olmanız gerektiğini unutmayın `operation_Id` . Sayfadan bir AJAX çağrısı yapıldığında, bağımlılık telemetrisine yeni bir benzersiz KIMLIK ( `qJSXU` ) atanır ve pageView kimliği olarak kullanılır `operation_ParentId` . Sunucu isteği daha sonra Ajax KIMLIĞINI olarak kullanır `operation_ParentId` .

| ItemType   | name                      | ID           | operation_ParentId | operation_Id |
|------------|---------------------------|--------------|--------------------|--------------|
| Sayfa görüntülemesi   | Hisse senedi sayfası                |              | Stilz               | Stilz         |
| bağımlılık | /Home/Stock al           | qJSXU        | Stilz               | Stilz         |
| istek    | Giriş/stok al            | KqKwlrSt9PA = | qJSXU              | Stilz         |
| bağımlılık | /Api/Stock/Value al      | bBrf2L7mm2g = | KqKwlrSt9PA =       | Stilz         |

`GET /api/stock/value`Bir dış hizmete çağrı yapıldığında, alanı uygun şekilde ayarlayabilmeniz için bu sunucunun kimliğini bilmeniz gerekir `dependency.target` . Dış hizmet izlemeyi desteklemiyorsa, `target` hizmetin ana bilgisayar adına ayarlanır (örneğin, `stock-prices-api.com` ). Ancak hizmet, önceden tanımlı bir HTTP üst bilgisi döndürerek kendisini tanımlarsa, `target` Application Insights bu hizmetten telemetri 'i sorgulayarak dağıtılmış bir izleme oluşturmasına izin veren hizmet kimliğini içerir.

## <a name="correlation-headers"></a>Bağıntı üstbilgileri

Application Insights, şunu tanımlayan [W3C Trace-Context](https://w3c.github.io/trace-context/)öğesine geçiyor:

- `traceparent`: Genel benzersiz işlem KIMLIĞI ve çağrının benzersiz tanımlayıcısını taşır.
- `tracestate`: Sisteme özgü izleme bağlamını taşır.

Application Insights SDK 'sının en son sürümü, Trace-Context protokolünü destekler, ancak bunu kabul etmeniz gerekebilir. (Application Insights SDK tarafından desteklenen önceki Bağıntı protokolü ile geriye dönük uyumluluk korunur.)

[Istek kimliği olarak da bilinen BAĞıNTı http Protokolü](https://github.com/dotnet/runtime/blob/master/src/libraries/System.Diagnostics.DiagnosticSource/src/HttpCorrelationProtocol.md)kullanım dışı bırakılıyor. Bu protokol iki üstbilgiyi tanımlar:

- `Request-Id`: Çağrının genel benzersiz KIMLIĞINI taşır.
- `Correlation-Context`: Dağıtılmış izleme özelliklerinin ad-değer çiftleri koleksiyonunu taşır.

Application Insights ayrıca bağıntı HTTP protokolünün [uzantısını](https://github.com/lmolkova/correlation/blob/master/http_protocol_proposal_v2.md) tanımlar. `Request-Context`Anında çağıran veya çağrılan tarafından kullanılan özellik koleksiyonunu yaymak için ad-değer çiftlerini kullanır. Application Insights SDK bu üstbilgiyi ve alanlarını ayarlamak için kullanır `dependency.target` `request.source` .

### <a name="enable-w3c-distributed-tracing-support-for-classic-aspnet-apps"></a>Klasik ASP.NET uygulamaları için W3C dağıtılmış izleme desteğini etkinleştir
 
  > [!NOTE]
  >  Ve ile `Microsoft.ApplicationInsights.Web` başlayarak `Microsoft.ApplicationInsights.DependencyCollector` yapılandırma gerekmez.

W3C Trace-Context desteği, geriye dönük olarak uyumlu bir şekilde uygulanır. Bağıntı, SDK 'nın önceki sürümleriyle (W3C desteği olmadan) işaretlenmiş uygulamalarla çalışması beklenir.

Eski Protokolü kullanmaya devam etmek istiyorsanız `Request-Id` , bu yapılandırmayı kullanarak Izleme bağlamını devre dışı bırakabilirsiniz:

```csharp
  Activity.DefaultIdFormat = ActivityIdFormat.Hierarchical;
  Activity.ForceDefaultIdFormat = true;
```

SDK 'nın eski bir sürümünü çalıştırırsanız, bunu güncelleştirmenizi veya Izleme bağlamını etkinleştirmek için aşağıdaki yapılandırmayı uygulamanızı öneririz.
Bu özellik, `Microsoft.ApplicationInsights.Web` ve `Microsoft.ApplicationInsights.DependencyCollector` paketlerinde 2.8.0-Beta1 sürümünden başlayarak kullanılabilir.
Varsayılan olarak devre dışıdır. Etkinleştirmek için şu değişiklikleri yapın `ApplicationInsights.config` :

- Altında `RequestTrackingTelemetryModule` , öğesini ekleyin `EnableW3CHeadersExtraction` ve değerini olarak ayarlayın `true` .
- Altında `DependencyTrackingTelemetryModule` , öğesini ekleyin `EnableW3CHeadersInjection` ve değerini olarak ayarlayın `true` .
- `W3COperationCorrelationTelemetryInitializer`Altına ekleyin `TelemetryInitializers` . Bu örneğe benzer şekilde görünür:

```xml
<TelemetryInitializers>
  <Add Type="Microsoft.ApplicationInsights.Extensibility.W3C.W3COperationCorrelationTelemetryInitializer, Microsoft.ApplicationInsights"/>
   ...
</TelemetryInitializers>
```

### <a name="enable-w3c-distributed-tracing-support-for-aspnet-core-apps"></a>ASP.NET Core uygulamalar için W3C dağıtılmış izleme desteğini etkinleştir

 > [!NOTE]
  > `Microsoft.ApplicationInsights.AspNetCore`Sürüm 2.8.0 ile başlayarak yapılandırma gerekmez.
 
W3C Trace-Context desteği, geriye dönük olarak uyumlu bir şekilde uygulanır. Bağıntı, SDK 'nın önceki sürümleriyle (W3C desteği olmadan) işaretlenmiş uygulamalarla çalışması beklenir.

Eski Protokolü kullanmaya devam etmek istiyorsanız `Request-Id` , bu yapılandırmayı kullanarak Izleme bağlamını devre dışı bırakabilirsiniz:

```csharp
  Activity.DefaultIdFormat = ActivityIdFormat.Hierarchical;
  Activity.ForceDefaultIdFormat = true;
```

SDK 'nın eski bir sürümünü çalıştırırsanız, bunu güncelleştirmenizi veya Izleme bağlamını etkinleştirmek için aşağıdaki yapılandırmayı uygulamanızı öneririz.

Bu özellik `Microsoft.ApplicationInsights.AspNetCore` 2.5.0-Beta1 sürümünde ve `Microsoft.ApplicationInsights.DependencyCollector` 2.8.0-Beta1 sürümünde bulunur.
Varsayılan olarak devre dışıdır. Etkinleştirmek için şu şekilde ayarlayın `ApplicationInsightsServiceOptions.RequestCollectionOptions.EnableW3CDistributedTracing` `true` :

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddApplicationInsightsTelemetry(o => 
        o.RequestCollectionOptions.EnableW3CDistributedTracing = true );
    // ....
}
```

### <a name="enable-w3c-distributed-tracing-support-for-java-apps"></a>Java uygulamaları için W3C dağıtılmış izleme desteğini etkinleştir

#### <a name="java-30-agent"></a>Java 3,0 Aracısı

  Java 3,0 Aracısı, W3C 'ı kutudan çıkar ve ek yapılandırma gerekmez. 

#### <a name="java-sdk"></a>Java SDK
- **Gelen yapılandırma**

  - Java EE uygulamaları için aşağıdaki `<TelemetryModules>` etikete ApplicationInsights.xml ekleyin:

    ```xml
    <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebRequestTrackingTelemetryModule>
       <Param name = "W3CEnabled" value ="true"/>
       <Param name ="enableW3CBackCompat" value = "true" />
    </Add>
    ```
    
  - Spring Boot uygulamaları için şu özellikleri ekleyin:

    - `azure.application-insights.web.enable-W3C=true`
    - `azure.application-insights.web.enable-W3C-backcompat-mode=true`

- **Giden yapılandırma**

  Aşağıdakileri AI-Agent.xml ekleyin:

  ```xml
  <Instrumentation>
    <BuiltIn enabled="true">
      <HTTP enabled="true" W3C="true" enableW3CBackCompat="true"/>
    </BuiltIn>
  </Instrumentation>
  ```

  > [!NOTE]
  > Geriye dönük uyumluluk modu varsayılan olarak etkindir ve `enableW3CBackCompat` parametresi isteğe bağlıdır. Bunu yalnızca geriye dönük uyumluluğu kapatmak istediğinizde kullanın.
  >
  > İdeal olarak, tüm hizmetleriniz W3C protokolünü destekleyen SDK 'ların daha yeni sürümlerine güncelleştirildiği zaman bunu devre dışı bırakabilirsiniz. Bu yeni SDK 'lara mümkün olan en kısa sürede geçiş yapmanızı önemle tavsiye ederiz.

> [!IMPORTANT]
> Gelen ve giden yapılandırmaların tam olarak aynı olduğundan emin olun.

### <a name="enable-w3c-distributed-tracing-support-for-web-apps"></a>Web Apps için W3C dağıtılmış izleme desteğini etkinleştir

Bu özellik ' de bulunur `Microsoft.ApplicationInsights.JavaScript` . Varsayılan olarak devre dışıdır. Etkinleştirmek için, config kullanın `distributedTracingMode` . AI_AND_W3C, Application Insights tarafından işaretlenmiş eski hizmetlerle geriye dönük uyumluluk için sağlanır.

- **NPM kurulumu (kod parçacığı kurulumu kullanılıyorsa yoksay)**

  ```javascript
  import { ApplicationInsights, DistributedTracingModes } from '@microsoft/applicationinsights-web';

  const appInsights = new ApplicationInsights({ config: {
    instrumentationKey: 'YOUR_INSTRUMENTATION_KEY_GOES_HERE',
    distributedTracingMode: DistributedTracingModes.W3C
    /* ...other configuration options... */
  } });
  appInsights.loadAppInsights();
  ```
  
- **Kod parçacığı kurulumu (NPM kurulumu kullanılıyorsa yoksay)**

  ```
  <script type="text/javascript">
  var sdkInstance="appInsightsSDK";window[sdkInstance]="appInsights";var aiName=window[sdkInstance],aisdk=window[aiName]||function(e){function n(e){i[e]=function(){var n=arguments;i.queue.push(function(){i[e].apply(i,n)})}}var i={config:e};i.initialize=!0;var a=document,t=window;setTimeout(function(){var n=a.createElement("script");n.src=e.url||"https://az416426.vo.msecnd.net/scripts/b/ai.2.min.js",a.getElementsByTagName("script")[0].parentNode.appendChild(n)});try{i.cookie=a.cookie}catch(e){}i.queue=[],i.version=2;for(var r=["Event","PageView","Exception","Trace","DependencyData","Metric","PageViewPerformance"];r.length;)n("track"+r.pop());n("startTrackPage"),n("stopTrackPage");var o="Track"+r[0];if(n("start"+o),n("stop"+o),!(!0===e.disableExceptionTracking||e.extensionConfig&&e.extensionConfig.ApplicationInsightsAnalytics&&!0===e.extensionConfig.ApplicationInsightsAnalytics.disableExceptionTracking)){n("_"+(r="onerror"));var s=t[r];t[r]=function(e,n,a,t,o){var c=s&&s(e,n,a,t,o);return!0!==c&&i["_"+r]({message:e,url:n,lineNumber:a,columnNumber:t,error:o}),c},e.autoExceptionInstrumented=!0}return i}
  (
    {
      instrumentationKey:"INSTRUMENTATION_KEY",
      distributedTracingMode: 2 // DistributedTracingModes.W3C
      /* ...other configuration options... */
    }
  );
  window[aiName]=aisdk,aisdk.queue&&0===aisdk.queue.length&&aisdk.trackPageView({});
  </script>
  ```

## <a name="opentracing-and-application-insights"></a>OpenTracing ve Application Insights

[Opentracing veri modeli belirtimi](https://opentracing.io/) ve Application Insights veri modelleri aşağıdaki şekilde eşlenir:

| Application Insights                   | OpenTracing                                        |
|------------------------------------    |-------------------------------------------------    |
| `Request`, `PageView`                  | `Span`kullanılarak`span.kind = server`                    |
| `Dependency`                           | `Span`kullanılarak`span.kind = client`                    |
| `Id``Request`ve`Dependency`     | `SpanId`                                            |
| `Operation_Id`                         | `TraceId`                                           |
| `Operation_ParentId`                   | `Reference`türündeki `ChildOf` (üst yayılma)     |

Daha fazla bilgi için bkz. [telemetri veri modeli Application Insights](./data-model.md).

OpenTracing kavramlarının tanımları için bkz. OpenTracing [belirtimi](https://github.com/opentracing/specification/blob/master/specification.md) ve [anlam kuralları](https://github.com/opentracing/specification/blob/master/semantic_conventions.md).

## <a name="telemetry-correlation-in-opencensus-python"></a>OpenCensus Python 'da telemetri bağıntısı

OpenCensus Python, `OpenTracing` daha önce özetlenen veri modeli belirtimlerini izler. Ayrıca, herhangi bir yapılandırma gerektirmeden [W3C Trace-Context](https://w3c.github.io/trace-context/) ' i destekler.

### <a name="incoming-request-correlation"></a>Gelen istek bağıntısı

OpenCensus Python, W3C Trace-Context üst bilgilerini gelen isteklerden, isteklerden oluşturulan yayılmaya göre ilişkilendirir. OpenCensus, bu popüler web uygulaması çerçeveleri için tümleştirmelerle otomatik olarak yapılır: Flask, Docgo ve piramit. Yalnızca W3C Trace-Context üst bilgilerini [doğru biçimle](https://www.w3.org/TR/trace-context/#trace-context-http-headers-format) doldurmanız ve istekle birlikte göndermeniz gerekir. Bunu gösteren örnek bir Flask uygulaması aşağıda verilmiştir:

```python
from flask import Flask
from opencensus.ext.azure.trace_exporter import AzureExporter
from opencensus.ext.flask.flask_middleware import FlaskMiddleware
from opencensus.trace.samplers import ProbabilitySampler

app = Flask(__name__)
middleware = FlaskMiddleware(
    app,
    exporter=AzureExporter(),
    sampler=ProbabilitySampler(rate=1.0),
)

@app.route('/')
def hello():
    return 'Hello World!'

if __name__ == '__main__':
    app.run(host='localhost', port=8080, threaded=True)
```

Bu kod, yerel makinenizde bir örnek Flask uygulaması çalıştırır ve bağlantı noktasını dinler `8080` . İzleme bağlamını ilişkilendirmek için uç noktaya bir istek gönderirsiniz. Bu örnekte, bir `curl` komut kullanabilirsiniz:
```
curl --header "traceparent: 00-4bf92f3577b34da6a3ce929d0e0e4736-00f067aa0ba902b7-01" localhost:8080
```
[Trace-Context üst bilgi biçimine](https://www.w3.org/TR/trace-context/#trace-context-http-headers-format)bakarak aşağıdaki bilgileri türetebilirsiniz:

`version`: `00`

`trace-id`: `4bf92f3577b34da6a3ce929d0e0e4736`

`parent-id/span-id`: `00f067aa0ba902b7`

`trace-flags`: `01`

Azure Izleyici 'ye gönderilen istek girişine bakarsanız, izleme üst bilgisi bilgileriyle doldurulmuş alanları görebilirsiniz. Bu verileri Azure Izleyici Application Insights kaynağında Günlükler (Analiz) altında bulabilirsiniz.

![Günlüklerde telemetri isteme (Analiz)](./media/opencensus-python/0011-correlation.png)

`id`Alanı, `<trace-id>.<span-id>` `trace-id` istek içinde geçirilen izleme başlığından alındığı ve `span-id` Bu yayılma için oluşturulan 8 baytlık bir dizi olan biçimindedir.

`operation_ParentId`Alanı, `<trace-id>.<parent-id>` hem hem de ' ın `trace-id` `parent-id` istekte geçirilen izleme başlığından alındığı biçimdedir.

### <a name="log-correlation"></a>Günlük bağıntısı

OpenCensus Python, kayıtları günlüğe kaydetmek için bir izleme KIMLIĞI, bir span ID ve örnekleme bayrağı ekleyerek günlüklerinizi ilişkilendirmenize olanak sağlar. Bu öznitelikleri, OpenCensus [günlük tümleştirmesi](https://pypi.org/project/opencensus-ext-logging/)' ni yükleyerek eklersiniz. Aşağıdaki öznitelikler Python `LogRecord` nesnelerine eklenecektir: `traceId` , `spanId` , ve `traceSampled` . Bu, yalnızca tümleştirmeden sonra oluşturulan Günlükçüler için geçerli olduğunu unutmayın.

Bunu gösteren örnek bir uygulama aşağıda verilmiştir:

```python
import logging

from opencensus.trace import config_integration
from opencensus.trace.samplers import AlwaysOnSampler
from opencensus.trace.tracer import Tracer

config_integration.trace_integrations(['logging'])
logging.basicConfig(format='%(asctime)s traceId=%(traceId)s spanId=%(spanId)s %(message)s')
tracer = Tracer(sampler=AlwaysOnSampler())

logger = logging.getLogger(__name__)
logger.warning('Before the span')
with tracer.span(name='hello'):
    logger.warning('In the span')
logger.warning('After the span')
```
Bu kod çalıştırıldığında, konsolunda aşağıdaki şekilde yazdırılır:
```
2019-10-17 11:25:59,382 traceId=c54cb1d4bbbec5864bf0917c64aeacdc spanId=0000000000000000 Before the span
2019-10-17 11:25:59,384 traceId=c54cb1d4bbbec5864bf0917c64aeacdc spanId=70da28f5a4831014 In the span
2019-10-17 11:25:59,385 traceId=c54cb1d4bbbec5864bf0917c64aeacdc spanId=0000000000000000 After the span
```
`spanId`Yayılma alanındaki günlük iletisi için bir mevcut olduğuna dikkat edin. Bu, `spanId` adlı yayılmasına ait olan aynıdır `hello` .

Kullanarak günlük verilerini dışa aktarabilirsiniz `AzureLogHandler` . Daha fazla bilgi için [Bu makaleye](./opencensus-python.md#logs)bakın.

## <a name="telemetry-correlation-in-net"></a>.NET 'te telemetri bağıntısı

Zaman içinde, .NET telemetri ve tanılama günlüklerini ilişkilendirmek için çeşitli yollar tanımladı:

- `System.Diagnostics.CorrelationManager`[LogicalOperationStack ve ActivityId](/dotnet/api/system.diagnostics.correlationmanager?view=netcore-3.1)'nin izlenmesine izin verir.
- `System.Diagnostics.Tracing.EventSource`ve Windows için olay Izleme (ETW) [SetCurrentThreadActivityId](/dotnet/api/system.diagnostics.tracing.eventsource.setcurrentthreadactivityid?view=netcore-3.1#overloads) metodunu tanımlar.
- `ILogger`[günlük kapsamlarını](/aspnet/core/fundamentals/logging#log-scopes)kullanır.
- Windows Communication Foundation (WCF) ve HTTP dağıtımı "geçerli" bağlam yayma.

Ancak bu yöntemler otomatik dağıtılmış izleme desteğini etkinleştirmedi. `DiagnosticSource`, otomatik makine çapraz bağıntısını destekler. .NET kitaplıkları, `DiagnosticSource` http gibi bir aktarım aracılığıyla bağıntı bağlamının otomatik makine çapraz olarak yayılmasını destekler ve bunlara izin verir.

İçindeki [etkinlik Kullanıcı Kılavuzu](https://github.com/dotnet/runtime/blob/master/src/libraries/System.Diagnostics.DiagnosticSource/src/ActivityUserGuide.md) , `DiagnosticSource` etkinliklerin izlenmesi hakkında temel bilgileri açıklar.

ASP.NET Core 2,0, HTTP üstbilgilerinin ayıklanmasını ve yeni etkinlikler başlatmasını destekler.

`System.Net.Http.HttpClient`sürüm 4.1.0 ile başlayarak, bağıntı HTTP üst bilgilerinin otomatik olarak eklenmesine ve HTTP çağrılarının etkinlik olarak izlenmesini destekler.

Klasik ASP.NET için [Microsoft. Aspnet. TelemetryCorrelation](https://www.nuget.org/packages/Microsoft.AspNet.TelemetryCorrelation/)yenı bir http modülü vardır. Bu modül kullanarak telemetri bağıntısını uygular `DiagnosticSource` . Gelen istek üst bilgilerine göre bir etkinlik başlatır. Ayrıca, her bir Internet Information Services (IIS) işleminin farklı bir yönetilen iş parçacığında çalışması durumunda bile, farklı istek işleme aşamalarından Telemetriyi de ilişkilendirir.

2.4.0-Beta1 sürümünden başlayarak Application Insights SDK, `DiagnosticSource` `Activity` telemetri toplamak ve geçerli etkinlikle ilişkilendirmek için ve kullanır.

<a name="java-correlation"></a>
## <a name="telemetry-correlation-in-java"></a>Java 'da telemetri bağıntısı

Java [aracısının](./java-in-process-agent.md) yanı sıra [Java SDK](../../azure-monitor/app/java-get-started.md) sürümü 2.0.0 veya üzeri, telemetri otomatik bağıntısını destekler. `operation_id`Bir isteğin kapsamında verilen tüm telemetri (izlemeler, özel durumlar ve özel olaylar gibi) otomatik olarak doldurulur. [Java SDK Aracısı](../../azure-monitor/app/java-agent.md) yapılandırılmışsa, hizmet-hizmet çağrıları için de bağıntı üst bilgilerini (daha önce AÇıKLANAN) http aracılığıyla yayar.

> [!NOTE]
> Application Insights Java Aracısı, JMS, Kafka, netty/Webflox ve daha fazlası için istekleri ve bağımlılıkları otomatik olarak toplar. Java SDK için yalnızca Apache HttpClient aracılığıyla yapılan çağrılar bağıntı özelliği için desteklenir. (Kafka, Kbbitmq ve Azure Service Bus gibi) mesajlaşma teknolojileri genelinde otomatik bağlam yayma SDK 'da desteklenmez. 

> [!NOTE]
> Özel telemetri toplamak için, uygulamayı Java 2,6 SDK 'Sı ile birlikte belirlemeniz gerekir. 

### <a name="role-names"></a>Rol adları

[Uygulama eşlemesinde](../../azure-monitor/app/app-map.md)bileşen adlarının görüntülenme şeklini özelleştirmek isteyebilirsiniz. Bunu yapmak için, `cloud_RoleName` aşağıdaki eylemlerden birini gerçekleştirerek el ile ayarlayabilirsiniz:

- Application Insights Java Aracısı 3,0 için, bulut rolü adını şu şekilde ayarlayın:

    ```json
    {
      "instrumentationSettings": {
        "preview": {
          "roleName": "my cloud role name"
        }
      }
    }
    ```
    Ayrıca, ortam değişkenini kullanarak bulut rolü adını da ayarlayabilirsiniz `APPLICATIONINSIGHTS_ROLE_NAME` .

- Application Insights Java SDK 2.5.0 ve üzeri ile `cloud_RoleName` `<RoleName>` ApplicationInsights.xml dosyanıza ekleyerek belirtebilirsiniz:

  ```XML
  <?xml version="1.0" encoding="utf-8"?>
  <ApplicationInsights xmlns="http://schemas.microsoft.com/ApplicationInsights/2013/Settings" schemaVersion="2014-05-30">
     <InstrumentationKey>** Your instrumentation key **</InstrumentationKey>
     <RoleName>** Your role name **</RoleName>
     ...
  </ApplicationInsights>
  ```

- Spring Boot uygulamasını Application Insights Spring Boot Starter ile birlikte kullanıyorsanız, uygulama için özel adınızı uygulama. Özellikler dosyasında ayarlamanız yeterlidir:

  `spring.application.name=<name-of-app>`

  Spring Boot Starter, `cloudRoleName` özelliği için girdiğiniz değere otomatik olarak atar `spring.application.name` .

## <a name="next-steps"></a>Sonraki adımlar

- [Özel telemetri](./api-custom-events-metrics.md)yazın.
- ASP.NET Core ve ASP.NET ' deki gelişmiş bağıntı senaryoları için bkz. [özel Işlemleri izleme](custom-operations-tracking.md).
- Diğer SDK 'lar için [cloud_RoleName ayarlama](./app-map.md#set-cloud-role-name) hakkında daha fazla bilgi edinin.
- Mikro hizmetinizin tüm bileşenlerini Application Insights ekleyin. [Desteklenen platformları](./platforms.md)inceleyin.
- Application Insights türleri için [veri modeline](./data-model.md) bakın.
- [Telemetrinin nasıl genişletileceğini ve filtreleneceğini](./api-filtering-sampling.md)öğrenin.
- [Application Insights config başvurusunu](configuration-with-applicationinsights-config.md)gözden geçirin.

