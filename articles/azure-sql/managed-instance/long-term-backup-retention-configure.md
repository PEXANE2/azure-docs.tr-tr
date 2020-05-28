---
title: 'Azure SQL yönetilen örneği: uzun süreli yedek saklama (PowerShell)'
description: PowerShell kullanarak Azure SQL yönetilen örneği için otomatik yedeklemeleri ayrı Azure Blob depolama kapsayıcılarında depolamayı ve geri yüklemeyi öğrenin.
services: sql-database
ms.service: sql-database
ms.subservice: operations
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: mathoma, carlrab
manager: craigg
ms.date: 04/29/2020
ms.openlocfilehash: 385a7594de48f1bcf04d79d0dcd9dfb521d4ff08
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84045119"
---
# <a name="manage-azure-sql-managed-instance-long-term-backup-retention-powershell"></a>Azure SQL yönetilen örneğini yönetme uzun vadeli yedek saklama (PowerShell)
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

Azure SQL yönetilen örneği 'nde [uzun süreli yedek saklama](../database/long-term-retention-overview.md#managed-instance-support) ILKESINI (LTR) sınırlı bir genel önizleme özelliği olarak yapılandırabilirsiniz. Bu sayede, veritabanı yedeklerini 10 yıla kadar ayrı Azure Blob depolama kapsayıcılarında otomatik olarak koruyabilirsiniz. Ardından, bu yedeklemeleri PowerShell ile kullanarak bir veritabanını kurtarabilirsiniz.

   > [!IMPORTANT]
   > Yönetilen örnekler için LTR Şu anda sınırlı önizlemededir ve büyük bir süre içinde EA ve CSP aboneliklerinde kullanılabilir. Kayıt istemek için lütfen bir [Azure destek bileti](https://azure.microsoft.com/support/create-ticket/)oluşturun. Sorun türü için teknik sorunu seçin, hizmet için SQL veritabanı yönetilen örneğini seçin ve sorun türü için **yedekleme, geri yükleme ve Iş sürekliliği/uzun süreli yedekleme saklama ' yı**seçin. İsteğiniz içinde, yönetilen örnek için LTR 'ın sınırlı genel önizlemesine kaydedilmesini istediğiniz durumu girin.

Aşağıdaki bölümlerde, PowerShell kullanarak uzun süreli yedek saklama, Azure SQL depolamada yedeklemeleri görüntüleme ve Azure SQL depolama 'daki bir yedekten geri yükleme işlemleri gösterilmektedir.

## <a name="rbac-roles-to-manage-long-term-retention"></a>Uzun süreli saklama yönetimi için RBAC rolleri

**Get-AzSqlInstanceDatabaseLongTermRetentionBackup** ve **restore-Azsqlınstancedatabase**için aşağıdaki rollerden birine sahip olmanız gerekir:

- Abonelik sahibi rolü veya
- Yönetilen örnek katılımcısı rolü veya
- Aşağıdaki izinlerle özel rol:
  - `Microsoft.Sql/locations/longTermRetentionManagedInstanceBackups/read`
  - `Microsoft.Sql/locations/longTermRetentionManagedInstances/longTermRetentionManagedInstanceBackups/read`
  - `Microsoft.Sql/locations/longTermRetentionManagedInstances/longTermRetentionDatabases/longTermRetentionManagedInstanceBackups/read`

**Remove-AzSqlInstanceDatabaseLongTermRetentionBackup**için aşağıdaki rollerden birine sahip olmanız gerekir:

- Abonelik sahibi rolü veya
- Aşağıdaki izne sahip özel rol:
  - `Microsoft.Sql/locations/longTermRetentionManagedInstances/longTermRetentionDatabases/longTermRetentionManagedInstanceBackups/delete`

> [!NOTE]
> Yönetilen örnek katılımcısı rolü, LTR yedeklemeleri silme iznine sahip değil.

*Abonelik* ya da *kaynak grubu* kapsamında RBAC izinleri verilebilir. Ancak, bırakılan bir örneğe ait olan LTR yedeklemelerine erişmek için, bu örneğin *abonelik* kapsamında izin verilmelidir.

- `Microsoft.Sql/locations/longTermRetentionManagedInstances/longTermRetentionDatabases/longTermRetentionManagedInstanceBackups/delete`

## <a name="create-an-ltr-policy"></a>Bir LTR ilkesi oluşturma

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
Set-AzSqlInstanceDatabaseBackupLongTermRetentionPolicy -InstanceName $instanceName `
   -DatabaseName $dbName -ResourceGroupName $resourceGroup -WeeklyRetention P12W

# create LTR policy with WeeklyRetention = 12 weeks, YearlyRetention = 5 years and WeekOfYear = 16 (week of April 15). MonthlyRetention = 0 by default.
Set-AzSqlInstanceDatabaseBackupLongTermRetentionPolicy -InstanceName $instanceName `
    -DatabaseName $dbName -ResourceGroupName $resourceGroup -WeeklyRetention P12W -YearlyRetention P5Y -WeekOfYear 16
```

## <a name="view-ltr-policies"></a>LTR ilkelerini görüntüle

Bu örnek, bir örnek içinde LTR ilkelerinin nasıl ekleneceğini gösterir

```powershell
# gets the current version of LTR policy for the database
$ltrPolicies = Get-AzSqlInstanceDatabaseBackupLongTermRetentionPolicy -InstanceName $instanceName `
    -DatabaseName $dbName -ResourceGroupName $resourceGroup
```

## <a name="clear-an-ltr-policy"></a>Bir LTR ilkesini Temizleme

Bu örnekte, bir veritabanından bir LTR ilkesinin nasıl temizlediğiniz gösterilmektedir

```powershell
Set-AzSqlInstanceDatabaseBackupLongTermRetentionPolicy -InstanceName $instanceName `
   -DatabaseName $dbName -ResourceGroupName $resourceGroup -RemovePolicy
```

## <a name="view-ltr-backups"></a>LTR yedeklemeleri görüntüle

Bu örnek, bir örnek içinde LTR yedeklemelerin nasıl ekleneceğini gösterir.

```powershell
# get the list of all LTR backups in a specific Azure region
# backups are grouped by the logical database id, within each group they are ordered by the timestamp, the earliest backup first
$ltrBackups = Get-AzSqlInstanceDatabaseLongTermRetentionBackup -Location $instance.Location

# get the list of LTR backups from the Azure region under the given managed instance
$ltrBackups = Get-AzSqlInstanceDatabaseLongTermRetentionBackup -Location $instance.Location -InstanceName $instanceName

# get the LTR backups for a specific database from the Azure region under the given managed instance
$ltrBackups = Get-AzSqlInstanceDatabaseLongTermRetentionBackup -Location $instance.Location -InstanceName $instanceName -DatabaseName $dbName

# list LTR backups only from live databases (you have option to choose All/Live/Deleted)
$ltrBackups = Get-AzSqlInstanceDatabaseLongTermRetentionBackup -Location $instance.Location -DatabaseState Live

# only list the latest LTR backup for each database
$ltrBackups = Get-AzSqlInstanceDatabaseLongTermRetentionBackup -Location $instance.Location -InstanceName $instanceName -OnlyLatestPerDatabase
```

## <a name="delete-ltr-backups"></a>LTR yedeklemeleri Sil

Bu örnek, yedeklemeler listesinden bir LTR yedeklemenin nasıl silineceğini gösterir.

```powershell
# remove the earliest backup
$ltrBackup = $ltrBackups[0]
Remove-AzSqlInstanceDatabaseLongTermRetentionBackup -ResourceId $ltrBackup.ResourceId
```

> [!IMPORTANT]
> LTR yedeklemenin silinmesi geri alınamaz. Örnek silindikten sonra bir LTR yedeklemesini silmek için abonelik kapsamı izninizin olması gerekir. Azure Izleyici 'de her silme hakkında bildirimleri, ' uzun süreli bekletme yedeklemesini siler ' işlemi için filtreleyerek ayarlayabilirsiniz. Etkinlik günlüğü, ne zaman ve ne zaman istek yaptığı hakkında bilgiler içerir. Ayrıntılı yönergeler için bkz. [etkinlik günlüğü uyarıları oluşturma](../../azure-monitor/platform/alerts-activity-log.md) .

## <a name="restore-from-ltr-backups"></a>LTR yedeklemelerden geri yükleme

Bu örnek, bir LTR yedeğinden nasıl geri yükleneceğini gösterir. Bu arabirim değişmediği halde kaynak kimliği parametresi artık LTR yedekleme kaynağı kimliğini gerektiriyor.

```powershell
# restore a specific LTR backup as an P1 database on the instance $instanceName of the resource group $resourceGroup
Restore-AzSqlInstanceDatabase -FromLongTermRetentionBackup -ResourceId $ltrBackup.ResourceId `
   -TargetInstanceName $instanceName -TargetResourceGroupName $resourceGroup -TargetInstanceDatabaseName $dbName
```

> [!IMPORTANT]
> Örnek silindikten sonra bir LTR yedeklemesinden geri yüklemek için, örneğin aboneliğine sahip olan ve bu aboneliğin etkin olması gereken izinlere sahip olmanız gerekir. İsteğe bağlı-ResourceGroupName parametresini de atmalısınız.

> [!NOTE]
> Buradan var olan veritabanına kopyalamak için geri yüklenen veritabanından veri ayıklama veya var olan veritabanını silerek geri yüklenen veritabanının adını var olan veritabanının adıyla değiştirme gibi görevleri gerçekleştirmek için SQL Server Management Studio kullanarak geri yüklenen veritabanına bağlanabilirsiniz. Bkz. [zaman noktası geri yükleme](../database/recovery-using-backups.md#point-in-time-restore).

## <a name="next-steps"></a>Sonraki adımlar

- Hizmet tarafından oluşturulan otomatik yedekler hakkında bilgi edinmek için bkz. [otomatik yedekler](../database/automated-backups-overview.md)
- Uzun süreli yedek saklama hakkında bilgi edinmek için bkz. [uzun süreli yedek saklama](../database/long-term-retention-overview.md)
