---
title: İş sürekliliği - PostgreSQL için Azure Veritabanı - Tek Sunucu
description: Bu makalede, PostgreSQL için Azure Veritabanı kullanılırken iş sürekliliği (nokta zaman geri yükleme, veri merkezi kesintisi, coğrafi geri yükleme) açıklanmaktadır.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 08/21/2019
ms.openlocfilehash: afa03399933bdc8bd8ff869125955cfd9e0abecb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75981926"
---
# <a name="overview-of-business-continuity-with-azure-database-for-postgresql---single-server"></a>PostgreSQL için Azure Veritabanı ile iş sürekliliğine genel bakış - Tek Sunucu

Bu genel bakış, PostgreSQL için Azure Veritabanı'nın iş sürekliliği ve olağanüstü durum kurtarma için sağladığı yetenekleri açıklar. Veri kaybına veya veritabanınızın ve uygulamanızın kullanılamamasına neden olabilecek yıkıcı olaylardan kurtarma seçenekleri hakkında bilgi edinin. Bir kullanıcı veya uygulama hatası veri bütünlüğünü etkilediğinde, bir Azure bölgesinde kesinti olduğunda veya uygulamanız bakım gerektirdiğinde ne yapmanız gerektiğini öğrenin.

## <a name="features-that-you-can-use-to-provide-business-continuity"></a>İş sürekliliği sağlamak için kullanabileceğiniz özellikler

PostgreSQL için Azure Veritabanı, otomatik yedeklemeler ve kullanıcıların coğrafi geri yükleme başlatma olanağı nı içeren iş sürekliliği özellikleri sağlar. Her biri Tahmini Kurtarma Süresi (ERT) ve olası veri kaybı için farklı özelliklere sahiptir. Bu seçenekleri anladıktan sonra, aralarından birini seçebilir ve bunları farklı senaryolar için birlikte kullanabilirsiniz. İş sürekliliği planınızı geliştirirken, uygulamanın rahatsız edici olaydan sonra tamamen iyileşmesi için kabul edilebilir maksimum zamanı anlamanız gerekir - bu sizin Kurtarma Süresi Hedefinizdir (RTO). Ayrıca, uygulamanın yıkıcı olaydan sonra kurtarma yaparken kaybetmeyi tolere edebileceği en yüksek son veri güncelleştirmesi miktarını (zaman aralığı) anlamanız gerekir - bu sizin Kurtarma Noktası Hedefinizdir (RPO).

Aşağıdaki tablo, kullanılabilir özellikler için ERT ve RPO'yu karşılaştırır:

| **Özellik** | **Temel** | **Genel Amaç** | **Bellek için iyileştirilmiş** |
| :------------: | :-------: | :-----------------: | :------------------: |
| Yedekten belirli bir noktaya geri yükleme | Bekletme süresi içindeki herhangi bir geri yükleme noktası | Bekletme süresi içindeki herhangi bir geri yükleme noktası | Bekletme süresi içindeki herhangi bir geri yükleme noktası |
| Coğrafi olarak çoğaltılan yedeklemelerden coğrafi geri yükleme | Desteklenmiyor | ERT < 12 saat<br/>RPO < 1 saat | ERT < 12 saat<br/>RPO < 1 saat |

> [!IMPORTANT]
> Silinen sunucular geri **yüklenemez.** Sunucuyu silerseniz, sunucuya ait tüm veritabanları da silinir ve kurtarılamaz. Sunucunuzun yanlışlıkla silinmesini önlemek için [Azure kaynak kilidini](../azure-resource-manager/management/lock-resources.md) kullanın.

## <a name="recover-a-server-after-a-user-or-application-error"></a>Kullanıcı veya uygulama hatasından sonra sunucu kurtarma

Bir sunucuyu çeşitli yıkıcı olaylardan kurtarmak için hizmetin yedeklerini kullanabilirsiniz. Kullanıcı yanlışlıkla bazı verileri silebilir, yanlışlıkla önemli bir tabloyu bırakabilir ve hatta tüm veritabanını bırakabilir. Bir uygulama yanlışlıkla bir uygulama hatası nedeniyle kötü veri ile iyi verilerin üzerine yazabilir, ve benzeri.

