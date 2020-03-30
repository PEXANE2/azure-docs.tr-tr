---
title: Bulut iş sürekliliği - veritabanı kurtarma
description: Azure SQL Veritabanı'nın bulutta iş sürekliliği ve veritabanı kurtarmanın yanı sıra görev açısından kritik bulut uygulamalarının kesintisiz çalışması için sunduğu destek özelliklerini öğrenin.
keywords: iş sürekliliği, bulutta iş sürekliliği, veritabanı olağanüstü durum kurtarma, veritabanı kurtarma
services: sql-database
ms.service: sql-database
ms.subservice: high-availability
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: mathoma, carlrab
ms.date: 06/25/2019
ms.openlocfilehash: 4f30bf112175742566c2957d78154e5a7abd1733
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79096871"
---
# <a name="overview-of-business-continuity-with-azure-sql-database"></a>Azure SQL Veritabanı'nda iş sürekliliğine genel bakış

Azure SQL Veritabanı'ndaki **iş sürekliliği,** işletmenizin özellikle bilgi işlem altyapısında kesinti karşısında çalışmaya devam etmesini sağlayan mekanizmalar, ilkeler ve yordamlar anlamına gelir. Çoğu durumda, Azure SQL Veritabanı bulut ortamında meydana gelebilecek yıkıcı olayları işler ve uygulamalarınızın ve iş süreçlerinizin çalışmasını sağlar. Ancak, SQL Veritabanı tarafından otomatik olarak işlenemeyen bazı yıkıcı olaylar vardır:

- Kullanıcı yanlışlıkla bir tablodaki bir satırı sildi veya güncelledi.
- Kötü amaçlı saldırgan verileri silmeyi veya veritabanını bırakmayı başardı.
- Deprem bir elektrik kesintisi ve geçici engelli veri merkezi neden oldu.

Bu genel bakış, Azure SQL Veritabanı'nın iş sürekliliği ve olağanüstü durum kurtarma özelliklerini açıklamaktadır. Veri kaybına veya veritabanınızın ve uygulamanızın kullanılamamasına neden olabilecek yıkıcı olaylardan kurtarmak için seçenekler, öneriler ve öğreticiler hakkında bilgi edinin. Bir kullanıcı veya uygulama hatası veri bütünlüğünü etkilediğinde, bir Azure bölgesinde kesinti olduğunda veya uygulamanız bakım gerektirdiğinde ne yapmanız gerektiğini öğrenin.

## <a name="sql-database-features-that-you-can-use-to-provide-business-continuity"></a>İş sürekliliği sağlamak için kullanabileceğiniz SQL Veritabanı özellikleri

Veritabanı açısından bakıldığında, dört ana olası bozulma senaryosu vardır:

- Disk sürücüsü hatası gibi veritabanı düğümlerini etkileyen yerel donanım veya yazılım hataları.
- Genellikle bir uygulama hatası veya insan hatasından kaynaklanan veri bozulması veya silme. Bu tür hatalar uygulamaya özgüdir ve genellikle veritabanı hizmeti tarafından algılanamaz.
- Veri merkezi kesintisi, muhtemelen doğal bir felaketten kaynaklanıyor. Bu senaryo, alternatif bir veri merkezine uygulama başarısız lığı ile coğrafi artıklık bazı düzeyde gerektirir.
- Yükseltme veya bakım hataları, planlanan altyapı bakımı veya yükseltmeleri sırasında ortaya çıkan beklenmeyen sorunlar, önceki bir veritabanı durumuna hızlı bir şekilde geri dönüş gerektirebilir.

Yerel donanım ve yazılım hatalarını azaltmak için SQL Veritabanı, %99,995'e varan kullanılabilirlik SLA ile bu hatalardan otomatik olarak kurtarmayı garanti eden [yüksek kullanılabilirlik mimarisi](sql-database-high-availability.md)içerir.  

