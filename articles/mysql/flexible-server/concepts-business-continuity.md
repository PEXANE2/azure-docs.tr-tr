---
title: İş sürekliliği 'ne genel bakış-MySQL için Azure veritabanı esnek sunucu
description: MySQL için Azure veritabanı esnek sunucusu ile iş sürekliliği kavramlarını öğrenin
author: mksuni
ms.author: sumuth
ms.service: mysql
ms.topic: conceptual
ms.date: 09/21/2020
ms.openlocfilehash: c29e952e22aaccf31c10de8f6e16d240b4660a23
ms.sourcegitcommit: 80034a1819072f45c1772940953fef06d92fefc8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2020
ms.locfileid: "93240724"
---
# <a name="overview-of-business-continuity-with-azure-database-for-mysql---flexible-server-preview"></a>MySQL için Azure veritabanı ile iş sürekliliği 'ne genel bakış-esnek sunucu (Önizleme)

> [!IMPORTANT]
> MySQL için Azure veritabanı-esnek sunucu şu anda genel önizlemededir.

MySQL için Azure veritabanı esnek sunucu, planlı ve plansız bir kesinti durumunda veritabanlarınızı koruyan iş sürekliliği özellikleri sunar. Otomatik yedeklemeler ve yüksek kullanılabilirlik gibi özellikler, farklı kurtarma süresi ve veri kaybı Etkilenmeler ile farklı hata koruması düzeylerine sahiptir. Uygulamanızı hatalara karşı korumak üzere mimarduğunuzda, her bir uygulama için kurtarma süresi hedefini (RTO) ve kurtarma noktası hedefini (RPO) göz önünde bulundurmanız gerekir. RTO, kapalı kalma süresi toleransı ve RPO, veritabanı hizmetine bir kesinti sonrasında veri kaybı toleransı sağlar.

Aşağıdaki tabloda esnek sunucu tarafından sunulan özellikler gösterilmektedir.

| **Özellik** | **Açıklama** | **Kısıtlamalar** |
| ---------- | ----------- | ------------ |
| **Yedekleme & kurtarma** | Esnek sunucu, veritabanı dosyalarınızın günlük yedeklemesini otomatik olarak gerçekleştirir ve işlem günlüklerini sürekli olarak yedekler. Yedeklemeler, 1 ila 35 gün arasında herhangi bir süre korunabilir. Veritabanı sunucunuzu, yedekleme bekletme süreniz içinde herhangi bir zaman noktasına geri yükleyebileceksiniz. Kurtarma süresi, geri yüklenecek verilerin boyutuna, günlük kurtarma gerçekleştirme zamanına göre değişir. Daha fazla ayrıntı için [bkz. kavramlar-yedekleme ve geri yükleme](./concepts-backup-restore.md) . |Yedekleme verileri bölge içinde kalır |
| **Yerel yedekli yedekleme** | Esnek sunucu yedeklemeleri, bir bölge içinde ve aynı Kullanılabilirlik bölgesinde yerel olarak yedekli bir depolama alanında otomatik olarak ve güvenli bir şekilde depolanır. Yerel olarak yedekli yedeklemeler, sunucu yedekleme veri dosyalarını birincil bölgedeki tek bir fiziksel konum içinde üç kez çoğaltır. Yerel olarak yedekli yedekleme depolaması, belirli bir yıl boyunca en az% 99,999999999 (11 nines) nesne dayanıklılığı sağlar. Daha fazla ayrıntı için [bkz. kavramlar-yedekleme ve geri yükleme](./concepts-backup-restore.md) .| Tüm bölgelerde geçerlidir |
| **Bölge yedekli yüksek kullanılabilirlik** | Esnek sunucu, bir bölgedeki iki farklı kullanılabilirlik bölgesinde birincil ve bekleme sunucularını dağıtan yüksek kullanılabilirlik modunda dağıtılabilir. Bu, bölge düzeyindeki hatalardan korur ve planlanmış ve planlanmamış kapalı kalma süresi olayları sırasında uygulamanın kapalı kalma süresini azaltmaya yardımcı olur. Birincil sunucudan alınan veriler, eşzamanlı çoğaltmaya zaman uyumlu olarak çoğaltılır. Herhangi bir kesinti süresi boyunca, veritabanı sunucusu otomatik olarak bekleme çoğaltmaya devredildi. Daha fazla ayrıntı için [Kavramlar-yüksek kullanılabilirlik](./concepts-high-availability.md) bölümüne bakın. | Genel amaçlı ve bellek için iyileştirilmiş işlem katmanlarında desteklenir. Yalnızca birden fazla bölgenin kullanılabildiği bölgelerde kullanılabilir.|
| **Premium dosya paylaşımları** | Veritabanı dosyaları, otomatik veri kurtarma özellikleri ile bir kullanılabilirlik alanında depolanan çoğaltmanın üç kopyası ile veri artıklığı sağlayan, yüksek oranda dayanıklı ve güvenilir bir Azure Premium dosya paylaşımlarında saklanır. Daha fazla ayrıntı için [Premium dosya paylaşımlarına](../../storage/files/storage-how-to-create-premium-fileshare.md) bakın. | Bir kullanılabilirlik alanı içinde depolanan veriler |

