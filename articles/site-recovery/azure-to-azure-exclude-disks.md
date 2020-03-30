---
title: Azure Site Kurtarma ve Azure PowerShell ile Azure VM disklerini çoğaltmadan hariç tutma
description: Azure PowerShell'i kullanarak Azure Site Kurtarma sırasında Azure sanal makinelerindisklerini nasıl dışlayacağınızı öğrenin.
author: sideeksh
manager: rochakm
ms.topic: how-to
ms.date: 02/18/2019
ms.openlocfilehash: 7355233bb7241571e3f3820aafac6952af245654
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75973669"
---
# <a name="exclude-disks-from-powershell-replication-of-azure-vms"></a>Azure VM'lerin PowerShell çoğaltmasından diskleri hariç tutma

Bu makalede, Azure VM'leri çoğalttırken disklerin nasıl dışlanır olduğu açıklanmaktadır. Tüketilen çoğaltma bant genişliğini veya bu disklerin kullandığı hedef tarafındaki kaynakları en iyi duruma getirmek için diskleri hariç tutabilirsiniz. Şu anda bu özellik yalnızca Azure PowerShell üzerinden kullanılabilir.


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce:

- Olağanüstü durum kurtarma [mimarisini ve bileşenlerini](azure-to-azure-architecture.md)anladığınızdan emin olun.
- Tüm bileşenler için [destek gereksinimlerini](azure-to-azure-support-matrix.md) gözden geçirin.
- AzureRm PowerShell "Az" modülüne sahip olduğundan emin olun. PowerShell'i yüklemek veya güncelleştirmek için Azure [PowerShell modüllerini yükleyin'e](https://docs.microsoft.com/powershell/azure/install-az-ps)bakın.
- Kurtarma hizmetleri kasası oluşturduğunuzdan ve sanal makineleri en az bir kez koruduğunuzdan emin olun. Bunları yapmadıysanız, [Azure PowerShell'i kullanarak Azure sanal makineleri için olağanüstü durum kurtarma ayarlama](azure-to-azure-powershell.md)işlemini izleyin.
- Çoğaltma için etkin leştirilmiş bir Azure VM'sine disk ekleme hakkında bilgi arıyorsanız, [bu makaleyi inceleyin.](azure-to-azure-enable-replication-added-disk.md)

## <a name="why-exclude-disks-from-replication"></a>Diskleri çoğaltmadan neden dışla
Diskleri çoğaltmadan çıkarmanız gerekebilir, çünkü:

- Sanal [makineniz, veri değiştirme oranlarını çoğaltmak için Azure Site Kurtarma sınırlarına](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-support-matrix)ulaştı.

- Dışlanan diskte çalkalanan veriler önemli değildir veya çoğaltılması gerekmez.

- Verileri çoğaltmayarak depolama ve ağ kaynaklarını kaydetmek istiyorsunuz.

## <a name="how-to-exclude-disks-from-replication"></a>Diskleri çoğaltmadan dışlama

Örneğimizde, Doğu ABD bölgesinde bulunan bir işletim sistemi ve üç veri diski olan sanal bir makineyi Batı ABD 2 bölgesine kopyalıyoruz. Sanal makinenin adı *AzureDemoVM'dir.* Disk 1'i hariç tutuyoruz ve disk2 ve 3'te tutuyoruz.

## <a name="get-details-of-the-virtual-machines-to-replicate"></a>Çoğaltmak için sanal makinelerin ayrıntılarını alın

```azurepowershell
# Get details of the virtual machine
$VM = Get-AzVM -ResourceGroupName "A2AdemoRG" -Name "AzureDemoVM"

Write-Output $VM     
```

```
ResourceGroupName  : A2AdemoRG
Id                 : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/A2AdemoRG/providers/Microsoft.Compute/virtualMachines/AzureDemoVM
VmId               : 1b864902-c7ea-499a-ad0f-65da2930b81b
Name               : AzureDemoVM
Type               : Microsoft.Compute/virtualMachines
Location           : eastus
Tags               : {}
DiagnosticsProfile : {BootDiagnostics}
HardwareProfile    : {VmSize}
NetworkProfile     : {NetworkInterfaces}
OSProfile          : {ComputerName, AdminUsername, WindowsConfiguration, Secrets}
ProvisioningState  : Succeeded
StorageProfile     : {ImageReference, OsDisk, DataDisks}
```

Sanal makinenin diskleri hakkında ayrıntılı bilgi alın. Bu bilgiler daha sonra VM çoğaltma başlattığınızda kullanılacaktır.

```azurepowershell
$OSDiskVhdURI = $VM.StorageProfile.OsDisk.Vhd
$DataDisk1VhdURI = $VM.StorageProfile.DataDisks[0].Vhd
```

## <a name="replicate-an-azure-virtual-machine"></a>Azure sanal makineyi çoğaltma

Aşağıdaki örnekiçin, zaten bir önbellek depolama hesabınız, çoğaltma ilkeniz ve eşlemeniz olduğunu varsayıyoruz. Bunlara sahip değilseniz, [Azure PowerShell'i kullanarak Azure sanal makineleri için olağanüstü durum kurtarma ayarlama](azure-to-azure-powershell.md)işlemini izleyin.

*Yönetilen disklerle*bir Azure sanal makinesini çoğaltma.

```azurepowershell

#Get the resource group that the virtual machine must be created in when failed over.
$RecoveryRG = Get-AzResourceGroup -Name "a2ademorecoveryrg" -Location "West US 2"

#Specify replication properties for each disk of the VM that is to be replicated (create disk replication configuration).

#OsDisk
$OSdiskId =  $vm.StorageProfile.OsDisk.ManagedDisk.Id
$RecoveryOSDiskAccountType = $vm.StorageProfile.OsDisk.ManagedDisk.StorageAccountType
$RecoveryReplicaDiskAccountType =  $vm.StorageProfile.OsDisk.ManagedDisk.StorageAccountType

$OSDiskReplicationConfig = New-AzRecoveryServicesAsrAzureToAzureDiskReplicationConfig -ManagedDisk -LogStorageAccountId $EastUSCacheStorageAccount.Id `
         -DiskId $OSdiskId -RecoveryResourceGroupId  $RecoveryRG.ResourceId -RecoveryReplicaDiskAccountType  $RecoveryReplicaDiskAccountType `
         -RecoveryTargetDiskAccountType $RecoveryOSDiskAccountType

# Data Disk 1 i.e StorageProfile.DataDisks[0] is excluded, so we will provide it during the time of replication.

# Data disk 2
$datadiskId2  = $vm.StorageProfile.DataDisks[1].ManagedDisk.id
$RecoveryReplicaDiskAccountType =  $vm.StorageProfile.DataDisks[1]. StorageAccountType
$RecoveryTargetDiskAccountType = $vm.StorageProfile.DataDisks[1]. StorageAccountType

$DataDisk2ReplicationConfig  = New-AzRecoveryServicesAsrAzureToAzureDiskReplicationConfig -ManagedDisk -LogStorageAccountId $CacheStorageAccount.Id `
         -DiskId $datadiskId2 -RecoveryResourceGroupId  $RecoveryRG.ResourceId -RecoveryReplicaDiskAccountType  $RecoveryReplicaDiskAccountType `
         -RecoveryTargetDiskAccountType $RecoveryTargetDiskAccountType

# Data Disk 3

$datadiskId3  = $vm.StorageProfile.DataDisks[2].ManagedDisk.id
$RecoveryReplicaDiskAccountType =  $vm.StorageProfile.DataDisks[2]. StorageAccountType
$RecoveryTargetDiskAccountType = $vm.StorageProfile.DataDisks[2]. StorageAccountType

$DataDisk3ReplicationConfig  = New-AzRecoveryServicesAsrAzureToAzureDiskReplicationConfig -ManagedDisk -LogStorageAccountId $CacheStorageAccount.Id `
         -DiskId $datadiskId3 -RecoveryResourceGroupId  $RecoveryRG.ResourceId -RecoveryReplicaDiskAccountType  $RecoveryReplicaDiskAccountType `
         -RecoveryTargetDiskAccountType $RecoveryTargetDiskAccountType

#Create a list of disk replication configuration objects for the disks of the virtual machine that are to be replicated.
$diskconfigs = @()
$diskconfigs += $OSDiskReplicationConfig, $DataDisk2ReplicationConfig, $DataDisk3ReplicationConfig


#Start replication by creating a replication protected item. Using a GUID for the name of the replication protected item to ensure uniqueness of name.
$TempASRJob = New-ASRReplicationProtectedItem -AzureToAzure -AzureVmId $VM.Id -Name (New-Guid).Guid -ProtectionContainerMapping $EusToWusPCMapping -AzureToAzureDiskReplicationConfiguration $diskconfigs -RecoveryResourceGroupId $RecoveryRG.ResourceId
```

Başlangıç çoğaltma işlemi başarılı olduğunda, VM verileri kurtarma bölgesine çoğaltılır.

Azure portalına gidebilir ve çoğaltılan VM'leri "çoğaltılan öğeler" altında görebilirsiniz.

Çoğaltma işlemi, kurtarma bölgesindeki sanal makinenin çoğaltma disklerinin bir kopyasını tohumlayarak başlar. Bu aşama, ilk çoğaltma aşaması olarak adlandırılır.

İlk çoğaltma bittikten sonra, çoğaltma diferansiyel eşitleme aşamasına geçer. Bu noktada, sanal makine korunur. Disklerin dışlanmış olup olmadığını görmek için korumalı sanal makineyi seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bir [test başarısızlığını çalıştırma](site-recovery-test-failover-to-azure.md)hakkında bilgi edinin.
