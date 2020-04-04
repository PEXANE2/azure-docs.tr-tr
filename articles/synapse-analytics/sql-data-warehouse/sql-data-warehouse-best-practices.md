---
title: Azure Synapse Analytics'teki Synapse SQL havuzu için en iyi uygulamalar (eski adıyla SQL DW)
description: Azure Synapse Analytics'te (eski adıyla SQL DW) SQL havuzu için çözümler geliştirmek için öneriler ve en iyi uygulamalar.
services: synapse-analytics
author: mlee3gsd
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 11/04/2019
ms.author: martinle
ms.reviewer: igorstan
ms.openlocfilehash: 0a2a49546a31f6d767b5e89348dc6b703278d877
ms.sourcegitcommit: d597800237783fc384875123ba47aab5671ceb88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80633623"
---
# <a name="best-practices-for-synapse-sql-pool-in-azure-synapse-analytics-formerly-sql-dw"></a>Azure Synapse Analytics'teki Synapse SQL havuzu için en iyi uygulamalar (eski adıyla SQL DW)

Bu makale, [SQL havuz](sql-data-warehouse-overview-what-is.md) dağıtımınızdan en iyi performansı elde etmenize yardımcı olacak en iyi uygulamalar topluluğudur.  Bu makalenin amacı, bazı temel rehberlik vermek ve odak önemli alanları vurgulamak.  

## <a name="reduce-cost-with-pause-and-scale"></a>Duraklatma ve ölçeklendirme ile maliyetleri azaltın

Duraklatma ve ölçeklendirme aracılığıyla maliyetleri azaltma hakkında daha fazla bilgi için bkz. [İşlem yönetme](sql-data-warehouse-manage-compute-overview.md).

## <a name="maintain-statistics"></a>İstatistiklerin bakımını yapın

SQL havuzu, sütunlarda istatistikleri otomatik olarak algılamak ve oluşturmak için yapılandırılabilir.  En iyi duruma getirici tarafından oluşturulan sorgu planları yalnızca kullanılabilir istatistikler kadar iyidir.  

Veritabanlarınız için AUTO_CREATE_STATISTICS etkinleştirmenizi ve sorgularınızda kullanılan sütunlarla ilgili istatistiklerin her zaman güncel olduğundan emin olmak için istatistikleri her yükten sonra günlük olarak veya sonra güncel tutmanızı öneririz.

Tüm istatistiklerinizi güncelleştirmenin çok uzun sürdüğünü düşünüyorsanız, hangi sütunların sık istatistik güncelleştirmelerine ihtiyaç duyduğu konusunda daha seçici olmayı deneyebilirsiniz. Örneğin, yeni değer eklenme ihtimali olan tarih sütunlarını her gün güncelleştirmeyi tercih edebilirsiniz.

> [!TIP]
> Birleştirmelerde yer alan sütunlar, WHERE yan tümcesi kullanılan sütunlar ve GROUP BY'de bulunan sütunlar hakkında güncelleştirilmiş istatistikler alarak en fazla faydayı elde eeceksiniz.

