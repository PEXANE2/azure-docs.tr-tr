---
title: Yönetilen diskdepolamayı standart ve premium SSD arasında dönüştürme
description: Azure CLI'yi kullanarak Azure yönetilen diskdepolamayı standarttan premium veya premium'a dönüştürme.
author: roygara
ms.service: virtual-machines-linux
ms.topic: conceptual
ms.date: 07/12/2018
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: cd9bb92b3ed86c3a57b5fc70411a4593335acedb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75431500"
---
# <a name="convert-azure-managed-disks-storage-from-standard-to-premium-or-premium-to-standard"></a>Azure yönetilen diskdepolamayı Standart'tan Premium'a veya Premium'dan Standard'a dönüştürme

Azure yönetilen disklerin dört disk türü vardır: Azure ultra SSD'leri (önizleme), premium SSD, standart SSD ve standart HDD. Performans gereksinimlerinize bağlı olarak üç GA disk türü (premium SSD, standart SSD ve standart HDD) arasında geçiş yapabilirsiniz. Henüz ultra SSD'den veya ultra SSD'ye geçiş yapamadığınız için, yeni bir tane dağıtmanız gerekir.

Bu işlevsellik yönetilmeyen diskler için desteklenmez. Ancak, disk türleri arasında geçiş [yapabilmek için yönetilmeyen bir diski kolayca yönetilen bir diske dönüştürebilirsiniz.](convert-unmanaged-to-managed-disks.md)

Bu makalede, azure CLI'yi kullanarak yönetilen disklerin Standart'tan Premium'a veya Premium'dan Standard'a nasıl dönüştürülür. Aracı yüklemek veya yükseltmek için [Azure CLI'yi yükle'ye](/cli/azure/install-azure-cli)bakın.

## <a name="before-you-begin"></a>Başlamadan önce

* Disk dönüştürme, sanal makinenin (VM) yeniden başlatılmasını gerektirir, bu nedenle önceden varolan bir bakım penceresi sırasında disk depolamanızın geçişini zamanlayın.
* Yönetilmeyen diskler için önce [yönetilen disklere dönüştürün,](convert-unmanaged-to-managed-disks.md) böylece depolama seçenekleri arasında geçiş yapabilirsiniz.


## <a name="switch-all-managed-disks-of-a-vm-between-premium-and-standard"></a>Bir VM'nin yönetilen tüm disklerini Premium ve Standart arasında değiştirme

Bu örnek, bir VM'nin tüm disklerinin Standart'tan Premium depolama alanına veya Premium depolama alanına nasıl dönüştürüleceğini gösterir. Premium yönetilen diskleri kullanmak için VM'nizin Premium depolama alanını destekleyen bir [VM boyutu](sizes.md) kullanması gerekir. Bu örnek, Premium depolama yı destekleyen bir boyuta da geçer.

 ```azurecli

#resource group that contains the virtual machine
rgName='yourResourceGroup'

#Name of the virtual machine
vmName='yourVM'

#Premium capable size 
#Required only if converting from Standard to Premium
size='Standard_DS2_v2'

#Choose between Standard_LRS and Premium_LRS based on your scenario
sku='Premium_LRS'

#Deallocate the VM before changing the size of the VM
az vm deallocate --name $vmName --resource-group $rgName

#Change the VM size to a size that supports Premium storage 
#Skip this step if converting storage from Premium to Standard
az vm resize --resource-group $rgName --name $vmName --size $size

#Update the SKU of all the data disks 
az vm show -n $vmName -g $rgName --query storageProfile.dataDisks[*].managedDisk -o tsv \
 | awk -v sku=$sku '{system("az disk update --sku "sku" --ids "$1)}'

#Update the SKU of the OS disk
az vm show -n $vmName -g $rgName --query storageProfile.osDisk.managedDisk -o tsv \
| awk -v sku=$sku '{system("az disk update --sku "sku" --ids "$1)}'

az vm start --name $vmName --resource-group $rgName

```
## <a name="switch-individual-managed-disks-between-standard-and-premium"></a>Tek tek yönetilen diskleri Standart ve Premium arasında geçiş

Geliştirme/test iş yükünüz için, maliyetlerinizi azaltmak için Standart ve Premium disklerin bir karışımına sahip olmak isteyebilirsiniz. Yalnızca daha iyi performans gerektiren diskleri yükseltmeyi seçebilirsiniz. Bu örnek, tek bir VM diskinin Standart depolamadan Premium depolama alanına veya Premium depolama alanına nasıl dönüştürüleceğini gösterir. Premium yönetilen diskleri kullanmak için VM'nizin Premium depolama alanını destekleyen bir [VM boyutu](sizes.md) kullanması gerekir. Bu örnek, Premium depolama yı destekleyen bir boyuta da geçer.

 ```azurecli

#resource group that contains the managed disk
rgName='yourResourceGroup'

#Name of your managed disk
diskName='yourManagedDiskName'

#Premium capable size 
#Required only if converting from Standard to Premium
size='Standard_DS2_v2'

#Choose between Standard_LRS and Premium_LRS based on your scenario
sku='Premium_LRS'

#Get the parent VM Id 
vmId=$(az disk show --name $diskName --resource-group $rgName --query managedBy --output tsv)

#Deallocate the VM before changing the size of the VM
az vm deallocate --ids $vmId 

#Change the VM size to a size that supports Premium storage 
#Skip this step if converting storage from Premium to Standard
az vm resize --ids $vmId --size $size

# Update the SKU
az disk update --sku $sku --name $diskName --resource-group $rgName 

az vm start --ids $vmId 
```

## <a name="switch-managed-disks-between-standard-hdd-and-standard-ssd"></a>Yönetilen diskleri Standart HDD ile Standart SSD arasında değiştirme

Bu örnek, tek bir VM diskinin Standart HDD'den Standart SSD'ye veya Standart SSD'den Standart HDD'ye nasıl dönüştürüleceğimi gösterir.

 ```azurecli

#resource group that contains the managed disk
rgName='yourResourceGroup'

#Name of your managed disk
diskName='yourManagedDiskName'

#Choose between Standard_LRS and StandardSSD_LRS based on your scenario
sku='StandardSSD_LRS'

#Get the parent VM ID 
vmId=$(az disk show --name $diskName --resource-group $rgName --query managedBy --output tsv)

#Deallocate the VM before changing the disk type
az vm deallocate --ids $vmId 

# Update the SKU
az disk update --sku $sku --name $diskName --resource-group $rgName 

az vm start --ids $vmId 
```

## <a name="switch-managed-disks-between-standard-and-premium-in-azure-portal"></a>Azure portalında Yönetilen diskleri Standart ve Premium arasında değiştirme

Şu adımları uygulayın:

1. [Azure portalında](https://portal.azure.com)oturum açın.
2. **Sanal makineler**listesinden VM'yi seçin.
3. VM durdurulmazsa, VM **Genel Bakış** bölmesinin üst kısmında **Dur'u** seçin ve VM'nin durmasını bekleyin.
4. VM bölmesinde menüden **Diskler'i** seçin.
5. Dönüştürmek istediğiniz diski seçin.
6. Menüden **Yapılandırma'yı** seçin.
7. Hesap **türünü** **Standart HDD'den** **Premium SSD'ye** veya **Premium SSD'den** **Standart HDD'ye**değiştirin.
8. **Kaydet'i**seçin ve disk bölmesini kapatın.

Disk türünün güncelleştirmesi anlıktır. Dönüşümden sonra VM'nizi yeniden başlatabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

[Anlık görüntüleri](snapshot-copy-managed-disk.md)kullanarak VM'nin salt okunur kopyasını yapın.
