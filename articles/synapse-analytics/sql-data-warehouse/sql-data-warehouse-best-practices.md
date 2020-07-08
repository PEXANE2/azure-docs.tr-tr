---
title: Azure SYNAPSE Analytics 'te SYNAPSE SQL havuzu için en iyi Yöntemler (eski adıyla SQL DW)
description: Azure SYNAPSE Analytics 'te (eski adıyla SQL DW) SQL havuzu için çözüm geliştirmeye yönelik öneriler ve en iyi uygulamalar.
services: synapse-analytics
author: mlee3gsd
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql-dw
ms.date: 11/04/2019
ms.author: martinle
ms.reviewer: igorstan
ms.openlocfilehash: 6978855e9b32a3842e76d02ef543d86cf0673019
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85206657"
---
# <a name="best-practices-for-synapse-sql-pool-in-azure-synapse-analytics-formerly-sql-dw"></a>Azure SYNAPSE Analytics 'te SYNAPSE SQL havuzu için en iyi Yöntemler (eski adıyla SQL DW)

Bu makale, [SQL havuzu](sql-data-warehouse-overview-what-is.md) dağıtımından en iyi performansı elde etmenize yardımcı olmak için en iyi uygulamalardan oluşan bir koleksiyondur.  Bu makalenin amacı, size bazı temel rehberlik sunmak ve odağın önemli bölümlerini vurgulayasağlamaktır.  

## <a name="reduce-cost-with-pause-and-scale"></a>Duraklatma ve ölçeklendirme ile maliyetleri azaltın

Duraklatma ve ölçeklendirme aracılığıyla maliyetleri azaltma hakkında daha fazla bilgi için bkz. [İşlem yönetme](sql-data-warehouse-manage-compute-overview.md).

## <a name="maintain-statistics"></a>İstatistiklerin bakımını yapın

SQL havuzu, sütunlarda istatistikleri otomatik olarak algılayacak ve oluşturulacak şekilde yapılandırılabilir.  İyileştirici tarafından oluşturulan sorgu planları yalnızca kullanılabilir istatistiklerle iyidir.  

Sorgularınızda kullanılan sütunlardaki istatistiklerin her zaman güncel olduğundan emin olmak için, veritabanlarınızda AUTO_CREATE_STATISTICS etkinleştirmenizi ve istatistiklerin her bir yükün her gün veya sonrasında güncelleştirilmesini öneririz.

Tüm istatistiklerinizin güncelleştirilmesi çok uzun sürüyor olduğunu fark ederseniz, hangi sütunların sık kullanılan istatistik güncelleştirmelerine ihtiyacı olduğunu daha seçmeli bir şekilde denemek isteyebilirsiniz. Örneğin, yeni değer eklenme ihtimali olan tarih sütunlarını her gün güncelleştirmeyi tercih edebilirsiniz.

> [!TIP]
> Birleşimlerde yer alan sütunlarda, grup tarafından bulunan WHERE yan tümcesinde ve sütunlarda kullanılan sütunlarda güncelleştirilmiş istatistikler bulundurarak en avantaja sahip olursunuz.

Ayrıca bkz. [tablo Istatistiklerini yönetme](sql-data-warehouse-tables-statistics.md), [Istatistik oluşturma](/sql/t-sql/statements/create-statistics-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)ve [güncelleştirme istatistikleri](/sql/t-sql/statements/update-statistics-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest).

## <a name="use-dmvs-to-monitor-and-optimize-your-queries"></a>Sorgularınızı izlemek ve iyileştirmek için DMV’leri kullanın

SQL havuzunda sorgu yürütmeyi izlemek için kullanılabilecek çeşitli DMVs vardır.  DMVs makalesini kullanarak iş yükünüzü Izleme ayrıntıları yürütülen bir sorgunun ayrıntılarına bakmak için adım adım yönergeler.  

Bu DMV’lerdeki sorguları hızlıca bulmak için sorgularınızla LABEL seçeneğini kullanabilirsiniz.

