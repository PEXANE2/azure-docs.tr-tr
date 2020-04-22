---
title: 'Yönetilen örnek: Uzun vadeli yedekleme tutma (PowerShell)'
description: PowerShell'i kullanarak yönetilen bir Azure SQL Veritabanı örneği için otomatik yedeklemeleri ayrı Azure Blob depolama kaplarında nasıl depolayıp geri yükleyin öğrenin.
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
ms.date: 04/19/2020
ms.openlocfilehash: 24eacb555704593fe44bc2d949de44de163345bc
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81677111"
---
# <a name="manage-azure-sql-database-managed-instance-long-term-backup-retention-powershell"></a>Azure SQL Veritabanı yönetilen örnek uzun vadeli yedekleme tutma (PowerShell) yönetme

Azure SQL Veritabanı yönetilen örneğinde, sınırlı bir genel önizleme özelliği olarak [uzun vadeli bir yedekleme bekletme](sql-database-long-term-retention.md#managed-instance-support) ilkesini (LTR) yapılandırabilirsiniz. Bu, veritabanı yedeklemelerini ayrı Azure Blob depolama kaplarında 10 yıla kadar otomatik olarak tutmanızı sağlar. Daha sonra PowerShell ile bu yedeklemeleri kullanarak bir veritabanı kurtarabilirsiniz.

   > [!IMPORTANT]
   > Yönetilen örnekler için LTR şu anda sınırlı önizlemededir ve ea ve CSP abonelikleri için duruma göre kullanılabilir. Kayıt talebinde bulunmak için lütfen destek konusu **Yedekleme, Geri Yükleme ve İş Sürekliliği/Uzun vadeli yedekleme tutma**altında bir [Azure destek bileti](https://azure.microsoft.com/support/create-ticket/) oluşturun. 


Aşağıdaki bölümler, uzun süreli yedekleme bekletme yapılandırmak, Yedeklemeleri Azure SQL depolama alanında görüntülemek ve Azure SQL depolama alanında bir yedeklemeden geri yüklemek için PowerShell'i nasıl kullanacağınızı gösterir.

## <a name="rbac-roles-to-manage-long-term-retention"></a>Uzun süreli tutma yönetmek için RBAC rolleri

**Get-AzSqlInstanceDatabaseLongTermRetentionBackup** ve **Restore-AzSqlInstanceDatabase**için aşağıdaki rollerden birine sahip olmanız gerekir:

- Abonelik Sahibi rolü veya
- Yönetilen Örnek Katılımcı rolü veya
- Aşağıdaki izinleri içeren özel rol:
  - `Microsoft.Sql/locations/longTermRetentionManagedInstanceBackups/read`
  - `Microsoft.Sql/locations/longTermRetentionManagedInstances/longTermRetentionManagedInstanceBackups/read`
  - `Microsoft.Sql/locations/longTermRetentionManagedInstances/longTermRetentionDatabases/longTermRetentionManagedInstanceBackups/read`

**Remove-AzSqlInstanceDatabaseLongTermRetentionBackup**için aşağıdaki rollerden birine sahip olmanız gerekir:

- Abonelik Sahibi rolü veya
- Aşağıdaki izinle özel rol:
  - `Microsoft.Sql/locations/longTermRetentionManagedInstances/longTermRetentionDatabases/longTermRetentionManagedInstanceBackups/delete`

> [!NOTE]
> Yönetilen Örnek Katılımcı sıfatının LTR yedeklemelerini silme izni yoktur.

RBAC izinleri *abonelik* veya kaynak *grubu* kapsamında verilebilir. Ancak, bırakılan bir örneğe ait LTR yedeklemelerine erişmek için, bu örneğin *abonelik* kapsamında izin verilmesi gerekir.

- `Microsoft.Sql/locations/longTermRetentionManagedInstances/longTermRetentionDatabases/longTermRetentionManagedInstanceBackups/delete`

## <a name="create-an-ltr-policy"></a>LTR ilkesi oluşturma

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

## <a name="view-ltr-policies"></a>LTR ilkelerini görüntüleyin

Bu örnekte, ltr ilkelerinin bir örnek içinde nasıl listelenebildiğini gösterir

```powershell
# gets the current version of LTR policy for the database
$ltrPolicies = Get-AzSqlInstanceDatabaseBackupLongTermRetentionPolicy -InstanceName $instanceName `
    -DatabaseName $dbName -ResourceGroupName $resourceGroup
```

## <a name="clear-an-ltr-policy"></a>Bir LTR ilkesini temizleme

Bu örnek, bir LTR ilkesini veritabanından nasıl temizler

```powershell
Set-AzSqlInstanceDatabaseBackupLongTermRetentionPolicy -InstanceName $instanceName `
   -DatabaseName $dbName -ResourceGroupName $resourceGroup -RemovePolicy
```

## <a name="view-ltr-backups"></a>LTR yedeklemelerini görüntüleyin

Bu örnekte LTR yedeklemelerinin nasıl listelenebildiğini gösterir.

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

## <a name="delete-ltr-backups"></a>LTR yedeklemelerini silme

Bu örnek, bir LTR yedeklemesinin yedekleme listesinden nasıl silinir olduğunu gösterir.

```powershell
# remove the earliest backup
$ltrBackup = $ltrBackups[0]
Remove-AzSqlInstanceDatabaseLongTermRetentionBackup -ResourceId $ltrBackup.ResourceId
```

> [!IMPORTANT]
> LTR yedeklemesi siler. Örnek silindikten sonra bir LTR yedeklemesini silmek için Abonelik kapsamı iznine sahip olmalısınız. 'Uzun süreli bekletme yedeklemesi siler' işlemi için filtre uygulayarak Azure Monitor'da her silme yle ilgili bildirimler ayarlayabilirsiniz. Etkinlik günlüğü, isteğin kim ve ne zaman yapıldığına ilişkin bilgileri içerir. Bkz. Ayrıntılı yönergeler için [etkinlik günlüğü uyarıları oluşturun.](../azure-monitor/platform/alerts-activity-log.md)

## <a name="restore-from-ltr-backups"></a>LTR yedeklemelerinden geri yükleme

Bu örnek, ltr yedeklemesinden nasıl geri yüklenirlerini gösterir. Not, bu arabirim değişmedi ancak kaynak kimliği parametresi artık LTR yedekleme kaynak kimliği gerektirir.

```powershell
# restore a specific LTR backup as an P1 database on the instance $instanceName of the resource group $resourceGroup
Restore-AzSqlInstanceDatabase -FromLongTermRetentionBackup -ResourceId $ltrBackup.ResourceId `
   -TargetInstanceName $instanceName -TargetResourceGroupName $resourceGroup -TargetInstanceDatabaseName $dbName
```

> [!IMPORTANT]
> Örnek silindikten sonra bir LTR yedeklemesinden geri yüklemek için, örneğin aboneliğine kapsamlı izinlere sahip olması ve aboneliğin etkin olması gerekir. İsteğe bağlı -ResourceGroupName parametresini de atlamalısınız.

> [!NOTE]
> Buradan var olan veritabanına kopyalamak için geri yüklenen veritabanından veri ayıklama veya var olan veritabanını silerek geri yüklenen veritabanının adını var olan veritabanının adıyla değiştirme gibi görevleri gerçekleştirmek için SQL Server Management Studio kullanarak geri yüklenen veritabanına bağlanabilirsiniz. [Bkz. zaman geri yükleme stoyonu.](sql-database-recovery-using-backups.md#point-in-time-restore)

## <a name="next-steps"></a>Sonraki adımlar

- Hizmet tarafından oluşturulan otomatik yedekler hakkında bilgi edinmek için bkz. [otomatik yedekler](sql-database-automated-backups.md)
- Uzun süreli yedek saklama hakkında bilgi edinmek için bkz. [uzun süreli yedek saklama](sql-database-long-term-retention.md)
