---
title: SQL Server 2014 Azure Sanal Makineler için Otomatik Yedekleme
description: Azure'da çalışan SQL Server 2014 VM'leri için Otomatik Yedekleme özelliğini açıklar. Bu makale, Kaynak Yöneticisi'ni kullanan VM'lere özgüdür.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: craigg
tags: azure-resource-manager
ms.assetid: bdc63fd1-db49-4e76-87d5-b5c6a890e53c
ms.service: virtual-machines-sql
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 05/03/2018
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: c7dea85d8de17a0f65e6e73b5b5fbe619d464d3d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77650359"
---
# <a name="automated-backup-for-sql-server-2014-virtual-machines-resource-manager"></a>SQL Server 2014 Sanal Makineler için Otomatik Yedekleme (Kaynak Yöneticisi)

> [!div class="op_single_selector"]
> * [SQL Server 2014](virtual-machines-windows-sql-automated-backup.md)
> * [SQL Server 2016/2017](virtual-machines-windows-sql-automated-backup-v2.md)

Otomatik Yedekleme, SQL Server 2014 Standard veya Enterprise çalıştıran bir Azure VM'deki mevcut ve yeni veritabanları için [Yönetilen Yedekleme'yi Microsoft Azure'a](https://msdn.microsoft.com/library/dn449496.aspx) otomatik olarak yapılandırır. Bu, dayanıklı Azure blob depolama kullanan düzenli veritabanı yedeklemelerini yapılandırmanızı sağlar. Otomatik [Yedekleme, SQL Server IaaS Aracı Uzantısı'na](virtual-machines-windows-sql-server-agent-extension.md)bağlıdır.

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

## <a name="prerequisites"></a>Ön koşullar
Otomatik Yedekleme'yi kullanmak için aşağıdaki ön koşulları göz önünde bulundurun:

**İşletim Sistemi**:

- Windows Server 2012
- Windows Server 2012 R2
- Windows Server 2016

**SQL Server sürümü/sürümü**:

- SQL Server 2014 Standardı
- SQL Server 2014 Kurumsal

> [!IMPORTANT]
> Otomatik Yedekleme, SQL Server 2014 ile çalışır. SQL Server 2016/2017 kullanıyorsanız, veritabanlarınızı yedeklemek için Automated Backup v2'yi kullanabilirsiniz. Daha fazla bilgi için [SQL Server 2016 Azure Sanal Makineler için Otomatik Yedekleme v2'ye](virtual-machines-windows-sql-automated-backup-v2.md)bakın.

**Veritabanı yapılandırması**:

- Hedef veritabanları tam kurtarma modelini kullanmalıdır. Tam kurtarma modelinin yedeklemeler üzerindeki etkisi hakkında daha fazla bilgi için, [Tam Kurtarma Modeli Altında Yedekleme bölümüne](https://technet.microsoft.com/library/ms190217.aspx)bakın.
- Hedef veritabanları varsayılan SQL Server örneğinde olmalıdır. SQL Server IaaS Uzantısı adlandırılmış örnekleri desteklemez.

> [!NOTE]
> Otomatik Yedekleme, SQL Server IaaS Aracı Uzantısı'na dayanır. Geçerli SQL sanal makine galerisi görüntüleri varsayılan olarak bu uzantıyı ekleyin. Daha fazla bilgi için [SQL Server IaaS Aracı Uzantısı'na](virtual-machines-windows-sql-server-agent-extension.md)bakın.

## <a name="settings"></a>Ayarlar

Aşağıdaki tabloda Otomatik Yedekleme için yapılandırılabilen seçenekler açıklanmaktadır. Gerçek yapılandırma adımları, Azure portalını veya Azure Windows PowerShell komutlarını kullanıp kullanmadığınıza bağlı olarak değişir.

