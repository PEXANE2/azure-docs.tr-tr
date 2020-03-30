---
title: Azure AD özel rolünü atama - Ayrıcalıklı Kimlik Yönetimi (PIM)
description: Atama Ayrıcalıklı Kimlik Yönetimi (PIM) için Azure AD özel rolü nasıl atayaşırınız?
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
ms.assetid: ''
ms.service: active-directory
ms.subservice: pim
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/06/2019
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0303d37ef5bbbf266feb5456b0bc224ce272ee13
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77499242"
---
# <a name="assign-an-azure-ad-custom-role-in-privileged-identity-management"></a>Ayrıcalıklı Kimlik Yönetimi'nde Azure AD özel rolü atama

Bu makalede, Azure Etkin Dizini (Azure AD) yönetim deneyiminde uygulamaları yönetmek için oluşturulan özel rollere tam zamanında ve zamana bağlı atama oluşturmak için Ayrıcalıklı Kimlik Yönetimi'ni (PIM) nasıl kullanacağınızı anlatır.

- Azure AD'de uygulama yönetimini devretmek için özel roller oluşturma hakkında daha fazla bilgi için Azure [Etkin Dizini'ndeki (önizleme) Özel yönetici rollerine](../users-groups-roles/roles-custom-overview.md)bakın.
- Henüz Ayrıcalıklı Kimlik Yönetimi'ni kullanmadıysanız, [Ayrıcalıklı Kimlik Yönetimini kullanarak Başlat'tan](pim-getting-started.md)daha fazla bilgi alın.
- Ayrıcalıklı Kimlik Yönetimi'ni yönetmek için başka bir yöneticiye erişim izni verme hakkında bilgi için, [Ayrıcalıklı Kimlik Yönetimi'ni yönetmek için diğer yöneticilere erişim izni verilmesi bölümüne](pim-how-to-give-access-to-pim.md)bakın.

> [!NOTE]
> Azure AD özel rolleri önizleme sırasında yerleşik dizin rolleri ile tümleşik değildir. Yetenek genel olarak kullanılabilir olduğunda, rol yönetimi yerleşik roller deneyiminde yer alacaktır. Aşağıdaki banner'ı görürseniz, bu roller [yerleşik roller deneyiminde](pim-how-to-activate-role.md) yönetilmelidir ve bu makale geçerli değildir:
>
> [![](media/pim-how-to-add-role-to-user/pim-new-version.png "Select Azure AD > Privileged Identity Management")](media/pim-how-to-add-role-to-user/pim-new-version.png#lightbox)

## <a name="assign-a-role"></a>Rol atama

Ayrıcalıklı Kimlik Yönetimi, Azure Etkin Dizin (Azure AD) uygulama yönetiminde oluşturabileceğiniz özel rolleri yönetebilir.  Aşağıdaki adımlar, özel bir dizin rolü için uygun bir atama yapar.

1. Azure portalında Ayrıcalıklı Rol Yöneticisi rolüne atanmış bir kullanıcı hesabıyla [Ayrıcalıklı Kimlik Yönetimi'nde](https://portal.azure.com/?Microsoft_AAD_IAM_enableCustomRoleManagement=true&Microsoft_AAD_IAM_enableCustomRoleAssignment=true&feature.rbacv2roles=true&feature.rbacv2=true&Microsoft_AAD_RegisteredApps=demo#blade/Microsoft_Azure_PIMCommon/CommonMenuBlade/quickStart) oturum açın.
1. **Azure AD özel rollerini seçin (Önizleme)**.

    ![Uygun rol atamalarını görmek için Azure AD özel roller önizlemesini seçin](./media/azure-ad-custom-roles-assign/view-custom.png)

1. Azure AD uygulamaları için özel rollerin listesini görmek için **Roller'i** seçin.

    ![Roller'i seçin uygun rol atamaları listesine bakın](./media/azure-ad-custom-roles-assign/view-roles.png)

1. Atama sayfasını açmak için **Üye Ekle'yi** seçin.
1. Rol atamasının kapsamını tek bir uygulamayla sınırlamak için, uygulama kapsamını belirtmek için **Kapsam'ı** seçin.

    ![Azure AD'de uygun rol atamalarının kapsamını kısıtlama](./media/azure-ad-custom-roles-assign/set-scope.png)

1. **Rol** seç'i açmak için **rol seç'i** seçin.

    ![bir kullanıcıya atamak için uygun rolü seçin](./media/azure-ad-custom-roles-assign/select-role.png)

1. Atamak istediğiniz bir rolü seçin ve sonra **Seç'i**tıklatın. **Üye seç** listesi açılır.

    ![rolü atadığınız kullanıcıyı seçin](./media/azure-ad-custom-roles-assign/select-member.png)

1. Role atamak istediğiniz bir kullanıcıyı seçin ve sonra **Seç'i**tıklatın. **Üyelik ayarları** listesi açılır.

    ![Rol atama türünü uygun veya etkin olarak ayarlama](./media/azure-ad-custom-roles-assign/membership-settings.png)

1. Üyelik **ayarları** sayfasında **Uygun** veya **Etkin'i**seçin:

    - **Uygun** atamalar, rolü kullanabilmeleri için rol için atanan kullanıcının bir eylemi gerçekleştirmesini gerektirir. Eylemler, çok faktörlü bir kimlik doğrulama denetimi nin geçirilmesini, iş gerekçesi nin sağlanmasını veya atanan onaylayıcılardan onay istemeyi içerebilir.
    - **Etkin** atamalar, atanan kullanıcının rolü kullanmak için herhangi bir eylem gerçekleştirmesini gerektirmez. Etkin kullanıcılar, bu role her zaman atanan ayrıcalıklara sahiptir.

1. **Kalıcı** onay kutusu varsa ve kullanılabilirse (rol ayarlarına bağlı olarak), atamanın kalıcı olup olmadığını belirtebilirsiniz. Atamayı kalıcı olarak uygun veya kalıcı olarak atanmış yapmak için onay kutusunu seçin. Atama süresini belirtmek için onay kutusunu temizleyin.
1. Yeni rol ataması oluşturmak için **Kaydet'i** tıklatın ve sonra **ekle'yi tıklatın.** Atama işlemi durumu bildirimi görüntülenir.

Açık bir roldeki rol atamasını doğrulamak için **Atamalar** > **Atamasını** seçin ve rol atamanızın uygun veya etkin olarak tanımlanıp tanımlandığını doğrulayın.

 ![Rol atamasının uygun veya etkin olarak görünip görünip görünip görünip görünip görünip görünip görünip görünip görünip görünip görmeyin](./media/azure-ad-custom-roles-assign/verify-assignments.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD özel rolünü etkinleştirme](azure-ad-custom-roles-assign.md)
- [Azure AD özel rol ataması kaldırma veya güncelleştirme](azure-ad-custom-roles-update-remove.md)
- [Azure AD özel rol ataması yapılandırma](azure-ad-custom-roles-configure.md)
- [Azure AD'de rol tanımları](../users-groups-roles/directory-assign-admin-roles.md)
