---
title: Azure Application Insights telemetri bağıntısı | Microsoft Docs
description: Application Insights telemetri bağıntısı
services: application-insights
documentationcenter: .net
author: lgayhardt
manager: carmonm
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 06/07/2019
ms.reviewer: sergkanz
ms.author: lagayhar
ms.openlocfilehash: fe52fe51b347b232e03bad943906413b90c853c0
ms.sourcegitcommit: e1b6a40a9c9341b33df384aa607ae359e4ab0f53
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71338182"
---
# <a name="telemetry-correlation-in-application-insights"></a>Application Insights telemetri bağıntısı

Mikro hizmetler dünyasında, her mantıksal işlem, hizmetin çeşitli bileşenlerinde iş yapılmasını gerektirir. Bu bileşenlerin her biri, [Azure Application Insights](../../azure-monitor/app/app-insights-overview.md)tarafından ayrı olarak izlenebilir. Web uygulaması bileşeni, Kullanıcı kimlik bilgilerini doğrulamak için kimlik doğrulama sağlayıcısı bileşeniyle ve görselleştirme için veri almak üzere API bileşeniyle iletişim kurar. API bileşeni, diğer hizmetlerden gelen verileri sorgulayabilir ve bu çağrı hakkında faturalandırma bileşenine bildirimde bulunan Cache-Provider bileşenlerini kullanabilir. Application Insights, hangi bileşenin hatalardan veya performans düşüşüne karşı sorumlu olduğunu saptamak için kullandığınız dağıtılmış telemetri bağıntısını destekler.

Bu makalede, birden çok bileşen tarafından gönderilen telemetrinin ilişkilendirilmesi için Application Insights tarafından kullanılan veri modeli açıklanmaktadır. Bağlam yayma tekniklerini ve protokollerini içerir. Ayrıca farklı diller ve platformlarda bağıntı kavramlarının uygulanmasını da içerir.

## <a name="data-model-for-telemetry-correlation"></a>Telemetri bağıntısı için veri modeli

Application Insights, dağıtılmış telemetri bağıntısı için bir [veri modeli](../../azure-monitor/app/data-model.md) tanımlar. Telemetrinin mantıksal işlemle ilişkilendirilmesi için, her telemetri öğesinin `operation_Id` adlı bir bağlam alanı vardır. Bu tanımlayıcı, dağıtılmış izlemede her telemetri öğesi tarafından paylaşılır. Bu nedenle, tek bir katmanda telemetri kaybı ile birlikte, diğer bileşenler tarafından raporlanan Telemetriyi yine de ilişkilendirebilirsiniz.

Dağıtılmış bir mantıksal işlem genellikle bileşenlerden biri tarafından işlenen istekler olan daha küçük işlemler kümesinden oluşur. Bu işlemler [istek telemetrisi](../../azure-monitor/app/data-model-request-telemetry.md)tarafından tanımlanır. Her istek telemetrisi, kendisini benzersiz ve küresel olarak tanımlayan `id` ' dır. Ve bu istekle ilişkili tüm telemetri öğeleri (izlemeler ve özel durumlar gibi), `operation_parentId` ' i isteğin değerine `id` ' i ayarlamanız gerekir.

Başka bir bileşene yönelik HTTP çağrısı gibi her giden işlem, [bağımlılık telemetrisi](../../azure-monitor/app/data-model-dependency-telemetry.md)tarafından temsil edilir. Bağımlılık telemetrisi, genel olarak benzersiz olan kendi @no__t de tanımlar. Bu bağımlılık çağrısıyla başlatılan istek telemetrisi, bu `id` ' ı `operation_parentId` olarak kullanır.

@No__t-0, `operation_parentId` ve `dependency.id` ile `request.id` kullanarak dağıtılmış mantıksal işlemin görünümünü oluşturabilirsiniz. Bu alanlar telemetri çağrılarının önem derecesine göre de tanımlar.

