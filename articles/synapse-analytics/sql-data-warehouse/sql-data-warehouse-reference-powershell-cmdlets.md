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
ms.openlocfilehash: 5f22de08c4eabd3f9a3d6ee29cad3f0a9e8509e0
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80351381"
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

Bir varyasyon, bu örnek, alınan nesneyi [Askıya Alma-AzSqlDatabase'e](https://docs.microsoft.com/powershell/module/az.sql/suspend-azsqldatabase)aktalar.  Sonuç olarak, veritabanı duraklatıldı. Son komut sonuçları gösterir.

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

Bir varyasyon, bu örnek "ResourceGroup1" adlı bir kaynak grubunda bulunan "Server01" adlı bir sunucudan "Database02" adlı bir veritabanı alır. Alınan nesneyi [Resume-AzSqlDatabase'e](https://docs.microsoft.com/powershell/module/az.sql/resume-azsqldatabase)borular.

```Powershell
$database = Get-AzSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Resume-AzSqlDatabase
```

> [!NOTE]
> Sunucunuz foo.database.windows.net ise PowerShell cmdlets'te "foo"yu -ServerName olarak kullanın.
> 
> 

## <a name="other-supported-powershell-cmdlets"></a>Diğer desteklenen PowerShell cmdlets
Bu PowerShell cmdlets Azure Synapse Analytics veri ambarı ile desteklenir.

* [Get-AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldatabase)
* [Get-AzSqlDeletedDatabaseBackup](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldeleteddatabasebackup)
* [Get-AzSqlDatabaseRestorePoint](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldatabaserestorepoint)
* [Yeni-AzSqlVeritabanı](https://docs.microsoft.com/powershell/module/az.sql/new-azsqldatabase)
* [Remove-AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/remove-azsqldatabase)
* [Geri Yükleme-AzSqlVeritabanı](https://docs.microsoft.com/powershell/module/az.sql/restore-azsqldatabase)
* [Özgeçmiş-AzSqlVeritabanı](https://docs.microsoft.com/powershell/module/az.sql/resume-azsqldatabase)
* [Seç-AzSubscription](https://msdn.microsoft.com/library/dn722499.aspx)
* [Set-AzSqlVeritabanı](https://docs.microsoft.com/powershell/module/az.sql/set-azsqldatabase)
* [Askıya Alma-AzSqlVeritabanı](https://docs.microsoft.com/powershell/module/az.sql/suspend-azsqldatabase)

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla PowerShell örneği için bkz:

* [PowerShell'i kullanarak veri ambarı oluşturma](create-data-warehouse-powershell.md)
* [Veritabanı geri yükleme](sql-data-warehouse-restore-points.md)

PowerShell ile otomatikleştirilebilen diğer görevler için [Azure SQL Veritabanı cmdlets'e](https://docs.microsoft.com/powershell/module/az.sql)bakın. Azure Synapse Analytics veri ambarı için tüm Azure SQL Veritabanı cmdletleri desteklenmez.  REST ile otomatikleştirilebilen görevlerin listesi için Azure [SQL Veritabanı Işlemleri'ne](/rest/api/sql/)bakın.
