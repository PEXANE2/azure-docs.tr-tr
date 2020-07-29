---
title: Ticari Market Analytics, Microsoft AppSource ve Azure Market 'teki iş ortağı merkezi siparişleri panosu
description: Ticari Market teklif siparişleriniz hakkındaki analitik raporlara grafik ve indirilebilir bir biçimde nasıl erişebileceğinizi öğrenin.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: article
ms.date: 07/22/2020
author: mingshen-ms
ms.author: mingshen
ms.openlocfilehash: d5adc1bfe19de48568d0e77bb488bea0e5a02818
ms.sourcegitcommit: a76ff927bd57d2fcc122fa36f7cb21eb22154cfa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87327387"
---
# <a name="orders-dashboard-in-commercial-marketplace-analytics"></a>Ticari market analizinde Siparişler panosu

Bu makalede, Iş Ortağı Merkezi 'nde **siparişler panosu** hakkında bilgi sağlanmaktadır. Bu Pano, siparişleriniz hakkındaki bilgileri grafik ve indirilebilir bir biçimde görüntüler.

Iş ortağı merkezi analiz araçları 'ndaki **siparişler panosuna** erişmek Için, ticari Market altında **[analiz panosunu](https://partner.microsoft.com/dashboard/commercial-marketplace/analytics/summary)** açın.

>[!NOTE]
> Analiz terminolojisi hakkında ayrıntılı tanımlamalar için bkz. [ticari Market Analizi Için sık sorulan sorular ve terminoloji](./faq-terminology.md).

## <a name="orders-dashboard"></a>Siparişler panosu

**Çözümle** menüsünün **siparişler panosu** , SaaS tekliflerinizin tamamına ait geçerli siparişleri görüntüler. Aşağıdaki öğelerin grafik sunumlarını görüntüleyebilirsiniz:

