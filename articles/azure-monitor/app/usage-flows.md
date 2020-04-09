---
title: Azure Uygulama Öngörüleri Kullanıcı Akışları navigasyon akışlarını analiz eder
description: Kullanıcıların web uygulamanızın sayfaları ve özellikleri arasında nasıl gezinip gezinebildiğini analiz edin.
ms.topic: conceptual
author: NumberByColors
ms.author: daviste
ms.date: 01/24/2018
ms.reviewer: mbullwin
ms.openlocfilehash: 8622ede9e6f7fba2fde2e0b2e90eb31520a23d04
ms.sourcegitcommit: df8b2c04ae4fc466b9875c7a2520da14beace222
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80892453"
---
# <a name="analyze-user-navigation-patterns-with-user-flows-in-application-insights"></a>Uygulama Öngörülerinde Kullanıcı Akışları ile kullanıcı gezinti modellerini analiz edin

![Uygulama Öngörüleri Kullanıcı Akışları aracı](./media/usage-flows/00001-flows.png)

Kullanıcı Akışları aracı, kullanıcıların sitenizin sayfaları ve özellikleri arasında nasıl gezinenleri görselleştirir. Bu gibi sorulara cevap için harika:

* Kullanıcılar sitenizdeki bir sayfadan nasıl uzaklaşıyor?
* Kullanıcılar sitenizdeki bir sayfaya ne tıklar?
* Kullanıcıların sitenizden en çok çalkaladığı yerler nerede?
* Kullanıcıların aynı eylemi tekrar tekrar tekrarladığı yerler var mı?

Kullanıcı Akışları aracı, belirttiğiniz ilk sayfa görünümünden, özel olaydan veya özel durumla başlar. Bu ilk olay göz önüne alındığında, Kullanıcı Akışları kullanıcı oturumları sırasında önce ve sonra meydana gelen olayları gösterir. Değişen kalınlıkta çizgiler, her yolun kullanıcılar tarafından kaç kez izlandığını gösterir. Özel **Oturum Başlangıç** düğümleri sonraki düğümlerin oturumun başladığı yeri gösterir. **Oturum Sonu** düğümleri, bir önceki düğümden sonra kaç kullanıcının sayfa görüntülemesi veya özel olaylar göndermedığını göstererek, kullanıcıların sitenizi büyük olasılıkla nerede bıraktığını vurgular.

> [!NOTE]
> Uygulama Öngörüleri kaynağınız, Kullanıcı Akışları aracını kullanmak için sayfa görünümleri veya özel olaylar içermelidir. [Application Insights JavaScript SDK ile sayfa görünümlerini otomatik olarak toplamak için uygulamanızı nasıl ayarlayabilirsiniz](../../azure-monitor/app/javascript.md)öğrenin.
>
>

## <a name="start-by-choosing-an-initial-event"></a>İlk etkinlik seçerek başlayın

![Kullanıcı Akışları için bir başlangıç olayı seçin](./media/usage-flows/00002-flows-initial-event.png)

Kullanıcı Akışları aracıyla soruları yanıtlamaya başlamak için, görselleştirmenin başlangıç noktası olarak hizmet vermek için bir başlangıç sayfası görünümü, özel olay veya özel durum seçin:

1. **Kullanıcılar sonra ne yapar bağlantıtıklayın...?** başlığı, ya da **Edit** düğmesini tıklatın.
2. **İlk olay** açılır sayfasından bir sayfa görünümü, özel olay veya özel durum seçin.
3. **Grafik Oluştur'u**tıklatın.

Görselleştirmenin "Adım 1" sütunu, kullanıcıların ilk olaydan hemen sonra en sık ne yaptığını gösterir ve en sık en sık en sık yukarıdan aşağıya doğru sıralanır. "Adım 2" ve sonraki sütunlar, kullanıcıların sitenizde gezinmelerinin tüm yollarının resmini oluşturarak, kullanıcıların bundan sonra ne yaptığını gösterir.

