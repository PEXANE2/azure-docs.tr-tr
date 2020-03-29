---
title: Azure tarafından atanan temsilcinin kaynak yönetimi
description: Yönetilen hizmetler, hizmet sağlayıcıların Azure Marketi'ndeki müşterilere kaynak yönetimi teklifleri satmasına olanak tanır.
ms.date: 01/30/2020
ms.topic: conceptual
ms.openlocfilehash: 15814b1ca3b1b78de521033836e3614d18fd0c71
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76904309"
---
# <a name="azure-delegated-resource-management"></a>Azure tarafından atanan temsilcinin kaynak yönetimi

Azure temsilci kaynak yönetimi, Azure Deniz Feneri'nin önemli bileşenlerinden biridir. Azure yetkili kaynak yönetimi yle, hizmet sağlayıcıları müşteri etkileşimini ve biniş deneyimlerini basitleştirebilir ve yetkin kaynakları çeviklik ve hassasiyetle ölçekte yönetebilir.

## <a name="what-is-azure-delegated-resource-management"></a>Azure temsilci kaynak yönetimi nedir?

Azure temsilci kaynak yönetimi, kaynakların bir kiracıdan başka bir kiracıya mantıksal olarak projeksiyonuna olanak tanır. Bu, tek bir Azure Etkin Dizin (Azure AD) kiracısındaki yetkili kullanıcıların müşterilerine ait farklı Azure AD kiracılarında yönetim işlemleri gerçekleştirmesini sağlar. Hizmet sağlayıcıları kendi Azure AD kiracılarında oturum açabilir ve temsilcileştirilmiş müşteri abonelikleri ve kaynak gruplarında çalışma yetkisine sahip olabilir. Bu, her bir müşteri kiracısına oturum açmadan müşterileri adına yönetim işlemleri gerçekleştirmelerine olanak tanır.

> [!NOTE]
> Azure temsilci kaynak yönetimi, kiracılar arası yönetimi basitleştirmek için [birden çok Azure AD kiracısına sahip bir kuruluşta](enterprise.md) da kullanılabilir.

Azure temsilci kaynak yönetimi yle, yetkili kullanıcılar müşterinin kiracısında bir hesap olmadan veya müşterinin kiracısının ortak sahibi olmadan doğrudan müşteri aboneliği bağlamında çalışabilir. Ayrıca Azure [portalındaki yeni **Müşterilerim** sayfasında ki tüm temsilci müşteri aboneliklerini görüntüleyebilir ve yönetebilirler.](../how-to/view-manage-customers.md)

[Kiracılar arası yönetim deneyimi,](cross-tenant-management-experience.md) Azure İlkesi, Azure Güvenlik Merkezi ve daha fazlası gibi Azure yönetim hizmetleriyle daha verimli çalışmanıza yardımcı olur. Tüm hizmet sağlayıcı etkinliği, hem servis sağlayıcısının hem de müşterinin kiracılarında depolanan etkinlik günlüğünde izlenir. Bu, hem müşteri hem de hizmet sağlayıcısının herhangi bir değişiklikle ilişkili kullanıcıyı kolayca tanımlayabileceği anlamına gelir.

Bir müşteriyi Azure'a devredilen kaynak yönetimine dahil ettiğinizde, [tekliflerini, hizmet sağlayıcılarını ve temsilci kaynaklarını onaylayıp yönetebilecekleri](../how-to/view-manage-service-providers.md)Azure portalındaki yeni **Hizmet sağlayıcıları** sayfasına erişebilirler. Müşteri bir hizmet sağlayıcısının erişimini iptal etmek isterse, bunu istediği zaman burada yapabilir.

Yeni [Yönetilen Hizmetler teklif türünü Azure Marketi'nde kolayca](../how-to/publish-managed-services-offers.md) yayımlayabilirsiniz ve müşteriler Azure temsilcikaynak yönetimine kolayca dahil edilebilir. Alternatif olarak, [Azure Kaynak Yöneticisi şablonlarını dağıtarak onboarding işlemini](../how-to/onboard-customer.md)tamamlayabilirsiniz.

## <a name="how-azure-delegated-resource-management-works"></a>Azure temsilci kaynak yönetimi nasıl çalışır?

Yüksek düzeyde, Azure temsilci kaynak yönetimi şu şekilde çalışır:

1. Bir hizmet sağlayıcısı olarak, gruplarınızın, hizmet ilkelerinizin veya kullanıcılarınızın müşterinin Azure kaynaklarını yönetmek için ihtiyaç dedeceği erişimi (rolleri) tanımlarsınız. Erişim tanımı, hizmet sağlayıcısının kiracı kimliğini ve teklif için gerekli erişimi içerir ve kiracınızdan yerleşik [ **roleDefinition** değerlerine](../../role-based-access-control/built-in-roles.md) (Katkıda Bulunan, VM Katılımcısı, Okuyucu, vb.) eşlenen **principalId** kimlikleri kullanılarak tanımlanır.
2. Bu erişimi ve müşteriyi Azure temsilcili kaynak yönetimine iki şekilde belirtirsiniz:
   - Müşterinin kabul edeceği [bir Azure Marketi yönetilen hizmet teklifi](../how-to/publish-managed-services-offers.md) (özel veya herkese açık) yayımlama
   - Bir veya daha fazla özel abonelik veya kaynak grubu için [müşterinin kiracısına azure kaynak yöneticisi şablonu dağıtma](../how-to/onboard-customer.md)
3. Müşteri uçağa bindikten sonra, yetkili kullanıcılar hizmet sağlayıcınızla oturum açabilir ve belirlediğiniz erişime bağlı olarak verilen müşteri kapsamında yönetim görevlerini gerçekleştirebilir.

> [!NOTE]
> Ayrı bulutlar arasında iki kiracı arasında bir abonelik delegasyonu desteklenmez.

## <a name="support-for-azure-delegated-resource-management"></a>Azure temsilci kaynak yönetimi desteği

Azure temsilci kaynak yönetimiyle ilgili yardıma ihtiyacınız varsa, Azure portalında bir destek isteği açabilirsiniz. **Sorun türü için** **Teknik'i**seçin. Bir abonelik seçin, ardından **Deniz Feneri'ni** seçin **(İzleme & Yönetimi**altında).

## <a name="next-steps"></a>Sonraki adımlar

- Kiracılar [arası yönetim deneyimleri](cross-tenant-management-experience.md)hakkında bilgi edinin.
- Azure [Marketi'nde yönetilen hizmet teklifleri](managed-services-offers.md)hakkında bilgi edinin.