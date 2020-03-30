---
title: İş Kritik hizmet katmanı
description: Azure SQL Veritabanı İş Kritik katmanı hakkında bilgi edinin
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: sstein
ms.date: 12/04/2018
ms.openlocfilehash: fc328c34c1543a75fdc885087d44b28e24c0850a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79268684"
---
# <a name="business-critical-tier---azure-sql-database"></a>İş Açısından Kritik Katman - Azure SQL Veritabanı

> [!NOTE]
> İş Kritik katmanı, DTU satın alma modelinde Premium olarak adlandırılır. vCore tabanlı satın alma modelinin DTU tabanlı satın alma modeliyle karşılaştırılması için [Azure SQL Veritabanı satın alma modelleri ve kaynaklarına](sql-database-purchase-models.md)bakın.

Azure SQL Veritabanı, altyapı hataları durumunda bile %99,99 kullanılabilirlik sağlamak için bulut ortamı için ayarlanan SQL Server Database Engine mimarisine dayanır. Azure SQL Veritabanı'nda kullanılan üç mimari model vardır:
- Genel Amaç/Standart 
- İş Kritik / Premium
- Hiper Ölçek

Premium/Business Critical hizmet katmanı modeli, veritabanı altyapısı işlemleri kümesini temel alır. Bu mimari model, her zaman kullanılabilir veritabanı altyapı düğümleri bir çoğunluk olduğu ve bakım faaliyetleri sırasında bile iş yükü üzerinde en az performans etkisi olduğu bir gerçeği dayanır.

Azure yükseltmeleri ve işletim sistemi, sürücüler ve SQL Server Veritabanı Altyapısı nın altında yatan düzeltme emaları, son kullanıcılar için en az kesinti süresiyle saydam olarak kullanılır. 

Azure SQL Veritabanı'nın Premium ve İş Açısından Kritik hizmet katmanlarında premium kullanılabilirlik etkinleştirilir ve devam eden bakım işlemleri nedeniyle herhangi bir performans etkisine tahammül edemeyen yoğun iş yükleri için tasarlanmıştır.

