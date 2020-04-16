---
title: İstatistik oluşturma, güncelleme
description: Synapse SQL'de sorgu optimizasyonu istatistikleri oluşturmak ve güncelleştirmek için öneriler ve örnekler.
services: synapse-analytics
author: filippopovic
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/15/2020
ms.author: fipopovi
ms.reviewer: jrasnick
ms.custom: ''
ms.openlocfilehash: 798fec4dacb33a9f16de319062baf12adaffdbd0
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81428751"
---
# <a name="statistics-in-synapse-sql"></a>Sinaps SQL İstatistikleri

Bu makalede sağlanan öneriler ve Synapse SQL kaynakları kullanarak sorgu optimizasyonu istatistikleri oluşturmak ve güncelleştirmek için örnekler: SQL havuzu ve SQL on-demand (önizleme).

## <a name="statistics-in-sql-pool"></a>SQL havuzundaki istatistikler

### <a name="why-use-statistics"></a>İstatistikleri neden kullanır?

SQL havuzu kaynağı verilerinizle ne kadar çok şey bilirse, sorguları o kadar hızlı yürütebilir. VERILERI SQL havuzuna yükledikten sonra, verilerinizle ilgili istatistikleri toplamak sorgu optimizasyonu için yapabileceğiniz en önemli şeylerden biridir.  

SQL havuz sorgu optimize edici maliyet tabanlı bir optimizer olduğunu. Çeşitli sorgu planlarının maliyetini karşılaştırır ve ardından planı en düşük maliyetle seçer. Çoğu durumda, en hızlı yürütecek planı seçer.

Örneğin, en iyi duruma getirici, sorgunuzun filtrelenediğini tahmin ederse bir satır döndürür ve bir plan seçer. Seçili tarihin 1 milyon satır döndüreceğini tahmin ederse, farklı bir plan döndürecektir.

### <a name="automatic-creation-of-statistics"></a>İstatistiklerin otomatik oluşturulması

SQL havuzu veritabanı AUTO_CREATE_STATISTICS seçeneği ayarlandığında eksik istatistikler için gelen `ON`kullanıcı sorgularını analiz edecek.  İstatistikler eksikse, sorgu en iyi duruma getirici, sorgu yüklemiveya birleştirme koşulundaki tek tek sütunlar hakkında istatistikler oluşturur. Bu işlev, sorgu planı için önemlilik tahminlerini geliştirmek için kullanılır.

> [!IMPORTANT]
> İstatistiklerin otomatik oluşturulması şu anda varsayılan olarak açık.

Veri ambarınızın AUTO_CREATE_STATISTICS aşağıdaki komutu çalıştırarak yapılandırılıp yapılandırılmamalarını denetleyebilirsiniz:

```sql
SELECT name, is_auto_create_stats_on
FROM sys.databases
```

Veri ambarınız AUTO_CREATE_STATISTICS etkin değilse, aşağıdaki komutu çalıştırarak bu özelliği etkinleştirmenizi öneririz:

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

İstatistiklerin otomatik oluşturulması eşzamanlı olarak yapılır. Bu nedenle, sütunlarınız istatistikleri eksikse, biraz bozulmuş sorgu performansı na tabi olabilirsiniz. Tek bir sütun için istatistik oluşturma zamanı tablonun boyutuna bağlıdır.

Ölçülebilir performans bozulmasını önlemek için, sistem profil oluşturmadan önce kıyaslama iş yükünü çalıştırarak önce istatistiklerin oluşturulduğundan emin olmalısınız.

> [!NOTE]
> İstatistiklerin oluşturulması farklı bir kullanıcı bağlamında [sys.dm_pdw_exec_requests](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) günlüğe kaydedilir.

Otomatik istatistikler oluşturulduğunda, formu alırlar: Hex>_'da 8 basamaklı sütun _id'<WA_Sys_>_ Hex>'da 8 haneli tablo id'i<. [DBCC SHOW_STATISTICS](/sql/t-sql/database-console-commands/dbcc-show-statistics-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) komutunu çalıştırarak zaten oluşturulmuş istatistikleri görüntüleyebilirsiniz:

```sql
DBCC SHOW_STATISTICS (<table_name>, <target>)
```

table_name, dış tablo olamaz görüntülemek için istatistikleri içeren tablonun adıdır. Hedef, istatistik bilgilerini görüntülemek için hedef dizinin, istatistiklerin veya sütunun adıdır.

### <a name="update-statistics"></a>İstatistikleri güncelleştirme

En iyi uygulama, yeni tarihler eklendikçe tarih sütunlarına ilişkin istatistikleri her gün güncelleştirmektir. Veri ambarına her yeni satır yüklendiğinde, yeni yükleme tarihleri veya hareket tarihleri eklenir. Bu eklemeler veri dağıtımını değiştirir ve istatistikleri güncel hale getirin.

