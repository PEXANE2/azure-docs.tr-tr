---
title: Azure Site İptalinde PowerShell'i kullanarak VMware olağanüstü durum kurtarmayı ayarlama
description: Azure Site Kurtarma'da PowerShell'i kullanarak VMware VM'lerin olağanüstü kurtarma için çoğaltma ve Azure'a nasıl başarısız olunmayı öğrenin.
author: sujayt
manager: rochakm
ms.service: site-recovery
ms.date: 01/10/2020
ms.topic: conceptual
ms.author: sutalasi
ms.openlocfilehash: d2dfaab3d01ea29b0f9ecba1e9d748415bed2edc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79257205"
---
# <a name="set-up-disaster-recovery-of-vmware-vms-to-azure-with-powershell"></a>PowerShell ile VMware VM'lerin Azure'a olağanüstü durum kurtarmasını ayarlama

Bu makalede, Azure PowerShell kullanarak VMware sanal makinelerini Azure'a nasıl kopyalayıp azure'a aktarabileceğinizi ve başarısız hale diğinizi görün.

Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> - Kurtarma Hizmetleri kasası oluşturun ve kasa bağlamını ayarlayın.
> - Kasadaki sunucu kaydını doğrulayın.
> - Çoğaltma ilkesi de dahil olmak üzere çoğaltmayı ayarlayın. vCenter sunucunuzu ekleyin ve VM'leri keşfedin.
> - bir vCenter sunucusu ekleyin ve keşfedin
> - Çoğaltma günlüklerini veya verileri tutmak ve VM'leri çoğaltmak için depolama hesapları oluşturun.
> - Yük devretme gerçekleştirin. Başarısız ayarları yapılandırın, sanal makineleri çoğaltmak için bir ayar gerçekleştirin.


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce:

- [Senaryo mimarisini ve bileşenlerini ](vmware-azure-architecture.md) anladığınızdan emin olun.
- Tüm bileşenler için [destek gereksinimlerini](site-recovery-support-matrix-to-azure.md) gözden geçirin.
- Azure PowerShell `Az` modülüne sahipsiniz. Azure PowerShell'i yüklemeniz veya yükseltmeniz gerekiyorsa, [Azure PowerShell'i yüklemek ve yapılandırmak için](/powershell/azure/install-az-ps)bu Kılavuzu izleyin.

## <a name="log-into-azure"></a>Azure'da oturum açın

Connect-AzAccount cmdlet'i kullanarak Azure aboneliğinize giriş yapın:

```azurepowershell
Connect-AzAccount
```
VMware sanal makinelerinizi çoğaltmak istediğiniz Azure aboneliğini seçin. Erişebildiğiniz Azure aboneliklerinin listesini almak için Get-AzSubscription cmdlet'ini kullanın. Select-AzSubscription cmdlet'i kullanarak çalışmak için Azure aboneliğini seçin.

```azurepowershell
Select-AzSubscription -SubscriptionName "ASR Test Subscription"
```
## <a name="set-up-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasası ayarlama

1. Kurtarma Hizmetleri kasasını oluşturmak için bir kaynak grubu oluşturun. Aşağıdaki örnekte, kaynak grubu VMwareDRtoAzurePS olarak adlandırılır ve Doğu Asya bölgesinde oluşturulur.

   ```azurepowershell
   New-AzResourceGroup -Name "VMwareDRtoAzurePS" -Location "East Asia"
   ```
   ```
   ResourceGroupName : VMwareDRtoAzurePS
   Location          : eastasia
   ProvisioningState : Succeeded
   Tags              :
   ResourceId        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/VMwareDRtoAzurePS
   ```

2. Kurtarma hizmetleri kasası oluşturun. Aşağıdaki örnekte, Kurtarma hizmetleri kasası VMwareDRToAzurePs olarak adlandırılır ve Doğu Asya bölgesinde ve önceki adımda oluşturulan kaynak grubunda oluşturulur.

   ```azurepowershell
   New-AzRecoveryServicesVault -Name "VMwareDRToAzurePs" -Location "East Asia" -ResourceGroupName "VMwareDRToAzurePs"
   ```
   ```
   Name              : VMwareDRToAzurePs
   ID                : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/VMwareDRToAzurePs/providers/Microsoft.RecoveryServices/vaults/VMwareDRToAzurePs
   Type              : Microsoft.RecoveryServices/vaults
   Location          : eastasia
   ResourceGroupName : VMwareDRToAzurePs
   SubscriptionId    : xxxxxxxx-xxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
   Properties        : Microsoft.Azure.Commands.RecoveryServices.ARSVaultProperties
   ```

