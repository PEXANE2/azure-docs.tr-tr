---
title: SQL veritabanı yönetilen örneği-zaman içinde geri yükleme | Microsoft Docs
description: SQL yönetilen örneğindeki bir veritabanını zaman içinde önceki bir noktaya geri yükleme.
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
ms.openlocfilehash: 67f13d16dcf6bbe4fa13fe3a6e78d3e4d61e1999
ms.sourcegitcommit: 65131f6188a02efe1704d92f0fd473b21c760d08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70862141"
---
# <a name="restore-a-sql-managed-instance-database-to-a-previous-point-in-time"></a>SQL yönetilen örnek veritabanını zaman içinde önceki bir noktaya geri yükleme

Belirli bir noktaya geri yükleme (ıNR), geçmişteki bir zamanda başka bir veritabanının kopyası olarak bir veritabanı oluşturmanıza olanak sağlar. Bu makalede, yönetilen bir örnekteki veritabanının bir zaman içinde geri yükleme işlemi nasıl gerçekleştirileceği açıklanır.

Zaman içinde geri yükleme, hatalara neden olan olaylar, yanlış yüklenmiş veriler, önemli verilerin silinmesi ve yalnızca test veya denetim amaçlarıyla diğer sorunlar gibi kurtarma senaryolarında kullanılabilir. Veritabanı ayarlarınıza bağlı olarak, yedekleme dosyaları 7 ila 35 gün arasında bir süre tutulur.

Zaman içinde bir noktaya geri yükleme şu şekilde kullanılabilir:

- Mevcut bir veritabanından bir veritabanını geri yükleyin.
- Silinen bir veritabanından bir veritabanını geri yükleyin.

Ayrıca, yönetilen bir örnek ile ilgili bir noktadan noktaya geri yükleme şu şekilde kullanılabilir: 

- Bir veritabanını aynı yönetilen örneğe geri yükleyin.
- Veritabanını başka bir yönetilen örneğe geri yükleyin.


> [!NOTE]
> Tüm yönetilen bir örnek için bir noktadan noktaya geri yükleme mümkün değildir. Mümkün olan ve bu makalede açıklanan, yönetilen bir örnekte barındırılan bir veritabanının zaman içindeki bir noktaya geri yüklenmesi olasıdır.


## <a name="limitations"></a>Sınırlamalar

Başka bir yönetilen örneğe geri yükleme yaparken her iki örnek de aynı abonelikte ve bölgede olmalıdır. Bölgeler arası ve çapraz abonelik geri yüklemeleri şu anda desteklenmiyor.

> [!WARNING]
> Verilerin geri yükleme boyutuna bağlı olarak, yönetilen örneğinizin depolama boyutuyla ilgili dikkatli olun; örnek depolama tükenmiş olabilirsiniz. Geri yüklenen veriler için yeterli alan yoksa alternatif bir yaklaşım kullanın.

Aşağıdaki tabloda, yönetilen örnek için bir noktadan noktaya kurtarma senaryoları gösterilmektedir:

|           |Mevcut DB 'yi geri yükle| Mevcut DB 'yi geri yükle|Bırakılan DB 'yi geri yükle| Bırakılan DB 'yi geri yükle|
|:----------|:----------|:----------|:----------|:----------|
|Hedef| Aynı mı?|Başka bir mı |Aynı mı?|Başka bir mı |
|Azure portal| Evet|Hayır |Hayır|Hayır|
|Azure CLI|Evet |Evet |Hayır|Hayır|
|PowerShell| Evet|Evet |Evet|Evet|


## <a name="restore-existing-database"></a>Mevcut veritabanını geri yükle

Mevcut bir veritabanını Azure portal, PowerShell veya Azure CLı kullanarak aynı örneğe geri yükleyin. Hedef yönetilen örnek ve kaynak grubu özelliklerini belirterek PowerShell veya Azure CLı kullanarak bir veritabanını başka bir örneğe geri yükleyin. Bu parametreler belirtilmemişse, varsayılan olarak veritabanı mevcut örneğe geri yüklenir. Başka bir örneğe geri yükleme, şu anda Azure portal aracılığıyla desteklenmiyor. 

# <a name="portaltabazure-portal"></a>[Portal](#tab/azure-portal)

