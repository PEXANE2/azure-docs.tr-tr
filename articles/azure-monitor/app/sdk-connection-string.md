---
title: Azure Uygulama Öngörüleri'nde bağlantı dizeleri | Microsoft Dokümanlar
description: Bağlantı dizeleri nasıl kullanılır.
ms.topic: conceptual
author: timothymothra
ms.author: tilee
ms.date: 01/17/2020
ms.reviewer: mbullwin
ms.openlocfilehash: 25eda0ae2b0d873fe9850e5b886489a5f2590e69
ms.sourcegitcommit: 27bbda320225c2c2a43ac370b604432679a6a7c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80410621"
---
# <a name="connection-strings"></a>Bağlantı dizeleri

## <a name="overview"></a>Genel Bakış

Bağlantı dizeleri, Application Insight kullanıcılarına tek bir yapılandırma ayarı vererek birden çok proxy ayarına duyulan gereksinimi ortadan kaldırır. İzleme hizmetine veri göndermek isteyen intranet web sunucuları, egemen veya karma bulut ortamları için son derece yararlıdır.

Anahtar değer çiftleri, kullanıcıların her Uygulama Öngörüleri (AI) hizmeti/ ürünü için bir önek sonek birleşimi tanımlamaları için kolay bir yol sağlar.

> [!IMPORTANT]
> Hem Bağlantı String hem de Enstrümantasyon anahtarını ayarlamanızı önermiyoruz. Bir kullanıcının her ikisini de ayarlaması durumunda, en son ayarlanan önceki gün olacaktır. 


## <a name="scenario-overview"></a>Senaryoya genel bakış 

Bunu en çok etkiye sahip olarak görselleştirdiğimiz müşteri senaryoları:

