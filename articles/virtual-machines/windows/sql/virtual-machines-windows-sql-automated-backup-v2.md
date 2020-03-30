---
title: SQL Server 2016/2017 Azure VM'ler için Otomatik Yedekleme v2 | Microsoft Dokümanlar
description: Azure'da çalışan SQL Server 2016/2017 VM'leri için Otomatik Yedekleme özelliğini açıklar. Bu makale, Kaynak Yöneticisi'ni kullanan VM'lere özgüdür.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: craigg
tags: azure-resource-manager
ms.assetid: ebd23868-821c-475b-b867-06d4a2e310c7
ms.service: virtual-machines-sql
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 05/03/2018
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: 458012982531e228f7c4968f29e79e8b2e29aa48
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77651449"
---
# <a name="automated-backup-v2-for-azure-virtual-machines-resource-manager"></a>Azure Sanal Makineler için Otomatik Yedekleme v2 (Kaynak Yöneticisi)

> [!div class="op_single_selector"]
> * [SQL Server 2014](virtual-machines-windows-sql-automated-backup.md)
> * [SQL Server 2016/2017](virtual-machines-windows-sql-automated-backup-v2.md)

Otomatik Yedekleme v2, SQL Server 2016/2017 Standard, Enterprise veya Developer sürümlerini çalıştıran bir Azure VM'deki tüm mevcut ve yeni veritabanları için [Yönetilen Yedekleme'yi Microsoft Azure'a](https://msdn.microsoft.com/library/dn449496.aspx) otomatik olarak yapılandırır. Bu, dayanıklı Azure blob depolama kullanan düzenli veritabanı yedeklemelerini yapılandırmanızı sağlar. Otomatik Yedekleme v2 [SQL Server IaaS Agent Extension](virtual-machines-windows-sql-server-agent-extension.md)bağlıdır.

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

## <a name="prerequisites"></a>Ön koşullar
Otomatik Yedekleme v2 kullanmak için aşağıdaki ön koşulları gözden geçirin:

**İşletim Sistemi**:

- Windows Server 2012 R2
- Windows Server 2016

**SQL Server sürümü/sürümü**:

- SQL Server 2016: Geliştirici, Standart veya Kurumsal
- SQL Server 2017: Geliştirici, Standart veya Kurumsal

> [!IMPORTANT]
> Otomatik Yedekleme v2, SQL Server 2016 veya üzeri ile çalışır. SQL Server 2014 kullanıyorsanız, veritabanlarınızı yedeklemek için Otomatik Yedekleme v1'i kullanabilirsiniz. Daha fazla bilgi için [SQL Server 2014 Azure Sanal Makineler için Otomatik Yedekleme'ye](virtual-machines-windows-sql-automated-backup.md)bakın.

**Veritabanı yapılandırması**:

