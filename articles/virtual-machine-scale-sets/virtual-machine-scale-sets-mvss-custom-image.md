---
title: Azure ölçeği kümesi şablonundaki özel bir görüntüye başvuru
description: Varolan Azure Sanal Makine Ölçeği Seti şablonuna özel bir görüntü nasıl ekleyeceğinizi öğrenin
author: mayanknayar
tags: azure-resource-manager
ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.topic: conceptual
ms.date: 04/26/2018
ms.author: manayar
ms.openlocfilehash: fd1a567af1c35cf6b659995e998b11a61a526508
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76275585"
---
# <a name="add-a-custom-image-to-an-azure-scale-set-template"></a>Azure ölçek kümesi şablonuna özel görüntü ekleme

Bu makalede, özel görüntüdağıtmak için [temel ölçek kümesi şablonu](virtual-machine-scale-sets-mvss-start.md) nasıl değiştirilen gösterir.

## <a name="change-the-template-definition"></a>Şablon tanımını değiştirme
Önceki [bir makalede](virtual-machine-scale-sets-mvss-start.md) temel bir ölçek kümesi şablonu oluşturmuştuk. Şimdi bu önceki şablonu kullanacağız ve özel bir görüntüden bir ölçek kümesi dağıtan bir şablon oluşturmak için değiştireceğiz.  

### <a name="creating-a-managed-disk-image"></a>Yönetilen disk görüntüsü oluşturma

Zaten özel yönetilen bir disk görüntü (tür `Microsoft.Compute/images`bir kaynak) varsa, o zaman bu bölümü atlayabilirsiniz.

İlk olarak, `sourceImageVhdUri` Azure Depolama'daki genelleştirilmiş blob'a URI olan ve dağıtılabilmek için özel görüntü içeren bir parametre ekleyin.


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

Ardından, URI'de `sourceImageVhdUri` `Microsoft.Compute/images`bulunan genelleştirilmiş blob'a dayalı yönetilen disk görüntüsü olan bir tür kaynağı ekleyin. Bu görüntü, onu kullanan ölçek kümesiyle aynı bölgede olmalıdır. Görüntünün özelliklerinde işletim sistemi türünü, blobun konumunu `sourceImageVhdUri` (parametreden) ve depolama hesabı türünü belirtin:

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

Ölçek kümesi kaynağında, `dependsOn` ölçek kümesi bu görüntüden dağıtmaya çalışmadan önce görüntünün oluşturulduğundan emin olmak için özel görüntüye atıfta bulunan bir yan tümce ekleyin:

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

### <a name="changing-scale-set-properties-to-use-the-managed-disk-image"></a>Yönetilen disk görüntüsünü kullanmak için ölçek kümesi özelliklerini değiştirme

Ölçek `imageReference` kümesinde, `storageProfile`bir platform görüntüsünün yayımcısını, teklifini, sku'sunu ve `id` sürümünü `Microsoft.Compute/images` belirtmek yerine, kaynağın aşağıdakileri belirtin:

```json
         "virtualMachineProfile": {
           "storageProfile": {
             "imageReference": {
              "id": "[resourceId('Microsoft.Compute/images', 'myCustomImage')]"
             }
           },
           "osProfile": {
```

Bu örnekte, `resourceId` aynı şablonda oluşturulan görüntünün kaynak kimliğini almak için işlevi kullanın. Yönetilen disk görüntüsünü önceden oluşturduysanız, bunun yerine bu görüntünün kimliğini sağlamanız gerekir. Bu kimlik formdan olmalıdır: `/subscriptions/<subscription-id>resourceGroups/<resource-group-name>/providers/Microsoft.Compute/images/<image-name>`.


## <a name="next-steps"></a>Sonraki Adımlar

[!INCLUDE [mvss-next-steps-include](../../includes/mvss-next-steps.md)]