| Ayar | Aralık (Varsayılan) | Açıklama |
| --- | --- | --- |
| **Otomatik Yedekleme** | Etkinleştirme/Devre Dışı Bırakma (Devre Dışı) | SQL Server 2014 Standard veya Enterprise çalıştıran bir Azure VM için Otomatik Yedekleme'yi etkinleştiriler veya devre dışı kılabilir. |
| **Bekletme Süresi** | 1-30 gün (30 gün) | Yedeklemeyi tutmak için gün sayısı. |
| **Depolama Hesabı** | Azure depolama hesabı | Otomatik Yedekleme dosyalarını blob depolamada depolamak için kullanılacak bir Azure depolama hesabı. Tüm yedekleme dosyalarını depolamak için bu konumda bir kapsayıcı oluşturulur. Yedekleme dosyası adlandırma kuralı tarih, saat ve makine adını içerir. |
| **Şifreleme** | Etkinleştirme/Devre Dışı Bırakma (Devre Dışı) | Şifrelemeyi etkinleştirir veya devre dışı kılabilir. Şifreleme etkinleştirildiğinde, yedeklemeyi geri yüklemek için kullanılan sertifikalar, aynı adlandırma `automaticbackup` kuralını kullanarak aynı kapsayıcıdaki belirtilen depolama hesabında bulunur. Parola değişirse, bu parolayla yeni bir sertifika oluşturulur, ancak önceki yedeklemeleri geri yüklemek için eski sertifika kalır. |
| **Parola** | Parola metni | Şifreleme anahtarları için bir şifre. Bu yalnızca şifreleme etkinse gereklidir. Şifreli bir yedeklemeyi geri yüklemek için, yedeklemenin alındığı sırada kullanılan doğru parolaya ve ilgili sertifikaya sahip olmalısınız. |


## <a name="configure-new-vms"></a>Yeni VM'leri yapılandırma

Kaynak Yöneticisi dağıtım modelinde yeni bir SQL Server 2014 Sanal Makine oluştururken Otomatik Yedekleme'yi yapılandırmak için Azure portalını kullanın.

SQL **Server ayarları** sekmesinde, **Otomatik yedeklemeye** gidin ve **Etkinleştir'i**seçin. Aşağıdaki Azure portalı ekran görüntüsü **SQL Otomatik Yedekleme** ayarlarını gösterir.

![Azure portalında SQL Otomatik Yedekleme yapılandırması](./media/virtual-machines-windows-sql-automated-backup/azure-sql-arm-autobackup.png)

## <a name="configure-existing-vms"></a>Varolan VM'leri yapılandırma

[!INCLUDE [windows-virtual-machines-sql-use-new-management-blade](../../../../includes/windows-virtual-machines-sql-new-resource.md)]

Mevcut SQL Server sanal makineleri için, otomatik yedeklemeleri etkinleştirebilir ve devre dışı kılabilir, bekletme süresini değiştirebilir, depolama hesabını belirtebilir ve Azure portalından şifrelemeyi etkinleştirebilirsiniz. 

