---
title: PowerShell & REST API 'Leri
description: Bir veritabanının nasıl duraklatılacağını ve sürdürüleceği dahil olmak üzere Azure SYNAPSE Analytics SQL havuzu için en iyi PowerShell cmdlet 'lerini bulun.
services: synapse-analytics
author: kevinvngo
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/17/2018
ms.author: kevin
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: f3d6d0c1f71e2262e943998cdc08717291903365
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80743827"
---
# <a name="powershell--rest-apis-for-azure-synapse-analytics-sql-pool"></a>Azure SYNAPSE Analytics SQL havuzu için PowerShell & REST API 'Leri

Birçok Azure SYNAPSE Analytics SQL havuzu yönetim görevi Azure PowerShell cmdlet 'leri ya da REST API 'Leri kullanılarak yönetilebilir.  Aşağıda, SQL havuzunuzdaki ortak görevleri otomatikleştirmek için PowerShell komutlarının nasıl kullanılacağına ilişkin bazı örnekler verilmiştir.  Bazı iyi REST örnekleri için, [rest ile ölçeklenebilirliği yönetme](sql-data-warehouse-manage-compute-rest-api.md)makalesine bakın.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="get-started-with-azure-powershell-cmdlets"></a>Azure PowerShell cmdlet 'lerini kullanmaya başlayın

1. Windows PowerShell'i açın.
2. PowerShell komut isteminde bu komutları çalıştırarak Azure Resource Manager oturum açın ve aboneliğinizi seçin.

    ```powershell
    Connect-AzAccount
    Get-AzSubscription
    Select-AzSubscription -SubscriptionName "MySubscription"
    ```

## <a name="pause-data-warehouse-example"></a>Veri ambarını Duraklat örneği

"Server01" adlı sunucuda barındırılan "Database02" adlı bir veritabanını duraklatın.  Sunucu, "ResourceGroup1" adlı bir Azure Kaynak grubunda bulunur.

```Powershell
Suspend-AzSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
```

Bir çeşitleme, bu örnek, alınan nesneyi [askıya al-AzSqlDatabase](/powershell/module/az.sql/suspend-azsqldatabase?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json)' A yöneltin.  Sonuç olarak, veritabanı duraklatılır. Son komut sonuçları gösterir.

```Powershell
$database = Get-AzSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Suspend-AzSqlDatabase
$resultDatabase
```

## <a name="start-data-warehouse-example"></a>Veri ambarını Başlat örneği

"Server01" adlı sunucuda barındırılan "Database02" adlı bir veritabanının işlemini sürdürür. Sunucu, "ResourceGroup1" adlı bir kaynak grubunda bulunur.

```Powershell
Resume-AzSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" -DatabaseName "Database02"
```

Bu örnek, "ResourceGroup1" adlı bir kaynak grubunda yer alan "Server01" adlı bir sunucudan "Database02" adlı bir veritabanını alır. It [-AzSqlDatabase](/powershell/module/az.sql/resume-azsqldatabase?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json)için alınan nesneyi kanallar.

```Powershell
$database = Get-AzSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Resume-AzSqlDatabase
```

> [!NOTE]
> Sunucunuz foo.database.windows.net ise PowerShell cmdlet 'lerinde-ServerName olarak "foo" kullanın.

## <a name="other-supported-powershell-cmdlets"></a>Desteklenen diğer PowerShell cmdlet 'leri

Bu PowerShell cmdlet 'leri Azure SYNAPSE Analytics veri ambarı ile desteklenir.

* [Get-AzSqlDatabase](/powershell/module/az.sql/get-azsqldatabase?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json)
* [Get-AzSqlDeletedDatabaseBackup](/powershell/module/az.sql/get-azsqldeleteddatabasebackup?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json)
* [Get-AzSqlDatabaseRestorePoint](/powershell/module/az.sql/get-azsqldatabaserestorepoint?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json)
* [New-AzSqlDatabase](/powershell/module/az.sql/new-azsqldatabase?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json)
* [Remove-AzSqlDatabase](/powershell/module/az.sql/remove-azsqldatabase?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json)
* [Restore-AzSqlDatabase]/PowerShell/Module/az.exe SQL/restore-azsqldatabase? TOC =/Azure/SYNAPSE-Analytics/SQL-Data-Warehouse/TOC.JSON&BC =/Azure/SYNAPSE-Analytics/SQL-Data-Warehouse/breadcrumb/TOC.JSON)
* [Özgeçmişi-AzSqlDatabase](/powershell/module/az.sql/resume-azsqldatabase?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json)
* [Set-AzSqlDatabase](/powershell/module/az.sql/set-azsqldatabase?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json)
* [Suspend-AzSqlDatabase](/powershell/module/az.sql/suspend-azsqldatabase?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json)

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla PowerShell örneği için bkz.:

* [PowerShell kullanarak veri ambarı oluşturma](create-data-warehouse-powershell.md)
* [Veritabanı geri yükleme](sql-data-warehouse-restore-points.md)

PowerShell ile otomatikleştirilen diğer görevler için, bkz. [Azure SQL veritabanı cmdlet 'leri]/PowerShell/Module/az.exe SQL? TOC =/Azure/SYNAPSE-Analytics/SQL-Data-Warehouse/TOC.JSON&BC =/Azure/SYNAPSE-Analytics/SQL-Data-Warehouse/breadcrumb/TOC.JSON). Azure SYNAPSE Analytics veri ambarı için tüm Azure SQL veritabanı cmdlet 'leri desteklenmez. REST ile otomatikleştirilen görevlerin bir listesi için bkz. [Azure SQL veritabanı Için işlemler](/rest/api/sql/?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json).
