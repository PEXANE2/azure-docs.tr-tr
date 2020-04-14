---
title: Yönetilen disk kullanmak üzere bir ölçek kümesi şablonu dönüştürme
description: Azure Kaynak Yöneticisi sanal makine ölçeği şablonu şablonu yönetilen disk ölçeği kümesi şablonuna dönüştürün.
keywords: sanal makine ölçek kümeleri
author: mimckitt
tags: azure-resource-manager
ms.assetid: bc8c377a-8c3f-45b8-8b2d-acc2d6d0b1e8
ms.service: virtual-machine-scale-sets
ms.topic: conceptual
ms.date: 5/18/2017
ms.author: mimckitt
ms.openlocfilehash: 79fafa8344312294f6df107b88c9b7c571af1969
ms.sourcegitcommit: 530e2d56fc3b91c520d3714a7fe4e8e0b75480c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81270664"
---
# <a name="convert-a-scale-set-template-to-a-managed-disk-scale-set-template"></a>Ölçek kümesi şablonu yönetilen disk ölçeği kümesi şablonuna dönüştürme

Yönetilen diski kullanmayan bir ölçek kümesi oluşturmak için Kaynak Yöneticisi şablonu olan müşteriler, yönetilen diski kullanmak üzere değiştirmek isteyebilir. Bu makalede, örnek Kaynak Yöneticisi şablonları için topluluk odaklı bir repo olan [Azure Quickstart Şablonlarından](https://github.com/Azure/azure-quickstart-templates)çekme isteği örnek olarak kullanarak yönetilen disklerin nasıl kullanılacağı gösterilmektedir. Tam çekme isteği burada görülebilir: [https://github.com/Azure/azure-quickstart-templates/pull/2998](https://github.com/Azure/azure-quickstart-templates/pull/2998), ve diff ilgili bölümleri aşağıda, açıklamalar ile birlikte:

## <a name="making-the-os-disks-managed"></a>İşletim sistemi disklerinin yönetilmesini sağlama

Aşağıdaki diff'te, depolama hesabı ve disk özellikleriyle ilgili çeşitli değişkenler kaldırılır. Depolama hesabı türü artık gerekli değildir (Standard_LRS varsayılandır), ancak istenirse belirtebilirsiniz. Yönetilen diskle yalnızca Standard_LRS ve Premium_LRS desteklenir. Depolama hesabı adları oluşturmak için eski şablonda yeni depolama hesabı soneki, benzersiz dize dizisi ve sa sayısı kullanılmıştır. Yönetilen disk müşteri adına depolama hesapları otomatik olarak oluşturduğundan, bu değişkenler artık yeni şablonda gerekli değildir. Benzer şekilde, yönetilen disk temel depolama blob kapsayıcılarını ve diskleri otomatik olarak adlandırdığından, vhd kapsayıcı adı ve işletim sistemi disk adı artık gerekli değildir.

```diff
   "variables": {
-    "storageAccountType": "Standard_LRS",
     "namingInfix": "[toLower(substring(concat(parameters('vmssName'), uniqueString(resourceGroup().id)), 0, 9))]",
     "longNamingInfix": "[toLower(parameters('vmssName'))]",
-    "newStorageAccountSuffix": "[concat(variables('namingInfix'), 'sa')]",
-    "uniqueStringArray": [
-      "[concat(uniqueString(concat(resourceGroup().id, variables('newStorageAccountSuffix'), '0')))]",
-      "[concat(uniqueString(concat(resourceGroup().id, variables('newStorageAccountSuffix'), '1')))]",
-      "[concat(uniqueString(concat(resourceGroup().id, variables('newStorageAccountSuffix'), '2')))]",
-      "[concat(uniqueString(concat(resourceGroup().id, variables('newStorageAccountSuffix'), '3')))]",
-      "[concat(uniqueString(concat(resourceGroup().id, variables('newStorageAccountSuffix'), '4')))]"
-    ],
-    "saCount": "[length(variables('uniqueStringArray'))]",
-    "vhdContainerName": "[concat(variables('namingInfix'), 'vhd')]",
-    "osDiskName": "[concat(variables('namingInfix'), 'osdisk')]",
     "addressPrefix": "10.0.0.0/16",
     "subnetPrefix": "10.0.0.0/24",
     "virtualNetworkName": "[concat(variables('namingInfix'), 'vnet')]",
```


Aşağıdaki diff'te, API'yi hesaplamak, ölçek kümeleriyle yönetilen disk desteği için en erken gerekli sürüm olan 2016-04-30 önizleme sürümüne güncelleştirilir. İstenirse eski sözdizimi ile yeni API sürümünde yönetilmeyen diskler kullanabilirsiniz. Yalnızca bilgi işlem API sürümünü güncellerseniz ve başka bir şeyi değiştirmezseniz, şablon eskisi gibi çalışmaya devam etmelidir.

```diff
@@ -86,7 +74,7 @@
       "version": "latest"
     },
     "imageReference": "[variables('osType')]",
-    "computeApiVersion": "2016-03-30",
+    "computeApiVersion": "2016-04-30-preview",
     "networkApiVersion": "2016-03-30",
     "storageApiVersion": "2015-06-15"
   },
```

Aşağıdaki diff'te, depolama hesabı kaynağı kaynak dizisinden tamamen kaldırılır. Yönetilen disk bunları otomatik olarak oluşturduğundan kaynak artık gerekli değildir.

```diff
@@ -113,19 +101,6 @@
       }
     },
-    {
-      "type": "Microsoft.Storage/storageAccounts",
-      "name": "[concat(variables('uniqueStringArray')[copyIndex()], variables('newStorageAccountSuffix'))]",
-      "location": "[resourceGroup().location]",
-      "apiVersion": "[variables('storageApiVersion')]",
-      "copy": {
-        "name": "storageLoop",
-        "count": "[variables('saCount')]"
-      },
-      "properties": {
-        "accountType": "[variables('storageAccountType')]"
-      }
-    },
     {
       "type": "Microsoft.Network/publicIPAddresses",
       "name": "[variables('publicIPAddressName')]",
       "location": "[resourceGroup().location]",
```

Aşağıdaki diff'te, ölçek kümesinden depolama hesapları oluşturan döngüye atıfta bulunan bağımsız maddeye bağlı lığı kaldırdığımızı görebiliriz. Eski şablonda, bu, depolama hesaplarının ölçek kümesi oluşturulmadan önce oluşturulmasını sağlamaktı, ancak bu yan tümce artık yönetilen disk için gerekli değildir. Bu özellikler yönetilen disk tarafından kaputun altında otomatik olarak işlendiğinden, vhd kapsayıcılar özelliği de OS disk adı özelliğiyle birlikte kaldırılır. Premium işletim `"managedDisk": { "storageAccountType": "Premium_LRS" }` sistemi diskleri istiyorsanız "osDisk" yapılandırmasını ekleyebilirsiniz. Yalnızca VM sku'da büyük veya küçük 'ler' bulunan VM'ler premium diskler kullanabilir.

```diff
@@ -183,7 +158,6 @@
       "location": "[resourceGroup().location]",
       "apiVersion": "[variables('computeApiVersion')]",
       "dependsOn": [
-        "storageLoop",
         "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]",
         "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]"
       ],
@@ -200,16 +174,8 @@
         "virtualMachineProfile": {
           "storageProfile": {
             "osDisk": {
-              "vhdContainers": [
-                "[concat('https://', variables('uniqueStringArray')[0], variables('newStorageAccountSuffix'), '.blob.core.windows.net/', variables('vhdContainerName'))]",
-                "[concat('https://', variables('uniqueStringArray')[1], variables('newStorageAccountSuffix'), '.blob.core.windows.net/', variables('vhdContainerName'))]",
-                "[concat('https://', variables('uniqueStringArray')[2], variables('newStorageAccountSuffix'), '.blob.core.windows.net/', variables('vhdContainerName'))]",
-                "[concat('https://', variables('uniqueStringArray')[3], variables('newStorageAccountSuffix'), '.blob.core.windows.net/', variables('vhdContainerName'))]",
-                "[concat('https://', variables('uniqueStringArray')[4], variables('newStorageAccountSuffix'), '.blob.core.windows.net/', variables('vhdContainerName'))]"
-              ],
-              "name": "[variables('osDiskName')]",
             },
             "imageReference": "[variables('imageReference')]"
           },

```

Yönetilen veya yönetilmeyen diskkullanılıp kullanılmayacağı için ölçek kümesi yapılandırmasında açık bir özellik yoktur. Ölçek kümesi, depolama profilinde bulunan özelliklere göre hangilerinin kullanılacağını bilir. Bu nedenle, doğru özellikleri ölçek kümesinin depolama profilinde olduğundan emin olmak için şablonu değiştirirken önemlidir.


## <a name="data-disks"></a>Veri diskleri

Yukarıdaki değişikliklerle, ölçek kümesi işletim sistemi diski için yönetilen diskleri kullanır, ancak veri diskleri ne olacak? Veri diskleri eklemek için "storageProfile" altında "dataDisks" özelliğini "osDisk" ile aynı düzeyde ekleyin. Özelliğin değeri, her biri "lun" (VM'deki veri diski başına benzersiz olması gereken) özelliklere sahip olan JSON nesne listesidir, "createOption" ("boş" şu anda desteklenen tek seçenektir) ve "diskSizeGB" (gigabaytlarda diskin boyutu; 0'dan büyük ve 1024'ten az olmalıdır) aşağıdaki örnekte olduğu gibi:

```
"dataDisks": [
  {
    "lun": "1",
    "createOption": "empty",
    "diskSizeGB": "1023"
  }
]
```

Bu dizide diskler belirtirseniz, `n` ölçek kümesindeki `n` her VM veri diskleri alır. Ancak, bu veri disklerinin ham aygıtlar olduğunu unutmayın. Biçimlendirilmemişlerdir. Diskleri kullanmadan önce eklemek, bölmek ve biçimlendirmek müşteriye kalmıştır. İsteğe bağlı olarak, `"managedDisk": { "storageAccountType": "Premium_LRS" }` her veri diski nesnesinde bunun bir premium veri diski olması gerektiğini belirtmek için de belirtebilirsiniz. Yalnızca VM sku'da büyük veya küçük 'ler' bulunan VM'ler premium diskler kullanabilir.

Ölçek kümeleri içeren veri disklerini kullanma hakkında daha fazla bilgi edinmek için [bu makaleye](./virtual-machine-scale-sets-attached-disks.md)bakın.


## <a name="next-steps"></a>Sonraki adımlar
Örneğin Ölçek kümelerini kullanan Kaynak Yöneticisi [şablonları, Azure Quickstart Şablonları GitHub repo'sunda "vmss"](https://github.com/Azure/azure-quickstart-templates)araması yapın.

Genel bilgi için, [ölçek kümeleri için ana açılış sayfasına](https://azure.microsoft.com/services/virtual-machine-scale-sets/)göz atın.

