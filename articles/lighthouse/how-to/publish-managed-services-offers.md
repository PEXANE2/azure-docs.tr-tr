---
title: Azure Market 'Te yönetilen hizmet teklifi yayımlama
description: Onpanolar müşterilerinin Azure tarafından yetkilendirilen kaynak yönetimine yönelik bir yönetilen hizmet teklifini yayımlamayı öğrenin.
ms.date: 05/04/2020
ms.topic: how-to
ms.openlocfilehash: 214a71faca59072660f1e1f413cb107d8e8f6fc9
ms.sourcegitcommit: dee7b84104741ddf74b660c3c0a291adf11ed349
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85920897"
---
# <a name="publish-a-managed-service-offer-to-azure-marketplace"></a>Azure Market 'Te yönetilen hizmet teklifi yayımlama

Bu makalede ortak veya özel yönetilen hizmet teklifini Iş Ortağı Merkezi 'ndeki [ticari Market](../../marketplace/partner-center-portal/commercial-marketplace-overview.md) programını kullanarak [Azure Marketi](https://azuremarketplace.microsoft.com) 'nde nasıl yayımlayacağınızı öğreneceksiniz. Teklifi satın alan müşteriler, [Azure tarafından atanan kaynak yönetimine](../concepts/azure-delegated-resource-management.md)yönelik abonelikleri ve kaynak gruplarını ekleyebilir.

## <a name="publishing-requirements"></a>Yayımlama gereksinimleri

