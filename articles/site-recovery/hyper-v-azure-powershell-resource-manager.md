---
title: Azure Site Kurtarma ve PowerShell kullanarak Hyper-V VM olağanüstü durum kurtarma
description: PowerShell ve Azure Kaynak Yöneticisi'ni kullanarak Azure Site Kurtarma hizmetiyle Hyper-V VM'lerin olağanüstü durum kurtarmasını Azure'a otomatikleştirin.
author: sujayt
manager: rochakm
ms.topic: article
ms.date: 01/10/2020
ms.author: sutalasi
ms.openlocfilehash: 6499c986bef965848303ee9833fd59f5e3f0889c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79257998"
---
# <a name="set-up-disaster-recovery-to-azure-for-hyper-v-vms-using-powershell-and-azure-resource-manager"></a>PowerShell ve Azure Kaynak Yöneticisi'ni kullanarak Hyper-V V MM'ler için Azure'da olağanüstü durum kurtarma ayarlama

[Azure Site Kurtarma,](site-recovery-overview.md) Azure sanal makinelerin (VM'ler) ve şirket içi Sanal M'lerin ve fiziksel sunucuların çoğaltılmasını, başarısızlığını ve kurtarılmasını düzenleyerek iş sürekliliğinize ve olağanüstü durum kurtarma (BCDR) stratejinize katkıda bulunur.

Bu makalede, Hyper-V V MM'leri Azure'da çoğaltmak için Azure Kaynak Yöneticisi ile birlikte Windows PowerShell'in nasıl kullanılacağı açıklanmaktadır. Bu makalede kullanılan örnek, Hyper-V ana bilgisayarda çalışan tek bir VM'yi Azure'a nasıl kopyalayabileceğinizi gösterir.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="azure-powershell"></a>Azure PowerShell

Azure PowerShell, Windows PowerShell'i kullanarak Azure'u yönetmek için cmdlet sağlar. Azure Kaynak Yöneticisi için Azure PowerShell ile kullanılabilen Site Kurtarma PowerShell cmdlets, Azure'da sunucularınızı korumanıza ve kurtarmanıza yardımcı olur.

Bu makaleyi kullanmak için PowerShell uzmanı olmanız gerekmez, ancak modüller, cmdlets ve oturumlar gibi temel kavramları anlamanız gerekir. Daha fazla bilgi için [PowerShell Dokümantasyonu'na](/powershell) bakın ve [Azure Kaynak Yöneticisi ile Azure PowerShell'i kullanma'yı kullanarak.](../powershell-azure-resource-manager.md)

> [!NOTE]
> Bulut Çözüm Sağlayıcısı (CSP) programındaki Microsoft iş ortakları, müşteri sunucularının korumasını ilgili CSP aboneliklerine (kiracı abonelikleri) göre yapılandırabilir ve yönetebilir.

## <a name="before-you-start"></a>Başlamadan önce

Şu ön koşulları yerine aldığınızdan emin olun:

- Bir [Microsoft Azure](https://azure.microsoft.com/) hesabı. [Ücretsiz deneme sürümüyle](https://azure.microsoft.com/pricing/free-trial/) başlayabilirsiniz. Ayrıca, Azure Site [Kurtarma Yöneticisi fiyatlandırması](https://azure.microsoft.com/pricing/details/site-recovery/)hakkında da bilgi edinebilirsiniz.
- Azure PowerShell. Bu sürüm ve nasıl yüklenir hakkında bilgi için Azure [PowerShell'i Yükle'ye](/powershell/azure/install-az-ps)bakın.

Buna ek olarak, bu makalede açıklanan özel örnek aşağıdaki ön koşullara sahiptir:

- Windows Server 2012 R2 veya Microsoft Hyper-V Server 2012 R2 çalıştıran bir Hyper-V ana bilgisayar, bir veya daha fazla VM içeren. Hyper-V sunucuları Internet'e doğrudan veya bir proxy aracılığıyla bağlanmalıdır.
- Çoğaltmak istediğiniz [VM'ler bu ön koşullara](hyper-v-azure-support-matrix.md#replicated-vms)uygun olmalıdır.

## <a name="step-1-sign-in-to-your-azure-account"></a>Adım 1: Azure hesabınızda oturum açın

1. Bir PowerShell konsolu açın ve Azure hesabınızda oturum açmak için bu komutu çalıştırın. Cmdlet bir web sayfası kadar getirir hesap kimlik `Connect-AzAccount`bilgileri için sorar: .
   - Alternatif olarak, **Kimlik Bilgileri** parametresini kullanarak `Connect-AzAccount` hesap kimlik bilgilerinizi cmdlet'e parametre olarak ekleyebilirsiniz.
   - Kiracı adına çalışan bir CSP ortağıysanız, kiracı kimliğini veya birincil etki alanı adını kullanarak müşteriyi kiracı olarak belirtin. Örneğin, `Connect-AzAccount -Tenant "fabrikam.com"`
1. Bir hesabın birden fazla aboneliği olabileceğinden, kullanmak istediğiniz aboneliği hesapla ilişkilendirin:

   ```azurepowershell
   Set-AzContext -Subscription $SubscriptionName
   ```

1. Aboneliğinizin Kurtarma Hizmetleri ve Site Kurtarma için Azure sağlayıcılarını kullanmak için kayıtlı olduğunu doğrulayın, bu komutları kullanarak:

   ```azurepowershell
   Get-AzResourceProvider -ProviderNamespace  Microsoft.RecoveryServices
   ```

1. Komut **çıktısında, RegistrationState'in** **Kayıtlı**olarak ayarladığını doğrulayın, Adım 2'ye geçebilirsiniz. Değilse, aşağıdaki komutları çalıştırarak eksik sağlayıcıyı aboneliğinizde kaydettirmelisiniz:

   ```azurepowershell
   Register-AzResourceProvider -ProviderNamespace Microsoft.RecoveryServices
   ```

1. Sağlayıcılar'ın aşağıdaki komutları kullanarak başarılı bir şekilde kaydolduğunu doğrulayın:

   ```azurepowershell
   Get-AzResourceProvider -ProviderNamespace  Microsoft.RecoveryServices
   ```

## <a name="step-2-set-up-the-vault"></a>Adım 2: Kasayı ayarlama

1. Kasa oluşturmak veya varolan bir kaynak grubunu kullanmak için bir Azure Kaynak Yöneticisi kaynak grubu oluşturun. Aşağıdaki gibi yeni bir kaynak grubu oluşturun. Değişken `$ResourceGroupName` oluşturmak istediğiniz kaynak grubunun adını içerir ve $Geo değişkeni kaynak grubunu oluşturmak için Azure bölgesini içerir (örneğin, "Brezilya Güney").

   ```azurepowershell
   New-AzResourceGroup -Name $ResourceGroupName -Location $Geo
   ```

1. Aboneliğinizdeki kaynak gruplarının listesini almak için `Get-AzResourceGroup` cmdlet'i çalıştırın.
1. Aşağıdaki gibi yeni bir Azure Kurtarma Hizmetleri kasası oluşturun:

   ```azurepowershell
   $vault = New-AzRecoveryServicesVault -Name <string> -ResourceGroupName <string> -Location <string>
   ```

`Get-AzRecoveryServicesVault` Cmdlet ile mevcut kasaların listesini alabilirsiniz.

## <a name="step-3-set-the-recovery-services-vault-context"></a>Adım 3: Kurtarma Hizmetleri kasa bağlamını ayarlama

Kasa bağlamını aşağıdaki gibi ayarlayın:

```azurepowershell
Set-AzRecoveryServicesAsrVaultContext -Vault $vault
```

## <a name="step-4-create-a-hyper-v-site"></a>Adım 4: Hyper-V sitesi oluşturma

1. Aşağıdaki gibi yeni bir Hyper-V sitesi oluşturun:

   ```azurepowershell
   $sitename = "MySite"                #Specify site friendly name
   New-AzRecoveryServicesAsrFabric -Type HyperVSite -Name $sitename
   ```

1. Bu cmdlet, siteyi oluşturmak için bir Site Kurtarma işi başlatır ve bir Site Kurtarma iş nesnesini döndürür. İşin tamamlanmasını bekleyin ve işin başarıyla tamamlandığını doğrulayın.
1. İş `Get-AzRecoveryServicesAsrJob` nesnesini almak için cmdlet'i kullanın ve işin geçerli durumunu denetleyin.
1. Aşağıdaki gibi, site için bir kayıt anahtarı oluşturun ve indirin:

   ```azurepowershell
   $SiteIdentifier = Get-AzRecoveryServicesAsrFabric -Name $sitename | Select-Object -ExpandProperty SiteIdentifier
   $path = Get-AzRecoveryServicesVaultSettingsFile -Vault $vault -SiteIdentifier $SiteIdentifier -SiteFriendlyName $sitename
   ```

1. İndirilen anahtarı Hyper-V ana bilgisayarına kopyalayın. Hyper-V ana bilgisayarını siteye kaydetmek için anahtara ihtiyacınız var.

## <a name="step-5-install-the-provider-and-agent"></a>Adım 5: Sağlayıcıyı ve aracıyı yükleyin

1. [Sağlayıcının](https://aka.ms/downloaddra)en son sürümü için yükleyiyi Microsoft'tan indirin.
1. Yükleyiciyi Hyper-V ana bilgisayarda çalıştırın.
1. Yükleme sonunda kayıt adımı devam edin.
1. İstendiğinde, indirilen anahtarı sağlayın ve Hyper-V ana bilgisayarının tam kaydını sağlayın.
1. Hyper-V ana bilgisayar sitesinin siteye aşağıdaki şekilde kayıtlı olduğunu doğrulayın:

   ```azurepowershell
   $server = Get-AzRecoveryServicesAsrFabric -Name $siteName | Get-AzRecoveryServicesAsrServicesProvider -FriendlyName $server-friendlyname
   ```

Hyper-V çekirdek sunucusu çalıştırıyorsanız, kurulum dosyasını indirin ve aşağıdaki adımları izleyin:

1. Bu komutu çalıştırarak dosyaları _AzureSiteRecoveryProvider.exe'den_ yerel bir dizine ayıklayın:

   ```console
   AzureSiteRecoveryProvider.exe /x:. /q
   ```

1. Şu komutu çalıştırın:

   ```console
   .\setupdr.exe /i
   ```

   Sonuçlar _%ProgramData%\ASRLogs\DRASetupWizard.log'a_kaydedilir.

1. Bu komutu çalıştırarak sunucuyu kaydedin:

   ```console
   cd  C:\Program Files\Microsoft Azure Site Recovery Provider\DRConfigurator.exe" /r /Friendlyname "FriendlyName of the Server" /Credentials "path to where the credential file is saved"
   ```

## <a name="step-6-create-a-replication-policy"></a>Adım 6: Çoğaltma ilkesi oluşturma

Başlamadan önce, belirtilen depolama hesabı kasayla aynı Azure bölgesinde olmalı ve coğrafi çoğaltma etkinleştirilmiş olmalıdır.

1. Aşağıdaki gibi bir çoğaltma ilkesi oluşturun:

   ```azurepowershell
   $ReplicationFrequencyInSeconds = "300";        #options are 30,300,900
   $PolicyName = “replicapolicy”
   $Recoverypoints = 6                    #specify the number of recovery points
   $storageaccountID = Get-AzStorageAccount -Name "mystorea" -ResourceGroupName "MyRG" | Select-Object -ExpandProperty Id

   $PolicyResult = New-AzRecoveryServicesAsrPolicy -Name $PolicyName -ReplicationProvider “HyperVReplicaAzure” -ReplicationFrequencyInSeconds $ReplicationFrequencyInSeconds -NumberOfRecoveryPointsToRetain $Recoverypoints -ApplicationConsistentSnapshotFrequencyInHours 1 -RecoveryAzureStorageAccountId $storageaccountID
   ```

1. Çoğaltma ilkesi oluşturmanın başarılı olduğundan emin olmak için döndürülen işi denetleyin.

1. Siteye karşılık gelen koruma konteynerini aşağıdaki gibi alın:

   ```azurepowershell
   $protectionContainer = Get-AzRecoveryServicesAsrProtectionContainer
   ```

1. Koruma kapsayıcısını aşağıdaki gibi çoğaltma ilkesiyle ilişkilendirin:

   ```azurepowershell
   $Policy = Get-AzRecoveryServicesAsrPolicy -FriendlyName $PolicyName
   $associationJob = New-AzRecoveryServicesAsrProtectionContainerMapping -Name $mappingName -Policy $Policy -PrimaryProtectionContainer $protectionContainer[0]
   ```

1. İlişkilendirme işinin başarıyla tamamlanmasını bekleyin.

1. Koruma kabı eşlemi alın.

   ```azurepowershell
   $ProtectionContainerMapping = Get-AzRecoveryServicesAsrProtectionContainerMapping -ProtectionContainer $protectionContainer
   ```

## <a name="step-7-enable-vm-protection"></a>Adım 7: VM korumasını etkinleştirin

1. Korumak istediğiniz VM'ye karşılık gelen korunabilir öğeyi aşağıdaki gibi alın:

   ```azurepowershell
   $VMFriendlyName = "Fabrikam-app"          #Name of the VM
   $ProtectableItem = Get-AzRecoveryServicesAsrProtectableItem -ProtectionContainer $protectionContainer -FriendlyName $VMFriendlyName
   ```

1. VM'yi koruyun. Koruduğunuz VM'de birden fazla disk varsa, **OSDiskName** parametresini kullanarak işletim sistemi diskini belirtin.

   ```azurepowershell
   $OSType = "Windows"          # "Windows" or "Linux"
   $DRjob = New-AzRecoveryServicesAsrReplicationProtectedItem -ProtectableItem $VM -Name $VM.Name -ProtectionContainerMapping $ProtectionContainerMapping -RecoveryAzureStorageAccountId $StorageAccountID -OSDiskName $OSDiskNameList[$i] -OS $OSType -RecoveryResourceGroupId $ResourceGroupID
   ```

1. İlk çoğaltmadan sonra VM'lerin korumalı bir duruma ulaşmasını bekleyin. Bu, çoğaltılacak veri miktarı ve Azure'a sunulan yukarı bant genişliği gibi etkenlere bağlı olarak biraz zaman alabilir. Korunan bir durum yerinde olduğunda, iş Durumu ve StateDescription aşağıdaki gibi güncelleştirilir:

   ```console
   PS C:\> $DRjob = Get-AzRecoveryServicesAsrJob -Job $DRjob

   PS C:\> $DRjob | Select-Object -ExpandProperty State
   Succeeded

   PS C:\> $DRjob | Select-Object -ExpandProperty StateDescription
   Completed
   ```

1. Kurtarma özelliklerini (VM rol boyutu gibi) ve başarısız olduktan sonra VM NIC'yi takacak ları Azure ağını güncelleştirin.

   ```console
   PS C:\> $nw1 = Get-AzVirtualNetwork -Name "FailoverNw" -ResourceGroupName "MyRG"

   PS C:\> $VMFriendlyName = "Fabrikam-App"

   PS C:\> $rpi = Get-AzRecoveryServicesAsrReplicationProtectedItem -ProtectionContainer $protectionContainer -FriendlyName $VMFriendlyName

   PS C:\> $UpdateJob = Set-AzRecoveryServicesAsrReplicationProtectedItem -InputObject $rpi -PrimaryNic $VM.NicDetailsList[0].NicId -RecoveryNetworkId $nw1.Id -RecoveryNicSubnetName $nw1.Subnets[0].Name

   PS C:\> $UpdateJob = Get-AzRecoveryServicesAsrJob -Job $UpdateJob

   PS C:\> $UpdateJob | Select-Object -ExpandProperty state

   PS C:\> Get-AzRecoveryServicesAsrJob -Job $job | Select-Object -ExpandProperty state

   Succeeded
   ```

> [!NOTE]
> Azure'da CMK etkin yönetilen disklere çoğaltmak istiyorsanız, Az PowerShell 3.3.0'ı kullanarak aşağıdaki adımları yapın:
>
> 1. VM özelliklerini güncelleştirerek yönetilen disklere başarısız olmasını etkinleştirme
> 1. Korunan `Get-AzRecoveryServicesAsrReplicationProtectedItem` öğenin her diski için disk kimliğini almak için cmdlet'i kullanın
> 1. Disk şifreleme kümesine disk kimliğinin eşlemesini içeren cmdlet kullanarak `New-Object "System.Collections.Generic.Dictionary``2[System.String,System.String]"` bir sözlük nesnesi oluşturun. Bu disk şifreleme kümeleri, hedef bölgede sizin yeriniz tarafından önceden oluşturulacaktır.
> 1. `Set-AzRecoveryServicesAsrReplicationProtectedItem` **DiskIdToDiskEncryptionSetMap** parametresinde sözlük nesnesini geçirerek cmdlet kullanarak VM özelliklerini güncelleştirin.

## <a name="step-8-run-a-test-failover"></a>Adım 8: Bir test başarısızlığını çalıştırma

1. Aşağıdaki gibi bir test failover çalıştırın:

   ```azurepowershell
   $nw = Get-AzVirtualNetwork -Name "TestFailoverNw" -ResourceGroupName "MyRG" #Specify Azure vnet name and resource group

   $rpi = Get-AzRecoveryServicesAsrReplicationProtectedItem -ProtectionContainer $protectionContainer -FriendlyName $VMFriendlyName

   $TFjob = Start-AzRecoveryServicesAsrTestFailoverJob -ReplicationProtectedItem $VM -Direction PrimaryToRecovery -AzureVMNetworkId $nw.Id
   ```

1. Test VM'sinin Azure'da oluşturulduğunu doğrulayın. Azure'da test VM'i oluşturulduktan sonra test başarısız olması işi askıya alınır.
1. Test başarısızlığını temizlemek ve tamamlamak için aşağıdakileri çalıştırın:

   ```azurepowershell
   $TFjob = Start-AzRecoveryServicesAsrTestFailoverCleanupJob -ReplicationProtectedItem $rpi -Comment "TFO done"
   ```

## <a name="next-steps"></a>Sonraki adımlar

Azure Kaynak Yöneticisi PowerShell cmdlets ile Azure Site Kurtarma hakkında [daha fazla bilgi edinin.](/powershell/module/az.recoveryservices)
