---
title: Şablon işlevleri-dağıtım
description: Dağıtım bilgilerini almak için bir Azure Resource Manager şablonunda kullanılacak işlevleri açıklar.
ms.topic: conceptual
ms.date: 11/19/2019
ms.openlocfilehash: a255cea128241465788f21013eb0522a29f5bd9e
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74230229"
---
# <a name="deployment-functions-for-azure-resource-manager-templates"></a>Azure Resource Manager şablonlar için dağıtım işlevleri 

Kaynak Yöneticisi, şablon bölümlerinden ve dağıtımla ilgili değerlerin değerlerini almak için aşağıdaki işlevleri sağlar:

* [dağıtmak](#deployment)
* [ortamınızın](#environment)
* [parametreler](#parameters)
* [değişkenlerinin](#variables)

Kaynaklardan, kaynak gruplarından veya aboneliklerden değerler almak için bkz. [kaynak işlevleri](resource-group-template-functions-resource.md).

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

Portalın dağıtım geçmişinden bir şablonu yeniden dağıtırsanız, şablon yerel bir dosya olarak dağıtılır. Dağıtım işlevinde `templateLink` özelliği döndürülmedi. Şablonunuz başka bir şablonun bağlantısını oluşturmak için `templateLink` kullanıyorsa, yeniden dağıtmak için portalını kullanmayın. Bunun yerine, şablonu ilk olarak dağıtmak için kullandığınız komutları kullanın.

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

Dağıtım işlevini kullanan bir abonelik düzeyi şablon için bkz. [abonelik dağıtım işlevi](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/deploymentsubscription.json). `az deployment create` ya da `New-AzDeployment` komutlarıyla dağıtılır.

## <a name="environment"></a>environment

`environment()`

Dağıtım için kullanılan Azure ortamı hakkındaki bilgileri döndürür.

### <a name="return-value"></a>Dönüş değeri

Bu işlev, geçerli Azure ortamı için özellikleri döndürür.

```json
{
  "name": "",
  "gallery": "",
  "graph": "",
  "portal": "",
  "graphAudience": "",
  "activeDirectoryDataLake": "",
  "batch": "",
  "media": "",
  "sqlManagement": "",
  "vmImageAliasDoc": "",
  "resourceManager": "",
  "authentication": {
    "loginEndpoint": "",
    "audiences": [
      "",
      ""
    ],
    "tenant": "",
    "identityProvider": ""
  },
  "suffixes": {
    "acrLoginServer": "",
    "azureDatalakeAnalyticsCatalogAndJob": "",
    "azureDatalakeStoreFileSystem": "",
    "azureFrontDoorEndpointSuffix": "",
    "keyvaultDns": "",
    "sqlServerHostname": "",
    "storage": ""
  }
}
```

### <a name="example"></a>Örnek

Aşağıdaki örnek şablon, ortam nesnesini döndürür.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "environmentOutput": {
            "value": "[environment()]",
            "type" : "object"
        }
    }
}
```

Yukarıdaki örnekte, genel Azure 'a dağıtıldığında aşağıdaki nesne döndürülür:

```json
{
  "name": "AzureCloud",
  "gallery": "https://gallery.azure.com/",
  "graph": "https://graph.windows.net/",
  "portal": "https://portal.azure.com",
  "graphAudience": "https://graph.windows.net/",
  "activeDirectoryDataLake": "https://datalake.azure.net/",
  "batch": "https://batch.core.windows.net/",
  "media": "https://rest.media.azure.net",
  "sqlManagement": "https://management.core.windows.net:8443/",
  "vmImageAliasDoc": "https://raw.githubusercontent.com/Azure/azure-rest-api-specs/master/arm-compute/quickstart-templates/aliases.json",
  "resourceManager": "https://management.azure.com/",
  "authentication": {
    "loginEndpoint": "https://login.windows.net/",
    "audiences": [
      "https://management.core.windows.net/",
      "https://management.azure.com/"
    ],
    "tenant": "common",
    "identityProvider": "AAD"
  },
  "suffixes": {
    "acrLoginServer": ".azurecr.io",
    "azureDatalakeAnalyticsCatalogAndJob": "azuredatalakeanalytics.net",
    "azureDatalakeStoreFileSystem": "azuredatalakestore.net",
    "azureFrontDoorEndpointSuffix": "azurefd.net",
    "keyvaultDns": ".vault.azure.net",
    "sqlServerHostname": ".database.windows.net",
    "storage": "core.windows.net"
  }
}
```

## <a name="parameters"></a>parameters

`parameters(parameterName)`

Bir parametre değeri döndürür. Belirtilen parametre adı, şablonun parametreler bölümünde tanımlanmalıdır.

### <a name="parameters"></a>Parametreler

| Parametre | Gereklidir | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| parameterName |Evet |string |Döndürülecek parametrenin adı. |

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

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| stringOutput | Dize | Seçenek 1 |
| ıntoutput | Int | 1 |
| objectOutput | Nesne | {"One": "a", "iki": "b"} |
| arrayOutput | Dizi | [1, 2, 3] |
| Çapraz Soutput | Dize | Seçenek 1 |

Parametreleri kullanma hakkında daha fazla bilgi için bkz. [Azure Resource Manager şablonundaki parametreler](template-parameters.md).

## <a name="variables"></a>değişkenlerinin

`variables(variableName)`

Değişkenin değerini döndürür. Belirtilen değişken adının şablonun değişkenler bölümünde tanımlanması gerekir.

### <a name="parameters"></a>Parametreler

| Parametre | Gereklidir | Tür | Açıklama |
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

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| exampleOutput1 | Dize | myVariable & lt |
| exampleOutput2 | Dizi | [1, 2, 3, 4] |
| exampleOutput3 | Dize | myVariable & lt |
| exampleOutput4 |  Nesne | {"Property1": "değer1", "Property2": "değer2"} |

Değişkenleri kullanma hakkında daha fazla bilgi için bkz. [Azure Resource Manager şablonundaki değişkenler](template-variables.md).

## <a name="next-steps"></a>Sonraki adımlar
* Azure Resource Manager şablonundaki bölümlerin açıklaması için bkz. [yazma Azure Resource Manager şablonları](resource-group-authoring-templates.md).
* Çeşitli şablonları birleştirmek için bkz. [bağlantılı şablonları Azure Resource Manager kullanma](resource-group-linked-templates.md).
* Kaynak türünü oluştururken belirtilen sayıda yinelemek için, bkz. [Azure Resource Manager birden fazla kaynak örneği oluşturma](resource-group-create-multiple.md).
* Oluşturduğunuz şablonun nasıl dağıtılacağını görmek için bkz. [Azure Resource Manager şablonuyla uygulama dağıtma](resource-group-template-deploy.md).

