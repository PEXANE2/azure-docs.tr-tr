---
title: Azure Cosmos DB'de değişiklik akışı desteğiyle çalışma
description: Belgelerdeki değişiklikleri, tetikleyiciler gibi olay tabanlı işlemleri izlemek ve önbellekleri ve analitik sistemleri güncel tutmak için Azure Cosmos DB değiştirme akışı desteğini kullanın
author: TheovanKraay
ms.author: thvankra
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/25/2019
ms.reviewer: sngun
ms.custom: seodec18
ms.openlocfilehash: 898dfe7a619981b93af98effa942fdecbeb42dde
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79368137"
---
# <a name="change-feed-in-azure-cosmos-db---overview"></a>Azure Cosmos DB'deki değişiklik akışı - genel bakış

Azure Cosmos DB’de değişiklik akışı desteği, Azure Cosmos kapsayıcısında yapılan değişiklikleri dinleyerek çalışır. Ardından çıkış olarak, değiştirilen belgelerin değiştirilme zamanına göre sıralandığı bir belge listesi oluşturur. Değişiklikler kalıcı hale getirilir, zaman uyumsuz ve artırımlı olarak işlenebilir ve çıkış, paralel işleme için bir veya daha fazla tüketiciye dağıtılabilir. 

Azure Cosmos DB, IoT, oyun, perakende ve operasyonel günlük uygulaması için uygundur. Bu uygulamalarda yaygın bir tasarım deseni, ek eylemleri tetiklemek için verilerdeki değişiklikleri kullanmaktır. Ek eylemlere örnek olarak şunlar verilebilir:

* Bir öğe eklendiğinde veya güncelleştirildiğinde, bir BILDIRIMI veya API'ye çağrıyı tetikleme.
* IoT veya operasyonel veriler üzerinde gerçek zamanlı analiz işleme için gerçek zamanlı akış işleme.
* Önbellek veya arama motoru veya veri ambarı yla senkronize ederek veya verileri soğuk depolamaya arşivleyerek ek veri hareketi.

Azure Cosmos DB'deki değişiklik akışı, aşağıdaki resimde gösterildiği gibi, bu desenlerin her biri için verimli ve ölçeklenebilir çözümler oluşturmanıza olanak tanır:

![Azure Cosmos DB'yi kullanarak gerçek zamanlı analitik ve olay odaklı bilgi işlem senaryolarına güç sağlamak için akış değiştirme](./media/change-feed/changefeedoverview.png)

## <a name="supported-apis-and-client-sdks"></a>Desteklenen API'ler ve istemci SDK'ları

Bu özellik şu anda aşağıdaki Azure Cosmos DB API'leri ve istemci SDK'ları tarafından desteklenir.

| **İstemci sürücüleri** | **Azure CLI** | **SQL API’si** | **Cassandra için Azure Cosmos DB'nin API'si** | **MongoDB için Azure Cosmos DB API'si** | **Gremlin API**|**Tablo API'si** |
| --- | --- | --- | --- | --- | --- | --- |
| .NET | NA | Evet | Evet | Evet | Evet | Hayır |
|Java|NA|Evet|Evet|Evet|Evet|Hayır|
|Python|NA|Evet|Evet|Evet|Evet|Hayır|
|Düğüm/JS|NA|Evet|Evet|Evet|Evet|Hayır|

## <a name="change-feed-and-different-operations"></a>Özet akışı ve farklı işlemleri değiştirme

Bugün, tüm işlemleri değişiklik akışında görürsünüz. Yalnızca güncelleştirmeler ve ekler gibi belirli işlemler için değişiklik akışını denetleyebileceğiniz işlevsellik henüz kullanılamıyor. Değişiklik akışındaki öğeleri işlerken, güncelleştirmeler için öğeye bir "yumuşak işaretçi" ekleyebilir ve buna göre filtreleyebilirsiniz. Şu anda akış değişikliği günlük silme değil. Önceki örneğe benzer şekilde, silinen öğelere yumuşak bir işaretçi ekleyebilirsiniz, örneğin, "silinmiş" olarak adlandırılan öğeye bir öznitelik ekleyebilir ve "true" olarak ayarlayabilir ve öğeye bir TTL ayarlayabilirsiniz, böylece otomatik olarak silinebilir. Geçmiş öğelerin değişiklik akışını (öğeye karşılık gelen en son değişiklik, ara değişiklikleri içermez), örneğin beş yıl önce eklenen öğeleri okuyabilirsiniz. Öğe silinmezse, değişiklik akışını kabınızın kaynağına kadar okuyabilirsiniz.

### <a name="sort-order-of-items-in-change-feed"></a>Değişiklik akışındaki maddelerin sırasını sıralama

Değişiklik besleme öğeleri, değişiklik zamansırasına göre gelir. Bu sıralama sırası mantıksal bölüm anahtarı başına garanti edilir.

### <a name="consistency-level"></a>Tutarlılık düzeyi

Değişiklik akışını Nihai tutarlılık düzeyinde tüketirken, sonraki değişiklik akışı okuma işlemleri arasında yinelenen olaylar olabilir (bir okuma işleminin son olayı bir sonrakiilk olay olarak görünür).

### <a name="change-feed-in-multi-region-azure-cosmos-accounts"></a>Çok bölgeli Azure Cosmos hesaplarında özet akışı değiştirme

Çok bölgeli bir Azure Cosmos hesabında, bir yazma bölgesi başarısız olursa, değişiklik akışı el ile başarısız olma işlemi boyunca çalışır ve bitişik olur.

### <a name="change-feed-and-time-to-live-ttl"></a>Akış ve Yaşam Süresini Değiştirme (TTL)

Bir TTL (Time to Live) özelliği bir öğe üzerinde -1 olarak ayarlanırsa, değişiklik akışı sonsuza kadar devam eder. Veriler silinmezse, değişiklik akışında kalır.  

### <a name="change-feed-and-_etag-_lsn-or-_ts"></a>Besleme ve _etag, _lsn veya _ts değiştirme

_etag biçimi dahilidir ve her zaman değişebileceğinden, bu biçime bağımlılık yapmamalısınız. _ts bir değişiklik veya oluşturma zaman damgası. Kronolojik karşılaştırma için _ts kullanabilirsiniz. _lsn yalnızca değişiklik akışı için eklenen bir toplu iş kimliğidir; hareket kimliğini temsil eder. Birçok öğe aynı _lsn sahip olabilir. FeedResponse'daki ETag, öğede gördüğünüz _etag farklıdır. _etag dahili bir tanımlayıcıdır ve eşzamanlılık denetimi için kullanılır öğenin sürümü hakkında söyler, ETag ise beslemeyi sıralamak için kullanılır.

## <a name="change-feed-use-cases-and-scenarios"></a>Özet akışı kullanım durumlarını ve senaryoları değiştirme

Akış değişikliği, yüksek miktarda yazma ile büyük veri kümelerinin verimli bir şekilde işlenmesini sağlar. Akış değişikliği, neyin değiştiğini belirlemek için tüm veri kümesini sorgulamaya da bir alternatif sunar.

### <a name="use-cases"></a>Uygulama alanları

Örneğin, değişiklik akışı ile aşağıdaki görevleri verimli bir şekilde gerçekleştirebilirsiniz:

* Önbelleği güncelleştirin, arama dizinini güncelleştirin veya Veri ambarında Azure Cosmos DB'de depolanan verilerle güncelleştirin.

* Uygulama düzeyinde veri katmanlama ve arşivleme uygulayın, örneğin Azure Cosmos DB'de "sıcak verileri" depolayın ve "soğuk verileri" diğer depolama sistemlerine [(örneğin, Azure Blob Depolama)](../storage/common/storage-introduction.md)yaşlayın.

* Farklı bir mantıksal bölüm anahtarına sahip başka bir Azure Cosmos hesabına veya başka bir Azure Cosmos kapsayıcısına sıfır aşağı zamanlı geçiş gerçekleştirin.

* Azure Cosmos DB'nin hem gerçek zamanlı, toplu hem de sorgu sunma katmanlarını desteklediği Azure Cosmos DB kullanarak [lambda mimarisini](https://blogs.technet.microsoft.com/msuspartner/2016/01/27/azure-partner-community-big-data-advanced-analytics-and-lambda-architecture/) uygulayın ve böylece düşük TCO ile lambda mimarisini etkinleştirin.

* Aygıtlardan, sensörlerden, altyapıdan ve uygulamalardan olay verilerini alın ve depolayın ve bu olayları örneğin [Kıvılcım'ı](../hdinsight/spark/apache-spark-overview.md)kullanarak gerçek zamanlı olarak işleyin.  Aşağıdaki resim, değiştirme akışı aracılığıyla Azure Cosmos DB'yi kullanarak lambda mimarisini nasıl uygulayabileceğinizi gösterir:

![Yutma ve sorgulama için Azure Cosmos DB tabanlı lambda boru hattı](./media/change-feed/lambda.png)

### <a name="scenarios"></a>Senaryolar

Değişiklik akışıyla kolayca uygulayabileceğiniz senaryolardan bazıları şunlardır:

* [Sunucusuz](https://azure.microsoft.com/solutions/serverless/) web veya mobil uygulamalarınızda, müşteriprofilinizdeki, tercihlerindeki veya konumundaki tüm değişiklikler gibi olayları izleyebilir ve [Azure İşlevlerini](change-feed-functions.md)kullanarak cihazlarına anında iletme bildirimleri göndermek gibi belirli eylemleri tetikleyebilirsiniz.

* Bir oyun oluşturmak için Azure Cosmos DB kullanıyorsanız, örneğin, tamamlanan oyunlardan alınan puanları temel alan gerçek zamanlı lider panolarını uygulamak için değişiklik akışını kullanabilirsiniz.


## <a name="working-with-change-feed"></a>Değişiklik akışıyla çalışma

Aşağıdaki seçenekleri kullanarak değişiklik akışıyla çalışabilirsiniz:

* [Azure İşlevleriyle değişiklik akışını kullanma](change-feed-functions.md)
* [Değişim beslemesi işlemcisi ile beslemeyi değiştirme](change-feed-processor.md) 

Değişiklik akışı kapsayıcıdaki her mantıksal bölüm anahtarı için kullanılabilir ve aşağıdaki resimde gösterildiği gibi paralel işleme için bir veya daha fazla tüketiciye dağıtılabilir.

![Azure Cosmos DB değişiklik akışı dağıtılmış işleme](./media/change-feed/changefeedvisual.png)

## <a name="features-of-change-feed"></a>Değişiklik akışı özellikleri

* Tüm Azure Cosmos hesapları için varsayılan olarak akış değişikliği etkinleştirilir.

* Azure Cosmos veritabanınızla ilişkili bölgelerden herhangi birinde, diğer Azure Cosmos DB işlemleri gibi değişiklik akışından okumak için [sağlanan iş lerinizi](request-units.md) kullanabilirsiniz.

* Değişiklik akışı, kapsayıcıiçindeki maddelere yapılan ekler ve güncelleştirme işlemlerini içerir. Öğelerinizde (örneğin, belgeler) silmek yerine bir "yumuşak silme" bayrağı ayarlayarak silmeleri yakalayabilirsiniz. Alternatif olarak, [TTL özelliğine](time-to-live.md)sahip öğeleriniz için sonlu bir son kullanma süresi ayarlayabilirsiniz. Örneğin, 24 saat ve siler yakalamak için bu özelliğin değerini kullanın. Bu çözümle, değişiklikleri TTL son kullanma süresinden daha kısa bir zaman aralığında işlemeniz gerekir. 

* Bir öğedeki her değişiklik, değişiklik akışında tam olarak bir kez görüntülenir ve istemcilerin denetim noktası mantığını yönetmesi gerekir. Denetim noktalarını yönetme karmaşıklığından kaçınmak istiyorsanız, değişiklik akışı işlemcisi otomatik denetim noktası ve "en az bir kez" semantik sağlar. [Değişim akışı işlemcisi ile beslemeyi değiştir'i kullanarak](change-feed-processor.md)bkz.

* Yalnızca belirli bir öğe için en son değişiklik değişiklik günlüğüne dahildir. Ara değişiklikler kullanılamayabilir.

* Değişiklik akışı, her mantıksal bölüm anahtar değeri içindeki değişiklik sırasına göre sıralanır. Bölüm anahtar değerleri arasında garantili bir sipariş yoktur.

* Değişiklikler herhangi bir noktadan eşitlenebilir, yani değişikliklerin kullanılabilir olduğu sabit bir veri saklama süresi yoktur.

* Değişiklikler, bir Azure Cosmos kapsayıcısının tüm mantıksal bölüm tuşları için paralel olarak kullanılabilir. Bu özellik, büyük kapsayıcılardan gelen değişikliklerin birden çok tüketici tarafından paralel olarak işlenmesine olanak tanır.

* Uygulamalar aynı kapta aynı anda birden çok değişiklik akışı isteyebilir. ChangeFeedOptions.StartTime bir başlangıç noktası sağlamak için kullanılabilir. Örneğin, belirli bir saat saatine karşılık gelen devamı belirteci bulmak için. ContinuationToken, belirtildiği takdirde Başlangıç Zamanı ve Başlangıç Başlangıç değerlerini kazanır. ChangeFeedOptions.StartTime hassasiyeti ~5 saniyedir. 

## <a name="change-feed-in-apis-for-cassandra-and-mongodb"></a>Cassandra ve MongoDB için API'lerde besleme değiştirme

Değişiklik akışı işlevi, MongoDB API ve Sorgu'da Cassandra API'de yüklem içeren değişim akışı olarak su yüzüne çıkar. MongoDB API'nin uygulama ayrıntıları hakkında daha fazla bilgi edinmek [için MongoDB için Azure Cosmos DB API'deki Değiştir akışlarına](mongodb-change-streams.md)bakın.

Yerli Apache Cassandra değişim veri yakalama (CDC), arşivleme için belirli tabloları bayrak için bir mekanizma yanı sıra CDC günlüğü için yapılandırılabilir boyutu-on-disk ulaştıktan sonra bu tablolara yazma ları reddederek sağlar. Cassandra için Azure Cosmos DB API'deki değişiklik akışı özelliği, değişiklikleri CQL üzerinden yüklemle sorgulama olanağını geliştirir. Uygulama ayrıntıları hakkında daha fazla bilgi edinmek [için Cassandra için Azure Cosmos DB API'sındaki akış değiştir'e](cassandra-change-feed.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar

Şimdi aşağıdaki makalelerde değişiklik akışı hakkında daha fazla bilgi edinmek için devam edebilirsiniz:

* [Değişiklik akışını okuma seçenekleri](read-change-feed.md)
* [Azure İşlevleriyle değişiklik akışını kullanma](change-feed-functions.md)
* [Değişiklik beslemeişlemcisi kullanma](change-feed-processor.md)
