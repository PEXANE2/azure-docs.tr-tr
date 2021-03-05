---
title: Şablonlarda çıkış çıkışları
description: Azure Resource Manager şablonunda (ARM şablonu) ve Bıcep dosyasında çıkış değerlerinin nasıl tanımlanacağını açıklar.
ms.topic: conceptual
ms.date: 02/19/2021
ms.openlocfilehash: 2b6a6afa127bf43102103baadae576233843f00d
ms.sourcegitcommit: dac05f662ac353c1c7c5294399fca2a99b4f89c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2021
ms.locfileid: "102123420"
---
# <a name="outputs-in-arm-templates"></a>ARM şablonlarındaki çıktılar

Bu makalede, Azure Resource Manager şablonunuzda (ARM şablonunda) ve Bıcep dosyasında çıkış değerlerinin nasıl tanımlanacağı açıklanmaktadır. Dağıtılan kaynaklardan değer döndürihtiyacınız olduğunda çıktıları kullanırsınız.

Her çıkış değerinin biçimi, [veri türlerinden](data-types.md)birine çözümlenmelidir.

[!INCLUDE [Bicep preview](../../../includes/resource-manager-bicep-preview.md)]

## <a name="define-output-values"></a>Çıkış değerlerini tanımla

Aşağıdaki örnek, dağıtılan bir kaynaktan bir özelliğin nasıl dönegösterdiğini gösterir.

# <a name="json"></a>[JSON](#tab/json)

JSON için, `outputs` bölümü şablona ekleyin. Çıkış değeri, genel bir IP adresi için tam etki alanı adını alır.

```json
"outputs": {
  "hostname": {
      "type": "string",
      "value": "[reference(resourceId('Microsoft.Network/publicIPAddresses', variables('publicIPAddressName'))).dnsSettings.fqdn]"
    },
}
```

# <a name="bicep"></a>[Bicep](#tab/bicep)

Bıcep için `output` anahtar sözcüğünü kullanın.

Aşağıdaki örnekte, `publicIP` Bıcep dosyasında dağıtılan bir genel IP adresinin tanımlayıcısıdır. Çıkış değeri, genel IP adresi için tam etki alanı adını alır.

```bicep
output hostname string = publicIP.properties.dnsSettings.fqdn
```

---

Adında bir kısa çizgi olan bir özelliğin çıktısını almanız gerekiyorsa, adın etrafında nokta gösterimi yerine köşeli ayraç kullanın. Örneğin, yerine kullanın  `['property-name']` `.property-name` .

# <a name="json"></a>[JSON](#tab/json)

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "variables": {
        "user": {
            "user-name": "Test Person"
        }
    },
    "resources": [
    ],
    "outputs": {
        "nameResult": {
            "type": "string",
            "value": "[variables('user')['user-name']]"
        }
    }
}
```

# <a name="bicep"></a>[Bicep](#tab/bicep)

```bicep
var user = {
  'user-name': 'Test Person'
}

output stringOutput string = user['user-name']
```

---

## <a name="conditional-output"></a>Koşullu çıkış

Koşullu bir değer döndürebilirsiniz. Genellikle [bir kaynağı koşullu olarak](conditional-resource-deployment.md) dağıttığınızda koşullu bir çıkış kullanırsınız. Aşağıdaki örnek, bir genel IP adresi için kaynak KIMLIĞININ, yeni bir birinin dağıtılıp dağıtıldığına göre nasıl koşullu olarak döndürülüp döndürülmeyeceğini göstermektedir:

# <a name="json"></a>[JSON](#tab/json)

JSON 'da, `condition` çıktının döndürülüp döndürülmeyeceğini tanımlamak için öğesini ekleyin.

```json
"outputs": {
  "resourceID": {
    "condition": "[equals(parameters('publicIpNewOrExisting'), 'new')]",
    "type": "string",
    "value": "[resourceId('Microsoft.Network/publicIPAddresses', parameters('publicIPAddresses_name'))]"
  }
}
```

# <a name="bicep"></a>[Bicep](#tab/bicep)

Bıcep 'de koşullu çıkış belirtmek için `?` işlecini kullanın. Aşağıdaki örnek, bir koşula bağlı olarak bir uç nokta URL 'SI ya da boş bir dize döndürür.

```bicep
param deployStorage bool = true
param storageName string
param location string = resourceGroup().location

