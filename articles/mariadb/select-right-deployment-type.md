---
title: Doğru dağıtım türünü seçme - MariaDB için Azure Veritabanı
description: Bu makalede, MariaDB için Azure Veritabanı'nı hizmet olarak altyapı (IaaS) veya hizmet olarak platform (PaaS) olarak dağıtmadan önce göz önünde bulundurulması gereken etkenler açıklanmaktadır.
author: kummanish
ms.author: manishku
ms.service: mariadb
ms.topic: conceptual
ms.date: 3/18/2020
ms.openlocfilehash: 4c7eb5e4f22cb432a9d17e6eafa653e62e1f9129
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79529909"
---
# <a name="choose-the-right-mariadb-server-option-in-azure"></a>Azure'da doğru MariaDB Server seçeneğini seçin

Azure ile MariaDB sunucu iş yükleriniz barındırılan sanal makine altyapısında hizmet (IaaS) veya hizmet olarak barındırılan bir platform (PaaS) olarak çalıştırılabilir. PaaS'ın birden çok dağıtım seçeneği vardır ve her dağıtım seçeneğinde hizmet katmanları vardır. IaaS ve PaaS arasında seçim yaptığınızda, veritabanınızı yönetmek, düzeltme ler uygulamak ve yedekleme yapmak mı istediğinize veya bu işlemleri Azure'a devretmek isteyip istemediğinize karar vermeniz gerekir.

Kararınızı verirken aşağıdaki iki seçeneği göz önünde bulundurun:

- **MariaDB için Azure Veritabanı:** Bu seçenek, MariaDB topluluk sürümünün kararlı sürümüne dayalı tam olarak yönetilen bir MariaDB veritabanı altyapısıdır. Azure bulut platformunda barındırılan hizmet olarak bu ilişkisel veritabanı (DBaaS), PaaS'ın endüstri kategorisine girer.

  Azure'da MariaDB'nin yönetilen bir örneğiyle, MariaDB Server şirket içinde veya Azure VM'de olduğunda kapsamlı yapılandırma gerektiren yerleşik özellikleri kullanabilirsiniz.

  MariaDB'yi bir hizmet olarak kullanırken, daha fazla denetim için kesintisiz olarak ölçeklendirme veya ölçeklendirme seçenekleriyle giderseniz ödeme yapmayız. Bağımsız MariaDB Server'ın aksine, MariaDB için Azure Veritabanı yerleşik yüksek kullanılabilirlik, zeka ve yönetim gibi ek özelliklere sahiptir.

- **Azure VM'lerde MariaDB:** Bu seçenek IaaS endüstri kategorisine girer. Bu hizmetle, MariaDB Server'ı Azure bulut platformunda tam olarak yönetilen bir sanal makineiçinde çalıştırabilirsiniz. MariaDB'nin tüm yeni sürümleri ve sürümleri Bir IaaS sanal makineye yüklenebilir.

  MariaDB için Azure Veritabanı'ndan en önemli fark, Azure VM'ler üzerinde MariaDB veritabanı altyapısı üzerinde denetim sunar. Ancak, bu denetim, VM'leri ve birçok veritabanı yönetimi (DBA) görevlerini yönetmek için sorumluluk maliyetine mal olur. Bu görevler, veritabanı sunucularını koruma yı ve yamamayı, veritabanı kurtarmayı ve yüksek kullanılabilirlik tasarımını içerir.

Bu seçenekler arasındaki temel farklar aşağıdaki tabloda listelenmiştir:

