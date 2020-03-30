---
title: Azure Toplu Iş havuzu oluşturma etkinliği
description: Toplu Iş havuzu için başvuru, bir havuz oluşturulduktan sonra yayılan olay oluşturun. Günlüğün içeriği havuz hakkında genel bilgileri ortaya çıkaracaktır.
services: batch
author: LauraBrenner
manager: evansma
ms.assetid: ''
ms.service: batch
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 04/20/2017
ms.author: labrenne
ms.openlocfilehash: dea025b274278aa5fed2900c95b4a274541ffef9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77022198"
---
# <a name="pool-create-event"></a>Havuz oluşturma olayı

 Bu olay, bir havuz oluşturulduktan sonra yayılır. Günlüğün içeriği havuz hakkında genel bilgileri ortaya çıkaracaktır. Havuzun hedef boyutu 0 işlem düğümünden büyükse, bu olaydan hemen sonra bir havuz yeniden boyutlandırma olayının izeceğini unutmayın.

 Aşağıdaki örnek, özellik kullanılarak oluşturulan bir havuz için `CloudServiceConfiguration` bir havuz oluşturma olayı gövdesini gösterir.

```
{
    "id": "myPool1",
    "displayName": "Production Pool",
    "vmSize": "Standard_F1s",
    "imageType": "VirtualMachineConfiguration",
    "cloudServiceConfiguration": {
        "osFamily": "3",
        "targetOsVersion": "*"
    },
    "networkConfiguration": {
        "subnetId": " "
    },
    "virtualMachineConfiguration": {
          "imageReference": {
            "publisher": " ",
            "offer": " ",
            "sku": " ",
            "version": " "
          },
          "nodeAgentId": " "
        },
    "resizeTimeout": "300000",
    "targetDedicatedNodes": 2,
    "targetLowPriorityNodes": 2,
    "maxTasksPerNode": 1,
    "vmFillType": "Spread",
    "enableAutoScale": false,
    "enableInterNodeCommunication": false,
    "isAutoPool": false
}
```

