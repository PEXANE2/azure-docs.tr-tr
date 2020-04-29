---
title: SYNAPSE SQL için geliştirme en iyi yöntemleri
description: SYNAPSE SQL için geliştirme yaparken bilmeniz gereken öneriler ve en iyi uygulamalar.
services: synapse-analytics
author: XiaoyuMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/15/2020
ms.author: xiaoyul
ms.reviewer: igorstan
ms.openlocfilehash: ed2638cfe4ab7e849e428729ccd17ffdeb6314af
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82086360"
---
# <a name="development-best-practices-for-synapse-sql"></a>SYNAPSE SQL için geliştirme en iyi yöntemleri
Bu makalede, veri ambarı Çözümünüzü geliştirirken rehberlik ve en iyi uygulamalar açıklanmaktadır. 

## <a name="sql-pool-development-best-practices"></a>SQL havuzu geliştirme en iyi uygulamaları

### <a name="reduce-cost-with-pause-and-scale"></a>Duraklatma ve ölçeklendirme ile maliyetleri azaltın

Duraklatma ve ölçeklendirme aracılığıyla maliyetleri azaltma hakkında daha fazla bilgi için bkz. [İşlem yönetme](../sql-data-warehouse/sql-data-warehouse-manage-compute-overview.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json).

### <a name="maintain-statistics"></a>İstatistiklerin bakımını yapın

İstatistiklerinizi günlük olarak veya her bir yük sonra güncelleştirdiğinizden emin olun.  İstatistiklerin sıfırdan oluşturulması veya mevcut olanların güncelleştirilmesi konusunda karar vermek için performans ve maliyet açısından değerlendirme yapmanız gerekir. Tüm istatistiklerinizin bakımını yapmak çok uzun sürerse, hangi sütunların istatistikte olduğunu veya sık güncelleştirilmesi gereken sütunları daha seçmeli hale getirmeniz gerekir.  

Örneğin, yeni değerlerin günlük olarak eklenebileceği Tarih sütunlarını güncelleştirmek isteyebilirsiniz. 

> [!NOTE]
> Birleşimlerde yer alan sütunlar, WHERE yan tümcesinde kullanılan sütunlar ve GROUP BY içinde bulunan sütunlar için istatistik bulundurarak en avantaja sahip olursunuz.

Ayrıca bkz. [tablo Istatistiklerini yönetme](develop-tables-statistics.md), [ISTATISTIK oluşturma](/sql/t-sql/statements/create-statistics-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), [istatistikleri güncelleştirme](/sql/t-sql/statements/update-statistics-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest).

### <a name="hash-distribute-large-tables"></a>Büyük tabloları karma olarak dağıtın

Tablolar varsayılan olarak Hepsini Bir Kez Deneme yöntemiyle dağıtılmıştır.  Bu, kullanıcıların tablolarının nasıl dağıtılması gerektiğini belirlemek zorunda kalmadan tabloların tablo oluşturmaya başlamasını kolaylaştırır.  Hepsini bir kez deneme tablosu, bazı iş yükleri için yeterince sürebilir. Ancak çoğu durumda, bir dağıtım sütununun seçilmesi çok daha iyi bir işlem yapar.  

Sütuna göre dağıtılmış bir tablonun Hepsini Bir Kez Deneme tablosundan daha iyi performans sunacağı bir örnek, iki büyük bilgi tablosunun birleştirilmesidir.  

Örneğin, order_id tarafından dağıtılan bir Siparişler tablonuz ve order_id tarafından da dağıtılan bir işlem tablosu varsa, Orders tablonuzu order_id işlem tablonuza eklediğinizde bu sorgu doğrudan bir sorgu haline gelir. 

Bu, veri taşıma işlemlerini ortadan kaldırdığımız anlamına gelir.  Adım sayısı ne kadar az olursa sorgu o kadar hızlı işlenir.  Taşınan veri miktarı azaldıkça sorgunun hızı artar.

