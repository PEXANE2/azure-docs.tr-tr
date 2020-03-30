---
title: Web Analizi
description: Bu makalede, işinizi en iyi şekilde büyütmek için Web Analytics'i nasıl öğreneceğiniz ve kullanacağınız hakkında talimatlar verilmektedir.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: dsindona
ms.openlocfilehash: 446e579a7205f0e785d7e940c1dbdd36cff8f370
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80285342"
---
<a name="web-analytics"></a>Web Analizi
=============

Bu makalede, işinizi en iyi şekilde büyütmek için Web Analytics'i nasıl öğreneceğiniz ve kullanacağınız hakkında talimatlar verilmektedir. Şu anda bu Öngörüler sekmesi herhangi bir AppSource teklifi için kullanılabilir.

Teklifinizi oluşturup yayınladığınıza göre, yolculuğunuzun bir sonraki bölümü onun\' başarısını izlemek ve ölçmektir. **Web Analytics**ile, tekliflerinizin her birinin pazarda tam olarak ne kadar iyi olduğunu görme olanağı nı ekledik. Seyahatinize başlamak için, yeni Analytics sekmesini görmek için Bulut İş Ortağı Portalı'nın sol tarafındaki Öngörüler sayfasına gidin.

![WebAnalytics sayfası](./media/si-getting-started/WebAnalytics1.png)

Microsoft Power BI ile oluşturulmuş ve her gün yenilenen teklif\' verilerinizin her birini görmenizi sağlayan Publisher ID'niz için zengin bir pano görürsünüz.

<a name="microsoft-campaigns"></a>**Microsoft Kampanyaları**
-----------------------

Tekliflerinizi büyütmek ve tekliflerinizi büyütmek için Bulut İş Ortağı Portalı'nda **Microsoft kampanyalarını** kullanma olanağı sağladık. Kampanyalar, müşterileri uygulama ayrıntıları sayfanıza gönderen farklı kanalları izlemenize olanak tanıyan pazar yeri için yeni desteklenen bir özelliktir.

### <a name="how-to-make-a-campaign"></a>**Kampanya Nasıl Yapılır?**

Kampanyaları tanımlamanın en basit yolu, URL'nize pazardaki uygulama ayrıntı sayfanıza inen özel bir sözcük/terim eklemenizdir. Google, Bing, LinkedIn ve diğer birçok site, bir reklam oluşturmanızı, kendi sitelerinden istediğiniz siteye bağlantı kurmanızı öneririz.

Genel olarak, bu çabalar yeni müşterileri ürününüzün içine itmeye yardımcı olmak içindir ve her bir kanalınızın başarısını ölçmek esastır. Kampanyalar burada devreye sayılsın.

Kendi kampanyanızı oluşturmanın iki yolu vardır.

1. URL'nize kampanyanın ne olduğunu ve bu müşterilerin hangi sayfadan/olaydan geldiğini açıklayan sorgu parametresi **mktcmpid'i** ekleyin.

Örneğin şunları kullanabilirsiniz:<https://appsource.microsoft.com/product/dynamics-365/contoso.offername?mktcmpid=NewCampaign>

1. (Gelişmiş): URL'de desteklenen genel kampanya kişilerimizden birini kullanın. Kullanmanız gereken ek ref etiketleri ile uyumlu olmak istiyoruz, bu nedenle bu ek etiketleri otomatik olarak tanımak için kuralı destekliyoruz:
    
    1. **utm\_kampanyası**
    2. **utm\_kaynak**
    3. **ref**
    4. **src**

Örneğin şunları kullanabilirsiniz:<https://appsource.microsoft.com/product/dynamics-365/contoso.offername?utm_campaign=NewCampaign>

Müşterinin geldiği yer (e-posta, blog, sosyal medya kaynağı, vb.) gibi kampanya için trafiği yönlendiren birden fazla kaynağı daha fazla belirlemek için bu kampanya kimliklerinin birden fazla sının bir birleşimine sahip olmayı seçebilirsiniz.

Örnek:

