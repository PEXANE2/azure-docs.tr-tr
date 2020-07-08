---
title: SQL havuzları için en iyi uygulamalar
description: SQL havuzlarıyla çalışırken bilmeniz gereken öneriler ve en iyi uygulamalar.
services: synapse-analytics
author: mlee3gsd
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql
ms.date: 04/15/2020
ms.author: martinle
ms.reviewer: igorstan
ms.openlocfilehash: 8483fd2a1b33330b868fb21d71922377e906e6c8
ms.sourcegitcommit: 845a55e6c391c79d2c1585ac1625ea7dc953ea89
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2020
ms.locfileid: "85958430"
---
# <a name="best-practices-for-sql-pools-in-azure-synapse-analytics"></a>Azure SYNAPSE Analytics 'te SQL havuzları için en iyi yöntemler

Bu makalede, Azure SYNAPSE Analytics 'te SQL havuzları için en iyi performansı elde etmenize yardımcı olacak en iyi yöntemler koleksiyonu sunulmaktadır. Aşağıda, çözümünüzü oluştururken odaklanmanız gereken temel kılavuz ve önemli alanların bulabilirsiniz. Her bölüm sizi kavram halinde tanıtarak kavramı daha ayrıntılı bir şekilde kapsayan daha ayrıntılı makalelere yönlendirir.

## <a name="sql-pools-loading"></a>SQL havuzları yükleniyor

SQL havuzları Yükleme Kılavuzu için bkz. [veri yükleme kılavuzu](data-loading-best-practices.md).

## <a name="reduce-cost-with-pause-and-scale"></a>Duraklatma ve ölçeklendirme ile maliyetleri azaltın

Duraklatma ve ölçeklendirme ile maliyetleri azaltma hakkında daha fazla bilgi için bkz. [Manage COMPUTE](../sql-data-warehouse/sql-data-warehouse-manage-compute-overview.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json).

## <a name="maintain-statistics"></a>İstatistiklerin bakımını yapın

SQL Server, sütunlardaki istatistikleri otomatik olarak algılayıp oluşturduğunda veya güncelleştirirken, SQL havuzları istatistiklerin el ile bakımını gerektirir. SQL havuz planlarının iyileştirildiğinden emin olmak için istatistiklerinizi korumak isteyeceksiniz.  İyileştirici tarafından oluşturulan planların verimi, istatistiklere bağlıdır.

> [!TIP]
> İstatistik tutmaya başlamanın kolay yollarından biri, her sütun için örnek istatistik oluşturmaktır.  

İstatistiklerin güncelleştirilmesi, verilerinizde yapılan değişiklikler kadar önemlidir.  Garanti yaklaşımlardan biri, istatistiklerinizi her gün veya her yükleme sonrasında güncelleştirmektir.  İstatistiklerin sıfırdan oluşturulması veya mevcut olanların güncelleştirilmesi konusunda karar vermek için performans ve maliyet açısından değerlendirme yapmanız gerekir.

İstatistik bakım süresini kısaltmak için, hangi sütunların istatistikte olduğunu veya en sık güncelleştirmenin gerekli olduğunu seçerek. Örneğin, yeni değerlerin her gün eklenebileceği Tarih sütunlarını güncelleştirmek isteyebilirsiniz. Birleşimlerde yer alan sütunlar, WHERE yan tümcesinde kullanılan sütunlar ve GROUP BY içinde bulunan sütunlar için istatistik olmaya odaklanmaya odaklanın.

İstatistiklerle ilgili ek bilgilere [tablo Istatistiklerini yönetme](develop-tables-statistics.md), [ISTATISTIK oluşturma](/sql/t-sql/statements/create-statistics-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)ve [istatistikleri güncelleştirme](/sql/t-sql/statements/update-statistics-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) makaleleri bulabilirsiniz.

## <a name="group-insert-statements-into-batches"></a>INSERT deyimlerini gruplayın

Küçük bir tabloya bir INSERT ifadesiyle bir kez yükleme `INSERT INTO MyLookup VALUES (1, 'Type 1')` , gereksinimlerinize bağlı olarak en iyi yaklaşım olabilir. Ancak, günde binlerce veya milyonlarca satır yüklemeniz gerekiyorsa, bu büyük olasılıkla tek ekleme en uygun değildir.

