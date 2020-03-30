---
title: Mevcut bir veri ambarını geri yükleme
description: Varolan bir SQL havuzunu geri getirmek için nasıl yapılır kılavuzu.
services: synapse-analytics
author: anumjs
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 08/29/2018
ms.author: anjangsh
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: 635b65d16ae9a59816506023d323243f043ce7da
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80350382"
---
# <a name="restore-an-existing-sql-pool"></a>Varolan bir SQL havuzunun geri yüklemesi

Bu makalede, Azure portalı ve PowerShell'i kullanarak Azure Synapse Analytics'te varolan bir SQL havuzunu nasıl geri yükleyebilirsiniz.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

**DTU kapasitenizi doğrulayın.** Her havuz, varsayılan DTU kotası olan bir SQL sunucusu (örneğin, myserver.database.windows.net) tarafından barındırılır. SQL sunucusunun veritabanının geri yüklenmesi için yeterli DTU kotası kaldığını doğrulayın. DTU'nun gerekli hesaplamasını öğrenmek veya daha fazla DTU istemek için [bkz.](sql-data-warehouse-get-started-create-support-ticket.md)

## <a name="before-you-begin"></a>Başlamadan önce

1. [Azure PowerShell'i yüklediğinizden](https://docs.microsoft.com/powershell/azure/overview)emin olun.
2. Geri yüklemek istediğiniz varolan bir geri yükleme noktasına sahip. Yeni bir geri yükleme oluşturmak istiyorsanız, [yeni bir kullanıcı tanımlı geri yükleme noktası oluşturmak için öğreticiye](sql-data-warehouse-restore-points.md)bakın.

## <a name="restore-an-existing-sql-pool-through-powershell"></a>PowerShell üzerinden varolan bir SQL havuzunu geri yükleme

Varolan bir SQL havuzunu geri yükleme noktasından geri yüklemek için [Geri Yükleme-AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/restore-azsqldatabase) PowerShell cmdlet'ini kullanın.

1. PowerShell’i açın.

2. Azure hesabınıza bağlanın ve hesabınızla ilişkili tüm abonelikleri listele.

3. Geri yüklenecek veritabanını içeren aboneliği seçin.

4. SQL havuzu için geri yükleme noktalarını listele.

5. RestorePointCreationDate'i kullanarak istediğiniz geri yükleme noktasını seçin.

6. [Geri Yükleme-AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/restore-azsqldatabase) PowerShell cmdlet kullanarak SQL havuzunu istenilen geri yükleme noktasına geri yükleyin.
        1. SQL havuzunu farklı bir mantıksal sunucuya geri yüklemek için diğer mantıksal sunucu adını belirttiğinden emin olun.  Bu mantıksal sunucu farklı bir kaynak grubu ve bölgede de olabilir.
        2. Farklı bir aboneye geri yüklemek için, mantıksal sunucuyu başka bir aboneye taşımak için 'Taşı' düğmesini kullanın.

7. Geri yüklenen SQL havuzunun çevrimiçi olduğunu doğrulayın.

8. Geri yükleme tamamlandıktan sonra, kurtarma sonra [veritabanınızı yapılandırmayı](../../sql-database/sql-database-disaster-recovery.md#configure-your-database-after-recovery)izleyerek kurtarılan SQL havuzu nu yapılandırabilirsiniz.

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

## <a name="restore-an-existing-sql-pool-through-the-azure-portal"></a>Azure portalı üzerinden varolan bir SQL havuzunu geri yükleme

1. [Azure portalında](https://portal.azure.com/)oturum açın.
2. Geri yüklemek istediğiniz SQL havuzuna gidin.
3. Genel Bakış bıçağının üst kısmında Geri **Yükle'yi**seçin.

    ![ Geri Yüklemeye Genel Bakış](./media/sql-data-warehouse-restore-active-paused-dw/restoring-01.png)

4. Otomatik **Geri Yükleme Noktaları** veya **Kullanıcı Tanımlı Geri Yükleme Noktaları'nı**seçin. SQL havuzunda otomatik geri yükleme noktası yoksa, geri yüklemeden önce birkaç saat bekleyin veya kullanıcı tanımlı bir geri yükleme noktası oluşturun. Kullanıcı Tanımlı Geri Yükleme Noktaları için varolan bir nokta seçin veya yeni bir nokta oluşturun. **Sunucu**için, farklı bir kaynak grubunda ve bölgesinde mantıksal bir sunucu seçebilir veya yeni bir sunucu oluşturabilirsiniz. Tüm parametreleri sağladıktan sonra **Gözden Geçir + Geri Yükle'yi**tıklatın.

    ![Otomatik Geri Yükleme Noktaları](./media/sql-data-warehouse-restore-active-paused-dw/restoring-11.png)

## <a name="next-steps"></a>Sonraki Adımlar
- [Silinen bir SQL havuzunun geri yüklemesi](sql-data-warehouse-restore-deleted-dw.md)
- [Coğrafi yedekleme SQL havuzundan geri yükleme](sql-data-warehouse-restore-from-geo-backup.md)

 