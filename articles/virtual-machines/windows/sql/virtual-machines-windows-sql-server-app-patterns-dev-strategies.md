---
title: VM'lerde SQL Server Uygulama Desenleri | Microsoft Dokümanlar
description: Bu makale, Azure VM'lerde SQL Server için uygulama desenleri kapsar. Bu çözüm mimarlar ve geliştiriciler iyi uygulama mimarisi ve tasarımı için bir temel sağlar.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: jroth
editor: ''
tags: azure-service-management,azure-resource-manager
ms.assetid: 41863c8d-f3a3-4584-ad86-b95094365e05
ms.service: virtual-machines-sql
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 05/31/2017
ms.author: mathoma
ms.openlocfilehash: cbc2bfbb68910c3eb12352bebb575c4548885a24
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "70124022"
---
# <a name="application-patterns-and-development-strategies-for-sql-server-in-azure-virtual-machines"></a>Azure Sanal Makineler'de SQL Server için Uygulama Desenleri ve Geliştirme Stratejileri
[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="summary"></a>Özet:
Azure ortamındaki SQL-Server tabanlı uygulamalarınız için hangi uygulama deseninin veya desenlerinin kullanılacağını belirlemek önemli bir tasarım kararıdır ve SQL Server ve Azure'un her altyapı bileşeninin birlikte nasıl çalıştığının sağlam bir şekilde anlaşılmasını gerektirir. Azure Altyapı Hizmetleri'ndeki SQL Server ile, Windows Server'da yerleşik olarak bulunan sql server uygulamalarınızı Azure'daki sanal makinelere kolayca aktarabilir, bakımını yapabilir ve izleyebilirsiniz.

Bu makalenin amacı, çözüm mimarlarına ve geliştiricilere, varolan uygulamaları Azure'a geçirerek ve Azure'da yeni uygulamalar geliştirirken izleyebilecekleri iyi bir uygulama mimarisi ve tasarımı için bir temel sağlamaktır.

Her uygulama deseni için şirket içi bir senaryo, ilgili bulut özellikli çözümü ve ilgili teknik öneriler bulacaksınız. Ayrıca makalede, uygulamalarınızı doğru bir şekilde tasarlayabilmeniz için Azure'a özgü geliştirme stratejileri açıklanmaktadır. Birçok olası uygulama deseni nedeniyle, mimarların ve geliştiricilerin uygulamaları ve kullanıcıları için en uygun deseni seçmeleri önerilir.

**Teknik Katkıda Bulunanlar:** Luis Carlos Vargas Ringa Balığı, Madhan Arumugam Ramakrishnan

**Teknik Gözden Geçirenler:** Corey Sanders, Drew McDaniel, Narayan Annamalai, Nir Mashkowski, Sanjay Mishra, Silvano Coriani, Stefan Schackow, Tim Hickey, Tim Wieman, Xin Jin

## <a name="introduction"></a>Giriş
Farklı makinelerdeki ve farklı bileşenlerdeki farklı uygulama katmanlarının bileşenlerini ayırarak birçok n katmanı uygulama türü geliştirebilirsiniz. Örneğin, istemci uygulama ve iş kuralları bileşenlerini bir makineye, ön uç web katmanına ve veri erişim katmanı bileşenlerini başka bir makineye ve bir arka uç veritabanı katmanını başka bir makineye yerleştirebilirsiniz. Bu tür bir yapılanma, her katmanı birbirinden izole etmeye yardımcı olur. Verilerin geldiği yeri değiştirirseniz, istemciyi veya web uygulamasını değil, yalnızca veri erişim katmanı bileşenlerini değiştirmeniz gerekmez.

Tipik bir *n katmanı* uygulaması sunu katmanını, iş katmanını ve veri katmanını içerir:

| Katman | Açıklama |
| --- | --- |
| **Sunum** |*Sunu katmanı* (web katmanı, ön uç katmanı), kullanıcıların bir uygulamayla etkileşimde bulunduğu katmandır. |
| **İş** |*İş katmanı* (orta katman), sunu katmanı nın ve veri katmanının birbirleriyle iletişim kurmak için kullandığı katmandır ve sistemin temel işlevselliğini içerir. |
| **Veri** |*Veri katmanı* temelde bir uygulamanın verilerini depolayan sunucudur (örneğin, SQL Server çalıştıran bir sunucu). |

Uygulama katmanları, bir uygulamadaki işlevsellik ve bileşenlerin mantıksal gruplandırmalarını açıklar; katmanlar ise işlevselliğin ve bileşenlerin ayrı fiziksel sunucularda, bilgisayarlarda, ağlarda veya uzak konumlarda fiziksel dağılımını açıklar. Bir uygulamakatmanları aynı fiziksel bilgisayarda (aynı katmanda) bulunabilir veya ayrı bilgisayarlara (n katmanı) dağıtılabilir ve her katmandaki bileşenler iyi tanımlanmış arabirimler aracılığıyla diğer katmanlarda bileşenlerle iletişim kurabilir. Katman terimini iki katmanlı, üç katmanlı ve n katmanlı gibi fiziksel dağılım kalıplarına atıfta bulunarak düşünebilirsiniz. **2 katmanlı uygulama deseni** iki uygulama katmanı içerir: uygulama sunucusu ve veritabanı sunucusu. Doğrudan iletişim uygulama sunucusu ve veritabanı sunucusu arasında olur. Uygulama sunucusu hem web katmanı hem de iş katmanı bileşenleri içerir. **3 katmanlı uygulama deseninde**üç uygulama katmanı vardır: web sunucusu, iş mantığı katmanı ve/veya iş katmanı veri erişim bileşenlerini içeren uygulama sunucusu ve veritabanı sunucusu. Web sunucusu ve veritabanı sunucusu arasındaki iletişim uygulama sunucusu üzerinden gerçekleşir. Uygulama katmanları ve katmanları hakkında ayrıntılı bilgi için [Microsoft Uygulama Mimarisi Kılavuzu'na](https://msdn.microsoft.com/library/ff650706.aspx)bakın.

Bu makaleyi okumaya başlamadan önce, SQL Server ve Azure'un temel kavramları hakkında bilgi sahibi olmalısınız. Daha fazla bilgi için Azure [Sanal Makinelerde SQL](virtual-machines-windows-sql-server-iaas-overview.md) Server Books [Online](https://msdn.microsoft.com/library/bb545450.aspx), SQL Server ve [Azure.com](https://azure.microsoft.com/)bakın.

Bu makalede, basit uygulamalar yanı sıra son derece karmaşık kurumsal uygulamalar için uygun olabilir çeşitli uygulama desenleri açıklanır. Her deseni ayrıntılandırmadan önce, [Azure Depolama, Azure](../../../storage/common/storage-introduction.md)SQL [Veritabanı](../../../sql-database/sql-database-technical-overview.md)ve SQL Server gibi Azure'daki kullanılabilir veri depolama hizmetlerini [azure sanal makinede](virtual-machines-windows-sql-server-iaas-overview.md)tanımanızı öneririz. Uygulamalarınız için en iyi tasarım kararlarını vermek için, hangi veri depolama hizmetini ne zaman kullanacağınızı net bir şekilde anlayın.

### <a name="choose-sql-server-in-an-azure-virtual-machine-when"></a>Azure Sanal Makine'de SQL Server'ı seçin:
* SQL Server ve Windows üzerinde denetime ihtiyacınız vardır. Örneğin, bu SQL Server sürümü, özel düzeltmeler, performans yapılandırması, vb içerebilir.
* SQL Server ile şirket içinde tam bir uyumluluğa ihtiyacınız vardır ve varolan uygulamaları olduğu gibi Azure'a taşımak istiyorsunuz.
* Azure ortamının özelliklerinden yararlanmak istiyorsunuz, ancak Azure SQL Veritabanı uygulamanızın gerektirdiği tüm özellikleri desteklemez. Bu, aşağıdaki alanları içerebilir:
  
  * **Veritabanı boyutu**: Bu makalenin güncelleştirici olduğu tarihte, SQL Veritabanı en fazla 1 TB veri veritabanını destekler. Uygulamanız 1 TB'den fazla veri gerektiriyorsa ve özel parçalama çözümleri uygulamak istemiyorsanız, SQL Server'ı bir Azure Sanal Makinesi'nde kullanmanız önerilir. En son bilgiler için Azure [SQL Veritabanını Ölçekleme](https://msdn.microsoft.com/library/azure/dn495641.aspx), [DTU Tabanlı Satın Alma Modeli](../../../sql-database/sql-database-service-tiers-dtu.md)ve [vCore Tabanlı Satın Alma Modeli](../../../sql-database/sql-database-service-tiers-vcore.md)(önizleme) bakın.
  * **HIPAA uyumluluğu**: Bir Azure Sanal Makinedeki SQL Server HIPAA Business Associate Agreement (BAA) kapsamında olduğundan, sağlık müşterileri ve Bağımsız Yazılım Satıcıları (ISV' ler) [Azure Sanal](../../../sql-database/sql-database-technical-overview.md) [Makineleryerine Sql Server'ı](virtual-machines-windows-sql-server-iaas-overview.md) Azure Sanal Makinelerde seçebilir. Uyumluluk hakkında bilgi için Bkz. [Microsoft Azure Güven Merkezi: Uyumluluk.](https://azure.microsoft.com/support/trust-center/compliance/)
  * **Örnek düzeyindeki özellikler**: Şu anda SQL Veritabanı veritabanı dışında yaşayan özellikleri desteklemez (Bağlantılı Sunucular, Aracı işleri, FileStream, Servis Aracısı vb.) Daha fazla bilgi için Azure [SQL Veritabanı Yönergeleri ve Sınırlamaları'na](https://msdn.microsoft.com/library/azure/ff394102.aspx)bakın.

## <a name="1-tier-simple-single-virtual-machine"></a>1 katmanlı (basit): tek sanal makine
Bu uygulama deseninde, SQL Server uygulamanızı ve veritabanınızı Azure'da bağımsız bir sanal makineye dağıtirsınız. Aynı sanal makine istemcinizi/web uygulamanızı, iş bileşenlerinizi, veri erişim katmanınızı ve veritabanı sunucunuzu içerir. Sunu, iş ve veri erişim kodu mantıksal olarak ayrılır, ancak fiziksel olarak tek sunuculu bir makinede bulunur. Çoğu müşteri bu uygulama deseni ile başlar ve daha sonra, kendi sistemine daha fazla web rolleri veya sanal makineler ekleyerek ölçeklendirilir.

Bu uygulama deseni aşağıdaki nde yararlıdır:

* Platformun uygulamanızın gereksinimlerini yanıtlayıp yanıtlamayacağını değerlendirmek için Azure platformuna basit bir geçiş gerçekleştirmek istiyorsunuz.
* Katmanlar arasındaki gecikme süresini azaltmak için, aynı sanal makinede barındırılan tüm uygulama katmanlarını aynı Azure veri merkezinde tutmak istiyorsunuz.
* Geliştirme ve test ortamlarını kısa süreler için hızlı bir şekilde sağlamak istiyorsunuz.
* Değişen iş yükü düzeyleri için stres testi yapmak istiyorsunuz, ancak aynı zamanda her zaman birçok fiziksel makineye sahip olmak ve bunları sürdürmek istemiyorsunuz.

Aşağıdaki diyagram, basit bir şirket içi senaryoyu ve bulut özellikli çözümünü Azure'daki tek bir sanal makinede nasıl dağıtabileceğinizi gösterir.

![1 katmanlı uygulama deseni](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728008.png)

Ölçeklenebilirlik veya güvenlik sorunları nedeniyle ayrı bir katman kullanmanız gerekmediği sürece, iş katmanını (iş mantığı ve veri erişim bileşenleri) sunu katmanıyla aynı fiziksel katmanda dağıtmak uygulama performansını en üst düzeye çıkarabilir.

Bu, başlangıçta çok yaygın bir desen olduğundan, verilerinizi SQL Server VM'nize taşımak için geçişle ilgili aşağıdaki makaleyi yararlı bulabilirsiniz: [Bir Veritabanını Azure VM'de SQL Server'a geçirmek.](virtual-machines-windows-migrate-sql.md)

## <a name="3-tier-simple-multiple-virtual-machines"></a>3 katmanlı (basit): birden fazla sanal makine
Bu uygulama deseninde, her uygulama katmanını farklı bir sanal makineye yerleştirerek Azure'da 3 katmanlı bir uygulama dağıtabilirsiniz. Bu, kolay ölçeklendirme ve ölçeklendirme senaryoları için esnek bir ortam sağlar. Sanal bir makine istemci/web uygulamanızı içeriyorsa, diğeri iş bileşenlerinizi, diğeri ise veritabanı sunucusunu barındırar.

Bu uygulama deseni aşağıdaki nde yararlıdır:

* Karmaşık veritabanı uygulamalarının Azure Sanal Makineleri'ne geçişini gerçekleştirmek istiyorsunuz.
* Farklı uygulama katmanlarının farklı bölgelerde barındırılmasını istiyorsunuz. Örneğin, raporlama amacıyla birden çok bölgeye dağıtılan veritabanlarını paylaşmış olabilirsiniz.
* Kurumsal uygulamaları şirket içi sanallaştırılmış platformlardan Azure Sanal Makineleri'ne taşımak istiyorsunuz. Kurumsal uygulamalar hakkında ayrıntılı bir tartışma için, [Kurumsal Uygulama nedir'e](https://msdn.microsoft.com/library/aa267045.aspx)bakın.
* Geliştirme ve test ortamlarını kısa süreler için hızlı bir şekilde sağlamak istiyorsunuz.
* Değişen iş yükü düzeyleri için stres testi yapmak istiyorsunuz, ancak aynı zamanda her zaman birçok fiziksel makineye sahip olmak ve bunları sürdürmek istemiyorsunuz.

Aşağıdaki diyagram, her uygulama katmanını farklı bir sanal makineye yerleştirerek Azure'a nasıl basit bir 3 katmanlı uygulama yerleştirebileceğinizi gösterir.

![3 katmanlı uygulama deseni](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728009.png)

Bu uygulama deseninde, her katmanda yalnızca bir sanal makine (VM) vardır. Azure'da birden çok Sanal M'niz varsa, sanal bir ağ kurmanızı öneririz. [Azure Sanal Ağı](../../../virtual-network/virtual-networks-overview.md) güvenilir bir güvenlik sınırı oluşturur ve Sanal M'lerin özel IP adresi üzerinden kendi aralarında iletişim kurmasına olanak tanır. Ayrıca, her zaman tüm Internet bağlantılarının yalnızca sunu katmanına gittiğinden emin olun. Bu uygulama deseni takip ederken, erişimi denetlemek için ağ güvenlik grubu kurallarını yönetin. Daha fazla bilgi için bkz. [Azure portalını kullanarak VM'nize harici erişime izin ver.](../nsg-quickstart-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

Diyagramda, Internet Protokolleri TCP, UDP, HTTP veya HTTPS olabilir.

> [!NOTE]
> Azure'da sanal ağ kurmak ücretsizdir. Ancak, şirket içi bağlantı sağlayan VPN ağ geçidi için ücretlendirilirsiniz. Bu ücret, bağlantının sağlanıp kullanılabilen süreye bağlıdır.
> 
> 

## <a name="2-tier-and-3-tier-with-presentation-tier-scale-out"></a>Sunum katmanı ölçeği ile 2 katmanlı ve 3 katmanlı
Bu uygulama deseninde, her uygulama katmanını farklı bir sanal makineye yerleştirerek Azure Sanal Makineleri'ne 2 katmanlı veya 3 katmanlı veritabanı uygulaması dağıtMış sınız. Ayrıca, gelen istemci isteklerinin artan hacmi nedeniyle sunu katmanını ölçeklendirirsiniz.

Bu uygulama deseni aşağıdaki nde yararlıdır:

* Kurumsal uygulamaları şirket içi sanallaştırılmış platformlardan Azure Sanal Makineleri'ne taşımak istiyorsunuz.
* Gelen istemci isteklerinin artan hacmi nedeniyle sunu katmanını ölçeklendirmek istiyorsunuz.
* Geliştirme ve test ortamlarını kısa süreler için hızlı bir şekilde sağlamak istiyorsunuz.
* Değişen iş yükü düzeyleri için stres testi yapmak istiyorsunuz, ancak aynı zamanda her zaman birçok fiziksel makineye sahip olmak ve bunları sürdürmek istemiyorsunuz.
* İsteğe bağlı olarak ölçeklendirilebilen bir altyapı ortamına sahip olmak istiyorsunuz.

Aşağıdaki diyagram, gelen istemci isteklerinin artan hacmi nedeniyle sunu katmanını ölçekleyerek uygulama katmanlarını Azure'daki birden çok sanal makineye nasıl yerleştirebileceğinizi göstermektedir. Diyagramda görüldüğü gibi, Azure Yük Dengeleyicisi trafiği birden çok sanal makineye dağıtmakve hangi web sunucusuna bağlanışlayacaklarını belirlemekten sorumludur. Bir yük dengeleyiciarkasında web sunucularının birden fazla örneği olması, sunu katmanının yüksek kullanılabilirliğini sağlar.

![Uygulama deseni - sunum katmanı ölçeği](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728010.png)

### <a name="best-practices-for-2-tier-3-tier-or-n-tier-patterns-that-have-multiple-vms-in-one-tier"></a>Bir katmanda birden fazla VM'si olan 2 katmanlı, 3 katmanlı veya n katmanlı desenler için en iyi uygulamalar
Aynı katmana ait sanal makineleri aynı bulut hizmetine ve aynı kullanılabilirlik kümesine yerleştirmeniz önerilir. Örneğin, **CloudService1** ve **AvailabilitySet1'e** bir web sunucusu kümesi ve **CloudService2** ve **AvailabilitySet2'deki**veritabanı sunucuları kümesini yerleştirin. Azure'da kullanılabilirlik kümesi, yüksek kullanılabilirlik düğümlerini ayrı hata etki adlarına yerleştirmenize ve etki alanlarını yükseltmenize olanak tanır.

Bir katmanın birden çok VM örneğinden yararlanmak için Azure Yük Dengeleyicisini uygulama katmanları arasında yapılandırmanız gerekir. Yük Dengeleyicisini her katmanda yapılandırmak için, her kademenin VM'lerinde ayrı ayrı yük dengeli bir uç noktası oluşturun. Belirli bir katman için önce aynı bulut hizmetinde VM'ler oluşturun. Bu, aynı genel Sanal IP adresine sahip olmalarını sağlar. Ardından, o katmandaki sanal makinelerden birinde bir bitiş noktası oluşturun. Ardından, yük dengeleme için bu katmandaki diğer sanal makinelere aynı bitiş noktasını atayın. Yük dengeli bir küme oluşturarak, trafiği birden çok sanal makineye dağıtır ve arka uçv düğümü başarısız olduğunda Yük Dengeleyicisinin hangi düğümün bağlanaca bağlanabağlanmasını belirlemesine izin verirsiniz. Örneğin, bir yük dengeleyiciarkasında web sunucularının birden çok örneği olması, sunu katmanının yüksek kullanılabilirliğini sağlar.

En iyi uygulama olarak, her zaman tüm internet bağlantılarının önce sunum katmanına gittiğinden emin olun. Sunu katmanı iş katmanına erişir ve işletme katmanı veri katmanına erişir. Sunu katmanına erişime nasıl izin vereceğimiz hakkında daha fazla bilgi için [bkz.](../nsg-quickstart-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

Azure'daki Yük Dengeleyici'nin şirket içi bir ortamda yük bakiyerlerine benzer şekilde çalıştığını unutmayın. Daha fazla bilgi için Azure [altyapı hizmetleri için Yük dengelemesi'ne](../tutorial-load-balancer.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)bakın.

Ayrıca, Azure Sanal Ağı'nı kullanarak sanal makineleriniz için özel bir ağ kurmanızı öneririz. Bu, özel IP adresi üzerinden kendi aralarında iletişim kurmalarını sağlar. Daha fazla bilgi için Azure [Sanal Ağı'na](../../../virtual-network/virtual-networks-overview.md)bakın.

## <a name="2-tier-and-3-tier-with-business-tier-scale-out"></a>2 katmanlı ve 3 katmanlı iş katmanı ölçeği
Bu uygulama deseninde, her uygulama katmanını farklı bir sanal makineye yerleştirerek Azure Sanal Makineleri'ne 2 katmanlı veya 3 katmanlı veritabanı uygulaması dağıtMış sınız. Ayrıca, uygulamanızın karmaşıklığı nedeniyle uygulama sunucusu bileşenlerini birden çok sanal makineye dağıtmak isteyebilirsiniz.

Bu uygulama deseni aşağıdaki nde yararlıdır:

* Kurumsal uygulamaları şirket içi sanallaştırılmış platformlardan Azure Sanal Makineleri'ne taşımak istiyorsunuz.
* Uygulama sunucu bileşenlerini, uygulamanızın karmaşıklığı nedeniyle birden çok sanal makineye dağıtmak istiyorsunuz.
* İş mantığını şirket içi LOB (iş yeri) uygulamalarını Azure Sanal Makineler'e taşımak istiyorsunuz. LOB uygulamaları, muhasebe, insan kaynakları (İk), bordro, tedarik zinciri yönetimi ve kaynak planlama uygulamaları gibi bir işletmeyi çalıştırmak için hayati önem taşıyan kritik bilgisayar uygulamaları kümesidir.
* Geliştirme ve test ortamlarını kısa süreler için hızlı bir şekilde sağlamak istiyorsunuz.
* Değişen iş yükü düzeyleri için stres testi yapmak istiyorsunuz, ancak aynı zamanda her zaman birçok fiziksel makineye sahip olmak ve bunları sürdürmek istemiyorsunuz.
* İsteğe bağlı olarak ölçeklendirilebilen bir altyapı ortamına sahip olmak istiyorsunuz.

Aşağıdaki diyagram, şirket içi bir senaryoyu ve bulut özellikli çözümünü gösterir. Bu senaryoda, iş mantığı katmanı ve veri erişim bileşenlerini içeren iş katmanını ölçekleyerek uygulama katmanlarını Azure'daki birden çok sanal makineye yerebilirsiniz. Diyagramda görüldüğü gibi, Azure Yük Dengeleyicisi trafiği birden çok sanal makineye dağıtmakve hangi web sunucusuna bağlanışlayacaklarını belirlemekten sorumludur. Bir yük dengeleyiciarkasında uygulama sunucularının birden fazla örneği olması iş katmanının yüksek kullanılabilirliğini sağlar. Daha fazla bilgi [için, tek bir katmanda birden fazla sanal makineiçeren 2 katmanlı, 3 katmanlı veya n katmanlı uygulama desenleri için en iyi uygulamalara](#best-practices-for-2-tier-3-tier-or-n-tier-patterns-that-have-multiple-vms-in-one-tier)bakın.

![İş katmanı ölçeği ile uygulama deseni](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728011.png)

## <a name="2-tier-and-3-tier-with-presentation-and-business-tiers-scale-out-and-hadr"></a>Sunum ve iş kademeleri ölçeklendirme ve HADR ile 2 katmanlı ve 3 katmanlı
Bu uygulama deseninde, sunu katmanı (web sunucusu) ve iş katmanı (uygulama sunucusu) bileşenlerini birden çok sanal makineye dağıtarak Azure Sanal Makineleri'ne 2 katmanlı veya 3 katmanlı veritabanı uygulaması dağıtırsınız. Ayrıca, Azure sanal makinelerinde veritabanlarınız için yüksek kullanılabilirlik ve olağanüstü durum kurtarma çözümleri uygularsınız.

Bu uygulama deseni aşağıdaki nde yararlıdır:

* SQL Server yüksek kullanılabilirlik ve olağanüstü durum kurtarma özellikleri uygulayarak kurumsal uygulamaları şirket içinde sanallaştırılmış platformlardan Azure'a taşımak istiyorsunuz.
* Gelen istemci isteklerinin artan hacmi ve uygulamanızın karmaşıklığı nedeniyle sunu katmanını ve iş katmanını ölçeklendirmek istiyorsunuz.
* Geliştirme ve test ortamlarını kısa süreler için hızlı bir şekilde sağlamak istiyorsunuz.
* Değişen iş yükü düzeyleri için stres testi yapmak istiyorsunuz, ancak aynı zamanda her zaman birçok fiziksel makineye sahip olmak ve bunları sürdürmek istemiyorsunuz.
* İsteğe bağlı olarak ölçeklendirilebilen bir altyapı ortamına sahip olmak istiyorsunuz.

Aşağıdaki diyagram, şirket içi bir senaryoyu ve bulut özellikli çözümünü gösterir. Bu senaryoda, Azure'daki birden çok sanal makinede sunu katmanını ve iş katmanı bileşenlerini ölçeklendirirsiniz. Ayrıca, Azure'daki SQL Server veritabanları için yüksek kullanılabilirlik ve olağanüstü durum kurtarma (HADR) teknikleri uygularsınız.

Bir uygulamanın birden çok kopyasını farklı VM'lerde çalıştırmak, dengeleme isteklerini bunlar anına yüklediğinizden emin olun. Birden çok sanal makineniz olduğunda, tüm SANAL M'lerinizin aynı anda erişilebilir ve çalışır durumda olduğundan emin olmanız gerekir. Yük dengelemeyi yapılandırırsanız, Azure Yük Dengeleyici VM'lerin durumunu izler ve gelen çağrıları sağlıklı işleyen VM düğümlerine doğru yönlendirir. Sanal makinelerin yük dengelemesinin nasıl ayarlanması hakkında bilgi için Azure [altyapı hizmetleri için Yük dengeleme bölümüne](../tutorial-load-balancer.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)bakın. Bir yük dengeleyiciarkasında web ve uygulama sunucularının birden fazla örneği olması, sunu ve iş katmanlarının yüksek kullanılabilirliğini sağlar.

![Ölçeklendirme ve yüksek kullanılabilirlik](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728012.png)

### <a name="best-practices-for-application-patterns-requiring-sql-hadr"></a>SQL HADR gerektiren uygulama desenleri için en iyi uygulamalar
Azure Sanal Makinelerde SQL Server yüksek kullanılabilirlik ve olağanüstü durum kurtarma çözümleri ayarladığınızda, [Azure Sanal Ağı'nı](../../../virtual-network/virtual-networks-overview.md) kullanarak sanal makineleriniz için sanal ağ kurmak zorunludur.  Sanal Ağ içindeki sanal makineler, hizmet kapalı kalma süresinden sonra bile kararlı bir özel IP adresine sahip olur, böylece DNS ad çözümü için gereken güncelleştirme süresini önleyebilirsiniz. Ayrıca, sanal ağ şirket içi ağınızı Azure'a genişletmenize olanak tanır ve güvenilir bir güvenlik sınırı oluşturur. Örneğin, uygulamanızda kurumsal etki alanı kısıtlamaları varsa (örneğin, Windows kimlik doğrulaması, Etkin Dizin), [Azure Sanal Ağı'nı](../../../virtual-network/virtual-networks-overview.md) ayarlamak gereklidir.

Azure'da üretim kodu çalıştıran müşterilerin çoğu, hem birincil hem de ikincil yinelemeleri Azure'da tutuyor.

Yüksek kullanılabilirlik ve olağanüstü durum kurtarma teknikleri hakkında kapsamlı bilgi ve öğreticiler için Azure [Sanal Makinelerde SQL Server için Yüksek Kullanılabilirlik ve Olağanüstü Durum Kurtarma'ya](virtual-machines-windows-sql-high-availability-dr.md)bakın.

## <a name="2-tier-and-3-tier-using-azure-vms-and-cloud-services"></a>Azure VM'leri ve Bulut Hizmetlerini kullanarak 2 katmanlı ve 3 katmanlı
Bu uygulama deseninde, hem [Azure Bulut Hizmetleri](../../../cloud-services/cloud-services-choose-me.md) (web ve çalışan rolleri - Hizmet Olarak Platform (PaaS)) hem de Azure Sanal [Makineleri](../overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (Hizmet Olarak Altyapı (IaaS)) kullanarak Azure'a 2 katmanlı veya 3 katmanlı uygulama dağıtMış olursunuz. Veri katmanı için Azure [Sanal Makinelerde](../overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) sunu katmanı/iş katmanı ve SQL Server için [Azure Bulut Hizmetlerini](https://azure.microsoft.com/documentation/services/cloud-services/) kullanmak, Azure'da çalışan çoğu uygulama için yararlıdır. Bunun nedeni, Bulut Hizmetleri'nde çalışan bir işlem örneğine sahip olmanın kolay bir yönetim, dağıtım, izleme ve ölçeklendirme sağlamasıdır.

Azure, Bulut Hizmetleri ile altyapıyı sizin için korur, rutin bakımları gerçekleştirir, işletim sistemlerini yamalar ve hizmet ve donanım hatalarından kurtulma girişimleri yapar. Uygulamanızın ölçeklendirme, otomatik ve el ile ölçeklendirme seçenekleri gerektiğinde, uygulamanız tarafından kullanılan örnek veya sanal makine sayısını artırarak veya azaltarak bulut hizmeti projeniz için kullanılabilir. Ayrıca, uygulamanızı Azure'daki bir bulut hizmeti projesine dağıtmak için şirket içi Visual Studio'yu kullanabilirsiniz.

Özetle, sunu/iş katmanı için kapsamlı yönetim görevlerine sahip olmak istemiyorsanız ve uygulamanız herhangi bir karmaşık yazılım yapılandırması veya işletim sistemi gerektiriyorsa, Azure Bulut Hizmetleri'ni kullanın. Azure SQL Veritabanı aradığınız tüm özellikleri desteklemiyorsa, veri katmanı için bir Azure Sanal Makinesi'nde SQL Server'ı kullanın. Azure Bulut Hizmetleri'nde bir uygulama çalıştırmak ve verileri Azure Sanal Makinelerde depolamak, her iki hizmetin de avantajlarını birleştirir. Ayrıntılı bir karşılaştırma için, [Azure'daki geliştirme stratejilerini karşılaştırma](#comparing-web-development-strategies-in-azure)konusubölümüne bakın.

Bu uygulama deseninde, sunu katmanı, Azure yürütme ortamında çalışan bir Bulut Hizmetleri bileşeni olan bir web rolü içerir ve IIS ve ASP.NET tarafından desteklenen web uygulama programlaması için özelleştirilmiştir. İş veya arka uç katmanı, Azure yürütme ortamında çalışan bir Bulut Hizmetleri bileşeni olan ve genelleştirilmiş geliştirme için yararlı olan ve bir web rolü için arka plan işleme gerçekleştirebilecek bir alt rol içerir. Veritabanı katmanı Azure'daki bir SQL Server sanal makinesinde bulunur. Sunu katmanı ile veritabanı katmanı arasındaki iletişim doğrudan veya iş katmanı üzerinden gerçekleşir – işçi rol bileşenleri.

Bu uygulama deseni aşağıdaki nde yararlıdır:

* SQL Server yüksek kullanılabilirlik ve olağanüstü durum kurtarma özellikleri uygulayarak kurumsal uygulamaları şirket içinde sanallaştırılmış platformlardan Azure'a taşımak istiyorsunuz.
* İsteğe bağlı olarak ölçeklendirilebilen bir altyapı ortamına sahip olmak istiyorsunuz.
* Azure SQL Veritabanı, uygulamanızın veya veritabanınızın ihtiyaç duyduğu tüm özellikleri desteklemez.
* Değişen iş yükü düzeyleri için stres testi yapmak istiyorsunuz, ancak aynı zamanda her zaman birçok fiziksel makineye sahip olmak ve bunları sürdürmek istemiyorsunuz.

Aşağıdaki diyagram, şirket içi bir senaryoyu ve bulut özellikli çözümünü gösterir. Bu senaryoda, sunu katmanını web rollerine, iş katmanını çalışan rollerine, veri katmanını da Azure'daki sanal makinelere yerleştirebilirsiniz. Sunu katmanının birden çok kopyasını farklı web rollerinde çalıştırmak, bakiye isteklerini bunlara yüklemeyi sağlar. Azure Bulut Hizmetlerini Azure Sanal Makineleri ile birleştirdiğinizde, [Azure Sanal Ağı'nı](../../../virtual-network/virtual-networks-overview.md) da kurmanızı öneririz. [Azure Sanal Ağı](../../../virtual-network/virtual-networks-overview.md)ile buluttaki aynı bulut hizmeti nde kararlı ve kalıcı özel IP adreslerine sahip olabilirsiniz. Sanal makineleriniz ve bulut hizmetleriniz için sanal bir ağ tanımladıktan sonra, özel IP adresi üzerinden kendi aralarında iletişim kurmaya başlayabilirler. Ayrıca, aynı [Azure Sanal Ağında](../../../virtual-network/virtual-networks-overview.md) sanal makinelere ve Azure web/iş çalışanı rollerine sahip olmak düşük gecikme sonu ve daha güvenli bağlantı sağlar. Daha fazla bilgi için bulut [hizmeti nedir'e](../../../cloud-services/cloud-services-choose-me.md)bakın.

Diyagramda görüldüğü gibi, Azure Yük Dengeleyici trafiği birden çok sanal makineye dağıtır ve hangi web sunucusuna veya uygulama sunucusuna bağlanılmayı belirler. Bir yük dengeleyiciarkasında web ve uygulama sunucularının birden fazla örneği olması, sunu katmanının ve iş katmanının yüksek kullanılabilirliğini sağlar. Daha fazla bilgi için, [SQL HADR gerektiren uygulama desenleri için en iyi uygulamalara](#best-practices-for-application-patterns-requiring-sql-hadr)bakın.

![Bulut Hizmetleri ile uygulama modelleri](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728013.png)

Bu uygulama deseni uygulamak için başka bir yaklaşım, aşağıdaki diyagramda gösterildiği gibi hem sunu katmanı ve iş katmanı bileşenleri içeren birleştirilmiş web rolü kullanmaktır. Bu uygulama deseni, durum lu tasarım gerektiren uygulamalar için yararlıdır. Azure web ve çalışan rollerinde durum salağınızda bilgi işlem düğümleri sağladığından, oturum durumunu aşağıdaki teknolojilerden birini kullanarak depolamak için bir mantık uygulamanızı öneririz: [Azure Önbelleğe Alma,](https://azure.microsoft.com/documentation/services/azure-cache-for-redis/) [Azure Tablo Depolama](../../../cosmos-db/table-storage-how-to-use-dotnet.md) veya Azure SQL [Veritabanı.](../../../sql-database/sql-database-technical-overview.md)

![Bulut Hizmetleri ile uygulama modelleri](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728014.png)

## <a name="pattern-with-azure-vms-azure-sql-database-and-azure-app-service-web-apps"></a>Azure VM'ler, Azure SQL Veritabanı ve Azure Uygulama Hizmeti (Web Uygulamaları) ile Desen
Bu uygulama deseninin birincil amacı, azure altyapısını hizmet (IaaS) bileşenleri yle hizmet olarak azure platformu bileşenlerini (PaaS) çözümünde nasıl birleştirdiğinizi göstermektir. Bu desen, ilişkisel veri depolama için Azure SQL Veritabanı'na odaklanmıştır. Hizmet teklifi olarak Azure altyapısının bir parçası olan bir Azure sanal makinesinde SQL Server içermez.

Bu uygulama deseninde, sunuyu ve iş katmanlarını aynı sanal makineye yerleştirerek ve Azure SQL Veritabanı (SQL Database) sunucularında bir veritabanına erişerek bir veritabanı uygulamasını Azure'a dağıtırsınız. Geleneksel IIS tabanlı web çözümlerini kullanarak sunu katmanını uygulayabilirsiniz. Veya Azure [Uygulama Hizmeti'ni](https://azure.microsoft.com/documentation/services/app-service/web/)kullanarak birleştirilmiş bir sunu ve iş katmanı uygulayabilirsiniz.

Bu uygulama deseni aşağıdaki nde yararlıdır:

* Azure'da yapılandırılmış varolan bir SQL Veritabanı sunucunuz var ve uygulamanızı hızlı bir şekilde sınamak istiyorsunuz.
* Azure ortamının özelliklerini sınamak istiyorsunuz.
* Geliştirme ve test ortamlarını kısa süreler için hızlı bir şekilde sağlamak istiyorsunuz.
* İş mantığınız ve veri erişim bileşenleriniz bir web uygulamasında kendi kendine bulunabilir.

Aşağıdaki diyagram, şirket içi bir senaryoyu ve bulut özellikli çözümünü gösterir. Bu senaryoda, uygulama katmanlarını Azure'da tek bir sanal makineye yerve Azure SQL Veritabanı'ndaki verilere erişirsiniz.

![Karışık uygulama deseni](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728015.png)

Azure Web Apps'ı kullanarak birleştirilmiş bir web ve uygulama katmanı uygulamayı seçerseniz, orta katman veya uygulama katmanını bir web uygulaması bağlamında dinamik bağlantı kitaplıkları (DL'ler) olarak tutmanızı öneririz.

Ayrıca, programlama teknikleri hakkında daha fazla bilgi edinmek için bu makalenin sonunda [Azure'daki Karşılaştırma web geliştirme stratejilerinde](#comparing-web-development-strategies-in-azure) verilen önerileri gözden geçirin.

## <a name="n-tier-hybrid-application-pattern"></a>N katmanlı hibrit uygulama deseni
N katmanlı karma uygulama deseninde, uygulamanızı şirket içi ve Azure arasında dağıtılan birden çok katmanda uygularsınız. Bu nedenle, diğer katmanları değiştirmeden belirli bir katmanı değiştirebileceğiniz veya ekleyebileceğiniz esnek ve yeniden kullanılabilir bir karma sistem oluşturursunuz. Şirket ağınızı buluta genişletmek için [Azure Sanal Ağ](../../../virtual-network/virtual-networks-overview.md) hizmetini kullanırsınız.

Bu karma uygulama deseni aşağıdaki nde yararlıdır:

* Kısmen bulutta ve kısmen şirket içinde çalışan uygulamalar oluşturmak istiyorsunuz.
* Varolan bir şirket içi uygulamanın bazı veya tüm öğelerini buluta geçirmek istiyorsunuz.
* Kurumsal uygulamaları şirket içi sanallaştırılmış platformlardan Azure'a taşımak istiyorsunuz.
* İsteğe bağlı olarak ölçeklendirilebilen bir altyapı ortamına sahip olmak istiyorsunuz.
* Geliştirme ve test ortamlarını kısa süreler için hızlı bir şekilde sağlamak istiyorsunuz.
* Kurumsal veritabanı uygulamaları için yedekleme almak için uygun maliyetli bir yol istiyorsunuz.

Aşağıdaki diyagram, şirket içi ve Azure'a yayılan n katmanlı karma uygulama deseni gösterir. Diyagramda gösterildiği gibi, şirket içi altyapı, kullanıcı kimlik doğrulaması ve yetkilendirmesini desteklemek için [Etkin Dizin Etki Alanı Hizmetleri](https://technet.microsoft.com/library/hh831484.aspx) etki alanı denetleyicisini içerir. Diyagramın, veri katmanının bazı bölümlerinin şirket içi bir veri merkezinde, bazı bölümlerinin ise Azure'da yaşadığı bir senaryo yu gösterdiğini unutmayın. Uygulamanızın gereksinimlerine bağlı olarak, diğer birkaç karma senaryo uygulayabilirsiniz. Örneğin, sunu katmanını ve iş katmanını şirket içi bir ortamda, ancak Azure'daki veri katmanında tutabilirsiniz.

![N katmanı uygulama deseni](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728016.png)

Azure'da, Etkin Dizini kuruluşunuz için bağımsız bir bulut dizini olarak kullanabilir veya mevcut şirket içi Active [Directory'yi Azure Etkin Dizini](https://azure.microsoft.com/documentation/services/active-directory/)ile tümleştirebilirsiniz. Diyagramda görüldüğü gibi, iş katmanı bileşenleri özel bir iç IP adresi aracılığıyla [Azure'daki SQL](virtual-machines-windows-sql-server-iaas-overview.md) Server'a, [Azure Sanal Ağı](../../../virtual-network/virtual-networks-overview.md)üzerinden şirket içi SQL Server'a veya .NET Framework veri sağlayıcısı teknolojilerini kullanarak SQL [Veritabanı'na](../../../sql-database/sql-database-technical-overview.md) erişebilir. Bu diyagramda, Azure SQL Veritabanı isteğe bağlı bir veri depolama hizmetidir.

n katmanlı karma uygulama deseninde, belirtilen sırada aşağıdaki iş akışını uygulayabilirsiniz:

1. [Microsoft Değerlendirme ve Planlama (MAP) Araç Kiti'ni](https://microsoft.com/map)kullanarak buluta taşınması gereken kurumsal veritabanı uygulamalarını tanımlayın. MAP Toolkit, sanallaştırma için düşündüğünüz bilgisayarlardan envanter ve performans verilerini toplar ve kapasite ve değerlendirme planlaması hakkında öneriler sunar.
2. Depolama hesapları ve sanal makineler gibi Azure platformunda gereken kaynakları ve yapılandırmayı planlayın.
3. Şirket içi ağ ile [Azure Sanal Ağı](../../../virtual-network/virtual-networks-overview.md)arasında ağ bağlantısı ayarlayın. Kurumsal ağ ile Azure'daki sanal makine arasındaki bağlantıyı kurmak için aşağıdaki iki yöntemden birini kullanın:
   
   1. Azure'daki sanal bir makinede ortak bitiş noktaları üzerinden şirket içi ve Azure arasında bağlantı kurun. Bu yöntem kolay bir kurulum sağlar ve sanal makinenizde SQL Server kimlik doğrulamasını kullanmanıza olanak sağlar. Ayrıca, VM'ye yönelik genel trafiği denetlemek için ağ güvenlik grubu kurallarınızı ayarlayın. Daha fazla bilgi için bkz. [Azure portalını kullanarak VM'nize harici erişime izin ver.](../nsg-quickstart-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
   2. Azure Sanal Özel Ağ (VPN) tüneli üzerinden şirket içi ve Azure arasında bağlantı kurun. Bu yöntem, etki alanı ilkelerini Azure'daki sanal bir makineye genişletmenize olanak tanır. Ayrıca, güvenlik duvarı kurallarını ayarlayabilir ve sanal makinenizde Windows kimlik doğrulamasını kullanabilirsiniz. Şu anda Azure, güvenli siteden siteye VPN ve site noktasından siteye VPN bağlantılarını destekler:
      
      * Güvenli site-site bağlantısı yla, Azure'da şirket içi ağınızla sanal ağınız arasında ağ bağlantısı oluşturabilirsiniz. Şirket içi veri merkezi ortamınızı Azure'a bağlamak önerilir.
      * Güvenli noktadan siteye bağlantı yla, Azure'daki sanal ağınızla her yerde çalışan tek tek bilgisayarlarınız arasında ağ bağlantısı kurabilirsiniz. Çoğunlukla geliştirme ve test amaçlı önerilir.
      
      Azure'da SQL Server'a nasıl bağlanılabilenler hakkında bilgi için [Azure'daki SQL Server Sanal Makinesine Bağlan'a](virtual-machines-windows-sql-connect.md)bakın.
4. Azure'daki sanal bir makine diskinde şirket içi verileri yedekleyen zamanlanmış işler ve uyarılar ayarlayın. Daha fazla bilgi için Azure [Sanal Makinelerde](virtual-machines-windows-sql-backup-recovery.md)Azure Blob Depolama Hizmeti ve YEDEKLEME ve SQL Server için Geri Yükleme ile SQL Server Yedekleme ve Geri [Yükleme'ye](https://msdn.microsoft.com/library/jj919148.aspx) bakın.
5. Uygulamanızın gereksinimlerine bağlı olarak, aşağıdaki üç yaygın senaryodan birini uygulayabilirsiniz:
   
   1. Web sunucunuzu, uygulama sunucunuzu ve duyarsız verilerinizi Azure'daki bir veritabanı sunucusunda tutarken, hassas verileri yerinde saklayabilirsiniz.
   2. Web sunucunuzu ve uygulama sunucunuzu şirket içinde, veritabanı sunucunuzu ise Azure'daki sanal bir makinede tutabilirsiniz.
   3. Veritabanı sunucunuzu, web sunucunuzu ve uygulama sunucunuzu şirket içinde tutarken, veritabanı yinelemelerini Azure'daki sanal makinelerde saklayabilirsiniz. Bu ayar, şirket içi web sunucularının veya raporlama uygulamalarının Azure'daki veritabanı yinelemelerine erişmesine olanak tanır. Bu nedenle, şirket içi veritabanında iş yükünü düşürmeyi başarabilirsiniz. Bu senaryoyu ağır okuma iş yükleri ve gelişimsel amaçlar için uygulamanızı öneririz. Azure'da veritabanı yinelemeleri oluşturma hakkında daha fazla bilgi için, [Azure Sanal Makinelerde SQL Server için Yüksek Kullanılabilirlik ve Olağanüstü Durum Kurtarma'da](virtual-machines-windows-sql-high-availability-dr.md)AlwaysOn Kullanılabilirlik Grupları'na bakın.

## <a name="comparing-web-development-strategies-in-azure"></a>Azure'da web geliştirme stratejilerini karşılaştırma
Azure'da çok katmanlı BIR SQL Server tabanlı uygulama uygulamak ve dağıtmak için aşağıdaki iki programlama yönteminden birini kullanabilirsiniz:

* Azure'da geleneksel bir web sunucusu (IIS - Internet Information Services) ayarlayın ve Azure Sanal Makineler'de SQL Server'daki veritabanlarına erişin.
* Azure'a bir bulut hizmeti uygulayın ve dağıtın. Ardından, bu bulut hizmetinin Azure Sanal makinelerdeki SQL Server veritabanlarına erişebilmesini unutmayın. Bulut hizmeti birden çok web ve çalışan rolü içerebilir.

Aşağıdaki tablo, Azure Sanal Makinelerdeki SQL Server ile ilgili olarak Azure Bulut Hizmetleri ve Azure Web Uygulamaları ile geleneksel web geliştirme karşılaştırmasını sağlar. Tablo, Azure VM'deki SQL Server'ı genel sanal IP adresi veya DNS adı aracılığıyla Azure Web Apps için veri kaynağı olarak kullanabileceğinden Azure Web Apps'ı içerir.

|  | Azure Sanal Makinelerde geleneksel web geliştirme | Azure'da Bulut Hizmetleri | Azure Web Uygulamaları ile Web Barındırma |
| --- | --- | --- | --- |
| **Şirket içi Uygulama Geçişi** |Varolan uygulamalar olduğu gibi. |Uygulamaların web ve çalışan rolleri ne gerekvardır. |Mevcut uygulamalar ancak hızlı ölçeklenebilirlik gerektiren bağımsız web uygulamaları ve web hizmetleri için uygundur. |
| **Geliştirme ve Dağıtım** |Visual Studio, WebMatrix, Visual Web Developer, WebDeploy, FTP, TFS, IIS Manager, PowerShell. |Visual Studio, Azure SDK, TFS, PowerShell. Her bulut hizmetinin, servis paketinizi ve yapılandırmanızı dağıtabileceğiniz iki ortam vardır: evreleme ve üretim. Bir bulut hizmetini üretime teşvik etmeden önce sınamak için hazırlama ortamına dağıtabilirsiniz. |Visual Studio, WebMatrix, Visual Web Developer, FTP, GIT, BitBucket, CodePlex, DropBox, GitHub, Mercurial, TFS, Web Deploy, PowerShell. |
| **Yönetim ve Kurulum** |Uygulama, veri, güvenlik duvarı kuralları, sanal ağ ve işletim sistemi yle ilgili yönetim görevlerden siz sorumlusunuz. |Uygulama, veri, güvenlik duvarı kuralları ve sanal ağdaki yönetim görevlerden siz sorumlusunuz. |Yalnızca uygulama ve verilerle ilgili idari görevlerden siz sorumlusunuz. |
| **Yüksek Kullanılabilirlik ve Olağanüstü Durum Kurtarma (HADR)** |Sanal makineleri aynı kullanılabilirlik kümesine ve aynı bulut hizmetine yerleştirmenizi öneririz. VM'lerinizi aynı kullanılabilirlik kümesinde tutmak, Azure'un yüksek kullanılabilirlik düğümlerini ayrı hata etki adlarına yerleştirmesine ve etki alanlarını yükseltmesine olanak tanır. Benzer şekilde, Sanal M'lerinizi aynı bulut hizmetinde tutmak, yük dengelemesini sağlar ve Sanal Taşıtlar bir Azure veri merkezi içinde yerel ağ üzerinden birbirleriyle doğrudan iletişim kurabilir.<br/><br/>Herhangi bir kesintiden kaçınmak için Azure Sanal Makineler'de SQL Server için yüksek kullanılabilirlik ve olağanüstü durum kurtarma çözümü uygulamaktan siz sorumlusunuz. Desteklenen HADR teknolojileri için [Azure Sanal Makinelerde SQL Server için Yüksek Kullanılabilirlik ve Olağanüstü Durum Kurtarma'ya](virtual-machines-windows-sql-high-availability-dr.md)bakın.<br/><br/>Kendi verilerinizi ve uygulamanızı yedeklemek sizin sorumluluğunuzdadır.<br/><br/>Veri merkezindeki ana bilgisayar makinesi donanım sorunları nedeniyle başarısız olursa Azure sanal makinelerinizi taşıyabilir. Ayrıca, ana bilgisayar makinesi güvenlik veya yazılım güncelleştirmeleri için güncelleştirildiğinde VM'nizin planlanmış kapalı kalma süresi de olabilir. Bu nedenle, sürekli kullanılabilirliği sağlamak için her uygulama katmanında en az iki VM korumanızı öneririz. Azure, tek bir sanal makine için SLA sağlamaz. |Azure, altta yatan donanım veya işletim sistemi yazılımından kaynaklanan hataları yönetir. Uygulamanızın yüksek kullanılabilirliğini sağlamak için bir web veya çalışan rolünün birden çok örneğini uygulamanızı öneririz. Bilgi için [Bulut Hizmetleri, Sanal Makineler ve Sanal Ağ Hizmet Düzeyi Sözleşmesi'ne](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_8/)bakın.<br/><br/>Kendi verilerinizi ve uygulamanızı yedeklemek sizin sorumluluğunuzdadır.<br/><br/>Azure VM'de bir SQL Server veritabanında bulunan veritabanları için, herhangi bir kapalı kalma süresini önlemek için yüksek kullanılabilirlik ve olağanüstü durum kurtarma çözümlerini uygulamaktan siz sorumlusunuz. Desteklenen HDAR teknolojileri için Azure Sanal Makineler'de SQL Server için Yüksek Kullanılabilirlik ve Olağanüstü Durum Kurtarma'ya bakın.<br/><br/>**SQL Server Veritabanı Yansıtma**: Azure Bulut Hizmetleri (web/işçi rolleri) ile kullanın. SQL Server VM'ler ve bir bulut hizmeti projesi aynı Azure Sanal Ağ'da olabilir. SQL Server VM aynı Sanal Ağda değilse, iletişimi SQL Server örneğine yönlendirmek için bir SQL Server Diğer Ad oluşturmanız gerekir. Ayrıca, diğer ad SQL Server adı ile eşleşmelidir. |Yüksek Kullanılabilirlik, Azure çalışanı rollerinden, Azure blob depolamasından ve Azure SQL Veritabanından devralır. Örneğin, Azure Depolama tüm blob, tablo ve sıra verilerinin üç kopyasını tutar. Azure SQL Veritabanı, herhangi bir anda, bir birincil yineleme ve iki ikincil yineleme olmak üzere üç veri yinelemesini çalışır durumda tutar. Daha fazla bilgi için Azure [Depolama](https://azure.microsoft.com/documentation/services/storage/) ve [Azure SQL Veritabanı'na](../../../sql-database/sql-database-technical-overview.md)bakın.<br/><br/>Azure Web Apps için veri kaynağı olarak Azure VM'deki SQL Server'ı kullanırken, Azure Web Uygulamaları'nın Azure Sanal Ağı'nı desteklemediğini unutmayın. Başka bir deyişle, Azure Web Apps'tan Azure'daki SQL Server VM'lere kadar tüm bağlantıların sanal makinelerin genel bitiş noktalarından geçmesi gerekir. Bu, yüksek kullanılabilirlik ve olağanüstü durum kurtarma senaryoları için bazı sınırlamalara neden olabilir. Örneğin, Veritabanı yansıtma ile SQL Server VM'ye bağlanan Azure Web Apps'taki istemci uygulaması, veritabanı yansıtması Azure'daki SQL Server ana bilgisayar VM'leri arasında Azure Sanal Ağı ayarlamanızı gerektirdiğinden, yeni birincil sunucuya bağlanamaz. Bu nedenle, **SQL Server Veritabanı** Yansıtma'yı Azure Web Apps ile yansıtma kığına sahip olmak şu anda desteklenmez.<br/><br/>**SQL Server AlwaysOn Kullanılabilirlik Grupları**: Azure'da SQL Server VM'lerle Azure Web Apps'ı kullanırken AlwaysOn Kullanılabilirlik Grupları ayarlayabilirsiniz. Ancak, iletişimi genel yük dengeli uç noktaları aracılığıyla birincil yinelemeye yönlendirmek için AlwaysOn Availability Group Listener'ı yapılandırmanız gerekir. |
| **binalar arası Bağlantı** |Şirket içinde bağlanmak için Azure Sanal Ağı'nı kullanabilirsiniz. |Şirket içinde bağlanmak için Azure Sanal Ağı'nı kullanabilirsiniz. |Azure Sanal Ağ desteklenir. Daha fazla bilgi için [Web Apps Sanal Ağ Tümleştirmesi'ne](https://azure.microsoft.com/blog/2014/09/15/azure-websites-virtual-network-integration/)bakın. |
| **Ölçeklenebilir -lik** |Ölçeklendirme, sanal makine boyutlarını artırarak veya daha fazla disk ekleyerek kullanılabilir. Sanal makine boyutları hakkında daha fazla bilgi için [Azure için Sanal Makine Boyutları'na](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)bakın.<br/><br/>**Database Server için**: Scale-out veritabanı bölümleme teknikleri ve SQL Server AlwaysOn Kullanılabilirlik grupları üzerinden kullanılabilir.<br/><br/>Ağır okuma iş yükleri için, birden çok ikincil düğümde [AlwaysOn Kullanılabilirlik Grupları'nın](https://msdn.microsoft.com/library/hh510230.aspx) yanı sıra SQL Server Çoğaltma'yı da kullanabilirsiniz.<br/><br/>Ağır yazma iş yükleri için, uygulama ölçeklendirmesağlamak için birden çok fiziksel sunucu arasında yatay bölümleme verileri uygulayabilirsiniz.<br/><br/>Buna ek olarak, [Veri Bağımlı Yönlendirme ile SQL Server](https://technet.microsoft.com/library/cc966448.aspx)kullanarak bir ölçeklendirme uygulayabilirsiniz. Veri Bağımlı Yönlendirme (DDR) ile, veritabanı isteklerini birden çok SQL Server düğümüne yönlendirmek için istemci uygulamasında genellikle iş katmanı katmanında bölümleme mekanizmasını uygulamanız gerekir. İş katmanı, verilerin nasıl bölümlendiği ve hangi düğümün verileri içerdiğine dair eşlemeler içerir.<br/><br/>Sanal makineleri çalıştıran uygulamaları ölçeklendirebilirsiniz. Daha fazla bilgi için, [uygulamanın nasıl ölçeklendirilene](../../../cloud-services/cloud-services-how-to-scale-portal.md)bakın.<br/><br/>**Önemli Not**: Azure'daki **Otomatik Ölçeklendirme** özelliği, uygulamanız tarafından kullanılan Sanal Makineleri otomatik olarak artırmanızı veya azaltmanızı sağlar. Bu özellik, son kullanıcı deneyiminin yoğun dönemlerde olumsuz etkilenmediğini ve talep düşük olduğunda VM'lerin kapatılmasını garanti eder. SQL Server VM'leri içeriyorsa bulut hizmetiniz için Otomatik Ölçek seçeneğini ayarlamamanız önerilir. Bunun nedeni, Otomatik Ölçek özelliğinin, O VM'deki CPU kullanımı bazı eşiklerden daha yüksek olduğunda Azure'un sanal bir makineyi açmasına ve CPU kullanımı nın daha düşük olduğunda sanal bir makineyi kapatmasına olanak tanır. Otomatik Ölçeklendirme özelliği, herhangi bir VM'nin iş yükünü önceki durumlara atıfta bulunulmadan yönetebileceği web sunucuları gibi durum tanımaz uygulamalar için yararlıdır. Ancak, Otomatik Ölçek özelliği, yalnızca bir örneğin veritabanına yazmaya izin verdiği SQL Server gibi durum lu uygulamalar için kullanışlı değildir. |Ölçeklendirme, birden çok web ve çalışan rolü kullanılarak kullanılabilir. Web rolleri ve çalışan rolleri için sanal makine boyutları hakkında daha fazla bilgi için Bulut [Hizmetleri için Boyutları Yapılandır'a](../../../cloud-services/cloud-services-sizes-specs.md)bakın.<br/><br/>Bulut **Hizmetleri'ni**kullanırken, işlemedağıtmak için birden çok rol tanımlayabilir ve uygulamanızın esnek ölçeklemesine de ulaşabilirsiniz. Her bulut hizmeti, her biri kendi uygulama dosyaları ve yapılandırması olan bir veya daha fazla web rolü ve/veya çalışan rolleri içerir. Rol örneklerinin sayısını azaltarak bir bulut hizmetini ölçeklendirebilir ve rol örneklerinin sayısını azaltarak bir bulut hizmetini küçültebilirsiniz. Ayrıntılı bilgi için Azure [Yürütme Modelleri'ne](../../../cloud-services/cloud-services-choose-me.md)bakın.<br/><br/>Ölçeklendirme, [Bulut Hizmetleri, Sanal Makineler ve Sanal Ağ Hizmet Düzeyi Sözleşmesi ve](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_8/) Yük Dengeleyici saaracılığıyla yerleşik Azure yüksek kullanılabilirlik desteği aracılığıyla kullanılabilir.<br/><br/>Çok katmanlı bir uygulama için, Azure Sanal Ağı üzerinden web/işçi rolleri uygulamasını veritabanı sunucusu VM'lerine bağlamanızı öneririz. Buna ek olarak Azure, aynı bulut hizmetindeki VM'ler için yük dengeleme salar ve kullanıcı isteklerini bunlara dağıtır. Bu şekilde bağlanan sanal makineler, azure veri merkezi içinde yerel ağ üzerinden birbirleriyle doğrudan iletişim kurabilir.<br/><br/>Azure portalında **AutoScale'i** ve zamanlama sürelerini ayarlayabilirsiniz. Daha fazla bilgi için, [portaldaki Bulut Hizmeti için otomatik ölçekleme nin nasıl yapılandırılabildiğini](../../../cloud-services/cloud-services-how-to-scale-portal.md)öğrenin. |**Yukarı ve aşağı ölçeklendir**: Web siteniz için ayrılmış örneğin (VM) boyutunu artırabilir/azaltabilirsiniz.<br/><br/>Ölçeklendirin: Web siteniz için daha fazla ayrılmış örnek (VM) ekleyebilirsiniz.<br/><br/>Otomatik **Ölçek'i** portalda ve zamanlama sürelerinde ayarlayabilirsiniz. Daha fazla bilgi için [Web Apps'ı Nasıl Ölçeklendirin.](../../../app-service/manage-scale-up.md) |

Bu programlama yöntemleri arasında seçim yapmak hakkında daha fazla bilgi için Azure [Web Uygulamaları, Bulut Hizmetleri ve VM'ler: Hangisini kullanacağız](/azure/architecture/guide/technology-choices/compute-decision-tree).

## <a name="next-steps"></a>Sonraki Adımlar
Azure Sanal makinelerde SQL Server'ı çalıştırma hakkında daha fazla bilgi için [Azure Sanal Makinelere Genel Bakış'ta SQL Server'a](virtual-machines-windows-sql-server-iaas-overview.md)bakın.

