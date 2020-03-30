---
title: SQL Server'dan yönetilen örne geçirin
description: Bir veritabanını SQL Server örneğinden Azure SQL Veritabanı'na nasıl geçirtileceğinizi öğrenin - Yönetilen örnek.
services: sql-database
ms.service: sql-database
ms.subservice: migration
ms.custom: seo-lt-2019
ms.devlang: ''
ms.topic: conceptual
author: bonova
ms.author: bonova
ms.reviewer: douglas, carlrab
ms.date: 07/11/2019
ms.openlocfilehash: 6bae9e871be2a5d56d057d2a077de53329b8c3ec
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79208939"
---
# <a name="sql-server-instance-migration-to-azure-sql-database-managed-instance"></a>SQL Server örnek Azure SQL Veritabanı'na geçiş yönetilen örnek

Bu makalede, bir SQL Server 2005 veya daha sonraki sürüm örneğini [Azure SQL Veritabanı yönetilen örneğine](sql-database-managed-instance.md)geçirme yöntemleri hakkında bilgi edinirsiniz. Tek bir veritabanına veya elastik havuza geçiş hakkında bilgi [için](sql-database-cloud-migrate.md)bkz. Diğer platformlardan geçiş hakkında geçiş bilgileri için [Azure Veritabanı Geçiş Kılavuzu'na](https://datamigration.microsoft.com/)bakın.

> [!NOTE]
> Yönetilen Örnek'i hızla başlatmak ve denemek istiyorsanız, bu sayfa yerine [Hızlı başlangıç kılavuzuna](sql-database-managed-instance-quickstart-guide.md) gitmek isteyebilirsiniz. 

Yüksek düzeyde, veritabanı geçiş işlemi gibi görünür:

![geçiş süreci](./media/sql-database-managed-instance-migration/migration-process.png)

- Geçişlerinizi engelleyecek engelleme sorunları olmadığından emin olması gereken [yönetilen örnek uyumluluğunu değerlendirin.](#assess-managed-instance-compatibility)
  - Bu adım, kaynak SQL Server örneğinizdeki kaynak kullanımını belirlemek için [performans taban çizgisinin](#create-performance-baseline) oluşturulmasını da içerir. O'nu doğru boyutlandırılmış Yönetilen Örnek'i dağıtmak ve geçişten sonra performansların etkilenmediğini doğrulamak istiyorsanız bu adım gereklidir.
- [Uygulama bağlantı seçeneklerini seçin](sql-database-managed-instance-connect-app.md)
- Yönetilen Örneğinizin teknik özelliklerini (vCore sayısı, bellek miktarı) ve performans katmanını (İş Açısından Kritik, Genel Amaç) seçeceğiniz [en uygun büyüklükte yönetilen bir örneğe dağıtın.](#deploy-to-an-optimally-sized-managed-instance)
- [Geçiş yöntemini seçin ve](#select-migration-method-and-migrate) çevrimdışı geçiş (yerel yedekleme/geri yükleme, veritabanı içe aktarma/dışakverme) veya çevrimiçi geçiş (Veri Geçişi Hizmeti, İşlem Çoğaltma) kullanarak veritabanlarınızı geçirin.
- Beklenen performansa sahip olduğundan emin olmak için [uygulamaları izleyin.](#monitor-applications)

> [!NOTE]
> Tek bir veritabanını tek bir veritabanına veya elastik havuza geçirmek [için](sql-database-single-database-migrate.md)bkz.

## <a name="assess-managed-instance-compatibility"></a>Yönetilen örnek uyumluluğunu değerlendirme

İlk olarak, yönetilen örneğin uygulamanızın veritabanı gereksinimleriyle uyumlu olup olmadığını belirleyin. Yönetilen örnek dağıtım seçeneği, SQL Server'ı şirket içinde veya sanal makinelerde kullanan varolan uygulamaların çoğu için kolay kaldırma ve kaydırma geçişi sağlamak üzere tasarlanmıştır. Ancak, bazen henüz desteklenmeyen özellikler veya özellikler isteyebilirsiniz ve geçici çözüm uygulama maliyeti çok yüksektir.

Azure SQL Veritabanı'nda veritabanı işlevselliğini etkileyen olası uyumluluk sorunlarını algılamak için [Veri Geçişi Yardımcısı'nı (DMA)](https://docs.microsoft.com/sql/dma/dma-overview) kullanın. DMA henüz geçiş hedefi olarak yönetilen örneği desteklemez, ancak Azure SQL Veritabanı'na karşı değerlendirme çalıştırmak ve bildirilen özellik paritesi ve ürün belgelerine karşı uyumluluk sorunlarının listesini dikkatle gözden geçirmek önerilir. Azure SQL Veritabanı'na geçişi engelleyen engelleme sorunlarının çoğu yönetilen örnekle kaldırıldığından, yönetilen örnekte engelleyici olmayan bazı bildirilen engelleme sorunları olup olmadığını denetlemek için [Azure SQL Veritabanı özelliklerine](sql-database-features.md) bakın. Örneğin, veritabanı arası sorgular, aynı örnekteki veritabanı arası işlemler, sunucuyu diğer SQL kaynaklarına bağlı sunucu, CLR, genel geçici tablolar, örnek düzeyi görünümleri, Servis Aracısı ve benzeri özellikler yönetilen örneklerde kullanılabilir.

Yönetilen örnek dağıtım seçeneğiyle kaldırılmayan bildirilen bazı engelleme sorunları varsa, [Azure sanal makinelerdeki SQL Server](https://azure.microsoft.com/services/virtual-machines/sql-server/)gibi alternatif bir seçeneği göz önünde bulundurmanız gerekebilir. İşte bazı örnekler:

- İşletim sistemine veya dosya sistemine doğrudan erişime ihtiyacınız varsa, örneğin SQL Server ile aynı sanal makineye üçüncü taraf veya özel aracılar yüklemek.
- FileStream / FileTable, PolyBase ve çapraz örnek işlemleri gibi hala desteklenmeyen özelliklere sıkı bir şekilde bağımlıysanız.
- SQL Server'ın belirli bir sürümünde kesinlikle kalmak gerekiyorsa (örneğin, 2012).
- İşlem gereksinimleriniz yönetilen örnek teklifleri (örneğin bir vCore) ve veritabanı konsolidasyonu kabul edilebilir bir seçenek değildir çok daha düşükse.

Tanımlanan tüm geçiş engelleyicilerini çözdüyseniz ve Geçiş'i Yönetilen Örnek'e devam ettiyseniz, değişikliklerden bazılarının iş yükünüzün performansını etkileyebileceğini unutmayın:
- Zorunlu tam kurtarma modeli ve düzenli otomatik yedekleme zamanlaması, isteğe bağlı olarak basit/toplu günlüğe kaydedilmiş modeli düzenli olarak kullandıysanız veya yedeklemeleri durdurduysanız iş yükü veya bakım/ETL eylemlerinizin performansını etkileyebilir.
- İzleme bayrakları veya uyumluluk düzeyleri gibi farklı sunucu veya veritabanı düzeyi yapılandırmaları
- Saydam Veritabanı Şifreleme (TDE) veya otomatik hata grupları gibi kullandığınız yeni özellikler CPU ve IO kullanımını etkileyebilir.

Yönetilen Örnek, kritik senaryolarda bile %99,99 kullanılabilirliği garanti eder, bu nedenle bu özelliklerin neden olduğu genel ek su devre dışı alınamaz. Daha fazla bilgi için, [SQL Server ve Yönetilen Örnek'te farklı performansa neden olabilecek kök nedenlerini](https://azure.microsoft.com/blog/key-causes-of-performance-differences-between-sql-managed-instance-and-sql-server/)görün.

### <a name="create-performance-baseline"></a>Performans taban çizgisi oluşturma

Yönetilen Örnek'teki iş yükünüzün performansını SQL Server'da çalışan özgün iş yükünüzüzle karşılaştırmak için kullanılacak bir performans taban çizgisi oluşturmanız gerekir. 

Performans taban çizgisi, ortalama/maksimum CPU kullanımı, ortalama/max disk IO gecikmesi, iş sonu, IOPS, ortalama/maksimum sayfa ömrü beklentisi, tempdb'nin ortalama maksimum boyutu gibi bir dizi parametredir. Geçişten sonra benzer veya daha iyi parametrelere sahip olmak istiyorsunuz, bu nedenle bu parametrelerin temel değerlerini ölçmek ve kaydetmek önemlidir. Sistem parametrelerine ek olarak, temsilci sorguları kümesini veya iş yüklerinizdeki en önemli sorguları seçmeniz ve seçili sorgular için min/ortalama/maksimum süre, CPU kullanımını ölçmeniz gerekir. Bu değerler, Yönetilen Örnek'te çalışan iş yükünün performansını kaynak SQL Server'ınızdaki özgün değerlerle karşılaştırmanızı sağlar.

SQL Server örneğinde ölçmeniz gereken parametrelerden bazıları şunlardır: 
- [CPU kullanımını SQL Server örneğinde izleyin](https://techcommunity.microsoft.com/t5/Azure-SQL-Database/Monitor-CPU-usage-on-SQL-Server/ba-p/680777#M131) ve ortalama ve en yüksek CPU kullanımını kaydedin.
- [SQL Server örneğinde bellek kullanımını izleyin](https://docs.microsoft.com/sql/relational-databases/performance-monitor/monitor-memory-usage) ve arabellek havuzu, plan önbelleği, sütun deposu havuzu, [bellek içi OLTP,](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/monitor-and-troubleshoot-memory-usage?view=sql-server-2017)vb. gibi farklı bileşenler tarafından kullanılan bellek miktarını belirleyin. Buna ek olarak, Sayfa Ömrü Beklenti bellek performans sayacının ortalama ve tepe değerlerini bulmanız gerekir.
- [Sys.dm_io_virtual_file_stats](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-io-virtual-file-stats-transact-sql) görünümü veya [performans sayaçlarını](https://docs.microsoft.com/sql/relational-databases/performance-monitor/monitor-disk-usage)kullanarak kaynak SQL Server örneğinde disk IO kullanımını izleyin.
- SQL Server 2016+ sürümünden geçiş yapıyorsunuzdinamik yönetim görünümlerini veya Query Store'u inceleyerek iş yükünü ve sorgu performansını veya SQL Server örneğini izleyin. İş yüklerinizdeki en önemli sorguların ortalama süresini ve CPU kullanımını belirleyerek Yönetilen Örnek'te çalışan sorgularla karşılaştırın.

> [!Note]
> SQL Server'daki yüksek CPU kullanımı, sabit bellek basıncı, tempdb veya parametreleştirme sorunları gibi iş yükünüzle ilgili herhangi bir sorun fark ederseniz, taban çizgisini ve geçişi almadan önce bunları kaynak SQL Server örneğinde çözmeye çalışmalısınız. Herhangi bir yeni sistem migh için know sorunları geçirin beklenmeyen sonuçlara neden olur ve herhangi bir performans karşılaştırma geçersiz.

Bu etkinliğin bir sonucu olarak, kaynak sisteminizde CPU, bellek ve IO kullanımının yanı sıra iş yükünüzdeki baskın ve en kritik sorguların ortalama ve maksimum süresi ve CPU kullanımını belgelemiş olmalısınız. Yönetilen Örnek'teki iş yükünüzün performansını kaynak SQL Server'daki iş yükünün temel performansıyla karşılaştırmak için bu değerleri daha sonra kullanmanız gerekir.

## <a name="deploy-to-an-optimally-sized-managed-instance"></a>En iyi boyutta yönetilen bir örne dağıtma

Yönetilen örnek, buluta geçmeyi planlayan şirket içi iş yükleri için özel olarak uyarlanır. İş yükleriniz için doğru kaynak düzeyini seçerken daha fazla esneklik sağlayan yeni bir [satın alma modeli](sql-database-service-tiers-vcore.md) sunar. Şirket içi dünyada, fiziksel çekirdekler ve IO bant genişliği kullanarak bu iş yüklerini boyutlandırmaya muhtemelen alışkınsınızdır. Yönetilen örneğin satın alma modeli sanal çekirdeklere veya ek depolama alanı ve IO'nun ayrı olarak kullanılabildiği "vCores"a dayanır. vCore modeli, bugün şirket içinde kullandığınız şeye karşı buluttaki işlem gereksinimlerinizi anlamanın daha basit bir yoludur. Bu yeni model, hedef ortamınızı bulutta doğru boyutlandırmanızı sağlar. Doğru hizmet katmanını ve özelliklerini seçmenize yardımcı olabilecek bazı genel yönergeler burada açıklanmıştır:
- Temel CPU kullanımına bağlı olarak, CPU özelliklerinin Yönetilen Örnek yüklü [olduğu VM özellikleriyle](sql-database-managed-instance-resource-limits.md#hardware-generation-characteristics)eşleşecek şekilde ölçeklenmesi gerekebileceğini göz önünde bulundurarak, SQL Server'da kullandığınız çekirdek sayısıyla eşleşen yönetilen bir Örnek sağlayabilirsiniz.
- Temel bellek kullanımına bağlı olarak [eşleşen belleğe sahip hizmet katmanını](sql-database-managed-instance-resource-limits.md#hardware-generation-characteristics)seçin. Bellek miktarı doğrudan seçilemez, bu nedenle eşleşen belleğe sahip vCores miktarıyla Yönetilen Örneği seçmeniz gerekir (örneğin, Gen5'te 5,1 GB/vCore). 
- Dosya alt sisteminin temel IO gecikmedurumuna bağlı olarak Genel Amaç (5 ms'den büyük gecikme) ve Business Critical hizmet katmanları (gecikme 3 ms'den az) arasında seçim yapın.
- Taban çizgisi iş başına göre beklenen IO performansını elde etmek için veri veya günlük dosyalarının boyutunu önceden ayırın.

Dağıtım zamanında bilgi işlem ve depolama kaynaklarını seçebilir ve [azure portalını](sql-database-scale-resources.md)kullanarak uygulamanızın kapalı kalma süresini tanıtmadan daha sonra değiştirebilirsiniz:

![yönetilen örnek boyutlandırma](./media/sql-database-managed-instance-migration/managed-instance-sizing.png)

VNet altyapısını ve yönetilen bir örneği nasıl oluşturacağınıöğrenmek için [bkz.](sql-database-managed-instance-get-started.md)

> [!IMPORTANT]
> Hedef VNet ve subnet her zaman [yönetilen örnek VNet gereksinimlerine](sql-database-managed-instance-connectivity-architecture.md#network-requirements)uygun tutmak önemlidir. Herhangi bir uyumsuzluk, yeni örnekler oluşturmanızı veya zaten oluşturduğunuz örnekleri kullanmanızı engelleyebilir. Yeni ağlar [oluşturma](sql-database-managed-instance-create-vnet-subnet.md) ve [varolan](sql-database-managed-instance-configure-vnet-subnet.md) ağları yapılandırma hakkında daha fazla bilgi edinin.

## <a name="select-migration-method-and-migrate"></a>Geçiş yöntemini seçin ve geçiş

Yönetilen örnek dağıtım seçeneği, şirket içi veya IaaS veritabanı uygulamalarından toplu veritabanı geçişi gerektiren kullanıcı senaryolarını hedefler. Bunlar, örnek düzeyi ve/veya veritabanı arası işlevleri düzenli olarak kullanan uygulamaların arka ucunu kaldırmanız ve kaydırmanız gerektiğinde en uygun seçimdir. Senaryonuz buysa, uygulamalarınızı yeniden yeniden yeniden yapılandırmaya gerek kalmadan tüm örneği Azure'daki ilgili ortama taşıyabilirsiniz.

SQL örneklerini taşımak için aşağıdakileri dikkatle planlamanız gerekir:

- Harmanlanmış olması gereken tüm veritabanlarının geçişi (aynı örnekte çalışan veritabanları)
- Oturum açma, kimlik bilgileri, SQL Aracı İşleri ve İşleçleri ve sunucu düzeyi tetikleyicileri dahil olmak üzere uygulamanızın bağlı olduğu örnek düzeyindeki nesnelerin geçişi.

Yönetilen örnek, normal DBA etkinliklerinden bazılarını yerleşik olarak platforma atamanızı sağlayan yönetilen bir hizmettir. Bu nedenle, [yüksek kullanılabilirlik](sql-database-high-availability.md) yerleşik olarak, normal yedeklemeler için bakım işleri veya Her Zaman yapılandırma gibi bazı örnek düzeyi verilerin infilak edilmesi gerekmez.

Yönetilen örnek aşağıdaki veritabanı geçiş seçeneklerini destekler (şu anda bunlar desteklenen tek geçiş yöntemleridir):

- Azure Veritabanı Geçiş Hizmeti - sıfıra yakın kapalı kalma süresiyle geçiş,
- Yerel `RESTORE DATABASE FROM URL` - SQL Server'ın yerel yedeklemelerini kullanır ve bazı kapalı kalma süresi gerektirir.

### <a name="azure-database-migration-service"></a>Azure Veritabanı Geçiş Hizmeti

[Azure Veritabanı Geçiş Hizmeti (DMS),](../dms/dms-overview.md) birden çok veritabanı kaynağından Azure Veri platformlarına en az kapalı kalma süresiyle sorunsuz geçişler sağlamak üzere tasarlanmış tam olarak yönetilen bir hizmettir. Bu hizmet, varolan üçüncü taraf ve SQL Server veritabanlarını Azure'a taşımak için gereken görevleri kolaylaştırır. Genel önizlemedeki dağıtım seçenekleri arasında Azure SQL Veritabanı'ndaki veritabanları ve Bir Azure Sanal Makinesi'ndeki SQL Server veritabanları yer almaktadır. DMS, kurumsal iş yükleriniz için önerilen geçiş yöntemidir.

SQL Server'ınızda SQL Server Integration Services (SSIS) kullanıyorsanız, DMS henüz SSIS paketlerini depolayan SSIS kataloğunu (SSISDB) desteklemez, ancak Azure Veri Fabrikası'nda (ADF) yeni bir SSISDB oluşturacak azure-SSIS Entegrasyon Çalışma Süresi (IR) sağlayabilir ve ardından paketlerinizi buna yeniden [dağıtabilirsiniz, Azure Create-SIS'e bakın.](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime)

DMS için bu senaryo ve yapılandırma adımları hakkında daha fazla bilgi edinmek için [bkz.](../dms/tutorial-sql-server-to-managed-instance.md)  

### <a name="native-restore-from-url"></a>URL'den yerel RESTORE

[Azure Depolama'da](https://azure.microsoft.com/services/storage/)kullanılabilen SQL Server şirket içi veya [Sanal Makinelerdeki SQL Server'dan](https://azure.microsoft.com/services/virtual-machines/sql-server/)alınan yerel yedeklemelerin (.bak dosyaları) geri yüklemesi, yönetilen örnek dağıtım seçeneğinin hızlı ve kolay çevrimdışı veritabanı geçişine olanak tanıyan önemli özelliklerinden biridir.

Aşağıdaki diyagram, işlemin üst düzey bir genel görünümünü sağlar:

![geçiş akışı](./media/sql-database-managed-instance-migration/migration-flow.png)

Aşağıdaki tablo, çalıştırdığınız kaynak SQL Server sürümüne bağlı olarak kullanabileceğiniz yöntemlerle ilgili daha fazla bilgi sağlar:

|Adım|SQL Motoru ve sürümü|Yedekleme / Geri yükleme yöntemi|
|---|---|---|
|Yedeklemeyi Azure Depolama'ya koyun|Önceki SQL 2012 SP1 CU2|.bak dosyasını doğrudan Azure depolama alanına yükleyin|
||2012 SP1 CU2 - 2016|Deprecated WITH [CREDENTIAL](https://docs.microsoft.com/sql/t-sql/statements/restore-statements-transact-sql) sözdizimini kullanarak doğrudan yedekleme|
||2016 ve üzeri|[WITH SAS CredentiAL](https://docs.microsoft.com/sql/relational-databases/backup-restore/sql-server-backup-to-url) kullanarak doğrudan yedekleme|
|Azure depolamadan yönetilen örne geri yükleme|[SAS Kimlik Bilgileri ile URL'DEN GERI YÜKLEME](sql-database-managed-instance-get-started-restore.md)|

> [!IMPORTANT]
> - [Saydam Veri Şifreleme](transparent-data-encryption-azure-sql.md) tarafından korunan bir veritabanını yerel geri yükleme seçeneğini kullanarak yönetilen bir örneğe geçirdiğinizde, şirket içi veya IaaS SQL Server'dan gelen ilgili sertifikanın veritabanı geri yüklemesinden önce geçirilmesi gerekir. Ayrıntılı adımlar [için](sql-database-managed-instance-migrate-tde-certificate.md) bkz.
> - Sistem veritabanlarının geri yüklenmeleri desteklenmez. Örnek düzeyindeki nesneleri (ana veya msdb veritabanlarında depolanan) geçirmek için, bunları komut dosyasıdışına atmanızı ve hedef örnekte T-SQL komut dosyalarını çalıştırmanızı öneririz.

Bir Veritabanı yedeklemesini SAS kimlik bilgisi kullanarak yönetilen bir örneğe nasıl geri yükleyin, [bkz.](sql-database-managed-instance-get-started-restore.md)

> [!VIDEO https://www.youtube.com/embed/RxWYojo_Y3Q]


## <a name="monitor-applications"></a>Uygulamaları izleme

Yönetilen Örnek'e geçişi tamamladıktan sonra, uygulama davranışını ve iş yükünüzün performansını izlemeniz gerekir. Bu işlem aşağıdaki etkinlikleri içerir:
- [Yönetilen Örnek'te çalışan iş yükünün performansını](#compare-performance-with-the-baseline) [kaynak SQL Server'da oluşturduğunuz performans taban çizgisiyle](#create-performance-baseline)karşılaştırın.
- Olası sorunları ve iyileştirmeyi belirlemek için [iş yükünüzün performansını](#monitor-performance) sürekli olarak izleyin.

### <a name="compare-performance-with-the-baseline"></a>Performansı taban çizgisiyle karşılaştırın

Başarılı geçişten hemen sonra almanız gereken ilk etkinlik, iş yükünün performansını temel iş yükü performansıyla karşılaştırmaktır. Bu etkinliğin amacı, Yönetilen Örneğinizdeki iş yükü performansının gereksinimlerinizi karşıladığını doğrulamaktır. 

Yönetilen Örnek'e veritabanı geçişi, çoğu durumda veritabanı ayarlarını ve özgün uyumluluk düzeyini tutar. Orijinal ayarlar, kaynak SQL Server'ınıza kıyasla bazı performans bozulmaları riskini azaltmak için mümkün olduğunca korunur. Bir kullanıcı veritabanının uyumluluk düzeyi geçişten önce 100 veya daha yüksekse, geçişten sonra aynı kalır. Bir kullanıcı veritabanının uyumluluk düzeyi geçişten önce 90 ise, yükseltilen veritabanında uyumluluk düzeyi yönetilen örnekte desteklenen en düşük uyumluluk düzeyi olan 100 olarak ayarlanır. Sistem veritabanlarının uyumluluk düzeyi 140'tır. Yönetilen Örnek'e geçiş aslında SQL Server Database Engine'in en son sürümüne geçiş olduğundan, bazı şaşırtıcı performans sorunlarını önlemek için iş yükünüzün performansını yeniden test etmeniz gerektiğini bilmeniz gerekir.

Ön koşul olarak, aşağıdaki etkinlikleri tamamladığınızdan emin olun:
- Çeşitli örnekleri, veritabanını, temdb ayarlarını ve yapılandırmaları araştırarak, Yönetilen Örnek'teki ayarlarınızı kaynak SQL Server örneğindeki ayarlarla hizalayın. İlk performans karşılaştırmasını çalıştırmadan önce uyumluluk düzeyleri veya şifreleme gibi ayarları değiştirmediğinizden emin olun veya etkinleştirdiğiniz yeni özelliklerden bazılarının bazı sorguları etkileme riskini kabul edin. Geçiş risklerini azaltmak için veritabanı uyumluluk düzeyini yalnızca performans izlemeden sonra değiştirin.
- Daha iyi performans elde etmek için dosyaların boyutunu önceden ayırmak gibi [Genel Amaç için depolama en iyi uygulama yönergeleri](https://techcommunity.microsoft.com/t5/DataCAT/Storage-performance-best-practices-and-considerations-for-Azure/ba-p/305525) uygulayın.
- [Yönetilen Örnek ve SQL Server arasındaki performans farklılıklarına neden olabilecek ve]( https://azure.microsoft.com/blog/key-causes-of-performance-differences-between-sql-managed-instance-and-sql-server/) performansı etkileyebilecek riskleri tanımlayabilen temel ortam farklılıkları hakkında bilgi edinin.
- Yönetilen Örneğinizde Etkin Sorgu Deposu ve Otomatik ayar tutmayı sakladığınızdan emin olun. Bu özellikler, iş yükü performansını ölçmenize ve olası performans sorunlarını otomatik olarak düzeltmenize olanak tanır. Sorgu Mağazası'nı, veritabanı uyumluluğu düzeyi değişikliğinden önce ve sonra iş yükü performansı hakkında bilgi almak için en uygun araç olarak nasıl kullanacağınızı öğrenin, [yeni SQL Server sürümüne yükseltme sırasında performans kararlılığını koruyun'da](https://docs.microsoft.com/sql/relational-databases/performance/query-store-usage-scenarios#CEUpgrade)açıklandığı gibi.
Şirket içi ortamınızla mümkün olduğunca karşılaştırılabilir bir ortam hazırladıktan sonra, iş yükünüzü çalıştırmaya başlayabilir ve performansınızı ölçebilirsiniz. Ölçüm [işlemi, kaynak SQL Server'da iş yükü ölçülerinizin temel performansını oluştururken](#create-performance-baseline)ölçtüğünüz parametreleri içermelidir.
Sonuç olarak, performans parametrelerini taban çizgisiyle karşılaştırmalı ve kritik farklılıkları belirlemelisiniz.

> [!NOTE]
> Çoğu durumda, Yönetilen Örnek ve SQL Server'da tam olarak eşleşen performans elde edemeyebilirsiniz. Yönetilen Örnek bir SQL Server veritabanı altyapısıdır, ancak Yönetilen Örnek'teki altyapı ve Yüksek kullanılabilirlik yapılandırması bazı farklar yaratabilir. Bazı sorguların daha hızlı, bazılarının ise daha yavaş olmasını bekleyebilirsiniz. Karşılaştırmanın amacı, Yönetilen Örnek'teki iş yükü performansının SQL Server'daki performansla eşleştiğini doğrulamaktır (ortalama olarak) ve orijinal performansınızla eşleşmeyan performansla ilgili kritik sorgular olduğunu belirlemektir.

Performans karşılaştırmasının sonucu şu olabilir:
- Yönetilen Örnek'teki iş yükü performansı, SQL Server'daki iş yükü performansıyla hizalanır veya daha iyi bir performanstır. Bu durumda, geçişin başarılı olduğunu başarıyla doğruladınız.
- Performans parametrelerinin ve iş yükündeki sorguların çoğu, azalan performansla bazı istisnalar dışında iyi çalışır. Bu durumda, farklılıkları ve bunların önemini belirlemeniz gerekir. Performansı bozulmuş bazı önemli sorgular varsa, temel SQL planlarının değiştiğini veya sorguların bazı kaynak sınırlarına çarptığını araştırmanız gerekir. Bu durumda azaltma, kritik sorgular (örneğin değiştirilen uyumluluk düzeyi, eski kardinallik tahmincisi) üzerinde bazı ipuçları uygulamak veya plan kılavuzları kullanarak, yeniden oluşturma veya planları etkileyebilecek istatistik ler ve dizinler oluşturmak olabilir. 
- Sorguların çoğu, kaynak SQL Server'ınızla karşılaştırıldığında Yönetilen Örnek'te daha yavaştır. Bu durumda, IO sınırları, bellek sınırı, örnek günlük oranı sınırı, vb gibi [bazı kaynak sınırına ulaşmak]( sql-database-managed-instance-resource-limits.md#service-tier-characteristics) gibi farkın kök nedenlerini belirlemeye çalışın. Farka neden olabilecek kaynak sınırları yoksa, veritabanının uyumluluk düzeyini değiştirmeyi veya eski kardinallik tahmini gibi veritabanı ayarlarını değiştirmeyi deneyin ve testi yeniden başlatın. Performansı gerileyen sorguları tanımlamak için Yönetilen Örnek veya Sorgu Mağazası görünümleri tarafından sağlanan önerileri gözden geçirin.

> [!IMPORTANT]
> Yönetilen Örnek varsayılan olarak etkinleştirilen yerleşik otomatik plan düzeltme özelliğine sahiptir. Bu özellik, yapıştırmada iyi çalışan sorguların gelecekte bozulmamasını sağlar. Yönetilen Örnek'in temel performans ve planlar hakkında bilgi edinmesini sağlamak için yeni ayarları değiştirmeden önce bu özelliğin etkin olduğundan ve iş yükünü eski ayarlarla yeterince uzun süre yürüttüğünden emin olun.

İhtiyaçlarınıza uygun iş yükü performansını elde edene kadar en uygun yapılandırmaya yakınsamak için parametreleri veya yükseltme hizmet katmanlarını değiştirin.

### <a name="monitor-performance"></a>Performansı izleme

Yönetilen Örnek izleme ve sorun giderme için çok sayıda gelişmiş araç sağlar ve bunları örneğinizdeki performansı izlemek için kullanmalısınız. İzlemeniz gereken parametrelerden bazıları şunlardır:
- Örneğin CPU kullanımı, size sağlanan vCores sayısını belirlemek için iş yükünüz için doğru eşleşir.
- [Ek belleğe ihtiyacınız olup olmadığını](https://techcommunity.microsoft.com/t5/Azure-SQL-Database/Do-you-need-more-memory-on-Azure-SQL-Managed-Instance/ba-p/563444)belirlemek için Yönetilen Örneğinizde sayfa ömrü beklentisi.
- Daha `INSTANCE_LOG_GOVERNOR` iyi `PAGEIOLATCH` IO performansı elde etmek için dosyaları önceden ayırmanız gerekebilecek Genel Amaçlı katmanda depolama IO sorunlarınız olup olmadığını gösteren istatistikler gibi veya bu istatistikleri bekleyin.

## <a name="leverage-advanced-paas-features"></a>Gelişmiş PaaS özelliklerinden yararlanın

Tam olarak yönetilen bir platforma sahip olduktan ve iş yükü performanslarının size SQL Server iş yükü performansıyla eşleştiğini doğruladıktan sonra, SQL Veritabanı hizmetinin bir parçası olarak otomatik olarak sağlanan avantajlarından yararlanın. 

Geçiş sırasında yönetilen örnekte bazı değişiklikler yapmasanız bile, en son veritabanı altyapısı geliştirmelerinden yararlanmak için örneğini çalıştırırken bazı yeni özellikleri açma olasılığınız yüksektir. Bazı değişiklikler yalnızca veritabanı uyumluluk düzeyi değiştirildikten sonra [etkinleştirilir.](https://docs.microsoft.com/sql/relational-databases/databases/view-or-change-the-compatibility-level-of-a-database)


Örneğin, yönetilen örnekte yedekleme oluşturmanız gerekmez - hizmet sizin için otomatik olarak yedekleme yapar. Artık yedeklemeleri zamanlama, alma ve yönetme konusunda endişelenmenize gerek yok. Yönetilen örnek, [Point in Time Recovery (PITR)](sql-database-recovery-using-backups.md#point-in-time-restore)kullanarak bu bekletme süresi içinde herhangi bir noktaya geri yükleme olanağı sağlar. Ayrıca, [yüksek kullanılabilirlik](sql-database-high-availability.md) yerleşik olarak yüksek kullanılabilirlik kurma konusunda endişelenmenize gerek yok.

Güvenliği güçlendirmek için [Azure Active Directory Authentication,](sql-database-security-overview.md) [Auditing,](sql-database-managed-instance-auditing.md) [threat detection,](sql-database-advanced-data-security.md) [row-level security](https://docs.microsoft.com/sql/relational-databases/security/row-level-security)ve [dinamik veri maskeleme](https://docs.microsoft.com/sql/relational-databases/security/dynamic-data-masking) yi kullanmayı düşünün.

Gelişmiş yönetim ve güvenlik özelliklerine ek olarak, Yönetilen [Örnek, iş yükünüzü izlemenize ve ayarlamanıza](sql-database-monitor-tune-overview.md)yardımcı olabilecek bir dizi gelişmiş araç sağlar. [Azure SQL analitiği,](https://docs.microsoft.com/azure/azure-monitor/insights/azure-sql) çok sayıda Yönetilen Örnek kümesini izlemenize ve çok sayıda örnek ve veritabanının izlenmesini merkezileştirmenize olanak tanır. Yönetilen Örnek'te [otomatik ayar,](https://docs.microsoft.com/sql/relational-databases/automatic-tuning/automatic-tuning#automatic-plan-correction) SQL plan yürütme istatistiklerinizin performansını sürekli olarak izler ve tanımlanan performans sorunlarını otomatik olarak giderir.

## <a name="next-steps"></a>Sonraki adımlar

- Yönetilen örnekler hakkında bilgi için [bkz.](sql-database-managed-instance.md)
- Yedeklemeden geri yükleme içeren bir öğretici için [bkz.](sql-database-managed-instance-get-started.md)
- DMS kullanarak geçişi gösteren öğretici için [bkz.](../dms/tutorial-sql-server-to-managed-instance.md)  
