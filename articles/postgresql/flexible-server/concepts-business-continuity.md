---
title: PostgreSQL için Azure veritabanı ile iş sürekliliği 'ne genel bakış-esnek sunucu
description: PostgreSQL için Azure veritabanı-esnek sunucu ile iş sürekliliği kavramlarını öğrenin
author: sr-msft
ms.author: srranga
ms.service: postgresql
ms.topic: conceptual
ms.date: 09/22/2020
ms.openlocfilehash: 264bb8c66510c90fecf12d2e4e68bd969b4fb474
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90941390"
---
# <a name="overview-of-business-continuity-with-azure-database-for-postgresql---flexible-server"></a>PostgreSQL için Azure veritabanı ile iş sürekliliği 'ne genel bakış-esnek sunucu

> [!IMPORTANT]
> PostgreSQL için Azure veritabanı-esnek sunucu önizlemededir

PostgreSQL için Azure veritabanı 'nda **iş sürekliliği** -esnek sunucu, işletmenizin kesintiye uğramasıyla, özellikle de bilgi işlem altyapısından çalışmaya devam etmesini sağlayan mekanizmaların, ilkelerin ve yordamların anlamına gelir. Çoğu durumda, esnek sunucu, bulut ortamında gerçekleşebilecek ve uygulamalarınızı ve iş süreçlerinizi çalışır durumda tutan kesintiye uğratan olayları idare eder. Ancak, şu gibi otomatik olarak işlenemeyebilir bazı olaylar vardır:

- Kullanıcı yanlışlıkla tablodaki bir satırı siler veya güncelleştirir.
- Deprem bir güç kesintisi ve geçici bir veri merkezini veya bir kullanılabilirlik bölgesini devre dışı bırakır.
- Bir hata veya güvenlik sorununu çözmesi için veritabanı düzeltme eki uygulama gerekiyor.

Esnek sunucu, planlı ve planlanmamış kesinti olayları durumunda görev açısından kritik veritabanlarınızın kapalı kalma süresini azaltır ve verileri koruyan özellikler sağlar. Daha önce güçlü dayanıklılık ve kullanılabilirlik sunan Azure altyapısının üzerine inşa edilen esnek sunucu, ek hata koruma, adres kurtarma süresi gereksinimleri sağlayan ve veri kaybı pozlamasını azaltan iş sürekliliği özelliklerine sahiptir. Uygulamalarınızı mimarilerlemesine göre, kurtarma noktası hedefi (RPO) olan kurtarma süresi hedefi (RTO) ve veri kaybı pozlaması olan kapalı kalma süresini göz önünde bulundurmanız gerekir. Örneğin, iş açısından kritik veritabanınız bir test veritabanıyla karşılaştırıldığında çok daha sıkı çalışma süresi gereksinimleri gerektirir.  

> [!IMPORTANT]
> Çalışma süresi% hizmet düzeyi sözleşmesi (SLA) Önizleme sırasında sunulmaz. 

Aşağıdaki tabloda esnek sunucu tarafından sunulan özellikler gösterilmektedir.


