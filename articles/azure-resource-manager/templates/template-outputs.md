---
title: Şablonlarda çıktılar
description: Azure Kaynak Yöneticisi şablonundaki çıktı değerlerinin nasıl tanımlandığını açıklar.
ms.topic: conceptual
ms.date: 02/25/2020
ms.openlocfilehash: 203bfc66e9515ef14a5fe1315ef5b9ee07075041
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79460033"
---
# <a name="outputs-in-azure-resource-manager-template"></a>Azure Kaynak Yöneticisi şablonundaki çıktılar

Bu makalede, Azure Kaynak Yöneticisi şablonunuzda çıktı değerlerinin nasıl tanımlanış olduğu açıklanmaktadır. Dağıtılan kaynaklardan değerleri döndürmeniz gerektiğinde çıktıları kullanırsınız.

## <a name="define-output-values"></a>Çıktı değerlerini tanımlama

Aşağıdaki örnek, ortak bir IP adresi için kaynak kimliğinin nasıl döndürülecek olduğunu gösterir:

```json
"outputs": {
  "resourceID": {
    "type": "string",
    "value": "[resourceId('Microsoft.Network/publicIPAddresses', parameters('publicIPAddresses_name'))]"
  }
}
```

## <a name="conditional-output"></a>Koşullu çıktı

Çıktılar bölümünde, koşullu bir değer döndürebilirsiniz. Genellikle, bir kaynağı [koşullu olarak dağıttığınızda](conditional-resource-deployment.md) çıktılardaki durumu kullanırsınız. Aşağıdaki örnek, yeni bir ip adresinin dağıtılıp dağıtılmadığına bağlı olarak ortak bir IP adresi için kaynak kimliğinin koşullu olarak nasıl döndürülebildiğini gösterir:

```json
"outputs": {
  "resourceID": {
    "condition": "[equals(parameters('publicIpNewOrExisting'), 'new')]",
    "type": "string",
    "value": "[resourceId('Microsoft.Network/publicIPAddresses', parameters('publicIPAddresses_name'))]"
  }
}
```

Koşullu çıktının basit bir örneği için [koşullu çıktı şablonuna](https://github.com/bmoore-msft/AzureRM-Samples/blob/master/conditional-output/azuredeploy.json)bakın.

## <a name="dynamic-number-of-outputs"></a>Dinamik çıktı sayısı

Bazı senaryolarda, şablonu oluştururken döndürmeniz gereken bir değerin örnek sayısını bilemezsin. **Kopyalama** öğesini kullanarak değişken sayıda değer döndürebilirsiniz.

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

Daha fazla bilgi için Azure [Kaynak Yöneticisi şablonlarında Çıktı yinelemesi'ne](copy-outputs.md)bakın.

## <a name="linked-templates"></a>Bağlı şablonlar

Bağlı bir şablondan çıktı değerini almak için üst şablondaki [başvuru](template-functions-resource.md#reference) işlevini kullanın. Üst şablondaki sözdizimi:

```json
"[reference('<deploymentName>').outputs.<propertyName>.value]"
```

Bağlı bir şablondan çıktı özelliği alırken, özellik adı tire içeremez.

Aşağıdaki örnek, bağlı bir şablondan bir değer alarak IP adresinin bir yük dengeleyicisi üzerinde nasıl ayarlanıngerektiğini gösterir.

```json
"publicIPAddress": {
  "id": "[reference('linkedTemplate').outputs.resourceID.value]"
}
```

İç içe bir `reference` [şablonun](linked-templates.md#nested-template)çıktılar bölümündeki işlevi kullanamazsınız. İç içe geçmiş bir şablonda dağıtılmış bir kaynağın değerlerini döndürmek için iç içe geçmiş şablonunuzu bağlantılı bir şablona dönüştürün.

## <a name="get-output-values"></a>Çıktı değerlerini alma

Dağıtım başarılı olduğunda, çıktı değerleri dağıtım sonuçlarında otomatik olarak döndürülür.

Dağıtım geçmişinden çıktı değerlerini almak için komut dosyasını kullanabilirsiniz.

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)

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

## <a name="example-templates"></a>Örnek şablonlar

Aşağıdaki örnekler, çıktıları kullanma senaryolarını gösterir.

|Şablon  |Açıklama  |
|---------|---------|
|[Kopya değişkenleri](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/copyvariables.json) | Karmaşık değişkenler oluşturur ve bu değerleri çıkar. Herhangi bir kaynak dağıtmaz. |
|[Genel IP adresi](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/linkedtemplates/public-ip.json) | Ortak bir IP adresi oluşturur ve kaynak kimliğini çıkartır. |
|[Yük dengeleyici](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/linkedtemplates/public-ip-parentloadbalancer.json) | Önceki şablona bağlantılar. Yük dengeleyicisini oluştururken çıktıdaki kaynak kimliğini kullanır. |

## <a name="next-steps"></a>Sonraki adımlar

* Çıktılar için kullanılabilir özellikler hakkında bilgi edinmek için [bkz.](template-syntax.md)
