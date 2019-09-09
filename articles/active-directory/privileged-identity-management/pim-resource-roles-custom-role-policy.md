---
title: PıM-Azure Active Directory 'de Azure kaynakları için özel roller kullanma | Microsoft Docs
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
ms.date: 03/30/2018
ms.author: curtand
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1d36514c97cf1f45ee0a435d3b716019d2762e5a
ms.sourcegitcommit: 95b180c92673507ccaa06f5d4afe9568b38a92fb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/08/2019
ms.locfileid: "70804183"
---
# <a name="use-custom-roles-for-azure-resources-in-pim"></a>PıM 'de Azure kaynakları için özel roller kullanma

Bir rolün bazı üyelerine katı Azure Active Directory (Azure AD) Privileged Identity Management (PıM) ayarları uygulamanız gerekebilir, diğerleri için daha fazla bağımsız çalışma sınırı sağlayabilirsiniz. Kuruluşunuzun çeşitli sözleşmeleri bir Azure aboneliğinde çalışacak bir uygulama geliştirmede yardımcı olacak şekilde ilişkilendiğini gösteren bir senaryo düşünün.

Bir kaynak yöneticisi olarak, çalışanların onaya gerek duymadan erişim için uygun olmasını istersiniz. Ancak, kuruluşun kaynaklarına erişim istediklerinde tüm sözleşme ilişkilendirmeleri onaylanmalıdır.

Azure Kaynak rolleri için hedeflenen PıM ayarlarını ayarlamak üzere sonraki bölümde özetlenen adımları izleyin.

## <a name="create-the-custom-role"></a>Özel rol oluşturma

Bir kaynak için özel rol oluşturmak üzere [Azure rol tabanlı Access Control için özel roller oluşturma](../role-based-access-control-custom-roles.md)bölümünde açıklanan adımları izleyin.

Özel rol oluşturduğunuzda, yinelemek istediğiniz yerleşik rolü kolayca hatırlayabilmeniz için açıklayıcı bir ad ekleyin.

> [!NOTE]
> Özel rolün, çoğaltmak istediğiniz yerleşik rolün bir yinelemesi olduğundan ve kapsamının yerleşik rolle eşleştiğinden emin olun.

## <a name="apply-pim-settings"></a>PıM ayarlarını uygula

Kiracınızda rol oluşturulduktan sonra, Azure portal **Privileged Identity Management-Azure kaynakları** bölmesine gidin. Rolün uygulandığı kaynağı seçin.

!["Privileged Identity Management-Azure kaynakları" bölmesi](media/pim-resource-roles-custom-role-policy/aadpim-manage-azure-resource-some-there.png)

Rolün bu üyeleri için uygulanması gereken [PIM rol ayarlarını yapılandırın](pim-resource-roles-configure-role-settings.md) .

Son olarak, bu ayarlarla hedeflemek istediğiniz farklı üye grubuna [Roller atayın](pim-resource-roles-assign-roles.md) .

## <a name="next-steps"></a>Sonraki adımlar

- [PıM 'de Azure Kaynak rolü ayarlarını yapılandırma](pim-resource-roles-configure-role-settings.md)
- [Azure'da özel roller](../../role-based-access-control/custom-roles.md)
