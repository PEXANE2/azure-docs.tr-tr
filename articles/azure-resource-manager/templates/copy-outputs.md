---
title: Bir çıkış değerinin birden çok örneğini tanımlama
description: Bir dağıtımdan değer döndürürken birden çok kez yinelemek için Azure Resource Manager şablonunda kopyalama işlemini kullanın.
ms.topic: conceptual
ms.date: 04/17/2020
ms.openlocfilehash: 0315af2f083285c4704b08fec608341b6f0b2231
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81617825"
---
# <a name="output-iteration-in-arm-templates"></a>ARM şablonlarındaki çıkış yinelemesi

Bu makalede, Azure Resource Manager (ARM) şablonunuzda bir çıktı için birden fazla değer oluşturma gösterilmektedir. Şablonunuzun çıktılar bölümüne **Copy** öğesini ekleyerek, dağıtım sırasında bir dizi öğeyi dinamik olarak döndürebilirsiniz.

Ayrıca, [kaynak, bir kaynaktaki Özellikler](copy-properties.md)ve [değişkenler](copy-variables.md) [ile kopyalama](copy-resources.md)özelliğini de kullanabilirsiniz.

## <a name="outputs-iteration"></a>Çıkış yinelemesi

Copy öğesi aşağıdaki genel biçime sahiptir:

```json
"copy": {
  "count": <number-of-iterations>,
  "input": <values-for-the-output>
}
```

**Count** özelliği, çıkış değeri için istediğiniz yineleme sayısını belirtir.

**Input** özelliği, yinelemek istediğiniz özellikleri belirtir. **Giriş** özelliğindeki değerden oluşturulan bir dizi öğe oluşturun. Tek bir Özellik (bir dize gibi) veya birkaç özelliği olan bir nesnesi olabilir.

Aşağıdaki örnek, değişken sayıda depolama hesabı oluşturur ve her depolama hesabı için bir uç nokta döndürür:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storageCount": {
            "type": "int",
            "defaultValue": 2
        }
    },
    "variables": {
        "baseName": "[concat('storage', uniqueString(resourceGroup().id))]"
    },
    "resources": [
        {
            "type": "Microsoft.Storage/storageAccounts",
            "apiVersion": "2019-04-01",
            "name": "[concat(copyIndex(), variables('baseName'))]",
            "location": "[resourceGroup().location]",
            "sku": {
                "name": "Standard_LRS"
            },
            "kind": "Storage",
            "properties": {},
            "copy": {
                "name": "storagecopy",
                "count": "[parameters('storageCount')]"
            }
        }
    ],
    "outputs": {
        "storageEndpoints": {
            "type": "array",
            "copy": {
                "count": "[parameters('storageCount')]",
                "input": "[reference(concat(copyIndex(), variables('baseName'))).primaryEndpoints.blob]"
            }
        }
    }
}
```

Önceki şablon, aşağıdaki değerlere sahip bir dizi döndürür:

```json
[
    "https://0storagecfrbqnnmpeudi.blob.core.windows.net/",
    "https://1storagecfrbqnnmpeudi.blob.core.windows.net/"
]
```

Sonraki örnekte, yeni depolama hesaplarından üç özellik döndürülür.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storageCount": {
            "type": "int",
            "defaultValue": 2
        }
    },
    "variables": {
        "baseName": "[concat('storage', uniqueString(resourceGroup().id))]"
    },
    "resources": [
        {
            "type": "Microsoft.Storage/storageAccounts",
            "apiVersion": "2019-04-01",
            "name": "[concat(copyIndex(), variables('baseName'))]",
            "location": "[resourceGroup().location]",
            "sku": {
                "name": "Standard_LRS"
            },
            "kind": "Storage",
            "properties": {},
            "copy": {
                "name": "storagecopy",
                "count": "[parameters('storageCount')]"
            }
        }
    ],
    "outputs": {
        "storageInfo": {
            "type": "array",
            "copy": {
                "count": "[parameters('storageCount')]",
                "input": {
                    "id": "[reference(concat(copyIndex(), variables('baseName')), '2019-04-01', 'Full').resourceId]",
                    "blobEndpoint": "[reference(concat(copyIndex(), variables('baseName'))).primaryEndpoints.blob]",
                    "status": "[reference(concat(copyIndex(), variables('baseName'))).statusOfPrimary]"
                }
            }
        }
    }
}
```

Yukarıdaki örnek aşağıdaki değerlere sahip bir dizi döndürür:

```json
[
    {
        "id": "Microsoft.Storage/storageAccounts/0storagecfrbqnnmpeudi",
        "blobEndpoint": "https://0storagecfrbqnnmpeudi.blob.core.windows.net/",
        "status": "available"
    },
    {
        "id": "Microsoft.Storage/storageAccounts/1storagecfrbqnnmpeudi",
        "blobEndpoint": "https://1storagecfrbqnnmpeudi.blob.core.windows.net/",
        "status": "available"
    }
]
```

## <a name="next-steps"></a>Sonraki adımlar

* Öğreticiye gitmek için bkz. [öğretici: ARM şablonları kullanarak birden çok kaynak örneği oluşturma](template-tutorial-create-multiple-instances.md).
* Copy öğesinin diğer kullanımları için bkz.:
  * [ARM şablonlarındaki kaynak yinelemesi](copy-resources.md)
  * [ARM şablonlarındaki Özellik yinelemesi](copy-properties.md)
  * [ARM şablonlarında değişken yineleme](copy-variables.md)
* Bir şablonun bölümleri hakkında daha fazla bilgi edinmek istiyorsanız, bkz. [ARM şablonları yazma](template-syntax.md).
* Şablonunuzu dağıtmayı öğrenmek için bkz. [ARM şablonuyla uygulama dağıtma](deploy-powershell.md).

