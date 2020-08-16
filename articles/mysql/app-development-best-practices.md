---
title: Uygulama geliştirme en iyi uygulamaları-MySQL için Azure veritabanı
description: MySQL için Azure veritabanı ile uygulama oluştururken en iyi yöntemler hakkında bilgi edinin
author: mksuni
ms.author: sumuth
ms.service: mysql
ms.topic: conceptual
ms.date: 08/11/2020
ms.openlocfilehash: 36f9cfa2369032351f6ed2948fc0c98c1648bcb6
ms.sourcegitcommit: ef055468d1cb0de4433e1403d6617fede7f5d00e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2020
ms.locfileid: "88259259"
---
# <a name="best-practices-for-building-applications-with-azure-database-for-mysql"></a>MySQL için Azure veritabanı ile uygulama oluşturmak için en iyi uygulamalar 

İşte uygulamanızın geliştirme süresini azaltabilmek için Azure veritabanı 'nı kullanarak buluta hazır uygulamalar oluşturmaya yardımcı olmak için bazı en iyi yöntemler aşağıda verilmiştir. 

## <a name="application-and-database-resource-configuration"></a>Uygulama ve veritabanı kaynak yapılandırması

### <a name="application-and-database-in-the-same-region"></a>Aynı bölgedeki uygulama ve veritabanı
Uygulamanızı Azure 'da dağıttığınızda **Tüm bağımlılıklarınızın aynı bölgede**  bulunduğundan emin olun. Bölgeler veya kullanılabilirlik alanları arasında örnekleri yayma, uygulamanızın genel performansını etkileyebilecek ağ gecikmesi oluşturur. 

