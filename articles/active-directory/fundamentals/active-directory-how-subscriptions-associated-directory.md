---
title: Mevcut bir Azure aboneliği, kiracınıza - Azure Active Directory Ekle | Microsoft Docs
description: Azure Active Directory kiracınız için mevcut bir Azure aboneliği ekleme hakkında yönergeler.
services: active-directory
author: msaburnley
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 03/13/2019
ms.author: ajburnle
ms.reviewer: jeffsta
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: a64bad11f5b83ddd7f6d7236ffed4ff4a6e39c2c
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68561854"
---
# <a name="associate-or-add-an-azure-subscription-to-your-azure-active-directory-tenant"></a>Azure Active Directory kiracınız için Azure aboneliği ekleme veya ilişkilendirme

Azure aboneliğinin Azure Active Directory (Azure AD) ile bir güven ilişkisi vardır. Bu, aboneliğin kullanıcıların, hizmetlerin ve cihazların kimliğini doğrulamak için Azure AD 'ye güveneceği anlamına gelir. Birden çok abonelik Azure AD ile aynı dizine güvenebilir ancak her abonelik yalnızca tek bir dizine güvenebilir.

Aboneliğinizin süresi dolarsa abonelikle ilişkili diğer kaynaklara erişimlerini kaybeder. Ancak, Azure AD dizini ilişkilendirmek ve farklı bir Azure aboneliğini kullanarak dizin yönetmenize izin vererek, Azure içinde kalır.

Tüm kullanıcılarınızın kimlik doğrulaması için tek bir *giriş* dizini vardır. Bununla birlikte, kullanıcılarınızın diğer dizinlerde Konuk da olabilir. Azure AD'de her kullanıcı için her iki bir ev ve Konuk dizin görebilirsiniz.

