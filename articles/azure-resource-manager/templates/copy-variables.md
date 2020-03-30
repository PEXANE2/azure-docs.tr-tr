---
title: Bir değişkenin birden çok örneğini tanımlama
description: Bir değişken oluştururken birden çok kez yeniden sıralamak için Azure Kaynak Yöneticisi şablonundaki kopyalama işlemini kullanın.
ms.topic: conceptual
ms.date: 02/13/2020
ms.openlocfilehash: ed0c2d87c48a18b0a065f6c76e1e69142a9df048
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80153310"
---
# <a name="variable-iteration-in-arm-templates"></a>ARM şablonlarında değişken yineleme

Bu makalede, Azure Kaynak Yöneticisi (ARM) şablonunuzda bir değişken için birden fazla değer oluşturmasınız gösterilmektedir. **Şablonunuzun** değişkenler bölümüne kopya öğesi ekleyerek, dağıtım sırasında bir değişkenin öğe sayısını dinamik olarak ayarlayabilirsiniz. Ayrıca şablon sözdizimini yinelemek zorunda kalmaktan da kaçının.

Ayrıca [kaynaklar,](copy-resources.md) [kaynak özellikleri](copy-properties.md)ve [çıktıları](copy-outputs.md)ile kopya kullanabilirsiniz.

## <a name="variable-iteration"></a>Değişken yineleme

Kopyalama öğesi aşağıdaki genel biçime sahiptir:

```json
"copy": [
  {
    "name": "<name-of-loop>",
    "count": <number-of-iterations>,
    "input": <values-for-the-variable>
  }
]
```

**Ad** özelliği döngütanımlayan herhangi bir değerdir. **Sayım** özelliği değişken için istediğiniz yineleme sayısını belirtir.

**Giriş** özelliği, yinelemek istediğiniz özellikleri belirtir. **Giriş** özelliğindeki değerden oluşturulmuş bir dizi öğe oluşturursunuz. Tek bir özellik (dize gibi) veya çeşitli özelliklere sahip bir nesne olabilir.

Aşağıdaki örnek, dize değerleri dizisinin nasıl oluşturulabildiğini gösterir:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "itemCount": {
            "type": "int",
            "defaultValue": 5
        }
     },
    "variables": {
        "copy": [
            {
                "name": "stringArray",
                "count": "[parameters('itemCount')]",
                "input": "[concat('item', copyIndex('stringArray', 1))]"
            }
        ]
    },
    "resources": [],
    "outputs": {
        "arrayResult": {
            "type": "array",
            "value": "[variables('stringArray')]"
        }
    }
}
```

Önceki şablon aşağıdaki değerlere sahip bir dizi döndürür:

```json
[
    "item1",
    "item2",
    "item3",
    "item4",
    "item5"
]
```

Sonraki örnek, ad, diskSizeGB ve diskIndex olmak üzere üç özellisi olan bir nesne dizisinin nasıl oluşturulabildiğini gösterir.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "itemCount": {
            "type": "int",
            "defaultValue": 5
        }
    },
    "variables": {
        "copy": [
            {
                "name": "objectArray",
                "count": "[parameters('itemCount')]",
                "input": {
                    "name": "[concat('myDataDisk', copyIndex('objectArray', 1))]",
                    "diskSizeGB": "1",
                    "diskIndex": "[copyIndex('objectArray')]"
                }
            }
        ]
    },
    "resources": [],
    "outputs": {
        "arrayResult": {
            "type": "array",
            "value": "[variables('objectArray')]"
        }
    }
}
```

Önceki örnek, aşağıdaki değerlere sahip bir dizi döndürür:

```json
[
    {
        "name": "myDataDisk1",
        "diskSizeGB": "1",
        "diskIndex": 0
    },
    {
        "name": "myDataDisk2",
        "diskSizeGB": "1",
        "diskIndex": 1
    },
    {
        "name": "myDataDisk3",
        "diskSizeGB": "1",
        "diskIndex": 2
    },
    {
        "name": "myDataDisk4",
        "diskSizeGB": "1",
        "diskIndex": 3
    },
    {
        "name": "myDataDisk5",
        "diskSizeGB": "1",
        "diskIndex": 4
    }
]
```

