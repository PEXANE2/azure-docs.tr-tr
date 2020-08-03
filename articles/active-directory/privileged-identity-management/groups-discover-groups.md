---
title: PıM-Azure AD 'de yönetilecek rol atanabilir grupları bul | Microsoft Docs
description: Privileged Identity Management (PıM) içinde ayrıcalıklı erişim grupları olarak yönetmek için rol atanabilir grupları bulmayı öğrenin.
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
ms.date: 07/27/2020
ms.author: curtand
ms.collection: M365-identity-device-management
ms.openlocfilehash: c8b66a41f0f1aa48433dac77fca56d5e2877989a
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2020
ms.locfileid: "87506010"
---
# <a name="discover-privileged-access-groups-preview-to-manage-in-privileged-identity-management"></a>Privileged Identity Management yönetmek için ayrıcalıklı erişim gruplarını (Önizleme) bulun

Azure Active Directory (Azure AD) ' de, rol atamalarının yönetimini basitleştirmek için Azure AD yerleşik rollerini bulut gruplarına atayabilirsiniz. Artık, Azure AD rollerini korumak ve erişimi güvenli hale getirmek için bu grupların üyelik veya sahipliğine uygunluk atamak üzere Privileged Identity Management (PıM) kullanabilirsiniz. Privileged Identity Management ' de bir Azure AD rolü atanabilir grubu yönetebilmeniz için önce, bunu bulmanız ve PıM 'de Yönetim altına getirmeniz gerekir.

## <a name="discover-resources"></a>Kaynak bulma

1. [Azure AD 'de](https://aad.portal.azure.com) ayrıcalıklı rol yöneticisi rol Izinleriyle oturum açın.
1. Azure AD 'de rol atanabilir bir grup oluşturun. Privileged Identity Management ile bulup yönetmek için grubun sahibi olmanız gerekir.
1. **Privileged Identity Management**açın.
1. **Ayrıcalıklı erişim (Önizleme)** seçeneğini belirleyin.

    ![İlk deneyime ilişkin grupları Bul komutu](./media/groups-discover-groups/groups-discover-groups.png)

1. **Grupları bul**' u seçin.
1. Grup adına göre arama yapın.
1. Grubunuzu seçin ve PıM yönetimi altına getirmek için **grupları yönet** ' i seçin.

    ![İlk deneyime ait hiçbir kaynağı listelenen grupları bulma](./media/groups-discover-groups/groups-bring-under-management.png)

    > [!NOTE]
    > Ayrıcalıklı bir erişim grubu yönetilmiyorsa, yönetim dışında alınamaz. Bu, başka bir kaynak yöneticisinin Privileged Identity Management ayarlarını kaldırmasını engeller.

1. Seçili kaynağı yönetim için eklemeyi onaylamak üzere bir ileti görürseniz, **Evet**' i seçin.

## <a name="next-steps"></a>Sonraki adımlar

- [Privileged Identity Management 'da ayrıcalıklı erişim grubu atamalarını yapılandırma](pim-resource-roles-configure-role-settings.md)
- [Privileged Identity Management ayrıcalıklı erişim grupları atama](pim-resource-roles-assign-roles.md)
