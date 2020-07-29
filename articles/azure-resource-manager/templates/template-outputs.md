---
title: Şablonlarda çıkış çıkışları
description: Azure Resource Manager şablonunda çıkış değerlerinin nasıl tanımlanacağını açıklar.
ms.topic: conceptual
ms.date: 02/25/2020
ms.openlocfilehash: 203bfc66e9515ef14a5fe1315ef5b9ee07075041
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "79460033"
---
# <a name="outputs-in-azure-resource-manager-template"></a>Azure Resource Manager şablondaki çıktılar

Bu makalede, Azure Resource Manager şablonunuzda çıkış değerlerinin nasıl tanımlanacağı açıklanmaktadır. Dağıtılan kaynaklardan değer döndürihtiyacınız olduğunda çıktıları kullanırsınız.

## <a name="define-output-values"></a>Çıkış değerlerini tanımla

Aşağıdaki örnek, bir genel IP adresi için kaynak KIMLIĞININ nasıl dönegösterdiğini göstermektedir:

```json
"outputs": {
  "resourceID": {
    "type": "string",
    "value": "[resourceId('Microsoft.Network/publicIPAddresses', parameters('publicIPAddresses_name'))]"
  }
}
```

## <a name="conditional-output"></a>Koşullu çıkış

Çıktılar bölümünde, koşullu bir değer döndürebilirsiniz. Genellikle, bir kaynağı [koşullu olarak dağıttığınız](conditional-resource-deployment.md) durumunda çıkışların durumunu kullanırsınız. Aşağıdaki örnek, bir genel IP adresi için kaynak KIMLIĞININ, yeni bir birinin dağıtılıp dağıtıldığına göre nasıl koşullu olarak döndürülüp döndürülmeyeceğini göstermektedir:

```json
"outputs": {
  "resourceID": {
    "condition": "[equals(parameters('publicIpNewOrExisting'), 'new')]",
    "type": "string",
    "value": "[resourceId('Microsoft.Network/publicIPAddresses', parameters('publicIPAddresses_name'))]"
  }
}
```

Koşullu çıkışın basit bir örneği için bkz. [koşullu çıkış şablonu](https://github.com/bmoore-msft/AzureRM-Samples/blob/master/conditional-output/azuredeploy.json).

## <a name="dynamic-number-of-outputs"></a>Dinamik çıkış sayısı

Bazı senaryolarda, şablonu oluştururken döndürmeniz gereken bir değerin örnek sayısını bilemezsiniz. **Copy** öğesini kullanarak, değişken sayıda değer döndürebilirsiniz.

```json
"outputs": {
  "storageEndpoints": {
    "type": "array",
    "copy": {
      "count": "[parameters('storageCount')]",
      "input": "[reference(concat(copyIndex(), variables('baseName'))).primaryEndpoints.blob]"
    }
  }
}
```

Daha fazla bilgi için bkz. [Azure Resource Manager şablonlarda çıkış yinelemesi](copy-outputs.md).

## <a name="linked-templates"></a>Bağlı şablonlar

Bağlı bir şablondan çıkış değerini almak için, üst şablondaki [başvuru](template-functions-resource.md#reference) işlevini kullanın. Üst şablondaki sözdizimi şöyledir:

```json
"[reference('<deploymentName>').outputs.<propertyName>.value]"
```

Bağlantılı şablondan çıkış özelliği alınırken, özellik adı bir tire içeremez.

Aşağıdaki örnek, bir yük dengeleyicide, bağlantılı şablondan bir değer alarak IP adresinin nasıl ayarlanacağını gösterir.

```json
"publicIPAddress": {
  "id": "[reference('linkedTemplate').outputs.resourceID.value]"
}
```

`reference` [İç içe geçmiş bir şablonun](linked-templates.md#nested-template)çıktılar bölümünde işlevini kullanamazsınız. Dağıtılan bir kaynağın değerlerini iç içe yerleştirilmiş bir şablonda döndürmek için, iç içe geçmiş şablonunuzu bağlı bir şablona dönüştürün.

## <a name="get-output-values"></a>Çıkış değerlerini al

Dağıtım başarılı olduğunda, çıkış değerleri dağıtım sonuçlarında otomatik olarak döndürülür.

Dağıtım geçmişinden çıkış değerleri almak için, komut dosyası kullanabilirsiniz.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
(Get-AzResourceGroupDeployment `
  -ResourceGroupName <resource-group-name> `
  -Name <deployment-name>).Outputs.resourceID.value
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli-interactive
az deployment group show \
  -g <resource-group-name> \
  -n <deployment-name> \
  --query properties.outputs.resourceID.value
```

---

## <a name="example-templates"></a>Örnek Şablonlar

Aşağıdaki örneklerde, çıkışları kullanmaya yönelik senaryolar gösterilmektedir.

|Şablon  |Açıklama  |
|---------|---------|
|[Değişkenleri Kopyala](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/copyvariables.json) | Karmaşık değişkenler oluşturur ve bu değerleri verir. Hiçbir kaynak dağıtmaz. |
|[Genel IP adresi](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/linkedtemplates/public-ip.json) | Genel bir IP adresi oluşturur ve kaynak KIMLIĞINI verir. |
|[Yük dengeleyici](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/linkedtemplates/public-ip-parentloadbalancer.json) | Önceki şablonun bağlantıları. Yük dengeleyiciyi oluştururken çıktıda kaynak KIMLIĞINI kullanır. |

## <a name="next-steps"></a>Sonraki adımlar

* Çıktılar için kullanılabilen özellikler hakkında bilgi edinmek için bkz. [Azure Resource Manager şablonlarının yapısını ve sözdizimini anlayın](template-syntax.md).
