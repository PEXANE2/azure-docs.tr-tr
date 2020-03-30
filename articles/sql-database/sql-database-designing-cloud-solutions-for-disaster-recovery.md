---
title: Küresel olarak kullanılabilir hizmetleri tasarla
description: Azure SQL Veritabanı'nı kullanarak yüksek kullanılabilir hizmetler için uygulama tasarımı hakkında bilgi edinin.
keywords: bulut felaket kurtarma, olağanüstü durum kurtarma çözümleri, uygulama veri yedekleme, coğrafi çoğaltma, iş sürekliliği planlama
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: carlrab
ms.date: 12/04/2018
ms.openlocfilehash: 348bd2b92801217a5aea2ef4d1426c020085e4c1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79269074"
---
# <a name="designing-globally-available-services-using-azure-sql-database"></a>Azure SQL Veritabanı'nı kullanarak genel olarak kullanılabilir hizmetler tasarlama

Azure SQL Veritabanı ile bulut hizmetleri oluşturma ve dağıtma da, bölgesel kesintilere ve yıkıcı hatalara karşı esneklik sağlamak için [etkin coğrafi çoğaltma](sql-database-active-geo-replication.md) veya otomatik [hata](sql-database-auto-failover-group.md) grupları kullanırsınız. Aynı özellik, verilere yerel erişim için optimize edilmiş genel olarak dağıtılmış uygulamalar oluşturmanıza olanak tanır. Bu makalede, her seçeneğin yararları ve dengeleri de dahil olmak üzere ortak uygulama kalıpları tartışılır.

> [!NOTE]
> Premium veya İş Açısından Kritik veritabanları nı ve esnek havuzları kullanıyorsanız, bunları bölge gereksiz dağıtım yapılandırmasına dönüştürerek bunları bölgesel kesintilere karşı esnek hale getirebilirsiniz. Bkz. [Bölge yedekli veritabanları.](sql-database-high-availability.md)  

## <a name="scenario-1-using-two-azure-regions-for-business-continuity-with-minimal-downtime"></a>Senaryo 1: En az kapalı kalma süresiyle iş sürekliliği için iki Azure bölgesi kullanma

Bu senaryoda, uygulamalar aşağıdaki özelliklere sahiptir:

* Uygulama tek bir Azure bölgesinde etkin
* Tüm veritabanı oturumları verilere okuma ve yazma erişimi (RW) gerektirir
* Gecikme süresini ve trafik maliyetini azaltmak için Web katmanı ve veri katmanı harmanlanmalıdır
* Temelde, kesinti veri kaybı daha bu uygulamalar için daha yüksek bir iş riski

Bu durumda, tüm uygulama bileşenlerinin birlikte başarısız olması gerektiğinde, uygulama dağıtım topolojisi bölgesel felaketleri işlemek için en iyi duruma getirilmiştir. Aşağıdaki diyagram bu topolojiyi göstermektedir. Coğrafi artıklık için, uygulamanın kaynakları Bölge A ve B'ye dağıtılır. Ancak, Bölge A başarısız olana kadar B bölgesindeki kaynaklar kullanılmaz. Veritabanı bağlantısı, çoğaltma ve başarısızlığı yönetmek için iki bölge arasında bir başarısızlık grubu yapılandırılır. Her iki bölgede web hizmeti okuma-yazma dinleyici ** &lt;failover-group-name&gt;.database.windows.net** (1) üzerinden veritabanına erişmek için yapılandırılmıştır. Trafik yöneticisi [öncelik yönlendirme yöntemini](../traffic-manager/traffic-manager-configure-priority-routing-method.md) kullanmak üzere ayarlanmıştır (2).  

> [!NOTE]
> [Azure trafik yöneticisi](../traffic-manager/traffic-manager-overview.md) bu makale boyunca yalnızca illüstrasyon amacıyla kullanılır. Öncelikli yönlendirme yöntemini destekleyen herhangi bir yük dengeleme çözümlerini kullanabilirsiniz.

Aşağıdaki diyagram, bir kesintiden önce bu yapılandırmayı gösterir:

![Senaryo 1. Kesintiden önce yapılandırma.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/scenario1-a.png)

