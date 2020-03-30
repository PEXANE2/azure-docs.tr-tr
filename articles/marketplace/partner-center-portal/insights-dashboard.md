---
title: İş Ortağı Merkezi'nde Ticari Pazar analizinde Market Öngörüleri panosu
description: Yayıncıların AppSource ve Azure Marketi'ndeki müşteri etkileşimini ölçmelerine olanak tanıyan pazar yeri web analitiğinin bir özetine erişin.
author: dsindona
ms.author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 12/16/2019
ms.openlocfilehash: a547ced9df98298361360ecab88036599cd86027
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80275858"
---
# <a name="marketplace-insights-dashboard-in-commercial-marketplace-analytics"></a>Ticari Pazar analizinde Market Öngörüleri panosu

Bu makale, İş Ortağı Merkezi'ndeki Market İstatistikleri panosu hakkında bilgi sağlar. Bu pano, yayıncıların pazar vitrinlerinde listelenen ilgili ürün ayrıntı sayfaları olan AppSource ve Azure Marketi için müşteri etkileşimini ölçmelerine olanak tanıyan pazar daki web analitiğinin bir özetini görüntüler.

## <a name="marketplace-insights-dashboard"></a>Market İçgörüleri panosu

İş Ortağı Merkezi'ndeki **Market İstatistikleri panosuna** erişmek için Commercial Marketplace altında **[Analiz sekmesini](https://partner.microsoft.com/dashboard/commercial-marketplace/analytics/summary)** açın.

Aşağıdaki öğelerin grafik gösterimlerini görüntüleyebilirsiniz:  

