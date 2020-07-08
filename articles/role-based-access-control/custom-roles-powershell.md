---
title: Azure PowerShell-Azure RBAC kullanarak Azure özel rolleri oluşturma veya güncelleştirme
description: Azure PowerShell ve Azure rol tabanlı erişim denetimi (Azure RBAC) kullanarak özel rolleri listeleme, oluşturma, güncelleştirme veya silme hakkında bilgi edinin.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: 9e225dba-9044-4b13-b573-2f30d77925a9
ms.service: role-based-access-control
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/18/2020
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 540da4103c3f7800521407441d645070e1e3e7ca
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84790220"
---
# <a name="create-or-update-azure-custom-roles-using-azure-powershell"></a>Azure PowerShell kullanarak Azure özel rolleri oluşturma veya güncelleştirme

> [!IMPORTANT]
> ' Ye bir yönetim grubu eklemek `AssignableScopes` Şu anda önizlemededir.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

[Azure yerleşik rolleri](built-in-roles.md) , kuruluşunuzun belirli ihtiyaçlarını karşılamıyorsa, kendi özel rollerinizi de oluşturabilirsiniz. Bu makalede, Azure PowerShell kullanarak özel rolleri listeleme, oluşturma, güncelleştirme veya silme işlemlerinin nasıl yapılacağı açıklanır.

Özel rol oluşturma hakkında adım adım bir öğretici için bkz. [öğretici: Azure PowerShell kullanarak Azure özel rolü oluşturma](tutorial-custom-role-powershell.md).

