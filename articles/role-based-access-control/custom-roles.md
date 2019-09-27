---
title: Azure kaynakları için özel roller | Microsoft Docs
description: Azure kaynaklarının ayrıntılı erişim yönetimi için rol tabanlı erişim denetimi (RBAC) ile özel roller oluşturmayı öğrenin.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: e4206ea9-52c3-47ee-af29-f6eef7566fa5
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/07/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: fbea0567ec125ce12acd8f757b32df723876fe09
ms.sourcegitcommit: e1b6a40a9c9341b33df384aa607ae359e4ab0f53
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71338563"
---
# <a name="custom-roles-for-azure-resources"></a>Azure kaynakları için özel roller

[Azure kaynaklarına yönelik yerleşik roller](built-in-roles.md) , kuruluşunuzun belirli ihtiyaçlarını karşılamıyorsa, kendi özel rollerinizi oluşturabilirsiniz. Yerleşik rollerde olduğu gibi, abonelik, kaynak grubu ve kaynak kapsamlarındaki kullanıcılara, gruplara ve hizmet sorumlularına özel roller de atayabilirsiniz.

Özel Roller bir Azure Active Directory (Azure AD) dizininde depolanır ve abonelikler arasında paylaşılabilir. Her bir dizin en fazla **5000** özel role sahip olabilir. (Azure Kamu, Azure Almanya ve Azure Çin 21Vianet gibi özel Bulutlar için 2000 özel rol sınırı vardır.) Özel roller Azure PowerShell, Azure CLı veya REST API kullanılarak oluşturulabilir.

## <a name="custom-role-example"></a>Özel rol örneği

Aşağıda, özel bir rolün JSON biçiminde gösterildiği gibi nasıl göründüğü gösterilmektedir. Bu özel rol, sanal makineleri izlemek ve yeniden başlatmak için kullanılabilir.

```json
{
  "Name": "Virtual Machine Operator",
  "Id": "88888888-8888-8888-8888-888888888888",
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
    "Microsoft.Insights/diagnosticSettings/*",
    "Microsoft.Support/*"
  ],
  "NotActions": [],
  "DataActions": [],
  "NotDataActions": [],
  "AssignableScopes": [
    "/subscriptions/{subscriptionId1}",
    "/subscriptions/{subscriptionId2}",
    "/subscriptions/{subscriptionId3}"
  ]
}
```

Özel bir rol oluşturduğunuzda, bir turuncu kaynak simgesiyle Azure portal görüntülenir.

![Özel rol simgesi](./media/custom-roles/roles-custom-role-icon.png)

## <a name="steps-to-create-a-custom-role"></a>Özel rol oluşturma adımları

1. Özel rolü nasıl oluşturmak istediğinize karar verme

    [Azure PowerShell](custom-roles-powershell.md), [Azure CLI](custom-roles-cli.md)veya [REST API](custom-roles-rest.md)kullanarak özel roller oluşturabilirsiniz.

1. İhtiyaç duyduğunuz izinleri belirleme

    Özel bir rol oluşturduğunuzda, izinlerinizi tanımlamak için kullanılabilir kaynak sağlayıcısı işlemlerini bilmeniz gerekir. İşlem listesini görüntülemek için [Azure Resource Manager kaynak sağlayıcısı işlemlerine](resource-provider-operations.md)bakın. İşlemleri [Rol tanımının](role-definitions.md)`Actions` veya `NotActions` özelliklerine ekleyeceksiniz. Veri İşlemsiz sahipseniz, `DataActions` veya `NotDataActions` özelliklerine eklersiniz.

