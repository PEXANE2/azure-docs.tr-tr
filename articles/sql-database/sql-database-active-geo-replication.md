---
title: Etkin coğrafi çoğaltma
description: Aynı veya farklı veri merkezinde (bölge) tek tek veritabanlarının okunabilir ikincil veritabanlarını oluşturmak için etkin coğrafi çoğaltmayı kullanın.
services: sql-database
ms.service: sql-database
ms.subservice: high-availability
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: mathoma, carlrab
ms.date: 02/17/2020
ms.openlocfilehash: b80b58d64ea27df95c2704243d8a89fa6ca12e2a
ms.sourcegitcommit: 980c3d827cc0f25b94b1eb93fd3d9041f3593036
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80548501"
---
# <a name="creating-and-using-active-geo-replication"></a>Etkin coğrafi çoğaltma oluşturma ve kullanma

Etkin coğrafi çoğaltma, aynı veya farklı veri merkezinde (bölge) bir SQL Veritabanı sunucusunda tek tek veritabanlarının okunabilir ikincil veritabanlarıoluşturmanızı sağlayan bir Azure SQL Veritabanı özelliğidir.

> [!NOTE]
> Etkin coğrafi çoğaltma yönetilen örnek tarafından desteklenmez. Yönetilen örneklerin coğrafi olarak başarısız lığı için [Otomatik hata grupları](sql-database-auto-failover-group.md)kullanın.

Etkin coğrafi çoğaltma, uygulamanın bölgesel bir felaket veya büyük ölçekli bir kesinti durumunda tek tek veritabanlarının hızlı felaket kurtarma işlemini gerçekleştirmesine olanak tanıyan bir iş sürekliliği çözümü olarak tasarlanmıştır. Coğrafi çoğaltma etkinleştirilmişse, uygulama farklı bir Azure bölgesinde ikincil bir veritabanında başarısız olmasını sağlayabilir. Aynı veya farklı bölgelerde en fazla dört saniye desteklenir ve ikinciler salt okunur erişim sorguları için de kullanılabilir. Başarısız uygulama veya kullanıcı tarafından el ile başlatılmalıdır. Başarısız olduktan sonra, yeni birincil farklı bir bağlantı bitiş noktası vardır. Aşağıdaki diyagram, Etkin coğrafi çoğaltma kullanarak coğrafi yedekli bulut uygulamasının tipik bir yapılandırmasını göstermektedir.

![etkin coğrafi çoğaltma](./media/sql-database-active-geo-replication/geo-replication.png )

> [!IMPORTANT]
> SQL Veritabanı da otomatik arıza grupları destekler. Daha fazla bilgi için otomatik [hata grupları](sql-database-auto-failover-group.md)nın kullanılmasına bakın. Ayrıca, etkin coğrafi çoğaltma Yönetilen Örnek içinde oluşturulan veritabanları için desteklenmez. Yönetilen Örneklerile [başarısız grupları](sql-database-auto-failover-group.md) kullanmayı düşünün.

Birincil veritabanınız herhangi bir nedenle başarısız olursa veya yalnızca çevrimdışı duruma alınması gerekiyorsa, ikincil veritabanlarınızdan herhangi birinde başarısız olma nızı başlatabilirsiniz. Başarısız lık ikincil veritabanlarından birine etkinleştirildiğinde, diğer tüm ikincil ler otomatik olarak yeni birincil veritabanına bağlanır.

Etkin coğrafi çoğaltmayı kullanarak tek bir veritabanının veya bir sunucuda veya elastik bir havuzda veritabanlarının çoğaltılması ve başarısız hale getirilebilir. Bunu şu ları kullanarak yapabilirsiniz:

- [Azure portalı](sql-database-geo-replication-portal.md)
- [PowerShell: Tek veritabanı](scripts/sql-database-setup-geodr-and-failover-database-powershell.md)
- [PowerShell: Elastik havuz](scripts/sql-database-setup-geodr-and-failover-pool-powershell.md)
- [Transact-SQL: Tek veritabanı veya elastik havuz](/sql/t-sql/statements/alter-database-azure-sql-database)
- [REST API: Tek veritabanı](https://docs.microsoft.com/rest/api/sql/replicationlinks)


Etkin coğrafi çoğaltma, SQL Server'ın [Always On](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server) teknolojisini kullanarak birincil veritabanında işlenen hareketleri anlık görüntü yalıtımını kullanarak ikincil bir veritabanına eşit bir şekilde çoğaltmak için kullanır. Otomatik hata grupları, etkin coğrafi çoğaltmanın üstüne grup semantiklerini sağlar, ancak aynı eşzamanlı çoğaltma mekanizması kullanılır. Herhangi bir noktada, ikincil veritabanı birincil veritabanının biraz gerisinde olabilir, ikincil veri kısmi işlemler asla garanti edilir. Bölgeler arası artıklık, uygulamaların doğal afetler, yıkıcı insan hataları veya kötü amaçlı eylemler nedeniyle tüm veri merkezinin veya veri merkezinin parçalarının kalıcı kaybından hızla kurtulmasını sağlar. Belirli RPO verileri İş [Sürekliliğine Genel Bakış'ta](sql-database-business-continuity.md)bulunabilir.

