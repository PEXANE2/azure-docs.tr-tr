---
title: Azure uygulaması teklif Marketplace sekmesi
description: Azure uygulama teklifi için pazarlama varlıklarını tanımlamak için Market sekmesini kullanın.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/23/2019
ms.author: dsindona
ms.openlocfilehash: 6113759cc68d07af4c22edf03b3274d92dcbe780
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81416305"
---
# <a name="azure-application-marketplace-tab"></a>Azure uygulaması Market sekmesi

Azure uygulamanızı açıklamak ve pazarlama varlıkları sağlamak için Market sekmesini kullanın. Bu sekme aşağıdaki formları içerir: Genel Bakış, Pazarlama Eserleri, Müşteri Adayı Yönetimi ve Yasal.

## <a name="overview-form"></a>Genel bakış formu

Genel Bakış formu, bir sonraki ekran çekiminde gösterilen gerekli ve isteğe bağlı alanlara sahiptir. Gerekli alanlar yıldız işareti (*) tarafından suçlanır.

![Genel bakış formu](./media/azureapp-marketplace-overview.png)

Aşağıdaki tabloda, teklif için bir vitrin oluşturmak için kullanılacak ayarlar açıklanmaktadır.   Yıldız işaretiyle eklenen alanlar gereklidir.

|      Alan         |    Açıklama    |
|  ---------------   |  ---------------  |
| **Başlık\***        | Teklifin başlığı. Bu pazarda belirgin görüntülenir. Maksimum uzunluk 50 karakterdir. |
| **Özet\***      | Teklifin kısa özeti. Maksimum uzunluk 100 karakterdir.           |
| **Uzun Özet\*** | Teklifin daha uzun özeti (özetle aynı olabilir). Maksimum uzunluk 256 karakterdir.           |
| **Açıklama\***  | Teklifin açıklaması. Maksimum uzunluk 3000 karakterdir. &gt; &lt;P, em,&gt; &lt;ul&gt;, li &lt;&gt;, &lt;ol&gt; ve üstbilgi etiketleri de dahil olmak üzere &lt;basit HTML biçimlendirmeye izin verilir.  |
| **Pazarlama Tanımlayıcısı\*** | Bu teklifle ilişkilendirilen benzersiz bir URL, genellikle kuruluş ve çözüm adınızı, maksimum uzunluk 50 karakteri içerir. Hizmetiniz için kısa ve samimi bir pazarlama tanımlayıcısı seçin. Bu teklif için pazar URL'lerinde kullanılacaktır. Örneğin, yayıncı kimliğiniz "contoso" ise ve pazarlama tanımlayıcınız "sampleApp" ise, Azure Market'teki teklifinizin URL'si`https://azuremarketplace.microsoft.com/marketplace/apps/contoso.sampleApp`  
| **Abonelik IYT'lerini Önizleme\*** | Bir den 100'e kadar abonelik tanımlayıcıları previewers ekleyin. Beyaz listedeki bu abonelikler, yayınlanmadan önce, yayınlanmadan önce önizlemede kullanılabilirken teklifinize erişebilir.          |
| **Faydalı Linkler**    | İsteğe bağlı olarak, teklifinizin kullanıcıları için destek, dokümantasyon, forum, vb. gibi çeşitli kaynaklara bağlantılar sağlayabilirsiniz.  Belgelerinize en az bir bağlantı eklemeniz önerilir.            |
| **Önerilen Kategoriler (Max 5)\*** | Bir ila beş kategori seçin. Seçili kategoriler, teklifinizi Azure Marketi ve Azure Portalı'nda bulunan ürün kategorilerine haritalamak için kullanılır. Bunlar gözatma sayfalarında ve ürün ayrıntıları sayfanızda gösterilir. |
|  |  |


## <a name="marketing-artifacts"></a>Pazarlama Eserleri

Pazarlama Eserleri formu, bir sonraki ekran yakalamada gösterilen gerekli ve isteğe bağlı alanlara sahiptir. Gerekli alanlar yıldız işareti (*) tarafından suçlanır.

![Pazarlama eserler formu](./media/azureapp-marketplace-artifacts.png)

Aşağıdaki tabloda pazarlama yapıları açıklanmaktadır.

|      Alan         |    Açıklama    |
|  ---------------   |  ---------------  |
| **Küçük\***        | Küçük logo: PNG formatında 40x40 piksel     |
| **Medium\***       | Orta logo: PNG formatında 90x90 piksel    |
| **Büyük\***        | Büyük logo: PNG formatında 115x115 piksel   |
| **Geniş\***         | Geniş logo: PNG formatında 255x115 piksel    |
| **Kahraman**           | İsteğe bağlı kahraman logosu: PNG formatında 815x290 piksel. **Not:** Kahraman simgesi yüklendikten sonra silinemez. |
| **Ekran Görüntüleri (Max 5)** |        Ekran görüntüleri ürün ayrıntıları sayfanızda görüntülenir. Bunlar, uygulamanızın ne yaptığını ve nasıl çalıştığını görsel olarak iletmek için iyi bir yoldur. Örneğin, mimari diyagramları gösterebilir veya büyük/küçük harf çizimleri kullanabilirsiniz. Ekran görüntüleri isteğe bağlıdır ve SKU başına 5 ile sınırlıdır. Ekran görüntüsü eklemek için:<ul><li>Ekran Görüntüsü penceresini açmak için **ekran görüntüsü ekle +** seçin</li><li>**Ad** - Bir ad/başlık girin (Maksimum uzunluk 100 karakter.)</li><li>**Upload** - Resmi yükleyin. PNG formatında olmalıdır ve boyutu 533 x 324 pikseldir.</li></ul>           |
| **Video ekleme**      | İsteğe bağlı olarak, videolar ürün ayrıntıları sayfanızda görüntülenir. Uygulamanızın ne yaptığını ve nasıl çalıştığını görsel olarak iletmek için iyi bir yoldur. Video eklemek için: <ul><li>Video penceresini açmak için **+ Video Ekle'yi** seçin</li><li>**Ad** - Bir ad/başlık girin (Maksimum uzunluk 100 karakter.)</li><li>**Bağlantı** - Videoyu barındıran sitenin URL'sini girin (YouTube veya Vimeo)</li><li>**Küçük resim** - Küçük resmi yükleyin. PNG formatında olmalıdır ve boyutu 533 x 324 pikseldir.</li></ul>          |
|  |  |


