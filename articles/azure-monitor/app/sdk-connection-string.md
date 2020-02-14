---
title: Azure Application Insights bağlantı dizeleri | Microsoft Docs
description: Bağlantı dizelerini kullanma.
ms.service: azure-monitor
ms.subservice: application-insights
ms.topic: conceptual
author: timothymothra
ms.author: tilee
ms.date: 01/17/2020
ms.reviewer: mbullwin
ms.openlocfilehash: 8febe1fd749842a6db0cd1c9991f4002eb7cf2ad
ms.sourcegitcommit: b07964632879a077b10f988aa33fa3907cbaaf0e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2020
ms.locfileid: "77190088"
---
# <a name="connection-strings"></a>Bağlantı dizeleri

## <a name="overview"></a>Genel Bakış

Bağlantı dizeleri, birden çok proxy ayarı gereksinimini ortadan kaldıran, Application Insights kullanıcılarına tek bir yapılandırma ayarı sağlar. Verileri izleme hizmetine göndermek isteyen intranet Web sunucuları, sogeign veya hibrit bulut ortamları için son derece kullanışlıdır.

Anahtar değer çiftleri, kullanıcıların her bir Application Insights (AI) hizmeti/ürünü için bir önek son eki birleşimini tanımlamak için kolay bir yol sağlar.

> [!IMPORTANT]
> Hem bağlantı dizesini hem de Izleme anahtarını ayarlamayı önermiyoruz. Bir kullanıcının her ikisini de ayarlaması durumunda, en son ayarlanan ilke öncelikli olur. 


## <a name="scenario-overview"></a>Senaryoya genel bakış 

Bu, en etkili olduğunu görselleştirdiğimiz müşteri senaryolarında:

- Güvenlik Duvarı özel durumları veya proxy yönlendirmeleri 

    Intranet web sunucusu için izlemenin gerekli olduğu durumlarda, daha önceki çözümümüzde müşterilerin yapılandırmanıza ayrı hizmet uç noktaları eklemesi istenir. Daha fazla bilgi için [buraya](https://docs.microsoft.com/azure/azure-monitor/app/troubleshoot-faq#can-i-monitor-an-intranet-web-server)bakın. 
    Bağlantı dizeleri, bu çabayı tek bir ayara azaltarak daha iyi bir alternatif sunar. Basit önek, sonek değişikliği, tüm uç noktaların doğru hizmetlere otomatik olarak doldurulmasını ve yeniden yönlendirilmesine olanak tanır. 

- Sogeign veya hibrit bulut ortamları

    Kullanıcılar, tanımlı bir [Azure Kamu bölgesine](https://docs.microsoft.com/azure/azure-government/documentation-government-services-monitoringandmanagement#application-insights)veri gönderebilir.
    Bağlantı dizeleri, intranet sunucularınız veya karma bulut ayarları için uç nokta ayarlarını tanımlamanızı sağlar. 

## <a name="getting-started"></a>Başlarken

### <a name="finding-my-connection-string"></a>Bağlantı dizim mi bulunuyor?

Bağlantı dizeniz Application Insights kaynağınızın genel bakış dikey penceresinde görüntülenir.

![Genel Bakış dikey penceresinde bağlantı dizesi](media/overview-dashboard/overview-connection-string.png)

### <a name="schema"></a>Şema

#### <a name="max-length"></a>En fazla uzunluk

Bağlantı desteklenen en fazla 4096 karakter uzunluğunda.

#### <a name="key-value-pairs"></a>Anahtar-değer çiftleri

Bağlantı dizesi, noktalı virgülle ayrılmış anahtar-değer çiftleri olarak temsil edilen ayarların bir listesini içerir: `key1=value1;key2=value2;key3=value3`

#### <a name="syntax"></a>Sözdizimi

- `InstrumentationKey` (örn: 00000000-0000-0000-0000-000000000000) bağlantı dizesi **gerekli** bir alandır.
- `Authorization` (örn: Ikey) (bugün yalnızca Ikey yetkilendirmesini desteklediğimiz için bu ayar isteğe bağlıdır.)
- `EndpointSuffix` (örn: applicationinsights.azure.cn) uç nokta son ekinin ayarlanması, SDK 'nın hangi Azure bulutunun bağlanmasına yol açacaktır. SDK, her bir hizmet için uç noktanın geri kalanını birleştirir.
- Açık uç noktalar.
  Bağlantı dizesinde, herhangi bir hizmet açıkça geçersiz kılınabilir.
   - `IngestionEndpoint` (örn: https://dc.applicationinsights.azure.com)
   - `LiveEndpoint` (örn: https://live.applicationinsights.azure.com)
   - `ProfilerEndpoint` (örn: https://profiler.applicationinsights.azure.com)
   - `SnapshotEndpoint` (örn: https://snapshot.applicationinsights.azure.com)

#### <a name="endpoint-schema"></a>Uç nokta şeması

`<prefix>.<suffix>`
- Ön ek: bir hizmeti tanımlar. 
- Sonek: ortak etki alanı adını tanımlar.

##### <a name="valid-suffixes"></a>Geçerli sonekler

Geçerli sonekler listesi aşağıdadır 
- applicationinsights.azure.cn
- applicationinsights.us


Ayrıca bkz: https://docs.microsoft.com/azure/azure-monitor/app/custom-endpoints#regions-that-require-endpoint-modification


##### <a name="valid-prefixes"></a>Geçerli ön ekler

- [Telemetri](./app-insights-overview.md)alımı: `dc`
- [Canlı ölçümler](./live-stream.md): `live`
- [Profil Oluşturucu](./profiler-overview.md): `profiler`
- [Anlık görüntü](./snapshot-debugger.md): `snapshot`



## <a name="connection-string-examples"></a>Bağlantı dizesi örnekleri


### <a name="minimal-valid-connection-string"></a>En az geçerli bağlantı dizesi

`InstrumentationKey=00000000-0000-0000-0000-000000000000;`

Bu örnekte, yalnızca Izleme anahtarı ayarlanmıştır.

- Yetkilendirme düzeni varsayılan olarak "Ikey" olarak belirlenmiştir 
- İzleme anahtarı: 00000000-0000-0000-0000-000000000000
- Bölgesel hizmet URI 'Leri, [SDK varsayılanlarına](https://github.com/microsoft/ApplicationInsights-dotnet/blob/e50d569cebf485e72e98f4a08a0bc0e30cdf42bc/BASE/src/Microsoft.ApplicationInsights/Extensibility/Implementation/Endpoints/Constants.cs#L6) dayanır ve genel Global Azure 'a bağlanır:
   - Alma: https://dc.services.visualstudio.com/
   - Canlı ölçümler: https://rt.services.visualstudio.com/
   - Profil Oluşturucu: https://agent.azureserviceprofiler.net/
   - Hata ayıklayıcı: https://agent.azureserviceprofiler.net/  



### <a name="connection-string-with-endpoint-suffix"></a>Uç nokta sonekine sahip bağlantı dizesi

`InstrumentationKey=00000000-0000-0000-0000-000000000000;EndpointSuffix=ai.contoso.com;`

Bu örnekte, bu bağlantı dizesi uç nokta sonekini belirtir ve SDK hizmet uç noktaları oluşturur.

- Yetkilendirme düzeni varsayılan olarak "Ikey" olarak belirlenmiştir 
- İzleme anahtarı: 00000000-0000-0000-0000-000000000000
- Bölgesel hizmet URI 'Leri, belirtilen uç nokta sonekine dayalıdır: 
   - Alma: https://dc.ai.contoso.com
   - Canlı ölçümler: https://live.ai.contoso.com
   - Profil Oluşturucu: https://profiler.ai.contoso.com 
   - Hata ayıklayıcı: https://snapshot.ai.contoso.com   



### <a name="connection-string-with-explicit-endpoint-overrides"></a>Açık uç nokta geçersiz kılmalarla bağlantı dizesi 

`InstrumentationKey=00000000-0000-0000-0000-000000000000;IngestionEndpoint=https://custom.com:111/;LiveEndpoint=https://custom.com:222/;ProfilerEndpoint=https://custom.com:333/;SnapshotEndpoint=https://custom.com:444/;`

Bu örnekte, bu bağlantı dizesi her hizmet için açık geçersiz kılmalar belirler. SDK, değişiklik yapılmadan belirtilen tam uç noktaları kullanır.

- Yetkilendirme düzeni varsayılan olarak "Ikey" olarak belirlenmiştir 
- İzleme anahtarı: 00000000-0000-0000-0000-000000000000
- Bölgesel hizmet URI 'Leri açık geçersiz kılma değerlerine dayalıdır: 
   - Alma: https:\//custom.com:111/
   - Canlı ölçümler: https:\//custom.com:222/
   - Profil Oluşturucu: https:\//custom.com:333/ 
   - Hata ayıklayıcı: https:\//custom.com:444/   


## <a name="how-to-set-a-connection-string"></a>Bağlantı dizesi ayarlama

Bağlantı dizeleri aşağıdaki SDK sürümlerinde desteklenir:
- .NET ve .NET Core v 2.12.0
- Java v 2.5.1
- JavaScript v 2.3.0
- NodeJS v 1.5.0
- Python v 1.0.0

Bir bağlantı dizesi, kod, ortam değişkeni veya yapılandırma dosyası aracılığıyla ayarlanabilir.



### <a name="environment-variable"></a>Ortam değişkeni

- Bağlantı dizesi: `APPLICATIONINSIGHTS_CONNECTION_STRING`

### <a name="net-sdk-example"></a>.NET SDK örneği

TelemetryConfiguration. ConnectionString: https://github.com/microsoft/ApplicationInsights-dotnet/blob/add45ceed35a817dc7202ec07d3df1672d1f610d/BASE/src/Microsoft.ApplicationInsights/Extensibility/TelemetryConfiguration.cs#L271-L274

.Net açıkça ayarlanmış:
```csharp
var configuration = new TelemetryConfiguration
{
    ConnectionString = "InstrumentationKey=00000000-0000-0000-0000-000000000000;"
};
```

.NET yapılandırma dosyası:

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationInsights xmlns="http://schemas.microsoft.com/ApplicationInsights/2013/Settings">
    <ConnectionString>InstrumentationKey=00000000-0000-0000-0000-000000000000</ConnectionString>
</ApplicationInsights>
```


NetCore config. JSON: 

```json
{
  "ApplicationInsights": {
    "ConnectionString" : "InstrumentationKey=00000000-0000-0000-0000-000000000000;"
    }
  }
```


### <a name="java-sdk-example"></a>Java SDK örneği


Java açıkça ayarlanmış:
```java
TelemetryConfiguration.getActive().setConnectionString("InstrumentationKey=00000000-0000-0000-0000-000000000000");
```

ApplicationInsights. xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationInsights xmlns="http://schemas.microsoft.com/ApplicationInsights/2013/Settings">
    <ConnectionString>InstrumentationKey=00000000-0000-0000-0000-000000000000;</ConnectionString>
</ApplicationInsights>
```

### <a name="javascript-sdk-example"></a>JavaScript SDK örneği

Önemli: JavaScript ortam değişkenlerinin kullanımını desteklemez.

Kod parçacığını kullanma:

```javascript
<script type="text/javascript">
var sdkInstance="appInsightsSDK";window[sdkInstance]="appInsights";var aiName=window[sdkInstance],aisdk=window[aiName]||function(e){function n(e){t[e]=function(){var n=arguments;t.queue.push(function(){t[e].apply(t,n)})}}var t={config:e};t.initialize=!0;var i=document,a=window;setTimeout(function(){var n=i.createElement("script");n.src=e.url||"https://az416426.vo.msecnd.net/scripts/b/ai.2.min.js",i.getElementsByTagName("script")[0].parentNode.appendChild(n)});try{t.cookie=i.cookie}catch(e){}t.queue=[],t.version=2;for(var r=["Event","PageView","Exception","Trace","DependencyData","Metric","PageViewPerformance"];r.length;)n("track"+r.pop());n("startTrackPage"),n("stopTrackPage");var s="Track"+r[0];if(n("start"+s),n("stop"+s),n("addTelemetryInitializer"),n("setAuthenticatedUserContext"),n("clearAuthenticatedUserContext"),n("flush"),t.SeverityLevel={Verbose:0,Information:1,Warning:2,Error:3,Critical:4},!(!0===e.disableExceptionTracking||e.extensionConfig&&e.extensionConfig.ApplicationInsightsAnalytics&&!0===e.extensionConfig.ApplicationInsightsAnalytics.disableExceptionTracking)){n("_"+(r="onerror"));var o=a[r];a[r]=function(e,n,i,a,s){var c=o&&o(e,n,i,a,s);return!0!==c&&t["_"+r]({message:e,url:n,lineNumber:i,columnNumber:a,error:s}),c},e.autoExceptionInstrumented=!0}return t}(
{
  connectionString:"InstrumentationKey=00000000-0000-0000-0000-000000000000;"
}
);window[aiName]=aisdk,aisdk.queue&&0===aisdk.queue.length&&aisdk.trackPageView({});
</script>
```


El ile kurulum:
```javascript
import { ApplicationInsights } from '@microsoft/applicationinsights-web'

const appInsights = new ApplicationInsights({ config: {
  connectionString: 'InstrumentationKey=00000000-0000-0000-0000-000000000000;'
  /* ...Other Configuration Options... */
} });
appInsights.loadAppInsights();
appInsights.trackPageView();
```

### <a name="node-sdk-example"></a>Node SDK örneği

```javascript
const appInsights = require("applicationinsights");
appInsights.setup("InstrumentationKey=00000000-0000-0000-0000-000000000000;");
appInsights.start();
```

### <a name="python-sdk-example"></a>Python SDK örneği

Kullanıcıların ortam değişkenini ayarlamanızı öneririz.

Bağlantı dizesini açıkça ayarlamak için:

```python
from opencensus.ext.azure.trace_exporter import AzureExporter
from opencensus.trace.samplers import ProbabilitySampler
from opencensus.trace.tracer import Tracer

tracer = Tracer(exporter=AzureExporter(connection_string='InstrumentationKey=00000000-0000-0000-0000-000000000000'), sampler=ProbabilitySampler(1.0))
```


## <a name="next-steps"></a>Sonraki adımlar

Çalışma zamanında şunlarla kullanmaya başlayın:

* [Azure VM ve Azure sanal makine ölçek kümesi IIS tarafından barındırılan uygulamalar](../../azure-monitor/app/azure-vm-vmss-apps.md)
* [IIS sunucusu](../../azure-monitor/app/monitor-performance-live-website-now.md)
* [Azure Web Apps](../../azure-monitor/app/azure-web-apps.md)

Geliştirme zamanında şunlarla kullanmaya başlayın:

* [ASP.NET](../../azure-monitor/app/asp-net.md)
* [ASP.NET Core](../../azure-monitor/app/asp-net-core.md)
* [Java](../../azure-monitor/app/java-get-started.md)
* [Node.js](../../azure-monitor/app/nodejs.md)
* [Python (Önizleme)](../../azure-monitor/app/opencensus-python.md)