3. Kasa için kasa kayıt anahtarını indirin. Kasa kayıt anahtarı, şirket içi Configuration Server'ı bu kasaya kaydetmek için kullanılır. Kayıt, Configuration Server yazılım yükleme sürecinin bir parçasıdır.

   ```azurepowershell
   #Get the vault object by name and resource group and save it to the $vault PowerShell variable 
   $vault = Get-AzRecoveryServicesVault -Name "VMwareDRToAzurePS" -ResourceGroupName "VMwareDRToAzurePS"

   #Download vault registration key to the path C:\Work
   Get-AzRecoveryServicesVaultSettingsFile -SiteRecovery -Vault $Vault -Path "C:\Work\"
   ```
   ```
   FilePath
   --------
   C:\Work\VMwareDRToAzurePs_2017-11-23T19-52-34.VaultCredentials
   ```

4. İndirilen kasa kayıt anahtarını kullanın ve Configuration Server'ın kurulumunu ve kaydını tamamlamak için aşağıda verilen makalelerdeki adımları izleyin.
   - [Koruma hedeflerinizi seçin](vmware-azure-set-up-source.md#choose-your-protection-goals)
   - [Kaynak ortamı ayarlama](vmware-azure-set-up-source.md#set-up-the-configuration-server)

### <a name="set-the-vault-context"></a>Kasa bağlamını ayarlama

Set-ASRVaultContext cmdlet'i kullanarak kasa bağlamını ayarlayın. Ayarlandıktan sonra, PowerShell oturumundaki sonraki Azure Site Kurtarma işlemleri seçili kasa bağlamında gerçekleştirilir.

> [!TIP]
> Azure Site Kurtarma PowerShell modülü (Az.RecoveryServices modülü) çoğu cmdlets için kolay takma adlar ile birlikte gelir. Modüldeki cmdletler * \<>-**AzRecoveryServicesAsr**\<Object>İşlemi* şeklinde dir ve * \<>-**ASR**\<Nesne>. * Kullanım kolaylığı için cmdlet takma adlarını değiştirebilirsiniz.

Aşağıdaki örnekte, PowerShell oturumunun kasa bağlamını belirtmek için $vault değişkeninden kasa ayrıntıları kullanılır.

   ```azurepowershell
   Set-ASRVaultContext -Vault $vault
   ```
   ```
   ResourceName      ResourceGroupName ResourceNamespace          ResourceType
   ------------      ----------------- -----------------          -----------
   VMwareDRToAzurePs VMwareDRToAzurePs Microsoft.RecoveryServices vaults
   ```

Set-ASRVaultContext cmdlet'e alternatif olarak, tonoz bağlamını ayarlamak için Import-AzRecoveryServicesAsrVaultSettingsFile cmdlet'i de kullanabilirsiniz. İthalat-AzRecoveryServicesAsrVaultSettingsFile cmdlet için kasa kayıt anahtarı dosyasının -yol parametresi olarak bulunduğu yolu belirtin. Örnek:

   ```azurepowershell
   Get-AzRecoveryServicesVaultSettingsFile -SiteRecovery -Vault $Vault -Path "C:\Work\"
   Import-AzRecoveryServicesAsrVaultSettingsFile -Path "C:\Work\VMwareDRToAzurePs_2017-11-23T19-52-34.VaultCredentials"
   ```
Bu makalenin sonraki bölümleri, Azure Site Kurtarma işlemleri için kasa bağlamının ayarlandığını varsayar.

## <a name="validate-vault-registration"></a>Kasa kaydını doğrula

Bu örnek için, aşağıdakileri görüyoruz:

- Bu kasaya bir yapılandırma sunucusu **(ConfigurationServer)** kaydedilmiştir.
- Ek bir işlem sunucusu **(ScaleOut-ProcessServer)** *ConfigurationServer'a* kaydedildi
- Hesaplar **(vCenter_account,** **WindowsAccount**, **LinuxAccount**) Yapılandırma sunucusunda kurulmuştur. Bu hesaplar vCenter sunucusunu eklemek, sanal makineleri keşfetmek ve çoğaltılacak olan Windows ve Linux sunucularında mobilite hizmeti yazılımını itmek için kullanılır.

1. Kayıtlı yapılandırma sunucuları Site Kurtarma'da bir kumaş nesnesi tarafından temsil edilir. Kasadaki kumaş nesnelerin listesini alın ve yapılandırma sunucusunu tanımlayın.

   ```azurepowershell
   # Verify that the Configuration server is successfully registered to the vault
   $ASRFabrics = Get-AzRecoveryServicesAsrFabric
   $ASRFabrics.count
   ```
   ```
   1
   ```
   ```azurepowershell
   #Print details of the Configuration Server
   $ASRFabrics[0]
   ```
   ```
   Name                  : 2c33d710a5ee6af753413e97f01e314fc75938ea4e9ac7bafbf4a31f6804460d
   FriendlyName          : ConfigurationServer
   ID                    : /Subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/VMwareDRToAzurePs/providers/Microsoft.RecoveryServices/vaults/VMwareDRToAzurePs/replicationFabrics
                           /2c33d710a5ee6af753413e97f01e314fc75938ea4e9ac7bafbf4a31f6804460d
   Type                  : Microsoft.RecoveryServices/vaults/replicationFabrics
   FabricType            : VMware
   SiteIdentifier        : ef7a1580-f356-4a00-aa30-7bf80f952510
   FabricSpecificDetails : Microsoft.Azure.Commands.RecoveryServices.SiteRecovery.ASRVMWareSpecificDetails
   ```

2. Makineleri çoğaltmak için kullanılabilecek işlem sunucularını tanımlayın.

   ```azurepowershell
   $ProcessServers = $ASRFabrics[0].FabricSpecificDetails.ProcessServers
   for($i=0; $i -lt $ProcessServers.count; $i++) {
    "{0,-5} {1}" -f $i, $ProcessServers[$i].FriendlyName
   }
   ```
   ```
   0     ScaleOut-ProcessServer
   1     ConfigurationServer
   ```

   Yukarıdaki çıktıdan ***$ProcessServers[0]*** *ScaleOut-ProcessServer'a* karşılık gelir ve ***$ProcessServers[1]*** *ConfigurationServer'daki* Process Server rolüne karşılık gelir

3. Configuration Server'da ayarlanan hesapları tanımlayın.

   ```azurepowershell
   $AccountHandles = $ASRFabrics[0].FabricSpecificDetails.RunAsAccounts
   #Print the account details
   $AccountHandles
   ```
   ```
   AccountId AccountName
   --------- -----------
   1         vCenter_account
   2         WindowsAccount
   3         LinuxAccount
   ```

   Yukarıdaki çıktıdan ***$AccountHandles[0]*** hesap *vCenter_account*, ***$AccountHandles[1]*** hesabına, *WindowsAccount*hesabına ve ***$AccountHandles[2]*** hesabına karşılık gelir *LinuxAccount* hesabına

## <a name="create-a-replication-policy"></a>Çoğaltma ilkesi oluşturma

Bu adımda, iki çoğaltma ilkeleri oluşturulur. VMware sanal makinelerini Azure'a çoğaltmak için bir ilke, diğeri de Azure'da çalışan sanal makineler üzerinden şirket içi VMware sitesine geri dönmek te başarısız oldu.

> [!NOTE]
> Çoğu Azure Site Kurtarma işlemi eş senkronize olarak yürütülür. Bir işlem başlattığınızda, bir Azure Site Kurtarma işi gönderilir ve bir iş izleme nesnesi döndürülür. Bu iş izleme nesnesi işlemin durumunu izlemek için kullanılabilir.

1. VMware sanal makinelerini belirtilen özelliklerle Azure'da çoğaltmak için *Çoğaltma Politikası* adlı bir çoğaltma ilkesi oluşturun.

   ```azurepowershell
   $Job_PolicyCreate = New-AzRecoveryServicesAsrPolicy -VMwareToAzure -Name "ReplicationPolicy" -RecoveryPointRetentionInHours 24 -ApplicationConsistentSnapshotFrequencyInHours 4 -RPOWarningThresholdInMinutes 60

   # Track Job status to check for completion
   while (($Job_PolicyCreate.State -eq "InProgress") -or ($Job_PolicyCreate.State -eq "NotStarted")){
           sleep 10;
           $Job_PolicyCreate = Get-ASRJob -Job $Job_PolicyCreate
   }

   #Display job status
   $Job_PolicyCreate
   ```
   ```
   Name             : 8d18e2d9-479f-430d-b76b-6bc7eb2d0b3e
   ID               : /Subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/VMwareDRToAzurePs/providers/Microsoft.RecoveryServices/vaults/VMwareDRToAzurePs/replicationJobs/8d18e2d
                      9-479f-430d-b76b-6bc7eb2d0b3e
   Type             :
   JobType          : AddProtectionProfile
   DisplayName      : Create replication policy
   ClientRequestId  : a162b233-55d7-4852-abac-3d595a1faac2 ActivityId: 9895234a-90ea-4c1a-83b5-1f2c6586252a
   State            : Succeeded
   StateDescription : Completed
   StartTime        : 11/24/2017 2:49:24 AM
   EndTime          : 11/24/2017 2:49:23 AM
   TargetObjectId   : ab31026e-4866-5440-969a-8ebcb13a372f
   TargetObjectType : ProtectionProfile
   TargetObjectName : ReplicationPolicy
   AllowedActions   :
   Tasks            : {Prerequisites check for creating the replication policy, Creating the replication policy}
   Errors           : {}
   ```

2. Azure'dan şirket içi VMware sitesine geri dönüş için kullanmak üzere bir çoğaltma ilkesi oluşturun.

   ```azurepowershell
   $Job_FailbackPolicyCreate = New-AzRecoveryServicesAsrPolicy -AzureToVMware -Name "ReplicationPolicy-Failback" -RecoveryPointRetentionInHours 24 -ApplicationConsistentSnapshotFrequencyInHours 4 -RPOWarningThresholdInMinutes 60
   ```

   İşlemin tamamlanmasını izlemek için *$Job_FailbackPolicyCreate'deki* iş ayrıntılarını kullanın.

   * Configuration Server ile çoğaltma ilkelerini eşlemek için bir koruma kapsayıcısı eşlemesi oluşturun.

   ```azurepowershell
   #Get the protection container corresponding to the Configuration Server
   $ProtectionContainer = Get-AzRecoveryServicesAsrProtectionContainer -Fabric $ASRFabrics[0]

   #Get the replication policies to map by name.
   $ReplicationPolicy = Get-AzRecoveryServicesAsrPolicy -Name "ReplicationPolicy"
   $FailbackReplicationPolicy = Get-AzRecoveryServicesAsrPolicy -Name "ReplicationPolicy-Failback"

   # Associate the replication policies to the protection container corresponding to the Configuration Server.

   $Job_AssociatePolicy = New-AzRecoveryServicesAsrProtectionContainerMapping -Name "PolicyAssociation1" -PrimaryProtectionContainer $ProtectionContainer -Policy $ReplicationPolicy

   # Check the job status
   while (($Job_AssociatePolicy.State -eq "InProgress") -or ($Job_AssociatePolicy.State -eq "NotStarted")){
           sleep 10;
           $Job_AssociatePolicy = Get-ASRJob -Job $Job_AssociatePolicy
   }
   $Job_AssociatePolicy.State

   <# In the protection container mapping used for failback (replicating failed over virtual machines
      running in Azure, to the primary VMware site.) the protection container corresponding to the
      Configuration server acts as both the Primary protection container and the recovery protection
      container
   #>
    $Job_AssociateFailbackPolicy = New-AzRecoveryServicesAsrProtectionContainerMapping -Name "FailbackPolicyAssociation" -PrimaryProtectionContainer $ProtectionContainer -RecoveryProtectionContainer $ProtectionContainer -Policy $FailbackReplicationPolicy

   # Check the job status
   while (($Job_AssociateFailbackPolicy.State -eq "InProgress") -or ($Job_AssociateFailbackPolicy.State -eq "NotStarted")){
           sleep 10;
           $Job_AssociateFailbackPolicy = Get-ASRJob -Job $Job_AssociateFailbackPolicy
   }
   $Job_AssociateFailbackPolicy.State

   ```

## <a name="add-a-vcenter-server-and-discover-vms"></a>VCenter sunucusu ekleyin ve VM'leri keşfedin

IP adresine veya ana bilgisayar adına göre bir vCenter Server ekleyin. **-bağlantı noktası** parametresi bağlanmak için vCenter sunucusundaki bağlantı noktasını belirtir, **-Ad** parametresi vCenter sunucusu için kullanılacak dostu bir ad belirtir ve **-Hesap** parametresi vCenter sunucusu tarafından yönetilen sanal makineleri keşfetmek için yapılandırma sunucusundaki hesap tutamacını belirtir.

```azurepowershell
# The $AccountHandles[0] variable holds details of vCenter_account

$Job_AddvCenterServer = New-AzRecoveryServicesAsrvCenter -Fabric $ASRFabrics[0] -Name "MyvCenterServer" -IpOrHostName "10.150.24.63" -Account $AccountHandles[0] -Port 443

#Wait for the job to complete and ensure it completed successfully

while (($Job_AddvCenterServer.State -eq "InProgress") -or ($Job_AddvCenterServer.State -eq "NotStarted")) {
        sleep 30;
        $Job_AddvCenterServer = Get-ASRJob -Job $Job_AddvCenterServer
}
$Job_AddvCenterServer
```
```
Name             : 0f76f937-f9cf-4e0e-bf27-10c9d1c252a4
ID               : /Subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/VMwareDRToAzurePs/providers/Microsoft.RecoveryServices/vaults/VMwareDRToAzurePs/replicationJobs/0f76f93
                   7-f9cf-4e0e-bf27-10c9d1c252a4
Type             :
JobType          : DiscoverVCenter
DisplayName      : Add vCenter server
ClientRequestId  : a2af8892-5686-4d64-a528-10445bc2f698 ActivityId: 7ec05aad-002e-4da0-991f-95d0de7a9f3a
State            : Succeeded
StateDescription : Completed
StartTime        : 11/24/2017 2:41:47 AM
EndTime          : 11/24/2017 2:44:37 AM
TargetObjectId   : 10.150.24.63
TargetObjectType : VCenter
TargetObjectName : MyvCenterServer
AllowedActions   :
Tasks            : {Adding vCenter server}
Errors           : {}
```

## <a name="create-storage-accounts-for-replication"></a>Çoğaltma için depolama hesapları oluşturma

**Yönetilen diske yazmak için [Powershell Az.RecoveryServices modül 2.0.0'ı](https://www.powershellgallery.com/packages/Az.RecoveryServices/2.0.0-preview) daha fazla kullanın.** Yalnızca bir günlük depolama hesabı oluşturulmasını gerektirir. Yalnızca geçici günlükleri depolamak için kullanıldığından standart bir hesap türü ve LRS artıklığı kullanılması önerilir. Depolama hesabının kasayla aynı Azure bölgesinde oluşturulduğundan emin olun.

Az.RecoveryServices modülünün 2.0.0'dan büyük bir sürümünü kullanıyorsanız, depolama hesapları oluşturmak için aşağıdaki adımları kullanın. Bu depolama hesapları daha sonra sanal makineleri çoğaltmak için kullanılır. Depolama hesaplarının kasayla aynı Azure bölgesinde oluşturulduğundan emin olun. Çoğaltma için varolan bir depolama hesabı kullanmayı planlıyorsanız, bu adımı atlayabilirsiniz.

> [!NOTE]
> Şirket içi sanal makineleri premium depolama hesabına kopyalarken, ek bir standart depolama hesabı (günlük depolama hesabı) belirtmeniz gerekir. Günlük depolama hesabı, günlükler premium depolama hedefine uygulanına kadar çoğaltma günlüklerini ara depo olarak tutar.
>

```azurepowershell

$PremiumStorageAccount = New-AzStorageAccount -ResourceGroupName "VMwareDRToAzurePs" -Name "premiumstorageaccount1" -Location "East Asia" -SkuName Premium_LRS

$LogStorageAccount = New-AzStorageAccount -ResourceGroupName "VMwareDRToAzurePs" -Name "logstorageaccount1" -Location "East Asia" -SkuName Standard_LRS

$ReplicationStdStorageAccount= New-AzStorageAccount -ResourceGroupName "VMwareDRToAzurePs" -Name "replicationstdstorageaccount1" -Location "East Asia" -SkuName Standard_LRS
```

## <a name="replicate-vmware-vms"></a>VMware VM'leri çoğaltma

Sanal makinelerin vCenter sunucusundan keşfedilmesi yaklaşık 15-20 dakika sürer. Keşfedildikten sonra, keşfedilen her sanal makine için Azure Site Kurtarma'da korunabilir bir öğe nesnesi oluşturulur. Bu adımda, keşfedilen sanal makinelerden üçü önceki adımda oluşturulan Azure Depolama hesaplarına çoğaltılır.

Keşfedilen bir sanal makineyi korumak için aşağıdaki ayrıntılara ihtiyacınız olacaktır:

* Çoğaltılacak korunabilir öğe.
* Sanal makineyi çoğaltmak için depolama hesabı (yalnızca depolama hesabına çoğalıyorsanız). 
* Sanal makineleri premium depolama hesabına veya yönetilen bir diske korumak için günlük depolama gereklidir.
* Çoğaltma için kullanılacak İşlem Sunucusu. Kullanılabilir işlem sunucularının listesi ***$ProcessServers[0]***  *(ScaleOut-ProcessServer)* ve ***$ProcessServers[1]*** *(ConfigurationServer)* değişkenlerinde alındı ve kaydedildi.
* Mobilite hizmet yazılımını makinelere itmek için kullanılacak hesap. Kullanılabilir hesapların listesi alındı ve ***$AccountHandles*** değişkeninde depolandı.
* Çoğaltma ilkesi için koruma kapsayıcıeşleme çoğaltma için kullanılacak.
* Sanal makinelerin başarısız bir şekilde oluşturulması gereken kaynak grubu.
* İsteğe bağlı olarak, başarısız olan Azure sanal ağı ve alt ağı nın bağlanması gerekir.

Şimdi bu tabloda belirtilen ayarları kullanarak aşağıdaki sanal makineleri çoğaltmak


|Sanal makine  |İşlem Sunucusu        |Depolama Hesabı              |Günlük Depolama hesabı  |İlke           |Mobilite hizmet kurulumu hesabı|Hedef kaynak grubu  | Hedef sanal ağ  |Hedef alt ağ  |
|-----------------|----------------------|-----------------------------|---------------------|-----------------|-----------------------------------------|-----------------------|-------------------------|---------------|
|CentOSVM1       |ConfigurationServer   |Yok| logstorageaccount1                 |Çoğaltma Politikası|LinuxHesabı                             |VMwareDRToAzurePs      |ASR-vnet                 |Alt ağ-1       |
|Win2K12VM1       |ScaleOut-ProcessServer|premiumstorageaccount1       |logstorageaccount1   |Çoğaltma Politikası|Windows Hesabı                           |VMwareDRToAzurePs      |ASR-vnet                 |Alt ağ-1       |   
|CentOSVM2       |ConfigurationServer   |replicationstdstorageaccount1| Yok                 |Çoğaltma Politikası|LinuxHesabı                             |VMwareDRToAzurePs      |ASR-vnet                 |Alt ağ-1       |   


```azurepowershell

#Get the target resource group to be used
$ResourceGroup = Get-AzResourceGroup -Name "VMwareToAzureDrPs"

#Get the target virtual network to be used
$RecoveryVnet = Get-AzVirtualNetwork -Name "ASR-vnet" -ResourceGroupName "asrrg" 

#Get the protection container mapping for replication policy named ReplicationPolicy
$PolicyMap  = Get-AzRecoveryServicesAsrProtectionContainerMapping -ProtectionContainer $ProtectionContainer | where PolicyFriendlyName -eq "ReplicationPolicy"

#Get the protectable item corresponding to the virtual machine CentOSVM1
$VM1 = Get-AzRecoveryServicesAsrProtectableItem -ProtectionContainer $ProtectionContainer -FriendlyName "CentOSVM1"

# Enable replication for virtual machine CentOSVM1 using the Az.RecoveryServices module 2.0.0 onwards to replicate to managed disks
# The name specified for the replicated item needs to be unique within the protection container. Using a random GUID to ensure uniqueness
$Job_EnableReplication1 = New-AzRecoveryServicesAsrReplicationProtectedItem -VMwareToAzure -ProtectableItem $VM1 -Name (New-Guid).Guid -ProtectionContainerMapping $PolicyMap -ProcessServer $ProcessServers[1] -Account $AccountHandles[2] -RecoveryResourceGroupId $ResourceGroup.ResourceId -logStorageAccountId $LogStorageAccount.Id -RecoveryAzureNetworkId $RecoveryVnet.Id -RecoveryAzureSubnetName "Subnet-1"

# Alternatively, if the virtual machine CentOSVM1 has CMK enabled disks, enable replication using Az module 3.3.0 onwards as below
# $diskID is the Disk Encryption Set ID to be used for all replica managed disks and target managed disks in the target region
$Job_EnableReplication1 = New-AzRecoveryServicesAsrReplicationProtectedItem -VMwareToAzure -ProtectableItem $VM1 -Name (New-Guid).Guid -ProtectionContainerMapping $PolicyMap -ProcessServer $ProcessServers[1] -Account $AccountHandles[2] -RecoveryResourceGroupId $ResourceGroup.ResourceId -logStorageAccountId -DiskEncryptionSetId $diskId $LogStorageAccount.Id -RecoveryAzureNetworkId $RecoveryVnet.Id -RecoveryAzureSubnetName "Subnet-1"

#Get the protectable item corresponding to the virtual machine Win2K12VM1
$VM2 = Get-AzRecoveryServicesAsrProtectableItem -ProtectionContainer $ProtectionContainer -FriendlyName "Win2K12VM1"

# Enable replication for virtual machine Win2K12VM1
$Job_EnableReplication2 = New-AzRecoveryServicesAsrReplicationProtectedItem -VMwareToAzure -ProtectableItem $VM2 -Name (New-Guid).Guid -ProtectionContainerMapping $PolicyMap -RecoveryAzureStorageAccountId $PremiumStorageAccount.Id -LogStorageAccountId $LogStorageAccount.Id -ProcessServer $ProcessServers[0] -Account $AccountHandles[1] -RecoveryResourceGroupId $ResourceGroup.ResourceId -RecoveryAzureNetworkId $RecoveryVnet.Id -RecoveryAzureSubnetName "Subnet-1"

#Get the protectable item corresponding to the virtual machine CentOSVM2
$VM3 = Get-AzRecoveryServicesAsrProtectableItem -ProtectionContainer $ProtectionContainer -FriendlyName "CentOSVM2"

# Enable replication for virtual machine CentOSVM2
$Job_EnableReplication3 = New-AzRecoveryServicesAsrReplicationProtectedItem -VMwareToAzure -ProtectableItem $VM3 -Name (New-Guid).Guid -ProtectionContainerMapping $PolicyMap -RecoveryAzureStorageAccountId $ReplicationStdStorageAccount.Id  -ProcessServer $ProcessServers[1] -Account $AccountHandles[2] -RecoveryResourceGroupId $ResourceGroup.ResourceId -RecoveryAzureNetworkId $RecoveryVnet.Id -RecoveryAzureSubnetName "Subnet-1"

```

Etkinleştirme çoğaltma işi başarıyla tamamlandıktan sonra, sanal makineler için ilk çoğaltma başlatılır. İlk çoğaltma, çoğaltılacak veri miktarına ve çoğaltma için kullanılabilir bant genişliğine bağlı olarak biraz zaman alabilir. İlk çoğaltma tamamlandıktan sonra, sanal makine korumalı duruma geçer. Sanal makine korunan bir duruma ulaştığında, sanal makine için bir test başarısızlık gerçekleştirebilir, kurtarma planları vb ekleyebilirsiniz.

Get-ASRReplicationProtectedItem cmdlet ile sanal makinenin çoğaltma durumunu ve çoğaltma durumunu kontrol edebilirsiniz.

```azurepowershell
Get-AzRecoveryServicesAsrReplicationProtectedItem -ProtectionContainer $ProtectionContainer | Select FriendlyName, ProtectionState, ReplicationHealth
```
```
FriendlyName ProtectionState                 ReplicationHealth
------------ ---------------                 -----------------
CentOSVM1    Protected                       Normal
CentOSVM2    InitialReplicationInProgress    Normal
Win2K12VM1   Protected                       Normal
```

## <a name="configure-failover-settings"></a>Hata ayarlarına yapı

Korumalı makineler için arıza ayarları Set-ASRReplicationProtectedItem cmdlet kullanılarak güncellenebilir. Bu cmdlet ile güncelleştirilebilen ayarlardan bazıları şunlardır:
* Failover'da oluşturulacak sanal makinenin adı
* Başarısız bir şekilde oluşturulacak sanal makinenin VM boyutu
* Sanal makinenin NIC'lerinin failover'da bağlanması gereken Azure sanal ağ ve alt ağ
* Yönetilen disklere başarısız
* Azure Karma Kullanım Avantajı Uygula
* Başarısız bir şekilde sanal makineye atanacak hedef sanal ağdan statik bir IP adresi atayın.

Bu örnekte, sanal makine *Win2K12VM1* için failover oluşturulacak sanal makinenin VM boyutunu günceller ve sanal makine nin başarısız diskler kullandığını belirtiriz.

```azurepowershell
$ReplicatedVM1 = Get-AzRecoveryServicesAsrReplicationProtectedItem -FriendlyName "Win2K12VM1" -ProtectionContainer $ProtectionContainer

Set-AzRecoveryServicesAsrReplicationProtectedItem -InputObject $ReplicatedVM1 -Size "Standard_DS11" -UseManagedDisk True
```
```
Name             : cafa459c-44a7-45b0-9de9-3d925b0e7db9
ID               : /Subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/VMwareDRToAzurePs/providers/Microsoft.RecoveryServices/vaults/VMwareDRToAzurePs/replicationJobs/cafa459
                   c-44a7-45b0-9de9-3d925b0e7db9
Type             :
JobType          : UpdateVmProperties
DisplayName      : Update the virtual machine
ClientRequestId  : b0b51b2a-f151-4e9a-a98e-064a5b5131f3 ActivityId: ac2ba316-be7b-4c94-a053-5363f683d38f
State            : InProgress
StateDescription : InProgress
StartTime        : 11/24/2017 2:04:26 PM
EndTime          :
TargetObjectId   : 88bc391e-d091-11e7-9484-000c2955bb50
TargetObjectType : ProtectionEntity
TargetObjectName : Win2K12VM1
AllowedActions   :
Tasks            : {Update the virtual machine properties}
Errors           : {}
```

## <a name="run-a-test-failover"></a>Yük devretme testi çalıştırma

1. Aşağıdaki gibi bir DR matkabı (test failover) çalıştırın:

   ```azurepowershell
   #Test failover of Win2K12VM1 to the test virtual network "V2TestNetwork"

   #Get details of the test failover virtual network to be used
   TestFailovervnet = Get-AzVirtualNetwork -Name "V2TestNetwork" -ResourceGroupName "asrrg" 

   #Start the test failover operation
   $TFOJob = Start-AzRecoveryServicesAsrTestFailoverJob -ReplicationProtectedItem $ReplicatedVM1 -AzureVMNetworkId $TestFailovervnet.Id -Direction PrimaryToRecovery
   ```
2. Test başarısızlığı işi başarıyla tamamlandığında, Azure'da adına *"-Test"* (Bu durumda Win2K12VM1-Test) ile sabitlenmiş bir sanal makinenin oluşturulduğunu fark edeceksiniz.
3. Artık sanal makine üzerinden başarısız olan teste bağlanabilir ve test inbaşarısızliğini doğrulayabilirsiniz.
4. Start-ASRTestFailoverCleanupJob cmdlet'i kullanarak test başarısızlarını temizleyin. Bu işlem, test başarısız işleminin bir parçası olarak oluşturulan sanal makineyi siler.

   ```azurepowershell
   $Job_TFOCleanup = Start-AzRecoveryServicesAsrTestFailoverCleanupJob -ReplicationProtectedItem $ReplicatedVM1
   ```

## <a name="fail-over-to-azure"></a>Azure'a yük devretme

Bu adımda, sanal makine Win2K12VM1 üzerinde belirli bir kurtarma noktasına başarısız.

1. Başarısız olmak için kullanılacak kullanılabilir kurtarma noktalarının listesini alın:
   ```azurepowershell
   # Get the list of available recovery points for Win2K12VM1
   $RecoveryPoints = Get-AzRecoveryServicesAsrRecoveryPoint -ReplicationProtectedItem $ReplicatedVM1
   "{0} {1}" -f $RecoveryPoints[0].RecoveryPointType, $RecoveryPoints[0].RecoveryPointTime
   ```
   ```
   CrashConsistent 11/24/2017 5:28:25 PM
   ```
   ```azurepowershell

   #Start the failover job
   $Job_Failover = Start-AzRecoveryServicesAsrUnplannedFailoverJob -ReplicationProtectedItem $ReplicatedVM1 -Direction PrimaryToRecovery -RecoveryPoint $RecoveryPoints[0]
   do {
           $Job_Failover = Get-ASRJob -Job $Job_Failover;
           sleep 60;
   } while (($Job_Failover.State -eq "InProgress") -or ($JobFailover.State -eq "NotStarted"))
   $Job_Failover.State
   ```
   ```
   Succeeded
   ```

2. Başarılı bir şekilde başarısız olduktan sonra, başarısız işlemi gerçekleştirebilir ve Azure'dan şirket içi VMware sitesine geri ters çoğaltma ayarlayabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
[Azure Site Kurtarma PowerShell başvurusunu](https://docs.microsoft.com/powershell/module/Az.RecoveryServices)kullanarak daha fazla görevi nasıl otomatikleştirebilirsiniz öğrenin.
