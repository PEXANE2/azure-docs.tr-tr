---
title: Azure Site Recovery ve PowerShell kullanarak Hyper-V VM olağanüstü durum kurtarma
description: PowerShell ve Azure Resource Manager kullanarak Hyper-V VM 'lerinin olağanüstü durum kurtarma işlemini Azure Site Recovery hizmetiyle Azure 'a otomatikleştirin.
author: sujayt
manager: rochakm
ms.topic: article
ms.date: 01/10/2020
ms.author: sutalasi
ms.openlocfilehash: 6499c986bef965848303ee9833fd59f5e3f0889c
ms.sourcegitcommit: 984c5b53851be35c7c3148dcd4dfd2a93cebe49f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/28/2020
ms.locfileid: "76773427"
---
# <a name="set-up-disaster-recovery-to-azure-for-hyper-v-vms-using-powershell-and-azure-resource-manager"></a>PowerShell ve Azure Resource Manager kullanarak Hyper-V VM 'Leri için Azure 'da olağanüstü durum kurtarmayı ayarlama

[Azure Site Recovery](site-recovery-overview.md) , Azure sanal makinelerini (VM) ve şirket Içi VM 'leri ve fiziksel sunucuları çoğaltma, yük devretme ve kurtarma işlemlerini düzenleyerek iş sürekliliği ve olağanüstü durum kurtarma (BCDR) stratejinize katkıda bulunur.

Bu makalede, Hyper-V VM 'lerini Azure 'a çoğaltmak için Azure Resource Manager ile birlikte Windows PowerShell 'in nasıl kullanılacağı açıklanır. Bu makalede kullanılan örnek, Hyper-V ana bilgisayarında çalışan tek bir VM 'yi Azure 'a nasıl çoğaltacağınızı gösterir.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="azure-powershell"></a>Azure PowerShell

Azure PowerShell, Windows PowerShell kullanarak Azure 'ı yönetmek için cmdlet 'ler sağlar. Azure Resource Manager için Azure PowerShell kullanılabilen PowerShell cmdlet 'leri Site Recovery Azure 'da sunucularınızı korumanıza ve kurtarmanıza yardımcı olur.

Bu makaleyi kullanmak için PowerShell uzmanı olmanız gerekmez, ancak modüller, cmdlet 'ler ve oturumlar gibi temel kavramları anlamanız gerekir. Daha fazla bilgi için bkz. [PowerShell belgeleri](/powershell) ve [Azure Resource Manager Azure PowerShell kullanımı](../powershell-azure-resource-manager.md).

> [!NOTE]
> Bulut çözümü sağlayıcısı (CSP) programındaki Microsoft iş ortakları, müşteri sunucularının korumasını kendi CSP aboneliklerine (kiracı abonelikleri) yapılandırabilir ve yönetebilir.

## <a name="before-you-start"></a>Başlamadan önce

Bu önkoşulların yerinde olduğundan emin olun:

- [Microsoft Azure](https://azure.microsoft.com/) hesabı. [Ücretsiz deneme sürümüyle](https://azure.microsoft.com/pricing/free-trial/) başlayabilirsiniz. Ayrıca, [Azure Site Recovery Manager fiyatlandırması](https://azure.microsoft.com/pricing/details/site-recovery/)hakkında bilgi edinebilirsiniz.
- Azure PowerShell. Bu sürüm ve nasıl yükleneceğine ilişkin bilgiler için bkz. [ınstall Azure PowerShell](/powershell/azure/install-az-ps).

Ayrıca, bu makalede açıklanan belirli örnek aşağıdaki önkoşullara sahiptir:

- Bir veya daha fazla VM içeren Windows Server 2012 R2 veya Microsoft Hyper-V Server 2012 R2 çalıştıran Hyper-V konağı. Hyper-V sunucuları doğrudan veya bir ara sunucu üzerinden Internet 'e bağlanmalıdır.
- Çoğaltmak istediğiniz VM 'Ler [Bu önkoşullara](hyper-v-azure-support-matrix.md#replicated-vms)uymalıdır.

## <a name="step-1-sign-in-to-your-azure-account"></a>1\. Adım: Azure hesabınızda oturum açın

1. Bir PowerShell konsolu açın ve Azure hesabınızda oturum açmak için bu komutu çalıştırın. Cmdlet 'i bir Web sayfası getirir, sizden hesap kimlik bilgilerinizi ister: `Connect-AzAccount`.
   - Alternatif olarak, **kimlik bilgisi** parametresini kullanarak hesap kimlik bilgilerinizi `Connect-AzAccount` cmdlet 'ine bir parametre olarak ekleyebilirsiniz.
   - Bir kiracı adına çalışan bir CSP iş ortağıysanız, istemci Tenantıd veya kiracı birincil etki alanı adını kullanarak müşteriyi kiracı olarak belirtin. Örneğin, `Connect-AzAccount -Tenant "fabrikam.com"`
1. Hesap birden çok aboneliğe sahip olduğundan, hesapla birlikte kullanmak istediğiniz aboneliği ilişkilendirin:

   ```azurepowershell
   Set-AzContext -Subscription $SubscriptionName
   ```

1. Aşağıdaki komutları kullanarak, aboneliğinizin kurtarma hizmetleri ve Site Recovery için Azure sağlayıcılarını kullanmak üzere kaydedildiğini doğrulayın:

   ```azurepowershell
   Get-AzResourceProvider -ProviderNamespace  Microsoft.RecoveryServices
   ```

1. Komut çıkışında, **Registrationstate** ' in **kayıtlı**olarak ayarlandığını doğrulayın. Adım 2 ' ye geçebilirsiniz. Aksi takdirde, aşağıdaki komutları çalıştırarak, eksik sağlayıcıyı aboneliğinize kaydetmeniz gerekir:

   ```azurepowershell
   Register-AzResourceProvider -ProviderNamespace Microsoft.RecoveryServices
   ```

1. Aşağıdaki komutları kullanarak sağlayıcıların başarıyla kaydedildiğini doğrulayın:

   ```azurepowershell
   Get-AzResourceProvider -ProviderNamespace  Microsoft.RecoveryServices
   ```

## <a name="step-2-set-up-the-vault"></a>2\. Adım: kasayı ayarlama

1. Kasanın oluşturulacağı bir Azure Resource Manager kaynak grubu oluşturun veya var olan bir kaynak grubunu kullanın. Yeni bir kaynak grubunu aşağıda gösterildiği gibi oluşturun. `$ResourceGroupName` değişkeni, oluşturmak istediğiniz kaynak grubunun adını içerir ve $Geo değişkeni, kaynak grubunun oluşturulacağı Azure bölgesini içerir (örneğin, "Brezilya Güney").

   ```azurepowershell
   New-AzResourceGroup -Name $ResourceGroupName -Location $Geo
   ```

1. Aboneliğinizdeki kaynak gruplarının bir listesini almak için `Get-AzResourceGroup` cmdlet 'ini çalıştırın.
1. Yeni bir Azure kurtarma hizmetleri Kasası oluşturmak için aşağıdaki adımları izleyin:

   ```azurepowershell
   $vault = New-AzRecoveryServicesVault -Name <string> -ResourceGroupName <string> -Location <string>
   ```

`Get-AzRecoveryServicesVault` cmdlet 'ini kullanarak mevcut kasaların bir listesini alabilirsiniz.

## <a name="step-3-set-the-recovery-services-vault-context"></a>3\. Adım: kurtarma hizmetleri Kasası bağlamını ayarlama

Kasa bağlamını aşağıdaki şekilde ayarlayın:

```azurepowershell
Set-AzRecoveryServicesAsrVaultContext -Vault $vault
```

## <a name="step-4-create-a-hyper-v-site"></a>4\. Adım: Hyper-V sitesi oluşturma

1. Yeni bir Hyper-V sitesini şu şekilde oluşturun:

   ```azurepowershell
   $sitename = "MySite"                #Specify site friendly name
   New-AzRecoveryServicesAsrFabric -Type HyperVSite -Name $sitename
   ```

1. Bu cmdlet, siteyi oluşturmak için bir Site Recovery işi başlatır ve bir Site Recovery iş nesnesi döndürür. İşin tamamlanmasını bekleyin ve işin başarıyla tamamlandığını doğrulayın.
1. İş nesnesini almak için `Get-AzRecoveryServicesAsrJob` cmdlet 'ini kullanın ve işin geçerli durumunu kontrol edin.
1. Site için bir kayıt anahtarı oluşturun ve şu şekilde indirin:

   ```azurepowershell
   $SiteIdentifier = Get-AzRecoveryServicesAsrFabric -Name $sitename | Select-Object -ExpandProperty SiteIdentifier
   $path = Get-AzRecoveryServicesVaultSettingsFile -Vault $vault -SiteIdentifier $SiteIdentifier -SiteFriendlyName $sitename
   ```

1. İndirilen anahtarı Hyper-V konağına kopyalayın. Hyper-V konağını siteye kaydetmek için anahtar gereklidir.

## <a name="step-5-install-the-provider-and-agent"></a>5\. Adım: sağlayıcıyı ve aracıyı yükler

1. [Microsoft](https://aka.ms/downloaddra)'tan sağlayıcının en son sürümüne yönelik yükleyiciyi indirin.
1. Yükleyiciyi Hyper-V konağında çalıştırın.
1. Yüklemenin sonunda kayıt adımına devam edin.
1. İstendiğinde, indirilen anahtarı sağlayın ve Hyper-V konağının kaydını yapın.
1. Hyper-V konağının siteye şu şekilde kaydedildiğini doğrulayın:

   ```azurepowershell
   $server = Get-AzRecoveryServicesAsrFabric -Name $siteName | Get-AzRecoveryServicesAsrServicesProvider -FriendlyName $server-friendlyname
   ```

Hyper-V çekirdek sunucusu çalıştırıyorsanız, kurulum dosyasını indirin ve şu adımları izleyin:

1. Şu komutu çalıştırarak dosyaları _AzureSiteRecoveryProvider. exe_ ' den yerel bir dizine ayıklayın:

   ```console
   AzureSiteRecoveryProvider.exe /x:. /q
   ```

1. Şu komutu çalıştırın:

   ```console
   .\setupdr.exe /i
   ```

   Sonuçlar _%ProgramData%\asrlogs\drasetupwizard.log_dosyasına kaydedilir.

1. Şu komutu çalıştırarak sunucuyu kaydedin:

   ```console
   cd  C:\Program Files\Microsoft Azure Site Recovery Provider\DRConfigurator.exe" /r /Friendlyname "FriendlyName of the Server" /Credentials "path to where the credential file is saved"
   ```

## <a name="step-6-create-a-replication-policy"></a>6\. Adım: çoğaltma ilkesi oluşturma

Başlamadan önce, belirtilen depolama hesabı kasayla aynı Azure bölgesinde olmalıdır ve coğrafi çoğaltmanın etkin olması gerekir.

1. Çoğaltma ilkesini aşağıdaki gibi oluşturun:

   ```azurepowershell
   $ReplicationFrequencyInSeconds = "300";        #options are 30,300,900
   $PolicyName = “replicapolicy”
   $Recoverypoints = 6                    #specify the number of recovery points
   $storageaccountID = Get-AzStorageAccount -Name "mystorea" -ResourceGroupName "MyRG" | Select-Object -ExpandProperty Id

   $PolicyResult = New-AzRecoveryServicesAsrPolicy -Name $PolicyName -ReplicationProvider “HyperVReplicaAzure” -ReplicationFrequencyInSeconds $ReplicationFrequencyInSeconds -NumberOfRecoveryPointsToRetain $Recoverypoints -ApplicationConsistentSnapshotFrequencyInHours 1 -RecoveryAzureStorageAccountId $storageaccountID
   ```

1. Çoğaltma ilkesi oluşturma işleminin başarılı olduğundan emin olmak için döndürülen işi denetleyin.

1. Sitesine karşılık gelen koruma kapsayıcısını aşağıdaki gibi alın:

   ```azurepowershell
   $protectionContainer = Get-AzRecoveryServicesAsrProtectionContainer
   ```

1. Koruma kapsayıcısını şu şekilde çoğaltma ilkesiyle ilişkilendirin:

   ```azurepowershell
   $Policy = Get-AzRecoveryServicesAsrPolicy -FriendlyName $PolicyName
   $associationJob = New-AzRecoveryServicesAsrProtectionContainerMapping -Name $mappingName -Policy $Policy -PrimaryProtectionContainer $protectionContainer[0]
   ```

1. İlişkilendirme işinin başarıyla tamamlanmasını bekleyin.

1. Koruma kapsayıcısı eşlemesini alın.

   ```azurepowershell
   $ProtectionContainerMapping = Get-AzRecoveryServicesAsrProtectionContainerMapping -ProtectionContainer $protectionContainer
   ```

## <a name="step-7-enable-vm-protection"></a>7\. Adım: VM korumasını etkinleştirme

1. Korumak istediğiniz VM 'ye karşılık gelen korunabilir öğeyi aşağıdaki gibi alın:

   ```azurepowershell
   $VMFriendlyName = "Fabrikam-app"          #Name of the VM
   $ProtectableItem = Get-AzRecoveryServicesAsrProtectableItem -ProtectionContainer $protectionContainer -FriendlyName $VMFriendlyName
   ```

1. VM 'yi koruyun. Koruduğunuz sanal makineye birden fazla disk ekli ise, **Osdiskname** parametresini kullanarak işletim sistemi diskini belirtin.

   ```azurepowershell
   $OSType = "Windows"          # "Windows" or "Linux"
   $DRjob = New-AzRecoveryServicesAsrReplicationProtectedItem -ProtectableItem $VM -Name $VM.Name -ProtectionContainerMapping $ProtectionContainerMapping -RecoveryAzureStorageAccountId $StorageAccountID -OSDiskName $OSDiskNameList[$i] -OS $OSType -RecoveryResourceGroupId $ResourceGroupID
   ```

1. İlk çoğaltmadan sonra VM 'Lerin korumalı duruma ulaşmasını bekleyin. Bu işlem, çoğaltılacak veri miktarı ve Azure için kullanılabilir yukarı akış bant genişliği gibi etkenlere bağlı olarak biraz zaman alabilir. Korunan bir durum olduğunda, iş durumu ve StateDescription aşağıdaki gibi güncelleştirilir:

   ```console
   PS C:\> $DRjob = Get-AzRecoveryServicesAsrJob -Job $DRjob

   PS C:\> $DRjob | Select-Object -ExpandProperty State
   Succeeded

   PS C:\> $DRjob | Select-Object -ExpandProperty StateDescription
   Completed
   ```

1. Yük devretmeden sonra kurtarma özelliklerini (VM rolü boyutu gibi) ve VM NIC 'nin ekleneceği Azure ağını güncelleştirin.

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
> Azure 'da CMK özellikli yönetilen disklere çoğaltmak istiyorsanız az PowerShell 3.3.0 onlıve sürümlerini kullanarak aşağıdaki adımları uygulayın:
>
> 1. VM özelliklerini güncelleştirerek yönetilen disklere yük devretmeyi etkinleştirme
> 1. Korunan öğenin her bir diski için disk KIMLIĞINI getirmek üzere `Get-AzRecoveryServicesAsrReplicationProtectedItem` cmdlet 'ini kullanın
> 1. Disk KIMLIĞININ disk şifreleme kümesine eşlenmesini içermesi için `New-Object "System.Collections.Generic.Dictionary``2[System.String,System.String]"` cmdlet 'ini kullanarak bir sözlük nesnesi oluşturun. Bu disk şifreleme kümelerinin hedef bölgede sizin tarafınızdan önceden oluşturulması gerekir.
> 1. **Diskidtodiskencryptionsetmap** parametresindeki sözlük nesnesini geçirerek `Set-AzRecoveryServicesAsrReplicationProtectedItem` cmdlet 'INI kullanarak VM özelliklerini güncelleştirin.

## <a name="step-8-run-a-test-failover"></a>8\. Adım: yük devretme testi çalıştırma

1. Yük devretme testini aşağıdaki şekilde çalıştırın:

   ```azurepowershell
   $nw = Get-AzVirtualNetwork -Name "TestFailoverNw" -ResourceGroupName "MyRG" #Specify Azure vnet name and resource group

   $rpi = Get-AzRecoveryServicesAsrReplicationProtectedItem -ProtectionContainer $protectionContainer -FriendlyName $VMFriendlyName

   $TFjob = Start-AzRecoveryServicesAsrTestFailoverJob -ReplicationProtectedItem $VM -Direction PrimaryToRecovery -AzureVMNetworkId $nw.Id
   ```

1. Test VM 'sinin Azure 'da oluşturulduğunu doğrulayın. Test yük devretmesi işi, Azure 'da test VM oluşturulduktan sonra askıya alındı.
1. Yük devretme testini temizlemek ve gerçekleştirmek için şunu çalıştırın:

   ```azurepowershell
   $TFjob = Start-AzRecoveryServicesAsrTestFailoverCleanupJob -ReplicationProtectedItem $rpi -Comment "TFO done"
   ```

## <a name="next-steps"></a>Sonraki adımlar

Azure Resource Manager PowerShell cmdlet 'leriyle Azure Site Recovery hakkında [daha fazla bilgi edinin](/powershell/module/az.recoveryservices) .