İşletmenizi veri kaybına karşı korumak için, SQL Veritabanı otomatik olarak haftalık tam veritabanı yedeklemeleri, her 12 saatte bir diferansiyel veritabanı yedeklemeleri ve her 5 -10 dakikada bir işlem günlüğü yedeklemeleri oluşturur. Yedekler, tüm hizmet katmanları için en az 7 gün boyunca RA-GRS depolama alanında saklanır. Temel destek dışındaki tüm hizmet katmanları, 35 güne kadar zaman içinde geri yükleme için yapılandırılabilir yedekleme bekletme süresini destekler. 

SQL Veritabanı ayrıca, çeşitli planlanmamış senaryoları azaltmak için kullanabileceğiniz çeşitli iş sürekliliği özellikleri de sağlar. 

- [Zamana bağlı tablolar](sql-database-temporal-tables.md) satır sürümlerini herhangi bir noktadan geri yüklemenize olanak sağlar.
- [Yerleşik otomatik yedeklemeler](sql-database-automated-backups.md) ve [Point in Time Geri Yükleme,](sql-database-recovery-using-backups.md#point-in-time-restore) 35 güne kadar yapılandırılmış bekletme süresi içinde tam veritabanını belirli bir noktaya geri yüklemenize olanak tanır.
- **SQL Veritabanı sunucusu silinmemişse**silinen veritabanını silindiği noktaya [geri yükleyebilirsiniz.](sql-database-recovery-using-backups.md#deleted-database-restore)
- [Uzun süreli yedekleme bekletme,](sql-database-long-term-retention.md) yedeklemeleri 10 yıla kadar tutmanızı sağlar.
- [Etkin coğrafi çoğaltma,](sql-database-active-geo-replication.md) okunabilir yinelemeler oluşturmanıza ve veri merkezi kesintisi veya uygulama yükseltmesi durumunda herhangi bir yinelemede el ile başarısız duruma geçmenize olanak tanır.
- [Otomatik arıza grubu,](sql-database-auto-failover-group.md#auto-failover-group-terminology-and-capabilities) veri merkezi kesintisi durumunda uygulamanın otomatik olarak kurtarılmasını sağlar.

## <a name="recover-a-database-within-the-same-azure-region"></a>Aynı Azure bölgesinde bir veritabanı kurtarma

Veritabanını geçmişte bir noktaya geri yüklemek için otomatik veritabanı yedeklemelerini kullanabilirsiniz. Bu şekilde, insan hatalarının neden olduğu veri bozulmalarını kurtarabilirsiniz. Zamanında geri yükleme, aynı sunucuda, bozan olaydan önce verilerin durumunu temsil eden yeni bir veritabanı oluşturmanıza olanak tanır. Çoğu veritabanları için geri yükleme işlemleri 12 saatten az sürer. Çok büyük veya çok etkin bir veritabanını kurtarmak daha uzun sürebilir. Kurtarma süresi hakkında daha fazla bilgi için [veritabanı kurtarma süresine](sql-database-recovery-using-backups.md#recovery-time)bakın. 

Zaman içinde geri yükleme (PITR) için desteklenen maksimum yedekleme bekletme süresi uygulamanız için yeterli değilse, veritabanı(lar) için uzun vadeli bir bekletme (LTR) ilkesini yapılandırarak bunu genişletebilirsiniz. Daha fazla bilgi için [bkz.](sql-database-long-term-retention.md)

## <a name="compare-geo-replication-with-failover-groups"></a>Coğrafi çoğaltmayı failover gruplarıyla karşılaştırın

[Otomatik arıza grupları,](sql-database-auto-failover-group.md#auto-failover-group-terminology-and-capabilities) [coğrafi çoğaltma](sql-database-active-geo-replication.md) dağıtımı ve kullanımını basitleştirir ve aşağıdaki tabloda açıklandığı gibi ek özellikler ekler:

|                                              | Coğrafi çoğaltma | Yük devretme grupları  |
|:---------------------------------------------| :-------------- | :----------------|
| Otomatik yük devretme                           |     Hayır          |      Evet         |
| Aynı anda birden çok veritabanı üzerinden başarısız  |     Hayır          |      Evet         |
| Kullanıcı, arızadan sonra bağlantı dizelerini güncelleştirmelidir      |     Evet         |      Hayır          |
| Yönetilen örnek destekli                   |     Hayır          |      Evet         |
| Birincil olarak aynı bölgede olabilir             |     Evet         |      Hayır          |
| Birden çok yineleme                            |     Evet         |      Hayır          |
| Okuma ölçeğini destekler                          |     Evet         |      Evet         |
| &nbsp; | &nbsp; | &nbsp; |


## <a name="recover-a-database-to-the-existing-server"></a>Veritabanını varolan sunucuya kurtarma

Çok sık olmasa da Azure veri merkezlerinde kesintiler yaşanabilir. Kesinti yaşandığında yalnızca birkaç dakika sürebilecek veya saatler alacak bir hizmet kesintisi söz konusu olabilir.

- Seçeneklerden biri, veri merkezi kesintisi sona erdiğinde veritabanınızın çevrimdışı olmasını beklemektir. Bu, veritabanının çevrimdışı olmasının kabul edilebildiği uygulamalar için geçerlidir. Örnek olarak üzerinde sürekli çalışma yapmadığınız bir geliştirme projesi veya ücretsiz deneme sürümü verilebilir. Bir veri merkezinin kesintisi olduğunda, kesintinin ne kadar sürebileceğini bilemezsiniz, bu nedenle bu seçenek yalnızca veritabanınıza bir süre ihtiyacınız yoksa çalışır.
- Başka bir seçenek, [coğrafi yedekli veritabanı yedeklemelerini](sql-database-recovery-using-backups.md#geo-restore) (coğrafi geri yükleme) kullanarak herhangi bir Azure bölgesindeki herhangi bir sunucudaki veritabanını geri yüklemektir. Coğrafi geri yükleme, kaynağı olarak coğrafi yedekli bir yedekleme kullanır ve bir kesinti nedeniyle veritabanına veya veri merkezine erişilemiyor olsa bile veritabanını kurtarmak için kullanılabilir.
- Son olarak, [etkin coğrafi çoğaltma](sql-database-active-geo-replication.md) yı veya veritabanınız veya veritabanlarınız için otomatik hata [grubu](sql-database-auto-failover-group.md) kullanarak jeo-ikincil yapılandırmışsanız, kesintiden hızlı bir şekilde kurtulabilirsiniz. Bu teknolojileri seçtiğiniz bağlı olarak, manuel veya otomatik failover kullanabilirsiniz. Kendisi üzerinde failover sadece birkaç saniye sürer iken, hizmet etkinleştirmek için en az 1 saat sürer. Bu, kesintinin kesintinin ölçeğiyle haklı olduğundan emin olmak için gereklidir. Ayrıca, failover asynchronous çoğaltma doğası nedeniyle küçük veri kaybına neden olabilir. 

İş sürekliliği planınızı geliştirirken, uygulamanın kesintiden sonra tamamen kurtarılmasına kadar kabul edilebilen maksimum süreyi anlamanız gerekir. Uygulamanın tam olarak kurtarmak için gereken süre, Kurtarma süresi hedefi (RTO) olarak bilinir. Ayrıca, uygulamanın planlanmamış bir yıkıcı olaydan kurtarırken kaybetmeyi tolere edebileceği en son veri güncelleştirmelerinin (zaman aralığı) maksimum süresini de anlamanız gerekir. Olası veri kaybı Kurtarma noktası hedefi (RPO) olarak bilinir.

Farklı kurtarma yöntemleri farklı rpo ve RTO düzeyleri sunar. Belirli bir kurtarma yöntemi seçebilir veya tam uygulama kurtarma elde etmek için yöntemlerin bir birleşimini kullanabilirsiniz. Aşağıdaki tablo, her kurtarma seçeneğinin RPO ve RTO'yu karşılaştırır. Otomatik arıza grupları, coğrafi çoğaltmadağıtımı ve kullanımını basitleştirir ve aşağıdaki tabloda açıklandığı gibi ek özellikler ekler.

| Kurtarma yöntemi | Rto | RPO |
| --- | --- | --- | 
| Coğrafi olarak çoğaltılan yedeklemelerden coğrafi geri yükleme | 12 saat | 1 saat |
| Otomatik yük devretme grupları | 1 saat | 5 s |
| Manuel veritabanı failover | 30 s | 5 s |

> [!NOTE]
> *El ile veritabanı failover* [planlanmamış modu](sql-database-active-geo-replication.md#active-geo-replication-terminology-and-capabilities)kullanarak jeo-çoğaltılan ikincil tek bir veritabanının failover anlamına gelir.
Otomatik arıza üzerinden RTO ve RPO ayrıntıları için bu makalenin önceki tabloya bakın.


Uygulamanız şu ölçütlerden birini karşılıyorsa otomatik arıza grupları kullanın:

- Görev açısından kritikse.
- 12 saat veya daha fazla kapalı kalma süresine izin vermeyen bir hizmet düzeyi sözleşmesi (SLA) vardır.
- Kesinti süresi mali yükümlülüğe neden olabilir.
- Yüksek veri değişim hızına sahiptir ve 1 saatlik veri kaybı kabul edilemez.
- Etkin coğrafi çoğaltma ek maliyeti, olası mali yükümlülükten veya ilgili iş kaybından daha düşükse.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Database-protecting-important-DBs-from-regional-disasters-is-easy/player]
>

Uygulama gereksinimlerinize bağlı olarak veritabanı yedeklemeleri ve etkin coğrafi çoğaltma nın bir birleşimini kullanmayı seçebilirsiniz. Tek başına veritabanları ve bu iş sürekliliği özelliklerini kullanan esnek havuzlar için tasarım konularının tartışılması için bulut felaket kurtarma ve [Elastik havuz olağanüstü durum kurtarma stratejileri](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md)için bir uygulama [tasarlayın.](sql-database-designing-cloud-solutions-for-disaster-recovery.md)

Aşağıdaki bölümler, veritabanı yedeklemelerini veya etkin coğrafi çoğaltmayı kullanarak kurtarılabilmek için gereken adımlara genel bir bakış sağlar. Planlama gereksinimleri, kurtarma sonrası adımları ve olağanüstü durum kurtarma matkabını gerçekleştirmek için kesintinin nasıl simüle edileceklerine ilişkin bilgiler gibi ayrıntılı adımlar için [bkz.](sql-database-disaster-recovery.md)

### <a name="prepare-for-an-outage"></a>Kesinti için hazırlanma

Kullandığınız iş sürekliliği özelliğinden bağımsız olarak aşağıdaki adımları uygulamanız gerekir:

- Sunucu düzeyindeKI IP güvenlik duvarı kuralları, oturum açmalar ve ana veritabanı düzeyi izinleri de dahil olmak üzere hedef sunucuyu tanımlayın ve hazırlayın.
- İstemcilerin ve istemci uygulamalarının yeni sunucuya nasıl yönlendirileceğini belirlemek
- Denetim ayarları ve uyarılar gibi diğer bağımlılık belgelerini oluşturmak

Düzgün hazırlanmazsanız, uygulamalarınızı bir hata dan veya veritabanı kurtarma dan sonra çevrimiçi duruma getirmek ek zaman alır ve büyük olasılıkla stres anında sorun giderme gerektirir - kötü bir kombinasyon.

### <a name="fail-over-to-a-geo-replicated-secondary-database"></a>Coğrafi olarak çoğaltılan ikincil veritabanında başarısız olun

Kurtarma mekanizmasınız olarak etkin coğrafi çoğaltma veya otomatik arıza grupları kullanıyorsanız, otomatik bir hata ilkesini yapılandırabilir veya [el ile planlanmamış başarısız lık](sql-database-active-geo-replication-portal.md#initiate-a-failover)kullanabilirsiniz. Bir kez başlatılan, failover ikincil yeni birincil ve yeni hareketler kaydetmek ve sorguları yanıt vermeye hazır olmasına neden olur - henüz çoğaltılmış değil veri için en az veri kaybı ile. Başarısız lama işleminin tasarımı hakkında daha fazla bilgi için bulut [felaketkurtarma için bir uygulama tasarla'ya](sql-database-designing-cloud-solutions-for-disaster-recovery.md)bakın.

> [!NOTE]
> Veri merkezi yeniden çevrimiçi olduğunda, eski öncelikler otomatik olarak yeni birincile yeniden bağlanır ve ikincil veritabanları haline gelir. Birincil işlemi özgün bölgeye geri taşımanız gerekiyorsa, planlı bir başarısızlık işlemini el ile başlatabilirsiniz (geri dönüş).

### <a name="perform-a-geo-restore"></a>Coğrafi geri yükleme gerçekleştirme

Coğrafi yedekli depolama alanına sahip otomatik yedeklemeleri kullanıyorsanız (varsayılan olarak etkinleştirildi), [veritabanını coğrafi geri yükleme](sql-database-disaster-recovery.md#recover-using-geo-restore)yi kullanarak kurtarabilirsiniz. Kurtarma genellikle 12 saat içinde gerçekleşir - son günlük yedekleme sinin alınıp çoğaltılıncasına göre bir saate kadar veri kaybı gerçekleşir. Kurtarma işlemi tamamlanana kadar veritabanı işlem kaydedemez ve sorgulara yanıt veremez. Not, coğrafi geri yükleme veritabanını yalnızca kullanılabilir son noktaya geri yükler.

> [!NOTE]
> Uygulamanızı kurtarılan veritabanına geçmeden önce veri merkezi yeniden çevrimiçi olursa, kurtarmayı iptal edebilirsiniz.

### <a name="perform-post-failover--recovery-tasks"></a>Yük devretme/kurtarma sonrası görevleri gerçekleştirme

Bu iki kurtarma sisteminden herhangi biriyle gerçekleştirilen kurtarma işleminden sonra kullanıcılarınızın ve uygulamalarınızın çalışmaya devam etmesi için aşağıdaki ek görevleri gerçekleştirmeniz gerekir:

- İstemcilerin ve istemci uygulamalarının yeni sunucuya ve geri yüklenen veritabanına nasıl yönlendirileceğini belirleme
- Uygun kuralları etkinleştirmek için kullanıcıların veritabanı [düzeyinde güvenlik duvarlarını](sql-database-firewall-configure.md#use-the-azure-portal-to-manage-server-level-ip-firewall-rules) bağlamaları veya kullanmaları için uygun sunucu düzeyinde IP güvenlik duvarı kurallarının yerinde olduğundan emin olun.
- Uygun giriş bilgilerinin ve ana veritabanı düzeyi izinlerin mevcut olduğunu doğrulama ([kapsanan kullanıcıları](https://docs.microsoft.com/sql/relational-databases/security/contained-database-users-making-your-database-portable) da kullanabilirsiniz)
- Denetimi uygun şekilde yapılandırma
- Uyarıları uygun şekilde yapılandırma

> [!NOTE]
> Bir failover grubu kullanıyorsanız ve okuma yazma lstener kullanarak veritabanlarına bağlanmak, başarısız olduktan sonra yeniden yönlendirme otomatik ve saydam olarak uygulamaya gerçekleşecek.

## <a name="upgrade-an-application-with-minimal-downtime"></a>Bir uygulamayı en az kesinti süresiyle yükseltme

Bazen bir uygulama yükseltmesi gibi planlı bakım nedeniyle bir uygulama çevrimdışı alınmalıdır. [Uygulama yükseltmelerini yönet,](sql-database-manage-application-rolling-upgrade.md) yükseltmeler sırasında kapalı kalma süresini en aza indirmek ve bir şeyler ters giderse kurtarma yolu sağlamak için bulut uygulamanızın yuvarlanma yükseltmelerini etkinleştirmek için etkin coğrafi çoğaltmanın nasıl kullanılacağını açıklar.

## <a name="next-steps"></a>Sonraki adımlar

Tek başına veritabanları ve elastik havuzlar için uygulama tasarımı konularının tartışılması için bulut felaket kurtarma ve [Elastik havuz kurtarma stratejileri](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md) [için bir uygulama tasarlayın.](sql-database-designing-cloud-solutions-for-disaster-recovery.md)
