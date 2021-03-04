---
title: 'Azure SQL yönetilen örneği: uzun süreli yedek saklama'
description: PowerShell kullanarak Azure SQL yönetilen örneği için otomatik yedeklemeleri ayrı Azure Blob depolama kapsayıcılarında depolamayı ve geri yüklemeyi öğrenin.
services: sql-database
ms.service: sql-managed-instance
ms.subservice: operations
ms.custom: ''
ms.devlang: ''
ms.topic: how-to
author: shkale-msft
ms.author: shkale
ms.reviewer: mathoma, sstein
ms.date: 02/25/2021
ms.openlocfilehash: f298f0f9d76750be932db79b5a08b6385e984f88
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2021
ms.locfileid: "102052109"
---
# <a name="manage-azure-sql-managed-instance-long-term-backup-retention-powershell"></a>Azure SQL yönetilen örneğini yönetme uzun vadeli yedek saklama (PowerShell)
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

Azure SQL yönetilen örneği 'nde [uzun süreli bir yedekleme bekletme](../database/long-term-retention-overview.md) IlkesI (LTR) genel önizleme özelliği olarak yapılandırabilirsiniz. Bu, veritabanı yedeklerini 10 yıla kadar ayrı Azure Blob depolama kapsayıcılarında otomatik olarak korumanızı sağlar. Ardından, bu yedeklemeleri PowerShell ile kullanarak bir veritabanını kurtarabilirsiniz.

   > [!IMPORTANT]
   > Yönetilen örnekler için LTR, Azure genel bölgelerinde genel önizlemede kullanılabilir. 

Aşağıdaki bölümlerde, PowerShell kullanarak uzun süreli yedek saklama, Azure SQL depolamada yedeklemeleri görüntüleme ve Azure SQL depolama 'daki bir yedekten geri yükleme işlemleri gösterilmektedir.


## <a name="using-the-azure-portal"></a>Azure portalını kullanma

Aşağıdaki bölümlerde, uzun süreli saklama ilkeleri ayarlamak, kullanılabilir uzun süreli bekletme yedeklemelerini yönetmek ve kullanılabilir bir yedekten geri yüklemek için Azure portal nasıl kullanılacağı gösterilmektedir.

### <a name="configure-long-term-retention-policies"></a>Uzun süreli saklama ilkelerini yapılandırma

Hizmet katmanınızın saklama süresinden daha uzun bir süre için [Otomatik yedeklemeleri koruyacak](../database/long-term-retention-overview.md) şekilde SQL yönetilen örneği yapılandırabilirsiniz.

1. Azure portal yönetilen örneğinizi seçin ve ardından **yedeklemeler**' e tıklayın. **Bekletme ilkeleri** sekmesinde, uzun süreli yedek saklama ilkelerini ayarlamak veya değiştirmek istediğiniz veritabanlarını seçin. Değişiklikler seçili olmayan hiçbir veritabanına uygulanmaz. 

   ![Yedeklemeleri Yönet bağlantısı](./media/long-term-backup-retention-configure/ltr-configure-ltr.png)

2. **Ilkeleri Yapılandır** bölmesinde, haftalık, aylık veya yıllık yedeklemeler için istediğiniz saklama süresini belirtin. Uzun süreli yedek saklama alanının ayarlanması gerektiğini belirtmek için ' 0 ' bekletme dönemini seçin.

   ![ilkeleri yapılandırma](./media/long-term-backup-retention-configure/ltr-configure-policies.png)

3. Tamamlandığında, **Uygula**' ya tıklayın.

> [!IMPORTANT]
> Uzun süreli yedek saklama ilkesini etkinleştirdiğinizde, ilk yedeklemenin görünür olması ve geri yükleme için kullanılabilir olması 7 güne kadar sürebilir. LTR yedekleme cadans ayrıntıları için bkz. [uzun süreli yedek saklama](../database/long-term-retention-overview.md).

### <a name="view-backups-and-restore-from-a-backup"></a>Yedeklemeleri görüntüleme ve yedekten geri yükleme

Bir LTR ilkesiyle belirli bir veritabanı için tutulan yedeklemeleri görüntüleyin ve bu yedeklerden geri yükleyin.

