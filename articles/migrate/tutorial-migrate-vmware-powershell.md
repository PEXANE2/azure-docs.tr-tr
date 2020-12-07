---
title: VMware VM 'lerini Azure 'a geçirme (aracısız)-PowerShell
description: PowerShell aracılığıyla Azure geçişi ile, VMware VM 'lerinin aracısız geçişini nasıl çalıştıracağınızı öğrenin.
author: rahulg1190
ms.author: rahugup
manager: bsiva
ms.topic: tutorial
ms.date: 10/1/2020
ms.openlocfilehash: 187e17bcf385ba6b3c17566fc00e5ee77cf009fe
ms.sourcegitcommit: ea551dad8d870ddcc0fee4423026f51bf4532e19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/07/2020
ms.locfileid: "96752490"
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

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiye başlamadan önce karşılamanız gereken ön koşullar şunlardır:

1. Azure ve VMware 'yi geçiş için hazırlamak üzere [bulma öğreticisini doldurun](tutorial-discover-vmware.md) .
2. [VMware VM](./tutorial-assess-vmware-azure-vm.md) 'lerini Azure 'a geçirmeden önce değerlendirmek için ikinci öğreticiyi tamamlamanızı öneririz.
3. Azure PowerShell `Az` modüle sahipsiniz. Azure PowerShell yüklemeniz veya yükseltmeniz gerekiyorsa [Azure PowerShell yüklemek ve yapılandırmak için bu kılavuzu](/powershell/azure/install-az-ps) izleyin

## <a name="install-azure-migrate-powershell-module"></a>Azure geçişi PowerShell modülünü yükler

Azure geçişi PowerShell modülü önizlemede kullanılabilir. Aşağıdaki komutu kullanarak PowerShell modülünü yüklemeniz gerekir. 

```azurepowershell
Install-Module -Name Az.Migrate 
```

## <a name="sign-in-to-your-microsoft-azure-subscription"></a>Microsoft Azure aboneliğinizde oturum açın

Cmdlet 'i ile Azure aboneliğinizde oturum açın `Connect-AzAccount` .

```azurepowershell
Connect-AzAccount
```

Azure aboneliğinizi seçin. `Get-AzSubscription`Erişiminiz olan Azure aboneliklerinin listesini almak için cmdlet 'ini kullanın. Cmdlet 'ini kullanarak çalışmak için Azure geçişi projenizin bulunduğu Azure aboneliğini seçin `Set-AzContext` .

```azurepowershell
Set-AzContext -SubscriptionId "xxxx-xxxx-xxxx-xxxx"
```

## <a name="retrieve-the-azure-migrate-project"></a>Azure geçişi projesini alma

Değerlendirme veya geçiş yaptığınız ortamdan toplanan bulma, değerlendirme ve geçiş meta verilerini depolamak için bir Azure geçişi projesi kullanılır.
Bir projede keşfedilen varlıkları izleyebilir, değerlendirmeleri düzenleyebilir ve geçişleri gerçekleştirebilirsiniz.

Önkoşulların bir parçası olarak, zaten bir Azure geçişi projesi oluşturmuş olursunuz. `Get-AzMigrateProject`Bir Azure geçişi projesinin ayrıntılarını almak için cmdlet 'ini kullanın. Azure geçişi projesinin ( `Name` ) adını ve Azure geçişi projesinin () kaynak grubunun adını belirtmeniz gerekir `ResourceGroupName` .

```azurepowershell
# Get resource group of the Azure Migrate project
$ResourceGroup = Get-AzResourceGroup -Name "MyResourceGroup"

# Get details of the Azure Migrate project
$MigrateProject = Get-AzMigrateProject -Name "MyMigrateProject" -ResourceGroupName $ResourceGroup.ResourceGroupName

# View Azure Migrate project details
$MigrateProject | ConvertTo-JSON
```

## <a name="retrieve-discovered-vms-in-an-azure-migrate-project"></a>Azure geçişi projesinde bulunan VM 'Leri alma

Azure geçişi basit bir [Azure geçiş](migrate-appliance-architecture.md)gereci kullanır. Önkoşulların bir parçası olarak Azure geçişi gereci bir VMware VM 'si olarak dağıttığınızı görürsünüz.

