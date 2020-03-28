---
title: PowerShell örneği - SQL Veri Eşitleme şemasını güncelleştir
description: SQL Data Sync için eşitleme şemasını güncelleştirmek için Azure PowerShell örnek betiği
services: sql-database
ms.service: sql-database
ms.subservice: data-movement
ms.custom: ''
ms.devlang: PowerShell
ms.topic: sample
author: allenwux
ms.author: xiwu
ms.reviewer: carlrab
ms.date: 03/12/2019
ms.openlocfilehash: 0106b80259083c6e5e3e527063a18aae2e7c6cee
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "74421612"
---
# <a name="use-powershell-to-update-the-sync-schema-in-an-existing-sync-group"></a>Mevcut bir eşitleme grubunda eşitleme şemasını güncelleştirmek için PowerShell kullanma

Bu PowerShell örneği, mevcut bir SQL Data Sync eşitleme grubundaki eşitleme şemasını güncelleştirir. Birden çok tabloyu eşitlerken bu betik, eşitleme şemasını verimli bir şekilde güncelleştirmenize yardımcı olur. Bu örnek, GitHub’da [UpdateSyncSchema.ps1](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/sql-data-sync/UpdateSyncSchema.ps1) olarak kullanılabilir olan **UpdateSyncSchema** betiğinin kullanımını göstermektedir.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]
[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]
[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

PowerShell'i yerel olarak yüklemeyi ve kullanmayı seçerseniz, bu öğretici AZ PowerShell 1.4.0 veya daha sonra gerektirir. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-az-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzAccount` komutunu da çalıştırmanız gerekir.

SQL Data Sync hizmetine genel bakış için bkz. [Azure SQL Data Sync ile birden fazla bulut ve şirket içi veritabanı arasında veri eşitleme](../sql-database-sync-data.md).

> [!IMPORTANT]
> Azure SQL Veri Eşitlemesi şu anda Azure SQL veritabanı yönetilen örneğini desteklemez.

## <a name="examples"></a>Örnekler

### <a name="add-all-tables-to-the-sync-schema"></a>Eşitleme şemasına tüm tabloları ekleme

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
| [Güncelleme-AzSqlSyncGroup](https://docs.microsoft.com/powershell/module/az.sql/update-azsqlsyncgroup) | Bir eşitleme grubunu güncelleştirir. |
| [Get-AzSqlSyncMember](https://docs.microsoft.com/powershell/module/az.sql/get-azsqlsyncmember) | Bir eşitleme üyesi hakkındaki bilgileri döndürür. |
| [Al-AzSqlSyncSchema](https://docs.microsoft.com/powershell/module/az.sql/get-azsqlsyncschema) | Bir eşitleme şeması hakkındaki bilgileri döndürür. |
| [Güncelleme-AzSqlSyncSchema](https://docs.microsoft.com/powershell/module/az.sql/update-azsqlsyncschema) | Bir eşitleme şemasını güncelleştirir. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

Ek SQL veritabanı PowerShell komut örnekleri [Azure SQL veritabanı PowerShell komut dosyaları](../sql-database-powershell-samples.md)bulunabilir.

SQL Data Sync hakkında daha fazla bilgi için bkz.:

- Genel Bakış - [Azure SQL Veri Eşitlemi ile verileri birden çok bulut ve şirket içi veritabanları nda eşitleme](../sql-database-sync-data.md)
- Veri Eşitlemeyi Ayarlama
    - Portalda - [Öğretici: Azure SQL Veritabanı ve SQL Server arasında verileri şirket içinde eşitlemek için SQL Veri Eşitlemeyi'ni ayarlayın](../sql-database-get-started-sql-data-sync.md)
    - PowerShell ile
        - [PowerShell kullanarak birden çok Azure SQL veritabanı arasında eşitleme](sql-database-sync-data-between-sql-databases.md)
        - [PowerShell kullanarak bir Azure SQL Veritabanı ile SQL Server şirket içi veritabanı arasında eşitleme](sql-database-sync-data-between-azure-onprem.md)
- Veri Eşitleme Aracısı - [Azure SQL Veri Eşitleme için Veri Eşitleme Aracısı](../sql-database-data-sync-agent.md)
- En iyi uygulamalar - [Azure SQL Veri Eşitleme için en iyi uygulamalar](../sql-database-best-practices-data-sync.md)
- Monitör - [Azure Monitor günlükleriyle SQL Veri Eşitlemesini Izleyin](../sql-database-sync-monitor-oms.md)
- Sorun Giderme - [Azure SQL Veri Eşitleme ile sorun giderme](../sql-database-troubleshoot-data-sync.md)
- Eşitleme şeasını güncelleştirme
    - Transact-SQL ile - [Azure SQL Veri Eşitleme'deki şema değişikliklerinin çoğaltılması otomatikleştirin](../sql-database-update-sync-schema.md)

SQL Veritabanı hakkında daha fazla bilgi için bkz.:

- [SQL Veritabanı'na Genel Bakış](../sql-database-technical-overview.md)
- [Veritabanı Yaşam Döngüsü Yönetimi](https://msdn.microsoft.com/library/jj907294.aspx)
