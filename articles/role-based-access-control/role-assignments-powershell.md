---
title: Azure PowerShell-Azure RBAC kullanarak Azure rol atamaları ekleme veya kaldırma
description: Azure PowerShell ve Azure rol tabanlı erişim denetimi (Azure RBAC) kullanarak kullanıcılar, gruplar, hizmet sorumluları veya yönetilen kimlikler için Azure kaynaklarına erişim izni verme hakkında bilgi edinin.
services: active-directory
author: rolyon
manager: mtillman
ms.service: role-based-access-control
ms.topic: how-to
ms.workload: identity
ms.date: 11/25/2020
ms.author: rolyon
ms.openlocfilehash: 3bb09133ba6991554072b4bf68b5306c78f868a7
ms.sourcegitcommit: f6f928180504444470af713c32e7df667c17ac20
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/07/2021
ms.locfileid: "97964295"
---
# <a name="add-or-remove-azure-role-assignments-using-azure-powershell"></a>Azure PowerShell kullanarak Azure rol atamalarını ekleme veya kaldırma

[!INCLUDE [Azure RBAC definition grant access](../../includes/role-based-access-control/definition-grant.md)] Bu makalede, Azure PowerShell kullanarak rollerin nasıl atanacağı açıklanır.

[!INCLUDE [az-powershell-update](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Ön koşullar

Rol atamaları eklemek veya kaldırmak için şunları yapmanız gerekir:

- `Microsoft.Authorization/roleAssignments/write`ve `Microsoft.Authorization/roleAssignments/delete` [Kullanıcı erişimi Yöneticisi](built-in-roles.md#user-access-administrator) veya [sahibi](built-in-roles.md#owner) gibi izinler
- [Azure Cloud Shell](../cloud-shell/overview.md) veya [Azure PowerShell](/powershell/azure/install-az-ps) PowerShell
- PowerShell komutunu çalıştırmak için kullandığınız hesap Microsoft Graph iznine sahip olmalıdır `Directory.Read.All` .

## <a name="steps-to-add-a-role-assignment"></a>Rol ataması ekleme adımları

Azure RBAC 'de, erişim izni vermek için bir rol ataması eklersiniz. Rol ataması üç öğeden oluşur: güvenlik sorumlusu, rol tanımı ve kapsam. Rol ataması eklemek için aşağıdaki adımları izleyin.

### <a name="step-1-determine-who-needs-access"></a>1. Adım: kimlerin erişime ihtiyacı olduğunu belirleme

Bir Kullanıcı, Grup, hizmet sorumlusu veya yönetilen kimliğe bir rol atayabilirsiniz. Rol ataması eklemek için, nesnenin benzersiz KIMLIĞINI belirtmeniz gerekebilir. KIMLIK şu biçimdedir: `11111111-1111-1111-1111-111111111111` . KIMLIĞI Azure portal veya Azure PowerShell kullanarak alabilirsiniz.

**Kullanıcı**

Bir Azure AD kullanıcısı için, *patlong \@ contoso.com* veya Kullanıcı nesne kimliği gibi Kullanıcı asıl adını alın. Nesne KIMLIĞINI almak için [Get-AzADUser](/powershell/module/az.resources/get-azaduser)komutunu kullanabilirsiniz.

```azurepowershell
Get-AzADUser -StartsWith <userName>
(Get-AzADUser -DisplayName <userName>).id
```

**Grup**

Bir Azure AD grubu için Grup nesne KIMLIĞI gereklidir. Nesne KIMLIĞINI almak için [Get-AzADGroup](/powershell/module/az.resources/get-azadgroup)komutunu kullanabilirsiniz.

```azurepowershell
Get-AzADGroup -SearchString <groupName>
(Get-AzADGroup -DisplayName <groupName>).id
```

**Hizmet sorumlusu**

Bir Azure AD hizmet sorumlusu (bir uygulama tarafından kullanılan kimlik) için hizmet sorumlusu nesne KIMLIĞI gereklidir. Nesne KIMLIĞINI almak için [Get-AzADServicePrincipal](/powershell/module/az.resources/get-azadserviceprincipal)komutunu kullanabilirsiniz. Hizmet sorumlusu için uygulama KIMLIĞINI **değil** , nesne kimliğini kullanın.

```azurepowershell
Get-AzADServicePrincipal -SearchString <principalName>
(Get-AzADServicePrincipal -DisplayName <principalName>).id
```

**Yönetilen kimlik**

Sistem tarafından atanan veya Kullanıcı tarafından atanan bir yönetilen kimlik için, nesne KIMLIĞI gereklidir. Nesne KIMLIĞINI almak için [Get-AzADServicePrincipal](/powershell/module/az.resources/get-azadserviceprincipal)komutunu kullanabilirsiniz.

```azurepowershell
Get-AzADServicePrincipal -SearchString <principalName>
(Get-AzADServicePrincipal -DisplayName <principalName>).id
```
    
### <a name="step-2-find-the-appropriate-role"></a>2. Adım: uygun rolü bulun

İzinler, rollerle birlikte gruplandırılır. Çeşitli [Azure yerleşik rollerinin](built-in-roles.md) listesinden seçim yapabilir veya kendi özel rollerinizi kullanabilirsiniz. Gerekli en az ayrıcalığa sahip erişim vermek en iyi uygulamadır, bu nedenle daha geniş bir rol atamaktan kaçının.

Rolleri listelemek ve benzersiz rol KIMLIĞINI almak için [Get-AzRoleDefinition](/powershell/module/az.resources/get-azroledefinition)' ı kullanabilirsiniz.

```azurepowershell
Get-AzRoleDefinition | FT Name, IsCustom, Id
```

Belirli bir rolün ayrıntılarının listesi aşağıda verilmiştir.

```azurepowershell
Get-AzRoleDefinition <roleName>
```

Daha fazla bilgi için bkz. [Azure rol tanımlarını listeleme](role-definitions-list.md#azure-powershell).
 
### <a name="step-3-identify-the-needed-scope"></a>3. Adım: gerekli kapsamı tanımla

Azure dört kapsam düzeyi sağlar: kaynak, [kaynak grubu](../azure-resource-manager/management/overview.md#resource-groups), abonelik ve [Yönetim grubu](../governance/management-groups/overview.md). Gerekli en az ayrıcalıkla erişim sağlamak en iyi uygulamadır. bu nedenle, daha geniş bir kapsamda rol atamaktan kaçının. Kapsam hakkında daha fazla bilgi için bkz. [kapsamı anlama](scope-overview.md).

**Kaynak kapsamı**

Kaynak kapsamı için kaynağın kaynak KIMLIĞI gereklidir. Kaynak KIMLIĞINI Azure portal kaynağın özelliklerine bakarak bulabilirsiniz. Kaynak KIMLIĞI aşağıdaki biçimdedir.

```
/subscriptions/<subscriptionId>/resourcegroups/<resourceGroupName>/providers/<providerName>/<resourceType>/<resourceSubType>/<resourceName>
```

**Kaynak grubu kapsamı**

Kaynak grubu kapsamı için kaynak grubunun adına ihtiyacınız vardır. Azure portal adı **kaynak grupları** sayfasında bulabilir veya [Get-azresourcegroup](/powershell/module/az.resources/get-azresourcegroup)' i kullanabilirsiniz.

```azurepowershell
Get-AzResourceGroup
```

**Abonelik kapsamı** 

Abonelik kapsamı için abonelik KIMLIĞI gereklidir. KIMLIĞI Azure portal **abonelikler** sayfasında bulabilir veya [Get-azsubscription](/powershell/module/az.accounts/get-azsubscription)' i kullanabilirsiniz.

```azurepowershell
Get-AzSubscription
```

**Yönetim grubu kapsamı** 

Yönetim grubu kapsamı için yönetim grubu adına ihtiyacınız vardır. Adı Azure portal **Yönetim grupları** sayfasında bulabilir veya [Get-azmanagementgroup](/powershell/module/az.resources/get-azmanagementgroup)' i kullanabilirsiniz.

```azurepowershell
Get-AzManagementGroup
```
    
### <a name="step-4-add-role-assignment"></a>4. Adım: rol ataması ekleme

Rol ataması eklemek için [New-Azroleatama](/powershell/module/az.resources/new-azroleassignment) komutunu kullanın. Kapsama bağlı olarak, komut genellikle aşağıdaki biçimlerden birine sahiptir.

**Kaynak kapsamı**

```azurepowershell
New-AzRoleAssignment -ObjectId <objectId> `
-RoleDefinitionName <roleName> `
-Scope /subscriptions/<subscriptionId>/resourcegroups/<resourceGroupName>/providers/<providerName>/<resourceType>/<resourceSubType>/<resourceName>
```

```azurepowershell
New-AzRoleAssignment -ObjectId <objectId> `
-RoleDefinitionId <roleId> `
-ResourceName <resourceName> `
-ResourceType <resourceType> `
-ResourceGroupName <resourceGroupName>
```

**Kaynak grubu kapsamı**

```azurepowershell
New-AzRoleAssignment -SignInName <emailOrUserprincipalname> `
-RoleDefinitionName <roleName> `
-ResourceGroupName <resourceGroupName>
```

```azurepowershell
New-AzRoleAssignment -ObjectId <objectId> `
-RoleDefinitionName <roleName> `
-ResourceGroupName <resourceGroupName>
```

**Abonelik kapsamı** 

```azurepowershell
New-AzRoleAssignment -SignInName <emailOrUserprincipalname> `
-RoleDefinitionName <roleName> `
-Scope /subscriptions/<subscriptionId>
```

```azurepowershell
New-AzRoleAssignment -ObjectId <objectId> `
-RoleDefinitionName <roleName> `
-Scope /subscriptions/<subscriptionId>
```

**Yönetim grubu kapsamı** 

```azurepowershell
New-AzRoleAssignment -SignInName <emailOrUserprincipalname> `
-RoleDefinitionName <roleName> `
-Scope /providers/Microsoft.Management/managementGroups/<groupName>
``` 

```azurepowershell
New-AzRoleAssignment -ObjectId <objectId> `
-RoleDefinitionName <roleName> `
-Scope /providers/Microsoft.Management/managementGroups/<groupName>
``` 
    
## <a name="add-role-assignment-examples"></a>Rol atama örnekleri ekleme

#### <a name="add-role-assignment-for-all-blob-containers-in-a-storage-account-resource-scope"></a>Bir depolama hesabı kaynak kapsamındaki tüm blob kapsayıcıları için rol ataması ekleme

*Storage12345* adlı bir depolama hesabının kaynak kapsamındaki *55555555-5555-5555-5555-555555555555* nesne kimliğine sahip bir hizmet sorumlusuna [Depolama Blobu veri katılımcısı](built-in-roles.md#storage-blob-data-contributor) rolünü atar.

```azurepowershell
PS C:\> New-AzRoleAssignment -ObjectId 55555555-5555-5555-5555-555555555555 `
-RoleDefinitionName "Storage Blob Data Contributor" `
-Scope "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/Example-Storage-rg/providers/Microsoft.Storage/storageAccounts/storage12345"

RoleAssignmentId   : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/Example-Storage-rg/providers/Microsoft.Storage/storageAccounts/storage12345/providers/Microsoft.Authorization/roleAssignments/cccccccc-cccc-cccc-cccc-cccccccccccc
Scope              : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/Example-Storage-rg/providers/Microsoft.Storage/storageAccounts/storage12345
DisplayName        : example-identity
SignInName         :
RoleDefinitionName : Storage Blob Data Contributor
RoleDefinitionId   : ba92f5b4-2d11-453d-a403-e96b0029c9fe
ObjectId           : 55555555-5555-5555-5555-555555555555
ObjectType         : ServicePrincipal
CanDelegate        : False
```

#### <a name="add-role-assignment-for-a-specific-blob-container-resource-scope"></a>Belirli bir blob kapsayıcısı kaynak kapsamı için rol ataması Ekle

*BLOB-Container-01* adlı bir blob kapsayıcısının kaynak kapsamındaki *55555555-5555-5555-5555-555555555555* nesne kimliğine sahip bir hizmet sorumlusuna [Depolama Blobu veri katılımcısı](built-in-roles.md#storage-blob-data-contributor) rolünü atar.

```azurepowershell
PS C:\> New-AzRoleAssignment -ObjectId 55555555-5555-5555-5555-555555555555 `
-RoleDefinitionName "Storage Blob Data Contributor" `
-Scope "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/Example-Storage-rg/providers/Microsoft.Storage/storageAccounts/storage12345/blobServices/default/containers/blob-container-01"

RoleAssignmentId   : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/Example-Storage-rg/providers/Microsoft.Storage/storageAccounts/storage12345/blobServices/default/containers/blob-container-01/providers/Microsoft.Authorization/roleAssignm
                     ents/dddddddd-dddd-dddd-dddd-dddddddddddd
Scope              : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/Example-Storage-rg/providers/Microsoft.Storage/storageAccounts/storage12345/blobServices/default/containers/blob-container-01
DisplayName        : example-identity
SignInName         :
RoleDefinitionName : Storage Blob Data Contributor
RoleDefinitionId   : ba92f5b4-2d11-453d-a403-e96b0029c9fe
ObjectId           : 55555555-5555-5555-5555-555555555555
ObjectType         : ServicePrincipal
CanDelegate        : False
```

#### <a name="add-role-assignment-for-a-group-in-a-specific-virtual-network-resource-scope"></a>Belirli bir sanal ağ kaynak kapsamındaki bir grup için rol ataması ekleme

[Sanal makine katılımcısı](built-in-roles.md#virtual-machine-contributor) rolünü, Ilaç *Sales ADMINS* grubuna aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa ID with the *ilaç-Sales-Project-Network* adlı bir sanal ağın kaynak kapsamına atar.

```azurepowershell
PS C:\> New-AzRoleAssignment -ObjectId aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa `
-RoleDefinitionName "Virtual Machine Contributor" `
-ResourceName pharma-sales-project-network `
-ResourceType Microsoft.Network/virtualNetworks `
-ResourceGroupName MyVirtualNetworkResourceGroup

RoleAssignmentId   : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/MyVirtualNetworkResourceGroup
                     /providers/Microsoft.Network/virtualNetworks/pharma-sales-project-network/providers/Microsoft.Authorizat
                     ion/roleAssignments/bbbbbbbb-bbbb-bbbb-bbbb-bbbbbbbbbbbb
Scope              : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/MyVirtualNetworkResourceGroup
                     /providers/Microsoft.Network/virtualNetworks/pharma-sales-project-network
DisplayName        : Pharma Sales Admins
SignInName         :
RoleDefinitionName : Virtual Machine Contributor
RoleDefinitionId   : 9980e02c-c2be-4d73-94e8-173b1dc7cf3c
ObjectId           : aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa
ObjectType         : Group
CanDelegate        : False
```

#### <a name="add-a-role-assignment-for-a-user-at-a-resource-group-scope"></a>Bir kaynak grubu kapsamındaki bir kullanıcı için rol ataması ekleme

[Sanal makine katılımcısı](built-in-roles.md#virtual-machine-contributor) rolünü *ilaç-Sales* kaynak grubu kapsamındaki *patlong \@ contoso.com* kullanıcısına atar.

```azurepowershell
PS C:\> New-AzRoleAssignment -SignInName patlong@contoso.com `
-RoleDefinitionName "Virtual Machine Contributor" `
-ResourceGroupName pharma-sales

RoleAssignmentId   : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/pharma-sales/pr
                     oviders/Microsoft.Authorization/roleAssignments/55555555-5555-5555-5555-555555555555
Scope              : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/pharma-sales
DisplayName        : Pat Long
SignInName         : patlong@contoso.com
RoleDefinitionName : Virtual Machine Contributor
RoleDefinitionId   : 9980e02c-c2be-4d73-94e8-173b1dc7cf3c
ObjectId           : 44444444-4444-4444-4444-444444444444
ObjectType         : User
CanDelegate        : False
```

Alternatif olarak, parametresiyle tam kaynak grubunu belirtebilirsiniz `-Scope` :

```azurepowershell
PS C:\> New-AzRoleAssignment -SignInName patlong@contoso.com `
-RoleDefinitionName "Virtual Machine Contributor" `
-Scope "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/pharma-sales"

RoleAssignmentId   : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/pharma-sales/providers/Microsoft.Authorization/roleAssignments/55555555-5555-5555-5555-555555555555
Scope              : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/pharma-sales
DisplayName        : Pat Long
SignInName         : patlong@contoso.com
RoleDefinitionName : Virtual Machine Contributor
RoleDefinitionId   : 9980e02c-c2be-4d73-94e8-173b1dc7cf3c
ObjectId           : 44444444-4444-4444-4444-444444444444
ObjectType         : User
CanDelegate        : False
```

#### <a name="add-role-assignment-for-a-user-using-the-unique-role-id-at-a-resource-group-scope"></a>Bir kaynak grubu kapsamındaki benzersiz rol KIMLIĞINI kullanarak bir kullanıcı için rol ataması ekleme

Rol adının değişebilir birkaç zaman vardır, örneğin:

- Kendi özel rolünüzü kullanıyorsunuz ve adı değiştirmeye karar verdiniz.
- Adında **(Önizleme)** olan bir önizleme rolü kullanıyorsunuz. Rol serbest bırakıldığında, rol yeniden adlandırılır.

Rol yeniden adlandırılsa bile, rol KIMLIĞI değişmez. Rol atamalarınızı oluşturmak için betikler veya Otomasyon kullanıyorsanız, rol adı yerine benzersiz rol KIMLIĞINI kullanmak en iyi uygulamadır. Bu nedenle, bir rol yeniden adlandırılırsa, betiklerinizin çalışması daha olasıdır.

Aşağıdaki örnek, [sanal makine katılımcısı](built-in-roles.md#virtual-machine-contributor) rolünü *ilaç-Sales* kaynak grubu kapsamındaki *patlong \@ contoso.com* kullanıcısına atar.

```azurepowershell
PS C:\> New-AzRoleAssignment -ObjectId 44444444-4444-4444-4444-444444444444 `
-RoleDefinitionId 9980e02c-c2be-4d73-94e8-173b1dc7cf3c `
-Scope "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/pharma-sales"

RoleAssignmentId   : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/pharma-sales/providers/Microsoft.Authorization/roleAssignments/55555555-5555-5555-5555-555555555555
Scope              : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/pharma-sales
DisplayName        : Pat Long
SignInName         : patlong@contoso.com
RoleDefinitionName : Virtual Machine Contributor
RoleDefinitionId   : 9980e02c-c2be-4d73-94e8-173b1dc7cf3c
ObjectId           : 44444444-4444-4444-4444-444444444444
ObjectType         : User
CanDelegate        : False
```

#### <a name="add-role-assignment-for-an-application-at-a-resource-group-scope"></a>Kaynak grubu kapsamındaki bir uygulama için rol ataması ekleme

[Sanal makine katılımcısı](built-in-roles.md#virtual-machine-contributor) rolünü, *ilaç-Sales* kaynak grubu KAPSAMıNDAKI hizmet sorumlusu nesne kimliği 77777777-7777-7777-7777-777777777777 olan bir uygulamaya atar.

```azurepowershell
PS C:\> New-AzRoleAssignment -ObjectId 77777777-7777-7777-7777-777777777777 `
-RoleDefinitionName "Virtual Machine Contributor" `
-ResourceGroupName pharma-sales

RoleAssignmentId   : /subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Authorization/roleAssignments/66666666-6666-6666-6666-666666666666
Scope              : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/pharma-sales
DisplayName        : MyApp1
SignInName         :
RoleDefinitionName : Virtual Machine Contributor
RoleDefinitionId   : 9980e02c-c2be-4d73-94e8-173b1dc7cf3c
ObjectId           : 77777777-7777-7777-7777-777777777777
ObjectType         : ServicePrincipal
CanDelegate        : False
```

#### <a name="add-role-assignment-for-a-user-at-a-subscription-scope"></a>Abonelik kapsamındaki bir kullanıcı için rol ataması ekleme

Bir abonelik kapsamındaki *annm \@ example.com* kullanıcısına [okuyucu](built-in-roles.md#reader) rolünü atar.

```azurepowershell
PS C:\> New-AzRoleAssignment -SignInName annm@example.com `
-RoleDefinitionName "Reader" `
-Scope "/subscriptions/00000000-0000-0000-0000-000000000000"

RoleAssignmentId   : /subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Authorization/roleAssignments/66666666-6666-6666-6666-666666666666
Scope              : /subscriptions/00000000-0000-0000-0000-000000000000
DisplayName        : Ann M
SignInName         : annm@example.com
RoleDefinitionName : Reader
RoleDefinitionId   : acdd72a7-3385-48ef-bd42-f606fba81ae7
ObjectId           : 77777777-7777-7777-7777-777777777777
ObjectType         : ServicePrincipal
CanDelegate        : False
```

#### <a name="add-role-assignment-for-a-user-at-a-management-group-scope"></a>Yönetim grubu kapsamındaki bir kullanıcı için rol ataması ekleme

Bir yönetim grubu kapsamındaki *Alain \@ example.com* kullanıcısına [faturalandırma okuyucusu](built-in-roles.md#billing-reader) rolünü atar.

```azurepowershell
PS C:\> New-AzRoleAssignment -SignInName alain@example.com `
-RoleDefinitionName "Billing Reader" `
-Scope "/providers/Microsoft.Management/managementGroups/marketing-group"

RoleAssignmentId   : /providers/Microsoft.Management/managementGroups/marketing-group/providers/Microsoft.Authorization/roleAssignments/22222222-2222-2222-2222-222222222222
Scope              : /providers/Microsoft.Management/managementGroups/marketing-group
DisplayName        : Alain Charon
SignInName         : alain@example.com
RoleDefinitionName : Billing Reader
RoleDefinitionId   : fa23ad8b-c56e-40d8-ac0c-ce449e1d2c64
ObjectId           : 44444444-4444-4444-4444-444444444444
ObjectType         : User
CanDelegate        : False
```

## <a name="remove-a-role-assignment"></a>Rol atamasını kaldırma

Azure RBAC 'de, erişimi kaldırmak için [Remove-Azroleatama](/powershell/module/az.resources/remove-azroleassignment)kullanarak bir rol atamasını kaldırırsınız.

Aşağıdaki örnek, *ilaç-Sales* kaynak grubundaki *patlong \@ contoso.com* kullanıcısının [sanal makine katılımcısı](built-in-roles.md#virtual-machine-contributor) rolü atamasını kaldırır:

```azurepowershell
PS C:\> Remove-AzRoleAssignment -SignInName patlong@contoso.com `
-RoleDefinitionName "Virtual Machine Contributor" `
-ResourceGroupName pharma-sales
```

Bir abonelik kapsamındaki KIMLIĞI 22222222-2222-2222-2222-222222222222 olan *Ann Mack ekip* grubundan [okuyucu](built-in-roles.md#reader) rolünü kaldırır.

```azurepowershell
PS C:\> Remove-AzRoleAssignment -ObjectId 22222222-2222-2222-2222-222222222222 `
-RoleDefinitionName "Reader" `
-Scope "/subscriptions/00000000-0000-0000-0000-000000000000"
```

Yönetim grubu kapsamındaki *Alain \@ example.com* kullanıcısının [faturalandırma okuyucusu](built-in-roles.md#billing-reader) rolünü kaldırır.

```azurepowershell
PS C:\> Remove-AzRoleAssignment -SignInName alain@example.com `
-RoleDefinitionName "Billing Reader" `
-Scope "/providers/Microsoft.Management/managementGroups/marketing-group"
```

"Belirtilen bilgiler bir rol atamasıyla eşlenmiyor" hata iletisini alırsanız, veya parametrelerini de belirttiğinizden emin olun `-Scope` `-ResourceGroupName` . Daha fazla bilgi için bkz. [Azure RBAC sorunlarını giderme](troubleshooting.md#role-assignments-with-identity-not-found).

## <a name="next-steps"></a>Sonraki adımlar

- [Azure PowerShell kullanarak Azure rol atamalarını listeleme](role-assignments-list-powershell.md)
- [Öğretici: Azure PowerShell kullanarak Azure kaynaklarına grup erişimi verme](tutorial-role-assignments-group-powershell.md)
- [Azure PowerShell ile kaynakları yönetme](../azure-resource-manager/management/manage-resources-powershell.md)
