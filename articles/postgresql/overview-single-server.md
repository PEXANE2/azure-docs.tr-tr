---
title: PostgreSQL için Azure veritabanı tek sunucu
description: PostgreSQL için Azure veritabanı tek sunuculu bir genel bakış sağlar.
author: sunilagarwal
ms.author: sunila
ms.custom: mvc
ms.service: postgresql
ms.topic: overview
ms.date: 09/21/2020
ms.openlocfilehash: 18d944cd2b3bfde1a0ee5fc53e83e4016b11b01f
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "100379225"
---
# <a name="azure-database-for-postgresql-single-server"></a>PostgreSQL için Azure veritabanı tek sunucu

PostgreSQL Community Edition tarafından desteklenen [PostgreSQL Için Azure veritabanı](./overview.md) , üç dağıtım modunda sunulmaktadır:

- Tek sunucu
- Esnek Sunucu (Önizleme)
- Hiper Ölçek (Citus)

Bu makalede, tek sunucu dağıtım modelinin temel kavramlarına genel bakış ve giriş sağlayacağız. Esnek sunucu dağıtım modu hakkında bilgi edinmek için sırasıyla [esnek sunucuya genel bakış](./flexible-server/overview.md) ve hyperscale (Citus) genel bakış bölümüne bakın.

## <a name="overview"></a>Genel Bakış

