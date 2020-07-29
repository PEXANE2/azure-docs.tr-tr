---
title: Azure ölçek kümesi şablonunda özel bir görüntüye başvurma
description: Var olan bir Azure sanal makine ölçek kümesi şablonuna nasıl özel görüntü ekleneceğini öğrenin
author: cynthn
ms.author: cynthn
ms.topic: how-to
ms.service: virtual-machine-scale-sets
ms.subservice: imaging
ms.date: 04/26/2018
ms.reviewer: akjosh
ms.custom: akjosh
ms.openlocfilehash: 5ed9ee79dde73e738417031b928a675ea913179c
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "83124916"
---
# <a name="add-a-custom-image-to-an-azure-scale-set-template"></a>Azure ölçek kümesi şablonuna özel görüntü ekleme

Bu makalede, [temel ölçek kümesi şablonunun](virtual-machine-scale-sets-mvss-start.md) özel görüntüden dağıtılması için nasıl değiştirileceği gösterilmektedir.

## <a name="change-the-template-definition"></a>Şablon tanımını değiştirme
Önceki bir [makalede](virtual-machine-scale-sets-mvss-start.md) temel bir ölçek kümesi şablonu oluşturduk. Şimdi bu şablonu kullanacağız ve özel görüntüden ölçek kümesi dağıtan bir şablon oluşturacak şekilde değiştirirsiniz.  

### <a name="creating-a-managed-disk-image"></a>Yönetilen disk görüntüsü oluşturma

Zaten özel bir yönetilen disk görüntünüz (türünde bir kaynağınız `Microsoft.Compute/images` ) varsa, bu bölümü atlayabilirsiniz.

İlk olarak, `sourceImageVhdUri` Azure depolama 'daki Genelleştirilmiş blob 'un dağıtım yapılacak özel görüntüyü içeren URI 'si olan bir parametre ekleyin.


```diff
     },
     "adminPassword": {
       "type": "securestring"
+    },
+    "sourceImageVhdUri": {
+      "type": "string",
+      "metadata": {
+        "description": "The source of the generalized blob containing the custom image"
+      }
     }
   },
   "variables": {},
```

Ardından, `Microsoft.Compute/images` URI konumunda bulunan Genelleştirilmiş blobu temel alan yönetilen disk görüntüsü olan türünde bir kaynak ekleyin `sourceImageVhdUri` . Bu görüntü, onu kullanan ölçek kümesiyle aynı bölgede olmalıdır. Görüntünün özelliklerinde işletim sistemi türünü, Blobun konumunu ( `sourceImageVhdUri` parametresinden) ve depolama hesabı türünü belirtin:

```diff
   "resources": [
     {
+      "type": "Microsoft.Compute/images",
+      "apiVersion": "2019-03-01",
+      "name": "myCustomImage",
+      "location": "[resourceGroup().location]",
+      "properties": {
+        "storageProfile": {
+          "osDisk": {
+            "osType": "Linux",
+            "osState": "Generalized",
+            "blobUri": "[parameters('sourceImageVhdUri')]",
+            "storageAccountType": "Standard_LRS"
+          }
+        }
+      }
+    },
+    {
       "type": "Microsoft.Network/virtualNetworks",
       "name": "myVnet",
       "location": "[resourceGroup().location]",

```

Ölçek kümesi kaynağında, `dependsOn` Ölçek kümesi bu görüntüden dağıtmayı denemeden önce görüntünün oluşturulduğundan emin olmak için özel görüntüye başvuran bir yan tümce ekleyin:

```diff
       "location": "[resourceGroup().location]",
       "apiVersion": "2019-03-01-preview",
       "dependsOn": [
-        "Microsoft.Network/virtualNetworks/myVnet"
+        "Microsoft.Network/virtualNetworks/myVnet",
+        "Microsoft.Compute/images/myCustomImage"
       ],
       "sku": {
         "name": "Standard_A1",

```

### <a name="changing-scale-set-properties-to-use-the-managed-disk-image"></a>Ölçek kümesi özelliklerini yönetilen disk görüntüsünü kullanacak şekilde değiştirme

`imageReference`Ölçek kümesinde `storageProfile` , bir platform görüntüsünün Yayımcı, teklif, SKU 'su ve sürümünü belirtmek yerine, kaynağın şunu belirtin `id` `Microsoft.Compute/images` :

```json
         "virtualMachineProfile": {
           "storageProfile": {
             "imageReference": {
              "id": "[resourceId('Microsoft.Compute/images', 'myCustomImage')]"
             }
           },
           "osProfile": {
```

Bu örnekte, `resourceId` aynı şablonda oluşturulan görüntünün kaynak kimliğini almak için işlevini kullanın. Yönetilen disk görüntüsünü önceden oluşturduysanız bunun yerine söz konusu görüntünün KIMLIĞINI sağlamanız gerekir. Bu KIMLIK şu biçimde olmalıdır: `/subscriptions/<subscription-id>resourceGroups/<resource-group-name>/providers/Microsoft.Compute/images/<image-name>` .


## <a name="next-steps"></a>Sonraki Adımlar

[!INCLUDE [mvss-next-steps-include](../../includes/mvss-next-steps.md)]
