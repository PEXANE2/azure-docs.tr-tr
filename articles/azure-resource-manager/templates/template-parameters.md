---
title: Şablonlardaki parametreler
description: Azure Resource Manager şablonunda parametrelerin nasıl tanımlanacağını açıklar.
ms.topic: conceptual
ms.date: 11/24/2020
ms.openlocfilehash: 8cdc95037967a32c2d8464f4dc39b1e7369102bb
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2020
ms.locfileid: "95911422"
---
# <a name="parameters-in-azure-resource-manager-templates"></a>Azure Resource Manager şablonlarında parametreler

Bu makalede Azure Resource Manager şablonunuzda parametrelerin nasıl tanımlanacağı ve kullanılacağı açıklanmaktadır. Parametreler için farklı değerler sunarak, farklı ortamlar için bir şablonu yeniden kullanabilirsiniz.

Kaynak Yöneticisi, dağıtım işlemlerini başlatmadan önce parametre değerlerini çözer. Parametrenin şablonda kullanıldığı her yerde, Kaynak Yöneticisi çözümlenme değeriyle değiştirilir.

Her parametrenin [veri türlerinden](template-syntax.md#data-types)birine ayarlanması gerekir.

## <a name="define-parameter"></a>Parametre tanımla

Aşağıdaki örnek bir basit parametre tanımını gösterir. **Storagesku** adlı bir parametre tanımlar. Parametresi bir dize değeridir ve yalnızca amaçlanan kullanımı için geçerli olan değerleri kabul eder. Dağıtım sırasında hiçbir değer sağlanmamışsa parametre varsayılan değeri kullanır.

```json
"parameters": {
  "storageSKU": {
    "type": "string",
    "allowedValues": [
      "Standard_LRS",
      "Standard_ZRS",
      "Standard_GRS",
      "Standard_RAGRS",
      "Premium_LRS"
    ],
    "defaultValue": "Standard_LRS",
    "metadata": {
      "description": "The type of replication to use for the storage account."
    }
  }
}
```

## <a name="use-parameter"></a>Parametre kullan

Şablonda [Parametreler](template-functions-deployment.md#parameters) işlevini kullanarak parametrenin değerine başvurarak. Aşağıdaki örnekte, depolama hesabı için SKU ayarlamak üzere parametre değeri kullanılır.

```json
"resources": [
  {
    "type": "Microsoft.Storage/storageAccounts",
    "sku": {
      "name": "[parameters('storageSKU')]"
    },
    ...
  }
]
```

## <a name="template-functions"></a>Şablon işlevleri

Bir parametre için varsayılan değeri belirtirken, çoğu şablon işlevini kullanabilirsiniz. Varsayılan bir değer oluşturmak için başka bir parametre değeri de kullanabilirsiniz. Aşağıdaki şablon, varsayılan değer içindeki işlevlerin kullanımını gösterir. Site için bir ad sağlanmamışsa, benzersiz bir dize değeri oluşturur ve **siteye** ekler. Konak planı için bir ad sağlanmamışsa, site için değeri alır **ve ekler.**

```json
"parameters": {
  "siteName": {
    "type": "string",
    "defaultValue": "[concat('site', uniqueString(resourceGroup().id))]",
    "metadata": {
      "description": "The site name. To use the default value, do not specify a new value."
    }
  },
  "hostingPlanName": {
    "type": "string",
    "defaultValue": "[concat(parameters('siteName'),'-plan')]",
    "metadata": {
      "description": "The host name. To use the default value, do not specify a new value."
    }
  }
}
```

Parameters bölümünde [başvuru](template-functions-resource.md#reference) işlevini veya [liste](template-functions-resource.md#list) işlevlerinden herhangi birini kullanamazsınız. Bu işlevler, bir kaynağın çalışma zamanı durumunu alır ve parametreler çözümlendiğinde dağıtımdan önce yürütülemez.

## <a name="objects-as-parameters"></a>Parametre olarak nesneler

İlgili değerleri bir nesne olarak geçirerek düzenlemek daha kolay olabilir. Bu yaklaşım ayrıca şablondaki parametre sayısını azaltır.

Aşağıdaki örnek bir nesnesi olan bir parametresini gösterir. Varsayılan değer, nesne için beklenen özellikleri gösterir.

```json
"parameters": {
  "VNetSettings": {
    "type": "object",
    "defaultValue": {
      "name": "VNet1",
      "location": "eastus",
      "addressPrefixes": [
        {
          "name": "firstPrefix",
          "addressPrefix": "10.0.0.0/22"
        }
      ],
      "subnets": [
        {
          "name": "firstSubnet",
          "addressPrefix": "10.0.0.0/24"
        },
        {
          "name": "secondSubnet",
          "addressPrefix": "10.0.1.0/24"
        }
      ]
    }
  }
},
```

Nokta işlecini kullanarak nesnesinin özelliklerine başvurun.

```json
"resources": [
  {
    "type": "Microsoft.Network/virtualNetworks",
    "apiVersion": "2015-06-15",
    "name": "[parameters('VNetSettings').name]",
    "location": "[parameters('VNetSettings').location]",
    "properties": {
      "addressSpace":{
        "addressPrefixes": [
          "[parameters('VNetSettings').addressPrefixes[0].addressPrefix]"
        ]
      },
      "subnets":[
        {
          "name":"[parameters('VNetSettings').subnets[0].name]",
          "properties": {
            "addressPrefix": "[parameters('VNetSettings').subnets[0].addressPrefix]"
          }
        },
        {
          "name":"[parameters('VNetSettings').subnets[1].name]",
          "properties": {
            "addressPrefix": "[parameters('VNetSettings').subnets[1].addressPrefix]"
          }
        }
      ]
    }
  }
]
```

## <a name="example-templates"></a>Örnek Şablonlar

Aşağıdaki örneklerde, parametreleri kullanmaya yönelik senaryolar gösterilmektedir.

|Şablon  |Description  |
|---------|---------|
|[Varsayılan değerler için işlevlere sahip parametreler](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/parameterswithfunctions.json) | Parametreler için varsayılan değerleri tanımlarken şablon işlevlerinin nasıl kullanılacağını gösterir. Şablon hiçbir kaynak dağıtmaz. Parametre değerlerini oluşturur ve bu değerleri döndürür. |
|[Parameter nesnesi](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/parameterobject.json) | Bir parametre için bir nesne kullanmayı gösterir. Şablon hiçbir kaynak dağıtmaz. Parametre değerlerini oluşturur ve bu değerleri döndürür. |


## <a name="next-steps"></a>Sonraki adımlar

* Parametrelerin kullanılabilir özellikleri hakkında bilgi edinmek için bkz. [Azure Resource Manager şablonlarının yapısını ve sözdizimini anlayın](template-syntax.md).
* Parametre değerlerini bir dosya olarak geçirme hakkında bilgi edinmek için bkz. [Kaynak Yöneticisi parametre dosyası oluşturma](parameter-files.md).
* Parametreleri oluşturma hakkında öneriler için bkz. [en iyi yöntemler-parametreler](template-best-practices.md#parameters).