Ayrıca bakınız [Tablo istatistiklerini yönetin,](sql-data-warehouse-tables-statistics.md) [İstATİstİkLER OLUŞTURUN](https://msdn.microsoft.com/library/ms188038.aspx)ve [İstATİstİkLerİ GÜNCELLİr.](https://msdn.microsoft.com/library/ms187348.aspx)

## <a name="use-dmvs-to-monitor-and-optimize-your-queries"></a>Sorgularınızı izlemek ve iyileştirmek için DMV’leri kullanın

SQL havuzunda sorgu yürütmesini izlemek için kullanılabilecek birkaç DMV vardır.  DMVs makalesini kullanarak iş yükünüzü izleyin, yürütme sorgusunun ayrıntılarına nasıl bakacağınıza ilişkin adım adım yönergeleri ayrıntılarıyla açıklar.  

Bu DMV’lerdeki sorguları hızlıca bulmak için sorgularınızla LABEL seçeneğini kullanabilirsiniz.

Ayrıca [bakınız DMVs,](sql-data-warehouse-manage-monitor.md) [LABEL](sql-data-warehouse-develop-label.md), [OPTION](https://msdn.microsoft.com/library/ms190322.aspx), [sys.dm_exec_sessions]( https://msdn.microsoft.com/library/ms176013.aspx), [sys.dm_pdw_exec_requests](https://msdn.microsoft.com/library/mt203887.aspx), [sys.dm_pdw_request_steps](https://msdn.microsoft.com/library/mt203913.aspx), [sys.dm_pdw_sql_requests](https://msdn.microsoft.com/library/mt203889.aspx), [sys.dm_pdw_dms_workers](https://msdn.microsoft.com/library/mt203878.aspx), [DBCC PDW_SHOWEXECUTIONPLAN](https://msdn.microsoft.com/library/mt204017.aspx)ve [sys.dm_pdw_waits](https://msdn.microsoft.com/library/mt203893.aspx)kullanarak iş yükünüzü izleyin.

## <a name="tune-query-performance-with-new-product-enhancements"></a>Yeni ürün geliştirmeleriyle sorgu performansını ayarlama

- [Gerçekleştirilmiş görünümler ile performans ayarlama](performance-tuning-materialized-views.md)
- [Sıralı kümelenmiş columnstore dizini ile performans ayarlama](performance-tuning-ordered-cci.md)
- [Sonuç kümesini önbelleğe ile performans ayarlama](performance-tuning-result-set-caching.md)

## <a name="group-insert-statements-into-batches"></a>INSERT deyimlerini gruplayın

INSERT deyimi yle küçük bir tabloya tek seferlik yük veya hatta periyodik olarak yeniden yükleme gibi `INSERT INTO MyLookup VALUES (1, 'Type 1')`bir ifadeyle ihtiyaçlarınız için iyi bir performans gösterebilir.  

Ancak gün içinde binlerce veya milyonlarca satır yüklemeniz gerekiyorsa, ayrı ayrı INSERT deyimleri gerekli performansı göstermeyebilir.  Bunun yerine işlemlerinizi bir dosyaya yazılacağı ve başka bir işlemin belirli aralıklarla bu dosyayı yükleyeceği şekilde geliştirin.

Ayrıca bakınız [INSERT](https://msdn.microsoft.com/library/ms174335.aspx).

## <a name="use-polybase-to-load-and-export-data-quickly"></a>Verileri hızlıca yüklemek ve dışarı aktarmak için PolyBase kullanın

SQL havuzu, Azure Veri Fabrikası, PolyBase ve BCP gibi çeşitli araçlar aracılığıyla veri yüklemeyi ve dışa aktarmayı destekler.  Performansın yüksek öneme sahip olmadığı küçük miktarlardaki veriler için bu araçlardan herhangi birini kullanabilirsiniz.  Ancak, büyük hacimli verileri yüklerken veya dışarı aktarırken ya da yüksek performansa ihtiyaç duyduğunuzda, PolyBase en iyi çözüm olacaktır.  

PolyBase, MPP (Massively Parallel Processing) mimarisinden yararlanmak üzere tasarlanmıştır ve veri büyüklüklerini diğer araçlardan daha hızlı yükleyecek ve dışa aktaracaktır.  PolyBase yükleri CTAS veya INSERT INTO ile çalıştırılabilir.  

> [!TIP]
> CTAS, işlem günlüğünü en aza indiren ve verilerinizi en hızlı yükleyen seçenektir.

Azure Veri Fabrikası ayrıca PolyBase yüklerini destekler ve CTAS ile benzer bir performans elde edebilir.  PolyBase, Gzip de dahil olmak üzere birçok dosya biçimi için destek sunar.  
  
> [!NOTE]
> Gzip metin dosyalarını kullanırken elde etmeyi en üst düzeye çıkarmak için, yükünüzün paralelliğini en üst düzeye çıkarmak için dosyaları 60 veya daha fazla dosyaya ayırın.  Toplam hızı artırmak için verilerinizi aynı anda yükleyin.

Ayrıca [bkz. Veri Yükle](design-elt-data-loading.md), [PolyBase kullanmak için kılavuz](guidance-for-loading-data.md), SQL havuz yükleme [desenleri ve stratejileri](https://blogs.msdn.microsoft.com/sqlcat/20../../), Azure Veri Fabrikası ile Veri Yükleme , Azure Veri [Fabrikası]( ../../data-factory/load-azure-sql-data-warehouse.md) [ile veri taşıma](../../data-factory/transform-data-using-machine-learning.md), ( ,https://msdn.microsoft.com/library/dn935026.aspx)( , seçin [(CTAS) olarak tablo oluştur](sql-data-warehouse-develop-ctas.md).

## <a name="load-then-query-external-tables"></a>Dış tabloları önce yükleyip sonra sorgu çalıştırın

Dış tablolar olarak da bilinen Polybase, veri yüklemenin en hızlı yolu olsa da sorgular için en iyi çözüm değildir. Polybase tabloları şu anda yalnızca Azure blob dosyalarını ve Azure Veri Gölü depolama alanını destekler. Bu dosyaları destekleyen herhangi bir işlem kaynağı yoktur.  

Sonuç olarak, SQL havuzu bu işi boşaltamaz ve bu nedenle verileri okumak için tempdb'ye yükleyerek dosyanın tamamını okumalıdır.  Sonuç olarak bu veriler için birden fazla sorgunuz varsa, verileri bir kez yükleyip sorguların yerel tabloyu kullanmalarını sağlamak daha iyi olacaktır.

Ayrıca [PolyBase kullanmak için Kılavuzu'na](guidance-for-loading-data.md)bakın.

## <a name="hash-distribute-large-tables"></a>Büyük tabloları karma olarak dağıtın

Tablolar varsayılan olarak Hepsini Bir Kez Deneme yöntemiyle dağıtılmıştır.  Bu durum, kullanıcıların tabloların dağıtma şekli hakkında düşünmelerine gerek kalmadan tablo oluşturmaya başlamalarını sağlar.  Hepsini Bir Kez Deneme tabloları, belirli iş yükleri için yeterli performans sunabilir ancak birçok durumda dağıtım sütunu seçilmesi daha iyi sonuç verecektir.  

Sütuna göre dağıtılmış bir tablonun Hepsini Bir Kez Deneme tablosundan daha iyi performans sunacağı bir örnek, iki büyük bilgi tablosunun birleştirilmesidir.  

Örneğin, order_id ile dağıtılmış bir sipariş tablonuz ve yine order_id ile dağıtılmış bir işlem tablonuz varsa, sipariş tablonuzla işlem tablonuzu order_id üzerinden birleştirdiğinizde, ilgili sorgu bir geçiş sorgusu olur ve veri taşıma işlemleri atlanır.  Adım sayısı ne kadar az olursa sorgu o kadar hızlı işlenir.  Taşınan veri miktarı azaldıkça sorgunun hızı artar.  

> [!TIP]
> Dağıtılmış bir tablo yüklenirken, yükleme işleminin yavaşlamaması için gelen verilerinizin dağıtım anahtarına göre sıralanmamış olduğundan emin olun.  

Create TABLE ekstrenizin WITH yan tümcesinde dağıtılmış bir tablonun nasıl tanımlanabileceği ve dağıtılmış tablonun nasıl tanımlanabileceği hakkında daha fazla bilgi için aşağıdaki bağlantılara bakın.

Ayrıca bakınız [Tablo genel bakış](sql-data-warehouse-tables-overview.md), Tablo [dağılımı](sql-data-warehouse-tables-distribute.md), Tablo dağılımı [nı seçme](https://blogs.msdn.microsoft.com/sqlcat/20../../choosing-hash-distributed-table-vs-round-robin-distributed-table-in-azure-sql-dw-service/), TABLO [OLUŞTUR](https://msdn.microsoft.com/library/mt203953.aspx), SELECT OLARAK [TABLO OLUŞTUR](https://msdn.microsoft.com/library/mt204041.aspx).

## <a name="do-not-over-partition"></a>Aşırı bölümleme yapmayın

Verileri bölümleme, bölüm değiştirme veya taramaları en iyi duruma geçirerek verilerinizi korumak için etkili olabilirken, çok fazla bölüm olması sorgularınızı yavaşlatabilir.  Genellikle SQL Server'da iyi çalışabilecek yüksek parçalılık bölümleme stratejisi SQL havuzunda iyi çalışmayabilir.  

Bölüm sayısının çok fazla olması, her bir bölümdeki satır sayısının 1 milyondan az olması halinde kümelenmiş columnstore dizinlerinin verimini de düşürebilir.  Sql havuzunun arka planda verilerinizi sizin için 60 veritabanına bölümlediğini unutmayın, bu nedenle 100 bölümden bir tablo oluşturursanız, bu aslında 6000 bölümle sonuçlanır.  

Her iş yükü farklı olduğundan, en iyi yöntem deneme yanılma ile iş yükünüze en uygun bölümleme şeklini belirlemektir.  SQL Server’da kullandığınızdan daha az parça kullanmayı deneyin.  Örneğin, günlük bölümler yerine haftalık veya aylık bölümler kullanın.

Ayrıca bakınız [Tablo bölümleme](sql-data-warehouse-tables-partition.md).

## <a name="minimize-transaction-sizes"></a>İşlem boyutları en aza indirin

Bir işlemde çalışan INSERT, UPDATE ve DELETE deyimleri başarısız olduğunda gerçekleştirilen adımların geri alınması gerekir.  Uzun sürecek bir geri alma işlemi olasılığını en aza indirmek için işlem boyutlarını mümkün oldukça küçültün.  Bunu yapmak için INSERT, UPDATE ve DELETE deyimlerini parçalara ayırabilirsiniz.  

Örneğin, mümkünse 1 saat sürmesini beklediğiniz bir INSERT'invarsa, INSERT'i her biri 15 dakika içinde çalışacak dört parçaya ayırın.  Geri alma riskini azaltmak için boş tablolara CTAS, TRUNCATE, DROP TABLE veya INSERT gibi özel Minimal Günlük kılıfları yararlanın.  

Geri alma işlemlerini ortadan kaldırmanın başka bir yöntemi de veri yönetimi için bölüm değiştirme gibi Yalnızca Meta Veri işlemlerini kullanmaktır.  Örneğin, order_date Ekim 2001'de olduğu tablodaki tüm satırları silmek için DELETE deyimi yürütmek yerine, verilerinizi aylık olarak bölümlerek ve ardından başka bir tablodan boş bir bölüm için verilerle bölümü değiştirebilirsiniz [(ALTER TABLE](https://msdn.microsoft.com/library/ms190273.aspx) örneklerine bakın).  

Bölümlenmemiş tablolar için DELETE kullanmak yerine tabloda tutmak istediğiniz verileri yazmak için CTAS kullanmayı düşünün.  Bir CTAS aynı miktarda zaman alıyorsa, en az işlem günlüğe kaydetmeye sahip olduğu ve gerekirse hızlı bir şekilde iptal edilebildiği için çalıştırmak çok daha güvenli bir işlemdir.

Ayrıca bakınız [Hareketleri Anlama](sql-data-warehouse-develop-transactions.md), Hareketleri [Optimize Etme](sql-data-warehouse-develop-best-practices-transactions.md), Tablo [bölümleme](sql-data-warehouse-tables-partition.md), [TRUNCATE TABLE](https://msdn.microsoft.com/library/ms177570.aspx), ALTER [TABLE](https://msdn.microsoft.com/library/ms190273.aspx), ve [Select (CTAS) olarak tablo oluşturun.](sql-data-warehouse-develop-ctas.md)

## <a name="reduce-query-result-sizes"></a>Sorgu sonucu boyutlarını küçültme

Bu adım, büyük sorgu sonucunun neden olduğu istemci tarafı sorunlarını önlemenize yardımcı olur.  Döndürülen satır sayısını azaltmak için sorgunuzu edinebilirsiniz. Bazı sorgu oluşturma araçları, her sorguya "üst N" sözdizimini eklemenize olanak sağlar.  Ayrıca, sorgu sonucunu geçici bir tabloya cetas olarak kullanabilir ve daha sonra alt düzey işleme için PolyBase dışa aktarımını kullanabilirsiniz.

## <a name="use-the-smallest-possible-column-size"></a>Mümkün olan en küçük sütun boyutunu kullanın

DDL'nizi tanımlarken, verilerinizi destekleyecek en küçük veri türünü kullanmak sorgu performansını artırır.  Bu yaklaşım özellikle CHAR ve VARCHAR sütunları için önemlidir.  

Bir sütundaki en uzun değer 25 karakterse, sütununuzu VARCHAR(25) olarak tanımlayın.  Tüm karakter sütunları için varsayılan uzunluk değeri olarak yüksek bir değer kullanmaktan kaçının.  Ayrıca, NVARCHAR şart değilse sütunları VARCHAR olarak tanımlayın.

Ayrıca [bakınız Tablo genel bakış](sql-data-warehouse-tables-overview.md), [Tablo veri türleri](sql-data-warehouse-tables-data-types.md), [TABLO OLUŞTUR](https://msdn.microsoft.com/library/mt203953.aspx).

## <a name="use-temporary-heap-tables-for-transient-data"></a>Geçiş verileri için geçici yığın tabloları kullanın

Verileri geçici olarak indirdiğinizde, yığın tablosu kullanmanın genel işlemi daha hızlı hale getireceğini görebilirsiniz.  Verileri yalnızca daha fazla dönüştürme gerçekleştirmeden önce hazırlamak için yüklüyorsanız, tabloyu yığın tablosuna yüklemek verileri kümelenmiş bir columnstore tablosuna yüklemekten çok daha hızlı olacaktır.  

Ayrıca verileri geçici bir tabloya yüklemek, tabloyu kalıcı bir depolama alanına yüklemekten çok daha hızlı gerçekleştirilecektir.  Geçici tablolar bir "#" ile başlar ve yalnızca onu oluşturan oturum tarafından erişilebilir, bu nedenle yalnızca sınırlı senaryolarda çalışabilirler.

Yığın tabloları, CREATE TABLE deyiminin WITH yan tümcesinde tanımlanır.  Geçici tablo kullanıyorsanız, onun için de istatistik oluşturmayı unutmayın.

Ayrıca bakınız [Geçici tablolar](sql-data-warehouse-tables-temporary.md), [TABLO OLUŞTUR](https://msdn.microsoft.com/library/mt203953.aspx), SELECT OLARAK [TABLO OLUŞTUR](https://msdn.microsoft.com/library/mt204041.aspx).

## <a name="optimize-clustered-columnstore-tables"></a>Kümelenmiş columnstore tablolarını iyileştirin

Kümelenmiş sütun deposu dizinleri, verilerinizi SQL havuzunda depolamanın en etkili yollarından biridir.  Varsayılan olarak, SQL havuzundaki tablolar Clustered ColumnStore olarak oluşturulur.  Columnstore tablolarında yapılan sorgularda en iyi performansı elde etmek için segment kalitesinin yüksek olması önemlidir.  

Satırlar columnstore tablolarına bellek baskısı altında yazıldığında, segment kalitesi düşebilir.  Segment kalitesi, sıkıştırılmış Satır Grubu içindeki satır sayısıyla ölçülebilir.  Kümelenmiş sütun deposu tabloları için segment kalitesini algılama ve iyileştirme yle ilgili adım adım yönergeler için [Tablo dizinleri](sql-data-warehouse-tables-index.md) makalesinde [zayıf sütun deposu dizin kalitesinin nedenleri'ne](sql-data-warehouse-tables-index.md#causes-of-poor-columnstore-index-quality) bakın.  

Yüksek kaliteli sütun deposu segmentleri önemli olduğundan, veri yüklemek için orta veya büyük kaynak sınıfında bulunan kullanıcı sözcülerini kullanmak iyi bir fikirdir. Daha düşük [veri ambarı birimlerini](what-is-a-data-warehouse-unit-dwu-cdwu.md) kullanmak, yükleme kullanıcınıza daha büyük bir kaynak sınıfı atamak istediğiniz anlamına gelir.

Sütun deposu tabloları genellikle tablo başına 1 milyondan fazla satır olana ve her SQL bilardo tablosu 60 tabloya bölünene kadar sıkıştırılmış sütun mağazası segmentine veri itmediğinden, sütun lu tablolar tablo60 milyondan fazla satır olmadığı sürece sorgudan yararlanamaz.  60 milyondan az satıra sahip tablolarda columnstore dizini kullanmaya gerek olmayabilir.  Kullanmanın da bir zararı olmayacaktır.  

Ayrıca, verilerinizi bölümlemeniz halinde her bir bölümün kümelenmiş columnstore dizini kullanabilmesi için en az 1 milyon satıra ihtiyaç duyacağını unutmayın.  Bir tabloda 100 bölüm varsa, kümelenmiş sütun deposundan (60 dağılım *100 bölüm 1* milyon satır) yararlanmak için en az 6 milyar satır olması gerekir.  

Bu örnekte tablonuzda 6 milyar satır yoksa, bölüm sayısını azaltabilir veya yığın tablo kullanabilirsiniz.  Deneme yaparak columnstore tablosu yerine ikincil dizine sahip yığın tablo ile daha iyi performans elde edip etmeyeceğinizi görebilirsiniz.

> [!TIP]
> Columnstore tablosunda çalıştırılan sorgular yalnızca ihtiyacınız olan sütunları seçmeniz halinde daha hızlı olacaktır.  

Ayrıca bkz. [Tablo dizinleri](sql-data-warehouse-tables-index.md), [Columnstore dizinleri kılavuzu](https://msdn.microsoft.com/library/gg492088.aspx), [Columnstore dizinlerini yeniden oluşturma](sql-data-warehouse-tables-index.md#rebuilding-indexes-to-improve-segment-quality)

## <a name="use-larger-resource-class-to-improve-query-performance"></a>Sorgu performansını artırmak için daha büyük kaynak sınıfı kullanın

SQL havuzu, bellekleri sorgulara ayırmanın bir yolu olarak kaynak gruplarını kullanır.  Kutunun dışında, tüm kullanıcılar dağıtım başına 100 MB bellek veren küçük kaynak sınıfına atanır.  Her zaman 60 dağıtım olduğundan ve her dağıtıma en az 100 MB verildiğinden, sistem genelinde ayrılan bellek toplam 6000 MB (yaklaşık 6 GB) olur.  

Büyük birleştirmeler veya kümelenmiş columnstore tablolarına yapılan yüklemeler gibi belirli sorgulara daha fazla bellek atanır.  Bazı sorgular, saf taramalar gibi, hiçbir yararı sağlayacaktır.  Ancak, daha büyük kaynak sınıflarını kullanmak eşzamanlılığı azaltır, bu nedenle tüm kullanıcılarınızı büyük bir kaynak sınıfına taşımadan önce bu etkiyi göz önünde bulundurmak isteyeceksiniz.

Ayrıca iş [yükü yönetimi için Kaynak sınıfları'na](resource-classes-for-workload-management.md)bakın.

## <a name="use-smaller-resource-class-to-increase-concurrency"></a>Eşzamanlılığı artırmak için daha küçük kaynak sınıfı kullanın

Kullanıcı sorgularının uzun bir gecikmeye sahip olduğunu fark ederseniz, kullanıcılarınızın daha büyük kaynak sınıflarında çalışıyor olması ve diğer sorguların sıraya alınmasına neden olan çok sayıda eşzamanlılık yuvası tüketiyor olması olabilir.  Kullanıcı sorgularının kuyrukta olup olmadığını görmek için `SELECT * FROM sys.dm_pdw_waits` çalıştırıp dönen satırlara bakın.

Ayrıca [bkz. İş yükü yönetimi için kaynak sınıfları](resource-classes-for-workload-management.md), [sys.dm_pdw_waits](https://msdn.microsoft.com/library/mt203893.aspx).

## <a name="other-resources"></a>Diğer kaynaklar

Genel sorunlar ve çözümleri için [Sorun giderme](sql-data-warehouse-troubleshoot.md) makalemizi de inceleyin.

Bu makalede aradığınızı bulamadıysanız, Azure Synapse belgelerinin tümlerinde arama yapmak için bu sayfanın sol tarafındaki "Doküman Ara"yı kullanmayı deneyin.  [Azure Synapse Forumu,](https://social.msdn.microsoft.com/Forums/sqlserver/home?forum=AzureSQLDataWarehouse) diğer kullanıcılara ve Azure Synapse Ürün Grubu'na soru göndermeniz gereken bir yerdir. Sorularınızın diğer kullanıcılar veya ekibimiz tarafından yanıtlandığından emin olmak için bu forumu sürekli takip ediyoruz.  

Stack Taşma ile ilgili sorularınızı sormayı tercih ederseniz, [bir Azure Synapse Stack Overflow Forumu](https://stackoverflow.com/questions/tagged/azure-sqldw)da var.

Özellik isteklerinde bulunmak için lütfen [Azure Synapse Geri Bildirim](https://feedback.azure.com/forums/307516-sql-data-warehouse) sayfasını kullanın.  İsteklerinizi eklemeniz veya diğer istekleri oylamanız, özellikleri önceliklendirme konusunda bize yardımcı olmaktadır.