### <a name="artifact-examples-in-azure-marketplace"></a>Azure Marketi'nde artefakt örnekleri

Sonraki ekran yakalama, Bir Market arama sonucunun bir örneğini gösterir.

![Pazar yeri teklif arama sonucu](./media/azureapp-marketplace-example-browse.png)

Aşağıdaki resim, bir müşteri arama sonucu teklifin döşemesini tıklattıktan sonra teklifin Market'te nasıl görüntülendiğini gösterir.

![Marketplace teklif arama sonucu ayrıntıları](./media/azureapp-marketplace-example-details.png)


### <a name="artifact-examples-in-azure-portal"></a>Azure Portalı'nda artefakt örnekleri

Aşağıdaki ekran görüntüleri, bir teklifin Azure Portalı'nda nasıl görüntülendiğini gösterir. Bu örnekte uygulama teklifi **Pazar>Her şey dev + Test>Jenkins>** tarama tarafından bulunur. Jenkins teklifi bir logo, başlık ve yayıncı ekran adını gösterir.

![Azure portalında tekliflere göz atın](./media/azureapp-portalbrowse-artifacts-jenkins.png)

Bir sonraki ekran yakalama, kullanıcı Jenkins'i seçtiğinde uygulama hakkında ayrıntılı bilgileri gösterir.

![Azure portalında teklif ayrıntıları](./media/azureapp-portal-artifacts-jenkins-details.png)


### <a name="logo-guidelines"></a>Logo yönergeleri

Bulut İş Ortağı Portalı'na yüklenen tüm logolar aşağıdaki kurallara uymalıdır:

- Azure tasarımının basit bir renk paleti vardır. Logonuzdaki birincil ve ikincil renk sayısını düşük tutun.
- Azure Portalı'nın tema renkleri beyaz ve siyahtır. Logolarınız için arka plan rengi olarak bu renkleri kullanmaktan kaçının. Logolarınızı Azure portalında öne çıkaracak bir renk kullanın. Basit birincil renkleri öneririz. Saydam bir arka plan kullanıyorsanız, logoların/metnin beyaz, siyah veya mavi olmadığından emin olun.
- Logonuzda degrade arka plan kullanmayın.
- Logoya metin, hatta şirketiniz veya marka adınızı bile koymaktan kaçının. Görünüm ve logo hissediyorum "düz" olmalı ve degradeler kaçınmalısınız.
- Logoyu esnetme.


#### <a name="hero-logo"></a>Kahraman logosu

Hero logosu isteğe bağlıdır.

>[!IMPORTANT]
>Hero logosu yüklendikten sonra silemezsiniz.

Hero logosu için aşağıdaki yönergeleri kullanın:

- Siyah, beyaz ve saydam arka planlara izin verilmez.
- Logo için arka plan olarak herhangi bir açık renk kullanmaktan kaçının. Yayımcı ekran adı, plan başlığı ve teklif uzun özeti beyaz yazı tipi renginde görüntülenir ve arka plana karşı öne çıkmalıdır.
- Logoyu tasarlarken çoğu metni kullanmaktan kaçının. Yayıncı adı, plan başlığı, teklif uzun özeti ve oluşturma düğmesi, teklif listelendiğinde logonun içine programlı olarak gömülür.
- Kahraman logonuzun sağ tarafına kullanılmayan dikdörtgen bir boşluk ekleyin. Bu boş alan 415x100 pikseldir ve soldan 370 piksel den uzaktır.


## <a name="lead-management"></a>Müşteri Yönetimi

Müşteri Adayı Yönetimi formu, müşteri adayı yönetimini yapılandırmak için isteğe bağlı bir alana sahiptir. Müşteri adayı yönetimini yapılandırmak için açılır listeden Müşteri Adayı hedefini seçin. Sonraki ekran yakalama kullanılabilir hedefleri gösterir.

![Müşteri adayı yönetimi hedefini seçin](./media/azureapp-marketplace-leadmgmt.png)

>[!TIP]
>Bu iletiyi görmek için bilgi simgesini seçin: "Müşteri adaylarınızın depolandığı sistemi seçin. CRM [sisteminize](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-get-customer-leads) nasıl bağlanıp bağlanılabildiğini buradan öğrenin."

Daha fazla bilgi için [bkz.](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-get-customer-leads)


## <a name="legal"></a>Yasal Bildirim

Her teklif için gerekli yasal belgeleri sağlamak için Yasal formu kullanın.

Şu bilgileri belirtin:

- **Gizlilik politikası\* URL'si** - Uygulamanızın gizlilik politikasına bir bağlantı girin.
- **Kullanım\* koşulları** - Uygulamanız için kullanım koşullarını girin. Müşterilerin uygulamanızı denemeden önce bu koşulları kabul etmeleri gerekir.

![Yasal form](./media/azureapp-marketplace-legal.png)


## <a name="next-steps"></a>Sonraki adımlar

[Destek sekmesi](./cpp-support-tab.md)