Azure geçişi projesindeki belirli bir VMware VM 'sini almak için Azure geçişi projesi ( `ProjectName` ), Azure geçişi projesinin kaynak grubu ( `ResourceGroupName` ) ve VM adı () adını belirtin `DisplayName` . 

> [!NOTE]
> **VM Name ( `DisplayName` ) parametre değeri büyük/küçük harfe duyarlıdır**.

```azurepowershell
# Get a specific VMware VM in an Azure Migrate project
$DiscoveredServer = Get-AzMigrateDiscoveredServer -ProjectName $MigrateProject.Name -ResourceGroupName $ResourceGroup.ResourceGroupName -DisplayName "MyTestVM"

# View discovered server details
$DiscoveredServer | ConvertTo-JSON
```
Bu öğreticinin bir parçası olarak bu VM 'yi geçireceğiz.

Ayrıca, ve parametrelerini kullanarak bir Azure geçişi projesindeki tüm VMware VM 'lerini de alabilirsiniz `ProjectName` `ResourceGroupName` .

```azurepowershell
# Get all VMware VMs in an Azure Migrate project
$DiscoveredServers = Get-AzMigrateDiscoveredServer -ProjectName $MigrateProject.Name -ResourceGroupName $ResourceGroup.ResourceGroupName 
```
Azure geçişi projesinde birden çok gereç varsa, `ProjectName` `ResourceGroupName` `ApplianceName` belirli bir Azure geçişi gereci kullanarak bulunan tüm VM 'leri almak için, ve parametrelerini kullanabilirsiniz. 

```azurepowershell
# Get all VMware VMs discovered by an Azure Migrate Appliance in an Azure Migrate project
$DiscoveredServers = Get-AzMigrateDiscoveredServer -ProjectName $MigrateProject.Name -ResourceGroupName $ResourceGroup.ResourceGroupName -ApplianceName "MyMigrateAppliance" |Format-Table DisplayName, Name, Type

```

## <a name="initialize-replication-infrastructure"></a>Çoğaltma altyapısını Başlat

