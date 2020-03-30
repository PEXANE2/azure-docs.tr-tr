---
title: Azure kaynakları için özel roller | Microsoft Dokümanlar
description: Azure kaynaklarının ince ayarlı erişim yönetimi için rol tabanlı erişim denetimi (RBAC) ile özel roller oluşturmayı öğrenin.
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
ms.date: 03/19/2020
ms.author: rolyon
ms.reviewer: bagovind
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 9454962e210781559f2fdceb1c36f499c4ae8ff7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80062178"
---
# <a name="custom-roles-for-azure-resources"></a>Azure kaynakları için özel roller

> [!IMPORTANT]
> Bir yönetim grubu `AssignableScopes` eklemek şu anda önizlemededir.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.

Azure [kaynakları için yerleşik roller](built-in-roles.md) kuruluşunuzun özel gereksinimlerini karşılamazsa, kendi özel rollerinizi oluşturabilirsiniz. Yerleşik roller gibi, yönetim grubu, abonelik ve kaynak grubu kapsamlarında kullanıcılara, gruplara ve hizmet ilkelerine özel roller atayabilirsiniz.

Özel roller, aynı Azure AD dizinine güvenen abonelikler arasında paylaşılabilir. Dizin başına **5.000** özel rol sınırı vardır. (Azure Almanya ve Azure China 21Vianet için sınır 2.000 özel roldür.) Azure portalı (Önizleme), Azure PowerShell, Azure CLI veya REST API kullanılarak özel roller oluşturulabilir.

## <a name="custom-role-example"></a>Özel rol örneği

Aşağıda, JSON formatında görüntülenen özel bir rolün nasıl göründüğü gösterilmektedir. Bu özel rol, sanal makineleri izlemek ve yeniden başlatmak için kullanılabilir.

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

Özel bir rol oluşturduğunuzda, turuncu bir kaynak simgesiyle Azure portalında görünür.

![Özel rol simgesi](./media/custom-roles/roles-custom-role-icon.png)

## <a name="steps-to-create-a-custom-role"></a>Özel bir rol oluşturma adımları

