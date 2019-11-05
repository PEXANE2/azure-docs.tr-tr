---
title: Kiracınıza mevcut bir Azure aboneliği ekleyin-Azure Active Directory | Microsoft Docs
description: Azure Active Directory kiracınıza mevcut bir Azure aboneliğinin nasıl ekleneceği hakkında yönergeler.
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
ms.openlocfilehash: f6bce05b99c14fb464cd1a17587d9cf254909d87
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73473405"
---
# <a name="associate-or-add-an-azure-subscription-to-your-azure-active-directory-tenant"></a>Azure Active Directory kiracınıza bir Azure aboneliği ilişkilendirin veya ekleyin

Azure aboneliğinin Azure Active Directory (Azure AD) ile bir güven ilişkisi vardır. Bu, aboneliğin kullanıcıların, hizmetlerin ve cihazların kimliğini doğrulamak için Azure AD 'ye güveneceği anlamına gelir. Birden çok abonelik aynı Azure AD dizinine güvenebilir, ancak her abonelik yalnızca tek bir dizine güvenebilir.

Aboneliğinizin süresi dolarsa, abonelikle ilişkili diğer tüm kaynaklara erişiminizi kaybedersiniz. Ancak Azure AD dizini, Azure 'da kalır ve farklı bir Azure aboneliği kullanarak dizini ilişkilendirebilir ve yönetebilir.

Tüm kullanıcılarınızın kimlik doğrulaması için tek bir *giriş* dizini vardır. Bununla birlikte, kullanıcılarınız da diğer dizinlerde Konuk da olabilir. Azure AD 'de her bir kullanıcı için hem ev hem de Konuk dizinleri görebilirsiniz.

