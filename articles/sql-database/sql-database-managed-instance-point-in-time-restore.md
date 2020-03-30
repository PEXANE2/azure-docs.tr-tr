---
title: Yönetilen örnek - Zamanında geri yükleme (PITR)
description: Yönetilen bir durumda bir SQL veritabanını önceki bir noktaya geri yükleyin.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: sstein, carlrab, mathoma
ms.date: 08/25/2019
ms.openlocfilehash: 27f465e6864d0ff639e825c8a816d86648bd8853
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79268814"
---
# <a name="restore-a-sql-database-in-a-managed-instance-to-a-previous-point-in-time"></a>Yönetilen bir durumda bir SQL veritabanını önceki bir noktaya geri yükleme

Geçmişte bir zamanlar başka bir veritabanının kopyası olarak bir veritabanı oluşturmak için zaman içinde geri yükleme (PITR) kullanın. Bu makalede, Azure SQL Veritabanı yönetilen bir örnekte veritabanının zamanında geri yüklemesi nasıl yapılacağını açıklanmaktadır.

Zaman içinde geri yükleme, hatalardan kaynaklanan olaylar, yanlış yüklenen veriler veya önemli verilerin silinmesi gibi kurtarma senaryolarında yararlıdır. Ayrıca sadece test veya denetim için kullanabilirsiniz. Yedekleme dosyaları veritabanı ayarlarınıza bağlı olarak 7 ila 35 gün boyunca saklanır.

Zamanında geri yükleme veritabanını geri yükleyebilir:

- varolan bir veritabanından.
- silinmiş bir veritabanından.
- aynı yönetilen örne veya yönetilen başka bir örne. 

## <a name="limitations"></a>Sınırlamalar

Yönetilen bir örneğin zaman geri yüklemesi noktası aşağıdaki sınırlamalara sahiptir:

- Yönetilen bir örnekten diğerine geri alırken, her iki örnekde de aynı abonelikte ve bölgede olmalıdır. Bölgeler arası ve çapraz abonelik geri yüklemesi şu anda desteklenmez.
- Yönetilen bir örneğin zamanında geri yüklemesi mümkün değildir. Bu makalede, yalnızca nelerin mümkün olduğu açıklanmaktadır: yönetilen bir örnekte barındırılan bir veritabanının zamanında geri yüklenir.

> [!WARNING]
> Yönetilen örneğinizin depolama boyutuna dikkat edin. Geri yüklenecek verilerin boyutuna bağlı olarak, örnek depolama nız tükenebilir. Geri yüklenen veriler için yeterli alan yoksa, farklı bir yaklaşım kullanın.

Aşağıdaki tablo, yönetilen örnekler için zaman içinde geri yükleme senaryolarını gösterir:

|           |Varolan DB'yi aynı yönetilen örneğe geri yükleme| Varolan DB'yi yönetilen başka bir örneğe geri yükleme|Bırakılan DB'yi aynı yönetilen örneğe geri yükleme|Bırakılan DB'yi başka bir yönetilen örneğe geri yükleme|
|:----------|:----------|:----------|:----------|:----------|
|**Azure portalında**| Evet|Hayır |Evet|Hayır|
|**Azure CLI**|Evet |Evet |Hayır|Hayır|
|**Powershell**| Evet|Evet |Evet|Evet|

## <a name="restore-an-existing-database"></a>Varolan bir veritabanını geri yükleme

Azure portalını, PowerShell'i veya Azure CLI'yi kullanarak varolan bir veritabanını aynı örne geri yükleyin. Bir veritabanını başka bir örneğe geri yüklemek için, hedef yönetilen örnek ve kaynak grubunun özelliklerini belirtmek için PowerShell veya Azure CLI'yi kullanın. Bu parametreleri belirtmezseniz, veritabanı varsayılan olarak varolan örneğin geri yüklenir. Azure portalı şu anda başka bir örne geri geri verilmesini desteklemiyor.

# <a name="portal"></a>[Portal](#tab/azure-portal)

1. [Azure portalında](https://portal.azure.com)oturum açın. 
2. Yönetilen örneğinize gidin ve geri yüklemek istediğiniz veritabanını seçin.
3. Veritabanı sayfasında **Geri Yükle'yi** seçin:

    ![Azure portalını kullanarak veritabanını geri yükleme](media/sql-database-managed-instance-point-in-time-restore/restore-database-to-mi.png)

4. Geri **Yükleme** sayfasında, veritabanını geri yüklemek istediğiniz tarih ve saat için noktayı seçin.
5. Veritabanınızı geri yüklemek için **Onayla'yı** seçin. Bu eylem, yeni bir veritabanı oluşturan ve zaman içinde belirtilen noktada özgün veritabanından veri ile dolduran geri yükleme işlemini başlatır. Kurtarma işlemi hakkında daha fazla bilgi için [Kurtarma süresine](sql-database-recovery-using-backups.md#recovery-time)bakın.

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)

