---
title: Azure SQL Veritabanı hizmeti nedir?
description: "SQL Veritabanı'na giriş alın: buluttaki Microsoft ilişkisel veritabanı yönetim sisteminin (RDBMS) teknik ayrıntıları ve yetenekleri."
keywords: sql'e giriş,sql veritabanı nedir
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: carlrab
ms.date: 04/08/2019
ms.openlocfilehash: 0d50ddbbeeaed48c14d07c42588efcbb20bb7d79
ms.sourcegitcommit: 27bbda320225c2c2a43ac370b604432679a6a7c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80411166"
---
# <a name="what-is-the-azure-sql-database-service"></a>Azure SQL Veritabanı hizmeti nedir?

Azure SQL Veritabanı, yükseltme, düzeltme eki uygulama, yedekleme ve izleme gibi çoğu veritabanı yönetimi işlevlerini kullanıcının müdahalesi gerekmeden yapan bir tam olarak yönetilen Hizmet olarak Platform (PaaS) Veritabanı Altyapısıdır. Azure SQL Veritabanı her zaman SQL Server Veritabanı Altyapısı’nın en son kararlı sürümünde çalışır ve %99,99 kullanılabilirlik ile düzeltme eki uygulanmış bir işletim sistemidir. Azure SQL veritabanında yerleşik olan PaaS yetenekleri, işletmeniz için kritik öneme sahip etki alanına özgü veritabanı yönetimi ve optimizasyon etkinliklerine odaklanmanızı sağlar.

Azure SQL Veritabanı ile Azure'daki uygulamalar ve çözümler için yüksek kullanılabilir ve yüksek performanslı bir veri depolama katmanı oluşturabilirsiniz. SQL Veritabanı, grafikler, JSON, uzamsal ve XML gibi hem ilişkisel verileri hem de [ilişkisel olmayan yapıları](sql-database-multi-model-features.md)işlemenizi sağladığından, çeşitli modern bulut uygulamaları için doğru seçim olabilir.

