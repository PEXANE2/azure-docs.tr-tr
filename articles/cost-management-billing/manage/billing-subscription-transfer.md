---
title: Azure aboneliğinin faturalama sahipliğini aktarma
description: Azure aboneliğinin faturalama sahipliğini başka bir hesaba aktarma işlemi açıklanır.
keywords: azure aboneliğini aktarma, azure aboneliği aktarma, azure aboneliğini başka bir hesaba taşıma, azure, abonelik sahibini değiştirme, azure aboneliğini başka bir hesaba aktarma, azure faturalandırma aktarma
author: bandersmsft
ms.reviewer: amberb
tags: billing,top-support-issue
ms.service: cost-management-billing
ms.subservice: billing
ms.topic: how-to
ms.date: 09/23/2020
ms.author: banders
ms.custom: contperfq1
ms.openlocfilehash: ef497439d77fef63f4b0c902aee06428a30c5ff5
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91276739"
---
# <a name="transfer-billing-ownership-of-an-azure-subscription-to-another-account"></a>Azure aboneliğinin faturalama sahipliğini başka bir hesaba aktarma

Bu makalede Azure aboneliğinin faturalama sahipliğini başka bir hesaba aktarma işleminde gereken adımlar gösterilir. Aboneliğin faturalama sahipliğini aktarmadan önce [Azure aboneliğinin faturalama sahipliğini aktarma hakkında](../understand/subscription-transfer.md) makalesini okuyun.

Faturalama sahipliğinizi koruyup aboneliğinizin türünü değiştirmek istiyorsanız bkz. [Azure aboneliğinizi başka bir teklifle değiştirme](switch-azure-offer.md). Abonelikte kaynaklara erişebilecek kişileri denetlemek için bkz. [Azure yerleşik rolleri](../../role-based-access-control/built-in-roles.md).

