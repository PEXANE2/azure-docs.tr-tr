---
title: Kaynakları dağıtım & çapraz abonelik kaynak grubu
description: Dağıtım sırasında birden fazla Azure aboneliğini ve kaynak grubunu nasıl hedefleyecek olduğunu gösterir.
ms.topic: conceptual
ms.date: 05/18/2020
ms.openlocfilehash: 34de1d9df53d61d849ffbb81a57b468020bc3b65
ms.sourcegitcommit: bcb962e74ee5302d0b9242b1ee006f769a94cfb8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2020
ms.locfileid: "86057390"
---
# <a name="deploy-azure-resources-across-subscriptions-or-resource-groups"></a>Azure kaynaklarını abonelikler veya kaynak grupları arasında dağıtma

Kaynak Yöneticisi, tek bir dağıtımda birden fazla kaynak grubuna dağıtmanıza olanak sağlar. Dağıtım işlemindeki kaynak grubundan farklı kaynak gruplarını belirtmek için iç içe geçmiş şablonlar kullanın. Kaynak grupları farklı aboneliklerde bulunabilir.

> [!NOTE]
> Tek bir dağıtımda **800 kaynak grubuna** dağıtım yapabilirsiniz. Genellikle, bu sınırlama, üst şablon için belirtilen bir kaynak grubuna ve iç içe veya bağlı dağıtımlardaki 799 ' e kadar kaynak grubuna dağıtabileceğiniz anlamına gelir. Ancak, ana şablonunuz yalnızca iç içe veya bağlı şablonlar içeriyorsa ve herhangi bir kaynak dağıtmadığından, iç içe veya bağlı dağıtımlara en fazla 800 kaynak grubu ekleyebilirsiniz.

## <a name="specify-subscription-and-resource-group"></a>Abonelik ve kaynak grubu belirtin

Üst şablon için olandan farklı bir kaynak grubunu hedeflemek için, [iç içe veya bağlı bir şablon](linked-templates.md)kullanın. Dağıtım kaynak türü içinde, iç içe şablonun dağıtılmasını istediğiniz abonelik KIMLIĞI ve kaynak grubu için değerleri belirtin.

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/crosssubscription.json" range="38-43" highlight="5-6":::

Abonelik KIMLIĞINI veya kaynak grubunu belirtmezseniz, üst şablondaki abonelik ve kaynak grubu kullanılır. Dağıtımı çalıştırmadan önce tüm kaynak gruplarının mevcut olması gerekir.

Şablonu dağıtan hesabın, belirtilen abonelik KIMLIĞINE dağıtım izni olması gerekir. Belirtilen abonelik farklı bir Azure Active Directory kiracısında varsa, [başka bir dizinden Konuk kullanıcılar eklemeniz](../../active-directory/b2b/what-is-b2b.md)gerekir.

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

## <a name="use-functions"></a>İşlev kullanma

[ResourceGroup ()](template-functions-resource.md#resourcegroup) ve [Subscription ()](template-functions-resource.md#subscription) işlevleri, şablonu nasıl belirtkullanılacağına göre farklı şekilde çözümlenir. Bir dış şablona bağlandığınızda işlevler her zaman ilgili şablonun kapsamına çözümlenir. Bir şablonu bir üst şablon içine yuvalandığınızda, `expressionEvaluationOptions` işlevlerin ana şablon veya iç içe şablon için kaynak grubuna ve aboneliğe çözümlenip çözümlenmediğini belirtmek için özelliğini kullanın. Özelliğini, `inner` iç içe geçmiş şablonun kapsamına çözülecek şekilde ayarlayın. Özelliğini, `outer` üst şablonun kapsamına çözülecek şekilde ayarlayın.

Aşağıdaki tabloda, işlevlerin üst veya katıştırılmış kaynak grubuna ve aboneliğe çözümlenip çözümlenmediğini gösterir.

| Şablon türü | Kapsam | Çözüm |
| ------------- | ----- | ---------- |
| ble        | dış (varsayılan) | Üst kaynak grubu |
| ble        | Dahili | Alt kaynak grubu |
| bağlandı        | YOK   | Alt kaynak grubu |

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
