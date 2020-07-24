---
title: Microsoft ticari Market Analytics, Azure Marketi ve Microsoft AppSource kullanım panosu
description: Tüm VM 'ler için kullanım ve ölçümlenen faturalandırma ölçümlerinin nasıl erişebileceğini öğrenin. Ticari Market altında Iş Ortağı Merkezi ' ndeki kullanım panosuna gidin.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: article
ms.date: 07/22/2020
ms.openlocfilehash: a03ef316b409fdeff687f0ee7bbbf6316857c54b
ms.sourcegitcommit: 0e8a4671aa3f5a9a54231fea48bcfb432a1e528c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87131755"
---
# <a name="usage-dashboard-in-microsoft-commercial-marketplace-analytics"></a>Microsoft ticari Market Analytics 'teki kullanım panosu

Bu makalede, Iş Ortağı Merkezi 'ndeki kullanım panosu hakkında bilgi sağlanır. Bu Pano, tüm VM 'leri kullanım ve ölçümlenen faturalandırma ölçümlerini iki ayrı sekmede görüntüler: VM kullanımı ve tarifeli faturalandırma kullanımı.

Kullanım panosuna erişmek için **ticari Market**altında **[analiz](https://partner.microsoft.com/dashboard/commercial-marketplace/analytics/summary)** panosunu açın.

>[!NOTE]
> Analiz terminolojisi hakkında ayrıntılı tanımlamalar için bkz. [ticari Market Analizi Için sık sorulan sorular ve terminoloji](./faq-terminology.md).

## <a name="usage-dashboard"></a>Kullanım panosu

Kullanım panosu, tüm sanal makineler (VM) için ölçümleri temsil eder ve kullanım ve tarifeli faturalandırma kullanımı sunar. Bu ölçümler iki ayrı sekmede bulunur: VM kullanımı ve tarifeli faturalandırma kullanımı.

VM kullanımı sekmesinde, aşağıdaki öğelerin grafik gösterimleri vardır:

- [Kullanım Özeti](#usage-summary)
- [Coğrafya tarafından kullanım](#usage-by-geography)
- [Tekliflere göre kullanım](#usage-by-offers)
- [Tekliflere ve planlara göre kullanım eğilimi](#usage-trend-by-offers-and-plans)
- [Teklif türüne göre kullanım](#usage-by-offer-type)
- [VM boyutuna göre kullanım](#usage-by-vm-size)
- [Satış kanalına göre kullanım](#usage-by-sales-channel)
- [Ayrıntılı kullanım verileri](#detailed-usage-data)

Iş Ortağı Merkezi 'nde kullanım olayı oluşturma ve raporlama arasındaki en uzun gecikme süresi 48 saattir.

### <a name="usage-summary"></a>Kullanım özeti

Kullanım Özeti tablosu, satın aldıkları tüm teklifler için müşterinin kullanım saatlerini görüntüler.

- Normalleştirilmiş kullanım saatleri, sanal makine çekirdekleri ([VM çekirdeği sayısı] x [saat ham kullanım]) için hesaba normalleştirilmiş kullanım saatleri olarak tanımlanır. "SHAREDCORE" olarak belirlenen VM 'Ler [VM çekirdeklerinin sayısı] çarpanı olarak 1/6 (veya 0,1666) kullanır.
- Ham kullanım saatleri, VM 'Lerin saat cinsinden çalıştığı zaman sayısı olarak tanımlanır.
- Yüzde değeri, seçilen tarih aralığı için kullanım büyüme değişikliğini ([son ay kullanımı – ilk ay kullanımı])/ilk ayın kullanımını temsil eder.
- Yukarıyı gösteren yeşil üçgenler, büyüme değişikliğini gösterir.
- Aşağıyı gösteren kırmızı üçgen, önceki aya göre negatif büyüme değişikliğini gösterir.
- Mikro çubuk grafikleri aylık değerleri temsil eder. Grafik içindeki sütunların üzerine gelindiğinde her ayın değerini görüntüleyebilirsiniz.

### <a name="usage-by-geography"></a>Coğrafya tarafından kullanım

Coğrafya ısı haritası **tarafından normalleştirilmiş kullanım** , müşteri ülkesine/bölgesine göre eşlenmiş kullanım saatlerini görüntüler. Ülke/bölge renk çeşitlemesi normalleştirilmiş kullanım yoğunluğu temsil eder. Haritadaki **giriş** düğmesine basarak özgün görünüme dönün.

### <a name="usage-by-offers"></a>Tekliflere göre kullanım

- Bir pasta grafiği sunarak **normalleştirilmiş kullanım** , seçilen tarih aralığına göre tekliflere göre normalleştirilmiş kullanım saatlerinin dökümünü görüntüler. İlk beş teklif grafikte görüntülenir, diğerleri ise **rest All** kategorisinde gruplandırılır.
- Çubuk grafik, seçilen tarih aralığı için aylık bir ay büyüme eğilimi gösterir. Month sütunları, ilgili ay için en yüksek kullanım saatlerine sahip tekliflerden kullanım saatlerini temsil eder. Çizgi grafik, ikincil Y ekseni üzerinde çizilen büyüme yüzdesi eğilimini gösterir.
- Sağ üst kısımdaki kaydırıcıyı kullanarak x ekseni üzerinde sola kaydırın ve/veya belirli veri noktalarına odaklayın.

### <a name="usage-trend-by-offers-and-plans"></a>Tekliflere ve planlara göre kullanım eğilimi

Bu grafik, bir teklifin seçili planları (eskiden SKU olarak adlandırılır) için normalleştirilmiş kullanımın eğilimini görüntüler. Teklif teklifi panosu, en yüksek kullanım için kullanım saatlerine göre sıralanan ilk 50 teklifi görüntüler. Plan öncü tablosu, seçili teklif için en yüksek kullanım olan 50 planı görüntüler.

### <a name="usage-by-offer-type"></a>Teklif türüne göre kullanım

- **Teklif türü** pasta grafiğinin kullanımı, kullanımı teklif türüne göre düzenler.
- Üstteki teklifler grafikte görüntülenir ve tekliflerin geri kalanı ' rest All ' olarak gruplandırılır.
- **Eğilim** grafiği aylık büyüme eğilimlerini görüntüler. Month sütunu, söz konusu aydaki en üst teklif türlerine göre kullanımı temsil eder.

### <a name="usage-by-vm-size"></a>VM boyutuna göre kullanım

Bu grafik, tüm tekliflerinizin/planlarınızın seçili VM boyutları (en fazla beş) için kullanım eğilimi gösterir. Sütun grafiği, seçilen VM boyutlarının kullanım saatleriyle yığılır.

Puan tablosu en yüksek kullanım ve kullanım saatlerine göre sıralanan ilk 50 VM boyutunu görüntüler.

### <a name="usage-by-sales-channel"></a>Satış kanalına göre kullanım

- Satış kanalı pasta grafiğinin kullanımı, kullanımı Satış kanalına göre düzenler
- En yüksek kullanımı olan en üst satış kanalı grafikte görüntülenir ve satış kanalının geri kalanı ' rest All ' olarak gruplandırılır.
- Month sütunu, söz konusu aydaki en üst satış kanalına göre kullanımı temsil eder.
- Bu grafiğin özellikleri ' tekliflere göre kullanım ' grafiğine göre aynıdır

### <a name="detailed-usage-data"></a>Ayrıntılı kullanım verileri

**Kullanım ayrıntıları tablosu** , kullanıma göre sıralanan ilk 1000 kullanım kaydının numaralandırılmış bir listesini görüntüler.

- Kılavuzdaki her bir sütun sıralanabilir.
- Kayıt sayısı 1000 'den küçükse, veriler bir CSV dosyasına ayıklanabilir.
- Kayıt sayısı 1000 ' den fazla ise, dışarı aktarma verileri, sonraki 30 gün boyunca kullanılabilecek bir indirmeler sayfasına zaman uyumsuz olarak yerleştirilir.
- Yalnızca ilgilendiğiniz verileri göstermek için **ayrıntılı kullanım verilerine** filtre uygulayın. Verileri ülkeye/bölgeye, Satış kanalına, Market lisans türüne, kullanım türüne, teklif adına, teklif türüne, ücretsiz deneme, Market abonelik KIMLIĞI, müşteri KIMLIĞI ve şirket adına göre filtreleyin.

> [!NOTE]
> Sayfadaki grafikleri "normalleştirilmiş görünüm" veya "ham görünüm" olarak görüntülemek için sayfa filtresindeki **Kullanım türünü** seçin. Bu grafiklerin varsayılan görünümü "normalleştirilmiş görünüm" dir.

**Kullanım sayfası filtreleri** sayfa düzeyinde uygulanır. Görüntülenecek ölçütler için grafiği işlemek üzere birden çok filtre seçebilirsiniz ve "ayrıntılı kullanım verileri" kılavuzunda/dışarı aktarma bölümünde görüntülenmesini istediğiniz verileri seçin. Filtreler, siparişler sayfasının sağ üst köşesinde seçtiğiniz veri aralığı için ayıklanan verilere uygulanır.

- **Teklif türleri** ve **teklif adları** yalnızca seçili tarih aralığı sırasında elde ettiğiniz teklifler için listelenir. Listedeki teklif adları, listeden seçilen teklif türleri için görüntülenir.
- **Kullanım türü**dışında, filtre seçeneklerinin her biri için varsayılan seçim "tümü" dir. **Kullanım türü** için varsayılan seçim normalleştirilmiş kullanımdır. Grafiklerde ham kullanımı göstermek için "ham kullanım" yi seçin.
- Uygulanan filtreler, yapılan filtre seçimlerinin sayı seçimini gösterir. Uygulanan filtreler varsayılan seçimler için gösterilmez.

> [!NOTE]
> "Ayrıntılı sıralama verileri" kılavuzunda, sayfa filtrelerinde ve tüm olası seçimlerdeki her bir alanın ayrıntılı tanımı, [SSS ve terminoloji](link needed) makalesinin veri sözlüğü bölümünde tanımlanmıştır.

**Ölçümlenen faturalandırma kullanımı** sekmesi, kullanım bilgilerinin ölçüm boyutu başına ölçülmüş olduğu teklif türleri için kullanım bilgilerini sunar. SaaS teklif türü fazla kullanımı Şu anda sunulmuştur. Sekme, SaaS ölçülen faturalama kullanımı için fazla kullanım eğilimlerinin grafiksel sunumlarını sunar:

- **Ölçüm boyutuna göre fazla kullanım eğilimi**: bir teklifin seçili ölçüm boyutu için aylık fazla kullanım eğilimi gösterir. X ekseni ayı temsil eder ve Y ekseni kullanım miktarını temsil eder. Özel ölçerin ölçü birimi Y ekseni üzerinde de görüntülenir.
- **Plana göre fazla kullanım eğilimi**: seçili ölçüm boyutunun kullanım miktarının planlara göre eğilimini temsil eder. Görüntülenecek planlar, seçilen teklif için en yüksek kullanım miktarına sahip olan ilk beş planı temsil eder.
- **Önde gelen 50 müşterilere göre fazla kullanım eğilimi**: en yüksek kullanım saatlerine sahip en iyi 50 teklifleri ***öncü bir panoda*** görüntülenir ve özel ölçerin en yüksek kullanımıyla derecelendirilir. Seçili ölçüm boyutunun kullanım eğilimini görüntülemek için öncü panoda bir müşteri seçin.
- **Popüler müşterilere göre fazla kullanım eğilimi**: genel kullanım yüzdesi ' ne katkıda bulunan en iyi müşteri yüzdebirlik 'leri sunar. Üst müşteri yüzdebirlik 'i X ekseni boyunca ve müşterinin kullanım miktarına göre belirlenir. Y ekseni kullanım miktarını görüntüler. Çizgi grafik boyunca noktaların üzerine gelerek ayrıntıları gösterebilirsiniz.

> [!NOTE]
> Bu sayfadaki kullanım ayrıntıları ve tüm grafikler sayfa filtresi için seçili ölçüm boyutu için görüntülenir.

## <a name="next-steps"></a>Sonraki Adımlar

- Iş ortağı merkezi ticari marketi 'nde bulunan analiz raporlarına genel bakış için bkz. [Iş Ortağı Merkezi 'nde ticari Market Için analiz](./analytics.md).
- Teklifinizin Market etkinliklerini özetleyen grafikler, eğilimler ve veri değerleri için bkz. [ticari Market Analytics 'Te Özet Panosu](./summary-dashboard.md).
- Grafik ve indirilebilir biçimdeki Siparişleriniz hakkında daha fazla bilgi için bkz. [Orders Dashboard in The Commercial Market Analytics](./orders-dashboard.md).
- Büyüme eğilimleri dahil olmak üzere müşterileriniz hakkında ayrıntılı bilgi için bkz. [ticari Market Analytics 'Te müşteri panosu](./customer-dashboard.md).
- Son 30 güne ait indirme isteklerinizin listesi için bkz. [Commercial Market Analytics 'Te panoyu indirme](./downloads-dashboard.md).
- Microsoft AppSource ve Azure Marketi 'ndeki teklifler için müşteri geri bildirimlerine yönelik birleştirilmiş bir görünüm görmek için bkz. [ticari Market Analytics 'Te derecelendirmeler ve İncelemeler panosu](./ratings-reviews.md).
- Ticari Market Analizi ve kapsamlı veri koşulları sözlüğü hakkında sık sorulan sorular için bkz. [ticari Market Analytics Için sık sorulan sorular ve terminoloji](./faq-terminology.md).
