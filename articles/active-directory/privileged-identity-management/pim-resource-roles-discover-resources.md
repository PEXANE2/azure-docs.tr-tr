---
title: PıM-Azure Active Directory 'de yönetilecek Azure kaynaklarını bulma | Microsoft Docs
description: Azure AD Privileged Identity Management (PıM) içinde yönetilecek Azure kaynaklarını bulmayı öğrenin.
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
ms.date: 04/09/2019
ms.author: curtand
ms.collection: M365-identity-device-management
ms.openlocfilehash: ef1f1712ab7474747829c8744a39e74b78777668
ms.sourcegitcommit: 95b180c92673507ccaa06f5d4afe9568b38a92fb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/08/2019
ms.locfileid: "70804173"
---
# <a name="discover-azure-resources-to-manage-in-pim"></a>PıM 'de yönetilecek Azure kaynaklarını bulma

Azure Active Directory (Azure AD) Privileged Identity Management (PıM) kullanarak Azure kaynaklarınızın korunmasını geliştirebilirsiniz. Bu, Azure AD rollerini korumak için zaten PıM kullanan kuruluşlar ve üretim kaynaklarını güvenli hale getirmek isteyen yönetim grubu ve abonelik sahipleri için yararlıdır.

Azure kaynakları için PıM 'yi ilk kez ayarladığınızda, PıM ile korunacak kaynakları bulmanız ve seçmeniz gerekir. PıM ile yönetebileceğiniz kaynak sayısıyla ilgili bir sınır yoktur. Ancak, en kritik (üretim) kaynaklarınızla başlamasını öneririz.

## <a name="discover-resources"></a>Kaynakları bulma

1. [Azure Portal](https://portal.azure.com/) oturum açın.

1. **Azure AD Privileged Identity Management**açın.

1. **Azure kaynakları**' na tıklayın.

    Azure kaynakları için PıM 'yi ilk kez kullanıyorsanız, bir bulma kaynakları bölmesi görürsünüz.

    ![İlk deneyime göre listelenen kaynakları içeren kaynakları bul bölmesi](./media/pim-resource-roles-discover-resources/discover-resources-first-run.png)

    Kuruluşunuzdaki başka bir kaynak veya dizin Yöneticisi zaten PıM 'de Azure kaynaklarını yönetiyorsanız, şu anda yönetilmekte olan kaynakların bir listesini görürsünüz.

    ![Şu anda yönetilmekte olan kaynakları listelemeyi bul kaynak bölmesi](./media/pim-resource-roles-discover-resources/discover-resources.png)

1. Bulma deneyimini başlatmak için **kaynakları keşfet** ' e tıklayın.

    ![Abonelikler ve yönetim grupları gibi yönetilebilecek bulma bölmesi](./media/pim-resource-roles-discover-resources/discovery-pane.png)

1. Bulma bölmesinde, **kaynak durumu Filtresi** ' ni kullanın ve yazma izninizin olduğu yönetim gruplarını veya abonelikleri filtrelemek için **kaynak türü** ' nü seçin. **Başlangıçta başlamak** en kolay yoldur.

    Yalnızca PıM kullanarak yönetmek için yönetim grubu veya abonelik kaynaklarını arayabilir ve seçebilirsiniz. Bir yönetim grubunu veya bir aboneliği PıM 'de yönetirken, alt kaynaklarını da yönetebilirsiniz.

1. Yönetmek istediğiniz yönetilmeyen kaynakların yanına onay işareti ekleyin.

1. Seçilen kaynakları yönetmeye başlamak için **kaynağı Yönet** ' e tıklayın.

    > [!NOTE]
    > Bir yönetim grubu veya abonelik yönetilmek üzere ayarlandığında, yönetilmeyen olamaz. Bu, başka bir kaynak yöneticisinin PıM ayarlarını kaldırmasını engeller.

    ![Kaynak seçiliyken bulma bölmesi ve kaynağı Yönet seçeneği vurgulandı](./media/pim-resource-roles-discover-resources/discovery-manage-resource.png)

1. Seçili kaynağı yönetim için eklemeyi onaylamak üzere bir ileti görürseniz, **Evet**' e tıklayın.

    ![Seçili kaynakları yönetim için ekleme onayı bildiren ileti](./media/pim-resource-roles-discover-resources/discovery-manage-resource-message.png)

## <a name="next-steps"></a>Sonraki adımlar

- [PıM 'de Azure Kaynak rolü ayarlarını yapılandırma](pim-resource-roles-configure-role-settings.md)
- [PıM 'de Azure Kaynak rolleri atama](pim-resource-roles-assign-roles.md)
