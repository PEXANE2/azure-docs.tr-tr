---
title: Canlı Ölçümler Akışı yla Tanılama - Azure Uygulama Öngörüleri
description: Web uygulamanızı özel ölçümlerle gerçek zamanlı olarak izleyin ve sorunları hatalar, izlemeler ve olaylarla dolu canlı bir yayınla tanıtın.
ms.topic: conceptual
ms.date: 04/22/2019
ms.reviewer: sdash
ms.openlocfilehash: ea0d786d0b8b96941d791bcc8e92fad9a869c5f3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77670109"
---
# <a name="live-metrics-stream-monitor--diagnose-with-1-second-latency"></a>Canlı Ölçümler Akışı: 1 saniyegecikmeyle izleme & Tanılama

[Application Insights'tan](../../azure-monitor/app/app-insights-overview.md)Canlı Ölçümler Akışı'nı kullanarak canlı, üretim içi web uygulamanızın atan kalbini incen. Hizmetinizi rahatsız etmeden gerçek zamanlı olarak izlemek için ölçümleri ve performans sayaçlarını seçin ve filtreleyin. Örnek başarısız isteklerden ve özel durumlardan yığın izlerini inceleyin. [Profiler](../../azure-monitor/app/profiler.md)ile birlikte , [Anlık görüntü hata ayıklayıcı](../../azure-monitor/app/snapshot-debugger.md). Canlı Ölçümler Akışı, canlı web siteniz için güçlü ve non-invaziv bir tanı aracı sağlar.

Canlı Ölçümler Akışı ile şunları yapabilirsiniz:

* Performans ve hata sayılarını izleyerek bir düzeltmeyi serbest bırakılırken doğrulayın.
* Test yüklerinin etkisini izleyin ve sorunları canlı tanıkoyun.
* İzlemek istediğiniz ölçümleri seçip filtreleyerek belirli test oturumlarına odaklanın veya bilinen sorunları filtreleyin.
* Özel durum izlerini olduğu gibi alın.
* En alakalı KP'leri bulmak için filtrelerle denemeler uygulayın.
* Herhangi bir Windows performans sayacını canlı izleyin.
* Sorun yaşayan bir sunucuyu kolayca tanımlayın ve tüm KPI/live akışını sadece o sunucuya filtreleyin.

[![Canlı Ölçümler Akış videosu](./media/live-stream/youtube.png)](https://www.youtube.com/watch?v=zqfHf1Oi5PY)

Canlı Ölçümler şu anda ASP.NET, ASP.NET Core, Azure İşlevler, Java ve Node.js uygulamaları için desteklenir.

## <a name="get-started"></a>Kullanmaya başlayın

1. [Uygulama Öngörüleri'ni](../../azure-monitor/azure-monitor-app-hub.yml) web uygulamanıza henüz yüklemediyseniz, bunu şimdi yapın.
2. Standart Application Insights paketlerine ek olarak [Microsoft.ApplicationInsights.PerfCounterCollector](https://www.nuget.org/packages/Microsoft.ApplicationInsights.PerfCounterCollector/) Canlı Ölçümler akışını etkinleştirmek için gereklidir.
3. Application Insights **paketinin en son sürümüne güncelleştirin.** Visual Studio'da projenize sağ tıklayın ve **Nuget paketlerini yönet'i**seçin. **Güncelleştirmeler** sekmesini açın ve tüm Microsoft.ApplicationInsights.* paketlerini seçin.

    Uygulamanızı yeniden dağıtın.

3. Azure [portalında](https://portal.azure.com)uygulamanız için Application Insights kaynağını açın ve ardından Live Stream'i açın.

4. Filtrelerinizde müşteri adları gibi hassas verileri kullanabiliyorsanız [denetim kanalını güvenli hale verin.](#secure-the-control-channel)

### <a name="no-data-check-your-server-firewall"></a>Veri yok mu? Sunucu güvenlik duvarınızı kontrol edin

Sunucularınızın güvenlik duvarında [Live Metrics Stream için giden bağlantı noktalarının](../../azure-monitor/app/ip-addresses.md#outgoing-ports) açık olup olduğunu kontrol edin.

## <a name="how-does-live-metrics-stream-differ-from-metrics-explorer-and-analytics"></a>Canlı Ölçümler Akışı'nın Metrics Explorer ve Analytics'ten farkı nedir?

| |Canlı Akış | Ölçümler Explorer ve Analitik |
|---|---|---|
|Gecikme süresi|Bir saniye içinde görüntülenen veriler|Dakikalar içinde toplanır|
|Bekletme yok|Veriler grafikteyken devam eder ve sonra atılır|[Veriler 90 gün boyunca saklanır](../../azure-monitor/app/data-retention-privacy.md#how-long-is-the-data-kept)|
|İsteğe bağlı|Canlı Ölçümleri açarken veriler akışlanır|SDK yüklendiğinde ve etkinleştirildiğinde veriler gönderilir|
|Ücretsiz|Canlı Akış verileri için ücret alınmaz|[Fiyatlandırmaya](../../azure-monitor/app/pricing.md) tabi
|Örnekleme|Seçilen tüm ölçümler ve sayaçlar iletilir. Hatalar ve yığın izleri örneklenir. Telemetriİşlemciler uygulanmaz.|Olaylar [örneklenebilir](../../azure-monitor/app/api-filtering-sampling.md)|
|Kontrol kanalı|Filtre kontrol sinyalleri SDK'ya gönderilir. Bu kanalı korumanızı öneririz.|İletişim tek yönlüdür, portala|

## <a name="select-and-filter-your-metrics"></a>Ölçümlerinizi seçin ve filtreleyin

(ASP.NET, ASP.NET Core ve Azure İşleriyle (v2))

Portaldan herhangi bir Uygulama Öngörüleri telemetrisi üzerinde rasgele filtreler uygulayarak özel KPI canlı izleyebilirsiniz. Grafiklerden herhangi birini ne zaman fare yle yendiğinizi gösteren filtre denetimini tıklatın. Aşağıdaki grafik, URL ve Süre özniteliklerindeki filtrelerle özel bir İstek sayısı KPI'sını çizer. Zaman içinde herhangi bir noktada belirttiğiniz ölçütlerle eşleşen canlı bir telemetri yayını gösteren Akış Önizleme bölümüyle filtrelerinizi doğrulayın.

![Özel İstek KPI](./media/live-stream/live-stream-filteredMetric.png)

Kont'tan farklı bir değeri izleyebilirsiniz. Seçenekler, herhangi bir Uygulama Öngörüleri telemetrisi olabilecek akış türüne bağlıdır: istekler, bağımlılıklar, özel durumlar, izlemeler, olaylar veya ölçümler. Bu kendi [özel ölçüm](../../azure-monitor/app/api-custom-events-metrics.md#properties)olabilir:

![Değer Seçenekleri](./media/live-stream/live-stream-valueoptions.png)

Application Insights telemetrisine ek olarak, akış seçeneklerinden bunu seçerek ve performans sayacının adını sağlayarak herhangi bir Windows performans sayacını da izleyebilirsiniz.

Canlı ölçümler iki noktada toplanır: her sunucuda yerel olarak ve daha sonra tüm sunucularda. İlgili açılır düşüşlerde diğer seçenekleri seçerek varsayılanı değiştirebilirsiniz.

## <a name="sample-telemetry-custom-live-diagnostic-events"></a>Örnek Telemetri: Özel Canlı Tanı etkinlikleri
Varsayılan olarak, olayların canlı akışı başarısız istekve bağımlılık çağrıları, özel durumlar, olaylar ve izleme örneklerini gösterir. Uygulanan ölçütleri zaman içinde herhangi bir noktada görmek için filtre simgesini tıklatın. 

![Varsayılan canlı besleme](./media/live-stream/live-stream-eventsdefault.png)

Ölçümlerde olduğu gibi, Uygulama Öngörüleri telemetri türlerinden herhangi biri için herhangi bir rasgele ölçüt belirtebilirsiniz. Bu örnekte, belirli istek hataları, izlemeler ve olaylar seçiyoruz. Ayrıca tüm özel durumları ve bağımlılık hatalarını da seçiyoruz.

![Özel canlı yayın](./media/live-stream/live-stream-events.png)

Not: Şu anda, Özel Durum iletisi tabanlı ölçütler için en dıştaki özel durum iletisini kullanın. Önceki örnekte, iç özel durum iletisi ile iyi huylu özel durum filtrelemek için ("<--" delimiter izler) "Istemci bağlantısı kesildi." "Hata okuma isteği içeriği" ölçütlerini içermeyen bir ileti kullanın.

Bir öğeyi tıklatarak canlı yayında bir öğenin ayrıntılarını görün. Yayını **Duraklat'ı** tıklatarak veya yalnızca aşağı kaydırarak veya bir öğeyi tıklatarak duraklatabilirsiniz. Canlı yayın, en üste geri kaydırıldıktan sonra veya duraklatılmışken toplanan öğelerin sayacına tıklayarak devam edecektir.

![Örneklenmiş canlı hatalar](./media/live-stream/live-metrics-eventdetail.png)

## <a name="filter-by-server-instance"></a>Sunucu örneğine göre filtreleme

Belirli bir sunucu rolü örneğini izlemek istiyorsanız, sunucuya göre filtre uygulayabilirsiniz.

![Örneklenmiş canlı hatalar](./media/live-stream/live-stream-filter.png)

## <a name="secure-the-control-channel"></a>Kontrol kanalını güvenli hale
Belirlediğiniz özel filtreler ölçütleri, Uygulama Öngörüleri SDK'daki Canlı Ölçümler bileşenine geri gönderilir. Filtreler, customerID'ler gibi hassas bilgiler içerebilir. Enstrümantasyon anahtarına ek olarak kanalı gizli bir API tuşu ile güvenli hale getirebilirsiniz.
### <a name="create-an-api-key"></a>API Anahtarı Oluşturma

![API anahtarı oluşturma](./media/live-stream/live-metrics-apikeycreate.png)

### <a name="add-api-key-to-configuration"></a>Yapılandırmaya API anahtarı ekleme

### <a name="classic-aspnet"></a>Klasik ASP.NET

applicationinsights.config dosyasında, AuthenticationApiKey'i QuickPulseTelemetryModule'e ekleyin:
``` XML

<Add Type="Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.QuickPulse.QuickPulseTelemetryModule, Microsoft.AI.PerfCounterCollector">
      <AuthenticationApiKey>YOUR-API-KEY-HERE</AuthenticationApiKey>
</Add>

```
Veya kod olarak, QuickPulseTelemetryModule ayarlayın:

```csharp
using Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.QuickPulse;
using Microsoft.ApplicationInsights.Extensibility;

             TelemetryConfiguration configuration = new TelemetryConfiguration();
            configuration.InstrumentationKey = "YOUR-IKEY-HERE";

            QuickPulseTelemetryProcessor processor = null;

            configuration.TelemetryProcessorChainBuilder
                .Use((next) =>
                {
                    processor = new QuickPulseTelemetryProcessor(next);
                    return processor;
                })
                        .Build();

            var QuickPulse = new QuickPulseTelemetryModule()
            {

                AuthenticationApiKey = "YOUR-API-KEY"
            };
            QuickPulse.Initialize(configuration);
            QuickPulse.RegisterTelemetryProcessor(processor);
            foreach (var telemetryProcessor in configuration.TelemetryProcessors)
                {
                if (telemetryProcessor is ITelemetryModule telemetryModule)
                    {
                    telemetryModule.Initialize(configuration);
                    }
                }

```

### <a name="azure-function-apps"></a>Azure İşlev Uygulamaları

Azure İşlev Uygulamaları (v2) için, kanalı bir API anahtarıyla güvence altına almak bir ortam değişkeni ile gerçekleştirilebilir.

Application Insights kaynağınızın içinden bir API anahtarı oluşturun ve İşlev Uygulamanız için **Uygulama Ayarları'na** gidin. **Yeni ayar ekle'yi** seçin `APPINSIGHTS_QUICKPULSEAUTHAPIKEY` ve API anahtarınıza karşılık gelen bir ad ve değer girin.

### <a name="aspnet-core-requires-application-insights-aspnet-core-sdk-230-or-greater"></a>ASP.NET Core (Core SDK 2.3.0 veya daha büyük ASP.NET Uygulama Öngörüleri Gerektirir)

startup.cs dosyanızı aşağıdaki gibi değiştirin:

İlk ekleme

```csharp
using Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.QuickPulse;
```

Sonra ConfigureServices yöntemi içinde ekleyin:

```csharp
services.ConfigureTelemetryModule<QuickPulseTelemetryModule> ((module, o) => module.AuthenticationApiKey = "YOUR-API-KEY-HERE");
```

Ancak, tüm bağlı sunucuları tanır ve güvenirseniz, kimliği doğrulanmış kanal olmadan özel filtreleri deneyebilirsiniz. Bu seçenek altı ay boyunca kullanılabilir. Bu geçersiz kılma, her yeni oturumda veya yeni bir sunucu çevrimiçi olduğunda gereklidir.

![Canlı Ölçümler Auth seçenekleri](./media/live-stream/live-stream-auth.png)

>[!NOTE]
>Filtre ölçütlerinde CustomerID gibi hassas olabilecek bilgileri girmeden önce kimlik doğrulaması yapılan kanalı ayarlamanızı öneririz.
>

## <a name="supported-features-table"></a>Desteklenen özellikler tablosu

| Dil                         | Temel Ölçümler       | Performans ölçümleri | Özel filtreleme    | Örnek telemetri    | CPU işlemine göre bölünür |
|----------------------------------|:--------------------|:--------------------|:--------------------|:--------------------|:---------------------|
| .NET                             | Desteklenen (V2.7.2+) | Desteklenen (V2.7.2+) | Desteklenen (V2.7.2+) | Desteklenen (V2.7.2+) | Desteklenen (V2.7.2+)  |
| .NET Çekirdek (hedef=.NET Framework)| Desteklenen (V2.4.1+) | Desteklenen (V2.4.1+) | Desteklenen (V2.4.1+) | Desteklenen (V2.4.1+) | Desteklenen (V2.4.1+)  |
| .NET Çekirdek (hedef=.NET Çekirdek)     | Desteklenen (V2.4.1+) | Destekleniyor*          | Desteklenen (V2.4.1+) | Desteklenen (V2.4.1+) | **Desteklenmiyor**    |
| Azure Fonksiyonları v2               | Destekleniyor           | Destekleniyor           | Destekleniyor           | Destekleniyor           | **Desteklenmiyor**    |
| Java                             | Desteklenen (V2.0.0+) | Desteklenen (V2.0.0+) | **Desteklenmiyor**   | **Desteklenmiyor**   | **Desteklenmiyor**    |
| Node.js                          | Desteklenen (V1.3.0+) | Desteklenen (V1.3.0+) | **Desteklenmiyor**   | Desteklenen (V1.3.0+) | **Desteklenmiyor**    |

Temel ölçümler istek, bağımlılık ve özel durum oranını içerir. Performans ölçümleri (performans sayaçları) bellek ve CPU içerir. Örnek telemetri, başarısız istekler ve bağımlılıklar, özel durumlar, olaylar ve izlemeler için ayrıntılı bilgi akışını gösterir.

 \*PerfCounters desteği .NET Core'un .NET Framework'ü hedeflemeyan sürümlerine göre biraz farklılık gösterir:

- PerfCounters ölçümleri, Windows için Azure Uygulama Hizmeti'nde çalışırken desteklenir. (AspNetCore SDK Sürüm 2.4.1 veya üzeri)
- PerfCounters, uygulama ANY Windows makinelerinde çalışırken desteklenir (VM veya Bulut Hizmeti veya On-prem vb.) (AspNetCore SDK Sürüm 2.7.1 veya üzeri), ancak .NET Core 2.0 veya daha yüksek hedefleyen uygulamalar için.
- PerfCounters, uygulama en son beta sürümünde ANYWHERE (Linux, Windows, Linux, konteynerler, vb. için uygulama hizmeti) çalıştırıldığında desteklenir ( AspNetCore SDK Sürüm 2.8.0-beta1 veya üzeri), ancak .NET Core 2.0 veya daha yüksek hedefleyen uygulamalar için.

Varsayılan olarak Canlı Ölçümler Düğüm.js SDK'da devre dışı bırakılır. Canlı Ölçümleri etkinleştirmek `setSendLiveMetrics(true)` için, SDK'yı ilk olarak [yapılandırma yöntemlerinize](https://github.com/Microsoft/ApplicationInsights-node.js#configuration) ekleyin.

## <a name="troubleshooting"></a>Sorun giderme

Veri yok mu? Uygulamanız korumalı bir ağdaysa: Live Metrics Stream, diğer Uygulama Öngörüleri telemetrisinden farklı IP adresleri kullanır. Bu [IP adreslerinin](../../azure-monitor/app/ip-addresses.md) güvenlik duvarınızda açık olduğundan emin olun.

## <a name="next-steps"></a>Sonraki adımlar
* [Uygulama Öngörüleri ile kullanımı izleme](../../azure-monitor/app/usage-overview.md)
* [Tanılama Arama'yı Kullanma](../../azure-monitor/app/diagnostic-search.md)
* [Profil Oluşturucu](../../azure-monitor/app/profiler.md)
* [Anlık görüntü hata ayıklayıcısı](../../azure-monitor/app/snapshot-debugger.md)
