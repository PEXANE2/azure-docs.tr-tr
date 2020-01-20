---
title: Azure ölçek kümesi şablonunda var olan bir sanal ağa başvurma
description: Var olan bir Azure sanal makine ölçek kümesi şablonuna nasıl sanal ağ ekleneceğini öğrenin
author: mayanknayar
tags: azure-resource-manager
ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.topic: conceptual
ms.date: 04/26/2019
ms.author: manayar
ms.openlocfilehash: e725e75b8b19fd8b3295226639b5e5aeb3736e34
ms.sourcegitcommit: 5397b08426da7f05d8aa2e5f465b71b97a75550b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2020
ms.locfileid: "76275542"
---
# <a name="add-reference-to-an-existing-virtual-network-in-an-azure-scale-set-template"></a>Azure ölçek kümesi şablonunda var olan bir sanal ağa başvuru ekleme

Bu makalede, [temel ölçek kümesi şablonunun](virtual-machine-scale-sets-mvss-start.md) yeni bir tane oluşturmak yerine var olan bir sanal ağa dağıtılması için nasıl değiştirileceği gösterilmektedir.

## <a name="change-the-template-definition"></a>Şablon tanımını değiştirme

Önceki bir [makalede](virtual-machine-scale-sets-mvss-start.md) temel bir ölçek kümesi şablonu oluşturduk. Şimdi bu şablonu kullanacağız ve var olan bir sanal ağa ölçek kümesi dağıtan bir şablon oluşturacak şekilde değiştirirsiniz. 

İlk olarak bir `subnetId` parametresi ekleyin. Bu dize ölçek kümesi yapılandırmasına geçirilir ve ölçek kümesinin sanal makineleri dağıtmak için önceden oluşturulmuş alt ağı belirlemesine izin verir. Bu dize şu biçimde olmalıdır: `/subscriptions/<subscription-id>resourceGroups/<resource-group-name>/providers/Microsoft.Network/virtualNetworks/<virtual-network-name>/subnets/<subnet-name>`

Örneğin, ölçek kümesini ad `myvnet`, alt ağ `mysubnet`, kaynak grubu `myrg`ve abonelik `00000000-0000-0000-0000-000000000000`olan mevcut bir sanal ağa dağıtmak için, SubnetID şöyle olacaktır: `/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myrg/providers/Microsoft.Network/virtualNetworks/myvnet/subnets/mysubnet`.

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

Ardından, var olan bir sanal ağı kullanırken ve yenisini dağıtmanız gerekmiyorsa, sanal ağ kaynağını `resources` dizisinden silin.

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

Şablon dağıtılmadan önce sanal ağ zaten var, bu nedenle ölçek kümesinden sanal ağa bağımlı bir yan tümce belirtmeniz gerekmez. Aşağıdaki satırları silin:

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

Son olarak, Kullanıcı tarafından ayarlanan `subnetId` parametresini geçirin (temel uygun ölçek kümesi şablonunun yaptığı, aynı dağıtımdaki VNET KIMLIĞINI almak için `resourceId` kullanmak yerine).

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
