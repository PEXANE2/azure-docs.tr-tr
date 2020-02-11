---
title: Yük devretme grupları
description: Otomatik yük devretme grupları, bir SQL veritabanı sunucusundaki bir veritabanı grubunun veya yönetilen örnekteki tüm veritabanlarının çoğaltılmasını ve otomatik/Eşgüdümlü yük devretmesini yönetmenizi sağlayan bir SQL veritabanı özelliğidir.
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
ms.sourcegitcommit: 7c18afdaf67442eeb537ae3574670541e471463d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/11/2020
ms.locfileid: "77116198"
---
# <a name="use-auto-failover-groups-to-enable-transparent-and-coordinated-failover-of-multiple-databases"></a>Birden çok veritabanının saydam ve koordine edilmiş yük devretmesini etkinleştirmek için otomatik yük devretme gruplarını kullanın

Otomatik yük devretme grupları bir SQL veritabanı sunucusundaki bir veritabanı grubunun veya yönetilen bir örnekteki tüm veritabanlarının çoğaltma ve yük devretme özelliklerini başka bir bölgeye yönetmenizi sağlayan bir SQL veritabanı özelliğidir. Bu, var olan [etkin coğrafi çoğaltma](sql-database-active-geo-replication.md) özelliğinin en üstünde, coğrafi olarak çoğaltılan veritabanlarının dağıtım ve yönetimini kolaylaştırmak için tasarlanan, bildirime dayalı bir soyutlamadır. Yük devretmeyi el ile başlatabilir veya Kullanıcı tanımlı bir ilkeye göre SQL veritabanı hizmetine temsilci seçebilirsiniz. İkinci seçenek, bir ikincil bölgedeki birden çok ilişkili veritabanını, birincil bölgede SQL veritabanı hizmetinin kullanılabilirliğinin tamamen veya kısmen kaybedilmesiyle sonuçlanan bir veya daha fazla plansız olaydan sonra otomatik olarak kurtarmanıza olanak sağlar. Yük devretme grubu, genellikle aynı uygulama tarafından kullanılan bir veya daha fazla veritabanı içerebilir. Ayrıca, salt okunur sorgu iş yüklerini boşaltmak için okunabilir ikincil veritabanlarını da kullanabilirsiniz. Otomatik yük devretme grupları birden çok veritabanı içerdiğinden, bu veritabanlarının birincil sunucuda yapılandırılması gerekir. Otomatik yük devretme grupları, gruptaki tüm veritabanlarının farklı bir bölgedeki yalnızca bir ikincil sunucuya çoğaltılmasını destekler.

> [!NOTE]
> Bir SQL veritabanı sunucusunda tek veya havuza alınmış veritabanlarıyla çalışırken ve aynı ya da farklı bölgelerde birden çok ikincil sunucu istiyorsanız [etkin coğrafi çoğaltma](sql-database-active-geo-replication.md)'yı kullanın. 

Otomatik yük devretme grupları otomatik yük devretme ilkesiyle kullanılırken, gruptaki bir veya birkaç veritabanını etkileyen herhangi bir kesinti otomatik yük devretmeye neden olur. Bunlar genellikle yerleşik otomatik yüksek kullanılabilirlik işlemleriyle kendini hafiflede azallamayan olaylardır. Yük devretme tetikleyicilerine örnek olarak bir SQL kiracı halkasının neden olduğu bir olay veya birkaç işlem düğümündeki bir işletim sistemi çekirdek belleği sızıntısı nedeniyle bir veya daha fazla kiracı halkasının neden olduğu bir olay vardır Utine donanıma yetki alma.  Daha fazla bilgi için bkz. [SQL veritabanı yüksek kullanılabilirlik](sql-database-high-availability.md).

