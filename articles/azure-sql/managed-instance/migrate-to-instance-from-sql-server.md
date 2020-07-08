---
title: SQL Server 'den Azure SQL yönetilen örneği 'ne geçiş
description: SQL Server bir veritabanını Azure SQL yönetilen örneği 'ne geçirmeyi öğrenin.
services: sql-database
ms.service: sql-managed-instance
ms.subservice: migration
ms.custom: seo-lt-2019, sqldbrb=1
ms.devlang: ''
ms.topic: conceptual
author: bonova
ms.author: bonova
ms.reviewer: douglas, carlrab
ms.date: 07/11/2019
ms.openlocfilehash: 47f766e8e5c1ea43d0c9bde8138c0ab17533f539
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84708697"
---
# <a name="sql-server-instance-migration-to-azure-sql-managed-instance"></a>Azure SQL yönetilen örneğine örnek geçişi SQL Server
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

Bu makalede, SQL Server 2005 veya sonraki bir sürüm örneğini [Azure SQL yönetilen örneği](sql-managed-instance-paas-overview.md)'ne geçirmeye yönelik yöntemler hakkında bilgi edineceksiniz. Tek bir veritabanına veya elastik havuza geçiş hakkında daha fazla bilgi için bkz. [tek veya havuza alınmış bir veritabanına geçme](../database/migrate-to-database-from-sql-server.md). Diğer platformlardan geçiş hakkında geçiş bilgileri için bkz. [Azure veritabanı geçiş kılavuzu](https://datamigration.microsoft.com/).

> [!NOTE]
> Azure SQL yönetilen örneğini hızla başlatıp denemek istiyorsanız, Bu sayfa yerine [hızlı başlangıç kılavuzuna](quickstart-content-reference-guide.md) gitmeniz gerekebilir.

Yüksek düzeyde, veritabanı geçiş işlemi şöyle görünür:

![Geçiş süreci](./media/migrate-to-instance-from-sql-server/migration-process.png)

