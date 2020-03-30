---
title: Yük devretme grupları
description: Otomatik failover grupları, bir SQL Veritabanı sunucusunda veya yönetilen örnekteki tüm veritabanlarında bir grup veritabanının çoğaltılmasını ve otomatik /eşgüdümlü failover'ı yönetmenize olanak tanıyan bir SQL Veritabanı özelliğidir.
services: sql-database
ms.service: sql-database
ms.subservice: high-availability
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: mathoma, carlrab
ms.date: 2/10/2020
ms.openlocfilehash: 6d87d3373711d12df3f2cced26ef35ae951ad41e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79269841"
---
# <a name="use-auto-failover-groups-to-enable-transparent-and-coordinated-failover-of-multiple-databases"></a>Birden çok veritabanının saydam ve eşgüdümlü olarak başarısız olmasını sağlamak için otomatik hata grupları kullanma

Otomatik hata grupları, bir SQL Veritabanı sunucusunda veya yönetilen bir örnekteki tüm veritabanlarındaki bir veritabanı grubunun çoğaltılmasını ve başarısız olmasını başka bir bölgeye yönetmenize olanak tanıyan bir SQL Veritabanı özelliğidir. Bu, coğrafi olarak çoğaltılan veritabanlarının ölçekte dağıtımını ve yönetimini kolaylaştırmak için tasarlanmış varolan [etkin coğrafi çoğaltma](sql-database-active-geo-replication.md) özelliğinin üzerine bildirimsel bir soyutlamadır. Failover'ı el ile başlatabilir veya kullanıcı tanımlı bir ilkeyi temel alarak SQL Veritabanı hizmetine devredebilirsiniz. İkinci seçenek, sql veritabanı hizmetinin birincil bölgedeki kullanılabilirliğinin tam veya kısmi olarak kaybolmasına neden olan bir felaket hatası veya diğer planlanmamış olaydan sonra ikincil bir bölgedeki birden çok ilişkili veritabanını otomatik olarak kurtarmanızı sağlar. Bir failover grubu, genellikle aynı uygulama tarafından kullanılan bir veya birden çok veritabanları içerebilir. Ayrıca, salt okunur sorgu iş yüklerini boşaltmak için okunabilir ikincil veritabanlarını kullanabilirsiniz. Otomatik hata grupları birden çok veritabanı içerdiğinden, bu veritabanlarıbirincil sunucuda yapılandırılmalıdır. Otomatik hata grupları, gruptaki tüm veritabanlarının farklı bir bölgede yalnızca bir ikincil sunucuya çoğaltılmasını destekler.

> [!NOTE]
> Bir SQL Veritabanı sunucusunda tek veya havuzlu veritabanları ile çalışırken ve aynı veya farklı bölgelerde birden çok ikincilik istiyorsanız, [etkin coğrafi çoğaltma](sql-database-active-geo-replication.md)kullanın. 

