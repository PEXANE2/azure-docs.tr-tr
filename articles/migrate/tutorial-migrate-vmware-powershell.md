---
title: VMware VM 'lerini Azure 'a geçirme (aracısız)-PowerShell
description: PowerShell aracılığıyla Azure geçişi ile, VMware VM 'lerinin aracısız geçişini nasıl çalıştıracağınızı öğrenin.
author: rahulg1190
ms.author: rahugup
manager: bsiva
ms.topic: tutorial
ms.date: 03/02/2021
ms.openlocfilehash: 24dd33495915a9f4d47a00fbbfe9e894df839d4d
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101715080"
---
# <a name="migrate-vmware-vms-to-azure-agentless---powershell"></a>VMware VM 'lerini Azure 'a geçirme (aracısız)-PowerShell

Bu makalede, [Azure geçişi: sunucu geçişi](migrate-services-overview.md#azure-migrate-server-migration-tool)için Azure PowerShell kullanarak, bulunan VMware VM 'lerini aracısız yöntemiyle nasıl geçirebileceğinizi öğreneceksiniz.

Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Bulunan VMware VM 'lerini bir Azure geçişi projesinde alın.
> * VM 'Leri çoğaltmaya başlayın.
> * VM 'Leri çoğaltmak için özellikleri güncelleştirin.
> * Çoğaltmayı izleme.
> * Her şeyin beklendiği gibi çalıştığından emin olmak için bir test geçişi çalıştırın.
> * Tam VM geçişini çalıştırın.

> [!NOTE]
> Öğreticiler, bir senaryo için en basit dağıtım yolunu gösterir, böylece bir kavram kanıtı hızlı bir şekilde ayarlayabilmenizi sağlayabilirsiniz. Öğreticiler mümkün olduğunca varsayılan seçenekleri kullanır ve tüm olası ayarları ve yolları göstermez.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturun.

## <a name="1-prerequisites"></a>1. ön koşullar

Bu öğreticiye başlamadan önce karşılamanız gereken ön koşullar şunlardır:

1. Öğreticiyi doldurun: Azure ve VMware 'yi geçişe hazırlamak için [Sunucu değerlendirmesi Ile VMware VM 'Lerini bulun](tutorial-discover-vmware.md) .
2. Öğreticiyi doldurun: VMware VM 'lerini Azure 'a geçirmeden önce [Azure VM 'lerine geçiş Için değerlendirin](./tutorial-assess-vmware-azure-vm.md) .
3. [Az PowerShell modülünü yükler](/powershell/azure/install-az-ps)

## <a name="2-install-azure-migrate-powershell-module"></a>2. Azure geçişi PowerShell modülünü yükler

Azure geçişi PowerShell modülü Azure PowerShell () bir parçası olarak kullanılabilir `Az` . `Get-InstalledModule -Name Az.Migrate`Makinenizde Azure geçişi PowerShell modülünün yüklü olup olmadığını denetlemek için komutunu çalıştırın.  

## <a name="3-sign-in-to-your-microsoft-azure-subscription"></a>3. Microsoft Azure aboneliğinizde oturum açın

[Connect-AzAccount](/powershell/module/az.accounts/connect-azaccount) cmdlet 'ı ile Azure aboneliğinizde oturum açın.

```azurepowershell-interactive
Connect-AzAccount
```

### <a name="select-your-azure-subscription"></a>Azure aboneliğinizi seçin

Erişiminiz olan Azure aboneliklerinin listesini almak için [Get-AzSubscription](/powershell/module/az.accounts/get-azsubscription) cmdlet 'ini kullanın. [Set-AzContext](/powershell/module/az.accounts/set-azcontext) cmdlet 'ini kullanarak çalışmak Için Azure geçişi projenizin bulunduğu Azure aboneliğini seçin.

```azurepowershell-interactive
Set-AzContext -SubscriptionId 00000000-0000-0000-0000-000000000000
```

## <a name="4-retrieve-the-azure-migrate-project"></a>4. Azure geçişi projesini alma

Değerlendirme veya geçiş yaptığınız ortamdan toplanan bulma, değerlendirme ve geçiş meta verilerini depolamak için bir Azure geçişi projesi kullanılır.
Bir projede keşfedilen varlıkları izleyebilir, değerlendirmeleri düzenleyebilir ve geçişleri gerçekleştirebilirsiniz.

Önkoşulların bir parçası olarak, zaten bir Azure geçişi projesi oluşturmuş olursunuz. Azure geçişi projesinin ayrıntılarını almak için [Get-AzMigrateProject](/powershell/module/az.migrate/get-azmigrateproject) cmdlet 'ini kullanın. Azure geçişi projesinin ( `Name` ) adını ve Azure geçişi projesinin () kaynak grubunun adını belirtmeniz gerekir `ResourceGroupName` .

```azurepowershell-interactive
# Get resource group of the Azure Migrate project
$ResourceGroup = Get-AzResourceGroup -Name MyResourceGroup

# Get details of the Azure Migrate project
$MigrateProject = Get-AzMigrateProject -Name MyMigrateProject -ResourceGroupName $ResourceGroup.ResourceGroupName

# View Azure Migrate project details
Write-Output $MigrateProject
```

## <a name="5-retrieve-discovered-vms-in-an-azure-migrate-project"></a>5. bulunan VM 'Leri bir Azure geçişi projesinde alma

Azure geçişi basit bir [Azure geçiş](migrate-appliance-architecture.md)gereci kullanır. Önkoşulların bir parçası olarak Azure geçişi gereci bir VMware VM 'si olarak dağıttığınızı görürsünüz.

Azure geçişi projesindeki belirli bir VMware VM 'sini almak için Azure geçişi projesi ( `ProjectName` ), Azure geçişi projesinin kaynak grubu ( `ResourceGroupName` ) ve VM adı () adını belirtin `DisplayName` .


```azurepowershell-interactive
# Get a specific VMware VM in an Azure Migrate project
$DiscoveredServer = Get-AzMigrateDiscoveredServer -ProjectName $MigrateProject.Name -ResourceGroupName $ResourceGroup.ResourceGroupName -DisplayName MyTestVM | Format-Table DisplayName, Name, Type

# View discovered server details
Write-Output $DiscoveredServer
```

Bu öğreticinin bir parçası olarak bu VM 'yi geçireceğiz.

Ayrıca, ( `ProjectName` ) ve () parametrelerini kullanarak bir Azure geçişi projesindeki tüm VMware VM 'lerini de alabilirsiniz `ResourceGroupName` .

```azurepowershell-interactive
# Get all VMware VMs in an Azure Migrate project
$DiscoveredServers = Get-AzMigrateDiscoveredServer -ProjectName $MigrateProject.Name -ResourceGroupName $ResourceGroup.ResourceGroupName
```

Azure geçişi projesinde birden çok gereç varsa, `ProjectName` `ResourceGroupName` `ApplianceName` belirli bir Azure geçişi gereci kullanarak bulunan tüm VM 'leri almak için (), () ve () parametrelerini kullanabilirsiniz.

```azurepowershell-interactive
# Get all VMware VMs discovered by an Azure Migrate Appliance in an Azure Migrate project
$DiscoveredServers = Get-AzMigrateDiscoveredServer -ProjectName $MigrateProject.Name -ResourceGroupName $ResourceGroup.ResourceGroupName -ApplianceName MyMigrateAppliance

```

## <a name="6-initialize-replication-infrastructure"></a>6. çoğaltma altyapısını başlatma

[Azure geçişi: sunucu geçişi](migrate-services-overview.md#azure-migrate-server-migration-tool) , VM 'leri geçirmek için birden çok Azure kaynağı kullanır. Sunucu geçişi, proje ile aynı kaynak grubunda aşağıdaki kaynakları sağlar.

- **Service Bus**: sunucu geçişi, yüklemeye çoğaltma düzenleme iletileri göndermek için Service Bus 'ı kullanır.
- **Ağ geçidi depolama hesabı**: sunucu geçişi, çoğaltılan VM 'ler hakkında durum bilgilerini depolamak için ağ geçidi depolama hesabını kullanır.
- **Günlük depolama hesabı**: Azure geçişi gereci, VM 'ler için çoğaltma günlüklerini bir günlük depolama hesabına yükler. Azure geçişi, çoğaltma bilgilerini çoğaltma tarafından yönetilen disklere uygular.
- **Anahtar Kasası**: Azure geçişi gereci, hizmet veri yolu için bağlantı dizelerini yönetmek üzere anahtar kasasını ve Çoğaltmada kullanılan depolama hesapları için erişim anahtarlarını kullanır.

Azure geçişi projesindeki ilk VM 'yi çoğaltmadan önce, çoğaltma altyapısını sağlamak için aşağıdaki komutu çalıştırın. Bu komut, VMware VM 'lerinizi geçirmeye başlayabilmeniz için, belirtilen kaynakları hazırlar ve yapılandırır.

> [!NOTE]
> Bir Azure geçişi projesi yalnızca bir Azure bölgesine geçişleri destekler. Bu betiği çalıştırdıktan sonra, VMware VM 'lerinizi geçirmek istediğiniz hedef bölgeyi değiştiremezsiniz.
> `Initialize-AzMigrateReplicationInfrastructure`Azure geçişi projenizde yeni bir gereç yapılandırırsanız, komutunu çalıştırmanız gerekir.

Bu makalede, VM 'lerimizi bölgeye geçirebilmemiz için çoğaltma altyapısını başlatacağız `Central US` .

```azurepowershell-interactive
# Initialize replication infrastructure for the current Migrate project
Initialize-AzMigrateReplicationInfrastructure -ResourceGroupName $ResourceGroup.ResourceGroupName -ProjectName $MigrateProject. Name -Scenario agentlessVMware -TargetRegion "CentralUS" 

```

## <a name="7-replicate-vms"></a>7. VM 'Leri çoğaltma

Bulma işlemini tamamladıktan ve çoğaltma altyapısını başlattıktan sonra, VMware VM 'lerinin Azure 'a çoğaltılmasını başlatabilirsiniz. Aynı anda 500 çoğaltma gerçekleştirebilirsiniz.

Çoğaltma özelliklerini aşağıdaki gibi belirtebilirsiniz.

- **Hedef abonelik ve kaynak grubu** -() parametresini kullanarak kaynak grubu KIMLIĞI sağlayarak VM 'nin geçirilmesi gereken aboneliği ve kaynak grubunu belirtin `TargetResourceGroupId` .
- **Hedef sanal ağ ve alt ağ** - `TargetNetworkId` sırasıyla () ve () parametreleri kullanılarak VM 'Nin geçirilmesi gereken Azure sanal ağının kimliğini ve alt ağın adını belirtin `TargetSubnetName` .
- **Hedef VM adı** -() parametresini kullanarak oluşturulacak Azure VM 'nin adını belirtin `TargetVMName` .
- **Hedef VM boyutu** -() parametresi KULLANıLARAK çoğaltma VM 'si Için kullanılacak Azure VM boyutunu belirtin `TargetVMSize` . Örneğin, bir VM 'yi Azure 'da D2_v2 VM 'ye geçirmek için, ( `TargetVMSize` ) değerini "Standard_D2_v2" olarak belirtin.
- **Lisans** -etkin yazılım güvencesi veya Windows Server abonelikleri kapsamındaki Windows Server makineleriniz için Azure hibrit avantajı kullanmak için, ( `LicenseType` ) parametresini **windowsserver** olarak belirtin. Aksi takdirde, ( `LicenseType` ) parametresinin değerini "NoLicenseType" olarak belirtin.
- İşletim sistemi **diski** -işletim sistemi önyükleme yükleyicisine ve yükleyiciye sahip diskin benzersiz tanımlayıcısını belirtin. Kullanılacak disk KIMLIĞI, [Get-AzMigrateDiscoveredServer](/powershell/module/az.migrate/get-azmigratediscoveredserver) cmdlet 'i kullanılarak alınan diskin benzersiz tanımlayıcı (UUID) özelliğidir.
- **Disk türü** -( `DiskType` ) parametresi için değeri aşağıdaki şekilde belirtin.
    - Premium yönetilen diskleri kullanmak için, () parametresi olarak "Premium_LRS" değerini belirtin `DiskType` .
    - Standart SSD disklerini kullanmak için, () parametresi olarak "StandardSSD_LRS" değerini belirtin `DiskType` .
    - Standart HDD disklerini kullanmak için, () parametresi olarak "Standard_LRS" değerini belirtin `DiskType` .
- **Altyapı artıklığı** -altyapı artıklığı seçeneğini aşağıdaki şekilde belirtin.
    - Bölge içindeki belirli bir kullanılabilirlik bölgesine geçirilen makineyi sabitlemek için kullanılabilirlik alanı. Kullanılabilirlik Alanları arasında çok düğümlü bir uygulama katmanı oluşturan sunucuları dağıtmak için bu seçeneği kullanın. Bu seçenek yalnızca geçiş için seçilen hedef bölge Kullanılabilirlik Alanları destekliyorsa kullanılabilir. Kullanılabilirlik alanlarını kullanmak için () parametresi için kullanılabilirlik alanı değerini belirtin `TargetAvailabilityZone` .
    - Geçirilen makinenin bir kullanılabilirlik kümesine yerleştirileceği kullanılabilirlik kümesi. Seçilen hedef kaynak grubu, bu seçeneği kullanmak için bir veya daha fazla kullanılabilirlik kümesine sahip olmalıdır. Kullanılabilirlik kümesini kullanmak için, () parametresinin kullanılabilirlik kümesi KIMLIĞINI belirtin `TargetAvailabilitySet` .
 - **Önyükleme tanılama depolama hesabı** -bir önyükleme tanılama depolama hesabı kullanmak için, () parametresinin kimliğini belirtin `TargetBootDiagnosticStorageAccount` .
    -  Önyükleme tanılaması için kullanılan depolama hesabı, VM 'lerinizi geçirdiğiniz abonelikte olmalıdır.  
    - Varsayılan olarak, bu parametre için değer ayarlanmadı. 

### <a name="replicate-vms-with-all-disks"></a>VM 'Leri tüm disklerle çoğaltma

Bu öğreticide, bulunan VM 'nin tüm disklerini çoğaltacak ve Azure 'da VM için yeni bir ad belirteceğiz. Bulunan sunucunun ilk diskini işletim sistemi diski olarak belirtir ve tüm diskleri Standart HDD olarak geçirin. İşletim Sistemi diski, işletim sistemi önyükleyiciye ve yükleyiciye sahip disktir. Cmdlet 'i, işlemin durumunu izlemek için izlenebilecek bir iş döndürür.

```azurepowershell-interactive
# Retrieve the resource group that you want to migrate to
$TargetResourceGroup = Get-AzResourceGroup -Name MyTargetResourceGroup

# Retrieve the Azure virtual network and subnet that you want to migrate to
$TargetVirtualNetwork = Get-AzVirtualNetwork -Name MyVirtualNetwork

# Start replication for a discovered VM in an Azure Migrate project
$MigrateJob =  New-AzMigrateServerReplication -InputObject $DiscoveredServer -TargetResourceGroupId $TargetResourceGroup.ResourceId -TargetNetworkId $TargetVirtualNetwork.Id -LicenseType NoLicenseType -OSDiskID $DiscoveredServer.Disk[0].Uuid -TargetSubnetName $TargetVirtualNetwork.Subnets[0].Name -DiskType Standard_LRS -TargetVMName MyMigratedTestVM -TargetVMSize Standard_DS2_v2

# Track job status to check for completion
while (($MigrateJob.State -eq 'InProgress') -or ($MigrateJob.State -eq 'NotStarted')){
        #If the job hasn't completed, sleep for 10 seconds before checking the job status again
        sleep 10;
        $MigrateJob = Get-AzMigrateJob -InputObject $MigrateJob
}
#Check if the Job completed successfully. The updated job state of a successfully completed job should be "Succeeded".
Write-Output $MigrateJob.State
```

### <a name="replicate-vms-with-select-disks"></a>VM 'Leri Select disklerle çoğaltma

Ayrıca, [New-AzMigrateDiskMapping](/powershell/module/az.migrate/new-azmigratediskmapping) cmdlet 'ini kullanarak keşfedilen VM disklerini seçmeli olarak çoğaltabilir ve bunu `DiskToInclude` [New-azmigrateserverreplication](/powershell/module/az.migrate/new-azmigrateserverreplication) cmdlet 'inde () parametresine bir girdi olarak sağlayabilirsiniz. Ayrıca, çoğaltılacak her bir disk için farklı hedef disk türlerini belirtmek üzere [New-AzMigrateDiskMapping](/powershell/module/az.migrate/new-azmigratediskmapping) cmdlet 'ini de kullanabilirsiniz.

[New-AzMigrateDiskMapping](/powershell/module/az.migrate/new-azmigratediskmapping) cmdlet 'inin aşağıdaki parametreleri için değerleri belirtin.

- **DiskID** -geçirilecek disk için benzersiz tanımlayıcıyı belirtin. Kullanılacak disk KIMLIĞI, [Get-AzMigrateDiscoveredServer](/powershell/module/az.migrate/get-azmigratediscoveredserver) cmdlet 'i kullanılarak alınan diskin benzersiz tanımlayıcı (UUID) özelliğidir.
- **İsosdisk** -geçirilecek disk, sanal makinenin işletim sistemi diski ise "false" olarak ayarlanırsa "true" değerini belirtin.
- **Disktype** -Azure 'da kullanılacak disk türünü belirtin.

Aşağıdaki örnekte, bulunan VM 'nin yalnızca iki diskini çoğaltacağız. İşletim sistemi diskini belirteceğiz ve çoğaltılacak her disk için farklı disk türleri kullanacağız. Cmdlet, işlemin durumunu izlemek için izlenebilecek bir iş döndürür.

```azurepowershell-interactive
# View disk details of the discovered server
Write-Output $DiscoveredServer.Disk

# Create a new disk mapping for the disks to be replicated
$DisksToReplicate = @()
$OSDisk = New-AzMigrateDiskMapping -DiskID $DiscoveredServer.Disk[0].Uuid -DiskType StandardSSD_LRS -IsOSDisk true
$DataDisk = New-AzMigrateDiskMapping -DiskID $DiscoveredServer.Disk[1].Uuid -DiskType Premium_LRS -IsOSDisk false

$DisksToReplicate += $OSDisk
$DisksToReplicate += $DataDisk

# Retrieve the resource group that you want to migrate to
$TargetResourceGroup = Get-AzResourceGroup -Name MyTargetResourceGroup

# Retrieve the Azure virtual network and subnet that you want to migrate to
$TargetVirtualNetwork = Get-AzVirtualNetwork -Name MyVirtualNetwork

# Start replication for the VM
$MigrateJob =  New-AzMigrateServerReplication -InputObject $DiscoveredServer -TargetResourceGroupId $TargetResourceGroup.ResourceId -TargetNetworkId $TargetVirtualNetwork.Id -LicenseType NoLicenseType -DiskToInclude $DisksToReplicate -TargetSubnetName $TargetVirtualNetwork.Subnets[0].Name -TargetVMName MyMigratedTestVM -TargetVMSize Standard_DS2_v2

# Track job status to check for completion
while (($MigrateJob.State -eq 'InProgress') -or ($MigrateJob.State -eq 'NotStarted')){
        #If the job hasn't completed, sleep for 10 seconds before checking the job status again
        sleep 10;
        $MigrateJob = Get-AzMigrateJob -InputObject $MigrateJob
}
# Check if the Job completed successfully. The updated job state of a successfully completed job should be "Succeeded".
Write-Output $MigrateJob.State
```

## <a name="8-monitor-replication"></a>8. çoğaltmayı izleme

Çoğaltma aşağıdaki gibi gerçekleşir:

- Çoğaltma Başlat işi başarıyla tamamlandığında, makineler ilk çoğaltmasını Azure 'a başlatır.
- İlk çoğaltma sırasında VM anlık görüntüsü oluşturulur. Anlık görüntüdeki disk verileri, Azure 'da çoğaltma ile yönetilen disklere çoğaltılır.
- İlk çoğaltma tamamlandıktan sonra Delta çoğaltma başlar. Şirket içi disklerde artımlı değişiklikler düzenli aralıklarla Azure 'daki çoğaltma disklerine çoğaltılır.

[Get-AzMigrateServerReplication](/powershell/module/az.migrate/get-azmigrateserverreplication) cmdlet 'ini kullanarak çoğaltmanın durumunu izleyin.



```azurepowershell-interactive
# List replicating VMs and filter the result for selecting a replicating VM. This cmdlet will not return all properties of the replicating VM.
$ReplicatingServer = Get-AzMigrateServerReplication -ProjectName $MigrateProject.Name -ResourceGroupName $ResourceGroup.ResourceGroupName -MachineName MyTestVM

# Retrieve all properties of a replicating VM 
$ReplicatingServer = Get-AzMigrateServerReplication -TargetObjectID $ReplicatingServer.Id
```

Çıktıda **geçiş durumunu** ve **geçiş durumu açıklama** özelliklerini izleyebilirsiniz.

- İlk çoğaltma için **geçiş durumu** ve **geçiş durumu açıklama** özellikleri değerleri `InitialSeedingInProgress` ve `Initial replication` sırasıyla olur.
- Delta çoğaltma sırasında, geçiş **durumu** ve **geçiş durumu açıklama** özellikleri değerleri `Replicating` ve `Ready to migrate` sırasıyla olur.
- Geçişi tamamladıktan sonra, geçiş **durumu** ve **geçiş durumu açıklama** özellikleri değerleri `Migration succeeded` ve `Migrated` sırasıyla olur.

```Output
AllowedOperation            : {DisableMigration, TestMigrate, Migrate}
CurrentJobId                : /Subscriptions/xxx/resourceGroups/xxx/providers/Micr
                              osoft.RecoveryServices/vaults/xxx/replicationJobs/None
CurrentJobName              : None
CurrentJobStartTime         : 1/1/1753 1:01:01 AM
EventCorrelationId          : 9d435c55-4660-41a5-a8ed-dd74213d85fa
Health                      : Normal
HealthError                 : {}
Id                          : /Subscriptions/xxx/resourceGroups/xxx/providers/Micr
                              osoft.RecoveryServices/vaults/xxx/replicationFabrics/xxx/replicationProtectionContainers/xxx/
                              replicationMigrationItems/10-150-8-52-b090bef3-b733-5e34-bc8f-eb6f2701432a_5009e941-3e40-
                              39b2-1e14-f90584522703
LastTestMigrationStatus     :
LastTestMigrationTime       :
Location                    :
MachineName                 : MyTestVM
MigrationState              : InitialSeedingInProgress
MigrationStateDescription   : Initial replication
Name                        : 10-150-8-52-b090bef3-b733-5e34-bc8f-eb6f2701432a_5009e941-3e40-39b2-1e14-f90584522703
PolicyFriendlyName          : xxx
PolicyId                    : /Subscriptions/xxx/resourceGroups/xxx/providers/Micr
                              osoft.RecoveryServices/vaults/xxx/replicationPolicies/xxx
ProviderSpecificDetail      : Microsoft.Azure.PowerShell.Cmdlets.Migrate.Models.Api20180110.VMwareCbtMigrationDetails
TestMigrateState            : None
TestMigrateStateDescription : None
Type                        : Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationMigrationItems
```

Çoğaltma ilerleme durumu hakkında daha fazla bilgi için aşağıdaki cmdlet 'i çalıştırın.

```azurepowershell-interactive
Write-Output $replicatingserver.ProviderSpecificDetail
```

İlk çoğaltma ilerlemesini, çıkışdaki **Ilk dengeli dağıtım Ilerleme yüzdesi** özelliklerini kullanarak izleyebilirsiniz.

```Output
    "DataMoverRunAsAccountId": "/subscriptions/xxx/resourceGroups/xxx/providers/Microsoft.OffAzure/VMwareSites/xxx/runasaccounts/xxx",
    "FirmwareType":  "BIOS",
    "InitialSeedingProgressPercentage": 20,
    "InstanceType":  "VMwareCbt",
    "LastRecoveryPointReceived":  "\/Date(1601733591427)\/",
    "LicenseType":  "NoLicenseType",
    "MigrationProgressPercentage":  null,
    "MigrationRecoveryPointId":  null,
    "OSType":  "Windows",
    "PerformAutoResync":  "true",
```

Çoğaltma aşağıdaki gibi gerçekleşir:

- Çoğaltma Başlat işi başarıyla tamamlandığında, makineler ilk çoğaltmasını Azure 'a başlatır.
- İlk çoğaltma sırasında VM anlık görüntüsü oluşturulur. Anlık görüntüdeki disk verileri, Azure 'da çoğaltma ile yönetilen disklere çoğaltılır.
- İlk çoğaltma tamamlandıktan sonra Delta çoğaltma başlar. Şirket içi disklerde artımlı değişiklikler düzenli aralıklarla Azure 'daki çoğaltma disklerine çoğaltılır.

## <a name="9-retrieve-the-status-of-a-job"></a>9. bir işin durumunu alın

[Get-AzMigrateJob](/powershell/module/az.migrate/get-azmigratejob) cmdlet 'ini kullanarak bir işin durumunu izleyebilirsiniz.

```azurepowershell-interactive
# Retrieve the updated status for a job
$job = Get-AzMigrateJob -InputObject $job
```

## <a name="10-update-properties-of-a-replicating-vm"></a>10. çoğaltılan bir VM 'nin özelliklerini güncelleştirme

[Azure geçişi: sunucu geçişi](migrate-services-overview.md#azure-migrate-server-migration-tool) , çoğaltılan bir sanal makine için ad, boyut, kaynak grubu, NIC yapılandırması gibi hedef özellikleri değiştirmenize olanak sağlar.

Aşağıdaki özellikler bir VM için güncelleştirilebilen olabilir.

- **VM adı** -[] parametresini kullanarak oluşturulacak Azure VM 'nin adını belirtin `TargetVMName` .
- **VM boyutu** -[] parametresini kullanarak çoğaltma VM 'si Için kullanılacak Azure VM boyutunu belirtin `TargetVMSize` . Örneğin, bir VM 'yi Azure 'da D2_v2 VM 'ye geçirmek için, [] için değeri `TargetVMSize` olarak belirtin `Standard_D2_v2` .
- **Sanal ağ** -[] PARAMETRESINI kullanarak VM 'nin geçirilmesi gereken Azure sanal ağının kimliğini belirtin `TargetNetworkId` .
- **Kaynak grubu** -[] parametresini kullanarak kaynak grubu KIMLIĞI sağlayarak VM 'nin geçirilmesi gereken kaynak grubunun kimliğini belirtin `TargetResourceGroupId` .
- **Ağ arabirimi** -NIC yapılandırması [New-AzMigrateNicMapping](/powershell/module/az.migrate/new-azmigratenicmapping) cmdlet 'i kullanılarak belirtilebilir. Daha sonra nesne, `NicToUpdate` [set-AzMigrateServerReplication](/powershell/module/az.migrate/set-azmigrateserverreplication) cmdlet 'inde [] parametresine bir giriş geçirdiğini.

    - **IP ayırmayı Değiştir** -NIC IÇIN statik IP belirtmek için, [] PARAMETRESINI kullanarak VM IÇIN statik IP olarak kullanılacak IPv4 adresini sağlayın `TargetNicIP` . Bir NIC için dinamik olarak bir IP atamak için `auto` **targetnicıp** parametresinin değeri olarak belirtin.
    - NIC 'in `Primary` `Secondary` `DoNotCreate` `TargetNicSelectionType` birincil, ikincil veya geçirilen VM 'de oluşturulup oluşturulmayacağını belirtmek için değerler veya [] parametresini kullanın. VM için birincil NIC olarak yalnızca bir NIC belirtilebilir.
    - Bir NIC birincil oluşturmak için, ikincil hale getirilmeli veya geçirilen VM 'de oluşturulmayan diğer NIC 'Leri de belirtmeniz gerekir.
    - NIC 'nin alt ağını değiştirmek için [] parametresini kullanarak alt ağın adını belirtin `TargetNicSubnet` .

 - **Kullanılabilirlik bölgesi** -kullanılabilirlik alanlarını kullanmak için [] parametresi için kullanılabilirlik alanı değerini belirtin `TargetAvailabilityZone` .
 - **Kullanılabilirlik kümesi** kullanım için kullanılabilirlik kümesi, [] parametresi için KULLANıLABILIRLIK kümesi kimliğini belirtin `TargetAvailabilitySet` .

[Get-AzMigrateServerReplication](/powershell/module/az.migrate/get-azmigrateserverreplication) cmdlet 'i, işlemin durumunu izlemek için izlenebilecek bir iş döndürür.

```azurepowershell-interactive
# List replicating VMs and filter the result for selecting a replicating VM. This cmdlet will not return all properties of the replicating VM.
$ReplicatingServer = Get-AzMigrateServerReplication -ProjectName $MigrateProject.Name -ResourceGroupName $ResourceGroup.ResourceGroupName -MachineName MyTestVM

# Retrieve all properties of a replicating VM 
$ReplicatingServer = Get-AzMigrateServerReplication -TargetObjectID $ReplicatingServer.Id

# View NIC details of the replicating server
Write-Output $ReplicatingServer.ProviderSpecificDetail.VMNic
```

Aşağıdaki örnekte, ilk NIC 'yi birincil olarak yaparak ve kendisine statik bir IP atayarak NIC yapılandırmasını güncelleştireceğiz. geçiş için ikinci NIC 'yi atacak ve hedef VM adını ve boyutunu güncelleştiririz.

```azurepowershell-interactive
# Specify the NIC properties to be updated for a replicating VM.
$NicMapping = @()
$NicMapping1 = New-AzMigrateNicMapping -NicId $ReplicatingServer.ProviderSpecificDetail.VMNic[0].NicId -TargetNicIP ###.###.###.### -TargetNicSelectionType Primary
$NicMapping2 = New-AzMigrateNicMapping -NicId $ReplicatingServer.ProviderSpecificDetail.VMNic[1].NicId -TargetNicSelectionType DoNotCreate

$NicMapping += $NicMapping1
$NicMapping += $NicMapping2

# Update the name, size and NIC configuration of a replicating server
$UpdateJob = Set-AzMigrateServerReplication -InputObject $ReplicatingServer -TargetVMSize Standard_DS13_v2 -TargetVMName MyMigratedVM -NicToUpdate $NicMapping

# Track job status to check for completion
while (($UpdateJob.State -eq 'InProgress') -or ($UpdateJob.State -eq 'NotStarted')){
        #If the job hasn't completed, sleep for 10 seconds before checking the job status again
        sleep 10;
        $UpdateJob = Get-AzMigrateJob -InputObject $UpdateJob
}
# Check if the Job completed successfully. The updated job state of a successfully completed job should be "Succeeded".
Write-Output $UpdateJob.State
```



## <a name="11-run-a-test-migration"></a>11. bir test geçişi çalıştırın

Delta çoğaltma başladığında, Azure 'a tam geçiş çalıştırmadan önce VM 'Ler için bir test geçişi çalıştırabilirsiniz. Geçirmeden önce her makine için geçişi en az bir kez test etmenizi kesinlikle öneririz. Cmdlet, işlemin durumunu izlemek için izlenebilecek bir iş döndürür.

- Test geçişi çalıştırmak, geçişin beklendiği gibi çalıştığını denetler. Test geçişi, çalışır durumda olan ve çoğaltmaya devam eden şirket içi makineyi etkilemez.
- Test geçişi, çoğaltılan verileri kullanarak bir Azure VM oluşturarak geçişe benzetir (genellikle Azure aboneliğinizdeki bir üretim dışı VNet 'e geçiş yapar).
- Geçişi doğrulamak, uygulama testi gerçekleştirmek ve tam geçişten önce herhangi bir sorunu gidermek için çoğaltılan test Azure VM 'yi kullanabilirsiniz.

[] Parametresini kullanarak sanal ağın KIMLIĞINI belirterek test için kullanılacak Azure sanal ağını seçin `TestNetworkID` .

```azurepowershell-interactive
# Retrieve the Azure virtual network created for testing
$TestVirtualNetwork = Get-AzVirtualNetwork -Name MyTestVirtualNetwork

# Start test migration for a replicating server
$TestMigrationJob = Start-AzMigrateTestMigration -InputObject $ReplicatingServer -TestNetworkID $TestVirtualNetwork.Id

# Track job status to check for completion
while (($TestMigrationJob.State -eq 'InProgress') -or ($TestMigrationJob.State -eq 'NotStarted')){
        #If the job hasn't completed, sleep for 10 seconds before checking the job status again
        sleep 10;
        $TestMigrationJob = Get-AzMigrateJob -InputObject $TestMigrationJob
}
# Check if the Job completed successfully. The updated job state of a successfully completed job should be "Succeeded".
Write-Output $TestMigrationJob.State
```

Sınama tamamlandıktan sonra, [Start-AzMigrateTestMigrationCleanup](/powershell/module/az.migrate/start-azmigratetestmigrationcleanup) cmdlet 'ini kullanarak test geçişini temizleyin. Cmdlet 'i, işlemin durumunu izlemek için izlenebilecek bir iş döndürür.

```azurepowershell-interactive
# Clean-up test migration for a replicating server
$CleanupTestMigrationJob = Start-AzMigrateTestMigrationCleanup -InputObject $ReplicatingServer

# Track job status to check for completion
while (($CleanupTestMigrationJob.State -eq "InProgress") -or ($CleanupTestMigrationJob.State -eq "NotStarted")){
        #If the job hasn't completed, sleep for 10 seconds before checking the job status again
        sleep 10;
        $CleanupTestMigrationJob = Get-AzMigrateJob -InputObject $CleanupTestMigrationJob
}
# Check if the Job completed successfully. The updated job state of a successfully completed job should be "Succeeded".
Write-Output $CleanupTestMigrationJob.State
```

## <a name="12-migrate-vms"></a>12. VM 'Leri geçirme

Test geçişinin beklendiği gibi çalışıp çalışmadığını doğruladıktan sonra, aşağıdaki cmdlet 'i kullanarak çoğaltma sunucusunu geçirebilirsiniz. Cmdlet 'i, işlemin durumunu izlemek için izlenebilecek bir iş döndürür.

Kaynak sunucuyu kapatmak istemiyorsanız [ `TurnOffSourceServer` ] parametresini kullanmayın.

```azurepowershell-interactive
# Start migration for a replicating server and turn off source server as part of migration
$MigrateJob = Start-AzMigrateServerMigration -InputObject $ReplicatingServer -TurnOffSourceServer

# Track job status to check for completion
while (($MigrateJob.State -eq 'InProgress') -or ($MigrateJob.State -eq 'NotStarted')){
        #If the job hasn't completed, sleep for 10 seconds before checking the job status again
        sleep 10;
        $MigrateJob = Get-AzMigrateJob -InputObject $MigrateJob
}
#Check if the Job completed successfully. The updated job state of a successfully completed job should be "Succeeded".
Write-Output $MigrateJob.State
```

## <a name="13-complete-the-migration"></a>13. geçişi tamamladıktan sonra

1. Geçiş yapıldıktan sonra, aşağıdaki cmdlet 'i kullanarak, şirket içi makine için çoğaltmayı durdurun ve VM için çoğaltma durumu bilgilerini temizleyin. Cmdlet 'i, işlemin durumunu izlemek için izlenebilecek bir iş döndürür.

   ```azurepowershell-interactive
   # Stop replication for a migrated server
   $StopReplicationJob = Remove-AzMigrateServerReplication -InputObject $ReplicatingServer

   # Track job status to check for completion
   while (($StopReplicationJob.State -eq 'InProgress') -or ($StopReplicationJob.State -eq 'NotStarted')){
           #If the job hasn't completed, sleep for 10 seconds before checking the job status again
           sleep 10;
           $StopReplicationJob = Get-AzMigrateJob -InputObject $StopReplicationJob
   }
   # Check if the Job completed successfully. The updated job state of a successfully completed job should be "Succeeded".
   Write-Output $StopReplicationJob.State
   ```

1. Makinede Linux işletim sistemi varsa, geçirilen makinelere [Linux](../virtual-machines/extensions/agent-linux.md) aracısını yükler. Geçiş sırasında Windows VM 'Leri için VM aracısını otomatik olarak yükledik.
1. Veritabanı bağlantısı dizelerini ve web sunucusu yapılandırmalarını güncelleştirme gibi herhangi bir geçiş sonrası uygulama ayarı gerçekleştirin.
1. Geçirilen uygulamada son uygulama ve geçiş kabul testi gerçekleştirme işlemi şimdi Azure’da çalıştırılmaktadır.
1. Geçirilen Azure VM örneğine giden trafiği kesin.
1. Yerel sanal makine envanterinizden şirket içi sanal makineleri kaldırın.
1. Yerel yedeklemelerden şirket içi sanal makineleri kaldırın.
1. Azure sanal makinelerinin yeni konumunu ve IP adresini göstermek için herhangi bir iç belgeyi güncelleştirin.

## <a name="14-post-migration-best-practices"></a>14. geçiş sonrası en iyi uygulamalar

- Daha fazla esneklik için:
    - Azure Backup hizmetini kullanarak Azure sanal makinelerini yedekleyip verileri güvende tutun. [Daha fazla bilgi edinin](../backup/quick-backup-vm-portal.md).
    - Site Recovery ile Azure sanal makinelerini ikincil bölgeye çoğaltarak iş yüklerinin çalışmaya devam etmesini ve sürekli kullanılabilir olmasını sağlayın. [Daha fazla bilgi edinin](../site-recovery/azure-to-azure-tutorial-enable-replication.md).
- Daha fazla güvenlik için:
    - Azure Güvenlik Merkezi ile gelen trafik erişimini kilitleme ve sınırlayın [-tam zamanında yönetim](../security-center/security-center-just-in-time.md).
    - [Ağ Güvenlik Grupları](../virtual-network/network-security-groups-overview.md) ile ağ trafiğini yönetim uç noktaları ile kısıtlayın.
    - [Azure Disk Şifrelemesi](../security/fundamentals/azure-disk-encryption-vms-vmss.md)’ni dağıtarak disklerin güvenliğinin sağlanmasına yardımcı olun ve verileri hırsızlık ve yetkisiz erişime karşı koruyun.
    - [IaaS kaynaklarının güvenliğini sağlama](https://azure.microsoft.com/services/virtual-machines/secure-well-managed-iaas/) hakkında daha fazla bilgi edinin ve [Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center/)’ni ziyaret edin.
- İzleme ve yönetim için:
-  [Azure Maliyet Yönetimi](../cost-management-billing/cloudyn/overview.md)’ni dağıtarak kaynak kullanımını ve harcamayı izleyin.
