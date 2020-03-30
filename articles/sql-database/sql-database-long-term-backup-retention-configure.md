---
title: Uzun süreli yedek saklamayı yönetme
description: Otomatik yedeklemeleri SQL Azure depolama alanında nasıl depolayıp geri yükleyin
services: sql-database
ms.service: sql-database
ms.subservice: backup-restore
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: mathoma, carlrab
manager: craigg
ms.date: 08/21/2019
ms.openlocfilehash: a560f4f1399792a7b150b37c3c048ccc0079b98d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74420795"
---
# <a name="manage-azure-sql-database-long-term-backup-retention"></a>Azure SQL Veritabanı'nı uzun süreli yedekleme bekletme yönetme

Azure SQL Veritabanı'nda, veritabanı yedeklemelerini ayrı Azure Blob depolama kaplarında 10 yıla kadar otomatik olarak tutmak için tek veya havuzlu bir veritabanını [uzun vadeli yedekleme bekletme](sql-database-long-term-retention.md) ilkesine (LTR) göre yapılandırabilirsiniz. Daha sonra Azure portalını veya PowerShell'i kullanarak bu yedeklemeleri kullanarak bir veritabanı kurtarabilirsiniz.

> [!IMPORTANT]
> [Azure SQL veritabanı yönetilen örnek](sql-database-managed-instance.md) şu anda uzun vadeli yedekleme bekletme desteklemiyor.

## <a name="using-azure-portal"></a>Azure portalını kullanma

Aşağıdaki bölümler, uzun süreli bekletmeyi yapılandırmak, yedeklemeleri uzun süreli bekletmede görüntülemek ve uzun süreli bekletmeden yedeklemeyi geri yüklemek için Azure portalını nasıl kullanacağınızı gösterir.

### <a name="configure-long-term-retention-policies"></a>Uzun vadeli bekletme ilkelerini yapılandırma

SQL Veritabanı'nı, hizmet katmanınızın bekletme süresinden daha uzun bir süre [için otomatik yedeklemeleri tutacak](sql-database-long-term-retention.md) şekilde yapılandırabilirsiniz.

1. Azure portalında, SQL sunucunuzu seçin ve ardından **Yedekleri Yönet'i**tıklatın. **Yapılandırma ilkeleri** sekmesinde, uzun vadeli yedekleme bekletme ilkelerini ayarlamak veya değiştirmek istediğiniz veritabanının onay kutusunu seçin. Veritabanının yanındaki onay kutusu seçilmezse, ilke değişiklikleri bu veritabanıiçin geçerli olmaz.  

   ![yedekleme bağlantısını yönetme](./media/sql-database-long-term-retention/ltr-configure-ltr.png)

2. **Yapılı ilkeler** bölümünde, haftalık, aYağlık veya yılyık yedeklemeleri sürmek isteyip istediğini seçin ve her biri için bekletme dönüşünü belirtin.

   ![ilkeleri yapılandırma](./media/sql-database-long-term-retention/ltr-configure-policies.png)

3. Tamamlandığında **Uygula'yı**tıklatın.

> [!IMPORTANT]
> Uzun vadeli bir yedekleme bekletme ilkesini etkinleştirdiğinizde, ilk yedeklemenin görünür olması ve geri yüklenebilmeleri 7 gün kadar sürebilir. LTR yedekleme cadance ayrıntıları için, [uzun vadeli yedekleme tutma](sql-database-long-term-retention.md)bakın.

### <a name="view-backups-and-restore-from-a-backup"></a>Yedeklemeleri görüntüleyin ve yedeklemeden geri yükleme

LTR ilkesiyle belirli bir veritabanı için tutulan yedeklemeleri görüntüleyin ve bu yedeklemelerden geri yükleyin.

1. Azure portalında, SQL sunucunuzu seçin ve ardından **Yedekleri Yönet'i**tıklatın. Kullanılabilir **yedeklemeler** sekmesinde, kullanılabilir yedeklemeleri görmek istediğiniz veritabanını seçin.

   ![veritabanını seçin](./media/sql-database-long-term-retention/ltr-available-backups-select-database.png)

1. Kullanılabilir **yedekleme** ler bölmesinde, kullanılabilir yedeklemeleri gözden geçirin.

   ![yedeklemeleri görüntüleme](./media/sql-database-long-term-retention/ltr-available-backups.png)

1. Geri yüklemek istediğiniz yedeklemeyi seçin ve ardından yeni veritabanı adını belirtin.

   ![geri yükleme](./media/sql-database-long-term-retention/ltr-restore.png)

1. Azure SQL depolama sındaki yedeklemeden yeni veritabanına veritabanınızı geri yüklemek için **Tamam'ı** tıklatın.

