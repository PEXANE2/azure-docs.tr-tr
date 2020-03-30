---
title: Azure PowerShell ' i kullanarak özel roller atama - Azure AD | Microsoft Dokümanlar
description: Azure PowerShell ile Azure AD yöneticisi özel rolünün üyelerini yönetin.
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
ms.openlocfilehash: 0a2096b7899039e7a9d3455bc0c6fb3ec84ebd1a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74025330"
---
# <a name="assign-custom-roles-with-resource-scope-using-powershell-in-azure-active-directory"></a>Azure Etkin Dizini'nde PowerShell'i kullanarak kaynak kapsamıyla özel roller atama

Bu makalede, Azure Etkin Dizini'nde (Azure AD) kuruluş genelinde bir rol ataması nasıl oluşturulacak açıklanmaktadır. Kuruluş genelinde bir rol atamak, Azure REKLAM kuruluşuna erişim verir. Tek bir Azure REKLAM kaynağının kapsamına sahip bir rol ataması oluşturmak [için, özel bir rol oluşturma ve kaynak kapsamında atama yı](roles-create-custom.md)görün. Bu makalede, [Azure Active Directory PowerShell Sürüm 2](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0#directory_roles) modülü kullanılır.

Azure AD yönetici rolleri hakkında daha fazla bilgi için Azure [Etkin Dizini'nde yönetici rolleri atama](directory-assign-admin-roles.md)'ya bakın.

## <a name="required-permissions"></a>Gerekli izinler

Rolleri atamak veya kaldırmak için küresel bir yönetici hesabını kullanarak Azure AD kiracınıza bağlanın.

## <a name="prepare-powershell"></a>PowerShell Hazırlayın

[PowerShell Galerisi'nden](https://www.powershellgallery.com/packages/AzureADPreview/2.0.0.17)Azure AD PowerShell modüllerini yükleyin. Ardından aşağıdaki komutu kullanarak Azure AD PowerShell önizleme modüllerini içe aktarın:

``` PowerShell
import-module azureadpreview
```

Modülün kullanıma hazır olduğunu doğrulamak için, aşağıdaki komutla döndürülen sürümü burada listelenen sürümle eşleştirin:

``` PowerShell
get-module azureadpreview
  ModuleType Version      Name                         ExportedCommands
  ---------- ---------    ----                         ----------------
  Binary     2.0.0.115    azureadpreview               {Add-AzureADMSAdministrati...}
```

Şimdi modüldeki cmdlets kullanmaya başlayabilirsiniz. Azure AD modülündeki cmdletlerin tam açıklaması için [Azure AD önizleme modülü](https://www.powershellgallery.com/packages/AzureADPreview/2.0.0.17)için çevrimiçi başvuru belgelerine bakın.

## <a name="assign-a-role-to-a-user-or-service-principal-with-resource-scope"></a>Kaynak kapsamı olan bir kullanıcı veya hizmet ilkesine rol atama

1. Azure AD önizleme PowerShell modüllerini açın.
1. Komutu `Connect-AzureAD`çalıştırarak oturum açın.
1. Aşağıdaki PowerShell komut dosyasını kullanarak yeni bir rol oluşturun.

``` PowerShell
## Assign a role to a user or service principal with resource scope
# Get the user and role definition you want to link
$user = Get-AzureADUser -Filter "userPrincipalName eq 'cburl@f128.info'"
$roleDefinition = Get-AzureADMSRoleDefinition -Filter "displayName eq 'Application Support Administrator'"

# Get app registration and construct resource scope for assignment.
$appRegistration = Get-AzureADApplication -Filter "displayName eq 'f/128 Filter Photos'"
$resourceScope = '/' + $appRegistration.objectId

# Create a scoped role assignment
$roleAssignment = New-AzureADMSRoleAssignment -ResourceScope $resourceScope -RoleDefinitionId $roleDefinition.Id -PrincipalId $user.objectId
```

Rolü kullanıcı yerine bir hizmet ilkesine atamak için [Get-AzureADMSServicePrincipal cmdlet'i](https://docs.microsoft.com/powershell/module/azuread/get-azureadserviceprincipal?view=azureadps-2.0)kullanın.

## <a name="operations-on-roledefinition"></a>RoleDefinition üzerinde Operasyonlar

Rol tanımı nesneleri, bu rol ataması tarafından verilen izinlerin yanı sıra yerleşik veya özel rolün tanımını içerir. Bu kaynak hem özel rol tanımlarını hem de dahili dizinRollerini (roleDefinition eşdeğer formunda görüntülenir) görüntüler. Bugün, bir Azure REKLAM kuruluşunun en fazla 30 benzersiz özel RoleDefinitions tanımlanmış olabilir.

### <a name="create-operations-on-roledefinition"></a>RoleDefinition'da İşlemler Oluşturma

``` PowerShell
# Basic information
$description = "Can manage credentials of application registrations"
$displayName = "Application Registration Credential Administrator"
$templateId = (New-Guid).Guid

# Set of actions to grant
$allowedResourceAction =
@(
    "microsoft.directory/applications/standard/read",
    "microsoft.directory/applications/credentials/update"
)
$rolePermissions = @{'allowedResourceActions'= $allowedResourceAction}

# Create new custom admin role
$customAdmin = New-AzureADMSRoleDefinition -RolePermissions $rolePermissions -DisplayName $displayName -Description $description -TemplateId $templateId -IsEnabled $true
```

### <a name="read-operations-on-roledefinition"></a>RoleDefinition'da Okuma İşlemleri

``` PowerShell
# Get all role definitions
Get-AzureADMSRoleDefinitions

# Get single role definition by objectId
Get-AzureADMSRoleDefinition -Id 86593cfc-114b-4a15-9954-97c3494ef49b

# Get single role definition by templateId
Get-AzureADMSRoleDefinition -Filter "templateId eq 'c4e39bd9-1100-46d3-8c65-fb160da0071f'"
```

### <a name="update-operations-on-roledefinition"></a>RoleDefinition'daki İşlemleri Güncelleştir

``` PowerShell
# Update role definition
# This works for any writable property on role definition. You can replace display name with other
# valid properties.
Set-AzureADMSRoleDefinition -Id c4e39bd9-1100-46d3-8c65-fb160da0071f -DisplayName "Updated DisplayName"
```

### <a name="delete-operations-on-roledefinition"></a>RoleDefinition üzerindeki işlemleri silme

``` PowerShell
# Delete role definition
Remove-AzureADMSRoleDefinitions -Id c4e39bd9-1100-46d3-8c65-fb160da0071f
```

## <a name="operations-on-roleassignment"></a>RoleAssignment Operasyonları

Rol atamaları, belirli bir güvenlik ilkesini (kullanıcı veya uygulama hizmeti ilkesi) rol tanımına bağlayan bilgileri içerir. Gerekirse, atanan izinler için tek bir Azure AD kaynağının kapsamını ekleyebilirsiniz.  İzinlerin kapsamını kısıtlamak yerleşik ve özel roller için desteklenir.

### <a name="create-operations-on-roleassignment"></a>RoleAssignment'da İşlemler Oluşturma

``` PowerShell
# Get the user and role definition you want to link
$user = Get-AzureADUser -Filter "userPrincipalName eq 'cburl@f128.info'"
$roleDefinition = Get-AzureADMSRoleDefinition -Filter "displayName eq 'Application Support Administrator'"

# Get app registration and construct resource scope for assignment.
$appRegistration = Get-AzureADApplication -Filter "displayName eq 'f/128 Filter Photos'"
$resourceScope = '/' + $appRegistration.objectId

# Create a scoped role assignment
$roleAssignment = New-AzureADMSRoleAssignment -ResourceScope $resourceScope -RoleDefinitionId $roleDefinition.Id -PrincipalId $user.objectId
```

### <a name="read-operations-on-roleassignment"></a>RoleAssignment'da Okuma İşlemleri

``` PowerShell
# Get role assignments for a given principal
Get-AzureADMSRoleAssignment -Filter "principalId eq '27c8ca78-ab1c-40ae-bd1b-eaeebd6f68ac'"

# Get role assignments for a given role definition 
Get-AzureADMSRoleAssignment -Filter "roleDefinitionId eq '355aed8a-864b-4e2b-b225-ea95482e7570'"
```

### <a name="delete-operations-on-roleassignment"></a>RoleAssignment'da İşlemleri Silme

``` PowerShell
# Delete role assignment
Remove-AzureADMSRoleAssignment -Id 'qiho4WOb9UKKgng_LbPV7tvKaKRCD61PkJeKMh7Y458-1'
```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD yönetim rolleri forumunda](https://feedback.azure.com/forums/169401-azure-active-directory?category_id=166032)bizimle paylaşın.
- Roller ve azure AD yöneticisi rol atamaları hakkında daha fazla şey için yönetici [rollerini atay'a](directory-assign-admin-roles.md)bakın.
- Varsayılan kullanıcı izinleri için varsayılan [konuk ve üye kullanıcı izinlerinin karşılaştırılmasına](../fundamentals/users-default-permissions.md)bakın.
