---
title: Azure Uygulama Öngörüleri uygulama verilerini görüntüleyin | Microsoft Dokümanlar
description: Performans sorunlarını tanılamak ve Uygulama Öngörüleri ile izlendiğinde kullanıcıların uygulamanızla ne yaptığını anlamak için Application Insights Bağlayıcısı çözümünü kullanabilirsiniz.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 02/13/2019
ms.openlocfilehash: c143d8aa24d3479f4619ea2c220d4a0c593f9cb1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77665174"
---
# <a name="application-insights-connector-management-solution-deprecated"></a>Uygulama Öngörüleri Bağlayıcı yönetimi çözümü (Amortismana Uğradı)

![Uygulama Öngörüleri simgesi](./media/app-insights-connector/app-insights-connector-symbol.png)

>[!NOTE]
> Kaynaklar arası [sorguların](../../azure-monitor/log-query/cross-workspace-query.md)desteğiyle, Uygulama Öngörüleri Bağlayıcısı yönetimi çözümü artık gerekli değildir. Azure ticari bulutu için 15 Ocak 2019'da resmen devreden OMS portalı ile birlikte Azure Marketi'nden kaldırıldı. Azure ABD Devlet bulutu için 30 Mart 2019'da kullanımdan kaldırılacaktır.
>
>Mevcut bağlantılar 30 Haziran 2019'a kadar çalışmaya devam edecektir.  OMS portalı amortismanı ile, varolan bağlantıları yapılandırmanın ve portaldan kaldırmanın bir yolu yoktur. Mevcut bağlantıları kaldırmak için PowerShell'i kullanma yla ilgili bir komut dosyası için aşağıdaki [PowerShell ile konektörü kaldırma](#removing-the-connector-with-powershell) yabakın.
>
>Birden çok uygulama için Uygulama Öngörüleri günlük verilerini sorgulama kılavuzu için [bkz.](../log-query/unify-app-resource-data.md) OMS portalı amortismanı hakkında daha fazla bilgi için [Azure'a taşınan OMS portalına](../../azure-monitor/platform/oms-portal-transition.md)bakın.
>
> 

Applications Insights Bağlayıcısı çözümü, uygulama [istatistikleri](../../azure-monitor/app/app-insights-overview.md)ile izlendiğinde performans sorunlarını tanılamanıza ve kullanıcıların uygulamanızla ne yaptığını anlamanıza yardımcı olur. Geliştiricilerin Application Insights'ta gördükleri aynı uygulama telemetrisinin görünümleri Log Analytics'te mevcuttur. Ancak, Application Insights uygulamalarınızı Log Analytics ile entegre ettiğinizde, operasyon ve uygulama verilerinin tek bir yerde olmasıyla uygulamalarınızın görünürlüğü artar. Aynı görünümlere sahip olmak, uygulama geliştiricilerinizle işbirliği yapmaya yardımcı olur. Ortak görünümler, hem uygulama hem de platform sorunlarını algılama ve çözme süresini azaltmaya yardımcı olabilir.

Çözümü kullandığınızda şunları yapabilirsiniz:

- Farklı Azure aboneliklerinde olsalar bile tüm Application Insights uygulamalarınızı tek bir yerde görüntüleyin
- Altyapı verilerini uygulama verileriyle ilişkilendirme
- Günlük aramasında perspektiflerle uygulama verilerini görselleştirin
- Azure portalındaki Log Analytics verilerinden Application Insights uygulamanıza dönüş


[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="connected-sources"></a>Bağlı kaynaklar

Diğer Log Analytics çözümlerinin aksine, uygulamalar için aracılar tarafından veri toplanmaz. Çözüm tarafından kullanılan tüm veriler doğrudan Azure'dan gelir.

| Bağlı Kaynak | Destekleniyor | Açıklama |
| --- | --- | --- |
| [Windows aracıları](../../azure-monitor/platform/agent-windows.md) | Hayır | Çözüm, Windows aracılarından bilgi toplamaz. |
| [Linux aracıları](../../azure-monitor/learn/quick-collect-linux-computer.md) | Hayır | Çözüm, Linux ajanlarından bilgi toplamaz. |
| [SCOM yönetim grubu](../../azure-monitor/platform/om-agents.md) | Hayır | Çözüm, bağlı bir SCOM yönetim grubundaki aracılardan bilgi toplamaz. |
| [Azure depolama hesabı](collect-azure-metrics-logs.md) | Hayır | Çözüm, Azure depolamadan bilgi toplamaz. |

## <a name="prerequisites"></a>Ön koşullar

- Application Insights Bağlayıcı bilgilerine erişmek için bir Azure aboneliğiniz olması gerekir
- En az bir yapılandırılmış Uygulama Öngörüleri kaynağınız olmalıdır.
- Application Insights kaynağının sahibi veya katılımcısı olmalısınız.

## <a name="configuration"></a>Yapılandırma

1. Azure Web Apps Analytics çözümlerini [Azure pazar alanından](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.AppInsights?tab=Overview) veya [Çözüm Galerisi'nden Log Analytics Ekle çözümlerinde](../../azure-monitor/insights/solutions.md)açıklanan işlemi kullanarak etkinleştirin.
2. [Azure portalına](https://portal.azure.com)göz atın. Application Insights'ı açmak için **tüm hizmetleri** seçin. Ardından, Uygulama Öngörüleri'ni arayın. 
3. **Abonelikler**altında, Application Insights kaynaklarına sahip bir abonelik seçin ve ardından **Ad**altında bir veya daha fazla uygulama seçin.
4. **Kaydet**'e tıklayın.

Yaklaşık 30 dakika içinde veriler kullanılabilir hale gelir ve Application Insights döşemesi aşağıdaki resim gibi verilerle güncelleştirilir:

![Uygulama Öngörüleri döşemesi](./media/app-insights-connector/app-insights-tile.png)

Akılda tutulması gereken diğer noktalar:

- Application Insights uygulamalarını yalnızca bir Log Analytics çalışma alanına bağlayabilirsiniz.
- Temel veya [Kurumsal Uygulama Öngörüleri kaynaklarını](https://azure.microsoft.com/pricing/details/application-insights) yalnızca Log Analytics'e bağlayabilirsiniz. Ancak, Log Analytics'in Ücretsiz katmanını kullanabilirsiniz.

## <a name="management-packs"></a>Yönetim paketleri

Bu çözüm, bağlı yönetim gruplarına herhangi bir yönetim paketi yüklemez.

## <a name="use-the-solution"></a>Çözümü kullanın

Aşağıdaki bölümlerde, uygulamalarınızdaki verileri görüntülemek ve bunlarla etkileşimde kalmak için Application Insights panosunda gösterilen bıçakları nasıl kullanabileceğiniz açıklanmaktadır.

### <a name="view-application-insights-connector-information"></a>Uygulama Öngörüleri Bağlayıcı bilgilerini görüntüleyin

Aşağıdaki **bıçakları** görmek için Application Insights panosunu açmak için Application **Insights** döşemesini tıklatın.

![Application Insights panosu](./media/app-insights-connector/app-insights-dash01.png)

![Application Insights panosu](./media/app-insights-connector/app-insights-dash02.png)

Pano, tabloda gösterilen bıçakları içerir. Her dikey pencerede, dikey pencerenin belirtilen kapsam ve zaman aralığına yönelik ölçütleriyle eşleşen en fazla 10 öğe listelenir. Bıçağın altındaki tüm kayıtları **gör'ü** tıklattığınızda veya bıçak başlığını tıklattığınızda tüm kayıtları döndüren bir günlük araması çalıştırabilirsiniz.


| **Sütun** | **Açıklama** |
| --- | --- |
| Uygulamalar - Başvuru sayısı | Uygulama kaynaklarındaki uygulama sayısını gösterir. Ayrıca uygulama adlarını ve her biri için uygulama kayıtlarının sayısını listeler. Bir günlük aramasını çalıştırmak için numarayı tıklatın<code>ApplicationInsights &#124; summarize AggregatedValue = sum(SampledCount) by ApplicationName</code> <br><br>  Ana bilgisayar başına uygulama kayıtlarını, telemetri türüne göre kayıtları ve türüne göre tüm verileri (son güne göre) gösteren uygulama için günlük araması çalıştırmak için bir uygulama adını tıklatın. |
| Veri Hacmi – Veri gönderen ana bilgisayarlar | Veri gönderen bilgisayar ana bilgisayarlarının sayısını gösterir. Ayrıca, her ana bilgisayar için bilgisayar ana bilgisayarlarını ve kayıt sayısını listeler. Bir günlük aramasını çalıştırmak için numarayı tıklatın<code>ApplicationInsights &#124; summarize AggregatedValue = sum(SampledCount) by Host</code> <br><br> Ana bilgisayar başına uygulama kayıtlarını, telemetri türüne göre kayıtları ve türüne göre tüm verileri (son güne göre) gösteren ana bilgisayar için günlük aramasını çalıştırmak için bir bilgisayar adını tıklatın. |
| Kullanılabilirlik – Webtest sonuçları | Geçiş veya başarısız olduğunu gösteren web test sonuçları için halka grafiği gösterir. Günlük aramasını çalıştırmak için grafiği tıklatın<code>ApplicationInsights &#124; where TelemetryType == "Availability" &#124; summarize AggregatedValue = sum(SampledCount) by AvailabilityResult</code> <br><br> Sonuçlar, tüm testler için geçiş ve hata sayısını gösterir. Tüm Web Uygulamalarını son dakika trafiğiyle gösterir. Başarısız web testlerinin ayrıntılarını gösteren günlük aramasını görüntülemek için bir uygulama adını tıklatın. |
| Sunucu İstekleri – Saat başına istekler | Çeşitli uygulamalar için sunucu isteklerinin saat başına bir çizgi grafiğini gösterir. Zaman içinde bir nokta için istek alan ilk 3 uygulamayı görmek için grafikteki bir çizginin üzerine takın. Ayrıca, istek alan uygulamaların listesini ve seçilen döneme ait istek sayısını da gösterir. <br><br>Çeşitli uygulamalar için sunucu isteklerinin saat başına daha ayrıntılı bir çizgi grafiğini gösteren bir günlük araması çalıştırmak için <code>ApplicationInsights &#124; where TelemetryType == "Request" &#124; summarize AggregatedValue = sum(SampledCount) by ApplicationName, bin(TimeGenerated, 1h)</code> grafiği tıklatın. <br><br> İsteklerin listesini, zaman içinde istek <code>ApplicationInsights &#124; where ApplicationName == "yourapplicationname" and TelemetryType == "Request" and iff(isnotnull(toint(RequestSuccess)), RequestSuccess == false, RequestSuccess == "false") == true</code> ve istek süresi için grafikleri ve istek yanıt kodlarının listesini gösteren bir günlük araması çalıştırmak için listedeki bir uygulamayı tıklatın.   |
| Hatalar – Saat başına başarısız istekler | Saat başına başarısız uygulama isteklerinin bir çizgi grafiğini gösterir. Zaman içinde bir nokta için başarısız istekleri ile ilk 3 uygulamaları görmek için grafik üzerinde gezinmek. Ayrıca, her biri için başarısız istek sayısına sahip uygulamaların listesini de gösterir. Başarısız uygulama isteklerinin daha <code>ApplicationInsights &#124; where TelemetryType == "Request" and iff(isnotnull(toint(RequestSuccess)), RequestSuccess == false, RequestSuccess == "false") == true &#124; summarize AggregatedValue = sum(SampledCount) by ApplicationName, bin(TimeGenerated, 1h)</code> ayrıntılı bir çizgi grafiğini gösteren bir günlük araması çalıştırmak için grafiği tıklatın. <br><br>Başarısız istekleri, zaman içinde başarısız isteklerin grafiklerini ve istek süresini ve başarısız istek yanıt kodlarının listesini gösteren bir günlük aramasını çalıştırmak için <code>ApplicationInsights &#124; where ApplicationName == "yourapplicationname" and TelemetryType == "Request" and iff(isnotnull(toint(RequestSuccess)), RequestSuccess == false, RequestSuccess == "false") == true</code> listedeki bir öğeyi tıklatın. |
| İstisnalar – Saat başına özel durumlar | Saat başına özel durumların bir çizgi grafiğini gösterir. Zaman içinde bir nokta için istisnalar dışında ilk 3 uygulamaları görmek için grafiğin üzerine tover. Ayrıca, her biri için özel durum sayısı olan uygulamaların listesini gösterir. Özel durumların daha ayrıntılı bir <code>ApplicationInsights &#124; where TelemetryType == "Exception" &#124; summarize AggregatedValue = sum(SampledCount) by ApplicationName, bin(TimeGenerated, 1h)</code> bağlantı grafiğini gösteren bir günlük araması çalıştırmak için grafiği tıklatın. <br><br>Özel durumların listesini, zaman içinde özel <code>ApplicationInsights &#124; where ApplicationName == "yourapplicationname" and TelemetryType == "Exception"</code> durumlara yönelik grafikleri ve başarısız istekleri gösteren günlük aramasını çalıştırmak için listedeki bir öğeyi ve özel durum türlerinin listesini tıklatın.  |

### <a name="view-the-application-insights-perspective-with-log-search"></a>Günlük aramaile Uygulama Öngörüleri perspektifini görüntüleyin

Panodaki herhangi bir öğeyi tıklattığınızda, aramada gösterilen bir Uygulama Öngörüleri perspektifi görürsünüz. Perspektif, seçilen telemetri türüne dayalı genişletilmiş bir görselleştirme sağlar. Yani, farklı telemetri türleri için görselleştirme içeriği değişiklikleri.

Uygulamalar çubuğunda herhangi bir yeri tıklattığınızda, varsayılan **Uygulamalar** perspektifini görürsünüz.

![Uygulama Öngörüleri Uygulamaları perspektifi](./media/app-insights-connector/applications-blade-drill-search.png)

Perspektif, seçtiğiniz uygulamanın genel görünümünü gösterir.

**Kullanılabilirlik** bıçağı, web test sonuçlarını ve ilgili başarısız istekleri görebileceğiniz farklı bir perspektif görünümü gösterir.

![Uygulama Öngörüleri Kullanılabilirlik perspektifi](./media/app-insights-connector/availability-blade-drill-search.png)

**Sunucu İstekleri** veya **Hataları** bıçaklarında herhangi bir yeri tıklattığınızda, perspektif bileşenleri size isteklerle ilgili bir görselleştirme sağlamak için değişir.

![Uygulama Öngörüleri Hatalar bıçak](./media/app-insights-connector/server-requests-failures-drill-search.png)

**Özel Durumlar** bıçağının herhangi bir yerine tıkladığınızda, özel durumlara göre uyarlanmış bir görselleştirme görürsünüz.

![Uygulama Öngörüleri İstisnalar bıçak](./media/app-insights-connector/exceptions-blade-drill-search.png)

Bir şeyi tıklayıp tıklatmadığınızdan bağımsız olarak, Arama **sayfasının** kendisi içinde, **Uygulama** Öngörüleri verilerini döndüren tüm sorgular Uygulama Öngörüleri perspektifini gösterir. Örneğin, Application Insights verilerini görüntüleyorsanız, **&#42;** sorgusu perspektif sekmesini aşağıdaki resim gibi gösterir:

![Application Insights](./media/app-insights-connector/app-insights-search.png)

Perspektif bileşenleri arama sorgusuna bağlı olarak güncelleştirilir. Bu, verileri görmenizi sağlayan herhangi bir arama alanını kullanarak sonuçları filtreleyebilirsiniz:

- Tüm uygulamalarınız
- Tek bir seçili uygulama
- Bir uygulama grubu

### <a name="pivot-to-an-app-in-the-azure-portal"></a>Azure portalındaki bir uygulamaya dönüş

Uygulama Öngörüleri Bağlayıcı bıçakları, *Azure portalını kullandığınızda*seçili Application Insights uygulamasına dönebilmeniz için tasarlanmıştır. Çözümü, bir uygulamayı sorun gidermenize yardımcı olan üst düzey bir izleme platformu olarak kullanabilirsiniz. Bağlı uygulamalarınızdan herhangi birinde olası bir sorun gördüğünüzde, Log Analytics aramasında bu sorunu inceleyebilir veya doğrudan Application Insights uygulamasına dönebilirsiniz.

Pivot için, her satırın sonunda görünen elipsler **(...**) 'yi tıklatın ve **Uygulama Öngörüleri'nde Aç'ı**seçin.

>[!NOTE]
>**Uygulama Öngörüleri'nde Açık,** Azure portalında kullanılamıyor.

![Uygulama Öngörülerinde Açık](./media/app-insights-connector/open-in-app-insights.png)

### <a name="sample-corrected-data"></a>Örnek düzeltilmiş veriler

Application Insights, telemetri trafiğini azaltmaya yardımcı olmak için *[örnekleme düzeltmesi](../../azure-monitor/app/sampling.md)* sağlar. Application Insights uygulamanızda örneklemeyi etkinleştirdiğinizde, hem Uygulama Öngörüleri'nde hem de Log Analytics'te depolanan daha az sayıda giriş elde elabilirsiniz. Veri tutarlılığı **Application Insights Bağlayıcı** sı ve perspektiflerinde korunurken, özel sorgularınız için örneklenmiş verileri el ile düzeltmeniz gerekir.

Burada bir günlük arama sorgusunda örnekleme düzeltme bir örnektir:

```
ApplicationInsights | summarize AggregatedValue = sum(SampledCount) by TelemetryType
```

**Örneklenmiş Sayım** alanı tüm girişlerde bulunur ve girişin temsil ettiği veri noktalarının sayısını gösterir. Application Insights uygulamanız için örneklemeyi açarsanız, **Örneklenmiş Sayı** 1'den büyüktür. Uygulamanızın oluşturduğu gerçek giriş sayısını saymak için **Örneklenmiş Sayım** alanlarını toplamı.

Örnekleme yalnızca uygulamanızın oluşturduğu toplam giriş sayısını etkiler. Bu alanlar temsil edilen girişlerin ortalamasını gösterdiğinden, **RequestDuration** veya **AvailabilityDuration** gibi metrik alanlar için örneklemeyi düzeltmeniz gerekmez.

## <a name="input-data"></a>Giriş verileri

Çözüm, bağlı Application Insights uygulamalarınızdan aşağıdaki telemetri veri türlerini alır:

- Kullanılabilirlik
- Özel durumlar
- İstekler
- Sayfa görünümleri – Çalışma alanınızın sayfa görüntülemelerini alabilmesi için uygulamalarınızı bu bilgileri toplamak üzere yapılandırmanız gerekir. Daha fazla bilgi için [Bkz. PageViews](../../azure-monitor/app/api-custom-events-metrics.md#page-views).
- Özel etkinlikler – Çalışma alanınızın özel etkinlikler alması için uygulamalarınızı bu bilgileri toplamak üzere yapılandırmanız gerekir. Daha fazla bilgi için [TrackEvent'e](../../azure-monitor/app/api-custom-events-metrics.md#trackevent)bakın.

Veriler, Kullanıma sunulduğunda Log Analytics tarafından Application Insights'tan alınır.

## <a name="output-data"></a>Çıktı verileri

Her giriş verisi türü için *applicationinsights* *türüne* sahip bir kayıt oluşturulur. ApplicationInsights kayıtlarıaşağıdaki bölümlerde gösterilen özelliklere sahiptir:

### <a name="generic-fields"></a>Genel alanlar

| Özellik | Açıklama |
| --- | --- |
| Tür | ApplicationInsights |
| Müşteri IP |   |
| TimeGenerated | Kaydın zamanı |
| ApplicationID | Application Insights uygulamasının enstrümantasyon anahtarı |
| ApplicationName | Uygulama Öngörüleri uygulamasının adı |
| RoleInstance | Sunucu ana bilgisayarkimliği |
| DeviceType | İstemci aygıtı |
| Ekran Çözünürlüğü |   |
| Kıta | Talebin kaynaklandığı kıta |
| Ülke | Talebin kaynaklandığı ülke/bölge |
| Eyaleti | İsteğin kaynağı olan il, eyalet veya yerel |
| Şehir | Talebin kaynaklandığı şehir veya kasaba |
| isSentetik | İsteğin bir kullanıcı tarafından mı yoksa otomatik yöntemle mi oluşturulduğunu gösterir. True = otomatik yöntem veya false = kullanıcı oluşturulan |
| Örnekleme Hızı | Portala gönderilen SDK tarafından oluşturulan telemetri yüzdesi. Aralık 0.0-100.0. |
| Örneklenmiş Sayma | 100/(Örnekleme Oranı). Örneğin, 4&gt; = 25% |
| ısauthenticated | Doğru veya yanlış |
| OperationID | Aynı işlem kimliğine sahip öğeler portalda İlgili Öğeler olarak gösterilir. Genellikle istek kimliği |
| ParentOperationID | Üst işlemin kimliği |
| ThrottledRequests |   |
| SessionId | İsteğin oluşturulduğu oturumu benzersiz olarak tanımlamak için GUID |
| SourceSystem | ApplicationInsights |

### <a name="availability-specific-fields"></a>Kullanılabilirlik özel alanlar

| Özellik | Açıklama |
| --- | --- |
| TelemetriTipi | Kullanılabilirlik |
| AvailabilityTestName | Web testinin adı |
| AvailabilityRunLocation | Http isteğinin coğrafi kaynağı |
| Kullanılabilirlik Sonucu | Web testinin başarı sonucunu gösterir |
| Kullanılabilirlik Mesajı | Web sınaması ekli ileti |
| Kullanılabilirlik Sayısı | 100/(Örnekleme Oranı). Örneğin, 4&gt; = 25% |
| DataSizeMetricValue | 1.0 veya 0.0 |
| VeriBoyutMetricCount | 100/(Örnekleme Oranı). Örneğin, 4&gt; = 25% |
| KullanılabilirlikSüresi | Web test süresinin milisaniye cinsinden süresi |
| AvailabilityDurationCount | 100/(Örnekleme Oranı). Örneğin, 4&gt; = 25% |
| Kullanılabilirlik Değeri |   |
| KullanılabilirlikMetricCount |   |
| AvailabilityTestid | Web testi için benzersiz GUID |
| KullanılabilirlikZaman Damgası | Kullanılabilirlik testinin kesin zaman damgası |
| KullanılabilirlikDurationMin | Örneklenen kayıtlar için bu alan, temsil edilen veri noktaları için minimum web test süresini (milisaniye) gösterir |
| AvailabilityDurationMax | Örneklenen kayıtlar için bu alan, temsil edilen veri noktaları için maksimum web test süresini (milisaniye) gösterir |
| AvailabilityDurationStdDev | Örneklenen kayıtlar için, bu alan temsil edilen veri noktaları için tüm web test süreleri (milisaniye) arasındaki standart sapmayı gösterir |
| KullanılabilirlikMin |   |
| AvailabilityMax |   |
| AvailabilityStdDev | &nbsp;  |

### <a name="exception-specific-fields"></a>Özel özel alanlar

| Tür | ApplicationInsights |
| --- | --- |
| TelemetriTipi | Özel durum |
| Özel Durum Türü | Özel durum türü |
| Özel Durum Yöntemi | Özel durum oluşturan yöntem |
| Özel Durum Montajı | Derleme çerçeve ve sürümü yanı sıra ortak anahtar belirteci içerir |
| Özel Durum Grubu | Özel durum türü |
| Özel durumele | Özel durumu işleyen düzeyi gösterir |
| Özel Durum Sayısı | 100/(Örnekleme Oranı). Örneğin, 4&gt; = 25% |
| Özel Durum Mesajı | Özel durum iletisi |
| Özel Durum Yığını | Özel durum tam yığını |
| ExceptionHasStack | Doğru, özel durum bir yığın varsa |



### <a name="request-specific-fields"></a>İstek özel alanlar

| Özellik | Açıklama |
| --- | --- |
| Tür | ApplicationInsights |
| TelemetriTipi | İstek |
| Yanıt Kodu | Istemciye gönderilen HTTP yanıtı |
| İstekBaşarı | Başarı veya başarısızlığı gösterir. True veya false. |
| RequestID | İsteği benzersiz bir şekilde tanımlamak için kimlik |
| İstek Adı | GET/POST + URL tabanı |
| İstekSüresi | İstek süresinin saniye cinsinden süresi |
| URL'si | Ana bilgisayar dahil olmayan isteğin URL'si |
| Host | Web sunucusu ana bilgisayar |
| URLBase | İsteğin tam URL'si |
| ApplicationProtocol | Uygulama tarafından kullanılan protokol türü |
| İstek Sayısı | 100/(Örnekleme Oranı). Örneğin, 4&gt; = 25% |
| İstekSüre Sayısı | 100/(Örnekleme Oranı). Örneğin, 4&gt; = 25% |
| İstekDurationMin | Örneklenen kayıtlar için bu alan, temsil edilen veri noktaları için en az istek süresini (milisaniye) gösterir. |
| RequestDurationMax | Örneklenen kayıtlar için bu alan, temsil edilen veri noktaları için maksimum istek süresini (milisaniye) gösterir |
| İstekDurationStdDev | Örneklenen kayıtlar için bu alan, temsil edilen veri noktaları için tüm istek süreleri (milisaniye) arasındaki standart sapmayı gösterir |

## <a name="sample-log-searches"></a>Örnek günlük aramaları

Bu çözümde panoda gösterilen örnek günlük aramaları kümesi yoktur. Ancak, açıklamalar içeren örnek günlük arama [sorguları, Uygulama Öngörülerini Görüntüle Bağlayıcı bilgi](#view-application-insights-connector-information) bölümünde gösterilir.

## <a name="removing-the-connector-with-powershell"></a>PowerShell ile konektörü kaldırma
OMS portalı amortismanı ile, varolan bağlantıları yapılandırmanın ve portaldan kaldırmanın bir yolu yoktur. Aşağıdaki PowerShell komut dosyasıyla varolan bağlantıları kaldırabilirsiniz. Bu işlemi gerçekleştirmek için çalışma alanının sahibi veya katılımcısı ve Application Insights kaynağının okuyucusu olmalısınız.

```powershell
$Subscription_app = "App Subscription Name"
$ResourceGroup_app = "App ResourceGroup"
$Application = "Application Name"
$Subscription_workspace = "Workspace Subscription Name"
$ResourceGroup_workspace = "Workspace ResourceGroup"
$Workspace = "Workspace Name"

Connect-AzAccount
Set-AzContext -SubscriptionId $Subscription_app
$AIApp = Get-AzApplicationInsights -ResourceGroupName $ResourceGroup_app -Name $Application 
Set-AzContext -SubscriptionId $Subscription_workspace
Remove-AzOperationalInsightsDataSource -WorkspaceName $Workspace -ResourceGroupName $ResourceGroup_workspace -Name $AIApp.Id
```

REST API çağrısı nı çağıran aşağıdaki PowerShell komut dosyasını kullanarak uygulamaların listesini alabilirsiniz. 

```powershell
Connect-AzAccount
$Tenant = "TenantId"
$Subscription_workspace = "Workspace Subscription Name"
$ResourceGroup_workspace = "Workspace ResourceGroup"
$Workspace = "Workspace Name"
$AccessToken = "AAD Authentication Token" 

Set-AzContext -SubscriptionId $Subscription_workspace
$LAWorkspace = Get-AzOperationalInsightsWorkspace -ResourceGroupName $ResourceGroup_workspace -Name $Workspace

$Headers = @{
    "Authorization" = "Bearer $($AccessToken)"
    "x-ms-client-tenant-id" = $Tenant
}

$Connections = Invoke-RestMethod -Method "GET" -Uri "https://management.azure.com$($LAWorkspace.ResourceId)/dataSources/?%24filter=kind%20eq%20'ApplicationInsights'&api-version=2015-11-01-preview" -Headers $Headers
$ConnectionsJson = $Connections | ConvertTo-Json
```
Bu komut dosyası, Azure Etkin Dizin'e karşı kimlik doğrulama için bir taşıyıcı kimlik doğrulaması belirteci gerektirir. Bu belirteci almak için bir yolu [REST API dokümantasyon sitesinde](https://docs.microsoft.com/rest/api/loganalytics/datasources/createorupdate)bir makale kullanıyor. **Tazyi'yi** tıklatın ve Azure aboneliğinize giriş yapın. Aşağıdaki resimde gösterildiği **gibi,** taşıyıcı belirteci İstek Önizleme'den kopyalayabilirsiniz.


![Taşıyıcı belirteci](media/app-insights-connector/bearer-token.png)


Ayrıca, günlük sorgusu kullanan uygulamaların listesini de alabilirsiniz:

```Kusto
ApplicationInsights | summarize by ApplicationName
```

## <a name="next-steps"></a>Sonraki adımlar

- Uygulama Öngörüleri uygulamalarınız için ayrıntılı bilgileri görüntülemek için [Günlük Arama'yı](../../azure-monitor/log-query/log-query-overview.md) kullanın.