[!INCLUDE [az-powershell-update](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Ön koşullar

Özel Roller oluşturmak için şunlar gerekir:

- [Sahip](built-in-roles.md#owner) veya [Kullanıcı Erişimi Yöneticisi](built-in-roles.md#user-access-administrator) gibi özel rol oluşturma izni
- [Azure Cloud Shell](../cloud-shell/overview.md) veya [Azure PowerShell](/powershell/azure/install-az-ps)

## <a name="list-custom-roles"></a>Özel rolleri listeleme

Bir kapsamda atanmak üzere kullanılabilen rolleri listelemek için [Get-AzRoleDefinition](/powershell/module/az.resources/get-azroledefinition) komutunu kullanın. Aşağıdaki örnek seçili abonelikte atama için kullanılabilen tüm rolleri listeler.

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

Aşağıdaki örnekte yalnızca seçili abonelikte atama için kullanılabilen özel roller listelenmektedir.

```azurepowershell
Get-AzRoleDefinition | ? {$_.IsCustom -eq $true} | FT Name, IsCustom
```

```Example
Name                     IsCustom
----                     --------
Virtual Machine Operator     True
```

Seçili abonelik rolün içinde değilse `AssignableScopes` , özel rol listelenmez.

## <a name="list-a-custom-role-definition"></a>Özel bir rol tanımı listeleme

Özel bir rol tanımı listelemek için [Get-AzRoleDefinition](/powershell/module/az.resources/get-azroledefinition)komutunu kullanın. Bu, yerleşik bir rol için kullandığınız komutla aynıdır.

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

Aşağıdaki örnek yalnızca rolün eylemlerini listeler:

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

Özel bir rol oluşturmak için [New-AzRoleDefinition](/powershell/module/az.resources/new-azroledefinition) komutunu kullanın. Rol, `PSRoleDefinition` nesne veya JSON şablonu kullanarak yapılandırmak için iki yöntem vardır. 

### <a name="get-operations-for-a-resource-provider"></a>Bir kaynak sağlayıcısı için işlemleri al

Özel roller oluşturduğunuzda, kaynak sağlayıcılardan tüm olası işlemleri bilmeniz önemlidir.
[Kaynak sağlayıcısı işlemlerinin](resource-provider-operations.md) listesini görüntüleyebilir veya bu bilgileri almak için [Get-azprovideroperation](/powershell/module/az.resources/get-azprovideroperation) komutunu kullanabilirsiniz.
Örneğin, sanal makineler için tüm kullanılabilir işlemleri denetlemek istiyorsanız şu komutu kullanın:

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

### <a name="create-a-custom-role-with-the-psroledefinition-object"></a>PSRoleDefinition nesnesiyle özel bir rol oluşturma

Özel bir rol oluşturmak için PowerShell kullandığınızda, [yerleşik rollerden](built-in-roles.md) birini başlangıç noktası olarak kullanabilir veya sıfırdan başlayabilirsiniz. Bu bölümdeki ilk örnek, yerleşik bir rolle başlar ve daha sonra daha fazla izinle özelleştirir. İstediğiniz, veya öğesini eklemek için özniteliklerini düzenleyin `Actions` `NotActions` `AssignableScopes` ve ardından değişiklikleri yeni bir rol olarak kaydedin.

Aşağıdaki örnek, sanal makine [katılımcısı](built-in-roles.md#virtual-machine-contributor) adlı yerleşik rol ile başlar ve *sanal makine operatörü*adlı özel bir rol oluşturulur. Yeni rol, *Microsoft. COMPUTE*, *Microsoft. Storage*ve *Microsoft. Network* kaynak sağlayıcılarının tüm okuma işlemlerine erişim verir ve sanal makineleri başlatma, yeniden başlatma ve izleme erişimi verir. Özel rol iki abonelik için kullanılabilir.

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

Aşağıdaki örnek, *sanal makine operatörü* özel rolünü oluşturmak için başka bir yol göstermektedir. Yeni bir nesne oluşturularak başlatılır `PSRoleDefinition` . Eylem işlemleri `perms` değişkende belirtilir ve `Actions` özelliğine ayarlanır. `NotActions`Özelliği, `NotActions` [sanal makine katılımcısı](built-in-roles.md#virtual-machine-contributor) yerleşik rolü tarafından okunarak ayarlanır. [Sanal makine katılımcısı](built-in-roles.md#virtual-machine-contributor) olmadığından `NotActions` , bu satır gerekli değildir, ancak bilgilerin başka bir rolden nasıl alınamayacağını gösterir.

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

JSON şablonu, özel rol için kaynak tanımı olarak kullanılabilir. Aşağıdaki örnek, depolama ve işlem kaynaklarına okuma erişimi, desteğe erişim ve bu rolü iki aboneliğe ekleme izni veren özel bir rol oluşturur. Aşağıdaki örneğe sahip yeni bir dosya oluşturun `C:\CustomRoles\customrole1.json` . `null`Otomatik olarak yeni BIR kimlik oluşturulduğundan kimlik, ilk rol oluşturma sırasında olarak ayarlanmalıdır. 

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

Özel bir rol oluşturmaya benzer şekilde, `PSRoleDefinition` nesne veya JSON şablonu kullanarak var olan bir özel rolü de değiştirebilirsiniz.

### <a name="update-a-custom-role-with-the-psroledefinition-object"></a>PSRoleDefinition nesnesiyle özel bir rol güncelleştirme

Özel bir rolü değiştirmek için önce [Get-AzRoleDefinition](/powershell/module/az.resources/get-azroledefinition) komutunu kullanarak rol tanımını alın. İkinci olarak, rol tanımında istenen değişiklikleri yapın. Son olarak, değiştirilen rol tanımını kaydetmek için [set-AzRoleDefinition](/powershell/module/az.resources/set-azroledefinition) komutunu kullanın.

Aşağıdaki örnek, `Microsoft.Insights/diagnosticSettings/*` Işlemi *sanal makine operatörü* özel rolüne ekler.

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

Aşağıdaki örnek, *sanal makine operatörü* özel rolünün atanabilir kapsamlarına bir Azure aboneliği ekler.

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

Aşağıdaki örnek, `AssignableScopes` *sanal makine operatörü* özel rolüne bir yönetim grubu ekler. ' Ye bir yönetim grubu eklemek `AssignableScopes` Şu anda önizlemededir.

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

### <a name="update-a-custom-role-with-a-json-template"></a>JSON şablonuyla özel bir rol güncelleştirme

Önceki JSON şablonunu kullanarak, eylemleri eklemek veya kaldırmak için mevcut bir özel rolü kolayca değiştirebilirsiniz. JSON şablonunu güncelleştirin ve aşağıdaki örnekte gösterildiği gibi ağ için okuma eylemi ekleyin. Şablonda listelenen tanımlar, var olan bir tanıma göre tamamen uygulanmaz, bu da rolün şablonda belirttiğiniz şekilde tam olarak göründüğü anlamına gelir. Ayrıca, kimlik alanını rolün KIMLIĞIYLE güncelleştirmeniz gerekir. Bu değerin ne olduğundan emin değilseniz, bu bilgileri almak için [Get-AzRoleDefinition](/powershell/module/az.resources/get-azroledefinition) cmdlet 'ini kullanabilirsiniz.

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

Mevcut rolü güncelleştirmek için aşağıdaki PowerShell komutunu çalıştırın:

```azurepowershell
Set-AzRoleDefinition -InputFile "C:\CustomRoles\customrole1.json"
```

## <a name="delete-a-custom-role"></a>Özel rolü silme

Özel bir rolü silmek için [Remove-AzRoleDefinition](/powershell/module/az.resources/remove-azroledefinition) komutunu kullanın.

Aşağıdaki örnek, *sanal makine operatörü* özel rolünü kaldırır.

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

- [Öğretici: Azure PowerShell kullanarak bir Azure özel rolü oluşturma](tutorial-custom-role-powershell.md)
- [Azure özel rolleri](custom-roles.md)
- [Azure Resource Manager kaynak sağlayıcısı işlemleri](resource-provider-operations.md)
