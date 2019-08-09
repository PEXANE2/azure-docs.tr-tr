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
ms.openlocfilehash: 1c6a0ce3e4e8d098d2bc048a331b0ae0cb5c6b13
ms.sourcegitcommit: aa042d4341054f437f3190da7c8a718729eb675e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/09/2019
ms.locfileid: "68881397"
---
# <a name="telemetry-correlation-in-application-insights"></a>Application Insights telemetri bağıntısı

Mikro hizmetler dünyasında, her mantıksal işlem, hizmetin çeşitli bileşenlerinde iş yapılmasını gerektirir. Bu bileşenlerin her biri, [Azure Application Insights](../../azure-monitor/app/app-insights-overview.md)tarafından ayrı olarak izlenebilir. Web uygulaması bileşeni, Kullanıcı kimlik bilgilerini doğrulamak için kimlik doğrulama sağlayıcısı bileşeniyle ve görselleştirme için veri almak üzere API bileşeniyle iletişim kurar. API bileşeni, diğer hizmetlerden gelen verileri sorgulayabilir ve bu çağrı hakkında faturalandırma bileşenine bildirimde bulunan Cache-Provider bileşenlerini kullanabilir. Application Insights, hangi bileşenin hatalardan veya performans düşüşüne karşı sorumlu olduğunu saptamak için kullandığınız dağıtılmış telemetri bağıntısını destekler.

Bu makalede, birden çok bileşen tarafından gönderilen telemetrinin ilişkilendirilmesi için Application Insights tarafından kullanılan veri modeli açıklanmaktadır. Bağlam yayma tekniklerini ve protokollerini içerir. Ayrıca farklı diller ve platformlarda bağıntı kavramlarının uygulanmasını da içerir.

## <a name="data-model-for-telemetry-correlation"></a>Telemetri bağıntısı için veri modeli

Application Insights, dağıtılmış telemetri bağıntısı için bir [veri modeli](../../azure-monitor/app/data-model.md) tanımlar. Telemetrinin mantıksal işlemle ilişkilendirilmesi için, her telemetri öğesinin adında `operation_Id`bir bağlam alanı vardır. Bu tanımlayıcı, dağıtılmış izlemede her telemetri öğesi tarafından paylaşılır. Bu nedenle, tek bir katmanda telemetri kaybı ile birlikte, diğer bileşenler tarafından raporlanan Telemetriyi yine de ilişkilendirebilirsiniz.

Dağıtılmış bir mantıksal işlem genellikle bileşenlerden biri tarafından işlenen istekler olan daha küçük işlemler kümesinden oluşur. Bu işlemler [istek telemetrisi](../../azure-monitor/app/data-model-request-telemetry.md)tarafından tanımlanır. Her istek telemetrisi, kendisini benzersiz `id` ve küresel olarak tanımlayan kendi kendine sahiptir. Ve bu istekle ilişkili tüm telemetri öğeleri (izlemeler ve özel durumlar gibi) isteğin `operation_parentId` `id`değerine ayarlanmalıdır.

Başka bir bileşene yönelik HTTP çağrısı gibi her giden işlem, [bağımlılık telemetrisi](../../azure-monitor/app/data-model-dependency-telemetry.md)tarafından temsil edilir. Bağımlılık telemetrisi, genel olarak benzersiz `id` olan kendisini de tanımlar. Bu bağımlılık çağrısıyla başlatılan istek telemetrisi, bunu `id` `operation_parentId`olarak kullanır.

, `operation_Id` Ve`operation_parentId` ilekullanarak`dependency.id`dağıtılmış mantıksal işlemin bir görünümünü oluşturabilirsiniz. `request.id` Bu alanlar telemetri çağrılarının önem derecesine göre de tanımlar.

Mikro hizmetler ortamında, bileşenlerden izlemeler farklı depolama öğelerine gidebilir. Her bileşen Application Insights kendi izleme anahtarına sahip olabilir. Mantıksal işlem için telemetri almak için Application Insights UX her depolama öğesinden verileri sorgular. Depolama öğelerinin sayısı çok büyük olduğunda, ileri bakabileceğiniz bir ipucu gerekir. Application Insights veri modeli, bu sorunu çözmek için iki alanı tanımlar: `request.source` ve `dependency.target`. İlk alan, bağımlılık isteğini Başlatan bileşeni tanımlar ve ikincisi, bağımlılık çağrısının yanıtını döndüren bileşeni tanımlar.

## <a name="example"></a>Örnek

Daha sonra, adlı `Stock`bir dış API kullanarak bir stokun geçerli pazar fiyatını gösteren hisse senedi fiyatları adlı bir uygulamaya örnek atalım. Hisse senedi fiyatları uygulamasında istemci Web tarayıcısının kullanılarak `Stock page` `GET /Home/Stock`açılan adlı bir sayfa bulunur. Uygulama, `Stock` API 'yi bir http çağrısı `GET /api/stock/value`kullanarak sorgular.

