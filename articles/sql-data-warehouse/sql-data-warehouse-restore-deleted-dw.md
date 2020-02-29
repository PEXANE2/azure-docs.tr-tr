---
title: Silinen bir SQL havuzunu geri yükleme
description: Silinen bir SQL havuzunu geri yükleme kılavuzu.
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
ms.openlocfilehash: 34851203432b7e2daf44e840e45275de76bc3b3a
ms.sourcegitcommit: 225a0b8a186687154c238305607192b75f1a8163
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/29/2020
ms.locfileid: "78196679"
---
# <a name="restore-a-deleted-sql-pool-using-azure-synapse-analytics"></a>Azure SYNAPSE Analytics kullanarak silinen bir SQL havuzunu geri yükleme

Bu makalede, Azure portal veya PowerShell kullanarak bir SQL geri yüklemeyi öğreneceksiniz.

## <a name="before-you-begin"></a>Başlamadan önce

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

**DTU kapasitenizi doğrulayın.** Her SQL havuzu, varsayılan bir DTU kotasına sahip bir SQL Server (örneğin, myserver.database.windows.net) tarafından barındırılır.  SQL Server 'ın geri yüklenmekte olan veritabanı için yeterli DTU kotasına sahip olduğunu doğrulayın. DTU 'yu nasıl hesaplayacağınızı veya daha fazla DTU isteğinde bulunmanız için bkz. [DTU kota değişikliği isteme](sql-data-warehouse-get-started-create-support-ticket.md).

## <a name="restore-a-deleted-data-warehouse-through-powershell"></a>Silinen bir veri ambarını PowerShell aracılığıyla geri yükleme

Silinen bir SQL havuzunu geri yüklemek için [restore-AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/restore-azsqldatabase) cmdlet 'ini kullanın. Karşılık gelen mantıksal sunucu da silinmişse, bu veri ambarını geri alamazsınız.

1. Başlamadan önce [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview)' yi yüklediğinizden emin olun.
2. PowerShell’i açın.
3. Azure hesabınıza bağlanın ve hesabınızla ilişkili tüm abonelikleri listeleyin.
4. Geri yüklenecek silinen veri ambarını içeren aboneliği seçin.
5. Silinen belirli veri ambarını alın.
6. Silinen veri ambarını geri yükleme
    1. Silinen SQL veri ambarını farklı bir mantıksal sunucuya geri yüklemek için diğer mantıksal sunucu adını belirttiğinizden emin olun.  Bu mantıksal sunucu, farklı bir kaynak grubunda ve bölgede de olabilir.
    1. Farklı bir aboneliğe geri dönmek için [Taşı](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-move-resources#use-the-portal) düğmesini kullanarak mantıksal sunucuyu başka bir aboneliğe taşıyın.
1. Geri yüklenen veri ambarının çevrimiçi olduğunu doğrulayın.
1. Geri yükleme tamamlandıktan sonra, [kurtarma sonrasında veritabanınızı yapılandırma](../sql-database/sql-database-disaster-recovery.md#configure-your-database-after-recovery)' yı izleyerek kurtarılan veri Ambarınızı yapılandırabilirsiniz.

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

# Get the deleted database to restore
$DeletedDatabase = Get-AzSqlDeletedDatabaseBackup -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName

# Restore deleted database
$RestoredDatabase = Restore-AzSqlDatabase –FromDeletedDatabaseBackup –DeletionDate $DeletedDatabase.DeletionDate -ResourceGroupName $DeletedDatabase.ResourceGroupName -ServerName $DeletedDatabase.ServerName -TargetDatabaseName $NewDatabaseName –ResourceId $DeletedDatabase.ResourceID

# Use the following command to restore deleted data warehouse to a different logical server
#$RestoredDatabase = Restore-AzSqlDatabase –FromDeletedDatabaseBackup –DeletionDate $DeletedDatabase.DeletionDate -ResourceGroupName $TargetResourceGroupName -ServerName $TargetServerName -TargetDatabaseName $NewDatabaseName –ResourceId $DeletedDatabase.ResourceID

# Verify the status of restored database
$RestoredDatabase.status
```

## <a name="restore-a-deleted-database-using-the-azure-portal"></a>Silinen veritabanını Azure portal kullanarak geri yükleme

1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. Silinen veri ambarınızın barındırıldığından SQL Server 'a gidin.
3. İçindekiler tablosunda **silinen veritabanları** simgesini seçin.

    ![Silinen veritabanları](./media/sql-data-warehouse-restore-deleted-dw/restoring-deleted-01.png)

4. Geri yüklemek istediğiniz silinen SQL veri ambarını seçin.

    ![Silinen veritabanları seçin](./media/sql-data-warehouse-restore-deleted-dw/restoring-deleted-11.png)

5. Yeni bir **veritabanı adı** belirtin ve **Tamam 'a** tıklayın

    ![Veritabanı adını belirtin](./media/sql-data-warehouse-restore-deleted-dw/restoring-deleted-21.png)

## <a name="next-steps"></a>Sonraki Adımlar
- [Mevcut bir SQL havuzunu geri yükleme](sql-data-warehouse-restore-active-paused-dw.md)
- [Coğrafi yedekleme SQL havuzundan geri yükleme](sql-data-warehouse-restore-from-geo-backup.md)