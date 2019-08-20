---
title: PıM-Azure Active Directory 'de Azure Kaynak rolleri atama | Microsoft Docs
description: Azure AD Privileged Identity Management (PıM) içinde Azure Kaynak rolleri atamayı öğrenin.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: pim
ms.date: 04/09/2019
ms.author: rolyon
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5919c9ab53761649c55e67ceacec0409174e9120
ms.sourcegitcommit: e42c778d38fd623f2ff8850bb6b1718cdb37309f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/19/2019
ms.locfileid: "69617039"
---
# <a name="assign-azure-resource-roles-in-pim"></a>PıM 'de Azure Kaynak rolleri atama

Azure Active Directory (Azure AD) Privileged Identity Management (PıM), yerleşik Azure Kaynak rollerinin yanı sıra (ancak bunlarla sınırlı olmamak üzere) özel roller yönetebilir:

- Sahip
- Kullanıcı Erişimi Yöneticisi
- Katılımcı
- Güvenlik Yöneticisi
- Güvenlik Yöneticisi ve daha fazlası

> [!NOTE]
> Sahibine veya Kullanıcı erişimi yönetici rollerine atanan bir grubun kullanıcıları veya üyeleri ve Azure AD 'de abonelik yönetimini etkinleştiren Genel Yöneticiler kaynak yöneticileridir. Bu yöneticiler, rol atayabilir, rol ayarlarını yapılandırabilir ve Azure kaynakları için PıM 'yi kullanarak erişimi gözden geçirebilir. Diğer bir deyişle, kullanıcının bir kaynak yöneticisi rolü yoksa, hesabın PıM kaynaklarını yönetme hakkı olmaz. [Azure kaynakları için yerleşik rollerin](../../role-based-access-control/built-in-roles.md)listesini görüntüleyin.

## <a name="assign-a-role"></a>Rol atama

Bir kullanıcıyı Azure Kaynak rolüne uygun hale getirmek için aşağıdaki adımları izleyin.

1. [Ayrıcalıklı rol yöneticisi](../users-groups-roles/directory-assign-admin-roles.md#privileged-role-administrator) rolünün üyesi olan bir kullanıcıyla [Azure Portal](https://portal.azure.com/) oturum açın.

    PıM 'yi yönetmek için başka bir yöneticiye erişim verme hakkında daha fazla bilgi için bkz. [PIM 'yi yönetmek için diğer yöneticilere erişim verme](pim-how-to-give-access-to-pim.md).

1. **Azure AD Privileged Identity Management**açın.

    Azure portal henüz PıM 'yi başlatmadıysanız [PIM 'yi kullanmaya başlayın](pim-getting-started.md)' a gidin.

1. **Azure kaynakları**' na tıklayın.

1. Yönetilen kaynakların listesini filtrelemek için **kaynak filtresini** kullanın.

    ![Yönetilecek Azure kaynakları listesi](./media/pim-resource-roles-assign-roles/resources-list.png)

1. Yönetmek istediğiniz kaynağa (abonelik veya yönetim grubu gibi) tıklayın.

1. Yönet altında, Azure kaynakları rollerinin listesini görmek için **Roller** ' e tıklayın.

    ![Azure kaynakları rolleri](./media/pim-resource-roles-assign-roles/resources-roles.png)

1. Yeni atama bölmesini açmak için **üye Ekle** ' ye tıklayın.

1. Rol Seç bölmesini açmak için **Rol Seç** ' e tıklayın.

    ![Yeni atama bölmesi](./media/pim-resource-roles-assign-roles/resources-select-role.png)

1. Atamak istediğiniz role tıklayın ve ardından **Seç**' e tıklayın.

    Üye Seç veya grup bölmesi açılır.

1. Role atamak istediğiniz üye veya gruba tıklayın ve ardından **Seç**' e tıklayın.

    ![Üye veya grup bölmesi seçin](./media/pim-resource-roles-assign-roles/resources-select-member-or-group.png)

    Üyelik ayarları bölmesi açılır.

1. **Atama türü** listesinde **uygun** veya **etkin**' i seçin.

    ![Üyelik ayarları bölmesi](./media/pim-resource-roles-assign-roles/resources-membership-settings-type.png)

    Azure kaynakları için PıM, iki farklı atama türü sağlar:

    - **Uygun** atamalar, rolü kullanmak için bir eylem gerçekleştirmek üzere rolün üyesini gerektirir. Eylemler, bir Multi-Factor Authentication (MFA) denetimi gerçekleştirmeye, iş gerekçesinin sağlanmasından veya belirlenen onaylayanlardan onay isteğinde bulunabilir.

    - **Etkin** atamalar, üyenin rolü kullanmak için herhangi bir eylem gerçekleştirmesini gerektirmez. Etkin olarak atanan üyelerin her zaman role atanan ayrıcalıkları vardır.

1. Atama kalıcı olmalıdır (kalıcı olarak uygun veya kalıcı olarak atanabilir), **kalıcı** onay kutusunu seçin.

    Rol ayarlarına bağlı olarak, onay kutusu görünmeyebilir veya hiç değiştirilebilir olmayabilir.

1. Belirli bir atama süresi belirtmek için onay kutusunu temizleyin ve başlangıç ve/veya bitiş tarih ve saat kutularını değiştirin.

    ![Üyelik ayarları-Tarih ve saat](./media/pim-resource-roles-assign-roles/resources-membership-settings-date.png)

1. Tamamladığınızda **Bitti**’ye tıklayın.

    ![Yeni atama-Ekle](./media/pim-resource-roles-assign-roles/resources-new-assignment-add.png)

1. Yeni rol atamasını oluşturmak için **Ekle**' ye tıklayın. Durum bildirimi görüntülenir.

    ![Yeni atama-bildirim](./media/pim-resource-roles-assign-roles/resources-new-assignment-notification.png)

## <a name="update-or-remove-an-existing-role-assignment"></a>Var olan bir rol atamasını güncelleştirme veya kaldırma

Varolan bir rol atamasını güncelleştirmek veya kaldırmak için bu adımları izleyin.

1. **Azure AD Privileged Identity Management**açın.

1. **Azure kaynakları**' na tıklayın.

1. Yönetmek istediğiniz kaynağa (abonelik veya yönetim grubu gibi) tıklayın.

1. Yönet altında, Azure kaynakları rollerinin listesini görmek için **Roller** ' e tıklayın.

    ![Azure Kaynak rolleri-rol seçin](./media/pim-resource-roles-assign-roles/resources-update-select-role.png)

1. Güncelleştirmek veya kaldırmak istediğiniz role tıklayın.

1. **Uygun roller** veya **etkin roller** sekmelerinde rol atamasını bulun.

    ![Rol atamasını güncelleştirme veya kaldırma](./media/pim-resource-roles-assign-roles/resources-update-remove.png)

1. Rol atamasını güncelleştirmek veya kaldırmak için **Güncelleştir** ' e veya **Kaldır** ' a tıklayın.

    Rol atamasını genişletme hakkında daha fazla bilgi için bkz. [PIM 'de Azure Kaynak rollerini genişletme veya yenileme](pim-resource-roles-renew-extend.md).

## <a name="next-steps"></a>Sonraki adımlar

- [PıM 'de Azure Kaynak rollerini genişletme veya yenileme](pim-resource-roles-renew-extend.md)
- [PıM 'de Azure Kaynak rolü ayarlarını yapılandırma](pim-resource-roles-configure-role-settings.md)
- [PıM 'de Azure AD rolleri atama](pim-how-to-add-role-to-user.md)
