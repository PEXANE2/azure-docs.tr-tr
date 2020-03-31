---
title: Ayrıcalıklı Kimlik Yönetimi'nde Azure kaynak rolleri atama - Azure Etkin Dizini | Microsoft Dokümanlar
description: Azure AD Ayrıcalıklı Kimlik Yönetimi'nde (PIM) Azure kaynak rollerini nasıl atadiğinizi öğrenin.
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
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 34051a31c6ccf69356f330d7c5ecb009f760857a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79266565"
---
# <a name="assign-azure-resource-roles-in-privileged-identity-management"></a>Ayrıcalıklı Kimlik Yönetimi'nde Azure kaynak rollerini atama

Azure Etkin Dizin (Azure AD) Ayrıcalıklı Kimlik Yönetimi (PIM), yerleşik Azure kaynak rollerinin yanı sıra aşağıdakiler dahil (ancak bunlarla sınırlı olmamak üzere) özel rolleri yönetebilir:

- Sahip
- Kullanıcı Erişimi Yöneticisi
- Katılımcı
- Güvenlik Yöneticisi
- Güvenlik Yöneticisi

> [!NOTE]
> Sahibi veya Kullanıcı Erişim Yöneticisi abonelik rollerine atanan bir grubun kullanıcıları veya üyeleri ve Azure AD'de abonelik yönetimini etkinleştiren Azure AD Global yöneticileri varsayılan olarak Kaynak yöneticisi izinlerine sahiptir. Bu yöneticiler, Azure kaynakları için Ayrıcalıklı Kimlik Yönetimi'ni kullanarak roller atayabilir, rol ayarlarını yapılandırabilir ve erişimi gözden geçirebilir. Bir kullanıcı, Kaynak yöneticisinin izinleri olmadan Kaynaklar için Ayrıcalıklı Kimlik Yönetimi'ni yönetemez. Azure kaynakları için yerleşik roller listesini [görüntüleyin.](../../role-based-access-control/built-in-roles.md)

## <a name="assign-a-role"></a>Rol atama

Bir kullanıcıyı Azure kaynak rolü için uygun hale getirmek için aşağıdaki adımları izleyin.

1. [Ayrıcalıklı rol yöneticisi](../users-groups-roles/directory-assign-admin-roles.md#privileged-role-administrator) rolünün bir üyesi olan bir kullanıcıyla Azure [portalında](https://portal.azure.com/) oturum açın.

    Ayrıcalıklı Kimlik Yönetimi'ni yönetmek için başka bir yöneticiye erişim izni verme hakkında bilgi için, [Ayrıcalıklı Kimlik Yönetimi'ni yönetmek için diğer yöneticilere erişim izni verilmesi bölümüne](pim-how-to-give-access-to-pim.md)bakın.

1. **Azure AD Ayrıcalıklı Kimlik Yönetimi'ni**açın.

1. **Azure kaynaklarını**seçin.

1. Yönetilen kaynaklar listesini filtrelemek için **Kaynak filtresini** kullanın.

    ![Yönetilen Azure kaynakları listesi](./media/pim-resource-roles-assign-roles/resources-list.png)

1. Abonelik veya yönetim grubu gibi yönetmek istediğiniz kaynağı seçin.

1. Yönet'in altında, Azure kaynaklarının rol listesini görmek için **Roller'i** seçin.

    ![Azure kaynakları rolleri](./media/pim-resource-roles-assign-roles/resources-roles.png)

1. Yeni atama bölmesini açmak için **üye ekle'yi** seçin.

1. Rol bölmesini seçmek için **rol seçin'i** seçin.

    ![Yeni atama bölmesi](./media/pim-resource-roles-assign-roles/resources-select-role.png)

1. Atamak istediğiniz bir rolü seçin ve sonra **Seç'i**tıklatın.

    Üye seç veya grup bölmesi açılır.

