---
title: İş sürekliliği-MySQL için Azure veritabanı
description: MySQL için Azure veritabanı hizmeti kullanılırken iş sürekliliği (zaman içinde geri yükleme, veri merkezi kesintisi, coğrafi geri yükleme) hakkında bilgi edinin.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 7/7/2020
ms.openlocfilehash: 74fdfb9f3a3f59f55b0f0ed4865601c0ddb7b7f2
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2020
ms.locfileid: "86241973"
---
# <a name="understand-business-continuity-in-azure-database-for-mysql"></a>MySQL için Azure veritabanı 'nda iş sürekliliğini anlama

Bu makalede, MySQL için Azure veritabanı 'nın iş sürekliliği ve olağanüstü durum kurtarma için sağladığı yetenekler açıklanır. Veri kaybına neden olabilecek veya veritabanınızın ve uygulamanızın kullanılamaz hale gelmesine neden olabilecek kurtarmayan olaylardan kurtarmaya yönelik seçenekler hakkında bilgi edinin. Bir kullanıcı veya uygulama hatası veri bütünlüğünü etkiliyorsa, bir Azure bölgesinin kesintiye neden olması veya uygulamanızın bakım gerektirmesi durumunda ne yapılacağını öğrenin.

## <a name="features-that-you-can-use-to-provide-business-continuity"></a>İş sürekliliği sağlamak için kullanabileceğiniz özellikler

MySQL için Azure veritabanı otomatik yedeklemeleri ve kullanıcıların coğrafi geri yükleme başlatma özelliğini içeren iş sürekliliği özellikleri sağlar. Her birinin tahmini kurtarma süresi (ERT) ve olası veri kaybı için farklı özellikleri vardır. Bu seçenekleri anladıktan sonra aralarında seçim yapabilir ve bunları farklı senaryolar için birlikte kullanabilirsiniz. İş sürekliliği planınızı geliştirdikçe, kurtarma süresi hedefiniz (RTO) bu şekilde kesintiye uğradıktan sonra uygulama tamamen kurtarmadan önce kabul edilebilir maksimum süreyi anlamanız gerekir. Ayrıca, kurtarma noktası hedefiniz (RPO) olduğundan, uygulamanın, kesintiye uğratan sonra kurtarma sırasında kaybedilmesi için en yüksek veri güncelleştirme miktarını (zaman aralığı) anlamanız gerekir.

Aşağıdaki tabloda, kullanılabilir özellikler için ERT ve RPO karşılaştırılır:

| **Özellik** | **Temel** | **Genel Amaçlı** | **Bellek için iyileştirilmiş** |
| :------------: | :-------: | :-----------------: | :------------------: |
| Yedekten belirli bir noktaya geri yükleme | Bekletme dönemi içinde herhangi bir geri yükleme noktası | Bekletme dönemi içinde herhangi bir geri yükleme noktası | Bekletme dönemi içinde herhangi bir geri yükleme noktası |
| Coğrafi olarak çoğaltılan yedeklerden coğrafi geri yükleme | Desteklenmez | ERT < 12 h<br/>RPO < 1 h | ERT < 12 h<br/>RPO < 1 h |

> [!IMPORTANT]
> Silinen sunucular **geri yüklenemez.** Sunucuyu silerseniz, sunucuya ait olan tüm veritabanları da silinir ve kurtarılamaz.

## <a name="recover-a-server-after-a-user-or-application-error"></a>Bir kullanıcı veya uygulama hatasından sonra bir sunucuyu kurtarma

Bir sunucuyu çeşitli kesintiye uğratan kurtarmak için hizmetin yedeklemelerini kullanabilirsiniz. Bir kullanıcı yanlışlıkla bazı verileri silebilir, yanlışlıkla önemli bir tabloyu bırakabilir, hatta tüm veritabanını bile bırakabilir. Bir uygulama, bir uygulama hatası nedeniyle yanlışlıkla hatalı verilerle iyi verilerin üzerine yazabilir ve bu şekilde devam edebilir.

