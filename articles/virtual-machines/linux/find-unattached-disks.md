---
title: Eklenmemiş yönetilen ve yönetilmeyen Azure disklerini bulma ve silme
description: Azure CLI'yi kullanarak eklenmemiş Azure yönetilen ve yönetilmeyen (VHD'ler/sayfa lekeleri) diskleri nasıl bulunur ve silinir.
author: roygara
ms.service: virtual-machines
ms.topic: article
ms.date: 03/30/2018
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: 6cf48c53e7b5c1cc8537abeda164460de66abddb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78945157"
---
# <a name="find-and-delete-unattached-azure-managed-and-unmanaged-disks-using-the-azure-cli"></a>Azure CLI'yi kullanarak eklenmemiş Azure yönetilen ve yönetilmeyen diskleri bulma ve silme
Azure'da bir sanal makineyi (VM) sildiğinizde, varsayılan olarak VM'ye bağlı diskler silinmez. Bu özellik, VM'lerin istemeden silinmesi nedeniyle veri kaybını önlemeye yardımcı olur. Bir VM silindikten sonra, iliştirilmemiş diskler için ödeme yapmaya devam eceksiniz. Bu makalede, herhangi bir bekar diskleri bulmak ve silmek ve gereksiz maliyetleri azaltmak nasıl gösterir. 


## <a name="managed-disks-find-and-delete-unattached-disks"></a>Yönetilen diskler: Eklenmemiş diskleri bulma ve silme 

Aşağıdaki komut **dosyası, ManagedBy** özelliğinin değerini inceleyerek eklenmemiş [yönetilen diskleri](managed-disks-overview.md) arar. Yönetilen bir disk bir VM'ye eklendiğinde, **ManagedBy** özelliği VM'nin kaynak kimliğini içerir. Yönetilen bir disk eksiz olduğunda, **Yönetilen ler** özelliği geçersizdir. Komut dosyası, Azure aboneliğindeki tüm yönetilen diskleri inceler. Komut **dosyası, ManagedBy** özelliği null olarak ayarlanmış yönetilen bir disk imal ettiğinde, komut dosyası diskin bağlı olmadığını belirler.

>[!IMPORTANT]
>İlk olarak, **deleteUnattachedDisks** değişkenini 0'a ayarlayarak komut dosyasını çalıştırın. Bu eylem, tüm eklenmemiş yönetilen diskleri bulmanızı ve görüntülemenizi sağlar.
>
>Tüm eklenmemiş diskleri gözden geçirdikten sonra komut dosyasını yeniden çalıştırın ve **deleteUnattachedDisks** değişkenini 1 olarak ayarlayın. Bu eylem, tüm eklenmemiş yönetilen diskleri silmenizi sağlar.
>

```azurecli

# Set deleteUnattachedDisks=1 if you want to delete unattached Managed Disks
# Set deleteUnattachedDisks=0 if you want to see the Id of the unattached Managed Disks
deleteUnattachedDisks=0

unattachedDiskIds=$(az disk list --query '[?managedBy==`null`].[id]' -o tsv)
for id in ${unattachedDiskIds[@]}
do
    if (( $deleteUnattachedDisks == 1 ))
    then

        echo "Deleting unattached Managed Disk with Id: "$id
        az disk delete --ids $id --yes
        echo "Deleted unattached Managed Disk with Id: "$id

    else
        echo $id
    fi
done
```

## <a name="unmanaged-disks-find-and-delete-unattached-disks"></a>Yönetilmeyen diskler: Eklenmemiş diskleri bulma ve silme 

Yönetilmeyen diskler, [Azure depolama hesaplarında](../../storage/common/storage-create-storage-account.md) [sayfa blob'ları](/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs#about-page-blobs) olarak depolanan VHD dosyalarıdır. Aşağıdaki komut dosyası, **LeaseStatus** özelliğinin değerini inceleyerek eklenmemiş yönetilmeyen diskleri (sayfa lekeleri) arar. Yönetilmeyen bir disk VM'ye bağlandığında, **LeaseStatus** özelliği **Kilitli**olarak ayarlanır. Yönetilmeyen bir disk eklenmemişse, **LeaseStatus** özelliği **Kilitlen'e**ayarlanır. Komut dosyası, bir Azure aboneliğindeki tüm Azure depolama hesaplarındaki tüm yönetilmeyen diskleri inceler. Komut dosyası, **Unlocked**olarak ayarlanmış **bir LeaseStatus** özelliğine sahip yönetilmeyen bir disk imal ettiğinde, komut dosyası diskin eksiz olduğunu belirler.

>[!IMPORTANT]
>İlk olarak, **deleteAttachedVHDs** değişkenini 0'a ayarlayarak komut dosyasını çalıştırın. Bu eylem, tüm eklenmemiş yönetilmeyen VHD'leri bulmanızı ve görüntülemenizi sağlar.
>
>Tüm eklenmemiş diskleri gözden geçirdikten sonra komut dosyasını yeniden çalıştırın ve **deleteAttachedVHDs** değişkenini 1 olarak ayarlayın. Bu eylem, tüm eklenmemiş yönetilmeyen VHD'leri silmenizi sağlar.
>

```azurecli
   
# Set deleteUnattachedVHDs=1 if you want to delete unattached VHDs
# Set deleteUnattachedVHDs=0 if you want to see the details of the unattached VHDs
deleteUnattachedVHDs=0

storageAccountIds=$(az storage account list --query [].[id] -o tsv)

for id in ${storageAccountIds[@]}
do
    connectionString=$(az storage account show-connection-string --ids $id --query connectionString -o tsv)
    containers=$(az storage container list --connection-string $connectionString --query [].[name] -o tsv)

    for container in ${containers[@]}
    do 
        
        blobs=$(az storage blob list -c $container --connection-string $connectionString --query "[?properties.blobType=='PageBlob' && ends_with(name,'.vhd')].[name]" -o tsv)
        
        for blob in ${blobs[@]}
        do
            leaseStatus=$(az storage blob show -n $blob -c $container --connection-string $connectionString --query "properties.lease.status" -o tsv)
            
            if [ "$leaseStatus" == "unlocked" ]
            then 

                if (( $deleteUnattachedVHDs == 1 ))
                then 

                    echo "Deleting VHD: "$blob" in container: "$container" in storage account: "$id

                    az storage blob delete --delete-snapshots include  -n $blob -c $container --connection-string $connectionString

                    echo "Deleted VHD: "$blob" in container: "$container" in storage account: "$id
                else
                    echo "StorageAccountId: "$id" container: "$container" VHD: "$blob
                fi

            fi
        done
    done
done 
```

## <a name="next-steps"></a>Sonraki adımlar

[Depolama hesabını silme](../../storage/common/storage-create-storage-account.md)



