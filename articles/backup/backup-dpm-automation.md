---
title: DPM iş yüklerini yedeklemek için PowerShell'i kullanma
description: PowerShell'i kullanarak Azure Yedekleme'yi Veri Koruma Yöneticisi (DPM) için nasıl dağıtıp yöneteceklerini öğrenin
ms.topic: conceptual
ms.date: 01/23/2017
ms.openlocfilehash: 06c138a4015a0b730369e091fc57a34d2190051d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77616726"
---
# <a name="deploy-and-manage-backup-to-azure-for-data-protection-manager-dpm-servers-using-powershell"></a>PowerShell kullanarak Data Protection Manager (DPM) sunucuları için Azure’a yedekleme dağıtma ve yönetme

Bu makalede, Bir DPM sunucusunda Azure Yedekleme kurulumu ve yedekleme ve kurtarma yönetmek için PowerShell nasıl kullanılacağını gösterir.

## <a name="setting-up-the-powershell-environment"></a>PowerShell ortamını ayarlama

Data Protection Manager'dan Azure'a yedeklemeleri yönetmek için PowerShell'i kullanabilmeniz için PowerShell'de doğru ortama sahip olmanız gerekir. PowerShell oturumunun başlangıcında, doğru modülleri almak için aşağıdaki komutu çalıştırdığınızdan emin olun ve DPM cmdlets'e doğru şekilde başvurmanızı sağlayın:

```powershell
& "C:\Program Files\Microsoft System Center 2012 R2\DPM\DPM\bin\DpmCliInitScript.ps1"
```

```Output
Welcome to the DPM Management Shell!

Full list of cmdlets: Get-Command
Only DPM cmdlets: Get-DPMCommand
Get general help: help
Get help for a cmdlet: help <cmdlet-name> or <cmdlet-name> -?
Get definition of a cmdlet: Get-Command <cmdlet-name> -Syntax
Sample DPM scripts: Get-DPMSampleScript
```

