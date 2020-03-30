---
title: PIM'de yönetmek için Azure kaynaklarını keşfedin - Azure AD | Microsoft Dokümanlar
description: Azure AD Ayrıcalıklı Kimlik Yönetimi'nde (PIM) yönetmek için Azure kaynaklarını nasıl keşfederiz öğrenin.
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
ms.openlocfilehash: 87da43100c7494937ddc842e0f903ba3a360959e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74022887"
---
# <a name="discover-azure-resources-to-manage-in-privileged-identity-management"></a>Ayrıcalıklı Kimlik Yönetimi'nde yönetmek için Azure kaynaklarını keşfedin

Azure Etkin Dizin (Azure AD) Ayrıcalıklı Kimlik Yönetimi 'ni (PIM) kullanarak Azure kaynaklarınızın korunmasını geliştirebilirsiniz. Bu, Azure AD rollerini korumak için Ayrıcalıklı Kimlik Yönetimi'ni zaten kullanan kuruluşlar ve üretim kaynaklarını güvence altına almak isteyen yönetim grubu ve abonelik sahiplerine yardımcı olur.

Azure kaynakları için Ayrıcalıklı Kimlik Yönetimi'ni ilk kurduğunuzda, Ayrıcalıklı Kimlik Yönetimi ile korumak için kaynakları keşfetmeniz ve seçmeniz gerekir. Ayrıcalıklı Kimlik Yönetimi ile yönetebileceğiniz kaynak sayısının sınırı yoktur. Ancak, en kritik (üretim) kaynaklarınızla başlamanızı öneririz.

## <a name="discover-resources"></a>Kaynakları bulma

1. [Azure portalında](https://portal.azure.com/)oturum açın.

1. **Azure AD Ayrıcalıklı Kimlik Yönetimi'ni**açın.

1. **Azure kaynaklarını**seçin.

    Azure kaynakları için Ayrıcalıklı Kimlik Yönetimi'ni ilk kez kullanıyorsanız, **kaynakları keşfet** sayfasını görürsünüz.

    ![İlk kez deneyim için listelenen kaynak olmadan kaynak bölmelerini keşfedin](./media/pim-resource-roles-discover-resources/discover-resources-first-run.png)

    Kuruluşunuzdaki başka bir yönetici Ayrıcalıklı Kimlik Yönetimi'nde Azure kaynaklarını zaten yönetiyorsa, şu anda yönetilen kaynakların listesini görürsünüz.

    ![Şu anda yönetilen kaynakları bulma](./media/pim-resource-roles-discover-resources/discover-resources.png)

1. Keşif deneyimini başlatmak için **Kaynakları Keşfet'i** seçin.

    ![Abonelikler ve yönetim grupları gibi yönetilebilen bulma bölmesi listesi kaynakları](./media/pim-resource-roles-discover-resources/discovery-pane.png)

1. **Bulma** sayfasında, kaynak **durum filtresini** kullanın ve yazma izniniz olan yönetim gruplarını veya abonelikleri filtrelemek için kaynak türünü **seçin.** Başlangıçta **All** ile başlamak muhtemelen en kolayı.

    Ayrıcalıklı Kimlik Yönetimi'ni kullanarak yönetmek için yalnızca yönetim grubunu veya abonelik kaynaklarını arayabilir ve seçebilirsiniz. Ayrıcalıklı Kimlik Yönetimi'nde bir yönetim grubunu veya aboneliği yönettiğiniz zaman, alt kaynaklarını da yönetebilirsiniz.

1. Yönetmek istediğiniz yönetilmeyen kaynakların yanındaki onay kutusunu seçin.

1. Seçili kaynakları yönetmeye başlamak için **kaynağı Yönet'i** seçin.

    > [!NOTE]
    > Bir yönetim grubu veya abonelik yönetildikten sonra yönetilemez. Bu, başka bir kaynak yöneticisinin Ayrıcalıklı Kimlik Yönetimi ayarlarını kaldırmasını engeller.

    ![Seçili bir kaynak ve kaynak seçeneği vurgulanan bulma bölmesi](./media/pim-resource-roles-discover-resources/discovery-manage-resource.png)

1. Yönetim için seçili kaynağın onboarding onaylamak için bir ileti görürseniz, **Evet'i**seçin.

    ![Yönetim için seçilen kaynakları onboard'a onaylayan ileti](./media/pim-resource-roles-discover-resources/discovery-manage-resource-message.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Ayrıcalıklı Kimlik Yönetimi'nde Azure kaynak rol ayarlarını yapılandırma](pim-resource-roles-configure-role-settings.md)
- [Ayrıcalıklı Kimlik Yönetimi'nde Azure kaynak rollerini atama](pim-resource-roles-assign-roles.md)