Birincil bölgede bir kesintiden sonra, SQL Veritabanı hizmeti birincil veritabanına erişilemediğini algılar ve otomatik hata ilkesinin parametrelerine bağlı olarak ikincil bölgeye başarısız lığı tetikler (1). Uygulamanız SLA bağlı olarak, kesinti nin algılanması ile failover kendisi arasındaki süreyi denetleyen bir yetkisiz kullanım süresi yapılandırabilirsiniz. Failover grubu veritabanının başarısızolmasını tetiklemeden önce trafik yöneticisinin bitiş noktası hatasını başlatması mümkündür. Bu durumda web uygulaması hemen veritabanına yeniden bağlanamaz. Ancak yeniden bağlantılar, veritabanı başarısız olur olmaz otomatik olarak başarılı olur. Başarısız olan bölge geri yüklendiğinde ve yeniden çevrimiçi olduğunda, eski birincil otomatik olarak yeni bir ikincil olarak yeniden bağlanır. Aşağıdaki diyagram, başarısız olduktan sonra yapılandırmayı göstermektedir.

> [!NOTE]
> Arıza dan sonra yapılan tüm işlemler yeniden bağlantı sırasında kaybolur. Hata tamamlandıktan sonra, B bölgesindeki uygulama kullanıcı isteklerini yeniden bağlayabilir ve yeniden başlatabilir. Hem web uygulaması hem de birincil veritabanı şu anda B bölgesinde dir ve birlikte konumlarını korumaktadır.

![Senaryo 1. Başarısız olduktan sonra yapılandırma](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/scenario1-b.png)

B bölgesinde bir kesinti olursa, birincil ve ikincil veritabanı arasındaki çoğaltma işlemi askıya alınır, ancak ikisi arasındaki bağlantı bozulmadan kalır (1). Trafik yönetilen Bölge B bağlantısı kırık olduğunu algılar ve bozulmuş olarak uç nokta web uygulaması 2 işaretler (2). Uygulamanın performansı bu durumda etkilenmez, ancak veritabanı maruz kalır ve bu nedenle a bölgesinin art arda başarısız olması durumunda veri kaybı riski daha yüksektir.

