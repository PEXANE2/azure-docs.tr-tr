---
title: Kiracınıza mevcut bir Azure aboneliği ekleme-Azure AD
description: Azure Active Directory kiracınıza mevcut bir Azure aboneliğinin nasıl ekleneceği hakkında yönergeler.
services: active-directory
author: msaburnley
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 06/01/2020
ms.author: ajburnle
ms.reviewer: jeffsta
ms.custom: it-pro, seodec18, contperfq4
ms.collection: M365-identity-device-management
ms.openlocfilehash: 23823a3e358706e6455410f073d87a07f9f61650
ms.sourcegitcommit: f57fa5f3ce40647eda93f8be4b0ab0726d479bca
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2020
ms.locfileid: "84483459"
---
# <a name="associate-or-add-an-azure-subscription-to-your-azure-active-directory-tenant"></a>Azure Active Directory kiracınıza bir Azure aboneliğini ekleme veya ilişkilendirme

Azure aboneliğinin Azure Active Directory (Azure AD) ile bir güven ilişkisi vardır. Abonelik, kullanıcıların, hizmetlerin ve cihazların kimliğini doğrulamak için Azure AD 'ye güvenir.

Birden çok abonelik aynı Azure AD dizinine güvenebilir. Her abonelik yalnızca tek bir dizine güvenebilir.

Aboneliğinizin süresi dolarsa, abonelikle ilişkili diğer tüm kaynaklara erişiminizi kaybedersiniz. Ancak Azure AD dizini Azure 'da kalır. Farklı bir Azure aboneliği kullanarak dizini ilişkilendirebilir ve yönetebilirsiniz.

Tüm kullanıcılarınızın kimlik doğrulaması için tek bir *giriş* dizini vardır. Kullanıcılarınız diğer dizinlerde da konuk olabilir. Azure AD 'de her bir kullanıcı için hem ev hem de Konuk dizinleri görebilirsiniz.

