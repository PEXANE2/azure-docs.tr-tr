---
title: Azure klasik abonelik yöneticileri
description: Azure ortak yönetici ve hizmet yöneticisi rollerinin nasıl ekleneceğini ve değiştirileceğini ve hesap yöneticisinin nasıl görüntüleneceğini açıklar.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: ''
ms.service: role-based-access-control
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/22/2020
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 298ec15700537f2368ef33ead267e85f5f22bfd7
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84791801"
---
# <a name="azure-classic-subscription-administrators"></a>Azure klasik abonelik yöneticileri

Microsoft, Azure rol tabanlı erişim denetimi (Azure RBAC) kullanarak Azure kaynaklarına erişimi yönetmenizi önerir. Ancak yine de klasik dağıtım modelini kullanıyorsanız, klasik bir abonelik yönetici rolü kullanmanız gerekir: Hizmet Yöneticisi ve ortak yönetici. Daha fazla bilgi için bkz. [Azure Resource Manager ile klasik dağıtım](../azure-resource-manager/management/deployment-models.md).

Bu makalede, ortak yönetici ve hizmet yöneticisi rollerinin nasıl ekleneceği ve değiştirileceği ve hesap yöneticisinin nasıl görüntüleneceği açıklanır.

## <a name="add-a-co-administrator"></a>Ortak Yönetici ekleme