Azure PowerShell yüklü [değilseniz,](https://docs.microsoft.com/powershell/azure/install-az-ps)bkz.

PowerShell'i kullanarak veritabanını geri yüklemek için aşağıdaki komuttaki parametreler için değerlerinizi belirtin. Ardından, komutu çalıştırın:

```powershell-interactive
$subscriptionId = "<Subscription ID>"
$resourceGroupName = "<Resource group name>"
$managedInstanceName = "<Managed instance name>"
$databaseName = "<Source-database>"
$pointInTime = "2018-06-27T08:51:39.3882806Z"
$targetDatabase = "<Name of new database to be created>"

Get-AzSubscription -SubscriptionId $subscriptionId
Select-AzSubscription -SubscriptionId $subscriptionId

Restore-AzSqlInstanceDatabase -FromPointInTimeBackup `
                              -ResourceGroupName $resourceGroupName `
                              -InstanceName $managedInstanceName `
                              -Name $databaseName `
                              -PointInTime $pointInTime `
                              -TargetInstanceDatabaseName $targetDatabase `
```

Veritabanını başka bir yönetilen örneğe geri yüklemek için, hedef kaynak grubunun ve hedef yönetilen örneğin adlarını da belirtin:  

```powershell-interactive
$targetResourceGroupName = "<Resource group of target managed instance>"
$targetInstanceName = "<Target managed instance name>"

Restore-AzSqlInstanceDatabase -FromPointInTimeBackup `
                              -ResourceGroupName $resourceGroupName `
                              -InstanceName $managedInstanceName `
                              -Name $databaseName `
                              -PointInTime $pointInTime `
                              -TargetInstanceDatabaseName $targetDatabase `
                              -TargetResourceGroupName $targetResourceGroupName `
                              -TargetInstanceName $targetInstanceName 
```

Ayrıntılar için [bkz.](https://docs.microsoft.com/powershell/module/az.sql/restore-azsqlinstancedatabase)

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Azure CLI'yi zaten yüklü değilseniz, [Install the Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest)bkz.

Azure CLI'yi kullanarak veritabanını geri yüklemek için aşağıdaki komuttaki parametreler için değerlerinizi belirtin. Ardından, komutu çalıştırın:

```azurecli-interactive
az sql midb restore -g mygroupname --mi myinstancename |
-n mymanageddbname --dest-name targetmidbname --time "2018-05-20T05:34:22"
```

Veritabanını yönetilen başka bir örneğe geri yüklemek için, hedef kaynak grubunun ve yönetilen örneğin adlarını da belirtin:  

```azurecli-interactive
az sql midb restore -g mygroupname --mi myinstancename -n mymanageddbname |
       --dest-name targetmidbname --time "2018-05-20T05:34:22" |
       --dest-resource-group mytargetinstancegroupname |
       --dest-mi mytargetinstancename
```

Kullanılabilir parametrelerin ayrıntılı bir açıklaması için, [yönetilen bir örnekte veritabanını geri yükleyecek CLI belgelerine](https://docs.microsoft.com/cli/azure/sql/midb?view=azure-cli-latest#az-sql-midb-restore)bakın.

---

## <a name="restore-a-deleted-database"></a>Silinen veritabanını geri yükleme

Silinen bir veritabanını geri kullanma, PowerShell veya Azure portalı kullanılarak yapılabilir. Silinen bir veritabanını aynı örne geri yüklemek için Azure portalını veya PowerShell'i kullanın. Silinen bir veritabanını başka bir örneğe geri yüklemek için PowerShell'i kullanın. 

### <a name="portal"></a>Portal 


Azure portalını kullanarak yönetilen bir veritabanını kurtarmak için yönetilen örneğe genel bakış sayfasını açın ve **Silinmiş veritabanlarını**seçin. Geri yüklemek istediğiniz silinmiş bir veritabanı seçin ve yedeklemeden geri yüklenen verilerle oluşturulacak yeni veritabanının adını yazın.

  ![Silinen Azure SQL örnek veritabanını geri yükleme ekran görüntüsü](./media/sql-database-recovery-using-backups/restore-deleted-sql-managed-instance-annotated.png)

### <a name="powershell"></a>PowerShell

Veritabanını aynı örne geri yüklemek için parametre değerlerini güncelleştirin ve ardından aşağıdaki PowerShell komutunu çalıştırın: 

```powershell-interactive
$subscriptionId = "<Subscription ID>"
Get-AzSubscription -SubscriptionId $subscriptionId
Select-AzSubscription -SubscriptionId $subscriptionId

$resourceGroupName = "<Resource group name>"
$managedInstanceName = "<Managed instance name>"
$deletedDatabaseName = "<Source database name>"
$targetDatabaseName = "<target database name>"

$deletedDatabase = Get-AzSqlDeletedInstanceDatabaseBackup -ResourceGroupName $resourceGroupName `
-InstanceName $managedInstanceName -DatabaseName $deletedDatabaseName

Restore-AzSqlinstanceDatabase -Name $deletedDatabase.Name `
   -InstanceName $deletedDatabase.ManagedInstanceName `
   -ResourceGroupName $deletedDatabase.ResourceGroupName `
   -DeletionDate $deletedDatabase.DeletionDate `
   -PointInTime UTCDateTime `
   -TargetInstanceDatabaseName $targetDatabaseName
```

Veritabanını başka bir yönetilen örneğe geri yüklemek için, hedef kaynak grubunun ve hedef yönetilen örneğin adlarını da belirtin:

```powershell-interactive
$targetResourceGroupName = "<Resource group of target managed instance>"
$targetInstanceName = "<Target managed instance name>"

Restore-AzSqlinstanceDatabase -Name $deletedDatabase.Name `
   -InstanceName $deletedDatabase.ManagedInstanceName `
   -ResourceGroupName $deletedDatabase.ResourceGroupName `
   -DeletionDate $deletedDatabase.DeletionDate `
   -PointInTime UTCDateTime `
   -TargetInstanceDatabaseName $targetDatabaseName `
   -TargetResourceGroupName $targetResourceGroupName `
   -TargetInstanceName $targetInstanceName 
```

## <a name="overwrite-an-existing-database"></a>Varolan bir veritabanının üzerine yazma

Varolan bir veritabanının üzerine yazmak için şunları

1. Üzerine yazmak istediğiniz varolan veritabanını bırakın.
2. Zamanında geri yüklenen veritabanını, bıraktığınız veritabanının adına yeniden adlandırın.

### <a name="drop-the-original-database"></a>Özgün veritabanını bırakma

Azure portalını, PowerShell'i veya Azure CLI'yi kullanarak veritabanını bırakabilirsiniz.

Ayrıca yönetilen örne doğrudan bağlanarak, SQL Server Management Studio'yu (SSMS) başlatarak ve aşağıdaki Transact-SQL (T-SQL) komutunu çalıştırarak veritabanını bırakabilirsiniz:

```sql
DROP DATABASE WorldWideImporters;
```

Yönetilen örnekte veritabanınıza bağlanmak için aşağıdaki yöntemlerden birini kullanın:

- [Azure sanal makinesi aracılığıyla SSMS/Azure Veri Stüdyosu](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-configure-vm)
- [Noktadan siteye](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-configure-p2s)
- [Genel uç nokta](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-public-endpoint-configure)

# <a name="portal"></a>[Portal](#tab/azure-portal)

Azure portalında, yönetilen örnekten veritabanını seçin ve sonra **Sil'i**seçin.

   ![Azure portalını kullanarak veritabanını silme](media/sql-database-managed-instance-point-in-time-restore/delete-database-from-mi.png)

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)

