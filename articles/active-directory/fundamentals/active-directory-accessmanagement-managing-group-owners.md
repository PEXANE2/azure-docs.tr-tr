---
title: Grup sahiplerini ekleme veya kaldırma-Azure Active Directory | Microsoft Docs
description: Azure Active Directory kullanarak Grup sahiplerini ekleme veya kaldırma hakkında yönergeler.
services: active-directory
author: ajburnle
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: how-to
ms.date: 09/11/2018
ms.author: ajburnle
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 30b547ecd4ff76e650d7be837fa5bce03046da50
ms.sourcegitcommit: 5ed504a9ddfbd69d4f2d256ec431e634eb38813e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89318599"
---
# <a name="add-or-remove-group-owners-in-azure-active-directory"></a>Azure Active Directory Grup sahiplerini ekleme veya kaldırma
Azure Active Directory (Azure AD) grupları, Grup sahiplerine aittir ve yönetilir. Grup sahipleri Kullanıcı veya hizmet sorumluları olabilir ve üyeliği dahil olmak üzere grubu yönetebilir. Yalnızca var olan Grup sahipleri veya grupları-Yöneticiler, Grup sahipleri atayabilir. Grup sahiplerinin grubun üyesi olması gerekmez.

Bir grubun sahibi olmadığında, Grup Yönetimi Yöneticiler grubu yine de yönetebilecektir. Her grubun en az bir sahibi olması önerilir. Sahipler bir gruba alındıktan sonra grubun son sahibi kaldırılamaz. Son sahibi gruptan kaldırmadan önce lütfen başka bir sahip seçtiğinizden emin olun.

## <a name="add-an-owner-to-a-group"></a>Gruba sahip ekleme
Aşağıda, bir kullanıcıyı Azure AD portalını kullanarak bir gruba sahip olarak ekleme yönergeleri verilmiştir. Bir hizmet sorumlusunu bir grubun sahibi olarak eklemek için, [PowerShell](/powershell/module/Azuread/Add-AzureADGroupOwner?view=azureadps-2.0)'i kullanarak bunu yapmak için yönergeleri izleyin.

### <a name="to-add-a-group-owner"></a>Grup sahibi eklemek için
1. Dizin için bir Genel yönetici hesabı kullanarak [Azure portalda](https://portal.azure.com) oturum açın.

2. **Azure Active Directory**, **gruplar**' ı seçin ve ardından sahip eklemek istediğiniz grubu seçin (Bu örnek Için *MDM ilkesi-Batı*).

3. **MDM ilkesi-Batı genel bakış** sayfasında **sahipler**' i seçin.

    ![MDM ilkesi--sahipler seçeneğinin vurgulandığı Batı genel bakış sayfası](media/active-directory-accessmanagement-managing-group-owners/add-owners-option-overview-blade.png)

4. **MDM ilkesi-Batı-sahipler** sayfasında, **sahip Ekle**' yi seçin ve ardından yeni Grup sahibi olacak kullanıcıyı arayıp seçin ve ardından **Seç**' i seçin.

    ![MDM ilkesi-sahip Ekle seçeneği vurgulanmış olan Batı-sahipler sayfası](media/active-directory-accessmanagement-managing-group-owners/add-owners-owners-blade.png)

    Yeni sahibi seçtikten sonra, **sahipler** sayfasını yenileyebilir ve sahipler listesine eklenen adı görebilirsiniz.

## <a name="remove-an-owner-from-a-group"></a>Bir gruptan sahip kaldırma
Azure AD 'yi kullanarak bir kullanıcıyı gruptan kaldırın.

### <a name="to-remove-an-owner"></a>Bir sahibi kaldırmak için
1. Dizin için bir Genel yönetici hesabı kullanarak [Azure portalda](https://portal.azure.com) oturum açın.

2. **Azure Active Directory**, **gruplar**' ı seçin ve ardından sahibini kaldırmak istediğiniz grubu seçin (Bu örnek Için *MDM ilkesi-Batı*).

3. **MDM ilkesi-Batı genel bakış** sayfasında **sahipler**' i seçin.

    ![MDM ilkesi--sahipler seçeneğinin vurgulandığı Batı genel bakış sayfası](media/active-directory-accessmanagement-managing-group-owners/remove-owners-option-overview-blade.png)

4. **MDM ilkesi-Batı-sahipler** sayfasında, Grup sahibi olarak kaldırmak istediğiniz kullanıcıyı seçin, kullanıcının bilgileri sayfasından **Kaldır** ' ı seçin ve kararlarınızı onaylamak için **Evet** ' i seçin.

    ![Kaldırma seçeneği vurgulanmış kullanıcının bilgi sayfası](media/active-directory-accessmanagement-managing-group-owners/remove-owner-info-blade.png)

    Sahibi kaldırdıktan sonra, **sahipler** sayfasına dönüp adın sahipler listesinden kaldırıldığını görebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
- [Azure Active Directory grupları ile kaynaklara erişimi yönetme](active-directory-manage-groups.md)

- [Grup ayarlarını yapılandırmak için Azure Active Directory cmdlet'leri](../users-groups-roles/groups-settings-cmdlets.md)

- [Tümleşik bir SaaS uygulamasına erişim atamak için grupları kullanma](../users-groups-roles/groups-saasapps.md)

- [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](../hybrid/whatis-hybrid-identity.md)

- [Grup ayarlarını yapılandırmak için Azure Active Directory cmdlet'leri](../users-groups-roles/groups-settings-v2-cmdlets.md)