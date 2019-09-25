---
title: include dosyası
description: include dosyası
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 09/23/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: e39f294f7902eabef401d4c8145f4f19a07f267f
ms.sourcegitcommit: 3fa4384af35c64f6674f40e0d4128e1274083487
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2019
ms.locfileid: "71224582"
---
# <a name="creating-an-incremental-snapshot-preview-for-managed-disks"></a>Yönetilen diskler için Artımlı anlık görüntü (Önizleme) oluşturma

Artımlı anlık görüntüler (Önizleme), alınan yönetilen diskler için zaman içinde son anlık görüntüden bu yana yapılan tüm değişikliklerden oluşur. Artımlı bir anlık görüntüyü indirmeye veya başka bir şekilde kullanmaya çalıştığınızda, tam VHD kullanılır. Yönetilen disk anlık görüntülerinin bu yeni özelliği, siz tercih etmediğiniz müddetçe her bir anlık görüntü ile tüm diski depolamak için artık gerekli olmadığından daha fazla maliyetli olabilir. Düzenli anlık görüntülerle olduğu gibi, artımlı anlık görüntüler tam yönetilen disk oluşturmak için veya düzenli bir anlık görüntü oluşturmak için kullanılabilir.

Artımlı anlık görüntü ve normal anlık görüntü arasında birkaç fark vardır. Artımlı anlık görüntüler, diskin depolama türünden bağımsız olarak her zaman standart HDD depolamayı kullanır, ancak normal anlık görüntüler Premium SSD 'Ler kullanabilir. VM dağıtımlarını ölçeklendirmek için Premium depolamada düzenli anlık görüntüler kullanıyorsanız, [paylaşılan görüntü galerisinde](../articles/virtual-machines/linux/shared-image-galleries.md)standart depolamada özel görüntüler kullanmanızı öneririz. Daha düşük maliyetle daha büyük ölçekli bir ölçek elde etmenize yardımcı olur. Ayrıca, artımlı anlık görüntüler, bölgesel olarak [yedekli depolama](../articles/storage/common/storage-redundancy-zrs.md) (ZRS) ile daha iyi güvenilirlik sunar. ZRS seçili bölgede kullanılabiliyorsa, artımlı bir anlık görüntü, ZRS 'yi otomatik olarak kullanır. ZRS bölgede yoksa, anlık görüntü varsayılan [olarak yerel olarak yedekli depolama](../articles/storage/common/storage-redundancy-lrs.md) (LRS) olarak değişir. Bu davranışı geçersiz kılabilir ve bir el ile seçebilirsiniz ancak bunun için önerilmez.

Artımlı anlık görüntüler, yönetilen diskler için benzersiz olarak kullanılabilen bir fark özelliği de sunar. Bunlar, blok düzeyine kadar, aynı yönetilen disklerin iki artımlı anlık görüntüsü arasında değişiklik almanızı sağlar. Bu özelliği, anlık görüntüleri bölgeler arasında kopyalarken veri parmak izini azaltmak için kullanabilirsiniz.

Henüz önizlemeye kaydolmadıysanız ve artımlı anlık görüntü kullanmaya başlamak istiyorsanız lütfen genel önizlemeye erişim sağlamak AzureDisks@microsoft.com için bize e-posta gönderin.

## <a name="restrictions"></a>Kısıtlamalar

- Bir diskin boyutunu değiştirdikten sonra Artımlı anlık görüntüler şu anda oluşturulamaz.
- Artımlı anlık görüntüler şu anda abonelikler arasında taşınamaz.
- Şu anda, belirli bir anda belirli bir anlık görüntü ailesinin en fazla beş anlık görüntüsüne sahip SAS URI 'Leri oluşturabilirsiniz.
- Bu diskin aboneliği dışındaki belirli bir disk için artımlı bir anlık görüntü oluşturamazsınız.
- Her beş dakikada bir disk başına en fazla yedi Artımlı anlık görüntü oluşturulabilir.
- Tek bir disk için toplam 200 Artımlı anlık görüntü oluşturulabilir.

