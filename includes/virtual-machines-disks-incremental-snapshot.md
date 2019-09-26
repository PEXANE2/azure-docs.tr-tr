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
ms.openlocfilehash: ee8a711a867f8abdc831b0d1d9d0b504b1104955
ms.sourcegitcommit: 0486aba120c284157dfebbdaf6e23e038c8a5a15
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/26/2019
ms.locfileid: "71310119"
---
# <a name="creating-an-incremental-snapshot-preview-for-managed-disks"></a>Yönetilen diskler için Artımlı anlık görüntü (Önizleme) oluşturma

Artımlı anlık görüntüler (Önizleme), alınan yönetilen diskler için zaman içinde son anlık görüntüden bu yana yapılan tüm değişikliklerden oluşur. Artımlı bir anlık görüntüyü indirmeye veya başka bir şekilde kullanmaya çalıştığınızda, tam VHD kullanılır. Yönetilen disk anlık görüntülerinin bu yeni özelliği, siz tercih etmediğiniz müddetçe her bir anlık görüntü ile tüm diski depolamak için artık gerekli olmadığından daha fazla maliyetli olabilir. Düzenli anlık görüntülerle olduğu gibi, artımlı anlık görüntüler tam yönetilen disk oluşturmak için veya düzenli bir anlık görüntü oluşturmak için kullanılabilir.

Artımlı anlık görüntü ve normal anlık görüntü arasında birkaç fark vardır. Artımlı anlık görüntüler, diskin depolama türünden bağımsız olarak her zaman standart HDD depolamayı kullanır, ancak normal anlık görüntüler Premium SSD 'Ler kullanabilir. VM dağıtımlarını ölçeklendirmek için Premium depolamada düzenli anlık görüntüler kullanıyorsanız, [paylaşılan görüntü galerisinde](../articles/virtual-machines/linux/shared-image-galleries.md)standart depolamada özel görüntüler kullanmanızı öneririz. Daha düşük maliyetle daha büyük ölçekli bir ölçek elde etmenize yardımcı olur. Ayrıca, artımlı anlık görüntüler, bölgesel olarak [yedekli depolama](../articles/storage/common/storage-redundancy-zrs.md) (ZRS) ile daha iyi güvenilirlik sunar. ZRS seçili bölgede kullanılabiliyorsa, artımlı bir anlık görüntü, ZRS 'yi otomatik olarak kullanır. ZRS bölgede yoksa, anlık görüntü varsayılan [olarak yerel olarak yedekli depolama](../articles/storage/common/storage-redundancy-lrs.md) (LRS) olarak değişir. Bu davranışı geçersiz kılabilir ve bir el ile seçebilirsiniz ancak bunun için önerilmez.

Artımlı anlık görüntüler, yönetilen diskler için benzersiz olarak kullanılabilen bir fark özelliği de sunar. Bunlar, blok düzeyine kadar, aynı yönetilen disklerin iki artımlı anlık görüntüsü arasında değişiklik almanızı sağlar. Bu özelliği, anlık görüntüleri bölgeler arasında kopyalarken veri parmak izini azaltmak için kullanabilirsiniz.

Henüz önizlemeye kaydolmadıysanız ve artımlı anlık görüntüleri kullanmaya başlamak istiyorsanız, genel önizlemeye erişim sağlamak AzureDisks@microsoft.com için bize e-posta gönderin.

## <a name="restrictions"></a>Kısıtlamalar

- Artımlı anlık görüntüler şu anda yalnızca Orta Batı ABD kullanılabilir.
- Bir diskin boyutunu değiştirdikten sonra Artımlı anlık görüntüler şu anda oluşturulamaz.
- Artımlı anlık görüntüler şu anda abonelikler arasında taşınamaz.
- Şu anda, belirli bir anda belirli bir anlık görüntü ailesinin en fazla beş anlık görüntüsüne sahip SAS URI 'Leri oluşturabilirsiniz.
- Bu diskin aboneliği dışındaki belirli bir disk için artımlı bir anlık görüntü oluşturamazsınız.
- Her beş dakikada bir disk başına en fazla yedi Artımlı anlık görüntü oluşturulabilir.
- Tek bir disk için toplam 200 Artımlı anlık görüntü oluşturulabilir.

## <a name="powershell"></a>PowerShell

Artımlı bir anlık görüntü oluşturmak için Azure PowerShell kullanabilirsiniz. Azure PowerShell en son sürümü gerekir, aşağıdaki komut bunu yükleyecek veya mevcut yüklemenizi en son sürümüne güncelleştirecek:

```PowerShell
Install-Module -Name Az -AllowClobber -Scope CurrentUser
```

Yüklendikten sonra PowerShell oturumunuzda ile `az login`oturum açın.

