---
title: Geliştirme en iyi uygulamaları
description: Synapse SQL havuzu için çözümler geliştirirken bilmeniz gereken öneriler ve en iyi uygulamalar.
services: synapse-analytics
author: XiaoyuMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 09/04/2018
ms.author: xiaoyul
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: a4902d2817c98b002b9767ad55d7ac60f5cb0c93
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80350702"
---
# <a name="development-best-practices-for-synapse-sql-pool"></a>Synapse SQL havuzu için geliştirme en iyi uygulamaları
Bu makalede, veri ambarı çözümünüzü geliştirirken kılavuz ve en iyi uygulamalar açıklanmaktadır. 

## <a name="tune-query-performance-with-new-product-enhancements"></a>Yeni ürün geliştirmeleriyle sorgu performansını ayarlama  
- [Gerçekleştirilmiş görünümler ile performans ayarlama](performance-tuning-materialized-views.md)
- [Sıralı kümelenmiş columnstore dizini ile performans ayarlama](performance-tuning-ordered-cci.md)
- [Sonuç kümesini önbelleğe ile performans ayarlama](performance-tuning-result-set-caching.md)

## <a name="reduce-cost-with-pause-and-scale"></a>Duraklatma ve ölçeklendirme ile maliyetleri azaltın
Duraklatma ve ölçeklendirme aracılığıyla maliyetleri azaltma hakkında daha fazla bilgi için bkz. [İşlem yönetme](sql-data-warehouse-manage-compute-overview.md). 

## <a name="maintain-statistics"></a>İstatistiklerin bakımını yapın
Synapse SQL havuzu, sütunlar üzerinde istatistikleri otomatik olarak algılamak ve oluşturmak için yapılandırılabilir.  En iyi duruma getirici tarafından oluşturulan sorgu planları yalnızca kullanılabilir istatistikler kadar iyidir.  Veritabanlarınız için AUTO_CREATE_STATISTICS etkinleştirmenizi ve sorgularınızda kullanılan sütunlarla ilgili istatistiklerin her zaman güncel olduğundan emin olmak için istatistikleri her yükten sonra günlük veya sonra güncel tutmanızı öneririz. 

Tüm istatistiklerinizi güncelleştirmenin çok uzun sürdüğünü düşünüyorsanız, hangi sütunların sık istatistik güncelleştirmelerine ihtiyaç duyduğu konusunda daha seçici olmayı deneyebilirsiniz. Örneğin, yeni değer eklenme ihtimali olan tarih sütunlarını her gün güncelleştirmeyi tercih edebilirsiniz. **Birleştirmelerde yer alan sütunlar, WHERE yan tümcesi kullanılan sütunlar ve GROUP BY'de bulunan sütunlar hakkında güncelleştirilmiş istatistikler alarak en fazla faydayı elde eeceksiniz.**

