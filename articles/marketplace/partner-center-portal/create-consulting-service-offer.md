---
title: İş Ortağı Merkezi-Azure Marketi'nde Danışmanlık hizmeti teklifi oluşturun
description: İş Ortağı Merkezi'ni kullanarak Azure Marketi veya AppSource'a danışmanlık hizmeti teklifini nasıl yayınlayacağınızı öğrenin.
author: anbene
ms.author: mingshen
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/06/2020
ms.openlocfilehash: eff37750f0580a28c9644ee1ffb7fe4e95038709
ms.sourcegitcommit: af1cbaaa4f0faa53f91fbde4d6009ffb7662f7eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81869792"
---
# <a name="create-a-consulting-service-offer"></a>Danışmanlık hizmeti teklifi oluşturun

> [!IMPORTANT]
> Danışmanlık hizmet tekliflerinizin yönetimini Cloud Partner Portal'dan İş Ortağı Merkezi'ne taşıyoruz. Tekliflerin izin verilene kadar, bulut iş ortağı portalının tekliflerinizi yönetmesi için [Azure ve Dynamics 365 danışmanlık hizmeti teklifindeki](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/consulting-services/cloud-partner-portal-consulting-services-publishing-offer) talimatları izleyin.

Bu makalede, Bir Danışmanlık hizmeti [teklifinin Azure Marketi](https://azuremarketplace.microsoft.com/) veya [AppSource'a](https://appsource.microsoft.com/)nasıl yayımlanılabildiğini açıklanmaktadır. AppSource'ta Microsoft [Dynamics 365](https://dynamics.microsoft.com/) ve Power Platform'u temel alan List Consulting hizmeti teklifleri. Azure Marketinde Microsoft Azure'u temel alan List Danışmanlık hizmeti teklifleri.

Azure Marketi veya AppSource Danışmanlık hizmetlerinde bir Danışmanlık hizmeti teklifi oluşturmak için öncelikle [İş Merkezi'nde bir yayımcı hesabınız](https://docs.microsoft.com/azure/marketplace/partner-center-portal/create-account)olması ve hesabınızın ticari pazar programına kaydolması gerekir. Teklifinizi oluşturmadan önce, Danışmanlık hizmeti [ön koşullarındaki](https://docs.microsoft.com/azure/marketplace/partner-center-portal/consulting-service-prerequisites)ön koşulları gözden geçirin.

## <a name="publishing-benefits"></a>Yararlanma avantajları

Ticari pazara yayıncılığın faydaları:

- Microsoft markasını kullanarak şirketinizi tanıtın.
- Azure Marketi aracılığıyla AppSource'ta 100 milyondan fazla Office 365 ve Dynamics 365 kullanıcısına ve 200.000'den fazla kuruluşa ulaşma potansiyeline sahip.
- Bu pazarlardan yüksek kaliteli müşteri adayları alın.
- Hizmetlerinizin Microsoft alanı ve tele-satış ekipleri tarafından tanıtınır

## <a name="create-a-new-offer"></a>Yeni teklif oluşturma

Yukarıda açıklanan gereksinimleri yerine getirin, bir Danışmanlık hizmeti teklifi oluşturmak için aşağıdaki adımları izleyin.

1. [Ortak Merkezi'nde](https://partner.microsoft.com)oturum açın ve ardından üst menüden **Pano'yu** seçin.
2. Sol daki gezinme **çubuğunda, Commercial Marketplace'i**seçin ve ardından **Genel Bakış'ı**seçin.

    :::image type="content" source="media/cs-menu-overview.png" alt-text="Ticari pazar için menüyü gösterir":::

3. + **Yeni Teklif**seçin, ardından Danışmanlık **hizmetini**seçin.

    :::image type="content" source="media/cs-menu-newoffer.png" alt-text="Yeni bir teklif oluşturmak için düğmeyi gösterir.":::

4. Teklif **Kimliği**girin. Bu, hesabınızdaki her teklif için benzersiz bir tanımlayıcıdır.

    - Bu kimlik, pazar teklifi için web adresindeki müşteriler tarafından görülebilir.
    - Yalnızca küçük harfleri, sayıları, tireleri ve alt çiziyi kullanın, ancak boşluk yok. Uzunluğu 50 karakterle sınırlıdır. Örneğin, **test-teklif-1**girerseniz, teklif URL'si `https://azuremarketplace.microsoft.com/marketplace/../test-offer-1`.
    - **Teklif Kimliği, Oluştur'u**seçtikten sonra değiştirilemez&#39;.

5. Teklif **takma adını**girin. Bu, Ortak Merkezi'ndeki teklifi ifade etmek için kullanılan addır.

    - Bu ad&#39;pazarda kullanılmaz. Teklif adından ve müşterilere gösterilen diğer değerlerden farklı&#39;. Teklifi dahili olarak tanımlamanız için sizin için daha yararlı olan teklife bir ad atamak için bu alanı kullanabilirsiniz; müşterilere gösterilmez.
    - Teklif diğer **adı, Oluştur'u**seçtikten sonra değiştirilemez&#39;.

Bu iki değeri girdikten sonra Teklif **kurulum** sayfasına devam etmek için **Oluştur'u** seçin.

## <a name="offer-setup"></a>Teklif kurulumu

Teklif Kimliği ve Teklif takma adını girdikten sonra, İş Ortağı Merkezi bir taslak teklif oluşturur ve **Teklif kurulum** sayfasını görüntüler. Teklifinizi ayarlamak için aşağıdaki adımları izleyin.

### <a name="connect-lead-management"></a>Müşteri adayı yönetimini bağlayın

Teklifinizi İş Ortağı Merkezi ile pazara yayınlarken, teklifi bir Müşteri İlişkileri Yönetimi (CRM) veya pazarlama otomasyon sistemine bağlamanız _gerekir._ Bu, birisi ürününüzün ilgisini ifade eder etmez veya ürününüzü kullanır kullanmaz müşteri iletişim bilgilerini almanıza izin verebebilirsiniz.

1. Müşteri müşteri adaylarını nereye göndermemizi istediğinizi belirtmek için **Bağlan'ı** seçin. İş Ortağı Merkezi aşağıdaki sistemleri destekler:

    - Müşteri Katılımı için [Dynamics 365](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-lead-management-instructions-dynamics)
    - [Marketo](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-lead-management-instructions-marketo)
    - [Salesforce](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-lead-management-instructions-salesforce)

    > [!NOTE]
    > CRM sisteminiz yukarıda listelenmemiş&#39;, müşteri müşteri adayı verilerini depolamak için [Azure Tablosu](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-lead-management-instructions-azure-table) veya [Https Endpoint'i](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-lead-management-instructions-https) kullanın ve verileri CRM sisteminize aktarın.

2. İş Ortağı Merkezi'nde yayın yaparken teklifinizi müşteri adayı hedefe bağlayın.
3. Müşteri adayı hedefine bağlantının düzgün şekilde yapılandırıldığından onaylayın. İş Ortağı Merkezi'nde yayımladıktan sonra, bağlantıyı&#39;ve size bir test müşteri adayı göndereceğiz. Teklifi yayına girmeden önce önizlemenize kadar, teklifi önizleme ortamında kendiniz satın almaya çalışarak müşteri adayı bağlantınızı da test edebilirsiniz.
4. Müşteri adayı nı kaybetmemek&#39;için müşteri adayı hedefine bağlantının güncel kaldığından emin olun.

Aşağıda bazı ek müşteri adayı yönetim kaynakları vereme

- [Müşteri adayı yönetimine genel bakış](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-get-customer-leads)
- [Müşteri adayı yönetimi hakkında SSS](https://docs.microsoft.com/azure/marketplace/lead-management-for-cloud-marketplace#frequently-asked-questions)
- [Sık karşılaşılan müşteri adayı yapılandırma hataları](https://docs.microsoft.com/azure/marketplace/lead-management-for-cloud-marketplace#common-lead-configuration-errors-during-publishing-on-cloud-partner-portal)
- [Müşteri Adayı Yönetimine Genel Bakış](https://assetsprod.microsoft.com/mpn/cloud-marketplace-lead-management.pdf) PDF (Açılır pencere engelleyicinizin kapalı olduğundan emin olun)

Özellikler adlı bir sonraki bölüme devam etmeden önce **taslağı kaydet'i** seçin.

### <a name="properties"></a>Özellikler

Bu sayfa, Danışmanlık hizmetinizin sunduğu birincil ürünü en iyi şekilde kapsamanızı, Bir Danışmanlık hizmet türünü belirlemenizi ve geçerli ürünleri seçmenize olanak tanır.

1. Açılan listeden birincil **ürün** seçin.
2. Açılan listeden bir **Danışmanlık hizmet türü** seçin:

    - **Değerlendirme** : Bir çözümün uygulanabilirliğini belirlemek ve maliyet ve zamanlama tahmini sağlamak için müşterinin&#39;ortamının değerlendirilmesi.
    - **Brifing** : Çerçeveler, demolar ve müşteri örneklerini kullanarak müşteri ilgisini çekmek için bir çözüm e-çözüm veya danışmanlık hizmetine giriş.
    - **Uygulama** : Tam çalışan bir çözümle sonuçlanan tam bir kurulum. İki hafta veya daha kısa sürede uygulanabilecek çözümleri sınırlandırın.
    - **Kavram Kanıtı** : Bir çözümün müşteri gereksinimlerini karşılayıp karşılamadığını belirlemek için sınırlı kapsamlı bir uygulamadır.
    - **Atölye** : Müşteri&#39;tesislerinde yapılan interaktif bir etkileşim. Müşteri&#39;verileri veya ortamı üzerine kurulu eğitim, brifingler, değerlendirmeler veya demolar içerebilir.

1. **Azure'un**birincil ürününü seçtiyseniz, en fazla üç **Çözüm Alanı**seçin. Bunlar, Azure Marketi'ndeki müşterilerin teklifinizi bulmasını kolaylaştırır. Azure'u seçmediyseniz&#39;, bu adımı atlayın.
2. Azure _dışında_ birincil bir ürün seçtiyseniz, en fazla üç **Geçerli ürün**seçin. Bunlar, AppSource'taki müşterilerin teklifinizi bulmasını kolaylaştırır. Ayrıntılar için [Microsoft AppSource Danışmanlık Hizmeti Listeleme Yönergeleri'ne](https://go.microsoft.com/fwlink/?LinkId=828734&amp;clcid=0x409) (PDF) bakın.
3. Teklifinizin geçerli olduğu en fazla altı **Endüstri'yi** seçin. Bu, müşterilerin teklifinizi bulmasını kolaylaştırır.
4. Şirketinizin Danışmanlık hizmeti teklif listenizde görüntülemek için kazandığı en fazla üç **Yetkinlik** ekleyin. Azure Uzman MSP&#39;ve Azure Ağ MSP's&#39;dışında en az bir uzmanlık gereklidir.

Bir sonraki bölüme, Teklif listesine devam etmeden önce **taslağı Kaydet'i** seçin.

## <a name="offer-listing"></a>Teklif listesi

Burada&#39;pazarda görüntülenen teklif ayrıntılarını tanımlarsınız. Bu teklif adı, açıklama, görüntüler ve benzeri içerir. Bu teklifi yapılandırırken [Microsoft&#39;ilke sayfasında](https://docs.microsoft.com/legal/marketplace/certification-policies#800-consulting-services) ayrıntılı olarak belirtilen ilkeleri uyguladığından emin olun.

> [!NOTE]
> Teklif açıklaması ifadeyle başlarsa, teklif ayrıntıları&#39;İngilizce olması &quot;gerekmez, bu uygulama yalnızca [İngilizce olmayan dilde] kullanılabilir. &quot; Teklif liste ayrıntılarında kullanılandan farklı&#39;bir dilde içerik sunmak için Yararlı Bir Bağlantı sağlamanın da sakıncası&#39;.

### <a name="name"></a>Adı

Buraya girdiğiniz ad, teklifinizin başlığı olarak görüntülenir. Bu alan, teklifi oluşturduğunuzda Teklif **takma adı** kutusuna girdiğiniz metinle önceden doldurulmuştur. Bu adı daha sonra değiştirebilirsiniz.

Adı:

- Ticari marka olabilir (ve ticari marka veya telif hakkı sembolleri dahil edebilirsiniz).
- &#39;50 karakterden uzun olamaz.
- &#39;emojis içeremez.

### <a name="search-results-summary"></a>Arama sonuçları özeti

Teklifinizin kısa bir açıklamasını sağlayın. Bu, 100 karaktere kadar uzun olabilir ve pazar yeri arama sonuçlarında kullanılır.

### <a name="description"></a>Açıklama

Teklifinizin 3.000 karaktere kadar daha uzun bir açıklamasını sağlayın. Bu, pazar yeri giriş özetinde müşterilere görüntülenir.

Açıklamanıza aşağıdakilerden birini veya birkaçını ekleyin:

- Teklifinizin sağladığı değer ve önemli avantajlar
- Kategori veya endüstri dernekleri veya her ikisi
- Uygulama içi satın alma fırsatları
- Gerekli açıklamalar

Açıklamanızı yazmak için bazı ipuçları aşağıda verilmiştir:

- Teklifinizin değerini açıklamanızın ilk birkaç cümlesinde açıkça açıklayın. Aşağıdaki öğeleri ekleyin:
  - Teklifin açıklaması.
  - Tekliften yararlanan kullanıcı türü.
  - Müşteri ihtiyaçları veya teklif adreslerini sorunları.
- İlk birkaç cümlenin arama sonuçlarında görüntülenebileceğini unutmayın.
- &#39;ürününüzü satmak için özelliklere ve işlevselliğe güvenmeyin. Bunun yerine, teklifinizin sağladığı değere odaklanın.
- Sektöre özgü kelime dağarcığı veya fayda tabanlı ifadeler kullanmayı deneyin.

Açıklamanızı daha ilgi çekici hale getirmek için, açıklamanızı biçimlendirmek için zengin metin düzenleyicisini kullanın. Zengin metin düzenleyicisi, açıklamanızı daha okunabilir hale getirmek için sayılar, madde işaretleri, kalın, italik ler ve girintiler eklemenizi sağlar.

:::image type="content" source="media/cs-rich-text-editor.png" alt-text="Teklif açıklamasını yazmak için zengin metin düzenleyicisini gösterir." border="false":::

### <a name="keywords"></a>Anahtar sözcükler

Birincil ürününuz ve Danışmanlık hizmetinizle alakalı en fazla üç arama anahtar kelimesini girin. Bunlar teklifinizi bulmanızı kolaylaştıracaktır.

### <a name="duration"></a>Süre

Müşterinizle bu etkileşimin beklenen süresini ayarlayın.

### <a name="contact-information"></a>İletişim Bilgileri

**Birincil** ve **İkincil bir kişi**için ad, e-posta ve telefon numarası sağlamanız gerekir. Bu bilgiler müşterilere gösterilmeden&#39;. Microsoft tarafından kullanılabilir ve Bulut Çözüm Sağlayıcısı (CSP) iş ortaklarına sağlanabilir.

### <a name="supporting-documents"></a>Destekleyici belgeler

Teklifiniz için en fazla üç (ama en az bir) destekleyici PDF belgesi ekleyin.

### <a name="marketplace-images"></a>Pazar yeri görüntüleri

Teklifinizle birlikte kullanılacak logolar ve resimler sağlayın. Tüm görüntüler .png formatında olmalıdır. Bulanık görüntüler reddedilir.

>[!Note]
>Dosya yükleme sorunu yaşıyorsanız, yerel ağınızın İş Ortağı https://upload.xboxlive.com Merkezi tarafından kullanılan hizmeti engellemediğinden emin olun.

#### <a name="store-logos"></a>Mağaza logoları

Teklifinizin .png dosyalarını aşağıdaki piksel boyutlarının her birinde&#39;logosunu sağlayın:

- **Küçük (48 x 48)**
- **Büyük (216 x 216)**

Tüm logolar gereklidir ve pazar listesinde farklı yerlerde kullanılır.

#### <a name="screenshots-optional"></a>Ekran görüntüleri (isteğe bağlı)

Teklifinizin nasıl çalıştığını gösteren en fazla beş ekran görüntüsü ekleyin. Her biri 1280 x 720 piksel boyutunda ve .png formatında olmalıdır.

#### <a name="videos-optional"></a>Videolar (isteğe bağlı)

Teklifinizi gösteren en fazla dört video ekleyin. Video&#39;adını, web adresini (URL) ve videonun 1280 x 720 piksel boyutunda küçük resmi .png görüntüsünü girin.

Bir sonraki bölüme, Fiyatlandırmaya ve kullanılabilirlike devam etmeden önce **taslağı Kaydet'i** seçin.

## <a name="pricing-and-availability"></a>Fiyatlandırma ve kullanılabilirlik

Burada fiyatlandırma, pazar ve özel anahtar gibi öğeleri tanımlarsınız.

1. **Pazar**: Teklifinizin mevcut olacağı piyasayı ayarlayın. Teklif başına yalnızca bir pazar seçebilirsiniz.
    1. Amerika Birleşik Devletleri veya Kanada pazarları için, teklifinizin nerede sunulacağını belirtmek için devletleri (veya **eyaletleri)** **edit'i** seçin.
2. **Önizleme Hedef Kitle**: Teklifiniz için özel hedef kitleyi ayarlamak için kullanılan **Gizli Anahtarı** yapılandırın.
3. **Fiyatlandırma**: Teklifinizin **Ücretsiz** mi yoksa **Ücretli** mi olduğunu belirtin.

    > [!NOTE]
    > Danışmanlık hizmeti teklifleri sadece liste içindir. Herhangi bir işlem doğrudan, ticari pazar dışında gerçekleşecektir.

4. Ücretli bir teklif için **Fiyatı ve para birimini ve** fiyatın **Sabit** mi yoksa **Tahmini**mi olduğunu belirtin. Tahmin edilirse, açıklamada fiyatı hangi faktörlerin etkileyeceğini belirtmeniz gerekir.
5. **Taslağı Kaydet'i**seçin.

## <a name="review-and-publish"></a>İnceleme ve yayınlama

Teklifin gerekli tüm bölümlerini tamamladıktan sonra, teklifinizi incelemek ve yayınlamak için gönderebilirsiniz.

1. Danışmanlık hizmeti teklifinizi yayınlamaya hazır olduğunuzda, **Gözden Geçir'i tıklatın ve yayınlayın.**
2. Son gönderim sayfasındaki ayrıntıları gözden geçirin.
3. Gerekirse, teklifinizin herhangi bir ayrıntısının açıklama gerektirdiğini düşünüyorsanız sertifika ekibine bir not yazın.
4. Hazır olduğunuzda **Gönder'i**seçin.
5. **Teklife genel bakış** sayfası, teklifinizin hangi yayımlama aşamasında olduğunu gösterir.

Teklifinizin her yayımlama aşamasında ne kadar süreyle olmasını bekleyebileceğiniz hakkında daha fazla bilgi [için](https://docs.microsoft.com/azure/marketplace/partner-center-portal/publishing-status)bkz.

## <a name="update-your-existing-consulting-service-offers"></a>Mevcut Danışmanlık hizmeti tekliflerinizi güncelleyin

- [Ticari pazarda mevcut bir teklifi güncelleştirme](https://docs.microsoft.com/azure/marketplace/partner-center-portal/update-existing-offer)