Premium modelde, Azure SQL veritabanı tek düğümde işlem ve depolamayı tümleştirir. Bu mimari modelde yüksek kullanılabilirlik, SQL Server [Always On Availability Groups'a](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server)benzer bir teknoloji kullanılarak, dört düğüm kümesinde dağıtılan bilgi işlem (SQL Server Database Engine process) ve depolama (yerel olarak bağlı SSD) çoğaltılması yla elde edilir.

![Veritabanı altyapı düğümleri kümesi](media/sql-database-managed-instance/business-critical-service-tier.png)

Hem SQL veritabanı altyapısı işlemi hem de altta yatan mdf/ldf dosyaları, iş yükünüze düşük gecikme gecikmesi sağlayan yerel olarak bağlı SSD depolama alanıyla aynı düğüme yerleştirilir. Yüksek kullanılabilirlik, SQL Server [Always On Availability Groups'a](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server)benzer bir teknoloji kullanılarak uygulanır. Her veritabanı, müşteri iş yükü için erişilebilir bir birincil veritabanına ve veri kopyalarını içeren üç ikincil işlemiçeren veritabanı düğümleri kümesidir. Birincil düğüm, birincil düğüm herhangi bir nedenle çökerse, verilerin ikincil yinelemelerde kullanılabilir olduğundan emin olmak için ikincil düğümdeğişiklikleri sürekli olarak iter. Failover SQL Server Database Engine tarafından işlenir – ikincil bir yineleme birincil düğüm olur ve kümede yeterli düğüm sağlamak için yeni bir ikincil yineleme oluşturulur. İş yükü otomatik olarak yeni birincil düğüme yönlendirilir.

Buna ek olarak, İş Açısından Kritik küme, birincil iş yükünüzün performansını etkilememesi gereken salt okunur sorguları (örneğin raporlar) çalıştırmak için kullanılabilecek ücretsiz yerleşik salt okunur düğümü sağlayan yerleşik [Okuma Ölçeği-Out](sql-database-read-scale-out.md) özelliğine sahiptir.

## <a name="when-to-choose-this-service-tier"></a>Bu hizmet katmanı ne zaman seçilir?

İş Kritik hizmet katmanı, temel SSD depolamadan (ortalama 1-2 ms) düşük gecikme gecikmeli yanıtlar gerektiren uygulamalar, temel altyapı başarısız olursa veya raporları, analizleri ve yalnızca salt yükleme ye ihtiyaç duyarsa hızlı kurtarma için tasarlanmıştır birincil veritabanının ücretsiz okunabilir ikincil yinelemesorguları.

Genel Amaç katmanı yerine Business Critical hizmet katmanını seçmeniz için temel nedenler şunlardır:
-   Düşük IO gecikme gereksinimleri – depolama katmanından hızlı yanıt alması gereken iş yükü (ortalama 1-2 milisaniye) İş Kritik katmanını kullanmalıdır. 
-   Uygulama ve veritabanı arasında sık sık iletişim. Uygulama katmanı önbelleğe alma kaldıraç veya [toplu işleme talep](sql-database-use-batching-to-improve-performance.md) edemez ve hızlı bir şekilde işlenmesi gereken birçok SQL sorguları göndermek gerekir Uygulama İş Kritik katmanı için iyi adaylardır.
-   Çok sayıda güncelleştirme – ekleme, güncelleme ve silme işlemleri, bellekteki (kirli sayfa) veri `CHECKPOINT` sayfalarını değiştirerek işlem le birlikte veri dosyalarına kaydedilmesi gereken işlemleri değiştirir. Olası veritabanı altyapısı işlemi çökmesi veya çok sayıda kirli sayfayla veritabanının başarısız lığı Genel Amaç katmanında kurtarma süresini artırabilir. Birçok bellek içi değişikye neden olan bir iş yükünüz varsa İş Açısından Kritik katmanıkullanın. 
-   Verileri değiştiren uzun süren hareketler. Daha uzun süre açık olan hareketler, günlük boyutunu ve Sanal günlük [dosyaları (VLF)](https://docs.microsoft.com/sql/relational-databases/sql-server-transaction-log-architecture-and-management-guide#physical_arch)sayısını artırabilecek günlük dosyasının kesilmesini engeller. Vlf yüksek sayıda başarısız olduktan sonra veritabanı kurtarma yavaşlatabilir.
-   Ücretsiz ikincil salt okunur yinelemeye yönlendirilebilen raporlama ve analitik sorgularla iş yükü.
- Daha yüksek esneklik ve arızalardan daha hızlı iyileşme. Sistem hatası durumunda, birincil örnekteki veritabanı devre dışı bırakılır ve ikincil yinelemelerden biri hemen sorguları işlemeye hazır yeni okuma-yazma birincil veritabanı na dönüştürülecektir. Veritabanı altyapısının günlük dosyasındaki hareketleri çözümleyip yeniden oluşturması ve tüm verileri bellek arabelleğinde yüklemesi gerekmez.
- Gelişmiş veri bozulması koruması - Business Critical katmanı, iş sürekliliği amacıyla kamera arkası veritabanı yinelemelerinden yararlanır ve böylece hizmet, SQL Server veritabanı [yansıtma ve kullanılabilirlik grupları](https://docs.microsoft.com/sql/sql-server/failover-clusters/automatic-page-repair-availability-groups-database-mirroring)için kullanılan teknolojinin aynısı olan otomatik sayfa onarımını da kullanır. Yinelemenin veri bütünlüğü sorunu nedeniyle sayfayı okuyamaması durumunda, sayfanın yeni bir kopyası başka bir yinelemeden alınır ve veri kaybı veya müşteri kapalı kalma süresi olmadan okunamayan sayfanın yerine alınır. Veritabanında jeo-ikincil yineleme varsa, bu işlevsellik Genel Amaç katmanında geçerlidir.
- Daha yüksek kullanılabilirlik - Multi-AZ yapılandırmasındaki Business Critical katmanı, Genel Amaçlı katmanının %99,99'una kıyasla %99,995 kullanılabilirliği garanti eder.
- Hızlı coğrafi kurtarma - Coğrafi çoğaltma ile yapılandırılan Business Critical katmanı, dağıtılan saatlerin %100'ü için 5 sn'lik garantili Kurtarma noktası hedefine (RPO) ve 30 sn'lik Kurtarma süresi hedefine (RTO) sahiptir.

## <a name="next-steps"></a>Sonraki adımlar

- [Yönetilen Örnek'te](sql-database-managed-instance-resource-limits.md#service-tier-characteristics)Business Critical katmanının kaynak özelliklerini (çekirdek sayısı, IO, bellek) bulun, [vCore modelinde](sql-database-vcore-resource-limits-single-databases.md#business-critical---provisioned-compute---gen4) veya [DTU modelinde](sql-database-dtu-resource-limits-single-databases.md#premium-service-tier)tek veritabanı veya [vCore modelinde](sql-database-vcore-resource-limits-elastic-pools.md#business-critical---provisioned-compute---gen4) ve [DTU modelinde](sql-database-dtu-resource-limits-elastic-pools.md#premium-elastic-pool-limits)Elastik havuz.
- Genel [Amaç](sql-database-service-tier-general-purpose.md) ve [Hiper Ölçekli](sql-database-service-tier-hyperscale.md) katmanlar hakkında bilgi edinin.
- Servis [Kumaşı](../service-fabric/service-fabric-overview.md)hakkında bilgi edinin.
- Yüksek kullanılabilirlik ve olağanüstü durum kurtarma için daha fazla seçenek için [İş Sürekliliği'ne](sql-database-business-continuity.md)bakın.