> [!NOTE]
> İki bölge arasında bir ağ hatası varsa, bağlantıları yeniden kurmak için her 10 saniyede bir yeniden deneriz.
> [!IMPORTANT]
> Birincil veritabanındaki kritik değişikliğin, başarısız olmadan önce ikincil bir veritabanına çoğaltılmasını garanti etmek için, kritik değişikliklerin (örneğin parola güncelleştirmeleri) çoğaltılmasını sağlamak için eşitlemesini zorlayabilirsiniz. Zorla eşitleme, tüm işlenen hareketler çoğaltılana kadar çağrı iş parçacığının engellediği için performansı etkiler. Ayrıntılar için [sp_wait_for_database_copy_sync.](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/active-geo-replication-sp-wait-for-database-copy-sync) Birincil veritabanı ve jeo-ikincil arasındaki çoğaltma gecikmesini izlemek için [bkz dm_geo_replication_link_status.](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-geo-replication-link-status-azure-sql-database)

Aşağıdaki şekil, Kuzey Orta ABD bölgesinde bir birincil ve Güney Orta ABD bölgesinde ikincil ile yapılandırılan etkin coğrafi çoğaltma bir örnek gösterir.

![coğrafi çoğaltma ilişkisi](./media/sql-database-active-geo-replication/geo-replication-relationship.png)

İkincil veritabanları okunabilir olduğundan, iş bildirme gibi salt okunur iş yüklerini boşaltmak için kullanılabilirler. Etkin coğrafi çoğaltma kullanıyorsanız, birincil ile aynı bölgede ikincil veritabanı oluşturmak mümkündür, ancak felaket hataları için uygulamanın esnekliğini artırmaz. Otomatik hata grupları kullanıyorsanız, ikincil veritabanınız her zaman farklı bir bölgede oluşturulur.

Olağanüstü durum kurtarma etkin coğrafi çoğaltma ek olarak aşağıdaki senaryolarda kullanılabilir:

- **Veritabanı geçişi**: Bir veritabanını en az kapalı kalma süresiyle bir sunucudan başka bir çevrimiçiye geçirmek için etkin coğrafi çoğaltmayı kullanabilirsiniz.
- **Uygulama yükseltmeleri**: Uygulama yükseltmeleri sırasında başarısız bir geri kopya olarak ekstra bir ikincil oluşturabilirsiniz.

