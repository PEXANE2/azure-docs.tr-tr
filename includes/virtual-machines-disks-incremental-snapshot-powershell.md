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
ms.openlocfilehash: 3eec6583ebdff35d7e40d2eec305a947de0cb87c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79299473"
---
[!INCLUDE [virtual-machines-disks-incremental-snapshots-description](virtual-machines-disks-incremental-snapshots-description.md)]

### <a name="supported-regions"></a>Desteklenen bölgeler
[!INCLUDE [virtual-machines-disks-incremental-snapshots-regions](virtual-machines-disks-incremental-snapshots-regions.md)]

## <a name="restrictions"></a>Kısıtlamalar

[!INCLUDE [virtual-machines-disks-incremental-snapshots-restrictions](virtual-machines-disks-incremental-snapshots-restrictions.md)]

## <a name="powershell"></a>PowerShell

Artımlı anlık görüntü oluşturmak için Azure PowerShell'i kullanabilirsiniz. Azure PowerShell'in en son sürümüne ihtiyacınız olacak, aşağıdaki komut ya yükler veya mevcut yüklemenizi en son güncelleştirmeye çalışacaktır:

```PowerShell
Install-Module -Name Az -AllowClobber -Scope CurrentUser
```

Bu yüklendikten sonra PowerShell oturumunuza `az login`'' ile giriş yapın.

Azure PowerShell ile artımlı anlık görüntü oluşturmak için, [yeni-AzSnapShotConfig](https://docs.microsoft.com/powershell/module/az.compute/new-azsnapshotconfig?view=azps-2.7.0) ile yapılandırmayı `-Incremental` parametre ile ayarlayın ve bunu parametre aracılığıyla Yeni [AzSnapshot'a](https://docs.microsoft.com/powershell/module/az.compute/new-azsnapshot?view=azps-2.7.0) değişken olarak geçirin. `-Snapshot`

`<yourDiskNameHere>`Değiştirin `<yourResourceGroupNameHere>`, `<yourDesiredSnapShotNameHere>` ve değerlerinizle, artımlı bir anlık görüntü oluşturmak için aşağıdaki komut dosyasını kullanabilirsiniz:

```PowerShell
# Get the disk that you need to backup by creating an incremental snapshot
$yourDisk = Get-AzDisk -DiskName <yourDiskNameHere> -ResourceGroupName <yourResourceGroupNameHere>

# Create an incremental snapshot by setting the SourceUri property with the value of the Id property of the disk
$snapshotConfig=New-AzSnapshotConfig -SourceUri $yourDisk.Id -Location $yourDisk.Location -CreateOption Copy -Incremental 
New-AzSnapshot -ResourceGroupName <yourResourceGroupNameHere> -SnapshotName <yourDesiredSnapshotNameHere> -Snapshot $snapshotConfig 
```

Anlık görüntülerve `SourceResourceId` `SourceUniqueId` özellikleriyle aynı diskten artan anlık görüntüleri tanımlayabilirsiniz. `SourceResourceId`üst diskin Azure Kaynak Yöneticisi kaynak kimliğidir. `SourceUniqueId`diskin `UniqueId` özelliğinden devralınan değerdir. Bir diski siler ve sonra aynı ada sahip yeni `UniqueId` bir disk oluşturursanız, özelliğin değeri değişir.

Belirli bir `SourceResourceId` `SourceUniqueId` diskle ilişkili tüm anlık görüntülerin listesini kullanabilir ve oluşturabilirsiniz. Değerinizle değiştirin `<yourResourceGroupNameHere>` ve ardından varolan artımlı anlık görüntülerinizi listelemek için aşağıdaki örneği kullanabilirsiniz:

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
