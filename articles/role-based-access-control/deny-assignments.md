---
title: Azure kaynakları için atamaları reddetmeyi anlama
description: Azure kaynakları için rol tabanlı erişim denetiminde (RBAC) atamaları reddetme hakkında bilgi edinin.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: ''
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/26/2020
ms.author: rolyon
ms.reviewer: bagovind
ms.custom: ''
ms.openlocfilehash: db249ccde1026cd468a1c30942891119482697ba
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80372480"
---
# <a name="understand-deny-assignments-for-azure-resources"></a>Azure kaynakları için atamaları reddetmeyi anlama

Bir rol atamasına benzer şekilde, *reddet ataması,* erişimi reddetmek amacıyla belirli bir kapsamdaki bir kullanıcı, grup veya hizmet ilkesine bir dizi reddetme eylemi bağlar. Atamaları reddetme, bir rol ataması onlara erişim izni verse bile kullanıcıların belirli Azure kaynak eylemlerini gerçekleştirmesini engeller.

Bu makalede, reddet atamalarınasıl tanımlandığı açıklanmaktadır.

## <a name="how-deny-assignments-are-created"></a>Reddet atamaları nasıl oluşturulur?

Reddet atamaları, kaynakları korumak için Azure tarafından oluşturulur ve yönetilir. Azure Planları ve Azure yönetilen uygulamalar, sistem tarafından yönetilen kaynakları korumak için atamaları reddetme yi kullanır. Azure Planları ve Azure yönetilen uygulamalar, atamaları reddetmenin tek yoludur. Doğrudan kendi reddi atamaları oluşturamaz. Blueprints'in kaynakları kilitlemek için atamaları nasıl reddettiği hakkında daha fazla bilgi için [bkz.](../governance/blueprints/concepts/resource-locking.md)

> [!NOTE]
> Doğrudan kendi reddi atamaları oluşturamaz.

## <a name="compare-role-assignments-and-deny-assignments"></a>Rol atamalarını karşılaştırma ve atamaları reddetme

Reddet atamaları rol atamaları gibi benzer bir desen izleyin, ancak bazı farklılıklar da vardır.

| Özellik | Rol ataması | Atamayı reddet |
| --- | --- | --- |
| Erişim verme | :heavy_check_mark: |  |
| Erişimi engelleme |  | :heavy_check_mark: |
| Doğrudan oluşturulabilir | :heavy_check_mark: |  |
| Bir kapsamda uygulama | :heavy_check_mark: | :heavy_check_mark: |
| İlkeleri hariç tutma |  | :heavy_check_mark: |
| Alt kapsamlara devralmayı önleme |  | :heavy_check_mark: |
| Klasik [abonelik yöneticisi](rbac-and-directory-admin-roles.md) atamalarına başvurun |  | :heavy_check_mark: |

## <a name="deny-assignment-properties"></a>Atama özelliklerini reddetme

 Reddet ataması aşağıdaki özelliklere sahiptir:

> [!div class="mx-tableFixed"]
> | Özellik | Gerekli | Tür | Açıklama |
> | --- | --- | --- | --- |
> | `DenyAssignmentName` | Evet | Dize | Reddet atamanın görüntü adı. Belirli bir kapsam için adlar benzersiz olmalıdır. |
> | `Description` | Hayır | Dize | Reddet atamanın açıklaması. |
> | `Permissions.Actions` | En az bir Eylem veya bir DataActions | Dize[] | Reddet atamanın erişimi engellediği yönetim işlemlerini belirten dizeler dizisi. |
> | `Permissions.NotActions` | Hayır | Dize[] | Reddet ataması hariç tutmak için yönetim işlemleri belirten dizeleri bir dizi. |
> | `Permissions.DataActions` | En az bir Eylem veya bir DataActions | Dize[] | Reddet atamanın erişimi engellediği veri işlemlerini belirten dizeler dizisi. |
> | `Permissions.NotDataActions` | Hayır | Dize[] | Reddet ataması hariç tutmak için veri işlemleri belirten dizeleri bir dizi. |
> | `Scope` | Hayır | Dize | Reddet atamasının uygulandığı kapsamı belirten bir dize. |
> | `DoNotApplyToChildScopes` | Hayır | Boole | Reddet atamasının alt kapsamlar için geçerli olup olmadığını belirtir. Varsayılan değer yanlıştır. |
> | `Principals[i].Id` | Evet | Dize[] | Reddet atamasının geçerli olduğu bir dizi Azure AD ana nesne kimliği (kullanıcı, grup, hizmet sorumlusu veya yönetilen kimlik). Tüm ilkeleri temsil `00000000-0000-0000-0000-000000000000` edecek boş bir GUID olarak ayarlayın. |
> | `Principals[i].Type` | Hayır | Dize[] | Principals[i].Id. Tarafından temsil edilen nesne türleri `SystemDefined` dizisi tüm ilkeleri temsil edecek şekilde ayarlanır. |
> | `ExcludePrincipals[i].Id` | Hayır | Dize[] | Reddet atamasının geçerli olmadığı bir dizi Azure AD ana nesne kimliği (kullanıcı, grup, hizmet sorumlusu veya yönetilen kimlik). |
> | `ExcludePrincipals[i].Type` | Hayır | Dize[] | ExcludePrincipals[i].Id tarafından temsil edilen nesne türleri dizisi. |
> | `IsSystemProtected` | Hayır | Boole | Bu reddet atamanın Azure tarafından oluşturulup oluşturulmayacağını ve düzenlenip silinemeyeceğini belirtir. Şu anda, tüm reddet atamaları sistem korumalı. |

## <a name="the-all-principals-principal"></a>Tüm Müdürler müdürü

Reddet atamaları desteklemek için, Tüm *İlkeler* adlı sistem tanımlı bir müdür tanıtıldı. Bu ilke, Azure REKLAM dizinindeki tüm kullanıcıları, grupları, hizmet ilkelerini ve yönetilen kimlikleri temsil eder. Asıl kimlik sıfır GUID `00000000-0000-0000-0000-000000000000` ise ve asıl `SystemDefined`tür, asıl tüm ilkeleri temsil eder. Azure PowerShell çıkışında, Tüm Prensipler aşağıdaki gibi görünür:

```azurepowershell
Principals              : {
                          DisplayName:  All Principals
                          ObjectType:   SystemDefined
                          ObjectId:     00000000-0000-0000-0000-000000000000
                          }
```

Bazı kullanıcılar hariç tüm `ExcludePrincipals` ilkeleri reddetmek için tüm Prensipler ile birleştirilebilir. Tüm İlkeler aşağıdaki kısıtlamalara sahiptir:

- Yalnızca içinde `Principals` kullanılabilir ve `ExcludePrincipals`kullanılamaz.
- `Principals[i].Type``SystemDefined`olarak ayarlanmalıdır.

## <a name="next-steps"></a>Sonraki adımlar

* [Öğretici: Azure Blueprints kaynak kilitleriyle yeni kaynakları koruyun](../governance/blueprints/tutorials/protect-new-resources.md)
* [Azure portalını kullanarak Azure kaynakları için atamaları reddetme listesi](deny-assignments-portal.md)
