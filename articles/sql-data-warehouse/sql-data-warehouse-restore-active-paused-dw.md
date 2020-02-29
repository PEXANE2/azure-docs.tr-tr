---
title: Mevcut bir veri ambarını geri yükleme
description: Mevcut bir SQL havuzunu geri yüklemek için nasıl yapılır Kılavuzu.
services: sql-data-warehouse
author: anumjs
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 08/29/2018
ms.author: anjangsh
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: bf83de481cf15711da45edf00e63262a05489247
ms.sourcegitcommit: 225a0b8a186687154c238305607192b75f1a8163
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/29/2020
ms.locfileid: "78199555"
---
# <a name="restore-an-existing-sql-pool"></a>Mevcut bir SQL havuzunu geri yükleme

Bu makalede, Azure portal ve PowerShell kullanarak Azure SYNAPSE Analytics 'te var olan bir SQL havuzunu geri yüklemeyi öğreneceksiniz.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

**DTU kapasitenizi doğrulayın.** Her havuz, varsayılan DTU kotasına sahip bir SQL Server (örneğin, myserver.database.windows.net) tarafından barındırılır. SQL Server 'ın geri yüklenmekte olan veritabanının kalan DTU kotasına sahip olduğunu doğrulayın. DTU 'yu nasıl hesaplayacağınızı veya daha fazla DTU isteğinde bulunmanız için bkz. [DTU kota değişikliği isteme](sql-data-warehouse-get-started-create-support-ticket.md).

## <a name="before-you-begin"></a>Başlamadan önce

1. [Azure PowerShell yüklediğinizden](https://docs.microsoft.com/powershell/azure/overview)emin olun.
2. Geri yüklemek istediğiniz mevcut bir geri yükleme noktasına sahip olabilirsiniz. Yeni bir geri yükleme oluşturmak isterseniz, [Kullanıcı tanımlı yeni bir geri yükleme noktası oluşturmak için öğreticiye](sql-data-warehouse-restore-points.md)bakın.

## <a name="restore-an-existing-sql-pool-through-powershell"></a>Mevcut bir SQL havuzunu PowerShell aracılığıyla geri yükleme

Mevcut bir SQL havuzunu geri yükleme noktasından geri yüklemek için [restore-AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/restore-azsqldatabase) PowerShell cmdlet 'ini kullanın.

1. PowerShell’i açın.

2. Azure hesabınıza bağlanın ve hesabınızla ilişkili tüm abonelikleri listeleyin.

3. Geri yüklenecek veritabanını içeren aboneliği seçin.

4. SQL havuzunun geri yükleme noktalarını listeleyin.

5. RestorePointCreationDate kullanarak istenen geri yükleme noktasını seçin.

6. [Restore-AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/restore-azsqldatabase) PowerShell cmdlet 'INI kullanarak SQL havuzunu istenen geri yükleme noktasına geri yükleyin.
        1. SQL havuzunu farklı bir mantıksal sunucuya geri yüklemek için diğer mantıksal sunucu adını belirttiğinizden emin olun.  Bu mantıksal sunucu, farklı bir kaynak grubunda ve bölgede de olabilir.
        2. Farklı bir aboneliğe geri yüklemek için ' Taşı ' düğmesini kullanarak mantıksal sunucuyu başka bir aboneliğe taşıyın.

7. Geri yüklenen SQL havuzunun çevrimiçi olduğunu doğrulayın.

8. Geri yükleme tamamlandıktan sonra, [kurtarma sonrasında veritabanınızı Yapılandır](../sql-database/sql-database-disaster-recovery.md#configure-your-database-after-recovery)' ı IZLEYEREK kurtarılan SQL havuzdan yapılandırma yapabilirsiniz.

```Powershell

$SubscriptionName="<YourSubscriptionName>"
$ResourceGroupName="<YourResourceGroupName>"
$ServerName="<YourServerNameWithoutURLSuffixSeeNote>"  # Without database.windows.net
#$TargetResourceGroupName="<YourTargetResourceGroupName>" # uncomment to restore to a different logical server.
#$TargetServerName="<YourtargetServerNameWithoutURLSuffixSeeNote>"  
$DatabaseName="<YourDatabaseName>"
$NewDatabaseName="<YourDatabaseName>"

Connect-AzAccount
Get-AzSubscription
Select-AzSubscription -SubscriptionName $SubscriptionName

# Or list all restore points
Get-AzSqlDatabaseRestorePoints -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName

# Get the specific database to restore
$Database = Get-AzSqlDatabase -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName

# Pick desired restore point using RestorePointCreationDate "xx/xx/xxxx xx:xx:xx xx"
$PointInTime="<RestorePointCreationDate>"  

# Restore database from a restore point
$RestoredDatabase = Restore-AzSqlDatabase –FromPointInTimeBackup –PointInTime $PointInTime -ResourceGroupName $Database.ResourceGroupName -ServerName $Database.ServerName -TargetDatabaseName $NewDatabaseName –ResourceId $Database.ResourceID

# Use the following command to restore to a different logical server
#$RestoredDatabase = Restore-AzSqlDatabase –FromPointInTimeBackup –PointInTime $PointInTime -ResourceGroupName $Database.ResourceTargetGroupName -ServerName $TargetServerName -TargetDatabaseName $NewDatabaseName –ResourceId $Database.ResourceID

# Verify the status of restored database
$RestoredDatabase.status

```

## <a name="restore-an-existing-sql-pool-through-the-azure-portal"></a>Mevcut bir SQL havuzunu Azure portal aracılığıyla geri yükleme

1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. Geri yüklemek istediğiniz SQL havuzuna gidin.
3. Genel Bakış dikey penceresinin en üstünde **geri yükle**' yi seçin.

    ![ Geri Yüklemeye Genel Bakış](./media/sql-data-warehouse-restore-active-paused-dw/restoring-01.png)

4. **Otomatik geri yükleme noktaları** veya **Kullanıcı tanımlı geri yükleme noktaları**seçeneklerinden birini belirleyin. SQL havuzunda otomatik geri yükleme noktaları yoksa, birkaç saat bekleyin veya geri yüklemeden önce Kullanıcı tanımlı geri yükleme noktası oluşturun. Kullanıcı tanımlı geri yükleme noktaları için, mevcut bir tane seçin veya yeni bir tane oluşturun. **Sunucu**için, farklı bir kaynak grubunda ve bölgede bir mantıksal sunucu seçebilir veya yeni bir tane oluşturabilirsiniz. Tüm parametreleri sağladıktan sonra, **gözden geçir + geri yükle**' ye tıklayın.

    ![Otomatik geri yükleme noktaları](./media/sql-data-warehouse-restore-active-paused-dw/restoring-11.png)

## <a name="next-steps"></a>Sonraki Adımlar
- [Silinen bir SQL havuzunu geri yükleme](sql-data-warehouse-restore-deleted-dw.md)
- [Coğrafi yedekleme SQL havuzundan geri yükleme](sql-data-warehouse-restore-from-geo-backup.md)

 