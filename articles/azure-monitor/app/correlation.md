---
title: Azure Uygulama Öngörüleri telemetri korelasyon | Microsoft Dokümanlar
description: Uygulama Öngörüleri telemetri korelasyon
ms.topic: conceptual
author: lgayhardt
ms.author: lagayhar
ms.date: 06/07/2019
ms.reviewer: sergkanz
ms.openlocfilehash: 2e862410e2bf12e09e1a6388bbb6f7105b5b2edf
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81405261"
---
# <a name="telemetry-correlation-in-application-insights"></a>Uygulama Öngörülerinde Telemetri korelasyon

Mikro hizmetler dünyasında, her mantıksal işlem hizmetin çeşitli bileşenlerinde yapılması gereken işleri gerektirir. [Uygulama Öngörüleri'ni](../../azure-monitor/app/app-insights-overview.md)kullanarak bu bileşenlerin her birini ayrı ayrı izleyebilirsiniz. Application Insights, hatalardan veya performans düşüşünden hangi bileşenin sorumlu olduğunu algılamak için kullandığınız dağıtılmış telemetri korelasyonunu destekler.

Bu makalede, birden çok bileşen tarafından gönderilen telemetri ilişkilendirmek için Application Insights tarafından kullanılan veri modeli açıklanmaktadır. Bağlam yayılımı tekniklerini ve protokollerini kapsar. Ayrıca farklı dil ve platformlarda korelasyon taktiklerinin uygulanmasını da kapsamaktadır.

## <a name="data-model-for-telemetry-correlation"></a>Telemetri korelasyon için veri modeli

Application Insights dağıtılmış telemetri korelasyon için bir [veri modeli](../../azure-monitor/app/data-model.md) tanımlar. Telemetriyi mantıksal bir işlemle ilişkilendirmek için, her `operation_Id`telemetri öğesinin bir bağlam alanı vardır. Bu tanımlayıcı, dağıtılmış izlemedeki her telemetri öğesi tarafından paylaşılır. Bu nedenle, tek bir katmandan telemetri yitirseniz bile, diğer bileşenler tarafından bildirilen telemetriyi ilişkilendirebilirsiniz.

Dağıtılmış mantıksal işlem genellikle bileşenlerden biri tarafından işlenen istekler küçük işlemler kümesinden oluşur. Bu işlemler [istek telemetrisi](../../azure-monitor/app/data-model-request-telemetry.md)ile tanımlanır. Her istek telemetri öğesi `id` benzersiz ve küresel olarak tanımlayan kendi vardır. Ve istekle ilişkili tüm telemetri öğeleri (izlemeler ve özel durumlar `operation_parentId` gibi) isteğin `id`değerini ayarlamalıdır.

Başka bir bileşene http çağrısı gibi her giden işlem bağımlılık [telemetrisi](../../azure-monitor/app/data-model-dependency-telemetry.md)ile temsil edilir. Bağımlılık telemetri de küresel `id` benzersiz kendi tanımlar. İstek telemetri, bu bağımlılık çağrısı tarafından `id` başlatılan, `operation_parentId`onun olarak kullanır.

Dağıtılmış mantıksal işlemin `operation_Id`görünümünü kullanarak `operation_parentId`ve `request.id` . `dependency.id` Bu alanlar aynı zamanda telemetri çağrılarının nedensellik sırasını da tanımlar.

Mikro hizmetler ortamında, bileşenlerden gelen izlemeler farklı depolama öğelerine gidebilir. Uygulama Öngörüleri'nde her bileşenin kendi enstrümantasyon anahtarı olabilir. Mantıksal işlem için telemetri almak için Application Insights her depolama öğesinden verileri sorgular. Depolama öğelerinin sayısı büyük olduğunda, bir sonraki aramanın nerelere bakacağı hakkında bir ipucuna ihtiyacınız vardır. Application Insights veri modeli, bu sorunu çözmek `request.source` `dependency.target`için iki alan tanımlar: ve . İlk alan, bağımlılık isteğini başlatan bileşeni tanımlar. İkinci alan, bağımlılık çağrısının yanıtını hangi bileşenin döndürtünlerini tanımlar.

## <a name="example"></a>Örnek

Şimdi örneği inceleyelim. Hisse Senedi Fiyatları adlı bir uygulama, Hisse Senedi adı verilen harici bir API kullanarak bir hisse senedinin geçerli piyasa fiyatını gösterir. Stok Fiyatları uygulaması, istemci web tarayıcısı kullanarak `GET /Home/Stock`açtığı Stok sayfası olarak adlandırılan bir sayfa vardır. Uygulama, HTTP çağrısını `GET /api/stock/value`kullanarak Stok API'sını sorgular.

Bir sorgu çalıştırarak ortaya çıkan telemetriyi çözümleyebilirsiniz:

```kusto
(requests | union dependencies | union pageViews)
| where operation_Id == "STYz"
| project timestamp, itemType, name, id, operation_ParentId, operation_Id
```

Sonuçlarda, tüm telemetri öğelerinin kökü `operation_Id`paylaştığını unutmayın. Bir Ajax çağrısı sayfadan yapıldığında, bağımlılık telemetrisine yeni bir benzersiz kimlik`qJSXU`atanır ve pageView kimliği `operation_ParentId`. Sunucu isteği daha sonra Ajax `operation_ParentId`KIMLIĞINI .

| ıtemtype   | ad                      | Kimlik           | operation_ParentId | operation_Id |
|------------|---------------------------|--------------|--------------------|--------------|
| sayfaGörünüm   | Stok sayfası                |              | STYz               | STYz         |
| Bağımlılık | GET /Ana Sayfa/Stok           | qJSXU        | STYz               | STYz         |
| istek    | GET Home/Stok            | KqKwlrSt9PA= | qJSXU              | STYz         |
| Bağımlılık | GET /api/stok/değer      | bBrf2L7mm2g= | KqKwlrSt9PA=       | STYz         |

Dış hizmete arama `GET /api/stock/value` yapıldığında, `dependency.target` alanı uygun şekilde ayarlayabilmeniz için sunucunun kimliğini bilmeniz gerekir. Dış hizmet izlemeyi desteklemediğinde, `target` hizmetin ana adı olarak ayarlanır (örneğin, `stock-prices-api.com`). Ancak, hizmet önceden tanımlanmış bir HTTP üstbilgisini `target` döndürerek kendisini tanımlarsa, Uygulama Öngörüleri'nin bu hizmetten telemetri sorgulayarak dağıtılmış bir izleme oluşturmasına olanak tanıyan hizmet kimliğini içerir.

## <a name="correlation-headers"></a>Korelasyon üsterleri

Uygulama Öngörüleri [W3C İzleme](https://w3c.github.io/trace-context/)Bağlamı'na geçiş yapmaktaolup, aşağıdakileri tanımlar:

- `traceparent`: Aramanın genel olarak benzersiz çalışma kimliğini ve benzersiz tanımlayıcısını taşır.
- `tracestate`: Sisteme özgü izleme bağlamı taşır.

Uygulama Öngörüleri SDK'nın en son sürümü İzleme Bağlamı protokolünü destekler, ancak bunu kabul etmeniz gerekebilir. (Application Insights SDK tarafından desteklenen önceki korelasyon protokolü ile geriye dönük uyumluluk korunacaktır.)

[İstek-Id olarak da adlandırılan http protokolü,](https://github.com/dotnet/runtime/blob/master/src/libraries/System.Diagnostics.DiagnosticSource/src/HttpCorrelationProtocol.md)amortismana neden oluyor. Bu protokol iki üstbilgi tanımlar:

- `Request-Id`: Aramanın genel olarak benzersiz kimliğini taşır.
- `Correlation-Context`: Dağıtılmış izleme özelliklerinin ad değeri çiftleri koleksiyonunu taşır.

Application Insights, http protokolü korelasyonun [adasını](https://github.com/lmolkova/correlation/blob/master/http_protocol_proposal_v2.md) da tanımlar. Hemen `Request-Context` arayan veya arayan tarafından kullanılan özelliklerin koleksiyonunu yaymak için ad değeri çiftleri kullanır. Application Insights SDK, alanları `dependency.target` ve `request.source` alanları ayarlamak için bu üstbilgikullanır.

### <a name="enable-w3c-distributed-tracing-support-for-classic-aspnet-apps"></a>Klasik ASP.NET uygulamaları için W3C dağıtılmış izleme desteğini etkinleştirme
 
  > [!NOTE]
  >  Ile `Microsoft.ApplicationInsights.Web` başlayan `Microsoft.ApplicationInsights.DependencyCollector`ve , hiçbir yapılandırma gereklidir.

W3C İzleme Bağlamı desteği geriye dönük uyumlu bir şekilde uygulanır. KorelasyonSDK önceki sürümleri (W3C desteği olmadan) ile enstrümante olan uygulamalar ile çalışması bekleniyor.

Eski `Request-Id` iletişim kuralını kullanmaya devam etmek istiyorsanız, bu yapılandırmayı kullanarak İzleme Bağlamını devre dışı kullanabilirsiniz:

```csharp
  Activity.DefaultIdFormat = ActivityIdFormat.Hierarchical;
  Activity.ForceDefaultIdFormat = true;
```

SDK'nın eski bir sürümünü çalıştırıyorsanız, Izleme İçeriği'ni etkinleştirmek için güncelleştirmenizi veya aşağıdaki yapılandırmayı uygulamanızı öneririz.
Bu özellik sürüm `Microsoft.ApplicationInsights.Web` 2.8.0-beta1 ile başlayan ve `Microsoft.ApplicationInsights.DependencyCollector` paketlerde mevcuttur.
Varsayılan olarak devre dışı bırakılır. Etkinleştirmek için şu değişiklikleri `ApplicationInsights.config`yapın:

- Altında, `RequestTrackingTelemetryModule`öğeyi `EnableW3CHeadersExtraction` ekleyin ve `true`değerini ayarlayın.
- Altında, `DependencyTrackingTelemetryModule`öğeyi `EnableW3CHeadersInjection` ekleyin ve `true`değerini ayarlayın.
- Altında `W3COperationCorrelationTelemetryInitializer` `TelemetryInitializers`ekle . Bu örnek benzer görünecektir:

```xml
<TelemetryInitializers>
  <Add Type="Microsoft.ApplicationInsights.Extensibility.W3C.W3COperationCorrelationTelemetryInitializer, Microsoft.ApplicationInsights"/>
   ...
</TelemetryInitializers>
```

### <a name="enable-w3c-distributed-tracing-support-for-aspnet-core-apps"></a>ASP.NET Core uygulamaları için W3C dağıtılmış izleme desteğini etkinleştirme

 > [!NOTE]
  > Sürüm `Microsoft.ApplicationInsights.AspNetCore` 2.8.0 ile başlayarak yapılandırma gerekmez.
 
W3C İzleme Bağlamı desteği geriye dönük uyumlu bir şekilde uygulanır. KorelasyonSDK önceki sürümleri (W3C desteği olmadan) ile enstrümante olan uygulamalar ile çalışması bekleniyor.

Eski `Request-Id` iletişim kuralını kullanmaya devam etmek istiyorsanız, bu yapılandırmayı kullanarak İzleme Bağlamını devre dışı kullanabilirsiniz:

```csharp
  Activity.DefaultIdFormat = ActivityIdFormat.Hierarchical;
  Activity.ForceDefaultIdFormat = true;
```

SDK'nın eski bir sürümünü çalıştırıyorsanız, Izleme İçeriği'ni etkinleştirmek için güncelleştirmenizi veya aşağıdaki yapılandırmayı uygulamanızı öneririz.

Bu özellik `Microsoft.ApplicationInsights.AspNetCore` sürüm 2.5.0-beta1 `Microsoft.ApplicationInsights.DependencyCollector` ve sürüm 2.8.0-beta1'dir.
Varsayılan olarak devre dışı bırakılır. Etkinleştirmek `ApplicationInsightsServiceOptions.RequestCollectionOptions.EnableW3CDistributedTracing` için: `true`

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddApplicationInsightsTelemetry(o => 
        o.RequestCollectionOptions.EnableW3CDistributedTracing = true );
    // ....
}
```

### <a name="enable-w3c-distributed-tracing-support-for-java-apps"></a>Java uygulamaları için W3C dağıtılmış izleme desteğini etkinleştirme

#### <a name="java-30-agent"></a>Java 3.0 aracısı

  Java 3.0 aracısı W3C'yi kutunun dışında destekler ve ek yapılandırma gerekmez. 

#### <a name="java-sdk"></a>Java SDK
- **Gelen yapılandırma**

  - Java EE uygulamaları için ApplicationInsights.xml'deki etikete `<TelemetryModules>` aşağıdakileri ekleyin:

    ```xml
    <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebRequestTrackingTelemetryModule>
       <Param name = "W3CEnabled" value ="true"/>
       <Param name ="enableW3CBackCompat" value = "true" />
    </Add>
    ```
    
  - Bahar Önyükleme uygulamaları için şu özellikleri ekleyin:

    - `azure.application-insights.web.enable-W3C=true`
    - `azure.application-insights.web.enable-W3C-backcompat-mode=true`

- **Giden yapılandırma**

  AI-Agent.xml'e aşağıdakileri ekleyin:

  ```xml
  <Instrumentation>
    <BuiltIn enabled="true">
      <HTTP enabled="true" W3C="true" enableW3CBackCompat="true"/>
    </BuiltIn>
  </Instrumentation>
  ```

  > [!NOTE]
  > Geriye dönük uyumluluk modu varsayılan olarak `enableW3CBackCompat` etkinleştirilir ve parametre isteğe bağlıdır. Yalnızca geriye dönük uyumluluğu kapatmak istediğinizde kullanın.
  >
  > İdeal olarak, tüm hizmetleriniz W3C protokolünü destekleyen SDK'ların yeni sürümlerine güncelleştirildiğinde bunu kapatırsınız. Bu yeni SDK'lara en kısa sürede geçmenizi şiddetle öneririz.

> [!IMPORTANT]
> Gelen ve giden yapılandırmaların tamamen aynı olduğundan emin olun.

### <a name="enable-w3c-distributed-tracing-support-for-web-apps"></a>Web uygulamaları için W3C dağıtılmış izleme desteğini etkinleştirme

Bu özellik `Microsoft.ApplicationInsights.JavaScript`. Varsayılan olarak devre dışı bırakılır. Etkinleştirmek için `distributedTracingMode` config kullanın. AI_AND_W3C, Application Insights tarafından sunulan eski hizmetlerle geriye dönük uyumluluk için sağlanır.

- **npm kurulumu (Snippet kurulum kullanıyorsanız yoksay)**

  ```javascript
  import { ApplicationInsights, DistributedTracingModes } from '@microsoft/applicationinsights-web';

  const appInsights = new ApplicationInsights({ config: {
    instrumentationKey: 'YOUR_INSTRUMENTATION_KEY_GOES_HERE',
    distributedTracingMode: DistributedTracingModes.W3C
    /* ...other configuration options... */
  } });
  appInsights.loadAppInsights();
  ```
  
- **Snippet kurulumu (npm kurulumu kullanıyorsanız yoksay)**

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

## <a name="opentracing-and-application-insights"></a>OpenTracing ve Uygulama Öngörüleri

[OpenTracing veri modeli belirtimi](https://opentracing.io/) ve Application Insights veri modelleri aşağıdaki şekilde eşlendi:

| Application Insights                   | OpenTracing                                        |
|------------------------------------    |-------------------------------------------------    |
| `Request`, `PageView`                  | `Span`Ile`span.kind = server`                    |
| `Dependency`                           | `Span`Ile`span.kind = client`                    |
| `Id`ve `Request``Dependency`     | `SpanId`                                            |
| `Operation_Id`                         | `TraceId`                                           |
| `Operation_ParentId`                   | `Reference`türü `ChildOf` (üst açıklık)     |

Daha fazla bilgi için [Bkz. Application Insights telemetri veri modeli.](../../azure-monitor/app/data-model.md)

OpenTracing kavramlarının tanımları için OpenTracing [belirtimi](https://github.com/opentracing/specification/blob/master/specification.md) ve [anlamsal kuralları](https://github.com/opentracing/specification/blob/master/semantic_conventions.md)görün.

## <a name="telemetry-correlation-in-opencensus-python"></a>OpenCensus Python'da Telemetri korelasyon

OpenCensus Python `OpenTracing` daha önce özetlenen veri modeli belirtimlerini izler. Ayrıca herhangi bir yapılandırma gerektirmeden [W3C İzleme Bağlamı](https://w3c.github.io/trace-context/) destekler.

### <a name="incoming-request-correlation"></a>Gelen istek korelasyon

OpenCensus Python, W3C İzleme Bağlamı üstlelerini gelen isteklerden isteklerden gelen isteklerden gelen isteklerden oluşturulan yayılma lara ilişkilendirer. OpenCensus bu popüler web uygulama çerçeveleri için entegrasyonlar ile otomatik olarak yapacak: Flask, Django, ve Piramit. W3C İzleme Bağlamı üstbilgilerini [doğru biçimde](https://www.w3.org/TR/trace-context/#trace-context-http-headers-format) doldurmanız ve istekle birlikte göndermeniz gerekir. Burada bunu gösteren bir örnek Flask uygulaması:

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

Bu kod, bağlantı noktasını `8080`dinleyerek yerel makinenizde örnek bir Flask uygulamasını çalıştırın. İzleme bağlamını ilişkilendirmek için, bir isteği bitiş noktasına gönderirsiniz. Bu örnekte, bir `curl` komut kullanabilirsiniz:
```
curl --header "traceparent: 00-4bf92f3577b34da6a3ce929d0e0e4736-00f067aa0ba902b7-01" localhost:8080
```
[İzleme Bağlamı üstbilgisi biçimine](https://www.w3.org/TR/trace-context/#trace-context-http-headers-format)bakarak aşağıdaki bilgileri elde edebilirsiniz:

`version`: `00`

`trace-id`: `4bf92f3577b34da6a3ce929d0e0e4736`

`parent-id/span-id`: `00f067aa0ba902b7`

`trace-flags`: `01`

Azure Monitor'a gönderilen istek girişine bakarsanız, izleme üstbilgisi bilgileriyle dolu alanları görebilirsiniz. Bu verileri Azure Monitor Application Insights kaynağında Günlükler (Analytics) altında bulabilirsiniz.

![Günlüklerde telemetri isteği (Analytics)](./media/opencensus-python/0011-correlation.png)

Alan `id` biçiminde `<trace-id>.<span-id>`, `trace-id` istekte geçirilen izleme üstbilgisinden alınır ve bu yayılma `span-id` alanı için oluşturulan 8 baytlık bir dizidir.

Alan `operation_ParentId` biçiminde `<trace-id>.<parent-id>`, hem ve `trace-id` hem `parent-id` de istekte geçirilen izleme üstbilgialınır.

### <a name="log-correlation"></a>Günlük korelasyon

OpenCensus Python, günlük kayıtlarına bir izleme kimliği, bir yayılma kimliği ve bir örnekleme bayrağı ekleyerek günlükleri ilişkilendirmenizi sağlar. OpenCensus [günlük tümleştirme](https://pypi.org/project/opencensus-ext-logging/)yükleyerek bu öznitelikleri ekleyin. `LogRecord` Python nesnelerine aşağıdaki öznitelikler `traceId`eklenir: `spanId`, `traceSampled`, ve . Bunun yalnızca tümleştirmeden sonra oluşturulan loggers için etkili olduğunu unutmayın.

Bunu gösteren bir örnek uygulama aşağıda veda edebilirsiniz:

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
Bu kod çalıştığında, konsolda aşağıdaki yazdırılır:
```
2019-10-17 11:25:59,382 traceId=c54cb1d4bbbec5864bf0917c64aeacdc spanId=0000000000000000 Before the span
2019-10-17 11:25:59,384 traceId=c54cb1d4bbbec5864bf0917c64aeacdc spanId=70da28f5a4831014 In the span
2019-10-17 11:25:59,385 traceId=c54cb1d4bbbec5864bf0917c64aeacdc spanId=0000000000000000 After the span
```
Giriş iletisi `spanId` için açıklık süresi içinde bir hediye olduğuna dikkat edin. Bu, `spanId` adlı `hello`açıklığa ait aynıdır.

Günlük verilerini kullanarak `AzureLogHandler`dışa aktarabilirsiniz. Daha fazla bilgi için [bu makaleye](https://docs.microsoft.com/azure/azure-monitor/app/opencensus-python#logs)bakın.

## <a name="telemetry-correlation-in-net"></a>.NET'te telemetri korelasyon

Zaman içinde .NET, telemetri ve tanılama günlüklerini ilişkilendirmek için çeşitli yollar tanımlamıştır:

- `System.Diagnostics.CorrelationManager`[LogicalOperationStack ve ActivityId](https://msdn.microsoft.com/library/system.diagnostics.correlationmanager.aspx)izleme sağlar.
- `System.Diagnostics.Tracing.EventSource`ve Windows için Olay İzleme (ETW) [SetCurrentThreadActivityId](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.setcurrentthreadactivityid.aspx) yöntemini tanımlar.
- `ILogger`[Günlük kapsamlarını](https://docs.microsoft.com/aspnet/core/fundamentals/logging#log-scopes)kullanır.
- Windows Communication Foundation (WCF) ve HTTP kablo "geçerli" bağlam yayılımı.

Ancak bu yöntemler otomatik dağıtılmış izleme desteğine olanak sağlamadı. `DiagnosticSource`otomatik çapraz makine korelasyonuna destek vedestekler. .NET kitaplıkları, http gibi taşıma yoluyla korelasyon bağlamının otomatik olarak makineler arası yayılmasını destekler `DiagnosticSource` ve sağlar.

[İzleme](https://github.com/dotnet/runtime/blob/master/src/libraries/System.Diagnostics.DiagnosticSource/src/ActivityUserGuide.md) faaliyetlerinin `DiagnosticSource` temellerini açıklar.

ASP.NET Core 2.0, HTTP üstbilgilerinin çıkarılmasını ve yeni etkinliklerin başlatılmasını destekler.

`System.Net.Http.HttpClient`, sürüm 4.1.0 ile başlayarak, korelasyon HTTP başlıkları otomatik enjeksiyon ve faaliyetleri olarak HTTP aramaları izleme destekler.

Klasik ASP.NET için yeni bir HTTP modülü, [Microsoft.AspNet.TelemetryCorrelation](https://www.nuget.org/packages/Microsoft.AspNet.TelemetryCorrelation/)var. Bu modül kullanarak `DiagnosticSource`telemetri korelasyon uyguluyor. Gelen istek üstbilgilerine dayalı bir etkinlik başlatır. Ayrıca, Internet Information Services (IIS) işleme işleminin her aşaması farklı yönetilen bir iş parçacığı üzerinde çalıştığında bile, istek işlemenin farklı aşamalarından telemetriyi de ilişkilendirmektedir.

Uygulama Öngörüleri SDK, sürüm 2.4.0-beta1 `DiagnosticSource` ile `Activity` başlayan, kullanır ve telemetri toplamak ve mevcut etkinlik ile ilişkilendirmek.

<a name="java-correlation"></a>
## <a name="telemetry-correlation-in-java"></a>Java'da Telemetri korelasyon

[Java aracısı](https://docs.microsoft.com/azure/azure-monitor/app/java-in-process-agent) yanı sıra [Java SDK](../../azure-monitor/app/java-get-started.md) sürüm 2.0.0 veya daha sonra telemetri otomatik korelasyon destekler. Bir istek kapsamında `operation_id` verilen tüm telemetriler (izlemeler, özel durumlar ve özel olaylar gibi) için otomatik olarak doldurulur. [Java SDK aracısı](../../azure-monitor/app/java-agent.md) yapılandırılırsa, http üzerinden hizmete çağrı çağrıları için korelasyon üstbilgisini (daha önce açıklanan) da yayar.

> [!NOTE]
> Uygulama Öngörüleri Java aracısı JMS, Kafka, Netty/Webflux ve daha fazlası için istekleri ve bağımlılıkları otomatik olarak toplar. Java SDK için sadece Apache HttpClient üzerinden yapılan aramalar korelasyon özelliği için desteklenir. SDK'da ileti teknolojileri (Kafka, RabbitMQ ve Azure Servis Veri Servisi gibi) arasında otomatik bağlam yayılımı desteklenmez. 

> [!NOTE]
> Özel telemetri toplamak için Java 2.6 SDK ile uygulama enstrüman gerekir. 

### <a name="role-names"></a>Rol adları

Bileşen adlarının [Uygulama Haritası'nda](../../azure-monitor/app/app-map.md)görüntülenme şeklini özelleştirmek isteyebilirsiniz. Bunu yapmak için, aşağıdaki eylemlerden birini `cloud_RoleName` alarak el ile ayarlayabilirsiniz:

- Application Insights Java aracısı 3.0 için bulut rol adını aşağıdaki gibi ayarlayın:

    ```json
    {
      "instrumentationSettings": {
        "preview": {
          "roleName": "my cloud role name"
        }
      }
    }
    ```
    Ayrıca, ortam değişkenini `APPLICATIONINSIGHTS_ROLE_NAME`kullanarak bulut rol adını da ayarlayabilirsiniz.

- Application Insights Java SDK 2.5.0 ve sonrası `cloud_RoleName` ile `<RoleName>` ApplicationInsights.xml dosyanıza ekleyerek şunları belirtebilirsiniz:

  ```XML
  <?xml version="1.0" encoding="utf-8"?>
  <ApplicationInsights xmlns="http://schemas.microsoft.com/ApplicationInsights/2013/Settings" schemaVersion="2014-05-30">
     <InstrumentationKey>** Your instrumentation key **</InstrumentationKey>
     <RoleName>** Your role name **</RoleName>
     ...
  </ApplicationInsights>
  ```

- Uygulama Öngörüleri Bahar Önyükleme Başlatıcısı ile Bahar Önyükleme kullanıyorsanız, uygulama.properties dosyasındaki uygulama için özel adınızı ayarlamanız gerekir:

  `spring.application.name=<name-of-app>`

  İlkbahar Önyükleme `cloudRoleName` `spring.application.name` Başlatıcısı, özellik için girdiğiniz değere otomatik olarak atar.

## <a name="next-steps"></a>Sonraki adımlar

- [Özel telemetri](../../azure-monitor/app/api-custom-events-metrics.md)yazın.
- Core ve ASP.NET ASP.NET gelişmiş korelasyon senaryoları için [bkz.](custom-operations-tracking.md)
- Diğer SDK'lar için [cloud_RoleName ayarlama](../../azure-monitor/app/app-map.md#set-cloud-role-name) hakkında daha fazla bilgi edinin.
- Application Insights'ta mikro hizmetinizin tüm bileşenlerine dahil edin. [Desteklenen platformlara](../../azure-monitor/app/platforms.md)göz atın.
- Uygulama Öngörüleri türleri için [veri modeline](../../azure-monitor/app/data-model.md) bakın.
- [Telemetriyi nasıl genişletip filtreleyeceklerini](../../azure-monitor/app/api-filtering-sampling.md)öğrenin.
- Uygulama [Öngörüleri config referansını](configuration-with-applicationinsights-config.md)gözden geçirin.
