---
title: Azure PowerShell ile Azure kaynakları için özel roller oluşturma veya güncelleştirme
description: Azure PowerShell'i kullanarak Azure kaynakları için rol tabanlı erişim denetimi (RBAC) ile özel rolleri nasıl listeleyişsüreceğini, oluşturarak, güncelleştirecek veya silebilirsiniz.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: 9e225dba-9044-4b13-b573-2f30d77925a9
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/18/2020
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 3c72e04ff7a08fecc2ef352a5879898c4c6d41c9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80062272"
---
# <a name="create-or-update-custom-roles-for-azure-resources-using-azure-powershell"></a>Azure PowerShell'i kullanarak Azure kaynakları için özel roller oluşturma veya güncelleştirme

> [!IMPORTANT]
> Bir yönetim grubu `AssignableScopes` eklemek şu anda önizlemededir.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.

Azure [kaynakları için yerleşik roller](built-in-roles.md) kuruluşunuzun özel gereksinimlerini karşılamazsa, kendi özel rollerinizi oluşturabilirsiniz. Bu makalede, Azure PowerShell kullanarak özel rollerin nasıl listelenebildiğini, oluşturularak, güncelleştirilenveya silininin açıklanmaktadır.

Özel bir rolün nasıl oluşturulabildiğini anlatan adım adım öğretici için [Bkz.](tutorial-custom-role-powershell.md)