## <a name="powershell"></a>PowerShell

Artımlı bir anlık görüntü oluşturmak için Azure PowerShell kullanabilirsiniz. PowerShell 'in en son sürümünü yerel olarak yükleyebilirsiniz. Azure PowerShell en son sürümü gerekir, aşağıdaki komut bunu yükleyecek veya mevcut yüklemenizi en son sürümüne güncelleştirecek:

```PowerShell
Install-Module -Name Az -AllowClobber -Scope CurrentUser
```

Yüklendikten sonra PowerShell oturumunuzda ile `az login`oturum açın.

, Ve değerlerinideğerleriyle`<yourDiskNameHere>`değiştirin, ardındanartımlıbiranlıkgörüntüoluşturmakiçinaşağıdakibetiğikullanabilirsiniz:`<yourDesiredSnapShotNameHere>` `<yourResourceGroupNameHere>`

```PowerShell
# Get the disk that you need to backup by creating an incremental snapshot
$yourDisk = Get-AzDisk -DiskName <yourDiskNameHere> -ResourceGroupName <yourResourceGroupNameHere>

# Create an incremental snapshot by setting:
# 1. Incremental property
# 2. SourceUri property with the value of the Id property of the disk
$snapshotConfig=New-AzSnapshotConfig -SourceUri $yourDisk.Id -Location $yourDisk.Location -CreateOption Copy -Incremental 
New-AzSnapshot -ResourceGroupName <yourResourceGroupNameHere> -SnapshotName <yourDesiredSnapshotNameHere> -Snapshot $snapshotConfig 

# You can identify incremental snapshots of the same disk by using the SourceResourceId and SourceUniqueId properties of snapshots. 
# SourceResourceId is the Azure Resource Manager resource ID of the parent disk. 
# SourceUniqueId is the value inherited from the UniqueId property of the disk. If you delete a disk and then create a disk with the same name, the value of the UniqueId property will change. 
# Following script shows how to get all the incremental snapshots in a resource group of same disk
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

## <a name="cli"></a>CLI

[Az Snapshot Create](https://docs.microsoft.com/cli/azure/snapshot?view=azure-cli-latest#az-snapshot-create)kullanarak Azure CLI ile artımlı bir anlık görüntü oluşturabilirsiniz. Örnek bir komut aşağıdaki gibi görünür:

```bash
az snapshot create -g <exampleResourceGroup> \
-n <exampleSnapshotName> \
-l <exampleLocation> \
--source <exampleVMId> \
--incremental
```

`--query` [Az Snapshot Show](https://docs.microsoft.com/cli/azure/snapshot?view=azure-cli-latest#az-snapshot-show)üzerinde parametresini kullanarak CLI 'de Artımlı anlık görüntüler olduğunu da belirleyebilirsiniz. Bu parametreyi, anlık görüntülerin **Sourceresourceıd** ve **sourceuniqueıd** özelliklerini doğrudan sorgulamak için kullanabilirsiniz. Sourceresourceıd, üst diskin Azure Resource Manager kaynak KIMLIĞIDIR. **Sourceuniqueıd** , diskin **UniqueId** özelliğinden devralınan değerdir. Bir diski siler ve aynı ada sahip bir disk oluşturursanız, **UniqueId** özelliğinin değeri değişecektir.

Sorguların örnekleri aşağıda gösterildiği gibi görünür:

```bash
az snapshot show -g <exampleResourceGroup> \
-n <yourSnapShotName> \
--query [creationData.sourceResourceId] -o tsv

az snapshot show -g <exampleResourceGroup> \
-n <yourSnapShotName> \
--query [creationData.sourceUniqueId] -o tsv
```

## <a name="next-steps"></a>Sonraki adımlar

Henüz önizlemeye kaydolmadıysanız ve artımlı anlık görüntü kullanmaya başlamak istiyorsanız lütfen genel önizlemeye erişim sağlamak AzureDisks@microsoft.com için bize e-posta gönderin.
