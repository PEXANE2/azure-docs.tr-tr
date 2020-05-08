---
title: Danışmanlık hizmeti teklifi oluşturma-Microsoft ticari Market
description: Iş Ortağı Merkezi 'ni kullanarak Microsoft AppSource veya Azure Marketi 'ne bir danışmanlık hizmeti teklifi yayımlamayı öğrenin.
author: anbene
ms.author: mingshen
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/06/2020
ms.openlocfilehash: 7e85175cbee53bd5984a38a587a90b98eaf8dcad
ms.sourcegitcommit: e0330ef620103256d39ca1426f09dd5bb39cd075
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/05/2020
ms.locfileid: "82791910"
---
# <a name="create-a-consulting-service-offer"></a>Danışmanlık hizmeti teklifi oluşturma

> [!IMPORTANT]
> Danışmanlık hizmeti tekliflerinizin yönetimini Bulut İş Ortağı Portalı Iş Ortağı Merkezi 'ne taşıdık. Tekliflerinizi geçirene kadar, tekliflerinizi yönetmek için lütfen [Azure ve Dynamics 365 danışmanlık hizmeti teklifiyle](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/consulting-services/cloud-partner-portal-consulting-services-publishing-offer) bulut iş ortağı portalı ilgili yönergeleri izleyin.

