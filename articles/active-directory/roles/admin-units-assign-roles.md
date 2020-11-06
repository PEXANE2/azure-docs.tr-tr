---
title: Yönetim birimi kapsamına sahip rolleri atama ve listeleme-Azure Active Directory | Microsoft Docs
description: Azure Active Directory içindeki rol atamalarının kapsamını kısıtlamak için yönetim birimlerini kullanın.
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
ms.service: active-directory
ms.topic: how-to
ms.subservice: roles
ms.workload: identity
ms.date: 11/04/2020
ms.author: curtand
ms.reviewer: anandy
ms.custom: oldportal;it-pro;
ms.collection: M365-identity-device-management
ms.openlocfilehash: 73ed2741c7dd754127a57642703b650a70637c63
ms.sourcegitcommit: 0ce1ccdb34ad60321a647c691b0cff3b9d7a39c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2020
ms.locfileid: "93393447"
---
# <a name="assign-scoped-roles-to-an-administrative-unit"></a>Yönetim birimine kapsamlı roller atama

Azure Active Directory (Azure AD) içinde, daha ayrıntılı yönetim denetimi için, bir veya daha fazla yönetim birimiyle sınırlı bir kapsama sahip bir Azure AD rolüne kullanıcı atayabilirsiniz.

Yönetim birimi yönetimi için PowerShell ve Microsoft Graph kullanmaya hazırlanmak için bkz. [kullanmaya başlayın](admin-units-manage.md#get-started).

## <a name="available-roles"></a>Kullanılabilir roller

Rol  |  Açıklama
----- |  -----------
Kimlik doğrulama Yöneticisi  |  Yalnızca atanan yönetim birimindeki yönetici olmayan kullanıcılar için kimlik doğrulama yöntemi bilgilerini görüntüleme, ayarlama ve sıfırlama erişimine sahiptir.
Grup Yöneticisi  |  , Yalnızca atanan yönetim birimindeki adlandırma ve sona erme ilkeleri gibi grupların ve grupların ayarlarının tüm yönlerini yönetebilir.
Yardım Masası Yöneticisi  |  , Yalnızca atanan yönetim birimindeki yönetici olmayanlar ve Yardım Masası yöneticileri için parolaları sıfırlayabilir.
Lisans Yöneticisi  |  , Lisans atamalarını yalnızca yönetim birimi içinde atayabilir, kaldırabilir ve güncelleştirebilir.
Parola Yöneticisi  |  , Yalnızca atanan yönetim birimi içindeki yönetici olmayanlar ve parola yöneticileri için parolaları sıfırlayabilir.
Kullanıcı Yöneticisi  |  , Yalnızca atanan yönetim birimi içinde sınırlı yöneticiler için parola sıfırlama dahil olmak üzere kullanıcıların ve grupların tüm yönlerini yönetebilir.

## <a name="security-principals-that-can-be-assigned-to-a-scoped-role"></a>Kapsamlı bir role atanabilecek güvenlik sorumluları

Aşağıdaki güvenlik sorumluları, yönetim birimi kapsamına sahip bir role atanabilir:

* Kullanıcılar
* Rol atanabilir bulut grupları (Önizleme)
* Hizmet Asıl Adı (SPN)

## <a name="assign-a-scoped-role"></a>Kapsamlı bir rol atama

Azure portal, PowerShell veya Microsoft Graph kullanarak kapsamlı bir rol atayabilirsiniz.

### <a name="use-the-azure-portal"></a>Azure portalı kullanma

1. Azure portal **Azure AD** 'ye gidin.

1. **Yönetim birimleri** ' ni seçin ve ardından bir kullanıcı rolü kapsamı atamak istediğiniz yönetim birimini seçin. 

1. Sol bölmede, tüm kullanılabilir rolleri listelemek için **Roller ve yöneticiler** ' i seçin.

   ![Rol kapsamını atamak istediğiniz yönetim birimini seçmek için "rol ve Yöneticiler" bölmesinin ekran görüntüsü.](./media/admin-units-assign-roles/select-role-to-scope.png)

1. Atanacak rolü seçin ve ardından **atama Ekle** ' yi seçin. 

1. **Atama Ekle** bölmesinde, role atanacak bir veya daha fazla kullanıcı seçin.

   ![Kapsam için rol seçin ve sonra atama Ekle ' yi seçin.](./media/admin-units-assign-roles/select-add-assignment.png)

> [!Note]
> Bir yönetim biriminde Azure AD Privileged Identity Management (PıM) kullanarak bir rol atamak için bkz. [PIM 'de Azure AD rolleri atama](../privileged-identity-management/pim-how-to-add-role-to-user.md?tabs=new#assign-a-role-with-restricted-scope).

### <a name="use-powershell"></a>PowerShell kullanma

```powershell
$AdminUser = Get-AzureADUser -ObjectId "Use the user's UPN, who would be an admin on this unit"
$Role = Get-AzureADDirectoryRole | Where-Object -Property DisplayName -EQ -Value "User Account Administrator"
$administrativeUnit = Get-AzureADMSAdministrativeUnit -Filter "displayname eq 'The display name of the unit'"
$RoleMember = New-Object -TypeName Microsoft.Open.AzureAD.Model.RoleMemberInfo
$RoleMember.ObjectId = $AdminUser.ObjectId
Add-AzureADMSScopedRoleMembership -ObjectId $administrativeUnit.ObjectId -RoleObjectId $Role.ObjectId -RoleMemberInfo $RoleMember
```

Vurgulanan bölümü, belirli bir ortam için gereken şekilde değiştirebilirsiniz.

### <a name="use-microsoft-graph"></a>Microsoft Graph kullan

```http
Http request
POST /directory/administrativeUnits/{id}/scopedRoleMembers
    
Request body
{
  "roleId": "roleId-value",
  "roleMemberInfo": {
    "id": "id-value"
  }
}
```

## <a name="view-a-list-of-the-scoped-admins-in-an-administrative-unit"></a>Yönetim birimindeki kapsamlı yöneticilerin listesini görüntüleme

Kapsamlı yöneticilerin listesini Azure portal, PowerShell veya Microsoft Graph kullanarak görüntüleyebilirsiniz.

### <a name="use-the-azure-portal"></a>Azure portalı kullanma

Yönetim birimi kapsamıyla oluşturulan tüm rol atamalarını [Azure AD 'Nin yönetim birimleri bölümünde](https://ms.portal.azure.com/?microsoft_aad_iam_adminunitprivatepreview=true&microsoft_aad_iam_rbacv2=true#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/AdminUnit)görüntüleyebilirsiniz. 

1. Azure portal **Azure AD** 'ye gidin.

1. Sol bölmede **yönetim birimleri** ' ni seçin ve ardından görüntülemek istediğiniz rol atamalarının listesi için yönetim birimini seçin. 

1. **Roller ve yöneticiler** ' i seçin ve ardından yönetim biriminde atamaları görüntülemek için bir rol açın.

### <a name="use-powershell"></a>PowerShell kullanma

```powershell
$administrativeUnit = Get-AzureADMSAdministrativeUnit -Filter "displayname eq 'The display name of the unit'"
Get-AzureADMSScopedRoleMembership -ObjectId $administrativeUnit.ObjectId | fl *
```

Vurgulanan bölümü, belirli ortamınız için gerektiği gibi değiştirebilirsiniz.

### <a name="use-microsoft-graph"></a>Microsoft Graph kullan

```http
Http request
GET /directory/administrativeUnits/{id}/scopedRoleMembers
Request body
{}
```

## <a name="next-steps"></a>Sonraki adımlar

- [Rol atamalarını yönetmek için bulut gruplarını kullanma](groups-concept.md)
- [Bulut gruplarına atanan rollerin sorunlarını giderme](groups-faq-troubleshooting.md)
