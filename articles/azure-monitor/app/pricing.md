---
title: Azure Application Insights kullanımı ve maliyetlerini yönetme | Microsoft Docs
description: Application Insights telemetri birimlerini yönetin ve maliyetleri izleyin.
services: application-insights
documentationcenter: ''
author: DaleKoetke
manager: carmonm
ms.assetid: ebd0d843-4780-4ff3-bc68-932aa44185f6
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.reviewer: mbullwin
ms.date: 08/13/2019
ms.author: dalek
ms.openlocfilehash: 4029a9e46b9c9bb7cbd677deff4a172d8fc982f8
ms.sourcegitcommit: 040abc24f031ac9d4d44dbdd832e5d99b34a8c61
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2019
ms.locfileid: "69534599"
---
# <a name="manage-usage-and-costs-for-application-insights"></a>Application Insights kullanım ve maliyetlerini yönetme

> [!NOTE]
> Bu makalede, veri kullanımı Application Insights nasıl analiz edileceği açıklanır.  İlgili bilgiler için aşağıdaki makalelere göz atın.
> - [Kullanım ve Tahmini maliyetler izleme](../../monitoring-and-diagnostics/monitoring-usage-and-estimated-costs.md) çoklu Azure İzleme özelliklerini farklı fiyatlandırma modelleri için tahmini maliyetleri ve kullanım görüntülemeyi açıklar. Ayrıca, uygulamanızın fiyatlandırma modelinin değiştirilmesi nasıl açıklar.

