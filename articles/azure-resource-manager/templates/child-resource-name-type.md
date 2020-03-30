---
title: Şablonlarda alt kaynaklar
description: Azure Kaynak Yöneticisi şablonunda alt kaynaklar için ad ve yazının nasıl ayarlanır olduğunu açıklar.
ms.topic: conceptual
ms.date: 08/26/2019
ms.openlocfilehash: 6de2b476fe19a057a62e4a54963dd8fde0d11579
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77207902"
---
# <a name="set-name-and-type-for-child-resources"></a>Alt kaynaklar için ad ve tür belirleme

Alt kaynaklar yalnızca başka bir kaynak bağlamında var olan kaynaklardır. Örneğin, [sanal makine uzantısı](/azure/templates/microsoft.compute/2019-03-01/virtualmachines/extensions) sanal bir [makine](/azure/templates/microsoft.compute/2019-03-01/virtualmachines)olmadan var olamaz. Uzantı kaynağı sanal makinenin bir alt alanıdır.

Kaynak Yemliği şablonunda, alt kaynağı üst kaynak içinde veya üst kaynağın dışında belirtebilirsiniz. Aşağıdaki örnekte, üst kaynağın kaynak özelliği içinde yer alan alt kaynak gösterilmektedir.

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

Sonraki örnek, alt kaynağı üst kaynağın dışında gösterir. Üst kaynak aynı şablonda dağıtılmıyorsa veya birden fazla alt kaynak oluşturmak için [kopya](copy-resources.md) kullanmak istiyorsanız bu yaklaşımı kullanabilirsiniz.

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

Kaynak adı ve türü için sağladığınız değerler, alt kaynağın ana kaynağın içinde veya dışında tanımlanıp tanımlanmadığına bağlı olarak değişir.

## <a name="within-parent-resource"></a>Üst kaynak içinde

Üst kaynak türü içinde tanımlandığında, tür ve ad değerlerini kesikler olmadan tek bir sözcük olarak biçimlendirin.

```json
"type": "{child-resource-type}",
"name": "{child-resource-name}",
```

Aşağıdaki örnekte sanal ağ ve bir alt ağ gösterilmektedir. Alt ağın sanal ağ için kaynak dizisine dahil edildiğine dikkat edin. Ad **Subnet1** olarak ayarlanır ve tür **alt ağlara**ayarlanır. Alt kaynak dağıtılmadan önce üst kaynağın bulunması gerektiğinden, alt kaynak ana kaynağa bağımlı olarak işaretlenir.

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

Tam kaynak türü hala **Microsoft.Network/virtualNetworks/subnets**olduğunu. **Microsoft.Network/virtualNetworks/** sağlamazsınız, çünkü üst kaynak türünden kabul edilir.

Alt kaynak adı **Subnet1** olarak ayarlanır, ancak tam ad üst adı içerir. **VNet1'i** ana kaynaktan kabul ettiği için sağlamazsınız.

## <a name="outside-parent-resource"></a>Dış üst kaynak

Ana kaynağın dışında tanımlandığında, türü ve kesikleri üst türü ve adı içerecek şekilde biçimlendirin.

```json
"type": "{resource-provider-namespace}/{parent-resource-type}/{child-resource-type}",
"name": "{parent-resource-name}/{child-resource-name}",
```

Aşağıdaki örnekte, her ikisi de kök düzeyinde tanımlanan bir sanal ağ ve alt ağ gösterilmektedir. Alt ağın sanal ağ için kaynak dizisine dahil olmadığına dikkat edin. Ad **VNet1/Subnet1** olarak ayarlanır ve türü **Microsoft.Network/virtualNetworks/subnets**olarak ayarlanır. Alt kaynak dağıtılmadan önce üst kaynağın bulunması gerektiğinden, alt kaynak ana kaynağa bağımlı olarak işaretlenir.

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

* Azure Kaynak Yöneticisi şablonları oluşturma hakkında bilgi edinmek için [bkz.](template-syntax.md)

* Kaynağa başvururken kaynak adının biçimi hakkında bilgi edinmek için [başvuru işlevine](template-functions-resource.md#reference)bakın.
