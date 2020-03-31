---
title: Hyper-V (VMM ile) olağanüstü durum kurtarmayı Azure Site Kurtarma/PowerShell ile ikincil bir siteye ayarlama
description: Azure Site Kurtarma ve PowerShell kullanarak VMM bulutlarında Hyper-V VM'lerin ikincil bir VMM sitesine nasıl ayarlanın gerektiğini açıklar.
services: site-recovery
author: sujayt
manager: rochakm
ms.topic: article
ms.date: 1/10/2020
ms.author: sutalasi
ms.openlocfilehash: deef7bfdbc28d744cb81da59d3ffc13a1abee54d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77048609"
---
# <a name="set-up-disaster-recovery-of-hyper-v-vms-to-a-secondary-site-by-using-powershell-resource-manager"></a>PowerShell (Kaynak Yöneticisi) kullanarak Hyper-V VM'lerin ikincil bir siteye olağanüstü durum kurtarmasını ayarlama

Bu makalede, System Center Virtual Machine Manager bulutlarında Hyper-V VM'lerin çoğaltılması için [adımların Azure Site Kurtarma'yı](site-recovery-overview.md)kullanarak ikincil bir şirket içi sitede Sanal Makine Yöneticisi bulutuna otomatikleştirilen adımlar gösterilmektedir.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Ön koşullar

- [Senaryo mimarisini ve bileşenlerini](hyper-v-vmm-architecture.md) gözden geçirin.
- Tüm bileşenler için [destek gereksinimlerini](site-recovery-support-matrix-to-sec-site.md) gözden geçirin.
- Virtual Machine Manager sunucuları ve Hyper-V ana bilgisayarları [destek gereksinimlerine](site-recovery-support-matrix-to-sec-site.md)uyduklarından emin olun.
- Çoğaltmak istediğiniz VM'lerin [çoğaltılan makine desteğine](site-recovery-support-matrix-to-sec-site.md)uygun olup olmadığını denetleyin.

## <a name="prepare-for-network-mapping"></a>Ağ eşlemesi için hazırlanma

Kaynak ve hedef bulutlarda şirket içi Sanal Makine Yöneticisi VM ağları arasındaki [ağ eşleme haritaları.](hyper-v-vmm-network-mapping.md) Eşleme sürecinde şu işlemler gerçekleştirilir:

- VM'ler yük devretme sonrasında uygun hedef VM ağlara bağlanır.
- Çoğaltma VM'lerini en uygun şekilde hedef Hyper-V konağı sunucularına yerleştirir.
- Ağ eşlemesi yapılandırmazsanız, çoğaltma VM'leri başarısız olduktan sonra bir VM ağına bağlanmaz.

Sanal Makine Yöneticisi'ni aşağıdaki gibi hazırlayın:

- Kaynak ve hedef Sanal Makine Yöneticisi sunucularında [Sanal Makine Yöneticisi mantıksal ağları](https://docs.microsoft.com/system-center/vmm/network-logical) olduğundan emin olun:
  - Kaynak sunucusundaki mantıksal ağ, Hyper-V konaklarının bulunduğu kaynak bulutla ilişkilendirilmelidir.
  - Hedef sunucudaki mantıksal ağ, hedef bulutla ilişkilendirilmelidir.
- Kaynakta [VM ağları](https://docs.microsoft.com/system-center/vmm/network-virtual) olduğundan emin olun ve Virtual Machine Manager sunucularını hedefleyin. VM ağları, her bir konumdaki mantıksal ağa bağlanmış olmalıdır.
- Kaynak Hyper-V konaklarındaki VM'leri kaynak VM ağına bağlayın.

## <a name="prepare-for-powershell"></a>PowerShell'e hazırlanın

Azure PowerShell'in hazır olduğundan emin olun:

- PowerShell'i zaten kullanıyorsanız, 0.8.10 veya sonraki sürümlere yükseltin. [Learn more](/powershell/azureps-cmdlets-docs) PowerShell'i nasıl kurup kurmayı zedeleyin.
- PowerShell'i kurduktan ve yapılandırdıktan sonra [hizmet cmdlets'i](/powershell/azure/overview)gözden geçirin.
- PowerShell'de parametre değerlerini, girişleri ve çıktıları nasıl kullanacağıhakkında daha fazla bilgi edinmek için [Başlat](/powershell/azure/get-started-azureps) kılavuzunu okuyun.

## <a name="set-up-a-subscription"></a>Abonelik ayarlama

1. PowerShell'den Azure hesabınızda oturum açın.

   ```azurepowershell
   $UserName = "<user@live.com>"
   $Password = "<password>"
   $SecurePassword = ConvertTo-SecureString -AsPlainText $Password -Force
   $Cred = New-Object System.Management.Automation.PSCredential -ArgumentList $UserName, $SecurePassword
   Connect-AzAccount #-Credential $Cred
   ```

1. Abonelik t.c. ile aboneliklerinizin listesini alın. Kurtarma Hizmetleri kasasını oluşturmak istediğiniz abonelik kimliğine dikkat edin.

   ```azurepowershell
   Get-AzSubscription
   ```

1. Kasanın aboneliğini ayarlayın.

   ```azurepowershell
   Set-AzContext –SubscriptionID <subscriptionId>
   ```

## <a name="create-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasası oluşturma

1. Yoksa bir Azure Kaynak Yöneticisi kaynak grubu oluşturun.

   ```azurepowershell
   New-AzResourceGroup -Name #ResourceGroupName -Location #location
   ```

1. Yeni bir Kurtarma Hizmetleri kasası oluşturun. Kasa nesnesini daha sonra kullanılacak bir değişkene kaydedin.

   ```azurepowershell
   $vault = New-AzRecoveryServicesVault -Name #vaultname -ResourceGroupName #ResourceGroupName -Location #location
   ```

   Cmdlet kullanarak oluşturduktan sonra kasa `Get-AzRecoveryServicesVault` nesnesini alabilirsiniz.

## <a name="set-the-vault-context"></a>Kasa bağlamını ayarlama

1. Varolan bir kasayı alın.

   ```azurepowershell
   $vault = Get-AzRecoveryServicesVault -Name #vaultname
   ```

1. Kasa bağlamını ayarlayın.

   ```azurepowershell
   Set-AzRecoveryServicesAsrVaultContext -Vault $vault
   ```

## <a name="install-the-site-recovery-provider"></a>Site Kurtarma sağlayıcısını yükleme

1. Sanal Makine Yöneticisi makinesinde, aşağıdaki komutu çalıştırarak bir dizin oluşturun:

   ```azurepowershell
   New-Item -Path C:\ASR -ItemType Directory
   ```

1. İndirilen sağlayıcı kurulum dosyasını kullanarak dosyaları ayıklayın.

   ```console
   pushd C:\ASR\
   .\AzureSiteRecoveryProvider.exe /x:. /q
   ```

1. Sağlayıcıyı yükleyin ve yüklemenin tamamlanmasını bekleyin.

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

1. Hyper-V 2012 R2 için bu durumda aşağıdaki gibi bir çoğaltma ilkesi oluşturun:

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
   > Sanal Makine Yöneticisi bulutu, Windows Server'ın farklı sürümlerini çalıştıran Hyper-V ana bilgisayarlarını içerebilir, ancak çoğaltma ilkesi işletim sisteminin belirli bir sürümü içindir. Farklı işletim sistemlerinde çalışan farklı ana bilgisayarlar varsa, her sistem için ayrı çoğaltma ilkeleri oluşturun. Örneğin, Windows Server 2012'de çalışan beş ana bilgisayar ve Windows Server 2012 R2'de çalışan üç ana bilgisayar varsa, iki çoğaltma ilkeleri oluşturun. Her işletim sistemi türü için bir tane oluşturursunuz.

1. Birincil koruma kapsayıcısını (birincil Sanal Makine Yöneticisi bulutu) ve kurtarma koruma kapsayıcısını (kurtarma Sanal Makine Yöneticisi bulutu) alın.

   ```azurepowershell
   $PrimaryCloud = "testprimarycloud"
   $primaryprotectionContainer = Get-AzRecoveryServicesAsrProtectionContainer -FriendlyName $PrimaryCloud;

   $RecoveryCloud = "testrecoverycloud"
   $recoveryprotectionContainer = Get-AzRecoveryServicesAsrProtectionContainer -FriendlyName $RecoveryCloud;
   ```

1. Dost adı kullanarak oluşturduğunuz çoğaltma ilkesini alın.

   ```azurepowershell
   $policy = Get-AzRecoveryServicesAsrPolicy -FriendlyName $policyname
   ```

1. Koruma kapsayıcısının (Virtual Machine Manager cloud) çoğaltma ilkesiyle ilişkisini başlatın.

   ```azurepowershell
   $associationJob  = New-AzRecoveryServicesAsrProtectionContainerMapping -Policy $Policy -PrimaryProtectionContainer $primaryprotectionContainer -RecoveryProtectionContainer $recoveryprotectionContainer
   ```

1. İlke ilişkilendirme işinin tamamlanmasını bekleyin. İşin bitip bitmeden tamamlanamasını kontrol etmek için aşağıdaki PowerShell parçacıklarını kullanın:

   ```azurepowershell
   $job = Get-AzRecoveryServicesAsrJob -Job $associationJob

   if($job -eq $null -or $job.StateDescription -ne "Completed")
   {
     $isJobLeftForProcessing = $true;
   }
   ```

1. İş işleme bittikten sonra aşağıdaki komutu çalıştırın:

   ```azurepowershell
   if($isJobLeftForProcessing)
   {
     Start-Sleep -Seconds 60
   }
   While($isJobLeftForProcessing)
   ```

İşlemin tamamlanmasını denetlemek [için, İzleme etkinliğindeki](#monitor-activity)adımları izleyin.

##  <a name="configure-network-mapping"></a>Ağ eşlemesini yapılandırma

1. Geçerli kasanın sunucularını almak için bu komutu kullanın. Komut, Site Kurtarma sunucularını `$Servers` dizi değişkeninde saklar.

   ```azurepowershell
   $Servers = Get-AzRecoveryServicesAsrFabric
   ```

1. Kaynak Virtual Machine Manager sunucusu ve hedef Virtual Machine Manager sunucusu için ağları almak için bu komutu çalıştırın.

   ```azurepowershell
   $PrimaryNetworks = Get-AzRecoveryServicesAsrNetwork -Fabric $Servers[0]

   $RecoveryNetworks = Get-AzRecoveryServicesAsrNetwork -Fabric $Servers[1]
   ```

   > [!NOTE]
   > Kaynak Virtual Machine Manager sunucusu sunucu dizisinde birinci veya ikinci olabilir. Virtual Machine Manager sunucu adlarını denetleyin ve ağları uygun şekilde alın.

1. Bu cmdlet birincil ağ ve kurtarma ağı arasında bir eşleme oluşturur. Birincil ağı ilk öğe olarak `$PrimaryNetworks`belirtir. Kurtarma ağını ilk öğe olarak `$RecoveryNetworks`belirtir.

   ```azurepowershell
   New-AzRecoveryServicesAsrNetworkMapping -PrimaryNetwork $PrimaryNetworks[0] -RecoveryNetwork $RecoveryNetworks[0]
   ```

## <a name="enable-protection-for-vms"></a>VM'ler için koruma sağlama

Sunucular, bulutlar ve ağlar doğru şekilde yapılandırıldıktan sonra buluttaki VM'ler için koruma sağlar.

1. Korumayı etkinleştirmek için, koruma kapsayıcısını almak için aşağıdaki komutu çalıştırın:

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
> Azure'da CMK etkin yönetilen disklere çoğaltmak istiyorsanız, Az PowerShell 3.3.0'ı kullanarak aşağıdaki adımları yapın:
>
> 1. VM özelliklerini güncelleştirerek yönetilen disklere başarısız olmasını etkinleştirme
> 1. Korunan `Get-AzRecoveryServicesAsrReplicationProtectedItem` öğenin her diski için disk kimliğini almak için cmdlet'i kullanın
> 1. Disk şifreleme kümesine disk kimliğinin eşlemesini içeren cmdlet kullanarak `New-Object "System.Collections.Generic.Dictionary``2[System.String,System.String]"` bir sözlük nesnesi oluşturun. Bu disk şifreleme kümeleri, hedef bölgede sizin yeriniz tarafından önceden oluşturulacaktır.
> 1. `Set-AzRecoveryServicesAsrReplicationProtectedItem` **DiskIdToDiskEncryptionSetMap** parametresinde sözlük nesnesini geçirerek cmdlet kullanarak VM özelliklerini güncelleştirin.

## <a name="run-a-test-failover"></a>Yük devretme testi çalıştırma

Dağıtımınızı test etmek için tek bir sanal makine için bir test başarısızlığını çalıştırın. Ayrıca, birden çok VM içeren bir kurtarma planı oluşturabilir ve plan için bir test başarısızlığını çalıştırabilirsiniz. Yük devretme testi, yük devretme işleminizi ve kurtarma mekanizmanızı yalıtılmış bir ortamda benzetimli olarak gerçekleştirir.

1. VM'lerin başarısız olacağı VM'yi alın.

   ```azurepowershell
   $Servers = Get-AzRecoveryServicesASRFabric
   $RecoveryNetworks = Get-AzRecoveryServicesAsrNetwork -Name $Servers[1]
   ```

1. Bir test başarısız gerçekleştirin.

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

İşlemin tamamlanmasını denetlemek [için, İzleme etkinliğindeki](#monitor-activity)adımları izleyin.

## <a name="run-planned-and-unplanned-failovers"></a>Planlı ve planlanmamış arızaları çalıştırma

1. Planlı bir başarısızlık gerçekleştirin.

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

1. Planlanmamış bir başarısızlık gerçekleştirin.

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

## <a name="monitor-activity"></a>Etkinliği izleme

Failover etkinliğini izlemek için aşağıdaki komutları kullanın. İşler arasında işlemin bitmesini bekleyin.

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

Kaynak Yöneticisi PowerShell cmdlets ile Site Kurtarma hakkında [daha fazla bilgi edinin.](/powershell/module/az.recoveryservices)