Ayrıca bkz. DMVs, [etiket](sql-data-warehouse-develop-label.md), [seçenek](/sql/t-sql/queries/option-clause-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest), [sys. dm_exec_sessions](/sql/relational-databases/system-dynamic-management-views/sys-dm-exec-sessions-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest), [sys. dm_pdw_exec_requests](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest), [sys. dm_pdw_request_steps](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest), [sys. dm_pdw_sql_requests](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-sql-requests-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest), sys. [dm_pdw_dms_workers](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-dms-workers-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest), [DBCC PDW_SHOWEXECUTIONPLAN](/sql/t-sql/database-console-commands/dbcc-pdw-showexecutionplan-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)ve [sys. dm_pdw_waits](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-waits-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) [kullanarak iş yükünüzü izleme](sql-data-warehouse-manage-monitor.md).

## <a name="tune-query-performance-with-new-product-enhancements"></a>Yeni ürün geliştirmeleriyle sorgu performansını ayarlama

- [Gerçekleştirilmiş görünümler ile performans ayarlama](performance-tuning-materialized-views.md)
- [Sıralı kümelenmiş columnstore dizini ile performans ayarlama](performance-tuning-ordered-cci.md)
- [Sonuç kümesini önbelleğe ile performans ayarlama](performance-tuning-result-set-caching.md)

## <a name="group-insert-statements-into-batches"></a>INSERT deyimlerini gruplayın

Bir INSERT ifadesiyle küçük bir tabloya bir kerelik yükleme veya hatta bir aramanın düzenli olarak yeniden yüklenmesi, gereksinimlerinize göre gereksinimleriniz için iyi bir şekilde çalışabilir `INSERT INTO MyLookup VALUES (1, 'Type 1')` .  

Ancak gün içinde binlerce veya milyonlarca satır yüklemeniz gerekiyorsa, ayrı ayrı INSERT deyimleri gerekli performansı göstermeyebilir.  Bunun yerine işlemlerinizi bir dosyaya yazılacağı ve başka bir işlemin belirli aralıklarla bu dosyayı yükleyeceği şekilde geliştirin.

Ayrıca bkz. [Insert](/sql/t-sql/statements/insert-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest).

## <a name="use-polybase-to-load-and-export-data-quickly"></a>Verileri hızlıca yüklemek ve dışarı aktarmak için PolyBase kullanın

SQL havuzu Azure Data Factory, PolyBase ve BCP gibi çeşitli araçlarla verileri yüklemeyi ve vermeyi destekler.  Performansın yüksek öneme sahip olmadığı küçük miktarlardaki veriler için bu araçlardan herhangi birini kullanabilirsiniz.  Ancak, büyük hacimli verileri yüklerken veya dışarı aktarırken ya da yüksek performansa ihtiyaç duyduğunuzda, PolyBase en iyi çözüm olacaktır.  

PolyBase, MPP (büyük ölçüde paralel Işleme) mimarisinden yararlanmak için tasarlanmıştır ve veri magnitudes diğer araçlardan daha hızlı yüklenip dışarı aktarır.  PolyBase yükleri CTAS veya INSERT INTO ile çalıştırılabilir.  

> [!TIP]
> CTAS, işlem günlüğünü en aza indiren ve verilerinizi en hızlı yükleyen seçenektir.

Azure Data Factory, PolyBase yüklerini da destekler ve CTAS olarak benzer performans elde edebilir.  PolyBase, Gzip de dahil olmak üzere birçok dosya biçimi için destek sunar.  
  
> [!NOTE]
> Gzip metin dosyalarını kullanırken üretilen işi en üst düzeye çıkarmak için, yüklerinizin paralelliğini en üst düzeye çıkarmak üzere dosyaları 60 veya daha fazla dosyaya bölün.  Toplam hızı artırmak için verilerinizi aynı anda yükleyin.