1. Bülten referrer:<https://appsource.microsoft.com/product/dynamics-365/contoso.offername?mktcmpid=NewCampaign&src=newsletter>
2. LinkedIn referrer:<https://appsource.microsoft.com/product/dynamics-365/contoso.offername?mktcmpid=NewCampaign&src=LinkedIn>

### <a name="ensuring-campaigns-pass-through-all-your-pages"></a>**Kampanyaların tüm sayfalarınızdan geçmesini sağlama**

Kampanyalarınızın, trafiği yönlendirdiğiniz ve müşterileri pazara göndermeye devam ettiği bir ara sayfaya sahip olduğu bir senaryo olabilir. İlk kampanya bilgilerinizi pazara gönderdiğiniz son URL'ye aktarmak önemlidir.

Örnek aşağıda verilmiştir:

1. Pazarlama çalışanı, şirketin\'açılış sayfasına <https://contoso.com>trafik çekmek için Google'dan reklam satın alır. Bu açılış sayfasında \"gider\" benim ürün <https://appsource.com>bağlantısı deneyin .
2. Bir kullanıcı reklamı tıklar ve\'şirketinin açılış sayfasına iner.
    1.  Sevk URL = google.com
    2.  Açılış Sayfası URL =<https://contoso.com/?utm_campaign=MyCampaignAdName&utm_source=MySourceAdName>
3. Kullanıcı benim ürün \"\" bağlantısını deneyin tıklar ve AppSource gider.
    1. Başvuru URL =<https://contoso.com/?utm_campaign=MyCampaignAdName&utm_source=MySourceAdName>
    2. Açılış Sayfası URL'si (**Bu URL'nin\_utm kampanyasına ve utm\_kaynağının bu URL'ye eklenmediğinden emin olun**) = [ https://appsource.microsoft.com/product/dynamics-365/contoso.offername?utm\_kampanyası=MyCampaignAdName&utm\_kaynak=MySourceAdName](https://appsource.microsoft.com/product/dynamics-365/contoso.offername?utm_campaign=MyCampaignAdName&utm_source=MySourceAdName)

<a name="how-to-evaluate-the-success-of-a-campaign"></a>Kampanyanın başarısı nasıl değerlendirilir?
-----------------------------------------

### <a name="page-visits-by-campaign"></a>**Kampanyaya Göre Sayfa Ziyaretleri**

![WebAnalytics2](./media/si-getting-started/WebAnalytics2.png)

Bu, geldikleri kampanyaya göre günlük sayfa ziyaretlerinizin her birinin dökümüdür.

### <a name="conversion-rate-by-campaign"></a>**Kampanyaya göre dönüşüm oranı**

![WebAnalytics3](./media/si-getting-started/WebAnalytics3.png)

Teklifinizin tamamının dönüşüm oranını gösterme şekline benzer şekilde, bu grafikte farklı kampanyalarınızın nasıl olduğunu görebilirsiniz. Bu grafik, daha yüksek dönüşüm oranı kampanyalarınızın nereden geldiğini belirlemenize yardımcı olur.

### <a name="distribution-by-campaign"></a>**Kampanyaya göre dağıtım**

![WebAnalytics4](./media/si-getting-started/WebAnalytics4.png)

Müşterilerinizin etki adlarına nasıl baktığımıza benzer şekilde, bu grafik, kullanıcıların pazara girdiği kampanya başına verilerinizin dağıtımını görmenizi sağlar. \_NoCampaign, müşterinin pazara gittiğinde url'de kampanya kimliği olmadığı anlamına gelir.

<a name="next-steps"></a>**Sonraki Adımlar**
--------------

Artık tekliflerinizin başarısını takip edebilme yeteneğine sahip olduğunuza göre, kendi kampanyalarınızı oluşturmanızı teşvik etmek istiyoruz.

Sorularınız/özellik talepleriniz varsa, bunları sağ üst köşede bulunan Geri Bildirim aracılığıyla paylaşın.

![Bulut İş Ortağı Portalında Geri Bildirim](./media/si-getting-started/WebAnalytics5.png)
