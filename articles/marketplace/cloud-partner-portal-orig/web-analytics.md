---
title: Web Analytics
description: Bu makalede, işletmenizin en iyi şekilde büyümesini sağlamak için Web analizinin nasıl öğreneceği ve kullanılacağı hakkında yönergeler sunulmaktadır.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: dsindona
ms.openlocfilehash: bd6d6375b19689c582c0f129378c76936e624e04
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81416275"
---
<a name="web-analytics"></a>Web Analytics
=============

Bu makalede, işletmenizin en iyi şekilde büyümesini sağlamak için Web analizinin nasıl öğreneceği ve kullanılacağı hakkında yönergeler sunulmaktadır. Şu anda bu Öngörüler sekmesi tüm AppSource teklifleri için kullanılabilir.

Teklifinizi oluşturup yayımladığınıza göre, yolculuğun bir sonraki kısmı\' başarınızı izleyip ölçmeye hazır. **Web Analytics**ile, tekliflerinizin her birinin Market 'te ne kadar iyi bir şekilde çalıştığını görme imkanını ekledik. Yolculuğa başlamak için, yeni analiz sekmesini görmek üzere Bulut İş Ortağı Portalı sol tarafındaki Öngörüler sayfasına gidin.

![WebAnalytics sayfası](./media/si-getting-started/WebAnalytics1.png)

Microsoft Power BI ile oluşturulmuş yayımcı KIMLIĞINIZ için zengin bir pano görürsünüz ve her gün, her gün yenilenen\' verileri görmenizi sağlar.

<a name="microsoft-campaigns"></a>**Microsoft kampanyaları**
-----------------------

Tekliflerinizi büyütmek ve tekliflerinizin büyümesini izlemek için Bulut İş Ortağı Portalı **Microsoft kampanyalarını** kullanma özelliğini etkinleştirdik. Kampanyalar, Market için, uygulama ayrıntıları sayfanıza müşteri gönderen farklı kanalları izlemenize imkan tanıyan yeni, desteklenen bir özelliktir.

### <a name="how-to-make-a-campaign"></a>**Kampanya oluşturma**

Kampanyaları tanımlamanın en kolay yolu, URL 'nize Market 'teki uygulama ayrıntısı sayfasında yer alan özel bir sözcük/terim ekliyoruz. Google, Bing, LinkedIn ve diğer birçok site, bir reklam oluşturmanızı, sitelerini istediğiniz siteye bağlamayı teşvik eder.

Genel olarak, bu çabalar yeni müşterileri ürününüzle kullanmanıza yardımcı olmak ve kanallarınızın her birinin nasıl yaptığı hakkında başarıyı ölçmek açısından önemlidir. Bu, kampanyaların geldiği yerdir.

Kendi kampanyanızı oluşturmak için iki yol vardır.

1. URL 'nize, kampanyanın ne olduğunu ve bu müşterilerin hangi sayfa/olay geldiğini açıklayan **mktcmpıd** sorgu parametresini ekleyin.

Örneğin, şunları kullanabilirsiniz:```https://appsource.microsoft.com/product/dynamics-365/contoso.offername?mktcmpid=NewCampaign```

1. (Gelişmiş): URL 'de desteklenen, genel kampanya kimliklerimizden birini kullanın. Kullanmanız gereken ek başvuru etiketlerine göz duymasını istiyoruz, bu nedenle bu ek etiketleri otomatik olarak tanıma kuralını destekliyoruz:
    
    1. **UTM\_kampanyası**
    2. **Utd\_kaynağı**
    3. **ref**
    4. **src**

Örneğin, şunları kullanabilirsiniz:```https://appsource.microsoft.com/product/dynamics-365/contoso.offername?utm_campaign=NewCampaign```

Kampanyanın, müşterinin geldiği konum (e-posta, blog, sosyal medya kaynağı vb.) gibi birden fazla kaynağı daha ayrıntılı olarak belirlemek için bu kampanya kimliklerinin birden çok birleşimini kullanabilirsiniz.

Örneğin:

