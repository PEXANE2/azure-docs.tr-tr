---
title: Tutorial - Create a custom role for Azure resources using Azure PowerShell
description: Get started creating a custom role for Azure resources using Azure PowerShell in this tutorial.
services: active-directory
documentationCenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: role-based-access-control
ms.devlang: ''
ms.topic: tutorial
ms.tgt_pltfrm: ''
ms.workload: identity
ms.date: 02/20/2019
ms.author: rolyon
ms.openlocfilehash: 176e465163d92156308eda64c4187467cc10ee15
ms.sourcegitcommit: 4c831e768bb43e232de9738b363063590faa0472
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/23/2019
ms.locfileid: "74419754"
---
# <a name="tutorial-create-a-custom-role-for-azure-resources-using-azure-powershell"></a>Tutorial: Create a custom role for Azure resources using Azure PowerShell

If the [built-in roles for Azure resources](built-in-roles.md) don't meet the specific needs of your organization, you can create your own custom roles. Bu öğretici için Azure PowerShell'i kullanarak Reader Support Tickets adlı özel bir rol oluşturacaksınız. The custom role allows the user to view everything in the management plane of a subscription and also open support tickets.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Özel rol oluşturma
> * Özel rolleri listeleme
> * Özel rolü güncelleştirme
> * Özel rolü silme

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [az-powershell-update](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

- [Sahip](built-in-roles.md#owner) veya [Kullanıcı Erişimi Yöneticisi](built-in-roles.md#user-access-administrator) gibi özel rol oluşturma izni
- [Azure Cloud Shell](../cloud-shell/overview.md) or [Azure PowerShell](/powershell/azure/install-az-ps)

## <a name="sign-in-to-azure-powershell"></a>Azure PowerShell oturumu açma

[Azure PowerShell](/powershell/azure/authenticate-azureps) oturumu açın.

## <a name="create-a-custom-role"></a>Özel rol oluşturma

Özel rol oluşturmanın en kolay yolu yerleşik rolle başlayıp düzenledikten sonra yeni bir rol oluşturmaktır.

1. In PowerShell, use the [Get-AzProviderOperation](/powershell/module/az.resources/get-azprovideroperation) command to get the list of operations for the Microsoft.Support resource provider. İzinlerinizi oluşturmak için kullanabileceğiniz işlemleri bilmeniz yararlıdır. İşlemlerin tam listesini [Azure Resource Manager kaynak sağlayıcısı işlemleri](resource-provider-operations.md#microsoftsupport) sayfasında da görebilirsiniz.

    ```azurepowershell
    Get-AzProviderOperation "Microsoft.Support/*" | FT Operation, Description -AutoSize
    ```
    
    ```Output
    Operation                              Description
    ---------                              -----------
    Microsoft.Support/register/action      Registers to Support Resource Provider
    Microsoft.Support/supportTickets/read  Gets Support Ticket details (including status, severity, contact ...
    Microsoft.Support/supportTickets/write Creates or Updates a Support Ticket. You can create a Support Tic...
    ```

1. Use the [Get-AzRoleDefinition](/powershell/module/az.resources/get-azroledefinition) command to output the [Reader](built-in-roles.md#reader) role in JSON format.

    ```azurepowershell
    Get-AzRoleDefinition -Name "Reader" | ConvertTo-Json | Out-File C:\CustomRoles\ReaderSupportRole.json
    ```

1. **ReaderSupportRole.json** dosyasını bir düzenleyicide açın.

    Aşağıdaki JSON çıktısı gösterilir. Farklı özellikler hakkında bilgi edinmek için bkz. [Özel roller](custom-roles.md).

    ```json
    {
      "Name": "Reader",
      "Id": "acdd72a7-3385-48ef-bd42-f606fba81ae7",
      "IsCustom": false,
      "Description": "Lets you view everything, but not make any changes.",
      "Actions": [
        "*/read"
      ],
      "NotActions": [],
      "DataActions": [],
      "NotDataActions": [],
      "AssignableScopes": [
        "/"
      ]
    }
    ```
    
1. JSON dosyasını düzenleyerek `"Microsoft.Support/*"` işlemini `Actions` özelliğine ekleyin. Okuma işleminden sonra virgül eklemeyi unutmayın. Bu eylem, kullanıcıya destek bileti oluşturma izni verecektir.

1. Get the ID of your subscription using the [Get-AzSubscription](/powershell/module/Az.Accounts/Get-AzSubscription) command.

    ```azurepowershell
    Get-AzSubscription
    ```

1. `AssignableScopes` içine abonelik kimliğinizi şu biçimde ekleyin: `"/subscriptions/00000000-0000-0000-0000-000000000000"`

    Açık abonelik kimliklerini girmeniz gerekir, aksi halde rolü aboneliğinize aktaramazsınız.

1. `Id` özellik satırını silin ve `IsCustom` özelliğini `true` olarak değiştirin.

1. `Name` ve `Description` özelliklerini "Okuyucu Destek Biletleri" ve "Abonelikteki her şeyi görüntüleme ve destek bileti açma." olarak değiştirin.

    JSON dosyanız aşağıdaki gibi görünmelidir:

    ```json
    {
      "Name": "Reader Support Tickets",
      "IsCustom": true,
      "Description": "View everything in the subscription and also open support tickets.",
      "Actions": [
        "*/read",
        "Microsoft.Support/*"
      ],
      "NotActions": [],
      "DataActions": [],
      "NotDataActions": [],
      "AssignableScopes": [
        "/subscriptions/00000000-0000-0000-0000-000000000000"
      ]
    }
    ```
    
1. To create the new custom role, use the [New-AzRoleDefinition](/powershell/module/az.resources/new-azroledefinition) command and specify the JSON role definition file.

    ```azurepowershell
    New-AzRoleDefinition -InputFile "C:\CustomRoles\ReaderSupportRole.json"
    ```

    ```Output
    Name             : Reader Support Tickets
    Id               : 22222222-2222-2222-2222-222222222222
    IsCustom         : True
    Description      : View everything in the subscription and also open support tickets.
    Actions          : {*/read, Microsoft.Support/*}
    NotActions       : {}
    DataActions      : {}
    NotDataActions   : {}
    AssignableScopes : {/subscriptions/00000000-0000-0000-0000-000000000000}
    ```
        
    Yeni özel rol artık Azure portalında kullanılabilir ve yerleşik roller gibi kullanıcılara, gruplara veya hizmet sorumlularına atanabilir.

## <a name="list-custom-roles"></a>Özel rolleri listeleme

- To list all your custom roles, use the [Get-AzRoleDefinition](/powershell/module/az.resources/get-azroledefinition) command.

    ```azurepowershell
    Get-AzRoleDefinition | ? {$_.IsCustom -eq $true} | FT Name, IsCustom
    ```

    ```Output
    Name                   IsCustom
    ----                   --------
    Reader Support Tickets     True
    ```
    
    Özel rolü Azure portalında da görebilirsiniz.

    ![Azure portalına aktarılmış olan özel rolün ekran görüntüsü](./media/tutorial-custom-role-powershell/custom-role-reader-support-tickets.png)

## <a name="update-a-custom-role"></a>Özel rolü güncelleştirme

Özel rolü güncelleştirmek için JSON dosyasını güncelleştirebilir veya `PSRoleDefinition` nesnesini kullanabilirsiniz.

1. To update the JSON file, use the [Get-AzRoleDefinition](/powershell/module/az.resources/get-azroledefinition) command to output the custom role in JSON format.

    ```azurepowershell
    Get-AzRoleDefinition -Name "Reader Support Tickets" | ConvertTo-Json | Out-File C:\CustomRoles\ReaderSupportRole2.json
    ```

1. Dosyayı bir düzenleyicide açın.

1. `Actions` içine kaynak grubu dağıtımlarını oluşturma ve yönetme işlemini ekleyin: `"Microsoft.Resources/deployments/*"`.

    Güncelleştirilmiş JSON dosyanız aşağıdaki gibi görünmelidir:

    ```json
    {
      "Name": "Reader Support Tickets",
      "Id": "22222222-2222-2222-2222-222222222222",
      "IsCustom": true,
      "Description": "View everything in the subscription and also open support tickets.",
      "Actions": [
        "*/read",
        "Microsoft.Support/*",
        "Microsoft.Resources/deployments/*"
      ],
      "NotActions": [],
      "DataActions": [],
      "NotDataActions": [],
      "AssignableScopes": [
        "/subscriptions/00000000-0000-0000-0000-000000000000"
      ]
    }
    ```
        
1. To update the custom role, use the [Set-AzRoleDefinition](/powershell/module/az.resources/set-azroledefinition) command and specify the updated JSON file.

    ```azurepowershell
    Set-AzRoleDefinition -InputFile "C:\CustomRoles\ReaderSupportRole2.json"
    ```

    ```Output
    Name             : Reader Support Tickets
    Id               : 22222222-2222-2222-2222-222222222222
    IsCustom         : True
    Description      : View everything in the subscription and also open support tickets.
    Actions          : {*/read, Microsoft.Support/*, Microsoft.Resources/deployments/*}
    NotActions       : {}
    DataActions      : {}
    NotDataActions   : {}
    AssignableScopes : {/subscriptions/00000000-0000-0000-0000-000000000000}
    ```

1. To use the `PSRoleDefintion` object to update your custom role, first use the [Get-AzRoleDefinition](/powershell/module/az.resources/get-azroledefinition) command to get the role.

    ```azurepowershell
    $role = Get-AzRoleDefinition "Reader Support Tickets"
    ```
    
1. Tanılama ayarlarını okuma işlemini eklemek için `Add` metodunu çağırın.

    ```azurepowershell
    $role.Actions.Add("Microsoft.Insights/diagnosticSettings/*/read")
    ```

1. Use the [Set-AzRoleDefinition](/powershell/module/az.resources/set-azroledefinition) to update the role.

    ```azurepowershell
    Set-AzRoleDefinition -Role $role
    ```
    
    ```Output
    Name             : Reader Support Tickets
    Id               : 22222222-2222-2222-2222-222222222222
    IsCustom         : True
    Description      : View everything in the subscription and also open support tickets.
    Actions          : {*/read, Microsoft.Support/*, Microsoft.Resources/deployments/*,
                       Microsoft.Insights/diagnosticSettings/*/read}
    NotActions       : {}
    DataActions      : {}
    NotDataActions   : {}
    AssignableScopes : {/subscriptions/00000000-0000-0000-0000-000000000000}
    ```
    
## <a name="delete-a-custom-role"></a>Özel rolü silme

1. Use the [Get-AzRoleDefinition](/powershell/module/az.resources/get-azroledefinition) command to get the ID of the custom role.

    ```azurepowershell
    Get-AzRoleDefinition "Reader Support Tickets"
    ```

1. Use the [Remove-AzRoleDefinition](/powershell/module/az.resources/remove-azroledefinition) command and specify the role ID to delete the custom role.

    ```azurepowershell
    Remove-AzRoleDefinition -Id "22222222-2222-2222-2222-222222222222"
    ```

    ```Output
    Confirm
    Are you sure you want to remove role definition with id '22222222-2222-2222-2222-222222222222'.
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):
    ```

1. Onaylamanız istendiğinde **Y** yazın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure PowerShell kullanarak Azure kaynakları için özel roller oluşturma](custom-roles-powershell.md)