Ayrıca bkz. [Tablo istatistiklerini yönetme](sql-data-warehouse-tables-statistics.md), [CREATE STATISTICS](sql-data-warehouse-tables-statistics.md), [UPDATE STATISTICS](sql-data-warehouse-tables-statistics.md#updating-statistics)

## <a name="hash-distribute-large-tables"></a>Büyük tabloları karma olarak dağıtın
Tablolar varsayılan olarak Hepsini Bir Kez Deneme yöntemiyle dağıtılmıştır.  Bu tasarım, kullanıcıların tablolarının nasıl dağıtılması gerektiğine karar vermek zorunda kalmadan tablo oluşturmaya başlamalarını kolaylaştırır.  Hepsini Bir Kez Deneme tabloları, belirli iş yükleri için yeterli performans sunabilir ancak birçok durumda dağıtım sütunu seçilmesi daha iyi sonuç verecektir.  Sütuna göre dağıtılmış bir tablonun Hepsini Bir Kez Deneme tablosundan daha iyi performans sunacağı bir örnek, iki büyük bilgi tablosunun birleştirilmesidir.  Örneğin, order_id ile dağıtılmış bir sipariş tablonuz ve yine order_id ile dağıtılmış bir işlem tablonuz varsa, sipariş tablonuzla işlem tablonuzu order_id üzerinden birleştirdiğinizde, ilgili sorgu bir geçiş sorgusu olur ve veri taşıma işlemleri atlanır.  Adım sayısı ne kadar az olursa sorgu o kadar hızlı işlenir.  Taşınan veri miktarı azaldıkça sorgunun hızı artar.  Bu açıklama sadece yüzeyi çizer. Dağıtılmış bir tablo yüklenirken, yükleme işleminin yavaşlamaması için gelen verilerinizin dağıtım anahtarına göre sıralanmamış olduğundan emin olun.  Bir dağıtım sütunu seçmenin performansı nasıl artırabileceği ve CREATE TABLES eksedenizin WITH yan tümcesinde dağıtılmış bir tablonun nasıl tanımlanabileceği hakkında daha fazla bilgi için aşağıdaki bağlantılara bakın.

Ayrıca bkz. [Tabloya genel bakış](sql-data-warehouse-tables-overview.md), [Tablo dağıtımı](sql-data-warehouse-tables-distribute.md), [Tablo dağıtımına genel bakış](https://blogs.msdn.microsoft.com/sqlcat/20../../choosing-hash-distributed-table-vs-round-robin-distributed-table-in-azure-sql-dw-service/), [CREATE TABLE](sql-data-warehouse-tables-overview.md), [CREATE TABLE AS SELECT](sql-data-warehouse-develop-ctas.md)

## <a name="do-not-over-partition"></a>Aşırı bölümleme yapmayın
Verileri bölümleme, bölüm değiştirme veya taramaları en iyi duruma geçirerek verilerinizi korumak için etkili olabilirken, çok fazla bölüm olması sorgularınızı yavaşlatabilir.  Genellikle SQL Server'da iyi çalışabilecek yüksek parçalılık bölümleme stratejisi SQL havuzunda iyi çalışmayabilir.  Bölüm sayısının çok fazla olması, her bir bölümdeki satır sayısının 1 milyondan az olması halinde kümelenmiş columnstore dizinlerinin verimini de düşürebilir.  Sql havuzunun arka planda verilerinizi sizin için 60 veritabanına bölümlediğini unutmayın, bu nedenle 100 bölümden bir tablo oluşturursanız, bu aslında 6000 bölümle sonuçlanır.  Her iş yükü farklı olduğundan, en iyi yöntem deneme yanılma ile iş yükünüze en uygun bölümleme şeklini belirlemektir.  SQL Server’da kullandığınızdan daha az parça kullanmayı deneyin.  Örneğin, günlük bölümler yerine haftalık veya aylık bölümler kullanın.

Ayrıca bkz. [Tablo bölümleme](sql-data-warehouse-tables-partition.md)

## <a name="minimize-transaction-sizes"></a>İşlem boyutları en aza indirin
Bir işlemde çalışan INSERT, UPDATE ve DELETE deyimleri başarısız olduğunda gerçekleştirilen adımların geri alınması gerekir.  Uzun sürecek bir geri alma işlemi olasılığını en aza indirmek için işlem boyutlarını mümkün oldukça küçültün.  Bunu yapmak için INSERT, UPDATE ve DELETE deyimlerini parçalara ayırabilirsiniz.  Örneğin, mümkünse 1 saat sürmesini beklediğiniz bir INSERT'inuz varsa, INSERT'i her biri 15 dakika içinde çalışacak dört parçaya ayırın.  Geri alma riskini azaltmak için CTAS, TRUNCATE, DROP TABLE veya INSERT gibi özel En Az Günlüğe Kaydetme durumlarında boş tablo kullanın.  Geri alma işlemlerini ortadan kaldırmanın başka bir yöntemi de veri yönetimi için bölüm değiştirme gibi Yalnızca Meta Veri işlemlerini kullanmaktır.  Örneğin, bir tablodaki order_date değerleri Ekim 2001 içinde olan tüm satırları silmek amacıyla bir DELETE deyimi çalıştırmak yerine verilerinizi aylık bölümlere ayırıp bölümü başka bir tablonun boş bölümüyle değiştirebilirsiniz (ALTER TABLE örneklerini inceleyin).  Bölümlenmemiş tablolar için DELETE deyimini kullanmak yerine CTAS kullanarak tutmak istediğiniz verileri bir tabloya yazabilirsiniz.  Bir CTAS aynı miktarda zaman alıyorsa, en az işlem günlüğe kaydetmeye sahip olduğu ve gerekirse hızlı bir şekilde iptal edilebildiği için çalıştırmak çok daha güvenli bir işlemdir.

Ayrıca bakınız [Hareketleri Anlama](sql-data-warehouse-develop-transactions.md), Hareketleri [Optimize Etme](sql-data-warehouse-develop-best-practices-transactions.md), Tablo [bölümleme](sql-data-warehouse-tables-partition.md), [TRUNCATE TABLE](https://msdn.microsoft.com/library/ms177570.aspx), ALTER [TABLE](https://msdn.microsoft.com/library/ms190273.aspx), [Select (CTAS) olarak tablo oluşturun.](sql-data-warehouse-develop-ctas.md)

## <a name="use-the-smallest-possible-column-size"></a>Mümkün olan en küçük sütun boyutunu kullanın
DDL'nizi tanımlarken, verilerinizi destekleyecek en küçük veri türünü kullanmak sorgu performansını artırır.  Bu durum özellikle CHAR ve VARCHAR sütunları için önemlidir.  Bir sütundaki en uzun değer 25 karakterse, sütununuzu VARCHAR(25) olarak tanımlayın.  Tüm karakter sütunları için varsayılan uzunluk değeri olarak yüksek bir değer kullanmaktan kaçının.  Ayrıca, NVARCHAR şart değilse sütunları VARCHAR olarak tanımlayın.

Ayrıca bkz. [Tabloya genel bakış](sql-data-warehouse-tables-overview.md), [Tablo veri türleri](sql-data-warehouse-tables-data-types.md), [CREATE TABLE](sql-data-warehouse-tables-overview.md)

## <a name="optimize-clustered-columnstore-tables"></a>Kümelenmiş columnstore tablolarını iyileştirin
Kümelenmiş sütun deposu dizinleri, verilerinizi SQL havuzunda depolamanın en etkili yollarından biridir.  Varsayılan olarak, SQL havuzundaki tablolar Clustered ColumnStore olarak oluşturulur.  Columnstore tablolarında yapılan sorgularda en iyi performansı elde etmek için segment kalitesinin yüksek olması önemlidir.  Satırlar columnstore tablolarına bellek baskısı altında yazıldığında, segment kalitesi düşebilir.  Segment kalitesi, sıkıştırılmış Satır Grubu içindeki satır sayısıyla ölçülebilir.  Kümelenmiş columnstore tablolarının segment kalitesini tespit etme ve iyileştirme talimatları için [Tablo dizinleri](sql-data-warehouse-tables-index.md) makalesindeki [Columnstore dizin kalitesinin düşük olmasının nedenleri](sql-data-warehouse-tables-index.md#causes-of-poor-columnstore-index-quality) bölümüne bakın.  Yüksek kaliteli sütun deposu segmentleri önemli olduğundan, veri yüklemek için orta veya büyük kaynak sınıfında bulunan kullanıcı sözcülerini kullanmak iyi bir fikirdir. Daha düşük [veri ambarı birimlerini](what-is-a-data-warehouse-unit-dwu-cdwu.md) kullanmak, yükleme kullanıcınıza daha büyük bir kaynak sınıfı atamak istediğiniz anlamına gelir.

Sütun deposu tabloları genellikle tablo başına 1 milyondan fazla satır olana ve her SQL bilardo tablosu 60 tabloya bölünene kadar verileri sıkıştırılmış bir sütun mağazası segmentine itmediğinden, sütun lu tablolar bir sorgudan yararlanamaz. tabloda 60 milyondan fazla satır vardır.  60 milyondan az satıra sahip tablolarda columnstore dizini kullanmaya gerek olmayabilir.  Kullanmanın da bir zararı olmayacaktır.  Ayrıca, verilerinizi bölümlemeniz halinde her bir bölümün kümelenmiş columnstore dizini kullanabilmesi için en az 1 milyon satıra ihtiyaç duyacağını unutmayın.  100 bölüme sahip bir tablonun kümelenmiş columnstore kullanabilmesi için en az 6 milyar satıra sahip olması gerekir (60 dağıtım * 100 bölüm * 1 milyon satır).  Bu örnekte tablonuzda 6 milyar satır yoksa, bölüm sayısını azaltabilir veya yığın tablo kullanabilirsiniz.  Deneme yaparak columnstore tablosu yerine ikincil dizine sahip yığın tablo ile daha iyi performans elde edip etmeyeceğinizi görebilirsiniz.

Columnstore tablosunda çalıştırılan sorgular yalnızca ihtiyacınız olan sütunları seçmeniz halinde daha hızlı olacaktır.  

Ayrıca bkz. [Tablo dizinleri](sql-data-warehouse-tables-index.md), [Columnstore dizinleri kılavuzu](https://msdn.microsoft.com/library/gg492088.aspx), [Columnstore dizinlerini yeniden oluşturma](sql-data-warehouse-tables-index.md#rebuilding-indexes-to-improve-segment-quality)

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede aradığınızı bulamazsanız, azure Synapse Analytics belgelerinin tümlerinde arama yapmak için bu sayfanın sol tarafındaki "Doküman Ara"yı kullanmayı deneyin.  [Azure SQL Veri Ambarı Forumu,](https://social.msdn.microsoft.com/Forums/sqlserver/home?forum=AzureSQLDataWarehouse) diğer kullanıcılara ve SQL Veri Ambarı Ürün Grubu'na soru sorabileceğiniz bir yerdir.  Sorularınızın diğer kullanıcılar veya ekibimiz tarafından yanıtlandığından emin olmak için bu forumu sürekli takip ediyoruz.  Sorularınızı Stack Overflow sitesinde sormak isterseniz, [Azure SQL Veri Ambarı Stack Overflow Forumu](https://stackoverflow.com/questions/tagged/azure-sqldw)’nu da kullanabilirsiniz.

Son olarak özellik isteğinde bulunmak için lütfen [Azure SQL Veri Ambarı Geri Bildirim](https://feedback.azure.com/forums/307516-sql-data-warehouse) sayfasını kullanın.  İsteklerinizi eklemeniz veya diğer istekleri oylamanız, özellikleri önceliklendirme konusunda bize yardımcı olmaktadır.


