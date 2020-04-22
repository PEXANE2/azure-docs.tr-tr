---
title: Cassandra için Azure Cosmos DB API ile ilgili sık sorulan sorular.
description: Cassandra için Azure Cosmos DB API hakkında sık sorulan soruların yanıtlarını alın.
author: TheovanKraay
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 04/09/2020
ms.author: thvankra
ms.openlocfilehash: 416f0c5f995a101298e84c81317c7d39fb5d43f8
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81727391"
---
# <a name="frequently-asked-questions-about-the-azure-cosmos-db-api-for-cassandra"></a>Cassandra için Azure Cosmos DB API hakkında sık sorulan sorular

## <a name="what-are-some-key-differences-between-apache-cassandra-and-cassandra-api"></a>Apache Cassandra ve Cassandra API arasındaki bazı önemli farklar nelerdir?

- Apache Cassandra bir bölüm anahtarı boyutu nda 100MB sınırı önerir. Cassandra API bölüm başına 10 GB'a kadar izin verir.
- Apache Cassandra dayanıklı taahhütleri devre dışı bırakabilirsiniz - yani taahhüt günlüğüne yazmayı atlayın ve doğrudan Memtable(lar) gidin. Memtables diskteki SStables'a atılmadan önce düğüm inerse, bu durum veri kaybına neden olabilir. Cosmos DB her zaman kalıcı işler yapar, böylece veri kaybı asla.
- Apache Cassandra, iş yükü çok fazla değiştirme ve/veya silme içeriyorsa performansın azaldığını görebilir. Bunun nedeni, okunan iş yükünün en son verileri almak için atlaması gereken mezar taşlarıdır. Cassandra API, iş yükü çok fazla değiştirme ve/veya sildiğinde okuma performansının azaldığını görmez.
- Yüksek değiştirme iş yükü senaryoları sırasında, sıkıştırma diskte SSTables birleştirmek için çalışması gerekir (Apache Cassandra'nın yazmaları yalnızca ek olduğundan birleştirme gereklidir, bu nedenle birden çok güncelleştirme periyodik olarak birleştirilmesi gereken tek tek SSTable girişleri olarak depolanır). Bu da sıkıştırma sırasında düşük okuma performansına yol açabilir. Sıkıştırma uygulamadığı için cassandra API'de bu durum oluşmaz.
- Apache Cassandra ile 1 çoğaltma faktörü ayarlamak mümkündür. Ancak, verilerle ilgili tek düğüm aşağı inerse, düşük kullanılabilirlik durumuna yol açar. Bu, Azure Cosmos DB Cassandra API ile ilgili bir sorun değildir, çünkü her zaman 4'lük bir çoğaltma faktörü vardır (3'ün çoğunluğu).
- Apache Cassandra'daki düğümlerin eklenmesi/çıkarılması çok sayıda manuel müdahale gerektirir, ancak varolan düğümler bazı belirteç aralıklarını yeni düğüme taşırken yeni düğümüzerinde de yüksek işlemci gerektirir. Bu, varolan bir düğümü devre dışı bırakmakta da aynıdır. Ancak, ölçekleme, hizmet/uygulamada herhangi bir sorun gözlenmeden Azure Cosmos DB Cassandra API'de kaputun altında sorunsuz bir şekilde yapılır.
- Apache Cassandra'daki gibi kümedeki her düğümde num_tokens ayarlamaya gerek yoktur. Düğümler ve belirteç aralıkları Cosmos DB tarafından tamamen yönetilir.
- Azure Cosmos DB Cassandra API tam olarak yönetilir, böylece Apache Cassandra'da kullanılan onarım, devre dışı bırakma vb. düğüm komutlarına ihtiyaç duymazsınız.

## <a name="other-frequently-asked-questions"></a>Diğer sık sorulan sorular

### <a name="what-is-the-protocol-version-supported-by-azure-cosmos-db-cassandra-api-is-there-a-plan-to-support-other-protocols"></a>Azure Cosmos DB Cassandra API tarafından desteklenen protokol sürümü nedir? Diğer protokolleri desteklemek için bir plan var mı?

