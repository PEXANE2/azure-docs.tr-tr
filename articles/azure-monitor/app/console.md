---
title: Konsol Uygulamaları için Azure Uygulama Öngörüleri | Microsoft Dokümanlar
description: Kullanılabilirlik, performans ve kullanım için web uygulamalarını izleyin.
ms.topic: conceptual
ms.date: 12/02/2019
ms.reviewer: lmolkova
ms.openlocfilehash: baaea0f8055eeff0314fcf5fde00729ea8091d12
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77655438"
---
# <a name="application-insights-for-net-console-applications"></a>.NET konsol uygulamaları için Uygulama Öngörüleri

[Uygulama Öngörüleri,](../../azure-monitor/app/app-insights-overview.md) kullanılabilirlik, performans ve kullanım için web uygulamanızı izlemenize olanak tanır.

[Microsoft Azure](https://azure.com)ile bir aboneliğe ihtiyacınız var. Windows, Xbox Live veya diğer Microsoft bulut hizmetleri için sahip olabileceğiniz bir Microsoft hesabıyla oturum açın. Ekibinizin Azure'a kurumsal bir aboneliği olabilir: Microsoft hesabınızı kullanarak sahibinden sizi bu aboneye eklemesini isteyin.

> [!NOTE]
> [Microsoft.ApplicationInsights.WorkerService](https://www.nuget.org/packages/Microsoft.ApplicationInsights.WorkerService) adında, herhangi bir Konsol Uygulamaları için Uygulama Öngörülerini etkinleştirmek için kullanılabilen yeni bir Uygulama Öngörüleri SDK vardır. Bu paketi ve ilişkili talimatları [buradan](../../azure-monitor/app/worker-service.md)kullanmanız önerilir. Bu paket [`NetStandard2.0`](https://docs.microsoft.com/dotnet/standard/net-standard),.NET Core 2.0 veya üstü ve .NET Framework 4.7.2 veya daha yüksek alanlarda kullanılabilir.

## <a name="getting-started"></a>Başlarken

* [Azure portalında](https://portal.azure.com)[bir Application Insights kaynağı oluşturun](../../azure-monitor/app/create-new-resource.md). Uygulama türü için **Genel'i**seçin.
* İzleme Anahtarının bir kopyasını oluşturun. Oluşturduğunuz yeni kaynağın **Essentials** açılır tesliminde anahtarı bulun.
* En son [Microsoft.ApplicationInsights](https://www.nuget.org/packages/Microsoft.ApplicationInsights) paketini yükleyin.
* Herhangi bir telemetriyi izlemeden (veya ortam değişkenini ayarlamadan veya APPINSIGHTS_INSTRUMENTATIONKEY ayarla) önce kodunuzda enstrümantasyon anahtarını ayarlayın. Bundan sonra, telemetriyi el ile takip edebilmeli ve Azure portalında görebilmelisiniz.

```csharp
// you may use different options to create configuration as shown later in this article
TelemetryConfiguration configuration = TelemetryConfiguration.CreateDefault();
configuration.InstrumentationKey = " *your key* ";
var telemetryClient = new TelemetryClient(configuration);
telemetryClient.TrackTrace("Hello World!");
```

> [!NOTE]
> Telemetri anında gönderilmez. Telemetri öğeleri toplu olarak paketlenir ve ApplicationInsights SDK tarafından gönderilir. Arama `Track()` yöntemlerinden hemen sonra çıkan Konsol uygulamalarında, bu makalede `Flush()` `Sleep` [tam örnek](#full-example) olarak gösterildiği gibi uygulama çıkmadan önce telemetri gönderilmeyebilir.


* [Microsoft.ApplicationInsights.DependencyCollector](https://www.nuget.org/packages/Microsoft.ApplicationInsights.DependencyCollector) paketinin en son sürümünü yükleyin - http, SQL veya diğer bazı dış bağımlılık çağrılarını otomatik olarak izler.

Uygulama Öngörülerini koddan veya dosyayı kullanarak `ApplicationInsights.config` başolarak açabilir ve yapılandırabilirsiniz. Başlatmanın mümkün olduğunca erken gerçekleştiğinden emin olun. 

> [!NOTE]
> **ApplicationInsights.config'e** atıfta bulunan talimatlar yalnızca .NET Framework'u hedefleyen uygulamalar için geçerlidir ve .NET Core uygulamaları için geçerli değildir.

### <a name="using-config-file"></a>Config dosyayı kullanma

Varsayılan olarak, Application Insights `ApplicationInsights.config` SDK oluşturulurken `TelemetryConfiguration` çalışma dizininde dosya arar

```csharp
TelemetryConfiguration config = TelemetryConfiguration.Active; // Reads ApplicationInsights.config file if present
```

Config dosyasına giden yolu da belirtebilirsiniz.

```csharp
using System.IO;
TelemetryConfiguration configuration = TelemetryConfiguration.CreateFromConfiguration(File.ReadAllText("C:\\ApplicationInsights.config"));
var telemetryClient = new TelemetryClient(configuration);
```

Daha fazla bilgi için [yapılandırma dosyası başvurusuna](configuration-with-applicationinsights-config.md)bakın.

[Microsoft.ApplicationInsights.WindowsServer](https://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer) paketinin en son sürümünü yükleyerek config dosyasının tam bir örneğini alabilirsiniz. Aşağıda, kod örneğine eşdeğer bağımlılık koleksiyonu için **en az** yapılandırma verilmiştir.

```XML
<?xml version="1.0" encoding="utf-8"?>
<ApplicationInsights xmlns="http://schemas.microsoft.com/ApplicationInsights/2013/Settings">
  <InstrumentationKey>Your Key</InstrumentationKey>
  <TelemetryInitializers>
    <Add Type="Microsoft.ApplicationInsights.DependencyCollector.HttpDependenciesParsingTelemetryInitializer, Microsoft.AI.DependencyCollector"/>
  </TelemetryInitializers>
  <TelemetryModules>
    <Add Type="Microsoft.ApplicationInsights.DependencyCollector.DependencyTrackingTelemetryModule, Microsoft.AI.DependencyCollector">
      <ExcludeComponentCorrelationHttpHeadersOnDomains>
        <Add>core.windows.net</Add>
        <Add>core.chinacloudapi.cn</Add>
        <Add>core.cloudapi.de</Add>
        <Add>core.usgovcloudapi.net</Add>
        <Add>localhost</Add>
        <Add>127.0.0.1</Add>
      </ExcludeComponentCorrelationHttpHeadersOnDomains>
      <IncludeDiagnosticSourceActivities>
        <Add>Microsoft.Azure.ServiceBus</Add>
        <Add>Microsoft.Azure.EventHubs</Add>
      </IncludeDiagnosticSourceActivities>
    </Add>
  </TelemetryModules>
  <TelemetryChannel Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.ServerTelemetryChannel, Microsoft.AI.ServerTelemetryChannel"/>
</ApplicationInsights>

```

### <a name="configuring-telemetry-collection-from-code"></a>Koddan telemetri koleksiyonunu yapılandırma
> [!NOTE]
> Config dosyasının okunması .NET Core'da desteklenmez. ASP.NET Core [için Application Insights SDK'yı](../../azure-monitor/app/asp-net-core.md) kullanmayı düşünebilirsiniz

* Uygulama başlatma sırasında `DependencyTrackingTelemetryModule` örnek oluşturma ve yapılandırma - tekton olmalı ve uygulama ömrü için korunmalıdır.

```csharp
var module = new DependencyTrackingTelemetryModule();

// prevent Correlation Id to be sent to certain endpoints. You may add other domains as needed.
module.ExcludeComponentCorrelationHttpHeadersOnDomains.Add("core.windows.net");
//...

// enable known dependency tracking, note that in future versions, we will extend this list. 
// please check default settings in https://github.com/Microsoft/ApplicationInsights-dotnet-server/blob/develop/Src/DependencyCollector/DependencyCollector/ApplicationInsights.config.install.xdt

module.IncludeDiagnosticSourceActivities.Add("Microsoft.Azure.ServiceBus");
module.IncludeDiagnosticSourceActivities.Add("Microsoft.Azure.EventHubs");
//....

// initialize the module
module.Initialize(configuration);
```

* Ortak telemetri başharfleri ekleyin

```csharp
// ensures proper DependencyTelemetry.Type is set for Azure RESTful API calls
configuration.TelemetryInitializers.Add(new HttpDependenciesParsingTelemetryInitializer());
```

Düz `TelemetryConfiguration()` oluşturucu ile yapılandırma oluşturduysanız, korelasyon desteğini ayrıca etkinleştirmeniz gerekir. **It is not needed** Dosyadan yapılandırmayı okuyorsanız, kullanılırsanız `TelemetryConfiguration.CreateDefault()` `TelemetryConfiguration.Active`veya .

```csharp
configuration.TelemetryInitializers.Add(new OperationCorrelationTelemetryInitializer());
```

* [Ayrıca, burada](https://apmtips.com/blog/2017/02/13/enable-application-insights-live-metrics-from-code/) açıklandığı gibi Performans Sayacı kolektör modüllerini yüklemek ve başlatmak isteyebilirsiniz


#### <a name="full-example"></a>Tam örnek

```csharp
using Microsoft.ApplicationInsights;
using Microsoft.ApplicationInsights.DependencyCollector;
using Microsoft.ApplicationInsights.Extensibility;
using System.Net.Http;
using System.Threading.Tasks;

namespace ConsoleApp
{
    class Program
    {
        static void Main(string[] args)
        {
            TelemetryConfiguration configuration = TelemetryConfiguration.CreateDefault();

            configuration.InstrumentationKey = "removed";
            configuration.TelemetryInitializers.Add(new HttpDependenciesParsingTelemetryInitializer());

            var telemetryClient = new TelemetryClient(configuration);
            using (InitializeDependencyTracking(configuration))
            {
                // run app...

                telemetryClient.TrackTrace("Hello World!");

                using (var httpClient = new HttpClient())
                {
                    // Http dependency is automatically tracked!
                    httpClient.GetAsync("https://microsoft.com").Wait();
                }

            }

            // before exit, flush the remaining data
            telemetryClient.Flush();

            // flush is not blocking so wait a bit
            Task.Delay(5000).Wait();

        }

        static DependencyTrackingTelemetryModule InitializeDependencyTracking(TelemetryConfiguration configuration)
        {
            var module = new DependencyTrackingTelemetryModule();

            // prevent Correlation Id to be sent to certain endpoints. You may add other domains as needed.
            module.ExcludeComponentCorrelationHttpHeadersOnDomains.Add("core.windows.net");
            module.ExcludeComponentCorrelationHttpHeadersOnDomains.Add("core.chinacloudapi.cn");
            module.ExcludeComponentCorrelationHttpHeadersOnDomains.Add("core.cloudapi.de");
            module.ExcludeComponentCorrelationHttpHeadersOnDomains.Add("core.usgovcloudapi.net");
            module.ExcludeComponentCorrelationHttpHeadersOnDomains.Add("localhost");
            module.ExcludeComponentCorrelationHttpHeadersOnDomains.Add("127.0.0.1");

            // enable known dependency tracking, note that in future versions, we will extend this list. 
            // please check default settings in https://github.com/microsoft/ApplicationInsights-dotnet-server/blob/develop/WEB/Src/DependencyCollector/DependencyCollector/ApplicationInsights.config.install.xdt

            module.IncludeDiagnosticSourceActivities.Add("Microsoft.Azure.ServiceBus");
            module.IncludeDiagnosticSourceActivities.Add("Microsoft.Azure.EventHubs");

            // initialize the module
            module.Initialize(configuration);

            return module;
        }
    }
}

```

## <a name="next-steps"></a>Sonraki adımlar
* REST, SQL veya diğer dış kaynakların sizi yavaşlatıp yavaşlatıp yavaşlatmamasını görmek için [bağımlılıkları izleyin.](../../azure-monitor/app/asp-net-dependencies.md)
* Uygulamanızın performansı ve kullanımıhakkında daha ayrıntılı bir görünüm elde etmek için kendi etkinliklerinizi ve ölçümlerinizi göndermek için [API'yi kullanın.](../../azure-monitor/app/api-custom-events-metrics.md)
