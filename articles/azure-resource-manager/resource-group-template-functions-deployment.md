---
title: Azure Resource Manager şablon işlevleri-dağıtım | Microsoft Docs
description: Dağıtım bilgilerini almak için bir Azure Resource Manager şablonunda kullanılacak işlevleri açıklar.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 01/03/2019
ms.author: tomfitz
ms.openlocfilehash: 236fbb9e4ed3283ecf9147e6eb5033fb906a127b
ms.sourcegitcommit: 532335f703ac7f6e1d2cc1b155c69fc258816ede
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2019
ms.locfileid: "70194342"
---
# <a name="deployment-functions-for-azure-resource-manager-templates"></a>Azure Resource Manager şablonlar için dağıtım işlevleri 

Kaynak Yöneticisi, şablon bölümlerinden ve dağıtımla ilgili değerlerin değerlerini almak için aşağıdaki işlevleri sağlar:

* [dağıtmak](#deployment)
* [parameters](#parameters)
* [variables](#variables)

Kaynaklardan, kaynak gruplarından veya aboneliklerden değerler almak için bkz. [kaynak işlevleri](resource-group-template-functions-resource.md).

<a id="deployment" />

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="deployment"></a>deployment
`deployment()`

Geçerli dağıtım işlemiyle ilgili bilgileri döndürür.

### <a name="return-value"></a>Dönüş değeri

Bu işlev, dağıtım sırasında geçirilen nesneyi döndürür. Döndürülen nesnedeki özellikler, dağıtım nesnesinin bir bağlantı olarak mı yoksa satır içi nesne olarak mı geçiriltiğine göre farklılık gösterir. Dağıtım nesnesi, bir yerel dosyayı işaret etmek için Azure PowerShell ' de **-TemplateFile** parametresinin kullanıldığı durumlarda olduğu gibi, döndürülen nesne aşağıdaki biçimdedir:

```json
{
    "name": "",
    "properties": {
        "template": {
            "$schema": "",
            "contentVersion": "",
            "parameters": {},
            "variables": {},
            "resources": [
            ],
            "outputs": {}
        },
        "parameters": {},
        "mode": "",
        "provisioningState": ""
    }
}
```

Nesne bir bağlantı olarak geçirildiğinde, örneğin uzak bir nesneyi işaret etmek için **-templateuri** parametresi kullanılırken, nesne aşağıdaki biçimde döndürülür: 

```json
{
    "name": "",
    "properties": {
        "templateLink": {
            "uri": ""
        },
        "template": {
            "$schema": "",
            "contentVersion": "",
            "parameters": {},
            "variables": {},
            "resources": [],
            "outputs": {}
        },
        "parameters": {},
        "mode": "",
        "provisioningState": ""
    }
}
```

Bir [Azure aboneliğine dağıtırken](deploy-to-subscription.md), bir kaynak grubu yerine, dönüş nesnesi bir `location` özelliği içerir. Location özelliği, yerel bir şablon ya da bir dış şablon dağıtıldığında dahil edilir.

### <a name="remarks"></a>Açıklamalar

Üst şablonun URI 'sini temel alan başka bir şablona bağlamak için Deployment () kullanabilirsiniz.

```json
"variables": {  
    "sharedTemplateUrl": "[uri(deployment().properties.templateLink.uri, 'shared-resources.json')]"  
}
```  

Portalın dağıtım geçmişinden bir şablonu yeniden dağıtırsanız, şablon yerel bir dosya olarak dağıtılır. `templateLink` Özellik dağıtım işlevinde döndürülmedi. Şablonunuz başka bir şablona bağlantı `templateLink` oluşturmak için kullanıyorsa, yeniden dağıtmak için portalını kullanmayın. Bunun yerine, şablonu ilk olarak dağıtmak için kullandığınız komutları kullanın.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/deployment.json) dağıtım nesnesini döndürür:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "subscriptionOutput": {
            "value": "[deployment()]",
            "type" : "object"
        }
    }
}
```

Önceki örnekte aşağıdaki nesne döndürülür:

```json
{
  "name": "deployment",
  "properties": {
    "template": {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "resources": [],
      "outputs": {
        "subscriptionOutput": {
          "type": "Object",
          "value": "[deployment()]"
        }
      }
    },
    "parameters": {},
    "mode": "Incremental",
    "provisioningState": "Accepted"
  }
}
```

Azure CLI ile bu örnek şablonu dağıtmak için şunu kullanın:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/deployment.json
```

PowerShell ile bu örnek şablonu dağıtmak için şunu kullanın:

```powershell
New-AzResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/deployment.json
```

Dağıtım işlevini kullanan bir abonelik düzeyi şablon için bkz. [abonelik dağıtım işlevi](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/deploymentsubscription.json). `az deployment create` Ya`New-AzDeployment` da komutlarıyla dağıtılır.

<a id="parameters" />

## <a name="parameters"></a>parameters
`parameters(parameterName)`

Bir parametre değeri döndürür. Belirtilen parametre adı, şablonun parametreler bölümünde tanımlanmalıdır.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| parameterName |Evet |dize |Döndürülecek parametrenin adı. |

### <a name="return-value"></a>Dönüş değeri

Belirtilen parametrenin değeri.

### <a name="remarks"></a>Açıklamalar

Genellikle, kaynak değerlerini ayarlamak için parametreleri kullanırsınız. Aşağıdaki örnek, Web sitesinin adını dağıtım sırasında geçirilen parametre değerine ayarlar.

