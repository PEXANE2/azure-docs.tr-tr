---
title: Azure PowerShell ile Windows sorun giderme VM'i kullanma | Microsoft Dokümanlar
description: Azure PowerShell kullanarak işletim sistemi diskini kurtarma VM'ine bağlayarak Azure'daki Windows VM sorunlarını nasıl gidereceklerini öğrenin
services: virtual-machines-windows
documentationCenter: ''
author: genlin
manager: dcscontentpm
editor: ''
ms.service: virtual-machines-windows
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 08/09/2018
ms.author: genli
ms.openlocfilehash: 66cda98f272e7353b620059a731972714db585ae
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75374141"
---
# <a name="troubleshoot-a-windows-vm-by-attaching-the-os-disk-to-a-recovery-vm-using-azure-powershell"></a>Azure PowerShell kullanarak işletim sistemi diskini kurtarma VM'ine takarak Windows VM sorun giderme
Azure'daki Windows sanal makineniz (VM) bir önyükleme veya disk hatasıyla karşılaşırsa, diskin kendisinde sorun giderme adımları gerçekleştirmeniz gerekebilir. Yaygın bir örnek, VM'nin başarılı bir şekilde önyükleme yapabilmesini engelleyen başarısız bir uygulama güncelleştirmesi olacaktır. Bu makalede, diski başka bir Windows VM'ye bağlamak ve ardından orijinal VM'nizi onarmak için Azure PowerShell'in nasıl kullanılacağı ayrıntılı olarak açıklanmaktadır. 

