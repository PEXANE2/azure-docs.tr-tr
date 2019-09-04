---
title: Azure SQL Veritabanı hizmeti nedir? | Microsoft Docs
description: "SQL Database ile tanışın: Microsoft'un bulut üzerindeki ilişkisel veritabanı yönetim sistemine (RDBMS) ilişkin teknik ayrıntılar ve özellikler."
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
ms.openlocfilehash: 8c931521e77123844817dff0e9209f8f95f7ad59
ms.sourcegitcommit: 32242bf7144c98a7d357712e75b1aefcf93a40cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70279893"
---
# <a name="what-is-azure-sql-database-service"></a>Azure SQL veritabanı hizmeti nedir?

Azure SQL veritabanı, Microsoft Azure buluttaki uygulamalar ve çözümler için yüksek düzeyde kullanılabilir ve yüksek performanslı bir veri depolama katmanı oluşturmanızı sağlayan genel amaçlı ilişkisel veritabanı tarafından yönetilen bir hizmettir. SQL veritabanı, hem ilişkisel verileri hem de grafik, JSON, uzamsal ve XML gibi [ilişkisel olmayan yapıları](sql-database-multi-model-features.md) işlemek için güçlü işlevler kullanmanıza olanak sağladığından, çeşitli modern bulut uygulamaları için doğru seçim olabilir. [Microsoft SQL Server veritabanı altyapısının](https://docs.microsoft.com/sql/sql-server/sql-server-technical-documentation?toc=/azure/sql-database/toc.json) en son kararlı sürümünü temel alır ve [yüksek performanslı bellek Içi teknolojiler](sql-database-in-memory.md) ve [akıllı sorgu işleme gibi zengin bir Gelişmiş sorgu işleme özelliği kullanmanızı sağlar ](https://docs.microsoft.com/sql/relational-databases/performance/intelligent-query-processing?toc=/azure/sql-database/toc.json).
Microsoft'un bulut öncelikli stratejisi kapsamında en yeni SQL Server özellikleri önce SQL Veritabanı'na, ardından da SQL Server'ın kendine uygulanır. Bu yaklaşım sayesinde düzeltme veya yükseltme zahmetine girmeden milyonlarca veritabanında test edilmiş en yeni SQL Server özelliklerine sahip olabilirsiniz. SQL veritabanı iki farklı satın alma modelinde performansı kolayca tanımlamanızı ve ölçeklendirmenizi sağlar: [sanal çekirdek tabanlı satın alma modeli](sql-database-service-tiers-vcore.md) ve [DTU tabanlı satın alma modeli](sql-database-service-tiers-dtu.md). SQL veritabanı, yerleşik yüksek kullanılabilirlik, yedeklemeler ve diğer yaygın bakım işlemlerine sahip olan tam olarak yönetilen bir hizmettir. Microsoft, SQL ve işletim sistemi kodunun tüm düzeltme ve güncelleştirme sürümlerini sorunsuz bir şekilde işler ve temel alınan altyapının tüm yönetimini uygular.

> [!NOTE]
> Azure SQL veritabanı 'nda terimler sözlüğü için bkz. [SQL veritabanı terimleri sözlüğü](sql-database-glossary-terms.md)

Azure SQL Veritabanı, bir Azure SQL veritabanı için aşağıdaki dağıtım seçeneklerini sunar:

![dağıtım seçenekleri](./media/sql-database-technical-overview/deployment-options.png)

- [Tek veritabanı](sql-database-single-database.md) , tek bir güvenilir veri kaynağı gerektiren modern bulut uygulamaları ve mikro hizmetler için kusursuz seçim içeren, tam olarak yönetilen yalıtılmış bir veritabanını temsil eder. Tek bir veritabanı, [Microsoft SQL Server veritabanı altyapısında](https://docs.microsoft.com/sql/sql-server/sql-server-technical-documentation?toc=/azure/sql-database/toc.json) [Kapsanan veritabanına](https://docs.microsoft.com/sql/relational-databases/databases/contained-databases?toc=/azure/sql-database/toc.json) benzerdir.
- [Yönetilen örnek](sql-database-managed-instance.md) , birlikte kullanılabilecek bir dizi veritabanını içeren [Microsoft SQL Server veritabanı altyapısının](https://docs.microsoft.com/sql/sql-server/sql-server-technical-documentation?toc=/azure/sql-database/toc.json) tam olarak yönetilen bir örneğidir. Şirket içi SQL Server veritabanlarının Azure bulutuna kolayca geçirilmesi ve SQL Server veritabanı altyapısının sağladığı güçlü veritabanı özelliklerinden yararlanmasını gerektiren uygulamalar için mükemmel bir seçenektir.
- [Elastik havuz](sql-database-elastic-pool.md) , CPU veya bellek gibi paylaşılan bir kaynak kümesiyle [tek bir veritabanı](sql-database-single-database.md) koleksiyonudur. Tek veritabanları, elastik bir havuzun içine ve dışına taşınabilir.

> [!IMPORTANT]
> SQL veritabanı ile SQL Server arasındaki özellik farklarını ve farklı Azure SQL veritabanı dağıtım seçenekleri arasındaki farkları anlamak için bkz. [SQL özellikleri](sql-database-features.md).

SQL veritabanı, etkin olmayan, yerleşik akıllı iyileştirme, küresel ölçeklenebilirlik ve kullanılabilirlik ve gelişmiş güvenlik işlemleri olmadan dinamik ölçeklenebilirlik sağlayan birden çok kaynak türü, hizmet katmanı ve işlem boyutu sayesinde öngörülebilir performans sunar Seçenekler — hepsi sıfırdan fazla yönetim. Bu özellikler sayesinde değerli zamanınızı ve kaynaklarınızı sanal makine ve altyapı yönetimi yerine hızlı uygulama geliştirmeye ve piyasaya sunma sürenizi kısaltmaya ayırabilirsiniz. SQL Veritabanı şu anda dünyanın farklı yerlerindeki toplam 38 veri merkezini kullanmaktadır ve düzenli aralıklarla eklenen yeni veri merkezleri, veritabanınızı bulunduğunuz konuma yakın bir veri merkezinde çalıştırmanızı sağlar.

## <a name="scalable-performance-and-pools"></a>Ölçeklenebilir performans ve havuzlar

SQL veritabanının tüm türleri, atanacak kaynak miktarını tanımlamanızı sağlar. 
- Tek veritabanları ile her bir veritabanı birbirleriyle ve taşınabilir, her biri kendi garantili işlem, bellek ve depolama kaynakları miktarına sahip ve taşınabilir. Veritabanına atanan kaynakların miktarı söz konusu veritabanına ayrılmıştır ve Azure bulutundaki diğer veritabanlarıyla paylaşılmaz. Ayrıca, [tek veritabanı kaynaklarını](sql-database-single-database-scale.md) yukarı ve aşağı dinamik olarak ölçeklendirmenize olanak tanır. Tek Veritabanı, 1 ile 80 sanal çekirdekten, 32GB'A 4 TB 'a kadar değişen farklı gereksinimlere yönelik farklı işlem, bellek ve depolama kaynakları sağlar. Tek veritabanı için [hiperscale hizmet katmanı](sql-database-service-tier-hyperscale.md) , hızlı yedekleme ve geri yükleme özellikleri Ile 100 TB 'ye ölçeklenmenize olanak sağlar.
- Elastik havuzlarla, havuzdaki tüm veritabanları tarafından paylaşılacak kaynakları atayabilirsiniz. Kaynak kullanımını en üst düzeye çıkarmak ve tasarruf etmek ve [Esnek havuz kaynaklarını](sql-database-elastic-pool-scale.md) yukarı ve aşağı dinamik olarak ölçeklendirmek için yeni bir veritabanı oluşturabilir veya var olan tek veritabanlarını bir kaynak havuzuna taşıyabilirsiniz.
- Yönetilen örneklerle, her yönetilen örnek garantili kaynaklarla diğer örneklerden yalıtılmıştır. Yönetilen bir örnek içinde, örnek veritabanları bir kaynak kümesini paylaşır ve [yönetilen örnek kaynaklarını dinamik olarak ölçeklendirmenize](sql-database-managed-instance-resource-limits.md) olanak tanır.

Genel amaçlı hizmet katmanında bir aylık düşük maliyetli küçük, tek bir veritabanında ilk uygulamanızı oluşturabilir ve ardından hizmet katmanını çözümünüzün ihtiyaçlarını karşılamak için iş açısından kritik hizmet katmanına el ile veya programlama yoluyla dilediğiniz zaman değiştirebilirsiniz. Performansı uygulamanız veya müşterileriniz kesinti yaşamadan ayarlayabilirsiniz. Dinamik ölçeklenebilirlik, veritabanınızın hızla değişen kaynak gereksinimlerine hızlı şekilde yanıt vermesini ve yalnızca ihtiyaç duyduğunuz kaynaklara ve ihtiyaç duyduğunuz süre boyunca ödeme yapmanızı sağlar.

Dinamik ölçeklenebilirlik, otomatik ölçeklendirmeden farklıdır. Bir hizmet ölçütlere dayalı olarak otomatik şekilde ölçeklendirildiğinde otomatik ölçeklendirme oluşurken, dinamik ölçeklenebilirlik ise kesinti süresi olmadan el ile ölçeklendirmeye olanak sağlar. Tek bir veritabanı el ile dinamik ölçeklenebilirliği destekler, ancak otomatik ölçeklendirme gerektirmez. Daha *otomatik* bir deneyim için, veritabanlarının tek tek veritabanı gereksinimlerine göre bir havuzdaki kaynakları paylaşmasına olanak sağlayan elastik havuzları kullanın. Ancak, tek bir veritabanı için ölçeklenebilirliği otomatik hale getirmeye yardımcı olabilecek betikler vardır. Bir örnek için bkz. [tek bir veritabanını izlemek ve ölçeklendirmek Için PowerShell 'ı kullanma](scripts/sql-database-monitor-and-scale-database-powershell.md).

### <a name="purchasing-models-service-tiers-compute-sizes-and-storage-amounts"></a>Modeller, hizmet katmanları, işlem boyutları ve depolama tutarları satın alma

SQL veritabanı, iki satın alma modeli sunar:
- [Sanal çekirdek tabanlı satın alma modeli](sql-database-service-tiers-vcore.md) , sanal çekirdek sayısını, miktarı veya belleği ve depolamanın miktarını ve hızını seçmenize olanak sağlar. Sanal çekirdek tabanlı satın alma modeli, maliyet tasarrufu kazanmak için [SQL Server Azure hibrit avantajı](https://azure.microsoft.com/pricing/hybrid-benefit/) kullanmanıza da olanak tanır. Azure Hibrit Avantajı hakkında daha fazla bilgi için bkz. [sık sorulan sorular](#sql-database-frequently-asked-questions-faq).
- [DTU tabanlı satın alma modeli](sql-database-service-tiers-dtu.md) , hafif ve ağır veritabanı iş yüklerini desteklemek için üç hizmet katmanında işlem, bellek, GÇ kaynakları Blend sağlar. Her katmandaki işlem boyutları bu kaynakların farklı bir karışımını sağlar ve bu da ek depolama kaynakları ekleyebilirsiniz.

### <a name="elastic-pools-to-maximize-resource-utilization"></a>Kaynak kullanımını en verimli hale getirmek için elastik havuzlar

Tek veritabanı oluşturabilmek ve veritabanı performansını isteğe göre yükseltip düşürebilmek, özellikle kullanım biçimlerinin nispeten tahmin edilebilir olduğu durumlarda birçok işletme ve uygulama için yeterlidir. Ancak tahmin edilemeyen kullanım biçimlerine sahipseniz bu durum maliyetlerin ve iş modelinizin yönetimini zorlaştırabilir. [Elastik havuzlar](sql-database-elastic-pool.md) bu sorunu çözmek için tasarlanmıştır. Esnek havuzların işleyiş mantığı gayet basittir. Performans kaynaklarını tek bir veritabanı yerine bir havuza ayırır ve tek veritabanı performansı yerine havuzun ortak performans kaynakları için ödeme yaparsınız.

   ![elastik havuzlar](./media/sql-database-what-is-a-dtu/sqldb_elastic_pools.png)

Elastik havuzlar sayesinde kaynak talebindeki dalgalanmalara ayak uydurmak için veritabanı performansında sürekli ayarlama yapmanız gerekmez. Havuza alınan veritabanları, gerektiğinde elastik havuzun performans kaynaklarını tüketir. Havuza alınan veritabanları havuzu kullanır ancak havuz sınırlarını aşmaz, böylece veritabanı kullanımınız tahmin edilebilir olmasa bile maliyetleriniz için durum tam tersidir. Ayrıca kontrolü sizin elinizde olan bir bütçe dahilinde [havuza veritabanı ekleme ve havuzdan veritabanı kaldırma](sql-database-elastic-pool-manage-portal.md) işlemlerini gerçekleştirebilir ve uygulamanızı birkaç veritabanından tutun binlerce veritabanına ölçeklendirebilirsiniz. Havuzdaki veritabanlarına sunulan kaynakların alt ve üst sınırlarını denetleyerek havuzdaki veritabanlarının havuzdaki tüm kaynakları kullanmasını önleyebilir ve havuza alınan tüm veritabanlarının kaynaklardan pay almasını garanti edebilirsiniz. Esnek havuzları kullanan SaaS uygulamalarının tasarım desenleri hakkında daha fazla bilgi edinmek için bkz. [SQL Veritabanı kullanan Çok Kiracılı SaaS Uygulamaları için Tasarım Desenleri](sql-database-design-patterns-multi-tenancy-saas-applications.md).

Betikler, elastik havuzların izlenmesine ve ölçeklendirilmesine yardımcı olabilir. Örnek için bkz. [PowerShell kullanarak Azure SQL Veritabanında bir SQL elastik havuzunu izleme ve ölçeklendirme](scripts/sql-database-monitor-and-scale-pool-powershell.md)

> [!IMPORTANT]
> Yönetilen bir örnek elastik havuzları desteklemez. Bunun yerine, yönetilen örnek, yönetilen örnek kaynaklarını paylaşan örnek veritabanlarının bir koleksiyonudur.

### <a name="blend-single-databases-with-pooled-databases"></a>Tek veritabanlarını havuza alınan veritabanlarıyla karıştırma

Tek veritabanlarını elastik havuzlarla karıştırabilirsiniz ve tek veritabanlarının ve elastik havuzların hizmet katmanlarını hızla ve kolayca durumunuza uyarlayabilirsiniz. Azure'un benzersiz gücü ve erişim özellikleri sayesinde benzersiz uygulama tasarımı ihtiyaçlarınızı karşılamak, maliyet ve kaynak verimliliği sağlamak ve yeni iş fırsatlarını yakalamak amacıyla diğer Azure hizmetlerini SQL Veritabanı ile birleştirebilir ve eşleştirebilirsiniz.

## <a name="extensive-monitoring-and-alerting-capabilities"></a>Kapsamlı izleme ve uyarı özellikleri

Azure SQL veritabanı, iş yükü özellikleriyle ilgili tüm öngörüleri almanıza yardımcı olabilecek bir dizi gelişmiş izleme ve sorun giderme özelliği sağlar. Özellikler ve araçlar şu şekilde kategorilere ayrılmıştır:
 - Gerçek zamanlı performans öngörülerini bulmanıza olanak tanıyan SQL Server veritabanı altyapısının en son sürümü tarafından sunulan yerleşik izleme özellikleri. 
 - Azure platformu tarafından sağlanan PaaS izleme özellikleri, çok sayıda veritabanı örneğini kolayca izlemenize olanak tanır ve ayrıca, performans sorunlarını gidermenize yardımcı olabilecek sorun giderme advıces.

Uygulamanız gereken en önemli yerleşik veritabanı motoru izleme özelliği, Sorgularınızın performansını gerçek zamanlı olarak kaydeden ve olası performans sorunlarını ve en üst kaynağı tanımlamanızı sağlayan [sorgu deposu](sql-database-operate-query-store.md) bileşenidir. tüketicilere. Otomatik ayarlama ve öneriler, gerileme performansına ve eksik veya yinelenen dizinlere sahip sorgularla ilgili bir performans sağlar. Azure SQL veritabanı 'nda otomatik ayarlama, sorunları giderebilen veya Azure SQL veritabanı 'nın sorunu çözebilmesine, test ve doğrulamaya izin vermesi için bu komut dosyalarını el ile uygulamanıza olanak tanır. Sorgu deposu ve otomatik ayarlama özelliklerine ek olarak, iş yükü performansını izlemek için standart [DMVs ve XEvent](sql-database-monitoring-with-dmvs.md) de kullanabilirsiniz.

Azure platformu, binlerce veritabanının durumunu kolayca izlemenize olanak sağlayan, performans derecelendirmeleri ile birlikte [yerleşik performans izleme](sql-database-performance.md) ve [Uyarı](sql-database-insights-alerts-portal.md) araçları sağlar. Bu araçları kullanarak, geçerli veya tahmini performans ihtiyaçlarınıza göre ölçeği artırma veya azaltma etkilerini hızla değerlendirebilirsiniz. SQL Veritabanı ayrıca izlemeyi kolaylaştırmak için [ölçümler ve tanılama günlükleri oluşturabilir](sql-database-metrics-diag-logging.md). SQL Veritabanını kaynak kullanımını, çalışanları, oturumları ve bu Azure kaynaklarından birine yapılan bağlantıları kaydedecek şekilde yapılandırabilirsiniz:

- **Azure depolama**: Küçük bir fiyata ait çok sayıda telemetrinin arşivlenmesi için.
- **Azure Olay Hub 'ı**: SQL veritabanı telemetrisini özel izleme çözümünüz veya etkin işlem hatlarınız ile tümleştirmek için.
- **Azure izleyici günlükleri**: Raporlama, uyarma ve azaltıcı yetenekler içeren yerleşik izleme çözümü için.

    ![architecture](./media/sql-database-metrics-diag-logging/architecture.png)

## <a name="availability-capabilities"></a>Kullanılabilirlik özellikleri

Geleneksel bir SQL Server ortamında, genel olarak (örneğin, AlwaysOn kullanılabilirlik grupları veya yük devretme kümesi örnekleri gibi özellikleri kullanarak) bir veri kopyası ile tek bir makine/bileşen hatası. Bu, yüksek kullanılabilirlik sağlar ancak veri merkezinizi yok etme doğal bir olağanüstü duruma karşı korumaz.

Olağanüstü durum kurtarma, çok zararlı bir olayın verilerinizin bir kopyasına sahip başka bir makine/makine kümesine sahip olacak şekilde coğrafi olarak yerelleştirilmiş olduğunu varsayar.  SQL Server, bu özelliği almak için her zaman zaman uyumsuz modda çalışan kullanılabilirlik gruplarını kullanabilirsiniz.  Hafif sorun hızı genellikle insanların, bir işlemi gerçekleştirmeden önce çoğaltmanın daha önce uzakta olmasını beklemek istemelerinin yanı, planlanmamış yük devretme işlemleri sırasında veri kaybına neden olabilir.

Premium ve iş açısından kritik hizmet katmanlarındaki veritabanları, bir kullanılabilirlik grubunun eşitlemesine [çok benzer bir şey zaten yapılır](sql-database-high-availability.md#premium-and-business-critical-service-tier-availability) . Daha düşük hizmet katmanlarındaki veritabanları, [farklı ancak eşdeğer bir mekanizma](sql-database-high-availability.md#basic-standard-and-general-purpose-service-tier-availability)kullanarak depolama aracılığıyla artıklık sağlar. Tek bir makine hatasına karşı koruyan mantık vardır.  Etkin coğrafi çoğaltma özelliği, tüm bir bölgenin yok edileceği olağanüstü duruma karşı koruma olanağı sağlar.

Azure Kullanılabilirlik Alanları yüksek kullanılabilirlik sorunu üzerinde oynatılır.  Tek bir bölge içinde tek bir veri merkezi oluşturma kesintisine karşı korunmaya çalışır.  Bu nedenle, bir binadan güç veya ağ kaybına karşı korunmak ister. SQL Azure, bu, farklı çoğaltmaları farklı kullanılabilirlik bölgelerine (farklı binalar, etkin) yerleştirerek ve daha önce olduğu gibi çalışarak çalışacaktır.

Azure 'un, Microsoft tarafından yönetilen küresel bir veri merkezi ağı tarafından desteklenen sektörün önde gelen% 99,99 kullanılabilirlik hizmet düzeyi sözleşmesi [(SLA)](https://azure.microsoft.com/support/legal/sla/), uygulamanızın 24/7 çalışmasına yardımcı olur. Azure platformu her veritabanını tamamen yönetir ve veri kaybı ve yüksek oranda veri kullanılabilirliği garantisi vermez. Azure düzeltme eki uygulama, yedekleme, çoğaltma, hata algılama, arka plandaki potansiyel donanım, yazılım veya ağ arızaları, hata giderme, yük devretme, veritabanı yükseltmesi ve diğer bakım görevlerini otomatik olarak gerçekleştirir. Standart kullanılabilirlik düzeyine ulaşmak için işlem ve depolama katmanları ayrılır. Premium kullanılabilirlik, performans için işlem ve depolamayı tek bir düğümde tümleştirerek ve ardından her zaman kapakları altındaki kullanılabilirlik gruplarıyla benzer bir teknoloji uygulayarak elde edilir. Azure SQL veritabanı 'nın yüksek kullanılabilirlik özelliklerine ilişkin tam bir tartışma için bkz. [SQL veritabanı kullanılabilirliği](sql-database-high-availability.md). SQL Veritabanı ayrıca aşağıdakiler dahil olmak üzere yerleşik [iş sürekliliği ve global ölçeklenebilirlik](sql-database-business-continuity.md) özelliklerine sahiptir:

- **[Otomatik yedeklemeler](sql-database-automated-backups.md)** :

  SQL veritabanı, zaman içinde herhangi bir noktaya geri yükleme olanağı sağlamak için Azure SQL veritabanlarının tam, değişiklik ve işlem günlüğü yedeklemelerini otomatik olarak gerçekleştirir. Tek veritabanları ve havuza alınmış veritabanları için SQL veritabanını, uzun süreli yedek saklama için Azure depolama 'ya tam veritabanı yedeklerini depolayacak şekilde yapılandırabilirsiniz. Yönetilen örnekler için, uzun süreli yedek saklama için yalnızca kopya yedeklemeler de gerçekleştirebilirsiniz.

- **[Zaman içinde bir noktaya geri yükleme](sql-database-recovery-using-backups.md)** :

  Tüm SQL veritabanı dağıtım seçenekleri herhangi bir Azure SQL veritabanı için otomatik yedekleme saklama süresi içinde herhangi bir zaman noktasına kurtarmayı destekler.
- **[Etkin coğrafi çoğaltma](sql-database-active-geo-replication.md)** :

  Tek veritabanı ve havuza alınmış veritabanları, aynı veya küresel olarak dağıtılmış Azure veri merkezlerinde dört adede kadar okunabilir ikincil veritabanını yapılandırmanızı sağlar.  Örneğin, yüksek hacimli eşzamanlı salt okunur işlemlere sahip bir katalog veritabanı kullanan bir SaaS uygulamanız varsa, etkin coğrafi çoğaltmayı kullanarak global okuma ölçeğini etkinleştirebilir ve birincil veritabanı üzerindeki okuma iş yükü kaynaklı performans sorunlarını ortadan kaldırabilirsiniz. Yönetilen örnekler için otomatik yük devretme grupları kullanın.
- **[Otomatik yük devretme grupları](sql-database-auto-failover-group.md)** :

  Tüm SQL veritabanı dağıtım seçenekleri, küresel ölçekte yüksek kullanılabilirlik ve yük dengelemeyi etkinleştirmek için yük devretme gruplarını kullanmanıza olanak sağlar. bu da, büyük veritabanı kümelerinin, elastik havuzların ve yönetilen örneklerin yük devretmesi dahildir. Yük devretme grupları, tüm karmaşık izleme, Yönlendirme ve yük devretme düzenlemesini SQL veritabanı 'na bırakarak küresel olarak dağıtılan SaaS uygulamalarının oluşturulmasına olanak sağlar.
- Bölgesel olarak **[yedekli veritabanları](sql-database-high-availability.md)** :

  SQL veritabanı, birden çok kullanılabilirlik alanında Premium veya iş açısından kritik veritabanları ya da elastik havuzlar sağlamanıza olanak tanır. Bu veritabanları ve elastik havuzlar yüksek kullanılabilirlik için birden fazla yedekli çoğaltma içerdiğinden, bu çoğaltmaların birden fazla kullanılabilirlik alanına yerleştirilmesi, veri merkezi ölçek hatalarından veri kaybı olmadan otomatik olarak kurtarma becerisi de dahil olmak üzere daha yüksek esneklik sağlar.

## <a name="built-in-intelligence"></a>Yerleşik zeka

SQL Veritabanı ile veritabanı çalıştırma ve yönetim maliyetlerini önemli ölçüde azaltmanıza yardımcı olan ve uygulamanızı hem performans hem de güvenlik açısından üst düzeye taşıyan yerleşik zekaya sahip olursunuz. Her saniye milyonlarca müşterinin iş yükünü çalıştıran SQL Veritabanı çok büyük miktarlarda telemetri verilerini toplayıp işlemenin yanı sıra arka planda müşteri gizliliğini de korumaktadır. Telemetri verilerini sürekli değerlendiren çok farklı algoritmalar sayesinde hizmet uygulamanızı öğrenip kendini ona göre uyarlayabilir. Bu Analize bağlı olarak hizmet, belirli iş yükünüze özel performans geliştirme önerilerini sunar.

### <a name="automatic-performance-monitoring-and-tuning"></a>Otomatik performans izleme ve ayarlama

SQL Veritabanı izlemeniz gereken sorgularla ilgili ayrıntılı öngörüler sunar. SQL Veritabanı'nın veritabanı desenleriniz hakkında öğrendikleri sayesinde veritabanı şemanızı iş yükünüze uyarlayabilirsiniz. SQL Veritabanı, [performans ayarlama önerilerinde](sql-database-advisor.md) bulunur. Siz de bu ayarları gözden geçirebilir ve uygulayabilirsiniz.

Ancak, bir veritabanını sürekli olarak izlemek, özellikle birçok veritabanı ile ilgilenirken zor ve sıkıcı bir görevdir. [Akıllı Öngörüler](sql-database-intelligent-insights.md), SQL Veritabanı performansını otomatik olarak ölçekte izleyerek bu işi sizin için yapar ve performans düşüşü sorunlarını size bildirir, sorunun kök kaynağını belirler ve mümkün olduğunda performans iyileştirme önerileri sağlar.

Çok sayıda veritabanını yönetmek SQL Veritabanı ve Azure portalı tarafından sunulan araçlarla bile verimli şekilde yapılması imkanız bir görev haline gelebilir. Veritabanınızı el ile izlemek ve ayarlamak yerine izleme ve ayarlama işlerinin bazılarını SQL Veritabanı'na bırakarak [otomatik ayarlamayı](sql-database-automatic-tuning.md) kullanabilirsiniz. SQL veritabanı, performansın sürekli olarak iyileştirildiğinden emin olmak için öneri, test ve ayarlama eylemlerinin her birini doğrular. Bu şekilde, SQL veritabanı otomatik olarak denetimli ve güvenli bir şekilde iş yükünüze uyum sağlar. Otomatik ayarlama, veritabanınızın performansının dikkatli bir şekilde izlenmesi ve her ayar işleminden önce ve sonraki durumunun karşılaştırılarak performans artışı görülmediği durumlarda ayarların geri alındığı bir durumdur.

Bugün SQL Veritabanı üzerinde [çok kiracılı SaaS multi-tenant uygulamaları](sql-database-design-patterns-multi-tenancy-saas-applications.md) çalıştıran iş ortaklarımızın çoğu uygulamalarının kararlı ve tahmin edilebilir performansa sahip olduğundan emin olmak için otomatik performans ayarlarına güveniyor. İş ortaklarımız bu özelliğin gecenin ortasında performans sorunu yaşama riskini önemli ölçüde azalttığını söylüyor. Ayrıca müşterilerinin belirli bölümü SQL Server kullandığından SQL Server müşterilerine yardımcı olmak için SQL Veritabanı tarafından sunulan dizinleme önerilerinden faydalanıyorlar.

[SQL Veritabanında](sql-database-automatic-tuning.md) iki otomatik ayarlama yöntemi mevcuttur:

- **Otomatik Dizin Yönetimi**: Veritabanınıza eklenmesi gereken dizinleri ve kaldırılması gereken dizinleri tanımlar.
- **Otomatik plan düzeltmesi**: Sorunlu planları tanımlar ve SQL planı performans sorunlarını düzeltir (çok yakında, SQL Server 2017 ' de zaten kullanılabilir).

### <a name="adaptive-query-processing"></a>Uyarlamalı sorgu işleme

SQL Veritabanı'na çok durumlu tablo değerli işlevler için araya eklemeli yürütme, toplu iş modu bellek ataması geri bildirimi ve toplu iş modu uyarlamalı birleşimler dahil olmak üzere [uyarlamalı sorgu işleme](/sql/relational-databases/performance/intelligent-query-processing) özellik ailesini de ekliyoruz. Bu uyarlamalı sorgu işleme özelliklerinin her biri benzer "öğren ve uyarla" tekniklerini uygulayarak geçmişe dönük zorlu sorgu iyileştirme sorunlarıyla ilgili performans sorunlarında yardımcı olmaktadır.

## <a name="advanced-security-and-compliance"></a>Gelişmiş koruma ve uyumluluk

SQL Veritabanı, uygulamanızın çeşitli güvenlik ve uyumluluk gereksinimlerine uygun olmasına yardımcı olmak için bir dizi [yerleşik güvenlik ve uyum özelliğine](sql-database-security-overview.md) sahiptir.

> [!IMPORTANT]
> Azure SQL veritabanı (tüm dağıtım seçenekleri), bir dizi uyumluluk standartlarına karşı sertifikalandırilmiştir. Daha fazla bilgi için SQL veritabanı uyumluluk sertifikalarının en güncel listesini bulabileceğiniz [Microsoft Azure Güven Merkezi](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942) ' ne bakın.

### <a name="advance-threat-protection"></a>Gelişmiş Tehdit Koruması

Gelişmiş veri güvenliği, gelişmiş SQL güvenlik özelliklerine yönelik Birleşik bir pakettir. Hassas verileri keşfedip sınıflandırma, veritabanınızın güvenlik açıklarını yönetme ve veritabanınıza ilişkin bir tehdit belirtisi olabilecek anormal etkinlikleri algılamaya yönelik işlevsellik sağlar. Bu özellikler tek bir konumdan etkinleştirilebilir ve yönetilebilir.

- [Veri bulma & sınıflandırması](sql-database-data-discovery-and-classification.md):

  Bu Özellik (Şu anda önizlemede), veritabanlarınızdaki hassas verileri korumak & bulmak, sınıflandırmak, etiketleme için Azure SQL veritabanı 'nda yerleşik olarak bulunan yetenekler sağlar. Veri sınıflandırma durumunuz için görünürlük sağlamanın yanı sıra veritabanı içindeki ve dışındaki hassas verilere erişimin izlenmesi için kullanılabilir.
- [Güvenlik açığı değerlendirmesi](sql-vulnerability-assessment.md):

  Bu hizmet, olası veritabanı güvenlik açıklarını düzeltmenizi, izleyebilir ve yardımcı olabilir. Güvenlik durumunuz hakkında görünürlük sağlamasının yanı sıra güvenlik sorunlarınızı çözmek ve veritabanı güçlendirmelerinizi geliştirmek için eyleme dönüştürülebilir adımlar sunar.
- [Tehdit algılama](sql-database-threat-detection.md):

  Bu özellik, veritabanınıza erişmek veya veritabanına yararlanmak için olağan dışı ve zararlı olabilecek girişimleri gösteren anormal etkinlikleri algılar. Veritabanınızı şüpheli etkinliklere karşı sürekli izler ve olası güvenlik açıkları, SQL ekleme saldırıları ve anormal veritabanı erişim modelleri hakkında anında güvenlik uyarıları sunar. Tehdit algılama uyarıları şüpheli etkinliğin ayrıntılarını sağlar ve tehdidi araştırmak ve azaltmak için eyleme önerilir.

### <a name="auditing-for-compliance-and-security"></a>Uyumluluk ve güvenlik denetimi

[Denetim](sql-database-auditing.md) , veritabanı olaylarını izler ve bunları Azure Depolama hesabınızdaki bir denetim günlüğüne yazar. Denetim mevzuatla uyumluluk, veritabanı etkinliğini anlama ve işletme sorunlarını veya şüpheli güvenlik ihlallerini işaret edebilecek farklılıklar ve anormal durumlar hakkında öngörü sahip olmanıza yardımcı olabilir.

### <a name="data-encryption"></a>Veri şifreleme

SQL veritabanı, [Aktarım Katmanı Güvenliği](https://support.microsoft.com/kb/3135244)ile hareket halindeki veriler için, [Saydam veri şifrelemesiyle](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql)bekleyen veriler için ve [her zaman şifreli](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine)olarak kullanılan veriler için şifreleme sağlayarak verilerinizin güvenliğini sağlar.

### <a name="azure-active-directory-integration-and-multi-factor-authentication"></a>Azure Active Directory tümleştirmesi ve çok faktörlü kimlik doğrulaması

SQL Veritabanı, [Azure Active Directory tümleştirmesi](sql-database-aad-authentication.md) ile veritabanı kullanıcısı ve diğer Microsoft hizmetleri kimliklerini bir merkezden yönetmenizi sağlar. Bu özellik, izin yönetimini kolaylaştırırken güvenliği artırır. Azure Active Directory, veri ve uygulama güvenliğini artırmak için [çok faktörlü kimlik doğrulamasını](sql-database-ssms-mfa-authentication.md) (MFA) ve çoklu oturum açma işlemini destekler.

### <a name="compliance-certification"></a>Uyumluluk sertifikası

SQL Veritabanı düzenli olarak denetimden geçmektedir ve birden fazla uyumluluk standardı sertifikasına sahiptir. Daha fazla bilgi için SQL veritabanı uyumluluk sertifikalarının en güncel listesini bulabileceğiniz [Microsoft Azure Güven Merkezi](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942) ' ne bakın.

## <a name="easy-to-use-tools"></a>Kullanımı kolay araçlar

SQL Veritabanı uygulama oluşturma ve uygulamaların bakımını yapma işlemlerinin daha kolay ve daha verimli şekilde yapılmasını sağlar. SQL Veritabanı size, en iyi yaptığınız işe; mükemmel uygulamalar oluşturmaya odaklanma seçeneği sunar. Sahip olduğunuz araçları ve becerileri kullanarak SQL Veritabanı ile yönetebilir ve geliştirebilirsiniz.

- **[Azure Portal](https://portal.azure.com/)** :

  Tüm Azure hizmetlerini yönetmek için Web tabanlı bir uygulama
- **[SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)** :

  SQL Server SQL veritabanı 'na kadar herhangi bir SQL altyapısını yönetmeye yönelik ücretsiz, indirilebilir bir istemci uygulaması
- **[Visual Studio 'da SQL Server veri araçları](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt)** :

  SQL Server ilişkisel veritabanları, Azure SQL veritabanları, Tümleştirme Hizmetleri paketleri, Analysis Services veri modelleri ve Raporlama Hizmetleri raporları geliştirmeye yönelik ücretsiz, indirilebilir bir istemci uygulaması.
- **[Visual Studio Code](https://code.visualstudio.com/docs)** :

  Microsoft SQL Server, Azure SQL veritabanı ve SQL veri ambarı 'nı sorgulamak için [MSSQL uzantısı](https://aka.ms/mssql-marketplace) dahil olmak üzere uzantıları destekleyen Windows, MacOS ve Linux için ücretsiz, indirilebilir, açık kaynaklı, kod Düzenleyicisi.

SQL Veritabanı MacOS, Linux ve Windows üzerinde Python, Java, Node.js, PHP, Ruby ve .NET ile uygulama oluşturmayı destekler. SQL Veritabanı, SQL Server ile aynı [bağlantı kitaplıklarını destekler](sql-database-libraries.md).

[!INCLUDE [sql-database-create-manage-portal](includes/sql-database-create-manage-portal.md)]

## <a name="sql-database-frequently-asked-questions-faq"></a>SQL veritabanı sık sorulan sorular (SSS)

### <a name="what-is-the-current-version-of-sql-database"></a>SQL veritabanı 'nın geçerli sürümü nedir?

SQL veritabanının geçerli sürümü v12. Sürüm V11 kullanımdan kaldırıldı.

### <a name="can-i-control-when-patching-downtime-occurs"></a>Düzeltme eki uygulama süresinin ne zaman gerçekleşeceğini denetleyebilir

Hayır. Uygulamanızda [yeniden deneme mantığı](sql-database-develop-overview.md#resiliency) kullanırsanız, düzeltme eki uygulamanın etkisi genellikle fark etmez. Azure SQL veritabanınızda planlı bakım olaylarına hazırlanma hakkında daha fazla bilgi için bkz. Azure [SQL veritabanı 'Nda Azure bakım olaylarını planlama](sql-database-planned-maintenance.md).

### <a name="azure-hybrid-benefit-questions"></a>Azure Hibrit Avantajı sorular

#### <a name="are-there-dual-use-rights-with-azure-hybrid-benefit-for-sql-server"></a>Azure Hibrit Avantajı SQL Server için çift kullanım hakları mevcuttur

Geçişlerin sorunsuz çalıştığından emin olmak için lisansın 180 gün süreyle çift kullanım haklarına sahip olursunuz. Bu 180 günlük süre sonra, SQL Server lisansı yalnızca SQL veritabanı 'nda bulutta kullanılabilir ve şirket içinde ve bulutta çift kullanım haklarına sahip değildir.

#### <a name="how-does-azure-hybrid-benefit-for-sql-server-differ-from-license-mobility"></a>SQL Server için Azure Hibrit Avantajı, lisans taşınabilirliği 'nden farklı

Günümüzde, Yazılım Güvencesi kapsamındaki müşterilerin lisanslarına üçüncü taraf paylaşılan sunuculara yeniden atanmasını sağlayan lisans taşınabilirlik SQL Server avantajları sunuyoruz. Bu avantaj, Azure IaaS ve AWS EC2 üzerinde kullanılabilir.
SQL Server için Azure Hibrit Avantajı, iki anahtar alanındaki lisans taşınabilirliğine göre farklılık gösterir:

- Yüksek oranda sanallaştırılmış iş yüklerini Azure 'a taşımak için ekonomik avantajlar sağlar. SQL EE müşterileri, yüksek oranda sanallaştırılmış uygulamalar için şirket içinde sahip oldukları her çekirdek için Genel Amaçlı SKU 'sunda Azure 'da 4 çekirdek alabilir. Lisans Taşınabilirliği, sanallaştırılmış iş yüklerini buluta taşımak için özel maliyet avantajlarına izin vermez.
- Şirket içi SQL Server ile yüksek oranda uyumlu Azure 'da (SQL veritabanı yönetilen örneği) bir PaaS hedefi sağlar

#### <a name="what-are-the-specific-rights-of-the-azure-hybrid-benefit-for-sql-server"></a>Azure Hibrit Avantajı SQL Server için özel haklar nelerdir?

SQL veritabanı müşterileri, SQL Server için Azure Hibrit Avantajı ilişkili aşağıdaki haklara sahip olacaktır:

|Lisans ayak Izi|SQL Server için ne Azure Hibrit Avantajı?|
|---|---|
|SA ile SQL Server Enterprise sürümü çekirdek müşterileri|<li>Genel Amaçlı veya İş Açısından Kritik SKU 'sunda temel ücret ödeyebilir</li><br><li>1 çekirdek şirket içi = Genel Amaçlı SKU 'da 4 çekirdek</li><br><li>1 çekirdek şirket içi = 1 çekirdek İş Açısından Kritik SKU 'da</li>|
|SA ile SQL Server Standard sürümü çekirdek müşterileri|<li>Temel fiyat yalnızca Genel Amaçlı SKU üzerinde ödeme yapabilir</li><br><li>1 çekirdek şirket içi = 1 çekirdek Genel Amaçlı SKU 'da</li>|
|||

## <a name="engage-with-the-sql-server-engineering-team"></a>SQL Server mühendislik ekibi ile iletişime geçme

- [DBA yığın değişimi](https://dba.stackexchange.com/questions/tagged/sql-server): Veritabanı yönetim sorularını sorma
- [Stack Overflow](https://stackoverflow.com/questions/tagged/sql-server): Geliştirme sorularını sorma
- [MSDN forumları](https://social.msdn.microsoft.com/Forums/home?category=sqlserver): Teknik sorular sorun
- [Geri bildirim](https://aka.ms/sqlfeedback): Hataları ve istek özelliğini bildir
- [Reddıt](https://www.reddit.com/r/SQLServer/): SQL Server tartışın

## <a name="next-steps"></a>Sonraki adımlar

- Tek veritabanı ve elastik havuz maliyet karşılaştırmaları ve hesaplayıcıları için bkz. [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/sql-database/).
- Başlamanıza yardımcı olacak şu hızlı başlangıçlara bakın:

  - [Azure portalda SQL veritabanı oluşturma](sql-database-single-database-get-started.md)  
  - [Azure CLI ile SQL veritabanı oluşturma](sql-database-get-started-cli.md)
  - [PowerShell ile SQL veritabanı oluşturma](sql-database-get-started-powershell.md)

- Azure CLI ve PowerShell örnekleri için bkz.:
  - [SQL Veritabanı için Azure CLI örnekleri](sql-database-cli-samples.md)
  - [SQL Veritabanı için Azure PowerShell örnekleri](sql-database-powershell-samples.md)

 - Duyurulduğu için yeni yetenekler hakkında daha fazla bilgi için bkz. 
   - **[SQL veritabanı Için Azure yol haritası](https://azure.microsoft.com/roadmap/?category=databases)** -yeni nelerin ve yakında geldiğini öğrenmek için bir yer.
  - **[Azure SQL veritabanı blogu](https://azure.microsoft.com/blog/topics/database)** -SQL Server ürün EKIBININ, SQL veritabanı haberleri ve özellikleri hakkında blogdaki bir yerdir.

