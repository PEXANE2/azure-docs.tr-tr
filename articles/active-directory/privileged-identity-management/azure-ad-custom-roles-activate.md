---
title: Azure AD özel rolünü etkinleştirme - Ayrıcalıklı Kimlik Yönetimi (PIM)
description: Atama Ayrıcalıklı Kimlik Yönetimi (PIM) için Azure AD özel rolü etkinleştirme
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
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
ms.openlocfilehash: cbd60d1311bd84adb303a0d329ab4e42f4d61525
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77498726"
---
# <a name="activate-an-azure-ad-custom-role-in-privileged-identity-management"></a>Ayrıcalıklı Kimlik Yönetimi'nde Azure AD özel rolünü etkinleştirme

Azure Etkin Dizininde (Azure AD)'de Ayrıcalıklı Kimlik Yönetimi, artık Kimlik ve Erişim Yönetimi yönetim deneyiminde Uygulama Yönetimi için oluşturulan özel rollere tam zamanında ve zamana bağlı atamayı destekler. Azure AD'de uygulama yönetimini devretmek için özel roller oluşturma hakkında daha fazla bilgi için Azure [Etkin Dizini'ndeki (önizleme) Özel yönetici rollerine](../users-groups-roles/roles-custom-overview.md)bakın.

> [!NOTE]
> Azure AD özel rolleri önizleme sırasında yerleşik dizin rolleri ile tümleşik değildir. Yetenek genel olarak kullanılabilir olduğunda, rol yönetimi yerleşik roller deneyiminde yer alacaktır. Aşağıdaki banner'ı görürseniz, bu roller [yerleşik roller deneyiminde](pim-how-to-activate-role.md) yönetilmelidir ve bu makale geçerli değildir:
>
> [![](media/pim-how-to-add-role-to-user/pim-new-version.png "Select Azure AD > Privileged Identity Management")](media/pim-how-to-add-role-to-user/pim-new-version.png#lightbox)

## <a name="activate-a-role"></a>Bir rolü etkinleştirme

Azure AD özel rolünü etkinleştirmeniz gerektiğinde, Ayrıcalıklı Kimlik Yönetimi'nde Rollerim gezinti seçeneğini seçerek etkinleştirme isteğinde bulunun.

1. [Azure portalında](https://portal.azure.com)oturum açın.
1. Azure AD [Ayrıcalıklı Kimlik Yönetimi'ni](https://portal.azure.com/?Microsoft_AAD_IAM_enableCustomRoleManagement=true&Microsoft_AAD_IAM_enableCustomRoleAssignment=true&feature.rbacv2roles=true&feature.rbacv2=true&Microsoft_AAD_RegisteredApps=demo#blade/Microsoft_Azure_PIMCommon/CommonMenuBlade/quickStart)açın.

1. Uygun Azure AD özel rol atamalarınızın listesini görmek için **Azure AD özel rollerini** seçin.

   ![Uygun Azure AD özel rol atamaları listesine bakın](./media/azure-ad-custom-roles-activate/view-preview-roles.png)

> [!Note] 
>  Bir rol atamadan önce, bir rol oluşturmanız/yapılandırmanız gerekir. AAD Özel Rollerin yapılandırılması ile ilgili daha fazla bilgi için bkz.https://docs.microsoft.com/azure/active-directory/privileged-identity-management/azure-ad-custom-roles-configure)

1. Azure **AD özel roller (Önizleme)** sayfasında, gereksinim duyduğunuz atamayı bulun.
1. **Etkinleştir** sayfasını açmak için **rolünüzü etkinleştir'i** seçin.
1. Rolünüz çok faktörlü kimlik doğrulama gerektiriyorsa, **devam etmeden önce kimliğinizi doğrulayın'ı**seçin. Oturum başına yalnızca bir kez kimlik doğrulamanız gerekir.
1. **Kimliğimi doğrula'yı** seçin ve ek güvenlik doğrulaması sağlamak için yönergeleri izleyin.
1. Özel bir uygulama kapsamı belirtmek için filtre bölmesini açmak için **Kapsam'ı** seçin. Bir role gereken minimum kapsamda erişim istemeniz gerekir. Atamanız bir uygulama kapsamındaysa, yalnızca bu kapsamda etkinleştirebilirsiniz.

   ![Rol atamasına Azure AD kaynak kapsamı atama](./media/azure-ad-custom-roles-activate/assign-scope.png)

1. Gerekirse, özel bir etkinleştirme başlangıç saati belirtin. Kullanıldığında, rol üyesi belirtilen zamanda etkinleştirilir.
1. **Reason** kutusuna etkinleştirme isteğinin nedenini girin. Bunlar gerekli veya rol ayarı yapılabilir.
1. **Etkinleştir'i**seçin.

Rol onay gerektirmiyorsa, ayarlarınıza göre etkinleştirilir ve etkin roller listesine eklenir. Etkinleştirilen rolü kullanmak istiyorsanız, [Ayrıcalıklı Kimlik Yönetimi'nde Azure AD özel rolünü atayın](azure-ad-custom-roles-assign.md)adımlarıyla başlayın.

Rol etkinleştirmek için onay gerektiriyorsa, isteğin onay beklemede olduğunu bildiren bir Azure bildirimi alırsınız.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD özel rolü atama](azure-ad-custom-roles-assign.md)
- [Azure AD özel rol ataması kaldırma veya güncelleştirme](azure-ad-custom-roles-update-remove.md)
- [Azure AD özel rol ataması yapılandırma](azure-ad-custom-roles-configure.md)
- [Azure AD'de rol tanımları](../users-groups-roles/directory-assign-admin-roles.md)
