---
title: Bir coğrafi yedeklemeden veri ambarını geri yükleme
description: Bir SQL havuzunun coğrafi geri yüklenmesi için nasıl yapılır Kılavuzu.
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
ms.openlocfilehash: 624c6665e70802907be8a41015b78d36cca7df1c
ms.sourcegitcommit: 225a0b8a186687154c238305607192b75f1a8163
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/29/2020
ms.locfileid: "78198413"
---
# <a name="geo-restore-for-sql-pool"></a>SQL havuzu için coğrafi geri yükleme

Bu makalede, Azure portal ve PowerShell aracılığıyla SQL havuzunuzu bir coğrafi yedeklemeden geri yüklemeyi öğreneceksiniz.

## <a name="before-you-begin"></a>Başlamadan önce

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

**DTU kapasitenizi doğrulayın.** Her SQL havuzu, varsayılan bir DTU kotasına sahip bir SQL Server (örneğin, myserver.database.windows.net) tarafından barındırılır. SQL Server 'ın geri yüklenmekte olan veritabanı için yeterli DTU kotasına sahip olduğunu doğrulayın. DTU 'yu nasıl hesaplayacağınızı veya daha fazla DTU isteğinde bulunmanız için bkz. [DTU kota değişikliği isteme](sql-data-warehouse-get-started-create-support-ticket.md).

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

Bir SQL havuzunu coğrafi yedeklemeden geri yüklemek için aşağıda özetlenen adımları izleyin:

1. [Azure Portal](https://portal.azure.com/) hesabınızda oturum açın.
1. **+ Kaynak oluştur ' a**tıklayın. 

![Yeni DW](./media/sql-data-warehouse-restore-from-geo-backup/georestore-new.png)

3. **Veritabanları** ' na tıklayın ve ardından * * Azure SYNAPSE Analytics (eskı ADıYLA SQL DW) * *.

![Yeni DW 2](./media/sql-data-warehouse-restore-from-geo-backup/georestore-new-02.png)

4. **Temel** bilgiler sekmesinde istenen bilgileri doldurun ve **İleri**' ye tıklayın.

![Temel Bilgiler](./media/sql-data-warehouse-restore-from-geo-backup/georestore-dw-1.png)

5. **Mevcut veri parametresini kullan** için **Yedekle** ' yi seçin ve aşağı kaydırma seçeneklerinden uygun yedeği seçin. **Gözden geçir + oluştur**' a tıklayın.
 
![yedekleme](./media/sql-data-warehouse-restore-from-geo-backup/georestore-select.png)

6. Veri ambarı geri yüklendikten sonra, **durumun** çevrimiçi olup olmadığını denetleyin.

## <a name="next-steps"></a>Sonraki Adımlar
- [Mevcut bir SQL havuzunu geri yükleme](sql-data-warehouse-restore-active-paused-dw.md)
- [Silinen bir SQL havuzunu geri yükleme](sql-data-warehouse-restore-deleted-dw.md)