---
title: dosya dahil etme
description: dosya dahil etme
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 03/05/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: d63ec0c2d82ec316a61771b4642731c932b045cf
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84224914"
---
[!INCLUDE [virtual-machines-disks-incremental-snapshots-description](virtual-machines-disks-incremental-snapshots-description.md)]

### <a name="supported-regions"></a>Desteklenen bölgeler
[!INCLUDE [virtual-machines-disks-incremental-snapshots-regions](virtual-machines-disks-incremental-snapshots-regions.md)]

## <a name="restrictions"></a>Kısıtlamalar

[!INCLUDE [virtual-machines-disks-incremental-snapshots-restrictions](virtual-machines-disks-incremental-snapshots-restrictions.md)]

## <a name="powershell"></a>PowerShell

Artımlı bir anlık görüntü oluşturmak için Azure PowerShell kullanabilirsiniz. Azure PowerShell en son sürümü gerekir, aşağıdaki komut bunu yükleyecek veya mevcut yüklemenizi en son sürümüne güncelleştirecek:

```PowerShell
Install-Module -Name Az -AllowClobber -Scope CurrentUser
```

Yüklendikten sonra PowerShell oturumunuzda ile oturum açın `Connect-AzAccount` .

Azure PowerShell ile artımlı bir anlık görüntü oluşturmak için, yapılandırmayı [New-AzSnapShotConfig](https://docs.microsoft.com/powershell/module/az.compute/new-azsnapshotconfig?view=azps-2.7.0) ile `-Incremental` parametresiyle ayarlayın ve ardından bu parametreyi parametresi aracılığıyla [New-azsnapshot](https://docs.microsoft.com/powershell/module/az.compute/new-azsnapshot?view=azps-2.7.0) öğesine bir değişken olarak geçirin `-Snapshot` .

, `<yourDiskNameHere>` `<yourResourceGroupNameHere>` Ve `<yourDesiredSnapShotNameHere>` değerlerini değerleriyle değiştirin, ardından artımlı bir anlık görüntü oluşturmak için aşağıdaki betiği kullanabilirsiniz:

```PowerShell
# Get the disk that you need to backup by creating an incremental snapshot
$yourDisk = Get-AzDisk -DiskName <yourDiskNameHere> -ResourceGroupName <yourResourceGroupNameHere>

# Create an incremental snapshot by setting the SourceUri property with the value of the Id property of the disk
$snapshotConfig=New-AzSnapshotConfig -SourceUri $yourDisk.Id -Location $yourDisk.Location -CreateOption Copy -Incremental 
New-AzSnapshot -ResourceGroupName <yourResourceGroupNameHere> -SnapshotName <yourDesiredSnapshotNameHere> -Snapshot $snapshotConfig 
```

Aynı diskten Artımlı anlık görüntüleri `SourceResourceId` ve `SourceUniqueId` anlık görüntülerin özelliklerini tanımlayabilirsiniz. `SourceResourceId`, üst diskin Azure Resource Manager kaynak KIMLIĞIDIR. `SourceUniqueId`, `UniqueId` diskin özelliğinden devralınan değerdir. Bir diski silip aynı ada sahip yeni bir disk oluşturursanız, `UniqueId` özelliğin değeri değişir.

`SourceResourceId` `SourceUniqueId` Belirli bir diskle ilişkili tüm anlık görüntülerin listesini oluşturmak için ve kullanabilirsiniz. `<yourResourceGroupNameHere>`Değerini, ile değiştirin ve ardından mevcut Artımlı anlık görüntülerinizi listelemek için aşağıdaki örneği kullanabilirsiniz:

```PowerShell
$snapshots = Get-AzSnapshot -ResourceGroupName <yourResourceGroupNameHere>

$incrementalSnapshots = New-Object System.Collections.ArrayList
foreach ($snapshot in $snapshots)
{
    
    if($snapshot.Incremental -and $snapshot.CreationData.SourceResourceId -eq $yourDisk.Id -and $snapshot.CreationData.SourceUniqueId -eq $yourDisk.UniqueId){

        $incrementalSnapshots.Add($snapshot)
    }
}

$incrementalSnapshots
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
