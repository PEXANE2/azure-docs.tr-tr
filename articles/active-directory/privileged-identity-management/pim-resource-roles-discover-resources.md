---
title: PıM-Azure AD 'de yönetilecek Azure kaynaklarını bulma | Microsoft Docs
description: Azure AD Privileged Identity Management (PıM) içinde yönetilecek Azure kaynaklarını bulmayı öğrenin.
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
ms.date: 11/08/2019
ms.author: curtand
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4852f841fc9ac8ebea586d5e24967a1db20b8044
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84743720"
---
# <a name="discover-azure-resources-to-manage-in-privileged-identity-management"></a>Privileged Identity Management içinde yönetilecek Azure kaynaklarını bulma

Azure Active Directory (Azure AD) Privileged Identity Management (PıM) kullanarak Azure kaynaklarınızın korunmasını geliştirebilirsiniz. Bu, Azure AD rollerini korumak için Privileged Identity Management kullanan kuruluşlar ve üretim kaynaklarını güvenli hale getirmek isteyen yönetim grubu ve abonelik sahipleri için yararlıdır.

Azure kaynakları için Privileged Identity Management ilk kez ayarladığınızda, Privileged Identity Management korunacak kaynakları bulmanız ve seçmeniz gerekir. Privileged Identity Management ile yönetebileceğiniz kaynak sayısı için bir sınır yoktur. Ancak, en kritik (üretim) kaynaklarınızla başlamasını öneririz.

## <a name="discover-resources"></a>Kaynak bulma

1. [Azure portalında](https://portal.azure.com/) oturum açın.

1. **Azure AD Privileged Identity Management**açın.

1. **Azure kaynakları**' nı seçin.

    Azure kaynakları için Privileged Identity Management ilk kez kullanıyorsanız, bir **bulma kaynakları** sayfası görürsünüz.

    ![İlk deneyime göre listelenen kaynakları içeren kaynakları bul bölmesi](./media/pim-resource-roles-discover-resources/discover-resources-first-run.png)

    Kuruluşunuzdaki başka bir yönetici zaten Privileged Identity Management ' de Azure kaynaklarını yönetiyor ise, şu anda yönetilmekte olan kaynakların bir listesini görürsünüz.

    ![Şu anda yönetilmekte olan kaynakları listelemeyi bul kaynak bölmesi](./media/pim-resource-roles-discover-resources/discover-resources.png)

1. Bulma deneyimini başlatmak için **kaynakları bul** ' u seçin.

    ![Abonelikler ve yönetim grupları gibi yönetilebilecek bulma bölmesi](./media/pim-resource-roles-discover-resources/discovery-pane.png)

1. **Bulma** sayfasında, **kaynak durumu Filtresi** ' ni kullanın ve yazma izninizin olduğu yönetim gruplarını veya abonelikleri filtrelemek için **kaynak türü** ' nü seçin. **Başlangıçta başlamak** en kolay yoldur.

    Yalnızca Privileged Identity Management kullanarak yönetmek için yönetim grubu veya abonelik kaynakları arayabilir ve seçebilirsiniz. Privileged Identity Management bir yönetim grubunu veya aboneliği yönetirken, alt kaynaklarını da yönetebilirsiniz.

1. Yönetmek istediğiniz yönetilmeyen kaynakların yanındaki onay kutusunu işaretleyin.

1. Seçilen kaynakları yönetmeye başlamak için **kaynağı Yönet** ' i seçin.

    > [!NOTE]
    > Bir yönetim grubu veya abonelik yönetilmiyorsa, yönetilmeyen olamaz. Bu, başka bir kaynak yöneticisinin Privileged Identity Management ayarlarını kaldırmasını engeller.

    ![Kaynak seçiliyken bulma bölmesi ve kaynağı Yönet seçeneği vurgulandı](./media/pim-resource-roles-discover-resources/discovery-manage-resource.png)

1. Seçili kaynağı yönetim için eklemeyi onaylamak üzere bir ileti görürseniz, **Evet**' i seçin.

    ![Seçili kaynakları yönetim için ekleme onayı bildiren ileti](./media/pim-resource-roles-discover-resources/discovery-manage-resource-message.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Privileged Identity Management Azure Kaynak rolü ayarlarını yapılandırma](pim-resource-roles-configure-role-settings.md)
- [Privileged Identity Management Azure Kaynak rolleri atama](pim-resource-roles-assign-roles.md)