Fiyatlandırma Application Insights için nasıl çalıştığı hakkında sorularınız varsa forumumuza soru gönderebilirsiniz. [](https://social.msdn.microsoft.com/Forums/home?forum=ApplicationInsights)

## <a name="pricing-model"></a>Fiyatlandırma modeli

[Azure Application Insights][start] fiyatlandırması, alınan veri hacmine bağlıdır. Her Application Insights kaynak ayrı bir hizmet olarak ücretlendirilir ve Azure aboneliğiniz için faturaya katkıda bulunur.

### <a name="data-volume-details"></a>Veri hacmi ayrıntıları

* Veri birimi, Application Insights tarafından alınan telemetri bayt sayısıdır. Veri hacmi, uygulamanızdan Application Insights tarafından alınan sıkıştırılmamış JSON veri paketinin boyutu olarak ölçülür. [Analiz 'e aktarılan tablo verileri](https://docs.microsoft.com/azure/application-insights/app-insights-analytics-import)için veri hacmi, Application Insights gönderilen dosyaların sıkıştırılmamış boyutu olarak ölçülür.
* Uygulamanızın veri hacmi ücretleri artık 2018 Nisan itibariyle **veri** alımı adlı yeni bir faturalandırma ölçümü üzerinden raporlanır. Bu yeni ölçüm, uygulamalar öngörüleri ve Log Analytics gibi izleme teknolojileri arasında paylaşılır ve şu anda hizmet adı **Log Analytics**altındadır. 
* [Canlı ölçüm akışı](../../azure-monitor/app/live-stream.md) veriler fiyatlandırma amacıyla sayılmaz.

> [!NOTE]
> Bu makaledeki ekran görüntülerinde görüntülenen tüm fiyatlar yalnızca örnek amaçlıdır. Para birimi ve bölgenizin geçerli fiyatları için bkz. [Application Insights fiyatlandırması][pricing].

### <a name="multi-step-web-tests"></a>Çok adımlı web testleri

[Çok adımlı Web testleri](../../azure-monitor/app/availability-multistep.md) ek bir ücret doğurur. Çok adımlı Web testleri, bir dizi eylemi gerçekleştiren Web sınamalardır.

Tek bir sayfanın *ping testlerine* yönelik ayrı ücret alınmaz. Ping sınamalarından ve çok adımlı testlerin telemetrisi, uygulamanızdan diğer telemetri ile aynı şekilde ücretlendirilir.

## <a name="review-usage-and-estimate-costs"></a>Kullanımı ve tahmini maliyetleri gözden geçirin

Application Insights, en son kullanım desenlerine göre maliyetlerinizin ne kadar büyük bir süre içinde olduğunu anlamayı kolaylaştırır. Başlamak için, Azure portal Application Insights kaynak için **kullanım ve tahmini maliyetler** sayfasına gidin:

![Fiyatlandırma seçin](./media/pricing/pricing-001.png)

A. Veri hacminin ayı için gözden geçirin. Bu, sunucunuza ve istemci uygulamalarınıza ve kullanılabilirlik testlerinden alınan ve saklanan tüm verileri (herhangi bir [örnekleme](../../azure-monitor/app/sampling.md)sonrasında) içerir.  
B. [Çok adımlı Web testleri](../../azure-monitor/app/availability-multistep.md)için ayrı bir ücret yapılır. (Bu, veri hacmi ücretine dahil olan basit kullanılabilirlik testlerini içermez.)  
C. Son ay için veri hacmi eğilimlerini görüntüleyin.  
D. Veri alımı [örneklemesi](../../azure-monitor/app/sampling.md)etkinleştirin.   
E. Günlük veri hacmi ucunu ayarlayın.  

Application Insights kullanımınızı daha derin araştırmak için **ölçümler** sayfasını açın, "veri noktası birimi" adlı ölçümü ekleyin ve ardından verileri "Telemetri öğe türü" olarak bölmek Için *bölmeyi Uygula* seçeneğini belirleyin. 

Application Insights ücretleri Azure faturanızda eklenir. Azure faturanızın ayrıntılarını Azure portal veya [Azure Faturalandırma portalındaki](https://account.windowsazure.com/Subscriptions) **faturalandırma** bölümünde görebilirsiniz. 

![Sol menüde Faturalandırma ' i seçin.](./media/pricing/02-billing.png)

## <a name="data-rate"></a>Veri hızı
Göndereceğiniz verilerin hacmi üç şekilde sınırlıdır:

* **Örnekleme**: , Ölçümlerde en az deformasyon ile sunucunuz ve istemci uygulamalarından gönderilen telemetri miktarını azaltmak için örnekleme kullanabilirsiniz. Örnekleme, göndereceğiniz veri miktarını ayarlamak için kullanabileceğiniz birincil araçtır. [Örnekleme özellikleri](../../azure-monitor/app/sampling.md)hakkında daha fazla bilgi edinin. 
* **Günlük üst sınır**: Azure portal Application Insights kaynak oluşturduğunuzda, günlük üst sınır 100 GB/gün olarak ayarlanır. Visual Studio 'da bir Application Insights kaynağı oluşturduğunuzda, varsayılan değer küçüktür (yalnızca 32,3 MB/gün). Günlük uç varsayılan, testi kolaylaştırmak için ayarlanır. Kullanıcının uygulamayı üretime dağıtmadan önce günlük üst sınırı oluşturması amaçlanmıştır. 

    Yüksek trafikli bir uygulama için daha yüksek bir en yüksek değer istemediğiniz müddetçe en büyük sınır 1.000 GB/gün olur. 

    Günlük ucunu ayarlarken dikkatli kullanın. Amacınızı *hiçbir şekilde günlük tepesine vurmamanız*gerekir. Günlük üst sınıra ulaşırsanız, günün geri kalanı için verileri kaybeder ve uygulamanızı izleyemezsiniz. Günlük ucunu değiştirmek için, **günlük hacim Cap** seçeneğini kullanın. Bu seçeneğe **kullanım ve tahmini maliyetler** bölmesinde erişebilirsiniz (Bu, makalenin ilerleyen kısımlarında daha ayrıntılı olarak açıklanmıştır).
    Application Insights için kullanılamayacak kredi içeren bazı abonelik türlerinde kısıtlama kaldırdık. Daha önce, aboneliğin bir harcama limiti varsa, günlük sınır iletişim kutusunda harcama limitini kaldırma ve günlük ucunun 32,3 MB/gün dışında bir şekilde çıkarılması için yönergeler vardır.
* **Daraltma**: Daraltma, veri hızını saniyede 32.000 olay ile sınırlandırır, izleme anahtarı başına 1 dakikadan fazla.

*Uygulamamın azaltma oranını aşması durumunda ne olur?*

* Uygulamanızın gönderdiği veri hacmi her dakikada değerlendirilir. Dakika boyunca ortalama saniye başına oranı aşarsa, sunucu bazı istekleri reddeder. SDK verileri arabelleğe alır ve sonra yeniden göndermeyi dener. Birkaç dakika içinde bir aşırı gerilim yayın. Uygulamanız sürekli olarak verileri daraltma hızından daha fazla gönderirse, bazı veriler bırakılır. (ASP.NET, Java ve JavaScript SDK 'Ları verileri bu şekilde yeniden göndermeyi dener; diğer SDK 'lar yalnızca daraltılmış verileri de bırakabilir.) Daraltma gerçekleşirse, bunun oluştuğunu bildiren bir bildirim uyarısı görürsünüz.

*Nasıl yaparım? Uygulamamın ne kadar veri gönderdiğini öğrenmek mi istiyorsunuz?*

Uygulamanızın ne kadar veri gönderdiğini görmek için aşağıdaki seçeneklerden birini kullanabilirsiniz:

* Günlük veri hacmi grafiğini görmek için **kullanım ve tahmini maliyet** bölmesine gidin. 
* Ölçüm Gezgini ' de yeni bir grafik ekleyin. Grafik ölçümü için, **veri noktası birimi**' ni seçin. **Gruplamayı**açın ve **veri türüne**göre gruplandırın.

## <a name="reduce-your-data-rate"></a>Veri Hızınızı azaltın
Veri haciminizi azaltmak için yapabileceğiniz bazı şeyler aşağıda verilmiştir:

* [Örnekleme](../../azure-monitor/app/sampling.md)kullanın. Bu teknoloji, ölçümlerinizi azaltmadan veri hızınızı azaltır. Aramada ilgili öğeler arasında gezinme özelliğini kaybetmezsiniz. Sunucu uygulamalarında örnekleme otomatik olarak çalışır.
* Her sayfa görünümünde [bildirilemeyen Ajax çağrısı sayısını sınırlayın](../../azure-monitor/app/javascript.md#configuration) veya Ajax raporlamayı devre dışı bırakabilirsiniz.
* Gerekli olmayan koleksiyon modüllerini kapatmak için [ApplicationInsights. config dosyasını düzenleyin](../../azure-monitor/app/configuration-with-applicationinsights-config.md) . Örneğin, performans sayaçları veya bağımlılık verilerinin yoksayılması olduğuna karar verebilirsiniz.
* Telemetrinizi ayrı izleme anahtarları arasında ayırın. 
* Ön toplama ölçümleri. Uygulamanızda TrackMetric çağrısı yaparsanız, bir ölçüm toplu işleminin ortalama ve standart sapması hesaplamasını kabul eden aşırı yüklemeyi kullanarak trafiği azaltabilirsiniz. Ya da, [önceden toplama paketi](https://www.myget.org/gallery/applicationinsights-sdk-labs)de kullanabilirsiniz.

## <a name="manage-the-maximum-daily-data-volume"></a>Maksimum günlük veri hacmini yönetme

Toplanan verileri sınırlandırmak için günlük hacim ucunu kullanabilirsiniz. Ancak, sınır karşılanıyorsa günün geri kalanı için uygulamanızdan gönderilen tüm Telemetriyi bir kayıp olur. Uygulamanızın günlük tepesine isabet etmek *önerilmez* . Günlük üst sınıra ulaştıktan sonra uygulamanızın sistem durumunu ve performansını izleyemezsiniz.

Günlük birim Cap 'i kullanmak yerine, veri hacmi istediğiniz düzeye ayarlamak için [örnekleme](../../azure-monitor/app/sampling.md) 'yı kullanın. Daha sonra, uygulamanızın çok daha yüksek sayıda telemetri göndermek için beklenmedik şekilde başlaması durumunda günlük ucunu yalnızca "son çare" olarak kullanın.

Günlük ucunu değiştirmek için, Application Insights kaynağınızın **Yapılandır** bölümünde, **kullanım ve tahmini maliyetler** bölmesinde **günlük üst sınır**' ı seçin.

![Günlük telemetri birimi ucunu ayarla](./media/pricing/pricing-003.png)

## <a name="sampling"></a>Örnekleme
[Örnekleme](../../azure-monitor/app/sampling.md) , tanılama aramaları sırasında ilgili olayları bulma özelliğini korurken, telemetrinin uygulamanıza gönderilme hızını azaltma yöntemidir. Ayrıca, doğru olay sayılarını da koruyabilirsiniz.

Örnekleme, ücretleri azaltmak ve aylık kotasında kalmak için etkili bir yoldur. Örnekleme algoritması ilgili telemetri öğelerini korur, örneğin, ara 'yı kullandığınızda belirli bir özel durumla ilgili isteği bulabilirsiniz. Algoritma Ayrıca, istek hızları, özel durum ücretleri ve diğer sayımlar için ölçüm Gezgininde doğru değerleri görmeniz için doğru sayıları de korur.

Birçok örnekleme biçimi vardır.

* [Uyarlamalı örnekleme](../../azure-monitor/app/sampling.md) , ASP.NET SDK için varsayılandır. Uyarlamalı örnekleme, uygulamanızın gönderdiği telemetri hacmine otomatik olarak ayarlanır. Ağ üzerindeki telemetri trafiğinin azaltılabilmesi için Web uygulamanızdaki SDK 'da otomatik olarak çalışır. 
* Alım *örnekleme* , uygulamanızdaki Telemetriyi Application Insights hizmetine girdiği noktada çalışan bir alternatiftir. Alım örnekleme, uygulamanızdan gönderilen telemetri hacmini etkilemez, ancak hizmet tarafından tutulan hacmi azaltır. Tarayıcıların ve diğer SDK 'lardan telemetri tarafından kullanılan kotayı azaltmak için alma örneklemesini kullanabilirsiniz.

Alım örneklemesini ayarlamak için **fiyatlandırma** bölmesine gidin:

![Kota ve fiyatlandırma bölmesinde örnekler kutucuğunu seçin ve ardından bir örnekleme kesri seçin](./media/pricing/pricing-004.png)

> [!WARNING]
> **Veri örnekleme** bölmesi yalnızca alma örneklemenin değerini denetler. Uygulamanızda Application Insights SDK tarafından uygulanan örnekleme oranını yansıtmaz. Gelen telemetri SDK 'da zaten örneklenir, Alım örnekleme uygulanmaz.
>

Gerçek örnekleme oranını öğrenmek için, nereye uygulandığını fark etmeksizin bir [analiz sorgusu](analytics.md)kullanın. Sorgu şöyle görünür:

    requests | where timestamp > ago(1d)
    | summarize 100/avg(itemCount) by bin(timestamp, 1h)
    | render areachart

Her bir tutulan kaydında, `itemCount` temsil ettiği özgün kayıt sayısını belirtir. 1 + önceki atılan kayıtların sayısına eşittir. 

## <a name="change-the-data-retention-period"></a>Veri saklama süresini değiştirme

Application Insights artık değişken bekletme önizlememiz için sınırlı sayıda Application Insights müşteri ekleme. Bu Önizleme programına katılma bilgilerine [buradan](https://feedback.azure.com/forums/357324-azure-monitor-application-insights/suggestions/17454031)ulaşabilirsiniz.

Application Insights kaynakları için varsayılan saklama 90 gündür. Her bir Application Insights kaynağı için farklı saklama dönemleri seçilebilir. Kullanılabilir saklama dönemlerinin tam kümesi 30, 60, 120, 180, 270, 365, 550 veya 730 günleridir. 

Faturalandırma, daha uzun süre için etkinleştirildiğinde, 90 günden daha uzun süre tutulan veriler, Azure Log Analytics veri saklama için şu anda faturalandırılan ücret üzerinden faturalandırılır. [Azure Izleyici fiyatlandırma sayfasında](https://azure.microsoft.com/pricing/details/monitor/)daha fazla bilgi edinin.  [Bu öneri için oylama](https://feedback.azure.com/forums/357324-azure-monitor-application-insights/suggestions/17454031)ile değişken bekletme ilerlemesini güncel tutun. 

## <a name="limits-summary"></a>Limit Özeti

[!INCLUDE [application-insights-limits](../../../includes/application-insights-limits.md)]

## <a name="disable-daily-cap-e-mails"></a>Günlük uç e-postalarını devreden çıkar

Günlük birim Cap e-postalarını devre dışı bırakmak için, Application Insights kaynağınızın **Yapılandır** bölümünde, **kullanım ve tahmini maliyetler** bölmesinde **günlük üst sınır**' ı seçin. Büyük/veya ayarlanabilir bir uyarı düzeyine ulaşıldığında e-posta gönderme ayarları vardır. Tüm günlük uç hacimlerle ilgili e-postaları devre dışı bırakmak isterseniz her iki kutunun işaretini kaldırın.

## <a name="legacy-enterprise-per-node-pricing-tier"></a>Eski Kurumsal (düğüm başına) Fiyatlandırma Katmanı

Azure Application Insights 'in erken benimseme için, hala iki olası fiyatlandırma katmanı vardır: Temel ve kurumsal. Temel fiyatlandırma katmanı yukarıda açıklananla aynıdır ve varsayılan katmandır. Ek bir ücret ödemeden tüm kurumsal katman özelliklerini içerir. Temel katman, birincil olarak alınan verilerin hacmi üzerinde yer alır. 

> [!NOTE]
> Bu eski fiyatlandırma katmanları yeniden adlandırıldı. Kurumsal fiyatlandırma katmanı artık **düğüm başına** çağrılır ve temel fiyatlandırma KATMANı artık **GB başına**çağırılır. Bu yeni adlar aşağıda ve Azure portal kullanılır.  

Düğüm başına (eskiden Enterprise) katmanının düğüm başına ücreti vardır ve her düğüm günlük veri indirimi alır. Düğüm başına fiyatlandırma katmanında, dahil edilen indirimin üzerinde alınan veriler için ücretlendirilirsiniz. Operations Management Suite kullanıyorsanız düğüm başına katmanını seçmeniz gerekir. 

Para birimi ve bölgenizin geçerli fiyatları için bkz. [Application Insights fiyatlandırması](https://azure.microsoft.com/pricing/details/application-insights/).

> [!NOTE]
> 2018 Nisan 'da, Azure [](https://azure.microsoft.com/blog/introducing-a-new-way-to-purchase-azure-monitoring-services/) izleme için yeni bir fiyatlandırma modeli sunuyoruz. Bu model, izleme hizmetlerinin tam portföyüne ilişkin basit bir "Kullandıkça öde" modeli benimsemektedir. [Yeni fiyatlandırma modeli](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-usage-and-estimated-costs)hakkında daha fazla bilgi edinin, kullanım modellerinize bağlı olarak [Bu modele geçme etkisini](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-usage-and-estimated-costs#assessing-the-impact-of-the-new-pricing-model) nasıl değerlendirirsiniz ve [yeni modeli nasıl kabul](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-usage-and-estimated-costs#moving-to-the-new-pricing-model) edebilirsiniz

### <a name="per-node-tier-and-operations-management-suite-subscription-entitlements"></a>Düğüm başına katman ve Operations Management Suite abonelik yetkilendirmeleri

Operations Management Suite E1 ve E2 satın alan müşteriler, düğüm başına Application Insights [daha önce duyurulan](https://blogs.technet.microsoft.com/msoms/2017/05/19/azure-application-insights-enterprise-as-part-of-operations-management-suite-subscription/)gibi ek ücret ödemeden ek bir bileşen olarak alabilir. Özellikle, her bir Operations Management Suite E1 ve E2 birimi düğüm katmanı başına Application Insights bir düğüme yönelik bir yetkilendirme içerir. Her Application Insights düğüm, hiçbir ek ücret ödemeden, 90 günlük veri saklama ile günde en fazla 200 MB veri alımı (Log Analytics veri alma işleminden ayrı) içerir. Katman, makalenin ilerleyen bölümlerinde daha ayrıntılı olarak açıklanmıştır. 

Bu katman yalnızca Operations Management Suite aboneliği olan müşterilere uygulanabilir olduğundan, Operations Management Suite aboneliğine sahip olmayan müşteriler bu katmanı seçme seçeneği görmez.

> [!NOTE]
> Bu yetkilendirmesini aldığınızdan emin olmak için Application Insights kaynaklarınızın, düğüm başına fiyatlandırma katmanında olması gerekir. Bu yetkilendirme yalnızca düğüm olarak geçerlidir. GB başına katmanda Application Insights kaynak, hiçbir avantaj sunmaz. Bu yetkilendirme, **kullanım ve tahmini maliyet** bölmesinde gösterilen tahmini maliyetlerde görünür değildir. Ayrıca, bir aboneliği, 2018 Nisan 'da yeni Azure izleme fiyatlandırma modeline taşırsanız, kullanılabilir tek katmandır. Bir Operations Management Suite aboneliğiniz varsa, aboneliğin yeni Azure izleme fiyatlandırma modeline taşınması önerilmez.

### <a name="how-the-per-node-tier-works"></a>Düğüm başına katmanın çalışma şekli

* Düğüm başına katmandaki tüm uygulamalar için telemetri gönderen her düğüm için ödeme yaparsınız.
  * *Düğüm* , uygulamanızı barındıran bir fiziksel veya sanal sunucu makinesi veya hizmet olarak platform bir rol örneğidir.
  * Geliştirme makineleri, istemci tarayıcıları ve mobil cihazlar düğüm olarak sayılmaz.
  * Uygulamanızda, Web hizmeti ve arka uç çalışanı gibi telemetri gönderen birkaç bileşen varsa, bileşenler ayrı olarak sayılır.
  * [Canlı ölçüm akışı](../../azure-monitor/app/live-stream.md) veriler fiyatlandırma amacıyla sayılmaz. Bir abonelikte, ücretleriniz uygulama başına değil düğüm başına alınır. 12 uygulama için telemetri gönderen beş düğümünüz varsa, ücret beş düğüm için yapılır.
* Ücretler ayda tırnak içine alınmış olsa da, yalnızca bir düğümün bir uygulamadan telemetri gönderdiği saatler için ücretlendirilirsiniz. Saatlik ücret, 744 ile bölünmüş, saatlik olarak aylık ücretlendirilir (31 günlük aydaki saat sayısı).
* Algılanan her düğüm için günde 200 MB 'lık bir veri birimi ayırması verilir (saatlik ayrıntı düzeyi ile). Kullanılmayan veri ayırma bir günden sonrakine taşınmaz.
  * Düğüm başına fiyatlandırma katmanını seçerseniz, her abonelik, Bu abonelikteki Application Insights kaynaklarına telemetri gönderen düğüm sayısına bağlı olarak günlük bir veri tahsisatı alır. Bu nedenle, her gün veri gönderen beş düğümünüz varsa, Bu abonelikteki tüm Application Insights kaynaklarına 1 GB 'lik bir havuz uygulanmış olması gerekir. Dahil edilen veriler tüm düğümlerde paylaşıldığından, bazı düğümlerin diğer düğümlere kıyasla daha fazla veri gönderemediği kesin değildir. Belirli bir gün içinde Application Insights kaynakları, bu abonelik için günlük veri ayırmaya dahil olandan daha fazla veri alır, GB başına fazla kullanım ücreti uygulanır. 
  * Günlük veri tahsisatı, her bir düğümün Telemetriyi 200 MB ile ayırarak, gün içindeki saat sayısı (UTC kullanılarak) olarak hesaplanır. Bu nedenle, günde 24 saat boyunca telemetri gönderen dört düğümünüz varsa, söz konusu gün için dahil edilen veri ((4 &#215; 15)/24) &#215; 200 MB = 500 MB olur. Veriler fazla kullanım için GB başına 2,30 ABD Doları tutarında, düğümler günde 1 GB veri gönderse, ücret 1,15 ABD doları olur.
  * Düğüm başına katmanı günlük indirimi, GB başına katmanı seçtiğiniz uygulamalarla paylaşılmaz. Kullanılmayan kesinti, günde bir günden fazla taşınmaz. 

### <a name="examples-of-how-to-determine-distinct-node-count"></a>Farklı düğüm sayısını belirleme örnekleri

| Senaryo                               | Günlük toplam düğüm sayısı |
|:---------------------------------------|:----------------:|
| 3 Azure App Service örnekleri ve 1 sanal sunucu kullanan 1 uygulama | 4 |
| 2 VM 'de çalışan 3 uygulama; Bu uygulamalar için Application Insights kaynakları aynı abonelikte ve düğüm başına katmanda bulunur | 2 | 
| Uygulama öngörüleri kaynakları aynı abonelikte olan 4 uygulama; 2 örnek çalıştıran her uygulama 16 yoğun saatler sırasında 2 örnek çalıştıran her uygulama ve 8 yoğun saat boyunca 4 örnek | 13.33 | 
| 1 çalışan rolü ve 1 Web rolü olan bulut Hizmetleri, her biri 2 örnek çalıştırıyor | 4 | 
| 50 mikro hizmet çalıştıran 5 düğümlü Azure Service Fabric kümesi; 3 örnek çalıştıran her bir mikro hizmet | 5|

* Kesin düğüm sayma, uygulamanızın kullandığı Application Insights SDK 'sına bağlıdır. 
  * SDK 2,2 ve sonraki sürümlerinde, hem Application Insights [Core SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights/) hem de [Web SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/) 'sı her uygulama konağını bir düğüm olarak bildirir. Örnek olarak, fiziksel sunucu ve VM konakları için bilgisayar adı veya bulut hizmetleri için örnek adı verilebilir.  Tek özel durum yalnızca [.NET Core](https://dotnet.github.io/) ve APPLICATION INSIGHTS Core SDK kullanan bir uygulamadır. Bu durumda, konak adı kullanılamadığından tüm konaklar için yalnızca bir düğüm raporlanır. 
  * SDK 'nın önceki sürümlerinde [Web SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/) , daha yeni SDK sürümleri gibi davranır, ancak [çekirdek SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights/) , uygulama ana bilgisayarlarının sayısından bağımsız olarak yalnızca bir düğüm bildirir. 
  * Uygulamanız **Roleınstance** 'ı özel bir değere ayarlamak için SDK kullanıyorsa, varsayılan olarak, düğüm sayısını belirlemede aynı değer kullanılır. 
  * İstemci makinelerinden veya mobil cihazlardan çalışan bir uygulamayla yeni bir SDK sürümü kullanıyorsanız, düğüm sayısı çok büyük bir sayı döndürebilir (çok sayıda istemci makinesi veya mobil cihaz nedeniyle). 

## <a name="automation"></a>Otomasyon

Fiyatlandırma katmanını Azure Kaynak Yönetimi 'ni kullanarak ayarlamak için bir komut dosyası yazabilirsiniz. [Nasıl olduğunu öğrenin](powershell.md#price).


## <a name="next-steps"></a>Sonraki adımlar

* [Örnekleme](../../azure-monitor/app/sampling.md)

[api]: app-insights-api-custom-events-metrics.md
[apiproperties]: app-insights-api-custom-events-metrics.md#properties
[start]: ../../azure-monitor/app/app-insights-overview.md
[pricing]: https://azure.microsoft.com/pricing/details/application-insights/