> [!NOTE]
> Değişken yineleme bir ofset bağımsız değişkeni destekler. Ofset, copyIndex('diskNames', 1 gibi yinelemenin adından sonra gelmelidir. Ofset değeri sağlamazsanız, ilk örnek için varsayılan olarak 0'a kadar dır.
>

Bir değişken içindeki kopyalama öğesini de kullanabilirsiniz. Aşağıdaki örnek, değerlerinden biri olarak bir dizi olan bir nesne oluşturur.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "itemCount": {
            "type": "int",
            "defaultValue": 5
        }
    },
    "variables": {
        "topLevelObject": {
            "sampleProperty": "sampleValue",
            "copy": [
                {
                    "name": "disks",
                    "count": "[parameters('itemCount')]",
                    "input": {
                        "name": "[concat('myDataDisk', copyIndex('disks', 1))]",
                        "diskSizeGB": "1",
                        "diskIndex": "[copyIndex('disks')]"
                    }
                }
            ]
        }
    },
    "resources": [],
    "outputs": {
        "objectResult": {
            "type": "object",
            "value": "[variables('topLevelObject')]"
        }
    }
}
```

Önceki örnek aşağıdaki değerlere sahip bir nesneyi döndürür:

```json
{
    "sampleProperty": "sampleValue",
    "disks": [
        {
            "name": "myDataDisk1",
            "diskSizeGB": "1",
            "diskIndex": 0
        },
        {
            "name": "myDataDisk2",
            "diskSizeGB": "1",
            "diskIndex": 1
        },
        {
            "name": "myDataDisk3",
            "diskSizeGB": "1",
            "diskIndex": 2
        },
        {
            "name": "myDataDisk4",
            "diskSizeGB": "1",
            "diskIndex": 3
        },
        {
            "name": "myDataDisk5",
            "diskSizeGB": "1",
            "diskIndex": 4
        }
    ]
}
```

Sonraki örnek, değişkenlerle kopyayı kullanabileceğiniz farklı yolları gösterir.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {},
  "variables": {
    "disk-array-on-object": {
      "copy": [
        {
          "name": "disks",
          "count": 5,
          "input": {
            "name": "[concat('myDataDisk', copyIndex('disks', 1))]",
            "diskSizeGB": "1",
            "diskIndex": "[copyIndex('disks')]"
          }
        },
        {
          "name": "diskNames",
          "count": 5,
          "input": "[concat('myDataDisk', copyIndex('diskNames', 1))]"
        }
      ]
    },
    "copy": [
      {
        "name": "top-level-object-array",
        "count": 5,
        "input": {
          "name": "[concat('myDataDisk', copyIndex('top-level-object-array', 1))]",
          "diskSizeGB": "1",
          "diskIndex": "[copyIndex('top-level-object-array')]"
        }
      },
      {
        "name": "top-level-string-array",
        "count": 5,
        "input": "[concat('myDataDisk', copyIndex('top-level-string-array', 1))]"
      },
      {
        "name": "top-level-integer-array",
        "count": 5,
        "input": "[copyIndex('top-level-integer-array')]"
      }
    ]
  },
  "resources": [],
  "outputs": {
    "exampleObject": {
      "value": "[variables('disk-array-on-object')]",
      "type": "object"
    },
    "exampleArrayOnObject": {
      "value": "[variables('disk-array-on-object').disks]",
      "type" : "array"
    },
    "exampleObjectArray": {
      "value": "[variables('top-level-object-array')]",
      "type" : "array"
    },
    "exampleStringArray": {
      "value": "[variables('top-level-string-array')]",
      "type" : "array"
    },
    "exampleIntegerArray": {
      "value": "[variables('top-level-integer-array')]",
      "type" : "array"
    }
  }
}
```

## <a name="copy-limits"></a>Kopyalama sınırları

Sayım 800'i geçemez.

Sayı negatif sayı olamaz. Azure PowerShell 2.6 veya sonraki sürümlerle, Azure CLI 2.0.74 veya sonraki sürümlerle veya REST API sürümü **2019-05-10** veya daha yeni bir şablon uyguluyorsanız, sayıyı sıfıra ayarlayabilirsiniz. PowerShell, CLI ve REST API'nin önceki sürümleri nde sayım için sıfır desteklenmez.

## <a name="example-templates"></a>Örnek şablonlar

Aşağıdaki örnekler, bir değişken için birden fazla değer oluşturmak için yaygın senaryoları gösterir.

|Şablon  |Açıklama  |
|---------|---------|
|[Kopya değişkenleri](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/copyvariables.json) |Değişkenler üzerinde eşitlenmenin farklı yollarını gösterir. |
|[Birden çok güvenlik kuralı](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/multiplesecurityrules.json) |Bir ağ güvenlik grubuna çeşitli güvenlik kuralları dağıtLar. Güvenlik kurallarını bir parametreden inşa eder. Parametre için [birden çok NSG parametre dosyasına](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/multiplesecurityrules.parameters.json)bakın. |

## <a name="next-steps"></a>Sonraki adımlar

* Bir öğreticiye geçmek için [Bkz. Öğretici: ARM şablonlarını kullanarak birden çok kaynak örneği oluşturun.](template-tutorial-create-multiple-instances.md)
* Kopyalama öğesinin diğer kullanımları için bkz:
  * [ARM şablonlarında kaynak yinelemesi](copy-resources.md)
  * [ARM şablonlarında özellik yinelemesi](copy-properties.md)
  * [ARM şablonlarında çıkış yinelemesi](copy-outputs.md)
* Şablonun bölümleri hakkında bilgi edinmek istiyorsanız, [Bkz.](template-syntax.md)
* Şablonunuzu nasıl dağıtılayarak dağıtılayacaklarını öğrenmek için [bkz.](deploy-powershell.md)

