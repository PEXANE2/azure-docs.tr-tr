---
title: Yönetici rolü atamasını atama ve kaldırma Azure PowerShell-Azure Active Directory | Microsoft Docs
description: Rol atamalarını sık sık yönetenler için artık Azure PowerShell bir Azure AD yönetici rolü üyelerini yönetebilirsiniz.
services: active-directory
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 07/31/2019
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: aa4bddf84720265afe361dff665f10ff8184f6f6
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2019
ms.locfileid: "68706494"
---
# <a name="assign-azure-active-directory-admin-roles-using-powershell"></a>PowerShell kullanarak Azure Active Directory yönetici rolleri atama

Azure PowerShell kullanarak Kullanıcı hesaplarına roller atamayı otomatik hale getirebilirsiniz. Bu makale, [Azure Active Directory PowerShell sürüm 2](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0#directory_roles) modülünü kullanır.

## <a name="prepare-powershell"></a>PowerShell 'i hazırlama

İlk olarak, [Azure AD PowerShell modülünü indirmeniz](https://www.powershellgallery.com/packages/AzureAD/)gerekir.

## <a name="install-the-azure-ad-powershell-module"></a>Azure AD PowerShell modülünü yükleme

Azure AD PowerShell modülünü yüklemek için aşağıdaki komutları kullanın:

```powershell
install-module azuread
import-module azuread
```

Modülün kullanıma hazırsa emin olmak için aşağıdaki komutu kullanın:

```powershell
get-module azuread
  ModuleType Version      Name                         ExportedCommands
  ---------- ---------    ----                         ----------------
  Binary     2.0.0.115    azuread                      {Add-AzureADAdministrati...}
```

Artık modüldeki cmdlet 'leri kullanmaya başlayabilirsiniz. Azure AD modülündeki cmdlet 'lerin tam açıklaması için lütfen [Azure Active Directory PowerShell sürüm 2](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0#directory_roles)' ye yönelik çevrimiçi başvuru belgelerine bakın.

## <a name="permissions-required"></a>Gerekli izinler

Rolleri atamak veya kaldırmak için bir genel yönetici hesabı kullanarak Azure AD kiracınıza bağlanın.

## <a name="assign-a-single-role"></a>Tek bir rol atama

Rol atamak için, önce kendi görünen adını ve atadığınız rolün adını edinmeniz gerekir. Hesabın görünen adına ve rolün adına sahip olduğunuzda, rolü kullanıcıya atamak için aşağıdaki cmdlet 'leri kullanın.

``` PowerShell
# Fetch user to assign to role
$roleMember = Get-AzureADUser -ObjectId "username@contoso.com"

# Fetch User Account Administrator role instance
$role = Get-AzureADDirectoryRole | Where-Object {$_.displayName -eq 'User Account Administrator'}

# If role instance does not exist, instantiate it based on the role template
if ($role -eq $null) {
    # Instantiate an instance of the role template
    $roleTemplate = Get-AzureADDirectoryRoleTemplate | Where-Object {$_.displayName -eq 'User Account Administrator'}
    Enable-AzureADDirectoryRole -RoleTemplateId $roleTemplate.ObjectId

    # Fetch User Account Administrator role instance again
    $role = Get-AzureADDirectoryRole | Where-Object {$_.displayName -eq 'User Account Administrator'}
}

# Add user to role
Add-AzureADDirectoryRoleMember -ObjectId $role.ObjectId -RefObjectId $roleMember.ObjectId

# Fetch role membership for role to confirm
Get-AzureADDirectoryRoleMember -ObjectId $role.ObjectId | Get-AzureADUser
```

## <a name="assign-a-role-to-a-service-principal"></a>Hizmet sorumlusuna rol atama

Bir role hizmet sorumlusu atama örneği.

```powershell
# Fetch a service principal to assign to role
$roleMember = Get-AzureADServicePrincipal -ObjectId "00221b6f-4387-4f3f-aa85-34316ad7f956"
 
#Fetch list of all directory roles with object ID
Get-AzureADDirectoryRole
 
# Fetch a directory role by ID
$role = Get-AzureADDirectoryRole -ObjectId "5b3fe201-fa8b-4144-b6f1-875829ff7543"
 
# Add user to role
Add-AzureADDirectoryRoleMember -ObjectId $role.ObjectId -RefObjectId $roleMember.ObjectId
 
# Fetch the assignment for the role to confirm
Get-AzureADDirectoryRoleMember -ObjectId $role.ObjectId | Get-AzureADServicePrincipal
```

## <a name="multiple-role-assignments"></a>Çoklu rol atamaları

Aynı anda birden çok rol atama ve kaldırma örnekleri.

```powershell
#File name
$fileName="<file path>" 
 
$input_Excel = New-Object -ComObject Excel.Application
$input_Workbook = $input_Excel.Workbooks.Open($fileName)
$input_Worksheet = $input_Workbook.Sheets.Item(1)
 
        #Count number of users who have to be assigned to role
$count = $input_Worksheet.UsedRange.Rows.Count
 
#Loop through each line of the csv file starting from line 2 (assuming first line is title)
for ($i=2; $i -le $count; $i++)
{
       #Fetch user display name
       $displayName = $input_Worksheet.Cells.Item($i,1).Text
       
       #Fetch role name
       $roleName = $input_Worksheet.Cells.Item($i,2).Text
 
       #Assign role
       Add-AzureADDirectoryRoleMember -ObjectId (Get-AzureADDirectoryRole | Where-Object DisplayName -eq $roleName).ObjectId -RefObjectId (Get-AzureADUser | Where-Object DisplayName -eq $displayName).ObjectId
}
 
#Remove multiple role assignments
for ($i=2; $i -le $count; $i++)
{
       $displayName = $input_Worksheet.Cells.Item($i,1).Text
       $roleName = $input_Worksheet.Cells.Item($i,2).Text
 
       Remove-AzureADDirectoryRoleMember -ObjectId (Get-AzureADDirectoryRole | Where-Object DisplayName -eq $roleName).ObjectId -MemberId (Get-AzureADUser | Where-Object DisplayName -eq $displayName).ObjectId
}
```

## <a name="remove-a-role-assignment"></a>Rol atamasını kaldırma

Bu örnek, belirtilen kullanıcı için bir rol atamasını kaldırır.

```powershell
# Fetch user to assign to role
$roleMember = Get-AzureADUser -ObjectId "username@contoso.com"

#Fetch list of all directory roles with object id
Get-AzureADDirectoryRole

# Fetch a directory role by id
$role = Get-AzureADDirectoryRole -ObjectId "5b3fe201-fa8b-4144-b6f1-875829ff7543"

# Remove user from role
Remove-AzureADDirectoryRoleMember -ObjectId $role.ObjectId -MemberId $roleMember.ObjectId 

# Fetch role membership for role to confirm
Get-AzureADDirectoryRoleMember -ObjectId $role.ObjectId | Get-AzureADUser

```

## <a name="next-steps"></a>Sonraki adımlar

* [Azure AD Yönetim rolleri forumundan](https://feedback.azure.com/forums/169401-azure-active-directory?category_id=166032)bizimle paylaşabilirsiniz.
* Roller ve yönetici rolü atama hakkında daha fazla bilgi için bkz. [yönetici rolleri atama](directory-assign-admin-roles.md).
* Varsayılan Kullanıcı izinleri için bkz. [varsayılan Konuk ve üye Kullanıcı izinlerinin karşılaştırması](../fundamentals/users-default-permissions.md).