| **Özellik** | **Açıklama** | **Dikkat edilmesi gerekenler** |
| ---------- | ----------- | ------------ |
| **Otomatik yedeklemeler** | Esnek sunucu, veritabanı dosyalarınızın günlük yedeklemesini otomatik olarak gerçekleştirir ve işlem günlüklerini sürekli olarak yedekler. Yedeklemeler 7 gün 35 güne kadar tutulabilirler. Veritabanı sunucunuzu, yedekleme bekletme süreniz içinde herhangi bir zaman noktasına geri yükleyebileceksiniz. RTO, geri yüklenecek verilerin boyutuna, günlük kurtarma gerçekleştirme zamanına bağlıdır. 12 saate kadar birkaç dakikadan fazla olabilir. Daha fazla ayrıntı için bkz. [Kavramlar-yedekleme ve geri yükleme](./concepts-backup-restore.md). |Yedekleme verileri bölge içinde kalır. |
| **Bölge yedekli yüksek kullanılabilirlik** | Esnek sunucu, birincil ve bekleme sunucularının bir bölgedeki iki farklı kullanılabilirlik bölgesinde dağıtıldığı bölge yedekli yüksek kullanılabilirlik (HA) yapılandırmasıyla dağıtılabilir. Bu HA yapılandırması, veritabanlarınızı bölge düzeyindeki hatalardan korur ve ayrıca planlanmış ve planlanmamış kesinti olayları sırasında uygulamanın kapalı kalma süresini azaltmaya yardımcı olur. Birincil sunucudan alınan veriler, zaman uyumlu modda bekleme çoğaltmaya çoğaltılır. Birincil sunucuya herhangi bir kesinti olması durumunda sunucu otomatik olarak bekleme çoğaltmaya devredildi. RTO 'ıN çoğu zaman, 60s-120s içinde olması beklenir. RPO 'nun sıfır olması beklenir (veri kaybı yok). Daha fazla bilgi için bkz. [Kavramlar-yüksek kullanılabilirlik](./concepts-high-availability.md). | Genel amaçlı ve bellek için iyileştirilmiş işlem katmanlarında desteklenir. Yalnızca birden fazla bölgenin kullanılabildiği bölgelerde kullanılabilir. |
| **Premium yönetilen diskler** | Veritabanı dosyaları, yüksek oranda dayanıklı ve güvenilir bir Premium yönetilen depolama alanında depolanır. Bu, otomatik veri kurtarma özellikleri ile bir kullanılabilirlik alanında depolanan çoğaltmanın üç kopyası ile veri artıklığı sağlar. Daha fazla bilgi için bkz. [yönetilen diskler belgeleri](https://docs.microsoft.com/azure/virtual-machines/managed-disks-overview). | Bir kullanılabilirlik alanı içinde depolanan veriler. |
| **Bölge yedekli yedekleme** | Esnek sunucu yedeklemeleri, bir bölge içinde yedekli bir depolama alanında otomatik olarak ve güvenli bir şekilde depolanır. Sunucunuzun sağlandığı bölge düzeyindeki bir başarısızlık sırasında ve sunucunuz bölge yedekliliği ile yapılandırılmadıysa, farklı bir bölgedeki en son geri yükleme noktasını kullanarak veritabanınızı geri yüklemeye devam edebilirsiniz. Daha fazla bilgi için bkz. [Kavramlar-yedekleme ve geri yükleme](./concepts-backup-restore.md).| Yalnızca birden fazla bölgenin kullanılabildiği bölgelerde geçerlidir.|


## <a name="planned-downtime-events"></a>Planlanan kapalı kalma süresi olayları
Aşağıda bazı planlı bakım senaryoları verilmiştir. Bu olaylar genellikle birkaç dakikalık kapalı kalma süresini ve veri kaybı olmadan oluşur.

| **Senaryo** | **İşleme**|
| ------------------- | ----------- | 
| <b>İşlem Ölçeklendirmesi (Kullanıcı tarafından başlatılan)| İşlem ölçeklendirme işlemi sırasında, etkin kontrol noktalarının tamamlanabilmesi için, istemci bağlantıları boşaltılır, kaydedilmeyen tüm işlemler iptal edilir, depolama ayrılır ve sonra kapanır. Ölçeklendirilmiş işlem yapılandırmasıyla aynı veritabanı sunucu adına sahip yeni bir esnek sunucu sağlandı. Daha sonra depolama alanı yeni sunucuya eklenir ve istemci bağlantılarını kabul etmeden önce gerekirse kurtarma işlemi gerçekleştirilir. |
| <b>Depolama alanını ölçeklendirme (Kullanıcı tarafından başlatılan) | Depolama alanı ölçeklendirme işlemi başlatıldığında, etkin kontrol noktalarının tamamlanmasını izin verilir, istemci bağlantıları kaldırılır, kaydedilmeyen tüm işlemler iptal edilir ve sonra kapatılır. Depolama alanı istenen boyuta ölçeklendirilir ve sonra yeni sunucuya eklenir. İstemci bağlantılarını kabul etmeden önce gerekirse bir kurtarma gerçekleştirilir. Depolama boyutunu ölçeklendirmenin desteklenmediğini unutmayın. |
| <b>Yeni yazılım dağıtımı (Azure-başlatıldı) | Yeni özellik dağıtımı veya hata düzeltmeleri, hizmetin planlı bakımının bir parçası olarak otomatik olarak gerçekleşir ve bu etkinliklerin ne zaman gerçekleşmesini zamanlayabilirsiniz. Daha fazla bilgi için [portalınızı](https://aka.ms/servicehealthpm)kontrol edin. | 
| <b>İkincil sürüm yükseltmeleri (Azure-başlatıldı) | PostgreSQL için Azure veritabanı, veritabanı sunucularını otomatik olarak Azure tarafından belirlenen alt sürüme ekler. Hizmetin planlı bakım kapsamında olur. Veritabanı sunucusu yeni ikincil sürümle otomatik olarak yeniden başlatılır. Daha fazla bilgi için bkz. [Belgeler](https://docs.microsoft.com/azure/postgresql/concepts-monitoring#planned-maintenance-notification). Ayrıca, [portalınızı](https://aka.ms/servicehealthpm)kontrol edebilirsiniz.| 

 Esnek sunucu, bölgesel olarak **yedekli yüksek kullanılabilirliğe**göre yapılandırıldığında, esnek sunucu önce bekleme sunucusunda ölçeklendirmeyi ve bakım işlemlerini gerçekleştirir. Daha fazla bilgi için bkz. [Kavramlar-yüksek kullanılabilirlik](./concepts-high-availability.md).

##  <a name="unplanned-downtime-mitigation"></a>Planlanmamış kapalı kalma süresi azaltma

Plansız kapalı kalma süreleri, temeldeki donanım hatası, ağ sorunları ve yazılım hataları gibi öngörülemeyen kesintilerin bir sonucu olarak gerçekleşebilir. Yüksek kullanılabilirlik ile yapılandırılan veritabanı sunucusu beklenmedik şekilde devre dışı bırakılırsa, bekleme çoğaltması etkinleştirilir ve istemciler işlemlerini sürdürebilir. Yüksek kullanılabilirlik (HA) ile yapılandırılmamışsa, yeniden başlatma denemesi başarısız olursa, otomatik olarak yeni bir veritabanı sunucusu sağlanır. Planlanmamış kapalı kalma süresi önlenemez, esnek sunucu, insan müdahalesine gerek kalmadan kurtarma işlemlerini otomatik olarak gerçekleştirerek kapalı kalma süresini azaltmaya yardımcı olur. 
   
### <a name="unplanned-downtime-failure-scenarios-and-service-recovery"></a>Planlanmamış kapalı kalma süresi: hata senaryoları ve hizmet kurtarma
Aşağıda bazı planlanmamış hata senaryoları ve kurtarma işlemi verilmiştir. 

| **Senaryo** | **Kurtarma işlemi [HA dışı]** | **Kurtarma işlemi [HA]** |
| ---------- | ---------- | ------- |
| <B>Veritabanı sunucusu hatası | Veritabanı sunucusu kapalıysa Azure, veritabanı sunucusunu yeniden başlatmayı dener. Bu işlem başarısız olursa, veritabanı sunucusu başka bir fiziksel düğümde yeniden başlatılır.  <br /> <br /> Kurtarma süresi (RTO), büyük işlem ve veritabanı sunucusu başlatma işlemi sırasında gerçekleştirilecek kurtarma hacmi gibi hata sırasında etkinlik dahil çeşitli faktörlere bağımlıdır. <br /> <br /> PostgreSQL veritabanlarını kullanan uygulamaların, kesilen bağlantıları ve başarısız işlemleri algılayıp yeniden denedikleri bir şekilde oluşturulması gerekir. | Veritabanı sunucusu hatası algılanırsa, sunucu yük devri sunucusuna devredildi, bu nedenle kapalı kalma süresini azaltır. Daha fazla bilgi için bkz. [ha kavramları sayfası](../concepts-high-availability.md). RTO 'ın sıfır veri kaybı ile 60-120s olması beklenir. |
| <B>Depolama hatası | Uygulamalar, disk arızası veya fiziksel blok bozulması gibi depolama ile ilgili herhangi bir sorun için herhangi bir etkisi görmez. Veriler üç kopyada depolandığından, verilerin kopyası, çalışan depolama alanı tarafından sunulur. Bozuk veri bloğu otomatik olarak onarıldı ve verilerin yeni bir kopyası otomatik olarak oluşturulur. | Tüm nadir ve kurtarılabilir olmayan hatalar için tüm depolama alanı erişilemez durumda, kapalı kalma süresini azaltmak için esnek sunucunun yük devri çoğaltmaya devredilmesini sağlar. Daha fazla bilgi için bkz. [ha kavramları sayfası](../concepts-high-availability.md). |
| <b> Mantıksal/Kullanıcı hataları | Yanlışlıkla bırakılan tablolar veya yanlış güncelleştirilmiş veriler gibi Kullanıcı hatalarından kurtarmak için, bir [zaman içinde kurtarma](https://docs.microsoft.com/azure/postgresql/concepts-backup) (sür) gerçekleştirmeniz gerekir. Geri yükleme işlemini gerçekleştirirken, hata oluşmadan hemen önce geçen süre olan özel geri yükleme noktasını belirtirsiniz.<br> <br>  Veritabanı sunucusundaki tüm veritabanları yerine veritabanlarının yalnızca bir alt kümesini veya belirli tabloları geri yüklemek isterseniz, veritabanı sunucusunu yeni bir örneğe geri yükleyebilir, [pg_dump](https://www.postgresql.org/docs/11/app-pgdump.html)aracılığıyla tabloları dışarı aktarabilir ve ardından bu tabloları veritabanınıza geri yüklemek için [pg_restore](https://www.postgresql.org/docs/11/app-pgrestore.html) kullanabilirsiniz. | Tüm değişiklikler bekleyen çoğaltmaya zaman uyumlu olarak çoğaltıldığından, bu kullanıcı hataları yüksek kullanılabilirlik ile korunmaz. Bu tür hatalardan kurtarmak için zaman noktası geri yükleme gerçekleştirmeniz gerekir. |
| <b> Kullanılabilirlik bölgesi hatası | Bölge düzeyindeki bir hatadan kurtarmak için, yedeklemeyi kullanarak bir zaman içinde geri yükleme gerçekleştirebilir ve en son verileri geri yüklemek için en son zamana sahip özel bir geri yükleme noktası seçebilirsiniz. Yeni bir esnek sunucu, etkilenmeyen başka bir bölgeye dağıtılır. Geri yüklemek için geçen süre, önceki yedeklemeye ve kurtarılacak işlem günlüklerinin hacmine bağlıdır. | Esnek sunucu, sıfır veri kaybı ile 60-120s içindeki bekleme sunucusuna otomatik olarak yük devreder. Daha fazla bilgi için bkz. [ha kavramları sayfası](../concepts-high-availability.md). | 
| <b> Bölge hatası | Çapraz bölge okuma çoğaltması ve yedekleme özelliklerinin coğrafi geri yüklemesi henüz önizlemede desteklenmiyor. | |


> [!IMPORTANT]
> Silinen sunucular **cannot**   geri yüklenemez. Sunucuyu silerseniz, sunucuya ait olan tüm veritabanları da silinir ve kurtarılamaz. [Azure resource lock](https://docs.microsoft.com/azure/azure-resource-manager/management/lock-resources)   Sunucunuzun yanlışlıkla silinmesini önlemeye yardımcı olmak için Azure Kaynak kilidini kullanın.


## <a name="next-steps"></a>Sonraki adımlar

-    [Bölge yedekli yüksek kullanılabilirlik](./concepts-high-availability.md) hakkında bilgi edinin
-   [Yedekleme ve kurtarma](./concepts-backup-restore.md) hakkında bilgi edinin