1. Özel rolü nasıl oluşturmak istediğinize karar verin

    [Azure portalı](custom-roles-portal.md) (Önizleme), [Azure PowerShell,](custom-roles-powershell.md) [Azure CLI](custom-roles-cli.md)veya [REST API'yi](custom-roles-rest.md)kullanarak özel roller oluşturabilirsiniz.

1. İhtiyacınız olan izinleri belirleme

    Özel bir rol oluşturduğunuzda, izinlerinizi tanımlamak için kullanılabilen kaynak sağlayıcı işlemlerini bilmeniz gerekir. İşlemler listesini görüntülemek için [Azure Kaynak Yöneticisi kaynak sağlayıcısı işlemlerine](resource-provider-operations.md)bakın. İşlemleri [rol tanımının](role-definitions.md) `Actions` özelliklerine `NotActions` eklersiniz. Veri işlemleriniz varsa, bunları veya `DataActions` `NotDataActions` özellikleri eklersiniz.

1. Özel rolü oluşturma

    Genellikle, varolan yerleşik bir rol ile başlar ve sonra gereksinimleriniz için değiştirin. Sonra özel rol oluşturmak için [Yeni-AzRoleDefinition](/powershell/module/az.resources/new-azroledefinition) veya [az rol tanımı oluşturmak](/cli/azure/role/definition#az-role-definition-create) komutları kullanın. Özel bir rol oluşturmak için, `Microsoft.Authorization/roleDefinitions/write` Sahibi `AssignableScopes`veya Kullanıcı [Owner](built-in-roles.md#owner) [Erişim Yöneticisi](built-in-roles.md#user-access-administrator)gibi tüm izine sahip olmalısınız.

1. Özel rolü test edin

    Özel rolünüze sahip olduktan sonra, beklediğiniz gibi çalıştığını doğrulamak için bunu test etmeniz gerekir. Ayarlamaları daha sonra yapmanız gerekirse, özel rolü güncelleştirebilirsiniz.

Özel bir rolün nasıl oluşturulabildiğini anlatan adım adım öğretici için [Bkz.](tutorial-custom-role-powershell.md) [Tutorial: Create a custom role using Azure CLI](tutorial-custom-role-cli.md)

## <a name="custom-role-properties"></a>Özel rol özellikleri

Özel bir rol aşağıdaki özelliklere sahiptir.

| Özellik | Gerekli | Tür | Açıklama |
| --- | --- | --- | --- |
| `Name` | Evet | Dize | Özel rolün görüntü adı. Rol tanımı bir yönetim grubu veya abonelik düzeyi kaynağı olsa da, aynı Azure REKLAM dizinini paylaşan birden çok abonelikte rol tanımı kullanılabilir. Bu görüntü adı, Azure AD dizininin kapsamında benzersiz olmalıdır. Harfleri, sayıları, boşlukları ve özel karakterleri içerebilir. Maksimum karakter sayısı 128'dir. |
| `Id` | Evet | Dize | Özel rolün benzersiz kimliği. Azure PowerShell ve Azure CLI için, yeni bir rol oluşturduğunuzda bu kimlik otomatik olarak oluşturulur. |
| `IsCustom` | Evet | Dize | Bunun özel bir rol olup olmadığını gösterir. Özel `true` roller için ayarlayın. |
| `Description` | Evet | Dize | Özel rolün açıklaması. Harfleri, sayıları, boşlukları ve özel karakterleri içerebilir. Maksimum karakter sayısı 1024'tür. |
| `Actions` | Evet | Dize[] | Rolün gerçekleştirilmesine izin verdiği yönetim işlemlerini belirten bir dizi dize. Daha fazla bilgi için [Eylemler'e](role-definitions.md#actions)bakın. |
| `NotActions` | Hayır | Dize[] | İzin verilenin `Actions`dışında olan yönetim işlemlerini belirten bir dizi dize. Daha fazla bilgi için [NotActions'a](role-definitions.md#notactions)bakın. |
| `DataActions` | Hayır | Dize[] | Rolün bu nesne içindeki verilerinize gerçekleştirilmesine izin verdiği veri işlemlerini belirten bir dizi dize. `DataActions`Bu rol yönetim grubu kapsamında atanamıyor. Daha fazla bilgi için [DataActions'a](role-definitions.md#dataactions)bakın. |
| `NotDataActions` | Hayır | Dize[] | İzin verilenin `DataActions`dışında olan veri işlemlerini belirten bir dizi dize. Daha fazla bilgi için [NotDataActions'a](role-definitions.md#notdataactions)bakın. |
| `AssignableScopes` | Evet | Dize[] | Özel rolüatama için kullanılabilir kapsamları belirten dizeleri bir dizi. Özel bir rolde `AssignableScopes` yalnızca bir yönetim grubu tanımlayabilirsiniz. Bir yönetim grubu `AssignableScopes` eklemek şu anda önizlemededir. Daha fazla bilgi için [Bkz. AssignableScopes.](role-definitions.md#assignablescopes) |

## <a name="who-can-create-delete-update-or-view-a-custom-role"></a>Özel bir rolü kimler oluşturabilir, silebilir, güncelleyebilir veya görüntüleyebilir?

Yerleşik roller gibi, `AssignableScopes` özellik de rolün atama için kullanılabilir olduğu kapsamları belirtir. Özel `AssignableScopes` bir rolün özelliği, özel rolü kimlerin oluşturabileceğini, silebileceğini, güncellenebilen veya görüntüleyebileceğini de denetler.

| Görev | İşlem | Açıklama |
| --- | --- | --- |
| Özel bir rol oluşturma/silme | `Microsoft.Authorization/ roleDefinitions/write` | Özel rolün `AssignableScopes` tümlerinde bu işlem verilen kullanıcılar, bu kapsamlarda kullanılmak üzere özel roller oluşturabilir (veya silebilirsiniz). Örneğin, yönetim gruplarının, aboneliklerin ve kaynak gruplarının [Sahipleri](built-in-roles.md#owner) ve [Kullanıcı Erişimi Yöneticileri.](built-in-roles.md#user-access-administrator) |
| Özel rolü güncelleştirme | `Microsoft.Authorization/ roleDefinitions/write` | Özel rolün `AssignableScopes` tümlerinde bu işlem verilen kullanıcılar, bu kapsamdaki özel rolleri güncelleştirebilir. Örneğin, yönetim gruplarının, aboneliklerin ve kaynak gruplarının [Sahipleri](built-in-roles.md#owner) ve [Kullanıcı Erişimi Yöneticileri.](built-in-roles.md#user-access-administrator) |
| Özel bir rolü görüntüleme | `Microsoft.Authorization/ roleDefinitions/read` | Bu kapsamda bu işlem verilen kullanıcılar, bu kapsamda atama için kullanılabilen özel rolleri görüntüleyebilir. Tüm yerleşik roller, özel rollerin atama için kullanılabilir olmasını sağlar. |

## <a name="custom-role-limits"></a>Özel rol sınırları

Aşağıdaki listede özel rollerin sınırları açıklanmaktadır.

- Her dizin en fazla **5000** özel rol olabilir.
- Azure Almanya ve Azure China 21Vianet her dizin için en fazla 2000 özel rol alabilir.
- Kök kapsamına `AssignableScopes` ayarlayamazsınız`"/"`( ).
- Özel bir rolde `AssignableScopes` yalnızca bir yönetim grubu tanımlayabilirsiniz. Bir yönetim grubu `AssignableScopes` eklemek şu anda önizlemededir.
- Yönetim grubu `DataActions` kapsamında özel roller atanamaz.
- Azure Kaynak Yöneticisi, rol tanımının devredilebilir kapsamında yönetim grubunun varlığını doğrulamaz.

Özel roller ve yönetim grupları hakkında daha fazla bilgi için [bkz.](../governance/management-groups/overview.md#custom-rbac-role-definition-and-assignment)

## <a name="next-steps"></a>Sonraki adımlar
- [Azure portalını kullanarak Azure özel rolleri oluşturma veya güncelleme (Önizleme)](custom-roles-portal.md)
- [Azure kaynakları için rol tanımlarını anlama](role-definitions.md)
- [Azure kaynakları için RBAC sorun giderme](troubleshooting.md)
