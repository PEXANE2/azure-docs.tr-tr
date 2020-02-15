---
title: Bir özelliğin birden çok örneğini tanımlama
description: Bir kaynak üzerinde bir özellik oluştururken birden çok kez yinelemek için Azure Resource Manager şablonunda kopyalama işlemini kullanın.
ms.topic: conceptual
ms.date: 02/13/2020
ms.openlocfilehash: b1e31f981f361b4cfbe7e7930f2c70bfce8b8656
ms.sourcegitcommit: 2823677304c10763c21bcb047df90f86339e476a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2020
ms.locfileid: "77210872"
---
# <a name="property-iteration-in-azure-resource-manager-templates"></a>Azure Resource Manager şablonlarda Özellik yinelemesi

Bu makalede, Azure Resource Manager şablonunuzda bir özelliğin birden fazla örneğini nasıl oluşturacağınız gösterilmektedir. Şablonunuzda bir kaynağın Özellikler bölümüne **Copy** öğesini ekleyerek, dağıtım sırasında bir özelliğin öğe sayısını dinamik olarak ayarlayabilirsiniz. Ayrıca, şablon söz dizimini yinelemek zorunda kalmaktan kaçının.

Ayrıca, [kaynak](copy-resources.md) ve [değişkenlerle](copy-variables.md)kopyalama kullanabilirsiniz.

## <a name="property-iteration"></a>Özellik yineleme

Copy öğesi aşağıdaki genel biçime sahiptir:

```json
"copy": [
  {
    "name": "<name-of-loop>",
    "count": <number-of-iterations>,
    "input": <values-for-the-property>
  }
]
```

**Ad**için oluşturmak istediğiniz kaynak özelliğinin adını sağlayın. **Count** özelliği, özelliği için istediğiniz yineleme sayısını belirtir.

**Input** özelliği, yinelemek istediğiniz özellikleri belirtir. **Giriş** özelliğindeki değerden oluşturulan bir dizi öğe oluşturun.

Aşağıdaki örnek, `copy` bir sanal makinede dataDisks özelliğine nasıl uygulanacağını gösterir:

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

Özellik yinelemesi içinde `copyIndex` kullanırken yinelemenin adını belirtmeniz gerektiğini unutmayın.

> [!NOTE]
> Özellik yinelemesi de bir konum bağımsız değişkenini destekler. Konum, Copyındex (' dataDisks ', 1) gibi yinelemenin adından sonra gelmelidir.
>

Kaynak Yöneticisi, dağıtım sırasında `copy` dizisini genişletir. Dizinin adı, özelliğin adı olur. Giriş değerleri nesne özellikleri olur. Dağıtılan şablon şu şekilde olur:

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

Copy öğesi bir dizidir, böylece kaynak için birden fazla özellik belirtebilirsiniz.

```json
{
  "type": "Microsoft.Network/loadBalancers",
  "apiVersion": "2017-10-01",
  "name": "examleLB",
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

Kaynak ve özellik yinelemesini birlikte kullanabilirsiniz. Özellik yinelemesine ada göre başvurun.

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

## <a name="copy-limits"></a>Sınırları Kopyala

Sayım 800 ' i aşamaz.

Sayı negatif bir sayı olamaz. Azure PowerShell 2,6 veya üzeri, Azure CLı 2.0.74 veya üzeri ya da REST API sürüm **2019-05-10** veya üzeri bir şablon dağıtırsanız, sayıyı sıfıra ayarlayabilirsiniz. PowerShell, CLı ve REST API 'nin önceki sürümleri Count için sıfırı desteklemez.

## <a name="example-templates"></a>Örnek şablonları

Aşağıdaki örnek, bir özellik için birden fazla değer oluşturmak için ortak bir senaryoyu gösterir.

|Şablon  |Açıklama  |
|---------|---------|
|[Değişken sayıda veri diskine sahip VM dağıtımı](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-windows-copy-datadisks) |Bir sanal makine ile birden fazla veri diski dağıtır. |

## <a name="next-steps"></a>Sonraki adımlar

* Öğreticiye gitmek için bkz. [öğretici: Kaynak Yöneticisi şablonları kullanarak birden çok kaynak örneği oluşturma](template-tutorial-create-multiple-instances.md).
* Copy öğesinin diğer kullanımları için, Azure Resource Manager şablonlarındaki [Azure Resource Manager şablonlarda kaynak yinelemesi](copy-resources.md) ve [değişken yinelemesi](copy-variables.md)' ne bakın.
* Bir şablonun bölümleri hakkında daha fazla bilgi edinmek istiyorsanız, bkz. [yazma Azure Resource Manager şablonları](template-syntax.md).
* Şablonunuzu dağıtmayı öğrenmek için bkz. [Azure Resource Manager şablonuyla uygulama dağıtma](deploy-powershell.md).

