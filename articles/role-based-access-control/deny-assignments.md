---
title: Azure kaynakları için atamaları reddetme ' yı anlama | Microsoft Docs
description: Azure kaynakları için rol tabanlı erişim denetimi 'nde (RBAC) atamaları reddetme hakkında bilgi edinin.
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
ms.date: 11/25/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.custom: ''
ms.openlocfilehash: 2c663b587d2e9ee278fc774c2841899b060ccbcf
ms.sourcegitcommit: 8cf199fbb3d7f36478a54700740eb2e9edb823e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2019
ms.locfileid: "74479368"
---
# <a name="understand-deny-assignments-for-azure-resources"></a>Azure kaynakları için reddedilen atamaları anlama

Bir rol atamasına benzer şekilde, *reddetme atama* , erişimi reddetmek amacıyla belirli bir kapsamdaki bir Kullanıcı, Grup veya hizmet sorumlusu için bir reddetme eylemleri kümesi iliştirir. Bir rol ataması erişime izin verirse, atamaları Reddet, kullanıcıların belirli Azure Kaynak eylemlerini gerçekleştirmesini engeller.

Bu makalede, reddetme atamalarının nasıl tanımlandığı açıklanır.

## <a name="how-deny-assignments-are-created"></a>Reddetme atamaları nasıl oluşturulur

Reddetme atamaları, kaynakları korumak için Azure tarafından oluşturulur ve yönetilir. Azure şemaları ve Azure yönetilen uygulamalar, sistem tarafından yönetilen kaynakları korumak için reddetme atamalarını kullanır. Azure şemaları ve Azure yönetilen uygulamalar, atamaları reddetme için tek yoldur. Kendi reddetme atamalarınızı doğrudan oluşturamazsınız.  Daha fazla bilgi için bkz. [Azure şemaları kaynak kilitleri ile yeni kaynakları koruma](../governance/blueprints/tutorials/protect-new-resources.md).

> [!NOTE]
> Kendi reddetme atamalarınızı doğrudan oluşturamazsınız.

## <a name="compare-role-assignments-and-deny-assignments"></a>Rol atamalarını karşılaştırma ve atamaları reddetme

Atamaları Reddet, benzer bir kalıbı rol atamaları olarak izler, ancak bazı farklılıklar da vardır.

| Özellik | Rol ataması | Atamayı Reddet |
| --- | --- | --- |
| Erişim verme | :heavy_check_mark: |  |
| Erişimi engelleme |  | :heavy_check_mark: |
| Doğrudan oluşturulabilir | :heavy_check_mark: |  |
| Bir kapsamda Uygula | :heavy_check_mark: | :heavy_check_mark: |
| Sorumluları hariç tut |  | :heavy_check_mark: |
| Alt kapsamlar devralınmasını engelle |  | :heavy_check_mark: |
| [Klasik abonelik Yöneticisi](rbac-and-directory-admin-roles.md) atamalarına Uygula |  | :heavy_check_mark: |

## <a name="deny-assignment-properties"></a>Atama özelliklerini Reddet

 Reddetme atama aşağıdaki özelliklere sahiptir:

> [!div class="mx-tableFixed"]
> | Özellik | Gerekli | Type | Açıklama |
> | --- | --- | --- | --- |
> | `DenyAssignmentName` | Evet | Dize | Reddetme atamasının görünen adı. Adlar, belirli bir kapsam için benzersiz olmalıdır. |
> | `Description` | Hayır | Dize | Reddetme atamasının açıklaması. |
> | `Permissions.Actions` | En az bir eylem veya bir veri eylemi | String[] | Reddetme atamasının erişimi engellediği yönetim işlemlerini belirten dizeler dizisi. |
> | `Permissions.NotActions` | Hayır | String[] | Reddetme atamasından dışlanacak yönetim işlemlerini belirten dizeler dizisi. |
> | `Permissions.DataActions` | En az bir eylem veya bir veri eylemi | String[] | Reddetme atamasının erişimi engellediği veri işlemlerini belirten dizeler dizisi. |
> | `Permissions.NotDataActions` | Hayır | String[] | Reddetme atamasından çıkarılacak veri işlemlerini belirten dizeler dizisi. |
> | `Scope` | Hayır | Dize | Reddetme atamasının geçerli olduğu kapsamı belirten bir dize. |
> | `DoNotApplyToChildScopes` | Hayır | Boole değeri | Reddetme atamasının alt kapsamlar için geçerli olup olmadığını belirtir. Varsayılan değer false 'dur. |
> | `Principals[i].Id` | Evet | String[] | Reddetme atamasının uygulandığı Azure AD Principal nesne kimliklerinin (Kullanıcı, Grup, hizmet sorumlusu veya yönetilen kimlik) bir dizisi. Tüm sorumluları temsil etmek için `00000000-0000-0000-0000-000000000000` boş bir GUID olarak ayarlayın. |
> | `Principals[i].Type` | Hayır | String[] | Sorumlular tarafından temsil edilen nesne türleri dizisi [i]. ID. tüm sorumlularını temsil etmek için `SystemDefined` olarak ayarlayın. |
> | `ExcludePrincipals[i].Id` | Hayır | String[] | Reddetme atamasının uygulanmadığından, Azure AD sorumlusu nesne kimliklerinin (Kullanıcı, Grup, hizmet sorumlusu veya yönetilen kimlik) bir dizisi. |
> | `ExcludePrincipals[i].Type` | Hayır | String[] | Excludesorumlularını [i]. ID tarafından temsil edilen nesne türleri dizisi. |
> | `IsSystemProtected` | Hayır | Boole değeri | Bu reddetme atamasının Azure tarafından oluşturulup oluşturulmayacağını veya silinemeyeceğini belirtir. Şu anda tüm reddetme atamaları sistem korumalıdır. |

## <a name="the-all-principals-principal"></a>Tüm asıl adlar

Reddedilen atamaları desteklemek için, *Tüm sorumlular* adlı sistem tanımlı bir sorumlu tanıtılmıştır. Bu sorumlu, bir Azure AD dizinindeki tüm kullanıcıları, grupları, hizmet sorumlularını ve yönetilen kimlikleri temsil eder. Asıl KIMLIK `00000000-0000-0000-0000-000000000000` sıfır bir GUID ise ve asıl tür `SystemDefined`, sorumlu ise tüm sorumluları temsil eder. Azure PowerShell çıktısında, tüm sorumlular aşağıdaki gibi görünür:

```azurepowershell
Principals              : {
                          DisplayName:  All Principals
                          ObjectType:   SystemDefined
                          ObjectId:     00000000-0000-0000-0000-000000000000
                          }
```

Tüm sorumlular, bazı kullanıcılar hariç tüm sorumluları reddetmek için `ExcludePrincipals` ile birleştirilebilir. Tüm sorumlular aşağıdaki kısıtlamalara sahiptir:

- Yalnızca `Principals` için kullanılabilir ve `ExcludePrincipals`kullanılamaz.
- `Principals[i].Type` `SystemDefined`olarak ayarlanmalıdır.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure portal kullanarak Azure kaynakları için atamaları reddetme listesi](deny-assignments-portal.md)
* [Azure kaynakları için rol tanımlarını anlama](role-definitions.md)
