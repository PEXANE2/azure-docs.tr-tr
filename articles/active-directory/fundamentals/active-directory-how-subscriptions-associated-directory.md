---
title: Kiracınıza varolan bir Azure aboneliği ekleme - Azure AD
description: Azure Etkin Dizin kiracınıza varolan bir Azure aboneliğini nasıl ekleyeceğinize ilişkin yönergeler.
services: active-directory
author: msaburnley
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 10/25/2019
ms.author: ajburnle
ms.reviewer: jeffsta
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 104bf51fb03d88ab0e5efd25ebebb0e3060bc264
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81457935"
---
# <a name="associate-or-add-an-azure-subscription-to-your-azure-active-directory-tenant"></a>Azure Active Directory kiracınıza bir Azure aboneliğini ekleme veya ilişkilendirme

Azure aboneliğinin Azure Etkin Dizin (Azure AD) ile güven ilişkisi vardır. Bir abonelik, kullanıcıların, hizmetlerin ve aygıtların kimliğini doğrulamak için Azure AD'ye güvenir.

Birden çok abonelik aynı Azure REKLAM dizinine güvenebilir. Her abonelik yalnızca tek bir dizine güvenebilir.

Aboneliğinizin süresi dolduğunda, abonelikle ilişkili diğer tüm kaynaklara erişiminizi kaybedersiniz. Ancak, Azure REKLAM dizini Azure'da kalır. Farklı bir Azure aboneliği kullanarak dizini ilişkilendirebilir ve yönetebilirsiniz.

Tüm kullanıcılarınızın kimlik doğrulaması için tek bir *ev* dizini vardır. Kullanıcılarınız diğer dizinlerde de konuk olabilir. Azure AD'de her kullanıcının hem ev hem de konuk dizinlerini görebilirsiniz.