Bunlara ek olarak, otomatik yük devretme grupları, yük devretme sırasında değişmeden kalan okuma/yazma ve salt okuma dinleyicisi uç noktaları sağlar. El ile veya otomatik yük devretme etkinleştirme kullanmanıza bakılmaksızın, yük devretme gruptaki tüm ikincil veritabanlarını birinciye geçirir. Veritabanı yük devretmesi tamamlandıktan sonra DNS kaydı, uç noktaları yeni bölgeye yönlendirmek üzere otomatik olarak güncelleştirilir. Belirli RPO ve RTO verileri için bkz. [Iş sürekliliği 'Ne genel bakış](sql-database-business-continuity.md).

Otomatik yük devretme grupları otomatik yük devretme ilkesiyle kullanılırken, SQL veritabanı sunucusundaki veya yönetilen örnekteki veritabanlarını etkileyen herhangi bir kesinti otomatik yük devretmeyle sonuçlanır. Şunu kullanarak otomatik yük devretme grubunu yönetebilirsiniz:

- [Azure Portal](sql-database-implement-geo-distributed-database.md)
- [Azure CLı: yük devretme grubu](scripts/sql-database-add-single-db-to-failover-group-cli.md)
- [PowerShell: yük devretme grubu](scripts/sql-database-add-single-db-to-failover-group-powershell.md)
- [REST API: yük devretme grubu](/rest/api/sql/failovergroups).

Yük devretmeden sonra sunucunuzun ve veritabanınızın kimlik doğrulama gereksinimlerinin yeni birincil üzerinde yapılandırıldığından emin olun. Ayrıntılar için bkz. [olağanüstü durum kurtarma sonrasında SQL veritabanı güvenliği](sql-database-geo-replication-security-config.md).

Gerçek iş sürekliliği sağlamak için, veri merkezleri arasında veritabanı yedekliliği eklemek çözümün yalnızca bir parçasıdır. Bir uygulamayı (hizmet) çok zararlı bir hatadan sonra kurtarmak, hizmeti ve bağımlı hizmetleri oluşturan tüm bileşenlerin kurtarılmasını gerektirir. Bu bileşenlere örnek olarak, istemci yazılımı (örneğin, özel JavaScript içeren bir tarayıcı), Web ön uçları, depolama alanı ve DNS sayılabilir. Tüm bileşenlerin aynı hatalara dayanıklı olması ve uygulamanızın kurtarma süresi hedefi (RTO) içinde kullanılabilir olması önemlidir. Bu nedenle, tüm bağımlı hizmetleri belirlemeniz ve sağladıkları garantileri ve özellikleri anlamanız gerekir. Daha sonra, hizmetin bağımlı olduğu hizmetlerin yük devretmesi sırasında işlevlerinizin çalıştığından emin olmak için yeterli adımları uygulamanız gerekir. Olağanüstü durum kurtarma çözümleri tasarlama hakkında daha fazla bilgi için bkz. [etkin coğrafi çoğaltma kullanarak olağanüstü durum kurtarma Için bulut çözümleri tasarlama](sql-database-designing-cloud-solutions-for-disaster-recovery.md).

## <a name="auto-failover-group-terminology-and-capabilities"></a>Otomatik yük devretme grubu terimleri ve özellikleri

- **Yük devretme grubu (FOG)**

  Yük devretme grubu, tek bir SQL veritabanı sunucusu tarafından yönetilen ve tek bir yönetilen örnek içinde, tüm veya bazı birincil veritabanlarının birincil bölgedeki bir kesinti nedeniyle kullanılamaz hale gelebilmesi için bir birim olarak yük devretmeye yönelik adlandırılmış bir grup veritabanıdır. Yönetilen örnekler için oluşturulduğunda, bir yük devretme grubu örnekteki tüm Kullanıcı veritabanlarını içerir ve bu nedenle bir örnek üzerinde yalnızca bir yük devretme grubu yapılandırılabilir.
  
  > [!IMPORTANT]
  > Yük devretme grubunun adı, `.database.windows.net` etki alanı içinde genel olarak benzersiz olmalıdır.

- **SQL veritabanı sunucuları**

     SQL veritabanı sunucularıyla tek bir SQL veritabanı sunucusundaki kullanıcı veritabanlarının bazıları veya tümü bir yük devretme grubuna yerleştirilebilir. Ayrıca, SQL veritabanı sunucusu tek bir SQL veritabanı sunucusunda birden fazla yük devretme grubunu destekler.

- **Birinc**

  Yük devretme grubundaki birincil veritabanlarını barındıran SQL veritabanı sunucusu veya yönetilen örnek.

- **İK**

  Yük devretme grubundaki ikincil veritabanlarını barındıran SQL veritabanı sunucusu veya yönetilen örnek. İkincil, birincil ile aynı bölgede olamaz.

- **Yük devretme grubuna tek veritabanları ekleme**

  Aynı SQL veritabanı sunucusuna aynı yük devretme grubuna birkaç tek veritabanı yerleştirebilirsiniz. Yük devretme grubuna tek bir veritabanı eklerseniz, ikincil sunucuda aynı sürümü ve işlem boyutunu kullanarak otomatik olarak ikincil bir veritabanı oluşturur.  Yük devretme grubu oluşturulduğunda bu sunucuyu belirttiniz. İkincil sunucuda zaten ikincil bir veritabanına sahip olan bir veritabanını eklerseniz, bu coğrafi çoğaltma bağlantısı Grup tarafından devralınır. Yük devretme grubunun parçası olmayan bir sunucuda zaten ikincil veritabanına sahip bir veritabanı eklediğinizde, ikincil sunucuda yeni bir ikincil oluşturulur.

  > [!IMPORTANT]
  > İkincil sunucuda, var olan bir ikincil veritabanı olmadığı müddetçe aynı ada sahip bir veritabanı bulunmadığından emin olun. Yönetilen örnek için yük devretme gruplarında tüm kullanıcı veritabanları çoğaltılır. Yük devretme grubundaki çoğaltma için kullanıcı veritabanlarının bir alt kümesini seçemezsiniz.

- **Elastik havuzdaki veritabanlarını yük devretme grubuna ekleme**

  Elastik havuz içindeki tüm veya birkaç veritabanını aynı yük devretme grubuna yerleştirebilirsiniz. Birincil veritabanı elastik bir havuzda ise, ikincil havuz aynı ada (ikincil havuz) sahip esnek havuzda otomatik olarak oluşturulur. İkincil sunucunun, yük devretme grubu tarafından oluşturulacak ikincil veritabanlarını barındırmak için aynı tam adı ve yeterli boş kapasiteye sahip esnek bir havuz içerdiğinden emin olmanız gerekir. Havuza ikincil havuzda zaten ikincil bir veritabanı olan bir veritabanı eklerseniz, bu coğrafi çoğaltma bağlantısı Grup tarafından devralınır. Yük devretme grubunun parçası olmayan bir sunucuda zaten ikincil veritabanına sahip bir veritabanı eklediğinizde ikincil havuzda yeni bir ikincil oluşturulur.
  
- **İlk dengeli dağıtım** 

  Veritabanları, elastik havuzlar veya yönetilen örnekleri bir yük devretme grubuna eklerken, veri çoğaltma başlamadan önce bir ilk dengeli dağıtım aşaması vardır. İlk dengeli dağıtım aşaması en uzun ve en pahalı işlemdir. İlk dengeli dağıtım tamamlandıktan sonra veriler eşitlenir ve ardından yalnızca sonraki veri değişiklikleri çoğaltılır. İlk çekirdek işleminin tamamlaması için gereken süre, verilerinizin boyutuna, çoğaltılan veritabanlarının sayısına ve yük devretme grubundaki varlıklar arasındaki bağlantının hızına bağlıdır. Normal koşullarda tipik dengeli dağıtım hızı, tek bir veritabanı veya elastik havuz için 50-500 GB, yönetilen bir örnek için saat 18-35 GB 'dir. Dengeli dağıtım, tüm veritabanları için paralel olarak gerçekleştirilir. Belirtilen dengeli dağıtım hızını, veri çoğaltma başlamadan önce ilk dengeli dağıtım aşamasının ne kadar süreyle alınacağını tahmin etmek için veritabanlarının sayısı ve toplam veri boyutu ile birlikte kullanabilirsiniz.

  Yönetilen örnekler için, iki örnek arasındaki hızlı rota bağlantısının hızının, ilk dengeli dağıtım aşamasının süresi tahmin edildiğinde dikkate alınması gerekir. İki örnek arasındaki bağlantının hızı gerekenden daha yavaşsa, tohum süresi büyük olasılıkla özellikle etkilendi. Veri çoğaltma başlamadan önce ilk dengeli dağıtım aşamasının ne kadar süreyle yapılacağını tahmin etmek için belirtilen dengeli dağıtım hızını, veritabanı sayısını, toplam veri boyutunu ve bağlantı hızını kullanabilirsiniz. Örneğin, tek bir 100 GB veritabanı için ilk Çekirdek aşaması, bağlantının saat başına 35 GB ile uyumlu olması halinde 2,8-5,5 saatten herhangi bir yere kadar sürer. Bağlantı yalnızca saat başına 10 GB aktarabiliyorsanız, 100 GB 'lik bir veritabanının dağıtımı, yaklaşık 10 saat sürer. Çoğaltılacak birden çok veritabanı varsa, sağlama paralel olarak yürütülür ve yavaş bir bağlantı hızıyla birleştirildiğinde, özellikle tüm veritabanlarındaki verilerin paralel dağıtımı kullanılabilir olan değeri aşarsa, ilk dengeli dağıtım aşaması önemli ölçüde uzun sürebilir. bant genişliğini bağlayın. İki örnek arasındaki ağ bant genişliği sınırlıysa ve bir yük devretme grubuna birden çok yönetilen örnek ekliyorsanız, yük devretme grubuna birden çok yönetilen örnek eklemeyi göz önünde bulundurun.

  
- **DNS bölgesi**

  Yeni bir örnek oluşturulduğunda otomatik olarak oluşturulan benzersiz bir KIMLIK. Aynı DNS bölgesindeki herhangi bir örneğe istemci bağlantılarının kimliğini doğrulamak için bu örnek için bir çoklu etki alanı (SAN) sertifikası sağlanır. Aynı yük devretme grubundaki iki yönetilen örnek, DNS bölgesini paylaşmalıdır.
  
  > [!NOTE]
  > SQL veritabanı sunucuları için oluşturulan yük devretme grupları için bir DNS bölge KIMLIĞI gerekli değildir.

- **Yük devretme grubu okuma-yazma dinleyicisi**

  Geçerli birincil URL 'yi işaret eden bir DNS CNAME kaydı. Yük devretme grubu oluşturulduğunda otomatik olarak oluşturulur ve yük devretme sonrasında birincil değişiklikler olan okuma/yazma SQL iş yükünün birincil veritabanına şeffaf bir şekilde yeniden bağlanmasına izin verir. Yük devretme grubu bir SQL veritabanı sunucusunda oluşturulduğunda, dinleyici URL 'SI için DNS CNAME kaydı `<fog-name>.database.windows.net`olarak biçimlendirilir. Yönetilen bir örnekte yük devretme grubu oluşturulduğunda, dinleyici URL 'SI için DNS CNAME kaydı `<fog-name>.zone_id.database.windows.net`olarak biçimlendirilir.

- **Yük devretme grubu salt okuma dinleyicisi**

  İkincil URL 'yi işaret eden salt okunurdur dinleyiciyi işaret eden bir DNS CNAME kaydı. Yük devretme grubu oluşturulduğunda otomatik olarak oluşturulur ve salt okunurdur SQL iş yükünün belirtilen Yük Dengeleme kurallarını kullanarak ikincil sunucuya şeffaf bir şekilde bağlanmasına izin verir. Yük devretme grubu bir SQL veritabanı sunucusunda oluşturulduğunda, dinleyici URL 'SI için DNS CNAME kaydı `<fog-name>.secondary.database.windows.net`olarak biçimlendirilir. Yönetilen bir örnekte yük devretme grubu oluşturulduğunda, dinleyici URL 'SI için DNS CNAME kaydı `<fog-name>.zone_id.secondary.database.windows.net`olarak biçimlendirilir.

- **Otomatik yük devretme ilkesi**

  Varsayılan olarak, bir yük devretme grubu otomatik yük devretme ilkesiyle yapılandırılır. SQL veritabanı hizmeti, hata algılandıktan ve yetkisiz kullanım süresi dolduktan sonra yük devretmeyi tetikler. Sistem, etkinin ölçeklendirilmesi nedeniyle, [SQL veritabanı hizmetinin yerleşik yüksek kullanılabilirlik altyapısı](sql-database-high-availability.md) tarafından kesinti olup olmadığı doğrulanmalıdır. Yük devretme iş akışını uygulamadan denetlemek istiyorsanız otomatik yük devretmeyi devre dışı bırakabilirsiniz.
  
  > [!NOTE]
  > Kesinti ölçeğinde ve ne kadar hızlı bir şekilde azalmayabileceğini doğrulamak için, işlem ekibine yetkisiz kullanım süresi bir saat altına ayarlanamaz.  Bu sınırlama, veri eşitleme durumu ne olursa olsun, yük devretme grubundaki tüm veritabanları için geçerlidir. 

- **Salt okuma yük devretme ilkesi**

  Varsayılan olarak, salt okunurdur dinleyicinin yük devretmesi devre dışıdır. İkincil çevrimdışıyken, birincil performans performansının etkilenmemesini sağlar. Bununla birlikte, ikincil kurtarılana kadar salt okuma oturumlarının bağlanamadığı anlamına gelir. Salt okuma oturumları için kapalı kalma süresini kabul edemıyorsanız ve birincil olarak hem salt okunurdur hem de okuma-yazma trafiği için birincil özelliği geçici olarak kullanmaya devam ediyorsanız, `AllowReadOnlyFailoverToPrimary` özelliğini yapılandırarak salt okuma dinleyicisi için yük devretmeyi etkinleştirebilirsiniz. Bu durumda, ikincil kullanılabilir değilse salt okuma trafiği otomatik olarak birincil olarak yönlendirilir.

- **Planlı Yük devretme**

   Planlı Yük devretme, birincil ve ikincil veritabanları arasında birincil role kadar olan tam eşitleme gerçekleştirir. Bu, veri kaybı garantisi vermez. Planlı Yük devretme aşağıdaki senaryolarda kullanılır:

  - Veri kaybı kabul edilebilir olmadığında üretimde olağanüstü durum kurtarma (DR) tatbilar gerçekleştirme
  - Veritabanlarını farklı bir bölgeye yeniden konumlandırma
  - Kesinti azaltıldıktan sonra veritabanlarını birincil bölgeye döndürün (yeniden çalışma).

- **Planlanmamış yük devretme**

   Planlanmamış veya zorlamalı yük devretme, birincili eşitleme yapmadan ikincili doğrudan birincil role geçirir. Bu işlem, veri kaybına neden olur. Planlanmamış yük devretme, birincil erişim olmadığında kesintiler sırasında kurtarma yöntemi olarak kullanılır. Özgün birincil yeniden çevrimiçi olduğunda, eşitleme olmadan otomatik olarak yeniden bağlanır ve yeni bir ikincil hale gelir.

- **El ile yük devretme**

  Otomatik yük devretme yapılandırması ne olursa olsun, yük devretmeyi dilediğiniz zaman el ile başlatabilirsiniz. Otomatik yük devretme ilkesi yapılandırılmamışsa, yük devretme grubundaki veritabanlarını ikincil gruba kurtarmak için el ile yük devretme gerekir. Zorunlu veya kolay yük devretme (tam veri eşitlemeyle) başlatabilirsiniz. İkincisi ikincil bölgeye yeniden konumlandırmak için kullanılabilir. Yük devretme tamamlandığında, DNS kayıtları, yeni birincili bağlantıyı sağlamak üzere otomatik olarak güncelleştirilir

- **Veri kaybı olan yetkisiz kullanım süresi**

  Birincil ve ikincil veritabanları zaman uyumsuz çoğaltma kullanılarak eşitlendiğinden, yük devretme veri kaybına neden olabilir. Otomatik yük devretme ilkesini, uygulamanızın veri kaybına karşı dayanıklılığını yansıtacak şekilde özelleştirebilirsiniz. `GracePeriodWithDataLossHours`yapılandırarak, sistemin sonuç olarak veri kaybına neden olan yük devretmeyi başlatmadan önce bekleyeceği süreyi kontrol edebilirsiniz.

- **Çoklu yük devretme grupları**

  Yük devretme ölçeğini denetlemek için aynı sunucu çifti için birden çok yük devretme grubu yapılandırabilirsiniz. Her grup bağımsız olarak devredildi. Çok kiracılı uygulamanız elastik havuzlar kullanıyorsa, bu özelliği kullanarak her havuzda birincil ve ikincil veritabanlarını karıştırabilirsiniz. Bu şekilde, bir kesinti etkisini yalnızca kiracıların yarısını azaltabilirsiniz.

  > [!NOTE]
  > Yönetilen örnek çoklu yük devretme gruplarını desteklemiyor.
  
## <a name="permissions"></a>İzinler

Yük devretme grubu izinleri [rol tabanlı erişim denetimi (RBAC)](../role-based-access-control/overview.md)aracılığıyla yönetilir. [SQL Server katkıda](../role-based-access-control/built-in-roles.md#sql-server-contributor) bulunan rolü, yük devretme gruplarını yönetmek için gereken tüm izinlere sahiptir.

### <a name="create-failover-group"></a>Yük devretme grubu oluştur

Bir yük devretme grubu oluşturmak için, hem birincil hem de ikincil sunuculara ve yük devretme grubundaki tüm veritabanlarına RBAC yazma erişimine sahip olmanız gerekir. Yönetilen bir örnek için, hem birincil hem de ikincil yönetilen örnek için RBAC yazma erişimine ihtiyacınız vardır, ancak bireysel olarak yönetilen örnek veritabanları bir yük devretme grubuna eklenemediği veya bu gruba kaldırıldığı için tek tek veritabanlarındaki izinler ilgili değildir. 

### <a name="update-a-failover-group"></a>Yük devretme grubunu güncelleştirme

Bir yük devretme grubunu güncelleştirmek için, yük devretme grubuna ve geçerli birincil sunucu veya yönetilen örnekteki tüm veritabanlarına RBAC yazma erişimine sahip olmanız gerekir.  

### <a name="failover-a-failover-group"></a>Yük devretme grubu yük devretme

Yük devretme grubu yükünü devretmek için, yeni birincil sunucu veya yönetilen örnekteki yük devretme grubuna RBAC yazma erişimine sahip olmanız gerekir.

## <a name="best-practices-of-using-failover-groups-with-single-databases-and-elastic-pools"></a>Tek veritabanları ve elastik havuzlarla yük devretme grupları kullanmanın en iyi yöntemleri

Otomatik yük devretme grubu, birincil SQL veritabanı sunucusunda yapılandırılmalı ve bunu farklı bir Azure bölgesindeki ikincil SQL veritabanı sunucusuna bağlayacaktır. Gruplar, bu sunuculardaki tüm veya bazı veritabanlarını içerebilir. Aşağıdaki diyagramda birden çok veritabanı ve otomatik yük devretme grubu kullanılarak coğrafi olarak yedekli bir bulut uygulamasının tipik bir yapılandırması gösterilmektedir.

![otomatik yük devretme](./media/sql-database-auto-failover-group/auto-failover-group.png)

> [!NOTE]
> Yük devretme grubuna tek bir veritabanı ekleme hakkında ayrıntılı adım adım öğretici için bkz. bir [Yük devretme grubuna tek veritabanı ekleme](sql-database-single-database-failover-group-tutorial.md) .

İş sürekliliği ile bir hizmet tasarlarken aşağıdaki genel yönergeleri izleyin:

### <a name="using-one-or-several-failover-groups-to-manage-failover-of-multiple-databases"></a>Birden çok veritabanının yük devretmesini yönetmek için bir veya birkaç yük devretme grubu kullanma

Farklı bölgelerde (birincil ve ikincil sunucular) iki sunucu arasında bir veya daha fazla yük devretme grubu oluşturulabilir. Her grup, birincil bölgedeki bir kesinti nedeniyle tüm veya bazı birincil veritabanlarının kullanılamaz duruma gelmesi durumunda birim olarak kurtarılan bir veya birkaç veritabanı içerebilir. Yük devretme grubu, birincil ile aynı hizmet hedefine sahip coğrafi ikincil veritabanı oluşturur. Yük devretme grubuna mevcut bir coğrafi çoğaltma ilişkisi eklerseniz, coğrafi ikincil öğenin aynı hizmet katmanıyla ve işlem boyutuyla birincil olarak yapılandırıldığından emin olun.
  
> [!IMPORTANT]
> Farklı aboneliklerde iki sunucu arasında yük devretme gruplarının oluşturulması, tek veritabanları ve elastik havuzlar için şu anda desteklenmemektedir. Yük devretme grubu oluşturulduktan sonra birincil veya ikincil sunucuyu farklı bir aboneliğe taşırsanız, yük devretme istekleri ve diğer işlemler hatalara neden olabilir.

### <a name="using-read-write-listener-for-oltp-workload"></a>OLTP iş yükü için okuma-yazma dinleyicisi kullanma

OLTP işlemleri gerçekleştirirken sunucu URL 'SI olarak `<fog-name>.database.windows.net` kullanın ve bağlantılar otomatik olarak birincil ağa yönlendirilir. Bu URL, yük devretmeden sonra değişmez. Bunun için yük devretme, DNS kaydının güncelleştirilmesini içerir, böylece istemci bağlantıları yalnızca istemci DNS önbelleği yenilendikten sonra yeni birincil yere yönlendirilir.

### <a name="using-read-only-listener-for-read-only-workload"></a>Salt okunurdur iş yükü için salt okunurdur dinleyicisi kullanma

Verilerin belirli bir şekilde kullanılması için dayanıklı bir mantıksal olarak yalıtılmış salt okunurdur, uygulamadaki ikincil veritabanını kullanabilirsiniz. Salt okuma oturumları için sunucu URL 'SI olarak `<fog-name>.secondary.database.windows.net` kullanın ve bağlantı otomatik olarak ikinciye yönlendirilir. `ApplicationIntent=ReadOnly`kullanarak bağlantı dizesi okuma hedefini de belirtmeniz önerilir. Yük devretme sonrasında salt okuma iş yükünün yeniden bağlanabildiğinden emin olmak istiyorsanız veya ikincil sunucunun çevrimdışı olması durumunda, yük devretme ilkesinin `AllowReadOnlyFailoverToPrimary` özelliğini yapılandırdığınızdan emin olun.

### <a name="preparing-for-performance-degradation"></a>Performans düşüşü için hazırlanma

Tipik bir Azure uygulaması birden çok Azure hizmeti kullanır ve birden çok bileşenden oluşur. Yük devretme grubunun otomatik yük devretmesi, Azure SQL bileşenleri yalnızca durum temelinde tetiklenir. Birincil bölgedeki diğer Azure hizmetleri kesinti tarafından etkilenmeyebilir ve bileşenleri bu bölgede kullanılabilir olmaya devam edebilir. Birincil veritabanları DR bölgesine geçiş yaptıktan sonra, bağımlı bileşenler arasındaki gecikme artabilir. Uygulamanın performansına yönelik daha yüksek gecikme süresini önlemek için tüm uygulama bileşenlerinin DR bölgesindeki yedekliliği olduğundan emin olun ve bu [ağ güvenlik yönergelerini](#failover-groups-and-network-security)izleyin.

### <a name="preparing-for-data-loss"></a>Veri kaybı için hazırlanma

Bir kesinti algılanırsa, SQL `GracePeriodWithDataLossHours`belirttiğiniz dönemi bekler. Varsayılan değer 1 saattir. Veri kaybını uygun hale getirmek için `GracePeriodWithDataLossHours`, 24 saat gibi yeterince büyük bir sayı olarak ayarladığınızdan emin olun. İkincil sunucudan birinciye yeniden yük devretmek için el ile grup yük devretmesini kullanın.

> [!IMPORTANT]
> 800 veya daha az DTU ile esnek havuzlar ve coğrafi çoğaltma kullanan 250 ' den fazla veritabanı, daha uzun planlı yük devretme ve performans düşüklükiyle ilgili sorunlarla karşılaşabilir.  Bu sorunların, yazma yoğunluklu iş yükleri, coğrafi çoğaltma uç noktaları Coğrafya tarafından yaygın olarak ayrıldığı veya her veritabanı için birden çok ikincil uç nokta kullanıldığı durumlarda oluşma olasılığı daha yüksektir.  Bu sorunların belirtileri, coğrafi çoğaltma gecikmesi zaman içinde arttıkça belirtilir.  Bu gecikme, [sys. dm_geo_replication_link_status](/sql/relational-databases/system-dynamic-management-views/sys-dm-geo-replication-link-status-azure-sql-database)kullanılarak izlenebilir.  Bu sorunlar oluşursa, azaltmaları, havuz DTU sayısını artırmayı veya aynı havuzdaki coğrafi çoğaltılan veritabanlarının sayısını azaltmayı içerir.

### <a name="changing-secondary-region-of-the-failover-group"></a>Yük devretme grubunun ikincil bölgesini değiştirme

Değişiklik sırasını göstermek için sunucu A 'nın birincil sunucu olduğunu, sunucu B 'nin var olan ikincil sunucu olduğunu ve sunucu C 'nin üçüncü bölgedeki yeni ikincil olduğunu varsayacağız.  Geçişi yapmak için şu adımları izleyin:

1.  [Etkin coğrafi çoğaltma](sql-database-active-geo-replication.md)kullanarak her bir veritabanı Için Sunucu A, sunucu C üzerinde ek ikincil sunucular oluşturun. Sunucu A 'daki her veritabanı, biri sunucu B 'de diğeri sunucu C 'de olmak üzere iki ikincil adına sahip olur. Bu, geçiş sırasında birincil veritabanlarının korunduğundan emin olmaya devam edecektir.
2.  Yük devretme grubunu silin. Bu noktada, oturum açma işlemleri başarısız olur. Bunun nedeni, yük devretme grubu dinleyicilerinin SQL diğer adlarının silindiği ve ağ geçidinin yük devretme grubu adını tanıyamayacağı içindir.
3.  Yük devretme grubunu, A ve C sunucuları arasında aynı adla yeniden oluşturun. Bu noktada, oturum açmalar başarısız olur.
4.  A sunucusundaki tüm birincil veritabanlarını yeni yük devretme grubuna ekleyin.
5.  Sunucu B 'yi bırakın. B üzerindeki tüm veritabanları otomatik olarak silinecek. 


### <a name="changing-primary-region-of-the-failover-group"></a>Yük devretme grubunun birincil bölgesi değiştiriliyor

Değişiklik sırasını göstermek için sunucu A 'nın birincil sunucu olduğunu, sunucu B 'nin var olan ikincil sunucuyu ve sunucu C 'nin, üçüncü bölgedeki yeni birincidir olduğunu varsayacağız.  Geçişi yapmak için şu adımları izleyin:

1.  Birincil sunucuyu B 'ye geçirmek için planlı bir yük devretme gerçekleştirin. Sunucu A, yeni ikincil sunucu olacak. Yük devretme işlemi birkaç dakika kapalı kalma süresine neden olabilir. Gerçek süre, yük devretme grubunun boyutuna bağlı olacaktır.
2.  [Etkin coğrafi çoğaltma](sql-database-active-geo-replication.md)kullanarak sunucu B 'de sunucu C 'ye her bir veritabanının ek ikincil sürümlerini oluşturun. Sunucu B üzerindeki her bir veritabanı, biri sunucu A ve sunucu C 'de olmak üzere iki ikincil adına sahip olur. Bu, geçiş sırasında birincil veritabanlarının korunduğundan emin olmaya devam edecektir.
3.  Yük devretme grubunu silin. Bu noktada, oturum açma işlemleri başarısız olur. Bunun nedeni, yük devretme grubu dinleyicilerinin SQL diğer adlarının silindiği ve ağ geçidinin yük devretme grubu adını tanıyamayacağı içindir.
4.  Yük devretme grubunu, A ve C sunucuları arasında aynı adla yeniden oluşturun. Bu noktada, oturum açmalar başarısız olur.
5.  B üzerindeki tüm birincil veritabanlarını yeni yük devretme grubuna ekleyin. 
6.  B ve C 'ye geçiş yapmak için yük devretme grubunun planlı bir yük devretmesini gerçekleştirin. Artık sunucu C birincil ve B-ikincil olur. A sunucusundaki tüm ikincil veritabanları, C 'deki temelinde otomatik olarak bağlanır. Adım 1 ' de olduğu gibi, yük devretme işlemi birkaç dakika kapalı kalma süresine yol açabilir.
6.  Sunucu A ' yı bırakın. İçindeki tüm veritabanları otomatik olarak silinir.

> [!IMPORTANT]
> Yük devretme grubu silindiğinde, dinleyici uç noktaları için DNS kayıtları da silinir. Bu noktada, başka birinin bir yük devretme grubu veya aynı ada sahip sunucu diğer adı oluştururken, onu yeniden kullanmanızı önleyen sıfır olmayan bir olasılık vardır. Riski en aza indirmek için genel yük devretme grubu adlarını kullanmayın.

## <a name="best-practices-of-using-failover-groups-with-managed-instances"></a>Yönetilen örneklerle yük devretme grupları kullanmanın en iyi yöntemleri

Otomatik yük devretme grubu birincil örnekte yapılandırılmalı ve bunu farklı bir Azure bölgesindeki ikincil örneğe bağlayacaktır.  Örnekteki tüm veritabanları ikincil örneğe çoğaltılır.

Aşağıdaki diyagramda, yönetilen örnek ve otomatik yük devretme grubu kullanılarak coğrafi olarak yedekli bir bulut uygulamasının tipik bir yapılandırması gösterilmektedir.

![otomatik yük devretme](./media/sql-database-auto-failover-group/auto-failover-group-mi.png)

> [!NOTE]
> Yük devretme grubunu kullanmak için yönetilen bir örnek ekleme hakkında ayrıntılı adım adım öğretici için bkz. [Yük devretme grubuna yönetilen örnek ekleme](sql-database-managed-instance-failover-group-tutorial.md) .

Uygulamanız veri katmanı olarak yönetilen örnek kullanıyorsa, iş sürekliliği için tasarlarken aşağıdaki genel yönergeleri izleyin:

### <a name="creating-the-secondary-instance"></a>İkincil örnek oluşturma 

Yük devretmeden sonra birincil örneğe kesintiye uğramayan bağlantı sağlamak için hem birincil hem de ikincil örneklerin aynı DNS bölgesinde olması gerekir. Aynı çoklu etki alanı (SAN) sertifikasının, yük devretme grubundaki iki örneklerden birine yönelik istemci bağlantılarının kimliğini doğrulamak için kullanılabilir olmasını garanti eder. Uygulamanız üretim dağıtımı için hazırsanız, farklı bir bölgede ikincil bir örnek oluşturun ve DNS bölgesini birincil örnekle paylaştığından emin olun. Azure portal, PowerShell veya REST API kullanarak isteğe bağlı `DNS Zone Partner` bir parametre belirterek bunu yapabilirsiniz.

> [!IMPORTANT]
> Alt ağda oluşturulan ilk örnek, aynı alt ağdaki sonraki tüm örnekler için DNS bölgesini belirler. Diğer bir deyişle, aynı alt ağdaki iki örnek farklı DNS bölgelerine ait olamaz.

Birincil örnekle aynı DNS bölgesinde ikincil örnek oluşturma hakkında daha fazla bilgi için bkz. [İkincil yönetilen örnek oluşturma](sql-database-managed-instance-failover-group-tutorial.md#3---create-a-secondary-managed-instance).

### <a name="enabling-replication-traffic-between-two-instances"></a>İki örnek arasında çoğaltma trafiği etkinleştiriliyor

Her örnek kendi VNet 'inde yalıtılmış olduğundan, bu VNET 'ler arasındaki iki yönlü trafiğe izin verilmelidir. Bkz. [Azure VPN ağ geçidi](../vpn-gateway/vpn-gateway-about-vpngateways.md)

### <a name="creating-a-failover-group-between-managed-instances-in-different-subscriptions"></a>Farklı aboneliklerdeki yönetilen örnekler arasında yük devretme grubu oluşturma

Yönetilen örnekler arasında iki farklı abonelikteki bir yük devretme grubu oluşturabilirsiniz. PowerShell API 'sini kullanırken, ikincil örnek için `PartnerSubscriptionId` parametresini belirterek bunu yapabilirsiniz. REST API kullanırken, `properties.managedInstancePairs` parametresinde bulunan her örnek KIMLIĞI kendi SubscriptionID değerine sahip olabilir.
  
> [!IMPORTANT]
> Azure portal, farklı aboneliklerde yük devretme gruplarının oluşturulmasını desteklemez. Ayrıca, farklı abonelikler ve/veya kaynak gruplarındaki mevcut yük devretme grupları için yük devretme, Portal aracılığıyla birincil örnekten el ile başlatılamaz. Bunun yerine coğrafi ikincil örnekten başlatın.

### <a name="managing-failover-to-secondary-instance"></a>İkincil örneğe yük devretmeyi yönetme

Yük devretme grubu, örnekteki tüm veritabanlarının yük devretmesini yönetecektir. Bir grup oluşturulduğunda, örnekteki her bir veritabanı, ikincil örneğe otomatik olarak coğrafi olarak çoğaltılır. Veritabanlarının bir alt kümesinin kısmi yük devretmesini başlatmak için yük devretme grupları kullanamazsınız.

> [!IMPORTANT]
> Birincil örnekten bir veritabanı kaldırılırsa, coğrafi ikincil örneğe de otomatik olarak bırakılır.

### <a name="using-read-write-listener-for-oltp-workload"></a>OLTP iş yükü için okuma-yazma dinleyicisi kullanma

OLTP işlemleri gerçekleştirirken sunucu URL 'SI olarak `<fog-name>.zone_id.database.windows.net` kullanın ve bağlantılar otomatik olarak birincil ağa yönlendirilir. Bu URL, yük devretmeden sonra değişmez. Yük devretme, DNS kaydının güncelleştirilmesini içerir, bu nedenle istemci bağlantıları yalnızca istemci DNS önbelleği yenilendikten sonra yeni birincil istemciye yönlendirilir. İkincil örnek, DNS bölgesini birincil ile paylaştığından, istemci uygulaması aynı SAN sertifikasını kullanarak buna yeniden bağlanabilir.

### <a name="using-read-only-listener-to-connect-to-the-secondary-instance"></a>İkincil örneğe bağlanmak için salt okuma dinleyicisi kullanma

Verilerin belirli bir şekilde kullanılması için dayanıklı bir mantıksal olarak yalıtılmış salt okunurdur, uygulamadaki ikincil veritabanını kullanabilirsiniz. Coğrafi olarak çoğaltılan ikinciye doğrudan bağlanmak için `server.secondary.zone_id.database.windows.net` sunucu URL 'SI olarak kullanın ve bağlantı doğrudan coğrafi çoğaltılan ikincil öğesine yapılır.

> [!NOTE]
> Bazı hizmet katmanlarında Azure SQL veritabanı, salt okunurdur ve salt okuma sorgusu yükünü [dengelemek için salt okunurdur ve](sql-database-read-scale-out.md) bağlantı dizesindeki `ApplicationIntent=ReadOnly` parametresini kullanarak salt okunurdur. Coğrafi olarak çoğaltılan bir ikincil yapılandırdığınız zaman, birincil konumdaki veya coğrafi olarak çoğaltılan konumdaki salt okunurdur bir kopyaya bağlanmak için bu özelliği kullanabilirsiniz.
> - Birincil konumdaki bir salt tanımlı çoğaltmaya bağlanmak için `<fog-name>.zone_id.database.windows.net`kullanın.
> - İkincil konumdaki bir salt okuma çoğaltmasına bağlanmak için `<fog-name>.secondary.zone_id.database.windows.net`kullanın.

### <a name="preparing-for-performance-degradation"></a>Performans düşüşü için hazırlanma

Tipik bir Azure uygulaması birden çok Azure hizmeti kullanır ve birden çok bileşenden oluşur. Yük devretme grubunun otomatik yük devretmesi, Azure SQL bileşenleri yalnızca durum temelinde tetiklenir. Birincil bölgedeki diğer Azure hizmetleri kesinti tarafından etkilenmeyebilir ve bileşenleri bu bölgede kullanılabilir olmaya devam edebilir. Birincil veritabanları DR bölgesine geçiş yaptıktan sonra, bağımlı bileşenler arasındaki gecikme artabilir. Uygulamanın performansına yönelik daha yüksek gecikme süresini önlemek için tüm uygulama bileşenlerinin DR bölgesindeki yedekliliği olduğundan emin olun ve bu [ağ güvenlik yönergelerini](#failover-groups-and-network-security)izleyin.

### <a name="preparing-for-data-loss"></a>Veri kaybı için hazırlanma

Bir kesinti algılanırsa, en iyisi hakkında hiç veri kaybı yoksa, SQL otomatik olarak okuma-yazma yük devretmesini tetikler. Aksi takdirde, `GracePeriodWithDataLossHours`belirttiğiniz dönemi bekler. `GracePeriodWithDataLossHours`belirlediyseniz, veri kaybı için hazırlıklı olun. Genel olarak, kesintiler sırasında Azure kullanılabilirliği tercih eder. Veri kaybını uygun hale getirmek için GracePeriodWithDataLossHours, 24 saat gibi yeterince büyük bir sayı olarak ayarladığınızdan emin olun.

Okuma-yazma dinleyicisinin DNS güncelleştirmesi, yük devretme başlatıldıktan hemen sonra gerçekleşir. Bu işlem, veri kaybına neden olmaz. Ancak, veritabanı rollerini değiştirme işlemi normal koşullarda 5 dakikaya kadar sürebilir. Tamamlanana kadar, yeni birincil örnekteki bazı veritabanları hala salt okunurdur. Yük devretme PowerShell kullanılarak başlatılmışsa, tüm işlem zaman uyumludur. Azure portal kullanılarak başlatılmışsa, Kullanıcı arabirimi tamamlanma durumunu gösterir. REST API kullanılarak başlatılmışsa, tamamlamayı izlemek için standart Azure Resource Manager yoklama mekanizmasını kullanın.

> [!IMPORTANT]
> El ile grup yük devretmesini kullanarak özgün konuma doğru bir şekilde geçiş yapın. Yük devretmeye neden olan kesinti azaltıldığında, birincil veritabanlarınızı özgün konuma taşıyabilirsiniz. Bunu yapmak için, grubun el ile yük devretmesini başlatmanız gerekir.
  
### <a name="changing-secondary-region-of-the-failover-group"></a>Yük devretme grubunun ikincil bölgesini değiştirme

Örnek A 'nın birincil örnek olduğu varsayıyoruz, örnek B var olan ikincil örnek ve C örneği üçüncü bölgedeki yeni ikincil örnek.  Geçişi yapmak için şu adımları izleyin:

1.  Aynı boyuta sahip C örneğini aynı DNS bölgesinde oluşturun. 
2.  A ve B örnekleri arasında yük devretme grubunu silin. Bu noktada, yük devretme grubu dinleyicilerinin SQL diğer adları silindiğinden ve ağ geçidi, yük devretme grubu adını tanımadığı için oturum açma işlemleri başarısız olur. İkincil veritabanlarının ön uç 'lerden bağlantısı kesilecek ve okuma/yazma veritabanları olacaktır. 
3.  A ve C örneğiyle aynı ada sahip bir yük devretme grubu oluşturun. [Yük devretme grubundaki yönergeleri yönetilen örnek öğreticisiyle](sql-database-managed-instance-failover-group-tutorial.md)izleyin. Bu bir veri boyutudur ve bir örnek A 'dan tüm veritabanları birlikte çalıştırıldığında ve eşitlendiğinde tamamlanacaktır.
4.  Gereksiz ücretlerden kaçınmak için gerekmiyorsa örnek B 'yi silin.

> [!NOTE]
> 2\. adım ve 3. adım tamamlanana kadar sonra A örneğindeki veritabanları korumasız olarak kalır.

### <a name="changing-primary-region-of-the-failover-group"></a>Yük devretme grubunun birincil bölgesi değiştiriliyor

Örnek A 'nın birincil örnek olduğu varsayıyoruz, örnek B var olan ikincil örnek ve C örneği, üçüncü bölgedeki yeni birincil örnek.  Geçişi yapmak için şu adımları izleyin:

1.  B örneğini aynı boyuta sahip C ve aynı DNS bölgesinde oluşturun. 
2.  Birincil örneği B 'ye geçirmek için B örneğine ve el ile yük devretmeye bağlanın. örnek A, yeni ikincil örnek otomatik olarak olur.
3.  A ve B örnekleri arasında yük devretme grubunu silin. Bu noktada, yük devretme grubu dinleyicilerinin SQL diğer adları silindiğinden ve ağ geçidi, yük devretme grubu adını tanımadığı için oturum açma işlemleri başarısız olur. İkincil veritabanlarının ön uç 'lerden bağlantısı kesilecek ve okuma/yazma veritabanları olacaktır. 
4.  A ve C örneğiyle aynı ada sahip bir yük devretme grubu oluşturun. [Yük devretme grubundaki yönergeleri yönetilen örnek öğreticisiyle](sql-database-managed-instance-failover-group-tutorial.md)izleyin. Bu bir veri boyutudur ve bir örnek A 'dan tüm veritabanları birlikte çalıştırıldığında ve eşitlendiğinde tamamlanacaktır.
5.  Gereksiz ücretlerden kaçınmak için gerekmiyorsa örneğini silin.

> [!NOTE] 
> Adım 3 ' ten sonra ve 4. adım tamamlanana kadar sonra a örneğindeki veritabanları korumasız olarak kalır.

> [!IMPORTANT]
> Yük devretme grubu silindiğinde, dinleyici uç noktaları için DNS kayıtları da silinir. Bu noktada, başka birinin bir yük devretme grubu veya aynı ada sahip sunucu diğer adı oluştururken, onu yeniden kullanmanızı önleyen sıfır olmayan bir olasılık vardır. Riski en aza indirmek için genel yük devretme grubu adlarını kullanmayın.

## <a name="failover-groups-and-network-security"></a>Yük devretme grupları ve ağ güvenliği

Bazı uygulamalarda, güvenlik kuralları, veri katmanına ağ erişiminin bir VM, Web hizmeti vb. gibi belirli bir bileşenle veya bileşenlerle sınırlı olmasını gerektirir. Bu gereksinim, iş sürekliliği tasarımı ve yük devretme gruplarının kullanımı için bazı güçlükleri sunmaktadır. Bu tür kısıtlı erişimi uygularken aşağıdaki seçenekleri göz önünde bulundurun.

### <a name="using-failover-groups-and-virtual-network-rules"></a>Yük devretme gruplarını ve sanal ağ kurallarını kullanma

SQL veritabanınıza erişimi kısıtlamak için [sanal ağ hizmet uç noktaları ve kuralları](sql-database-vnet-service-endpoint-rule-overview.md) kullanıyorsanız, her bir sanal ağ hizmeti uç noktasının yalnızca bir Azure bölgesi için geçerli olduğunu unutmayın. Uç nokta diğer bölgelerin alt ağdan iletişim kabul etmesine izin vermez. Bu nedenle, yalnızca aynı bölgede dağıtılan istemci uygulamaları birincil veritabanına bağlanabilir. Yük devretme işlemi SQL istemci oturumlarının farklı bir (ikincil) bölgedeki bir sunucuya tekrar yönlendirilmesine neden olduğundan, Bu oturumlar söz konusu bölgenin dışındaki bir istemciden kaynaklandığından başarısız olur. Bu nedenle, katılan sunucular sanal ağ kurallarında yer alıyorsa otomatik yük devretme ilkesi etkinleştirilemez. El ile yük devretmeyi desteklemek için şu adımları izleyin:

1. Uygulamanızın ön uç bileşenlerinin (Web hizmeti, sanal makineler vb.) yedek kopyalarını ikincil bölgede sağlama
2. [Sanal ağ kurallarını](sql-database-vnet-service-endpoint-rule-overview.md) birincil ve ikincil sunucu için ayrı ayrı yapılandırın
3. [Traffic Manager yapılandırması kullanarak ön uç yük devretmesini](sql-database-designing-cloud-solutions-for-disaster-recovery.md#scenario-1-using-two-azure-regions-for-business-continuity-with-minimal-downtime) etkinleştirme
4. Kesinti algılandığında el ile yük devretme başlatın. Bu seçenek, ön uç ve veri katmanı arasında tutarlı gecikme süresi gerektiren uygulamalar için en iyi duruma getirilmiştir ve ön uç, veri katmanı veya her ikisi de kesinti tarafından etkilendiğinde kurtarmayı destekler.

> [!NOTE]
> Salt yazılır bir iş yükünü dengelemek için **salt okunurdur dinleyiciyi** kullanıyorsanız, ikincil veritabanına bağlanabilmesi için bu iş yükünün ikincil BÖLGEDEKI bir VM 'de veya diğer bir kaynakta yürütüldüğünden emin olun.

### <a name="using-failover-groups-and-sql-database-firewall-rules"></a>Yük devretme gruplarını ve SQL veritabanı güvenlik duvarı kurallarını kullanma

İş sürekliliği planınız, otomatik yük devretme içeren grupları kullanarak yük devretme gerektiriyorsa, geleneksel güvenlik duvarı kurallarını kullanarak SQL veritabanınıza erişimi kısıtlayabilirsiniz. Otomatik yük devretmeyi desteklemek için şu adımları izleyin:

1. [Genel IP oluşturma](../virtual-network/virtual-network-public-ip-address.md#create-a-public-ip-address)
2. [Ortak yük dengeleyici oluşturun](../load-balancer/quickstart-load-balancer-standard-public-portal.md) ve genel IP 'yi buna atayın.
3. Ön uç bileşenleriniz için [bir sanal ağ ve sanal makine oluşturma](../load-balancer/quickstart-load-balancer-standard-public-portal.md)
4. [Ağ güvenlik grubu oluşturun](../virtual-network/security-overview.md) ve gelen bağlantıları yapılandırın.
5. Giden bağlantıların ' SQL ' [hizmet etiketi](../virtual-network/security-overview.md#service-tags)KULLANıLARAK Azure SQL veritabanı 'na açık olduğundan emin olun.
6. Adım 1 ' de oluşturduğunuz genel IP adresinden gelen trafiğe izin vermek için bir [SQL veritabanı güvenlik duvarı kuralı](sql-database-firewall-configure.md) oluşturun.

Giden erişimin nasıl yapılandırılacağı ve güvenlik duvarı kurallarında hangi IP 'nin kullanılacağı hakkında daha fazla bilgi için bkz. [yük dengeleyici giden bağlantıları](../load-balancer/load-balancer-outbound-connections.md).

Yukarıdaki yapılandırma, otomatik yük devretmenin ön uç bileşenlerinden gelen bağlantıları engelolmamasını ve uygulamanın ön uç ile veri katmanı arasındaki daha uzun gecikme süresine neden olduğunu varsaymaktadır.

> [!IMPORTANT]
> Bölgesel kesintiler için iş sürekliliği sağlamak üzere hem ön uç bileşenleri hem de veritabanları için coğrafi artıklık sağlamalısınız.

## <a name="enabling-geo-replication-between-managed-instances-and-their-vnets"></a>Yönetilen örnekler ve bunların sanal ağları arasında Coğrafi çoğaltmayı etkinleştirme

İki farklı bölgede birincil ve ikincil yönetilen örnekler arasında bir yük devretme grubu ayarlarken, her örnek bağımsız bir sanal ağ kullanılarak yalıtılmıştır. Bu sanal ağlar arasındaki çoğaltma trafiğine izin vermek için bu önkoşulların karşılandığından emin olun:

1. İki yönetilen örnek farklı Azure bölgelerinde olmalıdır.
2. İki yönetilen örneğinin aynı hizmet katmanı olması gerekir ve aynı depolama boyutuna sahip olmalıdır.
3. İkincil yönetilen örneğinizin boş olması gerekir (Kullanıcı veritabanı yok).
4. Yönetilen örneklerin kullandığı sanal ağların bir [VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md) veya [Express Route](../expressroute/expressroute-howto-circuit-portal-resource-manager.md)ile bağlanması gerekir. İki sanal ağ, şirket içi bir ağla bağlandığında, 5022 ve 11000-11999 bağlantı noktalarını engelleyen bir güvenlik duvarı kuralı bulunmadığından emin olun. Küresel VNet eşlemesi desteklenmiyor.
5. İki yönetilen örnek sanal ağ, çakışan IP adreslerine sahip olamaz.
6. Ağ güvenlik gruplarınızı (NSG), bağlantı noktaları 5022 ve 11000 ~ 12000 aralığı, diğer yönetilen örneğin alt ağından gelen bağlantılar için açık ve giden olarak ayarlamanız gerekir. Bu, örnekler arasında çoğaltma trafiğine izin vermek için kullanılır.

   > [!IMPORTANT]
   > Yanlış yapılandırılmış NSG güvenlik kuralları, veritabanı kopyalama işlemlerinin takılmasına yol açar.

7. İkincil örnek, doğru DNS bölge KIMLIĞIYLE yapılandırılır. DNS bölgesi, yönetilen bir örnek ve sanal kümenin bir özelliğidir ve KIMLIĞI ana bilgisayar adı adresine dahil edilir. Bölge KIMLIĞI, her VNet 'te ilk yönetilen örnek oluşturulduğunda rastgele bir dize olarak oluşturulur ve aynı KIMLIK aynı alt ağdaki diğer tüm örneklere atanır. Atandıktan sonra DNS bölgesi değiştirilemez. Aynı yük devretme grubuna dahil edilen yönetilen örnekler, DNS bölgesini paylaşmalıdır. Bu, ikincil örneği oluştururken birinci örneğin bölge KIMLIĞINI DnsZonePartner parametresinin değeri olarak geçirerek gerçekleştirirsiniz. 

   > [!NOTE]
   > Yönetilen örnek ile yük devretme gruplarını yapılandırmaya ilişkin ayrıntılı bir öğretici için bkz. [bir yük devretme grubuna yönetilen örnek ekleme](sql-database-managed-instance-failover-group-tutorial.md).

## <a name="upgrading-or-downgrading-a-primary-database"></a>Birincil veritabanını yükseltme veya eski sürüme indirme

İkincil veritabanlarının bağlantısını kesmeden, birincil veritabanını farklı bir işlem boyutuna yükseltebilir veya indirgeyebilmeniz gerekir (Genel Amaçlı ve İş Açısından Kritik arasında değil aynı hizmet katmanında). ' Yi yükseltirken, önce tüm ikincil veritabanlarını yükseltmeniz ve ardından birincili yükseltmeniz önerilir. Düşürme sırasında, sırayı tersine çevirin: önce birincili düşürme ve sonra tüm ikincil veritabanlarının düzeyini düşürme. Veritabanını yükseltirken veya farklı bir hizmet katmanına indirgemeniz durumunda, bu öneri zorlanır.

Bu dizi, daha düşük bir SKU 'daki ikincil öğenin aşırı yüklendiği ve yükseltme veya düşürme işlemi sırasında yeniden hazırlanması gereken sorunlardan kaçınmak için özellikle önerilir. Birincil olarak salt okuma yaparak, tüm okuma/yazma iş yüklerini birincili etkileyen masrafına göre de sorunu önleyebilirsiniz.

> [!NOTE]
> İkincil veritabanını yük devretme grubu yapılandırmasının bir parçası olarak oluşturduysanız, ikincil veritabanının indirgenmesini öneririz. Bu, veri katmanınızın yük devretme etkinleştirildikten sonra düzenli iş yükünüzü işlemek için yeterli kapasiteye sahip olmasını sağlamaktır.

## <a name="preventing-the-loss-of-critical-data"></a>Kritik verilerin kaybını önlemek

Geniş alan ağlarının yüksek gecikmesi nedeniyle, sürekli kopyalama zaman uyumsuz bir çoğaltma mekanizması kullanır. Zaman uyumsuz çoğaltma, bir hata oluşursa, bazı veri kaybını korumasız hale getirir. Ancak, bazı uygulamalar veri kaybı gerektirmez. Bu kritik güncelleştirmeleri korumak için, bir uygulama geliştiricisi işlemi tamamladıktan hemen sonra [sp_wait_for_database_copy_sync](/sql/relational-databases/system-stored-procedures/active-geo-replication-sp-wait-for-database-copy-sync) sistem yordamını çağırabilir. `sp_wait_for_database_copy_sync` çağırmak, son kaydedilen işlem ikincil veritabanına iletilene kadar çağıran iş parçacığını engeller. Ancak, iletilen işlemlerin yeniden çalınmasını ve ikincil üzerinde gerçekleşmesini beklemez. `sp_wait_for_database_copy_sync`, belirli bir sürekli kopyalama bağlantısının kapsamına alınır. Birincil veritabanında bağlantı hakları olan herhangi bir Kullanıcı, bu yordamı çağırabilir.

> [!NOTE]
> `sp_wait_for_database_copy_sync` yük devretmeden sonra veri kaybını önler, ancak okuma erişimi için tam eşitlemeyi garanti etmez. `sp_wait_for_database_copy_sync` yordam çağrısının neden olduğu gecikme önemli olabilir ve çağrı sırasında işlem günlüğünün boyutuna bağlıdır.

## <a name="failover-groups-and-point-in-time-restore"></a>Yük devretme grupları ve zaman içinde bir noktaya geri yükleme

Yük devretme gruplarıyla noktadan noktaya geri yükleme kullanma hakkında daha fazla bilgi için bkz. [Noktadan noktaya kurtarma (sür)](sql-database-recovery-using-backups.md#point-in-time-restore).

## <a name="limitations-of-failover-groups"></a>Yük devretme gruplarının sınırlamaları

Aşağıdaki sınırlamaları unutmayın:

- Yük devretme grubu, aynı Azure bölgelerindeki iki sunucu veya örnek arasında oluşturulamaz.
- Yük devretme grubu yeniden adlandırılamıyor. Grubu silip farklı bir adla yeniden oluşturmanız gerekir. 
- Veritabanı yeniden adlandırma, yük devretme grubundaki örneklerde desteklenmez. Bir veritabanını yeniden adlandırabilmek için yük devretme grubunu geçici olarak silmeniz gerekir.

## <a name="programmatically-managing-failover-groups"></a>Yük devretme gruplarını programlı olarak yönetme

Daha önce anlatıldığı gibi otomatik yük devretme grupları ve etkin coğrafi çoğaltma de Azure PowerShell ve REST API kullanılarak programlı bir şekilde yönetilebilir. Aşağıdaki tablolarda kullanılabilen komut kümesi açıklanır. Etkin coğrafi çoğaltma, [Azure SQL veritabanı REST API](https://docs.microsoft.com/rest/api/sql/) ve [Azure PowerShell cmdlet 'leri](https://docs.microsoft.com/powershell/azure/overview)dahil olmak üzere yönetim için Azure Resource Manager API 'ler kümesi içerir. Bu API 'Ler, kaynak gruplarının kullanımını gerektirir ve rol tabanlı güvenliği (RBAC) destekler. Erişim rollerinin nasıl uygulanacağı hakkında daha fazla bilgi için bkz. [Azure rol tabanlı Access Control](../role-based-access-control/overview.md).

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

### <a name="manage-sql-database-failover-with-single-databases-and-elastic-pools"></a>Tek veritabanları ve elastik havuzlarla SQL veritabanı yük devretmesini yönetme

| Cmdlet | Açıklama |
| --- | --- |
| [New-AzSqlDatabaseFailoverGroup](/powershell/module/az.sql/new-azsqldatabasefailovergroup) |Bu komut bir yük devretme grubu oluşturur ve hem birincil hem de ikincil sunuculara kaydeder|
| [Remove-AzSqlDatabaseFailoverGroup](/powershell/module/az.sql/remove-azsqldatabasefailovergroup) | Yük devretme grubunu sunucudan kaldırır |
| [Get-AzSqlDatabaseFailoverGroup](/powershell/module/az.sql/get-azsqldatabasefailovergroup) | Bir yük devretme grubunun yapılandırmasını alır |
| [Set-AzSqlDatabaseFailoverGroup](/powershell/module/az.sql/set-azsqldatabasefailovergroup) |Bir yük devretme grubunun yapılandırmasını değiştirir |
| [Switch-AzSqlDatabaseFailoverGroup](/powershell/module/az.sql/switch-azsqldatabasefailovergroup) | İkincil sunucuya bir yük devretme grubunun yük devretmesini tetikler |
| [Add-AzSqlDatabaseToFailoverGroup](/powershell/module/az.sql/add-azsqldatabasetofailovergroup)|Bir yük devretme grubuna bir veya daha fazla veritabanı ekler|

### <a name="manage-sql-database-failover-groups-with-managed-instances"></a>Yönetilen örneklerle SQL veritabanı yük devretme gruplarını yönetme

| Cmdlet | Açıklama |
| --- | --- |
| [New-Azsqldatabaseınstancefailovergroup](/powershell/module/az.sql/new-azsqldatabaseinstancefailovergroup) |Bu komut bir yük devretme grubu oluşturur ve hem birincil hem de ikincil örneklere kaydeder|
| [Set-Azsqldatabaseınstancefailovergroup](/powershell/module/az.sql/set-azsqldatabaseinstancefailovergroup) |Bir yük devretme grubunun yapılandırmasını değiştirir|
| [Get-Azsqldatabaseınstancefailovergroup](/powershell/module/az.sql/get-azsqldatabaseinstancefailovergroup) |Bir yük devretme grubunun yapılandırmasını alır|
| [Switch-Azsqldatabaseınstancefailovergroup](/powershell/module/az.sql/switch-azsqldatabaseinstancefailovergroup) |İkincil örneğe yük devretme grubunun yük devretmesini tetikler|
| [Remove-Azsqldatabaseınstancefailovergroup](/powershell/module/az.sql/remove-azsqldatabaseinstancefailovergroup) | Yük devretme grubunu kaldırır|

# <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)

### <a name="manage-sql-database-failover-with-single-databases-and-elastic-pools"></a>Tek veritabanları ve elastik havuzlarla SQL veritabanı yük devretmesini yönetme

| Komut | Açıklama |
| --- | --- |
| [az SQL yük devretme-Grup oluşturma](/cli/azure/sql/failover-group#az-sql-failover-group-create) |Bu komut bir yük devretme grubu oluşturur ve hem birincil hem de ikincil sunuculara kaydeder|
| [az SQL yük devretme-Grup silme](/cli/azure/sql/failover-group#az-sql-failover-group-delete) | Yük devretme grubunu sunucudan kaldırır |
| [az SQL yük devretme-grup göster](/cli/azure/sql/failover-group#az-sql-failover-group-show) | Yük devretme grubu yapılandırmasını alır |
| [az SQL Failover-Group Update](/cli/azure/sql/failover-group#az-sql-failover-group-update) |Bir yük devretme grubunun yapılandırmasını değiştirir ve/veya bir yük devretme grubuna bir veya daha fazla veritabanı ekler|
| [az SQL yük devretme-grup kümesi-birincil](/cli/azure/sql/failover-group#az-sql-failover-group-set-primary) | İkincil sunucuya bir yük devretme grubunun yük devretmesini tetikler |

### <a name="manage-sql-database-failover-groups-with-managed-instances"></a>Yönetilen örneklerle SQL veritabanı yük devretme gruplarını yönetme

| Komut | Açıklama |
| --- | --- |
| [az SQL Instance-yük devretme-Grup oluşturma](/cli/azure/sql/instance-failover-group#az-sql-instance-failover-group-create) | Bu komut bir yük devretme grubu oluşturur ve hem birincil hem de ikincil örneklere kaydeder |
| [az SQL Instance-yük devretme-grup güncelleştirmesi](/cli/azure/sql/instance-failover-group#az-sql-instance-failover-group-update) | Bir yük devretme grubunun yapılandırmasını değiştirir|
| [az SQL Instance-yük devretme-Grup Show](/cli/azure/sql/instance-failover-group#az-sql-instance-failover-group-show) | Bir yük devretme grubunun yapılandırmasını alır|
| [az SQL Instance-yük devretme-grup kümesi-birincil](/cli/azure/sql/instance-failover-group#az-sql-instance-failover-group-set-primary) | İkincil örneğe yük devretme grubunun yük devretmesini tetikler|
| [az SQL Instance-yük devretme-Grup silme](/cli/azure/sql/instance-failover-group#az-sql-instance-failover-group-delete) | Yük devretme grubunu kaldırır |

* * *

> [!IMPORTANT]
> Örnek betik için bkz. [tek bir veritabanı için yük devretme grubunu yapılandırma ve yük devretme](scripts/sql-database-add-single-db-to-failover-group-powershell.md).

### <a name="rest-api-manage-sql-database-failover-groups-with-single-and-pooled-databases"></a>REST API: SQL veritabanı yük devretme gruplarını tek ve havuza alınmış veritabanlarıyla yönetme

| API | Açıklama |
| --- | --- |
| [Yük devretme grubu oluştur veya güncelleştir](https://docs.microsoft.com/rest/api/sql/failovergroups/createorupdate) | Yük devretme grubu oluşturur veya güncelleştirir |
| [Yük devretme grubunu sil](https://docs.microsoft.com/rest/api/sql/failovergroups/delete) | Yük devretme grubunu sunucudan kaldırır |
| [Yük devretme (planlı)](https://docs.microsoft.com/rest/api/sql/failovergroups/failover) | Tam veri eşitlemesi ile geçerli birincil sunucudan ikincil sunucuya yük devretmeyi tetikler.|
| [Yük devretmeyi zorla veri kaybına Izin ver](https://docs.microsoft.com/rest/api/sql/failovergroups/forcefailoverallowdataloss) | Verileri eşitlemeden geçerli birincil sunucudan ikincil sunucuya yük devretmeyi tetikler. Bu işlem, veri kaybına neden olabilir. |
| [Yük devretme grubunu al](https://docs.microsoft.com/rest/api/sql/failovergroups/get) | Bir yük devretme grubunun yapılandırmasını alır. |
| [Yük devretme gruplarını sunucuya göre Listele](https://docs.microsoft.com/rest/api/sql/failovergroups/listbyserver) | Bir sunucudaki yük devretme gruplarını listeler. |
| [Yük devretme grubunu Güncelleştir](https://docs.microsoft.com/rest/api/sql/failovergroups/update) | Bir yük devretme grubunun yapılandırmasını güncelleştirir. |

### <a name="rest-api-manage-failover-groups-with-managed-instances"></a>REST API: yönetilen örneklerle yük devretme gruplarını yönetme

| API | Açıklama |
| --- | --- |
| [Yük devretme grubu oluştur veya güncelleştir](https://docs.microsoft.com/rest/api/sql/instancefailovergroups/createorupdate) | Bir yük devretme grubunun yapılandırmasını oluşturur veya güncelleştirir |
| [Yük devretme grubunu sil](https://docs.microsoft.com/rest/api/sql/instancefailovergroups/delete) | Yük devretme grubunu örnekten kaldırır |
| [Yük devretme (planlı)](https://docs.microsoft.com/rest/api/sql/instancefailovergroups/failover) | Tam veri eşitlemesi ile bu örneğe geçerli birincil örnekten yük devretmeyi tetikler. |
| [Yük devretmeyi zorla veri kaybına Izin ver](https://docs.microsoft.com/rest/api/sql/instancefailovergroups/forcefailoverallowdataloss) | Verileri eşitlemeden geçerli birincil örnekten ikincil örneğe yük devretmeyi tetikler. Bu işlem, veri kaybına neden olabilir. |
| [Yük devretme grubunu al](https://docs.microsoft.com/rest/api/sql/instancefailovergroups/get) | Bir yük devretme grubunun yapılandırmasını alır. |
| [Yük devretme gruplarını listeleme-konuma göre listeleme](https://docs.microsoft.com/rest/api/sql/instancefailovergroups/listbylocation) | Bir konumdaki yük devretme gruplarını listeler. |

## <a name="next-steps"></a>Sonraki adımlar

- Ayrıntılı öğreticiler için bkz.
    - [Yük devretme grubuna tek veritabanı ekleme](sql-database-single-database-failover-group-tutorial.md)
    - [Bir yük devretme grubuna elastik havuz ekleme](sql-database-elastic-pool-failover-group-tutorial.md)
    - [Bir yük devretme grubuna yönetilen örnek ekleme](sql-database-managed-instance-failover-group-tutorial.md)
- Örnek betikler için bkz.:
  - [Azure SQL veritabanı 'nda tek bir veritabanı için etkin Coğrafi çoğaltmayı yapılandırmak üzere PowerShell 'i kullanma](scripts/sql-database-setup-geodr-and-failover-database-powershell.md)
  - [Azure SQL veritabanı 'nda havuza alınmış bir veritabanı için etkin Coğrafi çoğaltmayı yapılandırmak için PowerShell 'i kullanma](scripts/sql-database-setup-geodr-and-failover-pool-powershell.md)
  - [PowerShell kullanarak bir yük devretme grubuna Azure SQL veritabanı tek veritabanı ekleme](scripts/sql-database-add-single-db-to-failover-group-powershell.md)
- İş sürekliliği için genel bakış ve senaryolar için bkz. [iş sürekliliği genel bakış](sql-database-business-continuity.md)
- Azure SQL veritabanı otomatik yedeklemeleri hakkında bilgi edinmek için bkz. [SQL veritabanı otomatik yedeklemeleri](sql-database-automated-backups.md).
- Kurtarmaya yönelik otomatik yedeklemeleri kullanma hakkında bilgi edinmek için bkz. [hizmet tarafından başlatılan yedeklemelerden bir veritabanını geri yükleme](sql-database-recovery-using-backups.md).
- Yeni bir birincil sunucu ve veritabanına yönelik kimlik doğrulama gereksinimleri hakkında bilgi edinmek için, bkz. [olağanüstü durum kurtarma sonrasında SQL veritabanı güvenliği](sql-database-geo-replication-security-config.md).
