---
title: Yönetilen disk kullanmak üzere bir ölçek kümesi şablonu dönüştürme
description: Bir Azure Resource Manager sanal makine ölçek kümesi şablonunu yönetilen disk ölçek kümesi şablonuna dönüştürün.
keywords: sanal makine ölçek kümeleri
author: mimckitt
tags: azure-resource-manager
ms.assetid: bc8c377a-8c3f-45b8-8b2d-acc2d6d0b1e8
ms.service: virtual-machine-scale-sets
ms.topic: conceptual
ms.date: 5/18/2017
ms.author: mimckitt
ms.openlocfilehash: 79fafa8344312294f6df107b88c9b7c571af1969
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81270664"
---
# <a name="convert-a-scale-set-template-to-a-managed-disk-scale-set-template"></a>Ölçek kümesi şablonunu yönetilen disk ölçek kümesi şablonuna dönüştürme

Yönetilen disk kullanmayan bir ölçek kümesi oluşturmak için Kaynak Yöneticisi şablonu olan müşteriler, yönetilen disk kullanmak üzere değiştirmek isteyebilir. Bu makalede, [Azure hızlı başlangıç şablonlarından](https://github.com/Azure/azure-quickstart-templates)örnek Kaynak Yöneticisi şablonları için topluluk odaklı depo olan bir çekme isteği örnek olarak kullanılarak yönetilen disklerin nasıl kullanılacağı gösterilmektedir. Tam çekme isteği şu şekilde görülebilir: [https://github.com/Azure/azure-quickstart-templates/pull/2998](https://github.com/Azure/azure-quickstart-templates/pull/2998)ve fark öğesinin ilgili bölümleri aşağıda, açıklamalarla birlikte aşağıdaki gibidir:

## <a name="making-the-os-disks-managed"></a>İşletim sistemi disklerinin yönetilmesini sağlama

Aşağıdaki fark içinde, depolama hesabı ve disk özellikleriyle ilgili birkaç değişken kaldırılır. Depolama hesabı türü artık gerekli değildir (varsayılan Standard_LRS), ancak isterseniz bunu belirtebilirsiniz. Yalnızca Standard_LRS ve Premium_LRS yönetilen disk ile desteklenir. Yeni depolama hesabı soneki, benzersiz dize dizisi ve SA sayısı, eski şablonda depolama hesabı adları oluşturmak için kullanıldı. Yönetilen disk otomatik olarak müşteri adına depolama hesapları oluşturduğundan, bu değişkenler yeni şablonda artık gerekli değildir. Benzer şekilde, yönetilen disk temeldeki Depolama Blobu kapsayıcılarını ve disklerini otomatik olarak isimlentiğinden, VHD kapsayıcısı adı ve işletim sistemi diski adı artık gerekli değildir.

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


Aşağıdaki fark içinde, işlem API 'SI, ölçek kümeleri ile yönetilen disk desteği için en erken gerekli sürüm olan 2016-04-30-Preview sürümüne güncelleştirilir. İsterseniz eski sözdizimi ile yeni API sürümünde yönetilmeyen diskler kullanabilirsiniz. Yalnızca işlem API sürümünü güncelleştirir ve başka herhangi bir şeyi değiştirmezseniz, şablon daha önce olduğu gibi çalışmaya devam etmelidir.

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

Aşağıdaki fark içinde, depolama hesabı kaynağı kaynak dizisinden tamamen kaldırılır. Yönetilen disk bu kaynakları otomatik olarak oluşturduğundan kaynağa artık gerek duyulmaz.

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

Aşağıdaki fark içinde, ölçek kümesinden, depolama hesapları oluşturan döngüye işaret eden bağlı olan yan tümceyi kaldırdığımızda görüyoruz. Eski şablonda, bu, depolama hesaplarının ölçek kümesi oluşturmaya başlamadan önce oluşturulmasını sağlamaktır, ancak bu yan tümce artık yönetilen disk ile gerekli değildir. VHD kapsayıcıları özelliği ayrıca, işletim sistemi disk adı özelliği ile birlikte kaldırılır ve bu özellikler yönetilen disk 'nin altında otomatik olarak işlenir. Premium işletim sistemi `"managedDisk": { "storageAccountType": "Premium_LRS" }` disklerini Isterseniz "OSDisk" yapılandırmasına ekleyebilirsiniz. Yalnızca VM SKU 'sunda büyük veya küçük harfli ' a sahip VM 'Ler Premium diskler kullanabilir.

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

Ölçek kümesi yapılandırmasında yönetilen veya yönetilmeyen disk kullanılıp kullanılmayacağını belirtir. Ölçek kümesi, depolama profilinde bulunan özelliklere göre hangisinin kullanılacağını bilir. Bu nedenle, doğru özelliklerin ölçek kümesinin depolama profilinde olduğundan emin olmak için şablonu değiştirirken önemlidir.


## <a name="data-disks"></a>Veri diskleri

Yukarıdaki değişikliklerle ölçek kümesi, işletim sistemi diski için yönetilen diskler kullanır, ancak veri diskleri ne kadar? Veri diskleri eklemek için "storageProfile" altına "dataDisks" özelliğini "osDisk" ile aynı düzeye ekleyin. Özelliğin değeri bir JSON nesne listesidir, her birinin "LUN" özellikleri (bir VM üzerinde veri diski başına benzersiz olması gerekir), "createOption" ("Empty" Şu anda desteklenen tek seçenektir) ve "diskSizeGB" (diskin gigabayt cinsinden boyutu) 0 ' dan büyük ve 1024 ' den küçük olmalıdır), aşağıdaki örnekte olduğu gibi:

```
"dataDisks": [
  {
    "lun": "1",
    "createOption": "empty",
    "diskSizeGB": "1023"
  }
]
```

Bu dizide diskler `n` belirtirseniz, ölçek kümesindeki her sanal makine veri disklerini alır `n` . Bununla birlikte, bu veri disklerinin ham cihaz olduğunu unutmayın. Bunlar biçimlendirilmez. Bu, kullanmadan önce diskleri eklemek, bölümlemek ve biçimlendirmek için müşteriye kadar sürer. İsteğe bağlı olarak, her bir `"managedDisk": { "storageAccountType": "Premium_LRS" }` veri diski nesnesinde bir Premium veri diski olması gerektiğini belirtmek için de belirtebilirsiniz. Yalnızca VM SKU 'sunda büyük veya küçük harfli ' a sahip VM 'Ler Premium diskler kullanabilir.

Ölçek kümeleri ile veri disklerini kullanma hakkında daha fazla bilgi edinmek için [Bu makaleye](./virtual-machine-scale-sets-attached-disks.md)bakın.


## <a name="next-steps"></a>Sonraki adımlar
Örneğin, ölçek kümeleri kullanan Kaynak Yöneticisi şablonlar için [Azure hızlı başlangıç şablonları GitHub](https://github.com/Azure/azure-quickstart-templates)deposunda "VMSS" araması yapın.

Genel bilgiler için [Ölçek kümelerinin ana giriş sayfasına](https://azure.microsoft.com/services/virtual-machine-scale-sets/)göz atın.

