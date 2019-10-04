---
title: Azure dağıtım kotası aşıldı
description: Kaynak grubu geçmişinde 800 ' den fazla dağıtıma sahip olma hatasının nasıl çözümleneceğini açıklar.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: troubleshooting
ms.date: 10/04/2019
ms.author: tomfitz
ms.openlocfilehash: 5bbb686597850aaceff3d3b5c142b0cb1fb0eefd
ms.sourcegitcommit: 4d177e6d273bba8af03a00e8bb9fe51a447196d0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/04/2019
ms.locfileid: "71959630"
---
# <a name="resolve-error-when-deployment-count-exceeds-800"></a>Dağıtım sayısı 800 ' i aştığında hatayı çözümle

Her kaynak grubu, dağıtım geçmişinde 800 dağıtım ile sınırlıdır. Bu makalede, izin verilen 800 dağıtımlarını aşacağından bir dağıtım başarısız olduğunda aldığınız hata açıklanmaktadır. Bu hatayı çözmek için kaynak grubu geçmişinden dağıtımları silin. Bir dağıtımı geçmişten silmek, dağıtılan kaynakların hiçbirini etkilemez.

## <a name="symptom"></a>Belirti

Dağıtım sırasında, geçerli dağıtımın 800 dağıtım kotasını aşalacağını belirten bir hata alırsınız.

## <a name="solution"></a>Çözüm

### <a name="azure-cli"></a>Azure CLı

Geçmişten dağıtımları silmek için [az Group Deployment Delete](/cli/azure/group/deployment#az-group-deployment-delete) komutunu kullanın.

```azurecli-interactive
az group deployment delete --resource-group exampleGroup --name deploymentName
```

Beş günden eski olan tüm dağıtımları silmek için şunu kullanın:

```azurecli-interactive
startdate=$(date +%F -d "-5days")
deployments=$(az group deployment list --resource-group exampleGroup --query "[?properties.timestamp>'$startdate'].name" --output tsv)

for deployment in $deployments
do
  az group deployment delete --resource-group exampleGroup --name $deployment
done
```

Aşağıdaki komutla dağıtım geçmişinde geçerli sayıyı edinebilirsiniz:

```azurecli-interactive
az group deployment list --resource-group exampleGroup --query "length(@)"
```

### <a name="azure-powershell"></a>Azure PowerShell

Geçmişten dağıtımları silmek için [Remove-AzResourceGroupDeployment](/powershell/module/az.resources/remove-azresourcegroupdeployment) komutunu kullanın.

```azurepowershell-interactive
Remove-AzResourceGroupDeployment -ResourceGroupName exampleGroup -Name deploymentName
```

Beş günden eski olan tüm dağıtımları silmek için şunu kullanın:

```azurepowershell-interactive
$deployments = Get-AzResourceGroupDeployment -ResourceGroupName exampleGroup | Where-Object Timestamp -gt ((Get-Date).AddDays(-5))

foreach ($deployment in $deployments) {
  Remove-AzResourceGroupDeployment -ResourceGroupName exampleGroup -Name $deployment.DeploymentName
}
```

Aşağıdaki komutla dağıtım geçmişinde geçerli sayıyı edinebilirsiniz:

```azurepowershell-interactive
(Get-AzResourceGroupDeployment -ResourceGroupName exampleGroup).Count
```

## <a name="third-party-solutions"></a>Üçüncü taraf çözümler

Aşağıdaki dış çözümlere özel senaryolar ele verilmiştir:

* [Azure Logic Apps ve PowerShell çözümleri](https://devkimchi.com/2018/05/30/managing-excessive-arm-deployment-histories-with-logic-apps/)
* [AzDevOps uzantısı](https://github.com/christianwaha/AzureDevOpsExtensionCleanRG)