> [!Important]
> Bu makaledeki komut dosyaları yalnızca [Yönetilen Disk'i](../windows/managed-disks-overview.md)kullanan VM'ler için geçerlidir. 

 

## <a name="recovery-process-overview"></a>Kurtarma işlemine genel bakış
Artık Bir VM için işletim sistemi diskini değiştirmek için Azure PowerShell'i kullanabiliriz. Artık VM'yi silip yeniden oluşturmamıza gerek yok.

Sorun giderme işlemi aşağıdaki gibidir:

1. Etkilenen VM'yi durdurun.
2. VM'nin işletim sistemi diskinden anlık görüntü oluşturun.
3. İşletim sistemi disk anlık görüntüsünden bir disk oluşturun.
4. Diski kurtarma VM'sine veri diski olarak takın.
5. Kurtarma VM'ine bağlanın. Dosyaları edin veya kopyalanan işletim sistemi diskindeki sorunları gidermek için herhangi bir araç çalıştırın.
6. Kurtarma VM'den sökme ve diski ayırma.
7. Etkilenen VM için işletim sistemi diskini değiştirin.

1, 2, 3, 4, 6 ve 7 adımlarını otomatikleştirmek için VM onarım komutlarını kullanabilirsiniz. Daha fazla belge ve yönerge için Azure [Sanal Makine onarım komutlarını kullanarak Windows VM'sini onar'a](repair-windows-vm-using-azure-virtual-machine-repair-commands.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)bakın.

En [son Azure PowerShell'inaboneliğinize](/powershell/azure/overview) yüklediğinizden ve oturum açtığınızdan emin olun:

```powershell
Connect-AzAccount
```

Aşağıdaki örneklerde, parametre adlarını kendi değerlerinizle değiştirin. 

## <a name="determine-boot-issues"></a>Önyükleme sorunlarını belirleme
Önyükleme sorunlarını gidermeye yardımcı olmak için Azure'daki VM'nizin ekran görüntüsünü görüntüleyebilirsiniz. Bu ekran görüntüsü, VM'nin neden önyükleme de başarısız olduğunu belirlemenize yardımcı olabilir. Aşağıdaki örnek, windows vm `myVM` adlı `myResourceGroup`kaynak grubundan ekran görüntüsünü alır:

```powershell
Get-AzVMBootDiagnosticsData -ResourceGroupName myResourceGroup `
    -Name myVM -Windows -LocalPath C:\Users\ops\
```

VM'nin neden önyükleme de başarısız olduğunu belirlemek için ekran görüntüsünü gözden geçirin. Sağlanan belirli hata iletilerini veya hata kodlarını not edin.

## <a name="stop-the-vm"></a>VM’yi durdurma

Aşağıdaki örnek, adlı kaynak `myVM` grubundan adlı `myResourceGroup`VM'yi durdurur:

```powershell
Stop-AzVM -ResourceGroupName "myResourceGroup" -Name "myVM"
```

Bir sonraki adıma işlemeden önce VM'nin silme işlemini tamamlamasını bekleyin.


## <a name="create-a-snapshot-from-the-os-disk-of-the-vm"></a>VM'nin işletim sistemi diskinden anlık görüntü oluşturma

Aşağıdaki örnekte, VM'nin işletim sistemi diskinden 'myVM' adlı adla `mySnapshot` bir anlık görüntü oluşturulur. 

```powershell
$resourceGroupName = 'myResourceGroup' 
$location = 'eastus' 
$vmName = 'myVM'
$snapshotName = 'mySnapshot'  

#Get the VM
$vm = get-azvm `
-ResourceGroupName $resourceGroupName `
-Name $vmName

#Create the snapshot configuration for the OS disk
$snapshot =  New-AzSnapshotConfig `
-SourceUri $vm.StorageProfile.OsDisk.ManagedDisk.Id `
-Location $location `
-CreateOption copy

#Take the snapshot
New-AzSnapshot `
   -Snapshot $snapshot `
   -SnapshotName $snapshotName `
   -ResourceGroupName $resourceGroupName 
```

Anlık görüntü, bir VHD'nin tam, salt okunur kopyasıdır. VM'ye bağlanamaz. Bir sonraki adımda, bu anlık görüntüden bir disk oluşturacağız.

## <a name="create-a-disk-from-the-snapshot"></a>Anlık görüntüden disk oluşturma

Bu komut dosyası, adlı `newOSDisk` `mysnapshot`anlık görüntüden adı olan yönetilen bir disk oluşturur.  

```powershell
#Set the context to the subscription Id where Managed Disk will be created
#You can skip this step if the subscription is already selected

$subscriptionId = 'yourSubscriptionId'

Select-AzSubscription -SubscriptionId $SubscriptionId

#Provide the name of your resource group
$resourceGroupName ='myResourceGroup'

#Provide the name of the snapshot that will be used to create Managed Disks
$snapshotName = 'mySnapshot' 

#Provide the name of the Managed Disk
$diskName = 'newOSDisk'

#Provide the size of the disks in GB. It should be greater than the VHD file size.
$diskSize = '128'

#Provide the storage type for Managed Disk. Premium_LRS or Standard_LRS.
$storageType = 'Standard_LRS'

#Provide the Azure region (e.g. westus) where Managed Disks will be located.
#This location should be same as the snapshot location
#Get all the Azure location using command below:
#Get-AzLocation
$location = 'eastus'

$snapshot = Get-AzSnapshot -ResourceGroupName $resourceGroupName -SnapshotName $snapshotName 
 
$diskConfig = New-AzDiskConfig -AccountType $storageType -Location $location -CreateOption Copy -SourceResourceId $snapshot.Id
 
New-AzDisk -Disk $diskConfig -ResourceGroupName $resourceGroupName -DiskName $diskName
```
Şimdi orijinal işletim sistemi diskinin bir kopyası var. Sorun giderme amacıyla bu diski başka bir Windows VM'ye monte edebilirsiniz.

## <a name="attach-the-disk-to-another-windows-vm-for-troubleshooting"></a>Sorun giderme için diski başka bir Windows VM'ye takın

Şimdi orijinal işletim sistemi diskinin kopyasını veri diski olarak VM'ye iliştiriyoruz. Bu işlem, yapılandırma hatalarını düzeltmenize veya diskteki ek uygulama veya sistem günlüğü dosyalarını gözden geçirmenize olanak tanır. Aşağıdaki örnek, VM `newOSDisk` adlı `RecoveryVM`diske iliştiriler.

> [!NOTE]
> Diski takmak için özgün işletim sistemi diskinin ve kurtarma VM'sinin kopyası aynı konumda olmalıdır.

```powershell
$rgName = "myResourceGroup"
$vmName = "RecoveryVM"
$location = "eastus" 
$dataDiskName = "newOSDisk"
$disk = Get-AzDisk -ResourceGroupName $rgName -DiskName $dataDiskName 

$vm = Get-AzVM -Name $vmName -ResourceGroupName $rgName 

$vm = Add-AzVMDataDisk -CreateOption Attach -Lun 0 -VM $vm -ManagedDiskId $disk.Id

Update-AzVM -VM $vm -ResourceGroupName $rgName
```

## <a name="connect-to-the-recovery-vm-and-fix-issues-on-the-attached-disk"></a>Kurtarma VM'sine bağlanın ve ekli diskteki sorunları giderin

1. RdP kurtarma VM için uygun kimlik bilgilerini kullanarak. Aşağıdaki örnek, adlı `RecoveryVM` `myResourceGroup`kaynak grubunda adı geçen VM için RDP bağlantı dosyasını indirir ve " `C:\Users\ops\Documents`

    ```powershell
    Get-AzRemoteDesktopFile -ResourceGroupName "myResourceGroup" -Name "RecoveryVM" `
        -LocalPath "C:\Users\ops\Documents\myVMRecovery.rdp"
    ```

2. Veri diski otomatik olarak algılanmalı ve iliştirilmelidir. Sürücü harfini belirlemek için ekli birimlerin listesini aşağıdaki gibi görüntüleyin:

    ```powershell
    Get-Disk
    ```

    Aşağıdaki örnek çıktı, diske bağlı **diski gösterir 2.** (Sürücü harfini `Get-Volume` görüntülemek için de kullanabilirsiniz):

    ```powershell
    Number   Friendly Name   Serial Number   HealthStatus   OperationalStatus   Total Size   Partition
                                                                                             Style
    ------   -------------   -------------   ------------   -----------------   ----------   ----------
    0        Virtual HD                                     Healthy             Online       127 GB MBR
    1        Virtual HD                                     Healthy             Online       50 GB MBR
    2        newOSDisk                                  Healthy             Online       127 GB MBR
    ```

Özgün işletim sistemi diskinin kopyası monte edilip sonra, gerektiğinde tüm bakım ve sorun giderme adımlarını gerçekleştirebilirsiniz. Sorunları giderdikten sonra aşağıdaki adımlarla devam edin.

## <a name="unmount-and-detach-original-os-disk"></a>Özgün işletim sistemi diskini sökme ve ayırma
Hatalarınız çözüldükten sonra, varolan diski kurtarma VM'nizden söküp ayırabilirsiniz. Kurtarma VM'sine diskililin kirası serbest bırakılmadan diskinizi başka bir VM ile kullanamazsınız.

1. RDP oturumunuz içinden, kurtarma VM'nizdeki veri diskini boşaltın. Bir önceki `Get-Disk` cmdletten disk numarası na ihtiyacınız var. Ardından, `Set-Disk` diski çevrimdışı olarak ayarlamak için kullanın:

    ```powershell
    Set-Disk -Number 2 -IsOffline $True
    ```

    Diskin yeniden kullanarak `Get-Disk` çevrimdışı olarak ayarlı olduğunu doğrulayın. Aşağıdaki örnek çıktı, diskin artık çevrimdışı olarak ayarlanır:

    ```powershell
    Number   Friendly Name   Serial Number   HealthStatus   OperationalStatus   Total Size   Partition
                                                                                             Style
    ------   -------------   -------------   ------------   -----------------   ----------   ----------
    0        Virtual HD                                     Healthy             Online       127 GB MBR
    1        Virtual HD                                     Healthy             Online       50 GB MBR
    2        Msft Virtu...                                  Healthy             Offline      127 GB MBR
    ```

2. RDP oturumunuzdan çıkın. Azure PowerShell oturumunuzdan, 'RecoveryVM' adlı VM'den adı geçen `newOSDisk` diski kaldırın.

    ```powershell
    $myVM = Get-AzVM -ResourceGroupName "myResourceGroup" -Name "RecoveryVM"
    Remove-AzVMDataDisk -VM $myVM -Name "newOSDisk"
    Update-AzVM -ResourceGroup "myResourceGroup" -VM $myVM
    ```

## <a name="change-the-os-disk-for-the-affected-vm"></a>Etkilenen VM için işletim sistemi diskini değiştirme

İşletim sistemi disklerini değiştirmek için Azure PowerShell'i kullanabilirsiniz. VM'yi silmek ve yeniden oluşturmak zorunda değilsiniz.

Bu örnek, VM `myVM` adlı durur ve `newOSDisk` yeni işletim sistemi diski olarak adlandırılan disk atar. 

```powershell
# Get the VM 
$vm = Get-AzVM -ResourceGroupName myResourceGroup -Name myVM 

# Make sure the VM is stopped\deallocated
Stop-AzVM -ResourceGroupName myResourceGroup -Name $vm.Name -Force

# Get the new disk that you want to swap in
$disk = Get-AzDisk -ResourceGroupName myResourceGroup -Name newDisk

# Set the VM configuration to point to the new disk  
Set-AzVMOSDisk -VM $vm -ManagedDiskId $disk.Id -Name $disk.Name  -sto

# Update the VM with the new OS disk. Possible values of StorageAccountType include: 'Standard_LRS' and 'Premium_LRS'
Update-AzVM -ResourceGroupName myResourceGroup -VM $vm -StorageAccountType <Type of the storage account >

# Start the VM
Start-AzVM -Name $vm.Name -ResourceGroupName myResourceGroup
```

## <a name="verify-and-enable-boot-diagnostics"></a>Önyükleme tanılamasını doğrulayın ve etkinleştirin

Aşağıdaki örnek, adlı `myVMDeployed` `myResourceGroup`kaynak grubunda adı geçen VM'deki tanı uzantısını sağlar:

```powershell
$myVM = Get-AzVM -ResourceGroupName "myResourceGroup" -Name "myVMDeployed"
Set-AzVMBootDiagnostics -ResourceGroupName myResourceGroup -VM $myVM -enable
Update-AzVM -ResourceGroup "myResourceGroup" -VM $myVM
```

## <a name="next-steps"></a>Sonraki adımlar
VM'nize bağlanmada sorun yaşıyorsanız, [bir Azure VM'ye bağlı SORUN Giderme RDP bağlantılarına](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)bakın. VM'nizde çalışan uygulamalara erişimle ilgili sorunlar için [Windows VM'deki Sorun Giderme uygulaması bağlantısı sorunlarına](troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)bakın.

Kaynak Yöneticisi'ni kullanma hakkında daha fazla bilgi için [Azure Kaynak Yöneticisi'ne genel bakış](../../azure-resource-manager/management/overview.md)bilgisine bakın.
