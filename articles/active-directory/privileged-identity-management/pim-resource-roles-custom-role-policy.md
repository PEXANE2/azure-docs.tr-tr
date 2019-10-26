---
title: Privileged Identity Management Azure Active Directory Azure kaynakları için özel roller kullanma | Microsoft Docs
description: Azure AD Privileged Identity Management (PıM) içinde Azure kaynakları için özel rolleri nasıl kullanacağınızı öğrenin.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: pim
ms.date: 10/23/2019
ms.author: curtand
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8e940dd0e4c653ed4921a9e8f245aab34107ba95
ms.sourcegitcommit: 5acd8f33a5adce3f5ded20dff2a7a48a07be8672
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2019
ms.locfileid: "72895705"
---
# <a name="use-custom-roles-for-azure-resources-in-privileged-identity-management"></a>Privileged Identity Management Azure kaynakları için özel roller kullanın

Bağımsız çalışma sınırı (Azure AD) kuruluşunuzda ayrıcalıklı bir Azure Active Directory roldeki bazı kullanıcılara katı Privileged Identity Management (PıM) ayarları uygulamanız gerekebilir, diğerleri için daha fazla sağlayabilirsiniz. Örneğin, kuruluşunuzun çeşitli sözleşmeleri bir Azure aboneliğinde çalışacak bir uygulama geliştirmede yardımcı olacak şekilde ilişkilendiğini göz önünde bulundurun.

Bir kaynak yöneticisi olarak, çalışanların onaya gerek duymadan erişim için uygun olmasını istersiniz. Ancak, kuruluşun kaynaklarına erişim istediklerinde tüm sözleşme ilişkilendirmeleri onaylanmalıdır.

Azure Kaynak rollerinin hedeflenen Privileged Identity Management ayarlarını yapmak için sonraki bölümde özetlenen adımları izleyin.

## <a name="create-the-custom-role"></a>Özel rol oluşturma

Bir kaynak için özel rol oluşturmak üzere [Azure rol tabanlı Access Control için özel roller oluşturma](../role-based-access-control-custom-roles.md)bölümünde açıklanan adımları izleyin.

Özel rol oluşturduğunuzda, yinelemek istediğiniz yerleşik rolü kolayca hatırlayabilmeniz için açıklayıcı bir ad ekleyin.

> [!NOTE]
> Özel rolün, çoğaltmak istediğiniz yerleşik rolün bir yinelemesi olduğundan ve kapsamının yerleşik rolle eşleştiğinden emin olun.

## <a name="apply-pim-settings"></a>PıM ayarlarını uygula

Rol Azure AD kuruluşunuzda oluşturulduktan sonra, Azure portal **Privileged Identity Management-Azure kaynakları** sayfasına gidin. Rolün uygulandığı kaynağı seçin.

!["Privileged Identity Management-Azure kaynakları" bölmesi](media/pim-resource-roles-custom-role-policy/aadpim-manage-azure-resource-some-there.png)

Rolün bu üyeleri için uygulanması gereken [Privileged Identity Management rol ayarlarını yapılandırın](pim-resource-roles-configure-role-settings.md) .

Son olarak, bu ayarlarla hedeflemek istediğiniz farklı üye grubuna [Roller atayın](pim-resource-roles-assign-roles.md) .

## <a name="next-steps"></a>Sonraki adımlar

- [Privileged Identity Management Azure Kaynak rolü ayarlarını yapılandırma](pim-resource-roles-configure-role-settings.md)
- [Azure'da özel roller](../../role-based-access-control/custom-roles.md)
