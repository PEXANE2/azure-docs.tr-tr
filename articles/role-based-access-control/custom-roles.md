---
title: Azure özel rolleri-Azure RBAC
description: Azure kaynakları için ayrıntılı erişim yönetimi için Azure rol tabanlı erişim denetimi (Azure RBAC) ile Azure özel rolleri oluşturmayı öğrenin.
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
ms.date: 04/30/2020
ms.author: rolyon
ms.reviewer: bagovind
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 5030fb50313e1db2173990c55930c22fdf58f559
ms.sourcegitcommit: 4499035f03e7a8fb40f5cff616eb01753b986278
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/03/2020
ms.locfileid: "82734799"
---
# <a name="azure-custom-roles"></a>Azure özel rolleri

> [!IMPORTANT]
> ' Ye `AssignableScopes` bir yönetim grubu eklemek Şu anda önizlemededir.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

[Azure yerleşik rolleri](built-in-roles.md) , kuruluşunuzun belirli ihtiyaçlarını karşılamıyorsa, kendi özel rollerinizi de oluşturabilirsiniz. Yerleşik rollerde olduğu gibi, yönetim grubu, abonelik ve kaynak grubu kapsamları ' nda kullanıcılara, gruplara ve hizmet sorumlularına özel roller atayabilirsiniz.