1. Azure portal yönetilen örneğinizi seçin ve ardından **yedeklemeler**' e tıklayın. **Kullanılabilir yedeklemeler** sekmesinde, kullanılabilir yedeklemeleri görmek istediğiniz veritabanını seçin. **Yönet**'e tıklayın.

   ![Veritabanı Seç](./media/long-term-backup-retention-configure/ltr-available-backups-select-database.png)

1. **Yedeklemeleri Yönet** bölmesinde, kullanılabilir yedeklemeleri gözden geçirin.

   ![yedeklemeleri görüntüle](./media/long-term-backup-retention-configure/ltr-available-backups.png)

1. Geri yüklemek istediğiniz yedeği seçin, **geri** Yükle ' ye tıklayın, ardından Geri Yükle sayfasında yeni veritabanı adını belirtin. Yedekleme ve kaynak bu sayfada önceden doldurulur. 

   ![geri yükleme için yedek seçin](./media/long-term-backup-retention-configure/ltr-available-backups-restore.png)
   
   ![geri yükleme](./media/long-term-backup-retention-configure/ltr-restore.png)

1. Geri yükleme ayrıntılarınızı gözden geçirmek için **gözden geçir + oluştur** seçeneğine tıklayın. Ardından, veritabanınızı seçilen yedekten geri yüklemek için **Oluştur** ' a tıklayın.

1. Geri yükleme işinin durumunu görüntülemek için araç çubuğundaki bildirim simgesine tıklayın.

   ![geri yükleme işi ilerleme durumu](./media/long-term-backup-retention-configure/restore-job-progress-long-term.png)

1. Geri yükleme işi tamamlandığında, yeni geri yüklenen veritabanını görüntülemek için **yönetilen örneğe genel bakış** sayfasını açın.

