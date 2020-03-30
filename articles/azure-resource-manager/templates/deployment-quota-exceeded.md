---
title: Dağıtım kotası aşıldı
description: Kaynak grubu geçmişinde 800'den fazla dağıtım alabildiğini açıklayan hata.
ms.topic: troubleshooting
ms.date: 10/04/2019
ms.openlocfilehash: 919cd9a3482401cd47516e2677b0bf58387488b0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80245098"
---
# <a name="resolve-error-when-deployment-count-exceeds-800"></a>Dağıtım sayısı 800'ü aştığında hatayı giderme

Her kaynak grubu, dağıtım geçmişinde 800 dağıtımla sınırlıdır. Bu makalede, izin verilen 800 dağıtımı aşacağı için bir dağıtım başarısız olduğunda aldığınız hatayı açıklanır. Bu hatayı gidermek için, kaynak grubu geçmişinden dağıtımları silin. Bir dağıtımı geçmişten sildim, dağıtılan kaynakların hiçbirini etkilemez.

## <a name="symptom"></a>Belirti

Dağıtım sırasında, geçerli dağıtımın 800 dağıtım kotasını aşacağını belirten bir hata alırsınız.

## <a name="solution"></a>Çözüm

### <a name="azure-cli"></a>Azure CLI

Dağıtımları geçmişten silmek için [az dağıtım grubu silme](/cli/azure/group/deployment) komutunu kullanın.

```azurecli-interactive
az deployment group delete --resource-group exampleGroup --name deploymentName
```

Beş günden eski tüm dağıtımları silmek için şunları kullanın:

```azurecli-interactive
startdate=$(date +%F -d "-5days")
deployments=$(az deployment group list --resource-group exampleGroup --query "[?properties.timestamp<'$startdate'].name" --output tsv)

for deployment in $deployments
do
  az deployment group delete --resource-group exampleGroup --name $deployment
done
```

Dağıtım geçmişindeki geçerli sayıyı aşağıdaki komutla alabilirsiniz:

```azurecli-interactive
az deployment group list --resource-group exampleGroup --query "length(@)"
```

### <a name="azure-powershell"></a>Azure PowerShell

Dağıtımları geçmişten silmek için [Kaldır-AzKaynakGrubuDağıtım](/powershell/module/az.resources/remove-azresourcegroupdeployment) komutunu kullanın.

```azurepowershell-interactive
Remove-AzResourceGroupDeployment -ResourceGroupName exampleGroup -Name deploymentName
```

Beş günden eski tüm dağıtımları silmek için şunları kullanın:

```azurepowershell-interactive
$deployments = Get-AzResourceGroupDeployment -ResourceGroupName exampleGroup | Where-Object Timestamp -lt ((Get-Date).AddDays(-5))

foreach ($deployment in $deployments) {
  Remove-AzResourceGroupDeployment -ResourceGroupName exampleGroup -Name $deployment.DeploymentName
}
```

Dağıtım geçmişindeki geçerli sayıyı aşağıdaki komutla alabilirsiniz:

```azurepowershell-interactive
(Get-AzResourceGroupDeployment -ResourceGroupName exampleGroup).Count
```

## <a name="third-party-solutions"></a>Üçüncü taraf çözümleri

Aşağıdaki dış çözümler belirli senaryoları ele alatır:

* [Azure Logic Apps ve PowerShell çözümleri](https://devkimchi.com/2018/05/30/managing-excessive-arm-deployment-histories-with-logic-apps/)
* [Azdevops Uzantısı](https://github.com/christianwaha/AzureDevOpsExtensionCleanRG)
