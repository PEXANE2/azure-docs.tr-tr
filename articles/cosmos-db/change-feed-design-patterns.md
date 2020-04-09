---
title: Azure Cosmos DB'de özet akışı tasarım modellerini değiştirme
description: Yaygın değişiklik besleme tasarım modellerine genel bakış
author: timsander1
ms.author: tisande
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 04/08/2020
ms.openlocfilehash: 7e6981fb57421846b491693bb6195ecef31a3773
ms.sourcegitcommit: 7d8158fcdcc25107dfda98a355bf4ee6343c0f5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2020
ms.locfileid: "80986310"
---
# <a name="change-feed-design-patterns-in-azure-cosmos-db"></a>Azure Cosmos DB'de özet akışı tasarım modellerini değiştirme

Azure Cosmos DB değiştirme akışı, yüksek miktarda yazma yla büyük veri kümelerinin verimli bir şekilde işlenmesine olanak tanır. Akış değişikliği, neyin değiştiğini belirlemek için tüm veri kümesini sorgulamaya da bir alternatif sunar. Bu belge, ortak değişiklik besleme tasarım desenleri, tasarım dengelemeleri ve değişiklik akışı sınırlamalarına odaklanır.

Azure Cosmos DB, IoT, oyun, perakende ve operasyonel günlük uygulaması için uygundur. Bu uygulamalarda yaygın bir tasarım deseni, ek eylemleri tetiklemek için verilerdeki değişiklikleri kullanmaktır. Ek eylemlere örnek olarak şunlar verilebilir:

* Bir öğe eklendiğinde veya güncelleştirildiğinde, bir BILDIRIMI veya API'ye çağrıyı tetikleme.
* IoT veya operasyonel veriler üzerinde gerçek zamanlı analiz işleme için gerçek zamanlı akış işleme.
* Önbellek, arama motoru, veri ambarı veya soğuk depolama ile eşitleme gibi veri hareketi.

Azure Cosmos DB'deki değişiklik akışı, aşağıdaki resimde gösterildiği gibi, bu desenlerin her biri için verimli ve ölçeklenebilir çözümler oluşturmanıza olanak tanır:

