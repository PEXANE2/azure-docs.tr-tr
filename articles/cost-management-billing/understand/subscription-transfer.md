---
title: Azure aboneliğimin fatura sahipliğini aktarma hakkında
description: Bu makalede, Azure aboneliğinin faturalama sahipliğini başka bir hesaba aktarmadan önce bilmeniz gerekenler açıklanır.
keywords: azure aboneliğini aktarma, azure aboneliği aktarma, azure aboneliğini başka bir hesaba taşıma, azure, abonelik sahibini değiştirme, azure aboneliğini başka bir hesaba aktarma, azure faturalandırma aktarma
author: bandersmsft
ms.reviewer: amberb
tags: billing,top-support-issue
ms.service: cost-management-billing
ms.subservice: billing
ms.topic: conceptual
ms.date: 09/22/2020
ms.author: banders
ms.custom: contperfq1
ms.openlocfilehash: f4dd6d67d60603ed6cad7056cff4bb07dcb1c2e5
ms.sourcegitcommit: dbe434f45f9d0f9d298076bf8c08672ceca416c6
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2020
ms.locfileid: "92149383"
---
# <a name="about-transferring-billing-ownership-for-an-azure-subscription"></a>Azure aboneliğimin fatura sahipliğini aktarma hakkında

Bu makale Azure aboneliğinin faturalama sahipliğini başka bir hesaba aktarmadan önce bilmeniz gerekenleri anlamanıza yardımcı olur. 

Kuruluşunuzdan ayrılacaksanız veya aboneliğinizin faturasının başka bir hesaba yansıtılmasını istiyorsanız Azure aboneliğinizin faturalama sahipliğini aktarmak isteyebilirsiniz. Faturalama sahipliğini başka bir hesaba aktarmak, yeni hesaptaki yöneticilere ödeme görevlerini gerçekleştirme izni sağlar. Bu yöneticiler ödeme yöntemini değiştirebilir, ücretleri görüntüleyebilir ve aboneliği iptal edebilir.

Faturalama sahipliğini değiştirmeyip aboneliğinizin türünü değiştirmek istiyorsanız bkz. [Azure aboneliğinizi başka bir teklifle değiştirme](../manage/switch-azure-offer.md). Abonelikte kaynaklara erişebilecek kişileri denetlemek için bkz. [Azure yerleşik rolleri](../../role-based-access-control/built-in-roles.md).

Kurumsal Anlaşma (EA) müşterisiyseniz kurumsal yöneticileriniz aboneliklerinizin faturalama sahipliğini hesaplar arasında aktarabilir.

Aboneliğin sahipliğini yalnızca hesabın faturalama yöneticisi aktarabilir.

## <a name="determine-account-billing-administrator"></a>Hesap faturalama yöneticisini belirleme

<a name="whoisaa"></a>

