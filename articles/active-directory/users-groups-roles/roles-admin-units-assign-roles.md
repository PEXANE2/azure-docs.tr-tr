---
title: Yönetim birim kapsamı (önizleme) ile rolleri atama ve listele - Azure Etkin Dizini | Microsoft Dokümanlar
description: Azure Etkin Dizini'nde rol atamalarının kapsamını kısıtlamak için yönetim birimlerini kullanma
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
ms.service: active-directory
ms.topic: article
ms.subservice: users-groups-roles
ms.workload: identity
ms.date: 04/16/2020
ms.author: curtand
ms.reviewer: anandy
ms.custom: oldportal;it-pro;
ms.collection: M365-identity-device-management
ms.openlocfilehash: c900b2a363a4ff271e7a436b358ecf170daca163
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81687032"
---
# <a name="assign-scoped-roles-to-an-administrative-unit"></a>Kapsamlı rolleri bir yönetim birimine atama

Azure Etkin Dizini'nde (Azure AD), daha ayrıntılı yönetim denetimi için kullanıcıları bir veya daha fazla yönetim birimiyle (AUs) sınırlı bir kapsamla Azure REKLAM rolüne atayabilirsiniz.

Yönetim birimi yönetimi için PowerShell ve Microsoft Graph'ı kullanmaya hazırlanma adımları [için](roles-admin-units-manage.md#get-started)bkz.

## <a name="roles-available"></a>Kullanılabilir roller

Rol  |  Açıklama
----- |  -----------
Kimlik Doğrulama Yöneticisi  |  Yalnızca atanan yönetim biriminde yönetici olmayan herhangi bir kullanıcı için kimlik doğrulama yöntemi bilgilerini görüntüleme, ayarlama ve sıfırlama yöntemi bilgilerine erişebilir.
Gruplar Yöneticisi  |  Yalnızca atanan yönetim biriminde adlandırma ve son kullanma ilkeleri gibi grup ve grup ayarlarının tüm yönlerini yönetebilir.
Yardım Masası Yöneticisi  |  Yalnızca atanan yönetim biriminde yönetici olmayan ların ve Yardım Masası yöneticilerinin parolalarını sıfırlayabilir.
Lisans Yöneticisi  |  Lisans atamalarını yalnızca yönetim birimi içinde atayabilir, kaldırabilir ve güncelleyebilir.
Şifre Yöneticisi  |  Yalnızca atanan yönetim birimi içinde yönetici olmayanların ve Parola Yöneticilerinin parolalarını sıfırlayabilir.
Kullanıcı Yöneticisi  |  Yalnızca atanan yönetim birimi içindeki sınırlı yöneticilerin parolalarını sıfırlama da dahil olmak üzere kullanıcıların ve grupların tüm yönlerini yönetebilirsiniz.

## <a name="assign-a-scoped-role"></a>Kapsamlı bir rol atama

### <a name="azure-portal"></a>Azure portal

Portaldaki **Azure AD > Yönetim birimlerine** gidin. Rolü bir kullanıcıya atamak istediğiniz yönetim birimini seçin. Sol bölmede, kullanılabilir tüm rolleri listelemek için Roller'i ve yöneticileri seçin.

![Rol kapsamını değiştirmek için bir yönetim birimi seçme](./media/roles-admin-units-assign-roles/select-role-to-scope.png)

Atanacak rolü seçin ve ardından **atamaekle'yi**seçin. Bu, sağ tarafta, role atanacak bir veya daha fazla kullanıcı seçebileceğiniz bir paneli açar.

![Rolü kapsam alavesin ve sonra atama ekle'yi seçin](./media/roles-admin-units-assign-roles/select-add-assignment.png)

### <a name="powershell"></a>PowerShell

    $administrative unitObj = Get-AzureADAdministrativeUnit -Filter "displayname eq 'Test administrative unit 2'"
    $AdminUser = Get-AzureADUser -ObjectId 'janedoe@fabidentity.onmicrosoft.com'
    $uaRoleMemberInfo = New-Object -TypeName Microsoft.Open.AzureAD.Model.RoleMemberInfo -Property @{ObjectId = $AdminUser.ObjectId}
    Add-AzureADScopedRoleMembership -RoleObjectId $UserAdminRole.ObjectId -ObjectId $administrative unitObj.ObjectId -RoleMemberInfo  $uaRoleMemberInfo

Vurgulanan bölüm, belirli bir ortam için gerektiği gibi değiştirilebilir.

### <a name="microsoft-graph"></a>Microsoft Graph

    Http request
    POST /administrativeUnits/{id}/scopedRoleMembers
    
    Request body
    {
      "roleId": "roleId-value",
      "roleMemberInfo": {
        "id": "id-value"
      }
    }

## <a name="list-the-scoped-admins-on-an-au"></a>Kapsamlı yöneticileri BIR AU'da listele

### <a name="azure-portal"></a>Azure portal

Yönetim birimi kapsamıyla yapılan tüm rol atamaları Azure [AD'nin Yönetim birimleri bölümünde](https://ms.portal.azure.com/?microsoft_aad_iam_adminunitprivatepreview=true&microsoft_aad_iam_rbacv2=true#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/AdminUnit)görüntülenebilir. Portaldaki **Azure AD > Yönetim birimlerine** gidin. Listelemek istediğiniz rol atamaları için yönetici birimini seçin. **Roller'i ve yöneticileri** seçin ve yönetici birimindeki atamaları görüntülemek için bir rol açın.

### <a name="powershell"></a>PowerShell

    $administrative unitObj = Get-AzureADAdministrativeUnit -Filter "displayname eq 'Test administrative unit 2'"
    Get-AzureADScopedRoleMembership -ObjectId $administrative unitObj.ObjectId | fl *

Vurgulanan bölüm, belirli bir ortam için gerektiği gibi değiştirilebilir.

### <a name="microsoft-graph"></a>Microsoft Graph

    Http request
    GET /administrativeUnits/{id}/scopedRoleMembers
    Request body
    {}

## <a name="next-steps"></a>Sonraki adımlar

- [İdari birimler sorun giderme ve SSS](roles-admin-units-faq-troubleshoot.md)
