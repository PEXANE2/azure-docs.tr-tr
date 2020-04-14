---
title: Commercial Marketplace'te İş Ortağı Merkezi analitiği için özet panosu
description: İş Ortağı Merkezi'ndeki Özet panosundan pazar yeri etkinliğini özetleyen toplu verilerin grafiklerine, eğilimlerine ve değerlerine nasıl erişilenleri öğrenin.
author: dsindona
ms.author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 12/11/2019
ms.openlocfilehash: 86bce85c6ec273b4ab5f9f00cbae68fc054f53f5
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81262409"
---
# <a name="summary-dashboard-in-commercial-marketplace-analytics"></a>Ticari pazar analizinde özet pano

Bu makale, İş Ortağı Merkezi'ndeki Özet panosu hakkında bilgi sağlar. Bu pano, tekliflerinizin pazar daki etkinliğini özetleyen toplu verilerin grafiklerini, eğilimlerini ve değerlerini görüntüler.

Özet panosuna erişmek için Ticari Pazar Yeri altında **[Analiz panosunu](https://partner.microsoft.com/dashboard/commercial-marketplace/analytics/summary)** açın.

>[!NOTE]
> Analitik terminolojinin ayrıntılı tanımları [için, ticari pazar analitiği için sık sorulan sorular ve terminolojiye](./faq-terminology.md)bakın.

## <a name="summary-dashboard"></a>Özet panosu

**Özet** panosu, her teklif türüne göre genel bir bakış sunar. **Öngörüler** kritik bilgileri bir bakışta gösterir ve tekliflerinizin satış etkinliğine geniş bir bakış sağlar. **Özet** panosunu kullanarak bu raporları görselleştirebilirsiniz. Bu makalede, aşağıdaki öğelerin her biri hakkında daha fazla ayrıntı gider:

- [Tarih aralığı](#date-range)
- [Özet bölümü](#summary-section)
- [Coğrafyaya göre müşteriler](#customers-by-geography)
- [Büyüme eğilim grafikleri](#growth-trend-charts)
- [Müşteri lider panosu](#customer-leaderboard)
- [Koltuk sayısı eğilimi](#seat-count-trend)
- [Ücretsiz denemeler SaaS siparişleri eğilim](#free-trials-saas-orders-trend)

## <a name="elements-of-the-summary-dashboard"></a>Özet panosunun öğeleri

Aşağıdaki bölümlerde özet panosunun nasıl kullanılacağı ve verilerin nasıl okunduğu açıklanmıştır.

### <a name="date-range"></a>Tarih aralığı

Her sayfanın sağ üst köşesinde bir tarih aralığı seçimi bulabilirsiniz. Son 3, 6 veya 12 aya dayalı bir tarih aralığı seçerek veya en fazla 12 aylık süreye sahip özel bir tarih aralığı seçerek **Özet** sayfa grafiklerinin çıktısını özelleştirin. Varsayılan tarih aralığı altı aydır.

![İş Ortağı Merkezi Analiz panosu](./media/analyze-dashboard.png)

### <a name="summary-section"></a>Özet bölümü

Özet bölümü, seçilen tarih aralığında oluşturulan tüm siparişlerin, edinilen müşterilerin ve bildirilen kullanımların sayısını görüntüler. Kısmi geçerli ay bu ölçümlerin hesaplamadışında olacaktır. Örneğin: 6M zaman dilimini seçtiyseniz, kullanım saatleri geçerli aydan önceki altı ay için hesaplanır. Özel bir tarih aralığı seçilirse, geçerli aydan kısmi bir tutar hesaplamanın dışında tutulur.

![Özet panosunda büyüme eğilimleri](./media/summary-summary-section.png)

#### <a name="reading-the-summary-section"></a>Özet bölümünü okuma

- **Siparişler**: Şimdiye kadar yayınladığınız teklifler için satın alınan tüm siparişlerin sayısı (iptal edilen siparişler hariç).
- **Müşteriler**: Tekliflerinizi satın alan ve en az bir tane iptal edilmeyen siparişi olan tüm müşterileri say.
- **Normalleştirilmiş kullanım saatleri**: VM çekirdeği ([VM çekirdek sayısı] x [ham kullanım saatleri]) sayısına göre normalleştirilmiş kullanım saatleri olarak tanımlanır. "SHAREDCORE" olarak atanan VM'ler 1/6 (veya 0,1666) [VM çekirdek] çarpanı olarak kullanır.
- **Ham kullanım saatleri**: VM'lerin saat bazında çalışma süresi. **Toplam siparişlerin**yanındaki yüzde değeri, **toplam müşteri,** **normalleştirilmiş kullanım saatleri,** ham **kullanım saatleri,** **sayfa ziyaretleri**ve **arama eylemleri,** seçilen tarih aralığı ([son ay kullanımı – ilk ay kullanımı])/ ilk ay kullanımı için kullanım artışı miktarını temsil eder. Yukarıda açıklandığı gibi, geçerli ayın kısmi tutarı bu ölçünün dışında tutulur.
- **Büyüme eğilimleri**: Grafiğin sütunlarının üzerinde gezinirseniz, çubuk grafikler her ayın değerini görüntüler.
- **Yeşil üçgen yukarı işaret**: Olumlu bir büyüme eğilimi gösterir.
- **Aşağı yönlü işaret eden kırmızı üçgen**: Bir önceki aya göre negatif büyüme eğilimini gösterir.

### <a name="customers-by-geography"></a>Coğrafyaya göre müşteriler

Coğrafya ısı **haritasına göre Müşteriler,** bir dünya haritasında müşteri sayısını görüntüler.

![Özet panosunda coğrafyaya göre müşteriler](./media/summary-customers-by-geography.png)

- Haritayı tam konumu görüntülemek için taşıyabilirsiniz.
- Belirli bir konumu yakınlaştırabilirsiniz.
- Isı haritası, belirli bir konumda müşteri sayısı, sipariş sayısı, normalleştirilmiş kullanım saatleri ayrıntılarını görüntülemek için tamamlayıcı bir ızgara vardır.
- Haritadaki konumu yakınlaştırmak için ızgarada bir ülkeyi arayabilir ve seçebilirsiniz. Haritadaki **Ana Ekran** düğmesine basarak orijinal görünüme geri dön.
- **Yeni** bir müşteri, seçilen tarih aralığında ay boyunca ilk kez tekliflerinizden birini satın aldı.

### <a name="growth-trend-charts"></a>Büyüme eğilim grafikleri

Satın **alınan siparişlerinizin** büyümesine (iptal edilen siparişleri içerir), **satın alınan müşterilerin** (kayıp müşterileri içerir) ve seçilen tarih aralığına göre ay ay görüntülenen **kullanım** durumuna bağlı olarak eğilimleri görüntüleyebilirsiniz. Bu eğilimleri, ilgili **sipariş,** **kullanım,** **müşteri**veya **Market İstatistikleri** sayfalarına yönlendiren grafiğin altındaki bağlantıları seçerek daha da analiz edebilirsiniz.

Market, Azure pazar yeri ve AppSource için iki sekmede **Sayfa ziyaretleri ve eylem çağrısı** eğilim grafikleri sunar.

![Özet panosunda sayfa ziyaretleri ve eylemlere çağrı eğilim grafikleri](./media/summary-page-visits-and-cta.png)

**Tekliflerin grafiğindeki siparişler,** siparişlerinizi Teklif adına göre düzenler.

Satış kanalı pasta **grafiğine göre siparişler,** seçilen tarih aralığında, Satış kanalına göre siparişlerinizi (müşterilerin iptal ettiği siparişler dahil) düzenler. Satış kanalı, müşterilerin Azure satın almak için kullandıkları bulut çözüm sağlayıcısı (CSP), Enterprise, Enterprise by Reseller, GTM ve Kullandıkça Öde satın almak için kullanılan lisans sözleşmesi türüdür.

**Tekliflerin kullanımı** ve satış kanalı pasta çizelgeleri **tarafından kullanımı,** sırasıyla üst teklifler ve satış kanalları tarafından kullanım dökümünü sunar. İç pasta grafiği ham kullanımı, dış pasta grafiği ise normalleştirilmiş kullanımı temsil eder.

**Pazar yeri lisans türüne göre verilen siparişler** ve pazar yeri lisans türü pasta çizelgelerine göre **kullanım,** siparişlerin ve kullanımlarının ilgili lisans türüne göre dökümünü görüntüler. Lisans türleri şunlardır:

- **Azure üzerinden faturalandırılır**: Microsoft, teklifinizi Microsoft üzerinden bu lisans türüyle satmayı seçtiğinizde müşterilerinizi sizin adınıza fatura eder. Ödeme türleri arasında kredi kartı veya Kurumsal faturalama yoluyla kullandıkça öde dahildir.
- **Kendi lisansınızı getirin**: Microsoft, bu tür pazar teklifiyle müşterilerine kullanımları için fatura landırmaz. Bu kullanım, pazaryerinde **şimdi (Ücretsiz) alın** olarak listelenir.
- **Ücretsiz**: Microsoft, bu tür pazar teklifiyle müşterilerine kullanımları için fatura landırmaz. Bu kullanım, pazarda **Ücretsiz deneme** olarak listelenir.
- **Microsoft satıcı olarak**: Microsoft satıcıları tarafından **Bulut Çözüm Sağlayıcısı (CSP) programının**bir parçası olarak satılan teklifleri temsil eder.

### <a name="customer-leaderboard"></a>Müşteri lider panosu

En yüksek sipariş sayısına sahip en iyi 50 müşteri, en yüksek sipariş sayısı ve sipariş yüzdesiyle sıralanmış bir *lider panosunda*görüntülenir.

- Profil ayrıntılarını, teklife göre düzenlenen siparişleri veya Azure lisans türü ve fiyatlandırma kanalına göre düzenlenen siparişleri görüntülemek için bir müşteri seçin.
- Siparişler donut grafiğine göre **Teklifler** ilk dört teklifi (sipariş sayısına göre) ve geri kalan teklifleri 'Rest All' olarak gruplandırılır sunar.
- Teklif donut grafiği **ile normalleştirilmiş kullanım,** kullanıma göre ilk beş teklifi sunar.

> [!NOTE]
> Müşterinin kişisel bilgileri yalnızca müşterinin rızası nı sağlamışsa sunulacaktır. **Bir Owner** rol izinleri düzeyi ile oturum açtıysanız bu bilgileri görüntüleyebilirsiniz. **Katılımcı** rolüne sahip kullanıcılar bu bilgileri görüntüleyemez. [Kullanıcı rolleri ve izinleri hakkında daha fazla bilgi edinin.](./manage-account.md#define-user-roles-and-permissions)

### <a name="seat-count-trend"></a>Koltuk Sayısı Trendi

**Koltuk başına/ site başına** grafik başına verilen siparişler, fiyatlandırma modeline göre satın alınan tüm siparişlerin dökümünü sunar. **Koltuk sayısı trend** grafiği, koltuk başına tüm Yazılımlar için satın alınan siparişlere karşılık koltuklar sunar Hizmet (SaaS) sunar.

### <a name="free-trials-saas-orders-trend"></a>Ücretsiz denemeler SaaS siparişleri eğilim

**Ücretsiz deneme SaaS siparişleri trend** grafiği, SaaS'in 30 günlük deneme süresiyle sunduğu ücretsiz denemeler için sipariş trendini sunar.

## <a name="next-steps"></a>Sonraki adımlar

- İş Ortağı Merkezi ticari pazarda bulunan analiz raporlarına genel bir bakış için, [İş Ortağı Merkezi'ndeki ticari pazar için Analytics'e](./analytics.md)bakın.
- Siparişleriniz hakkında grafiksel ve indirilebilir bir biçimde bilgi için, [ticari pazar analizinde Siparişler Panosu'na](./orders-dashboard.md)bakın.
- Sanal Makine (VM) kullanım ve ölçülü fatura ölçümleri için, [ticari pazar analizinde Kullanım Panosu'na](./usage-dashboard.md)bakın.
- Büyüme eğilimleri de dahil olmak üzere müşterileriniz hakkında ayrıntılı bilgi için [ticari pazar analizinde Müşteri Panosu'na](./customer-dashboard.md)bakın.
- Son 30 gün içinde indirme isteklerinizin bir listesi için, [ticari pazar analizinde İndirme Panosu'na](./downloads-dashboard.md)bakın.
- Azure Marketi ve AppSource'daki teklifler için müşteri geri bildirimlerinin konsolide görünümünü görmek için, [ticari pazar analizinde Derecelendirmeler ve incelemeler panosuna](./ratings-reviews.md)bakın.
- Ticari pazar analitiği hakkında sık sorulan sorular ve kapsamlı bir veri terimleri sözlüğü [için, ticari pazar analitiği için sık sorulan sorular ve terminolojiye](./faq-terminology.md)bakın.
