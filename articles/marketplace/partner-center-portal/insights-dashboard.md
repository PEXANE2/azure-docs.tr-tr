---
title: Market öngörüleri-Microsoft ticari Market
description: Microsoft AppSource ve Azure Marketi 'nde müşteri katılımını ölçmenizi sağlayan Market Web Analytics özetine erişin.
author: dsindona
ms.author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 12/16/2019
ms.openlocfilehash: 2d9691279b987f9d45f755ab340ff70671d350c0
ms.sourcegitcommit: 493b27fbfd7917c3823a1e4c313d07331d1b732f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2020
ms.locfileid: "83745666"
---
# <a name="marketplace-insights-dashboard-in-partner-center"></a>Iş Ortağı Merkezi 'nde Market öngörüleri panosu

Bu makalede, Iş Ortağı Merkezi 'nde Market öngörüleri panosu hakkında bilgi sağlanmaktadır. Bu pano Market Web Analytics 'in bir özetini görüntüler. Bu, yayımcıların Market storeları: Microsoft AppSource ve Azure Market 'te listelenen ilgili ürün ayrıntı sayfaları için müşteri etkileşimini ölçmesine olanak sağlar.

## <a name="marketplace-insights-dashboard"></a>Market İçgörüleri panosu

**Market Insights panosuna** Iş Ortağı Merkezi 'ne erişmek Için, ticari Market altında **[Çözümle sekmesini](https://partner.microsoft.com/dashboard/commercial-marketplace/analytics/summary)** açın.

Aşağıdaki öğelerin grafik sunumlarını görüntüleyebilirsiniz:  

