---
title: Başarılı dağıtım için hatayı geri alma
description: Başarısız bir dağıtımın başarılı bir dağıtıma geri dönmesi gerektiğini belirtin.
ms.topic: conceptual
ms.date: 10/04/2019
ms.openlocfilehash: 206c794996f58a4c5b6982c551ae50128ed4f5eb
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "79460152"
---
# <a name="rollback-on-error-to-successful-deployment"></a>Başarılı dağıtımda hata durumunda geri al

Bir dağıtım başarısız olduğunda, dağıtım geçmişinizden daha önceki ve başarılı bir dağıtımı otomatik olarak yeniden dağıtabilirsiniz. Bu işlevsellik, altyapı dağıtımınız için bilinen iyi bir durumanız varsa ve bu duruma dönmek istiyorsanız yararlıdır. Çeşitli uyarılar ve kısıtlamalar vardır:

- Yeniden dağıtım, daha önce aynı parametrelerle çalıştırıldığı için tam olarak çalıştırılır. Parametreleri değiştiremezsiniz.
- Önceki dağıtım, [Tüm modu](./deployment-modes.md#complete-mode)kullanılarak çalıştırılır. Önceki dağıtıma dahil olmayan tüm kaynaklar silinir ve tüm kaynak konfigürasyonları önceki durumlarına ayarlanır. [Dağıtım modlarını](./deployment-modes.md)tam olarak anladığınızdan emin olun.
- Yeniden dağıtım yalnızca kaynakları etkiler, tüm veri değişiklikleri etkilenmez.
- Bu özelliği yalnızca kaynak grubu dağıtımları ile, abonelik veya yönetim grubu düzeyi dağıtımlarıyla birlikte kullanabilirsiniz. Abonelik düzeyi dağıtımı hakkında daha fazla bilgi için bkz. [abonelik düzeyinde kaynak grupları ve kaynaklar oluşturma](./deploy-to-subscription.md).
- Bu seçeneği yalnızca kök düzeyinde dağıtımlar ile kullanabilirsiniz. İç içe geçmiş bir şablondan dağıtımlar yeniden dağıtım için kullanılamaz.

Bu seçeneği kullanmak için, dağıtımlarınızın geçmişte tanımlanabilmeleri için benzersiz adlara sahip olması gerekir. Benzersiz adlarınız yoksa geçerli başarısız dağıtım, geçmişte daha önce başarılı olan dağıtımın üzerine yazabilir.

## <a name="powershell"></a>PowerShell

Son başarılı dağıtımı yeniden dağıtmak için `-RollbackToLastDeployment` parametreyi bir bayrak olarak ekleyin.

```azurepowershell-interactive
New-AzResourceGroupDeployment -Name ExampleDeployment02 `
  -ResourceGroupName $resourceGroupName `
  -TemplateFile c:\MyTemplates\azuredeploy.json `
  -RollbackToLastDeployment
```

Belirli bir dağıtımı yeniden dağıtmak için parametresini kullanın `-RollBackDeploymentName` ve dağıtımın adını sağlayın. Belirtilen dağıtım başarılı olmalıdır.

```azurepowershell-interactive
New-AzResourceGroupDeployment -Name ExampleDeployment02 `
  -ResourceGroupName $resourceGroupName `
  -TemplateFile c:\MyTemplates\azuredeploy.json `
  -RollBackDeploymentName ExampleDeployment01
```

## <a name="azure-cli"></a>Azure CLI

Son başarılı dağıtımı yeniden dağıtmak için `--rollback-on-error` parametreyi bir bayrak olarak ekleyin.

```azurecli-interactive
az deployment group create \
  --name ExampleDeployment \
  --resource-group ExampleGroup \
  --template-file storage.json \
  --parameters storageAccountType=Standard_GRS \
  --rollback-on-error
```

Belirli bir dağıtımı yeniden dağıtmak için parametresini kullanın `--rollback-on-error` ve dağıtımın adını sağlayın. Belirtilen dağıtım başarılı olmalıdır.

```azurecli-interactive
az deployment group create \
  --name ExampleDeployment02 \
  --resource-group ExampleGroup \
  --template-file storage.json \
  --parameters storageAccountType=Standard_GRS \
  --rollback-on-error ExampleDeployment01
```

## <a name="rest-api"></a>REST API

Geçerli dağıtım başarısız olursa son başarılı dağıtımı yeniden dağıtmak için şunu kullanın:

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

Geçerli dağıtım başarısız olursa belirli bir dağıtımı yeniden dağıtmak için şunu kullanın:

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

Belirtilen dağıtım başarılı olmalıdır.

## <a name="next-steps"></a>Sonraki adımlar

- Hizmetinizi birden fazla bölgeye güvenle kullanıma almak için bkz. [Azure dağıtım Yöneticisi](deployment-manager-overview.md).
- Kaynak grubunda var olan, ancak şablonda tanımlanmamış kaynakların nasıl işleneceğini belirtmek için bkz. [Azure Resource Manager Dağıtım modları](deployment-modes.md).
- Şablonunuzda parametrelerin nasıl tanımlanacağını anlamak için bkz. [Azure Resource Manager şablonlarının yapısını ve sözdizimini anlayın](template-syntax.md).
- SAS belirteci gerektiren bir şablonu dağıtma hakkında daha fazla bilgi için bkz. [özel şablonu SAS belirteci Ile dağıtma](secure-template-with-sas-token.md).
