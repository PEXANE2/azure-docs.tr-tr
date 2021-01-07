---
title: Şablonlarda çıkış çıkışları
description: Azure Resource Manager şablonunda çıkış değerlerinin nasıl tanımlanacağını açıklar (ARM şablonu).
ms.topic: conceptual
ms.date: 11/24/2020
ms.openlocfilehash: f8f13b6caf063cea79dc71775fb936f406a3ee6c
ms.sourcegitcommit: f6f928180504444470af713c32e7df667c17ac20
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/07/2021
ms.locfileid: "97964023"
---
# <a name="outputs-in-arm-templates"></a>ARM şablonlarındaki çıktılar

Bu makalede, Azure Resource Manager şablonunuzda (ARM şablonu) çıkış değerlerinin nasıl tanımlanacağı açıklanmaktadır. `outputs`Dağıtılan kaynaklardan değer döndürihtiyacınız olduğunda kullanırsınız.

Her bir çıkış değerinin biçimi, [veri türlerinden](template-syntax.md#data-types)biriyle aynı olmalıdır.

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

`outputs`Bölümünde, koşullu bir değer döndürebilirsiniz. Genellikle, ' `condition` de `outputs` bir kaynağı [koşullu olarak dağıttığınıza](conditional-resource-deployment.md) kullanırsınız. Aşağıdaki örnek, bir genel IP adresi için kaynak KIMLIĞININ, yeni bir birinin dağıtılıp dağıtıldığına göre nasıl koşullu olarak döndürülüp döndürülmeyeceğini göstermektedir:

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

Bazı senaryolarda, şablonu oluştururken döndürmeniz gereken bir değerin örnek sayısını bilemezsiniz. Öğesini kullanarak değişken sayıda değer döndürebilirsiniz `copy` .

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

Daha fazla bilgi için bkz. [ARM şablonlarındaki çıkış yinelemesi](copy-outputs.md).

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

* Çıktılar için kullanılabilen özellikler hakkında bilgi edinmek için bkz. [ARM şablonlarının yapısını ve sözdizimini anlayın](template-syntax.md).
