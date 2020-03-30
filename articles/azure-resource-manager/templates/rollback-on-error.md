---
title: Başarılı dağıtım için hataya geri dön
description: Başarısız bir dağıtımın başarılı bir dağıtıma geri döneceğini belirtin.
ms.topic: conceptual
ms.date: 10/04/2019
ms.openlocfilehash: 206c794996f58a4c5b6982c551ae50128ed4f5eb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79460152"
---
# <a name="rollback-on-error-to-successful-deployment"></a>Başarılı dağıtım hatası üzerine geri alma

Bir dağıtım başarısız olduğunda, dağıtım geçmişinizden daha önceki ve başarılı bir dağıtımı otomatik olarak yeniden dağıtabilirsiniz. Bu işlevsellik, altyapı dağıtımınız için iyi bir durumunuz varsa ve bu duruma geri dönmek istiyorsanız yararlıdır. Uyarılar ve kısıtlamalar vardır:

- Yeniden dağıtım tam olarak aynı parametrelerle daha önce çalıştırıldığı gibi çalıştırılır. Parametreleri değiştiremezsin.
- Önceki dağıtım [tam modu](./deployment-modes.md#complete-mode)kullanılarak çalıştırılır. Önceki dağıtıma dahil olmayan tüm kaynaklar silinir ve tüm kaynak yapılandırmaları önceki durumlarına ayarlanır. [Dağıtım modlarını](./deployment-modes.md)tam olarak anladığınızdan emin olun.
- Yeniden dağıtım yalnızca kaynakları etkiler, veri değişiklikleri etkilenmez.
- Bu özelliği yalnızca kaynak grubu dağıtımlarıyla kullanabilirsiniz, abonelik veya yönetim grubu düzeyindeki dağıtımlarla değil. Abonelik düzeyi dağıtımı hakkında daha fazla bilgi için [bkz.](./deploy-to-subscription.md)
- Bu seçeneği yalnızca kök düzeyi dağıtımlarında kullanabilirsiniz. İç içe bir şablondaki dağıtımlar yeniden dağıtım için kullanılamaz.

Bu seçeneği kullanmak için, dağıtımlarınızın geçmişte tanımlanabilmesi için benzersiz adlara sahip olması gerekir. Benzersiz adlarınız yoksa, geçerli başarısız dağıtım, geçmişte daha önce başarılı olan dağıtımın üzerine yazabilir.

## <a name="powershell"></a>PowerShell

Son başarılı dağıtımı yeniden dağıtmak `-RollbackToLastDeployment` için parametreyi bayrak olarak ekleyin.

```azurepowershell-interactive
New-AzResourceGroupDeployment -Name ExampleDeployment02 `
  -ResourceGroupName $resourceGroupName `
  -TemplateFile c:\MyTemplates\azuredeploy.json `
  -RollbackToLastDeployment
```

Belirli bir dağıtımı yeniden dağıtmak `-RollBackDeploymentName` için parametreyi kullanın ve dağıtımın adını sağlayın. Belirtilen dağıtım başarılı olmuş olmalıdır.

```azurepowershell-interactive
New-AzResourceGroupDeployment -Name ExampleDeployment02 `
  -ResourceGroupName $resourceGroupName `
  -TemplateFile c:\MyTemplates\azuredeploy.json `
  -RollBackDeploymentName ExampleDeployment01
```

## <a name="azure-cli"></a>Azure CLI

Son başarılı dağıtımı yeniden dağıtmak `--rollback-on-error` için parametreyi bayrak olarak ekleyin.

```azurecli-interactive
az deployment group create \
  --name ExampleDeployment \
  --resource-group ExampleGroup \
  --template-file storage.json \
  --parameters storageAccountType=Standard_GRS \
  --rollback-on-error
```

Belirli bir dağıtımı yeniden dağıtmak `--rollback-on-error` için parametreyi kullanın ve dağıtımın adını sağlayın. Belirtilen dağıtım başarılı olmuş olmalıdır.

```azurecli-interactive
az deployment group create \
  --name ExampleDeployment02 \
  --resource-group ExampleGroup \
  --template-file storage.json \
  --parameters storageAccountType=Standard_GRS \
  --rollback-on-error ExampleDeployment01
```

## <a name="rest-api"></a>REST API

Geçerli dağıtım başarısız olursa, son başarılı dağıtımı yeniden dağıtmak için aşağıdakileri kullanın:

```json
{
  "properties": {
    "templateLink": {
      "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
      "contentVersion": "1.0.0.0"
    },
    "mode": "Incremental",
    "parametersLink": {
      "uri": "http://mystorageaccount.blob.core.windows.net/templates/parameters.json",
      "contentVersion": "1.0.0.0"
    },
    "onErrorDeployment": {
      "type": "LastSuccessful",
    }
  }
}
```

Geçerli dağıtım başarısız olursa belirli bir dağıtımı yeniden dağıtmak için şunları kullanın:

```json
{
  "properties": {
    "templateLink": {
      "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
      "contentVersion": "1.0.0.0"
    },
    "mode": "Incremental",
    "parametersLink": {
      "uri": "http://mystorageaccount.blob.core.windows.net/templates/parameters.json",
      "contentVersion": "1.0.0.0"
    },
    "onErrorDeployment": {
      "type": "SpecificDeployment",
      "deploymentName": "<deploymentname>"
    }
  }
}
```

Belirtilen dağıtım başarılı olmuş olmalıdır.

## <a name="next-steps"></a>Sonraki adımlar

- Hizmetinizi birden fazla bölgeye güvenli bir şekilde kullanıma sunmak için [Azure Dağıtım Yöneticisi'ne](deployment-manager-overview.md)bakın.
- Kaynak grubunda bulunan ancak şablonda tanımlanmayan kaynakların nasıl işleyeceğini belirtmek için Azure [Kaynak Yöneticisi dağıtım modlarına](deployment-modes.md)bakın.
- Şablonunuzdaparametrelerin nasıl tanımlandığını anlamak için [bkz.](template-syntax.md)
- SAS belirteci gerektiren bir şablonu dağıtma hakkında bilgi [için](secure-template-with-sas-token.md)bkz.
