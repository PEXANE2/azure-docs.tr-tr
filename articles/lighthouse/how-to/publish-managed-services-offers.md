---
title: Azure Marketi'nde Yönetilen Hizmet teklifi yayınlama
description: Müşterileri Azure temsilcikaynak yönetimine dahil eden yönetilen bir Hizmet teklifini nasıl yayınlayacağınızı öğrenin.
ms.date: 04/08/2020
ms.topic: conceptual
ms.openlocfilehash: 247f711188fa10de19cece27f164fdfa71612d1b
ms.sourcegitcommit: a53fe6e9e4a4c153e9ac1a93e9335f8cf762c604
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2020
ms.locfileid: "80991918"
---
# <a name="publish-a-managed-service-offer-to-azure-marketplace"></a>Azure Marketi'nde Yönetilen Hizmet teklifi yayınlama

> [!IMPORTANT]
> 13 Nisan 2020'den itibaren Yönetilen Hizmet tekliflerinizi İş Ortağı Merkezi'ne taşımaya başlayacağız. Geçişten sonra, Tekliflerinizi İş Ortağı Merkezi'nde oluşturur ve yönetirsiniz. Geçirilen tekliflerinizi yönetmek için [yeni bir Yönetilen Hizmet teklifi oluştur'daki](../../marketplace/partner-center-portal/create-new-managed-service-offer.md) yönergeleri izleyin.

