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
ms.openlocfilehash: ae773bc1a4e1831dbe462149bb827c26b7e74b96
ms.sourcegitcommit: 85b3973b104111f536dc5eccf8026749084d8789
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2019
ms.locfileid: "68722257"
---
# <a name="create-a-custom-role-and-assign-a-resource-scope-in-azure-active-directory"></a>Azure Active Directory bir özel rol oluşturma ve kaynak kapsamı atama

Bu makalede, Azure Active Directory (Azure AD) içinde yeni özel roller oluşturma açıklanır. Özel roller, Azure AD Genel Bakış sayfasında veya [uygulama kaydı sayfasında](https://portal.azure.com/?Microsoft_AAD_IAM_enableCustomRoleManagement=true&Microsoft_AAD_IAM_enableCustomRoleAssignment=true&feature.rbacv2roles=true&feature.rbacv2=true&Microsoft_AAD_RegisteredApps=demo#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredAppsPreview) **Roller ve yöneticiler** sekmesinde oluşturulabilir. Rol, dizin düzeyindeki kapsamda ya da yalnızca uygulama kayıtlarına atanabilir.

Daha fazla bilgi için özel rollerin [genel bakış](roles-custom-overview.md) bölümüne bakın.

## <a name="using-the-azure-ad-portal"></a>Azure AD portalını kullanma

### <a name="create-a-new-custom-role-to-grant-access-to-manage-app-registrations"></a>Uygulama kayıtlarını yönetmek için erişim izni vermek üzere yeni bir özel rol oluşturun

1. Azure AD kuruluşunda ayrıcalıklı rol yöneticisi veya genel yönetici izinleriyle [Azure AD Yönetim merkezinde](https://aad.portal.azure.com) oturum açın.
1.  **Azure Active Directory** > **Roller ve yöneticiler** > **Yeni özel rol**' i seçin.

   ![Roller ve yöneticiler sayfasından roller oluşturma veya düzenleme](./media/roles-create-custom/new-custom-role.png)

1. **Temel bilgiler** sekmesinde, rol için bir ad ve açıklama girin.

   ![Temel bilgiler sekmesinde özel rol için bir ad ve açıklama girin](./media/roles-create-custom/basics-tab.png)

1. Uygulama kayıt kimlik bilgilerini ve ad gibi temel özellikleri yönetmek için gerekli izinleri seçmek için:
   1. Arama çubuğuna "kimlik bilgileri" girin ve `microsoft.directory/applications/credentials/update` izni seçin.

      ![Izinler sekmesinde özel bir rol için izinleri seçin](./media/roles-create-custom/permissions-tab.png)

   1. Arama çubuğuna "temel" yazın, `microsoft.directory/applications/basic/update` izni seçin ve ardından **İleri**' ye tıklayın.
1. **Gözden geçir + oluştur** sekmesinde izinleri gözden geçirin ve **Oluştur**' u seçin.

Özel rolünüzün atanacak kullanılabilir roller listesinde görünür.

## <a name="assign-a-role-scoped-to-a-resource"></a>Kaynak kapsamına alınmış bir rol atama

Yerleşik roller gibi, özel roller, tüm uygulama kayıtları üzerinden erişim sağlamak için varsayılan kuruluş kapsamında atanabilir. Ancak, özel roller de nesne kapsamında atanabilir. Bu, bir ikinci özel rol oluşturmak zorunda kalmadan tek bir uygulamanın kimlik bilgilerini ve temel özelliklerini güncelleştirme izni vermenizi sağlar.

1. Azure AD kuruluşunda uygulama geliştirici izinleriyle [Azure AD Yönetim merkezinde](https://aad.portal.azure.com) oturum açın.
1. **Uygulama kayıtları**'nı seçin.
1. Yönetmek üzere erişim verdiğiniz uygulama kaydını seçin. Azure AD kuruluşunuzda uygulama kayıtlarının tüm listesini görmek için **tüm uygulamalar** ' ı seçmeniz gerekebilir.

    ![Rol ataması için kaynak kapsamı olarak uygulama kaydını seçin](./media/roles-create-custom/appreg-all-apps.png)

1. Uygulama kaydında **Roller ve yöneticiler**' i seçin. Henüz bir tane oluşturmadıysanız, yönergeler [önceki yordamda](#create-a-new-custom-role-to-grant-access-to-manage-app-registrations)bulunur.

    Bu rolü açık bir uygulama kaydı bağlamından atarsanız, atanan kullanıcının bu tek uygulama kaydında izinleri vardır. Atadığınız rol, her uygulama kaydında listede görüntülenir. Bu erişim modeli, bir sahibe rollere göre belirli Azure AD kaynaklarına atanabileceği, Azure kaynak erişimi denetimi için [Azure RBAC](../../role-based-access-control/overview.md) 'de kullanılan modele benzerdir.

1. **Atamalar** sayfasını açmak için rolü seçin.
1. Kullanıcı eklemek için **atama Ekle** ' yi seçin. Kullanıcıya seçili olan herhangi bir uygulama kaydı üzerinde herhangi bir izin verilmez.

## <a name="create-a-custom-role-using-azure-ad-powershell"></a>Azure AD PowerShell kullanarak özel bir rol oluşturma

### <a name="prepare-powershell"></a>PowerShell 'i hazırlama

İlk olarak, [Azure AD PowerShell modülünü indirmeniz](https://www.powershellgallery.com/packages/AzureAD/)gerekir.

Azure AD PowerShell modülünü yüklemek için aşağıdaki komutları kullanın:

``` PowerShell
install-module azureadpreview
import-module azureadpreview
```

Modülün kullanıma hazırsa emin olmak için aşağıdaki komutu kullanın:

``` PowerShell
get-module azuread
  ModuleType Version      Name                         ExportedCommands
  ---------- ---------    ----                         ----------------
  Binary     2.0.0.115    azuread                      {Add-AzureADAdministrati...}
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
    "microsoft.directory/applications/allProperties/read",
    "microsoft.directory/applications/basic/update",
    "microsoft.directory/applications/credentials/update"
)
$resourceActions = @{'allowedResourceActions'= $allowedResourceAction}
$rolePermission = @{'resourceActions' = $resourceActions}
$rolePermissions = $rolePermission

# Create new custom admin role
$customAdmin = New-AzureAdRoleDefinition -RolePermissions $rolePermissions -DisplayName $displayName -Description $description -TemplateId $templateId -IsEnabled $true
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
        {
            "allowedResourceActions": 
            [
                "microsoft.directory/applications/basic/read",
                "microsoft.directory/applications/credentials/update"
            ]
        }
    ]
    }
    ```

1. Rol atamasını oluşturun.

    Özel bir rol tanımı oluşturmak için HTTP isteği.

    POST

    ``` HTTP
    https://graph.microsoft.com/beta/roleManagement/directory/roleDefinitions/roleAssignments
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
