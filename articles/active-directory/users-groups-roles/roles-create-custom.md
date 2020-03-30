---
title: Azure AD rol tabanlı erişim denetiminde özel roller oluşturma | Microsoft Dokümanlar
description: Azure Etkin Dizin kaynaklarında kaynak kapsamıyla özel Azure REKLAM rolleri oluşturun ve atayın.
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
ms.openlocfilehash: c2cb19c82f8c19bf87eeef755adb5756b2452512
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74025285"
---
# <a name="create-and-assign-a-custom-role-in-azure-active-directory"></a>Azure Etkin Dizin'de özel bir rol oluşturma ve atama

Bu makalede, Azure Etkin Dizini'nde (Azure AD) yeni özel rollerin nasıl oluşturulacak olduğu açıklanmaktadır. Özel rollerin temelleri için [özel rollere genel bakış'a](roles-custom-overview.md)bakın. Rol, dizin düzeyindeki kapsamda veya yalnızca bir uygulama kaydı kaynak kapsamında atanabilir.

Azure REKLAM genel bakış sayfasındaki [Roller ve yöneticiler](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RolesAndAdministrators) sekmesinde özel roller oluşturulabilir.

## <a name="create-a-role-in-the-azure-portal"></a>Azure portalında rol oluşturma

### <a name="create-a-new-custom-role-to-grant-access-to-manage-app-registrations"></a>Uygulama kayıtlarını yönetmeye erişim sağlamak için yeni bir özel rol oluşturma