1. Geri yükleme işinin durumunu görüntülemek için araç çubuğundaki bildirim simgesine tıklayın.

   ![geri yükleme işi ilerleme durumu](./media/sql-database-get-started-backup-recovery/restore-job-progress-long-term.png)

1. Geri yükleme işi tamamlandığında, yeni geri yüklenen veritabanını görüntülemek için **SQL veritabanları** sayfasını açın.

> [!NOTE]
> Buradan [var olan veritabanına kopyalamak için geri yüklenen veritabanından veri ayıklama veya var olan veritabanını silerek geri yüklenen veritabanının adını var olan veritabanının adıyla değiştirme](sql-database-recovery-using-backups.md#point-in-time-restore) gibi görevleri gerçekleştirmek için SQL Server Management Studio kullanarak geri yüklenen veritabanına bağlanabilirsiniz.

## <a name="using-powershell"></a>PowerShell’i kullanma

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

> [!IMPORTANT]
> PowerShell Azure Kaynak Yöneticisi modülü hala Azure SQL Veritabanı tarafından desteklenir, ancak gelecekteki tüm geliştirme az.sql modülü içindir. Bu cmdlets için [AzureRM.Sql](https://docs.microsoft.com/powershell/module/AzureRM.Sql/)bakın. Az modülündeki ve AzureRm modüllerinde bulunan komutların bağımsız değişkenleri önemli ölçüde aynıdır.

Aşağıdaki bölümler, uzun süreli yedekleme bekletme yapılandırmak, Yedeklemeleri Azure SQL depolama alanında görüntülemek ve Azure SQL depolama alanında bir yedeklemeden geri yüklemek için PowerShell'i nasıl kullanacağınızı gösterir.

### <a name="rbac-roles-to-manage-long-term-retention"></a>Uzun süreli tutma yönetmek için RBAC rolleri

**Get-AzSqlDatabaseLongTermRetentionBackup** ve **Restore-AzSqlDatabase**için aşağıdaki rollerden birine sahip olmanız gerekir:

- Abonelik Sahibi rolü veya
- SQL Server Katılımcısıfatı rolü veya
- Aşağıdaki izinleri içeren özel rol:

   Microsoft.Sql/locations/longTermRetentionBackups/read Microsoft.Sql/locations/longTermRetentionBackups/read Microsoft.Sql/locations/longTermRetentionServers/longTermRetentionDatabaseDatabases/longTermRetentionBackups/read

**Remove-AzSqlDatabaseLongTermRetentionBackup**için aşağıdaki rollerden birine sahip olmanız gerekir:

- Abonelik Sahibi rolü veya
- Aşağıdaki izinle özel rol:

   Microsoft.Sql/locations/longTermRetentionServers/longTermRetentionDatabases/longTermRetentionBackups/delete

> [!NOTE]
> SQL Server Katılımcısı rolünün LTR yedeklemelerini silme izni yoktur.

RBAC izinleri *abonelik* veya kaynak *grubu* kapsamında verilebilir. Ancak, bırakılan bir sunucuya ait LTR yedeklemelerine erişmek için, bu sunucunun *abonelik* kapsamında izin verilmesi gerekir.

- Microsoft.Sql/locations/longTermRetentionServers/longTermRetentionDatabases/longTermRetentionBackups/delete

### <a name="create-an-ltr-policy"></a>LTR ilkesi oluşturma

```powershell
# get the SQL server
$subId = "<subscriptionId>"
$serverName = "<serverName>"
$resourceGroup = "<resourceGroupName>"
$dbName = "<databaseName>"

Connect-AzAccount
Select-AzSubscription -SubscriptionId $subId

$server = Get-AzSqlServer -ServerName $serverName -ResourceGroupName $resourceGroup

# create LTR policy with WeeklyRetention = 12 weeks. MonthlyRetention and YearlyRetention = 0 by default.
Set-AzSqlDatabaseBackupLongTermRetentionPolicy -ServerName $serverName -DatabaseName $dbName `
    -ResourceGroupName $resourceGroup -WeeklyRetention P12W

# create LTR policy with WeeklyRetention = 12 weeks, YearlyRetention = 5 years and WeekOfYear = 16 (week of April 15). MonthlyRetention = 0 by default.
Set-AzSqlDatabaseBackupLongTermRetentionPolicy -ServerName $serverName -DatabaseName $dbName `
    -ResourceGroupName $resourceGroup -WeeklyRetention P12W -YearlyRetention P5Y -WeekOfYear 16
```

### <a name="view-ltr-policies"></a>LTR ilkelerini görüntüleyin

Bu örnek, bir sunucudaki LTR ilkelerinin nasıl listelenebildiğini gösterir

```powershell
# get all LTR policies within a server
$ltrPolicies = Get-AzSqlDatabase -ResourceGroupName Default-SQL-WestCentralUS -ServerName trgrie-ltr-server | `
    Get-AzSqlDatabaseLongTermRetentionPolicy -Current

# get the LTR policy of a specific database
$ltrPolicies = Get-AzSqlDatabaseBackupLongTermRetentionPolicy -ServerName $serverName -DatabaseName $dbName `
    -ResourceGroupName $resourceGroup -Current
```

### <a name="clear-an-ltr-policy"></a>Bir LTR ilkesini temizleme

Bu örnek, bir LTR ilkesini veritabanından nasıl temizler

```powershell
Set-AzSqlDatabaseBackupLongTermRetentionPolicy -ServerName $serverName -DatabaseName $dbName `
    -ResourceGroupName $resourceGroup -RemovePolicy
```

### <a name="view-ltr-backups"></a>LTR yedeklemelerini görüntüleyin

Bu örnek, sunucudaki LTR yedeklemelerinin nasıl listelenebildiğini gösterir.

```powershell
# get the list of all LTR backups in a specific Azure region
# backups are grouped by the logical database id, within each group they are ordered by the timestamp, the earliest backup first
$ltrBackups = Get-AzSqlDatabaseLongTermRetentionBackup -Location $server.Location

# get the list of LTR backups from the Azure region under the named server
$ltrBackups = Get-AzSqlDatabaseLongTermRetentionBackup -Location $server.Location -ServerName $serverName

# get the LTR backups for a specific database from the Azure region under the named server
$ltrBackups = Get-AzSqlDatabaseLongTermRetentionBackup -Location $server.Location -ServerName $serverName -DatabaseName $dbName

# list LTR backups only from live databases (you have option to choose All/Live/Deleted)
$ltrBackups = Get-AzSqlDatabaseLongTermRetentionBackup -Location $server.Location -DatabaseState Live

# only list the latest LTR backup for each database
$ltrBackups = Get-AzSqlDatabaseLongTermRetentionBackup -Location $server.Location -ServerName $serverName -OnlyLatestPerDatabase
```

### <a name="delete-ltr-backups"></a>LTR yedeklemelerini silme

Bu örnek, bir LTR yedeklemesinin yedekleme listesinden nasıl silinir olduğunu gösterir.

```powershell
# remove the earliest backup
$ltrBackup = $ltrBackups[0]
Remove-AzSqlDatabaseLongTermRetentionBackup -ResourceId $ltrBackup.ResourceId
```

> [!IMPORTANT]
> LTR yedeklemesi siler. Sunucu silindikten sonra bir LTR yedeklemesini silmek için Abonelik kapsamı iznine sahip olmalısınız. 'Uzun süreli bekletme yedeklemesi siler' işlemi için filtre uygulayarak Azure Monitor'da her silme yle ilgili bildirimler ayarlayabilirsiniz. Etkinlik günlüğü, isteğin kim ve ne zaman yapıldığına ilişkin bilgileri içerir. Bkz. Ayrıntılı yönergeler için [etkinlik günlüğü uyarıları oluşturun.](../azure-monitor/platform/alerts-activity-log.md)

### <a name="restore-from-ltr-backups"></a>LTR yedeklemelerinden geri yükleme

Bu örnek, ltr yedeklemesinden nasıl geri yüklenirlerini gösterir. Not, bu arabirim değişmedi ancak kaynak kimliği parametresi artık LTR yedekleme kaynak kimliği gerektirir.

```powershell
# restore a specific LTR backup as an P1 database on the server $serverName of the resource group $resourceGroup
Restore-AzSqlDatabase -FromLongTermRetentionBackup -ResourceId $ltrBackup.ResourceId -ServerName $serverName -ResourceGroupName $resourceGroup `
    -TargetDatabaseName $dbName -ServiceObjectiveName P1
```

> [!IMPORTANT]
> Sunucu silindikten sonra ltr yedeklemesinden geri yüklemek için, izinlerin sunucunun aboneliğine kapsamı nın olması ve bu aboneliğin etkin olması gerekir. İsteğe bağlı -ResourceGroupName parametresini de atlamalısınız.

> [!NOTE]
> Buradan var olan veritabanına kopyalamak için geri yüklenen veritabanından veri ayıklama veya var olan veritabanını silerek geri yüklenen veritabanının adını var olan veritabanının adıyla değiştirme gibi görevleri gerçekleştirmek için SQL Server Management Studio kullanarak geri yüklenen veritabanına bağlanabilirsiniz. [Bkz. zaman geri yükleme stoyonu.](sql-database-recovery-using-backups.md#point-in-time-restore)

## <a name="next-steps"></a>Sonraki adımlar

- Hizmet tarafından oluşturulan otomatik yedekler hakkında bilgi edinmek için bkz. [otomatik yedekler](sql-database-automated-backups.md)
- Uzun süreli yedek saklama hakkında bilgi edinmek için bkz. [uzun süreli yedek saklama](sql-database-long-term-retention.md)