Faturalama yöneticisi bir hesabın faturalandırmasını yönetme iznine sahip olan kişidir. [Azure portalında](https://portal.azure.com) faturalandırma işlemine erişmek ve abonelik oluşturma, faturaları görüntüleme ve ödeme ya da ödeme yöntemlerini güncelleştirme gibi çeşitli faturalama görevlerini gerçekleştirmek için yetkilidir.

Faturalama yöneticisi olduğunuz hesapları belirlemek için [Azure portalda Azure Maliyet Yönetimi + Faturalama](https://portal.azure.com/#blade/Microsoft_Azure_Billing/ModernBillingMenuBlade/Overview)'yı ziyaret edin. Sonra sol bölmeden **Tüm faturalama kapsamları**’nı seçin. Abonelikler sayfasında faturalama yöneticisi olduğunuz üç abonelik de gösterilir.

Bir aboneliğin hesap yöneticisinin kim olduğunu bilmiyorsanız [Azure portalda Abonelikler sayfasını](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) ziyaret edin. Ardından denetlemek istediğiniz aboneliği seçin ve **Ayarlar** bölümüne bakın. **Özellikler**'i seçin; aboneliğin hesap yöneticisi **Hesap Yöneticisi** kutusunda gösterilir.


## <a name="supported-subscription-types"></a>Desteklenen abonelik türleri

Azure portalında abonelik aktarımı aşağıda listelenen abonelik türleri için yapılabilir. Şu anda [Ücretsiz Deneme](https://azure.microsoft.com/offers/ms-azr-0044p/) veya [Open Lisansı ile Azure (AIO)](https://azure.microsoft.com/offers/ms-azr-0111p/) abonelikleri için aktarım desteklenmemektedir. Geçici bir çözüm için bkz. [Kaynakları yeni kaynak grubuna veya aboneliğe taşıma](../../azure-resource-manager/management/move-resource-group-and-subscription.md). Destek planları gibi diğer abonelikleri aktarmak için [Azure Desteği ile iletişime geçin](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

- [Kurumsal Anlaşma (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/)<sup>1</sup>
- [Microsoft İş Ortağı Ağı](https://azure.microsoft.com/offers/ms-azr-0025p/)  
- [Visual Studio Enterprise (MPN) aboneleri](https://azure.microsoft.com/offers/ms-azr-0029p/)
- [MSDN Platformları](https://azure.microsoft.com/offers/ms-azr-0062p/)  
- [Kullandıkça Öde](https://azure.microsoft.com/offers/ms-azr-0003p/)
- [Kullandıkça Öde Geliştirme ve Test](https://azure.microsoft.com/offers/ms-azr-0023p/)
- [Visual Studio Enterprise](https://azure.microsoft.com/offers/ms-azr-0063p/)
- [Visual Studio Enterprise: BizSpark](https://azure.microsoft.com/offers/ms-azr-0064p/)
- [Visual Studio Professional](https://azure.microsoft.com/offers/ms-azr-0059p/)
- [Visual Studio Test Professional](https://azure.microsoft.com/offers/ms-azr-0060p/)
- [Microsoft Azure Planı](https://azure.microsoft.com/offers/ms-azr-0017g/)<sup>2</sup>

<sup>1</sup> EA portalı kullanılarak.

<sup>2</sup> Yalnızca Azure web sitesine kaydolurken oluşturulan hesaplar için desteklenir.

## <a name="resources-transferred-with-subscriptions"></a>Aboneliklerle aktarılan kaynaklar

Sanal Makineler, diskler ve web siteleri gibi tüm kaynaklarınız yeni hesaba aktarılır. Öte yandan aboneliği başka bir Azure AD kiracısındaki bir hesaba aktarırsanız abonelikteki hiçbir [yönetici rolü](../manage/add-change-subscription-administrator.md) ve [Azure rol ataması](../../role-based-access-control/role-assignments-portal.md) aktarılmaz. Ayrıca, [uygulama kayıtları](../../active-directory/develop/quickstart-register-app.md) ve kiracıya özgü diğer hizmetler abonelikle birlikte aktarılmaz.

## <a name="transfer-account-ownership-to-another-countryregion"></a>Hesap sahipliğini başka bir ülkeye/bölgeye aktarma

Ne yazık ki Azure portalı kullanarak abonelikleri ülkeler veya bölgeler arasında aktaramazsınız. Öte yandan bir Azure destek isteği açarsanız abonelikler aktarılabilir. Destek isteği oluşturmak için [desteğe başvurun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

## <a name="transfer-a-subscription-from-one-account-to-another"></a>Aboneliği bir hesaptan diğerine aktarma

İki hesabın da yöneticisiyseniz, aboneliği hesaplarınız arasında aktarabilirsiniz. Hesaplarınız kavramsal olarak iki farklı kullanıcının hesabı olarak kabul edildiği için, hesaplarınız arasında abonelikleri aktarabilirsiniz.
Aboneliğinizi aktarma işleminde gereken adımları görüntülemek için bkz. [Azure aboneliğinin faturalama sahipliğini aktarma](../manage/billing-subscription-transfer.md).

## <a name="transferring-a-subscription-shouldnt-create-downtime"></a>Abonelik aktarma işlemi kapalı kalma süresine neden olmamalıdır

Aboneliği aynı Azure AD kiracısında yer alan bir hesaba aktarırsanız abonelikte çalışan kaynaklar üzerinde hiçbir etkisi olmaz. Ancak PowerShell'deki bağlam bilgileri güncelleştirilmediğinden silmeniz veya ayarları değiştirmeniz gerekebilir. Aboneliği başka bir kiracıdaki bir hesaba aktarıp aboneliği kiracıya taşımaya karar verirseniz abonelikteki kaynaklara erişmek için [Azure rol atamalarına](../../role-based-access-control/role-assignments-portal.md) sahip olan tüm kullanıcılar, gruplar ve hizmet sorumluları erişimi kaybeder. Bunun sonucunda hizmette kesinti yaşanabilir.

## <a name="new-account-usage-and-billing-history"></a>Yeni hesap kullanımı ve fatura geçmişi

Yeni hesabın kullanıcılarına yalnızca aboneliğinize ilişkin geçen ayın maliyet bilgileri sağlanır. Kullanım ve fatura geçmişinin kalan bölümü abonelikle birlikte aktarılmaz.

## <a name="manually-migrate-data-and-services"></a>Verileri ve hizmetleri el ile geçirme

Aboneliği aktardığınızda, kaynakları da abonelikle birlikte kalır. Abonelik sahipliğini aktaramıyorsanız kaynaklarını el ile geçirebilirsiniz. Daha fazla bilgi için bkz. [Kaynakları yeni kaynak grubuna veya aboneliğe taşıma](../../azure-resource-manager/management/move-resource-group-and-subscription.md).

## <a name="remaining-subscription-credits"></a>Kalan abonelik kredileri 

Visual Studio veya Microsoft İş Ortağı Ağı aboneliğiniz varsa aylık krediler alırsınız. Krediniz, abonelikle birlikte yeni hesaba taşınmaz. Aktarım isteğini kabul eden kullanıcının, aktarım isteğini kabul etmesi için bir Visual Studio lisansına sahip olması gerekir. Abonelik, kullanıcının hesabında bulunan Visual Studio kredisini kullanır. Daha fazla bilgi için bkz. [Visual Studio ve İş Ortağı Ağı aboneliklerini aktarma](../manage/billing-subscription-transfer.md#transfer-visual-studio-and-partner-network-subscriptions).

## <a name="users-keep-access-to-transferred-resources"></a>Kullanıcılar aktarılan kaynaklara erişmeye devam eder

Abonelikteki kaynaklara erişimi olan kullanıcıların, sahiplik aktarıldıktan sonra da erişimi koruyacaklarını unutmayın. Ancak [yönetici rolleri](../manage/add-change-subscription-administrator.md) ve [Azure rol](../../role-based-access-control/role-assignments-portal.md) atamaları kaldırılabilir. Hesabınız, aboneliğin Azure AD kiracısından farklı bir kiracıda olması ve aktarım isteğini gönderen kullanıcı aboneliği sizin hesabınızın kiracısına taşıması erişimin kaybedilmesine neden olur. 

Abonelikteki kaynaklara erişmek için Azure rol atamalarına sahip olan kullanıcıları Azure portalda görüntüleyebilirsiniz. [Azure portalında Abonelikler sayfasını](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) ziyaret edin. Denetlemek istediğiniz aboneliği seçin ve ardından sol bölmedeki **Erişim denetimi (IAM)** seçeneğini belirleyin. Sonra sayfanın üst kısmından **Rol atamaları**'nı seçin. Rol atamaları sayfasında, abonelikte erişimi olan tüm kullanıcılar listelenir.

Aktarım sırasında [Azure rol](../../role-based-access-control/role-assignments-portal.md) atamaları kaldırılsa bile, ilk sahip hesabındaki kullanıcılar aşağıdaki gibi diğer güvenlik mekanizmaları üzerinden aboneliğe erişmeye devam edebilir:

* Abonelik kaynaklarına kullanıcı yöneticisi hakları veren yönetim sertifikaları. Daha fazla bilgi için bkz. [Azure için Yönetim Sertifikası Oluşturma ve Karşıya Yükleme](../../cloud-services/cloud-services-certs-create.md).
* Depolama gibi hizmetler için erişim anahtarları. Daha fazla bilgi için bkz. [Azure depolama hesapları hakkında](../../storage/common/storage-account-create.md).
* Azure Sanal Makineleri gibi hizmetler için Uzaktan Erişim kimlik bilgileri.

Alıcının kaynaklara erişimi kısıtlaması gerekiyorsa hizmetle ilişkili tüm gizli dizileri güncelleştirmesi faydalı olabilir. Kaynakların çoğu güncelleştirilebilir. [Azure portalda](https://portal.azure.com) oturum açın ve Merkez menüsünde **Tüm kaynaklar**'ı seçin. Sonra, kaynağı seçin. Kaynak sayfasında **Ayarlar**'ı seçin. Orada mevcut gizli dizileri görüntüleyebilir ve güncelleştirebilirsiniz.

## <a name="you-pay-for-usage-when-you-receive-ownership"></a>Sahipliği aldığınızda kullanım için ödeme yaparsınız

Hesabınız aktarım zamanından itibaren raporlanan tüm kullanımlar için ödeme yapmaktan sorumludur. Aktarımdan önce gerçekleşen ancak daha sonra bildirilen kullanımlar olabilir. Kullanım, hesabınızın faturasına dahil edilir.

## <a name="use-a-different-payment-method"></a>Farklı bir ödeme yöntemi kullanma

Aktarım isteğini kabul ederken hesabınıza bağlı mevcut ödeme yöntemini seçebilir veya yeni bir ödeme yöntemi ekleyebilirsiniz.

## <a name="transfer-enterprise-agreement-subscription-ownership"></a>Kurumsal Anlaşma abonelik sahipliğini aktarma

Başlangıçtaki hesap sahibi kuruluştan ayrılmış olsa bile, Kuruluş Yöneticisi tüm hesapların sahiplik durumunu güncelleştirebilir. Azure Kurumsal Anlaşma hesaplarını aktarma hakkında daha fazla bilgi için bkz. [Azure Kurumsal aktarımları](../manage/ea-transfers.md).

## <a name="need-help-contact-us"></a>Yardıma mı ihtiyacınız var? Bize ulaşın.

Sorularınız varsa ya da yardıma gereksinim duyuyorsanız [destek isteği oluşturun](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Sonraki adımlar

- [Azure aboneliğinin faturalama sahipliğini aktarma](../manage/billing-subscription-transfer.md)