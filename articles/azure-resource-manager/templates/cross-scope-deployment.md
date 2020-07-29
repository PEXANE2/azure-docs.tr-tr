---
title: Kapsamları kapsamlar arasında dağıtma
description: Dağıtım sırasında birden fazla kapsamın nasıl hedeflenecek gösterir. Kapsam bir kiracı, yönetim grubu, abonelik ve kaynak grupları olabilir.
ms.topic: conceptual
ms.date: 07/28/2020
ms.openlocfilehash: 6161401ac039551a814b595715f56df1ac62dd6c
ms.sourcegitcommit: f353fe5acd9698aa31631f38dd32790d889b4dbb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87374562"
---
# <a name="deploy-azure-resources-across-scopes"></a>Azure kaynaklarını kapsamlar arasında dağıtma

Azure Resource Manager şablonları (ARM şablonları) sayesinde, tek bir dağıtımda birden fazla kapsama dağıtabilirsiniz. Kullanılabilir kapsamlar bir kiracı, yönetim grubu, abonelik ve kaynak gruplarıdır. Örneğin, kaynakları bir kaynak grubuna dağıtabilirsiniz ve aynı şablonda kaynakları başka bir kaynak grubuna dağıtın. Ya da, kaynakları bir yönetim grubuna dağıtabilir ve ayrıca kaynakları bir yönetim grubu içindeki bir kaynak grubuna dağıtabilirsiniz.

Dağıtım işleminin birincil kapsamından farklı olan kapsamları belirtmek için [iç içe geçmiş veya bağlantılı şablonlar](linked-templates.md) kullanın.

## <a name="available-scopes"></a>Kullanılabilir kapsamlar

Dağıtım işlemi için kullandığınız kapsam, hangi diğer kapsamların kullanılabilir olduğunu belirler. [Kiracı](deploy-to-tenant.md), [Yönetim grubu](deploy-to-management-group.md), [abonelik](deploy-to-subscription.md)veya [kaynak grubuna](deploy-powershell.md)dağıtım yapabilirsiniz. Birincil dağıtım düzeyinden hiyerarşide düzeyler 'e gidemezsiniz. Örneğin, bir aboneliğe dağıtırsanız, kaynakları bir yönetim grubuna dağıtmak için bir düzey ilerleyemiyorum. Ancak, bir aboneliğe veya kaynak grubuna dağıtmak için yönetim grubuna ve adım aşağı düzeylere dağıtım yapabilirsiniz.

Her kapsam için, şablonu dağıtan kullanıcının kaynak oluşturmak için gerekli izinlere sahip olması gerekir.

## <a name="cross-resource-groups"></a>Çapraz kaynak grupları

Üst şablon için olandan farklı bir kaynak grubunu hedeflemek için, [iç içe veya bağlı bir şablon](linked-templates.md)kullanın. Dağıtım kaynak türü içinde, iç içe şablonun dağıtılmasını istediğiniz abonelik KIMLIĞI ve kaynak grubu için değerleri belirtin. Kaynak grupları farklı aboneliklerde bulunabilir.

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/crosssubscription.json" range="38-43" highlight="5-6":::

Abonelik KIMLIĞINI veya kaynak grubunu belirtmezseniz, üst şablondaki abonelik ve kaynak grubu kullanılır. Dağıtımı çalıştırmadan önce tüm kaynak gruplarının mevcut olması gerekir.

> [!NOTE]
> Tek bir dağıtımda **800 kaynak grubuna** dağıtım yapabilirsiniz. Genellikle, bu sınırlama, üst şablon için belirtilen bir kaynak grubuna ve iç içe veya bağlı dağıtımlardaki 799 ' e kadar kaynak grubuna dağıtabileceğiniz anlamına gelir. Ancak, ana şablonunuz yalnızca iç içe veya bağlı şablonlar içeriyorsa ve herhangi bir kaynak dağıtmadığından, iç içe veya bağlı dağıtımlara en fazla 800 kaynak grubu ekleyebilirsiniz.

