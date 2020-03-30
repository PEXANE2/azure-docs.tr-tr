---
title: Application Insights'ta .NET izleme günlüklerini keşfedin
description: Trace, NLog veya Log4Net tarafından oluşturulan arama günlükleri.
ms.topic: conceptual
ms.date: 05/08/2019
ms.openlocfilehash: 273d5a2f4e1155541e159332312bdaa68aa175d7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79276276"
---
# <a name="explore-netnet-core-and-python-trace-logs-in-application-insights"></a>.NET/.NET Core ve Python izleme günlüklerini Application Insights'da keşfedin

ASP.NET/ASP.NET Core uygulamanız için iLogger, NLog, log4Net veya System.Diagnostics.Trace'den [Azure Uygulama Öngörülerine][start]tanılama izleme günlükleri gönderin. Python uygulamaları için, Azure Monitor için OpenCensus Python'da AzureLogHandler kullanarak tanılama izleme günlükleri gönderin. Daha sonra onları keşfedebilir ve arayabilirsiniz. Bu günlükler uygulamanızdaki diğer günlük dosyalarıyla birleştirilir, böylece her kullanıcı isteğiyle ilişkili izleri belirleyebilir ve bunları diğer olaylar ve özel durum raporlarıyla ilişkilendirebilirsiniz.

