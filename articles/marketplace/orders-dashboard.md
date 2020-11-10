---
title: Ticari Market Analytics, Microsoft AppSource ve Azure Market 'teki iş ortağı merkezi siparişleri panosu
description: Ticari Market teklif siparişleriniz hakkındaki analitik raporlara grafik ve indirilebilir bir biçimde nasıl erişebileceğinizi öğrenin.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: article
ms.date: 11/09/2020
author: mingshen-ms
ms.author: mingshen
ms.openlocfilehash: 2682202b68deeeb1dbbe2493ecb9592acd1788b8
ms.sourcegitcommit: 17b36b13857f573639d19d2afb6f2aca74ae56c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2020
ms.locfileid: "94415355"
---
# <a name="orders-dashboard-in-commercial-marketplace-analytics"></a>Ticari market analizinde Siparişler panosu

Bu makalede, Iş Ortağı Merkezi 'nde siparişler panosu hakkında bilgi sağlanmaktadır. Bu Pano, bir grafik ve indirilebilir biçimde sunulan büyüme eğilimleri dahil olmak üzere siparişleriniz hakkındaki bilgileri görüntüler.

İş ortağı merkezindeki siparişler panosuna erişmek için **ticari Market** ' in altında siparişleri **[Çözümle](https://partner.microsoft.com/dashboard/commercial-marketplace/analytics/summary)** ' yi seçin  >  **Orders**.

>[!NOTE]
> Analiz terminolojisi hakkında ayrıntılı tanımlamalar için bkz. [ticari Market Analytics terminolojisi ve sık sorulan sorular](./partner-center-portal/faq-terminology.md).

## <a name="orders-dashboard"></a>Siparişler panosu

Siparişler panosu, tüm hizmet olarak yazılım (SaaS) teklifleri için geçerli siparişleri görüntüler. Aşağıdaki öğelerin grafik sunumlarını görüntüleyebilirsiniz:

- Sipariş eğilimi
- Bilgisayar ve site eğilimi başına siparişler
- Tekliflere ve SKU 'Lara göre siparişler
- Coğrafya tarafından siparişler
- Ayrıntılı Siparişler tablosu
- Sipariş sayfası filtreleri

> [!NOTE]
> Iş Ortağı Merkezi 'nde müşteri alımı ve raporlama arasındaki maksimum gecikme süresi 48 saattir.

## <a name="elements-of-the-orders-dashboard"></a>Siparişler panosu öğeleri

Aşağıdaki bölümlerde, siparişler panosunun nasıl kullanılacağı ve verilerin nasıl okunacağı açıklanır.

### <a name="month-range"></a>Ay aralığı

Her sayfanın sağ üst köşesinde bir ay aralığı seçimi bulabilirsiniz. Son 6 veya 12 aya göre bir ay aralığı seçerek veya en fazla 12 ay süresince özel bir ay aralığı seçerek **siparişler** sayfası grafiklerinin çıktısını özelleştirin. Varsayılan ay aralığı (hesaplama dönemi) altı aydır.

:::image type="content" source="./media/orders-dashboard/month-filters.png" alt-text="Siparişler panosundaki ay filtrelerini gösterir.":::

> [!NOTE]
> Görselleştirme pencere öğeleri ve dışarı aktarma raporlarında tüm ölçümler, Kullanıcı tarafından seçilen hesaplama dönemini kabul ediyor.

### <a name="orders-trend"></a>Sipariş eğilimi

Bu bölümde, seçili hesaplama dönemi için etkin ve iptal edilmiş siparişlerin eğilimini gösteren **siparişler** grafiğini bulacaksınız. Ölçümler ve büyüme eğilimleri bir çizgi grafik tarafından temsil edilir ve grafikteki satırın üzerine gelindiğinde her ay için değeri görüntüler. Pencere öğesi içindeki siparişler ölçümlerinin altındaki yüzde değeri, seçilen hesaplama döneminde büyüme veya reddetme miktarını temsil eder.

İki sipariş sayacı vardır: _etkin_ ve _iptal edildi_.

- **Etkin** , seçili tarih aralığı sırasında müşteriler tarafından kullanılmakta olan siparişlerin sayısına eşittir.
- **Iptal edildi** , daha önce satın alınan siparişlerin sayısına eşittir ve ardından seçilen tarih aralığı sırasında iptal edilir.

