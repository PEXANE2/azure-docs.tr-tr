---
title: Silinen bir SQL havuzunun geri yüklemesi
description: Silinen bir SQL havuzunu geri getirmek için nasıl kılavuzluk yapılır?
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
ms.openlocfilehash: d2e2fdb181b553d330368b043b75159e211dd0d2
ms.sourcegitcommit: bd5fee5c56f2cbe74aa8569a1a5bce12a3b3efa6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80745138"
---
# <a name="restore-a-deleted-sql-pool-using-azure-synapse-analytics"></a>Azure Synapse Analytics'i kullanarak silinmiş bir SQL havuzunun geri yüklenmesi

Bu makalede, Azure portalını veya PowerShell'i kullanarak bir SQL'i geri yüklemeyi öğrenirsiniz.

## <a name="before-you-begin"></a>Başlamadan önce

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

**DTU kapasitenizi doğrulayın.** Her SQL havuzu, varsayılan DTU kotası olan bir SQL sunucusu (örneğin, myserver.database.windows.net) tarafından barındırılır.  SQL sunucusunun veritabanının geri yüklenmesi için yeterli DTU kotası kaldığını doğrulayın. DTU'nun gerekli hesaplamasını öğrenmek veya daha fazla DTU istemek için [bkz.](sql-data-warehouse-get-started-create-support-ticket.md)

## <a name="restore-a-deleted-data-warehouse-through-powershell"></a>PowerShell aracılığıyla silinen bir veri ambarı geri yükleme

Silinmiş bir SQL havuzunu geri yüklemek için [Geri Yükleme-AzSqlDatabase](/powershell/module/az.sql/restore-azsqldatabase?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) cmdlet'ini kullanın. Karşılık gelen mantıksal sunucu da silinmişse, bu veri ambarını geri yükleyemezsiniz.

1. Başlamadan önce [Azure PowerShell'i yüklediğinizden](/powershell/azure/overview?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json)emin olun.
2. PowerShell’i açın.
3. Azure hesabınıza bağlanın ve hesabınızla ilişkili tüm abonelikleri listele.
4. Geri yüklenecek silinmiş veri ambarını içeren aboneliği seçin.
5. Belirli silinmiş veri ambarını alın.
6. Silinen veri ambarını geri yükleme
    1. Silinen SQL Veri Ambarı'nı farklı bir mantıksal sunucuya geri yüklemek için diğer mantıksal sunucu adını belirttiğinden emin olun.  Bu mantıksal sunucu farklı bir kaynak grubu ve bölgede de olabilir.
    1. Farklı bir aboneye geri yüklemek için, mantıksal sunucuyu başka bir aboneye taşımak için [Taşı](../../azure-resource-manager/management/move-resource-group-and-subscription.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json#use-the-portal) düğmesini kullanın.
7. Geri yüklenen veri ambarının çevrimiçi olduğunu doğrulayın.
8. Geri yükleme tamamlandıktan sonra, kurtarma sonra [veritabanınızı yapılandırmayı](../../sql-database/sql-database-disaster-recovery.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json#configure-your-database-after-recovery)izleyerek kurtarılan veri ambarını yapılandırabilirsiniz.

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

## <a name="restore-a-deleted-database-using-the-azure-portal"></a>Azure portalını kullanarak silinen bir veritabanını geri yükleme

1. [Azure portalında](https://portal.azure.com/)oturum açın.
2. Silinen veri ambarınızın barındırılan SQL sunucusuna gidin.
3. İçindekiler tablosundaki **Silinmiş veritabanları** simgesini seçin.

    ![Silinen Veritabanları](./media/sql-data-warehouse-restore-deleted-dw/restoring-deleted-01.png)

4. Geri yüklemek istediğiniz silinmiş SQL Veri Ambarı'nı seçin.

    ![Silinen Veritabanları'nı seçin](./media/sql-data-warehouse-restore-deleted-dw/restoring-deleted-11.png)

5. Yeni bir **Veritabanı adı** belirtin ve **Tamam'ı** tıklatın

    ![Veritabanı Adını Belirt](./media/sql-data-warehouse-restore-deleted-dw/restoring-deleted-21.png)

## <a name="next-steps"></a>Sonraki Adımlar

- [Varolan bir SQL havuzunun geri yüklemesi](sql-data-warehouse-restore-active-paused-dw.md)
- [Coğrafi yedekleme SQL havuzundan geri yükleme](sql-data-warehouse-restore-from-geo-backup.md)
