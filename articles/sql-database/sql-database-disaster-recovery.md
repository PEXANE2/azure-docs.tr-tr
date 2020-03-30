---
title: Olağanüstü durum kurtarma
description: Azure SQL Veritabanı etkin coğrafi çoğaltma ve coğrafi geri yükleme özellikleriyle birlikte bölgesel veri merkezi kesintisinden veya hatasından veritabanını nasıl kurtarılayarıp kurtarılamayı öğrenin.
services: sql-database
ms.service: sql-database
ms.subservice: high-availability
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: mathoma, carlrab
ms.date: 06/21/2019
ms.openlocfilehash: d28edd28dcbe31bfe63c2d0a9c3e975967efef04
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79256386"
---
# <a name="restore-an-azure-sql-database-or-failover-to-a-secondary"></a>Azure SQL Veritabanı'nı geri yükleme veya ikincil bir veritabanına başarısız

Azure SQL Veritabanı, kesintiden kurtarma için aşağıdaki özellikleri sunar:

- [Etkin coğrafi çoğaltma](sql-database-active-geo-replication.md)
- [Otomatik yük devretme grupları](sql-database-auto-failover-group.md)
- [Coğrafi Geri Yükleme](sql-database-recovery-using-backups.md#point-in-time-restore)
- [Bölge yedekli veritabanları](sql-database-high-availability.md)

İş sürekliliği senaryoları ve bu senaryoları destekleyen özellikler hakkında bilgi edinmek için İş [sürekliliği'ne](sql-database-business-continuity.md)bakın.

> [!NOTE]
> Bölge gereksiz Premium veya İş Açısından Kritik veritabanları veya havuzları kullanıyorsanız, kurtarma işlemi otomatiktir ve bu malzemenin geri kalanı geçerli değildir.

> [!NOTE]
> Hem birincil hem de ikincil veritabanlarının aynı hizmet katmanına sahip olması gerekir. Ayrıca, ikincil veritabanının birincil veritabanıyla aynı işlem boyutuyla (DTUs veya vCores) oluşturulması da önerilir. Daha fazla bilgi [Upgrading or downgrading as primary database](sql-database-active-geo-replication.md#upgrading-or-downgrading-primary-database)için bkz.

> [!NOTE]
> Birden çok veritabanının başarısız olmasını yönetmek için bir veya birkaç başarısız grup kullanın.
> Başarısız gruba varolan bir coğrafi çoğaltma ilişkisi eklerseniz, jeo-ikincil işlemin birincil le aynı hizmet katmanı ve bilgi işlem boyutuyla yapılandırıldığından emin olun. Daha fazla bilgi için [bkz.](sql-database-auto-failover-group.md)

## <a name="prepare-for-the-event-of-an-outage"></a>Kesinti olayına hazırlanın

Failover grupları veya coğrafi yedekli yedeklemeler kullanarak başka bir veri bölgesine kurtarma ile başarı için, ihtiyaç duyulması yanı sıra iyi tanımlanmış adımlar belgelenmiş olması durumunda yeni birincil sunucu olmak için başka bir veri merkezi kesintisi bir sunucu hazırlamak gerekir ve sorunsuz bir iyileşme sağlamak için test edilmiştir. Bu hazırlık adımları şunlardır:

- Yeni birincil sunucu olmak için başka bir bölgedeki SQL Veritabanı sunucusunu tanımlayın. Coğrafi geri yükleme için bu genellikle veritabanınızın bulunduğu bölge için [eşleştirilmiş bölgede](../best-practices-availability-paired-regions.md) bir sunucudur. Bu, coğrafi geri alma işlemleri sırasında ek trafik maliyetini ortadan kaldırır.
- Kullanıcıların yeni birincil veritabanına erişebilmesi için gereken sunucu düzeyindeki IP güvenlik duvarı kurallarını tanımlayın ve isteğe bağlı olarak tanımlayın.
- Bağlantı dizelerini değiştirerek veya DNS girişlerini değiştirerek kullanıcıları yeni birincil sunucuya nasıl yönlendireceğinizi belirleyin.
- Yeni birincil sunucudaki ana veritabanında bulunması gereken girişleri tanımlayın ve isteğe bağlı olarak oluşturun ve varsa bu girişlerin ana veritabanında uygun izinlere sahip olduğundan emin olun. Daha fazla bilgi [için, olağanüstü durum kurtarma sonrası SQL Veritabanı güvenliğine](sql-database-geo-replication-security-config.md) bakın
- Yeni birincil veritabanına eşlenmek üzere güncellenmesi gereken uyarı kurallarını tanımlayın.
- Denetim yapılandırmasını geçerli birincil veritabanında belgeleme
- Bir [felaket kurtarma matkap](sql-database-disaster-recovery-drills.md)gerçekleştirin. Coğrafi geri yükleme için bir kesinti simüle etmek için, uygulama bağlantısı hatasına neden olmak için kaynak veritabanını silebilir veya yeniden adlandırabilirsiniz. Hata grupları kullanarak bir kesintisi simüle etmek için, web uygulamasını veya veritabanına bağlı sanal makineyi devre dışı kaldıramaz veya uygulama bağlantısı hatalarına neden olmak için veritabanıüzerinde başarısız olabilirsiniz.

## <a name="when-to-initiate-recovery"></a>Kurtarma ne zaman başlatılır?

Kurtarma işlemi uygulamayı etkiler. DNS kullanarak SQL bağlantı dizesini veya yeniden yönlendirmeyi gerektirir ve kalıcı veri kaybına neden olabilir. Bu nedenle, kesintinin uygulamanızın kurtarma süresi hedefinden daha uzun sürmesi olası olduğunda yapılmalıdır. Uygulama üretime dağıtıldığında, uygulama durumunu düzenli olarak izlemeli ve kurtarmanın garanti edildiğini iddia etmek için aşağıdaki veri noktalarını kullanmalısınız:

1. Uygulama katmanından veritabanına kalıcı bağlantı hatası.
2. Azure portalı, bölgede geniş etkili bir olayla ilgili bir uyarı gösterir.

> [!NOTE]
> Başarısız grupları kullanıyorsanız ve otomatik hata yı seçtiyseniz, kurtarma işlemi otomatik ve uygulamada saydamdır.

Uygulamanızın kapalı kalma süresine ve olası iş yükümlülüğüne bağlı olarak aşağıdaki kurtarma seçeneklerini göz önünde bulundurabilirsiniz.

En son Coğrafi olarak çoğaltılan geri yükleme noktasını almak için [Kurtarılabilir Veritabanını](https://msdn.microsoft.com/library/dn800985.aspx) *(LastAvailableBackupDate)* kullanın.

## <a name="wait-for-service-recovery"></a>Hizmet kurtarma için bekleyin

Azure ekipleri, hizmet kullanılabilirliğini mümkün olduğunca çabuk geri yüklemek için özenle çalışır, ancak temel nedene bağlı olarak saatler veya günler sürebilir.  Uygulamanız önemli kapalı kalma sürelerine tahammül edemiyorsa, kurtarmanın tamamlanmasını bekleyebilirsiniz. Bu durumda, sizin tarafınızdan herhangi bir işlem yapılması gerekmez. [Azure Hizmet Durumu Panosumuzda](https://azure.microsoft.com/status/)geçerli hizmet durumunu görebilirsiniz. Bölgenin kurtarılmasından sonra, uygulamanızın kullanılabilirliği geri yüklenir.

## <a name="fail-over-to-geo-replicated-secondary-server-in-the-failover-group"></a>Failover grubunda coğrafi olarak çoğaltılan ikincil sunucuya geçeme

Uygulamanızın kapalı kalma süresi iş sorumluluğuna neden olabilirse, başarısız gruplar kullanıyor olmalısınız. Bir kesinti durumunda uygulamanın farklı bir bölgede kullanılabilirliği hızla geri yüklemesini sağlar. Öğretici için [bkz.](sql-database-implement-geo-distributed-database.md)

Veritabanı(lar) kullanılabilirliğini geri yüklemek için desteklenen yöntemlerden birini kullanarak ikincil sunucuya failover başlatmanız gerekir.

Coğrafi olarak çoğaltılan ikincil veritabanında başarısız olmak için aşağıdaki kılavuzlardan birini kullanın:

- [Azure portalını kullanarak coğrafi olarak çoğaltılan ikincil sunucuda başarısız olun](sql-database-geo-replication-portal.md)
- [PowerShell'i kullanarak ikincil sunucuda başarısız](scripts/sql-database-setup-geodr-and-failover-database-powershell.md)
- [Transact-SQL (T-SQL) kullanarak ikincil bir sunucuya geçe](/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-current#e-failover-to-a-geo-replication-secondary)

## <a name="recover-using-geo-restore"></a>Coğrafi geri yükleme yi kullanarak kurtarma

Uygulamanızın kapalı kalma süresi iş yükümlülüğüne yol alamazsa, uygulama veritabanınızı kurtarmak için [coğrafi geri yükleme](sql-database-recovery-using-backups.md) yöntemi olarak kullanabilirsiniz. Veritabanının en son coğrafi yedekli yedeklemesinden bir kopyasını oluşturur.

## <a name="configure-your-database-after-recovery"></a>Kurtarma dan sonra veritabanınızı yapılandırma

Bir kesintiden kurtarmak için coğrafi geri yükleme kullanıyorsanız, normal uygulama işlevinin sürdürülebilmesini sağlamak için yeni veritabanlarına bağlantının düzgün şekilde yapılandırıldığından emin olmalısınız. Bu, kurtarılan veritabanı üretiminizi hazır duruma getirmek için görevlerin bir denetim listesidir.

### <a name="update-connection-strings"></a>Bağlantı dizelerini güncelleştirme

Kurtarılan veritabanınız farklı bir sunucuda bulunduğundan, uygulamanızın bağlantı dizesini bu sunucuya işaret etmek için güncelleştirmeniz gerekir.

Bağlantı dizelerini değiştirme hakkında daha fazla bilgi için [bağlantı kitaplığınız](sql-database-libraries.md)için uygun geliştirme dilini görün.

### <a name="configure-firewall-rules"></a>Güvenlik Duvarı Kurallarını Yapılandırma

Sunucuda ve veritabanında yapılandırılan güvenlik duvarı kurallarının birincil sunucu ve birincil veritabanında yapılandırılankurallarla eşleştirdiğinden emin olmanız gerekir. Daha fazla bilgi için [bkz: Güvenlik Duvarı Ayarlarını Yapılandırma (Azure SQL Veritabanı)](sql-database-configure-firewall-settings.md).

### <a name="configure-logins-and-database-users"></a>Oturum açma ve veritabanı kullanıcılarını yapılandırma

Uygulamanız tarafından kullanılan tüm oturum açmaların kurtarılan veritabanınızı barındıran sunucuda olduğundan emin olmanız gerekir. Daha fazla bilgi [için, coğrafi çoğaltma için Güvenlik](sql-database-geo-replication-security-config.md)Yapılandırması'na bakın.

> [!NOTE]
> Bir olağanüstü durum kurtarma matkabı sırasında sunucu güvenlik duvarı kurallarınızı ve oturum açmalarınızı (ve izinlerini) yapılandırmalı ve test etmelidir. Bu sunucu düzeyindeki nesneler ve yapılandırmaları kesinti sırasında kullanılamayabilir.

### <a name="setup-telemetry-alerts"></a>Kurulum telemetri uyarıları

Varolan uyarı kuralı ayarlarınızın kurtarılan veritabanına ve farklı sunucuyla eşlenecek şekilde güncelleştirdiğinden emin olmanız gerekir.

Veritabanı uyarı kuralları hakkında daha fazla bilgi için Uyarı [Bildirimleri Al](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) ve [Hizmet Durumunu Takip Etme'ye](../monitoring-and-diagnostics/insights-service-health.md)bakın.

### <a name="enable-auditing"></a>Denetimi etkinleştirme

Veritabanınıza erişmek için denetim gerekiyorsa, veritabanı kurtarma sonrasında Denetimi etkinleştirmeniz gerekir. Daha fazla bilgi için [Veritabanı denetimine](sql-database-auditing.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar

- Azure SQL Veritabanı otomatik yedeklemeleri hakkında bilgi edinmek için [SQL Veritabanı otomatik yedeklemelerine](sql-database-automated-backups.md) bakın
- İş sürekliliği tasarımı ve kurtarma senaryoları hakkında bilgi edinmek için [bkz.](sql-database-business-continuity.md)
- Kurtarma için otomatik yedeklemekullanma hakkında bilgi edinmek [için, hizmet tarafından başlatılan yedeklemelerden bir veritabanını geri yükleme](sql-database-recovery-using-backups.md)