1. Bülten başvuran:```https://appsource.microsoft.com/product/dynamics-365/contoso.offername?mktcmpid=NewCampaign&src=newsletter```
2. LinkedIn başvuran:```https://appsource.microsoft.com/product/dynamics-365/contoso.offername?mktcmpid=NewCampaign&src=LinkedIn```

### <a name="ensuring-campaigns-pass-through-all-your-pages"></a>**Kampanyaların tüm sayfalarınızda geçiş yapmalarını sağlama**

Kampanyalarınızın, trafiği yönlendiren bir ara sayfasına sahip olduğu bir senaryo olabilir ve bu sayede müşterileri Market 'e gönderebilirsiniz. İlk kampanya kimliklerinizi Market 'e göndereceğiniz son URL 'ye geçirmek önemlidir.

Örnek aşağıda verilmiştir:

1. Pazarlama çalışanı, Google 'dan trafiği şirket\'s giriş sayfasına ```https://contoso.com```yönlendirmek için reklamları satın alarak. Bu giriş sayfasında, ürünümüzü \"\" deneyin bağlantısı bulunur. ```https://appsource.com```
2. Kullanıcı ad 'ye tıklar ve şirket\'s giriş sayfasında yer alır.
    1.  Başvuru URL 'SI = google.com
    2.  Giriş sayfası URL 'SI =```https://contoso.com/?utm_campaign=MyCampaignAdName&utm_source=MySourceAdName```
3. Kullanıcı ürünümüzü \"\" dene bağlantısına tıklar ve appsource 'a gider.
    1. Başvuru URL 'SI =```https://contoso.com/?utm_campaign=MyCampaignAdName&utm_source=MySourceAdName```
    2. Giriş sayfası URL 'SI (**Bu URL 'nin bu URL 'ye\_UTM kampanyası ve\_UTM kaynağı eklendiğinden emin olun**) =```https://appsource.microsoft.com/product/dynamics-365/contoso.offername?**utm\_campaign=MyCampaignAdName&utm\_source=MySourceAdName**```

<a name="how-to-evaluate-the-success-of-a-campaign"></a>Kampanyanın başarısını değerlendirme
-----------------------------------------

### <a name="page-visits-by-campaign"></a>**Kampanyaya göre sayfa ziyaretleri**

![WebAnalytics2](./media/si-getting-started/WebAnalytics2.png)

Bu, günlük sayfa ziyaretlerinizin her birinin, geldiği kampanya tarafından dökülübir dökümden oluşur.

### <a name="conversion-rate-by-campaign"></a>**Kampanyaya göre dönüştürme oranı**

![WebAnalytics3](./media/si-getting-started/WebAnalytics3.png)

Tüm teklifinizin dönüştürme oranını gösterdiğimiz gibi, bu grafikte farklı kampanyalarınızın nasıl yaptığına ilişkin dökümü görebilirsiniz. Bu grafik, daha yüksek dönüştürme oranı kampanyalarınızın nereden geldiğini belirlemenize yardımcı olmalıdır.

### <a name="distribution-by-campaign"></a>**Kampanyaya göre dağıtım**

![WebAnalytics4](./media/si-getting-started/WebAnalytics4.png)

Müşterilerinizin etki alanlarına baktığımız gibi, bu grafik, kullanıcıların kapsamındaki Market 'e geldiği kampanya başına verilerinizin dağılımını görmenizi sağlar. \_NoCampaign, müşterinin Market 'e gezindiklerinde URL 'de bir kampanya KIMLIĞI olmadığı anlamına gelir.

<a name="next-steps"></a>**Sonraki adımlar**
--------------

Tekliflerinizi başarılı bir şekilde izleyebilmenizi sağladığınıza göre, size kendi kampanyalarınızı oluşturmanız önerilir.

Sorularınız/Özellik istekleriniz varsa, bunları sağ üst köşede bulunan geri bildirimde bulunarak paylaşabilirsiniz.

![Bulut İş Ortağı Portalı geri bildirim](./media/si-getting-started/WebAnalytics5.png)
