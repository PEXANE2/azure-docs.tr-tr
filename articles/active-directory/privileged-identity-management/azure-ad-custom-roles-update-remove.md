---
title: Azure AD özel rolünü güncelleştirme veya kaldırma - Ayrıcalıklı Kimlik Yönetimi (PIM)
description: Azure AD özel rol ataması Ayrıcalıklı Kimlik Yönetimi (PIM) güncelleştirme veya kaldırma
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
ms.openlocfilehash: ad5747be47e250cf9c623cc40d21d12c91ee16f5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77499115"
---
# <a name="update-or-remove-an-assigned-azure-ad-custom-role-in-privileged-identity-management"></a>Ayrıcalıklı Kimlik Yönetimi'nde atanmış bir Azure REKLAM özel rolünü güncelleştirme veya kaldırma

Bu makalede, Azure Etkin Dizini (Azure AD) yönetim deneyiminde uygulama yönetimi için oluşturulan özel rollere tam zamanında ve zamana bağlı atamayı güncelleştirmek veya kaldırmak için Ayrıcalıklı Kimlik Yönetimi'ni (PIM) nasıl kullanacağınızı anlatacağım. 

- Azure AD'de uygulama yönetimini devretmek için özel roller oluşturma hakkında daha fazla bilgi için Azure [Etkin Dizini'ndeki (önizleme) Özel yönetici rollerine](../users-groups-roles/roles-custom-overview.md)bakın. 
- Henüz Ayrıcalıklı Kimlik Yönetimi'ni kullanmadıysanız, [Ayrıcalıklı Kimlik Yönetimini kullanarak Başlat'tan](pim-getting-started.md)daha fazla bilgi alın.

> [!NOTE]
> Azure AD özel rolleri önizleme sırasında yerleşik dizin rolleri ile tümleşik değildir. Yetenek genel olarak kullanılabilir olduğunda, rol yönetimi yerleşik roller deneyiminde yer alacaktır. Aşağıdaki banner'ı görürseniz, bu roller [yerleşik roller deneyiminde](pim-how-to-add-role-to-user.md) yönetilmelidir ve bu makale geçerli değildir:
>
> [![](media/pim-how-to-add-role-to-user/pim-new-version.png "Select Azure AD > Privileged Identity Management")](media/pim-how-to-add-role-to-user/pim-new-version.png#lightbox)

## <a name="update-or-remove-an-assignment"></a>Bir atamayı güncelleştirme veya kaldırma

Varolan özel rol atamasını güncelleştirmek veya kaldırmak için aşağıdaki adımları izleyin.

1. Azure portalında Ayrıcalıklı Rol Yöneticisi rolüne atanmış bir kullanıcı hesabıyla [Ayrıcalıklı Kimlik Yönetimi'nde](https://portal.azure.com/?Microsoft_AAD_IAM_enableCustomRoleManagement=true&Microsoft_AAD_IAM_enableCustomRoleAssignment=true&feature.rbacv2roles=true&feature.rbacv2=true&Microsoft_AAD_RegisteredApps=demo#blade/Microsoft_Azure_PIMCommon/CommonMenuBlade/quickStart) oturum açın.
1. **Azure AD özel rollerini seçin (Önizleme)**.

    ![Uygun rol atamalarını görmek için Azure AD özel roller önizlemesini seçin](./media/azure-ad-custom-roles-assign/view-custom.png)

1. Azure AD uygulamaları için Özel Roller'in **Atamalar** listesini görmek için **Roller'i** seçin.

    ![Roller'i seçin uygun rol atamaları listesine bakın](./media/azure-ad-custom-roles-update-remove/assignments-list.png)

1. Güncelleştirmek veya kaldırmak istediğiniz rolü seçin.
1. **Uygun rollerdeki** veya Etkin **roller** sekmelerindeki rol atamasını bulun.
1. Rol atamasını güncelleştirmek veya kaldırmak için **Güncelleştir** veya **Kaldır'ı** seçin.

    ![Uygun rol atamasında kaldır veya güncelleştir'i seçin](./media/azure-ad-custom-roles-update-remove/remove-update.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD özel rolünü etkinleştirme](azure-ad-custom-roles-assign.md)
- [Azure AD özel rolü atama](azure-ad-custom-roles-assign.md)
- [Azure AD özel rol ataması yapılandırma](azure-ad-custom-roles-configure.md)
