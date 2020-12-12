---
title: Uygulama geliştirme en iyi uygulamaları-PostgreSQL için Azure veritabanı
description: PostgreSQL için Azure veritabanı 'nı kullanarak uygulama oluşturmaya yönelik en iyi yöntemler hakkında bilgi edinin.
author: mksuni
ms.author: sumuth
ms.service: postgresql
ms.topic: conceptual
ms.date: 12/10/2020
ms.openlocfilehash: 6463f30bc79d937bd5a51a5c8c78fbdd72954b1e
ms.sourcegitcommit: dfc4e6b57b2cb87dbcce5562945678e76d3ac7b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2020
ms.locfileid: "97364623"
---
# <a name="best-practices-for-building-an-application-with-azure-database-for-postgresql"></a>PostgreSQL için Azure veritabanı ile uygulama oluşturmak için en iyi uygulamalar

PostgreSQL için Azure veritabanı 'nı kullanarak buluta hazır bir uygulama oluşturmanıza yardımcı olacak bazı en iyi yöntemler aşağıda verilmiştir. Bu en iyi uygulamalar, uygulamanız için geliştirme süresini azaltabilir.

## <a name="configuration-of-application-and-database-resources"></a>Uygulama ve veritabanı kaynaklarının yapılandırması

### <a name="keep-the-application-and-database-in-the-same-region"></a>Uygulamayı ve veritabanını aynı bölgede tut
Uygulamanızı Azure 'da dağıttığınızda tüm bağımlılıklarınızın aynı bölgede bulunduğundan emin olun. Bölgeler veya kullanılabilirlik alanları arasında örnekleri yayma, uygulamanızın genel performansını etkileyebilecek ağ gecikmesi oluşturur.

### <a name="keep-your-postgresql-server-secure"></a>PostgreSQL sunucunuzu güvende tutun
PostgreSQL sunucunuzu [güvenli](./concepts-security.md) ve erişilebilir hale getirmek üzere yapılandırın. Sunucunuzun güvenliğini sağlamak için şu seçeneklerden birini kullanın:
- [Güvenlik duvarı kuralları](./concepts-firewall-rules.md)
- [Sanal ağlar](./concepts-data-access-and-security-vnet.md)
- [Azure Özel Bağlantı](./concepts-data-access-and-security-private-link.md)

Güvenlik için, her zaman SSL üzerinden PostgreSQL sunucunuza bağlanmanız ve PostgreSQL sunucunuzu ve uygulamanızı TLS 1,2 kullanacak şekilde yapılandırmanız gerekir. Bkz. [SSL/TLS Yapılandırma](./concepts-ssl-connection-security.md).

### <a name="tune-your-server-parameters"></a>Sunucu parametrelerinizi ayarlayın
Okuma ağır iş yükleri ayarlama sunucu parametreleri için `tmp_table_size` ve `max_heap_table_size` daha iyi performans için iyileştirilmesine yardımcı olabilir. Bu değişkenler için gereken değerleri hesaplamak için, toplam bağlantı başına bellek değerlerine ve temel belleğe bakın. `tmp_table_size`Sunucudaki toplam bellek için temel bellek hesaplarıyla birlikte, hariç olmak üzere, bağlantı başına bellek parametrelerinin toplamı.

