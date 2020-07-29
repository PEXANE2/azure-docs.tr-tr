---
title: Yönetim birimi kapsamına (Önizleme) sahip rolleri atama ve listeleme-Azure Active Directory | Microsoft Docs
description: Azure Active Directory içindeki rol atamalarının kapsamını kısıtlamak için yönetim birimlerini kullanma
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
ms.service: active-directory
ms.topic: how-to
ms.subservice: users-groups-roles
ms.workload: identity
ms.date: 04/16/2020
ms.author: curtand
ms.reviewer: anandy
ms.custom: oldportal;it-pro;
ms.collection: M365-identity-device-management
ms.openlocfilehash: 71d0a1f2551338207a71b6c547d9c2cd57fea777
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87284396"
---
# <a name="assign-scoped-roles-to-an-administrative-unit"></a>Yönetim birimine kapsamlı roller atama

Azure Active Directory (Azure AD) ' de, daha ayrıntılı yönetim denetimi için kullanıcıları bir veya daha fazla yönetim birimiyle (AU) sınırlı bir kapsamla bir Azure AD rolüne atayabilirsiniz.

PowerShell 'i kullanmaya hazırlanma ve yönetim birimi yönetimi için Microsoft Graph adımlar için bkz. [kullanmaya başlayın](roles-admin-units-manage.md#get-started).

## <a name="roles-available"></a>Kullanılabilir roller

Rol  |  Açıklama
----- |  -----------
Kimlik doğrulama Yöneticisi  |  Yalnızca atanan yönetim birimindeki yönetici olmayan kullanıcılar için kimlik doğrulama yöntemi bilgilerini görüntüleme, ayarlama ve sıfırlama erişimine sahiptir.
Grup Yöneticisi  |  , Yalnızca atanan yönetim birimindeki adlandırma ve süre sonu ilkeleri gibi grupların ve grupların ayarlarının tüm yönlerini yönetebilir.
Yardım Masası Yöneticisi  |  , Yalnızca atanan yönetim birimindeki yönetici olmayanlar ve Yardım Masası yöneticileri için parolaları sıfırlayabilir.
Lisans Yöneticisi  |  , Lisans atamalarını yalnızca yönetim birimi içinde atayabilir, kaldırabilir ve güncelleştirebilir.
Parola Yöneticisi  |  , Yalnızca atanan yönetim birimi içindeki yönetici olmayanlar ve parola yöneticileri için parolaları sıfırlayabilir.
Kullanıcı Yöneticisi  |  , Yalnızca atanan yönetim birimi içinde sınırlı yöneticiler için parola sıfırlama dahil olmak üzere kullanıcıların ve grupların tüm yönlerini yönetebilir.

## <a name="assign-a-scoped-role"></a>Kapsamlı bir rol atama

### <a name="azure-portal"></a>Azure portal

Portalda **Azure AD > yönetim birimleri** ' ne gidin. Rolü bir kullanıcıya atamak istediğiniz yönetim birimini seçin. Sol bölmede, tüm kullanılabilir rolleri listelemek için roller ve yöneticiler ' i seçin.

![Rol kapsamını değiştirmek için bir yönetim birimi seçin](./media/roles-admin-units-assign-roles/select-role-to-scope.png)

Atanacak rolü seçin ve ardından **atama Ekle**' yi seçin. Bu işlem, role atanacak bir veya daha fazla kullanıcıyı seçebileceğiniz sağ tarafta bir panel açar.

![Kapsam için rol seçin ve sonra atama Ekle ' yi seçin.](./media/roles-admin-units-assign-roles/select-add-assignment.png)

### <a name="powershell"></a>PowerShell

```powershell
$AdminUser = Get-AzureADUser -ObjectId "Use the user's UPN, who would be an admin on this unit"
$Role = Get-AzureADDirectoryRole | Where-Object -Property DisplayName -EQ -Value "User Account Administrator"
$administrativeUnit = Get-AzureADAdministrativeUnit -Filter "displayname eq 'The display name of the unit'"
$RoleMember = New-Object -TypeName Microsoft.Open.AzureAD.Model.RoleMemberInfo
$RoleMember.ObjectId = $AdminUser.ObjectId
Add-AzureADScopedRoleMembership -ObjectId $administrativeUnit.ObjectId -RoleObjectId $Role.ObjectId -RoleMemberInfo $RoleMember
```

Vurgulanan bölüm, belirli bir ortam için gerektiği şekilde değiştirilebilir.

### <a name="microsoft-graph"></a>Microsoft Graph

```http
Http request
POST /administrativeUnits/{id}/scopedRoleMembers
    
Request body
{
  "roleId": "roleId-value",
  "roleMemberInfo": {
    "id": "id-value"
  }
}
```

## <a name="list-the-scoped-admins-on-an-au"></a>Bir AU 'da kapsamlı yöneticileri listeleme

### <a name="azure-portal"></a>Azure portal

Bir yönetim birimi kapsamıyla gerçekleştirilen tüm rol atamaları, [Azure AD 'Nin yönetim birimleri bölümünde](https://ms.portal.azure.com/?microsoft_aad_iam_adminunitprivatepreview=true&microsoft_aad_iam_rbacv2=true#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/AdminUnit)görüntülenebilir. Portalda **Azure AD > yönetim birimleri** ' ne gidin. Listelemek istediğiniz rol atamaları için yönetim birimini seçin. **Roller ve yöneticiler** ' i seçin ve yönetici birimindeki atamaları görüntülemek için bir rol açın.

### <a name="powershell"></a>PowerShell

```powershell
$administrativeUnit = Get-AzureADAdministrativeUnit -Filter "displayname eq 'The display name of the unit'"
Get-AzureADScopedRoleMembership -ObjectId $administrativeUnit.ObjectId | fl *
```

Vurgulanan bölüm, belirli bir ortam için gerektiği şekilde değiştirilebilir.

### <a name="microsoft-graph"></a>Microsoft Graph

```http
Http request
GET /administrativeUnits/{id}/scopedRoleMembers
Request body
{}
```

## <a name="next-steps"></a>Sonraki adımlar

- [Yönetim birimleri sorun giderme ve SSS](roles-admin-units-faq-troubleshoot.md)