Bir sorgu çalıştırarak elde edilen telemetrisini çözümleyebilirsiniz:

```kusto
(requests | union dependencies | union pageViews)
| where operation_Id == "STYz"
| project timestamp, itemType, name, id, operation_ParentId, operation_Id
```

Sonuçlarda, tüm telemetri öğelerinin kökü `operation_Id`paylaştığından emin olmanız gerektiğini unutmayın. Sayfadan bir AJAX çağrısı yapıldığında, bağımlılık telemetrisine yeni bir benzersiz kimlik (`qJSXU`) atanır ve PageView kimliği olarak `operation_ParentId`kullanılır. Sunucu isteği daha sonra Ajax KIMLIĞINI olarak `operation_ParentId`kullanır.

| ItemType   | name                      | id           | operation_ParentId | operation_Id |
|------------|---------------------------|--------------|--------------------|--------------|
| Sayfa görüntülemesi   | Hisse senedi sayfası                |              | Stilz               | Stilz         |
| bağımlılık | /Home/Stock al           | qJSXU        | Stilz               | Stilz         |
| request    | Giriş/stok al            | KqKwlrSt9PA = | qJSXU              | Stilz         |
| bağımlılık | /Api/Stock/Value al      | bBrf2L7mm2g= | KqKwlrSt9PA =       | Stilz         |

Bir dış hizmete `GET /api/stock/value` çağrı yapıldığında, `dependency.target` alanı uygun şekilde ayarlayabilmeniz için bu sunucunun kimliğini bilmek istersiniz. Dış hizmet izlemeyi desteklemiyorsa, `target` hizmetin ana bilgisayar adına ayarlanır (örneğin, `stock-prices-api.com`). Bununla birlikte, hizmet kendisini önceden tanımlı bir http üst bilgisi döndürerek tanımlarsa `target` , Application Insights bu hizmetten gelen telemetrisini sorgulayarak dağıtılmış bir izleme oluşturmasına izin veren hizmet kimliğini içerir.

## <a name="correlation-headers"></a>Bağıntı üstbilgileri

[BAĞıNTı http Protokolü](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/HttpCorrelationProtocol.md)IÇIN bir RFC teklifi üzerinde çalışıyoruz. Bu teklif iki üstbilgiyi tanımlar:

- `Request-Id`: Çağrının genel benzersiz KIMLIĞINI taşır.
- `Correlation-Context`: Dağıtılmış izleme özelliklerinin ad-değer çiftleri koleksiyonunu taşır.

Standart ayrıca oluşturma için `Request-Id` iki şema tanımlar: düz ve hiyerarşik. Düz şemayla, `Id` `Correlation-Context` koleksiyon için iyi bilinen bir anahtar tanımlanmıştır.

