---
title: Ticari Market 'te yeni bir yönetilen hizmet teklifi oluşturun
description: Iş Ortağı Merkezi 'nde ticari Market portalı 'nı kullanarak Azure Marketi 'nde listeleme için yeni bir yönetilen hizmet teklifi oluşturma.
author: JnHs
ms.author: jenhayes
manager: evansma
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/08/2020
ms.openlocfilehash: c82c3e0b6e6d11ccc33bf8556b06aeb04a6d37b5
ms.sourcegitcommit: f7fb9e7867798f46c80fe052b5ee73b9151b0e0b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2020
ms.locfileid: "82147921"
---
# <a name="create-a-new-managed-service-offer"></a>Yeni bir yönetilen hizmet teklifi oluşturun

> [!IMPORTANT]
> Yönetilen hizmet tekliflerinizin yönetimini Bulut İş Ortağı Portalı Iş ortağı merkezine taşıdık. Tekliflerinizi geçirene kadar Bulut İş Ortağı Portalı tekliflerinizi yönetmek için [Azure Market 'Te yönetilen hizmet sunma teklifi yayımlama](../../lighthouse/how-to/publish-managed-services-offers.md) bölümündeki yönergeleri izleyin.

Yönetilen hizmet, [Azure açık kullanım](../../lighthouse/overview.md) senaryolarını etkinleştirmek için yardım sağlar. Bir müşteri yönetilen bir hizmet teklifini kabul ettiğinde, [Azure tarafından atanan kaynak yönetimine](../../lighthouse/concepts/azure-delegated-resource-management.md)yönelik kaynakları ekleyebilir.

Yönetilen hizmet tekliflerini oluşturmaya başlamak için, ilk olarak [bir Iş Ortağı Merkezi hesabı](./create-account.md) oluşturduğunuzdan ve **genel bakış** sayfası seçili olarak [ticari Market panosunu](https://partner.microsoft.com/dashboard/commercial-marketplace/offers)seçtiğinizden emin olun. Yönetilen hizmet teklifi yayımlamak için [gümüş veya altın bir bulut platformu uzmanlığına](https://partner.microsoft.com/membership/cloud-platform-competency) sahip olmanız veya bir [Azure uzman msp](https://partner.microsoft.com/membership/azure-expert-msp) olmanız gerekir.

![Iş Ortağı Merkezi 'nde ticari Market panosu](./media/new-offer-overview.png)

>[!Note]
> Bir teklif yayımlandıktan sonra, Iş Ortağı Merkezi 'nde yapılan teklifle ilgili düzenlemeler yalnızca sistem sırasında ve yeniden yayımlamadan sonra mağazaların ön yüzlerinin güncelleştirilmesini sağlar. Lütfen değişiklikleri yaptıktan sonra teklifi yayın için gönderdiğinizden emin olun.

## <a name="create-a-new-offer"></a>Yeni teklif oluşturma

**+ Yeni teklif** düğmesini seçin ve ardından **yönetilen hizmet** menü öğesini seçin. **Yeni teklif** iletişim kutusu görüntülenir.

### <a name="offer-id-and-alias"></a>Teklif KIMLIĞI ve diğer ad

- **TEKLIF kimliği**: hesabınızdaki her teklif için benzersiz tanımlayıcı. Bu KIMLIK, Market teklifinin URL adresindeki müşterilere görünür olacaktır. Bu KIMLIK yalnızca küçük harfli alfasayısal karakterler (tireler ve alt çizgiler dahil ancak boşluk yok), 50 karakterle sınırlı olur ve **Oluştur**'u seçtikten sonra değiştirilemez.  Örneğin, burada *Test-teklif-1* girerseniz, teklif URL 'si olur `https://azuremarketplace.microsoft.com/marketplace/../test-offer-1`.

- **Teklif diğer adı**: Iş Ortağı Merkezi içindeki teklifine başvurmak için kullanılan ad. Bu ad Market 'te kullanılmayacak ve teklif adından ve müşterilere gösterilecek diğer değerlerden farklı. Bu değer, **Oluştur**' u seçtikten sonra değiştirilemez.

**TEKLIF kimliği** ve **teklif diğer adınızı**girdikten sonra **Oluştur**' u seçin. Daha sonra teklifinizin farklı bölümlerinin tümünde çalışabileceksiniz.

## <a name="offer-setup"></a>Teklif kurulumu

**Teklif kurulum** sayfası, aşağıdaki bilgileri ister. Bu alanları tamamladıktan sonra **Kaydet** ' i seçtiğinizden emin olun.

## <a name="connect-lead-management"></a>Müşteri adayı yönetimini bağlama

[!INCLUDE [Connect lead management](./includes/connect-lead-management.md)]

[Yönetilen hizmetler sertifika ilkelerine](https://docs.microsoft.com/legal/marketplace/certification-policies#700-managed-services)göre, bir **müşteri adayı hedefinin** gerekli olduğunu unutmayın. Bu, müşterinin teklifinizi dağıttığı her seferinde CRM sisteminizde bir kayıt oluşturur.

Daha fazla bilgi için bkz. [müşteri adayı yönetimine genel bakış](./commercial-marketplace-get-customer-leads.md).

Sonraki bölüme geçmeden önce sayfayı **kaydetmeyi** unutmayın.

## <a name="properties"></a>Özellikler

**Özellikler** sayfası, teklifinizi Market 'te gruplandırmak için kullanılan kategorileri ve teklifinizi destekleyen yasal sözleşmeleri tanımlamanızı sağlar. Bu sayfayı tamamladıktan sonra **Kaydet** ' i seçin.

### <a name="category"></a>Kategori

Teklifinizi uygun Market arama alanına yerleştirmek için kullanılacak en az bir ve en fazla beş kategori seçin. Teklifinizin teklif açıklamasında bu kategorileri nasıl desteklediğine dikkat edin.

### <a name="terms-and-conditions"></a>hüküm ve koşullar

**Hüküm ve koşullar** alanında kendi yasal hüküm ve koşullarınızı belirtin. Ayrıca, hüküm ve koşullarınızın bulunabileceği URL 'YI de sağlayabilirsiniz. Müşterilerinizin teklifinizi deneyebilmeleri için önce bu koşulları kabul etmesi gerekir.

## <a name="offer-listing"></a>Teklif listesi

**Teklif listesi** sayfası teklifiniz için Market ayrıntılarını (teklif adı, açıklama, görüntüler vb.) tanımlamanıza olanak sağlar.

> [!NOTE]
> Teklif açıklaması (örneğin, açıklama, belgeler, ekran görüntüleri, kullanım koşulları vb.) için teklif listesi içeriği, "Bu uygulama yalnızca [Ingilizce olmayan dil] içinde kullanılabilir" olarak, Ingilizce olması gerekmez. Ayrıca, teklif listeleme içeriğinde kullanılandan farklı bir dilde içerik sunmak için *yararlı bir bağlantı URL 'si* sağlamak da kabul edilebilir.

### <a name="name"></a>Adı

Buraya girdiğiniz ad, müşteriler tarafından teklif listelerinizin başlığı olarak gösterilir. Bu alan, teklifi oluştururken **teklif diğer adı** için girdiğiniz metin ile önceden doldurulur, ancak bu değeri değiştirebilirsiniz. Bu ad trademarked olabilir (ve ticari marka veya telif hakkı sembolleri dahil edebilirsiniz). Ad 50 karakterden uzun olamaz ve herhangi bir emojıs içeremez.

### <a name="search-results-summary"></a>Arama sonuçları Özeti

Sunabileceğiniz (en fazla 100 karakter) kısa bir açıklama sağlayın ve bu işlem Market arama sonuçlarında kullanılabilir.

### <a name="long-summary"></a>Uzun Özet

Teklifiniz için daha uzun bir açıklama sağlayın (en fazla 256 karakter). Bu uzun Özet, Market arama sonuçlarında da kullanılabilir.

### <a name="description"></a>Açıklama

Teklifiniz için daha uzun bir açıklama sağlayın (en fazla 3.000 karakter). Bu açıklama Market dökümüne Genel Bakış bölümündeki müşterilere görüntülenecektir. Teklifinizin değer teklifini, anahtar avantajlarını, kategori ve/veya sektör ilişkilendirmelerini, uygulama içi satın alma fırsatlarını ve gerekli tüm teklifleri dahil edin.

Tanımlarınızı yazmak için bazı ipuçları:  

- Tanımınızın ilk birkaç cümlede teklifinizin değer teklifini açık bir şekilde açıklamalıdır. Değer teklifinde aşağıdaki öğeleri ekleyin:
  - Teklifin açıklaması
  - Tekliften faydalanan Kullanıcı türü
  - Müşteri ihtiyaçları veya teklif adreslerine yönelik bir sorun
- İlk birkaç tümcenin arama motoru sonuçlarında görüntülenebileceğini aklınızda bulundurun.  
- Ürününüzü satmaya yönelik özelliklere ve işlevlere güvenmeyin. Bunun yerine, teslim ettiğiniz değere odaklanın.  
- Sektöre özgü sözlük veya avantaj temelli bir ifade mümkün olduğunca yararlanın.

Teklif açıklamanızı daha ilgi çekici hale getirmek için, tanımlarınızı biçimlendirmek için zengin metin düzenleyicisini kullanın.

![Zengin metin düzenleyicisini kullanma](./media/text-editor2.png)

Zengin metin düzenleyicisini kullanmak için aşağıdaki yönergeleri kullanın:

- İçeriğinizin biçimini değiştirmek için, biçimlendirmek istediğiniz metni vurgulayın ve aşağıda gösterildiği gibi bir metin stili seçin:

     ![Metin biçimini değiştirmek için zengin metin düzenleyicisini kullanma](./media/text-editor3.png)

- Metne bir madde işaretli veya numaralı liste eklemek için aşağıdaki seçenekleri kullanın:

     ![Liste eklemek için zengin metin düzenleyicisini kullanma](./media/text-editor4.png)

- Metne girintileme eklemek veya kaldırmak için aşağıdaki seçenekleri kullanın:

     ![Girintilemek için zengin metin düzenleyicisini kullanma](./media/text-editor5.png)

### <a name="privacy-policy-link"></a>Gizlilik ilkesi bağlantısı

Kuruluşunuzun gizlilik ilkesinin URL 'sini (sitenizde barındırılan) girin. Uygulamanızın gizlilik yasaları ve yönetmeliklerle uyumlu olmasını sağlamaktan ve geçerli bir gizlilik ilkesi sağlamaya yönelik siz sorumlusunuz.

### <a name="useful-links"></a>Yararlı bağlantılar

Çözümünüz hakkında isteğe bağlı ek çevrimiçi belgeler sağlayın. **+ Bağlantı ekle**' ye tıklayarak ek yararlı bağlantılar ekleyin.

### <a name="contact-information"></a>İletişim Bilgileri

Bu bölümde, bir **destek kişisi** ve **mühendislik ilgili kişisi**için ad, e-posta ve telefon numarası sağlamanız gerekir. Bu bilgiler müşterilere gösterilmez, ancak Microsoft tarafından kullanılabilir ve CSP iş ortakları için de sağlanıyor olabilir.

### <a name="support-urls"></a>Destek URL 'Leri

**Azure küresel müşterileri** ve/veya **Azure Kamu müşterileri**için destek web siteleri varsa, bu URL 'leri burada sağlayın.

### <a name="marketplace-images"></a>Market görüntüleri

Bu bölümde, müşteri teklifinizi gösterirken kullanılacak logo ve görüntüler sağlayabilirsiniz. Tüm görüntülerin. png biçiminde olması gerekir.

>[!Note]
>Dosya yükleme sorununuz varsa, yerel ağınızın Iş Ortağı Merkezi tarafından kullanılan https://upload.xboxlive.com hizmeti engellemediğinden emin olun.

#### <a name="marketplace-logos"></a>Market logoları

Dört logo boyutu gereklidir: **küçük (40x40)**, **Orta (90x90)**, **büyük (115x115)** ve **geniş (255x115)**. Logolarınız için şu yönergeleri izleyin:

- Azure tasarımının basit bir renk paleti vardır. Logonuzdaki birincil ve ikincil renklerinin sayısını sınırlandırın.
- Portalın tema renkleri siyah ve beyazdır. Bu renkleri logonuzun arka plan rengi olarak kullanmayın. Logonuzun portalda öne çıkmasını sağlayan bir renk kullanın. Basit birincil renkleri öneririz.
- Saydam bir arka plan kullanıyorsanız logo ve metnin beyaz, siyah ve mavi olmadığından emin olun.
- Logonuzun genel görünümü düz olmalı ve gradyanlardan kaçınmalıdır. Logoda gradyan arka plan kullanmayın.
- Şirket veya marka adınız dahil olmak üzere logoya metin yerleştirmeyin.
- Logonuzun esnetilmediğinden emin olun.

#### <a name="screenshots"></a>Ekran görüntüleri

Teklifinizin nasıl çalıştığını gösteren en fazla beş ekran görüntüsü ekleyin. Tüm ekran görüntüleri 1280 x 720 piksel olmalıdır.

#### <a name="videos"></a>Videolar

İsteğe bağlı olarak, teklifinizi gösteren en fazla beş video ekleyebilirsiniz. Bu videoların YouTube ve/veya Vimeo 'da barındırılması gerekir. Her biri için videonun adını, URL 'sini ve videonun küçük bir görüntüsünü (1280 x 720 piksel) girin.

#### <a name="additional-marketplace-listing-resources"></a>Ek Market listeleme kaynakları

- [Market teklif listeleri için en iyi uygulamalar](https://docs.microsoft.com/azure/marketplace/gtm-offer-listing-best-practices)

## <a name="preview"></a>Önizleme

Teklifinizi daha geniş Market teklifiyle yayımlamadan önce, önce onu sınırlı bir önizleme hedef kitlesi için kullanılabilir hale getirmeniz gerekir. Bu, müşterilerinizin müşterilere hazır hale gelmeden önce Azure Marketi 'nde nasıl göründüğünü doğrulamanıza olanak sağlar. Microsoft destek ve mühendislik ekipleri, bu önizleme döneminde teklifinizi da görüntüleyebilecektir.

Ön izleme **İzleyicisi** bölümüne Azure abonelik kimlikleri girerek önizleme kitleyi tanımlayabilirsiniz. En fazla 10 abonelik kimliğini el ile girebilir veya en fazla 100 abonelik kimliğiyle bir. csv dosyası yükleyebilirsiniz.

Bu aboneliklerle ilişkili müşteriler, teklifi canlı olmadan önce Azure Marketi 'nde görüntüleyebilecektir. Teklifinizin önizlemesini yapabilmeniz için kendi aboneliklerinizi buraya eklediğinizden emin olun.

## <a name="plan-overview"></a>Plana genel bakış

Her teklifin bir veya daha fazla planı olmalıdır (bazen SKU 'Lar olarak adlandırılır). Farklı fiyatlarla farklı özellik kümelerini desteklemek için veya belirli bir müşterinin sınırlı bir kitlesi için belirli bir planı özelleştirmek üzere birden çok plan ekleyebilirsiniz. Müşteriler, kendileri için kullanılabilir olan planları üst teklif altında görüntüleyebilir.

**Plana genel bakış** sayfasında **+ Yeni plan oluştur**' u seçin. Ardından bir **plan kimliği** ve bir **Plan adı**girin. Bu değerlerin her ikisi de en fazla 50 karakter içeren küçük harfli alfasayısal karakterler, tireler ve alt çizgiler içerebilir. Bu değerler müşterilere görünebilir ve teklifi yayımladıktan sonra değiştirilemez.

Planınızda çalışmaya devam etmek için bu değerleri girdikten sonra **Oluştur** ' u seçin. Tamamlanacak üç bölüm vardır: **plan listeleme**, **fiyatlandırma ve kullanılabilirlik**ve **Teknik yapılandırma**.

### <a name="plan-listing"></a>Planı listeleme

İlk olarak, plan için bir **Arama sonuçları Özeti** sağlayın. Bu, Market arama sonuçlarında kullanılabilen planınızın (en fazla 100 karakter) kısa bir açıklamasıdır.

Sonra, planın daha ayrıntılı bir açıklamasını sağlayan bir **Açıklama** girin.

### <a name="pricing-and-availability"></a>Fiyatlandırma ve kullanılabilirlik

Şu anda, yönetilen hizmet teklifi için kullanılabilecek yalnızca bir fiyatlandırma modeli vardır: **kendi lisansınızı getirin (KLG)**. Bu, müşterilerinize bu teklifle ilgili maliyetleri doğrudan faturalayeceğiniz ve Microsoft 'un size herhangi bir ücret ödemeyeceği anlamına gelir.

**Plan görünürlüğü** bölümü, bu planın [özel](../../marketplace/private-offers.md)olup olmadığını belirtmenize olanak tanır. **Bu özel bir plan** kutusunu işaretsiz bırakırsanız, planınız belirli müşteriler (veya belirli bir sayıda müşteri) ile sınırlandırılır.

Bu planı yalnızca belirli müşteriler için kullanılabilir hale getirmek için **Evet**' i seçin. Bunu yaptığınızda, abonelik kimliklerini sağlayarak müşterileri belirlemeniz gerekir. Bunlar tek bir (10 abonelik için) veya bir. csv dosyası karşıya yükleyerek (tüm planlar genelinde en fazla 10.000 abonelik için) girilebilir. Teklifi sınayabilmeniz ve doğrulayabilmeniz için kendi aboneliklerinizi buraya eklediğinizden emin olun.

> [!IMPORTANT]
> Bir plan ortak olarak yayımlandıktan sonra, özel olarak değiştiremezsiniz. Hangi müşterilerin teklif ve temsilci kaynaklarınızı kabul edeceğini denetlemek için özel bir plan kullanın. Ortak bir plan sayesinde, kullanılabilirliği belirli müşterilerle veya hatta belirli sayıda müşteriye kısıtlayamazsınız (ancak bunu yapmak isterseniz planı tamamen satmaya devam edebilirsiniz). Bir müşteri bir teklifi kabul ettikten sonra, teklifi yayımladığınızda rol **tanımıyla** [yönetilen hizmetler kayıt ataması silme rolü](../../role-based-access-control/built-in-roles.md#managed-services-registration-assignment-delete-role) olarak ayarlanmış bir **Yetkilendirme** eklediyseniz, bir [temsilciye erişimi kaldırabilirsiniz](../../lighthouse/how-to/remove-delegation.md) . Ayrıca müşteriye ulaşabilir ve [erişiminizi kaldırmasını](../../lighthouse/how-to/view-manage-service-providers.md#add-or-remove-service-provider-offers)isteyebilirsiniz.

### <a name="technical-configuration"></a>Teknik yapılandırma

Planınızın bu bölümü, müşteri kaynaklarını yönetmeye yönelik yetkilendirme bilgilerini içeren bir bildirim oluşturur. [Azure tarafından atanan kaynak yönetimini](../../lighthouse/concepts/azure-delegated-resource-management.md)etkinleştirmek için bu bilgiler gereklidir.

Hangi rollerin desteklendiğini ve yetkilendirmelerinizi tanımlamaya yönelik en iyi yöntemleri anlamak için [Azure Use senaryolarında kiracılar, roller ve kullanıcıları](../../lighthouse/concepts/tenants-users-roles.md#best-practices-for-defining-users-and-roles) gözden geçirdiğinizden emin olun.

> [!NOTE]
> Yukarıda belirtildiği gibi, **Yetkilendirme** girdinizdeki kullanıcılar ve roller planı satın alan her müşteri için de geçerlidir. Belirli bir müşteriye erişimi sınırlandırmak istiyorsanız, özel kullanım için özel bir plan yayımlamanız gerekir.

#### <a name="manifest"></a>Bildirim

İlk olarak, bildirim için bir **Sürüm** belirtin. *N. n. n* biçimini kullanın (örneğin, 1.2.5).

Ardından, **KIRACı kimliğinizi**girin. Bu, kuruluşunuzun Azure Active Directory (Azure AD) kiracı KIMLIĞIYLE ilişkili bir GUID 'dir; diğer bir deyişle, müşterilerinizin kaynaklarına erişecek olan kiracı yönetimi. Bu kullanışlı bir sahip değilseniz, Azure portal sağ üst tarafındaki hesap adınızın üzerine gelerek veya **Dizin Değiştir**' i seçerek bulabilirsiniz.

Teklifinizin yeni bir sürümünü yayımladığınızda ve güncelleştirilmiş bir bildirim oluşturmanız gerekiyorsa **+ Yeni bildirim**' ı seçin. Önceki bildirim sürümünden sürüm numarasını artırdığınızdan emin olun.

#### <a name="authorization"></a>Yetkilendirme

Yetkilendirmeler, yönetim kiracınızda, planı satın alan müşteriler için kaynaklara ve aboneliklere erişebilecek olan varlıkları tanımlar. Bu varlıkların her birine, belirli erişim düzeyleri veren bir yerleşik rol atanır.

Her plan için en fazla yirmi yetkilendirmeler oluşturabilirsiniz.

> [!TIP]
> Çoğu durumda, bir dizi bireysel kullanıcı hesabı yerine bir Azure AD kullanıcı grubuna veya hizmet sorumlusuna roller atamak isteyeceksiniz. Bu, erişim gereksinimleriniz değiştiğinde planı güncelleştirmek ve yeniden yayınlamak zorunda kalmadan bireysel kullanıcılar için erişim eklemenize veya kaldırmanıza olanak sağlar. Azure AD gruplarına rol atarken, [ **Grup türünün** **Office 365**değil **güvenlik** olduğundan emin olun](../../active-directory/fundamentals/active-directory-groups-create-azure-portal.md). Ek öneriler için bkz. [Azure 'Da kiracılar, roller ve kullanıcılar ile Ilgili kullanım senaryoları](../../lighthouse/concepts/tenants-users-roles.md).

Her **Yetkilendirme**için aşağıdakileri sağlamanız gerekir. Daha sonra, daha fazla Kullanıcı ve rol tanımı eklemek için gereken sayıda **Yetkilendirme** seçeneğini belirleyin.

- **Azure AD nesne kimliği**: bir kullanıcının, Kullanıcı grubunun veya UYGULAMANıN Azure AD tanımlayıcısı (rol tanımı tarafından tanımlandığı gibi) müşterilerinizin kaynaklarına verilecek.
- **Azure AD nesne görünen adı**: müşterinin bu yetkilendirmenin amacını anlamasına yardımcı olacak kolay bir ad. Müşteri, kaynakları yetkilendirirken bu adı görür.
- **Rol tanımı**: listeden mevcut Azure AD yerleşik rollerinden birini seçin. Bu rol, **Azure AD nesne kimliği** alanındaki kullanıcının kaynaklarınızın kaynaklarına sahip olacağı izinleri tespit eder. Bu rollerin açıklamaları için bkz. [Azure tarafından yetkilendirilen kaynak yönetimi Için](../../lighthouse/concepts/tenants-users-roles.md#role-support-for-azure-delegated-resource-management) [yerleşik roller](../../role-based-access-control/built-in-roles.md) ve rol desteği.
  > [!NOTE]
  > Uygun yeni yerleşik roller Azure 'a eklendikçe, bunlar görüntülenmeden önce biraz gecikme olabileceğinden burada kullanılabilir hale gelir.
- **Atanabilir roller**: Bu seçenek yalnızca, bu yetkilendirme Için **rol tanımında** Kullanıcı erişimi Yöneticisi ' ni seçtiyseniz görünür. Bu durumda, buraya bir veya daha fazla atanabilir rol eklemeniz gerekir. **Azure AD nesne kimliği** alanındaki Kullanıcı, bu rolleri, düzeltilebilecek [ilkeleri dağıtmak](../../lighthouse/how-to/deploy-policy-remediation.md)için gereken [yönetilen kimliklere](../../active-directory/managed-identities-azure-resources/overview.md)atayabilecektir. Normalde Kullanıcı erişimi Yöneticisi rolüyle ilişkili başka hiçbir izin bu kullanıcı için uygulanmayacak şekilde unutmayın.

> [!TIP]
> Gerekirse, [bir temsilciye erişimi kaldırabilmeniz](../../lighthouse/how-to/remove-delegation.md) Için, [yönetilen hizmetler kayıt ataması silme rolü](../../role-based-access-control/built-in-roles.md#managed-services-registration-assignment-delete-role)' ne ayarlanmış **rol tanımıyla** bir **Yetkilendirme** ekleyin. Bu rol atanmamışsa, atanan kaynaklar yalnızca müşterinin kiracısındaki bir kullanıcı tarafından kaldırılabilir.

Planınız için tüm bölümleri tamamladıktan sonra, ek planlar oluşturmanız gerektiği kadar **+ Yeni plan oluştur** ' u seçebilirsiniz. İşiniz bittiğinde **Kaydet**'i seçin.

## <a name="publish"></a>Yayımlama

### <a name="submit-offer-to-preview"></a>Önizlemeye teklif Gönder

Teklifin tüm gerekli bölümlerini tamamladıktan sonra portalın sağ üst köşesinde **Yayımla** ' yı seçin. **İnceleme ve yayımlama** sayfasına yönlendirilirsiniz.

Bu teklifi ilk kez yayımladıysanız şunları yapabilirsiniz:

- Teklifin her bölümü için tamamlanma durumuna bakın.
  - *Başlatılmamış* -bölüm dokunulmamış ve tamamlanması gereken anlamına gelir.
  - *Tamamlanmamış* -bölümde düzeltilmesi gereken hatalar olduğu veya daha fazla bilgi sağlanması gerekir. Bölüm (ler) e geri dönün ve güncelleştirin.
  - *Tam* -bölümün tamamlandığı, tüm gerekli verilerin sağlandığı ve hata olmadığı anlamına gelir. Teklifi gönderebilmeniz için teklifin tüm bölümlerinin tamamen bir durumda olması gerekir.
- **Sertifika notları** bölümünde, uygulamanızı anlamak için yararlı olan tüm ek notlara ek olarak, uygulamanızın doğru şekilde test edilmesini sağlamak için sertifika ekibine test yönergeleri sağlayın.
- **Gönder**' i seçerek teklifi yayımlamaya gönderebilirsiniz. Teklifin önizleme sürümü gözden geçirmeniz ve onaylamanız için kullanılabilir olduğunda size bir e-posta göndereceğiz. Iş Ortağı Merkezi 'ne dönün ve teklifinizi herkese açık bir şekilde (veya özel bir teklif olarak özel hedef kitleye) yayımlama teklifi için **Go-Live** ' ı seçin.

### <a name="customer-experience-and-offer-management"></a>Müşteri deneyimi ve teklif yönetimi

Müşteri teklifinizi dağıttığında, [Azure tarafından atanan kaynak yönetimi](../../lighthouse/concepts/azure-delegated-resource-management.md)için abonelikleri veya kaynak gruplarını devredebilir. Bu süreç hakkında daha fazla bilgi için bkz. [Müşteri ekleme işlemi](../../lighthouse/how-to/publish-managed-services-offers.md#the-customer-onboarding-process).

[Teklifinizin güncelleştirilmiş bir sürümünü](update-existing-offer.md) dilediğiniz zaman yayımlayabilirsiniz. Örneğin, daha önce yayımlanmış bir teklifine yeni bir rol tanımı eklemek isteyebilirsiniz. Bunu yaptığınızda, teklifi zaten eklemiş olan müşteriler, bir güncelleştirmenin kullanılabildiğini bilmesini sağlayan Azure portal [**hizmet sağlayıcıları**](../../lighthouse/how-to/view-manage-service-providers.md) sayfasında bir simge görür. Her müşteri, değişiklikleri gözden geçirebilecek ve yeni sürüme güncelleştirmek isteyip istemediğinize karar verecek.

## <a name="next-steps"></a>Sonraki adımlar

- [Ticari Market'te bulunan bir teklifi güncelleştirme](./update-existing-offer.md)
- [Azure ışıklı kullanımı hakkında bilgi edinin](../../lighthouse/overview.md)