Azure PowerShell ile artımlı bir anlık görüntü oluşturmak için, yapılandırmayı [New-azsnapshotconfig](https://docs.microsoft.com/en-us/powershell/module/az.compute/new-azsnapshotconfig?view=azps-2.7.0) `-Incremental` ile parametresiyle ayarlayın ve ardından bu `-Snapshot` parametreyi parametresi aracılığıyla [New-azsnapshot](https://docs.microsoft.com/en-us/powershell/module/az.compute/new-azsnapshot?view=azps-2.7.0) öğesine bir değişken olarak geçirin.

, Ve değerlerinideğerleriyle`<yourDiskNameHere>`değiştirin, ardındanartımlıbiranlıkgörüntüoluşturmakiçinaşağıdakibetiğikullanabilirsiniz:`<yourDesiredSnapShotNameHere>` `<yourResourceGroupNameHere>`

```PowerShell
# Get the disk that you need to backup by creating an incremental snapshot
$yourDisk = Get-AzDisk -DiskName <yourDiskNameHere> -ResourceGroupName <yourResourceGroupNameHere>

# Create an incremental snapshot by setting the SourceUri property with the value of the Id property of the disk
$snapshotConfig=New-AzSnapshotConfig -SourceUri $yourDisk.Id -Location $yourDisk.Location -CreateOption Copy -Incremental 
New-AzSnapshot -ResourceGroupName <yourResourceGroupNameHere> -SnapshotName <yourDesiredSnapshotNameHere> -Snapshot $snapshotConfig 
```

Aynı diskten `SourceResourceId` Artımlı anlık görüntüleri `SourceUniqueId` ve anlık görüntülerin özelliklerini tanımlayabilirsiniz. `SourceResourceId`, üst diskin Azure Resource Manager kaynak KIMLIĞIDIR. `SourceUniqueId`, diskin `UniqueId` özelliğinden devralınan değerdir. Bir diski silip aynı ada sahip yeni bir disk oluşturursanız, `UniqueId` özelliğin değeri değişir.

Belirli bir diskle `SourceResourceId` ilişkili `SourceUniqueId` tüm anlık görüntülerin listesini oluşturmak için ve kullanabilirsiniz. Değerini `<yourResourceGroupNameHere>` , ile değiştirin ve ardından mevcut Artımlı anlık görüntülerinizi listelemek için aşağıdaki örneği kullanabilirsiniz:

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

## <a name="cli"></a>CLI

Azure CLı ile artımlı bir anlık görüntü oluşturabilirsiniz. Azure CLı 'nin en son sürümüne ihtiyacınız olacaktır. Aşağıdaki komut, mevcut yüklemenizi en son sürüme yükler veya güncelleştirir:

```PowerShell
Invoke-WebRequest -Uri https://aka.ms/installazurecliwindows -OutFile .\AzureCLI.msi; Start-Process msiexec.exe -Wait -ArgumentList '/I AzureCLI.msi /quiet'
```

Artımlı bir anlık görüntü oluşturmak için, `--incremental` parametresiyle [az Snapshot Create](https://docs.microsoft.com/cli/azure/snapshot?view=azure-cli-latest#az-snapshot-create) kullanın.

Aşağıdaki örnek, artımlı bir anlık görüntü oluşturur, `<yourDesiredSnapShotNameHere>`, `<yourResourceGroupNameHere>`,`<exampleDiskName>`, ve `<exampleLocation>` değerlerini kendi değerlerinizle değiştirin, ardından örneği çalıştırın:

```bash
sourceResourceId=$(az disk show -g <yourResourceGroupNameHere> -n <exampleDiskName> --query '[id]' -o tsv)

az snapshot create -g <yourResourceGroupNameHere> \
-n <yourDesiredSnapShotNameHere> \
-l <exampleLocation> \
--source "$sourceResourceId" \
--incremental
```

Aynı diskten `SourceResourceId` Artımlı anlık görüntüleri `SourceUniqueId` ve anlık görüntülerin özelliklerini tanımlayabilirsiniz. `SourceResourceId`, üst diskin Azure Resource Manager kaynak KIMLIĞIDIR. `SourceUniqueId`, diskin `UniqueId` özelliğinden devralınan değerdir. Bir diski silip aynı ada sahip yeni bir disk oluşturursanız, `UniqueId` özelliğin değeri değişir.

Belirli bir diskle `SourceResourceId` ilişkili `SourceUniqueId` tüm anlık görüntülerin listesini oluşturmak için ve kullanabilirsiniz. Aşağıdaki örnekte belirli bir diskle ilişkili tüm artımlı anlık görüntüler listelenir, ancak bazı ayarlar gereklidir.

Bu örnek, verileri sorgulamak için JQ kullanır. Örneği çalıştırmak için [JQ 'yı yüklemelisiniz](https://stedolan.github.io/jq/download/).

`<yourResourceGroupNameHere>` Ve`<exampleDiskName>` değerlerinizle değiştirin, Ayrıca, JQ 'yi yüklediğiniz sürece mevcut Artımlı anlık görüntülerinizi listelemek için aşağıdaki örneği kullanabilirsiniz:

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

Henüz önizlemeye kaydolmadıysanız ve artımlı anlık görüntüleri kullanmaya başlamak istiyorsanız, genel önizlemeye erişim sağlamak AzureDisks@microsoft.com için bize e-posta gönderin.