[!INCLUDE [az-powershell-update](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Ön koşullar

Özel roller oluşturmak için şunları yapmanız gerekir:

- [Sahip](built-in-roles.md#owner) veya [Kullanıcı Erişimi Yöneticisi](built-in-roles.md#user-access-administrator) gibi özel rol oluşturma izni
- [Azure Bulut Shell](../cloud-shell/overview.md) veya [Azure PowerShell](/powershell/azure/install-az-ps)

## <a name="list-custom-roles"></a>Özel rolleri listeleme

Bir kapsamda atama için kullanılabilen rolleri listelemek için [Get-AzRoleDefinition](/powershell/module/az.resources/get-azroledefinition) komutunu kullanın. Aşağıdaki örnekte, seçili abonelikte atama için kullanılabilen tüm roller listelenir.

```azurepowershell
Get-AzRoleDefinition | FT Name, IsCustom
```

```Example
Name                                              IsCustom
----                                              --------
Virtual Machine Operator                              True
AcrImageSigner                                       False
AcrQuarantineReader                                  False
AcrQuarantineWriter                                  False
API Management Service Contributor                   False
...
```

Aşağıdaki örnekte, yalnızca seçili abonelikte atama için kullanılabilen özel rolleri listelenir.

```azurepowershell
Get-AzRoleDefinition | ? {$_.IsCustom -eq $true} | FT Name, IsCustom
```

```Example
Name                     IsCustom
----                     --------
Virtual Machine Operator     True
```

Seçili abonelik rolün `AssignableScopes` içinde değilse, özel rol listelenmez.

## <a name="list-a-custom-role-definition"></a>Özel rol tanımını listelama

Özel bir rol tanımı nı listelemek için [Get-AzRoleDefinition'ı](/powershell/module/az.resources/get-azroledefinition)kullanın. Bu, yerleşik bir rol için kullandığınız komutla aynıdır.

```azurepowershell
Get-AzRoleDefinition <role_name> | ConvertTo-Json
```

```Example
PS C:\> Get-AzRoleDefinition "Virtual Machine Operator" | ConvertTo-Json

{
  "Name": "Virtual Machine Operator",
  "Id": "00000000-0000-0000-0000-000000000000",
  "IsCustom": true,
  "Description": "Can monitor and restart virtual machines.",
  "Actions": [
    "Microsoft.Storage/*/read",
    "Microsoft.Network/*/read",
    "Microsoft.Compute/*/read",
    "Microsoft.Compute/virtualMachines/start/action",
    "Microsoft.Compute/virtualMachines/restart/action",
    "Microsoft.Authorization/*/read",
    "Microsoft.ResourceHealth/availabilityStatuses/read",
    "Microsoft.Resources/subscriptions/resourceGroups/read",
    "Microsoft.Insights/alertRules/*",
    "Microsoft.Support/*"
  ],
  "NotActions": [],
  "DataActions": [],
  "NotDataActions": [],
  "AssignableScopes": [
    "/subscriptions/11111111-1111-1111-1111-111111111111"
  ]
}
```

Aşağıdaki örnekte yalnızca rolün eylemleri listelenebvardır:

```azurepowershell
(Get-AzRoleDefinition <role_name>).Actions
```

```Example
PS C:\> (Get-AzRoleDefinition "Virtual Machine Operator").Actions

"Microsoft.Storage/*/read",
"Microsoft.Network/*/read",
"Microsoft.Compute/*/read",
"Microsoft.Compute/virtualMachines/start/action",
"Microsoft.Compute/virtualMachines/restart/action",
"Microsoft.Authorization/*/read",
"Microsoft.ResourceHealth/availabilityStatuses/read",
"Microsoft.Resources/subscriptions/resourceGroups/read",
"Microsoft.Insights/alertRules/*",
"Microsoft.Insights/diagnosticSettings/*",
"Microsoft.Support/*"
```

## <a name="create-a-custom-role"></a>Özel rol oluşturma

Özel bir rol oluşturmak için [Yeni-AzRoleDefinition](/powershell/module/az.resources/new-azroledefinition) komutunu kullanın. Nesne veya JSON şablonu `PSRoleDefinition` kullanarak rolü yapılandırmak için iki yöntem vardır. 

### <a name="get-operations-for-a-resource-provider"></a>Kaynak sağlayıcısı için işlem alma

Özel roller oluşturduğunuzda, kaynak sağlayıcılardan tüm olası işlemleri bilmek önemlidir.
[Kaynak sağlayıcı işlemleri](resource-provider-operations.md) listesini görüntüleyebilir veya bu bilgileri almak için [Get-AzProviderOperation](/powershell/module/az.resources/get-azprovideroperation) komutunu kullanabilirsiniz.
Örneğin, sanal makineler için kullanılabilir tüm işlemleri denetlemek istiyorsanız, şu komutu kullanın:

```azurepowershell
Get-AzProviderOperation <operation> | FT OperationName, Operation, Description -AutoSize
```

```Example
PS C:\> Get-AzProviderOperation "Microsoft.Compute/virtualMachines/*" | FT OperationName, Operation, Description -AutoSize

OperationName                                  Operation                                                      Description
-------------                                  ---------                                                      -----------
Get Virtual Machine                            Microsoft.Compute/virtualMachines/read                         Get the propertie...
Create or Update Virtual Machine               Microsoft.Compute/virtualMachines/write                        Creates a new vir...
Delete Virtual Machine                         Microsoft.Compute/virtualMachines/delete                       Deletes the virtu...
Start Virtual Machine                          Microsoft.Compute/virtualMachines/start/action                 Starts the virtua...
...
```

### <a name="create-a-custom-role-with-the-psroledefinition-object"></a>PSRoleDefinition nesnesi ile özel bir rol oluşturma

Özel bir rol oluşturmak için PowerShell'i kullandığınızda, [yerleşik rollerden](built-in-roles.md) birini başlangıç noktası olarak kullanabilir veya sıfırdan başlayabilirsiniz. Bu bölümdeki ilk örnek yerleşik bir rol ile başlar ve daha sonra daha fazla izinle özelleştirilir. ", istediğiniz `NotActions`veya `AssignableScopes` `Actions`istediğiniz" özniteliklerini edin ve değişiklikleri yeni bir rol olarak kaydedin.

Aşağıdaki örnek, Sanal Makine *Operatörü*adlı özel bir rol oluşturmak için [Sanal Makine Katılımcısı](built-in-roles.md#virtual-machine-contributor) yerleşik rolüyle başlar. Yeni rol, *Microsoft.Compute, Microsoft.Storage* *Microsoft.Storage*ve *Microsoft.Network* kaynak sağlayıcılarının tüm okuma işlemlerine erişim sağlar ve sanal makineleri başlatma, yeniden başlatma ve izleme erişimine erişim sağlar. Özel rol iki abonelikte kullanılabilir.

```azurepowershell
$role = Get-AzRoleDefinition "Virtual Machine Contributor"
$role.Id = $null
$role.Name = "Virtual Machine Operator"
$role.Description = "Can monitor and restart virtual machines."
$role.Actions.Clear()
$role.Actions.Add("Microsoft.Storage/*/read")
$role.Actions.Add("Microsoft.Network/*/read")
$role.Actions.Add("Microsoft.Compute/*/read")
$role.Actions.Add("Microsoft.Compute/virtualMachines/start/action")
$role.Actions.Add("Microsoft.Compute/virtualMachines/restart/action")
$role.Actions.Add("Microsoft.Authorization/*/read")
$role.Actions.Add("Microsoft.ResourceHealth/availabilityStatuses/read")
$role.Actions.Add("Microsoft.Resources/subscriptions/resourceGroups/read")
$role.Actions.Add("Microsoft.Insights/alertRules/*")
$role.Actions.Add("Microsoft.Support/*")
$role.AssignableScopes.Clear()
$role.AssignableScopes.Add("/subscriptions/00000000-0000-0000-0000-000000000000")
$role.AssignableScopes.Add("/subscriptions/11111111-1111-1111-1111-111111111111")
New-AzRoleDefinition -Role $role
```

Aşağıdaki örnek, Sanal Makine *Operatörü* özel rolünü oluşturmanın başka bir yolunu gösterir. Yeni `PSRoleDefinition` bir nesne oluşturarak başlar. Eylem işlemleri `perms` değişkende belirtilir ve `Actions` özelliğe ayarlanır. Özellik, `NotActions` Sanal Makine `NotActions` [Katılımcısı'nın](built-in-roles.md#virtual-machine-contributor) yerleşik rolünü okuyarak ayarlanır. [Sanal Makine Katılımcısı](built-in-roles.md#virtual-machine-contributor) herhangi `NotActions`bir, bu satır gerekli değildir, ancak bilgi başka bir rolden alınabilir nasıl gösterir.

```azurepowershell
$role = [Microsoft.Azure.Commands.Resources.Models.Authorization.PSRoleDefinition]::new()
$role.Name = 'Virtual Machine Operator 2'
$role.Description = 'Can monitor and restart virtual machines.'
$role.IsCustom = $true
$perms = 'Microsoft.Storage/*/read','Microsoft.Network/*/read','Microsoft.Compute/*/read'
$perms += 'Microsoft.Compute/virtualMachines/start/action','Microsoft.Compute/virtualMachines/restart/action'
$perms += 'Microsoft.Authorization/*/read'
$perms += 'Microsoft.ResourceHealth/availabilityStatuses/read'
$perms += 'Microsoft.Resources/subscriptions/resourceGroups/read'
$perms += 'Microsoft.Insights/alertRules/*','Microsoft.Support/*'
$role.Actions = $perms
$role.NotActions = (Get-AzRoleDefinition -Name 'Virtual Machine Contributor').NotActions
$subs = '/subscriptions/00000000-0000-0000-0000-000000000000','/subscriptions/11111111-1111-1111-1111-111111111111'
$role.AssignableScopes = $subs
New-AzRoleDefinition -Role $role
```

### <a name="create-a-custom-role-with-json-template"></a>JSON şablonuyla özel bir rol oluşturma

JSON şablonu, özel rol için kaynak tanımı olarak kullanılabilir. Aşağıdaki örnek, depolama ve bilgi işlem kaynaklarına okuma erişimi, desteğe erişim sağlayan ve bu rolü iki aboneye ekleyen özel bir rol oluşturur. Aşağıdaki örnekle `C:\CustomRoles\customrole1.json` yeni bir dosya oluşturun. Yeni bir kimlik `null` otomatik olarak oluşturulduğundan, kimlik ilk rol oluşturmada ayarlanmalıdır. 

```json
{
  "Name": "Custom Role 1",
  "Id": null,
  "IsCustom": true,
  "Description": "Allows for read access to Azure storage and compute resources and access to support",
  "Actions": [
    "Microsoft.Compute/*/read",
    "Microsoft.Storage/*/read",
    "Microsoft.Support/*"
  ],
  "NotActions": [],
  "AssignableScopes": [
    "/subscriptions/00000000-0000-0000-0000-000000000000",
    "/subscriptions/11111111-1111-1111-1111-111111111111"
  ]
}
```

Rolü aboneliklere eklemek için aşağıdaki PowerShell komutunu çalıştırın:

```azurepowershell
New-AzRoleDefinition -InputFile "C:\CustomRoles\customrole1.json"
```

## <a name="update-a-custom-role"></a>Özel rolü güncelleştirme

Özel bir rol oluşturmaya benzer şekilde, nesne veya `PSRoleDefinition` JSON şablonu kullanarak varolan bir özel rolü değiştirebilirsiniz.

### <a name="update-a-custom-role-with-the-psroledefinition-object"></a>PSRoleDefinition nesnesi ile özel bir rolü güncelleştirme

Özel bir rolü değiştirmek için önce rol tanımını almak için [Get-AzRoleDefinition](/powershell/module/az.resources/get-azroledefinition) komutunu kullanın. İkinci olarak, rol tanımında istenen değişiklikleri yapın. Son olarak, değiştirilen rol tanımını kaydetmek için [Set-AzRoleDefinition](/powershell/module/az.resources/set-azroledefinition) komutunu kullanın.

Aşağıdaki örnek, `Microsoft.Insights/diagnosticSettings/*` işlemi *Sanal Makine Operatörü* özel rolüne ekler.

```azurepowershell
$role = Get-AzRoleDefinition "Virtual Machine Operator"
$role.Actions.Add("Microsoft.Insights/diagnosticSettings/*")
Set-AzRoleDefinition -Role $role
```

```Example
PS C:\> $role = Get-AzRoleDefinition "Virtual Machine Operator"
PS C:\> $role.Actions.Add("Microsoft.Insights/diagnosticSettings/*")
PS C:\> Set-AzRoleDefinition -Role $role

Name             : Virtual Machine Operator
Id               : 88888888-8888-8888-8888-888888888888
IsCustom         : True
Description      : Can monitor and restart virtual machines.
Actions          : {Microsoft.Storage/*/read, Microsoft.Network/*/read, Microsoft.Compute/*/read,
                   Microsoft.Compute/virtualMachines/start/action...}
NotActions       : {}
AssignableScopes : {/subscriptions/00000000-0000-0000-0000-000000000000,
                   /subscriptions/11111111-1111-1111-1111-111111111111}
```

Aşağıdaki örnek, *Virtual Machine Operatör* özel rolünün atanabilir kapsamlarına bir Azure aboneliği ekler.

```azurepowershell
Get-AzSubscription -SubscriptionName Production3

$role = Get-AzRoleDefinition "Virtual Machine Operator"
$role.AssignableScopes.Add("/subscriptions/22222222-2222-2222-2222-222222222222")
Set-AzRoleDefinition -Role $role
```

```Example
PS C:\> Get-AzSubscription -SubscriptionName Production3

Name     : Production3
Id       : 22222222-2222-2222-2222-222222222222
TenantId : 99999999-9999-9999-9999-999999999999
State    : Enabled

PS C:\> $role = Get-AzRoleDefinition "Virtual Machine Operator"
PS C:\> $role.AssignableScopes.Add("/subscriptions/22222222-2222-2222-2222-222222222222")
PS C:\> Set-AzRoleDefinition -Role $role

Name             : Virtual Machine Operator
Id               : 88888888-8888-8888-8888-888888888888
IsCustom         : True
Description      : Can monitor and restart virtual machines.
Actions          : {Microsoft.Storage/*/read, Microsoft.Network/*/read, Microsoft.Compute/*/read,
                   Microsoft.Compute/virtualMachines/start/action...}
NotActions       : {}
AssignableScopes : {/subscriptions/00000000-0000-0000-0000-000000000000,
                   /subscriptions/11111111-1111-1111-1111-111111111111,
                   /subscriptions/22222222-2222-2222-2222-222222222222}
```

Aşağıdaki örnek, Sanal Makine `AssignableScopes` *Operatörü* özel rolüne bir yönetim grubu ekler. Bir yönetim grubu `AssignableScopes` eklemek şu anda önizlemededir.

```azurepowershell
Get-AzManagementGroup

$role = Get-AzRoleDefinition "Virtual Machine Operator"
$role.AssignableScopes.Add("/providers/Microsoft.Management/managementGroups/{groupId1}")
Set-AzRoleDefinition -Role $role
```

```Example
PS C:\> Get-AzManagementGroup

Id          : /providers/Microsoft.Management/managementGroups/marketing-group
Type        : /providers/Microsoft.Management/managementGroups
Name        : marketing-group
TenantId    : 99999999-9999-9999-9999-999999999999
DisplayName : Marketing group

PS C:\> $role = Get-AzRoleDefinition "Virtual Machine Operator"
PS C:\> $role.AssignableScopes.Add("/providers/Microsoft.Management/managementGroups/marketing-group")
PS C:\> Set-AzRoleDefinition -Role $role

Name             : Virtual Machine Operator
Id               : 88888888-8888-8888-8888-888888888888
IsCustom         : True
Description      : Can monitor and restart virtual machines.
Actions          : {Microsoft.Storage/*/read, Microsoft.Network/*/read, Microsoft.Compute/*/read,
                   Microsoft.Compute/virtualMachines/start/action...}
NotActions       : {}
AssignableScopes : {/subscriptions/00000000-0000-0000-0000-000000000000,
                   /subscriptions/11111111-1111-1111-1111-111111111111,
                   /subscriptions/22222222-2222-2222-2222-222222222222,
                   /providers/Microsoft.Management/managementGroups/marketing-group}
```

### <a name="update-a-custom-role-with-a-json-template"></a>JSON şablonuyla özel bir rolü güncelleştirme

Önceki JSON şablonunu kullanarak, Eylemleri eklemek veya kaldırmak için varolan özel bir rolü kolayca değiştirebilirsiniz. JSON şablonu güncelleştirin ve aşağıdaki örnekte gösterildiği gibi ağ için okuma eylemini ekleyin. Şablonda listelenen tanımlar varolan bir tanıma kümülatif olarak uygulanmaz, bu da rolün şablonda belirttiğiniz gibi tam olarak göründüğü anlamına gelir. Ayrıca, kimliği rolün kimliğiyle birlikte Kimlik alanını güncelleştirmeniz gerekir. Bu değerin ne olduğundan emin değilseniz, bu bilgileri almak için [Get-AzRoleDefinition](/powershell/module/az.resources/get-azroledefinition) cmdlet'i kullanabilirsiniz.

```json
{
  "Name": "Custom Role 1",
  "Id": "acce7ded-2559-449d-bcd5-e9604e50bad1",
  "IsCustom": true,
  "Description": "Allows for read access to Azure storage and compute resources and access to support",
  "Actions": [
    "Microsoft.Compute/*/read",
    "Microsoft.Storage/*/read",
    "Microsoft.Network/*/read",
    "Microsoft.Support/*"
  ],
  "NotActions": [],
  "AssignableScopes": [
    "/subscriptions/00000000-0000-0000-0000-000000000000",
    "/subscriptions/11111111-1111-1111-1111-111111111111"
  ]
}
```

Varolan rolü güncelleştirmek için aşağıdaki PowerShell komutunu çalıştırın:

```azurepowershell
Set-AzRoleDefinition -InputFile "C:\CustomRoles\customrole1.json"
```

## <a name="delete-a-custom-role"></a>Özel rolü silme

Özel bir rolü silmek için [Remove-AzRoleDefinition](/powershell/module/az.resources/remove-azroledefinition) komutunu kullanın.

Aşağıdaki örnek, *Sanal Makine Operatörü* özel rolünü kaldırır.

```azurepowershell
Get-AzRoleDefinition "Virtual Machine Operator"
Get-AzRoleDefinition "Virtual Machine Operator" | Remove-AzRoleDefinition
```

```Example
PS C:\> Get-AzRoleDefinition "Virtual Machine Operator"

Name             : Virtual Machine Operator
Id               : 88888888-8888-8888-8888-888888888888
IsCustom         : True
Description      : Can monitor and restart virtual machines.
Actions          : {Microsoft.Storage/*/read, Microsoft.Network/*/read, Microsoft.Compute/*/read,
                   Microsoft.Compute/virtualMachines/start/action...}
NotActions       : {}
AssignableScopes : {/subscriptions/00000000-0000-0000-0000-000000000000,
                   /subscriptions/11111111-1111-1111-1111-111111111111}

PS C:\> Get-AzRoleDefinition "Virtual Machine Operator" | Remove-AzRoleDefinition

Confirm
Are you sure you want to remove role definition with name 'Virtual Machine Operator'.
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y
```

## <a name="next-steps"></a>Sonraki adımlar

- [Öğretici: Azure PowerShell'i kullanarak Azure kaynakları için özel bir rol oluşturma](tutorial-custom-role-powershell.md)
- [Azure kaynakları için özel roller](custom-roles.md)
- [Azure Kaynak Yöneticisi kaynak sağlayıcısı işlemleri](resource-provider-operations.md)
