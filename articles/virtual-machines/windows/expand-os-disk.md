---
title: Azure'da Windows VM'nin işletim sistemi sürücüsünün genişletilmesi
description: Kaynak Yöneticisi dağıtım modelinde Azure Powershell'i kullanarak sanal bir makinenin işletim sistemi sürücüsünün boyutunu genişletin.
services: virtual-machines-windows
documentationcenter: ''
author: mimckitt
manager: vashan
editor: ''
tags: azure-resource-manager
ms.assetid: d9edfd9f-482f-4c0b-956c-0d2c2c30026c
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 07/05/2018
ms.author: mimckitt
ms.subservice: disks
ms.openlocfilehash: 208438b5bc600fa09584ca34c72b1981d9eda399
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81261406"
---
# <a name="how-to-expand-the-os-drive-of-a-virtual-machine"></a>Bir sanal makinenin işletim sistemi sürücüsünü genişletme

[Azure](https://azure.microsoft.com/marketplace/)Marketi'nden bir görüntü dağıtarak Kaynak Grubunda yeni bir sanal makine (VM) oluşturduğunuzda, varsayılan işletim sistemi sürücüsü genellikle 127 GB'dır (bazı görüntülerde varsayılan olarak daha küçük işletim sistemi disk boyutları vardır). VM’ye veri diskleri eklemek (kaç tane ekleyebileceğiniz seçtiğiniz SKU’ya bağlıdır) mümkün olmasına, hatta uygulamaları ve yoğun CPU kullanımlı iş yüklerini bu ek disklere yüklemeniz önerilmesine rağmen, sıklıkla müşterilerin aşağıdaki gibi belirli senaryoları etkinleştirmesi için işletim sistemi sürücüsünü genişletmesi gerekir:

- İşletim sistemi sürücüsüne bileşen yükleyen eski uygulamaları destekleme.
- Fiziksel bir bilgisayarı veya sanal makineyi daha büyük bir işletim sistemi sürücüsüne sahip şirket içi kaynaktan geçirme.


> [!IMPORTANT]
> Azure Sanal Makine'nin işletim sistemi diskini yeniden boyutlandırmak için sanal makinenin ayrılması gerekir.
>
> Diskleri genişlettikten sonra, daha büyük diskten yararlanmak için [işletim sistemi içindeki birimi genişletmeniz](#expand-the-volume-within-the-os) gerekir.
> 


 


## <a name="resize-a-managed-disk"></a>Yönetilen bir diski yeniden boyutlandırma

Powershell ISE veya Powershell pencerenizi yönetim modunda açın ve aşağıdaki adımları izleyin:

1. Kaynak yönetimi modunda Microsoft Azure hesabınızda oturum açın ve aboneliğinizi aşağıdaki gibi seçin:
   
   ```powershell
   Connect-AzAccount
   Select-AzSubscription –SubscriptionName 'my-subscription-name'
   ```
2. Aşağıdakileri yaparak kaynak grubunuzun adını ve VM adını ayarlayın:
   
   ```powershell
   $rgName = 'my-resource-group-name'
   $vmName = 'my-vm-name'
   ```
3. Aşağıdakileri yaparak sanal makineniz için bir başvuru edinin:
   
   ```powershell
   $vm = Get-AzVM -ResourceGroupName $rgName -Name $vmName
   ```
4. Aşağıdakileri yaparak diski yeniden boyutlandırmadan önce VM’yi durdurun:
   
    ```Powershell
    Stop-AzVM -ResourceGroupName $rgName -Name $vmName
    ```
5. Yönetilen işletim sistemi diskine bir başvuru alın. Yönetilen işletim sistemi diskinin boyutunu istenilen değere ayarlayın ve Diski aşağıdaki gibi güncelleştirin:
   
   ```Powershell
   $disk= Get-AzDisk -ResourceGroupName $rgName -DiskName $vm.StorageProfile.OsDisk.Name
   $disk.DiskSizeGB = 1023
   Update-AzDisk -ResourceGroupName $rgName -Disk $disk -DiskName $disk.Name
   ```   
   > [!WARNING]
   > Yeni boyut mevcut disk boyutundan büyük olmalıdır. İşletim sistemi diskleri için izin verilen maksimum 2048 GB'dır. (Bu boyutun ötesinde VHD blob genişletmek mümkündür, ancak işletim sistemi sadece alan ilk 2048 GB ile çalışmak mümkün olacak.)
   > 
   > 
6. VM güncelleştirmesi biraz zaman alabilir. Komutun yürütülmesi tamamlandığında aşağıdakileri yaparak VM’yi yeniden başlatın:
   
   ```Powershell
   Start-AzVM -ResourceGroupName $rgName -Name $vmName
   ```

Hepsi bu! Şimdi RDP ile sanal makinenize girin, Bilgisayar Yönetimi’ni (veya Disk Yönetimi) açın ve yeni ayrılan alanı kullanarak sürücüyü genişletin.

## <a name="resize-an-unmanaged-disk"></a>Yönetilmeyen bir diski yeniden boyutlandırma

Powershell ISE veya Powershell pencerenizi yönetim modunda açın ve aşağıdaki adımları izleyin:

1. Kaynak yönetimi modunda Microsoft Azure hesabınızda oturum açın ve aboneliğinizi aşağıdaki gibi seçin:
   
   ```Powershell
   Connect-AzAccount
   Select-AzSubscription –SubscriptionName 'my-subscription-name'
   ```
2. Aşağıdakileri yaparak kaynak grubunuzun adını ve VM adını ayarlayın:
   
   ```Powershell
   $rgName = 'my-resource-group-name'
   $vmName = 'my-vm-name'
   ```
3. Aşağıdakileri yaparak sanal makineniz için bir başvuru edinin:
   
   ```Powershell
   $vm = Get-AzVM -ResourceGroupName $rgName -Name $vmName
   ```
4. Aşağıdakileri yaparak diski yeniden boyutlandırmadan önce VM’yi durdurun:
   
    ```Powershell
    Stop-AzVM -ResourceGroupName $rgName -Name $vmName
    ```
5. Yönetilmeyen işletim sistemi diskinin boyutunu istenilen değere ayarlayın ve VM'yi aşağıdaki gibi güncelleştirin:
   
   ```Powershell
   $vm.StorageProfile.OSDisk.DiskSizeGB = 1023
   Update-AzVM -ResourceGroupName $rgName -VM $vm
   ```
   
   > [!WARNING]
   > Yeni boyut mevcut disk boyutundan büyük olmalıdır. İşletim sistemi diskleri için izin verilen maksimum 2048 GB'dır. (Bu boyutun ötesinde VHD blob genişletmek mümkündür, ancak işletim sistemi sadece alan ilk 2048 GB ile çalışmak mümkün olacak.)
   > 
   > 
   
6. VM güncelleştirmesi biraz zaman alabilir. Komutun yürütülmesi tamamlandığında aşağıdakileri yaparak VM’yi yeniden başlatın:
   
   ```Powershell
   Start-AzVM -ResourceGroupName $rgName -Name $vmName
   ```


## <a name="scripts-for-os-disk"></a>İşletim sistemi diski için komut dosyaları

Aşağıda hem yönetilen hem de yönetilmeyen diskler için başvuru için tam komut dosyası:


**Yönetilen diskler**

```Powershell
Connect-AzAccount
Select-AzSubscription -SubscriptionName 'my-subscription-name'
$rgName = 'my-resource-group-name'
$vmName = 'my-vm-name'
$vm = Get-AzVM -ResourceGroupName $rgName -Name $vmName
Stop-AzVM -ResourceGroupName $rgName -Name $vmName
$disk= Get-AzDisk -ResourceGroupName $rgName -DiskName $vm.StorageProfile.OsDisk.Name
$disk.DiskSizeGB = 1023
Update-AzDisk -ResourceGroupName $rgName -Disk $disk -DiskName $disk.Name
Start-AzVM -ResourceGroupName $rgName -Name $vmName
```

**Yönetilmeyen diskler**

```powershell
Connect-AzAccount
Select-AzSubscription -SubscriptionName 'my-subscription-name'
$rgName = 'my-resource-group-name'
$vmName = 'my-vm-name'
$vm = Get-AzVM -ResourceGroupName $rgName -Name $vmName
Stop-AzVM -ResourceGroupName $rgName -Name $vmName
$vm.StorageProfile.OSDisk.DiskSizeGB = 1023
Update-AzVM -ResourceGroupName $rgName -VM $vm
Start-AzVM -ResourceGroupName $rgName -Name $vmName
```

## <a name="resizing-data-disks"></a>Veri disklerini yeniden boyutlandırma

Bu makale, öncelikle VM işletim sistemi diskini genişletmeye odaklanmıştır, ancak komut dosyası VM'ye bağlı veri disklerini genişletmek için de kullanılabilir. Örneğin, VM’ye bağlı ilk veri diskini genişletmek için `StorageProfile` öğesinin `OSDisk` nesnesini `DataDisks` dizisi ile değiştirin ve aşağıda gösterildiği gibi sayısal bir dizin kullanarak ilk bağlanan veri diskinin başvurusunu edinin:

**Yönetilen disk**

```powershell
$disk= Get-AzDisk -ResourceGroupName $rgName -DiskName $vm.StorageProfile.DataDisks[0].Name
$disk.DiskSizeGB = 1023
```


**Yönetilmeyen disk**

```powershell
$vm.StorageProfile.DataDisks[0].DiskSizeGB = 1023
```



Benzer şekilde, yukarıda gösterildiği gibi bir dizin veya diskin **Ad** özelliğini kullanarak VM'ye iliştirilen diğer veri disklerine başvuruda bulunabilirsiniz:


**Yönetilen disk**

```powershell
(Get-AzDisk -ResourceGroupName $rgName -DiskName ($vm.StorageProfile.DataDisks | Where ({$_.Name -eq 'my-second-data-disk'})).Name).DiskSizeGB = 1023
```

**Yönetilmeyen disk**

```powershell
($vm.StorageProfile.DataDisks | Where ({$_.Name -eq 'my-second-data-disk'}).DiskSizeGB = 1023
```

## <a name="expand-the-volume-within-the-os"></a>İşletim sistemi içindeki hacmi genişletme

VM için diski genişlettikten sonra işletim sistemi içine girmeniz ve yeni alanı kapsayacak şekilde ses düzeyini genişletmeniz gerekir. Bir bölümü genişletmek için çeşitli yöntemler vardır. Bu bölüm, **DiskPart**kullanarak bölümü genişletmek için bir RDP bağlantısı kullanarak VM bağlamayı kapsar.

1. VM'nize bir RDP bağlantısı açın.

2.  Komut istemi ni açın ve **disk parçası**yazın.

2.  **DISKPART** isteminde, `list volume`yazın. Uzatmak istediğiniz ses düzeyine dikkat edin.

3.  **DISKPART** isteminde, `select volume <volumenumber>`yazın. Bu, aynı diskte bitişik, boş alana genişletmek istediğiniz *birim numarasını* seçer.

4.  **DISKPART** isteminde, `extend [size=<size>]`yazın. Bu megabayt (MB) *boyutuna* göre seçilen hacmi genişletir.


## <a name="next-steps"></a>Sonraki adımlar

[Azure portalını](attach-managed-disk-portal.md)kullanarak disk ekleyebilirsiniz.
