---
title: Yönetilen örnek-zamanında geri yükleme (ıNR)
description: Yönetilen bir örnekteki SQL veritabanını önceki bir noktaya geri yükleyin.
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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79268814"
---
# <a name="restore-a-sql-database-in-a-managed-instance-to-a-previous-point-in-time"></a>Yönetilen bir örnekteki SQL veritabanını önceki zaman noktasına geri yükleme

Bir veritabanını geçmişteki bir zamanda başka bir veritabanının kopyası olarak oluşturmak için belirli bir noktaya geri yükleme (ıNR) kullanın. Bu makalede, bir Azure SQL veritabanı yönetilen örneğinde bir veritabanının bir noktadan noktaya geri yüklemesi nasıl yapılacağı açıklanır.

Zaman içinde geri yükleme, hatalardan kaynaklanan olaylar, yanlış yüklenmiş veriler veya önemli verilerin silinmesi gibi kurtarma senaryolarında yararlıdır. Bunu yalnızca test veya denetim için de kullanabilirsiniz. Yedekleme dosyaları, veritabanı ayarlarınıza bağlı olarak 7-35 gün boyunca tutulur.

Bir zaman noktaya geri yükleme bir veritabanını geri yükleyebilir:

- Mevcut bir veritabanından.
- Silinen bir veritabanından.
- aynı yönetilen örneğe veya başka bir yönetilen örneğe. 

## <a name="limitations"></a>Sınırlamalar

Yönetilen bir örneğe geri yükleme sırasında bir noktaya geri yükleme aşağıdaki sınırlamalara sahiptir:

- Yönetilen bir örnekten diğerine geri yüklerken, her iki örnek de aynı abonelikte ve bölgede olmalıdır. Bölgeler arası ve çapraz abonelik geri yüklemesi şu anda desteklenmiyor.
- Tüm yönetilen bir örnek için bir noktadan noktaya geri yükleme mümkün değildir. Bu makalede, yalnızca mümkün olan özellikler açıklanmaktadır: yönetilen bir örnekte barındırılan bir veritabanının zaman içindeki nokta geri yüklemesi.

> [!WARNING]
> Yönetilen örneğinizin depolama boyutunu unutmayın. Geri yüklenecek verilerin boyutuna bağlı olarak, örnek depolamanın dışında kalabilirsiniz. Geri yüklenen veriler için yeterli alan yoksa farklı bir yaklaşım kullanın.

Aşağıdaki tabloda, yönetilen örnekler için zaman içinde geri yükleme senaryoları gösterilmektedir:

|           |Mevcut VERITABANıNı aynı yönetilen örneğe geri yükle| Varolan VERITABANıNı başka bir yönetilen örneğe geri yükleme|Bırakılan VERITABANıNı aynı yönetilen örneğe geri yükle|Bırakılan VERITABANıNı başka bir yönetilen örneğe geri yükleme|
|:----------|:----------|:----------|:----------|:----------|
|**Azure portal**| Yes|Hayır |Yes|Hayır|
|**Azure CLI**|Yes |Yes |Hayır|Hayır|
|**PowerShell**| Yes|Yes |Yes|Yes|

## <a name="restore-an-existing-database"></a>Var olan bir veritabanını geri yükleme

Azure portal, PowerShell veya Azure CLı kullanarak mevcut bir veritabanını aynı örneğe geri yükleyin. Bir veritabanını başka bir örneğe geri yüklemek için, hedef yönetilen örnek ve kaynak grubunun özelliklerini belirleyebilmeniz için PowerShell veya Azure CLı kullanın. Bu parametreleri belirtmezseniz, veritabanı varsayılan olarak mevcut örneğe geri yüklenir. Azure portal Şu anda başka bir örneğe geri yüklemeyi desteklemiyor.

# <a name="portal"></a>[Portal](#tab/azure-portal)

