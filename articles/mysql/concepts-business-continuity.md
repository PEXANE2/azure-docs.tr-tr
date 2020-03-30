---
title: İş sürekliliği - MySQL için Azure Veritabanı
description: MySQL hizmeti için Azure Veritabanı'nı kullanırken iş sürekliliği (zamanında geri yükleme, veri merkezi kesintisi, coğrafi geri yükleme) hakkında bilgi edinin.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 3/18/2020
ms.openlocfilehash: af0069adc741cfc802c37c90c0c7ec3c3ba74bb2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79537236"
---
# <a name="understand-business-continuity-in-azure-database-for-mysql"></a>MySQL için Azure Veritabanı'nda iş sürekliliğini anlama

Bu makalede, MySQL için Azure Veritabanı'nın iş sürekliliği ve olağanüstü durum kurtarma için sağladığı özellikler açıklanmaktadır. Veri kaybına veya veritabanınızın ve uygulamanızın kullanılamamasına neden olabilecek yıkıcı olaylardan kurtarma seçenekleri hakkında bilgi edinin. Bir kullanıcı veya uygulama hatası veri bütünlüğünü etkilediğinde, bir Azure bölgesinde kesinti olduğunda veya uygulamanız bakım gerektirdiğinde ne yapmanız gerektiğini öğrenin.

## <a name="features-that-you-can-use-to-provide-business-continuity"></a>İş sürekliliği sağlamak için kullanabileceğiniz özellikler

MySQL için Azure Veritabanı, otomatik yedeklemeler ve kullanıcıların coğrafi geri yükleme başlatma olanağı nı içeren iş sürekliliği özellikleri sağlar. Her biri Tahmini Kurtarma Süresi (ERT) ve olası veri kaybı için farklı özelliklere sahiptir. Bu seçenekleri anladıktan sonra, aralarından birini seçebilir ve bunları farklı senaryolar için birlikte kullanabilirsiniz. İş sürekliliği planınızı geliştirirken, uygulamanın rahatsız edici olaydan sonra tamamen iyileşmesi için kabul edilebilir maksimum zamanı anlamanız gerekir - bu sizin Kurtarma Süresi Hedefinizdir (RTO). Ayrıca, uygulamanın yıkıcı olaydan sonra kurtarma yaparken kaybetmeyi tolere edebileceği en yüksek son veri güncelleştirmesi miktarını (zaman aralığı) anlamanız gerekir - bu sizin Kurtarma Noktası Hedefinizdir (RPO).

Aşağıdaki tablo, kullanılabilir özellikler için ERT ve RPO'yu karşılaştırır:

| **Özellik** | **Temel** | **Genel Amaç** | **Bellek için iyileştirilmiş** |
| :------------: | :-------: | :-----------------: | :------------------: |
| Yedekten belirli bir noktaya geri yükleme | Bekletme süresi içindeki herhangi bir geri yükleme noktası | Bekletme süresi içindeki herhangi bir geri yükleme noktası | Bekletme süresi içindeki herhangi bir geri yükleme noktası |
| Coğrafi olarak çoğaltılan yedeklemelerden coğrafi geri yükleme | Desteklenmiyor | ERT < 12 saat<br/>RPO < 1 saat | ERT < 12 saat<br/>RPO < 1 saat |

> [!IMPORTANT]
> Silinen sunucular geri **yüklenemez.** Sunucuyu silerseniz, sunucuya ait tüm veritabanları da silinir ve kurtarılamaz.

## <a name="recover-a-server-after-a-user-or-application-error"></a>Kullanıcı veya uygulama hatasından sonra sunucu kurtarma

Bir sunucuyu çeşitli yıkıcı olaylardan kurtarmak için hizmetin yedeklerini kullanabilirsiniz. Kullanıcı yanlışlıkla bazı verileri silebilir, yanlışlıkla önemli bir tabloyu bırakabilir ve hatta tüm veritabanını bırakabilir. Bir uygulama yanlışlıkla bir uygulama hatası nedeniyle kötü veri ile iyi verilerin üzerine yazabilir, ve benzeri.

Sunucunuzun bir kopyasını zamanında zamanında geri yükleme gerçekleştirebilirsiniz. Bu nokta, sunucunuz için yapılandırdığınız yedekleme bekletme süresi içinde olmalıdır. Veriler yeni sunucuya geri yüklendikten sonra, özgün sunucuyu yeni yüklenen sunucuyla değiştirebilir veya geri yüklenen sunucudan gerekli verileri özgün sunucuya kopyalayabilirsiniz.

## <a name="recover-from-an-azure-regional-data-center-outage"></a>Azure bölgesel veri merkezi kesintisinden kurtarma

Çok sık olmasa da Azure veri merkezlerinde kesintiler yaşanabilir. Bir kesinti meydana geldiğinde, yalnızca birkaç dakika sürebilir, ancak saatlerce sürebilir bir iş kesintiye neden olur.

Bir seçenek, veri merkezi kesintisi sona erdiğinde sunucunuzun çevrimiçi olmasını beklemektir. Bu, sunucunun belirli bir süre için çevrimdışı olmasını göze alabilen uygulamalar için çalışır, örneğin bir geliştirme ortamı. Veri merkezinin kesintisi olduğunda, kesintinin ne kadar süreceğini bilemezsiniz, bu nedenle bu seçenek yalnızca sunucunuza bir süre ihtiyacınız yoksa çalışır.

Diğer seçenek, MySQL'in coğrafi yedekli yedeklemeleri kullanarak sunucuyu geri yükleyen coğrafi geri yükleme özelliği için Azure Veritabanı'nı kullanmaktır. Bu yedeklemelere, sunucunuzun barındırılan bölgesi çevrimdışı olsa bile erişilebilir. Bu yedeklemelerden başka bir bölgeye geri yüklenebilir ve sunucunuzu yeniden çevrimiçi duruma getirebilirsiniz.

> [!IMPORTANT]
> Coğrafi geri yükleme, yalnızca sunucuya coğrafi yedekli yedekleme depolama alanı sağlamanız halinde mümkündür. Varolan bir sunucu için yerel olarak yedekli den coğrafi yedekli yedeklemelere geçmek istiyorsanız, varolan sunucunuzun mysqldökümü kullanarak bir döküm almalı ve coğrafi yedekli yedeklemelerle yapılandırılan yeni oluşturulmuş bir sunucuya geri yüklemeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

- [MySQL için Azure Veritabanı'ndaki otomatik yedeklemeler](concepts-backup.md)hakkında daha fazla bilgi edinin.
- [Azure portalını](howto-restore-server-portal.md) veya Azure [CLI'yi](howto-restore-server-cli.md)kullanarak nasıl geri yükleyin ilerler öğrenin.
- [MySQL için Azure Veritabanı'nda okuma yinelemeleri](concepts-read-replicas.md)hakkında bilgi edinin.
