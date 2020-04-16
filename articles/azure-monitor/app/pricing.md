---
title: Azure Uygulama Öngörüleri için kullanımı ve maliyetleri yönetme | Microsoft Dokümanlar
description: Uygulama Öngörüleri'nde telemetri hacimlerini yönetin ve maliyetleri izleyin.
ms.topic: conceptual
author: DaleKoetke
ms.author: dalek
ms.date: 11/27/2019
ms.reviewer: mbullwin
ms.openlocfilehash: 0225484de06ae4e595f1dcbcdd520f4e0e4d53f5
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81405384"
---
# <a name="manage-usage-and-costs-for-application-insights"></a>Application Insights için kullanımı ve maliyetleri yönetme

> [!NOTE]
> Bu makalede, Uygulama Öngörüleri maliyetlerinizi nasıl anlayacağınızı ve denetlayacağınızı açıklanmaktadır.  İlgili bir makale, [Kullanımı ve tahmini maliyetleri izleme,](https://docs.microsoft.com/azure/azure-monitor/platform/usage-estimated-costs) farklı fiyatlandırma modelleri için birden çok Azure izleme özelliği nde kullanımı niçin görüntülenebildiğini ve tahmini maliyetleri açıklar.

Application Insights, ister Azure'da ister şirket içinde barındırılan web uygulamalarınızın kullanılabilirliğini, performansını ve kullanımını izlemek için ihtiyacınız olan her şeyi elde etmek için tasarlanmıştır. Application Insights ,.NET, Java ve Node.js gibi popüler dilleri ve çerçeveleri destekler ve DevOps işlemleri ve Azure DevOps, Jira ve PagerDuty gibi araçlarla tümleştirir. Uygulamalarınızı izlemenin maliyetlerini neyin belirlediğini anlamak önemlidir. Bu makalede, uygulama izleme maliyetlerinizi neyin yönlendirdiğini ve bunları nasıl proaktif olarak izleyip kontrol edebileceğinizi gözden geçiriyoruz.

Uygulama Öngörüleri için fiyatlandırmanın nasıl çalıştığı yla ilgili sorularınız varsa, [forumumuzda](https://social.msdn.microsoft.com/Forums/home?forum=ApplicationInsights&filter=alltypes&sort=lastpostdesc)bir soru gönderebilirsiniz.

## <a name="pricing-model"></a>Fiyatlandırma modeli

[Azure Uygulama Öngörüleri][start] için fiyatlandırma, satın alınan veri hacmine ve isteğe bağlı olarak daha uzun veri saklama ya da isteğe bağlı olarak temel alan Bir **Ödeme-Ardından** Ödeme modelidir. Her Application Insights kaynağı ayrı bir hizmet olarak ücretlendirilir ve Azure aboneliğinizin faturasına katkıda bulunur. Veri hacmi, uygulamanızdan Application Insights tarafından alınan sıkıştırılmamış JSON veri paketinin boyutu olarak ölçülür. [Canlı Ölçümler Akışı'nı](../../azure-monitor/app/live-stream.md)kullanmak için veri hacmi ücreti yoktur.

[Çok adımlı web testleri](../../azure-monitor/app/availability-multistep.md) ek bir ücrete tabidir. Çok adımlı web testleri, bir dizi eylem gerçekleştiren web testleridir. Tek bir sayfanın *ping testleri* için ayrı bir ücret alınmaz. Ping testlerinden ve çok adımlı testlerden telemetri, uygulamanızdaki diğer telemetriler ile aynı ücrete alınır.

Özel metrik boyutlarda [uyarıyı etkinleştirme](https://docs.microsoft.com/azure/azure-monitor/app/pre-aggregated-metrics-log-metrics#custom-metrics-dimensions-and-pre-aggregation) seçeneği, ek toplama öncesi ölçümlerin oluşturulmasına neden olabileceğinden ek maliyetler de oluşturabilir. Uygulama Öngörüleri'nde günlük tabanlı ve önceden toplanmış ölçümler ve Azure Monitor özel ölçümleri için [fiyatlandırma](https://azure.microsoft.com/pricing/details/monitor/) hakkında [daha fazla bilgi edinin.](https://docs.microsoft.com/azure/azure-monitor/app/pre-aggregated-metrics-log-metrics)

## <a name="estimating-the-costs-to-manage-your-application"></a>Uygulamanızı yönetmek için maliyetleri tahmin etme

Henüz Uygulama Öngörüleri kullanmıyorsanız, Uygulama Öngörüleri'ni kullanmanın maliyetini tahmin etmek için [Azure Monitor fiyatlandırma hesaplayıcısını](https://azure.microsoft.com/pricing/calculator/?service=monitor) kullanabilirsiniz. Arama kutusuna "Azure Monitörü" girerek ve ortaya çıkan Azure Monitor kutucuğuna tıklayarak başlayın. Sayfayı Azure Monitor'a kaydırın ve Tür açılır sayfasından Uygulama Öngörüleri'ni seçin.  Burada ayda toplamayı beklediğiniz GB veri sayısını girebilirsiniz, bu nedenle soru, Uygulama Öngörüleri'nin uygulamanızı izleme de ne kadar veri toplayacak olduğudur.

Bunu ele almak için iki yaklaşım vardır: ASP.NET SDK'da bulunan varsayılan izleme ve uyarlamalı örnekleme kullanımı veya diğer benzer müşterilerin gördüklerine göre olası veri alımınızı tahmin etmek.

### <a name="data-collection-when-using-sampling"></a>Örnekleme kullanırken veri toplama

ASP.NET SDK'nın [uyarlanabilir örneklemesi](sampling.md#adaptive-sampling)ile veri hacmi, varsayılan Application Insights izleme için belirli bir maksimum trafik hızında tutmak üzere otomatik olarak ayarlanır. Uygulama hata ayıklama veya düşük kullanım nedeniyle gibi düşük miktarda telemetri üretirse, birim saniyede yapılandırılan olayların altında olduğu sürece öğeler örnekleme işlemcisi tarafından düşmez. Yüksek hacimli bir uygulama için, varsayılan saniyede beş olay eşiği yle, uyarlanabilir örnekleme günlük olay sayısını 432.000 ile sınırlandıracaktır. Tipik bir ortalama olay boyutu 1 KB kullanarak, bu uygulamanızı barındıran düğüm başına 31 günlük ay başına 13,4 GB telemetri ye karşılık gelir (örnekleme her düğümiçin yerel yapıldığından.) 

Uyarlanabilir örneklemeyi desteklemeyen SDK'lar için, web sunucunuzdan ve web tarayıcılarınızdan gönderilen trafiği azaltmak için, saklanması gereken verilerin yüzdesine göre Uygulama Öngörüleri tarafından veri alındığı zaman hangi örnekleri veya [ASP.NET, ASP.NET Core ve Java web siteleri için sabit oranlı örnekleme](sampling.md#fixed-rate-sampling) yitirme [örneklemesi](https://docs.microsoft.com/azure/azure-monitor/app/sampling#ingestion-sampling)yapabilirsiniz

### <a name="learn-from-what-similar-customers-collect"></a>Benzer müşterilerin ne topladığıhakkında bilgi edinin

Uygulama Öngörüleri için Azure İzleme Fiyatlandırması hesaplayıcısında, "Uygulama etkinliğine dayalı veri hacmini tahmin et" işlevini etkinleştiriseniz, uygulamanız hakkında girişler sağlayabilirsiniz (müşteri tarafı telemetrisini toplarsanız, aylık istekler ve sayfa görünümleri). Bu uygulamalar, Uygulama Öngörüleri yapılandırmaaralığını kapsar (örneğin, bazıları varsayılan [örneklemeye](../../azure-monitor/app/sampling.md)sahiptir, bazılarının örneklemesi yoktur vb.), bu nedenle örneklemeyi kullanarak ortanca düzeyin çok altında yutturtuğunuz veri hacmini azaltma denetimine sahipsiniz. Ama bu, diğer benzer müşterilerin ne gördüğünü anlamak için bir başlangıç noktasıdır.

## <a name="understand-your-usage-and-estimate-costs"></a>Kullanımınızı ve tahmini maliyetlerinizi anlama

Uygulama Öngörüleri, maliyetlerinizin son kullanım alışkanlıklarına bağlı olarak ne gibi olabileceğini anlamanızı kolaylaştırır. Başlangıç için, Azure portalında, Application Insights kaynağı için **Kullanım ve tahmini maliyetler** sayfasına gidin:

![Fiyatlandırma seçin](./media/pricing/pricing-001.png)

A. Ay için veri hacminizi gözden geçirin. Bu, sunucu ve istemci uygulamalarınızdan ve kullanılabilirlik testlerinden alınan ve tutulan tüm verileri (herhangi bir [örneklemeden](../../azure-monitor/app/sampling.md)sonra) içerir.  
B. [Çok adımlı web testleri](../../azure-monitor/app/availability-multistep.md)için ayrı bir ücret lendirme yapılır. (Bu, veri hacmi ücretine dahil olan basit kullanılabilirlik testlerini içermez.)  
C. Son bir aya ait veri hacmi eğilimlerini görüntüleyin.  
D. Veri alım [örneklemesini](../../azure-monitor/app/sampling.md)etkinleştirin.
E. Günlük veri hacmi kapağını ayarlayın.  

(Bu makalede ekran görüntülerinde görüntülenen tüm fiyatların yalnızca örnek amaçlı olduğunu unutmayın. Para biriminizdeki ve bölgenizdeki güncel fiyatlar için [Bkz. Uygulama Öngörüleri fiyatlandırması][pricing].)

Application Insights kullanımınızı daha derinlemesine araştırmak için **Ölçümler** sayfasını açın, "Veri noktası hacmi" adlı metrik ekleyin ve verileri "Telemetri madde türüne" göre bölmek için *Splitting Uygula* seçeneğini seçin.

Uygulama Öngörüleri ücretleri Azure faturanıza eklenir. Azure faturanızın ayrıntılarını Azure portalının **Faturalandırma** bölümünde veya Azure [faturaportalında](https://account.windowsazure.com/Subscriptions)görebilirsiniz.

![Sol menüde Faturalandırma'yı seçin](./media/pricing/02-billing.png)

### <a name="using-data-volume-metrics"></a>Veri hacmi ölçümlerini kullanma
<a id="understanding-ingested-data-volume"></a>

Veri birimleriniz hakkında daha fazla bilgi edinmek için, Uygulama **Öngörüleri** kaynağınız için Ölçümler'i seçerek yeni bir grafik ekleyin. Grafik ölçümü için, **Günlük tabanlı ölçümler** **altında, Veri noktası hacmini**seçin. **Splitting Uygula'yı**tıklatın ve ** `Telemetryitem` türüne**göre grubu seçin.

![Veri hacmine bakmak için Ölçümleri kullanma](./media/pricing/10-billing.png)

### <a name="queries-to-understand-data-volume-details"></a>Veri hacmi ayrıntılarını anlamak için sorgular

Uygulama Öngörüleri için veri birimlerini araştırmak için iki yaklaşım vardır. İlktabloda `systemEvents` toplu bilgileri kullanır ve ikinci sindirilen her olay kullanılabilir `_BilledSize` özelliği kullanır.

#### <a name="using-aggregated-data-volume-information"></a>Toplanan veri hacmi bilgilerini kullanma

Örneğin, `systemEvents` sorguile son 24 saat içinde alınan veri hacmini görmek için tabloyu kullanabilirsiniz:

```kusto
systemEvents
| where timestamp >= ago(24h)
| where type == "Billing"
| extend BillingTelemetryType = tostring(dimensions["BillingTelemetryType"])
| extend BillingTelemetrySizeInBytes = todouble(measurements["BillingTelemetrySize"])
| summarize sum(BillingTelemetrySizeInBytes)
```

Veya son 30 gün boyunca veri türüne göre veri hacmi grafiğini (bayt olarak) görmek için şunları kullanabilirsiniz:

```kusto
systemEvents
| where timestamp >= startofday(ago(30d))
| where type == "Billing"
| extend BillingTelemetryType = tostring(dimensions["BillingTelemetryType"])
| extend BillingTelemetrySizeInBytes = todouble(measurements["BillingTelemetrySize"])
| summarize sum(BillingTelemetrySizeInBytes) by BillingTelemetryType, bin(timestamp, 1d) | render barchart  
```

Bu sorgunun veri hacimleri üzerinde uyarı ayarlamak için bir [Azure Günlüğü Uyarısı'nda](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-unified-log) kullanılabileceğini unutmayın.  

Telemetri veri değişiklikleriniz hakkında daha fazla bilgi edinmek için, sorguyu kullanarak olayların sayısını türüne göre alabiliriz:

```kusto
systemEvents
| where timestamp >= startofday(ago(30d))
| where type == "Billing"
| extend BillingTelemetryType = tostring(dimensions["BillingTelemetryType"])
| summarize count() by BillingTelemetryType, bin(timestamp, 1d)
| render barchart  
```

#### <a name="using-data-size-per-event-information"></a>Olay bilgileri başına veri boyutunu kullanma

Veri birimlerinizin kaynağı hakkında daha fazla bilgi edinmek `_BilledSize` için, yutulan her olayda bulunan özelliği kullanabilirsiniz.

Örneğin, son 30 gün içinde hangi işlemlerin en çok veri `_BilledSize` hacmi oluşturduğuna bakmak için, tüm bağımlılık olayları nın toplamı olabilir:

```kusto
dependencies
| where timestamp >= startofday(ago(30d))
| summarize sum(_BilledSize) by operation_Name
| render barchart  
```

## <a name="viewing-application-insights-usage-on-your-azure-bill"></a>Azure faturanızda Uygulama Öngörüleri kullanımını görüntüleme

Azure, [Azure Maliyet Yönetimi + Faturalandırma](https://docs.microsoft.com/azure/cost-management/quick-acm-cost-analysis?toc=/azure/billing/TOC.json) hub'ında çok sayıda kullanışlı işlevsellik sağlar. Örneğin, "Maliyet çözümlemesi" işlevi, Azure kaynakları için harcamalarınızı görüntülemenizi sağlar. Kaynak türüne göre bir filtre eklemek (Microsoft.insights/components for Application Insights için) harcamalarınızı izlemenize olanak sağlar.

[Kullanımınızı Azure portalından indirerek kullanımınızı](https://docs.microsoft.com/azure/billing/billing-download-azure-invoice-daily-usage-date#download-usage-in-azure-portal)daha iyi anlayabiliriz.
İndirilen elektronik tabloda, azure kaynağı başına günlük kullanımı görebilirsiniz. Bu Excel elektronik tablosunda, "Uygulama Öngörüleri" ve "Log Analytics" göstermek için önce "Sayaç Kategorisi" sütununa filtre uygulayarak ve ardından "Örnek Id" sütununa "microsoft.insights/components içeren" bir filtre ekleyerek Application Insights kaynaklarından kullanım bulunabilir.  Tüm Azure Monitor bileşenleri için tek bir günlük arka ucu olduğundan, Çoğu Uygulama Öngörüleri kullanımı, Günlük Analitiği Metre Kategorisi ile metrelerde raporlanır.  Yalnızca eski fiyatlandırma katmanları ve çok adımlı web testleri yle ilgili Application Insights kaynakları, Uygulama Öngörüleri Metre Kategorisi ile bildirilir.  Kullanım "Tüketilen Miktar" sütununda, her giriş için birim "Ölçü Birimi" sütununda gösterilir.  [Microsoft Azure faturanızı anlamanıza](https://docs.microsoft.com/azure/billing/billing-understand-your-bill)yardımcı olacak daha fazla ayrıntı mevcuttur.

## <a name="managing-your-data-volume"></a>Veri hacminizi yönetme

Gönderdiğiniz veri hacmi aşağıdaki teknikler kullanılarak yönetilebilir:

* **Örnekleme**: Örneklemeyi, sunucu nuzdan ve istemci uygulamalarınızdan gönderilen telemetri miktarını en az düzeyde çarpıtma yla azaltmak için kullanabilirsiniz. Örnekleme, gönderdiğiniz veri miktarını ayarlamak için kullanabileceğiniz birincil araçtır. [Örnekleme özellikleri](../../azure-monitor/app/sampling.md)hakkında daha fazla bilgi edinin.

* **Limit Ajax aramaları**: Her sayfa görünümünde [bildirilebilen Ajax aramalarının sayısını sınırlayabilir](../../azure-monitor/app/javascript.md#configuration) veya Ajax bildirimini kapatabilirsiniz.

* **Gereksiz modülleri devre dışı bırak**: İhtiyacınız olmayan toplama modüllerini kapatmak için [ApplicationInsights.config'i edin.](../../azure-monitor/app/configuration-with-applicationinsights-config.md) Örneğin, performans sayaçlarının veya bağımlılık verilerinin gerekli olmadığına karar verebilirsiniz.

* **Önceden toplu ölçümler**: Uygulamanızda TrackMetric'e çağrı lar koyarsanız, bir dizi ölçümün ortalama ve standart sapmasını hesaplamanızı kabul eden aşırı yükü kullanarak trafiği azaltabilirsiniz. Veya, [önceden toplayıcı](https://www.myget.org/gallery/applicationinsights-sdk-labs)bir paket kullanabilirsiniz.
 
* **Günlük kap**: Azure portalında bir Uygulama Öngörüleri kaynağı oluşturduğunuzda, günlük kap 100 GB/gün olarak ayarlanır. Visual Studio'da bir Application Insights kaynağı oluşturduğunuzda varsayılan değer küçüktür (yalnızca 32,3 MB/gün). Günlük kap varsayılanı, testi kolaylaştırmak için ayarlanmıştır. Kullanıcının uygulamayı üretime dağıtmadan önce günlük kapağı yükseltmesi amaçlanmıştır. 

    Yüksek trafikli bir uygulama için daha yüksek bir maksimum talep etmediğiniz sürece maksimum kapak 1.000 GB/gün'dür.
    
    Günlük kap hakkında uyarı e-postaları, Uygulama Öngörüleri kaynağınız için bu rollerin üyesi olan hesaba gönderilir: "ServiceAdmin", "AccountAdmin", "CoAdmin", "Owner".

    Günlük kapağı ayarlarken dikkatli kullanın. Amacınız günlük *kap vurmak asla*olmalıdır. Günlük kapağı vurursanız, günün geri kalanında veri kaybedersiniz ve uygulamanızı izleyebilirsiniz. Günlük kapağı değiştirmek için **Günlük hacim kapağı** seçeneğini kullanın. Bu seçeneği **Kullan ve tahmini maliyet** bölmesinde erişebilirsiniz (bu daha sonra makalede daha ayrıntılı olarak açıklanmıştır).
    
    Uygulama Öngörüleri için kullanılamamış kredisi olan bazı abonelik türlerine ilişkin kısıtlamayı kaldırdık. Daha önce, aboneliğin bir harcama sınırı varsa, günlük kap iletişim kutusunda harcama sınırını kaldırmak ve günlük kapağın 32,3 MB/gün'ün ötesine yükseltilmesini sağlamak için talimatlar vardır.
    
* **Azaltma**: Azaltma, veri oranını enstrümantasyon anahtarı başına ortalama 1 dakikadan fazla olan saniyede 32.000 olayla sınırlar. Uygulamanızın gönderdiği veri hacmi her dakika değerlendirilir. Dakika içinde ortalama saniyede ortalama oranı aşarsa, sunucu bazı istekleri reddeder. SDK verileri arabellekte ve sonra yeniden göndermeye çalışır. Birkaç dakikaya yayılan bir dalgalanma. Uygulamanız sürekli olarak azaltma hızından daha yüksek oranda veri gönderirse, bazı veriler bırakılır. (ASP.NET, Java ve JavaScript SDK'ları verileri bu şekilde yeniden göndermeye çalışır; diğer SDK'lar yalnızca daraltılmış verileri bırakabilir.) Azaltma gerçekleşirse, bir bildirim uyarısı bunun oluştuğu konusunda sizi uyarır.

## <a name="manage-your-maximum-daily-data-volume"></a>Maksimum günlük veri hacminizi yönetme

Toplanan verileri sınırlamak için günlük birim kapağını kullanabilirsiniz. Ancak, kapak karşılanırsa, günün geri kalanı için uygulamanızdan gönderilen tüm telemetri bir kayıp oluşur. Uygulamanızın günlük kapağı vurması *tavsiye edilmez.* Günlük kap ulaştıktan sonra uygulamanızın sağlık ve performansını izleyebilirsiniz.

Günlük ses kapağını kullanmak yerine, veri hacmini istediğiniz düzeye ayarlamak için [örneklemeyi](../../azure-monitor/app/sampling.md) kullanın. Daha sonra, uygulamanızın beklenmedik bir şekilde çok daha yüksek hacimlerde telemetri göndermeye başlaması durumunda günlük kapağı yalnızca "son çare" olarak kullanın.

### <a name="identify-what-daily-data-limit-to-define"></a>Tanımlayacak günlük veri limitini belirleme

Uygulama Öngörülerini gözden geçirin Veri alma eğilimini ve tanımlayacak günlük birim kapağının ne olduğunu anlamak için tahmini maliyetleri gözden geçirin. Sınıra ulaşıldıktan sonra kaynaklarınızı izleyemeyeceksiniz olduğundan, dikkatli bir şekilde düşünülmelidir.

### <a name="set-the-daily-cap"></a>Günlük Kapağı Nı Ayarla

**Kullanım ve tahmini maliyetler** sayfasında, Uygulama Öngörüleri kaynağınızın **Yapıla** bölümünde günlük kapağı değiştirmek için **Günlük Kap'ı**seçin.

![Günlük telemetri hacim kapağını ayarlama](./media/pricing/pricing-003.png)

[Azure Kaynak Yöneticisi aracılığıyla günlük kapağı değiştirmek](../../azure-monitor/app/powershell.md)için, `dailyQuota`değiştirilen özellik .  Azure Kaynak Yöneticisi aracılığıyla günlük `dailyQuotaResetTime` kapağı ve kapağını da `warningThreshold`ayarlayabilirsiniz.

### <a name="create-alerts-for-the-daily-cap"></a>Daily Cap için uyarılar oluşturma

Uygulama Öngörüleri Günlük Kapağı, yutulan veri hacimleri uyarı düzeyine veya günlük kapak düzeyine ulaştığında Azure etkinlik günlüğünde bir olay oluşturur.  Bu [etkinlik günlüğü olaylarını temel alan bir uyarı oluşturabilirsiniz.](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log#create-with-the-azure-portal) Bu olayların sinyal adları şunlardır:

* Uygulama Öngörüleri bileşeni günlük kap uyarı eşiğine ulaşıldı

* Uygulama Öngörüleri bileşeni günlük kap ulaştı

## <a name="sampling"></a>Örnekleme
[Örnekleme,](../../azure-monitor/app/sampling.md) tanılama aramaları sırasında ilgili olayları bulma yeteneğini korurken uygulamanıza telemetri nin gönderilme hızını azaltma yöntemidir. Ayrıca doğru olay sayılarını da korursunuz.

Örnekleme ücretleri azaltmak ve aylık kota içinde kalmak için etkili bir yoldur. Örnekleme algoritması telemetri ile ilgili öğeleri saklar, bu nedenle, örneğin, Arama'yı kullandığınızda, belirli bir özel durumla ilgili isteği bulabilirsiniz. Algoritma ayrıca doğru sayıları korur, böylece istek oranları, özel durum oranları ve diğer sayımlar için Metrik Gezgini'nde doğru değerleri görürsünüz.

Örneklemenin çeşitli biçimleri vardır.

* [Uyarlanabilir örnekleme,](../../azure-monitor/app/sampling.md) SDK'ASP.NET için varsayılan değerdir. Uyarlanabilir örnekleme, uygulamanızın gönderdiği telemetri hacmine otomatik olarak ayarlanır. Ağdaki telemetri trafiğinin azalması için web uygulamanızdaki SDK'da otomatik olarak çalışır. 
* *Yutma örneklemesi,* uygulamanızdaki telemetrinin Application Insights hizmetine girdiği noktada çalışan bir alternatiftir. Yutma örneklemesi uygulamanızdan gönderilen telemetrinin hacmini etkilemez, ancak hizmet tarafından tutulan hacmi azaltır. Tarayıcılardan ve diğer SDK'lardan gelen telemetri tarafından kullanılan kotayı azaltmak için yutma örneklemesini kullanabilirsiniz.

Alım örneklemesini ayarlamak için **Fiyatlandırma** bölmesine gidin:

![Kota ve fiyatlandırma bölmesinde, Örnekler döşemesini seçin ve ardından bir örnekleme fraksiyonu seçin](./media/pricing/pricing-004.png)

> [!WARNING]
> **Veri örnekleme** bölmesi yalnızca yutma örneklemesinin değerini denetler. Uygulamanızdaki Application Insights SDK tarafından uygulanan örnekleme oranını yansıtmaz. Gelen telemetri SDK'da zaten örneklenmişse, yutma örneklemesi uygulanmaz.
>

Gerçek örnekleme oranını keşfetmek için, nerede uygulanırsa uygulansın, bir [Analytics sorgusu](analytics.md)kullanın. Sorgu şuna benzer:

    requests | where timestamp > ago(1d)
    | summarize 100/avg(itemCount) by bin(timestamp, 1h)
    | render areachart

Tutulan her kayıtta, `itemCount` temsil ettiği özgün kayıt sayısını gösterir. Önceki atılan kayıtların sayısı 1 +'a eşittir.

## <a name="change-the-data-retention-period"></a>Veri saklama süresini değiştirme

Application Insights kaynakları için varsayılan bekletme 90 gündür. Her Uygulama Öngörüleri kaynağı için farklı bekletme süreleri seçilebilir. Kullanılabilir saklama sürelerinin tamamı 30, 60, 90, 120, 180, 270, 365, 550 veya 730 gündür.

Uygulama Öngörüleri kaynağınızdan bekletmeyi değiştirmek için **Kullanım ve Tahmini Maliyetler** sayfasına gidin ve Veri **Saklama** seçeneğini seçin:

![Günlük telemetri hacim kapağını ayarlama](./media/pricing/pricing-005.png)

Bekletme indirildiğinde, en eski verilerin kaldırılmasından önce birkaç günlük yetkisiz kullanım süresi vardır.

Bekletme, `retentionInDays` parametre kullanılarak [PowerShell kullanılarak programlı olarak](powershell.md#set-the-data-retention) da ayarlanabilir. Veri saklamayı 30 güne ayarlarsanız, parametreyi `immediatePurgeDataOn30Days` kullanarak eski verilerin hemen tasfiyesini tetikleyebilirsiniz ve bu da uyumlulukla ilgili senaryolar için yararlı olabilir. Bu temizleme işlevi yalnızca Azure Kaynak Yöneticisi aracılığıyla ortaya çıkarır ve çok dikkatli kullanılmalıdır. Veri hacmi kapağının günlük sıfırlama süresi, parametreyi `dailyQuotaResetTime` ayarlamak için Azure Kaynak Yöneticisi kullanılarak yapılandırılabilir.

## <a name="data-transfer-charges-using-application-insights"></a>Application Insights'ı kullanarak veri aktarım ücretleri

Application Insights'a veri göndermek veri bant genişliği ücretlerine neden olabilir. Azure Bant [Genişliği fiyatlandırma sayfasında](https://azure.microsoft.com/pricing/details/bandwidth/)açıklandığı gibi, iki bölgede bulunan Azure hizmetleri arasındaki veri aktarımı normal hızda giden veri aktarımı olarak ücretlendirilir. Gelen veri aktarımı ücretsizdir. Ancak, bu ücret çok küçük (% birkaç) Uygulama Öngörüleri günlük veri alımı maliyetleri ile karşılaştırıldığında. Sonuç olarak Log Analytics'in maliyetlerini kontrol etmek için sindirilen veri hacminize odaklanması gerekir ve [burada](https://docs.microsoft.com/azure/azure-monitor/app/pricing#managing-your-data-volume)bunu anlamamıza yardımcı olacak bir kılavuzumuz vardır.

## <a name="limits-summary"></a>Limitler özeti

[!INCLUDE [application-insights-limits](../../../includes/application-insights-limits.md)]

## <a name="disable-daily-cap-e-mails"></a>Günlük kapak e-postalarını devre dışı

**Kullanım ve tahmini maliyetler** bölmesinde, Uygulama Öngörüleri kaynağınızın **Yapılandırma** bölümü altında, günlük birim kapağı e-postaları devre dışı kalmak için **Günlük Kap'ı**seçin. Kapak ulaşıldığında ve ayarlanabilir bir uyarı düzeyine ulaşıldığında e-posta göndermek için ayarlar vardır. Günlük kapak hacmiyle ilgili tüm e-postaları devre dışı kılmış olmak istiyorsanız, her iki kutuyu da kaldırın.

## <a name="legacy-enterprise-per-node-pricing-tier"></a>Eski Enterprise (Düğüm Başına) fiyatlandırma katmanı

Azure Application Insights'ı ilk benimseyenler için hala iki olası fiyatlandırma katmanı vardır: Temel ve Kurumsal. Temel fiyatlandırma katmanı yukarıda açıklandığı gibi aynıdır ve varsayılan katmandır. Ek ücret ödemeden tüm Enterprise katman özelliklerini içerir. Temel katman, öncelikle yutulan veri hacmine göre faturalanır.

> [!NOTE]
> Bu eski fiyatlandırma katmanları yeniden adlandırıldı. Kurumsal fiyatlandırma katmanı artık **Düğüm Başına** olarak adlandırılır ve Temel fiyatlandırma katmanı artık GB **Başına**olarak adlandırılır. Bu yeni adlar aşağıda ve Azure portalında kullanılır.  

Düğüm Başına (eski adıyla Enterprise) katmanında düğüm başına ücret alınır ve her düğüm günlük veri payı alır. Per Node fiyatlandırma katmanında, dahil edilen ödeneğin üzerinde alınan veriler için ücretlendirilirsiniz. Operations Management Suite kullanıyorsanız, Düğüm Başına katmanı nı seçmelisiniz.

Para biriminizdeki ve bölgenizdeki güncel fiyatlar için [Uygulama Öngörüleri](https://azure.microsoft.com/pricing/details/application-insights/)fiyatlandırması'na bakın.

> [!NOTE]
> Nisan 2018'de Azure izleme için yeni bir fiyatlandırma modeli [sunduk.](https://azure.microsoft.com/blog/introducing-a-new-way-to-purchase-azure-monitoring-services/) Bu model, izleme hizmetlerinin tüm portföyünde basit bir "you-you-go öde" modelini benimser. Yeni fiyatlandırma [modeli](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-usage-and-estimated-costs)hakkında daha fazla bilgi edinin , kullanım alışkanlıklarınıza göre [bu modele geçmenin etkisini](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-usage-and-estimated-costs#understanding-your-azure-monitor-costs) nasıl değerlendirebilirsiniz ve [yeni modeli nasıl seçebilirsiniz](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-usage-and-estimated-costs#azure-monitor-pricing-model)

### <a name="per-node-tier-and-operations-management-suite-subscription-entitlements"></a>Düğüm katmanı ve Operasyon Yönetimi Paketi abonelik hakları

Operations Management Suite E1 ve E2 satın alan müşteriler, [daha önce duyurulduğu](https://blogs.technet.microsoft.com/msoms/2017/05/19/azure-application-insights-enterprise-as-part-of-operations-management-suite-subscription/)gibi ek bir ücret ödemeden Ek Bir bileşen olarak Node Başına Uygulama Öngörüleri alabilir. Özellikle, Operasyon Yönetimi Paketi E1 ve E2'nin her birimi, Düğüm Başına Uygulama Öngörüleri katmanının bir düğümüne hak kazanır. Her Uygulama Öngörüdüğümü, günde 200 MB'a kadar (Log Analytics veri alımından ayrı) ve ek ücret ödemeden 90 günlük veri saklama yı içerir. Katman daha sonra makalede ayrıntılı olarak açıklanmıştır.

Bu katman yalnızca Operasyon Yönetimi Paketi aboneliği olan müşteriler için geçerli olduğundan, Operasyon Yönetimi Paketi aboneliği olmayan müşteriler bu katmanı seçme seçeneği görmez.

> [!NOTE]
> Bu yetkiyi aldığınızdan emin olmak için, Application Insights kaynaklarınızın Per Node fiyatlandırma katmanında olması gerekir. Bu yetki yalnızca düğüm olarak geçerlidir. GB başına katmandaki Application Insights kaynakları herhangi bir fayda sağlamaz. Bu hak, **Kullanım'da ve tahmini maliyet** bölmesinde gösterilen tahmini maliyetlerde görünmez. Ayrıca, bir aboneliği Nisan 2018'de yeni Azure izleme fiyatlandırma modeline taşırsanız, GB Başına katman kullanılabilir tek katmandır. Bir aboneliğiyeni Azure izleme fiyatlandırma modeline taşımak, Bir Operations Management Suite aboneliğiniz varsa tavsiye edilmez.

### <a name="how-the-per-node-tier-works"></a>Per Node katmanı nasıl çalışır?

* Per Node katmanındaki herhangi bir uygulama için telemetri gönderen her düğüm için ödeme yaparsınız.
  * *Düğüm,* fiziksel veya sanal bir sunucu makinesi veya uygulamanızı barındıran bir hizmet olarak platform rolü örneğidir.
  * Geliştirme makineleri, istemci tarayıcıları ve mobil aygıtlar düğüm olarak sayılmaz.
  * Uygulamanızda web hizmeti ve arka uç çalışanı gibi telemetri gönderen birkaç bileşen varsa, bileşenler ayrı olarak sayılır.
  * [Canlı Ölçümler Akışı](../../azure-monitor/app/live-stream.md) verileri fiyatlandırma amacıyla sayılmaz. Bir abonelikte, ücretleriniz uygulama başına değil, düğüm başınadır. 12 uygulama için telemetri gönderen beş düğüm varsa, ücret beş düğüm içindir.
* Ücretler ayda teklif edilebilse de, yalnızca bir düğümün bir uygulamadan telemetri gönderdiği herhangi bir saat için ücretlendirilirsiniz. Saatlik ücret, 744'e (31 günlük bir aydaki saat sayısı) bölünen aylık ücrettir.
* Algılanan her düğüm için (saatlik parçalı olarak) günde 200 MB veri hacmi ayırma sı verilir. Kullanılmayan veri ayırma bir günden diğerine aktarılmaz.
  * Per Node fiyatlandırma katmanını seçerseniz, her abonelik, bu abonelikteki Application Insights kaynaklarına telemetri gönderen düğüm sayısına bağlı olarak günlük veri ödeneği alır. Bu nedenle, tüm gün veri gönderen beş düğümvarsa, bu abonelikteki tüm Application Insights kaynaklarına 1 GB'lık bir havuzlu ödenek uygulanır. Belirli düğümlerin diğer düğümlerden daha fazla veri göndermesi önemli değildir, çünkü dahil edilen veriler tüm düğümler arasında paylaşılır. Belirli bir günde, Application Insights kaynakları bu abonelik için günlük veri ayırmada dahil edilenden daha fazla veri alıyorsa, GB başına fazlalık veri ücretleri uygulanır. 
  * Günlük veri indirimi, her düğümün 24 mb'a bölünerek gönderdiği gün (UTC kullanılarak) saat sayısı olarak hesaplanır. Yani, günün 24 saat 15 sırasında telemetri göndermek dört düğüm varsa, o gün için dahil veri ((4 &#215; 15) / 24) &#215; 200 MB = 500 MB olacaktır. Veri aktarımı için GB başına 2,30 USD fiyatla, düğümlerin o gün 1 GB veri göndermesi durumunda ücret 1,15 USD olacaktır.
  * Düğüm başına günlük ödenek, GB Başına katmanı seçtiğiniz uygulamalarla paylaşılmaz. Kullanılmayan ödenek günden güne yapılmaz.

### <a name="examples-of-how-to-determine-distinct-node-count"></a>Farklı düğüm sayısını belirleme örnekleri

| Senaryo                               | Toplam günlük düğüm sayısı |
|:---------------------------------------|:----------------:|
| 3 Azure Uygulama Hizmeti örneği ve 1 sanal sunucu kullanarak 1 uygulama | 4 |
| 2 VM'de çalışan 3 uygulama; Bu uygulamalar için Application Insights kaynakları aynı abonelikte ve Per Node katmanında | 2 | 
| Applications Insights kaynakları aynı abonelikte olan 4 uygulama; her uygulama 16 yoğun olmayan saatlerde 2 örnek çalıştırıyor ve 8 yoğun saat boyunca 4 örnek | 13.33 |
| Her biri 2 örnek çalıştıran 1 İşçi Rolü ve 1 Web Rolüne sahip bulut hizmetleri | 4 | 
| 50 mikro hizmet çalıştıran 5 düğümlü Azure Service Fabric kümesi; her microservice çalışan 3 örnekleri | 5|

* Hassas düğüm sayımı, uygulamanızın hangi Application Insights SDK'yı kullandığına bağlıdır. 
  * SDK sürümleri 2.2 ve sonraki sürümlerinde, hem Application Insights [Core SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights/) hem de [Web SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/) her uygulama ana bilgisayarını düğüm olarak bildirir. Örnekler fiziksel sunucu ve VM ana bilgisayarları veya bulut hizmetleri için örnek adı için bilgisayar adıdır.  Bunun tek istisnası yalnızca [.NET Core](https://dotnet.github.io/) ve Application Insights Core SDK kullanan bir uygulamadır. Bu durumda, ana bilgisayar adı kullanılamadığından tüm ana bilgisayarlar için yalnızca bir düğüm bildirilir.
  * SDK'nın önceki [sürümlerinde, Web SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/) yeni SDK sürümleri gibi olur, ancak [Core SDK,](https://www.nuget.org/packages/Microsoft.ApplicationInsights/) uygulama ana bilgisayarlarının sayısına bakılmaksızın yalnızca bir düğüm bildirir.
  * Uygulamanız **roleInstance'ı** özel bir değere ayarlamak için SDK kullanıyorsa, varsayılan olarak, düğüm sayısını belirlemek için aynı değer kullanılır.
  * İstemci makinelerinden veya mobil cihazlardan çalışan bir uygulamayla yeni bir SDK sürümü kullanıyorsanız, düğüm sayısı büyük bir sayı döndürebilir (çok sayıda istemci makinesi veya mobil cihaz nedeniyle).

## <a name="automation"></a>Otomasyon

Azure Kaynak Yönetimi'ni kullanarak fiyatlandırma katmanını ayarlamak için bir komut dosyası yazabilirsiniz. [Nasıl olduğunu öğrenin](powershell.md#price).

## <a name="next-steps"></a>Sonraki adımlar

* [Örnekleme](../../azure-monitor/app/sampling.md)

[api]: app-insights-api-custom-events-metrics.md
[apiproperties]: app-insights-api-custom-events-metrics.md#properties
[start]: ../../azure-monitor/app/app-insights-overview.md
[pricing]: https://azure.microsoft.com/pricing/details/application-insights/
[pricing]: https://azure.microsoft.com/pricing/details/application-insights/