### <a name="use-environment-variables-for-connection-information"></a>Bağlantı bilgileri için ortam değişkenlerini kullanma
Veritabanı kimlik bilgilerinizi uygulama kodunuzda kaydetmeyin. Ön uç uygulamasına bağlı olarak, ortam değişkenlerini ayarlamak için yönergeleri izleyin. App Service kullanımı için bkz.[uygulama ayarlarını yapılandırma](../app-service/configure-common.md#configure-app-settings) ve Azure Kuberentes hizmeti için bkz. [Kubernetes gizli dizileri kullanma](https://kubernetes.io/docs/concepts/configuration/secret/).

## <a name="performance-and-resiliency"></a>Performans ve dayanıklılık
İşte performans sorunlarını ayıklamanıza yardımcı olmak için kullanabileceğiniz birkaç araç ve uygulama aşağıda verilmiştir.

### <a name="use-connection-pooling"></a>Bağlantı havuzu kullanma
Bağlantı havuzu oluşturma ile, başlangıçtaki zamanda bir bağlantı kümesi oluşturulur ve korunur. Bu Ayrıca, veritabanı sunucusunda kurulan dinamik yeni bağlantıların neden olduğu sunucudaki bellek parçalanmasını azaltmaya yardımcı olur. Uygulama çerçevesi veya veritabanı sürücüsü destekliyorsa, uygulama tarafında bağlantı havuzu yapılandırılabilir. Bu desteklenmiyorsa, önerilen diğer seçenek, uygulama dışında çalışan [Pgbouncer](https://pgbouncer.github.io/) veya [pgpool](https://pgpool.net/mediawiki/index.php/Main_Page) gibi bir ara sunucu bağlantısı havuzlayıcı hizmetinden faydalanır ve veritabanı sunucusuna bağlanılmamalıdır. Hem PgBouncer hem de Pgpool, PostgreSQL için Azure veritabanı ile çalışan topluluk tabanlı araçlardır.

### <a name="retry-logic-to-handle-transient-errors"></a>Geçici hataları işlemek için yeniden deneme mantığı
Uygulamanız, veritabanı bağlantılarının aralıklı olarak bırakılmakta veya kaybolduğu geçici hatalarla karşılaşabilir. Bu gibi durumlarda, sunucu, 5 ila 10 saniye içinde bir ila iki yeniden denemeden sonra çalışır. İlk yeniden denemeden önce 5 saniye beklemek iyi bir uygulamadır. Sonra, 60 saniyeye kadar yavaş bekleme süresini artırarak her yeniden denemeye uyun. Uygulamanızın işlemi kabul eden en fazla yeniden deneme sayısını sınırlayın, böylece daha fazla araştırma yapabilirsiniz. Daha fazla bilgi için bkz. [bağlantı hatalarıyla ilgili sorunları giderme](./concepts-connectivity.md) .

### <a name="enable-read-replication-to-mitigate-failovers"></a>Yük devretmeleri azaltmak için okuma çoğaltmasını etkinleştirin
Yük devretme senaryolarında [gelen verileri çoğaltma](./concepts-read-replicas.md) kullanabilirsiniz. Okuma çoğaltmaları kullanırken, kaynak ve çoğaltma sunucuları arasında otomatik yük devretme gerçekleşmez. Çoğaltma zaman uyumsuz olduğundan kaynak ve çoğaltma arasında bir gecikme fark edeceksiniz. Ağ gecikmesi, kaynak sunucuda çalışan iş yükünün boyutu ve veri merkezleri arasındaki gecikme süresi gibi birçok faktörden etkilenebilir. Çoğu durumda, çoğaltma gecikme süresi birkaç saniyeden birkaç dakikaya kadar değişir.


## <a name="database-deployment"></a>Veritabanı dağıtımı

### <a name="configure-cicd-deployment-pipeline"></a>CI/CD dağıtım işlem hattını yapılandırma
Bazen değişiklikleri veritabanınıza dağıtmanız gerekir. Bu gibi durumlarda, veritabanında özel bir komut dosyası çalıştırarak veritabanını güncelleştirmek için PostgreSQL sunucunuz için [GitHub eylemleri](https://github.com/Azure/postgresql/blob/master/README.md) aracılığıyla sürekli TÜMLEŞTIRME (CI) kullanabilirsiniz.

### <a name="define-manual-database-deployment-process"></a>El ile veritabanı dağıtım işlemini tanımla
El ile veritabanı dağıtımı sırasında, kapalı kalma süresini en aza indirmek veya başarısız dağıtım riskini azaltmak için aşağıdaki adımları izleyin:

- Pg_dump kullanarak yeni bir veritabanında üretim veritabanının bir kopyasını oluşturun.
- Yeni veritabanını yeni şema değişiklikleri veya veritabanınız için gereken güncelleştirmeler ile güncelleştirin.
- Üretim veritabanını salt bir okuma durumuna getirin. Dağıtım tamamlanana kadar üretim veritabanında yazma işlemlerine sahip olmanız gerekir.
- 1. adımdaki yeni güncelleştirilmiş veritabanıyla uygulamanızı test edin.
- Uygulama değişikliklerinizi dağıtın ve uygulamanın Şu anda en son güncelleştirmelere sahip olan yeni veritabanını kullanmakta olduğundan emin olun.
- Değişiklikleri geri almak için eski üretim veritabanını koruyun. Daha sonra, eski üretim veritabanını silmeyi ya da gerekirse Azure Storage 'a dışarı aktarmayı değerlendirebilirsiniz.

>  [!NOTE]
> Uygulama bir e-ticaret uygulaması gibi olduğunda ve salt okuma durumunda yer alıyorsa, bir yedekleme yaptıktan sonra değişiklikleri doğrudan üretim veritabanına dağıtın. Bazı kullanıcılar başarısız isteklerle karşılaşabileceğinden, etkiyi en aza indirmek için uygulamanın düşük trafiğiyle yoğun olmayan saatlerde gerçekleşmelidir. Uygulama kodunuzun başarısız istekleri de işlediğinizden emin olun.

## <a name="database-schema-and-queries"></a>Veritabanı şeması ve sorguları
Veritabanı şemanızı ve sorgularınızı oluştururken aklınızda bulundurmanız gereken birkaç ipucu aşağıda verilmiştir.

### <a name="use-bigint-or-uuid-for-primary-keys"></a>Birincil anahtarlar için BIGINT veya UUID kullanın
Özel uygulama veya bazı çerçeveler oluştururken `INT` , `BIGINT` birincil anahtarlar için kullanmak yerine. Kullandığınızda ```INT``` , veritabanınızdaki değerin veri türünün depolama kapasitesini aşabileceği riski görürsünüz ```INT``` . Bu değişikliğin mevcut bir üretim uygulamasında yapılması, maliyet daha fazla geliştirme süresi ile zaman alabilir. Diğer bir seçenek ise birincil anahtarlar için [UUID](https://www.postgresql.org/docs/current/datatype-uuid.html) kullanmaktır. Bu tanımlayıcı, örneğin, otomatik olarak oluşturulan bir 128 bitlik dize kullanır ```a0eebc99-9c0b-4ef8-bb6d-6bb9bd380a11``` . [PostgreSQL veri türleri](https://www.postgresql.org/docs/8.1/datatype.html)hakkında daha fazla bilgi edinin.

### <a name="use-indexes"></a>Dizinleri kullanma

Postgres 'de farklı şekillerde kullanılabilen birçok [Dizin](https://www.postgresql.org/docs/9.1/indexes.html) türü vardır. Bir dizin kullanmak, sunucunun bir dizin olmadan yapabileceğinden belirli satırları daha hızlı bulmasına ve almasına yardımcı olur. Ancak dizinler ayrıca veritabanı sunucusuna ek yük ekler, bu nedenle çok sayıda dizin kullanmaktan kaçının.

### <a name="use-autovacuum"></a>Oto vakum kullanın
PostgreSQL için Azure veritabanı sunucusu üzerinde sunucunuzu yeniden iyileştireleyebilirsiniz. PostgreSQL, daha fazla veritabanı eşzamanlılık sağlar ancak her güncelleştirme, INSERT ve delete ile sonuçlanır. Silme için kayıtlar, daha sonra temizlenecek geçici olarak işaretlenir. Bu görevleri gerçekleştirmek için PostgreSQL bir vakum işi çalıştırır. Zaman zaman Vakum yoksa, biriken atılacak tanımlama grupları şu şekilde olabilir:

- Daha büyük veritabanları ve tablolar gibi veri blobunun.
- Daha büyük olan alt dizinler.
- Artan g/ç.

[Oto vakum ile iyileştirme](howto-optimize-autovacuum.md)hakkında daha fazla bilgi edinin.

### <a name="use-pg_stats_statements"></a>Pg_stats_statements kullan
Pg_stat_statements, PostgreSQL için Azure veritabanı 'nda varsayılan olarak etkinleştirilen bir PostgreSQL uzantısıdır. Uzantı, bir sunucu tarafından yürütülen tüm SQL deyimlerinin yürütme istatistiklerini izlemek için bir yol sağlar. Bkz. [pg_statement kullanma](howto-optimize-query-stats-collection.md).


### <a name="use-the-query-store"></a>Sorgu deposunu kullanma
PostgreSQL için Azure veritabanı 'ndaki [sorgu depolama](./concepts-query-store.md) özelliği sorgu istatistiklerini izlemek için daha etkili bir yöntem sağlar. Bu özelliği pg_stats_statements kullanımına alternatif olarak öneririz.

### <a name="optimize-bulk-inserts-and-use-transient-data"></a>Toplu eklemeleri iyileştirme ve geçici verileri kullanma
Geçici verileri içeren veya büyük veri kümelerini toplu olarak ekleyen iş yükü işlemleriniz varsa, günlüğe kaydedilen tabloları kullanmayı göz önünde bulundurun. Varsayılan olarak, kararlılık ve dayanıklılık sağlar. Atomicity, tutarlılık, yalıtım ve dayanıklılık ACID özelliklerini yapar. Bkz. [toplu eklemeleri iyileştirme](howto-optimize-bulk-inserts.md).

## <a name="next-steps"></a>Sonraki Adımlar
[Postgres Kılavuzu](http://postgresguide.com/)