Mikro hizmetler ortamında, bileşenlerden izlemeler farklı depolama öğelerine gidebilir. Her bileşen Application Insights kendi izleme anahtarına sahip olabilir. Mantıksal işlem için telemetri almak için Application Insights UX her depolama öğesinden verileri sorgular. Depolama öğelerinin sayısı çok büyük olduğunda, ileri bakabileceğiniz bir ipucu gerekir. Application Insights veri modeli bu sorunu çözmek için iki alanı tanımlar: `request.source` ve `dependency.target`. İlk alan, bağımlılık isteğini Başlatan bileşeni tanımlar ve ikincisi, bağımlılık çağrısının yanıtını döndüren bileşeni tanımlar.

## <a name="example"></a>Örnek

Hisse senedi fiyatları adlı bir uygulamaya örnek olarak, `Stock` adlı bir dış API kullanarak bir hisse senedinin geçerli pazar fiyatını gösteren bir örnek alalım. Hisse senedi fiyatları uygulaması, `GET /Home/Stock` ' i kullanarak istemci Web tarayıcısının açtığı `Stock page` adlı bir sayfa içerir. Uygulama, `Stock` API 'sini bir HTTP çağrısı `GET /api/stock/value` kullanarak sorgular.

Bir sorgu çalıştırarak elde edilen telemetrisini çözümleyebilirsiniz:

```kusto
(requests | union dependencies | union pageViews)
| where operation_Id == "STYz"
| project timestamp, itemType, name, id, operation_ParentId, operation_Id
```

Sonuçlarda, tüm telemetri öğelerinin kök @no__t (0) paylaştığından emin olduğunu unutmayın. Sayfadan bir AJAX çağrısı yapıldığında, bağımlılık telemetrisine yeni bir benzersiz KIMLIK (`qJSXU`) atanır ve pageView KIMLIĞI `operation_ParentId` olarak kullanılır. Sunucu isteği daha sonra `operation_ParentId` olarak Ajax KIMLIĞINI kullanır.

| ItemType   | name                      | ID           | operation_ParentId | operation_Id |
|------------|---------------------------|--------------|--------------------|--------------|
| pageView   | Hisse senedi sayfası                |              | Stilz               | Stilz         |
| bağımlılık | /Home/Stock al           | qJSXU        | Stilz               | Stilz         |
| request    | Giriş/stok al            | KqKwlrSt9PA = | qJSXU              | Stilz         |
| bağımlılık | /Api/Stock/Value al      | bBrf2L7mm2g= | KqKwlrSt9PA =       | Stilz         |

@No__t-0 çağrısı bir dış hizmete yapıldığında, `dependency.target` alanını uygun şekilde ayarlayabilmeniz için bu sunucunun kimliğini bilmek istersiniz. Dış hizmet izlemeyi desteklemediği zaman, `target`, hizmetin ana bilgisayar adına ayarlanır (örneğin, `stock-prices-api.com`). Ancak, hizmet kendisini önceden tanımlı bir HTTP üst bilgisi döndürerek tanımlarsa, `target`, Application Insights, bu hizmetten telemetri sorgulayarak dağıtılmış bir izleme oluşturmasına izin veren hizmet kimliğini içerir.

## <a name="correlation-headers"></a>Bağıntı üstbilgileri

Şunu tanımlayan [W3C Trace-Context](https://w3c.github.io/trace-context/) 'e geçiş yapıyoruz:

- `traceparent`: Genel olarak benzersiz işlem KIMLIĞI ve çağrının benzersiz tanımlayıcısını taşır.
- `tracestate`: Sisteme özgü izleme bağlamını taşır.

Application Insights SDK 'ların en son sürümleri Trace-Context protokolünü destekler, ancak bunu kabul etmeniz gerekebilir (ApplicationInsights SDK 'Ları tarafından desteklenen eski Bağıntı protokolü ile geriye dönük uyumluluk sağlar).

