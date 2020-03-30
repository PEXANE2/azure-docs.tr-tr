---
title: Akıllı İçgörüler ile performans sorunlarını giderme
description: Intelligent Insights, Azure SQL Veritabanı performans sorunlarını gidermenize yardımcı olur.
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: danimir
ms.author: danil
ms.reviewer: jrasnik, carlrab
ms.date: 03/10/2020
ms.openlocfilehash: 739bba7ed9ab4770a762c08fccc422ce048ae11d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79214087"
---
# <a name="troubleshoot-azure-sql-database-performance-issues-with-intelligent-insights"></a>Intelligent Insights ile Azure SQL Veritabanı performans sorunlarını giderme

Bu sayfa, [Intelligent Insights](sql-database-intelligent-insights.md) kaynak günlüğü aracılığıyla algılanan Azure SQL Veritabanı ve Yönetilen Örnek performans sorunları hakkında bilgi sağlar. Ölçümler ve kaynak günlükleri Azure [Monitor günlüklerine,](../azure-monitor/insights/azure-sql.md) [Azure Etkinlik Hub'larına,](../azure-monitor/platform/resource-logs-stream-event-hubs.md) [Azure Depolama'ya](sql-database-metrics-diag-logging.md#stream-into-azure-storage)veya özel DevOps uyarı ve raporlama yetenekleri için üçüncü taraf çözümüne aktarılabilir.

> [!NOTE]
> Intelligent Insights'ı kullanan hızlı bir SQL Veritabanı performans sorun giderme kılavuzu için bu belgedeki [Önerilen sorun giderme akış akış](sql-database-intelligent-insights-troubleshoot-performance.md#recommended-troubleshooting-flow) şemasına bakın.

## <a name="detectable-database-performance-patterns"></a>Algılanabilir veritabanı performans desenleri

Intelligent Insights, sorgu yürütme bekleme sürelerine, hatalara veya zaman çıkışlarına bağlı olarak Azure SQL Veritabanı'ndaki veritabanlarındaki performans sorunlarını otomatik olarak algılar. Intelligent Insights çıktıları SQL Veritabanı kaynak günlüğüne performans desenleri algılandı. Algılanabilir performans desenleri aşağıdaki tabloda özetlenmiştir.

| Algılanabilir performans desenleri | Azure SQL Veritabanı ve esnek havuzlar için açıklama | Yönetilen Örnek'teki veritabanları için açıklama |
| :------------------- | ------------------- | ------------------- |
| [Kaynak sınırlarına ulaşma](sql-database-intelligent-insights-troubleshoot-performance.md#reaching-resource-limits) | Kullanılabilir kaynakların (DT'ler), veritabanı alt iş parçacıklarının veya izlenen abonelikte bulunan veritabanı oturumaçma oturumlarının tüketimi sınırlarına ulaşmıştır. Bu, SQL Veritabanı performansını etkiliyor. | CPU kaynaklarının tüketimi Yönetilen Örnek sınırlarına ulaşıyor. Bu veritabanı performansını etkiliyor. |
| [İş yükü artışı](sql-database-intelligent-insights-troubleshoot-performance.md#workload-increase) | Veritabanında iş yükü artışı veya sürekli iş yükü birikimi algılandı. Bu, SQL Veritabanı performansını etkiliyor. | İş yükü artışı algılandı. Bu veritabanı performansını etkiliyor. |
| [Bellek basıncı](sql-database-intelligent-insights-troubleshoot-performance.md#memory-pressure) | Bellek hibeleri isteyen işçiler, istatistiksel olarak önemli miktarda zaman için bellek ayırmalarını beklemek zorunda kalırlar veya istenen bellek hibeleri için artan bir birikim vardır. Bu, SQL Veritabanı performansını etkiliyor. | Bellek hibeleri isteyen işçiler, istatistiksel olarak önemli bir süre için bellek ayırmalarını beklerler. Bu veritabanı performansını etkiliyor. |
| [Kilitleme](sql-database-intelligent-insights-troubleshoot-performance.md#locking) | SQL Veritabanı performansını etkileyen aşırı veritabanı kilitleme algılandı. | Veritabanı performansını etkileyen aşırı veritabanı kilitleme algılandı. |
| [Artan MAXDOP](sql-database-intelligent-insights-troubleshoot-performance.md#increased-maxdop) | Paralellik seçeneğinin (MAXDOP) maksimum derecesi sorgu yürütme verimliliğini etkileyen değişti. Bu, SQL Veritabanı performansını etkiliyor. | Paralellik seçeneğinin (MAXDOP) maksimum derecesi sorgu yürütme verimliliğini etkileyen değişti. Bu veritabanı performansını etkiliyor. |
| [Pagelatch çekişme](sql-database-intelligent-insights-troubleshoot-performance.md#pagelatch-contention) | Birden çok iş parçacığı aynı anda aynı bellek içi veri arabellek sayfalarına erişmeye çalışarak bekleme sürelerinin artmasına ve pagelatch çekişmesine neden olur. Bu, SQL veritabanının performansını etkiliyor. | Birden çok iş parçacığı aynı anda aynı bellek içi veri arabellek sayfalarına erişmeye çalışarak bekleme sürelerinin artmasına ve pagelatch çekişmesine neden olur. Bu, veritabanının performansını etkiliyor. |
| [Eksik Dizini](sql-database-intelligent-insights-troubleshoot-performance.md#missing-index) | Eksik dizin SQL veritabanı performansını etkileyen algılandı. | Eksik dizin veritabanı performansını etkileyen algılandı. |
| [Yeni Sorgu](sql-database-intelligent-insights-troubleshoot-performance.md#new-query) | Genel SQL Veritabanı performansını etkileyen yeni sorgu algılandı. | Genel veritabanı performansını etkileyen yeni sorgu algılandı. |
| [Artırılmış Bekleme İstatistikleri](sql-database-intelligent-insights-troubleshoot-performance.md#increased-wait-statistic) | Artan veritabanı bekleme süreleri SQL veritabanı performansını etkileyen algılandı. | Veritabanı performansını etkileyen artırılmış veritabanı bekleme süreleri algılandı. |
| [TempDB Çekişme](sql-database-intelligent-insights-troubleshoot-performance.md#tempdb-contention) | Birden çok iş parçacığı, darboğaza neden olan aynı TempDB kaynağına erişmeye çalışıyor. Bu, SQL Veritabanı performansını etkiliyor. | Birden çok iş parçacığı, darboğaza neden olan aynı TempDB kaynağına erişmeye çalışıyor. Bu veritabanı performansını etkiliyor. |
| [Elastik havuz DTU sıkıntısı](sql-database-intelligent-insights-troubleshoot-performance.md#elastic-pool-dtu-shortage) | Elastik havuzdaki kullanılabilir eDT'lerin yetersizliği SQL Veritabanı performansını etkiliyor. | vCore modelini kullandığı için Yönetilen Örnek için kullanılamaz. |
| [Regresyon Planı](sql-database-intelligent-insights-troubleshoot-performance.md#plan-regression) | Yeni plan veya varolan bir planın iş yükünde bir değişiklik algılandı. Bu, SQL Veritabanı performansını etkiliyor. | Yeni plan veya varolan bir planın iş yükünde bir değişiklik algılandı. Bu veritabanı performansını etkiliyor. |
| [Veritabanı kapsamı nagöre yapılandırma değer değişikliği](sql-database-intelligent-insights-troubleshoot-performance.md#database-scoped-configuration-value-change) | SQL Veritabanı'ndaki yapılandırma değişikliği veritabanı performansını etkileyen algılandı. | Veritabanındaki yapılandırma değişikliği veritabanı performansını etkileyen algılandı. |
| [Yavaş istemci](sql-database-intelligent-insights-troubleshoot-performance.md#slow-client) | Yavaş uygulama istemcisi veritabanından çıktıyı yeterince hızlı tüketemiyor. Bu, SQL Veritabanı performansını etkiliyor. | Yavaş uygulama istemcisi veritabanından çıktıyı yeterince hızlı tüketemiyor. Bu veritabanı performansını etkiliyor. |
| [Fiyatlandırma katmanı düşürme](sql-database-intelligent-insights-troubleshoot-performance.md#pricing-tier-downgrade) | Fiyatlandırma katmanı düşürme eylemi kullanılabilir kaynakları azalttı. Bu, SQL Veritabanı performansını etkiliyor. | Fiyatlandırma katmanı düşürme eylemi kullanılabilir kaynakları azalttı. Bu veritabanı performansını etkiliyor. |

> [!TIP]
> SQL Veritabanı'nın sürekli performans optimizasyonu için [Azure SQL Veritabanı otomatik ayarını etkinleştirin.](sql-database-automatic-tuning.md) SQL Veritabanı yerleşik zekanın bu benzersiz özelliği, SQL veritabanınızı sürekli olarak izler, dizinleri otomatik olarak ait eder ve sorgu yürütme planı düzeltmelerini uygular.
>

Aşağıdaki bölümde daha ayrıntılı olarak algılanabilir performans desenleri açıklanır.

## <a name="reaching-resource-limits"></a>Kaynak sınırlarına ulaşma

### <a name="what-is-happening"></a>Ne oluyor

Bu algılanabilir performans deseni, kullanılabilir kaynak sınırlarına, alt çalışma sınırlarına ve oturum sınırlarına ulaşmakla ilgili performans sorunlarını birleştirir. Bu performans sorunu algılandıktan sonra, tanılama günlüğünün açıklama alanı, performans sorununun kaynak, alt veya oturum sınırlarıyla ilişkili olup olmadığını gösterir.

SQL Veritabanı'ndaki kaynaklar genellikle [DTU](sql-database-what-is-a-dtu.md) veya [vCore](sql-database-service-tiers-vcore.md) kaynaklarına başvurur. Algılanan sorgu performansı bozulması ölçülen kaynak sınırlarından herhangi birine ulaşıldığında kaynak sınırlarına ulaşma deseni tanınır.

Oturum, kaynak sınırlandırıyor, SQL veritabanında kullanılabilir eşzamanlı oturum açma sayısını gösterir. Bu performans deseni, SQL veritabanlarına bağlı uygulamalar veritabanında kullanılabilir eşzamanlı oturum açma sayısına ulaştığında tanınır. Uygulamalar veritabanında bulunandan daha fazla oturum kullanmaya çalışırsa, sorgu performansı etkilenir.

Kullanılabilir çalışanlar DTU veya vCore kullanımında sayılmadığından, çalışan sınırlarına ulaşmak, kaynak sınırlarına ulaşmanın özel bir örneğidir. Veritabanındaki alt sınıra ulaşmak kaynağa özgü bekleme sürelerinin artmasına neden olabilir ve bu da sorgu performansının düşmesine neden olabilir.

### <a name="troubleshooting"></a>Sorun giderme

Tanılama günlüğü, performans ve kaynak tüketim yüzdelerini etkileyen sorguların sorgu larını sorgular. Bu bilgileri veritabanı iş yükünüzü en iyi duruma duruma etmek için başlangıç noktası olarak kullanabilirsiniz. Özellikle, dizinler ekleyerek performans bozulmasını etkileyen sorguları en iyi duruma getirebilirsiniz. Veya uygulamaları daha eşit iş yükü dağılımıyla optimize edebilirsiniz. İş yüklerini azaltamıyor veya optimizasyon yapamıyorsanız, kullanılabilir kaynak miktarını artırmak için SQL veritabanı aboneliğinizin fiyatlandırma katmanını artırmayı düşünün.

Kullanılabilir oturum sınırlarına ulaştıysanız, veritabanına yapılan oturum sayısını azaltarak uygulamalarınızı en iyi duruma getirebilirsiniz. Uygulamalarınızdan veritabanına giriş sayısını azaltamıyorsanız, veritabanınızın fiyatlandırma katmanını artırmayı düşünün. Veya daha dengeli bir iş yükü dağıtımı için veritabanınızı birden çok veritabanına bölebilir ve taşıyabilirsiniz.

Oturum sınırlarını çözme yle ilgili daha fazla öneri için, [SQL Veritabanı maksimum oturum açma sınırlarıyla nasıl başa çıkılabildiğini](https://blogs.technet.microsoft.com/latam/20../../how-to-deal-with-the-limits-of-azure-sql-database-maximum-logins/)görün. Sunucu ve abonelik düzeylerindeki sınırlar hakkında bilgi için [bir SQL Veritabanı sunucusundaki kaynak sınırlarına genel bakış.](sql-database-resource-limits-database-server.md)

## <a name="workload-increase"></a>İş Yükü Artışı

### <a name="what-is-happening"></a>Ne oluyor

Bu performans deseni, iş yükü artışının veya daha ciddi formunda iş yükü yığınının neden olduğu sorunları tanımlar.

Bu algılama, çeşitli ölçümlerin bir kombinasyonu ile yapılır. Ölçülen temel metrik, geçmiş iş yükü taban çizgisiyle karşılaştırıldığında iş yükünde bir artış algılamaktır. Diğer algılama biçimi, sorgu performansını etkileyecek kadar büyük olan etkin alt iş parçacıklarındaki büyük bir artışı ölçmeye dayanır.

Daha ciddi formunda, SQL veritabanının iş yükünü işleyememesi nedeniyle iş yükü sürekli olarak birikebilir. Sonuç, sürekli büyüyen bir iş yükü boyutudur, bu da iş yükü yığlama koşuludur. Bu durum nedeniyle, iş yükünün yürütmeyi bekleme süresi büyür. Bu durum en ciddi veritabanı performans sorunlarından birini temsil eder. Bu sorun, iptal edilen alt iş parçacığı sayısındaki artış izleyerek algılanır.

### <a name="troubleshooting"></a>Sorun giderme

Tanılama günlüğü, yürütmesi artan sorgu sayısını ve iş yükü artışına en büyük katkıyı içeren sorgu nun sorgu karmasını çıkarır. Bu bilgileri iş yükünü en iyi duruma duruma etmek için bir başlangıç noktası olarak kullanabilirsiniz. İş yükü artışına en büyük katkıda bulunan kişi olarak tanımlanan sorgu, başlangıç noktanız olarak özellikle yararlıdır.

İş yüklerini veritabanına daha eşit bir şekilde dağıtmayı düşünebilirsiniz. Dizinler ekleyerek performansı etkileyen sorguyu en iyi duruma almayı düşünün. İş yükünüzü birden çok veritabanları arasında da dağıtabilirsiniz. Bu çözümler mümkün değilse, kullanılabilir kaynak miktarını artırmak için SQL veritabanı aboneliğinizin fiyatlandırma katmanını artırmayı düşünün.

## <a name="memory-pressure"></a>Bellek Baskısı

### <a name="what-is-happening"></a>Ne oluyor

Bu performans deseni, bellek basıncının neden olduğu geçerli veritabanı performansında veya daha ağır formunda, son yedi günlük performans taban çizgisiyle karşılaştırıldığında bir bellek yığını-up durumu ndaki bozulmayı gösterir.

Bellek basıncı, SQL veritabanında bellek hibeleri isteyen çok sayıda alt iş parçacığı nın bulunduğu bir performans durumunu gösterir. Yüksek ses düzeyi, SQL veritabanının belleği isteyen tüm çalışanlara verimli bir şekilde tahsis edemediği yüksek bellek kullanımı durumuna neden olur. Bu sorunun en yaygın nedenlerinden biri, bir yandan SQL veritabanıiçin kullanılabilir bellek miktarı ile ilgilidir. Öte yandan, iş yükündeki artış, alt iş parçacıklarında ve bellek basıncında artışa neden olur.

Bellek basıncının daha ağır formu bellek yığılma durumudur. Bu durum, bellek serbest sorguları daha fazla sayıda alt iş parçacığı bellek hibe leri istediğini gösterir. SQL veritabanı altyapısı talebi karşılamak için belleği verimli bir şekilde tahsis edemediğinden, bellek hibesi isteyen bu sayıda ki işçi iş parçacığı da sürekli olarak artabilir (birikiyor olabilir). Bellek yığını oluşturma durumu en ciddi veritabanı performansı sorunlarından birini temsil eder.

### <a name="troubleshooting"></a>Sorun giderme

Tanılama günlüğü, yüksek bellek kullanımı ve ilgili zaman damgaları için en yüksek neden olarak işaretlenmiş katip (diğer bir şekilde, alt iş parçacığı) ile bellek nesnesi depo ayrıntıları çıktıları. Bu bilgileri sorun giderme için temel olarak kullanabilirsiniz.

En yüksek bellek kullanımına sahip katiplerle ilgili sorguları en iyi duruma getirebilir veya kaldırabilirsiniz. Ayrıca, kullanmayı planlamadığınız verileri sorgulamadığınızdan da emin olabilirsiniz. İyi uygulama her zaman sorgularınızda bir WHERE yan tümcesi kullanmaktır. Ayrıca, verileri taramak yerine aramak için kümesiz dizinler oluşturmanızı öneririz.

Ayrıca, birden çok veritabanı üzerinden en iyi duruma veya dağıtarak iş yükünü azaltabilirsiniz. Veya iş yükünüzü birden çok veritabanları arasında dağıtabilirsiniz. Bu çözümler mümkün değilse, veritabanında bulunan bellek kaynaklarının miktarını artırmak için SQL veritabanı aboneliğinizin fiyatlandırma katmanını artırmayı düşünün.

Ek sorun giderme önerileri için [bkz: Bellek meditasyon verir: Birçok adile gizemli SQL Server bellek tüketici.](https://techcommunity.microsoft.com/t5/sql-server-support/memory-grants-meditation-the-mysterious-sql-server-memory/ba-p/333994)

## <a name="locking"></a>Kilitleme

### <a name="what-is-happening"></a>Ne oluyor

Bu performans deseni, son yedi günlük performans taban çizgisiyle karşılaştırıldığında aşırı veritabanı kilitlemesinin algılandığı geçerli veritabanı performansındaki bozulmayı gösterir.

Modern RDBMS'de kilitleme, mümkün olan birden fazla eşzamanlı çalışan ve paralel veritabanı işlemi çalıştırılarak performansın en üst düzeye çıkarıldığı çok iş parçacıklı sistemlerin uygulanması için gereklidir. Bu bağlamda kilitleme, yalnızca tek bir işlemin yalnızca gerekli olan satırlara, sayfalara, tablolara ve dosyalara erişebildiği ve kaynaklar için başka bir işlemle rekabet edilemeyen yerleşik erişim mekanizmasını ifade eder. Kullanım için kaynakları kilitleyen işlem bunlarla yapıldığında, bu kaynakların kilidi serbest bırakılır ve bu da diğer işlemlerin gerekli kaynaklara erişmesine olanak tanır. Kilitleme hakkında daha fazla bilgi için [veritabanı altyapısında Kilit'e](https://msdn.microsoft.com/library/ms190615.aspx)bakın.

SQL altyapısı tarafından yürütülen hareketler, kullanılmak üzere kilitlenen kaynaklara erişmek için uzun süreler bekliyorsa, bu bekleme süresi iş yükü yürütme performansının yavaşlamasına neden olur.

### <a name="troubleshooting"></a>Sorun giderme

Tanılama günlüğü, sorun giderme için temel olarak kullanabileceğiniz kilitleme ayrıntılarını kilitleme çıktıları. Bildirilen engelleme sorgularını, diğer bir şekilde kilitleme performansı bozulmasını sağlayan sorguları çözümleyebilir ve bunları kaldırabilirsiniz. Bazı durumlarda, engelleme sorgularını en iyi duruma getirerek başarılı olabilirsiniz.

Sorunu azaltmanın en basit ve en güvenli yolu, işlemleri kısa tutmak ve en pahalı sorguların kilit ayak izini azaltmaktır. Büyük bir işlem grubu daha küçük işlemlere ayırabilirsiniz. İyi bir uygulama, sorguyu olabildiğince verimli hale getirerek sorgu kilidi ayak izini azaltmaktır. Kilitlenme olasılığını artırdıkları ve genel veritabanı performansını olumsuz etkiledikleri için büyük taramaları azaltın. Kilitlenmeye neden olan tanımlanmış sorgular için, tablo taramalarından kaçınmak için yeni dizinler oluşturabilir veya varolan dizinlere sütun ekleyebilirsiniz.

Daha fazla öneri için, [SQL Server'da kilit yükseltmenin neden olduğu engelleme sorunlarını nasıl çözeceğiniz](https://support.microsoft.com/help/323630/how-to-resolve-blocking-problems-that-are-caused-by-lock-escalation-in)ebakını görün.

## <a name="increased-maxdop"></a>Artan MAXDOP

### <a name="what-is-happening"></a>Ne oluyor

Bu algılanabilir performans deseni, seçilen sorgu yürütme planının olması gerekenden daha fazla paralelleştirildiği durumu gösterir. SQL Veritabanı sorgu optimize edici, olası durumlarda işleri hızlandırmak için paralel olarak sorguları yürüterek iş yükü performansını artırabilir. Bazı durumlarda, sorguyu işleyen paralel çalışanlar, aynı sorguyu daha az paralel işçiyle yürütmeye kıyasla, hatta bazı durumlarda tek bir çalışan iş parçacığıyla karşılaştırıldığında sonuçları eşitlemek ve birleştirmek için birbirlerini beklemek için daha fazla zaman harcarlar.

Uzman sistem, temel döneme göre geçerli veritabanı performansını analiz eder. Sorgu yürütme planı olması gerekenden daha paralel olduğundan, daha önce çalışan bir sorgunun öncekinden daha yavaş çalıştığını belirler.

SQL Veritabanı'ndaki MAXDOP sunucu yapılandırma seçeneği, aynı sorguyu paralel olarak yürütmek için kaç CPU çekirdeğinin kullanılabileceğini denetlemek için kullanılır.

### <a name="troubleshooting"></a>Sorun giderme

Tanılama günlüğü çıktıları, olması gerekenden daha fazla paralelleştirildikleri için yürütme süresinin arttığı sorgularla ilgili sorguları sorgular. Günlük ayrıca CXP bekleme sürelerini de çıkar. Bu süre, tek bir düzenleyici/koordinatör iş parçacığının (iş parçacığı 0) sonuçları birleştirmeden ve ilerlemeden önce diğer tüm iş parçacıklarının tamamlanmasını beklediği zamanı temsil eder. Ayrıca, tanılama günlüğü, düşük performans gösteren sorguların genel olarak yürütmede beklediği bekleme sürelerini kaydeder. Bu bilgileri sorun giderme için temel olarak kullanabilirsiniz.

İlk olarak, karmaşık sorguları en iyi duruma getirin veya basitleştirin. İyi uygulama küçük olanlar içine uzun toplu işleri kırmak tır. Ayrıca, sorgularınızı desteklemek için dizinler oluşturduğunuzdan emin olun. Kötü performans olarak işaretlenmiş bir sorgu için en yüksek paralellik derecesini (MAXDOP) el ile de uygulayabilirsiniz. Bu işlemi T-SQL kullanarak yapılandırmak için [bkz.](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option)

Varsayılan değer olarak MAXDOP sunucu yapılandırma seçeneğini sıfıra (0) ayarlamak, SQL Veritabanı'nın tek bir sorguyu yürütmek için iş parçacıklarını paralelleştirmek için kullanılabilir tüm mantıksal CPU çekirdeklerini kullanabileceğini gösterir. MAXDOP'u bir (1) olarak ayarlamak, tek bir sorgu yürütmesi için yalnızca bir çekirdeğin kullanılabileceğini gösterir. Pratik açıdan, bu paralellik kapalı olduğu anlamına gelir. Vaka başına bazda, veritabanında kullanılabilir çekirdeklere ve tanılama günlük bilgilerine bağlı olarak, MAXDOP seçeneğini, sizin durumunuzdaki sorunu çözebilecek paralel sorgu yürütmesi için kullanılan çekirdek sayısına ayarlayabilirsiniz.

## <a name="pagelatch-contention"></a>Pagelatch Çekişme

### <a name="what-is-happening"></a>Ne oluyor

Bu performans deseni, son yedi günlük iş yükü taban çizgisiyle karşılaştırıldığında pagelatch çekişmesi nedeniyle geçerli veritabanı iş yükü performansı bozulmasını gösterir.

Mandallar, SQL Database tarafından çoklu iş parçacığı etkinleştirmek için kullanılan hafif senkronizasyon mekanizmalarıdır. Endeksler, veri sayfaları ve diğer iç yapıları içeren bellek içi yapıların tutarlılığını garanti eder.

SQL veritabanında birçok mandal türü vardır. Basitlik amacıyla, arabellek mandalları arabellek havuzundaki bellek içi sayfaları korumak için kullanılır. IO mandalları, henüz arabellek havuzuna yüklenmemiş sayfaları korumak için kullanılır. Arabellek havuzundaki bir sayfaya veriler yazıldığında veya okunduğunda, bir alt iş parçacığının önce sayfa için bir arabellek mandalı alması gerekir. Bir alt iş parçacığı, bellek içi arabellek havuzunda zaten bulunmayan bir sayfaya erişmeye çalıştığında, depolama alanından gerekli bilgileri yüklemek için bir IO isteği yapılır. Bu olaylar dizisi daha ciddi bir performans bozulması biçimini gösterir.

Sayfa mandalları üzerinde çekişme aynı anda aynı bellek yapısı üzerinde mandal elde etmek için çalışırken oluşur, hangi sorgu yürütme için artırılmış bir bekleme süresi tanıtır. Pagelatch IO çekişmesi söz konusu olduğunda, verilere depolamadan erişilmesi gerektiğinde, bu bekleme süresi daha da büyüktür. İş yükü performansını önemli ölçüde etkileyebilir. Pagelatch çekişmesi, birbirlerinden bekleyen ve birden çok CPU sistemindeki kaynaklar için rekabet eden iş parçacıklarının en yaygın senaryosudur.

### <a name="troubleshooting"></a>Sorun giderme

Tanılama günlüğü çıkışları pagelatch çekişme ayrıntıları. Bu bilgileri sorun giderme için temel olarak kullanabilirsiniz.

Pagelatch, SQL Veritabanı'nın bir iç denetim mekanizması olduğundan, bunları otomatik olarak ne zaman kullanacağını belirler. Şema tasarımı da dahil olmak üzere uygulama kararları, mandalların deterministik davranışı nedeniyle pagelatch davranışını etkileyebilir.

Mandal çekişmesini işlemek için kullanılan yöntemlerden biri, dizi uçlarını bir dizin aralığında eşit olarak dağıtmak için sıralı bir dizin anahtarını sıralı olmayan bir anahtarla değiştirmektir. Genellikle, dizindeki bir satır başsütunu iş yükünü orantılı olarak dağıtır. Göz önünde bulundurulması gereken başka bir yöntem tablo bölümleme olduğunu. Bölümlenmiş bir tabloda hesaplanmış bir sütun içeren karma bölümleme düzeni oluşturmak, aşırı mandal çekişmelerini hafifletmek için yaygın bir yaklaşımdır. Pagelatch IO çekişmesi söz konusu olduğunda, dizinlerin tanıtılması bu performans sorununu azaltmaya yardımcı olur.

Daha fazla bilgi için SQL [Server'da (PDF indir) mandal çekişmesini tanıla ve çözümle mesuliyetine](https://download.microsoft.com/download/B/9/E/B9EDF2CD-1DBF-4954-B81E-82522880A2DC/SQLServerLatchContention.pdf) bakın.

## <a name="missing-index"></a>Eksik Dizini

### <a name="what-is-happening"></a>Ne oluyor

Bu performans deseni, eksik bir dizin nedeniyle son yedi günlük taban çizgisiyle karşılaştırıldığında geçerli veritabanı iş yükü performansı bozulmasını gösterir.

Sorguların performansını hızlandırmak için bir dizin kullanılır. Ziyaret edilmesi veya taranması gereken veri kümesi sayfalarının sayısını azaltarak tablo verilerine hızlı erişim sağlar.

Performans düşüşüne neden olan belirli sorgular, dizin oluşturmanın performans için yararlı olacağı bu algılama aracılığıyla tanımlanır.

### <a name="troubleshooting"></a>Sorun giderme

Tanılama günlüğü, iş yükü performansını etkilemek üzere tanımlanan sorgular için sorguları sorguları sorguları. Bu sorgular için dizinler oluşturabilirsiniz. Ayrıca, gerekli değilse bu sorguları en iyi duruma getirebilir veya kaldırabilirsiniz. İyi bir performans uygulaması, kullanmadığınız verileri sorgulamaktan kaçınmaktır.

> [!TIP]
> SQL Veritabanı yerleşik zekasının veritabanlarınız için en iyi performans gösteren dizinleri otomatik olarak yönetebileceğini biliyor muydunuz?
>
> SQL Veritabanı'nın sürekli performans optimizasyonu için [SQL Veritabanı otomatik ayarını](sql-database-automatic-tuning.md)etkinleştirmenizi öneririz. SQL Veritabanı yerleşik zekanın bu benzersiz özelliği, SQL veritabanınızı sürekli olarak izler ve veritabanlarınız için otomatik olarak ayarve dizinler oluşturur.
>

## <a name="new-query"></a>Yeni Sorgu

### <a name="what-is-happening"></a>Ne oluyor

Bu performans deseni, yedi günlük performans taban çizgisiyle karşılaştırıldığında kötü performans gösteren ve iş yükü performansını etkileyen yeni bir sorgu algılanır.

İyi performanslı bir sorgu yazmak bazen zor bir görev olabilir. Sorgu yazma hakkında daha fazla bilgi için SQL [sorguları yazma'ya](https://msdn.microsoft.com/library/bb264565.aspx)bakın. Varolan sorgu performansını en iyi duruma getirmek için [sorgu atoması'na](https://msdn.microsoft.com/library/ms176005.aspx)bakın.

### <a name="troubleshooting"></a>Sorun giderme

Tanılama günlüğü, sorgu hashes de dahil olmak üzere en çok CPU tüketen iki sorguya kadar bilgi çıktıları. Algılanan sorgu iş yükü performansını etkilediğiiçin, sorgunuzu en iyi duruma getirebilirsiniz. İyi uygulama yalnızca kullanmanız gereken verileri almaktır. WHERE yan tümcesi içeren sorguları da kullanmanızı öneririz. Ayrıca, karmaşık sorguları basitleştirmenizi ve bunları daha küçük sorgulara ayırmanızı öneririz. Başka bir iyi uygulama daha küçük toplu sorguları içine büyük toplu sorguları yıkmak tır. Yeni sorgular için dizinler tanıtılması genellikle bu performans sorununu azaltmak için iyi bir uygulamadır.

Azure [SQL Veritabanı Sorgu Performans Öngörüsünyü](sql-database-query-performance.md)kullanmayı düşünün.

## <a name="increased-wait-statistic"></a>Artırılmış Bekleme İstatistikleri

### <a name="what-is-happening"></a>Ne oluyor

Bu algılanabilir performans deseni, son yedi günlük iş yükü taban çizgisiyle karşılaştırıldığında düşük performanslı sorguların tanımlandığı bir iş yükü performansı bozulmasını gösterir.

Bu durumda, sistem düşük performansgösteren sorguları başka bir standart algılanabilir performans kategorisi altında sınıflandıramaz, ancak gerilemeden sorumlu bekleme istatistiğini algılamıştır. Bu nedenle, bunları, gerilemeden sorumlu bekleme istatistiğinin de açığa *çıktığı, artırılmış bekleme istatistiklerine*sahip sorgular olarak kabul edilir.

### <a name="troubleshooting"></a>Sorun giderme

Tanılama günlüğü, artan bekleme süresi ayrıntıları ve etkilenen sorguların sorgu ları hakkında bilgi girişi sağlar.

Sistem yetersiz performans gösteren sorguların temel nedenini başarıyla tanımlayamadığından, tanılama bilgileri el ile sorun giderme için iyi bir başlangıç noktasıdır. Bu sorguların performansını en iyi duruma getirebilirsiniz. İyi bir uygulama, yalnızca kullanmanız gereken verileri almak ve karmaşık sorguları basitleştirmek ve küçük sorgulara ayırmaktır.

Sorgu performansını en iyi duruma getirilmesi hakkında daha fazla bilgi için [sorgu atonlama'ya](https://msdn.microsoft.com/library/ms176005.aspx)bakın.

## <a name="tempdb-contention"></a>TempDB Çekişme

### <a name="what-is-happening"></a>Ne oluyor

Bu algılanabilir performans deseni, tempDB kaynaklarına erişmeye çalışan iş parçacıklarının bir darboğazının bulunduğu bir veritabanı performans durumunu gösterir. (Bu durum IO ile ilgili değildir.) Bu performans sorunu için tipik senaryo, tüm oluşturmak, kullanmak ve sonra küçük tempDB tabloları bırakın eşzamanlı sorguları yüzlerce. Sistem, aynı tempDB tablolarını kullanan eşzamanlı sorgu sayısının, veritabanı performansını etkilemek için son yedi günlük performans taban çizgisine göre yeterli istatistiksel anlamlılıkla arttığını algılamıştır.

### <a name="troubleshooting"></a>Sorun giderme

Tanılama günlüğü çıkışları tempDB çekişme ayrıntıları. Bilgileri sorun giderme için başlangıç noktası olarak kullanabilirsiniz. Bu tür bir çekişmeyi hafifletmek ve genel iş yükünün iş yükünü artırmak için takip edebilirsiniz iki şey vardır: Geçici tabloları kullanmayı durdurabilirsiniz. Bellek için optimize edilmiş tabloları da kullanabilirsiniz.

Daha fazla bilgi için bellek [için optimize edilmiş tablolara Giriş'e](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/introduction-to-memory-optimized-tables)bakın.

## <a name="elastic-pool-dtu-shortage"></a>Elastik havuz DTU sıkıntısı

### <a name="what-is-happening"></a>Ne oluyor

Bu algılanabilir performans deseni, son yedi günlük taban çizgisiyle karşılaştırıldığında geçerli veritabanı iş yükü performansında bir bozulma olduğunu gösterir. Aboneliğinizin esnek havuzunda mevcut DT'lerin yetersizliğinden kaynaklanmaktadır.

SQL Veritabanı'ndaki kaynaklar genellikle CPU ve IO (veri ve işlem günlüğü IO) kaynaklarının karma ölçüsünden oluşan [DTU kaynakları](sql-database-purchase-models.md#dtu-based-purchasing-model)olarak adlandırılır. [Azure elastik havuz kaynakları,](sql-database-elastic-pool.md) ölçekleme amacıyla birden çok veritabanları arasında paylaşılan kullanılabilir eDTU kaynakları havuzu olarak kullanılır. Elastik havuzunuzdaki kullanılabilir elastik elastik edtü kaynakları havuzdaki tüm veritabanlarını destekleyecek kadar büyük değilse, sistem tarafından elastik havuz DTU sıkıntısı performansı sorunu algılanır.

### <a name="troubleshooting"></a>Sorun giderme

Tanılama, elastik havuzdaki bilgileri kaydeder, en çok DTU tüketen veritabanlarını listeler ve en çok tüketen veritabanı tarafından kullanılan havuzun DTU'su'nun bir yüzdesini sağlar.

Bu performans durumu, elastik havuzdaki aynı eDT'ler havuzunu kullanan birden çok veritabanlarıyla ilişkili olduğundan, sorun giderme adımları en üstteki DTU tüketen veritabanlarına odaklanır. En çok tüketen veritabanlarındaki iş yükünü azaltabilirsiniz, bu veritabanlarında en çok tüketen sorguların optimizasyonu da dahil. Ayrıca, kullanmadığınız verileri sorgulamadığınızdan da emin olabilirsiniz. Başka bir yaklaşım, en iyi DTU tüketen veritabanlarını kullanarak uygulamaları en iyi duruma getirmek ve iş yükünü birden çok veritabanları arasında yeniden dağıtmaktır.

En iyi DTU tüketen veritabanlarınızdaki geçerli iş yükünün azaltılması ve optimizasyonu mümkün değilse, elastik havuz fiyatlandırma katmanınızı artırmayı düşünün. Bu artış, elastik havuzda mevcut DT'lerin artmasına neden olur.

## <a name="plan-regression"></a>Regresyon Planı

### <a name="what-is-happening"></a>Ne oluyor

Bu algılanabilir performans deseni, SQL Veritabanı'nın bir alt optimal sorgu yürütme planı kullandığı bir durumu gösterir. Suboptimal planı genellikle geçerli ve diğer sorgular için daha uzun bekleme süreleri neden artan sorgu yürütme neden olur.

SQL veritabanı, sorgu yürütmesi için en az maliyetle sorgu yürütme planını belirler. Sorguların ve iş yüklerinin türü değiştikçe, bazen varolan planlar artık verimli değildir veya belki de SQL Veritabanı iyi bir değerlendirme yapmamaktadır. Düzeltme meselesi olarak, sorgu yürütme planları el ile zorlanabilir.

Bu algılanabilir performans deseni üç farklı plan regresyonu örneğini birleştirir: yeni plan regresyonu, eski plan regresyon ve varolan planlar iş yükünü değiştirdi. Meydana gelen plan regresyon belirli türü tanılama günlüğünde *ayrıntılar* özelliği sağlanır.

Yeni plan regresyon koşulu, SQL Veritabanı'nın eski plan kadar verimli olmayan yeni bir sorgu yürütme planı yürütmeye başladığı durumu ifade eder. Eski plan regresyon koşulu, SQL Veritabanı'nın yeni ve daha verimli bir plan kullanmaktan yeni plan kadar verimli olmayan eski plana geçiş yapma durumuna işaret eder. Mevcut planlar değiştirilen iş yükü regresyon, eski ve yeni planların sürekli olarak dönüşümlü olarak değiştiği ve dengenin daha çok düşük performanslı plana doğru yöneldiği durumu ifade eder.

Plan gerilemeleri hakkında daha fazla bilgi için SQL [Server'da plan regresyon nedir?](https://blogs.msdn.microsoft.com/sqlserverstorageengine/20../../what-is-plan-regression-in-sql-server/)

### <a name="troubleshooting"></a>Sorun giderme

Tanılama günlüğü sorgu kalıpları, iyi plan kimliği, kötü plan kimliği ve sorgu kimlikleri çıktıları. Bu bilgileri sorun giderme için temel olarak kullanabilirsiniz.

Sağlanan sorgu varslarıyla tanımlayabileceğiniz özel sorgularınız için hangi planın daha iyi performans gösterdiğini çözümleyebilirsiniz. Sorgularınız için hangi planın daha iyi çalıştığını belirledikten sonra, bunu el ile zorlayabilirsiniz.

Daha fazla bilgi için bkz. [SQL Server plan gerilemelerini nasıl önlediğini öğrenin.](https://blogs.msdn.microsoft.com/sqlserverstorageengine/20../../you-shall-not-regress-how-sql-server-2017-prevents-plan-regressions/)

> [!TIP]
> SQL Veritabanı yerleşik zekasının veritabanlarınız için en iyi performans gösteren sorgu yürütme planlarını otomatik olarak yönetebileceğini biliyor muydunuz?
>
> SQL Veritabanı'nın sürekli performans optimizasyonu için [SQL Veritabanı otomatik ayarını](sql-database-automatic-tuning.md)etkinleştirmenizi öneririz. SQL Veritabanı yerleşik zekanın bu benzersiz özelliği, SQL veritabanınızı sürekli olarak izler ve veritabanlarınız için en iyi performans gösteren sorgu yürütme planlarını otomatik olarak izler ve oluşturur.
>

## <a name="database-scoped-configuration-value-change"></a>Veritabanı Kapsamı Değer Değişikliği

### <a name="what-is-happening"></a>Ne oluyor

Bu algılanabilir performans deseni, veritabanı kapsamı yla yapılan yapılandırmadaki bir değişikliğin, son yedi günlük veritabanı iş yükü davranışıyla karşılaştırıldığında algılanan performans gerilemesine neden olduğu bir durumu gösterir. Bu desen, veritabanı kapsamı yla yapılan son değişikliğin veritabanı performansınız için yararlı görünmediğini gösterir.

Veritabanı kapsamı na göre yapılandırma değişiklikleri her bir veritabanı için ayarlanabilir. Bu yapılandırma, veritabanınızın bireysel performansını en iyi duruma getirmek için ayrı ayrı kullanılır. Aşağıdaki seçenekler her bir veritabanı için yapılandırılabilir: MAXDOP, LEGACY_CARDINALITY_ESTIMATION, PARAMETER_SNIFFING, QUERY_OPTIMIZER_HOTFIXES ve CLEAR PROCEDURE_CACHE.

### <a name="troubleshooting"></a>Sorun giderme

Tanılama günlüğü, önceki yedi günlük iş yükü davranışıyla karşılaştırıldığında performans düşüşüne neden olan son zamanlarda yapılan veritabanı kapsamıyla yapılandırma değişiklikleri. Yapılandırma değişikliklerini önceki değerlere geri dönebilirsiniz. İstenilen performans düzeyine ulaşılıncaya kadar değeri değere göre ayarlayabilirsiniz. Veritabanı kapsamı yapılandırma değerlerini benzer bir veritabanından tatmin edici bir performansla kopyalayabilirsiniz. Performansı sorun gideremiyorsanız, varsayılan SQL Veritabanı varsayılan değerlerine geri dön ve bu taban çizgisinden başlayarak ince ayar yapmaya çalış.

Yapılandırmayı değiştirme konusunda veritabanı kapsamı yapılandırmasını ve T-SQL sözdizimini en iyi duruma alma hakkında daha fazla bilgi için veritabanı [kapsamı yla ilgili yapılandırmayı değiştir (Transact-SQL)](https://msdn.microsoft.com/library/mt629158.aspx)bakın.

## <a name="slow-client"></a>Yavaş İstemci

### <a name="what-is-happening"></a>Ne oluyor

Bu algılanabilir performans deseni, SQL veritabanını kullanan istemcinin sonuçları gönderdiği kadar hızlı bir şekilde veritabanından çıktı yıkamayacağı bir durumu gösterir. SQL Veritabanı, yürütülen sorguların sonuçlarını arabellekte depolamadığından, yavaşlar ve istemcinin iletilen sorgu çıktılarını devam etmeden önce tüketmesini bekler. Bu durum, SQL veritabanından gelen çıktıları tüketen istemciye aktaracak kadar hızlı olmayan bir ağla da ilgili olabilir.

Bu durum, yalnızca son yedi günlük veritabanı iş yükü davranışıyla karşılaştırıldığında bir performans regresyonu algılanırsa oluşturulur. Bu performans sorunu yalnızca önceki performans davranışıyla karşılaştırıldığında istatistiksel olarak anlamlı bir performans bozulması oluşursa algılanır.

### <a name="troubleshooting"></a>Sorun giderme

Bu algılanabilir performans deseni istemci tarafı durumunu gösterir. İstemci tarafındaki uygulamada veya istemci tarafındaki ağda sorun giderme gereklidir. Tanılama günlüğü, sorgu işlerini ve istemcinin son iki saat içinde bunları tüketmesini en çok bekleyen bekleme sürelerini çıktırıyor. Bu bilgileri sorun giderme için temel olarak kullanabilirsiniz.

Bu sorguların tüketimi için uygulamanızın performansını optimize edebilirsiniz. Ayrıca olası ağ gecikmesi sorunlarını da göz önünde bulundurabilirsiniz. Performans bozulması sorunu son yedi günlük performans taban çizgisindeki değişikliğe dayandığından, son uygulama veya ağ koşulu değişikliklerinin bu performans regresyon olayına neden olup olmadığını araştırabilirsiniz.

## <a name="pricing-tier-downgrade"></a>Fiyatlandırma Tier Downgrade

### <a name="what-is-happening"></a>Ne oluyor

Bu algılanabilir performans deseni, SQL Veritabanı aboneliğinizin fiyatlandırma katmanının düşürüldüğü bir durumu gösterir. Veritabanında kullanılabilen kaynakların (DT'ler) azaltılması nedeniyle, sistem son yedi günlük taban çizgisine kıyasla geçerli veritabanı performansında bir düşüş algılamıştır.

Buna ek olarak, SQL Veritabanı aboneliğinizin fiyatlandırma katmanının kısa bir süre içinde düşürülüp daha yüksek bir katmana yükseltildiği bir durum da olabilir. Bu geçici performans bozulmasının algılanması, bir fiyatlandırma katmanı düşürme ve yükseltme olarak tanılama günlüğünün ayrıntılar bölümünde çıktı.

### <a name="troubleshooting"></a>Sorun giderme

Fiyatlandırma katmanınızı azalttıysanız ve bu nedenle SQL Veritabanı'nda kullanılabilen DT'ler ve performanstan memnunsanız, yapmanız gereken hiçbir şey yoktur. Fiyatlandırma katmanınızı azalttıysanız ve SQL veritabanı performansınızdan memnun değilseniz, veritabanı iş yüklerinizi azaltın veya fiyatlandırma katmanını daha yüksek bir düzeye artırmayı düşünün.

## <a name="recommended-troubleshooting-flow"></a>Önerilen sorun giderme akışı

 Intelligent Insights kullanarak performans sorunlarını gidermek için önerilen bir yaklaşım için akış şemasını izleyin.

Azure SQL Analytics'e giderek Azure portalı üzerinden Akıllı Öngörülere erişin. Gelen performans uyarısını bulmaya çalış ve onu seç. Algılamalar sayfasında neler olduğunu belirleyin. Sorunun sağlanan kök neden çözümlemesi, sorgu metni, sorgu zaman eğilimleri ve olay evrimi gözönünde bulun. Performans sorununu hafifletmek için Intelligent Insights önerisini kullanarak sorunu çözmeye çalış.

[![Sorun giderme akış şeması](./media/sql-database-intelligent-insights/intelligent-insights-troubleshooting-flowchart.png)](https://github.com/Microsoft/sql-server-samples/blob/master/samples/features/intelligent-insight/Troubleshoot%20Azure%20SQL%20Database%20performance%20issues%20using%20Intelligent%20Insight.pdf)

> [!TIP]
> PDF sürümünü indirmek için akış şemasını seçin.

Intelligent Insights'ın performans sorununun temel neden çözümlemesi için genellikle bir saat zamana ihtiyacı vardır. Sorununuzu Intelligent Insights'ta bulamıyorsanız ve bu sizin için çok önemliyse, performans sorununun temel nedenini el ile belirlemek için Sorgu Mağazası'nı kullanın. (Genellikle, bu sorunlar bir saatten daha eskidir.) Daha fazla bilgi için [Sorgu Deposu'nu kullanarak performansı izleyin'e](https://docs.microsoft.com/sql/relational-databases/performance/monitoring-performance-by-using-the-query-store)bakın.

## <a name="next-steps"></a>Sonraki adımlar

- [Intelligent Insights](sql-database-intelligent-insights.md) kavramlarını öğrenin.
- Intelligent [Insights Azure SQL Veritabanı performans tanılama günlüğünü](sql-database-intelligent-insights-use-diagnostics-log.md)kullanın.
- [Azure SQL Analytics'i kullanarak Azure SQL Veritabanını](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-sql)izleyin.
- Azure [kaynaklarından günlük verileri toplamayı ve tüketmeyi](../azure-monitor/platform/platform-logs-overview.md)öğrenin.
