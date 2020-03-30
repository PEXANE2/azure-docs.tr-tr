---
title: Bir özelliğin birden çok örneğini tanımlama
description: Bir kaynak üzerinde özellik oluştururken birden çok kez yeniden sıralamak için Azure Kaynak Yöneticisi şablonunda kopyalama işlemini kullanın.
ms.topic: conceptual
ms.date: 02/13/2020
ms.openlocfilehash: e86d38b0e5d2e39d54b3c419b6eebdcda74022db
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80258116"
---
# <a name="property-iteration-in-arm-templates"></a>ARM şablonlarında özellik yinelemesi

Bu makalede, Azure Kaynak Yöneticisi (ARM) şablonunuzda birden fazla özellik örneğinin nasıl oluşturulabileceğiniz gösterilmektedir. Şablonunuzdaki bir kaynağın özellikler bölümüne **kopya** öğesi ekleyerek, dağıtım sırasında bir mülkün öğe sayısını dinamik olarak ayarlayabilirsiniz. Ayrıca şablon sözdizimini yinelemek zorunda kalmaktan da kaçının.

Ayrıca [kaynaklar,](copy-resources.md) [değişkenler](copy-variables.md)ve [çıktıları](copy-outputs.md)ile kopya kullanabilirsiniz.

## <a name="property-iteration"></a>Özellik yineleme

Kopyalama öğesi aşağıdaki genel biçime sahiptir:

```json
"copy": [
  {
    "name": "<name-of-loop>",
    "count": <number-of-iterations>,
    "input": <values-for-the-property>
  }
]
```

**Ad**için, oluşturmak istediğiniz kaynak özelliğinin adını sağlayın. **Sayım** özelliği, özellik için istediğiniz yineleme sayısını belirtir.

**Giriş** özelliği, yinelemek istediğiniz özellikleri belirtir. **Giriş** özelliğindeki değerden oluşturulmuş bir dizi öğe oluşturursunuz.

Aşağıdaki örnek, sanal `copy` bir makinede dataDisks özelliğine nasıl uygulanacağı gösterilmektedir:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "numberOfDataDisks": {
      "type": "int",
      "minValue": 0,
      "maxValue": 16,
      "defaultValue": 16,
      "metadata": {
        "description": "The number of dataDisks to create."
      }
    },
    ...
  },
  "resources": [
    {
      "type": "Microsoft.Compute/virtualMachines",
      "apiVersion": "2017-03-30",
      ...
      "properties": {
        "storageProfile": {
          ...
          "copy": [
            {
              "name": "dataDisks",
              "count": "[parameters('numberOfDataDisks')]",
              "input": {
                "diskSizeGB": 1023,
                "lun": "[copyIndex('dataDisks')]",
                "createOption": "Empty"
              }
            }
          ]
        }
      }
    }
  ]
}
```

Bir özellik `copyIndex` yinelemeiçinde kullanırken, yinelemenin adını sağlamanız gerektiğine dikkat edin.

> [!NOTE]
> Özellik yinelemesi de bir ofset bağımsız değişkeni destekler. Ofset, copyIndex('dataDisks', 1 gibi yinelemenin adından sonra gelmelidir.
>

Kaynak Yöneticisi dağıtım `copy` sırasında diziyi genişletir. Dizinin adı özelliğin adı olur. Giriş değerleri nesne özellikleri olur. Dağıtılan şablon şu şekilde olur:

```json
{
  "name": "examplevm",
  "type": "Microsoft.Compute/virtualMachines",
  "apiVersion": "2017-03-30",
  "properties": {
    "storageProfile": {
      "dataDisks": [
        {
          "lun": 0,
          "createOption": "Empty",
          "diskSizeGB": 1023
        },
        {
          "lun": 1,
          "createOption": "Empty",
          "diskSizeGB": 1023
        },
        {
          "lun": 2,
          "createOption": "Empty",
          "diskSizeGB": 1023
        }
      ],
      ...
```

Kaynak için birden fazla özellik belirtebilmeniz için kopyalama öğesi bir dizidir.

```json
{
  "type": "Microsoft.Network/loadBalancers",
  "apiVersion": "2017-10-01",
  "name": "exampleLB",
  "properties": {
    "copy": [
      {
        "name": "loadBalancingRules",
        "count": "[length(parameters('loadBalancingRules'))]",
        "input": {
          ...
        }
      },
      {
        "name": "probes",
        "count": "[length(parameters('loadBalancingRules'))]",
        "input": {
          ...
        }
      }
    ]
  }
}
```

Kaynak ve özellik yinelemeyi birlikte kullanabilirsiniz. Ada göre özellik yinelemebaşvuru.

```json
{
  "type": "Microsoft.Network/virtualNetworks",
  "apiVersion": "2018-04-01",
  "name": "[concat(parameters('vnetname'), copyIndex())]",
  "copy":{
    "count": 2,
    "name": "vnetloop"
  },
  "location": "[resourceGroup().location]",
  "properties": {
    "addressSpace": {
      "addressPrefixes": [
        "[parameters('addressPrefix')]"
      ]
    },
    "copy": [
      {
        "name": "subnets",
        "count": 2,
        "input": {
          "name": "[concat('subnet-', copyIndex('subnets'))]",
          "properties": {
            "addressPrefix": "[variables('subnetAddressPrefix')[copyIndex('subnets')]]"
          }
        }
      }
    ]
  }
}
```

## <a name="copy-limits"></a>Kopyalama sınırları

Sayım 800'i geçemez.

Sayı negatif sayı olamaz. Azure PowerShell 2.6 veya sonraki sürümlerle, Azure CLI 2.0.74 veya sonraki sürümlerle veya REST API sürümü **2019-05-10** veya daha yeni bir şablon uyguluyorsanız, sayıyı sıfıra ayarlayabilirsiniz. PowerShell, CLI ve REST API'nin önceki sürümleri nde sayım için sıfır desteklenmez.

## <a name="example-templates"></a>Örnek şablonlar

Aşağıdaki örnek, bir özellik için birden fazla değer oluşturmak için ortak bir senaryo gösterir.

|Şablon  |Açıklama  |
|---------|---------|
|[Değişken sayıda veri diski içeren VM dağıtımı](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-windows-copy-datadisks) |Sanal bir makineyle birkaç veri diski dağıtıyor. |

## <a name="next-steps"></a>Sonraki adımlar

* Bir öğreticiye geçmek için [Bkz. Öğretici: ARM şablonlarını kullanarak birden çok kaynak örneği oluşturun.](template-tutorial-create-multiple-instances.md)
* Kopyalama öğesinin diğer kullanımları için bkz:
  * [ARM şablonlarında kaynak yinelemesi](copy-resources.md)
  * [ARM şablonlarında değişken yineleme](copy-variables.md)
  * [ARM şablonlarında çıkış yinelemesi](copy-outputs.md)
* Şablonun bölümleri hakkında bilgi edinmek istiyorsanız, [Bkz.](template-syntax.md)
* Şablonunuzu nasıl dağıtılayarak dağıtılayacaklarını öğrenmek için [bkz.](deploy-powershell.md)

