---
title: MySQL için Azure veritabanınız için doğru dağıtım türünü seçme
description: Bu makalede, MySQL için Azure veritabanınız için hizmet olarak altyapı (IaaS) veya hizmet olarak platform (PaaS) ile devam etmeden önce yapılması gereken dikkat edilmesi gerekenler açıklanmaktadır.
author: kummanish
ms.author: manishku
ms.service: mysql
ms.topic: conceptual
ms.date: 08/05/2019
ms.openlocfilehash: 7bcbf379ea8d046c8c477dc716711a6a6ffa1dc9
ms.sourcegitcommit: 80da36d4df7991628fd5a3df4b3aa92d55cc5ade
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71813840"
---
# <a name="choose-the-right-mysql-server-option-in-azure"></a>Azure 'da doğru MySQL sunucusu seçeneğini belirleyin

Azure ile MySQL Server iş yükleriniz barındırılan bir altyapı VM 'sinde (IaaS) veya barındırılan hizmet (PaaS) olarak çalışabilir. PaaS 'in birden çok dağıtım seçeneği vardır ve her bir dağıtım seçeneğinde hizmet katmanları vardır. PaaS veya IaaS arasında karar verirken, veritabanınızı yönetmek, yama uygulamak ve yedeklemeleri almak isteyip istemediğinizi belirlemeniz ve bu işlemleri Azure 'a atamak istediğinizi belirlemeniz gerekir.</br>

Bu soruya yönelik yanıtınızı temel alarak aşağıdaki seçenekleri göz önünde bulundurun: <br/>

**MySQL Için Azure veritabanı** , Topluluk sürümünün kararlı sürümünü temel alan tam olarak yönetilen bir MySQL veritabanı altyapısıdır. Azure bulutunda barındırılan bu ilişkisel hizmet olarak veritabanı (DBaaS), hizmet olarak platform (PaaS) endüstri kategorisine denk gelir. Azure 'da MySQL 'in yönetilen bir örneğiyle birlikte, MySQL Server (Şirket içi veya bir Azure sanal makinesinde) kullanılırken kapsamlı yapılandırma gerektiren yerleşik özellikleri ve işlevleri kullanabilirsiniz. Bir hizmet olarak MySQL kullanırken, kesinti olmadan daha fazla güç için ölçeği artırma veya genişletme seçenekleri ile Kullandıkça Öde seçeneğini kullanabilirsiniz. Ayrıca, tek başına MySQL sunucusunun aksine MySQL için Azure veritabanı, yerleşik yüksek kullanılabilirlik, zeka ve yönetim gibi ek özelliklere sahiptir. <br/><br/>
**Azure VM 'Deki MySQL** , sektör kategorisi olarak hizmet olarak altyapı (IaaS) ve Azure bulutundaki tam olarak yönetilen bir sanal makine içinde MySQL Server çalıştırmanızı sağlar. MySQL 'in tüm son sürümleri, IaaS sanal makinesine yüklenebilir. MySQL için Azure veritabanı 'nın en önemli farkı, Azure VM 'lerinde MySQL 'in veritabanı altyapısı üzerinde denetime izin vereceğidir. Bununla birlikte, bu denetim, sanal makineleri ve veritabanı sunucusu bakımı/düzeltme eki uygulama, kurtarma ve yüksek kullanılabilirlik tasarımı gibi çeşitli DBA görevlerini yönetmek için ek sorumluluğun maliyetinde sunulur.

Bu seçenekler arasındaki temel farklılıklar aşağıdaki tabloda listelenmiştir:

|            | **MySQL için Azure Veritabanı** | **Azure VM 'lerinde MySQL**    |
|:-------------------|:-----------------------------|:--------------------|
| **SLA**                | % 99,99 kullanılabilirlik için SLA sunar| Aynı Kullanılabilirlik kümesinde 2 veya daha fazla örnekle en fazla% 99,95 kullanılabilirlik vardır. <br/>Premium depolama kullanan% 99,9 tek örnekli VM <br/> 2 veya daha fazla kullanılabilirlik kümesinde 2 veya daha fazla örneğe sahip kullanılabilirlik bölgesi ile% 99,99.<br/> Note- [sanal makine SLA 'sı](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_8/) |
| **İşletim sistemi düzeltme eki uygulama**        | Automatic  | Müşteriler tarafından yönetiliyor |
|**MySQL düzeltme eki uygulama**     | Automatic  | Müşteriler tarafından yönetiliyor |
| **Yüksek kullanılabilirlik** | Yüksek kullanılabilirlik (HA) modeli, düğüm düzeyinde bir kesinti oluştuğunda yerleşik yük devretme mekanizmalarına dayalıdır. Bu gibi durumlarda, hizmet otomatik olarak örnek oluşturur ve depolama alanını bu yeni örneğe ekler. | Yüksek kullanılabilirlik, müşteri tarafından tasarlanmıştır, uygulanır, test edilir ve sürdürülür. Bu, kullanımdaki MySQL altyapısının sürümüne bağlı olarak her zaman açık (Yük Devretme Kümelemesi veya grup çoğaltma), günlük aktarma ve Işlem çoğaltma içerebilir.|
| **Bölge artıklığı** | Şu anda desteklenmiyor. | Azure VM 'Leri, farklı kullanılabilirlik bölgelerinde çalışacak şekilde ayarlanabilir. Şirket içi bir çözüm için müşterilerin kendi ikincil veri merkezini oluşturması, yönetmesi ve sürdürmeleri beklenmektedir.|
| **Karma Senaryolar** | [Gelen verileri çoğaltma](https://docs.microsoft.com/azure/mysql/concepts-data-in-replication) , bir dış MySQL sunucusundan verileri MySQL Için Azure veritabanı hizmetine eşitlemenize olanak tanır. Dış sunucu şirket içinde, sanal makinelerde veya diğer bulut sağlayıcıları tarafından barındırılan bir veritabanı hizmeti olabilir.  <br/> <br/> [Çoğaltma oku](https://docs.microsoft.com/azure/postgresql/concepts-read-replicas) özelliği, bir MySQL Için Azure veritabanı sunucusu (ana) Ile aynı Azure bölgesindeki veya bölgeler arasında en fazla beş salt okuma sunucusuna (çoğaltmalar) veri çoğaltmanıza olanak sağlar. Salt okuma çoğaltmaları, binlog çoğaltma teknolojisi kullanılarak zaman uyumsuz olarak güncelleştirilir.   <br/> <br/> Note-bölgeler arası okuma çoğaltması Şu anda genel önizlemededir.| Müşteriler tarafından yönetiliyor <br/>
| **Yedekleme ve geri yükleme** | Otomatik olarak [Sunucu yedeklemeleri](https://docs.microsoft.com/azure/mysql/concepts-backup#backups) oluşturur ve bunları Kullanıcı tarafından yerel olarak yedekli veya coğrafi olarak yedekli depolama olarak yapılandırılmış şekilde depolar. Hizmet tam, fark ve işlem günlüğü yedeklemeleri alır | Müşteriler tarafından yönetiliyor |
| **Veritabanı işlemlerini izleme** | Müşterilerin veritabanı işleminde [Uyarı ayarlamasına](https://docs.microsoft.com/azure/mysql/concepts-monitoring) ve eşiklere ulaşmasıyla çalışmasına izin verir. | Müşteriler tarafından yönetiliyor |
| **Gelişmiş iş parçacığı koruması** | Veritabanları için olağan dışı ve zararlı olabilecek girişimleri belirten anormal etkinlikleri algılayan [Gelişmiş tehdit koruması](https://docs.microsoft.com/azure/mysql/howto-database-threat-protection-portal) sağlar. | Müşterilerin kendilerini oluşturması gerekir.
| **Yedeklemeler (olağanüstü durum kurtarma)** | Kullanıcı tarafından [yerel olarak yedekli veya coğrafi olarak yedekli depolamada](https://docs.microsoft.com/azure/mysql/howto-restore-server-portal)yapılandırılmış otomatik yedeklemeleri depolar. Yedeklemeler, bir sunucuyu bir zaman noktasına geri yüklemek için de kullanılabilir. Bekletme dönemi 7-35 günden daha ayarlanabilir. Azure portal kullanılarak geri yükleme gerçekleştirilebilir. <br/> | Zamanlama, test etme, arşivleme, depolama ve bekletme ile sınırlı olmamak üzere müşteri tarafından tam olarak yönetilir. Azure kurtarma hizmetleri Kasası 'nı kullanarak VM 'lerde (önizlemede) Azure VM 'Leri ve veritabanlarını yedeklemek için ek bir seçenek kullanılır. |
| **Performans önerisi** | İş yüklerine en iyi duruma getirmeye yardımcı olmak için kullanım telemetrisine dayalı müşterilere proaktif [performans önerisi](https://techcommunity.microsoft.com/t5/Azure-Database-for-MySQL/Azure-brings-intelligence-and-high-performance-to-Azure-Database/ba-p/769110) sağlar. | Müşteriler tarafından yönetiliyor |


## <a name="business-motivations-for-choosing-paas-or-iaas"></a>PaaS veya IaaS seçimi için iş kesintileri

MySQL veritabanlarınızı barındırmak için PaaS veya IaaS seçme kararlarınızı etkileyebilecek birkaç etken vardır:

### <a name="cost"></a>Maliyet

Yakın bütçe kısıtlamaları altında çalışan kurulu bir şirkette nakit veya ekip için bir başlangıç yaptığınızda, sınırlı komik genellikle veritabanlarınızı barındırmak için en iyi çözüm belirlenirken birincil noktadır. Bu bölümde, Azure 'da MySQL ve MySQL için Azure veritabanı 'na göre Azure 'da faturalandırma ve lisanslama temelleri açıklanmaktadır:

#### <a name="billing"></a>Faturalandırma

Şu anda, MySQL için Azure veritabanı, kaynakların farklı fiyatlarına sahip birkaç katmanda bir hizmet olarak kullanılabilir ve her biri sabit bir fiyata saatlik olarak faturalandırılır. Desteklenen geçerli hizmet katmanları, işlem boyutları ve depolama miktarları hakkında en son bilgiler için bkz. [sanal çekirdek tabanlı satın alma modeli](https://docs.microsoft.com/azure/mysql/concepts-pricing-tiers). Hizmet katmanlarını ve işlem boyutlarını, uygulamanızın değişen aktarım hızı gereksinimleriyle eşleşecek şekilde dinamik olarak ayarlayabilirsiniz. Giden Internet trafiği için düzenli [veri aktarımı fiyatları](https://azure.microsoft.com/pricing/details/data-transfers/)üzerinden faturalandırılırsınız.

MySQL için Azure veritabanı ile veritabanı yazılımı, yönetim maliyetlerinizi azaltacak şekilde Microsoft tarafından otomatik olarak yapılandırılır, düzeltme eki uygulanmış ve yükseltilir. Ayrıca, [yerleşik yedekleme](https://docs.microsoft.com/azure/mysql/concepts-backup) özellikleri, özellikle çok sayıda veritabanınız olduğunda önemli maliyet tasarrufları sağlamanıza yardımcı olur. Azure VM 'lerinde MySQL ile MySQL sürümünü seçip çalıştırabilirsiniz. Kullandığınız MySQL sürümüne bakılmaksızın, sağlanan VM için ve MySQL için kullanılan belirli lisans türünün maliyetleri için ödeme yaparsınız.

MySQL için Azure veritabanı, herhangi bir tür düğüm düzeyinde kesinti için yerleşik yüksek kullanılabilirlik sağlar ve hizmet için% 99,99 SLA 'sını sürdürür. Ancak, VM 'Ler içindeki veritabanı yüksek kullanılabilirlik (HA) için, müşteri MySQL [çoğaltma](https://dev.mysql.com/doc/refman/8.0/en/replication.html)gibi MySQL veritabanında bulunan yüksek kullanılabilirlik seçeneklerinden oluşmalıdır. Desteklenen bir yüksek kullanılabilirlik seçeneğinin kullanılması ek bir SLA sağlamaz, ancak ek bir ücret ve yönetim yüküyle > 99,99% veritabanı kullanılabilirliği elde etmenizi sağlar.

Fiyatlandırma konusunda daha fazla bilgi için aşağıdaki kaynaklara bakın:
* [MySQL için Azure veritabanı fiyatlandırması](https://azure.microsoft.com/pricing/details/mysql/)
* [Sanal makine fiyatlandırması](https://azure.microsoft.com/pricing/details/virtual-machines/)
* [Azure Fiyatlandırma Hesaplayıcısı](https://azure.microsoft.com/pricing/calculator/)

### <a name="administration"></a>Yönetim

Birçok işletme için, bir bulut hizmetine geçiş kararı, yönetim karmaşıklığını boşaltma konusunda çok daha fazla. IaaS ve PaaS ile, Microsoft temel altyapıyı yönetir ve olağanüstü durum kurtarma sağlamak, veritabanı yazılımını yapılandırır ve yükseltir, yük dengelemeyi yönetir ve bir sunucu hatası.

* **MySQL Için Azure veritabanı ile**veritabanınızı yönetmeye devam edebilirsiniz, ancak artık veritabanı altyapısını, işletim sistemini veya donanımı yönetmeniz gerekmez. Yönetmeye devam edebileceğiniz öğelere örnek olarak veritabanları ve oturum açma bilgileri, dizin ve sorgu ayarları ile denetim ve güvenlik verilebilir. Ayrıca, yüksek kullanılabilirliği başka bir veri merkezine yapılandırmak, en az yapılandırma veya yönetim gerektirmez.<br/><br/>
* **Azure VM 'Lerinde MySQL ile**, işletim sistemi ve MySQL Server örnek yapılandırması üzerinde tam denetime sahip olursunuz. Bir VM ile, işletim sisteminin ve veritabanı yazılımının ne zaman güncelleştirilmesi/yükseltileceğine ve virüsten koruma uygulaması gibi herhangi bir ek yazılımın ne zaman yükleneceğine karar verirsiniz. Düzeltme eki uygulama, yedekleme ve yüksek kullanılabilirliği önemli ölçüde kolaylaştırmak amacıyla bazı otomatik özellikler sağlanır. Ayrıca, VM'nin boyutunu, disk sayısını ve disklerin depolama yapılandırmalarını denetleyebilirsiniz. Bilgi için bkz. Azure için sanal makine ve bulut hizmeti boyutları.

### <a name="time-to-move-to-azure-br"></a>Azure 'a geçiş süresi <br/>
* **MySQL Için Azure veritabanı** , geliştirici üretkenliği ve yeni çözümler için hızlı pazarlama süresi kritik olduğunda bulutta tasarlanan uygulamalar için doğru çözümdür. Programlı DBA benzeri işlevsellik sayesinde hizmet, bulut mimarları ve geliştiriciler için idealdir çünkü temel alınan işletim sistemini ve veritabanını yönetme ihtiyacını düşürür.<br/><br/>
* **Azure VM 'Lerinde MySQL** , MySQL veritabanı gerektiren mevcut veya yeni uygulamalar Için ve Windows/Linux üzerinde MySQL veritabanı özelliklerine erişim için idealdir ve yeni şirket içi donanım alma zamandan ve masrafından kaçınmak istiyorsunuz. Bu seçenek ayrıca, mevcut şirket içi uygulamaları ve veritabanlarını Azure 'a, örneğin bir MySQL için Azure veritabanı örneğinin uygun olmadığı durumlar için geçirmek üzere uygun bir yerdir. Sunu, uygulama ve veri katmanlarını değiştirmeye gerek olmadığından, mevcut çözümünüzü yeniden mimarmaya göre zaman ve bütçe tasarrufu yapabilirsiniz. Bunun yerine, tüm Çözümlerinizi Azure 'a geçirmeye ve Azure platformu tarafından gerekebilecek bazı performans iyileştirmelerini gidermeye odaklanırsınız.

## <a name="next-steps"></a>Sonraki Adımlar

* Bkz. [MySQL Için Azure veritabanı fiyatlandırması](https://azure.microsoft.com/pricing/details/mysql/)
* [İlk sunucunuzu oluşturarak](https://review.docs.microsoft.com/azure/mysql/quickstart-create-mysql-server-database-using-azure-portal) başlayın.