Bu makalede, [Bulut İş Ortağı Portalı'nı](https://cloudpartner.azure.com/)kullanarak Azure [Marketi'nde](https://azuremarketplace.microsoft.com) genel veya özel Yönetilen Hizmet teklifini nasıl yayınlayacağınızı öğreneceksiniz. Teklifi satın alan müşteriler, [Azure temsilci kaynak yönetimi](../concepts/azure-delegated-resource-management.md)için yerleşik aboneliklere ve kaynak gruplarına binebilir.

## <a name="publishing-requirements"></a>Yayımlama gereksinimleri

Teklifler oluşturmak ve yayınlamak için [İş Merkezi'nde](../../marketplace/partner-center-portal/create-account.md) geçerli bir hesabınız olması gerekir. Zaten bir hesabınız yoksa, kayıt [işlemi,](https://aka.ms/joinmarketplace) İş Ortağı Merkezi'nde bir hesap oluşturma ve Ticari Pazar programına kaydolma adımlarında size yol açar.

Yönetilen [Hizmet teklif lerine göre,](https://docs.microsoft.com/legal/marketplace/certification-policies#7004-business-requirements)Yönetilen Hizmet teklifini yayınlamak için [Gümüş veya Altın Bulut Platformu uzmanlık düzeyine](https://docs.microsoft.com/partner-center/learn-about-competencies) sahip olmalısınız veya Azure Uzmanı [MSP](https://partner.microsoft.com/membership/azure-expert-msp) olmalısınız.

Microsoft İş Ortağı Ağı (MPN) Kimliğiniz, müşteri etkileşimleri üzerindeki etkinizi izlemek için yayımladığınız tekliflerle [otomatik olarak ilişkilendirilir.](../../billing/billing-partner-admin-link-started.md)

> [!NOTE]
> Azure Marketi'nde bir teklif yayınlamak istemiyorsanız, Azure Kaynak Yöneticisi şablonlarını kullanarak müşterilere el ile binebilirsiniz. Daha fazla bilgi için, [Azure temsilcili kaynak yönetimine](onboard-customer.md)giden bir müşteriye bakın.

Yönetilen Hizmet teklifini yayımlamak, Azure Marketi'nde başka türde bir teklif yayınlamaya benzer. Genel yayımlama süreci hakkında bilgi edinmek için [Azure Marketi ve AppSource Yayımlama Kılavuzu'na](../../marketplace/marketplace-publishers-guide.md)bakın. Ayrıca [ticari pazar sertifikasyonu ilkelerini](https://docs.microsoft.com/legal/marketplace/certification-policies)( özellikle [Yönetilen Hizmetler](https://docs.microsoft.com/legal/marketplace/certification-policies#700-managed-services) bölümünü) gözden geçirmelisiniz.

Bir müşteri teklifinizi ekledikten sonra, bir veya daha fazla abonelik veya kaynak grubuna temsilci atayabilir ve bu grup azure [temsilci kaynak yönetimi için geçerli](#the-customer-onboarding-process)olacaktır.

> [!IMPORTANT]
> Yönetilen hizmet teklifindeki her plan, kiracınızdaki Azure Etkin Dizin (Azure AD) varlıklarını tanımladığınız ve bu planı satın alan müşteriler için temsilci kaynak gruplarına ve/veya aboneliklere erişebilecek bir **Bildirim Ayrıntıları** bölümünü içerir. Eklediğiniz herhangi bir grubun (veya kullanıcı veya hizmet sorumlusunun) planı satın alan her müşteri için aynı izinlere sahip olacağını unutmayın. Her müşteriyle çalışmak üzere farklı gruplar atamak için, her müşteriye özel ayrı bir [özel plan](../../marketplace/private-offers.md) yayımlamanız gerekir.

## <a name="create-your-offer"></a>Teklifinizi oluşturun

1. Bulut İş [Ortağı Portalı'nda](https://cloudpartner.azure.com/)oturum açın.
2. Soldaki gezinti menüsünden **Yeni teklifi**seçin, ardından Yönetilen **hizmetleri**seçin.
3. Teklifiniz için dört bölümden bir **Editör** bölümü görürsünüz: **Teklif Ayarları,** **Planlar,** **Pazar Yeri**ve **Destek.** Bu bölümlerin nasıl tamamlanabildiğini öğrenmek için okumaya devam edin.

### <a name="enter-offer-settings"></a>Teklif ayarlarını girin

Teklif **ayarları** bölümünde aşağıdakileri sağlayın:

|Alan  |Açıklama  |
|---------|---------|
|**Teklif Kimliği**     | Teklifiniz için benzersiz bir tanımlayıcı (yayıncı profiliniz içinde). Bu kimlik, en fazla 50 karakteriçeren küçük alfasayısal karakterler, tireler ve alt çizgi ler içerebilir. Teklif Kimliğinin ürün URL'leri ve faturalama raporları gibi yerlerde müşteriler tarafından görülebileceğini unutmayın. Teklifi yayımladıktan sonra bu değeri değiştiremezsiniz.        |
|**Yayıncı Kimliği**     | Teklifle ilişkilendirilecek yayımcı kimliği. Birden fazla yayımcı kimliğiniz varsa, bu teklif için kullanmak istediğinizi seçebilirsiniz.       |
|**Adı**     | Müşterilerin Azure Marketi'nde ve Azure portalında teklifiniz için görecekleri ad (en fazla 50 karakter). Bu teklifi kendi web siteniz aracılığıyla tanıtıyorsanız, burada da aynı adı kullandığınızdan emin olun.        |

Bitirdikten sonra **Kaydet'i**seçin. Artık **Planlar** bölümüne geçmeye hazırsınız.

### <a name="create-plans"></a>Plan oluşturma

Her teklifin bir veya daha fazla planı olmalıdır (bazen SUS olarak da adlandırılır). Farklı özellik kümelerini farklı fiyatlarla desteklemek veya belirli bir müşteri kitlesi için belirli bir planı özelleştirmek için birden çok plan ekleyebilirsiniz. Müşteriler, ana teklif altında kendileri için kullanılabilen planları görüntüleyebilir.

Planlar bölümünde Yeni **Plan'ı**seçin. Sonra bir **Plan Kimliği**girin. Bu kimlik, en fazla 50 karakteriçeren küçük alfasayısal karakterler, tireler ve alt çizgi ler içerebilir. Plan kimliği, ürün URL'leri ve faturalama raporları gibi yerlerde müşteriler tarafından görülebilir. Teklifi yayımladıktan sonra bu değeri değiştiremezsiniz.

#### <a name="plan-details"></a>Plan ayrıntıları

**Plan Ayrıntıları** bölümünde aşağıdaki bölümleri tamamlayın:

|Alan  |Açıklama  |
|---------|---------|
|**Başlık**     | Görüntülemek için plan için dostu adı. Maksimum uzunluğu 50 karakter.        |
|**Özet**     | Başlık altında görüntülemek için planın kısa açıklaması. Maksimum uzunluğu 100 karakter.        |
|**Açıklama**     | Planın daha ayrıntılı bir açıklamasını sağlayan açıklama metni.         |
|**Faturalama modeli**     | Burada gösterilen 2 faturalandırma modeli vardır, ancak yönetilen hizmetler teklifleri için **kendi lisansınızı getir'i** seçmeniz gerekir. Bu, müşterilerinize bu teklifle ilgili maliyetler için doğrudan fatura keseceğiniz anlamına gelir ve Microsoft size herhangi bir ücret talep etmez.   |
|**Bu özel bir plan mı?**     | SKU'nun özel mi yoksa herkese açık mı olduğunu gösterir. Varsayılan değer **Hayır** (ortak). Bu seçimden ayrılırsanız, planınız belirli müşterilerle (veya belirli sayıda müşteriyle) sınırlı olmayacaktır; bir ortak plan yayımladıktan sonra, daha sonra özel olarak değiştiremezsiniz. Bu planı yalnızca belirli müşteriler için kullanılabilir hale getirmek için **Evet'i**seçin. Bunu yaptığınızda, abonelik kimliklerini sağlayarak müşterileri tanımlamanız gerekir. Bunlar tek tek (en fazla 10 abonelik için) veya .csv dosyası yükleyerek (20.000'e kadar abonelik için) girilebilir. Teklifi test etmek ve doğrulamak için kendi aboneliklerinizi buraya eklediğinizden emin olun. Daha fazla bilgi için [Özel SK'ler ve Planlar'a](../../marketplace/cloud-partner-portal-orig/cloud-partner-portal-azure-private-skus.md)bakın.  |

> [!IMPORTANT]
> Bir plan genel olarak yayımlandıktan sonra, bunu özel olarak değiştiremezsiniz. Hangi müşterilerin teklifinizi kabul edip kaynakları temsilci olarak verebileceğini denetlemek için özel bir plan kullanın. Genel bir planla, belirli müşterilerle veya hatta belirli sayıda müşteriyle kullanılabilirliği kısıtlayamazsınız (ancak bunu yapmayı seçerseniz planı tamamen satmayı durdurabilirsiniz). Bir müşteri teklifi kabul ettikten sonra, teklifi yayınladığınızda [Yönetilen Hizmetler Kaydı Atama Silme Rolüne](../../role-based-access-control/built-in-roles.md#managed-services-registration-assignment-delete-role) Rol **Tanımı** kümesine bir **Yetkilendirme** dahil ettiğinizde bir [temsilciye erişimi kaldırabilirsiniz.](onboard-customer.md#remove-access-to-a-delegation) Ayrıca müşteriye ulaşabilir ve [erişiminizi kaldırmalarını isteyebilirsiniz.](view-manage-service-providers.md#add-or-remove-service-provider-offers)

#### <a name="manifest-details"></a>Bildirim ayrıntıları

Planınız için **Bildirim Ayrıntıları** bölümünü doldurun. Bu, müşteri kaynaklarını yönetmek için yetkilendirme bilgilerini içeren bir bildirim oluşturur. Bu bilgiler, Azure temsilci kaynak yönetimini etkinleştirmek için gereklidir.

> [!NOTE]
> Yukarıda belirtildiği gibi, **Yetkilendirme** girişlerinizdeki kullanıcılar ve roller, planı satın alan her müşteri için geçerli olacaktır. Belirli bir müşteriye erişimi sınırlamak istiyorsanız, bunların özel kullanımı için özel bir plan yayımlamanız gerekir.

İlk olarak, bildirim için bir **Sürüm** sağlayın. *n.n.n* biçimini kullanın (örneğin, 1.2.5).

Ardından, **Kiracı Kimliğinizi**girin. Bu, kuruluşunuzun Azure Etkin Dizin kiracı kimliğiyle (örneğin, müşterilerinizin kaynaklarını yönetmek için çalışacağınız kiracı) ilişkili bir GUID'dir. Bu kullanışlınız yoksa, Azure portalının sağ üst tarafında hesap adınızın üzerinde gezinerek veya **Switch dizinini**seçerek bulabilirsiniz.

Son olarak, planınıza bir veya daha fazla **Yetkilendirme** girişi ekleyin. Yetkilendirmeler, planı satın alan müşteriler için kaynaklara ve aboneliklere erişebilen varlıkları tanımlar ve belirli erişim düzeyleri veren roller atar.

> [!TIP]
> Çoğu durumda, bir dizi tek tek kullanıcı hesabı yerine bir Azure AD kullanıcı grubuna veya hizmet sorumlusuna izin atamak istersiniz. Bu, erişim gereksinimleriniz değiştiğinde planı güncelleştirmek ve yeniden yayımlamak zorunda kalmadan tek tek kullanıcılara erişim eklemenize veya kaldırmanıza olanak tanır. Ek öneriler için Azure [Deniz Feneri senaryolarında Kiracılar, roller ve kullanıcılara](../concepts/tenants-users-roles.md)bakın.

Her **Yetkilendirme**için aşağıdakileri sağlamanız gerekir. Daha sonra, daha fazla kullanıcı ve rol tanımı eklemek için gerektiğinde yeni **yetkilendirme** yi seçebilirsiniz.

- **Azure AD Nesne kimliği**: Bir kullanıcının, kullanıcı grubunun veya uygulamanın Azure AD tanımlayıcısı, müşterilerinizin kaynaklarına belirli izinler (Rol Tanımı'nda açıklandığı şekilde) verilecektir.
- **Azure AD NesneSi Görüntü Adı**: Müşterinin bu yetkilendirmenin amacını anlamasına yardımcı olacak kolay bir addır. Müşteri, kaynakları devre verirken bu adı görür.
- **Rol Tanımı**: Listeden kullanılabilir Azure AD yerleşik rollerinden birini seçin. Bu rol, **Azure AD Nesne kimliği** alanındaki kullanıcının müşterilerinizin kaynakları üzerinde sahip olacağı izinleri belirler. Bu rollerin açıklamaları için, Azure temsilci kaynak yönetimi için [Yerleşik roller](../../role-based-access-control/built-in-roles.md) ve [Rol desteği'ne](../concepts/tenants-users-roles.md#role-support-for-azure-delegated-resource-management)bakın.
  > [!NOTE]
  > Geçerli yeni yerleşik roller Azure'a eklendikçe, bunlar görünmeden önce bazı gecikmeler olsa da burada kullanılabilir hale gelir.
- **Atanabilir Roller**: Bu, yalnızca bu yetkilendirme için **Rol Tanımı'nda** Kullanıcı Erişim Yöneticisi'ni seçtiyseniz gereklidir. Bu öyleyse, buraya bir veya daha fazla devratılabilir rol eklemeniz gerekir. Azure AD **Nesne kimliği** alanındaki kullanıcı, [düzeltilebilen ilkeleri dağıtmak](deploy-policy-remediation.md)için gereken yönetilen [kimliklere](../../active-directory/managed-identities-azure-resources/overview.md)bu **Atabilen Rolleri** atayabilecektir. Normalde Kullanıcı Erişim Yöneticisi rolüyle ilişkili başka hiçbir izinin bu kullanıcı için geçerli olmayacağını unutmayın. Burada bir veya daha fazla rol seçmezseniz, gönderiniz sertifikayı geçemez. (Bu kullanıcının Rol Tanımı için Kullanıcı Erişim Yöneticisi'ni seçmediyseniz, bu alanın hiçbir etkisi yoktur.)

> [!TIP]
> Gerekirse bir [temsilciliğe erişimi](onboard-customer.md#remove-access-to-a-delegation) kaldırabildiğinizden emin olmak için, [Yönetilen Hizmetler Kaydı Atama Silme Rolüne](../../role-based-access-control/built-in-roles.md#managed-services-registration-assignment-delete-role) **Rol Tanımı** kümesine sahip bir **Yetkilendirme** ekleyin. Bu rol atanmazsa, devredilen kaynaklar yalnızca müşterinin kiracısındaki bir kullanıcı tarafından kaldırılabilir.

Bilgileri tamamladıktan sonra, ek planlar oluşturmak için gereken den fazla kez **Yeni plan'ı** seçebilirsiniz. İşi bittiğinde **Kaydet'i**seçin ve ardından **Market** bölümüne devam edin.

### <a name="provide-marketplace-text-and-images"></a>Pazar yeri metin ve resimlerini sağlama

**Market** bölümü, müşterilerin Azure Marketi'nde ve Azure portalında göreceği metin ve resimleri sağladığınız bölümdür.

**Genel Bakış** bölümünde aşağıdaki alanları tamamlayın:

|Alan  |Açıklama  |
|---------|---------|
|**Başlık**     |  Teklifin başlığı, genellikle uzun, resmi adı. Bu başlık pazarda belirgin bir şekilde görüntülenir. Maksimum uzunluğu 50 karakter. Çoğu durumda, **bu, Teklif Ayarları** bölümüne girdiğiniz **Ad** ile aynı olmalıdır.       |
|**Özet**     | Teklifinizin kısa amacı veya işlevi. Bu genellikle başlık altında görüntülenir. Maksimum uzunluğu 100 karakter.        |
|**Uzun Özet**     | Teklifinizin amacının veya işlevinin daha uzun bir özeti. Maksimum uzunluğu 256 karakter.        |
|**Açıklama**     | Teklifiniz hakkında daha fazla bilgi. Bu alan en fazla 3000 karakter uzunluğundadır ve basit HTML biçimlendirmesini destekler. Açıklamanızın bir yerinde "yönetilen hizmet" veya "yönetilen hizmetler" sözcüklerini eklemeniz gerekir.       |
|**Pazarlama Tanımlayıcısı**     | Benzersiz bir URL dostu tanımlayıcı. Bu tanımlayıcı yalnızca küçük alfasayısal karakterler ve tireler içerebilir. Bu teklif için Market URL'lerinde kullanılacaktır. Örneğin, yayıncı kimliğiniz *contoso* ise ve pazarlama tanımlayıcınız *sampleApp*ise, Azure Market'teki *https://azuremarketplace.microsoft.com/marketplace/apps/contoso-sampleApp*teklifinizin URL'si .        |
|**Abonelik IYT'lerini Önizleme**     | 100 abonelik tanımlayıcısına bir tane ekleyin. Bu aboneliklerle ilişkili müşteriler, teklif in yayına girmeden önce Azure Marketi'nde görüntüleyebilir. Müşterilerinizin kullanımına sunmadan önce teklifinizin Azure Marketi'nde nasıl görünebileceğini önizlemeniz için kendi aboneliklerinizi buraya dahil etmenizi öneririz.  (Microsoft destek ve mühendislik ekipleri de bu önizleme döneminde teklifinizi görüntüleyebilir.)   |
|**Faydalı Linkler**     | Belgeleriniz, sürüm notları, SSS'ler gibi teklifiniz ile ilgili URL'ler.        |
|**Önerilen Kategoriler (Max 5)**     | Teklifiniz için geçerli olan bir veya daha fazla kategori (en fazla beş). Bu kategoriler, müşterilerin Azure Marketi ve Azure portalındaki teklifinizi keşfetmenize yardımcı olur.        |

Pazarlama **Eserleri** bölümünde, teklifinizle birlikte gösterilecek logolar ve diğer varlıkları yükleyebilirsiniz. İsteğe bağlı olarak, müşterilerin teklifinizi anlamasına yardımcı olabilecek videolara ekran görüntüleri veya bağlantılar yükleyebilirsiniz.

Dört logo boyutları gereklidir: **Küçük (40x40)**, **Orta (90x90)**, **Büyük (115x115)** ve **Geniş (255x115)**. Logolarınız için aşağıdaki yönergeleri izleyin:

- Azure tasarımının basit bir renk paleti vardır. Logonuzdaki birincil ve ikincil renklerinin sayısını sınırlandırın.
- Portalın tema renkleri siyah ve beyazdır. Bu renkleri logonuzun arka plan rengi olarak kullanmayın. Logonuzun portalda öne çıkmasını sağlayan bir renk kullanın. Basit birincil renkleri öneririz.
- Saydam bir arka plan kullanıyorsanız logo ve metnin beyaz, siyah ve mavi olmadığından emin olun.
- Logonuzun genel görünümü düz olmalı ve gradyanlardan kaçınmalıdır. Logoda gradyan arka plan kullanmayın.
- Şirket veya marka adınız dahil olmak üzere logoya metin yerleştirmeyin.
- Logonuzun esnetilmediğinden emin olun.

**Hero (815x290)** logosu isteğe bağlıdır ancak önerilir. Bir kahraman logosu eklerseniz, aşağıdaki yönergeleri izleyin:

- Kahraman logosuna herhangi bir metin eklemeyin ve logonun sağ tarafında 415 piksel boş alan bıraktığından emin olun. Bu, programlı olarak katıştırılacak metin öğelerine yer bırakmak için gereklidir: yayıncınızın görüntü adı, plan başlığı, teklif uzun özet.
- Kahraman logonuzun arka planı siyah, beyaz veya saydam olmayabilir. Katıştırılmış metin beyaz renkte görüntüleneceği için arka plan renginizin çok açık olmadığından emin olun.
- Teklifinizi bir kahraman simgesiyle yayımladıktan sonra, teklifi kaldıramazsınız (ancak istediğiniz de farklı bir sürümle güncelleştirebilirsiniz).

Müşteri **Adayı Yönetimi** bölümünde, müşteri adaylarınızın depolanacağı CRM sistemini seçebilirsiniz. Yönetilen Hizmetler [sertifika ilkelerine](https://docs.microsoft.com/legal/marketplace/certification-policies#700-managed-services)göre bir **Müşteri Adayı Hedefi** gerektiğini unutmayın.

Son olarak, **Gizlilik Politikası URL'nizi** ve **Kullanım Koşullarınızı** **Yasal** bölümde sağlayın. Bu teklif için [Standart Sözleşme'yi](../../marketplace/standard-contract.md) kullanıp kullanmayacağını da burada belirtebilirsiniz.

**Destek** bölümüne geçmeden önce değişikliklerinizi kaydettiğinizden emin olun.

### <a name="add-support-info"></a>Destek bilgileri ekleme

**Destek** bölümünde, bir mühendislik kişisi ve müşteri destek kişisi için ad, e-posta ve telefon numarası sağlayın. Ayrıca destek URL'leri sağlamanız gerekir. Microsoft, iş ve destek sorunları hakkında sizinle iletişim kurmamız gerektiğinde bu bilgileri kullanabilir.

Bu bilgileri ekledikten sonra **Kaydet'i seçin.**

## <a name="publish-your-offer"></a>Teklifinizi yayınlayın

Tüm bölümleri tamamladıktan sonra, bir sonraki adımınız teklifi Azure Marketi'nde yayınlamaktır. Teklifinizi canlı yapma işlemini başlatmak için **Yayımla** düğmesini seçin. Bu işlem hakkında daha fazla şey için Azure [Marketi ve AppSource tekliflerini yayımla'ya](../../marketplace/cloud-partner-portal/manage-offers/cpp-publish-offer.md)bakın.

[Teklifinizin güncelleştirilmiş bir sürümünü](../../marketplace/cloud-partner-portal/manage-offers/cpp-update-offer.md) istediğiniz zaman yayımlayabilirsiniz. Örneğin, daha önce yayımlanmış bir teklife yeni bir rol tanımı eklemek isteyebilirsiniz. Bunu yaptığınızda, teklifi zaten eklemiş olan müşteriler Azure portalındaki [**Hizmet sağlayıcıları**](view-manage-service-providers.md) sayfasında bir güncelleştirmenin kullanılabilir olduğunu bildiren bir simge görür. Her müşteri değişiklikleri [gözden geçirebilecek](view-manage-service-providers.md#update-service-provider-offers) ve yeni sürüme güncelleştirmek isteyip istemediğine karar verebilir. 

## <a name="the-customer-onboarding-process"></a>Müşteri onboarding işlemi

Bir müşteri teklifinizi ekledikten sonra, [bir veya daha fazla özel abonelik veya kaynak grubuna (azure](view-manage-service-providers.md#delegate-resources)temsilcikaynak yönetimi için dahil edilecek) temsilci olarak devredilebilir. Bir müşteri bir teklifi kabul etmiş ancak henüz herhangi bir kaynak temsilcisi görevlendirmediyse, Azure portalında [**Hizmet sağlayıcıları**](view-manage-service-providers.md) sayfasının Sağlayıcı **teklifleri** bölümünün üst kısmında bir not görür.

> [!IMPORTANT]
> Temsilci, müşterinin kiracısında, yerleşik abonelik için [Sahibinin yerleşik rolüne](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#owner) sahip olan (veya gemide bulunan kaynak gruplarını içeren) konuk olmayan bir hesap tarafından yapılmalıdır. Aboneliği devratabilecek tüm kullanıcıları görmek için, müşterinin kiracısındaki bir kullanıcı Azure portalındaki aboneliği seçebilir, **Access denetimini (IAM)** açabilir ve [Sahibi rolü olan tüm kullanıcıları görüntüleyebilir.](../../role-based-access-control/role-assignments-list-portal.md#list-owners-of-a-subscription)

Müşteri bir aboneliği (veya bir abonelik içindeki bir veya daha fazla kaynak grubuna) temsilci olarak devrettikten sonra, **Microsoft.ManagedServices** kaynak sağlayıcısı bu abonelik için kaydedilir ve kiracınızdaki kullanıcılar teklifinizdeki yetkilendirmelere göre devredilen kaynaklara erişebilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Ticari Pazar Hakkında](../../marketplace/partner-center-portal/commercial-marketplace-overview.md)Bilgi Edinin.
- Kiracılar [arası yönetim deneyimleri](../concepts/cross-tenant-management-experience.md)hakkında bilgi edinin.
- Azure portalındaki **müşterilerime** giderek [müşterileri görüntüleyin ve yönetin.](view-manage-customers.md)
