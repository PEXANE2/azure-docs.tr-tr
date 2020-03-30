---
title: Azure PowerShell ile Azure kaynakları için atamaları reddetme listesi
description: Azure PowerShell'i kullanarak belirli kapsamlarda belirli Azure kaynak eylemlerine erişimi engellenen kullanıcıları, grupları, hizmet ilkelerini ve yönetilen kimlikleri nasıl listeleyiş süreceğinizi öğrenin.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/12/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 5ba18b89bd37dbd55350321c503e37ab0590ab87
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77137388"
---
# <a name="list-deny-assignments-for-azure-resources-using-azure-powershell"></a>Azure PowerShell kullanarak Azure kaynakları için atamaları reddetme listesi

[Atamaları reddetme,](deny-assignments.md) bir rol ataması onlara erişim izni verse bile kullanıcıların belirli Azure kaynak eylemlerini gerçekleştirmesini engeller. Bu makalede, Azure PowerShell kullanarak reddi atamalarınasıl listeleneniz açıklanmaktadır.

> [!NOTE]
> Doğrudan kendi reddi atamaları oluşturamaz. Reddet atamalarının nasıl oluşturulduğu hakkında bilgi için [bkz.](deny-assignments.md)

## <a name="prerequisites"></a>Ön koşullar

Reddet ataması hakkında bilgi almak için şunları yapmış olmalısınız:

- `Microsoft.Authorization/denyAssignments/read`Azure kaynakları için en [yerleşik rollere](built-in-roles.md) dahil olan izin
- Azure Bulut Shell veya [Azure PowerShell'de](/azure/cloud-shell/overview) [PowerShell](/powershell/azure/install-az-ps)

## <a name="list-deny-assignments"></a>Ret atamalarını listeleme

### <a name="list-all-deny-assignments"></a>Tüm reddet atamalarını listele

Geçerli abonelik için tüm atamaları reddetmeyi listelemek için [Get-AzDenyAssignment'ı](/powershell/module/az.resources/get-azdenyassignment)kullanın.

```azurepowershell
Get-AzDenyAssignment
```

```Example
PS C:\> Get-AzDenyAssignment

Id                      : 22222222-2222-2222-2222-222222222222
DenyAssignmentName      : Deny assignment '22222222-2222-2222-2222-222222222222' created by Blueprint Assignment
                          '/subscriptions/11111111-1111-1111-1111-111111111111/providers/Microsoft.Blueprint/blueprintAssignments/assignment-locked-storageaccount-TestingBPLocks'.
Description             : Created by Blueprint Assignment '/subscriptions/11111111-1111-1111-1111-111111111111/providers/Microsoft.Blueprint/blueprintAssignments/assignment-locked-storageaccount-TestingBPLocks'.
Actions                 : {*}
NotActions              : {*/read}
DataActions             : {}
NotDataActions          : {}
Scope                   : /subscriptions/11111111-1111-1111-1111-111111111111/resourceGroups/TestingBPLocks
DoNotApplyToChildScopes : True
Principals              : {
                          DisplayName:  All Principals
                          ObjectType:   SystemDefined
                          ObjectId:     00000000-0000-0000-0000-000000000000
                          }
ExcludePrincipals       : {
                          ObjectType:   ServicePrincipal
                          }
IsSystemProtected       : True

Id                      : 33333333-3333-3333-3333-333333333333
DenyAssignmentName      : Deny assignment '33333333-3333-3333-3333-333333333333' created by Blueprint Assignment
                          '/subscriptions/11111111-1111-1111-1111-111111111111/providers/Microsoft.Blueprint/blueprintAssignments/assignment-locked-storageaccount-TestingBPLocks'.
Description             : Created by Blueprint Assignment '/subscriptions/11111111-1111-1111-1111-111111111111/providers/Microsoft.Blueprint/blueprintAssignments/assignment-locked-storageaccount-TestingBPLocks'.
Actions                 : {*}
NotActions              : {*/read}
DataActions             : {}
NotDataActions          : {}
Scope                   : /subscriptions/11111111-1111-1111-1111-111111111111/resourceGroups/TestingBPLocks/providers/Microsoft.Storage/storageAccounts/storep6vkuxmu4m4pq
DoNotApplyToChildScopes : True
Principals              : {
                          DisplayName:  All Principals
                          ObjectType:   SystemDefined
                          ObjectId:     00000000-0000-0000-0000-000000000000
                          }
ExcludePrincipals       : {
                          DisplayName:  assignment-locked-storageaccount-TestingBPLocks
                          ObjectType:   ServicePrincipal
                          ObjectId:     2311a0b7-657a-4ca2-af6f-d1c33f6d2fff
                          }
IsSystemProtected       : True
```

### <a name="list-deny-assignments-at-a-resource-group-scope"></a>Kaynak grubu kapsamındaatamaları reddetme listesi

Kaynak grubu kapsamındaki tüm atamaları reddetmek için [Get-AzDenyAssignment'ı](/powershell/module/az.resources/get-azdenyassignment)kullanın.

```azurepowershell
Get-AzDenyAssignment -ResourceGroupName <resource_group_name>
```

```Example
PS C:\> Get-AzDenyAssignment -ResourceGroupName TestingBPLocks | FL DenyAssignmentName, Scope

DenyAssignmentName : Deny assignment '22222222-2222-2222-2222-222222222222' created by Blueprint Assignment
                     '/subscriptions/11111111-1111-1111-1111-111111111111/providers/Microsoft.Blueprint/blueprintAssignments/assignment-locked-storageaccount-TestingBPLocks'.
Scope              : /subscriptions/11111111-1111-1111-1111-111111111111/resourceGroups/TestingBPLocks
Principals         : {
                     DisplayName:       All Principals
                     ObjectType:        SystemDefined
                     ObjectId:  00000000-0000-0000-0000-000000000000
                     }
```

### <a name="list-deny-assignments-at-a-subscription-scope"></a>Abonelik kapsamındaatamaları reddetme listesi

Abonelik kapsamındaki tüm atamaları reddetmek için [Get-AzDenyAssignment'ı](/powershell/module/az.resources/get-azdenyassignment)kullanın. Abonelik kimliğini almak için Azure portalındaki **Abonelikler** bıyığa veya [Get-AzSubscription'ı](/powershell/module/Az.Accounts/Get-AzSubscription)kullanabilirsiniz.

```azurepowershell
Get-AzDenyAssignment -Scope /subscriptions/<subscription_id>
```

```Example
PS C:\> Get-AzDenyAssignment -Scope /subscriptions/11111111-1111-1111-1111-111111111111
```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure kaynakları için atamaları reddetmeyi anlama](deny-assignments.md)
- [Azure portalını kullanarak Azure kaynakları için atamaları reddetme listesi](deny-assignments-portal.md)
- [REST API'sini kullanarak Azure kaynakları için atamaları reddetme listesi](deny-assignments-rest.md)