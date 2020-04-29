---
title: Ticari Market 'teki Iş ortağı merkezi analizi için Özet Panosu
description: Iş Ortağı Merkezi 'ndeki Özet panosundan Market etkinliklerini özetleyen toplam veri verilerine, eğilimler ve değerlerine nasıl erişebileceğinizi öğrenin.
author: dsindona
ms.author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 12/11/2019
ms.openlocfilehash: 86bce85c6ec273b4ab5f9f00cbae68fc054f53f5
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81262409"
---
# <a name="summary-dashboard-in-commercial-marketplace-analytics"></a>Ticari Market Analytics 'teki Özet Panosu

Bu makalede, Iş Ortağı Merkezi 'ndeki Özet Panosu hakkında bilgi sağlanmaktadır. Bu Pano, tekliflerinizle ilgili Market etkinliklerini özetleyen grafikleri, eğilimleri ve toplam veri değerlerini görüntüler.

Özet panosuna erişmek için ticari Market altında **[analiz panosunu](https://partner.microsoft.com/dashboard/commercial-marketplace/analytics/summary)** açın.

>[!NOTE]
> Analiz terminolojisi hakkında ayrıntılı tanımlamalar için bkz. [ticari Market Analizi Için sık sorulan sorular ve terminoloji](./faq-terminology.md).

## <a name="summary-dashboard"></a>Özet panosu

**Özet** panosu, her teklif türüne göre genel bir bakış sunar. **Öngörüler** , önemli bilgileri bir bakışta gösterir ve tekliflerinizin satış etkinliğinin geniş bir görünümünü sağlar. **Özet** panosunu kullanarak bu raporları görselleştirebilirsiniz. Bu makale aşağıdaki öğelerin her biri hakkında daha fazla ayrıntıya gider:

- [Tarih aralığı](#date-range)
- [Özet bölümü](#summary-section)
- [Coğrafya ile müşteriler](#customers-by-geography)
- [Büyüme eğilimi grafikleri](#growth-trend-charts)
- [Müşteri puan tablosu](#customer-leaderboard)
- [Koltuk sayısı eğilimi](#seat-count-trend)
- [Ücretsiz deneme SaaS siparişlerinin eğilimi](#free-trials-saas-orders-trend)

## <a name="elements-of-the-summary-dashboard"></a>Özet panosunun öğeleri

Aşağıdaki bölümlerde Özet panosunun nasıl kullanılacağı ve verilerin nasıl okunacağı açıklanır.

### <a name="date-range"></a>Tarih aralığı

Her sayfanın sağ üst köşesinde bir tarih aralığı seçimi bulabilirsiniz. Son 3, 6 veya 12 aya dayalı bir tarih aralığı seçerek veya en fazla 12 ay süresince özel bir tarih aralığı seçerek **Özet** sayfası grafiklerinin çıkışını özelleştirin. Varsayılan tarih aralığı altı aydır.

![İş Ortağı Merkezi panoyu çözümle](./media/analyze-dashboard.png)

### <a name="summary-section"></a>Özet bölümü

Özet bölümü, seçilen tarih aralığı boyunca oluşturulan tüm siparişlerin, alınan müşterilerin ve kullanım sayısının sayısını görüntüler. Bu ölçümlerin hesaplamadan kısmi geçerli ay dışarıda bırakılacak. Örneğin: 6k zaman çerçevesini seçtiyseniz, kullanım saatleri geçerli aydan önceki altı ay için hesaplanır. Özel bir tarih aralığı seçilirse, geçerli aydan kısmi bir miktar hesaplamadan çıkarılır.

![Özet panosundaki büyüme eğilimleri](./media/summary-summary-section.png)

#### <a name="reading-the-summary-section"></a>Özet bölümünü okuma

- **Siparişler**: şimdiye kadar yayımladığınız teklifler için satın alınan tüm siparişlerin sayısı (iptal edilen siparişlerin hariç).
- **Müşteriler**: tekliflerinizi satın alan ve en az bir iptal edilmemiş siparişi olan tüm müşterilerin sayısı.
- **Normalleştirilmiş kullanım saatleri**: VM çekirdeklerinin ([VM çekirdeği sayısı] x [saat ham kullanımı]) hesaba göre normalleştirilmiş kullanım saatleri olarak tanımlanır. "SHAREDCORE" olarak belirlenen VM 'Ler [VM çekirdeklerinin sayısı] çarpanı olarak 1/6 (veya 0,1666) kullanır.
- **Ham kullanım saatleri**: sanal makinelerin saat cinsinden çalışma miktarı. **Toplam sipariş**, **Toplam Müşteri**, **normalleştirilmiş kullanım saati**, **Ham kullanım saati**, **sayfa ziyaretlerinin**yanındaki yüzde değeri, seçilen tarih aralığı ([son ay kullanımı – ilk ay kullanımı])/ilk ay **kullanımı için kullanım büyümesi miktarını temsil eder** . Yukarıda açıklandığı gibi, geçerli ayın kısmi miktarı bu ölçüden çıkarılır.
- **Büyüme eğilimleri**: grafiğin sütunlarının üzerine geldiğinizde çubuk grafiklerde her ay için değer görüntülenir.
- **Yukarıyı gösteren yeşil üçgen**: pozitif bir büyüme eğilimi gösterir.
- **Aşağı dönük kırmızı üçgen**: önceki aya göre negatif büyüme eğilimi gösterir.

### <a name="customers-by-geography"></a>Coğrafya ile müşteriler

Coğrafya ısı haritasını **tarafından müşteriler** bir dünya eşlemesindeki müşteri sayısını görüntüler.

![Özet panosunda coğrafya ile müşteriler](./media/summary-customers-by-geography.png)

- Tam konumu görüntülemek için Haritayı taşıyabilirsiniz.
- Belirli bir konuma yakınlaştırabilirsiniz.
- Isı haritasını 'in, belirli konumdaki müşteri sayısı, sipariş sayısı, normalleştirilmiş kullanım saatlerinin ayrıntılarını görüntülemek için bir ek kılavuz vardır.
- Haritada konum yakınlaştırmak için kılavuzda bir ülke arayabilir ve seçebilirsiniz. Haritadaki **giriş** düğmesine basarak özgün görünüme dönün.
- **Yeni** bir müşteri, seçili tarih aralığında yer alan ilk kez tekliflerinizin birini satın almış.

### <a name="growth-trend-charts"></a>Büyüme eğilimi grafikleri

Eğilimleri, **satın alınan** siparişlerin (iptal edilen siparişler dahil), **müşteriler tarafından alınan müşterilerin** (kayıp müşterileri dahil) ve raporlanan kullanım bildirilen, seçilen tarih aralığına göre aylık olarak görüntülenen **kullanım** artışına göre izleyebilirsiniz. Bu eğilimleri daha fazla analiz ederek grafiğin altındaki bağlantılar ' ı seçerek ilgili **sipariş**, **kullanım**, **Müşteri**veya **Market öngörüleri** sayfalarına gidebilirsiniz.

Market için **sayfa ziyaretlerini ve eylem eğilimi grafiklerine yapılan çağrı** , Azure Market ve appsource için iki sekmede görüntülenir.

![Özet panosunda sayfa ziyaretleri ve eylem eğilimi grafiklerine çağrı](./media/summary-page-visits-and-cta.png)

**Tekliflere göre siparişler** grafiği, siparişlerinizi teklif adına göre düzenler.

**Satış kanalı pasta grafiğine göre siparişler** , Satış kanalına göre, seçili tarih aralığı boyunca siparişlerinizi (müşterilerin iptal ettiği siparişler dahil) düzenler. Satış kanalı, müşteriler tarafından bulut çözümü sağlayıcısı (CSP), kurumsal, Bayi aracılığıyla kurumsal, GTM ve kullandıkça öde gibi Azure 'u satın almak için kullanılan lisans sözleşmesinin türüdür.

**Satış kanalı** pasta grafiklerine göre **teklifleri** ve kullanımları, sırasıyla en önde gelen teklifler ve satış kanallarına göre kullanım dökümünü sunar. İç pasta grafik, ham kullanımı ve dış Pasta grafiğini temsil eder normalleştirilmiş kullanımı temsil eder.

Market lisans türü ve **Market lisans türü** pasta grafiklerine göre **siparişler** , siparişlerin ve kullanımlarındaki ilgili lisans türlerine göre bir dökümünü görüntüler. Lisans türleri şunları içerir:

- **Azure üzerinden faturalandırılır**: Bu lisans türüyle Microsoft aracılığıyla teklifinizi satmayı seçtiğinizde Microsoft, müşterileri sizin adınıza faturalar. Ödeme türleri, kredi kartı veya kurumsal faturalandırma aracılığıyla Kullandıkça Öde 'yi içerir.
- **Kendi lisansınızı getir**: Microsoft, müşterileri bu tür Market teklifiyle kullanım için faturalandırmaz. Bu kullanım, Market 'te **Şimdi al (ücretsiz)** olarak listelenir.
- **Ücretsiz**: Microsoft, müşterileri bu tür Market teklifiyle kullanım için faturalandırmaz. Bu kullanım, Market 'te **ücretsiz deneme** olarak listelenir.
- **Satıcı olarak Microsoft**: **bulut çözümü sağlayıcısı (CSP) programının**bir parçası olarak Microsoft satıcıları tarafından satılan teklifleri temsil eder.

### <a name="customer-leaderboard"></a>Müşteri puan tablosu

En yüksek sayıda siparişi olan ilk 50 müşteri, en yüksek sıra sayısına ve sıra yüzdesine göre sıralanmış bir *öncü panoda*görüntülenir.

- Profil ayrıntılarını, teklif tarafından düzenlenen siparişleri veya Azure lisans türü ve fiyatlandırma kanalı tarafından düzenlenen siparişleri görüntülemek için bir müşteri seçin.
- Yer halka grafik **bazında teklifler** , ilk dört teklifi (order count 'a göre) ve kalan teklifleri ' rest All ' olarak gruplanmış olarak gösterir.
- Halka grafik **sunarak normalleştirilmiş kullanım** en iyi beş teklifi kullanıma sunar.

> [!NOTE]
> Müşterinin kişisel bilgileri yalnızca müşterinin onay sağlamış olması halinde sunulur. **Sahip** rolü izinleri düzeyi ile oturum açtıysanız, bu bilgileri görüntüleyebilirsiniz. **Katkıda** bulunan rolüne sahip kullanıcılar bu bilgileri görüntüleyemeyecektir. [Kullanıcı rolleri ve izinleri hakkında daha fazla bilgi edinin](./manage-account.md#define-user-roles-and-permissions).

### <a name="seat-count-trend"></a>Koltuk sayısı eğilimi

**Bilgisayar başına/site başına siparişler** , fiyatlandırma modeline göre satın alınan tüm siparişlerin dökümünü gösterir. **Bilgisayar sayısı eğilimi** grafiği, bilgisayar başına yazılım (SaaS) teklifleriniz için satın alınan siparişlere karşı siparişler sunar.

### <a name="free-trials-saas-orders-trend"></a>Ücretsiz deneme SaaS siparişlerinin eğilimi

**Ücretsiz deneme SaaS siparişleri eğilim** grafiği, 30 günlük deneme süresi sunan ücretsiz deneme SaaS teklifleri için siparişlerin eğilimini sunar.

## <a name="next-steps"></a>Sonraki adımlar

- Iş ortağı merkezi ticari marketi 'nde bulunan analiz raporlarına genel bakış için bkz. [Iş Ortağı Merkezi 'nde ticari Market Için analiz](./analytics.md).
- Grafik ve indirilebilir biçimdeki Siparişleriniz hakkında daha fazla bilgi için bkz. [Orders Dashboard in The Commercial Market Analytics](./orders-dashboard.md).
- Sanal makine (VM) için kullanım ve ölçümlenen faturalandırma ölçümleri, bkz. [ticari Market Analytics 'Te kullanım panosu](./usage-dashboard.md).
- Büyüme eğilimleri dahil olmak üzere müşterileriniz hakkında ayrıntılı bilgi için bkz. [ticari Market Analytics 'Te müşteri panosu](./customer-dashboard.md).
- Son 30 güne ait indirme isteklerinizin listesi için bkz. [Commercial Market Analytics 'Te panoyu indirme](./downloads-dashboard.md).
- Azure Market ve AppSource 'ta teklifler için müşteri geri bildirimlerinin birleştirilmiş bir görünümünü görmek için bkz. [ticari Market Analytics 'Te derecelendirmeler ve İncelemeler panosu](./ratings-reviews.md).
- Ticari Market Analizi ve kapsamlı veri koşulları sözlüğü hakkında sık sorulan sorular için bkz. [ticari Market Analytics Için sık sorulan sorular ve terminoloji](./faq-terminology.md).
