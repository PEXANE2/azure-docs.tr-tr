---
title: Azure SQL veritabanı dosya alanı yönetimi
description: Bu sayfada, Azure SQL veritabanı 'nda tek ve havuza alınmış veritabanlarıyla dosya alanının nasıl yönetileceği açıklanır ve tek veya havuza alınmış bir veritabanını daraltmanız gerekip gerekmediğini ve bir veritabanı küçültme işlemini nasıl gerçekleştireceğiniz hakkında kod örnekleri sağlanır.
services: sql-database
ms.service: sql-database
ms.subservice: operations
ms.custom: sqldbrb=1
ms.devlang: ''
ms.topic: conceptual
author: oslake
ms.author: moslake
ms.reviewer: jrasnick, carlrab
ms.date: 03/12/2019
ms.openlocfilehash: ebaddbcacbc20097b2ec5606244650ea2916edfe
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84324547"
---
# <a name="manage-file-space-for-databases-in-azure-sql-database"></a>Azure SQL veritabanında veritabanları için dosya alanını yönetme
[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

Bu makalede, Azure SQL veritabanı 'ndaki veritabanları için farklı depolama alanı türleri ve ayrılan dosya alanının açıkça yönetilmesi gerektiğinde alınabilecek adımlar açıklanmaktadır.

> [!NOTE]
> Bu makale, Azure SQL yönetilen örneği için geçerlidir.

## <a name="overview"></a>Genel Bakış

Azure SQL veritabanı ile, veritabanları için temel alınan veri dosyalarının ayrılması, kullanılan veri sayfalarının miktarından daha büyük hale gelebileceğinden iş yükü desenleri vardır. Bu durum kullanılan alanın artması ve sonrasında verilerin silinmesi durumunda ortaya çıkabilir. Bunun nedeni, veri silindiğinde ayrılan dosya alanının otomatik olarak geri kazanılamadır.

Aşağıdaki senaryolarda dosya alanı kullanımının izlenmesi ve veri dosyalarının küçültülmesi gerekli olabilir:

- Bir elastik havuzun veritabanları için ayrılan dosya alanının maksimum havuz boyutuna erişmesi durumunda veri artışına izin verilmesi.
- Tek bir veritabanının veya elastik havuzun maksimum boyutunun küçülmesine izin verilmesi.
- Tek bir veritabanının veya elastik havuzun daha düşük maksimum boyuta sahip farklı bir hizmet katmanına veya performans katmanına geçmesine izin verilmesi.

### <a name="monitoring-file-space-usage"></a>Dosya alanı kullanımını izleme

Azure portal görüntülendiği en fazla depolama alanı ölçümü ve aşağıdaki API 'Ler yalnızca kullanılan veri sayfalarının boyutunu ölçer:

- PowerShell [Get-ölçümleri](https://docs.microsoft.com/powershell/module/az.monitor/get-azmetric) dahil Azure Resource Manager tabanlı ölçüm API 'leri
- T-SQL: [sys. dm_db_resource_stats](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database)

Ancak aşağıdaki API 'Ler Ayrıca veritabanları ve elastik havuzlar için ayrılan alan boyutunu ölçer:

- T-SQL: [sys. resource_stats](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database)
- T-SQL: [sys. elastic_pool_resource_stats](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-elastic-pool-resource-stats-azure-sql-database)

### <a name="shrinking-data-files"></a>Veri dosyalarını küçültme

Azure SQL veritabanı, veritabanı performansının olası etkisi nedeniyle kullanılmayan ayrılmış alanı geri kazanmak için veri dosyalarını otomatik olarak daraltamaz.  Ancak, müşteriler [kullanılmayan ayrılmış alanı geri kazanma](#reclaim-unused-allocated-space)bölümünde açıklanan adımları izleyerek, kendi seçme sırasında veri dosyalarını kendi seçerken daraltabilir.

> [!NOTE]
> Veri dosyalarından farklı olarak, Azure SQL veritabanı günlük dosyalarını otomatik olarak küçültür çünkü bu işlem veritabanı performansını etkilemez.

## <a name="understanding-types-of-storage-space-for-a-database"></a>Bir veritabanı için depolama alanı türlerini anlama

Aşağıdaki depolama alanı miktarlarını anlamak bir veritabanının dosya alanını yönetmek için önemlidir.

|Veritabanı miktarı|Tanım|Yorumlar|
|---|---|---|
|**Kullanılan veri alanı**|Veritabanı verilerini 8 KB 'lik sayfalarda depolamak için kullanılan alan miktarı.|Genellikle, kullanılan alanın eklemeleri (azaltır) ekler (siler). Bazı durumlarda, kullanılan alan, işleme ve parçalanmaya dahil edilen verilerin miktarına ve düzenine bağlı olarak ekleme veya silme üzerinde değişiklik yapmaz. Örneğin, her veri sayfasından bir satırı silmek, kullanılan alanı azaltmayabilir.|
|**Ayrılan veri alanı**|Veritabanı verilerini depolamak için kullanılabilir hale getirilen biçimlendirilen dosya alanı miktarı.|Ayrılan alan miktarı otomatik olarak büyür, ancak silmeleri hiçbir şekilde azalmıştır. Bu davranış, boşluk yeniden biçimlendirilmesi gerekmediğinden gelecekteki eklemelerin daha hızlı olmasını sağlar.|
|**Ayrılan ancak kullanılmayan veri alanı**|Ayrılan veri alanı miktarı ve kullanılan veri alanı arasındaki fark.|Bu miktar, veritabanı veri dosyalarını küçülterek geri kazanılabileceğini en fazla boş alan miktarını temsil eder.|
|**Veri boyutu üst sınırı**|Veritabanı verilerini depolamak için kullanılabilecek maksimum alan miktarı.|Ayrılan veri alanı miktarı, veri boyutu üst sınırının ötesinde büyütülemez.|

Aşağıdaki diyagramda bir veritabanı için farklı depolama alanı türleri arasındaki ilişki gösterilmektedir.

![depolama alanı türleri ve ilişkileri](./media/file-space-manage/storage-types.png)

## <a name="query-a-single-database-for-storage-space-information"></a>Depolama alanı bilgileri için tek bir veritabanını sorgulama

Aşağıdaki sorgular tek bir veritabanı için depolama alanı miktarlarını belirlemede kullanılabilir.  

### <a name="database-data-space-used"></a>Kullanılan veritabanı veri alanı

Kullanılan veritabanı veri alanı miktarını döndürmek için aşağıdaki sorguyu değiştirin.  Sorgu sonucunun birimleri MB 'tır.

```sql
-- Connect to master
-- Database data space used in MB
SELECT TOP 1 storage_in_megabytes AS DatabaseDataSpaceUsedInMB
FROM sys.resource_stats
WHERE database_name = 'db1'
ORDER BY end_time DESC
```

### <a name="database-data-space-allocated-and-unused-allocated-space"></a>Ayrılan ve kullanılmayan ayrılan alan veritabanı veri alanı

Ayrılan veritabanı veri alanı miktarını ve ayrılan kullanılmayan alanı miktarını döndürmek için aşağıdaki sorguyu kullanın.  Sorgu sonucunun birimleri MB 'tır.

```sql
-- Connect to database
-- Database data space allocated in MB and database data space allocated unused in MB
SELECT SUM(size/128.0) AS DatabaseDataSpaceAllocatedInMB,
SUM(size/128.0 - CAST(FILEPROPERTY(name, 'SpaceUsed') AS int)/128.0) AS DatabaseDataSpaceAllocatedUnusedInMB
FROM sys.database_files
GROUP BY type_desc
HAVING type_desc = 'ROWS'
```

### <a name="database-data-max-size"></a>Veritabanı verileri en büyük boyutu

Veritabanı verilerinin en büyük boyutunu döndürmek için aşağıdaki sorguyu değiştirin.  Sorgu sonucunun birimleri bayt cinsinden.

```sql
-- Connect to database
-- Database data max size in bytes
SELECT DATABASEPROPERTYEX('db1', 'MaxSizeInBytes') AS DatabaseDataMaxSizeInBytes
```

## <a name="understanding-types-of-storage-space-for-an-elastic-pool"></a>Elastik havuz için depolama alanı türlerini anlama

Aşağıdaki depolama alanı miktarlarını anlamak, elastik havuzun dosya alanını yönetmek için önemlidir.

|Elastik havuz miktarı|Tanım|Yorumlar|
|---|---|---|
|**Kullanılan veri alanı**|Elastik havuzdaki tüm veritabanları tarafından kullanılan veri alanının toplamı.||
|**Ayrılan veri alanı**|Elastik havuzdaki tüm veritabanları tarafından ayrılan veri alanı toplamı.||
|**Ayrılan ancak kullanılmayan veri alanı**|Elastik havuzdaki tüm veritabanları tarafından kullanılan veri alanı miktarı ve veri alanı arasındaki fark.|Bu miktar, veritabanı veri dosyalarını küçülterek geri kazanılabilen elastik havuz için ayrılan en fazla alan miktarını temsil eder.|
|**Veri boyutu üst sınırı**|Tüm veritabanları için elastik havuz tarafından kullanılabilecek maksimum veri alanı miktarı.|Elastik havuz için ayrılan alan, esnek havuz en büyük boyutunu aşmamalıdır.  Bu durum oluşursa, kullanılmayan alana ayrılan alan, veritabanı veri dosyaları küçültülebileceğinden geri kazanılır.|

## <a name="query-an-elastic-pool-for-storage-space-information"></a>Depolama alanı bilgileri için elastik havuz sorgulama

Aşağıdaki sorguları kullanarak elastik havuzda ne kadar depolama alanı kaldığını öğrenebilirsiniz.  

### <a name="elastic-pool-data-space-used"></a>Kullanılan elastik havuz veri alanı

Kullanılan elastik havuz veri alanı miktarını döndürmek için aşağıdaki sorguyu değiştirin.  Sorgu sonucunun birimleri MB 'tır.

```sql
-- Connect to master
-- Elastic pool data space used in MB  
SELECT TOP 1 avg_storage_percent / 100.0 * elastic_pool_storage_limit_mb AS ElasticPoolDataSpaceUsedInMB
FROM sys.elastic_pool_resource_stats
WHERE elastic_pool_name = 'ep1'
ORDER BY end_time DESC
```

### <a name="elastic-pool-data-space-allocated-and-unused-allocated-space"></a>Elastik havuz veri alanı ayrılmış ve kullanılmayan ayrılan alan

Elastik havuzdaki her bir veritabanı için ayrılan alanı ve kullanılmayan alanı listelemek için bir tablo döndürmek üzere aşağıdaki örnekleri değiştirin. Tablo, kullanılmayan ayrılmış alana en büyük miktarda kullanılmayan ayrılmış alana sahip veritabanlarını bu veritabanlarından sıralar.  Sorgu sonucunun birimleri MB 'tır.  

Havuzdaki her bir veritabanı için ayrılan alanı belirlemeye yönelik sorgu sonuçları, elastik havuz için ayrılan toplam alanı belirlemek için birlikte eklenebilir. Ayrılan elastik havuz alanı, esnek havuz en büyük boyutunu aşmamalıdır.  

> [!IMPORTANT]
> PowerShell Azure Resource Manager modülü Azure SQL veritabanı tarafından hala desteklenmektedir, ancak gelecekteki tüm geliştirmeler az. SQL modülüne yöneliktir. AzureRM modülü, en az Aralık 2020 ' e kadar hata düzeltmeleri almaya devam edecektir. Az Module ve Azurerd modüllerinde komutların bağımsız değişkenleri önemli ölçüde aynıdır. Uyumluluklarını hakkında daha fazla bilgi için bkz. [new Azure PowerShell konusuna giriş az Module](/powershell/azure/new-azureps-module-az).

PowerShell betiği SQL Server PowerShell modülünü gerektiriyor. yüklemek için [PowerShell modülünü indirme](https://docs.microsoft.com/sql/powershell/download-sql-server-ps-module) bölümüne bakın.

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

Aşağıdaki ekran görüntüsü, betiğin çıkışının bir örneğidir:

![Elastik havuz ayrılmış alanı ve kullanılmayan ayrılmış alan örneği](./media/file-space-manage/elastic-pool-allocated-unused.png)

### <a name="elastic-pool-data-max-size"></a>Elastik havuz verileri en büyük boyutu

Aşağıdaki T-SQL sorgusunu, elastik havuz verilerinin en büyük boyutunu döndürecek şekilde değiştirin.  Sorgu sonucunun birimleri MB 'tır.

```sql
-- Connect to master
-- Elastic pools max size in MB
SELECT TOP 1 elastic_pool_storage_limit_mb AS ElasticPoolMaxSizeInMB
FROM sys.elastic_pool_resource_stats
WHERE elastic_pool_name = 'ep1'
ORDER BY end_time DESC
```

## <a name="reclaim-unused-allocated-space"></a>Kullanılmayan ayrılmış alanı geri kazan

> [!NOTE]
> Bu komut, çalışırken veritabanı performansını etkileyebilir ve mümkünse düşük kullanım dönemlerinde çalıştırılmalıdır.

### <a name="dbcc-shrink"></a>DBCC küçültme

Kullanılmayan geri kazanma ayrılmış alanı için veritabanları tanımlandıktan sonra, her bir veritabanı için veri dosyalarını daraltmak üzere aşağıdaki komutta veritabanının adını değiştirin.

```sql
-- Shrink database data space allocated.
DBCC SHRINKDATABASE (N'db1')
```

Bu komut, çalışırken veritabanı performansını etkileyebilir ve mümkünse düşük kullanım dönemlerinde çalıştırılmalıdır.  

Bu komut hakkında daha fazla bilgi için bkz. [SHRINKDATABASE](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-shrinkdatabase-transact-sql).

### <a name="auto-shrink"></a>Otomatik küçültme

Alternatif olarak, Otomatik küçültme bir veritabanı için etkinleştirilebilir.  Otomatik küçültme, dosya yönetimi karmaşıklığını azaltır ve veritabanı performansına göre daha az etkili olur `SHRINKDATABASE` `SHRINKFILE` .  Otomatik küçültme özellikle birçok veritabanı ile elastik havuzların yönetilmesi için yararlı olabilir.  Ancak Otomatik küçültme, geri kazanma dosya alanında ve ' den daha az etkili `SHRINKDATABASE` olabilir `SHRINKFILE` .
Otomatik küçültmeyi etkinleştirmek için aşağıdaki komutta veritabanının adını değiştirin.

```sql
-- Enable auto-shrink for the database.
ALTER DATABASE [db1] SET AUTO_SHRINK ON
```

Bu komut hakkında daha fazla bilgi için bkz. [VERITABANı kümesi](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-set-options?view=azuresqldb-current) seçenekleri.

### <a name="rebuild-indexes"></a>Dizinleri yeniden oluştur

Veritabanı veri dosyaları daraltıladıktan sonra, dizinler parçalanabilir ve performans iyileştirmesi verimliliğini kaybedebilir. Performans düşüşü gerçekleşirse, veritabanı dizinlerini yeniden oluşturmayı düşünün. Parçalama ve dizinleri yeniden oluşturma hakkında daha fazla bilgi için bkz. [dizinleri yeniden düzenleme ve yeniden oluşturma](https://docs.microsoft.com/sql/relational-databases/indexes/reorganize-and-rebuild-indexes).

## <a name="next-steps"></a>Sonraki adımlar

- En fazla veritabanı boyutu hakkında daha fazla bilgi için bkz.:
  - [Tek bir veritabanı için Azure SQL veritabanı sanal çekirdek tabanlı satın alma modeli sınırları](resource-limits-vcore-single-databases.md)
  - [DTU tabanlı satın alma modelini kullanan tek veritabanlarına yönelik kaynak sınırları](resource-limits-dtu-single-databases.md)
  - [Esnek havuzlar için Azure SQL veritabanı sanal çekirdek tabanlı satın alma modeli sınırları](resource-limits-vcore-elastic-pools.md)
  - [DTU tabanlı satın alma modelini kullanarak elastik havuzlar için kaynak limitleri](resource-limits-dtu-elastic-pools.md)
- Komutu hakkında daha fazla bilgi için `SHRINKDATABASE` bkz. [SHRINKDATABASE](/sql/t-sql/database-console-commands/dbcc-shrinkdatabase-transact-sql).
- Parçalama ve dizinleri yeniden oluşturma hakkında daha fazla bilgi için bkz. [dizinleri yeniden düzenleme ve yeniden oluşturma](/sql/relational-databases/indexes/reorganize-and-rebuild-indexes).
