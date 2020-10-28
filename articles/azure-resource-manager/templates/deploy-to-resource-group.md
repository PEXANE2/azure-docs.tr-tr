---
title: Kaynakları kaynak gruplarına dağıtma
description: Azure Resource Manager şablonunda kaynakların nasıl dağıtılacağını açıklar. Birden fazla kaynak grubunun nasıl hedeflenecek gösterilmektedir.
ms.topic: conceptual
ms.date: 10/26/2020
ms.openlocfilehash: fd211641d7fcc02a1db154053597497583b21ae5
ms.sourcegitcommit: 4cb89d880be26a2a4531fedcc59317471fe729cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2020
ms.locfileid: "92681672"
---
# <a name="resource-group-deployments-with-arm-templates"></a>ARM şablonlarıyla kaynak grubu dağıtımları

Bu makalede, dağıtımınızın bir kaynak grubuna nasıl kapsam yapılacağı açıklanır. Dağıtım için bir Azure Resource Manager şablonu (ARM şablonu) kullanırsınız. Makalede ayrıca kapsam, dağıtım işlemindeki kaynak grubunun ötesinde nasıl genişlebileceğiniz gösterilmektedir.

## <a name="supported-resources"></a>Desteklenen kaynaklar

Kaynakların çoğu, bir kaynak grubuna dağıtılabilir. Kullanılabilir kaynakların listesi için bkz. [ARM şablon başvurusu](/azure/templates).

## <a name="schema"></a>Şema

Şablonlar için aşağıdaki şemayı kullanın:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    ...
}
```

Parametre dosyaları için şunu kullanın:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentParameters.json#",
    ...
}
```

## <a name="deployment-commands"></a>Dağıtım komutları