Teklif oluşturmak ve yayımlamak için [Iş Ortağı Merkezi 'nde](../../marketplace/partner-center-portal/create-account.md) geçerli bir hesabınız olması gerekir. Zaten bir hesabınız yoksa [kaydolma işlemi](https://aka.ms/joinmarketplace) , Iş Ortağı Merkezi 'nde hesap oluşturma ve ticari Market programına kaydolma adımlarında size yol açacaktır.

Yönetilen hizmet [teklifi sertifika gereksinimleri](/legal/marketplace/certification-policies#7004-business-requirements)uyarınca, yönetilen bir hizmet teklifi yayımlamak için [gümüş veya altın bir bulut platformu uzmanlığına](/partner-center/learn-about-competencies) sahip olmanız veya bir [Azure uzman msp](https://partner.microsoft.com/membership/azure-expert-msp) olmanız gerekir.

Microsoft İş Ortağı Ağı (MPN) KIMLIĞINIZ, müşteri görevlendirmeleri genelinde etkilerini izlemek için yayımladığınız tekliflerle [otomatik olarak ilişkilendirilir](../../billing/billing-partner-admin-link-started.md) .

> [!NOTE]
> Bir teklifi Azure Marketi 'ne yayımlamak istemiyorsanız, Azure Resource Manager şablonları kullanarak müşterileri el ile ekleyebilirsiniz. Daha fazla bilgi için bkz. [Azure tarafından atanan kaynak yönetimine müşteri ekleme](onboard-customer.md).

## <a name="create-your-offer"></a>Teklifinizi oluşturma

Sağlamanız gereken tüm bilgiler ve varlıklar dahil olmak üzere teklifinizi oluşturma hakkında ayrıntılı yönergeler için bkz. [yönetilen hizmet teklifi oluşturma](../../marketplace/partner-center-portal/create-new-managed-service-offer.md).

Genel yayımlama süreci hakkında bilgi edinmek için bkz. [Azure Marketi ve AppSource Yayımlama Kılavuzu](../../marketplace/marketplace-publishers-guide.md). Ayrıca, özellikle [yönetilen hizmetler](/legal/marketplace/certification-policies#700-managed-services) bölümünü [ticari Market sertifika ilkelerini](/legal/marketplace/certification-policies)gözden geçirmeniz gerekir.

Müşteri teklifinizi eklediğinde bir veya daha fazla abonelik veya kaynak grubu temsilcilicektir ve bu, [Azure tarafından atanan kaynak yönetimi için eklendi](#the-customer-onboarding-process)olacaktır.

> [!IMPORTANT]
> Yönetilen bir hizmet teklifinin her bir planı, kiracınızda bu planı satın alan müşteriler için atanan kaynak gruplarına ve/veya aboneliklerine erişimi olacak Azure Active Directory (Azure AD) varlıklarını tanımladığınız bir **bildirim ayrıntıları** bölümü içerir. Eklediğiniz herhangi bir grup (veya Kullanıcı veya hizmet sorumlusu), planı satın alan her müşteri için aynı izinlere sahip olacağını unutmamak önemlidir. Her müşteriyle çalışacak farklı gruplar atamak için, her müşteri için özel olarak özel bir [plan](../../marketplace/private-offers.md) yayımlamanız gerekir.

## <a name="publish-your-offer"></a>Teklifinizi yayımlayın

Tüm bölümleri tamamladıktan sonra, bir sonraki adımınız teklifi Azure Marketi 'ne yayımlamaktır. Teklifinizi canlı hale getirme sürecini başlatmak için **Yayımla** düğmesini seçin. Bu süreç hakkında daha fazla bilgi için [burada](../../marketplace/partner-center-portal/create-new-managed-service-offer.md#publish)bulunabilir. 

[Teklifinizin güncelleştirilmiş bir sürümünü](../..//marketplace/partner-center-portal/update-existing-offer.md) dilediğiniz zaman yayımlayabilirsiniz. Örneğin, daha önce yayımlanmış bir teklifine yeni bir rol tanımı eklemek isteyebilirsiniz. Bunu yaptığınızda, teklifi zaten eklemiş olan müşteriler, bir güncelleştirmenin kullanılabildiğini bilmesini sağlayan Azure portal [**hizmet sağlayıcıları**](view-manage-service-providers.md) sayfasında bir simge görür. Her müşteri [, değişiklikleri gözden](view-manage-service-providers.md#update-service-provider-offers) geçirebilecek ve yeni sürüme güncelleştirmek isteyip istemediğinize karar verecek. 

## <a name="the-customer-onboarding-process"></a>Müşteri ekleme işlemi

Müşteri teklifinizi ekledikten sonra, [bir veya daha fazla belirli abonelik veya kaynak grubu temsilciliğini](view-manage-service-providers.md#delegate-resources)sağlar ve bu, Azure tarafından yetkilendirilen kaynak yönetimi için eklendi olacaktır. Bir müşteri bir teklifi kabul etmiş, ancak henüz hiç kaynak temsilcisi yoksa, Azure portal [**hizmet sağlayıcılar**](view-manage-service-providers.md) sayfasının en üstünde **sağlayıcı teklifleri** bölümünün üst kısmında bir not görür.

> [!IMPORTANT]
> Temsilciyi, müşterinin kiracısında, eklendi olan abonelik (veya eklendi olan kaynak gruplarını içeren) için [sahip yerleşik rolüne](../../role-based-access-control/built-in-roles.md#owner) sahip konuk olmayan bir hesap tarafından yapılmalıdır. Aboneliği temsil edebilen tüm kullanıcıları görmek için, müşterinin kiracısındaki bir Kullanıcı Azure portal aboneliği seçebilir, **erişim denetimini (IAM)** açabilir ve [sahip rolüne sahip tüm kullanıcıları görüntüleyebilir](../../role-based-access-control/role-assignments-list-portal.md#list-owners-of-a-subscription).

Müşteri bir abonelik (veya bir abonelik içindeki bir veya daha fazla kaynak grubu) atadıktan sonra, bu abonelik için **Microsoft. ManagedServices** kaynak sağlayıcısı kaydedilir ve kiracınızdaki kullanıcılar, teklifinizdeki Yetkilendirmelere göre Temsilcili kaynaklara erişebilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Ticari Market](../../marketplace/partner-center-portal/commercial-marketplace-overview.md)hakkında bilgi edinin.
- [Çapraz kiracı yönetim deneyimleri](../concepts/cross-tenant-management-experience.md)hakkında bilgi edinin.
- Azure portal **müşterilerime** giderek [müşterileri görüntüleyin ve yönetin](view-manage-customers.md) .
