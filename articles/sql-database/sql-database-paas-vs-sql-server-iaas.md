---
title: Doğru dağıtım seçeneğini seçin
description: Azure sanal makinelerinde SQL veritabanları, SQL yönetilen örnekler ve SQL Server arasında Azure SQL'deki dağıtım seçenekleri arasında nasıl seçim yapabileceğinizi öğrenin.
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
keywords: SQL Server bulut, bulutta SQL Server, PaaS veritabanı, bulut SQL Server, DBaaS
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 08/22/2019
ms.openlocfilehash: e642454807511e8e0bc0b6b6ca7af837e03de2cf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73821345"
---
# <a name="choose-the-right-deployment-option-in-azure-sql"></a>Azure SQL'de doğru dağıtım seçeneğini seçin

Her dağıtım seçeneğinin Microsoft'un Azure SQL veri platformuna nasıl uyduğunu öğrenin ve iş gereksinimleriniz için doğru seçeneği eşleştirme konusunda yardım alın. Maliyet tasarruflarına veya minimum yönetime öncelik verin, bu makalede, en çok önem verdiğiniz iş gereksinimlerine göre hangi yaklaşımın yapılacağına karar vermenize yardımcı olabilir.

## <a name="microsofts-azure-sql-data-platform"></a>Microsoft'un Azure SQL veri platformu

Azure SQL, kaldırma ve kaydırma geçişlerinden mevcut uygulamaların modernizasyonuna, modern bulut hizmetleri oluşturmaya kadar, sektörün önde gelen Microsoft SQL Server altyapısıtarafından desteklenen çeşitli dağıtım seçenekleri sunan modern bir SQL platformudur. Azure SQL, en zorlu geçiş ve modernizasyon gereksinimlerini karşılamak için temel platform üzerinde farklı denetim düzeylerine sahip çok çeşitli uygulama modellerini desteklemek üzere tasarlanmıştır. Azure SQL, tek birleştirilmiş yönetim deneyimi sağlayarak sql server tabanlı uygulamaların çeşitli koleksiyonlarını ölçekte yönetme karmaşıklığını ortadan kaldırır.

Azure ile şirket içi SQL Server veritabanlarının karşılaştırmasına yönelik herhangi bir tartışmada anlaşılması gereken ilk unsur, bunların tümünü kullanabileceğinizdir. Microsoft'un veri platformu SQL Server teknolojisinden yararlanır ve fiziksel şirket içi makineler, özel bulut ortamları, üçüncü taraf barındırılan özel bulut ortamları ve genel bulut arasında kullanılabilir hale getirir. Azure sanal makinelerdeki SQL Server (SQL sanal makineleri), aynı sunucu ürünleri, geliştirme araçları ve uzmanlık kümesini kullanırken şirket içi ve bulut barındırılan dağıtımların bir kombinasyonu aracılığıyla benzersiz ve çeşitli iş gereksinimlerini karşılamanızı sağlar bu ortamlarda.

   ![Bulut SQL Server seçenekleri: IaaS üzerinde SQL sunucusu veya bulutta SaaS veritabanı.](./media/sql-database-paas-vs-sql-server-iaas/SQLIAAS_SQL_Server_Cloud_Continuum.png)

Diyagramda görüldüğü gibi, her teklif, altyapı üzerinde sahip olduğunuz yönetim düzeyi ve maliyet verimliliği derecesi ile karakterize edilebilir.

Azure'da, SQL Server iş yüklerinizin barındırılan bir hizmet[(PaaS)](https://azure.microsoft.com/overview/what-is-paas/)veya barındırılan bir altyapı[(IaaS)](https://azure.microsoft.com/overview/what-is-iaas/)olarak çalışmasını sağlayabilirsiniz. PaaS içinde, her dağıtım seçeneği içinde birden çok dağıtım seçeneği ve hizmet katmanları vardır. PaaS veya IaaS arasında karar verirken sormanız gereken temel soru veritabanınızı yönetmek, yama uygulamak ve yedekleme almak mı yoksa bu işlemleri Azure'a devretmek mi istiyorsunuz?

