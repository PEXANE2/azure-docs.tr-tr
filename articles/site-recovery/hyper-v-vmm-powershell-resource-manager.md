---
title: Azure Site Recovery/PowerShell ile Hyper-V (VMM ile) ile ikincil bir siteye olağanüstü durum kurtarmayı ayarlama
description: VMM bulutlarındaki Hyper-V VM 'lerinin Azure Site Recovery ve PowerShell kullanarak ikincil bir VMM sitesine olağanüstü durum kurtarma işlemini nasıl ayarlayabileceğinizi açıklar.
services: site-recovery
author: sujayt
manager: rochakm
ms.topic: article
ms.date: 1/10/2020
ms.author: sutalasi
ms.openlocfilehash: a46bca5c5c55338f8bea7e1ff370f92ce6a2d577
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2020
ms.locfileid: "76841055"
---
# <a name="set-up-disaster-recovery-of-hyper-v-vms-to-a-secondary-site-by-using-powershell-resource-manager"></a>PowerShell kullanarak Hyper-V VM 'lerini ikincil bir siteye olağanüstü durum kurtarmayı ayarlama (Kaynak Yöneticisi)

Bu makalede, System Center Virtual Machine Manager bulutlarındaki Hyper-V VM 'lerinin [Azure Site Recovery](site-recovery-overview.md)kullanarak ikincil şirket içi sitede bir Virtual Machine Manager buluta çoğaltılmasına yönelik adımların nasıl otomatikleştirdiği gösterilmektedir.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Ön koşullar

- [Senaryo mimarisini ve bileşenlerini](hyper-v-vmm-architecture.md) gözden geçirin.
- Tüm bileşenler için [destek gereksinimlerini](site-recovery-support-matrix-to-sec-site.md) gözden geçirin.
- Virtual Machine Manager sunucularının ve Hyper-V konaklarının [destek gereksinimleriyle](site-recovery-support-matrix-to-sec-site.md)uyumlu olduğundan emin olun.
- Çoğaltmak istediğiniz VM 'Lerin [çoğaltılan makine desteğiyle](site-recovery-support-matrix-to-sec-site.md)uyumlu olduğundan emin olun.

## <a name="prepare-for-network-mapping"></a>Ağ eşlemeye hazırlama

Kaynak ve hedef bulutlarda şirket içi Virtual Machine Manager VM ağları arasında [ağ eşleme](hyper-v-vmm-network-mapping.md) haritaları. Eşleme sürecinde şu işlemler gerçekleştirilir:

- VM'ler yük devretme sonrasında uygun hedef VM ağlara bağlanır.
- Çoğaltma VM'lerini en uygun şekilde hedef Hyper-V konağı sunucularına yerleştirir.
- Ağ eşlemesini yapılandırmazsanız, yük devretme işleminden sonra çoğaltma VM 'Leri VM ağına bağlanmaz.

Virtual Machine Manager aşağıdaki şekilde hazırlayın:

