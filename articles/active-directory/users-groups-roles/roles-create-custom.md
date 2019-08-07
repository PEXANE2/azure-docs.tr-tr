---
title: Azure AD rol tabanlı erişim denetiminde özel bir rol tanımı oluşturma-Azure Active Directory | Microsoft Docs
description: Azure Active Directory kaynaklarda kaynak kapsamıyla özel Azure AD rolleri oluşturun.
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
ms.openlocfilehash: e0e33cf9fa1c4661dd71fc41cb667b0373c9e955
ms.sourcegitcommit: 4b5dcdcd80860764e291f18de081a41753946ec9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2019
ms.locfileid: "68774828"
---
# <a name="create-a-custom-role-and-assign-at-resource-scope-in-azure-active-directory"></a>Azure Active Directory bir özel rol oluşturma ve kaynak kapsamına atama

Bu makalede, Azure Active Directory (Azure AD) içinde yeni özel roller oluşturma açıklanır. Özel roller, Azure AD Genel Bakış sayfasındaki [Roller ve yöneticiler](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RolesAndAdministrators) sekmesinde oluşturulabilir. Rol, yalnızca dizin düzeyindeki kapsamda veya yalnızca bir uygulama kayıt kaynağı kapsamında atanabilir.

Daha fazla bilgi için özel rollerin [genel bakış](roles-custom-overview.md) bölümüne bakın.

## <a name="using-the-azure-ad-portal"></a>Azure AD portalını kullanma

### <a name="create-a-new-custom-role-to-grant-access-to-manage-app-registrations"></a>Uygulama kayıtlarını yönetmek için erişim izni vermek üzere yeni bir özel rol oluşturun

