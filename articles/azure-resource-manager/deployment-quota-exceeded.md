---
title: Azure dağıtım kotası aşıldı
description: Kaynak grubu geçmişinde 800 ' den fazla dağıtıma sahip olma hatasının nasıl çözümleneceğini açıklar.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: troubleshooting
ms.date: 10/02/2019
ms.author: tomfitz
ms.openlocfilehash: 755383c9d40c104d50ad9bb7a31b3a00f8348313
ms.sourcegitcommit: 7c2dba9bd9ef700b1ea4799260f0ad7ee919ff3b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71827010"
---
# <a name="resolve-error-when-deployment-count-exceeds-800"></a>Dağıtım sayısı 800 ' i aştığında hatayı çözümle

Her kaynak grubu, dağıtım geçmişinde 800 dağıtım ile sınırlıdır. Bu makalede, izin verilen 800 dağıtımlarını aşacağından bir dağıtım başarısız olduğunda aldığınız hata açıklanmaktadır. Bu hatayı çözmek için kaynak grubu geçmişinden dağıtımları silin. Bir dağıtımı geçmişten silmek, dağıtılan kaynakların hiçbirini etkilemez.

## <a name="symptom"></a>Belirti

Dağıtım sırasında, geçerli dağıtımın 800 dağıtım kotasını aşalacağını belirten bir hata alırsınız.

## <a name="solution"></a>Çözüm

### <a name="azure-cli"></a>Azure CLI

Geçmişten dağıtımları silmek için [az Group Deployment Delete](/cli/azure/group/deployment#az-group-deployment-delete) komutunu kullanın.

```azurecli-interactive
az group deployment delete --resource-group exampleGroup --name deploymentName
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

Aşağıdaki komutla dağıtım geçmişinde geçerli sayıyı edinebilirsiniz:

```azurepowershell-interactive
(Get-AzResourceGroupDeployment -ResourceGroupName exampleGroup).Count
```

## <a name="third-party-solutions"></a>Üçüncü taraf çözümler

Aşağıdaki dış çözümlere özel senaryolar ele verilmiştir:

* [Azure Logic Apps ve PowerShell çözümleri](https://devkimchi.com/2018/05/30/managing-excessive-arm-deployment-histories-with-logic-apps/)
* [AzDevOps uzantısı](https://github.com/christianwaha/AzureDevOpsExtensionCleanRG)