1. [Azure portal](https://portal.azure.com) oturum açın. 
1. Yönetilen örneğinize gidin ve geri yüklemek istediğiniz veritabanını seçin. 
1. Veritabanı sayfasında **geri yükle** ' yi seçin. 

    ![Mevcut veritabanını geri yükle](media/sql-database-managed-instance-point-in-time-restore/restore-database-to-mi.png)

1. **Geri yükle** sayfasında, veritabanının geri yüklenmesini istediğiniz tarih ve saat için noktayı seçin.
1. Veritabanınızı geri yüklemek için **Onayla** ' yı seçin. Bu, yeni bir veritabanı oluşturan ve istenen noktada özgün veritabanındaki verilerle doldurulan geri yükleme işlemini başlatır. Kurtarma işlemi hakkında daha fazla bilgi için bkz. [Kurtarma süresi](sql-database-recovery-using-backups.md#recovery-time). 

1. Yönetilen örneği bul
1. Geri yüklemek istediğiniz veritabanını seçin
1. Veritabanı ekranında geri yükleme eylemi ' ne tıklayın.
1. Geri yükleme ekranında, veritabanının geri yüklendiği geçmişin tarih ve saat ' i seçin
1. Onayladıktan sonra geri yükleme işlemi başlar ve veritabanının boyutuna bağlı olarak, yeni veritabanı oluşturulur ve istenen zamanda özgün veritabanındaki verilerle doldurulur. Geri yükleme işleminin süresi boyunca, yedeklemeleri kullanarak kurtarmayı denetleyin.


# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

Zaten yüklü Azure PowerShell yoksa, bkz. [Azure PowerShell modülünü yükleme](https://docs.microsoft.com/powershell/azure/install-az-ps).

Veritabanını PowerShell kullanarak geri yüklemek için, parametreleri değerlerinizle güncelleştirin ve şu komutu çalıştırın:

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

Veritabanını başka bir yönetilen örneğe geri yüklemek için hedef kaynak grubu adını ve hedef yönetilen örnek adını ayarlayın.  

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


# <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)

Azure CLı yüklü değilse, bkz. [Azure CLI 'Yı yükleme](/cli/azure/install-azure-cli?view=azure-cli-latest).

Azure CLı kullanarak veritabanını geri yüklemek için, parametreleri değerlerinizle güncelleştirin ve şu komutu çalıştırın:


```azurecli-interactive
az sql midb restore -g mygroupname --mi myinstancename |
-n mymanageddbname --dest-name targetmidbname --time "2018-05-20T05:34:22"
```


Veritabanını başka bir yönetilen örneğe geri yüklemek için hedef kaynak grubu adını ve hedef yönetilen örnek adını ayarlayın.  

```azurecli-interactive
az sql midb restore -g mygroupname --mi myinstancename -n mymanageddbname |
       --dest-name targetmidbname --time "2018-05-20T05:34:22" |
       --dest-resource-group mytargetinstancegroupname |
       --dest-mi mytargetinstancename
```

Kullanılabilir parametrelerin ayrıntılı açıklaması için bkz. [yönetilen örnek CLI](https://docs.microsoft.com/cli/azure/sql/midb?view=azure-cli-latest#az-sql-midb-restore). 

---

## <a name="restore-a-deleted-database"></a>Silinen veritabanını geri yükleme 
 
Silinen bir veritabanının geri yüklenmesi yalnızca PowerShell ile yapılabilir. Veritabanı aynı örneğe veya başka bir örneğe geri yüklenebilir. 

Silinen bir veritabanını PowerShell kullanarak geri yüklemek için, parametreleri değerlerinizle güncelleştirin ve şu komutu çalıştırın:

```powershell-interactive
$subscriptionId = "<Subscription ID>"
Get-AzSubscription -SubscriptionId $subscriptionId
Select-AzSubscription -SubscriptionId $subscriptionId

$resourceGroupName = "<Resource group name>"
$managedInstanceName = "<Managed instance name>"
$deletedDatabaseName = "<Source database name>"

$deleted_db = Get-AzSqlDeletedInstanceDatabaseBackup -ResourceGroupName $resourceGroupName `
            -InstanceName $managedInstanceName -DatabaseName $deletedDatabaseName 

$pointInTime = "2018-06-27T08:51:39.3882806Z"
$properties = New-Object System.Object
$properties | Add-Member -type NoteProperty -name CreateMode -Value "PointInTimeRestore"
$properties | Add-Member -type NoteProperty -name RestorePointInTime -Value $pointInTime
$properties | Add-Member -type NoteProperty -name RestorableDroppedDatabaseId -Value $deleted_db.Id
```

Silinen veritabanını başka bir örneğe geri yüklemek için, kaynak grubu adını ve yönetilen örnek adını değiştirin.

Konum parametresi, kaynak grubu ve yönetilen örnek konumuyla eşleşmelidir.

```powershell-interactive
$resourceGroupName = "<Second resource group name>"
$managedInstanceName = "<Second managed instance name>"

$location = "West Europe"

$restoredDBName = "WorldWideImportersPITR"
$resource_id = "subscriptions/$subscriptionId/resourceGroups/$resourceGroupName/providers/Microsoft.Sql/managedInstances/$managedInstanceName/databases/$restoredDBName"

New-AzResource -Location $location -Properties $properties `
        -ResourceId $resource_id -ApiVersion "2017-03-01-preview" -Force
```

## <a name="overwrite-existing-database"></a>Var olan veritabanının üzerine yaz 
 
Var olan bir veritabanının üzerine yazmak için ayrıca şunları yapmanız gerekir:

1. Üzerine yazmak istediğiniz var olan veritabanını bırakın.
1. Geri yüklenen zaman aşımı veritabanını, bırakılan veritabanının adına yeniden adlandırın. 


### <a name="drop-original-database"></a>Özgün veritabanını bırak 
 
Veritabanını bırakma Azure portal, PowerShell veya Azure CLı ile yapılabilir. 

Ayrıca, yönetilen örneğe doğrudan bağlanarak SQL Server Management Studio (SSMS) başlatarak ve aşağıdaki Transact-SQL (T-SQL) komutunu çalıştırarak veritabanını bırakabilirsiniz.

```sql
DROP DATABASE WorldWideImporters;
```

Yönetilen örnek veritabanınıza bağlanmak için aşağıdaki yöntemlerden birini kullanın: 

- [SQL sanal makinesi](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-configure-vm)
- [Noktadan siteye](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-configure-p2s)
- [Genel bir uç nokta](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-public-endpoint-configure)

# <a name="portaltabazure-portal"></a>[Portal](#tab/azure-portal)

Azure portal, yönetilen örnekten veritabanını seçin ve **Sil**' i seçin.

   ![Mevcut veritabanını geri yükle](media/sql-database-managed-instance-point-in-time-restore/delete-database-from-mi.png)

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

Yönetilen bir örnekten var olan bir veritabanını bırakmak için aşağıdaki PowerShell komutunu kullanın: 

```powershell
$resourceGroupName = "<Resource group name>"
$managedInstanceName = "<Managed instance name>"
$databaseName = "<Source database>"

Remove-AzSqlInstanceDatabase -Name $databaseName -InstanceName $managedInstanceName -ResourceGroupName $resourceGroupName
```

# <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)

Yönetilen bir örnekten var olan bir veritabanını bırakmak için aşağıdaki Azure CLı komutunu kullanın: 

```azurecli-interactive
az sql midb delete -g mygroupname --mi myinstancename -n mymanageddbname
```

---


### <a name="alter-new-database-name-to-original"></a>Yeni veritabanı adını özgün olarak DEĞIŞTIR

Doğrudan yönetilen örneğe bağlanın, SQL Server Management Studio başlatın ve ardından geri yüklenen veritabanının adını üzerine yazmak istediğiniz bırakılmış veritabanının adını değiştirmek için aşağıdaki Transact-SQL (T-SQL) sorgusunu yürütün. 


```sql
ALTER WorldWideImportersPITR MODIFY NAME = WorldWideImporters;
```


Yönetilen örnek veritabanınıza bağlanmak için aşağıdaki yöntemlerden birini kullanın: 

- [SQL sanal makinesi](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-configure-vm)
- [Noktadan siteye](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-configure-p2s)
- [Genel bir uç nokta](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-public-endpoint-configure)

## <a name="next-steps"></a>Sonraki adımlar

[Uzun süreli bekletme](sql-database-long-term-retention.md) ve [otomatik yedeklemeler](sql-database-automated-backups.md)hakkında bilgi edinin. 