![Azure Cosmos DB'yi kullanarak gerçek zamanlı analitik ve olay odaklı bilgi işlem senaryolarına güç sağlamak için akış değiştirme](./media/change-feed/changefeedoverview.png)

## <a name="event-computing-and-notifications"></a>Olay hesaplama ve bildirimler

Azure Cosmos DB değişiklik akışı, belirli bir olaya dayalı olarak bir bildirimi veya API'ye çağrı tetiklemeyi tetikleyen senaryoları basitleştirebilir. Değişiklik Akışı [İşlemi Kitaplığını](change-feed-processor.md) kullanarak kapsayıcınızı değişiklikler için otomatik olarak yoklayabilir ve her yazma veya güncelleştirme olduğunda harici bir API'yi arayabilirsiniz.

Ayrıca, belirli ölçütlere göre bir bildirimi seçici olarak tetikleyebilir veya API'ye çağrı gönderebilirsiniz. Örneğin, [Azure İşlevlerini](change-feed-functions.md)kullanarak değişiklik akışından okuyorsanız, yalnızca belirli bir ölçüt karşılanmışsa bildirim göndermek için işleve mantık ekleyebilirsiniz. Azure İşlev kodu her yazma ve güncelleştirme sırasında yürütülürken, bildirim yalnızca belirli ölçütler karşılanmışsa gönderilir.

## <a name="real-time-stream-processing"></a>Gerçek zamanlı akış işleme

Azure Cosmos DB değişiklik akışı, IoT için gerçek zamanlı akış işleme veya operasyonel veriler üzerinde gerçek zamanlı analiz işleme için kullanılabilir.
Örneğin, aygıtlardan, sensörlerden, altyapıdan ve uygulamalardan olay verilerini alabilir ve depolayabilir ve Bu olayları [Spark'ı](../hdinsight/spark/apache-spark-overview.md)kullanarak gerçek zamanlı olarak işleyebilirsiniz. Aşağıdaki resim, değişiklik akışı aracılığıyla Azure Cosmos DB'yi kullanarak lambda mimarisini nasıl uygulayabileceğinizi gösterir:

![Yutma ve sorgulama için Azure Cosmos DB tabanlı lambda boru hattı](./media/change-feed/lambda.png)

Çoğu durumda, akış işleme uygulamaları önce Azure Event Hub veya Apache Kafka gibi geçici bir ileti kuyruğuna yüksek hacimli gelen veri alır. Değişiklik akışı, Azure Cosmos DB'nin garantili düşük okuma ve yazma gecikmesiyle sürekli yüksek oranda veri alımını destekleme becerisi sayesinde harika bir alternatiftir. Azure Cosmos DB değiştirme akışının ileti kuyruğundaki avantajları şunlardır:

### <a name="data-persistence"></a>Veri kalıcılığı

Azure Cosmos DB'ye yazılan veriler değişiklik akışında gösterir ve silinene kadar saklanır. İleti kuyruklarının genellikle en fazla bekletme süresi vardır. Örneğin, [Azure Etkinlik Hub'ı](https://azure.microsoft.com/services/event-hubs/) en fazla 90 günlük veri saklama hizmeti sunar.

### <a name="querying-ability"></a>Sorgulama yeteneği

Cosmos kapsayıcısının değişiklik akışından okumanın yanı sıra, Azure Cosmos DB'de depolanan verilerde SQL sorguları da çalıştırabilirsiniz. Değişiklik akışı, kapsayıcıdaki verilerin yinelendiği bir yineleme değil, verileri okumanın farklı bir mekanizmasıdır. Bu nedenle, değişiklik akışındaki verileri okursanız, her zaman aynı Azure Cosmos DB kapsayıcısının sorgularıyla tutarlı olacaktır.

### <a name="high-availability"></a>Yüksek kullanılabilirlik

Azure Cosmos DB % 99,999'a kadar okuma ve yazma kullanılabilirliği sunar. Azure Cosmos DB verileri, birçok ileti kuyruğundan farklı olarak, sıfırlı [rto (Kurtarma Süresi Hedefi)](consistency-levels-tradeoffs.md#rto) ile kolayca küresel olarak dağıtılabilir ve yapılandırılabilir.

Değişiklik akışındaki öğeleri işledikten sonra, somutlaştırılmış bir görünüm oluşturabilir ve Azure Cosmos DB'de toplanan değerleri devam ettirebilirsiniz. Bir oyun oluşturmak için Azure Cosmos DB kullanıyorsanız, örneğin, tamamlanan oyunlardan alınan puanları temel alan gerçek zamanlı lider panolarını uygulamak için değişiklik akışını kullanabilirsiniz.

## <a name="data-movement"></a>Veri taşıma

Gerçek zamanlı veri hareketi için değişiklik akışından da okuyabilirsiniz.

Örneğin, değişiklik akışı aşağıdaki görevleri verimli bir şekilde gerçekleştirmenize yardımcı olur:

* Azure Cosmos DB'de depolanan verilerle önbellek, arama dizini veya veri ambarı güncelleştirin.

* Farklı bir mantıksal bölüm anahtarına sahip başka bir Azure Cosmos hesabına veya başka bir Azure Cosmos kapsayıcısına sıfır aşağı zamanlı geçiş gerçekleştirin.

* Uygulama düzeyinde veri katmanlama ve arşivleme uygulayın. Örneğin, "sıcak verileri" Azure Cosmos DB'de depolayabilir ve "soğuk verileri" [Azure Blob Depolama](../storage/common/storage-introduction.md)gibi diğer depolama sistemlerine yaşlandırabilirsiniz.

Verileri bölümler [ve kapsayıcılar arasında normalden arındırmak](how-to-model-partition-example.md#v2-introducing-denormalization-to-optimize-read-queries
)zorunda olduğunuzda, bu veri çoğaltma için kaynak olarak kapsayıcınızın değişiklik akışından okuyabilirsiniz. Değişiklik akışı ile gerçek zamanlı veri çoğaltma yalnızca nihai tutarlılığı garanti edebilir. Özet [Akışı İşlemcisi'ni](how-to-use-change-feed-estimator.md) Cosmos kapsayıcınızdaki işlemlerde ne kadar geride kaldığını izleyebilirsiniz.

## <a name="event-sourcing"></a>Olay kaynak

[Olay kaynak deseni,](https://docs.microsoft.com/azure/architecture/patterns/event-sourcing) bu verilerdeki eylemlerin tamamını kaydetmek için yalnızca bir ek deposu kullanmayı içerir. Azure Cosmos DB'nin değişiklik akışı, tüm veri alımlarının yazma olarak modellendiği (güncelleştirme veya silme yok) mimarileri kaynak oluşturma durumunda merkezi bir veri deposu olarak mükemmel bir seçimdir. Bu durumda, Azure Cosmos DB'ye her bir yazma bir "olay" dır ve değişiklik akışındaki geçmiş olayların tam bir kaydına sahip olacaksınız. Merkezi etkinlik deposu tarafından yayınlanan olayların tipik kullanımları, maddeleştirilmiş görünümleri korumak veya dış sistemlerle tümleştirme içindir. Değişiklik akışında bekletme için bir zaman sınırı olmadığından, Cosmos kapsayıcınızın değişiklik yayınının başından itibaren okuyarak geçmiş tüm olayları yeniden oynatabilirsiniz.

[Birden çok değişiklik akışı tüketicilerin aynı kapsayıcının değişim akışına abone](how-to-create-multiple-cosmos-db-triggers.md#optimizing-containers-for-multiple-triggers)olmasını sağlayabilirsiniz. [Kira kabının](change-feed-processor.md#components-of-the-change-feed-processor) sağlanan iş bakışının yanı sıra, değişiklik beslemesinden yararlanmak için herhangi bir maliyet yoktur. Değişiklik akışı, kullanılıp kullanılmadığına bakılmaksızın her kapta kullanılabilir.

Azure Cosmos DB, yatay ölçeklenebilirlik ve yüksek kullanılabilirlik teki güçlü yanları nedeniyle, kaynak modelinde yalnızca merkezi bir ek yalnızca kalıcı veri deposudur. Buna ek olarak, Değişiklik Özet Akışı İşlemci kitaplığı ["en az bir kez"](change-feed-processor.md#error-handling) garanti sunarak herhangi bir olayı işlemeyi kaçırmamanızı sağlar.

## <a name="current-limitations"></a>Geçerli sınırlamalar

Değişiklik akışı anlamanız gereken önemli sınırlamalar vardır. Cosmos kapsayıcısındaki öğeler her zaman değişiklik akışında kalacak olsa da, değişiklik akışı tam bir işlem günlüğü değildir. Değişiklik beslemesini kullanan bir uygulama tasarlarken göz önünde bulundurulması gereken önemli alanlar vardır.

### <a name="intermediate-updates"></a>Ara güncellemeler

Değişiklik akışına yalnızca belirli bir öğe için en son değişiklik dahildir. Değişiklikleri işlerken, kullanılabilir en son öğe sürümünü okuyabilirsiniz. Kısa bir süre içinde aynı öğede birden çok güncelleştirme varsa, ara güncelleştirmeleri işlemeyi kaçırmak mümkündür. Güncelleştirmeleri izlemek ve bir öğedeki geçmiş güncelleştirmeleri yeniden oynatabilmek istiyorsanız, bu güncelleştirmeleri bir dizi yazı olarak modellemenizi öneririz.

### <a name="deletes"></a>Silme

Değişiklik akışı silmeleri yakalamaz. Bir öğeyi kapsayıcınızdan silerseniz, değişiklik akışından da kaldırılır. Bu işleme en yaygın yöntem silinen öğeleri yumuşak bir işaretçi eklemektir. "Silinmiş" adlı bir özellik ekleyebilir ve silme sırasında "doğru" olarak ayarlayabilirsiniz. Bu belge güncelleştirmesi değişiklik akışında gösterilecek. Bu öğeye daha sonra otomatik olarak silinebilecek bir TTL ayarlayabilirsiniz.

### <a name="guaranteed-order"></a>Garantili sipariş

Bir bölüm anahtar değeri içinde değişiklik akışında garantili sipariş vardır, ancak bölüm anahtar değerleri arasında değil. Anlamlı bir sipariş garantisi veren bir bölüm anahtarı seçmelisiniz.

Örneğin, olay kaynak tasarım deseni kullanarak bir perakende uygulama düşünün. Bu uygulamada, farklı kullanıcı eylemleri Azure Cosmos DB'ye yazdığı gibi modellenen her bir "olay"dır. Bazı örnek olayların aşağıdaki sırayla oluştuğunu düşünün:

1. Müşteri alışveriş sepetine A öğesini ekler
2. Müşteri alışveriş sepetine B öğesini ekler
3. Müşteri, A maddesini alışveriş sepetinden kaldırır ekler
4. Müşteri çıkışları ve alışveriş sepeti içeriği sevk edilir

Her müşteri için geçerli alışveriş sepeti içeriğinin somutlaştırılmış görünümü korunur. Bu uygulama, bu olayların oluştukları sırayla işlendiğinden emin olmalıdır. Örneğin, sepet ödemesi A maddesi kaldırılmadan önce işlenecekse, müşterinin istenen B maddesi yerine A maddesi sevk edilmiş olması olasıdır. Bu dört olayın oluş sırasına göre işlendiğini garanti etmek için, aynı bölüm anahtar değeri içinde düşmeleri gerekir. Bölüm anahtarı olarak **kullanıcı adını** (her müşterinin benzersiz bir kullanıcı adı vardır) seçerseniz, bu olayların Azure Cosmos DB'ye yazıldıkları sırayla değişiklik akışında görüneceğini garanti edebilirsiniz.

## <a name="examples"></a>Örnekler

Microsoft dokümanlarında sağlanan örneklerin kapsamının ötesine uzanan bazı gerçek dünya değişiklik akış kodu örnekleri aşağıda verilmiştir:

- [Değişiklik akışına giriş](https://azurecosmosdb.github.io/labs/dotnet/labs/08-change_feed_with_azure_functions.html)
- [Değişiklik beslemesi etrafında ortalanmış IoT kullanım örneği](https://github.com/AzureCosmosDB/scenario-based-labs)
- [Değişim akışı etrafında ortalanmış perakende kullanım örneği](https://github.com/AzureCosmosDB/scenario-based-labs)

## <a name="next-steps"></a>Sonraki adımlar

* [Değişiklik akışına genel bakış](change-feed.md)
* [Değişiklik akışını okuma seçenekleri](read-change-feed.md)
* [Azure İşlevleriyle değişiklik akışını kullanma](change-feed-functions.md)