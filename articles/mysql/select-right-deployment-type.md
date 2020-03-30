---
title: Doğru dağıtım türünü seçme - MySQL için Azure Veritabanı
description: Bu makalede, MySQL için Azure Veritabanı'nı hizmet olarak altyapı (IaaS) veya hizmet olarak platform (PaaS) olarak dağıtmadan önce göz önünde bulundurulması gereken etkenler açıklanmaktadır.
author: kummanish
ms.author: manishku
ms.service: mysql
ms.topic: conceptual
ms.date: 12/02/2019
ms.openlocfilehash: 50bcd42189b1bcc945d726277975892f07f1baa1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78255542"
---
# <a name="choose-the-right-mysql-server-option-in-azure"></a>Azure'da doğru MySQL Server seçeneğini seçin

Azure ile MySQL sunucu iş yükleriniz barındırılan sanal makine altyapısında hizmet (IaaS) veya hizmet olarak barındırılan bir platform (PaaS) olarak çalıştırılabilir. PaaS'ın birden çok dağıtım seçeneği vardır ve her dağıtım seçeneğinde hizmet katmanları vardır. IaaS ve PaaS arasında seçim yaptığınızda, veritabanınızı yönetmek, düzeltme ler uygulamak ve yedekleme yapmak mı istediğinize veya bu işlemleri Azure'a devretmek isteyip istemediğinize karar vermeniz gerekir.

Kararınızı verirken aşağıdaki iki seçeneği göz önünde bulundurun:

- **MySQL için Azure Veritabanı.** Bu seçenek, MySQL topluluk sürümünün kararlı sürümüne dayalı tam olarak yönetilen bir MySQL veritabanı altyapısıdır. Azure bulut platformunda barındırılan hizmet olarak bu ilişkisel veritabanı (DBaaS), PaaS'ın endüstri kategorisine girer.

  Azure'da MySQL'in yönetilen bir örneğiyle, MySQL Server şirket içinde veya Azure VM'de olduğunda kapsamlı yapılandırma gerektiren yerleşik özellikleri kullanabilirsiniz.

  MySQL'i bir hizmet olarak kullanırken, daha fazla denetim için hiçbir kesinti olmadan ölçeklendirme veya ölçeklendirme seçenekleriyle giderseniz ödeme yapılır. Bağımsız MySQL Server'ın aksine, MySQL için Azure Veritabanı yerleşik yüksek kullanılabilirlik, zeka ve yönetim gibi ek özelliklere sahiptir.

- **Azure VM'lerde MySQL**. Bu seçenek IaaS endüstri kategorisine girer. Bu hizmetle MySQL Server'ı Azure bulut platformunda tam olarak yönetilen bir sanal makineiçinde çalıştırabilirsiniz. MySQL'in tüm yeni sürümleri ve sürümleri bir IaaS sanal makineye yüklenebilir.

  MySQL için Azure Veritabanı'ndan en önemli fark, Azure VM'lerde MySQL veritabanı altyapısı üzerinde denetim sunar. Ancak, bu denetim, VM'leri ve birçok veritabanı yönetimi (DBA) görevlerini yönetmek için sorumluluk maliyetine mal olur. Bu görevler, veritabanı sunucularını koruma yı ve yamamayı, veritabanı kurtarmayı ve yüksek kullanılabilirlik tasarımını içerir.

Bu seçenekler arasındaki temel farklar aşağıdaki tabloda listelenmiştir:

