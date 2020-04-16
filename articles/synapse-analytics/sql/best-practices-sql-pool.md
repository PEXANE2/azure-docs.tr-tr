---
title: SQL havuzları için en iyi uygulamalar
description: SQL havuzları ile çalışırken bilmeniz gereken öneriler ve en iyi uygulamalar.
services: synapse-analytics
author: mlee3gsd
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/15/2020
ms.author: martinle
ms.reviewer: igorstan
ms.openlocfilehash: b3dc111fe62cbae857f3369165ba29cf40e90342
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81427802"
---
# <a name="best-practices-for-sql-pools-in-azure-synapse-analytics"></a>Azure Synapse Analytics'teki SQL havuzları için en iyi uygulamalar

Bu makale, Azure Synapse Analytics'teki SQL havuzları için en iyi performansı elde etmenize yardımcı olacak en iyi uygulamalar koleksiyonu sağlar. Aşağıda, çözümünüzü oluştururken odaklanmanız gereken temel rehberlik ve önemli alanlar bulacaksınız. Her bölüm sizi bir kavramla tanıştırır ve daha sonra konsepti daha derinlemesine kapsayan daha ayrıntılı makalelere işaret eder.

## <a name="sql-pools-loading"></a>SQL havuzları yükleme

SQL havuzları yükleme kılavuzu [için veri yükleme kılavuzuna](data-loading-best-practices.md)bakın.

## <a name="reduce-cost-with-pause-and-scale"></a>Duraklatma ve ölçeklendirme ile maliyetleri azaltın

Duraklatma ve ölçekleme yoluyla maliyetleri azaltma hakkında daha fazla bilgi için [bkz.](../sql-data-warehouse/sql-data-warehouse-manage-compute-overview.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)

## <a name="maintain-statistics"></a>İstatistiklerin bakımını yapın

SQL Server sütunlar üzerindeki istatistikleri otomatik olarak algılar ve oluşturur veya güncelleştirir, ancak SQL havuzları istatistiklerin el ile bakımını gerektirir. SQL havuz planlarının optimize edilmiş olduğundan emin olmak için istatistiklerinizi korumak istersiniz.  İyileştirici tarafından oluşturulan planların verimi, istatistiklere bağlıdır.

> [!TIP]
> İstatistik tutmaya başlamanın kolay yollarından biri, her sütun için örnek istatistik oluşturmaktır.  

İstatistiklerin güncelleştirilmesi, verilerinizde yapılan değişiklikler kadar önemlidir.  Garanti yaklaşımlardan biri, istatistiklerinizi her gün veya her yükleme sonrasında güncelleştirmektir.  İstatistiklerin sıfırdan oluşturulması veya mevcut olanların güncelleştirilmesi konusunda karar vermek için performans ve maliyet açısından değerlendirme yapmanız gerekir.

İstatistiklerin bakım süresini kısaltmak için, hangi sütunların istatistikleri olduğu veya en sık güncelleştirmeye ihtiyaç duyulduğu konusunda seçici olun. Örneğin, yeni değerlerin her gün eklenebileceği tarih sütunlarını güncelleştirmek isteyebilirsiniz. Birleştirmelerde yer alan sütunlar, WHERE yan tümcesinde kullanılan sütunlar ve GROUP BY'de bulunan sütunlar için istatistiklere odaklanın.

İstatistikler hakkında ek bilgiler [Yönet tablosu istatistiklerinde](develop-tables-statistics.md)bulunabilir , [İstATİstİkLER OLUŞTUR](/sql/t-sql/statements/create-statistics-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)ve [İstATİstİkMAKALELERİ GÜNCELLE.](/sql/t-sql/statements/update-statistics-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)

## <a name="group-insert-statements-into-batches"></a>INSERT deyimlerini gruplayın

İhtiyaçlarınıza bağlı olarak en iyi yaklaşım olabilir gibi `INSERT INTO MyLookup VALUES (1, 'Type 1')`bir INSERT deyimi ile küçük bir tabloya tek seferlik yük. Ancak, gün boyunca binlerce veya milyonlarca satır yüklemeniz gerekiyorsa, singleton INSERTS'in en iyi şekilde olmaması olasıdır.

Bu sorunu çözmenin bir yolu, bir dosyaya yazan bir işlem geliştirmek ve ardından bu dosyayı düzenli olarak yüklemek için başka bir işlem geliştirmektir. Daha fazla bilgi için [INSERT](/sql/t-sql/statements/insert-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) makalesine bakın.