Azure Cosmos DB Cassandra API, CQL sürüm 3.x'i destekler. Bu CQL uyumluluk kamu [Apache Cassandra GitHub deposu](https://github.com/apache/cassandra/blob/trunk/doc/cql3/CQL.textile)dayanmaktadır. Diğer protokolleri destekleme hakkında geri bildiriminiz varsa, [kullanıcı ses geri](https://feedback.azure.com/forums/263030-azure-cosmos-db) [askcosmosdbcassandra@microsoft.com](mailto:askcosmosdbcassandra@microsoft.com)bildirimi aracılığıyla bize bildirin veya e-posta gönderin.

### <a name="why-is-choosing-a-throughput-for-a-table-a-requirement"></a>Tablo için iş elde etme gereksinimi neden gereklidir?

Azure Cosmos DB, tabloyu oluşturduğunuz yerden - portal veya CQL' ye göre kapsayıcınız için varsayılan iş ortasını ayarlar.
Azure Cosmos DB, üst sınırlarla birlikte performans ve gecikme sonu için garanti sağlar. Bu garanti, motor kiracının işlemlerinde yönetim icra edebildiği zaman mümkündür. Platform bu kapasiteyi rezerve ettiği ve operasyon başarısını garanti ettiği için, iş ortamının ayarlanması garantili iş sonu ve gecikme si elde etmenizi sağlar.
Uygulamanızın mevsimsellikten yararlanmak ve maliyetlerden tasarruf etmek için [iş bişini elastik olarak değiştirebilirsiniz.](manage-scale-cassandra.md)

İş ortası kavramı, [Azure Cosmos DB makalesindeki İstek Birimlerinde](request-units.md) açıklanmıştır. Bir tablonun iş bölümü, temel fiziksel bölümler arasında eşit olarak dağıtılır.

### <a name="what-is-the-default-rus-of-table-when-created-through-cql-what-if-i-need-to-change-it"></a>CQL ile oluşturulduğunda tablonun varsayılan RU/s nedir? Ya değiştirmem gerekirse?

Azure Cosmos DB, iş ortası sağlamak için bir para birimi olarak istek birimlerini (RU/s) saniyede kullanır. CQL ile oluşturulan tablolar 400 RU'ya sahiptir. Ru'yu portaldan değiştirebilirsiniz.

CQL

```shell
CREATE TABLE keyspaceName.tablename (user_id int PRIMARY KEY, lastname text) WITH cosmosdb_provisioned_throughput=1200
```

.NET

```csharp
int provisionedThroughput = 400;
var simpleStatement = new SimpleStatement($"CREATE TABLE {keyspaceName}.{tableName} (user_id int PRIMARY KEY, lastname text)");
var outgoingPayload = new Dictionary<string, byte[]>();
outgoingPayload["cosmosdb_provisioned_throughput"] = Encoding.UTF8.GetBytes(provisionedThroughput.ToString());
simpleStatement.SetOutgoingPayload(outgoingPayload);
```

### <a name="what-happens-when-throughput-is-used-up"></a>Elde iş bittiğinde ne olur?

Azure Cosmos DB, üst sınırlarla birlikte performans ve gecikme sonu için garanti sağlar. Bu garanti, motor kiracının işlemlerinde yönetim icra edebildiği zaman mümkündür. Platform bu kapasiteyi rezerve ettiği ve operasyon başarısını garanti ettiği için, garantili iş sonu ve gecikme si elde etmenizi sağlayan iş yerini ayarlamaya dayalı olarak bu mümkündür.
Bu kapasitenin üzerinden geçdiğinizde, kapasitenizin kullanıldığını gösteren aşırı yüklü hata iletisi alırsınız.
0x1001 Aşırı yüklendi: "İstek Oranı büyük" olduğundan istek işlenemez. Bu noktada, hangi işlemlerin ve seslerinin bu soruna neden olduğunu görmek önemlidir. Portaldaki ölçümlerle birlikte, tüketilen kapasite nin kullanılabilir kapasiteyi aşarak ilgili bir fikir edinebilirsiniz. Daha sonra kapasitenin tüm temel bölümlerde neredeyse eşit olarak tüketilmesini sağlamanız gerekir. Elde edilen iş bölümünün çoğunun tek bir bölüm tarafından tüketilince görürseniz, iş yükünün çarpıklığı vardır.