Bu sorunu çözmenin bir yolu, bir dosyaya yazan bir işlem geliştirmektir ve bu dosyayı düzenli olarak yüklemek için başka bir işlemdir. Daha fazla bilgi için [ekleme](/sql/t-sql/statements/insert-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) makalesine bakın.

## <a name="use-polybase-to-load-and-export-data-quickly"></a>Verileri hızlıca yüklemek ve dışarı aktarmak için PolyBase kullanın

SQL havuzu Azure Data Factory, PolyBase ve BCP gibi çeşitli araçlarla verileri yüklemeyi ve vermeyi destekler.  Performansın yüksek öneme sahip olmadığı küçük miktarlardaki veriler için bu araçlardan herhangi birini kullanabilirsiniz.  

> [!NOTE]
> PolyBase, büyük hacimli verileri yüklerken veya verirken en iyi seçenektir veya daha hızlı bir performansa ihtiyacınız vardır.

PolyBase yükleri CTAS veya INSERT INTO ile çalıştırılabilir. CTAS, işlem günlüğünü en aza indirir ve verilerinizi yüklemek için en hızlı yoldur. Azure Data Factory, PolyBase yüklerini da destekler ve CTAS 'ya benzer bir performans elde edebilir. PolyBase, gzip dosyaları dahil olmak üzere çeşitli dosya biçimlerini destekler.

Gzip metin dosyalarını kullanırken üretilen işi en üst düzeye çıkarmak için, yüklerinizin paralelliğini en üst düzeye çıkarmak üzere dosyaları 60 veya daha fazla dosyaya bölün. Toplam hızı artırmak için verilerinizi aynı anda yükleyin. Bu bölümle ilgili konular için ek bilgiler aşağıdaki makalelere eklenmiştir:

- [Veri yükleme](../sql-data-warehouse/design-elt-data-loading.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)
- [PolyBase’i kullanma kılavuzu](data-loading-best-practices.md)
- [Azure SQL havuzu yükleme desenleri ve stratejileri](https://blogs.msdn.microsoft.com/sqlcat/20../../azure-sql-data-warehouse-loading-patterns-and-strategies/)
- [Azure Data Factory ile veri yükleme](../../data-factory/load-azure-sql-data-warehouse.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)
- [Azure Data Factory ile veri taşıma](../../data-factory/transform-data-using-machine-learning.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)
- [CREATE EXTERNAL FILE FORMAT](/sql/t-sql/statements/create-external-file-format-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)
- [Create Table as Select (CTAS)](../sql-data-warehouse/sql-data-warehouse-develop-ctas.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)

## <a name="load-then-query-external-tables"></a>Dış tabloları önce yükleyip sonra sorgu çalıştırın

PolyBase sorgular için ideal değildir. SQL havuzları için PolyBase tabloları Şu anda yalnızca Azure Blob dosyalarını ve Azure Data Lake depolamayı destekler. Bu dosyalarda, onları yedekleyen işlem kaynakları yoktur. Sonuç olarak, SQL havuzları bu çalışmanın yükünü boşaltarak verileri okuyabilmesi için tempdb 'ye yükleyerek dosyanın tamamını okummalıdır.

Bu verileri sorgulamak için birkaç sorgunuz varsa, bu verilerin bir kez yüklenmesi ve sorguların yerel tabloyu kullanması daha iyidir. Ek PolyBase Kılavuzu, [PolyBase makalesini kullanmaya yönelik kılavuza](data-loading-best-practices.md) dahildir.

## <a name="hash-distribute-large-tables"></a>Büyük tabloları karma olarak dağıtın

Tablolar varsayılan olarak Hepsini Bir Kez Deneme yöntemiyle dağıtılmıştır.   Bu varsayılan değer, kullanıcıların tablolarının nasıl dağıtılacağına karar vermeden, tabloların tablo oluşturmaya başlamasını kolaylaştırır. Hepsini bir kez deneme tablosu, bazı iş yükleri için yeterince sürebilir. Ancak, çoğu durumda bir dağıtım sütunu daha iyi performans sağlar.  

Hepsini bir kez deneme tablosu tarafından dağıtılan bir tablonun en yaygın örneği, iki büyük olgu tablosunun birleştirilmesinden oluşur.  

Örneğin, order_id tarafından dağıtılan bir Siparişler tablonuz ve order_id tarafından dağıtılan bir işlem tablosu varsa, siparişler tablonuzu order_id işlem tablonuza eklediğinizde, bu sorgu doğrudan bir sorgu haline gelir. Veri taşıma işlemleri daha sonra ortadan kaldırılır. Adım sayısı ne kadar az olursa sorgu o kadar hızlı işlenir. Taşınan veri miktarı azaldıkça sorgunun hızı artar.

> [!NOTE]
> Dağıtılmış bir tablo yüklenirken, gelen verileriniz dağıtım anahtarında sıralanmamalıdır. Bunun yapılması, yüklerinizi yavaşlatır.

Aşağıda sunulan makale bağlantıları, bir dağıtım sütunu seçerek performansı iyileştirmeye ilişkin ek ayrıntılar verecektir. Ayrıca, CREATE TABLE deyiminizi WıTH yan tümcesinde dağıtılmış bir tablo tanımlama hakkında bilgi bulacaksınız:

- [Tabloya genel bakış](develop-tables-overview.md)
- [Tablo dağıtımı](../sql-data-warehouse/sql-data-warehouse-tables-distribute.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)
- [Tablo dağıtımı seçme](https://blogs.msdn.microsoft.com/sqlcat/20../../choosing-hash-distributed-table-vs-round-robin-distributed-table-in-azure-sql-dw-service/)
- [CREATE TABLE](/sql/t-sql/statements/create-table-azure-sql-data-warehouse?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)
- [CREATE TABLE AS SELECT](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)

## <a name="do-not-over-partition"></a>Aşırı bölümleme yapmayın

Verilerin bölümlenmesi, bölüm değiştirme veya taramayı en iyi duruma getirme ile, bölüm ile verileri korumak için etkili olsa da, çok fazla bölüm olması sorgularınızı yavaşlatabilir.  Genellikle SQL Server en iyi şekilde çalışan yüksek düzeyde parçalı bölümleme stratejisi, SQL havuzunda iyi çalışmayabilir.  

Her bölümde 1.000.000 'den az satır varsa, çok fazla bölüm olması kümelenmiş columnstore dizinlerinin verimliliğini azaltabilir. SQL havuzları, verilerinizi otomatik olarak 60 veritabanlarına bölümleyebilir. Bu nedenle, 100 bölümlü bir tablo oluşturursanız sonuç 6000 bölüm olacaktır. Her iş yükü farklıdır, bu nedenle en iyi öneri, iş yükünüz için en iyi şeyi görmek üzere bölümlendirme ile denemeler sağlamaktır.  

Göz önünde bulundurulması gereken bir seçenek, SQL Server kullanarak uyguladıklarınızı daha düşük bir ayrıntı düzeyi kullanmaktır. Örneğin, günlük bölümler yerine haftalık veya aylık bölümler kullanmayı göz önünde bulundurun.

Bölümlendirme hakkında daha fazla bilgi [tablo bölümlendirme](../sql-data-warehouse/sql-data-warehouse-tables-partition.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) makalesinde ayrıntılı olarak açıklanmıştır.

## <a name="minimize-transaction-sizes"></a>İşlem boyutları en aza indirin

Bir işlemde çalıştırılan INSERT, UPDATE ve DELETE deyimleri. Başarısız olduklarında geri alınması gerekir. Uzun geri alma potansiyelini azaltmak için mümkün olan her durumda işlem boyutlarını en aza indirin.  INSERT, UPDATE ve DELETE deyimlerini parçalara ayırarak işlem boyutlarının en aza küçültülmesi yapılabilir. Örneğin, 1 saat beklemeniz beklenen bir INSERT 'e sahipseniz, INSERT ' i dört parçaya kesebilirsiniz. Her çalıştırma daha sonra 15 dakikaya kısaltılacaktır.  

> [!TIP]
> Geri alma riskini azaltmak için CTAS, TRUNCATE, DROP TABLE veya boş tablolara ekleme gibi çok az sayıda günlüğe kaydetme durumu özelliğinden yararlanın.  

Geri alma işlemlerini ortadan kaldırmanın başka bir yöntemi de veri yönetimi için bölüm değiştirme gibi Yalnızca Meta Veri işlemlerini kullanmaktır.  Örneğin, order_date Ekim 2001 ' de olduğu bir tablodaki tüm satırları silmek için bir DELETE ifadesini yürütmek yerine, verilerinizi aylık olarak bölümleyebilirsiniz. Daha sonra, başka bir tablodaki boş bir bölüm için verileri kullanarak bölümü geçirebilirsiniz (bkz. ALTER TABLE örnekleri).  

Bölümlenmemiş tablolar için, SILME kullanmak yerine bir tabloda tutmak istediğiniz verileri yazmak üzere bir CTAS kullanmayı düşünün.  Bir CTAS aynı süre alırsa, en az işlem günlüğü içerdiğinden ve gerekirse hızlı bir şekilde iptal edebileceğinizden, çalıştırılması çok daha güvenlidir.

Bu bölümle ilgili içerik hakkında daha fazla bilgi aşağıdaki makalelere eklenmiştir:

- [Create Table as Select (CTAS)](../sql-data-warehouse/sql-data-warehouse-develop-ctas.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)
- [İşlemler hakkında bilgi sahibi olma](develop-transactions.md)
- [İşlemleri iyileştirme](../sql-data-warehouse/sql-data-warehouse-develop-best-practices-transactions.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)
- [Tablo bölümleme](../sql-data-warehouse/sql-data-warehouse-tables-partition.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)
- [TRUNCATE TABLE](/sql/t-sql/statements/truncate-table-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)
- [ALTER TABLE](/sql/t-sql/statements/alter-table-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)

## <a name="reduce-query-result-sizes"></a>Sorgu sonuç boyutlarını azalt

Sorgu sonuçları boyutlarının azaltılması, büyük sorgu sonuçlarının neden olduğu istemci tarafı sorunlarından kaçınmanıza yardımcı olur.  Döndürülen satır sayısını azaltmak için sorgunuzu düzenleyebilirsiniz. Bazı sorgu oluşturma araçları, her sorguya "ilk N" söz dizimini eklemenize olanak tanır.  Ayrıca sorgu sonucunu geçici bir tabloya CETAS ve sonra alt düzey işleme için PolyBase dışarı aktarmayı kullanabilirsiniz.

## <a name="use-the-smallest-possible-column-size"></a>Mümkün olan en küçük sütun boyutunu kullanın

DDL 'nizi tanımlarken, bunu yaparken verilerinizi destekleyecek en küçük veri türünü kullanın, sorgu performansını geliştirir.  Bu öneri, özellikle CHAR ve VARCHAR sütunları için önemlidir.  Bir sütundaki en uzun değer 25 karakterse, sütununuzu VARCHAR(25) olarak tanımlayın.  Tüm karakter sütunları için varsayılan uzunluk değeri olarak yüksek bir değer kullanmaktan kaçının.  Ayrıca, NVARCHAR kullanmak yerine, sütunları VARCHAR olarak tanımlayın.

Yukarıdaki bilgilerle ilgili önemli kavramların daha ayrıntılı incelemesi için bkz. [tabloya genel bakış](develop-tables-overview.md), [tablo veri türleri](develop-tables-data-types.md)ve [Create Table](/sql/t-sql/statements/create-table-azure-sql-data-warehouse?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) makaleleri.

## <a name="use-temporary-heap-tables-for-transient-data"></a>Geçiş verileri için geçici yığın tabloları kullanın

SQL havuzlarındaki verileri geçici olarak belirlediğinizde, yığın tabloları genellikle genel işlemi daha hızlı hale getirir.  Verileri yalnızca daha fazla dönüşüm çalıştırmadan önce hazırlamak için yüklüyorsanız, tablonun bir yığın tablosuna yüklenmesi, verileri kümelenmiş bir columnstore tablosuna yüklemeden daha hızlı olur.  

Verileri geçici bir tabloya yüklemek, bir tabloyu kalıcı depolamaya yüklemeden çok daha hızlı da yüklenir.  Geçici tablolar bir "#" ile başlar ve yalnızca onu oluşturan oturum tarafından erişilebilir. Sonuç olarak, bunlar yalnızca sınırlı senaryolarda çalışabilir. Yığın tabloları, CREATE TABLE deyiminin WITH yan tümcesinde tanımlanır.  Geçici tablo kullanıyorsanız, onun için de istatistik oluşturmayı unutmayın.

Ek rehberlik için, makale [Seç olarak](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) [geçici tablolara](/sql/t-sql/statements/alter-table-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), [Create Table](/sql/t-sql/statements/create-table-azure-sql-data-warehouse?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)ve Create Table bakın.

## <a name="optimize-clustered-columnstore-tables"></a>Kümelenmiş columnstore tablolarını iyileştirin

Kümelenmiş columnstore dizinleri, verilerinizi SQL havuzunda depolayabilmeniz için en etkili yöntemlerle biridir.  Varsayılan olarak, SQL havuzundaki tablolar kümelenmiş ColumnStore olarak oluşturulur.  Columnstore tablolarında yapılan sorgularda en iyi performansı elde etmek için segment kalitesinin yüksek olması önemlidir.  Satırlar columnstore tablolarına bellek baskısı altında yazıldığında, segment kalitesi düşebilir.  

Segment kalitesi, sıkıştırılmış bir satır grubundaki satır sayısıyla ölçülebilir. Kümelenmiş columnstore tabloları için segment kalitesini algılamaya ve geliştirmeye yönelik adım adım yönergeler için [tablo dizinleri](../sql-data-warehouse/sql-data-warehouse-tables-index.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) makalesindeki [kötü columnstore dizin kalitesinin nedenleri](../sql-data-warehouse/sql-data-warehouse-tables-index.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json#causes-of-poor-columnstore-index-quality) bölümüne bakın.  

Yüksek kaliteli columnstore kesimleri önemli olduğundan, verileri yüklemek için orta veya büyük kaynak sınıfında bulunan Kullanıcı kimliklerini kullanmak iyi bir fikirdir. Daha düşük [veri ambarı birimlerinin](resource-consumption-models.md) kullanılması, yükleme kullanıcıya daha büyük bir kaynak sınıfı atamak istediğiniz anlamına gelir.

Columnstore tabloları genellikle tablo başına 1.000.000 ' den fazla satır olana kadar verileri sıkıştırılmış bir columnstore kesimine göndermez. Her SQL havuzu tablosu 60 tablo halinde bölümlenir. Bu nedenle, tablo 60.000.000 ' den fazla satır içermiyorsa, columnstore tabloları bir sorgu avantajına sahip olmaz.  

> [!TIP]
> 60.000.000 ' den az satır içeren tablolar için bir columnstore dizinine sahip olmak en iyi çözüm olmayabilir.  

Verilerinizi bölümleyebilirsiniz, her bölümün bir kümelenmiş columnstore dizininden faydalanabilir 1.000.000 satıra sahip olması gerekir.  100 bölümlü bir tablo için, kümelenmiş bir sütun deposundan faydalanmak için en az 6.000.000.000 satıra sahip olması gerekir (60 dağıtımları *100 bölümleri* 1.000.000 satırları).  

Tablonuzun 6.000.000.000 satırı yoksa, iki ana seçeneğiniz vardır. Bölüm sayısını azaltın ya da bunun yerine bir yığın tablosu kullanmayı deneyin.  Ayrıca, bir columnstore tablosu yerine ikincil dizinlerle bir yığın tablosu kullanılarak daha iyi bir performans kazanılabilir.

Columnstore tablosunda çalıştırılan sorgular yalnızca ihtiyacınız olan sütunları seçmeniz halinde daha hızlı olacaktır.  Tablo ve columnstore dizinleri hakkında daha fazla bilgi ve [tablo dizinleri](../sql-data-warehouse/sql-data-warehouse-tables-index.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json), [columnstore dizinleri Kılavuzu](/sql/relational-databases/indexes/columnstore-indexes-overview?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), [columnstore dizinleri](../sql-data-warehouse/sql-data-warehouse-tables-index.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest#rebuilding-indexes-to-improve-segment-quality) makalelerini yeniden derleme içinde bulunabilir.

## <a name="use-larger-resource-class-to-improve-query-performance"></a>Sorgu performansını artırmak için daha büyük kaynak sınıfı kullanın

SQL havuzları, kaynak gruplarını sorgulara bellek ayırmak için bir yol olarak kullanır. Başlangıçta, tüm kullanıcılar, dağıtım başına 100 MB bellek veren küçük kaynak sınıfına atanır.  Her zaman 60 dağıtımları vardır. Her dağıtıma en az 100 MB verilir. Sistem genelinde toplam bellek ayırma 6.000 MB 'tır veya yalnızca 6 GB altındadır.  

Büyük birleştirmeler veya kümelenmiş columnstore tablolarına yapılan yüklemeler gibi belirli sorgulara daha fazla bellek atanır.  Saf taramalar gibi bazı sorgular hiçbir avantaj görmez. Daha büyük kaynak sınıflarının kullanılmasıyla eşzamanlılık etkiler. Bu nedenle, tüm kullanıcılarınızı büyük bir kaynak sınıfına taşımadan önce bu olguları aklınızda tutmanız gerekir.

Kaynak sınıfları hakkında daha fazla bilgi için [iş yükü yönetimi Için kaynak sınıfları](../sql-data-warehouse/resource-classes-for-workload-management.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) makalesine bakın.

## <a name="use-smaller-resource-class-to-increase-concurrency"></a>Eşzamanlılık artırma için daha küçük kaynak sınıfı kullanın

Kullanıcı sorgularında uzun bir gecikme fark ederseniz, kullanıcılarınız daha büyük kaynak sınıflarında çalışıyor olabilir. Bu senaryo, diğer sorguların sıraya alınmasına neden olabilecek eşzamanlılık yuvaları tüketimini yükseltir.  Kullanıcı sorgularının sıraya alınıp döndürülmeyeceğini anlamak için, `SELECT * FROM sys.dm_pdw_waits` bir satırın döndürülüp döndürülmediğine bakmak için öğesini çalıştırın.

İş yükü yönetimi ve [sys. dm_pdw_waits](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-waits-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) makalelerine [yönelik kaynak sınıfları](../sql-data-warehouse/resource-classes-for-workload-management.md) size daha fazla bilgi sağlayacaktır.

## <a name="use-dmvs-to-monitor-and-optimize-your-queries"></a>Sorgularınızı izlemek ve iyileştirmek için DMV’leri kullanın

SQL havuzlarının sorgu yürütmeyi izlemek için kullanılabilecek çeşitli DMVs 'ler vardır.  Aşağıdaki izleme makalesi, yürütülen bir sorgunun ayrıntılarının nasıl görüntüleneceği hakkında adım adım yönergeler sağlar.  Bu DMV’lerdeki sorguları hızlıca bulmak için sorgularınızla LABEL seçeneğini kullanabilirsiniz. Daha ayrıntılı bilgi için lütfen aşağıdaki listeye eklenen makalelere bakın:

- [DMV’leri kullanarak iş yükünüzü izleme](../sql-data-warehouse/sql-data-warehouse-manage-monitor.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)

- [ETIKETIN](develop-label.md)
- [OPTION](/sql/t-sql/queries/option-clause-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)
- [sys.dm_exec_sessions](/sql/relational-databases/system-dynamic-management-views/sys-dm-exec-sessions-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)
- [sys.dm_pdw_exec_requests](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)
- [sys.dm_pdw_request_steps](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)
- [sys.dm_pdw_sql_requests](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-sql-requests-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)
- [sys.dm_pdw_dms_workers](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-dms-workers-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)
- [DBCC PDW_SHOWEXECUTIONPLAN](/sql/t-sql/database-console-commands/dbcc-pdw-showexecutionplan-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)
- [sys.dm_pdw_waits](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-waits-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)

## <a name="next-steps"></a>Sonraki adımlar

Ayrıca yaygın sorunlar ve çözümleri için [sorun giderme](../sql-data-warehouse/sql-data-warehouse-troubleshoot.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) makalesine bakın.

Bu makalede sağlanmış bilgilere ihtiyacınız varsa, [Azure SYNAPSE Için Microsoft Q&soru sayfasında](https://docs.microsoft.com/answers/topics/azure-synapse-analytics.html) , diğer KULLANıCıLARA ve SQL havuzu ürün grubuna yönelik sorular oluşturabilmeniz için bir sorun olup olmadığını arayın.  

Sorularınızın diğer kullanıcılar veya ekibimiz tarafından yanıtlandığından emin olmak için bu forumu sürekli takip ediyoruz.  Stack Overflow sorularınızı sormayı tercih ediyorsanız [Azure SQL havuzu Stack Overflow forumumuz](https://stackoverflow.com/questions/tagged/azure-sqldw)de vardır.

Özellik istekleri için [Azure SQL havuzu geri bildirim](https://feedback.azure.com/forums/307516-sql-data-warehouse) sayfasını kullanın.  İsteklerinizi veya oylama diğer isteklerinizi eklemek, en fazla isteğe bağlı özelliklere odaklanmamıza yardımcı olur.