Özel Roller aynı Azure AD dizinine güvenen abonelikler arasında paylaşılabilir. Dizin başına **5.000** özel rol sınırı vardır. (Azure Almanya ve Azure Çin 21Vianet için sınır 2.000 özel rollerdir.) Özel roller Azure portal, Azure PowerShell, Azure CLı veya REST API kullanılarak oluşturulabilir.

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
    "/providers/Microsoft.Management/managementGroups/{groupId1}"
  ]
}
```

Özel bir rol oluşturduğunuzda, bir turuncu kaynak simgesiyle Azure portal görüntülenir.

![Özel rol simgesi](./media/custom-roles/roles-custom-role-icon.png)

## <a name="steps-to-create-a-custom-role"></a>Özel rol oluşturma adımları

1. Özel rolü nasıl oluşturmak istediğinize karar verme

    [Azure Portal](custom-roles-portal.md), [Azure POWERSHELL](custom-roles-powershell.md), [Azure CLI](custom-roles-cli.md)veya [REST API](custom-roles-rest.md)kullanarak özel roller oluşturabilirsiniz.

1. İhtiyaç duyduğunuz izinleri belirleme

    Özel bir rol oluşturduğunuzda, izinlerinizi tanımlamak için kullanılabilir kaynak sağlayıcısı işlemlerini bilmeniz gerekir. İşlem listesini görüntülemek için [Azure Resource Manager kaynak sağlayıcısı işlemlerine](resource-provider-operations.md)bakın. İşlemleri `Actions` [Rol tanımının](role-definitions.md)veya `NotActions` özelliklerine ekleyeceksiniz. Veri işlemlerdir, bunları `DataActions` veya `NotDataActions` özelliklerine eklersiniz.

1. Özel rol oluşturma

    Genellikle, mevcut yerleşik bir rolle başlayıp gereksinimlerinize göre değiştirirsiniz. Ardından, özel rolü oluşturmak için [New-AzRoleDefinition](/powershell/module/az.resources/new-azroledefinition) veya [az role Definition Create](/cli/azure/role/definition#az-role-definition-create) komutlarını kullanırsınız. Özel bir rol oluşturmak için `Microsoft.Authorization/roleDefinitions/write` , [sahip](built-in-roles.md#owner) veya [Kullanıcı erişimi Yöneticisi](built-in-roles.md#user-access-administrator)gibi bir `AssignableScopes`izninizin olması gerekir.

1. Özel rolü test etme

    Özel rolünüzü aldıktan sonra, beklendiği gibi çalıştığını doğrulamak için test etmeniz gerekir. Daha sonra ayarlamalar yapmanız gerekiyorsa, özel rolü güncelleştirebilirsiniz.

Özel rol oluşturma hakkında adım adım bir öğretici için bkz. [öğretici: Azure PowerShell veya öğretici kullanarak Azure özel rolü oluşturma](tutorial-custom-role-powershell.md) [: Azure CLI kullanarak Azure özel rolü](tutorial-custom-role-cli.md)oluşturma.

## <a name="custom-role-properties"></a>Özel rol özellikleri

Özel bir rol aşağıdaki özelliklere sahiptir.

| Özellik | Gerekli | Tür | Açıklama |
| --- | --- | --- | --- |
| `Name` | Yes | Dize | Özel rolün görünen adı. Rol tanımı bir yönetim grubu veya abonelik düzeyi kaynağı olsa da, aynı Azure AD dizinini paylaşan birden çok abonelikte rol tanımı kullanılabilir. Bu görünen ad, Azure AD dizininin kapsamında benzersiz olmalıdır. Harfler, rakamlar, boşluklar ve özel karakterler içerebilir. En fazla karakter sayısı 128 ' dir. |
| `Id` | Yes | Dize | Özel rolün benzersiz KIMLIĞI. Azure PowerShell ve Azure CLı için bu KIMLIK, yeni bir rol oluşturduğunuzda otomatik olarak oluşturulur. |
| `IsCustom` | Yes | Dize | Bunun özel bir rol olup olmadığını gösterir. Özel roller `true` için olarak ayarlayın. |
| `Description` | Yes | Dize | Özel rolün açıklaması. Harfler, rakamlar, boşluklar ve özel karakterler içerebilir. En fazla karakter sayısı 1024 ' dir. |
| `Actions` | Yes | String [] | Rolün gerçekleştirilmesine izin verdiği yönetim işlemlerini belirten dizeler dizisi. Daha fazla bilgi için bkz. [Eylemler](role-definitions.md#actions). |
| `NotActions` | No | String [] | İzin verilen `Actions`' dan dışlanan yönetim işlemlerini belirten dizeler dizisi. Daha fazla bilgi için bkz. [NotActions](role-definitions.md#notactions). |
| `DataActions` | No | String [] | Rolün bu nesne içindeki verilerinize gerçekleştirilmesine izin verdiği veri işlemlerini belirten dizeler dizisi. İle `DataActions`özel bir rol oluşturursanız, bu rol yönetim grubu kapsamında atanamaz. Daha fazla bilgi için bkz. [Dataactions](role-definitions.md#dataactions). |
| `NotDataActions` | No | String [] | İzin verilen `DataActions`' dan dışlanan veri işlemlerini belirten dizeler dizisi. Daha fazla bilgi için bkz. [Notdataactions](role-definitions.md#notdataactions). |
| `AssignableScopes` | Yes | String [] | Özel rolün atama için kullanılabilir olduğu kapsamları belirten dizeler dizisi. Özel bir rol içinde `AssignableScopes` yalnızca bir yönetim grubu tanımlayabilirsiniz. ' Ye `AssignableScopes` bir yönetim grubu eklemek Şu anda önizlemededir. Daha fazla bilgi için bkz. [Astifblescopes](role-definitions.md#assignablescopes). |

## <a name="who-can-create-delete-update-or-view-a-custom-role"></a>Özel bir rol oluşturabilir, silebilir, güncelleştirebilir veya görüntüleyebilir

Yerleşik rollerde olduğu gibi, `AssignableScopes` özelliği rolün atama için kullanılabilir olduğu kapsamları belirtir. Özel `AssignableScopes` bir rol için özelliği ayrıca, özel rolü kimlerin oluşturerişebileceğini, silediğini, güncelleştirediğini veya görüntüleyemeyeceğini de denetler.

| Görev | İşlem | Açıklama |
| --- | --- | --- |
| Özel bir rol oluşturma/silme | `Microsoft.Authorization/ roleDefinitions/write` | Tüm özel rol üzerinde bu işleme verilen kullanıcılar, `AssignableScopes` bu kapsamlarda kullanılmak üzere özel roller oluşturabilir (veya silebilir). Örneğin, [sahipler](built-in-roles.md#owner) ve [Kullanıcı erişimi](built-in-roles.md#user-access-administrator) yönetim gruplarının, aboneliklerinin ve kaynak gruplarının yöneticileri. |
| Özel rolü güncelleştirme | `Microsoft.Authorization/ roleDefinitions/write` | Tüm özel rol üzerinde bu işleme verilen kullanıcılar, `AssignableScopes` bu kapsamlardaki özel rolleri güncelleştirebilir. Örneğin, [sahipler](built-in-roles.md#owner) ve [Kullanıcı erişimi](built-in-roles.md#user-access-administrator) yönetim gruplarının, aboneliklerinin ve kaynak gruplarının yöneticileri. |
| Özel bir rol görüntüleme | `Microsoft.Authorization/ roleDefinitions/read` | Bu işlem bir kapsamda verilen kullanıcılar, bu kapsamda atama için kullanılabilen özel rolleri görüntüleyebilir. Tüm yerleşik roller, özel rollerin atama için kullanılabilir olmasını sağlar. |

## <a name="custom-role-limits"></a>Özel rol limitleri

Aşağıdaki listede özel roller için sınırlar açıklanmaktadır.

- Her bir dizin en fazla **5000** özel role sahip olabilir.
- Azure Almanya ve Azure Çin 21Vianet, her bir dizin için en fazla 2000 özel rol içerebilir.
- Kök kapsamına ( `AssignableScopes` `"/"`) ayarlayamazsınız.
- Özel bir rol içinde `AssignableScopes` yalnızca bir yönetim grubu tanımlayabilirsiniz. ' Ye `AssignableScopes` bir yönetim grubu eklemek Şu anda önizlemededir.
- Yönetim grubu kapsamında `DataActions` özel roller atanamaz.
- Azure Resource Manager, rol tanımının atanabilir kapsamındaki yönetim grubunun varlığını doğrulamaz.

Özel roller ve yönetim grupları hakkında daha fazla bilgi için bkz. [Azure Yönetim gruplarıyla kaynaklarınızı düzenleme](../governance/management-groups/overview.md#custom-rbac-role-definition-and-assignment).

## <a name="next-steps"></a>Sonraki adımlar
- [Azure portal kullanarak Azure özel rolleri oluşturun veya güncelleştirin](custom-roles-portal.md)
- [Azure rol tanımlarını anlama](role-definitions.md)
- [Azure RBAC sorunlarını giderme](troubleshooting.md)
