---
title: İş yükü yönetimi için kaynak sınıfları
description: Azure Synapse Analytics'teki sorgular için eşzamanlılık ve bilgi işlem kaynaklarını yönetmek için kaynak sınıflarını kullanma kılavuzu.
services: synapse-analytics
author: ronortloff
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 02/04/2020
ms.author: rortloff
ms.reviewer: jrasnick
ms.custom: azure-synapse
ms.openlocfilehash: 47fd30fbb6e6836d6edf18ac68164d515f3aeb93
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80350742"
---
# <a name="workload-management-with-resource-classes-in-azure-synapse-analytics"></a>Azure Synapse Analytics'te kaynak sınıflarıyla iş yükü yönetimi

Azure Sinaps'taki SQL Analytics sorguları için bellek ve eşzamanlılık yönetmek için kaynak sınıflarını kullanma kılavuzu.  

## <a name="what-are-resource-classes"></a>Kaynak sınıfları nelerdir

Bir sorgunun performans kapasitesi kullanıcının kaynak sınıfı tarafından belirlenir.  Kaynak sınıfları, sql analytics'te bilgi işlem kaynaklarını ve sorgu yürütmeiçin eşzamanlılığı yöneten önceden belirlenmiş kaynak sınırlarıdır. Kaynak sınıfları, aynı anda çalışan sorgu sayısına ve her sorguya atanan bilgi işlem kaynaklarına sınırlar koyarak sorgularınızın kaynaklarını yapılandırmanıza yardımcı olabilir.  Hafıza ve eşzamanlılık arasında bir denge var.

- Daha küçük kaynak sınıfları sorgu başına maksimum belleği azaltır, ancak eşzamanlılığı artırır.
- Daha büyük kaynak sınıfları sorgu başına maksimum belleği artırır, ancak eşzamanlılığı azaltır.

İki tür kaynak sınıfı vardır:

- Sabit bir veri kümesi boyutunda artırılmış eşzamanlılık için iyi bir şekilde uygun olan statik kaynak sınıfları.
- Boyut olarak büyüyen ve hizmet düzeyi ölçeklendirildikçe daha fazla performans gerektiren veri kümeleri için çok uygun olan dinamik kaynak sınıfları.

