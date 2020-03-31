---
title: Sanal makine ölçeği seti şablonları hakkında bilgi edinin
description: Azure sanal makine ölçeği kümeleri için birkaç basit adımda temel ölçek kümesi şablonu oluşturmayı öğrenin.
author: mayanknayar
tags: azure-resource-manager
ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.topic: conceptual
ms.date: 04/26/2019
ms.author: manayar
ms.openlocfilehash: 24db9b2d39771c481a8c43e2b55f12cef381b4d6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76271902"
---
# <a name="learn-about-virtual-machine-scale-set-templates"></a>Sanal makine ölçeği seti şablonları hakkında bilgi edinin
[Azure Resource Manager şablonları](https://docs.microsoft.com/azure/azure-resource-manager/template-deployment-overview#template-deployment-process), ilgili kaynak gruplarını dağıtmanın harika bir yoludur. Bu öğretici seri, temel ölçek kümesi şablonu oluşturmanın ve bu şablonun çeşitli senaryolara uyacak şekilde nasıl değiştirilebildiğini gösterir. Tüm örnekler bu [GitHub deposundan](https://github.com/gatneil/mvss)gelir.

Bu şablon basit olması amaçlanmıştır. Ölçek kümesi şablonlarının daha eksiksiz örnekleri için [Azure Quickstart Templates GitHub deposuna](https://github.com/Azure/azure-quickstart-templates) bakın ve `vmss`dizeyi içeren klasörleri arayın.

Şablon oluşturma konusunda zaten aşinaysanız, bu şablonu nasıl değiştirebileceğinizi görmek için "Sonraki adımlar" bölümüne atlayabilirsiniz.

## <a name="define-schema-and-contentversion"></a>$schema ve içeriği tanımlayınSürüm
İlk olarak, tanımlayın `$schema` ve `contentVersion` şablonda. Öğe `$schema` şablon dilinin sürümünü tanımlar ve Visual Studio sözdizimi vurgulama ve benzer doğrulama özellikleri için kullanılır. Öğe `contentVersion` Azure tarafından kullanılmaz. Bunun yerine, şablon sürümünü izlemenize yardımcı olur.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
  "contentVersion": "1.0.0.0",
```

## <a name="define-parameters"></a>Parametreleri tanımlama
Sonra, iki parametre tanımlayın `adminUsername` ve `adminPassword`. Parametreler, dağıtım sırasında belirttiğiniz değerlerdir. `adminUsername` Parametre sadece bir `string` tür, `adminPassword` ancak bir gizli olduğundan, türü `securestring`verin. Daha sonra, bu parametreler ölçek kümesi yapılandırmasına aktarılır.

```json
  "parameters": {
    "adminUsername": {
      "type": "string"
    },
    "adminPassword": {
      "type": "securestring"
    }
  },
```
## <a name="define-variables"></a>Değişkenleri tanımlama
Kaynak Yöneticisi şablonları, şablonda daha sonra kullanılacak değişkenleri tanımlamanıza da izin verir. Örnek herhangi bir değişken kullanmadığından JSON nesnesi boştur.

```json
  "variables": {},
```

## <a name="define-resources"></a>Kaynakları tanımlama
Sonraki şablonun kaynaklar bölümü. Burada, gerçekte ne dağıtmak istediğinizi tanımlarsınız. Aksine `parameters` `variables` ve (JSON nesneleri `resources` olan), JSON nesnelerin bir JSON listesidir.

```json
   "resources": [
```

Tüm kaynaklar `type` `name`, `apiVersion`, `location` , ve özellikleri gerektirir. Bu örneğin ilk kaynağımicrosoft.Network/virtualNetwork türüne `myVnet`sahiptir, adı `2018-11-01`ve apiVersion. [Microsoft.Network/virtualNetwork](/azure/templates/microsoft.network/virtualnetworks) (Kaynak türü için en son API sürümünü bulmak için [Azure Kaynak Yöneticisi şablon ubaşvurusuna](/azure/templates/)bakın.)

```json
     {
       "type": "Microsoft.Network/virtualNetworks",
       "name": "myVnet",
       "apiVersion": "2018-11-01",
```

## <a name="specify-location"></a>Konumu belirtin
Sanal ağın konumunu belirtmek için [Kaynak Yöneticisi şablon işlevini](../azure-resource-manager/templates/template-functions.md)kullanın. Bu işlev tırnak içinde ve bunun gibi kare `"[<template-function>]"`parantez içinde eklenmelidir: . Bu durumda, `resourceGroup` işlevi kullanın. Hiçbir bağımsız değişken alır ve bu dağıtım için dağıtılıyor kaynak grubu hakkında meta veri içeren bir JSON nesnesi döndürür. Kaynak grubu dağıtım sırasında kullanıcı tarafından ayarlanır. Bu değer daha sonra JSON nesnesinden konumu almak `.location` için bu JSON nesnesine dizine eklenir.

```json
       "location": "[resourceGroup().location]",
```

## <a name="specify-virtual-network-properties"></a>Sanal ağ özelliklerini belirtin
Her Kaynak Yöneticisi kaynağının, kaynağa özgü yapılandırmalar için kendi `properties` bölümü vardır. Bu durumda, sanal ağ özel IP adresi aralığını `10.0.0.0/16`kullanarak bir alt net olması gerektiğini belirtin. Ölçek kümesi her zaman bir alt ağ içinde bulunur. Alt ağlara yayılamaz.

```json
       "properties": {
         "addressSpace": {
           "addressPrefixes": [
             "10.0.0.0/16"
           ]
         },
         "subnets": [
           {
             "name": "mySubnet",
             "properties": {
               "addressPrefix": "10.0.0.0/16"
             }
           }
         ]
       }
     },
```

## <a name="add-dependson-list"></a>Bağlı listeye ekle
Gerekli `type` `name`, , `apiVersion`, , `location` ve özelliklere ek olarak, her kaynak dizeleri isteğe bağlı `dependsOn` bir listesi olabilir. Bu liste, bu kaynağı dağıtmadan önce bu dağıtımdaki diğer kaynakların hangilerinin tamamlanması gerektiğini belirtir.

Bu durumda, listede yalnızca bir öğe vardır, önceki örnekten sanal ağ. Ölçek kümesi herhangi bir VM oluşturmadan önce ağ var olması gerektiğinden bu bağımlılığı belirtirsiniz. Bu şekilde, ölçek kümesi bu VM'lere ağ özelliklerinde daha önce belirtilen IP adresi aralığından özel IP adresleri verebilir. DependsOn listesindeki her dizenin `<type>/<name>`biçimi . Sanal ağ `type` `name` kaynak tanımında aynı ve daha önce kullanılan kullanın.

```json
     {
       "type": "Microsoft.Compute/virtualMachineScaleSets",
       "name": "myScaleSet",
       "apiVersion": "2019-03-01",
       "location": "[resourceGroup().location]",
       "dependsOn": [
         "Microsoft.Network/virtualNetworks/myVnet"
       ],
```
## <a name="specify-scale-set-properties"></a>Ölçek kümesi özelliklerini belirtin
Ölçek kümeleri, ölçek kümesindeki VM'leri özelleştirmek için birçok özelliye sahiptir. Bu özelliklerin tam listesi için [şablon başvurusuna](/azure/templates/microsoft.compute/virtualmachinescalesets)bakın. Bu öğretici için, yalnızca birkaç yaygın olarak kullanılan özellikleri ayarlanır.
### <a name="supply-vm-size-and-capacity"></a>Tedarik VM boyutu ve kapasitesi
Ölçek kümesi oluşturmak için VM boyutu ("sku adı") ve oluşturmak için kaç tür VM ("sku kapasitesi") bilmek gerekir. Hangi VM boyutlarının kullanılabildiği görmek için [VM Boyutlar belgelerine](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-sizes)bakın.

```json
       "sku": {
         "name": "Standard_A1",
         "capacity": 2
       },
```

### <a name="choose-type-of-updates"></a>Güncelleştirme türünü seçin
Ölçek kümesi, ölçek kümesindeki güncelleştirmelerin nasıl işleyeceğini de bilmesi gerekir. Şu anda, üç `Manual` `Rolling` seçenek `Automatic`vardır ve . İkisi arasındaki farklar hakkında daha fazla bilgi için, [ölçek kümesini yükseltme nin nasıl olduğu](./virtual-machine-scale-sets-upgrade-scale-set.md#how-to-bring-vms-up-to-date-with-the-latest-scale-set-model)yla ilgili belgelere bakın.

```json
       "properties": {
         "upgradePolicy": {
           "mode": "Manual"
         },
```

### <a name="choose-vm-operating-system"></a>VM işletim sistemini seçin
Ölçek kümesinin VM'lere hangi işletim sistemini koyacağını bilmesi gerekir. Burada, tamamen yamalı Ubuntu 16.04-LTS görüntü ile VM'ler oluşturun.

```json
         "virtualMachineProfile": {
           "storageProfile": {
             "imageReference": {
               "publisher": "Canonical",
               "offer": "UbuntuServer",
               "sku": "16.04-LTS",
               "version": "latest"
             }
           },
```

### <a name="specify-computernameprefix"></a>ComputerNamePrefix'i belirtin
Ölçek kümesi birden çok VM dağır. Her VM adını belirtmek `computerNamePrefix`yerine. Ölçek kümesi, her VM için önekiiçin bir dizin ekler, `<computerNamePrefix>_<auto-generated-index>`böylece VM adları forma sahiptir.

Aşağıdaki snippet'te, ölçek kümesindeki tüm VM'ler için yönetici kullanıcı adı ve parolasını ayarlamak için önceki parametreleri kullanın. Bu işlem `parameters` şablon işlevini kullanır. Bu işlev, hangi parametreye başvurmak için belirten bir dize alır ve bu parametrenin değerini çıkarır.

```json
           "osProfile": {
             "computerNamePrefix": "vm",
             "adminUsername": "[parameters('adminUsername')]",
             "adminPassword": "[parameters('adminPassword')]"
           },
```

### <a name="specify-vm-network-configuration"></a>VM ağ yapılandırması belirtin
Son olarak, ölçek kümesindeki VM'ler için ağ yapılandırmasını belirtin. Bu durumda, yalnızca daha önce oluşturulan alt netin kimliğini belirtmeniz gerekir. Bu, ağ arabirimlerini bu alt ağa koymak için ölçek kümesini söyler.

`resourceId` Şablon işlevini kullanarak alt ağı içeren sanal ağın kimliğini alabilirsiniz. Bu işlev, kaynağın türünü ve adını alır ve bu kaynağın tam nitelikli tanımlayıcısını döndürür. Bu kimlik şu şekildedir:`/subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/<resourceProviderNamespace>/<resourceType>/<resourceName>`

Ancak, sanal ağın tanımlayıcısı yeterli değildir. Ölçek kümesi VM'lerin olması gereken belirli alt ağı sağlayın. Bunu yapmak için, `/subnets/mySubnet` sanal ağın kimliğine göz yumun. Sonuç, alt ağın tam nitelikli kimliğidir. Dizeleri bir dizi `concat` alır ve onların concatenation döndürür işlevi ile bu concatenation yapın.

```json
           "networkProfile": {
             "networkInterfaceConfigurations": [
               {
                 "name": "myNic",
                 "properties": {
                   "primary": "true",
                   "ipConfigurations": [
                     {
                       "name": "myIpConfig",
                       "properties": {
                         "subnet": {
                           "id": "[concat(resourceId('Microsoft.Network/virtualNetworks', 'myVnet'), '/subnets/mySubnet')]"
                         }
                       }
                     }
                   ]
                 }
               }
             ]
           }
         }
       }
     }
   ]
}

```

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [mvss-next-steps-include](../../includes/mvss-next-steps.md)]
