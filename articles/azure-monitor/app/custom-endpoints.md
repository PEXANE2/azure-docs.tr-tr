---
title: Azure Uygulama Öngörüleri varsayılan SDK uç noktalarını geçersiz kılar
description: Varsayılan Azure Monitörü Uygulama Öngörüleri SDK uç noktalarını Azure Kamu gibi bölgeler için değiştirin.
ms.topic: conceptual
ms.date: 07/26/2019
ms.openlocfilehash: b4ab05c7ee815b385ffb2d1ff9e621063d744dd7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80298329"
---
# <a name="application-insights-overriding-default-endpoints"></a>Varsayılan uç noktaları geçersiz kılan Uygulama Öngörüleri

Uygulama Öngörüleri'nden belirli bölgelere veri göndermek için varsayılan uç nokta adreslerini geçersiz kılmanız gerekir. Her SDK, bu makalede açıklanan biraz farklı değişiklikler gerektirir. Bu değişiklikler, örnek kodun ayarlanmasını ve yer `QuickPulse_Endpoint_Address` `TelemetryChannel_Endpoint_Address`tutucu `Profile_Query_Endpoint_address` değerlerinin , ve belirli bölgenizin gerçek bitiş noktası adresleriyle değiştirilmesini gerektirir. Bu makalenin sonunda, bu yapılandırmanın gerekli olduğu bölgelerin bitiş noktası adreslerine bağlantılar içerir.

## <a name="sdk-code-changes"></a>SDK kodu değişiklikleri

### <a name="net-with-applicationinsightsconfig"></a>.NET applicationinsights.config ile

> [!NOTE]
> Applicationinsights.config dosyası, SDK yükseltmesi yapıldığında otomatik olarak üzerine yazılır. Bir SDK yükseltmesi yaptıktan sonra bölgeye özgü uç nokta değerlerini yeniden girdiğinden emin olun.

```xml
<ApplicationInsights>
  ...
  <TelemetryModules>
    <Add Type="Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.QuickPulse.QuickPulseTelemetryModule, Microsoft.AI.PerfCounterCollector">
      <QuickPulseServiceEndpoint>Custom_QuickPulse_Endpoint_Address</QuickPulseServiceEndpoint>
    </Add>
  </TelemetryModules>
    ...
  <TelemetryChannel>
     <EndpointAddress>TelemetryChannel_Endpoint_Address</EndpointAddress>
  </TelemetryChannel>
  ...
  <ApplicationIdProvider Type="Microsoft.ApplicationInsights.Extensibility.Implementation.ApplicationId.ApplicationInsightsApplicationIdProvider, Microsoft.ApplicationInsights">
    <ProfileQueryEndpoint>Profile_Query_Endpoint_address</ProfileQueryEndpoint>
  </ApplicationIdProvider>
  ...
</ApplicationInsights>
```

### <a name="aspnet-core"></a>ASP.NET Core

Ana bitiş noktasını ayarlamak için projenizdeki appsettings.json dosyasını aşağıdaki gibi değiştirin:

```json
"ApplicationInsights": {
    "InstrumentationKey": "instrumentationkey",
    "TelemetryChannel": {
      "EndpointAddress": "TelemetryChannel_Endpoint_Address"
    }
  }
```

Canlı Ölçümler ve Profil Sorgusu Bitiş Noktası değerleri yalnızca kod aracılığıyla ayarlanabilir. Kod aracılığıyla tüm uç nokta değerleri için varsayılan değerleri geçersiz `ConfigureServices` kılmak `Startup.cs` için, dosya yönteminde aşağıdaki değişiklikleri yapın:

```csharp
using Microsoft.ApplicationInsights.Extensibility.Implementation.ApplicationId;
using Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.QuickPulse; //Place at top of Startup.cs file

   services.ConfigureTelemetryModule<QuickPulseTelemetryModule>((module, o) => module.QuickPulseServiceEndpoint="QuickPulse_Endpoint_Address");

   services.AddSingleton<IApplicationIdProvider, ApplicationInsightsApplicationIdProvider>(_ => new ApplicationInsightsApplicationIdProvider() { ProfileQueryEndpoint = "Profile_Query_Endpoint_address" });

   services.AddSingleton<ITelemetryChannel>(_ => new ServerTelemetryChannel() { EndpointAddress = "TelemetryChannel_Endpoint_Address" });

    //Place in the ConfigureServices method. Place this before services.AddApplicationInsightsTelemetry("instrumentation key"); if it's present
```

### <a name="azure-functions-v2x"></a>Azure Fonksiyonları v2.x

İşlev projenizde aşağıdaki paketleri yükleyin:

- Microsoft.ApplicationInsights sürüm 2.10.0
- Microsoft.ApplicationInsights.PerfCounterCollector sürüm 2.10.0
- Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel sürüm 2.10.0

Ardından, işlev uygulamanız için başlangıç kodunu ekleyin (veya değiştirin) :

```csharp
[assembly: WebJobsStartup(typeof(Example.Startup))]
namespace Example
{
  class Startup : FunctionsStartup
  {
      public override void Configure(IWebJobsBuilder builder)
      {
          var quickPulseFactory = builder.Services.FirstOrDefault(sd => sd.ServiceType == typeof(ITelemetryModule) && 
                                               sd.ImplementationType == typeof(QuickPulseTelemetryModule));
          if (quickPulseFactory != null)
          {
              builder.Services.Remove(quickPulseFactory);
          }

          var appIdFactory = builder.Services.FirstOrDefault(sd => sd.ServiceType == typeof(IApplicationIdProvider));
          if (appIdFactory != null)
          {
              builder.Services.Remove(appIdFactory);
          }

          var channelFactory = builder.Services.FirstOrDefault(sd => sd.ServiceType == typeof(ITelemetryChannel));
          if (channelFactory != null)
          {
              builder.Services.Remove(channelFactory);
          }

          builder.Services.AddSingleton<ITelemetryModule, QuickPulseTelemetryModule>(_ =>
              new QuickPulseTelemetryModule
              {
                  QuickPulseServiceEndpoint = "QuickPulse_Endpoint_Address"
              });

          builder.Services.AddSingleton<IApplicationIdProvider, ApplicationInsightsApplicationIdProvider>(_ => new ApplicationInsightsApplicationIdProvider() { ProfileQueryEndpoint = "Profile_Query_Endpoint_address" });

          builder.Services.AddSingleton<ITelemetryChannel>(_ => new ServerTelemetryChannel() { EndpointAddress = "TelemetryChannel_Endpoint_Address" });
      }
  }
}
```

### <a name="java"></a>Java

Varsayılan bitiş noktası adresini değiştirmek için applicationinsights.xml dosyasını değiştirin.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationInsights xmlns="http://schemas.microsoft.com/ApplicationInsights/2013/Settings">
  <InstrumentationKey>ffffeeee-dddd-cccc-bbbb-aaaa99998888</InstrumentationKey>
  <TelemetryModules>
    <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebRequestTrackingTelemetryModule"/>
    <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebSessionTrackingTelemetryModule"/>
    <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebUserTrackingTelemetryModule"/>
  </TelemetryModules>
  <TelemetryInitializers>
    <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebOperationIdTelemetryInitializer"/>
    <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebOperationNameTelemetryInitializer"/>
    <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebSessionTelemetryInitializer"/>
    <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebUserTelemetryInitializer"/>
    <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebUserAgentTelemetryInitializer"/>
  </TelemetryInitializers>
  <!--Add the following Channel value to modify the Endpoint address-->
  <Channel type="com.microsoft.applicationinsights.channel.concrete.inprocess.InProcessTelemetryChannel">
  <EndpointAddress>TelemetryChannel_Endpoint_Address</EndpointAddress>
  </Channel>