|            | MySQL için Azure Veritabanı | Azure VM'lerde MySQL    |
|:-------------------|:-----------------------------|:--------------------|
| Hizmet düzeyi sözleşmesi (SLA)                | %99,99 kullanılabilirlik sunan SLA| Aynı kullanılabilirlik kümesinde iki veya daha fazla örnekle %99,95'e kadar kullanılabilirlik.<br/><br/>Premium depolama yı kullanarak tek bir örnek VM ile %99,9 kullanılabilirlik.<br/><br/>%99,99'u birden fazla kullanılabilirlik kümesinde birden fazla örneği olan Kullanılabilirlik Bölgelerini kullanır.<br/><br/>Sanal [Makineler SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_8/)bakın. |
| İşletim sistemi yama        | Automatic  | Müşteriler tarafından yönetilir |
| MySQL yama     | Automatic  | Müşteriler tarafından yönetilir |
| Yüksek kullanılabilirlik | Yüksek kullanılabilirlik (HA) modeli, düğüm düzeyinde bir kesinti oluştuğunda yerleşik arıza mekanizmalarına dayanır. Bu gibi durumlarda, hizmet otomatik olarak yeni bir örnek oluşturur ve bu örne depolama ekler. | Müşteriler yüksek kullanılabilirliği tasarlar, uygular, sınar ve korur. Yetenekleri kümeleme, çoğaltma vb içerebilir.|
| Bölge artıklığı | Şu anda desteklenmiyor | Azure VM'ler farklı kullanılabilirlik bölgelerinde çalışacak şekilde ayarlanabilir. Şirket içi bir çözüm için, müşterilerin kendi ikincil veri merkezlerini oluşturması, yönetmesi ve sürdürmesi gerekir.|
| Karma senaryolar | [Data-in Replication](https://docs.microsoft.com/azure/mysql/concepts-data-in-replication)ile harici bir MySQL sunucusundaki verileri MySQL hizmeti için Azure Veritabanına senkronize edebilirsiniz. Harici sunucu, sanal makinelerde veya diğer bulut sağlayıcıları tarafından barındırılan bir veritabanı hizmetinde şirket içinde olabilir.<br/><br/> Okuma [yineleme](https://docs.microsoft.com/azure/mysql/concepts-read-replicas) özelliğiyle, MySQL ana sunucusu için bir Azure Veritabanındaki verileri yalnızca salt okunur çoğaltma sunucularına kadar çoğaltabilirsiniz. Yinelemeler aynı Azure bölgesinde veya bölgeler arasındadır. Salt okunur yinelemeler, binlog çoğaltma teknolojisi kullanılarak eş zamanlı olarak güncelleştirilir.| Müşteriler tarafından yönetilir
| Yedekleme ve geri yükleme | Sunucu [yedeklemelerini](https://docs.microsoft.com/azure/mysql/concepts-backup#backups) otomatik olarak oluşturur ve bunları yerel olarak yedekli veya coğrafi yedekli kullanıcı tarafından yapılandırılan depolama alanında saklar. Hizmet tam, diferansiyel ve işlem günlüğü yedeklemeleri alır | Müşteriler tarafından yönetilir |
| Veritabanı işlemlerini izleme | Müşterilere veritabanı işlemi yle ilgili [uyarıları ayarlama](https://docs.microsoft.com/azure/mysql/concepts-monitoring) ve eşiklere ulaştıktan sonra harekete geçme olanağı sunar. | Müşteriler tarafından yönetilir |
| Gelişmiş Tehdit Koruması | [Gelişmiş Tehdit Koruması](https://docs.microsoft.com/azure/mysql/howto-database-threat-protection-portal)sağlar. Bu koruma, veritabanlarına erişmek veya bunlardan yararlanmak için alışılmadık ve zararlı olabilecek girişimleri gösteren anormal etkinlikleri algılar. | Müşteriler bu korumayı kendileri için oluşturmalıdır.
| Olağanüstü durum kurtarma | Otomatik yedeklemeleri kullanıcı tarafından yapılandırılmış yerel olarak yedekli veya coğrafi yedekli [depolama alanında](https://docs.microsoft.com/azure/mysql/howto-restore-server-portal)depolar. Yedeklemeler, sunucunun zaman içinde bir noktaya geri yüklenebilir. Saklama süresi 7 ila 35 gün arasındadır. Geri yükleme, Azure portalı kullanılarak gerçekleştirilir. | Tamamen müşteriler tarafından yönetilir. Sorumluluklar zamanlama, sınama, arşivleme, depolama ve saklama yı içerir, ancak bunlarla sınırlı değildir. Ek bir seçenek, Azure VM'lerini ve veritabanlarını VM'lerde yedeklemek için Azure Kurtarma Hizmetleri kasası kullanmaktır. Bu seçenek önizlemededir. |
| Performans önerileri | Müşterilere sistem tarafından oluşturulan kullanım günlüğü dosyalarını temel alan [performans önerileri](https://techcommunity.microsoft.com/t5/Azure-Database-for-MySQL/Azure-brings-intelligence-and-high-performance-to-Azure-Database/ba-p/769110) sağlar. Öneriler iş yüklerini optimize etmeye yardımcı olur. | Müşteriler tarafından yönetilir |

## <a name="business-motivations-for-choosing-paas-or-iaas"></a>PaaS veya IaaS seçimi için iş motivasyonları

MySQL veritabanlarınızı barındırmak için PaaS veya IaaS'i seçme kararınızı etkileyebilecek çeşitli faktörler vardır.

### <a name="cost"></a>Maliyet

Sınırlı finansman genellikle veritabanlarınızı barındırmak için en iyi çözümü belirleyen birincil husustur. Bu, ister az nakit olan bir başlangıç olun, ister sıkı bütçe kısıtlamaları altında faaliyet gösteren kurulmuş bir şirkette çalışan bir ekip olun. Bu bölümde, Azure VM'lerde MySQL ve MySQL için Azure Veritabanı'na uygulanan faturalandırma ve lisanslama temelleri açıklanmaktadır.

#### <a name="billing"></a>Faturalandırma

MySQL için Azure Veritabanı şu anda çeşitli katmanlarda, kaynaklar için farklı fiyatlarla hizmet olarak kullanılabilir. Tüm kaynaklar saatlik olarak sabit bir hızda faturalandırılır. Şu anda desteklenen hizmet katmanları, bilgi işlem boyutları ve depolama tutarları hakkında en son bilgiler için [vCore tabanlı satın alma modeline](https://docs.microsoft.com/azure/mysql/concepts-pricing-tiers)bakın. Uygulamanızın çeşitli iş ortası gereksinimlerine uyacak şekilde hizmet katmanlarını ve işlem boyutlarını dinamik olarak ayarlayabilirsiniz. Normal [veri aktarım hızlarında](https://azure.microsoft.com/pricing/details/data-transfers/)giden Internet trafiği için faturalandırılırsınız.

Microsoft, MySQL için Azure Veritabanı ile veritabanı yazılımını otomatik olarak yapılandırır, yamalar ve yükseltir. Bu otomatik eylemler yönetim maliyetlerinizi düşürür. Ayrıca, MySQL için Azure Veritabanı [yerleşik yedekleme](https://docs.microsoft.com/azure/mysql/concepts-backup) özelliklerine sahiptir. Bu özellikler, özellikle çok sayıda veritabanınız varsa, önemli maliyet tasarrufları elde etmenize yardımcı olur. Buna karşılık, Azure VM'lerde MySQL ile herhangi bir MySQL sürümünü seçebilir ve çalıştırabilirsiniz. Hangi MySQL sürümünü kullanırsanız kullanın, verilen VM ve kullanılan belirli MySQL lisans türü için maliyetler için ödeme.

MySQL için Azure Veritabanı, hizmet için %99,99 SLA garantisini korurken her türlü düğüm düzeyinde kesinti için yerleşik yüksek kullanılabilirlik sağlar. Ancak, VM'ler içindeki veritabanı yüksek kullanılabilirlik için, müşteriler MySQL veritabanında kullanılabilen [MySQL çoğaltma](https://dev.mysql.com/doc/refman/8.0/en/replication.html) gibi yüksek kullanılabilirlik seçeneklerini kullanmalıdır. Desteklenen yüksek kullanılabilirlik seçeneğini kullanmak ek bir SLA sağlamaz. Ancak, ek ücret ve yönetim ek yüküyle %99,99'dan fazla veritabanı kullanılabilirliği elde edeyim.

Fiyatlandırma hakkında daha fazla bilgi için aşağıdaki makalelere bakın:
* [MySQL fiyatlandırması için Azure Veritabanı](https://azure.microsoft.com/pricing/details/mysql/)
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

* MySQL için Azure Veritabanı ile veritabanınızı yönetmeye devam edebilirsiniz. Ancak artık veritabanı altyapısını, işletim sistemini veya donanımı yönetmeniz gerekmez. Yönetmeye devam edebileceğiniz öğelere örnek olarak şunlar verilebilir:

  - Veritabanları
  - Oturum açma
  - Dizin
  - Sorgu aparat
  - Denetim
  - Güvenlik

  Ayrıca, yüksek kullanılabilirliği başka bir veri merkezine yapılandırmak en az ile yapılandırma veya yönetim gerektirir.

* Azure VM'lerde MySQL ile işletim sistemi ve MySQL sunucu örneği yapılandırması üzerinde tam denetime sahipsiniz. VM ile işletim sistemini ve veritabanı yazılımını ne zaman güncelleştireceğine veya yükseltmeye karar verirsiniz. Virüsten koruma uygulaması gibi ek yazılımları ne zaman yükleyeceğine de karar verirsiniz. Yama, yedekleme ve yüksek kullanılabilirliği büyük ölçüde basitleştirmek için bazı otomatik özellikler sağlanır. VM'nin boyutunu, disk sayısını ve bunların depolama yapılandırmalarını denetleyebilirsiniz. Daha fazla bilgi için [Azure için Sanal makine ve bulut hizmeti boyutlarına](https://docs.microsoft.com/azure/virtual-machines/windows/sizes)bakın.

### <a name="time-to-move-to-azure"></a>Azure'a geçme zamanı

* MySQL için Azure Veritabanı, geliştirici üretkenliği ve yeni çözümler için pazara hızlı zaman kritik olduğunda bulut tasarımı uygulamalar için doğru çözümdür. DBA gibi programlı işlevsellik ile hizmet bulut mimarları ve geliştiriciler için uygundur, çünkü temel işletim sistemi ve veritabanını yönetme gereksinimini düşürür.

* Yeni şirket içi donanım edinmenin zaman ve masraflarından kaçınmak istediğinizde, Azure VM'lerde MySQL, MySQL veritabanı veya Windows veya Linux'taki MySQL özelliklerine erişim gerektiren uygulamalar için doğru çözümdür. Bu çözüm, MySQL için Azure Veritabanı'nın yetersiz olduğu durumlar için, varolan şirket içi uygulamaları ve veritabanlarını Azure'a sağlam olarak geçirmek için de uygundur.

  Sunu, uygulama ve veri katmanlarını değiştirmenize gerek olmadığından, varolan çözümünüzü yeniden architecta etmek için zamandan ve bütçeden tasarruf emkişi siniz. Bunun yerine, tüm çözümlerinizi Azure'a geçirerek Azure platformunun gerektirebileceği bazı performans optimizasyonlarını ele almaya odaklanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [MySQL fiyatlandırması için Azure Veritabanı'na](https://azure.microsoft.com/pricing/details/MySQL/)bakın.
* [İlk sunucunuzu oluşturarak](https://docs.microsoft.com/azure/MySQL/quickstart-create-MySQL-server-database-using-azure-portal) başlayın.