Varsayılan olarak, Kullanıcı Akışları aracı sitenizden yalnızca son 24 saatlik sayfa görüntülemeve özel olayı rasgele örnekler. Zaman aralığını artırabilir ve Edit menüsünde rasgele örnekleme için performans ve doğruluk dengesini değiştirebilirsiniz.

Bazı sayfa görünümleri, özel olaylar ve özel durumlar sizinle alakalı değilse, gizlemek istediğiniz düğümlerde **X'i** tıklatın. Gizlemek istediğiniz düğümleri seçtikten sonra, visualization'ın altındaki **Grafik Oluştur** düğmesini tıklatın. Gizlediğiniz düğümlerin tümünün görülmesi **için, Edit** düğmesini tıklatın ve ardından **Dışlanan olaylar** bölümüne bakın.

Görselleştirmede görmeyi beklediğiniz sayfa görünümleri veya özel olaylar eksikse:

* Edit menüsündeki **Dışlanan olaylar** bölümünü kontrol **edin.**
* Görselleştirmeye daha az sıklıkta olaylar eklemek için **Diğerleri** düğümlerinde artı düğmelerini kullanın.
* Beklediğiniz sayfa görünümü veya özel etkinlik kullanıcılar tarafından seyrek olarak gönderiliyorsa, **Düzenleme** menüsündeki görselleştirmenin zaman aralığını artırmayı deneyin.
* Beklediğiniz sayfa görünümü, özel olay veya özel durum sitenizin kaynak kodunda Application Insights SDK tarafından toplanacak şekilde ayarlandığınızdan emin olun. [Özel etkinlikleri toplama hakkında daha fazla bilgi edinin.](../../azure-monitor/app/api-custom-events-metrics.md)

Görselleştirmede daha fazla adım görmek istiyorsanız, önceki adımları ve **sonraki** **adımları** visualization yukarıda açılır kullanın.

## <a name="after-visiting-a-page-or-feature-where-do-users-go-and-what-do-they-click"></a>Bir sayfayı veya özelliği ziyaret ettikten sonra, kullanıcılar nereye gider ve neleri tıklatabilirler?

![Kullanıcıların nereye tıkladıklarını anlamak için Kullanıcı Akışlarını kullanma](./media/usage-flows/00003-flows-one-step.png)

İlk etkinliğiniz bir sayfa görünümüyse, görselleştirmenin ilk sütunu ("Adım 1"), kullanıcıların sayfayı ziyaret ettikten hemen sonra ne yaptığını anlamanın hızlı bir yoludur. Sitenizi Kullanıcı Akışları görselleştirmesinin yanındaki pencerede açmayı deneyin. Kullanıcıların sayfayla nasıl etkileşimde bulunduklarına ilişkin beklentilerinizi "Adım 1" sütunundaki olaylar listesiyle karşılaştırın. Genellikle, sayfadaki takımınız için önemsiz görünen bir UI öğesi, sayfada en çok kullanılanlar arasında olabilir. Sitenizde tasarım iyileştirmeleri için harika bir başlangıç noktası olabilir.

İlk etkinliğiniz özel bir olaysa, ilk sütun kullanıcıların bu eylemi gerçekleştirmeden hemen sonra ne yaptığını gösterir. Sayfa görüntülemelerde olduğu gibi, kullanıcılarınızın gözlenen davranışlarının takımınızın hedef ve beklentilerine uygun olup olmadığını göz önünde bulundurun. Seçtiğiniz ilk etkinlik "Alışveriş Sepetine Öğe Eklendi" ise, örneğin, kısa bir süre sonra görselleştirmede "Kullanıma Git" ve "Tamamlanmış Satın Alma" görünüp görünmeyince görün. Kullanıcı davranışı beklentilerinizinizden farklıysa, kullanıcıların sitenizin geçerli tasarımı tarafından nasıl "kapana kısTırıldığını" anlamak için görselleştirmeyi kullanın.

