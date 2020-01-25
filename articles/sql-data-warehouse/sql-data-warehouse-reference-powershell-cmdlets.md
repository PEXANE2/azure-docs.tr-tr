---
title: PowerShell cmdlet'leri
description: Azure SQL veri ambarı için en iyi PowerShell cmdlet 'lerini, bir veritabanını duraklatma ve devam etme dahil olmak üzere bulun.
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 04/17/2018
ms.author: kevin
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: c5f85f102d72ac2e4a0315109748d48573f49407
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76721192"
---
# <a name="powershell-cmdlets-and-rest-apis-for-sql-data-warehouse"></a>SQL veri ambarı için PowerShell cmdlet 'leri ve REST API 'Leri
Birçok SQL veri ambarı yönetim görevi Azure PowerShell cmdlet 'leri ya da REST API 'Leri kullanılarak yönetilebilir.  Aşağıda, SQL veri ambarınızdaki ortak görevleri otomatikleştirmek için PowerShell komutlarının nasıl kullanılacağına ilişkin bazı örnekler verilmiştir.  Bazı iyi REST örnekleri için, [rest ile ölçeklenebilirliği yönetme](sql-data-warehouse-manage-compute-rest-api.md)makalesine bakın.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="get-started-with-azure-powershell-cmdlets"></a>Azure PowerShell cmdlet 'lerini kullanmaya başlayın
1. Windows PowerShell'i açın.
2. PowerShell komut isteminde bu komutları çalıştırarak Azure Resource Manager oturum açın ve aboneliğinizi seçin.
   
    ```powershell
    Connect-AzAccount
    Get-AzSubscription
    Select-AzSubscription -SubscriptionName "MySubscription"
    ```

## <a name="pause-sql-data-warehouse-example"></a>SQL veri ambarı örneğini Duraklat
"Server01" adlı sunucuda barındırılan "Database02" adlı bir veritabanını duraklatın.  Sunucu, "ResourceGroup1" adlı bir Azure Kaynak grubunda bulunur.

```Powershell
Suspend-AzSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
```
Bir çeşitleme, bu örnek, alınan nesneyi [askıya al-AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/suspend-azsqldatabase)' A yöneltin.  Sonuç olarak, veritabanı duraklatılır. Son komut sonuçları gösterir.

```Powershell
$database = Get-AzSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Suspend-AzSqlDatabase
$resultDatabase
```

## <a name="start-sql-data-warehouse-example"></a>SQL veri ambarı örneğini Başlat
"Server01" adlı sunucuda barındırılan "Database02" adlı bir veritabanının işlemini sürdürür. Sunucu, "ResourceGroup1" adlı bir kaynak grubunda bulunur.

```Powershell
Resume-AzSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" -DatabaseName "Database02"
```

Bu örnek, "ResourceGroup1" adlı bir kaynak grubunda yer alan "Server01" adlı bir sunucudan "Database02" adlı bir veritabanını alır. It [-AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/resume-azsqldatabase)için alınan nesneyi kanallar.

```Powershell
$database = Get-AzSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Resume-AzSqlDatabase
```

> [!NOTE]
> Sunucunuz foo.database.windows.net ise PowerShell cmdlet 'lerinde-ServerName olarak "foo" kullanın.
> 
> 

## <a name="other-supported-powershell-cmdlets"></a>Desteklenen diğer PowerShell cmdlet 'leri
Bu PowerShell cmdlet 'leri Azure SQL veri ambarı ile desteklenir.

* [Get-AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldatabase)
* [Get-AzSqlDeletedDatabaseBackup](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldeleteddatabasebackup)
* [Get-AzSqlDatabaseRestorePoint](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldatabaserestorepoint)
* [New-AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/new-azsqldatabase)
* [Remove-AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/remove-azsqldatabase)
* [Restore-AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/restore-azsqldatabase)
* [Özgeçmişi-AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/resume-azsqldatabase)
* [Select-AzSubscription](https://msdn.microsoft.com/library/dn722499.aspx)
* [Set-AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/set-azsqldatabase)
* [Suspend-AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/suspend-azsqldatabase)

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla PowerShell örneği için bkz.:

* [PowerShell kullanarak SQL veri ambarı oluşturma](create-data-warehouse-powershell.md)
* [Veritabanı geri yükleme](sql-data-warehouse-restore-database-powershell.md)

PowerShell ile otomatikleştirilen diğer görevler için bkz. [Azure SQL veritabanı cmdlet 'leri](https://docs.microsoft.com/powershell/module/az.sql). Azure SQL veri ambarı için tüm Azure SQL veritabanı cmdlet 'leri desteklenmez.  REST ile otomatikleştirilen görevlerin bir listesi için bkz. [Azure SQL veritabanı Için işlemler](https://msdn.microsoft.com/library/azure/dn505719.aspx).
