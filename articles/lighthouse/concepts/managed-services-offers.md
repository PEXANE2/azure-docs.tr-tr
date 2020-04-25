---
title: Azure Market 'te yönetilen hizmet teklifleri
description: Yönetilen hizmet, hizmet sağlayıcılarının Azure Marketi 'ndeki müşterilere kaynak yönetimi tekliflerini satmasını sağlar.
ms.date: 03/30/2020
ms.topic: conceptual
ms.openlocfilehash: 13e1825ae6eb50b1b376e3bd3de908a545fbe023
ms.sourcegitcommit: f7fb9e7867798f46c80fe052b5ee73b9151b0e0b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2020
ms.locfileid: "82144905"
---
# <a name="managed-service-offers-in-azure-marketplace"></a>Azure Market 'te yönetilen hizmet teklifleri

Bu makalede, [Azure Marketi](https://azuremarketplace.microsoft.com)'Ndeki **yönetilen hizmet** teklifi türü açıklanmaktadır. Yönetilen hizmet teklifleri, [Azure tarafından atanan kaynak yönetimini](azure-delegated-resource-management.md)kullanarak müşterilere kaynak yönetimi hizmetleri sunmanıza olanak tanır. Bu teklifleri tüm potansiyel müşteriler için veya yalnızca bir veya daha fazla belirli müşteriye kullanılabilir hale getirebilirsiniz. Müşterileri bu yönetilen hizmetlerle ilgili maliyetlerle doğrudan faturalandırınızdan, Microsoft tarafından ücretlendirilmez.

## <a name="understand-managed-service-offers"></a>Yönetilen hizmet tekliflerini anlama

Yönetilen hizmet, Azure tarafından atanan kaynak yönetimi için müşterileri ekleme sürecini kolaylaştırır. Bir müşteri Azure Marketi 'nde bir teklif satın aldığında, hangi aboneliklerin ve/veya kaynak gruplarının eklendi olması gerektiğini belirleyebilecektir.

Bundan sonra kuruluşunuzdaki kullanıcılar, teklifi oluştururken tanımladığınız erişime göre kuruluşunuzun kiracısında bu kaynaklar üzerinde çalışabilir. Bu işlem, Azure tarafından atanan kaynak yönetimi kullanılarak müşteri kaynaklarına erişimi olacak Azure AD kullanıcılarını, gruplarını ve hizmet sorumlularını belirten bir bildirimde ve erişim düzeyini tanımlayan rollerle birlikte yapılır. Bir dizi Kullanıcı veya uygulama hesabı yerine bir Azure AD grubuna izinler atayarak, erişim gereksinimleriniz değiştiğinde bireysel kullanıcı ekleyebilir veya kaldırabilirsiniz.

## <a name="public-and-private-offers"></a>Ortak ve özel teklifler

Her yönetilen hizmet teklifi bir veya daha fazla plan içerir. Planlar özel veya genel olabilir.

Teklifinizi belirli müşterilerle sınırlandırmak istiyorsanız, özel bir plan yayımlayabilirsiniz. Bunu yaptığınızda, plan yalnızca sağladığınız belirli abonelik kimlikleri için satın alınabilir. Daha fazla bilgi için bkz. [özel teklifler](../../marketplace/private-offers.md).

Genel planlar, hizmetlerinizi yeni müşterilere yükseltemenizi sağlar. Bunlar genellikle müşterinin kiracısına sınırlı erişim istediğinizde daha uygundur. Bir müşteriyle ilişki kurduktan sonra, kuruluşunuza ek erişim vermeye karar verirse, bu, yalnızca söz konusu müşteri için yeni bir özel plan yayımlayarak veya [Azure Resource Manager şablonları kullanarak daha fazla erişim sağlamak için bunları](../how-to/onboard-customer.md)ekleyebilirsiniz.

Uygunsa, hem genel hem de özel planları aynı teklifine dahil edebilirsiniz.

> [!IMPORTANT]
> Bir plan ortak olarak yayımlandıktan sonra, özel olarak değiştiremezsiniz. Hangi müşterilerin teklif ve temsilci kaynaklarınızı kabul edeceğini denetlemek için özel bir plan kullanın. Ortak bir plan sayesinde, kullanılabilirliği belirli müşterilerle veya hatta belirli sayıda müşteriye kısıtlayamazsınız (ancak bunu yapmak isterseniz planı tamamen satmaya devam edebilirsiniz). Bir müşteri bir teklifi kabul ettikten sonra, teklifi yayımladığınızda rol **tanımıyla** [yönetilen hizmetler kayıt ataması silme rolü](../../role-based-access-control/built-in-roles.md#managed-services-registration-assignment-delete-role) olarak ayarlanmış bir **Yetkilendirme** eklediyseniz, bir [temsilciye erişimi kaldırabilirsiniz](../how-to/remove-delegation.md) . Ayrıca müşteriye ulaşabilir ve [erişiminizi kaldırmasını](../how-to/view-manage-service-providers.md#add-or-remove-service-provider-offers)isteyebilirsiniz.

## <a name="publish-managed-service-offers"></a>Yönetilen hizmet tekliflerini yayımlama

Yönetilen bir hizmet teklifini yayımlamayı öğrenmek için bkz. [Azure Market 'Te yönetilen hizmetler teklifi yayımlama](../how-to/publish-managed-services-offers.md).

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Temsilcili kaynak yönetimi](azure-delegated-resource-management.md) ve [çapraz kiracı yönetim deneyimleri](cross-tenant-management-experience.md)hakkında bilgi edinin.
- [Yönetilen hizmet tekliflerini](../how-to/publish-managed-services-offers.md) Azure Market 'te yayımlayın.
