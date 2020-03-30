---
title: Esnek sorguya genel bakış
description: Elastik sorgu, birden çok veritabanını kapsayan bir Transact-SQL sorgusu çalıştırmanızı sağlar.
services: sql-database
ms.service: sql-database
ms.subservice: scale-out
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MladjoA
ms.author: mlandzic
ms.reviewer: sstein
ms.date: 12/05/2019
ms.openlocfilehash: 827fab0661a58bfa7d28452960ea6df64d18bf84
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74873752"
---
# <a name="azure-sql-database-elastic-query-overview-preview"></a>Azure SQL Veritabanı elastik sorgu genel bakışı (önizleme)

Elastik sorgu özelliği (önizlemede), Azure SQL Veritabanı'nda birden çok veritabanına yayılan bir Transact-SQL sorgusu çalıştırmanızı sağlar. Uzak tablolara erişmek için veritabanı arası sorgular gerçekleştirmenize ve Microsoft ve üçüncü taraf araçlarını (Excel, Power BI, Tableau, vb.) birden çok veritabanı içeren veri katmanları arasında sorgulamak için bağlamanızı sağlar. Bu özelliği kullanarak, sorguları SQL Veritabanı'ndaki büyük veri katmanlarına ölçeklendirebilir ve sonuçları iş zekası (BI) raporlarında görselleştirebilirsiniz.

## <a name="why-use-elastic-queries"></a>Neden elastik sorgular kullanın

### <a name="azure-sql-database"></a>Azure SQL Veritabanı

Azure SQL veritabanları nda tamamen T-SQL'de sorgula. Bu, uzak veritabanlarının salt okunur sorgulanmasına olanak tanır ve geçerli şirket içi SQL Server müşterilerinin üç ve dört parçalı adları veya sql DB'ye bağlı sunucu kullanarak uygulamaları geçirmeleri için bir seçenek sağlar.

### <a name="available-on-standard-tier"></a>Standart katmanda mevcuttur

Esnek sorgu, hem Standart hem de Premium hizmet katmanlarında desteklenir. Daha düşük hizmet katmanları için performans sınırlamaları ile ilgili aşağıdaki Önizleme Sınırlamaları bölümüne bakın.

### <a name="push-parameters-to-remote-databases"></a>Parametreleri uzak veritabanlarına itme

Esnek sorgular artık SQL parametrelerini yürütme için uzak veritabanlarına itebilir.

### <a name="stored-procedure-execution"></a>Depolanan yordam yürütme

