---
title: Eklenmemiş yönetilen ve yönetilmeyen Azure disklerini bulma ve silme
description: Azure PowerShell'i kullanarak eklenmemiş Azure yönetilen ve yönetilmeyen (VHD'ler/sayfa lekeleri) diskleri nasıl bulunur ve silinir.
author: roygara
ms.service: virtual-machines-windows
ms.topic: how-to
ms.date: 02/22/2019
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: cac192186c91259a5573dc27442137729816991a
ms.sourcegitcommit: af1cbaaa4f0faa53f91fbde4d6009ffb7662f7eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81869605"
---
# <a name="find-and-delete-unattached-azure-managed-and-unmanaged-disks"></a>Eklenmemiş yönetilen ve yönetilmeyen Azure disklerini bulma ve silme

Azure'da bir sanal makineyi (VM) sildiğinizde, varsayılan olarak VM'ye bağlı diskler silinmez. Bu özellik, VM'lerin istemeden silinmesi nedeniyle veri kaybını önlemeye yardımcı olur. Bir VM silindikten sonra, iliştirilmemiş diskler için ödeme yapmaya devam eceksiniz. Bu makalede, herhangi bir bekar diskleri bulmak ve silmek ve gereksiz maliyetleri azaltmak nasıl gösterir.

## <a name="managed-disks-find-and-delete-unattached-disks"></a>Yönetilen diskler: Eklenmemiş diskleri bulma ve silme

Aşağıdaki komut **dosyası, ManagedBy** özelliğinin değerini inceleyerek eklenmemiş [yönetilen diskleri](managed-disks-overview.md) arar. Yönetilen bir disk bir VM'ye eklendiğinde, **ManagedBy** özelliği VM'nin kaynak kimliğini içerir. Yönetilen bir disk eksiz olduğunda, **Yönetilen ler** özelliği geçersizdir. Komut dosyası, Azure aboneliğindeki tüm yönetilen diskleri inceler. Komut **dosyası, ManagedBy** özelliği null olarak ayarlanmış yönetilen bir disk imal ettiğinde, komut dosyası diskin bağlı olmadığını belirler.

>[!IMPORTANT]
>İlk olarak, **deleteUnattachedDisks** değişkenini 0'a ayarlayarak komut dosyasını çalıştırın. Bu eylem, tüm eklenmemiş yönetilen diskleri bulmanızı ve görüntülemenizi sağlar.
>
>Tüm eklenmemiş diskleri gözden geçirdikten sonra komut dosyasını yeniden çalıştırın ve **deleteUnattachedDisks** değişkenini 1 olarak ayarlayın. Bu eylem, tüm eklenmemiş yönetilen diskleri silmenizi sağlar.

```azurepowershell-interactive
# Set deleteUnattachedDisks=1 if you want to delete unattached Managed Disks
# Set deleteUnattachedDisks=0 if you want to see the Id of the unattached Managed Disks
$deleteUnattachedDisks=0
$managedDisks = Get-AzDisk
foreach ($md in $managedDisks) {
    # ManagedBy property stores the Id of the VM to which Managed Disk is attached to
    # If ManagedBy property is $null then it means that the Managed Disk is not attached to a VM
    if($md.ManagedBy -eq $null){
        if($deleteUnattachedDisks -eq 1){
            Write-Host "Deleting unattached Managed Disk with Id: $($md.Id)"
            $md | Remove-AzDisk -Force
            Write-Host "Deleted unattached Managed Disk with Id: $($md.Id) "
        }else{
            $md.Id
        }
    }
 }
```

## <a name="unmanaged-disks-find-and-delete-unattached-disks"></a>Yönetilmeyen diskler: Eklenmemiş diskleri bulma ve silme

Yönetilmeyen diskler, [Azure depolama hesaplarında](../../storage/common/storage-create-storage-account.md) [sayfa blob'ları](/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs#about-page-blobs) olarak depolanan VHD dosyalarıdır. Aşağıdaki komut dosyası, **LeaseStatus** özelliğinin değerini inceleyerek eklenmemiş yönetilmeyen diskleri (sayfa lekeleri) arar. Yönetilmeyen bir disk VM'ye bağlandığında, **LeaseStatus** özelliği **Kilitli**olarak ayarlanır. Yönetilmeyen bir disk eklenmemişse, **LeaseStatus** özelliği **Kilitlen'e**ayarlanır. Komut dosyası, bir Azure aboneliğindeki tüm Azure depolama hesaplarındaki tüm yönetilmeyen diskleri inceler. Komut dosyası, **Unlocked**olarak ayarlanmış **bir LeaseStatus** özelliğine sahip yönetilmeyen bir disk imal ettiğinde, komut dosyası diskin eksiz olduğunu belirler.

>[!IMPORTANT]
>İlk olarak, **deleteAttachedVHDs** değişkenini 0'a ayarlayarak komut dosyasını çalıştırın. Bu eylem, tüm eklenmemiş yönetilmeyen VHD'leri bulmanızı ve görüntülemenizi sağlar.
>
>Tüm eklenmemiş diskleri gözden geçirdikten sonra komut dosyasını yeniden çalıştırın ve **deleteAttachedVHDs** değişkenini 1 olarak ayarlayın. Bu eylem, tüm eklenmemiş yönetilmeyen VHD'leri silmenizi sağlar.

```azurepowershell-interactive
# Set deleteUnattachedVHDs=1 if you want to delete unattached VHDs
# Set deleteUnattachedVHDs=0 if you want to see the Uri of the unattached VHDs
$deleteUnattachedVHDs=0
$storageAccounts = Get-AzStorageAccount
foreach($storageAccount in $storageAccounts){
    $storageKey = (Get-AzStorageAccountKey -ResourceGroupName $storageAccount.ResourceGroupName -Name $storageAccount.StorageAccountName)[0].Value
    $context = New-AzStorageContext -StorageAccountName $storageAccount.StorageAccountName -StorageAccountKey $storageKey
    $containers = Get-AzStorageContainer -Context $context
    foreach($container in $containers){
        $blobs = Get-AzStorageBlob -Container $container.Name -Context $context
        #Fetch all the Page blobs with extension .vhd as only Page blobs can be attached as disk to Azure VMs
        $blobs | Where-Object {$_.BlobType -eq 'PageBlob' -and $_.Name.EndsWith('.vhd')} | ForEach-Object { 
            #If a Page blob is not attached as disk then LeaseStatus will be unlocked
            if($_.ICloudBlob.Properties.LeaseStatus -eq 'Unlocked'){
                    if($deleteUnattachedVHDs -eq 1){
                        Write-Host "Deleting unattached VHD with Uri: $($_.ICloudBlob.Uri.AbsoluteUri)"
                        $_ | Remove-AzStorageBlob -Force
                        Write-Host "Deleted unattached VHD with Uri: $($_.ICloudBlob.Uri.AbsoluteUri)"
                    }
                    else{
                        $_.ICloudBlob.Uri.AbsoluteUri
                    }
            }
        }
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için bkz: [Depolama hesabını sil](../../storage/common/storage-create-storage-account.md) ve [PowerShell'i Kullanarak Yetim Diskleri Tanımla](https://blogs.technet.microsoft.com/ukplatforms/2018/02/21/azure-cost-optimisation-series-identify-orphaned-disks-using-powershell/)
