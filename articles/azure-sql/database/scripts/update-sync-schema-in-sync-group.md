---
title: 'PowerShell: güncelleştirme SQL Data Sync eşitleme şeması'
description: SQL Data Sync için eşitleme şemasını güncelleştirmek için Azure PowerShell örnek betiği
services: sql-database
ms.service: sql-database
ms.subservice: data-movement
ms.custom: sqldbrb=1
ms.devlang: PowerShell
ms.topic: sample
author: stevestein
ms.author: sstein
ms.reviewer: carlrab
ms.date: 03/12/2019
ms.openlocfilehash: fc37ccac5a2a3373907fd7d066fb9aa16ace38b8
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84053522"
---
# <a name="use-powershell-to-update-the-sync-schema-in-an-existing-sync-group"></a>Mevcut bir eşitleme grubunda eşitleme şemasını güncelleştirmek için PowerShell kullanma
[!INCLUDE[appliesto-sqldb](../../includes/appliesto-sqldb.md)]

Bu PowerShell örneği, mevcut bir SQL Data Sync eşitleme grubundaki eşitleme şemasını güncelleştirir. Birden çok tabloyu eşitlerken bu betik, eşitleme şemasını verimli bir şekilde güncelleştirmenize yardımcı olur. Bu örnek, GitHub’da [UpdateSyncSchema.ps1](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/sql-data-sync/UpdateSyncSchema.ps1) olarak kullanılabilir olan **UpdateSyncSchema** betiğinin kullanımını göstermektedir.

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]
[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]
[!INCLUDE [cloud-shell-try-it.md](../../../../includes/cloud-shell-try-it.md)]

PowerShell 'i yerel olarak yükleyip kullanmayı tercih ederseniz bu öğretici AZ PowerShell 1.4.0 veya üstünü gerektirir. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-az-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzAccount` komutunu da çalıştırmanız gerekir.

SQL Data Sync hizmetine genel bakış için bkz. [Azure SQL Data Sync ile birden fazla bulut ve şirket içi veritabanı arasında veri eşitleme](../sql-data-sync-data-sql-server-sql-database.md).

> [!IMPORTANT]
> SQL Data Sync Şu anda Azure SQL yönetilen örneğini desteklemez.

## <a name="examples"></a>Örnekler

### <a name="add-all-tables-to-the-sync-schema"></a>Tüm tabloları eşitleme şemasına Ekle

Aşağıdaki örnek, veritabanı şemasını yeniler ve eşitleme şemasının hub veritabanındaki tüm geçerli tabloları ekler.

```powershell-interactive
UpdateSyncSchema.ps1 -SubscriptionId <subscriptionId> -ResourceGroupName <resourceGroupName> -ServerName <serverName> -DatabaseName <databaseName> `
    -SyncGroupName <syncGroupName> -RefreshDatabaseSchema $true -AddAllTables $true
```

### <a name="add-and-remove-tables-and-columns"></a>Tablo ve sütun ekleme ve kaldırma

Aşağıdaki örnek, eşitleme şemasına `[dbo].[Table1]` ve `[dbo].[Table2].[Column1]` ekler ve `[dbo].[Table3]` öğesini kaldırır.

```powershell-interactive
UpdateSyncSchema.ps1 -SubscriptionId <subscriptionId> -ResourceGroupName <resourceGroupName> -ServerName <serverName> -DatabaseName <databaseName> `
    -SyncGroupName <syncGroupName> -TablesAndColumnsToAdd "[dbo].[Table1],[dbo].[Table2].[Column1]" -TablesAndColumnsToRemove "[dbo].[Table3]"
```

## <a name="script-parameters"></a>Betik parametreleri

**UpdateSyncSchema** betiği aşağıdaki parametrelere sahiptir:

| Parametre | Notlar |
|---|---|
| $subscriptionId | Eşitleme grubunun oluşturulduğu abonelik. |
| $resourceGroupName | Eşitleme grubunun oluşturulduğu kaynak grubu.|
| $serverName | Hub veritabanının sunucu adı.|
| $databaseName | Hub veritabanı adı. |
| $syncGroupName | Eşitleme grubu adı. |
| $memberName | Hub veritabanı yerine, eşitleme üyesinden veritabanı şemasını yüklemek istiyorsanız üye adını belirtin. Hub’dan veritabanı şemasını yüklemek istiyorsanız bu parametreyi boş bırakın. |
| $timeoutInSeconds | Betik, veritabanı şemasını yenilediğinde gerçekleşen zaman aşımı. Varsayılan değer 900 saniyedir. |
| $refreshDatabaseSchema | Betiğin, veritabanı şemasını yenilemesi gerekip gerekmediğini belirtin. Önceki yapılandırmadan veritabanı şemanız değiştiyse (örneğin, yeni bir tablo veya yeni bir sütun eklediyseniz), yeniden yapılandırmadan önce şemayı yenilemeniz gerekir. Varsayılan değer false’tur. |
| $addAllTables | Bu değer true olursa, tüm geçerli tablolar ve sütunlar eşitleme şemasına eklenir. $TablesAndColumnsToAdd ve $TablesAndColumnsToRemove değerleri yoksayılır. |
| $tablesAndColumnsToAdd | Eşitleme şemasına eklenecek tabloları veya sütunları belirtin. Her bir tablo veya sütun adının şema adıyla tam olarak ayrılmış olması gerekir. Örneğin: `[dbo].[Table1]`, `[dbo].[Table2].[Column1]`. Birden çok tablo veya sütun adı belirtilebilir ve virgül (,) ile ayrılabilir. |
| $tablesAndColumnsToRemove | Eşitleme şemasından kaldırılacak tabloları veya sütunları belirtin. Her bir tablo veya sütun adının şema adıyla tam olarak ayrılmış olması gerekir. Örneğin: `[dbo].[Table1]`, `[dbo].[Table2].[Column1]`. Birden çok tablo veya sütun adı belirtilebilir ve virgül (,) ile ayrılabilir. |

## <a name="script-explanation"></a>Betik açıklaması

**UpdateSyncSchema** betiği aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [Get-AzSqlSyncGroup](https://docs.microsoft.com/powershell/module/az.sql/get-azsqlsyncgroup) | Bir eşitleme grubu hakkındaki bilgileri döndürür. |
| [Update-AzSqlSyncGroup](https://docs.microsoft.com/powershell/module/az.sql/update-azsqlsyncgroup) | Bir eşitleme grubunu güncelleştirir. |
| [Get-AzSqlSyncMember](https://docs.microsoft.com/powershell/module/az.sql/get-azsqlsyncmember) | Bir eşitleme üyesi hakkındaki bilgileri döndürür. |
| [Get-AzSqlSyncSchema](https://docs.microsoft.com/powershell/module/az.sql/get-azsqlsyncschema) | Bir eşitleme şeması hakkındaki bilgileri döndürür. |
| [Update-AzSqlSyncSchema](https://docs.microsoft.com/powershell/module/az.sql/update-azsqlsyncschema) | Bir eşitleme şemasını güncelleştirir. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

Ek SQL veritabanı PowerShell betiği örnekleri, [Azure SQL veritabanı PowerShell betikleri](../powershell-script-content-guide.md)içinde bulunabilir.

SQL Data Sync hakkında daha fazla bilgi için bkz.:

- Genel Bakış- [Azure 'da SQL Data Sync birden çok bulutta ve şirket içi veritabanlarında veri eşitleme](../sql-data-sync-data-sql-server-sql-database.md)
- Veri eşitlemesini ayarlama
    - Portalda- [öğreticide, Azure SQL veritabanı ve şirket içi SQL Server arasında veri eşitlemek için SQL Data Sync ayarlama](../sql-data-sync-sql-server-configure.md)
    - PowerShell ile
        -  [Azure SQL veritabanı 'nda birden çok veritabanı arasında eşitleme yapmak için PowerShell 'i kullanma](sql-data-sync-sync-data-between-sql-databases.md)
        -  [PowerShell kullanarak Azure SQL veritabanındaki bir veritabanı ile SQL Server örneğindeki bir veritabanı arasında eşitleme](sql-data-sync-sync-data-between-azure-onprem.md)
- Veri eşitleme Aracısı- [Azure 'da SQL Data Sync Için veri eşitleme Aracısı](../sql-data-sync-agent-overview.md)
- En iyi uygulamalar- [Azure 'da SQL Data Sync Için en iyi yöntemler](../sql-data-sync-best-practices.md)
- İzleyici- [Azure izleyici günlükleri ile izleyici SQL Data Sync](../sql-data-sync-monitor-sync.md)
- Sorun giderme- [Azure 'da SQL Data Sync sorunlarını giderme](../sql-data-sync-troubleshoot.md)
- Eşitleme şemasını güncelleştirme
    - Transact-SQL- [Azure 'da SQL Data Sync şema değişikliklerinin çoğaltılmasını otomatikleştirin](../sql-data-sync-update-sync-schema.md)

SQL Veritabanı hakkında daha fazla bilgi için bkz.:

- [SQL Veritabanı'na Genel Bakış](../sql-database-paas-overview.md)
- [Veritabanı Yaşam Döngüsü Yönetimi](https://msdn.microsoft.com/library/jj907294.aspx)