Kaynak sınıfları, kaynak tüketimini ölçmek için eşzamanlılık yuvalarını kullanır.  [Eşzamanlılık yuvaları](#concurrency-slots) bu makalede daha sonra açıklanmıştır.

- Kaynak sınıfları için kaynak kullanımını görüntülemek için [Bellek ve eşzamanlılık sınırlarına](memory-concurrency-limits.md)bakın.
- Kaynak sınıfını ayarlamak için sorguyu farklı bir kullanıcı altında çalıştırabilir veya [geçerli kullanıcının kaynak sınıfı üyeliğini değiştirebilirsiniz.](#change-a-users-resource-class)

### <a name="static-resource-classes"></a>Statik kaynak sınıfları

Statik kaynak sınıfları, [veri ambarı birimlerinde](what-is-a-data-warehouse-unit-dwu-cdwu.md)ölçülen geçerli performans düzeyine bakılmaksızın aynı miktarda bellek ayırır. Sorgular performans düzeyine bakılmaksızın aynı bellek ayırmayı aldığıiçin, [veri ambarını ölçeklemek](quickstart-scale-compute-portal.md) kaynak sınıfı içinde daha fazla sorgunun çalışmasına olanak tanır.  Veri hacmi biliniyorsa ve sabitse statik kaynak sınıfları idealdir.

Statik kaynak sınıfları, önceden tanımlanmış bu veritabanı rolleriyle uygulanır:

- staticrc10
- staticrc20
- staticrc30
- staticrc40
- staticrc50
- staticrc60
- staticrc70
- staticrc80

### <a name="dynamic-resource-classes"></a>Dinamik kaynak sınıfları

Dinamik Kaynak Sınıfları, geçerli hizmet düzeyine bağlı olarak değişken miktarda bellek ayırır. Statik kaynak sınıfları daha yüksek eşzamanlılık ve statik veri birimleri için yararlı olsa da, dinamik kaynak sınıfları büyüyen veya değişken veri miktarı için daha uygundur.  Daha büyük bir hizmet düzeyine kadar ölçeklendirildiğinde, sorgularınız otomatik olarak daha fazla bellek alır.  

Dinamik kaynak sınıfları bu önceden tanımlanmış veritabanı rolleri ile uygulanır:

- smallrc
- mediumrc
- daha büyük
- xbiggerc

Her kaynak sınıfı için bellek ayırma aşağıdaki gibidir. 

| Hizmet Düzeyi  | smallrc           | mediumrc               | daha büyük                | xbiggerc               |
|:--------------:|:-----------------:|:----------------------:|:----------------------:|:----------------------:|
| DW100c         | %25               | %25                    | %25                    | %70                    |
| DW200c         | 12.5%             | 12.5%                  | 22%                    | %70                    |
| DW300c         | %8                | %10                    | 22%                    | %70                    |
| DW400c         | 6.25%             | %10                    | 22%                    | %70                    |
| DW500c'yi seçin         | %5                | %10                    | 22%                    | %70                    |
| DW1000c için<br> DW30000c | %3       | %10                    | 22%                    | %70                    |



### <a name="default-resource-class"></a>Varsayılan kaynak sınıfı

Varsayılan olarak, her kullanıcı dinamik kaynak sınıfı **smallrc**üyesidir.

Hizmet yöneticisinin kaynak sınıfı smallrc'de sabitlenir ve değiştirilemez.  Hizmet yöneticisi, sağlama işlemi sırasında oluşturulan kullanıcıdır.  Bu bağlamdaki hizmet yöneticisi, yeni bir sunucuyla yeni bir SQL Analytics örneği oluştururken "Sunucu yöneticisi girişi" için belirtilen oturum açmadır.

> [!NOTE]
> Active Directory yöneticisi olarak tanımlanan kullanıcılar veya gruplar da hizmet yöneticileridir.
>
>

## <a name="resource-class-operations"></a>Kaynak sınıfı işlemleri

Kaynak sınıfları, veri yönetimi ve işleme etkinlikleri için performansı artırmak için tasarlanmıştır. Karmaşık sorgular, büyük bir kaynak sınıfı altında çalışmaktan da yararlanabilir. Örneğin, kaynak sınıfı sorgunun bellekte yürütülmesini sağlayacak kadar büyük olduğunda, büyük birleştirmeler ve sıralamalar için sorgu performansı artabilir.

### <a name="operations-governed-by-resource-classes"></a>Kaynak sınıfları tarafından yönetilen işlemler

Bu işlemler kaynak sınıfları tarafından yönetilir:

- EKLE-SEÇ, GÜNCELLEME, SILME
- SELECT (kullanıcı tablolarını sorgularken)
- ALTER INDEX - YENIDEN OLUŞTURMA veya YENIDEN DÜZENLEME
- TABLOYU YENIDEN DEĞIŞTIR
- CREATE INDEX
- KÜMELENMIŞ SÜTUN DEPOSU DIZINI OLUŞTURMA
- SELECT (CTAS) OLARAK TABLO OLUŞTUR
- Veri yükleme
- Veri Hareketi Servisi (DMS) tarafından yürütülen veri hareketi işlemleri

> [!NOTE]  
> Dinamik yönetim görünümleri (DMV' ler) veya diğer sistem görünümleri hakkındaki SELECT ifadeleri eşzamanlılık sınırlarının hiçbirine tabi değildir. Sistemde çalıştırılabilen sorgu sayısına bakılmaksızın sistemi izleyebilirsiniz.
>
>

### <a name="operations-not-governed-by-resource-classes"></a>Kaynak sınıfları tarafından yönetilmeyen işlemler

Kullanıcı daha büyük bir kaynak sınıfının üyesi olsa bile bazı sorgular her zaman smallrc kaynak sınıfında çalışır. Bu muaf sorgular eşzamanlılık sınırına dahil edilmez. Örneğin, eşzamanlılık sınırı 16 ise, birçok kullanıcı kullanılabilir eşzamanlılık yuvalarını etkilemeden sistem görünümlerinden seçim yapabilir.

Aşağıdaki ifadeler kaynak sınıflarından muaftır ve her zaman smallrc'de çalışır:

- CREATE veya DROP TABLE
- ALTER TABLO ... GEÇIŞ, BÖLME veya Birleştirme BÖLÜMÜ
- ALTER INDEX DEVRE Dışı
- DROP INDEX
- İstATİstİkLER OLUŞTURMA, GÜNCELLEME VEYA DAMLA
- BUKATET TABLOSU
- YETKILENDIRMEYI DEĞIŞTIR
- Gİrİş YAP
- CREATE, ALTER veya DROP USER
- OLUŞTURMA, DEĞIŞTIRME veya BıRAKMA YORDAMı
- CREATE veya DROP GÖRÜNÜMÜ
- EKLEME DEĞERLERI
- Sistem görünümlerinden ve DMV'lerden SELECT
- Açıklaya
- Dbcc

<!--
Removed as these two are not confirmed / supported under SQL DW
- CREATE REMOTE TABLE AS SELECT
- CREATE EXTERNAL TABLE AS SELECT
- REDISTRIBUTE
-->

## <a name="concurrency-slots"></a>Eşzamanlılık yuvaları

Eşzamanlılık yuvaları sorgu yürütme için kullanılabilir kaynakları izlemek için kullanışlı bir yoldur. Oturma sınırlı olduğu için bir konserde koltuk rezervasyonu için satın aldığınız biletler gibidir. Veri ambarı başına düşen toplam eşzamanlılık yuvası sayısı hizmet düzeyine göre belirlenir. Bir sorgu yürütmeye başlamadan önce, yeterli eşzamanlılık yuvası rezerve etmek gerekir. Bir sorgu tamamlandığında, eşzamanlılık yuvalarını serbest bırakır.  

- 10 eşzamanlılık yuvasıyla çalışan bir sorgu, 2 eşzamanlılık yuvasıyla çalışan bir sorgudan 5 kat daha fazla bilgi işlem kaynağına erişebilir.
- Her sorgu 10 eşzamanlılık yuvası gerektiriyorsa ve 40 eşzamanlılık yuvası varsa, aynı anda yalnızca 4 sorgu çalıştırılabilir.

Yalnızca kaynak yönetilen sorgular eşzamanlılık yuvalarını tüketir. Sistem sorguları ve bazı önemsiz sorgular herhangi bir yuva tüketir. Tüketilen eşzamanlılık yuvalarının tam sayısı sorgunun kaynak sınıfına göre belirlenir.

## <a name="view-the-resource-classes"></a>Kaynak sınıflarını görüntüleme

Kaynak sınıfları önceden tanımlanmış veritabanı rolleri olarak uygulanır. İki tür kaynak sınıfı vardır: dinamik ve statik. Kaynak sınıflarının listesini görüntülemek için aşağıdaki sorguyu kullanın:

```sql
SELECT name
FROM   sys.database_principals
WHERE  name LIKE '%rc%' AND type_desc = 'DATABASE_ROLE';
```

## <a name="change-a-users-resource-class"></a>Kullanıcının kaynak sınıfLarını değiştirme

Kaynak sınıfları, kullanıcıları veritabanı rollerine atayarak uygulanır. Bir kullanıcı bir sorgu çalıştırdığında, sorgu kullanıcının kaynak sınıfıyla çalışır. Örneğin, bir kullanıcı staticrc10 veritabanı rolünün bir üyesiyse, sorguları az miktarda bellekle çalışır. Bir veritabanı kullanıcısı xbiggerc veya staticrc80 veritabanı rollerinin bir üyesiyse, sorguları büyük miktarda bellekle çalışır.

Bir kullanıcının kaynak sınıfını artırmak için, kullanıcıyı büyük bir kaynak sınıfının veritabanı rolüne eklemek için [sp_addrolemember](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-addrolemember-transact-sql) kullanın.  Aşağıdaki kod, daha büyük veritabanı rolüne bir kullanıcı ekler.  Her istek sistem belleği% 22 alır.

```sql
EXEC sp_addrolemember 'largerc', 'loaduser';
```

Kaynak sınıfını azaltmak için [sp_droprolemember](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-droprolemember-transact-sql)kullanın.  'Loaduser' bir üye veya başka bir kaynak sınıfları değilse, varsayılan smallrc kaynak sınıfına %3 bellek hibesi ile girerler.  

```sql
EXEC sp_droprolemember 'largerc', 'loaduser';
```

## <a name="resource-class-precedence"></a>Kaynak sınıfı önceliği

Kullanıcılar birden çok kaynak sınıfının üyesi olabilir. Bir kullanıcı birden fazla kaynak sınıfına ait olduğunda:

- Dinamik kaynak sınıfları statik kaynak sınıfları üzerinde önceliklidir. Örneğin, bir kullanıcı hem mediumrc(dinamik) hem de staticrc80 (statik) üyesiyse, sorgular mediumrc ile çalışır.
- Daha büyük kaynak sınıfları, daha küçük kaynak sınıfları üzerinde önceliklidir. Örneğin, bir kullanıcı mediumrc ve biggerc üyesiyse, sorgular biggerc ile çalışır. Aynı şekilde, bir kullanıcı hem staticrc20 hem de statirc80 üyesiyse, sorgular staticrc80 kaynak ayırmaları ile çalışır.

## <a name="recommendations"></a>Öneriler

>[!NOTE]
>İş yükünüz ve öngörülebilir performansınız üzerinde daha fazla denetim için iş yükü yönetimi yeteneklerinden[(iş yükü yalıtımı,](sql-data-warehouse-workload-isolation.md) [sınıflandırma](sql-data-warehouse-workload-classification.md) ve [önem)](sql-data-warehouse-workload-importance.md)yararlanmayı düşünün.  
>
>

Belirli bir sorgu veya yükleme işlemi türünü çalıştırmaya adanmış bir kullanıcı oluşturmanızı öneririz. Kaynak sınıfını sık sık değiştirmek yerine bu kullanıcıya kalıcı bir kaynak sınıfı verin. Statik kaynak sınıfları iş yükü üzerinde daha fazla genel denetim sağlar, bu nedenle dinamik kaynak sınıflarını düşünmeden önce statik kaynak sınıfları kullanmanızı öneririz.

### <a name="resource-classes-for-load-users"></a>Yük kullanıcıları için kaynak sınıfları

`CREATE TABLE`varsayılan olarak kümelenmiş sütun deposu dizinlerini kullanır. Verileri sütun deposu dizinine sıkıştırmak bellek yoğun bir işlemdir ve bellek basıncı dizin kalitesini düşürebilir. Bellek basıncı, veri yüklerken daha yüksek bir kaynak sınıfına ihtiyaç duyulmasına neden olabilir. Yüklerin yeterli belleğe sahip olmasını sağlamak için, yükleri çalıştırmak için atanmış bir kullanıcı oluşturabilir ve bu kullanıcıyı daha yüksek bir kaynak sınıfına atayabilirsiniz.

Yükleri verimli bir şekilde işlemek için gereken bellek, yüklenen tablonun yapısına ve veri boyutuna bağlıdır. Bellek gereksinimleri hakkında daha fazla bilgi için [bkz.](sql-data-warehouse-memory-optimizations-for-columnstore-compression.md)

Bellek gereksinimini belirledikten sonra, yük kullanıcısını statik veya dinamik bir kaynak sınıfına atayıp atamayacağınızı seçin.

- Tablo belleği gereksinimleri belirli bir aralıkta kaldığında statik kaynak sınıfı kullanın. Yükler uygun bellekle çalışır. Veri ambarını ölçeklediğinizde, yüklerin daha fazla belleğe ihtiyacı yoktur. Statik kaynak sınıfı kullanarak, bellek ayırmaları sabit kalır. Bu tutarlılık bellek tasarrufu sağlar ve daha fazla sorguaynı anda çalışmasına izin verir. Yeni çözümlerin önce statik kaynak sınıflarını kullanmasını öneririz, çünkü bunlar daha fazla denetim sağlar.
- Tablo belleği gereksinimleri büyük ölçüde değiştiğinde dinamik bir kaynak sınıfı kullanın. Yükler, geçerli DWU veya cDWU düzeyinin sağladığından daha fazla bellek gerektirebilir. Veri ambarını ölçeklemek, yükleme işlemlerine daha fazla bellek ekler ve bu da yüklerin daha hızlı performans göstermesini sağlar.

### <a name="resource-classes-for-queries"></a>Sorgular için kaynak sınıfları

Bazı sorgular bilgi işlem yoğun, bazıları ise yoğun değildir.  

- Sorgular karmaşık olduğunda dinamik bir kaynak sınıfı seçin, ancak yüksek eşzamanlılık gerekmez.  Örneğin, günlük veya haftalık raporlar oluşturmak zaman zaman kaynak gereksinimidir. Raporlar büyük miktarda veri işliyorsa, veri ambarını ölçekleme, kullanıcının varolan kaynak sınıfına daha fazla bellek sağlar.
- Kaynak beklentileri gün boyunca değiştiğinde statik bir kaynak sınıfı seçin. Örneğin, veri ambarı birçok kişi tarafından sorgulandığında statik kaynak sınıfı iyi çalışır. Veri ambarını ölçeklendirirken, kullanıcıya ayrılan bellek miktarı değişmez. Sonuç olarak, sistemde paralel olarak daha fazla sorgu yürütülebilir.

Uygun bellek hibeleri, sorgulanmış veri miktarı, tablo şemalarının yapısı ve çeşitli birleştirmeler, seçme ler ve grup yüklemleri gibi birçok etkene bağlıdır. Genel olarak, daha fazla bellek ayırmak sorguların daha hızlı tamamlanmasını sağlar, ancak genel eşzamanlılığı azaltır. Eşzamanlılık bir sorun değilse, aşırı ayırma belleği iş haline zarar vermez.

Performansı ayarlamak için farklı kaynak sınıfları kullanın. Sonraki bölümde, en iyi kaynak sınıfını bulmanıza yardımcı olan depolanmış bir yordam verir.

## <a name="example-code-for-finding-the-best-resource-class"></a>En iyi kaynak sınıfını bulmak için örnek kod

Belirli bir SLO'da kaynak sınıfı başına eşzamanlılık ve bellek hibesini ve belirli bir kaynak sınıfında bölümlenmemiş CCI tablosundaki bellek yoğun CCI işlemleri için en iyi kaynak sınıfını bulmak için aşağıdaki belirtilen depolanmış yordamı kullanabilirsiniz:

Bu depolanan yordamın amacı aşağıdadır:

1. Belirli bir SLO'da kaynak sınıfı başına eşzamanlılık ve bellek hibesini görmek için. Kullanıcının bu örnekte gösterildiği gibi hem şema hem de tablo adı için NULL sağlaması gerekir.  
2. Belirli bir kaynak sınıfında bölümlenmemiş CCI tablosunda bellek yoğun CCI işlemleri (yük, kopyalama tablosu, yeniden dizini, vb.) için en iyi kaynak sınıfını görmek için. Depolanan proc gerekli bellek hibe bulmak için tablo şema kullanır.

### <a name="dependencies--restrictions"></a>Bağımlılıklar & Kısıtlamalar

- Bu depolanan yordam, bölümlenmiş bir cci tablosunun bellek gereksinimini hesaplamak için tasarlanmaz.
- Bu depolanan yordam, CTAS/INSERT-SELECT'in SELECT bölümü için bellek gereksinimlerini hesaba katmaz ve select olduğunu varsayar.
- Bu depolanan yordam, bu depolanan yordamın oluşturulduğu oturumda kullanılabilen geçici bir tablo kullanır.
- Bu depolanan yordam geçerli tekliflere (örneğin, donanım yapılandırması, DMS config) bağlıdır ve bu değişikliklerden herhangi biri değişirse, bu depolanan proc düzgün çalışmaz.  
- Bu depolanan yordam, varolan eşzamanlılık sınırı tekliflerine bağlıdır ve bu değişiklik durumunda bu depolanan yordam doğru çalışmaz.  
- Bu depolanan yordam varolan kaynak sınıfı tekliflerine bağlıdır ve bu değişiklik durumunda bu depolanan yordam doğru çalışmaz.  

>[!NOTE]  
>Sağlanan parametrelerle depolanan yordamı çalıştırdıktan sonra çıktı almıyorsanız, iki durum olabilir.
>
>1. Ya DW Parametresi geçersiz bir SLO değeri içerir
>2. Veya, tablodaki CCI işlemi için eşleşen bir kaynak sınıfı yoktur.
>
>Örneğin, DW100c'de, kullanılabilir en yüksek bellek hibesi 1 GB'dır ve tablo şeması 1 GB gereksinimini aşacak kadar genişse.

### <a name="usage-example"></a>Kullanım örneği

Söz dizimi:  
`EXEC dbo.prc_workload_management_by_DWU @DWU VARCHAR(7), @SCHEMA_NAME VARCHAR(128), @TABLE_NAME VARCHAR(128)`
  
1. @DWU:Ya DW DB geçerli DWU ayıklamak için bir NULL parametresi sağlamak veya 'DW100c' şeklinde desteklenen DWU sağlamak
2. @SCHEMA_NAME:Tablonun şema adını sağlama
3. @TABLE_NAME:İlgi alanının bir tablo adı sağlama

Bu depolanan proc çalıştıran örnekler:

```sql
EXEC dbo.prc_workload_management_by_DWU 'DW2000c', 'dbo', 'Table1';  
EXEC dbo.prc_workload_management_by_DWU NULL, 'dbo', 'Table1';  
EXEC dbo.prc_workload_management_by_DWU 'DW6000c', NULL, NULL;  
EXEC dbo.prc_workload_management_by_DWU NULL, NULL, NULL;  
```

Aşağıdaki deyim, önceki örneklerde kullanılan Tablo1'i oluşturur.
`CREATE TABLE Table1 (a int, b varchar(50), c decimal (18,10), d char(10), e varbinary(15), f float, g datetime, h date);`

### <a name="stored-procedure-definition"></a>Depolanan yordam tanımı

```sql
-------------------------------------------------------------------------------
-- Dropping prc_workload_management_by_DWU procedure if it exists.
-------------------------------------------------------------------------------
IF EXISTS (SELECT * FROM sys.objects WHERE type = 'P' AND name = 'prc_workload_management_by_DWU')
DROP PROCEDURE dbo.prc_workload_management_by_DWU
GO

-------------------------------------------------------------------------------
-- Creating prc_workload_management_by_DWU.
-------------------------------------------------------------------------------
CREATE PROCEDURE dbo.prc_workload_management_by_DWU
(@DWU VARCHAR(7),
 @SCHEMA_NAME VARCHAR(128),
 @TABLE_NAME VARCHAR(128)
)
AS

IF @DWU IS NULL
BEGIN
-- Selecting proper DWU for the current DB if not specified.

SELECT @DWU = 'DW'+ CAST(CASE WHEN Mem> 4 THEN Nodes*500 
  ELSE Mem*100 
  END AS VARCHAR(10)) +'c'
    FROM (
      SELECT Nodes=count(distinct n.pdw_node_id), Mem=max(i.committed_target_kb/1000/1000/60)
        FROM sys.dm_pdw_nodes n
        CROSS APPLY sys.dm_pdw_nodes_os_sys_info i
        WHERE type = 'COMPUTE')A
END

-- Dropping temp table if exists.
IF OBJECT_ID('tempdb..#ref') IS NOT NULL
BEGIN
  DROP TABLE #ref;
END;

-- Creating ref. temp table (CTAS) to hold mapping info.
CREATE TABLE #ref
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
WITH
-- Creating concurrency slots mapping for various DWUs.
alloc
AS
(
SELECT 'DW100c' AS DWU,4 AS max_queries,4 AS max_slots,1 AS slots_used_smallrc,1 AS slots_used_mediumrc,2 AS slots_used_largerc,4 AS slots_used_xlargerc,1 AS slots_used_staticrc10,2 AS slots_used_staticrc20,4 AS slots_used_staticrc30,4 AS slots_used_staticrc40,4 AS slots_used_staticrc50,4 AS slots_used_staticrc60,4 AS slots_used_staticrc70,4 AS slots_used_staticrc80
  UNION ALL
   SELECT 'DW200c',8,8,1,2,4,8,1,2,4,8,8,8,8,8
  UNION ALL
   SELECT 'DW300c',12,12,1,2,4,8,1,2,4,8,8,8,8,8
  UNION ALL
   SELECT 'DW400c',16,16,1,4,8,16,1,2,4,8,16,16,16,16
  UNION ALL
   SELECT 'DW500c',20,20,1,4,8,16,1,2,4,8,16,16,16,16
  UNION ALL
   SELECT 'DW1000c',32,40,1,4,8,28,1,2,4,8,16,32,32,32
  UNION ALL
   SELECT 'DW1500c',32,60,1,6,13,42,1,2,4,8,16,32,32,32
  UNION ALL
   SELECT 'DW2000c',48,80,2,8,17,56,1,2,4,8,16,32,64,64
  UNION ALL
   SELECT 'DW2500c',48,100,3,10,22,70,1,2,4,8,16,32,64,64
  UNION ALL
   SELECT 'DW3000c',64,120,3,12,26,84,1,2,4,8,16,32,64,64
  UNION ALL
   SELECT 'DW5000c',64,200,6,20,44,140,1,2,4,8,16,32,64,128
  UNION ALL
   SELECT 'DW6000c',128,240,7,24,52,168,1,2,4,8,16,32,64,128
  UNION ALL
   SELECT 'DW7500c',128,300,9,30,66,210,1,2,4,8,16,32,64,128
  UNION ALL
   SELECT 'DW10000c',128,400,12,40,88,280,1,2,4,8,16,32,64,128
  UNION ALL
   SELECT 'DW15000c',128,600,18,60,132,420,1,2,4,8,16,32,64,128
  UNION ALL
   SELECT 'DW30000c',128,1200,36,120,264,840,1,2,4,8,16,32,64,128
)
-- Creating workload mapping to their corresponding slot consumption and default memory grant.
,map  
AS
(
  SELECT CONVERT(varchar(20), 'SloDWGroupSmall') AS wg_name, slots_used_smallrc AS slots_used FROM alloc WHERE DWU = @DWU
UNION ALL
  SELECT CONVERT(varchar(20), 'SloDWGroupMedium') AS wg_name, slots_used_mediumrc AS slots_used FROM alloc WHERE DWU = @DWU
UNION ALL
  SELECT CONVERT(varchar(20), 'SloDWGroupLarge') AS wg_name, slots_used_largerc AS slots_used FROM alloc WHERE DWU = @DWU
UNION ALL
  SELECT CONVERT(varchar(20), 'SloDWGroupXLarge') AS wg_name, slots_used_xlargerc AS slots_used FROM alloc WHERE DWU = @DWU
  UNION ALL
  SELECT 'SloDWGroupC00',1
  UNION ALL
    SELECT 'SloDWGroupC01',2
  UNION ALL
    SELECT 'SloDWGroupC02',4
  UNION ALL
    SELECT 'SloDWGroupC03',8
  UNION ALL
    SELECT 'SloDWGroupC04',16
  UNION ALL
    SELECT 'SloDWGroupC05',32
  UNION ALL
    SELECT 'SloDWGroupC06',64
  UNION ALL
    SELECT 'SloDWGroupC07',128
)

-- Creating ref based on current / asked DWU.
, ref
AS
(
  SELECT  a1.*
  ,       m1.wg_name          AS wg_name_smallrc
  ,       m1.slots_used * 250 AS tgt_mem_grant_MB_smallrc
  ,       m2.wg_name          AS wg_name_mediumrc
  ,       m2.slots_used * 250 AS tgt_mem_grant_MB_mediumrc
  ,       m3.wg_name          AS wg_name_largerc
  ,       m3.slots_used * 250 AS tgt_mem_grant_MB_largerc
  ,       m4.wg_name          AS wg_name_xlargerc
  ,       m4.slots_used * 250 AS tgt_mem_grant_MB_xlargerc
  ,       m5.wg_name          AS wg_name_staticrc10
  ,       m5.slots_used * 250 AS tgt_mem_grant_MB_staticrc10
  ,       m6.wg_name          AS wg_name_staticrc20
  ,       m6.slots_used * 250 AS tgt_mem_grant_MB_staticrc20
  ,       m7.wg_name          AS wg_name_staticrc30
  ,       m7.slots_used * 250 AS tgt_mem_grant_MB_staticrc30
  ,       m8.wg_name          AS wg_name_staticrc40
  ,       m8.slots_used * 250 AS tgt_mem_grant_MB_staticrc40
  ,       m9.wg_name          AS wg_name_staticrc50
  ,       m9.slots_used * 250 AS tgt_mem_grant_MB_staticrc50
  ,       m10.wg_name          AS wg_name_staticrc60
  ,       m10.slots_used * 250 AS tgt_mem_grant_MB_staticrc60
  ,       m11.wg_name          AS wg_name_staticrc70
  ,       m11.slots_used * 250 AS tgt_mem_grant_MB_staticrc70
  ,       m12.wg_name          AS wg_name_staticrc80
  ,       m12.slots_used * 250 AS tgt_mem_grant_MB_staticrc80
  FROM alloc a1
  JOIN map   m1  ON a1.slots_used_smallrc     = m1.slots_used and m1.wg_name = 'SloDWGroupSmall'
  JOIN map   m2  ON a1.slots_used_mediumrc    = m2.slots_used and m2.wg_name = 'SloDWGroupMedium'
  JOIN map   m3  ON a1.slots_used_largerc     = m3.slots_used and m3.wg_name = 'SloDWGroupLarge'
  JOIN map   m4  ON a1.slots_used_xlargerc    = m4.slots_used and m4.wg_name = 'SloDWGroupXLarge'
  JOIN map   m5  ON a1.slots_used_staticrc10    = m5.slots_used and m5.wg_name NOT IN ('SloDWGroupSmall','SloDWGroupMedium','SloDWGroupLarge','SloDWGroupXLarge')
  JOIN map   m6  ON a1.slots_used_staticrc20    = m6.slots_used and m6.wg_name NOT IN ('SloDWGroupSmall','SloDWGroupMedium','SloDWGroupLarge','SloDWGroupXLarge')
  JOIN map   m7  ON a1.slots_used_staticrc30    = m7.slots_used and m7.wg_name NOT IN ('SloDWGroupSmall','SloDWGroupMedium','SloDWGroupLarge','SloDWGroupXLarge')
  JOIN map   m8  ON a1.slots_used_staticrc40    = m8.slots_used and m8.wg_name NOT IN ('SloDWGroupSmall','SloDWGroupMedium','SloDWGroupLarge','SloDWGroupXLarge')
  JOIN map   m9  ON a1.slots_used_staticrc50    = m9.slots_used and m9.wg_name NOT IN ('SloDWGroupSmall','SloDWGroupMedium','SloDWGroupLarge','SloDWGroupXLarge')
  JOIN map   m10  ON a1.slots_used_staticrc60    = m10.slots_used and m10.wg_name NOT IN ('SloDWGroupSmall','SloDWGroupMedium','SloDWGroupLarge','SloDWGroupXLarge')
  JOIN map   m11  ON a1.slots_used_staticrc70    = m11.slots_used and m11.wg_name NOT IN ('SloDWGroupSmall','SloDWGroupMedium','SloDWGroupLarge','SloDWGroupXLarge')
  JOIN map   m12  ON a1.slots_used_staticrc80    = m12.slots_used and m12.wg_name NOT IN ('SloDWGroupSmall','SloDWGroupMedium','SloDWGroupLarge','SloDWGroupXLarge')
  WHERE   a1.DWU = @DWU
)
SELECT  DWU
,       max_queries
,       max_slots
,       slots_used
,       wg_name
,       tgt_mem_grant_MB
,       up1 as rc
,       (ROW_NUMBER() OVER(PARTITION BY DWU ORDER BY DWU)) as rc_id
FROM
(
    SELECT  DWU
    ,       max_queries
    ,       max_slots
    ,       slots_used
    ,       wg_name
    ,       tgt_mem_grant_MB
    ,       REVERSE(SUBSTRING(REVERSE(wg_names),1,CHARINDEX('_',REVERSE(wg_names),1)-1)) as up1
    ,       REVERSE(SUBSTRING(REVERSE(tgt_mem_grant_MBs),1,CHARINDEX('_',REVERSE(tgt_mem_grant_MBs),1)-1)) as up2
    ,       REVERSE(SUBSTRING(REVERSE(slots_used_all),1,CHARINDEX('_',REVERSE(slots_used_all),1)-1)) as up3
    FROM    ref AS r1
    UNPIVOT
    (
        wg_name FOR wg_names IN (wg_name_smallrc,wg_name_mediumrc,wg_name_largerc,wg_name_xlargerc,
        wg_name_staticrc10, wg_name_staticrc20, wg_name_staticrc30, wg_name_staticrc40, wg_name_staticrc50,
        wg_name_staticrc60, wg_name_staticrc70, wg_name_staticrc80)
    ) AS r2
    UNPIVOT
    (
        tgt_mem_grant_MB FOR tgt_mem_grant_MBs IN (tgt_mem_grant_MB_smallrc,tgt_mem_grant_MB_mediumrc,
        tgt_mem_grant_MB_largerc,tgt_mem_grant_MB_xlargerc, tgt_mem_grant_MB_staticrc10, tgt_mem_grant_MB_staticrc20,
        tgt_mem_grant_MB_staticrc30, tgt_mem_grant_MB_staticrc40, tgt_mem_grant_MB_staticrc50,
        tgt_mem_grant_MB_staticrc60, tgt_mem_grant_MB_staticrc70, tgt_mem_grant_MB_staticrc80)
    ) AS r3
    UNPIVOT
    (
        slots_used FOR slots_used_all IN (slots_used_smallrc,slots_used_mediumrc,slots_used_largerc,
        slots_used_xlargerc, slots_used_staticrc10, slots_used_staticrc20, slots_used_staticrc30,
        slots_used_staticrc40, slots_used_staticrc50, slots_used_staticrc60, slots_used_staticrc70,
        slots_used_staticrc80)
    ) AS r4
) a
WHERE   up1 = up2
AND     up1 = up3
;

-- Getting current info about workload groups.
WITH  
dmv  
AS  
(
  SELECT
          rp.name                                           AS rp_name
  ,       rp.max_memory_kb*1.0/1048576                      AS rp_max_mem_GB
  ,       (rp.max_memory_kb*1.0/1024)
          *(request_max_memory_grant_percent/100)           AS max_memory_grant_MB
  ,       (rp.max_memory_kb*1.0/1048576)
          *(request_max_memory_grant_percent/100)           AS max_memory_grant_GB
  ,       wg.name                                           AS wg_name
  ,       wg.importance                                     AS importance
  ,       wg.request_max_memory_grant_percent               AS request_max_memory_grant_percent
  FROM    sys.dm_pdw_nodes_resource_governor_workload_groups wg
  JOIN    sys.dm_pdw_nodes_resource_governor_resource_pools rp    ON  wg.pdw_node_id  = rp.pdw_node_id
                                                                  AND wg.pool_id      = rp.pool_id
  WHERE   rp.name = 'SloDWPool'
  GROUP BY
          rp.name
  ,       rp.max_memory_kb
  ,       wg.name
  ,       wg.importance
  ,       wg.request_max_memory_grant_percent
)
-- Creating resource class name mapping.
,names
AS
(
  SELECT 'smallrc' as resource_class, 1 as rc_id
  UNION ALL
    SELECT 'mediumrc', 2
  UNION ALL
    SELECT 'largerc', 3
  UNION ALL
    SELECT 'xlargerc', 4
  UNION ALL
    SELECT 'staticrc10', 5
  UNION ALL
    SELECT 'staticrc20', 6
  UNION ALL
    SELECT 'staticrc30', 7
  UNION ALL
    SELECT 'staticrc40', 8
  UNION ALL
    SELECT 'staticrc50', 9
  UNION ALL
    SELECT 'staticrc60', 10
  UNION ALL
    SELECT 'staticrc70', 11
  UNION ALL
    SELECT 'staticrc80', 12
)
,base AS
(   SELECT  schema_name
    ,       table_name
    ,       SUM(column_count)                   AS column_count
    ,       ISNULL(SUM(short_string_column_count),0)   AS short_string_column_count
    ,       ISNULL(SUM(long_string_column_count),0)    AS long_string_column_count
    FROM    (   SELECT  sm.name                                             AS schema_name
                ,       tb.name                                             AS table_name
                ,       COUNT(co.column_id)                                 AS column_count
                           ,       CASE    WHEN co.system_type_id IN (36,43,106,108,165,167,173,175,231,239)
                                AND  co.max_length <= 32
                                THEN COUNT(co.column_id)
                        END                                                 AS short_string_column_count
                ,       CASE    WHEN co.system_type_id IN (165,167,173,175,231,239)
                                AND  co.max_length > 32 and co.max_length <=8000
                                THEN COUNT(co.column_id)
                        END                                                 AS long_string_column_count
                FROM    sys.schemas AS sm
                JOIN    sys.tables  AS tb   on sm.[schema_id] = tb.[schema_id]
                JOIN    sys.columns AS co   ON tb.[object_id] = co.[object_id]
                           WHERE tb.name = @TABLE_NAME AND sm.name = @SCHEMA_NAME
                GROUP BY sm.name
                ,        tb.name
                ,        co.system_type_id
                ,        co.max_length            ) a
GROUP BY schema_name
,        table_name
)
, size AS
(
SELECT  schema_name
,       table_name
,       75497472                                            AS table_overhead

,       column_count*1048576*8                              AS column_size
,       short_string_column_count*1048576*32                       AS short_string_size,       (long_string_column_count*16777216) AS long_string_size
FROM    base
UNION
SELECT CASE WHEN COUNT(*) = 0 THEN 'EMPTY' END as schema_name
         ,CASE WHEN COUNT(*) = 0 THEN 'EMPTY' END as table_name
         ,CASE WHEN COUNT(*) = 0 THEN 0 END as table_overhead
         ,CASE WHEN COUNT(*) = 0 THEN 0 END as column_size
         ,CASE WHEN COUNT(*) = 0 THEN 0 END as short_string_size

,CASE WHEN COUNT(*) = 0 THEN 0 END as long_string_size
FROM   base
)
, load_multiplier as
(
SELECT  CASE
          WHEN FLOOR(8 * (CAST (CAST(REPLACE(REPLACE(@DWU,'DW',''),'c','') AS INT) AS FLOAT)/6000)) > 0
            AND CHARINDEX(@DWU,'c')=0
          THEN FLOOR(8 * (CAST (CAST(REPLACE(REPLACE(@DWU,'DW',''),'c','') AS INT) AS FLOAT)/6000))
          ELSE 1
        END AS multiplication_factor
)
       SELECT  r1.DWU
       , schema_name
       , table_name
       , rc.resource_class as closest_rc_in_increasing_order
       , max_queries_at_this_rc = CASE
             WHEN (r1.max_slots / r1.slots_used > r1.max_queries)
                  THEN r1.max_queries
             ELSE r1.max_slots / r1.slots_used
                  END
       , r1.max_slots as max_concurrency_slots
       , r1.slots_used as required_slots_for_the_rc
       , r1.tgt_mem_grant_MB  as rc_mem_grant_MB
       , CAST((table_overhead*1.0+column_size+short_string_size+long_string_size)*multiplication_factor/1048576    AS DECIMAL(18,2)) AS est_mem_grant_required_for_cci_operation_MB
       FROM    size
       , load_multiplier
       , #ref r1, names  rc
       WHERE r1.rc_id=rc.rc_id
                     AND CAST((table_overhead*1.0+column_size+short_string_size+long_string_size)*multiplication_factor/1048576    AS DECIMAL(18,2)) < r1.tgt_mem_grant_MB
       ORDER BY ABS(CAST((table_overhead*1.0+column_size+short_string_size+long_string_size)*multiplication_factor/1048576    AS DECIMAL(18,2)) - r1.tgt_mem_grant_MB)
GO
```

## <a name="next-steps"></a>Sonraki adımlar

Veritabanı kullanıcılarını ve güvenliğini yönetme hakkında daha fazla bilgi için [SQL Analytics'te veritabanını güvenli](sql-data-warehouse-overview-manage-security.md)olarak ele abakın. Daha büyük kaynak sınıflarının kümelenmiş sütun deposu dizini kalitesini nasıl iyileştirebileceği hakkında daha fazla bilgi [için, sütun deposu sıkıştırma için Bellek optimizasyonlarına](sql-data-warehouse-memory-optimizations-for-columnstore-compression.md)bakın.

