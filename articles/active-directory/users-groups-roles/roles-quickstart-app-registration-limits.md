---
title: Uygulama kayıtları oluşturma yla ilgili sınırlamaları kaldırın - Azure AD | Microsoft Dokümanlar
description: Azure AD Etkin Dizini'nde sınırsız uygulama kaydı sağlamak için özel bir rol atama
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
ms.openlocfilehash: 7acd76ff45f783f614b2a1d3f0d5c10d800a1ea9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77559054"
---
# <a name="quickstart-grant-permission-to-create-unlimited-app-registrations"></a>Quickstart: Sınırsız uygulama kaydı oluşturmak için izin verme

Bu hızlı başlangıçta, sınırsız sayıda uygulama kaydı oluşturmak için izinle özel bir rol oluşturur ve bu rolü bir kullanıcıya atarsınız. Atanan kullanıcı daha sonra uygulama kayıtları oluşturmak için Azure AD portalını, Azure AD PowerShell'i veya Microsoft Graph API'yi kullanabilir. Yerleşik Uygulama Geliştiricisi rolünün aksine, bu özel rol sınırsız sayıda uygulama kaydı oluşturma olanağı verir. Uygulama Geliştiricisi rolü yeteneği verir, ancak oluşturulan nesnelerin toplam sayısı [dizin çapında nesne kotası](directory-service-limits-restrictions.md)isabet önlemek için 250 ile sınırlıdır.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisite"></a>Önkoşul

Azure AD özel rolleri oluşturmak ve atamak için gereken en az ayrıcalıklı rol Ayrıcalıklı Rol yöneticisidir.

## <a name="create-a-new-custom-role-using-the-azure-ad-portal"></a>Azure AD portalını kullanarak yeni bir özel rol oluşturma

