---
title: Microsoft ticari Market 'te yönetilen hizmet teklifi oluşturma
description: Iş Ortağı Merkezi 'nde ticari Market portalı 'nı kullanarak Azure Marketi 'nde listeleme için yeni bir yönetilen hizmet teklifi oluşturma.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
author: JnHs
ms.author: jenhayes
ms.date: 07/07/2020
ms.openlocfilehash: 70f0dbff2530b11e92bd0b0cc55ad92a8679ebe9
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86121801"
---
# <a name="create-a-managed-service-offer"></a>Yönetilen Hizmet teklifi oluşturma

Yönetilen hizmet, [Azure açık kullanım](../../lighthouse/overview.md) senaryolarını etkinleştirmek için yardım sağlar. Bir müşteri yönetilen bir hizmet teklifini kabul ettiğinde, [Azure tarafından atanan kaynak yönetimine](../../lighthouse/concepts/azure-delegated-resource-management.md)yönelik kaynakları ekleyebilir. Başlamadan önce, henüz yapmadıysanız [Iş Ortağı Merkezi 'nde bir ticari Market hesabı oluşturun](create-account.md) . Hesabınızın ticari Market programına kayıtlı olduğundan emin olun.

Yönetilen hizmet teklifi yayımlamak için [gümüş veya altın bir bulut platformu uzmanlığına](https://partner.microsoft.com/membership/cloud-platform-competency) sahip olmanız veya bir [Azure uzman msp](https://partner.microsoft.com/membership/azure-expert-msp) olmanız gerekir.

## <a name="create-a-new-offer"></a>Yeni teklif oluşturma

1. [Iş Ortağı Merkezi](https://partner.microsoft.com/dashboard/home)' nde oturum açın.
2. Sol gezinti menüsünde **ticari Market**  >  **genel bakış**' ı seçin.
3. Genel Bakış sayfasında **+ yeni teklif**  >  **yönetilen hizmet**' i seçin.

    ![Sol gezinti menüsünü gösterir.](./media/new-offer-managed-service.png)

>[!NOTE]
>Bir teklif yayımlandıktan sonra, Iş Ortağı Merkezi 'nde yapılan düzenlemeler, teklif yeniden yayımlandıktan sonra yalnızca stoporonts 'de görüntülenir. Değişiklik yaptıktan sonra her zaman yeniden yayımlaytığınızdan emin olun.

## <a name="new-offer"></a>Yeni teklif

Bir **TEKLIF kimliği**girin. Bu, hesabınızdaki her teklif için benzersiz bir tanımlayıcıdır.

* Bu KIMLIK, varsa Market teklifi ve Azure Resource Manager şablonları için Web adresinde müşteriler tarafından görülebilir.
* Yalnızca küçük harfleri ve rakamları kullanın. Kısa çizgi ve alt çizgi içerebilir, ancak boşluk içeremez ve 50 karakterle sınırlıdır. Örneğin, **Test-teklif-1**girerseniz, teklif Web adresi olacaktır `https://azuremarketplace.microsoft.com/marketplace/../test-offer-1` .
* Teklif KIMLIĞI, **Oluştur**' u seçtikten sonra değiştirilemez.

Bir **teklif diğer adı**girin. Bu, Iş Ortağı Merkezi 'nde teklif için kullanılan addır.

* Bu ad Market 'te kullanılmıyor ve teklif adından ve müşterilere gösterilen diğer değerlerden farklı.
* **Oluştur**' u seçtikten sonra, teklif diğer adı değiştirilemez.

Teklifi oluşturmak için **Oluştur** ' u seçin ve devam edin.

## <a name="offer-setup"></a>Teklif kurulumu

### <a name="customer-leads"></a>Müşteri liderleri

[!INCLUDE [Connect lead management](./includes/connect-lead-management.md)]

[Yönetilen hizmetler sertifika ilkelerine](https://docs.microsoft.com/legal/marketplace/certification-policies#700-managed-services)göre, bir **müşteri adayı hedefi** gereklidir. Bu, müşterinin teklifinizi dağıttığı her seferinde CRM sisteminizde bir kayıt oluşturur.

Daha fazla bilgi için bkz. [müşteri adayı yönetimine genel bakış](./commercial-marketplace-get-customer-leads.md).

Devam etmeden önce **Taslağı kaydet** ' i seçin.

## <a name="properties"></a>Özellikler

Bu sayfa, teklifinizi Market 'te gruplandırmak için kullanılan kategorileri ve teklifinizi destekleyen yasal sözleşmeleri tanımlamanızı sağlar.

### <a name="category"></a>Kategori

Teklifinizi uygun Market arama alanına yerleştirmek için kullanılacak en az bir ve en fazla beş kategori seçin. Teklifinizin teklif açıklamasında bu kategorileri nasıl desteklediğine dikkat edin.

### <a name="terms-and-conditions"></a>hüküm ve koşullar

**Hüküm ve koşullar** alanında kendi yasal hüküm ve koşullarınızı belirtin. Ayrıca, hüküm ve koşullarınızın bulunabileceği URL 'YI de sağlayabilirsiniz. Müşterilerinizin teklifinizi deneyebilmeleri için önce bu koşulları kabul etmesi gerekir.

Devam etmeden önce **Taslağı kaydet** ' i seçin.

## <a name="offer-listing"></a>Teklif listesi

Bu sayfa teklifiniz için Market ayrıntılarını (teklif adı, açıklama ve görüntüler gibi) tanımlamanıza olanak sağlar.

> [!NOTE]
> Teklif açıklaması (Açıklama, belgeler, ekran görüntüleri ve kullanım koşulları gibi) sunan teklif listesinin Ingilizce olması gerekmez, "Bu uygulama yalnızca [Ingilizce olmayan dilde] kullanılabilir". Ayrıca, teklif listeleme içeriğinde kullanılandan farklı bir dilde içerik sunmak için *yararlı bir bağlantı URL 'si* sağlamak da kabul edilebilir.

Teklif bilgilerinin Azure portal nasıl göründüğünü gösteren bir örnek aşağıda verilmiştir:

:::image type="content" source="media/example-managed-services.png" alt-text="Bu teklifin Azure portal nasıl göründüğünü gösterir.":::

#### <a name="call-out-descriptions"></a>Çağrı açıklamaları

1. Başlık
2. Açıklama
3. Yararlı bağlantılar
4. Ekran görüntüleri

### <a name="name"></a>Name

Buraya girdiğiniz ad, müşteriler tarafından teklif listelerinizin başlığı olarak gösterilir. Bu alan, teklifi oluştururken **teklif diğer adı** için girdiğiniz metinle önceden doldurulur, ancak bu değeri değiştirebilirsiniz. Bu ad trademarked olabilir (ve ticari marka veya telif hakkı sembolleri dahil edebilirsiniz). Ad 50 karakterden uzun olamaz ve herhangi bir emojıs içeremez.

### <a name="search-results-summary"></a>Arama sonuçları Özeti

Sunabileceğiniz (en fazla 100 karakter) kısa bir açıklama sağlayın ve bu işlem Market arama sonuçlarında kullanılabilir.

### <a name="long-summary"></a>Uzun Özet

Teklifiniz için daha uzun bir açıklama sağlayın (en fazla 256 karakter). Bu uzun Özet, Market arama sonuçlarında da kullanılabilir.

### <a name="description"></a>Description

[!INCLUDE [Long description-1](./includes/long-description-1.md)]

[!INCLUDE [Long description-2](./includes/long-description-2.md)]

[!INCLUDE [Long description-3](./includes/long-description-3.md)]

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

>[!NOTE]
>Dosya yükleme sorununuz varsa, yerel ağınızın https://upload.xboxlive.com Iş Ortağı Merkezi tarafından kullanılan hizmeti engellemediğinden emin olun.

#### <a name="marketplace-logos"></a>Market logoları

Teklifinizin logosunu dört piksel boyutunda sağlayın:

- **Küçük** (48 x 48)
- **Orta** (90 x 90)
- **Büyük** (216 x 216)
- **Geniş** (255 x 115)

Logolarınız için şu yönergeleri izleyin:

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

- [Market teklif listeleri için en iyi uygulamalar](../gtm-offer-listing-best-practices.md)

Devam etmeden önce **Taslağı kaydet** ' i seçin.

## <a name="preview"></a>Önizleme

Teklifinizi daha geniş Market teklifiyle yayımlamadan önce, önce onu sınırlı bir önizleme hedef kitlesi için kullanılabilir hale getirmeniz gerekir. Bu, müşterilerinizin müşterilere hazır hale gelmeden önce Azure Marketi 'nde nasıl göründüğünü doğrulamanıza olanak sağlar. Microsoft destek ve mühendislik ekipleri, bu önizleme döneminde teklifinizi da görüntüleyebilecektir.

Ön izleme **İzleyicisi** bölümüne Azure abonelik kimlikleri girerek önizleme kitleyi tanımlayabilirsiniz. En fazla 10 abonelik kimliğini el ile girebilir veya en fazla 100 abonelik kimliğiyle bir. csv dosyası yükleyebilirsiniz.

Bu aboneliklerle ilişkili müşteriler, teklifi canlı olmadan önce Azure Marketi 'nde görüntüleyebilecektir. Teklifinizin önizlemesini yapabilmeniz için kendi aboneliklerinizi buraya eklediğinizden emin olun.

Devam etmeden önce **Taslağı kaydet** ' i seçin.

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
- **Rol tanımı**: listeden mevcut Azure AD yerleşik rollerinden birini seçin. Bu rol, **Azure AD nesne kimliği** alanındaki kullanıcının kaynaklarınızın kaynaklarına sahip olacağı izinleri tespit eder. Bu rollerin açıklamaları için bkz. [Azure Use Için](../../lighthouse/concepts/tenants-users-roles.md#role-support-for-azure-lighthouse) [yerleşik roller](../../role-based-access-control/built-in-roles.md) ve rol desteği.
  > [!NOTE]
  > Uygun yeni yerleşik roller Azure 'a eklendikçe, bunlar görüntülenmeden önce biraz gecikme olabileceğinden burada kullanılabilir hale gelir.
- **Atanabilir roller**: Bu seçenek yalnızca, bu yetkilendirme Için **rol tanımında** Kullanıcı erişimi Yöneticisi ' ni seçtiyseniz görünür. Bu durumda, buraya bir veya daha fazla atanabilir rol eklemeniz gerekir. **Azure AD nesne kimliği** alanındaki Kullanıcı, bu rolleri, düzeltilebilecek [ilkeleri dağıtmak](../../lighthouse/how-to/deploy-policy-remediation.md)için gereken [yönetilen kimliklere](../../active-directory/managed-identities-azure-resources/overview.md)atayabilecektir. Normalde Kullanıcı erişimi Yöneticisi rolüyle ilişkili başka hiçbir izin bu kullanıcı için uygulanmayacak şekilde unutmayın.

> [!TIP]
> Gerekirse, [bir temsilciye erişimi kaldırabilmeniz](../../lighthouse/how-to/remove-delegation.md) Için, [yönetilen hizmetler kayıt ataması silme rolü](../../role-based-access-control/built-in-roles.md#managed-services-registration-assignment-delete-role)' ne ayarlanmış **rol tanımıyla** bir **Yetkilendirme** ekleyin. Bu rol atanmamışsa, atanan kaynaklar yalnızca müşterinin kiracısındaki bir kullanıcı tarafından kaldırılabilir.

Planınız için tüm bölümleri tamamladıktan sonra, ek planlar oluşturmanız gerektiği kadar **+ Yeni plan oluştur** ' u seçebilirsiniz. İşiniz bittiğinde **Kaydet**'i seçin.

Devam etmeden önce **Taslağı kaydet** ' i seçin.

## <a name="publish"></a>Yayımlama

### <a name="submit-offer-to-preview"></a>Önizlemeye teklif Gönder

Teklifin tüm gerekli bölümlerini tamamladıktan sonra portalın sağ üst köşesinde bulunan **gözden geçir ve Yayımla** ' yı seçin.

Bu teklifi ilk kez yayımladıysanız şunları yapabilirsiniz:

- Teklifin her bölümü için tamamlanma durumuna bakın.
  - **Başlatılmamış** -bölüm dokunulmamış ve tamamlanması gerekiyor.
  - **Tamamlanmamış** -bölümde düzeltilmesi gereken hatalar var veya daha fazla bilgi sağlanması gerekiyor. Bölüm (ler) e geri dönün ve güncelleştirin.
  - **Tamamlandı** -bölüm tamamlandı, tüm gerekli veriler sağlanmış ve hata yok. Teklifi gönderebilmeniz için teklifin tüm bölümlerinin tamamen bir durumda olması gerekir.
- **Sertifika notları** bölümünde, uygulamanızı anlamak için yararlı olan tüm ek notlara ek olarak, uygulamanızın doğru şekilde test edilmesini sağlamak için sertifika ekibine test yönergeleri sağlayın.
- **Gönder**' i seçerek teklifi yayımlamaya gönderebilirsiniz. Teklifin önizleme sürümü gözden geçirmeniz ve onaylamanız için kullanılabilir olduğunda size bir e-posta göndereceğiz. Iş Ortağı Merkezi 'ne dönün ve teklifinizi herkese açık bir şekilde (veya özel bir teklif olarak özel hedef kitleye) yayımlama teklifi için **Go-Live** ' ı seçin.

### <a name="customer-experience-and-offer-management"></a>Müşteri deneyimi ve teklif yönetimi

Müşteri teklifinizi dağıttığında, [Azure tarafından atanan kaynak yönetimi](../../lighthouse/concepts/azure-delegated-resource-management.md)için abonelikleri veya kaynak gruplarını devredebilir. Bu süreç hakkında daha fazla bilgi için bkz. [Müşteri ekleme işlemi](../../lighthouse/how-to/publish-managed-services-offers.md#the-customer-onboarding-process).

[Teklifinizin güncelleştirilmiş bir sürümünü](update-existing-offer.md) dilediğiniz zaman yayımlayabilirsiniz. Örneğin, daha önce yayımlanmış bir teklifine yeni bir rol tanımı eklemek isteyebilirsiniz. Bunu yaptığınızda, teklifi zaten eklemiş olan müşteriler, bir güncelleştirmenin kullanılabildiğini bilmesini sağlayan Azure portal [**hizmet sağlayıcıları**](../../lighthouse/how-to/view-manage-service-providers.md) sayfasında bir simge görür. Her müşteri, değişiklikleri gözden geçirebilecek ve yeni sürüme güncelleştirmek isteyip istemediğinize karar verecek.

## <a name="next-steps"></a>Sonraki adımlar

- [Ticari Market'te bulunan bir teklifi güncelleştirme](./update-existing-offer.md)
- [Azure ışıklı kullanımı hakkında bilgi edinin](../../lighthouse/overview.md)