## <a name="setup-and-registration"></a>Kurulum ve Kayıt

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Başlamak için en [son Azure PowerShell'i indirin.](/powershell/azure/install-az-ps)

Aşağıdaki kurulum ve kayıt görevleri PowerShell ile otomatikhale alınabilir:

* Kurtarma Hizmetleri kasası oluşturma
* Azure Yedekleme aracısını yükleme
* Azure Yedekleme hizmetine kaydolmak
* Ağ ayarları
* Şifreleme ayarları

## <a name="create-a-recovery-services-vault"></a>Kurtarma hizmetleri kasası oluşturma

Aşağıdaki adımlar, bir Kurtarma Hizmetleri kasası oluşturmanıza yol açar. Kurtarma Hizmetleri kasası Yedekleme kasasından farklıdır.

1. Azure Yedekleme'yi ilk kez kullanıyorsanız, Azure Kurtarma Hizmeti sağlayıcısını aboneliğinize kaydettirmek için **Register-AzResourceProvider** cmdlet'ini kullanmanız gerekir.

    ```powershell
    Register-AzResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ```

2. Kurtarma Hizmetleri kasası bir ARM kaynağıdır, bu nedenle kaynak grubuna yerleştirmeniz gerekir. Varolan bir kaynak grubunu kullanabilir veya yeni bir kaynak oluşturabilirsiniz. Yeni bir kaynak grubu oluştururken, kaynak grubunun adını ve konumunu belirtin.

    ```powershell
    New-AzResourceGroup –Name "test-rg" –Location "West US"
    ```

3. Yeni bir kasa oluşturmak için **Yeni-AzRecoveryServicesVault** cmdlet kullanın. Kaynak grubu için kullanılan kasa için aynı konumu belirttiğinden emin olun.

    ```powershell
    New-AzRecoveryServicesVault -Name "testvault" -ResourceGroupName " test-rg" -Location "West US"
    ```

4. Kullanılacak depolama artıklığının türünü belirtin; [Yerel Olarak Yedekli Depolama (LRS)](../storage/common/storage-redundancy-lrs.md) veya [Geo Redundant Depolama (GRS)](../storage/common/storage-redundancy-grs.md)kullanabilirsiniz. Aşağıdaki örnekte testVault için -BackupStorageRedundancy seçeneği GeoRedundant olarak ayarlanır.

   > [!TIP]
   > Çoğu Azure Backup cmdlet’i, girdi olarak Kurtarma Hizmetleri kasasını gerektirir. Bu nedenle, Yedekleme Kurtarma Hizmetleri kasasının bir değişkende depolanması uygundur.
   >
   >

    ```powershell
    $vault1 = Get-AzRecoveryServicesVault –Name "testVault"
    Set-AzRecoveryServicesBackupProperties  -vault $vault1 -BackupStorageRedundancy GeoRedundant
    ```

## <a name="view-the-vaults-in-a-subscription"></a>Abonelikteki kasaları görüntüleme

Geçerli abonelikteki tüm kasaların listesini görüntülemek için **Get-AzRecoveryServicesVault'u** kullanın. Bu komutu, yeni bir kasa oluşturulabilir mi denetlemek veya abonelikte hangi kasaların kullanılabildiğini görmek için kullanabilirsiniz.

Komutu çalıştırın, Get-AzRecoveryServicesVault ve abonelikteki tüm kasalar listelenir.

```powershell
Get-AzRecoveryServicesVault
```

```Output
Name              : Contoso-vault
ID                : /subscriptions/1234
Type              : Microsoft.RecoveryServices/vaults
Location          : WestUS
ResourceGroupName : Contoso-docs-rg
SubscriptionId    : 1234-567f-8910-abc
Properties        : Microsoft.Azure.Commands.RecoveryServices.ARSVaultProperties
```

## <a name="installing-the-azure-backup-agent-on-a-dpm-server"></a>Azure Yedekleme aracısını Bir DPM Sunucusuna yükleme

Azure Yedekleme aracısını yüklemeden önce yükleyicinin Windows Server'da indirilip sunulması gerekir. Yükleyicinin en son sürümünü Microsoft [İndirme Merkezi'nden](https://aka.ms/azurebackup_agent) veya Kurtarma Hizmetleri kasasının Pano sayfasından alabilirsiniz. Yükleyiciyi *C:\Downloads\*gibi kolayca erişilebilen bir konuma kaydedin.

Aracıyı yüklemek için, **DPM sunucusunda**yükseltilmiş bir PowerShell konsolunda aşağıdaki komutu çalıştırın:

```powershell
MARSAgentInstaller.exe /q
```

Bu, aracıyı tüm varsayılan seçenekleri yüklüyor. Yükleme arka planda birkaç dakika sürer. */nu* seçeneğini belirtmezseniz, yüklemenin sonunda herhangi bir güncelleştirme olup olmadığını kontrol etmek için **Windows Update** penceresi açılır.

Aracı, yüklenen programlar listesinde gösterir. Yüklü programların listesini görmek için **Denetim Masası** > **Programları** > **Programları ve Özellikleri'ne**gidin.

![Aracı yüklü](./media/backup-dpm-automation/installed-agent-listing.png)

### <a name="installation-options"></a>Yükleme seçenekleri

Komut satırı üzerinden kullanılabilen tüm seçenekleri görmek için aşağıdaki komutu kullanın:

```powershell
MARSAgentInstaller.exe /?
```

Kullanılabilir seçenekler şunlardır:

| Seçenek | Ayrıntılar | Varsayılan |
| --- | --- | --- |
| /q |Sessiz kurulum |- |
| /p:"konum" |Azure Yedekleme aracısının yükleme klasörüne giden yol. |C:\Program Files\Microsoft Azure Kurtarma Hizmetleri Aracısı |
| /s:"konum" |Azure Yedekleme aracısının önbellek klasörüne giden yol. |C:\Program Files\Microsoft Azure Kurtarma Hizmetleri Aracısı\Scratch |
| /m |Microsoft Update'e katılma |- |
| /nu |Yükleme tamamlandıktan sonra güncelleştirmeleri denetlemeyin |- |
| /d |Microsoft Azure Kurtarma Hizmetleri Aracısı'nı kaldır |- |
| /ph |Proxy Host Adresi |- |
| /po |Proxy Host Bağlantı Noktası Numarası |- |
| /pu |Proxy Host Kullanıcı Adı |- |
| /pw |Proxy Şifresi |- |

## <a name="registering-dpm-to-a-recovery-services-vault"></a>DPM'yi Kurtarma Hizmetleri Kasasına Kaydetme

Kurtarma Hizmetleri kasasını oluşturduktan sonra, en son aracıyı ve kasa kimlik bilgilerini indirin ve C:\Downloads gibi uygun bir yerde saklayın.

```powershell
$credspath = "C:\downloads"
$credsfilename = Get-AzRecoveryServicesVaultSettingsFile -Backup -Vault $vault1 -Path  $credspath
$credsfilename
```

```Output
C:\downloads\testvault\_Sun Apr 10 2016.VaultCredentials
```

DPM sunucusunda, makineyi kasaya kaydettirmek için [Start-OBRegistration](https://docs.microsoft.com/powershell/module/msonlinebackup/start-obregistration?view=winserver2012-ps) cmdlet'i çalıştırın.

```powershell
$cred = $credspath + $credsfilename
Start-OBRegistration-VaultCredentials $cred -Confirm:$false
```

```Output
CertThumbprint      :7a2ef2caa2e74b6ed1222a5e89288ddad438df2
SubscriptionID      : ef4ab577-c2c0-43e4-af80-af49f485f3d1
ServiceResourceName: testvault
Region              :West US
Machine registration succeeded.
```

### <a name="initial-configuration-settings"></a>İlk yapılandırma ayarları

DPM Server Kurtarma Hizmetleri kasasına kaydolduktan sonra varsayılan abonelik ayarlarıyla başlar. Bu abonelik ayarları, Ağ, Şifreleme ve Evreleme alanını içerir. Abonelik ayarlarını değiştirmek için öncelikle [Get-DPMCloudSubscriptionSetting](https://docs.microsoft.com/powershell/module/dataprotectionmanager/get-dpmcloudsubscriptionsetting?view=systemcenter-ps-2019) cmdlet'i kullanarak varolan (varsayılan) ayarları ele almanız gerekir:

```powershell
$setting = Get-DPMCloudSubscriptionSetting -DPMServerName "TestingServer"
```

Tüm değişiklikler bu yerel PowerShell ```$setting``` nesnesi için yapılır ve sonra tam nesne DPM ve Azure Yedekleme'ye [ayar-DPMCloudSubscriptionSetting](https://docs.microsoft.com/powershell/module/dataprotectionmanager/set-dpmcloudsubscriptionsetting?view=systemcenter-ps-2019) cmdlet kullanarak bunları kaydetmek için taahhüt edilir. Değişikliklerin kalıcı olduğundan ```–Commit``` emin olmak için bayrağı kullanmanız gerekir. Ayarlar, taahhüt edilmedikçe Azure Yedekleme tarafından uygulanmaz ve kullanılmaz.

```powershell
Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -Commit
```

## <a name="networking"></a>Ağ Oluşturma

DPM makinesinin Internet'teki Azure Yedekleme hizmetine bağlantısı bir proxy sunucusu aracılığıyla gerçekse, başarılı yedeklemeler için proxy sunucu ayarları sağlanmalıdır. Bu, [Set-DPMCloudSubscriptionSetting](https://docs.microsoft.com/powershell/module/dataprotectionmanager/set-dpmcloudsubscriptionsetting?view=systemcenter-ps-2019) cmdlet ile ve ```-ProxyPort```, ```-ProxyUsername``` ve ```ProxyPassword``` parametrelerkullanılarak yapılır. ```-ProxyServer``` Bu örnekte, proxy sunucusu bulunmadığından, proxy ile ilgili bilgileri açıkça temizliyoruz.

```powershell
Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -NoProxy
```

Bant genişliği kullanımı da seçenekleri ```-WorkHourBandwidth``` ile ```-NonWorkHourBandwidth``` kontrol edilebilir ve haftanın belirli bir dizi gün için. Bu örnekte, herhangi bir azaltma ayarlamiyoruz.

```powershell
Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -NoThrottle
```

## <a name="configuring-the-staging-area"></a>Evreleme Alanının Yapılandırılması

DPM sunucusunda çalışan Azure Yedekleme aracısı, buluttan geri yüklenen veriler (yerel hazırlama alanı) için geçici depolama alanına ihtiyaç duyar. [Set-DPMCloudSubscriptionSetting](https://docs.microsoft.com/powershell/module/dataprotectionmanager/set-dpmcloudsubscriptionsetting?view=systemcenter-ps-2019) cmdlet ve ```-StagingAreaPath``` parametrekullanarak hazırlama alanını yapılandırın.

```powershell
Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -StagingAreaPath "C:\StagingArea"
```

Yukarıdaki örnekte, evreleme alanı PowerShell nesnesindeki ```$setting``` *C:\EvrelemeAlanı* olarak ayarlanır. Belirtilen klasörün zaten var olduğundan emin olun, aksi takdirde abonelik ayarlarının son işleme başarısız olur.

### <a name="encryption-settings"></a>Şifreleme ayarları

Azure Yedekleme'ye gönderilen yedekleme verileri, verilerin gizliliğini korumak için şifrelenir. Şifreleme parolası, geri yükleme sırasında verilerin şifresini çözmek için "parola"dır. Ayarlandıktan sonra bu bilgileri güvende ve emniyetli tutmak önemlidir.

Aşağıdaki örnekte, ilk komut dizeyi ```passphrase123456789``` güvenli bir dize dönüştürür ve güvenli ```$Passphrase```dize adlı değişkene atar. ikinci komut, güvenli dizeyi yedeklemeleri şifreleme küçesi için parola ```$Passphrase``` olarak ayarlar.

```powershell
$Passphrase = ConvertTo-SecureString -string "passphrase123456789" -AsPlainText -Force

Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -EncryptionPassphrase $Passphrase
```

> [!IMPORTANT]
> Parola bilgilerini ayarlandıktan sonra güvenli ve emniyetli tutun. Bu parola olmadan Azure'dan veri geri yükleyemesiniz.
>
>

Bu noktada, ```$setting``` nesne için gerekli tüm değişiklikleri yapmış olmalıdır. Değişiklikleri gerçekleştirmeyi unutmayın.

```powershell
Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -Commit
```

## <a name="protect-data-to-azure-backup"></a>Verileri Azure Yedekleme'ye koruma

Bu bölümde, DPM'ye bir üretim sunucusu ekleyecek ve ardından verileri yerel DPM depolama alanına ve ardından Azure Yedekleme'ye koruyacaksınız. Örneklerde, dosya ve klasörlerin nasıl yedeklenebildiğini göstereceğiz. Mantık, DPM destekli herhangi bir veri kaynağını yedeklemek için kolayca genişletilebilir. Tüm DPM yedeklemeleriniz dört bölümden oluşur:

1. **Grup üyeleri,** aynı koruma grubunda korumak istediğiniz tüm korunabilir nesnelerin (DPM'deki *Datasources* olarak da bilinir) listesidir. Örneğin, üretim VM'lerini bir koruma grubunda ve SQL Server veritabanlarını başka bir koruma grubunda, farklı yedekleme gereksinimleri olabileceğinden korumak isteyebilirsiniz. Bir üretim sunucusunda herhangi bir veri kaynağını yedeklemeden önce DPM Aracısının sunucuya yüklendiğinden ve DPM tarafından yönetildiğinden emin olmanız gerekir. [DPM Aracısını yüklemek](https://docs.microsoft.com/system-center/dpm/deploy-dpm-protection-agent?view=sc-dpm-2019) ve uygun DPM Sunucusuna bağlamak için adımları izleyin.
2. **Veri koruma yöntemi,** teyp, disk ve bulut gibi hedef yedekleme konumlarını belirtir. Örneğimizde verileri yerel diske ve buluta koruyacağız.
3. Yedeklemelerin ne zaman alınması gerektiğini ve verilerin DPM Sunucusu ile üretim sunucusu arasında ne sıklıkta eşitlemesi gerektiğini belirten **bir yedekleme zamanlaması.**
4. Azure'daki kurtarma noktalarının ne kadar süreyle saklanması gerektiğini belirten bir **bekletme zamanlaması.**

### <a name="creating-a-protection-group"></a>Koruma grubu oluşturma

[New-DPMProtectionGroup](https://docs.microsoft.com/powershell/module/dataprotectionmanager/new-dpmprotectiongroup?view=systemcenter-ps-2019) cmdlet kullanarak yeni bir Koruma Grubu oluşturarak başlayın.

```powershell
$PG = New-DPMProtectionGroup -DPMServerName " TestingServer " -Name "ProtectGroup01"
```

Yukarıdaki cmdlet *ProtectGroup01*adlı bir Koruma Grubu oluşturacaktır. Varolan bir koruma grubu daha sonra Azure bulutuna yedekleme eklemek için değiştirilebilir. Ancak, Koruma Grubu'nda herhangi bir değişiklik yapmak için - yeni veya mevcut - [Get-DPMModifiableProtectionGroup](https://docs.microsoft.com/powershell/module/dataprotectionmanager/get-dpmmodifiableprotectiongroup?view=systemcenter-ps-2019) cmdlet kullanarak *değiştirilebilir* bir nesne üzerinde bir ele almak gerekir.

```powershell
$MPG = Get-ModifiableProtectionGroup $PG
```

### <a name="adding-group-members-to-the-protection-group"></a>Koruma Grubuna grup üyeleri ekleme

Her DPM Aracısı, yüklü olduğu sunucudaki veri kaynaklarının listesini bilir. Koruma Grubuna bir veri kaynağı eklemek için, DPM Aracısı'nın önce dpm sunucusuna veri kaynaklarının bir listesini göndermesi gerekir. Daha sonra bir veya daha fazla veri kaynağı seçilir ve Koruma Grubu'na eklenir. Bunu başarmak için gereken PowerShell adımları şunlardır:

1. DPM Aracısı aracılığıyla DPM tarafından yönetilen tüm sunucuların listesini getirin.
2. Belirli bir sunucu seçin.
3. Sunucudaki tüm veri kaynaklarının listesini getirin.
4. Bir veya daha fazla veri kaynağı seçin ve bunları Koruma Grubuna ekleyin

DPM Aracısı'nın yüklü olduğu ve DPM Server tarafından yönetildiği sunucuların listesi [Get-DPMProductionServer](https://docs.microsoft.com/powershell/module/dataprotectionmanager/get-dpmproductionserver?view=systemcenter-ps-2019) cmdlet ile elde edilir. Bu örnekte biz filtre ve sadece yedekleme için isim *productionserver01* ile PS yapılandırmak.

```powershell
$server = Get-ProductionServer -DPMServerName "TestingServer" | Where-Object {($_.servername) –contains "productionserver01"}
```

Şimdi ```$server``` [Get-DPMDatasource](https://docs.microsoft.com/powershell/module/dataprotectionmanager/get-dpmdatasource?view=systemcenter-ps-2019) cmdlet kullanarak veri kaynaklarının listesini getirin. Bu örnekte, yedekleme için yapılandırmak istediğimiz *D\\ * hacmiiçin filtreleme yapıyoruz. Bu veri kaynağı daha sonra [Add-DPMChildDatasource](https://docs.microsoft.com/powershell/module/dataprotectionmanager/add-dpmchilddatasource?view=systemcenter-ps-2019) cmdlet kullanılarak Koruma Grubuna eklenir. Eklemeleri yapmak için *değiştirilebilir* koruma ```$MPG``` grubu nesnesini kullanmayı unutmayın.

```powershell
$DS = Get-Datasource -ProductionServer $server -Inquire | Where-Object { $_.Name -contains "D:\" }

Add-DPMChildDatasource -ProtectionGroup $MPG -ChildDatasource $DS
```

Seçilen tüm veri kaynaklarını koruma grubuna ekleyene kadar bu adımı gerektiği kadar yineleyin. Ayrıca yalnızca bir veri kaynağıyla başlayabilir ve Koruma Grubu'nu oluşturmak için iş akışını tamamlayabilir ve daha sonraki bir noktada Koruma Grubu'na daha fazla veri kaynağı ekleyebilirsiniz.

### <a name="selecting-the-data-protection-method"></a>Veri koruma yöntemini seçme

Veri kaynakları Koruma Grubuna eklendikten sonra, bir sonraki adım [Set-DPMProtectionType](https://docs.microsoft.com/powershell/module/dataprotectionmanager/set-dpmprotectiontype?view=systemcenter-ps-2019) cmdlet kullanarak koruma yöntemini belirtmektir. Bu örnekte, Koruma Grubu yerel disk ve bulut yedekleme için kurulumdur. Ayrıca, -Çevrimiçi bayraklı [Add-DPMChildDatasource](https://docs.microsoft.com/powershell/module/dataprotectionmanager/add-dpmchilddatasource?view=systemcenter-ps-2019) cmdlet'i kullanarak buluta korumak istediğiniz veri kaynağını belirtmeniz gerekir.

```powershell
Set-DPMProtectionType -ProtectionGroup $MPG -ShortTerm Disk –LongTerm Online
Add-DPMChildDatasource -ProtectionGroup $MPG -ChildDatasource $DS –Online
```

### <a name="setting-the-retention-range"></a>Bekletme aralığını ayarlama

[Set-DPMPolicyObjective](https://docs.microsoft.com/powershell/module/dataprotectionmanager/set-dpmpolicyobjective?view=systemcenter-ps-2019) cmdlet kullanarak yedekleme noktaları için bekletme ayarlayın. Yedekleme zamanlaması tanımlanmadan önce bekletme ayarlamak garip görünse ```Set-DPMPolicyObjective``` de, cmdlet'i kullanarak otomatik olarak değiştirilebilen varsayılan bir yedekleme zamanlaması ayarlar. Yedekleme zamanlamasını önce ve bekletme ilkesini sonra ayarlamak her zaman mümkündür.

Aşağıdaki örnekte, cmdlet disk yedeklemeleri için bekletme parametrelerini ayarlar. Bu, yedeklemeleri 10 gün boyunca saklar ve verileri üretim sunucusu ile DPM sunucusu arasında her 6 saatte bir eşitler. Yedekleme ```SynchronizationFrequencyMinutes``` noktasının ne sıklıkta oluşturulduğunu değil, verilerin DPM sunucusuna ne sıklıkta kopyalanıldığını tanımlamaz.  Bu ayar, yedeklemelerin çok büyük olmasını önler.

```powershell
Set-DPMPolicyObjective –ProtectionGroup $MPG -RetentionRangeInDays 10 -SynchronizationFrequencyMinutes 360
```

Azure'a giden yedeklemeler için (DPM çevrimiçi yedekleme olarak ifade eder) bekletme [aralıkları, büyükbaba-baba-oğul düzeni (GFS) kullanılarak uzun süreli bekletme](backup-azure-backup-cloud-as-tape.md)için yapılandırılabilir. Diğer bir süre, günlük, haftalık, aylık ve yıllık bekletme ilkelerini içeren birleştirilmiş bekletme ilkesi tanımlayabilirsiniz. Bu örnekte, istediğimiz karmaşık bekletme düzenini temsil eden bir dizi oluşturuyoruz ve ardından [Set-DPMPolicyObjective](https://docs.microsoft.com/powershell/module/dataprotectionmanager/set-dpmpolicyobjective?view=systemcenter-ps-2019) cmdlet'i kullanarak bekletme aralığını yapılandırıyoruz.

```powershell
$RRlist = @()
$RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 180, Days)
$RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 104, Weeks)
$RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 60, Month)
$RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 10, Years)
Set-DPMPolicyObjective –ProtectionGroup $MPG -OnlineRetentionRangeList $RRlist
```

### <a name="set-the-backup-schedule"></a>Yedekleme zamanlamasını ayarlama

```Set-DPMPolicyObjective``` Cmdlet'i kullanarak koruma hedefini belirtirseniz DPM varsayılan yedekleme zamanlamasını otomatik olarak ayarlar. Varsayılan zamanlamaları değiştirmek için, [Set-DPMPolicySchedule](https://docs.microsoft.com/powershell/module/dataprotectionmanager/set-dpmpolicyschedule?view=systemcenter-ps-2019) cmdlet'inardından [Get-DPMPolicySchedule](https://docs.microsoft.com/powershell/module/dataprotectionmanager/get-dpmpolicyschedule?view=systemcenter-ps-2019) cmdlet'ini kullanın.

```powershell
$onlineSch = Get-DPMPolicySchedule -ProtectionGroup $mpg -LongTerm Online
Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[0] -TimesOfDay 02:00
Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[1] -TimesOfDay 02:00 -DaysOfWeek Sa,Su –Interval 1
Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[2] -TimesOfDay 02:00 -RelativeIntervals First,Third –DaysOfWeek Sa
Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[3] -TimesOfDay 02:00 -DaysOfMonth 2,5,8,9 -Months Jan,Jul
Set-DPMProtectionGroup -ProtectionGroup $MPG
```

Yukarıdaki örnekte, ```$onlineSch``` GFS düzeninde Koruma Grubu için varolan çevrimiçi koruma zamanlamasını içeren dört öğeiçeren bir dizi vardır:

1. ```$onlineSch[0]```günlük programı içerir
2. ```$onlineSch[1]```haftalık programı içerir
3. ```$onlineSch[2]```aylık zamanlamasını içerir
4. ```$onlineSch[3]```yıllık zamanlamaiçerir

Bu nedenle, haftalık programı değiştirmeniz gerekiyorsa, 'ye başvurmanız gerekir. ```$onlineSch[1]```

### <a name="initial-backup"></a>İlk yedekleme

Bir veri kaynağını ilk kez yedeklediğinizde, DPM'nin dpm yineleme hacminde korunması için veri kaynağının tam kopyasını oluşturan ilk yinelemeyi oluşturması gerekir. Bu etkinlik belirli bir süre için zamanlanabilir veya parametreile ```-NOW``` [Set-DPMReplicaCreationMethod](https://docs.microsoft.com/powershell/module/dataprotectionmanager/set-dpmreplicacreationmethod?view=systemcenter-ps-2019) cmdlet kullanılarak el ile tetiklenebilir.

```powershell
Set-DPMReplicaCreationMethod -ProtectionGroup $MPG -NOW
```

### <a name="changing-the-size-of-dpm-replica--recovery-point-volume"></a>DPM Çoğaltma & kurtarma noktası hacminin boyutunu değiştirme

Aşağıdaki örnekte olduğu gibi [Set-DPMDatasourceDiskAllocation](https://docs.microsoft.com/powershell/module/dataprotectionmanager/set-dpmdatasourcediskallocation?view=systemcenter-ps-2019) cmdlet kullanarak DPM Çoğaltma hacmi ve Gölge Kopyalama hacmiboyutunu da değiştirebilirsiniz: Get-DatasourceDiskAllocation -Datasource $DS Set-DatasourceDiskAllocation -Datasource $DS -ProtectionGroup $MPG -manuel -ReplicaArea (2gb) -ShadowCopyArea (2gb)

### <a name="committing-the-changes-to-the-protection-group"></a>Koruma Grubunda değişiklik yapılmasına dair

Son olarak, DPM'nin yeni Koruma Grubu yapılandırması başına yedeklemeyi alabilmek için değişikliklerin işlenmesi gerekir. Bu, [Set-DPMProtectionGroup](https://docs.microsoft.com/powershell/module/dataprotectionmanager/set-dpmprotectiongroup?view=systemcenter-ps-2019) cmdlet kullanılarak elde edilebilir.

```powershell
Set-DPMProtectionGroup -ProtectionGroup $MPG
```

## <a name="view-the-backup-points"></a>Yedekleme noktalarını görüntüleme

Bir veri kaynağı için tüm kurtarma noktalarının listesini almak için [Get-DPMRecoveryPoint](https://docs.microsoft.com/powershell/module/dataprotectionmanager/get-dpmrecoverypoint?view=systemcenter-ps-2019) cmdlet'i kullanabilirsiniz. Bu örnekte, şunları yapacağız:

* DPM sunucusundaki tüm PG'leri getirin ve bir dizide depolanır```$PG```
* ilgili veri kaynaklarını almak```$PG[0]```
* bir veri kaynağı için tüm kurtarma puanlarını alın.

```powershell
$PG = Get-DPMProtectionGroup –DPMServerName "TestingServer"
$DS = Get-DPMDatasource -ProtectionGroup $PG[0]
$RecoveryPoints = Get-DPMRecoverypoint -Datasource $DS[0] -Online
```

## <a name="restore-data-protected-on-azure"></a>Azure'da korumalı verileri geri yükleme

Verileri geri, bir ```RecoverableItem``` nesne ve nesnenin ```RecoveryOption``` birleşimidir. Önceki bölümde, bir veri kaynağı için yedekleme noktalarının bir listesini aldık.

Aşağıdaki örnekte, yedekleme noktalarını kurtarma hedefiyle birleştirerek Azure Yedekleme'den bir Hyper-V sanal makineyi nasıl geri yükleyebileceğimizi gösteriyoruz. Bu örnek şunları içerir:

* [New-DPMRecoveryOption](https://docs.microsoft.com/powershell/module/dataprotectionmanager/new-dpmrecoveryoption?view=systemcenter-ps-2019) cmdlet kullanarak bir kurtarma seçeneği oluşturma.
* ```Get-DPMRecoveryPoint``` Cmdlet kullanarak yedekleme noktaları dizi alma.
* Geri yükleme için bir yedekleme noktası seçme.

```powershell
$RecoveryOption = New-DPMRecoveryOption -HyperVDatasource -TargetServer "HVDCenter02" -RecoveryLocation AlternateHyperVServer -RecoveryType Recover -TargetLocation "C:\VMRecovery"

$PG = Get-DPMProtectionGroup –DPMServerName "TestingServer"
$DS = Get-DPMDatasource -ProtectionGroup $PG[0]
$RecoveryPoints = Get-DPMRecoverypoint -Datasource $DS[0] -Online

Restore-DPMRecoverableItem -RecoverableItem $RecoveryPoints[0] -RecoveryOption $RecoveryOption
```

Komutlar herhangi bir veri kaynağı türü için kolayca genişletilebilir.

## <a name="next-steps"></a>Sonraki adımlar

* DPM'den Azure Yedekleme'ye kadar daha fazla bilgi için [Bkz. DPM Yedeklemeye Giriş](backup-azure-dpm-introduction.md)