- Geçişlerinizi engelleyebilen engelleyici bir sorun olmadığından emin olmanız gereken [SQL yönetilen örnek uyumluluğunu değerlendirin](#assess-sql-managed-instance-compatibility) .
  
  Bu adım Ayrıca, kaynak SQL Server Örneğiniz üzerinde kaynak kullanımını belirlemede bir [performans temeli](#create-a-performance-baseline) oluşturulmasını da içerir. Düzgün şekilde boyutlandırılmış bir yönetilen örnek dağıtmak ve geçişten sonra performansın etkilenmemesi durumunda bu adım gereklidir.
- [Uygulama bağlantısı seçeneklerini belirleyin](connect-application-instance.md).
- Yönetilen örneğinizin teknik özelliklerini (sanal çekirdek sayısı, bellek miktarı) ve performans katmanını (İş Açısından Kritik Genel Amaçlı) seçeceğiniz, en uygun [Boyut yönetimli bir örneğe dağıtın](#deploy-to-an-optimally-sized-managed-instance) .
- [Geçiş yöntemi ' ni seçin ve](#select-a-migration-method-and-migrate) çevrimdışı geçiş (yerel yedekleme/geri yükleme, veritabanı içeri/dışarı aktarma) veya çevrimiçi geçiş (Azure veri geçiş hizmeti, işlemsel çoğaltma) kullanarak veritabanlarınızı geçireceğiniz yere geçiş yapın.
- Performansın beklendiğinden emin olmak için [uygulamaları izleyin](#monitor-applications) .

> [!NOTE]
> Tek bir veritabanını tek bir veritabanına veya elastik havuza geçirmek için, bkz. [SQL Server veritabanını Azure SQL veritabanı 'Na geçirme](../database/migrate-to-database-from-sql-server.md).

## <a name="assess-sql-managed-instance-compatibility"></a>SQL yönetilen örnek uyumluluğunu değerlendir

İlk olarak, SQL yönetilen örneğinin uygulamanızın veritabanı gereksinimleriyle uyumlu olup olmadığını saptayın. SQL yönetilen örneği, SQL Server kullanan mevcut uygulamaların çoğunluğu için kolay yükseltme ve kaydırma geçişi sağlamak üzere tasarlanmıştır. Ancak bazen henüz desteklenmeyen özellikler veya yetenekler gerektirebilir, geçici çözüm uygulama maliyeti de çok yüksektir.

Azure SQL veritabanında veritabanı işlevselliğini etkileyen olası uyumluluk sorunlarını algılamak için [Data Migration Yardımcısı](https://docs.microsoft.com/sql/dma/dma-overview) kullanın. Bazı bildirilmiş engelleme sorunları varsa, [Azure VM 'de SQL Server](https://azure.microsoft.com/services/virtual-machines/sql-server/)gibi alternatif bir seçeneği göz önünde bulundurmanız gerekebilir. Aşağıda bazı örnekler verilmiştir:

- İşletim sistemine veya dosya sistemine doğrudan erişim istiyorsanız, örneğin, SQL Server ile aynı sanal makineye üçüncü taraf veya özel aracılar yüklemek için.
- FILESTREAM/FileTable, PolyBase ve platformlar arası işlemler gibi hala desteklenmeyen özelliklerde kesin bağımlılığı varsa.
- Kesinlikle SQL Server (örneğin, 2012) belirli bir sürümünde kalmanıza gerek duyuyorsanız.
- İşlem gereksinimleriniz yönetilen örnek tekliflerinden çok daha düşükse (örneğin, bir sanal çekirdek) ve veritabanı birleştirme kabul edilebilir bir seçenek değildir.

Tüm belirlenen geçiş engelleyicilerini çözümledikten ve SQL yönetilen örneğine geçişe devam ediyorsanız, bazı değişikliklerin iş yükünüzün performansını etkileyebileceğini unutmayın:

- Zorunlu tam kurtarma modeli ve düzenli otomatik yedekleme zamanlaması, düzenli aralıklarla basit/toplu olarak günlüğe kaydedilmiş bir model kullandıysanız veya yedeklemeleri istek üzerine durdurduktan sonra iş yükünüzün veya bakım/ETL eylemlerinin performansını etkileyebilir.
- İzleme bayrakları veya uyumluluk düzeyleri gibi farklı sunucu veya veritabanı düzeyi yapılandırma.
- Saydam veritabanı şifreleme (TDE) veya otomatik yük devretme grupları gibi kullandığınız yeni özellikler CPU ve GÇ kullanımını etkileyebilir.

SQL yönetilen örneği kritik senaryolarda bile% 99,99 kullanılabilirlik sağlar, bu nedenle bu özelliklerden kaynaklanan yükün devre dışı bırakılamaz. Daha fazla bilgi için, [SQL Server ve Azure SQL yönetilen örneği üzerinde farklı performansa neden olabilecek kök nedenleri](https://azure.microsoft.com/blog/key-causes-of-performance-differences-between-sql-managed-instance-and-sql-server/)bölümüne bakın.

### <a name="create-a-performance-baseline"></a>Performans temeli oluşturma

Yönetilen bir örnekteki iş yükünüzün performansını SQL Server üzerinde çalışan özgün iş yükünüze göre karşılaştırmak istiyorsanız, karşılaştırma için kullanılacak bir performans temeli oluşturmanız gerekir.

Performans temeli, ortalama/en fazla CPU kullanımı, ortalama/en fazla disk GÇ gecikmesi, üretilen iş, ıOPS, ortalama/en fazla sayfa ömrü erkeklerin ve tempdb 'nin ortalama en büyük boyutu gibi bir dizi parametre kümesidir. Geçişten sonra benzer veya hatta daha iyi parametrelere sahip olmak istiyorsunuz. bu nedenle, bu parametrelerin taban çizgisi değerlerini ölçmek ve kaydetmek önemlidir. Sistem parametrelerine ek olarak, iş yükünüzün temsili sorguları veya en önemli sorguları bir kümesini seçmeniz ve seçili sorgular için min/Ortalama/maksimum süreyi ve CPU kullanımını ölçmenize gerek duyarsınız. Bu değerler, yönetilen örnek üzerinde çalışan iş yükünün performansını, kaynak SQL Server örneğinizin orijinal değerlerine göre karşılaştırmanızı sağlar.

SQL Server Örneğinizde ölçmeye ihtiyacınız olan parametrelerden bazıları şunlardır:

- [SQL Server Örneğiniz ÜZERINDEKI CPU kullanımını izleyin](https://techcommunity.microsoft.com/t5/Azure-SQL-Database/Monitor-CPU-usage-on-SQL-Server/ba-p/680777#M131) ve ortalama ve en yoğun CPU kullanımını kaydedin.
- [SQL Server Örneğiniz üzerinde bellek kullanımını izleyin](https://docs.microsoft.com/sql/relational-databases/performance-monitor/monitor-memory-usage) ve arabellek havuzu, plan önbelleği, sütun deposu havuzu, [bellek içi OLTP](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/monitor-and-troubleshoot-memory-usage?view=sql-server-2017)vb. gibi farklı bileşenler tarafından kullanılan bellek miktarını saptayın. Ayrıca, sayfa ömrü erkeklerin bellek performans sayacının ortalama ve en yüksek değerlerini de bulmanız gerekir.
- [Sys. dm_io_virtual_file_stats](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-io-virtual-file-stats-transact-sql) View veya [Performance sayaçlarını](https://docs.microsoft.com/sql/relational-databases/performance-monitor/monitor-disk-usage)kullanarak kaynak SQL Server örneğinde disk GÇ kullanımını izleyin.
- Bir SQL Server 2016 + sürümünden geçiş yapıyorsanız dinamik yönetim görünümlerini veya sorgu deposunu inceleyerek iş yükünü ve sorgu performansını veya SQL Server örneğinizi izleyin. Yönetilen örnek üzerinde çalışan sorgularla karşılaştırmak için iş yükünüzün en önemli sorgularının ortalama süresini ve CPU kullanımını belirler.

> [!Note]
> Yüksek CPU kullanımı, sabit bellek baskısı veya tempdb ya da Parametreleştirme sorunları gibi SQL Server iş yükünüz ile ilgili herhangi bir sorun fark ederseniz, taban çizgisini ve geçişi gerçekleştirmeden önce bunları kaynak SQL Server Örneğinizde çözmeyi denemeniz gerekir. Bilinen sorunların herhangi bir yeni sisteme geçirilmesi beklenmeyen sonuçlara neden olabilir ve herhangi bir performans karşılaştırmayı geçersiz kılabilir.

Bu etkinliğin bir sonucu olarak, kaynak sisteminizdeki CPU, bellek ve GÇ kullanımı için Ortalama ve en yüksek değerleri, iş yükünüzün en çok baskın ve en önemli sorgularının ortalama ve en fazla süresi ve CPU kullanımı için belgelenmiş olmalıdır. Yönetilen bir örnekteki iş yükünüzün performansını, kaynak SQL Server örneğindeki iş yükünün temel performansına göre karşılaştırmak için bu değerleri daha sonra kullanmanız gerekir.

## <a name="deploy-to-an-optimally-sized-managed-instance"></a>En iyi boyuta göre yönetilen örneğe dağıtın

SQL yönetilen örneği, buluta geçiş yapmayı planlayan şirket içi iş yükleri için tasarlanmıştır. İş yükleriniz için doğru kaynak düzeyini seçerken daha fazla esneklik sağlayan [Yeni bir satın alma modeli](../database/service-tiers-vcore.md) sunar. Şirket içi dünyada, fiziksel çekirdekler ve GÇ bant genişliği kullanarak bu iş yüklerini boyutlandırmayı merak ediyor olabilirsiniz. Yönetilen örnek için satın alma modeli, sanal çekirdekleri veya ek depolama ve ıO 'ları ayrı olarak kullanılabilir olan "Vçekirdekler" temelinde temel alır. Sanal çekirdek modeli, buluttaki işlem gereksinimlerinizi ve şirket içi olarak kullandığınız şekilde anlamanız için daha basit bir yoldur. Bu yeni model, bulutta hedef ortamınızı doğru bir şekilde boyutlandırmanızı sağlar. Doğru hizmet katmanını ve özelliklerini seçmenize yardımcı olabilecek bazı genel yönergeler aşağıda açıklanmıştır:

- Temel CPU kullanımına bağlı olarak, SQL Server üzerinde kullandığınız çekirdek sayısıyla eşleşen bir yönetilen örnek sağlayabilirsiniz. Bu, CPU özelliklerinin [yönetilen örneğin yüklü olduğu VM özellikleriyle](resource-limits.md#hardware-generation-characteristics)eşleşecek şekilde ölçeklendirilmesi gerektiğini aklınızda bulundurun.
- Temel bellek kullanımı temel alınarak, [eşleşen belleği olan hizmet katmanını](resource-limits.md#hardware-generation-characteristics)seçin. Bellek miktarı doğrudan seçilemez, bu nedenle yönetilen örneği, eşleşen belleği olan sanal çekirdeklerle seçmeniz gerekir (örneğin, 5. nesil içinde 5,1 GB/sanal çekirdek).
- Dosya alt sisteminin temel GÇ gecikme süresine bağlı olarak Genel Amaçlı (5 MS 'den büyük gecikme) ve İş Açısından Kritik (3 MS 'den az gecikme) hizmet katmanlarından birini seçin.
- Beklenen GÇ performansını almak için, taban çizgisi işleme temelinde verilerin veya günlük dosyalarının boyutunu önceden ayırın.

Dağıtım zamanında işlem ve depolama kaynaklarını seçebilir ve ardından [Azure Portal](../database/scale-resources.md)kullanarak uygulamanız için kapalı kalma süresine bildirmeden daha sonra değiştirebilirsiniz:

![Yönetilen örnek boyutlandırma](./media/migrate-to-instance-from-sql-server/managed-instance-sizing.png)

VNet altyapısı ve yönetilen bir örnek oluşturma hakkında bilgi edinmek için bkz. [yönetilen örnek oluşturma](instance-create-quickstart.md).

> [!IMPORTANT]
> [Yönetilen örnek VNET gereksinimlerine](connectivity-architecture-overview.md#network-requirements)uygun olarak hedef VNET ve alt ağınızın tutulması önemlidir. Herhangi bir uyumsuzluk, yeni örnekler oluşturmanızı veya önceden oluşturduğunuz olanları kullanmanızı engelleyebilir. [Yeni oluşturma](virtual-network-subnet-create-arm-template.md) ve [Mevcut ağları yapılandırma](vnet-existing-add-subnet.md) hakkında daha fazla bilgi edinin.

## <a name="select-a-migration-method-and-migrate"></a>Bir geçiş yöntemi seçin ve geçirin

SQL yönetilen örneği, şirket içi veya Azure VM veritabanı uygulamalarından toplu veritabanı geçişi gerektiren Kullanıcı senaryolarını hedefler. Düzenli olarak örnek düzeyi ve/veya veritabanları arası işlevleri kullanan uygulamaların arka ucu üzerinde geçiş ve kaydırma yapmanız gerektiğinde bu en iyi seçenektir. Bu senaryonuz söz konusu olduğunda, uygulamalarınızı yeniden mimarmadan Azure 'da ilgili bir ortama bir örnek taşıyabilirsiniz.

SQL örneklerini taşımak için dikkatle planlamanız gerekir:

- Birlikte bulunması gereken tüm veritabanlarının geçişi (aynı örnekte çalışan).
- Uygulamanız, kimlik bilgileri, SQL Aracısı işleri ve işleçleri ve sunucu düzeyi Tetikleyicileri dahil olmak üzere uygulamanızın bağımlı olduğu örnek düzeyi nesnelerin geçişi.

SQL yönetilen örneği, düzenli bir DBA etkinliklerinin bazılarını yerleşik oldukları gibi platforma atamanıza olanak tanıyan bir yönetilen hizmettir. Bu nedenle, [yüksek kullanılabilirlik](../database/high-availability-sla.md) yerleşik olarak oluşturulduğu için düzenli yedeklemeler veya her zaman yapılandırma için bakım işleri gibi bazı örnek düzeyi verilerin geçirilmesi gerekmez.

SQL yönetilen örneği aşağıdaki veritabanı geçiş seçeneklerini destekler (Şu anda desteklenen tek geçiş yöntemleridir):

- Azure veritabanı geçiş hizmeti-sıfıra yakın kapalı kalma süresi ile geçiş.
- Yerel `RESTORE DATABASE FROM URL` -SQL Server yerel yedeklemeleri kullanır ve bazı kapalı kalma süresi gerektirir.

### <a name="azure-database-migration-service"></a>Azure Veritabanı Geçiş Hizmeti

[Azure veritabanı geçiş hizmeti](../../dms/dms-overview.md) , birden çok veritabanı kaynağından Azure veri platformları arasında kesintisiz geçiş sağlamak için tasarlanan, tam olarak yönetilen bir hizmettir. Bu hizmet, var olan üçüncü taraf ve SQL Server veritabanlarını Azure 'a taşımak için gereken görevleri basitleştirir. Genel önizlemede dağıtım seçenekleri Azure SQL veritabanı 'ndaki veritabanlarını ve bir Azure sanal makinesinde SQL Server veritabanlarını içerir. Veritabanı geçiş hizmeti, kurumsal iş yükleriniz için önerilen geçiş yöntemidir.

Şirket içinde SQL Server SQL Server Integration Services (SSIS) kullanırsanız, veritabanı geçiş hizmeti, SSIS paketlerini depolayan SSIS kataloğunu (SSSıSDB) geçirmeyi henüz desteklememektedir, ancak paketleri buna yeniden dağıtmanıza olanak sağlamak için, yönetilen bir örnekte yeni bir SSıSDB oluşturacak şekilde Azure Data Factory içinde Azure-SSIS Integration Runtime (IR) sağlayabilirsiniz. Bkz. [oluşturma Azure-SSIS IR Azure Data Factory](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime).

Bu senaryo ve veritabanı geçiş hizmeti için yapılandırma adımları hakkında daha fazla bilgi edinmek için, bkz. Şirket [içi veritabanınızı veritabanı geçiş hizmetini kullanarak yönetilen örneğe geçirme](../../dms/tutorial-sql-server-to-managed-instance.md).  

### <a name="native-restore-from-url"></a>URL'den yerel RESTORE

[Azure depolama](https://azure.microsoft.com/services/storage/)'da bulunan bir SQL Server örneğinden alınan yerel yedeklemelerin (. bak dosyaları) geri YÜKLENMESI, SQL yönetilen örneği 'nin hızlı ve kolay bir şekilde çevrimdışı veritabanı geçişini sağlayan temel yetilerinden biridir.

Aşağıdaki diyagramda, işleme ilişkin üst düzey bir genel bakış sunulmaktadır:

![geçiş-akış](./media/migrate-to-instance-from-sql-server/migration-flow.png)

Aşağıdaki tabloda, çalıştırdığınız kaynak SQL Server sürümüne bağlı olarak kullanabileceğiniz yöntemlerle ilgili daha fazla bilgi verilmektedir:

|Adım|SQL altyapısı ve sürümü|Yedekleme/geri yükleme yöntemi|
|---|---|---|
|Azure depolama 'ya yedeklemeyi yerleştirme|2012 SP1 'den önce CU2 UYGULAMAZSANıZ|. Bak dosyasını doğrudan Azure Storage 'a yükleme|
||2012 SP1 CU2 UYGULAMAZSANıZ-2016|[KIMLIK bilgisi sözdizimi ile](https://docs.microsoft.com/sql/t-sql/statements/restore-statements-transact-sql) kullanım dışı kullanarak doğrudan yedekleme|
||2016 ve üzeri|[SAS KIMLIK bilgisiyle](https://docs.microsoft.com/sql/relational-databases/backup-restore/sql-server-backup-to-url) kullanarak doğrudan yedekleme|
|Azure depolama 'dan yönetilen bir örneğe geri yükleme|[SAS KIMLIK BILGISIYLE URL 'den GERI yükleme](restore-sample-database-quickstart.md)|

> [!IMPORTANT]
>
> - [Saydam veri şifrelemesi](../database/transparent-data-encryption-tde-overview.md) tarafından korunan bir veritabanını yerel geri yükleme seçeneğini kullanarak yönetilen bir örneğe geçirdiğinizde, şirket Içi veya Azure VM SQL Server karşılık gelen sertifikanın veritabanı geri yüklemeden önce geçirilmesi gerekir. Ayrıntılı adımlar için bkz. [BIR TDE sertifikayı yönetilen örneğe geçirme](tde-certificate-migrate.md).
> - Sistem veritabanlarının geri yüklenmesi desteklenmiyor. Örnek düzeyi nesneleri (ana veya msdb veritabanlarında depolanır) geçirmek için, bunları, hedef örnekte betiğe ve T-SQL betikleri çalıştırmaya önerilir.

Bir veritabanı yedeklemesinin SAS kimlik bilgilerini kullanarak yönetilen örneğe nasıl geri yükleneceğini gösteren bir hızlı başlangıç için bkz. [yedeklemeden yönetilen bir örneğe geri yükleme](restore-sample-database-quickstart.md).

> [!VIDEO https://www.youtube.com/embed/RxWYojo_Y3Q]

## <a name="monitor-applications"></a>Uygulamaları izleme

Yönetilen bir örneğe geçişi tamamladıktan sonra, iş yükünüzün uygulama davranışını ve performansını izlemeniz gerekir. Bu işlem aşağıdaki etkinlikleri içerir:

- [Yönetilen örnekte çalışan iş yükünün performansını](#compare-performance-with-the-baseline) [, kaynak SQL Server örneğinde oluşturduğunuz performans temeliyle](#create-a-performance-baseline)karşılaştırın.
- Olası sorunları ve gelişmeleri belirlemek için [iş yükünüzün performansını sürekli izleyin](#monitor-performance) .

### <a name="compare-performance-with-the-baseline"></a>Taban çizgisiyle performansı karşılaştırın

Başarılı geçişten hemen sonra yapmanız gereken ilk etkinlik, iş yükünün performansını temel iş yükü performansıyla karşılaştırmaktır. Bu etkinliğin amacı, yönetilen örnekteki iş yükü performansının gereksinimlerinizi karşıladığından emin olmaktır.

Yönetilen bir örneğe veritabanı geçişi, çoğu durumda veritabanı ayarlarını ve özgün uyumluluk düzeyini tutar. Özgün ayarlar, kaynak SQL Server Örneğiniz ile karşılaştırıldığında bazı performans azalmaları riskini azaltmak için mümkün olduğunda korunur. Bir kullanıcı veritabanının uyumluluk düzeyi, geçişten önce 100 veya daha yüksek bir değere sahip ise geçişten sonra aynı kalır. Bir kullanıcı veritabanının uyumluluk düzeyi geçişten önce 90 ise, yükseltilen veritabanında, uyumluluk düzeyi 100 olarak ayarlanır ve bu, yönetilen bir örnekteki en düşük desteklenen uyumluluk düzeyidir. Sistem veritabanlarının uyumluluk düzeyi 140 ' dir. Yönetilen bir örneğe geçiş gerçekten SQL Server veritabanı altyapısının en son sürümüne geçiş yaptığından, bazı yüksek performans sorunlarından kaçınmak için iş yükünüzün performansını yeniden test etmeniz gerektiğini unutmayın.

Bir önkoşul olarak, aşağıdaki etkinlikleri tamamladığınızdan emin olun:

- Çeşitli örnek, veritabanı, tempdb ayarları ve konfigürasyonları inceleyerek, yönetilen örnekteki ayarlarınızı kaynak SQL Server örneğindeki ayarlarla hizalayın. İlk performans karşılaştırmayı çalıştırmadan önce uyumluluk düzeyleri veya şifreleme gibi ayarları değiştirdiğinizden emin olun veya etkinleştirdiğiniz yeni özelliklerden bazılarının bazı sorguları etkileyebileceğini riski kabul edin. Geçiş risklerini azaltmak için, veritabanı uyumluluk düzeyini yalnızca performans izleme sonrasında değiştirin.
- Daha iyi performans elde etmek için dosyaların boyutunu önceden ayırma gibi [genel amaçlı için en iyi depolama uygulama kılavuzunu](https://techcommunity.microsoft.com/t5/DataCAT/Storage-performance-best-practices-and-considerations-for-Azure/ba-p/305525)uygulayın.
- [Yönetilen bir örnek ve SQL Server arasındaki performans farklılıklarına neden olabilecek temel ortam farklılıkları](https://azure.microsoft.com/blog/key-causes-of-performance-differences-between-sql-managed-instance-and-sql-server/)hakkında bilgi edinin ve performansı etkileyebilecek riskleri tanımlayabilir.
- Yönetilen örneğiniz üzerinde etkinleştirilmiş sorgu deposunu ve otomatik ayarlamayı sakladığınızdan emin olun. Bu özellikler iş yükü performansını ölçmenize ve olası performans sorunlarını otomatik olarak düzeltmenize olanak sağlar. [Daha yeni bir SQL Server sürümüne yükseltme sırasında performans kararlılığını koruyun](https://docs.microsoft.com/sql/relational-databases/performance/query-store-usage-scenarios#CEUpgrade)bölümünde açıklandığı gibi, veritabanı uyumluluk düzeyi değişikliğinden önce ve sonra iş yükü performansı hakkında bilgi almak için en iyi aracı olarak sorgu deposunu nasıl kullanacağınızı öğrenin.
Şirket içi ortamınızdan mümkün olduğunca uygun olan ortamı hazırladıktan sonra, iş yükünüzü çalıştırmaya başlayabilir ve performansı ölçebilir. Ölçüm süreci, [kaynak SQL Server örneğindeki iş yükü ölçülerinizin temel performansını](#create-a-performance-baseline)oluştururken ölçülmüş parametrelerin aynısını içermelidir.
Sonuç olarak, performans parametrelerini taban çizgisiyle karşılaştırmalı ve kritik farklılıkları belirlemeniz gerekir.

> [!NOTE]
> Çoğu durumda, yönetilen örnek ve SQL Server tam olarak eşleşen performansı sağlayamaabileceksiniz. Azure SQL yönetilen örneği bir SQL Server veritabanı altyapısıdır, ancak yönetilen bir örnekteki altyapı ve yüksek kullanılabilirlik yapılandırması bazı farklılıklar getirebilir. Bazı sorguların daha hızlı olacağını bekleyebilir, bazıları diğerleri daha yavaş olabilir. Karşılaştırma hedefi, yönetilen örnekteki iş yükü performansının SQL Server (Ortalama) performans ile eşleştiğini doğrulamak ve performans ile asıl performansınınkiyle eşleşmeyen tüm kritik sorguları belirlemektir.

Performans karşılaştırmasının sonucu şu olabilir:

- Yönetilen örnekteki iş yükü performansı, SQL Server iş yükü performansından daha iyi hizalı veya daha iyidir. Bu durumda, geçişin başarılı olduğunu başarıyla onaylamışınızı tamamladınız.
- Performans parametrelerinin büyük bölümü ve iş yükünde bulunan sorgular, performans düşüklüğü olan bazı özel durumlarla birlikte ince çalışır. Bu durumda, farkları ve bunların önemini belirlemeniz gerekir. Performans düşüklüğü olan bazı önemli sorgular varsa, temel alınan SQL planlarının değişip değişmediğini veya sorguların bazı kaynak limitlerine ulaşıp vurmadığını araştırmalısınız. Bu durumda hafifletme, kritik sorgulara (örneğin, değiştirilen uyumluluk düzeyi, eski kardinalite Estimator), doğrudan veya plan kılavuzlarını kullanarak bir miktar ipucu uygulayabilir, planları etkileyebilecek istatistikleri ve dizinleri yeniden oluşturabilir veya oluşturabilirsiniz.
- Sorguların çoğu, kaynak SQL Server Örneğiniz ile karşılaştırıldığında yönetilen bir örnekte daha yavaştır. Bu durumda, örneğin GÇ sınırları, bellek sınırı, örnek günlük hızı sınırı vb. gibi [bazı kaynak sınırlarına ulaşmak](resource-limits.md#service-tier-characteristics) gibi farkın kök nedenlerini belirlemeyi deneyin. Farka neden olabilecek kaynak sınırları yoksa, veritabanının uyumluluk düzeyini değiştirmeyi deneyin veya eski kardinalite tahmini gibi veritabanı ayarlarını değiştirip testi yeniden başlatın. Performansı gerileyen sorguları belirlemek için yönetilen örnek veya sorgu deposu görünümleri tarafından belirtilen önerileri gözden geçirin.

> [!IMPORTANT]
> Azure SQL yönetilen örneği, varsayılan olarak etkinleştirilen yerleşik bir otomatik plan düzeltme özelliğine sahiptir. Bu özellik, yapıştırırken düzgün çalışan sorguların gelecekte azalmamasını sağlar. Temel performans ve planlar hakkında bilgi edinmek için yönetilen örneği etkinleştirmek üzere yeni ayarları değiştirmeden önce, bu özelliğin etkinleştirildiğinden ve eski ayarlarla iş yüküyle yeterince uzun süredir yürütülmekte olduğunuzdan emin olun.

Gereksinimlerinize uygun iş yükü performansını elde edene kadar parametreleri değiştirin veya hizmet katmanlarını en iyi yapılandırmayla yakınsama olarak yükseltin.

### <a name="monitor-performance"></a>Performansı izleme

SQL yönetilen örneği, izleme ve sorun giderme için çok sayıda gelişmiş araç sağlar ve bunları örneğinizin performansını izlemek için kullanmanız gerekir. İzlemeniz gereken parametrelerden bazıları şunlardır:

- Sağladığınız sanal çekirdek sayısının iş yükünüz için doğru eşleşme olup olmadığını belirleme örneği üzerinde CPU kullanımı.
- Daha [fazla bellek](https://techcommunity.microsoft.com/t5/Azure-SQL-Database/Do-you-need-more-memory-on-Azure-SQL-Managed-Instance/ba-p/563444)gerekip gerekmediğini öğrenmek için yönetilen Örneğinizde sayfa ömrü erkeklerin.
- `INSTANCE_LOG_GOVERNOR` `PAGEIOLATCH` Daha iyi GÇ performansı almak için dosyaları önceden ayırmanız gerekebilecek, özellikle de genel amaçlı KATMANıNDA depolama IO sorunlarınız varsa, veya gibi istatistiklerdir.

## <a name="leverage-advanced-paas-features"></a>Gelişmiş PaaS özelliklerinden yararlanın

Tam olarak yönetilen bir platformda olduğunuzda ve iş yükü performanslarını SQL Server iş yükü performansından eşleştirdiğini doğruladıktan sonra, hizmetin bir parçası olarak otomatik olarak sunulan avantajları kullanın.

Geçiş sırasında yönetilen örnekte bazı değişiklikler yapmasanız bile, en son veritabanı altyapısı geliştirmelerinden faydalanmak için örneğinizi çalıştırırken yeni özelliklerden bazılarını açmanız çok yüksektir. Bazı değişiklikler yalnızca [veritabanı uyumluluk düzeyi değiştirildikten](https://docs.microsoft.com/sql/relational-databases/databases/view-or-change-the-compatibility-level-of-a-database)sonra etkinleştirilir.

Örneğin, yönetilen örnek üzerinde yedeklemeler oluşturmanız gerekmez; hizmet yedeklemeleri sizin için otomatik olarak gerçekleştirir. Yedeklemeleri zamanlama, alma ve yönetme konusunda artık endişelenmeniz gerekmez. SQL yönetilen örneği, bu bekletme döneminde zaman içindeki herhangi bir noktaya geri yükleme olanağını, zaman içindeki bir [noktaya kurtarma (sür)](../database/recovery-using-backups.md#point-in-time-restore)kullanarak sağlar. Ayrıca [, yüksek kullanılabilirlik yerleşik olarak oluşturulduğu](../database/high-availability-sla.md) için yüksek kullanılabilirlik ayarlama konusunda endişelenmeniz gerekmez.

Güvenliği güçlendirmek için [Azure Active Directory kimlik doğrulaması](../database/security-overview.md), [Denetim](auditing-configure.md), [tehdit algılama](../database/advanced-data-security.md), [satır düzeyi güvenlik](https://docs.microsoft.com/sql/relational-databases/security/row-level-security)ve [dinamik veri maskeleme](https://docs.microsoft.com/sql/relational-databases/security/dynamic-data-masking)kullanmayı göz önünde bulundurun.

Yönetilen bir örnek, gelişmiş yönetim ve güvenlik özelliklerine ek olarak, [iş yükünüzü izlemenize ve ayarlamanıza](../database/monitor-tune-overview.md)yardımcı olabilecek bir dizi gelişmiş araç sağlar. [Azure SQL Analytics](https://docs.microsoft.com/azure/azure-monitor/insights/azure-sql) , büyük bir yönetilen örnek kümesini izlemenizi ve çok sayıda örnek ve veritabanının izlenmesini merkezileştirmenizi sağlar. Yönetilen örneklerde [otomatik ayarlama](https://docs.microsoft.com/sql/relational-databases/automatic-tuning/automatic-tuning#automatic-plan-correction) , SQL planı yürütme istatistiklerinizin performansını sürekli olarak izler ve belirlenen performans sorunlarını otomatik olarak düzeltir.

## <a name="next-steps"></a>Sonraki adımlar

- Azure SQL yönetilen örneği hakkında daha fazla bilgi için bkz. [Azure SQL yönetilen örneği nedir?](sql-managed-instance-paas-overview.md).
- Yedekten geri yükleme içeren bir öğretici için bkz. [yönetilen örnek oluşturma](instance-create-quickstart.md).
- Veritabanı geçiş hizmeti 'ni kullanarak geçişi gösteren öğretici için, bkz. Şirket [içi veritabanınızı Azure SQL yönetilen örneğine veritabanı geçiş hizmetini kullanarak geçirme](../../dms/tutorial-sql-server-to-managed-instance.md).  