- Hedef veritabanları tam kurtarma modelini kullanmalıdır. Tam kurtarma modelinin yedeklemeler üzerindeki etkisi hakkında daha fazla bilgi için, [Tam Kurtarma Modeli Altında Yedekleme bölümüne](https://technet.microsoft.com/library/ms190217.aspx)bakın.
- Sistem veritabanları tam kurtarma modeli kullanmak zorunda değildir. Ancak, Model veya MSDB için günlük yedeklemelerinin alınmasını istiyorsanız, tam kurtarma modelini kullanmanız gerekir.
- Hedef veritabanları varsayılan SQL Server örneğinde veya [düzgün yüklü](virtual-machines-windows-sql-server-iaas-faq.md#administration) bir adlandırılmış örnekte olmalıdır. 

> [!NOTE]
> Otomatik **Yedekleme, SQL Server IaaS Aracı Uzantısı'na**dayanır. Geçerli SQL sanal makine galerisi görüntüleri varsayılan olarak bu uzantıyı ekleyin. Daha fazla bilgi için [SQL Server IaaS Aracı Uzantısı'na](virtual-machines-windows-sql-server-agent-extension.md)bakın.

## <a name="settings"></a>Ayarlar
Aşağıdaki tabloda Otomatik Yedekleme v2 için yapılandırılabilen seçenekler açıklanmaktadır. Gerçek yapılandırma adımları, Azure portalını veya Azure Windows PowerShell komutlarını kullanıp kullanmadığınıza bağlı olarak değişir.

### <a name="basic-settings"></a>Temel Ayarlar

| Ayar | Aralık (Varsayılan) | Açıklama |
| --- | --- | --- |
| **Otomatik Yedekleme** | Etkinleştirme/Devre Dışı Bırakma (Devre Dışı) | SQL Server 2016/2017 Developer, Standard veya Enterprise çalıştıran bir Azure VM için Otomatik Yedekleme'yi etkinleştiriler veya devre dışı kılabilir. |
| **Bekletme Süresi** | 1-30 gün (30 gün) | Yedeklemeleri tutmak için gün sayısı. |
| **Depolama Hesabı** | Azure depolama hesabı | Otomatik Yedekleme dosyalarını blob depolamada depolamak için kullanılacak bir Azure depolama hesabı. Tüm yedekleme dosyalarını depolamak için bu konumda bir kapsayıcı oluşturulur. Yedekleme dosyası adlandırma kuralı tarih, saat ve veritabanı GUID içerir. |
| **Şifreleme** |Etkinleştirme/Devre Dışı Bırakma (Devre Dışı) | Şifrelemeyi etkinleştirir veya devre dışı kılabilir. Şifreleme etkinleştirildiğinde, yedeklemeyi geri yüklemek için kullanılan sertifikalar belirtilen depolama hesabında bulunur. Aynı adlandırma kuralına sahip aynı **otomatik yedekleme** kapsayıcısını kullanır. Parola değişirse, bu parolayla yeni bir sertifika oluşturulur, ancak önceki yedeklemeleri geri yüklemek için eski sertifika kalır. |
| **Parola** |Parola metni | Şifreleme anahtarları için bir şifre. Bu parola yalnızca şifreleme etkinse gereklidir. Şifreli bir yedeklemeyi geri yüklemek için, yedeklemenin alındığı sırada kullanılan doğru parolaya ve ilgili sertifikaya sahip olmalısınız. |

### <a name="advanced-settings"></a>Gelişmiş Ayarlar

| Ayar | Aralık (Varsayılan) | Açıklama |
| --- | --- | --- |
| **Sistem Veritabanı Yedekleri** | Etkinleştirme/Devre Dışı Bırakma (Devre Dışı) | Etkinleştirildiğinde, bu özellik sistem veritabanlarını da yedekler: Master, MSDB ve Model. MSDB ve Model veritabanları için, günlük yedeklemelerinin alınmasını istiyorsanız, tam kurtarma modunda olduklarını doğrulayın. Günlük yedekleri Master için asla alınmaz. TempDB için de yedek alınmaz. |
| **Yedekleme Zamanlaması** | Manuel/Otomatik (Otomatik) | Varsayılan olarak, yedekleme zamanlaması günlük büyümesine göre otomatik olarak belirlenir. El ile yedekleme zamanlaması, kullanıcının yedeklemeler için zaman penceresini belirtmesine olanak tanır. Bu durumda, yedeklemeler yalnızca belirtilen frekansta ve belirli bir günün belirtilen zaman penceresinde gerçekleşir. |
| **Tam yedekleme frekansı** | Günlük/Haftalık | Tam yedekleme sıklığı. Her iki durumda da, tam yedeklemeler bir sonraki zamanlanan zamanlama süresi sırasında başlar. Haftalık seçildiğinde, yedeklemeler tüm veritabanları başarıyla yedeklenene kadar birden çok güne yayılabilir. |
| **Tam yedekleme başlangıç saati** | 00:00 – 23:00 (01:00) | Tam yedeklemelerin gerçekleşebileceği belirli bir günün başlangıç saati. |
| **Tam yedekleme zamanı penceresi** | 1 – 23 saat (1 saat) | Tam yedeklemelerin gerçekleşebileceği belirli bir günün zaman penceresinin süresi. |
| **Günlük yedekleme frekansı** | 5 – 60 dakika (60 dakika) | Günlük yedeklemelerinin sıklığı. |

## <a name="understanding-full-backup-frequency"></a>Tam yedekleme sıklığını anlama
Günlük ve haftalık tam yedeklemeler arasındaki farkı anlamak önemlidir. Aşağıdaki iki örnek senaryoyu göz önünde bulundurun.

### <a name="scenario-1-weekly-backups"></a>Senaryo 1: Haftalık yedeklemeler
Bir dizi büyük veritabanları içeren bir SQL Server VM'ine sahipsiniz.

Pazartesi günü, aşağıdaki ayarları ile Otomatik Yedekleme v2 etkinleştirin:

- Yedekleme çizelgesi: **El kitabı**
- Tam yedekleme frekansı: **Haftalık**
- Tam yedekleme başlangıç saati: **01:00**
- Tam yedekleme süresi penceresi: **1 saat**

Bu, bir sonraki kullanılabilir yedekleme penceresinin Salı günü saat 01:00'de 1 saat olduğu anlamına gelir. O anda, Otomatik Yedekleme veritabanlarınızı teker teker yedeklemeye başlar. Bu senaryoda, veritabanlarınız ilk çift veritabanları için tam yedeklemelerin tamamlanacak kadar büyüktür. Ancak, bir saat sonra tüm veritabanları yedeklenmedi.

Bu durumda, Otomatik Yedekleme sonraki gün kalan veritabanlarını yedeklemeye başlar, Çarşamba günü saat 01:00'de bir saat. Tüm veritabanları bu süre içinde yedeklenmiş değilse, aynı anda ertesi gün tekrar çalışır. Bu, tüm veritabanları başarıyla yedeklenene kadar devam eder.

Salı günü tekrar ulaştıktan sonra, Otomatik Yedekleme tüm veritabanlarını yeniden yedeklemeye başlar.

Bu senaryo, Otomatik Yedekleme'nin yalnızca belirtilen zaman penceresinde çalıştığını ve her veritabanının haftada bir kez yedeklediğini gösterir. Bu, tüm yedeklemelerin tek bir günde tamamlanmasının mümkün olmadığı durumlarda yedeklemelerin birden çok güne yayAbileceğinizi de gösterir.

### <a name="scenario-2-daily-backups"></a>Senaryo 2: Günlük yedeklemeler
Bir dizi büyük veritabanları içeren bir SQL Server VM'ine sahipsiniz.

Pazartesi günü, aşağıdaki ayarları ile Otomatik Yedekleme v2 etkinleştirin:

- Yedekleme çizelgesi: El kitabı
- Tam yedekleme frekansı: Günlük
- Tam yedekleme başlangıç saati: 22:00
- Tam yedekleme süresi penceresi: 6 saat

Bu, bir sonraki kullanılabilir yedekleme penceresinin Pazartesi saat 22:00'de 6 saat boyunca olduğu anlamına gelir. O anda, Otomatik Yedekleme veritabanlarınızı teker teker yedeklemeye başlar.

Daha sonra, Salı günü saat 10'da 6 saat boyunca, tüm veritabanlarının tam yedeklemeleri yeniden başlar.

> [!IMPORTANT]
> Günlük yedeklemeleri zamanlarken, tüm veritabanlarının bu süre içinde yedeklenebileceğinden emin olmak için geniş bir zaman penceresi zamanlamanız önerilir. Bu, yedeklemeniz gereken büyük miktarda veri olması durumunda özellikle önemlidir.

## <a name="configure-new-vms"></a>Yeni VM'leri yapılandırma

Kaynak Yöneticisi dağıtım modelinde yeni bir SQL Server 2016 veya 2017 Sanal Makine oluştururken Otomatik Yedekleme v2'yi yapılandırmak için Azure portalını kullanın.

SQL **Server ayarları** sekmesinde, **Otomatik yedekleme**altında **Etkinleştir'i** seçin. Aşağıdaki Azure portalı ekran görüntüsü **SQL Otomatik Yedekleme** ayarlarını gösterir.

![Azure portalında SQL Otomatik Yedekleme yapılandırması](./media/virtual-machines-windows-sql-automated-backup-v2/automated-backup-blade.png)

> [!NOTE]
> Otomatik Yedekleme v2 varsayılan olarak devre dışı bırakılır.

## <a name="configure-existing-vms"></a>Varolan VM'leri yapılandırma

[!INCLUDE [windows-virtual-machines-sql-use-new-management-blade](../../../../includes/windows-virtual-machines-sql-new-resource.md)]

Mevcut SQL Server sanal makineleri için [SQL sanal makineler kaynağına](virtual-machines-windows-sql-manage-portal.md#access-the-sql-virtual-machines-resource) gidin ve ardından otomatik yedeklemelerinizi yapılandırmak için **Yedekler'i** seçin.

![Mevcut VM'ler için SQL Otomatik Yedekleme](./media/virtual-machines-windows-sql-automated-backup-v2/sql-server-configuration.png)


Bittiğinde, değişikliklerinizi kaydetmek için **Yedekler** ayarları sayfasının altındaki **Uygula** düğmesini tıklatın.

Otomatik Yedekleme'yi ilk kez etkinleştiriyorsanız, Azure arka planda SQL Server IaaS Aracısını yapılandırır. Bu süre zarfında, Azure portalı Otomatik Yedekleme'nin yapılandırılmış olduğunu göstermeyebilir. Aracının yüklenmesi, yapılandırılması için birkaç dakika bekleyin. Bundan sonra, Azure portalı yeni ayarları yansıtır.

## <a name="configure-with-powershell"></a>PowerShell ile yapılandırın

Otomatik Yedekleme v2 yapılandırmak için PowerShell kullanabilirsiniz. Başlamadan önce şunları yapmalısın:

- [En son Azure PowerShell'i indirin ve kurun.](https://aka.ms/webpi-azps)
- Windows PowerShell'i açın ve hesabınızla **Bağlantı-AzAccount** komutuyla ilişkilendirin.

[!INCLUDE [updated-for-az.md](../../../../includes/updated-for-az.md)]

### <a name="install-the-sql-iaas-extension"></a>SQL IaaS Uzantısını Yükleyin
Azure portalından bir SQL Server sanal makine saysanız, SQL Server IaaS Uzantısı zaten yüklenmiş olmalıdır. **Get-AzVM** komutunu arayarak ve **Uzantılar** özelliğini inceleyerek VM'iniz için yüklü olup olmadığını belirleyebilirsiniz.

```powershell
$vmname = "vmname"
$resourcegroupname = "resourcegroupname"

(Get-AzVM -Name $vmname -ResourceGroupName $resourcegroupname).Extensions 
```

SQL Server IaaS Agent uzantısı yüklüyse, "SqlIaaSAgent" veya "SQLIaaSExtension" olarak listelenmiş olarak görmeniz gerekir. Uzantısı için **ProvisioningState** de "Başarılı" göstermelidir. 

Yüklenmezse veya sağlanmazsa, aşağıdaki komutla yükleyebilirsiniz. VM adı ve kaynak grubuna ek olarak, VM'nizin bulunduğu bölgeyi **($region)** da belirtmeniz gerekir.

```powershell
$region = "EASTUS2"
Set-AzVMSqlServerExtension -VMName $vmname `
    -ResourceGroupName $resourcegroupname -Name "SQLIaasExtension" `
    -Version "2.0" -Location $region 
```

### <a name="verify-current-settings"></a><a id="verifysettings"></a>Geçerli ayarları doğrulama
Sağlama sırasında otomatik yedeklemeyi etkinleştirdiyseniz, geçerli yapılandırmanızı denetlemek için PowerShell'i kullanabilirsiniz. **Get-AzVMSqlServerExtension** komutunu çalıştırın ve **Otomatik Yedekleme Ayarları** özelliğini inceleyin:

```powershell
(Get-AzVMSqlServerExtension -VMName $vmname -ResourceGroupName $resourcegroupname).AutoBackupSettings
```

Aşağıdakilere benzer çıktı almalısınız:

```
Enable                      : True
EnableEncryption            : False
RetentionPeriod             : 30
StorageUrl                  : https://test.blob.core.windows.net/
StorageAccessKey            :  
Password                    : 
BackupSystemDbs             : False
BackupScheduleType          : Manual
FullBackupFrequency         : WEEKLY
FullBackupStartTime         : 2
FullBackupWindowHours       : 2
LogBackupFrequency          : 60
```

Çıktınız **Enable'ın** **False**olarak ayarlı olduğunu gösteriyorsa, otomatik yedeklemeyi etkinleştirmeniz gerekir. İyi haber, Otomatik Yedekleme'yi aynı şekilde etkinleştirip yapılandırmanızdır. Bu bilgiler için sonraki bölüme bakın.

> [!NOTE] 
> Değişiklik yaptıktan hemen sonra ayarları denetlerseniz, eski yapılandırma değerlerini geri almanız mümkündür. Birkaç dakika bekleyin ve değişikliklerinizin uygulandığından emin olmak için ayarları tekrar kontrol edin.

### <a name="configure-automated-backup-v2"></a>Otomatik Yedekleme v2 yapılandırma
PowerShell'i Otomatik Yedekleme'yi etkinleştirmek ve yapılandırmasını ve davranışını istediğiniz zaman değiştirmek için kullanabilirsiniz. 

İlk olarak, yedek dosyalar için bir depolama hesabı seçin veya oluşturun. Aşağıdaki komut dosyası bir depolama hesabı seçer veya yoksa oluşturur.

```powershell
$storage_accountname = "yourstorageaccount"
$storage_resourcegroupname = $resourcegroupname

$storage = Get-AzStorageAccount -ResourceGroupName $resourcegroupname `
    -Name $storage_accountname -ErrorAction SilentlyContinue
If (-Not $storage)
    { $storage = New-AzStorageAccount -ResourceGroupName $storage_resourcegroupname `
    -Name $storage_accountname -SkuName Standard_GRS -Location $region } 
```

> [!NOTE]
> Otomatik Yedekleme, yedeklemelerin premium depolamaalanında depolanmasını desteklemez, ancak Premium Depolama kullanan VM disklerinden yedekleme alabilir.

Ardından, Yedeklemeleri Azure depolama hesabında depolamak için Otomatik Yedekleme v2 ayarlarını etkinleştirmek ve yapılandırmak için **Yeni-AzVMSqlSqlAutoBackupConfig** komutunu kullanın. Bu örnekte, yedeklemeler 10 gün boyunca saklanacak şekilde ayarlanmışolur. Sistem veritabanı yedeklemeleri etkinleştirildi. Tam yedeklemeler haftalık olarak zamanlanır ve iki saat boyunca saat 20:00'de başlar. Günlük yedeklemeleri her 30 dakikada bir zamanlanır. İkinci komut, **Set-AzVMSqlServerExtension**, bu ayarları ile belirtilen Azure VM güncelleştirir.

```powershell
$autobackupconfig = New-AzVMSqlServerAutoBackupConfig -Enable `
    -RetentionPeriodInDays 10 -StorageContext $storage.Context `
    -ResourceGroupName $storage_resourcegroupname -BackupSystemDbs `
    -BackupScheduleType Manual -FullBackupFrequency Weekly `
    -FullBackupStartHour 20 -FullBackupWindowInHours 2 `
    -LogBackupFrequencyInMinutes 30 

Set-AzVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
    -VMName $vmname -ResourceGroupName $resourcegroupname 
```

SQL Server IaaS Aracısını yüklemek ve yapılandırmak birkaç dakika sürebilir. 

Şifrelemeyi etkinleştirmek için, Önceki komut dosyasını değiştirerek **SertifikaParola** parametresi için bir parola (güvenli dize) ile birlikte **Şifrelemeyi Etkinleştirme** parametresini geçirin. Aşağıdaki komut dosyası, önceki örnekte Otomatik Yedekleme ayarlarını etkinleştiriyor ve şifreleme ekler.

```powershell
$password = "P@ssw0rd"
$encryptionpassword = $password | ConvertTo-SecureString -AsPlainText -Force  

$autobackupconfig = New-AzVMSqlServerAutoBackupConfig -Enable `
    -EnableEncryption -CertificatePassword $encryptionpassword `
    -RetentionPeriodInDays 10 -StorageContext $storage.Context `
    -ResourceGroupName $storage_resourcegroupname -BackupSystemDbs `
    -BackupScheduleType Manual -FullBackupFrequency Weekly `
    -FullBackupStartHour 20 -FullBackupWindowInHours 2 `
    -LogBackupFrequencyInMinutes 30 

Set-AzVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
    -VMName $vmname -ResourceGroupName $resourcegroupname
```

Ayarlarınızın uygulandığını doğrulamak için [Otomatik Yedekleme yapılandırmasını doğrulayın.](#verifysettings)

### <a name="disable-automated-backup"></a>Otomatik Yedeklemeyi Devre Dışı
Otomatik Yedekleme'yi devre dışı kılmış için, **New-AzVMSqlSqlAutoBackupConfig** komutuna **-Enable** parametresi olmadan aynı komut dosyasını çalıştırın. **-Enable** parametresinin yokluğu, özelliği devre dışı bırakabilmek için komutu bildirir. Yüklemede olduğu gibi, Otomatik Yedekleme'yi devre dışı bırakın birkaç dakika sürebilir.

```powershell
$autobackupconfig = New-AzVMSqlServerAutoBackupConfig -ResourceGroupName $storage_resourcegroupname

Set-AzVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
    -VMName $vmname -ResourceGroupName $resourcegroupname
```

### <a name="example-script"></a>Örnek betik
Aşağıdaki komut dosyası, VM'niz için Otomatik Yedekleme'yi etkinleştirmek ve yapılandırmak için özelleştirebileceğiniz bir değişken kümesi sağlar. Sizin durumunuzda, komut dosyasını gereksinimlerinize göre özelleştirmeniz gerekebilir. Örneğin, sistem veritabanlarının yedeklemesini devre dışı betmek veya şifrelemeyi etkinleştirmek istiyorsanız değişiklik yapmanız gerekir.

```powershell
$vmname = "yourvmname"
$resourcegroupname = "vmresourcegroupname"
$region = "Azure region name such as EASTUS2"
$storage_accountname = "storageaccountname"
$storage_resourcegroupname = $resourcegroupname
$retentionperiod = 10
$backupscheduletype = "Manual"
$fullbackupfrequency = "Weekly"
$fullbackupstarthour = "20"
$fullbackupwindow = "2"
$logbackupfrequency = "30"

# ResourceGroupName is the resource group which is hosting the VM where you are deploying the SQL IaaS Extension 

Set-AzVMSqlServerExtension -VMName $vmname `
    -ResourceGroupName $resourcegroupname -Name "SQLIaasExtension" `
    -Version "2.0" -Location $region

# Creates/use a storage account to store the backups

$storage = Get-AzStorageAccount -ResourceGroupName $resourcegroupname `
    -Name $storage_accountname -ErrorAction SilentlyContinue
If (-Not $storage)
    { $storage = New-AzStorageAccount -ResourceGroupName $storage_resourcegroupname `
    -Name $storage_accountname -SkuName Standard_GRS -Location $region }

# Configure Automated Backup settings

$autobackupconfig = New-AzVMSqlServerAutoBackupConfig -Enable `
    -RetentionPeriodInDays $retentionperiod -StorageContext $storage.Context `
    -ResourceGroupName $storage_resourcegroupname -BackupSystemDbs `
    -BackupScheduleType $backupscheduletype -FullBackupFrequency $fullbackupfrequency `
    -FullBackupStartHour $fullbackupstarthour -FullBackupWindowInHours $fullbackupwindow `
    -LogBackupFrequencyInMinutes $logbackupfrequency

# Apply the Automated Backup settings to the VM

Set-AzVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
    -VMName $vmname -ResourceGroupName $resourcegroupname
```

## <a name="monitoring"></a>İzleme

SQL Server 2016/2017'de Otomatik Yedekleme'yi izlemek için iki ana seçeneğiniz vardır. Otomatik Yedekleme, SQL Server Yönetilen Yedekleme özelliğini kullandığından, her ikisi için de aynı izleme teknikleri geçerlidir.

Öncelikle [msdb.managed_backup.sp_get_backup_diagnostics](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/managed-backup-sp-get-backup-diagnostics-transact-sql)numaralı telefonu arayarak durumu yoklayabilirsiniz. Veya [msdb.managed_backup.fn_get_health_status](https://docs.microsoft.com/sql/relational-databases/system-functions/managed-backup-fn-get-health-status-transact-sql) tablo değerindeki işlevini sorgula.

Başka bir seçenek bildirimler için yerleşik Veritabanı Posta özelliğinden yararlanmaktır.

1. **SSMBackup2WANotificationEmailIds** parametresine bir e-posta adresi atamak için [msdb.managed_backup.sp_set_parameter](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/managed-backup-sp-set-parameter-transact-sql) depolanan yordamı arayın. 
1. [SendGrid'in](../../../sendgrid-dotnet-how-to-send-email.md) e-postaları Azure VM'den göndermesini etkinleştirin.
1. Veritabanı Postasını yapılandırmak için SMTP sunucusunu ve kullanıcı adını kullanın. Veritabanı Postasını SQL Server Management Studio'da veya Transact-SQL komutlarıyla yapılandırabilirsiniz. Daha fazla bilgi için [Veritabanı Postası'na](https://docs.microsoft.com/sql/relational-databases/database-mail/database-mail)bakın.
1. [Veritabanı Postasını kullanmak için SQL Server Agent'ı yapılandırın.](https://docs.microsoft.com/sql/relational-databases/database-mail/configure-sql-server-agent-mail-to-use-database-mail)
1. SMTP bağlantı noktasına hem yerel VM güvenlik duvarı hem de VM için ağ güvenlik grubu aracılığıyla izin verildiğini doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar
Otomatik Yedekleme v2, Azure VM'lerinde Yönetilen Yedekleme'yi yapılandırır. Bu nedenle, davranışı ve sonuçlarını anlamak [için Yönetilen Yedekleme için belgeleri gözden geçirmek](https://msdn.microsoft.com/library/dn449496.aspx) önemlidir.

Aşağıdaki makalede Azure VM'lerde SQL Server için ek yedekleme ve geri yükleme kılavuzu bulabilirsiniz: [Azure Sanal Makinelerde SQL Server için Yedekleme ve Geri Yükleme.](virtual-machines-windows-sql-backup-recovery.md)

Mevcut diğer otomasyon görevleri hakkında daha fazla bilgi için [SQL Server IaaS Agent Extension'a](virtual-machines-windows-sql-server-agent-extension.md)bakın.

Azure VM'lerde SQL Server'ı çalıştırma hakkında daha fazla bilgi için [Azure Sanal Makinelere genel bakışta SQL Server'a](virtual-machines-windows-sql-server-iaas-overview.md)bakın.

