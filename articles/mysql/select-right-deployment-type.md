---
title: Doğru dağıtım türünü seçme-MySQL için Azure veritabanı
description: Bu makalede, MySQL için Azure veritabanı 'nı hizmet olarak altyapı (IaaS) veya hizmet olarak platform (PaaS) olarak dağıtmadan önce göz önünde bulundurmanız gereken faktörler açıklanmaktadır.
author: savjani
ms.author: pariks
ms.service: mysql
ms.topic: conceptual
ms.date: 08/26/2020
ms.openlocfilehash: 125431e6630ccfdd9e0e5d6b2a4ec5fa9b9e58fd
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "101736194"
---
# <a name="choose-the-right-mysql-server-option-in-azure"></a>Azure 'da doğru MySQL sunucusu seçeneğini belirleyin

Azure ile MySQL Server iş yükleriniz, barındırılan bir sanal makine altyapısı (IaaS) veya hizmet olarak barındırılan platform (PaaS) olarak çalışabilir. PaaS 'in birden çok dağıtım seçeneği vardır ve her bir dağıtım seçeneğinde hizmet katmanları vardır. IaaS ve PaaS arasında seçim yaptığınızda, veritabanınızı yönetmek, yama uygulamak ve yedeklemeler yapmak istediğinize ya da bu işlemleri Azure 'a atamak istediğinize karar vermelisiniz.

Kararlarınızı yaparken aşağıdaki iki seçeneği göz önünde bulundurun:

- **MySQL Için Azure veritabanı**. Bu seçenek, MySQL Community Edition 'ın kararlı sürümünü temel alan tam olarak yönetilen bir MySQL veritabanı altyapısıdır. Azure bulut platformunda barındırılan, hizmet olarak bu ilişkisel veritabanı (DBaaS) PaaS sektör kategorisine denk gelir.

  Azure 'da MySQL 'in yönetilen bir örneği sayesinde, otomatik düzeltme eki uygulama, yüksek kullanılabilirlik, otomatik yedeklemeler, elastik ölçekleme, kurumsal düzeyde güvenlik, uyumluluk ve idare, izleme ve uyarı özelliklerini kullanarak, MySQL Server şirket içinde veya bir Azure VM 'sinde geniş bir yapılandırma gerektiren yerleşik özellikleri kullanabilirsiniz. Bir hizmet olarak MySQL kullanırken, kesintiye uğramaması durumunda daha fazla denetim için ölçeği büyütme veya daraltma seçenekleri ile Kullandıkça Öde ile ödeme yaparsınız.
  
  MySQL Community Edition tarafından desteklenen [MySQL Için Azure veritabanı](overview.md), iki dağıtım modunda sunulmaktadır:

  - [Tek sunucu](single-server-overview.md) , veritabanının özelleştirmeleri için en düşük gereksinimleri olan tam olarak yönetilen bir veritabanı hizmetidir. Tek sunucu platformu, düzeltme eki uygulama, yedekleme, yüksek kullanılabilirlik, en az Kullanıcı Yapılandırması ve denetimiyle güvenlik gibi veritabanı yönetim işlevlerinin çoğunu işleyecek şekilde tasarlanmıştır. Mimari, tek kullanılabilirlik bölgesinde% 99,99 kullanılabilirlik sağlamak için iyileştirilmiştir. Tek sunucular, düzeltme eki uygulama ve özel MySQL yapılandırma ayarları üzerinde ayrıntılı denetim gereksinimi olmadan otomatik düzeltme eki uygulamayı işleyecek şekilde tasarlanan bulut Yerel uygulamaları için idealdir.

  - [Esnek sunucu (Önizleme)](flexible-server/overview.md) , veritabanı yönetim işlevleri ve yapılandırma ayarları üzerinde daha ayrıntılı denetim ve esneklik sağlamak için tasarlanan, tam olarak yönetilen bir veritabanı hizmetidir. Genel olarak, hizmet, kullanıcı gereksinimlerine göre tek sunucu dağıtımına kıyasla daha fazla esneklik ve sunucu yapılandırma özelleştirmeleri sağlar. Esnek sunucu mimarisi, kullanıcıların tek bir kullanılabilirlik bölgesinde ve birden çok kullanılabilirlik alanında yüksek kullanılabilirliği kabul etmesine olanak tanır. Esnek sunucular Ayrıca, sürekli olarak tam işlem kapasitesi gerektirmeyen iş yükleri için ideal olan sunucunuzu ve diğer ücretsiz SKU 'Ları başlatma/durdurma özelliğiyle daha iyi maliyet iyileştirme denetimleri sağlar.

  Esnek sunucular için idealdir:

  - MySQL altyapısının daha iyi denetim ve özelleştirmelerini gerektiren uygulama geliştirme.
  - Bölge yedekli yüksek kullanılabilirlik
  - Yönetilen bakım pencereleri

