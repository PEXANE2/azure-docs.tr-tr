---
title: Genel Bakış-MySQL için Azure veritabanı tek sunucu
description: MySQL Community sürümünü temel alan Microsoft bulutundaki bir ilişkisel veritabanı hizmeti olan MySQL tek sunucusu için Azure veritabanı hakkında bilgi edinin.
author: savjani
ms.service: mysql
ms.author: pariks
ms.custom: mvc
ms.topic: overview
ms.date: 8/20/2020
ms.openlocfilehash: 38cf9f5d5102fa4cc322b1a9c7134db85de87814
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90948056"
---
# <a name="azure-database-for-mysql-single-server"></a>MySQL için Azure Veritabanı Tek Sunucu

MySQL Community Edition tarafından desteklenen [MySQL Için Azure veritabanı](overview.md) , iki dağıtım modunda sunulmaktadır:
- Tek sunucu 
- Esnek Sunucu (Önizleme)

Bu makalede, tek sunucu dağıtım modelinin temel kavramlarına genel bakış ve giriş sağlayacağız. Esnek sunucu dağıtım modu hakkında bilgi edinmek için bkz. [esnek sunucuya genel bakış](flexible-server/index.yml)

## <a name="overview"></a>Genel Bakış

Tek sunucu, veritabanının özelleştirmeleri için en düşük gereksinimleri olan tam olarak yönetilen bir veritabanı hizmetidir. Tek sunucu platformu, düzeltme eki uygulama, yedekleme, yüksek kullanılabilirlik, en az Kullanıcı Yapılandırması ve denetimiyle güvenlik gibi veritabanı yönetim işlevlerinin çoğunu işleyecek şekilde tasarlanmıştır. Mimari, tek kullanılabilirlik bölgesinde% 99,99 kullanılabilirlik sağlamak için iyileştirilmiştir. MySQL 5,6, 5,7 ve 8,0 topluluk sürümünü destekler. Hizmet, günümüzde çok çeşitli [Azure bölgelerinde](https://azure.microsoft.com/global-infrastructure/services/)kullanılabilir. 

Tek sunucular, düzeltme eki uygulama ve özel MySQL yapılandırma ayarları üzerinde ayrıntılı denetim gereksinimi olmadan otomatik düzeltme eki uygulamayı işleyecek şekilde tasarlanan bulut Yerel uygulamaları için idealdir. 

## <a name="high-availability"></a>Yüksek kullanılabilirlik

Tek sunucu dağıtım modeli, yerleşik yüksek kullanılabilirlik ve esneklik için en iyi duruma getirilmiştir. Mimari, işlem ve depolamayı ayırır. Veritabanı altyapısı, veri dosyaları Azure Storage 'da yer alırken özel bir işlem kapsayıcısı üzerinde çalışır. Depolama, veri dayanıklılığı sağlayan veritabanı dosyalarının yerel olarak yedekli zaman uyumlu kopyasını tutar. 

Planlı veya planlanmamış yük devretme olayları sırasında, sunucu kapalıysa, hizmet aşağıdaki otomatik prosedürü kullanarak sunucuların yüksek oranda kullanılabilir olduğunu korur:

1. Yeni bir işlem kapsayıcısı sağlandı
2. Veri dosyaları içeren depolama alanı yeni kapsayıcıya eşlendi 
3. MySQL veritabanı altyapısı yeni işlem kapsayıcısına çevrimiçi olarak getirildi
4. Ağ Geçidi Hizmeti, hiçbir uygulama tarafı değişikliği gerektirmeyeceğinden saydam yük devretme sağlar. 
  
60-120 saniyeden oluşan tipik yük devretme süresi aralığı. Tek sunucu hizmetinin bulut Yerel tasarımı, etkin bekleme sürümünün maliyetini ortadan kaldıran kullanılabilirliğinin% 99,99 ' unu desteklemeye izin verir.

Azure 'un sektör lideri% 99,99 kullanılabilirlik hizmet düzeyi sözleşmesi (SLA), Microsoft tarafından yönetilen küresel bir veri merkezi ağı tarafından destekleniyor ve uygulamalarınızın 24/7 çalışmasına yardımcı olur.

:::image type="content" source="media/single-server-overview/1-singleserverarchitecture.png" alt-text="MySQL için Azure veritabanı tek sunucu mimarisi kavramsal diyagramı"::: 

## <a name="automated-patching"></a>Otomatik Düzeltme Eki Uygulama 

Hizmet, temel alınan donanım, işletim sistemi ve veritabanı altyapısının otomatik düzeltme eki uygular. Düzeltme eki uygulama, güvenlik ve yazılım güncelleştirmelerini içerir. MySQL altyapısı için, ikincil sürüm yükseltmeleri otomatik ve düzeltme eki uygulama döngüsünün bir parçası olarak dahil edilmiştir. Düzeltme için gereken kullanıcı eylemi veya yapılandırma ayarları yoktur. Düzeltme eki uygulama, yükün önem derecesine göre hizmet tarafından yönetilmektedir. Genel olarak, hizmet sürekli tümleştirme ve yayının bir parçası olarak aylık yayın zamanlamasını izler. Kullanıcılar, olaydan önce yaklaşan bakım 72 saati hakkında bildirim almak için [Planlı bakım bildirimine](concepts-monitoring.md) abone olabilir.

## <a name="automatic-backups"></a>Otomatik Yedeklemeler

Tek sunucu hizmeti otomatik olarak sunucu yedeklemeleri oluşturur ve bunları Kullanıcı tarafından yerel olarak yedekli veya coğrafi olarak yedekli depolama olarak yapılandırılmış şekilde depolar. Yedeklemeler, Sunucunuzu yedekleme saklama süresi içinde herhangi bir zaman noktasına geri yüklemek için kullanılabilir. Varsayılan yedekleme saklama süresi yedi gündür. Saklama, isteğe bağlı olarak 35 güne kadar yapılandırılabilir. Tüm yedeklemeler AES 256 bit şifreleme kullanılarak şifrelenir. Ayrıntılar için [yedeklemeler](concepts-backup.md) bölümüne bakın.

## <a name="adjust-performance-and-scale-within-seconds"></a>Saniyeler içinde performansı ve ölçeği ayarlama

Tek sunucu hizmeti üç SKU katmanında mevcuttur: temel, Genel Amaçlı ve bellek için Iyileştirilmiş. Temel katman, düşük maliyetli geliştirme ve düşük eşzamanlılık iş yükleri için idealdir. Genel Amaçlı ve bellek için Iyileştirilmiş, yüksek eşzamanlılık, ölçek ve öngörülebilir performans gerektiren üretim iş yükleri için daha uygundur. İlk uygulamanızı aylık birkaç dolar ücretle küçük bir veritabanı üzerinde oluşturabilir ve sonra çözümünüzün gereksinimlerine göre ölçeği ayarlayabilirsiniz. Depolama Ölçeklendirmesi çevrimiçi ve depolama otomatik büyümesini destekler. Dinamik ölçeklendirebilirlik, veritabanınızın hızla değişen kaynak gereksinimlerine saydam bir şekilde yanıt verebilmesini sağlar. Yalnızca kullandığınız kaynaklar için ödeme yaparsınız. Ayrıntılar için bkz. [fiyatlandırma katmanları](concepts-service-tiers.md) .

## <a name="enterprise-grade-security-compliance-and-governance"></a>Kurumsal düzeyde güvenlik, uyumluluk ve Idare

Tek sunucu hizmeti, bekleyen verilerin depolama şifrelemesi için FIPS 140-2 tarafından doğrulanan şifreleme modülünü kullanır. Yedeklemeler ve sorguları çalıştırırken oluşturulan geçici dosyalar da dahil olmak üzere veriler şifrelenir. Hizmet, Azure depolama şifrelemesi 'ne dahil olan AES 256 bitlik şifrelemeyi kullanır ve anahtarlar sistem tarafından yönetilen (varsayılan) veya [müşteri tarafından yönetilebilir](concepts-data-encryption-mysql.md). Hizmet, varsayılan olarak uygulanan Aktarım Katmanı Güvenliği (SSL/TLS) ile verileri hareket halinde şifreler. Hizmet, [En düşük TLS sürümünü](concepts-ssl-connection-security.md)zorlayabilme özelliği ile 1,2, 1,1 ve 1,0 TLS sürümlerini destekler. 

Hizmet [özel bağlantı](concepts-data-access-security-private-link.md) kullanarak sunuculara özel erişim sağlar ve [Gelişmiş tehdit koruması](concepts-data-access-and-security-threat-protection.md) özelliği sağlar. Gelişmiş tehdit koruması, veritabanlarına erişmek veya veritabanına yararlanmak için olağan dışı ve zararlı olabilecek girişimleri gösteren anormal etkinlikleri algılar.

Yerel kimlik doğrulamasına ek olarak, tek sunucu hizmeti  [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-whatis) kimlik doğrulamasını destekler. Azure AD kimlik doğrulaması, Azure AD 'de tanımlanan ve yönetilen kimlikleri kullanarak MySQL sunucularına bağlanma mekanizmasıdır. Azure AD kimlik doğrulamasıyla, veritabanı kullanıcı kimliklerini ve diğer Azure hizmetlerini, erişim denetimini basitleştiren ve merkezileştiren merkezi bir konumda yönetebilirsiniz.

Tüm veritabanı düzeyindeki etkinlikleri izlemek için [Denetim günlüğü](concepts-audit-logs.md) kullanılabilir. 

Tek sunucu hizmeti, Fedrampa, HIPAA, PCI DSS gibi sektör lideri tüm sertifikalarla uyumlu olur. Azure'ın platform güvenliği hakkında bilgi edinmek için [Azure Güven Merkezi](https://www.microsoft.com/trustcenter/security)'ni ziyaret edin. 

MySQL için Azure veritabanı güvenlik özellikleri hakkında daha fazla bilgi için bkz. [Güvenliğe genel bakış](concepts-security.md).

## <a name="monitoring-and-alerting"></a>İzleme ve uyarı

Tek sunucu hizmeti yerleşik performans izleme ve uyarı özellikleriyle donatılmıştır. Tüm Azure ölçümlerinin bir dakikalık sıklığı vardır ve her ölçüm 30 gün geçmiş sağlar. Ölçümler üzerinde uyarılar yapılandırabilirsiniz. Hizmet, yavaş sorgu günlüklerinin yapılandırılmasını sağlar ve farklılaştırılan bir [sorgu deposu](concepts-query-store.md) özelliğiyle birlikte gelir. Sorgu deposu, en uzun çalışan ve en fazla kaynak yoğunluklu sorguları hızlı bir şekilde bulmanıza yardımcı olarak performans sorunlarını basitleştirir. Bu araçları kullanarak, iş yüklerinizi hızlıca iyileştirebilmenizi ve sunucunuzu en iyi performansa göre yapılandırmanızı sağlayabilirsiniz. Ayrıntılar için bkz. [izleme](concepts-monitoring.md) .

## <a name="migration"></a>Geçiş

Hizmet MySQL 'in topluluk sürümünü çalıştırır. Bu, tam uygulama uyumluluğuna izin verir ve MySQL altyapısında geliştirilen mevcut uygulamayı tek sunucu hizmetine geçirmek için en az yeniden düzenleme maliyeti gerektirir. Tek sunucuya geçiş, aşağıdaki seçeneklerden biri kullanılarak gerçekleştirilebilir:

- **Dökümünü al ve geri yükle** – kullanıcıların bazı kapalı kalma süresini karşılayabileceği, döküm ve geri yükleme işlemleri, mysqldump/mydumper gibi topluluk araçlarını kullanarak geçirmek için en hızlı yolu sağlayabilir. Ayrıntılar için bkz. [döküm kullanarak geçiş ve geri yükleme](concepts-migrate-dump-restore.md) . 
- **Azure veritabanı geçiş hizmeti** : en az kapalı kalma süresiyle tek sunucuya sorunsuz ve Basitleştirilmiş geçişler Için [Azure veritabanı geçiş hizmeti](https://docs.microsoft.com/azure/dms/tutorial-mysql-azure-mysql-online) yararlanılabilir olabilir. 
- **Veri çoğaltma** : en düşük kesinti süresi geçişleri için, binlog tabanlı çoğaltmaya dayanan veri çoğaltma işlemi de yararlanılabilir olabilir. Geçiş üzerinde daha fazla denetim isteyen uygulamalı uzmanlar tarafından en az kapalı kalma süresi geçişleri için veri içi çoğaltma tercih edilir. Ayrıntılar için bkz. [veri çoğaltma](concepts-data-in-replication.md) .

## <a name="contacts"></a>Kişiler
MySQL için Azure veritabanı ile çalışmaya yönelik herhangi bir soru veya öneri için, MySQL için Azure veritabanı ekibine bir e-posta gönderin ([ @Ask MySQL IÇIN Azure DB](mailto:AskAzureDBforMySQL@service.microsoft.com)). Bu e-posta adresi bir teknik destek diğer adı değil.

Buna ek olarak, aşağıdaki iletişim noktalarını uygun şekilde göz önünde bulundurun:

- Azure Desteği ile iletişim kurmak için [Azure portaldan bir bilet oluşturun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
- Hesabınızla ilgili bir sorun gidermek için Azure portalda bir [destek isteği](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest) oluşturun.
- Görüş bildirmek veya yeni özellikler istemek için [UserVoice](https://feedback.azure.com/forums/597982-azure-database-for-mysql) aracılığıyla bir giriş oluşturun.

## <a name="next-steps"></a>Sonraki adımlar
Artık MySQL için Azure veritabanı tek sunucu dağıtım moduna giriş okudığınıza göre şunları yapabilirsiniz:

- İlk sunucunuzu oluşturun. 
  - [Azure portalını kullanarak MySQL için Azure Veritabanı sunucusu oluşturma](quickstart-create-mysql-server-database-using-azure-portal.md)
  - [Azure CLI aracını kullanarak MySQL için Azure Veritabanı sunucusu oluşturma](quickstart-create-mysql-server-database-using-azure-cli.md)
  - [MySQL için Azure Veritabanı’na yönelik Azure CLI örnekleri](sample-scripts-azure-cli.md)

- Tercih ettiğiniz dili kullanarak ilk uygulamanızı oluşturun:
  - [Python](./connect-python.md)
  - [Node.JS](./connect-nodejs.md)
  - [Java](./connect-java.md)
  - [Ruby](./connect-ruby.md)
  - [PHP](./connect-php.md)
  - [.NET (C#)](./connect-csharp.md)
  - [Git](./connect-go.md)
  