- Kaynak ve hedef Virtual Machine Manager sunucularında [Virtual Machine Manager Mantıksal ağlarınız](https://docs.microsoft.com/system-center/vmm/network-logical) olduğundan emin olun:
  - Kaynak sunucusundaki mantıksal ağ, Hyper-V konaklarının bulunduğu kaynak bulutla ilişkilendirilmelidir.
  - Hedef sunucudaki mantıksal ağ, hedef bulutla ilişkilendirilmelidir.
- Kaynak ve hedef Virtual Machine Manager sunucularında [VM ağlarının](https://docs.microsoft.com/system-center/vmm/network-virtual) bulunduğundan emin olun. VM ağları, her bir konumdaki mantıksal ağa bağlanmış olmalıdır.
- Kaynak Hyper-V konaklarındaki VM'leri kaynak VM ağına bağlayın.

## <a name="prepare-for-powershell"></a>PowerShell için hazırlanma

Başlamaya Azure PowerShell olduğunuzdan emin olun:

- Zaten PowerShell kullanıyorsanız, sürüm 0.8.10 veya sonraki bir sürüme yükseltin. PowerShell 'i ayarlama hakkında [daha fazla bilgi edinin](/powershell/azureps-cmdlets-docs) .
- PowerShell 'i ayarladıktan ve yapılandırdıktan sonra, [hizmet cmdlet 'lerini](/powershell/azure/overview)gözden geçirin.
- PowerShell 'de parametre değerlerini, girişleri ve çıkışları kullanma hakkında daha fazla bilgi edinmek için [kullanmaya başlama](/powershell/azure/get-started-azureps) kılavuzunu okuyun.

## <a name="set-up-a-subscription"></a>Abonelik ayarlama

1. PowerShell 'den Azure hesabınızda oturum açın.

   ```azurepowershell
   $UserName = "<user@live.com>"
   $Password = "<password>"
   $SecurePassword = ConvertTo-SecureString -AsPlainText $Password -Force
   $Cred = New-Object System.Management.Automation.PSCredential -ArgumentList $UserName, $SecurePassword
   Connect-AzAccount #-Credential $Cred
   ```

1. Abonelik kimlikleri ile aboneliklerinizin bir listesini alın. Kurtarma Hizmetleri kasasını oluşturmak istediğiniz aboneliğin KIMLIĞINI aklınızda bulabilirsiniz.

   ```azurepowershell
   Get-AzSubscription
   ```

1. Kasa için abonelik ayarlayın.

   ```azurepowershell
   Set-AzContext –SubscriptionID <subscriptionId>
   ```

## <a name="create-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasası oluşturma

1. Yoksa bir Azure Resource Manager kaynak grubu oluşturun.

   ```azurepowershell
   New-AzResourceGroup -Name #ResourceGroupName -Location #location
   ```

1. Yeni bir kurtarma hizmetleri Kasası oluşturun. Kasa nesnesini daha sonra kullanılacak bir değişkene kaydedin.

   ```azurepowershell
   $vault = New-AzRecoveryServicesVault -Name #vaultname -ResourceGroupName #ResourceGroupName -Location #location
   ```

   `Get-AzRecoveryServicesVault` cmdlet 'ini kullanarak oluşturduktan sonra kasa nesnesini elde edebilirsiniz.

## <a name="set-the-vault-context"></a>Kasa bağlamını ayarla

1. Mevcut bir kasayı alın.

   ```azurepowershell
   $vault = Get-AzRecoveryServicesVault -Name #vaultname
   ```

1. Kasa bağlamını ayarlayın.

   ```azurepowershell
   Set-AzRecoveryServicesAsrVaultContext -Vault $vault
   ```

## <a name="install-the-site-recovery-provider"></a>Site Recovery sağlayıcıyı yükler

1. Virtual Machine Manager makinede, aşağıdaki komutu çalıştırarak bir dizin oluşturun:

   ```azurepowershell
   New-Item -Path C:\ASR -ItemType Directory
   ```

1. İndirilen sağlayıcı kurulum dosyasını kullanarak dosyaları ayıklayın.

   ```console
   pushd C:\ASR\
   .\AzureSiteRecoveryProvider.exe /x:. /q
   ```

1. Sağlayıcıyı yükledikten sonra yükleme işleminin bitmesini bekleyin.

   ```console
   .\SetupDr.exe /i
   $installationRegPath = "HKLM:\Software\Microsoft\Microsoft System Center Virtual Machine Manager Server\DRAdapter"
   do
   {
     $isNotInstalled = $true;
     if(Test-Path $installationRegPath)
     {
       $isNotInstalled = $false;
     }
   }While($isNotInstalled)
   ```

1. Sunucuyu kasaya kaydedin.

   ```console
   $BinPath = $env:SystemDrive+"\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin"
   pushd $BinPath
   $encryptionFilePath = "C:\temp\".\DRConfigurator.exe /r /Credentials $VaultSettingFilePath /vmmfriendlyname $env:COMPUTERNAME /dataencryptionenabled $encryptionFilePath /startvmmservice
   ```

## <a name="create-and-associate-a-replication-policy"></a>Çoğaltma ilkesi oluşturma ve ilişkilendirme

1. Bu durumda Hyper-V 2012 R2 için şu şekilde bir çoğaltma ilkesi oluşturun:

   ```azurepowershell
   $ReplicationFrequencyInSeconds = "300";        #options are 30,300,900
   $PolicyName = “replicapolicy”
   $RepProvider = HyperVReplica2012R2
   $Recoverypoints = 24                    #specify the number of hours to retain recovery points
   $AppConsistentSnapshotFrequency = 4 #specify the frequency (in hours) at which app consistent snapshots are taken
   $AuthMode = "Kerberos"  #options are "Kerberos" or "Certificate"
   $AuthPort = "8083"  #specify the port number that will be used for replication traffic on Hyper-V hosts
   $InitialRepMethod = "Online" #options are "Online" or "Offline"

   $policyresult = New-AzRecoveryServicesAsrPolicy -Name $policyname -ReplicationProvider $RepProvider -ReplicationFrequencyInSeconds $Replicationfrequencyinseconds -NumberOfRecoveryPointsToRetain $recoverypoints -ApplicationConsistentSnapshotFrequencyInHours $AppConsistentSnapshotFrequency -Authentication $AuthMode -ReplicationPort $AuthPort -ReplicationMethod $InitialRepMethod
   ```

   > [!NOTE]
   > Virtual Machine Manager bulutu, farklı Windows Server sürümlerini çalıştıran Hyper-V konakları içerebilir, ancak çoğaltma ilkesi işletim sisteminin belirli bir sürümü içindir. Farklı işletim sistemlerinde çalışan farklı konaklarınız varsa, her bir sistem için ayrı çoğaltma ilkeleri oluşturun. Örneğin, Windows Server 2012 üzerinde çalışan beş ana bilgisayar ve Windows Server 2012 R2 üzerinde çalışan üç ana bilgisayar varsa, iki çoğaltma ilkesi oluşturun. Her işletim sistemi türü için bir tane oluşturabilirsiniz.

1. Birincil koruma kapsayıcısını (birincil Virtual Machine Manager bulutu) ve kurtarma koruma kapsayıcısını (kurtarma Virtual Machine Manager bulutu) alın.

   ```azurepowershell
   $PrimaryCloud = "testprimarycloud"
   $primaryprotectionContainer = Get-AzRecoveryServicesAsrProtectionContainer -FriendlyName $PrimaryCloud;

   $RecoveryCloud = "testrecoverycloud"
   $recoveryprotectionContainer = Get-AzRecoveryServicesAsrProtectionContainer -FriendlyName $RecoveryCloud;
   ```

1. Kolay adı kullanarak oluşturduğunuz çoğaltma ilkesini alın.

   ```azurepowershell
   $policy = Get-AzRecoveryServicesAsrPolicy -FriendlyName $policyname
   ```

1. Çoğaltma ilkesiyle koruma kapsayıcısının (Virtual Machine Manager bulutu) ilişkilendirmesini başlatın.

   ```azurepowershell
   $associationJob  = New-AzRecoveryServicesAsrProtectionContainerMapping -Policy $Policy -PrimaryProtectionContainer $primaryprotectionContainer -RecoveryProtectionContainer $recoveryprotectionContainer
   ```

1. İlke ilişkilendirme işinin bitmesini bekleyin. İşin tamamlanıp bitmediğini denetlemek için aşağıdaki PowerShell kod parçacığını kullanın:

   ```azurepowershell
   $job = Get-AzRecoveryServicesAsrJob -Job $associationJob

   if($job -eq $null -or $job.StateDescription -ne "Completed")
   {
     $isJobLeftForProcessing = $true;
   }
   ```

1. İşi işlemeyi tamamladıktan sonra aşağıdaki komutu çalıştırın:

   ```azurepowershell
   if($isJobLeftForProcessing)
   {
     Start-Sleep -Seconds 60
   }
   While($isJobLeftForProcessing)
   ```

İşlemin tamamlandığını denetlemek için, [izleme etkinliğindeki](#monitor-activity)adımları izleyin.

##  <a name="configure-network-mapping"></a>Ağ eşlemesini yapılandırma

1. Geçerli kasadaki sunucuları almak için bu komutu kullanın. Komut, Site Recovery sunucularını `$Servers` dizi değişkeninde depolar.

   ```azurepowershell
   $Servers = Get-AzRecoveryServicesAsrFabric
   ```

1. Kaynak Virtual Machine Manager sunucusu ve hedef Virtual Machine Manager sunucusu için ağları almak üzere bu komutu çalıştırın.

   ```azurepowershell
   $PrimaryNetworks = Get-AzRecoveryServicesAsrNetwork -Name $Servers[0]

   $RecoveryNetworks = Get-AzRecoveryServicesAsrNetwork -Name $Servers[1]
   ```

   > [!NOTE]
   > Kaynak Virtual Machine Manager sunucusu, sunucu dizisindeki birinci veya ikinci bir dizin olabilir. Virtual Machine Manager Server adlarını denetleyin ve ağları uygun şekilde alın.

1. Bu cmdlet, birincil ağ ile kurtarma ağı arasında bir eşleme oluşturur. Birincil ağı `$PrimaryNetworks`ilk öğesi olarak belirtir. Kurtarma ağını `$RecoveryNetworks`ilk öğesi olarak belirtir.

   ```azurepowershell
   New-AzRecoveryServicesAsrNetworkMapping -PrimaryNetwork $PrimaryNetworks[0] -RecoveryNetwork $RecoveryNetworks[0]
   ```

## <a name="enable-protection-for-vms"></a>VM 'Ler için korumayı etkinleştirme

Sunucular, bulutlar ve ağlar doğru yapılandırıldıktan sonra buluttaki VM 'Ler için korumayı etkinleştirin.

1. Korumayı etkinleştirmek için, koruma kapsayıcısını almak üzere aşağıdaki komutu çalıştırın:

   ```azurepowershell
   $PrimaryProtectionContainer = Get-AzRecoveryServicesAsrProtectionContainer -FriendlyName $PrimaryCloudName
   ```

1. Koruma varlığını (VM) aşağıdaki gibi alın:

   ```azurepowershell
   $protectionEntity = Get-AzRecoveryServicesAsrProtectableItem -FriendlyName $VMName -ProtectionContainer $PrimaryProtectionContainer
   ```

1. VM için çoğaltmayı etkinleştirin.

   ```azurepowershell
   $jobResult = New-AzRecoveryServicesAsrReplicationProtectedItem -ProtectableItem $protectionentity -ProtectionContainerMapping $policy -VmmToVmm
   ```

> [!NOTE]
> Azure 'da CMK özellikli yönetilen disklere çoğaltmak istiyorsanız az PowerShell 3.3.0 onlıve sürümlerini kullanarak aşağıdaki adımları uygulayın:
>
> 1. VM özelliklerini güncelleştirerek yönetilen disklere yük devretmeyi etkinleştirme
> 1. Korunan öğenin her bir diski için disk KIMLIĞINI getirmek üzere `Get-AzRecoveryServicesAsrReplicationProtectedItem` cmdlet 'ini kullanın
> 1. Disk KIMLIĞININ disk şifreleme kümesine eşlenmesini içermesi için `New-Object "System.Collections.Generic.Dictionary``2[System.String,System.String]"` cmdlet 'ini kullanarak bir sözlük nesnesi oluşturun. Bu disk şifreleme kümelerinin hedef bölgede sizin tarafınızdan önceden oluşturulması gerekir.
> 1. **Diskidtodiskencryptionsetmap** parametresindeki sözlük nesnesini geçirerek `Set-AzRecoveryServicesAsrReplicationProtectedItem` cmdlet 'INI kullanarak VM özelliklerini güncelleştirin.

## <a name="run-a-test-failover"></a>Yük devretme testi çalıştırma

Dağıtımınızı test etmek için tek bir sanal makine için yük devretme testi çalıştırın. Ayrıca, birden çok VM içeren ve plan için yük devretme testi çalıştıran bir kurtarma planı da oluşturabilirsiniz. Yük devretme testi, yük devretme işleminizi ve kurtarma mekanizmanızı yalıtılmış bir ortamda benzetimli olarak gerçekleştirir.

1. VM 'Lerin yük devredebileceği VM 'yi alın.

   ```azurepowershell
   $Servers = Get-AzRecoveryServicesASRFabric
   $RecoveryNetworks = Get-AzRecoveryServicesAsrNetwork -Name $Servers[1]
   ```

1. Yük devretme testi gerçekleştirin.

   Tek bir VM için:

   ```azurepowershell
   $protectionEntity = Get-AzRecoveryServicesAsrProtectableItem -FriendlyName $VMName -ProtectionContainer $PrimaryprotectionContainer

   $jobIDResult = Start-AzRecoveryServicesAsrTestFailoverJob -Direction PrimaryToRecovery -ReplicationProtectedItem $protectionEntity -VMNetwork $RecoveryNetworks[1]
   ```

   Kurtarma planı için:

   ```azurepowershell
   $recoveryplanname = "test-recovery-plan"

   $recoveryplan = Get-AzRecoveryServicesAsrRecoveryPlan -FriendlyName $recoveryplanname

   $jobIDResult = Start-AzRecoveryServicesAsrTestFailoverJob -Direction PrimaryToRecovery -RecoveryPlan $recoveryplan -VMNetwork $RecoveryNetworks[1]
   ```

İşlemin tamamlandığını denetlemek için, [izleme etkinliğindeki](#monitor-activity)adımları izleyin.

## <a name="run-planned-and-unplanned-failovers"></a>Planlı ve planlanmamış yük devretme Çalıştır

1. Planlanmış bir yük devretme gerçekleştirin.

   Tek bir VM için:

   ```azurepowershell
   $protectionEntity = Get-AzRecoveryServicesAsrProtectableItem -Name $VMName -ProtectionContainer $PrimaryprotectionContainer

   $jobIDResult = Start-AzRecoveryServicesAsrPlannedFailoverJob -Direction PrimaryToRecovery -ReplicationProtectedItem $protectionEntity
   ```

   Kurtarma planı için:

   ```azurepowershell
   $recoveryplanname = "test-recovery-plan"

   $recoveryplan = Get-AzRecoveryServicesAsrRecoveryPlan -FriendlyName $recoveryplanname

   $jobIDResult = Start-AzRecoveryServicesAsrPlannedFailoverJob -Direction PrimaryToRecovery -RecoveryPlan $recoveryplan
   ```

1. Planlanmamış bir yük devretme gerçekleştirin.

   Tek bir VM için:

   ```azurepowershell
   $protectionEntity = Get-AzRecoveryServicesAsrProtectableItem -Name $VMName -ProtectionContainer $PrimaryprotectionContainer

   $jobIDResult = Start-AzRecoveryServicesAsrUnplannedFailoverJob -Direction PrimaryToRecovery -ReplicationProtectedItem $protectionEntity
   ```

   Kurtarma planı için:

   ```azurepowershell
   $recoveryplanname = "test-recovery-plan"

   $recoveryplan = Get-AzRecoveryServicesAsrRecoveryPlan -FriendlyName $recoveryplanname

   $jobIDResult = Start-AzRecoveryServicesAsrUnplannedFailoverJob -Direction PrimaryToRecovery -RecoveryPlan $recoveryplan
   ```

## <a name="monitor-activity"></a>Etkinliği izle

Yük devretme etkinliğini izlemek için aşağıdaki komutları kullanın. İşlemler arasında işlemenin bitmesini bekleyin.

```azurepowershell
Do
{
    $job = Get-AzRecoveryServicesAsrJob -TargetObjectId $associationJob.JobId;
    Write-Host "Job State:{0}, StateDescription:{1}" -f Job.State, $job.StateDescription;
    if($job -eq $null -or $job.StateDescription -ne "Completed")
    {
        $isJobLeftForProcessing = $true;
    }

if($isJobLeftForProcessing)
    {
        Start-Sleep -Seconds 60
    }
}While($isJobLeftForProcessing)
```

## <a name="next-steps"></a>Sonraki adımlar

Kaynak Yöneticisi PowerShell cmdlet 'leriyle Site Recovery hakkında [daha fazla bilgi edinin](/powershell/module/az.recoveryservices) .