Değerlerin dağıtımı genellikle değişmediği için, müşteri tablosundaki bir ülke veya bölge sütunundaki istatistiklerin hiçbir zaman güncelleştirilmelerine gerek olmayabilir. Dağıtımın müşteriler arasında sabit olduğunu varsayarsak, tablo varyasyonuna yeni satırlar eklemek veri dağıtımını değiştirmez.

Ancak, veri ambarınız yalnızca bir ülke veya bölge içeriyorsa ve yeni bir ülkeden veya bölgeden veri getiriyorsanız, ülke veya bölge sütunundaki istatistikleri güncelleştirmeniz gerekir.

İstatistikleri güncelleyen öneriler şunlardır:

|||
|-|-|
| **İstatistik güncelleştirmelerinin sıklığı**  | Muhafazakar: Günlük </br> Verilerinizi yükledikten veya dönüştürdükten sonra |
| **Örnekleme** |  1 milyardan az satır, varsayılan örnekleme (yüzde 20) kullanın. </br> 1 milyardan fazla satırda, yüzde iki örnekleme kullanın. |

### <a name="determine-last-statistics-update"></a>Son istatistik güncelleştirmesi belirleme

Sorgusorunu giderirken sormanız gereken ilk sorulardan biri **şudur: "İstatistikler güncel mi?"**

Bu soru, verilerin yaşına göre cevaplanabilecek bir soru değildir. Temel verilerde herhangi bir maddi değişiklik yoksa, güncel bir istatistik nesnesi eski olabilir. Satır sayısı önemli ölçüde değiştiğinde veya bir sütun için değerlerin dağılımında önemli bir değişiklik meydana *geldiğinde,* istatistikleri güncelleştirme zamanı gelmiştir.

İstatistikler son kez güncelleştirilip güncelleştirilmediği nden beri tablo içindeki verilerin değişip değişmediğini belirlemek için dinamik bir yönetim görünümü yoktur. İstatistiklerinizin yaşını bilmek, resmin bir kısmını sağlayabilir. İstatistiklerinizin her tabloda en son ne zaman güncelleştirileni belirlemek için aşağıdaki sorguyu kullanabilirsiniz.

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

Örneğin, bir veri ambarındaki **tarih sütunları** genellikle sık sık istatistik güncelleştirmeleri gerekir. Veri ambarına her yeni satır yüklendiğinde, yeni yükleme tarihleri veya hareket tarihleri eklenir. Bu eklemeler veri dağıtımını değiştirir ve istatistikleri güncel hale getirin.

Müşteri tablosundaki cinsiyet sütunundaki istatistiklerin hiçbir zaman güncelleştirilmesi gerekmeyebilir. Dağıtımın müşteriler arasında sabit olduğunu varsayarsak, tablo varyasyonuna yeni satırlar eklemek veri dağıtımını değiştirmez.

Ancak, veri ambarınız yalnızca bir cinsiyet içeriyorsa ve birden çok cinsiyette yeni bir gereksinim sonuç alıyorsa, cinsiyet sütunundaki istatistikleri güncelleştirmeniz gerekir. Daha fazla bilgi için [İstatistik](/sql/relational-databases/statistics/statistics) makalesini inceleyin.

### <a name="implementing-statistics-management"></a>İstatistik yönetiminin uygulanması

İstatistiklerin yükün sonunda güncelleştirilmesini sağlamak için veri yükleme işlemini genişletmek genellikle iyi bir fikirdir. Veri yükü, tabloların en sık boyutlarını, değerlerin dağılımını veya her ikisini de değiştirdiği zamandır. Bu nedenle, yük işlemi bazı yönetim süreçlerini uygulamak için mantıklı bir yerdir.

Yükleme işlemi sırasında istatistiklerinizi güncelleştirmek için aşağıdaki kılavuz ilkeler sağlanır:

- Yüklenen her tabloda en az bir istatistik nesnesi güncelleştirdiğinden emin olun. Bu işlem, istatistik güncelleştirmesinin bir parçası olarak tablo boyutunu (satır sayısı ve sayfa sayısı) bilgilerini güncelleştirir.
- JOIN, GROUP BY, ORDER BY ve DISTINCT yan tümcelerine katılan sütunlara odaklanın.
- Bu değerler istatistik histogramına dahil olmayacağından, işlem tarihleri gibi "artan anahtar" sütunlarını daha sık güncelleştirmeyi düşünün.
- Statik dağıtım sütunlarını daha az sıklıkta güncelleştirmeyi düşünün.
- Unutmayın, her istatistik nesnesi sırayla güncelleştirilir. Özellikle çok `UPDATE STATISTICS <TABLE_NAME>` sayıda istatistik nesnesi olan geniş tablolar için, uygulama her zaman ideal değildir.

