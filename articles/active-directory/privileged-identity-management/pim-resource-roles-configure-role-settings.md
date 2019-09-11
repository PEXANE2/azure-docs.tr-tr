---
title: PıM-Azure Active Directory 'de Azure Kaynak rolü ayarlarını yapılandırma | Microsoft Docs
description: Azure AD Privileged Identity Management (PıM) içinde Azure Kaynak rolü ayarlarını yapılandırmayı öğrenin.
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
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 88645d07ff213b0b7a56f2b081f0aaed01fd2929
ms.sourcegitcommit: 95b180c92673507ccaa06f5d4afe9568b38a92fb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/08/2019
ms.locfileid: "70804210"
---
# <a name="configure-azure-resource-role-settings-in-pim"></a>PıM 'de Azure Kaynak rolü ayarlarını yapılandırma

Azure Kaynak rolü ayarlarını yapılandırırken, Azure Active Directory (Azure AD) Privileged Identity Management (PıM) içindeki Azure Kaynak rol atamalarına uygulanan varsayılan ayarları tanımlarsınız. Onay iş akışını yapılandırmak ve istekleri onaylayabilecek veya reddedebilecek kişileri belirtmek için aşağıdaki yordamları kullanın.

## <a name="open-role-settings"></a>Rol ayarlarını aç

Azure Kaynak rolü ayarlarını açmak için aşağıdaki adımları izleyin.