Sunucunuzun bir kopyasını zamanında zamanında **geri yükleme** gerçekleştirebilirsiniz. Bu nokta, sunucunuz için yapılandırdığınız yedekleme bekletme süresi içinde olmalıdır. Veriler yeni sunucuya geri yüklendikten sonra, özgün sunucuyu yeni yüklenen sunucuyla değiştirebilir veya geri yüklenen sunucudan gerekli verileri özgün sunucuya kopyalayabilirsiniz.

## <a name="recover-from-an-azure-data-center-outage"></a>Azure veri merkezi kesintisinden kurtarma

Çok sık olmasa da Azure veri merkezlerinde kesintiler yaşanabilir. Bir kesinti meydana geldiğinde, yalnızca birkaç dakika sürebilir, ancak saatlerce sürebilir bir iş kesintiye neden olur.

Bir seçenek, veri merkezi kesintisi sona erdiğinde sunucunuzun çevrimiçi olmasını beklemektir. Bu, sunucunun belirli bir süre için çevrimdışı olmasını göze alabilen uygulamalar için çalışır, örneğin bir geliştirme ortamı. Bir veri merkezinin kesintisi olduğunda, kesintinin ne kadar süreceğini bilemezsiniz, bu nedenle bu seçenek yalnızca sunucunuza bir süre ihtiyacınız yoksa çalışır.

## <a name="geo-restore"></a>Coğrafi Geri Yükleme

Coğrafi geri yükleme özelliği, coğrafi yedekli yedeklemeleri kullanarak sunucuyu geri yükler. Yedeklemeler sunucunuzun [eşleştirilmiş bölgesinde](../best-practices-availability-paired-regions.md)barındırılır. Bu yedeklemelerden başka bir bölgeye geri yükleyebilirsiniz. Coğrafi geri yükleme, yedeklemelerden gelen verileri içeren yeni bir sunucu oluşturur. Yedekleme ve geri yükleme [kavramları makalesinden](concepts-backup.md)coğrafi geri yükleme hakkında daha fazla bilgi edinin.

> [!IMPORTANT]
> Coğrafi geri yükleme, yalnızca sunucuya coğrafi yedekli yedekleme depolama alanı sağlamanız halinde mümkündür. Varolan bir sunucu için yerel olarak yedekli den coğrafi yedekli yedeklemelere geçmek istiyorsanız, varolan sunucunuzun pg_dump kullanarak bir döküm almalı ve coğrafi yedekli yedeklemelerle yapılandırılan yeni oluşturulan bir sunucuya geri yüklemeniz gerekir.

## <a name="cross-region-read-replicas"></a>Bölgeler arası okuma yinelemeleri
İş sürekliliğinizi ve olağanüstü durum kurtarma planlamanızı geliştirmek için bölgeler arası okuma yinelemelerini kullanabilirsiniz. Okuma yinelemeleri, PostgreSQL'in fiziksel çoğaltma teknolojisi kullanılarak eşzamanlı olarak güncelleştirilir. Okuma yinelemeleri, kullanılabilir bölgeler ve [okuma yinelemeleri kavramları makalesinden](concepts-read-replicas.md)nasıl başarısız oylanın başarısız olduğu hakkında daha fazla bilgi edinin. 

## <a name="next-steps"></a>Sonraki adımlar
- [PostgreSQL için Azure Veritabanı'ndaki otomatik yedeklemeler](concepts-backup.md)hakkında daha fazla bilgi edinin. 
- [Azure portalını](howto-restore-server-portal.md) veya Azure [CLI'yi](howto-restore-server-cli.md)kullanarak nasıl geri yükleyin ilerler öğrenin.
- [PostgreSQL için Azure Veritabanı'nda okuma yinelemeleri](concepts-read-replicas.md)hakkında bilgi edinin.