> [!Important]
> Bir aboneliği farklı bir dizinle ilişkilendirdiğinizde, [rol tabanlı erişim denetimi (RBAC)](../../role-based-access-control/role-assignments-portal.md) kullanılarak atanan rolleri olan kullanıcılar erişimlerini kaybeder. Hizmet Yöneticisi ve Ortak Yöneticiler dahil olmak üzere klasik abonelik yöneticileri de erişimi kaybeder.
>
> Abonelik farklı bir dizinle ilişkilendirildiğinde İlke Atamaları da abonelikten kaldırılır.
>
> Azure Kubernetes Hizmeti (AKS) kümenizi farklı bir aboneliğe taşımak veya küme sahibi aboneliğini yeni bir kiracıya taşımak, kümenin rol atamaları ve hizmet sorumlusunun hakları nedeniyle işlevselliğini kaybetmesine neden olur. AKS hakkında daha fazla bilgi için [Azure Kubernetes Hizmeti (AKS)](https://docs.microsoft.com/azure/aks/)'ye bakın.


## <a name="before-you-begin"></a>Başlamadan önce

Aboneliğinizi ilişkilendirmeden veya eklemeden önce aşağıdaki görevleri yapın:

- Aşağıdaki değişiklik listesini ve nasıl etkileneceğiniince gözden geçirin:

  - RBAC kullanarak roller atanan kullanıcılar erişimlerini kaybeder
  - Hizmet Yöneticisi ve Yardımcı Yöneticiler erişimi kaybeder
  - Eğer herhangi bir anahtar kasanız varsa, erişilemezler ve ilişkilendirmeden sonra bunları düzeltmek zorunda kaldığınız
  - Sanal Makineler veya Logic Apps gibi kaynaklar için yönetilen kimlikleriniz varsa, ilişkilendirmeden sonra bunları yeniden etkinleştirmeniz veya yeniden oluşturmanız gerekir
  - Kayıtlı bir Azure Yığınınız varsa, ilişkilendirmeden sonra yeniden kaydetmeniz gerekir

- Şu bir hesabı kullanarak oturum açın:

  - Abonelik için [Bir Sahip](../../role-based-access-control/built-in-roles.md#owner) rol ataması vardır. Sahibi rolünü nasıl atayacağım hakkında bilgi için [RBAC ve Azure portalını kullanarak Azure kaynaklarına erişimi yönet'e](../../role-based-access-control/role-assignments-portal.md)bakın.
  - Hem geçerli dizinde hem de yeni dizinde bulunur. Geçerli dizin abonelikle ilişkilidir. Yeni dizini abonelikle ilişkilendireceksiniz. Başka bir dizine erişim hakkında daha fazla bilgi için azure [portalındaki Azure Etkin Dizin B2B işbirliği kullanıcıları ekle'ye](../b2b/add-users-administrator.md)bakın.

- Azure Bulut Hizmet Sağlayıcıları (CSP) aboneliği (MS-AZR-0145P, MS-AZR-0146P, MS-AZR-159P), Microsoft Dahili aboneliği (MS-AZR-0015P) veya Microsoft Imagine aboneliği (MS-AZR-0144P) kullanmadığınızdan emin olun.

## <a name="associate-a-subscription-to-a-directory"></a>Aboneliği bir dizine ilişkilendirme<a name="to-associate-an-existing-subscription-to-your-azure-ad-directory"></a>

Varolan bir aboneliği Azure REKLAM dizininize ilişkilendirmek için aşağıdaki adımları izleyin:

1. Azure [portalındaki Abonelikler sayfasından](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade)oturum açın ve kullanmak istediğiniz aboneliği seçin.

1. **Dizini değiştir'i**seçin.

    ![Abonelikler sayfası, Dizin değiştir seçeneği vurgulanır](media/active-directory-how-subscriptions-associated-directory/change-directory-in-azure-subscriptions.png)

1. Görünen uyarıları gözden geçirin ve sonra **Değiştir'i**seçin.

    ![Dizin sayfasını değiştirerek dizin](media/active-directory-how-subscriptions-associated-directory/edit-directory-ui.png)

    Abonelik için dizin değiştirilir ve bir başarı iletisi alırsınız.

    ![Dizin değişikliği ile ilgili başarı iletisi](media/active-directory-how-subscriptions-associated-directory/edit-directory-success.png)

Yeni dizine gitmek için **Switch dizinini** kullanın. Her şeyin düzgün bir şekilde ortaya olması birkaç saat sürebilir. Çok uzun sürüyor gibi görünüyorsa, **Global abonelik filtresini**kontrol edin. Taşınan aboneliğin gizli olmadığından emin olun. Yeni dizini görmek için Azure portalından oturum açmanız ve yeniden oturum açmanız gerekebilir.

![Örnek bilgilerle dizin anahtarlayıcı sayfası](media/active-directory-how-subscriptions-associated-directory/directory-switcher.png)

Abonelik dizinini değiştirmek hizmet düzeyinde bir işlemdir, bu nedenle abonelik fatura sahipliğini etkilemez. Hesap Yöneticisi, Hizmet Yöneticisi'ni [Hesap Merkezi'nden](https://account.azure.com/subscriptions)değiştirmeye devam edebilir. Özgün dizini silmek için abonelik fatura sahipliğini yeni bir Hesap Yöneticisi'ne aktarmanız gerekir. Fatura sahipliğini aktarma hakkında daha fazla bilgi edinmek için azure [aboneliğinin sahipliğini başka bir hesaba aktar' bölümüne](../../cost-management-billing/manage/billing-subscription-transfer.md)bakın.

## <a name="post-association-steps"></a>İlişkilendirme sonrası adımlar

Bir aboneliği farklı bir dizine ilişkilendirdikten sonra, işlemleri sürdürmek için aşağıdaki görevleri yapmanız gerekebilir:

- Anahtar kasanız varsa, anahtar kasa kiracı kimliğini değiştirmeniz gerekir. Daha fazla bilgi için [bkz.](../../key-vault/general/subscription-move-fix.md)

- Kaynaklar için sistem tarafından atanan Yönetilen Kimlikler kullandıysanız, bu kimlikleri yeniden etkinleştirmeniz gerekir. Kullanıcı tarafından atanan Yönetilen Kimlikler kullandıysanız, bu kimlikleri yeniden oluşturmanız gerekir. Yönetilen Kimlikler'i yeniden etkinleştirdikten veya yeniden oluşturduktan sonra, bu kimliklere atanan izinleri yeniden oluşturmanız gerekir. Daha fazla bilgi için azure [kaynakları için yönetilen kimlikler nedir?](../managed-identities-azure-resources/overview.md)

- Bu aboneliği kullanarak bir Azure Yığını kaydettiyseniz, yeniden kaydolmanız gerekir. Daha fazla bilgi için azure [yığınını kaydedin.](/azure-stack/operator/azure-stack-registration)

## <a name="next-steps"></a>Sonraki adımlar

- Yeni bir Azure AD kiracısı oluşturmak için Bkz. [Hızlı Başlangıç: Azure Etkin Dizini'nde yeni bir kiracı oluşturun.](active-directory-access-create-new-tenant.md)

- Microsoft Azure'un kaynak erişimini nasıl denetlettiği hakkında daha fazla bilgi edinmek için [Klasik abonelik yöneticisi rolleri, Azure RBAC rolleri ve Azure REKLAM yöneticisi rolleri'ne](../../role-based-access-control/rbac-and-directory-admin-roles.md)bakın.

- Azure AD'de rollerin nasıl atayılabildiğini öğrenmek için azure etkin dizinine sahip [kullanıcılara yönetici ve yönetici olmayan roller atama](active-directory-users-assign-role-azure-portal.md)bilgisini edinin.
