---
title: İş sürekliliği-MariaDB için Azure veritabanı
description: MariaDB hizmeti için Azure veritabanı kullanılırken iş sürekliliği (belirli bir noktaya geri yükleme, veri merkezi kesintisi, coğrafi geri yükleme) hakkında bilgi edinin.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 7/7/2020
ms.openlocfilehash: c1f4945cda9db0f99799ea072737d4524f04e481
ms.sourcegitcommit: 124f7f699b6a43314e63af0101cd788db995d1cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86076581"
---
# <a name="understand-business-continuity-in-azure-database-for-mariadb"></a>MariaDB için Azure veritabanı 'nda iş sürekliliğini anlama

Bu makalede, MariaDB için Azure veritabanı 'nın iş sürekliliği ve olağanüstü durum kurtarma için sağladığı yetenekler açıklanır. Veri kaybına neden olabilecek veya veritabanınızın ve uygulamanızın kullanılamaz hale gelmesine neden olabilecek kurtarmayan olaylardan kurtarmaya yönelik seçenekler hakkında bilgi edinin. Bir kullanıcı veya uygulama hatası veri bütünlüğünü etkiliyorsa, bir Azure bölgesinin kesintiye neden olması veya uygulamanızın bakım gerektirmesi durumunda ne yapılacağını öğrenin.

## <a name="features-that-you-can-use-to-provide-business-continuity"></a>İş sürekliliği sağlamak için kullanabileceğiniz özellikler

MariaDB için Azure veritabanı otomatik yedeklemeleri ve kullanıcıların coğrafi geri yükleme başlatma özelliğini içeren iş sürekliliği özellikleri sağlar. Her birinin tahmini kurtarma süresi (ERT) ve olası veri kaybı için farklı özellikleri vardır. Bu seçenekleri anladıktan sonra aralarında seçim yapabilir ve bunları farklı senaryolar için birlikte kullanabilirsiniz. İş sürekliliği planınızı geliştirdikçe, kurtarma süresi hedefiniz (RTO) bu şekilde kesintiye uğradıktan sonra uygulama tamamen kurtarmadan önce kabul edilebilir maksimum süreyi anlamanız gerekir. Ayrıca, kurtarma noktası hedefiniz (RPO) olduğundan, uygulamanın, kesintiye uğratan sonra kurtarma sırasında kaybedilmesi için en yüksek veri güncelleştirme miktarını (zaman aralığı) anlamanız gerekir.

Aşağıdaki tabloda, kullanılabilir özellikler için ERT ve RPO karşılaştırılır:

| **Özellik** | **Temel** | **Genel Amaçlı** | **Bellek için iyileştirilmiş** |
| :------------: | :-------: | :-----------------: | :------------------: |
| Yedekten belirli bir noktaya geri yükleme | Bekletme dönemi içinde herhangi bir geri yükleme noktası | Bekletme dönemi içinde herhangi bir geri yükleme noktası | Bekletme dönemi içinde herhangi bir geri yükleme noktası |
| Coğrafi olarak çoğaltılan yedeklerden coğrafi geri yükleme | Desteklenmiyor | ERT < 12 h<br/>RPO < 1 h | ERT < 12 h<br/>RPO < 1 h |

> [!IMPORTANT]
> Sunucuyu silerseniz, sunucuda bulunan tüm veritabanları da silinir ve kurtarılamaz. Silinen bir sunucuyu geri yükleyemezsiniz.

## <a name="recover-a-server-after-a-user-or-application-error"></a>Bir kullanıcı veya uygulama hatasından sonra bir sunucuyu kurtarma

Bir sunucuyu çeşitli kesintiye uğratan kurtarmak için hizmetin yedeklemelerini kullanabilirsiniz. Bir kullanıcı yanlışlıkla bazı verileri silebilir, yanlışlıkla önemli bir tabloyu bırakabilir, hatta tüm veritabanını bile bırakabilir. Bir uygulama, bir uygulama hatası nedeniyle yanlışlıkla hatalı verilerle iyi verilerin üzerine yazabilir ve bu şekilde devam edebilir.

Sunucunuzun bir kopyasını zaman içinde bilinen iyi bir noktada oluşturmak için bir zaman içinde geri yükleme gerçekleştirebilirsiniz. Bu nokta, sunucunuz için yapılandırdığınız yedekleme saklama süresi içinde olmalıdır. Veriler yeni sunucuya geri yüklendikten sonra, özgün sunucuyu yeni geri yüklenen sunucuyla değiştirebilir veya gerekli verileri geri yüklenen sunucudan özgün sunucuya kopyalayabilirsiniz.

## <a name="recover-from-an-azure-regional-data-center-outage"></a>Azure bölgesel veri merkezi kesintisinden kurtarma

Çok sık olmasa da Azure veri merkezlerinde kesintiler yaşanabilir. Bir kesinti oluştuğunda, en son birkaç dakika içinde bir iş kesintilerine neden olabilir, ancak saat için en son zaman alabilir.

Bir seçenek, veri merkezi kesintisi olduğunda sunucunuzun çevrimiçi duruma gelmesini bekleyeyöneliktir. Bu, örneğin bir geliştirme ortamı gibi belirli bir süre için sunucunun çevrimdışı olmasını sağlayan uygulamalar için geçerlidir. Veri merkezi bir kesinti olduğunda, kesintiden en son ne kadar süre sürdüğünü bilemezsiniz, bu nedenle bu seçenek yalnızca sunucunuza bir süredir ihtiyacınız yoksa işe yarar.

Diğer seçenek de, coğrafi olarak yedekli yedeklemeler kullanarak sunucuyu geri yükleyen MariaDB 'nin coğrafi geri yükleme özelliği için Azure veritabanı 'nı kullanmaktır. Bu yedeklemeler, sunucunuzun barındırıldığı bölge çevrimdışı olduğunda bile erişilebilir. Bu yedeklerden başka bir bölgeye geri yükleme yapabilir ve sunucunuzu yeniden çevrimiçi hale getirebilirsiniz.

> [!IMPORTANT]
> Coğrafi geri yükleme yalnızca, sunucuyu coğrafi olarak yedekli yedekleme depolama alanı ile sağladıysanız mümkündür.

## <a name="cross-region-read-replicas"></a>Bölgeler arası okuma çoğaltmaları

İş sürekliliği ve olağanüstü durum kurtarma planlamasını iyileştirmek için bölgeler arası okuma çoğaltmalarını kullanabilirsiniz. Okuma çoğaltmaları, MariaDB 'nin ikili günlük çoğaltma teknolojisi kullanılarak zaman uyumsuz olarak güncelleştirilir. Okuma çoğaltmaları, kullanılabilir bölgeler ve [okuma çoğaltmaları kavramları makalesindeki](concepts-read-replicas.md)yük devretme hakkında daha fazla bilgi edinin. 

## <a name="next-steps"></a>Sonraki adımlar

- [MariaDB Için Azure veritabanı 'nda otomatikleştirilmiş yedeklemeler](concepts-backup.md)hakkında daha fazla bilgi edinin.
- [Azure Portal](howto-restore-server-portal.md) veya [Azure CLI](howto-restore-server-cli.md)kullanarak nasıl geri yükleyeceğinizi öğrenin.
- [MariaDB Için Azure veritabanı 'nda çoğaltmaları okuma](concepts-read-replicas.md)hakkında bilgi edinin.