Tek sunucu, veritabanının özelleştirmeleri için en düşük gereksinimleri olan tam olarak yönetilen bir veritabanı hizmetidir. Tek sunucu platformu, düzeltme eki uygulama, yedekleme, yüksek kullanılabilirlik, en az Kullanıcı Yapılandırması ve denetimiyle güvenlik gibi veritabanı yönetim işlevlerinin çoğunu işleyecek şekilde tasarlanmıştır. Mimari, tek kullanılabilirlik bölgesinde% 99,99 kullanılabilirlik sağlamak için iyileştirilmiştir. PostgreSQL 9,5, 9,6, 10 ve 11 Community sürümünü destekler. Hizmet, günümüzde çok çeşitli [Azure bölgelerinde](https://azure.microsoft.com/global-infrastructure/services/)kullanılabilir.

Tek sunucular, düzeltme eki uygulama ve özel PostgreSQL yapılandırma ayarları üzerinde ayrıntılı denetim gereksinimi olmadan otomatik düzeltme eki uygulamayı işleyecek şekilde tasarlanan bulut Yerel uygulamaları için idealdir.

## <a name="high-availability"></a>Yüksek kullanılabilirlik

Tek sunucu dağıtım modeli, yerleşik yüksek kullanılabilirlik ve esneklik için en iyi duruma getirilmiştir. Mimari, işlem ve depolamayı ayırır. Veritabanı altyapısı, veri dosyaları Azure Storage 'da yer alırken özel bir işlem kapsayıcısı üzerinde çalışır. Depolama, veri dayanıklılığı sağlayan veritabanı dosyalarının yerel olarak yedekli zaman uyumlu kopyasını tutar.

Planlı veya planlanmamış yük devretme olayları sırasında, sunucu kapalıysa, hizmet aşağıdaki otomatik prosedürü kullanarak sunucuların yüksek oranda kullanılabilir olduğunu korur:

1. Yeni bir işlem kapsayıcısı sağlandı.
2. Veri dosyaları içeren depolama alanı yeni kapsayıcıya eşlenir.
3. PostgreSQL veritabanı altyapısı yeni işlem kapsayıcısına çevrimiçi olarak getirilir.
4. Ağ Geçidi Hizmeti, hiçbir uygulama tarafı değişikliği gerektirmeyeceğinden saydam yük devretme sağlar.
   
 :::image type="content" source="./media/overview/overview-azure-postgres-single-server.png" alt-text="PostgreSQL için Azure veritabanı tek sunucu":::

60-120 saniyeden oluşan tipik yük devretme süresi aralığı. Tek sunucu hizmetinin bulut Yerel tasarımı, etkin bekleme sürümünün maliyetini ortadan kaldıran kullanılabilirliğinin% 99,99 ' unu desteklemeye izin verir.

Azure 'un sektör lideri% 99,99 kullanılabilirlik hizmet düzeyi sözleşmesi (SLA), Microsoft tarafından yönetilen küresel bir veri merkezi ağı tarafından destekleniyor ve uygulamalarınızın 24/7 çalışmasına yardımcı olur.

## <a name="automated-patching"></a>Otomatik düzeltme eki uygulama

Hizmet, temel alınan donanım, işletim sistemi ve veritabanı altyapısının otomatik düzeltme eki uygular. Düzeltme eki uygulama, güvenlik ve yazılım güncelleştirmelerini içerir. PostgreSQL altyapısı için, ikincil sürüm yükseltmeleri otomatik ve düzeltme eki uygulama döngüsünün bir parçası olarak dahil edilmiştir. Düzeltme için gereken kullanıcı eylemi veya yapılandırma ayarları yoktur. Düzeltme eki uygulama, yükün önem derecesine göre hizmet tarafından yönetilmektedir. Genel olarak, hizmet sürekli tümleştirme ve yayının bir parçası olarak aylık yayın zamanlamasını izler. Kullanıcılar, olaydan önce yaklaşan bakım 72 saati hakkında bildirim almak için [Planlı bakım bildirimine]() abone olabilir.

## <a name="automatic-backups"></a>Otomatik yedeklemeler

Tek sunucu hizmeti otomatik olarak sunucu yedeklemeleri oluşturur ve bunları Kullanıcı tarafından yerel olarak yedekli (LRS) veya coğrafi olarak yedekli depolama alanında depolar. Yedeklemeler, Sunucunuzu yedekleme saklama süresi içinde herhangi bir zaman noktasına geri yüklemek için kullanılabilir. Varsayılan yedekleme saklama süresi yedi gündür. Saklama, isteğe bağlı olarak 35 güne kadar yapılandırılabilir. Tüm yedeklemeler AES 256 bit şifreleme kullanılarak şifrelenir. Ayrıntılar için bkz. [yedeklemeler](./concepts-backup.md) .

## <a name="adjust-performance-and-scale-within-seconds"></a>Saniyeler içinde performansı ve ölçeği ayarlama

Tek sunucu hizmeti üç SKU katmanında mevcuttur: temel, Genel Amaçlı ve bellek için Iyileştirilmiş. Temel katman, düşük maliyetli geliştirme ve düşük eşzamanlılık iş yükleri için idealdir. Genel Amaçlı ve bellek için Iyileştirilmiş, yüksek eşzamanlılık, ölçek ve öngörülebilir performans gerektiren üretim iş yükleri için daha uygundur. İlk uygulamanızı aylık birkaç dolar ücretle küçük bir veritabanı üzerinde oluşturabilir ve sonra çözümünüzün gereksinimlerine göre ölçeği ayarlayabilirsiniz. Depolama Ölçeklendirmesi çevrimiçi ve depolama otomatik büyümesini destekler. Dinamik ölçeklendirebilirlik, veritabanınızın hızla değişen kaynak gereksinimlerine saydam bir şekilde yanıt verebilmesini sağlar. Yalnızca kullandığınız kaynaklar için ödeme yaparsınız. Ayrıntılar için bkz. [Fiyatlandırma katmanları]().

## <a name="enterprise-grade-security-compliance-and-governance"></a>Kurumsal düzeyde güvenlik, uyumluluk ve idare

Tek sunucu hizmeti, bekleyen verilerin depolama şifrelemesi için FIPS 140-2 tarafından doğrulanan şifreleme modülünü kullanır. Yedeklemeler ve sorguları çalıştırırken oluşturulan geçici dosyalar da dahil olmak üzere veriler şifrelenir. Hizmet, Azure depolama şifrelemesi 'ne dahil olan AES 256 bitlik şifrelemeyi kullanır ve anahtarlar sistem tarafından yönetilen (varsayılan) veya [müşteri tarafından yönetilebilir](). Hizmet, varsayılan olarak uygulanan Aktarım Katmanı Güvenliği (SSL/TLS) ile verileri hareket halinde şifreler. Hizmet, [En düşük TLS sürümünü]()zorlayabilme özelliği ile 1,2, 1,1 ve 1,0 TLS sürümlerini destekler.

Hizmet özel bağlantı kullanarak sunuculara özel erişim sağlar ve Gelişmiş tehdit koruması özelliği sağlar. Gelişmiş tehdit koruması, veritabanlarına erişmek veya veritabanına yararlanmak için olağan dışı ve zararlı olabilecek girişimleri gösteren anormal etkinlikleri algılar.

Yerel kimlik doğrulamasına ek olarak, tek sunucu hizmeti Azure Active Directory kimlik doğrulamasını destekler. Azure AD kimlik doğrulaması, Azure AD 'de tanımlanan ve yönetilen kimlikleri kullanarak PostgreSQL sunucularına bağlanma mekanizmasıdır. Azure AD kimlik doğrulamasıyla, veritabanı kullanıcı kimliklerini ve diğer Azure hizmetlerini, erişim denetimini basitleştiren ve merkezileştiren merkezi bir konumda yönetebilirsiniz.

Tüm veritabanı düzeyi etkinliklerini izlemek için [Denetim günlüğü]() (önizlemede) kullanılabilir.

Tek sunucu hizmeti, Fedrampa, HIPAA, PCI DSS gibi sektör lideri tüm sertifikalarla uyumludur. Azure'ın platform güvenliği hakkında bilgi edinmek için [Azure Güven Merkezi]()'ni ziyaret edin.

PostgreSQL için Azure veritabanı güvenlik özellikleri hakkında daha fazla bilgi için bkz. [Güvenliğe genel bakış]().

## <a name="monitoring-and-alerting"></a>İzleme ve uyarı

Tek sunucu hizmeti yerleşik performans izleme ve uyarı özellikleriyle donatılmıştır. Tüm Azure ölçümlerinin bir dakikalık sıklığı vardır ve her ölçüm 30 gün geçmiş sağlar. Ölçümler üzerinde uyarılar yapılandırabilirsiniz. Hizmet, yavaş sorgu günlüklerinin yapılandırılmasını sağlar ve farklılaştırılan bir [sorgu deposu](./concepts-query-store.md) özelliğiyle birlikte gelir. Sorgu deposu, en uzun çalışan ve en fazla kaynak yoğunluklu sorguları hızlı bir şekilde bulmanıza yardımcı olarak performans sorunlarını basitleştirir. Bu araçları kullanarak, iş yüklerinizi hızlıca iyileştirebilmenizi ve sunucunuzu en iyi performansa göre yapılandırmanızı sağlayabilirsiniz. Ayrıntılar için bkz. [izleme](./concepts-monitoring.md) .

## <a name="migration"></a>Geçiş

Hizmet, PostgreSQL 'in topluluk sürümünü çalıştırır. Bu, tam uygulama uyumluluğuna izin verir ve PostgreSQL altyapısında geliştirilen mevcut uygulamayı tek sunuculu hizmete geçirmek için en az yeniden düzenleme maliyeti gerektirir. Tek sunucuya geçiş, aşağıdaki seçeneklerden biri kullanılarak gerçekleştirilebilir:

- **Döküm ve geri yükleme** – kullanıcıların bazı kapalı kalma süresini karşılayabileceği, Pg_restore Pg_dump gibi topluluk araçlarını kullanarak döküm ve geri yükleme işlemleri için en hızlı geçiş yolu sağlayabildiği çevrimdışı geçişler için. Ayrıntılar için bkz. [döküm kullanarak geçiş ve geri yükleme](./howto-migrate-using-dump-and-restore.md) .
- **Azure veritabanı geçiş hizmeti** : en az kapalı kalma süresiyle tek sunucuya sorunsuz ve Basitleştirilmiş geçişler Için Azure veritabanı geçiş hizmeti yararlanılabilir olabilir. [CLI aracılığıyla](../dms/tutorial-postgresql-azure-postgresql-online.md)Portal ve DMS [aracılığıyla DMS](../dms/tutorial-postgresql-azure-postgresql-online-portal.md) 'e bakın.

## <a name="contacts"></a>Kişiler

PostgreSQL için Azure veritabanı ile çalışmaya yönelik herhangi bir soru veya öneri için, PostgreSQL için Azure veritabanı ekibine bir e-posta gönderin ([ @Ask PostgreSQL IÇIN Azure DB](mailto:AskAzureDBforPostgreSQL@service.microsoft.com)). Bu e-posta adresi bir teknik destek diğer adı değil.

Buna ek olarak, aşağıdaki iletişim noktalarını uygun şekilde göz önünde bulundurun:

- Azure Desteği ile iletişim kurmak için [Azure portaldan bir bilet oluşturun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
- Hesabınızla ilgili bir sorun gidermek için Azure portalda bir [destek isteği](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest) oluşturun.
- Görüş bildirmek veya yeni özellikler istemek için [UserVoice](https://feedback.azure.com/forums/597976-azure-database-for-postgresql) aracılığıyla bir giriş oluşturun.

## <a name="next-steps"></a>Sonraki adımlar

Artık PostgreSQL için Azure veritabanı 'nın tek sunuculu dağıtım moduna giriş okudığınıza göre şu şekilde hazırsınız:
- İlk sunucunuzu oluşturun.