- Güvenlik duvarı özel durumları veya proxy yönlendirmeleri 

    Intranet web sunucusu için izleme nin gerekli olduğu durumlarda, önceki çözümümüz müşterilerden yapılandırmanıza ayrı hizmet uç noktaları eklemelerini istemiştir. Daha fazla bilgi için [buraya](https://docs.microsoft.com/azure/azure-monitor/app/troubleshoot-faq#can-i-monitor-an-intranet-web-server)bakın. 
    Bağlantı dizeleri, bu çabayı tek bir ayara indirgeyerek daha iyi bir alternatif sunar. Basit bir önek, sonek değişikliği otomatik popülasyon ve doğru hizmetleriçin tüm uç noktaların yeniden yönlendirme sağlar. 

- Egemen veya Karma bulut ortamları

    Kullanıcılar tanımlı bir [Azure Devlet Bölgesine](https://docs.microsoft.com/azure/azure-government/documentation-government-services-monitoringandmanagement#application-insights)veri gönderebilir.
    Bağlantı dizeleri, intranet sunucularınız veya karma bulut ayarlarınız için uç nokta ayarlarını tanımlamanıza olanak tanır. 

## <a name="getting-started"></a>Başlarken

### <a name="finding-my-connection-string"></a>Bağlantı dizemi bulmak mı?

Bağlantı dizeniz, Application Insights kaynağınızın Genel Bakış bıçağında görüntülenir.

![genel bakış bıçak üzerinde bağlantı dizesi](media/overview-dashboard/overview-connection-string.png)

### <a name="schema"></a>Şema

#### <a name="max-length"></a>Uzunluk üst sınırı

Bağlantı, en fazla desteklenen 4096 karakter uzunluğundadır.

#### <a name="key-value-pairs"></a>Anahtar-değer çiftleri

Bağlantı dizesi, yarı nokta nokta ile ayrılmış anahtar değer çiftleri olarak temsil edilen ayarların bir listesini oluşturur:`key1=value1;key2=value2;key3=value3`

#### <a name="syntax"></a>Sözdizimi

- `InstrumentationKey`(ör. 0000000-0000-0000-0000-00000000000000)  Bağlantı dizesi **gerekli** bir alandır.
- `Authorization`(ör. ikey) (Bugün yalnızca ikey yetkilendirmeyi desteklediğimiz için bu ayar isteğe bağlıdır.)
- `EndpointSuffix`(ör. applicationinsights.azure.cn) Bitiş noktası sonekinin ayarlanması, SDK'ya hangi Azure bulutuna bağlanacağını bildirir. SDK, bireysel hizmetler için bitiş noktasının geri kalanını bir araya getirecektir.
- Açık Uç Noktalar.
  Herhangi bir hizmet, bağlantı dizesinde açıkça geçersiz kılınabilir.
   - `IngestionEndpoint`(ör.https://dc.applicationinsights.azure.com)
   - `LiveEndpoint`(ör.https://live.applicationinsights.azure.com)
   - `ProfilerEndpoint`(ör.https://profiler.applicationinsights.azure.com)
   - `SnapshotEndpoint`(ör.https://snapshot.applicationinsights.azure.com)

#### <a name="endpoint-schema"></a>Uç nokta şeması

`<prefix>.<suffix>`
- Önek: Bir hizmeti tanımlar. 
- Sonek: Ortak etki alanı adını tanımlar.

##### <a name="valid-suffixes"></a>Geçerli sonekler

Geçerli soneklerin listesi aşağıda veda edebilirsiniz 
- applicationinsights.azure.cn
- applicationinsights.us


Ayrıca bakınız:https://docs.microsoft.com/azure/azure-monitor/app/custom-endpoints#regions-that-require-endpoint-modification


##### <a name="valid-prefixes"></a>Geçerli önekler

- [Telemetri Yutma](./app-insights-overview.md):`dc`
- [Canlı Ölçümler](./live-stream.md):`live`
- [Profilci](./profiler-overview.md):`profiler`
- [Anlık görüntü](./snapshot-debugger.md):`snapshot`



## <a name="connection-string-examples"></a>Bağlantı dize örnekleri


### <a name="minimal-valid-connection-string"></a>En az geçerli bağlantı dizesi

`InstrumentationKey=00000000-0000-0000-0000-000000000000;`

Bu örnekte, yalnızca Enstrümantasyon Anahtarı ayarlanmıştır.

- Yetkilendirme düzeni varsayılan olarak "ikey" 
- Enstrümantasyon Anahtarı: 00000000-0000-0000-00000-00000000000
- Bölgesel hizmet URI'leri [SDK varsayılanlarına](https://github.com/microsoft/ApplicationInsights-dotnet/blob/e50d569cebf485e72e98f4a08a0bc0e30cdf42bc/BASE/src/Microsoft.ApplicationInsights/Extensibility/Implementation/Endpoints/Constants.cs#L6) dayanır ve genel azure'a bağlanır:
   - Yenmesi:https://dc.services.visualstudio.com/
   - Canlı ölçümler:https://rt.services.visualstudio.com/
   - Profiler:https://agent.azureserviceprofiler.net/
   - Hata ayıklayıcı:https://agent.azureserviceprofiler.net/  



### <a name="connection-string-with-endpoint-suffix"></a>Bitiş noktası soneki ile bağlantı dizesi

`InstrumentationKey=00000000-0000-0000-0000-000000000000;EndpointSuffix=ai.contoso.com;`

Bu örnekte, bu bağlantı dizesi bitiş noktası soneki belirtir ve SDK hizmet uç noktaları inşa eder.

- Yetkilendirme düzeni varsayılan olarak "ikey" 
- Enstrümantasyon Anahtarı: 00000000-0000-0000-00000-00000000000
- Bölgesel hizmet URI'leri sağlanan uç nokta sonekine dayanır: 
   - Yenmesi:https://dc.ai.contoso.com
   - Canlı ölçümler:https://live.ai.contoso.com
   - Profiler:https://profiler.ai.contoso.com 
   - Hata ayıklayıcı:https://snapshot.ai.contoso.com   



### <a name="connection-string-with-explicit-endpoint-overrides"></a>Açık uç nokta geçersiz kılarile bağlantı dizesi 

`InstrumentationKey=00000000-0000-0000-0000-000000000000;IngestionEndpoint=https://custom.com:111/;LiveEndpoint=https://custom.com:222/;ProfilerEndpoint=https://custom.com:333/;SnapshotEndpoint=https://custom.com:444/;`

Bu örnekte, bu bağlantı dizesi her hizmet için açık geçersiz kılmaları belirtir. SDK, herhangi bir değişiklik yapılmadan sağlanan tam uç noktaları kullanır.

- Yetkilendirme düzeni varsayılan olarak "ikey" 
- Enstrümantasyon Anahtarı: 00000000-0000-0000-00000-00000000000
- Bölgesel hizmet URI'leri açık geçersiz kılma değerlerini temel alar: 
   - Yutma: https:\//custom.com:111/
   - Canlı ölçümler: https:\//custom.com:222/
   - Profilci: https:\//custom.com:333/ 
   - Hata Ayıklama:\/https: /custom.com:444/   


## <a name="how-to-set-a-connection-string"></a>Bağlantı dizesi nasıl ayarlanır?

Bağlantı Dizeleri aşağıdaki SDK sürümlerinde desteklenir:
- .NET ve .NET Çekirdek v2.12.0
- Java v2.5.1 ve Java 3.0
- Javascript v2.3.0
- NodeJS v1.5.0
- Python v1.0.0

Bağlantı dizesi kod, ortam değişkeni veya yapılandırma dosyasında ayarlanabilir.



### <a name="environment-variable"></a>Ortam değişkeni

- Bağlantı Dizesi:`APPLICATIONINSIGHTS_CONNECTION_STRING`

# <a name="netnetcore"></a>[.NET/.NetCore](#tab/net)

TelemetriConfiguration.ConnectionString:https://github.com/microsoft/ApplicationInsights-dotnet/blob/add45ceed35a817dc7202ec07d3df1672d1f610d/BASE/src/Microsoft.ApplicationInsights/Extensibility/TelemetryConfiguration.cs#L271-L274

.Net Açıkça Ayarlayın:
```csharp
var configuration = new TelemetryConfiguration
{
    ConnectionString = "InstrumentationKey=00000000-0000-0000-0000-000000000000;"
};
```

.Net Config Dosyası:

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationInsights xmlns="http://schemas.microsoft.com/ApplicationInsights/2013/Settings">
    <ConnectionString>InstrumentationKey=00000000-0000-0000-0000-000000000000</ConnectionString>
</ApplicationInsights>
```


NetCore config.json: 

```json
{
  "ApplicationInsights": {
    "ConnectionString" : "InstrumentationKey=00000000-0000-0000-0000-000000000000;"
    }
  }
```


# <a name="java"></a>[Java](#tab/java)


Java (v2.5.x) Açıkça Ayarlayın:
```java
TelemetryConfiguration.getActive().setConnectionString("InstrumentationKey=00000000-0000-0000-0000-000000000000");
```

UygulamaInsights.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationInsights xmlns="http://schemas.microsoft.com/ApplicationInsights/2013/Settings">
    <ConnectionString>InstrumentationKey=00000000-0000-0000-0000-000000000000;</ConnectionString>
</ApplicationInsights>
```

# <a name="javascript"></a>[JavaScript](#tab/js)

Önemli: Javascript Çevre Değişkenlerinin kullanımını desteklemez.

Parçacığı kullanma:

```javascript
<script type="text/javascript">
var sdkInstance="appInsightsSDK";window[sdkInstance]="appInsights";var aiName=window[sdkInstance],aisdk=window[aiName]||function(e){function n(e){t[e]=function(){var n=arguments;t.queue.push(function(){t[e].apply(t,n)})}}var t={config:e};t.initialize=!0;var i=document,a=window;setTimeout(function(){var n=i.createElement("script");n.src=e.url||"https://az416426.vo.msecnd.net/scripts/b/ai.2.min.js",i.getElementsByTagName("script")[0].parentNode.appendChild(n)});try{t.cookie=i.cookie}catch(e){}t.queue=[],t.version=2;for(var r=["Event","PageView","Exception","Trace","DependencyData","Metric","PageViewPerformance"];r.length;)n("track"+r.pop());n("startTrackPage"),n("stopTrackPage");var s="Track"+r[0];if(n("start"+s),n("stop"+s),n("addTelemetryInitializer"),n("setAuthenticatedUserContext"),n("clearAuthenticatedUserContext"),n("flush"),t.SeverityLevel={Verbose:0,Information:1,Warning:2,Error:3,Critical:4},!(!0===e.disableExceptionTracking||e.extensionConfig&&e.extensionConfig.ApplicationInsightsAnalytics&&!0===e.extensionConfig.ApplicationInsightsAnalytics.disableExceptionTracking)){n("_"+(r="onerror"));var o=a[r];a[r]=function(e,n,i,a,s){var c=o&&o(e,n,i,a,s);return!0!==c&&t["_"+r]({message:e,url:n,lineNumber:i,columnNumber:a,error:s}),c},e.autoExceptionInstrumented=!0}return t}(
{
  connectionString:"InstrumentationKey=00000000-0000-0000-0000-000000000000;"
}
);window[aiName]=aisdk,aisdk.queue&&0===aisdk.queue.length&&aisdk.trackPageView({});
</script>
```


Manuel Kurulum:
```javascript
import { ApplicationInsights } from '@microsoft/applicationinsights-web'

const appInsights = new ApplicationInsights({ config: {
  connectionString: 'InstrumentationKey=00000000-0000-0000-0000-000000000000;'
  /* ...Other Configuration Options... */
} });
appInsights.loadAppInsights();
appInsights.trackPageView();
```

# <a name="nodejs"></a>[Node.js](#tab/nodejs)

```javascript
const appInsights = require("applicationinsights");
appInsights.setup("InstrumentationKey=00000000-0000-0000-0000-000000000000;");
appInsights.start();
```

# <a name="python"></a>[Python](#tab/python)

Kullanıcıların ortam değişkenini ayarlamalarını öneririz.

Bağlantı dizesini açıkça ayarlamak için:

```python
from opencensus.ext.azure.trace_exporter import AzureExporter
from opencensus.trace.samplers import ProbabilitySampler
from opencensus.trace.tracer import Tracer

tracer = Tracer(exporter=AzureExporter(connection_string='InstrumentationKey=00000000-0000-0000-0000-000000000000'), sampler=ProbabilitySampler(1.0))
```


## <a name="next-steps"></a>Sonraki adımlar

Çalışma zamanında şunlarla kullanmaya başlayın:

* [Azure VM ve Azure sanal makine ölçeği IIS tarafından barındırılan uygulamaları ayarla](../../azure-monitor/app/azure-vm-vmss-apps.md)
* [IIS sunucusu](../../azure-monitor/app/monitor-performance-live-website-now.md)
* [Azure Web Apps](../../azure-monitor/app/azure-web-apps.md)

Geliştirme zamanında şunlarla kullanmaya başlayın:

* [ASP.NET](../../azure-monitor/app/asp-net.md)
* [ASP.NET Core](../../azure-monitor/app/asp-net-core.md)
* [Java](../../azure-monitor/app/java-get-started.md)
* [Node.js](../../azure-monitor/app/nodejs.md)
* [Python](../../azure-monitor/app/opencensus-python.md)
