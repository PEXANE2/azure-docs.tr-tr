---
title: Ticari Pazar Da yeni bir Yönetilen Hizmet teklifi oluşturun
description: İş Ortağı Merkezi'ndeki Ticari Pazar Portalı'nı kullanarak Azure Market'te giriş yapmak için yeni bir Yönetilen Hizmet teklifi oluşturma.
author: JnHs
ms.author: jenhayes
manager: evansma
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 03/30/2020
ms.openlocfilehash: 3d7015d5e1c3b67b271e89b5653f56415bfb722e
ms.sourcegitcommit: b129186667a696134d3b93363f8f92d175d51475
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80674208"
---
# <a name="create-a-new-managed-service-offer"></a>Yeni yönetilen hizmet teklifi oluşturma

Yönetilen Hizmet, [Azure Deniz Feneri](../../lighthouse/overview.md) senaryolarını etkinleştirmeye yardımcı olur. Bir müşteri Yönetilen Hizmet teklifini kabul ettiğinde, Azure temsilci [kaynak yönetimi](../../lighthouse/concepts/azure-delegated-resource-management.md)için kaynaklara binebilir.

Yönetilen Hizmet teklifleri oluşturmaya başlamak için, ilk önce [Bir İş Ortağı Merkezi hesabı oluşturduğunuzdan](./create-account.md) ve Seçilen Genel **Bakış** sayfasıyla Ticari [Pazar panosunu](https://partner.microsoft.com/dashboard/commercial-marketplace/offers)açtığınızı sağlayın. Yönetilen Hizmet teklifini yayınlamak için [Gümüş veya Altın Bulut Platformu uzmanlık düzeyine](https://partner.microsoft.com/membership/cloud-platform-competency) sahip olmalısınız veya Azure Uzmanı [MSP](https://partner.microsoft.com/membership/azure-expert-msp) olmalısınız.

![İş Ortağı Merkezi'nde Ticari Pazar panosu](./media/new-offer-overview.png)

>[!Note]
> Bir teklif yayımlandıktan sonra, Ortak Merkezi'nde yapılan teklife yönelik yapılan güncellemeler yalnızca yeniden yayımlandıktan sonra sistemde ve mağaza önlerinde güncellenir. Lütfen değişiklik yaptıktan sonra teklifi yayınlanmak üzere gönderdiğinden emin olun.

## <a name="create-a-new-offer"></a>Yeni teklif oluşturma

+ **Yeni teklif** düğmesini seçin ve ardından **Yönetilen Hizmet** menü öğesini seçin. **Yeni teklif** iletişim kutusu görüntülenir.

### <a name="offer-id-and-alias"></a>Teklif kimliği ve takma ad

- **Teklif Kimliği**: Hesabınızdaki her teklif için benzersiz tanımlayıcı. Bu kimlik, pazar teklifiiçin URL adresindeki müşteriler tarafından görülebilir. Bu kimlik yalnızca küçük alfasayısal karakterler (tireler ve alt çizerler dahil, ancak beyaz boşluk yok), 50 karakterle sınırlı olabilir ve **Oluştur'u**seçtikten sonra değiştirilemez.  Örneğin, *burada test-teklif-1* girerseniz, teklif URL'si `https://azuremarketplace.microsoft.com/marketplace/../test-offer-1`.

- **Teklif diğer adı**: Ortak Merkezi'ndeki teklife atıfta bulunmak için kullanılan ad. Bu ad pazarda kullanılmaz ve teklif adı ve müşterilere gösterilecek diğer değerlerden farklıdır. **Oluştur'u**seçtikten sonra bu değer değiştirilemez.

**Teklif Kimliği** nizi ve **Teklif takma**adını girdikten sonra **Oluştur'u**seçin. Daha sonra teklifinizin tüm farklı kısımları üzerinde çalışabileceksiniz.

## <a name="offer-setup"></a>Teklif kurulumu

**Teklif kurulum** sayfası aşağıdaki bilgileri ister. Bu alanları tamamladıktan sonra **Kaydet'i** seçtiğinizden emin olun.

## <a name="connect-lead-management"></a>Müşteri adayı yönetimini bağlayın

[!INCLUDE [Connect lead management](./includes/connect-lead-management.md)]

Yönetilen Hizmetler [sertifika ilkelerine](https://docs.microsoft.com/legal/marketplace/certification-policies#700-managed-services)göre bir **Müşteri Adayı Hedefi** gerektiğini unutmayın. Bu, bir müşteri teklifinizi her dağıtında CRM sisteminizde bir kayıt oluşturur.

Daha fazla bilgi için Müşteri [Adayı yönetimine genel bakış](./commercial-marketplace-get-customer-leads.md)adabına bakın.

Bir sonraki bölüme geçmeden önce sayfayı **kaydetmeyi** unutmayın.

## <a name="properties"></a>Özellikler

**Özellikler** sayfası, teklifinizi pazarda gruplandırmak için kullanılan kategorileri ve teklifinizi destekleyen yasal sözleşmeleri tanımlamanızı sağlar. Bu sayfayı tamamladıktan sonra **Kaydet'i** seçin.

### <a name="category"></a>Kategori

Teklifinizi uygun pazar yeri arama alanlarına yerleştirmek için kullanılacak en az bir ve en fazla beş kategori seçin. Teklifinizin teklif açıklamasında bu kategorileri nasıl desteklediğini mutlaka unutmayın.

### <a name="terms-and-conditions"></a>Hüküm ve koşullar

**Şartlar ve koşullar** alanında kendi yasal hüküm ve koşullarınızı sağlayın. Ayrıca, şart ve koşullarınızın bulunabileceği URL'yi de sağlayabilirsiniz. Müşterilerin teklifinizi denemeden önce bu koşulları kabul etmeleri gerekmektedir.

## <a name="offer-listing"></a>Teklif listesi

**Teklif listesi** sayfası, teklifiniz için pazar yeri ayrıntılarını (teklif adı, açıklama, resim vb.) tanımlamanızı sağlar.

> [!NOTE]
> Teklif listesi içeriği (açıklama, belge, ekran görüntüsü, kullanım koşulları, vb.) "Bu uygulama yalnızca [İngilizce olmayan dilde] ifadesiyle başladığı sürece İngilizce olması gerekmez." Teklif listesi içeriğinde kullanılandan başka bir dilde içerik sunmak için yararlı bir *bağlantı URL'si* sağlamak da kabul edilebilir.

### <a name="name"></a>Adı

Buraya girdiğiniz ad, teklif listenizin başlığı olarak müşterilere gösterilir. Bu alan, teklifi oluşturduğunuzda **Teklif takma adı** için girdiğiniz metinle önceden doldurulmuştur, ancak bu değeri değiştirebilirsiniz. Bu ad ticari marka olabilir (ve ticari marka veya telif hakkı sembolleri içerebilir). Ad 50 karakterden fazla olamaz ve herhangi bir emoji içeremez.

### <a name="search-results-summary"></a>Arama sonuçları özeti

Teklifinizin kısa bir açıklamasını (en fazla 100 karakter) sağlayın ve bu açıklamanızı pazar yeri arama sonuçlarında da kullanılabilir.

### <a name="long-summary"></a>Uzun özet

Teklifinizin daha uzun bir açıklamasını sağlayın (en fazla 256 karakter). Bu uzun özet, pazar yeri arama sonuçlarında da kullanılabilir.

### <a name="description"></a>Açıklama

Teklifinizin daha uzun bir açıklamasını sağlayın (en fazla 3.000 karakter). Bu açıklama, pazar yeri giriş özetinde müşterilere görüntülenir. Teklifinizin değer teklifini, temel avantajlarını, kategorive/veya endüstri derneklerini, uygulama içi satın alma fırsatlarını ve gerekli açıklamaları ekleyin.

Açıklamanızı yazmak için bazı ipuçları:  

- Teklifinizin değer teklifini açıklamanızın ilk birkaç cümlesinde açıkça açıklayın. Değer teklifinize aşağıdaki öğeleri ekleyin:
  - Teklifin açıklaması
  - Tekliften yararlanan kullanıcı türü
  - Müşteri ihtiyaçları veya teklif adresleri ağrı
- İlk birkaç cümlenin arama motoru sonuçlarında görüntülenebileceğini unutmayın.  
- Ürününüzü satmak için özelliklere ve işlevselliğe güvenmeyin. Bunun yerine, sağladığınız değere odaklanın.  
- Sektöre özgü kelime dağarcığı veya fayda tabanlı ifadeler mümkün olduğunca kullanın.

Teklif açıklamanızı daha ilgi çekici hale getirmek için, açıklamanızı biçimlendirmek için zengin metin düzenleyicisini kullanın.

![Zengin metin düzenleyicisini kullanma](./media/text-editor2.png)

Zengin metin düzenleyicisini kullanmak için aşağıdaki yönergeleri kullanın:

- İçeriğinizin biçimini değiştirmek için, biçimlendirmek istediğiniz metni vurgulayın ve aşağıda gösterildiği gibi bir metin stili seçin:

     ![Metin biçimini değiştirmek için zengin metin düzenleyicisini kullanma](./media/text-editor3.png)

- Metne madde işaretli veya numaralanmış bir liste eklemek için aşağıdaki seçenekleri kullanın:

     ![Liste eklemek için zengin metin düzenleyicisini kullanma](./media/text-editor4.png)

- Metne girintiyi eklemek veya kaldırmak için aşağıdaki seçenekleri kullanın:

     ![Girinti için zengin metin düzenleyicisini kullanma](./media/text-editor5.png)

### <a name="privacy-policy-link"></a>Gizlilik politikası bağlantısı

Kuruluşunuzun gizlilik politikasına (sitenizde barındırılan) URL'yi girin. Uygulamanızın gizlilik yasalarına ve yönetmeliklerine uygun olmasını sağlamaktan ve geçerli bir gizlilik politikası sağlamaktan siz sorumlusunuz.

### <a name="useful-links"></a>Yararlı bağlantılar

Çözümünüz le ilgili isteğe bağlı tamamlayıcı çevrimiçi belgeler sağlayın. + **Link ekle'ye**tıklayarak ek yararlı bağlantılar ekleyin.

### <a name="contact-information"></a>İletişim Bilgileri

Bu bölümde, bir **Destek kişi** ve mühendislik **kişi**için ad, e-posta ve telefon numarası sağlamanız gerekir. Bu bilgiler müşterilere gösterilmez, ancak Microsoft tarafından kullanılabilir ve CSP iş ortaklarına sağlanabilir.

### <a name="support-urls"></a>Destek URL'leri

**Azure Global Müşterileri** ve/veya Azure **Kamu müşterileri**için destek web sitelerine sahipseniz, bu URL'leri buradan sağlayın.

### <a name="marketplace-images"></a>Pazar yeri görüntüleri

Bu bölümde, teklifinizi müşteriye gösterirken kullanılacak logolar ve resimler sağlayabilirsiniz. Tüm görüntüler .png formatında olmalıdır.

#### <a name="marketplace-logos"></a>Pazar yeri logoları

Dört logo boyutları gereklidir: **Küçük (40x40)**, **Orta (90x90)**, **Büyük (115x115)** ve **Geniş (255x115)**. Logolarınız için aşağıdaki yönergeleri izleyin:

- Azure tasarımının basit bir renk paleti vardır. Logonuzdaki birincil ve ikincil renklerinin sayısını sınırlandırın.
- Portalın tema renkleri siyah ve beyazdır. Bu renkleri logonuzun arka plan rengi olarak kullanmayın. Logonuzun portalda öne çıkmasını sağlayan bir renk kullanın. Basit birincil renkleri öneririz.
- Saydam bir arka plan kullanıyorsanız logo ve metnin beyaz, siyah ve mavi olmadığından emin olun.
- Logonuzun genel görünümü düz olmalı ve gradyanlardan kaçınmalıdır. Logoda gradyan arka plan kullanmayın.
- Şirket veya marka adınız dahil olmak üzere logoya metin yerleştirmeyin.
- Logonuzun esnetilmediğinden emin olun.

#### <a name="screenshots"></a>Ekran görüntüleri

Teklifinizin nasıl çalıştığını gösteren en fazla beş ekran görüntüsü ekleyin. Tüm ekran görüntüleri 1280 x 720 piksel olmalıdır.

#### <a name="videos"></a>Videolar

İsteğe bağlı olarak, teklifinizi gösteren en fazla beş video ekleyebilirsiniz. Bu videolar YouTube ve/veya Vimeo'da barındırılmalıdır. Her biri için videonun adını, URL'sini ve videonun küçük resmini (1280 x 720 piksel) girin.

#### <a name="additional-marketplace-listing-resources"></a>Ek pazar yeri listeleme kaynakları

- [Pazar teklifi listeleri için en iyi uygulamalar](https://docs.microsoft.com/azure/marketplace/gtm-offer-listing-best-practices)

## <a name="preview"></a>Önizleme

Teklifinizi daha geniş bir pazar yeri teklifine canlı olarak yayınlamadan önce, önce teklifi sınırlı bir önizleme kitlesitarafından kullanılabilir hale getirmeniz gerekir. Bu, müşterilerinizin kullanımına sunmadan önce Azure Marketi'nde nasıl göründüğünü onaylamanızı sağlar. Microsoft destek ve mühendislik ekipleri de bu önizleme döneminde teklifinizi görüntüleyebilir.

**Önizleme Hedef Kitlesi** bölümüne Azure abonelik dislerini girerek önizleme hedef kitlesini tanımlayabilirsiniz. 10'a kadar abonelik tonu'nu el ile girebilir veya 100'e kadar abonelik tonu içeren bir .csv dosyası yükleyebilirsiniz.

Bu aboneliklerle ilişkili tüm müşteriler, teklif in yayına girmeden önce Azure Marketi'nde görüntüleyebilir. Teklifinizi önizlemek için kendi aboneliklerinizi buraya eklediğinizden emin olun.

## <a name="plan-overview"></a>Plana genel bakış

Her teklifin bir veya daha fazla planı olmalıdır (bazen SUS olarak da adlandırılır). Farklı özellik kümelerini farklı fiyatlarla desteklemek veya belirli bir müşteri kitlesi için belirli bir planı özelleştirmek için birden çok plan ekleyebilirsiniz. Müşteriler, ana teklif altında kendileri için kullanılabilen planları görüntüleyebilir.

**Plan'a genel bakış** sayfasında , **+ Yeni plan oluştur'u**seçin. Ardından bir **Plan Kimliği** ve Plan **adı**girin. Bu değerlerin her ikisi de en fazla 50 karakteriçeren küçük alfasayısal karakterler, tireler ve alt çizgi ler içerebilir. Bu değerler müşteriler tarafından görülebilir ve teklifi yayımladıktan sonra değiştirilemez.

Planınız üzerinde çalışmaya devam etmek için bu değerleri girdikten sonra **Oluştur'u** seçin. Tamamlanması gereken üç bölüm vardır: **Plan listesi,** **Fiyatlandırma ve kullanılabilirlik**ve **Teknik yapılandırma.**

### <a name="plan-listing"></a>Plan listesi

İlk olarak, plan için bir **Arama sonuçları özeti** sağlayın. Bu, pazar yeri arama sonuçlarında kullanılabilecek planınızın (en fazla 100 karakter) kısa bir açıklamasıdır.

Ardından, planın daha ayrıntılı bir açıklamasını sağlayan bir **Açıklama** girin.

### <a name="pricing-and-availability"></a>Fiyatlandırma ve kullanılabilirlik

Şu anda Yönetilen Hizmet teklifi için kullanılabilecek tek bir fiyatlandırma modeli vardır: **Kendi lisansınızı getirin (BYOL)**. Bu, müşterilerinize bu teklifle ilgili maliyetler için doğrudan fatura keseceğiniz anlamına gelir ve Microsoft size herhangi bir ücret talep etmez.

**Plan görünürlüğü** bölümü, bu planın [özel](../../marketplace/private-offers.md)olup olmadığını belirtmenizi sağlar. Bu özel **plan** kutusunu işaretlenmemiş olarak bırakırsanız, planınız belirli müşterilerle (veya belirli sayıda müşteriyle) sınırlı olmayacaktır.

Bu planı yalnızca belirli müşteriler için kullanılabilir hale getirmek için **Evet'i**seçin. Bunu yaptığınızda, abonelik kimliklerini sağlayarak müşterileri tanımlamanız gerekir. Bunlar tek tek (en fazla 10 abonelik için) veya .csv dosyası yükleyerek (tüm planlarda en fazla 10.000 abonelik için) girilebilir. Teklifi test etmek ve doğrulamak için kendi aboneliklerinizi buraya eklediğinizden emin olun.

> [!IMPORTANT]
> Bir plan genel olarak yayımlandıktan sonra, bunu özel olarak değiştiremezsiniz. Hangi müşterilerin teklifinizi kabul edip kaynakları temsilci olarak verebileceğini denetlemek için özel bir plan kullanın. Genel bir planla, belirli müşterilerle veya hatta belirli sayıda müşteriyle kullanılabilirliği kısıtlayamazsınız (ancak bunu yapmayı seçerseniz planı tamamen satmayı durdurabilirsiniz). Bir müşteri teklifi kabul ettikten sonra, teklifi yayınladığınızda [Yönetilen Hizmetler Kaydı Atama Silme Rolüne](../../role-based-access-control/built-in-roles.md#managed-services-registration-assignment-delete-role) Rol **Tanımı** kümesine bir **Yetkilendirme** dahil ettiğinizde bir [temsilciye erişimi kaldırabilirsiniz.](../../lighthouse/how-to/onboard-customer.md#remove-access-to-a-delegation) Ayrıca müşteriye ulaşabilir ve [erişiminizi kaldırmalarını isteyebilirsiniz.](../../lighthouse/how-to/view-manage-service-providers.md#add-or-remove-service-provider-offers)

### <a name="technical-configuration"></a>Teknik yapılandırma

Planınızın bu bölümü, müşteri kaynaklarını yönetmek için yetkilendirme bilgilerini içeren bir bildirim oluşturur. Bu bilgiler, [Azure temsilci kaynak yönetimini](../../lighthouse/concepts/azure-delegated-resource-management.md)etkinleştirmek için gereklidir.

Hangi rollerin destekleneceğini ve yetkilendirmelerinizi tanımlamak için en iyi uygulamaları anlamak için [Azure Deniz Feneri senaryolarında Kiracıları, rolleri ve kullanıcıları](../../lighthouse/concepts/tenants-users-roles.md#best-practices-for-defining-users-and-roles) gözden geçirin.

> [!NOTE]
> Yukarıda belirtildiği gibi, **Yetkilendirme** girişlerinizdeki kullanıcılar ve roller, planı satın alan her müşteri için geçerli olacaktır. Belirli bir müşteriye erişimi sınırlamak istiyorsanız, bunların özel kullanımı için özel bir plan yayımlamanız gerekir.

#### <a name="manifest"></a>Bildirim

İlk olarak, bildirim için bir **Sürüm** sağlayın. *n.n.n* biçimini kullanın (örneğin, 1.2.5).

Ardından, **Kiracı Kimliğinizi**girin. Bu, kuruluşunuzun Azure Etkin Dizini (Azure AD) kiracı kimliğiyle ilişkili bir GUID'dir; diğer bir şekilde, müşterilerinizin kaynaklarına erişeceğiniz yönetici kiracı. Bu kullanışlınız yoksa, Azure portalının sağ üst tarafında hesap adınızın üzerinde gezinerek veya **Switch dizinini**seçerek bulabilirsiniz.

Teklifinizin yeni bir sürümünü yayımladıysanız ve güncelleştirilmiş bir manifesto oluşturmanız gerekiyorsa, **+ Yeni manifesto'u**seçin. Önceki bildirim sürümünden sürüm numarasını artırdığından emin olun.

#### <a name="authorization"></a>Yetkilendirme

Yetkilendirmeler, planı satın alan müşteriler için kaynaklara ve aboneliklere erişebilen yönetici kiracınızdaki varlıkları tanımlar. Bu varlıkların her birine, belirli erişim düzeyleri veren yerleşik bir rol atanır.

Her plan için en fazla yirmi yetkilendirme oluşturabilirsiniz.

> [!TIP]
> Çoğu durumda, bir dizi tek tek kullanıcı hesabı yerine bir Azure AD kullanıcı grubuna veya hizmet sorumlusuna roller atamak istersiniz. Bu, erişim gereksinimleriniz değiştiğinde planı güncelleştirmek ve yeniden yayımlamak zorunda kalmadan tek tek kullanıcılara erişim eklemenize veya kaldırmanıza olanak tanır. Azure REKLAM gruplarına roller atarken, [Grup **türünün** **Office 365** **değil, Güvenlik** olduğundan emin olun.](../../active-directory/fundamentals/active-directory-groups-create-azure-portal.md) Ek öneriler için Azure [Deniz Feneri senaryolarında Kiracılar, roller ve kullanıcılara](../../lighthouse/concepts/tenants-users-roles.md)bakın.

Her **Yetkilendirme**için aşağıdakileri sağlamanız gerekir. Daha sonra **daha** fazla kullanıcı ve rol tanımı eklemek için gerektiğinde + Yetki Ekle'yi seçebilirsiniz.

- **Azure AD Nesne kimliği**: Bir kullanıcının, kullanıcı grubunun veya uygulamanın Azure AD tanımlayıcısı, müşterilerinizin kaynaklarına belirli izinler (Rol Tanımı ile tanımlandığı şekilde) verilecektir.
- **Azure AD NesneSi Görüntü Adı**: Müşterinin bu yetkilendirmenin amacını anlamasına yardımcı olacak kolay bir addır. Müşteri, kaynakları devre verirken bu adı görür.
- **Rol Tanımı**: Listeden kullanılabilir Azure AD yerleşik rollerinden birini seçin. Bu rol, **Azure AD Nesne kimliği** alanındaki kullanıcının müşterilerinizin kaynakları üzerinde sahip olacağı izinleri belirler. Bu rollerin açıklamaları için, Azure temsilci kaynak yönetimi için [Yerleşik roller](../../role-based-access-control/built-in-roles.md) ve [Rol desteği'ne](../../lighthouse/concepts/tenants-users-roles.md#role-support-for-azure-delegated-resource-management)bakın.
  > [!NOTE]
  > Geçerli yeni yerleşik roller Azure'a eklendikçe, bunlar görünmeden önce bazı gecikmeler olsa da burada kullanılabilir hale gelir.
- **Atanabilir Roller**: Bu seçenek, yalnızca bu yetkilendirme için **Rol Tanımı'nda** Kullanıcı Erişim Yöneticisi'ni seçtiyseniz görünür. Bu öyleyse, buraya bir veya daha fazla devratılabilir rol eklemeniz gerekir. Azure AD **Nesne kimliği** alanındaki kullanıcı, [düzeltilebilen ilkeleri dağıtmak](../../lighthouse/how-to/deploy-policy-remediation.md)için gereken yönetilen [kimliklere](../../active-directory/managed-identities-azure-resources/overview.md)bu rolleri atayabilecektir. Normalde Kullanıcı Erişim Yöneticisi rolüyle ilişkili başka hiçbir izinin bu kullanıcı için geçerli olmayacağını unutmayın.

> [!TIP]
> Gerekirse bir [temsilciliğe erişimi](../../lighthouse/how-to/onboard-customer.md#remove-access-to-a-delegation) kaldırabildiğinizden emin olmak için, [Yönetilen Hizmetler Kaydı Atama Silme Rolüne](../../role-based-access-control/built-in-roles.md#managed-services-registration-assignment-delete-role) **Rol Tanımı** kümesine sahip bir **Yetkilendirme** ekleyin. Bu rol atanmazsa, devredilen kaynaklar yalnızca müşterinin kiracısındaki bir kullanıcı tarafından kaldırılabilir.

Planınız için tüm bölümleri tamamladıktan sonra, + Ek planlar oluşturmak için gereken den fazla kez **yeni plan oluştur** seçeneğini belirleyebilirsiniz. İşiniz bittiğinde **Kaydet**'i seçin.

## <a name="publish"></a>Yayımlama

### <a name="submit-offer-to-preview"></a>Teklifönizlemeye gönderme

Teklifin gerekli tüm bölümlerini tamamladıktan **publish** sonra, portalın sağ üst köşesinde yayınla'yı seçin. **İnceleme ve yayımlama** sayfasına yönlendirileceksiniz.

Bu teklifi ilk kez yayınlıyorsanız, şunları yapabilirsiniz:

- Teklifin her bölümü için tamamlanma durumuna bakın.
  - *Başlatılmamamıştır* - bölüme dokunulmadığı ve tamamlanması gerektiği anlamına gelir.
  - *Eksik* - bölümün düzeltilmesi gereken veya daha fazla bilgi sağlanması nı gerektiren hataları olduğu anlamına gelir. Bölüme geri dön ve güncelleştir.
  - *Tam* - bölüm tamamlandı, gerekli tüm veriler sağlandı ve hiçbir hata yok demektir. Teklifi sunamadan önce teklifin tüm bölümlerinin tam bir durumda olması gerekir.
- Sertifika **notları** bölümünde, uygulamanızın anlaşılmasına yardımcı olan ek notlara ek olarak uygulamanızın doğru test edildiğinden emin olmak için sertifika ekibine test talimatları sağlayın.
- **Gönder'i**seçerek yayımlama için teklif gönderin. Teklifin önizleme sürümü gözden geçirip onaylamanız için kullanılabilir olduğunda size bir e-posta göndeririz. Ortak Merkezi'ne dönün ve teklifinizi halka (veya özel bir teklifte özel bir hedef kitleye) yayınlamak için teklif için **Go-live'ı** seçin.

### <a name="customer-experience-and-offer-management"></a>Müşteri deneyimi ve teklif yönetimi

Bir müşteri teklifinizi dağıttığında, [Azure temsilcikaynak yönetimi](../../lighthouse/concepts/azure-delegated-resource-management.md)için abonelikleri veya kaynak gruplarını devredebilecektir. Bu işlem hakkında daha fazla şey [için, müşteri onboarding işlemi](../../lighthouse/how-to/publish-managed-services-offers.md#the-customer-onboarding-process)bakın.

[Teklifinizin güncelleştirilmiş bir sürümünü](update-existing-offer.md) istediğiniz zaman yayımlayabilirsiniz. Örneğin, daha önce yayımlanmış bir teklife yeni bir rol tanımı eklemek isteyebilirsiniz. Bunu yaptığınızda, teklifi zaten eklemiş olan müşteriler Azure portalındaki [**Hizmet sağlayıcıları**](../../lighthouse/how-to/view-manage-service-providers.md) sayfasında bir güncelleştirmenin kullanılabilir olduğunu bildiren bir simge görür. Her müşteri değişiklikleri gözden geçirebilecek ve yeni sürüme güncelleştirmek isteyip istemediğine karar verebilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Ticari Market'te bulunan bir teklifi güncelleştirme](./update-existing-offer.md)
- [Azure Deniz Feneri hakkında bilgi edinin](../../lighthouse/overview.md)