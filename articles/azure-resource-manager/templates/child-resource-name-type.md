---
title: Şablonlarda alt kaynaklar
description: Bir Azure Resource Manager şablonunda (ARM şablonu) alt kaynaklar için ad ve tür ayarlamayı açıklar.
ms.topic: conceptual
ms.date: 12/21/2020
ms.openlocfilehash: a950d72751b829c0a2aa3ba5ca27316a0544d9cc
ms.sourcegitcommit: f6f928180504444470af713c32e7df667c17ac20
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/07/2021
ms.locfileid: "97963921"
---
# <a name="set-name-and-type-for-child-resources"></a>Alt kaynaklar için ad ve tür ayarla

Alt kaynaklar yalnızca başka bir kaynağın bağlamında bulunan kaynaklardır. Örneğin, sanal makine [Uzantısı](/azure/templates/microsoft.compute/virtualmachines/extensions) bir [sanal makine](/azure/templates/microsoft.compute/virtualmachines)olmadan mevcut olamaz. Uzantı kaynağı, sanal makinenin bir alt öğesidir.

Her üst kaynak, alt kaynaklar olarak yalnızca belirli kaynak türlerini kabul eder. Alt kaynak için kaynak türü, üst kaynak için kaynak türünü içerir. Örneğin, `Microsoft.Web/sites/config` ve öğelerinin `Microsoft.Web/sites/extensions` her ikisi de alt kaynaklarıdır `Microsoft.Web/sites` . Kabul edilen kaynak türleri, üst kaynağın [şablon şemasında](https://github.com/Azure/azure-resource-manager-schemas) belirtilir.

Bir Azure Resource Manager şablonunda (ARM şablonu), alt kaynağı üst kaynak içinde veya üst kaynağın dışında belirtebilirsiniz. Aşağıdaki örnek, üst kaynağın Resources özelliği içinde yer alan alt kaynağı gösterir.

```json
"resources": [
  {
    <parent-resource>
    "resources": [
      <child-resource>
    ]
  }
]
```

Alt kaynaklar yalnızca beş düzey derinlikli tanımlanmış olabilir.

Sonraki örnekte alt kaynak, üst kaynağın dışında gösterilmektedir. Üst kaynak aynı şablonda dağıtılmamışsa veya birden fazla alt kaynak oluşturmak için [kopyalama](copy-resources.md) kullanmak istiyorsanız bu yaklaşımı kullanabilirsiniz.

```json
"resources": [
  {
    <parent-resource>
  },
  {
    <child-resource>
  }
]
```

Kaynak adı ve türü için sağladığınız değerler, alt kaynağın üst kaynağın içinde mi yoksa dışında mı tanımlandığına göre değişir.

## <a name="within-parent-resource"></a>Üst kaynak içinde

Üst kaynak türü içinde tanımlandığında, tür ve ad değerlerini eğik çizgiler olmadan tek bir sözcük olarak biçimlendirin.

```json
"type": "{child-resource-type}",
"name": "{child-resource-name}",
```

Aşağıdaki örnekte bir sanal ağ ve bir alt ağ gösterilmektedir. Alt ağın, sanal ağ için kaynaklar dizisine dahil edildiğini unutmayın. Ad **Subnet1** olarak ayarlanır ve tür **alt ağlar** olarak ayarlanır. Alt kaynak dağıtılmadan önce üst kaynağın mevcut olması gerektiğinden, alt kaynak üst kaynağa bağımlı olarak işaretlenir.

```json
"resources": [
  {
    "type": "Microsoft.Network/virtualNetworks",
    "apiVersion": "2018-10-01",
    "name": "VNet1",
    "location": "[parameters('location')]",
    "properties": {
      "addressSpace": {
        "addressPrefixes": [
          "10.0.0.0/16"
        ]
      }
    },
    "resources": [
      {
        "type": "subnets",
        "apiVersion": "2018-10-01",
        "name": "Subnet1",
        "location": "[parameters('location')]",
        "dependsOn": [
          "VNet1"
        ],
        "properties": {
          "addressPrefix": "10.0.0.0/24"
        }
      }
    ]
  }
]
```

Tam kaynak türü hala `Microsoft.Network/virtualNetworks/subnets` . `Microsoft.Network/virtualNetworks/`Bu, üst kaynak türünden olduğu için sağlamamazsınız.

Alt kaynak adı **Subnet1** olarak ayarlanır ancak tam ad üst adı içerir. Üst kaynaktan varsaydığı için **VNet1** sağlamamazsınız.

## <a name="outside-parent-resource"></a>Üst kaynak dışında

Üst kaynağın dışında tanımlandığında, üst tür ve adı içerecek şekilde türü ve eğik çizgileri biçimlendirin.

```json
"type": "{resource-provider-namespace}/{parent-resource-type}/{child-resource-type}",
"name": "{parent-resource-name}/{child-resource-name}",
```

Aşağıdaki örnekte, kök düzeyinde tanımlanan bir sanal ağ ve alt ağ gösterilmektedir. Alt ağın, sanal ağ için kaynaklar dizisine dahil edilmediğini unutmayın. Ad **VNet1/Subnet1** olarak ayarlanır ve tür olarak ayarlanır `Microsoft.Network/virtualNetworks/subnets` . Alt kaynak dağıtılmadan önce üst kaynağın mevcut olması gerektiğinden, alt kaynak üst kaynağa bağımlı olarak işaretlenir.

```json
"resources": [
  {
    "type": "Microsoft.Network/virtualNetworks",
    "apiVersion": "2018-10-01",
    "name": "VNet1",
    "location": "[parameters('location')]",
    "properties": {
      "addressSpace": {
        "addressPrefixes": [
          "10.0.0.0/16"
        ]
      }
    }
  },
  {
    "type": "Microsoft.Network/virtualNetworks/subnets",
    "apiVersion": "2018-10-01",
    "location": "[parameters('location')]",
    "name": "VNet1/Subnet1",
    "dependsOn": [
      "VNet1"
    ],
    "properties": {
      "addressPrefix": "10.0.0.0/24"
    }
  }
]
```

## <a name="next-steps"></a>Sonraki adımlar

* ARM şablonları oluşturma hakkında bilgi edinmek için bkz. [ARM şablonlarının yapısını ve sözdizimini anlayın](template-syntax.md).
* Kaynağa başvururken kaynak adının biçimi hakkında bilgi edinmek için [başvuru işlevine](template-functions-resource.md#reference)bakın.
