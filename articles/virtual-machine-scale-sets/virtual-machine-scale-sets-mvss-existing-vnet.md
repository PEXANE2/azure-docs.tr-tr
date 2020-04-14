---
title: Azure ölçeği kümesi şablonundaki varolan sanal ağa başvuru
description: Varolan Azure Sanal Makine Ölçeği Seti şablonuna sanal ağ eklemeyi öğrenin
author: mimckitt
tags: azure-resource-manager
ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.topic: conceptual
ms.date: 04/26/2019
ms.author: mimckitt
ms.openlocfilehash: 83328a31dad8009c28e146c81b24d6d9244f88a8
ms.sourcegitcommit: 530e2d56fc3b91c520d3714a7fe4e8e0b75480c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81273673"
---
# <a name="add-reference-to-an-existing-virtual-network-in-an-azure-scale-set-template"></a>Azure ölçeği kümesi şablonunda varolan bir sanal ağa başvuru ekleme

Bu makalede, yeni bir tane oluşturmak yerine varolan bir sanal ağa dağıtmak için [temel ölçek kümesi şablonu](virtual-machine-scale-sets-mvss-start.md) nasıl değiştirilen gösterir.

## <a name="change-the-template-definition"></a>Şablon tanımını değiştirme

Önceki [bir makalede](virtual-machine-scale-sets-mvss-start.md) temel bir ölçek kümesi şablonu oluşturmuştuk. Şimdi bu önceki şablonu kullanacağız ve varolan bir sanal ağa ölçek kümesi dağıtan bir şablon oluşturmak için değiştireceğiz. 

İlk olarak, `subnetId` bir parametre ekleyin. Bu dize ölçek kümesi yapılandırmasına geçirilir ve ölçek kümesinin sanal makineleri dağıtmak için önceden oluşturulmuş alt ağı tanımlamasına olanak sağlar. Bu dize biçiminde olmalıdır:`/subscriptions/<subscription-id>resourceGroups/<resource-group-name>/providers/Microsoft.Network/virtualNetworks/<virtual-network-name>/subnets/<subnet-name>`

Örneğin, ölçek kümesini `myvnet`ad, alt ağ, `mysubnet`kaynak grubu `myrg`ve abonelik `00000000-0000-0000-0000-000000000000`içeren varolan bir sanal `/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myrg/providers/Microsoft.Network/virtualNetworks/myvnet/subnets/mysubnet`ağa dağıtmak için subnetId: .

```diff
     },
     "adminPassword": {
       "type": "securestring"
+    },
+    "subnetId": {
+      "type": "string"
     }
   },
```

Ardından, varolan bir sanal `resources` ağ kullandığınızdan ve yeni bir ağ dağıtmanız gerekmeden sanal ağ kaynağını diziden silin.

```diff
   "variables": {},
   "resources": [
-    {
-      "type": "Microsoft.Network/virtualNetworks",
-      "name": "myVnet",
-      "location": "[resourceGroup().location]",
-      "apiVersion": "2018-11-01",
-      "properties": {
-        "addressSpace": {
-          "addressPrefixes": [
-            "10.0.0.0/16"
-          ]
-        },
-        "subnets": [
-          {
-            "name": "mySubnet",
-            "properties": {
-              "addressPrefix": "10.0.0.0/16"
-            }
-          }
-        ]
-      }
-    },
```

Şablon dağıtılmadan önce sanal ağ zaten var olduğundan, ölçek kümesinden sanal ağa bağlı bir yan tümce belirtmeye gerek yoktur. Aşağıdaki satırları silin:

```diff
     {
       "type": "Microsoft.Compute/virtualMachineScaleSets",
       "name": "myScaleSet",
       "location": "[resourceGroup().location]",
       "apiVersion": "2019-03-01",
-      "dependsOn": [
-        "Microsoft.Network/virtualNetworks/myVnet"
-      ],
       "sku": {
         "name": "Standard_A1",
         "capacity": 2
```

Son olarak, `subnetId` kullanıcı tarafından ayarlanan parametreyi `resourceId` geçirin (aynı dağıtımda bir vnet in kimliğini almak yerine, temel uygulanabilir ölçek kümesi şablonu ne yapar).

```diff
                       "name": "myIpConfig",
                       "properties": {
                         "subnet": {
-                          "id": "[concat(resourceId('Microsoft.Network/virtualNetworks', 'myVnet'), '/subnets/mySubnet')]"
+                          "id": "[parameters('subnetId')]"
                         }
                       }
                     }
```




## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [mvss-next-steps-include](../../includes/mvss-next-steps.md)]