Varolan bir veritabanını yönetilen bir örnekten düşürmek için aşağıdaki PowerShell komutunu kullanın:

```powershell
$resourceGroupName = "<Resource group name>"
$managedInstanceName = "<Managed instance name>"
$databaseName = "<Source database>"

Remove-AzSqlInstanceDatabase -Name $databaseName -InstanceName $managedInstanceName -ResourceGroupName $resourceGroupName
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Varolan bir veritabanını yönetilen bir örnekten düşürmek için aşağıdaki Azure CLI komutunu kullanın:

```azurecli-interactive
az sql midb delete -g mygroupname --mi myinstancename -n mymanageddbname
```

---

### <a name="alter-the-new-database-name-to-match-the-original-database-name"></a>Özgün veritabanı adıyla eşleşecek şekilde yeni veritabanı adını değiştirme

Doğrudan yönetilen örne bağlanın ve SQL Server Management Studio'yu başlatın. Ardından, aşağıdaki Transact-SQL (T-SQL) sorgusunu çalıştırın. Sorgu, geri yüklenen veritabanının adını, üzerine yazmak istediğiniz bırakılan veritabanıyla değiştirir.

```sql
ALTER DATABASE WorldWideImportersPITR MODIFY NAME = WorldWideImporters;
```

Yönetilen örnekte veritabanınıza bağlanmak için aşağıdaki yöntemlerden birini kullanın:

- [Azure sanal makine](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-configure-vm)
- [Noktadan siteye](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-configure-p2s)
- [Genel uç nokta](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-public-endpoint-configure)

## <a name="next-steps"></a>Sonraki adımlar

Otomatik [yedeklemeler](sql-database-automated-backups.md)hakkında bilgi edinin.
