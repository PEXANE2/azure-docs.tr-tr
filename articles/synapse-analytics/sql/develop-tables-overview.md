---
title: Synapse SQL kullanarak tablo ları tasarlama
description: Synapse SQL'de tablo tasarlamaya giriş.
services: synapse-analytics
author: filippopovic
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/15/2020
ms.author: fipopovi
ms.reviewer: jrasnick
ms.openlocfilehash: f1eec76d92edc97f7e4058d3afe813f0bb2aae47
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81431884"
---
# <a name="design-tables-using-synapse-sql"></a>Synapse SQL kullanarak tablo ları tasarlama

Bu belge, SQL havuzu ve isteğe bağlı SQL (önizleme) ile tablolar tasarlamak için anahtar kavramları içerir.  

[SQL isteğe bağlı (önizleme),](on-demand-workspace-overview.md) veri gölünüzdeki veriler üzerinde bir sorgu hizmetidir. Veri alımı için yerel depolama alanı yoktur. [SQL havuzu,](best-practices-sql-pool.md) Synapse SQL kullanırken sağlanan analitik kaynakların bir koleksiyonunu temsil eder. SQL havuzunun boyutu Veri Depolama Birimleri (DWU) tarafından belirlenir.

Aşağıdaki tabloda SQL havuzu ile isteğe bağlı SQL ile ilgili konular listelenir:

| Konu başlığı                                                        | SQL havuzu | İsteğe bağlı SQL |
| ------------------------------------------------------------ | ------------------ | ----------------------- |
| [Tablo kategorilerini belirleme](#determine-table-category)        | Evet                | Hayır                      |
| [Şema isimleri](#schema-names)                                | Evet                | Evet                     |
| [Tablo adları](#table-names)                                  | Evet                | Hayır                      |
| [Tablo kalıcılığı](#table-persistence)                      | Evet                | Hayır                      |
| [Normal tablo](#regular-table)                              | Evet                | Hayır                      |
| [Geçici tablo](#temporary-table)                          | Evet                | Evet                     |
| [Dış tablo](#external-table)                            | Evet                | Evet                     |
| [Veri türleri](#data-types)                                    | Evet                | Evet                     |
| [Dağıtılmış tablolar](#distributed-tables)                    | Evet                | Hayır                      |
| [Karma dağıtılmış tablolar](#hash-distributed-tables)          | Evet                | Hayır                      |
| [Çoğaltılmış tablolar](#replicated-tables)                      | Evet                | Hayır                      |
| [Yuvarlak-robin tablolar](#round-robin-tables)                    | Evet                | Hayır                      |
| [Tablolar için ortak dağıtım yöntemleri](#common-distribution-methods-for-tables) | Evet                | Hayır                      |
| [Bölümler](#partitions)                                    | Evet                | Evet                     |
| [Columnstore dizinleri](#columnstore-indexes)                  | Evet                | Hayır                      |
| [İstatistikler](#statistics)                                    | Evet                | Evet                     |
| [Birincil anahtar ve benzersiz anahtar](#primary-key-and-unique-key)    | Evet                | Hayır                      |
| [Tablo oluşturma komutları](#commands-for-creating-tables) | Evet                | Hayır                      |
| [Kaynak verilerini veri ambarıyla hizalama](#aligning-source-data-with-the-data-warehouse) | Evet                | Hayır                      |
| [Desteklenmeyen tablo özellikleri](#unsupported-table-features)    | Evet                | Hayır                      |
| [Tablo boyutu sorguları](#table-size-queries)                    | Evet                | Hayır                      |

## <a name="determine-table-category"></a>Tablo kategorilerini belirleme

Bir [yıldız şeması](https://en.wikipedia.org/wiki/Star_schema) verileri olgu ve boyut tabloları na göre düzenler. Bazı tablolar, bir olgu veya boyut tablosuna geçmeden önce tümleştirme veya veri hazırlama için kullanılır. Bir tablo tasarlarken, tablo verilerinin bir gerçeğe, boyuta mı yoksa tümleştirme tablosuna mı ait olduğuna karar verin. Bu karar uygun tablo yapısını ve dağılımını bildirir.

- **Olgu tabloları,** genellikle bir işlem sisteminde oluşturulan ve daha sonra veri ambarına yüklenen nicel verileri içerir. Örneğin, bir perakende işletmesi her gün satış hareketleri oluşturur ve verileri çözümleme için veri ambarı durum tablosuna yükler.

- **Boyut tabloları** değişebilir ama genellikle seyrek olarak değişebilecek öznitelik verileri içerir. Örneğin, bir müşterinin adı ve adresi bir boyut tablosunda depolanır ve yalnızca müşterinin profili değiştiğinde güncelleştirilir. Büyük bir olgu tablosunun boyutunu en aza indirmek için, müşterinin adı ve adresinin bir olgu tablosunun her satırında olması gerekmez. Bunun yerine, olgu tablosu ve boyut tablosu bir müşteri kimliğini paylaşabilir. Bir sorgu, müşterinin profilini ve hareketlerini ilişkilendirmek için iki tabloyu biraraya getirebilir.

- **Tümleştirme tabloları,** verileri tümleştirme veya hazırlama için bir yer sağlar. Normal tablo, dış tablo veya geçici tablo olarak bir tümleştirme tablosu oluşturabilirsiniz. Örneğin, verileri bir hazırlama tablosuna yükleyebilir, evrelemedeki veriler üzerinde dönüşümler gerçekleştirebilir ve sonra verileri üretim tablosuna ekleyebilirsiniz.

## <a name="schema-names"></a>Şema isimleri

Şemalar, benzer şekilde kullanılan nesneleri bir araya getirmek için iyi bir yoldur. Aşağıdaki kod, wwi adı verilen [kullanıcı tanımlı](/sql/t-sql/statements/create-schema-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) bir şema oluşturur.

```sql
CREATE SCHEMA wwi;
```

## <a name="table-names"></a>Tablo adları

Bir ön hazırlık çözümünden SQL havuzuna birden çok veritabanı taşıyorsanız, en iyi yöntem tüm gerçeği, boyutu ve tümleştirme tablolarını tek bir SQL havuzu şemasına geçirmektir. Örneğin, [wideworldimportersDW](/sql/samples/wide-world-importers-dw-database-catalog?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) örnek veri ambarındaki tüm tabloları wwi adı verilen bir şema içinde depolayabilirsiniz.

SQL havuzundaki tabloların organizasyonunu göstermek için tablo adlarına önek olarak olgu, dim ve int kullanabilirsiniz. Aşağıdaki tablo, WideWorldImportersDW için şema ve tablo adlarından bazılarını göstermektedir.  

| WideWorldImportersDW tablosu  | Tablo türü | SQL havuzu |
|:-----|:-----|:------|:-----|
| Şehir | Boyut | Wwı. DimCity |
| Sipariş verme | Fact | Wwı. FactOrder |

## <a name="table-persistence"></a>Tablo kalıcılığı

Tablolar verileri kalıcı olarak Azure Depolama'da, geçici olarak Azure Depolama'da veya veri ambarı dışındaki bir veri deposunda depolar.

### <a name="regular-table"></a>Normal tablo

Normal bir tablo, verileri veri ambarının bir parçası olarak Azure Depolama'da depolar. Tablo ve veriler bir oturumun açık olup olmadığı devam ediyor.  Aşağıdaki örnekte iki sütunlu normal bir tablo oluşturulur.

```sql
CREATE TABLE MyTable (col1 int, col2 int );  
```

### <a name="temporary-table"></a>Geçici tablo

Geçici bir tablo yalnızca oturum süresince vardır. Diğer kullanıcıların geçici sonuçları görmesini önlemek için geçici bir tablo kullanabilirsiniz. Geçici tablolar kullanmak da temizleme gereksinimini azaltır.  Geçici tablolar yerel depolama kullanır ve SQL havuzunda daha hızlı performans sunabilir.  

SQL isteğe bağlı geçici tabloları destekler. Ancak, geçici tablodan seçim yapabileceğiniz, ancak depolama daki dosyalarla birleştiremediğiniz için kullanımı sınırlıdır.

Daha fazla bilgi için [Geçici tablolara](develop-tables-temporary.md)bakın.

### <a name="external-table"></a>Dış tablo

[Dış tablolar,](develop-tables-external-tables.md) Azure Depolama blob'unda veya Azure Veri Gölü Depolama'da bulunan verilere işaret ediyor.

[CREATE TABLE AS SELECT](../sql-data-warehouse/sql-data-warehouse-develop-ctas.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) deyimini kullanarak dış tablolardaki verileri SQL havuzuna aktarın. Yükleme eğitimi için Azure [blob depolamasından veri yüklemek için PolyBase'i kullanın'a](../sql-data-warehouse/load-data-from-azure-blob-storage-using-polybase.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)bakın.

İsteğe bağlı SQL için, sorgu sonucunu Azure Depolama'daki harici bir tabloya kaydetmek için [CETAS'ı](develop-tables-cetas.md) kullanabilirsiniz.

## <a name="data-types"></a>Veri türleri

SQL havuzu en sık kullanılan veri türlerini destekler. Desteklenen veri türlerinin listesi için CREATE TABLE [deyimindeki CREATE TABLE başvurusundaki veri türlerine](/sql/t-sql/statements/create-table-azure-sql-data-warehouse?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest#DataTypes) bakın. Veri türlerini kullanma hakkında daha fazla bilgi için [Bkz. Veri türleri.](../sql/develop-tables-data-types.md)

## <a name="distributed-tables"></a>Dağıtılmış tablolar

SQL havuzunun temel bir özelliği, [dağıtımlar](../sql-data-warehouse/massively-parallel-processing-mpp-architecture.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json#distributions)arasında tablolarda depolayabildiği ve çalıştırabildiği yoldur.  SQL havuzu veri dağıtmak için üç yöntemi destekler:

- Hepsini bir kez deneme (varsayılan)
- Karma
- Çoğaltılmış

### <a name="hash-distributed-tables"></a>Karma dağıtılmış tablolar

Karma dağıtılmış tablo, dağıtım sütunundaki değeri temel alan satırları dağıtır. Karma dağıtılmış tablo, büyük tablolardaki sorgular için yüksek performans elde etmek üzere tasarlanmıştır. Bir dağıtım sütunu seçerken göz önünde bulundurulması gereken çeşitli etkenler vardır.

Daha fazla bilgi [için dağıtılan tablolar için Tasarım kılavuzuna](../sql-data-warehouse/sql-data-warehouse-tables-distribute.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)bakın.

### <a name="replicated-tables"></a>Çoğaltılmış tablolar

Çoğaltılan tablo, her İşlem düğümünde kullanılabilir tablonun tam kopyasına sahiptir. Yinelenen tablolarda birleşenler veri hareketi gerektirmediği için sorgular çoğaltılan tablolarda hızlı çalışır. Çoğaltma, ancak ekstra depolama gerektirir ve büyük tablolar için pratik değildir.

Daha fazla bilgi için, [çoğaltılan tablolar için Tasarım kılavuzuna](../sql-data-warehouse/design-guidance-for-replicated-tables.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)bakın.

### <a name="round-robin-tables"></a>Yuvarlak-robin tablolar

Yuvarlak robin tablosu, tablo satırlarını tüm dağıtımlara eşit olarak dağıtır. Satırlar rasgele dağıtılır. Verileri yuvarlak-robin tablosuna yüklemek hızlıdır.  Ancak, sorgular diğer dağıtım yöntemlerine göre daha fazla veri hareketi gerektirebilir.

Daha fazla bilgi [için dağıtılan tablolar için Tasarım kılavuzuna](../sql-data-warehouse/sql-data-warehouse-tables-distribute.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)bakın.

### <a name="common-distribution-methods-for-tables"></a>Tablolar için ortak dağıtım yöntemleri

Tablo kategorisi genellikle tablo dağıtımı için en uygun seçeneği belirler.

| Tablo kategorisi | Önerilen dağıtım seçeneği |
|:---------------|:--------------------|
| Fact           | Kümelenmiş sütun deposu dizini ile karma dağılım kullanın. Aynı dağıtım sütununda iki karma tablo birleştirildiğinde performans artar. |
| Boyut      | Daha küçük tablolar için çoğaltılmış kullanın. Tablolar her İşlemçi düğümünde depolanamayacak kadar büyükse, karma dağıtılmış kullanın. |
| Hazırlama        | Evreleme tablosu için yuvarlak-robin kullanın. CTAS ile yük hızlıdır. Veriler evreleme tablosuna girdiğinde INSERT'i kullanın... Verileri üretim tablolarına taşımak için SEÇIN. |

## <a name="partitions"></a>Bölümler

SQL havuzunda, bölümlenmiş bir tablo veri aralıklarına göre tablo satırlarındaki işlemleri depolar ve yürütür. Örneğin, bir tablo güne, aya veya yıla göre bölümlenebilir. Sorgu talanını bölüm içindeki verilerle sınırlayan bölüm eliminasyonu yoluyla sorgu performansını artırabilirsiniz.

Ayrıca, bölüm geçişi yoluyla verileri koruyabilirsiniz. SQL havuzundaki veriler zaten dağıtıldığından, çok fazla bölüm sorgu performansını yavaşlatabilir. Daha fazla bilgi için [Bölümleme kılavuzuna](../sql-data-warehouse/sql-data-warehouse-tables-partition.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)bakın.  

> [!TIP]
> Bölüm boş olmayan tablo bölümlerine geçerken, varolan veriler kesilirse [ALTER TABLE](/sql/t-sql/statements/alter-table-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) ekstrenizdeki TRUNCATE_TARGET seçeneğini kullanmayı düşünün.

Aşağıdaki kod, dönüştürülmüş günlük verileri SalesFact bölümüne dönüştürür ve varolan verilerin üzerine yazar.

```sql
ALTER TABLE SalesFact_DailyFinalLoad SWITCH PARTITION 256 TO SalesFact PARTITION 256 WITH (TRUNCATE_TARGET = ON);  
```

İsteğe bağlı SQL'de, sorgunuz tarafından okunacak dosyaları/klasörleri (bölümler) sınırlayabilirsiniz. Yol tarafından bölümleme, [depolama dosyalarını sorgulamada](develop-storage-files-overview.md)açıklanan dosya yolu ve dosya bilgisi işlevleri kullanılarak desteklenir. Aşağıdaki örnekte, 2017 yılı için veri içeren bir klasör okur:

```sql
SELECT
    nyc.filepath(1) AS [year],
    payment_type,
    SUM(fare_amount) AS fare_total
FROM  
    OPENROWSET(
        BULK 'https://sqlondemandstorage.blob.core.windows.net/parquet/taxi/year=*/month=*/*.parquet',
        FORMAT='PARQUET'
    ) AS nyc
WHERE
    nyc.filepath(1) = 2017
GROUP BY
    nyc.filepath(1),
    payment_type
ORDER BY
    nyc.filepath(1),
    payment_type
```

## <a name="columnstore-indexes"></a>Columnstore dizinleri

Varsayılan olarak, SQL havuzu bir tabloyu kümelenmiş sütun deposu dizini olarak depolar. Bu veri depolama biçimi, büyük tablolarda yüksek veri sıkıştırma ve sorgu performansı sağlar.  Kümelenmiş sütun deposu dizini genellikle en iyi seçimdir, ancak bazı durumlarda kümelenmiş dizin veya yığın uygun depolama yapısıdır.  

> [!TIP]
> Yığın tablo, özellikle son tabloya dönüştürülen evreleme tablosu gibi geçici verileri yüklemek için yararlı olabilir.

Sütun mağazası özelliklerinin listesi [için, sütun mağazası dizinleri için yeniliklere](/sql/relational-databases/indexes/columnstore-indexes-what-s-new?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)bakın. Sütun deposu dizini performansını artırmak [için, sütun deposu dizinleri için satır grubu kalitesini en üst düzeye çıkarma'ya](../sql/data-load-columnstore-compression.md)bakın.

## <a name="statistics"></a>İstatistikler


Sorgu en iyi duruma getirici, sorguyu yürütme planı oluştururken sütun düzeyindeki istatistikleri kullanır. Sorgu performansını artırmak için, tek tek sütunlar, özellikle sorgu birleştirmelerinde kullanılan sütunlar istatistikleri olması önemlidir. Synapse SQL istatistiklerin otomatik oluşturulmasını destekler. 

İstatistiksel güncelleme otomatik olarak gerçekleşmez. Önemli sayıda satır eklendikten veya değiştirildikten sonra istatistikleri güncelleştirin. Örneğin, bir yüklemeden sonra istatistikleri güncelleştirin. [İstatistikler kılavuz](develop-tables-statistics.md) makalesinde ek bilgiler verilmiştir.

## <a name="primary-key-and-unique-key"></a>Birincil anahtar ve benzersiz anahtar

BIRINCIL ANAHTAR yalnızca CLUSTERED ve NOT ENFORCED her ikisi de kullanıldığında desteklenir.  BENZERSIZ kısıtlama yalnızca UYGULANMADıĞıNDA desteklenir.  Daha fazla bilgi için [SQL havuzu Tablo Kısıtlamaları](../sql-data-warehouse/sql-data-warehouse-table-constraints.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) makalesine bakın.

## <a name="commands-for-creating-tables"></a>Tablo oluşturma komutları

Yeni boş tablo olarak bir tablo oluşturabilirsiniz. Ayrıca, seçili bir deyimin sonuçlarıyla bir tablo oluşturabilir ve doldurabilirsiniz. Tablo oluşturmak için T-SQL komutları aşağıda veda edileb'dir.

| T-SQL Bildirimi | Açıklama |
|:----------------|:------------|
| [TABLO OLUŞTUR](/sql/t-sql/statements/create-table-azure-sql-data-warehouse?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) | Tüm tablo sütunlarını ve seçeneklerini tanımlayarak boş bir tablo oluşturur. |
| [DıŞ TABLO OLUŞTURMA](/sql/t-sql/statements/create-external-table-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) | Harici bir tablo oluşturur. Tablonun tanımı SQL havuzunda depolanır. Tablo verileri Azure Blob depolama veya Azure Veri Gölü Depolama'da depolanır. |
| [CREATE TABLE AS SELECT](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) | Seçili bir deyimin sonuçlarıyla birlikte yeni bir tabloyu doldurur. Tablo sütunları ve veri türleri, select deyimi sonuçlarına dayanır. Veri almak için bu deyim harici bir tablodan seçim yapabilir. |
| [SEÇOLARAK HARICI TABLO OLUŞTUR](/sql/t-sql/statements/create-external-table-as-select-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) | Seçili bir deyimin sonuçlarını dış konuma dışa dışa dışa dışa dışa aktararak yeni bir dış tablo oluşturur.  Konumu, Azure Blob depolama veya Azure Veri Gölü Depolama'dır. |

## <a name="aligning-source-data-with-the-data-warehouse"></a>Kaynak verilerini veri ambarıyla hizalama

Veri ambarı tabloları, başka bir veri kaynağından veri yükleyerek doldurulur. Başarılı bir yük elde etmek için, kaynak verilerdeki sütunların sayısı ve veri türleri veri ambarındaki tablo tanımıyla hizalanmalıdır.

> [!NOTE]
> Verilerin hizalanması, tablolarınızı tasarlamanın en zor kısmı olabilir.

Veriler birden çok veri deposundan geliyorsa, verileri veri ambarına taşınabilir ve tümleştirme tablosunda depolayabilirsiniz. Veriler tümleştirme tablosuna girince, dönüştürme işlemlerini uygulamak için SQL havuzunun gücünü kullanabilirsiniz. Veriler hazırlandıktan sonra üretim tablolarına ekleyebilirsiniz.

## <a name="unsupported-table-features"></a>Desteklenmeyen tablo özellikleri

SQL havuzu, diğer veritabanları tarafından sunulan tablo özelliklerinin çoğunu destekler, ancak tümü ne de tümüne destekler.  Aşağıdaki liste, SQL havuzunda desteklenmeyen tablo özelliklerinden bazılarını gösterir.

- Yabancı anahtar, [Tablo Kısıtlamaları](/sql/t-sql/statements/alter-table-table-constraint-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) kontrol
- [Hesaplanan Sütunlar](/sql/t-sql/statements/alter-table-computed-column-definition-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)
- [Dizine Eklenmiş Görünümler](/sql/relational-databases/views/create-indexed-views?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)
- [Sequence](/sql/t-sql/statements/create-sequence-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)
- [Seyrek Sütunlar](/sql/relational-databases/tables/use-sparse-columns?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)
- Vekil Anahtarları, [Kimlik](../sql-data-warehouse/sql-data-warehouse-tables-identity.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) ile uygulamak
- [Eş anlamlılar](/sql/t-sql/statements/create-synonym-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)
- [Tetikleyiciler](/sql/t-sql/statements/create-trigger-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)
- [Benzersiz Dizinler](/sql/t-sql/statements/create-index-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)
- [Kullanıcı Tanımlı Türler](/sql/relational-databases/native-client/features/using-user-defined-types?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)

## <a name="table-size-queries"></a>Tablo boyutu sorguları

60 dağıtımın her birinde bir tablo tarafından tüketilen alanı ve satırları belirlemenin basit bir [yolu, DBCC PDW_SHOWSPACEUSED](/sql/t-sql/database-console-commands/dbcc-pdw-showspaceused-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)kullanmaktır.

```sql
DBCC PDW_SHOWSPACEUSED('dbo.FactInternetSales');
```

DBCC komutlarını kullanmanın oldukça sınırlayıcı olabileceğini unutmayın.  Dinamik yönetim görünümleri (DMV'ler) DBCC komutlarından daha fazla ayrıntı gösterir. Aşağıdaki görünümü oluşturarak başlayın.

```sql
CREATE VIEW dbo.vTableSizes
AS
WITH base
AS
(
SELECT
 GETDATE()                                                             AS  [execution_time]
, DB_NAME()                                                            AS  [database_name]
, s.name                                                               AS  [schema_name]
, t.name                                                               AS  [table_name]
, QUOTENAME(s.name)+'.'+QUOTENAME(t.name)                              AS  [two_part_name]
, nt.[name]                                                            AS  [node_table_name]
, ROW_NUMBER() OVER(PARTITION BY nt.[name] ORDER BY (SELECT NULL))     AS  [node_table_name_seq]
, tp.[distribution_policy_desc]                                        AS  [distribution_policy_name]
, c.[name]                                                             AS  [distribution_column]
, nt.[distribution_id]                                                 AS  [distribution_id]
, i.[type]                                                             AS  [index_type]
, i.[type_desc]                                                        AS  [index_type_desc]
, nt.[pdw_node_id]                                                     AS  [pdw_node_id]
, pn.[type]                                                            AS  [pdw_node_type]
, pn.[name]                                                            AS  [pdw_node_name]
, di.name                                                              AS  [dist_name]
, di.position                                                          AS  [dist_position]
, nps.[partition_number]                                               AS  [partition_nmbr]
, nps.[reserved_page_count]                                            AS  [reserved_space_page_count]
, nps.[reserved_page_count] - nps.[used_page_count]                    AS  [unused_space_page_count]
, nps.[in_row_data_page_count]
    + nps.[row_overflow_used_page_count]
    + nps.[lob_used_page_count]                                        AS  [data_space_page_count]
, nps.[reserved_page_count]
 - (nps.[reserved_page_count] - nps.[used_page_count])
 - ([in_row_data_page_count]
         + [row_overflow_used_page_count]+[lob_used_page_count])       AS  [index_space_page_count]
, nps.[row_count]                                                      AS  [row_count]
from
    sys.schemas s
INNER JOIN sys.tables t
    ON s.[schema_id] = t.[schema_id]
INNER JOIN sys.indexes i
    ON  t.[object_id] = i.[object_id]
    AND i.[index_id] <= 1
INNER JOIN sys.pdw_table_distribution_properties tp
    ON t.[object_id] = tp.[object_id]
INNER JOIN sys.pdw_table_mappings tm
    ON t.[object_id] = tm.[object_id]
INNER JOIN sys.pdw_nodes_tables nt
    ON tm.[physical_name] = nt.[name]
INNER JOIN sys.dm_pdw_nodes pn
    ON  nt.[pdw_node_id] = pn.[pdw_node_id]
INNER JOIN sys.pdw_distributions di
    ON  nt.[distribution_id] = di.[distribution_id]
INNER JOIN sys.dm_pdw_nodes_db_partition_stats nps
    ON nt.[object_id] = nps.[object_id]
    AND nt.[pdw_node_id] = nps.[pdw_node_id]
    AND nt.[distribution_id] = nps.[distribution_id]
LEFT OUTER JOIN (select * from sys.pdw_column_distribution_properties where distribution_ordinal = 1) cdp
    ON t.[object_id] = cdp.[object_id]
LEFT OUTER JOIN sys.columns c
    ON cdp.[object_id] = c.[object_id]
    AND cdp.[column_id] = c.[column_id]
WHERE pn.[type] = 'COMPUTE'
)
, size
AS
(
SELECT
   [execution_time]
,  [database_name]
,  [schema_name]
,  [table_name]
,  [two_part_name]
,  [node_table_name]
,  [node_table_name_seq]
,  [distribution_policy_name]
,  [distribution_column]
,  [distribution_id]
,  [index_type]
,  [index_type_desc]
,  [pdw_node_id]
,  [pdw_node_type]
,  [pdw_node_name]
,  [dist_name]
,  [dist_position]
,  [partition_nmbr]
,  [reserved_space_page_count]
,  [unused_space_page_count]
,  [data_space_page_count]
,  [index_space_page_count]
,  [row_count]
,  ([reserved_space_page_count] * 8.0)                                 AS [reserved_space_KB]
,  ([reserved_space_page_count] * 8.0)/1000                            AS [reserved_space_MB]
,  ([reserved_space_page_count] * 8.0)/1000000                         AS [reserved_space_GB]
,  ([reserved_space_page_count] * 8.0)/1000000000                      AS [reserved_space_TB]
,  ([unused_space_page_count]   * 8.0)                                 AS [unused_space_KB]
,  ([unused_space_page_count]   * 8.0)/1000                            AS [unused_space_MB]
,  ([unused_space_page_count]   * 8.0)/1000000                         AS [unused_space_GB]
,  ([unused_space_page_count]   * 8.0)/1000000000                      AS [unused_space_TB]
,  ([data_space_page_count]     * 8.0)                                 AS [data_space_KB]
,  ([data_space_page_count]     * 8.0)/1000                            AS [data_space_MB]
,  ([data_space_page_count]     * 8.0)/1000000                         AS [data_space_GB]
,  ([data_space_page_count]     * 8.0)/1000000000                      AS [data_space_TB]
,  ([index_space_page_count]  * 8.0)                                   AS [index_space_KB]
,  ([index_space_page_count]  * 8.0)/1000                              AS [index_space_MB]
,  ([index_space_page_count]  * 8.0)/1000000                           AS [index_space_GB]
,  ([index_space_page_count]  * 8.0)/1000000000                        AS [index_space_TB]
FROM base
)
SELECT *
FROM size
;
```

### <a name="table-space-summary"></a>Tablo alanı özeti

Bu sorgu satırları ve alanı tabloya göre döndürür.  Tablo alanı özeti, en büyük tablolarınız hangi tabloların olduğunu görmenizi sağlar. Ayrıca, bunların yuvarlak-robin, çoğaltılmış veya karma dağıtılmış olup olmadığını da görürsünüz.  Karma dağıtılmış tablolar için sorgu dağıtım sütununa gösterir.  

```sql
SELECT
     database_name
,    schema_name
,    table_name
,    distribution_policy_name
,      distribution_column
,    index_type_desc
,    COUNT(distinct partition_nmbr) as nbr_partitions
,    SUM(row_count)                 as table_row_count
,    SUM(reserved_space_GB)         as table_reserved_space_GB
,    SUM(data_space_GB)             as table_data_space_GB
,    SUM(index_space_GB)            as table_index_space_GB
,    SUM(unused_space_GB)           as table_unused_space_GB
FROM
    dbo.vTableSizes
GROUP BY
     database_name
,    schema_name
,    table_name
,    distribution_policy_name
,      distribution_column
,    index_type_desc
ORDER BY
    table_reserved_space_GB desc
;
```

### <a name="table-space-by-distribution-type"></a>Dağılım türüne göre tablo alanı

```sql
SELECT
     distribution_policy_name
,    SUM(row_count)                as table_type_row_count
,    SUM(reserved_space_GB)        as table_type_reserved_space_GB
,    SUM(data_space_GB)            as table_type_data_space_GB
,    SUM(index_space_GB)           as table_type_index_space_GB
,    SUM(unused_space_GB)          as table_type_unused_space_GB
FROM dbo.vTableSizes
GROUP BY distribution_policy_name
;
```

### <a name="table-space-by-index-type"></a>Dizin türüne göre tablo alanı

```sql
SELECT
     index_type_desc
,    SUM(row_count)                as table_type_row_count
,    SUM(reserved_space_GB)        as table_type_reserved_space_GB
,    SUM(data_space_GB)            as table_type_data_space_GB
,    SUM(index_space_GB)           as table_type_index_space_GB
,    SUM(unused_space_GB)          as table_type_unused_space_GB
FROM dbo.vTableSizes
GROUP BY index_type_desc
;
```

### <a name="distribution-space-summary"></a>Dağıtım alanı özeti

```sql
SELECT
    distribution_id
,    SUM(row_count)                as total_node_distribution_row_count
,    SUM(reserved_space_MB)        as total_node_distribution_reserved_space_MB
,    SUM(data_space_MB)            as total_node_distribution_data_space_MB
,    SUM(index_space_MB)           as total_node_distribution_index_space_MB
,    SUM(unused_space_MB)          as total_node_distribution_unused_space_MB
FROM dbo.vTableSizes
GROUP BY     distribution_id
ORDER BY    distribution_id
;
```

## <a name="next-steps"></a>Sonraki adımlar

Veri ambarınız için tabloları oluşturduktan sonra, bir sonraki adım tabloya veri yüklemektir.  Yükleme eğitimi için [bkz.](../sql-data-warehouse/load-data-wideworldimportersdw.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json#load-the-data-into-sql-pool)