Ayrıca bkz. [veri yükleme](design-elt-data-loading.md), [PolyBase kullanma kılavuzu](guidance-for-loading-data.md), [SQL havuzu yükleme desenleri ve stratejileri](https://blogs.msdn.microsoft.com/sqlcat/20../../), [Azure Data Factory ile VERI yükleme]( ../../data-factory/load-azure-sql-data-warehouse.md), [Azure Data Factory verileri taşıma](../../data-factory/transform-data-using-machine-learning.md), [dış dosya biçimi oluşturma](/sql/t-sql/statements/create-external-file-format-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)ve [Create Table as Select (CTAS)](sql-data-warehouse-develop-ctas.md).

## <a name="load-then-query-external-tables"></a>Dış tabloları önce yükleyip sonra sorgu çalıştırın

Dış tablolar olarak da bilinen Polybase, veri yüklemenin en hızlı yolu olsa da sorgular için en iyi çözüm değildir. PolyBase tabloları Şu anda yalnızca Azure Blob dosyalarını ve Azure Data Lake depolamayı destekler. Bu dosyaları destekleyen herhangi bir işlem kaynağı yoktur.  

Sonuç olarak, SQL havuzu bu çalışmanın yükünü devretmek ve bu nedenle verileri okumak için tempdb 'ye yükleyerek dosyanın tamamını okumalı olmalıdır.  Sonuç olarak bu veriler için birden fazla sorgunuz varsa, verileri bir kez yükleyip sorguların yerel tabloyu kullanmalarını sağlamak daha iyi olacaktır.

Ayrıca bkz. [PolyBase kullanma kılavuzu](guidance-for-loading-data.md).

## <a name="hash-distribute-large-tables"></a>Büyük tabloları karma olarak dağıtın

Tablolar varsayılan olarak Hepsini Bir Kez Deneme yöntemiyle dağıtılmıştır.  Bu durum, kullanıcıların tabloların dağıtma şekli hakkında düşünmelerine gerek kalmadan tablo oluşturmaya başlamalarını sağlar.  Hepsini Bir Kez Deneme tabloları, belirli iş yükleri için yeterli performans sunabilir ancak birçok durumda dağıtım sütunu seçilmesi daha iyi sonuç verecektir.  

Sütuna göre dağıtılmış bir tablonun Hepsini Bir Kez Deneme tablosundan daha iyi performans sunacağı bir örnek, iki büyük bilgi tablosunun birleştirilmesidir.  

Örneğin, order_id ile dağıtılmış bir sipariş tablonuz ve yine order_id ile dağıtılmış bir işlem tablonuz varsa, sipariş tablonuzla işlem tablonuzu order_id üzerinden birleştirdiğinizde, ilgili sorgu bir geçiş sorgusu olur ve veri taşıma işlemleri atlanır.  Adım sayısı ne kadar az olursa sorgu o kadar hızlı işlenir.  Taşınan veri miktarı azaldıkça sorgunun hızı artar.  

> [!TIP]
> Dağıtılmış bir tablo yüklenirken, yükleme işleminin yavaşlamaması için gelen verilerinizin dağıtım anahtarına göre sıralanmamış olduğundan emin olun.  

Dağıtım sütununun nasıl seçileceği hakkında daha fazla bilgi için aşağıdaki bağlantılara bakın ve CREATE TABLE deyimlerinizin WıTH yan tümcesinde dağıtılmış bir tablo tanımlayabilirsiniz.

Ayrıca bkz. [tabloya genel bakış](sql-data-warehouse-tables-overview.md), [tablo dağıtımı](sql-data-warehouse-tables-distribute.md), [tablo DAĞıTıMı seçme](https://blogs.msdn.microsoft.com/sqlcat/20../../choosing-hash-distributed-table-vs-round-robin-distributed-table-in-azure-sql-dw-service/), [Create Table](/sql/t-sql/statements/create-table-azure-sql-data-warehouse?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) [Create Table seçme](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest).

## <a name="do-not-over-partition"></a>Aşırı bölümleme yapmayın

Verilerin bölümlenmesi, bölüm değiştirme veya taramayı en iyi duruma getirme ile, bölüm ile verileri korumak için etkili olsa da, çok fazla bölüm olması sorgularınızı yavaşlatabilir.  Genellikle SQL Server en iyi şekilde çalışan yüksek düzeyde ayrıntı veren bir bölümlendirme stratejisi, SQL havuzunda iyi çalışmayabilir.  

Bölüm sayısının çok fazla olması, her bir bölümdeki satır sayısının 1 milyondan az olması halinde kümelenmiş columnstore dizinlerinin verimini de düşürebilir.  Arka planda SQL havuzunun verilerinizi 60 veritabanlarına bölümleyip, 100 bölümden oluşan bir tablo oluşturursanız bu durum, 6000 bölümlerinde sonuçlanır.  

Her iş yükü farklı olduğundan, en iyi yöntem deneme yanılma ile iş yükünüze en uygun bölümleme şeklini belirlemektir.  SQL Server’da kullandığınızdan daha az parça kullanmayı deneyin.  Örneğin, günlük bölümler yerine haftalık veya aylık bölümler kullanın.

Ayrıca bkz. [tablo bölümleme](sql-data-warehouse-tables-partition.md).

## <a name="minimize-transaction-sizes"></a>İşlem boyutları en aza indirin

Bir işlemde çalışan INSERT, UPDATE ve DELETE deyimleri başarısız olduğunda gerçekleştirilen adımların geri alınması gerekir.  Uzun sürecek bir geri alma işlemi olasılığını en aza indirmek için işlem boyutlarını mümkün oldukça küçültün.  Bunu yapmak için INSERT, UPDATE ve DELETE deyimlerini parçalara ayırabilirsiniz.  

Örneğin, 1 saat beklemeniz beklenen bir EKLEMEMENIZ varsa, her biri 15 dakika içinde çalışacak şekilde, EKLEMEYI dört parçaya bölün.  Geri alma riskini azaltmak için CTAS, TRUNCATE, DROP TABLE veya boş tablolara ekleme gibi çok az sayıda günlüğe kaydetme durumu özelliğinden yararlanın.  

Geri alma işlemlerini ortadan kaldırmanın başka bir yöntemi de veri yönetimi için bölüm değiştirme gibi Yalnızca Meta Veri işlemlerini kullanmaktır.  Örneğin, order_date Ekim 2001 ' de olduğu bir tablodaki tüm satırları silmek için bir DELETE deyimini yürütmek yerine, verilerinizi aylık olarak bölümleyebilir ve sonra bölümü başka bir tablodaki boş bir bölümün verileriyle geçirebilirsiniz (bkz. [alter table](/sql/t-sql/statements/alter-table-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) örnekleri).  

Bölümlenmemiş tablolar için, SILME kullanmak yerine bir tabloda tutmak istediğiniz verileri yazmak üzere bir CTAS kullanmayı düşünün.  Bir CTAS aynı süre alırsa, en az işlem günlüğüne sahip olduğu için çalıştırılması çok daha güvenli bir işlemdir ve gerekirse hızlı bir şekilde iptal edilebilir.

Ayrıca bkz. [Işlemleri anlama](sql-data-warehouse-develop-transactions.md), [işlemleri iyileştirme](sql-data-warehouse-develop-best-practices-transactions.md), [tablo bölümleme](sql-data-warehouse-tables-partition.md), [truncate table](/sql/t-sql/statements/truncate-table-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest), [alter table](/sql/t-sql/statements/alter-table-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)ve [Create Table as Select (CTAS)](sql-data-warehouse-develop-ctas.md).

## <a name="reduce-query-result-sizes"></a>Sorgu sonuç boyutlarını azalt

Bu adım büyük sorgu sonucundan kaynaklanan istemci tarafı sorunlarından kaçınmanıza yardımcı olur.  Döndürülen satır sayısını azaltmak için sorgunuzu düzenleyebilirsiniz. Bazı sorgu oluşturma araçları, her sorguya "ilk N" söz dizimini eklemenize olanak tanır.  Ayrıca sorgu sonucunu geçici bir tabloya CETAS ve sonra alt düzey işleme için PolyBase dışarı aktarmayı kullanabilirsiniz.

## <a name="use-the-smallest-possible-column-size"></a>Mümkün olan en küçük sütun boyutunu kullanın

DDL 'nizi tanımlarken, verilerinizi destekleyecek en küçük veri türü kullanılarak sorgu performansı iyileştirecektir.  Bu yaklaşım özellikle CHAR ve VARCHAR sütunları için önemlidir.  

Bir sütundaki en uzun değer 25 karakterse, sütununuzu VARCHAR(25) olarak tanımlayın.  Tüm karakter sütunları için varsayılan uzunluk değeri olarak yüksek bir değer kullanmaktan kaçının.  Ayrıca, NVARCHAR şart değilse sütunları VARCHAR olarak tanımlayın.

Ayrıca bkz. [tabloya genel bakış](sql-data-warehouse-tables-overview.md), [tablo veri türleri](sql-data-warehouse-tables-data-types.md), [Create Table](/sql/t-sql/statements/create-table-azure-sql-data-warehouse?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest).

## <a name="use-temporary-heap-tables-for-transient-data"></a>Geçiş verileri için geçici yığın tabloları kullanın

Geçici olarak veri giriş yaptığınızda, yığın tablosu kullanmanın genel işlemi daha hızlı hale getirir.  Verileri yalnızca daha fazla dönüştürme gerçekleştirmeden önce hazırlamak için yüklüyorsanız, tabloyu yığın tablosuna yüklemek verileri kümelenmiş bir columnstore tablosuna yüklemekten çok daha hızlı olacaktır.  

Ayrıca verileri geçici bir tabloya yüklemek, tabloyu kalıcı bir depolama alanına yüklemekten çok daha hızlı gerçekleştirilecektir.  Geçici tablolar bir "#" ile başlar ve yalnızca onu oluşturan oturum tarafından erişilebilir, bu nedenle yalnızca sınırlı senaryolarda çalışabilir.

Yığın tabloları, CREATE TABLE deyiminin WITH yan tümcesinde tanımlanır.  Geçici tablo kullanıyorsanız, onun için de istatistik oluşturmayı unutmayın.

Ayrıca bkz. [geçici tablolar](sql-data-warehouse-tables-temporary.md), [Create Table](/sql/t-sql/statements/create-table-azure-sql-data-warehouse?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) [Create Table seçin](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest).

## <a name="optimize-clustered-columnstore-tables"></a>Kümelenmiş columnstore tablolarını iyileştirin

Kümelenmiş columnstore dizinleri, verilerinizi SQL havuzunda depolayabilmeniz için en etkili yöntemlerle biridir.  Varsayılan olarak, SQL havuzundaki tablolar kümelenmiş ColumnStore olarak oluşturulur.  Columnstore tablolarında yapılan sorgularda en iyi performansı elde etmek için segment kalitesinin yüksek olması önemlidir.  

Satırlar columnstore tablolarına bellek baskısı altında yazıldığında, segment kalitesi düşebilir.  Segment kalitesi, sıkıştırılmış Satır Grubu içindeki satır sayısıyla ölçülebilir.  Kümelenmiş columnstore tabloları için segment kalitesini algılamaya ve geliştirmeye yönelik adım adım yönergeler için [tablo dizinleri](sql-data-warehouse-tables-index.md) makalesindeki [kötü columnstore dizin kalitesinin nedenleri](sql-data-warehouse-tables-index.md#causes-of-poor-columnstore-index-quality) bölümüne bakın.  

Yüksek kaliteli columnstore kesimleri önemli olduğundan, verileri yüklemek için orta veya büyük kaynak sınıfında bulunan Kullanıcı kimliklerini kullanmak iyi bir fikirdir. Daha düşük [veri ambarı birimlerinin](what-is-a-data-warehouse-unit-dwu-cdwu.md) kullanılması, yükleme kullanıcıya daha büyük bir kaynak sınıfı atamak istediğiniz anlamına gelir.

Columnstore tabloları, tablo başına 1.000.000 ' den fazla satır olmadığı ve her bir SQL havuzu tablosu 60 tablo olarak bölümlenene kadar genellikle verileri sıkıştırılmış bir columnstore kesimine yerleştirmediği için, bir Thumb kuralı olarak, tablo 60.000.000 ' den fazla satır içermiyorsa, columnstore tabloları bir sorgu avantajına sahip değildir.  60 milyondan az satıra sahip tablolarda columnstore dizini kullanmaya gerek olmayabilir.  Kullanmanın da bir zararı olmayacaktır.  

Ayrıca, verilerinizi bölümlemeniz halinde her bir bölümün kümelenmiş columnstore dizini kullanabilmesi için en az 1 milyon satıra ihtiyaç duyacağını unutmayın.  Bir tabloda 100 bölüm varsa, kümelenmiş bir sütun deposundan faydalanmak için en az 6.000.000.000 satıra sahip olması gerekir (60 dağıtımları *100 bölümleri* 1.000.000 satırları).  

Bu örnekte tablonuzda 6 milyar satır yoksa, bölüm sayısını azaltabilir veya yığın tablo kullanabilirsiniz.  Deneme yaparak columnstore tablosu yerine ikincil dizine sahip yığın tablo ile daha iyi performans elde edip etmeyeceğinizi görebilirsiniz.

> [!TIP]
> Columnstore tablosunda çalıştırılan sorgular yalnızca ihtiyacınız olan sütunları seçmeniz halinde daha hızlı olacaktır.  

Ayrıca bkz. [Tablo dizinleri](sql-data-warehouse-tables-index.md), [Columnstore dizinleri kılavuzu](/sql/relational-databases/indexes/columnstore-indexes-overview?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest), [Columnstore dizinlerini yeniden oluşturma](sql-data-warehouse-tables-index.md#rebuilding-indexes-to-improve-segment-quality)

## <a name="use-larger-resource-class-to-improve-query-performance"></a>Sorgu performansını artırmak için daha büyük kaynak sınıfı kullanın

SQL havuzu, kaynak gruplarını sorgulara bellek ayırmak için bir yol olarak kullanır.  Kullanıma hazır olan tüm kullanıcılar, dağıtım başına 100 MB bellek veren küçük kaynak sınıfına atanır.  Her zaman 60 dağıtım olduğundan ve her dağıtıma en az 100 MB verildiğinden, sistem genelinde ayrılan bellek toplam 6000 MB (yaklaşık 6 GB) olur.  

Büyük birleştirmeler veya kümelenmiş columnstore tablolarına yapılan yüklemeler gibi belirli sorgulara daha fazla bellek atanır.  Saf taramalar gibi bazı sorgular hiçbir avantaj vermez.  Ancak, daha büyük kaynak sınıfları kullanmak eşzamanlılık azaltır, bu nedenle tüm kullanıcılarınızı büyük bir kaynak sınıfına taşımadan önce bu etkiyi göz önünde bulundurmanız gerekir.

Ayrıca bkz. [iş yükü yönetimi Için kaynak sınıfları](resource-classes-for-workload-management.md).

## <a name="use-smaller-resource-class-to-increase-concurrency"></a>Eşzamanlılığı artırmak için daha küçük kaynak sınıfı kullanın

Kullanıcı sorgularının uzun bir gecikme olduğunu fark ederseniz, kullanıcılarınız daha büyük kaynak sınıflarında çalışıyor ve diğer sorguların sıraya çıkmasına neden olan birçok eşzamanlılık yuvası kullanıyor olabilir.  Kullanıcı sorgularının kuyrukta olup olmadığını görmek için `SELECT * FROM sys.dm_pdw_waits` çalıştırıp dönen satırlara bakın.

Ayrıca bkz. [iş yükü yönetimi Için kaynak sınıfları](resource-classes-for-workload-management.md), [sys. dm_pdw_waits](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-waits-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest).

## <a name="other-resources"></a>Diğer kaynaklar

Genel sorunlar ve çözümleri için [Sorun giderme](sql-data-warehouse-troubleshoot.md) makalemizi de inceleyin.

Bu makalede aradığınızı bulamazsanız, tüm Azure SYNAPSE belgelerini aramak için bu sayfanın sol tarafındaki "belgeleri ara" seçeneğini kullanmayı deneyin.  [Azure SYNAPSE Için Microsoft Q&soru sayfası](https://docs.microsoft.com/answers/topics/azure-synapse-analytics.html) , diğer kullanıcılara ve Azure SYNAPSE ürün grubuna soru göndermek için kullanabileceğiniz bir yerdir. Sorularınızın diğer kullanıcılar veya ekibimiz tarafından yanıtlandığından emin olmak için bu forumu sürekli takip ediyoruz.  

Stack Overflow sorularınızı sormak isterseniz, [Azure Synapse Stack Overflow Forumumuzu](https://stackoverflow.com/questions/tagged/azure-sqldw)da sunuyoruz.

Özellik istekleri yapmak için lütfen [Azure SYNAPSE geri bildirim](https://feedback.azure.com/forums/307516-sql-data-warehouse) sayfasını kullanın.  İsteklerinizi eklemeniz veya diğer istekleri oylamanız, özellikleri önceliklendirme konusunda bize yardımcı olmaktadır.