- **Azure VM 'Lerde MySQL**. Bu seçenek IaaS sektör kategorisine denk gelir. Bu hizmetle, MySQL Server 'ı Azure bulut platformunda yönetilen bir sanal makine içinde çalıştırabilirsiniz. MySQL 'in tüm son sürümleri sanal makineye yüklenebilir.

## <a name="comparing-the-mysql-deployment-options-in-azure"></a>Azure 'da MySQL dağıtım seçeneklerini karşılaştırma

Bu seçenekler arasındaki temel farklılıklar aşağıdaki tabloda listelenmiştir:

| Öznitelik          | MySQL için Azure Veritabanı<br/>Tek sunucu |MySQL için Azure Veritabanı<br/>Esnek sunucu  |Azure VM 'lerinde MySQL                      |
|:-------------------|:-------------------------------------------|:---------------------------------------------|:---------------------------------------|
| MySQL sürüm desteği | 5,6, 5,7 & 8,0| 5,7 & 8,0 | Herhangi bir sürüm|
| İşlem ölçekleme | Desteklenir (ve temel katmandan ölçekleme desteklenmez)| Desteklenir | Desteklenir|
| Depolama boyutu | 5 GiB ile 16 TiB| 5 GiB ile 16 TiB | 32 GiB 'den 32.767 GiB 'ye|
| Çevrimiçi depolama Ölçeklendirmesi | Desteklenir| Desteklenir| Desteklenmez|
| Otomatik depolama Ölçeklendirmesi | Desteklenir| Önizlemede desteklenmez| Desteklenmez|
| Ek IOPS Ölçeklendirmesi | Desteklenmiyor| Desteklenir| Desteklenmez|
| Ağ bağlantısı | -Sunucu güvenlik duvarıyla ortak uç noktalar.<br/> -Özel bağlantı desteğiyle özel erişim.|-Sunucu güvenlik duvarıyla ortak uç noktalar.<br/> -Sanal ağ tümleştirmesiyle özel erişim.| -Sunucu güvenlik duvarıyla ortak uç noktalar.<br/> -Özel bağlantı desteğiyle özel erişim.|
| Hizmet düzeyi sözleşmesi (SLA) | % 99,99 kullanılabilirlik SLA 'Sı |Önizlemede SLA yok| Kullanılabilirlik Alanları kullanarak% 99,99|
| İşletim sistemi düzeltme eki uygulama| Automatic  | Özel bakım penceresi denetimiyle otomatik | Son kullanıcılar tarafından yönetiliyor |
| MySQL düzeltme eki uygulama     | Automatic  | Özel bakım penceresi denetimiyle otomatik | Son kullanıcılar tarafından yönetiliyor |
| Yüksek kullanılabilirlik | Tek kullanılabilirlik bölgesi içinde yerleşik HA| Kullanılabilirlik alanları içinde ve genelinde yerleşik HA | Kümeleme, çoğaltma vb. kullanarak özel yönetilen|
| Bölge yedekliliği | Desteklenmez | Desteklenir | Desteklenir|
| Bölge yerleşimi | Desteklenmez | Desteklenir | Desteklenir|
| Karma senaryolar | [Gelen verileri çoğaltma](./concepts-data-in-replication.md) ile desteklenir| Önizlemede yok | Son kullanıcılar tarafından yönetiliyor |
| Okuma amaçlı çoğaltmalar | Desteklenir (en fazla 5 çoğaltma)| Desteklenir (10 çoğaltmaya kadar)| Son kullanıcılar tarafından yönetiliyor |
| Backup | 7-35 gün bekletme ile otomatik | 1-35 gün bekletme ile otomatik | Son kullanıcılar tarafından yönetiliyor |
| Veritabanı işlemlerini izleme | Desteklenir | Desteklenir | Son kullanıcılar tarafından yönetiliyor |
| Olağanüstü durum kurtarma | Coğrafi olarak yedekli yedekleme depolama ve çapraz bölge okuma çoğaltmalarıyla desteklenir | Önizlemede desteklenmez| Çoğaltma teknolojileri ile özel yönetilen |
| Sorgu Performansı İçgörüleri | Desteklenir | Önizlemede yok| Son kullanıcılar tarafından yönetiliyor |
| Ayrılmış Örnek Fiyatlandırması | Desteklenir | Önizlemede yok | Desteklenir |
| Azure AD Kimlik Doğrulaması | Desteklenir | Önizlemede yok | Desteklenmiyor|
| Bekleyen veri şifrelemesi | Müşteri tarafından yönetilen anahtarlarla desteklenir | Hizmet tarafından yönetilen anahtarlarla desteklenir | Desteklenmiyor|
| SSL/TLS | TLS v 1.2, 1,1 ve 1,0 desteğiyle varsayılan olarak etkindir | TLS v 1.2 ile zorlanır | TLS v 1.2, 1,1 ve 1,0 ile desteklenir |
| Fleet yönetimi | Azure CLı, PowerShell, REST ve Azure Resource Manager desteklenir | Azure CLı, PowerShell, REST ve Azure Resource Manager desteklenir  | Azure CLı, PowerShell, REST ve Azure Resource Manager VM 'Ler için desteklenir |

