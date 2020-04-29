---
title: Azure ölçek kümesi şablonunda özel bir görüntüye başvurma
description: Var olan bir Azure sanal makine ölçek kümesi şablonuna nasıl özel görüntü ekleneceğini öğrenin
author: mimckitt
tags: azure-resource-manager
ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.topic: conceptual
ms.date: 04/26/2018
ms.author: mimckitt
ms.openlocfilehash: 3965090239949b5e1116ceebe427728e49ffafe4
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81273707"
---
# <a name="add-a-custom-image-to-an-azure-scale-set-template"></a>Azure ölçek kümesi şablonuna özel görüntü ekleme

Bu makalede, [temel ölçek kümesi şablonunun](virtual-machine-scale-sets-mvss-start.md) özel görüntüden dağıtılması için nasıl değiştirileceği gösterilmektedir.

## <a name="change-the-template-definition"></a>Şablon tanımını değiştirme
Önceki bir [makalede](virtual-machine-scale-sets-mvss-start.md) temel bir ölçek kümesi şablonu oluşturduk. Şimdi bu şablonu kullanacağız ve özel görüntüden ölçek kümesi dağıtan bir şablon oluşturacak şekilde değiştirirsiniz.  

### <a name="creating-a-managed-disk-image"></a>Yönetilen disk görüntüsü oluşturma

Zaten özel bir yönetilen disk görüntünüz (türünde `Microsoft.Compute/images`bir kaynağınız) varsa, bu bölümü atlayabilirsiniz.

İlk olarak, Azure `sourceImageVhdUri` depolama 'daki Genelleştirilmiş blob 'un dağıtım yapılacak özel görüntüyü içeren URI 'si olan bir parametre ekleyin.


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

Ardından, URI `sourceImageVhdUri`konumunda bulunan Genelleştirilmiş blobu `Microsoft.Compute/images`temel alan yönetilen disk görüntüsü olan türünde bir kaynak ekleyin. Bu görüntü, onu kullanan ölçek kümesiyle aynı bölgede olmalıdır. Görüntünün özelliklerinde işletim sistemi türünü, Blobun konumunu ( `sourceImageVhdUri` parametresinden) ve depolama hesabı türünü belirtin:

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

Ölçek kümesi kaynağında, ölçek kümesi bu görüntüden `dependsOn` dağıtmayı denemeden önce görüntünün oluşturulduğundan emin olmak için özel görüntüye başvuran bir yan tümce ekleyin:

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

`imageReference` Ölçek `storageProfile`kümesinde, bir platform görüntüsünün Yayımcı, teklif, SKU 'su ve sürümünü belirtmek yerine, `id` `Microsoft.Compute/images` kaynağın şunu belirtin:

```json
         "virtualMachineProfile": {
           "storageProfile": {
             "imageReference": {
              "id": "[resourceId('Microsoft.Compute/images', 'myCustomImage')]"
             }
           },
           "osProfile": {
```

Bu örnekte, aynı şablonda oluşturulan `resourceId` görüntünün kaynak kimliğini almak için işlevini kullanın. Yönetilen disk görüntüsünü önceden oluşturduysanız bunun yerine söz konusu görüntünün KIMLIĞINI sağlamanız gerekir. Bu KIMLIK şu biçimde olmalıdır: `/subscriptions/<subscription-id>resourceGroups/<resource-group-name>/providers/Microsoft.Compute/images/<image-name>`.


## <a name="next-steps"></a>Sonraki Adımlar

[!INCLUDE [mvss-next-steps-include](../../includes/mvss-next-steps.md)]