### <a name="keep-your-mysql-server-secure"></a>MySQL sunucunuzu güvende tutun
MySQL sunucunuz [güvenli](https://docs.microsoft.com/azure/mysql/concepts-security) ve erişilebilir olacak şekilde yapılandırılmalıdır. Sunucunuzun güvenliğini sağlamak için şu seçeneklerden birini kullanın: 
- [Güvenlik duvarı kuralları](https://docs.microsoft.com/azure/mysql/concepts-firewall-rules) veya
- [Sanal ağlar](https://docs.microsoft.com/azure/mysql/concepts-data-access-and-security-vnet) veya 
- [Özel Bağlantı](https://docs.microsoft.com/azure/mysql/concepts-data-access-security-private-link)

Güvenlik için, her zaman **SSL** üzerinden MySQL sunucunuza bağlanmanız ve MySQL sunucunuzu ve uygulamanızı **TLS 1.2**kullanacak şekilde yapılandırmanız gerekir. Bkz. [SSL/TLS Yapılandırma](https://docs.microsoft.com/azure/mysql/concepts-ssl-connection-security). 

### <a name="tune-your-server-parameters"></a>Sunucu parametrelerinizi ayarlayın
Bu parametreleri ayarlama, okuma ağır iş yükleri için, ' tmp_table_size ve max_heap_table_size ' daha iyi performans için iyileştirilmesine yardımcı olabilir. Tmp_table_size ve max_heap_table_size için gereken değerleri hesaplamak için, toplam bağlantı başına bellek değerlerine ve temel belleğe bakın. Sunucu başına toplam bellek için temel bellek hesaplarıyla birlikte tmp_table_size hariç olmak üzere, bağlantı başına bellek parametrelerinin toplamı.

Tmp_table_size ve max_heap_table_size en büyük olası boyutunu hesaplamak için aşağıdaki formülü kullanın:

```(total memory - (base memory + (sum of per-connection memory * # of connections)) / # of connections```

>[!NOTE]
> Toplam bellek, sunucunun sağlanan sanal çekirdekler üzerinde sahip olduğu toplam bellek miktarını gösterir.  Örneğin, Genel Amaçlı 2 sanal çekirdek MySQL için Azure veritabanı sunucusunda toplam bellek 5 GB * 2 olacaktır.  [Fiyatlandırma katmanı](https://docs.microsoft.com/azure/mysql/concepts-pricing-tiers) belgelerinde her bir katmanın belleği hakkında daha fazla ayrıntı bulunabilir.
> Taban bellek, query_cache_size ve innodb_buffer_pool_size gibi, MySQL 'in sunucu başlangıcında başlatılacağı ve ayıracağını belirten bellek değişkenlerini belirtir.  Sort_buffer_size ve join_buffer_size gibi bağlantı belleği başına, yalnızca bir sorgu gerektirdiğinde ayrılan bellektir.

### <a name="create-a-non-admin-user"></a>Yönetici olmayan kullanıcı oluşturma 
Veritabanlarının her biri için [yönetici olmayan kullanıcılar oluşturun](https://docs.microsoft.com/azure/mysql/howto-create-users) . Genellikle, Kullanıcı adları VERITABANı adları olarak tanımlanır.

### <a name="resetting-your-password"></a>Parolanızı sıfırlama
Azure portal kullanarak MySQL sunucunuzun [parolasını sıfırlayabilirsiniz](https://docs.microsoft.com/azure/mysql/howto-create-manage-server-portal#update-admin-password) . 

Bir üretim veritabanı için sunucu parolanızı sıfırlama, uygulamanızı getirebilir. Uygulama son kullanıcılarınızın etkisini en aza indirmek için, tüm üretim iş yükleri için parolayı yoğun olmayan saatlerde sıfırlamak iyi bir modeldir.

## <a name="performance-and-resiliency"></a>Performans ve dayanıklılık 
Uygulamanızda performans sorunlarını ayıklamanıza yardımcı olmak için kullanabileceğiniz birkaç araç ve desen aşağıda verilmiştir.

### <a name="enable-slow-query-logs-identify-performance-issues"></a>Yavaş sorgu günlüklerini etkinleştirme performans sorunlarını tanımla
Sunucunuzdaki [yavaş sorgu günlüklerini](https://docs.microsoft.com/azure/mysql/concepts-server-logs) ve [Denetim günlüklerini](https://docs.microsoft.com/azure/mysql/concepts-audit-logs) etkinleştirebilirsiniz. Yavaş sorgu günlüklerinin çözümlenmesi, sorun giderme için performans sorunlarını belirlemenize yardımcı olabilir. 

Denetim günlükleri Ayrıca Azure Izleyici günlükleri, Event Hubs ve depolama hesabı 'ndaki Azure tanılama günlükleri aracılığıyla da kullanılabilir. Bkz. [sorgu performans sorunlarını giderme](https://docs.microsoft.com/azure/mysql/howto-troubleshoot-query-performance).

### <a name="use-connection-pooling"></a>Bağlantı havuzu kullanma
Veritabanı bağlantılarını yönetmek, uygulamanın performansı üzerinde önemli bir etkiye sahip olabilir. Performansı en iyi duruma getirmek için, anahtar kodu yollarında bağlantıların oluşturulması ve zaman sayısını azaltmanız gerekir.  Esneklik ve performansı geliştirmek için MySQL için Azure veritabanı 'na bağlanmak üzere [bağlantı havuzunu](https://docs.microsoft.com/azure/mysql/concepts-connectivity#access-databases-by-using-connection-pooling-recommended) kullanın. 

Bağlantı havuzlayıcı olan [Proxysql](https://proxysql.com/), bağlantıları yönetmek için etkili bir şekilde kullanılabilir. Bağlantı havuzlayıcı kullanılması, boştaki bağlantıları azaltabilir ve mevcut bağlantıları yeniden kullanabilir sorunlardan kaçınmak için yardımcı olur. Daha fazla bilgi edinmek için bkz. [ProxySQL 'i ayarlama](https://techcommunity.microsoft.com/t5/azure-database-for-mysql/connecting-efficiently-to-azure-database-for-mysql-with-proxysql/ba-p/1279842) . 

### <a name="retry-logic-to-handle-transient-errors"></a>Geçici hataları işlemek için yeniden deneme mantığı
Uygulamanız, veritabanı bağlantılarının aralıklı olarak bırakılmakta veya kaybolduğu durumlarda [geçici hatalarla](https://docs.microsoft.com/azure/mysql/concepts-connectivity#handling-transient-errors) karşılaşabilir. Bu gibi durumlarda, 5-10 saniye içinde bir veya iki yeniden denemeden sonra sunucu çalışır ve çalışır. 

Yeniden deneme ile izlenecek iyi bir model, ilk yeniden denemeden önce 5 saniye bekleyip her bir yeniden denemeye sonra, yavaş bir şekilde 60 saniyeye kadar bekleyecektir. Uygulamanızın işlemi kabul eden en fazla yeniden deneme sayısını sınırlayın, böylece daha fazla araştırma yapabilirsiniz. Daha fazla bilgi için bkz. [bağlantı hatalarıyla ilgili sorunları giderme](https://docs.microsoft.com/azure/mysql/howto-troubleshoot-common-connection-issues) . 

### <a name="enable-read-replication-to-mitigate-failovers"></a>Yük devretmeleri azaltmak için okuma çoğaltmasını etkinleştirin
Yük devretme senaryoları için [verileri çoğaltma '](https://docs.microsoft.com/azure/mysql/howto-data-in-replication) yı kullanabilirsiniz. Okuma çoğaltmaları kullanılırken, ana ve çoğaltma sunucuları arasında otomatik yük devretme gerçekleşmez. Çoğaltma zaman uyumsuz olduğundan, ana öğe ve çoğaltma arasında bir gecikme fark edeceksiniz. Ağ gecikmesi, ana sunucuda çalışan iş yükünün ne kadar ağır ve veri merkezleri arasındaki gecikme süresi gibi birçok faktörden etkilenebilir. Çoğu durumda, çoğaltma gecikmesi birkaç saniye ile birkaç dakika arasında değişir.

## <a name="database-deployment"></a>Veritabanı dağıtımı 

### <a name="configure-azure-database-for-mysql-task-in-your-cicd-deployment-pipeline"></a>CI/CD dağıtım işlem hattınızda MySQL için Azure veritabanı görevini yapılandırma
Bazen değişiklikleri veritabanınıza dağıtmanız gerekir. Bu gibi durumlarda, [Azure işlem hatları](https://azure.microsoft.com/services/devops/pipelines/) aracılığıyla sürekli TÜMLEŞTIRME (CI) ve sürekli teslım (CD) kullanabilirsiniz ve veritabanınıza özel bir komut dosyası çalıştırarak veritabanını güncelleştirmek için [MySQL sunucunuz](https://docs.microsoft.com/azure/devops/pipelines/tasks/deploy/azure-mysql-deployment?view=azure-devops) için bir görev kullanabilirsiniz.

### <a name="manual-database-deployment"></a>El ile veritabanı dağıtımı 
El ile veritabanı dağıtımı sırasında, kapalı kalma süresini en aza indirmek veya başarısız dağıtım riskini azaltmak için izlenecek iyi bir örüntü aşağıda verilmiştir: 

1. Üretim veritabanının bir kopyasını [mysqldump](https://dev.mysql.com/doc/refman/8.0/en/mysqldump.html) veya [MySQL çalışma ekranı](https://dev.mysql.com/doc/workbench/en/wb-admin-export-import-management.html) kullanarak yeni bir veritabanında oluşturun 
2. Yeni veritabanını yeni şema değişiklikleri veya veritabanınız için gereken güncelleştirmeler ile güncelleştirin. 
3. Üretim veritabanını salt okuma durumuna getirin. Dağıtım tamamlanana kadar üretim veritabanında yazma işlemlerine sahip olmanız gerekir. 
4. 1. adımdaki yeni güncelleştirilmiş veritabanıyla uygulamanızı test edin.
5. Uygulama değişikliklerinizi dağıtın ve uygulamanın Şu anda en son güncelleştirmelere sahip olan yeni veritabanını kullanmakta olduğundan emin olun. 
6. Değişiklikleri geri almak için eski üretim veritabanını koruyun. Daha sonra, eski üretim veritabanını silmeyi ya da gerekirse Azure Storage 'a dışarı aktarmayı değerlendirebilirsiniz. 

>[!NOTE]
>  - Uygulama, salt okuma durumuna geçirebileceğiniz e-ticaret uygulamasına benziyorsa, bir yedekleme yaptıktan sonra değişiklikleri doğrudan üretim veritabanında dağıtın.  Bazı kullanıcılar başarısız bir istek yaşabileceğinden etkiyi minimze için en yoğun saatlerde, uygulamanın düşük trafiğiyle büyük bir değişiklik gerçekleşmelidir. 
>  - Uygulama kodunuzun başarısız istekleri de işlediğinizden emin olun.

### <a name="use-mysql-native-metrics-to-see-if-your-workload-is-exceeding-in-memory-temporary-table-sizes"></a>İş yükünüzün bellek içi geçici tablo boyutlarını aşıp aşmadığından bakmak için MySQL yerel ölçümlerini kullanın
Okuma ağır iş yüküyle, MySQL sunucunuza karşı yürütülen sorgular bellek içi geçici tablo boyutlarını aşabilir. Bu, sunucunuzun, uygulamanızın performansını etkileyen geçici tabloları diske yazma anahtarına geçmesine neden olabilir. Sunucunuzun geçici tablo boyutunu aşarak diske yazmadığını öğrenmek için aşağıdaki ölçümlere bakın:

```
show global status like 'created_tmp_disk_tables';
show global status like 'created_tmp_tables';
```
Created_tmp_disk_tables ölçümü diskte kaç tablo oluşturulduğunu gösterir ve created_tmp_table ölçümü, iş yükünüze göre bellekte kaç tane geçici tablo biçimlendirilmesini söyler. Belirli bir sorgu çalıştırmanın geçici tablolar kullanıp kullanmadığını anlamak için sorguda açıkla ' yı çalıştırın. ' Ekstra ' sütunundaki ayrıntı, sorgu geçici tablolar kullanılarak çalıştırılacağından ' geçici ' Using ' i gösterir.

Sorgular disklere sığmayan iş yükünüzün yüzdesini hesaplamak için aşağıdaki formülde ölçüm değerlerinizi kullanın:

```(created_tmp_disk_tables / (created_tmp_disk_tables + created_tmp_tables)) * 100```

İdeal olarak, bu yüzde 25 ' in az olması gerekir. Yüzdenin %25 veya daha büyük olduğunu görürseniz, tmp_table_size ve max_heap_table_si iki sunucu parametresini değiştirmeyi öneririz


## <a name="database-schema-and-queries"></a>Veritabanı şeması ve sorguları

Veritabanı şemanızı ve sorgularınızı oluştururken aklınızda bulundurmanız gereken birkaç ipucu ve püf noktası aşağıda verilmiştir.

### <a name="always-use-the-right-datatype-for-your--table-columns"></a>Tablo sütunlarınız için her zaman doğru veri türünü kullanın
Depolamak istediğiniz verilerin türüne göre doğru veri türlerini kullanmak depolamayı iyileştirebilirler ve hatalı veri türleri nedeniyle oluşabilecek hataları azaltabilir.

### <a name="use-indexes"></a>Dizinleri kullanma
Yavaş sorgulardan kaçınmak için dizinleri kullanabilirsiniz. Dizinler, belirli sütunlara hızlıca sahip satırları bulmaya yardımcı olabilir. Bkz. [MySQL 'de dizinleri kullanma](https://dev.mysql.com/doc/refman/8.0/en/mysql-indexes.html).

### <a name="explain-your-select-queries"></a>SEÇIM sorgularınızı açıklayın
Sorgunuzu yürütmek için MySQL ile ilgili Öngörüler elde etmek için [açıkla](https://dev.mysql.com/doc/refman/8.0/en/explain.html) ' yı kullanın. Bu, Sorgunuzla sorunları veya sorunları tespit etmenize yardımcı olabilir. Bkz. [Açıklama kullanarak sorgu performansını profil](https://docs.microsoft.com/azure/mysql/howto-troubleshoot-query-performance)oluşturma.


