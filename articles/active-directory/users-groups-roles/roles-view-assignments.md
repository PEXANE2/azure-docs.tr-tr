---
title: Azure AD portalında özel rol atamalarını görüntüleyin | Microsoft Dokümanlar
description: Azure AD yönetici merkezinde artık bir Azure AD yöneticisi rolünün üyelerini görebilir ve yönetebilirsiniz.
services: active-directory
author: curtand
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 11/08/2019
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: f04bd7788a9cc9657e14aedfb153182d6e53eb95
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79259714"
---
# <a name="view-custom-role-assignments-in-azure-active-directory"></a>Azure Etkin Dizini'nde özel rol atamalarını görüntüleme

Bu makalede, Azure Etkin Dizini'nde (Azure AD) atadığınız özel rollerin nasıl görüntüleneniz açıklanmaktadır. Azure Etkin Dizini'nde (Azure AD), roller kuruluş genelinde veya tek uygulama kapsamında atanabilir.

- Kuruluş genelindeki rol atamaları eklenir ve tek bir uygulama rolü atamaları listesinde görülebilir.
- Tek uygulama kapsamındaki rol atamaları eklenmez ve kuruluş genelinde ki kapsamlı atamalar listesinde görülemez.

## <a name="view-role-assignments-in-the-azure-portal"></a>Azure portalında rol atamalarını görüntüleme

Bu yordam, kuruluş genelinde kapsamı olan bir rolün atamalarını görüntülemeyi açıklar.

1. Azure AD kuruluşunda Ayrıcalıklı rol yöneticisi veya Global yönetici izinleriyle [Azure AD yönetici merkezinde](https://aad.portal.azure.com) oturum açın.
1. **Azure Etkin Dizini'ni**seçin, **Roller'i ve yöneticileri**seçin ve ardından açmak ve özelliklerini görüntülemek için bir rol seçin.
1. Rolün atamalarını görüntülemek için **Atamalar'ı** seçin.

    ![Bir rolü listeden açtığınızda rol atamalarını ve izinleri görüntüleme](./media/roles-view-assignments/role-assignments.png)

## <a name="view-role-assignments-using-azure-ad-powershell"></a>Azure AD PowerShell'i kullanarak rol atamalarını görüntüleme

Bu bölümde, kuruluş genelinde kapsamı olan bir rolün atamalarını görüntüleme açıklanmaktadır. Bu makalede, [Azure Active Directory PowerShell Sürüm 2](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0#directory_roles) modülü kullanılır. PowerShell'i kullanarak tek uygulama kapsamı atamalarını görüntülemek için [PowerShell ile özel roller atamada](roles-assign-powershell.md)cmdlet'leri kullanabilirsiniz.

### <a name="prepare-powershell"></a>PowerShell Hazırlayın

İlk olarak, [Azure AD önizleme PowerShell modüllerini indirmeniz](https://www.powershellgallery.com/packages/AzureAD/)gerekir.

Azure AD PowerShell modüllerini yüklemek için aşağıdaki komutları kullanın:

``` PowerShell
install-module azureadpreview
import-module azureadpreview
```

Modülün kullanıma hazır olup olmadığını doğrulamak için aşağıdaki komutu kullanın:

``` PowerShell
get-module azuread
  ModuleType Version      Name                         ExportedCommands
  ---------- ---------    ----                         ----------------
  Binary     2.0.0.115    azuread                      {Add-AzureADAdministrati...}
```

### <a name="view-the-assignments-of-a-role"></a>Rolün atamalarını görüntüleme

Bir rolün atamalarını görüntüleme örneği.

``` PowerShell
# Fetch list of all directory roles with object ID
Get-AzureADDirectoryRole

# Fetch a specific directory role by ID
$role = Get-AzureADDirectoryRole -ObjectId "5b3fe201-fa8b-4144-b6f1-875829ff7543"

# Fetch role membership for a role
Get-AzureADDirectoryRoleMember -ObjectId $role.ObjectId | Get-AzureADUser
```

## <a name="view-role-assignments-using-microsoft-graph-api"></a>Microsoft Graph API'yi kullanarak rol atamalarını görüntüleme

Bu bölümde, kuruluş genelinde kapsamı olan bir rolün atamalarını görüntüleme açıklanmaktadır.  Grafik API'yi kullanarak tek uygulama kapsamı atamalarını görüntülemek için, [Graph API ile özel roller atama](roles-assign-graph.md)işlemlerini kullanabilirsiniz.

HTTP belirli bir rol tanımı için bir rol ataması almak için istek.

GET

``` HTTP
https://graph.microsoft.com/beta/roleManagement/directory/roleAssignments&$filter=roleDefinitionId eq ‘<object-id-or-template-id-of-role-definition>’
```

Yanıt

``` HTTP
HTTP/1.1 200 OK
{
    "id":"CtRxNqwabEKgwaOCHr2CGJIiSDKQoTVJrLE9etXyrY0-1"
    "principalId":"ab2e1023-bddc-4038-9ac1-ad4843e7e539",
    "roleDefinitionId":"3671d40a-1aac-426c-a0c1-a3821ebd8218",
    "resourceScopes":["/"]
}
```

## <a name="view-assignments-of-single-application-scope"></a>Tek uygulama kapsamının atamalarını görüntüleme

Bu bölümde, tek uygulama kapsamı olan bir rolün atamaları görüntüleme açıklanmaktadır. Bu özellik şu anda genel önizlemede dir.

1. Azure AD kuruluşunda Ayrıcalıklı rol yöneticisi veya Global yönetici izinleriyle [Azure AD yönetici merkezinde](https://aad.portal.azure.com) oturum açın.
1. **Uygulama kayıtlarını**seçin ve özelliklerini görüntülemek için uygulama kaydını seçin. Azure REKLAM kuruluşunuzdaki uygulama kayıtlarının tam listesini görmek için **Tüm uygulamaları** seçmeniz gerekebilir.

    ![Uygulama kayıtları sayfasından uygulama kayıtları oluşturma veya oluşturma](./media/roles-create-custom/appreg-all-apps.png)

1. Uygulama kaydında, **Roller ve yöneticiler'i**seçin ve sonra özelliklerini görüntülemek için bir rol seçin.

    ![Uygulama kayıtları sayfasından uygulama kaydı rol atamaları görüntüleme](./media/roles-view-assignments/appreg-assignments.png)

1. Rolün atamalarını görüntülemek için **Atamalar'ı** seçin. Uygulama kaydının içinden atamalar görünümünü açmak, bu Azure AD kaynağına kapsamlı atamaları gösterir.

    ![Uygulama kaydının özelliklerinden uygulama kaydı rol atamalarını görüntüleme](./media/roles-view-assignments/appreg-assignments-2.png)

## <a name="next-steps"></a>Sonraki adımlar

* Azure AD yönetim rolleri [forumunda](https://feedback.azure.com/forums/169401-azure-active-directory?category_id=166032)bizimle paylaşmakta çekinmeyin.
* Roller ve Yönetici rol ataması hakkında daha fazla şey için [yönetici rollerini atay'a](directory-assign-admin-roles.md)bakın.
* Varsayılan kullanıcı izinleri için varsayılan [konuk ve üye kullanıcı izinlerinin karşılaştırılmasına](../fundamentals/users-default-permissions.md)bakın.