> [!NOTE]
> Olağanüstü durum kurtarma için, uygulama dağıtımı iki bölgeyle sınırlı yapılandırma öneririz. Bunun nedeni, Azure coğrafyalarının çoğunun yalnızca iki bölgesi olmasıdır. Bu yapılandırma, uygulamanızı her iki bölgenin eşzamanlı felaket hatasından korumaz. Böyle bir hata olasılığı düşük bir durumda, [coğrafi geri yükleme işlemini](sql-database-disaster-recovery.md#recover-using-geo-restore)kullanarak veritabanlarınızı üçüncü bir bölgedeki kurtarabilirsiniz.
>

 Kesinti azaltıldıktan sonra, ikincil veritabanı birincil veritabanıyla otomatik olarak yeniden eşitlenir. Eşitleme sırasında birincil performansın etkilenebilir. Belirli etki, başarısızlıktan sonra yeni birincil olarak elde edilen veri miktarına bağlıdır. Aşağıdaki diyagram, ikincil bölgede bir kesintisi göstermektedir:

![Senaryo 1. İkincil bölgede bir kesintiden sonra yapılandırma.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/scenario1-c.png)

Bu tasarım deseninin temel **avantajları** şunlardır:

* Aynı web uygulaması, bölgeye özgü yapılandırması olmayan her iki bölgeye de dağıtılır ve başarısız olmayı yönetmek için ek mantık gerektirmez.
* Uygulama performansı, web uygulaması ve veritabanı her zaman birlikte bulunduğundan, başarısızlından etkilenmez.

Ana **denge,** B bölgesindeki uygulama kaynaklarının çoğu zaman yeterince kullanılmamış olmasıdır.

## <a name="scenario-2-azure-regions-for-business-continuity-with-maximum-data-preservation"></a>Senaryo 2: Maksimum veri koruma ile iş sürekliliği için Azure bölgeleri

Bu seçenek en iyi aşağıdaki özelliklere sahip uygulamalar için uygundur:

* Herhangi bir veri kaybı yüksek iş riskidir. Veritabanı hatası yalnızca kesinti felaket bir hatadan kaynaklanıyorsa son çare olarak kullanılabilir.
* Uygulama, salt okunur ve okundu-yazma modlarını destekler ve belirli bir süre için "salt okunur modunda" çalışabilir.

Bu desende, okuma-yazma bağlantıları zaman-out hataları almaya başladığında uygulama salt okunur moduna geçer. Web Uygulaması her iki bölgeye de dağıtılır ve okuma-yazma dinleyici bitiş noktasına bir bağlantı ve yalnızca okundu dinleyici bitiş noktasına farklı bağlantı (1) içerir. Trafik yöneticisi profili [öncelikli yönlendirme](../traffic-manager/traffic-manager-configure-priority-routing-method.md)kullanmalıdır. Her bölgedeki uygulama bitiş noktası için [bitiş noktası izleme](../traffic-manager/traffic-manager-monitoring.md) etkinleştirilmelidir (2).

Aşağıdaki diyagram, bir kesintiden önce bu yapılandırmayı göstermektedir:

![Senaryo 2. Kesintiden önce yapılandırma.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/scenario2-a.png)

Trafik yöneticisi A bölgesinde bir bağlantı hatası algıladığında, kullanıcı trafiğini otomatik olarak B bölgesindeki uygulama örneğine değiştirir. Bu desenle, yetkisiz kullanım süresini veri kaybıyla birlikte örneğin 24 saat gibi yeterince yüksek bir değere ayarlamanız önemlidir. Bu süre içinde kesinti azaltılırsa veri kaybının önlenmesini sağlar. B bölgesindeki Web uygulaması etkinleştirildiğinde okuma yazma işlemleri başarısız olmaya başlar. Bu noktada, salt okunur moduna geçmelidir (1). Bu modda istekler otomatik olarak ikincil veritabanına yönlendirilir. Kesinti felaket bir başarısızlık neden ise, büyük olasılıkla yetkisiz kullanım süresi içinde azaltılamaz. Süresi dolduğunda, başarısız grup, başarısızı tetikler. Bundan sonra okuma-yazma dinleyicisi kullanılabilir hale gelir ve bağlantıbaşarısız olmayı durdurur (2). Aşağıdaki diyagram, kurtarma işleminin iki aşamasını göstermektedir.

> [!NOTE]
> Birincil bölgedeki kesinti yetkisiz kullanım süresi içinde azaltılırsa, trafik yöneticisi birincil bölgedeki bağlantının geri yüklemesini algılar ve kullanıcı trafiğini A bölgesindeki uygulama örneğine geri döndürür. Bu uygulama örneği, önceki diyagramda gösterildiği gibi A bölgesindeki birincil veritabanını kullanarak okuma yazma modunda devam eder ve çalışır.

![Senaryo 2. Olağanüstü durum kurtarma aşamaları.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/scenario2-b.png)

B bölgesinde bir kesinti olursa, trafik yöneticisi B bölgesindeki bitiş noktası web-app-2'nin başarısızlığını algılar ve bozulduğunu işaretler (1). Bu arada, failover grubu salt okunur dinleyiciyi A (2) bölgesine değiştirir. Bu kesinti son kullanıcı deneyimini etkilemez, ancak birincil veritabanı kesinti sırasında açığa çıkar. Aşağıdaki diyagram, ikincil bölgede bir hata gösterir:

![Senaryo 2. İkincil bölgede kesinti.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/scenario2-c.png)

Kesinti azaltıldıktan sonra, ikincil veritabanı hemen birincil ile senkronize edilir ve salt okunur dinleyici b bölgesindeki ikincil veritabanına geri değiştirilir. Birincil eşitleme performansı sırasında eşitlenmesi gereken veri miktarına bağlı olarak biraz etkilenebilir.

Bu tasarım deseni çeşitli **avantajları**vardır:

* Geçici kesintiler sırasında veri kaybını önler.
* Kapalı kalma süresi yalnızca trafik yöneticisinin yapılandırılabilir olan bağlantı hatasını ne kadar hızlı algıladığına bağlıdır.

Bunun **sakıncası,** uygulamanın salt okunur modda çalışabilmesidir.

## <a name="scenario-3-application-relocation-to-a-different-geography-without-data-loss-and-near-zero-downtime"></a>Senaryo 3: Veri kaybı ve sıfıra yakın kapalı kalma süresi olmadan farklı bir coğrafyaya uygulama taşınması

Bu senaryoda uygulama aşağıdaki özelliklere sahiptir:

* Son kullanıcılar uygulamaya farklı coğrafyalardan erişiciler
* Uygulama, en son güncelleştirmelerle tam eşitlemeye bağlı olmayan salt okunur iş yüklerini içerir
* Verilere yazma erişimi kullanıcıların çoğunluğu için aynı coğrafyada desteklenmelidir
* Okuma gecikmesi son kullanıcı deneyimi için çok önemlidir

Bu gereksinimleri karşılamak için, kullanıcı aygıtının, gözatma verileri, analitik vb. gibi salt okunur işlemler için aynı coğrafyada dağıtılan uygulamaya **her zaman** bağlandığını garanti etmeniz gerekir. Oysa OLTP işlemleri **çoğu zaman**aynı coğrafyada işlenir. Örneğin, gün içinde OLTP işlemleri aynı coğrafyada işlenir, ancak kapalı saatlerde farklı bir coğrafyada işlenebilir. Son kullanıcı etkinliği çoğunlukla çalışma saatleri içinde gerçekleşirse, çoğu zaman kullanıcıların çoğu için en iyi performansı garanti edebilirsiniz. Aşağıdaki diyagram bu topolojiyi göstermektedir.

Uygulamanın kaynakları, önemli kullanım talebinizin olduğu her coğrafyada dağıtılmalıdır. Örneğin, uygulamanız Amerika Birleşik Devletleri, Avrupa Birliği ve Güney Doğu Asya'da etkin olarak kullanılıyorsa, uygulama nın tüm bu coğrafyalara dağıtılması gerekir. Birincil veritabanı, çalışma saatlerinin sonunda dinamik olarak bir coğrafyadan diğerine geçilmelidir. Bu yöntem "güneşi takip" olarak adlandırılır. OLTP iş yükü her zaman okuma-yazma ** &lt;&gt;dinleyicifailover-group-name .database.windows.net** (1) üzerinden veritabanına bağlanır. Salt okunur iş yükü, sunucu bitiş noktası ** &lt;sunucu adı&gt;.database.windows.net** (2) veritabanını kullanarak doğrudan yerel veritabanına bağlanır. Trafik yöneticisi [performans yönlendirme yöntemi](../traffic-manager/traffic-manager-configure-performance-routing-method.md)ile yapılandırılır. Son kullanıcının aygıtının en yakın bölgedeki web hizmetine bağlanmasını sağlar. Trafik yöneticisi, her web hizmeti bitiş noktası için etkin leştirilmiş son nokta izleme ile ayarlanmalıdır (3).

> [!NOTE]
> Failover grubu yapılandırması, failover için hangi bölgenin kullanıldığını tanımlar. Yeni birincil farklı bir coğrafyada olduğundan, etkilenen bölge yeniden çevrimiçi olana kadar hem OLTP hem de salt okunur iş yükleri için daha uzun gecikme sonu ile sonuçlanır.

![Senaryo 3. Doğu ABD'de birincil ile yapılandırma.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/scenario3-a.png)

Günün sonunda, örneğin yerel saatle 23:00'te, etkin veritabanları bir sonraki bölgeye (Kuzey Avrupa) geçilmelidir. Bu görev Azure Logic [Apps](../logic-apps/logic-apps-overview.md)kullanılarak tam otomatikolabilir. Görev aşağıdaki adımları içerir:

* Failover grubundaki birincil sunucuyu dost failover kullanarak Kuzey Avrupa'ya geçirin (1)
* Doğu ABD ve Kuzey Avrupa arasındaki başarısız grubu kaldırın
* Kuzey Avrupa ve Doğu Asya arasında aynı adı taşıyan yeni bir başarısız grup oluşturun (2).
* Kuzey Avrupa'daki birincil ve Doğu Asya'daki ikincil primary'i bu başarısız gruba ekleyin (3).

Aşağıdaki diyagram, planlanan hatadan sonra yeni yapılandırmayı gösterir:

![Senaryo 3. Ön seçimin Kuzey Avrupa'ya geçişi.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/scenario3-b.png)

Örneğin Kuzey Avrupa'da bir kesinti gerçekleşirse, otomatik veritabanı başarısızlığı failover grubu tarafından başlatılır ve bu da uygulamanın zamanlamadan önce bir sonraki bölgeye taşınmasıyla sonuçlanır (1).  Bu durumda, Kuzey Avrupa tekrar devreye dönene kadar ABD Doğusu kalan tek ikincil bölgedir. Geri kalan iki bölge, rollerdeğiştirerek üç coğrafyadaki müşterilere hizmet vermektedir. Azure Logic Apps buna göre ayarlanmalıdır. Kalan bölgeler Avrupa'dan ek kullanıcı trafiği aldığı için, uygulamanın performansı yalnızca ek gecikme süresinden değil, aynı zamanda artan son kullanıcı bağlantısı sayısından da etkilenir. Kesinti Kuzey Avrupa'da azaltıldıktan sonra, oradaki ikincil veritabanı hemen geçerli birincil veritabanıyla senkronize edilir. Aşağıdaki diyagram, Kuzey Avrupa'daki bir kesintiyi göstermektedir:

![Senaryo 3. Kuzey Avrupa'da kesinti.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/scenario3-c.png)

> [!NOTE]
> Son kullanıcının Avrupa'daki deneyiminin uzun gecikme süresiyle bozulduğu zamanı azaltabilirsiniz. Bunu yapmak için, kuzey Avrupa'daki çevrimdışı uygulama örneğinin yerine, bir uygulama kopyasını proaktif olarak dağıtmalı ve başka bir yerel bölgede (Batı Avrupa) ikincil veritabanı(lar) oluşturmalısınız. İkincisi tekrar çevrimiçi olduğunda, Batı Avrupa'yı kullanmaya devam edip etmemeye veya uygulamanın kopyasını kaldırmaya ve Kuzey Avrupa'yı kullanmaya geri dönüp dönmeyeceğinizkonusunda karar verebilirsiniz.

Bu tasarımın temel **yararları** şunlardır:

* Salt okunur uygulama iş yükü, dolaplar bölgesindeki verilere her zaman erişir.
* Okuma yazma uygulaması iş yükü, her coğrafyada en yüksek etkinlik döneminde en yakın bölgedeki verilere erişir
* Uygulama birden çok bölgeye dağıtıldığı için, önemli bir kesinti olmaksızın bölgelerden birinin kaybına neden olabilir.

Ama bazı **tradeoffs**vardır:

* Bölgesel bir kesinti, coğrafyanın daha uzun gecikme süreden etkilenmesine neden olur. Uygulama tarafından farklı bir coğrafyada hem okuma-yazma hem de salt okunur iş yükleri sunulur.
* Salt okunur iş yükleri, her bölgedeki farklı bir bitiş noktasına bağlanmalıdır.

## <a name="business-continuity-planning-choose-an-application-design-for-cloud-disaster-recovery"></a>İş sürekliliği planlaması: Bulut felaketini kurtarma için bir uygulama tasarımı seçin

Özel bulut olağanüstü durum kurtarma stratejiniz, uygulamanızın gereksinimlerini en iyi şekilde karşılamak için bu tasarım modellerini birleştirebilir veya genişletebilir.  Daha önce de belirtildiği gibi, seçtiğiniz strateji, müşterilerinize sunmak istediğiniz SLA'ya ve uygulama dağıtım topolojisine dayanır. Kararınızı yönlendirmeye yardımcı olmak için, aşağıdaki tablo kurtarma noktası hedefine (RPO) ve tahmini kurtarma süresine (ERT) dayalı seçenekleri karşılaştırır.

| Desen | RPO | ERT |
|:--- |:--- |:--- |
| Ortak bulunan veritabanı erişimi yle olağanüstü durum kurtarma için etkin-pasif dağıtım |5 sn < okuma-yazma erişimi |Arıza algılama süresi + DNS TTL |
| Uygulama yük dengelemesi için etkin dağıtım |5 sn < okuma-yazma erişimi |Arıza algılama süresi + DNS TTL |
| Veri koruma için etkin-pasif dağıtım |5 sn < salt okunur erişim | Salt okunur erişim = 0 |
||Okuma-yazma erişimi = sıfır | Okuma-yazma erişimi = Hata algılama süresi + veri kaybı ile yetkisiz kullanım süresi |
|||

## <a name="next-steps"></a>Sonraki adımlar

* İş sürekliliğine genel bakış ve senaryolar için [bkz.](sql-database-business-continuity.md)
* Etkin coğrafi çoğaltma hakkında bilgi edinmek için [Etkin coğrafi çoğaltma'ya](sql-database-active-geo-replication.md)bakın.
* Otomatik hata grupları hakkında bilgi edinmek için Otomatik [başarısız grupları'na](sql-database-auto-failover-group.md)bakın.
* Elastik havuzlu etkin coğrafi çoğaltma hakkında bilgi için [elastik havuz felaket kurtarma stratejilerine](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md)bakın.
