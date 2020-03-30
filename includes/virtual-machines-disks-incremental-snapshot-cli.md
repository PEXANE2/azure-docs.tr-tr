---
title: include dosyası
description: include dosyası
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 03/05/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: cbd6f821326c86983ceb3ae5b90969e522c187fe
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80343051"
---
[!INCLUDE [virtual-machines-disks-incremental-snapshots-description](virtual-machines-disks-incremental-snapshots-description.md)]

## <a name="restrictions"></a>Kısıtlamalar

[!INCLUDE [virtual-machines-disks-incremental-snapshots-restrictions](virtual-machines-disks-incremental-snapshots-restrictions.md)]

## <a name="cli"></a>CLI

Azure CLI ile artımlı bir anlık görüntü oluşturabilirsiniz, Azure CLI'nin en son sürümüne ihtiyacınız olacaktır. 

Windows'da, aşağıdaki komut varolan yüklemenizi en son sürüme yükler veya günceller:
```PowerShell
Invoke-WebRequest -Uri https://aka.ms/installazurecliwindows -OutFile .\AzureCLI.msi; Start-Process msiexec.exe -Wait -ArgumentList '/I AzureCLI.msi /quiet'
```
Linux'ta, CLI kurulumu işletim sistemi sürümüne bağlı olarak değişir.  Bkz. Belirli Linux sürümünüz için [Azure CLI'yi yükleyin.](https://docs.microsoft.com/cli/azure/install-azure-cli)

Artımlı anlık görüntü oluşturmak [az snapshot create](https://docs.microsoft.com/cli/azure/snapshot?view=azure-cli-latest#az-snapshot-create) için `--incremental` parametre ile az anlık görüntü oluşturma kullanın.

Aşağıdaki örnek, artımlı bir anlık `<yourDesiredSnapShotNameHere>` `<yourResourceGroupNameHere>`görüntü`<exampleDiskName>`oluşturur, değiştirin , , , ve `<exampleLocation>` kendi değerlerinizle, sonra örneği çalıştırın:

```bash
sourceResourceId=$(az disk show -g <yourResourceGroupNameHere> -n <exampleDiskName> --query '[id]' -o tsv)

az snapshot create -g <yourResourceGroupNameHere> \
-n <yourDesiredSnapShotNameHere> \
-l <exampleLocation> \
--source "$sourceResourceId" \
--incremental
```

Anlık görüntülerve `SourceResourceId` `SourceUniqueId` özellikleriyle aynı diskten artan anlık görüntüleri tanımlayabilirsiniz. `SourceResourceId`üst diskin Azure Kaynak Yöneticisi kaynak kimliğidir. `SourceUniqueId`diskin `UniqueId` özelliğinden devralınan değerdir. Bir diski siler ve sonra aynı ada sahip yeni `UniqueId` bir disk oluşturursanız, özelliğin değeri değişir.

Belirli bir `SourceResourceId` `SourceUniqueId` diskle ilişkili tüm anlık görüntülerin listesini kullanabilir ve oluşturabilirsiniz. Aşağıdaki örnek, belirli bir diskle ilişkili tüm artımlı anlık görüntüleri listeletir, ancak bazı kurulum gerektirir.

Bu örnek, verileri sorgulamak için jq kullanır. Örneği çalıştırmak için [jq yüklemeniz](https://stedolan.github.io/jq/download/)gerekir.

`<yourResourceGroupNameHere>` Değiştirin `<exampleDiskName>` ve değerlerinizle birlikte, jq yüklediğiniz sürece varolan artımlı anlık görüntülerinizi listelemek için aşağıdaki örneği kullanabilirsiniz:

```bash
sourceUniqueId=$(az disk show -g <yourResourceGroupNameHere> -n <exampleDiskName> --query '[uniqueId]' -o tsv)

 
sourceResourceId=$(az disk show -g <yourResourceGroupNameHere> -n <exampleDiskName> --query '[id]' -o tsv)

az snapshot list -g <yourResourceGroupNameHere> -o json \
| jq -cr --arg SUID "$sourceUniqueId" --arg SRID "$sourceResourceId" '.[] | select(.incremental==true and .creationData.sourceUniqueId==$SUID and .creationData.sourceResourceId==$SRID)'
```

## <a name="resource-manager-template"></a>Resource Manager şablonu

Artımlı anlık görüntü oluşturmak için Azure Kaynak Yöneticisi şablonlarını da kullanabilirsiniz. ApiVersion **2019-03-01** olarak ayarlanmış olduğundan ve artımlı özelliğin de doğru olduğundan emin olmanız gerekir. Aşağıdaki parçacık, Kaynak Yöneticisi şablonları ile artımlı anlık görüntü oluşturmanın bir örneğidir:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "diskName": {
      "type": "string",
      "defaultValue": "contosodisk1"
    },
  "diskResourceId": {
    "defaultValue": "<your_managed_disk_resource_ID>",
    "type": "String"
  }
  }, 
  "resources": [
  {
    "type": "Microsoft.Compute/snapshots",
    "name": "[concat( parameters('diskName'),'_snapshot1')]",
    "location": "[resourceGroup().location]",
    "apiVersion": "2019-03-01",
    "properties": {
      "creationData": {
        "createOption": "Copy",
        "sourceResourceId": "[parameters('diskResourceId')]"
      },
      "incremental": true
    }
  }
  ]
}
```

## <a name="next-steps"></a>Sonraki adımlar

.NET'i kullanarak artımlı anlık görüntünün diferansiyel yeteneğini gösteren örnek kodu görmek istiyorsanız, [Azure Yönetilen Diskleri yedeklemelerini artımlı anlık görüntü fark özelliğine sahip başka bir bölgeye](https://github.com/Azure-Samples/managed-disks-dotnet-backup-with-incremental-snapshots)kopyalayın'a bakın.
