---
title: İş sürekliliği - MariaDB için Azure Veritabanı
description: MariaDB hizmeti için Azure Veritabanı'nı kullanırken iş sürekliliği (zamanında geri yükleme, veri merkezi kesintisi, coğrafi geri yükleme) hakkında bilgi edinin.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 3/18/2020
ms.openlocfilehash: c01e0df1f420c8489ca3445d9fa025b251a870f2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79532400"
---
# <a name="understand-business-continuity-in-azure-database-for-mariadb"></a>MariaDB için Azure Veritabanı'nda iş sürekliliğini anlama

Bu makalede, MariaDB için Azure Veritabanı'nın iş sürekliliği ve olağanüstü durum kurtarma için sağladığı özellikler açıklanmaktadır. Veri kaybına veya veritabanınızın ve uygulamanızın kullanılamamasına neden olabilecek yıkıcı olaylardan kurtarma seçenekleri hakkında bilgi edinin. Bir kullanıcı veya uygulama hatası veri bütünlüğünü etkilediğinde, bir Azure bölgesinde kesinti olduğunda veya uygulamanız bakım gerektirdiğinde ne yapmanız gerektiğini öğrenin.

## <a name="features-that-you-can-use-to-provide-business-continuity"></a>İş sürekliliği sağlamak için kullanabileceğiniz özellikler

MariaDB için Azure Veritabanı, otomatik yedeklemeler ve kullanıcıların coğrafi geri yükleme başlatma olanağı nı içeren iş sürekliliği özellikleri sağlar. Her biri Tahmini Kurtarma Süresi (ERT) ve olası veri kaybı için farklı özelliklere sahiptir. Bu seçenekleri anladıktan sonra, aralarından birini seçebilir ve bunları farklı senaryolar için birlikte kullanabilirsiniz. İş sürekliliği planınızı geliştirirken, uygulamanın rahatsız edici olaydan sonra tamamen iyileşmesi için kabul edilebilir maksimum zamanı anlamanız gerekir - bu sizin Kurtarma Süresi Hedefinizdir (RTO). Ayrıca, uygulamanın yıkıcı olaydan sonra kurtarma yaparken kaybetmeyi tolere edebileceği en yüksek son veri güncelleştirmesi miktarını (zaman aralığı) anlamanız gerekir - bu sizin Kurtarma Noktası Hedefinizdir (RPO).

Aşağıdaki tablo, kullanılabilir özellikler için ERT ve RPO'yu karşılaştırır:

| **Özellik** | **Temel** | **Genel Amaç** | **Bellek için iyileştirilmiş** |
| :------------: | :-------: | :-----------------: | :------------------: |
| Yedekten belirli bir noktaya geri yükleme | Bekletme süresi içindeki herhangi bir geri yükleme noktası | Bekletme süresi içindeki herhangi bir geri yükleme noktası | Bekletme süresi içindeki herhangi bir geri yükleme noktası |
| Coğrafi olarak çoğaltılan yedeklemelerden coğrafi geri yükleme | Desteklenmiyor | ERT < 12 saat<br/>RPO < 1 saat | ERT < 12 saat<br/>RPO < 1 saat |

> [!IMPORTANT]
> Sunucuyu silerseniz, sunucuda bulunan tüm veritabanları da silinir ve kurtarılamaz. Silinen bir sunucugeri yükleyemezsiniz.

## <a name="recover-a-server-after-a-user-or-application-error"></a>Kullanıcı veya uygulama hatasından sonra sunucu kurtarma

Bir sunucuyu çeşitli yıkıcı olaylardan kurtarmak için hizmetin yedeklerini kullanabilirsiniz. Kullanıcı yanlışlıkla bazı verileri silebilir, yanlışlıkla önemli bir tabloyu bırakabilir ve hatta tüm veritabanını bırakabilir. Bir uygulama yanlışlıkla bir uygulama hatası nedeniyle kötü veri ile iyi verilerin üzerine yazabilir, ve benzeri.

Sunucunuzun bir kopyasını zamanında zamanında geri yükleme gerçekleştirebilirsiniz. Bu nokta, sunucunuz için yapılandırdığınız yedekleme bekletme süresi içinde olmalıdır. Veriler yeni sunucuya geri yüklendikten sonra, özgün sunucuyu yeni yüklenen sunucuyla değiştirebilir veya geri yüklenen sunucudan gerekli verileri özgün sunucuya kopyalayabilirsiniz.

## <a name="recover-from-an-azure-regional-data-center-outage"></a>Azure bölgesel veri merkezi kesintisinden kurtarma

Çok sık olmasa da Azure veri merkezlerinde kesintiler yaşanabilir. Bir kesinti meydana geldiğinde, yalnızca birkaç dakika sürebilir, ancak saatlerce sürebilir bir iş kesintiye neden olur.

Bir seçenek, veri merkezi kesintisi sona erdiğinde sunucunuzun çevrimiçi olmasını beklemektir. Bu, sunucunun belirli bir süre için çevrimdışı olmasını göze alabilen uygulamalar için çalışır, örneğin bir geliştirme ortamı. Veri merkezinin kesintisi olduğunda, kesintinin ne kadar süreceğini bilemezsiniz, bu nedenle bu seçenek yalnızca sunucunuza bir süre ihtiyacınız yoksa çalışır.

Diğer seçenek, MariaDB'nin coğrafi yedekli yedeklemeleri kullanarak sunucuyu geri yükleyen coğrafi geri yükleme özelliği için Azure Veritabanı'nı kullanmaktır. Bu yedeklemelere, sunucunuzun barındırılan bölgesi çevrimdışı olsa bile erişilebilir. Bu yedeklemelerden başka bir bölgeye geri yüklenebilir ve sunucunuzu yeniden çevrimiçi duruma getirebilirsiniz.

> [!IMPORTANT]
> Coğrafi geri yükleme, yalnızca sunucuya coğrafi yedekli yedekleme depolama alanı sağlamanız halinde mümkündür.

## <a name="next-steps"></a>Sonraki adımlar

- Otomatik yedeklemeler hakkında daha fazla bilgi edinmek [için, MariaDB için Azure Veritabanı'ndaki Yedeklemeler'e](concepts-backup.md)bakın.
- Azure portalını kullanarak bir noktaya geri yüklemek için, [Azure portalını kullanarak veritabanını bir noktaya geri yükleme konusuna](howto-restore-server-portal.md)bakın.

<!--
- To restore to a point in time using Azure CLI, see [restore database to a point in time using CLI](howto-restore-server-cli.md). 
-->
