---
title: Öğretici-Azure PowerShell kullanarak Azure kaynakları için özel bir rol oluşturma
description: Bu öğreticide Azure PowerShell kullanarak Azure kaynakları için özel bir rol oluşturmaya başlayın.
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
# <a name="tutorial-create-a-custom-role-for-azure-resources-using-azure-powershell"></a>Öğretici: Azure PowerShell kullanarak Azure kaynakları için özel bir rol oluşturma

[Azure kaynaklarına yönelik yerleşik roller](built-in-roles.md) , kuruluşunuzun belirli ihtiyaçlarını karşılamıyorsa, kendi özel rollerinizi oluşturabilirsiniz. Bu öğretici için Azure PowerShell'i kullanarak Reader Support Tickets adlı özel bir rol oluşturacaksınız. Özel rol, kullanıcının bir aboneliğin yönetim düzlemine ait her şeyi görüntülemesine olanak sağlar ve destek biletlerini de açabilir.

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
- [Azure Cloud Shell](../cloud-shell/overview.md) veya [Azure PowerShell](/powershell/azure/install-az-ps)

## <a name="sign-in-to-azure-powershell"></a>Azure PowerShell oturumu açma

[Azure PowerShell](/powershell/azure/authenticate-azureps) oturumu açın.

## <a name="create-a-custom-role"></a>Özel rol oluşturma

Özel rol oluşturmanın en kolay yolu yerleşik rolle başlayıp düzenledikten sonra yeni bir rol oluşturmaktır.

1. PowerShell 'de, Microsoft. support kaynak sağlayıcısı 'nın işlem listesini almak için [Get-AzProviderOperation](/powershell/module/az.resources/get-azprovideroperation) komutunu kullanın. İzinlerinizi oluşturmak için kullanabileceğiniz işlemleri bilmeniz yararlıdır. İşlemlerin tam listesini [Azure Resource Manager kaynak sağlayıcısı işlemleri](resource-provider-operations.md#microsoftsupport) sayfasında da görebilirsiniz.

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

1. [Reader](built-in-roles.md#reader) rolünü JSON biçiminde çıkarmak için [Get-azroledefinition](/powershell/module/az.resources/get-azroledefinition) komutunu kullanın.

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

1. [Get-AzSubscription](/powershell/module/Az.Accounts/Get-AzSubscription) komutunu kullanarak aboneliğinizin kimliğini alın.

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
    
1. Yeni özel rolü oluşturmak için [New-AzRoleDefinition](/powershell/module/az.resources/new-azroledefinition) komutunu kullanın ve JSON rol tanımı dosyasını belirtin.

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

- Tüm özel rollerinizi listelemek için [Get-AzRoleDefinition](/powershell/module/az.resources/get-azroledefinition) komutunu kullanın.

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

1. JSON dosyasını güncelleştirmek için, özel rolü JSON biçiminde çıkarmak üzere [Get-AzRoleDefinition](/powershell/module/az.resources/get-azroledefinition) komutunu kullanın.

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
        
1. Özel rolü güncelleştirmek için [set-AzRoleDefinition](/powershell/module/az.resources/set-azroledefinition) komutunu kullanın ve güncelleştirilmiş json dosyasını belirtin.

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

1. Özel rolünüzü güncelleştirmek için `PSRoleDefintion` nesnesini kullanmak için, önce rolü almak üzere [Get-AzRoleDefinition](/powershell/module/az.resources/get-azroledefinition) komutunu kullanın.

    ```azurepowershell
    $role = Get-AzRoleDefinition "Reader Support Tickets"
    ```
    
1. Tanılama ayarlarını okuma işlemini eklemek için `Add` metodunu çağırın.

    ```azurepowershell
    $role.Actions.Add("Microsoft.Insights/diagnosticSettings/*/read")
    ```

1. Rolü güncelleştirmek için [set-AzRoleDefinition](/powershell/module/az.resources/set-azroledefinition) ' i kullanın.

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

1. Özel rolün KIMLIĞINI almak için [Get-AzRoleDefinition](/powershell/module/az.resources/get-azroledefinition) komutunu kullanın.

    ```azurepowershell
    Get-AzRoleDefinition "Reader Support Tickets"
    ```

1. [Remove-AzRoleDefinition](/powershell/module/az.resources/remove-azroledefinition) komutunu kullanın ve özel rolü silmek IÇIN rol kimliğini belirtin.

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