- [Sipariş Özeti](#order-summary)
- [Coğrafya tarafından siparişler](#orders-by-geography)
- [Tekliflere göre siparişler](#orders-by-offers)
- [Site başına ve bilgisayar başına düşen sipariş eğilimi](#orders-trend-per-site-versus-per-seat)
- [Planlara göre siparişler](#orders-by-plans)
- [Siparişler ve koltuk eğilimi](#orders-and-seats-trend)
- [Sipariş Ayrıntıları tablosu](#order-details-table)

Iş Ortağı Merkezi 'nde siparişler oluşturma ve raporlama arasındaki maksimum gecikme süresi 48 saattir.

## <a name="order-dashboard-details"></a>Pano ayrıntılarını Sırala

Bu bölümde analiz raporları daha ayrıntılı olarak açıklanmaktadır.

### <a name="order-summary"></a>Sipariş Özeti

Sipariş Özeti bölümünde satın alınan tüm siparişlerin sayısı (iptal edilen siparişler hariç), iptal edilen siparişler ve koltuk görüntülenir.

Toplam siparişlerin yanındaki yüzde değeri, seçilen tarih aralığındaki büyüme miktarını temsil eder.

![İş Ortağı Merkezi sipariş özetini analiz etme](./media/order-summary.png)

- Yukarıyı gösteren bir yeşil üçgen, pozitif bir büyüme eğilimi gösterir.
- Aşağıyı gösteren kırmızı üçgen, önceki aya göre negatif büyüme eğilimi gösterir.
- Büyüme eğilimleri mikro çubuk grafikleriyle temsil edilir. Grafik içindeki sütunların üzerine gelindiğinde her ayın değerini görüntüleyebilirsiniz.
- İptal edilen siparişler, daha önce satın alınan siparişlerin sayılardır ve ardından seçilen tarih aralığı sırasında iptal edilir.
- Koltuk, seçilen tarih aralığı sırasında oluşturulan bir lisans sayısıdır.

### <a name="orders-by-geography"></a>Coğrafya tarafından siparişler

**Coğrafya, coğrafi olarak yapılacak siparişler** bir dünya haritası üzerinde siparişlerin sayımını gösterir ve müşteri ülkesi/bölgesine göre eşlenmiş olan bilgisayarları gösterir. Bu ısı haritasını, **[Coğrafya ısı haritasını ile müşteriyle](./customer-dashboard.md#customer-by-geography)** aynı şekilde çalışır.

![İş Ortağı Merkezi, coğrafi olarak sipariş Analizi](./media/orders-by-geography.png)

### <a name="orders-by-offers"></a>Tekliflere göre siparişler

Bu **siparişler** , halka grafik sunarak, teklif adlarına göre siparişleri (iptal edilen siparişler dahil) düzenler.

- Üstteki teklifler grafikte görüntülenir ve tekliflerin geri kalanı ' rest All ' olarak gruplandırılır.
- Yalnızca grafikteki teklifleri göstermek için göstergede belirli teklifleri seçebilirsiniz.
- Grafikteki bir dilimin üzerine gelindiğinde, tüm tekliflerde toplam sipariş sayısı ile karşılaştırıldığında, bu teklifin sipariş sayısı ve yüzdesi görüntülenir.
- **Siparişlere göre siparişler eğilimi** , aylık büyüme eğilimlerini görüntüler. Month sütunu, teklif adına göre sipariş sayısını temsil eder. Çizgi grafik, z ekseni üzerinde çizilen büyüme yüzdesi eğilimini görüntüler.
- X ekseninde sağa ve sola kaydırmak ve belirli veri noktalarına odaklanmak için grafiğin en üstündeki kaydırıcıyı kullanabilirsiniz.
- Açıklamada belirli bir öğe seçerek eğilim grafiğini görüntüleyebilirsiniz.
- Ayrıca, **iptal edilen siparişlerin**eğilimlerini ve verilerini görüntülemeyi de seçebilirsiniz. Grafik, **Teklifler tarafından sunulan** grafikle aynı şekilde çalışır.

### <a name="orders-trend-per-site-versus-per-seat"></a>Site başına ve bilgisayar başına düşen sipariş eğilimi

Site başına ve **bilgisayar** başına halka grafik, müşteriler tarafından satın alınan site başına SaaS ve bilgisayar başına SaaS siparişlerinin dökümünü temsil eder (Bu grafik iptal edilen siparişleri içerir). Sütun grafiği, her site SaaS için ve müşteriler tarafından satın alınan her bilgisayar SaaS siparişi için eğilimi temsil eder (Bu grafik iptal edilen siparişleri içerir).

### <a name="orders-by-plans"></a>Planlara göre siparişler

Plana **göre siparişler** grafiği, tüm teklifleriniz için plan düzeyinde siparişlerin eğilimini temsil eder (iptal edilen siparişler dahildir). Halka grafik, ilk beş plan siparişlerinin dökümünü temsil eder ve sütun grafiği, ilk beş plana ait siparişlerin eğilimini temsil eder.

### <a name="orders-and-seats-trend"></a>Siparişler ve koltuk eğilimi

**Siparişler ve koltuk eğilimi** grafiği en yüksek sipariş sayısı ile ilk 50 teklifi sunar. Bunlar bir öncü panoda görüntülenir ve en yüksek sıra sayısına ve sıra yüzdesine göre sıralanır.

- **Planlara göre siparişler** – grafikteki en üstteki beş plan için sıra sayısı dökümünü görüntülemek üzere bir teklif seçin.
- **Planlara göre** bilgisayar sayısı: ilk beş plan için bir koltuk için aylık eğilim. Seçtiğiniz teklif, bilgisayar başına bir teklif değilse burada hiçbir veri gösterilmez.

### <a name="canceled-orders-by-offers"></a>Tekliflere göre iptal edilen siparişler

**Iptal edilen siparişler** , pasta grafiği sunarak, iptal edilen tüm siparişlerinizi kendi teklif adlarına göre düzenler. Üstteki teklifler grafikte görüntülenir ve tekliflerin geri kalanı "Rest All" olarak gruplandırılır. Grafikte göstermek için göstergede belirli teklifleri seçebilirsiniz.

- Grafikteki bir dilimin üzerine gelindiğinde, tüm tekliflerde bulunan toplam sipariş sayısı ile karşılaştırıldığında seçilen teklifin sipariş sayısı ve yüzdesi görüntülenir.
- Sütun grafiği, aylık eğilimleri gösterir. Sütunlar, teklif adına göre iptal edilen siparişlerin sayısını temsil eder. X ekseninde sağ ve sol ok ve belirli veri noktalarına odaklanılacak şekilde grafiğin üstündeki kaydırıcıyı kullanabilirsiniz. Açıklamada belirli bir öğe seçerek eğilim grafiğini görüntüleyebilirsiniz.

### <a name="order-details-table"></a>Sipariş Ayrıntıları tablosu

Sipariş Ayrıntıları tablosu, Alım tarihine göre sıralanan 1000 üst siparişin numaralandırılmış bir listesini görüntüler.

- Kılavuzdaki her bir sütun sıralanabilir.
- Kayıt sayısı 1000 ' den küçükse veriler TSV dosyasına ayıklanabilir.
- 1000 üzerinden kayıt numarası varsa, verilen veriler sonraki 30 güne ait bir indirmeler sayfasına zaman uyumsuz olarak yerleştirilir.
- Yalnızca ilgilendiğiniz verileri göstermek için **Sipariş Ayrıntıları tablosuna** filtre uygulayın. Ülke/bölge, Azure lisans türü, ticari Market lisans türü, teklif türü, sipariş durumu, ücretsiz izler, ticari Market abonelik KIMLIĞI, müşteri KIMLIĞI ve şirket adına göre filtreleyin.
- Azure Market veya AppSource aracılığıyla satın alınan SaaS teklifleri için Azure aboneliği gerekmez, **ayrıntılı siparişler verileri** bölümünde Market abonelik kimliği 00000000-0000-0000-0000-000000000000 olarak görünür.

#### <a name="orders-page-filters"></a>Sipariş sayfası filtreleri

Bu filtreler sayfa düzeyinde uygulanır.

Görüntülemek istediğiniz ölçütlere ve **ayrıntılı sıralama verileri** Kılavuzu/dışarı aktarma bölümünde görüntülenmesini istediğiniz verilere göre grafiği işlemek için birden çok filtre seçebilirsiniz. Filtreler, siparişler sayfasının sağ üst köşesinde seçtiğiniz veri aralığı için ayıklanan verilere uygulanır.

- Teklif türleri ve teklif adları yalnızca seçili tarih aralığı boyunca siparişiniz olan teklifler için listelenir. Listedeki teklif adları, listede seçtiğiniz teklif türleri için görüntülenir.
- Uygulanan filtreler seçili her bir filtre için her seçim içindeki toplam ölçümleri gösterir. Uygulanan filtreler varsayılan seçim seçildiğinde gösterilmez.
- Açılan listelerden biri için **Tümü** seçilirse, seçilen sayfadaki tüm ölçümler toplanır. Örneğin: teklif türleri filtre seçeneğinde "tümü", tüm teklif türlerinin seçildiği anlamına gelir. Bu, açılan listeler için varsayılan seçimdir. Uygulanan filtreler, **Tümü** seçildiğinde hiçbir şey göstermez.
- **Birden çok değer seçimi**: sayfadaki tüm ölçümler, açılan liste altında yapılan tüm seçimler için toplanacaktır. Birden çok seçim yapılırsa, uygulanan filtre yapılan tüm seçimlerin sayısını gösterir. Başvuru için aşağıdaki resme bakın.

    ![İş Ortağı Merkezi, filtreye birden çok değer uygulanmış şekilde çözümleme sırası](./media/filters-applied.png)

- **Tek değer seçimi**: bir değer seçilirse, uygulanan filtre seçili bir filtrenin sayısını gösterir. Başvuru için aşağıdaki resme bakın.

     ![İş Ortağı Merkezi, tek değer filtreye uygulanan sırayı analiz eder](./media/filters-applied-single.png)

## <a name="next-steps"></a>Sonraki adımlar

- Iş ortağı merkezi ticari marketi 'nde bulunan analiz raporlarına genel bakış için bkz. [Iş Ortağı Merkezi 'nde ticari Market Için analiz](./analytics.md).
- Teklifinizin Market etkinliklerini özetleyen grafikler, eğilimler ve veri değerleri için bkz. [ticari Market Analytics 'Te Özet Panosu](./summary-dashboard.md).
- Sanal makine (VM) için kullanım ve ölçümlenen faturalandırma ölçümleri, bkz. [ticari Market Analytics 'Te kullanım panosu](./usage-dashboard.md).
- Büyüme eğilimleri dahil olmak üzere müşterileriniz hakkında ayrıntılı bilgi için bkz. [ticari Market Analytics 'Te müşteri panosu](./customer-dashboard.md).
- Son 30 güne ait indirme isteklerinizin listesi için bkz. [Commercial Market Analytics 'te panoyu indirme](./downloads-dashboard.md).
- Azure Market ve AppSource 'ta teklifler için müşteri geri bildirimlerinin birleştirilmiş bir görünümünü görmek için bkz. [ticari Market Analytics 'Te derecelendirmeler ve İncelemeler panosu](./ratings-reviews.md).
- Ticari Market Analizi ve kapsamlı veri koşulları sözlüğü hakkında sık sorulan sorular için bkz. [ticari Market Analytics Için sık sorulan sorular ve terminoloji](./faq-terminology.md).