> [!NOTE]
> Buradan [var olan veritabanına kopyalamak için geri yüklenen veritabanından veri ayıklama veya var olan veritabanını silerek geri yüklenen veritabanının adını var olan veritabanının adıyla değiştirme](../database/recovery-using-backups.md#point-in-time-restore) gibi görevleri gerçekleştirmek için SQL Server Management Studio kullanarak geri yüklenen veritabanına bağlanabilirsiniz.


## <a name="using-powershell"></a>PowerShell’i kullanma
[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

> [!IMPORTANT]
> PowerShell Azure Resource Manager modülü Azure SQL veritabanı tarafından hala desteklenmektedir, ancak gelecekteki geliştirme az. SQL modülünde yapılır. Bu cmdlet 'ler için bkz. [Azurerd. SQL](/powershell/module/AzureRM.Sql/). Az Module ve Azurerd modüllerinde komutların bağımsız değişkenleri önemli ölçüde aynıdır.

Aşağıdaki bölümlerde, PowerShell kullanarak uzun süreli yedek saklama, Azure depolama 'daki yedeklemeleri görüntüleme ve Azure Storage 'daki bir yedekten geri yükleme işlemleri gösterilmektedir.

### <a name="azure-rbac-roles-to-manage-long-term-retention"></a>Uzun süreli saklama yönetimi için Azure RBAC rolleri

**Get-AzSqlInstanceDatabaseLongTermRetentionBackup** ve **restore-Azsqlınstancedatabase** için aşağıdaki rollerden birine sahip olmanız gerekir:

- Abonelik sahibi rolü veya
- Yönetilen örnek katılımcısı rolü veya
- Aşağıdaki izinlerle özel rol:
  - `Microsoft.Sql/locations/longTermRetentionManagedInstanceBackups/read`
  - `Microsoft.Sql/locations/longTermRetentionManagedInstances/longTermRetentionManagedInstanceBackups/read`
  - `Microsoft.Sql/locations/longTermRetentionManagedInstances/longTermRetentionDatabases/longTermRetentionManagedInstanceBackups/read`

**Remove-AzSqlInstanceDatabaseLongTermRetentionBackup** için aşağıdaki rollerden birine sahip olmanız gerekir:

- Abonelik sahibi rolü veya
- Aşağıdaki izne sahip özel rol:
  - `Microsoft.Sql/locations/longTermRetentionManagedInstances/longTermRetentionDatabases/longTermRetentionManagedInstanceBackups/delete`

> [!NOTE]
> Yönetilen örnek katılımcısı rolü, LTR yedeklemeleri silme iznine sahip değil.

Azure RBAC izinleri, *abonelik* ya da *kaynak grubu* kapsamında verilebilir. Ancak, bırakılan bir örneğe ait olan LTR yedeklemelerine erişmek için, bu örneğin *abonelik* kapsamında izin verilmelidir.

- `Microsoft.Sql/locations/longTermRetentionManagedInstances/longTermRetentionDatabases/longTermRetentionManagedInstanceBackups/delete`

### <a name="create-an-ltr-policy"></a>Bir LTR ilkesi oluşturma

```powershell
# get the Managed Instance
$subId = "<subscriptionId>"
$instanceName = "<instanceName>"
$resourceGroup = "<resourceGroupName>"
$dbName = "<databaseName>"

Connect-AzAccount

Select-AzSubscription -SubscriptionId $subId

$instance = Get-AzSqlInstance -Name $instanceName -ResourceGroupName $resourceGroup

# create LTR policy with WeeklyRetention = 12 weeks. MonthlyRetention and YearlyRetention = 0 by default.
$LTRPolicy = @{
    InstanceName = $instanceName 
    DatabaseName = $dbName 
    ResourceGroupName = $resourceGroup 
    WeeklyRetention = 'P12W'
}
Set-AzSqlInstanceDatabaseBackupLongTermRetentionPolicy @LTRPolicy

# create LTR policy with WeeklyRetention = 12 weeks, YearlyRetention = 5 years and WeekOfYear = 16 (week of April 15). MonthlyRetention = 0 by default.
$LTRPolicy = @{
    InstanceName = $instanceName 
    DatabaseName = $dbName 
    ResourceGroupName = $resourceGroup 
    WeeklyRetention = 'P12W' 
    YearlyRetention = 'P5Y' 
    WeekOfYear = '16'
}
Set-AzSqlInstanceDatabaseBackupLongTermRetentionPolicy @LTRPolicy
```

### <a name="view-ltr-policies"></a>LTR ilkelerini görüntüle

Bu örnek, tek bir veritabanı için bir örnek içinde LTR ilkelerinin nasıl ekleneceğini gösterir

```powershell
# gets the current version of LTR policy for a database
$LTRPolicies = @{
    InstanceName = $instanceName 
    DatabaseName = $dbName 
    ResourceGroupName = $resourceGroup
}
Get-AzSqlInstanceDatabaseBackupLongTermRetentionPolicy @LTRPolicy 
```

Bu örnek, bir örnekteki tüm veritabanları için LTR ilkelerinin nasıl ekleneceğini gösterir

```powershell
# gets the current version of LTR policy for all of the databases on an instance

$Databases = Get-AzSqlInstanceDatabase -ResourceGroupName $resourceGroup -InstanceName $instanceName

$LTRParams = @{
    InstanceName = $instanceName
    ResourceGroupName = $resourceGroup
}

foreach($database in $Databases.Name){
    Get-AzSqlInstanceDatabaseBackupLongTermRetentionPolicy @LTRParams  -DatabaseName $database
 }
```

### <a name="clear-an-ltr-policy"></a>Bir LTR ilkesini Temizleme

Bu örnekte, bir veritabanından bir LTR ilkesinin nasıl temizlediğiniz gösterilmektedir

```powershell
# remove the LTR policy from a database
$LTRPolicy = @{
    InstanceName = $instanceName 
    DatabaseName = $dbName 
    ResourceGroupName = $resourceGroup 
    RemovePolicy = $true
}
Set-AzSqlInstanceDatabaseBackupLongTermRetentionPolicy @LTRPolicy
```

### <a name="view-ltr-backups"></a>LTR yedeklemeleri görüntüle

Bu örnek, bir örnek içinde LTR yedeklemelerin nasıl ekleneceğini gösterir.

```powershell

$instance = Get-AzSqlInstance -Name $instanceName -ResourceGroupName $resourceGroup

# get the list of all LTR backups in a specific Azure region
# backups are grouped by the logical database id, within each group they are ordered by the timestamp, the earliest backup first
Get-AzSqlInstanceDatabaseLongTermRetentionBackup -Location $instance.Location

# get the list of LTR backups from the Azure region under the given managed instance
$LTRBackupParam = @{
    Location = $instance.Location 
    InstanceName = $instanceName
}
Get-AzSqlInstanceDatabaseLongTermRetentionBackup @LTRBackupParam

# get the LTR backups for a specific database from the Azure region under the given managed instance
$LTRBackupParam = @{
    Location = $instance.Location 
    InstanceName = $instanceName
    DatabaseName = $dbName
}
Get-AzSqlInstanceDatabaseLongTermRetentionBackup @LTRBackupParam

# list LTR backups only from live databases (you have option to choose All/Live/Deleted)
$LTRBackupParam = @{
    Location = $instance.Location 
    DatabaseState = 'Live'
}
Get-AzSqlInstanceDatabaseLongTermRetentionBackup @LTRBackupParam

# only list the latest LTR backup for each database
$LTRBackupParam = @{
    Location = $instance.Location 
    InstanceName = $instanceName
    OnlyLatestPerDatabase = $true
}
Get-AzSqlInstanceDatabaseLongTermRetentionBackup @LTRBackupParam 
```

### <a name="delete-ltr-backups"></a>LTR yedeklemeleri Sil

Bu örnek, yedeklemeler listesinden bir LTR yedeklemenin nasıl silineceğini gösterir.

```powershell
# remove the earliest backup
# get the LTR backups for a specific database from the Azure region under the given managed instance
$LTRBackupParam = @{
    Location = $instance.Location 
    InstanceName = $instanceName
    DatabaseName = $dbName
}
$ltrBackups = Get-AzSqlInstanceDatabaseLongTermRetentionBackup @LTRBackupParam
$ltrBackup = $ltrBackups[0]
Remove-AzSqlInstanceDatabaseLongTermRetentionBackup -ResourceId $ltrBackup.ResourceId
```

> [!IMPORTANT]
> LTR yedeklemenin silinmesi geri alınamaz. Örnek silindikten sonra bir LTR yedeklemesini silmek için abonelik kapsamı izninizin olması gerekir. Azure Izleyici 'de her silme hakkında bildirimleri, ' uzun süreli bekletme yedeklemesini siler ' işlemi için filtreleyerek ayarlayabilirsiniz. Etkinlik günlüğü, ne zaman ve ne zaman istek yaptığı hakkında bilgiler içerir. Ayrıntılı yönergeler için bkz. [etkinlik günlüğü uyarıları oluşturma](../../azure-monitor/alerts/alerts-activity-log.md) .

### <a name="restore-from-ltr-backups"></a>LTR yedeklemelerden geri yükleme

Bu örnek, bir LTR yedeğinden nasıl geri yükleneceğini gösterir. Bu arabirim değişmediği halde kaynak KIMLIĞI parametresi artık LTR yedekleme kaynağı KIMLIĞINI gerektiriyor.

```powershell
# restore a specific LTR backup as an P1 database on the instance $instanceName of the resource group $resourceGroup
$LTRBackupParam = @{
    Location = $instance.Location 
    InstanceName = $instanceName
    DatabaseName = $dbname
    OnlyLatestPerDatabase = $true
}
$ltrBackup = Get-AzSqlInstanceDatabaseLongTermRetentionBackup @LTRBackupParam 

$RestoreLTRParam = @{
    TargetInstanceName          = $instanceName 
    TargetResourceGroupName     = $resourceGroup 
    TargetInstanceDatabaseName  = $dbName
    FromLongTermRetentionBackup = $true
    ResourceId                  = $ltrBackup.ResourceId 
}
Restore-AzSqlInstanceDatabase @RestoreLTRParam
```

> [!IMPORTANT]
> Örnek silindikten sonra bir LTR yedeklemesinden geri yüklemek için, örneğin aboneliğine sahip olan ve bu aboneliğin etkin olması gereken izinlere sahip olmanız gerekir. İsteğe bağlı-ResourceGroupName parametresini de atmalısınız.

> [!NOTE]
> Buradan var olan veritabanına kopyalamak için geri yüklenen veritabanından veri ayıklama veya var olan veritabanını silerek geri yüklenen veritabanının adını var olan veritabanının adıyla değiştirme gibi görevleri gerçekleştirmek için SQL Server Management Studio kullanarak geri yüklenen veritabanına bağlanabilirsiniz. Bkz. [zaman noktası geri yükleme](../database/recovery-using-backups.md#point-in-time-restore).

## <a name="next-steps"></a>Sonraki adımlar

- Hizmet tarafından oluşturulan otomatik yedekler hakkında bilgi edinmek için bkz. [otomatik yedekler](../database/automated-backups-overview.md)
- Uzun süreli yedek saklama hakkında bilgi edinmek için bkz. [uzun süreli yedek saklama](../database/long-term-retention-overview.md)