</ApplicationInsights>
```

### <a name="spring-boot"></a>Spring Boot

Dosyayı `application.properties` değiştirin ve ekleyin:

```yaml
azure.application-insights.channel.in-process.endpoint-address= TelemetryChannel_Endpoint_Address
```

### <a name="nodejs"></a>Node.js

```javascript
var appInsights = require("applicationinsights");
appInsights.setup('INSTRUMENTATION_KEY');
appInsights.defaultClient.config.endpointUrl = "TelemetryChannel_Endpoint_Address"; // ingestion
appInsights.defaultClient.config.profileQueryEndpoint = "Profile_Query_Endpoint_address"; // appid/profile lookup
appInsights.defaultClient.config.quickPulseHost = "QuickPulse_Endpoint_Address"; //live metrics
appInsights.Configuration.start();
```

Uç noktalar çevre değişkenleri aracılığıyla da yapılandırılabilir:

```
Instrumentation Key: "APPINSIGHTS_INSTRUMENTATIONKEY"
Profile Endpoint: "Profile_Query_Endpoint_address"
Live Metrics Endpoint: "QuickPulse_Endpoint_Address"
```

### <a name="javascript"></a>JavaScript

```javascript
<script type="text/javascript">
    var sdkInstance="appInsightsSDK";window[sdkInstance]="appInsights";var aiName=window[sdkInstance],aisdk=window[aiName]||function(e){function n(e){t[e]=function(){var n=arguments;t.queue.push(function(){t[e].apply(t,n)})}}var t={config:e};t.initialize=!0;var i=document,a=window;setTimeout(function(){var n=i.createElement("script");n.src=e.url||"https://az416426.vo.msecnd.net/scripts/b/ai.2.min.js",i.getElementsByTagName("script")[0].parentNode.appendChild(n)});try{t.cookie=i.cookie}catch(e){}t.queue=[],t.version=2;for(var r=["Event","PageView","Exception","Trace","DependencyData","Metric","PageViewPerformance"];r.length;)n("track"+r.pop());n("startTrackPage"),n("stopTrackPage");var s="Track"+r[0];if(n("start"+s),n("stop"+s),n("setAuthenticatedUserContext"),n("clearAuthenticatedUserContext"),n("flush"),!(!0===e.disableExceptionTracking||e.extensionConfig&&e.extensionConfig.ApplicationInsightsAnalytics&&!0===e.extensionConfig.ApplicationInsightsAnalytics.disableExceptionTracking)){n("_"+(r="onerror"));var o=a[r];a[r]=function(e,n,i,a,s){var c=o&&o(e,n,i,a,s);return!0!==c&&t["_"+r]({message:e,url:n,lineNumber:i,columnNumber:a,error:s}),c},e.autoExceptionInstrumented=!0}return t}(
    {
      instrumentationKey:"INSTRUMENTATION_KEY",
      endpointUrl: "TelemetryChannel_Endpoint_Address"
    }
    );window[aiName]=aisdk,aisdk.queue&&0===aisdk.queue.length&&aisdk.trackPageView({});
</script>
```

### <a name="python"></a>Python

Opencensus-python SDK için alım bitiş noktasını değiştirme kılavuzu için [opencensus-python repo'ya başvurun.](https://github.com/census-instrumentation/opencensus-python/blob/af284a92b80bcbaf5db53e7e0813f96691b4c696/contrib/opencensus-ext-azure/opencensus/ext/azure/common/__init__.py)

## <a name="regions-that-require-endpoint-modification"></a>Uç nokta değişikliği gerektiren bölgeler

Şu anda uç nokta değişiklikleri gerektiren tek bölgeler [Azure Kamu](https://docs.microsoft.com/azure/azure-government/documentation-government-services-monitoringandmanagement#application-insights) ve [Azure Çin'dir.](https://docs.microsoft.com/azure/china/resources-developer-guide)

|Bölge |  Bitiş Noktası Adı | Değer |
|-----------------|:------------|:-------------|
| Azure Çin | Telemetri Kanalı | `https://dc.applicationinsights.azure.cn/v2/track` |
| Azure Çin | QuickPulse (Canlı Ölçümler) |`https://live.applicationinsights.azure.cn/QuickPulseService.svc` |
| Azure Çin | Profil Sorgusu |`https://dc.applicationinsights.azure.cn/api/profiles/{0}/appId`  |
| Azure Kamu | Telemetri Kanalı |`https://dc.applicationinsights.us/v2/track` |
| Azure Kamu | QuickPulse (Canlı Ölçümler) |`https://quickpulse.applicationinsights.us/QuickPulseService.svc` |
| Azure Kamu | Profil Sorgusu |`https://dc.applicationinsights.us/api/profiles/{0}/appId` |

Şu anda normalde 'api.applicationinsights.io' üzerinden erişilen [Application Insights REST API'yi](https://dev.applicationinsights.io/
) kullanıyorsanız, bölgenize yerel bir uç nokta kullanmanız gerekir:

|Bölge |  Bitiş Noktası Adı | Değer |
|-----------------|:------------|:-------------|
| Azure Çin | REST API | `api.applicationinsights.azure.cn` |
| Azure Kamu | REST API | `api.applicationinsights.us`|

> [!NOTE]
> Azure Uygulama Hizmetleri için kodsuz aracı/uzantı tabanlı izleme şu anda bu bölgelerde **desteklenmemektedir.** Bu işlevsellik kullanılabilir hale gelir gelmez bu makale güncelleştirilir.

## <a name="next-steps"></a>Sonraki adımlar

- Azure Kamu için özel değişiklikler hakkında daha fazla bilgi edinmek için Azure [izleme ve yönetimi](https://docs.microsoft.com/azure/azure-government/documentation-government-services-monitoringandmanagement#application-insights)için ayrıntılı kılavuza başvurun.
- Azure Çin'i hakkında daha fazla bilgi edinmek için [Azure Çin Oyun Kitabı'na](https://docs.microsoft.com/azure/china/)başvurun.