[![Siparişler panosunda, etkin ve iptal edilmiş siparişlerin eğilimini gösteren siparişler pencere öğesini gösterir.](./media/orders-dashboard/orders-trend.png)](./media/orders-dashboard/orders-trend.png#lightbox)

### <a name="orders-by-per-seat-and-site-trend"></a>Bilgisayar başına ve site eğilimi bazında siparişler

**Bilgisayar başına ve site tabanlı** çizgi grafik temelinde siparişler, müşteriler tarafından satın alınan site başına SaaS ve bilgisayar başına SaaS siparişlerinin ölçüsünü ve eğilimini temsil eder (Bu grafik iptal edilen siparişleri içerir).

[![Siparişler panosundaki, bilgisayar ve site eğilimi başına düşen siparişleri gösteren siparişler pencere öğesini gösterir.](./media/orders-dashboard/seats-sites.png)](./media/orders-dashboard/seats-sites.png#lightbox)

SaaS teklifleri, her plan ile iki fiyatlandırma modelinden birini kullanabilir: sabit fiyat (site tabanlı) veya Kullanıcı başına (bilgisayar tabanlı).

- **Düz** Fiyat: tek bir aylık veya yıllık sabit fiyat fiyatı ile teklifinizin erişimini etkinleştirin. Bu, bazen site tabanlı fiyatlandırma olarak adlandırılır.
- **Kullanıcı başına** : teklifine erişebilen veya hiç oturmayan kullanıcı sayısına bağlı olarak teklifiniz için fiyata erişimi etkinleştirin. Bu kullanım tabanlı model ile, plan tarafından desteklenen en düşük ve en fazla kullanıcı sayısını ayarlayabilirsiniz. Kullanıcı sayısına göre farklı fiyat noktalarını yapılandırmak için birden çok plan oluşturabilirsiniz. Bu alanlar isteğe bağlıdır. Sol tarafta seçili değilse, Kullanıcı sayısı bir sınıra sahip değildir (en az 1 ve en fazla, hizmetinizin destekleyebiliyor) olarak yorumlanır. Bu alanlar, planınızdaki bir güncelleştirmenin parçası olarak düzenlenebilir.
- **Ölçülen faturalandırma** : sabit fiyat fiyatlandırmasının üstünde. Bu fiyatlandırma modeliyle isteğe bağlı olarak, sabit fiyat kapsamında olmayan kullanımlar için müşterileri ücretlendirmede Market ölçüm hizmeti API 'sini kullanan tarifeli planlar tanımlayabilirsiniz.

Bilgisayar, site ve tarifeli tabanlı faturalandırma hakkında daha fazla bilgi için bkz. [ticari Market Için SaaS teklifi planlaması](plan-saas-offer.md).

### <a name="orders-by-offers-and-skus"></a>Tekliflere ve SKU 'Lara göre siparişler

Tekliflere ve SKU şemasına göre siparişler, tüm tekliflerin ölçülerini ve eğilimlerini gösterir:

- En üstteki teklifler grafikte görüntülenir ve tekliflerin geri kalanı **rest** olarak gruplandırılır.
- Yalnızca bu teklifi ve grafikteki ilişkili SKU 'Ları göstermek için göstergede belirli teklifleri seçebilirsiniz.
- Grafikteki bir dilimin üzerine gelindiğinde, tüm tekliflerde toplam sipariş sayısı ile karşılaştırıldığında, bu teklifin sipariş sayısı ve yüzdesi görüntülenir.
- **Siparişlere göre siparişler eğilimi** , aylık büyüme eğilimlerini görüntüler. Month sütunu, teklif adına göre sipariş sayısını temsil eder. Çizgi grafik, z ekseni üzerinde çizilen büyüme yüzdesi eğilimini görüntüler.

:::image type="content" source="./media/orders-dashboard/orders-by-offers.png" alt-text="Siparişler panosunda, tekliflere göre siparişler grafiğini gösterir.":::

Teklif, SKU 'Lar ve koltuk için aylık aylık eğilimi görüntülemek üzere teklif ve en fazla üç SKU 'yu seçebilirsiniz.

### <a name="orders-by-geography"></a>Coğrafya tarafından siparişler

Seçilen hesaplama dönemi için ısı haritasını, toplam sipariş sayısını ve yeni eklenen siparişlerin büyüme yüzdesini bir Coğrafya üzerinden görüntüler.  Haritadaki koyu renkli bir renk, müşteri sayısının yüksek değerini gösterir. Belirli bir ülkede veya bölgede yakınlaştırmak için tablodaki bir kaydı seçin.

[![Siparişler panosundaki coğrafi forma grafik gösterir.](./media/orders-dashboard/geographical-spread.png)](./media/orders-dashboard/geographical-spread.png#lightbox)

Şunlara dikkat edin:

- Tam konumu görüntülemek için Haritayı taşıyabilirsiniz.
- Belirli bir konuma yakınlaştırabilirsiniz.
- Isı haritasını 'in, belirli bir konum için müşteri sayısı, sipariş sayısı ve normalleştirilmiş kullanım saatlerinin ayrıntılarını görüntülemek için bir ek kılavuz vardır.
- Haritada yer alan bir ülke/bölge arayabilir ve seçebilirsiniz. Haritadaki **giriş** düğmesini seçerek özgün görünüme dönün.

### <a name="orders-details-table"></a>Sipariş Ayrıntıları tablosu

Sipariş Ayrıntıları tablosu, Alım tarihine göre sıralanan 1.000 üst siparişin numaralandırılmış bir listesini görüntüler.

- Kılavuzdaki her bir sütun sıralanabilir.
- Veriler bir öğesine ayıklanabilir. CSV veya. TSV dosyası 1.000 ' den küçükse, kayıt sayısı.
- 1.000 üzerinden kayıt numarası varsa, verilen veriler sonraki 30 güne ait bir indirmeler sayfasına zaman uyumsuz olarak yerleştirilir.
- Yalnızca ilgilendiğiniz verileri göstermek için **sipariş ayrıntıları** tablosuna filtre uygulayın. Ülke/bölge, Azure lisans türü, ticari Market lisans türü, teklif türü, sipariş durumu, ücretsiz izler, ticari Market abonelik KIMLIĞI, müşteri KIMLIĞI ve şirket adına göre filtreleyin.
- Azure Market veya Microsoft AppSource aracılığıyla satın alınan SaaS teklifleri için Azure aboneliği gerektirmeyin, Market abonelik KIMLIĞI **ayrıntılı siparişler verileri** bölümünde 00000000-0000-0000-0000-000000000000 biçiminde görünür.
- Bir sipariş, korunan bir müşteri tarafından satın alındığında, **ayrıntılı verilerin emirdeki** bilgileri maskelenir (* * * * * * * * * * * *).

**_Tablo 1: veri koşullarının sözlüğü_* _

| Sütun adı | Öznitelik adı | Tanım |
| ------------ | ------------- | ------------- |
| Market abonelik kimliği | Market abonelik KIMLIĞI | Ticari Market teklifinizi satın almak için kullanılan müşterinin Azure aboneliğiyle ilişkili benzersiz tanımlayıcı. KIMLIĞI daha önce Azure aboneliği GUID 'iydi. |
| MonthStartDate | Ay başlangıç tarihi | Ayın başlangıç tarihi, satın alma ayı ile temsil eder. |
| Teklif Türü | Teklif Türü | Ticari Market teklifi türü. |
| Azure lisans türü | Azure lisans türü | Müşteriler tarafından Azure satın alma için kullanılan lisans sözleşmesinin türü. Kanal olarak da bilinir. Olası değerler şunlardır:<ul><li>Bulut çözümü sağlayıcısı</li><li>Enterprise</li><li>Satıcı aracılığıyla kurumsal</li><li>Kullandıkça öde</li></ul> |
| Market lisans türü | Market lisans türü | Ticari Market teklifinin faturalandırma yöntemi. Farklı değerler şunlardır:<ul><li>Azure üzerinden faturalandırılır</li><li>Kendi lisansınızı getirin</li><li>Ücretsiz</li><li>Satıcı olarak Microsoft</li></ul> |
| SKU | SKU | Teklifle ilişkili plan |
| Müşteri ülkesi | Müşteri ülkesi/bölgesi | Müşterinin sunduğu ülke/bölge adı. Ülke/bölge, müşterinin Azure aboneliğindeki ülke/bölgeden farklı olabilir. |
| Önizleme SKU 'SU | Önizleme SKU 'SU | Bu değer, SKU 'YU "Önizleme" olarak etiketlediyseniz size bilgi verir. SKU buna uygun şekilde etiketlenmişse değer "Evet" olur ve yalnızca sizin tarafından yetkilendirilen Azure abonelikleri bu görüntüyü dağıtabilir ve kullanabilir. SKU "Önizleme" olarak tanımlanmamışsa, değer "Hayır" olacaktır. |
| Sipariş kimliği | Sipariş kimliği | Ticari Market hizmetiniz için müşteri siparişinin benzersiz tanımlayıcısı. Sanal makine kullanımı tabanlı teklifler bir siparişle ilişkili değildir. |
| Sipariş Miktarı | Sipariş Miktarı | Etkin siparişler için sipariş KIMLIĞIYLE ilişkili varlıkların sayısı |
| Bulut örneği adı | Bulut örneği adı | Bir VM dağıtımının gerçekleştiği Microsoft Bulut. |
| Yeni müşteri | Yeni müşteri | Bu değer, yeni bir müşterinin bir veya daha fazla teklifinizden ilk kez elde edilip edilmeyeceğini tanımlar. "Tarihi alındı" için aynı takvim ayı içinde değer "Evet" olacaktır. Müşteri tekliflerinizin herhangi birini bildirilen takvim ayına göre satın alıyorsa değer "Hayır" olacaktır. |
| Sipariş Durumu | Sipariş Durumu | Verilerin son yenilenme sırasındaki ticari Market siparişi durumu. |
| Sipariş Iptali tarihi | Sipariş Iptali tarihi | Ticari Market siparişinin iptal edildiği tarih. |
| Müşteri şirket adı | Müşteri şirket adı | Müşterinin sunduğu şirket adı. Ad, müşterinin Azure aboneliğindeki şehirden farklı olabilir. |
| Sipariş satın alma tarihi | Sipariş satın alma tarihi | Ticari Market siparişinin oluşturulduğu tarih. |
| Teklif Adı | Teklif Adı | Ticari Market sunumunun adı. |
| Deneme bitiş tarihi | Deneme bitiş tarihi | Bu sipariş için deneme döneminin sona erdiği veya sonlandırmış olacağı tarih. |
| Müşteri kimliği | Müşteri Kimliği | Müşteriye atanan benzersiz tanımlayıcı. Müşterinin sıfır veya daha fazla Azure Market aboneliği olabilir. |
| Faturalandırma hesabı kimliği | Faturalandırma hesabı KIMLIĞI | Faturalandırma 'nin oluşturulduğu hesabın tanımlayıcısı. Ödeme Işlem raporunuzu müşteri, sipariş ve kullanım raporlarıyla bağlamak için _ *faturalandırma HESABı kimliği* * ' yi **CustomerID** olarak eşleştirin. |
| AssetCount | Varlık sayısı | Sipariş KIMLIĞIYLE ilişkili varlıkların sayısı. |
||||

### <a name="orders-page-filters"></a>Sipariş sayfası filtreleri

**Siparişler** sayfası filtreleri, siparişler sayfa düzeyinde uygulanır. Görüntülemek istediğiniz ölçütlere ve ' ayrıntılı siparişler verileri ' kılavuzunda/dışarı aktarmada görmek istediğiniz verilere ilişkin grafiği işlemek için bir veya birden çok filtre seçebilirsiniz. Filtreler, siparişler sayfasının sağ üst köşesinde seçtiğiniz ay aralığı için ayıklanan verilere uygulanır.

> [!TIP]
> Verileri indirmek için herhangi bir pencere öğesinin sağ üst köşesinde bulunan İndir simgesini kullanabilirsiniz. "Thumbs up" veya "thumbs aşağı" simgesine tıklayarak Pencere öğelerinin her biri hakkında geri bildirim sağlayabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- Ticari Market 'te bulunan analiz raporlarına genel bakış için bkz. [Partner Center 'da ticari Market için analitik raporlara erişme](./partner-center-portal/analytics.md).
- Teklifinizin Market etkinliklerini özetleyen grafikler, eğilimler ve veri değerleri için bkz. [ticari Market Analytics 'Te Özet Panosu](./summary-dashboard.md).
- Grafik ve indirilebilir biçimdeki Siparişleriniz hakkında daha fazla bilgi için bkz. [Orders Dashboard in The Commercial Market Analytics](orders-dashboard.md).
- Sanal makine (VM) için kullanım ve ölçümlenen faturalandırma ölçümleri, bkz. [ticari Market Analytics 'Te kullanım panosu](./usage-dashboard.md).
- Son 30 güne ait indirme isteklerinizin listesi için bkz. [Commercial Market Analytics 'te panoyu indirme](./partner-center-portal/downloads-dashboard.md).
- Azure Market ve AppSource 'ta teklifler için müşteri geri bildirimlerinin birleştirilmiş bir görünümünü görmek için bkz. [Iş Ortağı Merkezi 'Nde derecelendirmeler & İncelemeleri analiz panosu](./partner-center-portal/ratings-reviews.md).
- Ticari Market Analytics hakkında sık sorulan sorular ve veri koşullarının kapsamlı bir sözlüğü için bkz. [ticari Market Analytics terminolojisi ve sık sorulan sorular](./partner-center-portal/faq-terminology.md).
