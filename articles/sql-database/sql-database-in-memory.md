---
title: Bellek içi teknolojiler
description: Azure SQL Veritabanı Bellek İçi teknolojiler, işlem ve analitik iş yüklerinin performansını büyük ölçüde artırır.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 03/19/2019
ms.openlocfilehash: de60712451d4c2e8a7d931f7a09352f55be05694
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73810256"
---
# <a name="optimize-performance-by-using-in-memory-technologies-in-sql-database"></a>SQL Veritabanı'nda Bellek Içi teknolojileri kullanarak performansı optimize edin

Azure SQL Veritabanı'ndaki Bellek İçi teknolojiler, uygulamanızın performansını artırmanıza ve veritabanınızın maliyetini düşürmenize olanak tanır. 

## <a name="when-to-use-in-memory-technologies"></a>Bellek içi teknolojilerin ne zaman kullanılacağı

Azure SQL Veritabanı'nda Bellek Içi teknolojileri kullanarak, çeşitli iş yükleriyle performans iyileştirmeleri elde edebilirsiniz:

- **İsteklerin** çoğunun daha küçük veri kümesini (örneğin, CRUD işlemleri) okuduğu veya güncelleştirdiği işlemsel (çevrimiçi işlem işleme (OLTP)).
- **Sorguların** çoğunun raporlama amacıyla karmaşık hesaplamalara sahip olduğu analitik (çevrimiçi analitik işleme (OLAP), verileri varolan tablolara yükleyen ve ekleyen belirli sayıda sorguyla (toplu yük olarak adlandırılır) veya tabloların verilerini silen belirli sayıda sorguyla. 
- **Hem** OLTP hem de OLAP sorgularının aynı veri kümesinde yürütüldüğü karma (karma işlem/analitik işleme (HTAP))

Bellek içi teknolojiler, işlenmesi gereken verileri belleğe saklayarak, sorguların yerel derlemesini kullanarak veya toplu işleme ve SIMD yönergeleri gibi gelişmiş işlemeleri kullanarak bu iş yüklerinin performansını artırabilir. altında yatan donanım. 

## <a name="overview"></a>Genel Bakış

Azure SQL Veritabanı'nda aşağıdaki In-Memory teknolojileri vardır:
- *[Bellek içi OLTP](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization)* saniyedeki hareket sayısını artırır ve işlem işleme için gecikme süresini azaltır. In-Memory OLTP'den yararlanan senaryolar şunlardır: ticaret ve oyun oynama, olaylardan veya IoT aygıtlarından veri alma, önbelleğe alma, veri yükü ve geçici tablo ve tablo değişken senaryoları gibi yüksek işlem hacmine sahip işlem işlemleri.
- *Kümelenmiş sütun deposu dizinleri* depolama ayak izinizi (en fazla 10 kat) azaltır ve raporlama ve analiz sorguları için performansı artırır. Veritabanınıza daha fazla veri sığdırmak ve performansı artırmak için veri martlarınızda gerçek tablolarla kullanabilirsiniz. Ayrıca, arşivlemek ve 10 kata kadar daha fazla veri sorgulayabilmek için operasyonel veritabanınızdaki geçmiş verileriyle birlikte kullanabilirsiniz.
- HTAP için *kümelenmemiş sütun deposu dizini,* pahalı bir ekstre, dönüştürme ve yükleme (ETL) işlemini çalıştırmaya ve veri ambarının doldurulmasını beklemeye gerek kalmadan, doğrudan operasyonel veritabanını sorgulayarak işletmeniz hakkında gerçek zamanlı öngörüler elde etmenize yardımcı olur. Kümeslenmemiş sütun deposu dizinleri, işlevsel iş yükü üzerindeki etkisini azaltırken OLTP veritabanında analitik sorguların hızlı bir şekilde yürütülmesine olanak tanır.
- HTAP için *bellek için optimize edilmiş kümelenmiş sütun deposu dizileri,* hızlı işlem işleme gerçekleştirmenize ve aynı *anda* aynı verilerüzerinde analitiği sorguları çok hızlı bir şekilde çalıştırmanızı sağlar.

Hem columnstore indexes hem de In-Memory OLTP, sırasıyla 2012 ve 2014'ten beri SQL Server ürününün bir parçası olmuştur. Azure SQL Veritabanı ve SQL Server, Bellek Içi teknolojilerin aynı uygulamasını paylaşır. İleriye dönük olarak, bu teknolojiler için yeni özellikler, SQL Server'da yayımlanmadan önce önce Azure SQL Veritabanı'nda yayımlanır.

## <a name="benefits-of-in-memory-technology"></a>Bellek içi teknolojinin faydaları

Daha verimli sorgu ve işlem işleme nedeniyle, Bellek içi teknolojiler de maliyeti azaltmanıza yardımcı olur. Performans kazançları elde etmek için genellikle veritabanının fiyatlandırma katmanını yükseltmeniz gerekmez. Bazı durumlarda, Bellek Içi teknolojilerde performans iyileştirmelerini görürken fiyatlandırma katmanını bile azaltabilirsiniz.

Bellek içi OLTP'nin performansı önemli ölçüde artırmaya nasıl yardımcı olduğuna iki örnek verilmiştir:

- In-Memory OLTP kullanarak, [Quorum İş Çözümleri% 70 dus geliştirirken onların iş yükünü iki katına başardı.](https://customers.microsoft.com/story/quorum-doubles-key-databases-workload-while-lowering-dtu-with-sql-database)

  - DTU *veritabanı işlem birimi*anlamına gelir ve kaynak tüketiminin bir ölçüsüiçerir.
- Aşağıdaki video, örnek bir iş yüküyle kaynak tüketiminde önemli bir iyileşme olduğunu göstermektedir: [Azure SQL Veritabanı Videosu'nda Bellek Içi OLTP.](https://channel9.msdn.com/Shows/Data-Exposed/In-Memory-OTLP-in-Azure-SQL-DB)
  - Daha fazla bilgi için blog gönderisine bakın: [Azure SQL Database Blog Post'ta Bellek İçi OLTP](https://azure.microsoft.com/blog/in-memory-oltp-in-azure-sql-database/)

> [!NOTE]  
> Bellek içi teknolojiler Premium ve İş Açısından Kritik seviye Azure SQL veritabanlarında ve Premium elastik havuzlarda kullanılabilir.

Aşağıdaki videoda, Azure SQL Veritabanı'ndaki Bellek içi teknolojiler ile olası performans kazanımları açıklanmaktadır. Gördüğünüz performans artışının her zaman iş yükü ve verilerin doğası, veritabanının erişim deseni ve benzeri birçok etkene bağlı olduğunu unutmayın.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Database-In-Memory-Technologies/player]
>
>

Bu makalede, Azure SQL Veritabanı'na özgü Bellek İçi OLTP ve sütun deposu dizilerinin yönleri açıklanır ve örnekler de içerir:

- Bu teknolojilerin depolama ve veri boyutu sınırları üzerindeki etkisini görürsünüz.
- Farklı fiyatlandırma katmanları arasında bu teknolojileri kullanan veritabanlarının hareketini nasıl yöneteceğiniz üzerinde çalışacağınızı görürsünüz.
- Azure SQL Veritabanı'nda In-Memory OLTP'nin yanı sıra sütun deposu dizinlerinin kullanımını gösteren iki örnek görürsünüz.

Daha fazla bilgi için bkz.

- [Bellek İçi OLTP Genel Bakış ve Kullanım Senaryoları](https://msdn.microsoft.com/library/mt774593.aspx) (müşteri vaka çalışmaları ve başlamak için bilgi referansları içerir)
- [Bellek IÇI OLTP için Dokümantasyon](https://msdn.microsoft.com/library/dn133186.aspx)
- [Columnstore Dizinler Kılavuzu](https://msdn.microsoft.com/library/gg492088.aspx)
- [Gerçek zamanlı operasyonel analitik](https://msdn.microsoft.com/library/dn817827.aspx) olarak da bilinen karma işlemsel/analitik işleme (HTAP)

## <a name="in-memory-oltp"></a>Hafıza içi OLTP

Bellek içi OLTP teknolojisi, tüm verileri bellekte tutarak son derece hızlı veri erişim işlemleri sağlar. Oltp iş yükünün performansını artırmak için özel dizinler, sorguların yerel derlemesi ve mandalsız veri erişimi de kullanır. Bellek IÇI OLTP verilerinizi düzenlemenin iki yolu vardır:

- Her **satırın** ayrı bir bellek nesnesi olduğu bellek için optimize edilmiş satır deposu biçimi. Bu, yüksek performanslı OLTP iş yükleri için optimize edilmiş klasik bir Bellek IÇI OLTP biçimidir. Bellek için en iyi duruma getirilmiş satır deposu biçiminde kullanılabilecek iki tür bellek için optimize edilmiş tablo vardır:
  - Sunucu yeniden başlatıldıktan sonra belleğe yerleştirilen satırların korunduğu *dayanıklı tablolar* (SCHEMA_AND_DATA). Bu tür tablolar, bellek içi optimizasyonların ek avantajlarıyla geleneksel bir rowstore tablosu gibi şekilde hizmet eder.
  - Satırların yeniden başlatıldıktan sonra korunmadığı *dayanıklı olmayan tablolar* (SCHEMA_ONLY). Bu tablo türü geçici veriler (örneğin, geçici tabloların değiştirilmesi) veya verileri kalıcı bir tabloya (evreleme tabloları olarak adlandırılır) taşımadan önce hızlı bir şekilde yüklemeniz gereken tablolar için tasarlanmıştır.
- Verilerin sütun biçiminde düzenlendiği **bellek için optimize edilmiş sütun deposu** biçimi. Bu yapı, OLTP iş yükünüzün çalıştığı aynı veri yapısında analitik sorgular çalıştırmanız gereken HTAP senaryoları için tasarlanmıştır.

> [!Note]
> In-Memory OLTP teknolojisi, bellekte tam olarak bulunan veri yapıları için tasarlanmıştır. Bellek içi veriler diske aktarılamadığından, yeterli belleğe sahip veritabanıkullandığınızdan emin olun. Daha fazla bilgi [için Bellek içi OLTP için Veri boyutu ve depolama başlığına](#data-size-and-storage-cap-for-in-memory-oltp) bakın.

Bellek IÇI OLTP: [Quickstart 1: Hızlı T-SQL Performansı için Bellek içi OLTP Teknolojileri](https://msdn.microsoft.com/library/mt694156.aspx) (başlamanıza yardımcı olacak başka bir makale)

Teknolojiler hakkında derinlemesine videolar:

- [Azure SQL Veritabanı'nda Bellek İçi OLTP](https://channel9.msdn.com/Shows/Data-Exposed/In-Memory-OTLP-in-Azure-SQL-DB) (bu sonuçları kendiniz çoğaltmak için performans avantajları nın ve adımların bir demosu içerir)
- [In-Memory OLTP Videolar: Nedir ve Ne zaman / Nasıl kullanılır](https://blogs.msdn.microsoft.com/sqlserverstorageengine/20../../in-memory-oltp-video-what-it-is-and-whenhow-to-use-it/)

Belirli bir veritabanının Bellek IÇI OLTP'yi destekleyip desteklemediğini anlamanın programatik bir yolu vardır. Aşağıdaki Transact-SQL sorgusunu yürütebilirsiniz:
```
SELECT DatabasePropertyEx(DB_NAME(), 'IsXTPSupported');
```
Sorgu **1**döndürürse, Bellek içi OLTP bu veritabanında desteklenir. Aşağıdaki sorgular, veritabanıstandard/basic'e indirgenmeden önce kaldırılması gereken tüm nesneleri tanımlar:
```
SELECT * FROM sys.tables WHERE is_memory_optimized=1
SELECT * FROM sys.table_types WHERE is_memory_optimized=1
SELECT * FROM sys.sql_modules WHERE uses_native_compilation=1
```

### <a name="data-size-and-storage-cap-for-in-memory-oltp"></a>Bellek içi OLTP için veri boyutu ve depolama kapağı

Bellek içi OLTP, kullanıcı verilerini depolamak için kullanılan bellek için optimize edilmiş tablolar içerir. Bu tablolar belleğe sığması için gereklidir. Belleği doğrudan SQL Veritabanı hizmetinde yönettiğiniz için, kullanıcı verileri için kota kavramına sahibiz. Bu fikir *In-Memory OLTP depolama*olarak adlandırılır.

Desteklenen her bir veritabanı fiyatlandırma katmanı ve her elastik havuz fiyatlandırma katmanı belirli bir miktarda Bellek IÇI OLTP depolama içerir. Bkz. [DTU tabanlı kaynak sınırları - tek veritabanı,](sql-database-dtu-resource-limits-single-databases.md) [DTU tabanlı kaynak sınırları - elastik havuzlar,](sql-database-dtu-resource-limits-elastic-pools.md)[vCore tabanlı kaynak sınırları - tek veritabanları](sql-database-vcore-resource-limits-single-databases.md) ve [vCore tabanlı kaynak sınırları - elastik havuzlar.](sql-database-vcore-resource-limits-elastic-pools.md)

Aşağıdaki öğeler Bellek içi OLTP depolama başlığınıza doğru sayılır:

- Bellek için optimize edilmiş tablolarda ve tablo değişkenlerinde etkin kullanıcı veri satırları. Eski satır sürümlerinin kapağın doğru olmadığını unutmayın.
- Bellek için en iyi duruma getirilmiş tablolardaki dizinler.
- ALTER TABLE operasyonlarının operasyonel yükü.

Kapağı na vurursanız, kota dışı bir hata alırsınız ve artık veri ekleyebilir veya güncelleştiremezsınız. Bu hatayı azaltmak için verileri silin veya veritabanı veya havuzun fiyatlandırma katmanını artırın.

Neredeyse kapağıvurduğunuzda Bellek içi OLTP depolama kullanımını izleme ve uyarıları yapılandırma hakkında daha fazla bilgi [için](sql-database-in-memory-oltp-monitoring.md)Bkz.

#### <a name="about-elastic-pools"></a>Elastik havuzlar hakkında

Elastik havuzlarda, In-Memory OLTP depolama havuzundaki tüm veritabanları arasında paylaşılır. Bu nedenle, bir veritabanında kullanımı diğer veritabanları etkileyebilir. Bunun için iki azaltıcı etken vardır:

- Havuz için `Max-eDTU` `MaxvCore` eDTU veya vCore sayısından daha düşük olan veritabanları için yapılandırın. Bu maksimum, havuzdaki herhangi bir veritabanında, eDTU sayısına karşılık gelen boyuta In-Memory OLTP depolama kullanımını kaplar.
- Bir `Min-eDTU` veya `MinvCore` 0'dan büyük olan yapılandırın. Bu minimum, havuzdaki her veritabanının yapılandırılan `Min-eDTU` veya `vCore`.

### <a name="changing-service-tiers-of-databases-that-use-in-memory-oltp-technologies"></a>Bellek içi OLTP teknolojilerini kullanan veritabanlarının hizmet katmanlarını değiştirme

Veritabanınızı veya örneğini genel amaçlıdan Kritik İş'e (veya Standarttan Premium'a) kadar her zaman daha yüksek bir katmana yükseltebilirsiniz. Kullanılabilir işlevsellik ve kaynaklar yalnızca artar.

Ancak katmanı düşürmek veritabanınızı olumsuz etkileyebilir. Veritabanınızda Bellek IÇI OLTP nesneleri bulunduğunda, özellikle Business Critical'ten Genel Amaç'a (veya Premium'dan Standart veya Temel'e) düşürüldüğünüzde etkisi belirgindir. Bellek için en iyi duruma getirilmiş tablolar düşürmeden sonra kullanılamaz (görünür olsalar bile). Aynı hususlar, elastik bir havuzun fiyatlandırma katmanını düşürürken veya bellek içi teknolojilere sahip bir veritabanını Standart veya Temel elastik havuza taşıyorken de geçerlidir.

> [!Important]
> Bellek Içi OLTP Genel Amaç, Standart veya Temel katmanda desteklenmez. Bu nedenle, bellek içi OLTP nesnelerine sahip bir veritabanını Standart veya Temel katmana taşımak mümkün değildir.

Veritabanını Standart/Temel'e düşürmeden önce, bellek için optimize edilmiş tüm tabloları ve tablo türlerinin yanı sıra yerel olarak derlenmiş tüm T-SQL modüllerini kaldırın. 

*İş Açısından Kritik katmanda ölçeklendirme kaynakları*: Bellek için optimize edilmiş tablolardaki veriler, veritabanının veya Yönetilen Örnek'in katmanıyla ilişkili Bellek İçi OLTP depolama alanına sığmalıdır veya elastik havuzda kullanılabilir. Katmanı küçültmeye veya veritabanını yeterli kullanılabilir Bellek IÇI OLTP depolama alanına sahip olmayan bir havuza taşımaya çalışırsanız, işlem başarısız olur.

## <a name="in-memory-columnstore"></a>Bellek içi sütun mağazası

Bellek içi sütun mağazası teknolojisi, büyük miktarda veriyi tablolarda depolamanızı ve sorgulamanızı sağlar. Sütun deposu teknolojisi, geleneksel satır ayarı yönelimli depolama üzerinde OLAP iş yüklerinde sorgu performansının 10 katına kadar kazanç elde etmek için sütun tabanlı veri depolama biçimi ve toplu sorgu işleme kullanır. Ayrıca, sıkıştırılmamış veri boyutu üzerinden veri sıkıştırmasının 10 katına kadar kazanç elde edebilirsiniz.
Verilerinizi düzenlemek için kullanabileceğiniz iki tür sütun mağazası modeli vardır:

- Tablodaki tüm verilerin sütun biçiminde düzenlendiği **kümelenmiş sütun deposu.** Bu modelde, tablodaki tüm satırlar, verileri yüksek oranda sıkıştıran ve tabloda hızlı analitik sorgular ve raporlar yürütmenize olanak tanıyan sütun biçiminde yerleştirilir. Verilerinizin yapısına bağlı olarak, verilerinizin boyutu 10x-100x azaltılabilir. Kümelenmiş sütun deposu modeli, 100K'dan büyük veri yığınlarının diskte depolandırılmadan önce sıkıştırılı olduğundan, büyük miktarda verinin (toplu yük) hızlı bir şekilde yutulmasını da sağlar. Bu model, klasik veri ambarı senaryoları için iyi bir seçimdir. 
- Verilerin geleneksel satır deposu tablosunda depolandığı ve sütun deposu biçiminde analitik sorgular için kullanılan bir dizin bulunduğu **kümelenmiş olmayan sütun deposu.** Bu model, Karma İşlemsel-Analitik İşleme (HTAP): bir işlem iş yükü üzerinde performant gerçek zamanlı analitik çalıştırma yeteneği sağlar. OLTP sorguları, küçük bir satır kümesine erişmek için en iyi duruma getirilmiş olan rowstore tablosunda yürütülürken, OLAP sorguları taramalar ve analizler için daha iyi bir seçim olan sütun deposu dizininde yürütülür. Azure SQL Veritabanı Sorgusu optimize edici, sorguyu temel alarak satır mağazası veya sütun mağazası biçimini dinamik olarak seçer. Kümeslenmiş olmayan sütun deposu dizinleri, özgün veri kümesi hiçbir değişiklik yapmadan özgün satır deposu tablosunda tutulduğundan, verilerin boyutunu azaltmaz. Ancak, ek sütun deposu dizini boyutu, eşdeğer B-ağacı dizini daha küçük büyüklük sırasına göre olmalıdır.

> [!Note]
> Bellek içi sütun deposu teknolojisi yalnızca bellekte işlenmesi için gereken verileri tutarken, belleğe sığamayan veriler diskte depolanır. Bu nedenle, Bellek içi sütun deposu yapılarındaki veri miktarı kullanılabilir bellek miktarını aşabilir. 

Teknoloji hakkında derinlemesine video:

- [Columnstore Index: Ignite 2016'dan Bellek İçi Analiz Videoları](https://blogs.msdn.microsoft.com/sqlserverstorageengine/20../../columnstore-index-in-memory-analytics-i-e-columnstore-index-videos-from-ignite-2016/)

### <a name="data-size-and-storage-for-columnstore-indexes"></a>Sütun deposu dizinleri için veri boyutu ve depolama

Sütun mağazası dizinlerinin belleğe sığması gerekmez. Bu nedenle, dizinlerin boyutundaki tek kapak, [DTU tabanlı satın alma modeli](sql-database-service-tiers-dtu.md) nde ve [vCore tabanlı satın alma modeli](sql-database-service-tiers-vcore.md) makalelerinde belgelenen maksimum genel veritabanı boyutudur.

Kümelenmiş sütun deposu dizinleri kullandığınızda, taban tablo depolama için sütun sıkıştırma kullanılır. Bu sıkıştırma, kullanıcı verilerinizin depolama ayak izini önemli ölçüde azaltabilir, bu da veritabanına daha fazla veri sığdırabileceğiniz anlamına gelir. Ve sıkıştırma daha [columnar arşiv sıkıştırma](https://msdn.microsoft.com/library/cc280449.aspx#using-columnstore-and-columnstore-archive-compression)ile artabilir. Elde edebilirsiniz sıkıştırma miktarı verilerin doğasına bağlıdır, ancak 10 kez sıkıştırma nadir değildir.

Örneğin, en fazla 1 terabayt (TB) boyutuna sahip bir veritabanınız varsa ve sütun deposu dizinlerini kullanarak sıkıştırmanın 10 katını elde ederseniz, veritabanına toplam 10 TB kullanıcı verisi sığdırabilirsiniz.

Kümeslenmemiş sütun mağazası dizinleri kullandığınızda, taban tablo hala geleneksel satır deposu biçiminde depolanır. Bu nedenle, depolama tasarrufu kümelenmiş sütun deposu dizinleri ile büyük değildir. Ancak, bir dizi geleneksel kümelenmesiz dizini tek bir sütun deposu diziniyle değiştiriyorsanız, tablonun depolama ayak izinde genel bir tasarruf görebilirsiniz.

### <a name="changing-service-tiers-of-databases-containing-columnstore-indexes"></a>Columnstore dizinleri içeren veritabanlarının hizmet katmanlarını değiştirme

Hedef katmanınız S3'ün *altındaysa, tek bir veritabanını Temel veya Standart* olarak düşürmek mümkün olmayabilir. Sütun mağazası dizinleri yalnızca İş Açısından/Premium fiyatlandırma katmanında ve Temel katmanda değil, Standart katman, S3 ve üzeri katmanda desteklenir. Veritabanınızı desteklenmeyen bir katmana veya düzeye düşürdüğünüzde, sütun mağazası dizininiz kullanılamaz hale gelir. Sistem sütun mağazası dizininizi korur, ancak dizini asla kullanamaz. Daha sonra desteklenen bir katmana veya düzeye yükseltme niz varsa, sütun mağaza dizininiz hemen yeniden kullanıma hazır dır.

**Kümelenmiş** bir sütun mağazası dizininiz varsa, alt nottan sonra tüm tablo kullanılamaz hale gelir. Bu nedenle, veritabanınızı desteklenmeyen bir katmana veya düzeye düşürmeden önce tüm *kümelenmiş* sütun mağazası dizinlerini bırakmanızı öneririz.

> [!Note]
> Yönetilen Örnek, tüm katmanlarda ColumnStore dizinlerini destekler.

<a id="install_oltp_manuallink" name="install_oltp_manuallink"></a>

## <a name="next-steps"></a>Sonraki adımlar

- [Quickstart 1: Daha hızlı T-SQL Performansı için Bellek içi OLTP Teknolojileri](https://msdn.microsoft.com/library/mt694156.aspx)
- [Varolan bir Azure SQL uygulamasında Bellek Içi OLTP'yi kullanma](sql-database-in-memory-oltp-migration.md)
- Bellek [IÇI OLTP için Bellek İçi OLTP depolamasını izleyin](sql-database-in-memory-oltp-monitoring.md)
- [Azure SQL Veritabanı'nda Bellek Içi özellikleri deneyin](sql-database-in-memory-sample.md)

## <a name="additional-resources"></a>Ek kaynaklar

### <a name="deeper-information"></a>Daha derin bilgiler

- [SQL Veritabanı'nda In-Memory OLTP ile DTU'nun %70 oranında azaltılarken Quorum'un anahtar veritabanının iş yükünü nasıl ikikatına çıkardığını öğrenin](https://customers.microsoft.com/story/quorum-doubles-key-databases-workload-while-lowering-dtu-with-sql-database)
- [Azure SQL Veritabanı Blog Gönderisinde Bellek Içi OLTP](https://azure.microsoft.com/blog/in-memory-oltp-in-azure-sql-database/)
- [Bellek IÇI OLTP hakkında bilgi edinin](https://msdn.microsoft.com/library/dn133186.aspx)
- [Sütun mağaza dizinleri hakkında bilgi edinin](https://msdn.microsoft.com/library/gg492088.aspx)
- [Gerçek zamanlı operasyonel analitik hakkında bilgi edinin](https://msdn.microsoft.com/library/dn817827.aspx)
- Bkz. [Ortak İş Yükü Desenleri ve Geçiş Hususları](https://msdn.microsoft.com/library/dn673538.aspx) (Bellek içi OLTP'nin sıklıkla önemli performans kazançları sağladığı iş yükü modellerini açıklar)

### <a name="application-design"></a>Uygulama tasarımı

- [Bellek Içi OLTP (Bellek Içi Optimizasyon)](https://msdn.microsoft.com/library/dn133186.aspx)
- [Varolan bir Azure SQL uygulamasında Bellek Içi OLTP'yi kullanma](sql-database-in-memory-oltp-migration.md)

### <a name="tools"></a>Araçlar

- [Azure portalında](https://portal.azure.com/)
- [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx)
- [SQL Server Veri Araçları (SSDT)](https://msdn.microsoft.com/library/mt204009.aspx)