Bu makalede, [Microsoft AppSource](https://appsource.microsoft.com/) veya [Azure Market](https://azuremarketplace.microsoft.com/)'e bir danışmanlık hizmeti teklifinin nasıl yayımlanacağı açıklanır. AppSource 'ta Microsoft [Dynamics 365](https://dynamics.microsoft.com/) ve Power platform temelinde danışmanlık hizmeti tekliflerini listeleyin. Azure Marketi 'ndeki Microsoft Azure temel alan danışmanlık hizmeti tekliflerini listeleyin. Başlamadan önce, henüz yapmadıysanız [Iş Ortağı Merkezi 'nde bir ticari Market hesabı oluşturun](https://docs.microsoft.com/azure/marketplace/partner-center-portal/create-account) . Hesabınızın ticari Market programına kayıtlı olduğundan emin olun.

Teklifinizi oluşturmadan önce, [danışmanlık hizmeti ön koşulları](https://docs.microsoft.com/azure/marketplace/partner-center-portal/consulting-service-prerequisites)' nda önkoşulları gözden geçirin.

## <a name="publishing-benefits"></a>Yayımlama avantajları

Ticari Market 'e yayımlamanın avantajları:

- Microsoft markasını kullanarak şirketinizi yükseltin.
- AppSource 'ta 100.000.000 ' den fazla Office 365 ve Dynamics 365 kullanıcısı ve Azure Marketi aracılığıyla 200.000 ' ten fazla kuruluşa ulaşın.
- Bu Pazar yerlerden yüksek kaliteli müşteri adayları alın.
- Hizmetlerinizin Microsoft alanı ve Telesales ekipleri tarafından yükseltilme

## <a name="create-a-new-offer"></a>Yeni teklif oluşturma

1. [Iş Ortağı Merkezi](https://partner.microsoft.com/dashboard/home)' nde oturum açın.
2. Sol gezinti menüsünde **ticari Market** > **genel bakış**' ı seçin.
3. Genel Bakış sayfasında **+ yeni teklif** > **danışmanlık hizmeti**' ni seçin.

    ![Sol gezinti menüsünü gösterir.](./media/new-offer-consulting-service.png)

>[!NOTE]
>Bir teklif yayımlandıktan sonra, Iş Ortağı Merkezi 'nde yapılan düzenlemeler, teklif yeniden yayımlandıktan sonra yalnızca stoporonts 'de görüntülenir. Değişiklik yaptıktan sonra her zaman yeniden yayımlaytığınızdan emin olun.

## <a name="new-offer"></a>Yeni teklif

Bir **TEKLIF kimliği**girin. Bu, hesabınızdaki her teklif için benzersiz bir tanımlayıcıdır.

- Bu KIMLIK, Market teklifinin Web adresinde müşteriler tarafından görülebilir.
- Yalnızca küçük harfleri ve rakamları kullanın. Kısa çizgi ve alt çizgi içerebilir, ancak boşluk içeremez ve 50 karakterle sınırlıdır. Örneğin, **Test-teklif-1**girerseniz, teklif Web adresi olacaktır `https://azuremarketplace.microsoft.com/marketplace/../test-offer-1`.
- Teklif KIMLIĞI, **Oluştur**' u seçtikten sonra değiştirilemez.

Bir **teklif diğer adı**girin. Bu, Iş Ortağı Merkezi 'nde teklif için kullanılan addır.

- Bu ad Market 'te kullanılmıyor ve teklif adından ve müşterilere gösterilen diğer değerlerden farklı.
- Teklif diğer adı, **Oluştur**' u seçtikten sonra değiştirilemez.

Teklifi oluşturmak için **Oluştur** ' u seçin ve devam edin.

## <a name="offer-setup"></a>Teklif kurulumu

### <a name="connect-lead-management"></a>Müşteri adayı yönetimini bağlama

Teklifinizi Iş Ortağı Merkezi ile Market 'te yayımlarken bir Müşteri Ilişkileri yönetimi (CRM) veya pazarlama otomasyonu sistemine bağlamanız _gerekir_ . Bu, bir kişi, ürününüzü ilgilendiren veya ürünü kullandığı anda müşteri iletişim bilgilerini almanızı sağlar.

1. Müşteri adaylarını nereden göndermemizi istediğinizi belirtmek için **Bağlan** ' ı seçin. İş Ortağı Merkezi aşağıdaki sistemleri destekler:

    - Müşteri katılımı için [Dynamics 365](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-lead-management-instructions-dynamics)
    - [Marketo](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-lead-management-instructions-marketo)
    - [Salesforce](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-lead-management-instructions-salesforce)

    > [!NOTE]
    > CRM sisteminiz yukarıda listelenmiyorsa, müşteri adayı verilerini depolamak için [Azure tablosu](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-lead-management-instructions-azure-table) veya [https uç noktasını](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-lead-management-instructions-https) kullanın, ardından verileri CRM sisteminize dışarı aktarın.

2. Teklifinizi Iş Ortağı Merkezi 'nde yayımlarken lider hedefine bağlayın.
3. Müşteri adayı hedefi bağlantısının düzgün şekilde yapılandırıldığını doğrulayın. Iş Ortağı Merkezi 'nde yayımladıktan sonra, bağlantıyı doğrulayacağız ve size bir test lideri göndereceğiz. Teklifi canlı olmadan önce önizlerken, teklifi önizleme ortamında satın almayı deneyerek lider bağlantınızı da test edebilirsiniz.
4. Müşteri adayı hedefine olan bağlantının güncelleştirildiğinden emin olun, böylece müşteri adaylarını kaybetmezsiniz.

Bazı ek müşteri yönetimi kaynakları aşağıda verilmiştir:

- [Müşteri adayı yönetimine genel bakış](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-get-customer-leads)
- [Müşteri adayı yönetimi hakkında SSS](https://docs.microsoft.com/azure/marketplace/lead-management-for-cloud-marketplace#frequently-asked-questions)
- [Sık karşılaşılan müşteri adayı yapılandırma hataları](https://docs.microsoft.com/azure/marketplace/lead-management-for-cloud-marketplace#publishing-config-errors)
- [Müşteri adayı yönetimine genel bakış](https://assetsprod.microsoft.com/mpn/cloud-marketplace-lead-management.pdf) PDF (açılır pencere engelleyicinizin kapalı olduğundan emin olun)

Devam etmeden önce **Taslağı kaydet** ' i seçin.

### <a name="properties"></a>Özellikler

Bu sayfa, danışmanlık hizmetinizin en iyi şekilde kapsaabileceği birincil ürünü ayarlamanıza, bir danışmanlık hizmeti türü ayarlayabilmenizi ve geçerli ürünleri seçmenizi sağlar.

1. Açılan listeden bir **birincil ürün** seçin.
2. Açılan listeden bir **danışmanlık hizmeti türü** seçin:

    - **Değerlendirme**: bir çözümün uygulanabilirliğini tespit etmek ve maliyet ve zamanlama tahminini sağlamak için bir müşterinin ortamının değerlendirmesi.
    - **Briing**: çerçeveler, tanıtımlar ve müşteri örneklerini kullanarak müşteri ilgilenmesini çizmek için bir çözüme veya bir danışmanlık hizmetine giriş.
    - **Uygulama**: tam olarak çalışan bir çözüme neden olan tam bir yükleme. İki hafta veya daha az bir şekilde uygulanabilen çözümlerle sınırlayın.
    - **Kavram kanıtı**: bir çözümün müşteri gereksinimlerini karşılayıp karşılamadığını tespit etmek için sınırlı kapsamlı bir uygulama.
    - **Workshop**: bir müşterinin şirket içinde etkileşimli bir katılım yürütülür. Müşterinin verileri veya ortamında oluşturulan eğitim, briler, değerlendirmeler veya tanıtımlar içerebilir.

1. **Azure**'ın birincil bir ürününü seçtiyseniz en fazla üç **çözüm alanı**seçin. Bu, Azure Marketi 'ndeki müşterilerin teklifinizi bulmasını kolaylaştırır. Azure 'u seçmediyseniz, bu adımı atlayın.
2. Azure _dışında bir birincil ürün seçtiyseniz_ , en fazla üç **uygulanabilir ürünü**seçin. Bu, AppSource 'taki müşterilerin teklifinizi bulmasını kolaylaştırır. Ayrıntılar için bkz. [Microsoft AppSource danışmanlık hizmeti listeleme yönergeleri](https://go.microsoft.com/fwlink/?LinkId=828734&amp;clcid=0x409) (PDF).
3. Teklifinizin uygulanacağı en fazla altı **sektör** seçin. Bu, müşterilerin teklifinizi bulmasını kolaylaştırır.
4. Şirketinizin danışmanlık hizmeti teklifi listesinde görüntülenmek üzere kazanıldığını en fazla üç **uzmanlık** ekleyin. Azure Expert MSPs ve Azure ağı MSPs hariç en az bir yetenek gereklidir.

Devam etmeden önce **Taslağı kaydet** ' i seçin.

## <a name="offer-listing"></a>Teklif listesi

Burada, Market 'te görüntülenen teklif ayrıntılarını tanımlayacaksınız. Buna teklif adı, açıklama, görüntüler vb. dahildir. Bu teklifi yapılandırırken [ticari Market sertifika ilkeleri sayfasında](https://docs.microsoft.com/legal/marketplace/certification-policies#800-consulting-services) ayrıntılı ilkeleri izlediğinizden emin olun.

> [!NOTE]
> Teklif ayrıntılarının, tümcesiyle birlikte başlaması durumunda, &quot;bu uygulamanın yalnızca [İngilizce olmayan dil] bölümünde kullanılabilmesi için teklif ayrıntılarının İngilizce olması gerekmez. &quot; Ayrıca, teklif listesi ayrıntılarından farklı bir dilde içerik sunmak için faydalı bir bağlantı sağlamak da yararlı olabilir.

### <a name="name"></a>Name

Buraya girdiğiniz ad, teklifinizin başlığı olarak görüntülenir. Bu alan, teklifi oluştururken **teklif diğer adı** kutusuna girdiğiniz metin ile önceden doldurulur. Bu adı daha sonra değiştirebilirsiniz.

Ad:

- Trademarked olabilir (ve ticari marka veya telif hakkı sembolleri dahil edebilirsiniz).
- 50 karakterden uzun olamaz.
- Emojıs dahil olamaz.

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
- Ürününüzü satmaya yönelik özelliklere ve işlevlere güvenmeyin. Bunun yerine, teklifinizin sağladığı değere odaklanın.
- Sektöre özgü sözlük veya avantaj tabanlı bir ifade kullanmayı deneyin.

Teklif açıklamanızı daha ilgi çekici hale getirmek için, biçimlendirme uygulamak için zengin metin düzenleyicisini kullanın.

![Zengin metin düzenleyicisini kullanma](./media/rich-text-editor.png)

| <center>Metin biçimini değiştir | <center>Madde işaretleri veya numaralandırma ekleme | <center>Metin girintisi ekleme veya kaldırma |
| --- | --- | --- |
| <center>![Metin biçimini değiştirmek için zengin metin düzenleyicisini kullanma](./media/text-editor3.png) |  <center>![Liste eklemek için zengin metin düzenleyicisini kullanma](./media/text-editor4.png) |  <center>![Girintilemek için zengin metin düzenleyicisini kullanma](./media/text-editor5.png) |

### <a name="keywords"></a>Anahtar sözcükler

Birincil ürün ve danışmanlık hizmetinize uygun en fazla üç arama anahtar sözcüğü girin. Tekliflerinizi bulmayı daha kolay hale getirir.

### <a name="duration"></a>Süre

Bu katılım için beklenen süreyi müşterinizden ayarlayın.

### <a name="contact-information"></a>İletişim Bilgileri

**Birincil** ve **ikincil bir kişi**için ad, e-posta ve telefon numarası sağlamalısınız. Bu bilgiler müşterilere gösterilmez. Microsoft tarafından kullanılabilir ve bulut çözümü sağlayıcısı (CSP) iş ortakları için sağlanmış olabilir.

### <a name="supporting-documents"></a>Destekleyici belgeler

Teklifiniz için PDF belgelerini destekleyen en fazla üç (ancak en az bir) ekleyin.

### <a name="marketplace-images"></a>Market görüntüleri

Teklifinizle birlikte kullanılacak logo ve görüntüler sağlayın. Tüm görüntülerin. png biçiminde olması gerekir. Bulanık görüntüler reddedilir.

>[!Note]
>Dosya yükleme sorununuz varsa, yerel ağınızın Iş Ortağı Merkezi tarafından kullanılan https://upload.xboxlive.com hizmeti engellemediğinden emin olun.

#### <a name="store-logos"></a>Mağaza logoları

Aşağıdaki piksel boyutlarının her birinde teklifinizin logosunun PNG dosyalarını sağlayın:

- **Küçük (48 x 48)**
- **Büyük (216 x 216)**

Tüm logolar gereklidir ve Market listesindeki farklı yerlerde kullanılır.

#### <a name="screenshots-optional"></a>Ekran görüntüleri (isteğe bağlı)

Teklifinizin nasıl çalıştığını gösteren en fazla beş ekran görüntüsü ekleyin. Her biri, boyut ve PNG biçiminde 1280 x 720 piksel olmalıdır.

#### <a name="videos-optional"></a>Videolar (isteğe bağlı)

Teklifinizi gösteren dört adede kadar video ekleyin. Videonun adını, Web adresini (URL) ve videonun küçük resim PNG görüntüsünü 1280 x 720 piksel boyutunda girin.

Devam etmeden önce **Taslağı kaydet** ' i seçin.

## <a name="pricing-and-availability"></a>Fiyatlandırma ve kullanılabilirlik

Burada fiyatlandırma, Pazar ve özel anahtar gibi öğeleri tanımlayacaksınız.

1. **Pazar**: teklifinizin pazara açık olacağını belirleyin. Her teklif için yalnızca bir pazar seçebilirsiniz.
    1. Birleşik Devletler veya Kanada pazarlarında, teklifinizin nerede kullanılabilir olacağını belirtmek için **durumları Düzenle** ' yi (veya **ilçler**) seçin.
2. **Izleyiciyi Önizle**: teklifinizin özel kitlesini ayarlamak Için kullanılan **gizleme anahtarını** yapılandırın.
3. **Fiyatlandırma**: teklifinizin **ücretsiz** veya **ücretli** bir teklif olup olmadığını belirtin.

    > [!NOTE]
    > Danışmanlık hizmeti teklifleri yalnızca liste içindir. Tüm işlemler, ticari Market dışında doğrudan gerçekleşir.

4. Ücretli bir teklif için **Fiyat ve para birimini** ve fiyatın **sabit** veya **tahmin**edilip edilmeyeceğini belirtin. Tahmin varsa, fiyata hangi faktörlerin etkilediğini belirtmeniz gerekir.
5. Devam etmeden önce **Taslağı kaydet** ' i seçin.

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
