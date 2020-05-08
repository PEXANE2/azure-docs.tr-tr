---
title: Azure PowerShell-Azure RBAC kullanarak Azure rol atamaları ekleme veya kaldırma
description: Azure PowerShell ve Azure rol tabanlı erişim denetimi (Azure RBAC) kullanarak kullanıcılar, gruplar, hizmet sorumluları veya yönetilen kimlikler için Azure kaynaklarına erişim izni verme hakkında bilgi edinin.
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
ms.date: 11/25/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: db6b38f142254fa1812f34674e6a870629713d7e
ms.sourcegitcommit: 4499035f03e7a8fb40f5cff616eb01753b986278
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/03/2020
ms.locfileid: "82735666"
---
# <a name="add-or-remove-azure-role-assignments-using-azure-powershell"></a>Azure PowerShell kullanarak Azure rol atamaları ekleme veya kaldırma

[!INCLUDE [Azure RBAC definition grant access](../../includes/role-based-access-control-definition-grant.md)]Bu makalede, Azure PowerShell kullanarak rollerin nasıl atanacağı açıklanır.

[!INCLUDE [az-powershell-update](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Ön koşullar

Rol atamaları eklemek veya kaldırmak için şunları yapmanız gerekir:

- `Microsoft.Authorization/roleAssignments/write`ve `Microsoft.Authorization/roleAssignments/delete` [Kullanıcı erişimi Yöneticisi](built-in-roles.md#user-access-administrator) veya [sahibi](built-in-roles.md#owner) gibi izinler
- [Azure Cloud Shell](/azure/cloud-shell/overview) veya [Azure PowerShell](/powershell/azure/install-az-ps) PowerShell

## <a name="get-object-ids"></a>Nesne kimliklerini al

Rol atamaları eklemek veya kaldırmak için, bir nesnenin benzersiz KIMLIĞINI belirtmeniz gerekebilir. KIMLIK şu biçimdedir: `11111111-1111-1111-1111-111111111111`. KIMLIĞI Azure portal veya Azure PowerShell kullanarak alabilirsiniz.

### <a name="user"></a>Kullanıcı

Bir Azure AD kullanıcısının nesne KIMLIĞINI almak için [Get-AzADUser](/powershell/module/az.resources/get-azaduser)öğesini kullanabilirsiniz.

```azurepowershell
Get-AzADUser -StartsWith <string_in_quotes>
(Get-AzADUser -DisplayName <name_in_quotes>).id
```

### <a name="group"></a>Grup

Bir Azure AD grubunun nesne KIMLIĞINI almak için [Get-AzADGroup](/powershell/module/az.resources/get-azadgroup)' u kullanabilirsiniz.

```azurepowershell
Get-AzADGroup -SearchString <group_name_in_quotes>
(Get-AzADGroup -DisplayName <group_name_in_quotes>).id
```

### <a name="application"></a>Uygulama

Bir Azure AD hizmet sorumlusunun (bir uygulama tarafından kullanılan kimlik) nesne KIMLIĞINI almak için [Get-AzADServicePrincipal](/powershell/module/az.resources/get-azadserviceprincipal)kullanabilirsiniz. Hizmet sorumlusu için uygulama KIMLIĞINI **değil** , nesne kimliğini kullanın.

```azurepowershell
Get-AzADServicePrincipal -SearchString <service_name_in_quotes>
(Get-AzADServicePrincipal -DisplayName <service_name_in_quotes>).id
```

## <a name="add-a-role-assignment"></a>Rol ataması ekleyin

Azure RBAC 'de, erişim izni vermek için bir rol ataması eklersiniz.

### <a name="user-at-a-resource-group-scope"></a>Kaynak grubu kapsamındaki Kullanıcı

Bir kaynak grubu kapsamındaki bir kullanıcı için rol ataması eklemek üzere [New-Azroleatama](/powershell/module/az.resources/new-azroleassignment)kullanın.

```azurepowershell
New-AzRoleAssignment -SignInName <email_or_userprincipalname> -RoleDefinitionName <role_name> -ResourceGroupName <resource_group_name>
```

```Example
PS C:\> New-AzRoleAssignment -SignInName alain@example.com -RoleDefinitionName "Virtual Machine Contributor" -ResourceGroupName pharma-sales


RoleAssignmentId   : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/pharma-sales/pr
                     oviders/Microsoft.Authorization/roleAssignments/55555555-5555-5555-5555-555555555555
Scope              : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/pharma-sales
DisplayName        : Alain Charon
SignInName         : alain@example.com
RoleDefinitionName : Virtual Machine Contributor
RoleDefinitionId   : 9980e02c-c2be-4d73-94e8-173b1dc7cf3c
ObjectId           : 44444444-4444-4444-4444-444444444444
ObjectType         : User
CanDelegate        : False
```

### <a name="using-the-unique-role-id"></a>Benzersiz rol KIMLIĞINI kullanma

Rol adının değişebilir birkaç zaman vardır, örneğin:

- Kendi özel rolünüzü kullanıyorsunuz ve adı değiştirmeye karar verdiniz.
- Adında **(Önizleme)** olan bir önizleme rolü kullanıyorsunuz. Rol serbest bırakıldığında, rol yeniden adlandırılır.

> [!IMPORTANT]
> Bir önizleme sürümü, bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Rol yeniden adlandırılsa bile, rol KIMLIĞI değişmez. Rol atamalarınızı oluşturmak için betikler veya Otomasyon kullanıyorsanız, rol adı yerine benzersiz rol KIMLIĞINI kullanmak en iyi uygulamadır. Bu nedenle, bir rol yeniden adlandırılırsa, betiklerinizin çalışması daha olasıdır.

Rol adı yerine benzersiz rol KIMLIĞI kullanarak bir rol ataması eklemek için, [New-Azroleatama](/powershell/module/az.resources/new-azroleassignment)kullanın.

```azurepowershell
New-AzRoleAssignment -ObjectId <object_id> -RoleDefinitionId <role_id> -ResourceGroupName <resource_group_name>
```

Aşağıdaki örnek, [sanal makine katılımcısı](built-in-roles.md#virtual-machine-contributor) rolünü ilaç *\@example.com* kullanıcısına *ilaç-Sales* kaynak grubu kapsamında atar. Benzersiz rol KIMLIĞINI almak için [Get-AzRoleDefinition](/powershell/module/az.resources/get-azroledefinition) ' ı kullanabilir veya [Azure yerleşik rolleri](built-in-roles.md)' ne bakabilirsiniz.

```Example
PS C:\> New-AzRoleAssignment -ObjectId 44444444-4444-4444-4444-444444444444 -RoleDefinitionId 9980e02c-c2be-4d73-94e8-173b1dc7cf3c -Scope /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/pharma-sales

RoleAssignmentId   : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/pharma-sales/providers/Microsoft.Authorization/roleAssignments/55555555-5555-5555-5555-555555555555
Scope              : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/pharma-sales
DisplayName        : Alain Charon
SignInName         : alain@example.com
RoleDefinitionName : Virtual Machine Contributor
RoleDefinitionId   : 9980e02c-c2be-4d73-94e8-173b1dc7cf3c
ObjectId           : 44444444-4444-4444-4444-444444444444
ObjectType         : User
CanDelegate        : False
```

### <a name="group-at-a-resource-scope"></a>Kaynak kapsamındaki Grup

Kaynak kapsamındaki bir gruba rol ataması eklemek için [New-Azroleatama](/powershell/module/az.resources/new-azroleassignment)kullanın. Grubun nesne KIMLIĞINI alma hakkında daha fazla bilgi için bkz. [nesne kimliklerini alma](#get-object-ids).

```azurepowershell
New-AzRoleAssignment -ObjectId <object_id> -RoleDefinitionName <role_name> -ResourceName <resource_name> -ResourceType <resource_type> -ParentResource <parent resource> -ResourceGroupName <resource_group_name>
```

```Example
PS C:\> Get-AzADGroup -SearchString "Pharma"

SecurityEnabled DisplayName         Id                                   Type
--------------- -----------         --                                   ----
           True Pharma Sales Admins aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa Group

PS C:\> New-AzRoleAssignment -ObjectId aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa -RoleDefinitionName "Virtual Machine Contributor" -ResourceName RobertVirtualNetwork -ResourceType Microsoft.Network/virtualNetworks -ResourceGroupName RobertVirtualNetworkResourceGroup

RoleAssignmentId   : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/MyVirtualNetworkResourceGroup
                     /providers/Microsoft.Network/virtualNetworks/RobertVirtualNetwork/providers/Microsoft.Authorizat
                     ion/roleAssignments/bbbbbbbb-bbbb-bbbb-bbbb-bbbbbbbbbbbb
Scope              : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/MyVirtualNetworkResourceGroup
                     /providers/Microsoft.Network/virtualNetworks/RobertVirtualNetwork
DisplayName        : Pharma Sales Admins
SignInName         :
RoleDefinitionName : Virtual Machine Contributor
RoleDefinitionId   : 9980e02c-c2be-4d73-94e8-173b1dc7cf3c
ObjectId           : aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa
ObjectType         : Group
CanDelegate        : False
```

### <a name="application-at-a-subscription-scope"></a>Bir abonelik kapsamındaki uygulama

Bir abonelik kapsamındaki bir uygulama için rol ataması eklemek üzere [New-Azroleatama](/powershell/module/az.resources/new-azroleassignment)kullanın. Uygulamanın nesne KIMLIĞINI alma hakkında daha fazla bilgi için bkz. [nesne kimliklerini alma](#get-object-ids).

```azurepowershell
New-AzRoleAssignment -ObjectId <object_id> -RoleDefinitionName <role_name> -Scope /subscriptions/<subscription_id>
```

```Example
PS C:\> New-AzRoleAssignment -ObjectId 77777777-7777-7777-7777-777777777777 -RoleDefinitionName "Reader" -Scope /subscriptions/00000000-0000-0000-0000-000000000000

RoleAssignmentId   : /subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Authorization/roleAssignments/66666666-6666-6666-6666-666666666666
Scope              : /subscriptions/00000000-0000-0000-0000-000000000000
DisplayName        : MyApp1
SignInName         :
RoleDefinitionName : Reader
RoleDefinitionId   : acdd72a7-3385-48ef-bd42-f606fba81ae7
ObjectId           : 77777777-7777-7777-7777-777777777777
ObjectType         : ServicePrincipal
CanDelegate        : False
```

### <a name="user-at-a-management-group-scope"></a>Bir yönetim grubu kapsamındaki Kullanıcı

Bir yönetim grubu kapsamındaki bir kullanıcı için rol ataması eklemek üzere [New-Azroleatama](/powershell/module/az.resources/new-azroleassignment)kullanın. Yönetim grubu KIMLIĞINI almak için Azure portal **Yönetim grupları** dikey penceresinde bulabilir veya [Get-azmanagementgroup](/powershell/module/az.resources/get-azmanagementgroup)' yi kullanabilirsiniz.

```azurepowershell
New-AzRoleAssignment -SignInName <email_or_userprincipalname> -RoleDefinitionName <role_name> -Scope /providers/Microsoft.Management/managementGroups/<group_id>
```

```Example
PS C:\> New-AzRoleAssignment -SignInName alain@example.com -RoleDefinitionName "Billing Reader" -Scope /providers/Microsoft.Management/managementGroups/marketing-group

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

Aşağıdaki örnek, *ilaç-Sales* kaynak grubundaki *\@Alain example.com* kullanıcısının *sanal makine katılımcısı* rolü atamasını kaldırır:

```Example
PS C:\> Remove-AzRoleAssignment -SignInName alain@example.com -RoleDefinitionName "Virtual Machine Contributor" -ResourceGroupName pharma-sales
```

Aşağıdaki örnek, <role_name> rolünü bir abonelik kapsamındaki <object_id> kaldırır.

```azurepowershell
Remove-AzRoleAssignment -ObjectId <object_id> -RoleDefinitionName <role_name> -Scope /subscriptions/<subscription_id>
```

Aşağıdaki örnek <role_name> rolünü yönetim grubu kapsamındaki <object_id> kaldırır.

```azurepowershell
Remove-AzRoleAssignment -ObjectId <object_id> -RoleDefinitionName <role_name> -Scope /providers/Microsoft.Management/managementGroups/<group_id>
```

"Belirtilen bilgiler bir rol atamasıyla eşlenmiyor" hata iletisini alırsanız, `-Scope` veya `-ResourceGroupName` parametrelerini de belirttiğinizden emin olun. Daha fazla bilgi için bkz. [Azure RBAC sorunlarını giderme](troubleshooting.md#role-assignments-with-identity-not-found).

## <a name="next-steps"></a>Sonraki adımlar

- [Azure PowerShell kullanarak Azure rol atamalarını listeleme](role-assignments-list-powershell.md)
- [Öğretici: Azure PowerShell kullanarak Azure kaynaklarına grup erişimi verme](tutorial-role-assignments-group-powershell.md)
- [Öğretici: Azure PowerShell kullanarak bir Azure özel rolü oluşturma](tutorial-custom-role-powershell.md)
- [Azure PowerShell ile kaynakları yönetme](../azure-resource-manager/management/manage-resources-powershell.md)