```json
"parameters": { 
  "siteName": {
      "type": "string"
  }
},
"resources": [
   {
      "apiVersion": "2016-08-01",
      "name": "[parameters('siteName')]",
      "type": "Microsoft.Web/Sites",
      ...
   }
]
```

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/parameters.json) , Parameters işlevinin Basitleştirilmiş kullanımını gösterir.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "stringParameter": {
            "type" : "string",
            "defaultValue": "option 1"
        },
        "intParameter": {
            "type": "int",
            "defaultValue": 1
        },
        "objectParameter": {
            "type": "object",
            "defaultValue": {"one": "a", "two": "b"}
        },
        "arrayParameter": {
            "type": "array",
            "defaultValue": [1, 2, 3]
        },
        "crossParameter": {
            "type": "string",
            "defaultValue": "[parameters('stringParameter')]"
        }
    },
    "variables": {},
    "resources": [],
    "outputs": {
        "stringOutput": {
            "value": "[parameters('stringParameter')]",
            "type" : "string"
        },
        "intOutput": {
            "value": "[parameters('intParameter')]",
            "type" : "int"
        },
        "objectOutput": {
            "value": "[parameters('objectParameter')]",
            "type" : "object"
        },
        "arrayOutput": {
            "value": "[parameters('arrayParameter')]",
            "type" : "array"
        },
        "crossOutput": {
            "value": "[parameters('crossParameter')]",
            "type" : "string"
        }
    }
}
```

Önceki örnekte varsayılan değerlere sahip çıktı.

| Ad | Tür | Value |
| ---- | ---- | ----- |
| stringOutput | Dize | seçenek 1 |
| ıntoutput | Int | 1\. |
| objectOutput | Object | {"One": "a", "iki": "b"} |
| arrayOutput | Array | [1, 2, 3] |
| Çapraz Soutput | Dize | seçenek 1 |

Azure CLI ile bu örnek şablonu dağıtmak için şunu kullanın:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/parameters.json
```

PowerShell ile bu örnek şablonu dağıtmak için şunu kullanın:

```powershell
New-AzResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/parameters.json
```

<a id="variables" />

## <a name="variables"></a>değişkenlerinin
`variables(variableName)`

Değişkenin değerini döndürür. Belirtilen değişken adının şablonun değişkenler bölümünde tanımlanması gerekir.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| variableName |Evet |Dize |Döndürülecek değişkenin adı. |

### <a name="return-value"></a>Dönüş değeri

Belirtilen değişkenin değeri.

### <a name="remarks"></a>Açıklamalar

Genellikle, karmaşık değerleri yalnızca bir kez oluşturarak şablonunuzu basitleştirmek için değişkenler kullanırsınız. Aşağıdaki örnek, bir depolama hesabı için benzersiz bir ad oluşturur.

```json
"variables": {
    "storageName": "[concat('storage', uniqueString(resourceGroup().id))]"
},
"resources": [
    {
        "type": "Microsoft.Storage/storageAccounts",
        "name": "[variables('storageName')]",
        ...
    },
    {
        "type": "Microsoft.Compute/virtualMachines",
        "dependsOn": [
            "[variables('storageName')]"
        ],
        ...
    }
],
```

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/variables.json) farklı değişken değerleri döndürüyor.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {},
    "variables": {
        "var1": "myVariable",
        "var2": [ 1,2,3,4 ],
        "var3": "[ variables('var1') ]",
        "var4": {
            "property1": "value1",
            "property2": "value2"
        }
    },
    "resources": [],
    "outputs": {
        "exampleOutput1": {
            "value": "[variables('var1')]",
            "type" : "string"
        },
        "exampleOutput2": {
            "value": "[variables('var2')]",
            "type" : "array"
        },
        "exampleOutput3": {
            "value": "[variables('var3')]",
            "type" : "string"
        },
        "exampleOutput4": {
            "value": "[variables('var4')]",
            "type" : "object"
        }
    }
}
```

Önceki örnekte varsayılan değerlere sahip çıktı.

| Ad | Tür | Value |
| ---- | ---- | ----- |
| exampleOutput1 | Dize | myVariable & lt |
| exampleOutput2 | Array | [1, 2, 3, 4] |
| exampleOutput3 | Dize | myVariable & lt |
| exampleOutput4 |  Object | {"Property1": "değer1", "Property2": "değer2"} |

Azure CLI ile bu örnek şablonu dağıtmak için şunu kullanın:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/variables.json
```

PowerShell ile bu örnek şablonu dağıtmak için şunu kullanın:

```powershell
New-AzResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/variables.json
```

## <a name="next-steps"></a>Sonraki adımlar
* Bir Azure Resource Manager şablonu olarak bölümlerde açıklaması için bkz: [Azure Resource Manager şablonları yazma](resource-group-authoring-templates.md).
* Çeşitli şablonları birleştirmek için bkz. [bağlantılı şablonları Azure Resource Manager kullanma](resource-group-linked-templates.md).
* Belirtilen sayıda yineleme için bir kaynak türünü oluştururken bkz [Azure Resource Manager'da kaynakları birden çok örneğini oluşturma](resource-group-create-multiple.md).
* Oluşturduğunuz bir şablonu dağıtmayı öğrenmek için bkz [Azure Resource Manager şablonu ile uygulama dağıtma](resource-group-template-deploy.md).

