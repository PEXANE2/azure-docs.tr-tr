---
title: Afet Kurtarma Matkaplar
description: Olağanüstü durum kurtarma tatbikatları gerçekleştirmek için Azure SQL Veritabanı'nı kullanmak için rehberlik ve en iyi uygulamaları öğrenin.
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
ms.openlocfilehash: 3ca00a03976ae38b7956616b8287220a7bc5998c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73825845"
---
# <a name="performing-disaster-recovery-drill"></a>Olağanüstü Durum Kurtarma Tatbikatı

Kurtarma iş akışı için uygulamaya hazır olduğunun doğrulanmasının periyodik olarak yapılması önerilir. Uygulama davranışının ve veri kaybının ve/veya başarısızlığın neden olduğu aksaklıkların doğrulanması iyi bir mühendislik uygulamasıdır. Aynı zamanda iş sürekliliği belgelendirmesinin bir parçası olarak çoğu endüstri standardı tarafından bir gerekliliktir.

Olağanüstü durum kurtarma tatbikatı yapmak aşağıdakilerden oluşur:

* Veri katmanı kesintisinin benzetimi
* Kurtarma
* Uygulama bütünlüğü sonrası kurtarmayı doğrulama

[Uygulamanızı iş sürekliliği için](sql-database-business-continuity.md)nasıl tasarladığınıza bağlı olarak, matkabı çalıştırmak için iş akışı değişebilir. Bu makalede, Azure SQL Veritabanı bağlamında bir olağanüstü durum kurtarma sondajı yürütmek için en iyi uygulamalar açıklanmaktadır.

## <a name="geo-restore"></a>Coğrafi Geri Yükleme

Olağanüstü durum kurtarma matkabını çalıştırırken olası veri kaybını önlemek için, üretim ortamının bir kopyasını oluşturarak ve uygulamanın başarısız iş akışını doğrulamak için kullanarak bir test ortamı kullanarak matkabı gerçekleştirin.

### <a name="outage-simulation"></a>Kesinti simülasyonu

Kesintisi simüle etmek için kaynak veritabanını yeniden adlandırabilirsiniz. Bu ad değişikliği uygulama bağlantısı hatalarına neden olur.

### <a name="recovery"></a>Kurtarma

* [Burada](sql-database-disaster-recovery.md)açıklandığı gibi veritabanının coğrafi geri yüklemesini farklı bir sunucuya gerçekleştirin.
* Kurtarılan veritabanına bağlanmak için uygulama yapılandırmasını değiştirin ve kurtarmayı tamamlamak için kurtarma kılavuzundan [sonra bir veritabanını yapılandırın.](sql-database-disaster-recovery.md)

### <a name="validation"></a>Doğrulama

Uygulama bütünlüğü kurtarma sonrası kurtarma (bağlantı dizeleri, oturum açma, temel işlevsellik testi veya standart uygulama imzalama yordamları parçası dahil) doğrulayarak matkap tamamlayın.

## <a name="failover-groups"></a>Yük devretme grupları

Failover grupları kullanılarak korunan bir veritabanı için, alıştırma işlemi ikincil sunucuya planlanmış başarısız olmayı içerir. Planlanan hata, roller değiştirildiğinde, başarısız gruptaki birincil ve ikincil veritabanlarının eşit kalmasını sağlar. Planlanmamış arızanın aksine, bu işlem veri kaybına neden olmaz, bu nedenle matkap üretim ortamında gerçekleştirilebilir.

### <a name="outage-simulation"></a>Kesinti simülasyonu

Kesintisi simüle etmek için, web uygulamasını veya veritabanına bağlı sanal makineyi devre dışı kullanabilirsiniz. Bu kesinti simülasyonu, web istemcileri için bağlantı hatalarıyla sonuçlanır.

### <a name="recovery"></a>Kurtarma

* DR bölgesindeki uygulama yapılandırmasının, tamamen erişilebilir yeni birincil olan eski ikincil maddeyi işaret edeceğinden emin olun.
* İkincil sunucudan failover grubunun [planlı hatasını](scripts/sql-database-setup-geodr-and-failover-database-powershell.md) başlatın.
* Kurtarmayı tamamlamak için kurtarma kılavuzundan [sonra bir veritabanını yapılandır'ı](sql-database-disaster-recovery.md) izleyin.

### <a name="validation"></a>Doğrulama

Uygulama bütünlüğü kurtarma sonrası kurtarmayı doğrulayarak (bağlantı, temel işlevsellik testi veya matkap imzalamaları için gerekli diğer doğrulamalar dahil) delinme yi tamamlayın.

## <a name="next-steps"></a>Sonraki adımlar

* İş sürekliliği senaryoları hakkında bilgi edinmek için [Süreklilik senaryolarına](sql-database-business-continuity.md)bakın.
* Azure SQL Veritabanı otomatik yedeklemeleri hakkında bilgi edinmek için [SQL Veritabanı otomatik yedeklemelerine](sql-database-automated-backups.md) bakın
* Kurtarma için otomatik yedeklemekullanma hakkında bilgi edinmek [için, hizmet tarafından başlatılan yedeklemelerden bir veritabanıgeri yükleme'ye](sql-database-recovery-using-backups.md)bakın.
* Daha hızlı kurtarma seçenekleri hakkında bilgi edinmek için [Etkin coğrafi çoğaltma](sql-database-active-geo-replication.md) ve Otomatik başarısız [gruplarına](sql-database-auto-failover-group.md)bakın.
