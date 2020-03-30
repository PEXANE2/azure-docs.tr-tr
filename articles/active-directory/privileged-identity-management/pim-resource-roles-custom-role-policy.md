---
title: PIM'de Azure kaynakları için özel roller kullanma - Azure AD | Microsoft Dokümanlar
description: Azure AD Ayrıcalıklı Kimlik Yönetimi'nde (PIM) Azure kaynakları için özel rolleri nasıl kullanacağınızı öğrenin.
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: pim
ms.date: 11/08/2019
ms.author: curtand
ms.collection: M365-identity-device-management
ms.openlocfilehash: fbe08cff2b57155f8f3315f5d3454abfbdad47a0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73847055"
---
# <a name="use-custom-roles-for-azure-resources-in-privileged-identity-management"></a>Ayrıcalıklı Kimlik Yönetimi'nde Azure kaynakları için özel roller kullanma

Azure Etkin Dizin (Azure AD) kuruluşunuzdaki ayrıcalıklı rolü olan bazı kullanıcılara katı Ayrıcalıklı Kimlik Yönetimi (PIM) ayarları uygulamanız ve diğerleri için daha fazla özerklik sağlamanız gerekebilir. Örneğin, kuruluşunuzun Azure aboneliğinde çalışacak bir uygulamanın geliştirilmesine yardımcı olması için birkaç sözleşme ortağı tuttuğu bir senaryo düşünün.

Kaynak yöneticisi olarak, çalışanların onay gerektirmeden erişime uygun olmasını istiyorsunuz. Ancak, tüm sözleşme ortakları, kuruluşun kaynaklarına erişim istediklerinde onaylanmalıdır.

Azure kaynak rolleri için hedeflenen Ayrıcalıklı Kimlik Yönetimi ayarlarını ayarlamak için sonraki bölümde belirtilen adımları izleyin.

## <a name="create-the-custom-role"></a>Özel rolü oluşturma

Bir kaynak için özel bir rol oluşturmak [için, Azure Rol Tabanlı Erişim Denetimi için özel roller oluşturma'da](../role-based-access-control-custom-roles.md)açıklanan adımları izleyin.

Özel rol oluşturduğunuzda, hangi yerleşik rolü çoğaltmayı planladığınızı kolayca hatırlayabilmeniz için açıklayıcı bir ad ekleyin.

> [!NOTE]
> Özel rolün yinelemek istediğiniz yerleşik rolün bir kopyası olduğundan ve kapsamının yerleşik rolle eşleştiğinden emin olun.

## <a name="apply-pim-settings"></a>PIM ayarlarını uygulayın

Azure REKLAM kuruluşunuzdaki rol oluşturulduktan sonra, Azure portalındaki **Ayrıcalıklı Kimlik Yönetimi - Azure kaynakları** sayfasına gidin. Rolün uygulandığı kaynağı seçin.

!["Ayrıcalıklı Kimlik Yönetimi - Azure kaynakları" bölmesi](media/pim-resource-roles-custom-role-policy/aadpim-manage-azure-resource-some-there.png)

Rolün bu üyelerine uygulanacak [Ayrıcalıklı Kimlik Yönetimi rol ayarlarını yapılandırın.](pim-resource-roles-configure-role-settings.md)

Son olarak, bu ayarlarla hedeflemek istediğiniz farklı üye grubuna [roller atayın.](pim-resource-roles-assign-roles.md)

## <a name="next-steps"></a>Sonraki adımlar

- [Ayrıcalıklı Kimlik Yönetimi'nde Azure kaynak rol ayarlarını yapılandırma](pim-resource-roles-configure-role-settings.md)
- [Azure'da özel roller](../../role-based-access-control/custom-roles.md)