> [!TIP]
> Yalnızca kullanıcının [Azure hizmet yönetimi PowerShell modülünü](https://docs.microsoft.com/powershell/module/servicemanagement/azure)kullanarak klasik Azure dağıtımlarını yönetmesi gerekiyorsa bir ortak yönetici eklemeniz gerekir. Kullanıcı yalnızca klasik kaynakları yönetmek için Azure portal kullanıyorsa, Kullanıcı için klasik yönetici eklemeniz gerekmez.

1. [Azure Portal](https://portal.azure.com) bir hizmet yöneticisi veya ortak yönetici olarak oturum açın.

1. [Abonelikler](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade)'i açın ve bir abonelik seçin.

    Ortak Yöneticiler yalnızca abonelik kapsamında atanabilir.

1. **Erişim denetimi (IAM)** öğesine tıklayın.

1. **Klasik Yöneticiler** sekmesine tıklayın.

    ![Klasik yöneticileri açan ekran görüntüsü](./media/classic-administrators/classic-administrators.png)

1. Ortak **Add**  >  yönetici Ekle bölmesini açmak için**ortak yönetici Ekle** ' ye tıklayın.

    Ortak yönetici Ekle seçeneği devre dışıysa, izinleriniz yoktur.

1. Eklemek istediğiniz kullanıcıyı seçin ve **Ekle**' ye tıklayın.

    ![Ortak yönetici ekleyen ekran görüntüsü](./media/classic-administrators/add-coadmin.png)

## <a name="add-a-guest-user-as-a-co-administrator"></a>Konuk kullanıcıyı ortak yönetici olarak ekleme

Konuk kullanıcıyı ortak yönetici olarak eklemek için, önceki [ortak yönetici ekleme](#add-a-co-administrator) bölümünde ile aynı adımları izleyin. Konuk kullanıcının aşağıdaki ölçütlere uyması gerekir:

- Konuk kullanıcının dizininizde bir varlık olması gerekir. Bu, kullanıcının dizininize davet edildiği ve daveti kabul ettiği anlamına gelir.

Dizininize Konuk Kullanıcı ekleme hakkında daha fazla bilgi için, bkz. [Azure portal Azure ACTIVE DIRECTORY B2B işbirliği kullanıcıları ekleme](../active-directory/b2b/add-users-administrator.md).

### <a name="differences-for-guest-users"></a>Konuk kullanıcılar için farklılıklar

Ortak yönetici rolüne atanan Konuk kullanıcılar, ortak yönetici rolüne sahip üye kullanıcılarla karşılaştırıldığında bazı farklılıklar görebilirler. Şu senaryoyu göz önünde bulundurun:

- Bir Azure AD hesabı (iş veya okul hesabı) olan A kullanıcısı, bir Azure aboneliği için hizmet yöneticisidir.
- B kullanıcısının bir Microsoft hesabı vardır.
- A kullanıcısı B kullanıcısına ortak yönetici rolünü atar.
- B kullanıcısı neredeyse her şeyi yapabilir, ancak Azure AD dizininde uygulama kaydedemez veya kullanıcıları arayabilir.

B kullanıcısının her şeyi yönetebileceğini beklemeniz gerekir. Bu farkın nedeni, Microsoft hesabı aboneliğe üye kullanıcı yerine Konuk Kullanıcı olarak eklendiğine yöneliktir. Konuk kullanıcıların Azure AD 'de üye kullanıcılarla karşılaştırıldığında farklı varsayılan izinleri vardır. Örneğin, üye kullanıcılar Azure AD 'de diğer kullanıcıları okuyabilir ve Konuk kullanıcılar tarafından kullanılamaz. Üye kullanıcılar, Azure AD 'de yeni hizmet sorumlularını kaydedebilir ve Konuk kullanıcılar bu hizmetleri kullanamaz.

Konuk kullanıcının bu görevleri gerçekleştirebilmesi gerekiyorsa, Konuk kullanıcının ihtiyacı olan belirli Azure AD rollerinin atanması olası bir çözümdür. Örneğin, önceki senaryoda, diğer kullanıcıları okumak için [Dizin okuyucuları](../active-directory/users-groups-roles/directory-assign-admin-roles.md#directory-readers) rolünü atayabilir ve hizmet sorumlularını oluşturabilmeniz Için [uygulama geliştirici](../active-directory/users-groups-roles/directory-assign-admin-roles.md#application-developer) rolünü atayabilirsiniz. Üye ve Konuk kullanıcılar ve bunların izinleri hakkında daha fazla bilgi için bkz. [Azure Active Directory varsayılan kullanıcı Izinleri nelerdir?](../active-directory/fundamentals/users-default-permissions.md). Konuk kullanıcılar için erişim verme hakkında daha fazla bilgi için, bkz. [Azure Portal kullanarak dış Konuk kullanıcılar Için Azure rol atamaları ekleme veya kaldırma](role-assignments-external-users.md).

[Azure yerleşik rollerinin](../role-based-access-control/built-in-roles.md) [Azure AD rollerinden](../active-directory/users-groups-roles/directory-assign-admin-roles.md)farklı olduğunu unutmayın. Yerleşik roller Azure AD 'ye hiçbir erişim vermez. Daha fazla bilgi için bkz. [farklı rolleri anlama](../role-based-access-control/rbac-and-directory-admin-roles.md).

Üye kullanıcıları ve konuk kullanıcıları karşılaştıran bilgiler için, bkz. [Azure Active Directory varsayılan kullanıcı Izinleri nelerdir?](../active-directory/fundamentals/users-default-permissions.md).

## <a name="remove-a-co-administrator"></a>Ortak yöneticiyi kaldırma

1. [Azure Portal](https://portal.azure.com) bir hizmet yöneticisi veya ortak yönetici olarak oturum açın.

1. [Abonelikler](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade)'i açın ve bir abonelik seçin.

1. **Erişim denetimi (IAM)** öğesine tıklayın.

1. **Klasik Yöneticiler** sekmesine tıklayın.

1. Kaldırmak istediğiniz ortak yöneticinin yanına onay işareti ekleyin.

1. **Kaldır**’a tıklayın.

1. Görüntülenen ileti kutusunda **Evet**' e tıklayın.

    ![Ortak yöneticiyi kaldıran ekran görüntüsü](./media/classic-administrators/remove-coadmin.png)

## <a name="change-the-service-administrator"></a>Hizmet Yöneticisini değiştirme

Yalnızca hesap yöneticisi bir abonelik için hizmet yöneticisini değiştirebilir. Varsayılan olarak, bir Azure aboneliğine kaydolduğunuzda, hizmet yöneticisi hesap yöneticisiyle aynı olur. Hesap Yöneticisi rolüne sahip kullanıcının Azure portal erişimi yok. Hizmet Yöneticisi rolüne sahip kullanıcının Azure portal tam erişimi vardır. Hesap Yöneticisi ve hizmet yöneticisi aynı kullanıcı ise ve hizmet yöneticisini farklı bir kullanıcıyla değiştirirseniz, hesap yöneticisi Azure portal erişimi kaybeder. Ancak hesap yöneticisi her zaman hesap merkezini kullanarak hizmet yöneticisini kendilerine geri değiştirebilir.

**Hesap Merkezi**'Nde hizmet yöneticisini değiştirmek için bu adımları izleyin.

### <a name="account-center"></a>Hesap Merkezi

1. [Hizmet yöneticisini değiştirme sınırlamalarını](#limitations-for-changing-the-service-administrator)denetleyerek senaryonuzun desteklendiğinden emin olun.

1. Hesap [Merkezi](https://account.windowsazure.com/subscriptions) 'Nde hesap yöneticisi olarak oturum açın.

1. Bir aboneliğe tıklayın.

1. Sağ tarafta **abonelik ayrıntılarını Düzenle**' ye tıklayın.

    ![Hesap Merkezi 'nde abonelik Düzenle düğmesini gösteren ekran görüntüsü](./media/classic-administrators/editsub.png)

1. **HIZMET Yöneticisi** kutusuna yeni hizmet yöneticisinin e-posta adresini girin.

    ![Hizmet Yöneticisi e-postasını değiştirme kutusunu gösteren ekran görüntüsü](./media/classic-administrators/change-service-admin.png)

1. Değişikliği kaydetmek için onay işaretine tıklayın.

### <a name="limitations-for-changing-the-service-administrator"></a>Hizmet yöneticisini değiştirme sınırlamaları

Azure aboneliği başına yalnızca bir hizmet yöneticisi olabilir. Hizmet yöneticisinin değiştirilmesi, hesap yöneticisinin bir Microsoft hesabı mı yoksa bir Azure AD hesabı mı (iş veya okul hesabı) olup olmadığına bağlı olarak farklı davranaacaktır.

| Hesap yönetici hesabı | Hizmet Yöneticisi farklı bir Microsoft hesabı değiştirilebilir mi? | Hizmet yöneticisini aynı dizindeki bir Azure AD hesabına değiştirebilir mi? | Hizmet Yöneticisi 'ni farklı bir dizinde Azure AD hesabı olarak değiştirebilir miyim? |
| --- | --- | --- | --- |
| Microsoft hesabı | Evet | Hayır | Hayır |
| Azure AD hesabı | Yes | Evet | Hayır |

Hesap Yöneticisi bir Azure AD hesabı ise, hizmet yöneticisini aynı dizinde bir Azure AD hesabı olarak değiştirebilirsiniz, ancak farklı bir dizinde kullanamazsınız. Örneğin, abby@contoso.com hizmet yöneticisini olarak değiştirebilir bob@contoso.com , ancak john@notcontoso.com john@notcontoso.com contoso.com dizininde bir varlık olmadığı müddetçe hizmet yöneticisini olarak değiştiremezsiniz.

Microsoft hesapları ve Azure AD hesapları hakkında daha fazla bilgi için bkz. [Azure Active Directory nedir?](../active-directory/fundamentals/active-directory-whatis.md).

## <a name="view-the-account-administrator"></a>Hesap Yöneticisini görüntüleme

Hesap Yöneticisi, başlangıçta Azure aboneliğine kaydolan ve aboneliğin fatura sahibi olarak sorumlu olan Kullanıcı olur. Bir aboneliğin hesap yöneticisini değiştirmek için bkz. bir [Azure aboneliğinin sahipliğini başka bir hesaba aktarma](../cost-management-billing/manage/billing-subscription-transfer.md).

Hesap yöneticisini görüntülemek için aşağıdaki adımları izleyin.

1. [Azure portalında](https://portal.azure.com) oturum açın.

1. [Abonelikler](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade)'i açın ve bir abonelik seçin.

1. **Özellikler**'e tıklayın.

    Aboneliğin Hesap Yöneticisi, **Hesap** Yöneticisi kutusunda görüntülenir.

    ![Hesap yöneticisini gösteren ekran görüntüsü](./media/classic-administrators/account-admin.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Farklı rolleri anlama](../role-based-access-control/rbac-and-directory-admin-roles.md)
* [Azure portal kullanarak Azure rol atamaları ekleme veya kaldırma](../role-based-access-control/role-assignments-portal.md)
* [Azure aboneliği yöneticileri ekleme veya değiştirme](../cost-management-billing/manage/add-change-subscription-administrator.md)
