---
title: Windows Server'ı Azure'a yedeklemek için PowerShell'i kullanın
description: Bu makalede, Windows Server veya Windows istemcisinde Azure Yedekleme'yi ayarlamak ve yedekleme ve kurtarmayı yönetmek için PowerShell'i nasıl kullanacağınızı öğrenin.
ms.topic: conceptual
ms.date: 12/2/2019
ms.openlocfilehash: efe0b93fe1e37990422ffbd2256e38c12401dca5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78673186"
---
# <a name="deploy-and-manage-backup-to-azure-for-windows-serverwindows-client-using-powershell"></a>PowerShell kullanarak Windows Server/Windows İstemcisi için Azure’a yedekleme dağıtma ve yönetme

Bu makalede, Windows Server veya Windows istemcisi üzerinde Azure Yedekleme'yi ayarlamak ve yedekleme ve kurtarma yı yönetmek için PowerShell'i nasıl kullanacağınızı gösterir.

## <a name="install-azure-powershell"></a>Azure PowerShell'i yükleme

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Başlamak için, [en son PowerShell sürümü yükleyin.](/powershell/azure/install-az-ps)

## <a name="create-a-recovery-services-vault"></a>Kurtarma hizmetleri kasası oluşturma

Aşağıdaki adımlar, bir Kurtarma Hizmetleri kasası oluşturmanıza yol açar. Kurtarma Hizmetleri kasası Yedekleme kasasından farklıdır.

1. Azure Yedekleme'yi ilk kez kullanıyorsanız, Azure Kurtarma Hizmeti sağlayıcısını aboneliğinize kaydettirmek için **Register-AzResourceProvider** cmdlet'ini kullanmanız gerekir.

    ```powershell
    Register-AzResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ```

2. Kurtarma Hizmetleri kasası bir ARM kaynağıdır, bu nedenle kaynak grubuna yerleştirmeniz gerekir. Varolan bir kaynak grubunu kullanabilir veya yeni bir kaynak oluşturabilirsiniz. Yeni bir kaynak grubu oluştururken, kaynak grubunun adını ve konumunu belirtin.  

    ```powershell
    New-AzResourceGroup –Name "test-rg" –Location "WestUS"
    ```

3. Yeni tonoz oluşturmak için **Yeni-AzRecoveryServicesVault** cmdlet kullanın. Kaynak grubu için kullanılan kasa için aynı konumu belirttiğinden emin olun.

    ```powershell
    New-AzRecoveryServicesVault -Name "testvault" -ResourceGroupName " test-rg" -Location "WestUS"
    ```

4. Kullanılacak depolama artıklığının türünü belirtin; [Yerel Olarak Yedekli Depolama (LRS)](../storage/common/storage-redundancy-lrs.md) veya [Geo Redundant Depolama (GRS)](../storage/common/storage-redundancy-grs.md)kullanabilirsiniz. Aşağıdaki örnekte testVault için -BackupStorageRedundancy seçeneği GeoRedundant olarak ayarlanır.

   > [!TIP]
   > Çoğu Azure Backup cmdlet’i, girdi olarak Kurtarma Hizmetleri kasasını gerektirir. Bu nedenle, Yedekleme Kurtarma Hizmetleri kasasının bir değişkende depolanması uygundur.
   >
   >

    ```powershell
    $Vault1 = Get-AzRecoveryServicesVault –Name "testVault"
    Set-AzRecoveryServicesBackupProperties -Vault $Vault1 -BackupStorageRedundancy GeoRedundant
    ```

## <a name="view-the-vaults-in-a-subscription"></a>Abonelikteki kasaları görüntüleme

Geçerli abonelikteki tüm kasaların listesini görüntülemek için **Get-AzRecoveryServicesVault'u** kullanın. Bu komutu, yeni bir kasa oluşturulabilir mi denetlemek veya abonelikte hangi kasaların kullanılabildiğini görmek için kullanabilirsiniz.

Komutu çalıştırın, **Get-AzRecoveryServicesVault**ve abonelikteki tüm kasalar listelenir.

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

[!INCLUDE [backup-upgrade-mars-agent.md](../../includes/backup-upgrade-mars-agent.md)]

## <a name="installing-the-azure-backup-agent"></a>Azure Yedekleme aracısını yükleme

