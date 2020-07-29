---
title: Olağanüstü durum kurtarma ayrıntılarına ls
description: Olağanüstü durum kurtarma detaylarını gerçekleştirmek için Azure SQL veritabanı 'nı kullanmaya yönelik kılavuzluk ve en iyi uygulamaları öğrenin.
services: sql-database
ms.service: sql-database
ms.subservice: high-availability
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: mathoma, carlrab
ms.date: 12/18/2018
ms.openlocfilehash: e32f2bf6f353e32fe96cd3c8b109d698cd3d40ef
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84344586"
---
# <a name="performing-disaster-recovery-drills"></a>Olağanüstü durum kurtarma detaylarını gerçekleştirme
[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

Kurtarma iş akışı için uygulama hazırlığını doğrulamanın düzenli aralıklarla gerçekleştirilmesi önerilir. Uygulama davranışının ve veri kaybı etkilerine ve/veya yük devretmenin kapsadığı kesintiye karşı doğrulama iyi bir mühendislik uygulamasıdır. Ayrıca, iş sürekliliği sertifikasının bir parçası olarak birçok sektör standardı için de bir gereksinimdir.

Olağanüstü durum kurtarma detayının gerçekleştirilmesi aşağıdakilerden oluşur:

* Veri katmanı kesintisi benzetimi yapma
* Kurtar
* Uygulama bütünlüğünü doğrulama sonrası kurtarma

[Uygulamanızı iş sürekliliği için nasıl tasarlamış](business-continuity-high-availability-disaster-recover-hadr-overview.md)olduğunuza bağlı olarak, detaya gitmeyi yürütmek için iş akışı farklılık gösterebilir. Bu makalede, Azure SQL veritabanı bağlamında olağanüstü durum kurtarma detayına yönelik en iyi yöntemler açıklanmaktadır.

## <a name="geo-restore"></a>Coğrafi Geri Yükleme

Olağanüstü durum kurtarma detayını gerçekleştirirken olası veri kaybını engellemek için, üretim ortamının bir kopyasını oluşturarak ve uygulamanın yük devretme iş akışını doğrulamak üzere kullanarak bir test ortamı kullanarak detaya gitmeyi gerçekleştirin.

### <a name="outage-simulation"></a>Kesinti simülasyonu

Kesinti benzetimini yapmak için kaynak veritabanını yeniden adlandırabilirsiniz. Bu ad değişikliği uygulama bağlantı hatalarıyla neden olur.

### <a name="recovery"></a>Kurtarma

* Veritabanının coğrafi geri yüklemesini [burada](disaster-recovery-guidance.md)açıklandığı gibi farklı bir sunucuda gerçekleştirin.
* Kurtarılan veritabanına bağlanacak uygulama yapılandırmasını değiştirin ve kurtarmayı tamamladıktan [sonra kurtarma işleminden sonra veritabanını Yapılandır](disaster-recovery-guidance.md) kılavuzunu izleyin.

### <a name="validation"></a>Doğrulama

Uygulama bütünlüğü gönderisini (bağlantı dizeleri, oturum açmalar, temel işlevsellik testi veya standart uygulama sonuçlandır yordamlarının diğer doğrulamaları dahil) doğrulayarak detaya gitmeyi doldurun.

## <a name="failover-groups"></a>Yük devretme grupları

Yük devretme grupları kullanılarak korunan bir veritabanı için, detaya gitme işlemi ikincil sunucuya planlı yük devretme işlemini içerir. Planlı Yük devretme, roller geçildiğinde, yük devretme grubundaki birincil ve ikincil veritabanlarının eşitlenmiş durumda kalmasını sağlar. Planlanmamış yük devretmenin aksine, bu işlem veri kaybına neden olmaz; bu nedenle, detaya gitme üretim ortamında gerçekleştirilebilir.

### <a name="outage-simulation"></a>Kesinti simülasyonu

Kesinti benzetimi yapmak için, Web uygulamasını veya veritabanına bağlı sanal makineyi devre dışı bırakabilirsiniz. Bu kesinti simülasyonu, Web istemcilerinin bağlantı arızalarına neden olur.

### <a name="recovery"></a>Kurtarma

* DR bölgesindeki uygulama yapılandırmasının, tümüyle erişilebilen yeni birincil haline gelen eski ikinciye işaret ettiğini doğrulayın.
* İkincil sunucudan yük devretme grubunun [planlı yük devretmesini](scripts/setup-geodr-and-failover-database-powershell.md) başlatın.
* Kurtarmayı tamamladıktan [sonra kurtarma işleminden sonra veritabanını Yapılandır](disaster-recovery-guidance.md) kılavuzunu izleyin.

### <a name="validation"></a>Doğrulama

Uygulama bütünlüğü sonrası kurtarma (bağlantı, temel işlevsellik testi veya detaya gitme için gereken diğer doğrulamalar dahil) doğrulayarak detaya gitmeyi doldurun.

## <a name="next-steps"></a>Sonraki adımlar

* İş sürekliliği senaryoları hakkında bilgi edinmek için bkz. [süreklilik senaryoları](business-continuity-high-availability-disaster-recover-hadr-overview.md).
* Azure SQL veritabanı otomatik yedeklemeleri hakkında bilgi edinmek için bkz. [SQL veritabanı otomatik yedeklemeleri](automated-backups-overview.md)
* Kurtarmaya yönelik otomatik yedeklemeleri kullanma hakkında bilgi edinmek için bkz. [hizmet tarafından başlatılan yedeklemelerden bir veritabanını geri yükleme](recovery-using-backups.md).
* Daha hızlı kurtarma seçenekleri hakkında daha fazla bilgi edinmek için bkz. [etkin coğrafi çoğaltma](active-geo-replication-overview.md) ve [otomatik yük devretme grupları](auto-failover-group-overview.md).
