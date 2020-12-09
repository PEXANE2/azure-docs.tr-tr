---
title: Bir çıkış değerinin birden çok örneğini tanımlama
description: Bir dağıtımdan değer döndürürken birden çok kez yinelemek için bir Azure Resource Manager şablonunda kopyalama işlemi kullanın (ARM şablonu).
ms.topic: conceptual
ms.date: 04/17/2020
ms.openlocfilehash: 5ae20ed9ec3fdb3b76adbd370f5ba22f9386d613
ms.sourcegitcommit: 80c1056113a9d65b6db69c06ca79fa531b9e3a00
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/09/2020
ms.locfileid: "96905953"
---
# <a name="output-iteration-in-arm-templates"></a>ARM şablonlarındaki çıkış yinelemesi

Bu makalede, Azure Resource Manager şablonunuzda (ARM şablonu) bir çıktı için birden fazla değer oluşturma gösterilmektedir. `copy`Şablonunuzun çıktılar bölümüne öğesini ekleyerek dağıtım sırasında bir dizi öğeyi dinamik olarak döndürebilirsiniz.

Ayrıca, [kaynak, bir kaynaktaki Özellikler](copy-properties.md)ve [değişkenler](copy-variables.md) [ile kopyalama](copy-resources.md)özelliğini de kullanabilirsiniz.

## <a name="syntax"></a>Syntax

Copy öğesi aşağıdaki genel biçime sahiptir:

```json
"copy": {
  "count": <number-of-iterations>,
  "input": <values-for-the-output>
}
```

`count`Özelliği, çıkış değeri için istediğiniz yineleme sayısını belirtir.

`input`Özelliği yinelemek istediğiniz özellikleri belirtir. Özelliğindeki değerden oluşturulan bir dizi öğe oluşturursunuz `input` . Tek bir Özellik (bir dize gibi) veya birkaç özelliği olan bir nesnesi olabilir.

## <a name="copy-limits"></a>Sınırları Kopyala

Sayım 800 ' i aşamaz.

Sayı negatif bir sayı olamaz. Yeni bir Azure CLı, PowerShell veya REST API sürümü ile şablonu dağıtırsanız sıfır olabilir. Özellikle, şunu kullanmanız gerekir:

* Azure PowerShell **2,6** veya üzeri
* Azure CLı **2.0.74** veya üzeri
* REST API sürüm **2019-05-10** veya üzeri
* [Bağlı dağıtımlar](linked-templates.md) , dağıtım kaynak türü için apı sürüm **2019-05-10** veya üstünü kullanmalıdır

PowerShell, CLı ve REST API 'nin önceki sürümleri Count için sıfırı desteklemez.

## <a name="outputs-iteration"></a>Çıkış yinelemesi

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

* Öğreticiye gitmek için bkz. [öğretici: ARM şablonlarıyla birden çok kaynak örneği oluşturma](template-tutorial-create-multiple-instances.md).
* Copy öğesinin diğer kullanımları için bkz.:
  * [ARM şablonlarındaki kaynak yinelemesi](copy-resources.md)
  * [ARM şablonlarındaki Özellik yinelemesi](copy-properties.md)
  * [ARM şablonlarında değişken yineleme](copy-variables.md)
* Bir şablonun bölümleri hakkında bilgi edinmek istiyorsanız, bkz. [ARM şablonlarının yapısını ve sözdizimini anlayın](template-syntax.md).
* Şablonunuzu dağıtmayı öğrenmek için bkz. [ARM şablonlarıyla kaynak dağıtma ve Azure PowerShell](deploy-powershell.md).