> [!Important]
> Bir aboneliği farklı bir dizinle ilişkilendirdiğinizde, [rol tabanlı erişim denetimi (RBAC)](../../role-based-access-control/role-assignments-portal.md) kullanılarak atanmış rollere sahip kullanıcılar erişimleri kaybeder. Klasik abonelik yöneticileri (Hizmet Yöneticisi ve ortak yöneticiler) da erişimi kaybedecektir.
> 
> Ayrıca, Azure Kubernetes hizmeti (AKS) kümenizi farklı bir aboneliğe taşımak veya küme sahibi aboneliği yeni bir kiracıya taşımak, kayıp rol atamaları ve hizmet sorumluları hakları nedeniyle kümenin işlevselliği kaybetmesine neden olur. AKS hakkında daha fazla bilgi için bkz. [Azure Kubernetes Service (aks)](https://docs.microsoft.com/azure/aks/).

## <a name="before-you-begin"></a>Başlamadan önce

İlişkilendirme veya aboneliğinize eklemek için önce aşağıdaki görevleri gerçekleştirmeniz gerekir:

1. Aşağıdaki değişiklik listesini ve nasıl etkilenebileceğini gözden geçirin:

    - RBAC kullanılarak roller atanmış olan kullanıcılar, erişimini kaybeder
    - Hizmet Yöneticisi ve ortak yöneticiler erişimi kaybedecektir
    - Herhangi bir Anahtar Kasası varsa, bunlar erişilemez olur ve ilişkilendirmeden sonra bunları çözmeniz gerekir
    - Sanal makineler veya Logic Apps gibi kaynaklar için yönetilen kimlikleriniz varsa, ilişkilendirmeyi sonra yeniden etkinleştirmeniz veya yeniden oluşturmanız gerekir
    - Kayıtlı bir Azure Stack varsa ilişkilendirmeden sonra yeniden kaydetmeniz gerekir

1. Bir hesap kullanarak oturum açın:
    - , Abonelik için bir [sahip](../../role-based-access-control/built-in-roles.md#owner) rolü atamasına sahiptir. Sahip rolünü atama hakkında daha fazla bilgi için bkz. [RBAC ve Azure Portal kullanarak Azure kaynaklarına erişimi yönetme](../../role-based-access-control/role-assignments-portal.md).
    - Abonelikle ilişkili iki geçerli dizindeki ve, bundan sonra abonelik ilişkilendirmek istediğiniz yeni dizine bulunmaktadır. Başka bir dizine erişim sağlama hakkında daha fazla bilgi için bkz. [nasıl Azure Active Directory yöneticileri ekleme B2B işbirliği kullanıcıları?](../b2b/add-users-administrator.md).

1. Bir Azure bulut hizmeti sağlayıcıları (CSP) aboneliği (MS-AZR - 0145 P, MS - AZR - 0146 P, MS - AZR - 159 P), bir Microsoft Internal abonelik (MS-AZR - 0015 P) veya Microsoft Imagine aboneliği (MS-AZR - 0144 P) kullanmadığınızdan emin olun.
    
## <a name="to-associate-an-existing-subscription-to-your-azure-ad-directory"></a>Var olan bir aboneliği Azure AD dizininizle ilişkilendirme

1. Oturum açın ve kullanmak istediğiniz aboneliği seçin [Azure portalındaki abonelikler sayfasından](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).

2. Seçin **dizinini Değiştir**.

    ![Abonelik sayfasında, dizin seçeneği vurgulanmış olarak değiştir](media/active-directory-how-subscriptions-associated-directory/change-directory-button.png)

3. Görüntülenen ve ardından tüm uyarıları gözden **değişiklik**.

    ![Dizini değiştirmek için gösteren dizin sayfası Değiştir](media/active-directory-how-subscriptions-associated-directory/edit-directory-ui.png)

    Abonelik için dizinin değiştirilir ve bir başarı iletisi alın.

    ![Dizin değişikliği hakkında başarı iletisi](media/active-directory-how-subscriptions-associated-directory/edit-directory-success.png)
4. Yeni dizininize gitmek için **Dizin değiştiricisini** kullanın. Her şeyin düzgün şekilde gösterilmesi birkaç saat sürebilir. Çok uzun sürmek üzere görünüyorsa, taşınan aboneliğin **genel abonelik filtresini** denetleyerek gizli olmadığından emin olun.

    ![Örnek bilgilerle Dizin değiştirici sayfası](media/active-directory-how-subscriptions-associated-directory/directory-switcher.png)

Abonelik faturalandırma sahipliğini etkilemez abonelik dizininin değiştirilmesi bir hizmet düzeyi işlemi olduğundan. Hesap Yöneticisi hala Hizmet Yöneticisi'nden değiştirebilirsiniz [hesap Merkezi](https://account.azure.com/subscriptions). Özgün dizini silmek için abonelik faturalandırma sahipliğini için yeni bir hesap yöneticisine aktarmanız gerekir Faturalandırma sahipliğini aktarma hakkında daha fazla bilgi için bkz. [Bir Azure aboneliğinin sahipliğini başka bir hesaba aktarma](../../billing/billing-subscription-transfer.md).

## <a name="post-association-steps"></a>İlişkilendirme sonrası adımları
Bir aboneliği farklı bir dizine ilişkilendirdikten sonra, işlemleri sürdürmek için gerçekleştirmeniz gereken ek adımlar olabilir.

1. Herhangi bir Anahtar Kasası varsa, Anahtar Kasası kiracı KIMLIĞINI değiştirmeniz gerekir. Daha fazla bilgi için bkz. [bir abonelik taşıdıktan sonra Anahtar Kasası KIRACı kimliğini değiştirme](../../key-vault/key-vault-subscription-move-fix.md).

2. Kaynaklar için sistem tarafından atanan Yönetilen kimlikler kullanıyorsanız, bunları yeniden etkinleştirmeniz gerekir. Kullanıcı tarafından atanan Yönetilen kimlikler kullanıyorsanız, bunları yeniden oluşturmanız gerekir. Yönetilen kimlikleri yeniden etkinleştirdikten veya yeniden oluşturduktan sonra, bu kimliklere atanan izinleri yeniden oluşturmanız gerekir. Daha fazla bilgi için bkz. [Azure kaynakları için Yönetilen kimlikler nelerdir?](../managed-identities-azure-resources/overview.md).

3. Bu aboneliği kullanarak bir Azure Stack kaydolduysanız, yeniden kaydetmeniz gerekir. Daha fazla bilgi için bkz. [Azure ile Azure Stack kaydetme](/azure-stack/operator/azure-stack-registration).



## <a name="next-steps"></a>Sonraki adımlar

- Yeni bir Azure AD oluşturmak için Kiracı için bkz: [erişim Azure yeni bir kiracı oluşturmak için Active Directory](active-directory-access-create-new-tenant.md)

- Microsoft Azure'da kaynak erişiminin nasıl denetlendiği konusunda daha fazla bilgi için bkz. [Azure'da kaynak erişimini anlama](../../role-based-access-control/rbac-and-directory-admin-roles.md)

- Azure AD'de rol atama hakkında daha fazla bilgi için bkz: [kullanıcılar Azure Active Directory'ye Dizin rolleri atama](active-directory-users-assign-role-azure-portal.md)