> [!IMPORTANT]
> Önizleme dönemi boyunca çalışma süresi, RTO ve RPO SLA 'Sı sunulmaz. Yalnızca bilgi ve planlama amaçlarıyla bu sayfada sunulan ayrıntılar.

## <a name="planned-downtime-mitigation"></a>Planlanmış kapalı kalma süresi hafifletme

Kapalı kalma süresi olan bazı planlı bakım senaryoları aşağıda verilmiştir:

| **Senaryo** | **İşleme**|
| :------------ | :----------- |
| **İşlem Ölçeklendirmesi (Kullanıcı)**| İşlem ölçekleme işlemi gerçekleştirdiğinizde, ölçeklendirilmiş işlem yapılandırması kullanılarak yeni bir esnek sunucu sağlanır. Var olan veritabanı sunucusunda, etkin kontrol noktalarının tamamlanabilmesi için, istemci bağlantıları boşaltılır, kaydedilmeyen tüm işlemler iptal edilir ve sonra kapatılır. Daha sonra depolama alanı yeni sunucuya eklenir ve istemci bağlantılarını kabul etmeden önce gerekirse kurtarma işlemi gerçekleştirilir. |
| **Yeni yazılım dağıtımı (Azure)** | Yeni özellik dağıtımı veya hata düzeltmeleri, hizmetin planlı bakımının bir parçası olarak otomatik olarak gerçekleşir ve bu etkinliklerin ne zaman gerçekleşmesini zamanlayabilirsiniz. Daha fazla bilgi için [belgelere](https://aka.ms/servicehealthpm)bakın ve ayrıca [portalınızı](https://aka.ms/servicehealthpm) denetleyin |
| **İkincil sürüm yükseltmeleri (Azure)** | MySQL için Azure veritabanı, veritabanı sunucularını otomatik olarak Azure tarafından belirlenen alt sürüme ekler. Hizmetin planlı bakım kapsamında olur. Bu, saniye cinsinden kısa bir kapalı kalma süresine neden olur ve veritabanı sunucusu yeni ikincil sürümle otomatik olarak yeniden başlatılır. Daha fazla bilgi için [belgelere](../concepts-monitoring.md#planned-maintenance-notification)bakın ve ayrıca [portalınızı](https://aka.ms/servicehealthpm)kontrol edin.|

Esnek sunucu, bölgesel olarak **yedekli yüksek kullanılabilirliğe** göre yapılandırıldığında, esnek sunucu önce bekleme sunucusunda, sonra da yük devretme olmadan birincil sunucuda işlemler gerçekleştirir. Daha fazla ayrıntı için [Kavramlar-yüksek kullanılabilirlik](./concepts-high-availability.md) bölümüne bakın.

## <a name="unplanned-downtime-mitigation"></a>Planlanmamış kapalı kalma süresi azaltma

Temel alınan donanım hatası, ağ sorunları ve yazılım hataları da dahil, öngörülemeyen hataların bir sonucu olarak plansız kapalı kalma süreleri meydana gelebilir. Veritabanı sunucusu beklenmedik şekilde kapanıyorsa, yüksek kullanılabilirlik [HA] ile yapılandırıldıysa, bekleme çoğaltması etkinleştirilir. Aksi takdirde, yeni bir veritabanı sunucusu otomatik olarak sağlanır. Planlanmamış kapalı kalma süresi önlenemez, esnek sunucu, kurtarma işlemlerini hem veritabanı sunucusunda hem de depolama katmanlarında, insan müdahalesine gerek kalmadan otomatik olarak gerçekleştirerek kapalı kalma süresini azaltır.

### <a name="unplanned-downtime-failure-scenarios-and-service-recovery"></a>Planlanmamış kapalı kalma süresi: hata senaryoları ve hizmet kurtarma

Bazı planlanmamış hata senaryoları ve kurtarma işlemi aşağıda verilmiştir:

| **Senaryo** | **Kurtarma işlemi [HA dışı]** | **Kurtarma işlemi [HA]** |
| :---------- | ---------- | ------- |
| **Veritabanı sunucusu hatası** | Temel alınan bazı donanım hatası nedeniyle veritabanı sunucusu kapalıysa, etkin bağlantılar bırakılır ve tüm esnek işlemler iptal edilir. Azure, veritabanı sunucusunu yeniden başlatmayı deneyecek. Bu başarılı olursa, veritabanı kurtarma gerçekleştirilir. Yeniden başlatma başarısız olursa, veritabanı sunucusu başka bir fiziksel düğümde yeniden başlatmaya çalışır.  <br /> <br /> Kurtarma süresi (RTO), büyük işlem ve veritabanı sunucusu başlatma işlemi sırasında gerçekleştirilecek kurtarma miktarı gibi hata sırasında etkinlik dahil çeşitli faktörlere bağımlıdır. <br /> <br /> MySQL veritabanlarını kullanan uygulamaların, kesilen bağlantıları ve başarısız işlemleri algılayıp yeniden denedikleri bir şekilde oluşturulması gerekir.  Uygulama yeniden denense, bağlantılar yeni oluşturulan veritabanı sunucusuna yönlendirilir. | Veritabanı sunucusu hatası algılanırsa, bekleyen veritabanı sunucusu etkinleştirilir ve bu nedenle kapalı kalma süresini azaltır. Daha fazla ayrıntı için [ha kavramları sayfasına](concepts-high-availability.md) bakın. RTO 'ın RPO = 0 ile 60-120 s olması beklenir |
| **Depolama hatası** | Uygulamalar, disk arızası veya fiziksel blok bozulması gibi depolama ile ilgili herhangi bir sorun için herhangi bir etkisi görmez. Veriler 3 kopyada depolandığından, verilerin kopyası, çalışan depolama alanı tarafından sunulur. Blok bozulmaları otomatik olarak düzeltilir. Verilerin bir kopyası kaybolursa, verilerin yeni bir kopyası otomatik olarak oluşturulur. | Kurtarılabilir olmayan hatalar için, kapalı kalma süresini azaltmak için esnek sunucunun yük devri çoğaltmaya devredilmesini sağlar. Daha fazla ayrıntı için [ha kavramları sayfasına](./concepts-high-availability.md) bakın. |
| **Mantıksal/Kullanıcı hataları** | Yanlışlıkla bırakılan tablolar veya yanlış güncelleştirilmiş veriler gibi Kullanıcı hatalarından kurtarma, hata oluşmadan hemen önce geçen zamana kadar verileri geri yükleyerek ve kurtararak bir [zaman içinde kurtarma](concepts-backup-restore.md) (sür) gerçekleştirmeyi içerir.<br> <br>  Veritabanı sunucusundaki tüm veritabanları yerine veritabanlarının yalnızca bir alt kümesini veya belirli tabloları geri yüklemek isterseniz, veritabanı sunucusunu yeni bir örneğe geri yükleyebilir, [pg_dump](https://www.postgresql.org/docs/current/app-pgdump.html)aracılığıyla tabloları dışarı aktarabilir ve ardından bu tabloları veritabanınıza geri yüklemek için [pg_restore](https://www.postgresql.org/docs/current/app-pgrestore.html) kullanabilirsiniz. | Tüm kullanıcı işlemlerinin bekleme konumuna çoğaltılması nedeniyle bu kullanıcı hataları yüksek kullanılabilirlik ile korunmaz. |
| **Kullanılabilirlik bölgesi hatası** | Nadir bir olay olsa da, bölge düzeyindeki bir hatadan kurtarmak istiyorsanız, yedeklemeyi kullanarak bir noktadan noktaya kurtarma gerçekleştirebilir ve en son verilere ulaşmak için özel geri yükleme noktasını seçebilirsiniz. Yeni bir esnek sunucu, başka bir bölgeye dağıtılır. Geri yüklemek için geçen süre, önceki yedeklemeye ve kurtarılacak işlem günlüklerinin sayısına bağlıdır. | Esnek sunucu, bekleme sitesine otomatik yük devretme işlemi gerçekleştirir. Daha fazla ayrıntı için [ha kavramları sayfasına](./concepts-high-availability.md) bakın. |
| **Bölge hatası** | Çapraz-bölge çoğaltma ve coğrafi geri yükleme özellikleri henüz önizlemede desteklenmiyor. | |

> [!IMPORTANT]
> Silinen sunucular **geri yüklenemez.** Sunucuyu silerseniz, sunucuya ait olan tüm veritabanları da silinir ve kurtarılamaz. Sunucunuzun yanlışlıkla silinmesini önlemeye yardımcı olmak için [Azure Kaynak kilidini](../../azure-resource-manager/management/lock-resources.md) kullanın.

## <a name="next-steps"></a>Sonraki adımlar

- [Bölge yedekli yüksek kullanılabilirlik](./concepts-high-availability.md) hakkında bilgi edinin
- [Yedekleme ve kurtarma](./concepts-backup-restore.md) hakkında bilgi edinin