> [!NOTE]
> Günlük yakalama modülüne mi ihtiyacınız var? Üçüncü taraf loggers için yararlı bir bağdaştırıcı. Ancak NLog, log4Net veya System.Diagnostics.Trace kullanmıyorsanız, doğrudan [**Application Insights TrackTrace()**](../../azure-monitor/app/api-custom-events-metrics.md#tracktrace) aramayı düşünün.
>
>
## <a name="install-logging-on-your-app"></a>Uygulamanızda günlük yükleme
Seçtiğiniz günlük çerçevesini projenizde yükleyin, bu da app.config veya web.config'e girişle sonuçlanmalıdır.

```XML
 <configuration>
  <system.diagnostics>
    <trace>
      <listeners>
        <add name="myAppInsightsListener" type="Microsoft.ApplicationInsights.TraceListener.ApplicationInsightsTraceListener, Microsoft.ApplicationInsights.TraceListener" />
      </listeners>
    </trace>
  </system.diagnostics>
</configuration>
```

## <a name="configure-application-insights-to-collect-logs"></a>Günlükleri toplamak için Uygulama Öngörülerini Yapılandır
Henüz [yapmadıysanız, projenize Uygulama Öngörüleri ekleyin.](../../azure-monitor/app/asp-net.md) Günlük toplayıcısını ekleme seçeneği görürsünüz.

Veya **Uygulama Öngörülerini Yapılandırmak**için Solution Explorer'da projenizi sağ tıklatın. İzleme **koleksiyonunu yapılandır** seçeneğini seçin.

> [!NOTE]
> Application Insights menüsü veya günlük toplayıcı seçeneği yok mu? [Sorun Giderme'yi](#troubleshooting)deneyin.

## <a name="manual-installation"></a>El ile yükleme
Proje türünüz Application Insights yükleyicisi (örneğin bir Windows masaüstü projesi) tarafından desteklenmiyorsa bu yöntemi kullanın.

1. log4Net veya NLog kullanmayı planlıyorsanız, projenize yükleyin.
2. Çözüm Gezgini'nde projenizi sağ tıklatın ve **NuGet Paketlerini Yönet'i**seçin.
3. "Uygulama Öngörüleri" araması yapın.
4. Aşağıdaki paketlerden birini seçin:

   - ILogger için: [Microsoft.Extensions.Logging.ApplicationInsights](https://www.nuget.org/packages/Microsoft.Extensions.Logging.ApplicationInsights/)
[![NuGet](https://img.shields.io/nuget/vpre/Microsoft.Extensions.Logging.ApplicationInsights.svg)](https://www.nuget.org/packages/Microsoft.Extensions.Logging.ApplicationInsights/)
   - NLog için: [Microsoft.ApplicationInsights.NLogTarget](https://www.nuget.org/packages/Microsoft.ApplicationInsights.NLogTarget/)
[![NuGet](https://img.shields.io/nuget/vpre/Microsoft.ApplicationInsights.NLogTarget.svg)](https://www.nuget.org/packages/Microsoft.ApplicationInsights.NLogTarget/)
   - Log4Net için: [Microsoft.ApplicationInsights.Log4NetAppender](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Log4NetAppender/)
[![NuGet](https://img.shields.io/nuget/vpre/Microsoft.ApplicationInsights.Log4NetAppender.svg)](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Log4NetAppender/)
   - System.Diagnostics için: [Microsoft.ApplicationInsights.TraceListener](https://www.nuget.org/packages/Microsoft.ApplicationInsights.TraceListener/)
[![NuGet](https://img.shields.io/nuget/vpre/Microsoft.ApplicationInsights.TraceListener.svg)](https://www.nuget.org/packages/Microsoft.ApplicationInsights.TraceListener/)
   - [Microsoft.ApplicationInsights.DiagnosticSourceListener](https://www.nuget.org/packages/Microsoft.ApplicationInsights.DiagnosticSourceListener/)
[![NuGet](https://img.shields.io/nuget/vpre/Microsoft.ApplicationInsights.DiagnosticSourceListener.svg)](https://www.nuget.org/packages/Microsoft.ApplicationInsights.DiagnosticSourceListener/)
   - [Microsoft.ApplicationInsights.EtwCollector](https://www.nuget.org/packages/Microsoft.ApplicationInsights.EtwCollector/)
[![NuGet](https://img.shields.io/nuget/vpre/Microsoft.ApplicationInsights.EtwCollector.svg)](https://www.nuget.org/packages/Microsoft.ApplicationInsights.EtwCollector/)
   - [Microsoft.ApplicationInsights.EventSourceListener](https://www.nuget.org/packages/Microsoft.ApplicationInsights.EventSourceListener/)
[![Nuget](https://img.shields.io/nuget/vpre/Microsoft.ApplicationInsights.EventSourceListener.svg)](https://www.nuget.org/packages/Microsoft.ApplicationInsights.EventSourceListener/)

NuGet paketi gerekli derlemeleri yükler ve varsa web.config veya app.config'i değiştirir.

## <a name="ilogger"></a>ILogger

Konsol uygulamaları ve ASP.NET Core ile Uygulama Öngörüleri ILogger uygulamasını kullanma örnekleri [için ,.NET Core ILogger günlükleri için ApplicationInsightsLoggerProvider](ilogger.md)bölümüne bakın.

## <a name="insert-diagnostic-log-calls"></a>Tanılama günlüğü çağrıları ekleme
System.Diagnostics.Trace kullanıyorsanız, tipik bir arama olacaktır:

    System.Diagnostics.Trace.TraceWarning("Slow response - database01");

log4net veya NLog'u tercih ederseniz, şunları kullanın:

    logger.Warn("Slow response - database01");

## <a name="use-eventsource-events"></a>EventSource olaylarını kullanma
[System.Diagnostics.Tracing.EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) olaylarını traceolarak Application Insights'a gönderilecek şekilde yapılandırabilirsiniz. İlk olarak, `Microsoft.ApplicationInsights.EventSourceListener` NuGet paketini yükleyin. Ardından `TelemetryModules` [ApplicationInsights.config](../../azure-monitor/app/configuration-with-applicationinsights-config.md) dosyasının bölümünü edin.

```xml
    <Add Type="Microsoft.ApplicationInsights.EventSourceListener.EventSourceTelemetryModule, Microsoft.ApplicationInsights.EventSourceListener">
      <Sources>
        <Add Name="MyCompany" Level="Verbose" />
      </Sources>
    </Add>
```

Her kaynak için aşağıdaki parametreleri ayarlayabilirsiniz:
 * **Ad,** toplanacak EventSource'un adını belirtir.
 * **Düzey** toplamak için günlük düzeyini belirtir: *Kritik*, *Hata*, *Bilgilendirme*, *LogAlways*, *Verbose*, veya *Uyarı*.
 * **Anahtar kelimeler** (isteğe bağlı) kullanılacak anahtar kelime birleşimlerinin gerçek değerini belirtir.

## <a name="use-diagnosticsource-events"></a>DiagnosticSource olaylarını kullanma
[System.Diagnostics.DiagnosticSource](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md) olaylarını traces olarak Uygulama Öngörüleri'ne gönderilecek şekilde yapılandırabilirsiniz. İlk olarak, [`Microsoft.ApplicationInsights.DiagnosticSourceListener`](https://www.nuget.org/packages/Microsoft.ApplicationInsights.DiagnosticSourceListener) NuGet paketini yükleyin. Daha sonra [ApplicationInsights.config](../../azure-monitor/app/configuration-with-applicationinsights-config.md) dosyasının "Telemetri Modülleri" bölümünü değiştirin.

```xml
    <Add Type="Microsoft.ApplicationInsights.DiagnosticSourceListener.DiagnosticSourceTelemetryModule, Microsoft.ApplicationInsights.DiagnosticSourceListener">
      <Sources>
        <Add Name="MyDiagnosticSourceName" />
      </Sources>
    </Add>
```

İzlemek istediğiniz her DiagnosticSource için, DiagnosticSource'unuzun adına **Ad** özniteliği ayarlanmış bir giriş ekleyin.

## <a name="use-etw-events"></a>ETW olaylarını kullanma
Windows için Olay İzleme (ETW) olaylarının Uygulama Öngörüleri'ne iz olarak gönderilmesini yapılandırabilirsiniz. İlk olarak, `Microsoft.ApplicationInsights.EtwCollector` NuGet paketini yükleyin. Daha sonra [ApplicationInsights.config](../../azure-monitor/app/configuration-with-applicationinsights-config.md) dosyasının "Telemetri Modülleri" bölümünü değiştirin.

> [!NOTE] 
> ETW olayları yalnızca SDK'yı barındıran işlem Performans Günlüğü Kullanıcıları veya Yöneticilerinin üyesi bir kimlik altında çalıştığında toplanabilir.

```xml
    <Add Type="Microsoft.ApplicationInsights.EtwCollector.EtwCollectorTelemetryModule, Microsoft.ApplicationInsights.EtwCollector">
      <Sources>
        <Add ProviderName="MyCompanyEventSourceName" Level="Verbose" />
      </Sources>
    </Add>
```

Her kaynak için aşağıdaki parametreleri ayarlayabilirsiniz:
 * **ProviderName,** toplamak için ETW sağlayıcısının adıdır.
 * **ProviderGuid,** toplamak için ETW sağlayıcısının GUID'ini belirtir. Bunun yerine `ProviderName`kullanılabilir.
 * **Düzey** toplamak için günlük düzeyini ayarlar. Bu *Kritik*olabilir , *Hata*, *Bilgilendirme*, *LogAlways*, *Verbose*, veya *Uyarı*.
 * **Anahtar kelimeler** (isteğe bağlı) kullanılacak anahtar kelime kombinasyonlarının gerçek değerini ayarlar.

## <a name="use-the-trace-api-directly"></a>İzleme API'sini doğrudan kullanma
Uygulama Öngörüleri izleme API'sini doğrudan arayabilirsiniz. Günlüğe kaydeden bağdaştırıcılar bu API'yi kullanır.

Örnek:

    var telemetry = new Microsoft.ApplicationInsights.TelemetryClient();
    telemetry.TrackTrace("Slow response - database01");

TrackTrace'in bir avantajı, iletiye nispeten uzun veriler koyabiliyor olmasıdır. Örneğin, POST verilerini burada kodlayabilirsiniz.

İletinize önem düzeyi de ekleyebilirsiniz. Ve diğer telemetriler gibi, farklı izleme kümelerini filtrelemeye veya arama yapmaya yardımcı olmak için özellik değerleri ekleyebilirsiniz. Örnek:

    var telemetry = new Microsoft.ApplicationInsights.TelemetryClient();
    telemetry.TrackTrace("Slow database response",
                   SeverityLevel.Warning,
                   new Dictionary<string,string> { {"database", db.ID} });

Bu, [Arama'da][diagnostic] belirli bir veritabanıyla ilgili belirli bir önem düzeyidüzeyinin tüm iletilerini kolayca filtrelemenizi sağlar.

## <a name="azureloghandler-for-opencensus-python"></a>OpenCensus Python için AzureLogHandler
Azure Monitör Günlük İşleyicisi, Python günlüklerini Azure Monitor'a dışa aktarmanızı sağlar.

Azure Monitor için [OpenCensus Python SDK](../../azure-monitor/app/opencensus-python.md) ile uygulamanızı enstrüman.

Bu örnek, Azure Monitor'a bir uyarı düzeyi günlüğünasıl gönderilenleri gösterir.

```python
import logging

from opencensus.ext.azure.log_exporter import AzureLogHandler

logger = logging.getLogger(__name__)
logger.addHandler(AzureLogHandler(connection_string='InstrumentationKey=<your-instrumentation_key-here>'))
logger.warning('Hello, World!')
```

## <a name="explore-your-logs"></a>Günlüklerinizi keşfedin
Uygulamanızı hata ayıklama modunda çalıştırın veya canlı olarak dağıtın.

[Uygulama Öngörüleri portalındaki][portal]uygulamanızın genel bakış bölmesinde [Ara'yı][diagnostic]seçin.

Örneğin şunları yapabilirsiniz:

* Günlük izlemelerine veya belirli özelliklere sahip öğelere filtre uygulayın.
* Belirli bir öğeyi ayrıntılı olarak inceleyin.
* Aynı kullanıcı isteğiyle ilgili diğer sistem günlüğü verilerini bulun (aynı OperationId'ye sahiptir).
* Bir sayfanın yapılandırmasını sık kullanılan olarak kaydedin.

> [!NOTE]
>Uygulamanız çok fazla veri gönderiyorsa ve 2.0.0-beta3 veya sonraki ASP.NET sürümiçin Application Insights SDK'yı kullanıyorsanız, *uyarlanabilir örnekleme* özelliği çalışabilir ve telemetrinizin yalnızca bir kısmını gönderebilir. [Örnekleme hakkında daha fazla bilgi edinin.](../../azure-monitor/app/sampling.md)
>

## <a name="troubleshooting"></a>Sorun giderme
### <a name="how-do-i-do-this-for-java"></a>Bunu Java için nasıl yaparım?
Java [günlük bağdaştırıcılarını](../../azure-monitor/app/java-trace-logs.md)kullanın.

### <a name="theres-no-application-insights-option-on-the-project-context-menu"></a>Proje bağlamı menüsünde Uygulama Öngörüleri seçeneği yok
* Geliştirici Analitik Araçları'nın geliştirme makinesine yüklü olduğundan emin olun. Visual Studio **Tools** > **Uzantıları ve Güncellemeleri,** **Geliştirici Analitik Araçları**arayın. **Yüklü** sekmesinde değilse, **Çevrimiçi** sekmesini açın ve yükleyin.
* Bu, Devloper Analytics Tools'un desteklemediği bir proje türü olabilir. [Manuel yükleme yi](#manual-installation)kullanın.

### <a name="theres-no-log-adapter-option-in-the-configuration-tool"></a>Yapılandırma aracında günlük bağdaştırıcısı seçeneği yok
* Önce günlük çerçevesini yükleyin.
* System.Diagnostics.Trace kullanıyorsanız, [ *web.config'de*yapılandırdığınızdan](https://msdn.microsoft.com/library/system.diagnostics.eventlogtracelistener.aspx)emin olun.
* Application Insights'ın en son sürümüne sahip olduğundan emin olun. Visual Studio'da **Araçlar** > **Uzantıları ve Güncelleştirmeleri'ne**gidin ve **Güncelleştirmeler** sekmesini açın. **Geliştirici Analitik Araçları** varsa, güncellemek için seçin.

### <a name="i-get-the-instrumentation-key-cannot-be-empty-error-message"></a><a name="emptykey"></a>"Enstrümantasyon tuşu boş olamaz" hata iletisini alıyorum
Uygulama Bilgileri'ni yüklemeden günlük bağdaştırıcısı Nuget paketini yüklemiş olabilirsiniz. Solution Explorer'da *ApplicationInsights.config'e*sağ tıklayın ve **Uygulama Öngörülerini Güncelleştir'i**seçin. Azure'da oturum açmanız ve bir Application Insights kaynağı oluşturmanız veya varolan bir kaynağı yeniden kullanmanız istenir. Bu sorunu çözer.

### <a name="i-can-see-traces-but-not-other-events-in-diagnostic-search"></a>İzleri görebiliyorum ama tanıaramadaki diğer olayları göremiyorum
Tüm olaylar ve istekler için boru hattı üzerinden almak için bir süre alabilir.

### <a name="how-much-data-is-retained"></a><a name="limits"></a>Ne kadar veri tutulur?
Çeşitli etkenler, tutulan veri miktarını etkiler. Daha fazla bilgi için, müşteri etkinlik ölçümleri sayfasının [sınırlar](../../azure-monitor/app/api-custom-events-metrics.md#limits) bölümüne bakın.

### <a name="i-dont-see-some-log-entries-that-i-expected"></a>Beklediğim bazı günlük girişlerini görmüyorum
Uygulamanız hacimli miktarda veri gönderiyorsa ve 2.0.0-beta3 veya sonraki sürümASP.NET için Application Insights SDK'yı kullanıyorsanız, uyarlanabilir örnekleme özelliği çalışabilir ve telemetrinizin yalnızca bir kısmını gönderebilir. [Örnekleme hakkında daha fazla bilgi edinin.](../../azure-monitor/app/sampling.md)

## <a name="next-steps"></a><a name="add"></a>Sonraki adımlar

* [ASP.NET hataları ve özel durumları tanılamak][exceptions]
* [Arama hakkında daha fazla bilgi edinin][diagnostic]
* [Kullanılabilirlik ve yanıt verme testlerini ayarlama][availability]
* [Sorun giderme][qna]

<!--Link references-->

[availability]: ../../azure-monitor/app/monitor-web-app-availability.md
[diagnostic]: ../../azure-monitor/app/diagnostic-search.md
[exceptions]: asp-net-exceptions.md
[portal]: https://portal.azure.com/
[qna]: ../../azure-monitor/app/troubleshoot-faq.md
[start]: ../../azure-monitor/app/app-insights-overview.md