Aşağıdaki örnek iki depolama hesabı dağıtır. İlk depolama hesabı, dağıtım işleminde belirtilen kaynak grubuna dağıtılır. İkinci depolama hesabı, ve parametrelerinde belirtilen kaynak grubuna dağıtılır `secondResourceGroup` `secondSubscriptionID` :

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/crosssubscription.json":::

`resourceGroup`Mevcut olmayan bir kaynak grubunun adına ayarlarsanız, dağıtım başarısız olur.

Önceki şablonu test etmek ve sonuçları görmek için PowerShell veya Azure CLı kullanın.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

İki depolama hesabını **aynı abonelikte**iki kaynak grubuna dağıtmak için şunu kullanın:

```azurepowershell-interactive
$firstRG = "primarygroup"
$secondRG = "secondarygroup"

New-AzResourceGroup -Name $firstRG -Location southcentralus
New-AzResourceGroup -Name $secondRG -Location eastus

New-AzResourceGroupDeployment `
  -ResourceGroupName $firstRG `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/crosssubscription.json `
  -storagePrefix storage `
  -secondResourceGroup $secondRG `
  -secondStorageLocation eastus
```

İki **aboneliğe**iki depolama hesabı dağıtmak için şunu kullanın:

```azurepowershell-interactive
$firstRG = "primarygroup"
$secondRG = "secondarygroup"

$firstSub = "<first-subscription-id>"
$secondSub = "<second-subscription-id>"

Select-AzSubscription -Subscription $secondSub
New-AzResourceGroup -Name $secondRG -Location eastus

Select-AzSubscription -Subscription $firstSub
New-AzResourceGroup -Name $firstRG -Location southcentralus

New-AzResourceGroupDeployment `
  -ResourceGroupName $firstRG `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/crosssubscription.json `
  -storagePrefix storage `
  -secondResourceGroup $secondRG `
  -secondStorageLocation eastus `
  -secondSubscriptionID $secondSub
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

İki depolama hesabını **aynı abonelikte**iki kaynak grubuna dağıtmak için şunu kullanın:

```azurecli-interactive
firstRG="primarygroup"
secondRG="secondarygroup"

az group create --name $firstRG --location southcentralus
az group create --name $secondRG --location eastus
az deployment group create \
  --name ExampleDeployment \
  --resource-group $firstRG \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/crosssubscription.json \
  --parameters storagePrefix=tfstorage secondResourceGroup=$secondRG secondStorageLocation=eastus
```

İki **aboneliğe**iki depolama hesabı dağıtmak için şunu kullanın:

```azurecli-interactive
firstRG="primarygroup"
secondRG="secondarygroup"

firstSub="<first-subscription-id>"
secondSub="<second-subscription-id>"

az account set --subscription $secondSub
az group create --name $secondRG --location eastus

az account set --subscription $firstSub
az group create --name $firstRG --location southcentralus

az deployment group create \
  --name ExampleDeployment \
  --resource-group $firstRG \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/crosssubscription.json \
  --parameters storagePrefix=storage secondResourceGroup=$secondRG secondStorageLocation=eastus secondSubscriptionID=$secondSub
```

---

## <a name="cross-subscription-management-group-and-tenant"></a>Çapraz abonelik, yönetim grubu ve kiracı

Abonelik, yönetim grubu ve kiracı düzeyinde dağıtımlar için farklı kapsamlar belirtirken, kaynak grupları için örnek gibi iç içe geçmiş dağıtımlar kullanırsınız. Kapsam belirtmek için kullandığınız özellikler farklılık gösterebilir. Bu senaryolar, dağıtım düzeyleri hakkındaki makalelerde ele alınmıştır. Daha fazla bilgi için bkz.

* [Abonelik düzeyinde kaynak grupları ve kaynaklar oluşturma](deploy-to-subscription.md)
* [Yönetim grubu düzeyinde kaynaklar oluşturma](deploy-to-management-group.md)
* [Kiracı düzeyinde kaynaklar oluşturma](deploy-to-tenant.md)