1. Özel rol oluşturma

    Genellikle, mevcut yerleşik bir rolle başlayıp gereksinimlerinize göre değiştirirsiniz. Ardından, özel rolü oluşturmak için [New-AzRoleDefinition](/powershell/module/az.resources/new-azroledefinition) veya [az role Definition Create](/cli/azure/role/definition#az-role-definition-create) komutlarını kullanırsınız. Özel bir rol oluşturmak için, [sahip](built-in-roles.md#owner) veya [Kullanıcı erişimi yöneticisi](built-in-roles.md#user-access-administrator)gibi tüm `AssignableScopes` `Microsoft.Authorization/roleDefinitions/write` izninizin olması gerekir.

1. Özel rolü test etme

    Özel rolünüzü aldıktan sonra, beklendiği gibi çalıştığını doğrulamak için test etmeniz gerekir. Daha sonra ayarlamalar yapmanız gerekiyorsa, özel rolü güncelleştirebilirsiniz.

Özel rol oluşturma hakkında adım adım bir öğretici için bkz. [Öğreticisi: Azure PowerShell @ no__t-0 veya [Öğreticisi kullanarak özel bir rol oluşturun: Azure CLı @ no__t-0 kullanarak özel bir rol oluşturun.

## <a name="custom-role-properties"></a>Özel rol özellikleri

Özel bir rol aşağıdaki özelliklere sahiptir.

| Özellik | Gerekli | Tür | Açıklama |
| --- | --- | --- | --- |
| `Name` | Evet | Dize | Özel rolün görünen adı. Rol tanımı, abonelik düzeyinde bir kaynaktır, ancak aynı Azure AD dizinini paylaşan birden çok abonelikte rol tanımı kullanılabilir. Bu görünen ad, Azure AD dizininin kapsamında benzersiz olmalıdır. Harfler, rakamlar, boşluklar ve özel karakterler içerebilir. En fazla karakter sayısı 128 ' dir. |
| `Id` | Evet | Dize | Özel rolün benzersiz KIMLIĞI. Azure PowerShell ve Azure CLı için bu KIMLIK, yeni bir rol oluşturduğunuzda otomatik olarak oluşturulur. |
| `IsCustom` | Evet | Dize | Bunun özel bir rol olup olmadığını gösterir. Özel roller için `true` olarak ayarlayın. |
| `Description` | Evet | Dize | Özel rolün açıklaması. Harfler, rakamlar, boşluklar ve özel karakterler içerebilir. En fazla karakter sayısı 1024 ' dir. |
| `Actions` | Evet | String[] | Rolün gerçekleştirilmesine izin verdiği yönetim işlemlerini belirten dizeler dizisi. Daha fazla bilgi için bkz. [Eylemler](role-definitions.md#actions). |
| `NotActions` | Hayır | String[] | İzin verilen `Actions` ' dan dışlanan yönetim işlemlerini belirten dizeler dizisi. Daha fazla bilgi için bkz. [NotActions](role-definitions.md#notactions). |
| `DataActions` | Hayır | String[] | Rolün bu nesne içindeki verilerinize gerçekleştirilmesine izin verdiği veri işlemlerini belirten dizeler dizisi. Daha fazla bilgi için bkz. [Dataactions](role-definitions.md#dataactions). |
| `NotDataActions` | Hayır | String[] | İzin verilen `DataActions` ' dan dışlanan veri işlemlerini belirten dizeler dizisi. Daha fazla bilgi için bkz. [Notdataactions](role-definitions.md#notdataactions). |
| `AssignableScopes` | Evet | String[] | Özel rolün atama için kullanılabilir olduğu kapsamları belirten dizeler dizisi. Özel roller için şu anda `AssignableScopes` ' ı kök kapsamına (`"/"`) veya bir yönetim grubu kapsamına ayarlayamazsınız. Daha fazla bilgi için bkz. [Astifblescopes](role-definitions.md#assignablescopes) ve [kaynaklarınızı Azure Yönetim gruplarıyla düzenleme](../governance/management-groups/overview.md#custom-rbac-role-definition-and-assignment). |

## <a name="who-can-create-delete-update-or-view-a-custom-role"></a>Özel bir rol oluşturabilir, silebilir, güncelleştirebilir veya görüntüleyebilir

Yerleşik rollerde olduğu gibi, `AssignableScopes` özelliği rolün atama için kullanılabilir olduğu kapsamları belirtir. Özel bir rol için `AssignableScopes` özelliği ayrıca, özel rolü kimlerin oluşturerişebileceğini, silediğini, güncelleştirediğini veya görüntüleyemeyeceğini de denetler.

| Görev | İşlem | Açıklama |
| --- | --- | --- |
| Özel bir rol oluşturma/silme | `Microsoft.Authorization/ roleDefinitions/write` | Özel rolün tüm `AssignableScopes` ' larında bu işleme verilen kullanıcılar, bu kapsamlarda kullanılmak üzere özel roller oluşturabilir (veya silebilir). Örneğin, [sahipler](built-in-roles.md#owner) ve [Kullanıcı erişimi](built-in-roles.md#user-access-administrator) aboneliklerinin, kaynak gruplarının ve kaynakların yöneticileri. |
| Özel rolü güncelleştirme | `Microsoft.Authorization/ roleDefinitions/write` | Özel rolün tüm `AssignableScopes` ' larında bu işleme verilen kullanıcılar, bu kapsamlardaki özel rolleri güncelleştirebilir. Örneğin, [sahipler](built-in-roles.md#owner) ve [Kullanıcı erişimi](built-in-roles.md#user-access-administrator) aboneliklerinin, kaynak gruplarının ve kaynakların yöneticileri. |
| Özel bir rol görüntüleme | `Microsoft.Authorization/ roleDefinitions/read` | Bu işlem bir kapsamda verilen kullanıcılar, bu kapsamda atama için kullanılabilen özel rolleri görüntüleyebilir. Tüm yerleşik roller, özel rollerin atama için kullanılabilir olmasını sağlar. |

## <a name="next-steps"></a>Sonraki adımlar
- [Azure PowerShell kullanarak Azure kaynakları için özel roller oluşturma](custom-roles-powershell.md)
- [Azure CLI kullanarak Azure kaynakları için özel roller oluşturma](custom-roles-cli.md)
- [Azure kaynakları için rol tanımlarını anlama](role-definitions.md)
- [Azure kaynakları için RBAC sorunlarını giderme](troubleshooting.md)