|Öğe|Tür|Notlar|
|-------------|----------|-----------|
|`id`|Dize|Havuzun kimliği.|
|`displayName`|Dize|Havuzun görüntü adı.|
|`vmSize`|Dize|Havuzdaki sanal makinelerin boyutu. Havuzdaki tüm sanal makineler aynı boyuttadır. <br/><br/> Bulut Hizmetleri havuzları için kullanılabilir sanal makinelerin boyutları (cloudServiceConfiguration ile oluşturulan havuzlar) hakkında bilgi için [Bulut Hizmetleri Için Boyutlar bölümüne](https://azure.microsoft.com/documentation/articles/cloud-services-sizes-specs/)bakın. Toplu iş, tüm `ExtraSmall`Bulut Hizmetleri VM boyutlarını destekler.<br/><br/> Sanal Makineler Marketplace'ten (virtualMachineConfiguration ile oluşturulan havuzlar) görüntüleri kullanan havuzlar için kullanılabilir VM boyutları hakkında bilgi için [Sanal Makineler için Boyutlar](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-sizes/) (Linux) veya Sanal Makineler için [Boyutlar](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-sizes/) (Windows) bakın. Batch `STANDARD_A0` ve premium depolama alanına sahip olanlar (`STANDARD_GS`, `STANDARD_DS` ve `STANDARD_DSV2` serisi) dışında tüm Azure sanal makinelerini destekler.|
|`imageType`|Dize|Görüntü için dağıtım yöntemi. Desteklenen değerler `virtualMachineConfiguration` veya`cloudServiceConfiguration`|
|[`cloudServiceConfiguration`](#bk_csconf)|Karmaşık Tür|Havuz için bulut hizmeti yapılandırması.|
|[`virtualMachineConfiguration`](#bk_vmconf)|Karmaşık Tür|Havuz için sanal makine yapılandırması.|
|[`networkConfiguration`](#bk_netconf)|Karmaşık Tür|Havuz için ağ yapılandırması.|
|`resizeTimeout`|Zaman|Havuzdaki son yeniden boyutlandırma işlemi için belirtilen havuza bilgi işlem düğümleri tahsisi için zaman aşımı.  (Havuz oluşturulduğunda ilk boyutlandırma yeniden boyutlandırma olarak sayılır.)|
|`targetDedicatedNodes`|Int32|Havuz için istenen özel işlem düğümlerinin sayısı.|
|`targetLowPriorityNodes`|Int32|Havuz için istenen düşük öncelikli işlem düğümlerinin sayısı.|
|`enableAutoScale`|Bool|Havuz boyutunun zaman içinde otomatik olarak ayarlanıp ayarlanmadığını belirtir.|
|`enableInterNodeCommunication`|Bool|Havuzun düğümler arasında doğrudan iletişim için ayarlanıp ayarlanıp ayarlmadığını belirtir.|
|`isAutoPool`|Bool|Havuzun bir işin AutoPool mekanizması aracılığıyla oluşturulup oluşturulmadığını belirtir.|
|`maxTasksPerNode`|Int32|Havuzdaki tek bir işlem düğümünde aynı anda çalıştırılabilen en fazla görev sayısı.|
|`vmFillType`|Dize|Toplu İşlem hizmetinin görevleri havuzdaki işlem düğümleri arasında nasıl dağıttığını tanımlar. Geçerli değerler Spread veya Pack'tir.|

###  <a name="cloudserviceconfiguration"></a><a name="bk_csconf"></a>cloudServiceConfiguration

|Öğe adı|Tür|Notlar|
|------------------|----------|-----------|
|`osFamily`|Dize|Azure Guest OS ailesi havuzdaki sanal makinelere yüklenecek.<br /><br /> Olası değerler şunlardır:<br /><br /> **2** – Windows Server 2008 R2 SP1'e eşdeğer OS Family 2.<br /><br /> **3** – Windows Server 2012'ye eşdeğer OS Family 3.<br /><br /> **4** – Windows Server 2012 R2'ye eşdeğer OS Family 4.<br /><br /> Daha fazla bilgi için Azure [Konuk İşletim Sistemi Sürümleri'ne](https://azure.microsoft.com/documentation/articles/cloud-services-guestos-update-matrix/#releases)bakın.|
|`targetOSVersion`|Dize|Azure Konuk İşletim Sistemi sürümü havuzdaki sanal makinelere yüklenecek.<br /><br /> Varsayılan değer, **\*** belirtilen aile için en son işletim sistemi sürümünü belirten değerdir.<br /><br /> İzin verilen diğer değerler için [Azure Konuk İşletim Sistemi Sürümleri'ne](https://azure.microsoft.com/documentation/articles/cloud-services-guestos-update-matrix/#releases)bakın.|

###  <a name="virtualmachineconfiguration"></a><a name="bk_vmconf"></a>virtualMachineConfiguration

|Öğe adı|Tür|Notlar|
|------------------|----------|-----------|
|[`imageReference`](#bk_imgref)|Karmaşık Tür|Kullanılacak platform veya Market görüntüsü hakkındaki bilgileri belirtir.|
|`nodeAgentId`|Dize|İşlem düğümünde verilen Toplu İşlem düğümü aracısının SKU'su.|
|[`windowsConfiguration`](#bk_winconf)|Karmaşık Tür|Sanal makinede Windows işletim sistemi ayarlarını belirtir. ImageReference bir Linux IŞLETIM Sistemi görüntüsüne atıfta bulunuyorsa, bu özellik belirtilmemelidir.|

###  <a name="imagereference"></a><a name="bk_imgref"></a>imageReference

|Öğe adı|Tür|Notlar|
|------------------|----------|-----------|
|`publisher`|Dize|Resmin yayıncısı.|
|`offer`|Dize|Görüntünün teklifi.|
|`sku`|Dize|Görüntünün SKU'su.|
|`version`|Dize|Resmin sürümü.|

###  <a name="windowsconfiguration"></a><a name="bk_winconf"></a>windowsYapılandırma

|Öğe adı|Tür|Notlar|
|------------------|----------|-----------|
|`enableAutomaticUpdates`|Boole|Sanal makinenin otomatik güncelleştirmeler için etkin olup olmadığını gösterir. Bu özellik belirtilmemişse, varsayılan değer doğrudur.|

###  <a name="networkconfiguration"></a><a name="bk_netconf"></a>ağYapılandırma

|Öğe adı|Tür|Notlar|
|------------------|--------------|----------|
|`subnetId`|Dize|Havuzun bilgi işlem düğümlerinin oluşturulduğu alt ağın kaynak tanımlayıcısını belirtir.|