Application Insights bağıntı HTTP protokolünün [uzantısını](https://github.com/lmolkova/correlation/blob/master/http_protocol_proposal_v2.md) tanımlar. Anında çağıran `Request-Context` veya çağrılan tarafından kullanılan özellik koleksiyonunu yaymak için ad-değer çiftlerini kullanır. Application Insights SDK bu üstbilgiyi ve `dependency.target` `request.source` alanlarını ayarlamak için kullanır.

### <a name="w3c-distributed-tracing"></a>W3C dağıtılmış izleme

[W3C dağıtılmış izleme biçimine](https://w3c.github.io/trace-context/)geçiş yapıyoruz. Şunları tanımlar:

- `traceparent`: Genel olarak benzersiz işlem KIMLIĞI ve çağrının benzersiz tanımlayıcısını taşır.
- `tracestate`: Sisteme özgü izleme bağlamını taşır.

#### <a name="enable-w3c-distributed-tracing-support-for-classic-aspnet-apps"></a>Klasik ASP.NET uygulamaları için W3C dağıtılmış izleme desteğini etkinleştir

Bu özellik içinde `Microsoft.ApplicationInsights.Web` ve `Microsoft.ApplicationInsights.DependencyCollector` 2.8.0-Beta1 sürümü ile başlayan paketlerde kullanılabilir.
Varsayılan olarak devre dışıdır. Etkinleştirmek için şunu değiştirin `ApplicationInsights.config`:

- Altında `RequestTrackingTelemetryModule`, değerine ayarlanmış `EnableW3CHeadersExtraction` olan `true`öğeyi ekleyin.
- Altında `DependencyTrackingTelemetryModule`, değerine ayarlanmış `EnableW3CHeadersInjection` olan `true`öğeyi ekleyin.
- Şuna benzer şekilde ekleyin `W3COperationCorrelationTelemetryInitializer` `TelemetryInitializers` 

```xml
<TelemetryInitializers>
  <Add Type="Microsoft.ApplicationInsights.Extensibility.W3C.W3COperationCorrelationTelemetryInitializer, Microsoft.ApplicationInsights"/>
   ...
</TelemetryInitializers> 
```

#### <a name="enable-w3c-distributed-tracing-support-for-aspnet-core-apps"></a>ASP.NET Core uygulamalar için W3C dağıtılmış izleme desteğini etkinleştir

Bu özellik 2.5.0- `Microsoft.ApplicationInsights.AspNetCore` Beta1 sürümünde `Microsoft.ApplicationInsights.DependencyCollector` ve 2.8.0-Beta1 sürümünde bulunur.
Varsayılan olarak devre dışıdır. Etkinleştirmek için şu şekilde `ApplicationInsightsServiceOptions.RequestCollectionOptions.EnableW3CDistributedTracing` `true`ayarlayın:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddApplicationInsightsTelemetry(o => 
        o.RequestCollectionOptions.EnableW3CDistributedTracing = true );
    // ....
}
```

#### <a name="enable-w3c-distributed-tracing-support-for-java-apps"></a>Java uygulamaları için W3C dağıtılmış izleme desteğini etkinleştir

- **Gelen yapılandırma**

  - Java EE uygulamaları için, aşağıdakileri `<TelemetryModules>` ApplicationInsights. xml içindeki etikete ekleyin:

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

#### <a name="enable-w3c-distributed-tracing-support-for-web-apps"></a>Web Apps için W3C dağıtılmış izleme desteğini etkinleştir

Bu özellik ' de `Microsoft.ApplicationInsights.JavaScript`bulunur. Varsayılan olarak devre dışıdır. Etkinleştirmek için, config kullanın `distributedTracingMode` . AI_AND_W3C, eski Application Insights belgelenmiş hizmetlerle geri uyumluluk için sağlanır:

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
| `Id``Request` ve`Dependency`    | `SpanId`                                          |
| `Operation_Id`                        | `TraceId`                                         |
| `Operation_ParentId`                  | `Reference`türündeki `ChildOf` (üst yayılma)   |

Daha fazla bilgi için bkz. [telemetri veri modeli Application Insights](../../azure-monitor/app/data-model.md). 

OpenTracing kavramlarının tanımları için bkz. OpenTracing [belirtimi](https://github.com/opentracing/specification/blob/master/specification.md) ve [semantic_conventions](https://github.com/opentracing/specification/blob/master/semantic_conventions.md).

## <a name="telemetry-correlation-in-net"></a>.NET 'te telemetri bağıntısı

Zaman içinde, .NET telemetri ve tanılama günlüklerini ilişkilendirmek için çeşitli yollar tanımladı:

- `System.Diagnostics.CorrelationManager`[LogicalOperationStack ve ActivityId](https://msdn.microsoft.com/library/system.diagnostics.correlationmanager.aspx)'nin izlenmesine izin verir. 
- `System.Diagnostics.Tracing.EventSource`ve Windows için olay Izleme (ETW) [SetCurrentThreadActivityId](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.setcurrentthreadactivityid.aspx) metodunu tanımlar.
- `ILogger`[günlük kapsamlarını](https://docs.microsoft.com/aspnet/core/fundamentals/logging#log-scopes)kullanır. 
- Windows Communication Foundation (WCF) ve HTTP dağıtımı "geçerli" bağlam yayma.

Ancak, bu yöntemler otomatik dağıtılmış izleme desteğini etkinleştirmiyordu. `DiagnosticSource`, otomatik makine çapraz bağıntısını desteklemeye yönelik bir yoldur. .NET kitaplıkları ' DiagnosticSource ' öğesini destekler ve HTTP gibi bir aktarım aracılığıyla bağıntı bağlamının otomatik makine çapraz yayılmasını sağlar.

İçindeki`DiagnosticSource` [etkinliklere yönelik kılavuz](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/ActivityUserGuide.md) , etkinliklerin izlenmesi hakkında temel bilgileri açıklar.

ASP.NET Core 2,0, HTTP üstbilgilerinin ayıklanmasını ve yeni bir etkinliğin başlatılmasını destekler.

`System.Net.HttpClient`sürüm 4.1.0 ile başlayarak, bağıntı HTTP üst bilgilerinin otomatik olarak eklenmesine ve HTTP çağrısının etkinlik olarak izlenmesini destekler.

Klasik ASP.NET için [Microsoft. Aspnet. TelemetryCorrelation](https://www.nuget.org/packages/Microsoft.AspNet.TelemetryCorrelation/)yenı bir http modülü vardır. Bu modül kullanarak `DiagnosticSource`telemetri bağıntısını uygular. Gelen istek üst bilgilerine göre bir etkinlik başlatır. Ayrıca, her bir Internet Information Services (IIS) işleminin farklı bir yönetilen iş parçacığında çalıştığı durumlar da dahil olmak üzere istek işlemenin farklı aşamalarından Telemetriyi de ilişkilendirir.

2\.4.0-Beta1 sürümünden başlayarak Application Insights SDK, telemetri toplamak ve geçerli `DiagnosticSource` etkinlikle `Activity` ilişkilendirmek için ve kullanır.

<a name="java-correlation"></a>
## <a name="telemetry-correlation-in-the-java-sdk"></a>Java SDK 'sında telemetri bağıntısı

[Java için APPLICATION INSIGHTS SDK](../../azure-monitor/app/java-get-started.md) , sürüm 2.0.0 ile başlayan Telemetriyi otomatik olarak destekler. Bir isteğin kapsamında `operation_id` verilen tüm telemetri (izlemeler, özel durumlar ve özel olaylar gibi) otomatik olarak doldurulur. [Java SDK Aracısı](../../azure-monitor/app/java-agent.md) YAPıLANDıRıLDıYSA, http aracılığıyla hizmet-hizmet çağrıları için bağıntı üst bilgilerini (daha önce açıklanan) yayma işlemini de gerçekleştirir.

> [!NOTE]
> Bağıntı özelliği için yalnızca Apache HTTPClient aracılığıyla yapılan çağrılar desteklenir. Yay RestTemplate veya Feign kullanıyorsanız, her ikisi de aynı şekilde Apache HTTPClient ile kullanılabilir.

Şu anda, mesajlaşma teknolojileri genelinde otomatik bağlam yayma (Kafka, Kbbitmq veya Azure Service Bus) desteklenmez. Ancak, `trackDependency` ve `trackRequest` API 'leri kullanılarak bu tür senaryolara el ile kod eklemek mümkündür. Bu API 'lerde bir bağımlılık telemetrisi, bir üretici tarafından kuyruğa alınan bir iletiyi temsil eder ve istek bir tüketici tarafından işlenen bir iletiyi temsil eder. Bu durumda, her ikisi `operation_id` de `operation_parentId` iletinin özelliklerine yayılmalıdır.

### <a name="telemetry-correlation-in-asynchronous-java-application"></a>Zaman uyumsuz Java uygulamasında telemetri bağıntısı

Zaman uyumsuz Spring Boot uygulamasındaki Telemetriyi ilişkilendirmek için lütfen [Bu](https://github.com/Microsoft/ApplicationInsights-Java/wiki/Distributed-Tracing-in-Asynchronous-Java-Applications) ayrıntılı makaleyi izleyin. Bu, bahar 'in [Threadpooltaskyürütücü](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/scheduling/concurrent/ThreadPoolTaskExecutor.html) 'in yanı sıra [Threadpooltaskscheduler](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/scheduling/concurrent/ThreadPoolTaskScheduler.html)' a yönelik rehberlik sağlar. 


<a name="java-role-name"></a>
## <a name="role-name"></a>Rol adı

Her zaman, bileşen adlarının [uygulama eşlemesinde](../../azure-monitor/app/app-map.md)görüntülenme şeklini özelleştirmek isteyebilirsiniz. Bunu yapmak için aşağıdakilerden birini yaparak el ile ayarlayabilirsiniz `cloud_RoleName` :

- Spring Boot uygulamasını Application Insights Spring Boot Starter ile kullanırsanız, tek yapmanız gereken tek değişiklik, uygulama. Özellikler dosyasında uygulama için özel adınızı ayarlamanıza yöneliktir.

  `spring.application.name=<name-of-app>`

  Spring Boot Starter, `cloudRoleName` `spring.application.name` özelliği için girdiğiniz değere otomatik olarak atar.

- Kullanıyorsanız, uygulama adını otomatik olarak ayarlar.`WebAppNameContextInitializer` `WebRequestTrackingFilter` Yapılandırma dosyanıza (ApplicationInsights. xml) aşağıdakini ekleyin:

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
- Diğer SDK 'lar için [cloud_RoleName ayarlama](../../azure-monitor/app/app-map.md#set-cloud-role-name) hakkında daha fazla bilgi edinin.
- Mikro hizmetinizin tüm bileşenlerini Application Insights ekleyin. [Desteklenen platformları](../../azure-monitor/app/platforms.md)inceleyin.
- Application Insights türleri için [veri modeline](../../azure-monitor/app/data-model.md) bakın.
- Telemetrinin nasıl [genişletileceğini ve filtreleneceğini](../../azure-monitor/app/api-filtering-sampling.md)öğrenin.
- [Application Insights config başvurusunu](configuration-with-applicationinsights-config.md)gözden geçirin.
