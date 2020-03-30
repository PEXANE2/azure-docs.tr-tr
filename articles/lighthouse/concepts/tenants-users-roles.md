---
title: Azure Deniz Feneri senaryolarında kiracılar, roller ve kullanıcılar
description: Azure Etkin Dizin kiracıları, kullanıcıları ve rolleri kavramlarını ve Azure Deniz Feneri senaryolarında nasıl kullanılabileceğini anlayın.
ms.date: 03/24/2020
ms.topic: conceptual
ms.openlocfilehash: 7540e17fd80f9a1d8e996295000c126614b838d2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80246900"
---
# <a name="tenants-roles-and-users-in-azure-lighthouse-scenarios"></a>Azure Deniz Feneri senaryolarında kiracılar, roller ve kullanıcılar

[Azure temsilci kaynak yönetimi](azure-delegated-resource-management.md)için müşterilere binmeden önce, Azure Etkin Dizin (Azure AD) kiracılarının, kullanıcılarının ve rollerinin nasıl çalıştığını ve Azure Deniz Feneri senaryolarında nasıl kullanılabileceğini anlamak önemlidir.

*Kiracı,* Azure AD'nin adanmış ve güvenilir bir örneğidir. Genellikle, her kiracı tek bir kuruluşu temsil eder. Azure temsilci kaynak yönetimi, kaynakların bir kiracıdan başka bir kiracıya mantıksal olarak projeksiyonuna olanak tanır. Bu, yönetici kiracıdaki (bir hizmet sağlayıcısına ait olan) kullanıcıların müşterinin kiracısında ki temsilci kaynaklarına erişmesine veya [birden çok kiracıya sahip işletmelerin yönetim işlemlerini merkezileştirmesine](enterprise.md)olanak tanır.

Bu mantıksal projeksiyonu gerçekleştirmek için, müşteri kiracısında bulunan bir abonelik (veya abonelik içindeki bir veya daha fazla kaynak grubu) Azure temsilcikaynak yönetimi için *gemide* olmalıdır. Bu onboarding işlemi, [Azure Kaynak Yöneticisi şablonları aracılığıyla](../how-to/onboard-customer.md) veya [Azure Marketi'ne genel veya özel bir teklif yayımlayarak](../how-to/publish-managed-services-offers.md)yapılabilir.

Hangi onboarding yöntemini seçerseniz seçin, *yetkilendirmeleri*tanımlamanız gerekir. Her yetkilendirme, yönetici kiracıda, devredilen kaynaklara erişimi olacak bir kullanıcı hesabı ve bu kullanıcıların her birinin bu kaynaklar için sahip olacağı izinleri belirleyen yerleşik bir rol belirtir.

## <a name="role-support-for-azure-delegated-resource-management"></a>Azure temsilci kaynak yönetimi için rol desteği

Yetkilendirme tanımlanırken, her kullanıcı hesabına rol [tabanlı erişim denetimi (RBAC) yerleşik rollerinden](../../role-based-access-control/built-in-roles.md)biri atanmalıdır. Özel roller ve [klasik abonelik yöneticisi rolleri](../../role-based-access-control/classic-administrators.md) desteklenmez.

Tüm [yerleşik roller](../../role-based-access-control/built-in-roles.md) şu anda Azure temsilci kaynak yönetimiyle aşağıdaki istisnalar dışında desteklenir:

- [Sahip](../../role-based-access-control/built-in-roles.md#owner) rolü desteklenmez.
- [DataActions](../../role-based-access-control/role-definitions.md#dataactions) izniyle yerleşik roller desteklenmez.
- [Kullanıcı Erişim Yöneticisi](../../role-based-access-control/built-in-roles.md#user-access-administrator) yerleşik rolü desteklenir, ancak yalnızca müşteri [kiracısında yönetilen bir kimliğe rol atamak](../how-to/deploy-policy-remediation.md#create-a-user-who-can-assign-roles-to-a-managed-identity-in-the-customer-tenant)için sınırlı bir amaç için. Genellikle bu rol tarafından verilen başka izinler geçerli olmayacaktır. Bu role sahip bir kullanıcı tanımlarsanız, bu kullanıcının yönetilen kimliklere atayabileceği yerleşik rolü(ler) de belirtmeniz gerekir.

> [!NOTE]
> Azure'a geçerli bir yeni yerleşik rol eklendikten sonra, [Azure Kaynak Yöneticisi şablonlarını kullanarak bir müşteriye binerken](../how-to/onboard-customer.md)atanabilir. Yönetilen bir [hizmet teklifini yayınlarken,](../how-to/publish-managed-services-offers.md)yeni eklenen rolün Bulut İş Ortağı Portalı'nda kullanıma sunulması gecikme olabilir.

## <a name="best-practices-for-defining-users-and-roles"></a>Kullanıcıları ve rolleri tanımlamak için en iyi uygulamalar

Yetkilendirmelerinizi oluştururken, aşağıdaki en iyi uygulamaları öneririz:

- Çoğu durumda, bir dizi tek tek kullanıcı hesabı yerine bir Azure AD kullanıcı grubuna veya hizmet sorumlusuna izin atamak istersiniz. Bu, erişim gereksinimleriniz değiştiğinde planı güncelleştirmek ve yeniden yayımlamak zorunda kalmadan tek tek kullanıcılara erişim eklemenize veya kaldırmanıza olanak tanır.
- Kullanıcıların yalnızca işlerini tamamlamak için gereken izinlere sahip olmaları ve yanlışlıkla hata olasılığını azaltmalarına yardımcı olmak için en az ayrıcalık ilkesine uymayı unutmayın. Daha fazla bilgi için önerilen [güvenlik uygulamalarına](../concepts/recommended-security-practices.md)bakın.
- Gerekirse daha sonra [temsilciye erişimi kaldırabilmeniz için](../how-to/onboard-customer.md#remove-access-to-a-delegation) Yönetilen Hizmetler Kaydı Atama Silme [Rolü'ne](../../role-based-access-control/built-in-roles.md#managed-services-registration-assignment-delete-role) sahip bir kullanıcı ekleyin. Bu rol atanmazsa, devredilen kaynaklar yalnızca müşterinin kiracısındaki bir kullanıcı tarafından kaldırılabilir.
- [Azure portalında Müşterilerim sayfasını görüntülemesi](../how-to/view-manage-customers.md) gereken herhangi bir kullanıcının [Okuyucu](../../role-based-access-control/built-in-roles.md#reader) rolüne (veya Okuyucu erişimini içeren başka bir yerleşik role) sahip olduğundan emin olun.

> [!IMPORTANT]
> Bir Azure REKLAM grubu için izin eklemek için **Grup türü** **Office 365** **değil, Güvenlik** olmalıdır. Grup oluşturulduğunda bu seçenek seçilir. Daha fazla bilgi için [bkz.](../../active-directory/fundamentals/active-directory-groups-create-azure-portal.md)

## <a name="next-steps"></a>Sonraki adımlar

- Azure [temsilci kaynak yönetimi için önerilen güvenlik uygulamaları](recommended-security-practices.md)hakkında bilgi edinin.
- Müşterilerinizi [Azure Kaynak Yöneticisi şablonlarını kullanarak](../how-to/onboard-customer.md) veya Azure [Marketi'nde özel veya genel olarak yönetilen bir hizmet teklifi yayınlayarak](../how-to/publish-managed-services-offers.md)Azure'a devredilen kaynak yönetimine dahil edin.
