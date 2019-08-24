---
title: Azure aboneliğinin faturalandırma sahipliğini başka bir hesaba aktarma | Microsoft Docs
description: Bir Azure aboneliğinin fatura sahipliğinin başka bir hesaba nasıl aktarılacağını ve işlemle ilgili bazı sık sorulan sorular (SSS) açıklanmaktadır
keywords: Azure aboneliğini aktarma, Azure aktarım aboneliği, Azure aboneliğini başka bir hesaba taşıma, Azure, abonelik sahibini değiştirme, Azure aboneliğini başka bir hesaba aktarma, Azure aktarım faturalandırma
author: bandersmsft
manager: amberb
tags: billing,top-support-issue
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/01/2019
ms.author: banders
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 1e4496e2b5d2b21fd878ef68665b8e5b06fa6cc5
ms.sourcegitcommit: dcf3e03ef228fcbdaf0c83ae1ec2ba996a4b1892
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/23/2019
ms.locfileid: "70012528"
---
# <a name="transfer-billing-ownership-of-an-azure-subscription-to-another-account"></a>Bir Azure aboneliğinin faturalandırma sahipliğini başka bir hesaba aktarma

Kuruluşunuzu terk ederseniz veya aboneliğinizin başka bir hesaba faturalanmasını istiyorsanız, Azure aboneliğinizin faturalandırma sahipliğini aktarmak isteyebilirsiniz. Faturalandırma sahipliğini başka bir hesaba aktarmak, yeni hesap iznindeki yöneticileri, ödeme yöntemini değiştir, ücretleri görüntüle ve aboneliği iptal etme gibi faturalandırma görevlerini gerçekleştirmek için sağlar.

Faturalandırma sahipliğini korumak, ancak aboneliğinizin türünü değiştirmek istiyorsanız bkz. [Azure aboneliğinizi başka bir](billing-how-to-switch-azure-offer.md)teklifle değiştirme. Abonelikteki kaynakları kimlerin yönetebileceğini denetlemek isterseniz, bkz. [Azure kaynakları Için yerleşik roller](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles).

