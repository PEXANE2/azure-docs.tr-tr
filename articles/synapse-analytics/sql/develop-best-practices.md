---
title: Synapse SQL için geliştirme en iyi uygulamaları
description: Synapse SQL için geliştirirken bilmeniz gereken öneriler ve en iyi uygulamalar.
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
ms.sourcegitcommit: 09a124d851fbbab7bc0b14efd6ef4e0275c7ee88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2020
ms.locfileid: "82086360"
---
# <a name="development-best-practices-for-synapse-sql"></a>Synapse SQL için geliştirme en iyi uygulamaları
Bu makalede, veri ambarı çözümünüzü geliştirirken kılavuz ve en iyi uygulamalar açıklanmaktadır. 

## <a name="sql-pool-development-best-practices"></a>SQL havuz geliştirme en iyi uygulamalar

### <a name="reduce-cost-with-pause-and-scale"></a>Duraklatma ve ölçeklendirme ile maliyetleri azaltın

Duraklatma ve ölçeklendirme aracılığıyla maliyetleri azaltma hakkında daha fazla bilgi için bkz. [İşlem yönetme](../sql-data-warehouse/sql-data-warehouse-manage-compute-overview.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json).

### <a name="maintain-statistics"></a>İstatistiklerin bakımını yapın

İstatistiklerinizi günlük veya her yüklemeden sonra güncelleştirdiğinizden emin olun.  İstatistiklerin sıfırdan oluşturulması veya mevcut olanların güncelleştirilmesi konusunda karar vermek için performans ve maliyet açısından değerlendirme yapmanız gerekir. Tüm istatistiklerinizi korumanın çok uzun sürdüğünü düşünüyorsanız, hangi sütunların istatistikleri nin olması veya hangi sütunların sık sık güncellenmesi gerektiği konusunda daha seçici olun.  

Örneğin, yeni değerlerin günlük olarak eklenebileceği tarih sütunlarını güncelleştirmek isteyebilirsiniz. 

> [!NOTE]
> Birleştirmelerde yer alan sütunlar, WHERE yan tümcesinde kullanılan sütunlar ve GROUP BY'de bulunan sütunlar hakkında istatistikler alarak en fazla faydayı elde erecektir.

Ayrıca bakınız [Tablo istatistiklerini yönetin,](develop-tables-statistics.md) [İstATİstİkLER OLUŞTURUN,](/sql/t-sql/statements/create-statistics-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) [İstATİstİkLerİ GÜNCELLİR.](/sql/t-sql/statements/update-statistics-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)

### <a name="hash-distribute-large-tables"></a>Büyük tabloları karma olarak dağıtın

Tablolar varsayılan olarak Hepsini Bir Kez Deneme yöntemiyle dağıtılmıştır.  Bu, kullanıcıların tablolarının nasıl dağıtılacağına karar vermek zorunda kalmadan tablo oluşturmaya başlamalarını kolaylaştırır.  Yuvarlak Robin tabloları bazı iş yükleri için yeterince performans gösterebilir. Ancak, çoğu durumda, bir dağıtım sütunu seçmek çok daha iyi performans gösterir.  

Sütuna göre dağıtılmış bir tablonun Hepsini Bir Kez Deneme tablosundan daha iyi performans sunacağı bir örnek, iki büyük bilgi tablosunun birleştirilmesidir.  

Örneğin, order_id tarafından dağıtılan bir sipariş tablonuz ve order_id tarafından da dağıtılan bir hareketler tablonuz varsa, siparişler tablonuzu order_id'daki hareketler tablonuza katıldığınızda, bu sorgu geçiş sorgusu olur. 

Bu, veri hareketi işlemlerini ortadan kaldırdığımız anlamına gelir.  Adım sayısı ne kadar az olursa sorgu o kadar hızlı işlenir.  Taşınan veri miktarı azaldıkça sorgunun hızı artar.

> [!TIP]
> Dağıtılmış bir tablo yüklenirken, yükleme işleminin yavaşlamaması için gelen verilerinizin dağıtım anahtarına göre sıralanmamış olduğundan emin olun.  