1. Azure AD kuruluşunda Ayrıcalıklı Rol yöneticisi veya Global yönetici izinleriyle [Azure AD yönetici merkezinde](https://aad.portal.azure.com) oturum açın.
1. **Azure Etkin Dizini'ni**seçin, **Roller'i ve yöneticileri**seçin ve ardından Yeni özel **rolü**seçin.

    ![Roller ve yöneticiler sayfasından roller oluşturma veya oluşturma](./media/roles-create-custom/new-custom-role.png)

1. Temel **bilgiler** sekmesinde, rolün adı için "Uygulama Kayıt Oluşturucusu" ve rol açıklaması için "Sınırsız sayıda uygulama kaydı oluşturabilir" seçeneğini belirleyin ve ardından **İleri'yi**seçin.

    ![Temel ler sekmesinde özel bir rol için bir ad ve açıklama sağlayın](./media/roles-quickstart-app-registration-limits/basics-tab.png)

1. **İzinler** sekmesinde, arama kutusuna "microsoft.directory/applications/create" girin ve ardından istenen izinlerin yanındaki onay kutularını seçin ve **sonra İleri'yi**seçin.

    ![İzinler sekmesinde özel bir rol için izinleri seçme](./media/roles-quickstart-app-registration-limits/permissions-tab.png)

1. Gözden **Geçir + oluştur** sekmesinde, izinleri gözden geçirin ve **Oluştur'u**seçin.

### <a name="assign-the-role-to-a-user-using-the-azure-ad-portal"></a>Azure AD portalını kullanan bir kullanıcıya rol atama

1. Azure AD kuruluşunuzdaki Ayrıcalıklı rol yöneticisi veya Global yönetici izinleriyle [Azure AD yönetici merkezinde](https://aad.portal.azure.com) oturum açın.
1. **Azure Etkin Dizini'ni** seçin ve ardından **Roller'i ve yöneticileri**seçin.
1. Uygulama Kayıt Oluşturucusu rolünü seçin ve **atama ekle'yi**seçin.
1. İstenilen kullanıcıyı seçin ve kullanıcıyı role eklemek için **Seç'i** tıklatın.

Bitti! Bu hızlı başlangıçta, sınırsız sayıda uygulama kaydı oluşturmak ve sonra bu rolü bir kullanıcıya atamak için izinle özel bir rolü başarıyla oluşturdunuz.

> [!TIP]
> Rolü Azure AD portalını kullanarak bir uygulamaya atamak için, uygulamanın adını atama sayfasının arama kutusuna girin. Uygulamalar listede varsayılan olarak gösterilmez, ancak arama sonuçlarında döndürülür.

### <a name="app-registration-permissions"></a>Uygulama kayıt izinleri

Her biri farklı davranışta bulunan uygulama kayıtları oluşturma olanağı vermek için iki izin vardır.

- microsoft.directory/applications/createAsOwner: Bu izin atanması, oluşturulan uygulama kaydının ilk sahibi olarak eklenen oluşturucuyla sonuçlanır ve oluşturulan uygulama kaydı, oluşturucunun oluşturulan 250 nesne kotasına dahil edilir.
- microsoft.directory/applicationPolitikalar/create: Bu izin atanması, oluşturulan uygulama kaydının ilk sahibi olarak eklenmemesi ile sonuçlanır ve oluşturulan uygulama kaydı, oluşturucunun oluşturulan 250 nesne kotasına dahil edilmez. Dizin düzeyindekota vuruluncaya kadar devralan firmanın uygulama kayıtları oluşturmasını engelleyen hiçbir şey olmadığından, bu izni dikkatle kullanın. Her iki izin de atanmışsa, bu izin önceliklidir.

## <a name="create-a-custom-role-using-azure-ad-powershell"></a>Azure AD PowerShell'i kullanarak özel bir rol oluşturma

Aşağıdaki PowerShell komut dosyasını kullanarak yeni bir rol oluşturun:

``` PowerShell
# Basic role information
$description = "Application Registration Creator"
$displayName = "Can create an unlimited number of application registrations."
$templateId = (New-Guid).Guid

# Set of permissions to grant
$allowedResourceAction =
@(
    "microsoft.directory/applications/createAsOwner"
)
$resourceActions = @{'allowedResourceActions'= $allowedResourceAction}
$rolePermission = @{'resourceActions' = $resourceActions}
$rolePermissions = $rolePermission

# Create new custom admin role
$customRole = New-AzureAdRoleDefinition -RolePermissions $rolePermissions -DisplayName $displayName -Description $description -TemplateId $templateId -IsEnabled $true
```

### <a name="assign-the-custom-role-using-azure-ad-powershell"></a>Azure AD PowerShell'i kullanarak özel rolü atama

#### <a name="prepare-powershell"></a>PowerShell Hazırlayın

İlk olarak, [PowerShell Galerisi'nden](https://www.powershellgallery.com/packages/AzureADPreview/2.0.0.17)Azure AD PowerShell modüllerini yükleyin. Ardından aşağıdaki komutu kullanarak Azure AD PowerShell önizleme modüllerini içe aktarın:

```powershell
import-module azureadpreview
```

Modülün kullanıma hazır olduğunu doğrulamak için, aşağıdaki komutla döndürülen sürümü burada listelenen sürümle eşleştirin:

```powershell
get-module azureadpreview
  ModuleType Version      Name                         ExportedCommands
  ---------- ---------    ----                         ----------------
  Binary     2.0.0.115    azureadpreview               {Add-AzureADAdministrati...}
```

#### <a name="assign-the-custom-role"></a>Özel rolü atama

Aşağıdaki PowerShell komut dosyasını kullanarak rolü atayın:

``` PowerShell
# Basic role information
$description = "Application Registration Creator"
$displayName = "Can create an unlimited number of application registrations."
$templateId = (New-Guid).Guid

# Set of permissions to grant
$allowedResourceAction =
@(
    "microsoft.directory/applications/create"
)
$resourceActions = @{'allowedResourceActions'= $allowedResourceAction}
$rolePermission = @{'resourceActions' = $resourceActions}
$rolePermissions = $rolePermission

# Create new custom admin role
$customRole = New-AzureAdRoleDefinition -RolePermissions $rolePermissions -DisplayName $displayName -Description $description -TemplateId $templateId -IsEnabled $true
```

### <a name="create-a-custom-role-using-microsoft-graph-api"></a>Microsoft Graph API'yi kullanarak özel bir rol oluşturma

HTTP özel rolü oluşturmak için istek.

POST

``` HTTP
https://graph.microsoft.com/beta/roleManagement/directory/roleDefinitions
```

Gövde

```HTTP
{
    "description":"Can create an unlimited number of application registrations.",
    "displayName":"Application Registration Creator",
    "isEnabled":true,
    "rolePermissions":
    [
        {
            "resourceActions":
            {
                "allowedResourceActions":
                [
                    "microsoft.directory/applications/create"
                ]
            },
            "condition":null
        }
    ],
    "templateId":"<PROVIDE NEW GUID HERE>",
    "version":"1"
}
```

### <a name="assign-the-custom-role-using-microsoft-graph-api"></a>Microsoft Graph API'yi kullanarak özel rolü atama

Rol ataması, bir güvenlik temel kimliği (kullanıcı veya hizmet ilkesi olabilir), rol tanımı (rol) kimliği ve Azure AD kaynak kapsamını birleştirir.

HTTP özel bir rol atamaisteği.

POST

``` HTTP
https://graph.microsoft.com/beta/roleManagement/directory/roleAssignments
```

Gövde

``` HTTP
{
    "principalId":"<PROVIDE OBJECTID OF USER TO ASSIGN HERE>",
    "roleDefinitionId":"<PROVIDE OBJECTID OF ROLE DEFINITION HERE>",
    "resourceScopes":["/"]
}
```

## <a name="next-steps"></a>Sonraki adımlar

- Azure AD yönetim rolleri [forumunda](https://feedback.azure.com/forums/169401-azure-active-directory?category_id=166032)bizimle paylaşmakta çekinmeyin.
- Roller ve Yönetici rol ataması hakkında daha fazla şey için [yönetici rollerini atay'a](directory-assign-admin-roles.md)bakın.
- Varsayılan kullanıcı izinleri için varsayılan [konuk ve üye kullanıcı izinlerinin karşılaştırılmasına](../fundamentals/users-default-permissions.md)bakın.
