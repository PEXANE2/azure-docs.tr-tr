---
title: Tek/havuzlu veritabanları dosya alanı yönetimi
description: Bu sayfa, Azure SQL Veritabanı'nda tek ve birhavuzlu veritabanlarıyla dosya alanının nasıl yönetilenin açıklanır ve tek bir veritabanını veya birleştirilmiş veritabanını küçültmeniz gerekip gerekmediğinizi ve veritabanı küçültme işlemini nasıl gerçekleştireceklerini belirlemek için kod örnekleri sağlar.
services: sql-database
ms.service: sql-database
ms.subservice: operations
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: oslake
ms.author: moslake
ms.reviewer: jrasnick, carlrab
ms.date: 03/12/2019
ms.openlocfilehash: 007bbffbd7c4fcad339f88eb78991eb39fb829e6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74420973"
---
# <a name="manage-file-space-for-single-and-pooled-databases-in-azure-sql-database"></a>Azure SQL Veritabanı'nda tek ve havuza toplanan veritabanları için dosya alanını yönetme

Bu makalede, Azure SQL Veritabanı'nda tek ve havuzlu veritabanları için farklı depolama alanı türleri açıklanır ve veritabanları ve esnek havuzlar için ayrılan dosya alanının açıkça yönetilmesi gerektiğinde atılabilir adımlar açıklanır.

> [!NOTE]
> Bu makale, Azure SQL Veritabanı'nda yönetilen örnek dağıtım seçeneği için geçerli değildir.

## <a name="overview"></a>Genel Bakış

Azure SQL Veritabanı'nda tek ve birhavuzlu veritabanları yla, veritabanları için temel veri dosyalarının tahsisinin kullanılan veri sayfası miktarından daha büyük olabileceği iş yükü desenleri vardır. Bu durum kullanılan alanın artması ve sonrasında verilerin silinmesi durumunda ortaya çıkabilir. Bunun nedeni, ayrılan dosya alanının veriler silindiğinde otomatik olarak geri alınmamasıdır.

Aşağıdaki senaryolarda dosya alanı kullanımının izlenmesi ve veri dosyalarının küçültülmesi gerekli olabilir:

- Bir elastik havuzun veritabanları için ayrılan dosya alanının maksimum havuz boyutuna erişmesi durumunda veri artışına izin verilmesi.
- Tek bir veritabanının veya elastik havuzun maksimum boyutunun küçülmesine izin verilmesi.
- Tek bir veritabanının veya elastik havuzun daha düşük maksimum boyuta sahip farklı bir hizmet katmanına veya performans katmanına geçmesine izin verilmesi.

### <a name="monitoring-file-space-usage"></a>Dosya alanı kullanımını izleme

Azure portalında görüntülenen depolama alanı ölçümlerinin çoğu ve aşağıdaki API'ler yalnızca kullanılan veri sayfalarının boyutunu ölçer:

- PowerShell [get-ölçümleri](https://docs.microsoft.com/powershell/module/az.monitor/get-azmetric) de dahil olmak üzere Azure Kaynak Yöneticisi tabanlı ölçüm API'leri
- T-SQL: [sys.dm_db_resource_stats](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database)

Ancak, aşağıdaki API'ler veritabanları ve elastik havuzlar için ayrılan alanın boyutunu da ölçer:

- T-SQL: [sys.resource_stats](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database)
- T-SQL: [sys.elastic_pool_resource_stats](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-elastic-pool-resource-stats-azure-sql-database)

### <a name="shrinking-data-files"></a>Veri dosyalarını küçültme

SQL Veritabanı hizmeti, veritabanı performansının olası etkisi nedeniyle kullanılmayan ayrılmış alanı geri almak için veri dosyalarını otomatik olarak küçültmez.  Ancak, [müşteriler, kullanılmayan ayrılan alanı geri almada](#reclaim-unused-allocated-space)açıklanan adımları izleyerek, kendi seçtikleri bir zamanda self servis yoluyla veri dosyalarını küçültebilir.

> [!NOTE]
> Veri dosyalarının aksine, SQL Veritabanı hizmeti günlük dosyalarını otomatik olarak küçültür, çünkü bu işlem veritabanı performansını etkilemez.

## <a name="understanding-types-of-storage-space-for-a-database"></a>Veritabanı için depolama alanı türlerini anlama

Aşağıdaki depolama alanı miktarlarını anlamak, bir veritabanının dosya alanını yönetmek için önemlidir.

|Veritabanı miktarı|Tanım|Yorumlar|
|---|---|---|
|**Kullanılan veri alanı**|Veritabanı verilerini 8 KB sayfada depolamak için kullanılan alan miktarı.|Genellikle, kullanılan alan ekler (siler) artar (azalır). Bazı durumlarda, kullanılan alan, işlemle ilgili verilerin miktarına ve desenine ve herhangi bir parçalanmaya bağlı olarak eklerde veya silmelerde değişmez. Örneğin, her veri sayfasından bir satır ı silerken, kullanılan alanı mutlaka azaltmaz.|
|**Ayrılan veri alanı**|Veritabanı verilerini depolamak için kullanılabilir hale biçimlendirilmiş dosya alanı miktarı.|Ayrılan alan miktarı otomatik olarak büyür, ancak sildikten sonra asla azalmaz. Bu davranış, uzayın yeniden biçimlendirilmesi gerekmediğinden, gelecekteki eklerin daha hızlı olmasını sağlar.|
|**Veri alanı ayrılmış ancak kullanılmayan**|Ayrılan veri alanı miktarı ile kullanılan veri alanı arasındaki fark.|Bu miktar, veritabanı veri dosyalarını küçülterek geri alınabilecek maksimum boş alan miktarını temsil eder.|
|**Veri max boyutu**|Veritabanı verilerini depolamak için kullanılabilecek maksimum alan miktarı.|Ayrılan veri alanı miktarı veri max boyutunun ötesine geçemez.|

Aşağıdaki diyagram, bir veritabanı için farklı depolama alanı türleri arasındaki ilişkiyi göstermektedir.

![depolama alanı türleri ve ilişkileri](./media/sql-database-file-space-management/storage-types.png)

## <a name="query-a-single-database-for-storage-space-information"></a>Depolama alanı bilgileri için tek bir veritabanını sorgula

Aşağıdaki sorgular, tek bir veritabanı için depolama alanı miktarlarını belirlemek için kullanılabilir.  

### <a name="database-data-space-used"></a>Kullanılan veritabanı veri alanı

Kullanılan veritabanı veri alanı miktarını döndürmek için aşağıdaki sorguyu değiştirin.  Sorgu sonucunun birimleri MB'dedir.

```sql
-- Connect to master
-- Database data space used in MB
SELECT TOP 1 storage_in_megabytes AS DatabaseDataSpaceUsedInMB
FROM sys.resource_stats
WHERE database_name = 'db1'
ORDER BY end_time DESC
```

### <a name="database-data-space-allocated-and-unused-allocated-space"></a>Veritabanı veri alanı ayrılmış ve kullanılmayan ayrılmış alan

Ayrılan veritabanı veri alanı miktarını ve ayrılan kullanılmayan alan miktarını döndürmek için aşağıdaki sorguyu kullanın.  Sorgu sonucunun birimleri MB'dedir.

```sql
-- Connect to database
-- Database data space allocated in MB and database data space allocated unused in MB
SELECT SUM(size/128.0) AS DatabaseDataSpaceAllocatedInMB,
SUM(size/128.0 - CAST(FILEPROPERTY(name, 'SpaceUsed') AS int)/128.0) AS DatabaseDataSpaceAllocatedUnusedInMB
FROM sys.database_files
GROUP BY type_desc
HAVING type_desc = 'ROWS'
```

### <a name="database-data-max-size"></a>Veritabanı veri max boyutu

Veritabanı veri max boyutunu döndürmek için aşağıdaki sorguyu değiştirin.  Sorgu sonucunun birimleri bayt olarak dır.

```sql
-- Connect to database
-- Database data max size in bytes
SELECT DATABASEPROPERTYEX('db1', 'MaxSizeInBytes') AS DatabaseDataMaxSizeInBytes
```

## <a name="understanding-types-of-storage-space-for-an-elastic-pool"></a>Elastik bir havuz için depolama alanı türlerini anlama

Aşağıdaki depolama alanı miktarlarını anlamak, elastik bir havuzun dosya alanını yönetmek için önemlidir.

|Elastik havuz miktarı|Tanım|Yorumlar|
|---|---|---|
|**Kullanılan veri alanı**|Elastik havuzdaki tüm veritabanları tarafından kullanılan veri alanının toplamı.||
|**Ayrılan veri alanı**|Elastik havuzdaki tüm veritabanları tarafından ayrılan veri alanının toplamı.||
|**Veri alanı ayrılmış ancak kullanılmayan**|Ayrılan veri alanı miktarı ile elastik havuzdaki tüm veritabanları tarafından kullanılan veri alanı arasındaki fark.|Bu miktar, veritabanı veri dosyaları küçülterek geri alınabilir elastik havuz için ayrılan maksimum alan miktarını temsil eder.|
|**Veri max boyutu**|Tüm veritabanları için elastik havuz tarafından kullanılabilecek maksimum veri alanı miktarı.|Elastik havuz için ayrılan alan elastik havuz maksimum boyutunu aşmamalıdır.  Bu durum oluşursa, kullanılmayan ayrılan alan veritabanı veri dosyalarını küçülterek geri alınabilir.|

## <a name="query-an-elastic-pool-for-storage-space-information"></a>Depolama alanı bilgileri için elastik bir havuzu sorgula

Aşağıdaki sorgular, elastik bir havuz için depolama alanı miktarlarını belirlemek için kullanılabilir.  

### <a name="elastic-pool-data-space-used"></a>Kullanılan elastik havuz veri alanı

Kullanılan elastik havuz veri alanı miktarını döndürmek için aşağıdaki sorguyu değiştirin.  Sorgu sonucunun birimleri MB'dedir.

```sql
-- Connect to master
-- Elastic pool data space used in MB  
SELECT TOP 1 avg_storage_percent / 100.0 * elastic_pool_storage_limit_mb AS ElasticPoolDataSpaceUsedInMB
FROM sys.elastic_pool_resource_stats
WHERE elastic_pool_name = 'ep1'
ORDER BY end_time DESC
```

### <a name="elastic-pool-data-space-allocated-and-unused-allocated-space"></a>Elastik havuz veri alanı ayrılmış ve kullanılmayan ayrılmış alan

Elastik bir havuzda her veritabanı için ayrılan ve kullanılmayan ayrılmış alanı listeleyen bir tablo döndürmek için aşağıdaki örnekleri değiştirin. Tablo, bu veritabanlarından veritabanlarını en fazla kullanılmayan ayrılmış alana en az miktarda kullanılmayan alana sipariş eder.  Sorgu sonucunun birimleri MB'dedir.  

Havuzdaki her veritabanı için ayrılan alanı belirlemek için sorgu sonuçları, elastik havuz için ayrılan toplam alanı belirlemek için birlikte eklenebilir. Ayrılan elastik havuz alanı elastik havuz maksimum boyutunu aşmamalıdır.  

> [!IMPORTANT]
> PowerShell Azure Kaynak Yöneticisi (RM) modülü hala Azure SQL Veritabanı tarafından desteklenir, ancak gelecekteki tüm geliştirmeler Az.Sql modülü içindir. AzureRM modülü en az Aralık 2020'ye kadar hata düzeltmeleri almaya devam edecektir.  Az modülündeki ve AzureRm modüllerinde bulunan komutların bağımsız değişkenleri önemli ölçüde aynıdır. Uyumlulukları hakkında daha fazla bilgi için [yeni Azure PowerShell Az modüllerini tanıtın.](/powershell/azure/new-azureps-module-az)

PowerShell komut dosyası, SQL Server PowerShell modülü gerektirir – yüklemek için [PowerShell modüllerini karşıdan yükleyin.](https://docs.microsoft.com/sql/powershell/download-sql-server-ps-module)

```powershell
$resourceGroupName = "<resourceGroupName>"
$serverName = "<serverName>"
$poolName = "<poolName>"
$userName = "<userName>"
$password = "<password>"

# get list of databases in elastic pool
$databasesInPool = Get-AzSqlElasticPoolDatabase -ResourceGroupName $resourceGroupName `
    -ServerName $serverName -ElasticPoolName $poolName
$databaseStorageMetrics = @()

# for each database in the elastic pool, get space allocated in MB and space allocated unused in MB
foreach ($database in $databasesInPool) {
    $sqlCommand = "SELECT DB_NAME() as DatabaseName, `
    SUM(size/128.0) AS DatabaseDataSpaceAllocatedInMB, `
    SUM(size/128.0 - CAST(FILEPROPERTY(name, 'SpaceUsed') AS int)/128.0) AS DatabaseDataSpaceAllocatedUnusedInMB `
    FROM sys.database_files `
    GROUP BY type_desc `
    HAVING type_desc = 'ROWS'"
    $serverFqdn = "tcp:" + $serverName + ".database.windows.net,1433"
    $databaseStorageMetrics = $databaseStorageMetrics + 
        (Invoke-Sqlcmd -ServerInstance $serverFqdn -Database $database.DatabaseName `
            -Username $userName -Password $password -Query $sqlCommand)
}

# display databases in descending order of space allocated unused
Write-Output "`n" "ElasticPoolName: $poolName"
Write-Output $databaseStorageMetrics | Sort -Property DatabaseDataSpaceAllocatedUnusedInMB -Descending | Format-Table
```

Aşağıdaki ekran görüntüsü komut dosyasının çıktısı bir örnektir:

![elastik havuz ayrılmış alan ve kullanılmayan ayrılmış alan örneği](./media/sql-database-file-space-management/elastic-pool-allocated-unused.png)

### <a name="elastic-pool-data-max-size"></a>Elastik havuz veri max boyutu

Elastik havuz veri max boyutunu döndürmek için aşağıdaki T-SQL sorgusunu değiştirin.  Sorgu sonucunun birimleri MB'dedir.

```sql
-- Connect to master
-- Elastic pools max size in MB
SELECT TOP 1 elastic_pool_storage_limit_mb AS ElasticPoolMaxSizeInMB
FROM sys.elastic_pool_resource_stats
WHERE elastic_pool_name = 'ep1'
ORDER BY end_time DESC
```

## <a name="reclaim-unused-allocated-space"></a>Kullanılmayan ayrılan alanı geri alma

> [!NOTE]
> Bu komut çalışırken veritabanı performansını etkileyebilir ve mümkünse düşük kullanım dönemlerinde çalıştırılmalıdır.

### <a name="dbcc-shrink"></a>DBCC küçültme

Kullanılmayan ayrılan alanı geri almak için veritabanları tanımlandıktan sonra, her veritabanı için veri dosyalarını küçültmek için aşağıdaki komuttaki veritabanının adını değiştirin.

```sql
-- Shrink database data space allocated.
DBCC SHRINKDATABASE (N'db1')
```

Bu komut çalışırken veritabanı performansını etkileyebilir ve mümkünse düşük kullanım dönemlerinde çalıştırılmalıdır.  

Bu komut hakkında daha fazla bilgi için [SHRINKDATABASE'e](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-shrinkdatabase-transact-sql)bakın.

### <a name="auto-shrink"></a>Otomatik küçültme

Alternatif olarak, otomatik küçültme bir veritabanı için etkinleştirilebilir.  Otomatik küçültme, dosya yönetimi karmaşıklığını azaltır ve `SHRINKDATABASE` veritabanı `SHRINKFILE`performansına daha az etkilenir veya .  Otomatik küçültme, birçok veritabanları ile elastik havuzları yönetmek için özellikle yararlı olabilir.  Ancak, otomatik küçültme dosya alanı geri alma `SHRINKDATABASE` daha `SHRINKFILE`az etkili olabilir ve .
Otomatik küçültmeyi etkinleştirmek için aşağıdaki komutta veritabanının adını değiştirin.

```sql
-- Enable auto-shrink for the database.
ALTER DATABASE [db1] SET AUTO_SHRINK ON
```

Bu komut hakkında daha fazla bilgi için [DATABASE SET](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-set-options?view=azuresqldb-current) seçeneklerine bakın.

### <a name="rebuild-indexes"></a>Dizinleri yeniden oluşturma

Veritabanı veri dosyaları küçültüldükten sonra dizinler parçalanabilir ve performans optimizasyonu etkinliğini kaybedebilir. Performans bozulması oluşursa, veritabanı dizinlerini yeniden oluşturmayı düşünün. Parçalanma ve yeniden oluşturma dizinleri hakkında daha fazla bilgi [için](https://docs.microsoft.com/sql/relational-databases/indexes/reorganize-and-rebuild-indexes)bkz.

## <a name="next-steps"></a>Sonraki adımlar

- Veritabanı en büyük boyutları hakkında bilgi için bkz:
  - [Azure SQL Database vCore tabanlı satın alma modeli tek bir veritabanı için sınırlar](sql-database-vcore-resource-limits-single-databases.md)
  - [DTU tabanlı satın alma modelini kullanarak tek veritabanları için kaynak sınırları](sql-database-dtu-resource-limits-single-databases.md)
  - [Elastik havuzlar için Azure SQL Database vCore tabanlı satın alma modeli sınırları](sql-database-vcore-resource-limits-elastic-pools.md)
  - [DTU tabanlı satın alma modelini kullanan esnek havuzlar için kaynak sınırları](sql-database-dtu-resource-limits-elastic-pools.md)
- `SHRINKDATABASE` Komut hakkında daha fazla bilgi için [SHRINKDATABASE'e](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-shrinkdatabase-transact-sql)bakın.
- Parçalanma ve yeniden oluşturma dizinleri hakkında daha fazla bilgi [için](https://docs.microsoft.com/sql/relational-databases/indexes/reorganize-and-rebuild-indexes)bkz.
