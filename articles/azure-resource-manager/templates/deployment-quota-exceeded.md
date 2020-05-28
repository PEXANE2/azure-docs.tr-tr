---
title: Dağıtım kotası aşıldı
description: Kaynak grubu geçmişinde 800 ' den fazla dağıtıma sahip olma hatasının nasıl çözümleneceğini açıklar.
ms.topic: troubleshooting
ms.date: 05/26/2020
ms.openlocfilehash: 0fec8fa91ec79822115d83b15e8c8a839dc7fd69
ms.sourcegitcommit: 6a9f01bbef4b442d474747773b2ae6ce7c428c1f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84117813"
---
# <a name="resolve-error-when-deployment-count-exceeds-800"></a>Dağıtım sayısı 800 ' i aştığında hatayı çözümle

Her kaynak grubu, dağıtım geçmişinde 800 dağıtım ile sınırlıdır. Bu makalede, izin verilen 800 dağıtımlarını aşacağından bir dağıtım başarısız olduğunda aldığınız hata açıklanmaktadır. Bu hatayı çözmek için kaynak grubu geçmişinden dağıtımları silin. Bir dağıtımı geçmişten silmek, dağıtılan kaynakların hiçbirini etkilemez.

> [!NOTE]
> 2020 Haziran 'dan itibaren, sınıra yaklaşmanıza kadar Azure Resource Manager otomatik olarak geçmişinizden dağıtımları siler. Otomatik silme işlemleri yapmadıysanız bu hatayı görmeye devam edebilirsiniz. Daha fazla bilgi için bkz. [dağıtım geçmişinden otomatik silme işlemleri](deployment-history-deletions.md).

## <a name="symptom"></a>Belirti

Dağıtım sırasında, geçerli dağıtımın 800 dağıtım kotasını aşalacağını belirten bir hata alırsınız.

## <a name="solution"></a>Çözüm

### <a name="azure-cli"></a>Azure CLI’si

Geçmişten dağıtımları silmek için [az Deployment Group Delete](/cli/azure/group/deployment) komutunu kullanın.

```azurecli-interactive
az deployment group delete --resource-group exampleGroup --name deploymentName
```

Beş günden eski olan tüm dağıtımları silmek için şunu kullanın:

```azurecli-interactive
startdate=$(date +%F -d "-5days")
deployments=$(az deployment group list --resource-group exampleGroup --query "[?properties.timestamp<'$startdate'].name" --output tsv)

for deployment in $deployments
do
  az deployment group delete --resource-group exampleGroup --name $deployment
done
```

Aşağıdaki komutla dağıtım geçmişinde geçerli sayıyı edinebilirsiniz:

```azurecli-interactive
az deployment group list --resource-group exampleGroup --query "length(@)"
```

### <a name="azure-powershell"></a>Azure PowerShell

Geçmişten dağıtımları silmek için [Remove-AzResourceGroupDeployment](/powershell/module/az.resources/remove-azresourcegroupdeployment) komutunu kullanın.

```azurepowershell-interactive
Remove-AzResourceGroupDeployment -ResourceGroupName exampleGroup -Name deploymentName
```

Beş günden eski olan tüm dağıtımları silmek için şunu kullanın:

```azurepowershell-interactive
$deployments = Get-AzResourceGroupDeployment -ResourceGroupName exampleGroup | Where-Object Timestamp -lt ((Get-Date).AddDays(-5))

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
