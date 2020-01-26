---
title: Bir coğrafi yedeklemeden veri ambarını geri yükleme
description: Azure SQL veri ambarı coğrafi geri yükleme için nasıl yapılır Kılavuzu.
services: sql-data-warehouse
author: anumjs
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 07/12/2019
ms.author: anjangsh
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: 69ba3ed981a27dfff41ea9ea52e1da769a9366c4
ms.sourcegitcommit: b5d646969d7b665539beb18ed0dc6df87b7ba83d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/26/2020
ms.locfileid: "76759645"
---
# <a name="geo-restore-azure-sql-data-warehouse"></a>Azure SQL veri ambarını coğrafi olarak geri yükleme

Bu makalede, Azure portal ve PowerShell aracılığıyla veri Ambarınızı coğrafi yedeklemeden geri yüklemeyi öğreneceksiniz.

## <a name="before-you-begin"></a>Başlamadan önce

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

**DTU kapasitenizi doğrulayın.** Her SQL veri ambarı, varsayılan DTU kotasına sahip bir SQL Server (örneğin, myserver.database.windows.net) tarafından barındırılır. SQL Server 'ın geri yüklenmekte olan veritabanı için yeterli DTU kotasına sahip olduğunu doğrulayın. DTU 'yu nasıl hesaplayacağınızı veya daha fazla DTU isteğinde bulunmanız için bkz. [DTU kota değişikliği isteme](sql-data-warehouse-get-started-create-support-ticket.md).

## <a name="restore-from-an-azure-geographical-region-through-powershell"></a>PowerShell aracılığıyla Azure coğrafi bölgesinden geri yükleme

Bir coğrafi yedeklemeden geri yüklemek için [Get-AzSqlDatabaseGeoBackup](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldatabasegeobackup) ve [restore-azsqldatabase](https://docs.microsoft.com/powershell/module/az.sql/restore-azsqldatabase) cmdlet 'ini kullanın.

> [!NOTE]
> Gen2 'e coğrafi geri yükleme yapabilirsiniz! Bunu yapmak için, isteğe bağlı bir parametre olarak bir Gen2 ServiceObjectiveName (ör. DW1000**c**) belirtin.
>

1. Başlamadan önce [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview)' yi yüklediğinizden emin olun.
2. PowerShell’i açın.
2. Azure hesabınıza bağlanın ve hesabınızla ilişkili tüm abonelikleri listeleyin.
3. Geri yüklenecek veri ambarını içeren aboneliği seçin.
4. Kurtarmak istediğiniz veri ambarını alın.
5. Veri ambarı için kurtarma isteği oluşturun.
6. Coğrafi olarak geri yüklenen veri ambarının durumunu doğrulayın.
7. Geri yükleme tamamlandıktan sonra veri Ambarınızı yapılandırmak için, [kurtarma sonrasında veritabanınızı yapılandırma]( ../sql-database/sql-database-disaster-recovery.md#configure-your-database-after-recovery)konusuna bakın.

```Powershell
$SubscriptionName="<YourSubscriptionName>"
$ResourceGroupName="<YourResourceGroupName>"
$ServerName="<YourServerNameWithoutURLSuffixSeeNote>"  # Without database.windows.net
$TargetResourceGroupName="<YourTargetResourceGroupName>" # Restore to a different logical server.
$TargetServerName="<YourtargetServerNameWithoutURLSuffixSeeNote>"  
$DatabaseName="<YourDatabaseName>"
$NewDatabaseName="<YourDatabaseName>"
$TargetServiceObjective="<YourTargetServiceObjective-DWXXXc>"

Connect-AzAccount
Get-AzSubscription
Select-AzSubscription -SubscriptionName $SubscriptionName
Get-AzureSqlDatabase -ServerName $ServerName

# Get the data warehouse you want to recover
$GeoBackup = Get-AzSqlDatabaseGeoBackup -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName

# Recover data warehouse
$GeoRestoredDatabase = Restore-AzSqlDatabase –FromGeoBackup -ResourceGroupName $TargetResourceGroupName -ServerName $TargetServerName -TargetDatabaseName $NewDatabaseName –ResourceId $GeoBackup.ResourceID -ServiceObjectiveName $TargetServiceObjective

# Verify that the geo-restored data warehouse is online
$GeoRestoredDatabase.status
```

Kaynak veritabanı TDE etkinse Kurtarılan veritabanı TDE etkinleştirilir.

## <a name="restore-from-an-azure-geographical-region-through-azure-portal"></a>Azure portal aracılığıyla bir Azure coğrafi bölgesinden geri yükleme

Bir coğrafi yedeklemeden Azure SQL veri ambarı 'nı geri yüklemek için aşağıda açıklanan adımları izleyin:

1. [Azure Portal](https://portal.azure.com/) hesabınızda oturum açın.
1. **+ Kaynak oluştur ' a** tıklayın ve SQL veri ambarı için arama yapın ve **Oluştur**' a tıklayın.

    ![Yeni DW](./media/sql-data-warehouse-restore-from-geo-backup/georestore-new.png)
1. **Temel** bilgiler sekmesinde istenen bilgileri doldurun ve **İleri**' ye tıklayın.

    ![Temel Bilgiler](./media/sql-data-warehouse-restore-from-geo-backup/georestore-dw-1.png)
1. **Mevcut veri parametresini kullan** için **Yedekle** ' yi seçin ve aşağı kaydırma seçeneklerinden uygun yedeği seçin. **Gözden geçir + oluştur**' a tıklayın.
 
   ![backup](./media/sql-data-warehouse-restore-from-geo-backup/georestore-select.png)
2. Veri ambarı geri yüklendikten sonra, **durumun** çevrimiçi olup olmadığını denetleyin.

## <a name="next-steps"></a>Sonraki Adımlar
- [Mevcut bir veri ambarını geri yükleme](sql-data-warehouse-restore-active-paused-dw.md)
- [Silinen bir veri ambarını geri yükleme](sql-data-warehouse-restore-deleted-dw.md)