1. Azure AD kuruluşunda Ayrıcalıklı rol yöneticisi veya Global yönetici izinleriyle [Azure AD yönetici merkezinde](https://aad.portal.azure.com) oturum açın.
1. **Azure Etkin Dizin** > **Rolleri ve yöneticileri** > yeni**özel rolü**seçin.

   ![Roller ve yöneticiler sayfasından roller oluşturma veya oluşturma](./media/roles-create-custom/new-custom-role.png)

1. Temel **Bilgiler** sekmesinde, rol için bir ad ve açıklama sağlayın ve sonra **İleri'yi**tıklatın.

   ![Temel ler sekmesinde özel bir rol için bir ad ve açıklama sağlayın](./media/roles-create-custom/basics-tab.png)

1. **İzinler** sekmesinde, uygulama kayıtlarının temel özelliklerini ve kimlik bilgilerini yönetmek için gereken izinleri seçin. Her bir iznin ayrıntılı açıklaması için [Azure Etkin Dizini'ndeki Uygulama kayıt alt türlerine ve izinlerine](./roles-custom-available-permissions.md)bakın.
   1. İlk olarak, arama çubuğuna "kimlik `microsoft.directory/applications/credentials/update` bilgileri" girin ve izni seçin.

      ![İzinler sekmesinde özel bir rol için izinleri seçme](./media/roles-create-custom/permissions-tab.png)

   1. Ardından, arama çubuğuna "temel" girin, `microsoft.directory/applications/basic/update` izni seçin ve sonra **İleri'yi**tıklatın.
1. Gözden **Geçir + oluştur** sekmesinde, izinleri gözden geçirin ve **Oluştur'u**seçin.

Özel rolünüz atayacak kullanılabilir roller listesinde gösterecektir.

## <a name="create-a-role-using-powershell"></a>PowerShell'i kullanarak rol oluşturma

### <a name="prepare-powershell"></a>PowerShell Hazırlayın

İlk olarak, [Azure AD Preview PowerShell modüllerini indirmeniz](https://www.powershellgallery.com/packages/AzureADPreview)gerekir.

Azure AD PowerShell modüllerini yüklemek için aşağıdaki komutları kullanın:

``` PowerShell
install-module azureadpreview
import-module azureadpreview
```

Modülün kullanıma hazır olup olmadığını doğrulamak için aşağıdaki komutu kullanın:

``` PowerShell
get-module azureadpreview
  ModuleType Version      Name                         ExportedCommands
  ---------- ---------    ----                         ----------------
  Binary     2.0.2.31     azuread                      {Add-AzureADAdministrati...}
```

### <a name="create-the-custom-role"></a>Özel rolü oluşturma

Aşağıdaki PowerShell komut dosyasını kullanarak yeni bir rol oluşturun:

``` PowerShell
# Basic role information
$displayName = "Application Support Administrator"
$description = "Can manage basic aspects of application registrations."
$templateId = (New-Guid).Guid
 
# Set of permissions to grant
$allowedResourceAction =
@(
    "microsoft.directory/applications/basic/update",
    "microsoft.directory/applications/credentials/update"
)
$rolePermissions = @{'allowedResourceActions'= $allowedResourceAction}
 
# Create new custom admin role
$customAdmin = New-AzureADMSRoleDefinition -RolePermissions $rolePermissions -DisplayName $displayName -Description $description -TemplateId $templateId -IsEnabled $true
```

### <a name="assign-the-custom-role-using-azure-ad-powershell"></a>Azure AD PowerShell'i kullanarak özel rolü atama

Aşağıdaki PowerShell komut dosyasını kullanarak rolü atayın:

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

## <a name="create-a-role-with-graph-api"></a>Grafik API ile rol oluşturma

1. Rol tanımını oluşturun.

    ÖZEL rol tanımı oluşturmak için HTTP isteği.

    POST

    ``` HTTP
    https://graph.microsoft.com/beta/roleManagement/directory/roleDefinitions
    ```

    Gövde

    ``` HTTP
   {
       "description": "Can manage basic aspects of application registrations.",
       "displayName": "Application Support Administrator",
       "isEnabled": true,
       "templateId": "<GUID>",
       "rolePermissions": [
           {
               "allowedResourceActions": [
                   "microsoft.directory/applications/basic/update",
                   "microsoft.directory/applications/credentials/update"
               ]
           }
       ]
   }
    ```

1. Rol atamasını oluşturun.

    ÖZEL rol tanımı oluşturmak için HTTP isteği.

    POST

    ``` HTTP
    https://graph.microsoft.com/beta/roleManagement/directory/roleAssignments
    ```

    Gövde

    ``` HTTP
   {
       "principalId":"<GUID OF USER>",
       "roleDefinitionId":"<GUID OF ROLE DEFINITION>",
       "resourceScope":"/<GUID OF APPLICATION REGISTRATION>"
   }
    ```

## <a name="assign-a-custom-role-scoped-to-a-resource"></a>Kaynağa kapsamlı özel bir rol atama

Yerleşik roller gibi, kuruluşunuzdaki tüm uygulama kayıtları üzerinde erişim izinleri vermek için varsayılan olarak kuruluş genelinde varsayılan olarak özel roller atanır. Ancak yerleşik rollerin aksine, özel roller tek bir Azure REKLAM kaynağı kapsamında da atanabilir. Bu, kullanıcıya ikinci bir özel rol oluşturmak zorunda kalmadan tek bir uygulamanın kimlik bilgilerini ve temel özelliklerini güncelleştirme izni vermenizi sağlar.

1. Azure AD kuruluşundaki Uygulama geliştiriciizinleri ile [Azure AD yönetici merkezinde](https://aad.portal.azure.com) oturum açın.
1. **Uygulama kayıtları**'nı seçin.
1. Yönetmeye erişim izni verdiğiniz uygulama kaydını seçin. Azure REKLAM kuruluşunuzdaki uygulama kayıtlarının tam listesini görmek için **Tüm uygulamaları** seçmeniz gerekebilir.

    ![Rol ataması için kaynak kapsamı olarak uygulama kaydını seçme](./media/roles-create-custom/appreg-all-apps.png)

1. Uygulama kaydında, **Roller'i ve yöneticileri**seçin. Daha önce bir tane oluşturmadıysanız, yönergeler [önceki yordamdadır.](#create-a-new-custom-role-to-grant-access-to-manage-app-registrations)

1. **Atamalar** sayfasını açmak için rolü seçin.
1. Kullanıcı eklemek için **atama ekle'yi** seçin. Kullanıcıya yalnızca seçilen uygulama kaydı üzerinden izin verilecektir.

## <a name="next-steps"></a>Sonraki adımlar

- Azure AD yönetim rolleri [forumunda](https://feedback.azure.com/forums/169401-azure-active-directory?category_id=166032)bizimle paylaşmakta çekinmeyin.
- Roller ve Yönetici rol ataması hakkında daha fazla şey için [yönetici rollerini atay'a](directory-assign-admin-roles.md)bakın.
- Varsayılan kullanıcı izinleri için varsayılan [konuk ve üye kullanıcı izinlerinin karşılaştırılmasına](../fundamentals/users-default-permissions.md)bakın.