Create TABLES ekstrenizin WITH yan tümcesinde dağıtılmış bir tablonun nasıl tanımlanabileceği ve dağıtım tablosunun nasıl tanımlanabileceği hakkında ek ayrıntılar için aşağıdaki bağlantılara bakın.

Ayrıca bakınız [Tablo genel bakış](develop-tables-overview.md), Tablo [dağılımı](../sql-data-warehouse/sql-data-warehouse-tables-distribute.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json), Tablo [dağılımı seçimi](https://blogs.msdn.microsoft.com/sqlcat/20../../choosing-hash-distributed-table-vs-round-robin-distributed-table-in-azure-sql-dw-service/), TABLO [OLUŞTUR](/sql/t-sql/statements/create-table-azure-sql-data-warehouse?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)ve SELECT olarak [TABLO OLUŞTUR](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest).

### <a name="do-not-over-partition"></a>Aşırı bölümleme yapmayın
Verileri bölümleme, bölüm değiştirme veya taramaları en iyi duruma geçirerek verilerinizi korumak için etkili olabilirken, çok fazla bölüm olması sorgularınızı yavaşlatabilir.  Genellikle SQL Server'da iyi çalışabilecek yüksek parçalılık bölümleme stratejisi SQL havuzunda iyi çalışmayabilir.  

> [!NOTE]
> Genellikle SQL Server'da iyi çalışabilecek yüksek parçalılık bölümleme stratejisi SQL havuzunda iyi çalışmayabilir.  

Bölüm sayısının çok fazla olması, her bir bölümdeki satır sayısının 1 milyondan az olması halinde kümelenmiş columnstore dizinlerinin verimini de düşürebilir. SQL havuzu verilerinizi sizin için 60 veritabanına bölümlere ayırır. 

Yani, 100 bölümlü bir tablo oluşturursanız, sonuç 6000 bölüm olacaktır.  Her iş yükü farklı olduğundan, en iyi yöntem deneme yanılma ile iş yükünüze en uygun bölümleme şeklini belirlemektir.  

Göz önünde bulundurulması gereken seçeneklerden biri, SQL Server'da sizin için çalışmış olabileceklerden daha düşük bir parçalılık kullanmaktır.  Örneğin, günlük bölümler yerine haftalık veya aylık bölümler kullanın.

Ayrıca bakınız [Tablo bölümleme](../sql-data-warehouse/sql-data-warehouse-tables-partition.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json).

### <a name="minimize-transaction-sizes"></a>İşlem boyutları en aza indirin

Bir işlemde çalışan INSERT, UPDATE ve DELETE deyimleri başarısız olduğunda gerçekleştirilen adımların geri alınması gerekir.  Uzun sürecek bir geri alma işlemi olasılığını en aza indirmek için işlem boyutlarını mümkün oldukça küçültün.  Bunu yapmak için INSERT, UPDATE ve DELETE deyimlerini parçalara ayırabilirsiniz.  

Örneğin, 1 saat sürmesini beklediğiniz bir INSERT'iniz varsa, INSERT'i dört parçaya ayırarak her çalıştırmayı 15 dakikaya kadar kısaltabilirsiniz.

> [!TIP]
> Geri alma riskini azaltmak için CTAS, TRUNCATE, DROP TABLE veya INSERT gibi özel En Az Günlüğe Kaydetme durumlarında boş tablo kullanın.  

Geri alma işlemlerini ortadan kaldırmanın başka bir yöntemi de veri yönetimi için bölüm değiştirme gibi Yalnızca Meta Veri işlemlerini kullanmaktır.  

Örneğin, bir tablodaki order_date değerleri Ekim 2001 içinde olan tüm satırları silmek amacıyla bir DELETE deyimi çalıştırmak yerine verilerinizi aylık bölümlere ayırıp bölümü başka bir tablonun boş bölümüyle değiştirebilirsiniz (ALTER TABLE örneklerini inceleyin).  

Bölümlenmemiş tablolar için DELETE deyimini kullanmak yerine CTAS kullanarak tutmak istediğiniz verileri bir tabloya yazabilirsiniz.  Gereken süre aynıysa, günlüğe çok az işlem kaydı yapıldığı ve gerekli olması halinde hızlıca iptal edilebildiği için CTAS işlemi daha güvenli bir seçenektir.

Ayrıca bakınız [Hareketleri Anlama](develop-transactions.md), Hareketleri [Optimize Etme](../sql-data-warehouse/sql-data-warehouse-develop-best-practices-transactions.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json), Tablo [bölümleme](../sql-data-warehouse/sql-data-warehouse-tables-partition.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json), [TRUNCATE TABLE](/sql/t-sql/statements/truncate-table-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), ALTER [TABLE](/sql/t-sql/statements/alter-table-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), ve [Select (CTAS) olarak tablo oluşturun.](../sql-data-warehouse/sql-data-warehouse-develop-ctas.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)

### <a name="use-the-smallest-possible-column-size"></a>Mümkün olan en küçük sütun boyutunu kullanın

Sorgu performansını artırmak için, DDL tanımlaması yaparken verilerinizi destekleyecek en küçük veri türünü kullanın.  Bu durum özellikle CHAR ve VARCHAR sütunları için önemlidir.  

Bir sütundaki en uzun değer 25 karakterse, sütununuzu VARCHAR(25) olarak tanımlayın.  Tüm karakter sütunları için varsayılan uzunluk değeri olarak yüksek bir değer kullanmaktan kaçının.  Ayrıca, NVARCHAR şart değilse sütunları VARCHAR olarak tanımlayın.

Ayrıca [bakınız Tablo genel bakış](develop-tables-overview.md), [Tablo veri türleri](develop-tables-data-types.md)ve [CREATE TABLE](/sql/t-sql/statements/create-table-azure-sql-data-warehouse?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest).

### <a name="optimize-clustered-columnstore-tables"></a>Kümelenmiş columnstore tablolarını iyileştirin

Kümelenmiş sütun deposu dizinleri, verilerinizi SQL havuzunda depolamanın en etkili yollarından biridir.  Varsayılan olarak, SQL havuzundaki tablolar Clustered ColumnStore olarak oluşturulur.  

Columnstore tablolarında yapılan sorgularda en iyi performansı elde etmek için segment kalitesinin yüksek olması önemlidir.  Satırlar columnstore tablolarına bellek baskısı altında yazıldığında, segment kalitesi düşebilir.  

Segment kalitesi, sıkıştırılmış Satır Grubu içindeki satır sayısıyla ölçülebilir.  Kümelenmiş sütun deposu tabloları için segment kalitesini algılama ve iyileştirme yle ilgili adım adım yönergeler için [kötü sütun deposu dizin kalitesinin nedenleri](../sql-data-warehouse/sql-data-warehouse-tables-index.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json#causes-of-poor-columnstore-index-quality) ve Tablo [dizinleri](../sql-data-warehouse/sql-data-warehouse-tables-index.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) makalesine bakın.  

Yüksek kaliteli sütun deposu segmentleri önemli olduğundan, veri yüklemek için orta veya büyük kaynak sınıfında bulunan kullanıcı bilgisayarlarını kullanmak iyi bir fikirdir. Daha düşük [veri ambarı birimlerini](resource-consumption-models.md) kullanmak, yükleme kullanıcınıza daha büyük bir kaynak sınıfı atamak istediğiniz anlamına gelir.

Sütun deposu tabloları genellikle tablo başına 1 milyondan fazla satır olana kadar sıkıştırılmış sütun mağazası segmentine veri itmez ve her SQL bilardo tablosu 60 tabloya bölünür, sütun deposu tabloları tablo 60 milyondan fazla satır olmadığı sürece bir sorgudan yararlanamaz.  

> [!TIP]
> 60 milyondan az satıra sahip tablolar için, columstore indeksi olması en uygun çözüm olmayabilir.  

Ayrıca, verilerinizi bölümlemeniz halinde her bir bölümün kümelenmiş columnstore dizini kullanabilmesi için en az 1 milyon satıra ihtiyaç duyacağını unutmayın.  Bir tabloda 100 bölüm varsa, kümelenmiş sütun deposundan (60 dağılım *100 bölüm 1* milyon satır) yararlanmak için en az 6 milyar satır olması gerekir.  

Tablonuzda 6 milyar satır yoksa, bölüm sayısını azaltın veya bunun yerine yığın tablo kullanmayı düşünün.  Ayrıca, sütun deposu tablosu yerine ikincil dizinleri olan bir yığın tablo kullanılarak daha iyi performans elde edilip edililebildiğini görmek için denemeye değer olabilir.

Columnstore tablosunda çalıştırılan sorgular yalnızca ihtiyacınız olan sütunları seçmeniz halinde daha hızlı olacaktır.  

Ayrıca bakınız [Tablo dizinleri](../sql-data-warehouse/sql-data-warehouse-tables-index.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json), Sütun mağazası [dizinleri kılavuzu](/sql/relational-databases/indexes/columnstore-indexes-overview?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), [Sütun mağaza dizinlerini yeniden oluşturma](../sql-data-warehouse/sql-data-warehouse-tables-index.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json#rebuilding-indexes-to-improve-segment-quality).

## <a name="sql-on-demand-development-best-practices"></a>SQL isteğe bağlı geliştirme en iyi uygulamaları

### <a name="general-considerations"></a>Dikkat edilmesi gereken temel noktalar

İsteğe bağlı SQL, Azure depolama hesaplarınızdaki dosyaları sorgulamanızı sağlar. Yerel depolama veya yutma yetenekleri yoktur, yani sorgu hedeflerinin tüm dosyaları isteğe bağlı OLARAK SQL'in dışındadır. Bu nedenle, depolama dan dosya okuma ile ilgili her şey sorgu performansı üzerinde bir etkisi olabilir.

### <a name="colocate-azure-storage-account-and-sql-on-demand"></a>Azure Depolama hesabını ve SQL isteğe bağlı olarak birlikte bulun

Gecikme yi en aza indirmek için Azure depolama hesabınızı ve SQL isteğe bağlı bitiş noktanızı birlikte bulunduun. Çalışma alanı oluşturma sırasında sağlanan depolama hesapları ve uç noktaları aynı bölgede bulunur.

En iyi performans için, SQL isteğe bağlı diğer depolama hesaplarına erişirseniz, aynı bölgede olduğundan emin olun. Aksi takdirde, uzak bölgeden bitiş noktasının bölgesine veri ağ aktarımı için gecikme sayılmı artar.

### <a name="azure-storage-throttling"></a>Azure Depolama azaltma

Depolama hesabınıza birden çok uygulama ve hizmet erişebilir. Uygulamalar, hizmetler ve SQL isteğe bağlı iş yükü tarafından oluşturulan birleşik IOPS veya iş yükü depolama hesabının sınırlarını aştığında, depolama azaltma oluşur. Depolama azaltma oluştuğunda, sorgu performansı üzerinde önemli bir olumsuz etkisi vardır.

Azaltma algılandıktan sonra, SQL isteğe bağlı olarak bu senaryoyu yerleşik olarak ele almıştır. İsteğe bağlı SQL, azaltma çözülene kadar depolama isteklerini daha yavaş bir hızda yapar. 

Ancak, en iyi sorgu yürütme için, sorgu yürütme sırasında diğer iş yükleri ile depolama hesabı vurgulamak değil önerilir.

### <a name="prepare-files-for-querying"></a>Sorgu için dosya hazırlama

Mümkünse, dosyaları daha iyi performans için hazırlayabilirsiniz:

- CSV'yi Parke Dönüştürün – Parke columnar formatındadır. Sıkıştırılmış olduğundan, aynı verilere sahip CSV dosyalarına göre daha küçük dosya boyutlarına sahiptir ve SQL on-demand'in okumak için daha az zamana ve depolama isteklerine ihtiyacı vardır.
- Bir sorgu tek bir büyük dosyayı hedefliyorsa, dosyayı birden çok küçük dosyaya bölmenin avantajını elde elabilirsiniz.
- CSV dosya boyutunu10 GB'ın altında tutmayı deneyin.
- Tek bir OPENROWSET yolu veya harici bir tablo KONUMU için eşit büyüklükte dosyalar olması tercih edilir.
- Bölümlerinizi farklı klasörlere veya dosya adlarına depolayarak verilerinizi bölme - [belirli bölümleri hedeflemek için dosya adı ve dosya yolu işlevlerini kullanın.](#use-fileinfo-and-filepath-functions-to-target-specific-partitions)

### <a name="use-fileinfo-and-filepath-functions-to-target-specific-partitions"></a>Belirli bölümleri hedeflemek için dosya bilgisi ve dosya yolu işlevlerini kullanma

Veriler genellikle bölümler halinde düzenlenir. Belirli klasörleri ve dosyaları sorgulamak için SQL on-demand'e talimat verebilirsiniz. Bu, dosya sayısını ve sorgunun okuması ve işlemesi gereken veri miktarını azaltır. 

Sonuç olarak, daha iyi performans elde edecektir. Daha fazla bilgi [için, dosya adı](develop-storage-files-overview.md#filename-function) ve dosya [yolu](develop-storage-files-overview.md#filepath-function) işlevlerini ve belirli dosyaları nasıl [sorgulayabildiğini](query-specific-files.md)denetleyin.

Depolama daki verileriniz bölümlenmiyorsa, bu dosyaları hedefleyen sorguları en iyi duruma getirmek için bu işlevleri kullanabilmek için bu verileri bölümlemeyi düşünün.

[Bölümlenmiş Spark tablolarını](develop-storage-files-spark-tables.md) isteğe bağlı OLARAK SQL'den sorgularken, sorgu otomatik olarak yalnızca gerekli dosyaları hedefler.

### <a name="use-cetas-to-enhance-query-performance-and-joins"></a>Sorgu performansını artırmak için CETAS'ı kullanın ve birleştirir

[CETAS,](develop-tables-cetas.md) isteğe bağlı SQL'de mevcut olan en önemli özelliklerden biridir. CETAS, dış tablo meta verileri oluşturan ve SELECT sorgusunun sonucunu depolama hesabınızdaki bir dosya kümesine aktaran paralel bir işlemdir.

CETAS'ı, sık sık kullanılan sorguların birbölümünü (birleştirilmiş başvuru tabloları gibi) yeni bir dosya kümesinde depolamak için kullanabilirsiniz. Daha sonra, birden çok sorguda ortak birleştirmeleri yinelemek yerine bu tek dış tabloya katılabilirsiniz. 

CETAŞ Parke dosyaları oluşturdukça, ilk sorgu bu dış tabloyu hedefaldığında istatistikler otomatik olarak oluşturulur ve daha iyi performans elde erecektir.

### <a name="next-steps"></a>Sonraki adımlar

Bu makalede sağlanmayan bilgilere ihtiyacınız varsa, tüm SQL havuzu belgelerini aramak için bu sayfanın sol tarafındaki "Doküman ara"yı kullanın.  [SQL havuzu Forumu,](https://social.msdn.microsoft.com/Forums/sqlserver/home?forum=AzureSQLDataWarehouse) diğer kullanıcılara ve SQL havuzu Ürün Grubu'na soru yöneltmeniz için bir yerdir.  

Sorularınızın diğer kullanıcılar veya ekibimiz tarafından yanıtlandığından emin olmak için bu forumu sürekli takip ediyoruz.  Stack Taşma ile ilgili sorularınızı sormayı tercih ederseniz, [Azure SQL havuzu Yığın Taşma Forumu](https://stackoverflow.com/questions/tagged/azure-sqldw)da vardır.
 