## <a name="business-motivations-for-choosing-paas-or-iaas"></a>PaaS veya IaaS seçimi için iş kesintileri

MySQL veritabanlarınızı barındırmak için PaaS veya IaaS seçme kararlarınızı etkileyebilecek birkaç etken vardır.

### <a name="cost"></a>Maliyet

Maliyet azaltma genellikle veritabanlarınızı barındırmak için en iyi çözümü belirleyen birincil noktadır. Bu, sıkı bütçe kısıtlamaları altında çalışan kurulu bir şirkette çok az nakit veya ekip ile bir başlangıç yaptığınız için geçerlidir. Bu bölümde, Azure VM 'lerde MySQL ve MySQL için Azure veritabanı 'na uygulanan Azure 'da faturalandırma ve lisanslama temelleri açıklanmaktadır.

#### <a name="billing"></a>Faturalandırma

MySQL için Azure veritabanı şu anda birkaç katmanda kaynaklar için farklı fiyatlarla bir hizmet olarak sunulmaktadır. Tüm kaynaklar, saatlik olarak sabit bir fiyat üzerinden faturalandırılır. Şu anda desteklenen hizmet katmanları, işlem boyutları ve depolama miktarları hakkında en son bilgiler için bkz. [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/mysql/). Hizmet katmanlarını ve işlem boyutlarını, uygulamanızın değişen aktarım hızı gereksinimleriyle eşleşecek şekilde dinamik olarak ayarlayabilirsiniz. Giden Internet trafiği için düzenli [veri aktarımı fiyatları](https://azure.microsoft.com/pricing/details/data-transfers/)üzerinden faturalandırılırsınız.

Microsoft, MySQL için Azure veritabanı ile veritabanı yazılımını otomatik olarak yapılandırır, yaekler ve yükseltir. Bu otomatik eylemler, yönetim maliyetlerinizi azaltır. Ayrıca, MySQL için Azure veritabanı [otomatik yedeklemeler](./concepts-backup.md) özelliklerine sahiptir. Bu yetenekler, özellikle çok sayıda veritabanınız olduğunda önemli maliyet tasarrufları elde etmenize yardımcı olur. Buna karşılık, Azure VM 'lerinde MySQL ile herhangi bir MySQL sürümü seçip çalıştırabilirsiniz. Kullandığınız MySQL sürümü ne olursa olsun, sağlanan VM için, veri, yedekleme, izleme verileri ve günlük depolama alanı maliyeti ve kullanılan belirli MySQL lisans türünün maliyetleri (varsa) için ödeme yaparsınız.

MySQL için Azure veritabanı, hizmet için% 99,99 SLA garantisi sağlarken her türlü düğüm düzeyinde kesintiye neden olan yerleşik yüksek kullanılabilirlik sağlar. Ancak VM 'Lerde yüksek kullanılabilirlik için, MySQL veritabanında bulunan [MySQL Çoğaltma](https://dev.mysql.com/doc/refman/8.0/en/replication.html) gibi yüksek kullanılabilirlik seçeneklerini kullanırsınız. Desteklenen yüksek kullanılabilirlik seçeneğinin kullanılması ek bir SLA sağlamaz. Ancak bu, ek maliyet ve yönetim yüküyle% 99,99 ' den fazla veritabanı kullanılabilirliği elde etmenizi sağlar.

Fiyatlandırma hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

- [MySQL için Azure veritabanı fiyatlandırması](https://azure.microsoft.com/pricing/details/mysql/)
- [Sanal makine fiyatlandırması](https://azure.microsoft.com/pricing/details/virtual-machines/)
- [Azure fiyatlandırma hesaplayıcısı](https://azure.microsoft.com/pricing/calculator/)

### <a name="administration"></a>Yönetim

Birçok işletme için, bir bulut hizmetine geçiş kararı, maliyetle ilgili olduğu gibi yönetim karmaşıklığını boşaltma konusunda çok önemlidir.

IaaS ile Microsoft:

- Temel altyapıyı yönetir.
- Temel alınan donanım ve işletim sistemi için otomatik düzeltme eki uygulama sağlar.
  
PaaS ile Microsoft:

- Temel altyapıyı yönetir.
- Temel alınan donanım, işletim sistemi ve veritabanı altyapısı için otomatik düzeltme eki uygulama sağlar.
- Veritabanının yüksek kullanılabilirliğini yönetir.
- Otomatik olarak yedeklemeleri gerçekleştirir ve olağanüstü durum kurtarma sağlamak için tüm verileri çoğaltır.
- Varsayılan olarak bekleyen ve hareketli verileri şifreler.
- Sunucunuzu izler ve sorgu performansı öngörüleri ve performans önerileri için özellikler sağlar

Aşağıdaki listede her bir seçeneğe ilişkin yönetim konuları açıklanmaktadır:

- MySQL için Azure veritabanı ile veritabanınızı yönetmeye devam edebilirsiniz. Ancak artık veritabanı altyapısını, işletim sistemini veya donanımı yönetmeniz gerekmez. Yönetmeye devam edebilmeniz için öğelerin örnekleri şunlardır:

  - Veritabanları
  - Oturum açma
  - Dizin ayarlama
  - Sorgu ayarlama
  - Denetim
  - Güvenlik

  Ayrıca, başka bir veri merkezine yüksek kullanılabilirliği yapılandırmak, yapılandırma veya yönetim gerektirmeyen en az düzeyde olmalıdır.

- Azure VM 'lerinde MySQL ile, işletim sistemi ve MySQL sunucu örneği yapılandırması üzerinde tam denetime sahip olursunuz. Bir VM ile, işletim sisteminin ve veritabanı yazılımının ne zaman güncelleceğine ve hangi düzeltme eklerinin uygulanacağını siz karar verirsiniz. Ayrıca, virüsten koruma uygulaması gibi ek yazılımları ne zaman yükleyeceğinize karar verirsiniz. Düzeltme eki uygulama, yedekleme ve yüksek kullanılabilirliği büyük ölçüde basitleştirmek için bazı otomatik özellikler sağlanır. VM boyutunu, disk sayısını ve bunların depolama yapılandırmalarının boyutunu kontrol edebilirsiniz. Daha fazla bilgi için bkz. [Azure Için sanal makine ve bulut hizmeti boyutları](../virtual-machines/sizes.md).

### <a name="time-to-move-to-azure"></a>Azure 'a geçiş süresi

- MySQL için Azure veritabanı, geliştirici üretkenliği ve yeni çözümler için hızlı pazar süresi kritik olduğunda bulutta tasarlanan uygulamalar için doğru çözümdür. DBA gibi programlama işlevselliğiyle, hizmet, temel işletim sisteminin ve veritabanının yönetilmesi gereksinimini düşürür, bulut mimarları ve geliştiriciler için uygundur.

- Yeni şirket içi donanım alma zamandan ve masrafından kaçınmak istediğinizde, Azure VM 'lerinde MySQL, hizmet tarafından desteklenmeyen veya temel alınan işletim sistemine erişim gerektiren MySQL altyapısının ayrıntılı bir denetimini ve özelleştirmesini gerektiren uygulamalar için doğru çözümdür. Bu çözüm, MySQL için Azure veritabanı 'nın yetersiz olduğu durumlarda, mevcut şirket içi uygulamaları ve veritabanlarını Azure 'a dokunulmadan geçirmek için de uygundur.

Sunu, uygulama ve veri katmanlarını değiştirmenize gerek olmadığından, mevcut çözümünüzü yeniden tasarlayarak zamandan ve bütçelere tasarruf edersiniz. Bunun yerine, tüm Çözümlerinizi Azure 'a geçirmeye ve Azure platformunun gerek duyduğu bazı performans iyileştirmelerini gidermeye odaklanırsınız.

## <a name="next-steps"></a>Sonraki adımlar

- Bkz. [MySQL Için Azure veritabanı fiyatlandırması](https://azure.microsoft.com/pricing/details/MySQL/).
- [İlk sunucunuzu oluşturarak](./quickstart-create-mysql-server-database-using-azure-portal.md) başlayın.