Sunucunuzun bir kopyasını zaman içinde bilinen iyi bir noktada oluşturmak için bir zaman içinde geri yükleme gerçekleştirebilirsiniz. Bu nokta, sunucunuz için yapılandırdığınız yedekleme saklama süresi içinde olmalıdır. Veriler yeni sunucuya geri yüklendikten sonra, özgün sunucuyu yeni geri yüklenen sunucuyla değiştirebilir veya gerekli verileri geri yüklenen sunucudan özgün sunucuya kopyalayabilirsiniz.

## <a name="recover-from-an-azure-regional-data-center-outage"></a>Azure bölgesel veri merkezi kesintisinden kurtarma

Çok sık olmasa da Azure veri merkezlerinde kesintiler yaşanabilir. Bir kesinti oluştuğunda, en son birkaç dakika içinde bir iş kesintilerine neden olabilir, ancak saat için en son zaman alabilir.

Bir seçenek, veri merkezi kesintisi olduğunda sunucunuzun çevrimiçi duruma gelmesini bekleyeyöneliktir. Bu, örneğin bir geliştirme ortamı gibi belirli bir süre için sunucunun çevrimdışı olmasını sağlayan uygulamalar için geçerlidir. Veri merkezi bir kesinti olduğunda, kesintiden en son ne kadar süre sürdüğünü bilemezsiniz, bu nedenle bu seçenek yalnızca sunucunuza bir süredir ihtiyacınız yoksa işe yarar.

Diğer seçenek ise, MySQL için Azure veritabanı 'nın coğrafi olarak yedekli yedeklemeleri kullanarak sunucuyu geri yükleyen coğrafi geri yükleme özelliğini kullanmaktır. Bu yedeklemeler, sunucunuzun barındırıldığı bölge çevrimdışı olduğunda bile erişilebilir. Bu yedeklerden başka bir bölgeye geri yükleme yapabilir ve sunucunuzu yeniden çevrimiçi hale getirebilirsiniz.

> [!IMPORTANT]
> Coğrafi geri yükleme yalnızca, sunucuyu coğrafi olarak yedekli yedekleme depolama alanı ile sağladıysanız mümkündür. Mevcut bir sunucu için yerel olarak yedekli yedekten coğrafi olarak yedekli yedeklemelere geçiş yapmak istiyorsanız, var olan sunucunuzun mysqldump ' ı kullanarak bir döküm almanız ve coğrafi olarak yedekli yedeklemeler ile yapılandırılmış yeni oluşturulan bir sunucuya geri yüklemeniz gerekir.

## <a name="cross-region-read-replicas"></a>Bölgeler arası okuma çoğaltmaları

İş sürekliliği ve olağanüstü durum kurtarma planlamasını iyileştirmek için bölgeler arası okuma çoğaltmalarını kullanabilirsiniz. Okuma çoğaltmaları, MySQL 'in ikili günlük çoğaltma teknolojisi kullanılarak zaman uyumsuz olarak güncelleştirilir. Okuma çoğaltmaları, kullanılabilir bölgeler ve [okuma çoğaltmaları kavramları makalesindeki](concepts-read-replicas.md)yük devretme hakkında daha fazla bilgi edinin. 

## <a name="faq"></a>SSS
### <a name="where-does-azure-database-for-mysql-store-customer-data"></a>MySQL için Azure veritabanı müşteri verilerini nerede depolar?
Varsayılan olarak, MySQL için Azure veritabanı, müşteri verilerini dağıtıldığı bölgeden taşımaz veya depolamaz. Ancak, müşteriler isteğe bağlı olarak coğrafi olarak [yedekli yedeklemeleri](concepts-backup.md#backup-redundancy-options) etkinleştirmeyi veya farklı bir bölgedeki verileri depolamak için [çapraz bölge okuma çoğaltması](concepts-read-replicas.md#cross-region-replication) oluşturmayı tercih edebilir.

## <a name="next-steps"></a>Sonraki adımlar

- [MySQL Için Azure veritabanı 'nda otomatikleştirilmiş yedeklemeler](concepts-backup.md)hakkında daha fazla bilgi edinin.
- [Azure Portal](howto-restore-server-portal.md) veya [Azure CLI](howto-restore-server-cli.md)kullanarak nasıl geri yükleyeceğinizi öğrenin.
- [MySQL Için Azure veritabanı 'nda okuma çoğaltmaları](concepts-read-replicas.md)hakkında bilgi edinin.
