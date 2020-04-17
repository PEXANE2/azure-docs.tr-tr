---
title: Nasıl yapabilirim ... azure uygulama istatistikleri | Microsoft Dokümanlar
description: Uygulama Öngörülerinde SSS.
ms.topic: conceptual
ms.date: 04/04/2017
ms.openlocfilehash: 8d4b1e79c48b14ed7dce756468e4c48d633c3f04
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81536871"
---
# <a name="how-do-i--in-application-insights"></a>Application Insights’ta nasıl ... yapabilirim?
## <a name="get-an-email-when-"></a>Bir e-posta alın zaman ...
### <a name="email-if-my-site-goes-down"></a>Sitem çökerse e-posta gönder
[Kullanılabilirlik web sınayı](../../azure-monitor/app/monitor-web-app-availability.md)ayarlayın.

### <a name="email-if-my-site-is-overloaded"></a>Sitem aşırı yüklüyse e-posta
Sunucu yanıt **süresinde**bir [uyarı](../../azure-monitor/app/alerts.md) ayarlayın. 1 ile 2 saniye arasında bir eşik çalışması gerekir.

![](./media/how-do-i/030-server.png)

Uygulamanız, hata kodlarını döndürerek de gerginlik belirtileri gösterebilir. **Başarısız isteklerde**bir uyarı ayarlama.

**Sunucu özel durumlarında**bir uyarı ayarlamak istiyorsanız, verileri görmek için [bazı ek kurulum](../../azure-monitor/app/asp-net-exceptions.md) yapmanız gerekebilir.

### <a name="email-on-exceptions"></a>Özel durumlar a.ş. hakkında e-posta
1. [Özel durum izlemeyi ayarlama](../../azure-monitor/app/asp-net-exceptions.md)
2. Özel Durum sayısı ölçümünde [bir uyarı ayarlama](../../azure-monitor/app/alerts.md)

### <a name="email-on-an-event-in-my-app"></a>Uygulamamdaki bir etkinliğe e-posta
Belirli bir olay gerçekleştiğinde bir e-posta almak istediğinizi varsayalım. Uygulama Öngörüleri bu tesisi doğrudan sağlamaz, ancak [bir metrik bir eşiği geçtiğinde bir uyarı gönderebilir.](../../azure-monitor/app/alerts.md)