## <a name="how-functions-resolve-in-scopes"></a>Kapsamların içindeki işlevler nasıl çözümlenir

Birden fazla kapsama dağıtırken, [resourceGroup ()](template-functions-resource.md#resourcegroup) ve [Subscription ()](template-functions-resource.md#subscription) işlevleri, şablonu nasıl belirtbir şekilde farklı şekilde çözümlenir. Bir dış şablona bağlandığınızda işlevler her zaman ilgili şablonun kapsamına çözümlenir. Bir şablonu bir üst şablon içine yuvalandığınızda, `expressionEvaluationOptions` işlevlerin ana şablon veya iç içe şablon için kaynak grubuna ve aboneliğe çözümlenip çözümlenmediğini belirtmek için özelliğini kullanın. Özelliğini, `inner` iç içe geçmiş şablonun kapsamına çözülecek şekilde ayarlayın. Özelliğini, `outer` üst şablonun kapsamına çözülecek şekilde ayarlayın.

Aşağıdaki tabloda, işlevlerin üst veya katıştırılmış kaynak grubuna ve aboneliğe çözümlenip çözümlenmediğini gösterir.

| Şablon türü | Kapsam | Çözüm |
| ------------- | ----- | ---------- |
| ble        | dış (varsayılan) | Üst kaynak grubu |
| ble        | Dahili | Alt kaynak grubu |
| bağlandı        | Yok   | Alt kaynak grubu |

Aşağıdaki [örnek şablonda](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/crossresourcegroupproperties.json) şunları gösterir:

* Varsayılan (dış) kapsamlı iç içe şablon
* iç kapsama sahip iç içe şablon
* bağlantılı şablon

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/crossresourcegroupproperties.json":::

Önceki şablonu test etmek ve sonuçları görmek için PowerShell veya Azure CLı kullanın.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
New-AzResourceGroup -Name parentGroup -Location southcentralus
New-AzResourceGroup -Name inlineGroup -Location southcentralus
New-AzResourceGroup -Name linkedGroup -Location southcentralus

New-AzResourceGroupDeployment `
  -ResourceGroupName parentGroup `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/crossresourcegroupproperties.json
```

Yukarıdaki örnekteki çıktı:

```output
 Name             Type                       Value
 ===============  =========================  ==========
 parentRG         String                     Parent resource group is parentGroup
 defaultScopeRG   String                     Default scope resource group is parentGroup
 innerScopeRG     String                     Inner scope resource group is inlineGroup
 linkedRG         String                     Linked resource group is linkedgroup
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli-interactive
az group create --name parentGroup --location southcentralus
az group create --name inlineGroup --location southcentralus
az group create --name linkedGroup --location southcentralus

az deployment group create \
  --name ExampleDeployment \
  --resource-group parentGroup \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/crossresourcegroupproperties.json
```

Yukarıdaki örnekteki çıktı:

```output
"outputs": {
  "defaultScopeRG": {
    "type": "String",
    "value": "Default scope resource group is parentGroup"
  },
  "innerScopeRG": {
    "type": "String",
    "value": "Inner scope resource group is inlineGroup"
  },
  "linkedRG": {
    "type": "String",
    "value": "Linked resource group is linkedGroup"
  },
  "parentRG": {
    "type": "String",
    "value": "Parent resource group is parentGroup"
  }
},
```

---



## <a name="next-steps"></a>Sonraki adımlar

* Şablonunuzda parametrelerin nasıl tanımlanacağını anlamak için bkz. [Azure Resource Manager şablonlarının yapısını ve sözdizimini anlayın](template-syntax.md).
* Yaygın dağıtım hatalarını çözümlemeye yönelik ipuçları için bkz. [Azure Resource Manager ile yaygın Azure dağıtım hatalarını giderme](common-deployment-errors.md).
* SAS belirteci gerektiren bir şablonu dağıtma hakkında daha fazla bilgi için bkz. [özel şablonu SAS belirteci Ile dağıtma](secure-template-with-sas-token.md).