1. [Azure Portal](https://portal.azure.com) oturum açın. 
2. Yönetilen örneğinize gidin ve geri yüklemek istediğiniz veritabanını seçin.
3. Veritabanı sayfasında **geri yükle** ' yi seçin:

    ![Azure portal kullanarak bir veritabanını geri yükleme](media/sql-database-managed-instance-point-in-time-restore/restore-database-to-mi.png)

4. **Geri yükle** sayfasında, veritabanını geri yüklemek istediğiniz tarih ve saat için noktayı seçin.
5. Veritabanınızı geri yüklemek için **Onayla** ' yı seçin. Bu eylem, yeni bir veritabanı oluşturan ve belirli bir zaman noktasındaki özgün veritabanındaki verilerle dolduran geri yükleme işlemini başlatır. Kurtarma işlemi hakkında daha fazla bilgi için bkz. [Kurtarma süresi](sql-database-recovery-using-backups.md#recovery-time).

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Zaten yüklü Azure PowerShell yoksa, bkz. [Azure PowerShell modülünü yükleme](https://docs.microsoft.com/powershell/azure/install-az-ps).

Veritabanını PowerShell kullanarak geri yüklemek için aşağıdaki komutta parametre değerlerini belirtin. Ardından şu komutu çalıştırın:

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

Veritabanını başka bir yönetilen örneğe geri yüklemek için, hedef kaynak grubu ve hedef yönetilen örnek adlarını da belirtin:  

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

Ayrıntılar için bkz. [restore-Azsqlınstancedatabase](https://docs.microsoft.com/powershell/module/az.sql/restore-azsqlinstancedatabase).

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Azure CLı yüklü değilse, bkz. [Azure CLI 'Yı yükleme](/cli/azure/install-azure-cli?view=azure-cli-latest).

Azure CLı kullanarak veritabanını geri yüklemek için aşağıdaki komutta parametre değerlerini belirtin. Ardından şu komutu çalıştırın:

```azurecli-interactive
az sql midb restore -g mygroupname --mi myinstancename |
-n mymanageddbname --dest-name targetmidbname --time "2018-05-20T05:34:22"
```

Veritabanını başka bir yönetilen örneğe geri yüklemek için, hedef kaynak grubu ve yönetilen örnek adlarını da belirtin:  

```azurecli-interactive
az sql midb restore -g mygroupname --mi myinstancename -n mymanageddbname |
       --dest-name targetmidbname --time "2018-05-20T05:34:22" |
       --dest-resource-group mytargetinstancegroupname |
       --dest-mi mytargetinstancename
```

Kullanılabilir parametrelerin ayrıntılı bir açıklaması için bkz. [yönetilen bir örneğe bir veritabanını geri yüklemek Için CLI belgeleri](https://docs.microsoft.com/cli/azure/sql/midb?view=azure-cli-latest#az-sql-midb-restore).

---

## <a name="restore-a-deleted-database"></a>Silinen veritabanını geri yükleme

Silinen bir veritabanının geri yüklenmesi, PowerShell veya Azure portal kullanılarak yapılabilir. Silinen bir veritabanını aynı örneğe geri yüklemek için Azure portal ya da PowerShell kullanın. Silinen bir veritabanını başka bir örneğe geri yüklemek için PowerShell 'i kullanın. 

### <a name="portal"></a>Portal 


Azure portal kullanarak yönetilen bir veritabanını kurtarmak için, yönetilen örneğe genel bakış sayfasını açın ve **silinen veritabanları**' nı seçin. Geri yüklemek istediğiniz silinmiş bir veritabanını seçin ve yedeklemeden geri yüklenen verilerle oluşturulacak yeni veritabanının adını yazın.

  ![Silinen Azure SQL örneği veritabanını geri yükleme ekran görüntüsü](./media/sql-database-recovery-using-backups/restore-deleted-sql-managed-instance-annotated.png)

### <a name="powershell"></a>PowerShell

Bir veritabanını aynı örneğe geri yüklemek için, parametre değerlerini güncelleştirin ve ardından aşağıdaki PowerShell komutunu çalıştırın: 

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

Veritabanını başka bir yönetilen örneğe geri yüklemek için, hedef kaynak grubu ve hedef yönetilen örnek adlarını da belirtin:

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

## <a name="overwrite-an-existing-database"></a>Var olan veritabanının üzerine yaz

Var olan bir veritabanının üzerine yazmak için şunları yapmanız gerekir:

1. Üzerine yazmak istediğiniz var olan veritabanını bırakın.
2. Zaman içinde geri yüklenen veritabanını, bıraktığınız veritabanının adıyla yeniden adlandırın.

### <a name="drop-the-original-database"></a>Özgün veritabanını bırak

Azure portal, PowerShell veya Azure CLı kullanarak veritabanını bırakabilirsiniz.

Ayrıca, yönetilen örneğe doğrudan bağlanarak SQL Server Management Studio (SSMS) ve ardından aşağıdaki Transact-SQL (T-SQL) komutunu çalıştırarak veritabanını da bırakabilirsiniz:

```sql
DROP DATABASE WorldWideImporters;
```

Yönetilen örnekteki veritabanınıza bağlanmak için aşağıdaki yöntemlerden birini kullanın:

- [Bir Azure sanal makinesi aracılığıyla SSMS/Azure Data Studio](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-configure-vm)
- [Noktadan siteye](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-configure-p2s)
- [Genel uç nokta](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-public-endpoint-configure)

# <a name="portal"></a>[Portal](#tab/azure-portal)

Azure portal, yönetilen örnekten veritabanını seçin ve **Sil**' i seçin.

   ![Azure portal kullanarak bir veritabanını silme](media/sql-database-managed-instance-point-in-time-restore/delete-database-from-mi.png)

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Yönetilen bir örnekten var olan bir veritabanını bırakmak için aşağıdaki PowerShell komutunu kullanın:

```powershell
$resourceGroupName = "<Resource group name>"
$managedInstanceName = "<Managed instance name>"
$databaseName = "<Source database>"

Remove-AzSqlInstanceDatabase -Name $databaseName -InstanceName $managedInstanceName -ResourceGroupName $resourceGroupName
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Yönetilen bir örnekten var olan bir veritabanını bırakmak için aşağıdaki Azure CLı komutunu kullanın:

```azurecli-interactive
az sql midb delete -g mygroupname --mi myinstancename -n mymanageddbname
```

---

### <a name="alter-the-new-database-name-to-match-the-original-database-name"></a>Yeni veritabanı adını özgün veritabanı adıyla eşleşecek şekilde değiştirin

Doğrudan yönetilen örneğe bağlanın ve SQL Server Management Studio başlatın. Ardından, aşağıdaki Transact-SQL (T-SQL) sorgusunu çalıştırın. Sorgu, geri yüklenen veritabanının adını üzerine yazmak istediğiniz bırakılmış veritabanının adını değiştirecek.

```sql
ALTER DATABASE WorldWideImportersPITR MODIFY NAME = WorldWideImporters;
```

Yönetilen örnekteki veritabanınıza bağlanmak için aşağıdaki yöntemlerden birini kullanın:

- [Azure sanal makinesi](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-configure-vm)
- [Noktadan siteye](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-configure-p2s)
- [Genel uç nokta](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-public-endpoint-configure)

## <a name="next-steps"></a>Sonraki adımlar

[Otomatikleştirilmiş yedeklemeler](sql-database-automated-backups.md)hakkında bilgi edinin.
