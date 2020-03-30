---
title: Azure Market’teki yönetilen hizmetler teklifleri
description: Yönetilen hizmetler, hizmet sağlayıcıların Azure Marketi'ndeki müşterilere kaynak yönetimi teklifleri satmasına olanak tanır.
ms.date: 03/17/2020
ms.topic: conceptual
ms.openlocfilehash: 2d6e39f753736c0582e9d91870a99b66ae41255b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79500791"
---
# <a name="managed-services-offers-in-azure-marketplace"></a>Azure Market’teki yönetilen hizmetler teklifleri

Bu makalede, [Azure Marketi'ndeki](https://azuremarketplace.microsoft.com)yeni Yönetilen **Hizmetler** teklif türü açıklanmaktadır. Yönetilen hizmetler, Azure temsilci kaynak yönetimi ile müşterilere kaynak yönetimi hizmetleri sunmanıza olanak tanır. Bu teklifleri tüm potansiyel müşteriler veya yalnızca bir veya daha fazla belirli müşteri için kullanılabilir hale getirebilirsiniz. Bu yönetilen hizmetlerle ilgili maliyetler için doğrudan müşterilere fatura kesildiğinden, Microsoft tarafından herhangi bir ücret alınmaz.

## <a name="understand-managed-services-offers"></a>Yönetilen hizmet tekliflerini anlama

Yönetilen hizmetler, Azure temsilci kaynak yönetimi için müşterilerin biniş sürecini kolaylaştırıyor. Bir müşteri Azure Marketi'nde bir teklif satın aldığında, hangi aboneliklerin ve/veya kaynak gruplarının dahil edilmesi gerektiğini belirtebilir. Aboneliğin öncelikle **Microsoft.ManagedServices** kaynak sağlayıcısını el ile kaydederek onboarding için yetkilendirilmesi gerektiğini unutmayın.

Bundan sonra, kuruluşunuzdaki kullanıcılar, [Bulut İş Ortağı Portalı'nda](https://cloudpartner.azure.com/)teklifi oluştururken tanımladığınız erişime göre, kuruluşunuzun kiracısı içinden bu kaynaklar için yönetim görevlerini gerçekleştirebilecektir. Bu işlem, Azure temsilci kaynak yönetimini kullanarak müşteri kaynaklarına erişimi olacak Azure AD kullanıcılarını, gruplarını ve hizmet ilkelerini belirten bir bildirim ve erişim düzeylerini tanımlayan roller aracılığıyla yapılır. Bir dizi bireysel kullanıcı veya uygulama hesabı yerine bir Azure REKLAM grubuna izin atayarak, erişim gereksinimleriniz değiştiğinde tek tek kullanıcıları ekleyebilir veya kaldırabilirsiniz.

## <a name="public-and-private-offers"></a>Genel ve özel teklifler

Yönetilen her hizmet teklifi bir veya daha fazla plan içerir. Planlar özel veya genel olabilir. 

Teklifinizi belirli müşterilerle sınırlamak istiyorsanız, özel bir plan yayımlayabilirsiniz. Bunu yaptığınızda, plan yalnızca sağladığınız belirli] abonelik iyelikleri için satın alınabilir. Daha fazla bilgi için [Özel tekliflere](../../marketplace/private-offers.md)bakın.

Genel planlar, hizmetlerinizi yeni müşterilere tanıtmanıza izin sağlar. Bunlar genellikle müşterinin kiracısına sınırlı erişim gerektiren durumlarda daha uygundur. Bir müşteriyle ilişki kurduktan sonra, kuruluşunuza ek erişim vermeye karar verirlerse, bunu yalnızca o müşteri için yeni bir özel plan yayımlayarak veya [Azure Kaynak Yöneticisi şablonlarını kullanarak daha fazla erişim için bunları](../how-to/onboard-customer.md)ekleyerek yapabilirsiniz.

Uygunsa, hem genel hem de özel planları aynı teklife dahil edebilirsiniz.

> [!IMPORTANT]
> Bir plan genel olarak yayımlandıktan sonra, bunu özel olarak değiştiremezsiniz. Hangi müşterilerin teklifinizi kabul edip kaynakları temsilci olarak verebileceğini denetlemek için özel bir plan kullanın. Genel bir planla, belirli müşterilerle veya hatta belirli sayıda müşteriyle kullanılabilirliği kısıtlayamazsınız (ancak bunu yapmayı seçerseniz planı tamamen satmayı durdurabilirsiniz). Bir müşteri teklifi kabul ettikten sonra, teklifi yayınladığınızda [Yönetilen Hizmetler Kaydı Atama Silme Rolüne](../../role-based-access-control/built-in-roles.md#managed-services-registration-assignment-delete-role) Rol **Tanımı** kümesine bir **Yetkilendirme** dahil ettiğinizde bir [temsilciye erişimi kaldırabilirsiniz.](../how-to/onboard-customer.md#remove-access-to-a-delegation) Ayrıca müşteriye ulaşabilir ve [erişiminizi kaldırmalarını isteyebilirsiniz.](../how-to/view-manage-service-providers.md#add-or-remove-service-provider-offers)

## <a name="publish-managed-service-offers"></a>Yönetilen hizmet tekliflerini yayımlama

Yönetilen bir hizmet teklifini nasıl yayımlayacağınızı öğrenmek [için](../how-to/publish-managed-services-offers.md)bkz. Bulut İş Ortağı Portalını kullanarak Azure Marketi'nde yayımlama hakkında genel bilgi için [Azure Marketi ve AppSource Yayımlama Kılavuzu'na](../../marketplace/marketplace-publishers-guide.md) bakın ve [Azure ve AppSource Marketplace tekliflerini yönetin.](../../marketplace/cloud-partner-portal/manage-offers/cpp-manage-offers.md)

## <a name="next-steps"></a>Sonraki adımlar

- Azure [temsilci kaynak yönetimi](azure-delegated-resource-management.md) ve [kiracılar arası yönetim deneyimleri](cross-tenant-management-experience.md)hakkında bilgi edinin.
- [Azure Marketi'nde yönetilen hizmet tekliflerini yayımlayın.](../how-to/publish-managed-services-offers.md)
