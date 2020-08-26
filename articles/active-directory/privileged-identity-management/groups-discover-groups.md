---
title: Rol atanabilir grupları ayrıcalıklı erişim grupları olarak yönetme-Azure AD | Microsoft Docs
description: Privileged Identity Management (PıM) içinde ayrıcalıklı erişim grupları olarak yönetilecek rol atanabilir grupları ekleme onayı.
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
ms.service: active-directory
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: pim
ms.date: 08/17/2020
ms.author: curtand
ms.collection: M365-identity-device-management
ms.openlocfilehash: a707c66f2562fe442c58ce6292e51c4e67a20ae2
ms.sourcegitcommit: 927dd0e3d44d48b413b446384214f4661f33db04
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88869549"
---
# <a name="manage-privileged-access-groups-preview-in-privileged-identity-management"></a>Privileged Identity Management 'da ayrıcalıklı erişim gruplarını yönetme (Önizleme)

Azure Active Directory (Azure AD) ' de, rol atamalarını nasıl yöneteceğinizi basitleştirmek için bulut gruplarına Azure AD yerleşik rolleri atayabilirsiniz. Azure AD rollerini korumak ve erişimin güvenliğini sağlamak için artık bu grupların üyeleri veya sahipleri için tam zamanında erişimi yönetmek üzere Privileged Identity Management (PıM) kullanabilirsiniz. Privileged Identity Management bir Azure AD rolüne atanabilir grubu, bir ayrıcalıklı erişim grubu olarak yönetmek için, bunu PıM 'de Yönetim altına almanız gerekir.

## <a name="identify-groups-to-manage"></a>Yönetilecek grupları tanımla

[Azure Active Directory içinde rol atanabilir Grup oluşturma](../users-groups-roles/roles-groups-create-eligible.md)bölümünde açıklandığı gibi, Azure AD 'de rol atanabilir bir grup oluşturabilirsiniz. Privileged Identity Management ' de yönetim altına getirmek için grubun sahibi siz olursunuz.

1. [Azure AD 'de](https://aad.portal.azure.com) ayrıcalıklı rol yöneticisi Izinleriyle oturum açın.
1. **Gruplar** ' ı seçin ve ardından yönetmek istediğiniz rol atanabilir grubu seçin. Listede arama yapabilir veya filtre uygulayabilirsiniz.

    ![PıM içinde yönetilecek rol atanabilir bir grup bulun](./media/groups-discover-groups/groups-list-in-azure-ad.png)

1. Grubu açın ve **ayrıcalıklı erişim (Önizleme)** seçeneğini belirleyin.

    ![Privileged Identity Management deneyimini açın](./media/groups-discover-groups/groups-discover-groups.png)

1. Gruplarınız henüz PıM 'de Yönetim altına getirilmediyse, yönetime izin vermek için **ayrıcalıklı erişimi etkinleştir** ' i seçin. Yalnızca genel yönetici veya bir grubun sahibi bu izni verebilir.

    ![gerekirse Privileged Identity Management grubunu yönetme onayı](./media/groups-discover-groups/consent-page.png)

1. PıM 'de atamaları yönetmeye başlayın.

    ![Privileged Identity Management atamaları yönetme](./media/groups-discover-groups/groups-bring-under-management.png)

> [!NOTE]
> Ayrıcalıklı bir erişim grubu yönetilmiyorsa, yönetim dışında alınamaz. Bu, başka bir yöneticinin Privileged Identity Management ayarlarını kaldırmasını engeller.

## <a name="next-steps"></a>Sonraki adımlar

- [Privileged Identity Management 'da ayrıcalıklı erişim grubu atamalarını yapılandırma](pim-resource-roles-configure-role-settings.md)
- [Privileged Identity Management ayrıcalıklı erişim grupları atama](pim-resource-roles-assign-roles.md)
