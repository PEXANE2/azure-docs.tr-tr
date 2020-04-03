---
title: Azure Synapse Analytics'te SQL Analytics için en iyi uygulamalar (eski adıyla SQL DW)
description: Azure Synapse Analytics'te (eski adıyla SQL DW) SQL Analytics için çözümler geliştirmek için öneriler ve en iyi uygulamalar.
services: synapse-analytics
author: mlee3gsd
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 11/04/2019
ms.author: martinle
ms.reviewer: igorstan
ms.openlocfilehash: 44dbc03a41cfde94c344ae331b21d7536778050c
ms.sourcegitcommit: bc738d2986f9d9601921baf9dded778853489b16
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80619111"
---
# <a name="best-practices-for-sql-analytics-in-azure-synapse-analytics-formerly-sql-dw"></a>Azure Synapse Analytics'te SQL Analytics için en iyi uygulamalar (eski adıyla SQL DW)

Bu makale, [SQL Analytics](sql-data-warehouse-overview-what-is.md#synapse-sql-pool-in-azure-synapse) dağıtımınızdan en iyi performansı elde etmenize yardımcı olacak en iyi uygulamalar topluluğudur.  Bu makalenin amacı, bazı temel rehberlik vermek ve odak önemli alanları vurgulamak.  Her bölüm sizi bir kavramla tanıştırır ve daha sonra konsepti daha derinlemesine kapsayan daha ayrıntılı makalelere işaret eder. Konuların sırası önem sırasına göredir. 

## <a name="reduce-cost-with-pause-and-scale"></a>Duraklatma ve ölçeklendirme ile maliyetleri azaltın

Duraklatma ve ölçeklendirme aracılığıyla maliyetleri azaltma hakkında daha fazla bilgi için bkz. [İşlem yönetme](sql-data-warehouse-manage-compute-overview.md). 

## <a name="maintain-statistics"></a>İstatistiklerin bakımını yapın
Azure SQL Veri Ambarı, sütunlarda istatistikleri otomatik olarak algılamak ve oluşturmak için yapılandırılabilir.  En iyi duruma getirici tarafından oluşturulan sorgu planları yalnızca kullanılabilir istatistikler kadar iyidir.  Veritabanlarınız için AUTO_CREATE_STATISTICS etkinleştirmenizi ve sorgularınızda kullanılan sütunlarla ilgili istatistiklerin her zaman güncel olduğundan emin olmak için istatistikleri her yükten sonra günlük veya sonra güncel tutmanızı öneririz. 

Tüm istatistiklerinizi güncelleştirmenin çok uzun sürdüğünü düşünüyorsanız, hangi sütunların sık istatistik güncelleştirmelerine ihtiyaç duyduğu konusunda daha seçici olmayı deneyebilirsiniz. Örneğin, yeni değer eklenme ihtimali olan tarih sütunlarını her gün güncelleştirmeyi tercih edebilirsiniz. **Birleştirmelerde yer alan sütunlar, WHERE yan tümcesi kullanılan sütunlar ve GROUP BY'de bulunan sütunlar hakkında güncelleştirilmiş istatistikler alarak en fazla faydayı elde eeceksiniz.**

Ayrıca bkz. [Tablo istatistiklerini yönetme][Manage table statistics], [CREATE STATISTICS][CREATE STATISTICS], [UPDATE STATISTICS][UPDATE STATISTICS]

## <a name="use-dmvs-to-monitor-and-optimize-your-queries"></a>Sorgularınızı izlemek ve iyileştirmek için DMV’leri kullanın
SQL Analytics sorgu yürütmeizlemek için kullanılabilecek birkaç DMVs vardır.  Aşağıdaki izleme makalesinde çalıştırılan bir sorgunun ayrıntılarını incelemeyle ilgili adım adım talimatlar yer almaktadır.  Bu DMV’lerdeki sorguları hızlıca bulmak için sorgularınızla LABEL seçeneğini kullanabilirsiniz.

Ayrıca bkz. [DMV’leri kullanarak iş yükünüzü izleme][Monitor your workload using DMVs], [LABEL][LABEL], [OPTION][OPTION], [sys.dm_exec_sessions][sys.dm_exec_sessions], [sys.dm_pdw_exec_requests][sys.dm_pdw_exec_requests], [sys.dm_pdw_request_steps][sys.dm_pdw_request_steps], [sys.dm_pdw_sql_requests][sys.dm_pdw_sql_requests], [sys.dm_pdw_dms_workers], [DBCC PDW_SHOWEXECUTIONPLAN][DBCC PDW_SHOWEXECUTIONPLAN], [sys.dm_pdw_waits][sys.dm_pdw_waits]

## <a name="tune-query-performance-with-new-product-enhancements"></a>Yeni ürün geliştirmeleriyle sorgu performansını ayarlama
- [Gerçekleştirilmiş görünümler ile performans ayarlama](https://docs.microsoft.com/azure/sql-data-warehouse/performance-tuning-materialized-views)
- [Sıralı kümelenmiş columnstore dizini ile performans ayarlama](https://docs.microsoft.com/azure/sql-data-warehouse/performance-tuning-ordered-cci)
- [Sonuç kümesini önbelleğe ile performans ayarlama](https://docs.microsoft.com/azure/sql-data-warehouse/performance-tuning-result-set-caching)

## <a name="group-insert-statements-into-batches"></a>INSERT deyimlerini gruplayın
Küçük bir tabloya bir INSERT deyimiyle tek seferlik yükleme yapmak veya `INSERT INTO MyLookup VALUES (1, 'Type 1')` gibi bir deyimle bir aramanın düzenli aralıklarla yeniden yüklenmesi işinizi fazlasıyla görebilir.  Ancak gün içinde binlerce veya milyonlarca satır yüklemeniz gerekiyorsa, ayrı ayrı INSERT deyimleri gerekli performansı göstermeyebilir.  Bunun yerine işlemlerinizi bir dosyaya yazılacağı ve başka bir işlemin belirli aralıklarla bu dosyayı yükleyeceği şekilde geliştirin.

Ayrıca bkz. [INSERT][INSERT]

## <a name="use-polybase-to-load-and-export-data-quickly"></a>Verileri hızlıca yüklemek ve dışarı aktarmak için PolyBase kullanın

 SQL Analytics, Azure Veri Fabrikası, PolyBase ve BCP gibi çeşitli araçlar aracılığıyla veri yüklemeyi ve dışa aktarmayı destekler.  Performansın yüksek öneme sahip olmadığı küçük miktarlardaki veriler için bu araçlardan herhangi birini kullanabilirsiniz.  Ancak, büyük hacimli verileri yüklerken veya dışarı aktarırken ya da yüksek performansa ihtiyaç duyduğunuzda, PolyBase en iyi çözüm olacaktır.  
 
 PolyBase, MPP (Massively Parallel Processing) mimarisinden yararlanmak üzere tasarlanmıştır ve veri büyüklüklerini diğer araçlardan daha hızlı yükleyecek ve dışa aktaracaktır.  PolyBase yükleri CTAS veya INSERT INTO ile çalıştırılabilir.  **CTAS, işlem günlüğünü en aza indiren ve verilerinizi en hızlı yükleyen seçenektir.** 
 
  Azure Veri Fabrikası ayrıca PolyBase yüklerini destekler ve CTAS ile benzer bir performans elde edebilir.  PolyBase, Gzip de dahil olmak üzere birçok dosya biçimi için destek sunar.  **Gzip metin dosyalarını kullanırken elde etmeyi en üst düzeye çıkarmak için, yükünüzün paralelliğini en üst düzeye çıkarmak için dosyaları 60 veya daha fazla dosyaya ayırın.**  Toplam hızı artırmak için verilerinizi aynı anda yükleyin.

Ayrıca bakınız [Veri Yükle][Load data], [PolyBase kullanmak için kılavuz][Guide for using PolyBase], SQL havuz yükleme [desenleri ve stratejileri][Azure SQL Data Warehouse loading patterns and strategies], Azure Veri Fabrikası ile Veri [Yükleme][Load Data with Azure Data Factory], Azure Veri Fabrikası ile veri [taşıma][Move data with Azure Data Factory], CREATE EXTERNAL [FILE FORMAT][CREATE EXTERNAL FILE FORMAT], Create table as [select (CTAS)][Create table as select (CTAS)]

## <a name="load-then-query-external-tables"></a>Dış tabloları önce yükleyip sonra sorgu çalıştırın
Dış tablolar olarak da bilinen Polybase, veri yüklemenin en hızlı yolu olsa da sorgular için en iyi çözüm değildir. Polybase tabloları şu anda yalnızca Azure blob dosyalarını ve Azure Veri Gölü depolama alanını destekler. Bu dosyaları destekleyen herhangi bir işlem kaynağı yoktur.  

Sonuç olarak, SQL Analytics bu çalışmayı boşaltamaz ve bu nedenle verileri okumak için dosyayı tempdb'ye yükleyerek dosyanın tamamını okumalıdır.  Sonuç olarak bu veriler için birden fazla sorgunuz varsa, verileri bir kez yükleyip sorguların yerel tabloyu kullanmalarını sağlamak daha iyi olacaktır.

Ayrıca bkz. [PolyBase kullanma kılavuzu][Guide for using PolyBase]

## <a name="hash-distribute-large-tables"></a>Büyük tabloları karma olarak dağıtın
Tablolar varsayılan olarak Hepsini Bir Kez Deneme yöntemiyle dağıtılmıştır.  Bu durum, kullanıcıların tabloların dağıtma şekli hakkında düşünmelerine gerek kalmadan tablo oluşturmaya başlamalarını sağlar.  Hepsini Bir Kez Deneme tabloları, belirli iş yükleri için yeterli performans sunabilir ancak birçok durumda dağıtım sütunu seçilmesi daha iyi sonuç verecektir.  Sütuna göre dağıtılmış bir tablonun Hepsini Bir Kez Deneme tablosundan daha iyi performans sunacağı bir örnek, iki büyük bilgi tablosunun birleştirilmesidir.  

Örneğin, order_id ile dağıtılmış bir sipariş tablonuz ve yine order_id ile dağıtılmış bir işlem tablonuz varsa, sipariş tablonuzla işlem tablonuzu order_id üzerinden birleştirdiğinizde, ilgili sorgu bir geçiş sorgusu olur ve veri taşıma işlemleri atlanır.  Adım sayısı ne kadar az olursa sorgu o kadar hızlı işlenir.  Taşınan veri miktarı azaldıkça sorgunun hızı artar.  

Dağıtılmış bir tablo yüklenirken, yükleme işleminin yavaşlamaması için gelen verilerinizin dağıtım anahtarına göre sıralanmamış olduğundan emin olun.  Bir dağıtım sütunu seçmenin performansı nasıl artırabileceği ve CREATE TABLE ekskünüzün WITH yan tümcesinde dağıtılmış bir tablonun nasıl tanımlanabileceği hakkında daha fazla bilgi için aşağıdaki bağlantılara bakın.

Ayrıca bkz. [Tabloya genel bakış][Table overview], [Tablo dağıtımı][Table distribution], [Tablo dağıtımına genel bakış][Selecting table distribution], [CREATE TABLE][CREATE TABLE], [CREATE TABLE AS SELECT][CREATE TABLE AS SELECT]

## <a name="do-not-over-partition"></a>Aşırı bölümleme yapmayın
Verileri bölümleme, bölüm değiştirme veya taramaları en iyi duruma geçirerek verilerinizi korumak için etkili olabilirken, çok fazla bölüm olması sorgularınızı yavaşlatabilir.  Genellikle SQL Server'da iyi çalışabilecek yüksek parçalılık bölümleme stratejisi SQL Analytics'te iyi çalışmayabilir.  

Bölüm sayısının çok fazla olması, her bir bölümdeki satır sayısının 1 milyondan az olması halinde kümelenmiş columnstore dizinlerinin verimini de düşürebilir.  SQL Analytics'in arka planda verilerinizi sizin için 60 veritabanına bölümlediğini unutmayın, böylece 100 bölümden bir tablo oluşturursanız, bu aslında 6000 bölümle sonuçlanır.  

Her iş yükü farklı olduğundan, en iyi yöntem deneme yanılma ile iş yükünüze en uygun bölümleme şeklini belirlemektir.  SQL Server’da kullandığınızdan daha az parça kullanmayı deneyin.  Örneğin, günlük bölümler yerine haftalık veya aylık bölümler kullanın.

Ayrıca bkz. [Tablo bölümleme][Table partitioning]

## <a name="minimize-transaction-sizes"></a>İşlem boyutları en aza indirin
Bir işlemde çalışan INSERT, UPDATE ve DELETE deyimleri başarısız olduğunda gerçekleştirilen adımların geri alınması gerekir.  Uzun sürecek bir geri alma işlemi olasılığını en aza indirmek için işlem boyutlarını mümkün oldukça küçültün.  Bunu yapmak için INSERT, UPDATE ve DELETE deyimlerini parçalara ayırabilirsiniz.  

Örneğin, mümkünse 1 saat sürmesini beklediğiniz bir INSERT'invarsa, INSERT'i her biri 15 dakika içinde çalışacak dört parçaya ayırın.  Geri alma riskini azaltmak için CTAS, TRUNCATE, DROP TABLE veya INSERT gibi özel En Az Günlüğe Kaydetme durumlarında boş tablo kullanın.  

Geri alma işlemlerini ortadan kaldırmanın başka bir yöntemi de veri yönetimi için bölüm değiştirme gibi Yalnızca Meta Veri işlemlerini kullanmaktır.  Örneğin, bir tablodaki order_date değerleri Ekim 2001 içinde olan tüm satırları silmek amacıyla bir DELETE deyimi çalıştırmak yerine verilerinizi aylık bölümlere ayırıp bölümü başka bir tablonun boş bölümüyle değiştirebilirsiniz (ALTER TABLE örneklerini inceleyin).  

Bölümlenmemiş tablolar için DELETE kullanmak yerine tabloda tutmak istediğiniz verileri yazmak için CTAS kullanmayı düşünün.  Bir CTAS aynı miktarda zaman alıyorsa, en az işlem günlüğe kaydetmeye sahip olduğu ve gerekirse hızlı bir şekilde iptal edilebildiği için çalıştırmak çok daha güvenli bir işlemdir.

Ayrıca bkz. [İşlemleri anlama][Understanding transactions], [İşlemleri iyileştirme][Optimizing transactions], [Tablo bölümleme][Table partitioning], [TRUNCATE TABLE][TRUNCATE TABLE], [ALTER TABLE][ALTER TABLE], [Create table as select (CTAS)][Create table as select (CTAS)]

## <a name="reduce-query-result-sizes"></a>Sorgu sonucu boyutlarını küçültme  
Bu adım, büyük sorgu sonucunun neden olduğu istemci tarafı sorunlarını önlemenize yardımcı olur.  Döndürülen satır sayısını azaltmak için sorgunuzu edinebilirsiniz. Bazı sorgu oluşturma araçları, her sorguya "üst N" sözdizimini eklemenize olanak sağlar.  Ayrıca, sorgu sonucunu geçici bir tabloya cetas olarak kullanabilir ve daha sonra alt düzey işleme için PolyBase dışa aktarımını kullanabilirsiniz.

## <a name="use-the-smallest-possible-column-size"></a>Mümkün olan en küçük sütun boyutunu kullanın
DDL'nizi tanımlarken, verilerinizi destekleyecek en küçük veri türünü kullanmak sorgu performansını artırır.  Bu durum özellikle CHAR ve VARCHAR sütunları için önemlidir.  Bir sütundaki en uzun değer 25 karakterse, sütununuzu VARCHAR(25) olarak tanımlayın.  Tüm karakter sütunları için varsayılan uzunluk değeri olarak yüksek bir değer kullanmaktan kaçının.  Ayrıca, NVARCHAR şart değilse sütunları VARCHAR olarak tanımlayın.

Ayrıca bkz. [Tabloya genel bakış][Table overview], [Tablo veri türleri][Table data types], [CREATE TABLE][CREATE TABLE]

## <a name="use-temporary-heap-tables-for-transient-data"></a>Geçiş verileri için geçici yığın tabloları kullanın
Verileri geçici olarak indirdiğinizde, yığın tablosu kullanmanın genel işlemi daha hızlı hale getireceğini görebilirsiniz.  Verileri yalnızca daha fazla dönüştürme gerçekleştirmeden önce hazırlamak için yüklüyorsanız, tabloyu yığın tablosuna yüklemek verileri kümelenmiş bir columnstore tablosuna yüklemekten çok daha hızlı olacaktır.  

Ayrıca verileri geçici bir tabloya yüklemek, tabloyu kalıcı bir depolama alanına yüklemekten çok daha hızlı gerçekleştirilecektir.  Geçici tablolar bir "#" ile başlar ve yalnızca onu oluşturan oturum tarafından erişilebilir, bu nedenle yalnızca sınırlı senaryolarda çalışabilirler.   Yığın tabloları, CREATE TABLE deyiminin WITH yan tümcesinde tanımlanır.  Geçici tablo kullanıyorsanız, onun için de istatistik oluşturmayı unutmayın.

Ayrıca bkz. [Geçici tablolar][Temporary tables], [CREATE TABLE][CREATE TABLE], [CREATE TABLE AS SELECT][CREATE TABLE AS SELECT]

## <a name="optimize-clustered-columnstore-tables"></a>Kümelenmiş columnstore tablolarını iyileştirin
Kümelenmiş sütun deposu dizinleri, verilerinizi SQL Analytics'te depolamanın en etkili yollarından biridir.  Varsayılan olarak, SQL Analytics'teki tablolar Clustered ColumnStore olarak oluşturulur.  Columnstore tablolarında yapılan sorgularda en iyi performansı elde etmek için segment kalitesinin yüksek olması önemlidir.  

Satırlar columnstore tablolarına bellek baskısı altında yazıldığında, segment kalitesi düşebilir.  Segment kalitesi, sıkıştırılmış Satır Grubu içindeki satır sayısıyla ölçülebilir.  Kümelenmiş columnstore tablolarının segment kalitesini tespit etme ve iyileştirme talimatları için [Tablo dizinleri][Table indexes] makalesindeki [Columnstore dizin kalitesinin düşük olmasının nedenleri][Causes of poor columnstore index quality] bölümüne bakın.  

Yüksek kaliteli sütun deposu segmentleri önemli olduğundan, veri yüklemek için orta veya büyük kaynak sınıfında bulunan kullanıcı sözcülerini kullanmak iyi bir fikirdir. Daha düşük [veri ambarı birimlerini](what-is-a-data-warehouse-unit-dwu-cdwu.md) kullanmak, yükleme kullanıcınıza daha büyük bir kaynak sınıfı atamak istediğiniz anlamına gelir.

Sütun deposu tabloları genellikle tablo başına 1 milyondan fazla satır olana ve her SQL Analytics tablosu 60 tabloya bölünene kadar sıkıştırılmış sütun mağazası segmentine veri itmediğinden, sütun lu tablo, tabloda 60 milyondan fazla satır olmadığı sürece sorgudan yararlanamaz.  60 milyondan az satıra sahip tablolarda columnstore dizini kullanmaya gerek olmayabilir.  Kullanmanın da bir zararı olmayacaktır.  

Ayrıca, verilerinizi bölümlemeniz halinde her bir bölümün kümelenmiş columnstore dizini kullanabilmesi için en az 1 milyon satıra ihtiyaç duyacağını unutmayın.  100 bölüme sahip bir tablonun kümelenmiş columnstore kullanabilmesi için en az 6 milyar satıra sahip olması gerekir (60 dağıtım * 100 bölüm * 1 milyon satır).  

Bu örnekte tablonuzda 6 milyar satır yoksa, bölüm sayısını azaltabilir veya yığın tablo kullanabilirsiniz.  Deneme yaparak columnstore tablosu yerine ikincil dizine sahip yığın tablo ile daha iyi performans elde edip etmeyeceğinizi görebilirsiniz.

Columnstore tablosunda çalıştırılan sorgular yalnızca ihtiyacınız olan sütunları seçmeniz halinde daha hızlı olacaktır.  

Ayrıca bkz. [Tablo dizinleri][Table indexes], [Columnstore dizinleri kılavuzu][Columnstore indexes guide], [Columnstore dizinlerini yeniden oluşturma][Rebuilding columnstore indexes]

## <a name="use-larger-resource-class-to-improve-query-performance"></a>Sorgu performansını artırmak için daha büyük kaynak sınıfı kullanın
SQL Analytics, bellekleri sorgulara ayırmanın bir yolu olarak kaynak gruplarını kullanır.  Kutunun dışında, tüm kullanıcılar dağıtım başına 100 MB bellek veren küçük kaynak sınıfına atanır.  Her zaman 60 dağıtım olduğundan ve her dağıtıma en az 100 MB verildiğinden, sistem genelinde ayrılan bellek toplam 6000 MB (yaklaşık 6 GB) olur.  

Büyük birleştirmeler veya kümelenmiş columnstore tablolarına yapılan yüklemeler gibi belirli sorgulara daha fazla bellek atanır.  Yalnızca tarama gibi bazı sorgulara ek atama yapılmaz.  Diğer taraftan, daha büyük kaynak sınıflarını kullanmak eşzamanlılığı azaltır, bu nedenle tüm kullanıcılarınızı büyük bir kaynak sınıfına taşımadan önce bu etkiyi göz önünde bulundurmak isteyeceksiniz.

Ayrıca bkz. [İş yükü yönetimi için kaynak sınıfları](resource-classes-for-workload-management.md)

## <a name="use-smaller-resource-class-to-increase-concurrency"></a>Eşzamanlılığı artırmak için daha küçük kaynak sınıfı kullanın
Kullanıcı sorgularının uzun bir gecikmesi olduğunu fark ediyorsanız, kullanıcılarınızın daha büyük kaynak sınıflarında çalışıyor olması ve diğer sorguların sıraya alınmasına neden olan çok sayıda eşzamanlılık yuvası tüketiyor olması olabilir.  Kullanıcı sorgularının kuyrukta olup olmadığını görmek için `SELECT * FROM sys.dm_pdw_waits` çalıştırıp dönen satırlara bakın.

Ayrıca bkz. [İş yükü yönetimi için kaynak sınıfları](resource-classes-for-workload-management.md), [sys.dm_pdw_waits][sys.dm_pdw_waits]

## <a name="other-resources"></a>Diğer kaynaklar
Genel sorunlar ve çözümleri için [Sorun giderme][Troubleshooting] makalemizi de inceleyin.

Bu makalede aradığınızı bulamadıysanız, Azure Synapse belgelerinin tümlerinde arama yapmak için bu sayfanın sol tarafındaki "Doküman Ara"yı kullanmayı deneyin.  [Azure Synapse Forumu,][Azure SQL Data Warehouse MSDN Forum] diğer kullanıcılara ve Azure Synapse Ürün Grubu'na soru sorabileceğiniz bir yerdir. 

Sorularınızın diğer kullanıcılar veya ekibimiz tarafından yanıtlandığından emin olmak için bu forumu sürekli takip ediyoruz.  Stack Taşma ile ilgili sorularınızı sormayı tercih ederseniz, [bir Azure Synapse Stack Overflow Forumu][Azure SQL Data Warehouse Stack Overflow Forum]da var.

Son olarak, özellik isteklerinde bulunmak için lütfen [Azure Synapse Geri Bildirim][Azure SQL Data Warehouse Feedback] sayfasını kullanın.  İsteklerinizi eklemeniz veya diğer istekleri oylamanız, özellikleri önceliklendirme konusunda bize yardımcı olmaktadır.

<!--Image references-->

<!--Article references-->
[Create a support ticket]:sql-data-warehouse-get-started-create-support-ticket.md
[Create table as select (CTAS)]:sql-data-warehouse-develop-ctas.md
[Table overview]:sql-data-warehouse-tables-overview.md
[Table data types]:sql-data-warehouse-tables-data-types.md
[Table distribution]:sql-data-warehouse-tables-distribute.md
[Table indexes]:sql-data-warehouse-tables-index.md
[Causes of poor columnstore index quality]:sql-data-warehouse-tables-index.md#causes-of-poor-columnstore-index-quality
[Rebuilding columnstore indexes]:sql-data-warehouse-tables-index.md#rebuilding-indexes-to-improve-segment-quality
[Table partitioning]:sql-data-warehouse-tables-partition.md
[Manage table statistics]:sql-data-warehouse-tables-statistics.md
[Temporary tables]:sql-data-warehouse-tables-temporary.md
[Guide for using PolyBase]:guidance-for-loading-data.md
[Load data]:design-elt-data-loading.md
[Move data with Azure Data Factory]: ../../data-factory/transform-data-using-machine-learning.md
[Load data with Azure Data Factory]: ../../data-factory/load-azure-sql-data-warehouse.md
[Load data with bcp]: /sql/tools/bcp-utility
[Load data with PolyBase]:load-data-wideworldimportersdw.md
[Monitor your workload using DMVs]:sql-data-warehouse-manage-monitor.md
[Pause compute resources]: ../../sql-data-warehouse/sql-data-warehouse-manage-compute-overview.md#pause-compute-bk
[Resume compute resources]: ../../sql-data-warehouse/sql-data-warehouse-manage-compute-overview.md#resume-compute-bk
[Scale compute resources]: ../../sql-data-warehouse/sql-data-warehouse-manage-compute-overview.md#scale-compute
[Understanding transactions]:sql-data-warehouse-develop-transactions.md
[Optimizing transactions]:sql-data-warehouse-develop-best-practices-transactions.md
[Troubleshooting]:sql-data-warehouse-troubleshoot.md
[LABEL]:sql-data-warehouse-develop-label.md

<!--MSDN references-->
[ALTER TABLE]: https://msdn.microsoft.com/library/ms190273.aspx
[CREATE EXTERNAL FILE FORMAT]: https://msdn.microsoft.com/library/dn935026.aspx
[CREATE STATISTICS]: https://msdn.microsoft.com/library/ms188038.aspx
[CREATE TABLE]: https://msdn.microsoft.com/library/mt203953.aspx
[CREATE TABLE AS SELECT]: https://msdn.microsoft.com/library/mt204041.aspx
[DBCC PDW_SHOWEXECUTIONPLAN]: https://msdn.microsoft.com/library/mt204017.aspx
[INSERT]: https://msdn.microsoft.com/library/ms174335.aspx
[OPTION]: https://msdn.microsoft.com/library/ms190322.aspx
[TRUNCATE TABLE]: https://msdn.microsoft.com/library/ms177570.aspx
[UPDATE STATISTICS]: https://msdn.microsoft.com/library/ms187348.aspx
[sys.dm_exec_sessions]: https://msdn.microsoft.com/library/ms176013.aspx
[sys.dm_pdw_exec_requests]: https://msdn.microsoft.com/library/mt203887.aspx
[sys.dm_pdw_request_steps]: https://msdn.microsoft.com/library/mt203913.aspx
[sys.dm_pdw_sql_requests]: https://msdn.microsoft.com/library/mt203889.aspx
[sys.dm_pdw_dms_workers]: https://msdn.microsoft.com/library/mt203878.aspx
[sys.dm_pdw_waits]: https://msdn.microsoft.com/library/mt203893.aspx
[Columnstore indexes guide]: https://msdn.microsoft.com/library/gg492088.aspx

<!--Other Web references-->
[Selecting table distribution]: https://blogs.msdn.microsoft.com/sqlcat/20../../choosing-hash-distributed-table-vs-round-robin-distributed-table-in-azure-sql-dw-service/
[Azure SQL Data Warehouse Feedback]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[Azure SQL Data Warehouse MSDN Forum]: https://social.msdn.microsoft.com/Forums/sqlserver/home?forum=AzureSQLDataWarehouse
[Azure SQL Data Warehouse Stack Overflow Forum]:  https://stackoverflow.com/questions/tagged/azure-sqldw
[Azure SQL Data Warehouse loading patterns and strategies]: https://blogs.msdn.microsoft.com/sqlcat/20../../azure-sql-data-warehouse-loading-patterns-and-strategies/
