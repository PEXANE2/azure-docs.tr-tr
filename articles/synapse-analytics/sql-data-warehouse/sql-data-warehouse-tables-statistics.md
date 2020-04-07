---
title: İstatistik oluşturma, güncelleme
description: Synapse SQL havuzundaki tablolarda sorgu optimizasyonu istatistikleri oluşturmak ve güncelleştirmek için öneriler ve örnekler.
services: synapse-analytics
author: XiaoyuMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 05/09/2018
ms.author: xiaoyul
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: 5fae2bba0acc4ab462c91f7272694d032fc6ceaa
ms.sourcegitcommit: bd5fee5c56f2cbe74aa8569a1a5bce12a3b3efa6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80742670"
---
# <a name="table-statistics-in-synapse-sql-pool"></a>Synapse SQL havuzunda tablo istatistikleri

Bu makalede, SQL havuzundaki tablolarda sorgu optimizasyonu istatistikleri oluşturmak ve güncelleştirmek için öneriler ve örnekler bulacaksınız.

## <a name="why-use-statistics"></a>İstatistikleri neden kullanır?

SQL havuzu verileriniz hakkında ne kadar çok şey bilirse, karşı sorguları o kadar hızlı yürütebilir. VERILERI SQL havuzuna yükledikten sonra, verilerinizle ilgili istatistikler toplamak sorgularınızı en iyi duruma getirmek için yapabileceğiniz en önemli şeylerden biridir.

SQL havuz sorgu optimize edici maliyet tabanlı bir optimizer olduğunu. Çeşitli sorgu planlarının maliyetini karşılaştırır ve ardından planı en düşük maliyetle seçer. Çoğu durumda, en hızlı yürütecek planı seçer.

Örneğin, en iyi duruma getirici, sorgunuzun filtrelenediğini tahmin ederse bir satır döndürür ve bir plan seçer. Seçili tarihin 1 milyon satır döndüreceğini tahmin ederse, farklı bir plan döndürecektir.

## <a name="automatic-creation-of-statistic"></a>İstatistiklerin otomatik oluşturulması

Veritabanı AUTO_CREATE_STATISTICS seçeneği anızda, SQL havuzu eksik istatistikler için gelen kullanıcı sorgularını çözümler.

İstatistikler eksikse, sorgu optimize edici, sorgu planının önemli lik tahminlerini iyileştirmek için sorgu yüklemindeki tek tek sütunlar veya birleştirme koşulu yla ilgili istatistikler oluşturur.

> [!NOTE]
> İstatistiklerin otomatik oluşturulması şu anda varsayılan olarak açık.

SQL havuzunuzun AUTO_CREATE_STATISTICS aşağıdaki komutu çalıştırarak yapılandırılıp yapılandırılmadınızı kontrol edebilirsiniz:

```sql
SELECT name, is_auto_create_stats_on
FROM sys.databases
```

SQL havuzunuzda AUTO_CREATE_STATISTICS yapılandırılmamışsa, aşağıdaki komutu çalıştırarak bu özelliği etkinleştirmenizi öneririz:

```sql
ALTER DATABASE <yourdatawarehousename>
SET AUTO_CREATE_STATISTICS ON
```

Bu ifadeler istatistiklerin otomatik oluşturulmasını tetikler:

- SELECT
- EKLE-SEÇ
- CTAS
- UPDATE
- DELETE
- Birbirleştirme içerdiğinde veya bir yüklemin varlığı algılandığında AçıkLA

> [!NOTE]
> İstatistiklerin otomatik oluşturulması geçici veya dış tablolarda oluşturulmaz.

İstatistiklerin otomatik olarak oluşturulması eşzamanlı olarak yapılır, bu nedenle sütunlarınız istatistikleri eksikse biraz bozulmuş sorgu performansına maruz kalabilirsiniz. Tek bir sütun için istatistik oluşturma zamanı tablonun boyutuna bağlıdır.

Ölçülebilir performans bozulmasını önlemek için, sistem profil oluşturmadan önce kıyaslama iş yükünü çalıştırarak önce istatistiklerin oluşturulduğundan emin olmalısınız.

