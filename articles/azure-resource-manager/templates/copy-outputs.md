---
title: Çıktı değerinin birden çok örneğini tanımlama
description: Bir dağıtımdan değer döndürerken birden çok kez yeniden sıralamak için Azure Kaynak Yöneticisi şablonundaki kopyalama işlemini kullanın.
ms.topic: conceptual
ms.date: 04/17/2020
ms.openlocfilehash: 0315af2f083285c4704b08fec608341b6f0b2231
ms.sourcegitcommit: eefb0f30426a138366a9d405dacdb61330df65e7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81617825"
---
# <a name="output-iteration-in-arm-templates"></a>ARM şablonlarında çıkış yinelemesi

Bu makalede, Azure Kaynak Yöneticisi (ARM) şablonunuzda bir çıktı için birden fazla değer oluşturmasınız gösterilmektedir. Şablonunuzun çıktılar bölümüne **kopyalama** öğesini ekleyerek, dağıtım sırasında bir dizi öğeyi dinamik olarak döndürebilirsiniz.

Ayrıca [kaynaklar,](copy-resources.md) [kaynak özellikleri](copy-properties.md)ve [değişkenler](copy-variables.md)ile kopya kullanabilirsiniz.

## <a name="outputs-iteration"></a>Çıktılar yineleme

Kopyalama öğesi aşağıdaki genel biçime sahiptir:

```json
"copy": {
  "count": <number-of-iterations>,
  "input": <values-for-the-output>
}
```

**Sayım** özelliği, çıktı değeri için istediğiniz yineleme sayısını belirtir.

**Giriş** özelliği, yinelemek istediğiniz özellikleri belirtir. **Giriş** özelliğindeki değerden oluşturulmuş bir dizi öğe oluşturursunuz. Tek bir özellik (dize gibi) veya çeşitli özelliklere sahip bir nesne olabilir.

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

Önceki şablon aşağıdaki değerlere sahip bir dizi döndürür:

```json
[
    "https://0storagecfrbqnnmpeudi.blob.core.windows.net/",
    "https://1storagecfrbqnnmpeudi.blob.core.windows.net/"
]
```

Sonraki örnek, yeni depolama hesaplarından üç özellik döndürür.

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

Önceki örnek, aşağıdaki değerlere sahip bir dizi döndürür:

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

* Bir öğreticiye geçmek için [Bkz. Öğretici: ARM şablonlarını kullanarak birden çok kaynak örneği oluşturun.](template-tutorial-create-multiple-instances.md)
* Kopyalama öğesinin diğer kullanımları için bkz:
  * [ARM şablonlarında kaynak yinelemesi](copy-resources.md)
  * [ARM şablonlarında özellik yinelemesi](copy-properties.md)
  * [ARM şablonlarında değişken yineleme](copy-variables.md)
* Şablonun bölümleri hakkında bilgi edinmek istiyorsanız, [Bkz.](template-syntax.md)
* Şablonunuzu nasıl dağıtılayarak dağıtılayacaklarını öğrenmek için [bkz.](deploy-powershell.md)

