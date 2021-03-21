---
title: Başarılı dağıtım için hatayı geri alma
description: Başarısız bir dağıtımın başarılı bir dağıtıma geri dönmesi gerektiğini belirtin.
ms.topic: conceptual
ms.date: 02/02/2021
ms.openlocfilehash: 742a8f16a2dce3204b48085759091540586a4522
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "99492221"
---
# <a name="rollback-on-error-to-successful-deployment"></a>Başarılı dağıtımda hata durumunda geri al

Bir dağıtım başarısız olduğunda, dağıtım geçmişinizden daha önceki ve başarılı bir dağıtımı otomatik olarak yeniden dağıtabilirsiniz. Bu işlevsellik, altyapı dağıtımınız için bilinen iyi bir durumanız varsa ve bu duruma dönmek istiyorsanız yararlıdır. Belirli bir önceki dağıtımı veya son başarılı dağıtımı belirtebilirsiniz.

> [!IMPORTANT]
> Bu özellik önceki bir dağıtımı yeniden dağıtarak başarısız bir dağıtımı geri yedekler. Bu sonuç, başarısız dağıtımı geri alma işlemini beklediğiniz miktardan farklı olabilir. Önceki dağıtımın yeniden nasıl dağıtıldığını anladığınızdan emin olun.

## <a name="considerations-for-redeploying"></a>Yeniden dağıtım konuları

Bu özelliği kullanmadan önce, yeniden dağıtım işlemlerinin nasıl işlendiği hakkındaki ayrıntıları göz önünde bulundurun:

- Önceki dağıtım sırasında [artımlı modu](./deployment-modes.md#incremental-mode) kullanmış olsanız bile, önceki dağıtım, [Tüm modu](./deployment-modes.md#complete-mode)kullanılarak çalıştırılır. Daha önceki dağıtım artımlı olarak kullanıldığında, tamamlanmış modda yeniden dağıtım beklenmedik sonuçlara neden olabilir. Tamamlanma modu, önceki dağıtımda bulunmayan tüm kaynakların silindiği anlamına gelir. Kaynak grubunda mevcut olmasını istediğiniz tüm kaynakları ve durumlarını temsil eden önceki bir dağıtım belirtin. Daha fazla bilgi için bkz. [Dağıtım modları](./deployment-modes.md).
- Yeniden dağıtım, daha önce aynı parametrelerle çalıştırıldığı için tam olarak çalıştırılır. Parametreleri değiştiremezsiniz.
- Yeniden dağıtım yalnızca kaynakları etkiler, tüm veri değişiklikleri etkilenmez.
- Bu özelliği yalnızca kaynak grubu dağıtımları ile kullanabilirsiniz. Abonelik, yönetim grubu veya kiracı düzeyi dağıtımlarını desteklemez. Abonelik düzeyi dağıtımı hakkında daha fazla bilgi için bkz. [abonelik düzeyinde kaynak grupları ve kaynaklar oluşturma](./deploy-to-subscription.md).
- Bu seçeneği yalnızca kök düzeyinde dağıtımlar ile kullanabilirsiniz. İç içe geçmiş bir şablondan dağıtımlar yeniden dağıtım için kullanılamaz.

Bu seçeneği kullanmak için, dağıtımlarınızın dağıtım geçmişinde benzersiz adlara sahip olması gerekir. Yalnızca belirli bir dağıtımın tanımlanabilecek benzersiz adlara sahiptir. Benzersiz adlarınız yoksa, başarısız bir dağıtım, geçmişteki bir dağıtımın üzerine yazabilir.

Dağıtım geçmişinde mevcut olmayan önceki bir dağıtımı belirtirseniz geri alma işlemi bir hata döndürür.

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

## <a name="azure-cli"></a>Azure CLI’si

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

- Tüm ve artımlı modlarını anlamak için bkz. [Azure Resource Manager Dağıtım modları](deployment-modes.md).
- Şablonunuzda parametrelerin nasıl tanımlanacağını anlamak için bkz. [Azure Resource Manager şablonlarının yapısını ve sözdizimini anlayın](template-syntax.md).