[Microsoft SQL Server veritabanı altyapısının](https://docs.microsoft.com/sql/sql-server/sql-server-technical-documentation?toc=/azure/sql-database/toc.json)en son kararlı sürümüne dayanır. [Yüksek performanslı bellek içi teknolojiler](sql-database-in-memory.md) ve akıllı sorgu [işleme](https://docs.microsoft.com/sql/relational-databases/performance/intelligent-query-processing?toc=/azure/sql-database/toc.json)gibi gelişmiş sorgu işleme özelliklerini kullanabilirsiniz. Aslında, SQL Server'ın en yeni yetenekleri önce SQL Database'e, sonra SQL Server'ın kendisine bırakılır. Milyonlarca veritabanında test edilmiş, yama veya yükseltme için ek yükü olmayan en yeni SQL Server özelliklerini elde edersiniz. 

SQL Veritabanı, iki farklı satın alma modeli nde performansı kolayca tanımlamanızı ve ölçeklendirmenizi sağlar: [vCore tabanlı satın alma modeli](sql-database-service-tiers-vcore.md) ve [DTU tabanlı satın alma modeli.](sql-database-service-tiers-dtu.md) SQL Veritabanı, yerleşik yüksek kullanılabilirlik, yedekleme ler ve diğer yaygın bakım işlemlerine sahip tam olarak yönetilen bir hizmettir. Microsoft, SQL ve işletim sistemi kodunun tüm yama larını ve güncellenmesini işler. Altta yatan altyapıyı yönetmek zorunda değilsiniz.

> [!NOTE]
> İlgili terimler ve tanımları için [SQL Veritabanı terimleri sözlüğüne](sql-database-glossary-terms.md)bakın.

## <a name="deployment-models"></a>Dağıtım modelleri

Azure SQL Veritabanı, bir Azure SQL veritabanı için aşağıdaki dağıtım seçeneklerini sunar:

![Dağıtım seçeneklerinin diyagramı](./media/sql-database-technical-overview/deployment-options.png)

- [Tek veritabanı](sql-database-single-database.md) tam olarak yönetilen, yalıtılmış bir veritabanını temsil eder. Tek bir güvenilir veri kaynağına ihtiyaç duyan modern bulut uygulamalarınız ve mikro hizmetleriniz varsa bu seçeneği kullanabilirsiniz. Tek bir [veritabanı, Microsoft SQL Server Database Engine'deki](https://docs.microsoft.com/sql/sql-server/sql-server-technical-documentation?toc=/azure/sql-database/toc.json) [bulunan veritabanına](https://docs.microsoft.com/sql/relational-databases/databases/contained-databases?toc=/azure/sql-database/toc.json) benzer.
- [Yönetilen örnek](sql-database-managed-instance.md) [Microsoft SQL Server Database Engine](https://docs.microsoft.com/sql/sql-server/sql-server-technical-documentation?toc=/azure/sql-database/toc.json)tam yönetilen bir örneğidir. Birlikte kullanılabilecek bir veritabanı kümesi içerir. Şirket içi SQL Server veritabanlarının Azure bulutuna kolayca aktarılması ve SQL Server Database Engine'in sağladığı veritabanı özelliklerini kullanması gereken uygulamalar için bu seçeneği kullanın.
- [Elastik havuz,](sql-database-elastic-pool.md) CPU veya bellek gibi paylaşılan bir kaynak kümesine sahip [tek veritabanları](sql-database-single-database.md) topluluğudur. Tek veritabanları elastik bir havuza ve dışına taşınabilir.

> [!IMPORTANT]
> SQL Veritabanı ve SQL Server arasındaki özellik farklılıklarının yanı sıra farklı Azure SQL Veritabanı dağıtım seçenekleri arasındaki farkları anlamak için [SQL Veritabanı özelliklerine](sql-database-features.md)bakın.

SQL Veritabanı, birden çok kaynak türü, hizmet katmanı ve bilgi işlem boyutlarıyla öngörülebilir performans sunar. Hiçbir kesinti, yerleşik akıllı optimizasyon, küresel ölçeklenebilirlik ve kullanılabilirlik ve gelişmiş güvenlik seçenekleri ile dinamik ölçeklenebilirlik sağlar. Bu özellikler, sanal makineleri ve altyapıyı yönetmek yerine hızlı uygulama geliştirmeye ve pazara zaman ayırmanızı hızlandırmaya odaklanmanızı sağlar. SQL Veritabanı hizmeti şu anda dünya çapında 38 veri merkezinde bulunmaktadır, böylece veritabanınızı size yakın bir veri merkezinde çalıştırabilirsiniz.

## <a name="scalable-performance-and-pools"></a>Ölçeklenebilir performans ve havuzlar

Atanan kaynak miktarını tanımlayabilirsiniz. 
- Tek veritabanları ile, her veritabanı diğerlerinden yalıtılmış ve taşınabilir. Her biri kendi garantili miktarda işlem, bellek ve depolama kaynaklarına sahiptir. Veritabanına atanan kaynakların miktarı bu veritabanına ayrılmıştır ve Azure'daki diğer veritabanlarıyla paylaşılmaz. Tek veritabanı [kaynaklarını](sql-database-single-database-scale.md) dinamik olarak yukarı ve aşağı ölçeklendirebilirsiniz. Tek veritabanı seçeneği, farklı ihtiyaçlar için farklı işlem, bellek ve depolama kaynakları sağlar. Örneğin, 1 ila 80 vCores veya 4 TB için 32 GB alabilirsiniz. Tek bir veritabanı için [hiper ölçekli hizmet katmanı,](sql-database-service-tier-hyperscale.md) hızlı yedekleme ve geri yükleme özellikleriyle 100 TB'ye ölçeklendirme yapmanızı sağlar.
- Esnek havuzlarla, havuzdaki tüm veritabanları tarafından paylaşılan kaynakları atayabilirsiniz. Yeni bir veritabanı oluşturabilir veya kaynakların kullanımını en üst düzeye çıkarmak ve paradan tasarruf etmek için varolan tek veritabanlarını kaynak havuzuna taşıyabilirsiniz. Bu seçenek aynı zamanda dinamik olarak esnek havuz kaynaklarını yukarı ve aşağı [ölçeklendirme](sql-database-elastic-pool-scale.md) olanağı sağlar.
- Yönetilen örneklerde, yönetilen her örnek, garantili kaynaklara sahip diğer örneklerden yalıtılır. Yönetilen bir örnek te, örnek veritabanları bir kaynak kümesini paylaşır. Yönetilen örnek kaynaklarını dinamik olarak yukarı ve aşağı [ölçeklendirebilirsiniz.](sql-database-managed-instance-resource-limits.md)

Genel amaçlı hizmet katmanında ilk uygulamanızı ayda düşük bir maliyetle küçük, tek bir veritabanında oluşturabilirsiniz. Daha sonra, çözümünüzün gereksinimlerini karşılamak için hizmet katmanını istediğiniz zaman el ile veya programlı olarak iş açısından kritik hizmet katmanına değiştirebilirsiniz. Performansı uygulamanız veya müşterileriniz kesinti yaşamadan ayarlayabilirsiniz. Dinamik ölçeklendirebilirlik, veritabanınızın hızla değişen kaynak gereksinimlerine saydam bir şekilde yanıt verebilmesini sağlar. Yalnızca ihtiyacınız olduğunda ihtiyacınız olan kaynakları ödlersiniz.

*Dinamik ölçeklenebilirlik* *otomatik ölçekfarklıdır.* Bir hizmet ölçütlere dayalı olarak otomatik şekilde ölçeklendirildiğinde otomatik ölçeklendirme oluşurken, dinamik ölçeklenebilirlik ise kesinti süresi olmadan el ile ölçeklendirmeye olanak sağlar. Tek veritabanı seçeneği manuel dinamik ölçeklenebilirliği destekler, ancak otomatik ölçeklendirmeyi destekler. Daha otomatik bir deneyim için, veritabanlarının tek tek veritabanı gereksinimlerine göre bir havuzdaki kaynakları paylaşmasına olanak sağlayan elastik havuzları kullanın. Başka bir seçenek, tek bir veritabanı için ölçeklenebilirliği otomatikleştirmeye yardımcı olabilecek komut dosyaları kullanmaktır. Örneğin, tek [bir veritabanını izlemek ve ölçeklendirmek için PowerShell'i kullanın'a](scripts/sql-database-monitor-and-scale-database-powershell.md)bakın.

### <a name="purchasing-models"></a>Satın alma modelleri

SQL Veritabanı aşağıdaki satın alma modellerini sunar:
- [vCore tabanlı satın alma modeli,](sql-database-service-tiers-vcore.md) vCores sayısını, bellek miktarını ve depolama miktarını ve hızını seçmenize olanak tanır. vCore tabanlı satın alma modeli, maliyet tasarrufu elde etmek [için SQL Server için Azure Karma Avantajı'nı](https://azure.microsoft.com/pricing/hybrid-benefit/) kullanmanıza da olanak tanır. Azure Karma Avantajı hakkında daha fazla bilgi için bu makalenin ilerleyen bölümlerinde "Sık sorulan sorular" bölümüne bakın.
- [DTU tabanlı satın alma modeli,](sql-database-service-tiers-dtu.md) ağır veritabanı iş yüklerine hafif desteği sağlamak için üç hizmet katmanında işlem, bellek ve G/Ç kaynaklarının bir karışımını sunar. Her katmandaki bilgi işlem boyutları, ek depolama kaynakları ekleyebileceğiniz bu kaynakların farklı bir karışımını sağlar.
- [Sunucusuz model,](sql-database-serverless.md) iş yükü talebine göre otomatik olarak işlem ölçeklendirilir ve saniyede kullanılan işlem miktarı için faturalar. Sunucusuz bilgi işlem katmanı, yalnızca depolama nın faturalandırıldığu etkin olmayan dönemlerde veritabanlarını otomatik olarak duraklatır ve etkinlik döndüğünde veritabanlarını otomatik olarak devam ettirir.

### <a name="service-tiers"></a>Hizmet katmanları

Azure SQL Veritabanı, farklı uygulama türleri için tasarlanmış üç hizmet katmanı sunar:
- [Genel Amaçlı/Standart](sql-database-service-tier-general-purpose.md) hizmet katmanı, ortak iş yükleri için tasarlanmıştır. Bütçe odaklı dengeli işlem ve depolama seçenekleri sunar.
- Yüksek işlem hızı ve en düşük gecikmeli G/Ç ile OLTP uygulamaları için tasarlanan [Business Critical/Premium](sql-database-service-tier-business-critical.md) hizmet katmanı. Birkaç yalıtılmış çoğaltma kullanarak hatalara karşı en yüksek esnekliği sunar.
- [Çok](sql-database-service-tier-hyperscale.md) büyük OLTP veritabanı ve depolamayı otomatik ölçeklendirme ve akıcı hesaplama yı ölçeklendirme yeteneği için tasarlanmış hiper ölçekli hizmet katmanı.    

### <a name="elastic-pools-to-maximize-resource-utilization"></a>Kaynak kullanımını en verimli hale getirmek için elastik havuzlar

Tek veritabanı oluşturabilmek ve veritabanı performansını isteğe göre yükseltip düşürebilmek, özellikle kullanım biçimlerinin nispeten tahmin edilebilir olduğu durumlarda birçok işletme ve uygulama için yeterlidir. Öngörülemeyen kullanım kalıpları maliyetleri ve iş modelinizi yönetmeyi zorlaştırabilir. [Elastik havuzlar](sql-database-elastic-pool.md) bu sorunu çözmek için tasarlanmıştır. Performans kaynaklarını tek bir veritabanı yerine bir havuza ayırırsınız. Tek bir veritabanı performansı yerine havuzun toplu performans kaynakları için ödeme yapabilirsiniz.

   ![Temel, standart ve premium sürümlerde elastik havuzları gösteren grafik](./media/sql-database-what-is-a-dtu/sqldb_elastic_pools.png)

Esnek havuzlarda, kaynaklara olan talep dalgalandıkça veritabanı performansını yukarı ve aşağı çevirmeye odaklanmanız gerekmez. Havuza alınan veritabanları, gerektiğinde elastik havuzun performans kaynaklarını tüketir. Havuza toplanan veritabanları tüketir, ancak havuzun sınırlarını aşmaz, bu nedenle tek tek veritabanı kullanımı olmasa bile maliyetiniz tahmin edilebilir kalır.

Veritabanıları [havuza ekleyebilir ve kaldırarak](sql-database-elastic-pool-manage-portal.md)uygulamanızı bir avuç veritabanlarından binlerce veritabanına ölçekleyebilir ve bunların hepsi de kontrol ettiğiniz bir bütçe dahilinde. Havuzdaki hiçbir veritabanının tüm havuz kaynaklarını kullanmadığından ve havuza alınan her veritabanının garantili minimum kaynak miktarına sahip olduğundan emin olmak için havuzdaki veritabanlarıiçin kullanılabilen minimum ve maksimum kaynakları da denetleyebilirsiniz. Elastik havuzlar kullanan hizmet (SaaS) uygulamaları olarak yazılım tasarım desenleri hakkında daha fazla bilgi edinmek [için, SQL Veritabanı ile çok kiracılı SaaS uygulamaları için tasarım desenleri](sql-database-design-patterns-multi-tenancy-saas-applications.md)bölümüne bakın.

Betikler, elastik havuzların izlenmesine ve ölçeklendirilmesine yardımcı olabilir. Örneğin, Azure [SQL Veritabanı'ndaki bir SQL elastik havuzunu izlemek ve ölçeklendirmek için PowerShell'i kullanın'a](scripts/sql-database-monitor-and-scale-pool-powershell.md)bakın.

> [!IMPORTANT]
> Yönetilen bir örnek elastik havuzları desteklemez. Yönetilen örnek, yönetilen örnek kaynaklarını paylaşan örnek veritabanları topluluğudur.

### <a name="blend-single-databases-with-pooled-databases"></a>Tek veritabanlarını havuza alınan veritabanlarıyla karıştırma

Tek veritabanlarını elastik havuzlarla harmanlayabilir ve durumunuza uyum sağlamak için tek veritabanlarının ve esnek havuzların servis katmanlarını değiştirebilirsiniz. Ayrıca, benzersiz uygulama tasarım gereksinimlerinizi karşılamak, maliyet ve kaynak verimliliği nizi artırmak ve yeni iş fırsatlarının kilidini açmak için diğer Azure hizmetlerini SQL Veritabanı ile karıştırıp eşleştirebilirsiniz.

## <a name="extensive-monitoring-and-alerting-capabilities"></a>Kapsamlı izleme ve uyarı özellikleri

Azure SQL Veritabanı, iş yükü özellikleri hakkında daha derin bilgiler edinmenize yardımcı olan gelişmiş izleme ve sorun giderme özellikleri sağlar. Bu özellikler ve araçlar şunlardır:
 - SQL Server Database Engine'in en son sürümü tarafından sağlanan yerleşik izleme özellikleri. Bunlar, gerçek zamanlı performans öngörülerini bulmanızı sağlar. 
 - Azure tarafından sağlanan ve çok sayıda veritabanı örneğini izlemenize ve sorun gidermenize olanak tanıyan PaaS izleme özellikleri.

Yerleşik bir SQL Server izleme özelliği olan [Query Store,](https://docs.microsoft.com/sql/relational-databases/performance/best-practice-with-the-query-store)sorgularınızın performansını gerçek zamanlı olarak kaydeder ve olası performans sorunlarını ve en iyi kaynak tüketicilerini belirlemenize olanak tanır. Otomatik ayar ve öneriler, gerileyen performans ve eksik veya yinelenen dizinlere sahip sorgularla ilgili öneriler sağlar. SQL Veritabanı'nda otomatik ayar, sorunları çözebilecek komut dosyalarını el ile uygulamanızı veya düzeltmeyi SQL Veritabanı'nın uygulamasına izin vermenizi sağlar. SQL Veritabanı ayrıca düzeltmenin bazı avantajlar sağladığını sınayabilir ve doğrulayabilir ve sonuca bağlı olarak değişikliği koruyabilir veya tersine çevirebilir. Sorgu Deposu ve otomatik ayarlama özelliklerine ek olarak, iş yükü performansını izlemek için standart [DMV'leri ve XEvent'i](sql-database-monitoring-with-dmvs.md) kullanabilirsiniz.

Azure, binlerce veritabanının durumunu izlemenize olanak tanıyan performans derecelendirmeleriyle birlikte [yerleşik performans izleme](sql-database-performance-guidance.md) ve [uyarı](sql-database-insights-alerts-portal.md) araçları sağlar. Bu araçları kullanarak, mevcut veya öngörülen performans gereksinimlerinize bağlı olarak yukarı veya aşağı ölçeklemenin etkisini hızlı bir şekilde değerlendirebilirsiniz. SQL Veritabanı ayrıca izlemeyi kolaylaştırmak için [ölçümler ve tanılama günlükleri oluşturabilir](sql-database-metrics-diag-logging.md). SQL Veritabanını kaynak kullanımını, çalışanları, oturumları ve bu Azure kaynaklarından birine yapılan bağlantıları kaydedecek şekilde yapılandırabilirsiniz:

- **Azure Depolama**: Küçük maliyetlerle çok sayıda telemetri arşivleme için.
- **Azure Etkinlik Hub'ları**: SQL Veritabanı telemetrisini özel izleme çözümünüzveya sıcak ardışık pipeline'Larınizle tümleştirmek için.
- **Azure Monitör günlükleri**: Raporlama, uyarı ve azaltma özelliklerine sahip yerleşik bir izleme çözümü için.

![Azure izleme mimarisi diyagramı](./media/sql-database-metrics-diag-logging/architecture.png)

## <a name="availability-capabilities"></a>Kullanılabilirlik özellikleri

Azure SQL Veritabanı, işletmenizin kesintiler sırasında çalışmaya devam etmesini sağlar. Geleneksel bir SQL Server ortamında, genellikle yerel olarak ayarlanmış en az iki makineniz vardır. Bu makineler, tek bir makine veya bileşenin arızalarına karşı korumak için verilerin tam, eşzamanlı olarak muhafaza edilmesine sahiptir. Bu ortam yüksek kullanılabilirlik sağlar, ancak veri merkezinizi yok olan doğal bir felakete karşı koruma sağlamaz.

Olağanüstü durum kurtarma, yıkıcı bir olayın coğrafi olarak başka bir makineye veya verilerinizin bir kopyasına sahip makinelere sahip olacak kadar yerelleştirilmeye sahip olduğunu varsayar. SQL Server'da, bu yeteneği elde etmek için her zaman kullanılabilirlik gruplarında async modunda çalışır durumdasınız. İnsanlar genellikle bir işlem gerçekleştirmeden önce çoğaltmanın bu kadar uzaklarda gerçekleşmesini beklemek istemezler, bu nedenle planlanmamış arızalar yaptığınızda veri kaybı potansiyeli vardır.

Premium ve iş açısından kritik hizmet katmanlarındaki veritabanları zaten kullanılabilirlik grubunun eşitlenmesine [benzer bir şey yapar.](sql-database-high-availability.md#premium-and-business-critical-service-tier-availability) Alt hizmet katmanlarındaki [veritabanları, farklı ama eşdeğer](sql-database-high-availability.md#basic-standard-and-general-purpose-service-tier-availability)bir mekanizma kullanarak depolama yoluyla artıklık sağlar. Yerleşik mantık, tek bir makine arızasına karşı korumaya yardımcı olur. Etkin coğrafi çoğaltma özelliği, tüm bölgenin yok edildiği felaketlere karşı koruma olanağı sağlar.

Azure Kullanılabilirlik Bölgeleri, tek bir bölge içindeki tek bir veri merkezi binasının kesintisine karşı koruma sağlamaya çalışır. Bir binaya güç veya ağ kaybına karşı korumanıza yardımcı olur. SQL Veritabanı'nda, farklı yinelemeleri farklı kullanılabilirlik bölgelerine (farklı binalar, etkili bir şekilde) yerlikir.

Aslında, Microsoft tarafından yönetilen veri merkezlerinden oluşan küresel bir ağ tarafından desteklenen Azure'un hizmet düzeyi sözleşmesi [(SLA),](https://azure.microsoft.com/support/legal/sla/) uygulamanızın 7/24 çalışmasını sağlar. Azure platformu her veritabanını tam olarak yönetir ve veri kaybı ve yüksek veri kullanılabilirliği yüzdesi garanti eder. Azure, yama, yedekleme, çoğaltma, hata algılama, altta yatan olası donanım, yazılım veya ağ hataları, hata düzeltmeleri, başarısız lıklar, veritabanı yükseltmeleri ve diğer bakım görevlerini otomatik olarak işler. Standart kullanılabilirlik düzeyine ulaşmak için işlem ve depolama katmanları ayrılır. Premium kullanılabilirlik, performans için tek bir düğümüzerinde bilgi işlem ve depolama yı entegre ederek ve ardından Her Zaman Kullanılabilirlik Grupları'na benzer teknolojiyi uygulayarak elde edilir. Azure SQL Veritabanı'nın yüksek kullanılabilirlik özelliklerinin tam olarak tartışılması için [SQL Veritabanı kullanılabilirliği](sql-database-high-availability.md)bölümüne bakın. 

Buna ek olarak, SQL Veritabanı yerleşik [iş sürekliliği ve küresel ölçeklenebilirlik](sql-database-business-continuity.md) özellikleri sağlar. Bunlar:

- [Otomatik yedeklemeler](sql-database-automated-backups.md):

  SQL Veritabanı, sql veritabanlarının tam, diferansiyel ve işlem günlüğü yedeklemelerini otomatik olarak gerçekleştirir ve herhangi bir noktaya geri yüklemenizi sağlar. Tek veritabanları ve havuzlu veritabanları için, uzun süreli yedekleme bekletme için tam veritabanı yedeklemelerini Azure Depolama'da depolamak üzere SQL Veritabanı'nı yapılandırabilirsiniz. Yönetilen örnekler için, uzun süreli yedekleme bekletme için yalnızca kopya yedeklemeleri de gerçekleştirebilirsiniz.

- [Nokta-in-time geri yüklemeleri:](sql-database-recovery-using-backups.md)

  Tüm SQL Veritabanı dağıtım seçenekleri, herhangi bir SQL veritabanı için otomatik yedekleme bekletme süresi içinde herhangi bir noktaya kurtarma yı destekler.
- [Etkin coğrafi çoğaltma:](sql-database-active-geo-replication.md)

  Tek veritabanı ve havuza alınan veritabanları seçenekleri, aynı veya genel olarak dağıtılmış Azure veri merkezlerinde en fazla dört okunabilir ikincil veritabanını yapılandırmanıza olanak tanır. Örneğin, eşzamanlı okuma işlemleri yüksek hacimli bir katalog veritabanına sahip bir SaaS uygulamanız varsa, genel okuma ölçeğini etkinleştirmek için etkin coğrafi çoğaltmayı kullanın. Bu, okunan iş yüklerinden kaynaklanan birincil alandaki darboğazları giderir. Yönetilen örnekler için otomatik hata grupları kullanın.
- [Otomatik arıza grupları:](sql-database-auto-failover-group.md)

  Tüm SQL Veritabanı dağıtım seçenekleri, genel ölçekte yüksek kullanılabilirlik ve yük dengelemesi sağlamak için başarısız grupları kullanmanıza olanak tanır. Bu, saydam coğrafi çoğaltma ve büyük veritabanları kümelerinin, elastik havuzların ve yönetilen örneklerin başarısız lığı içerir. Failover grupları, en az yönetim yüküyle küresel olarak dağıtılan SaaS uygulamalarının oluşturulmasına olanak tanır. Bu, tüm karmaşık izleme, yönlendirme ve başarısız düzenlemeyi SQL Veritabanı'na bırakır.
- [Bölge yedekli veritabanları:](sql-database-high-availability.md)

  SQL Veritabanı, birden çok kullanılabilirlik bölgesinde premium veya iş açısından kritik veritabanları veya esnek havuzlar sağlamanıza olanak tanır. Bu veritabanları ve elastik havuzlar yüksek kullanılabilirlik için birden çok yedek kopyaları olduğundan, bu yinelemeleri birden çok kullanılabilirlik bölgesine yerleştirmek daha yüksek esneklik sağlar. Bu, veri merkezi ölçek hatalarından veri kaybı olmadan otomatik olarak kurtarma olanağını içerir.

## <a name="built-in-intelligence"></a>Yerleşik zeka

SQL Database ile, veritabanlarını çalıştırma ve yönetme maliyetlerini önemli ölçüde azaltmanıza yardımcı olan ve uygulamanızın hem performansını hem de güvenliğini en üst düzeye çıkaran yerleşik zekaya sahip olursunuz. Sql Veritabanı, her gün milyonlarca müşteri iş yükünü çalıştırArak, büyük miktarda telemetri verisi toplar ve işlerken, aynı zamanda müşteri gizliliğine de tam olarak saygı duyar. Çeşitli algoritmalar, telemetri verilerini sürekli olarak değerlendirir, böylece hizmet inuygulamanıza uyum sağlayabilir ve öğrenebilir.

### <a name="automatic-performance-monitoring-and-tuning"></a>Otomatik performans izleme ve ayarlama

SQL Veritabanı izlemeniz gereken sorgularla ilgili ayrıntılı öngörüler sunar. SQL Veritabanı veritabanı desenlerinizi öğrenir ve veritabanı şemanızı iş yükünüze uyarlamanızı sağlar. SQL Veritabanı, [performans ayarlama önerilerinde](sql-database-advisor.md) bulunur. Siz de bu ayarları gözden geçirebilir ve uygulayabilirsiniz.

Ancak, bir veritabanını sürekli olarak izlemek, özellikle birçok veritabanlarıyla uğraşırken zor ve sıkıcı bir görevdir. [Intelligent Insights,](sql-database-intelligent-insights.md) SQL Veritabanı performansını ölçekte otomatik olarak izleyerek bu işi sizin yeriniz için yapar. Performans bozulması sorunları hakkında sizi bilgilendirir, her sorunun temel nedenini tanımlar ve mümkün olduğunda performans geliştirme önerileri sağlar.

SQL Database ve Azure'un sağladığı tüm kullanılabilir araçlar ve raporlarla bile çok sayıda veritabanlarını yönetmek verimli bir şekilde yapmak imkansız olabilir. Veritabanınızı el ile izlemek ve ayarlamak yerine, [otomatik ayar](sql-database-automatic-tuning.md)kullanarak bazı izleme ve ayar eylemlerini SQL Veritabanı'na atayabilirsiniz. SQL Veritabanı, performansın sürekli olarak iyileştirilmesinden emin olmak için önerileri otomatik olarak uygular, testler ve ayar eylemlerinin her birini doğrular. Bu şekilde, SQL Veritabanı otomatik olarak kontrollü ve güvenli bir şekilde iş yükünün ize uyarlar. Otomatik ayar, veritabanınızın performansının dikkatle izlendiği ve her ayar eyleminden önce ve sonra karşılaştırıldığı anlamına gelir. Performans iyileşmezse, tuning eylemi geri döndürülz.

SQL Veritabanı'nın üstünde [SaaS çok kiracılı uygulamaları](sql-database-design-patterns-multi-tenancy-saas-applications.md) çalıştıran iş ortaklarımızın çoğu, uygulamalarının her zaman istikrarlı ve öngörülebilir performansa sahip olduğundan emin olmak için otomatik performans ayarı'na güveniyor. İş ortaklarımız bu özelliğin gecenin ortasında performans sorunu yaşama riskini önemli ölçüde azalttığını söylüyor. Buna ek olarak, müşteri tabanının bir bölümü sql server da kullandığından, SQL Server müşterilerine yardımcı olmak için SQL Database tarafından sağlanan aynı dizin oluşturma önerilerini kullanıyorlar.

[SQL Veritabanı'nda](sql-database-automatic-tuning.md)iki otomatik ayar yönü mevcuttur:

- **Otomatik dizin yönetimi**: Veritabanınıza eklenmesi ve veritabanınızdan kaldırılması gereken dizinleri tanımlar.
- **Otomatik plan düzeltmesi**: Sorunlu planları tanımlar ve SQL planı performans sorunlarını giderir.

### <a name="adaptive-query-processing"></a>Uyarlamalı sorgu işleme

Çok ekstre tablo değerli işlevler, toplu iş modu bellek hibe geri bildirimi ve toplu iş modu uyarlamalı birleştirmeler için ara sıra yürütme de dahil olmak üzere [uyarlanabilir sorgu işleme](/sql/relational-databases/performance/intelligent-query-processing)kullanabilirsiniz. Bu uyarlanabilir sorgu işleme özelliklerinin her biri benzer "öğren ve uyarlama" tekniklerini uygular ve geçmiş olarak zor layıcı sorgu optimizasyonu sorunlarıyla ilgili performans sorunlarını daha da ele almaya yardımcı olur.

## <a name="advanced-security-and-compliance"></a>Gelişmiş koruma ve uyumluluk

SQL Veritabanı, uygulamanızın çeşitli güvenlik ve uyumluluk gereksinimlerine uygun olmasına yardımcı olmak için bir dizi [yerleşik güvenlik ve uyum özelliğine](sql-database-security-overview.md) sahiptir.

> [!IMPORTANT]
> Microsoft, azure SQL Veritabanı'nı (tüm dağıtım seçenekleri) bir dizi uyumluluk standardına göre onaylamıştır. Daha fazla bilgi için, SQL Veritabanı uyumluluk sertifikalarının en güncel listesini bulabileceğiniz [Microsoft Azure Güven Merkezi'ne](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942)bakın.

### <a name="advance-threat-protection"></a>Önceden tehdit koruması

Gelişmiş veri güvenliği, gelişmiş SQL güvenlik yetenekleri için birleşik bir pakettir. Hassas verileri bulma ve sınıflandırma, veritabanı güvenlik açıklarınızı yönetme ve veritabanınız için bir tehdit oluşturabilecek anormal etkinlikleri algılama işlevlerini içerir. Bu yetenekleri etkinleştirmek ve yönetmek için tek bir konum sağlar.

- [Veri bulma ve sınıflandırma:](sql-database-data-discovery-and-classification.md)

  Bu özellik, veritabanlarınızdaki hassas verileri keşfetmek, sınıflandırmak, etiketlemek ve korumak için Azure SQL Veritabanı'nda yerleşik özellikler sağlar. Veritabanı sınıflandırma durumunuza görünürlük sağlar ve veritabanı içindeki ve sınırlarının ötesindeki hassas verilere erişimi izler.
- [Güvenlik açığı değerlendirmesi](sql-vulnerability-assessment.md):

  Bu hizmet, olası veritabanı açıklarını keşfedebilir, izleyebilir ve düzeltmenize yardımcı olabilir. Güvenlik durumunuz hakkında görünürlük sağlamasının yanı sıra güvenlik sorunlarınızı çözmek ve veritabanı güçlendirmelerinizi geliştirmek için eyleme dönüştürülebilir adımlar sunar.
- [Tehdit algılama](sql-database-threat-detection.md):

  Bu özellik, veritabanınıza erişmek veya bunlardan yararlanmak için olağandışı ve zararlı olabilecek girişimleri gösteren anormal etkinlikleri algılar. Veritabanınızı şüpheli etkinliklere karşı sürekli izler ve olası güvenlik açıkları, SQL ekleme saldırıları ve anormal veritabanı erişim modelleri hakkında anında güvenlik uyarıları sunar. Tehdit algılama uyarıları şüpheli etkinliğin ayrıntılarını sağlar ve tehdidin nasıl soruşturulup azaltınaaçıklanması konusunda eylem önerir.

### <a name="auditing-for-compliance-and-security"></a>Uyumluluk ve güvenlik denetimi

[Denetim](sql-database-auditing.md) veritabanı olaylarını izler ve bunları Azure depolama hesabınızdaki bir denetim günlüğüne yazar. Denetim, mevzuata uygunluğu korumanıza, veritabanı etkinliğini anlamanıza ve iş kaygıları veya şüpheli güvenlik ihlallerini gösterebilecek tutarsızlıklar ve anormallikler hakkında bilgi edinmenize yardımcı olabilir.

### <a name="data-encryption"></a>Veri şifrelemesi

SQL Veritabanı şifreleme sağlayarak verilerinizin güvenliğini sağlamaya yardımcı olur. Hareket halindeki veriler için [aktarım katmanı güvenliğini](https://support.microsoft.com/kb/3135244)kullanır. Veri istirahati için [saydam veri şifrelemesi](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql)kullanır. Kullanılan veriler için, [her zaman şifreli](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine)kullanır.

### <a name="azure-active-directory-integration-and-multi-factor-authentication"></a>Azure Active Directory tümleştirmesi ve çok faktörlü kimlik doğrulaması

SQL Veritabanı, [Azure Active Directory tümleştirmesi](sql-database-aad-authentication.md) ile veritabanı kullanıcısı ve diğer Microsoft hizmetleri kimliklerini bir merkezden yönetmenizi sağlar. Bu özellik, izin yönetimini kolaylaştırırken güvenliği artırır. Azure Active Directory, tek bir oturum açma işlemini desteklerken veri ve uygulama güvenliğini artırmak için [çok faktörlü kimlik doğrulamayı](sql-database-ssms-mfa-authentication.md) destekler.

## <a name="easy-to-use-tools"></a>Kullanımı kolay araçlar

SQL Veritabanı uygulama oluşturma ve uygulamaların bakımını yapma işlemlerinin daha kolay ve daha verimli şekilde yapılmasını sağlar. SQL Veritabanı size, en iyi yaptığınız işe; mükemmel uygulamalar oluşturmaya odaklanma seçeneği sunar. Sql Veritabanı'nda zaten sahip olduğunuz araçları ve becerileri kullanarak yönetebilir ve geliştirebilirsiniz.

- [Azure portalı:](https://portal.azure.com/)

  Tüm Azure hizmetlerini yönetmek için web tabanlı bir uygulama.
- [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms):

  SQL Server'dan SQL Database'e kadar herhangi bir SQL altyapısını yönetmek için ücretsiz, indirilebilir istemci uygulaması.
- [Visual Studio'da SQL Server Veri Araçları](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt):

  SQL Server ilişkisel veritabanları, SQL veritabanları, Entegrasyon Hizmetleri paketleri, Analiz Hizmetleri veri modelleri ve Raporlama Hizmetleri raporları geliştirmek için ücretsiz, indirilebilir istemci uygulaması.
- [Görsel Stüdyo Kodu](https://code.visualstudio.com/docs):

  Windows, macOS ve Linux için ücretsiz, indirilebilir, açık kaynak kod düzenleyicisi. Microsoft SQL Server, Azure SQL Veritabanı ve Azure SQL Veri Ambarı sorgulamaiçin [mssql uzantısı](https://aka.ms/mssql-marketplace) da dahil olmak üzere uzantıları destekler.

SQL Veritabanı, macOS, Linux ve Windows'da Python, Java, Node.js, PHP, Ruby ve .NET ile uygulama oluşturmayı destekler. SQL Veritabanı, SQL Server ile aynı [bağlantı kitaplıklarını destekler](sql-database-libraries.md).

[!INCLUDE [sql-database-create-manage-portal](includes/sql-database-create-manage-portal.md)]

## <a name="sql-database-frequently-asked-questions"></a>SQL Veritabanı sık sorulan sorular

### <a name="what-is-the-current-version-of-sql-database"></a>SQL Veritabanı'nın geçerli sürümü nedir?

SQL Veritabanı'nın geçerli sürümü V12'dir. Sürüm V11 kullanımdan kaldırıldı.

### <a name="can-i-control-when-patching-downtime-occurs"></a>Yama kapalı kalma süresi oluştuğunda kontrol edebilir miyim?

Hayır. Uygulamanızda [yeniden deneme mantığı kullanırsanız,](sql-database-develop-overview.md#resiliency) yama nın etkisi genellikle fark edilmez. Daha fazla bilgi için Azure [SQL Veritabanı'nda Azure bakım etkinlikleri için Planlama'ya](sql-database-planned-maintenance.md)bakın.

### <a name="azure-hybrid-benefit-questions"></a>Azure Karma Avantaj soruları

#### <a name="are-there-dual-use-rights-with-azure-hybrid-benefit-for-sql-server"></a>SQL Server için Azure Karma Avantajı ile çift kullanım hakları var mı?

Geçişlerin sorunsuz bir şekilde çalışmasını sağlamak için lisansın 180 günlük çift kullanım hakkına sahipsiniz. Bu 180 günlük süreden sonra, SQL Server lisansını yalnızca SQL Veritabanı'nda bulutta kullanabilirsiniz. Artık şirket içinde ve bulutta çift kullanım hakkınız yok.

#### <a name="how-does-azure-hybrid-benefit-for-sql-server-differ-from-license-mobility"></a>SQL Server için Azure Karma Avantajı lisans taşınabilirliğinden nasıl farklıdır?

Yazılım Güvencesi ile SQL Server müşterilerine lisans mobilitesi avantajları sunuyoruz. Bu, lisanslarının bir ortağın paylaşılan sunucularına yeniden atanmasına olanak tanır. Bu avantajı Azure IaaS ve AWS EC2'de kullanabilirsiniz.

SQL Server için Azure Karma Avantajı, iki temel alanda lisans taşınabilirliğinden farklıdır:

- Yüksek oranda sanallaştırılmış iş yüklerini Azure'a taşımak için ekonomik avantajlar sağlar. SQL Server Enterprise Edition müşterileri, yüksek sanallaştırılmış uygulamalar için şirket içinde sahip oldukları her çekirdek için Genel Amaçlı SKU'da Azure'da dört çekirdek edinebilirler. Lisans hareketliliği, sanallaştırılmış iş yüklerini buluta taşımak için özel maliyet avantajlarına izin vermez.
- Azure'da (SQL Database yönetilen örnek) bir PaaS hedefi sağlar ve bu hedef, şirket içinde SQL Server ile son derece uyumludur.

#### <a name="what-are-the-specific-rights-of-the-azure-hybrid-benefit-for-sql-server"></a>SQL Server için Azure Karma Avantajı'nın özel hakları nelerdir?

SQL Veritabanı müşterileri, SQL Server için Azure Karma Avantajı ile ilişkili aşağıdaki haklara sahiptir:

|Lisans ayak izi|SQL Server için Azure Hybrid Benefit size ne alır?|
|---|---|
|SA ile SQL Server Enterprise Edition çekirdek müşterileri|<li>Genel Amaç veya İş Kritik SKU'da taban ücret ödeyebilirsiniz</li><br><li>1 çekirdek şirket içi = Genel Amaçlı SKU 4 çekirdek</li><br><li>1 çekirdek şirket içi = İş Kritik SKU 1 çekirdek</li>|
|SA ile SQL Server Standard Edition çekirdek müşterileri|<li>Sadece Genel Amaçlı SKU taban oranı ödeyebilirsiniz</li><br><li>1 çekirdek şirket içi = Genel Amaçlı SKU 1 çekirdek</li>|
|||

## <a name="engage-with-the-sql-server-engineering-team"></a>SQL Server mühendislik ekibi ile iletişime geçme

- [DBA Stack Exchange](https://dba.stackexchange.com/questions/tagged/sql-server): Veritabanı yönetimine sorular sorun.
- [Yığın Taşma](https://stackoverflow.com/questions/tagged/sql-server): Geliştirme soruları sorun.
- [MSDN Forumları](https://social.msdn.microsoft.com/Forums/home?category=sqlserver): Teknik sorular sorun.
- [Geri bildirim](https://aka.ms/sqlfeedback): Hataları bildirin ve özellik isteyin.
- [Reddit](https://www.reddit.com/r/SQLServer/): SQL Server'ı tartışın.

## <a name="next-steps"></a>Sonraki adımlar

- Tek veritabanları ve esnek havuzlarla ilgili maliyet karşılaştırmaları ve hesap makineleri için [fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/sql-database/) bakın.
- Başlamak için bu hızlı başlangıçlara bakın:

  - [Azure portalda SQL veritabanı oluşturma](sql-database-single-database-get-started.md)  
  - [Azure CLI ile SQL veritabanı oluşturma](sql-database-get-started-cli.md)
  - [PowerShell ile SQL veritabanı oluşturma](sql-database-get-started-powershell.md)

- Azure CLI ve PowerShell örnekleri için bkz.:
  - [SQL Veritabanı için Azure CLI örnekleri](sql-database-cli-samples.md)
  - [SQL Veritabanı için Azure PowerShell örnekleri](sql-database-powershell-samples.md)

- Yeni özellikler duyurulduğu gibi bilgi için [SQL Veritabanı için Azure Yol Haritası'na](https://azure.microsoft.com/roadmap/?category=databases)bakın.
- SQL Server ürün ekibi üyelerinin SQL Veritabanı haberleri ve özellikleri hakkında blog attığı [Azure SQL Veritabanı bloguna](https://azure.microsoft.com/blog/topics/database)bakın.

