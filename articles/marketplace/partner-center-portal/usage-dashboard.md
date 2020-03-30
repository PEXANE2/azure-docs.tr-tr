---
title: İş Ortağı Merkezi'nde Commercial Marketplace analitiğinde kullanım panosu
description: Tüm VM tekliflerine ve tarifeli faturalandırma ölçümlerine nasıl erişilenleri öğrenin.
author: dsindona
ms.author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 12/11/2019
ms.openlocfilehash: 0cd6614c1ccc51a2b25f115dccf0ee389075aa70
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80285138"
---
# <a name="usage-dashboard-in-commercial-marketplace-analytics"></a>Ticari Pazar Analizi'nde Kullanım Panosu

Bu makalede, İş Ortağı Merkezi'ndeki Kullanım panosu hakkında bilgi verilmektedir. Bu pano, tüm VM'lerin kullanım ve ölçülü fatura ölçümlerini iki ayrı sekmede görüntüler: VM kullanımı ve ölçülü faturalandırma kullanımı.

Kullanım panosuna erişmek için Ticari Pazar Yeri altında **[Analiz](https://partner.microsoft.com/dashboard/commercial-marketplace/analytics/summary)** panosunu açın.

>[!NOTE]
> Analitik terminolojinin ayrıntılı tanımları [için, Ticari Pazar analitiği için sık sorulan sorular ve terminolojiye](./faq-terminology.md)bakın.

## <a name="usage-dashboard"></a>Kullanım panosu

Kullanım Panosu, tüm Sanal Makine (VM) için yapılan ölçümleri temsil eder ve kullanım ve ölçülü faturalandırma kullanımı sunar. Bunlar iki ayrı sekmede bulunur: VM kullanımı ve ölçülü faturalandırma kullanımı.

VM kullanım sekmesinde, aşağıdaki öğelerin grafik gösterimleri vardır:

- [Kullanım özeti](#usage-summary)
- [Coğrafyaya göre kullanım](#usage-by-geography)
- [Tekliflere göre kullanım](#usage-by-offers)
- [Tekliflere ve SK'lere göre kullanım trendi](#usage-trend-by-offers-and-skus)
- [Teklif türüne göre kullanım](#usage-by-offer-type)
- [VM boyutuna göre kullanım](#usage-by-vm-size)
- [Satış kanalına göre kullanım](#usage-by-sales-channel)
- [Ayrıntılı kullanım verileri](#detailed-usage-data)

> [!NOTE]
> Analytics raporları Bulut İş Ortağı Portalı (CPP) ve İş Ortağı Merkezi'nde farklı görüntüler. CPP'deki **satıcı öngörüleri,** hem kullanıma dayalı tekliflerhem de kullanıma dayalı olmayan tekliflerin verilerini görüntüleyen bir sipariş ve kullanım sekmesine sahiptir. Ortak Merkezi'nde kullanım ölçümleri ayrı bir sayfada görüntülenir.

### <a name="usage-summary"></a>Kullanım özeti

Kullanım özeti tablosu, satın aldıkları tüm tekliflerin müşteri kullanım saatlerini görüntüler.

- Normalleştirilmiş kullanım saatleri, VM çekirdeği sayısını ([VM çekirdek sayısı] x [ham kullanım saatleri]) hesaba katmak için normalleştirilmiş kullanım saatleri olarak tanımlanır. "SHAREDCORE" olarak atanan VM'ler 1/6 (veya 0,1666) [VM çekirdek] çarpanı olarak kullanır.
- Ham kullanım saatleri, VM'lerin saat bazında çalışma süresi olarak tanımlanır.
- Yüzde değeri, seçili tarih aralığı ([son ay kullanımı – ilk ay kullanımı])/ ilk ay kullanımı için kullanım artışını gösterir.
- Yukarıyı gösteren yeşil üçgenler büyüme değişimini gösterir.
- Aşağıyı gösteren kırmızı üçgen, bir önceki aya göre negatif bir büyüme değişimini gösterir.
- Mikro çubuk grafikler aylık değerleri temsil eder. Grafikiçindeki sütunların üzerinde gezinerek her ayın değerini görüntüleyebilirsiniz.

### <a name="usage-by-geography"></a>Coğrafyaya göre kullanım

Coğrafya ısı haritası **tarafından normalleştirilmiş kullanım,** müşteri ülkesine göre eşlenen kullanım saatlerini görüntüler. Ülke renk değişimi normalleştirilmiş kullanım konsantrasyonu temsil eder. Haritadaki **ana ekran** düğmesine basarak orijinal görünüme geri dön.

### <a name="usage-by-offers"></a>Tekliflere göre kullanım

- Teklifler pasta grafiği **tarafından normalleştirilmiş kullanım,** seçilen tarih aralığına göre tekliflere göre normalleştirilmiş kullanım saatlerinin dökümünü görüntüler. En iyi 5 teklif grafikte görüntülenirken, geri kalanı "geri kalan tüm" kategorisinde gruplandırılır.
- Çubuk grafik, seçili tarih aralığı için aylık büyüme eğilimini göstermektedir. Ay sütunları, ilgili ayın en yüksek kullanım saatleriyle tekliflerin kullanım saatlerini gösterir. Çizgi grafiği ikincil Y ekseninde çizilen büyüme yüzdesi eğilimini göstermektedir.
- X ekseni boyunca sağa sola kaydırmak ve/veya belirli veri noktalarına odaklanmak için grafiğin üst kısmındaki kaydırıcıyı kullanın.

### <a name="usage-trend-by-offers-and-skus"></a>Tekliflere ve SK'lere göre kullanım trendi

Bu grafik, bir teklifin seçili SNU'ları için normalleştirilmiş kullanım eğilimini görüntüler. Teklif lider panosu en yüksek kullanıma sahip en iyi 50 teklifi görüntüler ve kullanım saatlerine göre sıralanır. SKU lider panosu, seçilen teklif için en yüksek kullanıma sahip ilk 50 SKU'yu görüntüler.

### <a name="usage-by-offer-type"></a>Teklif türüne göre kullanım

- **Teklif türüne göre kullanım** pasta grafiği, kullanımı teklif türüne göre düzenler.
- En iyi teklifler grafikte görüntülenir ve tekliflerin geri kalanı 'Rest All' olarak gruplandırılır.
- **Eğilim** grafiği aylık büyüme eğilimlerini görüntüler. Ay sütunu, o aydaki en iyi teklif türlerine göre kullanımı temsil eder.

### <a name="usage-by-vm-size"></a>VM boyutuna göre kullanım

Bu grafik, tüm tekliflerinizin/SNU'larınızın seçilen VM Boyutlarının (max 5) kullanım eğilimini temsil eder. Sütun grafiği, seçili VM Boyutlarının kullanım saatleri ile birlikte istiflenir.

Lider panosu, en yüksek kullanımla en iyi 50 VM boyutunu görüntüler ve kullanım saatlerine göre sıralanır.

### <a name="usage-by-sales-channel"></a>Satış kanalına göre kullanım

- Satış kanalı pasta grafiğine göre kullanım, kullanımı satış kanalına göre düzenler
- En yüksek kullanıma sahip en iyi satış kanalı grafikte görüntülenir ve satış kanalının geri kalanı 'Rest All' olarak gruplandırılır.
- Ay sütunu, o aydaki en iyi satış kanalının kullanımını temsil eder.
- Bu grafiğin özellikleri 'Tekliflere Göre Kullanım' grafiğiyle aynıdır

### <a name="detailed-usage-data"></a>Ayrıntılı kullanım verileri

**Kullanım ayrıntıları tablosu,** kullanıma göre sıralanmış en iyi 1000 kullanım kaydının numaralı listesini görüntüler.

- Kılavuzdaki her sütun sıralanabilir.
- Kayıtların sayısı 1000'den azsa veriler bir CSV dosyasına ayıklanabilir.
- Kayıt sayısı 1000'in üzerindeyse, dışa aktarma verileri, önümüzdeki 30 gün boyunca kullanılabilecek bir indirme sayfasına eşit bir şekilde yerleştirilir.
- Filtreler, yalnızca ilgilendiğiniz verileri görüntülemek için **ayrıntılı kullanım verilerine** uygulanabilir. Veriler ülkeye, satış kanalına, Market lisans türüne, kullanım türüne, teklif adına, teklif türüne, ücretsiz denemelerine, Pazar Yeri abonelik kimliğine, müşteri kimliğine ve şirket adına göre filtrelenebilir.

> [!NOTE]
> "Normalleştirilmiş görünüm" veya "Ham görünüm" sayfadaki grafikleri görüntülemek için sayfa filtresindeki **Kullanım türünü** seçin. Bu grafikler için varsayılan görünüm "Normalleştirilmiş görünüm" olur.

**Kullanım sayfası filtreleri** sayfa düzeyinde uygulanır. Grafiği görüntülemeyi seçtiğiniz ölçütler için ve "ayrıntılı kullanım verileri" ızgarasında/dışa aktarmada görüntülenmesini istediğiniz verileri işlemek için birden çok filtre seçebilirsiniz. Filtreler, siparişler sayfasının sağ üst köşesinde seçtiğiniz veri aralığı için ayıklanan verilere uygulanır.

- **Teklif türleri** ve **Teklif adları** yalnızca seçilen tarih aralığında edindiğiniz teklifler için listelenir. Listedeki teklif adları, listeden seçilen teklif türleri için görüntülenir.
- Varsayılan seçim, Kullanım türü dışında filtre seçeneklerinin her biri için "Tümü" **olur.** **Kullanım türü** için varsayılan seçim normalleştirilmiş kullanımdır. Grafiklerde ham kullanımı görüntülemek için "ham kullanım" seçeneğini belirleyin.
- Uygulanan filtreler, yapılan filtre seçimleri için sayım seçimlerini(ler) gösterir. Uygulanan filtreler varsayılan seçimler için görüntülenmez.

> [!NOTE]
> "Ayrıntılı sipariş verileri" ızgarasındaki alanların her birinin ayrıntılı bir tanımı, sayfa filtreleri ve olası tüm seçimler [SSS ve terminoloji](link needed) makalesinin veri sözlüğü bölümünde tanımlanır.

**Tarifeli faturalandırma kullanım** sekmesi, kullanımın metre başına boyutla ölçüldüğü teklif türleri için kullanım bilgilerini sunar. SaaS teklif türü overage şu anda sunulmaktadır. Sekme, SaaS tarifeli faturakullanımı için aşiyen eğilimlerin grafik gösterimlerini sunar:

- **Sayaç boyutuna göre fazlalık eğilimi**: Bir teklifin seçilen metre boyutu için aylık aşırı yaşam trendini görüntüler. X Ekseni ayı, Y Ekseni ise kullanım miktarını temsil eder. Özel sayacın ölçüm birimi de Y-Ekseni üzerinde görüntülenir.
- **SKU'ya göre aşırı yas:** Seçilen sayaç boyutunun SKU'lar tarafından kullanım miktarını gösterir. Görüntülenen SNU'lar, seçilen teklif için en yüksek kullanım miktarına sahip ilk 5 SNU'yu temsil eder.
- **Top 50 Müşteriler tarafından Overage eğilim**: En yüksek kullanım saatleri ile ilk 50 teklifleri bir ***lider panosunda*** görüntülenir ve özel metre en yüksek kullanım tarafından sıralanır. Seçili sayaç boyutunun kullanım eğilimini görüntülemek için lider panosunda bir müşteri seçin.
- **Üst düzey müşteriler tarafından aşırı yayılım eğilimi**: Genel kullanım %'sine katkıda bulunan en iyi müşteri yüzdesini sunar. Üst müşteri yüzdesi X ekseni boyunca görüntülenir ve müşterinin kullanım miktarına göre belirlenir. Y ekseni kullanım miktarını görüntüler. Çizgi grafiği boyunca noktaların üzerinde gezinerek ayrıntıları görüntüleyebilirsiniz.

> [!NOTE]
> Bu sayfadaki kullanım ayrıntıları ve tüm grafikler, sayfa filtresi için hangi sayaç boyutu seçilirse seçilsin görüntülenir.

## <a name="next-steps"></a>Sonraki Adımlar

- İş Ortağı Merkezi Ticari Pazarı'nda bulunan analiz raporlarına genel bir bakış için, [İş Ortağı Merkezi'ndeki Ticari Pazar Için Analitik bölümüne](./analytics.md)bakın.
- Teklifiniz için pazar daki etkinliği özetleyen toplam verilerin grafikleri, eğilimleri ve değerleri için, [Ticari Pazar analitiğinde Özet Panosu'na](./summary-dashboard.md)bakın.
- Siparişleriniz hakkında grafiksel ve indirilebilir bir biçimde bilgi için, [Ticari Pazar analitiğinde Siparişler Panosu'na](./orders-dashboard.md)bakın.
- Büyüme eğilimleri de dahil olmak üzere müşterileriniz hakkında ayrıntılı bilgi için [Ticari Pazar analitiğinde Müşteri Panosu'na](./customer-dashboard.md)bakın.
- Son 30 gün içinde indirme isteklerinizin bir listesi için, [Ticari Pazar analizinde İndirme Panosu'na](./downloads-dashboard.md)bakın.
- Azure Marketi ve AppSource'daki teklifler için müşteri geri bildirimlerinin konsolide görünümünü görmek için, [Commercial Marketplace analitiğinde Derecelendirmeler ve incelemeler panosuna](./ratings-reviews.md)bakın.
- Ticari Pazar yeri analitiği hakkında sık sorulan sorular ve kapsamlı bir veri terimleri sözlüğü [için, Ticari Pazar analitiği için sık sorulan sorular ve terminolojiye](./faq-terminology.md)bakın.
