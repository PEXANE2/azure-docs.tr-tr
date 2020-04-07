---
title: PowerShell & REST API'leri
description: Azure Synapse Analytics SQL havuzu nun veritabanını nasıl duraklatacağı ve devam ettirilen dahil olmak üzere en iyi PowerShell cmdlets'ini bulun.
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
ms.sourcegitcommit: bd5fee5c56f2cbe74aa8569a1a5bce12a3b3efa6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80743827"
---
# <a name="powershell--rest-apis-for-azure-synapse-analytics-sql-pool"></a>Azure Synapse Analytics SQL havuzu için PowerShell & REST API'leri

Birçok Azure Synapse Analytics SQL havuzu yönetim görevi, Azure PowerShell cmdlets veya REST API'leri kullanılarak yönetilebilir.  Aşağıda, SQL havuzunuzdaki ortak görevleri otomatikleştirmek için PowerShell komutlarının nasıl kullanılacağına dair bazı örnekler verilmiştir.  Bazı iyi REST örnekleri için, REST [ile ölçeklenebilirliği yönet'e](sql-data-warehouse-manage-compute-rest-api.md)bakın.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="get-started-with-azure-powershell-cmdlets"></a>Azure PowerShell cmdlets ile başlayın

1. Windows PowerShell'i açın.
2. PowerShell komut isteminde, Azure Kaynak Yöneticisi'nde oturum açmak ve aboneliğinizi seçmek için bu komutları çalıştırın.

    ```powershell
    Connect-AzAccount
    Get-AzSubscription
    Select-AzSubscription -SubscriptionName "MySubscription"
    ```

## <a name="pause-data-warehouse-example"></a>Veri ambarı örneğini duraklatma

"Server01" adlı bir sunucuda barındırılan "Database02" adlı bir veritabanını duraklatın.  Sunucu, "ResourceGroup1" adlı bir Azure kaynak grubundadır.

```Powershell
Suspend-AzSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
```

Bir varyasyon, bu örnek, alınan nesneyi [Askıya Alma-AzSqlDatabase'e](/powershell/module/az.sql/suspend-azsqldatabase?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json)aktalar.  Sonuç olarak, veritabanı duraklatıldı. Son komut sonuçları gösterir.

```Powershell
$database = Get-AzSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Suspend-AzSqlDatabase
$resultDatabase
```

## <a name="start-data-warehouse-example"></a>Veri ambarı örneğini başlat

"Server01" adlı bir sunucuda barındırılan "Database02" adlı veritabanının çalışmasını devam ettirin. Sunucu,"ResourceGroup1" adlı bir kaynak grubunda bulunur.

```Powershell
Resume-AzSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" -DatabaseName "Database02"
```

Bir varyasyon, bu örnek "ResourceGroup1" adlı bir kaynak grubunda bulunan "Server01" adlı bir sunucudan "Database02" adlı bir veritabanı alır. Alınan nesneyi [Resume-AzSqlDatabase'e](/powershell/module/az.sql/resume-azsqldatabase?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json)borular.

```Powershell
$database = Get-AzSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Resume-AzSqlDatabase
```

> [!NOTE]
> Sunucunuz foo.database.windows.net ise PowerShell cmdlets'te "foo"yu -ServerName olarak kullanın.

## <a name="other-supported-powershell-cmdlets"></a>Diğer desteklenen PowerShell cmdlets

Bu PowerShell cmdlets Azure Synapse Analytics veri ambarı ile desteklenir.

* [Get-AzSqlDatabase](/powershell/module/az.sql/get-azsqldatabase?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json)
* [Get-AzSqlDeletedDatabaseBackup](/powershell/module/az.sql/get-azsqldeleteddatabasebackup?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json)
* [Get-AzSqlDatabaseRestorePoint](/powershell/module/az.sql/get-azsqldatabaserestorepoint?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json)
* [Yeni-AzSqlVeritabanı](/powershell/module/az.sql/new-azsqldatabase?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json)
* [Remove-AzSqlDatabase](/powershell/module/az.sql/remove-azsqldatabase?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json)
* [Geri yükleme-AzSqlVeritabanı]/powershell/module/az.sql/restore-azsqldatabase?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/crumbcrumb/toc.json)
* [Özgeçmiş-AzSqlVeritabanı](/powershell/module/az.sql/resume-azsqldatabase?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json)
* [Set-AzSqlVeritabanı](/powershell/module/az.sql/set-azsqldatabase?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json)
* [Askıya Alma-AzSqlVeritabanı](/powershell/module/az.sql/suspend-azsqldatabase?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json)

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla PowerShell örneği için bkz:

* [PowerShell'i kullanarak veri ambarı oluşturma](create-data-warehouse-powershell.md)
* [Veritabanı geri yükleme](sql-data-warehouse-restore-points.md)

PowerShell ile otomatikleştirilebilen diğer görevler için bkz: [Azure SQL Veritabanı cmdlets]/powershell/module/az.sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json). Azure Synapse Analytics veri ambarı için tüm Azure SQL Veritabanı cmdletleri desteklenmez. REST ile otomatikleştirilebilen görevlerin listesi için Azure [SQL Veritabanı Işlemleri'ne](/rest/api/sql/?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json)bakın.
