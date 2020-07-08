---
title: Azure ölçek kümesi şablonunda var olan bir sanal ağa başvurma
description: Var olan bir Azure sanal makine ölçek kümesi şablonuna nasıl sanal ağ ekleneceğini öğrenin
author: ju-shim
ms.author: jushiman
ms.topic: how-to
ms.service: virtual-machine-scale-sets
ms.subservice: networking
ms.date: 04/26/2019
ms.reviewer: mimckitt
ms.custom: mimckitt
ms.openlocfilehash: fab6e6742fa43e1e38ee661b67896ae4aa11b3ed
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "83124831"
---
# <a name="add-reference-to-an-existing-virtual-network-in-an-azure-scale-set-template"></a>Azure ölçek kümesi şablonunda var olan bir sanal ağa başvuru ekleme

Bu makalede, [temel ölçek kümesi şablonunun](virtual-machine-scale-sets-mvss-start.md) yeni bir tane oluşturmak yerine var olan bir sanal ağa dağıtılması için nasıl değiştirileceği gösterilmektedir.

## <a name="change-the-template-definition"></a>Şablon tanımını değiştirme

Önceki bir [makalede](virtual-machine-scale-sets-mvss-start.md) temel bir ölçek kümesi şablonu oluşturduk. Şimdi bu şablonu kullanacağız ve var olan bir sanal ağa ölçek kümesi dağıtan bir şablon oluşturacak şekilde değiştirirsiniz. 

Önce bir parametre ekleyin `subnetId` . Bu dize ölçek kümesi yapılandırmasına geçirilir ve ölçek kümesinin sanal makineleri dağıtmak için önceden oluşturulmuş alt ağı belirlemesine izin verir. Bu dize şu biçimde olmalıdır:`/subscriptions/<subscription-id>resourceGroups/<resource-group-name>/providers/Microsoft.Network/virtualNetworks/<virtual-network-name>/subnets/<subnet-name>`

Örneğin, ölçek kümesini ada `myvnet` , alt ağa, kaynak grubuna ve aboneliğe sahip mevcut bir sanal ağa dağıtmak için `mysubnet` `myrg` `00000000-0000-0000-0000-000000000000` , SubnetID şöyle olacaktır: `/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myrg/providers/Microsoft.Network/virtualNetworks/myvnet/subnets/mysubnet` .

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

Sonra, `resources` var olan bir sanal ağı kullanırken ve yenisini dağıtmanız gerekmiyorsa sanal ağ kaynağını diziden silin.

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

Son olarak, `subnetId` Kullanıcı tarafından ayarlanan parametreyi geçirin ( `resourceId` temel uygun ölçek kümesi şablonunun yaptığı aynı dağıtımda bir VNET 'in kimliğini almak için kullanmak yerine).

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
