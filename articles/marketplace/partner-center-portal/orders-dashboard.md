---
title: Ticari Pazar analizinde İş Ortağı Merkezi Siparişleri panosu
description: Pazar yeri teklif siparişlerinizle ilgili analitik raporlara grafiksel ve indirilebilir biçimde nasıl eriştiğinizi öğrenin.
author: dsindona
ms.author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 12/11/2019
ms.openlocfilehash: d0db6553a774a69bb8a55538cbd2b4a333be9316
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80281417"
---
# <a name="orders-dashboard-in-commercial-marketplace-analytics"></a>Ticari Pazar analizinde sipariş panosu

Bu makale, İş Ortağı Merkezi'ndeki **Siparişler panosu** hakkında bilgi sağlar. Bu pano, siparişlerinizle ilgili bilgileri grafiksel ve indirilebilir biçimde görüntüler.

İş Ortağı Merkezi analiz araçlarındaki **Siparişler panosuna** erişmek için Commercial Marketplace altında **[Analiz panosunu](https://partner.microsoft.com/dashboard/commercial-marketplace/analytics/summary)** açın.

>[!NOTE]
> Analitik terminolojinin ayrıntılı tanımları [için, Ticari Pazar analitiği için sık sorulan sorular ve terminolojiye](./faq-terminology.md)bakın.

## <a name="orders-dashboard"></a>Siparişler panosu

**Analiz** menüsünün **Siparişler panosu,** Tüm SaaS tekliflerinizin geçerli siparişlerini görüntüler. Aşağıdaki öğelerin grafik gösterimlerini görüntüleyebilirsiniz:

- [Sipariş özeti](#order-summary)
- [Coğrafyaya göre siparişler](#orders-by-geography)
- [Tekliflere göre siparişler](#orders-by-offers)
- [Site başına koltuk başına siparişler trendi](#orders-trend-per-site-versus-per-seat)
- [SK'lardan siparişler](#orders-by-skus)
- [Siparişler ve koltuk trendi](#orders-and-seats-trend)
- [Sipariş ayrıntıları tablosu](#order-details-table)

> [!NOTE]
> Analitik raporların Bulut İş Ortağı Portalı'nda (CPP) nasıl görüntülenmesi ile İş Ortağı Merkezi'ndeki yeni Ticari Pazar programı arasında farklar vardır. Belirli bir yol, CPP'deki **Satıcı Öngörüleri'nin,** kullanıma dayalı teklifler ve kullanıma dayalı olmayan tekliflere ilişkin verileri görüntüleyen **Siparişler & Kullanım** sekmesine sahip olmasıdır. Ortak Merkezi'nde, **Siparişler** sayfasının SaaS Teklifleri için ayrı bir sekmesi vardır.

## <a name="order-dashboard-details"></a>Pano ayrıntılarını sipariş edin

Bu bölümde analiz raporları daha ayrıntılı olarak açıklanmaktadır.

### <a name="order-summary"></a>Sipariş Özeti

Sipariş Özeti bölümü, satın alınan tüm siparişlerin (iptal edilen siparişler, iptal edilen siparişler ve koltuklar hariç) bir sayısını görüntüler.

Toplam Siparişler'in yanındaki yüzde değeri, seçili tarih aralığındaki büyüme miktarını gösterir.

![Ortak Merkezi Analiz sipariş özeti](./media/order-summary.png)

- Yukarıyı gösteren yeşil üçgen olumlu bir büyüme eğilimigösterir.
- Aşağıyı gösteren kırmızı üçgen, bir önceki aya göre negatif bir büyüme eğilimigösterir.
- Büyüme eğilimleri mikro çubuk grafiklerle temsil edilir. Grafikiçindeki sütunların üzerinde gezinerek her ayın değerini görüntüleyebilirsiniz.
- İptal edilen siparişler, seçili tarih aralığında önceden satın alınan ve sonra iptal edilen siparişlerin sayısıdır.
- Koltuklar, seçilen tarih aralığında oluşturulan koltuk sayısıdır.

### <a name="orders-by-geography"></a>Coğrafyaya göre siparişler

**Coğrafyaya Göre Siparişler** ısı haritası, bir dünya haritasında siparişlerinizin sayısını görüntüler ve Müşteri Ülkesine göre eşlenen koltukları gösterir. Bu ısı haritası coğrafya **[ısı haritası](./customer-dashboard.md#customer-by-geography)** ile Müşteri ile aynı işlevleri.

![Ortak Merkezi Coğrafyaya Göre Siparişleri Analiz Et](./media/orders-by-geography.png)

### <a name="orders-by-offers"></a>Tekliflere göre siparişler

Teklif donut grafiğine göre **Siparişler,** teklif adlarına göre siparişleri (iptal edilen siparişler dahil) düzenler.

- Üst teklifler grafikte görüntülenir ve tekliflerin geri kalanı 'Rest All' olarak gruplandırılır.
- Yalnızca grafikteki teklifleri görüntülemek için göstergedeki belirli teklifleri seçebilirsiniz.
- Grafikteki bir dilimin üzerinde gezinmek, tüm tekliflerdeki toplam sipariş sayısıyla karşılaştırıldığında, bu teklifin sayısını ve yüzdesini görüntüler.
- **Tekliflerin verdiği siparişler,** aylık büyüme eğilimlerini gösterir. Ay sütunu teklif adına göre sipariş sayısını gösterir. Çizgi grafiği z ekseninde çizilen büyüme yüzdesi eğilimini görüntüler.
- Grafiğin üst kısmındaki kaydırıcıyı kullanarak x ekseni boyunca sağa ve sola kaydırılabilir ve belirli veri noktalarına odaklanabilirsiniz.
- Göstergeüzerinde belirli bir öğe seçerek eğilim grafiğini görüntüleyebilirsiniz.
- İptal **edilen siparişler**için eğilimleri ve verileri görüntülemeyi de seçebilirsiniz. Grafik, teklifler grafiğiile **siparişlerle** aynı şekilde çalışacaktır.

### <a name="orders-trend-per-site-versus-per-seat"></a>Siparişler Site başına koltuk başına eğilim

Site başına ve **koltuk başına** donut grafiği, site başına SaaS ve müşteriler tarafından satın alınan koltuk başına SaaS siparişlerinin dökümünü temsil eder (bu grafik iptal edilen siparişleri içerir). Sütun grafiği, site başına SaaS ve müşteriler tarafından satın alınan koltuk başına SaaS siparişlerinin eğilimini temsil eder (bu grafik iptal edilen siparişleri içerir).

### <a name="orders-by-skus"></a>SK'lardan siparişler

**SKU'lar** tarafından Siparişler grafiği, tüm teklifleriniz için Stok Tutma Birimi (SKU) düzeyindeki siparişlerin eğilimini temsil eder (bu, iptal edilen siparişleri içerir). Donut grafiği ilk beş SKU siparişinin dökümünü temsil eder ve sütun grafiği ilk beş SKU'nun sipariş eğilimini temsil eder.

### <a name="orders-and-seats-trend"></a>Siparişler ve Koltuk Trendi

**Siparişler ve koltuk eğilim** grafiği en yüksek sipariş sayısıile ilk 50 teklifi sunar. Bunlar bir lider panosunda görüntülenir ve en yüksek sıra sayısı ve sipariş yüzdesi ile sıralanır.

- **SK'lere göre siparişler**: Grafikteki ilk beş SUK'un sipariş sayısının dökümünü görüntülemek için bir teklif seçin.
- **SKUs'lara göre koltuklar**: En iyi beş SNU'nun aylık koltuk trendi. Seçtiğiniz teklif koltuk başına bir teklif değilse, bu alan grafiğinde herhangi bir veri görmezsiniz.

### <a name="canceled-orders-by-offers"></a>Tekliflere göre iptal edilen siparişler

Teklif pasta grafiği **tarafından İptal Edilen siparişler,** iptal edilen tüm siparişlerinizi teklif adlarına göre düzenler. En iyi teklifler grafikte görüntülenir ve tekliflerin geri kalanı "Rest All" olarak gruplandırılır. Grafikte görüntülemek için göstergedeki belirli teklifleri seçebilirsiniz.

- Grafikteki bir dilimin üzerinde gezinmek, tüm tekliflerdeki toplam sipariş sayısıyla karşılaştırıldığında, seçilen teklifin sayısını ve yüzdesini görüntüler.
- Sütun grafiği aylık eğilimleri görüntüler. Sütunlar, teklif adına göre iptal edilen sipariş sayısını temsil eder. Grafiğin üstündeki kaydırıcıyı kullanarak x ekseni boyunca sağa ve sola kaydırAbilir ve belirli veri noktalarına odaklanabilirsiniz. Göstergeüzerinde belirli bir öğe seçerek eğilim grafiğini görüntüleyebilirsiniz.

### <a name="order-details-table"></a>Sipariş ayrıntıları tablosu

Sipariş ayrıntıları tablosu, satın alma tarihine göre sıralanmış 1000 üst siparişin sayılı listesini görüntüler.

- Kılavuzdaki her sütun sıralanabilir.
- Kayıtların sayısı 1000'den azsa veriler Bir TSV dosyasına ayıklanabilir.
- Kayıt sayısı 1000'in üzerindeyse, dışa aktarılan veriler, önümüzdeki 30 gün boyunca bir indirme sayfasına eşit olarak yerleştirilir.
- Filtreler, yalnızca ilgilendiğiniz verileri görüntülemek için **Sipariş ayrıntıları tablosuna** uygulanabilir. Veriler Ülke, Azure lisans türü, Market lisans türü, Teklif türü, Sipariş durumu, Ücretsiz yollar, Market abonelik kimliği, Müşteri Kimliği ve Şirket adına göre filtrelenebilir.

#### <a name="orders-page-filters"></a>Siparişler Sayfa filtreleri

Bu filtreler sayfa düzeyinde uygulanır.

Grafiği görüntülemek için seçtiğiniz ölçütler ve **Ayrıntılı Sipariş Veri** ızgarası/dışa aktarma da görüntülenmesini istediğiniz veriler için grafiği işlemek için birden çok filtre seçebilirsiniz. Filtreler, siparişler sayfasının sağ üst köşesinde seçtiğiniz veri aralığı için ayıklanan verilere uygulanır.

- Teklif türleri ve teklif adları yalnızca seçilen tarih aralığında sipariş ettiğiniz teklifler için listelenir. Listedeki teklif adları, listede seçtiğiniz teklif türleri için görüntülenir.
- Uygulanan filtreler, seçilen her filtre için her seçim(ler) içindeki toplam ölçümleri gösterir. Varsayılan seçim seçildiğinde uygulanan filtreler görüntülenmez.
- Açılır listelerden biri için **Tümü** seçilirse, seçilen sayfadaki tüm ölçümler toplanır. Örneğin: Teklif türleri filtresi seçeneğindeki "Tümü" tüm teklif türlerinin seçildiği anlamına gelir. Bu, açılır listeler için varsayılan seçimdir. Uygulanan filtreler, **Tümü** seçildiğinde hiçbir şey göstermez.
- **Çoklu değer seçimi**: Sayfadaki tüm ölçümler açılır liste altında yapılan tüm seçimler için toplanır. Birden çok seçim yapılırsa, uygulanan filtre yapılan tüm seçimlerin sayısını gösterir. Başvuru için aşağıdaki resme bakın.

    ![İş Ortağı Merkezi Filtreye uygulanan birden çok değerle siparişi analiz etme](./media/filters-applied.png)

- **Tek değer seçimi**: Bir değer seçilirse, uygulanan filtre seçili bir filtrenin sayısını gösterir. Referans için aşağıya bakın.

     ![İş Ortağı Merkezi Filtreye uygulanan tek değerli siparişi analiz etme](./media/filters-applied-single.png)

## <a name="next-steps"></a>Sonraki adımlar

- İş Ortağı Merkezi Ticari Pazarı'nda bulunan analiz raporlarına genel bir bakış için, [İş Ortağı Merkezi'ndeki Ticari Pazar Için Analitik bölümüne](./analytics.md)bakın.
- Teklifiniz için pazar daki etkinliği özetleyen toplam verilerin grafikleri, eğilimleri ve değerleri için, [Ticari Pazar analitiğinde Özet panosuna](./summary-dashboard.md)bakın.
- Sanal Makine (VM) kullanım ve ölçülü fatura ölçümleri için, [Ticari Pazar analitiğinde Kullanım panosuna](./usage-dashboard.md)bakın.
- Büyüme eğilimleri de dahil olmak üzere müşterileriniz hakkında ayrıntılı bilgi için [Ticari Pazar analitiğinde Müşteri panosuna](./customer-dashboard.md)bakın.
- Son 30 gün içinde indirme isteklerinizin bir listesi için, [Commercial Marketplace analitiğinde İndirme panosuna](./downloads-dashboard.md)bakın.
- Azure Marketi ve AppSource'daki teklifler için müşteri geri bildirimlerinin konsolide görünümünü görmek için, [Commercial Marketplace analitiğinde Derecelendirmeler ve incelemeler panosuna](./ratings-reviews.md)bakın.
- Ticari Pazar yeri analitiği hakkında sık sorulan sorular ve kapsamlı bir veri terimleri sözlüğü [için, Ticari Pazar analitiği için sık sorulan sorular ve terminolojiye](./faq-terminology.md)bakın.
