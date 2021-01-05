---
title: Çok siteli ve çok bölgeli federasyon-Azure Event Hubs | Microsoft Docs
description: Bu makalede, Azure Event Hubs ile çok siteli ve çok bölgeli federasyona genel bakış sunulmaktadır.
ms.topic: article
ms.date: 12/12/2020
ms.author: spelluru
ms.openlocfilehash: 12ef895c8b16fe18ed02ebf01d17624ac71c2f3e
ms.sourcegitcommit: aeba98c7b85ad435b631d40cbe1f9419727d5884
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/04/2021
ms.locfileid: "97861466"
---
# <a name="multi-site-and-multi-region-federation"></a>Çok siteli ve çok bölgeli federasyon

Birçok gelişmiş çözüm, aynı olay akışlarının birden fazla konumda tüketimine yönelik olarak kullanılabilmesini gerektirir veya olay akışlarının birden fazla konumda toplanmasını ve daha sonra tüketim için belirli bir konuma birleştirilmesini gerektirir. Ayrıca, tek bir bölge ve çözüm içinde de olmak üzere olay akışlarını zenginleştirme veya azaltmanın yanı sıra olay biçim dönüştürmeleri de yapmanız gerekir.

Pratikte, çözümünüz genellikle farklı bölgelerde ve Event Hubs ad alanlarında birden çok Event Hubs koruacaktır ve olayları aralarında çoğaltabilir. Ayrıca, [Azure Service Bus](../service-bus-messaging/service-bus-messaging-overview.md), [Azure IoT Hub](../iot-fundamentals/iot-introduction.md)veya [Apache Kafka](https://kafka.apache.org)gibi kaynak ve hedeflerle da olay alışverişi yapabilirsiniz. 

Farklı bölgelerde birden çok etkin Event Hubs saklanması, istemcilerin içeriği birleştirilebilirliği halinde aralarında geçiş yapmasına ve aralarında geçiş yapmasına olanak tanır. Bu, genel sistemin bölgesel kullanılabilirlik sorunlarına karşı daha dayanıklı olmasını sağlar.

Bu "Federasyon" bölümünde, Federasyon desenleri ve sunucusuz [Azure Stream Analytics](../stream-analytics/stream-analytics-introduction.md) ya da [Azure işlevleri][1] çalışma zamanları kullanılarak, olay akışı yolunda kendi dönüştürmemize veya zenginleştirme koduna sahip olmak üzere bu desenlerin nasıl kullanılacağı açıklanmaktadır. 

## <a name="federation-patterns"></a>Federasyon desenleri

Farklı Event Hubs veya diğer kaynak ve hedefler arasında olayları taşımak isteyebileceğiniz birçok olası canlandırma vardır. Bu bölümdeki en önemli desenleri listeliyoruz ve ayrıca ilgili düzen için daha ayrıntılı kılavuza bağlanır. 

- [Bölgesel kullanılabilirlik olaylarına karşı dayanıklılık](#resiliency-against-regional-availability-events)
- [Gecikme süresi iyileştirmesi](#latency-optimization)
- [Doğrulama, azaltma ve zenginleştirme](#validation-reduction-and-enrichment)
- [Analiz hizmetleriyle tümleştirme](#integration-with-analytics-services)
- [Olay akışlarının birleştirilmesi ve normalleştirilmesi](#consolidation-and-normalization-of-event-streams)
- [Olay akışlarını bölme ve yönlendirme](#splitting-and-routing-of-event-streams)
- [Günlük projeksiyonlarını](#log-projections)
  
### <a name="resiliency-against-regional-availability-events"></a>Bölgesel kullanılabilirlik olaylarına karşı dayanıklılık 

![Bölgesel Kullanılabilirlik](media/event-hubs-federation-overview/regional-availability.jpg)

En yüksek kullanılabilirlik ve güvenilirlik Event Hubs için en üst işlem öncelikleridir, ancak bir üreticinin veya tüketicinin ağ veya ad çözümleme sorunları nedeniyle veya bir olay hub 'ının geçici olarak yanıt verme veya hata döndürme olabileceği birçok yol vardır. 

Bu tür koşullar, bir [olağanüstü durum kurtarma](event-hubs-geo-dr.md) durumunda olduğu gibi bölgesel dağıtımı iptal etmek isteyeceksiniz. ancak bazı uygulamaların iş senaryosu, en son birkaç dakika veya hatta saniye içinde olmayan kullanılabilirlik olayları tarafından zaten etkilenebilir. 

Böyle senaryolara yönelik iki temel desen vardır:

- [Çoğaltma][4] stili, birincil olay hub 'ının içeriğini Ikincil bir olay hub 'ına kopyalama hakkında, birincil olay hub 'ının genellikle uygulama tarafından hem üretim hem de kullanma olayları ve Ikincil Olay Hub 'ının kullanılamaz hale gelmesine yönelik bir geri dönüş seçeneği olarak kullanıldığı durumdur. Çoğaltma tek yönlü olduğundan, Birincilden ikinciye, hem üreticileri hem de tüketicilerinin, kullanılamayan bir birincil sunucudan ikincil öğesine geçişi, eski birincil öğenin artık yeni olayları almasına neden olur ve bu nedenle artık geçerli olmayacaktır.
  Bu nedenle saf çoğaltma yalnızca tek yönlü yük devretme senaryoları için uygundur. Yük devretme gerçekleştirildikten sonra, eski birincil terk edilir ve farklı bir hedef bölgede yeni bir ikincil Olay Hub 'ının oluşturulması gerekir.
- [Birleştirme][5] stili, iki veya daha fazla Event Hubs içeriğin sürekli bir birleştirilmesini gerçekleştirerek çoğaltma modelini genişletir. İlk olarak, düzene eklenen Event Hubs biri içinde oluşturulan her olay diğer Event Hubs çoğaltılır. Olaylar çoğaltıldıklarında, bunlar daha sonra çoğaltma hedefinin çoğaltma işlemi tarafından yoksayıldıklarından açıklanırlar. Birleştirme modelini kullanmanın sonuçları, sonunda tutarlı bir biçimde aynı olay kümesini içerecek iki veya daha fazla Event Hubs. 
  
Her iki durumda da Event Hubs içeriği aynı olmayacaktır. Her bir üretici ve aynı bölüm anahtarına göre gruplanmış olaylar, başlangıçta gönderilen şekilde aynı göreli sırada görünür, ancak olayların mutlak sırası farklı olabilir. Bu özellikle, kaynak ve hedef Event Hubs bölüm sayısının farklı olduğu senaryolarda, burada açıklanan genişletilmiş desenlerin bazıları için de geçerlidir. Bir [Splitter veya yönlendirici](#splitting-and-routing-of-event-streams) , yüzlerce bölümden oluşan çok daha büyük bir olay hub 'ının bir dilimini alabilir ve alt kümeyi sınırlı işleme kaynaklarıyla işlemek için daha uygundur. Buna karşılık, bir [birleştirme](#consolidation-and-normalization-of-event-streams) , birkaç küçük Event Hubs verileri, birleştirilmiş aktarım hızı ve işleme gereksinimleriyle daha fazla bölüme sahip tek ve daha büyük bir olay hub 'ına huni olarak içerebilir.

Olayları birlikte tutma ölçütü, özgün bölüm KIMLIĞI değil, Bölüm anahtarıdır. Göreli sipariş ve bir olay hub 'ından bir sonrakine, aynı akış uzaklıkları kapsamında güvenmek zorunda kalmadan bir yük devretme gerçekleştirme hakkında daha fazla dikkat edilecek konular, [çoğaltma][4] düzeni açıklamasında ele alınmıştır.


Kılavuzu 
- [Çoğaltma kalıbı][4]
- [Birleştirme kalıbı][5]

### <a name="latency-optimization"></a>Gecikme süresi iyileştirmesi 

![Gecikme süresi Iyileştirmesi](media/event-hubs-federation-overview/latency-optimization.jpg)  

Olay akışları, üreticileri tarafından bir kez yazılır, ancak olay tüketicilerine göre herhangi bir sayıda okunabilir. Bir bölgedeki bir olay akışının birden çok tüketici tarafından paylaşıldığı ve farklı bir bölgede bulunan analiz işlemleri sırasında sürekli olarak erişilmesi veya eşzamanlı tüketicilerle karşılaşması gereken talepler için, gidiş dönüş gecikmesini azaltmak amacıyla olay akışının bir kopyasını analiz işlemcisine eklemek yararlı olabilir. 

Bölgelerin, bölgelere göre çok büyük bir süre boyunca tercih edildiği durumlarda, Avrupa ve Avustralya 'nın neredeyse kolay bir şekilde olduğu, coğrafi olarak ve ağ gecikmelerinin her türlü gidiş dönüş için 250 MS 'yi kolayca aşabileceği iyi örnekler vardır.
Işığın hızını hızlandırabilir, ancak verilerle etkileşimde bulunmak için yüksek gecikmeli gidiş dönüş sayısını azaltabilirsiniz.

Kılavuzu 
- [Çoğaltma kalıbı][4]

### <a name="validation-reduction-and-enrichment"></a>Doğrulama, azaltma ve zenginleştirme

![Doğrulama, azaltma, zenginleştirme](media/event-hubs-federation-overview/validation-enrichment.jpg)  

Olay akışları, kendi çözümünüzün dışındaki istemciler tarafından bir olay hub 'ına gönderilebilir. Bu tür olay akışları, harici olarak gönderilen olayların belirli bir şemayla uyumluluk için denetlenmesi ve uyumlu olmayan olayların bırakılması için gerekli olabilir. 

Tarifeli bant genişliğine sahip çok sayıda "Nesnelerin İnterneti" senaryosunda veya olayların ilk olarak kısıtlanmış paket boyutları ile IP dışı ağlarda gönderildiği durumlarda istemcilerin son derece bant genişliği sınırlı olduğu senaryolarda, bu olayların, aşağı akış olay işlemcileri tarafından kullanılabilir olması için daha fazla bağlam eklemek üzere başvuru verileriyle uyumlu olması gerekebilir.

Diğer durumlarda, özellikle akışlar birleştirileceğinden, bazı ayrıntıları atlayarak olay verilerinin karmaşıklık veya yavaya boyutunu düşürmek gerekebilir.

Bu işlemlerden herhangi biri çoğaltma, birleştirme veya birleştirme akışlarının bir parçası olarak gerçekleşebilir. 

Kılavuzu 
- [Düzenleyici deseninin][6]

### <a name="integration-with-analytics-services"></a>Analiz hizmetleriyle tümleştirme

![Analiz hizmetleriyle tümleştirme](media/event-hubs-federation-overview/integration.jpg)

Azure 'un Azure Stream Analytics veya Azure SYNAPSE gibi bulut Yerel analiz hizmetlerinden bazıları, Azure Event Hubs tarafından sunulan akışlı veya ön toplanmış verilerle en iyi şekilde çalışır ve Azure Event Hubs Ayrıca Apache Samza, Apache flink, Apache Spark ve Apache Storm gibi çeşitli açık kaynaklı analiz paketleriyle tümleştirmeyi de mümkün değildir. 

Çözümünüz öncelikle Service Bus veya Event Grid kullanıyorsa, bu olayları bu analiz sistemleri için kolayca erişilebilir hale getirebilirsiniz ve ayrıca bunları Olay Hub 'ına huni halinde Event Hubs Capture ile arşivleme için yapabilirsiniz. Event Grid, Service Bus, [Service Bus çoğaltma kılavuzunu](https://github.com/Azure-Samples/azure-messaging-replication-dotnet/tree/main/functions/config/ServiceBusCopyToEventHub)takip ettiğiniz [Olay Hub 'ı tümleştirmesiyle](../event-grid/handler-event-hubs.md)yerel olarak yapabilir.

Azure Stream Analytics [doğrudan Event Hubs ile tümleşir](../stream-analytics/stream-analytics-define-inputs.md#stream-data-from-event-hubs).

Kılavuzu 
- [Çoğaltma kalıbı][4]

### <a name="consolidation-and-normalization-of-event-streams"></a>Olay akışlarının birleştirilmesi ve normalleştirilmesi

![Olay akışlarının birleştirilmesi ve normalleştirilmesi](media/event-hubs-federation-overview/consolidation.jpg)

Genel çözümler genellikle kendi analiz özelliklerine sahip olmak dahil olmak üzere büyük ölçüde bağımsız olan bölgesel fikirlerden oluşur, ancak Supra bölgesel ve genel analiz perspektiflerinin tümleşik bir perspektifi olması gerekir ve bu, ilgili bölgesel yolculuğuna göre değerlendirilen aynı olay akışlarının merkezi bir birleştirmesi yerel perspektifle birlikte yazdırılır. 

Normalleştirme, iki veya daha fazla gelen olay akışının aynı türde olayları (ancak farklı yapılar ya da farklı kodlamalar ile) ve en çok bir olay, tüketilebilmesi için dönüştürülebildikleri veya dönüştürülebildiği birleştirme senaryosunun bir türüdür. 

Normalleştirme, uçtan uca şifrelenmiş yüklerin şifresini çözme ve aşağı akış tüketicisi için farklı anahtarlar ve algoritmalarla yeniden şifreleme gibi şifreleme işi de içerebilir. 

Kılavuzu 
- [Birleştirme kalıbı][5]
- [Düzenleyici deseninin][6]

### <a name="splitting-and-routing-of-event-streams"></a>Olay akışlarını bölme ve yönlendirme

![Olay akışlarını bölme ve yönlendirme](media/event-hubs-federation-overview/splitting.jpg)

Azure Event Hubs, her ikisi de yerel yayımlama-abone ol filtreleme ve dağıtım özelliklerine sahip olan ve bu model için tercih edilen gelen bir olay alınan olayların, Azure Service Bus veya Azure Event Grid kapasitesini aşan "yayımla-abone ol" stil senaryolarında kullanılır. 

Doğru bir "yayımla-abone ol" özelliği bunu, istedikleri olayları seçmek için abonelere bıraksa da, bölme deseninin, önceden belirlenmiş bir dağıtım modeli tarafından bölümler için üretici eşlemesi olayları vardır ve belirlenen tüketiciler, özel olarak "kendi" bölümünden çekirler. Olay Hub 'ında genel trafiği arabelleğe alarak, belirli bir bölümün içeriği özgün üretilen iş hacmi bölümünün bir bölümünü temsil eden, güvenilir, hareketsel ve rekabet eden müşteri tüketimi için bir sıraya çoğaltılır.

Event Hubs birincil olarak bir bölgedeki bir uygulama içindeki olayları taşımak için kullanılan birçok senaryo, tek bir bölümden yalnızca bir yerde, diğer bir yerde de kullanılabilir hale getirilmelidir. Bu senaryo bölme senaryosuna benzerdir, ancak bir olay hub 'ına ulaşan tüm iletileri göz önünde tutarak ölçeklenebilir bir yönlendirici kullanabilir ve tek tek, yeni bir uygulama için yalnızca birkaç kez seçer ve yönlendirme hedeflerini olay meta verileri veya içeriğine göre ayırt edebilir. 

Kılavuzu
- [Yönlendirme deseninin][7]

### <a name="log-projections"></a>Günlük projeksiyonlarını 

![Günlük projeksiyonu](media/event-hubs-federation-overview/log-projection.jpg)

Bazı senaryolarda, bir olayın herhangi bir alt akışı için gönderilen en son değere erişmek ve genellikle bölüm anahtarı ile ayırt etmek isteyeceksiniz. Apache Kafka, bu genellikle bir konu üzerinde "günlük düzenleme" etkinleştirilerek elde edilir, ancak herhangi bir benzersiz anahtarla etiketlenmiş en son olay hariç olur. Günlük düzenleme yaklaşımının üç bileşik olumsuz yönleri vardır: 

- Sıkıştırma işlemi, yalnızca Append iş yükleri için iyileştirilmiş bir aracı için aşırı pahalı bir işlem olan günlüğün sürekli bir yeniden organizasyonunu gerektirir. 
- Sıkıştırma bozucu ve aynı akışın sıkıştırılmış ve sıkıştırılmamış bir perspektifine izin vermez.
- Sıkıştırılan bir akışta hala sıralı bir erişim modeli vardır. Bu, günlükteki istenen değeri bulmanın en kötü durumda, genellikle burada sunulan tam modeli uygulayan iyileştirmelere neden olan en kötü durum için günlük okumayı gerektirir. günlük içeriğini bir veritabanı veya önbellekte yansıtma. 

Sonuç olarak, sıkıştırılan bir günlük anahtar-değer deposudur ve bu tür bir mağaza için olası en kötü uygulama seçeneğidir. Aramalar ve sorguların, doğru bir anahtar-değer deposunda veya başka bir veritabanında kalıcı projeksiyonu oluşturmak ve kullanmak için daha verimli bir işlemdir. 

Olaylar sabit olduğu ve sıra bir günlükte her zaman korunduğu için, bir günlüğün anahtar-değer deposuna her zaman aynı olması gerekir. böylece, güncelleştirmekte olduğunuz bir projeksiyon her zaman yetkili bir görünüm sağlar ve bu öğeyi, oluşturulduktan sonra günlük içeriğinden yeniden oluşturmak için hiçbir neden iyi bir neden yoktur. 

Kılavuzu
- [Günlük projeksiyonu][8]

## <a name="replication-application-technologies"></a>Çoğaltma uygulaması teknolojileri

Yukarıdaki desenleri uygulamak, yapılandırmak ve çalıştırmak istediğiniz çoğaltma görevleri için ölçeklenebilir ve güvenilir bir yürütme ortamı gerektirir. Azure 'da, bu tür görevlere en uygun çalışma zamanı ortamları durum bilgisiz akış çoğaltma görevleri için Azure Stream Analytics ve durum bilgisiz çoğaltma görevleri için [Azure işlevleri](../azure-functions/functions-overview.md) için [](../stream-analytics/stream-analytics-introduction.md) .

### <a name="stateful-replication-applications-in-azure-stream-analytics"></a>Azure Stream Analytics 'da durum bilgisi olan çoğaltma uygulamaları

Olaylar arasındaki ilişkileri göz önünde bulundurmanız gereken, bileşik olaylar oluşturma, olayları zenginleştirme veya olayları azaltma, veri toplamaları oluşturma ve olay yüklerini dönüştürme, en iyi uygulama seçeneği [Azure Stream Analytics](../stream-analytics/stream-analytics-introduction.md) .

Azure Stream Analytics, [girişleri](../stream-analytics/stream-analytics-add-inputs.md) ve [çıkışları](../stream-analytics/stream-analytics-define-outputs.md) tümleştiren ve bu verileri, çıkışlardan daha sonra kullanılabilir hale getirilen [sorgular](/stream-analytics-query/stream-analytics-query-language-reference) aracılığıyla girişlerle tümleştiren [işler oluşturursunuz](../stream-analytics/stream-analytics-quick-create-portal.md) .

Sorgular [SQL sorgu diline](/stream-analytics-query/stream-analytics-query-language-reference) dayalıdır ve akış verilerini kolayca filtrelemek, sıralamak, toplamak ve bir süre içinde birleştirmek için kullanılabilir. [JavaScript](../stream-analytics/stream-analytics-javascript-user-defined-functions.md) ve [C# Kullanıcı tanımlı işlevlerle (UDF 'ler)](../stream-analytics/stream-analytics-edge-csharp-udf-methods.md)bu SQL dilini de genişletebilirsiniz. Basit dil yapıları ve/veya yapılandırmalarında toplama işlemleri gerçekleştirirken olay sıralama seçeneklerini ve zaman pencerelerinin süresini kolayca ayarlayabilirsiniz.

Her işin dönüştürülmüş veriler için bir veya birkaç çıkışı vardır ve analiz ettiğiniz bilgilere yanıt olarak neler olduğunu denetleyebilirsiniz. Örneğin, şunları yapabilirsiniz:

- İletişimleri veya özel iş akışlarını aşağı yönde tetiklemek için Azure Işlevleri, Service Bus konuları veya kuyrukları gibi hizmetlere veri gönderin.
- Gerçek zamanlı bir taslak oluşturma için Power BI panoya veri gönderme.
- Toplu analizler gerçekleştirmek veya çok büyük, dizinli geçmiş verileri havuzlarına göre makine öğrenimi modellerini eğitmek için diğer Azure depolama hizmetlerinde (örneğin, Azure Data Lake, Azure SYNAPSE Analytics vb.) veri depolayın.
- Veritabanları ([SQL veritabanı](../stream-analytics/sql-database-output.md), [Cosmos DB](../stream-analytics/azure-cosmos-db-output.md) ) içindeki projeksiyonları ("gerçekleştirilmiş görünümler" olarak da bilinir) depolayın.

### <a name="stateless-replication-applications-in-azure-functions"></a>Azure Işlevlerinde durum bilgisiz çoğaltma uygulamaları

Olaylarını göz önünde bulundurmadan veya listedir işler olmadan olayları iletmek istediğiniz durum bilgisiz çoğaltma görevleri için (göreli sırası hariç), Azure Işlevleri 'ni kullanarak bir veya daha fazla esneklik sağlar.

Azure Işlevleri, [azure Event Hubs](../azure-functions/functions-bindings-event-hubs.md), [azure IoT Hub](../azure-functions/functions-bindings-event-iot.md), [Azure Service Bus](../azure-functions/functions-bindings-service-bus.md), [Azure Event Grid](../azure-functions/functions-bindings-event-grid.md)ve [Azure kuyruk depolama](../azure-functions/functions-bindings-storage-queue.md)için önceden oluşturulmuş, ölçeklenebilir Tetikleyiciler ve çıkış bağlamaları ve ayrıca, [kbbitmq](https://github.com/azure/azure-functions-rabbitmq-extension)ve [Apache Kafka](https://github.com/azure/azure-functions-kafka-extension)için özel uzantılar içerir. Birçok tetikleyici, belgeli ölçümlere göre aynı anda yürütülen örneklerin sayısını ölçeklendirerek dinamik olarak işleme gereksinimlerine uyum sağlar. 

Azure Işlevleri, günlük projeksiyonlarını oluşturmak için [Cosmos DB](../azure-functions/functions-bindings-cosmosdb-v2-output.md) ve [Azure Tablo Depolaması](../azure-functions/functions-bindings-storage-table-output.md)için çıkış bağlamalarını destekler.

Azure Işlevleri, [Azure tarafından yönetilen bir kimlik](../active-directory/managed-identities-azure-resources/overview.md) altında çalışabilir ve bu kimlik bilgileri [Azure Key Vault](../key-vault/general/overview.md)içindeki sıkı erişimli denetimli depolamada kimlik bilgileri için yapılandırma değerlerini tutabilir.

Azure Işlevleri ayrıca çoğaltma görevlerinin tüm Azure mesajlaşma hizmetleri için Azure sanal ağları ve [hizmet uç](../virtual-network/virtual-network-service-endpoints-overview.md) noktalarıyla doğrudan tümleştirilmesine olanak tanır ve [Azure izleyici](../azure-monitor/overview.md)ile kolayca tümleşiktir.

Azure Işlevleri tüketim planı sayesinde, önceden oluşturulmuş Tetikleyiciler, çoğaltma için kullanılabilir bir ileti olmadığında bile sıfıra ölçeklendirebilir, bu da yapılandırmanın ölçeği geri oluşturmaya hazır hale getirmek için hiçbir ücret ödemenize neden olur. tüketim planı kullanmanın alt tarafında, bu durumdan "uyandırma" çoğaltma görevlerinin gecikmesi, altyapının çalıştığı barındırma planlarından önemli ölçüde daha yüksek bir durumdur.  

Tüm bunların aksine, Apache Kafka [Mirrormaker](http://kafka.apache.org/documentation/#basic_ops_mirror_maker) gibi ileti ve olay için en yaygın çoğaltma motorları, bir barındırma ortamı sağlamanıza ve çoğaltma altyapısını kendiniz ölçeklendirmenize gerek duyar. Bu, güvenlik ve ağ özelliklerini yapılandırmayı ve tümleştirmeyi ve izleme verilerinin akışını kolaylaştırmanın yanı sıra, yine de akışa özel çoğaltma görevleri ekleme şansınız yoktur. 

### <a name="choosing-between-azure-functions-and-azure-stream-analytics"></a>Azure Işlevleri ve Azure Stream Analytics arasında seçim yapma

Azure Stream Analytics (ASA), çoğaltma sırasında olaylarınızın yükünü her işlemek için gereken en iyi seçenektir. ASA olayları tek tek kopyalayabilir veya iletmeden önce olay akışlarının bilgilerini sıkıştırmak üzere toplamalar oluşturabilir. Bu verileri bir akışa aktarmak zorunda kalmadan Azure Blob depolama veya Azure SQL veritabanı 'nda tutulan [başvuru verilerini kolayca ele](../stream-analytics/stream-analytics-use-reference-data.md) alabilir.

ASA ile, Hyper-ölçekli veritabanlarında akışların kalıcı, gerçekleştirilmiş görünümlerini kolayca oluşturabilirsiniz. Bu, clunky "günlük düzenleme" Apache Kafka modeli ve Kafka akışlarının geçici, istemci tarafı tablo projeksiyonileri için en üstün bir yaklaşımdır. 

ASA [, CSV, JSON ve Apache avro biçimlerinde](../stream-analytics/stream-analytics-parsing-json.md) kodlanmış yükleri olan olayları kolayca işleyebilir ve [özel seri hale Getiricileri](../stream-analytics/custom-deserializer.md) başka herhangi bir biçim için de takabilirsiniz.

Olay akışlarını "olduğu gibi" ve yükleri dokunarak kopyalamak istediğiniz tüm çoğaltma görevleri için veya bir yönlendirici uygulamanız, şifreleme işi yapmanız, yüklerin kodlamasını değiştirmeniz veya aksi takdirde veri akışı içeriği üzerinde tam denetim gerekiyorsa, Azure Işlevleri en iyi seçenektir.

## <a name="next-steps"></a>Sonraki Adımlar

Bu makalede, Azure 'daki olay ve mesajlaşma çoğaltma çalışma zamanı gibi bir dizi Federasyon desenini araştırdık ve Azure Işlevleri rolü açıklanmıştı.

Daha sonra, Azure Stream Analytics veya Azure Işlevleri ile bir Replicator uygulaması ayarlamayı ve ardından Event Hubs ile çeşitli diğer olay ve mesajlaşma sistemleri arasında olay akışlarını nasıl çoğaltacağınızı okumak isteyebilirsiniz:

- [Olay çoğaltma görevi desenleri][10]
- [Azure Stream Analytics verileri işleme][9]
- [Azure Işlevlerinde olay çoğaltıcı uygulamaları][1]
- [Olayları Event Hubs arasında çoğaltma][2]
- [Olayları Azure Service Bus çoğaltma][3]
- [Event Hubs ile Apache Kafka MirrorMaker kullanma][11] 

[1]: event-hubs-federation-replicator-functions.md
[2]: https://github.com/Azure-Samples/azure-messaging-replication-dotnet/tree/main/functions/config/EventHubCopy
[3]: https://github.com/Azure-Samples/azure-messaging-replication-dotnet/tree/main/functions/config/EventHubCopyToServiceBus
[4]: event-hubs-federation-patterns.md#replication
[5]: event-hubs-federation-patterns.md#merge
[6]: event-hubs-federation-patterns.md#editor
[7]: event-hubs-federation-patterns.md#routing
[8]: event-hubs-federation-patterns.md#log-projection
[9]: process-data-azure-stream-analytics.md
[10]: event-hubs-federation-patterns.md#replication
[11]: event-hubs-kafka-mirror-maker-tutorial.md