[Bağıntı http Protokolü diğer adıyla istek kimliği](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/HttpCorrelationProtocol.md) , kullanımdan kaldırma yolunda. Bu protokol iki üstbilgiyi tanımlar:

- `Request-Id`: Çağrının genel benzersiz KIMLIĞINI taşır.
- `Correlation-Context`: Dağıtılmış izleme özelliklerinin ad-değer çiftleri koleksiyonunu taşır.

Application Insights ayrıca bağıntı HTTP protokolünün [uzantısını](https://github.com/lmolkova/correlation/blob/master/http_protocol_proposal_v2.md) tanımlar. Anında çağıran veya çağrılan tarafından kullanılan özelliklerin koleksiyonunu yaymak için `Request-Context` ad-değer çiftleri kullanır. Application Insights SDK bu üstbilgiyi, `dependency.target` ve `request.source` alanlarını ayarlamak için kullanır.

### <a name="enable-w3c-distributed-tracing-support-for-classic-aspnet-apps"></a>Klasik ASP.NET uygulamaları için W3C dağıtılmış izleme desteğini etkinleştir
 
  > [!NOTE]
  > @No__t-0 ve `Microsoft.ApplicationInsights.DependencyCollector` ile başlayan yapılandırma gerekmiyor 

W3C Trace-Context support, geriye dönük olarak uyumlu şekilde yapılır ve bağıntı, önceki SDK sürümleriyle (W3C desteği olmadan) işaretlenmiş uygulamalarla birlikte çalışmak üzere beklenmektedir. 

Eski `Request-Id` protokolünü kullanmaya devam etmek istediğiniz nedenlerden dolayı, izleme bağlamını aşağıdaki yapılandırmayla *devre dışı bırakabilirsiniz*

```csharp
  Activity.DefaultIdFormat = ActivityIdFormat.Hierarchical;
  Activity.ForceDefaultIdFormat = true;
```

SDK 'nın eski bir sürümünü çalıştırırsanız, bunu güncelleştirmenizi veya Izleme bağlamını etkinleştirmek için aşağıdaki yapılandırmayı uygulamayı öneririz.
Bu özellik, 2.8.0-Beta1 sürümü ile başlayan `Microsoft.ApplicationInsights.Web` ve `Microsoft.ApplicationInsights.DependencyCollector` paketlerinde kullanılabilir.
Varsayılan olarak devre dışıdır. Etkinleştirmek için @no__t değiştirin-0:

- @No__t-0 ' ın altında, değeri `true` olarak ayarlanan `EnableW3CHeadersExtraction` öğesini ekleyin.
- @No__t-0 ' ın altında, değeri `true` olarak ayarlanan `EnableW3CHeadersInjection` öğesini ekleyin.
- Şuna benzer `TelemetryInitializers` `W3COperationCorrelationTelemetryInitializer` ekleyin 

```xml
<TelemetryInitializers>
  <Add Type="Microsoft.ApplicationInsights.Extensibility.W3C.W3COperationCorrelationTelemetryInitializer, Microsoft.ApplicationInsights"/>
   ...
</TelemetryInitializers> 
```

### <a name="enable-w3c-distributed-tracing-support-for-aspnet-core-apps"></a>ASP.NET Core uygulamalar için W3C dağıtılmış izleme desteğini etkinleştir

 > [!NOTE]
  > @No__t-0 sürüm 2.8.0 ile başlayan yapılandırma gerekmiyor.
 
W3C Trace-Context support, geriye dönük olarak uyumlu şekilde yapılır ve bağıntı, önceki SDK sürümleriyle (W3C desteği olmadan) işaretlenmiş uygulamalarla birlikte çalışmak üzere beklenmektedir. 

Eski `Request-Id` protokolünü kullanmaya devam etmek istediğiniz nedenlerden dolayı, izleme bağlamını aşağıdaki yapılandırmayla *devre dışı bırakabilirsiniz*

```csharp
  Activity.DefaultIdFormat = ActivityIdFormat.Hierarchical;
  Activity.ForceDefaultIdFormat = true;
```

SDK 'nın eski bir sürümünü çalıştırırsanız, bunu güncelleştirmenizi veya Izleme bağlamını etkinleştirmek için aşağıdaki yapılandırmayı uygulamayı öneririz.

Bu özellik `Microsoft.ApplicationInsights.AspNetCore` sürüm 2.5.0-Beta1 ve `Microsoft.ApplicationInsights.DependencyCollector` 2.8.0-Beta1 sürümünde bulunur.
Varsayılan olarak devre dışıdır. Etkinleştirmek için `ApplicationInsightsServiceOptions.RequestCollectionOptions.EnableW3CDistributedTracing` ' ı `true` olarak ayarlayın:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddApplicationInsightsTelemetry(o => 
        o.RequestCollectionOptions.EnableW3CDistributedTracing = true );
    // ....
}
```

### <a name="enable-w3c-distributed-tracing-support-for-java-apps"></a>Java uygulamaları için W3C dağıtılmış izleme desteğini etkinleştir

- **Gelen yapılandırma**

  - Java EE uygulamaları için, ApplicationInsights. xml içinde `<TelemetryModules>` etiketine aşağıdakileri ekleyin:

    ```xml
    <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebRequestTrackingTelemetryModule>
       <Param name = "W3CEnabled" value ="true"/>
       <Param name ="enableW3CBackCompat" value = "true" />
    </Add>
    ```
  - Spring Boot uygulamaları için aşağıdaki özellikleri ekleyin:

    - `azure.application-insights.web.enable-W3C=true`
    - `azure.application-insights.web.enable-W3C-backcompat-mode=true`

- **Giden yapılandırma**

  Aşağıdakileri AI-Agent. xml ' ye ekleyin:

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
> Hem gelen hem de giden yapılandırmaların tam olarak aynı olduğundan emin olun.

### <a name="enable-w3c-distributed-tracing-support-for-web-apps"></a>Web Apps için W3C dağıtılmış izleme desteğini etkinleştir

Bu özellik `Microsoft.ApplicationInsights.JavaScript` ' dır. Varsayılan olarak devre dışıdır. Etkinleştirmek için `distributedTracingMode` yapılandırması kullanın. AI_AND_W3C, eski Application Insights belgelenmiş hizmetlerle geri uyumluluk için sağlanır:

- **NPM kurulumu (kod parçacığı kurulumu kullanılıyorsa yoksay)**

  ```javascript
  import { ApplicationInsights, DistributedTracingModes } from '@microsoft/applicationinsights-web';

  const appInsights = new ApplicationInsights({ config: {
    instrumentationKey: 'YOUR_INSTRUMENTATION_KEY_GOES_HERE',
    distributedTracingMode: DistributedTracingModes.W3C
    /* ...Other Configuration Options... */
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
      /* ...Other Configuration Options... */
    }
  );
  window[aiName]=aisdk,aisdk.queue&&0===aisdk.queue.length&&aisdk.trackPageView({});
  </script>
  ```

## <a name="opentracing-and-application-insights"></a>OpenTracing ve Application Insights

[Opentracing veri modeli belirtimi](https://opentracing.io/) ve Application Insights veri modelleri aşağıdaki şekilde eşlenir:

| Application Insights                  | OpenTracing                                       |
|------------------------------------   |-------------------------------------------------  |
| `Request`, `PageView`                 | `Span`kullanılarak`span.kind = server`                  |
| `Dependency`                          | `Span`kullanılarak`span.kind = client`                  |
| `Id`/`Request` ve `Dependency`    | `SpanId`                                          |
| `Operation_Id`                        | `TraceId`                                         |
| `Operation_ParentId`                  | `Reference` tür `ChildOf` (üst Aralık)   |

Daha fazla bilgi için bkz. [telemetri veri modeli Application Insights](../../azure-monitor/app/data-model.md). 

OpenTracing kavramlarının tanımları için bkz. OpenTracing [belirtimi](https://github.com/opentracing/specification/blob/master/specification.md) ve [semantic_conventions](https://github.com/opentracing/specification/blob/master/semantic_conventions.md).

## <a name="telemetry-correlation-in-net"></a>.NET 'te telemetri bağıntısı

Zaman içinde, .NET telemetri ve tanılama günlüklerini ilişkilendirmek için çeşitli yollar tanımladı:

- `System.Diagnostics.CorrelationManager` [LogicalOperationStack ve ActivityId](https://msdn.microsoft.com/library/system.diagnostics.correlationmanager.aspx)'nin izlenmesine izin verir. 
- `System.Diagnostics.Tracing.EventSource` ve Windows için olay Izleme (ETW) [SetCurrentThreadActivityId](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.setcurrentthreadactivityid.aspx) metodunu tanımlar.
- `ILogger` [günlük kapsamlarını](https://docs.microsoft.com/aspnet/core/fundamentals/logging#log-scopes)kullanır. 
- Windows Communication Foundation (WCF) ve HTTP dağıtımı "geçerli" bağlam yayma.

Ancak, bu yöntemler otomatik dağıtılmış izleme desteğini etkinleştirmiyordu. `DiagnosticSource`, otomatik makine çapraz bağıntısını desteklemeye yönelik bir yoldur. .NET kitaplıkları ' DiagnosticSource ' öğesini destekler ve HTTP gibi bir aktarım aracılığıyla bağıntı bağlamının otomatik makine çapraz yayılmasını sağlar.

@No__t-1 ' deki [etkinliklere yönelik kılavuz](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/ActivityUserGuide.md) , etkinliklerin izlenmesi hakkında temel bilgileri açıklar.

ASP.NET Core 2,0, HTTP üstbilgilerinin ayıklanmasını ve yeni bir etkinliğin başlatılmasını destekler.

`System.Net.Http.HttpClient`, sürüm 4.1.0 ile başlayarak, bağıntı HTTP üst bilgilerinin otomatik olarak eklenmesine ve HTTP çağrısının etkinlik olarak izlenmesini destekler.

Klasik ASP.NET için [Microsoft. Aspnet. TelemetryCorrelation](https://www.nuget.org/packages/Microsoft.AspNet.TelemetryCorrelation/)yenı bir http modülü vardır. Bu modül `DiagnosticSource` kullanarak telemetri bağıntısını uygular. Gelen istek üst bilgilerine göre bir etkinlik başlatır. Ayrıca, her bir Internet Information Services (IIS) işleminin farklı bir yönetilen iş parçacığında çalıştığı durumlar da dahil olmak üzere istek işlemenin farklı aşamalarından Telemetriyi de ilişkilendirir.

2\.4.0-Beta1 sürümünden başlayarak Application Insights SDK, telemetri toplamak ve geçerli etkinlikle ilişkilendirmek için `DiagnosticSource` ve `Activity` kullanır.

<a name="java-correlation"></a>
## <a name="telemetry-correlation-in-the-java-sdk"></a>Java SDK 'sında telemetri bağıntısı

[Java için APPLICATION INSIGHTS SDK](../../azure-monitor/app/java-get-started.md) , sürüm 2.0.0 ile başlayan Telemetriyi otomatik olarak destekler. Bir isteğin kapsamında verilen tüm telemetri (izlemeler, özel durumlar ve özel olaylar gibi) için `operation_id` ' ı otomatik olarak doldurur. [Java SDK Aracısı](../../azure-monitor/app/java-agent.md) YAPıLANDıRıLDıYSA, http aracılığıyla hizmet-hizmet çağrıları için bağıntı üst bilgilerini (daha önce açıklanan) yayma işlemini de gerçekleştirir.

> [!NOTE]
> Bağıntı özelliği için yalnızca Apache HTTPClient aracılığıyla yapılan çağrılar desteklenir. Yay RestTemplate veya Feign kullanıyorsanız, her ikisi de aynı şekilde Apache HTTPClient ile kullanılabilir.

Şu anda, mesajlaşma teknolojileri genelinde otomatik bağlam yayma (Kafka, Kbbitmq veya Azure Service Bus) desteklenmez. Ancak, `trackDependency` ve `trackRequest` API 'Lerini kullanarak bu tür senaryolara el ile kod eklemek mümkündür. Bu API 'lerde bir bağımlılık telemetrisi, bir üretici tarafından kuyruğa alınan bir iletiyi temsil eder ve istek bir tüketici tarafından işlenen bir iletiyi temsil eder. Bu durumda, `operation_id` ve `operation_parentId` her ikisi de iletinin özelliklerine yayılmalıdır.

### <a name="telemetry-correlation-in-asynchronous-java-application"></a>Zaman uyumsuz Java uygulamasında telemetri bağıntısı

Zaman uyumsuz Spring Boot uygulamasındaki Telemetriyi ilişkilendirmek için lütfen [Bu](https://github.com/Microsoft/ApplicationInsights-Java/wiki/Distributed-Tracing-in-Asynchronous-Java-Applications) ayrıntılı makaleyi izleyin. Bu, bahar 'in [Threadpooltaskyürütücü](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/scheduling/concurrent/ThreadPoolTaskExecutor.html) 'in yanı sıra [Threadpooltaskscheduler](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/scheduling/concurrent/ThreadPoolTaskScheduler.html)' a yönelik rehberlik sağlar. 


<a name="java-role-name"></a>
## <a name="role-name"></a>Rol adı

Her zaman, bileşen adlarının [uygulama eşlemesinde](../../azure-monitor/app/app-map.md)görüntülenme şeklini özelleştirmek isteyebilirsiniz. Bunu yapmak için, aşağıdakilerden birini yaparak `cloud_RoleName` ' ı el ile ayarlayabilirsiniz:

- Spring Boot uygulamasını Application Insights Spring Boot Starter ile kullanırsanız, tek yapmanız gereken tek değişiklik, uygulama. Özellikler dosyasında uygulama için özel adınızı ayarlamanıza yöneliktir.

  `spring.application.name=<name-of-app>`

  Spring Boot Starter, `spring.application.name` özelliği için girdiğiniz değere `cloudRoleName` ' yı otomatik olarak atar.

- @No__t-0 kullanıyorsanız, `WebAppNameContextInitializer` uygulama adını otomatik olarak ayarlar. Yapılandırma dosyanıza (ApplicationInsights. xml) aşağıdakini ekleyin:

  ```XML
  <ContextInitializers>
    <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebAppNameContextInitializer" />
  </ContextInitializers>
  ```

- Bulut bağlamı sınıfını kullanıyorsanız:

  ```Java
  telemetryClient.getContext().getCloud().setRole("My Component Name");
  ```

## <a name="next-steps"></a>Sonraki adımlar

- [Özel telemetri](../../azure-monitor/app/api-custom-events-metrics.md)yazın.
- ASP.NET Core ve ASP.NET ' deki gelişmiş bağıntı senaryoları için [özel işlemleri izleme](custom-operations-tracking.md) makalesine başvurun.
- Diğer SDK 'lar için [cloud_RoleName ayarlama](../../azure-monitor/app/app-map.md#set-cloud-role-name) hakkında daha fazla bilgi edinin.
- Mikro hizmetinizin tüm bileşenlerini Application Insights ekleyin. [Desteklenen platformları](../../azure-monitor/app/platforms.md)inceleyin.
- Application Insights türleri için [veri modeline](../../azure-monitor/app/data-model.md) bakın.
- [Telemetrinin nasıl genişletileceğini ve filtreleneceğini](../../azure-monitor/app/api-filtering-sampling.md)öğrenin.
- [Application Insights config başvurusunu](configuration-with-applicationinsights-config.md)gözden geçirin.