Otomatik hata ilkesine sahip otomatik hata grupları kullandığınızda, gruptaki veritabanlarından birini veya birkaçını etkileyen herhangi bir kesinti otomatik hatayla sonuçlanır. Bunlar genellikle yerleşik otomatik yüksek kullanılabilirlik işlemleri tarafından kendi kendini azaltamayan olaylardır. Başarısız tetikleyicilere örnek olarak, birkaç işlem düğümündeki işletim sistemi çekirdeği bellek sızıntısı nedeniyle bir SQL kiracı halkasının veya kontrol halkasının kapatılmasından kaynaklanan bir olay veya yanlış bir ağ kablosu kesildiği için bir veya daha fazla kiracı halkasının kapatılmasından kaynaklanan bir olay verilebilir. rutin donanım devre dışı bırakma.  Daha fazla bilgi için [SQL Veritabanı Yüksek Kullanılabilirlik'e](sql-database-high-availability.md)bakın.

Buna ek olarak, otomatik başarısız gruplar, başarısızlıklar sırasında değişmeden kalan okuma-yazma ve yalnızca okuma dinleyicisi bitiş noktaları sağlar. İster manuel ister otomatik failover etkinleştirme kullanın, failover gruptaki tüm ikincil veritabanlarını birincile geçer. Veritabanı başarısız mıtamamlandıktan sonra, dns kaydı uç noktaları yeni bölgeye yönlendirmek için otomatik olarak güncelleştirilir. Belirli RPO ve RTO verileri için İş [Sürekliliğine Genel Bakış'a](sql-database-business-continuity.md)bakın.

Otomatik hata ilkesine sahip otomatik hata grupları kullandığınızda, SQL Veritabanı sunucusundaki veritabanlarını etkileyen veya yönetilen örnek sonuçları otomatik olarak başarısız olur. Otomatik arıza grubunu şu kullanarak yönetebilirsiniz:

- [Azure portalında](sql-database-implement-geo-distributed-database.md)
- [Azure CLI: Failover Grubu](scripts/sql-database-add-single-db-to-failover-group-cli.md)
- [PowerShell: Failover Grubu](scripts/sql-database-add-single-db-to-failover-group-powershell.md)
- [REST API: Failover grubu](/rest/api/sql/failovergroups).

Başarısız olduktan sonra, sunucunuzun ve veritabanınızın kimlik doğrulama gereksinimlerinin yeni birincil üzerinde yapılandırıldığından emin olun. Ayrıntılar [için, olağanüstü durum kurtarma sonrası SQL Veritabanı güvenliğine](sql-database-geo-replication-security-config.md)bakın.

Gerçek iş sürekliliği elde etmek için, veri merkezleri arasında veritabanı artıklığı eklemek çözümün yalnızca bir parçasıdır. Bir uygulamanın (hizmetin) felaket bir hatadan sonra uçtan uca kurtarılması, hizmeti oluşturan tüm bileşenlerin ve bağımlı hizmetlerin kurtarılmasını gerektirir. Bu bileşenlere örnek olarak istemci yazılımı (örneğin, özel JavaScript'li bir tarayıcı), web ön uçları, depolama ve DNS verilebilir. Tüm bileşenlerin aynı hatalara karşı dirençli olması ve uygulamanızın kurtarma süresi hedefi (RTO) içinde kullanılabilir hale gelmesi çok önemlidir. Bu nedenle, tüm bağımlı hizmetleri belirlemeniz ve sağladıkları garantileri ve yetenekleri anlamanız gerekir. Daha sonra, hizmetinizin bağlı olduğu hizmetlerin başarısız lığı sırasında çalışmasını sağlamak için yeterli adımları atmalısınız. Olağanüstü durum kurtarma çözümleri tasarımı hakkında daha fazla bilgi için bkz: [Etkin coğrafi çoğaltmayı kullanarak Olağanüstü Durum Kurtarma için Bulut Çözümleri Tasarlama.](sql-database-designing-cloud-solutions-for-disaster-recovery.md)

## <a name="auto-failover-group-terminology-and-capabilities"></a>Otomatik başarısız grup terminolojisi ve yetenekleri

- **Failover grubu (FOG)**

  Başarısız olma grubu, tek bir SQL Veritabanı sunucusu tarafından yönetilen veya birincil bölgedeki bir kesinti nedeniyle tüm veya bazı birincil veritabanlarının kullanılamaması durumunda birim olarak başka bir bölgeye birbirim olarak başarısız olabilen tek bir yönetilen örnek içinde yönetilen adlandırılmış veritabanları grubudur. Yönetilen örnekler için oluşturulduğunda, bir failover grubu örnekteki tüm kullanıcı veritabanlarını içerir ve bu nedenle bir örnekte yalnızca bir başarısız grup yapılandırılabilir.
  
  > [!IMPORTANT]
  > Failover grubunun adı `.database.windows.net` etki alanı içinde genel olarak benzersiz olmalıdır.

- **SQL Veritabanı sunucuları**

     SQL Veritabanı sunucularında, tek bir SQL Veritabanı sunucusundaki kullanıcı veritabanlarının bir kısmı veya tümü bir başarısız lık grubuna yerleştirilebilir. Ayrıca, bir SQL Veritabanı sunucusu tek bir SQL Veritabanı sunucusunda birden çok başarısız grupları destekler.

- **Birincil**

  Başarısız gruptaki birincil veritabanlarını barındıran SQL Veritabanı sunucusu veya yönetilen örnek.

- **İkincil**

  Başarısız gruptaki ikincil veritabanlarını barındıran SQL Veritabanı sunucusu veya yönetilen örnek. İkincil birincil olarak aynı bölgede olamaz.

- **Failover grubuna tek veritabanları ekleme**

  Aynı SQL Veritabanı sunucusunda birden fazla tek veritabanını aynı başarısız gruba koyabilirsiniz. Başarısız gruba tek bir veritabanı eklerseniz, ikincil sunucuda aynı sürüm ve bilgi işlem boyutunu kullanarak otomatik olarak ikincil bir veritabanı oluşturur.  Başarısız grup oluşturulduğunda bu sunucuyu belirttiniz. İkincil sunucuda zaten ikincil bir veritabanı olan bir veritabanı eklerseniz, bu coğrafi çoğaltma bağlantısı grup tarafından devralınır. Başarısız lık grubunun bir parçası olmayan bir sunucuda zaten ikincil veritabanı olan bir veritabanı eklediğinizde, ikincil sunucuda yeni bir ikincil veritabanı oluşturulur.

  > [!IMPORTANT]
  > Varolan bir ikincil veritabanı olmadığı sürece ikincil sunucunun aynı ada sahip bir veritabanı olmadığından emin olun. Yönetilen örnek için failover gruplarında tüm kullanıcı veritabanları çoğaltılır. Failover grubunda çoğaltma için kullanıcı veritabanlarının bir alt kümesini seçemezsiniz.

- **Başarısız gruba elastik havuzda veritabanları ekleme**

  Elastik bir havuzdaki tüm veya birkaç veritabanını aynı başarısız gruba koyabilirsiniz. Birincil veritabanı elastik bir havuzdaysa, ikincil otomatik olarak aynı ada (ikincil havuz) elastik havuzda oluşturulur. İkincil sunucunun aynı tam ada ve başarısız grup tarafından oluşturulacak ikincil veritabanlarını barındırmak için yeterli serbest kapasiteye sahip elastik bir havuz içerdiğinden emin olmalısınız. İkincil havuzda zaten ikincil bir veritabanı olan havuza bir veritabanı eklerseniz, bu coğrafi çoğaltma bağlantısı grup tarafından devralır. Başarısız lık grubunun bir parçası olmayan bir sunucuda zaten ikincil veritabanı olan bir veritabanı eklediğinizde, ikincil havuzda yeni bir ikincil veritabanı oluşturulur.
  
- **İlk Tohumlama** 

  Bir başarısız gruba veritabanları, elastik havuzlar veya yönetilen örnekler eklerken, veri çoğaltma başlamadan önce bir başlangıç tohumlama aşaması vardır. İlk tohumlama aşaması en uzun ve en pahalı işlemdir. İlk tohumlama tamamlandıktan sonra veriler eşitlenir ve ardından yalnızca sonraki veri değişiklikleri çoğaltılır. İlk tohumun tamamlanması için gereken süre, verilerinizin boyutuna, çoğaltılan veritabanlarının sayısına ve başarısız gruptaki varlıklar arasındaki bağlantının hızına bağlıdır. Normal şartlar altında, tipik tohumlama hızı 50-500 GB tek bir veritabanı veya elastik havuz için bir saat ve 18-35 GB yönetilen bir örnek için bir saat. Tohumlama paralel olarak tüm veritabanları için gerçekleştirilir. Veri çoğaltma başlamadan önce ilk tohumlama aşamasının ne kadar süreceğini tahmin etmek için veritabanlarının sayısı ve toplam veri boyutuyla birlikte belirtilen tohumlama hızını kullanabilirsiniz.

  Yönetilen örneklerde, iki örnek arasındaki Ekspres Rota bağlantısının hızının, ilk tohumlama aşamasının süresini tahmin ederken de göz önünde bulundurulması gerekir. İki örnek arasındaki bağlantının hızı gerekenden daha yavaşsa, tohum için zaman büyük olasılıkla etkilenebilir. Belirtilen tohumlama hızını, veritabanlarının sayısını, toplam veri boyutunu ve bağlantı hızını kullanarak ilk tohumlama aşamasının veri çoğaltma başlamadan önce ne kadar süreceğini tahmin edebilirsiniz. Örneğin, tek bir 100 GB veritabanı için, bağlantı saatte 35 GB itme yeteneğine sahipse, ilk tohum aşaması 2,8 - 5,5 saat arasında herhangi bir yerde sürer. Bağlantı saatte yalnızca 10 GB aktarabiliyorsa, 100 GB'lık bir veritabanını tohumlama yaklaşık 10 saat sürer. Çoğaltmak için birden fazla veritabanları varsa, tohumlama paralel olarak yürütülür ve yavaş bir bağlantı hızı ile birleştirildiğinde, ilk tohumlama aşaması çok daha uzun sürebilir, özellikle tüm veritabanlarından veri paralel tohumlama kullanılabilir aşıyorsa bağlantı bant genişliği. İki örnek arasındaki ağ bant genişliği sınırlıysa ve bir başarısız lık grubuna birden çok yönetilen örnek ekliyorsanız, başarısız gruba tek tek birden birden çok yönetilen örnek eklemeyi düşünün.

  
- **DNS bölgesi**

  Yeni bir örnek oluşturulduğunda otomatik olarak oluşturulan benzersiz bir kimlik. Bu örnek için çok etki alanı (SAN) sertifikası, istemci bağlantılarını aynı DNS bölgesindeki herhangi bir örneğin kimliğini doğrulamak için verilir. Aynı başarısız grupta yönetilen iki örnek DNS bölgesini paylaşmalıdır.
  
  > [!NOTE]
  > SQL Veritabanı sunucuları için oluşturulan başarısız gruplar için DNS bölge kimliği gerekmez.

- **Failover grup okuma-yazma dinleyicisi**

  Geçerli birincil URL'ye işaret eden bir DNS CNAME kaydı. Başarısız grup oluşturulduğunda otomatik olarak oluşturulur ve okuma yazma SQL iş yükünün, başarısız olduktan sonra birincil veritabanıdeğiştiğinde birincil veritabanına saydam olarak yeniden bağlanmasına olanak tanır. Bir SQL Veritabanı sunucusunda başarısız grup oluşturulduğunda, dinleyici URL'si için DNS CNAME kaydı `<fog-name>.database.windows.net`. Başarısız grup yönetilen bir örnekte oluşturulduğunda, dinleyici URL'si için DNS `<fog-name>.zone_id.database.windows.net`CNAME kaydı .

- **Failover grubu salt okunur dinleyici**

  Bir DNS CNAME kaydı, ikincil listenin URL'sini işaret eden salt okunur dinleyiciye işaret eden bir kayıt oluşturdu. Başarısız grup oluşturulduğunda otomatik olarak oluşturulur ve yalnızca okunan SQL IŞ yükünün belirtilen yük dengeleme kurallarını kullanarak ikincil yüke saydam olarak bağlanmasına izin verir. Bir SQL Veritabanı sunucusunda başarısız grup oluşturulduğunda, dinleyici URL'si için DNS CNAME kaydı `<fog-name>.secondary.database.windows.net`. Başarısız grup yönetilen bir örnekte oluşturulduğunda, dinleyici URL'si için DNS `<fog-name>.zone_id.secondary.database.windows.net`CNAME kaydı .

- **Otomatik arıza ilkesi**

  Varsayılan olarak, bir başarısız grup otomatik bir hata ilkesi yle yapılandırılır. SQL Veritabanı hizmeti, hata algılandıktan ve yetkisiz kullanım süresi dolduktan sonra başarısız lığa neden oluyor. Sistem, kesintinin etkisinin ölçeği nedeniyle SQL Veritabanı hizmetinin yerleşik [yüksek kullanılabilirlik altyapısı](sql-database-high-availability.md) tarafından azaltılamayacağını doğrulamalıdır. Uygulamadan gelen başarısız iş akışını denetlemek istiyorsanız, otomatik hata yı kapatabilirsiniz.
  
  > [!NOTE]
  > Kesintinin ölçeğinin doğrulanması ve ne kadar hızlı azaltılabilir, operasyon ekibinin insan eylemlerini içerdiğinden, yetkisiz kullanım süresi bir saatin altına ayarlanamaz.  Bu sınırlama, veri eşitleme durumlarına bakılmaksızın failover grubundaki tüm veritabanları için geçerlidir. 

- **Salt okunur başarısızlık ilkesi**

  Varsayılan olarak, salt okunur dinleyicinin başarısız lığı devre dışı bırakılır. Birincil çevrimdışı olduğunda birincil performansın etkilenmemesini sağlar. Ancak, aynı zamanda salt okunur oturumları ikincil kurtarılıncaya kadar bağlanmak mümkün olmayacaktır anlamına gelir. Salt okunur oturumlarıiçin kapalı kalma süresini tolere edemiyorsanız ve birincil inolasın olası performans bozulması pahasına hem salt okunur hem de okuma yazma trafiği için birincil özelliği geçici olarak `AllowReadOnlyFailoverToPrimary` kullanabilirseniz, yalnızca okunur dinleyiciiçin yalnızca okunur dinleyicinin başarısız olmasını sağlayabilirsiniz. Bu durumda, salt okunur trafik, ikincil kullanılabilir değilse otomatik olarak birincil e doğru yönlendirilir.

- **Planlı yük devretme**

   Planlı failover birincil role ikincil geçmeden önce birincil ve ikincil veritabanları arasında tam eşitleme gerçekleştirir. Bu, veri kaybını garanti etmez. Planlı başarısız olmak aşağıdaki senaryolarda kullanılır:

  - Veri kaybı kabul edilemez olduğunda üretimde olağanüstü durum kurtarma (DR) matkapları gerçekleştirin
  - Veritabanlarını farklı bir bölgeye taşıma
  - Kesinti azaltıldıktan (failback) sonra veritabanlarını birincil bölgeye döndürün.

- **Planlanmamış yük devretme**

   Planlanmamış veya zorlanmış başarısız, birincil rol ile herhangi bir senkronizasyon olmadan hemen birincil role ikincil anahtarları. Bu işlem veri kaybına neden olur. Ön ödemeli kesintiler sırasında planlanmamış başarısızlık, birincil erişime erişilemediğinde kurtarma yöntemi olarak kullanılır. Özgün birincil çevrimiçi olduğunda, senkronizasyon olmadan otomatik olarak yeniden bağlanır ve yeni bir ikincil olur.

- **Manuel arıza**

  Otomatik arıza yapılandırması ne olursa olsun, istediğiniz zaman el ile başarısız başlatabilirsiniz. Otomatik hata ilkesi yapılandırılmamışsa, başarısız gruptaki veritabanlarını ikincil gruba kurtarmak için el ile başarısız olmak gerekir. Zorunlu veya dostça bir hata başlatabilirsiniz (tam veri eşitlemesi ile). İkincisi birincil ikincil bölgeye taşınması için kullanılabilir. Başarısız olmak tamamlandığında, DNS kayıtları otomatik olarak güncelleştirildiğinde yeni birincil

- **Veri kaybı ile yetkisiz kullanım süresi**

  Birincil ve ikincil veritabanları eşzamanlı çoğaltma kullanılarak eşitlendirildiği için, hata veri kaybına neden olabilir. Uygulamanızın veri kaybına karşı toleransını yansıtacak şekilde otomatik hata ilkesini özelleştirebilirsiniz. Yapılandırarak, `GracePeriodWithDataLossHours`veri kaybına neden olma olasılığı yüksek olan failover'ı başlatmadan önce sistemin ne kadar beklemediğini denetleyebilirsiniz.

- **Birden çok başarısız grup**

  Başarısızların ölçeğini denetlemek için aynı sunucu çifti için birden çok başarısız grup yapılandırabilirsiniz. Her grup bağımsız olarak başarısız olur. Çok kiracılı uygulamanız elastik havuzlar kullanıyorsa, bu özelliği her havuzda birincil ve ikincil veritabanlarını karıştırmak için kullanabilirsiniz. Bu şekilde, bir kesintinin etkisini kiracıların yalnızca yarısına indirebilirsiniz.

  > [!NOTE]
  > Yönetilen Örnek, birden çok başarısız grupları desteklemez.
  
## <a name="permissions"></a>İzinler

Bir başarısız grup için izinler [rol tabanlı erişim denetimi (RBAC)](../role-based-access-control/overview.md)üzerinden yönetilir. [SQL Server Katılımcısı](../role-based-access-control/built-in-roles.md#sql-server-contributor) rolü, başarısız grupları yönetmek için gerekli tüm izinlere sahiptir.

### <a name="create-failover-group"></a>Başarısız grup oluşturma

Bir başarısız grup oluşturmak için, RBAC'ın hem birincil hem de ikincil sunuculara ve başarısız gruptaki tüm veritabanlarına erişim yazması gerekir. Yönetilen bir örnek için, rbac hem birincil hem de ikincil yönetilen örnek için erişim yazmak gerekir, ancak tek tek yönetilen örnek veritabanları eklenemez veya bir failover grubundan kaldırılamaz beri tek tek veritabanları üzerinde izinleri ilgili değildir. 

### <a name="update-a-failover-group"></a>Bir başarısız grubu güncelleştirme

Bir başarısız grubu güncelleştirmek için, başarısız gruba ve geçerli birincil sunucu veya yönetilen örnekteki tüm veritabanlarına RBAC yazma erişimi gerekir.  

### <a name="failover-a-failover-group"></a>Failover bir failover grubu

Bir başarısız grup üzerinde başarısız olmak için, yeni birincil sunucu veya yönetilen örnek üzerinde failover grubuna RBAC yazma erişimi gerekir.

## <a name="best-practices-of-using-failover-groups-with-single-databases-and-elastic-pools"></a>Tek veritabanları ve elastik havuzları ile failover grupları kullanarak en iyi uygulamalar

Otomatik hata grubu birincil SQL Veritabanı sunucusunda yapılandırılmalıdır ve onu farklı bir Azure bölgesindeki ikincil SQL Veritabanı sunucusuna bağlar. Gruplar bu sunucularda tüm veya bazı veritabanları içerebilir. Aşağıdaki diyagram, birden çok veritabanı ve otomatik hata grubu kullanarak coğrafi yedekli bulut uygulamasının tipik bir yapılandırmasını göstermektedir.

![otomatik failover](./media/sql-database-auto-failover-group/auto-failover-group.png)

> [!NOTE]
> Bkz. Bir başarısız gruba tek bir veritabanı eklemek için ayrıntılı bir adım adım öğretici için bir [failover grubuna](sql-database-single-database-failover-group-tutorial.md) tek bir veritabanı ekleyin.

İş sürekliliği göz önünde bulundurularak bir hizmet tasarlarken aşağıdaki genel yönergelere uyun:

### <a name="using-one-or-several-failover-groups-to-manage-failover-of-multiple-databases"></a>Birden çok veritabanının başarısız olmasını yönetmek için bir veya birkaç başarısız grup kullanma

Farklı bölgelerdeki iki sunucu (birincil ve ikincil sunucular) arasında bir veya birden çok başarısız grup oluşturulabilir. Her grup, birincil bölgedeki bir kesinti nedeniyle tüm veya bazı birincil veritabanlarının kullanılamaması durumunda birim olarak kurtarılan bir veya birkaç veritabanları içerebilir. Failover grubu birincil olarak aynı hizmet hedefi ile jeo-ikincil veritabanı oluşturur. Başarısız gruba varolan bir coğrafi çoğaltma ilişkisi eklerseniz, jeo-ikincil işlemin birincil le aynı hizmet katmanı ve bilgi işlem boyutuyla yapılandırıldığından emin olun.
  
> [!IMPORTANT]
> Farklı aboneliklerde iki sunucu arasında başarısız gruplar oluşturmak şu anda tek veritabanları ve esnek havuzlar için desteklenmemektedir. Başarısız grup oluşturulduktan sonra birincil veya ikincil sunucuyu farklı bir aboneye taşırsanız, başarısız lık isteklerinin ve diğer işlemlerin arızalanmasına neden olabilir.

### <a name="using-read-write-listener-for-oltp-workload"></a>OLTP iş yükü için okuma yazma dinleyicisi kullanma

OLTP işlemleri gerçekleştirirken `<fog-name>.database.windows.net` sunucu URL'si olarak kullanın ve bağlantılar otomatik olarak birincil edoğru yönlendirilir. Bu URL, başarısız olduktan sonra değişmez. Hatanın DNS kaydının güncelleştirilmesini içerdiğini, böylece istemci bağlantılarının yalnızca istemci DNS önbelleği yenilendikten sonra yeni birincil ana tonlarına yönlendirilmesini gerektirdiğini unutmayın.

### <a name="using-read-only-listener-for-read-only-workload"></a>Salt okunur iş yükü için salt okunur dinleyicikullanma

Belirli veri bayatlığına toleranslı, mantıksal olarak yalıtılmış salt salt okuma iş yükünüz varsa, uygulamada ikincil veritabanını kullanabilirsiniz. Salt okunur oturumlar `<fog-name>.secondary.database.windows.net` için sunucu URL'si olarak kullanın ve bağlantı otomatik olarak ikincil oturuma yönlendirilir. Ayrıca bağlantı dizesi kullanarak niyet okuma `ApplicationIntent=ReadOnly`belirtmek önerilir. Salt okunur iş yükünün başarısız olduktan sonra yeniden bağlanabilmesini veya ikincil sunucunun çevrimdışı duruma geldiğini `AllowReadOnlyFailoverToPrimary` görmek istiyorsanız, başarısızlık ilkesinin özelliğini yapılandırdığınızdan emin olun.

### <a name="preparing-for-performance-degradation"></a>Performans düşüşüne hazırlık

Tipik bir Azure uygulaması birden çok Azure hizmeti kullanır ve birden çok bileşenden oluşur. Başarısız grubun otomatik olarak başarısız olması, yalnızca Azure SQL bileşenlerinin durumuna göre tetiklenir. Birincil bölgedeki diğer Azure hizmetleri kesintiden etkilenmeyebilir ve bileşenleri bu bölgede kullanılabilir durumda olabilir. Birincil veritabanları DR bölgesine geçtikten sonra, bağımlı bileşenler arasındaki gecikme artabilir. Uygulamanın performansı üzerinde daha yüksek gecikme süresinin etkisini önlemek için, DR bölgesindeki tüm uygulama bileşenlerinin artıklığını sağlayın ve bu [ağ güvenliği yönergelerine](#failover-groups-and-network-security)uyun.

### <a name="preparing-for-data-loss"></a>Veri kaybına hazırlanma

Bir kesinti algılanırsa, SQL tarafından `GracePeriodWithDataLossHours`belirtilen dönemi bekler. Varsayılan değer 1 saattir. Veri kaybını karşılayamıyorsanız, 24 saat gibi yeterince büyük bir sayıya ayarladığınızdan `GracePeriodWithDataLossHours` emin olun. İkincilden birincile geri başarısız olmak için el ile grup başarısız kullanın.

> [!IMPORTANT]
> 800 veya daha az DT'ye sahip elastik havuzlar ve coğrafi çoğaltma kullanan 250'den fazla veritabanları, daha uzun planlanmış arızalar ve bozulmuş performans gibi sorunlarla karşılaşabilir.  Bu sorunlar, coğrafi çoğaltma uç noktaları coğrafya ile geniş ölçüde ayrıldığında veya her veritabanı için birden çok ikincil uç nokta kullanıldığında, yazma yoğun iş yükleri için daha olasıdır.  Bu sorunların belirtileri, jeo-çoğaltma gecikmesi zaman içinde arttığında gösterilir.  Bu gecikme [sys.dm_geo_replication_link_status](/sql/relational-databases/system-dynamic-management-views/sys-dm-geo-replication-link-status-azure-sql-database)kullanılarak izlenebilir.  Bu sorunlar oluşursa, azaltmalar havuz DT'lerinin sayısını artırmayı veya aynı havuzdaki coğrafi olarak çoğaltılan veritabanlarının sayısını azaltmayı içerir.

### <a name="changing-secondary-region-of-the-failover-group"></a>Başarısız grubun ikincil bölgesini değiştirme

Değişiklik sırasını göstermek için, Sunucu A'nın birincil sunucu, B sunucusunun varolan ikincil sunucu ve sunucu C'nin üçüncü bölgedeki yeni ikincil sunucu olduğunu varsayacağız.  Geçiş yapmak için aşağıdaki adımları izleyin:

1.  [Etkin coğrafi çoğaltma](sql-database-active-geo-replication.md)kullanarak sunucu A'dan sunucu C'ye her veritabanının ek saniyelerini oluşturun. Sunucu A'daki her veritabanında, biri B, diğeri sunucu C'de olmak üzere iki saniyelik olacaktır. Bu, geçiş sırasında birincil veritabanlarının korumaaltında kalmasını garanti eder.
2.  Başarısız grubu silin. Bu noktada oturum açmabaşarısız olacaktır. Bunun nedeni, failover grubu dinleyicilerinin SQL diğer adlarının silinmiş olması ve ağ geçidinin failover grup adını tanımamasıdır.
3.  A ve C sunucuları arasında aynı ada sahip başarısız grubu yeniden oluşturun. Bu noktada oturum açmabaşarısız durdurulacaktır.
4.  Sunucu A'daki tüm birincil veritabanlarını yeni failover grubuna ekleyin.
5.  Bırak sunucu B. B'deki tüm veritabanları otomatik olarak silinir. 


### <a name="changing-primary-region-of-the-failover-group"></a>Başarısız grubun birincil bölgesini değiştirme

Değişiklik sırasını göstermek için, sunucu A'nın birincil sunucu, B sunucusunun varolan ikincil sunucu ve C sunucusunun üçüncü bölgedeki yeni birincil sunucu olduğunu varsayacağız.  Geçiş yapmak için aşağıdaki adımları izleyin:

1.  Birincil sunucuyu B. Server A'ya geçmek için planlanan bir hata gerçekleştirin yeni ikincil sunucu olacak. Bu hata birkaç dakikalık kapalı kalma süresine neden olabilir. Gerçek zaman, başarısız grubun boyutuna bağlıdır.
2.  [Etkin coğrafi çoğaltma](sql-database-active-geo-replication.md)kullanarak sunucu B'den sunucu C'ye her veritabanının ek saniyelerini oluşturun. Sunucu B'deki her veritabanında, biri A, diğeri sunucu C'de olmak üzere iki saniyelik olacaktır. Bu, geçiş sırasında birincil veritabanlarının korumaaltında kalmasını garanti eder.
3.  Başarısız grubu silin. Bu noktada oturum açmabaşarısız olacaktır. Bunun nedeni, failover grubu dinleyicilerinin SQL diğer adlarının silinmiş olması ve ağ geçidinin failover grup adını tanımamasıdır.
4.  A ve C sunucuları arasında aynı ada sahip başarısız grubu yeniden oluşturun. Bu noktada oturum açmabaşarısız durdurulacaktır.
5.  B'deki tüm birincil veritabanlarını yeni başarısız gruba ekleyin. 
6.  B ve C'yi değiştirmek için başarısız grubun planlı bir hatasını gerçekleştirin. Şimdi sunucu C birincil ve B olacak - ikincil. Sunucu A'daki tüm ikincil veritabanları otomatik olarak C'deki önceliklere bağlanır. 1. adımda olduğu gibi, başarısızlık birkaç dakika kapalı kalma süresine neden olabilir.
6.  Sunucu A'yı bırakın. A'daki tüm veritabanları otomatik olarak silinir.

> [!IMPORTANT]
> Başarısız grup silindiğinde, dinleyici uç noktalarının DNS kayıtları da silinir. Bu noktada, başka birinin aynı ada sahip bir başarısız grup veya sunucu takma adı oluşturma olasılığı sıfır değildir ve bu da onu yeniden kullanmanızı engeller. Riski en aza indirmek için genel hata grubu adlarını kullanmayın.

## <a name="best-practices-of-using-failover-groups-with-managed-instances"></a>Yönetilen örneklerle başarısız grupları kullanmanın en iyi uygulamaları

Otomatik hata grubu birincil örnekte yapılandırılmalıdır ve bunu farklı bir Azure bölgesindeki ikincil örnek'e bağlar.  Örnekteki tüm veritabanları ikincil örnek için çoğaltılır.

Aşağıdaki diyagram, yönetilen örnek ve otomatik hata grubu kullanarak coğrafi yedekli bulut uygulamasının tipik bir yapılandırmasını göstermektedir.

![otomatik failover](./media/sql-database-auto-failover-group/auto-failover-group-mi.png)

> [!NOTE]
> Bkz. Ayrıntılı bir adım adım öğretici için [başarısız bir gruba yönetilen örneği ekle](sql-database-managed-instance-failover-group-tutorial.md) ve başarısız grubu kullanmak için yönetilen bir örnek ekleyin.

Uygulamanız yönetilen örneği veri katmanı olarak kullanıyorsa, iş sürekliliği için tasarım yaparken aşağıdaki genel yönergelere uyun:

### <a name="creating-the-secondary-instance"></a>İkincil örneği oluşturma 

Başarısız olduktan sonra birincil örneğe kesintisiz bağlantı sağlamak için hem birincil hem de ikincil örneklerin aynı DNS bölgesinde olması gerekir. Aynı çok etki alanı (SAN) sertifikasının istemci bağlantılarını başarısız gruptaki iki örnekten herhangi birinin kimliğini doğrulamak için kullanılabileceğini garanti eder. Uygulamanız üretim dağıtımı na hazır olduğunda, farklı bir bölgede ikincil bir örnek oluşturun ve DNS bölgesini birincil örnekle paylaştığından emin olun. Bunu Azure portalını, `DNS Zone Partner` PowerShell'i veya REST API'sini kullanarak isteğe bağlı bir parametre belirterek yapabilirsiniz.

> [!IMPORTANT]
> Alt ağda oluşturulan ilk örnek, aynı alt ağdaki sonraki tüm örnekler için DNS bölgesini belirler. Bu, aynı alt ağdan iki örneğin farklı DNS bölgelerine ait olamayacağı anlamına gelir.

Birincil örnekle aynı DNS bölgesinde ikincil örnek oluşturma hakkında daha fazla bilgi için [bkz.](sql-database-managed-instance-failover-group-tutorial.md#3---create-a-secondary-managed-instance)

### <a name="enabling-replication-traffic-between-two-instances"></a>İki örnek arasındaki çoğaltma trafiğini etkinleştirme

Her örnek kendi VNet'inde yalıtılmış olduğundan, bu VNet'ler arasındaki iki yönlü trafiğe izin verilmelidir. Bkz. [Azure VPN ağ geçidi](../vpn-gateway/vpn-gateway-about-vpngateways.md)

### <a name="creating-a-failover-group-between-managed-instances-in-different-subscriptions"></a>Farklı aboneliklerde yönetilen örnekler arasında bir başarısız grup oluşturma

İki farklı abonelikte yönetilen örnekler arasında bir başarısız lık grubu oluşturabilirsiniz. PowerShell API kullanırken ikincil örnek için `PartnerSubscriptionId` parametre belirterek bunu yapabilirsiniz. REST API'yi kullanırken, `properties.managedInstancePairs` parametreye dahil olan her örnek kimliğin kendi abonelik kimliği olabilir.
  
> [!IMPORTANT]
> Azure portalı, farklı abonelikler arasında başarısız grupların oluşturulmasını desteklemez. Ayrıca, farklı abonelikler ve/veya kaynak grupları arasında varolan başarısız gruplar için, ana örnekten portal aracılığıyla el ile başlatılamaz. Bunun yerine jeo-ikincil örnekten başlatın.

### <a name="managing-failover-to-secondary-instance"></a>İkincil örneğe failover yönetme

Failover grubu, örnekteki tüm veritabanlarının başarısızlığı yönetir. Bir grup oluşturulduğunda, örnekteki her veritabanı otomatik olarak ikincil örnek için coğrafi olarak çoğaltılır. Veritabanlarının bir alt kümesinin kısmi bir hatasını başlatmak için failover gruplarını kullanamazsınız.

> [!IMPORTANT]
> Bir veritabanı birincil örnekten kaldırılırsa, aynı zamanda jeo ikincil örnekte otomatik olarak bırakılır.

### <a name="using-read-write-listener-for-oltp-workload"></a>OLTP iş yükü için okuma yazma dinleyicisi kullanma

OLTP işlemleri gerçekleştirirken `<fog-name>.zone_id.database.windows.net` sunucu URL'si olarak kullanın ve bağlantılar otomatik olarak birincil edoğru yönlendirilir. Bu URL, başarısız olduktan sonra değişmez. Hata, DNS kaydının güncelleştirilmesini içerir, bu nedenle istemci bağlantıları yalnızca istemci DNS önbelleği yenilendikten sonra yeni birincil eve yönlendirilir. İkincil örnek DNS bölgesini birincil le paylaştığından, istemci uygulaması aynı SAN sertifikasını kullanarak bu bölgeye yeniden bağlanabilecektir.

### <a name="using-read-only-listener-to-connect-to-the-secondary-instance"></a>İkincil örne bağlanmak için salt okunur dinleyicikullanma

Belirli veri bayatlığına toleranslı, mantıksal olarak yalıtılmış salt salt okuma iş yükünüz varsa, uygulamada ikincil veritabanını kullanabilirsiniz. Doğrudan coğrafi olarak çoğaltılan ikincil bağlantı `server.secondary.zone_id.database.windows.net` için sunucu URL'si olarak kullanın ve bağlantı doğrudan coğrafi olarak çoğaltılan ikincil yapılır.

> [!NOTE]
> Azure SQL Veritabanı, belirli hizmet katmanlarında, yalnızca salt okunur sorgu iş yüklerini yalnızca okunur yineleme nin kapasitesini `ApplicationIntent=ReadOnly` kullanarak ve bağlantı dizesinde parametreyi kullanmak için [salt okunur yinelemelerin](sql-database-read-scale-out.md) kullanımını destekler. Coğrafi olarak çoğaltılan bir ikincil yapılandırıldığınızda, bu özelliği birincil konumda veya coğrafi olarak çoğaltılan konuma salt okunur yinelemeye bağlanmak için kullanabilirsiniz.
> - Birincil konumda salt okunur yinelemeye bağlanmak için `<fog-name>.zone_id.database.windows.net`.
> - İkincil konumda salt okunur yinelemeye bağlanmak `<fog-name>.secondary.zone_id.database.windows.net`için .

### <a name="preparing-for-performance-degradation"></a>Performans düşüşüne hazırlık

Tipik bir Azure uygulaması birden çok Azure hizmeti kullanır ve birden çok bileşenden oluşur. Başarısız grubun otomatik olarak başarısız olması, yalnızca Azure SQL bileşenlerinin durumuna göre tetiklenir. Birincil bölgedeki diğer Azure hizmetleri kesintiden etkilenmeyebilir ve bileşenleri bu bölgede kullanılabilir durumda olabilir. Birincil veritabanları DR bölgesine geçtikten sonra, bağımlı bileşenler arasındaki gecikme artabilir. Uygulamanın performansı üzerinde daha yüksek gecikme süresinin etkisini önlemek için, DR bölgesindeki tüm uygulama bileşenlerinin artıklığını sağlayın ve bu [ağ güvenliği yönergelerine](#failover-groups-and-network-security)uyun.

### <a name="preparing-for-data-loss"></a>Veri kaybına hazırlanma

Bir kesinti algılanırsa, bilgimizin en iyi olduğu yerde sıfır veri kaybı varsa SQL otomatik olarak okuma yazma hatasını tetikler. Aksi takdirde, tarafından belirtilen dönemi `GracePeriodWithDataLossHours`bekler. Belirttiyseniz, `GracePeriodWithDataLossHours`veri kaybına karşı hazırlıklı olun. Genel olarak, kesintiler sırasında Azure kullanılabilirliği tercih eder. Veri kaybını göze alamıyorsanız, GracePeriodWithDataLossHours'u 24 saat gibi yeterince büyük bir sayıya ayarladığınızdan emin olun.

Okuma yazma dinleyicisinin DNS güncelleştirmesi, başarısızlık başlatıldıktan hemen sonra gerçekleşir. Bu işlem veri kaybına neden olmaz. Ancak, veritabanı rollerini değiştirme işlemi normal koşullar altında 5 dakika kadar sürebilir. Tamamlanana kadar, yeni birincil örnekteki bazı veritabanları yine de salt okunur. PowerShell kullanılarak başarısız olursa, tüm işlem eşzamanlıdır. Azure portalı kullanılarak başlatılırsa, UI tamamlanma durumunu gösterir. REST API kullanılarak başlatılırsa, tamamlanmasını izlemek için standart Azure Kaynak Yöneticisi'nin yoklama mekanizmasını kullanın.

> [!IMPORTANT]
> Öncelikler orijinal konuma geri taşımak için el ile grup başarısız kullanın. Başarısıza neden olan kesinti azaltıldığında, birincil veritabanlarınızı özgün konuma taşıyabilirsiniz. Bunu yapmak için grubun el ile başarısız başlatılması gerekir.
  
### <a name="changing-secondary-region-of-the-failover-group"></a>Başarısız grubun ikincil bölgesini değiştirme

A örneğinin birincil örnek olduğunu, B örneğinin varolan ikincil örnek olduğunu ve C örneğinin üçüncü bölgedeki yeni ikincil örnek olduğunu varsayalım.  Geçiş yapmak için aşağıdaki adımları izleyin:

1.  A ile aynı boyutta ve aynı DNS bölgesinde C örneği oluşturun. 
2.  A ve B örnekleri arasındaki başarısız grubu silin. Bu noktada, failover grubu dinleyicilerinin SQL diğer adları silindiğinden ve ağ geçidi failover grup adını tanımadığıiçin oturum açmalar başarısız olacaktır. İkincil veritabanları önceliklenlerden kesilir ve okuma-yazma veritabanları haline gelir. 
3.  Örnek A ve C arasında aynı ada sahip bir [failover group with managed instance tutorial](sql-database-managed-instance-failover-group-tutorial.md)başarısız grup oluşturun. Bu bir veri boyutu işlemidir ve A örneğindeki tüm veritabanları tohumlanıp eşitlendiğinde tamamlanır.
4.  Gereksiz ücretleri önlemek için gerekli değilse B örneğini silin.

> [!NOTE]
> Adım 2 ve adım 3 tamamlanana kadar veritabanları örneğin A örneği A'nın yıkıcı bir hatasına karşı korumasız kalır.

### <a name="changing-primary-region-of-the-failover-group"></a>Başarısız grubun birincil bölgesini değiştirme

A örneğinin birincil örnek olduğunu varsayalım, B örneği varolan ikincil örnektir ve C örneği üçüncü bölgedeki yeni birincil örnektir.  Geçiş yapmak için aşağıdaki adımları izleyin:

1.  B ile aynı boyutta ve aynı DNS bölgesinde C örneği oluşturun. 
2.  B örneğine bağlanın ve birincil örneği B. Örnek A'ya geçmek için el ile başarısız olun, otomatik olarak yeni ikincil örnek haline gelir.
3.  A ve B örnekleri arasındaki başarısız grubu silin. Bu noktada, failover grubu dinleyicilerinin SQL diğer adları silindiğinden ve ağ geçidi failover grup adını tanımadığıiçin oturum açmalar başarısız olacaktır. İkincil veritabanları önceliklenlerden kesilir ve okuma-yazma veritabanları haline gelir. 
4.  Örnek A ve C arasında aynı ada sahip bir başarısız [failover group with managed instance tutorial](sql-database-managed-instance-failover-group-tutorial.md)grup oluşturun. Bu bir veri boyutu işlemidir ve A örneğindeki tüm veritabanları tohumlanıp eşitlendiğinde tamamlanır.
5.  Gereksiz ücretleri önlemek için gerekli değilse a örneğini silin.

> [!NOTE] 
> Adım 3 ve adım 4 tamamlanana kadar veritabanları örneğin A örneği A bir felaket hatasına karşı korumasız kalır.

> [!IMPORTANT]
> Başarısız grup silindiğinde, dinleyici uç noktalarının DNS kayıtları da silinir. Bu noktada, başka birinin aynı ada sahip bir başarısız grup veya sunucu takma adı oluşturma olasılığı sıfır değildir ve bu da onu yeniden kullanmanızı engeller. Riski en aza indirmek için genel hata grubu adlarını kullanmayın.

## <a name="failover-groups-and-network-security"></a>Failover grupları ve ağ güvenliği

Bazı uygulamalar için güvenlik kuralları, veri katmanına ağ erişiminin VM, web hizmeti vb. gibi belirli bir bileşen veya bileşenlerle sınırlandırılmasını gerektirir. Bu gereksinim, iş sürekliliği tasarımı ve başarısız grupların kullanımı için bazı zorluklar sunmaktadır. Bu tür kısıtlı erişimi uygularken aşağıdaki seçenekleri göz önünde bulundurun.

### <a name="using-failover-groups-and-virtual-network-rules"></a>Başarısız grupları ve sanal ağ kurallarını kullanma

SQL veritabanınıza erişimi kısıtlamak için [Sanal Ağ hizmet bitiş noktalarını ve kurallarını](sql-database-vnet-service-endpoint-rule-overview.md) kullanıyorsanız, Her Sanal Ağ hizmeti bitiş noktasının yalnızca bir Azure bölgesi için geçerli olduğunu unutmayın. Bitiş noktası, diğer bölgelerin alt ağdan iletişimi kabul etmesini sağlamaz. Bu nedenle, yalnızca aynı bölgede dağıtılan istemci uygulamaları birincil veritabanına bağlanabilir. SQL istemci oturumlarının farklı (ikincil) bir bölgedeki bir sunucuya yönlendirilmesinde başarısız olduğundan, bu oturumlar bu bölgenin dışındaki bir istemciden kaynaklanıyorsa başarısız olur. Bu nedenle, katılan sunucular Sanal Ağ kurallarına dahil edilirse otomatik hata yapma ilkesi etkinleştirilemez. El ile başarısız olmayı desteklemek için aşağıdaki adımları izleyin:

1. Uygulamanızın ön uç bileşenlerinin (web hizmeti, sanal makineler vb.) yedek kopyalarını ikincil bölgede sağlama
2. Birincil ve ikincil sunucu için [sanal ağ kurallarını](sql-database-vnet-service-endpoint-rule-overview.md) ayrı ayrı yapılandırma
3. Trafik [yöneticisi yapılandırmasını kullanarak ön uç hatasını](sql-database-designing-cloud-solutions-for-disaster-recovery.md#scenario-1-using-two-azure-regions-for-business-continuity-with-minimal-downtime) etkinleştirme
4. Kesinti algılandığında manuel arıza başlatın. Bu seçenek, ön uç ve veri katmanı arasında tutarlı gecikme süresi gerektiren uygulamalar için optimize edilmiştir ve ön uç, veri katmanı veya her ikisi de kesintiden etkilendiğinde kurtarmayı destekler.

> [!NOTE]
> Salt okunur iş yükünü yüklemek için **salt okunur dinleyiciyi** kullanıyorsanız, bu iş yükünün ikincil veritabanına bağlanabilmesi için bu iş yükünün ikincil bölgedeki bir VM veya başka bir kaynakta yürütüldiğinden emin olun.

### <a name="using-failover-groups-and-sql-database-firewall-rules"></a>Failover gruplarını ve SQL veritabanı güvenlik duvarı kurallarını kullanma

İş sürekliliği planınız otomatik olarak başarısız olan grupları kullanarak başarısız olmayı gerektiriyorsa, geleneksel güvenlik duvarı kurallarını kullanarak SQL veritabanınıza erişimi kısıtlayabilirsiniz. Otomatik başarısızolmayı desteklemek için aşağıdaki adımları izleyin:

1. [Herkese açık bir IP oluşturma](../virtual-network/virtual-network-public-ip-address.md#create-a-public-ip-address)
2. [Bir ortak yük dengeleyicisi oluşturun](../load-balancer/quickstart-load-balancer-standard-public-portal.md) ve genel IP'yi buna atayın.
3. Ön uç bileşenleriniz için [sanal ağ ve sanal makineler oluşturun](../load-balancer/quickstart-load-balancer-standard-public-portal.md)
4. [Ağ güvenlik grubu oluşturun](../virtual-network/security-overview.md) ve gelen bağlantıları yapılandırın.
5. Giden bağlantıların 'Sql' [hizmet etiketini](../virtual-network/security-overview.md#service-tags)kullanarak Azure SQL veritabanına açık olduğundan emin olun.
6. Adım 1'de oluşturduğunuz genel IP adresinden gelen trafiğine izin vermek için bir [SQL veritabanı güvenlik duvarı kuralı](sql-database-firewall-configure.md) oluşturun.

Giden erişimin nasıl yapılandırılabilen ve güvenlik duvarı kurallarında hangi IP'nin kullanılacağı hakkında daha fazla bilgi için [Yük bakiyesi giden bağlantılarına](../load-balancer/load-balancer-outbound-connections.md)bakın.

Yukarıdaki yapılandırma, otomatik arızanın ön uç bileşenlerindeki bağlantıları engellememesini sağlar ve uygulamanın ön uç ile veri katmanı arasındaki uzun gecikme süresine tahammül edemeyeceğini varsayar.

> [!IMPORTANT]
> Bölgesel kesintiler için iş sürekliliğini garanti etmek için hem ön uç bileşenleri hem de veritabanları için coğrafi fazlalık sağlamanız gerekir.

## <a name="enabling-geo-replication-between-managed-instances-and-their-vnets"></a>Yönetilen örnekler ve VNet'leri arasında coğrafi çoğaltmayı etkinleştirme

İki farklı bölgede birincil ve ikincil yönetilen örnekler arasında bir başarısızlık grubu ayarladığınızda, her örnek bağımsız bir sanal ağ kullanılarak yalıtılır. Bu VNet'ler arasındaki çoğaltma trafiğinin karşılanmasını sağlamak için bu ön koşullar karşılanır:

1. Yönetilen iki örneğin farklı Azure bölgelerinde olması gerekir.
2. Yönetilen iki örneğin aynı hizmet katmanı olması ve aynı depolama boyutuna sahip olması gerekir.
3. İkincil yönetilen örneğiniz boş olmalıdır (kullanıcı veritabanları olmamalıdır).
4. Yönetilen örnekler tarafından kullanılan sanal ağların vpn [ağ geçidi](../vpn-gateway/vpn-gateway-about-vpngateways.md) veya [Ekspres Rota](../expressroute/expressroute-howto-circuit-portal-resource-manager.md)üzerinden bağlanması gerekir. İki sanal ağ şirket içi ağ üzerinden bağlandığında, 5022 ve 11000-11999 bağlantı noktalarını engelleyen güvenlik duvarı kuralı olmadığından emin olun. Global VNet Peering desteklenmez.
5. Yönetilen iki örnek VNets çakışan IP adresleri olamaz.
6. Ağ Güvenlik Gruplarınızı (NSG) 5022 ve 11000~12000 aralığının açık ve diğer yönetilen örneğin alt netinden bağlantılar için giden olacak şekilde ayarlamanız gerekir. Bu, örnekler arasında çoğaltma trafiğine izin vermekiçindir.

   > [!IMPORTANT]
   > Yanlış yapılandırılmış NSG güvenlik kuralları, veritabanı kopyalama işlemlerine neden olabilir.

7. İkincil örnek doğru DNS bölge kimliği ile yapılandırılır. DNS bölgesi yönetilen bir örneğin ve sanal kümenin özelliğidir ve kimliği ana bilgisayar ad adresine dahildir. Bölge kimliği, her VNet'te ilk yönetilen örnek oluşturulduğunda ve aynı kimlik aynı alt ağdaki diğer tüm örneklere atandığında rasgele bir dize olarak oluşturulur. Bir kez atandıktan sonra, DNS bölgesi değiştirilemez. Aynı başarısız grupta yer alan yönetilen örneklerin DNS bölgesini paylaşması gerekir. Bunu, ikincil örneği oluştururken birincil örneğin bölge kimliğini DnsZonePartner parametresinin değeri olarak geçirerek gerçekleştirirsiniz. 

   > [!NOTE]
   > Yönetilen örnekle failover gruplarını yapılandırma hakkında ayrıntılı [bir](sql-database-managed-instance-failover-group-tutorial.md)öğretici için bkz.

## <a name="upgrading-or-downgrading-a-primary-database"></a>Birincil veritabanını yükseltme veya düşürme

Birincil veritabanını, ikincil veritabanlarını kesmeden farklı bir işlem boyutuna (Genel Amaç ve İş Açısından Genel Olarak değil, aynı hizmet katmanı içinde) yükseltebilir veya düşürebilirsiniz. Yükseltme yaparken, önce ikincil veritabanlarının tümlerini yükseltmenizi ve ardından birincil veritabanını yükseltmenizi öneririz. Düşürme yaparken, sırayı tersine çevirin: önce birincil veritabanını düşürün ve ardından ikincil veritabanlarının tümünün aşağısını indirin. Veritabanını farklı bir hizmet katmanına yükselttidiğinizde veya düşürdüğünüzde, bu öneri uygulanır.

Bu dizi, özellikle daha düşük bir SKU'daki ikincil işlemin aşırı yüklendiği ve yükseltme veya düşürme işlemi sırasında yeniden tohumlanmış olması gereken sorunu önlemek için önerilir. Ayrıca, birincil okuma-yazma iş yüklerini birincile karşı etkilemek pahasına birincil salt okunur yaparak sorunu önleyebilirsiniz.

> [!NOTE]
> Başarısız grup yapılandırmasının bir parçası olarak ikincil veritabanı oluşturduysanız, ikincil veritabanını düşürmeniz önerilmez. Bu, veri katmanınızın, başarısız olduktan sonra düzenli iş yükünüzü işlemek için yeterli kapasiteye sahip olmasını sağlamak tır.

## <a name="preventing-the-loss-of-critical-data"></a>Kritik verilerin kaybını önleme

Geniş alan ağlarının yüksek gecikme sayılması nedeniyle, sürekli kopya bir eşzamanlı çoğaltma mekanizması kullanır. Asynchronous çoğaltma, bir hata oluşursa bazı veri kaybını kaçınılmaz hale getirir. Ancak, bazı uygulamalar veri kaybı gerektirebilir. Bu kritik güncelleştirmeleri korumak için, bir uygulama geliştiricisi işlemi yaptıktan hemen sonra [sp_wait_for_database_copy_sync](/sql/relational-databases/system-stored-procedures/active-geo-replication-sp-wait-for-database-copy-sync) sistem yordamını arayabilir. Son `sp_wait_for_database_copy_sync` işlenen işlem ikincil veritabanına iletilene kadar arama iş parçacığı engeller. Ancak, aktarılan hareketlerin ikincil işlemde yeniden oynatılması ve işlenmesini beklemez. `sp_wait_for_database_copy_sync`belirli bir sürekli kopyalama bağlantısına göre yapılır. Birincil veritabanına bağlantı haklarına sahip herhangi bir kullanıcı bu yordamı arayabilir.

> [!NOTE]
> `sp_wait_for_database_copy_sync`başarısız olduktan sonra veri kaybını önler, ancak okuma erişimi için tam eşitleme garantisi vermez. `sp_wait_for_database_copy_sync` Yordam çağrısının neden olduğu gecikme önemli olabilir ve arama sırasındaki hareket günlüğünün boyutuna bağlıdır.

## <a name="failover-groups-and-point-in-time-restore"></a>Failover grupları ve nokta-in-time geri yükleme

Başarısız gruplarla zaman içinde geri yükleme kullanma hakkında bilgi için [bkz.](sql-database-recovery-using-backups.md#point-in-time-restore)

## <a name="limitations-of-failover-groups"></a>Başarısız grupların sınırlamaları

Aşağıdaki sınırlamalardan haberdar olun:

- Failover grubu, aynı Azure bölgelerindeki iki sunucu veya örnek arasında oluşturulamaz.
- Failover grubu yeniden adlandırılamaz. Grubu silmeniz ve farklı bir adla yeniden oluşturmanız gerekir. 
- Veritabanı yeniden adı failover grubundaki örnekler için desteklenmez. Bir veritabanını yeniden adlandırabilmek için failover grubunu geçici olarak silmeniz gerekir.

## <a name="programmatically-managing-failover-groups"></a>Başarısız grupları programlı bir şekilde yönetme

Daha önce de tartışıldığı gibi, otomatik hata grupları ve etkin coğrafi çoğaltma, Azure PowerShell ve REST API kullanılarak programlı olarak da yönetilebilir. Aşağıdaki tablolarda kullanılabilir komutkümesi açıklayınız. Etkin coğrafi çoğaltma, Azure SQL Database REST API ve [Azure PowerShell cmdlets](https://docs.microsoft.com/powershell/azure/overview)dahil olmak üzere yönetim için bir dizi Azure Kaynak Yöneticisi [API'sini](https://docs.microsoft.com/rest/api/sql/) içerir. Bu API'ler kaynak gruplarının kullanımını ve rol tabanlı güvenliği (RBAC) desteklemeyi gerektirir. Erişim rollerinin nasıl uygulanacağı hakkında daha fazla bilgi için [Azure Role Tabanlı Erişim Denetimi'ne](../role-based-access-control/overview.md)bakın.

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)

### <a name="manage-sql-database-failover-with-single-databases-and-elastic-pools"></a>SQL veritabanı failover'ı tek veritabanları ve elastik havuzlarla yönetme

| Cmdlet | Açıklama |
| --- | --- |
| [Yeni-AzSqlDatabaseFailoverGroup](/powershell/module/az.sql/new-azsqldatabasefailovergroup) |Bu komut bir başarısız grup oluşturur ve hem birincil hem de ikincil sunucularda kaydeder|
| [Kaldır-AzSqlDatabaseFailoverGroup](/powershell/module/az.sql/remove-azsqldatabasefailovergroup) | Bir hata grubunu sunucudan kaldırır |
| [Get-AzSqlDatabaseFailoverGroup](/powershell/module/az.sql/get-azsqldatabasefailovergroup) | Başarısız grubun yapılandırması alır |
| [Set-AzSqlDatabaseFailoverGroup](/powershell/module/az.sql/set-azsqldatabasefailovergroup) |Bir başarısız grubun yapılandırması modifies |
| [Switch-AzSqlDatabaseFailoverGroup](/powershell/module/az.sql/switch-azsqldatabasefailovergroup) | Bir başarısız grubun ikincil sunucuya başarısız olmasını tetikler |
| [Ekle-AzSqlDatabaseToFailoverGroup](/powershell/module/az.sql/add-azsqldatabasetofailovergroup)|Bir başarısız gruba bir veya daha fazla veritabanı ekler|

### <a name="manage-sql-database-failover-groups-with-managed-instances"></a>Yönetilen örneklerle SQL veritabanı başarısız gruplarını yönetme

| Cmdlet | Açıklama |
| --- | --- |
| [Yeni-AzSqlDatabaseInstanceFailoverGroup](/powershell/module/az.sql/new-azsqldatabaseinstancefailovergroup) |Bu komut bir başarısız grup oluşturur ve hem birincil hem de ikincil örneklerde kaydeder|
| [Set-AzSqlDatabaseInstanceFailoverGroup](/powershell/module/az.sql/set-azsqldatabaseinstancefailovergroup) |Bir başarısız grubun yapılandırması modifies|
| [Get-AzSqlDatabaseInstanceFailoverGroup](/powershell/module/az.sql/get-azsqldatabaseinstancefailovergroup) |Başarısız grubun yapılandırması alır|
| [Switch-AzSqlDatabaseInstanceFailoverGroup](/powershell/module/az.sql/switch-azsqldatabaseinstancefailovergroup) |Bir başarısız grubun ikincil örne başarısız olmasını tetikler|
| [Kaldır-AzSqlDatabaseInstanceFailoverGroup](/powershell/module/az.sql/remove-azsqldatabaseinstancefailovergroup) | Bir başarısız grubu kaldırır|

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

### <a name="manage-sql-database-failover-with-single-databases-and-elastic-pools"></a>SQL veritabanı failover'ı tek veritabanları ve elastik havuzlarla yönetme

| Komut | Açıklama |
| --- | --- |
| [az sql failover-grup oluşturma](/cli/azure/sql/failover-group#az-sql-failover-group-create) |Bu komut bir başarısız grup oluşturur ve hem birincil hem de ikincil sunucularda kaydeder|
| [az sql failover-grup silme](/cli/azure/sql/failover-group#az-sql-failover-group-delete) | Bir hata grubunu sunucudan kaldırır |
| [az sql failover-group gösterisi](/cli/azure/sql/failover-group#az-sql-failover-group-show) | Başarısız grup yapılandırması alır |
| [az sql failover-group güncelleme](/cli/azure/sql/failover-group#az-sql-failover-group-update) |Bir failover grubunun yapılandırmasını değiştirir ve/veya bir veya daha fazla veritabanıyı bir başarısız gruba ekler|
| [az sql failover-group set-primary](/cli/azure/sql/failover-group#az-sql-failover-group-set-primary) | Bir başarısız grubun ikincil sunucuya başarısız olmasını tetikler |

### <a name="manage-sql-database-failover-groups-with-managed-instances"></a>Yönetilen örneklerle SQL veritabanı başarısız gruplarını yönetme

| Komut | Açıklama |
| --- | --- |
| [az sql instance-failover-group oluşturma](/cli/azure/sql/instance-failover-group#az-sql-instance-failover-group-create) | Bu komut bir başarısız grup oluşturur ve hem birincil hem de ikincil örneklerde kaydeder |
| [az sql instance-failover-group update](/cli/azure/sql/instance-failover-group#az-sql-instance-failover-group-update) | Başarısız grubun yapılandırması değişir|
| [az sql instance-failover-group show](/cli/azure/sql/instance-failover-group#az-sql-instance-failover-group-show) | Başarısız grubun yapılandırması alır|
| [az sql instance-failover-group set-primary](/cli/azure/sql/instance-failover-group#az-sql-instance-failover-group-set-primary) | Bir başarısız grubun ikincil örne başarısız olmasını tetikler|
| [az sql instance-failover-group silme](/cli/azure/sql/instance-failover-group#az-sql-instance-failover-group-delete) | Bir başarısız grubu kaldırır |

* * *

> [!IMPORTANT]
> Örnek bir komut dosyası [için](scripts/sql-database-add-single-db-to-failover-group-powershell.md)bkz.

### <a name="rest-api-manage-sql-database-failover-groups-with-single-and-pooled-databases"></a>REST API: SQL veritabanı hata gruplarını tek ve havuzlu veritabanlarıile yönetme

| API | Açıklama |
| --- | --- |
| [Failover Grubu Oluştur veya Güncelleştir](https://docs.microsoft.com/rest/api/sql/failovergroups/createorupdate) | Bir başarısız grup oluşturur veya güncelleştirir |
| [Failover Grubunu Sil](https://docs.microsoft.com/rest/api/sql/failovergroups/delete) | Bir hata grubunu sunucudan kaldırır |
| [Failover (Planlanmış)](https://docs.microsoft.com/rest/api/sql/failovergroups/failover) | Tam veri eşitleme ile geçerli birincil sunucudan ikincil sunucuya başarısız tetikler.|
| [Güç Failover Veri Kaybına İzin Ver](https://docs.microsoft.com/rest/api/sql/failovergroups/forcefailoverallowdataloss) | Verileri eşitlemeden geçerli birincil sunucudan ikincil sunucuya geçmeyi tetikler. Bu işlem veri kaybına neden olabilir. |
| [Failover Grubu alın](https://docs.microsoft.com/rest/api/sql/failovergroups/get) | Bir failover grubunun yapılandırması alır. |
| [Sunucuya Göre Failover Gruplarını Listele](https://docs.microsoft.com/rest/api/sql/failovergroups/listbyserver) | Bir sunucudaki başarısız grupları listeler. |
| [Failover Grubunu Güncelleştir](https://docs.microsoft.com/rest/api/sql/failovergroups/update) | Başarısız grubun yapılandırması güncellenir. |

### <a name="rest-api-manage-failover-groups-with-managed-instances"></a>REST API: Yönetilen Örneklerle başarısız grupları yönetme

| API | Açıklama |
| --- | --- |
| [Failover Grubu Oluştur veya Güncelleştir](https://docs.microsoft.com/rest/api/sql/instancefailovergroups/createorupdate) | Başarısız grubun yapılandırması oluşturur veya güncelleştirir |
| [Failover Grubunu Sil](https://docs.microsoft.com/rest/api/sql/instancefailovergroups/delete) | Bir hata grubunu örnekten kaldırır |
| [Failover (Planlanmış)](https://docs.microsoft.com/rest/api/sql/instancefailovergroups/failover) | Tam veri eşitlemesi ile geçerli birincil örnekten bu örne başarısız olmayı tetikler. |
| [Güç Failover Veri Kaybına İzin Ver](https://docs.microsoft.com/rest/api/sql/instancefailovergroups/forcefailoverallowdataloss) | Verileri eşitlemeden geçerli birincil örnekten ikincil örne başarısız olmayı tetikler. Bu işlem veri kaybına neden olabilir. |
| [Failover Grubu alın](https://docs.microsoft.com/rest/api/sql/instancefailovergroups/get) | bir failover grubunun yapılandırması alır. |
| [Failover Gruplarını Listele - Konuma Göre Listele](https://docs.microsoft.com/rest/api/sql/instancefailovergroups/listbylocation) | Bir konumdaki başarısız grupları listeler. |

## <a name="next-steps"></a>Sonraki adımlar

- Ayrıntılı eğitimler için bkz.
    - [Bir başarısız gruba tek veritabanı ekleme](sql-database-single-database-failover-group-tutorial.md)
    - [Yük devretme grubuna elastik havuz ekleme](sql-database-elastic-pool-failover-group-tutorial.md)
    - [Başarısız bir gruba yönetilen bir örnek ekleme](sql-database-managed-instance-failover-group-tutorial.md)
- Örnek komut dosyaları için bkz:
  - [Azure SQL Veritabanı'nda tek bir veritabanı için etkin coğrafi çoğaltma yapılandırmak için PowerShell'i kullanın](scripts/sql-database-setup-geodr-and-failover-database-powershell.md)
  - [Azure SQL Veritabanı'nda havuza konan bir veritabanı için etkin coğrafi çoğaltma yapılandırmak için PowerShell'i kullanın](scripts/sql-database-setup-geodr-and-failover-pool-powershell.md)
  - [Bir azure sql veritabanı tek veritabanını bir başarısız gruba eklemek için PowerShell'i kullanın](scripts/sql-database-add-single-db-to-failover-group-powershell.md)
- İş sürekliliğine genel bakış ve senaryolar için [bkz.](sql-database-business-continuity.md)
- Azure SQL Veritabanı otomatik yedeklemeleri hakkında bilgi edinmek için [SQL Veritabanı otomatik yedeklemelerine](sql-database-automated-backups.md)bakın.
- Kurtarma için otomatik yedeklemekullanma hakkında bilgi edinmek için, [hizmet tarafından başlatılan yedeklemelerden veritabanını geri yükleme'ye](sql-database-recovery-using-backups.md)bakın.
- Yeni bir birincil sunucu ve veritabanı için kimlik doğrulama gereksinimleri hakkında bilgi edinmek [için, olağanüstü durum kurtarma sonra SQL Veritabanı güvenliği](sql-database-geo-replication-security-config.md)bakın.