> [!NOTE]
> İstatistiklerin oluşturulması farklı bir kullanıcı bağlamında [sys.dm_pdw_exec_requests](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) günlüğe kaydedilir.

Otomatik istatistikler oluşturulduğunda, onlar formu alır: _hex_>_ 8 haneli sütun id<WA_Sys> 8 haneli tablo id<. [DBCC SHOW_STATISTICS](/sql/t-sql/database-console-commands/dbcc-show-statistics-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) komutunu çalıştırarak zaten oluşturulmuş istatistikleri görüntüleyebilirsiniz:

```sql
DBCC SHOW_STATISTICS (<table_name>, <target>)
```

table_name, görüntülenecek istatistikleri içeren tablonun adıdır. Bu tablo harici bir tablo olamaz. Hedef, istatistik bilgilerini görüntülemek için hedef dizinin, istatistiklerin veya sütunun adıdır.

## <a name="update-statistics"></a>İstatistikleri güncelleştirme

En iyi uygulama, yeni tarihler eklendikçe tarih sütunlarına ilişkin istatistikleri her gün güncelleştirmektir. Sql havuzuna her yeni satır yüklendiğinde, yeni yükleme tarihleri veya hareket tarihleri eklenir. Bu eklemeler veri dağıtımını değiştirir ve istatistikleri güncel hale getirin.

Değer dağılımı genellikle değişmediğinden, müşteri tablosundaki ülke/bölge sütunundaki istatistiklerin hiçbir zaman güncelleştirilmesi gerekmeyebilir. Dağıtımın müşteriler arasında sabit olduğunu varsayarsak, tablo varyasyonuna yeni satırlar eklemek veri dağıtımını değiştirmez.

Ancak, SQL havuzunuzyalnızca bir ülke/bölge içeriyorsa ve yeni bir ülkeden/bölgeden veri getiriyorsanız ve bu da birden çok ülkeden/bölgeden verilerin depolanmasına neden oluyorsa, ülke/bölge sütunundaki istatistikleri güncelleştirmeniz gerekir.

İstatistikleri güncelleyen öneriler şunlardır:

|||
|-|-|
| **İstatistik güncelleştirmelerinin sıklığı**  | Muhafazakar: Günlük </br> Verilerinizi yükledikten veya dönüştürdükten sonra |
| **Örnekleme** |  1 milyardan az satır, varsayılan örnekleme (yüzde 20) kullanın. </br> 1 milyardan fazla satırda, yüzde iki örnekleme kullanın. |

Sorgusorunu giderirken sormanız gereken ilk sorulardan biri **şudur: "İstatistikler güncel mi?"**

Bu soru, verilerin yaşına göre cevaplanabilecek bir soru değildir. Temel verilerde herhangi bir maddi değişiklik yoksa, güncel bir istatistik nesnesi eski olabilir.

> [!TIP]
> Satır sayısı önemli ölçüde değiştiğinde veya bir sütuniçin değerlerin dağılımında önemli bir değişiklik *olduğunda,* istatistikleri güncelleştirme zamanı gelmiştir.

İstatistikler son kez güncelleştirilip güncelleştirilmediğinden beri tablo içindeki verilerin değişip değişmediğini belirlemek için dinamik bir yönetim görünümü yoktur. İstatistiklerinizin yaşını bilmek, resmin bir kısmını sağlayabilir.

İstatistiklerinizin her tabloda en son ne zaman güncelleştirileni belirlemek için aşağıdaki sorguyu kullanabilirsiniz.

> [!NOTE]
> Bir sütuniçin değerlerin dağılımında önemli bir değişiklik varsa, istatistikleri en son ne zaman güncelleştirildiklerine bakılmaksızın güncelleştirmeniz gerekir.