1. Role atamak istediğiniz bir üye veya grubu seçin ve ardından **Seç'i**tıklatın.

    ![Üye veya grup bölmesi seçin](./media/pim-resource-roles-assign-roles/resources-select-member-or-group.png)

    Üyelik ayarları bölmesi açılır.

1. Atama **türü** listesinde **Uygun** veya **Etkin'i**seçin.

    ![Üyelikler ayarları bölmesi](./media/pim-resource-roles-assign-roles/resources-membership-settings-type.png)

    Azure kaynakları için ayrıcalıklı kimlik yönetimi iki farklı atama türü sağlar:

    - **Uygun** atamalar, rolü kullanmak için bir eylem gerçekleştirmek için rolün üyesigerektirir. Eylemler, çok faktörlü bir kimlik doğrulama (MFA) denetimi gerçekleştirmeyi, bir iş gerekçesi sağlamayı veya atanan onaylayıcılardan onay istemeyi içerebilir.

    - **Etkin** atamalar, üyenin rolü kullanmak için herhangi bir eylem gerçekleştirmesini gerektirmez. Etkin olarak atanan üyeler, her zaman bu role atanan ayrıcalıklara sahiptir.

1. Atama kalıcı (kalıcı olarak uygun veya kalıcı olarak atanmış) olmalıdır, **Kalıcı** onay kutusunu seçin.

    Rol ayarlarına bağlı olarak, onay kutusu görünmeyebilir veya değiştirilemez olabilir.

1. Belirli bir atama süresini belirtmek için onay kutusunu temizleyin ve başlangıç ve/veya bitiş tarihi ve saat kutularını değiştirin.

    ![Üyelik ayarları - tarih ve saat](./media/pim-resource-roles-assign-roles/resources-membership-settings-date.png)

1. Tamamlandığında, **Bitti'yi**seçin.

    ![Yeni atama - Ekle](./media/pim-resource-roles-assign-roles/resources-new-assignment-add.png)

1. Yeni rol ataması oluşturmak için **Ekle'yi**seçin. Durum bildirimi görüntülenir.

    ![Yeni atama - Bildirim](./media/pim-resource-roles-assign-roles/resources-new-assignment-notification.png)

## <a name="update-or-remove-an-existing-role-assignment"></a>Varolan bir rol atamayı güncelleştirme veya kaldırma

Varolan bir rol atamasını güncelleştirmek veya kaldırmak için aşağıdaki adımları izleyin.

1. **Azure AD Ayrıcalıklı Kimlik Yönetimi'ni**açın.

1. **Azure kaynaklarını**seçin.

1. Abonelik veya yönetim grubu gibi yönetmek istediğiniz kaynağı seçin.

1. Yönet'in altında, Azure kaynaklarının rol listesini görmek için **Roller'i** seçin.

    ![Azure kaynak rolleri - Rolü seçin](./media/pim-resource-roles-assign-roles/resources-update-select-role.png)

1. Güncelleştirmek veya kaldırmak istediğiniz rolü seçin.

1. **Uygun rollerdeki** veya Etkin **roller** sekmelerindeki rol atamasını bulun.

    ![Rol atamayı güncelleştirme veya kaldırma](./media/pim-resource-roles-assign-roles/resources-update-remove.png)

1. Rol atamasını güncelleştirmek veya kaldırmak için **Güncelleştir** veya **Kaldır'ı** seçin.

    Rol ataması genişletme hakkında bilgi için [bkz.](pim-resource-roles-renew-extend.md)

## <a name="next-steps"></a>Sonraki adımlar

- [Ayrıcalıklı Kimlik Yönetimi'nde Azure kaynak rollerini genişletme veya yenileme](pim-resource-roles-renew-extend.md)
- [Ayrıcalıklı Kimlik Yönetimi'nde Azure kaynak rol ayarlarını yapılandırma](pim-resource-roles-configure-role-settings.md)
- [Ayrıcalıklı Kimlik Yönetiminde Azure AD rollerini atama](pim-how-to-add-role-to-user.md)
