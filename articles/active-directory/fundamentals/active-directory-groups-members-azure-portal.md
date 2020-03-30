---
title: Grup üyelerini ekleme veya kaldırma - Azure Etkin Dizini | Microsoft Dokümanlar
description: Azure Active Directory'yi kullanarak bir gruptan üye ekleme veya kaldırma hakkında yönergeler.
services: active-directory
author: msaburnley
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 08/23/2018
ms.author: ajburnle
ms.custom: it-pro, seodec18
ms.reviewer: krbain
ms.collection: M365-identity-device-management
ms.openlocfilehash: f1c83c57be63ae9e2a4d4113accaefe8a2c2b525
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68561961"
---
# <a name="add-or-remove-group-members-using-azure-active-directory"></a>Azure Etkin Dizini'ni kullanarak grup üyeleri ekleme veya kaldırma
Azure Etkin Dizini'ni kullanarak grup üyeleri eklemeye ve kaldırmaya devam edebilirsiniz.

## <a name="to-add-group-members"></a>Grup üyeleri eklemek için

1. Dizin için bir Genel yönetici hesabı kullanarak [Azure portalda](https://portal.azure.com) oturum açın.

2. **Azure Etkin Dizini'ni**seçin ve ardından **Gruplar'ı**seçin.

3. **Gruplardan - Tüm gruplar** sayfasında, üyeyi eklemek istediğiniz grubu arayın ve seçin. Bu durumda, daha önce oluşturulmuş grubumuz **OLAN MDM ilkesini**kullanın - Batı .

    ![Gruplar-Tüm gruplar sayfası, grup adı vurgulanır](media/active-directory-groups-members-azure-portal/group-all-groups-screen.png)

4. **MDM ilkesi - Batı Genel Bakışı** sayfasında, **Yönet** alanından **Üyeler** seçeneğini belirleyin.

    ![MDM ilkesi - Üyeler seçeneği vurgulanan Batı Genel Bakış sayfası](media/active-directory-groups-members-azure-portal/group-overview-blade.png)

5. **Üye Ekle'yi**seçin ve ardından gruba eklemek istediğiniz üyelerin her birini arayın ve seçin ve sonra **Seç'i**seçin.

    Üyelerin başarıyla eklandığını belirten bir ileti alırsınız.

    ![Üye sayfası ekle, aranan üye gösterilir](media/active-directory-groups-members-azure-portal/update-members.png)

6. Gruba eklenen tüm üye adlarını görmek için ekranı yenileyin.

## <a name="to-remove-group-members"></a>Grup üyelerini kaldırmak için

1. **Gruplardan - Tüm gruplar** sayfasında, üyeyi kaldırmak istediğiniz grubu arayın ve seçin. Yine kullanacağız, **MDM politikası - Batı**.

2. **Yönet** alanından **Üyeleri** seçin, kaldırmak için üyenin adını arayın ve seçin ve ardından **Kaldır'ı**seçin.

    ![Üye bilgileri sayfası, Kaldır seçeneği ile](media/active-directory-groups-members-azure-portal/remove-members-from-group.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Grupları ve üyeleri görüntüleme](active-directory-groups-view-azure-portal.md)

- [Grup ayarlarınızı düzenleme](active-directory-groups-settings-azure-portal.md)

- [Grupları kullanarak kaynaklara erişimi yönetme](active-directory-manage-groups.md)

- [Bir gruptaki kullanıcılar için dinamik kuralları yönetme](../users-groups-roles/groups-create-rule.md)

- [Azure Etkin Dizini'ne Azure aboneliği ni ilişkilendirme veya ekleme](active-directory-how-subscriptions-associated-directory.md)