|            | MariaDB için Azure Veritabanı | Azure VM'lerde MariaDB    |
|:-------------------|:-----------------------------|:--------------------|
| Hizmet düzeyi sözleşmesi (SLA)                | %99,99 kullanılabilirlik sunan SLA| Aynı kullanılabilirlik kümesinde iki veya daha fazla örnekle %99,95'e kadar kullanılabilirlik.<br/><br/>Premium depolama yı kullanarak tek bir örnek VM ile %99,9 kullanılabilirlik.<br/><br/>%99,99'u birden fazla kullanılabilirlik kümesinde birden fazla örneği olan Kullanılabilirlik Bölgelerini kullanır.<br/><br/>Sanal [Makineler SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_8/)bakın. |
| İşletim sistemi yama        | Automatic  | Müşteriler tarafından yönetilir |
| MariaDB yama     | Automatic  | Müşteriler tarafından yönetilir |
| Yüksek kullanılabilirlik | Yüksek kullanılabilirlik (HA) modeli, düğüm düzeyinde bir kesinti oluştuğunda yerleşik arıza mekanizmalarına dayanır. Bu gibi durumlarda, hizmet otomatik olarak yeni bir örnek oluşturur ve bu örne depolama ekler. | Müşteriler yüksek kullanılabilirliği tasarlar, uygular, sınar ve korur. Özellikler, kümeleme, her zaman açık grup çoğaltma, günlük gönderimi veya işlem çoğaltma yı içerebilir.|
| Bölge artıklığı | Şu anda desteklenmiyor | Azure VM'ler farklı kullanılabilirlik bölgelerinde çalışacak şekilde ayarlanabilir. Şirket içi bir çözüm için, müşterilerin kendi ikincil veri merkezlerini oluşturması, yönetmesi ve sürdürmesi gerekir.|
| Karma senaryolar | [Data-in Replication](https://docs.microsoft.com/azure/MariaDB/concepts-data-in-replication)ile harici bir MariaDB sunucusundaki verileri MariaDB hizmeti için Azure Veritabanına senkronize edebilirsiniz. Harici sunucu, sanal makinelerde veya diğer bulut sağlayıcıları tarafından barındırılan bir veritabanı hizmetinde şirket içinde olabilir.<br/><br/> Okuma [yineleme](https://docs.microsoft.com/azure/mariadb/concepts-read-replicas) özelliğiyle, MariaDB ana sunucusu için azure veritabanındaki verileri yalnızca salt okunur çoğaltma sunucularına kadar çoğaltabilirsiniz. Yinelemeler aynı Azure bölgesinde veya bölgeler arasındadır. Salt okunur yinelemeler, binlog çoğaltma teknolojisi kullanılarak eş zamanlı olarak güncelleştirilir.<br/><br/>Bölgeler arası okuma çoğaltma şu anda genel önizlemede.| Müşteriler tarafından yönetilir
| Yedekleme ve geri yükleme | Sunucu [yedeklemelerini](https://docs.microsoft.com/azure/MariaDB/concepts-backup#backups) otomatik olarak oluşturur ve bunları yerel olarak yedekli veya coğrafi yedekli kullanıcı tarafından yapılandırılan depolama alanında saklar. Hizmet tam, diferansiyel ve işlem günlüğü yedeklemeleri alır | Müşteriler tarafından yönetilir |
| Veritabanı işlemlerini izleme | Müşterilere veritabanı işlemi yle ilgili [uyarıları ayarlama](https://docs.microsoft.com/azure/MariaDB/concepts-monitoring) ve eşiklere ulaştıktan sonra harekete geçme olanağı sunar. | Müşteriler tarafından yönetilir |
| Gelişmiş Tehdit Koruması | [Gelişmiş Tehdit Koruması](https://docs.microsoft.com/azure/MariaDB/howto-database-threat-protection-portal)sağlar. Bu koruma, veritabanlarına erişmek veya bunlardan yararlanmak için alışılmadık ve zararlı olabilecek girişimleri gösteren anormal etkinlikleri algılar.<br/><br/>Gelişmiş Tehdit Koruması şu anda genel önizlemededir.| Müşteriler bu korumayı kendileri için oluşturmalıdır.
| Olağanüstü durum kurtarma | Otomatik yedeklemeleri kullanıcı tarafından yapılandırılmış yerel olarak yedekli veya coğrafi yedekli [depolama alanında](https://docs.microsoft.com/azure/MariaDB/howto-restore-server-portal)depolar. Yedeklemeler, sunucunun zaman içinde bir noktaya geri yüklenebilir. Saklama süresi 7 ila 35 gün arasındadır. Geri yükleme, Azure portalı kullanılarak gerçekleştirilir. | Tamamen müşteriler tarafından yönetilir. Sorumluluklar zamanlama, sınama, arşivleme, depolama ve saklama yı içerir, ancak bunlarla sınırlı değildir. Ek bir seçenek, Azure VM'lerini ve veritabanlarını VM'lerde yedeklemek için Azure Kurtarma Hizmetleri kasası kullanmaktır. Bu seçenek önizlemededir. |
| Performans önerileri | Müşterilere sistem tarafından oluşturulan kullanım günlüğü dosyalarını temel alan [performans önerileri](https://techcommunity.microsoft.com/t5/Azure-Database-for-MariaDB/Azure-brings-intelligence-and-high-performance-to-Azure-Database/ba-p/769110) sağlar. Öneriler iş yüklerini optimize etmeye yardımcı olur.<br/><br/>Performans önerileri şu anda genel önizlemededir. | Müşteriler tarafından yönetilir |

## <a name="business-motivations-for-choosing-paas-or-iaas"></a>PaaS veya IaaS seçimi için iş motivasyonları

MariaDB veritabanlarınızı barındırmak için PaaS veya IaaS'i seçme kararınızı etkileyebilecek çeşitli faktörler vardır.

### <a name="cost"></a>Maliyet

Sınırlı finansman genellikle veritabanlarınızı barındırmak için en iyi çözümü belirleyen birincil husustur. Bu, ister az nakit olan bir başlangıç olun, ister sıkı bütçe kısıtlamaları altında faaliyet gösteren kurulmuş bir şirkette çalışan bir ekip olun. Bu bölümde, Azure VM'lerinde MariaDB ve MariaDB için Azure Veritabanı'na geçerli olan Azure'daki faturalandırma ve lisanslama temelleri açıklanmaktadır.

#### <a name="billing"></a>Faturalandırma

MariaDB için Azure Veritabanı şu anda çeşitli katmanlarda, kaynaklar için farklı fiyatlarla hizmet olarak kullanılabilir. Tüm kaynaklar saatlik olarak sabit bir hızda faturalandırılır. Şu anda desteklenen hizmet katmanları, bilgi işlem boyutları ve depolama tutarları hakkında en son bilgiler için [vCore tabanlı satın alma modeline](https://docs.microsoft.com/azure/MariaDB/concepts-pricing-tiers)bakın. Uygulamanızın çeşitli iş ortası gereksinimlerine uyacak şekilde hizmet katmanlarını ve işlem boyutlarını dinamik olarak ayarlayabilirsiniz. Normal [veri aktarım hızlarında](https://azure.microsoft.com/pricing/details/data-transfers/)giden Internet trafiği için faturalandırılırsınız.

Microsoft, MariaDB için Azure Veritabanı ile veritabanı yazılımını otomatik olarak yapılandırır, yamalar ve yükseltir. Bu otomatik eylemler yönetim maliyetlerinizi düşürür. Ayrıca, MariaDB için Azure Veritabanı [yerleşik yedekleme](https://docs.microsoft.com/azure/MariaDB/concepts-backup) özelliklerine sahiptir. Bu özellikler, özellikle çok sayıda veritabanınız varsa, önemli maliyet tasarrufları elde etmenize yardımcı olur. Buna karşılık, Azure VM'lerde MariaDB ile herhangi bir MariaDB sürümünü seçebilir ve çalıştırabilirsiniz. Kullandığınız MariaDB sürümünü niçin kullanırsanız kullanın, verilen VM ve kullanılan belirli MariaDB lisans türü için maliyetler için ödeme.

MariaDB için Azure Veritabanı, her türlü düğüm düzeyinde kesinti için yerleşik yüksek kullanılabilirlik sağlarken, hizmet için %99,99'luk SLA garantisi korunur. Ancak, VM'ler içindeki veritabanı yüksek kullanılabilirliği için, müşteriler MariaDB veritabanında kullanılabilen [MariaDB çoğaltma](https://mariadb.com/kb/en/library/setting-up-replication/) gibi yüksek kullanılabilirlik seçeneklerini kullanmalıdır. Desteklenen yüksek kullanılabilirlik seçeneğini kullanmak ek bir SLA sağlamaz. Ancak, ek ücret ve yönetim ek yüküyle %99,99'dan fazla veritabanı kullanılabilirliği elde edeyim.

Fiyatlandırma hakkında daha fazla bilgi için aşağıdaki makalelere bakın:
* [MariaDB fiyatlandırması için Azure Veritabanı](https://azure.microsoft.com/pricing/details/MariaDB/)
* [Sanal makine fiyatlandırması](https://azure.microsoft.com/pricing/details/virtual-machines/)
* [Azure fiyatlandırma hesaplayıcısı](https://azure.microsoft.com/pricing/calculator/)

### <a name="administration"></a>Yönetim

Birçok işletme için, bir bulut hizmetine geçiş kararı, maliyetle olduğu kadar yönetimin karmaşıklığını da boşaltmayla ilgilidir. IaaS ve PaaS ile Microsoft:

- Altta yatan altyapıyı yönetir.
- Olağanüstü durum kurtarma sağlamak için tüm verileri otomatik olarak çoğaltır.
- Veritabanı yazılımını yapılandırır ve yükseltir.
- Yük dengelemeyi yönetir.
- Bir sunucu hatası varsa saydam hata yapar.

Aşağıdaki liste, her seçenek için yönetimsel hususları açıklar:

* MariaDB için Azure Veritabanı ile veritabanınızı yönetmeye devam edebilirsiniz. Ancak artık veritabanı altyapısını, işletim sistemini veya donanımı yönetmeniz gerekmez. Yönetmeye devam edebileceğiniz öğelere örnek olarak şunlar verilebilir:

  - Veritabanları
  - Oturum açma
  - Dizin
  - Sorgu aparat
  - Denetim
  - Güvenlik

  Ayrıca, yüksek kullanılabilirliği başka bir veri merkezine yapılandırmak en az ile yapılandırma veya yönetim gerektirir.

* Azure VM'lerde MariaDB ile işletim sistemi ve MariaDB sunucu örneği yapılandırması üzerinde tam denetime sahipsiniz. VM ile işletim sistemini ve veritabanı yazılımını ne zaman güncelleştireceğine veya yükseltmeye karar verirsiniz. Virüsten koruma uygulaması gibi ek yazılımları ne zaman yükleyeceğine de karar verirsiniz. Yama, yedekleme ve yüksek kullanılabilirliği büyük ölçüde basitleştirmek için bazı otomatik özellikler sağlanır. VM'nin boyutunu, disk sayısını ve bunların depolama yapılandırmalarını denetleyebilirsiniz. Daha fazla bilgi için [Azure için Sanal makine ve bulut hizmeti boyutlarına](https://docs.microsoft.com/azure/virtual-machines/windows/sizes)bakın.

### <a name="time-to-move-to-azure"></a>Azure'a geçme zamanı

* MariaDB için Azure Veritabanı, geliştirici üretkenliği ve yeni çözümler için pazara hızlı zaman kritik olduğunda bulut tasarımı uygulamalar için doğru çözümdür. DBA gibi programlı işlevsellik ile hizmet bulut mimarları ve geliştiriciler için uygundur, çünkü temel işletim sistemi ve veritabanını yönetme gereksinimini düşürür.

* Yeni şirket içi donanım edinmenin zaman ve masraflarından kaçınmak istediğinizde, Azure VM'lerde MariaDB, MariaDB veritabanı veya Windows veya Linux'taki MariaDB özelliklerine erişim gerektiren uygulamalar için doğru çözümdür. Bu çözüm, Varolan şirket içi uygulamaları ve veritabanlarını Azure'a sağlam olarak geçirmek için de uygundur.

  Sunu, uygulama ve veri katmanlarını değiştirmenize gerek olmadığından, varolan çözümünüzü yeniden architecta etmek için zamandan ve bütçeden tasarruf emkişi siniz. Bunun yerine, tüm çözümlerinizi Azure'a geçirerek Azure platformunun gerektirebileceği bazı performans optimizasyonlarını ele almaya odaklanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [MariaDB fiyatlandırması için Azure Veritabanı'na](https://azure.microsoft.com/pricing/details/MariaDB/)bakın.
* [İlk sunucunuzu oluşturarak](https://docs.microsoft.com/azure/MariaDB/quickstart-create-MariaDB-server-database-using-azure-portal) başlayın.
