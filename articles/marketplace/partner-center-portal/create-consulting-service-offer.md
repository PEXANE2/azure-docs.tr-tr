---
title: Iş Ortağı Merkezi 'nde bir danışmanlık hizmeti teklifi oluşturma-Azure Marketi
description: Iş Ortağı Merkezi 'ni kullanarak Azure Market veya AppSource 'a bir danışmanlık hizmeti teklifi yayımlamayı öğrenin.
author: anbene
ms.author: mingshen
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/06/2020
ms.openlocfilehash: eff37750f0580a28c9644ee1ffb7fe4e95038709
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81869792"
---
# <a name="create-a-consulting-service-offer"></a>Danışmanlık hizmeti teklifi oluşturma

> [!IMPORTANT]
> Danışmanlık hizmeti tekliflerinizin yönetimini Bulut İş Ortağı Portalı Iş Ortağı Merkezi 'ne taşıdık. Tekliflerinizi geçirene kadar, tekliflerinizi yönetmek için lütfen [Azure ve Dynamics 365 danışmanlık hizmeti teklifiyle](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/consulting-services/cloud-partner-portal-consulting-services-publishing-offer) bulut iş ortağı portalı ilgili yönergeleri izleyin.

Bu makalede, [Azure Market](https://azuremarketplace.microsoft.com/) veya [appsource](https://appsource.microsoft.com/)'a bir danışmanlık hizmeti teklifinin nasıl yayımlanacağı açıklanır. AppSource 'ta Microsoft [Dynamics 365](https://dynamics.microsoft.com/) ve Power platform temelinde danışmanlık hizmeti tekliflerini listeleyin. Azure Marketi 'ndeki Microsoft Azure temel alan danışmanlık hizmeti tekliflerini listeleyin.

Azure Market veya AppSource danışmanlık hizmetlerinde bir danışmanlık hizmeti teklifi oluşturmak için önce [Iş Ortağı Merkezi 'nde bir yayımcı hesabınız](https://docs.microsoft.com/azure/marketplace/partner-center-portal/create-account)olmalıdır ve hesabınızın ticari Market programına kayıtlı olması gerekir. Teklifinizi oluşturmadan önce, [danışmanlık hizmeti ön koşulları](https://docs.microsoft.com/azure/marketplace/partner-center-portal/consulting-service-prerequisites)' nda önkoşulları gözden geçirin.

## <a name="publishing-benefits"></a>Yayımlama avantajları

Ticari Market 'e yayımlamanın avantajları:

- Microsoft markasını kullanarak şirketinizi yükseltin.
- AppSource 'ta 100.000.000 ' den fazla Office 365 ve Dynamics 365 kullanıcısı ve Azure Marketi aracılığıyla 200.000 ' ten fazla kuruluşa ulaşın.
- Bu Pazar yerlerden yüksek kaliteli müşteri adayları alın.
- Hizmetlerinizin Microsoft alanı ve Telesales ekipleri tarafından yükseltilme

## <a name="create-a-new-offer"></a>Yeni teklif oluşturma

Yukarıda açıklanan gereksinimleri karşıladıktan sonra, bir danışmanlık hizmeti teklifi oluşturmak için aşağıdaki adımları izleyin.

1. [Iş Ortağı Merkezi](https://partner.microsoft.com)' nde oturum açın ve üst menüden **Pano** ' yı seçin.
2. Sol gezinti çubuğunda **ticari Market**' i seçin ve **genel bakış**' ı seçin.

    :::image type="content" source="media/cs-menu-overview.png" alt-text="Ticari Market 'in menüsünü gösterir":::

3. **+ Yeni teklif**' i seçin ve ardından **danışmanlık hizmeti**' ni seçin.

    :::image type="content" source="media/cs-menu-newoffer.png" alt-text="Yeni bir teklif oluşturmak için düğmeyi gösterir.":::

4. Bir **TEKLIF kimliği**girin. Bu, hesabınızdaki her teklif için benzersiz bir tanımlayıcıdır.

    - Bu KIMLIK, Market teklifinin Web adresinde müşteriler tarafından görülebilir.
    - Yalnızca küçük harf, sayı, kısa çizgi ve alt çizgi kullanın, ancak boşluk kullanmayın. Uzunluk 50 karakterle sınırlıdır. Örneğin, **Test-teklif-1**girerseniz, teklif URL 'si olur `https://azuremarketplace.microsoft.com/marketplace/../test-offer-1`.
    - Teklif KIMLIĞI&#39;, **Oluştur**' u seçtikten sonra değiştirilebilir.

5. Bir **teklif diğer adı**girin. Bu, Iş Ortağı Merkezi 'nde teklifi ifade etmek için kullanılan addır.

    - Bu ad, Market 'te kullanılan ISN&#39;t. Teklif adından ve müşterilere gösterilen diğer değerlerden farklı&#39;s. Teklifin dahili olarak tanımlanması için daha faydalı bir ad atamak üzere bu alanı kullanabilirsiniz; müşteriler bu şekilde gösterilmez.
    - Teklif diğer adı, **Oluştur**' u seçtikten sonra değiştirilebilir&#39;.

Bu iki değeri girdikten sonra, **teklif kurulumu** sayfasına devam etmek için **Oluştur** ' u seçin.

## <a name="offer-setup"></a>Teklif kurulumu

Bir teklif KIMLIĞI ve teklif diğer adı girdikten sonra, Iş ortağı merkezi bir taslak teklif oluşturur ve **teklif kurulumu** sayfasını görüntüler. Teklifinizi ayarlamak için bu adımları izleyin.

### <a name="connect-lead-management"></a>Müşteri adayı yönetimini bağlama

Teklifinizi Iş Ortağı Merkezi ile Market 'te yayımlarken bir Müşteri Ilişkileri yönetimi (CRM) veya pazarlama otomasyonu sistemine bağlamanız _gerekir_ . Bu, bir kişi, ürününüzü ilgilendiren veya ürünü kullandığı anda müşteri iletişim bilgilerini almanızı sağlar.

1. Müşteri adaylarını nereden göndermemizi istediğinizi belirtmek için **Bağlan** ' ı seçin. İş Ortağı Merkezi aşağıdaki sistemleri destekler:

    - Müşteri katılımı için [Dynamics 365](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-lead-management-instructions-dynamics)
    - [Marketo](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-lead-management-instructions-marketo)
    - [Salesforce](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-lead-management-instructions-salesforce)

    > [!NOTE]
    > CRM sisteminiz yukarıda listelenen&#39;, müşteri adayı verilerini depolamak için [Azure tablosu](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-lead-management-instructions-azure-table) veya [https uç noktasını](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-lead-management-instructions-https) kullanın, ardından verileri CRM sisteminize dışarı aktarın.

2. Teklifinizi Iş Ortağı Merkezi 'nde yayımlarken lider hedefine bağlayın.
3. Müşteri adayı hedefi bağlantısının düzgün şekilde yapılandırıldığını doğrulayın. Iş Ortağı Merkezi 'nde yayımladıktan sonra, bağlantıyı doğrulamamız ve size bir test lideri göndereceğiz&#39;. Teklifi canlı olmadan önce önizlerken, teklifi önizleme ortamında satın almayı deneyerek lider bağlantınızı da test edebilirsiniz.
4. Müşteri adayı hedefine olan bağlantının güncelleştirildiğinden emin olun, böylece hiçbir müşteri adaylarını kaybetmezsiniz&#39;.

Bazı ek müşteri yönetimi kaynakları aşağıda verilmiştir:

- [Müşteri adayı yönetimine genel bakış](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-get-customer-leads)
- [Müşteri adayı yönetimi hakkında SSS](https://docs.microsoft.com/azure/marketplace/lead-management-for-cloud-marketplace#frequently-asked-questions)
- [Sık karşılaşılan müşteri adayı yapılandırma hataları](https://docs.microsoft.com/azure/marketplace/lead-management-for-cloud-marketplace#common-lead-configuration-errors-during-publishing-on-cloud-partner-portal)
- [Müşteri adayı yönetimine genel bakış](https://assetsprod.microsoft.com/mpn/cloud-marketplace-lead-management.pdf) PDF (açılır pencere engelleyicinizin kapalı olduğundan emin olun)

Sonraki bölüme devam etmeden önce **Taslağı kaydet** ' i seçin.

### <a name="properties"></a>Özellikler

Bu sayfa, danışmanlık hizmetinizin en iyi şekilde kapsaabileceği birincil ürünü ayarlamanıza, bir danışmanlık hizmeti türü ayarlayabilmenizi ve geçerli ürünleri seçmenizi sağlar.

1. Açılan listeden bir **birincil ürün** seçin.
2. Açılan listeden bir **danışmanlık hizmeti türü** seçin:

    - **Değerlendirme** : bir çözümün uygulanabilirliğini tespit etmek ve maliyet ve zamanlama tahminini sağlamak için bir müşteri&#39;ortamı değerlendirmesi.
    - **Briing** : çerçeveler, tanıtımlar ve müşteri örneklerini kullanarak müşteri ilgilenmesini çizmek için bir çözüme veya bir danışmanlık hizmetine giriş.
    - **Uygulama** : tam olarak çalışan bir çözüme neden olan tam bir yükleme. İki hafta veya daha az bir şekilde uygulanabilen çözümlerle sınırlayın.
    - **Kavram kanıtı** : bir çözümün müşteri gereksinimlerini karşılayıp karşılamadığını tespit etmek için sınırlı kapsamlı bir uygulama.
    - **Workshop** : müşteri&#39;şirket içi etkileşimli bir katılım. Müşteri&#39;s verileri veya ortamı üzerinde oluşturulmuş eğitim, Brit, değerlendirmeler veya tanıtımlar içerebilir.

1. **Azure**'ın birincil bir ürününü seçtiyseniz en fazla üç **çözüm alanı**seçin. Bu, Azure Marketi 'ndeki müşterilerin teklifinizi bulmasını kolaylaştırır. Azure 'u seçin&#39;t 'i seçtiyseniz bu adımı atlayın.
2. Azure _dışında bir birincil ürün seçtiyseniz_ , en fazla üç **uygulanabilir ürünü**seçin. Bu, AppSource 'taki müşterilerin teklifinizi bulmasını kolaylaştırır. Ayrıntılar için bkz. [Microsoft AppSource danışmanlık hizmeti listeleme yönergeleri](https://go.microsoft.com/fwlink/?LinkId=828734&amp;clcid=0x409) (PDF).
3. Teklifinizin uygulanacağı en fazla altı **sektör** seçin. Bu, müşterilerin teklifinizi bulmasını kolaylaştırır.
4. Şirketinizin danışmanlık hizmeti teklifi listesinde görüntülenmek üzere kazanıldığını en fazla üç **uzmanlık** ekleyin. Azure Expert MSP&#39;s ve Azure ağ MSP&#39;s dışında en az bir yetenek gereklidir.

Bir sonraki bölüme devam etmeden önce **Taslağı kaydet** ' i seçin, teklif listesi.

## <a name="offer-listing"></a>Teklif listesi

Burada, Market 'te görüntülenen teklif ayrıntılarını tanımlamanız&#39;. Buna teklif adı, açıklama, görüntüler vb. dahildir. Bu teklifi yapılandırırken [Microsoft&#39;s ilke sayfasında](https://docs.microsoft.com/legal/marketplace/certification-policies#800-consulting-services) ayrıntılı ilkeleri izlediğinizden emin olun.

> [!NOTE]
> Teklif&#39;ayrıntıları, tümcecikle başlıyorsa, &quot;bu uygulama yalnızca [İngilizce olmayan dil] ' de kullanılabilir. &quot; Ayrıca, teklif listesi ayrıntılarında kullanılan bir dilde&#39;s 'nin içerik sunmak için yararlı bir bağlantı sağlamayı da&#39;.

### <a name="name"></a>Adı

Buraya girdiğiniz ad, teklifinizin başlığı olarak görüntülenir. Bu alan, teklifi oluştururken **teklif diğer adı** kutusuna girdiğiniz metin ile önceden doldurulur. Bu adı daha sonra değiştirebilirsiniz.

Ad:

- Trademarked olabilir (ve ticari marka veya telif hakkı sembolleri dahil edebilirsiniz).
- 50 karakterden daha uzun bir süre&#39;olabilir.
- &#39;, emojıs dahil edebilir.

### <a name="search-results-summary"></a>Arama sonuçları Özeti

Teklifinizin kısa bir açıklamasını sağlayın. Bu, 100 karakter uzunluğunda olabilir ve Market arama sonuçlarında kullanılır.

### <a name="description"></a>Açıklama

Teklifinizin en fazla 3.000 karakter uzunluğunda olması için daha uzun bir açıklama sağlayın. Bu, Market dökümüne Genel Bakış bölümündeki müşterilere görüntülenir.

Açıklamasında aşağıdakilerden birini veya birkaçını ekleyin:

- Teklifinizin sağladığı değer ve önemli avantajlar
- Kategori veya sektör ilişkilendirmeleri veya her ikisi
- Uygulama içi satın alma fırsatları
- Gerekli tüm kaldırlar

Tanımlarınızı yazmak için bazı ipuçları aşağıda verilmiştir:

- Açıklayamlarınızın ilk birkaç cümlede teklifinizin değerini açıkça açıklamalıdır. Aşağıdaki öğeleri ekleyin:
  - Teklifin açıklaması.
  - Tekliften faydalanan Kullanıcı türü.
  - Müşteri ihtiyaçları veya teklif adreslerini yayınlar.
- İlk birkaç tümcenin arama sonuçlarında görüntülenebileceğini unutmayın.
- Ürününüzü satmaya yönelik özelliklere ve işlevlere güvenmedim&#39;. Bunun yerine, teklifinizin sağladığı değere odaklanın.
- Sektöre özgü sözlük veya avantaj tabanlı bir ifade kullanmayı deneyin.

Açıklamanızı daha ilgi çekici hale getirmek için, tanımlarınızı biçimlendirmek için zengin metin düzenleyicisini kullanın. Zengin metin Düzenleyicisi, tanımlarınızın daha okunaklı olması için sayılar, madde işaretleri, kalın, italik ve girintiler eklemenize olanak tanır.

:::image type="content" source="media/cs-rich-text-editor.png" alt-text="Teklif açıklamasını yazmak için zengin metin düzenleyicisini gösterir." border="false":::

### <a name="keywords"></a>Anahtar Sözcükler

Birincil ürün ve danışmanlık hizmetinize uygun en fazla üç arama anahtar sözcüğü girin. Tekliflerinizi bulmayı daha kolay hale getirir.

### <a name="duration"></a>Süre

Bu katılım için beklenen süreyi müşterinizden ayarlayın.

### <a name="contact-information"></a>İletişim Bilgileri

**Birincil** ve **ikincil bir kişi**için ad, e-posta ve telefon numarası sağlamalısınız. Bu bilgiler, müşteriler tarafından gösterilen&#39;s. Microsoft tarafından kullanılabilir ve bulut çözümü sağlayıcısı (CSP) iş ortakları için sağlanmış olabilir.

### <a name="supporting-documents"></a>Destekleyici belgeler

Teklifiniz için PDF belgelerini destekleyen en fazla üç (ancak en az bir) ekleyin.

### <a name="marketplace-images"></a>Market görüntüleri

Teklifinizle birlikte kullanılacak logo ve görüntüler sağlayın. Tüm görüntülerin. png biçiminde olması gerekir. Bulanık görüntüler reddedilir.

>[!Note]
>Dosya yükleme sorununuz varsa, yerel ağınızın Iş Ortağı Merkezi tarafından kullanılan https://upload.xboxlive.com hizmeti engellemediğinden emin olun.

#### <a name="store-logos"></a>Mağaza logoları

Aşağıdaki piksel boyutlarının her birinde teklifinizin&#39;s logosunun. png dosyalarını sağlayın:

- **Küçük (48 x 48)**
- **Büyük (216 x 216)**

Tüm logolar gereklidir ve Market listesindeki farklı yerlerde kullanılır.

#### <a name="screenshots-optional"></a>Ekran görüntüleri (isteğe bağlı)

Teklifinizin nasıl çalıştığını gösteren en fazla beş ekran görüntüsü ekleyin. Her biri, boyut ve. png biçiminde 1280 x 720 piksel olmalıdır.

#### <a name="videos-optional"></a>Videolar (isteğe bağlı)

Teklifinizi gösteren dört adede kadar video ekleyin. Videonun video&#39;adını, Web adresini (URL) ve küçük resim. png görüntüsünü 1280 x 720 piksel boyutunda girin.

Sonraki bölüme, fiyatlandırmaya ve kullanılabilirliğine devam etmeden önce **Taslağı kaydet** ' i seçin.

## <a name="pricing-and-availability"></a>Fiyatlandırma ve kullanılabilirlik

Burada fiyatlandırma, Pazar ve özel anahtar gibi öğeleri tanımlayacaksınız.

1. **Pazar**: teklifinizin pazara açık olacağını belirleyin. Her teklif için yalnızca bir pazar seçebilirsiniz.
    1. Birleşik Devletler veya Kanada pazarlarında, teklifinizin nerede kullanılabilir olacağını belirtmek için **durumları Düzenle** ' yi (veya **ilçler**) seçin.
2. **Izleyiciyi Önizle**: teklifinizin özel kitlesini ayarlamak Için kullanılan **gizleme anahtarını** yapılandırın.
3. **Fiyatlandırma**: teklifinizin **ücretsiz** veya **ücretli** bir teklif olup olmadığını belirtin.

    > [!NOTE]
    > Danışmanlık hizmeti teklifleri yalnızca liste içindir. Tüm işlemler, ticari Market dışında doğrudan gerçekleşir.

4. Ücretli bir teklif için **Fiyat ve para birimini** ve fiyatın **sabit** veya **tahmin**edilip edilmeyeceğini belirtin. Tahmin varsa, fiyata hangi faktörlerin etkilediğini belirtmeniz gerekir.
5. **Taslağı kaydet**' i seçin.

## <a name="review-and-publish"></a>İnceleme ve yayımlama

Teklifin tüm gerekli bölümlerini tamamladıktan sonra, teklifinizi gözden geçirme ve yayımlama için gönderebilirsiniz.

1. Danışmanlık hizmeti teklifinizi yayımlamaya hazırsanız, **gözden geçir ve Yayımla**' ya tıklayın.
2. Son gönderme sayfasındaki ayrıntıları gözden geçirin.
3. Gerekirse, teklifinizin ayrıntılarından herhangi birine açıklama gerektirdiğini düşünüyorsanız, sertifika ekibine bir Note yazın.
4. Hazırsanız **Gönder**' i seçin.
5. **Teklifin genel bakış** sayfası, teklifinizin hangi yayımlama aşamasına ait olduğunu gösterir.

Teklifinizin her yayımlama aşamasında ne kadar süreyle beklendiğini öğrenmek için bkz. [ticari Market teklifinizin yayımlama durumunu denetleme](https://docs.microsoft.com/azure/marketplace/partner-center-portal/publishing-status).

## <a name="update-your-existing-consulting-service-offers"></a>Mevcut danışmanlık hizmeti tekliflerinizi güncelleştirin

- [Ticari Market 'te mevcut bir teklifi güncelleştirme](https://docs.microsoft.com/azure/marketplace/partner-center-portal/update-existing-offer)