- [Marketplace Insights özeti](#marketplace-insights-summary)
- [Coğrafyaya göre sayfa ziyaretleri](#page-visits-by-geography)  
- [Sayfa ziyaretleri ve benzersiz ziyaretçi eğilimi](#page-visits-versus-unique-visitors-trend)
- [Tok'lu benzersiz ziyaretçilere karşı harekete geçme çağrısı](#call-to-action-versus-unique-visitors-with-ctas)
- [Teklife göre sayfa ziyaretleri ve eylem çağrısı](#page-visits-and-calls-to-action-by-offers)
- [Eylem yüzdesi eğilimini arama](#call-to-action-percentage-trend)
- [Başvuru etki alanları tarafından sayfa ziyaretleri ve eylem çağrıları](#page-visits-and-calls-to-action-by-referral-domains)
- [Marketplace Insights ayrıntılar tablosu](#marketplace-insights-details-table)

>[!NOTE]
> Analitik terminolojinin ayrıntılı tanımları [için, Ticari Pazar analitiği için sık sorulan sorular ve terminolojiye](./faq-terminology.md)bakın.

### <a name="insights-dashboard-layout"></a>İstatistikler pano düzeni

Market ölçümlerini çeşitli şekillerde görüntüleyebilirsiniz:

- Storefront sekmeleri
- Sayfa filtreleri
- Tarih filtreleri

**Storefront sekmeleri**: AppSource & Azure Marketi sekmeleri aracılığıyla tekliflerinizin ölçümlerini ayrı ayrı görüntüleyebilirsiniz. Seçili teklif(ler) için ölçümlerin görselleştirilmesini görmek için sağdaki teklif açılır listesinden teklif(ler)'i seçin. Varsayılan olarak, tüm teklifler seçilir.

![İş Ortağı Merkezi Öngörüleri panosu açılır liste teklifi](./media/insights-offer-dropdown.png)

**Öngörüler sayfa filtreleri**: Bu filtreler teklif (ürün ayrıntı sayfası) düzeyinde uygulanır. Görüntülemek istediğiniz ölçütler için birden çok filtre seçebilirsiniz. Bu filtre, grafikler ve ayrıntılar da dahil olmak üzere tüm Market İstatistikleri bölümü için geçerlidir.

![İş Ortağı Merkezi Öngörüleri pano sayfası filtreleri](./media/insights-page-filter.png)

- Teklif Adları yalnızca seçilen tarih aralığında sayfa ziyaretleri olan teklifler için listelenir.  
- Varsayılan seçim, filtre seçeneklerinin her biri için 'Tümü'
- Uygulanan filtreler, yapılan seçimler için seçim sayısını gösterir. Uygulanan filtreler varsayılan 'Tümü' seçimi için görüntülenmez.

![İş Ortağı Merkezi Öngörüleri uygulanan filtreler](./media/insights-page-filter-two.png)

**İstatistikler tarih filtreleri**: Bu filtre, Tüm Market İstatistikleri bölümü için geçerlidir. Filtreler önceden belirlenmiş tarih aralıkları veya özel bir tarih aralığı içerebilir.

![İş Ortağı Merkezi Öngörüleri tarih filtreleri](./media/insights-date-range.png)

## <a name="marketplace-insights-summary"></a>Marketplace Insights özeti

Pazar yeri öngörüleri özeti **bölümünde, seçilen**tarih aralığı için Sayfa ziyaretleri, **eylem çağrıları**ve Benzersiz **ziyaretçiler** in sayısı görüntülenir.

### <a name="page-visits"></a>Sayfa ziyaretleri

Bu sayı, seçili bir tarih aralığı için teklif sayfasındaki (ürün ayrıntı sayfası) farklı kullanıcı oturumlarının sayısını temsil eder. Kırmızı/yeşil yüzde göstergesi, sayfa ziyaretlerinin büyüme %'sini temsil eder. Eğilim grafiği, sayfa ziyaretlerinin aydan aya sayısını gösterir.

### <a name="unique-visitors"></a>Benzersiz ziyaretçiler

Bu sayı, sayfa filtresinde seçilen teklif(ler) için seçili tarih aralığındaki farklı ziyaretçi sayısını temsil eder. Bir veya daha fazla ürün ayrıntı sayfasını ziyaret eden bir ziyaretçi tek bir ziyaretçi olarak sayılır.

### <a name="call-to-action"></a>Harekete geçme çağrısı

Bu numara, teklif sayfasında tamamlanan **Eyleme Çağrı** düğmesinin sayısını temsil eder (ürün ayrıntı sayfası). **Şimdi** **Al**, **Ücretsiz deneme**, Bana **Ulaşın**ve **Test drive** düğmeleri seçildiğinde eylem çağrıları sayılır.

![İş Ortağı Merkezi Öngörüleri eylem özeti çağrısı](./media/insights-summary.png)

## <a name="page-visits-by-geography"></a>Coğrafyaya göre sayfa ziyaretleri

Aşağıdaki ısı haritası Sayfa **ziyaretlerinin**sayısını, **eylem çağrılarını**ve müşteri **ülkesine göre Benzersiz ziyaretçileri**görüntüler. Daha yüksek sayfa ziyaretleri daha koyu harita renkleriyle, alt sayfa ziyaretleri ise daha açık harita renkleriyle temsil edilir.

![İş Ortağı Merkezi Öngörüleri coğrafi dağılımı](./media/insights-geography.png)

Isı haritası aşağıdaki yetenekleri içerir:

- Isı **haritası, Sayfa ziyaretlerinin**ayrıntılarını, eylem **çağrılarının** ve belirli bir konumdaki **Benzersiz ziyaretçilerin** ayrıntılarını görüntülemek için tamamlayıcı bir ızgaraya sahiptir; tercih edilirse belirli bir konuma yakınlaştırabilirsiniz.  
- **Yayılan ülkeler,** müşterilerinizin seçilen tarih aralığında sayfa ziyaretlerini bildirdiği tüm ülkelerin sayısıdır.
- Haritadaki konumu yakınlaştırmak için ızgarada bir ülkeyi arayabilir ve seçebilirsiniz. Haritada **Ana Sayfa'yı** seçerek orijinal görünüme geri dön.

## <a name="page-visits-versus-unique-visitors-trend"></a>Sayfa ziyaretleri ve benzersiz ziyaretçi eğilimi

Aşağıdaki sütunlar, Y ekseninde (grafiğin sol tarafındaki eksen) görüntülenen aylık sayfa ziyaretlerinin sayısını temsil eder. Trend çizgisi, vitrinlerde yayınlanan teklifleriniz için ikincil Y Ekseni'nde (grafiğin sağ tarafındaki eksen) görüntülenen benzersiz ziyaretçilerin aylık eğilimini temsil eder: Azure Marketi ve AppSource.

![İş Ortağı Merkezi Öngörüleri sayfa ziyaretleri ve benzersiz ziyaretçi trendi](./media/insights-page-vists-unique-visitors.png)

## <a name="call-to-action-versus-unique-visitors-with-ctas"></a>Tok'lu benzersiz ziyaretçilere karşı harekete geçme çağrısı

Yığılmış sütunlar, CTA türlerine göre ayrılmış olan aylık eylem çağrılarını (CTA) temsil eder **(Şimdi alın**, **Bana Ulaşın**ve **Ücretsiz Deneme**) ve Y ekseni üzerinde çizilir (sayfanın sol tarafındaki eksen). Eğilim çizgisi, Azure Marketi ve AppSource'da yayınlanan teklifleriniz için ikincil Y Ekseni'nde (grafiğin sağ tarafındaki eksen) görüntülenen CTA'lı benzersiz ziyaretçilerin aylık eğilimini temsil eder.

![İş Ortağı Merkezi Öngörüleri, CTA'lı benzersiz ziyaretçilere karşı harekete geçme çağrısı](./media/insights-call-to-action-unique-visitors.png)

## <a name="page-visits-and-calls-to-action-by-offers"></a>Teklife göre sayfa ziyaretleri ve eylem çağrıları

Dış pasta grafiği, pazarda yayınladığınız ve filtrede seçtiğiniz tekliflere dayalı **olarak Sayfa ziyaretlerinin** dökümünü temsil eder. İç grafik, aynı teklifler için eylem dökümü **Çağrıları'nı** temsil eder.

![İş Ortağı Merkezi Öngörüleri sayfa ziyaretleri ve tekliflere göre eylem çağrıları](./media/insights-page-visits-and-cta-by-offer.png)

## <a name="call-to-action-percentage-trend"></a>Eylem yüzdesi eğilimini arama

**Eylem yüzdesi çağrısı eğilimi,** pazarda yayınlanan teklifler için CTA yüzdesini sunar. CTA % = (CTAs/ sayfa ziyaretleri) * 100.

![İş Ortağı Merkezi Öngörüleri eylem yüzdesi eğilimine çağrı](./media/insights-call-to-action-percentage-trend.png)

## <a name="page-visits-and-calls-to-action-by-referral-domains"></a>Başvuru etki alanları tarafından sayfa ziyaretleri ve eylem çağrıları

Aşağıdaki grafikte en iyi 50 başvuru etki alanı yer almaktadır. Belirli bir yönlendirme etki alanı nın seçilmesi, sayfa ziyaretlerinin aylık eğilimini ve sağda grafikte eylem çağrıları gösterir.

![İş Ortağı Merkezi Öngörüleri sayfa ziyaretleri ve yönlendirme etki alanları ve kampanyalar tarafından eylem çağrıları](./media/insights-page-visits-call-to-actions.png)

## <a name="marketplace-insights-details-table"></a>Marketplace Insights ayrıntılar tablosu

Bu tablo, sayfa ziyaretlerinin ve seçili tekliflerinizin tarihe göre sıralanmış eylem çağrılarının bir listesini sağlar.

![İş Ortağı Merkezi Öngörüleri ayrıntılar tablosu](./media/insights-details-page.png)

- Kayıt sayısı 1000'den küçükse veriler bir CSV dosyasına ayıklanabilir.
- Kayıt sayısı 1000'in üzerindeyse, dışa aktarılan veriler, önümüzdeki 30 gün boyunca bir indirme sayfasına eşit olarak yerleştirilir.
- Filtreler, ilgilendiğiniz verileri görüntülemek için uygulanabilir. Veriler Teklif adlarıyla ve Kampanya adlarıyla filtrelenebilir.  

## <a name="next-steps"></a>Sonraki adımlar

- İş Ortağı Merkezi Ticari Pazarı'nda bulunan analiz raporlarına genel bir bakış için, [İş Ortağı Merkezi'ndeki Ticari Pazar Için Analitik bölümüne](./analytics.md)bakın.
- Teklifiniz için pazar daki etkinliği özetleyen toplam verilerin grafikleri, eğilimleri ve değerleri için, [Ticari Pazar analitiğinde Özet Panosu'na](./summary-dashboard.md)bakın.
- Siparişleriniz hakkında grafiksel ve indirilebilir bir biçimde bilgi için, [Ticari Pazar analitiğinde Siparişler Panosu'na](./orders-dashboard.md)bakın.
- Sanal Makine (VM) kullanım ve ölçülü fatura ölçümleri için, [Ticari Pazar analitiğinde Kullanım Panosu'na](./usage-dashboard.md)bakın.
- Büyüme eğilimleri de dahil olmak üzere müşterileriniz hakkında ayrıntılı bilgi için [Ticari Pazar analitiğinde Müşteri Panosu'na](./customer-dashboard.md)bakın.
- Son 30 gün içinde indirme isteklerinizin bir listesi için, [Ticari Pazar analizinde İndirme Panosu'na](./downloads-dashboard.md)bakın.
- Azure Marketi ve AppSource'daki teklifler için müşteri geri bildirimlerinin konsolide görünümünü görmek için, [Commercial Marketplace analitiğinde Derecelendirmeler ve incelemeler panosuna](./ratings-reviews.md)bakın.
- Ticari Pazar yeri analitiği hakkında sık sorulan sorular ve kapsamlı bir veri terimleri sözlüğü [için, Ticari Pazar analitiği için sık sorulan sorular ve terminolojiye](./faq-terminology.md)bakın.