> [!TIP]
> Dağıtılmış bir tablo yüklenirken, yükleme işleminin yavaşlamaması için gelen verilerinizin dağıtım anahtarına göre sıralanmamış olduğundan emin olun.  

Dağıtım sütununun nasıl seçileceği hakkında daha fazla bilgi için aşağıdaki bağlantılara bakın ve CREATE TABLES deyiminiz WıTH yan tümcesinde dağıtılmış bir tablo tanımlayabilirsiniz.

Ayrıca bkz. [tabloya genel bakış](develop-tables-overview.md), [tablo dağıtımı](../sql-data-warehouse/sql-data-warehouse-tables-distribute.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json), [tablo dağıtımı](https://blogs.msdn.microsoft.com/sqlcat/20../../choosing-hash-distributed-table-vs-round-robin-distributed-table-in-azure-sql-dw-service/), [Create Table](/sql/t-sql/statements/create-table-azure-sql-data-warehouse?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)ve [Create Table](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)seçme.

### <a name="do-not-over-partition"></a>Aşırı bölümleme yapmayın
Verilerin bölümlenmesi, bölüm değiştirme veya taramayı en iyi duruma getirme ile, bölüm ile verileri korumak için etkili olsa da, çok fazla bölüm olması sorgularınızı yavaşlatabilir.  Genellikle SQL Server en iyi şekilde çalışan yüksek düzeyde parçalı bölümleme stratejisi, SQL havuzunda iyi çalışmayabilir.  

> [!NOTE]
> Genellikle SQL Server en iyi şekilde çalışan yüksek düzeyde parçalı bölümleme stratejisi, SQL havuzunda iyi çalışmayabilir.  

Bölüm sayısının çok fazla olması, her bir bölümdeki satır sayısının 1 milyondan az olması halinde kümelenmiş columnstore dizinlerinin verimini de düşürebilir. SQL havuzu verilerinizi sizin için 60 veritabanlarına göre bölümlendirir. 

Bu nedenle, 100 bölümlü bir tablo oluşturursanız sonuç 6000 bölüm olacaktır.  Her iş yükü farklı olduğundan, en iyi yöntem deneme yanılma ile iş yükünüze en uygun bölümleme şeklini belirlemektir.  

Göz önünde bulundurulması gereken bir seçenek, SQL Server ' de sizin için çalışmış olabilecek değerden daha düşük bir ayrıntı düzeyi kullanmaktır.  Örneğin, günlük bölümler yerine haftalık veya aylık bölümler kullanın.

Ayrıca bkz. [tablo bölümleme](../sql-data-warehouse/sql-data-warehouse-tables-partition.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json).

### <a name="minimize-transaction-sizes"></a>İşlem boyutları en aza indirin

Bir işlemde çalışan INSERT, UPDATE ve DELETE deyimleri başarısız olduğunda gerçekleştirilen adımların geri alınması gerekir.  Uzun sürecek bir geri alma işlemi olasılığını en aza indirmek için işlem boyutlarını mümkün oldukça küçültün.  Bunu yapmak için INSERT, UPDATE ve DELETE deyimlerini parçalara ayırabilirsiniz.  

Örneğin, 1 saat beklemeniz beklenen bir EKLEMEMENIZ varsa, EKLEMEYI dört parçaya bölebilir ve bu sayede her bir çalıştırmayı 15 dakika kadar kısaltabiliriz.

> [!TIP]
> Geri alma riskini azaltmak için CTAS, TRUNCATE, DROP TABLE veya INSERT gibi özel En Az Günlüğe Kaydetme durumlarında boş tablo kullanın.  

Geri alma işlemlerini ortadan kaldırmanın başka bir yöntemi de veri yönetimi için bölüm değiştirme gibi Yalnızca Meta Veri işlemlerini kullanmaktır.  

Örneğin, bir tablodaki order_date değerleri Ekim 2001 içinde olan tüm satırları silmek amacıyla bir DELETE deyimi çalıştırmak yerine verilerinizi aylık bölümlere ayırıp bölümü başka bir tablonun boş bölümüyle değiştirebilirsiniz (ALTER TABLE örneklerini inceleyin).  

Bölümlenmemiş tablolar için DELETE deyimini kullanmak yerine CTAS kullanarak tutmak istediğiniz verileri bir tabloya yazabilirsiniz.  Gereken süre aynıysa, günlüğe çok az işlem kaydı yapıldığı ve gerekli olması halinde hızlıca iptal edilebildiği için CTAS işlemi daha güvenli bir seçenektir.

Ayrıca bkz. [Işlemleri anlama](develop-transactions.md), [işlemleri iyileştirme](../sql-data-warehouse/sql-data-warehouse-develop-best-practices-transactions.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json), [tablo bölümleme](../sql-data-warehouse/sql-data-warehouse-tables-partition.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json), [truncate table](/sql/t-sql/statements/truncate-table-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), [alter table](/sql/t-sql/statements/alter-table-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)ve [Create Table as Select (CTAS)](../sql-data-warehouse/sql-data-warehouse-develop-ctas.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json).

### <a name="use-the-smallest-possible-column-size"></a>Mümkün olan en küçük sütun boyutunu kullanın

Sorgu performansını artırmak için, DDL tanımlaması yaparken verilerinizi destekleyecek en küçük veri türünü kullanın.  Bu durum özellikle CHAR ve VARCHAR sütunları için önemlidir.  

Bir sütundaki en uzun değer 25 karakterse, sütununuzu VARCHAR(25) olarak tanımlayın.  Tüm karakter sütunları için varsayılan uzunluk değeri olarak yüksek bir değer kullanmaktan kaçının.  Ayrıca, NVARCHAR şart değilse sütunları VARCHAR olarak tanımlayın.

Ayrıca bkz. [tabloya genel bakış](develop-tables-overview.md), [tablo veri türleri](develop-tables-data-types.md)ve [Create Table](/sql/t-sql/statements/create-table-azure-sql-data-warehouse?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest).

### <a name="optimize-clustered-columnstore-tables"></a>Kümelenmiş columnstore tablolarını iyileştirin

Kümelenmiş columnstore dizinleri, verilerinizi SQL havuzunda depolayabilmeniz için en etkili yöntemlerle biridir.  Varsayılan olarak, SQL havuzundaki tablolar kümelenmiş ColumnStore olarak oluşturulur.  

Columnstore tablolarında yapılan sorgularda en iyi performansı elde etmek için segment kalitesinin yüksek olması önemlidir.  Satırlar columnstore tablolarına bellek baskısı altında yazıldığında, segment kalitesi düşebilir.  

Segment kalitesi, sıkıştırılmış Satır Grubu içindeki satır sayısıyla ölçülebilir.  Kümelenmiş columnstore tabloları için segment kalitesini algılamaya ve geliştirmeye yönelik adım adım yönergeler için, [kötü columnstore dizin kalitesi](../sql-data-warehouse/sql-data-warehouse-tables-index.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json#causes-of-poor-columnstore-index-quality) ve [tablo dizinlerinin](../sql-data-warehouse/sql-data-warehouse-tables-index.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) nedenleri makalesine bakın.  

Yüksek kaliteli columnstore kesimleri önemli olduğundan, verileri yüklemek için orta veya büyük kaynak sınıfında bulunan Kullanıcı kimliklerini kullanmak iyi bir fikirdir. Daha düşük [veri ambarı birimlerinin](resource-consumption-models.md) kullanılması, yükleme kullanıcıya daha büyük bir kaynak sınıfı atamak istediğiniz anlamına gelir.

Columnstore tabloları genellikle tablo başına 1.000.000 ' den fazla satır olana kadar sıkıştırılmış bir columnstore segmentine veri gönderemeyeceği ve her bir SQL havuzu tablosu 60 tablo olarak bölümlendiğinden, tablo 60.000.000 ' den fazla satır içermiyorsa, columnstore tabloları bir sorgu avantajına sahip olmaz.  

> [!TIP]
> 60.000.000 ' den az satır içeren tablolar için columstore dizinine sahip olmak en iyi çözüm olmayabilir.  

Ayrıca, verilerinizi bölümlemeniz halinde her bir bölümün kümelenmiş columnstore dizini kullanabilmesi için en az 1 milyon satıra ihtiyaç duyacağını unutmayın.  Bir tabloda 100 bölüm varsa, kümelenmiş bir sütun deposundan faydalanmak için en az 6.000.000.000 satıra sahip olması gerekir (60 dağıtımları *100 bölümleri* 1.000.000 satırları).  

Tablonuzun 6.000.000.000 satırı yoksa, bölüm sayısını azaltın veya bunun yerine bir yığın tablosu kullanmayı deneyin.  Ayrıca, bir columnstore tablosu yerine ikincil dizinlerle bir yığın tablosu kullanılarak daha iyi bir performans kazanılabilir.

Columnstore tablosunda çalıştırılan sorgular yalnızca ihtiyacınız olan sütunları seçmeniz halinde daha hızlı olacaktır.  

Ayrıca bkz. [tablo dizinleri](../sql-data-warehouse/sql-data-warehouse-tables-index.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json), [columnstore dizinleri Kılavuzu](/sql/relational-databases/indexes/columnstore-indexes-overview?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), [columnstore dizinlerini yeniden oluşturma](../sql-data-warehouse/sql-data-warehouse-tables-index.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json#rebuilding-indexes-to-improve-segment-quality).

## <a name="sql-on-demand-development-best-practices"></a>İsteğe bağlı SQL geliştirme en iyi uygulamaları

### <a name="general-considerations"></a>Dikkat edilmesi gereken temel noktalar

İsteğe bağlı SQL, Azure depolama hesaplarınızdaki dosyaları sorgulamanızı sağlar. Yerel depolama veya alma özelliklerine sahip değildir, yani sorgu hedeflerinin tüm dosyalar isteğe bağlı SQL 'e harici olur. Bu nedenle, depolama alanından dosya okuma ile ilgili her şey sorgu performansının üzerinde bir etkiye sahip olabilir.

### <a name="colocate-azure-storage-account-and-sql-on-demand"></a>Azure Storage hesabını ve isteğe bağlı SQL 'i birlikte bulundurma

Gecikme süresini en aza indirmek için, Azure depolama hesabınızı ve SQL isteğe bağlı uç noktanızı birlikte bulundurma. Çalışma alanı oluşturma sırasında sağlanan depolama hesapları ve uç noktaları aynı bölgede bulunur.

En iyi performans için, isteğe bağlı SQL ile diğer depolama hesaplarına eriştiğinizde aynı bölgede olduklarından emin olun. Aksi takdirde, verilerin uzak bölgeden uç noktanın bölgesine ağ aktarımı için gecikme süresi artacaktır.

### <a name="azure-storage-throttling"></a>Azure depolama alanı azaltma

Birden çok uygulama ve hizmet, depolama hesabınıza erişebilir. Uygulamalar, hizmetler ve SQL istek üzerine SQL iş yükü tarafından oluşturulan Birleşik ıOPS veya üretilen iş yükü depolama hesabının sınırlarını aşarsa, depolama alanı azaltma oluşur. Depolama alanı azaltma gerçekleştiğinde, sorgu performansı üzerinde önemli ölçüde olumsuz bir etkiye sahip olur.

Daraltma algılandıktan sonra, SQL isteğe bağlı, bu senaryonun yerleşik olarak işlenmesini içerir. İsteğe bağlı SQL, azaltma çözümlenene kadar daha yavaş bir hızda depolama istekleri yapar. 

Ancak, en iyi sorgu yürütmesi için, sorgu yürütme sırasında diğer iş yükleriyle depolama hesabını vurgulamanızı öneririz.

### <a name="prepare-files-for-querying"></a>Dosyaları sorgulamaya hazırlama

Mümkünse, daha iyi performans için dosyaları hazırlayacaksınız:

- CSV 'yi Parquet – Parquet, sütunlu biçim olarak dönüştürür. Sıkıştırılmış olduğundan, aynı verilere sahip CSV dosyalarından daha küçük dosya boyutlarına sahiptir ve isteğe bağlı SQL 'i okumak için daha az zaman ve depolama istekleri gerekecektir.
- Bir sorgu tek bir büyük dosyayı hedefliyorsa, onu birden çok daha küçük dosyaya bölebilmeniz yararlı olacaktır.
- CSV dosyanızın boyutunu 10 GB 'tan daha düşük tutmaya çalışın.
- Tek bir OPENROWSET yolu veya dış tablo konumu için eşit boyutlu dosyalar olması tercih edilir.
- Bölümleri farklı klasörlere veya dosya adlarına depolayarak verilerinizi bölümleyin; [belirli bölümleri hedeflemek için dosya adı ve FilePath işlevlerini kullanın](#use-fileinfo-and-filepath-functions-to-target-specific-partitions).

### <a name="use-fileinfo-and-filepath-functions-to-target-specific-partitions"></a>Belirli bölümleri hedeflemek için FileInfo ve FilePath işlevlerini kullanın

Veriler genellikle bölümler halinde düzenlenir. Belirli klasörleri ve dosyaları sorgulamak için isteğe bağlı SQL 'e bildirebilirsiniz. Bu işlem, sorgunun okuması ve işlemesi gereken dosya sayısını ve veri miktarını azaltır. 

Sonuç olarak, daha iyi performans elde edersiniz. Daha fazla bilgi için [dosya adı](develop-storage-files-overview.md#filename-function) ve [FilePath](develop-storage-files-overview.md#filepath-function) işlevlerini ve [belirli dosyaları sorgulama](query-specific-files.md)ile ilgili örnekleri denetleyin.

Depolamadaki verileriniz bölümlendirilmemişse, bu dosyaları hedefleyen sorguları iyileştirmek için bu işlevleri kullanabilmeniz için Bölümlendirmeyi düşünün.

İsteğe bağlı SQL 'de [bölümlenmiş Spark tabloları sorgulanırken](develop-storage-files-spark-tables.md) , sorgu yalnızca gerekli dosyaları hedefleyecek.

### <a name="use-cetas-to-enhance-query-performance-and-joins"></a>Sorgu performansını ve birleştirmeleri geliştirmek için CETAS kullanın

[Cetas](develop-tables-cetas.md) , SQL isteğe bağlı olarak sunulan en önemli özelliklerden biridir. CETAS, dış tablo meta verileri oluşturan ve SELECT sorgusunun sonucunu Depolama hesabınızdaki bir dosya kümesine dışarı aktaran paralel bir işlemdir.

Birleştirilmiş başvuru tabloları gibi sorguların sık kullanılan parçalarını yeni bir dosya kümesine depolamak için CETAS kullanabilirsiniz. Daha sonra, birden çok sorgu içinde ortak birleştirmeleri yinelemek yerine bu tek dış tabloya katabilirsiniz. 

CETAS, Parquet dosyalarını oluşturduğunda, ilk sorgu bu dış tabloyu hedeflediğinde istatistikler otomatik olarak oluşturulur ve daha iyi performans elde edersiniz.

### <a name="next-steps"></a>Sonraki adımlar

Bu makalede sağlanmayan bilgilere ihtiyacınız varsa, tüm SQL havuzu belgelerini aramak için bu sayfanın sol tarafındaki "belgeleri ara" seçeneğini kullanın.  [SQL havuzu Forumu](https://social.msdn.microsoft.com/Forums/sqlserver/home?forum=AzureSQLDataWarehouse) , diğer KULLANıCıLARA ve SQL havuzu ürün grubuna yönelik sorularınızı oluşturabilmeniz için bir yerdir.  

Sorularınızın diğer kullanıcılar veya ekibimiz tarafından yanıtlandığından emin olmak için bu forumu sürekli takip ediyoruz.  Stack Overflow sorularınızı sormayı tercih ediyorsanız [Azure SQL havuzu Stack Overflow forumumuz](https://stackoverflow.com/questions/tagged/azure-sqldw)de vardır.
 