Bir kaynak grubuna dağıtmak için, kaynak grubu dağıtım komutlarını kullanın.

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Azure CLı için [az Deployment Group Create](/cli/azure/deployment/group#az_deployment_group_create)kullanın. Aşağıdaki örnek, bir kaynak grubu oluşturmak için bir şablon dağıtır:

```azurecli-interactive
az deployment group create \
  --name demoRGDeployment \
  --resource-group ExampleGroup \
  --template-uri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json" \
  --parameters storageAccountType=Standard_GRS
```

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

PowerShell dağıtım komutu için [New-AzResourceGroupDeployment](/powershell/module/az.resources/new-azresourcegroupdeployment)' ı kullanın. Aşağıdaki örnek, bir kaynak grubu oluşturmak için bir şablon dağıtır:

```azurepowershell-interactive
New-AzResourceGroupDeployment `
  -Name demoRGDeployment `
  -ResourceGroupName ExampleGroup `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json `
  -storageAccountType Standard_GRS `
```

---

ARM şablonları dağıtmaya yönelik dağıtım komutları ve seçenekleri hakkında daha ayrıntılı bilgi için bkz.:

* [ARM şablonları ve Azure portal kaynak dağıtma](deploy-portal.md)
* [ARM şablonları ve Azure CLı ile kaynak dağıtma](deploy-cli.md)
* [ARM şablonları ve Azure PowerShell kaynak dağıtma](deploy-powershell.md)
* [ARM şablonlarıyla kaynakları dağıtma ve Azure Resource Manager REST API](deploy-rest.md)
* [GitHub deposundan şablon dağıtmak için bir dağıtım düğmesi kullanın](deploy-to-azure-button.md)
* [ARM şablonlarını Cloud Shell dağıtma](deploy-cloud-shell.md)

## <a name="deployment-scopes"></a>Dağıtım kapsamları

Bir kaynak grubuna dağıtırken, kaynakların dağıtımını yapabilirsiniz:

* işlemin hedef kaynak grubu
* aynı abonelikte veya diğer aboneliklerdeki diğer kaynak grupları
* [uzantı kaynakları](scope-extension-resources.md) , kaynaklara uygulanabilir

Şablonu dağıtan kullanıcının belirtilen kapsama erişimi olmalıdır.

Bu bölümde, farklı kapsamların nasıl ayarlanacağı gösterilmektedir. Bu farklı kapsamları tek bir şablonda birleştirebilirsiniz.

### <a name="scope-to-target-resource-group"></a>Hedef kaynak grubu için kapsam

Hedef kaynağa kaynak dağıtmak için bu kaynakları şablonun kaynaklar bölümüne ekleyin.

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/scope/default-rg.json" highlight="5":::

### <a name="scope-to-resource-group-in-same-subscription"></a>Aynı abonelikte kaynak grubu için kapsam

Aynı abonelikte farklı bir kaynak grubuna kaynak dağıtmak için, iç içe geçmiş bir dağıtım ekleyin ve `resourceGroup` özelliğini ekleyin. Abonelik KIMLIĞINI veya kaynak grubunu belirtmezseniz, üst şablondaki abonelik ve kaynak grubu kullanılır. Dağıtımı çalıştırmadan önce tüm kaynak gruplarının mevcut olması gerekir.

Aşağıdaki örnekte, iç içe dağıtım adlı bir kaynak grubunu hedefler `demoResourceGroup` .

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/scope/same-sub-to-resource-group.json" highlight="9,13":::

### <a name="scope-to-resource-group-in-different-subscription"></a>Farklı abonelikteki kaynak grubuna kapsam

Kaynakları farklı bir abonelikte kaynak grubuna dağıtmak için, iç içe geçmiş bir dağıtım ekleyin ve `subscriptionId` ve özelliklerini dahil edin `resourceGroup` . Aşağıdaki örnekte, iç içe dağıtım adlı bir kaynak grubunu hedefler `demoResourceGroup` .

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/scope/different-sub-to-resource-group.json" highlight="9,10,14":::

## <a name="cross-resource-groups"></a>Çapraz kaynak grupları

Tek bir ARM şablonunda birden fazla kaynak grubuna dağıtım yapabilirsiniz. Üst şablon için olandan farklı bir kaynak grubunu hedeflemek için, [iç içe veya bağlı bir şablon](linked-templates.md)kullanın. Dağıtım kaynak türü içinde, iç içe şablonun dağıtılmasını istediğiniz abonelik KIMLIĞI ve kaynak grubu için değerleri belirtin. Kaynak grupları farklı aboneliklerde bulunabilir.

> [!NOTE]
> Tek bir dağıtımda **800 kaynak grubuna** dağıtım yapabilirsiniz. Genellikle, bu sınırlama, üst şablon için belirtilen bir kaynak grubuna ve iç içe veya bağlı dağıtımlardaki 799 ' e kadar kaynak grubuna dağıtabileceğiniz anlamına gelir. Ancak, ana şablonunuz yalnızca iç içe veya bağlı şablonlar içeriyorsa ve herhangi bir kaynak dağıtmadığından, iç içe veya bağlı dağıtımlara en fazla 800 kaynak grubu ekleyebilirsiniz.

Aşağıdaki örnek iki depolama hesabı dağıtır. İlk depolama hesabı, dağıtım işleminde belirtilen kaynak grubuna dağıtılır. İkinci depolama hesabı, ve parametrelerinde belirtilen kaynak grubuna dağıtılır `secondResourceGroup` `secondSubscriptionID` :

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/crosssubscription.json":::

`resourceGroup`Mevcut olmayan bir kaynak grubunun adına ayarlarsanız, dağıtım başarısız olur.

Önceki şablonu test etmek ve sonuçları görmek için PowerShell veya Azure CLı kullanın.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

İki depolama hesabını **aynı abonelikte** iki kaynak grubuna dağıtmak için şunu kullanın:

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

İki **aboneliğe** iki depolama hesabı dağıtmak için şunu kullanın:

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

İki depolama hesabını **aynı abonelikte** iki kaynak grubuna dağıtmak için şunu kullanın:

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

İki **aboneliğe** iki depolama hesabı dağıtmak için şunu kullanın:

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

## <a name="next-steps"></a>Sonraki adımlar

* Azure Güvenlik Merkezi için çalışma alanı ayarlarını dağıtmaya ilişkin bir örnek için bkz. [deployASCwithWorkspaceSettings.json](https://github.com/krnese/AzureDeploy/blob/master/ARM/deployments/deployASCwithWorkspaceSettings.json).