SQL Server 2014 sanal makineniz için [SQL sanal makineler kaynağına](virtual-machines-windows-sql-manage-portal.md#access-the-sql-virtual-machines-resource) gidin ve ardından **Yedekler'i**seçin. 

![Mevcut VM'ler için SQL Otomatik Yedekleme](./media/virtual-machines-windows-sql-automated-backup/azure-sql-rm-autobackup-existing-vms.png)

Bittiğinde, değişikliklerinizi kaydetmek için **Yedekler** sayfasının altındaki **Uygula** düğmesini seçin.

Otomatik Yedekleme'yi ilk kez etkinleştiriyorsanız, Azure arka planda SQL Server IaaS Aracısını yapılandırır. Bu süre zarfında, Azure portalı Otomatik Yedekleme'nin yapılandırılmış olduğunu göstermeyebilir. Aracının yüklenmesi ve yapılandırılması için birkaç dakika bekleyin. Bundan sonra, Azure portalı yeni ayarları yansıtır.

> [!NOTE]
> Otomatik Yedekleme'yi şablon kullanarak da yapılandırabilirsiniz. Daha fazla bilgi için Otomatik [Yedekleme için Azure hızlı başlatma şablonuna](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-sql-existing-autobackup-update)bakın.

## <a name="configure-with-powershell"></a>PowerShell ile yapılandırın

Otomatik Yedekleme'yi yapılandırmak için PowerShell'i kullanabilirsiniz. Başlamadan önce şunları yapmalısın:

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

Yüklenmezse veya sağlanmazsa, aşağıdaki komutla yükleyebilirsiniz. VM adı ve kaynak grubuna ek olarak, VM'nizin bulunduğu bölgeyi **($region)** da belirtmeniz gerekir. SQL Server VM'nizin lisans türünü belirterek, [Azure Karma Avantajı](https://azure.microsoft.com/pricing/hybrid-benefit/)üzerinden kullandıkça öde veya kendi lisansınızı getir arasında seçim yapmayı belirleyin. Lisanslama hakkında daha fazla bilgi için lisans [lama modeline](virtual-machines-windows-sql-ahb.md)bakın. 

```powershell
New-AzSqlVM  -Name $vmname `
    -ResourceGroupName $resourcegroupname `
    -Location $region -LicenseType <PAYG/AHUB>
```

> [!IMPORTANT]
> Uzantı zaten yüklenmediyse, uzantıyükleme SQL Server hizmetini yeniden başlatır.

### <a name="verify-current-settings"></a><a id="verifysettings"></a>Geçerli ayarları doğrulama

Sağlama sırasında otomatik yedeklemeyi etkinleştirdiyseniz, geçerli yapılandırmanızı denetlemek için PowerShell'i kullanabilirsiniz. **Get-AzVMSqlServerExtension** komutunu çalıştırın ve **Otomatik Yedekleme Ayarları** özelliğini inceleyin:

```powershell
(Get-AzVMSqlServerExtension -VMName $vmname -ResourceGroupName $resourcegroupname).AutoBackupSettings
```

Aşağıdakilere benzer çıktı almalısınız:

```
Enable                      : False
EnableEncryption            : False
RetentionPeriod             : -1
StorageUrl                  : NOTSET
StorageAccessKey            : 
Password                    : 
BackupSystemDbs             : False
BackupScheduleType          : 
FullBackupFrequency         : 
FullBackupStartTime         : 
FullBackupWindowHours       : 
LogBackupFrequency          : 
```

Çıktınız **Enable'ın** **False**olarak ayarlı olduğunu gösteriyorsa, otomatik yedeklemeyi etkinleştirmeniz gerekir. İyi haber, Otomatik Yedekleme'yi aynı şekilde etkinleştirip yapılandırmanızdır. Bu bilgiler için sonraki bölüme bakın.

> [!NOTE] 
> Değişiklik yaptıktan hemen sonra ayarları denetlerseniz, eski yapılandırma değerlerini geri almanız mümkündür. Birkaç dakika bekleyin ve değişikliklerinizin uygulandığından emin olmak için ayarları tekrar kontrol edin.

### <a name="configure-automated-backup"></a>Otomatik Yedeklemeyi Yapılandırma
PowerShell'i Otomatik Yedekleme'yi etkinleştirmek ve yapılandırmasını ve davranışını istediğiniz zaman değiştirmek için kullanabilirsiniz.

İlk olarak, yedekleme dosyaları için bir depolama hesabı seçin veya oluşturun. Aşağıdaki komut dosyası bir depolama hesabı seçer veya yoksa oluşturur.

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

Ardından, Yedeklemeleri Azure depolama hesabında depolamak için Otomatik Yedekleme ayarlarını etkinleştirmek ve yapılandırmak için **Yeni-AzVMSqlSqlAutoBackupConfig** komutunu kullanın. Bu örnekte, yedeklemeler 10 gün boyunca saklanır. İkinci komut, **Set-AzVMSqlServerExtension**, bu ayarları ile belirtilen Azure VM güncelleştirir.

```powershell
$autobackupconfig = New-AzVMSqlServerAutoBackupConfig -Enable `
    -RetentionPeriodInDays 10 -StorageContext $storage.Context `
    -ResourceGroupName $storage_resourcegroupname

Set-AzVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
    -VMName $vmname -ResourceGroupName $resourcegroupname
```

SQL Server IaaS Aracısını yüklemek ve yapılandırmak birkaç dakika sürebilir.

> [!NOTE]
> **New-AzVMSqlServerAutoBackupConfig** için yalnızca SQL Server 2016 ve Automated Backup v2 için geçerli olan diğer ayarlar vardır. SQL Server 2014 aşağıdaki ayarları desteklemez: **BackupSystemDbs**, **BackupScheduleType**, **FullBackupFrequency**, **FullBackupStartHour**, **FullBackupWindowInHours**, ve **LogBackupFrequencyInMinutes**. Bu ayarları bir SQL Server 2014 sanal makinesinde yapılandırmaya çalışırsanız, hata yoktur, ancak ayarlar uygulanmaz. Bu ayarları bir SQL Server 2016 sanal makinesinde kullanmak istiyorsanız, [SQL Server 2016 Azure Sanal Makineler için Otomatik Yedekleme v2'ye](virtual-machines-windows-sql-automated-backup-v2.md)bakın.

Şifrelemeyi etkinleştirmek için, Önceki komut dosyasını değiştirerek **SertifikaParola** parametresi için bir parola (güvenli dize) ile birlikte **Şifrelemeyi Etkinleştirme** parametresini geçirin. Aşağıdaki komut dosyası, önceki örnekte Otomatik Yedekleme ayarlarını etkinleştiriyor ve şifreleme ekler.

```powershell
$password = "P@ssw0rd"
$encryptionpassword = $password | ConvertTo-SecureString -AsPlainText -Force

$autobackupconfig = New-AzVMSqlServerAutoBackupConfig -Enable `
    -EnableEncryption -CertificatePassword $encryptionpassword `
    -RetentionPeriodInDays 10 -StorageContext $storage.Context `
    -ResourceGroupName $storage_resourcegroupname

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
    -ResourceGroupName $storage_resourcegroupname

# Apply the Automated Backup settings to the VM

Set-AzVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
    -VMName $vmname -ResourceGroupName $resourcegroupname
```

## <a name="monitoring"></a>İzleme

SQL Server 2014'te Otomatik Yedekleme'yi izlemek için iki ana seçeneğiniz vardır. Otomatik Yedekleme, SQL Server Yönetilen Yedekleme özelliğini kullandığından, her ikisi için de aynı izleme teknikleri geçerlidir.

Öncelikle [msdb.smart_admin.sp_get_backup_diagnostics](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/managed-backup-sp-get-backup-diagnostics-transact-sql)numaralı telefonu arayarak durumu yoklayabilirsiniz. Veya [msdb.smart_admin.fn_get_health_status](https://docs.microsoft.com/sql/relational-databases/system-functions/managed-backup-fn-get-health-status-transact-sql) tablo değerli işlevini sorgula.

> [!NOTE]
> SQL Server 2014'te Yönetilen Yedekleme şeması **msdb.smart_admin'** dir. SQL Server 2016'da bu **durum msdb.managed_backup**olarak değiştirildi ve başvuru başlıkları bu yeni şemayı kullanıyor. Ancak SQL Server 2014 için, tüm Yönetilen Yedekleme nesneleri için **smart_admin** şemasını kullanmaya devam etmelisiniz.

Başka bir seçenek bildirimler için yerleşik Veritabanı Posta özelliğinden yararlanmaktır.

1. **SSMBackup2WANotificationEmailIds** parametresine bir e-posta adresi atamak için [msdb.smart_admin.sp_set_parameter](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/managed-backup-sp-set-parameter-transact-sql) saklı yordamı arayın. 
1. [SendGrid'in](../../../sendgrid-dotnet-how-to-send-email.md) e-postaları Azure VM'den göndermesini etkinleştirin.
1. Veritabanı Postasını yapılandırmak için SMTP sunucusunu ve kullanıcı adını kullanın. Veritabanı Postasını SQL Server Management Studio'da veya Transact-SQL komutlarıyla yapılandırabilirsiniz. Daha fazla bilgi için [Veritabanı Postası'na](https://docs.microsoft.com/sql/relational-databases/database-mail/database-mail)bakın.
1. [Veritabanı Postasını kullanmak için SQL Server Agent'ı yapılandırın.](https://docs.microsoft.com/sql/relational-databases/database-mail/configure-sql-server-agent-mail-to-use-database-mail)
1. SMTP bağlantı noktasına hem yerel VM güvenlik duvarı hem de VM için ağ güvenlik grubu aracılığıyla izin verildiğini doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar

Otomatik Yedekleme, Azure VM'lerinde Yönetilen Yedekleme'yi yapılandırır. Bu nedenle [SQL Server 2014'te Yönetilen Yedekleme için belgeleri gözden geçirmek](https://msdn.microsoft.com/library/dn449497(v=sql.120).aspx)önemlidir.

Aşağıdaki makalede Azure VM'lerde SQL Server için ek yedekleme ve geri yükleme kılavuzu bulabilirsiniz: [Azure Sanal Makinelerde SQL Server için Yedekleme ve Geri Yükleme.](virtual-machines-windows-sql-backup-recovery.md)

Mevcut diğer otomasyon görevleri hakkında daha fazla bilgi için [SQL Server IaaS Agent Extension'a](virtual-machines-windows-sql-server-agent-extension.md)bakın.

Azure VM'lerde SQL Server'ı çalıştırma hakkında daha fazla bilgi için [Azure Sanal Makinelere genel bakışta SQL Server'a](virtual-machines-windows-sql-server-iaas-overview.md)bakın.