[Azure geçişi: sunucu geçişi](migrate-services-overview.md#azure-migrate-server-migration-tool) , VM 'leri geçirmek için birden çok Azure kaynağı kullanır. Sunucu geçişi, proje ile aynı kaynak grubunda aşağıdaki kaynakları sağlar.

- **Service Bus**: sunucu geçişi, yüklemeye çoğaltma düzenleme iletileri göndermek için Service Bus 'ı kullanır.
- **Ağ geçidi depolama hesabı**: sunucu geçişi, çoğaltılan VM 'ler hakkında durum bilgilerini depolamak için ağ geçidi depolama hesabını kullanır.
- **Günlük depolama hesabı**: Azure geçişi gereci, VM 'ler için çoğaltma günlüklerini bir günlük depolama hesabına yükler. Azure geçişi, çoğaltma bilgilerini çoğaltma tarafından yönetilen disklere uygular.
- **Anahtar Kasası**: Azure geçişi gereci, hizmet veri yolu için bağlantı dizelerini yönetmek üzere anahtar kasasını ve Çoğaltmada kullanılan depolama hesapları için erişim anahtarlarını kullanır.

Azure geçişi projesindeki ilk VM 'yi çoğaltmadan önce, çoğaltma altyapısını sağlamak için aşağıdaki betiği çalıştırın. Bu betik, VMware VM 'lerinizi geçirmeye başlayabilmeniz için, belirtilen kaynakları sağlar ve yapılandırır.

> [!NOTE]
> Bir Azure geçişi projesi yalnızca bir Azure bölgesine geçişleri destekler. Bu betiği çalıştırdıktan sonra, VMware VM 'lerinizi geçirmek istediğiniz hedef bölgeyi değiştiremezsiniz.
> `Initialize-AzMigrateReplicationInfrastructure`Azure geçişi projenizde yeni bir gereç yapılandırırsanız, betiği çalıştırmanız gerekir. 

Bu makalede, VM 'lerimizi bölgeye geçirebilmemiz için çoğaltma altyapısını başlatacağız `Central US` . Dosyayı GitHub deposundan [indirebilir](https://github.com/Azure/azure-docs-powershell-samples/tree/master/azure-migrate/migrate-at-scale-vmware-agentles) veya aşağıdaki kod parçacığını kullanarak çalıştırabilirsiniz. 

```azurepowershell
# Download the script from Azure Migrate GitHub repository 
Invoke-WebRequest https://raw.githubusercontent.com/Azure/azure-docs-powershell-samples/master/azure-migrate/migrate-at-scale-vmware-agentles/Initialize-AzMigrateReplicationInfrastructure.ps1 -OutFile .\AzMigrateReplicationinfrastructure.ps1

# Run the script for initializing replication infrastructure for the current Migrate project
.\AzMigrateReplicationInfrastructure.ps1 -ResourceGroupName $ResourceGroup.ResourceGroupName -ProjectName $MigrateProject.Name -Scenario agentlessVMware -TargetRegion "CentralUS" 
```


## <a name="replicate-vms"></a>Sanal makineleri çoğaltma

Bulma işlemini tamamladıktan ve çoğaltma altyapısını başlattıktan sonra, VMware VM 'lerinin Azure 'a çoğaltılmasını başlatabilirsiniz. Aynı anda 300 çoğaltma gerçekleştirebilirsiniz.

Çoğaltma özelliklerini aşağıdaki gibi belirtebilirsiniz.

- **Hedef abonelik ve kaynak grubu** -parametresini kullanarak kaynak grubu KIMLIĞI sağlayarak VM 'nin geçirilmesi gereken aboneliği ve kaynak grubunu belirtin `TargetResourceGroupId` . 
- **Hedef sanal ağ ve alt ağ** -Azure sanal ağının kimliğini ve `TargetNetworkId` sırasıyla ve parametrelerini kullanarak VM 'nin geçirilmesi gereken alt ağın adını belirtin `TargetSubnetName` . 
- **Hedef VM adı** -parametresini kullanarak oluşturulacak Azure VM 'nin adını belirtin `TargetVMName` .
- **Hedef VM boyutu** -parametre kullanarak çoğaltma VM 'si Için kullanılacak Azure VM boyutunu belirtin `TargetVMSize` . Örneğin, bir VM 'yi Azure 'da D2_v2 VM 'ye geçirmek için değeri `TargetVMSize` "Standard_D2_v2" olarak belirtin.  
- **Lisans** -etkin yazılım güvencesi veya Windows Server abonelikleri kapsamındaki Windows Server makineleriniz için Azure hibrit avantajı kullanmak için, parametre için değeri `LicenseType` "windowsserver" olarak belirtin. Aksi takdirde, parametre için değeri `LicenseType` "NoLicenseType" olarak belirtin.
- İşletim sistemi **diski** -işletim sistemi önyükleme yükleyicisine ve yükleyiciye sahip diskin benzersiz tanımlayıcısını belirtin. Kullanılacak disk KIMLIĞI, cmdlet 'i kullanılarak alınan diskin benzersiz tanımlayıcı (UUID) özelliğidir `Get-AzMigrateServer` .
- **Disk türü** -parametre için değeri `DiskType` aşağıdaki şekilde belirtin.
    - Premium yönetilen diskleri kullanmak için, "Premium_LRS" parametresini parametre değeri olarak belirtin `DiskType` . 
    - Standart SSD disklerini kullanmak için, "StandardSSD_LRS" parametresini parametre değeri olarak belirtin `DiskType` . 
    - Standart HDD disklerini kullanmak için, "Standard_LRS" parametresini parametre değeri olarak belirtin `DiskType` . 
- **Altyapı artıklığı** -altyapı artıklığı seçeneğini aşağıdaki şekilde belirtin. 
    - Bölge içindeki belirli bir kullanılabilirlik bölgesine geçirilen makineyi sabitlemek için kullanılabilirlik alanı. Kullanılabilirlik Alanları arasında çok düğümlü bir uygulama katmanı oluşturan sunucuları dağıtmak için bu seçeneği kullanın. Bu seçenek yalnızca geçiş için seçilen hedef bölge Kullanılabilirlik Alanları destekliyorsa kullanılabilir. Kullanılabilirlik alanlarını kullanmak için, parametre için kullanılabilirlik alanı değerini belirtin `TargetAvailabilityZone` .
    - Geçirilen makinenin bir kullanılabilirlik kümesine yerleştirileceği kullanılabilirlik kümesi. Seçilen hedef kaynak grubu, bu seçeneği kullanmak için bir veya daha fazla kullanılabilirlik kümesine sahip olmalıdır. Kullanılabilirlik kümesini kullanmak için, parametre için kullanılabilirlik kümesi KIMLIĞINI belirtin `TargetAvailabilitySet` . 

### <a name="replicate-vms-with-all-disks"></a>VM 'Leri tüm disklerle çoğaltma
Bu öğreticide, bulunan VM 'nin tüm disklerini çoğaltacak ve Azure 'da VM için yeni bir ad belirteceğiz. Bulunan sunucunun ilk diskini işletim sistemi diski olarak belirtir ve tüm diskleri Standart HDD olarak geçirin. İşletim Sistemi diski, işletim sistemi önyükleyiciye ve yükleyiciye sahip disktir. Cmdlet, işlemin durumunu izlemek için izlenebilecek bir iş döndürür. 

```azurepowershell
# Retrieve the resource group that you want to migrate to
$TargetResourceGroup = Get-AzResourceGroup -Name "MyTargetResourceGroup"

# Retrieve the Azure virtual network and subnet that you want to migrate to
$TargetVirtualNetwork = Get-AzVirtualNetwork -Name "MyVirtualNetwork"

# Start replication for a discovered VM in an Azure Migrate project
$MigrateJob =  New-AzMigrateServerReplication -InputObject $DiscoveredServer -TargetResourceGroupId $TargetResourceGroup.ResourceId -TargetNetworkId $TargetVirtualNetwork.Id -LicenseType "NoLicenseType" -OSDiskID $DiscoveredServer.Disk[0].Uuid -TargetSubnetName $TargetVirtualNetwork.Subnets[0].Name -DiskType "Standard_LRS" -TargetVMName "MyMigratedTestVM" -TargetVMSize "Standard_DS2_v2"

# Track job status to check for completion
while (($MigrateJob.State -eq "InProgress") -or ($MigrateJob.State -eq "NotStarted")){
        #If the job hasn't completed, sleep for 10 seconds before checking the job status again
        sleep 10;
        $MigrateJob = Get-AzMigrateJob -InputObject $MigrateJob
}
#Check if the Job completed successfully. The updated job state of a successfully completed job should be "Succeeded"
Write-Output $MigrateJob.State
```

### <a name="replicate-vms-with-select-disks"></a>VM 'Leri Select disklerle çoğaltma
Ayrıca, cmdlet 'ini kullanarak keşfedilen VM disklerini seçmeli `New-AzMigrateDiskMapping` olarak çoğaltabilir ve `DiskToInclude` cmdlet 'teki parametreye bir girdi olarak bunu sağlayabilirsiniz `New-AzMigrateServerReplication` . Ayrıca, `New-AzMigrateDiskMapping` çoğaltılacak her bir disk için farklı hedef disk türlerini belirtmek için cmdlet 'ini de kullanabilirsiniz. 

Cmdlet 'in aşağıdaki parametreleri için değerleri belirtin `New-AzMigrateDiskMapping` .

- **DiskID** -geçirilecek disk için benzersiz tanımlayıcıyı belirtin. Kullanılacak disk KIMLIĞI, cmdlet 'i kullanılarak alınan diskin benzersiz tanımlayıcı (UUID) özelliğidir `Get-AzMigrateServer` .  
- **İsosdisk** -geçirilecek disk, sanal makinenin işletim sistemi diski ise "false" olarak ayarlanırsa "true" değerini belirtin.
- **Disktype** -Azure 'da kullanılacak disk türünü belirtin. 

Aşağıdaki örnekte, bulunan VM 'nin yalnızca iki diskini çoğaltacağız. işletim sistemi diskini belirteceğiz ve çoğaltılacak her disk için farklı disk türleri kullanacağız. Cmdlet, işlemin durumunu izlemek için izlenebilecek bir iş döndürür. 

```azurepowershell
# View disk details of the discovered server
$DiscoveredServer.Disk | ConvertTo-JSON

# Create a new disk mapping for the disks to be replicated
$DisksToReplicate = @()
$OSDisk = New-AzMigrateDiskMapping -DiskID $DiscoveredServer.Disk[0].Uuid -DiskType "StandardSSD_LRS" -IsOSDisk "true"
$DataDisk = New-AzMigrateDiskMapping -DiskID $DiscoveredServer.Disk[1].Uuid -DiskType "Premium_LRS" -IsOSDisk "false"

$DisksToReplicate += $OSDisk
$DisksToReplicate += $DataDisk 

# Retrieve the resource group that you want to migrate to
$TargetResourceGroup = Get-AzResourceGroup -Name "MyTargetResourceGroup"

# Retrieve the Azure virtual network and subnet that you want to migrate to
$TargetVirtualNetwork = Get-AzVirtualNetwork -Name "MyVirtualNetwork"

# Start replication for the VM
$MigrateJob =  New-AzMigrateServerReplication -InputObject $DiscoveredServer -TargetResourceGroupId $TargetResourceGroup.ResourceId -TargetNetworkId $TargetVirtualNetwork.Id -LicenseType "NoLicenseType" -DiskToInclude $DisksToReplicate -TargetSubnetName $TargetVirtualNetwork.Subnets[0].Name -TargetVMName "MyMigratedTestVM" -TargetVMSize "Standard_DS2_v2"

# Track job status to check for completion
while (($MigrateJob.State -eq "InProgress") -or ($MigrateJob.State -eq "NotStarted")){
        #If the job hasn't completed, sleep for 10 seconds before checking the job status again
        sleep 10;
        $MigrateJob = Get-AzMigrateJob -InputObject $MigrateJob
}
#Check if the Job completed successfully. The updated job state of a successfully completed job should be "Succeeded"
Write-Output $MigrateJob.State
```

## <a name="monitor-replication"></a>Çoğaltmayı izleme 

Çoğaltma aşağıdaki gibi gerçekleşir:

- Çoğaltma Başlat işi başarıyla tamamlandığında, makineler ilk çoğaltmasını Azure 'a başlatır.
- İlk çoğaltma sırasında VM anlık görüntüsü oluşturulur. Anlık görüntüdeki disk verileri, Azure 'da çoğaltma ile yönetilen disklere çoğaltılır.
- İlk çoğaltma tamamlandıktan sonra Delta çoğaltma başlar. Şirket içi disklerde artımlı değişiklikler düzenli aralıklarla Azure 'daki çoğaltma disklerine çoğaltılır.

Cmdlet 'ini kullanarak çoğaltmanın durumunu izleyin `Get-AzMigrateServerReplication` . 

> [!NOTE]
> Bulunan VM KIMLIĞI ve çoğaltılan VM KIMLIĞI, iki farklı benzersiz tanımlayıcılardır. Bu tanımlayıcıların her ikisi de, çoğaltılan bir sunucunun ayrıntılarını almak için kullanılabilir.  

### <a name="monitor-replication-using-discovered-vm-identifier"></a>Keşfedilen VM tanımlayıcısını kullanarak çoğaltmayı izleme
```azurepowershell
# Retrieve the replicating VM details by using the discovered VM identifier
$ReplicatingServer = Get-AzMigrateServerReplication -DiscoveredMachineId $DiscoveredServer.ID
```

### <a name="monitor-replication-using-replicating-vm-identifier"></a>Çoğaltma VM tanımlayıcısı kullanarak çoğaltmayı izleme

```azurepowershell
# List all replicating VMs in an Azure Migrate project and filter the result for selecting the replication VM. This cmdlet will not return all properties of the replicating VM.
$ReplicatingServer = Get-AzMigrateServerReplication -ProjectName $MigrateProject.Name -ResourceGroupName $ResourceGroup.ResourceGroupName | where MachineName -eq $DiscoveredServer.DisplayName

# Retrieve replicating VM details using replicating VM identifier
$ReplicatingServer = Get-AzMigrateServerReplication -TargetObjectID $ReplicatingServer.Id 
```

Çıktıda "geçiş durumu" ve "geçiş durumu açıklaması" özelliklerini izleyebilirsiniz. 
- İlk çoğaltma için geçiş durumu ve geçiş durumu açıklama özellikleri değerleri sırasıyla "ınitialseedingınprogress" ve "Ilk çoğaltma" olacaktır. 
- Delta çoğaltma sırasında, geçişi durum ve geçiş durumu açıklama özellikleri için değerler sırasıyla "çoğaltılıyor" ve "geçişe hazırlanıyor" olacaktır.
- Geçişi tamamladıktan sonra, geçiş durumu ve geçiş durumu açıklama özellikleri değerleri sırasıyla "geçiş başarılı" ve "geçirilir" olacaktır.

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

```azurepowershell
$replicatingserver.ProviderSpecificDetail | convertto-json
```
İlk çoğaltma ilerlemesini, çıktıda "Ilk dengeli dağıtım Ilerleme yüzdesi" özelliklerini kullanarak izleyebilirsiniz.

```output
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

## <a name="retrieve-the-status-of-a-job"></a>Bir işin durumunu alma

Cmdlet 'ini kullanarak bir işin durumunu izleyebilirsiniz `Get-AzMigrateJob` . 

```azurepowershell
# Retrieve the updated status for a job
$job = Get-AzMigrateJob -InputObject $job
```

## <a name="update-properties-of-a-replicating-vm"></a>Çoğaltılan bir VM 'nin özelliklerini güncelleştirme

[Azure geçişi: sunucu geçişi](migrate-services-overview.md#azure-migrate-server-migration-tool) , çoğaltılan bir sanal makine için ad, boyut, kaynak grubu, NIC yapılandırması gibi hedef özellikleri değiştirmenize olanak sağlar. Cmdlet, işlemin durumunu izlemek için izlenebilecek bir iş döndürür. 

```azurepowershell
# Retrieve the replicating VM details by using the discovered VM identifier
$ReplicatingServer = Get-AzMigrateServerReplication -DiscoveredMachineId $DiscoveredServer.ID

# View NIC details of the replicating server
Write-Output $ReplicatingServer.ProviderSpecificDetail.VMNic 
```
Aşağıdaki özellikler bir VM için güncelleştirilebilen olabilir.

- **VM adı** -parametresini kullanarak oluşturulacak Azure VM 'nin adını belirtin `TargetVMName` .
- **VM boyutu** -parametre kullanarak çoğaltma VM 'si Için kullanılacak Azure VM boyutunu belirtin `TargetVMSize` . Örneğin, bir VM 'yi Azure 'da D2_v2 VM 'ye geçirmek için değeri `TargetVMSize` "Standard_D2_v2" olarak belirtin.  
- **Sanal ağ** -PARAMETRESI kullanılarak VM 'nin geçirilmesi gereken Azure sanal ağının kimliğini belirtin `TargetNetworkId` . 
- **Kaynak grubu** -parametresini kullanarak kaynak grubu KIMLIĞI sağlayarak VM 'nin geçirilmesi gereken kaynak grubunun kimliğini belirtin `TargetResourceGroupId` .
- **Ağ arabirimi** -NIC yapılandırması cmdlet kullanılarak belirtilebilir `New-AzMigrateNicMapping` . Daha sonra nesnesi, `NicToUpdate` cmdlet 'teki parametresine bir giriş geçirilir `Set-AzMigrateServerReplication` . 

    - **IP ayırmayı Değiştir** -NIC IÇIN statik IP belirtmek üzere, PARAMETRESINI kullanarak VM IÇIN statik IP olarak kullanılacak IPv4 adresini sağlayın `TargetNicIP` . Bir NIC için dinamik olarak IP atamak için, parametresinin değeri olarak "Auto" sağlayın `TargetNicIP` .
    - "Birincil", "Ikincil" veya "DoNotCreate" değerlerini `TargetNicSelectionType` , NIC 'in birincil, ikincil veya GEÇIRILEN VM 'de oluşturulup oluşturulmayacağını belirtmek için kullanın. VM için birincil NIC olarak yalnızca bir NIC belirtilebilir. 
    - Bir NIC birincil oluşturmak için, ikincil hale getirilmeli veya geçirilen VM 'de oluşturulmayan diğer NIC 'Leri de belirtmeniz gerekir.  
    - NIC 'nin alt ağını değiştirmek için, parametresini kullanarak alt ağın adını belirtin `TargetNicSubnet` .

 - **Kullanılabilirlik bölgesi** -kullanılabilirlik alanlarını kullanmak için, parametre için kullanılabilirlik alanı değerini belirtin `TargetAvailabilityZone` .
 - **Kullanılabilirlik kümesi** kullanım için kullanılabilirlik kümesi, parametre için KULLANıLABILIRLIK kümesi kimliğini belirtin `TargetAvailabilitySet` .

Aşağıdaki örnekte, ilk NIC 'yi birincil olarak yaparak ve kendisine statik bir IP atayarak NIC yapılandırmasını güncelleştireceğiz. geçiş için ikinci NIC 'yi atacak ve hedef VM adını ve boyutunu güncelleştiririz. 

```azurepowershell
# Specify the NIC properties to be updated for a replicating VM. 
$NicMapping = @()
$NicMapping1 = New-AzMigrateNicMapping -NicId $ReplicatingServer.ProviderSpecificDetail.VMNic[0].NicId -TargetNicIP "xxx.xxx.xxx.xxx" -TargetNicSelectionType "Primary"
$NicMapping2 = New-AzMigrateNicMapping -NicId $ReplicatingServer.ProviderSpecificDetail.VMNic[1].NicId -TargetNicSelectionType "DoNotCreate"

$NicMapping += $NicMapping1
$NicMapping += $NicMapping2

# Update the name, size and NIC configuration of a replicating server
$UpdateJob = Set-AzMigrateServerReplication -InputObject $ReplicatingServer -TargetVMSize "Standard_DS13_v2" -TargetVMName "MyMigratedVM" -NicToUpdate $NicMapping

# Track job status to check for completion
while (($UpdateJob.State -eq "InProgress") -or ($UpdateJob.State -eq "NotStarted")){
        #If the job hasn't completed, sleep for 10 seconds before checking the job status again
        sleep 10;
        $UpdateJob = Get-AzMigrateJob -InputObject $UpdateJob
}
#Check if the Job completed successfully. The updated job state of a successfully completed job should be "Succeeded"
Write-Output $UpdateJob.State
```

Ayrıca, Azure geçişi projesindeki tüm çoğaltılan sunucuları listeleyebilir ve sonra VM özelliklerini güncelleştirmek için çoğaltma VM tanımlayıcısını kullanabilirsiniz.

```azurepowershell
# List all replicating VMs in an Azure Migrate project and filter the result for selecting the replication VM. This cmdlet will not return all properties of the replicating VM.
$ReplicatingServer = Get-AzMigrateServerReplication -ProjectName $MigrateProject.Name -ResourceGroupName $ResourceGroup.ResourceGroupName | where MachineName -eq $DiscoveredServer.DisplayName

# Retrieve replicating VM details using replicating VM identifier
$ReplicatingServer = Get-AzMigrateServerReplication -TargetObjectID $ReplicatingServer.Id 
```


## <a name="run-a-test-migration"></a>Geçiş testi çalıştırma

Delta çoğaltma başladığında, Azure 'a tam geçiş çalıştırmadan önce VM 'Ler için bir test geçişi çalıştırabilirsiniz. Geçirmeden önce her makine için geçişi en az bir kez test etmenizi kesinlikle öneririz. Cmdlet, işlemin durumunu izlemek için izlenebilecek bir iş döndürür. 

- Test geçişi çalıştırmak, geçişin beklendiği gibi çalıştığını denetler. Test geçişi, çalışır durumda olan ve çoğaltmaya devam eden şirket içi makineyi etkilemez. 
- Test geçişi, çoğaltılan verileri kullanarak bir Azure VM oluşturarak geçişe benzetir (genellikle Azure aboneliğinizdeki bir üretim dışı VNet 'e geçiş yapar).
- Geçişi doğrulamak, uygulama testi gerçekleştirmek ve tam geçişten önce herhangi bir sorunu gidermek için çoğaltılan test Azure VM 'yi kullanabilirsiniz.

Parametresini kullanarak sanal ağın KIMLIĞINI belirterek test için kullanılacak Azure sanal ağını seçin `TestNetworkID` .

```azurepowershell
# Retrieve the Azure virtual network created for testing 
$TestVirtualNetwork = Get-AzVirtualNetwork -Name MyTestVirtualNetwork

# Start test migration for a replicating server
$TestMigrationJob = Start-AzMigrateTestMigration -InputObject $ReplicatingServer -TestNetworkID $TestVirtualNetwork.Id

# Track job status to check for completion
while (($TestMigrationJob.State -eq "InProgress") -or ($TestMigrationJob.State -eq "NotStarted")){
        #If the job hasn't completed, sleep for 10 seconds before checking the job status again
        sleep 10;
        $TestMigrationJob = Get-AzMigrateJob -InputObject $TestMigrationJob
}
#Check if the Job completed successfully. The updated job state of a successfully completed job should be "Succeeded"
Write-Output $TestMigrationJob.State
```

Sınama tamamlandıktan sonra cmdlet 'ini kullanarak test geçişini temizleyin `Start-AzMigrateTestMigrationCleanup` . Cmdlet, işlemin durumunu izlemek için izlenebilecek bir iş döndürür. 

```azurepowershell
# Clean-up test migration for a replicating server
$CleanupTestMigrationJob = Start-AzMigrateTestMigrationCleanup -InputObject $ReplicatingServer

# Track job status to check for completion
while (($CleanupTestMigrationJob.State -eq "InProgress") -or ($CleanupTestMigrationJob.State -eq "NotStarted")){
        #If the job hasn't completed, sleep for 10 seconds before checking the job status again
        sleep 10;
        $CleanupTestMigrationJob = Get-AzMigrateJob -InputObject $CleanupTestMigrationJob
}
#Check if the Job completed successfully. The updated job state of a successfully completed job should be "Succeeded"
Write-Output $CleanupTestMigrationJob.State
```

## <a name="migrate-vms"></a>VM geçirme

Test geçişinin beklendiği gibi çalışıp çalışmadığını doğruladıktan sonra, aşağıdaki cmdlet 'i kullanarak çoğaltma sunucusunu geçirebilirsiniz. Cmdlet, işlemin durumunu izlemek için izlenebilecek bir iş döndürür. 

Kaynak sunucuyu kapatmak istemiyorsanız, `TurnOffSourceServer` parametresini kullanmayın.

```azurepowershell
# Start migration for a replicating server and turn off source server as part of migration
$MigrateJob = Start-AzMigrateServerMigration -InputObject $ReplicatingServer -TurnOffSourceServer 

# Track job status to check for completion
while (($MigrateJob.State -eq "InProgress") -or ($MigrateJob.State -eq "NotStarted")){
        #If the job hasn't completed, sleep for 10 seconds before checking the job status again
        sleep 10;
        $MigrateJob = Get-AzMigrateJob -InputObject $MigrateJob
}
#Check if the Job completed successfully. The updated job state of a successfully completed job should be "Succeeded"
Write-Output $MigrateJob.State
```

## <a name="complete-the-migration"></a>Geçişi tamamlamayı

1. Geçiş yapıldıktan sonra, aşağıdaki cmdlet 'i kullanarak, şirket içi makine için çoğaltmayı durdurun ve VM için çoğaltma durumu bilgilerini temizleyin. Cmdlet, işlemin durumunu izlemek için izlenebilecek bir iş döndürür. 

```azurepowershell
# Stop replication for a migrated server
$StopReplicationJob = Remove-AzMigrateServerReplication -InputObject $ReplicatingServer 

# Track job status to check for completion
while (($StopReplicationJob.State -eq "InProgress") -or ($StopReplicationJob.State -eq "NotStarted")){
        #If the job hasn't completed, sleep for 10 seconds before checking the job status again
        sleep 10;
        $StopReplicationJob = Get-AzMigrateJob -InputObject $StopReplicationJob
}
#Check if the Job completed successfully. The updated job state of a successfully completed job should be "Succeeded"
Write-Output $StopReplicationJob.State
```

2. Geçirilen makinelere Azure VM [Windows](../virtual-machines/extensions/agent-windows.md) veya [Linux](../virtual-machines/extensions/agent-linux.md) Aracısı 'nı yükler.
3. Veritabanı bağlantısı dizelerini ve web sunucusu yapılandırmalarını güncelleştirme gibi herhangi bir geçiş sonrası uygulama ayarı gerçekleştirin.
4. Geçirilen uygulamada son uygulama ve geçiş kabul testi gerçekleştirme işlemi şimdi Azure’da çalıştırılmaktadır.
5. Geçirilen Azure VM örneğine giden trafiği kesin.
6. Yerel sanal makine envanterinizden şirket içi sanal makineleri kaldırın.
7. Yerel yedeklemelerden şirket içi sanal makineleri kaldırın.
8. Azure sanal makinelerinin yeni konumunu ve IP adresini göstermek için herhangi bir iç belgeyi güncelleştirin. 

## <a name="post-migration-best-practices"></a>Geçiş sonrası en iyi uygulamalar

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