- [Market öngörüleri Özeti](#marketplace-insights-summary)
- [Coğrafya tarafından sayfa ziyaretleri](#page-visits-by-geography)  
- [Sayfa ziyaretleri ve benzersiz ziyaretçi eğilimi karşılaştırması](#page-visits-versus-unique-visitors-trend)
- [CTAs ile eyleme karşı benzersiz ziyaretçilere çağrı](#call-to-action-versus-unique-visitors-with-ctas)
- [Sayfa ziyaretleri ve tekliflere göre eyleme çağrı](#page-visits-and-calls-to-action-by-offers)
- [Eylem yüzdesi eğilimi çağrısı](#call-to-action-percentage-trend)
- [Sayfa ziyaretleri ve başvuru etki alanlarına göre eyleme çağrılar](#page-visits-and-calls-to-action-by-referral-domains)
- [Market öngörüleri Ayrıntıları tablosu](#marketplace-insights-details-table)

>[!NOTE]
> Analiz terminolojisi hakkında ayrıntılı tanımlamalar için bkz. [ticari Market Analizi Için sık sorulan sorular ve terminoloji](./faq-terminology.md).

### <a name="insights-dashboard-layout"></a>Öngörüler Pano düzeni

Market ölçümlerini çeşitli yollarla görüntüleyebilirsiniz:

- Storefront sekmeleri
- Sayfa filtreleri
- Tarih filtreleri

**Storefront sekmeleri**: Azure Marketi sekmeleri appsource & aracılığıyla tekliflerinizin ölçümlerini ayrı olarak görebilirsiniz. Seçili tekliflerle ilgili ölçümlerin bir görselliğini görmek için sağdaki teklif açılan listesinden teklif (ler) i seçin. Varsayılan olarak, tüm teklifler seçilidir.

![İş Ortağı Merkezi Öngörüler panosu teklif açılan listesi](./media/insights-offer-dropdown.png)

**Öngörüler sayfa filtreleri**: Bu filtreler teklif (Ürün Ayrıntısı sayfası) düzeyinde uygulanır. Görüntülemek istediğiniz ölçüt için birden çok filtre seçebilirsiniz. Bu filtre, grafikler ve ayrıntılar dahil olmak üzere tüm Market öngörüleri için geçerlidir.

![İş Ortağı Merkezi öngörüleri pano sayfası filtreleri](./media/insights-page-filter.png)

- Teklif adları yalnızca seçili tarih aralığında sayfa ziyaret eden teklifler için listelenir.  
- Filtre seçeneklerinin her biri için varsayılan seçim ' All ' olur
- Uygulanan filtreler, yapılan seçimler için seçim sayısını gösterir. Uygulanan filtreler varsayılan ' tümü ' seçimi için görüntülenmeyecek.

![İş Ortağı Merkezi öngörüleri filtreleri uygulandı](./media/insights-page-filter-two.png)

**Öngörüler tarih filtreleri**: Bu filtre tüm Market öngörüleri bölümüne uygulanır. Filtreler, önceden belirlenmiş tarih aralıkları veya özel bir tarih aralığı içerebilir.

![İş Ortağı Merkezi öngörüleri tarih filtreleri](./media/insights-date-range.png)

## <a name="marketplace-insights-summary"></a>Market öngörüleri Özeti

Market öngörüleri Özeti bölümünde, seçilen tarih aralığı için **sayfa ziyaretlerinin**sayısı, **eyleme yapılan çağrılar**ve **benzersiz ziyaretçiler** bulunur.

### <a name="page-visits"></a>Sayfa ziyaretleri

Bu sayı, seçilen bir tarih aralığı için teklif sayfasındaki (Ürün Ayrıntısı sayfası) farklı kullanıcı oturumlarının sayısını temsil eder. Kırmızı/yeşil yüzde göstergesi, sayfa ziyaretlerinin% büyümesini temsil eder. Eğilim grafiği, sayfa ziyaretlerinin aya göre ayı sayısını temsil eder.

### <a name="unique-visitors"></a>Benzersiz ziyaretçiler

Bu sayı, sayfa filtresinde seçilen teklif (ler) i için seçilen tarih aralığı boyunca ayrı ziyaretçi sayısını temsil eder. Bir veya daha fazla ürün ayrıntısı sayfasını ziyaret eden bir ziyaretçi, benzersiz bir ziyaretçi olarak sayılır.

### <a name="call-to-action"></a>Eyleme çağrı

Bu sayı teklif sayfasında (Ürün Ayrıntısı sayfası) tamamlanan eylem düğmesi tıklamaları **çağrısının** sayısını temsil eder. **Şimdi al**, **ücretsiz deneme**, **benimle iletişim**ve **test sürücüsü** düğmeleri seçili olduğunda **eyleme yapılan çağrılar** sayılır.

![İş Ortağı Merkezi öngörüleri eylem özetine çağrı](./media/insights-summary.png)

## <a name="page-visits-by-geography"></a>Coğrafya tarafından sayfa ziyaretleri

Aşağıdaki ısı haritasını, **sayfa ziyaretlerinin**sayısını, eyleme yapılan **çağrıları**ve **Müşteri ülkesine göre benzersiz ziyaretçileri**görüntüler. Daha büyük sayfa ziyaretlerinin daha koyu eşleme renkleriyle temsil edildiği ve daha küçük sayfa ziyaretlerinin daha açık harita renkleriyle temsil edildiği.

![İş Ortağı Merkezi öngörüleri coğrafi yayma](./media/insights-geography.png)

Isı haritasını aşağıdaki özellikleri içerir:

- Isı haritasını, **sayfa ziyaretlerinin**ayrıntılarını, belirli bir konumdaki eyleme ve **benzersiz ziyaretçilere** **yapılan çağrıları** görüntülemek için bir ek kılavuz içerir; tercih ederseniz, belirli bir konuma yakınlaştırabilirsiniz.  
- **Ülkeler/bölgeler** , seçili tarih aralığı sırasında müşterilerinizin sayfa ziyaretlerini bildirdiği tüm ülkelerin/bölgelerin sayısıdır.
- Haritada konum yakınlaştırmak için kılavuzdaki bir ülkeyi arayabilir ve seçebilirsiniz. Haritada **giriş** ' i seçerek özgün görünüme dönün.

## <a name="page-visits-versus-unique-visitors-trend"></a>Sayfa ziyaretleri ve benzersiz ziyaretçi eğilimi karşılaştırması

Aşağıdaki sütunlar, Y ekseninde (grafiğin sol tarafındaki eksen) görüntülenen aylık sayfa ziyaretlerinin sayısını temsil eder. Eğilim çizgisi, Azure Marketi ve AppSource 'ta yayınlanan tekliflerinize yönelik ikincil Y ekseni (grafiğin sağ tarafındaki eksen) üzerinde, benzersiz ziyaretçilerin aylık eğilimini temsil eder.

![İş Ortağı Merkezi Öngörüler sayfası ziyaretleri ve benzersiz ziyaretçi eğilimi](./media/insights-page-vists-unique-visitors.png)

## <a name="call-to-action-versus-unique-visitors-with-ctas"></a>CTAs ile eyleme karşı benzersiz ziyaretçilere çağrı

Yığılmış sütunlar, CTA türlerine (**Şimdi alın**, **benimle Iletişim kurun**ve **ücretsiz deneme**) göre kırılır ve Y ekseni (sayfanın sol tarafındaki eksen) üzerine çizilerek (CTA) eyleme yapılan aylık çağrıları temsil eder. Eğilim çizgisi, Azure Marketi 'nde ve AppSource 'ta yayınlanan tekliflerinizin ikincil Y ekseni (grafiğin sağ tarafındaki eksen) üzerinde, CTAs ile benzersiz ziyaretçilerin aylık eğilimini temsil eder.

![İş Ortağı Merkezi öngörüleri, CTAs ile benzersiz ziyaretçilere karşı işleme](./media/insights-call-to-action-unique-visitors.png)

## <a name="page-visits-and-calls-to-action-by-offers"></a>Teklifler tarafından yapılacak sayfa ziyaretleri ve çağrıları

Dış pasta grafiği, markette yayımladığınız ve filtrede seçtiğiniz teklifleri temel alarak **sayfa ziyaretlerinin** dökümünü temsil eder. İç grafik, aynı tekliflerle ilgili eylem dökümünü **çağırır** .

![İş Ortağı Merkezi Öngörüler sayfası ziyaretleri ve tekliflere göre eyleme çağrı](./media/insights-page-visits-and-cta-by-offer.png)

## <a name="call-to-action-percentage-trend"></a>Eylem yüzdesi eğilimi çağrısı

**Eylem yüzdesi eğilimi çağrısı** , Market 'te yayınlanan tekliflerin CTA yüzdesini gösterir. CTA% = (CTAs/sayfa ziyaretleri) * 100.

![İş Ortağı Merkezi öngörüleri çağrısı-eylem yüzdesi eğilimi](./media/insights-call-to-action-percentage-trend.png)

## <a name="page-visits-and-calls-to-action-by-referral-domains"></a>Sayfa ziyaretleri ve başvuru etki alanlarına göre eyleme çağrılar

Aşağıdaki grafik, ilk 50 başvuru etki alanını gösterir. Belirli bir başvuru etki alanının seçilmesi, sayfa ziyaretlerinin aylık eğilimini ve sağdaki grafik üzerindeki eyleme yapılan çağrıları gösterir.

![İş Ortağı Merkezi Öngörüler sayfası ziyaretleri ve başvuru etki alanları ve kampanyalar tarafından eyleme çağrı](./media/insights-page-visits-call-to-actions.png)

## <a name="marketplace-insights-details-table"></a>Market öngörüleri Ayrıntıları tablosu

Bu tabloda, sayfa ziyaretlerinin bir liste görünümü ve seçili tekliflerinizin tarihe göre sıralandığı eyleme yapılan çağrılar verilmektedir.

![İş Ortağı Merkezi Öngörüler Ayrıntıları tablosu](./media/insights-details-page.png)

- Kayıt sayısı 1000 ' den küçükse veriler bir CSV dosyasına ayıklanabilir.
- Kayıt sayısı 1000 üzerinde ise, verilen veriler sonraki 30 gün boyunca bir indirmeler sayfasına zaman uyumsuz olarak yerleştirilir.
- İlgilendiğiniz verileri göstermek için filtreler uygulanabilir. Veriler, teklif adlarına ve kampanya adlarına göre filtrelenebilir.  

## <a name="next-steps"></a>Sonraki adımlar

- Iş ortağı merkezi ticari marketi 'nde bulunan analiz raporlarına genel bakış için bkz. [Iş Ortağı Merkezi 'nde ticari Market Için analiz](./analytics.md).
- Teklifinizin Market etkinliklerini özetleyen grafikler, eğilimler ve veri değerleri için bkz. [ticari Market Analytics 'Te Özet Panosu](./summary-dashboard.md).
- Grafik ve indirilebilir biçimdeki Siparişleriniz hakkında daha fazla bilgi için bkz. [Orders Dashboard in The Commercial Market Analytics](./orders-dashboard.md).
- Sanal makine (VM) için kullanım ve ölçümlenen faturalandırma ölçümleri, bkz. [ticari Market Analytics 'Te kullanım panosu](./usage-dashboard.md).
- Büyüme eğilimleri dahil olmak üzere müşterileriniz hakkında ayrıntılı bilgi için bkz. [ticari Market Analytics 'Te müşteri panosu](./customer-dashboard.md).
- Son 30 güne ait indirme isteklerinizin listesi için bkz. [Commercial Market Analytics 'Te panoyu indirme](./downloads-dashboard.md).
- Azure Market ve AppSource 'ta teklifler için müşteri geri bildirimlerinin birleştirilmiş bir görünümünü görmek için bkz. [ticari Market Analytics 'Te derecelendirmeler ve İncelemeler panosu](./ratings-reviews.md).
- Ticari Market Analizi ve kapsamlı veri koşulları sözlüğü hakkında sık sorulan sorular için bkz. [ticari Market Analytics Için sık sorulan sorular ve terminoloji](./faq-terminology.md).
