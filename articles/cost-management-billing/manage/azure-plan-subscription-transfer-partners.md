---
title: Azure aboneliklerini bir iş ortağından diğerine aktarma (Önizleme)
description: Bu makale, Azure aboneliğinizin faturalama sahipliğini aktarmadan önce ve aktardıktan sonra bilmeniz gerekenleri anlamanıza yardımcı olur.
author: bandersmsft
ms.reviewer: mcville
ms.service: cost-management-billing
ms.topic: conceptual
ms.date: 09/07/2020
ms.author: banders
ms.openlocfilehash: 246128ec3d26e4bf3f2a5a8f0660aab7d9daca3d
ms.sourcegitcommit: c52e50ea04dfb8d4da0e18735477b80cafccc2cf
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/08/2020
ms.locfileid: "89554219"
---
# <a name="transfer-azure-subscriptions-from-one-partner-to-another-preview"></a>Azure aboneliklerini bir iş ortağından diğerine aktarma (Önizleme)

Bu makale, Azure aboneliğinizin faturalama sahipliğini aktarmadan önce ve aktardıktan sonra bilmeniz gerekenleri anlamanıza yardımcı olur. Azure planı kapsamındaki bir Azure aboneliğini bir Microsoft iş ortağından diğerine aktarmaya başlamak için iş ortağınıza ulaşmanız gerekir. İş ortağı, işleme nasıl başlayacağınız hakkındaki yönergeleri size gönderir. Aktarım süreci tamamlandıktan sonra, aboneliğinizin faturalama sahipliği değişir.

## <a name="user-access"></a>Kullanıcı erişimi

Bu geçiş, Azure rol tabanlı erişim denetimi (RBAC) kullanılarak atanan mevcut kullanıcı, grup ve hizmet sorumlularını etkilemez. [Azure RBAC](../../role-based-access-control/overview.md) Azure kaynaklarına erişebilecek kişileri, bu kişilerin kaynaklarla yapabilecekleri işlemleri ve erişebilecekleri alanları yönetmenize yardımcı olur. Abonelik aktarımında yeni iş ortağınıza kaynaklarınıza RBAC erişimi verilmez. RBAC erişimi önceki iş ortağınızda kalır.

Bu nedenle, Azure RBAC erişimini eski iş ortağınızdan kaldırıp yeni iş ortağınıza eklemeniz önemlidir. Yeni iş ortağınıza erişim verme hakkında daha fazla bilgi için bkz. [Azure rol tabanlı erişim denetimi (Azure RBAC) nedir?](../../role-based-access-control/overview.md) Önceki iş ortağınızın RBAC erişimini kaldırma hakkında daha fazla bilgi için bkz. [Rol atamasını kaldırma](../../role-based-access-control/role-assignments-portal.md#remove-a-role-assignment).

Ayrıca, yeni iş ortağınız aboneliklerinize otomatik olarak [Yönetici Adına (AOBO)](https://channel9.msdn.com/Series/cspdev/Module-11-Admin-On-Behalf-Of-AOBO) erişimi elde etmez. AOBO, iş ortağınızın Azure aboneliklerini sizin adınıza yönetmesi için gereklidir. Yeni iş ortağınıza AOBO erişimi verme hakkında daha fazla bilgi için bkz. [Bir Azure aboneliği hesabının faturalama sahipliğini başka bir hesaba aktarma](billing-subscription-transfer.md).

## <a name="stop-a-transfer"></a>Aktarımı durdurma

Aktarım isteğinin gönderilmesine ilişkin onay aldıktan sonra, **aktarıma devam etmek istemiyorsanız** aşağıdaki seçeneklerden birini kullanabilirsiniz.

- İstek henüz geçerli iş ortağınız tarafından kabul edilmediyse yeni iş ortağınızın başlattığı aktarım isteğini (durum beklemede iken) iptal etmesini isteyebilirsiniz.
- Geçerli iş ortağınızdan, aktarım isteğini aldığında herhangi bir işlem yapmamasını isteyin. Geçerli iş ortağınızın onayı olmadan aktarım işlemi devam edemez. İsteğin süresi dolar.
- Yeni iş ortağınızla _satıcı ilişkinizi_ kaldırabilirsiniz. Bu eylem, aboneliğinizin taşınması özelliğini ortadan kaldırır. İsteği etkin bir şekilde iptal eder.

[Microsoft Yasal](https://www.microsoft.com/legal/) web sitesindeki seçeneklerden birini kullanarak yardım bulabilir, yanlış kullanımı veya şüpheli etkinlikleri bildirebilirsiniz. Uyumluluk ve Etik kapsamında endişe bildirme seçeneği.

## <a name="next-steps"></a>Sonraki adımlar

- Yeni iş ortağınıza RBAC erişimi vermek için bkz. [Azure rol tabanlı erişim denetimi (Azure RBAC) nedir?](../../role-based-access-control/overview.md)
- Yeni iş ortağınıza AOBO erişimi vermek için bkz. [Azure abonelik hesabının faturalama sahipliğini başka bir hesaba aktarma](billing-subscription-transfer.md).