Azure Yedekleme aracısını yüklemeden önce yükleyicinin Windows Server'da indirilip sunulması gerekir. Yükleyicinin en son sürümünü Microsoft [İndirme Merkezi'nden](https://aka.ms/azurebackup_agent) veya Kurtarma Hizmetleri kasasının Pano sayfasından alabilirsiniz. Yükleyiciyi *C:\Downloads\*gibi kolayca erişilebilen bir konuma kaydedin.

Alternatif olarak, indirici almak için PowerShell kullanın:

 ```powershell
 $MarsAURL = 'https://aka.ms/Azurebackup_Agent'
 $WC = New-Object System.Net.WebClient
 $WC.DownloadFile($MarsAURL,'C:\downloads\MARSAgentInstaller.EXE')
 C:\Downloads\MARSAgentInstaller.EXE /q
 ```

Aracıyı yüklemek için, yükseltilmiş bir PowerShell konsolunda aşağıdaki komutu çalıştırın:

```powershell
MARSAgentInstaller.exe /q
```

Bu, aracıyı tüm varsayılan seçenekleri yüklüyor. Yükleme arka planda birkaç dakika sürer. */nu* seçeneğini belirtmezseniz, yüklemenin sonunda herhangi bir güncelleştirme olup olmadığını denetlemek için **Windows Update** penceresi açılır. Yüklendikten sonra aracı yüklü programlar listesinde gösterir.

Yüklü programların listesini görmek için **Denetim Masası** > **Programları** > **Programları ve Özellikleri'ne**gidin.

![Aracı yüklü](./media/backup-client-automation/installed-agent-listing.png)

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

## <a name="registering-windows-server-or-windows-client-machine-to-a-recovery-services-vault"></a>Windows Server veya Windows istemci makinesini Kurtarma Hizmetleri Kasasına Kaydetme

Kurtarma Hizmetleri kasasını oluşturduktan sonra, en son aracıyı ve kasa kimlik bilgilerini indirin ve C:\Downloads gibi uygun bir yerde saklayın.

```powershell
$CredsPath = "C:\downloads"
$CredsFilename = Get-AzRecoveryServicesVaultSettingsFile -Backup -Vault $Vault1 -Path $CredsPath
```

### <a name="registering-using-the-ps-az-module"></a>PS Az modüllerini kullanarak kayıt

> [!NOTE]
> Tonoz sertifikası oluşturma ile bir hata Az 3.5.0 sürümünde sabittir. Kasa sertifikası indirmek için Az 3.5.0 sürüm sürümünü veya daha büyük sürümü kullanın.

PowerShell'in en son Az modülünde, temel platform sınırlamaları nedeniyle, kasa kimlik bilgilerini indirmek için kendi imzalı bir sertifika gerekir. Aşağıdaki örnekte, kendi imzalı bir sertifikanın nasıl sağlandığı ve kasa kimlik bilgilerinin nasıl indirilen gösterilmektedir.

```powershell
$dt = $(Get-Date).ToString("M-d-yyyy")
$cert = New-SelfSignedCertificate -CertStoreLocation Cert:\CurrentUser\My -FriendlyName 'test-vaultcredentials' -subject "Windows Azure Tools" -KeyExportPolicy Exportable -NotAfter $(Get-Date).AddHours(48) -NotBefore $(Get-Date).AddHours(-24) -KeyProtection None -KeyUsage None -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.2") -Provider "Microsoft Enhanced Cryptographic Provider v1.0"
$certficate = [convert]::ToBase64String($cert.Export([System.Security.Cryptography.X509Certificates.X509ContentType]::Pfx))
$CredsFilename = Get-AzRecoveryServicesVaultSettingsFile -Backup -Vault $Vault -Path $CredsPath -Certificate $certficate
```

Windows Server veya Windows istemci makinesinde, makineyi kasaya kaydettirmek için [Başlat-OBRegistration](https://docs.microsoft.com/powershell/module/msonlinebackup/start-obregistration?view=winserver2012-ps) cmdlet'ini çalıştırın.
Bu ve yedekleme için kullanılan diğer cmdlet'ler, Mars AgentInstaller'ın yükleme işleminin bir parçası olarak eklediği MSONLINE modülünden dir.

Aracı yükleyici $Env:PSModulePath değişkenini güncellemez. Bu, modülün otomatik yüklemebaşarısız olduğu anlamına gelir. Bunu çözmek için aşağıdakileri yapabilirsiniz:

```powershell
$Env:PSModulePath += ';C:\Program Files\Microsoft Azure Recovery Services Agent\bin\Modules'
```

Alternatif olarak, modülü komut dosyanızdaki aşağıdaki gibi el ile yükleyebilirsiniz:

```powershell
Import-Module -Name 'C:\Program Files\Microsoft Azure Recovery Services Agent\bin\Modules\MSOnlineBackup'
```

Çevrimiçi Yedekleme cmdletlerini yükledikten sonra kasa kimlik bilgilerini kaydedersiniz:

```powershell
Start-OBRegistration -VaultCredentials $CredsFilename.FilePath -Confirm:$false
```

```Output
CertThumbprint      : 7a2ef2caa2e74b6ed1222a5e89288ddad438df2
SubscriptionID      : ef4ab577-c2c0-43e4-af80-af49f485f3d1
ServiceResourceName : testvault
Region              : WestUS
Machine registration succeeded.
```

> [!IMPORTANT]
> Kasa kimlik bilgileri dosyasını belirtmek için göreli yollar kullanmayın. Cmdlet bir giriş olarak mutlak bir yol sağlamalıdır.
>
>

## <a name="networking-settings"></a>Ağ ayarları

Windows makinesinin internete bağlantısı proxy sunucusu aracılığıyla olduğunda, proxy ayarları aracıya da sağlanabilir. Bu örnekte, proxy sunucusu bulunmadığından, proxy ile ilgili bilgileri açıkça temizliyoruz.

Bant genişliği kullanımı da seçenekleri ile `work hour bandwidth` `non-work hour bandwidth` kontrol edilebilir ve haftanın belirli bir dizi gün için.

Proxy ve bant genişliği ayrıntılarınıayarlama [Set-OBMachineSetting](https://docs.microsoft.com/powershell/module/msonlinebackup/set-obmachinesetting?view=winserver2012-ps) cmdlet kullanılarak yapılır:

```powershell
Set-OBMachineSetting -NoProxy
```

```Output
Server properties updated successfully.
```

```powershell
Set-OBMachineSetting -NoThrottle
```

```Output
Server properties updated successfully.
```

## <a name="encryption-settings"></a>Şifreleme ayarları

Azure Yedekleme'ye gönderilen yedekleme verileri, verilerin gizliliğini korumak için şifrelenir. Şifreleme parolası, geri yükleme sırasında verilerin şifresini çözmek için "parola"dır.

Azure portalının **Kurtarma Hizmetleri kasası** bölümünde **Ayarlar** > **Özellikleri** > **Güvenlik PINi** altında **Oluştur'u**seçerek bir güvenlik pini oluşturmanız gerekir. Ardından, bunu komut `generatedPIN` olarak kullanın:

```powershell
$PassPhrase = ConvertTo-SecureString -String "Complex!123_STRING" -AsPlainText -Force
Set-OBMachineSetting -EncryptionPassPhrase $PassPhrase -SecurityPin "<generatedPIN>"
```

```Output
Server properties updated successfully
```

> [!IMPORTANT]
> Parola bilgilerini ayarlandıktan sonra güvenli ve emniyetli tutun. Bu parola olmadan Azure'dan veri geri yükleyemezsiniz.
>
>

## <a name="back-up-files-and-folders"></a>Dosya ve klasörleri yedekleme

Windows Sunucuları'ndan Azure Yedekleme'ye kadar tüm yedeklemeler bir ilke tarafından yönetilir. İlke üç bölümden oluşur:

1. Yedeklemelerin ne zaman alınması ve hizmetle eşitlenmeleri gerektiğini belirten **bir yedekleme zamanlaması.**
2. Azure'daki kurtarma noktalarının ne kadar süreyle saklanması gerektiğini belirten bir **bekletme zamanlaması.**
3. Neyin yedekleneceğini belirleyen bir **dosya ekleme/dışlama belirtimi.**

Bu belgede, yedeklemeyi otomatikleştirdiğimiziçin hiçbir şeyin yapılandırılmamadığını varsayacağız. [Yeni OBPolicy](https://docs.microsoft.com/powershell/module/msonlinebackup/new-obpolicy?view=winserver2012-ps) cmdlet kullanarak yeni bir yedekleme ilkesi oluşturarak başlıyoruz.

```powershell
$NewPolicy = New-OBPolicy
```

Şu anda ilke boştur ve hangi öğelerin dahil edilip dışlanacağını, yedeklemelerin ne zaman çalıştırAcağını ve yedeklemelerin nerede depolanacağını tanımlamak için diğer cmdlets'ler gereklidir.

### <a name="configuring-the-backup-schedule"></a>Yedekleme zamanlamasını yapılandırma

Bir ilkenin üç bölümünün ilki, [Yeni OBSchedule](https://docs.microsoft.com/powershell/module/msonlinebackup/new-obschedule?view=winserver2012-ps) cmdlet kullanılarak oluşturulan yedekleme zamanlamasıdır. Yedekleme zamanlaması, yedeklemelerin ne zaman alınması gerektiğini tanımlar. Zamanlama oluştururken, iki giriş parametresini belirtmeniz gerekir:

* Yedeklemenin çalışması gereken **haftanın günleri.** Yedekleme işini sadece bir günde veya haftanın her günü veya aradaki herhangi bir kombinasyonda çalıştırabilirsiniz.
* Yedeklemenin çalışması gereken **günün saatleri.** Yedeklemenin tetikleneceği günün en fazla üç farklı saatini tanımlayabilirsiniz.

Örneğin, her Cumartesi ve Pazar günü saat 16:00'da çalışan bir yedekleme ilkesini yapılandırabilirsiniz.

```powershell
$Schedule = New-OBSchedule -DaysOfWeek Saturday, Sunday -TimesOfDay 16:00
```

Yedekleme zamanlaması bir ilke ile ilişkili olması gerekir ve bu [Set-OBSchedule](https://docs.microsoft.com/powershell/module/msonlinebackup/set-obschedule?view=winserver2012-ps) cmdlet kullanılarak elde edilebilir.

```powershell
Set-OBSchedule -Policy $NewPolicy -Schedule $Schedule
```

```Output
BackupSchedule : 4:00 PM Saturday, Sunday, Every 1 week(s) DsList : PolicyName : RetentionPolicy : State : New PolicyState : Valid
```

### <a name="configuring-a-retention-policy"></a>Bekletme ilkesini yapılandırma

Bekletme ilkesi, yedekleme işlerinden oluşturulan kurtarma noktalarının ne kadar süreyle korunup korunabildiğini tanımlar. [Yeni OBRetentionİlki](https://docs.microsoft.com/powershell/module/msonlinebackup/new-obretentionpolicy?view=winserver2012-ps) cmdlet'ini kullanarak yeni bir bekletme ilkesi oluştururken, yedekleme kurtarma noktalarının Azure Yedekleme ile korunması gereken gün sayısını belirtebilirsiniz. Aşağıdaki örnekte yedi günlük bir bekletme ilkesi belirlenir.

```powershell
$RetentionPolicy = New-OBRetentionPolicy -RetentionDays 7
```

Bekletme ilkesi, cmdlet [Set-OBRetentionPolicy](https://docs.microsoft.com/powershell/module/msonlinebackup/set-obretentionpolicy?view=winserver2012-ps)kullanılarak ana politika ile ilişkilendirilmelidir:

```powershell
Set-OBRetentionPolicy -Policy $NewPolicy -RetentionPolicy $RetentionPolicy
```

```Output
BackupSchedule  : 4:00 PM
                  Saturday, Sunday,
                  Every 1 week(s)
DsList          :
PolicyName      :
RetentionPolicy : Retention Days : 7

                  WeeklyLTRSchedule :
                  Weekly schedule is not set

                  MonthlyLTRSchedule :
                  Monthly schedule is not set

                  YearlyLTRSchedule :
                  Yearly schedule is not set

State           : New
PolicyState     : Valid
```

### <a name="including-and-excluding-files-to-be-backed-up"></a>Yedeklenecek dosyaları ekleme ve hariç tut

Nesne, `OBFileSpec` eklenecek ve yedekte dışlanacak dosyaları tanımlar. Bu, bir makinedeki korumalı dosya ve klasörleri kapsamaya giren kurallar kümesidir. Gerektiğinde olabildiğince çok dosya ekleme veya dışlama kuralına sahip olabilir ve bunları bir ilkeyle ilişkilendirebilirsiniz. Yeni bir OBFileSpec nesnesi oluştururken şunları yapabilirsiniz:

* Eklenecek dosya ve klasörleri belirtin
* Dışlanacak dosya ve klasörleri belirtin
* Yalnızca belirtilen klasördeki üst düzey dosyaların yedeklenip yedeklenmemesi gerekip gerekmediğini bir klasördeki (veya) verilerin özyinelemeli yedeklemesini belirtin.

İkincisi, New-OBFileSpec komutundaki -NonRecursive bayrağı kullanılarak elde edilir.

Aşağıdaki örnekte, C: ve D hacmini yedekleriz ve Windows klasöründeki işletim sistemi ikililerini ve geçici klasörleri hariç tutarız. Bunu yapmak için, [New-OBFileSpec](https://docs.microsoft.com/powershell/module/msonlinebackup/new-obfilespec?view=winserver2012-ps) cmdlet kullanarak iki dosya belirtimi oluşturacağız - biri dahil, diğeri hariç tutulmak üzere. Dosya belirtimleri oluşturulduktan [sonra, Ekle-OBFileSpec](https://docs.microsoft.com/powershell/module/msonlinebackup/add-obfilespec?view=winserver2012-ps) cmdlet kullanarak ilke ile ilişkilendirilsin.

```powershell
$Inclusions = New-OBFileSpec -FileSpec @("C:\", "D:\")
$Exclusions = New-OBFileSpec -FileSpec @("C:\windows", "C:\temp") -Exclude
Add-OBFileSpec -Policy $NewPolicy -FileSpec $Inclusions
```

```Output
BackupSchedule  : 4:00 PM
                  Saturday, Sunday,
                  Every 1 week(s)
DsList          : {DataSource
                  DatasourceId:0
                  Name:C:\
                  FileSpec:FileSpec
                  FileSpec:C:\
                  IsExclude:False
                  IsRecursive:True

                  , DataSource
                  DatasourceId:0
                  Name:D:\
                  FileSpec:FileSpec
                  FileSpec:D:\
                  IsExclude:False
                  IsRecursive:True

                  }
PolicyName      :
RetentionPolicy : Retention Days : 7

                  WeeklyLTRSchedule :
                  Weekly schedule is not set

                  MonthlyLTRSchedule :
                  Monthly schedule is not set

                  YearlyLTRSchedule :
                  Yearly schedule is not set

State           : New
PolicyState     : Valid
```

```powershell
Add-OBFileSpec -Policy $NewPolicy -FileSpec $Exclusions
```

```Output
BackupSchedule  : 4:00 PM
                  Saturday, Sunday,
                  Every 1 week(s)
DsList          : {DataSource
                  DatasourceId:0
                  Name:C:\
                  FileSpec:FileSpec
                  FileSpec:C:\
                  IsExclude:False
                  IsRecursive:True
                  ,FileSpec
                  FileSpec:C:\windows
                  IsExclude:True
                  IsRecursive:True
                  ,FileSpec
                  FileSpec:C:\temp
                  IsExclude:True
                  IsRecursive:True

                  , DataSource
                  DatasourceId:0
                  Name:D:\
                  FileSpec:FileSpec
                  FileSpec:D:\
                  IsExclude:False
                  IsRecursive:True

                  }
PolicyName      :
RetentionPolicy : Retention Days : 7

                  WeeklyLTRSchedule :
                  Weekly schedule is not set

                  MonthlyLTRSchedule :
                  Monthly schedule is not set

                  YearlyLTRSchedule :
                  Yearly schedule is not set

State           : New
PolicyState     : Valid
```

### <a name="applying-the-policy"></a>İlkenin uygulanması

Şimdi ilke nesnesi tamamlandı ve ilişkili bir yedekleme zamanlaması, bekletme ilkesi ve dosyaların bir ekleme/dışlama listesi vardır. Bu ilke artık Azure Yedekleme'nin kullanması için işlenebilir. Yeni oluşturulan ilkeyi uygulamadan önce, [Remove-OBPolicy](https://docs.microsoft.com/powershell/module/msonlinebackup/remove-obpolicy?view=winserver2012-ps) cmdlet kullanarak sunucuyla ilişkili varolan yedekleme ilkeleri olmadığından emin olun. İlkenin kaldırılması onay ister. Onayı atlamak için cmdlet ile `-Confirm:$false` bayrağı kullanın.

```powershell
Get-OBPolicy | Remove-OBPolicy
```

```Output
Microsoft Azure Backup Are you sure you want to remove this backup policy? This will delete all the backed up data. [Y] Yes [A] Yes to All [N] No [L] No to All [S] Suspend [?] Help (default is "Y"):
```

İlke nesnesinin işletilmesi [Set-OBPolicy](https://docs.microsoft.com/powershell/module/msonlinebackup/set-obpolicy?view=winserver2012-ps) cmdlet kullanılarak yapılır. Bu da onay isteyecektir. Onayı atlamak için cmdlet ile `-Confirm:$false` bayrağı kullanın.

```powershell
Set-OBPolicy -Policy $NewPolicy
```

```Output
Microsoft Azure Backup Do you want to save this backup policy ? [Y] Yes [A] Yes to All [N] No [L] No to All [S] Suspend [?] Help (default is "Y"):
BackupSchedule : 4:00 PM Saturday, Sunday, Every 1 week(s)
DsList : {DataSource
         DatasourceId:4508156004108672185
         Name:C:\
         FileSpec:FileSpec
         FileSpec:C:\
         IsExclude:False
         IsRecursive:True,

         FileSpec
         FileSpec:C:\windows
         IsExclude:True
         IsRecursive:True,

         FileSpec
         FileSpec:C:\temp
         IsExclude:True
         IsRecursive:True,

         DataSource
         DatasourceId:4508156005178868542
         Name:D:\
         FileSpec:FileSpec
         FileSpec:D:\
         IsExclude:False
         IsRecursive:True
    }
PolicyName : c2eb6568-8a06-49f4-a20e-3019ae411bac
RetentionPolicy : Retention Days : 7
              WeeklyLTRSchedule :
              Weekly schedule is not set

              MonthlyLTRSchedule :
              Monthly schedule is not set

              YearlyLTRSchedule :
              Yearly schedule is not set
State : Existing PolicyState : Valid
```

[Get-OBPolicy](https://docs.microsoft.com/powershell/module/msonlinebackup/get-obpolicy?view=winserver2012-ps) cmdlet'ini kullanarak varolan yedekleme ilkesinin ayrıntılarını görüntüleyebilirsiniz. Yedekleme zamanlaması için [Get-OBSchedule](https://docs.microsoft.com/powershell/module/msonlinebackup/get-obschedule?view=winserver2012-ps) cmdlet'i ve bekletme ilkeleri için [Get-OBRetentionPolicy](https://docs.microsoft.com/powershell/module/msonlinebackup/get-obretentionpolicy?view=winserver2012-ps) cmdlet'i kullanarak daha fazla ayrıntıya girebilirsiniz

```powershell
Get-OBPolicy | Get-OBSchedule
```

```Output
SchedulePolicyName : 71944081-9950-4f7e-841d-32f0a0a1359a
ScheduleRunDays : {Saturday, Sunday}
ScheduleRunTimes : {16:00:00}
State : Existing
```

```powershell
Get-OBPolicy | Get-OBRetentionPolicy
```

```Output
RetentionDays : 7
RetentionPolicyName : ca3574ec-8331-46fd-a605-c01743a5265e
State : Existing
```

```powershell
Get-OBPolicy | Get-OBFileSpec
```

```Output
FileName : *
FilePath : \?\Volume{b835d359-a1dd-11e2-be72-2016d8d89f0f}\
FileSpec : D:\
IsExclude : False
IsRecursive : True

FileName : *
FilePath : \?\Volume{cdd41007-a22f-11e2-be6c-806e6f6e6963}\
FileSpec : C:\
IsExclude : False
IsRecursive : True

FileName : *
FilePath : \?\Volume{cdd41007-a22f-11e2-be6c-806e6f6e6963}\windows
FileSpec : C:\windows
IsExclude : True
IsRecursive : True

FileName : *
FilePath : \?\Volume{cdd41007-a22f-11e2-be6c-806e6f6e6963}\temp
FileSpec : C:\temp
IsExclude : True
IsRecursive : True
```

### <a name="performing-an-on-demand-backup"></a>İsteğe bağlı yedekleme gerçekleştirme

Yedekleme ilkesi ayarlandıktan sonra, yedeklemeler zamanlamaya göre oluşur. İsteğe bağlı yedeklemeyi tetiklemek, [Start-OBBackup](https://docs.microsoft.com/powershell/module/msonlinebackup/start-obbackup?view=winserver2012-ps) cmdlet kullanılarak da mümkündür:

```powershell
Get-OBPolicy | Start-OBBackup
```

```Output
Initializing
Taking snapshot of volumes...
Preparing storage...
Generating backup metadata information and preparing the metadata VHD...
Data transfer is in progress. It might take longer since it is the first backup and all data needs to be transferred...
Data transfer completed and all backed up data is in the cloud. Verifying data integrity...
Data transfer completed
In progress...
Job completed.
The backup operation completed successfully.
```

## <a name="back-up-windows-server-system-state-in-mabs-agent"></a>MABS aracısında Windows Server System State'i yedekleme

Bu bölüm, MABS aracısında System State'i kurmak için PowerShell komutunu kapsar

### <a name="schedule"></a>Zamanlama

```powershell
$sched = New-OBSchedule -DaysOfWeek Sunday,Monday,Tuesday,Wednesday,Thursday,Friday,Saturday -TimesOfDay 2:00
```

### <a name="retention"></a>Bekletme

```powershell
$rtn = New-OBRetentionPolicy -RetentionDays 32 -RetentionWeeklyPolicy -RetentionWeeks 13 -WeekDaysOfWeek Sunday -WeekTimesOfDay 2:00  -RetentionMonthlyPolicy -RetentionMonths 13 -MonthDaysOfMonth 1 -MonthTimesOfDay 2:00
```

### <a name="configuring-schedule-and-retention"></a>Zamanlama ve bekletme yapılandırma

```powershell
New-OBPolicy | Add-OBSystemState |  Set-OBRetentionPolicy -RetentionPolicy $rtn | Set-OBSchedule -Schedule $sched | Set-OBSystemStatePolicy
 ```

### <a name="verifying-the-policy"></a>İlkeyi doğrulama

```powershell
Get-OBSystemStatePolicy
 ```

## <a name="restore-data-from-azure-backup"></a>Azure Yedekleme'den verileri geri yükleme

Bu bölüm, Azure Yedekleme'den veri kurtarmayı otomatikleştirmek için size yol gösterecektir. Bunu yapmak aşağıdaki adımları içerir:

1. Kaynak hacmini seçin
2. Geri yüklemek için bir yedekleme noktası seçin
3. Geri yükecek bir öğe belirtin
4. Geri yükleme işlemini tetikle

### <a name="picking-the-source-volume"></a>Kaynak hacmini seçme

Bir öğeyi Azure Yedekleme'den geri yüklemek için öncelikle öğenin kaynağını belirlemeniz gerekir. Komutları bir Windows Server veya Windows istemcisi bağlamında yürüttettiğimiziçin, makine zaten tanımlanmıştır. Kaynağı tanımlamada bir sonraki adım, kaynağı içeren birimi tanımlamaktır. Bu makineden yedeklenen birimlerin veya kaynakların listesi [Get-OBRecoverableSource](https://docs.microsoft.com/powershell/module/msonlinebackup/get-obrecoverablesource?view=winserver2012-ps) cmdlet çalıştırılarak alınabilir. Bu komut, bu sunucudan/istemciden yedeklenen tüm kaynaklardan oluşan bir dizi döndürür.

```powershell
$Source = Get-OBRecoverableSource
$Source
```

```Output
FriendlyName : C:\
RecoverySourceName : C:\
ServerName : myserver.microsoft.com

FriendlyName : D:\
RecoverySourceName : D:\
ServerName : myserver.microsoft.com
```

### <a name="choosing-a-backup-point-from-which-to-restore"></a>Geri yükleyirecek bir yedekleme noktası seçme

Uygun parametrelerle [Get-OBRecoverableItem](https://docs.microsoft.com/powershell/module/msonlinebackup/get-obrecoverableitem?view=winserver2012-ps) cmdlet'i çalıştırarak yedek noktaların listesini alırsınız. Örneğimizde, kaynak birim C için en son yedekleme noktasını *seçeriz:* ve belirli bir dosyayı kurtarmak için kullanırız.

```powershell
$Rps = Get-OBRecoverableItem $Source[0]
$Rps
```

```Output

IsDir                : False
ItemNameFriendly     : C:\
ItemNameGuid         : \\?\Volume{297cbf7a-0000-0000-0000-401f00000000}\
LocalMountPoint      : C:\
MountPointName       : C:\
Name                 : C:\
PointInTime          : 10/17/2019 7:52:13 PM
ServerName           : myserver.microsoft.com
ItemSize             :
ItemLastModifiedTime :

IsDir                : False
ItemNameFriendly     : C:\
ItemNameGuid         : \\?\Volume{297cbf7a-0000-0000-0000-401f00000000}\
LocalMountPoint      : C:\
MountPointName       : C:\
Name                 : C:\
PointInTime          : 10/16/2019 7:00:19 PM
ServerName           : myserver.microsoft.com
ItemSize             :
ItemLastModifiedTime :
```

Nesne `$Rps` yedek noktaları bir dizi. İlk öğe en son noktadır ve Nth öğesi en eski noktadır. En son noktayı seçmek için `$Rps[0]`kullanacağız.

### <a name="specifying-an-item-to-restore"></a>Geri yüklanacak bir öğe belirtme

Belirli bir dosyayı geri yüklemek için, dosya adını kök birimine göre belirtin. Örneğin, C:\Test\Cat.job almak için aşağıdaki komutu çalıştırın.

```powershell
$Item = New-OBRecoverableItem $Rps[0] "Test\cat.jpg" $FALSE
$Item
```

```Output
IsDir                : False
ItemNameFriendly     : C:\Test\cat.jpg
ItemNameGuid         :
LocalMountPoint      : C:\
MountPointName       : C:\
Name                 : cat.jpg
PointInTime          : 10/17/2019 7:52:13 PM
ServerName           : myserver.microsoft.com
ItemSize             :
ItemLastModifiedTime : 21-Jun-14 6:43:02 AM

```

### <a name="triggering-the-restore-process"></a>Geri yükleme işlemini tetikleme

Geri yükleme işlemini tetiklemek için öncelikle kurtarma seçeneklerini belirtmemiz gerekir. Bu [New-OBRecoveryOption](https://docs.microsoft.com/powershell/module/msonlinebackup/new-obrecoveryoption?view=winserver2012-ps) cmdlet kullanılarak yapılabilir. Bu örnek için, dosyaları *C:\temp'a*geri yüklemek istediğimizi varsayalım. Ayrıca hedef *klasörC:\temp'ta*zaten var olan dosyaları atlamak istediğimizi varsayalım. Böyle bir kurtarma seçeneği oluşturmak için aşağıdaki komutu kullanın:

```powershell
$RecoveryOption = New-OBRecoveryOption -DestinationPath "C:\temp" -OverwriteType Skip
```

Şimdi `$Item` `Get-OBRecoverableItem` cmdlet çıktısından seçilen [Başlat-OBRecovery](https://docs.microsoft.com/powershell/module/msonlinebackup/start-obrecovery?view=winserver2012-ps) komutunu kullanarak geri yükleme işlemini tetikler:

```powershell
Start-OBRecovery -RecoverableItem $Item -RecoveryOption $RecoveryOption
```

```Output
Estimating size of backup items...
Estimating size of backup items...
Estimating size of backup items...
Estimating size of backup items...
Job completed.
The recovery operation completed successfully.
```

## <a name="uninstalling-the-azure-backup-agent"></a>Azure Yedekleme aracısını kaldırma

Azure Yedekleme aracısını kaldırma aşağıdaki komutu kullanarak yapılabilir:

```powershell
.\MARSAgentInstaller.exe /d /q
```

Aracı ikililerinin makineden uzaklaşması göz önünde bulundurulması gereken bazı sonuçlar doğurur:

* Dosya filtresini makineden kaldırır ve değişikliklerin izlenmesi durdurulur.
* Tüm ilke bilgileri makineden kaldırılır, ancak ilke bilgileri hizmette depolanır.
* Tüm yedekleme zamanlamaları kaldırılır ve başka yedekleme alınmaz.

Ancak, Azure'da depolanan veriler kalır ve sizin yanınıza saklama ilkesi kurulumuna göre korunur. Eski puanlar otomatik olarak yaşlandırılır.

## <a name="remote-management"></a>Uzaktan yönetim

Azure Yedekleme aracısı, ilkeleri ve veri kaynaklarının çevresindeki tüm yönetimler PowerShell aracılığıyla uzaktan yapılabilir. Uzaktan yönetilecek makinenin doğru şekilde hazırlanması gerekir.

Varsayılan olarak, WinRM hizmeti el ile başlatma için yapılandırılır. Başlangıç türü *Otomatik* olarak ayarlanmalıdır ve hizmet başlatılmalıdır. WinRM hizmetinin çalıştığını doğrulamak için Durum özelliğinin değeri *çalışıyor*olmalıdır.

```powershell
Get-Service -Name WinRM
```

```Output
Status   Name               DisplayName
------   ----               -----------
Running  winrm              Windows Remote Management (WS-Manag...
```

PowerShell remoting için yapılandırılmalıdır.

```powershell
Enable-PSRemoting -Force
```

```Output
WinRM is already set up to receive requests on this computer.
WinRM has been updated for remote management.
WinRM firewall exception enabled.
```

```powershell
Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Force
```

Makine artık uzaktan yönetilebilir - aracının kurulumundan başlayarak. Örneğin, aşağıdaki komut dosyası aracıyı uzak makineye kopyalar ve yükler.

```powershell
$DLoc = "\\REMOTESERVER01\c$\Windows\Temp"
$Agent = "\\REMOTESERVER01\c$\Windows\Temp\MARSAgentInstaller.exe"
$Args = "/q"
Copy-Item "C:\Downloads\MARSAgentInstaller.exe" -Destination $DLoc -Force

$Session = New-PSSession -ComputerName REMOTESERVER01
Invoke-Command -Session $Session -Script { param($D, $A) Start-Process -FilePath $D $A -Wait } -ArgumentList $Agent, $Args
```

## <a name="next-steps"></a>Sonraki adımlar

Windows Server/Client için Azure Yedekleme hakkında daha fazla bilgi için:

* [Azure Backup'a giriş](backup-introduction-to-azure-backup.md)
* [Windows Sunucularını Yedekleme](backup-windows-with-mars-agent.md)