```sql
SELECT
    sm.[name] AS [schema_name],
    tb.[name] AS [table_name],
    co.[name] AS [stats_column_name],
    st.[name] AS [stats_name],
    STATS_DATE(st.[object_id],st.[stats_id]) AS [stats_last_updated_date]
FROM
    sys.objects ob
    JOIN sys.stats st
        ON  ob.[object_id] = st.[object_id]
    JOIN sys.stats_columns sc
        ON  st.[stats_id] = sc.[stats_id]
        AND st.[object_id] = sc.[object_id]
    JOIN sys.columns co
        ON  sc.[column_id] = co.[column_id]
        AND sc.[object_id] = co.[object_id]
    JOIN sys.types  ty
        ON  co.[user_type_id] = ty.[user_type_id]
    JOIN sys.tables tb
        ON  co.[object_id] = tb.[object_id]
    JOIN sys.schemas sm
        ON  tb.[schema_id] = sm.[schema_id]
WHERE
    st.[user_created] = 1;
```

Örneğin, BIR SQL havuzundaki **tarih sütunları** genellikle sık sık istatistik güncelleştirmeleri gerekir. Sql havuzuna her yeni satır yüklendiğinde, yeni yükleme tarihleri veya hareket tarihleri eklenir. Bu eklemeler veri dağıtımını değiştirir ve istatistikleri güncel hale getirin.

Tersine, müşteri tablosundaki cinsiyet sütunundaki istatistiklerin hiçbir zaman güncelleştirilmesi gerekmeyebilir. Dağıtımın müşteriler arasında sabit olduğunu varsayarsak, tablo varyasyonuna yeni satırlar eklemek veri dağıtımını değiştirmez.

SQL havuzunuz yalnızca bir cinsiyet içeriyorsa ve birden çok cinsiyette yeni bir gereksinim sonuç alıyorsa, cinsiyet sütunundaki istatistikleri güncelleştirmeniz gerekir.

Daha fazla bilgi için [İstatistikler](/sql/relational-databases/statistics/statistics?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)için genel kılavuza bakın.

## <a name="implementing-statistics-management"></a>İstatistik yönetiminin uygulanması

İstatistiklerin yükün sonunda güncelleştirilmesini sağlamak için veri yükleme işlemini genişletmek genellikle iyi bir fikirdir.

Veri yükü, tabloların boyutlarını ve/veya değer dağılımlarını en sık değiştirdiği zamandır. Veri yükleme, bazı yönetim süreçlerini uygulamak için mantıklı bir yerdir.

Yükleme işlemi sırasında istatistiklerinizi güncelleştirmek için aşağıdaki kılavuz ilkeler sağlanır:

- Yüklenen her tabloda en az bir istatistik nesnesi güncelleştirdiğinden emin olun. Bu, istatistik güncelleştirmesinin bir parçası olarak tablo boyutunu (satır sayısı ve sayfa sayısı) bilgilerini güncelleştirir.
- JOIN, GROUP BY, ORDER BY ve DISTINCT yan tümcelerine katılan sütunlara odaklanın.
- Bu değerler histogram istatistiklerine dahil edilmeyeceğine göre, işlem tarihleri gibi "artan anahtar" sütunlarını daha sık güncelleştirmeyi düşünün.
- Statik dağıtım sütunlarını daha az sıklıkta güncelleştirmeyi düşünün.
- Unutmayın, her istatistik nesnesi sırayla güncelleştirilir. Özellikle çok `UPDATE STATISTICS <TABLE_NAME>` sayıda istatistik nesnesi olan geniş tablolar için, uygulama her zaman ideal değildir.