Yanıta bağlı olarak, aşağıdaki seçeneklere sahipsiniz:

- [**SQL veritabanları**](sql-database-technical-overview.md): En son kararlı SQL Server özelliklerini kullanmak isteyen ve geliştirme ve pazarlamada zaman kısıtlamaları olan modern bulut uygulamaları için en iyisidir. SQL Server'ın en son kararlı Enterprise Edition'ına dayanan tam yönetilen bir SQL veritabanı altyapısı. Bu, Azure bulutunda barındırılan ve Hizmet Olarak Platform (PaaS) sektör kategorisine giren ilişkisel bir hizmet *veritabanıdır (DBaaS).* SQL veritabanında, her biri Microsoft'un sahip olduğu, barındırılan ve bakımı yapılan standartlaştırılmış donanım ve yazılımlar üzerine inşa edilen birden çok dağıtım seçeneği vardır. SQL Server ile, kapsamlı yapılandırma gerektiren yerleşik özellikleri ve işlevleri (şirket içinde veya Azure sanal makinesinde) kullanabilirsiniz. SQL Database'i kullandığınızda, kesinti olmadan daha fazla güç için ölçeğinizi artırmaya veya genişletmeye yönelik seçeneklerle kullandıkça ödersiniz. SQL Veritabanı, yerleşik yüksek kullanılabilirlik, istihbarat ve yönetim gibi SQL Server'da bulunmayan bazı ek özelliklere sahiptir.


  Veritabanları aşağıdaki dağıtım seçeneklerini sunar:
  - Bir [***veritabanı***](sql-database-single-database.md) sunucusu üzerinden yönetilen kendi kaynakları kümesi ile tek bir veritabanı olarak. Tek bir veritabanı, SQL Server'daki [bulunan veritabanına](https://docs.microsoft.com/sql/relational-databases/databases/contained-databases) benzer. Bu seçenek, bulut kaynaklı yeni uygulamaların modern uygulama geliştirmesi için optimize edecektir. [Hiper ölçekve](sql-database-service-tier-hyperscale.md) [sunucusuz](sql-database-serverless.md) seçenekler mevcuttur.
  - Bir veritabanı sunucusu üzerinden yönetilen paylaşılan bir kaynak kümesine sahip veritabanları topluluğu olan elastik bir [***havuz.***](sql-database-elastic-pool.md) Tek veritabanları elastik bir havuza ve dışına taşınabilir. Bu seçenek, çok kiracılı SaaS uygulama deseni kullanılarak yeni bulut kaynaklı uygulamaların modern uygulama geliştirmesi için optimize edecektir. Elastik havuzlar, değişken kullanım desenleri olan birden çok veritabanının performansını yönetmek için uygun maliyetli bir çözüm sağlar.
  - Tek veritabanları ve elastik havuzları grupları yönetmek için kullanılan bir [***veritabanı sunucusu.***](sql-database-servers.md) Veritabanı sunucuları birden çok tek veya havuzlu veritabanları, [oturum açmalar,](sql-database-manage-logins.md) [güvenlik duvarı kuralları,](sql-database-firewall-configure.md) [denetim kuralları,](sql-database-auditing.md) [tehdit algılama ilkeleri](sql-database-threat-detection.md)ve [başarısız gruplar](sql-database-auto-failover-group.md)için merkezi bir yönetim noktası olarak hareket eder.