## <a name="where-are-the-places-that-users-churn-most-from-your-site"></a>Kullanıcıların sitenizden en çok çalkaladığı yerler nerede?

Oturum **Sona Erdi** düğümleri için izleyin, özellikle bir akış erken, görselleştirme bir sütunda yüksek-up görünür. Bu, birçok kullanıcının önceki sayfaları ve Kullanıcı Aracı etkileşimlerini takip ettikten sonra sitenizden çalkalanmış olabilir. Bazen çalkal bekleniyor - örneğin, bir e-ticaret sitesinde bir satın alma tamamladıktan sonra - ama genellikle churn tasarım sorunları, kötü performans veya geliştirilebilir sitenizle ilgili diğer sorunların bir işaretidir.

**Oturum Sona Eren** düğümlerin yalnızca bu Application Insights kaynağı tarafından toplanan telemetriye dayandığını unutmayın. Uygulama Öngörüleri belirli kullanıcı etkileşimleri için telemetri almazsa, Kullanıcı Akışları aracı oturumun sona erdiğini söyledikten sonra kullanıcılar sitenizle bu şekilde etkileşimde olabilir.

## <a name="are-there-places-where-users-repeat-the-same-action-over-and-over"></a>Kullanıcıların aynı eylemi tekrar tekrar tekrarladığı yerler var mı?

Görselleştirmede sonraki adımlarda birçok kullanıcı tarafından yinelenen bir sayfa görünümü veya özel olay arayın. Bu genellikle kullanıcıların sitenizde yinelenen eylemler gerçekleştirdiği anlamına gelir. Yineleme bulursanız, sitenizin tasarımını değiştirmeyi veya yinelemeyi azaltmak için yeni işlevler eklemeyi düşünün. Örneğin, bir tablo öğesinin her satırında yinelenen eylemler gerçekleştiren kullanıcılar bulursanız toplu edit işlevselliği ekleme.

## <a name="common-questions"></a>Sık sorulan sorular

### <a name="does-the-initial-event-represent-the-first-time-the-event-appears-in-a-session-or-any-time-it-appears-in-a-session"></a>İlk olay, olayın bir oturumda ilk kez veya bir oturumda göründüğü zamanı mı temsil ediyor?

Görselleştirmedeki ilk olay, yalnızca bir kullanıcının oturum sırasında bu sayfa görünümünü veya özel olayı ilk kez gönderdiği zamanı temsil eder. Kullanıcılar ilk olayı bir oturumda birden çok kez gönderebiliyorsa, "Adım 1" sütunu yalnızca kullanıcıların tüm örneklerden değil, ilk olayın *ilk* örneğinden sonra nasıl tepki gösterdiğini gösterir.

### <a name="some-of-the-nodes-in-my-visualization-are-too-high-level-for-example-a-node-that-just-says-button-clicked-how-can-i-break-it-down-into-more-detailed-nodes"></a>Görselleştirmemdeki düğümlerin bazıları çok üst düzey. Örneğin, sadece "Düğme Tıklandı" yazan bir düğüm. Nasıl daha ayrıntılı düğümler halinde kırabilirim?

**Düzenleme** menüsünde seçeneklere **göre Bölme'yi** kullanın:

1. **Etkinlik** menüsünde yıkmak istediğiniz olayı seçin.
2. **Dimension** menüsünde bir boyut seçin. Örneğin, "Düğme Tıklatıldı" adlı bir etkinliğiniz varsa, "Düğme Adı" adlı özel bir özellik deneyin.

## <a name="next-steps"></a>Sonraki adımlar

* [Kullanıma genel bakış](usage-overview.md)
* [Kullanıcılar, Oturumlar ve Etkinlikler](usage-segmentation.md)
* [Bekletme](usage-retention.md)
* [Uygulamanıza özel etkinlikler ekleme](../../azure-monitor/app/api-custom-events-metrics.md)