Kurumsal Anlaşma (EA) müşterisiyseniz kurumsal yöneticiniz aboneliklerinizin faturalama sahipliğini hesaplar arasında aktarabilir. Daha fazla bilgi için [Hesap sahibini değiştirme](ea-portal-get-started.md#change-account-owner) konusuna bakın.

Aboneliğin sahipliğini yalnızca hesabın faturalama yöneticisi aktarabilir.

## <a name="transfer-billing-ownership-of-an-azure-subscription"></a>Azure aboneliğinin faturalama sahipliğini aktarma

1. [Azure portalında](https://portal.azure.com), aktarmak istediğiniz aboneliği içeren ödeme hesabının yöneticisi olarak oturum açın. Yönetici olup olmadığınızı bilmiyorsanız veya kimin yönetici olduğunu belirlemeniz gerekiyorsa bkz. [Hesap faturalama yöneticisini belirleme](../understand/subscription-transfer.md#whoisaa).
1. **Maliyet Yönetimi + Faturalama** araması yapın.  
   ![Azure portalı aramasını gösteren ekran görüntüsü](./media/billing-subscription-transfer/billing-search-cost-management-billing.png)
1. Sol bölmeden **Abonelikler**’i seçin. Erişim izninize bağlı olarak faturalama kapsamını ve ardından **Abonelikler**'i veya **Azure abonelikleri**'ni seçmeniz gerekebilir.
1. Aktarmak istediğiniz abonelik için **Faturalama sahipliğini aktar**'ı seçin.  
   ![Aktarılacak aboneliği seçin](./media/billing-subscription-transfer/billing-select-subscription-to-transfer.png)
1. Aboneliğin yeni sahibi olacak hesabın faturalama yöneticisi olan kullanıcının e-posta adresini girin.
1. Aboneliğinizi başka bir Azure AD kiracısındaki bir hesaba aktarıyorsanız, aboneliği yeni hesabın kiracısına taşımak isteyip istemediğinizi seçin. Daha fazla bilgi için bkz. [Aboneliği başka Azure AD kiracısındaki bir hesaba aktarma](#transfer-a-subscription-to-another-azure-ad-tenant-account).
    > [!IMPORTANT]
    > Aboneliği yeni hesabın Azure AD kiracısına taşımayı seçerseniz abonelikteki kaynaklara erişmeye yönelik tüm [Azure rol atamaları](../../role-based-access-control/role-assignments-portal.md) kalıcı olarak kaldırılır. Yalnızca aktarım isteğinizi kabul eden yeni hesaptaki kullanıcı, abonelikteki kaynakları yönetmek için erişime sahip olur. Alternatif olarak, aboneliği yeni hesabın kiracısına taşımadan faturalama sahipliğini aktarmak için **Abonelik Azure AD kiracısı** seçeneğinin işaretini temizleyebilirsiniz. Bunu yaparsanız Azure kaynaklarına erişecek mevcut Azure rol atamaları korunur.  
    ![Aktarım gönder sayfası](./media/billing-subscription-transfer/billing-send-transfer-request.png)
1. **Aktarım isteği gönder**’i seçin.
1. Kullanıcı, aktarma isteğinizi gözden geçirme yönergelerini içeren bir e-posta alır.  
   ![Alıcıya gönderilen abonelik aktarım e-postası](./media/billing-subscription-transfer/billing-receiver-email.png)
1. Kullanıcı, aktarma isteğini onaylamak için e-postadaki bağlantıyı seçer ve yönergeleri izler. Ardından kullanıcı, abonelik ödemesini yapmak için kullanılacak bir ödeme yöntemi seçer. Kullanıcının bir Azure hesabı yoksa yeni bir hesap için kaydolması gerekir.  
   ![İlk abonelik aktarımı web sayfası](./media/billing-subscription-transfer/billing-accept-ownership-step1.png)
   ![İkinci abonelik aktarımı web sayfası](./media/billing-subscription-transfer/billing-accept-ownership-step2.png)
   ![Üçüncü abonelik aktarımı web sayfası](./media/billing-subscription-transfer/billing-accept-ownership-step3.png)
1. Başarılı! Abonelik aktarıldı.

## <a name="transfer-a-subscription-to-another-azure-ad-tenant-account"></a>Aboneliği başka bir Azure AD kiracısı hesabına aktarma

Azure'a kaydolduğunuzda sizin için bir Azure Active Directory (AD) kiracısı oluşturulur. Kiracı, hesabınızı temsil eder. Kiracıyı, aboneliklerinize ve kaynaklarınıza erişimi yönetmek için kullanırsınız.

Yeni bir abonelik oluşturduğunuzda hesabınızın Azure AD kiracısında barındırılır. Aboneliğinize veya kaynaklarınıza erişmesi için başkalarına erişim izni vermek isterseniz kiracınıza katılmaya davet etmeniz gerekir. Bunun yapılması, aboneliklerinize ve kaynaklarınıza erişimi denetlemenize yardımcı olur.

Aboneliğinizin faturalama sahipliğini başka bir Azure AD kiracısındaki bir hesaba aktarırken aboneliği yeni hesabın kiracısına taşıyabilirsiniz. Bunu yaparsanız abonelikleri ve kaynaklarını yönetmek için [Azure rol atamalarına](../../role-based-access-control/role-assignments-portal.md) sahip olan tüm kullanıcılar, gruplar veya hizmet sorumluları erişimlerini kaybeder. Yalnızca aktarım isteğinizi kabul eden yeni hesaptaki kullanıcı, kaynakları yönetmek için erişime sahip olur. Yeni sahibin erişimi kaybeden kişilere erişim sağlamak için bu kullanıcıları aboneliğe el ile eklemesi gerekir. Daha fazla bilgi için bkz. [Azure aboneliğini farklı bir Azure AD dizinine aktarma (Önizleme)](../../role-based-access-control/transfer-subscription.md).

## <a name="transfer-visual-studio-and-partner-network-subscriptions"></a>Visual Studio ve İş Ortağı Ağı aboneliklerini aktarma

Visual Studio ve Microsoft İş Ortağı Ağı abonelikleriyle ilişkili aylık yinelenen Azure kredisi vardır. Bu abonelikleri aktardığınızda krediniz hedef ödeme hesabında kullanılamaz. Abonelik, hedef ödeme hesabındaki krediyi kullanır. Örneğin, Kemal bir Visual Studio Enterprise aboneliğini 9 Eylül tarihinde Gamze’nin hesabına aktardı ve Gamze aktarımı kabul etti. Aktarım tamamlandıktan sonra abonelik Gamze'nin hesabındaki krediyi kullanmaya başlar. Kredi her ayın dokuzunda sıfırlanır.

## <a name="next-steps-after-accepting-billing-ownership"></a>Faturalama sahipliğini kabul ettikten sonraki adımlar

Bir Azure aboneliğinin faturalama sahipliğini kabul ettiyseniz, sonraki adımları incelemenizi öneririz:

1. Hizmet Yöneticisi, Ortak Yöneticiler ve diğer Azure rol atamalarını gözden geçirin ve güncelleştirin. Daha fazla bilgi edinmek için bkz. [Azure abonelik yöneticileri ekleme veya değiştirme](add-change-subscription-administrator.md) ve [Azure portalını Azure rol atamalarını ekleme veya kaldırma](../../role-based-access-control/role-assignments-portal.md).
1. Bu aboneliğin hizmetleriyle ilişkili kimlik bilgilerini şu şekilde güncelleştirin:
   1. Abonelik kaynaklarına kullanıcı yöneticisi hakları veren yönetim sertifikaları. Daha fazla bilgi için bkz. [Azure için yönetim sertifikası oluşturma ve karşıya yükleme](../../cloud-services/cloud-services-certs-create.md)
   1. Depolama gibi hizmetler için erişim anahtarları. Daha fazla bilgi için bkz. [Azure depolama hesapları hakkında](../../storage/common/storage-create-storage-account.md)
   1. Azure Sanal Makineleri gibi hizmetler için Uzaktan Erişim kimlik bilgileri.
1. Bir iş ortağıyla çalışıyorsanız, abonelikteki iş ortağı kimliğini güncelleştirmeyi göz önünde bulundurun. [Azure portalından](https://portal.azure.com) iş ortağı kimliğini güncelleştirebilirsiniz. Daha fazla bilgi için bkz. [İş ortağı kimliğini Azure hesaplarınıza bağlama](link-partner-id.md)

## <a name="troubleshooting"></a>Sorun giderme

Abonelikleri aktarırken sorun yaşıyorsanız aşağıdaki sorun giderme bilgilerini kullanın.

### <a name="the-transfer-subscription-option-is-unavailable"></a>"Aboneliği aktar" seçeneği kullanılamıyor

<a name="no-button"></a> 

Ödeme hesabınız için self servis abonelik aktarımı kullanılamamaktadır. Şu anda, Azure portalın Kurumsal Anlaşma (EA) hesaplarındaki aboneliklerin faturalama sahipliğinin aktarımını desteklemiyoruz. Ayrıca, Microsoft temsilcisiyle çalışırken oluşturulan Microsoft Müşteri Sözleşmesi hesapları faturalama sahipliğinin aktarılmasını desteklemez.

###  <a name="not-all-subscription-types-can-transfer"></a>Bazı abonelik türleri aktarılamaz

Abonelik türlerinin hepsi faturalama sahipliğinin aktarımını desteklemez. Aktarımları destekleyen abonelik türlerinin listesini görüntülemek için bkz. [Desteklenen abonelik türleri](../understand/subscription-transfer.md#supported-subscription-types)

###  <a name="access-denied-error-shown-when-trying-to-transfer-subscription-billing-ownership"></a>Abonelik faturalama sahipliğini aktarmaya çalışırken Erişim reddedildi hatası gösteriliyor

Bir Microsoft Azure planı aboneliğini aktarmaya çalışıyorsanız ve gerekli izniniz yoksa bu hatayı görürsünüz. Bir Microsoft Azure planı aboneliğini aktarmak için, aboneliğin faturalandırıldığı fatura bölümünde bir sahip veya katkıda bulunan olmanız gerekir. Daha fazla bilgi için bkz. [Fatura bölümleri için abonelikleri yönetme](../manage/understand-mca-roles.md#manage-subscriptions-for-invoice-section).

## <a name="need-help-contact-us"></a>Yardıma mı ihtiyacınız var? Bize ulaşın.

Sorularınız varsa ya da yardıma gereksinim duyuyorsanız [destek isteği oluşturun](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Sonraki adımlar

- Hizmet Yöneticisi, Ortak Yöneticiler ve diğer Azure rol atamalarını gözden geçirin ve güncelleştirin. Daha fazla bilgi edinmek için bkz. [Azure abonelik yöneticileri ekleme veya değiştirme](add-change-subscription-administrator.md) ve [Azure portalını Azure rol atamalarını ekleme veya kaldırma](../../role-based-access-control/role-assignments-portal.md).
