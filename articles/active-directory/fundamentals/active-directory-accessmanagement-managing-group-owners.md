---
title: Grup sahiplerine ekleme veya kaldırma - Azure Etkin Dizini | Microsoft Dokümanlar
description: Azure Active Directory'yi kullanarak grup sahiplerinin nasıl ekleyeceğiniz veya kaldırılacağına ilişkin yönergeler.
services: active-directory
author: msaburnley
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 09/11/2018
ms.author: ajburnle
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: f83a48ad13fe4ae217319b2aa85adf976aa6a7ae
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77149856"
---
# <a name="add-or-remove-group-owners-in-azure-active-directory"></a>Azure Etkin Dizini'nde grup sahipleri ekleme veya kaldırma
Azure Etkin Dizin (Azure AD) grupları grup sahiplerine aittir ve yönetilir. Grup sahipleri kullanıcı veya hizmet sorumlusu olabilir ve üyelik de dahil olmak üzere grubu yönetebilir. Grup sahiplerini yalnızca varolan grup sahipleri veya grup yönetimi yöneticileri atayabilir. Grup sahiplerinin grubun üyesi olması gerekmez.

Bir grubun sahibi yoksa, grup yönetimi yöneticileri grubu yönetmeye devam edebilir. Her grubun en az bir sahibi olması önerilir. Sahipler bir gruba assgined sonra, grubun son sahibi kaldırılamaz. Lütfen son sahibini gruptan çıkarmadan önce başka bir sahip seçtiğinizden emin olun.

## <a name="add-an-owner-to-a-group"></a>Gruba sahip ekleme
Aşağıda, Azure AD portalını kullanan bir gruba kullanıcıyı sahip olarak ekleme yönergeleri verilmiştir. Bir grubun sahibi olarak bir hizmet müdürü eklemek için [PowerShell'i](https://docs.microsoft.com/powershell/module/Azuread/Add-AzureADGroupOwner?view=azureadps-2.0)kullanarak yönergeleri izleyin.

### <a name="to-add-a-group-owner"></a>Grup sahibi eklemek için
1. Dizin için bir Genel yönetici hesabı kullanarak [Azure portalda](https://portal.azure.com) oturum açın.

2. **Azure Etkin Dizini'ni**seçin, **Gruplar'ı**seçin ve ardından bir sahip eklemek istediğiniz grubu seçin (bu örnek *için, MDM ilkesi - Batı).*

3. **MDM politikasında - Batı genel bakış** **sayfasında, Sahipleri**seçin.

    ![MDM ilkesi - Sahipleri seçeneği vurgulanan Batı Genel Bakış sayfası](media/active-directory-accessmanagement-managing-group-owners/add-owners-option-overview-blade.png)

4. **MDM ilkesinde - Batı - Sahipleri** sayfasında, Sahipleri **Ekle'yi**seçin ve ardından yeni grup sahibi olacak kullanıcıyı arayıp seçin ve sonra **Seç'i**seçin.

    ![MDM politikası - Batı - Sahipleri ekle seçeneği vurgulanan sahipleri sayfası](media/active-directory-accessmanagement-managing-group-owners/add-owners-owners-blade.png)

    Yeni sahibi ni seçtikten **sonra, Sahipler** sayfasını yenileyebilir ve sahipler listesine eklenen adı görebilirsiniz.

## <a name="remove-an-owner-from-a-group"></a>Sahibini gruptan kaldırma
Azure AD'yi kullanarak bir grup sahibini gruptan kaldırın.

### <a name="to-remove-an-owner"></a>Sahibini kaldırmak için
1. Dizin için bir Genel yönetici hesabı kullanarak [Azure portalda](https://portal.azure.com) oturum açın.

2. **Azure Etkin Dizini'ni**seçin, **Gruplar'ı**seçin ve ardından sahibini kaldırmak istediğiniz grubu seçin (bu örnek için *MDM ilkesi - Batı).*

3. **MDM politikasında - Batı genel bakış** **sayfasında, Sahipleri**seçin.

    ![MDM ilkesi - Sahipleri seçeneği vurgulanan Batı Genel Bakış sayfası](media/active-directory-accessmanagement-managing-group-owners/remove-owners-option-overview-blade.png)

4. **MDM ilkesinde - Batı - Sahipler** sayfasında, grup sahibi olarak kaldırmak istediğiniz kullanıcıyı seçin, kullanıcının bilgi sayfasından **kaldır'ı** seçin ve kararınızı onaylamak için **Evet'i** seçin.

    ![Kaldır seçeneği vurgulanmış kullanıcı bilgileri sayfası](media/active-directory-accessmanagement-managing-group-owners/remove-owner-info-blade.png)

    Sahibini kaldırdıktan sonra, **Sahipler** sayfasına geri dönebilir ve adın sahipler listesinden kaldırıldığını görebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
- [Azure Active Directory grupları ile kaynaklara erişimi yönetme](active-directory-manage-groups.md)

- [Grup ayarlarını yapılandırmak için Azure Active Directory cmdlet'leri](../users-groups-roles/groups-settings-cmdlets.md)

- [Tümleşik bir SaaS uygulamasına erişim atamak için grupları kullanma](../users-groups-roles/groups-saasapps.md)

- [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](../hybrid/whatis-hybrid-identity.md)

- [Grup ayarlarını yapılandırmak için Azure Active Directory cmdlet'leri](../users-groups-roles/groups-settings-v2-cmdlets.md)