Gerçek iş sürekliliği elde etmek için, veri merkezleri arasında veritabanı artıklığı eklemek çözümün yalnızca bir parçasıdır. Bir uygulamanın (hizmetin) felaket bir hatadan sonra uçtan uca kurtarılması, hizmeti oluşturan tüm bileşenlerin ve bağımlı hizmetlerin kurtarılmasını gerektirir. Bu bileşenlere örnek olarak istemci yazılımı (örneğin, özel JavaScript'li bir tarayıcı), web ön uçları, depolama ve DNS verilebilir. Tüm bileşenlerin aynı hatalara karşı dirençli olması ve uygulamanızın kurtarma süresi hedefi (RTO) içinde kullanılabilir hale gelmesi çok önemlidir. Bu nedenle, tüm bağımlı hizmetleri belirlemeniz ve sağladıkları garantileri ve yetenekleri anlamanız gerekir. Daha sonra, hizmetinizin bağlı olduğu hizmetlerin başarısız lığı sırasında çalışmasını sağlamak için yeterli adımları atmalısınız. Olağanüstü durum kurtarma çözümleri tasarımı hakkında daha fazla bilgi için bkz: [Etkin coğrafi çoğaltmayı kullanarak Olağanüstü Durum Kurtarma için Bulut Çözümleri Tasarlama.](sql-database-designing-cloud-solutions-for-disaster-recovery.md)

## <a name="active-geo-replication-terminology-and-capabilities"></a>Aktif jeo-replikasyon terminolojisi ve yetenekleri

- **Otomatik Asynchronous Çoğaltma**

  Yalnızca varolan bir veritabanına ekleyerek ikincil bir veritabanı oluşturabilirsiniz. İkincil herhangi bir Azure SQL Veritabanı sunucusunda oluşturulabilir. Oluşturulduktan sonra, ikincil veritabanı birincil veritabanından kopyalanan verilerle doldurulur. Bu işlem tohumlama olarak bilinir. İkincil veritabanı oluşturulduktan ve tohumlandıktan sonra, birincil veritabanındaki güncelleştirmeler otomatik olarak ikincil veritabanına eşit olarak çoğaltılır. Asynchronous çoğaltma, hareketlerin ikincil veritabanına çoğaltılmadan önce birincil veritabanında işlendiği anlamına gelir.

- **Okunabilir ikincil veritabanları**

  Uygulama, birincil veritabanına erişmek için kullanılan aynı veya farklı güvenlik ilkelerini kullanarak salt okunur işlemler için ikincil bir veritabanına erişebilir. İkincil veritabanları, birincil (günlük tekrarı) güncelleştirmelerinin çoğaltılmasını sağlamak için anlık görüntü yalıtım modunda çalışır, ikincil de yürütülen sorgular tarafından geciktirilmez.

> [!NOTE]
> Birincil şema güncelleştirmeleri varsa günlük yeniden oynatma ikincil veritabanında geciktirilir. İkincisi ikincil veritabanında bir şema kilidi gerektirir.
> [!IMPORTANT]
> Birincil olarak aynı bölgede ikincil bir veritabanı oluşturmak için coğrafi çoğaltma kullanabilirsiniz. Bu ikincil yükü aynı bölgede salt okunur iş yüklerini dengelemek için kullanabilirsiniz. Ancak, aynı bölgedeki ikincil bir veritabanı ek hata esnekliği sağlamaz ve bu nedenle olağanüstü durum kurtarma için uygun bir hata üzerinde hedef değildir. Ayrıca kullanılabilirlik bölgesi yalıtımı garanti etmez. Kullanılabilirlik bölgesi yalıtımı elde etmek için [bölge yedekli yapılandırmasıyla](sql-database-high-availability.md#zone-redundant-configuration) İş kritik veya Premium hizmet katmanını kullanın.   
>

- **Planlı yük devretme**

  Planlı hata, tam eşitleme tamamlandıktan sonra birincil ve ikincil veritabanlarının rollerini değiştirir. Bu veri kaybına neden olmayan bir çevrimiçi işlemdir. İşlemin süresi, eşitlenmesi gereken birincil işlem günlüğünün boyutuna bağlıdır. Planlanan hata aşağıdaki senaryolar için tasarlanmıştır: (a) veri kaybı kabul edilemez olduğunda üretimde DR matkapları gerçekleştirmek için; (b) veritabanını farklı bir bölgeye taşımak için; ve (c) kesinti azaltıldıktan sonra veritabanını birincil bölgeye döndürmek (failback).

- **Planlanmamış yük devretme**

  Planlanmamış veya zorlanmış başarısız, birincil rol ile herhangi bir senkronizasyon olmadan hemen birincil role ikincil anahtarları. Birincil taahhüt edilen ancak ikincil çoğaltılmayan tüm hareketler kaybolur. Bu işlem, birincil erişilebilir değil kesintileri sırasında bir kurtarma yöntemi olarak tasarlanmıştır, ancak veritabanı kullanılabilirliği hızla geri yüklenmelidir. Orijinal birincil yeniden çevrimiçi olduğunda otomatik olarak yeniden bağlanır ve yeni bir ikincil olur. Başarısız olmadan önce eşitlenmemiş tüm hareketler yedekleme dosyasında korunur, ancak çakışmaları önlemek için yeni birincil ile eşitlenmez. Bu hareketlerin birincil veritabanının en son sürümüyle el ile birleştirilmesi gerekir.
 
- **Birden çok okunabilir ikinciller**

  Her birincil için en fazla 4 ikincil veritabanları oluşturulabilir. Yalnızca bir ikincil veritabanı varsa ve başarısız olursa, yeni bir ikincil veritabanı oluşturulana kadar uygulama daha yüksek risklere maruz kalır. Birden çok ikincil veritabanları varsa, ikincil veritabanlarından biri başarısız olsa bile uygulama korumalı kalır. Ek ikinciler, salt okunur iş yüklerini ölçeklendirmek için de kullanılabilir

  > [!NOTE]
  > Genel olarak dağıtılmış bir uygulama oluşturmak için etkin coğrafi çoğaltma kullanıyorsanız ve dörtten fazla bölgede salt okunur verilere erişim sağlamanız gerekiyorsa, ikincil (zincirleme olarak bilinen bir işlem) ikincil bir işlem oluşturabilirsiniz. Bu şekilde veritabanı çoğaltma neredeyse sınırsız ölçekte elde edebilirsiniz. Buna ek olarak, zincirleme birincil veritabanından çoğaltma yükü azaltır. Dengeleme, yaprak en ikincil veritabanlarında artan çoğaltma gecikmesi.

- **Elastik bir havuzda veritabanlarının coğrafi çoğaltılması**

  Her ikincil veritabanı ayrı ayrı bir elastik havuza katılabilir veya herhangi bir elastik havuzda olamaz. Her ikincil veritabanı için havuz seçimi ayrıdır ve başka bir ikincil veritabanının yapılandırmasına (birincil veya ikincil olsun) bağlı değildir. Her elastik havuz tek bir bölge içinde bulunur, bu nedenle aynı topolojideki birden çok ikincil veritabanları asla elastik bir havuzu paylaşamaz.


- **Kullanıcı kontrollü failover ve failback**

  İkincil bir veritabanı, uygulama veya kullanıcı tarafından herhangi bir zamanda birincil role açıkça geçilebilir. Gerçek bir kesinti sırasında "planlanmamış" seçeneği kullanılmalıdır, hangi hemen birincil olarak ikincil teşvik. Başarısız birincil kurtarılır ve yeniden kullanılabilir olduğunda, sistem otomatik olarak kurtarılan birincil ikincil olarak işaretler ve yeni birincil ile güncel getirmek. Çoğaltmanın eşzamanlı yapısı nedeniyle, birincil bir birincil ikincil en son değişiklikleri çoğaltmadan önce başarısız olursa, planlanmamış arızalar sırasında az miktarda veri kaybedilebilir. Birden çok ikincilik içeren birincil bir birincil başarısız olduğunda, sistem çoğaltma ilişkilerini otomatik olarak yeniden yapılandırır ve kalan ikincileri herhangi bir kullanıcı müdahalesi gerektirmeden yeni tanıtılan birincil e bağlar. Başarısıza neden olan kesinti azaltıldıktan sonra, uygulamayı birincil bölgeye döndürmek istenebilir. Bunu yapmak için, failover komutu "planlanmış" seçeneği ile çağrılmalıdır.

## <a name="preparing-secondary-database-for-failover"></a>Başarısız olmaya ikincil veritabanı hazırlama

Uygulamanızın başarısız olduktan sonra yeni birincil ana maddeye hemen erişebilmesini sağlamak için, ikincil sunucunuz ve veritabanınız için kimlik doğrulama gereksinimlerinin düzgün şekilde yapılandırıldığından emin olun. Ayrıntılar [için, olağanüstü durum kurtarma sonrası SQL Veritabanı güvenliğine](sql-database-geo-replication-security-config.md)bakın. Başarısız olduktan sonra uyumluluğu garanti etmek için, ikincil veritabanındaki yedekleme bekletme ilkesinin birincil veritabanıyla eşleştiğinden emin olun. Bu ayarlar veritabanının bir parçası değildir ve çoğaltılamaz. Varsayılan olarak, ikincil yedi günlük varsayılan PITR bekletme süresi ile yapılandırılır. Ayrıntılar için [SQL Veritabanı otomatik yedeklemelerine](sql-database-automated-backups.md)bakın.

> [!IMPORTANT]
> Veritabanınız bir başarısız grubun üyesiyse, coğrafi çoğaltma faiover komutunu kullanarak bu grubun başarısızlığını başlatamazsınız. Grup için failover komutunu kullanmayı düşünün. Tek bir veritabanı üzerinde başarısız olması gerekiyorsa, önce failover grubundan kaldırmanız gerekir. Ayrıntılar için [başarısız gruplara](sql-database-auto-failover-group.md) bakın. 


## <a name="configuring-secondary-database"></a>İkincil veritabanını yapılandırma

Hem birincil hem de ikincil veritabanlarının aynı hizmet katmanına sahip olması gerekir. İkincil veritabanının birincil veritabanıyla aynı işlem boyutuyla (DTUs veya vCores) oluşturulması da önemle önerilir. Birincil veritabanında ağır bir yazma iş yükü yaşıyorsa, daha düşük işlem boyutuna sahip ikincil bir veritabanı bu veritabanına ayak uyduramayabilir. Bu ikincil ve potansiyel kullanılamazlık üzerinde redo gecikme neden olur. Birincil arkasında kalan ikincil bir veritabanı da zorunlu bir hata gerekli olması halinde büyük bir veri kaybı riskleri. Bu riskleri azaltmak için, etkin etkin coğrafi çoğaltma birincil günlük oranını azaltacak ve ikincilerinin yetişmesine izin verir. Dengesiz bir ikincil yapılandırmanın diğer sonucu, uygulamanın performansının başarısız lığından sonra yeni birincil in yetersiz bilgi işlem kapasitesi nedeniyle zarar görecek olmasıdır. Kesinti azaltılına kadar mümkün olmayacak olan, gerekli seviyeye daha yüksek bir bilgi işlem için yükseltmek için gerekli olacaktır. 


> [!IMPORTANT]
> İkincil veritabanı birincil le aynı işlem boyutuyla yapılandırılmadığı sürece yayınlanan RPO = 5 sn garanti edilemez. 


Daha düşük işlem boyutuna sahip ikincil boyutu oluşturmaya karar verirseniz, Azure portalındaki günlük IO yüzdesi grafiği, çoğaltma yükünü sürdürmek için gereken ikincil işlemin en az işlem boyutunu tahmin etmek için iyi bir yol sağlar. Örneğin, Birincil veritabanınız P6 (1000 DTU) ise ve günlük IO yüzdesi %50 ikincil olması gereken en az P4 (500 DTU) ise. Ayrıca [sys.resource_stats veya sys.dm_db_resource_stats](/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database) [sys.dm_db_resource_stats](/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database) veritabanı görünümlerini kullanarak günlük IO verilerini de alabilirsiniz.  Azaltma, [sys.dm_exec_requests](/sql/relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql) ve [sys.dm_os_wait_stats](/sql/relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql) veritabanı görünümlerinde HADR_THROTTLE_LOG_RATE_MISMATCHED_SLO bekleme durumu olarak bildirilir. 

SQL Veritabanı bilgi işlem boyutları hakkında daha fazla bilgi için SQL [Veritabanı Hizmet Katmanları nelerdir'e](sql-database-purchase-models.md)bakın.

## <a name="cross-subscription-geo-replication"></a>Çapraz abonelik coğrafi çoğaltma

Farklı aboneliklere ait iki veritabanı (aynı kiracı altında olsun veya olmasın) arasında etkin coğrafi çoğaltma kurmak için, bu bölümde açıklanan özel yordamı izlemeniz gerekir.  Yordam SQL komutlarına dayanır ve şunları gerektirir: 

- Her iki sunucuda da ayrıcalıklı bir oturum açma oluşturma
- IP adresini her iki sunucuda da değişikliği gerçekleştiren istemcinin izin listesine ekleme (SQL Server Management Studio'yu çalıştıran ana bilgisayar sahibinin IP adresi gibi). 

Değişiklikleri gerçekleştiren istemcinin birincil sunucuya ağ erişimi gerekir. İstemcinin aynı IP adresi ikincil sunucudaki izin listesine eklenmesi gerekse de, ikincil sunucuya ağ bağlantısı kesinlikle gerekli değildir. 

### <a name="on-the-master-of-the-primary-server"></a>Birincil sunucunun ana üzerinde

1. IP adresini değişiklikleri gerçekleştiren istemcinin izin listesine ekleyin (daha fazla bilgi için bkz. [güvenlik duvarını yapılandır).](sql-database-firewall-configure.md) 
1. Kurulum etkin coğrafi çoğaltma adanmış bir oturum açma oluşturma (ve gerektiğinde kimlik bilgilerini ayarlamak):

   ```sql
   create login geodrsetup with password = 'ComplexPassword01'
   ```

1. Karşılık gelen bir kullanıcı oluşturun ve dbmanager rolüne atayın: 

   ```sql
   create user geodrsetup for login geodrsetup
   alter role dbmanager add member geodrsetup
   ```

1. Bu sorguyu kullanarak yeni girişin SID'sini dikkate alın: 

   ```sql
   select sid from sys.sql_logins where name = 'geodrsetup'
   ```

### <a name="on-the-source-database-on-the-primary-server"></a>Birincil sunucudaki kaynak veritabanında

1. Aynı oturum açma için bir kullanıcı oluşturun:

   ```sql
   create user geodrsetup for login geodrsetup
   ```

1. Kullanıcıyı db_owner rolüne ekleyin:

   ```sql
   alter role db_owner add member geodrsetup
   ```

### <a name="on-the-master-of-the-secondary-server"></a>İkincil sunucunun ana üzerinde 

1. IP adresini değişiklikleri gerçekleştiren istemcinin izin listesine ekleyin. Birincil sunucunun aynı IP adresi olmalıdır. 
1. Aynı kullanıcı adı parolasını kullanarak birincil sunucuda aynı girişi ve SID'yi oluşturun: 

   ```sql
   create login geodrsetup with password = 'ComplexPassword01', sid=0x010600000000006400000000000000001C98F52B95D9C84BBBA8578FACE37C3E
   ```

1. Karşılık gelen bir kullanıcı oluşturun ve dbmanager rolüne atayın:

   ```sql
   create user geodrsetup for login geodrsetup;
   alter role dbmanager add member geodrsetup
   ```

### <a name="on-the-master-of-the-primary-server"></a>Birincil sunucunun ana üzerinde

1. Yeni girişi kullanarak birincil sunucunun yöneticisine giriş yapın. 
1. İkincil sunucuda kaynak veritabanının ikincil bir kopyasını oluşturun (veritabanı adını ve sunucu adını gerektiği gibi ayarlayın):

   ```sql
   alter database dbrep add secondary on server <servername>
   ```

İlk kurulumdan sonra oluşturulan kullanıcılar, oturum açmalar ve güvenlik duvarı kuralları kaldırılabilir. 


## <a name="keeping-credentials-and-firewall-rules-in-sync"></a>Kimlik bilgilerini ve güvenlik duvarı kurallarını eşit tutma

Bu kuralların tüm ikincil veritabanlarının birincil veritabanlarıyla aynı IP güvenlik duvarı kurallarına sahip olmasını sağlamak için veritabanıyla çoğaltılabilmesi için, coğrafi olarak çoğaltılan veritabanları için [veritabanı düzeyinde IP güvenlik duvarı kuralları](sql-database-firewall-configure.md) kullanmanızı öneririz. Bu yaklaşım, müşterilerin hem birincil hem de ikincil veritabanlarını barındıran sunucularda güvenlik duvarı kurallarını el ile yapılandırma ve koruma gereksinimini ortadan kaldırır. Benzer şekilde, veri erişimi için [içerdiği veritabanı kullanıcılarının](sql-database-manage-logins.md) kullanılması, hem birincil hem de ikincil veritabanlarının her zaman aynı kullanıcı kimlik bilgilerine sahip olmasını sağlar, bu nedenle bir hata sırasında, oturum açma ve parolalarla uyumsuzluklar nedeniyle herhangi bir aksaklık yaşanmaz. Azure Active [Directory'nin](../active-directory/fundamentals/active-directory-whatis.md)eklenmesiyle müşteriler, hem birincil hem de ikincil veritabanlarına kullanıcı erişimini yönetebilir ve veritabanlarındaki kimlik bilgilerini yönetme gereksinimini tamamen ortadan kaldırabilir.

## <a name="upgrading-or-downgrading-primary-database"></a>Birincil veritabanını yükseltme veya düşürme

Birincil veritabanını, ikincil veritabanlarını kesmeden farklı bir işlem boyutuna (Genel Amaç ve İş Açısından Genel Olarak değil, aynı hizmet katmanı içinde) yükseltebilir veya düşürebilirsiniz. Yükseltme yaparken, önce ikincil veritabanını yükseltmenizi ve ardından birincil veritabanını yükseltmenizi öneririz. Düşürme yaparken, sırayı tersine çevirin: önce birincil önce ve sonra ikincil denemi düşürün. Veritabanını farklı bir hizmet katmanına yükselttidiğinizde veya düşürdüğünüzde, bu öneri uygulanır.

> [!NOTE]
> Başarısız grup yapılandırmasının bir parçası olarak ikincil veritabanı oluşturduysanız, ikincil veritabanını düşürmeniz önerilmez. Bu, veri katmanınızın, başarısız olduktan sonra düzenli iş yükünüzü işlemek için yeterli kapasiteye sahip olmasını sağlamak tır.

> [!IMPORTANT]
> Bir başarısız gruptaki birincil veritabanı, ikincil veritabanı önce daha yüksek katmana ölçeklendirildikçe daha yüksek bir katmana ölçeklendiremez. İkincil veritabanı ölçeklendirilmeden önce birincil veritabanını ölçeklendirmeye çalışırsanız, aşağıdaki hatayı alabilirsiniz:
>
> `Error message: The source database 'Primaryserver.DBName' cannot have higher edition than the target database 'Secondaryserver.DBName'. Upgrade the edition on the target before upgrading the source.`
>

## <a name="preventing-the-loss-of-critical-data"></a>Kritik verilerin kaybını önleme

Geniş alan ağlarının yüksek gecikme sayılması nedeniyle, sürekli kopya bir eşzamanlı çoğaltma mekanizması kullanır. Asynchronous çoğaltma, bir hata oluşursa bazı veri kaybını kaçınılmaz hale getirir. Ancak, bazı uygulamalar veri kaybı gerektirebilir. Bu kritik güncelleştirmeleri korumak için, bir uygulama geliştiricisi işlemi yaptıktan hemen sonra [sp_wait_for_database_copy_sync](/sql/relational-databases/system-stored-procedures/active-geo-replication-sp-wait-for-database-copy-sync) sistem yordamını arayabilir. **Çağrı sp_wait_for_database_copy_sync,** son işlenen işlem ikincil veritabanına iletene kadar çağrı iş parçacığının engellenmesini engeller. Ancak, aktarılan hareketlerin ikincil işlemde yeniden oynatılması ve işlenmesini beklemez. **sp_wait_for_database_copy_sync** belirli bir sürekli kopyalama bağlantısına göre dir. Birincil veritabanına bağlantı haklarına sahip herhangi bir kullanıcı bu yordamı arayabilir.

> [!NOTE]
> **sp_wait_for_database_copy_sync,** başarısız olduktan sonra veri kaybını önler, ancak okuma erişimi için tam eşitleme garantisi vermez. **sp_wait_for_database_copy_sync** yordam çağrısının neden olduğu gecikme önemli olabilir ve arama sırasındaki hareket günlüğünün boyutuna bağlıdır.

## <a name="monitoring-geo-replication-lag"></a>Coğrafi çoğaltma gecikmesi izleme

RPO ile ilgili gecikmeyi izlemek için birincil veritabanında [sys.dm_geo_replication_link_status](/sql/relational-databases/system-dynamic-management-views/sys-dm-geo-replication-link-status-azure-sql-database) *replication_lag_sec* sütunkullanın. Birincil ve ikincil üzerinde devam eden işlemler arasında saniye cinsinden gecikme gösterir. Örneğin gecikmenin değeri 1 saniye ise, birincil şu anda bir kesintiden etkilenir ve başarısızlık başlatılırsa, en son geçişlerin 1 saniyesi kaydedilmez. 

İkincil veritabanında uygulanan, yani ikincil veritabanından okunabilen değişikliklere ilişkin gecikmeyi ölçmek için, ikincil veritabanındaki *last_commit* süreyi birincil veritabanındaki aynı değerle karşılaştırın.

> [!NOTE]
> Bazen birincil veritabanında *replication_lag_sec* bir NULL değeri vardır, bu da birincil in şu anda ikincil ne kadar olduğunu bilmediği anlamına gelir.   Bu genellikle işlem yeniden başlatıldıktan sonra olur ve geçici bir durum olmalıdır. *replication_lag_sec* uzun bir süre null döndürürse uygulamayı uyarmayı düşünün. Bu, ikincil veritabanının kalıcı bir bağlantı hatası nedeniyle birincil veritabanıyla iletişim kuramayacağını gösterir. İkincil ve birincil veritabanında *last_commit* zaman arasındaki farkın büyük olmasına neden olabilecek koşullar da vardır. Örneğin uzun bir değişiklik ten sonra birincil bir taahhüt yapılırsa, fark hızla 0'a dönmeden önce büyük bir değere fırlar. Bu iki değer arasındaki fark uzun süre büyük kaldığında bunu bir hata koşulu olarak düşünün.


## <a name="programmatically-managing-active-geo-replication"></a>Etkin coğrafi çoğaltmayı programlı olarak yönetme

Daha önce de tartışıldığı gibi, etkin coğrafi çoğaltma Azure PowerShell ve REST API kullanılarak programlı olarak da yönetilebilir. Aşağıdaki tablolarda kullanılabilir komutkümesi açıklayınız. Etkin coğrafi çoğaltma, Azure SQL Database REST API ve [Azure PowerShell cmdlets](https://docs.microsoft.com/powershell/azure/overview)dahil olmak üzere yönetim için bir dizi Azure Kaynak Yöneticisi [API'sini](https://docs.microsoft.com/rest/api/sql/) içerir. Bu API'ler kaynak gruplarının kullanımını ve rol tabanlı güvenliği (RBAC) desteklemeyi gerektirir. Erişim rollerinin nasıl uygulanacağı hakkında daha fazla bilgi için [Azure Role Tabanlı Erişim Denetimi'ne](../role-based-access-control/overview.md)bakın.

### <a name="t-sql-manage-failover-of-single-and-pooled-databases"></a>T-SQL: Tek ve havuzlu veritabanlarının başarısız larını yönetme

> [!IMPORTANT]
> Bu Transact-SQL komutları yalnızca etkin coğrafi çoğaltma için geçerlidir ve başarısız gruplar için geçerli değildir. Bu nedenle, yalnızca başarısız grupları destekledikleri için Yönetilen Örnekler için de geçerli değildir.

| Komut | Açıklama |
| --- | --- |
| [VERITABANıNı DEĞIŞTIR](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-current) |Varolan bir veritabanı için ikincil bir veritabanı oluşturmak için ADD SECONDARY ON SERVER bağımsız değişkenini kullanın ve veri çoğaltmayı başlatır |
| [VERITABANıNı DEĞIŞTIR](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-current) |Failover'ı başlatmak için birincil olacak ikincil bir veritabanını değiştirmek için FAILOVER veya FORCE_FAILOVER_ALLOW_DATA_LOSS kullanın |
| [VERITABANıNı DEĞIŞTIR](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-current) |SQL Veritabanı ile belirtilen ikincil veritabanı arasındaki veri çoğaltmaişlemini sonlandırmak için REMOVE SECONDARY ON SERVER'ı kullanın. |
| [sys.geo_replication_links](/sql/relational-databases/system-dynamic-management-views/sys-geo-replication-links-azure-sql-database) |Azure SQL Veritabanı sunucusundaki her veritabanı için varolan tüm çoğaltma bağlantıları hakkında bilgi verir. |
| [sys.dm_geo_replication_link_status](/sql/relational-databases/system-dynamic-management-views/sys-dm-geo-replication-link-status-azure-sql-database) |Belirli bir SQL veritabanıiçin çoğaltma bağlantısı yla ilgili son çoğaltma süresini, son çoğaltma gecikmesini ve diğer bilgileri alır. |
| [sys.dm_operation_status](/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database) |Çoğaltma bağlantılarının durumu da dahil olmak üzere tüm veritabanı işlemlerinin durumunu gösterir. |
| [sp_wait_for_database_copy_sync](/sql/relational-databases/system-stored-procedures/active-geo-replication-sp-wait-for-database-copy-sync) |tüm taahhüt edilen hareketler çoğaltılır ve etkin ikincil veritabanı tarafından kabul kadar beklemek uygulama neden olur. |
|  | |

### <a name="powershell-manage-failover-of-single-and-pooled-databases"></a>PowerShell: Tek ve havuzlu veritabanlarının başarısız lığı

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> PowerShell Azure Kaynak Yöneticisi modülü hala Azure SQL Veritabanı tarafından desteklenir, ancak gelecekteki tüm geliştirme az.sql modülü içindir. Bu cmdlets için [AzureRM.Sql](https://docs.microsoft.com/powershell/module/AzureRM.Sql/)bakın. Az modülündeki ve AzureRm modüllerinde bulunan komutların bağımsız değişkenleri önemli ölçüde aynıdır.

| Cmdlet | Açıklama |
| --- | --- |
| [Get-AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldatabase) |Bir veya daha fazla veritabanını alır. |
| [Yeni-AzSqlDatabaseSecondary](https://docs.microsoft.com/powershell/module/az.sql/new-azsqldatabasesecondary) |Mevcut bir veritabanı için ikincil bir veritabanı oluşturur ve veri çoğaltmaya başlar. |
| [Set-AzSqlDatabaseSecondary](https://docs.microsoft.com/powershell/module/az.sql/set-azsqldatabasesecondary) |Yük devretmeyi başlatmak için ikincil bir veritabanını birincil olarak değiştirir. |
| [Remove-AzSqlDatabaseSecondary](https://docs.microsoft.com/powershell/module/az.sql/remove-azsqldatabasesecondary) |Bir SQL Veritabanı ile belirtilen ikincil veritabanı arasında veri çoğaltmayı sonlandırır. |
| [Get-AzSqlDatabaseReplicationLink](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldatabasereplicationlink) |Bir Azure SQL Veritabanı ve bir kaynak grubu veya SQL Server arasındaki coğrafi çoğaltma bağlantılarını alır. |
|  | |

> [!IMPORTANT]
> Örnek komut dosyaları için, etkin coğrafi çoğaltma ve yapılandırma ve [etkin coğrafi çoğaltma kullanarak havuzlu bir veritabanı üzerinde başarısız](scripts/sql-database-setup-geodr-and-failover-pool-powershell.md)kullanarak tek bir veritabanı üzerinde Yapılandırma ve [başarısız](scripts/sql-database-setup-geodr-and-failover-database-powershell.md) bakın.

### <a name="rest-api-manage-failover-of-single-and-pooled-databases"></a>REST API: Tek ve havuzlu veritabanlarının başarısız larını yönetme

| API | Açıklama |
| --- | --- |
| [Veritabanı Oluştur veya Güncelleştir (createMode=Geri Yükle)](https://docs.microsoft.com/rest/api/sql/databases/createorupdate) |Birincil veya ikincil bir veritabanı oluşturur, güncelleştirir veya geri yükler. |
| [Veritabanı Durumu Oluştur veya Güncelleştir](https://docs.microsoft.com/rest/api/sql/databases/createorupdate) |Oluşturma işlemi sırasında durumu döndürür. |
| [İkincil Veritabanını Birincil Olarak Ayarlama (Planlı Failover)](https://docs.microsoft.com/rest/api/sql/replicationlinks/failover) |Geçerli birincil veritabanından başarısız olarak hangi ikincil veritabanının birincil olduğunu ayarlar. **Yönetilen Örnek için bu seçenek desteklenmez.**|
| [İkincil Veritabanını Birincil Olarak Ayarlama (Planlanmamış Failover)](https://docs.microsoft.com/rest/api/sql/replicationlinks/failoverallowdataloss) |Geçerli birincil veritabanından başarısız olarak hangi ikincil veritabanının birincil olduğunu ayarlar. Bu işlem veri kaybına neden olabilir. **Yönetilen Örnek için bu seçenek desteklenmez.**|
| [Çoğaltma Bağlantısı Al](https://docs.microsoft.com/rest/api/sql/replicationlinks/get) |Bir coğrafi çoğaltma ortaklığında belirli bir SQL veritabanı için belirli bir çoğaltma bağlantısı alır. Sys.geo_replication_links kataloğu görünümünde görünen bilgileri alır. **Yönetilen Örnek için bu seçenek desteklenmez.**|
| [Çoğaltma Bağlantıları - Veritabanına Göre Listele](https://docs.microsoft.com/rest/api/sql/replicationlinks/listbydatabase) | Belirli bir SQL veritabanı için tüm çoğaltma bağlantılarını bir coğrafi çoğaltma ortaklığında alır. Sys.geo_replication_links kataloğu görünümünde görünen bilgileri alır. |
| [Çoğaltma Bağlantısını Sil](https://docs.microsoft.com/rest/api/sql/replicationlinks/delete) | Veritabanı çoğaltma bağlantısını siler. Başarısız lık sırasında yapılamaz. |
|  | |

## <a name="next-steps"></a>Sonraki adımlar

- Örnek komut dosyaları için bkz:
  - [Etkin coğrafi çoğaltmayı kullanarak tek bir veritabanını yapılandırma ve tek bir veritabanının yükünü devretme](scripts/sql-database-setup-geodr-and-failover-database-powershell.md)
  - [Etkin coğrafi çoğaltmayı kullanarak havuza alınan veritabanını yapılandırma ve havuza alınmış veritabanının yükünü devretme](scripts/sql-database-setup-geodr-and-failover-pool-powershell.md)
- SQL Veritabanı da otomatik arıza grupları destekler. Daha fazla bilgi için otomatik [hata grupları](sql-database-auto-failover-group.md)nın kullanılmasına bakın.
- İş sürekliliğine genel bakış ve senaryolar için [bkz.](sql-database-business-continuity.md)
- Azure SQL Veritabanı otomatik yedeklemeleri hakkında bilgi edinmek için [SQL Veritabanı otomatik yedeklemelerine](sql-database-automated-backups.md)bakın.
- Kurtarma için otomatik yedeklemekullanma hakkında bilgi edinmek için, [hizmet tarafından başlatılan yedeklemelerden veritabanını geri yükleme'ye](sql-database-recovery-using-backups.md)bakın.
- Yeni bir birincil sunucu ve veritabanı için kimlik doğrulama gereksinimleri hakkında bilgi edinmek [için, olağanüstü durum kurtarma sonra SQL Veritabanı güvenliği](sql-database-geo-replication-security-config.md)bakın.