> [!Important]
> Bir aboneliği farklı bir dizinle ilişkilendirdiğinizde, [rol tabanlı erişim denetimi (RBAC)](../../role-based-access-control/role-assignments-portal.md) kullanılarak atanmış rollere sahip kullanıcılar erişimleri kaybeder. Klasik abonelik yöneticileri (Hizmet Yöneticisi ve ortak yöneticiler) da erişimi kaybedecektir. 
>
> Abonelik farklı bir dizinle ilişkilendirildiğinde, ilke atamaları da bir abonelikten kaldırılır.
> 
> Ayrıca, Azure Kubernetes hizmeti (AKS) kümenizi farklı bir aboneliğe taşımak veya küme sahibi aboneliği yeni bir kiracıya taşımak, kayıp rol atamaları ve hizmet sorumluları hakları nedeniyle kümenin işlevselliği kaybetmesine neden olur. AKS hakkında daha fazla bilgi için bkz. [Azure Kubernetes Service (aks)](https://docs.microsoft.com/azure/aks/).

## <a name="before-you-begin"></a>Başlamadan önce

Aboneliğinizi ilişkilendirebilmeniz veya ekleyebilmek için önce aşağıdaki görevleri gerçekleştirmeniz gerekir:

1. Aşağıdaki değişiklik listesini ve nasıl etkilenebileceğini gözden geçirin:

    - RBAC kullanılarak roller atanmış olan kullanıcılar, erişimini kaybeder
    - Hizmet Yöneticisi ve ortak yöneticiler erişimi kaybedecektir
    - Herhangi bir Anahtar Kasası varsa, bunlar erişilemez olur ve ilişkilendirmeden sonra bunları çözmeniz gerekir
    - Sanal makineler veya Logic Apps gibi kaynaklar için yönetilen kimlikleriniz varsa, ilişkilendirmeyi sonra yeniden etkinleştirmeniz veya yeniden oluşturmanız gerekir
    - Kayıtlı bir Azure Stack varsa ilişkilendirmeden sonra yeniden kaydetmeniz gerekir

1. Şunları içeren bir hesap kullanarak oturum açın:
    - , Abonelik için bir [sahip](../../role-based-access-control/built-in-roles.md#owner) rolü atamasına sahiptir. Sahip rolünü atama hakkında daha fazla bilgi için bkz. [RBAC ve Azure Portal kullanarak Azure kaynaklarına erişimi yönetme](../../role-based-access-control/role-assignments-portal.md).
    - Hem abonelikle hem de aboneliğin ileriye doğru ilişkilendirmek istediğiniz yeni dizinde bulunan geçerli dizinde bulunur. Başka bir dizine erişim elde etme hakkında daha fazla bilgi için bkz. [Azure Active Directory YÖNETICILERI B2B işbirliği kullanıcılarını nasıl ekler?](../b2b/add-users-administrator.md).

1. Bir Azure bulut hizmeti sağlayıcıları (CSP) aboneliği (MS-AZR-0145P, MS-AZR-0146P, MS-AZR-159P), bir Microsoft Iç aboneliği (MS-AZR-0015P) veya Microsoft Imagine aboneliği (MS-AZR-0144P) kullandığınızdan emin olun.
    
## <a name="to-associate-an-existing-subscription-to-your-azure-ad-directory"></a>Var olan bir aboneliği Azure AD dizininizle ilişkilendirme

1. Oturum açın ve [Azure Portal abonelikler sayfasından](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade)kullanmak istediğiniz aboneliği seçin.

2. **Dizini Değiştir**' i seçin.

    ![Abonelik Değiştir seçeneği vurgulanmış şekilde abonelikler sayfası](media/active-directory-how-subscriptions-associated-directory/change-directory-button.png)

3. Görüntülenen tüm uyarıları gözden geçirin ve ardından **Değiştir**' i seçin.

    ![Değiştirilecek dizini gösteren Dizin sayfasını değiştirin](media/active-directory-how-subscriptions-associated-directory/edit-directory-ui.png)

    Abonelik için Dizin değiştirilir ve başarılı bir ileti alırsınız.

    ![Dizin değişikliği hakkında başarı iletisi](media/active-directory-how-subscriptions-associated-directory/edit-directory-success.png)
4. Yeni dizininize gitmek için **Dizin değiştiricisini** kullanın. Her şeyin düzgün şekilde gösterilmesi birkaç saat sürebilir. Çok uzun sürüyor görünüyorsa, gizlenen aboneliğin gizli olmadığından emin olmak için **genel abonelik filtresini** kontrol edin. Yeni dizini görebilmeniz için Azure portal oturumunuzu kapatıp yeniden açmanız gerekebilir. 

    ![Örnek bilgilerle Dizin değiştirici sayfası](media/active-directory-how-subscriptions-associated-directory/directory-switcher.png)

Abonelik dizininin değiştirilmesi, hizmet düzeyi bir işlemdir, bu nedenle Abonelik Faturalandırma sahipliğini etkilemez. Hesap Yöneticisi hala hizmet yöneticisini [Hesap merkezinden](https://account.azure.com/subscriptions)değiştirebilir. Özgün dizini silmek için, Abonelik Faturalandırma sahipliğini yeni bir hesap yöneticisine aktarmanız gerekir. Faturalama sahipliğini aktarma hakkında daha fazla bilgi edinmek için bkz. [Azure aboneliğinin sahipliğini başka bir hesaba aktarma](../../billing/billing-subscription-transfer.md).

## <a name="post-association-steps"></a>İlişkilendirme sonrası adımları
Bir aboneliği farklı bir dizine ilişkilendirdikten sonra, işlemleri sürdürmek için gerçekleştirmeniz gereken ek adımlar olabilir.

1. Herhangi bir Anahtar Kasası varsa, Anahtar Kasası kiracı KIMLIĞINI değiştirmeniz gerekir. Daha fazla bilgi için bkz. [bir abonelik taşıdıktan sonra Anahtar Kasası KIRACı kimliğini değiştirme](../../key-vault/key-vault-subscription-move-fix.md).

2. Kaynaklar için sistem tarafından atanan Yönetilen kimlikler kullanıyorsanız, bunları yeniden etkinleştirmeniz gerekir. Kullanıcı tarafından atanan Yönetilen kimlikler kullanıyorsanız, bunları yeniden oluşturmanız gerekir. Yönetilen kimlikleri yeniden etkinleştirdikten veya yeniden oluşturduktan sonra, bu kimliklere atanan izinleri yeniden oluşturmanız gerekir. Daha fazla bilgi için bkz. [Azure kaynakları için Yönetilen kimlikler nelerdir?](../managed-identities-azure-resources/overview.md).

3. Bu aboneliği kullanarak bir Azure Stack kaydolduysanız, yeniden kaydetmeniz gerekir. Daha fazla bilgi için bkz. [Azure ile Azure Stack kaydetme](/azure-stack/operator/azure-stack-registration).



## <a name="next-steps"></a>Sonraki adımlar

- Yeni bir Azure AD kiracısı oluşturmak için bkz. [erişim Azure Active Directory yeni bir kiracı oluşturmak için](active-directory-access-create-new-tenant.md)

- Microsoft Azure'da kaynak erişiminin nasıl denetlendiği konusunda daha fazla bilgi için bkz. [Azure'da kaynak erişimini anlama](../../role-based-access-control/rbac-and-directory-admin-roles.md)

- Azure AD 'de rol atama hakkında daha fazla bilgi edinmek için bkz. [Azure Active Directory sahip kullanıcılara dizin rolleri atama](active-directory-users-assign-role-azure-portal.md)