[SP\_execute \_remote](https://msdn.microsoft.com/library/mt703714)kullanarak uzaktan depolanan yordam çağrıları veya uzak işlevleri yürütmek.

### <a name="flexibility"></a>Esneklik

Elastik sorguiçeren dış tablolar, farklı bir şemaya veya tablo adına sahip uzak tablolara başvurabilir.

## <a name="elastic-query-scenarios"></a>Esnek sorgu senaryoları

Amaç, birden çok veritabanının satırları tek bir genel sonuca katkıda bulunduğu senaryoları sorgulamayı kolaylaştırmaktır. Sorgu, kullanıcı veya uygulama tarafından doğrudan veya dolaylı olarak veritabanına bağlı araçlar aracılığıyla oluşturulabilir. Bu, özellikle rapor oluştururken, ticari BI veya veri tümleştirme araçlarını veya değiştirilemeyen herhangi bir uygulama kullanırken kullanışlıdır. Esnek bir sorguyla, Excel, Power BI, Tableau veya Cognos gibi araçlarda tanıdık SQL Server bağlantı deneyimini kullanarak çeşitli veritabanları arasında sorgu yapabilirsiniz.
Esnek bir sorgu, SQL Server Management Studio veya Visual Studio tarafından verilen sorgular aracılığıyla tüm veritabanları koleksiyonuna kolay erişim sağlar ve Entity Framework veya diğer ORM ortamlarından çapraz veritabanı sorgusunun kolaylaştırılmasını kolaylaştırır. Şekil 1, varolan bir bulut uygulamasının [(elastik veritabanı istemcikitaplığını](sql-database-elastic-database-client-library.md)kullanan) ölçeklenmiş bir veri katmanı üzerinde oluşturduğu ve veritabanı arası raporlama için esnek bir sorgunun kullanıldığı bir senaryoyu gösterir.

**Şekil 1** Ölçeklenmiş veri katmanında kullanılan elastik sorgu

![Ölçeklenmiş veri katmanında kullanılan elastik sorgu][1]

Elastik sorgu için müşteri senaryoları aşağıdaki topolojilerle karakterize edilir:

* **Dikey bölümleme - Veritabanı** arası sorgular (Topoloji 1): Veriler, veri katmanındaki bir dizi veritabanları arasında dikey olarak bölümlere ayrılmıştır. Genellikle, farklı tablo kümeleri farklı veritabanlarında yer almaktadır. Bu şema farklı veritabanlarında farklı olduğu anlamına gelir. Örneğin, muhasebeyle ilgili tüm tablolar ikinci bir veritabanında yken, stok için tüm tablolar tek bir veritabanındadır. Bu topolojiile ortak kullanım örnekleri arasında sorgu veya çeşitli veritabanlarında tablolar arasında rapor derlemek gerekir.
* **Yatay Bölümleme - Parçalama** (Topoloji 2): Veriler, satırları ölçeklenmiş bir veri katmanına dağıtmak için yatay olarak bölümlenir. Bu yaklaşımla, şema tüm katılımcı veritabanlarında aynıdır. Bu yaklaşıma "parçalama" da denir. (1) elastik veritabanı araçları kitaplıkları veya (2) kendi kendine parçalama kullanılarak parçalama yapılabilir ve yönetilebilir. Birçok parça arasında raporları sorgulamak veya derlemek için elastik bir sorgu kullanılır. Kırıklar genellikle elastik bir havuz içindeki veritabanlarıdır. Veritabanları ortak şema paylaştığı sürece, elastik sorguyu aynı anda tüm elastik havuzun tüm veritabanlarını sorgulamak için etkili bir yol olarak düşünebilirsiniz.

> [!NOTE]
> Elastik sorgu, işlemenin (filtreleme, toplama) çoğunun dış kaynak tarafında gerçekleştirilebildiği senaryoları raporlamak için en iyi sonucu gösterir. Uzak veritabanından büyük miktarda veri aktarıldığı ETL işlemleri için uygun değildir. Ağır raporlama iş yükleri veya daha karmaşık sorgulara sahip veri depolama senaryoları için [Azure Synapse Analytics'i](https://azure.microsoft.com/services/synapse-analytics)de kullanmayı düşünün.
>  

## <a name="vertical-partitioning---cross-database-queries"></a>Dikey bölümleme - veritabanı arası sorgular

Kodlamaya başlamak için [bkz.](sql-database-elastic-query-getting-started-vertical.md)

Elastik bir sorgu, SQL veritabanında bulunan verileri diğer SQL veritabanları için kullanılabilir hale getirmek için kullanılabilir. Bu, bir veritabanındaki sorguların başka bir uzak SQL veritabanındaki tablolara başvurmasına olanak tanır. İlk adım, her uzak veritabanı için harici bir veri kaynağı tanımlamaktır. Dış veri kaynağı, uzak veritabanında bulunan tablolara erişmek istediğiniz yerel veritabanında tanımlanır. Uzak veritabanında değişiklik gerekmez. Farklı veritabanlarının farklı şemalara sahip olduğu tipik dikey bölümleme senaryoları için, başvuru verilerine erişim ve veritabanı arası sorgulama gibi yaygın kullanım durumlarını uygulamak için elastik sorgular kullanılabilir.

> [!IMPORTANT]
> ALTER HER HANGİ Bİr DIS VERİ KAYNAĞI İznine sahip olmalısınız. Bu izin ALTER DATABASE iznine dahildir. ALTER HERHANGI BIR Dış VERI Kaynağı izinleri temel veri kaynağıbaşvurmak için gereklidir.
>

**Referans veriler**: Topoloji referans veri yönetimi için kullanılır. Aşağıdaki şekilde, referans verileri ile iki tablo (T1 ve T2) özel bir veritabanında tutulur. Elastik bir sorgu kullanarak, artık şekilde gösterildiği gibi, diğer veritabanlarından t1 ve T2 tablolarına uzaktan erişebilirsiniz. Başvuru tabloları küçük veya uzak sorgular başvuru tablosuna seçici yüklemler varsa topoloji 1 kullanın.

**Şekil 2** Dikey bölümleme - Başvuru verilerini sorgulamak için elastik sorgu kullanma

![Dikey bölümleme - Başvuru verilerini sorgulamak için elastik sorgu kullanma][3]

**Çapraz veritabanı sorgulama**: Elastik sorgular, çeşitli SQL veritabanlarında sorgu gerektiren kullanım durumlarını etkinleştirin. Şekil 3 dört farklı veritabanları gösterir: CRM, Envanter, İk ve Ürünler. Veritabanlarından birinde gerçekleştirilen sorguların da diğer veritabanlarından birine veya tümüne erişmeleri gerekir. Esnek bir sorgu kullanarak, dört veritabanının her birinde birkaç basit DDL deyimleri çalıştırarak veritabanınızı bu durum için yapılandırabilirsiniz. Bu tek seferlik yapılandırmadan sonra, uzak bir tabloya erişim, T-SQL sorgularınızdan veya BI araçlarınızdan yerel bir tabloya atıfta bulunulmak kadar basittir. Uzak sorgular büyük sonuçlar döndürmezse bu yaklaşım önerilir.

**Şekil 3** Dikey bölümleme - Çeşitli veritabanları arasında sorgulama küçelim

![Dikey bölümleme - Çeşitli veritabanları arasında sorgulama küçelim][4]

Aşağıdaki adımlar, aynı şemaya sahip uzak SQL veritabanlarında bulunan bir tabloya erişim gerektiren dikey bölümleme senaryoları için esnek veritabanı sorgularını yapılandırır:

* [MASTER KEY](https://msdn.microsoft.com/library/ms174382.aspx) mymasterkey OLUŞTUR
* [VERITABANı KAPSAMLI Kimlik](https://msdn.microsoft.com/library/mt270260.aspx) MiyatiYAL OLUŞTURMA
* [CREATE/DROP EXTERNAL DATA SOURCE](https://msdn.microsoft.com/library/dn935022.aspx) tip **RDBMS** mydatasource
* [CREATE/DROP EXTERNAL TABLE](https://msdn.microsoft.com/library/dn935021.aspx) mytable

DDL deyimlerini çalıştırdıktan sonra, uzak tablo "mytable" yerel bir tablo gibi erişebilirsiniz. Azure SQL Veritabanı otomatik olarak uzak veritabanına bir bağlantı açar, isteğinizi uzak veritabanında işler ve sonuçları döndürür.

## <a name="horizontal-partitioning---sharding"></a>Yatay bölümleme - parçalama

Parçalı, yani yatay olarak bölümlenmiş veri katmanı üzerinden raporlama görevlerini gerçekleştirmek için esnek sorgu yu kullanmak, veri katmanının veritabanlarını temsil etmek için elastik bir [veritabanı parçası eşlemi](sql-database-elastic-scale-shard-map-management.md) gerektirir. Genellikle, bu senaryoda yalnızca tek bir parça eşlem kullanılır ve elastik sorgu özelliklerine (kafa düğümü) sahip özel bir veritabanı sorguları raporlama için giriş noktası olarak hizmet vermektedir. Yalnızca bu özel veritabanının parça haritaya erişmesi gerekir. Şekil 4 bu topolojiyi ve yapılandırmasını elastik sorgu veritabanı ve parça harita ile göstermektedir. Veri katmanındaki veritabanları herhangi bir Azure SQL Veritabanı sürümüne veya sürümüne ait olabilir. Elastik veritabanı istemci kitaplığı ve basılı haritalar oluşturma hakkında daha fazla bilgi için [Shard harita yönetimine](sql-database-elastic-scale-shard-map-management.md)bakın.

**Şekil 4** Yatay bölümleme - Parçalanmış veri katmanları üzerinden raporlama için elastik sorgu kullanma

![Yatay bölümleme - Parçalanmış veri katmanları üzerinden raporlama için elastik sorgu kullanma][5]

> [!NOTE]
> Elastik Sorgu Veritabanı (kafa düğümü) ayrı bir veritabanı olabilir veya parça eşlemi barındıran aynı veritabanı olabilir.
> Hangi yapılandırmayı seçerseniz seçin, bu veritabanının hizmet katmanı ve bilgi işlem boyutunun beklenen oturum açma/sorgu isteklerini işlemek için yeterince yüksek olduğundan emin olun.

Aşağıdaki adımlar, (genellikle) birkaç uzak SQL veritabanlarında bulunan tablolar kümesine erişim gerektiren yatay bölümleme senaryoları için elastik veritabanı sorgularını yapılandırır:

* [MASTER KEY](https://docs.microsoft.com/sql/t-sql/statements/create-master-key-transact-sql) mymasterkey OLUŞTUR
* [VERITABANı KAPSAMLI Kimlik](https://docs.microsoft.com/sql/t-sql/statements/create-database-scoped-credential-transact-sql) MiyatiYAL OLUŞTURMA
* Elastik veritabanı istemci kitaplığını kullanarak veri katmanınızı temsil eden basılı bir [harita](sql-database-elastic-scale-shard-map-management.md) oluşturun.
* [CREATE/DROP EXTERNAL DATA SOURCE](https://docs.microsoft.com/sql/t-sql/statements/create-external-data-source-transact-sql) mydatasource of type **SHARD_MAP_MANAGER**
* [CREATE/DROP EXTERNAL TABLE](https://docs.microsoft.com/sql/t-sql/statements/create-external-table-transact-sql) mytable

Bu adımları gerçekleştirdikten sonra, yatay olarak bölümlenmiş tablo "mytable" yerel bir tablo yatmış gibi erişebilirsiniz. Azure SQL Veritabanı, tabloların fiziksel olarak depolandığı uzak veritabanlarına otomatik olarak birden çok paralel bağlantı açar, uzak veritabanlarındaki istekleri işler ve sonuçları döndürür.
Yatay bölümleme senaryosu için gerekli adımlar hakkında daha fazla bilgi [yatay bölümleme için elastik sorgu](sql-database-elastic-query-horizontal-partitioning.md)bulunabilir.

Kodlamaya başlamak için bkz: [Yatay bölümleme (parçalama) için elastik sorguyla başlarken.](sql-database-elastic-query-getting-started.md)

> [!IMPORTANT]
> Büyük bir veritabanları kümesi üzerinde esnek sorgunun başarılı bir şekilde yürütülmesi, sorgu yürütme sırasında veritabanlarının her birinin kullanılabilirliğine büyük ölçüde dayanır. Veritabanlarından biri kullanılamıyorsa, sorgunun tamamı başarısız olur. Aynı anda yüzlerce veya binlerce veritabanısorgulamayı planlıyorsanız, istemci uygulamanızın mantığı yeniden denediğinden emin olun veya [Elastik Veritabanı İşleri'nden](https://docs.microsoft.com/azure/sql-database/sql-database-job-automation-overview#elastic-database-jobs-preview) (önizleme) yararlanmayı ve daha küçük veritabanları alt kümelerini sorgulayarak her sorgunun sonuçlarını tek bir hedefte birleştirmeyi düşünün.

## <a name="t-sql-querying"></a>T-SQL sorgulama

Harici veri kaynaklarınızı ve dış tablolarınızı tanımladıktan sonra, dış tablolarınızı tanımladığınız veritabanlarına bağlanmak için normal SQL Server bağlantı dizelerini kullanabilirsiniz. Daha sonra, aşağıda özetlenen sınırlamalarla ilgili olarak harici tablolarınız üzerinde T-SQL deyimleri çalıştırabilirsiniz. [Yatay bölümleme](sql-database-elastic-query-horizontal-partitioning.md) ve dikey [bölümleme](sql-database-elastic-query-vertical-partitioning.md)için dokümantasyon konularında daha fazla bilgi ve T-SQL sorguları örnekleri bulabilirsiniz.

## <a name="connectivity-for-tools"></a>Araçlar için bağlantı

Uygulamalarınızı ve BI veya veri tümleştirme araçlarını harici tabloları olan veritabanlarına bağlamak için normal SQL Server bağlantı dizelerini kullanabilirsiniz. SQL Server'ın aracınız için veri kaynağı olarak desteklendirdiğinden emin olun. Bağlandıktan sonra, aracınızla bağlantı kurduğunuz diğer SQL Server veritabanında yaptığınız gibi, elastik sorgu veritabanına ve o veritabanındaki dış tablolara bakın.

> [!IMPORTANT]
> Elastik sorgularla Azure Etkin Dizini kullanılarak kimlik doğrulaması şu anda desteklenmez.

## <a name="cost"></a>Maliyet

Elastik sorgu, Azure SQL Veritabanı veritabanlarının maliyetine dahildir. Uzak veritabanlarınızın elastik sorgu bitiş noktasından farklı bir veri merkezinde olduğu topolojilerin desteklendiğine, ancak uzak veritabanlarından gelen verilerin düzenli olarak [Azure hızlarından](https://azure.microsoft.com/pricing/details/data-transfers/)ücretlendirildiğini unutmayın.

## <a name="preview-limitations"></a>Önizleme sınırlamaları

* İlk esnek sorgunuzu çalıştırmak Standart servis katmanında birkaç dakika kadar sürebilir. Bu süre elastik sorgu işlevselliğini yüklemek için gereklidir; yükleme performansı, daha yüksek hizmet katmanları ve işlem boyutlarıyla artar.
* SSMS veya SSDT'den dış veri kaynaklarının veya dış tabloların komut dosyası komut dosyası henüz desteklenmedi.
* SQL DB için alma/dışa aktarma henüz dış veri kaynaklarını ve dış tabloları desteklemez. Dışa aktarma/dışa aktarma kullanmanız gerekiyorsa, bu nesneleri dışa aktarmadan önce bırakın ve sonra içe aktardıktan sonra yeniden oluşturun.
* Esnek sorgu şu anda yalnızca salt okunur dış tablolara erişimi destekler. Ancak, dış tablonun tanımlandığı veritabanında tam T-SQL işlevini kullanabilirsiniz. Bu, örneğin, örneğin,> local_table INTO <<column_list> kullanarak veya dış tablolara başvuran esnek sorgu veritabanında depolanan yordamları tanımlamak için geçici sonuçlar kalıcı olabilir.
* Nvarchar (max) dışında, LOB türleri (uzamsal türler dahil) dış tablo tanımlarında desteklenmez. Geçici çözüm olarak, lob türünü nvarchar(max) içine atan uzak veritabanında bir görünüm oluşturabilir, taban tablo yerine dış tablonuzu görünümün üzerinde tanımlayabilir ve ardından sorgularınızda özgün LOB türüne geri dökümyapabilirsiniz.
* Sonuç olarak nvarchar(max) veri türü sütunları Elastik Sorgu uygulamasında kullanılan gelişmiş toplu iş tekniklerini devre dışı bırakarak ve büyüklük sırası için sorgunun performansını, hatta büyük miktarda kanonik olmayan kullanım durumlarında iki büyüklük sırasını etkileyebilir sorgu sonucunda toplu olmayan veriler aktarılıyor.
* Dış tablolar üzerindeki sütun istatistikleri şu anda desteklenmez. Tablo istatistikleri desteklenir, ancak el ile oluşturulması gerekir.
* Esnek sorgu yalnızca Azure SQL Veritabanı ile çalışır. Şirket içi SQL Server veya SQL Server'ı VM'de sorgulamak için kullanamazsınız.

## <a name="feedback"></a>Geri Bildirim

Aşağıdaki elastik sorgularla, MSDN forumlarında veya Stack Overflow'da deneyiminizi bizimle paylaşın. Biz hizmet (kusurları, pürüzlü kenarları, özellik boşlukları) hakkında geribildirim her türlü ilgileniyoruz.

## <a name="next-steps"></a>Sonraki adımlar

* Dikey bölümleme öğreticisi için [bkz.](sql-database-elastic-query-getting-started-vertical.md)
* Dikey olarak bölümlenmiş veriler için sözdizimi ve örnek sorgular için [bkz.](sql-database-elastic-query-vertical-partitioning.md)
* Yatay bölümleme (parçalama) öğreticisi [için](sql-database-elastic-query-getting-started.md)bkz.
* Yatay olarak bölümlenmiş veriler için sözdizimi ve örnek sorgular için bkz: [Yatay olarak bölümlenmiş verileri sorgula)](sql-database-elastic-query-horizontal-partitioning.md)
* Tek bir uzak Azure SQL Veritabanında Transact-SQL deyimini yürüten veya yatay bir bölümleme düzeninde parça olarak hizmet veren veritabanları kümesini yürüten [sp\_ \_execute remote'a](https://msdn.microsoft.com/library/mt703714) bakın.

<!--Image references-->
[1]: ./media/sql-database-elastic-query-overview/overview.png
[2]: ./media/sql-database-elastic-query-overview/topology1.png
[3]: ./media/sql-database-elastic-query-overview/vertpartrrefdata.png
[4]: ./media/sql-database-elastic-query-overview/verticalpartitioning.png
[5]: ./media/sql-database-elastic-query-overview/horizontalpartitioning.png

<!--anchors-->