- [**SQL yönetilen örnekleri**](sql-database-managed-instance.md): Buluta yapılan çoğu geçiş için en iyisidir. Yönetilen örnek, kaldırma ve kaydırma hazır kaynakların paylaşılan bir kümesi ile sistem ve kullanıcı veritabanları topluluğudur. En son kararlı SQL Server özelliklerini kullanmak isteyen ve en az değişiklikle buluta geçirilen yeni uygulamalar veya mevcut şirket içi uygulamalar için en iyisidir. Yönetilen örnek, veritabanları ve örnek kapsamlı ek özellikler için paylaşılan kaynaklar sunan [Microsoft SQL Server veritabanı altyapısının](https://docs.microsoft.com/sql/database-engine/sql-server-database-engine-overview) bir örneğine benzer. Yönetilen örnek, veritabanı değişiminin en az olduğu şirket içi geçişini destekler. Bu seçenek, Azure SQL Veritabanı'nın tüm PaaS avantajlarını sağlar, ancak daha önce yalnızca SQL VM'lerde kullanılabilen özellikler ekler. Buna yerel bir sanal ağ (VNet) ve şirket içi SQL Server ile %100'e yakın uyumluluk dahildir. Yönetilen örnekler, SQL Sunucularını Azure'a geçirmek için tam SQL Server erişimi ve özellik uyumluluğu sağlar.


- [**SQL sanal makineleri**](../virtual-machines/windows/sql/virtual-machines-windows-sql-server-iaas-overview.md): Os düzeyinde erişim gerektiren geçişler ve uygulamalar için en iyisidir. SQL sanal makineleri, en az değişiklik veya hiç değişiklik olmadan buluta hızlı geçiş gerektiren varolan uygulamalar için kaldırma ve kaydırma hazırdır. SQL sanal makineleri, Azure'a geçiş için SQL Server örneği ve temel işletim sistemi üzerinde tam yönetim denetimi sunar. Şirket içi üretim dışı SQL Server donanımı satın almak istemediğinizde hızlı geliştirme ve test senaryoları. SQL sanal makineleri endüstri kategorisiNe girer *Hizmet Olarak Altyapı (IaaS)* ve Sql Server'ı Azure bulutunda tam olarak yönetilen bir sanal makinede (VM) çalıştırmanızı sağlar. SQL sanal makineleri, Microsoft'un sahip olduğu, barındırdığı ve bakımı yapılan standart laştırılmış donanımda da çalışır. SQL sanal makineleri kullanırken, sql server görüntüsünde zaten bulunan bir SQL Server lisansı için kullandıkça ödeyebilir veya varolan bir lisansı kolayca kullanabilirsiniz. Gerektiğinde VM'yi durdurabilir veya devam ettirebilirsiniz. SQL Server yüklü ve bulut barındırılan Windows Server veya Linux sanal makineleri Azure üzerinde çalışan çalışır , ayrıca bir hizmet olarak altyapı olarak bilinen (IaaS). SQL sanal makineleri herhangi bir veritabanı değişikliği olmadan şirket içi SQL Server veritabanları ve uygulamaları geçirmek için iyi bir seçenektir. SQL Server'ın tüm yeni sürümleri ve sürümleri iaas sanal makinede kurulum için kullanılabilir. SQL veritabanları ve SQL yönetilen örnekleri en önemli fark, SQL Server VMs veritabanı altyapısı üzerinde tam kontrol sağlar. Bakım/yama başlatmayı ne zaman başlatacağınızı seçebilir, kurtarma modelini basit veya toplu günlüğe kaydedilmiş olarak değiştirebilir, gerektiğinde hizmeti duraklatabilir veya başlatabilir ve SQL Server veritabanı altyapısını tamamen özelleştirebilirsiniz. Bu ek kontrol ile sanal makine yönetmek için ek sorumluluk geliyor.

  Varolan uygulamaları Azure'a geçirmek veya karma dağıtımlarda mevcut şirket içi uygulamaları buluta genişletmek için optimize edilsin. Ayrıca, SQL Server’ı sanal bir makinede kullanarak geleneksel SQL Server uygulamaları geliştirip test edebilirsiniz. SQL sanal makineleri ile, özel bir SQL Server örneği ve bulut tabanlı VM üzerinde tam yönetim haklarına sahipsiniz. Bu, bir kuruluşun halihazırda sanal makinelerin bakımını yapmak için kullanılabilir BT kaynaklarına sahip olması durumunda ideal bir seçenektir. Bu özellikler uygulamanızın belirli performans ve kullanılabilirlik gereksinimlerini karşılamak üzere yüksek düzeyde özelleştirilmiş bir sistem oluşturmanıza olanak sağlar.


Ek farklar aşağıdaki tabloda listelenir, ancak ***hem veritabanları hem de yönetilen örnekler, birçok veritabanını sağlamak ve yönetmek için genel yönetim maliyetlerini en aza indirecek şekilde optimize edilsin.*** Herhangi bir sanal makineyi, işletim sistemini veya veritabanı yazılımını yönetmeniz gerekmediğinden, bu, devam eden yönetim maliyetlerini azaltır. Yükseltme, yüksek kullanılabilirlik veya [yedeklemeleri](sql-database-automated-backups.md) yönetmeniz gerekli değildir. Genellikle, Azure SQL Veritabanı tek bir BT veya geliştirme kaynağı tarafından yönetilen veritabanlarının sayısını önemli ölçüde artırabilir. [Elastik havuzlar](sql-database-elastic-pool.md) ayrıca, kiracı yalıtımı ve kaynakları veritabanları arasında paylaşarak maliyetleri azaltmak için ölçeklendirme olanağı gibi özelliklere sahip SaaS çok kiracılı uygulama mimarilerini de destekler. [Yönetilen örnek,](sql-database-managed-instance.md) varolan uygulamaların kolay geçişini sağlayan örnek kapsamlı özellikler için destek sağlamanın yanı sıra kaynakları veritabanları arasında paylaşır.

| SQL veritabanları | SQL yönetilen örnekleri | SQL sanal makineler |
| :--- | :--- | :--- |
|Şirket içi veritabanı düzeyindeki özelliklerin çoğunu destekler. En sık kullanılan SQL Server özellikleri mevcuttur.<br/>%99,995 kullanılabilirlik garantisi.<br/>Dahili yedeklemeler, yama, kurtarma.<br/>En son kararlı Veritabanı Motoru sürümü.<br/>Tek tek veritabanlarına gerekli kaynakları (CPU/depolama) atama yeteneği.<br/>Dahili gelişmiş istihbarat ve güvenlik.<br/>Kaynakların çevrimiçi değişimi (CPU/depolama).| Hemen hemen tüm şirket içi örnek düzeyi ve veritabanı düzeyinde yetenekleri destekler. SQL Server şirket içi ile yüksek uyumluluk.<br/>%99,99 kullanılabilirlik garantisi.<br/>Dahili yedeklemeler, yama, kurtarma.<br/>En son kararlı Veritabanı Motoru sürümü.<br/>SQL Server'dan kolay geçiş.<br/>Azure VNet içindeki özel IP adresi.<br/>Dahili gelişmiş istihbarat ve güvenlik.<br/>Kaynakların çevrimiçi değişimi (CPU/depolama).| SQL Server motoru üzerinde tam kontrole sahipsiniz. Tüm şirket içi yetenekleri destekler.<br/>%99,99'a kadar kullanılabilirlik.<br/>Şirket içi SQL Server'ın eşleşen sürümüyle tam eşlik.<br/>Düzeltildi, iyi bilinen veritabanı motoru sürümü.<br/>SQL Server'dan şirket içinde kolay geçiş.<br/>Azure VNet içindeki özel IP adresi.<br/>SQL Server'ın yerleştirildiği ana bilgisayara uygulama veya hizmetleri dağıtma olanağınız var.|
|SQL Server'dan geçiş zor olabilir.<br/>Bazı SQL Server özellikleri kullanılamıyor.<br/>Hiçbir garantili tam bakım süresi (ama neredeyse şeffaf).<br/>SQL Server sürümüyle uyumluluk yalnızca veritabanı uyumluluk düzeyleri kullanılarak sağlanabilir.<br/>Özel IP adresi atanamıyor (güvenlik duvarı kurallarını kullanarak erişimi sınırlayabilirsiniz).|Hala kullanılamayan bazı az sayıda SQL Server özelliği vardır.<br/>Hiçbir garantili tam bakım süresi (ama neredeyse şeffaf).<br/>SQL Server sürümüyle uyumluluk yalnızca veritabanı uyumluluk düzeyleri kullanılarak sağlanabilir.|Yedeklerinizi ve yamalarınızı yönetmeniz gerekir.<br>Kendi Yüksek Kullanılabilirlik çözümünüzü uygulamanız gerekir.<br/>Kaynakları değiştirirken bir kesinti vardır (CPU/depolama)|
| 100 TB'a kadar veritabanları. | 8 TB'a kadar. | 256 TB'a kadar depolama alanına sahip SQL Server örnekleri. Örnek gereken sayıda veritabanını destekleyebilir. |
| Şirket içi uygulama, Azure SQL Veritabanı'ndaki verilere erişebilir. | Azure Express Route veya VPN Ağ Geçidi'ni kullanarak [yerel sanal ağ uygulaması](sql-database-managed-instance-vnet-configuration.md) ve şirket ortamınıza bağlantı. | SQL sanal makineleri ile, kısmen bulutta ve kısmen şirket içinde çalışan uygulamalara sahip olabilirsiniz. Örneğin, şirket içi ağınızı ve Active Directory Etki Alanı'nı [Azure Virtual Network](../virtual-network/virtual-networks-overview.md) üzerinden buluta genişletebilirsiniz. Karma bulut çözümleri hakkında daha fazla bilgi için [bkz.](https://docs.microsoft.com/azure/architecture/data-guide/scenarios/hybrid-on-premises-and-cloud) |




## <a name="business-motivations-for-choosing-databases-managed-instances-or-sql-virtual-machines"></a>Veritabanları, yönetilen örnekler veya SQL sanal makineleri seçmek için iş motivasyonları

Farklı veri teklifleri arasında seçim yapma kararınızı etkileyebilecek çeşitli faktörler vardır:

- [Maliyet](#cost) - Hem PaaS hem de IaaS seçeneği, temel altyapı ve lisanslamayı kapsayan taban fiyatı içerir. Ancak, IaaS seçeneği ile veritabanınızı yönetmek için ek zaman ve kaynak yatırımı yapmanız gerekirken, PaaS'ta bu yönetim özelliklerini fiyata dahil edebilirsiniz. IaaS seçeneği, siz bunları kullanmadığınız halde kaynaklarınızı maliyeti azaltmak için kapatmanızı sağlarken, gerekli olduklarında kaynaklarınızı düşürüp yeniden oluşturmadığınız sürece PaaS sürümü her zaman çalışır durumdadır.
- [Yönetim](#administration) - PaaS seçenekleri, veritabanını yönetmek için yatırım yapmanız gereken süreyi azaltır. Ancak, gerçekleştirebileceğiniz veya çalıştırabileceğiniz özel yönetim görevlerinin ve komut dosyalarının aralığını da sınırlar. Örneğin, CLR tek veya havuzlu veritabanları ile desteklenmez, ancak yönetilen bir örnek için desteklenir. Ayrıca, PaaS'ta hiçbir dağıtım seçeneği izleme bayraklarının kullanımını desteklemez.
- [Hizmet Düzeyi Sözleşmesi](#service-level-agreement-sla) - Hem IaaS hem de PaaS yüksek, endüstri standardı SLA sağlar. PaaS seçeneği %99,99 SLA'yı garanti ederken, IaaS altyapı için %99,95 SLA garanti eder, bu da veritabanlarınızın kullanılabilirliğini sağlamak için ek mekanizmalar uygulamanız gerektiği anlamına gelir. VM'de ek bir SQL Server oluşturarak %99,99 oranında Yüksek kullanılabilirlik çözümünü uygulayabilir ve AlwaysOn Kullanılabilirlik gruplarını yapılandırabilirsiniz.
- [Azure](#market) VM'de Azure - SQL Server'a geçme süresi ortamınızın tam eşleşmesi olduğundan, şirket içi Azure SQL VM'ye geçiş, veritabanlarını şirket içi bir sunucudan diğerine taşımaktan farklı değildir. Yönetilen örnek de son derece kolay geçiş sağlar; ancak, yönetilen bir örneğe geçiş yapmadan önce uygulamanız gereken bazı değişiklikler olabilir.

Bu faktörler aşağıdaki bölümlerde daha ayrıntılı olarak ele alınacaktır.

### <a name="cost"></a>Maliyet

İster nakit ihtiyacında olan yeni bir girişim ister sıkı bütçe kısıtlamaları altında işletilen yerleşik bir şirket bünyesindeki bir ekip olun, sınırlı fon genellikle veritabanlarınızın barındırılmasına yönelik kararların verilmesinde en önemli etmendir. Bu bölümde, bu iki ilişkisel veritabanı seçeneğiyle ilgili olarak Azure'daki faturalandırma ve lisanslama temelleri hakkında bilgi edinebilirsiniz: SQL Veritabanı ve SQL sanal makineleri. Ayrıca toplam uygulama maliyetini hesaplama hakkında bilgi edineceksiniz.

#### <a name="billing-and-licensing-basics"></a>Faturalama ve lisanslama ile ilgili temel bilgiler

Şu **anda, SQL Veritabanı** bir hizmet olarak satılır ve çeşitli dağıtım seçenekleri yle ve çeşitli hizmet katmanlarında, kaynaklar için farklı fiyatlarla kullanılabilir ve bunların tümü seçtiğiniz hizmet katmanı ve işlem boyutuna bağlı olarak sabit bir hızda saatlik olarak faturalandırılır. Geçerli desteklenen hizmet katmanları, bilgi işlem boyutları ve depolama tutarları hakkında en son bilgiler için Bkz. [DTU tabanlı satın alma modeli](sql-database-service-tiers-dtu.md) ve [vCore tabanlı satın alma modeli.](sql-database-service-tiers-vcore.md)

- SQL veritabanı ile, temel katman için ayda 5$'tan başlayan geniş bir fiyat yelpazesinden ihtiyaçlarınıza uygun bir hizmet katmanı seçebilirsiniz.
- Maliyetleri azaltmak ve kullanım artışlarını karşılamak için kaynakları veritabanı örnekleri arasında paylaşmak için [elastik havuzlar](sql-database-elastic-pool.md) oluşturabilirsiniz.
- SQL yönetilen örnek ile, aynı zamanda kendi lisans getirebilir. Kendi lisansınızı getir hakkında daha fazla bilgi için [Azure'da Yazılım Güvencesi aracılığıyla Lisans Mobilitesi'ne](https://azure.microsoft.com/pricing/license-mobility/) bakın veya **%40'a kadar tasarruf**etmeyi görmek için Azure Karma Avantajı hesap [makinesini](https://azure.microsoft.com/pricing/hybrid-benefit/#sql-database) kullanın.

Buna ek olarak, giden Internet trafiği için normal [veri aktarımı ücretleriyle](https://azure.microsoft.com/pricing/details/data-transfers/) faturalandırılırsınız. Uygulamanızın çeşitli iş ortası gereksinimlerine uyacak şekilde hizmet katmanlarını ve işlem boyutlarını dinamik olarak ayarlayabilirsiniz.

**SQL veritabanları ve SQL yönetilen örnekleri**ile veritabanı yazılımı otomatik olarak yapılandırılır, yamalı ve Microsoft tarafından yükseltilir, bu da yönetim maliyetlerinizi azaltır. Ayrıca, [yerleşik yedekleme](sql-database-automated-backups.md) özellikleri, özellikle çok sayıda veritabanınız olduğunda önemli maliyet tasarrufları sağlamanıza yardımcı olur.

**SQL sanal makineleri**ile platform tarafından sağlanan SQL Server görüntülerinden herhangi birini (lisans içeren) kullanabilir veya SQL Server lisansınızı getirebilirsiniz. Desteklenen tüm SQL Server sürümleri (2008R2, 2012, 2014, 2016) (Developer, Express, Web, Standard, Enterprise) kullanılabilir. Ayrıca, görüntülerin Kendi Lisansını Getir sürümleri (KLG) mevcuttur. Azure tarafından sağlanan görüntüler kullanıldığında, işletim maliyeti, VM boyutunun yanı sıra seçtiğiniz SQL Server sürümüne bağlıdır. VM boyutu veya SQL Server sürümünden bağımsız olarak, SQL Server ve Windows veya Linux Server'ın dakika başına lisanslama maliyetini ve VM diskler için Azure Depolama maliyetini ödersiniz. Dakika başına faturalandırma seçeneği, SQL Server'ı ek SQL Server lisansları satın almadan istediğiniz süreyle kullanmanıza olanak sağlar. Azure'a kendi SQL Server lisansınızı getirirseniz, yalnızca sunucu ve depolama maliyetleri için ücretlendirilirsiniz. Kendi lisansını getir seçeneği hakkında daha fazla bilgi için bkz. [Azure'da Yazılım Güvencesi ile Lisans Mobilitesi](https://azure.microsoft.com/pricing/license-mobility/). Buna ek olarak, giden Internet trafiği için normal [veri aktarımı ücretleriyle](https://azure.microsoft.com/pricing/details/data-transfers/) faturalandırılırsınız.

#### <a name="calculating-the-total-application-cost"></a>Toplam uygulama maliyetini hesaplama

Bir bulut platformu kullanmaya başladığınızda, uygulamanızı çalıştırmanın maliyeti, yeni geliştirme ve devam eden yönetim maliyetlerinin maliyetini ve genel bulut platformu hizmet maliyetlerini içerir.

Fiyatlandırma konusunda daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [SQL Veritabanı fiyatlandırması](https://azure.microsoft.com/pricing/details/sql-database/)
- [SQL](https://azure.microsoft.com/pricing/details/virtual-machines/#sql) ve [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/#windows) için sanal [makine fiyatlandırması](https://azure.microsoft.com/pricing/details/virtual-machines/)
- [Azure Fiyatlandırma Hesaplayıcısı](https://azure.microsoft.com/pricing/calculator/)

### <a name="administration"></a>Yönetim

Birçok işletme için, bir bulut hizmetine geçiş yapma kararı, maliyetle ilgili olduğu kadar, yönetim karmaşıklığı yükünü gidermekle de ilgilidir. IaaS ve PaaS ile, Microsoft altta yatan altyapıyı yönetir ve olağanüstü durum kurtarma sağlamak için tüm verileri otomatik olarak çoğaltır, veritabanı yazılımını yapılandırır ve yükseltir, yük dengelemeyi yönetir ve bir veri merkezi içinde sunucu hatası.

- **SQL veritabanları ve SQL yönetilen örnekleri**ile veritabanınızı yönetmeye devam edebilirsiniz, ancak artık veritabanı altyapısını, işletim sistemini veya donanımı yönetmeniz gerekmez. Yönetmeye devam edebileceğiniz öğelere örnek olarak veritabanları ve oturum açma bilgileri, dizin ve sorgu ayarları ile denetim ve güvenlik verilebilir. Ayrıca, yüksek kullanılabilirliği başka bir veri merkezine yapılandırmak en az yapılandırma ve yönetim gerektirir.
- **SQL sanal makineleri**ile, işletim sistemi ve SQL Server örnek yapılandırma üzerinde tam kontrole sahip. Bir VM ile, işletim sisteminin ve veritabanı yazılımının ne zaman güncelleştirileceğine/yükseltileceğine ve virüsten koruma araçları gibi herhangi bir ek yazılımın ne zaman yükleneceğine ilişkin karar size aittir. Düzeltme eki uygulama, yedekleme ve yüksek kullanılabilirliği önemli ölçüde kolaylaştırmak amacıyla bazı otomatik özellikler sağlanır. Ayrıca, VM'nin boyutunu, disk sayısını ve disklerin depolama yapılandırmalarını denetleyebilirsiniz. Azure gerektiğinde bir VM’nin boyutunu değiştirmenize izin verir. Bilgi için bkz. [Azure için Sanal Makine ve Bulut Hizmeti Boyutları](../virtual-machines/windows/sizes.md).

### <a name="service-level-agreement-sla"></a>Hizmet Düzeyi Sözleşmesi (SLA)

Birçok BT departmanı için, bir Hizmet Düzeyi Sözleşmesi'nin çalışma süresi yükümlülüklerinin yerine getirilmesi birincil önceliğe sahiptir. Bu bölümde, her bir veritabanı barındırma seçeneği için geçerli olan SLA'yı ele alacağız.

**SQL Veritabanı**için Microsoft, kullanılabilirlik SLA'sı %99,99'luk bir kullanılabilirlik sağlar. En son bilgiler için bkz. [Hizmet Düzeyi Sözleşmesi](https://azure.microsoft.com/support/legal/sla/sql-database/).

**SQL sanal makineler için,** Microsoft sadece Sanal Makine kapsayan% 99,95 bir kullanılabilirlik SLA sağlar. Bu SLA, VM'de çalıştırılan işlemleri (SQL Server gibi) kapsamaz ve bir kullanılabilirlik kümesinde en az iki VM örneğini barındırmanızı gerektirir. En son bilgiler için bkz. [VM SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/). VM'ler içindeki veritabanı yüksek kullanılabilirliği (HA) için, SQL Server'da desteklenen yüksek kullanılabilirlik seçeneklerinden birini yapılandırmanız gerekir( örneğin [Her Zaman Kullanılabilirlik Grupları' nda).](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server) Desteklenen bir yüksek kullanılabilirlik seçeneğinin kullanılması ek bir SLA sağlamaz, ancak %99,99’un üzerinde veritabanı kullanılabilirliği elde etmenize imkan tanır.

### <a name="time-to-move-to-azure"></a><a name="market"></a>Azure'a geçme zamanı

**SQL veritabanı** (tek veritabanları veya elastik havuzlar), geliştirici üretkenliği ve yeni çözümler için pazara hızlı zaman açısından kritik olduğunda bulut tasarımı uygulamalar için doğru çözümdür. Programlama DBA benzeri işlevsellik ile, altta yatan işletim sistemini ve veritabanını yönetmeye yönelik ihtiyacı azalttığından, bulut mimarları ve geliştiricileri için idealdir.

**SQL yönetilen örnek,** varolan uygulamaların Azure SQL'e geçişini büyük ölçüde basitleştirerek, geçirilen veritabanı uygulamalarını Azure'da hızla pazara getirmenizi sağlar.

Mevcut veya yeni uygulamalarınız büyük veritabanları gerektiriyorsa veya SQL Server veya Windows/Linux'taki tüm özelliklere erişim gerektiriyorsa ve yeni şirket içi donanım edinmenin zaman ve masraflarından kaçınmak istiyorsanız, **SQL sanal makineleri** mükemmeldir. Azure SQL Veritabanı yönetilen örneğinin uygun olmadığı durumlarda, varolan şirket içi uygulamaları ve veritabanlarını olduğu gibi Azure'a geçirmek istediğinizde de uygun olur. Sunu, uygulama ve veri katmanlarını değiştirmeniz gerekmediğinden, varolan çözümünüzü yeniden yeniden tasarlamak için zamandan ve bütçeden tasarruf emkişi siniz. Bunun yerine, tüm çözümlerinizin Azure'a geçişini sağlamaya ve Azure platformu tarafından gerekli kılınabilen bazı performans iyileştirmelerini gerçekleştirmeye odaklanabilirsiniz. Daha fazla bilgi için bkz. [Azure Virtual Machines'de SQL Server için En İyi Performans Uygulamaları](../virtual-machines/windows/sql/virtual-machines-windows-sql-performance.md).

[!INCLUDE [sql-database-create-manage-portal](includes/sql-database-create-manage-portal.md)]

## <a name="next-steps"></a>Sonraki adımlar

- SQL Veritabanı'na başlamak için [ilk Azure SQL veritabanınıza](sql-database-single-database-get-started.md) bakın.
- Bkz. [SQL Veritabanı Fiyatlandırması](https://azure.microsoft.com/pricing/details/sql-database/).
- Azure VM'lerinde SQL Server'ı kullanmaya başlamak için bkz. [Azure'da SQL Server sanal makinesi sağlama](../virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision.md).
- [Şirket içi veritabanınız için doğru SQL veritabanını veya SQL yönetilen örneği SKU'yu tanımlayın.](/sql/dma/dma-sku-recommend-sql-db/)