Daha fazla bilgi için [Kardinallik Tahmini'ne](/sql/relational-databases/performance/cardinality-estimation-sql-server)bakın.

### <a name="examples-create-statistics"></a>Örnekler: İstatistik oluşturma

Bu örnekler, istatistik oluşturmak için çeşitli seçeneklerin nasıl kullanılacağını gösterir. Her sütun için kullandığınız seçenekler, verilerinizin özelliklerine ve sütunun sorgularda nasıl kullanılacağına bağlıdır.

#### <a name="create-single-column-statistics-with-default-options"></a>Varsayılan seçenekleri olan tek sütunlu istatistikler oluşturma

Bir sütunda istatistik oluşturmak için, istatistik nesnesi ve sütunun adı için bir ad sağlayın.
Bu sözdizimi tüm varsayılan seçenekleri kullanır. Varsayılan olarak, SQL havuzu istatistik oluştururken tablonun **yüzde 20'sini** örnekler.

```sql
CREATE STATISTICS [statistics_name] ON [schema_name].[table_name]([column_name]);
```

Örneğin:

```sql
CREATE STATISTICS col1_stats ON dbo.table1 (col1);
```

#### <a name="create-single-column-statistics-by-examining-every-row"></a>Her satırı inceleyerek tek sütunlu istatistikler oluşturma

Yüzde 20'lik varsayılan örnekleme oranı çoğu durum için yeterlidir. Ancak, örnekleme hızını ayarlayabilirsiniz. Tam tabloyu örneklemek için şu sözdizimini kullanın:

```sql
CREATE STATISTICS [statistics_name] ON [schema_name].[table_name]([column_name]) WITH FULLSCAN;
```

Örneğin:

```sql
CREATE STATISTICS col1_stats ON dbo.table1 (col1) WITH FULLSCAN;
```

#### <a name="create-single-column-statistics-by-specifying-the-sample-size"></a>Örnek boyutunu belirterek tek sütunlu istatistikler oluşturma

Sahip olduğunuz başka bir seçenek, örnek boyutunu yüzde olarak belirtmektir:

```sql
CREATE STATISTICS col1_stats ON dbo.table1 (col1) WITH SAMPLE = 50 PERCENT;
```

#### <a name="create-single-column-statistics-on-only-some-of-the-rows"></a>Yalnızca bazı satırlarda tek sütunlu istatistikler oluşturma

Ayrıca, tablonuzdaki filtreuygulanmış istatistik olarak adlandırılan satırların bir bölümü için istatistikler de oluşturabilirsiniz.

Örneğin, büyük bölümlenmiş bir tablonun belirli bir bölümü sorgulamayı planlarken filtreuygulanmış istatistikleri kullanabilirsiniz. Yalnızca bölüm değerleri yle ilgili istatistikler oluşturarak, istatistiklerin doğruluğu artar. Ayrıca sorgu performansında bir iyileşme yaşarsınız.

Bu örnek, bir değer aralığı yla ilgili istatistikler oluşturur. Değerler, bir bölümdeki değer aralığıyla eşleşecek şekilde kolayca tanımlanabilir.

```sql
CREATE STATISTICS stats_col1 ON table1(col1) WHERE col1 > '2000101' AND col1 < '20001231';
```

> [!NOTE]
> Sorgu optimize edicinin dağıtılmış sorgu planını seçerken filtrelenmiş istatistikleri kullanmayı düşünemesi için, sorgunun istatistik nesnesinin tanımına sığması gerekir. Önceki örneği kullanarak, sorgunun WHERE yan tümcesinin 2000101 ile 20001231 arasında col1 değerlerini belirtmesi gerekir.

#### <a name="create-single-column-statistics-with-all-the-options"></a>Tüm seçenekleri içeren tek sütunlu istatistikler oluşturma

Seçenekleri bir araya da getirebilirsiniz. Aşağıdaki örnek, özel bir örnek boyutu ile filtreuygulanmış bir istatistik nesnesi oluşturur:

```sql
CREATE STATISTICS stats_col1 ON table1 (col1) WHERE col1 > '2000101' AND col1 < '20001231' WITH SAMPLE = 50 PERCENT;
```

Tam başvuru için, CREATE [STATISTICS'a](/sql/t-sql/statements/create-statistics-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)bakın.

#### <a name="create-multi-column-statistics"></a>Çok sütunlu istatistikler oluşturma

Çok sütunlu istatistik nesnesi oluşturmak için önceki örnekleri kullanın, ancak daha fazla sütun belirtin.

> [!NOTE]
> Sorgu sonucundaki satır sayısını tahmin etmek için kullanılan histogram, yalnızca istatistik nesnesi tanımında listelenen ilk sütun için kullanılabilir.

Bu örnekte, histogram *ürün\_kategorisindedir.* Çapraz sütun istatistikleri *ürün\_kategorisi* ve ürün *\_sub_category*hesaplanır:

```sql
CREATE STATISTICS stats_2cols ON table1 (product_category, product_sub_category) WHERE product_category > '2000101' AND product_category < '20001231' WITH SAMPLE = 50 PERCENT;
```

*Ürün\_kategorisi* ile *ürün\_\_alt kategorisi*arasında bir korelasyon olduğundan, bu sütunlara aynı anda erişilirse çok sütunlu istatistik nesnesi yararlı olabilir.

#### <a name="create-statistics-on-all-columns-in-a-table"></a>Tablodaki tüm sütunlarda istatistik oluşturma

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

#### <a name="use-a-stored-procedure-to-create-statistics-on-all-columns-in-a-database"></a>Veritabanındaki tüm sütunlarda istatistik oluşturmak için depolanmış yordamı kullanma

SQL havuzunda SQL Server'da sp_create_stats eşdeğer bir sistem depolanan yordamı yoktur. Bu depolanan yordam, veritabanının zaten istatistikleri olmayan her sütununda tek bir sütun istatistik nesnesi oluşturur.
Aşağıdaki örnek, veritabanı tasarımınıza başlamanıza yardımcı olacaktır. İhtiyaçlarınıza göre uyarlamakiçin çekinmeyin:

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

Tam scan kullanarak tablodaki tüm sütunlarda istatistik oluşturmak için şu yordamı arayın:

```sql
EXEC [dbo].[prc_sqldw_create_stats] 2, NULL;
```

Tablodaki tüm sütunlarda örneklenmiş istatistikler oluşturmak için 3 ve örnek yüzdesi girin. Aşağıdaki yordamyüzde 20 örnek oranı kullanır.

```sql
EXEC [dbo].[prc_sqldw_create_stats] 3, 20;
```

### <a name="examples-update-statistics"></a>Örnekler: İstatistikleri güncelleştir

İstatistikleri güncelleştirmek için şunları yapabilirsiniz:

- Bir istatistik nesnesini güncelleştirin. Güncelleştirmek istediğiniz istatistik nesnesinin adını belirtin.
- Tablodaki tüm istatistik nesnelerini güncelleştirin. Belirli bir istatistik nesnesi yerine tablonun adını belirtin.

#### <a name="update-one-specific-statistics-object"></a>Belirli bir istatistik nesnesini güncelleştirme

Belirli bir istatistik nesnesini güncelleştirmek için aşağıdaki sözdizimini kullanın:

```sql
UPDATE STATISTICS [schema_name].[table_name]([stat_name]);
```

Örneğin:

```sql
UPDATE STATISTICS [dbo].[table1] ([stats_col1]);
```

Belirli istatistik nesnelerini güncelleştirerek, istatistikleri yönetmek için gereken zamanı ve kaynakları en aza indirebilirsiniz. Bu eylem, güncelleştirmek için en iyi istatistik nesneleri seçmek için bazı düşünce gerektirir.

#### <a name="update-all-statistics-on-a-table"></a>Tablodaki tüm istatistikleri güncelleştirme

Tablodaki tüm istatistik nesnelerini güncelleştirmek için basit bir yöntem:

```sql
UPDATE STATISTICS [schema_name].[table_name];
```

Örneğin:

```sql
UPDATE STATISTICS dbo.table1;
```

UPDATE İstATİstİkler deyiminin kullanımı kolaydır. Yalnızca, gerekli olandan daha fazla iş isteyen, tablodaki *tüm* istatistikleri günceller unutmayın. Performans bir sorun değilse, bu yöntem istatistiklerin güncel olduğunu garanti etmenin en kolay ve en eksiksiz yoludur.

> [!NOTE]
> Bir tablodaki tüm istatistikleri güncellerken, SQL havuzu her istatistik nesnesi için tabloyu örneklemek için bir taramaya yapar. Tablo büyükse ve çok sayıda sütun ve çok sayıda istatistik varsa, tek tek istatistikleri ihtiyaca göre güncelleştirmek daha verimli olabilir.

Yordamın `UPDATE STATISTICS` uygulanması için [Geçici tablolara](develop-tables-temporary.md)bakın. Uygulama yöntemi önceki `CREATE STATISTICS` yordamdan biraz farklıdır, ancak sonuç aynıdır.
Tam sözdizimi için [istatistikleri güncelleştir'e](/sql/t-sql/statements/update-statistics-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)bakın.

### <a name="statistics-metadata"></a>İstatistikler meta verileri

İstatistikler hakkında bilgi bulmak için kullanabileceğiniz çeşitli sistem görünümleri ve işlevleri vardır. Örneğin, STATS_DATE() işlevini kullanarak bir istatistik nesnesinin güncel olup olmadığını görebilirsiniz. STATS_DATE() istatistiklerin en son ne zaman oluşturulduğunu veya güncelleştirilmelerini görmenizi sağlar.

#### <a name="catalog-views-for-statistics"></a>İstatistikler için katalog görünümleri

Bu sistem görünümleri istatistikler hakkında bilgi sağlar:

| Katalog görünümü | Açıklama |
|:--- |:--- |
| [Columns](/sql/relational-databases/system-catalog-views/sys-columns-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) |Her sütun için bir satır. |
| [Objects](/sql/relational-databases/system-catalog-views/sys-objects-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) |Veritabanındaki her nesne için bir satır. |
| [sys.şemalar](/sql/relational-databases/system-catalog-views/sys-objects-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) |Veritabanındaki her şema için bir satır. |
| [Stats](/sql/relational-databases/system-catalog-views/sys-stats-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) |Her istatistik nesnesi için bir satır. |
| [sys.stats_columns](/sql/relational-databases/system-catalog-views/sys-stats-columns-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) |İstatistik nesnesindeki her sütun için bir satır. Sys.columns'a geri bağlantılar. |
| [Tables](/sql/relational-databases/system-catalog-views/sys-tables-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) |Her tablo için bir satır (dış tabloları içerir). |
| [sys.table_types](/sql/relational-databases/system-catalog-views/sys-table-types-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) |Her veri türü için bir satır. |

#### <a name="system-functions-for-statistics"></a>İstatistikler için sistem fonksiyonları

Bu sistem işlevleri istatistiklerle çalışmak için yararlıdır:

| Sistem fonksiyonu | Açıklama |
|:--- |:--- |
| [STATS_DATE](/sql/t-sql/functions/stats-date-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) |İstatistik nesnesinin en son güncelleştirilen tarih. |
| [DBCC SHOW_STATISTICS](/sql/t-sql/database-console-commands/dbcc-show-statistics-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) |İstatistik nesnesi tarafından anlaşıldığı gibi değerlerin dağılımı hakkında özet düzeyi ve ayrıntılı bilgi. |

#### <a name="combine-statistics-columns-and-functions-into-one-view"></a>İstatistik sütunlarını ve işlevlerini tek bir görünümde birleştirme

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

### <a name="dbcc-show_statistics-examples"></a>DBCC SHOW_STATISTICS() örnekleri

DBCC SHOW_STATISTICS() bir istatistik nesnesi içinde tutulan verileri gösterir. Bu veriler üç bölümden oluşur:

- Üst bilgi
- Yoğunluk vektörü
- Histogram

Üstbilgi, istatistiklerle ilgili meta verilerdir. Histogram, istatistik nesnesinin ilk anahtar sütunundaki değerlerin dağılımını görüntüler. Yoğunluk vektörü çapraz sütun bağıntısını ölçer. SQL havuzu, istatistik nesnesindeki verilerden herhangi biriyle kardinallik tahminlerini hesaplar.

#### <a name="show-header-density-and-histogram"></a>Üstbilgi, yoğunluk ve histogramı göster

Bu basit örnek, istatistik nesnesinin üç bölümünü de gösterir:

```sql
DBCC SHOW_STATISTICS([<schema_name>.<table_name>],<stats_name>)
```

Örneğin:

```sql
DBCC SHOW_STATISTICS (dbo.table1, stats_col1);
```

#### <a name="show-one-or-more-parts-of-dbcc-show_statistics"></a>DBCC SHOW_STATISTICS bir veya daha fazla bölümünü göster()

Yalnızca belirli bölümleri görüntülemekistiyorsanız, yan tümceyi `WITH` kullanın ve hangi bölümleri görmek istediğinizi belirtin:

```sql
DBCC SHOW_STATISTICS([<schema_name>.<table_name>],<stats_name>) WITH stat_header, histogram, density_vector
```

Örneğin:

```sql
DBCC SHOW_STATISTICS (dbo.table1, stats_col1) WITH histogram, density_vector
```

### <a name="dbcc-show_statistics-differences"></a>DBCC SHOW_STATISTICS() farkları

DBCC SHOW_STATISTICS() SQL Server'a göre SQL havuzunda daha sıkı bir şekilde uygulanır:

- Belgelenmemiş özellikler desteklenmez.
- Stats_stream kullanamam.
- Belirli istatistik veri alt kümeleri için sonuçlara katıolamaz. Örneğin, DENSITY_VECTOR join STAT_HEADER.
- NO_INFOMSGS mesaj bastırma için ayarlanamadı.
- İstatistik adlarının etrafındaki kare ayraçlar kullanılamaz.
- İstatistik nesnelerini tanımlamak için sütun adlarını kullanamazsınız.
- Özel hata 2767 desteklenmez.

### <a name="next-steps"></a>Sonraki adımlar

Sorgu performansını daha da artırmak için [bkz.](../sql-data-warehouse/sql-data-warehouse-manage-monitor.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)

## <a name="statistics-in-sql-on-demand-preview"></a>İsteğe bağlı SQL istatistikleri (önizleme)

İstatistikler belirli veri kümesi (depolama yolu) için belirli bir sütun başına oluşturulur.

### <a name="why-use-statistics"></a>İstatistikleri neden kullanır?

İsteğe bağlı SQL (önizleme) verileriniz hakkında ne kadar çok şey bilirse, karşı sorguları o kadar hızlı yürütebilir. Verilerinizle ilgili istatistikleri toplamak, sorgularınızı en iyi duruma getirmek için yapabileceğiniz en önemli şeylerden biridir. SQL isteğe bağlı sorgu optimize edici maliyet tabanlı bir optimizerdir. Çeşitli sorgu planlarının maliyetini karşılaştırır ve ardından planı en düşük maliyetle seçer. Çoğu durumda, en hızlı yürütecek planı seçer. Örneğin, en iyi duruma getirici, sorgunuzun filtrelenediğini tahmin ederse bir satır döndürür ve bir plan seçer. Seçili tarihin 1 milyon satır döndüreceğini tahmin ederse, farklı bir plan döndürecektir.

### <a name="automatic-creation-of-statistics"></a>İstatistiklerin otomatik oluşturulması

SQL isteğe bağlı, eksik istatistikler için gelen kullanıcı sorgularını analiz eder. İstatistikler eksikse, sorgu optimize edici, sorgu planının önemli lik tahminlerini iyileştirmek için sorgu yüklemindeki tek tek sütunlar veya birleştirme koşulu yla ilgili istatistikler oluşturur.

SELECT deyimi istatistiklerin otomatik oluşturulmasını tetikler.

> [!NOTE]
> Parke dosyaları için istatistiklerin otomatik olarak oluşturulması açık. CSV dosyaları için, CSV dosyaları istatistiklerinin otomatik olarak oluşturulması desteklenene kadar istatistikleri el ile oluşturmanız gerekir.

İstatistiklerin otomatik olarak oluşturulması eşzamanlı olarak yapılır, bu nedenle sütunlarınız istatistikleri eksikse biraz bozulmuş sorgu performansına maruz kalabilirsiniz. Tek bir sütun için istatistik oluşturma zamanı, hedeflenen dosyaların boyutuna bağlıdır.

### <a name="manual-creation-of-statistics"></a>İstatistiklerin manuel oluşturulması

İsteğe bağlı SQL, istatistikleri el ile oluşturmanıza olanak tanır. CSV dosyaları için, csv dosyaları için otomatik istatistik oluşturma açık olmadığından, istatistiklerin el ile oluşturulması gerekir. İstatistiklerin el ile nasıl oluşturulacağına ilişkin talimatlar için aşağıdaki örneklere bakın.

### <a name="updating-statistics"></a>İstatistikleri güncelleştirme

Dosyalardaki verilerde yapılan değişiklikler, silme ve dosya ekleme, veri dağıtımında değişikliklere neden olabilir ve istatistikleri güncel hale getirir. Bu durumda, istatistiklerin güncelleştirilmesi gerekir.

Veriler önemli ölçüde değiştirilirse, isteğe bağlı SQL istatistikleri otomatik olarak yeniden oluşturur. İstatistikler her otomatik olarak oluşturulduğunda, veri kümesinin geçerli durumu da kaydedilir: dosya yolları, boyutlar, son değişiklik tarihleri.

İstatistikler bayat olduğunda, yenileri oluşturulur. Algoritma verileri gözden geçirir ve veri kümesinin geçerli durumuyla karşılaştırır. Değişikliklerin boyutu belirli eşiğe göre büyükse, eski istatistikler silinir ve yeni veri kümesi üzerinde yeniden oluşturulur.

Manuel istatistikler asla bayat olarak ilan edilmemiştir.

> [!NOTE]
> Parke dosyaları için istatistiklerin otomatik olarak dinlenmesi açılır. CSV dosyaları için, CSV dosyaları istatistiklerinin otomatik olarak oluşturulması desteklenene kadar istatistikleri el ile bırakmanız ve oluşturmanız gerekir. Nasıl bırakıp istatistik oluşturabilirsiniz ile ilgili aşağıdaki örnekleri kontrol edin.

Sorgusorunu giderirken sormanız gereken ilk sorulardan biri **şudur: "İstatistikler güncel mi?"**

Satır sayısı önemli ölçüde değiştiğinde veya bir sütuniçin değerlerin dağılımında önemli bir değişiklik *olduğunda,* istatistikleri güncelleştirme zamanı gelmiştir.

> [!NOTE]
> Bir sütuniçin değerlerin dağılımında önemli bir değişiklik varsa, istatistikleri en son ne zaman güncelleştirildiklerine bakılmaksızın güncelleştirmeniz gerekir.

### <a name="implementing-statistics-management"></a>İstatistik yönetiminin uygulanması

Veri ekleme, silme veya dosya değişikliği yoluyla önemli ölçüde değiştirildiğinde istatistiklerin güncelleştirildiğinden emin olmak için veri ardınız

İstatistiklerinizi güncelleştirmek için aşağıdaki kılavuz ilkeler sağlanmıştır:

- Veri kümesinin en az bir istatistik nesnesi güncelleştirdiğinden emin olun. Bu güncelleştirmeboyutu (satır sayısı ve sayfa sayısı) bilgileri istatistik güncelleştirmesinin bir parçası olarak.
- JOIN, GROUP BY, ORDER BY ve DISTINCT yan tümcelerine katılan sütunlara odaklanın.
- Bu değerler histogram istatistiklerine dahil olmayacağından işlem tarihleri gibi "artan anahtar" sütunlarını daha sık güncelleştirin.
- Statik dağıtım sütunlarını daha az sıklıkta güncelleştirin.

Daha fazla bilgi için [Kardinallik Tahmini'ne](/sql/relational-databases/performance/cardinality-estimation-sql-server)bakın.

### <a name="examples-create-statistics-for-column-in-openrowset-path"></a>Örnekler: OPENROWSET yolundasütun için istatistik oluşturma

Aşağıdaki örnekler, istatistik oluşturmak için çeşitli seçenekleri nasıl kullanacağınızı gösterir. Her sütun için kullandığınız seçenekler, verilerinizin özelliklerine ve sütunun sorgularda nasıl kullanılacağına bağlıdır.

> [!NOTE]
> Yalnızca şu anda tek sütunlu istatistikler oluşturabilirsiniz.

İstatistik oluşturmak için aşağıdaki depolanan yordam kullanılır:

```sql
sys.sp_create_file_statistics [ @stmt = ] N'statement_text'
```

Bağımsız değişkenler: [ @stmt = ] N'statement_text' - İstatistikler için kullanılacak sütun değerlerini döndürecek bir Transact-SQL deyimi belirtir. Kullanılacak veri örneklerini belirtmek için TABLESAMPLE'ı kullanabilirsiniz. TABLESAMPLE belirtilmemişse FULLSCAN kullanılır.

```sql
<tablesample_clause> ::= TABLESAMPLE ( sample_number PERCENT )
```

> [!NOTE]
> CSV örneklemesi şu anda çalışmıyor, csv için yalnızca FULLSCAN desteklenir.

#### <a name="create-single-column-statistics-by-examining-every-row"></a>Her satırı inceleyerek tek sütunlu istatistikler oluşturma

Sütunda istatistik oluşturmak için, istatistiklere ihtiyaç duyduğunuz sütunu döndüren bir sorgu sağlayın.

Varsayılan olarak, aksini belirtmezseniz, SQL isteğe bağlı olarak istatistik oluştururken veri kümesinde sağlanan verilerin %100'ünü kullanır.

Örneğin, population.csv dosyasını temel alan veri kümesinin bir yıllık sütunu için varsayılan seçeneklere (FULLSCAN) sahip istatistikler oluşturmak için:

```sql
/* make sure you have credentials for storage account access created
IF EXISTS (SELECT * FROM sys.credentials WHERE name = 'https://azureopendatastorage.blob.core.windows.net/censusdatacontainer')
DROP CREDENTIAL [https://azureopendatastorage.blob.core.windows.net/censusdatacontainer]
GO

CREATE CREDENTIAL [https://azureopendatastorage.blob.core.windows.net/censusdatacontainer]  
WITH IDENTITY='SHARED ACCESS SIGNATURE',  
SECRET = ''
GO
*/

EXEC sys.sp_create_file_statistics N'SELECT year
FROM OPENROWSET(
        BULK ''https://sqlondemandstorage.blob.core.windows.net/csv/population/population.csv'',
        FORMAT = ''CSV'',
        FIELDTERMINATOR ='','',
        ROWTERMINATOR = ''\n''
    )
WITH (
    [country_code] VARCHAR (5) COLLATE Latin1_General_BIN2,
    [country_name] VARCHAR (100) COLLATE Latin1_General_BIN2,
    [year] smallint,
    [population] bigint
) AS [r]
'
```

#### <a name="create-single-column-statistics-by-specifying-the-sample-size"></a>Örnek boyutunu belirterek tek sütunlu istatistikler oluşturma

Örnek boyutunu yüzde olarak belirtebilirsiniz:

```sql
/* make sure you have credentials for storage account access created
IF EXISTS (SELECT * FROM sys.credentials WHERE name = 'https://azureopendatastorage.blob.core.windows.net/censusdatacontainer')
DROP CREDENTIAL [https://azureopendatastorage.blob.core.windows.net/censusdatacontainer]
GO

CREATE CREDENTIAL [https://azureopendatastorage.blob.core.windows.net/censusdatacontainer]  
WITH IDENTITY='SHARED ACCESS SIGNATURE',  
SECRET = ''
GO
*/

EXEC sys.sp_create_file_statistics N'SELECT payment_type
FROM OPENROWSET(
        BULK ''https://sqlondemandstorage.blob.core.windows.net/parquet/taxi/year=2018/month=6/*.parquet'',
         FORMAT = ''PARQUET''
    ) AS [nyc]
    TABLESAMPLE(5 PERCENT)
'
```

### <a name="examples-update-statistics"></a>Örnekler: İstatistikleri güncelleştir

İstatistikleri güncelleştirmek için, istatistikleri bırakmanız ve oluşturmanız gerekir. İstatistikleri bırakmak için aşağıdaki depolanan yordam kullanılır:

```sql
sys.sp_drop_file_statistics [ @stmt = ] N'statement_text'
```

Bağımsız değişkenler: [ @stmt = ] N'statement_text' - İstatistikler oluşturulduğunda kullanılan aynı Transact-SQL deyimini belirtir.

population.csv dosyasını temel alan veri kümesindeki yıl sütununa ilişkin istatistikleri güncelleştirmek için, istatistikleri bırakmanız ve oluşturmanız gerekir:

```sql
EXEC sys.sp_drop_file_statistics N'SELECT payment_type
FROM OPENROWSET(
        BULK ''https://sqlondemandstorage.blob.core.windows.net/parquet/taxi/year=2018/month=6/*.parquet'',
         FORMAT = ''PARQUET''
    ) AS [nyc]
    TABLESAMPLE(5 PERCENT)
'
GO

/* make sure you have credentials for storage account access created
IF EXISTS (SELECT * FROM sys.credentials WHERE name = 'https://azureopendatastorage.blob.core.windows.net/censusdatacontainer')
DROP CREDENTIAL [https://azureopendatastorage.blob.core.windows.net/censusdatacontainer]
GO

CREATE CREDENTIAL [https://azureopendatastorage.blob.core.windows.net/censusdatacontainer]  
WITH IDENTITY='SHARED ACCESS SIGNATURE',  
SECRET = ''
GO
*/

EXEC sys.sp_create_file_statistics N'SELECT payment_type
FROM OPENROWSET(
        BULK ''https://sqlondemandstorage.blob.core.windows.net/parquet/taxi/year=2018/month=6/*.parquet'',
         FORMAT = ''PARQUET''
    ) AS [nyc]
    TABLESAMPLE(5 PERCENT)
'
```

### <a name="examples-create-statistics-for-external-table-column"></a>Örnekler: Dış tablo sütunu için istatistik oluşturma

Aşağıdaki örnekler, istatistik oluşturmak için çeşitli seçenekleri nasıl kullanacağınızı gösterir. Her sütun için kullandığınız seçenekler, verilerinizin özelliklerine ve sütunun sorgularda nasıl kullanılacağına bağlıdır.

> [!NOTE]
> Yalnızca şu anda tek sütunlu istatistikler oluşturabilirsiniz.

Bir sütunda istatistik oluşturmak için, istatistik nesnesi ve sütunun adı için bir ad sağlayın.

```sql
CREATE STATISTICS statistics_name
ON { external_table } ( column )
    WITH
        { FULLSCAN
          | [ SAMPLE number PERCENT ] }
        , { NORECOMPUTE }
```

Bağımsız değişkenler: external_table İstatistiklerin oluşturulması gerektiğini dış tablo belirtir.

FULLSCAN Tüm satırları tarayarak istatistikleri hesaplama. FULLSCAN ve ÖRNEK YÜZDE 100 aynı sonuçlara sahiptir. FULLSCAN ÖRNEK seçeneği ile kullanılamaz.

ÖRNEK numarası YÜZDE Tablodaki yaklaşık yüzde veya satır sayısını veya istatistik oluştururken kullanılacak sorgu optimize ediciiçin dizine görünümü belirtir. Sayı 0'dan 100'e kadar olabilir.

FULLSCAN seçeneği ile ÖRNEK kullanılamaz.

> [!NOTE]
> CSV örneklemesi şu anda çalışmıyor, csv için yalnızca FULLSCAN desteklenir.

#### <a name="create-single-column-statistics-by-examining-every-row"></a>Her satırı inceleyerek tek sütunlu istatistikler oluşturma

```sql
CREATE STATISTICS sState on census_external_table (STATENAME) WITH FULLSCAN, NORECOMPUTE
```

#### <a name="create-single-column-statistics-by-specifying-the-sample-size"></a>Örnek boyutunu belirterek tek sütunlu istatistikler oluşturma

```sql
-- following sample creates statistics with sampling 20%
CREATE STATISTICS sState on census_external_table (STATENAME) WITH SAMPLE 5 percent, NORECOMPUTE
```

### <a name="examples-update-statistics"></a>Örnekler: İstatistikleri güncelleştir

İstatistikleri güncelleştirmek için, istatistikleri bırakmanız ve oluşturmanız gerekir. Önce istatistikleri bırak:

```sql
DROP STATISTICS census_external_table.sState
```

Ve istatistik ler oluşturun:

```sql
CREATE STATISTICS sState on census_external_table (STATENAME) WITH FULLSCAN, NORECOMPUTE
```

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla sorgu performansı geliştirmeleri [için SQL havuzu için en iyi uygulamalara](best-practices-sql-pool.md#maintain-statistics)bakın.