> [!Important]
> Bir aboneliği farklı bir dizinle ilişkilendirdiğinizde, [rol tabanlı erişim denetimi (RBAC)](../../role-based-access-control/role-assignments-portal.md) kullanılarak atanmış rollere sahip kullanıcılar erişimleri kaybeder. Hizmet Yöneticisi ve Ortak Yöneticiler dahil olmak üzere klasik abonelik yöneticileri de erişimi kaybeder.
>
> Abonelik farklı bir dizinle ilişkilendirildiğinde İlke Atamaları da abonelikten kaldırılır.
>
> Azure Kubernetes Service (AKS) kümenizi farklı bir aboneliğe taşımak veya küme sahibi aboneliği yeni bir kiracıya taşımak, kayıp rol atamaları ve hizmet sorumlusunun hakları nedeniyle kümenin işlevselliği kaybetmesine neden olur. AKS hakkında daha fazla bilgi için bkz. [Azure Kubernetes Service (aks)](https://docs.microsoft.com/azure/aks/).


## <a name="before-you-begin"></a>Başlamadan önce

Aboneliğinizi ilişkilendirebilmeniz veya ekleyebilmek için önce aşağıdaki görevleri yapın:

- Aboneliğinizi ilişkilendirdikten veya ekledikten sonra gerçekleşecek ve nasıl etkilenebileceğiniz aşağıdaki değişiklikler listesini gözden geçirin:

  - RBAC kullanılarak roller atanmış olan kullanıcılar, erişimini kaybeder
  - Hizmet Yöneticisi ve ortak yöneticiler erişimi kaybedecektir
  - Herhangi bir Anahtar Kasası varsa, bunlar erişilemez olur ve ilişkilendirmeden sonra bunları çözmeniz gerekir
  - Sanal makineler veya Logic Apps gibi kaynaklar için yönetilen kimlikleriniz varsa, ilişkilendirmeden sonra yeniden etkinleştirmeniz veya yeniden oluşturmanız gerekir
  - Kayıtlı bir Azure Stack varsa ilişkilendirmeden sonra yeniden kaydetmeniz gerekir

- Şunları içeren bir hesap kullanarak oturum açın:

  - , Abonelik için bir [sahip](../../role-based-access-control/built-in-roles.md#owner) rolü atamasına sahiptir. Sahip rolünü atama hakkında daha fazla bilgi için bkz. [RBAC ve Azure Portal kullanarak Azure kaynaklarına erişimi yönetme](../../role-based-access-control/role-assignments-portal.md).
  - Hem geçerli dizinde hem de yeni dizinde bulunur. Geçerli dizin abonelikle ilişkili. Yeni dizini abonelikle ilişkilendireceğiz. Başka bir dizine erişim elde etme hakkında daha fazla bilgi için, [Azure portal Azure ACTIVE DIRECTORY B2B işbirliği kullanıcıları ekleme](../b2b/add-users-administrator.md)bölümüne bakın.

- Bir Azure bulut hizmeti sağlayıcıları (CSP) aboneliği (MS-AZR-0145P, MS-AZR-0146P, MS-AZR-159P), bir Microsoft Iç aboneliği (MS-AZR-0015P) veya Microsoft Imagine aboneliği (MS-AZR-0144P) kullandığınızdan emin olun.

## <a name="associate-a-subscription-to-a-directory"></a>Aboneliği bir dizinle ilişkilendir<a name="to-associate-an-existing-subscription-to-your-azure-ad-directory"></a>

Mevcut bir aboneliği Azure AD dizininizle ilişkilendirmek için şu adımları izleyin:

1. Oturum açın ve [Azure Portal abonelikler sayfasından](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade)kullanmak istediğiniz aboneliği seçin.

1. **Dizini Değiştir**' i seçin.

    ![Abonelik Değiştir seçeneği vurgulanmış şekilde abonelikler sayfası](media/active-directory-how-subscriptions-associated-directory/change-directory-in-azure-subscriptions.png)

1. Görüntülenen tüm uyarıları gözden geçirin ve ardından **Değiştir**' i seçin.

    ![Değiştirilecek dizini gösteren Dizin sayfasını değiştirin](media/active-directory-how-subscriptions-associated-directory/edit-directory-ui.png)

    Abonelik için Dizin değiştirildikten sonra, başarılı bir ileti alırsınız.

  1. Yeni dizininize gitmek için abonelik sayfasında **dizinleri Değiştir** ' i seçin. 

      ![Örnek bilgilerle Dizin değiştirici sayfası](media/active-directory-how-subscriptions-associated-directory/directory-switcher.png)

      Her şeyin düzgün şekilde gösterilmesi birkaç saat sürebilir. Çok uzun sürüyor görünüyorsa **genel abonelik filtresini**kontrol edin. Taşınan aboneliğin gizlenmediğinden emin olun. Yeni dizini görmek için Azure portal oturumunuzu kapatıp yeniden açmanız gerekebilir.

Abonelik dizininin değiştirilmesi, hizmet düzeyi bir işlemdir, bu nedenle Abonelik Faturalandırma sahipliğini etkilemez. Hesap Yöneticisi hala hizmet yöneticisini [Hesap merkezinden](https://account.azure.com/subscriptions)değiştirebilir. Özgün dizini silmek için, Abonelik Faturalandırma sahipliğini yeni bir hesap yöneticisine aktarmanız gerekir. Faturalama sahipliğini aktarma hakkında daha fazla bilgi edinmek için bkz. [Azure aboneliğinin sahipliğini başka bir hesaba aktarma](../../cost-management-billing/manage/billing-subscription-transfer.md).

## <a name="post-association-steps"></a>İlişkilendirme sonrası adımları

Bir aboneliği farklı bir dizine ilişkilendirdikten sonra, işlemleri sürdürmek için aşağıdaki görevleri gerçekleştirmeniz gerekebilir:

- Herhangi bir Anahtar Kasası varsa, Anahtar Kasası kiracı KIMLIĞINI değiştirmeniz gerekir. Daha fazla bilgi için bkz. [bir abonelik taşıdıktan sonra Anahtar Kasası KIRACı kimliğini değiştirme](../../key-vault/general/subscription-move-fix.md).

- Kaynaklar için sistem tarafından atanan Yönetilen kimlikler kullandıysanız, bu kimlikleri yeniden etkinleştirmeniz gerekir. Kullanıcı tarafından atanan Yönetilen kimlikler kullandıysanız, bu kimlikleri yeniden oluşturmanız gerekir. Yönetilen kimlikleri yeniden etkinleştirdikten veya yeniden oluşturduktan sonra, bu kimliklere atanan izinleri yeniden oluşturmanız gerekir. Daha fazla bilgi için bkz. [Azure kaynakları için Yönetilen kimlikler nelerdir?](../managed-identities-azure-resources/overview.md).

- Bu aboneliği kullanarak bir Azure Stack kaydolduysanız, yeniden kaydetmeniz gerekir. Daha fazla bilgi için bkz. [Azure ile Azure Stack kaydetme](/azure-stack/operator/azure-stack-registration).

## <a name="next-steps"></a>Sonraki adımlar

- Yeni bir Azure AD kiracısı oluşturmak için bkz. [hızlı başlangıç: Azure Active Directory yeni kiracı oluşturma](active-directory-access-create-new-tenant.md).

- Microsoft Azure kaynak erişimini nasıl denetlediği hakkında daha fazla bilgi edinmek için bkz. [Klasik abonelik yöneticisi rolleri, Azure RBAC rolleri ve Azure AD yönetici rolleri](../../role-based-access-control/rbac-and-directory-admin-roles.md).

- Azure AD 'de rol atama hakkında daha fazla bilgi için, bkz. [Azure Active Directory kullanıcılara yönetici ve yönetici olmayan roller atama](active-directory-users-assign-role-azure-portal.md).
