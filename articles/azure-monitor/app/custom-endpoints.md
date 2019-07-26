---
title: Azure Izleyici-Azure Application Insights geçersiz kılma varsayılan SDK uç noktaları | Microsoft Docs
description: Azure Kamu gibi bölgeler için varsayılan Azure Application Insights SDK uç noktalarını değiştirin.
services: application-insights
author: mrbullwinkle
manager: carmonm
ms.assetid: 3b722e47-38bd-4667-9ba4-65b7006c074c
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 07/24/2019
ms.author: mbullwin
ms.openlocfilehash: c086f94a161853cba3a9ed2b98f13ea17b90dd20
ms.sourcegitcommit: 75a56915dce1c538dc7a921beb4a5305e79d3c7a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2019
ms.locfileid: "68478810"
---
 # <a name="application-insights-overriding-default-endpoints"></a>Varsayılan uç noktaları geçersiz kılmak Application Insights

Application Insights verileri belirli bölgelere göndermek için varsayılan uç nokta adreslerini geçersiz kılmanız gerekir. Her SDK, hepsi bu makalede açıklanan biraz farklı değişiklik gerektirir. Bu değişiklikler, örnek kodu ayarlamayı ve,, ve `QuickPulse_Endpoint_Address` `Profile_Query_Endpoint_address` için yer tutucu değerlerini `TelemetryChannel_Endpoint_Address`, belirli bölgeniz için gerçek uç nokta adresleriyle değiştirmeyi gerektirir. Bu makalenin sonunda, bu yapılandırmanın gerekli olduğu bölgelere yönelik uç nokta adreslerinin bağlantıları bulunur.

## <a name="sdk-code-changes"></a>SDK kodu değişiklikleri

### <a name="net-with-applicationinsightsconfig"></a>ApplicationInsights. config ile .NET

> [!NOTE]
> Her bir SDK yükseltmesi gerçekleştirildiğinde ApplicationInsights. config dosyası otomatik olarak üzerine yazılır. SDK yükseltmesini gerçekleştirdikten sonra bölgeye özgü uç nokta değerlerini yeniden girdiğinizden emin olun.

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

### <a name="net-core"></a>.NET Core

Ana uç noktayı ayarlamak için projenizdeki appSettings. json dosyasını aşağıdaki şekilde değiştirin:

```json
"ApplicationInsights": {
    "InstrumentationKey": "instrumentationkey",
    "TelemetryChannel": {
      "EndpointAddress": "TelemetryChannel_Endpoint_Address"
    }
  }
```

Canlı ölçümler ve profil sorgu uç noktası değerleri yalnızca kod aracılığıyla ayarlanabilir. Tüm uç nokta değerlerinin varsayılan değerlerini kod aracılığıyla geçersiz kılmak için, `ConfigureServices` `Startup.cs` dosyanın yönteminde aşağıdaki değişiklikleri yapın:

```csharp
using Microsoft.ApplicationInsights.Extensibility.Implementation.ApplicationId;
using Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.QuickPulse; //place at top of Startup.cs file

   services.ConfigureTelemetryModule<QuickPulseTelemetryModule>((module, o) => module.QuickPulseServiceEndpoint="QuickPulse_Endpoint_Address");

   services.AddSingleton(new ApplicationInsightsApplicationIdProvider() { ProfileQueryEndpoint = "Profile_Query_Endpoint_address" });

   services.AddSingleton<ITelemetryChannel>(new ServerTelemetryChannel() { EndpointAddress = "TelemetryChannel_Endpoint_Address" });

    //place in ConfigureServices method. If present, place this prior to   services.AddApplicationInsightsTelemetry("instrumentation key");
```

### <a name="java"></a>Java

ApplicationInsights. xml dosyasını değiştirerek varsayılan uç nokta adresini değiştirin.

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

`application.properties` Dosyayı değiştirin ve ekleyin:

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

Uç noktalar, ortam değişkenleri aracılığıyla da yapılandırılabilir:

```
Instrumentation Key: "APPINSIGHTS_INSTRUMENTATIONKEY"
Profile Endpoint: "Profile_Query_Endpoint_address"
Live Metrics Endpoint: "QuickPulse_Endpoint_Address"
```