1. Azure AD kuruluşunda ayrıcalıklı rol yöneticisi veya genel yönetici izinleriyle [Azure AD Yönetim merkezinde](https://aad.portal.azure.com) oturum açın.
1.  **Azure Active Directory** > **Roller ve yöneticiler** > **Yeni özel rol**' i seçin.

   ![Roller ve yöneticiler sayfasından roller oluşturma veya düzenleme](./media/roles-create-custom/new-custom-role.png)

1. **Temel bilgiler** sekmesinde, rol için bir ad ve açıklama girin ve ardından **İleri**' ye tıklayın.

   ![Temel bilgiler sekmesinde özel rol için bir ad ve açıklama girin](./media/roles-create-custom/basics-tab.png)

1. **İzinler** sekmesinde, uygulama kayıtlarının temel özelliklerini ve kimlik bilgisi özelliklerini yönetmek için gerekli izinleri seçin. Her iznin ayrıntılı bir açıklaması için bkz. [Azure Active Directory 'de uygulama kaydı alt türleri ve izinleri](./roles-custom-available-permissions.md).
   1. İlk olarak, arama çubuğuna "kimlik bilgileri" girin ve `microsoft.directory/applications/credentials/update` izni seçin.

      ![Izinler sekmesinde özel bir rol için izinleri seçin](./media/roles-create-custom/permissions-tab.png)

   1. Sonra, arama çubuğuna "temel" yazın, `microsoft.directory/applications/basic/update` izni seçin ve ardından **İleri**' ye tıklayın.
1. **Gözden geçir + oluştur** sekmesinde izinleri gözden geçirin ve **Oluştur**' u seçin.

Özel rolünüzün atanacak kullanılabilir roller listesinde görünür.

## <a name="assign-a-role-scoped-to-a-resource"></a>Kaynak kapsamına alınmış bir rol atama

Yerleşik roller gibi, özel roller, tüm uygulama kayıtları üzerinden erişim sağlamak için kuruluş genelinde kapsama atanabilir. Ancak, özel roller de kaynak kapsamında atanabilir. Bu, bir ikinci özel rol oluşturmak zorunda kalmadan tek bir uygulamanın kimlik bilgilerini ve temel özelliklerini güncelleştirme izni vermenizi sağlar.

1. Henüz yoksa Azure AD kuruluşunda uygulama geliştirici izinleriyle [Azure AD Yönetim merkezinde](https://aad.portal.azure.com) oturum açın.
1. **Uygulama kayıtları**'nı seçin.
1. Yönetmek üzere erişim verdiğiniz uygulama kaydını seçin. Azure AD kuruluşunuzda uygulama kayıtlarının tüm listesini görmek için **tüm uygulamalar** ' ı seçmeniz gerekebilir.

    ![Rol ataması için kaynak kapsamı olarak uygulama kaydını seçin](./media/roles-create-custom/appreg-all-apps.png)

1. Uygulama kaydında **Roller ve yöneticiler**' i seçin. Henüz bir tane oluşturmadıysanız, yönergeler [önceki yordamda](#create-a-new-custom-role-to-grant-access-to-manage-app-registrations)bulunur.

1. **Atamalar** sayfasını açmak için rolü seçin.
1. Kullanıcı eklemek için **atama Ekle** ' yi seçin. Kullanıcıya seçili olan herhangi bir uygulama kaydı üzerinde herhangi bir izin verilmez.

## <a name="create-a-custom-role-using-azure-ad-powershell"></a>Azure AD PowerShell kullanarak özel bir rol oluşturma

### <a name="prepare-powershell"></a>PowerShell 'i hazırlama

İlk olarak, [Azure AD önizleme PowerShell modülünü indirmeniz](https://www.powershellgallery.com/packages/AzureADPreview)gerekir.

Azure AD PowerShell modülünü yüklemek için aşağıdaki komutları kullanın:

``` PowerShell
install-module azureadpreview
import-module azureadpreview
```

Modülün kullanıma hazırsa emin olmak için aşağıdaki komutu kullanın:

``` PowerShell
get-module azureadpreview
  ModuleType Version      Name                         ExportedCommands
  ---------- ---------    ----                         ----------------
  Binary     2.0.2.31     azuread                      {Add-AzureADAdministrati...}
```

### <a name="create-the-custom-role"></a>Özel rol oluşturma

Aşağıdaki PowerShell betiğini kullanarak yeni bir rol oluşturun:

``` PowerShell
# Basic role information
$description = "Application Support Administrator"
$displayName = "Can manage basic aspects of application registrations."
$templateId = (New-Guid).Guid

# Set of permissions to grant
$allowedResourceAction =
@(
    "microsoft.directory/applications/basic/update",
    "microsoft.directory/applications/credentials/update"
)
$resourceActions = @{'allowedResourceActions'= $allowedResourceAction}
$rolePermission = @{'resourceActions' = $resourceActions}
$rolePermissions = $rolePermission

# Create new custom admin role
$customAdmin = New-AzureADMSRoleDefinition -RolePermissions $rolePermissions -DisplayName $displayName -Description $description -TemplateId $templateId -IsEnabled $true
```

### <a name="assign-the-custom-role-using-azure-ad-powershell"></a>Azure AD PowerShell kullanarak özel rol atama

Aşağıdaki PowerShell betiğini kullanarak rolü atayın:

``` PowerShell
# Get the user and role definition you want to link
$user = Get-AzureADUser -Filter "userPrincipalName eq 'cburl@f128.info'"
$roleDefinition = Get-AzureADRoleDefinition -Filter "displayName eq ' Application Registration Creator'"

# Get app registration and construct resource scope for assignment.
$appRegistration = Get-AzureADApplication -Filter "displayName eq 'f/128 Filter Photos'"
$resourceScopes = '/' + $appRegistration.objectId

# Create a scoped role assignment
$roleAssignment = New-AzureADRoleAssignment -ResourceScopes $resourceScopes -RoleDefinitionId $roleDefinition.objectId -PrincipalId $user.objectId
```

## <a name="create-a-custom-role-using-microsoft-graph-api"></a>Microsoft Graph API kullanarak özel bir rol oluşturma

1. Rol tanımını oluşturun.

    Özel bir rol tanımı oluşturmak için HTTP isteği.

    POST

    ``` HTTP
    https://graph.microsoft.com/beta/roleManagement/directory/roleDefinitions
    ```

    Body

    ``` HTTP
    {
    "description":"Can manage basic aspects of application registrations.",
    "displayName":"Application Support Administrator",
    "isEnabled":true,
    "rolePermissions":
    [
        "microsoft.directory/applications/basic/update",
        "microsoft.directory/applications/credentials/update"
    ]
    }
    ```

1. Rol atamasını oluşturun.

    Özel bir rol tanımı oluşturmak için HTTP isteği.

    POST

    ``` HTTP
    https://graph.microsoft.com/beta/roleManagement/directory/roleAssignments
    ```

    Body

    ``` HTTP
    {
    "principalId":"<GUID OF USER>",
    "roleDefinitionId":"<GUID OF ROLE DEFINITION>",
    "resourceScope":["/<GUID OF APPLICATION REGISTRATION>"]
    }
    ```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD Yönetim rolleri forumundan](https://feedback.azure.com/forums/169401-azure-active-directory?category_id=166032)bizimle paylaşabilirsiniz.
- Roller ve yönetici rolü atama hakkında daha fazla bilgi için bkz. [yönetici rolleri atama](directory-assign-admin-roles.md).
- Varsayılan Kullanıcı izinleri için bkz. [varsayılan Konuk ve üye Kullanıcı izinlerinin karşılaştırması](../fundamentals/users-default-permissions.md).
