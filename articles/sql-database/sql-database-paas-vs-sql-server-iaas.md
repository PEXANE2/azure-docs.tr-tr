---
title: Azure 'da SQL seçenekleri arasından seçim yapma | Microsoft Docs
description: Azure SQL veritabanı 'nda ve Azure sanal makineler 'de Azure SQL veritabanı ve SQL Server arasında dağıtım seçenekleri arasından seçim yapma hakkında bilgi edinin
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
ms.date: 03/11/2019
ms.openlocfilehash: fc0204e774e66e9304887f47377a102df1b828cb
ms.sourcegitcommit: aa042d4341054f437f3190da7c8a718729eb675e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/09/2019
ms.locfileid: "68884323"
---
# <a name="choose-the-right-deployment-option-in-azure-sql"></a>Azure SQL 'de doğru dağıtım seçeneğini belirleyin

Azure 'da, SQL Server iş yüklerinizin barındırılan bir altyapıda (IaaS) veya barındırılan hizmet olarak ([PaaS](https://azure.microsoft.com/overview/what-is-paas/)) çalışmasını sağlayabilirsiniz. PaaS 'de, her dağıtım seçeneğinde birden çok dağıtım seçeneğiniz ve hizmet katmanınız vardır. PaaS veya IaaS arasında seçim yaparken sormanız gereken önemli soru, veritabanınızı yönetmek, düzeltme eklerini uygulamak, yedeklemeleri gerçekleştirmek veya bu işlemleri Azure 'a atamak istiyor musunuz?
Yanıta bağlı olarak aşağıdaki seçenekleriniz vardır:

- [Azure SQL veritabanı](sql-database-technical-overview.md): SQL Server en son kararlı kurumsal sürümünü temel alan, tam olarak yönetilen bir SQL veritabanı altyapısı. Bu, *bir hizmet olarak platform (PaaS)* endüstri kategorisine denk gelen Azure bulutu 'nda barındırılan ilişkisel bir hizmet olarak veritabanıdır (DBaaS). SQL veritabanı 'nda, her biri Microsoft tarafından sahip olunan, barındırılan ve korunan, standartlaştırılmış donanım ve yazılım üzerinde oluşturulmuş birden çok dağıtım seçeneği vardır. SQL veritabanı ile, SQL Server (Şirket içinde veya bir Azure sanal makinesinde) kullanılırken kapsamlı yapılandırma gerektiren yerleşik özellikleri ve işlevleri kullanabilirsiniz. SQL Database'i kullandığınızda, kesinti olmadan daha fazla güç için ölçeğinizi artırmaya veya genişletmeye yönelik seçeneklerle kullandıkça ödersiniz. SQL veritabanı, yerleşik yüksek kullanılabilirlik, zeka ve yönetim gibi SQL Server bulunmayan ek özelliklere sahiptir. Azure SQL veritabanı aşağıdaki dağıtım seçeneklerini sunar:
  
  - Tek bir [veritabanı](sql-database-single-database.md) olarak bir SQL veritabanı sunucusu aracılığıyla yönetilen kaynak kümesi olarak. Tek bir veritabanı, SQL Server [Kapsanan veritabanlarına](https://docs.microsoft.com/sql/relational-databases/databases/contained-databases) benzerdir. Bu seçenek, yeni bulut kaynaklı uygulamaların modern uygulama geliştirmesi için iyileştirilmiştir.
  - Bir SQL veritabanı sunucusu aracılığıyla yönetilen ve paylaşılan bir kaynak kümesiyle bir veritabanı koleksiyonu olan [elastik havuz](sql-database-elastic-pool.md). Tek veritabanları, elastik bir havuzun içine ve dışına taşınabilir. Bu seçenek, çok kiracılı SaaS uygulaması kullanılarak yeni bulut kaynaklı uygulamaların modern uygulama geliştirmesi için iyileştirilmiştir.
  - Paylaşılan bir kaynak kümesiyle sistem ve kullanıcı veritabanlarının bir koleksiyonu olan [yönetilen örnek](sql-database-managed-instance.md). Yönetilen bir örnek, veritabanları ve diğer örnek kapsamlı özellikler için paylaşılan kaynaklar sunan [Microsoft SQL Server veritabanı altyapısı] örneğine benzer. Yönetilen örnek, veritabanı değişikliğine sahip olmayan en az ile şirket içi veritabanı geçişini destekler. Bu seçenek, Azure SQL veritabanı 'nın tüm PaaS avantajlarını sağlar, ancak daha önce yalnızca SQL VM 'lerinde bulunan özellikleri ekler. Bu, yerel bir sanal ağ (VNet) ve şirket içi SQL Server uyumluluk% 100 ' ını içerir.
- [Azure sanal makinelerinde SQL Server](https://azure.microsoft.com/services/virtual-machines/sql-server/) , sektör kategorisi *hizmet olarak altyapı (IaaS) olarak* yer alır ve Azure bulutundaki tam olarak yönetilen bir sanal makine içinde SQL Server çalıştırmanızı sağlar. [SQL Server sanal makineler](../virtual-machines/windows/sql/virtual-machines-windows-sql-server-iaas-overview.md) , Microsoft tarafından sahip olunan, barındırılan ve korunan standartlaştırılmış donanımlar üzerinde de çalışır. Bir VM üzerinde SQL Server kullanırken, SQL Server görüntüde zaten bulunan bir SQL Server Lisansı için Kullandıkça Öde veya var olan bir lisansı kolayca kullanabilirsiniz. Ayrıca, gerektiğinde VM 'yi durdurabilir veya sürdürebilirsiniz. Azure 'da çalışan ve hizmet olarak altyapı (IaaS) olarak da bilinen Windows Server veya Linux sanal makinelerinde (VM) bulutta yüklü ve barındırılmış SQL Server. Azure sanal makinelerinde SQL Server, herhangi bir veritabanı değişikliği olmadan şirket içi SQL Server veritabanlarına ve uygulamalarına geçiş yapmak için iyi bir seçenektir. SQL Server tüm yeni sürümleri ve sürümleri IaaS sanal makinesinde yüklenebilir. SQL veritabanından en önemli fark, SQL Server VM 'Lerin veritabanı altyapısı üzerinde tam denetime izin vereceğidir. Bakım/düzeltme eki uygulamayı ne zaman başlatacağınızı, kurtarma modelini basit veya toplu Günlüksüz olarak değiştirmeyi, gerektiğinde hizmeti duraklatmayı veya başlatmayı seçebilirsiniz ve SQL Server veritabanı altyapısını tam olarak özelleştirebilirsiniz. Bu ek denetimle, sanal makineyi yönetmek için eklenen sorumluluğa yer verilir.

Bu seçenekler arasındaki temel farklılıklar aşağıdaki tabloda listelenmiştir:

| VM’de SQL Server | SQL veritabanında yönetilen örnek | SQL veritabanında tek veritabanı/elastik havuz |
| --- | --- | --- |
|SQL Server altyapısı üzerinde tam denetiminiz vardır.<br/>% 99,99 ' e kadar kullanılabilirlik.<br/>Şirket içi SQL Server eşleşen sürümüyle tam eşlik.<br/>Sabit, iyi bilinen Veritabanı Altyapısı sürümü.<br/>Şirket içi SQL Server kolayca geçiş.<br/>Azure sanal ağı içindeki özel IP adresi.<br/>SQL Server yerleştirildiği konakta uygulama veya hizmet dağıtmanıza olanak tanır.| Şirket içi SQL Server yüksek uyumluluk.<br/>% 99,99 kullanılabilirlik garanti edilir.<br/>Yerleşik yedeklemeler, düzeltme eki uygulama.<br/>En son kararlı veritabanı altyapısı sürümü.<br/>SQL Server 'den kolay geçiş.<br/>Azure sanal ağı içindeki özel IP adresi.<br/>Yerleşik Gelişmiş zeka ve güvenlik.<br/>Kaynakların çevrimiçi değişikliği (CPU/depolama).|En yaygın olarak kullanılan SQL Server özellikleri mevcuttur.<br/>% 99,99 kullanılabilirlik garanti edilir.<br/>Yerleşik yedeklemeler, düzeltme eki uygulama.<br/>En son kararlı veritabanı altyapısı sürümü.<br/>Gerekli kaynakları (CPU/depolama) ayrı veritabanlarına atama özelliği.<br/>Yerleşik Gelişmiş zeka ve güvenlik.<br/>Kaynakların çevrimiçi değişikliği (CPU/depolama).|
|Yedeklemelerinizi ve düzeltme eklerini yönetmeniz gerekir.<br>Kendi yüksek kullanılabilirlik çözümünüzü uygulamanız gerekir.<br/>Kaynakları değiştirirken kapalı kalma süresi var (CPU/depolama)|Kullanılabilir olmayan en az sayıda SQL Server özelliği hala vardır.<br/>Garanti eden tam bakım süresi (ancak neredeyse saydam).<br/>SQL Server sürümle uyumluluk, yalnızca veritabanı uyumluluk düzeyleri kullanılarak sağlanabilir.|SQL Server geçiş zor olabilir.<br/>Bazı SQL Server özellikleri kullanılamaz.<br/>Garanti eden tam bakım süresi (ancak neredeyse saydam).<br/>SQL Server sürümle uyumluluk, yalnızca veritabanı uyumluluk düzeyleri kullanılarak sağlanabilir.<br/>Özel IP adresi atanamıyor (güvenlik duvarı kurallarını kullanarak erişimi sınırlayabilirsiniz).|

Her dağıtım seçeneğinin Microsoft veri platformuna nasıl uyduğunu öğrenin ve iş gereksinimlerinizin doğru seçeneğiyle eşleşen yardım alın. Sizin için maliyet tasarrufu ve minimum yönetim tüm diğer unsurlardan önce geliyorsa bu makale, hangi yaklaşımın en fazla önem verdiğiniz iş gereksinimleri açısından en iyi sonucu verdiği konusunda karar vermenize yardımcı olabilir.

## <a name="microsofts-sql-data-platform"></a>Microsoft 'un SQL veri platformu

Azure ile şirket içi SQL Server veritabanlarının karşılaştırmasına yönelik herhangi bir tartışmada anlaşılması gereken ilk unsur, bunların tümünü kullanabileceğinizdir. Microsoft'un veri platformu, SQL Server teknolojisini kullanır ve fiziksel şirket içi makineler, özel bulut ortamları, üçüncü taraflarca barındırılan özel bulut ortamları ve genel bulutta kullanılabilir kılar. Azure sanal makineleri üzerinde SQL Server, söz konusu ortamlar genelinde aynı sunucu ürünleri, geliştirme araçları ve uzmanlık kümesin kullanırken, şirket içi ve bulut üzerinde barındırılan dağıtımların bir birleşimi yoluyla benzersiz ve çeşitli iş gereksinimlerini karşılamanıza olanak sağlar.

   ![Bulut SQL Server seçenekleri: IaaS üzerinde SQL Server veya bulutta SaaS SQL veritabanı.](./media/sql-database-paas-vs-sql-server-iaas/SQLIAAS_SQL_Server_Cloud_Continuum.png)

Diyagramda görüldüğü gibi, her bir teklif, altyapı üzerinde sahip olduğunuz yönetim düzeyine (X ekseni üzerinde) ve veritabanı düzeyi birleştirme ve otomasyon işlemleri ile elde edilen maliyet verimliliği (Y ekseni üzerinde) düzeyine göre belirlenebilir.

Bir uygulama tasarlarken, uygulamanın SQL Server kısmını barındırmak için dört temel seçenek mevcuttur:

- Sanallaştırılmamış fiziksel makinelerde SQL Server
- Şirket içi sanallaştırılmış makinelerde SQL Server (özel bulut)
- Azure Sanal Makine'de SQL Server (Microsoft genel bulut)
- Azure SQL Veritabanı (Microsoft genel bulut)

Aşağıdaki bölümlerde, Microsoft genel bulutundaki SQL Server hakkında bilgi edinirsiniz: Azure SQL veritabanı ve Azure VM 'lerinde SQL Server. Ayrıca, hangi seçeneğin uygulamanız için en uygun olduğunu belirlemek üzere genel iş teşviklerini inceleyeceksiniz.

## <a name="a-closer-look-at-azure-sql-database-and-sql-server-on-azure-vms"></a>Azure SQL Veritabanı ve Azure VM'lerinde SQL Server'a daha ayrıntılı bir bakış

Genellikle, bu iki SQL seçeneği farklı amaçlar için en iyi hale getirilmiştir:

- **Azure SQL Veritabanı**

Birçok veritabanını sağlamak ve yönetmek için genel yönetim maliyetlerini en düşük düzeye düşürmek üzere en iyi duruma getirilmiştir. Herhangi bir sanal makineyi, işletim sistemini veya veritabanı yazılımını yönetmeniz gerekmediğinden, bu, devam eden yönetim maliyetlerini azaltır. Yükseltme, yüksek kullanılabilirlik veya [yedeklemeleri](sql-database-automated-backups.md) yönetmeniz gerekli değildir. Genellikle, Azure SQL Veritabanı tek bir BT veya geliştirme kaynağı tarafından yönetilen veritabanlarının sayısını önemli ölçüde artırabilir. [Elastik havuzlar](sql-database-elastic-pool.md) , kiracı yalıtımı ve veritabanları arasında kaynakları paylaşarak maliyetleri azaltmak için ölçekleyebilme dahil özelliklerle çok kiracılı uygulama mimarilerini de destekler. [Yönetilen örnek](sql-database-managed-instance.md) , mevcut uygulamaların kolay geçişini sağlayan örnek kapsamlı özellikler için destek sağlar ve veritabanları arasında kaynakları destekler.

- **Azure VM 'lerde çalışan SQL Server**

Mevcut uygulamaları Azure 'a geçirmek veya mevcut şirket içi uygulamaları karma dağıtımlarda buluta genişletmek için iyileştirilmiştir. Ayrıca, SQL Server’ı sanal bir makinede kullanarak geleneksel SQL Server uygulamaları geliştirip test edebilirsiniz. Azure VM'lerinde SQL Server ile, adanmış bir SQL Server örneği ve bulut tabanlı bir VM üzerinde tam yönetici haklarına sahip olursunuz. Bu, bir kuruluşun halihazırda sanal makinelerin bakımını yapmak için kullanılabilir BT kaynaklarına sahip olması durumunda ideal bir seçenektir. Bu özellikler uygulamanızın belirli performans ve kullanılabilirlik gereksinimlerini karşılamak üzere yüksek düzeyde özelleştirilmiş bir sistem oluşturmanıza olanak sağlar.

Aşağıdaki tabloda, SQL Database ve Azure VM'lerinde SQL Server'ın temel özellikleri özetlenmektedir:

| | SQL veritabanı tek veritabanları ve elastik havuzlar | SQL veritabanı yönetilen örnekleri |SQL Server sahip Azure sanal makineleri |
| --- | --- | --- |---|
| **En iyi kullanım alanı:** |En son kararlı SQL Server özelliklerini kullanmak ve geliştirme ve pazarlama sırasında zaman kısıtlamalarına sahip olmak isteyen, bulutta tasarlanan yeni uygulamalar. | Yeni uygulamalar veya en son kararlı SQL Server özelliklerini kullanmak isteyen ve en az değişiklikle buluta geçirilen mevcut şirket içi uygulamalar.  | En az değişiklikle veya hiçbir değişikliğe sahip buluta hızlı geçiş gerektiren mevcut uygulamalar. Şirket içi üretim dışı SQL Server donanımı satın almak istemediğinizde hızlı geliştirme ve test senaryoları. |
|  | Veritabanı için yerleşik yüksek kullanılabilirlik, olağanüstü durum kurtarma ve yükseltme mekanizmalarına gereksinim duyan ekipler. | SQL veritabanı tekli ve havuza alınmış veritabanlarıyla aynı. | Yüksek kullanılabilirlik, olağanüstü durum kurtarma ve SQL Server için düzeltme eki uygulama ve yönetme olanağı sunan takımlar. Sağlanan bazı otomatik özellikler bunu önemli ölçüde basitleştirir. |
|  | Altta yatan işletim sistemi ve yapılandırma ayarlarını yönetmek istemeyen ekipler. | SQL veritabanı tekli ve havuza alınmış veritabanlarıyla aynı. | Tam yönetici haklarına sahip özelleştirilmiş bir ortama ihtiyacınız vardır. |
|  | 100 TB 'a kadar veritabanları. | 8 TB 'a kadar. | 256 TB depolama alanına kadar örnek SQL Server. Örnek gereken sayıda veritabanını destekleyebilir. |
| **Kları** | , Çoğu şirket içi veritabanı düzeyi özelliği destekler. | Neredeyse tüm şirket içi örnek düzeyinde ve veritabanı düzeyinde özellikleri destekler. | Tüm şirket içi özellikleri destekler. |
| **Kaynaklar:** | Temel altyapının yapılandırması ve yönetimi için BT kaynaklarını kullanmak istemezsiniz, ancak uygulama katmanına odaklanmak istiyorsunuz. | SQL veritabanı tekli ve havuza alınmış veritabanlarıyla aynı. | Yapılandırma ve yönetim için bazı BT kaynaklarına sahipsiniz. Sağlanan bazı otomatik özellikler bunu önemli ölçüde basitleştirir. |
| **Toplam sahip olma maliyeti:** | Donanım maliyetlerini ortadan kaldırır ve yönetim maliyetlerini azaltır. | SQL veritabanı tekli ve havuza alınmış veritabanlarıyla aynı. | Donanım maliyetlerini ortadan kaldırır. |
| **İş sürekliliği:** |Azure SQL veritabanı, [yerleşik hata toleransı altyapı özelliklerine](sql-database-high-availability.md)ek olarak [otomatik yedeklemeler](sql-database-automated-backups.md), [zaman içinde geri yükleme](sql-database-recovery-using-backups.md#point-in-time-restore), [coğrafi geri yükleme](sql-database-recovery-using-backups.md#geo-restore)ve [etkin coğrafi çoğaltma](sql-database-active-geo-replication.md) [gibi özellikler sağlar. ](sql-database-auto-failover-group.md)İş devamlılığını artırmak için otomatik yük devretme grupları. Daha fazla bilgi için bkz. [SQL Database iş sürekliliğine genel bakış](sql-database-business-continuity.md). | SQL veritabanı tekli ve havuza alınmış veritabanları ile aynı ve yalnızca Kullanıcı tarafından başlatılan, salt kopya yedeklemeleri mevcuttur. | Azure VM’lerde SQL Server, veritabanınızın belirli gereksinimleri için bir yüksek kullanılabilirlik ve olağanüstü durum kurtarma çözümü ayarlamanıza olanak sağlar. Böylece, uygulamanız için en iyi hale getirilmiş bir sisteme sahip olabilirsiniz. Yük devretme işlemlerini ihtiyaç duyulduğunda kendi kendinize test edebilir ve çalıştırabilirsiniz. Daha fazla bilgi için bkz. [Azure Virtual Machines'de SQL Server için Yüksek Kullanılabilirlik ve Olağanüstü Durum Kurtarma](../virtual-machines/windows/sql/virtual-machines-windows-sql-high-availability-dr.md). |
| **Karma bulut:** |Şirket içi uygulamanız, Azure SQL Veritabanı'ndaki verilere erişebilir. | Azure Express Route veya VPN Gateway kullanarak şirket içi ortamınıza [yerel sanal ağ uygulama](sql-database-managed-instance-vnet-configuration.md) ve bağlantı. | Azure VM'lerinde SQL Server ile kısmen bulutta ve kısmen şirket içinde çalıştırılan uygulamalara sahip olabilirsiniz. Örneğin, şirket içi ağınızı ve Active Directory Etki Alanı'nı [Azure Virtual Network](../virtual-network/virtual-networks-overview.md) üzerinden buluta genişletebilirsiniz. Karma bulut çözümleri hakkında daha fazla bilgi için bkz. [Şirket içi veri çözümlerini buluta genişletme](https://docs.microsoft.com/azure/architecture/data-guide/scenarios/hybrid-on-premises-and-cloud). |
|  | Verileri çoğaltmak için abone olarak [SQL Server işlem çoğaltmayı](https://msdn.microsoft.com/library/mt589530.aspx) destekler. | Yönetilen örnek için Önizleme özelliği olarak çoğaltma desteklenir. | [SQL Server işlemsel çoğaltmayı](https://msdn.microsoft.com/library/mt589530.aspx), [her zaman kullanılabilirlik gruplarını](../virtual-machines/windows/sql/virtual-machines-windows-sql-high-availability-dr.md), tümleştirme hizmetlerini ve verileri çoğaltmak için günlük dağıtımını tam olarak destekler. Ayrıca, geleneksel SQL Server yedeklemeleri tam olarak desteklenir |
|  | | |

## <a name="business-motivations-for-choosing-azure-sql-database-or-sql-server-on-azure-vms"></a>Azure VM'lerinde Azure SQL Veritabanı'in veya SQL Server'ın seçilmesine yönelik iş faydaları

SQL veritabanlarınızı barındırmak için PaaS veya IaaS seçme kararlarınızı etkileyebilecek birkaç etken vardır:

- [](#cost) Her Iki PaaS ve IaaS seçeneği, temel altyapıyı ve lisanslamayı kapsayan temel fiyatı içerir. Bununla birlikte, IaaS seçeneği ile veritabanınızı yönetmek için ek saat ve kaynakları yatırım yapmanız gerekir. PaaS 'de, fiyata dahil olan bu yönetim özellikleri elde edersiniz. IaaS seçeneği, kaynakları, ihtiyaç duyulduklarında onları bırakıp yeniden oluşturmadığınız sürece, bu kaynakları bir arada kullanmadığınız sürece, kaynakları kapatmanızı sağlar.
- [Yönetim](#administration) -PaaS seçenekleri, veritabanını yönetmek için yatırım yapmanız gereken süre miktarını azaltır. Ancak, gerçekleştirebileceğiniz veya çalıştırabileceğiniz özel yönetim görevlerinin ve betiklerin aralığını da sınırlandırır. Örneğin, CLR tek veya havuza alınmış veritabanlarıyla desteklenmez, ancak yönetilen bir örnek için desteklenir. Ayrıca PaaS 'deki dağıtım seçenekleri, izleme bayraklarının kullanımını desteklemez.
- [Hizmet düzeyi sözleşmesi](#service-level-agreement-sla) -IaaS ve PaaS, yüksek ENDÜSTRI standardı SLA sağlar. PaaS seçeneği% 99,99 SLA 'sını garanti ederken IaaS, altyapı için% 99,95 SLA 'sını garanti ederken, veritabanlarının kullanılabilirliğini sağlamak için ek mekanizmalar uygulamanız gerekir. VM 'de ek bir SQL Server oluşturup AlwaysOn kullanılabilirlik gruplarını yapılandırabilmeniz için% 99,99 ' de yüksek kullanılabilirlik çözümü uygulayabilirsiniz.
- Azure VM 'de SQL Server [Azure 'a geçme süresi](#market) ortamınızın tam eşleşmesinden itibaren, şirket Içi SUNUCUDAN Azure SQL VM 'sine geçiş, veritabanlarını şirket içi bir sunucudan diğerine taşımadan farklı değildir. Yönetilen örnek, son derece kolay geçişe de izin verebilir; Ancak, yönetilen bir örneğe geçirmeden önce uygulamanız gereken bazı değişiklikler olabilir.

Bu faktörler aşağıdaki bölümlerde daha ayrıntılı olarak ele alınacaktır.

### <a name="cost"></a>Maliyet

İster nakit ihtiyacında olan yeni bir girişim ister sıkı bütçe kısıtlamaları altında işletilen yerleşik bir şirket bünyesindeki bir ekip olun, sınırlı fon genellikle veritabanlarınızın barındırılmasına yönelik kararların verilmesinde en önemli etmendir. Bu bölümde, bu iki ilişkisel veritabanı seçeneğine göre Azure 'da faturalandırma ve lisans temelleri hakkında bilgi edineceksiniz: SQL veritabanı ve Azure VM 'lerinde SQL Server. Ayrıca toplam uygulama maliyetini hesaplama hakkında bilgi edineceksiniz.

#### <a name="billing-and-licensing-basics"></a>Faturalama ve lisanslama ile ilgili temel bilgiler

Şu anda **SQL veritabanı** bir hizmet olarak satılır ve birkaç dağıtım seçeneği ve çeşitli hizmet katmanlarında kullanılabilir ve bu işlemler, her biri, seçtiğiniz hizmet katmanına ve işlem boyutuna bağlı olarak sabit bir fiyata faturalandırılır. . Desteklenen geçerli hizmet katmanları, işlem boyutları ve depolama miktarları hakkında en son bilgiler için bkz. [DTU tabanlı satın alma modeli](sql-database-service-tiers-dtu.md) ve [sanal çekirdek tabanlı satın alma modeli](sql-database-service-tiers-vcore.md).

- SQL veritabanı tek veritabanı ile, temel katman için 5 $/aydan itibaren çok çeşitli fiyatlarla ihtiyaçlarınıza uyan bir hizmet katmanını seçebilirsiniz.
- Maliyetleri azaltmak ve kullanım artışlarını karşılamak için, veritabanı örnekleri arasında kaynak paylaşmak üzere [elastik havuzlar](sql-database-elastic-pool.md) oluşturabilirsiniz.
- SQL veritabanı yönetilen örneği ile kendi lisansınızı da getirebilirsiniz. Kendi lisansını getir hakkında daha fazla bilgi için bkz. [Azure 'da Yazılım Güvencesiyle Lisans Taşınabilirliği](https://azure.microsoft.com/pricing/license-mobility/) veya [Azure hibrit avantajı hesaplayıcıyı](https://azure.microsoft.com/pricing/hybrid-benefit/#sql-database) kullanarak **% 40**' a nasıl tasarruf etmek gerektiğini öğrenin.

Buna ek olarak, giden Internet trafiği için normal [veri aktarımı ücretleriyle](https://azure.microsoft.com/pricing/details/data-transfers/) faturalandırılırsınız. Hizmet katmanlarını ve işlem boyutlarını, uygulamanızın değişen aktarım hızı gereksinimleriyle eşleşecek şekilde dinamik olarak ayarlayabilirsiniz.

**SQL Database** ile, veritabanı yazılımına Microsoft tarafından otomatik olarak yapılandırma, düzeltme eki ve yükseltme uygulanır; bu da yönetim maliyetlerinizi azaltır. Ayrıca, [yerleşik yedekleme](sql-database-automated-backups.md) özellikleri, özellikle çok sayıda veritabanınız olduğunda önemli maliyet tasarrufları sağlamanıza yardımcı olur.

**Azure VM’lerde SQL Server** ile platform tarafından sağlanan SQL Server görüntülerinin herhangi birini (lisans içerir) kullanabilir veya kendi SQL Server lisansınızı getirebilirsiniz. Desteklenen tüm SQL Server sürümleri (2008R2, 2012, 2014, 2016) (Developer, Express, Web, Standard, Enterprise) kullanılabilir. Ayrıca, görüntülerin Kendi Lisansını Getir sürümleri (KLG) mevcuttur. Azure tarafından sağlanan görüntüler kullanıldığında, işletim maliyeti, VM boyutunun yanı sıra seçtiğiniz SQL Server sürümüne bağlıdır. VM boyutundan veya SQL Server sürümünden bağımsız olarak, VM diskleri için Azure depolama maliyetiyle birlikte, SQL Server ve Windows veya Linux sunucusu için dakikalık lisanslama ücreti ödersiniz. Dakika başına faturalandırma seçeneği, SQL Server'ı ek SQL Server lisansları satın almadan istediğiniz süreyle kullanmanıza olanak sağlar. Azure 'a kendi SQL Server lisansınızı getirdiğiniz takdirde, yalnızca sunucu ve depolama maliyetleri için ücretlendirilirsiniz. Kendi lisansını getir seçeneği hakkında daha fazla bilgi için bkz. [Azure'da Yazılım Güvencesi ile Lisans Mobilitesi](https://azure.microsoft.com/pricing/license-mobility/). Buna ek olarak, giden Internet trafiği için normal [veri aktarımı ücretleriyle](https://azure.microsoft.com/pricing/details/data-transfers/) faturalandırılırsınız.

#### <a name="calculating-the-total-application-cost"></a>Toplam uygulama maliyetini hesaplama

Bulut platformunu kullanmaya başladığınızda, uygulamanızı çalıştırmanın maliyeti yeni geliştirme ve devam eden yönetim maliyetlerinin yanı sıra genel bulut platformu hizmet maliyetlerine dahil olmak üzere maliyeti içerir.

**Azure SQL Veritabanı kullanıldığında:**

- Yüksek oranda küçültülmüş yönetim maliyetleri
- Geçirilen uygulamalar için sınırlı geliştirme maliyetleri (yönetilen örnekler)
- SQL veritabanı hizmet maliyetleri
- Donanım satın alma maliyeti yok

**Azure VM'lerinde SQL Server kullanıldığında:**

- Daha yüksek yönetim maliyetleri
- Geçirilen uygulamalar için geliştirme maliyeti olmadan sınırlı
- Sanal makine hizmet maliyetleri
- Donanım satın alma maliyeti yok

Fiyatlandırma konusunda daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [SQL veritabanı fiyatlandırması](https://azure.microsoft.com/pricing/details/sql-database/)
- [SQL](https://azure.microsoft.com/pricing/details/virtual-machines/#sql) ve [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/#windows) için [sanal makine fiyatlandırması](https://azure.microsoft.com/pricing/details/virtual-machines/)
- [Azure Fiyatlandırma Hesaplayıcısı](https://azure.microsoft.com/pricing/calculator/)

### <a name="administration"></a>Yönetim

Birçok işletme için, bir bulut hizmetine geçiş yapma kararı, maliyetle ilgili olduğu kadar, yönetim karmaşıklığı yükünü gidermekle de ilgilidir. IaaS ve PaaS ile, Microsoft temel altyapıyı yönetir ve olağanüstü durum kurtarma sağlamak, veritabanı yazılımını yapılandırır ve yükseltir, yük dengelemeyi yönetir ve bir bir veri merkezinde sunucu hatası.

- **Azure SQL veritabanı**ile veritabanınızı yönetmeye devam edebilirsiniz, ancak artık veritabanı altyapısını, işletim sistemini veya donanımı yönetmeniz gerekmez. Yönetmeye devam edebileceğiniz öğelere örnek olarak veritabanları ve oturum açma bilgileri, dizin ve sorgu ayarları ile denetim ve güvenlik verilebilir. Ayrıca, yüksek kullanılabilirliği başka bir veri merkezine yapılandırmak, en az yapılandırma ve yönetim gerektirir.
- **Azure VM’lerde SQL Server** ile işletim sistemi ve SQL Server örneği yapılandırması üzerinde tam denetime sahip olursunuz. Bir VM ile, işletim sisteminin ve veritabanı yazılımının ne zaman güncelleştirileceğine/yükseltileceğine ve virüsten koruma araçları gibi herhangi bir ek yazılımın ne zaman yükleneceğine ilişkin karar size aittir. Düzeltme eki uygulama, yedekleme ve yüksek kullanılabilirliği önemli ölçüde kolaylaştırmak amacıyla bazı otomatik özellikler sağlanır. Ayrıca, VM'nin boyutunu, disk sayısını ve disklerin depolama yapılandırmalarını denetleyebilirsiniz. Azure gerektiğinde bir VM’nin boyutunu değiştirmenize izin verir. Bilgi için bkz. [Azure için Sanal Makine ve Bulut Hizmeti Boyutları](../virtual-machines/windows/sizes.md).

### <a name="service-level-agreement-sla"></a>Hizmet Düzeyi Sözleşmesi (SLA)

Birçok BT departmanı için, bir Hizmet Düzeyi Sözleşmesi'nin çalışma süresi yükümlülüklerinin yerine getirilmesi birincil önceliğe sahiptir. Bu bölümde, her bir veritabanı barındırma seçeneği için geçerli olan SLA'yı ele alacağız.

**SQL veritabanı**için Microsoft,% 99,99 oranında bir kullanılabilirlik SLA 'sı sağlar. En son bilgiler için bkz. [Hizmet Düzeyi Sözleşmesi](https://azure.microsoft.com/support/legal/sla/sql-database/).

**Azure VM'lerinde çalıştırılan SQL Server** için Microsoft, yalnızca Sanal Makine'yi kapsayan %99,95 oranında bir kullanılabilirlik SLA'sı sağlar. Bu SLA, VM'de çalıştırılan işlemleri (SQL Server gibi) kapsamaz ve bir kullanılabilirlik kümesinde en az iki VM örneğini barındırmanızı gerektirir. En son bilgiler için bkz. [VM SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/). VM 'Ler içindeki veritabanı yüksek kullanılabilirlik (HA) için, [her zaman açık kullanılabilirlik grupları](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server)gibi SQL Server desteklenen yüksek kullanılabilirlik seçeneklerinden birini yapılandırmalısınız. Desteklenen bir yüksek kullanılabilirlik seçeneğinin kullanılması ek bir SLA sağlamaz, ancak %99,99’un üzerinde veritabanı kullanılabilirliği elde etmenize imkan tanır.

### <a name="market"></a>Azure 'a geçiş süresi

**SQL veritabanı tek veritabanları veya elastik havuzlar** , geliştirici üretkenliği ve yeni çözümler için hızlı pazarlama süresi kritik olduğunda bulutta tasarlanan uygulamalar için doğru çözümdür. Programlama DBA benzeri işlevsellik ile, altta yatan işletim sistemini ve veritabanını yönetmeye yönelik ihtiyacı azalttığından, bulut mimarları ve geliştiricileri için idealdir.

**SQL veritabanı yönetilen örneği** , mevcut UYGULAMALARıN Azure SQL veritabanı 'na geçirilmesini büyük ölçüde basitleştirir, böylece geçirilmiş veritabanı uygulamalarını Azure 'da hızla pazara sunmanıza olanak tanır.

**Azure VM 'lerde çalışan SQL Server** , mevcut veya yeni uygulamalarınızın büyük veritabanları gerektirmesi veya SQL Server ya da Windows/Linux 'taki tüm özelliklere erişimi olması ve yeni şirket içi donanım alma süresini ve harcamasını önlemek istiyorsanız idealdir. Ayrıca, mevcut şirket içi uygulamaları ve veritabanlarını Azure SQL veritabanı yönetilen örneği iyi bir uygun olmadığı durumlarda Azure 'a geçirmek istediğinizde de iyi bir uyum sağlar. Sunu, uygulama ve veri katmanlarını değiştirmenize gerek olmadığından, mevcut çözümünüzü yeniden mimarmaya göre zaman ve bütçe tasarrufu yapabilirsiniz. Bunun yerine, tüm çözümlerinizin Azure'a geçişini sağlamaya ve Azure platformu tarafından gerekli kılınabilen bazı performans iyileştirmelerini gerçekleştirmeye odaklanabilirsiniz. Daha fazla bilgi için bkz. [Azure Virtual Machines'de SQL Server için En İyi Performans Uygulamaları](../virtual-machines/windows/sql/virtual-machines-windows-sql-performance.md).

## <a name="next-steps"></a>Sonraki adımlar

- SQL Veritabanı’nı kullanmaya başlamak için bkz. [İlk Azure SQL Veritabanınız](sql-database-single-database-get-started.md).
- Bkz. [SQL Veritabanı Fiyatlandırması](https://azure.microsoft.com/pricing/details/sql-database/).
- Azure VM'lerinde SQL Server'ı kullanmaya başlamak için bkz. [Azure'da SQL Server sanal makinesi sağlama](../virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision.md).
- Şirket [içi veritabanınız için doğru Azure SQL veritabanı/yönetilen örnek SKU 'Sunu belirler](/sql/dma/dma-sku-recommend-sql-db/).