### <a name="javascript"></a>JavaScript

```javascript
<script type="text/javascript">
   var sdkInstance="appInsightsSDK";window[sdkInstance]="appInsights";var aiName=window[sdkInstance],aisdk=window[aiName]||function(e){
      function n(e){t[e]=function(){var n=arguments;t.queue.push(function(){t[e].apply(t,n)})}}var t={config:e};t.initialize=!0;var i=document,a=window;setTimeout(function(){var n=i.createElement("script");n.src=e.url||"https://az416426.vo.msecnd.net/next/ai.2.min.js",i.getElementsByTagName("script")[0].parentNode.appendChild(n)});try{t.cookie=i.cookie}catch(e){}t.queue=[],t.version=2;for(var r=["Event","PageView","Exception","Trace","DependencyData","Metric","PageViewPerformance"];r.length;)n("track"+r.pop());n("startTrackPage"),n("stopTrackPage");var s="Track"+r[0];if(n("start"+s),n("stop"+s),n("setAuthenticatedUserContext"),n("clearAuthenticatedUserContext"),n("flush"),!(!0===e.disableExceptionTracking||e.extensionConfig&&e.extensionConfig.ApplicationInsightsAnalytics&&!0===e.extensionConfig.ApplicationInsightsAnalytics.disableExceptionTracking)){n("_"+(r="onerror"));var o=a[r];a[r]=function(e,n,i,a,s){var c=o&&o(e,n,i,a,s);return!0!==c&&t["_"+r]({message:e,url:n,lineNumber:i,columnNumber:a,error:s}),c},e.autoExceptionInstrumented=!0}return t
   }({
      instrumentationKey:"INSTRUMENTATION_KEY"
      endpointUrl: "TelemetryChannel_Endpoint_Address"
   });

   window[aiName]=aisdk,aisdk.queue&&0===aisdk.queue.length&&aisdk.trackPageView({});
</script>
```

## <a name="regions-that-require-endpoint-modification"></a>Uç nokta değişikliği gerektiren bölgeler

Şu anda yalnızca uç nokta değişiklikleri gerektiren bölgeler [Azure Kamu](https://docs.microsoft.com/azure/azure-government/documentation-government-services-monitoringandmanagement#application-insights) ve [Azure Çin](https://docs.microsoft.com/azure/china/resources-developer-guide)' dir.

|Bölge |  Uç nokta adı | Value |
|-----------------|:------------|:-------------|
| Azure Çin | Telemetri kanalı | `https://dc.applicationinsights.azure.cn/v2/track` |
| Azure Çin | QuickPulse (canlı ölçümler) |`https://live.applicationinsights.azure.cn/QuickPulseService.svc` |
| Azure Çin | Profil sorgusu |`https://dc.applicationinsights.azure.cn/api/profiles/{0}/appId`  |
| Azure Kamu | Telemetri kanalı |`https://dc.applicationinsights.us/v2/track` |
| Azure Kamu | QuickPulse (canlı ölçümler) |`https://quickpulse.applicationinsights.us/QuickPulseService.svc` |
| Azure Kamu | Profil sorgusu |`https://dc.applicationinsights.us/api/profiles/{0}/appId` |

> [!NOTE]
> Azure Uygulama Hizmetleri için codeless Aracısı/uzantısı tabanlı izleme şu **anda** bu bölgelerde desteklenmiyor. Bu işlevsellik kullanılabilir hale geldiğinde, bu makale güncelleştirilir.

## <a name="next-steps"></a>Sonraki adımlar

- Azure Kamu ile ilgili özel değişiklikler hakkında daha fazla bilgi edinmek için [Azure izleme ve yönetimine](https://docs.microsoft.com/azure/azure-government/documentation-government-services-monitoringandmanagement#application-insights)yönelik ayrıntılı kılavuza başvurun.
- Azure Çin hakkında daha fazla bilgi edinmek için [Azure Çin PlayBook](https://docs.microsoft.com/azure/china/)' a bakın.