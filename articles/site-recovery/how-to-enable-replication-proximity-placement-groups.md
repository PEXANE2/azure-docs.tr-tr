---
title: Yakınlık yerleşimi gruplarında çalışan Azure VM 'lerini çoğaltma
description: Azure Site Recovery kullanarak yakınlık yerleşimi gruplarında çalışan Azure sanal makinelerini nasıl çoğaltacağınızı öğrenin.
author: Sharmistha-Rai
manager: gaggupta
ms.topic: how-to
ms.date: 05/25/2020
ms.openlocfilehash: 204ac3be46ac7ba0e1ea96e50379ca417b1299ce
ms.sourcegitcommit: 1f25aa993c38b37472cf8a0359bc6f0bf97b6784
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/26/2020
ms.locfileid: "83847642"
---
# <a name="replicate-azure-virtual-machines-running-in-proximity-placement-groups-to-another-region"></a>Yakınlık yerleştirme gruplarında çalışan Azure sanal makinelerini başka bir bölgeye çoğaltma

Bu makalede, bir yakınlık yerleştirme grubunda çalışan sanal makinelerin ikincil bir bölgeye nasıl çoğaltılacağı, yük devretmeyle ve yeniden çalıştırıldığı açıklanmaktadır.

[Yakınlık yerleştirme grupları](https://docs.microsoft.com/azure/virtual-machines/windows/proximity-placement-groups-portal) , UYGULAMALARıNıZLA ilişkili VM 'ler arası ağ gecikmesini azaltmak için kullanabileceğiniz bir Azure sanal makine mantıksal gruplandırma özelliğidir. VM 'Ler aynı yakınlık yerleşimi grubu içinde dağıtıldığında, fiziksel olarak birbirlerine mümkün olduğunca yakın şekilde bulunur. Yakınlık yerleşimi grupları özellikle gecikme süresine duyarlı iş yüklerinin gereksinimlerini karşılamak için yararlıdır.

## <a name="disaster-recovery-with-proximity-placement-groups"></a>Yakınlık yerleştirme gruplarıyla olağanüstü durum kurtarma

Tipik bir senaryoda, uygulamanızın çeşitli katmanları arasındaki ağ gecikmesini önlemek için sanal makineleriniz bir yakınlık yerleşimi grubunda çalışıyor olabilir. Bu, uygulamanızın en iyi ağ gecikmesini sağlayabildiği sürece, herhangi bir bölge düzeyindeki başarısızlık için Site Recovery kullanarak bu uygulamaları korumak istersiniz. Site Recovery, verileri bir bölgeden başka bir Azure bölgesine çoğaltır ve yük devretme durumunda makineleri olağanüstü durum kurtarma bölgesinde getirir.

## <a name="considerations"></a>Dikkat edilmesi gerekenler

- En iyi çaba, sanal makineleri bir yakınlık yerleşimi grubuna devretmek/yeniden çalıştırmanız olacaktır. Ancak, VM yük devretme/yeniden çalışma sırasında yakınlık yerleşimi içinde getirilemeiyorsa, yük devretme/yeniden çalışma işlemi devam eder ve sanal makineler bir yakınlık yerleşimi grubu dışında oluşturulur.
-  Bir kullanılabilirlik kümesi bir yakınlık yerleşimi grubuna sabitlenmiştir ve kullanılabilirlik kümesindeki yük devretme/yeniden çalışma VM 'Leri için bir ayırma kısıtlaması varsa, sanal makineler hem kullanılabilirlik kümesi hem de yakınlık yerleşimi grubu dışında oluşturulur.
-  Yönetilmeyen diskler için Site Recovery yakınlık yerleştirme grupları desteklenmez.

## <a name="prerequisites"></a>Ön koşullar

1. Azure PowerShell az modüle sahip olduğunuzdan emin olun. Azure PowerShell yüklemeniz veya yükseltmeniz gerekiyorsa, [Azure PowerShell yüklemek ve yapılandırmak için bu kılavuzu](https://docs.microsoft.com/powershell/azure/install-az-ps)izleyin.

## <a name="set-up-site-recovery-for-virtual-machines-in-proximity-placement-group"></a>Yakınlık yerleştirme grubundaki sanal makineler için Site Recovery ayarlama

### <a name="azure-to-azure"></a>Azure-Azure arası

1. Hesabınızda [oturum açın](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-powershell#sign-in-to-your-microsoft-azure-subscription) ve aboneliğinizi ayarlayın.
2. Çoğaltmak istediğiniz sanal makinenin ayrıntılarını [burada](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-powershell#get-details-of-the-virtual-machine-to-be-replicated)belirtildiği gibi alın.
3. Kurtarma Hizmetleri kasanızı [oluşturun](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-powershell#create-a-recovery-services-vault) ve kasa bağlamını [ayarlayın](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-powershell#set-the-vault-context) .
4. Kasayı çoğaltma sanal makinesini başlatacak şekilde hazırlayın. Bu, hem birincil hem de kurtarma bölgeleri için bir [Service Fabric nesnesi](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-powershell#create-a-site-recovery-fabric-object-to-represent-the-primary-source-region) oluşturulmasını içerir.
5. Hem birincil hem de kurtarma yapıları için Site Recovery bir koruma kapsayıcısı [oluşturun](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-powershell#create-a-site-recovery-protection-container-in-the-primary-fabric) .
6. Çoğaltma İlkesi [oluşturun](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-powershell#create-a-replication-policy) .
7. [Bu](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-powershell#create-a-protection-container-mapping-between-the-primary-and-recovery-protection-container) adımları ve [burada](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-powershell#create-a-protection-container-mapping-for-failback-reverse-replication-after-a-failover)belirtildiği gibi yeniden çalışma için bir koruma kapsayıcısı eşlemesini kullanarak birincil ve kurtarma koruma kapsayıcısı arasında bir koruma kapsayıcısı eşlemesi oluşturun.
8. [Aşağıdaki adımları izleyerek](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-powershell#create-cache-storage-account-and-target-storage-account) önbellek depolama hesabı oluşturun.
9. [Burada](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-powershell#create-network-mappings)belirtildiği gibi gerekli ağ eşlemelerini oluşturun.
10. Azure sanal makinesini yönetilen disklerle çoğaltmak için aşağıdaki PowerShell cmdlet 'ini kullanın: 

```azurepowershell
#Get the resource group that the virtual machine must be created in when failed over.
$RecoveryRG = Get-AzResourceGroup -Name "a2ademorecoveryrg" -Location "West US 2"

#Specify replication properties for each disk of the VM that is to be replicated (create disk replication configuration)

#OsDisk
$OSdiskId = $vm.StorageProfile.OsDisk.ManagedDisk.Id
$RecoveryOSDiskAccountType = $vm.StorageProfile.OsDisk.ManagedDisk.StorageAccountType
$RecoveryReplicaDiskAccountType = $vm.StorageProfile.OsDisk.ManagedDisk.StorageAccountType

$OSDiskReplicationConfig = New-AzRecoveryServicesAsrAzureToAzureDiskReplicationConfig -ManagedDisk -LogStorageAccountId $EastUSCacheStorageAccount.Id ` -DiskId $OSdiskId -RecoveryResourceGroupId  $RecoveryRG.ResourceId -RecoveryReplicaDiskAccountType  $RecoveryReplicaDiskAccountType ` -RecoveryTargetDiskAccountType $RecoveryOSDiskAccountType -RecoveryProximityPlacementGroupId $recPpg.Id

# Data disk
$datadiskId1 = $vm.StorageProfile.DataDisks[0].ManagedDisk.Id
$RecoveryReplicaDiskAccountType = $vm.StorageProfile.DataDisks[0].ManagedDisk.StorageAccountType
$RecoveryTargetDiskAccountType = $vm.StorageProfile.DataDisks[0].ManagedDisk.StorageAccountType

$DataDisk1ReplicationConfig  = New-AzRecoveryServicesAsrAzureToAzureDiskReplicationConfig -ManagedDisk -LogStorageAccountId $CacheStorageAccount.Id ` -DiskId $datadiskId1 -RecoveryResourceGroupId $RecoveryRG.ResourceId -RecoveryReplicaDiskAccountType $RecoveryReplicaDiskAccountType ` -RecoveryTargetDiskAccountType $RecoveryTargetDiskAccountType

#Create a list of disk replication configuration objects for the disks of the virtual machine that are to be replicated.

$diskconfigs = @()
$diskconfigs += $OSDiskReplicationConfig, $DataDisk1ReplicationConfig

#Start replication by creating replication protected item. Using a GUID for the name of the replication protected item to ensure uniqueness of name.

$TempASRJob = New-AzRecoveryServicesAsrReplicationProtectedItem -AzureToAzure -AzureVmId $VM.Id -Name (New-Guid).Guid -ProtectionContainerMapping $EusToWusPCMapping -AzureToAzureDiskReplicationConfiguration $diskconfigs -RecoveryResourceGroupId $RecoveryRG.ResourceId -RecoveryProximityPlacementGroupId $recPpg.Id
```
Çoğaltma başlatma işlemi başarılı olduktan sonra, sanal makine verileri kurtarma bölgesine çoğaltılır.

Çoğaltma işlemi başlangıçta, kurtarma bölgesindeki sanal makinenin çoğaltılan disklerinin bir kopyasını dengeli olarak başlatır. Bu aşamaya ilk çoğaltma aşaması denir.

İlk çoğaltma tamamlandıktan sonra çoğaltma, fark eşitleme aşamasına gider. Bu noktada, sanal makine korunur ve üzerinde bir test yük devretmesi işlemi gerçekleştirilebilir. İlk çoğaltma tamamlandıktan sonra, sanal makineyi temsil eden çoğaltılan öğenin çoğaltma durumu korunan duruma geçer.

Kendisine karşılık gelen çoğaltma korumalı öğenin ayrıntılarını alarak sanal makinenin çoğaltma durumunu ve çoğaltma durumunu izleyin. 

```azurepowershell
Get-AzRecoveryServicesAsrReplicationProtectedItem -ProtectionContainer $PrimaryProtContainer | Select FriendlyName, ProtectionState, ReplicationHealth
```

11. Yük devretme testini doğrulamak, test yük devretmesini doğrulamak ve temizlemek için [aşağıdaki](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-powershell#do-a-test-failover-validate-and-cleanup-test-failover) adımları izleyin.
12. Yük devretme için [burada](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-powershell#fail-over-to-azure)bahsedilen adımları izleyin.
13. Kaynak bölgeye yeniden koruma ve yeniden çalışma için aşağıdaki PowerShell cmdlet 'ini kullanın:

```azurepowershell
#Create Cache storage account for replication logs in the primary region
$WestUSCacheStorageAccount = New-AzStorageAccount -Name "a2acachestoragewestus" -ResourceGroupName "A2AdemoRG" -Location 'West US' -SkuName Standard_LRS -Kind Storage


#Use the recovery protection container, new cache storage account in West US and the source region VM resource group 
Update-AzRecoveryServicesAsrProtectionDirection -ReplicationProtectedItem $ReplicationProtectedItem -AzureToAzure -ProtectionContainerMapping $WusToEusPCMapping -LogStorageAccountId $WestUSCacheStorageAccount.Id -RecoveryResourceGroupID $sourceVMResourcegroup.ResourceId -RecoveryProximityPlacementGroupId $vm.ProximityPlacementGroup.Id
```
14. Çoğaltmayı devre dışı bırakmak için [buradaki](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-powershell#disable-replication)adımları izleyin.

### <a name="vmware-to-azure"></a>Vmware’den Azure’a

1. Şirket [Içi VMware sunucularını](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial-prepare-on-premises) Azure 'a olağanüstü durum kurtarma için hazırladığınızdan emin olun.
2. Hesabınızda oturum açın ve aboneliğinizi [burada](https://docs.microsoft.com/azure/site-recovery/vmware-azure-disaster-recovery-powershell#log-into-azure)belirtilen şekilde ayarlayın.
3. Bir kurtarma hizmetleri Kasası [kurun](https://docs.microsoft.com/azure/site-recovery/vmware-azure-disaster-recovery-powershell#set-up-a-recovery-services-vault) ve [kasa bağlamını ayarlayın](https://docs.microsoft.com/azure/site-recovery/vmware-azure-disaster-recovery-powershell#set-the-vault-context).
4. Kasa kaydınızı [doğrulayın](https://docs.microsoft.com/azure/site-recovery/vmware-azure-disaster-recovery-powershell#validate-vault-registration) .
5. Çoğaltma İlkesi [oluşturun](https://docs.microsoft.com/azure/site-recovery/vmware-azure-disaster-recovery-powershell#create-a-replication-policy) .
6. Bir vCenter sunucusu [ekleyin](https://docs.microsoft.com/azure/site-recovery/vmware-azure-disaster-recovery-powershell#add-a-vcenter-server-and-discover-vms) ve sanal makineleri bulun ve çoğaltma için depolama hesapları [oluşturun](https://docs.microsoft.com/azure/site-recovery/vmware-azure-disaster-recovery-powershell#create-storage-accounts-for-replication) .
7. VMware sanal makinelerini çoğaltmak için buradaki ayrıntıları denetleyin ve aşağıdaki PowerShell cmdlet 'ini izleyin:

```azurepowershell
#Get the target resource group to be used
$ResourceGroup = Get-AzResourceGroup -Name "VMwareToAzureDrPs"

#Get the target virtual network to be used
$RecoveryVnet = Get-AzVirtualNetwork -Name "ASR-vnet" -ResourceGroupName "asrrg" 

#Get the protection container mapping for replication policy named ReplicationPolicy
$PolicyMap = Get-AzRecoveryServicesAsrProtectionContainerMapping -ProtectionContainer $ProtectionContainer | where PolicyFriendlyName -eq "ReplicationPolicy"

#Get the protectable item corresponding to the virtual machine CentOSVM1
$VM1 = Get-AzRecoveryServicesAsrProtectableItem -ProtectionContainer $ProtectionContainer -FriendlyName "CentOSVM1"

# Enable replication for virtual machine CentOSVM1 using the Az.RecoveryServices module 2.0.0 onwards to replicate to managed disks
# The name specified for the replicated item needs to be unique within the protection container. Using a random GUID to ensure uniqueness
$Job_EnableReplication1 = New-AzRecoveryServicesAsrReplicationProtectedItem -VMwareToAzure -ProtectableItem $VM1 -Name (New-Guid).Guid -ProtectionContainerMapping $PolicyMap -ProcessServer $ProcessServers[1] -Account $AccountHandles[2] -RecoveryResourceGroupId $ResourceGroup.ResourceId -logStorageAccountId $LogStorageAccount.Id -RecoveryAzureNetworkId $RecoveryVnet.Id -RecoveryAzureSubnetName "Subnet-1" -RecoveryProximityPlacementGroupId $recPpg.Id
```
8. Get-Asrreplicationkorunabilir Dıdıtem cmdlet 'ini kullanarak sanal makinenin çoğaltma durumunu ve çoğaltma durumunu kontrol edebilirsiniz.

```azurepowershell
Get-AzRecoveryServicesAsrReplicationProtectedItem -ProtectionContainer $ProtectionContainer | Select FriendlyName, ProtectionState, ReplicationHealth
```
9. [Buradaki](https://docs.microsoft.com/azure/site-recovery/vmware-azure-disaster-recovery-powershell#configure-failover-settings)adımları izleyerek yük devretme ayarlarını yapılandırın.
10. Yük devretme testi [çalıştırın](https://docs.microsoft.com/azure/site-recovery/vmware-azure-disaster-recovery-powershell#run-a-test-failover) . 
11. [Bu](https://docs.microsoft.com/azure/site-recovery/vmware-azure-disaster-recovery-powershell#fail-over-to-azure) adımları kullanarak Azure 'a yük devretme.

### <a name="hyper-v-to-azure"></a>Hyper-V’den Azure’a

1. Şirket [Içi Hyper-V sunucularınızı](https://docs.microsoft.com/azure/site-recovery/hyper-v-prepare-on-premises-tutorial) Azure 'a olağanüstü durum kurtarma için hazırlayın.
2. Azure ['Da oturum açın](https://docs.microsoft.com/azure/site-recovery/hyper-v-azure-powershell-resource-manager#step-1-sign-in-to-your-azure-account) .
3. Kasanızı [ayarlayın](https://docs.microsoft.com/azure/site-recovery/hyper-v-azure-powershell-resource-manager#step-2-set-up-the-vault) ve kurtarma hizmetleri Kasası bağlamını [ayarlayın](https://docs.microsoft.com/azure/site-recovery/hyper-v-azure-powershell-resource-manager#step-3-set-the-recovery-services-vault-context) .
4. Hyper-V sitesi [oluşturun](https://docs.microsoft.com/azure/site-recovery/hyper-v-azure-powershell-resource-manager#step-4-create-a-hyper-v-site) .
5. Sağlayıcıyı ve aracıyı [yükler](https://docs.microsoft.com/azure/site-recovery/hyper-v-azure-powershell-resource-manager#step-5-install-the-provider-and-agent) .
6. Çoğaltma İlkesi [oluşturun](https://docs.microsoft.com/azure/site-recovery/hyper-v-azure-powershell-resource-manager#step-6-create-a-replication-policy) .
7. Aşağıdaki adımları kullanarak çoğaltmayı etkinleştirin – 
    
    a. Korumak istediğiniz VM 'ye karşılık gelen korunabilir öğeyi aşağıdaki gibi alın:

    ```azurepowershell
    $VMFriendlyName = "Fabrikam-app"          #Name of the VM
    $ProtectableItem = Get-AzRecoveryServicesAsrProtectableItem -ProtectionContainer $protectionContainer -FriendlyName $VMFriendlyName
    ```
    b. VM 'yi koruyun. Koruduğunuz sanal makineye birden fazla disk ekli ise, OSDiskName parametresini kullanarak işletim sistemi diskini belirtin.
    
    ```azurepowershell
    $OSType = "Windows"          # "Windows" or "Linux"
    $DRjob = New-AzRecoveryServicesAsrReplicationProtectedItem -ProtectableItem $VM -Name $VM.Name -ProtectionContainerMapping $ProtectionContainerMapping -RecoveryAzureStorageAccountId   $StorageAccountID -OSDiskName $OSDiskNameList[$i] -OS $OSType -RecoveryResourceGroupId $ResourceGroupID -RecoveryProximityPlacementGroupId $recPpg.Id
    ```
    c. İlk çoğaltmadan sonra VM 'Lerin korumalı duruma ulaşmasını bekleyin. Bu işlem, çoğaltılacak veri miktarı ve Azure için kullanılabilir yukarı akış bant genişliği gibi etkenlere bağlı olarak biraz zaman alabilir. Korunan bir durum olduğunda, iş durumu ve StateDescription aşağıdaki gibi güncelleştirilir: 
    
    ```azurepowershell
    $DRjob = Get-AzRecoveryServicesAsrJob -Job $DRjob
    $DRjob | Select-Object -ExpandProperty State

    $DRjob | Select-Object -ExpandProperty StateDescription
    ```
    d. Yük devretmeden sonra kurtarma özelliklerini (VM rolü boyutu gibi) ve VM NIC 'nin ekleneceği Azure ağını güncelleştirin.

    ```azurepowershell
    $nw1 = Get-AzVirtualNetwork -Name "FailoverNw" -ResourceGroupName "MyRG"

    $VMFriendlyName = "Fabrikam-App"

    $rpi = Get-AzRecoveryServicesAsrReplicationProtectedItem -ProtectionContainer $protectionContainer -FriendlyName $VMFriendlyName

    $UpdateJob = Set-AzRecoveryServicesAsrReplicationProtectedItem -InputObject $rpi -PrimaryNic $VM.NicDetailsList[0].NicId -RecoveryNetworkId $nw1.Id -RecoveryNicSubnetName $nw1.Subnets[0].Name

    $UpdateJob = Get-AzRecoveryServicesAsrJob -Job $UpdateJob

    $UpdateJob | Select-Object -ExpandProperty state

    Get-AzRecoveryServicesAsrJob -Job $job | Select-Object -ExpandProperty state
    ```
8. [Yük devretme](https://docs.microsoft.com/azure/site-recovery/hyper-v-azure-powershell-resource-manager#step-8-run-a-test-failover)testi çalıştırın.


## <a name="next-steps"></a>Sonraki adımlar

VMware 'den Azure 'a yeniden koruma ve yeniden çalışma gerçekleştirmek için [burada](https://docs.microsoft.com/azure/site-recovery/vmware-azure-prepare-failback)özetlenen adımları izleyin.

Azure 'da Hyper-V ' d e yük devretme gerçekleştirmek için [burada](https://docs.microsoft.com/azure/site-recovery/site-recovery-failover) özetlenen adımları izleyin ve yeniden çalışma gerçekleştirmek için [burada](https://docs.microsoft.com/azure/site-recovery/hyper-v-azure-failback)özetlenen adımları izleyin.

Daha fazla bilgi için bkz. [Site Recovery 'de yük devretme](site-recovery-failover.md).
