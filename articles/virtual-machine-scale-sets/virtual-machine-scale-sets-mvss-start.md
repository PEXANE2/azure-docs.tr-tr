---
title: Sanal Makine Ölçek Kümesi şablonları hakkında bilgi edinin
description: Birkaç basit adım aracılığıyla Azure sanal makine ölçek kümeleri için temel ölçek kümesi şablonu oluşturmayı öğrenin.
services: virtual-machine-scale-sets
documentationcenter: ''
author: mayanknayar
manager: drewm
editor: ''
tags: azure-resource-manager
ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2019
ms.author: manayar
ms.openlocfilehash: 52fb7c770e9f9e2570cad92a8c0dd9bc4374e708
ms.sourcegitcommit: 3dc1a23a7570552f0d1cc2ffdfb915ea871e257c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/15/2020
ms.locfileid: "75980185"
---
# <a name="learn-about-virtual-machine-scale-set-templates"></a>Sanal Makine Ölçek Kümesi şablonları hakkında bilgi edinin
[Azure Resource Manager şablonları](https://docs.microsoft.com/azure/azure-resource-manager/template-deployment-overview#template-deployment-process), ilgili kaynak gruplarını dağıtmanın harika bir yoludur. Bu öğretici serisinde, temel bir ölçek kümesi şablonunun nasıl oluşturulduğu ve bu şablonun çeşitli senaryolara uyacak şekilde nasıl değiştirileceği gösterilmektedir. Tüm örnekler bu [GitHub deposundan](https://github.com/gatneil/mvss)gelir.

Bu şablonun basit olması amaçlanmıştır. Ölçek kümesi şablonlarının daha kapsamlı örnekleri için bkz. [Azure hızlı başlangıç şablonları GitHub deposu](https://github.com/Azure/azure-quickstart-templates) ve `vmss`dize içeren klasörleri arama.

Şablon oluşturma konusunda zaten bilgi sahibiyseniz, bu şablonu nasıl değiştireceğiniz hakkında daha fazla bilgi için "sonraki adımlar" bölümüne geçebilirsiniz.

## <a name="define-schema-and-contentversion"></a>$schema ve contentVersion tanımlayın
İlk olarak, şablonda `$schema` ve `contentVersion` tanımlayın. `$schema` öğesi, şablon dilinin sürümünü tanımlar ve Visual Studio sözdizimi vurgulama ve benzer doğrulama özellikleri için kullanılır. `contentVersion` öğesi Azure tarafından kullanılmıyor. Bunun yerine, şablon sürümünü izlemenize yardımcı olur.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
  "contentVersion": "1.0.0.0",
```

## <a name="define-parameters"></a>Parametreleri tanımlama
Sonra, `adminUsername` ve `adminPassword`olmak üzere iki parametre tanımlayın. Parametreler, dağıtım sırasında belirttiğiniz değerlerdir. `adminUsername` parametresi yalnızca bir `string` türüdür, ancak `adminPassword` bir sır olduğundan, bu tür `securestring`verin. Daha sonra, bu parametreler ölçek kümesi yapılandırmasına geçirilir.

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
Kaynak Yöneticisi şablonlar, daha sonra şablonda kullanılacak değişkenleri tanımlamanızı da sağlar. Örnek herhangi bir değişken kullanmaz, bu nedenle JSON nesnesi boş olur.

```json
  "variables": {},
```

## <a name="define-resources"></a>Kaynakları tanımlama
Bundan sonraki, şablonun kaynaklar bölümüdür. Burada, gerçekten ne dağıtmak istediğinizi tanımlarsınız. `parameters` ve `variables` (JSON nesneleri) aksine, `resources` JSON nesnelerinin JSON listesidir.

```json
   "resources": [
```

Tüm kaynaklar `type`, `name`, `apiVersion`ve `location` özellikleri gerektirir. Bu örneğin ilk kaynağında [Microsoft. Network/virtualNetwork](/azure/templates/microsoft.network/virtualnetworks), Name `myVnet`ve apiVersion `2018-11-01`türü vardır. (Bir kaynak türü için en son API sürümünü bulmak için, [Azure Resource Manager şablonu başvurusuna](/azure/templates/)bakın.)

```json
     {
       "type": "Microsoft.Network/virtualNetworks",
       "name": "myVnet",
       "apiVersion": "2018-11-01",
```

## <a name="specify-location"></a>Konum belirt
Sanal ağın konumunu belirtmek için bir [Kaynak Yöneticisi Şablon işlevi](../azure-resource-manager/templates/template-functions.md)kullanın. Bu işlev tırnak içine alınmış ve köşeli ayraçlar şöyle olmalıdır: `"[<template-function>]"`. Bu durumda `resourceGroup` işlevini kullanın. Bağımsız değişken içermez ve bu dağıtımın dağıtıldığı kaynak grubu hakkında meta veriler içeren bir JSON nesnesi döndürür. Kaynak grubu, dağıtım sırasında Kullanıcı tarafından ayarlanır. Bu değer daha sonra JSON nesnesinden konumu almak için `.location` ile bu JSON nesnesine dizinlenir.

```json
       "location": "[resourceGroup().location]",
```

## <a name="specify-virtual-network-properties"></a>Sanal ağ özelliklerini belirtin
Her bir Kaynak Yöneticisi kaynağının, kaynağa özgü yapılandırmalara yönelik kendi `properties` bölümü vardır. Bu durumda, sanal ağın, `10.0.0.0/16`özel IP adresi aralığını kullanan bir alt ağa sahip olması gerektiğini belirtin. Ölçek kümesi her zaman bir alt ağ içinde yer alır. Alt ağlara yayılamaz.

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

## <a name="add-dependson-list"></a>Bağımlıdson listesi ekle
Gerekli `type`, `name`, `apiVersion`ve `location` özelliklerine ek olarak, her bir kaynak için isteğe bağlı `dependsOn` dizeler listesi bulunabilir. Bu liste, bu kaynağı dağıtılmadan önce bu dağıtımın hangi diğer kaynakların bitmesini gerektiğini belirtir.

Bu durumda, listede bir önceki örnekteki sanal ağ olmak üzere yalnızca bir öğe vardır. Ölçek kümesi herhangi bir VM oluşturmadan önce ağın mevcut olması gerektiğinden bu bağımlılığı belirtirsiniz. Bu şekilde, ölçek kümesi bu VM 'Lere daha önce ağ özelliklerinde belirtilen IP adres aralığından özel IP adresleri verebilir. Bağımlıdson listesindeki her bir dizenin biçimi `<type>/<name>`. Daha önce sanal ağ kaynak tanımında kullanılan `type` ve `name` kullanın.

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
Ölçek kümelerinin ölçek kümesindeki VM 'Leri özelleştirmek için birçok özelliği vardır. Bu özelliklerin tam listesi için [şablon başvurusuna](/azure/templates/microsoft.compute/virtualmachinescalesets)bakın. Bu öğretici için, yalnızca birkaç yaygın kullanılan özellik ayarlanır.
### <a name="supply-vm-size-and-capacity"></a>VM boyut ve kapasitesini belirtin
Ölçek kümesinin oluşturulacak sanal makine boyutunu ("SKU adı") ve kaç tane sanal makine oluşturulacağını ("SKU kapasitesi") bilmeleri gerekir. Hangi VM boyutlarının kullanılabilir olduğunu görmek için, [VM boyutları belgelerine](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-sizes)bakın.

```json
       "sku": {
         "name": "Standard_A1",
         "capacity": 2
       },
```

### <a name="choose-type-of-updates"></a>Güncelleştirme türünü seçin
Ölçek kümesinin Ayrıca ölçek kümesinde güncelleştirmelerin nasıl işleneceğini bilmeleri gerekir. Şu anda üç seçenek vardır `Manual`, `Rolling` ve `Automatic`. İkisi arasındaki farklılıklar hakkında daha fazla bilgi için bkz. [Ölçek kümesini yükseltme](./virtual-machine-scale-sets-upgrade-scale-set.md#how-to-bring-vms-up-to-date-with-the-latest-scale-set-model)hakkındaki belgeler.

```json
       "properties": {
         "upgradePolicy": {
           "mode": "Manual"
         },
```

### <a name="choose-vm-operating-system"></a>VM işletim sistemi seçin
Ölçek kümesinin VM 'Lere hangi işletim sisteminin yerleştirileceğini bilmeleri gerekir. Burada, tam düzeltme eki uygulanan Ubuntu 16,04-LTS görüntüsüne sahip VM 'Ler oluşturun.

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

### <a name="specify-computernameprefix"></a>ComputerNamePrefix belirtin
Ölçek kümesi birden çok VM dağıtır. Her VM adını belirtmek yerine `computerNamePrefix`belirtin. Ölçek kümesi, her sanal makinenin ön ekine bir dizin ekler, bu nedenle VM adlarının biçimi `<computerNamePrefix>_<auto-generated-index>`.

Aşağıdaki kod parçacığında, ölçek kümesindeki tüm VM 'Ler için Yönetici Kullanıcı adı ve parolasını ayarlamak üzere önceki parametreleri kullanın. Bu işlem `parameters` şablonu işlevini kullanır. Bu işlev, hangi parametrenin başvurabileceği ve bu parametrenin değerini çıkaran bir dizeyi alır.

```json
           "osProfile": {
             "computerNamePrefix": "vm",
             "adminUsername": "[parameters('adminUsername')]",
             "adminPassword": "[parameters('adminPassword')]"
           },
```

### <a name="specify-vm-network-configuration"></a>VM ağ yapılandırmasını belirtin
Son olarak, ölçek kümesindeki VM 'Ler için ağ yapılandırmasını belirtin. Bu durumda, yalnızca daha önce oluşturulan alt ağın KIMLIĞINI belirtmeniz gerekir. Bu, ölçek kümesine ağ arabirimlerini bu alt ağa yerleştirmesine söyler.

`resourceId` şablonu işlevini kullanarak alt ağı içeren sanal ağın KIMLIĞINI alabilirsiniz. Bu işlev, bir kaynağın türünü ve adını alır ve bu kaynağın tam nitelikli tanımlayıcısını döndürür. Bu KIMLIK şu biçimdedir: `/subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/<resourceProviderNamespace>/<resourceType>/<resourceName>`

Ancak, sanal ağın tanımlayıcısı yeterli değildir. Ölçek kümesi VM 'lerinin içinde olması gereken belirli bir alt ağı sağlayın. Bunu yapmak için `/subnets/mySubnet` sanal ağın KIMLIĞINE birleştirin. Sonuç, alt ağın tam KIMLIĞIDIR. Bu birleştirme işlemi, bir dizi dizeyi alan ve bunların bitiştirilmesi döndüren `concat` işleviyle birlikte yapılır.

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