Uyarılar özel olaylar olmasa da [özel ölçümlere](../../azure-monitor/app/api-custom-events-metrics.md#trackmetric)ayarlanabilir. Olay oluştuğunda bir metrik artırmak için bazı kod yazın:

    telemetry.TrackMetric("Alarm", 10);

veya:

    var measurements = new Dictionary<string,double>();
    measurements ["Alarm"] = 10;
    telemetry.TrackEvent("status", null, measurements);

Uyarıların iki durumu olduğundan, uyarının sona ermiş olduğunu düşündüğünüzde düşük bir değer göndermeniz gerekir:

    telemetry.TrackMetric("Alarm", 0.5);

Alarmınızı görmek için [metrik gezginde](../../azure-monitor/platform/metrics-charts.md) bir grafik oluşturun:

![](./media/how-do-i/010-alarm.png)

Şimdi metrik kısa bir süre için bir orta değerin üzerine çıktığında yangın için bir uyarı ayarlayın:

![](./media/how-do-i/020-threshold.png)

Ortalama süreyi minimuma ayarlayın.

Metrik eşiğin üstünde ve altında olduğunda e-postalar alırsınız.

Dikkat edilmesi gereken bazı noktalar:

* Bir uyarının iki durumu vardır ("uyarı" ve "sağlıklı"). Durum yalnızca bir metrik alındığı zaman değerlendirilir.
* Bir e-posta yalnızca durum değiştiğinde gönderilir. Bu nedenle hem yüksek hem de düşük değerli ölçümler göndermeniz gerekir.
* Uyarıyı değerlendirmek için, önceki dönemde alınan değerlerin ortalaması alınır. Bu, bir metrik her aldığında oluşur, böylece e-postalar belirlediğiniz döneme göre daha sık gönderilebilir.
* E-postalar hem "uyarı" hem de "sağlıklı" olarak gönderildiğinden, tek çekimli etkinliğinizi iki durum lu bir durum olarak yeniden düşünmeyi düşünebilirsiniz. Örneğin, "tamamlanan iş" olayı yerine, bir işin başında ve sonunda e-posta aldığınız bir "devam eden iş" koşuluna sahip olun.

### <a name="set-up-alerts-automatically"></a>Uyarıları otomatik olarak ayarlama
[Yeni uyarılar oluşturmak için PowerShell'i kullanın](../../azure-monitor/app/alerts.md#automation)

## <a name="use-powershell-to-manage-application-insights"></a>Uygulama Öngörülerini Yönetmek için PowerShell'i kullanın
* [Yeni kaynaklar oluşturma](https://docs.microsoft.com/azure/azure-monitor/app/create-new-resource#creating-a-resource-automatically)
* [Yeni uyarılar oluşturma](../../azure-monitor/app/alerts.md#automation)

## <a name="separate-telemetry-from-different-versions"></a>Farklı sürümlerden ayrı telemetri

* Bir uygulamada birden çok rol: Tek bir Application Insights kaynağını kullanın ve [cloud_Rolename](../../azure-monitor/app/app-map.md)filtre uygulayın.
* Geliştirme, test ve sürüm sürümlerini ayırma: Farklı Application Insights kaynaklarını kullanın. Enstrümantasyon tuşlarını web.config'den al. [Daha fazla bilgi edinin](../../azure-monitor/app/separate-resources.md)
* Yapı sürümlerini bildirme: Telemetri başlandırıcıkullanarak özellik ekleyin. [Daha fazlasını öğrenin](../../azure-monitor/app/separate-resources.md)

## <a name="monitor-backend-servers-and-desktop-apps"></a>Arka uç sunucularını ve masaüstü uygulamalarını izleyin
[Windows Server SDK modüllerini kullanın.](../../azure-monitor/app/windows-desktop.md)

## <a name="visualize-data"></a>Verileri görselleştirme
#### <a name="dashboard-with-metrics-from-multiple-apps"></a>Birden çok uygulamadan ölçümler içeren pano
* [Metrik](../../azure-monitor/platform/metrics-charts.md)Gezgin'de, grafiğinizi özelleştirin ve sık kullanılan olarak kaydedin. Azure panosuna sabitle.

#### <a name="dashboard-with-data-from-other-sources-and-application-insights"></a>Diğer kaynaklardan ve Uygulama Öngörülerinden gelen verilerle pano
* [Güç BI telemetri ihracat](../../azure-monitor/app/export-power-bi.md ).

Veya

* SharePoint web bölümlerinde veri görüntüleyen, panonuz olarak SharePoint'i kullanın. [SQL'e dışa aktarmak için sürekli dışa aktarma ve Akış Analizi'ni kullanın.](../../azure-monitor/app/code-sample-export-sql-stream-analytics.md)  Veritabanını incelemek ve PowerView için bir SharePoint web bölümü oluşturmak için PowerView'ı kullanın.

<a name="search-specific-users"></a>

### <a name="filter-out-anonymous-or-authenticated-users"></a>Anonim veya kimlik doğrulaması kullanıcılarını filtreleyin
Kullanıcılarınız oturum [açsa, kimlik doğrulaması yapılan kullanıcı kimliğini](../../azure-monitor/app/api-custom-events-metrics.md#authenticated-users)ayarlayabilirsiniz. (Bu otomatik olarak gerçekleşmez.)

Ardından şunları yapabilirsiniz:

* Belirli kullanıcı adlarında arama

![](./media/how-do-i/110-search.png)

* Anonim veya kimlik doğrulaması verilen kullanıcılara filtre ölçümleri

![](./media/how-do-i/115-metrics.png)

## <a name="modify-property-names-or-values"></a>Özellik adlarını veya değerlerini değiştirme
[Filtre](../../azure-monitor/app/api-filtering-sampling.md#filtering)oluşturun. Bu, uygulamanızdan Application Insights'a gönderilmeden önce telemetriyi değiştirmenize veya filtrelemenize olanak tanır.

## <a name="list-specific-users-and-their-usage"></a>Belirli kullanıcıları ve bunların kullanımını listele
Yalnızca [belirli kullanıcıları aramak](#search-specific-users)istiyorsanız, kimlik [doğrulaması yapılan kullanıcı kimliğini](../../azure-monitor/app/api-custom-events-metrics.md#authenticated-users)ayarlayabilirsiniz.

Hangi sayfalara baktıkları veya ne sıklıkta oturum açtıkları gibi verilere sahip kullanıcıların listesini istiyorsanız, iki seçeneğiniz vardır:

* [Kimlik doğrulaması kullanıcı kimliğini ayarlayın,](../../azure-monitor/app/api-custom-events-metrics.md#authenticated-users) [bir veritabanına dışa](../../azure-monitor/app/code-sample-export-sql-stream-analytics.md) aktarın ve kullanıcı verilerinizi analiz etmek için uygun araçları kullanın.
* Yalnızca az sayıda kullanıcınız varsa, ilgi alan verileri metrik değer veya olay adı olarak kullanarak ve kullanıcı kimliğini bir özellik olarak ayarlayarak özel olaylar veya ölçümler gönderin. Sayfa görünümlerini analiz etmek için standart JavaScript trackPageView çağrısını değiştirin. Sunucu tarafındaki telemetriyi analiz etmek için, kullanıcı kimliğini tüm sunucu telemetrilerine eklemek için bir telemetri başharfi kullanın. Daha sonra kullanıcı kimliğinde ölçümleri ve aramaları filtreleyebilir ve bölümlere ayırabilirsiniz.

## <a name="reduce-traffic-from-my-app-to-application-insights"></a>Uygulama mayından Uygulama Öngörüleri'ne olan trafiği azaltın
* [ApplicationInsights.config'de,](../../azure-monitor/app/configuration-with-applicationinsights-config.md)ihtiyacınız olmayan tüm modülleri devre dışı bırakma , bu tür performans sayacı toplayıcısı.
* SDK'da [Örnekleme ve filtreleme](../../azure-monitor/app/api-filtering-sampling.md) kullanın.
* Web sayfalarınızda, her sayfa görünümü için bildirilen Ajax çağrılarının sayısını sınırlayın. Komut dosyası snippet `instrumentationKey:...` sonra , `,maxAjaxCallsPerView:3` eklemek: (veya uygun bir sayı).
* [TrackMetric](../../azure-monitor/app/api-custom-events-metrics.md#trackmetric)kullanıyorsanız, sonucu göndermeden önce metrik değerlerin toplu toplarını hesaplayın. TrackMetric() bunu sağlayan bir aşırı yük var.

[Fiyatlandırma ve kotalar](../../azure-monitor/app/pricing.md)hakkında daha fazla bilgi edinin.

## <a name="disable-telemetry"></a>Devre dışı telemetri
Telemetrinin sunucudan toplanmasını ve iletimini **dinamik olarak durdurmak ve başlatmak** için:

### <a name="aspnet-classic-applications"></a>ASP.NET Klasik uygulamalar

```csharp
    using  Microsoft.ApplicationInsights.Extensibility;

    TelemetryConfiguration.Active.DisableTelemetry = true;
```

### <a name="other-applications"></a>Diğer uygulamalar
Konsolda veya ASP.NET `TelemetryConfiguration.Active` Core uygulamalarında singleton kullanılması önerilmez.
instance'ı `TelemetryConfiguration` kendiniz oluşturduysanız `true`- ayarlayın. `DisableTelemetry`

ASP.NET Core uygulamaları için `TelemetryConfiguration` ASP.NET [Core bağımlılık enjeksiyonu](/aspnet/core/fundamentals/dependency-injection/)kullanarak örnek erişebilirsiniz. [ASP.NET Core uygulamaları için ApplicationInsights](../../azure-monitor/app/asp-net-core.md) makalesinde daha fazla ayrıntı bulabilirsiniz.

## <a name="disable-selected-standard-collectors"></a>Seçili standart toplayıcıları devre dışı
Standart toplayıcıları devre dışı atabilirsiniz (örneğin, performans sayaçları, HTTP istekleri veya bağımlılıklar)

* **ASP.NET uygulamaları** - [ApplicationInsights.config'deki](../../azure-monitor/app/configuration-with-applicationinsights-config.md) ilgili satırları silin veya yorum yapın
* **ASP.NET Core uygulamaları** - [ApplicationInsights ASP.NET Core'da](../../azure-monitor/app/asp-net-core.md#configuring-or-removing-default-telemetrymodules) telemetri modülleri yapılandırma seçeneklerini takip edin

## <a name="view-system-performance-counters"></a>Sistem performans sayaçlarını görüntüleme
Ölçümler explorer'da gösterebileceğiniz ölçümler arasında bir dizi sistem performans sayacı vardır. Sunucular **adında** önceden tanımlanmış bir bıçak var ve bunlardan birkaçını görüntüler.

![Application Insights kaynağınızı açın ve Sunucular'ı tıklatın](./media/how-do-i/121-servers.png)

### <a name="if-you-see-no-performance-counter-data"></a>Performans sayacı verisi görmüyorsanız
* Kendi makinenizde veya VM'nizde **IIS sunucusu.** [Durum Monitörünü Yükleyin.](../../azure-monitor/app/monitor-performance-live-website-now.md)
* **Azure web sitesi** - henüz performans sayaçlarını destekliyoruz. Azure web sitesi denetim panelinin standart bir parçası olarak alabileceğiniz birkaç ölçüm vardır.
* **Unix sunucu** - [Yükle toplandı](../../azure-monitor/app/java-collectd.md)

### <a name="to-display-more-performance-counters"></a>Daha fazla performans sayacı görüntülemek için
* İlk olarak, [yeni bir grafik ekleyin](../../azure-monitor/platform/metrics-charts.md) ve sayaç sunduğumuz temel kümede olup olmadığını görmek.
* Değilse, [sayaç performans sayacı modülü tarafından toplanan kümeye ekleyin.](../../azure-monitor/app/performance-counters.md)