Daha fazla bilgi için [Kardinallik Tahmini'ne](/sql/relational-databases/performance/cardinality-estimation-sql-server?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)bakın.

## <a name="examples-create-statistics"></a>Örnekler: İstatistik oluşturma

Bu örnekler, istatistik oluşturmak için çeşitli seçeneklerin nasıl kullanılacağını gösterir. Her sütun için kullandığınız seçenekler, verilerinizin özelliklerine ve sütunun sorgularda nasıl kullanılacağına bağlıdır.

### <a name="create-single-column-statistics-with-default-options"></a>Varsayılan seçenekleri olan tek sütunlu istatistikler oluşturma

Bir sütunda istatistik oluşturmak için, istatistik nesnesi ve sütunun adı için bir ad sağlayın.

Bu sözdizimi tüm varsayılan seçenekleri kullanır. Varsayılan olarak, SQL havuzu istatistik oluştururken tablonun **yüzde 20'sini** örnekler.

```sql
CREATE STATISTICS [statistics_name] ON [schema_name].[table_name]([column_name]);
```

Örnek:

```sql
CREATE STATISTICS col1_stats ON dbo.table1 (col1);
```

### <a name="create-single-column-statistics-by-examining-every-row"></a>Her satırı inceleyerek tek sütunlu istatistikler oluşturma

Yüzde 20'lik varsayılan örnekleme oranı çoğu durum için yeterlidir. Ancak, örnekleme hızını ayarlayabilirsiniz.

Tam tabloyu örneklemek için şu sözdizimini kullanın:

```sql
CREATE STATISTICS [statistics_name] ON [schema_name].[table_name]([column_name]) WITH FULLSCAN;
```

Örnek:

```sql
CREATE STATISTICS col1_stats ON dbo.table1 (col1) WITH FULLSCAN;
```

### <a name="create-single-column-statistics-by-specifying-the-sample-size"></a>Örnek boyutunu belirterek tek sütunlu istatistikler oluşturma

Alternatif olarak, örnek boyutunu yüzde olarak belirtebilirsiniz:

```sql
CREATE STATISTICS col1_stats ON dbo.table1 (col1) WITH SAMPLE = 50 PERCENT;
```

### <a name="create-single-column-statistics-on-only-some-of-the-rows"></a>Yalnızca bazı satırlarda tek sütunlu istatistikler oluşturma

Ayrıca tablonuzdaki satırların bir bölümü için istatistikler oluşturabilirsiniz. Buna filtreuygulanmış istatistik denir.

Örneğin, büyük bölümlenmiş bir tablonun belirli bir bölümü sorgulamayı planlarken filtreuygulanmış istatistikleri kullanabilirsiniz. Yalnızca bölüm değerleri yle ilgili istatistikler oluşturarak, istatistiklerin doğruluğu iyileşir ve bu nedenle sorgu performansını artırır.

Bu örnek, bir değer aralığı yla ilgili istatistikler oluşturur. Değerler, bir bölümdeki değer aralığıyla eşleşecek şekilde kolayca tanımlanabilir.

```sql
CREATE STATISTICS stats_col1 ON table1(col1) WHERE col1 > '2000101' AND col1 < '20001231';
```

> [!NOTE]
> Sorgu optimize edicinin dağıtılmış sorgu planını seçerken filtrelenmiş istatistikleri kullanmayı düşünemesi için, sorgunun istatistik nesnesinin tanımına sığması gerekir. Önceki örneği kullanarak, sorgunun WHERE yan tümcesinin 2000101 ile 20001231 arasında col1 değerlerini belirtmesi gerekir.

### <a name="create-single-column-statistics-with-all-the-options"></a>Tüm seçenekleri içeren tek sütunlu istatistikler oluşturma

Seçenekleri bir araya da getirebilirsiniz. Aşağıdaki örnek, özel bir örnek boyutu ile filtreuygulanmış bir istatistik nesnesi oluşturur:

```sql
CREATE STATISTICS stats_col1 ON table1 (col1) WHERE col1 > '2000101' AND col1 < '20001231' WITH SAMPLE = 50 PERCENT;
```

Tam başvuru için, CREATE [STATISTICS'a](/sql/t-sql/statements/create-statistics-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)bakın.

### <a name="create-multi-column-statistics"></a>Çok sütunlu istatistikler oluşturma

Çok sütunlu istatistik nesnesi oluşturmak için önceki örnekleri kullanın, ancak daha fazla sütun belirtin.

> [!NOTE]
> Sorgu sonucundaki satır sayısını tahmin etmek için kullanılan histogram, yalnızca istatistik nesnesi tanımında listelenen ilk sütun için kullanılabilir.

Bu örnekte, histogram *ürün\_kategorisindedir.* Çapraz sütun istatistikleri *ürün\_kategorisi* ve ürün *\_sub_category*hesaplanır:

```sql
CREATE STATISTICS stats_2cols ON table1 (product_category, product_sub_category) WHERE product_category > '2000101' AND product_category < '20001231' WITH SAMPLE = 50 PERCENT;
```

*Ürün\_kategorisi* ile *ürün\_\_alt kategorisi*arasında bir korelasyon olduğundan, bu sütunlara aynı anda erişilirse çok sütunlu istatistik nesnesi yararlı olabilir.

### <a name="create-statistics-on-all-columns-in-a-table"></a>Tablodaki tüm sütunlarda istatistik oluşturma

İstatistik oluşturmanın bir yolu, tabloyu oluşturduktan sonra CREATE STATISTICS komutları vermektir:

```sql
CREATE TABLE dbo.table1
(
   col1 int
,  col2 int
,  col3 int
)
WITH
  (
    CLUSTERED COLUMNSTORE INDEX
  )
;

CREATE STATISTICS stats_col1 on dbo.table1 (col1);
CREATE STATISTICS stats_col2 on dbo.table2 (col2);
CREATE STATISTICS stats_col3 on dbo.table3 (col3);
```

### <a name="use-a-stored-procedure-to-create-statistics-on-all-columns-in-a-database"></a>Veritabanındaki tüm sütunlarda istatistik oluşturmak için depolanmış yordamı kullanma

SQL havuzunda SQL Server'da sp_create_stats eşdeğer bir sistem depolanan yordamı yoktur. Bu depolanan yordam, veritabanının zaten istatistikleri olmayan her sütununda tek bir sütun istatistik nesnesi oluşturur.

Aşağıdaki örnek, veritabanı tasarımınıza başlamanıza yardımcı olacaktır. İhtiyaçlarınıza göre uyarlamakiçin çekinmeyin.

```sql
CREATE PROCEDURE    [dbo].[prc_sqldw_create_stats]
(   @create_type    tinyint -- 1 default 2 Fullscan 3 Sample
,   @sample_pct     tinyint
)
AS

IF @create_type IS NULL
BEGIN
    SET @create_type = 1;
END;

IF @create_type NOT IN (1,2,3)
BEGIN
    THROW 151000,'Invalid value for @stats_type parameter. Valid range 1 (default), 2 (fullscan) or 3 (sample).',1;
END;

IF @sample_pct IS NULL
BEGIN;
    SET @sample_pct = 20;
END;

IF OBJECT_ID('tempdb..#stats_ddl') IS NOT NULL
BEGIN;
    DROP TABLE #stats_ddl;
END;

CREATE TABLE #stats_ddl
WITH    (   DISTRIBUTION    = HASH([seq_nmbr])
        ,   LOCATION        = USER_DB
        )
AS
WITH T
AS
(
SELECT      t.[name]                        AS [table_name]
,           s.[name]                        AS [table_schema_name]
,           c.[name]                        AS [column_name]
,           c.[column_id]                   AS [column_id]
,           t.[object_id]                   AS [object_id]
,           ROW_NUMBER()
            OVER(ORDER BY (SELECT NULL))    AS [seq_nmbr]
FROM        sys.[tables] t
JOIN        sys.[schemas] s         ON  t.[schema_id]       = s.[schema_id]
JOIN        sys.[columns] c         ON  t.[object_id]       = c.[object_id]
LEFT JOIN   sys.[stats_columns] l   ON  l.[object_id]       = c.[object_id]
                                    AND l.[column_id]       = c.[column_id]
                                    AND l.[stats_column_id] = 1
LEFT JOIN    sys.[external_tables] e    ON    e.[object_id]        = t.[object_id]
WHERE       l.[object_id] IS NULL
AND            e.[object_id] IS NULL -- not an external table
)
SELECT  [table_schema_name]
,       [table_name]
,       [column_name]
,       [column_id]
,       [object_id]
,       [seq_nmbr]
,       CASE @create_type
        WHEN 1
        THEN    CAST('CREATE STATISTICS '+QUOTENAME('stat_'+table_schema_name+ '_' + table_name + '_'+column_name)+' ON '+QUOTENAME(table_schema_name)+'.'+QUOTENAME(table_name)+'('+QUOTENAME(column_name)+')' AS VARCHAR(8000))
        WHEN 2
        THEN    CAST('CREATE STATISTICS '+QUOTENAME('stat_'+table_schema_name+ '_' + table_name + '_'+column_name)+' ON '+QUOTENAME(table_schema_name)+'.'+QUOTENAME(table_name)+'('+QUOTENAME(column_name)+') WITH FULLSCAN' AS VARCHAR(8000))
        WHEN 3
        THEN    CAST('CREATE STATISTICS '+QUOTENAME('stat_'+table_schema_name+ '_' + table_name + '_'+column_name)+' ON '+QUOTENAME(table_schema_name)+'.'+QUOTENAME(table_name)+'('+QUOTENAME(column_name)+') WITH SAMPLE '+CONVERT(varchar(4),@sample_pct)+' PERCENT' AS VARCHAR(8000))
        END AS create_stat_ddl
FROM T
;

DECLARE @i INT              = 1
,       @t INT              = (SELECT COUNT(*) FROM #stats_ddl)
,       @s NVARCHAR(4000)   = N''
;

WHILE @i <= @t
BEGIN
    SET @s=(SELECT create_stat_ddl FROM #stats_ddl WHERE seq_nmbr = @i);

    PRINT @s
    EXEC sp_executesql @s
    SET @i+=1;
END

DROP TABLE #stats_ddl;
```

Varsayılanları kullanarak tablodaki tüm sütunlarda istatistik oluşturmak için depolanan yordamı çalıştırın.

```sql
EXEC [dbo].[prc_sqldw_create_stats] 1, NULL;
```

Tam scan kullanarak tablodaki tüm sütunlar üzerinde istatistik oluşturmak için bu yordamı arayın.

```sql
EXEC [dbo].[prc_sqldw_create_stats] 2, NULL;
```

Tablodaki tüm sütunlarda örneklenmiş istatistikler oluşturmak için 3 ve örnek yüzdesi girin. Bu yordam yüzde 20 örnek oranı kullanır.

```sql
EXEC [dbo].[prc_sqldw_create_stats] 3, 20;
```

## <a name="examples-update-statistics"></a>Örnekler: İstatistikleri güncelleştir

İstatistikleri güncelleştirmek için şunları yapabilirsiniz:

- Bir istatistik nesnesini güncelleştirin. Güncelleştirmek istediğiniz istatistik nesnesinin adını belirtin.
- Tablodaki tüm istatistik nesnelerini güncelleştirin. Belirli bir istatistik nesnesi yerine tablonun adını belirtin.

### <a name="update-one-specific-statistics-object"></a>Belirli bir istatistik nesnesini güncelleştirme

Belirli bir istatistik nesnesini güncelleştirmek için aşağıdaki sözdizimini kullanın:

```sql
UPDATE STATISTICS [schema_name].[table_name]([stat_name]);
```

Örnek:

```sql
UPDATE STATISTICS [dbo].[table1] ([stats_col1]);
```

Belirli istatistik nesnelerini güncelleştirerek, istatistikleri yönetmek için gereken zamanı ve kaynakları en aza indirebilirsiniz. Bunu yapmak için güncellemek için en iyi istatistik nesneleri seçmek için bazı düşünce gerektirir.

### <a name="update-all-statistics-on-a-table"></a>Tablodaki tüm istatistikleri güncelleştirme

Tablodaki tüm istatistik nesnelerini güncelleştirmek için basit bir yöntem:

```sql
UPDATE STATISTICS [schema_name].[table_name];
```

Örnek:

```sql
UPDATE STATISTICS dbo.table1;
```

UPDATE İstATİstİkler deyiminin kullanımı kolaydır. Yalnızca, masadaki *tüm* istatistikleri günceller ve bu nedenle gerekenden daha fazla iş gerçekleştirebileceğini unutmayın. Performans bir sorun değilse, istatistiklerin güncel olduğunu garanti etmenin en kolay ve en eksiksiz yoludur.

> [!NOTE]
> Bir tablodaki tüm istatistikleri güncellerken, SQL havuzu her istatistik nesnesi için tabloyu örneklemek için bir taramaya yapar. Tablo büyükse ve çok sayıda sütun ve çok sayıda istatistik varsa, tek tek istatistikleri ihtiyaca göre güncelleştirmek daha verimli olabilir.

Yordamın `UPDATE STATISTICS` uygulanması için [Geçici Tablolar'a](sql-data-warehouse-tables-temporary.md)bakın. Uygulama yöntemi önceki `CREATE STATISTICS` yordamdan biraz farklıdır, ancak sonuç aynıdır.

Sözdiziminin tamamı için [Istatistikleri Güncelleştir'e](/sql/t-sql/statements/update-statistics-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)bakın.

## <a name="statistics-metadata"></a>İstatistikler meta verileri

İstatistikler hakkında bilgi bulmak için kullanabileceğiniz çeşitli sistem görünümleri ve işlevleri vardır. Örneğin, istatistiklerin en son ne zaman oluşturulduğunu veya güncelleştirilmeye çalışıldığını görmek için istatistik tarihi işlevini kullanarak bir istatistik nesnesinin güncel olup olmadığını görebilirsiniz.

### <a name="catalog-views-for-statistics"></a>İstatistikler için katalog görünümleri

Bu sistem görünümleri istatistikler hakkında bilgi sağlar:

| Katalog görünümü | Açıklama |
|:--- |:--- |
| [Columns](/sql/relational-databases/system-catalog-views/sys-columns-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) |Her sütun için bir satır. |
| [Objects](/sql/relational-databases/system-catalog-views/sys-objects-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) |Veritabanındaki her nesne için bir satır. |
| [sys.şemalar](/sql/relational-databases/system-catalog-views/sys-objects-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) |Veritabanındaki her şema için bir satır. |
| [Stats](/sql/relational-databases/system-catalog-views/sys-stats-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) |Her istatistik nesnesi için bir satır. |
| [sys.stats_columns](/sql/relational-databases/system-catalog-views/sys-stats-columns-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) |İstatistik nesnesindeki her sütun için bir satır. Sys.columns'a geri bağlantılar. |
| [Tables](/sql/relational-databases/system-catalog-views/sys-tables-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) |Her tablo için bir satır (dış tabloları içerir). |
| [sys.table_types](/sql/relational-databases/system-catalog-views/sys-table-types-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) |Her veri türü için bir satır. |

### <a name="system-functions-for-statistics"></a>İstatistikler için sistem fonksiyonları

Bu sistem işlevleri istatistiklerle çalışmak için yararlıdır:

| Sistem fonksiyonu | Açıklama |
|:--- |:--- |
| [STATS_DATE](/sql/t-sql/functions/stats-date-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) |İstatistik nesnesinin en son güncelleştirilen tarih. |
| [DBCC SHOW_STATISTICS](/sql/t-sql/database-console-commands/dbcc-show-statistics-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) |İstatistik nesnesi tarafından anlaşıldığı gibi değerlerin dağılımı hakkında özet düzeyi ve ayrıntılı bilgi. |

### <a name="combine-statistics-columns-and-functions-into-one-view"></a>İstatistik sütunlarını ve işlevlerini tek bir görünümde birleştirme

Bu görünüm, istatistiklerle ilgili sütunları ve STATS_DATE() işlevinden elde edilen sonuçları bir araya getirir.

```sql
CREATE VIEW dbo.vstats_columns
AS
SELECT
        sm.[name]                           AS [schema_name]
,       tb.[name]                           AS [table_name]
,       st.[name]                           AS [stats_name]
,       st.[filter_definition]              AS [stats_filter_definition]
,       st.[has_filter]                     AS [stats_is_filtered]
,       STATS_DATE(st.[object_id],st.[stats_id])
                                            AS [stats_last_updated_date]
,       co.[name]                           AS [stats_column_name]
,       ty.[name]                           AS [column_type]
,       co.[max_length]                     AS [column_max_length]
,       co.[precision]                      AS [column_precision]
,       co.[scale]                          AS [column_scale]
,       co.[is_nullable]                    AS [column_is_nullable]
,       co.[collation_name]                 AS [column_collation_name]
,       QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])
                                            AS two_part_name
,       QUOTENAME(DB_NAME())+'.'+QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])
                                            AS three_part_name
FROM    sys.objects                         AS ob
JOIN    sys.stats           AS st ON    ob.[object_id]      = st.[object_id]
JOIN    sys.stats_columns   AS sc ON    st.[stats_id]       = sc.[stats_id]
                            AND         st.[object_id]      = sc.[object_id]
JOIN    sys.columns         AS co ON    sc.[column_id]      = co.[column_id]
                            AND         sc.[object_id]      = co.[object_id]
JOIN    sys.types           AS ty ON    co.[user_type_id]   = ty.[user_type_id]
JOIN    sys.tables          AS tb ON  co.[object_id]        = tb.[object_id]
JOIN    sys.schemas         AS sm ON  tb.[schema_id]        = sm.[schema_id]
WHERE   1=1
AND     st.[user_created] = 1
;
```

## <a name="dbcc-show_statistics-examples"></a>DBCC SHOW_STATISTICS() örnekleri

DBCC SHOW_STATISTICS() bir istatistik nesnesi içinde tutulan verileri gösterir. Bu veriler üç bölümden oluşur:

- Üst bilgi
- Yoğunluk vektörü
- Histogram

İstatistikler hakkında üstbilgi meta verileri. Histogram, istatistik nesnesinin ilk anahtar sütunundaki değerlerin dağılımını görüntüler. Yoğunluk vektörü çapraz sütun bağıntısını ölçer.

> [!NOTE]
> SQL havuzu, istatistik nesnesindeki verilerden herhangi biriyle kardinallik tahminlerini hesaplar.

### <a name="show-header-density-and-histogram"></a>Üstbilgi, yoğunluk ve histogramı göster

Bu basit örnek, istatistik nesnesinin üç bölümünü de gösterir:

```sql
DBCC SHOW_STATISTICS([<schema_name>.<table_name>],<stats_name>)
```

Örnek:

```sql
DBCC SHOW_STATISTICS (dbo.table1, stats_col1);
```

### <a name="show-one-or-more-parts-of-dbcc-show_statistics"></a>DBCC SHOW_STATISTICS bir veya daha fazla bölümünü göster()

Yalnızca belirli bölümleri görüntülemekistiyorsanız, yan tümceyi `WITH` kullanın ve hangi bölümleri görmek istediğinizi belirtin:

```sql
DBCC SHOW_STATISTICS([<schema_name>.<table_name>],<stats_name>) WITH stat_header, histogram, density_vector
```

Örnek:

```sql
DBCC SHOW_STATISTICS (dbo.table1, stats_col1) WITH histogram, density_vector
```

## <a name="dbcc-show_statistics-differences"></a>DBCC SHOW_STATISTICS() farkları

DBCC SHOW_STATISTICS() SQL Server'a göre SQL havuzunda daha sıkı bir şekilde uygulanır:

- Belgelenmemiş özellikler desteklenmez.
- Stats_stream kullanamaz.
- Belirli istatistik veri alt kümeleri için sonuçlara katılamaz. Örneğin, DENSITY_VECTOR join STAT_HEADER.
- NO_INFOMSGS ileti bastırma için ayarlanamaz.
- İstatistik adlarının etrafındaki kare ayraçlar kullanılamaz.
- İstatistik nesnelerini tanımlamak için sütun adlarını kullanamaz.
- Özel hata 2767 desteklenmez.

## <a name="next-steps"></a>Sonraki adımlar

Sorgu performansını daha da artırmak için [bkz.](sql-data-warehouse-manage-monitor.md)