1. [Ayrıcalıklı rol yöneticisi](../users-groups-roles/directory-assign-admin-roles.md#privileged-role-administrator) rolünün üyesi olan bir kullanıcıyla [Azure Portal](https://portal.azure.com/) oturum açın.

1. **Azure AD Privileged Identity Management**açın.

1. **Azure kaynakları**' na tıklayın.

1. Yönetmek istediğiniz kaynağa (abonelik veya yönetim grubu gibi) tıklayın.

    ![Azure kaynakları sayfası yönetilebilecek kaynakları listeleme](./media/pim-resource-roles-configure-role-settings/resources-list.png)

1. **Rol ayarları**' na tıklayın.

    ![Rol ayarları sayfası Azure Kaynak rollerini listeleme](./media/pim-resource-roles-configure-role-settings/resources-role-settings.png)

1. Ayarlarını yapılandırmak istediğiniz role tıklayın.

    ![Rol ayarı ayrıntıları sayfası çeşitli atama ve etkinleştirme ayarlarını listeleme](./media/pim-resource-roles-configure-role-settings/resources-role-setting-details.png)

1. Rol ayarları bölmesini açmak için **Düzenle** ' ye tıklayın.

    ![Rol ayarları sayfasını düzenleme atama ve etkinleştirme ayarlarını güncelleştirme seçenekleri](./media/pim-resource-roles-configure-role-settings/resources-role-settings-edit.png)

    Her rolün rol ayarı bölmesinde yapılandırabileceğiniz çeşitli ayarlar vardır.

## <a name="assignment-duration"></a>Atama süresi

Bir rolün ayarlarını yapılandırırken her atama türü için iki atama süresi seçeneğinden birini seçebilirsiniz (uygun ve etkin). Bu seçenekler, bir üyenin PıM 'de role atandığı varsayılan en uzun süre olur.

**Uygun** atama süresi seçeneklerinden birini belirleyebilirsiniz:

| | |
| --- | --- |
| **Kalıcı uygun atamaya izin ver** | Kaynak yöneticileri kalıcı uygun üyelik atayabilir. |
| **Uygun atamayı sona ermeden önce** | Kaynak yöneticileri, tüm uygun atamaların belirtilen başlangıç ve bitiş tarihine sahip olmasını gerektirebilir. |

Ve bu **etkin** atama süresi seçeneklerinden birini belirleyebilirsiniz:

| | |
| --- | --- |
| **Kalıcı etkin atamaya izin ver** | Kaynak yöneticileri kalıcı etkin üyelik atayabilir. |
| **Etkin atamanın son kullanım tarihi** | Kaynak yöneticileri, tüm etkin atamaların belirtilen başlangıç ve bitiş tarihine sahip olmasını gerektirebilir. |

> [!NOTE] 
> Belirtilen bitiş tarihine sahip tüm atamalar, kaynak yöneticileri tarafından yenilenebilir. Ayrıca, Üyeler [rol atamalarını genişletmek veya yenilemek](pim-resource-roles-renew-extend.md)için self servis istekleri başlatabilir.

## <a name="require-multi-factor-authentication"></a>Multi-Factor Authentication gerektir

PıM, iki ayrı senaryo için isteğe bağlı Azure Multi-Factor Authentication (MFA) zorlaması sağlar.

### <a name="require-multi-factor-authentication-on-active-assignment"></a>Etkin atama üzerinde Multi-Factor Authentication gerektir

Bazı durumlarda, bir rolün kısa bir süre (örneğin, bir gün) için bir üyeyi atamak isteyebilirsiniz. Bu durumda, etkinleştirme istemek için atanan üyelere gerek kalmaz. Bu senaryoda, üye rol atamasını kullandığında, bu rol atandığı andan itibaren zaten etkin olduklarından, PıM MFA 'yı zorunlu kılamaz.

Atamayı karşılayan kaynak yöneticisinin bunları söyledikleri kim olduğundan emin olmak için, **etkin atama üzerinde Multi-Factor Authentication gerektir** kutusunu işaretleyerek, etkin atama üzerinde MFA 'yı zorunlu kılabilirsiniz.

### <a name="require-multi-factor-authentication-on-activation"></a>Etkinleştirme üzerinde Multi-Factor Authentication gerektir

Etkinleştirilecek bir rolün uygun üyelerinin, etkinleştirebilmeniz için önce MFA 'yı çalıştırmasını isteyebilirsiniz. Bu işlem, etkinleştirme isteyen kullanıcının makul belirsizlik sağlar. Bu seçeneğin zorunlu kılınması, Kullanıcı hesabının tehlikeye girdiği durumlarda kritik kaynakları korur.

Etkinleştirmeden önce uygun bir üyenin MFA 'yı çalıştırmasını gerektirmek için **etkinleştirme Multi-Factor Authentication gerektir** kutusunu işaretleyin.

Daha fazla bilgi için bkz. [Multi-Factor Authentication (MFA) ve PIM](pim-how-to-require-mfa.md).

## <a name="activation-maximum-duration"></a>Maksimum etkinlik süresi

Rolün süresi dolmadan önce etkin kalacağı en uzun süreyi saat cinsinden ayarlamak için **etkinleştirme en uzun süre** kaydırıcısını kullanın. Bu değer 1 ile 24 saat arasında olabilir.

## <a name="require-justification"></a>Gerekçe gerektir

Üyelerin etkin atamaya veya etkinleştirildiklerinde bir gerekçe girmesini zorunlu kılabilirsiniz. Gerekçe gerektirmek için **etkin atamada gerekçe iste** kutusunu veya **etkinleştirme üzerinde gerekçe iste** kutusunu işaretleyin.

## <a name="require-approval-to-activate"></a>Etkinleştirmek için onay gerektir

Bir rolü etkinleştirmek için onay gerektirmek isterseniz, aşağıdaki adımları izleyin.

1. **Etkinleştirmek için onay gerektir** onay kutusunu işaretleyin.

1. Bir üye veya grup seçin bölmesini açmak için **onaylayanları Seç** ' e tıklayın.

    ![Onaylayanları seçmek için bir üye veya grup bölmesi seçin](./media/pim-resource-roles-configure-role-settings/resources-role-settings-select-approvers.png)

1. En az bir üye veya grup seçin ve ardından **Seç**' e tıklayın. Üyelerin ve grupların herhangi bir birleşimini ekleyebilirsiniz. En az bir onaylayan seçmelisiniz. Varsayılan onaylayanlar yok.

    Seçimleriniz seçili onaylayanlar listesinde görünür.

1. Tüm rol ayarlarınızı belirledikten sonra, değişikliklerinizi kaydetmek için **Güncelleştir** ' e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

- [PıM 'de Azure Kaynak rolleri atama](pim-resource-roles-assign-roles.md)
- [PıM 'de Azure Kaynak rolleri için güvenlik uyarılarını yapılandırma](pim-resource-roles-configure-alerts.md)