İşlem in saat, gün ve yedi gün içinde, bölümler arasında veya toplu olarak nasıl kullanıldığını gösteren ölçümler kullanılabilir. Daha fazla bilgi için Azure [Cosmos DB'deki ölçümleri izleme ve hata ayıklama](use-metrics.md)bölümüne bakın.

Tanılama günlükleri [Azure Cosmos DB tanılama günlüğü](logging.md) makalesinde açıklanır.

### <a name="does-the-primary-key-map-to-the-partition-key-concept-of-azure-cosmos-db"></a>Azure Cosmos DB'nin bölüm anahtarı kavramının birincil anahtarı eşlenemiyor mu?

Evet, bölüm anahtarı varlığı doğru konuma yerleştirmek için kullanılır. Azure Cosmos DB'de, fiziksel bir bölümüzerinde depolanan doğru mantıksal bölümü bulmak için kullanılır. Bölümleme kavramı, [Azure Cosmos DB makalesinde Bölüm ve ölçekte](partition-data.md) iyi bir şekilde açıklanmıştır. Burada önemli almak mantıksal bir bölüm bugün 10 GB sınırı üzerinden gitmemelidir.

### <a name="what-happens-when-i-get-a-quota-full-notification-indicating-that-a-partition-is-full"></a>Bir bölümün dolu olduğunu belirten bir kota tam" bildirimi alırsam ne olur?

Azure Cosmos DB, gecikme, iş ortası, kullanılabilirlik ve tutarlılık garantileriyle sınırsız ölçek sağlayan SLA tabanlı bir sistemdir. Bu sınırsız depolama anahtar kavram olarak bölümleme kullanarak veri dışında yatay ölçek dayanmaktadır. Bölümleme kavramı, [Azure Cosmos DB makalesinde Bölüm ve ölçekte](partition-data.md) iyi bir şekilde açıklanmıştır.

Uymanız gereken mantıksal bölüm başına varlık veya öğe sayısına ilişkin 10 GB sınırı. Uygulamanızın iyi ölçeklendirildiğından emin olmak için, tüm bilgileri tek bir bölümde depolayıp sorgulayarak sıcak bir bölüm *oluşturmamanızı* öneririz. Bu hata yalnızca verileriniz çarpıksa gelebilir: yani, bir bölüm anahtarı (10&nbsp;GB'dan fazla) için çok fazla veriniz vardır. Depolama portalını kullanarak veri dağıtımını bulabilirsiniz. Bu hatayı düzeltmenin yolu, tabloyu yeniden oluşturmak ve verilerin daha iyi dağıtılmasına olanak tanıyan parçalı birincil (bölüm anahtarı) seçmektir.

### <a name="is-it-possible-to-use-cassandra-api-as-key-value-store-with-millions-or-billions-of-individual-partition-keys"></a>Cassandra API'yi milyonlarca veya milyarlarca ayrı bölüm anahtarıyla anahtar değer deposu olarak kullanmak mümkün mü?

Azure Cosmos DB, depolama alanını ölçekleyerek sınırsız veri depolayabilir. Bu, iş açısından bağımsızdır. Evet, sağ birincil/bölüm anahtarını belirterek anahtar/değerleri depolamak ve almak için cassandra API'yi her zaman kullanabilirsiniz. Bu tek tek anahtarlar kendi mantıksal bölüm olsun ve sorunları olmadan fiziksel bölüm üstüne oturup.

### <a name="is-it-possible-to-create-more-than-one-table-with-apache-cassandra-api-of-azure-cosmos-db"></a>Azure Cosmos DB'nin Apache Cassandra API'si ile birden fazla tablo oluşturmak mümkün mü?

Evet, Apache Cassandra API ile birden fazla tablo oluşturmak mümkündür. Bu tabloların her biri, iş artışı ve depolama birimi olarak kabul edilir.

### <a name="is-it-possible-to-create-more-than-one-table-in-succession"></a>Art arda birden fazla tablo oluşturmak mümkün mü?

Azure Cosmos DB, hem veri hem de denetim düzlemi etkinlikleri için kaynak yönetimli sistemdir. Koleksiyonlar, tablolar gibi kapsayıcılar, verilen iş verme kapasitesi için sağlanan çalışma zamanı varlıklarıdır. Bu kapsayıcıların hızlı bir şekilde oluşturulması beklenen etkinlik ve daraltılmış değildir. Tabloları hemen düşüren/oluşturan testleriniz varsa, bunları boşluğa düşürmeyi deneyin.

### <a name="what-is-maximum-number-of-tables-that-can-be-created"></a>Oluşturulabilecek en fazla tablo sayısı nedir?

Tablo sayısında fiziksel bir sınır yoktur, her [askcosmosdbcassandra@microsoft.com](mailto:askcosmosdbcassandra@microsoft.com) zamanki 10'lardan veya 100'lerden oluşturulması gereken çok sayıda tablonuz varsa (toplam sabit boyutun 10 TB'nin üzerinde veri gittiği) bir e-posta gönderin.

### <a name="what-is-the-maximum--of-keyspace-that-we-can-create"></a>Oluşturabileceğimiz keyspace'in maksimum # değeri nedir?

Meta veri kapsayıcıları oldukları için anahtar alanı sayısında fiziksel bir sınır [askcosmosdbcassandra@microsoft.com](mailto:askcosmosdbcassandra@microsoft.com) yoktur, herhangi bir nedenle çok sayıda anahtar alanınız varsa e-posta gönderin.

### <a name="is-it-possible-to-bring-in-lot-of-data-after-starting-from-normal-table"></a>Normal tablodan başladıktan sonra çok fazla veri getirmek mümkün mü?

Evet, düzgün dağılmış bölümler varsayarsak, depolama kapasitesi otomatik olarak yönetilir ve siz daha fazla veri ittikçe artar. Böylece düğümleri yönetmeden ve sağlamadan istediğiniz kadar veriyi ve daha fazlasını güvenle içe aktarabilirsiniz. Ancak, hemen veri büyüme bir sürü bekliyorsanız, daha düşük başlayan ve hemen artan yerine [beklenen iş üretimi için](set-throughput.md) doğrudan sağlanması daha mantıklı.

### <a name="is-it-possible-to-supply-yaml-file-settings-to-configure-apache-casssandra-api-of-azure-cosmos-db-behavior"></a>Azure Cosmos DB davranışının Apache Casssandra API'sini yapılandırmak için yaml dosya ayarlarını sağlama nız mümkün mü?

Azure Cosmos DB'nin Apache Cassandra API'si bir platform hizmetidir. Yürütme işlemleri için protokol düzeyi uyumluluğu sağlar. Yönetim, izleme ve yapılandırmanın karmaşıklığını gizler. Geliştirici/kullanıcı olarak, kullanılabilirlik, mezar taşları, anahtar önbelleği, satır önbelleği, çiçek filtresi ve diğer ayarların çokluğu hakkında endişelenmenize gerek yoktur. Azure Cosmos DB'nin Apache Cassandra API'si, yapılandırma ve yönetim yükü olmadan gereksinim duyduğunuz okuma yazma performansı sağlamaya odaklanır.

### <a name="will-apache-cassandra-api-for-azure-cosmos-db-support-node-additioncluster-statusnode-status-commands"></a>Azure Cosmos DB için Apache Cassandra API düğüm ekleme/küme durumu/düğüm durumu komutlarını destekleyecek mi?

Apache Cassandra API, kapasite planlaması yapan, & depolama için elastikiyet taleplerine cevap veren bir platform hizmetidir. Azure Cosmos DB ile iş ortası sağlamanız gerekir. Daha sonra düğümleri ekleme/silme veya yönetme endişesi duymadan gün boyunca herhangi bir sayıda yukarı ve aşağı ölçeklendirebilirsiniz. Bu düğüm, küme yönetimi aracı nı da kullanmanız gerekmez anlamına gelir.

### <a name="what-happens-with-respect-to-various-config-settings-for-keyspace-creation-like-simplenetwork"></a>Basit/ağ gibi keyspace oluşturma için çeşitli config ayarları ile ilgili olarak ne olur?

Azure Cosmos DB, kullanılabilirlik ve düşük gecikme nedeniyle kutudışında genel dağıtım sağlar. Çoğaltmaları veya başka şeyleri kurmanız gerekmez. Tüm yazılar, performans garantisi sağlarken yazdığınız herhangi bir bölgede her zaman dayanıklı bir çoğunluk vardır.

### <a name="what-happens-with-respect-to-various-settings-for-table-metadata-like-bloom-filter-caching-read-repair-change-gc_grace-compression-memtable_flush_period-and-more"></a>Çiçek filtresi, önbelleğe alma, onarım değişikliği okuma, gc_grace, sıkıştırma memtable_flush_period ve daha fazlası gibi tablo meta verileri için çeşitli ayarlara ilişkin olarak ne olur?

Azure Cosmos DB, yapılandırma ayarlarının hiçbirine dokunmaya ve yanlışlıkla manipüle etmeye gerek kalmadan okuma/yazma ve iş elde etme performansı sağlar.

### <a name="is-time-to-live-ttl-supported-for-cassandra-tables"></a>Cassandra tabloları için zaman-to-live (TTL) desteklenir mi?

Evet, TTL desteklenir.

### <a name="is-it-possible-to-monitor-node-status-replica-status-gc-and-os-parameters-earlier-with-various-tools-what-needs-to-be-monitored-now"></a>Düğüm durumunu, çoğaltma durumunu, gc ve işletim sistemi parametrelerini daha önce çeşitli araçlarla izlemek mümkün mü? Şimdi izlenmesi gereken nedir?

Azure Cosmos DB, üretkenliği artırmanıza ve altyapıyı yönetme ve izleme konusunda endişelenmemenize yardımcı olan bir platform hizmetidir. Yalnızca, daraltılmış olup olmadığınızı bulmak ve bu çıktıyı artırmak veya azaltmak için portal ölçümlerinde kullanılabilen iş bölümüne dikkat etmeniz gerekir.
Monitör [SLA' ları](monitor-accounts.md).
[Ölçümleri](use-metrics.md) Kullanma [Tanılama günlüklerini](logging.md)kullanın.

### <a name="which-client-sdks-can-work-with-apache-cassandra-api-of-azure-cosmos-db"></a>Hangi istemci SDK'lar Azure Cosmos DB'nin Apache Cassandra API'si ile çalışabilir?

Apache Cassandra SDK'nın CQLv3 kullanan istemci sürücüleri istemci programları için kullanılmıştır. Kullandığınız başka sürücüleriniz varsa veya sorunlarla karşı karşıyaysanız, [askcosmosdbcassandra@microsoft.com](mailto:askcosmosdbcassandra@microsoft.com)posta gönderin.

### <a name="is-composite-partition-key-supported"></a>Bileşik bölüm anahtarı desteklendi mi?

Evet, bileşik bölüm anahtarı oluşturmak için normal sözdizimini kullanabilirsiniz.

### <a name="can-i-use-sstableloader-for-data-loading"></a>Veri yüklemesi için sstableloader kullanabilir miyim?

Hayır, sstableloader desteklenmiyor.

### <a name="can-an-on-premises-apache-cassandra-cluster-be-paired-with-azure-cosmos-dbs-cassandra-api"></a>Şirket içi Bir Apache Cassandra kümesi Azure Cosmos DB'nin Cassandra API'si ile eşlenebilir mi?

Şu anda Azure Cosmos DB, genel işlemler olmadan bulut ortamı için optimize edilmiş bir deneyime sahiptir. Eşleştirme ye ihtiyacınız varsa, [askcosmosdbcassandra@microsoft.com](mailto:askcosmosdbcassandra@microsoft.com) senaryonuzun açıklamasıyla birlikte posta gönderin. Şirket içi/farklı bulut Cassandra kümesini Cosomos DB'nin Cassandra API'si ile eşleştirmeye yardımcı olmak için çalışıyoruz.

### <a name="does-cassandra-api-provide-full-backups"></a>Cassandra API tam yedekleme sağlıyor mu?

Azure Cosmos DB, bugün tüm API'lerde dört saatlik aralıklarla alınan iki ücretsiz tam yedekleme sağlar. Bu, bir yedekleme zamanlaması ve diğer şeyler ayarlamanız gerekmez.
Bekletme ve sıklığı değiştirmek istiyorsanız, bir [askcosmosdbcassandra@microsoft.com](mailto:askcosmosdbcassandra@microsoft.com) destek talebine e-posta gönderin veya yükseltin. Yedekleme yeteneği hakkında bilgi [Otomatik çevrimiçi yedekleme ve Azure Cosmos DB](../synapse-analytics/sql-data-warehouse/backup-and-restore.md) makalesi ile geri yükleme sağlanır.

### <a name="how-does-the-cassandra-api-account-handle-failover-if-a-region-goes-down"></a>Bir bölge çökerse Cassandra API hesabı nasıl başarısız olur?

Azure Cosmos DB Cassandra API, azure cosmos DB'nin dünya çapında dağıtılan platformundan ödünç alınr. Uygulamanızın veri merkezi kapalı kalma süresini tolere edebileceğinden emin olmak için, [çok bölgeli Azure Cosmos DB hesaplarıyla geliştirme](high-availability.md)Azure Cosmos DB portalındaki hesap için en az bir bölge daha etkinleştirin. [Çok bölgeli Azure Cosmos DB hesaplarıyla Geliştirme](high-availability.md)portalını kullanarak bölgenin önceliğini belirleyebilirsiniz.

Hesap için istediğiniz kadar bölge ekleyebilir ve bir hata önceliği sağlayarak hesap üzerinde başarısız olabilir denetim. Veritabanını kullanmak için, orada da bir uygulama sağlamanız gerekir. Bunu yaptığınızda, müşterileriniz kapalı kalma süresi yle karşılaşmazlar.

### <a name="does-the-apache-cassandra-api-index-all-attributes-of-an-entity-by-default"></a>Apache Cassandra API varsayılan olarak bir varlığın tüm özniteliklerini dizine yapar mı?

Hayır. Cassandra API [ikincil indeksleri](cassandra-secondary-index.md)destekler , Hangi Apache Cassandra çok benzer bir şekilde davranır. Varsayılan olarak her özniteliği dizine almaz.  


### <a name="can-i-use-the-new-cassandra-api-sdk-locally-with-the-emulator"></a>Yeni Cassandra API SDK'yı emülatörle birlikte yerel olarak kullanabilir miyim?

Evet bu desteklenir. Bunu nasıl etkinleştirebileceğinize dair ayrıntıları [burada](local-emulator.md#cassandra-api) bulabilirsiniz


### <a name="how-can-i-migrate-data-from-their-apache-cassandra-clusters-to-cosmos-db"></a>Apache Cassandra kümelerinden verileri Cosmos DB'ye nasıl aktarabilirim?

Geçiş seçenekleri hakkında [buradan](cassandra-import-data.md)okuyabilirsiniz.


### <a name="feature-x-of-regular-cassandra-api-isnt-working-as-today-where-can-the-feedback-be-provided"></a>Düzenli Cassandra API Özelliği x bugün gibi çalışmıyor, nerede geribildirim sağlanabilir?

[Kullanıcı ses geri bildirimi](https://feedback.azure.com/forums/263030-azure-cosmos-db)ile geri bildirim sağlayın.

[azure-portal]: https://portal.azure.com
[query]: sql-api-sql-query.md

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Cosmos DB Cassandra API Hesabı'nı elastik ölçekleme](manage-scale-cassandra.md)ile başlayın.