## <a name="use-polybase-to-load-and-export-data-quickly"></a>Verileri hızlıca yüklemek ve dışarı aktarmak için PolyBase kullanın

SQL havuzu, Azure Veri Fabrikası, PolyBase ve BCP gibi çeşitli araçlar aracılığıyla veri yüklemeyi ve dışa aktarmayı destekler.  Performansın yüksek öneme sahip olmadığı küçük miktarlardaki veriler için bu araçlardan herhangi birini kullanabilirsiniz.  

> [!NOTE]
> Çok temel, büyük hacimli verileri yüklerken veya dışa aktarırken veya daha hızlı performansa ihtiyaç duyarken en iyi seçimdir.

PolyBase yükleri CTAS veya INSERT INTO ile çalıştırılabilir. CTAS işlem günlüğe kaydetmeyi en aza indirir ve verilerinizi yüklemenin en hızlı yoludur. Azure Veri Fabrikası ayrıca PolyBase yüklerini destekler ve CTAS'ye benzer performanslar elde edebilir. PolyBase, Gzip dosyaları da dahil olmak üzere çeşitli dosya biçimlerini destekler.

Gzip metin dosyalarını kullanırken elde etmeyi en üst düzeye çıkarmak için, yükünüzün paralelliğini en üst düzeye çıkarmak için dosyaları 60 veya daha fazla dosyaya ayırın. Toplam hızı artırmak için verilerinizi aynı anda yükleyin. Bu bölümle ilgili konulara ilişkin ek bilgiler aşağıdaki makalelerde yer almaktadır:

- [Veri yükleme](data-loading-overview.md)
- [PolyBase’i kullanma kılavuzu](data-loading-best-practices.md)
- [Azure SQL havuz yükleme desenleri ve stratejileri](https://blogs.msdn.microsoft.com/sqlcat/20../../azure-sql-data-warehouse-loading-patterns-and-strategies/)
- [Azure Data Factory ile veri yükleme](../../data-factory/load-azure-sql-data-warehouse.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)
- [Azure Data Factory ile veri taşıma](../../data-factory/transform-data-using-machine-learning.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)
- [CREATE EXTERNAL FILE FORMAT](/sql/t-sql/statements/create-external-file-format-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)
- [Tabloyu seç olarak oluşturma (CTAS)](../sql-data-warehouse/sql-data-warehouse-develop-ctas.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)

## <a name="load-then-query-external-tables"></a>Dış tabloları önce yükleyip sonra sorgu çalıştırın

Polybase sorgular için en uygun değildir. SQL havuzları için çok temel tablolar şu anda yalnızca Azure blob dosyalarını ve Azure Veri Gölü depolamaalanını destekler. Bu dosyaların onları destekleyen herhangi bir işlem kaynağı yoktur. Sonuç olarak, SQL havuzları bu işi boşaltamaz ve verileri okuyabilmesi için dosyayı tempdb'ye yükleyerek dosyanın tamamını okumalıdır.

Bu verileri sorgulamak için birden fazla sorgunuz varsa, bu verileri bir kez yüklemek ve sorguların yerel tabloyu kullanmasını sağlamak daha iyidir. Daha fazla Polybase kılavuzunda [PolyBase](data-loading-best-practices.md) makalesini kullanmak için eklenmiştir.

## <a name="hash-distribute-large-tables"></a>Büyük tabloları karma olarak dağıtın

Tablolar varsayılan olarak Hepsini Bir Kez Deneme yöntemiyle dağıtılmıştır.   Bu varsayılan değer, kullanıcıların tablolarının nasıl dağıtılacağına karar vermek zorunda kalmadan tablo oluşturmaya başlamalarını kolaylaştırır. Yuvarlak Robin tabloları bazı iş yükleri için yeterince performans gösterebilir. Ancak, çoğu durumda, bir dağıtım sütunu daha iyi performans sağlar.  

Yuvarlak Robin tablosundan daha iyi performans gösteren bir sütun tarafından dağıtılan tablonun en yaygın örneği, iki büyük olgu tablosunun birleştirilmesidir.  

Örneğin, order_id tarafından dağıtılan bir sipariş tablonuz ve order_id tarafından dağıtılan bir hareketler tablonuz varsa, siparişler tablonuzu order_id'daki hareketler tablonuza katıldığınızda, bu sorgu geçiş sorgusu olur. Veri hareketi işlemleri daha sonra ortadan kalkar. Adım sayısı ne kadar az olursa sorgu o kadar hızlı işlenir. Taşınan veri miktarı azaldıkça sorgunun hızı artar.

> [!NOTE]
> Dağıtılmış bir tablo yüklerken, gelen verileriniz dağıtım anahtarında sıralanmamamalıdır. Bunu yapmak yüklerinizi yavaşlatacaktır.

Aşağıda verilen makale bağlantıları, bir dağıtım sütunu seçerek performansı artırma hakkında ek ayrıntılar verecektir. Ayrıca, CREATE TABLE ekstrenizin WITH yan tümcesinde dağıtılmış tablonun nasıl tanımlanacağınız hakkında bilgi bulabilirsiniz:

- [Tabloya genel bakış](develop-tables-overview.md)
- [Tablo dağıtımı](../sql-data-warehouse/sql-data-warehouse-tables-distribute.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)
- [Tablo dağıtımı seçme](https://blogs.msdn.microsoft.com/sqlcat/20../../choosing-hash-distributed-table-vs-round-robin-distributed-table-in-azure-sql-dw-service/)
- [TABLO OLUŞTUR](/sql/t-sql/statements/create-table-azure-sql-data-warehouse?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)
- [CREATE TABLE AS SELECT](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)

## <a name="do-not-over-partition"></a>Aşırı bölümleme yapmayın

Verileri bölümleme, bölüm değiştirme veya taramaları en iyi duruma geçirerek verilerinizi korumak için etkili olabilirken, çok fazla bölüm olması sorgularınızı yavaşlatabilir.  Genellikle SQL Server'da iyi çalışabilecek yüksek parçalılık bölümleme stratejisi SQL havuzunda iyi çalışmayabilir.  

Çok fazla bölüm olması, her bölüm 1 milyondan az satır varsa kümelenmiş sütun deposu dizinlerinin etkinliğini azaltabilir. SQL havuzları verilerinizi otomatik olarak 60 veritabanına bölümler. Yani, 100 bölümlü bir tablo oluşturursanız, sonuç 6000 bölüm olacaktır. Her iş yükü farklıdır, bu nedenle en iyi tavsiye, iş yükünüz için en iyi olanı görmek için bölümleme denemeleri yapmaktır.  

Göz önünde bulundurulması gereken seçeneklerden biri, SQL Server'ı kullanarak uyguladığınız dan daha düşük bir parçalılık kullanmaktır. Örneğin, günlük bölümler yerine haftalık veya aylık bölümler kullanmayı düşünün.

Bölümleme hakkında daha fazla bilgi [Tablo bölümleme](../sql-data-warehouse/sql-data-warehouse-tables-partition.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) makalesinde ayrıntılı olarak açıklanır.

## <a name="minimize-transaction-sizes"></a>İşlem boyutları en aza indirin

InSERT, UPDATE ve DELETE deyimleri bir harekette çalışır. Başarısız olduklarında, geri alınmalıdır. Uzun bir geri alma potansiyelini azaltmak için, mümkün olduğunca işlem boyutlarını en aza indirin.  Hareket boyutlarını en aza indirmek, INSERT, UPDATE ve DELETE ifadelerini parçalara bölerek yapılabilir. Örneğin, 1 saat beklemediğiniz bir INSERT'iniz varsa, INSERT'i dört parçaya ayırabilirsiniz. Her çalışma daha sonra 15 dakikaya kısaltılır.  

> [!TIP]
> Geri alma riskini azaltmak için boş tablolara CTAS, TRUNCATE, DROP TABLE veya INSERT gibi özel Minimal Günlük durumlardan yararlanın.  

Geri alma işlemlerini ortadan kaldırmanın başka bir yöntemi de veri yönetimi için bölüm değiştirme gibi Yalnızca Meta Veri işlemlerini kullanmaktır.  Örneğin, order_date Ekim 2001'de olduğu tablodaki tüm satırları silmek için DELETE deyimi yürütmek yerine, verilerinizi aylık olarak bölümlere aktarabilirsiniz. Daha sonra başka bir tablodan boş bir bölüm için verilerle bölümü değiştirebilirsiniz (ALTER TABLE örneklerine bakın).  

Bölümlenmemiş tablolar için DELETE kullanmak yerine tabloda tutmak istediğiniz verileri yazmak için CTAS kullanmayı düşünün.  Bir CTAS aynı miktarda zaman alıyorsa, minimum işlem günlüğe kaydetmeye sahip olduğundan ve gerekirse hızlı bir şekilde iptal edilebildiği için çalıştırmak çok daha güvenlidir.

Bu bölümle ilgili içerik le ilgili daha fazla bilgi aşağıdaki makalelerde yer almaktadır:

- [Tabloyu seç olarak oluşturma (CTAS)](../sql-data-warehouse/sql-data-warehouse-develop-ctas.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)
- [İşlemler hakkında bilgi sahibi olma](develop-transactions.md)
- [İşlemleri iyileştirme](../sql-data-warehouse/sql-data-warehouse-develop-best-practices-transactions.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)
- [Tablo bölümleme](../sql-data-warehouse/sql-data-warehouse-tables-partition.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)
- [BUKATET TABLOSU](/sql/t-sql/statements/truncate-table-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)
- [ALTER TABLE](/sql/t-sql/statements/alter-table-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)

## <a name="reduce-query-result-sizes"></a>Sorgu sonucu boyutlarını küçültme

Sorgu sonuçları boyutlarını azaltmak, büyük sorgu sonuçlarının neden olduğu istemci tarafı sorunlarını önlemenize yardımcı olur.  Döndürülen satır sayısını azaltmak için sorgunuzu edinebilirsiniz. Bazı sorgu oluşturma araçları, her sorguya "üst N" sözdizimini eklemenize olanak sağlar.  Ayrıca, sorgu sonucunu geçici bir tabloya cetas olarak kullanabilir ve daha sonra alt düzey işleme için PolyBase dışa aktarımını kullanabilirsiniz.

## <a name="use-the-smallest-possible-column-size"></a>Mümkün olan en küçük sütun boyutunu kullanın

DDL'nizi tanımlarken, sorgu performansını artıracağı için verilerinizi destekleyecek en küçük veri türünü kullanın.  Bu öneri özellikle CHAR ve VARCHAR sütunları için önemlidir.  Bir sütundaki en uzun değer 25 karakterse, sütununuzu VARCHAR(25) olarak tanımlayın.  Tüm karakter sütunları için varsayılan uzunluk değeri olarak yüksek bir değer kullanmaktan kaçının.  Ayrıca, NVARCHAR kullanmak yerine gereken tüm olduğunda sütunları VARCHAR olarak tanımlayın.

Yukarıdaki bilgilerle ilgili temel kavramların daha ayrıntılı bir incelemesi için [lütfen Tablogenel bakışı,](develop-tables-overview.md) [Tablo veri türleri](develop-tables-data-types.md)ve CREATE [TABLE](/sql/t-sql/statements/create-table-azure-sql-data-warehouse?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) makalelerine bakın.

## <a name="use-temporary-heap-tables-for-transient-data"></a>Geçiş verileri için geçici yığın tabloları kullanın

SQL havuzlarına geçici olarak veri indirdiğinizde, yığın tablolar genellikle genel işlemi daha hızlı yapar.  Verileri yalnızca daha fazla dönüşüm çalıştırmadan önce aşamalı olarak dolduruyorsanız, tabloyu yığın tabloya yüklemek verileri kümelenmiş bir sütun deposu tablosuna yüklemekten daha hızlı olacaktır.  

Geçici tabloya veri yüklemek de bir tabloyu kalıcı depolamaya yüklemekten çok daha hızlı yüklenir.  Geçici tablolar bir "#" ile başlar ve yalnızca onu oluşturan oturum tarafından erişilebilir. Sonuç olarak, yalnızca sınırlı senaryolarda çalışabilirler. Yığın tabloları, CREATE TABLE deyiminin WITH yan tümcesinde tanımlanır.  Geçici tablo kullanıyorsanız, onun için de istatistik oluşturmayı unutmayın.

Ek kılavuz için [Geçici tablolara](/sql/t-sql/statements/alter-table-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), [CREATE TABLE'a](/sql/t-sql/statements/create-table-azure-sql-data-warehouse?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)ve [SELECT makaleleri olarak TABLO OLUŞTUR'a](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) bakın.

## <a name="optimize-clustered-columnstore-tables"></a>Kümelenmiş columnstore tablolarını iyileştirin

Kümelenmiş sütun deposu dizinleri, verilerinizi SQL havuzunda depolamanın en etkili yollarından biridir.  Varsayılan olarak, SQL havuzundaki tablolar Clustered ColumnStore olarak oluşturulur.  Columnstore tablolarında yapılan sorgularda en iyi performansı elde etmek için segment kalitesinin yüksek olması önemlidir.  Satırlar columnstore tablolarına bellek baskısı altında yazıldığında, segment kalitesi düşebilir.  

Segment kalitesi sıkıştırılmış satır grubundaki satır sayısıyla ölçülebilir. Kümelenmiş sütun deposu tabloları için segment kalitesini algılama ve iyileştirme yle ilgili adım adım yönergeler için [Tablo dizinleri](../sql-data-warehouse/sql-data-warehouse-tables-index.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) makalesinde [zayıf sütun deposu dizin kalitesinin nedenleri'ne](../sql-data-warehouse/sql-data-warehouse-tables-index.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json#causes-of-poor-columnstore-index-quality) bakın.  

Yüksek kaliteli sütun deposu segmentleri önemli olduğundan, veri yüklemek için orta veya büyük kaynak sınıfında bulunan kullanıcı sözcülerini kullanmak iyi bir fikirdir. Daha düşük [veri ambarı birimlerini](resource-consumption-models.md) kullanmak, yükleme kullanıcınıza daha büyük bir kaynak sınıfı atamak istediğiniz anlamına gelir.

Sütun deposu tabloları genellikle tablo başına 1 milyondan fazla satır olana kadar verileri sıkıştırılmış sütun deposu segmentine itmez. Her SQL bilardo tablosu 60 tabloya bölünür. Bu nedenle, sütun deposu tabloları, tabloda 60 milyondan fazla satır yoksa bir sorgudan yararlanamaz.  

> [!TIP]
> 60 milyondan az satırı olan tablolar için, sütun deposu dizinine sahip olmak en uygun çözüm olmayabilir.  

Verilerinizi bölümlere ayırırsanız, kümelenmiş sütun deposu dizininden yararlanabilmek için her bölümün 1 milyon satırolması gerekir.  100 bölümlü bir tablo için, kümelenmiş sütun deposundan (60 dağılım *100 bölüm 1* milyon satır) yararlanabilmesi için en az 6 milyar satırolması gerekir.  

Tablonuzda 6 milyar satır yoksa, iki ana seçeneğiniz vardır. Bölüm sayısını azaltın veya bunun yerine bir yığın tablo kullanmayı düşünün.  Ayrıca, sütun deposu tablosu yerine ikincil dizinleri olan bir yığın tablo kullanılarak daha iyi performans elde edilip edililebildiğini görmek için denemeye değer olabilir.

Columnstore tablosunda çalıştırılan sorgular yalnızca ihtiyacınız olan sütunları seçmeniz halinde daha hızlı olacaktır.  Tablo ve sütun deposu dizinleri hakkında daha fazla bilgi ve [Tablo dizinleri](../sql-data-warehouse/sql-data-warehouse-tables-index.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)içinde bulunabilir , [Sütun mağaza dizinler kılavuzu](/sql/relational-databases/indexes/columnstore-indexes-overview?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), ve Sütun mağaza [dizinleri makaleleri yeniden oluşturma.](../sql-data-warehouse/sql-data-warehouse-tables-index.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest#rebuilding-indexes-to-improve-segment-quality)

## <a name="use-larger-resource-class-to-improve-query-performance"></a>Sorgu performansını artırmak için daha büyük kaynak sınıfı kullanın

SQL havuzları, bellekleri sorgulara ayırmanın bir yolu olarak kaynak gruplarını kullanır. Başlangıçta, tüm kullanıcılar dağıtım başına 100 MB bellek veren küçük kaynak sınıfına atanır.  Her zaman 60 dağıtım vardır. Her dağıtıma en az 100 MB verilir. Sistem genelinde toplam bellek ayırma 6.000 MB veya 6 GB'ın hemen altındadır.  

Büyük birleştirmeler veya kümelenmiş columnstore tablolarına yapılan yüklemeler gibi belirli sorgulara daha fazla bellek atanır.  Saf taramalar gibi bazı sorgular hiçbir fayda görmez. Daha büyük kaynak sınıflarının kullanılması eşzamanlılığı etkiler. Bu nedenle, tüm kullanıcılarınızı büyük bir kaynak sınıfına taşımadan önce bu gerçekleri göz önünde bulundurmak isteyeceksiniz.

Kaynak sınıfları hakkında daha fazla bilgi için iş yükü yönetimi makalesi [için Kaynak sınıflarına](../sql-data-warehouse/resource-classes-for-workload-management.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) bakın.

## <a name="use-smaller-resource-class-to-increase-concurrency"></a>Eşzamanlılığı artırmak için daha küçük kaynak sınıflarını kullanma

Kullanıcı sorgularında uzun bir gecikme fark ederseniz, kullanıcılarınız daha büyük kaynak sınıflarında çalışıyor olabilir. Bu senaryo, diğer sorguların sıraya girebiliyor eşzamanlılık yuvalarının tüketimini teşvik eder.  Kullanıcı sorgularının sıraya girip sıralı olmadığını belirlemek için, herhangi bir satırın döndürülip döndürülmedigini görmek için çalıştırın. `SELECT * FROM sys.dm_pdw_waits`

İş yükü yönetimi ve [sys.dm_pdw_waits](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-waits-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) [makaleleri için Kaynak sınıfları](../sql-data-warehouse/resource-classes-for-workload-management.md) size daha fazla bilgi sağlayacaktır.

## <a name="use-dmvs-to-monitor-and-optimize-your-queries"></a>Sorgularınızı izlemek ve iyileştirmek için DMV’leri kullanın

SQL havuzları sorgu yürütmeizlemek için kullanılabilecek birkaç DMV vardır.  Aşağıdaki izleme makalesi, yürütme sorgusunun ayrıntılarını nasıl görüntüleyirsiniz e-adım yönergeleri ile size yol gösterir.  Bu DMV’lerdeki sorguları hızlıca bulmak için sorgularınızla LABEL seçeneğini kullanabilirsiniz. Daha ayrıntılı bilgi için lütfen aşağıdaki listede yer alan makalelere bakın:

- [DMV’leri kullanarak iş yükünüzü izleme](../sql-data-warehouse/sql-data-warehouse-manage-monitor.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)

- [Etiket](develop-label.md)
- [OPTION](/sql/t-sql/queries/option-clause-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)
- [sys.dm_exec_sessions](/sql/relational-databases/system-dynamic-management-views/sys-dm-exec-sessions-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)
- [sys.dm_pdw_exec_requests](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)
- [sys.dm_pdw_request_steps](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)
- [sys.dm_pdw_sql_requests](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-sql-requests-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)
- [sys.dm_pdw_dms_workers](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-dms-workers-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)
- [DBCC PDW_SHOWEXECUTIONPLAN](/sql/t-sql/database-console-commands/dbcc-pdw-showexecutionplan-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)
- [sys.dm_pdw_waits](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-waits-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)

## <a name="next-steps"></a>Sonraki adımlar

Ayrıca, sık karşılaşılan sorunlar ve çözümler için [Sorun Giderme](../sql-data-warehouse/sql-data-warehouse-troubleshoot.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) makalesine bakın.

Bu makalede sağlanmayan bilgilere ihtiyacınız varsa, tüm SQL havuzu belgelerini aramak için bu sayfanın sol tarafındaki "Doküman ara"yı kullanın.  [SQL havuzu Forumu,](https://social.msdn.microsoft.com/Forums/sqlserver/home?forum=AzureSQLDataWarehouse) diğer kullanıcılara ve SQL havuzu Ürün Grubu'na soru yöneltmeniz için bir yerdir.  

Sorularınızın diğer kullanıcılar veya ekibimiz tarafından yanıtlandığından emin olmak için bu forumu sürekli takip ediyoruz.  Stack Taşma ile ilgili sorularınızı sormayı tercih ederseniz, [Azure SQL havuzu Yığın Taşma Forumu](https://stackoverflow.com/questions/tagged/azure-sqldw)da vardır.

Özellik istekleri için [Azure SQL havuzu Geri Bildirim](https://feedback.azure.com/forums/307516-sql-data-warehouse) sayfasını kullanın.  İsteklerinizi eklemek veya diğer isteklerinizi yukarı oylamak, en çok talep edilen özelliklere odaklanmamıza yardımcı olur.