Kurumsal Anlaşma (EA) müşterisiyseniz kurumsal yöneticileriniz, aboneliklerinizin faturalama sahipliğini hesaplar arasında aktarabilir. Daha fazla bilgi için bkz. [Kurumsal Anlaşma (EA) aboneliklerinin faturalandırma sahipliğini aktarma](#transfer-billing-ownership-of-enterprise-agreement-ea-subscriptions).

## <a name="transfer-billing-ownership-of-an-azure-subscription"></a>Azure aboneliğinin faturalama sahipliğini aktarma

1. [Azure Portal](https://portal.azure.com) , aktarmak istediğiniz aboneliği olan faturalandırma hesabının Yöneticisi olarak oturum açın. Yönetici olup olmadığını öğrenmek için bkz. [sık sorulan sorular](#faq).

1. Arama **maliyet Yönetimi + faturalandırma**.

   ![Azure portalı arama gösteren ekran görüntüsü](./media/billing-subscription-transfer/billing-search-cost-management-billing.png)

1. Sol bölmeden **abonelikler** ' i seçin. Erişimlerinize bağlı olarak, bir faturalandırma kapsamı seçip **abonelikler** veya **Azure abonelikleri**' ni seçmeniz gerekebilir.

1. Aktarmak istediğiniz abonelik için **Fatura sahipliğini aktar** ' ı seçin. 

   ![Aktarılacak aboneliği seçin](./media/billing-subscription-transfer/billing-select-subscription-to-transfer.png)

1. Aboneliğin yeni sahibi olacak hesabın faturalama yöneticisi olan bir kullanıcının e-posta adresini girin.

1. Aboneliğinizi başka bir Azure AD kiracısındaki bir hesaba aktarıyorsanız, aboneliği yeni hesabın kiracısına taşımak istiyorsanız seçin. Daha fazla bilgi için bkz. [başka bir Azure AD kiracısındaki bir hesaba abonelik aktarma](#transferring-subscription-to-an-account-in-another-azure-ad-tenant)

    > [!IMPORTANT]
    >
    > Aboneliği yeni hesabın Azure AD kiracısına taşımayı seçerseniz, abonelikteki kaynakları yönetmek için tüm [rol tabanlı erişim denetimi (RBAC)](../role-based-access-control/overview.md) atamaları kalıcı olarak kaldırılır. Yalnızca Aktarım isteğinizi kabul eden yeni hesaptaki Kullanıcı, abonelikteki kaynakları yönetmek için erişime sahip olur. Daha fazla bilgi için bkz. [aboneliği başka bir Azure AD kiracısında bir kullanıcıya aktarma](../active-directory/managed-identities-azure-resources/known-issues.md). Alternatif olarak, aboneliği yeni hesabın kiracısına taşımadan faturalandırma sahipliğini aktarmak için Azure AD kiracısının abonelik kutusunun işaretini kaldırabilirsiniz. Bunu yaparsanız, Azure kaynaklarını yönetmek için mevcut RBAC izinleri korunur.
  
    ![Aktarım sayfası Gönder](./media/billing-subscription-transfer/billing-send-transfer-request.PNG)

1. **Aktarım Isteği gönder**' i seçin.

1. Kullanıcı, aktarım isteğinizi gözden geçirmek için yönergeler içeren bir e-posta alır.

   ![Alıcıya gönderilen abonelik aktarım e-postası](./media/billing-subscription-transfer/billing-receiver-email.png)

1. Aktarım isteğini onaylamak için kullanıcı e-postadaki bağlantıyı seçer ve yönergeleri izler. Kullanıcının abonelik için ödeme yapmak için kullanılacak bir ödeme yöntemi seçmeniz gerekir. Üstelik, kullanıcının bir Azure hesabı yoksa, yeni bir hesap için kaydolmaları gerekir. 

   ![İlk abonelik aktarımı Web sayfası](./media/billing-subscription-transfer/billing-accept-ownership-step1.png)

   ![İkinci abonelik aktarımı Web sayfası](./media/billing-subscription-transfer/billing-accept-ownership-step2.png)

   ![İkinci abonelik aktarımı Web sayfası](./media/billing-subscription-transfer/billing-accept-ownership-step3.png)

1. Başarılı! Abonelik artık aktarılmıştır.

## <a name="transferring-subscription-to-an-account-in-another-azure-ad-tenant"></a>Başka bir Azure AD kiracısındaki bir hesaba abonelik aktarılıyor

Azure 'a kaydolduğunuzda, sizin için bir Azure Active Directory (AD) kiracısı oluşturulur. Kiracı hesabınızı temsil eder. Aboneliklerinizi, aboneliklerinize ve kaynaklarınıza erişimi yönetmek için kullanırsınız.

Yeni bir abonelik oluşturduğunuzda, hesabınız Azure AD kiracısında barındırılır. Başkalarına aboneliğinize veya kaynaklarına erişim sağlamak istiyorsanız, bunları kiracınıza katılmaya davet etmeniz gerekir. Bu, aboneliklerinize ve kaynaklarınıza erişimi denetlemenize yardımcı olur.

Aboneliğinizin faturalandırma sahipliğini başka bir Azure AD kiracısındaki bir hesaba aktardığınızda, aboneliği yeni hesabın kiracısına taşıyabilirsiniz. Bunu yaparsanız, abonelikleri ve kaynaklarını yönetmek için [rol tabanlı erişim (RBAC)](../role-based-access-control/role-assignments-portal.md) olan tüm kullanıcılar, gruplar veya hizmet sorumluları erişimleri kaybeder. Yalnızca Aktarım isteğinizi kabul eden yeni hesaptaki Kullanıcı, kaynakları yönetmek için erişime sahip olur. İlk olarak erişimi olan kullanıcılara erişim sağlamak için yeni sahibin [Bu kullanıcıları aboneliğe el ile eklemesi](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal)gerekir.


## <a name="transferring-visual-studio-mpn-and-pay-as-you-go-devtest-subscriptions"></a>Visual Studio 'yu aktarma, MPN ve kullandıkça öde geliştirme/test abonelikleri

Visual Studio ve Microsoft İş Ortağı Ağı abonelikleriyle ilişkili aylık yinelenen Azure kredisi vardır. Bu abonelikleri aktardığınızda krediniz hedef faturalandırma hesabında kullanılamaz. Abonelik, hedef faturalandırma hesabındaki krediyi kullanır. Örneğin, Bob bir Visual Studio Enterprise aboneliğini 9 Eylül ve kemal 'teki gamze 'nin hesabına aktardıysanız, aktarımı kabul eder. Aktarım tamamlandıktan sonra, abonelik gamze 'nin hesabında kredisi kullanmaya başlar. Kredi 9 ' da her ay sıfırlanır. 


<a id="EA"></a>

## <a name="transfer-billing-ownership-of-enterprise-agreement-ea-subscriptions"></a>Kurumsal Anlaşma (EA) aboneliklerinin fatura sahipliğini aktarma

Kurumsal Yönetici, aboneliklerin sahipliğini bir kayıt içindeki hesaplar arasında aktarabilir. Daha fazla bilgi için, bkz. EA portalındaki [hesap sahipliğini aktarma](https://ea.azure.com/helpdocs/changeAccountOwnerForASubscription) .

## <a name="next-steps-after-accepting-billing-ownership"></a>Faturalama sahipliğini kabul ettikten sonraki adımlar

Bir Azure aboneliğinin faturalandırma sahipliğini kabul ettiyseniz, bu sonraki adımları incelemenizi öneririz:

1. Hizmet Yöneticisi, ortak yöneticiler ve diğer RBAC rollerini gözden geçirin ve güncelleştirin. Daha fazla bilgi edinmek için bkz. [Azure abonelik yöneticileri ekleme veya değiştirme](billing-add-change-azure-subscription-administrator.md) ve [RBAC ve Azure Portal erişimi yönetme](../role-based-access-control/role-assignments-portal.md).
1. Bu aboneliğin hizmetleriyle ilişkili kimlik bilgilerini şu şekilde güncelleştirin:
   1. Abonelik kaynaklarına Kullanıcı Yöneticisi hakları veren yönetim sertifikaları. Daha fazla bilgi için bkz. [Azure için yönetim sertifikası oluşturma ve karşıya yükleme](../cloud-services/cloud-services-certs-create.md)
   1. Depolama gibi hizmetler için erişim anahtarları. Daha fazla bilgi için bkz. [Azure depolama hesapları hakkında](../storage/common/storage-create-storage-account.md)
   1. Azure sanal makineleri gibi hizmetler için uzaktan erişim kimlik bilgileri.
1. Bir iş ortağıyla çalışıyorsanız, Bu abonelikteki iş ortağı KIMLIĞINI güncelleştirmeyi göz önünde bulundurun. [Azure Portal](https://portal.azure.com)Iş ortağı kimliğini güncelleştirebilirsiniz. Daha fazla bilgi için bkz. [Azure hesaplarınıza bir iş ortağı kimliği bağlama](billing-partner-admin-link-started.md)

<a id="supported"></a>

## <a name="supported-subscription-types"></a>Desteklenen Abonelik türleri

Azure portal abonelik aktarımı aşağıda listelenen Abonelik türleri için kullanılabilir. Şu anda Aktarım, [ücretsiz deneme](https://azure.microsoft.com/offers/ms-azr-0044p/) veya [Open ile Azure (AIO)](https://azure.microsoft.com/offers/ms-azr-0111p/) abonelikleri için desteklenmez. Geçici bir çözüm için bkz. [kaynakları yeni kaynak grubuna veya aboneliğe taşıma](../azure-resource-manager/resource-group-move-resources.md). [Sponsorluk](https://azure.microsoft.com/offers/ms-azr-0036p/) veya destek planları gibi diğer abonelikleri aktarmak Için [Azure desteği 'ne başvurun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

- [Kurumsal Anlaşma (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/)\*
- [Microsoft iş ortağı ağı](https://azure.microsoft.com/offers/ms-azr-0025p/)  
- [Visual Studio Enterprise (MPN) aboneleri](https://azure.microsoft.com/offers/ms-azr-0029p/) 
- [MSDN Platformları](https://azure.microsoft.com/offers/ms-azr-0062p/)  
- [Kullandıkça öde](https://azure.microsoft.com/offers/ms-azr-0003p/) 
- [Kullandıkça Öde Geliştirme ve Test](https://azure.microsoft.com/offers/ms-azr-0023p/)
- [Visual Studio Enterprise](https://azure.microsoft.com/offers/ms-azr-0063p/)
- [Visual Studio Enterprise: BizSpark](https://azure.microsoft.com/offers/ms-azr-0064p/)
- [Visual Studio Professional](https://azure.microsoft.com/offers/ms-azr-0059p/)
- [Visual Studio Test Professional](https://azure.microsoft.com/offers/ms-azr-0060p/)
- [Microsoft Azure planı](https://azure.microsoft.com/offers/ms-azr-0017g/)\*\*

\*[EA Portalı aracılığıyla](#EA).

\*\*Yalnızca Azure Web sitesinde kaydolma sırasında oluşturulan hesaplar için desteklenir. 

<a id="faq"></a>

## <a name="frequently-asked-questions-faq-for-senders"></a>Gönderenler için sık sorulan sorular (SSS)

Bu SSS 'ler, bir Azure aboneliğinin faturalandırma sahipliğini başka bir hesaba aktaran kullanıcılar için geçerlidir.

### <a name="whoisaa"></a>Bir hesabın faturalandırma Yöneticisi kim?

Faturalandırma Yöneticisi, bir hesap için faturalandırmayı yönetme iznine sahip olan bir kişidir. [Azure Portal](https://portal.azure.com) faturalandırmaya erişme yetkisine sahiptir ve abonelik oluşturma, faturaları görüntüleme ve ödeme veya ödeme yöntemlerini güncelleştirme gibi çeşitli faturalandırma görevlerini gerçekleştirebilir.

Faturalama yöneticisi olduğunuz hesapları belirlemek için aşağıdaki adımları kullanın:

1. [Azure Portal maliyet yönetimi + faturalandırma sayfasını](https://portal.azure.com/#blade/Microsoft_Azure_Billing/ModernBillingMenuBlade/Overview)ziyaret edin.
1. Sol bölmedeki **Tüm faturalama kapsamları** ' nı seçin. 
1. Abonelikler sayfasında, bir faturalandırma Yöneticisi olduğunuz tüm abonelikler listelenir.

### <a name="does-everything-transfer-including-resource-groups-vms-disks-and-other-running-services"></a>Her şey aktarılsın mı? Kaynak grupları, VM 'Ler, diskler ve diğer çalışan hizmetler de dahil edilsin mi?

VM 'Ler, diskler ve Web siteleri gibi tüm kaynaklarınız yeni hesaba aktarılır. Ancak, aboneliği başka bir Azure AD kiracısındaki bir hesaba aktarırsanız, abonelikteki tüm [yönetici rolleri](billing-add-change-azure-subscription-administrator.md) ve [rol tabanlı Access Control (RBAC)](../role-based-access-control/role-assignments-portal.md) atamaları [aktarılmaz](#transferring-subscription-to-an-account-in-another-azure-ad-tenant). Ayrıca, [uygulama kayıtları](../active-directory/develop/quickstart-v1-integrate-apps-with-azure-ad.md) ve kiracıya özgü diğer hizmetler abonelikle birlikte aktarılmaz.

### <a name="can-i-transfer-ownership-to-an-account-in-another-country"></a>Sahipliği başka bir ülkede bir hesaba aktarabilir miyim?
Ne yazık ki, Azure portal çapraz ülke aktarımları gerçekleştirilemez. Aboneliğinizi ülkeler arasında aktarmak için [desteğe başvurun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

### <a name="i-am-an-administrator-on-two-accounts-can-i-transfer-a-subscription-from-one-of-my-accounts-to-another"></a>İki hesap üzerinde yöneticim. Hesaplarından birinden diğerine abonelik aktarabilir miyim?
Evet, hesaplarınız arasında aboneliği aktarabilirsiniz. Hesaplarınız, hesaplar arasında abonelikler aktarmak için yukarıdaki adımları kullanabilmeniz için kavramsal olarak iki farklı kullanıcı hesabı olarak kabul edilir.

### <a name="does-a-subscription-transfer-result-in-any-service-downtime"></a>Abonelik aktarımı herhangi bir hizmette kesinti oluşmasına neden olur mu?

Aynı Azure AD kiracısındaki bir hesaba bir abonelik aktarırsanız, abonelikte çalışan kaynaklar üzerinde herhangi bir etkisi yoktur. Ancak, aboneliği başka bir Kiracıdaki bir hesaba aktarırsanız ve aboneliği kiracıya taşımaya karar verirseniz, abonelikteki kaynakları yönetmek için [rol tabanlı erişim (RBAC)](../role-based-access-control/overview.md) olan tüm kullanıcılar, gruplar ve hizmet sorumluları, erişimleri kaybeder . Bu, hizmet kapalı kalma süresine yol açabilir.

### <a name="do-users-in-new-account-have-access-to-usage-and-billing-history"></a>Yeni hesaptaki kullanıcılar kullanım ve faturalandırma geçmişine erişebilsin mi?

Yeni hesaptaki kullanıcılara sunulan tek bilgi, aboneliğinizin en son ayının maliyetidir. Kullanım ve faturalandırma geçmişinin geri kalanı abonelikle aktarılmaz

### <a name="how-do-i-migrate-data-and-services-for-my-azure-subscription-to-new-subscription"></a>Azure Aboneliğimin verilerini ve hizmetlerini yeni aboneliğe geçirmek Nasıl yaparım? mı?

Abonelik sahipliğini aktaramıyorsunuz, kaynaklarınızı el ile geçirebilirsiniz. Bkz. [kaynakları yeni kaynak grubuna veya aboneliğe taşıma](../azure-resource-manager/resource-group-move-resources.md).

### <a name="if-i-transfer-a-visual-studio-or-microsoft-partner-network-subscription-does-my-credit-carry-forward-with-the-subscription-in-the-new-account"></a>Bir Visual Studio veya Microsoft İş Ortağı Ağı aboneliği aktardığımda, kredimin yeni hesaptaki abonelikle ileri doğru mu?

Hayır, krediniz yeni hesapta kullanılamıyor. Aktarım isteğini kabul eden kullanıcının, aktarım isteğini kabul etmesi için bir Visual Studio lisansına sahip olması gerekir. Abonelik, kullanıcının hesabında bulunan Visual Studio kredisi 'nı kullanır. Daha fazla bilgi için bkz. [Visual Studio 'Yu aktarma, Microsoft iş ortağı ağı (MPN) ve kullandıkça öde geliştirme/test abonelikleri](#transferring-visual-studio-mpn-and-pay-as-you-go-devtest-subscriptions).


## <a name="frequently-asked-questions-faq-for-recipients"></a>Alıcılar için sık sorulan sorular (SSS)

Bu SSS 'ler, başka bir hesaptan bir Azure aboneliğinin faturalandırma sahipliğini kabul eden kullanıcılar için geçerlidir.

### <a name="if-i-take-over-billing-ownership-of-a-subscription-from-another-account-do-users-in-that-account-continue-to-have-access-to-my-resources"></a>Başka bir hesaptan aboneliğin faturalama sahipliğini devralıyorsa, söz konusu hesaptaki kullanıcılar kaynaklarıma erişime sahip olmaya devam ediyor mu?

Evet. Ancak, hesabınız aboneliğin kiracısından farklı bir Azure AD kiracısında ise ve aktarım isteğini gönderen kullanıcı aboneliği hesabınızın kiracısına, tüm [yönetici rollerine](billing-add-change-azure-subscription-administrator.md) ve [rol tabanlı Access Control (RBAC) taşıdıkça ](../role-based-access-control/role-assignments-portal.md)atamalar kaldırılır. Abonelikte kaynakları yönetmek üzere [rol tabanlı erişim (RBAC)](../role-based-access-control/overview.md) erişimi olan kullanıcıları görüntülemek için aşağıdaki adımları kullanın:

1. [Azure Portal abonelik sayfasını](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade)ziyaret edin.
1. Denetlemek istediğiniz aboneliği seçin ve ardından sol bölmedeki **erişim denetimi (IAM)** seçeneğini belirleyin.
1. Sayfanın üst kısmından **rol atamaları** ' nı seçin. Rol atamaları sayfasında, abonelikte RBAC erişimi olan tüm kullanıcılar listelenir.

[Rol tabanlı Access Control (RBAC)](../role-based-access-control/role-assignments-portal.md) atamaları aktarım sırasında kaldırılsa da, özgün sahip hesabındaki kullanıcılar yine de aşağıdakiler dahil bazı güvenlik mekanizmaları aracılığıyla aboneliğe erişebilmeye devam edebilir.

* Abonelik kaynaklarına Kullanıcı Yöneticisi hakları veren yönetim sertifikaları. Daha fazla bilgi için bkz. [Azure Için yönetim sertifikası oluşturma ve karşıya yükleme](../cloud-services/cloud-services-certs-create.md).
* Depolama gibi hizmetler için erişim anahtarları. Daha fazla bilgi için bkz. [Azure depolama hesapları hakkında](../storage/common/storage-create-storage-account.md).
* Azure sanal makineleri gibi hizmetler için uzaktan erişim kimlik bilgileri.

Alıcının kaynaklarına erişmesi gerekiyorsa, hizmet ile ilişkili tüm gizli dizileri güncellemeyi göz önünde bulundurmalıdır. Çoğu kaynak aşağıdaki adımlar kullanılarak güncelleştirilemeyebilir:

  1. [Azure Portal](https://portal.azure.com) oturum açın.
  2. Hub menüsünde **tüm kaynaklar**' ı seçin.
  3. Kaynağı seçin.
  4. Kaynak sayfasında, **Ayarlar**' a tıklayın. Burada, mevcut gizli dizileri görüntüleyebilir ve güncelleştirebilirsiniz.

### <a name="if-i-take-over-the-billing-ownership-of-a-subscription-in-the-middle-of-the-billing-cycle-do-i-have-to-pay-for-the-entire-billing-cycle"></a>Faturalandırma döngüsünün ortasında bir aboneliğin faturalama sahipliğini devralıyorsa, tüm faturalandırma döngüsünün ödemem gerekir mi?

Hesabınız, aktarma zamanından itibaren raporlanan tüm kullanımlar için ödeme yapmaktan sorumludur. Aktarmadan önce gerçekleşen ancak daha sonra raporlanan bazı kullanımlar olabilir. Kullanım hesabı faturanızda yer alır.

### <a name="can-i-use-a-different-payment-method"></a>Farklı bir ödeme yöntemi kullanabilir miyim?

Evet. Aktarım isteğini kabul ederken, hesabınıza bağlı mevcut bir ödeme yöntemini seçebilir veya yeni bir ödeme yöntemi ekleyebilirsiniz.

## <a name="troubleshooting"></a>Sorun giderme

### <a id="no-button"></a>"Aboneliği aktar" düğmesini neden görmüyorum?

Self servis abonelik aktarımı faturalandırma hesabınız için kullanılamaz. Şu anda, Azure portal Kurumsal Anlaşma (EA) hesaplarındaki aboneliklerin fatura sahipliğinin aktarımını desteklemiyoruz. Üstelik, Microsoft temsilcisiyle çalışırken oluşturulan Microsoft Müşteri Sözleşmesi hesapları faturalandırma sahipliğinin aktarılmasını desteklemez. 

### <a id="no-button"></a>Abonelik türü neden aktarımı desteklemiyor? 

Abonelik türlerinin hepsi faturalandırma sahiplik aktarımını desteklemez. Aktarımları destekleyen Abonelik türleri listesini görüntülemek için bkz. [desteklenen Abonelik türleri](#supported-subscription-types)

### <a id="no-button"></a>Bir aboneliğin fatura sahipliğini aktarmaya çalıştığımda neden erişim reddedildi hatası alıyorum? 

Microsoft Azure planı aboneliğini aktarmaya çalışıyorsanız ve gerekli izniniz yoksa bu hatayı görürsünüz. Microsoft Azure planı aboneliğini aktarmak için, aboneliğin faturalandırılabileceği fatura bölümünde bir sahip veya katkıda bulunan olması gerekir. Daha fazla bilgi için bkz. [Faturalanacak abonelikleri yönetme bölümü](billing-understand-mca-roles.md#manage-subscriptions-for-invoice-section).


## <a name="need-help-contact-us"></a>Yardım mı gerekiyor? Bizimle iletişim kurun.

Sorularınız varsa veya yardıma ihtiyacınız varsa, [destek isteği oluşturun](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Sonraki adımlar

- Hizmet Yöneticisi, ortak yöneticiler ve diğer RBAC rollerini gözden geçirin ve güncelleştirin. Daha fazla bilgi edinmek için bkz. [Azure abonelik yöneticileri ekleme veya değiştirme](billing-add-change-azure-subscription-administrator.md) ve [RBAC ve Azure Portal erişimi yönetme](../role-based-access-control/role-assignments-portal.md).