resource sa 'Microsoft.Storage/storageAccounts@2019-06-01' = if (deployStorage) {
  name: storageName
  location: location
  kind: 'StorageV2'
  sku:{
    name:'Standard_LRS'
    tier: 'Standard'
  }
  properties: {
    accessTier: 'Hot'
  }
}

output endpoint string = deployStorage ? sa.properties.primaryEndpoints.blob : ''
```

---

Koşullu çıkışın basit bir örneği için bkz. [koşullu çıkış şablonu](https://github.com/bmoore-msft/AzureRM-Samples/blob/master/conditional-output/azuredeploy.json).

## <a name="dynamic-number-of-outputs"></a>Dinamik çıkış sayısı

Bazı senaryolarda, şablonu oluştururken döndürmeniz gereken bir değerin örnek sayısını bilemezsiniz. Yinelemeli çıkış kullanarak, değişken sayıda değer döndürebilirsiniz.

# <a name="json"></a>[JSON](#tab/json)

JSON 'da, `copy` bir çıktıyı yinelemek için öğesini ekleyin.

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

# <a name="bicep"></a>[Bicep](#tab/bicep)

Yinelemeli çıkış Şu anda Bıcep için kullanılamaz.

---

Daha fazla bilgi için bkz. [ARM şablonlarındaki çıkış yinelemesi](copy-outputs.md).

## <a name="linked-templates"></a>Bağlı şablonlar

JSON şablonlarında, [bağlantılı şablonları](linked-templates.md)kullanarak ilgili şablonları dağıtabilirsiniz. Bağlı bir şablondan çıkış değerini almak için, üst şablondaki [başvuru](template-functions-resource.md#reference) işlevini kullanın. Üst şablondaki sözdizimi şöyledir:

```json
"[reference('<deploymentName>').outputs.<propertyName>.value]"
```

Aşağıdaki örnek, bir yük dengeleyicide, bağlantılı şablondan bir değer alarak IP adresinin nasıl ayarlanacağını gösterir.

```json
"publicIPAddress": {
  "id": "[reference('linkedTemplate').outputs.resourceID.value]"
}
```

Özellik adında bir tire varsa, nokta gösterimi yerine adın etrafında köşeli ayraç kullanın.

```json
"publicIPAddress": {
  "id": "[reference('linkedTemplate').outputs['resource-ID'].value]"
}
```

`reference` [İç içe geçmiş bir şablonun](linked-templates.md#nested-template)çıktılar bölümünde işlevini kullanamazsınız. Dağıtılan bir kaynağın değerlerini iç içe yerleştirilmiş bir şablonda döndürmek için, iç içe geçmiş şablonunuzu bağlı bir şablona dönüştürün.

[Genel IP adresi şablonu](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/linkedtemplates/public-ip.json) , genel bir IP adresi oluşturur ve kaynak Kimliğini verir. [Yük dengeleyici şablonu](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/linkedtemplates/public-ip-parentloadbalancer.json) önceki şablona bağlanır. Yük dengeleyiciyi oluştururken çıktıda kaynak KIMLIĞINI kullanır.

## <a name="modules"></a>Modül

Bıcep dosyalarında, modülleri kullanarak ilgili şablonları dağıtabilirsiniz. Bir modülden çıkış değeri almak için aşağıdaki sözdizimini kullanın:

```bicep
<module-name>.outputs.<property-name>
```

Aşağıdaki örnek, bir modülden bir değer alarak yük dengeleyicide IP adresinin nasıl ayarlanacağını gösterir. Modülün adı `publicIP` .

```bicep
publicIPAddress: {
  id: publicIP.outputs.resourceID
}
```

## <a name="example-template"></a>Örnek şablon

Aşağıdaki şablon hiçbir kaynak dağıtmaz. Farklı türlerde çıktıları döndürmenin bazı yollarını gösterir.

# <a name="json"></a>[JSON](#tab/json)

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/outputs.json":::

# <a name="bicep"></a>[Bicep](#tab/bicep)

Bıcep Şu anda döngüleri desteklemiyor.

:::code language="bicep" source="~/resourcemanager-templates/azure-resource-manager/outputs.bicep":::

---

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

## <a name="next-steps"></a>Sonraki adımlar

* Çıktılar için kullanılabilen özellikler hakkında bilgi edinmek için bkz. [ARM şablonlarının yapısını ve sözdizimini anlayın](template-syntax.md).
