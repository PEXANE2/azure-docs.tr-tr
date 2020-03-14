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
ms.openlocfilehash: 69be71a58c3aed4f52b77e63c9ddf12365301b08
ms.sourcegitcommit: c29b7870f1d478cec6ada67afa0233d483db1181
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79299216"
---
[!INCLUDE [virtual-machines-disks-incremental-snapshots-description](virtual-machines-disks-incremental-snapshots-description.md)]

### <a name="supported-regions"></a>Desteklenen bölgeler
[!INCLUDE [virtual-machines-disks-incremental-snapshots-regions](virtual-machines-disks-incremental-snapshots-regions.md)]

## <a name="restrictions"></a>Kısıtlamalar

[!INCLUDE [virtual-machines-disks-incremental-snapshots-restrictions](virtual-machines-disks-incremental-snapshots-restrictions.md)]

## <a name="cli"></a>CLI

Azure CLı ile artımlı bir anlık görüntü oluşturabilirsiniz. Azure CLı 'nin en son sürümüne ihtiyacınız olacaktır. 

Windows 'da aşağıdaki komut, mevcut yüklemenizi en son sürüme yükler veya güncelleştirir:
```PowerShell
Invoke-WebRequest -Uri https://aka.ms/installazurecliwindows -OutFile .\AzureCLI.msi; Start-Process msiexec.exe -Wait -ArgumentList '/I AzureCLI.msi /quiet'
```
Linux 'ta CLı yüklemesi, işletim sistemi sürümüne bağlı olarak değişir.  Bkz. belirli Linux sürümünüz için [Azure CLI 'Yı yüklemeyin](https://docs.microsoft.com/cli/azure/install-azure-cli) .

Artımlı bir anlık görüntü oluşturmak için, `--incremental` parametresiyle [az Snapshot Create](https://docs.microsoft.com/cli/azure/snapshot?view=azure-cli-latest#az-snapshot-create) kullanın.

Aşağıdaki örnek, artımlı bir anlık görüntü oluşturur, `<yourDesiredSnapShotNameHere>`, `<yourResourceGroupNameHere>`,`<exampleDiskName>`ve `<exampleLocation>` kendi değerlerinizle değiştirin, ardından örneği çalıştırın:

```bash
sourceResourceId=$(az disk show -g <yourResourceGroupNameHere> -n <exampleDiskName> --query '[id]' -o tsv)

az snapshot create -g <yourResourceGroupNameHere> \
-n <yourDesiredSnapShotNameHere> \
-l <exampleLocation> \
--source "$sourceResourceId" \
--incremental
```

`SourceResourceId` ve anlık görüntülerin `SourceUniqueId` özellikleriyle aynı diskten Artımlı anlık görüntüleri tanımlayabilirsiniz. `SourceResourceId`, üst diskin Azure Resource Manager kaynak KIMLIĞIDIR. `SourceUniqueId`, diskin `UniqueId` özelliğinden devralınan değerdir. Bir diski silip aynı ada sahip yeni bir disk oluşturursanız, `UniqueId` özelliğinin değeri değişir.

Belirli bir diskle ilişkili tüm anlık görüntülerin listesini oluşturmak için `SourceResourceId` ve `SourceUniqueId` kullanabilirsiniz. Aşağıdaki örnekte belirli bir diskle ilişkili tüm artımlı anlık görüntüler listelenir, ancak bazı ayarlar gereklidir.

Bu örnek, verileri sorgulamak için JQ kullanır. Örneği çalıştırmak için [JQ 'yı yüklemelisiniz](https://stedolan.github.io/jq/download/).

`<yourResourceGroupNameHere>` ve `<exampleDiskName>` değerlerini değerlerinizle değiştirin, ardından JQ 'yi de yüklediğiniz sürece mevcut Artımlı anlık görüntülerinizi listelemek için aşağıdaki örneği kullanabilirsiniz:

```bash
sourceUniqueId=$(az disk show -g <yourResourceGroupNameHere> -n <exampleDiskName> --query '[uniqueId]' -o tsv)

 
sourceResourceId=$(az disk show -g <yourResourceGroupNameHere> -n <exampleDiskName> --query '[id]' -o tsv)

az snapshot list -g <yourResourceGroupNameHere> -o json \
| jq -cr --arg SUID "$sourceUniqueId" --arg SRID "$sourceResourceId" '.[] | select(.incremental==true and .creationData.sourceUniqueId==$SUID and .creationData.sourceResourceId==$SRID)'
```

## <a name="resource-manager-template"></a>Resource Manager şablonu

Artımlı bir anlık görüntü oluşturmak için Azure Resource Manager şablonlarını da kullanabilirsiniz. ApiVersion 'ın **2019-03-01** olarak ayarlandığından ve artımlı özelliğinin de true olarak ayarlandığından emin olmanız gerekir. Aşağıdaki kod parçacığı, Kaynak Yöneticisi şablonlarıyla artımlı bir anlık görüntü oluşturma örneğidir:

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

.NET kullanarak Artımlı anlık görüntülerin fark yeteneğini gösteren örnek kodu görmek isterseniz, bkz. [Azure yönetilen diskler yedeklemelerini, artımlı anlık görüntülerin fark yeteneği ile başka bir bölgeye kopyalama](https://github.com/Azure-Samples/managed-disks-dotnet-backup-with-incremental-snapshots).
