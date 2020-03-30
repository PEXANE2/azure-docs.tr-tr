---
title: Azure klasik abonelik yöneticileri | Microsoft Dokümanlar
description: Azure Yardımcı Yönetici ve Hizmet Yöneticisi rollerinin nasıl ekleyeceğiniz veya değiştirilen ve Hesap Yöneticisi'ni nasıl görüntüleyeceğiniaçık.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: ''
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/22/2020
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 2493e893f9afda0642bd838c94538dd0b984bce5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79243490"
---
# <a name="azure-classic-subscription-administrators"></a>Azure klasik abonelik yöneticileri

Microsoft, rol tabanlı erişim denetimi (RBAC) kullanarak Azure kaynaklarına erişimi yönetmenizi önerir. Ancak, klasik dağıtım modelini kullanmaya devam ediyorsanız, klasik bir abonelik yöneticisi rolü kullanmanız gerekir: Hizmet Yöneticisi ve Yardımcı Yönetici. Daha fazla bilgi için Azure [Kaynak Yöneticisi ve klasik dağıtım a.ş.'ye](../azure-resource-manager/management/deployment-models.md)bakın.

Bu makalede, Ortak Yönetici ve Hizmet Yöneticisi rollerinin nasıl ekleyeceğiniz veya değiştirilen ve Hesap Yöneticisi'nin nasıl görüntüleneniz açıklanmaktadır.

## <a name="add-a-co-administrator"></a>Ortak Yönetici ekleme

> [!TIP]
> Yalnızca kullanıcının [Azure Hizmet Yönetimi PowerShell Modülü'ni](https://docs.microsoft.com/powershell/module/servicemanagement/azure)kullanarak Azure klasik dağıtımlarını yönetmesi gerekiyorsa bir Yardımcı Yönetici eklemeniz gerekir. Kullanıcı yalnızca klasik kaynakları yönetmek için Azure portalını kullanıyorsa, kullanıcı için klasik yöneticiyi eklemeniz gerekmez.

1. [Azure portalında](https://portal.azure.com) Hizmet Yöneticisi veya Yardımcı Yönetici olarak oturum açın.

1. [Abonelikler](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade)'i açın ve bir abonelik seçin.

    Yardımcı Yöneticiler yalnızca abonelik kapsamında atanabilir.

1. **Erişim denetimi (IAM)** öğesine tıklayın.

1. Klasik **yöneticiler** sekmesini tıklatın.

    ![Klasik yöneticileri açan ekran görüntüsü](./media/classic-administrators/classic-administrators.png)

1. Ortak yönetici Ekle bölmesini açmak için**ekle ortak yöneticisini** **ekle'yi** > tıklatın.

    Ortak yönetici ekle seçeneği devre dışı bırakılırsa, izinleriniz yoktur.

1. Eklemek istediğiniz kullanıcıyı seçin ve **Ekle'yi**tıklatın.

    ![Yardımcı yöneticiyi ekleyen ekran görüntüsü](./media/classic-administrators/add-coadmin.png)

## <a name="add-a-guest-user-as-a-co-administrator"></a>Yardımcı Yönetici olarak konuk kullanıcı ekleme

Yardımcı Yönetici olarak konuk kullanıcı eklemek için, önceki Ortak [Yönetici Ekle](#add-a-co-administrator) bölümündeki adımları izleyin. Konuk kullanıcı aşağıdaki kriterleri karşılamalıdır:

- Konuk kullanıcının dizininizde bir varlığı olmalıdır. Bu, kullanıcının dizininize davet edildiği ve daveti kabul ettiği anlamına gelir.

Dizininize konuk kullanıcı ekleme hakkında daha fazla bilgi için Azure [portalındaki Azure Etkin Dizin B2B işbirliği kullanıcıları ekle'ye](../active-directory/b2b/add-users-administrator.md)bakın.

### <a name="differences-for-guest-users"></a>Konuk kullanıcılar için farklılıklar

Eş Yönetici rolüne atanan konuk kullanıcılar, Co-Administrator rolüne sahip üye kullanıcılarla karşılaştırıldığında bazı farklılıklar görebilir. Şu senaryoyu göz önünde bulundurun:

- Azure AD hesabı (iş veya okul hesabı) olan Kullanıcı A, Azure aboneliği için bir Hizmet Yöneticisidir.
- Kullanıcı B'nin bir Microsoft hesabı vardır.
- Kullanıcı A, Ortak Yönetici rolünü kullanıcı B'ye atar.
- B kullanıcısı hemen hemen her şeyi yapabilir, ancak uygulamaları kaydedemiyor veya Azure AD dizinindeki kullanıcıları aramıyor.

B kullanıcısının her şeyi yönetebileceğini beklersiniz. Bu farkın nedeni, Microsoft hesabının abone kullanıcı yerine konuk kullanıcı olarak aboneye eklenmesidir. Konuk kullanıcılar, Azure AD'de üye kullanıcılarla karşılaştırıldığında farklı varsayılan izinlere sahiptir. Örneğin, üye kullanıcılar Azure AD'deki diğer kullanıcıları okuyabilir ve konuk kullanıcılar okuyamaz. Üye kullanıcılar Azure AD'ye yeni hizmet ilkelerini kaydedebilir ve konuk kullanıcılar kaydedemez.

Konuk kullanıcının bu görevleri gerçekleştirebilmesi gerekiyorsa, olası bir çözüm konuk kullanıcının ihtiyaç duyduğu belirli Azure AD yöneticisi rollerini atamaktır. Örneğin, önceki senaryoda, diğer kullanıcıları okumak için [Dizin Okuyucular](../active-directory/users-groups-roles/directory-assign-admin-roles.md#directory-readers) rolünü atayabilir ve hizmet ilkeleri oluşturabilmek için [Uygulama Geliştiricisi](../active-directory/users-groups-roles/directory-assign-admin-roles.md#application-developer) rolünü atayabilirsiniz. Üye ve konuk kullanıcılar ve izinleri hakkında daha fazla bilgi için Azure [Etkin Dizini'nde varsayılan kullanıcı izinleri nelerdir?](../active-directory/fundamentals/users-default-permissions.md) Konuk kullanıcılara erişim izni verme hakkında daha fazla bilgi için [bkz.](role-assignments-external-users.md)

[Azure kaynakları için yerleşik rollerin](../role-based-access-control/built-in-roles.md) Azure AD yöneticisi [rollerinden](../active-directory/users-groups-roles/directory-assign-admin-roles.md)farklı olduğunu unutmayın. Yerleşik roller Azure AD'ye erişim izni vermez. Daha fazla bilgi için [bkz.](../role-based-access-control/rbac-and-directory-admin-roles.md)

Üye kullanıcılar la konuk kullanıcıları karşılaştıran bilgiler için Azure [Etkin Dizini'nde varsayılan kullanıcı izinleri nelerdir?](../active-directory/fundamentals/users-default-permissions.md)

## <a name="remove-a-co-administrator"></a>Yardımcı Yöneticiyi Kaldırma

1. [Azure portalında](https://portal.azure.com) Hizmet Yöneticisi veya Yardımcı Yönetici olarak oturum açın.

1. [Abonelikler](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade)'i açın ve bir abonelik seçin.

1. **Erişim denetimi (IAM)** öğesine tıklayın.

1. Klasik **yöneticiler** sekmesini tıklatın.

1. Kaldırmak istediğiniz Yardımcı Yöneticinin yanına bir onay işareti ekleyin.

1. **Kaldır**’a tıklayın.

1. Görünen ileti kutusunda **Evet'i**tıklatın.

    ![Yardımcı yöneticiyi kaldıran ekran görüntüsü](./media/classic-administrators/remove-coadmin.png)

## <a name="change-the-service-administrator"></a>Hizmet Yöneticisini değiştirme

Hizmet Yöneticisi'ni bir abonelik için yalnızca Hesap Yöneticisi değiştirebilir. Varsayılan olarak, bir Azure aboneliğine kaydolduğunuzda, Hizmet Yöneticisi Hesap Yöneticisi ile aynıdır. Hesap Yöneticisi rolüne sahip kullanıcının Azure portalına erişimi yoktur. Hizmet Yöneticisi rolüne sahip kullanıcı, Azure portalına tam erişime sahiptir. Hesap Yöneticisi ve Hizmet Yöneticisi aynı kullanıcıysa ve Hizmet Yöneticisi'ni farklı bir kullanıcıyla değiştirirseniz, Hesap Yöneticisi Azure portalına erişimi kaybeder. Ancak, Hesap Yöneticisi, Hizmet Yöneticisi'ni kendilerine değiştirmek için Hesap Merkezi'ni her zaman kullanabilir.

**Hesap Merkezi'ndeki**Servis Yöneticisini değiştirmek için aşağıdaki adımları izleyin.

### <a name="account-center"></a>Hesap Merkezi

1. [Hizmet Yöneticisi'ni değiştirme sınırlamalarını](#limitations-for-changing-the-service-administrator)denetleyerek senaryonuzun desteklendirdiğinden emin olun.

1. Hesap Yöneticisi olarak [Hesap Merkezi'nde](https://account.windowsazure.com/subscriptions) oturum açın.

1. Bir abonelik'i tıklatın.

1. Sağ tarafta, **abonelik ayrıntılarını edit'i**tıklatın.

    ![Hesap Merkezi'nde abonelik düğmesini göstererek ekran görüntüsü](./media/classic-administrators/editsub.png)

1. SERVICE **ADMINISTRATOR** kutusuna yeni Hizmet Yöneticisi'nin e-posta adresini girin.

    ![Hizmet Yöneticisi e-postasını değiştirmek için kutuyu gösteren ekran görüntüsü](./media/classic-administrators/change-service-admin.png)

1. Değişikliği kaydetmek için onay işaretini tıklatın.

### <a name="limitations-for-changing-the-service-administrator"></a>Hizmet Yöneticisini değiştirme sınırlamaları

Azure aboneliği başına yalnızca bir Hizmet Yöneticisi olabilir. Hizmet Yöneticisi'ni değiştirmek, Hesap Yöneticisi'nin Microsoft hesabı olmasına veya Azure REKLAM hesabı (iş veya okul hesabı) olmasına bağlı olarak farklı davranacaktır.

| Hesap Yöneticisi hesabı | Hizmet Yöneticisi'ni farklı bir Microsoft hesabıyla değiştirebilir misiniz? | Hizmet Yöneticisi'ni aynı dizindeki bir Azure REKLAM hesabıyla değiştirebilir misiniz? | Hizmet Yöneticisini farklı bir dizindeki bir Azure REKLAM hesabıyla değiştirebilir misiniz? |
| --- | --- | --- | --- |
| Microsoft hesabı | Evet | Hayır | Hayır |
| Azure AD hesabı | Evet | Evet | Hayır |

Hesap Yöneticisi bir Azure AD hesabıysa, Hizmet Yöneticisi'ni aynı dizinde bir Azure REKLAM hesabı olarak değiştirebilirsiniz, ancak farklı bir dizinde değiştiremezsiniz. Örneğin, abby@contoso.com Hizmet Yöneticisi'ni bob@contoso.com,, ancak contoso.com dizininde bir varlık john@notcontoso.com olmadığı sürece john@notcontoso.com Hizmet Yöneticisi'ni değiştiremez.

Microsoft hesapları ve Azure REKLAM hesapları hakkında daha fazla bilgi için Azure [Etkin Dizin nedir?](../active-directory/fundamentals/active-directory-whatis.md)

## <a name="view-the-account-administrator"></a>Hesap Yöneticisini görüntüleme

Hesap Yöneticisi, başlangıçta Azure aboneliğine kaydolan kullanıcıdır ve aboneliğin fatura sahibi olarak sorumludur. Bir aboneliğin Hesap Yöneticisini değiştirmek [için](../cost-management-billing/manage/billing-subscription-transfer.md)bkz.

Hesap Yöneticisi'ni görüntülemek için aşağıdaki adımları izleyin.

1. [Azure portalında](https://portal.azure.com)oturum açın.

1. [Abonelikler](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade)'i açın ve bir abonelik seçin.

1. **Özellikler**'e tıklayın.

    Aboneliğin Hesap Yöneticisi **Hesap Yöneticisi** kutusunda görüntülenir.

    ![Hesap Yöneticisini gösteren ekran görüntüsü](./media/classic-administrators/account-admin.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure'daki farklı rolleri anlama](../role-based-access-control/rbac-and-directory-admin-roles.md)
* [RBAC ve Azure portalını kullanarak Azure kaynaklarına erişimi yönetme](../role-based-access-control/role-assignments-portal.md)
* [Azure aboneliği yöneticileri ekleme veya değiştirme](../cost-management-billing/manage/add-change-subscription-administrator.md)
