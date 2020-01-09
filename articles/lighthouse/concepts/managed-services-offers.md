---
title: Azure Market’teki yönetilen hizmetler teklifleri
description: Yönetilen hizmetler, hizmet sağlayıcılarının Azure Marketi 'ndeki müşterilere kaynak yönetimi tekliflerini satmasını sağlar.
ms.date: 12/16/2019
ms.topic: conceptual
ms.openlocfilehash: 1b4f0d7457a74afe710a48f429cfe47535a9b122
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75453579"
---
# <a name="managed-services-offers-in-azure-marketplace"></a>Azure Market’teki yönetilen hizmetler teklifleri

Bu makalede, [Azure Marketi](https://azuremarketplace.microsoft.com)'Nde yeni **yönetilen hizmetler** teklif türü açıklanmaktadır. Yönetilen hizmetler teklifi, Azure tarafından atanan kaynak yönetimine sahip müşterilere kaynak yönetimi hizmetleri sunmanıza olanak tanır. Bu teklifleri tüm potansiyel müşteriler için veya yalnızca bir veya daha fazla belirli müşteriye kullanılabilir hale getirebilirsiniz. Müşterileri bu yönetilen hizmetlerle ilgili maliyetlerle doğrudan faturalandırınızdan, Microsoft tarafından ücretlendirilmez.

## <a name="understand-managed-services-offers"></a>Yönetilen hizmet tekliflerini anlama

Yönetilen hizmetler, Azure tarafından atanan kaynak yönetimi için müşterileri ekleme sürecini kolaylaştırır. Bir müşteri Azure Marketi 'nde bir teklif satın aldığında, hangi aboneliklerin ve/veya kaynak gruplarının eklendi olması gerektiğini belirleyebilecektir. Aboneliğin öncelikle **Microsoft. ManagedServices** kaynak sağlayıcısını el ile kaydederek ekleme için yetkilendirilmiş olması gerektiğini unutmayın.

Bundan sonra kuruluşunuzdaki kullanıcılar, [bulut iş ortağı portalı](https://cloudpartner.azure.com/)teklifi oluştururken tanımladığınız erişime göre kuruluşunuzun kiracısından bu kaynaklar için yönetim görevleri gerçekleştirebilir. Bu işlem, Azure tarafından atanan kaynak yönetimi kullanılarak müşteri kaynaklarına erişimi olacak Azure AD kullanıcılarını, gruplarını ve hizmet sorumlularını belirten bir bildirimde ve erişim düzeyini tanımlayan rollerle birlikte yapılır. Bir dizi Kullanıcı veya uygulama hesabı yerine bir Azure AD grubuna izinler atayarak, erişim gereksinimleriniz değiştiğinde bireysel kullanıcı ekleyebilir veya kaldırabilirsiniz.

## <a name="public-and-private-offers"></a>Ortak ve özel teklifler

Her yönetilen hizmet teklifi bir veya daha fazla plan içerir. Planlar özel veya genel olabilir. 

Teklifinizi belirli müşterilerle sınırlandırmak istiyorsanız, özel bir plan yayımlayabilirsiniz. Bunu yaptığınızda, plan yalnızca sağladığınız belirli abonelik kimlikleri için satın alınabilir. Daha fazla bilgi için bkz. [özel teklifler](../../marketplace/private-offers.md).

Genel planlar, hizmetlerinizi yeni müşterilere yükseltemenizi sağlar. Bunlar genellikle müşterinin kiracısına sınırlı erişim istediğinizde daha uygundur. Bir müşteriyle ilişki kurduktan sonra, kuruluşunuza ek erişim vermeye karar verirse, bu, yalnızca söz konusu müşteri için yeni bir özel plan yayımlayarak veya [Azure Resource Manager şablonları kullanarak daha fazla erişim sağlamak için bunları](../how-to/onboard-customer.md)ekleyebilirsiniz.

Uygunsa, hem genel hem de özel planları aynı teklifine dahil edebilirsiniz.

> [!IMPORTANT]
> Bir plan ortak olarak yayımlandıktan sonra, özel olarak değiştiremezsiniz. Hangi müşterilerin teklif ve temsilci kaynaklarınızı kabul edeceğini denetlemek için özel bir plan kullanın. Ortak bir plan sayesinde, kullanılabilirliği belirli müşterilerle veya hatta belirli sayıda müşteriye kısıtlayamazsınız (ancak bunu yapmak isterseniz planı tamamen satmaya devam edebilirsiniz). Bir müşteri teklifi kabul ettiğinde, her zaman bir müşteriye erişebilseniz ve [erişiminizi kaldırmasını](../how-to/view-manage-service-providers.md#add-or-remove-service-provider-offers)istemek için şu anda temsilcileri reddetme veya kaldırma mekanizması yoktur.

## <a name="publish-managed-service-offers"></a>Yönetilen hizmet tekliflerini yayımlama

Yönetilen bir hizmet teklifini yayımlamayı öğrenmek için bkz. [Azure Market 'Te yönetilen hizmetler teklifi yayımlama](../how-to/publish-managed-services-offers.md). Bulut İş Ortağı Portalı kullanarak Azure Marketi 'Nde yayımlama hakkında genel bilgi için bkz. [Azure Market ve Appsource Yayımlama Kılavuzu](../../marketplace/marketplace-publishers-guide.md) ve [Azure ve appsource Market tekliflerini yönetme](../../marketplace/cloud-partner-portal/manage-offers/cpp-manage-offers.md).

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Temsilcili kaynak yönetimi](azure-delegated-resource-management.md) ve [çapraz kiracı yönetim deneyimleri](cross-tenant-management-experience.md)hakkında bilgi edinin.
- [Yönetilen hizmet tekliflerini](../how-to/publish-managed-services-offers.md) Azure Market 'te yayımlayın.
