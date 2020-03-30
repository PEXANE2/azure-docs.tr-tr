---
title: RBAC ve Azure PowerShell ile rol atamaları ekleme veya kaldırma
description: Azure rol tabanlı erişim denetimi (RBAC) ve Azure PowerShell'i kullanarak kullanıcılar, gruplar, hizmet müdürleri veya yönetilen kimlikler için Azure kaynaklarına nasıl erişim tanıyın öğrenin.
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
ms.openlocfilehash: 68a73f622dc69b70870ddc1db16edcf406b63800
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79283218"
---
# <a name="add-or-remove-role-assignments-using-azure-rbac-and-azure-powershell"></a>Azure RBAC ve Azure PowerShell'i kullanarak rol atamaları ekleme veya kaldırma

[!INCLUDE [Azure RBAC definition grant access](../../includes/role-based-access-control-definition-grant.md)]Bu makalede, Azure PowerShell kullanarak rollerin nasıl atanılabildiğini açıklanmaktadır.

[!INCLUDE [az-powershell-update](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Ön koşullar

Rol atamaları eklemek veya kaldırmak için şunları yapmış olmalısınız:

- `Microsoft.Authorization/roleAssignments/write`ve `Microsoft.Authorization/roleAssignments/delete` Kullanıcı Erişim [Yöneticisi](built-in-roles.md#user-access-administrator) veya [Sahibi](built-in-roles.md#owner) gibi izinler
- Azure Bulut Shell veya [Azure PowerShell'de](/azure/cloud-shell/overview) [PowerShell](/powershell/azure/install-az-ps)

## <a name="get-object-ids"></a>Nesne tünelerini alma

Rol atamaları eklemek veya kaldırmak için nesnenin benzersiz kimliğini belirtmeniz gerekebilir. Kimlik biçimi vardır: `11111111-1111-1111-1111-111111111111`. Kimliği Azure portalını veya Azure PowerShell'i kullanarak alabilirsiniz.

### <a name="user"></a>Kullanıcı

Bir Azure AD kullanıcısının nesne kimliğini almak için [Get-AzADUser'ı](/powershell/module/az.resources/get-azaduser)kullanabilirsiniz.

```azurepowershell
Get-AzADUser -StartsWith <string_in_quotes>
(Get-AzADUser -DisplayName <name_in_quotes>).id
```

### <a name="group"></a>Grup

Bir Azure REKLAM grubunun nesne kimliğini almak için [Get-AzADGroup'u](/powershell/module/az.resources/get-azadgroup)kullanabilirsiniz.

```azurepowershell
Get-AzADGroup -SearchString <group_name_in_quotes>
(Get-AzADGroup -DisplayName <group_name_in_quotes>).id
```

### <a name="application"></a>Uygulama

Azure AD hizmet sorumlusunun nesne kimliğini almak için (bir uygulama tarafından kullanılan kimlik), [Get-AzADServicePrincipal'ı](/powershell/module/az.resources/get-azadserviceprincipal)kullanabilirsiniz. Bir hizmet sorumlusu için, uygulama kimliğini **değil,** nesne kimliğini kullanın.

```azurepowershell
Get-AzADServicePrincipal -SearchString <service_name_in_quotes>
(Get-AzADServicePrincipal -DisplayName <service_name_in_quotes>).id
```

## <a name="add-a-role-assignment"></a>Rol ataması ekleyin

RBAC'da, erişim sağlamak için bir rol ataması eklersiniz.

### <a name="user-at-a-resource-group-scope"></a>Kaynak grubu kapsamındaki kullanıcı

Kaynak grubu kapsamındaki bir kullanıcı için rol ataması eklemek için [Yeni-AzRoleAssignment'ı](/powershell/module/az.resources/new-azroleassignment)kullanın.

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

### <a name="using-the-unique-role-id"></a>Benzersiz rol kimliğini kullanma

Bir rol adının değişebileceği birkaç kez vardır, örneğin:

- Kendi özel rolünüzü kullanıyorsunuz ve adı değiştirmeye karar vermissiniz.
- Adda **(Önizleme)** bulunan bir önizleme rolü kullanıyorsunuz. Rol yayımlandığında, rol yeniden adlandırılır.

> [!IMPORTANT]
> Önizleme sürümü hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.

Bir rol yeniden adlandırılsa bile, rol kimliği değişmez. Rol atamalarınızı oluşturmak için komut dosyalarını veya otomasyonu kullanıyorsanız, rol adı yerine benzersiz rol kimliğini kullanmak en iyi yöntemdir. Bu nedenle, bir rol yeniden adlandırıldıysa, komut dosyalarınızın çalışma olasılığı daha yüksektir.

Rol adı yerine benzersiz rol kimliği kullanarak bir rol ataması eklemek için [Yeni-AzRoleAssignment'ı](/powershell/module/az.resources/new-azroleassignment)kullanın.

```azurepowershell
New-AzRoleAssignment -ObjectId <object_id> -RoleDefinitionId <role_id> -ResourceGroupName <resource_group_name>
```

Aşağıdaki örnek, [Sanal Makine Katılımcısı](built-in-roles.md#virtual-machine-contributor) *rolünü, ilaç satış* kaynak grubu kapsamındaki *\@example.com* kullanıcıya atar. Benzersiz rol kimliği almak için [Get-AzRoleDefinition'ı](/powershell/module/az.resources/get-azroledefinition) kullanabilir veya [Azure kaynakları için Yerleşik rolleri](built-in-roles.md)görebilirsiniz.

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

### <a name="group-at-a-resource-scope"></a>Kaynak kapsamında gruplandırma

Kaynak kapsamındaki bir grup için rol ataması eklemek için [Yeni-AzRoleAssignment'ı](/powershell/module/az.resources/new-azroleassignment)kullanın. Grubun nesne kimliğini nasıl alacağıhakkında bilgi için [bkz.](#get-object-ids)

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

### <a name="application-at-a-subscription-scope"></a>Abonelik kapsamında uygulama

Abonelik kapsamındaki bir uygulama için rol ataması eklemek için [Yeni-AzRoleAssignment'ı](/powershell/module/az.resources/new-azroleassignment)kullanın. Uygulamanın nesne kimliğini nasıl alacağı hakkında bilgi için [bkz.](#get-object-ids)

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

### <a name="user-at-a-management-group-scope"></a>Yönetim grubu kapsamında ki kullanıcı

Yönetim grubu kapsamındaki bir kullanıcı için rol ataması eklemek için [Yeni-AzRoleAssignment'ı](/powershell/module/az.resources/new-azroleassignment)kullanın. Yönetim grubu kimliğini almak için, Azure portalındaki **Yönetim grupları** bıçağında bulabilir veya [Get-AzManagementGroup'u](/powershell/module/az.resources/get-azmanagementgroup)kullanabilirsiniz.

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

RBAC'da, erişimi kaldırmak için [Remove-AzRoleAssignment'ı](/powershell/module/az.resources/remove-azroleassignment)kullanarak bir rol atamasını kaldırırsınız.

Aşağıdaki örnek, *Sanal Makine Katılımcısı* rol atamasını *ilaç satış* kaynak grubundaki *\@alain example.com* kullanıcısından kaldırır:

```Example
PS C:\> Remove-AzRoleAssignment -SignInName alain@example.com -RoleDefinitionName "Virtual Machine Contributor" -ResourceGroupName pharma-sales
```

Aşağıdaki örnek, <role_name> rolü abonelik kapsamındaki <object_id> kaldırır.

```azurepowershell
Remove-AzRoleAssignment -ObjectId <object_id> -RoleDefinitionName <role_name> -Scope /subscriptions/<subscription_id>
```

Aşağıdaki örnek, yönetim grubu kapsamında ki <role_name> rolü <object_id> kaldırır.

```azurepowershell
Remove-AzRoleAssignment -ObjectId <object_id> -RoleDefinitionName <role_name> -Scope /providers/Microsoft.Management/managementGroups/<group_id>
```

Hata iletisi alırsanız: "Sağlanan bilgiler bir rol atamasının haritasını çıkarmaz", ayrıca bu veya `-Scope` `-ResourceGroupName` parametreleri belirttiğinden emin olun. Daha fazla bilgi için [Azure kaynakları için Sorun Giderme RBAC'a](troubleshooting.md#role-assignments-with-unknown-security-principal)bakın.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure RBAC ve Azure PowerShell'i kullanarak rol atamalarını listele](role-assignments-list-powershell.md)
- [Öğretici: RBAC ve Azure PowerShell'i kullanarak Azure kaynaklarına grup erişimi verme](tutorial-role-assignments-group-powershell.md)
- [Öğretici: Azure PowerShell'i kullanarak Azure kaynakları için özel bir rol oluşturma](tutorial-custom-role-powershell.md)
- [Azure PowerShell ile kaynakları yönetme](../azure-resource-manager/management/manage-resources-powershell.md)
